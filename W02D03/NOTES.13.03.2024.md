## Basic Function execution rules

  Understand the difference between local function scoped variables that get discarded and re-defined on each function call, and global variables (outside the function scope) that retain their values across different calls:

  ```js
  // V, Normal local function variables that get redefined on each function call: 0 => 1 every time
  function show(){
    let counter = 0;
    counter++;
  }
  ```

  ```js
  // counter here is a global (State) variable
  // that retains its value across each function call
  // 0 => 1 => 2 ... n
  let counter = 0;
  function show2(){
    counter++;
  }
  ```

# How to think about Rendering (Component function calls):

function Box(){
  return <></>
}

function App(){
  return (
    <>
      <Box /> => Box() => renders
      <Box /> => Box() => renders
    </>
  )
}

<App /> => App() => A Component renders
<Box /> => Box()