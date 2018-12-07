## 将页面中的表格数据导出excel

### 1、 安装相关依赖
```
 npm install -S file-saver xlsx
 
 npm install script-loader -S -D
```

### 2、在文件夹中加入Export2Excel.js文件
    直接在vendor文件夹中拷贝文件即可
    
    
### 3、在程序中加入以下两个方法
由于js-xlsx体积还是很大的，导出功能也不是一个非常常用的功能，所以使用的时候建议使用懒加载。例如下面的import('@/vendor/Export2Excel')
```
tableExport(bookType) {
  this.downloadLoading = true
  import('@/vendor/Export2Excel').then(excel => {
    const tHeader = ['timestamp', 'title', 'type', 'importance', 'status'] // 表头
    const filterVal = ['timestamp', 'title', 'type', 'importance', 'status'] // 表头对应字段名
    const list = this.list // 数据来源
    const data = this.formatJson(filterVal, list) //对导出数据格式化处理
    excel.export_json_to_excel({
      header: tHeader, //表头 必填
      data, //具体数据 必填
      filename: 'table-list', // 可自定义文件名，非必填
      autoWidth: true, //是否自适应宽度，非必填，默认true
      bookType //导出类型xlsx、csv、txt，非必填，默认xlsx
    })
    this.downloadLoading = false
  })
},
formatJson(filterVal, jsonData) {
  return jsonData.map(v => filterVal.map(j => {
    if (j === 'timestamp') {
      return parseTime(v[j])
    } else {
      return v[j]
    }
  }))
}
```
### 4、支持两种导出方式
```
<el-button :loading="downloadLoading" type="primary" icon="el-icon-download" @click="exportExcel('xlsx')">导出</el-button>

或者

<el-button :loading="downloadLoading" type="primary" icon="el-icon-download" @click="exportExcel('csv')">导出</el-button>
```
