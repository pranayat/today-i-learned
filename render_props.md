# Render Props
https://reactjs.org/docs/render-props.html

## The Lowdown
* I've got a Mouse component which stores mouse position in State
* If Cat needs this mouse location, we make it a child of Mouse Component renaming it to MouseWithCat
* If Dog needs this mouse location, we make it a child of Dog Component renaming it to MouseWithDog
* We can avoid creating two separate components using the same mouse functionality by using render props
* Render Props: Just call <Mouse> and pass a prop called 'render' which renders the component you need (Cat or Dog) and also
  pass the state of Mouse componentt to this component. eg <Mouse render={(mouse) => <Cat mouse={mouse}} /> // the value of 'mouse' prop will be passed inside render of <Mouse>, this.props.render(this.state).
* If you want 'Dog' do <Mouse render={(mouse) => <Dog mouse={mouse}} /> /}

* Basically a prop which is a function which returns JSX which can be rendered
  in the componentt recieving the props
  ```
  class A extends React.Component {
  constructor(){
  this.state = {
    name: 'A'
  }
  render(){
    return(<div>
      {this.props.myRender(this.state.name)}
      </div>)
  }
  
  
  
  class B extends React.Component {
    return(
      <div> The stuff is {this.props.stuff} </div>
      )
  }
  
  
  
  class Test extends React.Component {
    render(){
    return(
    {<div>
      Render Props Example
      <A myRender = { (stuff) => {
                                    return(<B stuff={stuff} />)
                                   }
                    } />
      </div>
     }
    }
  }
  
  ```
  A supplies 'this.state.name' to B's 'stuff' prop.
  Then A calls its myRender prop function which renders B, which renders itself with the 'stuff' prop.
