---
layout: post
title:  "multiselect only checkbox"
date:   2016-08-10
categories: jqgrid
tags: javascript jqgrid
excerpt: multiselect only checkbox
---

`jqGrid 에서 multiselect` 옵션 사용시 row 단위로 check box 가 된다.


`event` 발생한 위치 상위 `td`의 인덱스 를 구해서
grid col index 위의 `name` 가져와서 비교 `cb`

`jqGrid multiselect` 생성되는 체크박스 이름은 `cb`인듯하다.

```
beforeSelectRow: function (rowid, e) {
 var $myGrid = $(this),
 i = $.jgrid.getCellIndex($(e.target).closest('td')[0]),
 cm = $myGrid.jqGrid('getGridParam', 'colModel');
 return (cm[i].name === 'cb');
}
```
