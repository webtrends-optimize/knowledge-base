# Custom CSS Pagehide

``` javascript
// Page Hide
(function pageHide(){
    // Add things into here for all traffic.
    WT.optimizeModule.prototype.whitelist = [
        {
            URLs: [
                /[\?&]_wt.testWhitelist=true/i
            ],
            css: 'body { opacity: 0.00000001 !important; }'
        },
    ];
    
    WT.optimizeModule.prototype.checkWhitelist_CUSTOM=function(){var e,t,o,n=function(e){var o=e||"";return{add:function(e){e.length&&(o+=e+"\n")},output:function(e){var t;if(o.length){if(e&&(t=document.getElementById(e)),t)return!1;(t=document.createElement("style")).setAttribute("type","text/css"),e&&(t.id=e),t.styleSheet?t.styleSheet.cssText=o:t.appendChild(document.createTextNode(o)),document.getElementsByTagName("head")[0].appendChild(t),o=""}},remove:function(e,t){if(!e)return!1;e=(t=t||window.document).getElementById(e);e&&"style"==e.nodeName.toLowerCase()&&e.parentNode.removeChild(e)}}},i=WT.optimizeModule.prototype.whitelist||[];try{for(var r="",a=!1,d=0,s=i.length;d<s;d++){var u,c=i[d],p=c;c.URLs&&c.css&&(p=c.URLs,u=c.css||""),!0===function(e){if(!e)return!1;!1==e instanceof Array&&(e=[e]);for(var t,o=0;t=e[o];o++)if(window.location.href.match(t))return!0;return!1}(p)&&(u&&(r+=u),WT.optimizeModule.prototype.wtConfigObj.s_pageTimeout=5e3,WT.optimizeModule.prototype.wtConfigObj.s_pageDisplayMode="custom",a=!0)}return""!==(WT.obfHide=r)&&(e=(e=r).match(/[\{\}]+/)?e:e+"{ opacity: 0.00001 !important; }",t=new n(e),o="wto-css-capi-"+Math.floor(1e3*Math.random()),WT.addEventHandler("hide_show",function(e){e.params&&(!1===e.params.display&&t.output(o),!0===e.params.display&&t.remove(o))}),setTimeout(function(){t.remove(o)},5100)),a}catch(e){}};
    WT.optimizeModule.prototype.checkWhitelist_CUSTOM();
})();
```