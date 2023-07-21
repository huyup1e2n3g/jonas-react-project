## Create React App

```shell
npx create-react-app@5 pizza-menu
```

## Random Avatar

[pravatar.cc](https://pravatar.cc/)

```
https://i.pravatar.cc/{size}
```

## Arrow Function

使用普通的函数定义来作为 onClick 事件处理：

```js
<button
  className="button"
  onClick={function () {
    onSelection(friend);
  }}
>
  Select
</button>
```

使用箭头函数：

```js
<button className="button" onClick={() => onSelection(friend)}>
  Select
</button>
```

在这段代码中，箭头函数的作用是在按钮被点击时执行一个特定的操作。具体来说，当按钮被点击时，箭头函数会调用 `onSelection` 函数并将 `friend` 作为参数传递给它。箭头函数的使用允许我们在事件处理程序中定义一个匿名函数，并且可以在其中访问到 `friend` 变量。这样可以实现在点击按钮时执行特定操作的功能。

箭头函数具有以下几个特点和作用：

1. 简洁的语法：箭头函数提供了一种简洁的语法来定义函数，可以减少代码的冗余和可读性的提升。
2. 隐式的返回值：如果箭头函数只有一行语句，并且没有使用大括号包裹函数体，那么该语句的结果将自动成为函数的返回值。
3. 自动绑定 this：箭头函数没有自己的 this 值，它会继承外层函数的 this 值。这样可以避免使用 bind()方法或在回调函数中使用额外的变量来绑定 this。
4. 适合作为回调函数：由于箭头函数的简洁性和自动绑定 this 的特点，它非常适合作为回调函数使用，特别是在 React 等框架中。

不能写成以下的方式：

```js
<button className="button" onClick="{onSelection(friend)}">
  Select
</button>
```

在这种情况下，将直接调用 `onSelection(friend)` 函数，并将其返回值作为 `onClick` 事件处理程序。这意味着在渲染按钮时，`onSelection(friend)` 函数将被立即调用，而不是在按钮被点击时。

## 验证输入框输入的内容

eat-n-split 项目中有一个问题，当输入框中输入非数字时，会导致输入框的值变为 `NaN` ，并且无法再重新输入。

要解决这个问题，可以在 `onChange` 事件处理程序中添加一些逻辑来处理非数字输入。

方法一：使用正则表达式来验证输入的值是否为数字，如果不是数字，则不更新 `state` 中的 `bill` 值。

```js
// 修改前
<input
  type="text"
  value={bill}
  onChange={(e) => setBill(Number(e.target.value))}
></input>

// 修改后
<input
  type="text"
  value={bill}
  onChange={(e) => {
    const inputValue = e.target.value;
    if (inputValue === "") {
      setBill(0);
    } else if (/^\d+$/.test(inputValue)) {
      setBill(Number(inputValue));
    }
  }}
></input>
```

使用正则表达式 `/^\d+$/` 来检查输入的值是否为数字。如果输入的值是由一个或多个数字字符组成的字符串，则将其转换为数字并更新 `state` 中的 `bill` 值。如果输入的值不是数字，则不更新 `bill` 值。

这样，当输入非数字时，输入框的值将保持不变，并且用户可以继续输入正确的数字。

方法二：使用 `isNaN()` 函数来检查输入的值是否为 `NaN`。

```js
<input
  type="text"
  value={bill}
  onChange={(e) => {
    const inputValue = e.target.value;
    if (!isNaN(Number(inputValue))) {
      setBill(Number(inputValue));
    } else {
      setBill(bill);
    }
  }}
></input>
```
