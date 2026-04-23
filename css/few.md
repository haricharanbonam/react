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
