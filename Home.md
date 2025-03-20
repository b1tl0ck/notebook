## 足迹
```contributionGraph
title: "2025"
graphType: default
dateRangeValue: 1
dateRangeType: LATEST_YEAR
startOfWeek: 1
showCellRuleIndicators: true
titleStyle:
  textAlign: center
  fontSize: 14px
  fontWeight: normal
dataSource:
  type: PAGE
  value: ""
  dateField: {}
  countField:
    type: DEFAULT
fillTheScreen: true
enableMainContainerShadow: false
mainContainerStyle: {}
cellStyleRules:
  - id: default_b
    color: "#9be9a8"
    min: 1
    max: 2
  - id: default_c
    color: "#40c463"
    min: 2
    max: 5
  - id: default_d
    color: "#30a14e"
    min: 5
    max: 10
  - id: default_e
    color: "#216e39"
    min: 10
    max: 999
cellStyle:
  minHeight: 10px
  minWidth: 10px

```
- [*] 今天是`=dateformat(date(today),"DD")`，`=date(today).year` 年已经过去了 `=(date(today)-date(date(today).year + "-01-01")).days` 天
- [*] 今年你总共在天中留下了你的足迹，继续加油！
---
## TODO
- [ ] mysql注入及Trick小结
- [ ] sql注入方法及绕过
**这两个文件需要重新复制，更新附件的路径**

---
## 最近

```dataview
table without id file.link as "文件", status as "状态", dateformat(file.ctime,"y/MM/dd") as "创建时间", dateformat(file.mtime,"y/MM/dd") as "最近修改时间"
from ""
where file.name != "Home"
sort status desc
limit 10
```
