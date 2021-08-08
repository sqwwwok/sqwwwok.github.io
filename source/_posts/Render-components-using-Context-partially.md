---
title: Render components using Context partially
categories:
  - React
---


[reference](https://zhuanlan.zhihu.com/p/51073183)

```jsx
import React, { useContext, createContext, useState } from "react";

const ThemeColorChangedBits = 0b10;
const ThemeCountChangedBits = 0b01;

const Context = createContext(null, (prev, next) => {
  let result = 0;
  if (prev.theme !== next.theme) {
    result |= ThemeColorChangedBits;
  }
  if (prev.count !== next.count) {
    result |= ThemeCountChangedBits;
  }
  console.log("calculatedBits ", result);
  return result;
});

const redTheme = {
  color: "red"
};

const greenTheme = {
  color: "green"
};


function Content() {
  console.log("render Content");
  const { theme, switchTheme } = useContext(Context, ThemeColorChangedBits);
  //const { theme, switchTheme } = useContext(Context);

  return (
    <>
      <h1 style={theme}>Hello world</h1>
      <button onClick={() => switchTheme(redTheme)}>Red Theme</button>
      <button onClick={() => switchTheme(greenTheme)}>Green Theme</button>
    </>
  );
}

function Counter() {
  console.log("render Counter");
  const { count, increment } = useContext(Context, ThemeCountChangedBits);
  //const { count, increment } = useContext(Context);

  return (
    <>
      <h1>{count}</h1>
      <button onClick={() => increment()}>+</button>
    </>
  );
}

function Header() {
  //console.log("render Header");
  return <h1>Hello CodeSandbox</h1>;
}


function ThemeProvider ({children}) {
  const [theme, setTheme] = useState(redTheme)
  const [count, setCount] = useState(0)

  function switchTheme(theme) {
    setTheme(theme)
  }

  function increment() {
    setCount(count+1)
  }

  // 注意不要直接把Content的消费者直接写进子组件，不然setState后所有子组件都会重新渲染
  // <Context.Provider
  //   value={{
  //     theme: theme,
  //     count: count,
  //     switchTheme: switchTheme,
  //     increment: increment
  //   }}
  // >
  //   <div className="App">
  //     <Header />
  //     <Content />
  //     <Counter />
  //   </div>
  // </Context.Provider>

  return (
    <Context.Provider
      value={{
        theme: theme,
        count: count,
        switchTheme: switchTheme,
        increment: increment
      }}
    >
      {children}
    </Context.Provider>
  )
}

function App() {
  //console.log("render App");
  return (
    <ThemeProvider>
      <div className="App">
        <Header />
        <Content />
        <Counter />
      </div>
    </ThemeProvider>
  );
}

export default App;
```
