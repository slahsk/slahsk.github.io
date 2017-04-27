---
layout: post
title:  "jqgrid multiselect get row data"
date:   2016-10-10
categories: jqgrid
tags: javascript jqgrid
excerpt: multiselect 사용시 체크 되어 있는 index 가져오기
---

multiselect 사용시 체크 되어 있는 index 가져오기

```
var indexs = $(id).jqGrid('getGridParam', 'selarrrow');
$.each(indexs,function(){
 var rowData =  $(id).jqGrid('getRowData', this);
});

```
