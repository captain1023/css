#antd

今儿用antd 实现了一个嵌入式列表


expandedRowRender	额外的展开行	function(record, index, indent, expanded): ReactNode
record->传入给外层表格的信息
index->下标
indent->缩进
expanded->是否展开 (每次点击展开按钮数据会变)

查阅资料时发现

当我们需要通过外层表格的信息去调用后端信息(dispatch,jQuery ,etc..)

不能将相关代码放在expandedRowRender函数中

因为 expandedRowRender 实际上是在 Table 组件的 render 方法中调用的，React render 中用 dispatch 会造成重复调用的问题，dispatch -> setState -> render -> dispatch -> setState -> render，循环往复。所以应该把 dispatch 放在 onExpand 中。

onExpand = (expanded, record)
record是外层表格的信息
当每次点击onexpand的时候，通过外层表格传递的信息调用后端信息
但此时onexpand设置state信息后所有已经展开的表格会变成当前的这个子表格
所以使用键值对方式来解决

```
class ProductList extends PureComponent {
  constructor(props) {
    super(props)
    this.state = {
      lineTabData:[ ]
    }
}

  render() {
    const columns = [表格数据]
    return (    
        <Table
          rowKey={record => record.id}
          columns={columns}
          dataSource={dataSource}
          onExpand={(expanded,record)=>this.onExpand(expanded,record)}
          expandedRowRender={record => this.expandedRowRender(record)}
      />
    )
  }

}
1
```

```
onExpand = (expanded, record) => {
    const { dispatch } = this.props
      dispatch({
        type: 'brand/getArrival',
        payload: {id:record.id},
      }).then(dataArr => {
        this.setState({
          lineTabData: {
            ...this.state.lineTabData,
            [record.id]: dataArr ,
          }
        })
      })
}
```

```
expandedRowRender = (record) => {
 console.log(this.state.lineTabData[record.id])//打印出对应id展开行所请求的数据
   return (    
       <p>this.state.lineTabData[record.id]</p>
   )
 }
 ```

 onexpand 请求 更新数据

 expandedRowRender 渲染内层数据

 table 渲染外层数据
