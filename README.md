# react-study
## 重新学习react并记录笔记  2024.4.7开始

### 基本语法
- React 组件必须以大写字母开头，而 HTML 标签则必须是小写字母
- 必须闭合标签，如 <br />  
- 不能返回多个 JSX 标签。你必须将它们包裹到一个共享的父级中，比如 <div>...</div> 或使用空的 <>...</> 包裹
- JSX 会让你把标签放到 JavaScript 中 。 大括号会让你 “回到” JavaScript 中
```
return (
  <h1>
    {user.name}
  </h1>
);
```
- 将 JSX 属性 “转义到 JavaScript”，必须使用大括号 而非 引号
- 若要在jsx大括号里搞样式，可以使用以下办法： 其中style={ } JSX 大括号内的一个普通 {} 对象。当样式依赖于 JavaScript 变量时，可以使用 style 属性
```
export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```
- React 没有特殊的语法来编写条件语句，因此使用的就是普通的 JavaScript 代码
- （条件渲染）若有条件判断显示某页面，jsx内部是用三目运算符 ？：  若不需要else则可以用 &&。     若在jsx内部，则为 if else语句
- javascript的map() 函数： 对数组中的每个元素执行指定的函数，并返回一个新的数组，该数组包含每个函数执行的结果。map() 函数不会改变原始数组，而是返回一个新的数组，其中包含根据指定函数转换后的值。  例子：
```
const numbers = [1, 2, 3, 4, 5];

const doubledNumbers = numbers.map(function(number) {
    return number * 2;
});

console.log(doubledNumbers); // 输出: [2, 4, 6, 8, 10]
```
- 根据map函数，则可以实现渲染列表，如下：
```
// App.js文件
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

- 处理响应事件(注意，onClick={handleClick} 的结尾没有小括号！不要 调用 事件处理函数：只需 把函数传递给事件 即可。当用户点击按钮时 React 会调用传递的事件处理函数)
```
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

- 更新界面 : 引入 useState：  import { useState } from 'react';
```
// 在组件中声明一个 state 变量：
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
}
  // 上述将从 useState 中获得两样东西：当前的 state（count），以及用于更新它的函数（setCount）。你可以给它们起任何名字，但按照惯例会像 [something, setSomething] 这样为它们命名
```
```
// 第一次显示按钮时，count 的值为 0，因为你把 0 传给了 useState()。当你想改变 state 时，调用 setCount() 并将新的值传递给它。点击该按钮计数器将递增：

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
// 第一次 count 变成 1。接着点击会变成 2。继续点击会逐步递增。
// 注意：多次渲染同一个组件，每个组件都会拥有自己的 state
```

- Hook：以 use 开头的函数被称为 Hook。useState 是 React 提供的一个内置 Hook，其他hook请详见官方文档，或者自己编写hook函数
注意：Hook 比普通函数更为严格。你只能在你的组件（或其他 Hook）的 顶层 调用 Hook。如果你想在一个条件或循环中使用 useState，请提取一个新的组件并在组件内部使用它
PS： 若要在一个大组件里面同时控制小组的state，则需要把小组件里的state移到大组件里面去，例子如下：
```
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
当你点击按钮时，onClick 处理程序会启动。每个按钮的 onClick prop 会被设置为 MyApp 内的 handleClick 函数，所以函数内的代码会被执行。该代码会调用 setCount(count + 1)，使得 state 变量 count 递增。新的 count 值会被作为 prop 传递给每个按钮，因此它们每次展示的都是最新的值。这被称为“状态提升”。通过向上移动 state，我们实现了在组件间共享它。
```


  