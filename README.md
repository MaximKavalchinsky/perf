# perf.js
Simple JavaScript library to monitor frame rate (**FPS**), frame time (**MS**) and memory (**MEM**).

Uses `performance.now()` with `Date.now()` fallback on older browsers.

**perf.js** is a self contained library, so monitoring of stats will start by just instantiating it.

`var stats = new Perf();`

#### Installation:

`npm install perf.js`

For haxe users:

`haxelib install perf.js`

#### Basic Stats:

<img alt="basic" src="https://raw.githubusercontent.com/adireddy/perf/master/assets/basic.png" width="84" height="28"/>

#### With Memory (Chrome only):

<img alt="memory" src="https://raw.githubusercontent.com/adireddy/perf/master/assets/memory.png" width="84" height="42"/>

#### With Custom Info (Optional):

Optional custom info can be added if needed, For example the following shows what kind of renderer and pixel ratio pixi.js is using. 

<img alt="info" src="https://raw.githubusercontent.com/adireddy/perf/master/assets/info.png" width="84" height="56"/>

To use, simply call `addInfo(info)` on perf instance.

`stats.addInfo("custom info");`

#### Customisation:

You can customize the following:

```
Perf.MEASUREMENT_INTERVAL = 1000;
Perf.FONT_FAMILY = "Helvetica,Arial";
Perf.FPS_BG_CLR = "#00FF00";
Perf.FPS_WARN_BG_CLR = "#FF8000";
Perf.FPS_PROB_BG_CLR = "#FF0000";
Perf.MS_BG_CLR = "#FFFF00";
Perf.MEM_BG_CLR = "#086A87";
Perf.INFO_BG_CLR = "#00FFFF";
Perf.FPS_TXT_CLR = "#000000";
Perf.MS_TXT_CLR = "#000000";
Perf.MEM_TXT_CLR = "#FFFFFF";
Perf.INFO_TXT_CLR = "#000000";
```

The values assigned above are the default values.

#### Positioning:

By default stats appear on the top right corner. The position can be changed by passing the position to the constructor when instantiating `Perf`.

**Top Right** - default

`var stats = new Perf();`

**Top Left**

`var stats = new Perf(Perf.TOP_LEFT);`

**Bottom Right**

`var stats = new Perf(Perf.BOTTOM_RIGHT);`

**Bottom Left**

`var stats = new Perf(Perf.BOTTOM_LEFT);`

#### Bookmarklet:

```js
javascript:(function(){var script=document.createElement('script');!function(a,b){"use strict";function c(a,b){if(null==b)return null;null==b.__id__&&(b.__id__=f++);var c;return null==a.hx__closures__?a.hx__closures__={}:c=a.hx__closures__[b.__id__],null==c&&(c=function(){return c.method.apply(c.scope,arguments)},c.scope=a,c.method=b,a.hx__closures__[b.__id__]=c),c}var d=b.Perf=function(a,b){null==b&&(b=0),null==a&&(a="TR"),this._perfObj=window.performance,this._memoryObj=window.performance.memory,this._memCheck=null!=this._perfObj&&null!=this._memoryObj&&this._memoryObj.totalJSHeapSize>0,this.currentFps=0,this.currentMs=0,this.currentMem="0",this._pos=a,this._offset=b,this._time=0,this._ticks=0,this._fpsMin=1/0,this._fpsMax=0,null!=this._perfObj&&null!=(e=this._perfObj,c(e,e.now))?this._startTime=this._perfObj.now():this._startTime=(new Date).getTime(),this._prevTime=-d.MEASUREMENT_INTERVAL,this._createFpsDom(),this._createMsDom(),this._memCheck&&this._createMemoryDom(),window.requestAnimationFrame(c(this,this._tick))};d.prototype={_now:function(){return null!=this._perfObj&&null!=(e=this._perfObj,c(e,e.now))?this._perfObj.now():(new Date).getTime()},_tick:function(){var a;a=null!=this._perfObj&&null!=(e=this._perfObj,c(e,e.now))?this._perfObj.now():(new Date).getTime(),this._ticks++,a>this._prevTime+d.MEASUREMENT_INTERVAL&&(this.currentMs=Math.round(a-this._startTime),this.ms.innerHTML="MS: "+this.currentMs,this.currentFps=Math.round(1e3*this._ticks/(a-this._prevTime)),this._fpsMin=Math.min(this._fpsMin,this.currentFps),this._fpsMax=Math.max(this._fpsMax,this.currentFps),this.fps.innerHTML="FPS: "+this.currentFps+" ("+this._fpsMin+"-"+this._fpsMax+")",this.currentFps>=30?this.fps.style.backgroundColor=d.FPS_BG_CLR:this.currentFps>=15?this.fps.style.backgroundColor=d.FPS_WARN_BG_CLR:this.fps.style.backgroundColor=d.FPS_PROB_BG_CLR,this._prevTime=a,this._ticks=0,this._memCheck&&(this.currentMem=this._getFormattedSize(this._memoryObj.usedJSHeapSize,2),this.memory.innerHTML="MEM: "+this.currentMem)),this._startTime=a,window.requestAnimationFrame(c(this,this._tick))},_createDiv:function(a,b){null==b&&(b=0);var c,e=window.document;c=e.createElement("div"),c.id=a,c.className=a,c.style.position="absolute";var f=this._pos;switch(f){case"TL":c.style.left=this._offset+"px",c.style.top=b+"px";break;case"TR":c.style.right=this._offset+"px",c.style.top=b+"px";break;case"BL":c.style.left=this._offset+"px",c.style.bottom=(this._memCheck?48:32)-b+"px";break;case"BR":c.style.right=this._offset+"px",c.style.bottom=(this._memCheck?48:32)-b+"px"}return c.style.width="80px",c.style.height="12px",c.style.lineHeight="12px",c.style.padding="2px",c.style.fontFamily=d.FONT_FAMILY,c.style.fontSize="9px",c.style.fontWeight="bold",c.style.textAlign="center",window.document.body.appendChild(c),c},_createFpsDom:function(){this.fps=this._createDiv("fps"),this.fps.style.backgroundColor=d.FPS_BG_CLR,this.fps.style.zIndex="995",this.fps.style.color=d.FPS_TXT_CLR,this.fps.innerHTML="FPS: 0"},_createMsDom:function(){this.ms=this._createDiv("ms",16),this.ms.style.backgroundColor=d.MS_BG_CLR,this.ms.style.zIndex="996",this.ms.style.color=d.MS_TXT_CLR,this.ms.innerHTML="MS: 0"},_createMemoryDom:function(){this.memory=this._createDiv("memory",32),this.memory.style.backgroundColor=d.MEM_BG_CLR,this.memory.style.color=d.MEM_TXT_CLR,this.memory.style.zIndex="997",this.memory.innerHTML="MEM: 0"},_getFormattedSize:function(a,b){null==b&&(b=0);var c=["Bytes","KB","MB","GB","TB"];if(0==a)return"0";var d=Math.pow(10,b),e=Math.floor(Math.log(a)/Math.log(1024));return Math.round(a*d/Math.pow(1024,e))/d+" "+c[e]},addInfo:function(a){this.info=this._createDiv("info",this._memCheck?48:32),this.info.style.backgroundColor=d.INFO_BG_CLR,this.info.style.color=d.INFO_TXT_CLR,this.info.style.zIndex="998",this.info.innerHTML=a},clearInfo:function(){null!=this.info&&(window.document.body.removeChild(this.info),this.info=null)}};var e,f=0;d.MEASUREMENT_INTERVAL=1e3,d.FONT_FAMILY="Helvetica,Arial",d.FPS_BG_CLR="#00FF00",d.FPS_WARN_BG_CLR="#FF8000",d.FPS_PROB_BG_CLR="#FF0000",d.MS_BG_CLR="#FFFF00",d.MEM_BG_CLR="#086A87",d.INFO_BG_CLR="#00FFFF",d.FPS_TXT_CLR="#000000",d.MS_TXT_CLR="#000000",d.MEM_TXT_CLR="#FFFFFF",d.INFO_TXT_CLR="#000000",d.TOP_LEFT="TL",d.TOP_RIGHT="TR",d.BOTTOM_LEFT="BL",d.BOTTOM_RIGHT="BR"}("undefined"!=typeof console?console:{log:function(){}},"undefined"!=typeof window?window:exports);stats = new Perf(Perf.TOP_RIGHT, 0);document.head.appendChild(script);})();
```

#### Licensing Information

<a rel="license" href="http://opensource.org/licenses/MIT">
<img alt="MIT license" height="40" src="http://upload.wikimedia.org/wikipedia/commons/c/c3/License_icon-mit.svg" /></a>

This content is released under the [MIT](http://opensource.org/licenses/MIT) License.