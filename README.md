# 📘 JavaScript Inheritance — Complete Notes (Prototypes & Classes)

JavaScript does **not** have classical inheritance like Java or C++.
Instead, JavaScript uses **Prototype-based Inheritance**.

ES6 `class` syntax is **syntactic sugar** over prototypes.

This document covers:
- Prototype-based inheritance (all variations)
- Class-based inheritance (ES6+)
- Edge cases & exceptions
- Internal mechanics
- Common mistakes & best practices

---

## 1️⃣ What Is Inheritance in JavaScript?

**Inheritance** means:
> An object can access properties and methods of another object.

In JS:
- Every object has an internal `[[Prototype]]`
- This creates a **prototype chain**
- Property lookup walks up the chain

---

## 2️⃣ Prototype-Based Inheritance (Core JS)

### 🔹 2.1 Using `__proto__` (NOT recommended)

```js
const parent = {
  greet() {
    console.log("Hello from parent");
  }
};

const child = {
  __proto__: parent
};

child.greet(); // Hello from parent
```

⚠️ Deprecated, slow, and unsafe for production.

---

### 🔹 2.2 Using `Object.create()` (Recommended)

```js
const parent = {
  greet() {
    console.log("Hello from parent");
  }
};

const child = Object.create(parent);
child.name = "JS";

child.greet();
```

---

### 🔹 2.3 Constructor Function Inheritance

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayName = function () {
  console.log(this.name);
};

function Student(name, roll) {
  Person.call(this, name);
  this.roll = roll;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

const s1 = new Student("Shashwat", 101);
s1.sayName();
```

---

### 🔹 2.4 Forgetting `.constructor` Fix

```js
Student.prototype = Object.create(Person.prototype);
console.log(Student.prototype.constructor === Person); // true ❌
```

Fix:
```js
Student.prototype.constructor = Student;
```

---

### 🔹 2.5 Shared Prototype Example

```js
const animal = {
  eat() {
    console.log("Eating");
  }
};

const dog = Object.create(animal);
const cat = Object.create(animal);

dog.eat();
cat.eat();
```

---

## 3️⃣ Prototype Chain

```text
obj → obj.__proto__ → Constructor.prototype → Object.prototype → null
```

---

## 4️⃣ Edge Cases

### 🔸 Shadowing

```js
const parent = { value: 10 };
const child = Object.create(parent);

child.value = 20;
```

### 🔸 Deleting Reveals Prototype Property

```js
delete child.value;
console.log(child.value); // 10
```

### 🔸 Shared Reference Pitfall

```js
const parent = { skills: [] };
const c1 = Object.create(parent);
const c2 = Object.create(parent);

c1.skills.push("JS");
console.log(c2.skills); // ["JS"]
```

---

## 5️⃣ ES6 Class Inheritance

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello ${this.name}`);
  }
}

class Student extends Person {
  constructor(name, roll) {
    super(name);
    this.roll = roll;
  }
}
```

---

## 6️⃣ super() Rule

```js
class A {
  constructor() {
    this.x = 10;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 20;
  }
}
```

---

## 7️⃣ Method Overriding

```js
class Parent {
  greet() {
    console.log("Parent");
  }
}

class Child extends Parent {
  greet() {
    super.greet();
    console.log("Child");
  }
}
```

---

## 8️⃣ Static Inheritance

```js
class A {
  static info() {
    console.log("Static A");
  }
}

class B extends A {}
B.info();
```

---

## 9️⃣ Extending Built-ins

```js
class MyArray extends Array {
  first() {
    return this[0];
  }
}
```

---

## 🔟 Mixins

```js
const canWalk = {
  walk() {
    console.log("Walking");
  }
};

const canTalk = {
  talk() {
    console.log("Talking");
  }
};

class Person {}
Object.assign(Person.prototype, canWalk, canTalk);
```

---

## 1️⃣1️⃣ Composition over Inheritance

```js
function Engine() {
  this.start = () => console.log("Engine started");
}

function Car() {
  this.engine = new Engine();
}
```

---

## 1️⃣2️⃣ Checking Inheritance

```js
obj instanceof Constructor;
Object.getPrototypeOf(obj) === Constructor.prototype;
```

---

## 1️⃣3️⃣ Final Truth

> JavaScript has **only prototype-based inheritance**.
> Classes are just syntactic sugar.

---

## ✅ Summary

✔ Prototypes are the foundation  
✔ Classes improve readability  
✔ Avoid shared mutable state  
✔ Prefer composition when possible  
