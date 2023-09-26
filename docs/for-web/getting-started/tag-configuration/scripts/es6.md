# ES6 Check / Abort

This script aborts Optimize if the browser is not ES6 compatible. 

``` javascript
var supportsES6 = (function () {
    try {
        new Function("async () => {}");
        new Function("(a = 0) => a");
        return true;
    }
    catch (err) {
        return false;
    }
}());
if (!supportsES6) {
    WT.optimizeModule.prototype.abort();
    return;
}
```

