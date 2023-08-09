# pizza-menu

## Create React App

```shell
npx create-react-app@5 pizza-menu
```

# Eat-n-split

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

## KEY PROP

eat-n-split 项目中，选择不同的朋友时，会保留已输入的内容，为了解决这个问题，可以用 `key` 参数，使其变为独一无二的元素。

```js
{
  selectedFriend && (
    <FormSplitBill
      selectedFriend={selectedFriend}
      onSplitBill={handleSplitBill}
      key={selectedFriend.id}
    />
  );
}
```

# usePopcorn

## OMDb API

[http://www.omdbapi.com/](http://www.omdbapi.com/)

## 滚动条问题

滚动条出现白色边框问题，重试多次后自动修复了。

```css
overflow: auto;
overflow: scroll;
```

## Cleaning Up Data Fetching

当输入 query 时，浏览器会不断发出请求，如果其中一个请求时间过长会导致得到的结果不是最终所输入的结果，为了解决这个问题，可以用 AbortController，它是浏览器的 API。

这样当有新的 query 请求时，前面的请求会自动中断。

```js
useEffect(
  function () {
    const controller = new AbortController();

    async function fetchMovies() {
      try {
        // ...
        const res = await fetch(
          `http://www.omdbapi.com/?apikey=${KEY}&s=${query}`,
          { signal: controller.signal }
        );
        // ...
      } catch (err) {
        if (err.name !== "AbortError") {
          console.log(err.message);
          setError(err.message);
        }
      } finally {
        // ...
      }
    }

    return function () {
      controller.abort();
    };
  },
  [query]
);
```

这段代码是使用 React 的 useEffect 钩子函数来执行副作用操作。在这个例子中，副作用操作是在组件挂载或更新时异步获取电影数据。

首先，我们创建了一个新的 AbortController 实例，用于在需要时中止异步请求。然后，我们定义了一个名为 fetchMovies 的异步函数，用于实际获取电影数据。在 try 块中，我们可以执行实际的数据获取操作。在 catch 块中，我们处理可能的错误。如果错误的名称不是 "AbortError"，我们将错误消息记录到控制台并设置组件的错误状态。在 finally 块中，我们可以执行一些清理操作，例如隐藏加载指示器。

最后，我们返回一个函数，该函数在组件卸载时会被调用。在这个函数中，我们调用了 AbortController 实例的 abort 方法，以中止之前发起的异步请求。这样做是为了确保在组件卸载时取消未完成的请求，以避免可能的内存泄漏或错误。

在 useEffect 的第二个参数中，我们传递了一个依赖数组 [query]。这意味着只有当 query 发生变化时，才会重新运行副作用操作。这可以帮助我们避免不必要的重复请求，只在 query 发生变化时才获取新的电影数据。

总之，这段代码使用 useEffect 和异步函数来获取电影数据，并在组件卸载时中止未完成的请求，以确保代码的正确性和性能。

## Key Press Events

实现在电影详情页面按下 Escape 键关闭该页面的功能。

在 MovieDetails 组建中添加新的 useEffect ：

```js
useEffect(
  function () {
    function callback(e) {
      if (e.code === "Escape") {
        onCloseMovie();
      }
    }

    document.addEventListener("keydown", callback);

    return function () {
      document.removeEventListener("keydown", callback);
    };
  },
  [onCloseMovie]
);
```

在这个代码中，useEffect 钩子函数来执行副作用操作，在组件挂载或更新时添加一个事件监听器，以便在按下 Escape 键时触发 onCloseMovie 函数。

首先，我们定义了一个名为 callback 的函数，它接受一个事件对象作为参数。在 callback 函数中，我们检查事件对象的 code 属性是否等于 "Escape"，如果是，则调用 onCloseMovie 函数。

然后，我们使用 document.addEventListener 方法将 callback 函数添加为 keydown 事件的监听器。这意味着当用户按下键盘上的任意键时，都会触发 callback 函数。

最后，我们返回一个函数，该函数在组件卸载时会被调用。在这个函数中，我们使用 document.removeEventListener 方法将之前添加的事件监听器移除，以避免在组件卸载后仍然触发回调函数。

在 useEffect 的第二个参数中，我们传递了一个依赖数组 [onCloseMovie]。这意味着只有当 onCloseMovie 发生变化时，才会重新运行副作用操作。这可以帮助我们避免不必要的重复添加和移除事件监听器。

总之，这段代码使用 useEffect 和事件监听器来在按下 Escape 键时触发 onCloseMovie 函数，并在组件卸载时移除事件监听器，以确保代码的正确性和性能。

## Initializing State With a Callback (Lazy Initial State/Local Storage)

```js
export default function App() {
  const [watched, setWatched] = useState(function () {
    const storedValue = localStorage.getItem("watched");
    return JSON.parse(storedValue);
  });

  useEffect(
    function () {
      localStorage.setItem("watched", JSON.stringify(watched));
    },
    [watched]
  );
}
```

# react-quiz

## Loading Questions from a Fake API

```shell
npm i json-server
```

```json
// package.json
"scripts": {
    // ...
    "server": "json-server --watch data/questions.json --port 9000"
  },

```

```shell
npm run server
```

localhost:9000/questions

## tick 状态下的 highscore 问题

```js
case "tick":
  return {
    ...state,
    secondsRemaining: state.secondsRemaining - 1,
    status: state.secondsRemaining === 0 ? "finished" : state.status,
  };
```

在 "tick" 状态下，剩余时间为 0 后，状态将更新为 "finished"，但是 highscore 并没有更新，导致界面转到 FinishScreen 后，不会正确显示 highscore。

目前的解决方法是在 "tick" 状态下新增 highscore :

```js
case "tick":
  return {
    ...state,
    secondsRemaining: state.secondsRemaining - 1,
    status: state.secondsRemaining === 0 ? "finished" : state.status,
    highscore:
      state.points > state.highscore ? state.points : state.highscore,
  };
```

但应该有更好的解决方法，如何让 `state.secondsRemaining === 0` 时，dispatch 到 "finish" 下。
