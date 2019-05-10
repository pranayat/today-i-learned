# React Lifecycle Methods Cheatsheet

constructor ( Mount phase)

|

getDerivedStateFromProps

|

shouldComponentUpdate ( Update phase)

|

render ( First  render in Mount phase followed by subsequent renders in Update phase)

| (getSnapshotBeforeUpdate)

componentDidMount (Mount phase), componentDidUpdate (Update phase), componentWillUnmount (Unmount phase)

# setState and re-renders

regardless of whether state is set in constructor, componentWillMount or componentDidMount, render will be triggered again

* render()

cannot setState() inside render, pure function
