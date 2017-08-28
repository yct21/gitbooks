# Language
## 类型

JavaScript 中的类型包括：

- `number`
- `string`
- `boolean`
- `object`
- `function`
- `undefined`

JavaScript 的 `number`类型统一按浮点数处理，64 位存储，整数安最大 54 位来计算最
大最小数的，否则会丧失精度；某些操作（如数组索引和位操作）按 32 位处理。

另外有些特殊符号也视为 `number`，包括：

- `Infinity`
- `-Infinity`
- `NaN`

JavaScript 中有关 `Infinity` 和 `-Infinity` 的运算并不完全符合数学(not math-solid)。

关于 `undefined` 类型：

> Many operations in the language that don’t produce a meaningful value (you’ll see some later) yield undefined simply because they have to yield some value.

> The difference in meaning between undefined and null is an accident of JavaScript’s design, and it doesn’t matter most of the time. In the cases where you actually have to concern yourself with these values, I recommend treating them as interchangeable (more on that in a moment).

JavaScript 中的隐式类型转换是一个著名的大坑：

```js
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
```

## 函数

当函数的执行过程中未执行 `return` 语句时，该函数返回 `undefined`。

当一个函数以 “function declaration” 的方式声明时，这个函数会被移动到这个 scope
的最前：

```js
console.log("The future says:", future());

function future() {
  return "We STILL have no flying cars.";
}
```

如果这个函数声明在条件语句或者循环语句内部，各个 JavaScript 平台对此有不同的执行
方法。

```js
function example() {
  function a() {} // Okay
  if (something) {
    function b() {} // Danger!
  }
}
```

## `object`

JavaScript 中，几乎所有的 object 都有 properties，例外是 `null` 和 `undefined`。

使用 `.` 来获取一个 object 的属性时，属性名必须是一个合法的变量名，而使用 `[]`
则无此要求。

作为一个面向原型的语言，JavaScript 中绝大部分的 object 都有 `__prototype__` 属性，
当使用一个 object 中的不存在的某个属性时，会从原型链上查询这个属性。

面向原型：

```js
function Rabbit(type) {
  this.type = type;
}

var killerRabbit = new Rabbit("killer");
var blackRabbit = new Rabbit("black");
console.log(blackRabbit.type);

Rabbit.prototype.speak = function(line) {
  console.log("The " + this.type + " rabbit says '" +
              line + "'");
};
blackRabbit.speak("Doom...");
// → The black rabbit says 'Doom...'
// → black
```

直接处于 object 内的属性会覆盖原型链中的属性。

JavaScript 中某些属性是 "nonenumerable"，例如 `Object.protoype` 中的属性。此外，
可以用 `Object.defineProperty` 来定义 nonenumerable 的属性。

```js
Object.defineProperty(Object.prototype, "hiddenNonsense",
                      {enumerable: false, value: "hi"});
for (var name in map)
  console.log(name);
// → pizza
// → touched tree
console.log(map.hiddenNonsense);
// → hi
```

使用 `Object.create(null)` 可以创造一个无原型的 object。

面向原型的继承：

```js
function RTextCell(text) {
  TextCell.call(this, text);
}
RTextCell.prototype = Object.create(TextCell.prototype);
RTextCell.prototype.draw = function(width, height) {
  var result = [];
  for (var i = 0; i < height; i++) {
    var line = this.text[i] || "";
    result.push(repeat(" ", width - line.length) + line);
  }
  return result;
};
```

在函数被调用时，会绑定相应的 `this`。

## Reference

- [Eloquent JavaScript](http://eloquentjavascript.net/)
