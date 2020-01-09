# V8Harvest  
The Harvest of V8 regress in 2020.  
  

## **regress-1038573.js (chromium issue)**  
   
**[Issue: Security: crash with charCodeAt and BigInt.asUintN](https://crbug.com/1038573)**  
**[Commit: Fixes lost TypeError for BigInts](https://chromium.googlesource.com/v8/v8/+/9471427)**  
  
Date(Commit): Wed Jan 08 13:41:15 2020  
Components: Blink>JavaScript  
Labels: M-80, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-80  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1986003](https://chromium-review.googlesource.com/c/v8/v8/+/1986003)  
Regress: [mjsunit/regress/regress-1038573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1038573.js)  
```javascript
(function(){
  function f(x) {
    return "abcd".charCodeAt(BigInt.asUintN(64, 10n));
  }

  %PrepareFunctionForOptimization(f);
  try { f(1); } catch(e) {}
  try { f(1); } catch(e) {}
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();


(function(){
  function f(x) {
    return "abcd".charCodeAt(BigInt.asUintN(2, 10n));
  }

  %PrepareFunctionForOptimization(f);
  try { f(1); } catch(e) {}
  try { f(1); } catch(e) {}
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9471427^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=9471427)  
[test/mjsunit/regress/regress-1038573.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1038573.js?cl=9471427)  
  

---   

## **regress-1038588.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1038588)**  
**[Commit: [parser] Fix conflict detection loop early exit](https://chromium.googlesource.com/v8/v8/+/2a6c0f4)**  
  
Date(Commit): Tue Jan 07 13:15:05 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1985991](https://chromium-review.googlesource.com/c/v8/v8/+/1985991)  
Regress: [mjsunit/regress/regress-1038588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1038588.js)  
```javascript
function foo(arg){
  const x = 0;
  eval("var arg, x;");
}
assertThrows(foo, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a6c0f4^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=2a6c0f4)  
[test/mjsunit/regress/regress-1038588.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1038588.js?cl=2a6c0f4)  
  

---   

## **regress-1033966.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1033966)**  
**[Commit: [protectors] Remove invalid DCHECK in protectors.](https://chromium.googlesource.com/v8/v8/+/643ae46)**  
  
Date(Commit): Thu Jan 02 15:49:54 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1982582](https://chromium-review.googlesource.com/c/v8/v8/+/1982582)  
Regress: [mjsunit/regress/regress-1033966.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1033966.js)  
```javascript
var regexp = Realm.global(Realm.createAllowCrossRealmAccess()).RegExp;
regexp.prototype.constructor = 1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/643ae46^!)  
[src/execution/protectors.cc](https://cs.chromium.org/chromium/src/v8/src/execution/protectors.cc?cl=643ae46)  
[test/mjsunit/regress/regress-1033966.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1033966.js?cl=643ae46)  
  

---   
