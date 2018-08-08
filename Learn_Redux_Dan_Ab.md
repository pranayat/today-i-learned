```javascript
const counter = (state = 0, action) => {
  switch(action.type){
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
       return state -1
    default:
      return state
  }
}

const Counter = ({ value,onIncrement,onDecrement }) => {
   return(
   <div>
     <h1>{value}</h1>
     <button onClick={onIncrement}>+</button>
     <button onClick={onDecrement}>-</button>
   </div>
   )
}

const { createStore } = Redux
const store = createStore(counter)

const render = () => {
  ReactDOM.render(
   <Counter value={store.getState()} 
            onIncrement={() =>
                        store.dispatch({
                         type : 'INCREMENT'
                        })}
            onDecrement={() => {
                         store.dispatch({
                         type : 'DECREMENT'
                        })}/>,
    document.getElementById('root')
)
```
# Reducer composition
```javascript
const todo = (state,action) => {
  switch(action.type) {
    case 'ADD_TODO' :
      return {
        id : action.id,
        text : action.text,
        completed : false
        }
     case 'TOGGLE_TODO' :
        if(state.id !== action.id){
            return state
         }
         return {
            ...state,
            completed : !state.completed
         }
       default : return state
}

const todos = (state = [], action) => {
  switch(action.type){
    case 'ADD_TODO': //thid reducer handles returning the new state
      return([
        ...state,
        todo(undefined,action) //the actual logic is delegated to this reducer
        ])
    case 'TOGGLE_TODO':
      return state.map(t => todo(t,action))
     default : 
        return state
  }
}
```
