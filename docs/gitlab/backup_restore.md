## gitlab 備份還原

## TL;DR

:::info
這篇比較偏記錄過程而非教學，因為看 gitlab 官方文件其實滿詳細的。  
gitlab 有提供備份與還原的功能，  
但重要的檔案例如金鑰或憑證還是要自己備份與還原(其實就是自己複製貼上到目錄)。
詳細可以看官方文件的[備份](https://docs.gitlab.com/ee/raketasks/backup_gitlab.html)與[還原](https://github.com/webpack-contrib/webpack-bundle-analyzer)  
然後還原備份不能降版也要注意一下
:::

## 備份

如果連 gitlab 都沒有會建議使用`sameersbn/docker-gitlab`這個 image 搭建，因為可以省去很多設定的時間。  
`sameersbn/docker-gitlab`預設每天凌晨 4 點自動備份，且最多保留 7 天。

若是使用 gitlab 官方的 image 則要手動備份或設定
備份需要先看 gitlab 版本，12.2 開始的版本

```bash
sudo gitlab-backup create
```

而更早則是

```bash
gitlab-rake gitlab:backup:create
```

在 14.9 、14.10 和 15 版有個很酷的增量備份功能
版本 >= 15.0：

```bash
sudo gitlab-backup create INCREMENTAL=yes PREVIOUS_BACKUP=<timestamp_of_backup>
```

14.9 和 14.10：

```bash
sudo gitlab-backup create INCREMENTAL=yes BACKUP=<timestamp_of_backup>
```

備份檔案會存在容器內的 **/home/git/data/backups**，檔名前面有日期、以 gitlab_backup 結尾，且是 tar 壓縮檔。
備份還有其他詳細設定可以看[官方文件](https://docs.gitlab.com/ee/raketasks/backup_gitlab.html)

## 還原

還原有一點很重要的是不能拿新的 gitlab 版本備份去還原舊版的 gitlab (不能降版)，聽起來很像廢話，

但我還真的就遇過版本升上去有問題要降回來，但備份檔又剛好是新版的窘境。

在還原備份時必須確保容器中 **/home/git/data/backups** 路徑有要還原的備份檔，

如果沒有的話要先拿到備份檔，然後放到容器內儲存備份的路徑 **/home/git/data/backups** (或是透過 volume 映射)

```bash
sudo docker cp <備份檔> <容器名稱>:/home/git/data/backups/

# 這是我的例子
# sudo docker cp ./1685206820_2023_05_28_15.10.1_gitlab_backup.tar cim_gitlab:/home/git/data/backups/
```

以我自己為例，我使用 docker-compose 和 `sameersbn/docker-gitlab`架設 gitlab，
那我還原就是

```bash
sudo docker-compose run --rm gitlab app:rake gitlab:backup:restore BACKUP=1685206820_2023_05_28_15.10.1
# --rm gitlab 的 gitlab 是 docker-compose service 的名字
```

那要注意 3 點:

1. 要確定容器中 **/home/git/data/backups/** 有你的備份檔，不確定的話可以進去容器看一下
2. BACKUP 後面的是備份檔檔名，檔名不包含結尾的 `gitlab_backup` 和附檔名`.tar`
3. gitlab.rb 和 gitlab-secrets.json 還是要自己放進容器，因為不會還原備份
