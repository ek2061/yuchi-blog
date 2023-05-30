# 箭頭函數和普通函數的區別

## TL;DR

:::info
this 指向不同  
call / apply / bind 無法改變 this 指向  
無法做為建構函數  
無法使用 arguments 物件  
箭頭函數不支援 new.target
:::

## this 指向不同

```javascript
const person = {
  fullName: "yuchi",
  // 普通函數
  sayName1() {
    console.log(`my name is ${this.fullName}`);
  },
  // 箭頭函數
  sayName2: () => {
    console.log(`my name is ${this.fullName}`);
  },
};

person.sayName1(); // my name is yuchi
person.sayName2(); // my name is undefined
```

箭頭函數的 this 指向當前函數父級作用域的 this(上述例子為全局 window)，因此為`undefined`，

如果在父作用域(window)加上宣告即可取得

```javascript
window.fullName = "joe";

const person = {
  ...
}

person.sayName2(); // my name is joe
```

## call / apply / bind 無法改變 this 指向

```javascript
// 普通函數
function sayName1() {
  console.log(`my name is ${this.fullName}`);
}

// 箭頭函數
const sayName2 = () => {
  console.log(`my name is ${this.fullName}`);
};

const person = {
  fullName: "yuchi",
};

sayName1.call(person); // my name is yuchi
sayName2.call(person); // my name is undefined
```

call 無法改變箭頭函數的 this 指向所以會拿到`undefined`

## 無法做為建構函數

```javascript
// 普通函數
function Person1() {
  this.fullName = "yuchi";
}
const person1 = new Person1(); // Person1 {fullName: 'yuchi'}

// 箭頭函數
const Person2 = () => {
  this.fullName = "yuchi";
};

const person2 = new Person2(); // VM2046:5 Uncaught TypeError: Person2 is not a constructor at <anonymous>:5:17
```

## 無法使用 arguments 物件

```javascript
// 普通函數
function sayName1() {
  const args = arguments;
  console.log(args);
}
sayName1("a", "b"); // Arguments(2) ['a', 'b', callee: ƒ, Symbol(Symbol.iterator): ƒ]

// 箭頭函數
const sayName2 = () => {
  const args = arguments;
  console.log(args);
};
sayName2("a", "b"); // Uncaught ReferenceError: arguments is not defined at sayName2 (<anonymous>:2:16)
```

如果箭頭函數要用 `arguments`，那要改成

```javascript
const sayName2 = (...args) => {
  console.log(args);
};
sayName2("a", "b"); // ['a', 'b']
```

## 箭頭函數不支援 new.target

```javascript
// 普通函數
function Person1() {
  this.name = "yuchi";
  const target = new.target;
  console.log(target);
  /* console.log 結果
  ƒ Person1() {
  this.name = "yuchi";
  const target = new.target;
  console.log(target);
  }
  */
}
const person1 = new Person1();
console.log(person1); // Person1 {name: 'yuchi'}
```

普通函數中的 new.target 會指向建構函數

```javascript
// 箭頭函數
const Person2 = () => {
  this.name = "yuchi";
  const target = new.target;
  console.log(target);  // Uncaught SyntaxError: new.target expression is not allowed here
}
const person2 = new Person2();
console.log(person2);
```

箭頭函數中的 new.target 不允許使用
