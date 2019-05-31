# Bookmarklet


> Jesse's Bookmarklets Site https://www.squarefree.com/bookmarklets/


- **高亮特定文字**

```javascript
javascript:(function(){var count=0, text, dv;text=prompt("Search phrase:", "");if(text==null || text.length==0)return;dv=document.defaultView;function searchWithinNode(node, te, len){var pos, skip, spannode, middlebit, endbit, middleclone;skip=0;if( node.nodeType==3 ){pos=node.data.toUpperCase().indexOf(te);if(pos>=0){spannode=document.createElement("SPAN");spannode.style.backgroundColor="yellow";middlebit=node.splitText(pos);endbit=middlebit.splitText(len);middleclone=middlebit.cloneNode(true);spannode.appendChild(middleclone);middlebit.parentNode.replaceChild(spannode,middlebit);++count;skip=1;}}else if( node.nodeType==1&& node.childNodes && node.tagName.toUpperCase()!="SCRIPT" && node.tagName.toUpperCase!="STYLE"){for (var child=0; child < node.childNodes.length; ++child){child=child+searchWithinNode(node.childNodes[child], te, len);}}return skip;}window.status="Searching for '"+text+"'...";searchWithinNode(document.body, text.toUpperCase(), text.length);window.status="Found "+count+" occurrence"+(count==1?"":"s")+" of '"+text+"'.";})();
```

- **编辑页面**

```javascript
javascript:document.body.contentEditable = 'true'; document.designMode='on'; void 0
```

- **反色**

```javascript
javascript:(function(){function RGBtoHSL(RGBColor){with(Math){var R,G,B;var cMax,cMin;var sum,diff;var Rdelta,Gdelta,Bdelta;var H,L,S;R=RGBColor[0];G=RGBColor[1];B=RGBColor[2];cMax=max(max(R,G),B);cMin=min(min(R,G),B);sum=cMax+cMin;diff=cMax-cMin;L=sum/2;if(cMax==cMin){S=0;H=0;}else{if(L<=(1/2))S=diff/sum;else S=diff/(2-sum);Rdelta=R/6/diff;Gdelta=G/6/diff;Bdelta=B/6/diff;if(R==cMax)H=Gdelta-Bdelta;else if(G==cMax)H=(1/3)+Bdelta-Rdelta;else H=(2/3)+Rdelta-Gdelta;if(H<0)H+=1;if(H>1)H-=1;}return[H,S,L];}}function getRGBColor(node,prop){var rgb=getComputedStyle(node,null).getPropertyValue(prop);var r,g,b;if(/rgb\((\d+),\s(\d+),\s(\d+)\)/.exec(rgb)){r=parseInt(RegExp.$1,10);g=parseInt(RegExp.$2,10);b=parseInt(RegExp.$3,10);return[r/255,g/255,b/255];}return rgb;}function hslToCSS(hsl){return "hsl("+Math.round(hsl[0]*360)+", "+Math.round(hsl[1]*100)+"%, "+Math.round(hsl[2]*100)+"%)";}var props=["color","background-color","border-left-color","border-right-color","border-top-color","border-bottom-color"];var props2=["color","backgroundColor","borderLeftColor","borderRightColor","borderTopColor","borderBottomColor"];if(typeof getRGBColor(document.documentElement,"background-color")=="string")document.documentElement.style.backgroundColor="white";revl(document.documentElement);function revl(n){var i,x,color,hsl;if(n.nodeType==Node.ELEMENT_NODE){for(i=0;x=n.childNodes[i];++i)revl(x);for(i=0;x=props[i];++i){color=getRGBColor(n,x);if(typeof(color)!="string"){hsl=RGBtoHSL(color);hsl[2]=1-hsl[2];n.style[props2[i]]=hslToCSS(hsl);}}}}})()
```

- **新标签页打开链接**

```javascript
javascript:(function(){var x,i; x=document.links; for(i=0;i<x.length;++i) { x[i].target="_blank"; } })();
```

- **当前标签页打开链接**

```javascript
javascript:(function(){var x,i; x=document.links; for(i=0;i<x.length;++i) { x[i].target="_self"; } })();
```

- **后一标签页打开链接**

```javascript
javascript:(function(){var x,i,r=Math.random(); x=document.links; for(i=0;i<x.length;++i) { x[i].target=r; } })();
```

- **查看选择**

```javascript
javascript:(function(){ var d=open().document; d.title="Selection"; if (window.getSelection) { /*Moz*/ var s = getSelection(); for(i=0; i<s.rangeCount; ++i) { var a, r = s.getRangeAt(i); if (!r.collapsed) { var x = document.createElement("div"); x.appendChild(r.cloneContents()); if (d.importNode) x = d.importNode(x, true); d.body.appendChild(x); } } } else { /*IE*/ d.body.innerHTML = document.selection.createRange().htmlText; } })();
```

- **排序表**

```javascript
javascript:function toArray (c){var a, k;a=new Array;for (k=0; k<c.length; ++k)a[k]=c[k];return a;}function insAtTop(par,child){if(par.childNodes.length) par.insertBefore(child, par.childNodes[0]);else par.appendChild(child);}function countCols(tab){var nCols, i;nCols=0;for(i=0;i<tab.rows.length;++i)if(tab.rows[i].cells.length>nCols)nCols=tab.rows[i].cells.length;return nCols;}function makeHeaderLink(tableNo, colNo, ord){var link;link=document.createElement('a');link.href='javascript:sortTable('+tableNo+','+colNo+','+ord+');';link.appendChild(document.createTextNode((ord>0)?'a':'d'));return link;}function makeHeader(tableNo,nCols){var header, headerCell, i;header=document.createElement('tr');for(i=0;i<nCols;++i){headerCell=document.createElement('td');headerCell.appendChild(makeHeaderLink(tableNo,i,1));headerCell.appendChild(document.createTextNode('/'));headerCell.appendChild(makeHeaderLink(tableNo,i,-1));header.appendChild(headerCell);}return header;}g_tables=toArray(document.getElementsByTagName('table'));if(!g_tables.length) alert("This page doesn't contain any tables.");(function(){var j, thead;for(j=0;j<g_tables.length;++j){thead=g_tables[j].createTHead();insAtTop(thead, makeHeader(j,countCols(g_tables[j])))}}) ();function compareRows(a,b){if(a.sortKey==b.sortKey)return 0;return (a.sortKey < b.sortKey) ? g_order : -g_order;}function sortTable(tableNo, colNo, ord){var table, rows, nR, bs, i, j, temp;g_order=ord;g_colNo=colNo;table=g_tables[tableNo];rows=new Array();nR=0;bs=table.tBodies;for(i=0; i<bs.length; ++i)for(j=0; j<bs[i].rows.length; ++j){rows[nR]=bs[i].rows[j];temp=rows[nR].cells[g_colNo];if(temp) rows[nR].sortKey=temp.innerHTML;else rows[nR].sortKey="";++nR;}rows.sort(compareRows);for (i=0; i < rows.length; ++i)insAtTop(table.tBodies[0], rows[i]);}
```

- **数字编号**

```javascript
javascript:(function(){function has(par,ctag){for(var k=0;k<par.childNodes.length;++k)if(par.childNodes[k].tagName==ctag)return true;} function add(par,ctag,text){var c=document.createElement(ctag); c.appendChild(document.createTextNode(text)); par.insertBefore(c,par.childNodes[0]);} var i,ts=document.getElementsByTagName("TABLE"); for(i=0;i<ts.length;++i) { var n=0,trs=ts[i].rows,j,tr; for(j=0;j<trs.length;++j) {tr=trs[j]; if(has(tr,"TD"))add(tr,"TD",++n); else if(has(tr,"TH"))add(tr,"TH","Row");}}})()
```

- **查看网页最后更新时间**

```javascript
javascript:alert(document.lastModified)
```

- **网页旋转**

```javascript
javascript:window.i=1;setInterval(function(){i++;document.body.style.cssText+="-webkit-transform: rotate(-"+i+"deg);-moz-transform: rotate(-"+i+"deg)";},100);void(0);
```

- **Bing词典**

```javascript
javascript:X=""+(window.getSelection?window.getSelection():document.getSelection?document.getSelection():document.selection.createRange().text);X=X.replace(/\r\n|\r|\n/g," ,");if(!X)X=prompt("%E8%AF%B7%E8%BE%93%E5%85%A5%E5%85%B3%E9%94%AE%E8%AF%8D---Bing%E8%AF%8D%E5%85%B8","");if(X!=null)window.open('http://cn.bing.com/dict/search?q='+escape(X).replace(/ /g,"+"),'_newtab');void 0
```

- **Wiki**

```javascript
javascript:function se (d) {return d.selection ? d.selection.createRange().text : d.getSelection()} s = se (document); for(i=0; i<frames.length && !s; i++)s = se(frames[i].document); if(!s || s=='')s = prompt('%E8%BE%93%E5%85%A5%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91%E6%90%9C%E7%B4%A2%E5%85%B3%E9%94%AE%E5%AD%97',''); open('http://zh.wikipedia.org' + (s ? '/w/index.php?title=Special:Search&search=' + encodeURIComponent(s): '')).focus();
```

- **500px.pro**

```javascript
javascript: void((function() {var element = document.createElement('script');element.id = 'pro5';element.charset = 'utf-8',element.setAttribute('src', 'https://qrusb.oss-cn-shenzhen.aliyuncs.com/plugin.js?' + Date.parse(new Date()));document.body.appendChild(element);})())
```

- **查看内核版本**

```javascript
javascript:alert(navigator.userAgent)
```

- **显示表单密码**

```javascript
javascript:Array.prototype.slice.call(document.querySelectorAll('input[type=password]')).map(function(el){el.setAttribute('type','text')})
```

- **查看密码**

```javascript
javascript:(function(){var s,F,j,f,i; s = ""; F = document.forms; for(j=0; j<F.length; ++j) { f = F[j]; for (i=0; i<f.length; ++i) { if (f[i].type.toLowerCase() == "password") s += f[i].value + "\n"; } } if (s) alert("Passwords in forms on this page:\n\n" + s); else alert("There are no passwords in forms on this page.");})();
```

- **恢复上下文菜单**

```javascript
javascript:(function() { function R(a){ona = "on"+a; if(window.addEventListener) window.addEventListener(a, function (e) { for(var n=e.originalTarget; n; n=n.parentNode) n[ona]=null; }, true); window[ona]=null; document[ona]=null; if(document.body) document.body[ona]=null; } R("contextmenu"); R("click"); R("mousedown"); R("mouseup"); })()
```

- **恢复选择**

```javascript
javascript:(function() { function R(a){ona = "on"+a; if(window.addEventListener) window.addEventListener(a, function (e) { for(var n=e.originalTarget; n; n=n.parentNode) n[ona]=null; }, true); window[ona]=null; document[ona]=null; if(document.body) document.body[ona]=null; } R("click"); R("mousedown"); R("mouseup"); R("selectstart"); })()
```

- **文本转链接**

```javascript
javascript:(function(){var D=document; D.body.normalize(); F(D.body); function F(n){var u,A,M,R,c,x; if(n.nodeType==3){ u=n.data.search(/https?\:\/\/[^\s]*[^.,;'">\s\)\]]/); if(u>=0) { M=n.splitText(u); R=M.splitText(RegExp.lastMatch.length); A=document.createElement("A"); A.href=M.data; A.appendChild(M); R.parentNode.insertBefore(A,R); } }else if(n.tagName!="STYLE" && n.tagName!="SCRIPT" && n.tagName!="A")for(c=0;x=n.childNodes[c];++c)F(x); } })();
```

- **记住密码**

```javascript
javascript:(function(){var ca,cea,cs,df,dfe,i,j,x,y;function n(i,what){return i+" "+what+((i==1)?"":"s")}ca=cea=cs=0;df=document.forms;for(i=0;i<df.length;++i){x=df[i];dfe=x.elements;if(x.onsubmit){x.onsubmit="";++cs;}if(x.attributes["autocomplete"]){x.attributes["autocomplete"].value="on";++ca;}for(j=0;j<dfe.length;++j){y=dfe[j];if(y.attributes["autocomplete"]){y.attributes["autocomplete"].value="on";++cea;}}}alert("Removed autocomplete=off from "+n(ca,"form")+" and from "+n(cea,"form element")+", and removed onsubmit from "+n(cs,"form")+". After you type your password and submit the form, the browser will offer to remember your password.")})();
```





