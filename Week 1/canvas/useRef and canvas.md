## useRef

**React useRef**: hook that allows data to persist between renders:

```js
// using useRef to keep track of component renders

import {useEffect, useRef} from 'react';

function App() {
  const count = useRef(0);

  useEffect((=>{
    count.current = count.current + 1;
  }),[])
}
```

Data stored in `useRef()` is accessed with its child `current`.

`useRef()` returns one item: the object `current`

## in this project
`useRef()` will be our way to access the `canvas` element and its properties on the DOM:

### 1. Create react canvas component

```js
const Canvas: React.FC<{}> = () => {
  return <canvas></canvas>
}
```
This will create a bare `<canvas>` element in the html output


### 2. use `useRef()` to access the element
``` js
const Canvas: React.FC<{}> = () => {
  const canvasRef = useRef(null);
  return <canvas ref = {canvasRef}></canvas>
}
```

Without `useRef()`, changes to the canvas will trigger a rerender, nullifying the changes.

### 3. Typing canvasRef

```js
const canvasRef = useRef<HTMLCanvasElement |null>(null);
```

### 4. Accessing/typing context

```js
const ctxRef = useRef<CanvasRenderingContext2D | null>(null)
```

### 5. Instantiating context

Populating the context correctly can only happen once the page is loaded. Therefore, we use `useEffect()`

```js
useEffect(()=>{
  if (canvsRef.current){
    ctxRef.current = canvasRef.current.getContext('2d');
  }
},[])
```

### 6. Actual canvas stuff

Finally, the actual stuff can happen in the `useEffect()`

```js
useEffect(()=>{
  if (canvsRef.current){
    ctxRef.current = canvasRef.current.getContext('2d');
    let ctx = ctxRef.current;
    // non-null assertion: if we've gotten this far, we know ctx is not null. TS cannot parse the logic, though.
    ctx!.beginPath();
    ctx!.arc(100,100,20, 0, 2*Math.PI);
    ctx!.stroke();
  }
},[])
```

credit to [Blaine Garrett](https://hashnode.com/@blainegarrrett) and [this article](https://hashnode.blainegarrett.com/html-5-canvas-react-refs-and-typescript-ckf4jju8r00eypos1gyisenyf)