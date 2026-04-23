<div style="display: flex; flex-direction: row; justify-content: space-around;">

<div>
  
```css
font-style: italic;
font-weight: bold;
font-size: 50px;
line-height: 50px;
font-family: Georgia;
```
</div>
<div>
  
```css
font: italic bold 50px/50px Georgia;
```
</div>
</div>

## col grp is something that we can use on top of out table tr , td , th s properties ... where irrespective of the whether header or not
## u can just apply styling to it with col group
```html
<colgroup>
  <col span="2" style="background-color:red">
  <col style="background-color:yellow">
  <col span="1" style="background-color:pink">
  <col style="background-color:yellow">
</colgroup>
```
```text
if i write some random like this . html works in order
- first it will take first 2 cols and apply color red
- second it will take that 3 apply rellow
- after rakes 1 apply pink
so on
```
```html
<!DOCTYPE html>
<html>
<head>
<style>
table, th, td {
  border: 1px solid black;
}
table td:first-child
{
background-color:purple;
}
<!---
table td:first-child
{
background-color:purple;
}
-->
td:first-child { border-left: none; }
table td:first-child + td { font-family: monospace; }
</style>
</head>
<body>

<h1>The col element</h1>

<table>
  <colgroup>
    <col span="2" style="background-color:red">
    <col style="background-color:yellow">
  </colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
  <tr>
    <td>5869207</td>
    <td>My first CSS</td>
    <td>$49</td>
  </tr>
</table>

</body>
</html>

```
```jsx
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    // Initialize state with a count of 0
    this.state = {
      count: 0
    };
  }

  // Method to increment count using an arrow function for auto-binding
  increment = () => {
    this.setState((prevState) => ({
      count: prevState.count + 1
    }));
  };

  // Method to decrement count
  decrement = () => {
    this.setState((prevState) => ({
      count: prevState.count - 1
    }));
  };

  render() {
    return (
      <div style={{ textAlign: 'center', marginTop: '50px' }}>
        <h1>Class Component Counter</h1>
        <h2>Current Count: {this.state.count}</h2>
        <button onClick={this.increment}>Increment</button>
        <button onClick={this.decrement} style={{ marginLeft: '10px' }}>
          Decrement
        </button>
      </div>
    );
  }
}

export default Counter;

```

```js
let name = "hari"

function test()
{
    let name ="charan";
    function pika()
    {
        console.log(name)
    }
    pika()
}
test()
console.log(name)
```
