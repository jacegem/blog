---
title: 'Vue.js Start'
date: 2017-04-10 19:13:45
tags: [javascript, vue, start]
categories:
- Programming
- Javascript
---


# Vue.js Start

<table class="table table-bordered table-striped table-org table-hover" ng-controller="StatEventCtrl" id="StatEventCtrl">

id="StatEventCtrl"

id를 대상으로 vue 생성

vue repeat

v-for="(item, index) in items

eventTypeClass 필터를 생성

<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>

v-bind

:class="{
        'first': index == 0,
        'odd': !(index % 2),
        'even': index % 2,
        'last': index == (filteredItems.length - 1)
      }"



app.filter('eventTypeClass', function(){
	return function(event_type){
		switch(event_type){
			case '0001': return 'event_emergency';
			case '0002': return 'event_missing';
			case '0003': return 'event_crime';
			case '0004': return 'event_facility';
			case '0005': return 'event_traffic';
			default : return 'event_etc';
		}    		
	}
})

app.filter('spreadTypeClass', function(){
	return function(type){
		switch(type){
			case '0001': return 'spread_emergency';    			
			case '0002': return 'spread_missing';
			case '0003': return 'spread_crime';
			case '0004': return 'spread_facility';
			case '0005': return 'spread_traffic_accident';
			case '0006': return 'spread_traffic_control';
			case '0007': return 'spread_fastival';
			default : return 'spread_etc';
		}    		
	}
})

<tr v-for="data in datas" v-cloak :click="setView(data.st_x, data.st_y)">


wmap.setView(x, y);

// 댓글 정보 조회
var scope = angular.element(document.getElementById("popSpreadDetail")).scope();
scope.update(data.spread_seq, "<c:url value='/web/stat/spreadDetailReply.json' />");

var scope = angular.element(document.getElementById("popEventDetail")).scope();
scope.update(base.event_seq, "<c:url value='/web/stat/eventDetailReply.json' />");

vue_event_detail.update(base.event_seq);


var vue_stat_event_ctrl = new Vue({
    vue_stat_event_ctrl.update("<c:url value='/web/stat/statEventList.json' />");

var vue_stat_spread_ctrl = new Vue({
    vue_stat_spread_ctrl.update("<c:url value='/web/stat/statSpreadList.json' />");


url : "/web/stat/spreadRegist.json",

<td><select class="form-control" type="text" title="제목" onchange="fnChangeSpreadRegistTitle(this)">
		<option value="0000" selected>직접입력</option>
		<c:forEach items="${codeSpreadText}" var="code" varStatus="status">
			<option value="${code.code_id}">${code.code_name}</option>
		</c:forEach>
	</select>									
	<input id="spreadRegistTitle" type="text" name="title" value="" class="form-control" />
</td>

this.options[this.selectedIndex].text
