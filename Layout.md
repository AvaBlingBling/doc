### 流布局


### 浮动布局


### 定位布局


### flex布局


### grid布局

```
.container {
  width: 500px;
  display: grid;
  // fr是指定格网空间如何划分的一种弹性单位，grid-template-columns代表格网列属性，grid-template-rows对应行属性
  grid-template-columns: 1fr 1fr 1fr;
  grid-auto-rows: 100px;
  grid-gap: 20px;
  align-content: space-between;
  justify-content: end;
}
```


### 多列布局（报纸）

```
column-width
column-count
column-gap
column-span子元素
```