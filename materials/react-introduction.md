# React 介绍

## React 是什么

React 是一个用于构建用户界面的 JavaScript 库，由 Meta（原 Facebook）及开源社区维护。它适合开发从简单交互页面到大型单页应用的前端界面。

React 的核心思想是：把页面拆分成可复用的组件，并根据数据变化声明式地描述界面应该呈现的状态。

## 核心概念

### 组件（Component）

组件是 React 应用的基本构建单元。组件通常是返回 JSX 的 JavaScript 函数：

```jsx
function Welcome({ name }) {
  return <h1>你好，{name}！</h1>;
}
```

组件应尽量职责单一、接口清晰，这样更容易测试、复用和维护。

### JSX

JSX 是 JavaScript 的语法扩展，允许在代码中编写类似 HTML 的结构：

```jsx
const title = <h2 className="page-title">课程列表</h2>;
```

JSX 最终会被编译成创建 React 元素的 JavaScript 代码。JSX 中可以使用大括号插入表达式，但属性名通常遵循 JavaScript 命名规则，例如 `className` 而不是 `class`。

### Props

Props（属性）用于父组件向子组件传递只读数据：

```jsx
function ProductCard({ product }) {
  return (
    <article>
      <h2>{product.name}</h2>
      <p>价格：{product.price} 元</p>
    </article>
  );
}
```

子组件不应直接修改收到的 props；需要改变数据时，应通过回调通知父组件。

### State

State（状态）表示组件自身需要记住、并且可能随交互变化的数据。常用的 `useState` Hook 示例：

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      已点击 {count} 次
    </button>
  );
}
```

调用状态更新函数后，React 会重新渲染受影响的组件。更新对象或数组时应创建新值，而不是直接修改原值。

### Hooks

Hooks 是以 `use` 开头的函数，用于在函数组件中使用状态、生命周期相关能力和其他 React 功能。常见 Hook 包括：

- `useState`：保存和更新局部状态。
- `useEffect`：处理与外部系统同步的副作用，例如请求数据或订阅事件。
- `useContext`：读取跨层级共享的上下文数据。
- `useMemo` 与 `useCallback`：在确有性能需要时缓存计算结果或函数。

Hooks 只能在组件或自定义 Hook 的顶层调用，不能放在条件、循环或普通函数中。

## React 如何更新界面

React 使用声明式渲染：开发者描述给定数据下的 UI，React 负责将数据变化反映到 DOM。组件状态或 props 改变时，React 会重新计算组件输出，并通过协调过程尽量只更新必要的 DOM 节点。

渲染列表时应提供稳定且唯一的 `key`，帮助 React 识别列表项：

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

不要使用会随渲染变化的随机值作为 `key`，也不要在没有必要时使用数组下标。

## 一个完整的小例子

下面的组件展示了受控输入框和列表渲染：

```jsx
import { useState } from 'react';

export default function TodoApp() {
  const [text, setText] = useState('');
  const [todos, setTodos] = useState([]);

  function addTodo(event) {
    event.preventDefault();
    const title = text.trim();
    if (!title) return;

    setTodos((current) => [
      ...current,
      { id: crypto.randomUUID(), title },
    ]);
    setText('');
  }

  return (
    <section>
      <form onSubmit={addTodo}>
        <input
          value={text}
          onChange={(event) => setText(event.target.value)}
          placeholder="输入待办事项"
        />
        <button type="submit">添加</button>
      </form>

      <ul>
        {todos.map((todo) => <li key={todo.id}>{todo.title}</li>)}
      </ul>
    </section>
  );
}
```

这里的输入框由 React 状态控制，因此称为“受控组件”。使用函数形式的状态更新可以基于最新状态安全地追加列表项。

## React 生态

React 主要负责 UI 层，实际项目通常还会组合其他工具：

- **构建工具**：Vite、Create React App（历史项目中常见）。
- **路由**：React Router 等，用于管理多页面视图和 URL。
- **数据请求与缓存**：原生 `fetch`、TanStack Query 等。
- **样式方案**：CSS Modules、普通 CSS、Tailwind CSS 或组件库。
- **全局状态**：优先使用 props 和 context；复杂场景可选择 Redux、Zustand 等。

选择依赖时应根据项目规模、团队经验和维护成本判断，而不是为了使用更多库而使用库。

## 学习与实践建议

1. 先掌握现代 JavaScript：模块、数组方法、异步编程和不可变数据更新。
2. 从组件、props、state 和事件处理开始，制作一个小型交互应用。
3. 学会拆分组件，并明确每个状态应该由哪个组件拥有。
4. 练习列表、表单、加载状态、错误状态和空状态等真实场景。
5. 使用 React 官方文档和浏览器开发者工具检查渲染与状态变化。
6. 关注可访问性、性能和测试，不要只关注页面是否“能显示”。

## 小结

React 通过组件化、JSX、props、state 和 Hooks，让开发者能够以声明式方式构建可维护的用户界面。学习 React 的重点不是记忆 API，而是理解数据流、组件边界和状态管理，并在真实交互中持续练习。
