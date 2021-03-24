## The Standard UI Update Cycle

Using Redux with any UI layer requires the same consistent set of steps:

1. Create a Redux store
2. Subscribe to updates(监听更新)
3. Inside the subscription callback:
   1. Get the current store state
   2. Extract the data needed by this piece of UI
   3. Update the UI with the data
4. If necessary, render the UI with initial state
5. Respond to UI inputs by dispatching Redux actions



一个简单的counter例子。

```
// 1) Create a store
const store = createStore(counter)

// 2) Subscribe to store updates
store.subscribe(render);

const valueEl = document.getElementById('value');

// 3. When the subscription callback runs:
function render() {
    // 3.1) Get the current store state
    const state = store.getState();

    // 3.2) Extract the data you want
    const newValue = state.toString();

    // 3.3) Update the UI with the new value
    valueEl.innerHTML = newValue;
}

// 4) Display the UI with the initial store state
render();

// 5) Dispatch actions based on UI inputs
document.getElementById("increment")
    .addEventListener('click', () => {
        store.dispatch({type : "INCREMENT"});
    })
```

