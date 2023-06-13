# Node.js 生成 xlsx 文档

```js
var xls = require("exceljs");

// 定义：变量
var tabType = "admin";
var tabNum = 1;

console.log(tabType);
console.log(tabNum);

function operation(docs, tabType, tabNum, time, fileDateDir) {
    var reportName = docs.reportName;                                           // 报表名称
    var reportCompanyName = docs.reportCompanyName;                             // 报告公司名称
    var generationTime = time;                                                  // 定义生成时间
    var startTime = moment(parseInt(start)).format("YYYY-MM-DD HH:mm:ss");      // 开始时间
    var endTime = moment(parseInt(end)).format("YYYY-MM-DD HH:mm:ss");          // 结束时间

    var data1 =  [
        {
            TabType : tabType,
            TabNum : tabNum,
            ReportCompanyName : reportCompanyName,
            GenerationTime : generationTime,
            StartTime : startTime,
            EndTime : endTime,
        }
    ]
    // 新建一个工作表
    var workbook = new xls.Workbook();
    // 创建日期
    workbook.created = new Date();
    // 修改日期
    workbook.modified = new Date();
    // 作者名称
    workbook.creator = 'test';
    // 最后修改人
    workbook.lastModifiedBy = 'test';

    // 添加sheet，并且初始化该sheet的名称
    let sheet = workbook.addWorksheet(reportName);

    // 设置表头
    sheet.columns = [
        {header: '类别', key: 'TabType', width: 15},
        {header: '总量', key: 'TabNum', width: 15},
        {header: '报告公司名称', key: 'ReportCompanyName', width: 15},
        {header: '生成时间', key: 'GenerationTime', width: 15},
        {header: '开始时间', key: 'StartTime', width: 15},
        {header: '结束时间', key: 'EndTime', width: 15},
    ];

    // 添加多行，data1要是个数组类型(能用foreach遍历)
    sheet.addRows(data1);

    // 写文件 指定文件路径_findir + /xlsx
    workbook.xlsx.writeFile(fileDateDir + "/xlsx_report.xlsx")
        .then(function() {
            console.log('write done')
        });
};
```
