---
title: 'AXISJ GRID PageNo'
date: 2017-01-09 19:13:45
tags: [javascript, axisj]
categories:
- Programming
- Javascript
---

# AXISJ GRID PageNo

목적 : 그리드에서 변경한 페이지 정보 유지

그리드 생성 HTML 코드
```html
<div class="ez-AXGrid ax-grid" id="axGrid" data-options="axGridConfig"></div>
```

그리드 기본 설정
```javascript
$(".ez-AXGrid").each(function(t, a) {
        var n = $(a),
            i = n.attr("id"),
            o = n.data("options") || "{}",
            r = new AXGrid,
            d = {
                targetID: i,
                mediaQuery: e,
                page: {
                    paging: !0,
                    pageSize: 20
                },
                height: 600,
                colHead: {
                    onclick: function() {
                        if (r.ajaxInfo) {
                            if (r.ajaxInfo.ajaxPars) {
                                var e = $.extend(r.ajaxInfo.ajaxPars.queryToObject(), r.getSortParam("one").queryToObject());
                                r.ajaxInfo.ajaxPars = axdom.param(e, !0)
                            } else r.ajaxInfo.ajaxPars = r.getSortParam("one");
                            r.reloadList()
                        }
                    }
                },
                colHeadAlign: "center"
            };
        return $.extend(d, o.object()), "colGroup" in d == 0 ? void trace("AXJ.easy.js .ez-AXGrid [ERROR] colGroup 옵션을 반드시 설정해야 합니다.") : (r.setConfig(d), window[i] = r, gv && gv.axGrids && gv.axGrids.push(r), void trace('AXJ.easy.js .ez-AXGrid [id="' + i + '"]'))
    })
```

`axGrid.page.pageNo` 에서 현재 페이지 정보를 얻을 수 있다.

현재 페이지 정보를 view 페이지로 넘어갈때에 정보를 가지고 간다.

```javascript
var config = {
  body: {
    onclick: function(){
      // 페이지 정보를 파라미터로 넘긴다.
    }
  }
}
```

`c:url` 태그는 서버에서 해석되는 jstl(JavaServer Pages Standard Tag Library) 구문임. 클라이언트에서 해당 태그는 보이지 않는다. 클라이언트에서는 변환된 문자열이 보일 뿐 이다.

클릭시에, 현재 페이지 정보를 받아서, 파라미터로 넘기기 위해 동적인 URL 을 생성해서 서버로 전송해야 한다.
