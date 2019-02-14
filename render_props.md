# Render Props
https://reactjs.org/docs/render-props.html

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
