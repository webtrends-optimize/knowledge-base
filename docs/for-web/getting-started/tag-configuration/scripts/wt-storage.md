# WT.Storage

## Script for pre-init

``` javascript
/* 
    WT.Storage

    A simple utility that allows users to write to storage, with local/sessionstorage preferred, but falling back to cookies, without having to worry about detection on every use.
*/

WT.Storage = {
    hasLS: (function(){ try { var a = 'a'; localStorage.setItem(a, a); localStorage.removeItem(a); return true; } catch(e) { return false; } })(),
    hasSS: (function(){ try { var a = 'a'; sessionStorage.setItem(a, a); sessionStorage.removeItem(a); return true; } catch(e) { return false; } })(),

    set: function(name, value, expiry){

        if(!expiry){
            if(WT.Storage.hasSS){
                sessionStorage.setItem(name, value);
            } else {
                WT.helpers.cookie.set(name, value);
            }

        } else {
            if(WT.Storage.hasLS){
                localStorage.setItem(name, value);
            } else {
                WT.helpers.cookie.set(name, value, exipry);
            }
        }

    },
    get: function(name){

        return (WT.Storage.hasLS && localStorage.getItem(name) || WT.Storage.hasSS && sessionStorage.getItem(name) || WT.helpers.cookie.get(name))

    }
};
```

## WT.Storage.set 

This method allows you to push data into storage. We make this selection based on whether local/sessionstorage is available, and what input you provide.

If they are available, we avoid cookies. If not, we leverage cookies.

### Examples of WT.Storage.set

- `WT.Storage.set(name, value, 1)` - if you provide a 3rd argument, we will use either localStorage (if available) or a cookie with that expiry.
- `WT.Storage.set(name, value)` - if you do not provide a 3rd argument, we will use either sessionStorage (if available) or a session cookie.

## WT.Storage.get 

This is a simpler method - we will check all of localStorage, sessionStorage and cookies for your name. The first place it is found, we return the value. Whilst taking away control, the abstraction provides simplicity.

### Examples of WT.Storage.get 

`WT.Storage.get(name)`