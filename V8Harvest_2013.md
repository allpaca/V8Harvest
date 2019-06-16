# V8Harvest  
The Harvest of V8 regress in 2013.  
  

## **regress-330046.js (chromium issue)**  
   
**[Issue 330046:
 Render crash @ v8::internal::JSObject::FastPropertyAtPut](https://crbug.com/330046)**  
**[Commit: Fix a race between concurrent recompilation and OSR.](https://chromium.googlesource.com/v8/v8/+/7ac7a7e)**  
  
Date(Commit): Fri Dec 27 09:22:56 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-33"]  
Code Review: [https://codereview.chromium.org/109033003](https://codereview.chromium.org/109033003)  
Regress: [mjsunit/regress/regress-330046.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-330046.js)  
```javascript
var o1 = {a : 10};
var o2 = { };
o2.__proto__ = o1;
var o3 = { };
o3.__proto__ = o2;

function f(n, x, b) {
  var sum = x.a;
  for (var i = 0; i < n; i++) {
    sum = 1.0 / i;
  }
  return sum;
}
%PrepareFunctionForOptimization(f);

f(10, o3);
f(20, o3);
f(30, o3);
%OptimizeFunctionOnNextCall(f, "concurrent");
f(100000, o3);

o2.a = 5;

%OptimizeFunctionOnNextCall(f);
f(10, o3);

%DisassembleFunction(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ac7a7e^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=7ac7a7e)  
[test/mjsunit/regress-330046.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-330046.js?cl=7ac7a7e)  
  

---   

## **regress-3026.js (v8 issue)**  
   
**[Issue 3026:
 Minor spec conformance issue in String.split()](https://crbug.com/v8/3026)**  
**[Commit: Fix small spec violation in String.prototype.split.](https://chromium.googlesource.com/v8/v8/+/cd7d61cf)**  
  
Date(Commit): Mon Dec 23 10:01:22 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/119093002](https://codereview.chromium.org/119093002)  
Regress: [mjsunit/regress/regress-3026.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3026.js)  
```javascript
assertEquals([], "abc".split(undefined, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd7d61cf^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=cd7d61cf)  
[test/mjsunit/regress/regress-3026.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3026.js?cl=cd7d61cf)  
[test/mjsunit/string-split.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/string-split.js?cl=cd7d61cf)  
  

---   

## **regress-270142.js (chromium issue)**  
   
**[Issue 270142:
 Bound functions incorrectly report 'name' property as writable](https://crbug.com/270142)**  
**[Commit: Make a strict function's "name" property non-writable.](https://chromium.googlesource.com/v8/v8/+/b882bdd)**  
  
Date(Commit): Fri Dec 20 12:06:11 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard", "M-34", "MovedFrom-33", "Hotlist-Google"]  
Code Review: [https://codereview.chromium.org/99203006](https://codereview.chromium.org/99203006)  
Regress: [mjsunit/regress/regress-270142.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-270142.js)  
```javascript
function f(x) {
  return x;
}

function g(x) {
  "use strict";
  return x;
}

function checkNameDescriptor(f) {
  var descriptor = Object.getOwnPropertyDescriptor(f, "name");
  assertTrue(descriptor.configurable);
  assertFalse(descriptor.enumerable);
  assertFalse(descriptor.writable);
}

checkNameDescriptor(f);
checkNameDescriptor(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b882bdd^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=b882bdd)  
[test/mjsunit/regress/regress-270142.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-270142.js?cl=b882bdd)  
  

---   

## **regress-crbug-329709.js (chromium issue)**  
   
**[Issue 329709:
 Maps test crashing on GPU bots after V8 roll](https://crbug.com/329709)**  
**[Commit: Fix switch statements with non-Smi integer labels and no type feedback](https://chromium.googlesource.com/v8/v8/+/3c76ecd)**  
  
Date(Commit): Thu Dec 19 14:25:58 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/98643010](https://codereview.chromium.org/98643010)  
Regress: [mjsunit/regress/regress-crbug-329709.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-329709.js)  
```javascript
function boom(x) {
  switch (x) {
    case 1:
      return 'one';
    case 1500000000:
      return 'non-smi int32';
    default:
      return 'default';
  }
};
%PrepareFunctionForOptimization(boom);
assertEquals("one", boom(1));
assertEquals("one", boom(1));
%OptimizeFunctionOnNextCall(boom);
assertEquals("non-smi int32", boom(1500000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c76ecd^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3c76ecd)  
[test/mjsunit/regress/regress-crbug-329709.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-329709.js?cl=3c76ecd)  
  

---   

## **regress-calls-with-migrating-prototypes.js (other issue)**  
   
**[Commit: Fix polymorphic inlined calls with migrating prototypes](https://chromium.googlesource.com/v8/v8/+/48ff79a)**  
  
Date(Commit): Thu Dec 12 14:57:00 2013  
Code Review: [https://codereview.chromium.org/104793003](https://codereview.chromium.org/104793003)  
Regress: [mjsunit/regress/regress-calls-with-migrating-prototypes.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-calls-with-migrating-prototypes.js)  
```javascript
function f() {
  return 1;
}
function C1(f) {
  this.f = f;
}
var o1 = new C1(f);
var o2 = {__proto__: new C1(f)};
function foo(o) {
  return o.f();
};
%PrepareFunctionForOptimization(foo);
foo(o1);
foo(o1);
foo(o2);
foo(o1);
var o3 = new C1(function() {
  return 2;
});
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo(o2));
o2.__proto__.f = function() {
  return 3;
};
assertEquals(3, foo(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/48ff79a^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=48ff79a)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=48ff79a)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=48ff79a)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=48ff79a)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=48ff79a)  
...  
  
  
---   

## **regress-280531.js (chromium issue)**  
   
**[Issue 280531:
 JavaScript - wrong date parsing using Date contructor](https://crbug.com/280531)**  
**[Commit: Initialize Date parse cache with SMI instead of double to workaround sharing mutable heap numbers in snapshot.](https://chromium.googlesource.com/v8/v8/+/cc40109)**  
  
Date(Commit): Wed Dec 11 13:11:44 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Hotlist-DevRel", "M-29"]  
Code Review: [https://chromiumcodereview.appspot.com/112003005](https://chromiumcodereview.appspot.com/112003005)  
Regress: [mjsunit/regress/regress-280531.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-280531.js)  
```javascript
var contextA = Realm.create();
var date1 = Realm.eval(contextA, "new Date('Thu, 29 Aug 2013 00:00:00 UTC')");
new Date('Thu, 29 Aug 2013 00:00:01 UTC');
var date2 = Realm.eval(contextA, "new Date('Thu, 29 Aug 2013 00:00:00 UTC')");
assertEquals(date1, date2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc40109^!)  
[src/date.js](https://cs.chromium.org/chromium/src/v8/src/date.js?cl=cc40109)  
[test/mjsunit/regress/regress-280531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-280531.js?cl=cc40109)  
  

---   

## **regress-param-local-type.js (other issue)**  
   
**[Commit: Fix off-by-one error in AstTyper.](https://chromium.googlesource.com/v8/v8/+/5bc64b9)**  
  
Date(Commit): Wed Dec 11 11:34:09 2013  
Code Review: [https://codereview.chromium.org/105943007](https://codereview.chromium.org/105943007)  
Regress: [mjsunit/regress/regress-param-local-type.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-param-local-type.js)  
```javascript
function f(a) {  // First parameter is tagged.
  var s = '';    // First local has string type.
  var n = 0;
  var i = 1;
  n = i + a;
}

%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);
assertOptimized(f);


function g() {  // 0th parameter (receiver) is tagged.
  var s = '';   // First local has string type.
  var n = 0;
  var i = 1;
  n = i + this;
}

%PrepareFunctionForOptimization(g);
g.call(1);
g.call(1);
%OptimizeFunctionOnNextCall(g);
g.call(1);
assertOptimized(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bc64b9^!)  
[src/typing.h](https://cs.chromium.org/chromium/src/v8/src/typing.h?cl=5bc64b9)  
[test/mjsunit/regress/regress-param-local-type.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-param-local-type.js?cl=5bc64b9)  
  
  
---   

## **regress-3039.js (v8 issue)**  
   
**[Issue 3039:
 ARM: Division in simulator breaks for certain inputs](https://crbug.com/v8/3039)**  
**[Commit: Avoid FP exceptions when doing integer division.](https://chromium.googlesource.com/v8/v8/+/e1db6d8)**  
  
Date(Commit): Mon Dec 09 10:15:19 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/104003004](https://codereview.chromium.org/104003004)  
Regress: [mjsunit/regress/regress-3039.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3039.js)  
```javascript
function do_div(x, y) {
  return x / y | 0;
}

;
%PrepareFunctionForOptimization(do_div);
assertEquals(17, do_div(51, 3));
assertEquals(13, do_div(65, 5));
%OptimizeFunctionOnNextCall(do_div);
assertEquals(11, do_div(77, 7));

assertEquals(-2147483648, do_div(-2147483648, -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1db6d8^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=e1db6d8)  
[test/mjsunit/div-mul-minus-one.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/div-mul-minus-one.js?cl=e1db6d8)  
[test/mjsunit/regress/regress-3039.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3039.js?cl=e1db6d8)  
  

---   

## **regress-context-osr.js (other issue)**  
   
**[Commit: Fix loop side-effects of deoptimizing loops with a nested live OSR loop.](https://chromium.googlesource.com/v8/v8/+/8a4df12)**  
  
Date(Commit): Thu Dec 05 18:31:06 2013  
Code Review: [https://chromiumcodereview.appspot.com/106723002](https://chromiumcodereview.appspot.com/106723002)  
Regress: [mjsunit/regress/regress-context-osr.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-context-osr.js)  
```javascript
"use strict";
function f() {
  try { } catch (e) { }
}

for (this.x = 0; this.x < 1; ++this.x) {
  for (this.y = 0; this.y < 1; ++this.y) {
    for (this.ll = 0; this.ll < 70670; ++this.ll) {
      f();
    }
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a4df12^!)  
[src/hydrogen-gvn.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-gvn.cc?cl=8a4df12)  
[test/mjsunit/regress/regress-context-osr.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-context-osr.js?cl=8a4df12)  
  
  
---   

## **regress-crbug-325225.js (chromium issue)**  
   
**[Issue 325225:
 Crash on keyed load invocation](https://crbug.com/325225)**  
**[Commit: Check whether the receiver to a keyed-call is actually a heapobject.](https://chromium.googlesource.com/v8/v8/+/d4eaae3)**  
  
Date(Commit): Tue Dec 03 17:59:31 2013  
Components/Type: None/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "M-33", "Security_Severity-Medium", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromiumcodereview.appspot.com/101863004](https://chromiumcodereview.appspot.com/101863004)  
Regress: [mjsunit/regress/regress-crbug-325225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-325225.js)  
```javascript
function f1(a) {
  a[0](0);
}

function do1() {
  f1([f1]);
}

assertThrows(do1, TypeError);

function f2(a) {
  a[0](true);
}

function do2() {
  f2([function(a) { return f2("undefined", typeof f2(42, 0)); }]);
}

assertThrows(do2, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d4eaae3^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=d4eaae3)  
[test/mjsunit/regress/regress-crbug-325225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-325225.js?cl=d4eaae3)  
  

---   

## **regress-3032.js (v8 issue)**  
   
**[Issue 3032:
 Failed assertion with OSR in BuildBinaryOperation for Token::MOD](https://crbug.com/v8/3032)**  
**[Commit: Fix invalid assertion with OSR in BuildBinaryOperation.](https://chromium.googlesource.com/v8/v8/+/aa83f29)**  
  
Date(Commit): Mon Dec 02 13:12:07 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/98623004](https://codereview.chromium.org/98623004)  
Regress: [mjsunit/regress/regress-3032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3032.js)  
```javascript
function f() {
  for (var i = 0; i < 10; i++) { if (i == 5) %OptimizeOsr(); }
  var xl = 4096;
  var z = i % xl;
}
%PrepareFunctionForOptimization(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa83f29^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=aa83f29)  
[test/mjsunit/regress/regress-3032.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3032.js?cl=aa83f29)  
  

---   

## **regress-3029.js (v8 issue)**  
   
**[Issue 3029:
 Dematerialized objects not materialized in OptimizedFrame::Summarize](https://crbug.com/v8/3029)**  
**[Commit: Handle captured objects in OptimizedFrame::Summarize.](https://chromium.googlesource.com/v8/v8/+/db915fe)**  
  
Date(Commit): Mon Dec 02 12:11:02 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/96773002](https://codereview.chromium.org/96773002)  
Regress: [mjsunit/regress/regress-3029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3029.js)  
```javascript
function c(x) {
  undefined.boom();
}

function f() {
  return new c();
}

function g() {
  f();
};
%PrepareFunctionForOptimization(g);
assertThrows("g()", TypeError);
assertThrows("g()", TypeError);
%OptimizeFunctionOnNextCall(g);
assertThrows("g()", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db915fe^!)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=db915fe)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=db915fe)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=db915fe)  
[test/cctest/test-debug.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-debug.cc?cl=db915fe)  
[test/mjsunit/regress/regress-3029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3029.js?cl=db915fe)  
  

---   

## **regress-299979.js (chromium issue)**  
   
**[Issue 299979:
 Should not allow unshift on frozen array](https://crbug.com/299979)**  
**[Commit: Array builtins need to be prevented from changing frozen objects, and changing structure on sealed objects.](https://chromium.googlesource.com/v8/v8/+/5ba1304)**  
  
Date(Commit): Fri Nov 29 15:22:16 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/80623002](https://codereview.chromium.org/80623002)  
Regress: [mjsunit/regress/regress-299979.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-299979.js)  
```javascript
(function(){
  "use strict";
  var list = Object.freeze([1, 2, 3]);
  assertThrows(function() { list.unshift(4); }, TypeError);
  assertThrows(function() { list.shift(); }, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ba1304^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=5ba1304)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=5ba1304)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=5ba1304)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=5ba1304)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=5ba1304)  
...  
  

---   

## **regress-324028.js (chromium issue)**  
   
**[Issue 324028:
 CHECK failure in CHECK(is_valid) failed: ../../v8/src/v8conversions.h(89)](https://crbug.com/324028)**  
**[Commit: Ensure that length is Smi in TypedArrayFromArrayLike constructor.](https://chromium.googlesource.com/v8/v8/+/7372596)**  
  
Date(Commit): Thu Nov 28 15:22:36 2013  
Components/Type: None/Bug  
Labels: ["M-34", "MovedFrom-33", "Clusterfuzz", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/94473002](https://codereview.chromium.org/94473002)  
Regress: [mjsunit/regress/regress-324028.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-324028.js)  
```javascript
var badObj = { length : 1e40 };

assertThrows(function() { new Uint8Array(badObj); }, RangeError);
assertThrows(function() { new Uint8ClampedArray(badObj); }, RangeError);
assertThrows(function() { new Int8Array(badObj); }, RangeError);
assertThrows(function() { new Uint16Array(badObj); }, RangeError);
assertThrows(function() { new Int16Array(badObj); }, RangeError);
assertThrows(function() { new Uint32Array(badObj); }, RangeError);
assertThrows(function() { new Int32Array(badObj); }, RangeError);
assertThrows(function() { new Float32Array(badObj); }, RangeError);
assertThrows(function() { new Float64Array(badObj); }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7372596^!)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=7372596)  
[test/mjsunit/regress/regress-324028.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-324028.js?cl=7372596)  
  

---   

## **regress-3027.js (v8 issue)**  
   
**[Issue 3027:
 The n-arguments Array constructor might exceed allocation limit](https://crbug.com/v8/3027)**  
**[Commit: Fix missing bounds check in n-arguments Array constructor.](https://chromium.googlesource.com/v8/v8/+/d53e387)**  
  
Date(Commit): Thu Nov 28 09:29:57 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/92103003](https://codereview.chromium.org/92103003)  
Regress: [mjsunit/regress/regress-3027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3027.js)  
```javascript
function boom() {
  var args = [];
  for (var i = 0; i < 125000; i++) {
    args.push(i);
  }
  return Array.apply(Array, args);
}

var array = boom();

assertEquals(125000, array.length);
assertEquals(124999, array[124999]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d53e387^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=d53e387)  
[test/mjsunit/regress/regress-3027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3027.js?cl=d53e387)  
  

---   

## **regress-3025.js (v8 issue)**  
   
**[Issue 3025:
 number.toString(5) doesn't generate the correct result.](https://crbug.com/v8/3025)**  
**[Commit: Increase precision for base conversion for large integers.](https://chromium.googlesource.com/v8/v8/+/ab96631)**  
  
Date(Commit): Tue Nov 26 15:48:13 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/88583002](https://codereview.chromium.org/88583002)  
Regress: [mjsunit/regress/regress-3025.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3025.js)  
```javascript
var n = 0x8000000000000800;
assertEquals(n, 9223372036854778000);
var s = n.toString(5);
var v = parseInt(s, 5);
assertEquals(n, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ab96631^!)  
[src/conversions.cc](https://cs.chromium.org/chromium/src/v8/src/conversions.cc?cl=ab96631)  
[test/mjsunit/regress/regress-3025.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3025.js?cl=ab96631)  
  

---   

## **regress-clobbered-fp-regs.js (other issue)**  
   
**[Commit: Restore saved caller FP registers on stub failure](https://chromium.googlesource.com/v8/v8/+/21fb140)**  
  
Date(Commit): Fri Nov 22 10:21:47 2013  
Code Review: [https://chromiumcodereview.appspot.com/78283002](https://chromiumcodereview.appspot.com/78283002)  
Regress: [mjsunit/regress/regress-clobbered-fp-regs.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-clobbered-fp-regs.js)  
```javascript
function store(a, x, y) {
  var f1 = 0.1 * y;
  var f2 = 0.2 * y;
  var f3 = 0.3 * y;
  var f4 = 0.4 * y;
  var f5 = 0.5 * y;
  var f6 = 0.6 * y;
  var f7 = 0.7 * y;
  var f8 = 0.8 * y;
  a[0] = x;
  var sum = f1 + f2 + f3 + f4 + f5 + f6 + f7 + f8;
  assertEquals(1, y);
  var expected = 3.6;
  if (Math.abs(expected - sum) > 0.01) {
    assertEquals(expected, sum);
  }
}

;
%PrepareFunctionForOptimization(store);
store([1], 1, 1);
store([1], 1.1, 1);
store([1], 1.1, 1);
%OptimizeFunctionOnNextCall(store);
store([1], 1, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21fb140^!)  
[src/arguments.cc](https://cs.chromium.org/chromium/src/v8/src/arguments.cc?cl=21fb140)  
[src/arguments.h](https://cs.chromium.org/chromium/src/v8/src/arguments.h?cl=21fb140)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=21fb140)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=21fb140)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=21fb140)  
...  
  
  
---   

## **regress-3010.js (v8 issue)**  
   
**[Issue 3010:
 Large object literal definitions get mangled.](https://crbug.com/v8/3010)**  
**[Commit: Match max property descriptor length to corresponding bit fields](https://chromium.googlesource.com/v8/v8/+/f27f2fa)**  
  
Date(Commit): Mon Nov 18 11:44:06 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/72333004](https://codereview.chromium.org/72333004)  
Regress: [mjsunit/regress/regress-3010.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3010.js)  
```javascript
(function() {
  function testOneSize(current_size) {
    var eval_string = 'obj = {';
    for (var current = 0; current <= current_size; ++current) {
      eval_string += 'k' + current + ':' + current + ','
    }
    eval_string += '};';
    eval(eval_string);
    for (var i = 0; i <= current_size; i++) {
      assertEquals(i, obj['k'+i]);
    }
    var current_number = 0;
    for (var x in obj) {
      assertEquals(current_number, obj[x]);
      current_number++;
    }
  }

  testOneSize(127);
  testOneSize(128);
  testOneSize(129);

  testOneSize(255);
  testOneSize(256);
  testOneSize(257);

  testOneSize(511);
  testOneSize(512);
  testOneSize(513);

  testOneSize(1023);
  testOneSize(1024);
  testOneSize(1025);

  testOneSize(2047);
  testOneSize(2048);
  testOneSize(2049);
}())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f27f2fa^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=f27f2fa)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=f27f2fa)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=f27f2fa)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=f27f2fa)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f27f2fa)  
...  
  

---   

## **regress-crbug-319860.js (chromium issue)**  
   
**[Issue 319860:
 OOB read in V8](https://crbug.com/319860)**  
**[Commit: Limit size of dehoistable array indices](https://chromium.googlesource.com/v8/v8/+/c9b41c6)**  
  
Date(Commit): Fri Nov 15 17:24:10 2013  
Components/Type: None/Bug-Security  
Labels: ["Release-1-M31", "Stability-ThreadSanitizer", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "CVE-2013-6640", "M-31", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/74113002](https://codereview.chromium.org/74113002)  
Regress: [mjsunit/regress/regress-crbug-319860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-319860.js)  
```javascript
function read(a, index) {
  var offset = 0x2000000;
  var result;
  for (var i = 0; i < 1; i++) {
    result = a[index + offset];
  }
  return result;
}

%PrepareFunctionForOptimization(read);
var a = new Int8Array(0x2000001);
read(a, 0);
read(a, 0);
%OptimizeFunctionOnNextCall(read);

for (var i = 0; i > -1000000; --i) {
  read(a, i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9b41c6^!)  
[src/elements-kind.cc](https://cs.chromium.org/chromium/src/v8/src/elements-kind.cc?cl=c9b41c6)  
[src/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/elements-kind.h?cl=c9b41c6)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=c9b41c6)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=c9b41c6)  
[src/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/lithium.cc?cl=c9b41c6)  
...  
  

---   

## **regress-crbug-319835.js (chromium issue)**  
   
**[Issue 319835:
 OOB write in V8 (only 64bit)](https://crbug.com/319835)**  
**[Commit: Limit size of dehoistable array indices](https://chromium.googlesource.com/v8/v8/+/c9b41c6)**  
  
Date(Commit): Fri Nov 15 17:24:10 2013  
Components/Type: None/Bug-Security  
Labels: ["Release-1-M31", "Stability-ThreadSanitizer", "Merge-Merged", "Security_Impact-Stable", "Arch-x86_64", "CVE-2013-6639", "M-31", "Security_Severity-High", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/74113002](https://codereview.chromium.org/74113002)  
Regress: [mjsunit/regress/regress-crbug-319835.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-319835.js)  
```javascript
try {
} catch (e) {
}  // No need to optimize the top level.

var size = 0x20000;
var a = new Float64Array(size);
var training = new Float64Array(10);
function store(a, index) {
  var offset = 0x20000000;
  for (var i = 0; i < 1; i++) {
    a[index + offset] = 0xcc;
  }
};
%PrepareFunctionForOptimization(store);
store(training, -0x20000000);
store(training, -0x20000000 + 1);
store(training, -0x20000000);
store(training, -0x20000000 + 1);
%OptimizeFunctionOnNextCall(store);

for (var i = -0x20000000; i < -0x20000000 + size; i++) {
  store(a, i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9b41c6^!)  
[src/elements-kind.cc](https://cs.chromium.org/chromium/src/v8/src/elements-kind.cc?cl=c9b41c6)  
[src/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/elements-kind.h?cl=c9b41c6)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=c9b41c6)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=c9b41c6)  
[src/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/lithium.cc?cl=c9b41c6)  
...  
  

---   

## **regress-319722-ArrayBuffer.js (chromium issue)**  
   
**[Issue 319722:
 Heap-buffer-overflow in v8::internal::ExternalByteArray::SetValue](https://crbug.com/319722)**  
**[Commit: Limit the size for typed arrays to MaxSmi.](https://chromium.googlesource.com/v8/v8/+/09ca131)**  
  
Date(Commit): Fri Nov 15 16:09:56 2013  
Components/Type: None/Bug-Security  
Labels: ["M-32", "Release-1-M31", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "M-31", "CVE-2013-6638", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/73943004](https://codereview.chromium.org/73943004)  
Regress: [mjsunit/regress/regress-319722-ArrayBuffer.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-319722-ArrayBuffer.js), [mjsunit/regress/regress-319722-TypedArrays.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-319722-TypedArrays.js)  
```javascript
var maxSize = %MaxSmi() + 1;
var ab;

for (k = 8; k >= 1 && ab == null; k = k/2) {
  try {
    ab = new ArrayBuffer(maxSize * k);
  } catch (e) {
    ab = null;
  }
}

assertTrue(ab != null);

function TestArray(constr) {
  assertThrows(function() {
    new constr(ab, 0, maxSize);
  }, RangeError);
}

TestArray(Uint8Array);
TestArray(Int8Array);
TestArray(Uint16Array);
TestArray(Int16Array);
TestArray(Uint32Array);
TestArray(Int32Array);
TestArray(Float32Array);
TestArray(Float64Array);
TestArray(Uint8ClampedArray);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09ca131^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=09ca131)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=09ca131)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=09ca131)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=09ca131)  
[test/mjsunit/regress/regress-319722-ArrayBuffer.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-319722-ArrayBuffer.js?cl=09ca131)  
...  
  

---   

## **regress-crbug-318671.js (chromium issue)**  
   
**[Issue 318671:
 Weird illegal access exception](https://crbug.com/318671)**  
**[Commit: Fix missing type feedback check for Generic*String addition.](https://chromium.googlesource.com/v8/v8/+/2ee5aa9)**  
  
Date(Commit): Fri Nov 15 09:13:36 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/67473007](https://codereview.chromium.org/67473007)  
Regress: [mjsunit/regress/regress-crbug-318671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-318671.js)  
```javascript
function add(x, y) {
  return x + y;
};
%PrepareFunctionForOptimization(add);
print(add({ a: 1 }, "a"));
print(add({ b: 1 }, "b"));
print(add({ c: 1 }, "c"));

%OptimizeFunctionOnNextCall(add);

print(add("a", 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ee5aa9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2ee5aa9)  
[test/mjsunit/regress/regress-crbug-318671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-318671.js?cl=2ee5aa9)  
  

---   

## **regress-2987.js (v8 issue)**  
   
**[Issue 2987:
 branches/3.22: wrong behavior for Dart-compiled DeltaBlue variant](https://crbug.com/v8/2987)**  
**[Commit: Make HCapturedObjects non-deletable for DCE.](https://chromium.googlesource.com/v8/v8/+/59536de)**  
  
Date(Commit): Thu Nov 07 16:07:19 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/64433002](https://codereview.chromium.org/64433002)  
Regress: [mjsunit/regress/regress-2987.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2987.js)  
```javascript
function constructor() {
  this.x = 0;
}

var deopt = {deopt: false};
function boogeyman(mode, value) {
  var object = new constructor();
  if (mode) {
    object.x = 1;
  } else {
    object.x = 2;
  }
  deopt.deopt;
  assertEquals(value, object.x);
};
%PrepareFunctionForOptimization(boogeyman);
boogeyman(true, 1);
boogeyman(true, 1);
boogeyman(false, 2);
boogeyman(false, 2);
%OptimizeFunctionOnNextCall(boogeyman);
boogeyman(false, 2);
delete deopt.deopt;
boogeyman(false, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/59536de^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=59536de)  
[test/mjsunit/regress/regress-2987.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2987.js?cl=59536de)  
  

---   

## **regress-2984.js (v8 issue)**  
   
**[Issue 2984:
 "\xff".toUpperCase() gives incorrect result.](https://crbug.com/v8/2984)**  
**[Commit: Fix y-umlaut to uppercase.](https://chromium.googlesource.com/v8/v8/+/eb550c6)**  
  
Date(Commit): Thu Nov 07 09:08:34 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/59853006](https://codereview.chromium.org/59853006)  
Regress: [mjsunit/regress/regress-2984.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2984.js)  
```javascript
assertEquals("\u0178", "\xff".toUpperCase());
assertEquals("abcdefghijklmn\xffopq",
             ("ABCDEFGHIJKL" + "MN\u0178OPQ").toLowerCase());
assertEquals("\xff", "\u0178".toLowerCase());
assertEquals("ABCDEFGHIJKLMN\u0178OPQ",
             ("abcdefghijk" + "lmn\xffopq").toUpperCase());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eb550c6^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=eb550c6)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=eb550c6)  
[src/unicode.h](https://cs.chromium.org/chromium/src/v8/src/unicode.h?cl=eb550c6)  
[test/mjsunit/regress/regress-2984.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2984.js?cl=eb550c6)  
  

---   

## **regress-crbug-306220.js (chromium issue)**  
   
**[Issue 306220:
 Error.prototype.toString calls message getter with wrong this object](https://crbug.com/306220)**  
**[Commit: Correctly load message from an Error object.](https://chromium.googlesource.com/v8/v8/+/a5ed9a7)**  
  
Date(Commit): Tue Nov 05 13:04:51 2013  
Components/Type: None/Bug  
Labels: ["Via-Wizard", "Needs-Feedback"]  
Code Review: [https://codereview.chromium.org/46593010](https://codereview.chromium.org/46593010)  
Regress: [mjsunit/regress/regress-crbug-306220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-306220.js)  
```javascript
var CustomError = function(x) { this.x = x; };
CustomError.prototype = new Error();
CustomError.prototype.x = "prototype";

Object.defineProperties(CustomError.prototype, {
   'message': {
      'get': function() { return this.x; }
   }
});

assertEquals("Error: instance", String(new CustomError("instance")));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5ed9a7^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=a5ed9a7)  
[test/mjsunit/regress/regress-crbug-306220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-306220.js?cl=a5ed9a7)  
  

---   

## **regress-2980.js (v8 issue)**  
   
**[Issue 2980:
 Regression: Deleting the last key in an object may make the object disappear.](https://crbug.com/v8/2980)**  
**[Commit: Add missing negative dictionary lookup to NonexistentHandlerFrontend](https://chromium.googlesource.com/v8/v8/+/2ebfd6e)**  
  
Date(Commit): Mon Nov 04 14:14:09 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/57433003](https://codereview.chromium.org/57433003)  
Regress: [mjsunit/regress/regress-2980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2980.js)  
```javascript
function test(expected, holder) {
  assertEquals(expected, holder.property);
}

var holder = {}
holder.__proto__ = null;
holder.property = "foo";
delete holder.property;
test(undefined, holder);
test(undefined, holder);
test(undefined, holder);
holder.property = "bar";
test("bar", holder);
test("bar", holder);


function test2(expected, holder) {
  assertEquals(expected, holder.prop2);
}

var holder2 = {}
holder2.prop2 = "foo";
holder2.__proto__ = null;
function Receiver() {}
Receiver.prototype = holder2;

var rec2 = new Receiver();
delete holder2.prop2;

test2(undefined, rec2);
test2(undefined, rec2);
test2(undefined, rec2);
holder2.prop2 = "bar";
test2("bar", rec2);
test2("bar", rec2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ebfd6e^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=2ebfd6e)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=2ebfd6e)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=2ebfd6e)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=2ebfd6e)  
[src/stub-cache.h](https://cs.chromium.org/chromium/src/v8/src/stub-cache.h?cl=2ebfd6e)  
...  
  

---   

## **regress-crbug-309623.js (chromium issue)**  
   
**[Issue 309623:
 Wrong WebGL display](https://crbug.com/309623)**  
**[Commit: Fix uint32-to-smi conversion in Lithium](https://chromium.googlesource.com/v8/v8/+/316271f)**  
  
Date(Commit): Thu Oct 31 10:18:51 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-30", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/54393002](https://codereview.chromium.org/54393002)  
Regress: [mjsunit/regress/regress-crbug-309623.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-309623.js)  
```javascript
var u = new Uint32Array(2);
u[0] = 1;
u[1] = 0xEE6B2800;

var a = [0, 1, 2];
a[0] = 0;  // Kill the COW.
assertTrue(%HasSmiElements(a));

function foo(i) {
  a[0] = u[i];
  return a[0];
};
%PrepareFunctionForOptimization(foo);
assertEquals(u[0], foo(0));
assertEquals(u[0], foo(0));
%OptimizeFunctionOnNextCall(foo);
assertEquals(u[1], foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/316271f^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=316271f)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=316271f)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=316271f)  
[src/hydrogen-uint32-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-uint32-analysis.cc?cl=316271f)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=316271f)  
...  
  

---   

## **regress-add-minus-zero.js (other issue)**  
   
**[Commit: Do not remove HAdd with zero if the other operand is a double.](https://chromium.googlesource.com/v8/v8/+/3f1a833)**  
  
Date(Commit): Wed Oct 30 10:22:52 2013  
Code Review: [https://codereview.chromium.org/52173003](https://codereview.chromium.org/52173003)  
Regress: [mjsunit/regress/regress-add-minus-zero.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-add-minus-zero.js)  
```javascript
var o = { a: 0 };

function f(x) {
  return -o.a + 0;
};
%PrepareFunctionForOptimization(f);
;
assertEquals('Infinity', String(1 / f()));
assertEquals('Infinity', String(1 / f()));
%OptimizeFunctionOnNextCall(f);
assertEquals('Infinity', String(1 / f()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f1a833^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=3f1a833)  
[test/mjsunit/regress/regress-add-minus-zero.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-add-minus-zero.js?cl=3f1a833)  
  
  
---   

## **regress-compare-constant-doubles.js (other issue)**  
   
**[Commit: ia32: Fix comparisons of two constant double operands when exactly one of them is in new space.](https://chromium.googlesource.com/v8/v8/+/9e88c23)**  
  
Date(Commit): Tue Oct 29 14:34:07 2013  
Code Review: [https://codereview.chromium.org/46883008](https://codereview.chromium.org/46883008)  
Regress: [mjsunit/regress/regress-compare-constant-doubles.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-compare-constant-doubles.js)  
```javascript
var left = 1.5;
var right;

var keepalive;

function foo() {
  // Fill XMM registers with cruft.
  var a1 = Math.sin(1) + 10;
  var a2 = a1 + 1;
  var a3 = a2 + 1;
  var a4 = a3 + 1;
  var a5 = a4 + 1;
  var a6 = a5 + 1;
  keepalive = [a1, a2, a3, a4, a5, a6];

  // Actual test.
  if (left < right) return "ok";
  return "bad";
}

function prepare(base) {
  right = 0.5 * base;
}

prepare(21);
assertEquals("ok", foo());
assertEquals("ok", foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals("ok", foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e88c23^!)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=9e88c23)  
[test/mjsunit/regress/regress-compare-constant-doubles.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-compare-constant-doubles.js?cl=9e88c23)  
  
  
---   

## **regress-array-pop-nonconfigurable.js (other issue)**  
   
**[Commit: Make Array.prototype.pop throw if the last element is not configurable.](https://chromium.googlesource.com/v8/v8/+/e25920d)**  
  
Date(Commit): Wed Oct 23 16:19:24 2013  
Code Review: [https://codereview.chromium.org/29513004](https://codereview.chromium.org/29513004)  
Regress: [mjsunit/regress/regress-array-pop-nonconfigurable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-array-pop-nonconfigurable.js)  
```javascript
var a = [];
Object.defineProperty(a, 0, {});
assertThrows(function() { a.pop(); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e25920d^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=e25920d)  
[test/mjsunit/regress/regress-array-pop-nonconfigurable.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-array-pop-nonconfigurable.js?cl=e25920d)  
[tools/presubmit.py](https://cs.chromium.org/chromium/src/v8/tools/presubmit.py?cl=e25920d)  
  
  
---   

## **regress-crbug-305309.js (chromium issue)**  
   
**[Issue 305309:
 Property lookups sometimes return the wrong property's value](https://crbug.com/305309)**  
**[Commit: Fix HObjectAccess for loads from migrating prototypes](https://chromium.googlesource.com/v8/v8/+/8259439)**  
  
Date(Commit): Wed Oct 23 15:15:15 2013  
Components/Type: None/Bug-Regression  
Labels: ["M-30", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/35173005](https://codereview.chromium.org/35173005)  
Regress: [mjsunit/regress/regress-crbug-305309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-305309.js)  
```javascript
function BadProto() {
  this.constant_function = function() {};
  this.one = 1;
  this.two = 2;
}
var b1 = new BadProto();
var b2 = new BadProto();

function Ctor() {}
Ctor.prototype = b1;
var a = new Ctor();

function Two(x) {
  return x.two;
};
%PrepareFunctionForOptimization(Two);
assertEquals(2, Two(a));
assertEquals(2, Two(a));
b2.constant_function = "no longer constant!";
%OptimizeFunctionOnNextCall(Two);
assertEquals(2, Two(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8259439^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=8259439)  
[test/mjsunit/regress/regress-crbug-305309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-305309.js?cl=8259439)  
  

---   

## **regress-crbug-306851.js (chromium issue)**  
   
**[Issue 306851:
 Object.defineProperty with postfix increment or decrement operator stalls after 132 iterations.](https://crbug.com/306851)**  
**[Commit: Add regression test for optimized count operation.](https://chromium.googlesource.com/v8/v8/+/0a2b4ec)**  
  
Date(Commit): Thu Oct 17 12:48:28 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/27702002](https://codereview.chromium.org/27702002)  
Regress: [mjsunit/regress/regress-crbug-306851.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-306851.js)  
```javascript
function Counter() {
  this.value = 0;
};

Object.defineProperty(Counter.prototype, 'count', {
  get: function() {
    return this.value;
  },
  set: function(value) {
    this.value = value;
  }
});

var obj = new Counter();

function bummer() {
  obj.count++;
  return obj.count;
};
%PrepareFunctionForOptimization(bummer);
assertEquals(1, bummer());
assertEquals(2, bummer());
assertEquals(3, bummer());
%OptimizeFunctionOnNextCall(bummer);
assertEquals(4, bummer());
assertEquals(5, bummer());
assertEquals(6, bummer());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a2b4ec^!)  
[test/mjsunit/regress/regress-crbug-306851.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-306851.js?cl=0a2b4ec)  
  

---   

## **regress-2931.js (v8 issue)**  
   
**[Issue 2931:
 `new TypedArray(n)` not ArrayBuffer tamper proof](https://crbug.com/v8/2931)**  
**[Commit: Do not look up ArrayBuffer on global object in typed array constructor.](https://chromium.googlesource.com/v8/v8/+/5ccd697)**  
  
Date(Commit): Tue Oct 15 11:27:12 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/27238009](https://codereview.chromium.org/27238009)  
Regress: [mjsunit/regress/regress-2931.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2931.js)  
```javascript
this.ArrayBuffer = function() { throw Error('BAM'); };
var u8 = new Uint8Array(100);
assertSame(100, u8.byteLength);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ccd697^!)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=5ccd697)  
[test/mjsunit/regress/regress-2931.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2931.js?cl=5ccd697)  
  

---   

## **regress-parse-use-strict.js (other issue)**  
   
**[Commit: Fix pre-parsing of 'use strict' directive after string literals.](https://chromium.googlesource.com/v8/v8/+/f878c1c)**  
  
Date(Commit): Fri Oct 11 14:03:54 2013  
Code Review: [https://codereview.chromium.org/27025002](https://codereview.chromium.org/27025002)  
Regress: [mjsunit/regress/regress-parse-use-strict.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-parse-use-strict.js)  
```javascript
var filler = "/*" + new Array(1024).join('x') + "*/";

var strict = '"use strict"; with({}) {}';

assertThrows('function f() { "use sanity";' + strict + '}');
assertThrows('function f() { "use sanity";' + strict + filler + '}');

eval('function f() { function g() {}' + strict + '}');
eval('function f() { function g() {}' + strict + filler + '}');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f878c1c^!)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=f878c1c)  
[test/mjsunit/regress/regress-parse-use-strict.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-parse-use-strict.js?cl=f878c1c)  
  
  
---   

## **regress-polymorphic-load.js (other issue)**  
   
**[Commit: Only fold polymorphic into monomorphic load if all load from either receiver or same prototype.](https://chromium.googlesource.com/v8/v8/+/7e0ea6a)**  
  
Date(Commit): Wed Oct 02 13:24:08 2013  
Code Review: [https://chromiumcodereview.appspot.com/25718002](https://chromiumcodereview.appspot.com/25718002)  
Regress: [mjsunit/regress/regress-polymorphic-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-polymorphic-load.js)  
```javascript
function f(o) {
  return o.x;
};
%PrepareFunctionForOptimization(f);
var o1 = {x: 1};
var o2 = {__proto__: {x: 2}};

f(o2);
f(o2);
f(o2);
f(o1);
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f(o1));
assertEquals(2, f(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e0ea6a^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=7e0ea6a)  
[test/mjsunit/regress/regress-polymorphic-load.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-polymorphic-load.js?cl=7e0ea6a)  
  
  
---   

## **regress-binop.js (other issue)**  
   
**[Commit: Fix Environment size mismatch in r6849.](https://chromium.googlesource.com/v8/v8/+/6fc2875)**  
  
Date(Commit): Fri Sep 20 08:34:23 2013  
Code Review: [https://codereview.chromium.org/23983043](https://codereview.chromium.org/23983043)  
Regress: [mjsunit/regress/regress-binop.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-binop.js)  
```javascript
var e31 = Math.pow(2, 31);

assertEquals(-e31, -1*e31);
assertEquals(e31, -1*e31*(-1));
assertEquals(e31, -1*-e31);
assertEquals(e31, -e31*(-1));

var x = {toString : function() {return 1}}
function add(a,b){return a+b;}
add(1,x);
add(1,x);
%OptimizeFunctionOnNextCall(add);
add(1,x);
x.toString = function() {return "2"};

assertEquals(add(1,x), "12");

function Checker() {
  this.str = "1";
  var toStringCalled = 0;
  var toStringExpected = 0;
  this.toString = function() {
    toStringCalled++;
    return this.str;
  };
  this.check = function() {
    toStringExpected++;
    assertEquals(toStringExpected, toStringCalled);
  };
};
var left = new Checker();
var right = new Checker();

function test(fun,check_fun,a,b,does_throw) {
  left.str = a;
  right.str = b;
  try {
    assertEquals(check_fun(a,b), fun(left, right));
    assertTrue(!does_throw);
  } catch(e) {
    if (e instanceof TypeError) {
      assertTrue(!!does_throw);
    } else {
      throw e;
    }
  } finally {
    left.check();
    if (!does_throw || does_throw>1) {
      right.check();
    }
  }
}

function minus(a,b) { return a-b };
function check_minus(a,b) { return a-b };
function mod(a,b) { return a%b };
function check_mod(a,b) { return a%b };

test(minus,check_minus,1,2);
test(minus,check_minus,1<<30,1);
test(minus,check_minus,1,1<<30);
test(minus,check_minus,1<<30,-(1<<30));

test(minus,check_minus,1,1.4);
test(minus,check_minus,1.3,4);
test(minus,check_minus,1.3,1.4);
test(minus,check_minus,1,2);
test(minus,check_minus,1,undefined);
test(minus,check_minus,1,2);
test(minus,check_minus,1,true);
test(minus,check_minus,1,2);
test(minus,check_minus,1,null);
test(minus,check_minus,1,2);
test(minus,check_minus,1,"");
test(minus,check_minus,1,2);

test(minus,check_minus,{},1,1);
test(minus,check_minus,1,{},2);
test(minus,check_minus,{},{},1);

test(minus,check_minus,1,2);

test(mod,check_mod,1,2);
%OptimizeFunctionOnNextCall(mod);
test(mod,check_mod,1,2);

test(mod,check_mod,1<<30,1);
%OptimizeFunctionOnNextCall(mod);
test(mod,check_mod,1<<30,1);
test(mod,check_mod,1,1<<30);
%OptimizeFunctionOnNextCall(mod);
test(mod,check_mod,1,1<<30);
test(mod,check_mod,1<<30,-(1<<30));
%OptimizeFunctionOnNextCall(mod);
test(mod,check_mod,1<<30,-(1<<30));

test(mod,check_mod,1,{},2);
%OptimizeFunctionOnNextCall(mod);
test(mod,check_mod,1,{},2);

test(mod,check_mod,1,2);


function t1(a, b) {return a-b}
assertEquals(t1(1,2), 1-2);
assertEquals(t1(2,true), 2-1);
assertEquals(t1(false,2), 0-2);
assertEquals(t1(1,2.4), 1-2.4);
assertEquals(t1(1.3,2.4), 1.3-2.4);
assertEquals(t1(true,2.4), 1-2.4);
assertEquals(t1(1,undefined), 1-NaN);
assertEquals(t1(1,1<<30), 1-(1<<30));
assertEquals(t1(1,2), 1-2);

function t2(a, b) {return a/b}
assertEquals(t2(1,2), 1/2);
assertEquals(t2(null,2), 0/2);
assertEquals(t2(null,-2), 0/-2);
assertEquals(t2(2,null), 2/0);
assertEquals(t2(-2,null), -2/0);
assertEquals(t2(1,2.4), 1/2.4);
assertEquals(t2(1.3,2.4), 1.3/2.4);
assertEquals(t2(null,2.4), 0/2.4);
assertEquals(t2(1.3,null), 1.3/0);
assertEquals(t2(undefined,2), NaN/2);
assertEquals(t2(1,1<<30), 1/(1<<30));
assertEquals(t2(1,2), 1/2);


function string_add(a,i) {
  var d = [0.1, ,0.3];
  return a + d[i];
}

string_add(1.1, 0);
string_add("", 0);
%OptimizeFunctionOnNextCall(string_add);
string_add(1.1, 0);
assertEquals("undefined", string_add("", 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6fc2875^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6fc2875)  
[test/mjsunit/regress/regress-binop-nosse2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-binop-nosse2.js?cl=6fc2875)  
[test/mjsunit/regress/regress-binop.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-binop.js?cl=6fc2875)  
  
  
---   

## **regress-crbug-285355.js (chromium issue)**  
   
**[Issue 285355:
 Chrome_Linux: Crash Report - Magic Signature: v8::internal::Runtime_LazyRecompile](https://crbug.com/285355)**  
**[Commit: Fix bitwise negation on x64](https://chromium.googlesource.com/v8/v8/+/daee0d8)**  
  
Date(Commit): Fri Sep 06 15:21:38 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/24037003](https://codereview.chromium.org/24037003)  
Regress: [mjsunit/regress/regress-crbug-285355.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-285355.js)  
```javascript
function inverted_index() {
  return ~1;
}

%NeverOptimizeFunction(inverted_index);

function crash(array) {
  return array[~inverted_index()] = 2;
};
%PrepareFunctionForOptimization(crash);
assertEquals(2, crash(new Array(1)));
assertEquals(2, crash(new Array(1)));
%OptimizeFunctionOnNextCall(crash);
assertEquals(2, crash(new Array(1)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/daee0d8^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=daee0d8)  
[test/mjsunit/regress/regress-crbug-285355.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-285355.js?cl=daee0d8)  
  

---   

## **regress-regexp-construct-result.js (other issue)**  
   
**[Commit: Fix bug in regexp result object construction.](https://chromium.googlesource.com/v8/v8/+/d9659da)**  
  
Date(Commit): Thu Sep 05 14:32:49 2013  
Code Review: [https://codereview.chromium.org/23548018](https://codereview.chromium.org/23548018)  
Regress: [mjsunit/regress/regress-regexp-construct-result.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-regexp-construct-result.js)  
```javascript
var num_captures = 1000;
var regexp_string = "(a)";
for (var i = 0; i < num_captures - 1; i++) {
  regexp_string += "|(b)";
}
var regexp = new RegExp(regexp_string);

for (var i = 0; i < 10; i++) {
  var matches = regexp.exec("a");
  var count = 0;
  matches.forEach(function() { count++; });
  assertEquals(num_captures + 1, count);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9659da^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=d9659da)  
[test/mjsunit/regress/regress-regexp-construct-result.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-regexp-construct-result.js?cl=d9659da)  
  
  
---   

## **regress-store-uncacheable.js (other issue)**  
   
**[Commit: Allow uncacheable identifiers to go generic.](https://chromium.googlesource.com/v8/v8/+/3f70c3b)**  
  
Date(Commit): Mon Sep 02 16:32:11 2013  
Code Review: [https://chromiumcodereview.appspot.com/23453019](https://chromiumcodereview.appspot.com/23453019)  
Regress: [mjsunit/regress/regress-store-uncacheable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-uncacheable.js)  
```javascript
function f() {
  var o = {};
  o["<abc>"] = 123;
}

%PrepareFunctionForOptimization(f);
f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();
assertOptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f70c3b^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=3f70c3b)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=3f70c3b)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=3f70c3b)  
[test/mjsunit/regress/regress-store-uncacheable.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-store-uncacheable.js?cl=3f70c3b)  
  
  
---   

## **regress-2843.js (v8 issue)**  
   
**[Issue 2843:
 Flaky error on the 3.20 branch](https://crbug.com/v8/2843)**  
**[Commit: Delete HAbnormalExit. It does more harm than good.](https://chromium.googlesource.com/v8/v8/+/3747b5b)**  
  
Date(Commit): Wed Aug 28 15:00:30 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/23462007](https://codereview.chromium.org/23462007)  
Regress: [mjsunit/regress/regress-2843.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2843.js)  
```javascript
function bailout() { throw "bailout"; }
var global;

function foo(x, fun) {
  var a = x + 1;
  var b = x + 2;  // Need another Simulate to fold the first one into.
  global = true;  // Need a side effect to deopt to.
  fun();
  return a;
}
%PrepareFunctionForOptimization(foo);

%PrepareFunctionForOptimization(foo);
assertThrows("foo(1, bailout)");
assertThrows("foo(1, bailout)");
%OptimizeFunctionOnNextCall(foo);
assertThrows("foo(1, bailout)");
assertEquals(2, foo(1, function() {}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3747b5b^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=3747b5b)  
[src/hydrogen-environment-liveness.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-environment-liveness.cc?cl=3747b5b)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=3747b5b)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3747b5b)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=3747b5b)  
...  
  

---   

## **regress-merge-descriptors.js (other issue)**  
   
**[Commit: Merge verbatim descriptors from other (the descriptor of the map being updated) rather than this (descriptors of the most updated map found in the transition tree).](https://chromium.googlesource.com/v8/v8/+/652b174)**  
  
Date(Commit): Wed Aug 28 12:37:14 2013  
Code Review: [https://chromiumcodereview.appspot.com/23676003](https://chromiumcodereview.appspot.com/23676003)  
Regress: [mjsunit/regress/regress-merge-descriptors.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-merge-descriptors.js)  
```javascript
var extend = function (d, b) {
  function __() { this.constructor = d; }
  __.prototype = b.prototype;
  d.prototype = new __();
};

var Car = (function (Super) {
  var Car = function () {
    var self = this;

    Super.call(self);

    Object.defineProperties(self, {
      "make": {
        enumerable: true,
        configurable: true,
        get: function () {
          return "Ford";
        }
      }
    });

    self.copy = function () {
      throw new Error("Meant to be overriden");
    };

    return self;
  };

  extend(Car, Super);

  return Car;
}(Object));


var SuperCar = ((function (Super) {
  var SuperCar = function (make) {
    var self = this;

    Super.call(self);


    Object.defineProperties(self, {
      "make": {
        enumerable: true,
        configurable: true,
        get: function () {
          return make;
        }
      }
    });

    // Convert self.copy from DATA_CONSTANT to DATA.
    self.copy = function () { };

    return self;
  };
  extend(SuperCar, Super);
  return SuperCar;
})(Car));

assertEquals("Ford", new Car().make);
assertEquals("Bugatti", new SuperCar("Bugatti").make);
assertEquals("Lambo", new SuperCar("Lambo").make);
assertEquals("Shelby", new SuperCar("Shelby").make);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/652b174^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=652b174)  
[test/mjsunit/regress/regress-merge-descriptors.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-merge-descriptors.js?cl=652b174)  
  
  
---   

## **regress-2790.js (v8 issue)**  
   
**[Issue 2790:
 Sparse array constructor gives "illegal access" at 98297 elements and beyond](https://crbug.com/v8/2790)**  
**[Commit: Lower kInitialMaxFastElementArray constant to 95K](https://chromium.googlesource.com/v8/v8/+/11fd577)**  
  
Date(Commit): Mon Aug 26 13:04:05 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/22877039](https://codereview.chromium.org/22877039)  
Regress: [mjsunit/regress/regress-2790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2790.js)  
```javascript
for (var i = 1000; i < 1000000; i += 19703) {
  new Array(i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11fd577^!)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=11fd577)  
[test/mjsunit/regress/regress-2790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2790.js?cl=11fd577)  
  

---   

## **regress-2855.js (v8 issue)**  
   
**[Issue 2855:
 overriding String.prototype.valueOf doesn't work in v8](https://crbug.com/v8/2855)**  
**[Commit: Temporarily disable optimization for StringWrappers to use native valueOf](https://chromium.googlesource.com/v8/v8/+/de7352d)**  
  
Date(Commit): Fri Aug 23 11:31:18 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/23060030](https://codereview.chromium.org/23060030)  
Regress: [mjsunit/regress/regress-2855.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2855.js)  
```javascript
function foo(a) {
  for (var i = 0; i < 100; ++i)
    a = new String(a);
  return a;
}

var expected = "hello";
for (var i = 0; i < 4; ++i) {
  if (i == 2) {
    String.prototype.valueOf = function() { return 42; }
    expected = "42";
  }
  assertEquals(expected, "" + foo("hello"));
}

var count = 0;
var x = new String("foo");
Object.defineProperty(x, "valueOf",
    { get: function() {
             count++;
             return function() {
                      return 11;
                    }
           }
    });
for (var i = 0; i < 3; i++) {
  assertEquals("11", "" + x);
  assertEquals(i + 1, count);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/de7352d^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=de7352d)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=de7352d)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=de7352d)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=de7352d)  
[test/mjsunit/regress/regress-2855.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2855.js?cl=de7352d)  
  

---   

## **regress-2594.js (v8 issue)**  
   
**[Issue 2594:
 Crash when accessing lexically scoped catch-variable from eval'ed function declaration](https://crbug.com/v8/2594)**  
**[Commit: Fix scoping of function declarations in eval inside non-trivial local scope](https://chromium.googlesource.com/v8/v8/+/971df38)**  
  
Date(Commit): Fri Aug 23 09:25:37 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/22901010](https://codereview.chromium.org/22901010)  
Regress: [mjsunit/regress/regress-2594.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2594.js)  
```javascript
function f1() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    eval("var h = function() { return XXX }")
  }
  return h()
}
assertEquals(1, f1())

function f2() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    eval("function h(){ return XXX }")
  }
  return h()
}
assertEquals(1, f2())

function f3() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    try { throw 2 } catch (y) {
      eval("function h(){ return XXX }")
    }
  }
  return h()
}
assertEquals(1, f3())

function f4() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    with ({}) {
      eval("function h(){ return XXX }")
    }
  }
  return h()
}
assertEquals(1, f4())

function f5() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    eval('eval("function h(){ return XXX }")')
  }
  return h()
}
assertEquals(1, f5())

function f6() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    eval("var h = (function() { function g(){ return XXX } return g })()")
  }
  return h()
}
assertEquals(1, f6())

function f7() {
  var XXX = 0
  try { throw 1 } catch (XXX) {
    eval("function h() { var XXX=2; function g(){ return XXX } return g }")
  }
  return h()()
}
assertEquals(2, f7())  // !

var XXX = 0
try { throw 1 } catch (XXX) {
  eval("function h(){ return XXX }")
}
assertEquals(1, h())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/971df38^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=971df38)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=971df38)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=971df38)  
[test/mjsunit/regress/regress-2594.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2594.js?cl=971df38)  
  

---   

## **regress-2829.js (v8 issue)**  
   
**[Issue 2829:
 WeakMap does not keep entries after interaction with Object.freeze(Object.prototype)](https://crbug.com/v8/2829)**  
**[Commit: Fix hidden properties on object with frozen prototype.](https://chromium.googlesource.com/v8/v8/+/0ecd03a)**  
  
Date(Commit): Thu Aug 22 13:51:32 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/22799021](https://codereview.chromium.org/22799021)  
Regress: [mjsunit/es6/regress/regress-2829.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2829.js)  
```javascript
(function test1() {
  var wm1 = new WeakMap();
  wm1.set(Object.prototype, 23);
  assertTrue(wm1.has(Object.prototype));
  Object.freeze(Object.prototype);

  var wm2 = new WeakMap();
  var o = {};
  wm2.set(o, 42);
  assertEquals(42, wm2.get(o));
})();

(function test2() {
  var wm1 = new WeakMap();
  var o1 = {};
  wm1.set(o1, 23);
  assertTrue(wm1.has(o1));
  Object.freeze(o1);

  var wm2 = new WeakMap();
  var o2 = Object.create(o1);
  wm2.set(o2, 42);
  assertEquals(42, wm2.get(o2));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0ecd03a^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0ecd03a)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=0ecd03a)  
[test/mjsunit/regress/regress-2829.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2829.js?cl=0ecd03a)  
  

---   

## **regress-shared-deopt.js (other issue)**  
   
**[Commit: Fix deoptimization bug, where recursive call can frighten and confuse the unwitting, simple, poor caveman that is Runtime_NotifyDeoptimized.](https://chromium.googlesource.com/v8/v8/+/6f3169e)**  
  
Date(Commit): Thu Aug 22 13:03:40 2013  
Code Review: [https://codereview.chromium.org/23201016](https://codereview.chromium.org/23201016)  
Regress: [mjsunit/compiler/regress-shared-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shared-deopt.js)  
```javascript
var soft = false;

soft = true;
soft = false;
soft = true;
soft = false;

function test() {
  var f4 = makeF(4);
  var f5 = makeF(5);

  function makeF(i) {
    return function f(x) {
      if (x == 0) return i;
      if (i == 4) if (soft) print("wahoo" + i);
      return f4(x - 1);
    }
  }

  %PrepareFunctionForOptimization(f4);
  f4(9);
  f4(11);
  %OptimizeFunctionOnNextCall(f4);
  f4(12);

  %PrepareFunctionForOptimization(f5);
  f5(9);
  f5(11);
  %OptimizeFunctionOnNextCall(f5);
  f5(12);

  soft = true;
  f4(1);
  f5(9);
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f3169e^!)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=6f3169e)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=6f3169e)  
[test/mjsunit/compiler/regress-shared-deopt.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-shared-deopt.js?cl=6f3169e)  
  
  
---   

## **regress-x87.js (other issue)**  
   
**[Commit: Add X87 implementations for Integer32ToDouble, DoubleToI, DoubleToSmi](https://chromium.googlesource.com/v8/v8/+/383a167)**  
  
Date(Commit): Tue Aug 20 13:01:54 2013  
Code Review: [https://codereview.chromium.org/20781007](https://codereview.chromium.org/20781007)  
Regress: [mjsunit/regress/regress-x87.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-x87.js)  
```javascript
var x;
var a = new Float32Array([1,2, 4, 6, 8, 11, NaN, 1/0, -3])
var val = 2.1*a[1]*a[0]*a[1*2*3*0]*a[1*1]*1.0;
assertEquals(8.4, val);

var a;
var t = true;
var res = [2.5, 2];
for (var i = 0; i < 2; i++) {
  if (t) {
    a = 1.5;
  } else {
    a = true;
  }
  assertEquals(res[i], a+1);
  t = false;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/383a167^!)  
[src/ia32/assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.cc?cl=383a167)  
[src/ia32/assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.h?cl=383a167)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=383a167)  
[src/ia32/lithium-codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.h?cl=383a167)  
[src/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.h?cl=383a167)  
...  
  
  
---   

## **regress-convert-function-to-double.js (other issue)**  
   
**[Commit: Store copied value rather than the original double.](https://chromium.googlesource.com/v8/v8/+/d81af53)**  
  
Date(Commit): Fri Aug 16 15:43:42 2013  
Code Review: [https://chromiumcodereview.appspot.com/23262002](https://chromiumcodereview.appspot.com/23262002)  
Regress: [mjsunit/regress/regress-convert-function-to-double.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-function-to-double.js)  
```javascript
function f(v) {
  this.func = v;
}

var o1 = new f(f);
var d = 1.4;
var o2 = new f(d);
o2.func = 1.8;
assertEquals(1.4, d)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d81af53^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d81af53)  
[test/mjsunit/regress/regress-convert-function-to-double.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-convert-function-to-double.js?cl=d81af53)  
  
  
---   

## **regress-crbug-274438.js (chromium issue)**  
   
**[Issue 274438:
 Chrome: Crash Report - Magic Signature: V8_Fatal](https://crbug.com/274438)**  
**[Commit: Mark HStringCompareAndBranch as potentially causing GCs.](https://chromium.googlesource.com/v8/v8/+/3e4fbd0)**  
  
Date(Commit): Fri Aug 16 15:10:07 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/22933006](https://codereview.chromium.org/22933006)  
Regress: [mjsunit/regress/regress-crbug-274438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-274438.js)  
```javascript
function f(a, b) {
  var x = {a: a};
  switch (b) { case 'string': }
  var y = {b: b};
  return y;
};
%PrepareFunctionForOptimization(f);
f("a", "b");
f("a", "b");
%OptimizeFunctionOnNextCall(f);
f("a", "b");
%SetAllocationTimeout(100, 0);
var killer = f("bang", "bo" + "om");
assertEquals("boom", killer.b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e4fbd0^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=3e4fbd0)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=3e4fbd0)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=3e4fbd0)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3e4fbd0)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=3e4fbd0)  
...  
  

---   

## **regress-crbug-272564.js (chromium issue)**  
   
**[Issue 272564:
 Chrome_Mac: Crash Report - Magic Signature: v8::internal::TypeInfo::FromValue](https://crbug.com/272564)**  
**[Commit: Fix Math.round/floor that had bogus Smi representation](https://chromium.googlesource.com/v8/v8/+/e71a91c)**  
  
Date(Commit): Wed Aug 14 12:14:08 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/23022005](https://codereview.chromium.org/23022005)  
Regress: [mjsunit/regress/regress-crbug-272564.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-272564.js)  
```javascript
function Bb(w) {
  this.width = w;
}

function ce(a, b) {
  "number" == typeof a && (a = (b ? Math.round(a) : a) + "px");
  return a;
}

function pe(a, b, c) {
  if (b instanceof Bb) b = b.width;
  a.width = ce(b, !0);
};
%PrepareFunctionForOptimization(pe);
var a = new Bb(1);
var b = new Bb(5);
pe(a, b, 0);
pe(a, b, 0);
%OptimizeFunctionOnNextCall(pe);
pe(a, b, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e71a91c^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=e71a91c)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=e71a91c)  
[test/mjsunit/regress/regress-crbug-272564.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-272564.js?cl=e71a91c)  
  

---   

## **regress-convert-hole2.js (other issue)**  
   
**[Commit: Never hchange nan-hole to hole or hole to nan-hole.](https://chromium.googlesource.com/v8/v8/+/169f5a9)**  
  
Date(Commit): Wed Aug 14 08:54:27 2013  
Code Review: [https://chromiumcodereview.appspot.com/22152003](https://chromiumcodereview.appspot.com/22152003)  
Regress: [mjsunit/regress/regress-convert-hole2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-hole2.js)  
```javascript
var a = [1.5, , 1.8];

function f(a, i, l) {
  var v = a[i];
  return l + v;
}

assertEquals("test1.5", f(a, 0, "test"));
assertEquals("test1.5", f(a, 0, "test"));
%OptimizeFunctionOnNextCall(f);
assertEquals("testundefined", f(a, 1, "test"));

function f2(b, a1, a2) {
  var v;
  if (b) {
    v = a1[0];
  } else {
    v = a2[0];
  }
  x = v * 2;
  return "test" + v + x;
}

f2(true, [1.4,1.8,,1.9], [1.4,1.8,,1.9]);
f2(true, [1.4,1.8,,1.9], [1.4,1.8,,1.9]);
f2(false, [1.4,1.8,,1.9], [1.4,1.8,,1.9]);
f2(false, [1.4,1.8,,1.9], [1.4,1.8,,1.9]);
%OptimizeFunctionOnNextCall(f2);
assertEquals("testundefinedNaN", f2(false, [,1.8,,1.9], [,1.9,,1.9]));

function t_smi(a) {
  a[0] = 1.5;
}

t_smi([1,,3]);
t_smi([1,,3]);
t_smi([1,,3]);
%OptimizeFunctionOnNextCall(t_smi);
var ta = [1,,3];
t_smi(ta);
ta.__proto__ = [6,6,6];
assertEquals([1.5,6,3], ta);

function t(b) {
  b[1] = {};
}

t([1.4, 1.6,,1.8, NaN]);
t([1.4, 1.6,,1.8, NaN]);
%OptimizeFunctionOnNextCall(t);
var a = [1.6, 1.8,,1.9, NaN];
t(a);
a.__proto__ = [6,6,6,6,6];
assertEquals([1.6, {}, 6, 1.9, NaN], a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/169f5a9^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=169f5a9)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=169f5a9)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=169f5a9)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=169f5a9)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=169f5a9)  
...  
  
  
---   

## **regress-2836.js (v8 issue)**  
   
**[Issue 2836:
 Smi operations erroneously skip overflow check](https://crbug.com/v8/2836)**  
**[Commit: Fix overflow check computation for Smi Phis](https://chromium.googlesource.com/v8/v8/+/6f800f9)**  
  
Date(Commit): Tue Aug 13 18:18:24 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/22629011](https://codereview.chromium.org/22629011)  
Regress: [mjsunit/regress/regress-2836.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2836.js)  
```javascript
function f() {
  var end = 1073741823;  // 2^30 - 1
  var start = end - 100000;  // Run long enough to trigger OSR.
  for (var i = start; i <= end; ++i) {
    assertTrue(i >= start);  // No overflow allowed!
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f800f9^!)  
[src/hydrogen-representation-changes.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-representation-changes.cc?cl=6f800f9)  
[test/mjsunit/regress/regress-2836.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2836.js?cl=6f800f9)  
  

---   

## **regress-phi-truncation.js (other issue)**  
   
**[Commit: Fix bug in HPhi::SimplifyConstantInput](https://chromium.googlesource.com/v8/v8/+/415b61e)**  
  
Date(Commit): Tue Aug 13 16:47:27 2013  
Code Review: [https://codereview.chromium.org/23075003](https://codereview.chromium.org/23075003)  
Regress: [mjsunit/regress/regress-phi-truncation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-phi-truncation.js)  
```javascript
function test(fun, expectation) {
  assertEquals(1, fun(1));
  %OptimizeFunctionOnNextCall(fun);
  assertEquals(expectation, fun(0));
}

test(function(x) {
  var a = x ? true : false;
  return a | 0;
}, 0);

test(function(x) {
  var a = x ? true : true;
  return a | 0;
}, 1);

test(function(x) {
  var a = x ? true : "0";
  return a | 0;
}, 0);

test(function(x) {
  var a = x ? true : "1";
  return a | 0;
}, 1);

test(function(x) {
  var a = x ? true : "-1";
  return a | 0;
}, -1);

test(function(x) {
  var a = x ? true : "-0";
  return a | 0;
}, 0);

test(function(x) {
  var a = x ? true : "0x1234";
  return a | 0;
}, 0x1234);

test(function(x) {
  var a = x ? true : { valueOf: function() { return 2; } };
  return a | 0;
}, 2);

test(function(x) {
  var a = x ? true : undefined;
  return a | 0;
}, 0);

test(function(x) {
  var a = x ? true : null;
  return a | 0;
}, 0);

test(function(x) {
  var a = x ? true : "";
  return a | 0;
}, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/415b61e^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=415b61e)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=415b61e)  
[test/mjsunit/regress/regress-phi-truncation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-phi-truncation.js?cl=415b61e)  
  
  
---   

## **regress-et-clobbers-doubles.js (other issue)**  
   
**[Commit: Store doubles before calling into the elements transition stub on ARM](https://chromium.googlesource.com/v8/v8/+/145f240)**  
  
Date(Commit): Tue Aug 13 15:06:17 2013  
Code Review: [https://chromiumcodereview.appspot.com/22854011](https://chromiumcodereview.appspot.com/22854011)  
Regress: [mjsunit/regress/regress-et-clobbers-doubles.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-et-clobbers-doubles.js)  
```javascript
function t_smi(a) {
  a[0] = 1.5;
};
%PrepareFunctionForOptimization(t_smi);
t_smi([1, , 3]);
t_smi([1, , 3]);
t_smi([1, , 3]);
%OptimizeFunctionOnNextCall(t_smi);
var ta = [1, , 3];
t_smi(ta);
assertEquals([1.5, , 3], ta);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/145f240^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=145f240)  
[test/mjsunit/regress/regress-et-clobbers-doubles.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-et-clobbers-doubles.js?cl=145f240)  
  
  
---   

## **regress-map-invalidation-2.js (other issue)**  
   
**[Commit: Fix regressions triggered by map invalidation during graph creation.](https://chromium.googlesource.com/v8/v8/+/c52b7bb)**  
  
Date(Commit): Mon Aug 12 14:10:25 2013  
Code Review: [https://codereview.chromium.org/22807003](https://codereview.chromium.org/22807003)  
Regress: [mjsunit/regress/regress-map-invalidation-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-map-invalidation-2.js)  
```javascript
var c = { x: 2, y: 1 };

function g() {
  var outer = { foo: 1 };
  function f(b, c) {
    var n = outer.foo;
    for (var i = 0; i < 10; i++) {
      n += c.x + outer.foo;
    }
    if (b) return [{ x: 1.5, y: 1 }];
    else return c;
  }
  // Clear type feedback from previous stress runs.
  %ClearFunctionFeedback(f);
  return f;
}

var fun = g();
fun(false, c);
fun(false, c);
fun(false, c);
%OptimizeFunctionOnNextCall(fun);
fun(false, c);
fun(true, c);
assertOptimized(fun);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c52b7bb^!)  
[src/assert-scope.h](https://cs.chromium.org/chromium/src/v8/src/assert-scope.h?cl=c52b7bb)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=c52b7bb)  
[src/compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler.h?cl=c52b7bb)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c52b7bb)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=c52b7bb)  
...  
  
  
---   

## **regress-map-invalidation-1.js (other issue)**  
   
**[Commit: Fix regressions triggered by map invalidation during graph creation.](https://chromium.googlesource.com/v8/v8/+/c52b7bb)**  
  
Date(Commit): Mon Aug 12 14:10:25 2013  
Code Review: [https://codereview.chromium.org/22807003](https://codereview.chromium.org/22807003)  
Regress: [mjsunit/regress/regress-map-invalidation-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-map-invalidation-1.js)  
```javascript
var c = { x: 2, y: 1 };

function h() {
  %TryMigrateInstance(c);
  return 2;
}
%NeverOptimizeFunction(h);

function f() {
  for (var i = 0; i < 100000; i++) {
    var n = c.x + h();
    assertEquals(4, n);
  }
  var o2 = [{ x: 2.5, y:1 }];
  return o2;
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c52b7bb^!)  
[src/assert-scope.h](https://cs.chromium.org/chromium/src/v8/src/assert-scope.h?cl=c52b7bb)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=c52b7bb)  
[src/compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler.h?cl=c52b7bb)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c52b7bb)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=c52b7bb)  
...  
  
  
---   

## **regress-hoist-load-named-field.js (other issue)**  
   
**[Commit: Make all load-named-fields depend on their map-check, unless explicitly ignored.](https://chromium.googlesource.com/v8/v8/+/ee53b0a)**  
  
Date(Commit): Fri Aug 09 18:40:10 2013  
Code Review: [https://chromiumcodereview.appspot.com/22555004](https://chromiumcodereview.appspot.com/22555004)  
Regress: [mjsunit/regress/regress-hoist-load-named-field.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-hoist-load-named-field.js)  
```javascript
function f(o, a) {
  var v;
  var i = 1;
  while (i < 2) {
    v = o.y;
    a[0] = 1.5;
    i++;
  }
  return v;
}

f({y:1.4}, [1]);
f({y:1.6}, [1]);
%OptimizeFunctionOnNextCall(f);
f({x:1}, [1]);

function f2(o) {
  var i = 0;
  var v;
  while (i < 1) {
    v = o.x;
    i++;
  }
  return v;
}

var o1 = { x: 1.5 };
var o2 = { y: 1, x: 1 };

f2(o1);
f2(o1);
f2(o2);
%OptimizeFunctionOnNextCall(f2);
f2(o2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee53b0a^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=ee53b0a)  
[src/hydrogen-gvn.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-gvn.h?cl=ee53b0a)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=ee53b0a)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=ee53b0a)  
[test/mjsunit/regress/regress-hoist-load-named-field.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-hoist-load-named-field.js?cl=ee53b0a)  
  
  
---   

## **regress-smi-math-floor-round.js (other issue)**  
   
**[Commit: Fix smi-based math floor.](https://chromium.googlesource.com/v8/v8/+/1965964)**  
  
Date(Commit): Fri Aug 09 11:21:03 2013  
Code Review: [https://chromiumcodereview.appspot.com/22623007](https://chromiumcodereview.appspot.com/22623007)  
Regress: [mjsunit/regress/regress-smi-math-floor-round.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-math-floor-round.js)  
```javascript
function f(o) {
  return Math.floor(o.x_smi) + 1;
};
%PrepareFunctionForOptimization(f);
assertEquals(2, f({x_smi: 1}));
assertEquals(2, f({x_smi: 1}));
%OptimizeFunctionOnNextCall(f);
assertEquals(2, f({x_smi: 1}));

function f2(o) {
  return Math.floor(o.x_tagged) + 1;
};
%PrepareFunctionForOptimization(f2);
var o = {x_tagged: {}};
o.x_tagged = 1.4;
assertEquals(2, f2(o));
assertEquals(2, f2(o));
%OptimizeFunctionOnNextCall(f2);
assertEquals(2, f2(o));

function f3(o) {
  return Math.round(o.x_smi) + 1;
};
%PrepareFunctionForOptimization(f3);
assertEquals(2, f3({x_smi: 1}));
assertEquals(2, f3({x_smi: 1}));
%OptimizeFunctionOnNextCall(f3);
assertEquals(2, f3({x_smi: 1}));

function f4(o) {
  return Math.round(o.x_tagged) + 1;
};
%PrepareFunctionForOptimization(f4);
assertEquals(2, f4(o));
assertEquals(2, f4(o));
%OptimizeFunctionOnNextCall(f4);
assertEquals(2, f4(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1965964^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=1965964)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=1965964)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=1965964)  
[test/mjsunit/regress/regress-smi-math-floor-round.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-smi-math-floor-round.js?cl=1965964)  
  
  
---   

## **regress-freeze.js (other issue)**  
   
**[Commit: Fix Object.freeze, Object.observe wrt CountOperation and CompoundAssignment.](https://chromium.googlesource.com/v8/v8/+/e5afd32)**  
  
Date(Commit): Wed Aug 07 18:45:41 2013  
Code Review: [https://chromiumcodereview.appspot.com/22562004](https://chromiumcodereview.appspot.com/22562004)  
Regress: [mjsunit/regress/regress-freeze.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-freeze.js)  
```javascript
function f(o) {
  o.x++;
};
%PrepareFunctionForOptimization(f);
;
var o = {x: 5};
Object.freeze(o);
f(o);
f(o);
%OptimizeFunctionOnNextCall(f);
f(o);
assertEquals(5, o.x);

function f2(o) {
  o.x += 3;
};
%PrepareFunctionForOptimization(f2);
;
f2(o);
f2(o);
%OptimizeFunctionOnNextCall(f2);
f2(o);
assertEquals(5, o.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5afd32^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e5afd32)  
[src/typing.cc](https://cs.chromium.org/chromium/src/v8/src/typing.cc?cl=e5afd32)  
[test/mjsunit/harmony/object-observe.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/object-observe.js?cl=e5afd32)  
[test/mjsunit/regress/regress-freeze.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-freeze.js?cl=e5afd32)  
  
  
---   

## **regress-264203.js (chromium issue)**  
   
**[Issue 264203:
 Gun Bros doesn't play in R29](https://crbug.com/264203)**  
**[Commit: Fix Array index dehoisting.](https://chromium.googlesource.com/v8/v8/+/3511f7a)**  
  
Date(Commit): Tue Aug 06 16:38:39 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-29", "ReleaseBlock-Stable"]  
Code Review: [https://chromiumcodereview.appspot.com/22314012](https://chromiumcodereview.appspot.com/22314012)  
Regress: [mjsunit/regress/regress-264203.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-264203.js)  
```javascript
function foo(x) {
  var a = [1, 2, 3, 4, 5, 6, 7, 8];
  a[x + 5];
  var result;
  for (var i = 0; i < 3; i++) {
    result = a[0 - x];
  }
  return result;
}
%PrepareFunctionForOptimization(foo);

%PrepareFunctionForOptimization(foo);
foo(0);
foo(0);
%OptimizeFunctionOnNextCall(foo);
var r = foo(-2);
assertEquals(3, r);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3511f7a^!)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=3511f7a)  
[test/mjsunit/regress/regress-264203.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-264203.js?cl=3511f7a)  
  

---   

## **regress-2813.js (v8 issue)**  
   
**[Issue 2813:
 Incorrect behavior on box2d.js (regression)](https://crbug.com/v8/2813)**  
**[Commit: Regression test for issue 2813 / r16008](https://chromium.googlesource.com/v8/v8/+/232a2c0)**  
  
Date(Commit): Fri Aug 02 12:17:19 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/21806002](https://codereview.chromium.org/21806002)  
Regress: [mjsunit/regress/regress-2813.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2813.js)  
```javascript
function foo(x) {
  var a = x + 1;
  var b = x + 2;
  if (x != 0) {
    if (x > 0 & x < 100) {
      return a;
    }
  }
  return 0;
};
%PrepareFunctionForOptimization(foo);
assertEquals(0, foo(0));
assertEquals(0, foo(0));
%OptimizeFunctionOnNextCall(foo);
assertEquals(3, foo(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/232a2c0^!)  
[test/mjsunit/regress/regress-2813.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2813.js?cl=232a2c0)  
  

---   

## **regress-omit-checks.js (other issue)**  
   
**[Commit: Mark maps as unstable if their instances potentially transition away.](https://chromium.googlesource.com/v8/v8/+/2af164f)**  
  
Date(Commit): Tue Jul 30 16:33:58 2013  
Code Review: [https://chromiumcodereview.appspot.com/21095005](https://chromiumcodereview.appspot.com/21095005)  
Regress: [mjsunit/regress/regress-omit-checks.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-omit-checks.js)  
```javascript
var a = {x: 1};
var a_deprecate = {x: 1};
a_deprecate.x = 1.5;
function create() {
  return {__proto__: a, y: 1};
}
var b1 = create();
var b2 = create();
var b3 = create();
var b4 = create();

function set(b) {
  b.x = 5;
  b.z = 10;
};
%PrepareFunctionForOptimization(set);
set(b1);
set(b2);
%OptimizeFunctionOnNextCall(set);
set(b3);
var called = false;
a.x = 1.5;
Object.defineProperty(a, 'z', {
  set: function(v) {
    called = true;
  }
});
set(b4);
assertTrue(called);
assertEquals(undefined, b4.z);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2af164f^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=2af164f)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=2af164f)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=2af164f)  
[test/mjsunit/regress/regress-omit-checks.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-omit-checks.js?cl=2af164f)  
  
  
---   

## **regress-crbug-258519.js (chromium issue)**  
   
**[Issue 258519:
 Impossible "Cannot call method 'l$' of undefined" error](https://crbug.com/258519)**  
**[Commit: Add regression test for recently fixed bug](https://chromium.googlesource.com/v8/v8/+/3619dcf)**  
  
Date(Commit): Fri Jul 26 14:58:30 2013  
Components/Type: None/Bug-Regression  
Labels: ["Via-Wizard", "Needs-Feedback", "Hotlist-GoogleApps"]  
Code Review: [https://codereview.chromium.org/20732002](https://codereview.chromium.org/20732002)  
Regress: [mjsunit/regress/regress-crbug-258519.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-258519.js)  
```javascript
var a = {
  compare_null: function(x) {
    return null != x;
  },
  kaboom: function() {}
};

function crash(x) {
  var b = a;
  b.compare_null(x) && b.kaboom();
  return "ok";
};
%PrepareFunctionForOptimization(crash);
assertEquals("ok", crash(null));
assertEquals("ok", crash(null));
%OptimizeFunctionOnNextCall(crash);
assertEquals("ok", crash(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3619dcf^!)  
[test/mjsunit/regress/regress-crbug-258519.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-258519.js?cl=3619dcf)  
  

---   

## **regress-deopt-store-effect.js (other issue)**  
   
**[Commit: Fix deopt in store with effect context.](https://chromium.googlesource.com/v8/v8/+/88a4b0d)**  
  
Date(Commit): Fri Jul 19 13:45:26 2013  
Code Review: [https://chromiumcodereview.appspot.com/19693004](https://chromiumcodereview.appspot.com/19693004)  
Regress: [mjsunit/regress/regress-deopt-store-effect.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-store-effect.js)  
```javascript
var pro = {x: 1};
var a = {};
a.__proto__ = pro;
delete pro.x;

function g(o) {
  return 7 + (o.z = 1, 20);
};
%PrepareFunctionForOptimization(g);
g(a);
g(a);
%OptimizeFunctionOnNextCall(g);
Object.defineProperty(pro, 'z', {
  set: function(v) {
    %DeoptimizeFunction(g);
  },
  get: function() {
    return 20;
  }
});

assertEquals(27, g(a));


var i = {z: 2, r: 1};
var j = {z: 2};
var p = {a: 10};
var pp = {a: 20, b: 1};

function bar(o, p) {
  return 7 + (o.z = 1, p.a);
};
%PrepareFunctionForOptimization(bar);
bar(i, p);
bar(i, p);
bar(j, p);
%OptimizeFunctionOnNextCall(bar);
assertEquals(27, bar(i, pp));


var i = {r: 1, z: 2};
var j = {z: 2};
var p = {a: 10};
var pp = {a: 20, b: 1};

function bar1(o, p) {
  return 7 + (o.z = 1, p.a);
};
%PrepareFunctionForOptimization(bar1);
bar1(i, p);
bar1(i, p);
bar1(j, p);
%OptimizeFunctionOnNextCall(bar1);
assertEquals(27, bar1(i, pp));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88a4b0d^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=88a4b0d)  
[test/mjsunit/regress/regress-deopt-store-effect.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-deopt-store-effect.js?cl=88a4b0d)  
  
  
---   

## **regress-252797.js (chromium issue)**  
   
**[Issue 252797:
 IodineGBA runs slowly in Google Chrome](https://crbug.com/252797)**  
**[Commit: Fixed type feedback in presence of negative lookups.](https://chromium.googlesource.com/v8/v8/+/b951f03)**  
  
Date(Commit): Thu Jul 18 09:12:44 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/19588002](https://codereview.chromium.org/19588002)  
Regress: [mjsunit/regress/regress-252797.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-252797.js)  
```javascript
var holder = Object.create({}, {
  holderMethod: {value: function() {}}
});
assertTrue(%HasFastProperties(holder));

var holder = Object.create(null, {
  holderMethod: {value: function() {}}
});
assertFalse(%HasFastProperties(holder));

var receiver = Object.create(holder, {
  killMe: {value: 0, configurable: true},
  keepMe: {value: 0, configurable: true}
});
delete receiver.killMe;
assertFalse(%HasFastProperties(receiver));

function callConstantFunctionOnPrototype(obj) {
  obj.holderMethod();
}

%PrepareFunctionForOptimization(callConstantFunctionOnPrototype);
callConstantFunctionOnPrototype(receiver);
callConstantFunctionOnPrototype(receiver);
%OptimizeFunctionOnNextCall(callConstantFunctionOnPrototype);
callConstantFunctionOnPrototype(receiver);

assertOptimized(callConstantFunctionOnPrototype);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b951f03^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=b951f03)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=b951f03)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=b951f03)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=b951f03)  
[test/mjsunit/regress/regress-252797.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-252797.js?cl=b951f03)  
  

---   

## **regress-crbug-260345.js (chromium issue)**  
   
**[Issue 260345:
 [Crash] v8::internal::StackFrameIterator::AdvanceWithHandler](https://crbug.com/260345)**  
**[Commit: Synchronize Compare-Literal behavior in FullCodegen and Hydrogen](https://chromium.googlesource.com/v8/v8/+/22f2fd8)**  
  
Date(Commit): Wed Jul 17 13:13:38 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-GoogleApps"]  
Code Review: [https://codereview.chromium.org/19582002](https://codereview.chromium.org/19582002)  
Regress: [mjsunit/regress/regress-crbug-260345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-260345.js)  
```javascript
var steps = 100000;
var undefined_values = [undefined, "go on"];
var null_values = [null, "go on"];

function get_undefined_object(i) {
  return undefined_values[(i / steps) | 0];
}

function test_undefined() {
  var objects = 0;
  for (var i = 0; i < 2 * steps; i++) {
    undefined == get_undefined_object(i) && objects++;
  }
  return objects;
}

assertEquals(steps, test_undefined());


function get_null_object(i) {
  return null_values[(i / steps) | 0];
}

function test_null() {
  var objects = 0;
  for (var i = 0; i < 2 * steps; i++) {
    null == get_null_object(i) && objects++;
  }
  return objects;
}

assertEquals(steps, test_null());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22f2fd8^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=22f2fd8)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=22f2fd8)  
[test/mjsunit/regress/regress-crbug-260345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-260345.js?cl=22f2fd8)  
  

---   

## **regress-173361.js (chromium issue)**  
   
**[Issue 173361:
 Crash in webview](https://crbug.com/173361)**  
**[Commit: Fix sloppy-mode 'const' under Harmony flag.](https://chromium.googlesource.com/v8/v8/+/db76aa2)**  
  
Date(Commit): Mon Jul 15 14:12:20 2013  
Components/Type: Platform>Apps/Bug  
Labels: ["HasTestcase"]  
Code Review: [https://codereview.chromium.org/19199002](https://codereview.chromium.org/19199002)  
Regress: [mjsunit/harmony/regress/regress-173361.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-173361.js)  
```javascript
const x = 7;

function f() { const y = 8; }
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db76aa2^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=db76aa2)  
[test/mjsunit/regress/regress-173361.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-173361.js?cl=db76aa2)  
  

---   

## **regress-2711.js (v8 issue)**  
   
**[Issue 2711:
 Array.prototype.push apparently no longer honoring frozen](https://crbug.com/v8/2711)**  
**[Commit: Don't use StoreIC_ArrayLength on frozen arrays](https://chromium.googlesource.com/v8/v8/+/c65f4f7)**  
  
Date(Commit): Sun Jul 14 22:03:46 2013  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/19115002](https://chromiumcodereview.appspot.com/19115002)  
Regress: [mjsunit/regress/regress-2711.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2711.js)  
```javascript
var a = Object.freeze([1]);
assertThrows(function() { a.push(2); }, TypeError);
assertEquals(1, a.length);
assertThrows(function() { a.push(2); }, TypeError);
assertEquals(1, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c65f4f7^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=c65f4f7)  
[test/mjsunit/regress/regress-2711.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2711.js?cl=c65f4f7)  
  

---   

## **regress-mul-canoverflowb.js (other issue)**  
   
**[Commit: Added %NeverOptimize runtime call that can disable optimizations for a method for tests.](https://chromium.googlesource.com/v8/v8/+/9e7819f)**  
  
Date(Commit): Thu Jul 11 14:17:56 2013  
Code Review: [https://codereview.chromium.org/18214005](https://codereview.chromium.org/18214005)  
Regress: [mjsunit/regress/regress-mul-canoverflowb.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mul-canoverflowb.js)  
```javascript
function boom(a) {
  return (a | 0) * (a | 0) | 0;
};
%PrepareFunctionForOptimization(boom);
%NeverOptimizeFunction(boom_unoptimized);
function boom_unoptimized(a) {
  return (a | 0) * (a | 0) | 0;
}

boom(1, 1);
boom(2, 2);

%OptimizeFunctionOnNextCall(boom);
var big_int = 0x5F00000F;
var expected = boom_unoptimized(big_int);
var actual = boom(big_int);
assertEquals(expected, actual);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e7819f^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9e7819f)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9e7819f)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=9e7819f)  
[test/mjsunit/compiler/inline-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/inline-arguments.js?cl=9e7819f)  
[test/mjsunit/elements-kind.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/elements-kind.js?cl=9e7819f)  
...  
  
  
---   

## **regress-deopt-gcb.js (other issue)**  
   
**[Commit: Added %NeverOptimize runtime call that can disable optimizations for a method for tests.](https://chromium.googlesource.com/v8/v8/+/9e7819f)**  
  
Date(Commit): Thu Jul 11 14:17:56 2013  
Code Review: [https://codereview.chromium.org/18214005](https://codereview.chromium.org/18214005)  
Regress: [mjsunit/regress/regress-deopt-gcb.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-gcb.js)  
```javascript
(function() { var a = 10; a++; })();

function opt_me() {
  deopt();
}

%NeverOptimizeFunction(deopt);
function deopt() {
  %DeoptimizeFunction(opt_me);
  gc();
}


opt_me();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e7819f^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9e7819f)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9e7819f)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=9e7819f)  
[test/mjsunit/compiler/inline-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/inline-arguments.js?cl=9e7819f)  
[test/mjsunit/elements-kind.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/elements-kind.js?cl=9e7819f)  
...  
  
  
---   

## **regress-2758.js (v8 issue)**  
   
**[Issue 2758:
 Capability leak: Various built-in functions leak the global object](https://crbug.com/v8/2758)**  
**[Commit: Tweak error message](https://chromium.googlesource.com/v8/v8/+/929e193)**  
  
Date(Commit): Fri Jul 05 08:34:31 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/18759002](https://codereview.chromium.org/18759002)  
Regress: [mjsunit/regress/regress-2758.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2758.js)  
```javascript
var functions = [
  function() { var f = [].concat; f() },
  function() { var f = [].push; f() },
  function() { var f = [].shift; f() },
  function() { (0, [].concat)() },
  function() { (0, [].push)() },
  function() { (0, [].shift)() }
]

for (var i = 0; i < 5; ++i) {
  for (var j in functions) {
    %PrepareFunctionForOptimization(functions[j]);
    print(functions[i])
    assertThrows(functions[j], TypeError)
  }

  if (i === 3) {
    for (var j in functions)
      %OptimizeFunctionOnNextCall(functions[j]);
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/929e193^!)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=929e193)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=929e193)  
[src/runtime.js](https://cs.chromium.org/chromium/src/v8/src/runtime.js?cl=929e193)  
[test/mjsunit/bugs/bug-2758.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-2758.js?cl=929e193)  
[test/mjsunit/function-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/function-call.js?cl=929e193)  
  

---   

## **regress-embedded-cons-string.js (other issue)**  
   
**[Commit: Short-circuit embedded cons strings.](https://chromium.googlesource.com/v8/v8/+/b7b92bd)**  
  
Date(Commit): Fri Jun 21 09:24:30 2013  
Code Review: [https://codereview.chromium.org/17418003](https://codereview.chromium.org/17418003)  
Regress: [mjsunit/regress/regress-embedded-cons-string.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-embedded-cons-string.js)  
```javascript
if (!%IsConcurrentRecompilationSupported()) {
  print("Concurrent recompilation is disabled. Skipping this test.");
  quit();
}

function test(fun) {
  fun();
  fun();
  // Mark for concurrent optimization.
  %OptimizeFunctionOnNextCall(fun, "concurrent");
  // Kick off recompilation.
  fun();
  // Tenure cons string after compile graph has been created.
  gc();
  // In the mean time, concurrent recompiling is still blocked.
  assertUnoptimized(fun, "no sync");
  // Let concurrent recompilation proceed.
  %UnblockConcurrentRecompilation();
  // Concurrent recompilation eventually finishes, embeds tenured cons string.
  assertOptimized(fun, "sync");
  // Visit embedded cons string during mark compact.
  gc();
}

function f() {
  return "abcdefghijklmn" + "123456789";
}

function g() {
  return "abcdefghijklmn\u2603" + "123456789";
}

function h() {
  return "abcdefghijklmn\u2603" + "123456789\u2604";
}

test(f);
test(g);
test(h);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b7b92bd^!)  
[src/arm/assembler-arm-inl.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm-inl.h?cl=b7b92bd)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=b7b92bd)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=b7b92bd)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=b7b92bd)  
[src/ia32/assembler-ia32-inl.h](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32-inl.h?cl=b7b92bd)  
...  
  
  
---   

## **regress-polymorphic-store.js (other issue)**  
   
**[Commit: Fix using monomorphic store instruction for polymorphic stores.](https://chromium.googlesource.com/v8/v8/+/2ca5c6c)**  
  
Date(Commit): Wed Jun 19 18:07:35 2013  
Code Review: [https://chromiumcodereview.appspot.com/16875008](https://chromiumcodereview.appspot.com/16875008)  
Regress: [mjsunit/regress/regress-polymorphic-store.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-polymorphic-store.js)  
```javascript
var o1 = {};
o1.f1 = function() {
  return 10;
};
o1.x = 5;
o1.y = 2;
var o2 = {};
o2.x = 5;
o2.y = 5;

function store(o, v) {
  o.y = v;
};
%PrepareFunctionForOptimization(store);
store(o2, 0);
store(o1, 0);
store(o2, 0);
%OptimizeFunctionOnNextCall(store);
store(o1, 10);
assertEquals(5, o1.x);
assertEquals(10, o1.y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ca5c6c^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2ca5c6c)  
[test/mjsunit/regress/regress-polymorphic-store.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-polymorphic-store.js?cl=2ca5c6c)  
  
  
---   

## **regress-function-length-strict.js (other issue)**  
   
**[Commit: Fixed read-only attribute of Function.length in strict mode.](https://chromium.googlesource.com/v8/v8/+/fb7310b)**  
  
Date(Commit): Tue Jun 18 07:51:50 2013  
Code Review: [https://codereview.chromium.org/17006006](https://codereview.chromium.org/17006006)  
Regress: [mjsunit/regress/regress-function-length-strict.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-function-length-strict.js)  
```javascript
"use strict";

function foo(a, b, c) {
  return b;
}

var desc = Object.getOwnPropertyDescriptor(foo, 'length');
assertEquals(3, desc.value);
assertFalse(desc.writable);
assertFalse(desc.enumerable);
assertTrue(desc.configurable);
assertThrows(function() { foo.length = 2; }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb7310b^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=fb7310b)  
[test/mjsunit/regress/regress-function-length-strict.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-function-length-strict.js?cl=fb7310b)  
  
  
---   

## **regress-2691.js (v8 issue)**  
   
**[Issue 2691:
 yield* with value lacking a 'send' method throws "illegal access"](https://crbug.com/v8/2691)**  
**[Commit: Use keyed-call inline caches in delegating yield](https://chromium.googlesource.com/v8/v8/+/09fcac5)**  
  
Date(Commit): Thu Jun 13 10:18:28 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/15455002](https://codereview.chromium.org/15455002)  
Regress: [mjsunit/es6/regress/regress-2691.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2691.js)  
```javascript
assertThrows('(function*() { yield* 10 })().next()', TypeError);
assertThrows('(function*() { yield* {} })().next()', TypeError);
assertThrows('(function*() { yield* undefined })().next()', TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09fcac5^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=09fcac5)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=09fcac5)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=09fcac5)  
[test/mjsunit/regress/regress-2691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2691.js?cl=09fcac5)  
  

---   

## **regress-2132.js (v8 issue)**  
   
**[Issue 2132:
 Canonicalization pass removes i|0 operation too early before it can affect -0 checks](https://crbug.com/v8/2132)**  
**[Commit: Skip some conditional deopts for Div/Mul when all uses are truncating.](https://chromium.googlesource.com/v8/v8/+/9447014)**  
  
Date(Commit): Tue Jun 11 11:43:57 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/16741002](https://codereview.chromium.org/16741002)  
Regress: [mjsunit/regress/regress-2132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2132.js)  
```javascript
function mul(x, y) {
  return (x * y) | 0;
}

%PrepareFunctionForOptimization(mul);
mul(0, 0);
mul(0, 0);
%OptimizeFunctionOnNextCall(mul);
assertEquals(0, mul(0, -1));
assertOptimized(mul);

function div(x, y) {
  return (x / y) | 0;
}

%PrepareFunctionForOptimization(div);
div(4, 2);
div(4, 2);
%OptimizeFunctionOnNextCall(div);
assertEquals(1, div(5, 3));
assertOptimized(div);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9447014^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=9447014)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=9447014)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=9447014)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9447014)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=9447014)  
...  
  

---   

## **regress-2717.js (v8 issue)**  
   
**[Issue 2717:
 The language fuzzer found a crasher in object literals.](https://crbug.com/v8/2717)**  
**[Commit: Fix re-initialization of existing double field.](https://chromium.googlesource.com/v8/v8/+/ecc41e3)**  
  
Date(Commit): Mon Jun 10 11:55:47 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/16735003](https://codereview.chromium.org/16735003)  
Regress: [mjsunit/regress/regress-2717.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2717.js)  
```javascript
(function() {
  function test1(a) {
    return { x: 1.5, x: a };
  };

  assertEquals({}, test1({}).x);
})();

(function() {
  function test1(a) {
    return { y: a };
  };

  function test2(a) {
    return { y: a };
  };

  assertEquals(1.5, test1(1.5).y);
  assertEquals({}, test2({}).y);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecc41e3^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ecc41e3)  
[test/mjsunit/regress/regress-2717.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2717.js?cl=ecc41e3)  
  

---   

## **regress-int32-truncation.js (other issue)**  
   
**[Commit: Take all uses into account to clear int32 truncation.](https://chromium.googlesource.com/v8/v8/+/3588aa4)**  
  
Date(Commit): Fri Jun 07 17:28:46 2013  
Code Review: [https://chromiumcodereview.appspot.com/16656002](https://chromiumcodereview.appspot.com/16656002)  
Regress: [mjsunit/regress/regress-int32-truncation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-int32-truncation.js)  
```javascript
function f(i, b) {
  var a = 0;
  if (b) {
    var c = 1 << i;
    a = c + c;
  }
  var x = a >> 3;
  return a;
};
%PrepareFunctionForOptimization(f);
f(1, false);
f(1, true);
%OptimizeFunctionOnNextCall(f);
assertEquals((1 << 30) * 2, f(30, true));


var global = 1;

function f2(b) {
  var a = 0;
  if (b) {
    a = global;
  }
  var x = a >> 3;
  return a;
};
%PrepareFunctionForOptimization(f2);
f2(false);
f2(true);
%OptimizeFunctionOnNextCall(f2);
global = 2.5;
assertEquals(global, f2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3588aa4^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3588aa4)  
[test/mjsunit/regress/regress-int32-truncation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-int32-truncation.js?cl=3588aa4)  
  
  
---   

## **regress-crbug-244461.js (chromium issue)**  
   
**[Issue 244461:
 Chrome: Crash Report - Stack Signature: v8::internal::HOptimizedGraphBuilder::TryInlineBuiltinFunctionCall...](https://crbug.com/244461)**  
**[Commit: Special Array constructor type feedback erroneously recorded when Array](https://chromium.googlesource.com/v8/v8/+/3d3c6b1)**  
  
Date(Commit): Mon Jun 03 14:46:23 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-29", "ReleaseBlock-Dev"]  
Code Review: [https://codereview.chromium.org/16057005](https://codereview.chromium.org/16057005)  
Regress: [mjsunit/regress/regress-crbug-244461.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-244461.js)  
```javascript
function foo(arg) {
  var a = arg();
  return a;
};
%PrepareFunctionForOptimization(foo);

foo(Array);
foo(Array);
%OptimizeFunctionOnNextCall(foo);
foo(Array);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d3c6b1^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=3d3c6b1)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=3d3c6b1)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=3d3c6b1)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=3d3c6b1)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=3d3c6b1)  
...  
  

---   

## **regress-crbug-245424.js (chromium issue)**  
   
**[Issue 245424:
 Chrome: Crash Report - Stack Signature: v8::internal::FlexibleBodyVisitor<v8::internal::IncrementalMarkingMarking](https://crbug.com/245424)**  
**[Commit: Fast literals: fixed initialization of non-copied in-object property fields](https://chromium.googlesource.com/v8/v8/+/b4058a3)**  
  
Date(Commit): Fri May 31 15:50:19 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-29"]  
Code Review: [https://codereview.chromium.org/16190008](https://codereview.chromium.org/16190008)  
Regress: [mjsunit/regress/regress-crbug-245424.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-245424.js)  
```javascript
function boom() {
  var a = {foo: 'bar', foo: 'baz'};

  return a;
};
%PrepareFunctionForOptimization(boom);
assertEquals("baz", boom().foo);
assertEquals("baz", boom().foo);
%OptimizeFunctionOnNextCall(boom);
assertEquals("baz", boom().foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4058a3^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=b4058a3)  
[test/mjsunit/regress/regress-crbug-245424.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-245424.js?cl=b4058a3)  
  

---   

## **regress-crbug-243868.js (chromium issue)**  
   
**[Issue 243868:
 Chrome: Crash Report - Stack Signature: v8::internal::HOptimizedGraphBuilder::VisitLogicalExpression...](https://crbug.com/243868)**  
**[Commit: Fix IfBuilder::Deopt to clear the current block.](https://chromium.googlesource.com/v8/v8/+/3b114cd)**  
  
Date(Commit): Tue May 28 15:30:49 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["ReleaseBlock-Beta", "M-29"]  
Code Review: [https://codereview.chromium.org/16155003](https://codereview.chromium.org/16155003)  
Regress: [mjsunit/regress/regress-crbug-243868.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-243868.js)  
```javascript
var non_const_true = true;

function f(o) {
  return non_const_true && (o.val == null || false);
}

;
%PrepareFunctionForOptimization(f);
var realm = Realm.create();
var realmObject = Realm.eval(realm, 'function g() {}; var o = { val:g }; o;');

assertFalse(f(realmObject));
assertFalse(f(realmObject));

%OptimizeFunctionOnNextCall(f);
assertFalse(f(realmObject));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b114cd^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3b114cd)  
[test/mjsunit/regress/regress-crbug-243868.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-243868.js?cl=3b114cd)  
  

---   

## **regress-convert-hole.js (other issue)**  
   
**[Commit: Fix the hole loading optimization.](https://chromium.googlesource.com/v8/v8/+/aa24442)**  
  
Date(Commit): Mon May 27 17:33:14 2013  
Code Review: [https://chromiumcodereview.appspot.com/16099006](https://chromiumcodereview.appspot.com/16099006)  
Regress: [mjsunit/regress/regress-convert-hole.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-hole.js)  
```javascript
function f_store(test, test2, a, i) {
  var o = [0.5,1,,3];
  var d;
  if (test) {
    d = 1.5;
  } else {
    d = o[i];
  }
  if (test2) {
    d += 1;
  }
  a[i] = d;
  return d;
}

var a1 = [0, 0, 0, {}];
f_store(true, false, a1, 0);
f_store(true, true, a1, 0);
f_store(false, false, a1, 1);
f_store(false, true, a1, 1);
%OptimizeFunctionOnNextCall(f_store);
f_store(false, false, a1, 2);
assertEquals(undefined, a1[2]);

function test_arg(expected) {
  return function(v) {
    assertEquals(expected, v);
  }
}

function f_call(f, test, test2, i) {
  var o = [0.5,1,,3];
  var d;
  if (test) {
    d = 1.5;
  } else {
    d = o[i];
  }
  if (test2) {
    d += 1;
  }
  f(d);
  return d;
}

f_call(test_arg(1.5), true, false, 0);
f_call(test_arg(2.5), true, true, 0);
f_call(test_arg(1), false, false, 1);
f_call(test_arg(2), false, true, 1);
%OptimizeFunctionOnNextCall(f_call);
f_call(test_arg(undefined), false, false, 2);


function f_external(test, test2, test3, a, i) {
  var o = [0.5,1,,3];
  var d;
  if (test) {
    d = 1.5;
  } else {
    d = o[i];
  }
  if (test2) {
    d += 1;
  }
  if (test3) {
    d = d|0;
  }
  a[d] = 1;
  assertEquals(1, a[d]);
  return d;
}

var a2 = new Int32Array(10);
f_external(true, false, true, a2, 0);
f_external(true, true, true, a2, 0);
f_external(false, false, true, a2, 1);
f_external(false, true, true, a2, 1);
%OptimizeFunctionOnNextCall(f_external);
f_external(false, false, false, a2, 2);
assertEquals(1, a2[undefined]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa24442^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=aa24442)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=aa24442)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=aa24442)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=aa24442)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=aa24442)  
...  
  
  
---   

## **regress-2690.js (v8 issue)**  
   
**[Issue 2690:
 SyntaxError: Unterminated character class for valid regular expression](https://crbug.com/v8/2690)**  
**[Commit: Regexp parser: reset flag after scanning ahead for capture groups.](https://chromium.googlesource.com/v8/v8/+/3e41834)**  
  
Date(Commit): Mon May 27 10:53:37 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/15712006](https://chromiumcodereview.appspot.com/15712006)  
Regress: [mjsunit/regress/regress-2690.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2690.js)  
```javascript
assertTrue(/\1[a]/.test("\1a"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e41834^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=3e41834)  
[test/mjsunit/regress/regress-2690.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2690.js?cl=3e41834)  
  

---   

## **regress-237617.js (chromium issue)**  
   
**[Issue 237617:
 'use strict' causes <error: illegal access> frames in exception stacks](https://crbug.com/237617)**  
**[Commit: Fix edge case in stack trace formatting.](https://chromium.googlesource.com/v8/v8/+/7c2a134)**  
  
Date(Commit): Fri May 24 11:33:46 2013  
Components/Type: Platform>Extensions>API/Bug  
Labels: []  
Code Review: [https://chromiumcodereview.appspot.com/15670003](https://chromiumcodereview.appspot.com/15670003)  
Regress: [mjsunit/regress/regress-237617.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-237617.js)  
```javascript
"use strict"

function f() {
  throw new Error("test stack");
}

var error_stack = "";
try {
  f.call(null);
} catch (e) {
  error_stack = e.stack;
}

assertTrue(error_stack.indexOf("test stack") > 0);
assertTrue(error_stack.indexOf("illegal") < 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c2a134^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=7c2a134)  
[test/mjsunit/regress/regress-237617.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-237617.js?cl=7c2a134)  
  

---   

## **regress-crbug-240032.js (chromium issue)**  
   
**[Issue 240032:
 Security: chrome_70ee0000!v8::internal::ScavengingVisitor<1,1>::EvacuateShortcutCandidate crash](https://crbug.com/240032)**  
**[Commit: Fix embedded new-space pointer in LCmpObjectEqAndBranch.](https://chromium.googlesource.com/v8/v8/+/8fb2086)**  
  
Date(Commit): Thu May 23 14:06:28 2013  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["reward-500", "Merge-Merged", "Security_Severity-Medium", "M-28", "Security_Impact-Beta", "allpublic"]  
Code Review: [https://codereview.chromium.org/15779004](https://codereview.chromium.org/15779004)  
Regress: [mjsunit/regress/regress-crbug-240032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-240032.js)  
```javascript
function mk() {
  return function() {};
}
assertInstanceof(mk(), Function);
assertInstanceof(mk(), Function);

var o = {};
o.func = mk();

function cmp(o, f) {
  return f === o.func;
};
%PrepareFunctionForOptimization(cmp);
assertTrue(cmp(o, o.func));
assertTrue(cmp(o, o.func));
%OptimizeFunctionOnNextCall(cmp);
assertTrue(cmp(o, o.func));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8fb2086^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8fb2086)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=8fb2086)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=8fb2086)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=8fb2086)  
[src/x64/macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.cc?cl=8fb2086)  
...  
  

---   

## **regress-crbug-242924.js (chromium issue)**  
   
**[Issue 242924:
 [LangFuzz] Crash at v8::internal::HeapObject::Size() on 64 bit with invalid read](https://crbug.com/242924)**  
**[Commit: Fix bogus deopt in BuildEmitDeepCopy for holey arrays.](https://chromium.googlesource.com/v8/v8/+/b704cb9)**  
  
Date(Commit): Wed May 22 17:58:21 2013  
Components/Type: None/Bug-Security  
Labels: ["Reward-1000", "Security-Code28", "Via-Wizard", "Merge-Merged", "M-28", "Security_Severity-High", "Security_Impact-Beta", "allpublic"]  
Code Review: [https://codereview.chromium.org/15735012](https://codereview.chromium.org/15735012)  
Regress: [mjsunit/regress/regress-crbug-242924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242924.js)  
```javascript
function f() {
  return [, {}];
};
%PrepareFunctionForOptimization(f);
assertEquals([, {}], f());
assertEquals([, {}], f());
%OptimizeFunctionOnNextCall(f);
assertEquals([, {}], f());
gc();

function g() {
  return [[, 1.5], {}];
};
%PrepareFunctionForOptimization(g);
assertEquals([[, 1.5], {}], g());
assertEquals([[, 1.5], {}], g());
%OptimizeFunctionOnNextCall(g);
assertEquals([[, 1.5], {}], g());
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b704cb9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=b704cb9)  
[test/mjsunit/regress/regress-crbug-242924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242924.js?cl=b704cb9)  
  

---   

## **regress-copy-hole-to-field.js (other issue)**  
   
**[Commit: Don't allow copying holes to fields.](https://chromium.googlesource.com/v8/v8/+/b353b1d)**  
  
Date(Commit): Wed May 22 15:33:53 2013  
Code Review: [https://chromiumcodereview.appspot.com/15745006](https://chromiumcodereview.appspot.com/15745006)  
Regress: [mjsunit/regress/regress-copy-hole-to-field.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-copy-hole-to-field.js)  
```javascript
var a = [1.5, , 1.7];
var o = {a: 1.8};

function f1(o, a, i) {
  o.a = a[i];
};
%PrepareFunctionForOptimization(f1);
f1(o, a, 0);
f1(o, a, 0);
assertEquals(1.5, o.a);
%OptimizeFunctionOnNextCall(f1);
f1(o, a, 1);
assertEquals(undefined, o.a);

var a = [1, , 3];
var o = {ab: 5};

function f2(o, a, i) {
  o.ab = a[i];
};
%PrepareFunctionForOptimization(f2);
f2(o, a, 0);
f2(o, a, 0);
%OptimizeFunctionOnNextCall(f2);
f2(o, a, 1);
assertEquals(undefined, o.ab);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b353b1d^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=b353b1d)  
[test/mjsunit/regress/regress-copy-hole-to-field.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-copy-hole-to-field.js?cl=b353b1d)  
  
  
---   

## **regress-crbug-242870.js (chromium issue)**  
   
**[Issue 242870:
 UNKNOWN in v8::internal::HOptimizedGraphBuilder::VisitLogicalExpression](https://crbug.com/242870)**  
**[Commit: Fix VisitLogicalExpression for empty blocks on RHS.](https://chromium.googlesource.com/v8/v8/+/bf413b5)**  
  
Date(Commit): Wed May 22 13:27:00 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/15744002](https://codereview.chromium.org/15744002)  
Regress: [mjsunit/regress/regress-crbug-242870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242870.js)  
```javascript
var non_const_true = true;

function f() {
  return non_const_true || true && g();
};
%PrepareFunctionForOptimization(f);
function g() {
  for (;;) {}
}

assertTrue(f());
assertTrue(f());
%OptimizeFunctionOnNextCall(f);
assertTrue(f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf413b5^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=bf413b5)  
[test/mjsunit/regress/regress-crbug-242870.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242870.js?cl=bf413b5)  
  

---   

## **regress-241344.js (chromium issue)**  
   
**[Issue 241344:
 JSON.parse() produces content which crashes tab when read](https://crbug.com/241344)**  
**[Commit: Fix unexpected elements transition in JSON.parse](https://chromium.googlesource.com/v8/v8/+/9960b24)**  
  
Date(Commit): Wed May 22 13:24:18 2013  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "reward-500", "Via-Wizard", "Security_Severity-Medium", "M-28", "reward-inprocess"]  
Code Review: [https://chromiumcodereview.appspot.com/15739003](https://chromiumcodereview.appspot.com/15739003)  
Regress: [mjsunit/regress/regress-241344.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-241344.js)  
```javascript
var jsonstring = '{"0":0.1, "10000":0.4, ';
for (var i = 1; i < 9999; i++) {
  jsonstring += '"' + i + '":0.2, ';
}
jsonstring += '"9999":0.3}';

var jsonobject = JSON.parse(jsonstring);
for (var i = 1; i < 9999; i++) {
  assertEquals(0.2, jsonobject[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9960b24^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=9960b24)  
[test/mjsunit/regress/regress-241344.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-241344.js?cl=9960b24)  
  

---   

## **regress-crbug-242502.js (chromium issue)**  
   
**[Issue 242502:
 UNKNOWN in v8::internal::TypeFeedbackOracle::CanRetainOtherContext](https://crbug.com/242502)**  
**[Commit: Add regression test for fix from r14732.](https://chromium.googlesource.com/v8/v8/+/db4a770)**  
  
Date(Commit): Tue May 21 14:20:42 2013  
Components/Type: None/Bug-Security  
Labels: ["M-27", "Stability-Memory-AddressSanitizer", "Security-Code28", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "Release-1", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/15288008](https://codereview.chromium.org/15288008)  
Regress: [mjsunit/regress/regress-crbug-242502.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242502.js)  
```javascript
function f() {
  return 23;
}

function call(o) {
  return o['']();
};
%PrepareFunctionForOptimization(call);
function test() {
  var o1 = %ToFastProperties(Object.create({foo: 1}, {'': {value: f}}));
  var o2 = %ToFastProperties(Object.create({bar: 1}, {'': {value: f}}));
  var o3 = %ToFastProperties(Object.create({baz: 1}, {'': {value: f}}));
  var o4 = %ToFastProperties(Object.create({qux: 1}, {'': {value: f}}));
  var o5 = %ToFastProperties(Object.create({loo: 1}, {'': {value: f}}));
  // Called twice on o1 to turn monomorphic.
  assertEquals(23, call(o1));
  assertEquals(23, call(o1));
  // Called on four other objects to turn megamorphic.
  assertEquals(23, call(o2));
  assertEquals(23, call(o3));
  assertEquals(23, call(o4));
  assertEquals(23, call(o5));
  return o1;
}

test();

gc();

var oboom = test();

%OptimizeFunctionOnNextCall(call);
assertEquals(23, call(oboom));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db4a770^!)  
[test/mjsunit/regress/regress-crbug-242502.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242502.js?cl=db4a770)  
  

---   

## **regress-2686.js (v8 issue)**  
   
**[Issue 2686:
 Correctness of Function constructor compromised by String.indexOf monkeypatching](https://crbug.com/v8/2686)**  
**[Commit: Function constructor should avoid String.prototype methods](https://chromium.googlesource.com/v8/v8/+/d6fa1d8)**  
  
Date(Commit): Wed May 15 10:52:06 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/14668013](https://codereview.chromium.org/14668013)  
Regress: [mjsunit/regress/regress-2686.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2686.js)  
```javascript
assertThrows(function() { Function('){ function foo(', '}') }, SyntaxError);
String.prototype.indexOf = function () { return -1; }
assertThrows(function() { Function('){ function foo(', '}') }, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6fa1d8^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=d6fa1d8)  
[test/mjsunit/regress/regress-2686.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2686.js?cl=d6fa1d8)  
  

---   

## **regress-2681.js (v8 issue)**  
   
**[Issue 2681:
 Crash in generators test](https://crbug.com/v8/2681)**  
**[Commit: Don't flush code for generator functions.](https://chromium.googlesource.com/v8/v8/+/1634369)**  
  
Date(Commit): Mon May 13 17:36:26 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/14731023](https://codereview.chromium.org/14731023)  
Regress: [mjsunit/es6/regress/regress-2681.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2681.js)  
```javascript
function flush_all_code() {
  // Each GC ages code, and currently 6 gcs will flush all code.
  for (var i = 0; i < 10; i++) gc();
}

function* g() {
  yield 1;
  yield 2;
}

var o = g();
assertEquals({ value: 1, done: false }, o.next());

flush_all_code();

assertEquals({ value: 2, done: false }, o.next());
assertEquals({ value: undefined, done: true }, o.next());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1634369^!)  
[src/objects-visiting-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-visiting-inl.h?cl=1634369)  
[test/mjsunit/regress/regress-2681.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2681.js?cl=1634369)  
  

---   

## **regress-crbug-233737.js (chromium issue)**  
   
**[Issue 233737:
 JavaScript performs a calculation incorrectly](https://crbug.com/233737)**  
**[Commit: Fix missing hole check for loads from Smi arrays when all uses are changes](https://chromium.googlesource.com/v8/v8/+/7636fde)**  
  
Date(Commit): Mon May 13 11:58:10 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-29", "MovedFrom-28"]  
Code Review: [https://codereview.chromium.org/14978004](https://codereview.chromium.org/14978004)  
Regress: [mjsunit/regress/regress-crbug-233737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-233737.js)  
```javascript
var a = new Array(2);
a[0] = 1;
assertTrue(%HasSmiElements(a));
assertTrue(%HasHoleyElements(a));

function hole(i) {
  return a[i] << 0;
};
%PrepareFunctionForOptimization(hole);
assertEquals(1, hole(0));
assertEquals(1, hole(0));
%OptimizeFunctionOnNextCall(hole);
assertEquals(0, hole(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7636fde^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=7636fde)  
[test/mjsunit/regress/regress-crbug-233737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-233737.js?cl=7636fde)  
  

---   

## **regress-2671-1.js (v8 issue)**  
   
**[Issue 2671:
 Issue with optimizing array code in 64-bit](https://crbug.com/v8/2671)**  
**[Commit: Fix environment in HOptimizedGraphBuilder::VisitCountOperation. Follow-up for r14584.](https://chromium.googlesource.com/v8/v8/+/cd4e986)**  
  
Date(Commit): Wed May 08 14:58:06 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/14972009](https://chromiumcodereview.appspot.com/14972009)  
Regress: [mjsunit/regress/regress-2671-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2671-1.js), [mjsunit/regress/regress-2671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2671.js)  
```javascript
var y;
function f() {
  var a = [];
  a[20] = 0;
  y = 3;
  var i = 7 * (y + -0);
  a[i]++;
  assertTrue(isNaN(a[i]));
}
%PrepareFunctionForOptimization(f);

%PrepareFunctionForOptimization(f);
f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd4e986^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=cd4e986)  
[test/mjsunit/regress/regress-2671-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2671-1.js?cl=cd4e986)  
  

---   

## **regress-235311.js (chromium issue)**  
   
**[Issue 235311:
 [LangFuzz] Crash on heap with invalid read on dangerous (possibly uninitialized) address (64 bit)](https://crbug.com/235311)**  
**[Commit: Fix beyond-heap load on x64 Crankshafted StringCharFromCode](https://chromium.googlesource.com/v8/v8/+/528792e)**  
  
Date(Commit): Mon Apr 29 14:34:24 2013  
Components/Type: None/Bug-Security  
Labels: ["M-27", "Release-0", "reward-500", "CVE-2013-2838", "Via-Wizard", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/14387008](https://codereview.chromium.org/14387008)  
Regress: [mjsunit/regress/regress-235311.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-235311.js)  
```javascript
var new_space_string = "";
for (var i = 0; i < 12800; ++i) {
  new_space_string +=
      String.fromCharCode(Math.random() * 26 + (4294967295) | 0);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/528792e^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=528792e)  
[test/mjsunit/regress/regress-235311.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-235311.js?cl=528792e)  
  

---   

## **regress-mul-canoverflow.js (other issue)**  
   
**[Commit: Fix overflow check in mul-i which was missing since r14322](https://chromium.googlesource.com/v8/v8/+/6288754)**  
  
Date(Commit): Thu Apr 25 07:36:59 2013  
Code Review: [https://codereview.chromium.org/14471012](https://codereview.chromium.org/14471012)  
Regress: [mjsunit/regress/regress-mul-canoverflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mul-canoverflow.js)  
```javascript
function boom(a) {
  return (a | 0) * (a | 0) | 0;
};
%PrepareFunctionForOptimization(boom);
function boom_unoptimized(a) {
  try {
  } catch (_) {
  }
  return (a | 0) * (a | 0) | 0;
}

boom(1, 1);
boom(2, 2);

%OptimizeFunctionOnNextCall(boom);
var big_int = 0x5F00000F;
var expected = boom_unoptimized(big_int);
var actual = boom(big_int);
assertEquals(expected, actual);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6288754^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=6288754)  
[test/mjsunit/regress/regress-mul-canoverflow.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-mul-canoverflow.js?cl=6288754)  
  
  
---   

## **regress-2646.js (v8 issue)**  
   
**[Issue 2646:
 Float64Array: Fatal error when profiling heap with --heap_stats in spaces.cc](https://crbug.com/v8/2646)**  
**[Commit: Adds EXTERNAL_DOUBLE_ARRAY to a list of instance types](https://chromium.googlesource.com/v8/v8/+/852f903)**  
  
Date(Commit): Tue Apr 23 17:02:09 2013  
Type: ----  
Code Review: [https://codereview.chromium.org/14042008](https://codereview.chromium.org/14042008)  
Regress: [mjsunit/regress/regress-2646.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2646.js)  
```javascript
var expectedItemsCount = 10000,
    itemSize = 5,
    heap = new ArrayBuffer(expectedItemsCount * itemSize * 8),
    storage = [];

for (var i = 0; i < expectedItemsCount; i++) {
    storage.push(new Float64Array(heap, 0, itemSize));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/852f903^!)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=852f903)  
[test/mjsunit/regress/regress-2646.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2646.js?cl=852f903)  
  

---   

## **regress-234101.js (chromium issue)**  
   
**[Issue 234101:
 V8 Crash on Telemetry scroll_benchmark for docs.google.com](https://crbug.com/234101)**  
**[Commit: Do not emit double values at their use sites.](https://chromium.googlesource.com/v8/v8/+/cd34acd)**  
  
Date(Commit): Tue Apr 23 13:08:10 2013  
Components/Type: Test/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/14429002](https://codereview.chromium.org/14429002)  
Regress: [mjsunit/regress/regress-234101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-234101.js)  
```javascript
function foo(x) {
  return (x ? NaN : 0.2) + 0.1;
};
%PrepareFunctionForOptimization(foo);
foo(false);
foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd34acd^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=cd34acd)  
[test/mjsunit/regress/regress-234101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-234101.js?cl=cd34acd)  
  

---   

## **regress-grow-store-smi-check.js (other issue)**  
   
**[Commit: Fix missing Smi check in grow mode keyed stores.](https://chromium.googlesource.com/v8/v8/+/adf9afc)**  
  
Date(Commit): Thu Apr 18 14:18:27 2013  
Code Review: [https://codereview.chromium.org/14352011](https://codereview.chromium.org/14352011)  
Regress: [mjsunit/regress/regress-grow-store-smi-check.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-grow-store-smi-check.js)  
```javascript
function test(crc32) {
  for (var i = 0; i < 256; i++) {
    var c = i;
    for (var j = 0; j < 8; j++) {
      if (c & 1) {
        c = -306674912 ^ c >> 1 & 0x7fffffff;
      } else {
        c = c >> 1 & 0x7fffffff;
      }
    }
    crc32[i] = c;
  }
};
%PrepareFunctionForOptimization(test);
var a = [0.5];
for (var i = 0; i < 256; ++i) a[i] = i;

test([0.5]);
test(a);
%OptimizeFunctionOnNextCall(test);
test(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/adf9afc^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=adf9afc)  
[test/mjsunit/regress/regress-grow-store-smi-check.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-grow-store-smi-check.js?cl=adf9afc)  
  
  
---   

## **regress-2624.js (v8 issue)**  
   
**[Issue 2624:
 running the test suite with --print-all-code causes crashes in debug mode](https://crbug.com/v8/2624)**  
**[Commit: Fix OOB write in --print-code.](https://chromium.googlesource.com/v8/v8/+/d7b78dc)**  
  
Date(Commit): Mon Apr 15 15:19:51 2013  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/14018010](https://chromiumcodereview.appspot.com/14018010)  
Regress: [mjsunit/regress/regress-2624.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2624.js)  
```javascript
var source = '"snowmen invasion " + "';
for(var i = 0; i < 800; i++) {
  source += '\u2603';
}
source += '"';
eval(source);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d7b78dc^!)  
[src/disassembler.cc](https://cs.chromium.org/chromium/src/v8/src/disassembler.cc?cl=d7b78dc)  
[src/utils.cc](https://cs.chromium.org/chromium/src/v8/src/utils.cc?cl=d7b78dc)  
[src/v8utils.cc](https://cs.chromium.org/chromium/src/v8/src/v8utils.cc?cl=d7b78dc)  
[test/mjsunit/regress/regress-2624.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2624.js?cl=d7b78dc)  
  

---   

## **regress-crbug-229923.js (chromium issue)**  
   
**[Issue 229923:
 Crash while loading pages due to V8 roll](https://crbug.com/229923)**  
**[Commit: Fix JSON.stringify's slow path wrt sliced strings.](https://chromium.googlesource.com/v8/v8/+/da5c11a)**  
  
Date(Commit): Thu Apr 11 09:53:00 2013  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Performance", "ReleaseBlock-Dev", "M-28", "TE-Verified-28.0.1474.0"]  
Code Review: [https://chromiumcodereview.appspot.com/14107004](https://chromiumcodereview.appspot.com/14107004)  
Regress: [mjsunit/regress/regress-crbug-229923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-229923.js)  
```javascript
var slice = "slow path of JSON.stringify for sliced string".substring(1);
assertEquals('"' + slice + '"', JSON.stringify(slice, null, 0));

var parent = "external string turned into two byte";
var slice_of_external = parent.substring(1);
try {
  // Turn the string to a two-byte external string, so that the sliced
  // string looks like one-byte, but its parent is actually two-byte.
  externalizeString(parent, true);
} catch (e) { }
assertEquals('"' + slice_of_external + '"',
             JSON.stringify(slice_of_external, null, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da5c11a^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=da5c11a)  
[test/mjsunit/regress/regress-crbug-229923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-229923.js?cl=da5c11a)  
  

---   

## **regress-2618.js (v8 issue)**  
   
**[Issue 2618:
 Optimize dual loop](https://crbug.com/v8/2618)**  
**[Commit: Fix OSR for nested loops.](https://chromium.googlesource.com/v8/v8/+/996a80d)**  
  
Date(Commit): Wed Apr 10 09:24:31 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/13811014](https://chromiumcodereview.appspot.com/13811014)  
Regress: [mjsunit/regress/regress-2618.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2618.js)  
```javascript
if (isNeverOptimizeLiteMode()) {
  print("Warning: skipping test that requires optimization in Lite mode.");
  testRunner.quit(0);
}
assertFalse(isAlwaysOptimize());

function f() {
  do {
    do {
      for (var i = 0; i < 10; i++) %OptimizeOsr();
      // Note: this check can't be wrapped in a function, because
      // calling that function causes a deopt from lack of call
      // feedback.
      var opt_status = %GetOptimizationStatus(f);
      assertTrue(
        (opt_status & V8OptimizationStatus.kMaybeDeopted) !== 0 ||
        (opt_status & V8OptimizationStatus.kTopmostFrameIsTurboFanned) !== 0);
    } while (false);
  } while (false);
}

%PrepareFunctionForOptimization(f);
f();

function g() {
  for (var i = 0; i < 1; i++) { }

  do {
    do {
      for (var i = 0; i < 1; i++) { }
    } while (false);
  } while (false);

  do {
    do {
      do {
        do {
          do {
            do {
              do {
                do {
                  for (var i = 0; i < 10; i++) %OptimizeOsr();
                  var opt_status = %GetOptimizationStatus(g);
                  assertTrue(
                    (opt_status & V8OptimizationStatus.kMaybeDeopted) !== 0 ||
                    (opt_status &
                        V8OptimizationStatus.kTopmostFrameIsTurboFanned) !== 0);
                } while (false);
              } while (false);
            } while (false);
          } while (false);
        } while (false);
      } while (false);
    } while (false);
  } while (false);
}

%PrepareFunctionForOptimization(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/996a80d^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=996a80d)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=996a80d)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=996a80d)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=996a80d)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=996a80d)  
...  
  

---   

## **regress-2595.js (v8 issue)**  
   
**[Issue 2595:
 Wrong inlining target](https://crbug.com/v8/2595)**  
**[Commit: Let ComputeTarget fail if it skips over NORMAL objects.](https://chromium.googlesource.com/v8/v8/+/79d18ea)**  
  
Date(Commit): Tue Apr 09 16:38:51 2013  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/13862008](https://chromiumcodereview.appspot.com/13862008)  
Regress: [mjsunit/regress/regress-2595.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2595.js)  
```javascript
var p = {
  f: function() {
    return 'p';
  }
};
var o = Object.create(p);
o.x = true;
delete o.x;  // slow case object

var u = {
  x: 0,
  f: function() {
    return 'u';
  }
};  // object with some other map

function F(x) {
  return x.f();
}

;
%PrepareFunctionForOptimization(F);
assertEquals('p', F(o));
assertEquals('p', F(o));
assertEquals("u", F(u));
assertEquals("p", F(o));
assertEquals("u", F(u));

%OptimizeFunctionOnNextCall(F);
assertEquals("p", F(o));

o.f = function() {
  return 'o';
};
assertEquals("o", F(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79d18ea^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=79d18ea)  
[test/mjsunit/regress/regress-2595.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2595.js?cl=79d18ea)  
  

---   

## **regress-2612.js (v8 issue)**  
   
**[Issue 2612:
 Memory black hole for a loop above a certain number of iterations](https://crbug.com/v8/2612)**  
**[Commit: Fix worst-case behavior of MergeRemovableSimulates().](https://chromium.googlesource.com/v8/v8/+/9559181)**  
  
Date(Commit): Mon Apr 08 17:37:22 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/13649003](https://chromiumcodereview.appspot.com/13649003)  
Regress: [mjsunit/regress/regress-2612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2612.js)  
```javascript
var seed = 1;

function rand() {
  seed = seed * 171 % 1337 + 17;
  return (seed % 1000) / 1000;
}

function randi(max) {
  seed = seed * 131 % 1773 + 13;
  return seed % max;
}

function varname(i) {
  return "_" + i;
}

var source = "var ";

for (var i = 0; i < 750; i++) {
  source += [varname(i), "=", rand(), ","].join("");
}

for (var i = 750; i < 3000; i++) {
  source += [varname(i), "=",
             varname(randi(i)), "+",
             varname(randi(i)), ","].join("");
}

source += "x=1; return _0;"
var f = new Function(source);
%PrepareFunctionForOptimization(f);

%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9559181^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=9559181)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=9559181)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9559181)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=9559181)  
[test/mjsunit/regress/regress-2612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2612.js?cl=9559181)  
  

---   

## **regress-581.js (v8 issue)**  
   
**[Issue 581:
 Concatenating arrays with total length over 2^32-1 doesn't work correctly.](https://crbug.com/v8/581)**  
**[Commit: Fix Array.prototype.concat when exceeding array size limit.](https://chromium.googlesource.com/v8/v8/+/e33b688)**  
  
Date(Commit): Fri Apr 05 15:12:59 2013  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/13465008](https://chromiumcodereview.appspot.com/13465008)  
Regress: [mjsunit/regress/regress-581.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-581.js)  
```javascript
var pow30 = Math.pow(2, 30);
var pow31 = Math.pow(2, 31);

var a = [];
a[pow31] = 31;

assertEquals(pow31 + 1, a.length);
assertThrows(function() { a.concat(a); }, RangeError);

var b = [];
b[pow31 - 3] = 32;
var ab = a.concat(b);
assertEquals(2 * pow31 - 1, ab.length);
assertEquals(31, ab[pow31]);
assertEquals(32, ab[2 * pow31 - 2]);
assertEquals(undefined, ab[2 * pow31 - 1]);

var c = [];
c[pow30] = 30;
assertThrows(function() { c.concat(c, a); }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e33b688^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=e33b688)  
[test/mjsunit/regress/regress-581.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-581.js?cl=e33b688)  
  

---   

## **regress-2273.js (v8 issue)**  
   
**[Issue 2273:
 Built-in functions should not coerce 'this' value in a strict mode function](https://crbug.com/v8/2273)**  
**[Commit: Do not implicitly convert non-object receivers for strict mode functions.](https://chromium.googlesource.com/v8/v8/+/deecbb2)**  
  
Date(Commit): Fri Apr 05 11:57:02 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/13473009](https://chromiumcodereview.appspot.com/13473009)  
Regress: [mjsunit/regress/regress-2273.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2273.js)  
```javascript
var CheckStringReceiver = function() {
  "use strict";
  // Receivers of strict functions are not coerced.
  assertEquals("string", typeof this);
};

var CheckNumberReceiver = function() {
  "use strict";
  // Receivers of strict functions are not coerced.
  assertEquals("number", typeof this);
};

var CheckUndefinedReceiver = function() {
  "use strict";
  // Receivers of strict functions are not coerced.
  assertEquals("undefined", String(this));
};

var CheckNullReceiver = function() {
  "use strict";
  // Receivers of strict functions are not coerced.
  assertEquals("null", String(this));
};

var CheckCoersion = function() {
  // Receivers of non-strict functions are coerced to objects.
  assertEquals("object", typeof this);
};


function strict_mode() {
  "use strict";
  CheckStringReceiver.call("foo");
  CheckNumberReceiver.call(42);
  CheckUndefinedReceiver.call(undefined);
  CheckNullReceiver.call(null);
  [1].forEach(CheckStringReceiver, "foo");
  [2].every(CheckStringReceiver, "foo");
  [3].filter(CheckStringReceiver, "foo");
  [4].some(CheckNumberReceiver, 42);
  [5].map(CheckNumberReceiver, 42);

  CheckCoersion.call("foo");
  CheckCoersion.call(42);
  CheckCoersion.call(undefined);
  CheckCoersion.call(null);
  [1].forEach(CheckCoersion, "foo");
  [2].every(CheckCoersion, "foo");
  [3].filter(CheckCoersion, "foo");
  [4].some(CheckCoersion, 42);
  [5].map(CheckCoersion, 42);
};
strict_mode();

function sloppy_mode() {
  CheckStringReceiver.call("foo");
  CheckNumberReceiver.call(42);
  CheckUndefinedReceiver.call(undefined);
  CheckNullReceiver.call(null);
  [1].forEach(CheckStringReceiver, "foo");
  [2].every(CheckStringReceiver, "foo");
  [3].filter(CheckStringReceiver, "foo");
  [4].some(CheckNumberReceiver, 42);
  [5].map(CheckNumberReceiver, 42);

  CheckCoersion.call("foo");
  CheckCoersion.call(42);
  CheckCoersion.call(undefined);
  CheckCoersion.call(null);
  [1].forEach(CheckCoersion, "foo");
  [2].every(CheckCoersion, "foo");
  [3].filter(CheckCoersion, "foo");
  [4].some(CheckCoersion, 42);
  [5].map(CheckCoersion, 42);
};
sloppy_mode();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/deecbb2^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=deecbb2)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=deecbb2)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=deecbb2)  
[test/mjsunit/regress/regress-2273.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2273.js?cl=deecbb2)  
  

---   

## **regress-2606.js (v8 issue)**  
   
**[Issue 2606:
 Unable to change writability and configurability of __proto__](https://crbug.com/v8/2606)**  
**[Commit: Make __proto__ a real JavaScript accessor property.](https://chromium.googlesource.com/v8/v8/+/9e757a6)**  
  
Date(Commit): Thu Apr 04 12:10:23 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/13533004](https://codereview.chromium.org/13533004)  
Regress: [mjsunit/regress/regress-2606.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2606.js)  
```javascript
var desc = Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
assertFalse(desc.enumerable);
assertTrue(desc.configurable);
assertEquals("function", typeof desc.get);
assertEquals("function", typeof desc.set);

function replaced_get() {};
Object.defineProperty(Object.prototype, "__proto__", { get:replaced_get });
desc = Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
assertFalse(desc.enumerable);
assertTrue(desc.configurable);
assertSame(replaced_get, desc.get);

function replaced_set(x) {};
Object.defineProperty(Object.prototype, "__proto__", { set:replaced_set });
desc = Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
assertFalse(desc.enumerable);
assertTrue(desc.configurable);
assertSame(replaced_set, desc.set);

Object.defineProperty(Object.prototype, "__proto__", { configurable:false });
desc = Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
assertFalse(desc.enumerable);
assertFalse(desc.configurable);
assertSame(replaced_get, desc.get);
assertSame(replaced_set, desc.set);

Object.freeze(Object.prototype);
assertTrue(Object.isFrozen(Object.prototype));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e757a6^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=9e757a6)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=9e757a6)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=9e757a6)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=9e757a6)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=9e757a6)  
...  
  

---   

## **regress-2593.js (v8 issue)**  
   
**[Issue 2593:
 Object.create and Object.hasOwnProperty optimizations](https://crbug.com/v8/2593)**  
**[Commit: Add extra flag for load-ic stubs in code cache.](https://chromium.googlesource.com/v8/v8/+/eee5884)**  
  
Date(Commit): Thu Apr 04 08:29:25 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/13552003](https://chromiumcodereview.appspot.com/13552003)  
Regress: [mjsunit/regress/regress-2593.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2593.js)  
```javascript
p1 =  { };
p2 =  { };
p3 =  { x : 1 };
p2.__proto__ = p3
p1.__proto__ = p2

p1.z = 1
delete p1.z

for (var i = 0; i < 10; i++) gc();

function f2() {
  p2.x;
}

function f1() {
  return p1.x;
}

for (var i = 0; i < 10; i++) f2();

for (var i = 0; i < 10; i++) f1();

assertEquals(1, f1());

p2.x = 2;

assertEquals(2, f1());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eee5884^!)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=eee5884)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=eee5884)  
[src/stub-cache.h](https://cs.chromium.org/chromium/src/v8/src/stub-cache.h?cl=eee5884)  
[test/mjsunit/regress/regress-2593.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2593.js?cl=eee5884)  
  

---   

## **regress-201590.js (chromium issue)**  
   
**[Issue 201590:
 JavaScript function interpreted incorrectly](https://crbug.com/201590)**  
**[Commit: Ensure UseRegisterAtStart not used with fixed temp/return register](https://chromium.googlesource.com/v8/v8/+/98281c6)**  
  
Date(Commit): Wed Apr 03 14:45:39 2013  
Components/Type: Blink/Bug-Regression  
Labels: ["M-27", "Via-Wizard", "Arch-x86", "ReleaseBlock-Stable"]  
Code Review: [https://codereview.chromium.org/13527007](https://codereview.chromium.org/13527007)  
Regress: [mjsunit/regress/regress-201590.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-201590.js)  
```javascript
var gdpRatio = 16/9;

function Foo(initialX, initialY, initialScale, initialMapHeight) {
  this.ORIGIN = { x: initialX, y: initialY };
  this.scale = initialScale;
  this.mapHeight = initialMapHeight;
}

Foo.prototype.bar = function (x, y, xOffset, yOffset) {
  var tileHeight =  64 * 0.25 * this.scale,
  tileWidth  = 128 * 0.25 * this.scale,
  xOffset    = xOffset * 0.5 || 0,
  yOffset    = yOffset * 0.5 || 0;
  var xPos = ((xOffset) * gdpRatio) + this.ORIGIN.x * this.scale -
      ((y * tileWidth ) * gdpRatio) + ((x * tileWidth ) * gdpRatio);
  var yPos = ((yOffset) * gdpRatio) + this.ORIGIN.y * this.scale +
      ((y * tileHeight) * gdpRatio) + ((x * tileHeight) * gdpRatio);
  xPos = xPos - Math.round(((tileWidth) * gdpRatio)) +
      this.mapHeight * Math.round(((tileWidth) * gdpRatio));
  return {
      x: Math.floor(xPos),
      y: Math.floor(yPos)
  };
}

var f = new Foo(10, 20, 2.5, 400);

function baz() {
  var b = f.bar(1.1, 2.2, 3.3, 4.4);
  assertEquals(56529, b.x);
  assertEquals(288, b.y);
}

%PrepareFunctionForOptimization(Foo.prototype.bar);
baz();
baz();
%OptimizeFunctionOnNextCall(Foo.prototype.bar);
baz();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/98281c6^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=98281c6)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=98281c6)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=98281c6)  
[test/mjsunit/regress/regress-201590.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-201590.js?cl=98281c6)  
  

---   

## **regress-105.js (v8 issue)**  
   
**[Issue 105:
 Implicit call to valueOf has wrong caller](https://crbug.com/v8/105)**  
**[Commit: Add test to check that Function.caller must not expose native functions.](https://chromium.googlesource.com/v8/v8/+/443f85e)**  
  
Date(Commit): Thu Mar 28 14:31:48 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/13166002](https://chromiumcodereview.appspot.com/13166002)  
Regress: [mjsunit/regress/regress-105.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-105.js)  
```javascript
var custom_valueOf = function() {
  assertEquals(null, custom_valueOf.caller);
  return 2;
}

var custom_toString = function() {
  assertEquals(null, custom_toString.caller);
  return "I used to be an adventurer like you";
}

var object = {};
object.valueOf = custom_valueOf;
object.toString = custom_toString;

assertEquals(2, Number(object));
assertEquals('I', String(object)[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/443f85e^!)  
[test/mjsunit/regress/regress-105.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-105.js?cl=443f85e)  
  

---   

## **regress-2596.js (v8 issue)**  
   
**[Issue 2596:
 V8 is leaking holes](https://crbug.com/v8/2596)**  
**[Commit: Canonicalize NaNs on store to Fast(Float|Double) arrays](https://chromium.googlesource.com/v8/v8/+/47d8af7)**  
  
Date(Commit): Thu Mar 28 13:30:16 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12918028](https://codereview.chromium.org/12918028)  
Regress: [mjsunit/regress/regress-2596.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2596.js)  
```javascript
var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper();
i_view[1] = %GetHoleNaNLower();
var doubles = new Float64Array(ab);  // kHoleNaN
assertTrue(isNaN(doubles[0]));

var prototype = [2.5, 2.5];
var array = [3.5, 3.5];
array.__proto__ = prototype;
assertTrue(%HasDoubleElements(array));

function boom(index) {
  array[index] = doubles[0];
  return array[index];
};
%PrepareFunctionForOptimization(boom);
assertTrue(isNaN(boom(0)));
assertTrue(isNaN(boom(0)));
assertTrue(isNaN(boom(0)));

%OptimizeFunctionOnNextCall(boom);
assertTrue(isNaN(boom(0)));
assertTrue(isNaN(boom(0)));
assertTrue(isNaN(boom(0)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/47d8af7^!)  
[src/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/elements-kind.h?cl=47d8af7)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=47d8af7)  
[test/mjsunit/regress/regress-2596.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2596.js?cl=47d8af7)  
  

---   

## **regress-crbug-217858.js (chromium issue)**  
   
**[Issue 217858:
 [LangFuzz] Crash on Heap with invalid read (possibly due to uninitialized value) on 64 bit](https://crbug.com/217858)**  
**[Commit: Add test case for missing deopt sequence after forced deopt.](https://chromium.googlesource.com/v8/v8/+/a942fcd)**  
  
Date(Commit): Wed Mar 27 09:58:32 2013  
Components/Type: Blink/Bug-Security  
Labels: ["M-27", "Reward-1000", "Via-Wizard", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/13042005](https://chromiumcodereview.appspot.com/13042005)  
Regress: [mjsunit/regress/regress-crbug-217858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-217858.js)  
```javascript
var r = /r/;
function f() {
  r[r] = function() {};
}

for (var i = 0; i < 300; i++) {
  f();
  if (i == 150) %OptimizeOsr();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a942fcd^!)  
[test/mjsunit/regress/regress-crbug-217858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-217858.js?cl=a942fcd)  
  

---   

## **regress-crbug-181422.js (chromium issue)**  
   
**[Issue 181422:
 Special character \s in RegExp does not match \u00a0 (NBSP)](https://crbug.com/181422)**  
**[Commit: Fix white space matching in latin-1 strings wrt \u00a0.](https://chromium.googlesource.com/v8/v8/+/b85237a)**  
  
Date(Commit): Mon Mar 11 11:52:11 2013  
Components/Type: Blink/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://chromiumcodereview.appspot.com/12644008](https://chromiumcodereview.appspot.com/12644008)  
Regress: [mjsunit/regress/regress-crbug-181422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-181422.js)  
```javascript
assertArrayEquals(["\u00a0"], "ab\u00a0cd".match(/\s/));
assertArrayEquals(["a", "b", "c", "d"], "ab\u00a0cd".match(/\S/g));

assertArrayEquals(["\u00a0"], "\u2604b\u00a0cd".match(/\s/));
assertArrayEquals(["\u2604", "b", "c", "d"], "\u2604b\u00a0cd".match(/\S/g));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b85237a^!)  
[src/arm/regexp-macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/regexp-macro-assembler-arm.cc?cl=b85237a)  
[src/ia32/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/regexp-macro-assembler-ia32.cc?cl=b85237a)  
[src/x64/regexp-macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/regexp-macro-assembler-x64.cc?cl=b85237a)  
[test/mjsunit/regress/regress-crbug-181422.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-181422.js?cl=b85237a)  
  

---   

## **regress-177883.js (chromium issue)**  
   
**[Issue 177883:
 Hard hang in Chrome with specific emscripten-compiled app, Firefox / Safari works fine](https://crbug.com/177883)**  
**[Commit: Fixed register allocation corner case.](https://chromium.googlesource.com/v8/v8/+/e44d3b7)**  
  
Date(Commit): Mon Mar 11 09:49:00 2013  
Components/Type: Blink/Bug  
Labels: ["Performance", "Via-Wizard", "Arch-x86"]  
Code Review: [https://codereview.chromium.org/12631012](https://codereview.chromium.org/12631012)  
Regress: [mjsunit/compiler/regress-177883.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-177883.js)  
```javascript
(function () {
  function __ZNK4Math5plane3dotERKNS_6float4E(i1, i2) {
    i1 = i1 | 0;
    i2 = i2 | 0;
    return +(+HEAPF32[i1 >> 2] * +HEAPF32[i2 >> 2] + +HEAPF32[i1 + 4 >> 2] * +HEAPF32[i2 + 4 >> 2] + +HEAPF32[i1 + 8 >> 2] * +HEAPF32[i2 + 8 >> 2] + +HEAPF32[i1 + 12 >> 2] * +HEAPF32[i2 + 12 >> 2]);
  }

  function __ZNK4Math7frustum8clipmaskERKNS_5pointE(i1, i2) {
    i1 = i1 | 0;
    i2 = i2 | 0;
    var i3 = 0, i4 = 0;
    i3 = i2 | 0;
    i2 = +__ZNK4Math5plane3dotERKNS_6float4E(i1 | 0, i3) > 0.0 & 1;
    i4 = +__ZNK4Math5plane3dotERKNS_6float4E(i1 + 16 | 0, i3) > 0.0 ? i2 | 2 : i2;
    i2 = +__ZNK4Math5plane3dotERKNS_6float4E(i1 + 32 | 0, i3) > 0.0 ? i4 | 4 : i4;
    i4 = +__ZNK4Math5plane3dotERKNS_6float4E(i1 + 48 | 0, i3) > 0.0 ? i2 | 8 : i2;
    i2 = +__ZNK4Math5plane3dotERKNS_6float4E(i1 + 64 | 0, i3) > 0.0 ? i4 | 16 : i4;
    return (+__ZNK4Math5plane3dotERKNS_6float4E(i1 + 80 | 0, i3) > 0.0 ? i2 | 32 : i2) | 0;
  }

  function __ZNK4Math7frustum10clipstatusERKNS_4bboxE(i1, i2) {
    i1 = i1 | 0;
    i2 = i2 | 0;
    var i3 = 0, i4 = 0, i5 = 0, i6 = 0, i7 = 0, i8 = 0, i9 = 0, i10 = 0, i11 = 0, i12 = 0, i13 = 0, i14 = 0, i15 = 0, i16 = 0, d17 = 0.0, d18 = 0.0, i19 = 0, i20 = 0, i21 = 0, i22 = 0;
    i3 = STACKTOP;
    STACKTOP = STACKTOP + 16 | 0;
    i4 = i3 | 0;
    i5 = i4 | 0;
    HEAPF32[i5 >> 2] = 0.0;
    i6 = i4 + 4 | 0;
    HEAPF32[i6 >> 2] = 0.0;
    i7 = i4 + 8 | 0;
    HEAPF32[i7 >> 2] = 0.0;
    i8 = i4 + 12 | 0;
    HEAPF32[i8 >> 2] = 1.0;
    i9 = i2 | 0;
    i10 = i2 + 4 | 0;
    i11 = i2 + 8 | 0;
    i12 = i2 + 20 | 0;
    i13 = i2 + 16 | 0;
    i14 = i2 + 24 | 0;
    i2 = 65535;
    i15 = 0;
    i16 = 0;
    while (1) {
      if ((i16 | 0) == 1) {
        d17 = +HEAPF32[i12 >> 2];
        d18 = +HEAPF32[i11 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i9 >> 2];
        HEAPF32[i6 >> 2] = d17;
        HEAPF32[i7 >> 2] = d18;
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 4) {
        HEAPF32[i5 >> 2] = +HEAPF32[i13 >> 2];
        HEAPF32[i6 >> 2] = +HEAPF32[i12 >> 2];
        HEAPF32[i7 >> 2] = +HEAPF32[i14 >> 2];
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 6) {
        d18 = +HEAPF32[i10 >> 2];
        d17 = +HEAPF32[i14 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i9 >> 2];
        HEAPF32[i6 >> 2] = d18;
        HEAPF32[i7 >> 2] = d17;
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 5) {
        d17 = +HEAPF32[i12 >> 2];
        d18 = +HEAPF32[i14 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i9 >> 2];
        HEAPF32[i6 >> 2] = d17;
        HEAPF32[i7 >> 2] = d18;
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 3) {
        d18 = +HEAPF32[i10 >> 2];
        d17 = +HEAPF32[i11 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i13 >> 2];
        HEAPF32[i6 >> 2] = d18;
        HEAPF32[i7 >> 2] = d17;
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 0) {
        HEAPF32[i5 >> 2] = +HEAPF32[i9 >> 2];
        HEAPF32[i6 >> 2] = +HEAPF32[i10 >> 2];
        HEAPF32[i7 >> 2] = +HEAPF32[i11 >> 2];
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 2) {
        d17 = +HEAPF32[i12 >> 2];
        d18 = +HEAPF32[i11 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i13 >> 2];
        HEAPF32[i6 >> 2] = d17;
        HEAPF32[i7 >> 2] = d18;
        HEAPF32[i8 >> 2] = 1.0;
      } else if ((i16 | 0) == 7) {
        d18 = +HEAPF32[i10 >> 2];
        d17 = +HEAPF32[i14 >> 2];
        HEAPF32[i5 >> 2] = +HEAPF32[i13 >> 2];
        HEAPF32[i6 >> 2] = d18;
        HEAPF32[i7 >> 2] = d17;
        HEAPF32[i8 >> 2] = 1.0;
      }
      i19 = __ZNK4Math7frustum8clipmaskERKNS_5pointE(i1, i4) | 0;
      i20 = i19 & i2;
      i21 = i19 | i15;
      i19 = i16 + 1 | 0;
      if ((i19 | 0) == 8) {
        break;
      } else {
        i2 = i20;
        i15 = i21;
        i16 = i19;
      }
    }
    if ((i21 | 0) == 0) {
      i22 = 0;
      STACKTOP = i3;
      return i22 | 0;
    }
    i22 = (i20 | 0) == 0 ? 2 : 1;
    STACKTOP = i3;
    return i22 | 0;
  }

  // -----------------------------------------------------------------

  var STACKTOP = 0;
  var HEAPF32 = new Float32Array(1000);

  for (var i = 0; i < HEAPF32.length; i++) {
    HEAPF32[i] = 1.0;
  }

  %PrepareFunctionForOptimization(__ZNK4Math7frustum10clipstatusERKNS_4bboxE);
  __ZNK4Math7frustum10clipstatusERKNS_4bboxE(0, 0);
  __ZNK4Math7frustum10clipstatusERKNS_4bboxE(0, 0);
  __ZNK4Math7frustum10clipstatusERKNS_4bboxE(0, 0);
  %OptimizeFunctionOnNextCall(__ZNK4Math7frustum10clipstatusERKNS_4bboxE);
  __ZNK4Math7frustum10clipstatusERKNS_4bboxE(0, 0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e44d3b7^!)  
[src/lithium-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.cc?cl=e44d3b7)  
[test/mjsunit/compiler/regress-177883.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-177883.js?cl=e44d3b7)  
  

---   

## **regress-2470.js (v8 issue)**  
   
**[Issue 2470:
 Function constructor call allows invalid FormalParameterList/FunctionBody](https://crbug.com/v8/2470)**  
**[Commit: Harden Function()'s parsing of function literals.](https://chromium.googlesource.com/v8/v8/+/4b0395c)**  
  
Date(Commit): Thu Mar 07 15:46:14 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12613007](https://codereview.chromium.org/12613007)  
Regress: [mjsunit/regress/regress-2470.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2470.js)  
```javascript
assertThrows('Function("/*", "*/){");', SyntaxError);

assertThrows('Function("});(function(){");', SyntaxError);

assertDoesNotThrow('Function("/*", "*/", "/**/");');
assertDoesNotThrow('Function("/*", "a", "*/", "/**/");');
assertDoesNotThrow('Function("a", "/*", "*/", "/**/");');
assertThrows('Function("a", "/*", "*/", "b", "/*", "*/", "/**/");', SyntaxError);

assertDoesNotThrow('Function("//", "//")');
assertDoesNotThrow('Function("//", "//", "//")');
assertDoesNotThrow('Function("a", "//", "//")');
assertThrows('Function("a", "", "//", "//")', SyntaxError);

var asString = Function("return 23").toString();
assertSame("function anonymous(\n) {\nreturn 23\n}", asString);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b0395c^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=4b0395c)  
[src/compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler.h?cl=4b0395c)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=4b0395c)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4b0395c)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=4b0395c)  
...  
  

---   

## **regress-2570.js (v8 issue)**  
   
**[Issue 2570:
 JSON.stringify encoding issue](https://crbug.com/v8/2570)**  
**[Commit: Insert missing type cast in JSON.stringify.](https://chromium.googlesource.com/v8/v8/+/3a497df)**  
  
Date(Commit): Thu Mar 07 09:58:27 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/12599003](https://chromiumcodereview.appspot.com/12599003)  
Regress: [mjsunit/regress/regress-2570.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2570.js)  
```javascript
var o = ["\u56e7",   // Switch JSON stringifier to two-byte mode.
         "\u00e6"];  // Latin-1 character.

assertEquals('["\u56e7","\u00e6"]', JSON.stringify(o));
assertEquals('["\u56e7","\u00e6"]', JSON.stringify(o, null, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3a497df^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=3a497df)  
[test/mjsunit/regress/regress-2570.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2570.js?cl=3a497df)  
  

---   

## **regress-2568.js (v8 issue)**  
   
**[Issue 2568:
 Array.prototype.map plucking out "length" causes function to stick](https://crbug.com/v8/2568)**  
**[Commit: Fix Array.length, String.length and Function.prototype LoadICs on x64.](https://chromium.googlesource.com/v8/v8/+/a62cfd1)**  
  
Date(Commit): Wed Mar 06 18:19:35 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/12545004](https://chromiumcodereview.appspot.com/12545004)  
Regress: [mjsunit/regress/regress-2568.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2568.js)  
```javascript
function pluck1(a, key) {
    return a.map(function(item) { return item[key]; });
};
assertArrayEquals([2, 2], pluck1([[0, 0], [0, 0]], 'length'));
assertArrayEquals([1, 3], pluck1([[1, 2], [3, 4]], '0'));

function pluck2(a, key) {
    return a.map(function(item) { return item[key]; });
};
assertArrayEquals([2, 2], pluck2(["ab", "cd"], 'length'));
assertArrayEquals(["a", "c"], pluck2(["ab", "cd"], '0'));

function pluck3(a, key) {
    return a.map(function(item) { return item[key]; });
};
f = function() { 1 };
f.prototype = g = function() { 2 };
assertArrayEquals([g, g], pluck3([f, f], 'prototype'));
assertArrayEquals([undefined, undefined], pluck3([f, f], '0'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a62cfd1^!)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=a62cfd1)  
[test/mjsunit/regress/regress-2568.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2568.js?cl=a62cfd1)  
  

---   

## **regress-2566.js (v8 issue)**  
   
**[Issue 2566:
 Incorrect compilation of property setting for arrays when property is 'length'](https://crbug.com/v8/2566)**  
**[Commit: Properly handle misses for StoreArrayLengthStub on ia32 and x64](https://chromium.googlesource.com/v8/v8/+/7fe9bd5)**  
  
Date(Commit): Tue Mar 05 16:31:11 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12378085](https://codereview.chromium.org/12378085)  
Regress: [mjsunit/regress/regress-2566.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2566.js)  
```javascript
function setProp(obj, prop, val) {
  obj[prop] = val;
}
var obj = [];
setProp(obj, 'length', 1);
setProp(obj, 0, 5);
assertEquals(1, obj.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7fe9bd5^!)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=7fe9bd5)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=7fe9bd5)  
[test/mjsunit/regress/regress-2566.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2566.js?cl=7fe9bd5)  
  

---   

## **regress-2565.js (v8 issue)**  
   
**[Issue 2565:
 Changes to __proto__ broke Object.create](https://crbug.com/v8/2565)**  
**[Commit: Add workaround for redefinition of __proto__ property.](https://chromium.googlesource.com/v8/v8/+/2aabf62)**  
  
Date(Commit): Mon Mar 04 17:53:40 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12398010](https://codereview.chromium.org/12398010)  
Regress: [mjsunit/regress/regress-2565.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2565.js)  
```javascript
Object.freeze(Object.prototype);
var p = {};
var o = Object.create(p);
assertSame(p, o.__proto__);
assertSame(p, Object.getPrototypeOf(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2aabf62^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=2aabf62)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=2aabf62)  
[test/mjsunit/regress/regress-2565.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2565.js?cl=2aabf62)  
  

---   

## **regress-crbug-178790.js (chromium issue)**  
   
**[Issue 178790:
 Regex causing browser to become unresponsive](https://crbug.com/178790)**  
**[Commit: Limit EatAtLeast recursion by a budget.](https://chromium.googlesource.com/v8/v8/+/358311e)**  
  
Date(Commit): Fri Mar 01 14:50:14 2013  
Components/Type: Blink/Bug  
Labels: ["Performance"]  
Code Review: [https://chromiumcodereview.appspot.com/12380026](https://chromiumcodereview.appspot.com/12380026)  
Regress: [mjsunit/regress/regress-crbug-178790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-178790.js)  
```javascript
var r1 = "";
for (var i = 0; i < 1000; i++) {
  r1 += "a?";
}
"test".match(RegExp(r1));

var r2 = "";
for (var i = 0; i < 100; i++) {
  r2 += "(a?|b?|c?|d?|e?|f?|g?)";
}
"test".match(RegExp(r2));

var r3 = "a";
for (var i = 0; i < 1000; i++) {
  r3 = "(" + r3 + ")a";
}
"test".match(RegExp(r3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/358311e^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=358311e)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=358311e)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=358311e)  
[test/mjsunit/regress/regress-crbug-178790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-178790.js?cl=358311e)  
  

---   

## **regress-2451.js (v8 issue)**  
   
**[Issue 2451:
 Math.round with negative numbers causes a method to de/re-optimize repeatedly and finally disabled](https://crbug.com/v8/2451)**  
**[Commit: Handle negative input in inlined Math.round on Intel CPUs.](https://chromium.googlesource.com/v8/v8/+/2a3063a)**  
  
Date(Commit): Wed Feb 27 14:44:57 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/12342037](https://chromiumcodereview.appspot.com/12342037)  
Regress: [mjsunit/regress/regress-2451.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2451.js)  
```javascript
function f() {
  assertEquals(-1.0, Math.round(-1.5));
  assertEquals(-2.0, Math.round(-2.5));
  assertEquals(-1.0, Math.round(-0.5000000000000001));
}

%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();
assertOptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a3063a^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=2a3063a)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=2a3063a)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=2a3063a)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=2a3063a)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=2a3063a)  
...  
  

---   

## **regress-crbug-163530.js (chromium issue)**  
   
**[Issue 163530:
 arguments occasionally contains arguments of callee.](https://crbug.com/163530)**  
**[Commit: Fix materialization of arguments objects with unknown values.](https://chromium.googlesource.com/v8/v8/+/ea5e9ed)**  
  
Date(Commit): Wed Feb 27 14:37:51 2013  
Components/Type: Blink/Bug  
Labels: ["M-26", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/12335132](https://codereview.chromium.org/12335132)  
Regress: [mjsunit/regress/regress-crbug-163530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-163530.js)  
```javascript
(function() {
  var deoptimize = { deopt:true };
  var object = {};

  object.a = function A(x, y, z) {
    assertSame(0, arguments.length);
    return this.b();
  };

  object.b = function B() {
    assertSame(0, arguments.length);
    deoptimize.deopt;
    return arguments.length;
  };

  assertSame(0, object.a());
  assertSame(0, object.a());
  %OptimizeFunctionOnNextCall(object.a);
  assertSame(0, object.a());
  delete deoptimize.deopt;
  assertSame(0, object.a());
})();


(function() {
  'use strict';
  var deoptimize = { deopt:true };
  var object = {};

  object.a = function A(x, y, z) {
    assertSame(0, arguments.length);
    return this.b(1, 2, 3, 4, 5, 6, 7, 8);
  };

  object.b = function B(a, b, c, d, e, f, g, h) {
    assertSame(8, arguments.length);
    deoptimize.deopt;
    return arguments.length;
  };

  assertSame(8, object.a());
  assertSame(8, object.a());
  %OptimizeFunctionOnNextCall(object.a);
  assertSame(8, object.a());
  delete deoptimize.deopt;
  assertSame(8, object.a());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea5e9ed^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=ea5e9ed)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=ea5e9ed)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=ea5e9ed)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=ea5e9ed)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=ea5e9ed)  
...  
  

---   

## **regress-2441.js (v8 issue)**  
   
**[Issue 2441:
 defineProperty fails silently in strict mode](https://crbug.com/v8/2441)**  
**[Commit: Make __proto__ a foreign callback on Object.prototype.](https://chromium.googlesource.com/v8/v8/+/ce1e10f)**  
  
Date(Commit): Tue Feb 26 10:46:00 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12212011](https://codereview.chromium.org/12212011)  
Regress: [mjsunit/regress/regress-2441.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2441.js)  
```javascript
var o = {};
Object.preventExtensions(o);
assertThrows("Object.defineProperty(o, 'foobarloo', {value:{}});", TypeError);
assertThrows("Object.defineProperty(o, '__proto__', {value:{}});", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce1e10f^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=ce1e10f)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=ce1e10f)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=ce1e10f)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=ce1e10f)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ce1e10f)  
...  
  

---   

## **regress-2539.js (v8 issue)**  
   
**[Issue 2539:
 Optimized function.apply(this, arguments) broken when declared arguments are mutated.](https://crbug.com/v8/2539)**  
**[Commit: Fix f.apply() optimization when declared arguments are mutated.](https://chromium.googlesource.com/v8/v8/+/300413b)**  
  
Date(Commit): Thu Feb 14 15:12:49 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12255033](https://codereview.chromium.org/12255033)  
Regress: [mjsunit/regress/regress-2539.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2539.js)  
```javascript
"use strict";
var dispatcher = {};
dispatcher.func = C;

function A() {
  B(10, 11);
};
%PrepareFunctionForOptimization(A);
function B(x, y) {
  x = 0;
  y = 0;
  dispatcher.func.apply(this, arguments);
  assertSame(2, arguments.length);
  assertSame(10, arguments[0]);
  assertSame(11, arguments[1]);
}

function C(x, y) {
  assertSame(2, arguments.length);
  assertSame(10, arguments[0]);
  assertSame(11, arguments[1]);
}

A();
A();
%OptimizeFunctionOnNextCall(A);
A();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/300413b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=300413b)  
[test/mjsunit/compiler/inline-function-apply.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/inline-function-apply.js?cl=300413b)  
[test/mjsunit/regress/regress-2539.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2539.js?cl=300413b)  
  

---   

## **regress-2537.js (v8 issue)**  
   
**[Issue 2537:
 Range analysis propagates incorrect relation operators](https://crbug.com/v8/2537)**  
**[Commit: Fix NegateCompareOp and InvertCompareOp](https://chromium.googlesource.com/v8/v8/+/19dab05)**  
  
Date(Commit): Wed Feb 13 14:36:19 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/12217136](https://codereview.chromium.org/12217136)  
Regress: [mjsunit/regress/regress-2537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2537.js)  
```javascript
var large_int = 0x40000000;

function foo(x, expected) {
  assertEquals(expected, x);  // This succeeds.
  x += 0;                     // Force int32 representation so that
  // CompareNumericAndBranch is used.
  if (3 != x) {
    x += 0;  // Poor man's "iDef".
    // Fails due to Smi-tagging without overflow check.
    assertEquals(expected, x);
  }
};
%PrepareFunctionForOptimization(foo);
foo(1, 1);
foo(3, 3);
%OptimizeFunctionOnNextCall(foo);
foo(large_int, large_int);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19dab05^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=19dab05)  
[src/token.h](https://cs.chromium.org/chromium/src/v8/src/token.h?cl=19dab05)  
[test/mjsunit/regress/regress-2537.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2537.js?cl=19dab05)  
  

---   

## **regress-crbug-173907.js (chromium issue)**  
   
**[Issue 173907:
 CryptoJS show different behavior when debugged](https://crbug.com/173907)**  
**[Commit: Add regression test for r13617](https://chromium.googlesource.com/v8/v8/+/e83ff19)**  
  
Date(Commit): Thu Feb 07 15:38:24 2013  
Components/Type: Blink/Bug-Regression  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/12212066](https://codereview.chromium.org/12212066)  
Regress: [mjsunit/regress/regress-crbug-173907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173907.js), [mjsunit/regress/regress-crbug-173907b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173907b.js)  
```javascript
var X = 1.1;
var K = 0.5;

var O = 0;
var result = new Float64Array(2);

function spill() {
  try {
  } catch (e) {
  }
}

function buggy() {
  var v = X;
  var phi1 = v + K;
  var phi2 = v - K;

  spill();  // At this point initial values for phi1 and phi2 are spilled.

  var xmm1 = v;
  var xmm2 = v * v * v;
  var xmm3 = v * v * v * v;
  var xmm4 = v * v * v * v * v;
  var xmm5 = v * v * v * v * v * v;
  var xmm6 = v * v * v * v * v * v * v;
  var xmm7 = v * v * v * v * v * v * v * v;
  var xmm8 = v * v * v * v * v * v * v * v * v;

  // All registers are blocked and phis for phi1 and phi2 are spilled because
  // their left (incoming) value is spilled, there are no free registers,
  // and phis themselves have only ANY-policy uses.

  for (var x = 0; x < 2; x++) {
    xmm1 += xmm1 * xmm6;
    xmm2 += xmm1 * xmm5;
    xmm3 += xmm1 * xmm4;
    xmm4 += xmm1 * xmm3;
    xmm5 += xmm1 * xmm2;

    // Now swap values of phi1 and phi2 to create cycle between phis.
    var t = phi1;
    phi1 = phi2;
    phi2 = t;
  }

  // Now we want to get values of phi1 and phi2. However we would like to
  // do it in a way that does not produce any uses of phi1&phi2 that have
  // a register beneficial policy. How? We just hide these uses behind phis.
  result[0] = O === 0 ? phi1 : phi2;
  result[1] = O !== 0 ? phi1 : phi2;
};
%PrepareFunctionForOptimization(buggy);
function test() {
  buggy();
  assertArrayEquals([X + K, X - K], result);
}

test();
test();
%OptimizeFunctionOnNextCall(buggy);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e83ff19^!)  
[test/mjsunit/regress/regress-crbug-173907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-173907.js?cl=e83ff19)  
  

---   

## **regress-crbug-173974.js (chromium issue)**  
   
**[Issue 173974:
 Type-feedback oracle in V8 gets confused aoubt monomorphic IC](https://crbug.com/173974)**  
**[Commit: Tag stubs that rely on instance types as MEGAMORPHIC.](https://chromium.googlesource.com/v8/v8/+/aca87c2)**  
  
Date(Commit): Mon Feb 04 13:12:03 2013  
Components/Type: Blink/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://chromiumcodereview.appspot.com/12178017](https://chromiumcodereview.appspot.com/12178017)  
Regress: [mjsunit/regress/regress-crbug-173974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173974.js)  
```javascript
function f() {
  var count = "";
  count[0]--;
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aca87c2^!)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=aca87c2)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=aca87c2)  
[test/mjsunit/regress/regress-crbug-173974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-173974.js?cl=aca87c2)  
  

---   

## **regress-2073.js (v8 issue)**  
   
**[Issue 2073:
 Garbage collector sometimes fails on objects with Object.defineProperty (memleak?)](https://crbug.com/v8/2073)**  
**[Commit: Make embedded maps in optimized code weak.](https://chromium.googlesource.com/v8/v8/+/e6224d2)**  
  
Date(Commit): Thu Jan 24 11:55:05 2013  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/11575007](https://chromiumcodereview.appspot.com/11575007)  
Regress: [mjsunit/regress/regress-2073.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2073.js)  
```javascript
var counter = 0;

function nextid() {
  counter += 1;
  return counter;
}

function Scope() {
  this.id = nextid();
  this.parent = null;
  this.left = null;
  this.right = null;
  this.head = null;
  this.tail = null;
  this.counter = 0;
}

Scope.prototype = {
  new: function() {
    var Child,
        child;
    Child = function() {};
    Child.prototype = this;
    child = new Child();
    child.id = nextid();
    child.parent = this;
    child.left = this.last;
    child.right = null;
    child.head = null;
    child.tail = null;
    child.counter = 0;
    if (this.head) {
      this.tail.right = child;
      this.tail = child;
    } else {
      this.head = this.tail = child;
    }
    return child;
  },

  destroy: function() {
    if ($root == this) return;
    var parent = this.parent;
    if (parent.head == this) parent.head = this.right;
    if (parent.tail == this) parent.tail = this.left;
    if (this.left) this.left.right = this.right;
    if (this.right) this.right.left = this.left;
  }
};

function inc(scope) {
  scope.counter = scope.counter + 1;
}

var $root = new Scope();

n = 100000;
m = 10;

function doit() {
   var a = $root.new();
   var b = a.new();
   inc(b);
   if (i > m) $root.head.destroy();
}

for (var i = 0; i < n; i++) {
   doit();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6224d2^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=e6224d2)  
[src/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/lithium.cc?cl=e6224d2)  
[src/lithium.h](https://cs.chromium.org/chromium/src/v8/src/lithium.h?cl=e6224d2)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=e6224d2)  
[src/mark-compact.h](https://cs.chromium.org/chromium/src/v8/src/mark-compact.h?cl=e6224d2)  
...  
  

---   

## **regress-crbug-170856.js (chromium issue)**  
   
**[Issue 170856:
 Chrome Canary snaps when rendering Google AppScript pages](https://crbug.com/170856)**  
**[Commit: Correctly reset lastIndex in an RegExp object.](https://chromium.googlesource.com/v8/v8/+/9296975)**  
  
Date(Commit): Wed Jan 23 12:28:16 2013  
Components/Type: Blink/Bug-Regression  
Labels: ["M-25", "Via-Wizard", "Merge-Merged", "Hotlist-GoogleApps", "ReleaseBlock-Stable"]  
Code Review: [https://chromiumcodereview.appspot.com/11896060](https://chromiumcodereview.appspot.com/11896060)  
Regress: [mjsunit/regress/regress-crbug-170856.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-170856.js)  
```javascript
r = new RegExp("a");
for (var i = 0; i < 100; i++) {
  r["abc" + i] = i;
}
"zzzz".replace(r, "");
assertEquals(0, r.lastIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9296975^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=9296975)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=9296975)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9296975)  
[test/mjsunit/regress/regress-crbug-170856.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-170856.js?cl=9296975)  
  

---   

## **regress-2499.js (v8 issue)**  
   
**[Issue 2499:
 A specific form of bitwise operations has incorrect value sometimes](https://crbug.com/v8/2499)**  
**[Commit: Fix pattern detection for replacing shifts by rotation.](https://chromium.googlesource.com/v8/v8/+/79a0e3b)**  
  
Date(Commit): Tue Jan 22 13:55:22 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/12047015](https://chromiumcodereview.appspot.com/12047015)  
Regress: [mjsunit/regress/regress-2499.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2499.js)  
```javascript
function foo(word, nBits) {
  return word[1] >>> nBits | word[0] << 32 - nBits;
};
%PrepareFunctionForOptimization(foo);
word = [0x1001, 0];

var expected = foo(word, 1);
foo(word, 1);
%OptimizeFunctionOnNextCall(foo);
var optimized = foo(word, 1);
assertEquals(expected, optimized);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79a0e3b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=79a0e3b)  
[test/mjsunit/regress/regress-2499.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2499.js?cl=79a0e3b)  
  

---   

## **regress-2489.js (v8 issue)**  
   
**[Issue 2489:
 Deoptimization of strict arguments object with function.prototype.apply(this, arguments) dispatching produces wrong values](https://crbug.com/v8/2489)**  
**[Commit: Fix arguments materialization for inlined apply().](https://chromium.googlesource.com/v8/v8/+/0484ddc)**  
  
Date(Commit): Wed Jan 16 09:25:45 2013  
Type: Bug  
Code Review: [https://codereview.chromium.org/11931012](https://codereview.chromium.org/11931012)  
Regress: [mjsunit/regress/regress-2489.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2489.js)  
```javascript
"use strict";

function f(a, b) {
  return g("c", "d");
};
%PrepareFunctionForOptimization(f);
function g(a, b) {
  g.constructor.apply(this, arguments);
}

g.constructor = function(a, b) {
  assertEquals("c", a);
  assertEquals("d", b);
};

f("a", "b");
f("a", "b");
%OptimizeFunctionOnNextCall(f);
f("a", "b");
g.x = "deopt";
f("a", "b");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0484ddc^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=0484ddc)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=0484ddc)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=0484ddc)  
[test/mjsunit/regress/regress-2489.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2489.js?cl=0484ddc)  
  

---   

## **regress-latin-1.js (other issue)**  
   
**[Commit: Continues Latin-1 support. All tests pass with ENABLE_LATIN_1 flag.](https://chromium.googlesource.com/v8/v8/+/e41c170)**  
  
Date(Commit): Wed Jan 09 15:47:53 2013  
Code Review: [https://chromiumcodereview.appspot.com/11818025](https://chromiumcodereview.appspot.com/11818025)  
Regress: [mjsunit/regress/regress-latin-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-latin-1.js)  
```javascript
assertEquals(String.fromCharCode(97, 220, 256), 'a' + '\u00DC' + '\u0100');
assertEquals(String.fromCharCode(97, 220, 256), 'a\u00DC\u0100');

assertEquals(0x80, JSON.stringify("\x80").charCodeAt(1));
assertEquals(0x80, JSON.stringify("\x80", 0, null).charCodeAt(1));

assertEquals(['a', 'b', '\xdc'], ['b', '\xdc', 'a'].sort());

assertEquals(['\xfc\xdc', '\xfc'], new RegExp('(\xdc)\\1', 'i').exec('\xfc\xdc'));
var total_lo = 0;
for (var i = 0; i < 0xff; i++) {
  var base = String.fromCharCode(i);
  var escaped = base;
  if (base == '(' || base == ')' || base == '*' || base == '+' ||
      base == '?' || base == '[' || base == ']' || base == '\\' ||
      base == '$' || base == '^' || base == '|') {
    escaped = '\\' + base;
  }
  var lo = String.fromCharCode(i + 0x20);
  base_result = new RegExp('(' + escaped + ')\\1', 'i').exec(base + base);
  assertEquals( base_result, [base + base, base]);
  lo_result = new RegExp('(' + escaped + ')\\1', 'i').exec(base + lo);
  if (base.toLowerCase() == lo) {
    assertEquals([base + lo, base], lo_result);
    total_lo++;
  } else {
    assertEquals(null, lo_result);
  }
}
assertEquals((90-65+1)+(222-192-1+1), total_lo);

assertEquals( 1, +(String.fromCharCode(0xA0) + '1') );

assertEquals(["+\u00a3", "=="], "+\u00a3==".match(/\W\W/g));

assertTrue(/\u0178/i.test('\u00ff'));

assertTrue(/\u039c/i.test('\u00b5'));
assertTrue(/\u039c/i.test('\u03bc'));
assertTrue(/\u00b5/i.test('\u03bc'));
assertTrue(/[\u039b-\u039d]/i.test('\u00b5'));
assertFalse(/[^\u039b-\u039d]/i.test('\u00b5'));
assertFalse(/[\u039b-\u039d]/.test('\u00b5'));
assertTrue(/[^\u039b-\u039d]/.test('\u00b5'));

for (var testNumber = 0; testNumber < 2; testNumber++) {
  var testString = "\xdc";
  var loopLength = testNumber == 0 ? 0 : 20;
  for (var i = 0; i < loopLength; i++ ) {
    testString += testString;
  }
  var stringified = JSON.stringify({"test" : testString}, null, 0);
  var stringifiedExpected = '{"test":"' + testString + '"}';
  assertEquals(stringifiedExpected, stringified);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e41c170^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=e41c170)  
[src/arm/regexp-macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/regexp-macro-assembler-arm.cc?cl=e41c170)  
[src/extensions/externalize-string-extension.cc](https://cs.chromium.org/chromium/src/v8/src/extensions/externalize-string-extension.cc?cl=e41c170)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=e41c170)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=e41c170)  
...  
  
  
---   

## **regress-2419.js (v8 issue)**  
   
**[Issue 2419:
 Array.prototype.sort does not check frozenness](https://crbug.com/v8/2419)**  
**[Commit: Check for read-only-ness when preparing for array sort.](https://chromium.googlesource.com/v8/v8/+/4ee20d8)**  
  
Date(Commit): Fri Jan 04 15:24:47 2013  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11759022](https://chromiumcodereview.appspot.com/11759022)  
Regress: [mjsunit/regress/regress-2419.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2419.js)  
```javascript
var a = [5, 4, 3, 2, 1, 0];
Object.freeze(a);
assertThrows(function() { a.sort(); });
assertArrayEquals([5, 4, 3, 2, 1, 0], a);

var b = {0: 5, 1: 4, 2: 3, 3: 2, 4: 1, 5: 0, length: 6};
Object.freeze(b);
assertThrows(function() { Array.prototype.sort.call(b); });
assertPropertiesEqual({0: 5, 1: 4, 2: 3, 3: 2, 4: 1, 5: 0, length: 6}, b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ee20d8^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4ee20d8)  
[test/mjsunit/regress/regress-2419.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2419.js?cl=4ee20d8)  
  

---   
