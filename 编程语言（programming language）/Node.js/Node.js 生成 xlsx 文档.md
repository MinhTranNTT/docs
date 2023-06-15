# Node.js 生成 xlsx 文档

`var tabType = "admin";` 定义：变量 `docs.reportName` 报表名称 `docs.reportCompanyName` 报告公司名称 `time` 定义生成时间 `moment(parseInt(start)).format("YYYY-MM-DD HH:mm:ss")` 开始时间 `moment(parseInt(end)).format("YYYY-MM-DD HH:mm:ss")` 结束时间 `new xls.Workbook()` 新建一个工作表
` workbook.created` 创建日期 `modified` 创建日期 `creator` 作者名称 `lastModifiedBy` 最后修改人 `let sheet = workbook.addWorksheet(reportName);` 添加sheet，并且初始化该 `sheet` 的名称 `sheet.columns` 设置表头 `sheet.addRows(data1);` 添加多行，`data1` 要是个数组类型(能用 `foreach` 遍历) 写文件 指定文件路径 `findir + /xlsx`

```js
var xls = require("exceljs");
var tabType = "admin";
var tabNum = 1;

console.log(tabType);
console.log(tabNum);

function operation(docs, tabType, tabNum, time, fileDateDir) {
    var reportName = docs.reportName;
    var reportCompanyName = docs.reportCompanyName;
    var generationTime = time;
    var startTime = moment(parseInt(start)).format("YYYY-MM-DD HH:mm:ss");
    var endTime = moment(parseInt(end)).format("YYYY-MM-DD HH:mm:ss");
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

    var workbook = new xls.Workbook();
    workbook.created = new Date();
    workbook.modified = new Date();
    workbook.creator = 'test';
    workbook.lastModifiedBy = 'test';
    let sheet = workbook.addWorksheet(reportName);
    sheet.columns = [
        {header: '类别', key: 'TabType', width: 15},
        {header: '总量', key: 'TabNum', width: 15},
        {header: '报告公司名称', key: 'ReportCompanyName', width: 15},
        {header: '生成时间', key: 'GenerationTime', width: 15},
        {header: '开始时间', key: 'StartTime', width: 15},
        {header: '结束时间', key: 'EndTime', width: 15},
    ];

    sheet.addRows(data1);
    workbook.xlsx.writeFile(fileDateDir + "/xlsx_report.xlsx")
        .then(function() {
            console.log('write done')
        });
};
```
