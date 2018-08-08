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

store.subscribe(render) //everytime store updates. render is called
render() // call once for initial render
```
# Reducer composition
1. Extract to smaller reducers

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
2. Combine multiple reducers
```javascript
 const visibilityFilter = (state = 'SHOW_ALL', action) => {
    switch(action.type){
      case 'SET_VISIBILITY_FILTER':
        return action.filter
      default : return state
      }
 }
 
 //this reducer sends down the state variables 'todos' and 'visibilityFilter' to reducers 'todos' and 'visibilityFilter' respectively, these return the updated values of 'todos' and 'visibilityFilter' and the resulting updated state object is 
returned by the 'toDoApp' reducer

 const toDoApp = (state = {},action) => {
    return({
      todos : todos(state.todos,action), //only hand over 'todos' to this reducer
      visibilityFilter : visibilityFilter(state.visiblityFilter,action)
    })
 }
    }
 }
```
<strong>combineReducers</strong> does this for us. It takes in an object containing all the reducers and returns a top level reducer.
```javascript
const { combineReducers } = Redux;
const toDoApp = combineReducers({
  todos : todos,
  visibilityFilter : visibilityFilter
  })  
```
The keys `todos` and `visiblityFilter` correspond to the respective fields in the state that will be handled by the respective reducers. The result will be combined into a single state object. Conventionally you give the reducer the same name as the state field they manage.

Combine Reducers implementation : 
```javascript 
cosnt combineReducers = (reducers) => {
      return (state = {}, action) => { //returns a reducer function
        return Object.keys(reducers).reduce(
           
           (nextState,key) => {
            nextState[key] = reducers[key](state[key],action)  //call the reducer for that key and assign it to the corresp. state field         
            return nextState
            },
            {}) //returns single object with the state keys as keys and corresponding reducer as the value
      }
}
```
# Connect
 
 ```javascript
 
 ////////////////// container component below IS REPLACED BY connect() which generates a similar container component
 //////////////////and mapStateToProps and mapDispatchToProps
 
 class VisibleTodoList extends Component {
  
  componentDidMount(){  
    const { store } = this.context
    this.unsubscribe = store.subscribe(() => {
        this.forceUpdate()
        })
    
    componentWillUnmount(){
        this.unsubscribe()
      }
     
    render(){
      const props = this.props
      const { store } = this.context
      const state = store.getState()
      
      return(
        <TodoList todos={
                          getVisibleTodos(state,todos,state.visibilityFilter)
                        } 
                  onTodoClick={(id) => {
                          store.dispatch({
                            type : 'TOGGLE_TODO',
                            id : id
                          })    
                        }                        
                  } />
      )
    }
    visibleTodoList.contextTypes = {
      store : React.PropTypes.object
    }
}
/////////////////////////////////////////////
/////////////////connect() //////////////////

const { connect } = ReactRedux

const mapStateToProps = (dispatch) => {
  return ({
    onTodoClick : (id) => {
        dispatch({
            type: 'TOGGLE_TODO',
            id
        })
    }
  })
}

const mapDispatchToProps = (dispatch) => {
    return({
      onTodoClick : (id) =>   
        dispatch({
            type : 'TOGGLE_TODO',
            id
            })
     })
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
 )(TodoList)
 
/////////////////////////////////////////////
const TodoApp = () => (
   <div>
      <AddTodo />
      <VisibleTodoList />
      <Footer />
   </div>
  )
  
 const { Provider } = ReactRedux
 const { createStore } = Redux
 
 ReactDOM.render(
  <Provider store={createStore(todoApp)}>
      <TodoApp />
  </Provider>,
  document.getElementById('root')
 )
 ```


