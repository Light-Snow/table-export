## 将页面中的表格数据导出excel

### 1、 安装相关依赖
```
 npm install -S file-saver xlsx
 
 npm install -D script-loader
```

### 2、在文件夹中加入Export2Excel.js文件
    直接在vendor文件夹中拷贝文件即可
    
    
### 3、在程序中加入以下两个方法
```
tableExport(bookType) {
  import('@/vendor/Export2Excel').then(excel => {
    const tHeader = ['timestamp', 'title', 'type', 'importance', 'status'] // 表头
    const filterVal = ['timestamp', 'title', 'type', 'importance', 'status'] // 表头对应字段名
    const data = this.formatJson(filterVal, this.list)
    excel.export_json_to_excel({
      header: tHeader,
      data,
      filename: 'table-list', // 可自定义文件名
      bookType
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
