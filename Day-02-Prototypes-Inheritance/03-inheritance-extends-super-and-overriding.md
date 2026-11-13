# Class Inheritance: extends, super, and Method Overriding

## Inheritance

Inheritance allows a child class to reuse and extend behavior from a parent class.

## extends

Use `extends` to establish a subclass relationship.

## super()

Use `super(...args)` inside a derived class constructor to call the parent constructor.

If a derived class declares its own constructor, it must call `super()` before accessing `this`.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `My name is ${this.name}`;
  }
}

class Developer extends Person {
  constructor(name, language) {
    super(name);
    this.language = language;
  }

  code() {
    return `${this.name} writes ${this.language}`;
  }
}

const dev = new Developer("Raj", "JavaScript");

console.log(dev.introduce()); // "My name is Raj"
console.log(dev.code());      // "Raj writes JavaScript"
```

## super.method()

Use `super.methodName()` to call a parent class method.

```js
class Person {
  greet() {
    return "Hello from Person";
  }
}

class Developer extends Person {
  greet() {
    return super.greet() + " and Developer";
  }
}

console.log(new Developer().greet());
// "Hello from Person and Developer"
```

- `super()` calls the parent constructor.
- `super.method()` calls a parent method.

## Method overriding

When a child class defines a method with the same name as a parent method, the child's implementation is used when called on a child instance.

```js
class Person {
  greet() {
    return "Hello from Person";
  }
}

class Developer extends Person {
  greet() {
    return "Hello from Developer";
  }
}

console.log(new Developer().greet());
// "Hello from Developer"
```

This is called method overriding.

## Coding exercise: Employee and Manager

```js
class Employee {
  constructor(name, salary) {
    this.name = name;
    this.salary = salary;
  }

  getDetails() {
    return `${this.name} earns ${this.salary}`;
  }
}

class Manager extends Employee {
  constructor(name, salary, teamSize) {
    super(name, salary);
    this.teamSize = teamSize;
  }

  getTeamDetails() {
    return `${this.name} manages a team of ${this.teamSize}`;
  }
}

const manager = new Manager("Raj", 50000, 5);

console.log(manager.getDetails());     // Raj earns 50000
console.log(manager.getTeamDetails()); // Raj manages a team of 5
```

## Interview-ready explanation

"JavaScript uses prototype-based inheritance. Objects inherit properties and methods through the prototype chain. ES6 introduced classes, which provide syntax over the prototype system. Using `extends`, a child class inherits from a parent class. `super()` calls the parent constructor, while `super.method()` calls a parent method. If a property is not found on an object, JavaScript searches up the prototype chain until it finds it or reaches `null`."

## Common mistakes

- Forgetting to call `super()` before using `this` in a derived constructor.
- Confusing `super()` with `super.method()`.
- Forgetting that a child method can override a parent method.
- Misspelling the special `constructor` method name.
