# V8Harvest  
The Harvest of V8 regress in 2011.  
  

## **regress-1849.js (v8 issue)**  
   
**[Preallocated Array incorrectly transitions to fast-double-elements backing store](https://crbug.com/v8/1849)**  
**[Commit: Ensure large Smi-only arrays don't transition to FAST_DOUBLE_ARRAY](https://chromium.googlesource.com/v8/v8/+/dff0e36)**  
  
Date(Commit): Fri Dec 30 12:54:23 2011  
Code Review: [http://codereview.chromium.org/8968028](http://codereview.chromium.org/8968028)  
Regress: [mjsunit/regress/regress-1849.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1849.js)  
```javascript
var count = 1e5;
var arr = new Array(count);
assertFalse(%HasDoubleElements(arr));
for (var i = 0; i < count; i++) {
  arr[i] = 0;
}
assertFalse(%HasDoubleElements(arr));
assertTrue(%HasSmiElements(arr));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dff0e36^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=dff0e36)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=dff0e36)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=dff0e36)  
[test/mjsunit/regress/regress-1849.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1849.js?cl=dff0e36)  
[test/mjsunit/regress/regress-95113.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-95113.js?cl=dff0e36)  
  

---   

## **regress-108296.js (chromium issue)**  
   
**[UNKNOWN in v8::internal::MarkCompactCollector::EmptyMarkingDeque](https://crbug.com/108296)**  
**[Commit: Avoid embedding new space objects into code objects in the lithium gap resolver.](https://chromium.googlesource.com/v8/v8/+/3947056)**  
  
Date(Commit): Fri Dec 23 10:39:01 2011  
Components: Blink>JavaScript>APIBlink>JavaScriptBlink  
Labels: ["MovedFrom-22", "MovedFrom18", "MovedFrom-20", "MovedFrom-21"]  
Code Review: [http://codereview.chromium.org/8960004](http://codereview.chromium.org/8960004)  
Regress: [mjsunit/regress/regress-108296.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-108296.js)  
```javascript
function f(k, a, b) {
  // Create control flow for a.foo.  Control flow resolution will
  // be generated as a part of a gap move. Gap move operate on immediates as
  // a.foo is a CONSTANT_FUNCTION.
  var x = k ? a.foo : a.foo;
  return x.prototype;
};
%PrepareFunctionForOptimization(f);
var a = {};

a.foo = function() {
  return function() {};
}();

f(true, a, a);
f(true, a, a);
f(false, a, a);
f(false, a, a);
%OptimizeFunctionOnNextCall(f);
f(true, a, a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3947056^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=3947056)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=3947056)  
[src/arm/lithium-gap-resolver-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-gap-resolver-arm.cc?cl=3947056)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=3947056)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=3947056)  
...  
  

---   

## **regress-1530.js (v8 issue)**  
   
**[Defining function's 'prototype' property with Object.defineProperty sets different value and makes property immutable](https://crbug.com/v8/1530)**  
**[Commit: Fix handling of foreign callbacks in DefineOwnProperty.](https://chromium.googlesource.com/v8/v8/+/04f0e33)**  
  
Date(Commit): Tue Dec 20 08:49:51 2011  
Code Review: [http://codereview.chromium.org/8996008](http://codereview.chromium.org/8996008)  
Regress: [mjsunit/regress/regress-1530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1530.js)  
```javascript
var f = function() {};

var a = { foo: 'bar' };
f.prototype = a;
assertSame(f.prototype, a);
assertSame(f.prototype.foo, 'bar');
assertSame(new f().foo, 'bar');
assertSame(Object.getPrototypeOf(new f()), a);
assertSame(Object.getOwnPropertyDescriptor(f, 'prototype').value, a);
assertTrue(Object.getOwnPropertyDescriptor(f, 'prototype').writable);

var b = { foo: 'baz' };
Object.defineProperty(f, 'prototype', { value: b, writable: true });
assertSame(f.prototype, b);
assertSame(f.prototype.foo, 'baz');
assertSame(new f().foo, 'baz');
assertSame(Object.getPrototypeOf(new f()), b);
assertSame(Object.getOwnPropertyDescriptor(f, 'prototype').value, b);
assertTrue(Object.getOwnPropertyDescriptor(f, 'prototype').writable);

var c = { foo: 'other' };
f.prototype = c;
assertSame(f.prototype, c);
assertSame(f.prototype.foo, 'other');
assertSame(new f().foo, 'other');
assertSame(Object.getPrototypeOf(new f()), c);
assertSame(Object.getOwnPropertyDescriptor(f, 'prototype').value, c);
assertTrue(Object.getOwnPropertyDescriptor(f, 'prototype').writable);

var d = { foo: 'final' };
Object.defineProperty(f, 'prototype', { value: d, writable: false });
assertSame(f.prototype, d);
assertSame(f.prototype.foo, 'final');
assertSame(new f().foo, 'final');
assertSame(Object.getPrototypeOf(new f()), d);
assertSame(Object.getOwnPropertyDescriptor(f, 'prototype').value, d);
assertFalse(Object.getOwnPropertyDescriptor(f, 'prototype').writable);

assertThrows("'use strict'; f.prototype = {}");
assertThrows("Object.defineProperty(f, 'prototype', { value: {} })");

Object.defineProperty(f, 'name', { value: {} });
Object.defineProperty(f, 'length', { value: {} });
assertThrows("Object.defineProperty(f, 'caller', { value: {} })");
assertThrows("Object.defineProperty(f, 'arguments', { value: {} })");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/04f0e33^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=04f0e33)  
[test/mjsunit/regress/regress-1530.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1530.js?cl=04f0e33)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=04f0e33)  
  

---   

## **regress-crbug-100859.js (chromium issue)**  
   
**[[LangFuzz] Crash on Heap with null deref](https://crbug.com/100859)**  
**[Commit: Fix crash in d8 when external array ctor hits stack overflow](https://chromium.googlesource.com/v8/v8/+/91efb31)**  
  
Date(Commit): Tue Dec 13 13:51:58 2011  
Components: None  
Labels: []  
Code Review: [http://codereview.chromium.org/8898021](http://codereview.chromium.org/8898021)  
Regress: [mjsunit/regress/regress-crbug-100859.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-100859.js)  
```javascript
function setx() {
  setx(typeof new Uint16Array('x') === 'object');
}
var exception = false;
try {
  setx();
} catch (ex) {
  assertTrue(ex instanceof RangeError);
  exception = true;
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91efb31^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=91efb31)  
[test/mjsunit/regress/regress-crbug-100859.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-100859.js?cl=91efb31)  
  

---   

## **regress-97116.js (chromium issue)**  
   
**[Chrome: Crash Report - Magic Signature: v8::internal::HeapObjectIterator::HeapObjectIterator](https://crbug.com/97116)**  
**[Commit: Ensure that non-optimized code objects are not flushed for inlined functions.](https://chromium.googlesource.com/v8/v8/+/a457040)**  
  
Date(Commit): Thu Dec 08 16:07:07 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "M-17", "MovedFrom-16"]  
Code Review: [http://codereview.chromium.org/8888011](http://codereview.chromium.org/8888011)  
Regress: [mjsunit/regress/regress-97116.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-97116.js), [mjsunit/regress/regress-97116b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-97116b.js)  
```javascript
function deopt() {
  try {
  } catch (e) {
  }  // Avoid inlining.
  %DeoptimizeFunction(outer);
  for (var i = 0; i < 10; i++) gc();  // Force code flushing.
}

function outer(should_deopt) {
  inner(should_deopt);
};
%PrepareFunctionForOptimization(outer);
function inner(should_deopt) {
  if (should_deopt) deopt();
}

outer(false);
outer(false);
%OptimizeFunctionOnNextCall(outer);
outer(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a457040^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=a457040)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=a457040)  
[src/mark-compact.h](https://cs.chromium.org/chromium/src/v8/src/mark-compact.h?cl=a457040)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=a457040)  
[src/v8threads.h](https://cs.chromium.org/chromium/src/v8/src/v8threads.h?cl=a457040)  
...  
  

---   

## **regress-106351.js (chromium issue)**  
   
**[Javascript "if" is nondeterministic](https://crbug.com/106351)**  
**[Commit: Fix a bug with register use in optimized Math.round.](https://chromium.googlesource.com/v8/v8/+/c1662a1)**  
  
Date(Commit): Wed Dec 07 10:13:46 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/8833007](http://codereview.chromium.org/8833007)  
Regress: [mjsunit/compiler/regress-106351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-106351.js)  
```javascript
function test(x) {
  var v = Math.round(x) - x;
  assertEquals(0.5, v);
}

%PrepareFunctionForOptimization(test);
for (var i = 0; i < 5; ++i) test(0.5);
%OptimizeFunctionOnNextCall(test);
test(0.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c1662a1^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=c1662a1)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=c1662a1)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=c1662a1)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=c1662a1)  
[test/mjsunit/compiler/regress-106351.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-106351.js?cl=c1662a1)  
  

---   

## **regress-lazy-deopt.js (other issue)**  
   
**[Commit: Fix lazy deoptimization at HInvokeFunction and enable target-recording call-function stub.](https://chromium.googlesource.com/v8/v8/+/8480569)**  
  
Date(Commit): Wed Nov 16 08:44:30 2011  
Code Review: [http://codereview.chromium.org/8492004](http://codereview.chromium.org/8492004)  
Regress: [mjsunit/compiler/regress-lazy-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-lazy-deopt.js)  
```javascript
function foo() { return 1; }

function f(x, y) {
  var a = [0];
  if (x == 0) {
    %DeoptimizeFunction(f);
    return 1;
  }
  a[0] = %_Call(f, null, x - 1);
  return x >> a[0];
}

%PrepareFunctionForOptimization(f);
f(42);
f(42);
assertEquals(42, f(42));
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f(42));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8480569^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=8480569)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=8480569)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=8480569)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=8480569)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=8480569)  
...  
  
  
---   

## **regress-103259.js (chromium issue)**  
   
**[[LangFuzz] Crash at v8::internal::WriteQuoteJsonString with invalid write](https://crbug.com/103259)**  
**[Commit: Fixing issue 103259.](https://chromium.googlesource.com/v8/v8/+/53c6077)**  
  
Date(Commit): Tue Nov 08 14:59:40 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "Merge-Merged", "Security_Impact-Stable", "M-15", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "CVE-2011-3900", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/8498011](http://codereview.chromium.org/8498011)  
Regress: [mjsunit/regress/regress-103259.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-103259.js)  
```javascript
var a = [];
a[8192] = '';
assertTrue(%HasDictionaryElements(a));
var uc16 = '\u0094';
var test = uc16;
for (var i = 0; i < 13; i++) test += test;
assertEquals(test, a.join(uc16));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53c6077^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=53c6077)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=53c6077)  
[test/mjsunit/regress/regress-103259.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-103259.js?cl=53c6077)  
  

---   

## **regress-inline-callfunctionstub.js (other issue)**  
   
**[Commit: Fix bug in inlining call-as-function when inlining multiple levels deep.](https://chromium.googlesource.com/v8/v8/+/2d4bb18)**  
  
Date(Commit): Wed Oct 26 10:31:06 2011  
Code Review: [http://codereview.chromium.org/8341019](http://codereview.chromium.org/8341019)  
Regress: [mjsunit/compiler/regress-inline-callfunctionstub.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-inline-callfunctionstub.js)  
```javascript
function f() { return 42; }

var o = {g : function () { return f(); } }
function main(func) {
  var v=0;
  for (var i=0; i<1; i++) {
    if (func()) v = 42;
  }
}

%PrepareFunctionForOptimization(main);
main(o.g);
main(o.g);
main(o.g);
%OptimizeFunctionOnNextCall(main);
main(o.g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d4bb18^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2d4bb18)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=2d4bb18)  
[test/mjsunit/compiler/regress-inline-callfunctionstub.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-inline-callfunctionstub.js?cl=2d4bb18)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=2d4bb18)  
  
  
---   

## **regress-100409.js (chromium issue)**  
   
**[Tricky and subtle memory corruption, manifesting in buggy charAt on large strings](https://crbug.com/100409)**  
**[Commit: Take loop side-effects into account when collecting side-effects on the path between two blocks.](https://chromium.googlesource.com/v8/v8/+/f8c2d38)**  
  
Date(Commit): Tue Oct 25 15:39:55 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-15"]  
Code Review: [http://codereview.chromium.org/8395002](http://codereview.chromium.org/8395002)  
Regress: [mjsunit/regress/regress-100409.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-100409.js)  
```javascript
function outer() {
  var val = 0;

  function foo() {
    val = 0;
    val;
    var z = false;
    var y = true;
    if (!z) {
      while (z = !z) {
        if (y) val++;
      }
    }
    return val++;
  };
  %PrepareFunctionForOptimization(foo);
  return foo;
}

var foo = outer();

assertEquals(1, foo());
assertEquals(1, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f8c2d38^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=f8c2d38)  
[test/mjsunit/regress/regress-100409.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-100409.js?cl=f8c2d38)  
  

---   

## **regress-deopt-call-as-function.js (other issue)**  
   
**[Commit: Fix bug in environment simulation after inlined call-as-function.](https://chromium.googlesource.com/v8/v8/+/53e7502)**  
  
Date(Commit): Mon Oct 24 13:53:08 2011  
Code Review: [http://codereview.chromium.org/8360001](http://codereview.chromium.org/8360001)  
Regress: [mjsunit/compiler/regress-deopt-call-as-function.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-deopt-call-as-function.js)  
```javascript
function bar(a, b) {try { return a; } finally { } }

function test_context() {
  function foo(x) { return 42; }
  var s, t;
  for (var i = 0x7fff0000; i < 0x80000000; i++) {
    bar(t = foo(i) ? bar(42 + i - i) : bar(0), s = i + t);
  }
  return s;
}
assertEquals(0x7fffffff + 42, test_context());


function value_context() {
  function foo(x) { return 42; }
  var s, t;
  for (var i = 0x7fff0000; i < 0x80000000; i++) {
    bar(t = foo(i), s = i + t);
  }
  return s;
}
assertEquals(0x7fffffff + 42, value_context());


function effect_context() {
  function foo(x) { return 42; }
  var s, t;
  for (var i = 0x7fff0000; i < 0x80000000; i++) {
    bar(foo(i), s = i + 42);
  }
  return s;
}
assertEquals(0x7fffffff + 42, effect_context());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53e7502^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=53e7502)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=53e7502)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=53e7502)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=53e7502)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=53e7502)  
...  
  
  
---   

## **regress-100702.js (chromium issue)**  
   
**[Array#extras do not convert primitives as any other function does](https://crbug.com/100702)**  
**[Commit: Fix handling of non-object receivers for array builtins.](https://chromium.googlesource.com/v8/v8/+/b3eba9e)**  
  
Date(Commit): Wed Oct 19 09:24:37 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/8347034](http://codereview.chromium.org/8347034)  
Regress: [mjsunit/regress/regress-100702.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-100702.js)  
```javascript
String.prototype.isThatMe = function () {
  assertFalse(this === str);
};

var str = "abc";
str.isThatMe();
str.isThatMe.call(str);

var arr = [1];
arr.forEach("".isThatMe, str);
arr.filter("".isThatMe, str);
arr.some("".isThatMe, str);
arr.every("".isThatMe, str);
arr.map("".isThatMe, str);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b3eba9e^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=b3eba9e)  
[test/mjsunit/regress/regress-100702.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-100702.js?cl=b3eba9e)  
  

---   

## **regress-1757.js (v8 issue)**  
   
**[String.charAt on a substring of an externalized string returns wrong result.](https://crbug.com/v8/1757)**  
**[Commit: Fixing issue 1757 (string slices of external strings).](https://chromium.googlesource.com/v8/v8/+/3249530)**  
  
Date(Commit): Mon Oct 10 16:09:03 2011  
Code Review: [http://codereview.chromium.org/8217011](http://codereview.chromium.org/8217011)  
Regress: [mjsunit/regress/regress-1757.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1757.js)  
```javascript
var a = "internalized dummy";
a = "abcdefghijklmnopqrstuvqxy"+"z";
externalizeString(a, true);
assertEquals('b', a.substring(1).charAt(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3249530^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=3249530)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=3249530)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=3249530)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=3249530)  
[test/mjsunit/regress/regress-1757.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1757.js?cl=3249530)  
  

---   

## **regress-99167.js (chromium issue)**  
   
**[[LangFuzz] Crash on Heap involving GC (invalid write)](https://crbug.com/99167)**  
**[Commit: Add a regression test for an already fixed issue.](https://chromium.googlesource.com/v8/v8/+/fa18fdb)**  
  
Date(Commit): Mon Oct 10 10:46:27 2011  
Components: InternalsBlink>JavaScriptBlink  
Labels: ["CVE-2011-3886", "Restrict-AddIssueComment-EditIssue", "Reward-1000", "Security_Impact-Stable", "Stability-Valgrind", "M-15", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/8222002](http://codereview.chromium.org/8222002)  
Regress: [mjsunit/regress/regress-99167.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-99167.js)  
```javascript
eval("function Node() { this.a = 1; this.a = 3; }");
new Node;
for (var i = 0; i < 4; ++i) gc();
for (var i = 0; i < 100000; ++i) new Node;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa18fdb^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=fa18fdb)  
[test/mjsunit/regress/regress-99167.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-99167.js?cl=fa18fdb)  
  

---   

## **regress-98773.js (chromium issue)**  
   
**[[LangFuzz] Crash at v8::Object::SlowGetPointerFromInternalField with invalid read](https://crbug.com/98773)**  
**[Commit: Fix preparation for sorting of external arrays.](https://chromium.googlesource.com/v8/v8/+/c0345184)**  
  
Date(Commit): Tue Oct 04 13:49:50 2011  
Components: Internals  
Labels: ["CVE-2011-3886", "Restrict-AddIssueComment-EditIssue", "Reward-1000", "merge-merged-874", "Merge-Merged", "Security_Impact-Stable", "Stability-Valgrind", "M-15", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/8122020](http://codereview.chromium.org/8122020)  
Regress: [mjsunit/regress/regress-98773.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-98773.js)  
```javascript
var array = new Int16Array(23);
array[7] = 7; array[9] = 9;
assertEquals(23, array.length);
assertEquals(7, array[7]);
assertEquals(9, array[9]);

Array.prototype.sort.call(array);
assertEquals(23, array.length);
assertEquals(7, array[21]);
assertEquals(9, array[22]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c0345184^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c0345184)  
[test/mjsunit/regress/regress-98773.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-98773.js?cl=c0345184)  
  

---   

## **regress-1415.js (v8 issue)**  
   
**[UTF-8 to UCS4 invalid conversion](https://crbug.com/v8/1415)**  
**[Commit: Fix issue 1415 - allow surrogate pair codes in decodeURIComponent.](https://chromium.googlesource.com/v8/v8/+/4750f0c)**  
  
Date(Commit): Tue Oct 04 07:15:07 2011  
Code Review: [http://codereview.chromium.org/8118004](http://codereview.chromium.org/8118004)  
Regress: [mjsunit/regress/regress-1415.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1415.js)  
```javascript
assertThrows(function(){ decodeURIComponent("%ED%A0%80"); }, URIError);
assertThrows(function(){ decodeURIComponent("%ED%AF%BF"); }, URIError);
assertThrows(function(){ decodeURIComponent("%ED%B0%80"); }, URIError);
assertThrows(function(){ decodeURIComponent("%ED%BF%BF"); }, URIError);

assertThrows(function(){ decodeURIComponent("%C1%BF"); }, URIError);
assertThrows(function(){ decodeURIComponent("%E0%9F%BF"); }, URIError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4750f0c^!)  
[src/uri.js](https://cs.chromium.org/chromium/src/v8/src/uri.js?cl=4750f0c)  
[test/mjsunit/regress/regress-1415.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1415.js?cl=4750f0c)  
  

---   

## **regress-1748.js (v8 issue)**  
   
**[Problem with regex operators ^ and $](https://crbug.com/v8/1748)**  
**[Commit: Fix bug in x64 RegExp detecting start of string.](https://chromium.googlesource.com/v8/v8/+/4b385d7)**  
  
Date(Commit): Mon Oct 03 10:31:01 2011  
Code Review: [http://codereview.chromium.org/8116001](http://codereview.chromium.org/8116001)  
Regress: [mjsunit/regress/regress-1748.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1748.js)  
```javascript
var str = Array(10000).join("X");
str.replace(/^|X/g, function(m, i, s) {
  if (i > 0) assertEquals("X", m, "at position 0x" + i.toString(16));
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b385d7^!)  
[src/regexp-macro-assembler-tracer.cc](https://cs.chromium.org/chromium/src/v8/src/regexp-macro-assembler-tracer.cc?cl=4b385d7)  
[src/x64/regexp-macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/regexp-macro-assembler-x64.cc?cl=4b385d7)  
[test/mjsunit/regress/regress-1748.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1748.js?cl=4b385d7)  
  

---   

## **regress-1692.js (v8 issue)**  
   
**[propertyIsEnumerable is broken for numeric property names](https://crbug.com/v8/1692)**  
**[Commit: Check enumerability of array indices correctly in propertyIsEnumerable.](https://chromium.googlesource.com/v8/v8/+/165e105)**  
  
Date(Commit): Mon Oct 03 09:15:58 2011  
Code Review: [http://codereview.chromium.org/8113001](http://codereview.chromium.org/8113001)  
Regress: [mjsunit/regress/regress-1692.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1692.js)  
```javascript
var p = Object.create({}, {
  a : { value : 42, enumerable : true },
  b : { value : 42, enumerable : false },
  1 : { value : 42, enumerable : true },
  2 : { value : 42, enumerable : false },
  f : { get: function(){}, enumerable: true },
  g : { get: function(){}, enumerable: false },
  11 : { get: function(){}, enumerable: true },
  12 : { get: function(){}, enumerable: false }
});
var o = Object.create(p, {
  c : { value : 42, enumerable : true },
  d : { value : 42, enumerable : false },
  3 : { value : 42, enumerable : true },
  4 : { value : 42, enumerable : false },
  h : { get: function(){}, enumerable: true },
  k : { get: function(){}, enumerable: false },
  13 : { get: function(){}, enumerable: true },
  14 : { get: function(){}, enumerable: false }
});

assertFalse(o.propertyIsEnumerable("a"));
assertFalse(o.propertyIsEnumerable("b"));
assertFalse(o.propertyIsEnumerable("1"));
assertFalse(o.propertyIsEnumerable("2"));

assertTrue(o.propertyIsEnumerable("c"));
assertFalse(o.propertyIsEnumerable("d"));
assertTrue(o.propertyIsEnumerable("3"));
assertFalse(o.propertyIsEnumerable("4"));

assertFalse(o.propertyIsEnumerable("f"));
assertFalse(o.propertyIsEnumerable("g"));
assertFalse(o.propertyIsEnumerable("11"));
assertFalse(o.propertyIsEnumerable("12"));

assertTrue(o.propertyIsEnumerable("h"));
assertFalse(o.propertyIsEnumerable("k"));
assertTrue(o.propertyIsEnumerable("13"));
assertFalse(o.propertyIsEnumerable("14"));

assertFalse(o.propertyIsEnumerable("xxx"));
assertFalse(o.propertyIsEnumerable("999"));

var o = Object("string");
o[10] = 42;
assertTrue(o.propertyIsEnumerable(10));
assertTrue(o.propertyIsEnumerable(0));

var o = [1,2,3,4,5];
assertTrue(o.propertyIsEnumerable(3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/165e105^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=165e105)  
[test/mjsunit/regress/regress-1692.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1692.js?cl=165e105)  
  

---   

## **regress-1708.js (v8 issue)**  
   
**[MarkWordToObjectStarts encountered consecutive one-bits.](https://crbug.com/v8/1708)**  
**[Commit: Fix transferal of marking bits on array trimming.](https://chromium.googlesource.com/v8/v8/+/873e498)**  
  
Date(Commit): Thu Sep 22 13:03:22 2011  
Code Review: [http://codereview.chromium.org/7979038](http://codereview.chromium.org/7979038)  
Regress: [mjsunit/regress/regress-1708.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1708.js)  
```javascript
(function() {
  var head = new Array(1);
  var tail = head;

  // Fill heap to increase old-space size and trigger concurrent sweeping on
  // some of the old-space pages.
  for (var i = 0; i < 200; i++) {
    tail[1] = new Array(1000);
    tail = tail[1];
  }
  array = new Array(100);
  gc(); gc();

  // At this point "array" should have been promoted to old-space and be
  // located in a concurrently swept page with intact marking bits. Now shift
  // the array to trigger left-trimming operations.
  assertEquals(100, array.length);
  for (var i = 0; i < 50; i++) {
    array.shift();
  }
  assertEquals(50, array.length);

  // At this point "array" should have been trimmed from the left with
  // marking bits being correctly transferred to the new object start.
  // Scavenging operations cause concurrent sweeping to advance and verify
  // that marking bit patterns are still sane.
  for (var i = 0; i < 200; i++) {
    tail[1] = new Array(1000);
    tail = tail[1];
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/873e498^!)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=873e498)  
[test/mjsunit/regress/regress-1708.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1708.js?cl=873e498)  
  

---   

## **regress-1711.js (v8 issue)**  
   
**[String.prototype.split should execute spec-defined steps in order](https://crbug.com/v8/1711)**  
**[Commit: Fixed string.split: always convert non-regexp separator to string.](https://chromium.googlesource.com/v8/v8/+/b7cac76)**  
  
Date(Commit): Thu Sep 22 08:18:58 2011  
Code Review: [http://codereview.chromium.org/7976046](http://codereview.chromium.org/7976046)  
Regress: [mjsunit/regress/regress-1711.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1711.js)  
```javascript
var side_effect = false;
var separator = new Object();
separator.toString = function() {
  side_effect = true;
  return undefined;
}
'subject'.split(separator, 0);
assertTrue(side_effect);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b7cac76^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=b7cac76)  
[test/mjsunit/regress/regress-1711.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1711.js?cl=b7cac76)  
  

---   

## **regress-96523.js (chromium issue)**  
   
**[dvice.com doesn’t show me articles](https://crbug.com/96523)**  
**[Commit: Mark variables as being accessed from any inner scope, not only function scopes](https://chromium.googlesource.com/v8/v8/+/96de832)**  
  
Date(Commit): Wed Sep 14 13:51:29 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-15"]  
Code Review: [http://codereview.chromium.org/7890031](http://codereview.chromium.org/7890031)  
Regress: [mjsunit/regress/regress-96523.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-96523.js)  
```javascript
with ({x:'outer'}) {
  (function() {
    var x = 'inner';
    try {
      throw 'Exception';
    } catch (e) {
      assertEquals('inner', x);
    }
  })()
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96de832^!)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=96de832)  
[src/variables.cc](https://cs.chromium.org/chromium/src/v8/src/variables.cc?cl=96de832)  
[src/variables.h](https://cs.chromium.org/chromium/src/v8/src/variables.h?cl=96de832)  
[test/mjsunit/regress/regress-96523.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-96523.js?cl=96de832)  
  

---   

## **regress-bind-receiver.js (other issue)**  
   
**[Commit: Fix for .bind regression.](https://chromium.googlesource.com/v8/v8/+/28f7136)**  
  
Date(Commit): Tue Sep 13 17:14:39 2011  
Code Review: [http://codereview.chromium.org/7892013](http://codereview.chromium.org/7892013)  
Regress: [mjsunit/regress/regress-bind-receiver.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-bind-receiver.js)  
```javascript
function strict() { 'use strict'; return this; }
function lenient() { return this; }
var obj = {};

assertEquals(true, strict.bind(true)());
assertEquals(42, strict.bind(42)());
assertEquals("", strict.bind("")());
assertEquals(null, strict.bind(null)());
assertEquals(undefined, strict.bind(undefined)());
assertEquals(obj, strict.bind(obj)());

assertEquals(true, lenient.bind(true)() instanceof Boolean);
assertEquals(true, lenient.bind(42)() instanceof Number);
assertEquals(true, lenient.bind("")() instanceof String);
assertEquals(this, lenient.bind(null)());
assertEquals(this, lenient.bind(undefined)());
assertEquals(obj, lenient.bind(obj)());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/28f7136^!)  
[src/execution.cc](https://cs.chromium.org/chromium/src/v8/src/execution.cc?cl=28f7136)  
[test/mjsunit/regress/regress-bind-receiver.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-bind-receiver.js?cl=28f7136)  
  
  
---   

## **regress-95920.js (chromium issue)**  
   
**[[LangFuzz] Crash at v8::internal::ElementsAccessorBase with invalid read](https://crbug.com/95920)**  
**[Commit: Don't allow seal or element property re-definition on external arrays.](https://chromium.googlesource.com/v8/v8/+/df860ed)**  
  
Date(Commit): Fri Sep 09 14:30:00 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Reward-1000", "Merge-Merged", "Security_Impact-Stable", "M-14", "Security_Severity-High", "CVE-2011-2875", "allpublic", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/7858031](http://codereview.chromium.org/7858031)  
Regress: [mjsunit/regress/regress-95920.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-95920.js)  
```javascript
Object.preventExtensions(new Int8Array(42));
Object.seal(new Int8Array(42));

Object.freeze(new Int8Array(0));

var o = new Int8Array(42);
assertThrows(function() {
  Object.freeze(o);
  assertUnreable();
  }, TypeError);

assertFalse(Object.isExtensible(o));

assertThrows(function() {
    Object.defineProperty(new Int8Array(42), "1",
                          { writable: false, value: "1" });
    assertUnreable();
  });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df860ed^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=df860ed)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=df860ed)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=df860ed)  
[test/mjsunit/regress/regress-95920.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-95920.js?cl=df860ed)  
  

---   

## **regress-95485.js (chromium issue)**  
   
**[[LangFuzz] Crash at v8::internal::Object::Lookup](https://crbug.com/95485)**  
**[Commit: Fix a bug in abrupt exit from with or catch inside finally.](https://chromium.googlesource.com/v8/v8/+/8b165d4)**  
  
Date(Commit): Wed Sep 07 09:21:44 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-15", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7837023](http://codereview.chromium.org/7837023)  
Regress: [mjsunit/regress/regress-95485.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-95485.js)  
```javascript
function Test() {
  var left  = 'XXX';
  var right = 'YYY';
  for (var i = 0; i < 3; i++) {
    var cons = left + right;
    var substring = cons.substring(2, 4);
    try {
      with ({Test: i})
          continue;
    } finally { }
  }
  return substring;
}

assertEquals('XY', Test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8b165d4^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=8b165d4)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=8b165d4)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=8b165d4)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=8b165d4)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=8b165d4)  
...  
  

---   

## **regress-95113.js (chromium issue)**  
   
**[Chrome_Mac: Crash Report - Stack Signature: v8::internal::JSObject::SetFastDoubleElement-960f3d54_f2e2406b_1106978a_b6d23d44_a05a4531](https://crbug.com/95113)**  
**[Commit: Fix possible crash in FixedDoubleArray::Initialize()](https://chromium.googlesource.com/v8/v8/+/09c66d2)**  
  
Date(Commit): Tue Sep 06 14:07:54 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "M-15"]  
Code Review: [http://codereview.chromium.org/7833040](http://codereview.chromium.org/7833040)  
Regress: [mjsunit/regress/regress-95113.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-95113.js)  
```javascript
function get_double_array() {
  var a = new Array(100000);
  var i = 0;
  while (!%HasDoubleElements(a)) {
    a[i] = i + 0.1;
    i += 1;
  }
  assertTrue(%HasDoubleElements(a));
  a.length = 1;
  a[0] = 1.5;
  a.length = 2;
  a[1] = 2.5;
  assertEquals(a[0], 1.5);
  assertEquals(a[1], 2.5);
  assertTrue(%HasDoubleElements(a));
  return a;
}

var a = get_double_array();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09c66d2^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=09c66d2)  
[test/mjsunit/regress/regress-95113.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-95113.js?cl=09c66d2)  
  

---   

## **regress-94425.js (chromium issue)**  
   
**[Crash when clicking on tab contents.](https://crbug.com/94425)**  
**[Commit: Fix bug in Page::GetRegionMaskForSpan.](https://chromium.googlesource.com/v8/v8/+/d451878)**  
  
Date(Commit): Tue Sep 06 11:24:48 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Stability-Crash", "M-15"]  
Code Review: [http://codereview.chromium.org/7779037](http://codereview.chromium.org/7779037)  
Regress: [mjsunit/regress/regress-94425.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-94425.js)  
```javascript
var N = 2040 - 2 + 10;
var arr = new Array(N);

gc();
gc();
gc();

arr[arr.length - 2] = new Object;

for (var i = 0; i < 9; i++) arr.shift();

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d451878^!)  
[src/spaces-inl.h](https://cs.chromium.org/chromium/src/v8/src/spaces-inl.h?cl=d451878)  
[test/mjsunit/regress/regress-94425.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-94425.js?cl=d451878)  
  

---   

## **regress-1215.js (v8 issue)**  
   
**["name" and "message" have bad attributes on *Error.prototype](https://crbug.com/v8/1215)**  
**[Commit: Add regression test for issue 1215, expand regression test for issue 1447.](https://chromium.googlesource.com/v8/v8/+/0fbf8c88)**  
  
Date(Commit): Tue Sep 06 07:43:51 2011  
Code Review: [http://codereview.chromium.org/7739024](http://codereview.chromium.org/7739024)  
Regress: [mjsunit/regress/regress-1215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1215.js)  
```javascript
var desc = Object.getOwnPropertyDescriptor(Error.prototype, 'message');

assertEquals(desc.writable, true);
assertEquals(desc.enumerable, false);
assertEquals(desc.configurable, true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0fbf8c88^!)  
[test/mjsunit/regress/regress-1215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1215.js?cl=0fbf8c88)  
[test/mjsunit/regress/regress-1447.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1447.js?cl=0fbf8c88)  
  

---   

## **regress-1548.js (v8 issue)**  
   
**[The 'caller' and 'arguments' properties on built-in functions can't be neutered.](https://crbug.com/v8/1548)**  
**[Commit: Make arguments and caller always be null on native functions (fixes issue 1548 and issue 1643).](https://chromium.googlesource.com/v8/v8/+/4e94cd8)**  
  
Date(Commit): Thu Sep 01 11:09:11 2011  
Code Review: [http://codereview.chromium.org/7792054](http://codereview.chromium.org/7792054)  
Regress: [mjsunit/regress/regress-1548.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1548.js)  
```javascript
function testfn(f) { return [1].map(f)[0]; }
function foo() { return [].map.caller; }
assertThrows(function() { testfn(foo); } );

delete Array.prototype.map.caller;
assertThrows(function() { testfn(foo); } );

function testarguments(f) { return [1].map(f)[0]; }
function bar() { return [].map.arguments; }
assertThrows(function() { testarguments(bar); } );

delete Array.prototype.map.arguments;
assertThrows(function() { testarguments(bar); } );  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e94cd8^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=4e94cd8)  
[test/mjsunit/regress/regress-1548.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1548.js?cl=4e94cd8)  
  

---   

## **regress-1650.js (v8 issue)**  
   
**[Constant function check inserted for HApplyArguments deoptimizes into incorrect state](https://crbug.com/v8/1650)**  
**[Commit: Do constant function check earlier in TryCallApply and ensure correct environment for deopt.](https://chromium.googlesource.com/v8/v8/+/e833f91)**  
  
Date(Commit): Thu Sep 01 10:33:59 2011  
Code Review: [http://codereview.chromium.org/7812033](http://codereview.chromium.org/7812033)  
Regress: [mjsunit/regress/regress-1650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1650.js)  
```javascript
function g(f) {
  return f.call.apply(f.bind, arguments);
};
%PrepareFunctionForOptimization(g);
var x = new Object();

function t() {}

g(t, x);
g(t, x);
g(t, x);
%OptimizeFunctionOnNextCall(g);

function Fake() {}

var fakeCallInvoked = false;

Fake.prototype.call = function () {
  assertSame(Fake.prototype.bind, this);
  assertEquals(2, arguments.length);
  assertSame(fake, arguments[0]);
  assertSame(x, arguments[1]);
  fakeCallInvoked = true;
};

Fake.prototype.bind = function () {
};

var fake = new Fake();

g(fake, x);

assertTrue(fakeCallInvoked);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e833f91^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e833f91)  
[test/mjsunit/regress/regress-1650.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1650.js?cl=e833f91)  
  

---   

## **regress-fundecl.js (other issue)**  
   
**[Commit: Introduce local function declarations in Crankshaft and fix issue 1647.](https://chromium.googlesource.com/v8/v8/+/ffc6c7e)**  
  
Date(Commit): Wed Aug 31 13:26:08 2011  
Code Review: [http://codereview.chromium.org/7776009](http://codereview.chromium.org/7776009)  
Regress: [mjsunit/regress/regress-fundecl.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fundecl.js)  
```javascript
function h(a, b) {
  var r = a + b;
  function X() {
    return 42;
  }
  return r + X();
};
%PrepareFunctionForOptimization(h);
for (var i = 0; i < 5; i++) h(1, 2);

%OptimizeFunctionOnNextCall(h);

assertEquals(45, h(1, 2));
assertEquals("foo742", h("foo", 7));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ffc6c7e^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=ffc6c7e)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=ffc6c7e)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=ffc6c7e)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=ffc6c7e)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=ffc6c7e)  
...  
  
  
---   

## **regress-1647.js (v8 issue)**  
   
**[Immediately called function turns to undefined](https://crbug.com/v8/1647)**  
**[Commit: Introduce local function declarations in Crankshaft and fix issue 1647.](https://chromium.googlesource.com/v8/v8/+/ffc6c7e)**  
  
Date(Commit): Wed Aug 31 13:26:08 2011  
Code Review: [http://codereview.chromium.org/7776009](http://codereview.chromium.org/7776009)  
Regress: [mjsunit/regress/regress-1647.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1647.js)  
```javascript
var t = {foo: function() {}};

var f = function bar() {
  t.foo();
  assertEquals('function', typeof bar);
};
;
%PrepareFunctionForOptimization(f);
for (var i = 0; i < 10; i++) f();
%OptimizeFunctionOnNextCall(f);
t.number = 2;
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ffc6c7e^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=ffc6c7e)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=ffc6c7e)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=ffc6c7e)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=ffc6c7e)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=ffc6c7e)  
...  
  

---   

## **regress-1625.js (v8 issue)**  
   
**[Array.prototype.push = 1; breaks Object.defineProperties.](https://crbug.com/v8/1625)**  
**[Commit: Use InternalArray in Object.defineProperties to avoid issues with overwriten properties on Array.prototype](https://chromium.googlesource.com/v8/v8/+/d9c1984)**  
  
Date(Commit): Thu Aug 18 08:39:06 2011  
Code Review: [http://codereview.chromium.org/7631039](http://codereview.chromium.org/7631039)  
Regress: [mjsunit/regress/regress-1625.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1625.js)  
```javascript
Array.prototype.push = 1;
var desc = {foo: {value: 10}, bar: {get: function() {return 42; }}};
var obj = {};
var x = Object.defineProperties(obj, desc);
assertEquals(x.foo, 10);
assertEquals(x.bar, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9c1984^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=d9c1984)  
[test/mjsunit/regress/regress-1625.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1625.js?cl=d9c1984)  
  

---   

## **regress-1620.js (v8 issue)**  
   
**[V8 allows escapes in identifiers that are not unicode escapes](https://crbug.com/v8/1620)**  
**[Commit: Make scanner not accept invalid unicode escapes in identifiers.](https://chromium.googlesource.com/v8/v8/+/7d17c8d)**  
  
Date(Commit): Tue Aug 16 13:31:08 2011  
Code Review: [http://codereview.chromium.org/7663005](http://codereview.chromium.org/7663005)  
Regress: [mjsunit/regress/regress-1620.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1620.js)  
```javascript
assertThrows("var \\u\\u\\u = 42;");
assertThrows("var \\u41 = 42;");
assertThrows("var \\u123 = 42;");
eval("var \\u1234 = 42;");
assertEquals(42, eval("\u1234"));
assertThrows("var uuu = 42; var x = \\u\\u\\u");


assertEquals(0xFFFD, "\uFFFD".charCodeAt(0));

assertThrows("/x/g\\uim", SyntaxError);
assertThrows("/x/g\\u2im", SyntaxError);
assertThrows("/x/g\\u22im", SyntaxError);
assertThrows("/x/g\\u222im", SyntaxError);
assertThrows("/x/g\\\\u2222im", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d17c8d^!)  
[src/scanner-base.cc](https://cs.chromium.org/chromium/src/v8/src/scanner-base.cc?cl=7d17c8d)  
[src/scanner-base.h](https://cs.chromium.org/chromium/src/v8/src/scanner-base.h?cl=7d17c8d)  
[test/mjsunit/regress/regress-1620.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1620.js?cl=7d17c8d)  
  

---   

## **regress-1592.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1592)**  
**[Commit: Fix fun.apply(receiver, arguments) optimization.](https://chromium.googlesource.com/v8/v8/+/a107387)**  
  
Date(Commit): Wed Aug 10 16:05:17 2011  
Code Review: [http://codereview.chromium.org/7497067](http://codereview.chromium.org/7497067)  
Regress: [mjsunit/regress/regress-1592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1592.js)  
```javascript
var f = {apply: function(a, b) {}};

function test(a) {
  f.apply(this, arguments);
}

;
%PrepareFunctionForOptimization(test);
test(1);
test(1);

%OptimizeFunctionOnNextCall(test);

test(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a107387^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=a107387)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=a107387)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=a107387)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=a107387)  
[test/mjsunit/regress/regress-1592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1592.js?cl=a107387)  
  

---   

## **regress-91787.js (chromium issue)**  
   
**[Stack overflow with JSON decoding](https://crbug.com/91787)**  
**[Commit: Avoid infinite recursion for unterminated non-ASCII JSON string literals.](https://chromium.googlesource.com/v8/v8/+/e9bc76c)**  
  
Date(Commit): Fri Aug 05 12:55:29 2011  
Components: Internals  
Labels: ["Hotlist-Recharge", "Hotlist-Recharge-Stale"]  
Code Review: [http://codereview.chromium.org/7569008](http://codereview.chromium.org/7569008)  
Regress: [mjsunit/regress/regress-91787.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91787.js)  
```javascript
assertThrows(function() {
  JSON.parse('"\x80unterminated');
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e9bc76c^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=e9bc76c)  
[test/mjsunit/regress/regress-91787.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91787.js?cl=e9bc76c)  
  

---   

## **regress-1546.js (v8 issue)**  
   
**[c-style comments is closed unexpectedly by some characters](https://crbug.com/v8/1546)**  
**[Commit: Fix bug in scanner.](https://chromium.googlesource.com/v8/v8/+/61ae1be)**  
  
Date(Commit): Fri Aug 05 11:21:04 2011  
Code Review: [http://codereview.chromium.org/7585004](http://codereview.chromium.org/7585004)  
Regress: [mjsunit/regress/regress-1546.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1546.js)  
```javascript
eval("/*\u822a/ */");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/61ae1be^!)  
[src/scanner-base.cc](https://cs.chromium.org/chromium/src/v8/src/scanner-base.cc?cl=61ae1be)  
[test/mjsunit/regress/regress-1546.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1546.js?cl=61ae1be)  
  

---   

## **regress-1583.js (v8 issue)**  
   
**[Incorrect scope chain for hoisted functions in optimizing compiler.](https://crbug.com/v8/1583)**  
**[Commit: Fix a bug in scope analysis.](https://chromium.googlesource.com/v8/v8/+/b625ce2)**  
  
Date(Commit): Fri Aug 05 08:28:11 2011  
Code Review: [http://codereview.chromium.org/7572019](http://codereview.chromium.org/7572019)  
Regress: [mjsunit/regress/regress-1583.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1583.js)  
```javascript
function f() {
  try {
    throw 0;
  } catch (e) {
    try {
      var x = { a: 'hest' };
      x.m = function (e) { return x.a; };
    } catch (e) {
    }
  }
  return x;
}

var o = f();
%PrepareFunctionForOptimization(o.m);
assertEquals('hest', o.m());
assertEquals('hest', o.m());
assertEquals('hest', o.m());
%OptimizeFunctionOnNextCall(o.m);
assertEquals('hest', o.m());

var global = 'horse';
var p = { get global() { return global; }};
assertEquals('horse', p.global);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b625ce2^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=b625ce2)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=b625ce2)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=b625ce2)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=b625ce2)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=b625ce2)  
...  
  

---   

## **regress-1419.js (v8 issue)**  
   
**[Binded function's length changes after applying bind to other function](https://crbug.com/v8/1419)**  
**[Commit: Ensure that the length property of bound functions are actual unique](https://chromium.googlesource.com/v8/v8/+/9721edd)**  
  
Date(Commit): Wed Aug 03 12:44:17 2011  
Code Review: [http://codereview.chromium.org/7475002](http://codereview.chromium.org/7475002)  
Regress: [mjsunit/regress/regress-1419.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1419.js)  
```javascript
function foo() {
}

var f1 = function (x) {}.bind(foo);
var f2 = function () {};

assertEquals(1, f1.length);

f2.bind(foo);

assertEquals(1, f1.length);

var desc = Object.getOwnPropertyDescriptor(f1, 'length');
assertEquals(false, desc.writable);
assertEquals(false, desc.enumerable);
assertEquals(true, desc.configurable);

Object.defineProperty(f1, 'length', {
  value: 'abc',
  writable: true
});
assertEquals('abc', f1.length);
f1.length = 42;
assertEquals(42, f1.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9721edd^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9721edd)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=9721edd)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=9721edd)  
[test/mjsunit/regress/regress-1419.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1419.js?cl=9721edd)  
  

---   

## **regress-lbranch-double.js (other issue)**  
   
**[Commit: Fixed code generation for LBranch on ARM when the operand's representation is double.](https://chromium.googlesource.com/v8/v8/+/6f6c882)**  
  
Date(Commit): Tue Aug 02 15:14:12 2011  
Code Review: [http://codereview.chromium.org/7553010](http://codereview.chromium.org/7553010)  
Regress: [mjsunit/compiler/regress-lbranch-double.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-lbranch-double.js)  
```javascript
function foo() {
  return Math.sqrt(2.6415) ? 88 : 99;
}

%PrepareFunctionForOptimization(foo);
assertEquals(88, foo());
assertEquals(88, foo());
%OptimizeFunctionOnNextCall(foo)
assertEquals(88, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f6c882^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=6f6c882)  
[test/mjsunit/compiler/regress-lbranch-double.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-lbranch-double.js?cl=6f6c882)  
  
  
---   

## **regress-91120.js (chromium issue)**  
   
**[[LangFuzz] Crash at Runtime_QuoteJSONString with invalid write](https://crbug.com/91120)**  
**[Commit: Fix a bug in scope analysis.](https://chromium.googlesource.com/v8/v8/+/f37f6e8)**  
  
Date(Commit): Tue Aug 02 15:04:31 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "ReleaseBlock-Beta", "CVE-2011-2852", "reward-500", "Merge-Merged", "Security_Impact-Stable", "M-14", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/7548011](http://codereview.chromium.org/7548011)  
Regress: [mjsunit/regress/regress-91120.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91120.js)  
```javascript
var x = 'global';

function f() {
  var x = 'function';
  assertEquals(undefined, g);
  try {
    assertEquals(undefined, g);
    throw 'catch';
  } catch (x) {
    function g() { return x; }
    assertEquals('catch', g());
  }
  assertEquals('catch', g());
  return g;
}

assertEquals('catch', f()());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f37f6e8^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=f37f6e8)  
[test/mjsunit/regress/regress-91120.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91120.js?cl=f37f6e8)  
  

---   

## **regress-91008.js (chromium issue)**  
   
**[[LangFuzz] Crash at JSObject::PrepareElementsForSort with invalid read](https://crbug.com/91008)**  
**[Commit: Properly handle FixedDoubleArrays in sort()](https://chromium.googlesource.com/v8/v8/+/b333719)**  
  
Date(Commit): Tue Aug 02 14:05:11 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-15", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7542008](http://codereview.chromium.org/7542008)  
Regress: [mjsunit/regress/regress-91008.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91008.js)  
```javascript
function testsort(n) {
  var numbers=new Array(n);
  for (var i=0;i<n;i++) numbers[i]=i;
  delete numbers[50];
  delete numbers[150];
  delete numbers[25000];
  delete numbers[n-1];
  delete numbers[n-2];
  delete numbers[30];
  delete numbers[2];
  delete numbers[1];
  delete numbers[0];
  numbers.sort();
}

testsort(100000)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b333719^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b333719)  
[test/mjsunit/regress/regress-91008.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91008.js?cl=b333719)  
  

---   

## **regress-91013.js (chromium issue)**  
   
**[[LangFuzz] Crash at RootMarkingVisitor::VisitPointers (32 bit)](https://crbug.com/91013)**  
**[Commit: Ensure that GenerateStoreFastDoubleElement returns stored value on all paths.](https://chromium.googlesource.com/v8/v8/+/9226cfe)**  
  
Date(Commit): Tue Aug 02 13:36:38 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-15", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7551009](http://codereview.chromium.org/7551009)  
Regress: [mjsunit/regress/regress-91013.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91013.js)  
```javascript
var i = 100000;
var a = new Array(i);
for (var j = 0; j < i; j++) {
  a[j] = 0.5;
}

assertTrue(%HasDoubleElements(a));

for (var j = 0; j < 10; j++) {
  assertEquals(j, a[j] = j);
}

for (var j = 0; j < 10; j++) {
  var v = j + 0.5;
  assertEquals(v, a[j] = v);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9226cfe^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=9226cfe)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=9226cfe)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=9226cfe)  
[test/mjsunit/regress/regress-91013.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91013.js?cl=9226cfe)  
  

---   

## **regress-1582.js (v8 issue)**  
   
**[Maximum call stack size exceeded error with SproutCore apps](https://crbug.com/v8/1582)**  
**[Commit: Check for phi-uses of arguments object before eliminating dead phi's.](https://chromium.googlesource.com/v8/v8/+/a547d33)**  
  
Date(Commit): Tue Aug 02 09:32:28 2011  
Code Review: [http://codereview.chromium.org/7553006](http://codereview.chromium.org/7553006)  
Regress: [mjsunit/regress/regress-1582.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1582.js)  
```javascript
function f(restIsArray, rest) {
  var arr;
  if (typeof rest === "object" && (rest instanceof Array)) {
    arr = rest;
  } else {
    arr = arguments;
  }
  var i = arr.length;
  while (--i >= 0) arr[i];
  var arrIsArguments = (arr[1] !== rest);
  assertEquals(restIsArray, arrIsArguments);
}
%PrepareFunctionForOptimization(f);

%PrepareFunctionForOptimization(f);
f(false, 'b', 'c');
f(false, 'b', 'c');
f(false, 'b', 'c');
%OptimizeFunctionOnNextCall(f);
f(true, ['b', 'c']);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a547d33^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=a547d33)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=a547d33)  
[test/mjsunit/regress/regress-1582.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1582.js?cl=a547d33)  
  

---   

## **regress-91010.js (chromium issue)**  
   
**[[LangFuzz] Crash at JSObject::SetDictionaryElement with invalid read (32 bit)](https://crbug.com/91010)**  
**[Commit: Properly handle FastDoubleArrays in Runtime_MoveArrayContents](https://chromium.googlesource.com/v8/v8/+/008f834)**  
  
Date(Commit): Tue Aug 02 09:28:55 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-15", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7551004](http://codereview.chromium.org/7551004)  
Regress: [mjsunit/regress/regress-91010.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91010.js)  
```javascript
try {
  try {
    var N = 100*1000;
    var array = Array(N);
    for (var i = 0; i != N; ++i)
      array[i] = i;
  } catch(ex) {}
  array.unshift('Kibo');
} catch(ex) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/008f834^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=008f834)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=008f834)  
[test/mjsunit/regress/regress-91010.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91010.js?cl=008f834)  
  

---   

## **regress-1563.js (v8 issue)**  
   
**[Arm movt translation bug in DoClampTToUint8](https://crbug.com/v8/1563)**  
**[Commit: Fix bug in ARM pixel array clamping](https://chromium.googlesource.com/v8/v8/+/1f9801b)**  
  
Date(Commit): Fri Jul 22 16:01:53 2011  
Code Review: [http://codereview.chromium.org/7461028](http://codereview.chromium.org/7461028)  
Regress: [mjsunit/regress/regress-1563.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1563.js)  
```javascript
obj = new Uint8ClampedArray(10);

function set_pixel(obj, arg) {
  obj[0] = arg;
};
%PrepareFunctionForOptimization(set_pixel);
set_pixel(obj, 1.5);
set_pixel(obj, NaN);
%OptimizeFunctionOnNextCall(set_pixel);
set_pixel(obj, undefined);
set_pixel(obj, undefined);

assertEquals(0, obj[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f9801b^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=1f9801b)  
[test/mjsunit/regress/regress-1563.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1563.js?cl=1f9801b)  
  

---   

## **regress-1560.js (v8 issue)**  
   
**[Optimized code for polymorphic keyed stores does not handle COW backing stores.](https://crbug.com/v8/1560)**  
**[Commit: Add map check for COW elements to crankshaft array handling code.](https://chromium.googlesource.com/v8/v8/+/d477928)**  
  
Date(Commit): Thu Jul 14 14:45:20 2011  
Code Review: [http://codereview.chromium.org/7366008](http://codereview.chromium.org/7366008)  
Regress: [mjsunit/regress/regress-1560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1560.js)  
```javascript
function mkCOWArray() {
  var a = [''];
  assertEquals('', a[0]);
  return a;
}

function mkArray() {
  var a = [];
  a[0] = '';
  return a;
}

function mkNumberDictionary() {
  var a = new Array();
  a[0] = '';
  a[100000] = '';
  return a;
}

function write(a, i) { a[i] = "bazinga!"; }
%PrepareFunctionForOptimization(write);

function test(factories, w) {
  %PrepareFunctionForOptimization(w);
  factories.forEach(function(f) { w(f(), 0); });
  factories.forEach(function(f) { w(f(), 0); });
  %OptimizeFunctionOnNextCall(w);
  factories.forEach(function(f) { w(f(), 0); });
}

for (var i = 0; i < 5; i++) write(mkArray(), 0);
%OptimizeFunctionOnNextCall(write);
write(mkCOWArray(), 0);
var failure = mkCOWArray();

%DeoptimizeFunction(write);
gc();
test([mkArray, mkNumberDictionary], write);
test([mkArray, mkNumberDictionary, mkCOWArray], write);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d477928^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=d477928)  
[test/mjsunit/regress/regress-1560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1560.js?cl=d477928)  
  

---   

## **regress-88591.js (chromium issue)**  
   
**[[LangFuzz] CHECK(!value->IsTheHole()) failed // Crash with invalid read in shell](https://crbug.com/88591)**  
**[Commit: Fix a potential crash in const declaration.](https://chromium.googlesource.com/v8/v8/+/890bc16)**  
  
Date(Commit): Mon Jul 11 14:07:12 2011  
Components: Blink>JavaScriptBlink  
Labels: ["CVE-2011-2802", "Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-13", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [http://codereview.chromium.org/7324048](http://codereview.chromium.org/7324048)  
Regress: [mjsunit/regress/regress-88591.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-88591.js)  
```javascript
var called = false;
Object.prototype.__defineSetter__('x', function(x) { called = true; });
Object.prototype.__defineGetter__('x', function () { return 0; });

this.__proto__ = { x: 1 };

try { fail; } catch (e) { eval('var x = 2'); }

var o = Object.getOwnPropertyDescriptor(this, 'x');
assertFalse(called);
assertEquals(2, o.value);
assertEquals(true, o.writable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/890bc16^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=890bc16)  
[test/mjsunit/regress/regress-88591.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-88591.js?cl=890bc16)  
  

---   

## **regress-88858.js (chromium issue)**  
   
**[[LangFuzz] Crash at JSObject::LocalLookupRealNamedProperty with invalid read on gc](https://crbug.com/88858)**  
**[Commit: Allow JSObject::PreventExtensions to work for arguments objects.](https://chromium.googlesource.com/v8/v8/+/cbaf1bc)**  
  
Date(Commit): Mon Jul 11 06:48:19 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-14", "HasTestcase", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7335002](http://codereview.chromium.org/7335002)  
Regress: [mjsunit/regress/regress-88858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-88858.js)  
```javascript
try {
    function make_watcher(name) { }
    var o, p;
    function f(flag) {
        if (flag) {
            o = arguments;
        } else {
            p = arguments;
            o.watch(0, (arguments-1901)('o'));
            p.watch(0, make_watcher('p'));
            p.unwatch(0);
            o.unwatch(0);
            p[0] = 4;
            assertEq(flag, 4);
        }
    }
    f(true);
    f(false);
    reportCompare(true, true);
} catch(exc1) { }

try {
    function __noSuchMethod__() {
       if (anonymous == "1")
           return NaN;
       return __construct__;
    }
    f.p = function() { };
    Object.freeze(p);
    new new freeze().p;
    reportCompare(0, 0, "ok");
} catch(exc2) { }

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cbaf1bc^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=cbaf1bc)  
[test/mjsunit/regress/regress-88858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-88858.js?cl=cbaf1bc)  
  

---   

## **regress-1531.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1531)**  
**[Commit: Fix a bug in for/in iteration of arguments objects.](https://chromium.googlesource.com/v8/v8/+/fe23339b)**  
  
Date(Commit): Fri Jul 08 07:31:48 2011  
Code Review: [http://codereview.chromium.org/7278033](http://codereview.chromium.org/7278033)  
Regress: [mjsunit/regress/regress-1531.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1531.js)  
```javascript
function test(x) {
  arguments[10] = 0;
  var arr = [];
  for (var p in arguments) arr.push(p);
  return arr;
}
assertEquals(["0", "10"], test(0));

function test1(x, y, z) {
  // Put into dictionary mode.
  arguments.__defineGetter__("5", function () { return 0; });
  // Delete a property from the dictionary.
  delete arguments[5];
  // Look up a property in the dictionary.
  return arguments[2];
}

assertEquals(void 0, test1(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe23339b^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=fe23339b)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=fe23339b)  
[test/mjsunit/regress/regress-1531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1531.js?cl=fe23339b)  
  

---   

## **regress-regexp-codeflush.js (other issue)**  
   
**[Commit: Ensure that regexps always have code object, even if GC happened while running multiple times in runtime.](https://chromium.googlesource.com/v8/v8/+/82e5327)**  
  
Date(Commit): Thu Jul 07 10:04:56 2011  
Code Review: [http://codereview.chromium.org/7316018](http://codereview.chromium.org/7316018)  
Regress: [mjsunit/regress/regress-regexp-codeflush.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-regexp-codeflush.js)  
```javascript
var re = new RegExp('(s)', "g");

function foo() {
  return "42";
}

for ( var i = 0; i < 10; i++) {
  // Make a long string with plenty of matches for re.
  var x = "s foo s bar s foo s bar s";
  x = x + x;
  x = x + x;
  x = x + x;
  x = x + x;
  x = x + x;
  x = x + x;
  x = x + x;
  x.replace(re, foo);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82e5327^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=82e5327)  
[test/mjsunit/regress/regress-regexp-codeflush.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-regexp-codeflush.js?cl=82e5327)  
  
  
---   

## **regress-1529.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1529)**  
**[Commit: Fix bug 1529: check for NULL handle in v8::TryCatch::StackTrace.](https://chromium.googlesource.com/v8/v8/+/8f60208)**  
  
Date(Commit): Mon Jul 04 13:29:56 2011  
Code Review: [http://codereview.chromium.org/7309004](http://codereview.chromium.org/7309004)  
Regress: [mjsunit/regress/regress-1529.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1529.js)  
```javascript
try {
  Error.prepareStackTrace = function (error, stackTrace) {
    stackTrace.some();
  };
  x;
} catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8f60208^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=8f60208)  
[test/mjsunit/regress/regress-1529.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1529.js?cl=8f60208)  
  

---   

## **regress-1528.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1528)**  
**[Commit: Fix a bug in with and catch context allocation.](https://chromium.googlesource.com/v8/v8/+/57c29c1)**  
  
Date(Commit): Mon Jul 04 09:34:47 2011  
Code Review: [http://codereview.chromium.org/7309002](http://codereview.chromium.org/7309002)  
Regress: [mjsunit/regress/regress-1528.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1528.js)  
```javascript
try {
  fail;
} catch (e) {
  with({}) {  // With scope inside catch scope.
    // Dynamic declaration forces runtime lookup to observe the context chain.
    eval('const x = 7');
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/57c29c1^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=57c29c1)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=57c29c1)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=57c29c1)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=57c29c1)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=57c29c1)  
...  
  

---   

## **regress-1521.js (v8 issue)**  
   
**[Optimizing compiler incorrectly resolves variable reference from closure created in the catch clause.](https://crbug.com/v8/1521)**  
**[Commit: Fix an issue with optimization of functions inside catch.](https://chromium.googlesource.com/v8/v8/+/a48c03b)**  
  
Date(Commit): Fri Jul 01 14:05:46 2011  
Code Review: [http://codereview.chromium.org/7285032](http://codereview.chromium.org/7285032)  
Regress: [mjsunit/regress/regress-1521.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1521.js)  
```javascript
function test(x) {
  try {
    throw new Error();
  } catch (e) {
    var y = {f: 1};
    var f = function() {
      var z = y;
      var g = function() {
        if (y.f === z.f) return x;
      };
      ;
      %PrepareFunctionForOptimization(g);
      %OptimizeFunctionOnNextCall(g);
      return g;
    };
    assertEquals(3, f()());
  }
}

test(3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a48c03b^!)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=a48c03b)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=a48c03b)  
[test/mjsunit/regress/regress-1521.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1521.js?cl=a48c03b)  
  

---   

## **regress-1360.js (v8 issue)**  
   
**[Don't use Global receiver for calls in Array.prototype.sort and String.prototype.replace](https://crbug.com/v8/1360)**  
**[Commit: Do not pass the global object as the receiver to strict-mode and](https://chromium.googlesource.com/v8/v8/+/0d8c343)**  
  
Date(Commit): Thu Jun 30 12:29:19 2011  
Code Review: [http://codereview.chromium.org/7283006](http://codereview.chromium.org/7283006)  
Regress: [mjsunit/regress/regress-1360.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1360.js)  
```javascript
var global = this;
function strict() { "use strict"; assertEquals(void 0, this); }
function non_strict() { assertEquals(global, this); }

[1,2,3].sort(strict);
[1,2,3].sort(non_strict);

"axc".replace("x", strict);
"axc".replace("x", non_strict);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0d8c343^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=0d8c343)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=0d8c343)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=0d8c343)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=0d8c343)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=0d8c343)  
...  
  

---   

## **regress-1513.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1513)**  
**[Commit: Fix a bug in Object.defineProperty.](https://chromium.googlesource.com/v8/v8/+/3f84fcf)**  
  
Date(Commit): Thu Jun 30 11:11:19 2011  
Code Review: [http://codereview.chromium.org/7289011](http://codereview.chromium.org/7289011)  
Regress: [mjsunit/regress/regress-1513.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1513.js)  
```javascript
function testcase() {
  return (function (a, b, c) {
      delete arguments[0];
      Object.defineProperty(arguments, "0", {
              value: 10,
              writable: false,
              enumerable: false,
              configurable: false
            });
      assertEquals(10, arguments[0]);
    }(0, 1, 2));
}

testcase();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f84fcf^!)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=3f84fcf)  
[src/handles.h](https://cs.chromium.org/chromium/src/v8/src/handles.h?cl=3f84fcf)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3f84fcf)  
[test/mjsunit/regress/regress-1513.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1513.js?cl=3f84fcf)  
  

---   

## **regress-crbug-87478.js (chromium issue)**  
   
**[[LangFuzz] Crash on heap with invalid read](https://crbug.com/87478)**  
**[Commit: Fix receiver check in arguments ICs.](https://chromium.googlesource.com/v8/v8/+/89cc886)**  
  
Date(Commit): Mon Jun 27 13:02:51 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-14", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7259015](http://codereview.chromium.org/7259015)  
Regress: [mjsunit/regress/regress-crbug-87478.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-87478.js)  
```javascript
function f(array) { return array[0]; }
function args(a) { return arguments; }
for (var i = 0; i < 10; i++) {
  f(args(1));
}
f('123');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89cc886^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=89cc886)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=89cc886)  
[src/x64/ic-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/ic-x64.cc?cl=89cc886)  
[test/mjsunit/regress/regress-crbug-87478.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-87478.js?cl=89cc886)  
  

---   

## **regress-1491.js (v8 issue)**  
   
**[Property validation and shadowing in objects with array prototypes](https://crbug.com/v8/1491)**  
**[Commit: Correctly handle non-array receivers in Array length setter.](https://chromium.googlesource.com/v8/v8/+/a96b915)**  
  
Date(Commit): Tue Jun 21 08:07:45 2011  
Code Review: [http://codereview.chromium.org/7206038](http://codereview.chromium.org/7206038)  
Regress: [mjsunit/regress/regress-1491.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1491.js)  
```javascript
var o = Object.create([]);

var value = "asdf";
o.length = value;
assertEquals(value, o.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a96b915^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=a96b915)  
[test/mjsunit/regress/regress-1491.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1491.js?cl=a96b915)  
  

---   

## **regress-1472.js (v8 issue)**  
   
**[RegExp causes OOM](https://crbug.com/v8/1472)**  
**[Commit: Refix issue 1472.  The previous fix worked for the example in the bug](https://chromium.googlesource.com/v8/v8/+/c95ecb1)**  
  
Date(Commit): Fri Jun 17 08:01:12 2011  
Code Review: [http://codereview.chromium.org/7193007](http://codereview.chromium.org/7193007)  
Regress: [mjsunit/regress/regress-1472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1472.js)  
```javascript
var r1 = /(?:a(?:b(?:c(?:d(?:e(?:f(?:g(?:h(?:i(?:j(?:k(?:l(?:m(?:n(?:o(?:p(?:q(?:r(?:s(?:t(?:u(?:v(?:w(?:x(?:y(?:z(?:FooBar)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)+)/;
"xxx".match(r1);

var r2 = /(?:a(?:b(?:c(?:d(?:e(?:f(?:g(?:h(?:i(?:j(?:k(?:l(?:FooBar){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}){0,2}/;
"xxx".match(r2);

var r3 = /(?:a(?:b(?:c(?:d(?:e(?:f(?:g(?:h(?:i(?:j(?:k(?:l(?:FooBar){2}){2}){2}){2}){2}){2}){2}){2}){2}){2}){2}){2}){2}/;
"xxx".match(r3);

var r4 = /(?:a(?:b(?:c(?:d(?:e(?:f(?:g(?:h(?:i(?:FooBar){3,6}){3,6}){3,6}){3,6}){3,6}){3,6}){3,6}){3,6}){3,6}){3,6}/;
"xxx".match(r4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c95ecb1^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=c95ecb1)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=c95ecb1)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=c95ecb1)  
[test/mjsunit/regress/regress-1472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1472.js?cl=c95ecb1)  
  

---   

## **regress-1476.js (v8 issue)**  
   
**[Incorrect code generated for LModI with power-of-2 divisor](https://crbug.com/v8/1476)**  
**[Commit: Add missing branches in code generated for LModI with power-of-2 divisor.](https://chromium.googlesource.com/v8/v8/+/14bf246)**  
  
Date(Commit): Wed Jun 15 19:57:39 2011  
Code Review: [http://codereview.chromium.org/7097015](http://codereview.chromium.org/7097015)  
Regress: [mjsunit/regress/regress-1476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1476.js)  
```javascript
function foo(i) {
  return i % 2 | 0;
};
%PrepareFunctionForOptimization(foo);
assertEquals(-1, foo(-1));
assertEquals(-1, foo(-1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(-1, foo(-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14bf246^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=14bf246)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=14bf246)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=14bf246)  
[test/mjsunit/regress/regress-1476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1476.js?cl=14bf246)  
  

---   

## **regress-794.js (v8 issue)**  
   
**[Function.prototype.bind created function has prototype property](https://crbug.com/v8/794)**  
**[Commit: Ensure that bound functions does not have a prototype (fixes issue 794)](https://chromium.googlesource.com/v8/v8/+/23d0aa6)**  
  
Date(Commit): Wed Jun 15 10:47:37 2011  
Code Review: [http://codereview.chromium.org/7148014](http://codereview.chromium.org/7148014)  
Regress: [mjsunit/regress/regress-794.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-794.js)  
```javascript
function foo() {}
assertFalse("prototype" in foo.bind());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/23d0aa6^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=23d0aa6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=23d0aa6)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=23d0aa6)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=23d0aa6)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=23d0aa6)  
...  
  

---   

## **regress-1447.js (v8 issue)**  
   
**[Can't freeze forEach while forEach call is in progress](https://crbug.com/v8/1447)**  
**[Commit: Fix issue 1447 by not redefining properties unneccesarily in seal and freeze.](https://chromium.googlesource.com/v8/v8/+/aa7ad8e)**  
  
Date(Commit): Fri Jun 10 09:45:02 2011  
Code Review: [http://codereview.chromium.org/7044104](http://codereview.chromium.org/7044104)  
Regress: [mjsunit/regress/regress-1447.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1447.js)  
```javascript
[0].forEach(function(){ Object.freeze(Array.prototype.forEach); });
[0].every(function(){ Object.seal(Array.prototype.every); });

function testStrict(){
  "use strict";
  [0].forEach(function(){ Object.freeze(Array.prototype.forEach); });
  [0].every(function(){ Object.seal(Array.prototype.every); });
}

testStrict();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa7ad8e^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=aa7ad8e)  
[test/mjsunit/regress/regress-1447.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1447.js?cl=aa7ad8e)  
  

---   

## **osr-regress-max-locals.js (other issue)**  
   
**[Commit: Correct the limit of local variables in a optimized functions.](https://chromium.googlesource.com/v8/v8/+/8ec22db)**  
  
Date(Commit): Thu Jun 09 14:52:58 2011  
Code Review: [http://codereview.chromium.org/6995108](http://codereview.chromium.org/6995108)  
Regress: [mjsunit/compiler/osr-regress-max-locals.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/osr-regress-max-locals.js)  
```javascript
var limit = %RunningInSimulator() ? 10000 : 10000000;

function f() {
  var a1, a2, a3, a4, a5, a6, a7, a8, a9, a10,
      a11, a12, a13, a14, a15, a16, a17, a18, a19, a20,
      a21, a22, a23, a24, a25, a26, a27, a28, a29, a30,
      a31, a32, a33, a34, a35, a36, a37, a38, a39, a40,
      a41, a42, a43, a44, a45, a46, a47, a48, a49, a50,
      a51, a52, a53, a54, a55, a56, a57, a58, a59, a60,
      a61, a62, a63, a64;
  for (a1 = 0; a1 < limit; a1++) a2 = 23;
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ec22db^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=8ec22db)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=8ec22db)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=8ec22db)  
[test/mjsunit/compiler/regress-max-locals-for-osr.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-max-locals-for-osr.js?cl=8ec22db)  
  
  
---   

## **regress-1436.js (v8 issue)**  
   
**[reduce and reduceRight bind callback's this to null rather than undefined.](https://crbug.com/v8/1436)**  
**[Commit: Fix Array.prototype.{reduce,reduceRight} to pass undefined as receiver for strict mode callbacks.](https://chromium.googlesource.com/v8/v8/+/626cdff)**  
  
Date(Commit): Thu Jun 09 09:05:15 2011  
Code Review: [http://codereview.chromium.org/7044054](http://codereview.chromium.org/7044054)  
Regress: [mjsunit/regress/regress-1436.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1436.js)  
```javascript
var global = this;
function non_strict(){ assertEquals(global, this); }
function strict(){ "use strict"; assertEquals(void 0, this); }
function strict_null(){ "use strict"; assertEquals(null, this); }

[2, 3].reduce(non_strict);
[2, 3].reduce(strict);
[2, 3].reduceRight(non_strict);
[2, 3].reduceRight(strict);


[2, 3].every(non_strict);
[2, 3].every(non_strict, undefined);
[2, 3].every(non_strict, null);
[2, 3].every(strict);
[2, 3].every(strict, undefined);
[2, 3].every(strict_null, null);

[2, 3].filter(non_strict);
[2, 3].filter(non_strict, undefined);
[2, 3].filter(non_strict, null);
[2, 3].filter(strict);
[2, 3].filter(strict, undefined);
[2, 3].filter(strict_null, null);

[2, 3].forEach(non_strict);
[2, 3].forEach(non_strict, undefined);
[2, 3].forEach(non_strict, null);
[2, 3].forEach(strict);
[2, 3].forEach(strict, undefined);
[2, 3].forEach(strict_null, null);

[2, 3].map(non_strict);
[2, 3].map(non_strict, undefined);
[2, 3].map(non_strict, null);
[2, 3].map(strict);
[2, 3].map(strict, undefined);
[2, 3].map(strict_null, null);

[2, 3].some(non_strict);
[2, 3].some(non_strict, undefined);
[2, 3].some(non_strict, null);
[2, 3].some(strict);
[2, 3].some(strict, undefined);
[2, 3].some(strict_null, null);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/626cdff^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=626cdff)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=626cdff)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=626cdff)  
[src/preparse-data-format.h](https://cs.chromium.org/chromium/src/v8/src/preparse-data-format.h?cl=626cdff)  
[src/preparse-data.h](https://cs.chromium.org/chromium/src/v8/src/preparse-data.h?cl=626cdff)  
...  
  

---   

## **regress-1434.js (v8 issue)**  
   
**[Crankshafted === specialized for doubles doesn't handle undefined correctly](https://crbug.com/v8/1434)**  
**[Commit: Add failing test case for bug 1434](https://chromium.googlesource.com/v8/v8/+/ad98d14)**  
  
Date(Commit): Wed Jun 08 07:45:37 2011  
Code Review: [http://codereview.chromium.org/7131006](http://codereview.chromium.org/7131006)  
Regress: [mjsunit/regress/regress-1434.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1434.js)  
```javascript
function compare(a, b) {
  return a === b;
};
%PrepareFunctionForOptimization(compare);
compare(1.5, 2.5);
%OptimizeFunctionOnNextCall(compare);
assertTrue(compare(undefined, undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad98d14^!)  
[test/mjsunit/bugs/bug-1434.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-1434.js?cl=ad98d14)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=ad98d14)  
  

---   

## **regress-1423.js (v8 issue)**  
   
**["use strict" causes page to crash](https://crbug.com/v8/1423)**  
**[Commit: Fix a bug in Lithium environment iteration.](https://chromium.googlesource.com/v8/v8/+/6a81642)**  
  
Date(Commit): Mon Jun 06 11:30:17 2011  
Code Review: [http://codereview.chromium.org/6993023](http://codereview.chromium.org/6993023)  
Regress: [mjsunit/regress/regress-1423.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1423.js)  
```javascript
"use strict";

function f0() {
  return f1('literal', true);
};
%PrepareFunctionForOptimization(f0);
function f1(x, y) {
  return f2(x, y);
}

function f2(x, y) {
  if (y) {
    if (f3(x, 'other-literal')) {
      return 0;
    } else {
      return 1;
    }
  } else {
    return 2;
  }
}

function f3(x, y) {
  return x === y;
}

for (var i = 0; i < 5; ++i) f0();
%OptimizeFunctionOnNextCall(f0);
assertEquals(1, f0());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6a81642^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=6a81642)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=6a81642)  
[src/lithium-allocator-inl.h](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator-inl.h?cl=6a81642)  
[src/lithium-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.cc?cl=6a81642)  
[src/lithium-allocator.h](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.h?cl=6a81642)  
...  
  

---   

## **regress-84234.js (chromium issue)**  
   
**[[LangFuzz] Crash @ MarkCompactCollector::SweepSpaces() or SeqTwoByteString::SeqTwoByteStringReadBlockIntoBuffer() (64 bit)](https://crbug.com/84234)**  
**[Commit: Fix traversal of the map transition tree to take the prototype](https://chromium.googlesource.com/v8/v8/+/0023cac)**  
  
Date(Commit): Fri Jun 03 14:48:09 2011  
Components: InternalsBlink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-12", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/7074052](http://codereview.chromium.org/7074052)  
Regress: [mjsunit/regress/regress-84234.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-84234.js)  
```javascript
var gTestcases = new Array();

function TestCase(n, d, e, a) {
  gTestcases[gTc++] = this;
  for ( gTc=0; gTc < gTestcases.length; gTc++ );
}

for ( var i = 0x0530; i <= 0x058F; i++ ) {
  new TestCase("15.5.4.11-6",
               eval("var s = new String(String.fromCharCode(i)); s.toLowerCase().charCodeAt(0)"));
}
var gTc= 0;


for (var j = 0; j < 10; j++) {
  test();
  function test() {
    for ( 0; gTc < gTestcases.length; gTc++ ) {
      var MYOBJECT = new MyObject();
    }
    gc();
  }
  function MyObject( n ) {
    this.__proto__ = Number.prototype;
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0023cac^!)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=0023cac)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0023cac)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=0023cac)  
[test/mjsunit/regress/regress-84234.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-84234.js?cl=0023cac)  
  

---   

## **regress-1412.js (v8 issue)**  
   
**[Apply with arguments optimization needs updating for ES5](https://crbug.com/v8/1412)**  
**[Commit: Fix a number of IC stubs to correctly set the call kind.](https://chromium.googlesource.com/v8/v8/+/cc4a2d7)**  
  
Date(Commit): Mon May 30 13:23:17 2011  
Code Review: [http://codereview.chromium.org/7086029](http://codereview.chromium.org/7086029)  
Regress: [mjsunit/regress/regress-1412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1412.js)  
```javascript
function strict() {
  'use strict';
  return this;
}

function test_strict() {
  assertEquals(void 0, strict.apply(undefined, arguments));
  assertEquals(42, strict.apply(42, arguments));
  assertEquals("asdf", strict.apply("asdf", arguments));
};
%PrepareFunctionForOptimization(test_strict);
for (var i = 0; i < 10; i++) test_strict();
%OptimizeFunctionOnNextCall(test_strict);
test_strict();

function test_builtin(receiver) {
  Object.prototype.valueOf.apply(receiver, arguments);
};
%PrepareFunctionForOptimization(test_builtin);
for (var i = 0; i < 10; i++) test_builtin(this);
%OptimizeFunctionOnNextCall(test_builtin);
test_builtin(this);

var exception = false;
try {
  test_builtin(undefined);
} catch (e) {
  exception = true;
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc4a2d7^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=cc4a2d7)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=cc4a2d7)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=cc4a2d7)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=cc4a2d7)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=cc4a2d7)  
...  
  

---   

## **regress-const.js (other issue)**  
   
**[Commit: Simple support for const variables in Crankshaft.](https://chromium.googlesource.com/v8/v8/+/e098588)**  
  
Date(Commit): Mon May 30 11:31:41 2011  
Code Review: [http://codereview.chromium.org/6026006](http://codereview.chromium.org/6026006)  
Regress: [mjsunit/compiler/regress-const.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-const.js)  
```javascript
function f() {
  var x = 42;
  while (true) {
    const y = x;
    if (--x == 0) return y;
  }
}

function g() {
  const x = 42;
  return x;
}

%PrepareFunctionForOptimization(f);
for (var i = 0; i < 5; i++) {
  f();
}
%OptimizeFunctionOnNextCall(f);

assertEquals(1, f());

%PrepareFunctionForOptimization(g);
for (var i = 0; i < 5; i++) {
  g();
}
%OptimizeFunctionOnNextCall(g);

assertEquals(42, g());


function h(a, b) {
  var r = a + b;
  const X = 42;
  return r + X;
}

%PrepareFunctionForOptimization(h);

for (var i = 0; i < 5; i++) h(1,2);

%OptimizeFunctionOnNextCall(h);

assertEquals(45, h(1,2));
assertEquals("foo742", h("foo", 7));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e098588^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=e098588)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=e098588)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e098588)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=e098588)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=e098588)  
...  
  
  
---   

## **regress-1365.js (v8 issue)**  
   
**[Calling functions through variables doesn't use undefined as receiver.](https://crbug.com/v8/1365)**  
**[Commit: Pass undefined to JS builtins when called with implicit receiver.](https://chromium.googlesource.com/v8/v8/+/19b718f)**  
  
Date(Commit): Thu May 26 11:07:48 2011  
Code Review: [http://codereview.chromium.org/7068009](http://codereview.chromium.org/7068009)  
Regress: [mjsunit/regress/regress-1365.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1365.js)  
```javascript
var valueOf = Object.prototype.valueOf;
var hasOwnProperty = Object.prototype.hasOwnProperty;

function callGlobalValueOf() { valueOf(); }
function callGlobalHasOwnProperty() { valueOf(); }

assertEquals(Object.prototype, Object.prototype.valueOf());
assertThrows(callGlobalValueOf);
assertThrows(callGlobalHasOwnProperty);

%OptimizeFunctionOnNextCall(Object.prototype.valueOf);
Object.prototype.valueOf();

assertEquals(Object.prototype, Object.prototype.valueOf());
assertThrows(callGlobalValueOf);
assertThrows(callGlobalHasOwnProperty);

function CheckExceptionCallLocal() {
  var valueOf = Object.prototype.valueOf;
  var hasOwnProperty = Object.prototype.hasOwnProperty;
  var exception = false;
  try { valueOf(); } catch(e) { exception = true; }
  assertTrue(exception);
  exception = false;
  try { hasOwnProperty(); } catch(e) { exception = true; }
  assertTrue(exception);
}
CheckExceptionCallLocal();

function CheckExceptionCallParameter(f) {
  var exception = false;
  try { f(); } catch(e) { exception = true; }
  assertTrue(exception);
}
CheckExceptionCallParameter(Object.prototype.valueOf);
CheckExceptionCallParameter(Object.prototype.hasOwnProperty);

function CheckPotentiallyShadowedByEval() {
  var exception = false;
  try {
    eval("hasOwnProperty('x')");
  } catch(e) {
    exception = true;
  }
  assertTrue(exception);
}
CheckPotentiallyShadowedByEval();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19b718f^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=19b718f)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=19b718f)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=19b718f)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=19b718f)  
[src/compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler.h?cl=19b718f)  
...  
  

---   

## **regress-1355.js (v8 issue)**  
   
**[TypeError on assignment to a read-only accessor property not in strict mode](https://crbug.com/v8/1355)**  
**[Commit: Change calls to undefined property setters to not throw (fixes issue 1355).](https://chromium.googlesource.com/v8/v8/+/f675db6)**  
  
Date(Commit): Wed May 25 08:37:38 2011  
Code Review: [http://codereview.chromium.org/7064027](http://codereview.chromium.org/7064027)  
Regress: [mjsunit/regress/regress-1355.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1355.js)  
```javascript
var foo = Object.defineProperty({}, "bar", {
 get: function () {
      return 10;
    }
  });

assertDoesNotThrow("foo.bar = 20");

function shouldThrow() {
  'use strict';
  foo.bar = 20;
}

assertThrows("shouldThrow()");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f675db6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f675db6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=f675db6)  
[test/mjsunit/getter-in-prototype.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/getter-in-prototype.js?cl=f675db6)  
[test/mjsunit/indexed-accessors.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/indexed-accessors.js?cl=f675db6)  
[test/mjsunit/regress/regress-1355.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1355.js?cl=f675db6)  
  

---   

## **regress-1403.js (v8 issue)**  
   
**[CHECK(array_proto->GetPrototype()->IsNull()) failed (64 bit)](https://crbug.com/v8/1403)**  
**[Commit: Handle changes to the Object prototype in fast handling of arrays](https://chromium.googlesource.com/v8/v8/+/eff2946)**  
  
Date(Commit): Tue May 24 12:28:10 2011  
Code Review: [http://codereview.chromium.org//7067019](http://codereview.chromium.org//7067019)  
Regress: [mjsunit/regress/regress-1403.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1403.js)  
```javascript
a = [];
assertThrows(() => Object.prototype.__proto__ = { __proto__: null }, TypeError);
a.shift();

a = [];
Array.prototype.__proto__ = { __proto__: null };
a.shift();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eff2946^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=eff2946)  
[test/mjsunit/regress/regress-1403.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1403.js?cl=eff2946)  
  

---   

## **regress-1387.js (v8 issue)**  
   
**[There can be only one [[ThrowTypeError]] function](https://crbug.com/v8/1387)**  
**[Commit: Change strict mode poison pill to be the samme type error function (fixes issue 1387).](https://chromium.googlesource.com/v8/v8/+/ab67432)**  
  
Date(Commit): Tue May 24 11:07:06 2011  
Code Review: [http://codereview.chromium.org/7067017](http://codereview.chromium.org/7067017)  
Regress: [mjsunit/regress/regress-1387.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1387.js)  
```javascript
function foo() {
  'use strict';
  return arguments;
}

var get = Object.getOwnPropertyDescriptor(foo(), "callee").get;
var set = Object.getOwnPropertyDescriptor(foo(), "callee").set;
assertEquals(get, set);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ab67432^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=ab67432)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=ab67432)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=ab67432)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=ab67432)  
[test/mjsunit/regress/regress-1387.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1387.js?cl=ab67432)  
  

---   

## **regress-1401.js (v8 issue)**  
   
**[ARM: Wrong value returned from keyed property assignemnt](https://crbug.com/v8/1401)**  
**[Commit: Add regression test for issue 1401](https://chromium.googlesource.com/v8/v8/+/825a433)**  
  
Date(Commit): Mon May 23 13:03:45 2011  
Code Review: [http://codereview.chromium.org//7062002](http://codereview.chromium.org//7062002)  
Regress: [mjsunit/regress/regress-1401.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1401.js)  
```javascript
var bottom = 0;
var sizes = new Array();

for (i = 0; i < 10; i++) {
  sizes[i] = 0;
}

function foo() {
  var size = bottom + 1 + 10;
  var t =  (sizes[++bottom] = size);
  return t;
}

for (i = 0; i < 5; i++) {
  assertEquals(i + 11, foo());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/825a433^!)  
[test/mjsunit/regress/regress-1401.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1401.js?cl=825a433)  
  

---   

## **regress-82769.js (chromium issue)**  
   
**[Regression: incorrect results in Crypto-JS on Linux amd64](https://crbug.com/82769)**  
**[Commit: Add regression test for http://crbug.com/82769](https://chromium.googlesource.com/v8/v8/+/7fba506)**  
  
Date(Commit): Wed May 18 12:46:21 2011  
Components: None  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://crbug.com/82769](http://crbug.com/82769)  
Regress: [mjsunit/regress/regress-82769.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-82769.js)  
```javascript
x = -1;
y = -0;
for (var i = 0; i < 5; i++) {
  assertEquals(0xFFFFFFFF, (x >>> y));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7fba506^!)  
[test/mjsunit/regress/regress-82769.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-82769.js?cl=7fba506)  
  

---   

## **regress-1394.js (v8 issue)**  
   
**[JSLinux bootup is slow.](https://crbug.com/v8/1394)**  
**[Commit: Fix bug in optimized compiler's switch-statement.](https://chromium.googlesource.com/v8/v8/+/6691196)**  
  
Date(Commit): Wed May 18 11:06:07 2011  
Code Review: [http://codereview.chromium.org/7037023](http://codereview.chromium.org/7037023)  
Regress: [mjsunit/compiler/regress-1394.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1394.js)  
```javascript
function f(x) {
  var ret = -1;
  switch(x){
    default:
    case 0:
      ret = 0;
      break;
    case 1:
      ret = 1;
      break;
    case 2:
      ret = 2;
      break;
    case 3:
      ret = 3;
      break;
    case 4:
      ret = 4;
      break;
  }
  return ret;
};

%PrepareFunctionForOptimization(f);

for (var i = 0; i < 3; i++) assertEquals(i, f(i));

%OptimizeFunctionOnNextCall(f);

assertEquals(0, f(0));
assertEquals(1, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6691196^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6691196)  
[test/mjsunit/compiler/regress-1394.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1394.js?cl=6691196)  
  

---   

## **regress-1389.js (v8 issue)**  
   
**[Incorrect result with -always-opt for x++](https://crbug.com/v8/1389)**  
**[Commit: Fix error in postfix ++ in Crankshaft.](https://chromium.googlesource.com/v8/v8/+/0eca2b4)**  
  
Date(Commit): Tue May 17 11:41:59 2011  
Code Review: [http://codereview.chromium.org/7033008](http://codereview.chromium.org/7033008)  
Regress: [mjsunit/regress/regress-1389.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1389.js)  
```javascript
for (var i=0; i<4; i++) {
  (function () {
    (function () {
      (function () {
        var x;
        y = x++;
      })();
    })();
  })();
}

assertEquals(NaN, y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0eca2b4^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=0eca2b4)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=0eca2b4)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=0eca2b4)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=0eca2b4)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=0eca2b4)  
...  
  

---   

## **regress-1383.js (v8 issue)**  
   
**[Assertion hit in ComputeFlags when called from ComputeKeyedLoadOrStoreExternalArray](https://crbug.com/v8/1383)**  
**[Commit: Allow strict mode flag as extraicstate for keyed external array store ic](https://chromium.googlesource.com/v8/v8/+/7f8a918)**  
  
Date(Commit): Wed May 11 08:53:46 2011  
Code Review: [http://codereview.chromium.org/7003022](http://codereview.chromium.org/7003022)  
Regress: [mjsunit/regress/regress-1383.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1383.js)  
```javascript
x="";
function foo(){
  "use strict";
  var wxemsx=(4);
  var wxemsx_0=new Float32Array(wxemsx);
  wxemsx_0[0]={};
}

foo()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7f8a918^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=7f8a918)  
[test/mjsunit/regress/regress-1383.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1383.js?cl=7f8a918)  
  

---   

## **regress-1369.js (v8 issue)**  
   
**[Assertion failure when calling gc function on non-object receiver.](https://crbug.com/v8/1369)**  
**[Commit: Check that receiver is JSObject on API calls.](https://chromium.googlesource.com/v8/v8/+/0961b1a)**  
  
Date(Commit): Fri May 06 14:14:16 2011  
Code Review: [http://codereview.chromium.org/6931056](http://codereview.chromium.org/6931056)  
Regress: [mjsunit/regress/regress-1369.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1369.js)  
```javascript
assertDoesNotThrow('gc.call(1)');
assertDoesNotThrow('gc.call("asdf")');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0961b1a^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=0961b1a)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=0961b1a)  
[test/mjsunit/regress/regress-1369.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1369.js?cl=0961b1a)  
  

---   

## **regress-955.js (v8 issue)**  
   
**[parseInt('-\t0') should return NaN, not 0](https://crbug.com/v8/955)**  
**[Commit: Don't allow whitespace after sign characters in parseInt.](https://chromium.googlesource.com/v8/v8/+/d141160)**  
  
Date(Commit): Tue May 03 07:11:17 2011  
Code Review: [http://codereview.chromium.org/6903171](http://codereview.chromium.org/6903171)  
Regress: [mjsunit/regress/regress-955.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-955.js)  
```javascript
assertEquals(-0, parseInt("-0"));
assertEquals(0, parseInt("+0"));

assertEquals(NaN, parseInt("- 0"));
assertEquals(NaN, parseInt("+ 0"));
assertEquals(NaN, parseInt("-\t0"));
assertEquals(NaN, parseInt("+\t0"));

assertEquals(-0, parseInt(" -0"));
assertEquals(0, parseInt(" +0"));
assertEquals(-0, parseInt("\t-0"));
assertEquals(0, parseInt("\t+0"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d141160^!)  
[src/conversions.cc](https://cs.chromium.org/chromium/src/v8/src/conversions.cc?cl=d141160)  
[test/mjsunit/regress/regress-955.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-955.js?cl=d141160)  
  

---   

## **regress-1351.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1351)**  
**[Commit: Be more discriminating about uses of the arguments object in optimized code.](https://chromium.googlesource.com/v8/v8/+/1af840a)**  
  
Date(Commit): Mon May 02 11:35:51 2011  
Code Review: [http://codereview.chromium.org/6902202](http://codereview.chromium.org/6902202)  
Regress: [mjsunit/regress/regress-1351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1351.js)  
```javascript
function h() {}

function f() {
  var a = null;
  h(a = arguments);
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1af840a^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=1af840a)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=1af840a)  
[test/mjsunit/regress/regress-1351.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1351.js?cl=1af840a)  
  

---   

## **regress-1337.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1337)**  
**[Commit: Make throw inlineable only if the exception is inlineable.](https://chromium.googlesource.com/v8/v8/+/3b6fe22)**  
  
Date(Commit): Wed Apr 20 09:15:52 2011  
Code Review: [http://codereview.chromium.org/6881079](http://codereview.chromium.org/6881079)  
Regress: [mjsunit/regress/regress-1337.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1337.js)  
```javascript
function bar() {
  throw {};
}

function foo() {
  bar();
};
%PrepareFunctionForOptimization(foo);
for (var i = 0; i < 5; ++i) {
  try {
    foo();
  } catch (e) {
  }
}
%OptimizeFunctionOnNextCall(foo);
try {
  foo();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b6fe22^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=3b6fe22)  
[test/mjsunit/regress/regress-1337.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1337.js?cl=3b6fe22)  
  

---   

## **regress-1323.js (v8 issue)**  
   
**[Loading/Storing external float arrays broken in Crankshaft on ARM](https://crbug.com/v8/1323)**  
**[Commit: Fix load/store of external float arrays on ARM](https://chromium.googlesource.com/v8/v8/+/1d774ac)**  
  
Date(Commit): Tue Apr 12 15:20:26 2011  
Code Review: [http://codereview.chromium.org/6822054](http://codereview.chromium.org/6822054)  
Regress: [mjsunit/regress/regress-1323.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1323.js)  
```javascript
function get(a, index) {
  return a[index];
};
%PrepareFunctionForOptimization(get);
var a = new Float32Array(2);
a[0] = 2.5;
a[1] = 3.5;
for (var i = 0; i < 5; i++) get(a, 0);
%OptimizeFunctionOnNextCall(get);
assertEquals(2.5, get(a, 0));
assertEquals(3.5, get(a, 1));

function set(a, index, value) {
  a[index] = value;
};
%PrepareFunctionForOptimization(set);
for (var i = 0; i < 5; i++) set(a, 0, 4.5);
%OptimizeFunctionOnNextCall(set);
set(a, 0, 4.5);
assertEquals(4.5, a[0]);
assertEquals(3.5, a[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d774ac^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=1d774ac)  
[test/mjsunit/regress/regress-1323.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1323.js?cl=1d774ac)  
  

---   

## **regress-78270.js (chromium issue)**  
   
**[[LangFuzz] V8: Crash in HeapObject::map_word on GC](https://crbug.com/78270)**  
**[Commit: In LCodeGen::DoDeferredLInstanceOfKnownGlobal emit safepoint with registers for the call to stub.](https://chromium.googlesource.com/v8/v8/+/8a8d3bb)**  
  
Date(Commit): Thu Apr 07 13:32:45 2011  
Components: Internals  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-11", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [http://codereview.chromium.org/6793017](http://codereview.chromium.org/6793017)  
Regress: [mjsunit/regress/regress-78270.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-78270.js)  
```javascript
for (var i = 0; i < 10000; i++) {
  try {
    var object = { };
    function g(f0) {
      var f0 = (object instanceof encodeURI)('foo');
    }
    g(75);
  } catch (g) {
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a8d3bb^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=8a8d3bb)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=8a8d3bb)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8a8d3bb)  
[src/ia32/lithium-codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.h?cl=8a8d3bb)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=8a8d3bb)  
...  
  

---   

## **regress-1309.js (v8 issue)**  
   
**[__proto__ should not be mutable if object is not Extensible](https://crbug.com/v8/1309)**  
**[Commit: 1309 fix](https://chromium.googlesource.com/v8/v8/+/e3d7883)**  
  
Date(Commit): Wed Apr 06 16:22:06 2011  
Code Review: [http://codereview.chromium.org/6800018](http://codereview.chromium.org/6800018)  
Regress: [mjsunit/regress/regress-1309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1309.js)  
```javascript
var o = Object.preventExtensions({});
assertThrows("o.__proto__ = {}");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3d7883^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=e3d7883)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e3d7883)  
[test/mjsunit/regress/regress-1309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1309.js?cl=e3d7883)  
  

---   

## **regress-1278.js (v8 issue)**  
   
**[ARM: mjsunit test mul-exhaustive fails](https://crbug.com/v8/1278)**  
**[Commit: ARM: Check for minus zero when converting binary operation result to smi](https://chromium.googlesource.com/v8/v8/+/1eb224c)**  
  
Date(Commit): Tue Mar 29 07:43:27 2011  
Code Review: [http://codereview.chromium.org/6755009](http://codereview.chromium.org/6755009)  
Regress: [mjsunit/regress/regress-1278.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1278.js)  
```javascript
function add(x, y) {
  return x + y;
}

function sub(x, y) {
  return x - y;
}

function mul(x, y) {
  return x * y;
}

function div(x, y) {
  return x / y;
}

for (var i = 0; i < 10; i++) {
  assertEquals(0, add(0, 0));
  assertEquals(0, add(0, -0));
  assertEquals(0, add(-0, 0));
  assertEquals(-0, add(-0, -0));

  assertEquals(0, sub(0, 0));
  assertEquals(0, sub(0, -0));
  assertEquals(-0, sub(-0, 0));
  assertEquals(0, sub(-0, -0));

  assertEquals(0, mul(0, 0));
  assertEquals(-0, mul(0, -0));
  assertEquals(-0, mul(-0, 0));
  assertEquals(0, mul(-0, -0));

  assertEquals(0, div(0, 1));
  assertEquals(-0, div(0, -1));
  assertEquals(-0, div(-0, 1));
  assertEquals(0, div(-0, -1));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1eb224c^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=1eb224c)  
[test/mjsunit/regress/regress-1278.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1278.js?cl=1eb224c)  
  

---   

## **regress-loadfield.js (other issue)**  
   
**[Commit: Fix bug that caused invalid code motion for certain loads instructions.](https://chromium.googlesource.com/v8/v8/+/e6cbf65)**  
  
Date(Commit): Thu Mar 24 11:37:24 2011  
Code Review: [http://codereview.chromium.org/6730025](http://codereview.chromium.org/6730025)  
Regress: [mjsunit/compiler/regress-loadfield.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-loadfield.js)  
```javascript
function bar() {}

var b = new bar();
b.bar = "bar";

function test(a) {
  var b = new Array(10);
  for (var i = 0; i < 10; i++) {
    b[i] = new bar();
  }

  for (var i = 0; i < 10; i++) {
    b[i].bar = a.foo;
  }
}

%PrepareFunctionForOptimization(test);

var a = {};
a.p1 = "";
a.p2 = "";
a.p3 = "";
a.p4 = "";
a.p5 = "";
a.p6 = "";
a.p7 = "";
a.p8 = "";
a.p9 = "";
a.p10 = "";
a.p11 = "";
a.foo = "foo";
for (var i = 0; i < 5; i++) {
 test(a);
}
%OptimizeFunctionOnNextCall(test);
test(a);

test("");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6cbf65^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=e6cbf65)  
[test/mjsunit/compiler/regress-loadfield.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-loadfield.js?cl=e6cbf65)  
  
  
---   

## **regress-lazy-deopt-reloc.js (other issue)**  
   
**[Commit: Add regression test.](https://chromium.googlesource.com/v8/v8/+/a7d44c4)**  
  
Date(Commit): Thu Mar 24 11:03:08 2011  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@7338](http://v8.googlecode.com/svn/branches/bleeding_edge@7338)  
Regress: [mjsunit/regress/regress-lazy-deopt-reloc.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lazy-deopt-reloc.js)  
```javascript
function kaboom() {
  var a = function () {},
      b = function () {},
      c, d = function () { var d = []; },
      e = function () { var e = {}; };
    c = function () { d(); b(); };
    return function (x, y) {
      c();
      a();
      return function f() { }({});
    };
}

kaboom();

%DeoptimizeFunction(kaboom);

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7d44c4^!)  
[test/mjsunit/regress/regress-lazy-deopt-reloc.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-lazy-deopt-reloc.js?cl=a7d44c4)  
  
  
---   

## **regress-1257.js (v8 issue)**  
   
**[Unexpected errors and randomness with large compiled JS script](https://crbug.com/v8/1257)**  
**[Commit: Make HDeoptimize to explicitly use environment values.](https://chromium.googlesource.com/v8/v8/+/c83f0a7)**  
  
Date(Commit): Thu Mar 17 12:22:49 2011  
Code Review: [http://codereview.chromium.org/6672066](http://codereview.chromium.org/6672066)  
Regress: [mjsunit/regress/regress-1257.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1257.js)  
```javascript
function g(y) { assertEquals(y, 12); }

var X = 0;

function foo () {
  var cnt = 0;
  var l = -1;
  var x = 0;
  while (1) switch (l) {
      case -1:
        var y = x + 12;
        l = 0;
        break;
      case 0:
        if (cnt++ == 5) {
          %OptimizeOsr();
          l = 1;
        }
        break;
      case 1:
        // This case will contain deoptimization
        // because it has no type feedback.
        g(y);
        return;
    };
}

%PrepareFunctionForOptimization(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c83f0a7^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=c83f0a7)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=c83f0a7)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=c83f0a7)  
[test/mjsunit/regress/regress-1257.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1257.js?cl=c83f0a7)  
  

---   

## **regress-1233.js (v8 issue)**  
   
**[Can't get descriptor of property whose values doesn't inherit from Object.prototype](https://crbug.com/v8/1233)**  
**[Commit: Fix a problem where Object.getOwnPropertyDescriptor and related functions unintentionally called toString on the values of an object's properties.  Fixes issue 1233.](https://chromium.googlesource.com/v8/v8/+/f6e1b82)**  
  
Date(Commit): Fri Mar 11 13:57:20 2011  
Code Review: [http://codereview.chromium.org/6677017](http://codereview.chromium.org/6677017)  
Regress: [mjsunit/regress/regress-1233.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1233.js)  
```javascript
var delicate = new Object();
delicate.toString = function(){ throw Error("toString"); };
delicate.valueOf = function(){ throw Error("valueOf"); };

var x = { foo: delicate };

var status = "fail";
try {
  Object.getOwnPropertyDescriptor(x, "foo");
  Object.freeze(x);
  status = "succeed";
} catch (e) {}

assertEquals("succeed", status);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6e1b82^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=f6e1b82)  
[test/mjsunit/regress/regress-1233.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1233.js?cl=f6e1b82)  
  

---   

## **regress-1240.js (v8 issue)**  
   
**[`Object#__defineSetter__` drops `configurable` to `true` what allows redefine properties with `configurable = false`](https://crbug.com/v8/1240)**  
**[Commit: Change __defineGetter__ and __defineSetter__ to respect non-configurable.](https://chromium.googlesource.com/v8/v8/+/fa9e57e)**  
  
Date(Commit): Fri Mar 11 08:05:59 2011  
Code Review: [http://codereview.chromium.org/6658037](http://codereview.chromium.org/6658037)  
Regress: [mjsunit/regress/regress-1240.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1240.js)  
```javascript
var a = {};
Object.defineProperty(a, 'b',
                      { get: function () { return 42; }, configurable: false });
try {
  a.__defineGetter__('b', function _b(){ return 'foo'; });
} catch (e) {}
assertEquals(42, a.b);
var desc = Object.getOwnPropertyDescriptor(a, 'b');
assertFalse(desc.configurable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa9e57e^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=fa9e57e)  
[test/mjsunit/accessors-on-global-object.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/accessors-on-global-object.js?cl=fa9e57e)  
[test/mjsunit/regress/regress-1240.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1240.js?cl=fa9e57e)  
[test/mozilla/mozilla.status](https://cs.chromium.org/chromium/src/v8/test/mozilla/mozilla.status?cl=fa9e57e)  
  

---   

## **regress-1236.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1236)**  
**[Commit: Fix bug in X64 RegExpExec stub.](https://chromium.googlesource.com/v8/v8/+/a8b41a0)**  
  
Date(Commit): Tue Mar 08 14:15:25 2011  
Code Review: [http://codereview.chromium.org/6635041](http://codereview.chromium.org/6635041)  
Regress: [mjsunit/regress/regress-1236.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1236.js)  
```javascript
pattern = RegExp("","");  // RegExp is irrelevant, as long as it's not an atom.
string = 'a';             // Anything non-empty (flat ASCII).
pattern.exec(string);     // Ensure that JSRegExp is compiled.
pattern["@"] = 42;        // Change layout of JSRegExp object.
pattern.exec(string);     // Call again to trigger bug in stub.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8b41a0^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=a8b41a0)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=a8b41a0)  
[src/x64/macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.cc?cl=a8b41a0)  
[test/mjsunit/regress/regress-1236.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1236.js?cl=a8b41a0)  
  

---   

## **regress-1237.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1237)**  
**[Commit: Fix a stack-height mismatch during deoptimization.](https://chromium.googlesource.com/v8/v8/+/4a9056c)**  
  
Date(Commit): Mon Mar 07 17:01:12 2011  
Code Review: [http://codereview.chromium.org/6625057](http://codereview.chromium.org/6625057)  
Regress: [mjsunit/regress/regress-1237.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1237.js)  
```javascript
function observe(x, y) {
  return x;
}
function test(x) {
  return observe(1, (x ? observe(observe.prototype.x) : 'c', x + 1));
};
%PrepareFunctionForOptimization(test);
for (var i = 0; i < 5; ++i) test(0);
%OptimizeFunctionOnNextCall(test);
test(0);

test("a");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a9056c^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=4a9056c)  
[test/mjsunit/regress/regress-1237.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1237.js?cl=4a9056c)  
  

---   

## **regress-1207.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1207)**  
**[Commit: Add lazy deoptimization environment to instanceof by marking it as a call.](https://chromium.googlesource.com/v8/v8/+/8a72161)**  
  
Date(Commit): Tue Mar 01 15:37:24 2011  
Code Review: [http://codereview.chromium.org/6588083](http://codereview.chromium.org/6588083)  
Regress: [mjsunit/regress/regress-1207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1207.js)  
```javascript
try {
var object = { };
function fib(n) {
  var f0 = (object instanceof encodeURI)('#2: var x = 1; x <= 1 === true'), f1 = 1;
}
fib(75);
} catch (o) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a72161^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=8a72161)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=8a72161)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8a72161)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=8a72161)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=8a72161)  
...  
  

---   

## **regress-1210.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1210)**  
**[Commit: Fix a stack height mismatch when deoptimizing.](https://chromium.googlesource.com/v8/v8/+/6b1530e)**  
  
Date(Commit): Tue Mar 01 09:32:45 2011  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@6981](http://v8.googlecode.com/svn/branches/bleeding_edge@6981)  
Regress: [mjsunit/regress/regress-1210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1210.js)  
```javascript
var a = 0;

function observe(x, y) {
  return x;
}

function side_effect(x) {
  a = x;
}

function test() {
  // We will trigger deoptimization of 'a + 0' which should bail out to
  // immediately after the call to 'side_effect' (i.e., still in the key
  // subexpression of the arguments access).
  return observe(a, arguments[(side_effect(a), a + 0)]);
}

;
%PrepareFunctionForOptimization(test);
for (var i = 0; i < 10; ++i) test(0);
%OptimizeFunctionOnNextCall(test);
test(0);

a = "hello";
test(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b1530e^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6b1530e)  
[test/mjsunit/regress/regress-1210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1210.js?cl=6b1530e)  
  

---   

## **regress-1213.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1213)**  
**[Commit: Do not allow non-configurable global properties to be made configurable (fixes issue 1213).](https://chromium.googlesource.com/v8/v8/+/c63d9c9)**  
  
Date(Commit): Tue Mar 01 08:09:17 2011  
Code Review: [http://codereview.chromium.org/6597045](http://codereview.chromium.org/6597045)  
Regress: [mjsunit/regress/regress-1213.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1213.js)  
```javascript
var x = 0;

function TestGlobal() {
  for (var i = 0; i < 2; i++) {
    x = x + 1;
  }
  this.eval('function x() {};');
  delete this['x'];
}

TestGlobal();
TestGlobal();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c63d9c9^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=c63d9c9)  
[test/mjsunit/regress/regress-1213.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1213.js?cl=c63d9c9)  
  

---   

## **regress-1218.js (v8 issue)**  
   
**[Error.prototype.toString should not have a "prototype" property](https://crbug.com/v8/1218)**  
**[Commit: Remove Error.prototype.toStrings prototype property.](https://chromium.googlesource.com/v8/v8/+/7c561be)**  
  
Date(Commit): Mon Feb 28 13:29:05 2011  
Code Review: [http://codereview.chromium.org/6588050](http://codereview.chromium.org/6588050)  
Regress: [mjsunit/regress/regress-1218.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1218.js)  
```javascript
assertFalse(Error.prototype.toString.hasOwnProperty("prototype"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c561be^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=7c561be)  
[test/mjsunit/regress/regress-1218.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1218.js?cl=7c561be)  
  

---   

## **regress-1209.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1209)**  
**[Commit: When checking number of parameters in MakeCrankshaft code don't forget about receiver.](https://chromium.googlesource.com/v8/v8/+/88b70c8)**  
  
Date(Commit): Mon Feb 28 13:20:10 2011  
Code Review: [http://codereview.chromium.org/6591042](http://codereview.chromium.org/6591042)  
Regress: [mjsunit/regress/regress-1209.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1209.js)  
```javascript
function crashMe(n) {
  var nasty = [];
  while (n--)
    nasty.push("a" + 0);
  return Function.apply(null, nasty);
}
crashMe(64 + 1).length;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88b70c8^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=88b70c8)  
[test/mjsunit/regress/regress-1209.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1209.js?cl=88b70c8)  
  

---   

## **regress-1172-bis.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1172)**  
**[Commit: Get property may throw an exception thanks to JS accessors.](https://chromium.googlesource.com/v8/v8/+/da463ab)**  
  
Date(Commit): Thu Feb 24 17:42:56 2011  
Code Review: [http://codereview.chromium.org/6580030](http://codereview.chromium.org/6580030)  
Regress: [mjsunit/regress/regress-1172-bis.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1172-bis.js), [mjsunit/regress/regress-1172.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1172.js)  
```javascript
Object.prototype.__defineGetter__(0, function() { throw 42; });
var exception = false;
try {
  Object[0]();
} catch(e) {
  exception = true;
  assertEquals(42, e);
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da463ab^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=da463ab)  
[test/mjsunit/regress/regress-1172-bis.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1172-bis.js?cl=da463ab)  
  

---   

## **regress-1181.js (v8 issue)**  
   
**[Bug on ARM when using OSR](https://crbug.com/v8/1181)**  
**[Commit: ARM: Fix DoubleToI.](https://chromium.googlesource.com/v8/v8/+/5572d24)**  
  
Date(Commit): Thu Feb 24 10:07:35 2011  
Code Review: [http://codereview.chromium.org/6573004](http://codereview.chromium.org/6573004)  
Regress: [mjsunit/regress/regress-1181.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1181.js)  
```javascript
function test(x) {
    var xp = x  * 1 - 1;
    return xp;
}


function check(count) {
    %DeoptimizeFunction(test);
    var i;
    for(var x=0; x < count; x++){
        for(var y=0; y < count; y++){
            i = test(x / 100);
        }
    }
    assertEquals((count - 1) / 100, i + 1);
}


check(150);
check(200);
check(350);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5572d24^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=5572d24)  
[test/mjsunit/regress/regress-1181.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1181.js?cl=5572d24)  
  

---   

## **regress-1184.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1184)**  
**[Commit: Properly reset external catcher if exception couldn't be externally caught.](https://chromium.googlesource.com/v8/v8/+/ae328e6)**  
  
Date(Commit): Wed Feb 23 06:55:47 2011  
Code Review: [http://codereview.chromium.org/6538081](http://codereview.chromium.org/6538081)  
Regress: [mjsunit/regress/regress-1184.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1184.js)  
```javascript
o = {};
o.__defineGetter__('foo', function() { throw 42; });
function f() {
 try {
   // throw below sets up Top::thread_local_.catcher_...
   throw 42;
 } finally {
   // ...JS accessor traverses v8 runtime/JS boundary and
   // when coming back from JS to v8 runtime, retraverses
   // stack with catcher set while processing exception
   // which is not caught by external try catch.
   try { o.foo; } catch(e) { };
   return;
 }
};
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae328e6^!)  
[src/top.cc](https://cs.chromium.org/chromium/src/v8/src/top.cc?cl=ae328e6)  
[test/mjsunit/regress/regress-1184.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1184.js?cl=ae328e6)  
  

---   

## **regress-1176.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1176)**  
**[Commit: Fix for bug http://code.google.com/p/v8/issues/detail?id=1176.](https://chromium.googlesource.com/v8/v8/+/3ff7aa0)**  
  
Date(Commit): Tue Feb 22 17:20:25 2011  
Code Review: [http://code.google.com/p/v8/issues/detail?id=1176.](http://code.google.com/p/v8/issues/detail?id=1176.)  
Regress: [mjsunit/regress/regress-1176.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1176.js)  
```javascript
"use strict";
function strict_delete_this() {
  // "delete this" is allowed in strict mode.
  delete this;
}
strict_delete_this();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ff7aa0^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=3ff7aa0)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=3ff7aa0)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=3ff7aa0)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=3ff7aa0)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=3ff7aa0)  
...  
  

---   

## **regress-1174.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1174)**  
**[Commit: Add more generic version of reloc info padding to ensure enough space for reloc patching during deoptimization (fixes issue 1174).](https://chromium.googlesource.com/v8/v8/+/45c63ff)**  
  
Date(Commit): Tue Feb 22 12:28:33 2011  
Code Review: [http://codereview.chromium.org/6541053](http://codereview.chromium.org/6541053)  
Regress: [mjsunit/regress/regress-1174.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1174.js)  
```javascript
function Regular() {
  this[0] >>=  0;
  this[1] ^=  1;
}

function foo() {
  var regular = new Regular();
  %DeoptimizeFunction(Regular);
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/45c63ff^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=45c63ff)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=45c63ff)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=45c63ff)  
[src/ia32/lithium-codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.h?cl=45c63ff)  
[test/mjsunit/regress/regress-1174.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1174.js?cl=45c63ff)  
  

---   

## **regress-1167.js (v8 issue)**  
   
**[[LangFuzz] Assertion CHECK(fixed_size + height_in_bytes == input_frame_size) failed](https://crbug.com/v8/1167)**  
**[Commit: Fix incorrect deoptimization for logical not in an effect context.](https://chromium.googlesource.com/v8/v8/+/b021072)**  
  
Date(Commit): Thu Feb 17 13:05:49 2011  
Code Review: [http://codereview.chromium.org/6537018](http://codereview.chromium.org/6537018)  
Regress: [mjsunit/regress/regress-1167.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1167.js)  
```javascript
function test0(n) {
  var a = new Array(n);
  for (var i = 0; i < n; ++i) {
    // ~ of a non-numeric value is used to trigger deoptimization.
    a[i] = void !delete 'object' % ~delete 4;
  }
}

for (var i = 0; i < 5; ++i) {
  for (var j = 1; j < 12; ++j) {
    test0(j * 1000);
  }
}


function test1(n) {
  var a = new Array(n);
  for (var i = 0; i < n; ++i) {
    a[i] = void !-'object' % ~delete 4;
  }
}

for (i = 0; i < 5; ++i) {
  for (j = 1; j < 12; ++j) {
    test1(j * 1000);
  }
}


function side_effect() {}
function observe(x, y) {
  return x;
}
function test2(x) {
  return observe(
      this, (side_effect.observe <= side_effect.side_effect !== false, x + 1));
};
%PrepareFunctionForOptimization(test2);
for (var i = 0; i < 5; ++i) test2(0);
%OptimizeFunctionOnNextCall(test2);
test2(0);
test2(test2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b021072^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=b021072)  
[test/mjsunit/regress/regress-1167.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1167.js?cl=b021072)  
  

---   

## **regress-1166.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1166)**  
**[Commit: Fix a bug in deoptimization after logical expressions in an effect context.](https://chromium.googlesource.com/v8/v8/+/82cdd48)**  
  
Date(Commit): Thu Feb 17 11:06:50 2011  
Code Review: [http://codereview.chromium.org/6519046](http://codereview.chromium.org/6519046)  
Regress: [mjsunit/regress/regress-1166.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1166.js)  
```javascript
function observe(x, y) {
  return x;
}

function test(x) {
  return observe(1, (false || false, x + 1));
};
%PrepareFunctionForOptimization(test);
for (var i = 0; i < 5; ++i) test(0);
%OptimizeFunctionOnNextCall(test);
test(0);

test("a");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82cdd48^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=82cdd48)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=82cdd48)  
[test/mjsunit/regress/regress-1166.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1166.js?cl=82cdd48)  
  

---   

## **regress-1160.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1160)**  
**[Commit: Fix issue 1160: check array elements in ArrayJoin.](https://chromium.googlesource.com/v8/v8/+/4143e4c)**  
  
Date(Commit): Tue Feb 15 15:12:51 2011  
Code Review: [http://codereview.chromium.org/6529020](http://codereview.chromium.org/6529020)  
Regress: [mjsunit/regress/regress-1160.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1160.js)  
```javascript
var N = 10;
var array = Array(N);
for (var i = 0; i < N; ++i) {
  array[i] = i;
}
Array.prototype.__defineSetter__(2, function() { });
assertEquals("0,1,2,3,4,5,6,7,8,9", array.join(","));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4143e4c^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4143e4c)  
[test/mjsunit/regress/regress-1160.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1160.js?cl=4143e4c)  
  

---   

## **regress-1156.js (v8 issue)**  
   
**[V8 Crash when idling at news.google.com](https://crbug.com/v8/1156)**  
**[Commit: Make sure we always have room for patching the reloc info during lazy deoptimization (fixes issue 1156).](https://chromium.googlesource.com/v8/v8/+/a8d4360)**  
  
Date(Commit): Tue Feb 15 14:36:12 2011  
Code Review: [http://codereview.chromium.org/6499015](http://codereview.chromium.org/6499015)  
Regress: [mjsunit/regress/regress-1156.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1156.js)  
```javascript
function foo(a) {
  delete a[1];
  delete a[2];
  delete a[3];
  delete a[4];
  delete a[5];
  return void 0;
}

function call_and_deopt() {
  var b = [1,2,3];
  foo(b);
  foo(b);
  %DeoptimizeFunction(foo);
}

call_and_deopt();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8d4360^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=a8d4360)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=a8d4360)  
[src/ia32/assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.cc?cl=a8d4360)  
[src/ia32/assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.h?cl=a8d4360)  
[src/ia32/deoptimizer-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/deoptimizer-ia32.cc?cl=a8d4360)  
...  
  

---   

## **regress-1146.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1146)**  
**[Commit: Fix a potential crash bug in keyed calls for non-string keys.](https://chromium.googlesource.com/v8/v8/+/ad70b7d)**  
  
Date(Commit): Mon Feb 14 13:13:41 2011  
Code Review: [http://codereview.chromium.org/6517010](http://codereview.chromium.org/6517010)  
Regress: [mjsunit/regress/regress-1146.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1146.js)  
```javascript
function F() {}
var a = new F();
function f(i) { return a[i](); }

a.first = function() { return 11; }
a[0] = function() { return 22; }
var obj = {};
a[obj] = function() { return 33; }

a.foo = 0;
delete a.foo;
var b = "first";
f(b);
f(b);

assertEquals(11, f(b));
assertEquals(22, f(0));
assertEquals(33, f(obj));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad70b7d^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=ad70b7d)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=ad70b7d)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=ad70b7d)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=ad70b7d)  
[src/x64/ic-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/ic-x64.cc?cl=ad70b7d)  
...  
  

---   

## **regress-1149.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1149)**  
**[Commit: Fix a duplicate AST ID recorded for for/in.](https://chromium.googlesource.com/v8/v8/+/c73ce4f)**  
  
Date(Commit): Mon Feb 14 12:51:25 2011  
Code Review: [http://codereview.chromium.org/6475009](http://codereview.chromium.org/6475009)  
Regress: [mjsunit/regress/regress-1149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1149.js)  
```javascript
function f(x) {
  for (x in arguments) {
    for (x in arguments) {
    }
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c73ce4f^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c73ce4f)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c73ce4f)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=c73ce4f)  
[test/mjsunit/regress/regress-1149.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1149.js?cl=c73ce4f)  
  

---   

## **regress-crbug-72736.js (chromium issue)**  
   
**[Object.defineProperty does not override existing properties ](https://crbug.com/72736)**  
**[Commit: Use ForceSetObjectProperty in DefineOrRedefineDataProperty (fixes crbug 72736).](https://chromium.googlesource.com/v8/v8/+/34eeb88)**  
  
Date(Commit): Mon Feb 14 10:43:21 2011  
Components: Blink>JavaScriptBlink  
Labels: []  
Code Review: [http://codereview.chromium.org/6518004](http://codereview.chromium.org/6518004)  
Regress: [mjsunit/regress/regress-crbug-72736.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-72736.js)  
```javascript
var obj = {};
Object.defineProperty(obj, 'foo', { value: 10, configurable: true });
assertEquals(obj.foo, 10);
Object.defineProperty(obj, 'foo', { value: 20, configurable: true });
assertEquals(obj.foo, 20);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34eeb88^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=34eeb88)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=34eeb88)  
[test/mjsunit/regress/regress-crbug-72736.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-72736.js?cl=34eeb88)  
  

---   

## **regress-1151.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1151)**  
**[Commit: Do not allow calls to SetProtoType on functions that should not have a prototype (fixes issue 1151)](https://chromium.googlesource.com/v8/v8/+/6d9fde4)**  
  
Date(Commit): Mon Feb 14 09:37:56 2011  
Code Review: [http://codereview.chromium.org/6518003](http://codereview.chromium.org/6518003)  
Regress: [mjsunit/regress/regress-1151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1151.js)  
```javascript
__defineSetter__.__proto__ = function() {};
__defineSetter__['prototype']

eval.__proto__ = function () { };
eval['prototype'] = {};

function f() { return 42; }
f.prototype = 43;
__defineGetter__.__proto__ = f;

assertEquals(__defineGetter__.prototype, 43);

__defineGetter__.prototype = "foo";
assertEquals(__defineGetter__.prototype, "foo");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d9fde4^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=6d9fde4)  
[test/mjsunit/regress/regress-1151.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1151.js?cl=6d9fde4)  
  

---   

## **regress-1150.js (v8 issue)**  
   
**[Object.keys and Object.getOwnPropertyDescriptor disagree for global variables](https://crbug.com/v8/1150)**  
**[Commit: Add support for the global object in Object.keys (fixes issue 1150)](https://chromium.googlesource.com/v8/v8/+/46bde30)**  
  
Date(Commit): Mon Feb 14 07:49:13 2011  
Code Review: [http://codereview.chromium.org/6516008](http://codereview.chromium.org/6516008)  
Regress: [mjsunit/regress/regress-1150.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1150.js)  
```javascript
var a = 10;
var global = (function () { return this; }) ();
var keys = Object.keys(global);
assertTrue(keys.indexOf("a") > 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/46bde30^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=46bde30)  
[test/mjsunit/regress/regress-1150.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1150.js?cl=46bde30)  
  

---   

## **regress-1132.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1132)**  
**[Commit: Properly treat exceptions thrown while compiling.](https://chromium.googlesource.com/v8/v8/+/e96c24b)**  
  
Date(Commit): Fri Feb 11 14:26:56 2011  
Code Review: [http://codereview.chromium.org/6487021](http://codereview.chromium.org/6487021)  
Regress: [mjsunit/regress/regress-1132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1132.js)  
```javascript
function test() {
  try {
    test(1, test(1));
  } catch(e) {
    assertFalse(delete e, "deleting catch variable");
    assertEquals(42, e);
  }
}

var exception = false;
try {
  test();
} catch (e) {
  exception = true;
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e96c24b^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=e96c24b)  
[src/execution.cc](https://cs.chromium.org/chromium/src/v8/src/execution.cc?cl=e96c24b)  
[src/execution.h](https://cs.chromium.org/chromium/src/v8/src/execution.h?cl=e96c24b)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=e96c24b)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e96c24b)  
...  
  

---   

## **regress-1129.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1129)**  
**[Commit: Fix typo in ASSERT in object-verifier for RegExp.](https://chromium.googlesource.com/v8/v8/+/fdfbdfb)**  
  
Date(Commit): Thu Feb 10 16:43:01 2011  
Code Review: [http://codereview.chromium.org/6476027](http://codereview.chromium.org/6476027)  
Regress: [mjsunit/regress/regress-1129.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1129.js)  
```javascript
var source = Array(50000).join("(") + "a" + Array(50000).join(")");
var r = RegExp(source);
try {
  // Try to compile in UC16 mode, and drop the exception.
  r.test("\x80");
  assertUnreachable();
} catch (e) {
}

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fdfbdfb^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=fdfbdfb)  
[test/mjsunit/regress/regress-1129.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1129.js?cl=fdfbdfb)  
  

---   

## **regress-1130.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1130)**  
**[Commit: Bypass JS accessors when building error array.](https://chromium.googlesource.com/v8/v8/+/ab244857)**  
  
Date(Commit): Thu Feb 10 15:02:13 2011  
Code Review: [http://codereview.chromium.org/6481001](http://codereview.chromium.org/6481001)  
Regress: [mjsunit/regress/regress-1130.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1130.js)  
```javascript
Object.prototype.__defineGetter__(0, function() { throw 42; } );

var exception = false;
try {
  eval("(function() { const x; var x })")();
} catch (e) {
  exception = true;
  assertTrue(e instanceof SyntaxError);
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ab244857^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=ab244857)  
[test/mjsunit/regress/regress-1130.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1130.js?cl=ab244857)  
  

---   

## **regress-1131.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1131)**  
**[Commit: Bailout from PrepareSlowElementsForSort when hiting a key outside of smi-range.](https://chromium.googlesource.com/v8/v8/+/49adfd0)**  
  
Date(Commit): Thu Feb 10 12:33:34 2011  
Code Review: [http://codereview.chromium.org/6469006](http://codereview.chromium.org/6469006)  
Regress: [mjsunit/regress/regress-1131.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1131.js)  
```javascript
var nonArray = { length: 4, 0: 42, 2: 37, 0xf7da5000: undefined, 4: 0 };
Array.prototype.sort.call(nonArray);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49adfd0^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=49adfd0)  
[test/mjsunit/regress/regress-1131.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1131.js?cl=49adfd0)  
  

---   

## **regress-1106.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1106)**  
**[Commit: Add a regression test for issue 1106, optimized access to the prototype chain of the global object.](https://chromium.googlesource.com/v8/v8/+/0fb5a1f)**  
  
Date(Commit): Wed Feb 09 15:50:39 2011  
Code Review: [http://codereview.chromium.org/6459023](http://codereview.chromium.org/6459023)  
Regress: [mjsunit/regress/regress-1106.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1106.js)  
```javascript
x = Object.prototype;
x.foo = 3;
x.bar = 4;
delete x.foo;
x.foo = 5;

function f() {
  return foo;
};
%PrepareFunctionForOptimization(f);
for (i = 0; i < 5; ++i) {
  assertEquals(5, f());
}
%OptimizeFunctionOnNextCall(f);
assertEquals(5, f());

x.gee = function() {
  return 42;
};
function g() {
  return gee();
};
%PrepareFunctionForOptimization(g);
for (i = 0; i < 5; ++i) {
  assertEquals(42, g());
}
%OptimizeFunctionOnNextCall(g);
assertEquals(42, g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0fb5a1f^!)  
[test/mjsunit/regress/regress-1106.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1106.js?cl=0fb5a1f)  
  

---   

## **regress-1126.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1126)**  
**[Commit: Fix incorrect asserts in scanner.](https://chromium.googlesource.com/v8/v8/+/d358e2e)**  
  
Date(Commit): Wed Feb 09 14:16:25 2011  
Code Review: [http://codereview.chromium.org/6459021](http://codereview.chromium.org/6459021)  
Regress: [mjsunit/regress/regress-1126.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1126.js)  
```javascript
try {
   eval('--');
   assertUnreachable();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d358e2e^!)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=d358e2e)  
[test/mjsunit/regress/regress-1126.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1126.js?cl=d358e2e)  
  

---   

## **regress-1122.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1122)**  
**[Commit: Fix a bug that occurs when functions are defined with more than 16,382 parameters.](https://chromium.googlesource.com/v8/v8/+/602d5cf)**  
  
Date(Commit): Wed Feb 09 12:46:22 2011  
Code Review: [http://codereview.chromium.org/6447007](http://codereview.chromium.org/6447007)  
Regress: [mjsunit/regress/regress-1122.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1122.js)  
```javascript
function function_with_n_params_and_m_args(n, m) {
  test_prefix = 'prefix ';
  test_suffix = ' suffix';
  var source = 'test_prefix + (function f(';
  for (var arg = 0; arg < n ; arg++) {
    if (arg != 0) source += ',';
    source += 'arg' + arg;
  }
  source += ') { return arg' + (n - n % 2) / 2 + '; })(';
  for (var arg = 0; arg < m ; arg++) {
    if (arg != 0) source += ',';
    source += arg;
  }
  source += ') + test_suffix';
  return eval(source);
}

assertEquals('prefix 4000 suffix',
             function_with_n_params_and_m_args(8000, 8000));
assertEquals('prefix 3000 suffix',
             function_with_n_params_and_m_args(6000, 8000));
assertEquals('prefix 5000 suffix',
             function_with_n_params_and_m_args(10000, 8000));
assertEquals('prefix 9000 suffix',
             function_with_n_params_and_m_args(18000, 18000));
assertEquals('prefix 16000 suffix',
             function_with_n_params_and_m_args(32000, 32000));
assertEquals('prefix undefined suffix',
             function_with_n_params_and_m_args(32000, 10000));

assertThrows("function_with_n_params_and_m_args(66000, 30000)");
assertThrows("function_with_n_params_and_m_args(30000, 66000)");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/602d5cf^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=602d5cf)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=602d5cf)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=602d5cf)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=602d5cf)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=602d5cf)  
...  
  

---   

## **regress-1118.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1118)**  
**[Commit: Fix an assertion failure in stack trace construction.](https://chromium.googlesource.com/v8/v8/+/991a1ca)**  
  
Date(Commit): Wed Feb 09 11:45:50 2011  
Code Review: [http://codereview.chromium.org/6465023](http://codereview.chromium.org/6465023)  
Regress: [mjsunit/regress/regress-1118.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1118.js)  
```javascript
function A() { }
%EnsureFeedbackVectorForFunction(A);
A.prototype.f = function() { }

function B() { }
%EnsureFeedbackVectorForFunction(B);

var o = new A();

function g() { try { return o.f(); } finally { }}
%EnsureFeedbackVectorForFunction(g);

function h() {
  for (var i = 0; i < 10; i++) {
    %OptimizeOsr();
    %PrepareFunctionForOptimization(h);
  }
  g();
}
%PrepareFunctionForOptimization(h);

h();
o = new B();
assertThrows("h()");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/991a1ca^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=991a1ca)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=991a1ca)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=991a1ca)  
[test/mjsunit/regress/regress-1118.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1118.js?cl=991a1ca)  
  

---   

## **regress-1125.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1125)**  
**[Commit: Use GC-safe version when setting elements.](https://chromium.googlesource.com/v8/v8/+/d724993)**  
  
Date(Commit): Wed Feb 09 11:38:10 2011  
Code Review: [http://codereview.chromium.org/6463001](http://codereview.chromium.org/6463001)  
Regress: [mjsunit/regress/regress-1125.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1125.js)  
```javascript
function f(x, y) {
  with ("abcdefghijxxxxxxxxxx")
    var y = {};
}

function g() {
  f.apply(this, arguments);
}

for (var i = 0; i < 150000; i++) {
  g(i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d724993^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=d724993)  
[test/mjsunit/regress/regress-1125.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1125.js?cl=d724993)  
  

---   

## **regress-1121.js (v8 issue)**  
   
**[getOwnPropertyNames can trigger "CHECK(object->IsJSObject()) failed"](https://crbug.com/v8/1121)**  
**[Commit: Check if Array.prototype.__proto__ has been reset to null.](https://chromium.googlesource.com/v8/v8/+/cf30cef)**  
  
Date(Commit): Tue Feb 08 19:56:44 2011  
Code Review: [http://codereview.chromium.org/6454004](http://codereview.chromium.org/6454004)  
Regress: [mjsunit/regress/regress-1121.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1121.js)  
```javascript
Array.prototype.__proto__ = null;
assertEquals([1, 2, 3], [1, 2, 3].slice());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf30cef^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=cf30cef)  
[test/mjsunit/regress/regress-1121.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1121.js?cl=cf30cef)  
  

---   

## **regress-1107.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1107)**  
**[Commit: Propagate exceptions thrown when setting elements.](https://chromium.googlesource.com/v8/v8/+/0273e81)**  
  
Date(Commit): Tue Feb 08 19:42:14 2011  
Code Review: [http://codereview.chromium.org/6451004](http://codereview.chromium.org/6451004)  
Regress: [mjsunit/regress/regress-1107.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1107.js)  
```javascript
Object.prototype.__defineGetter__(0, function(){});
assertThrows("x");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0273e81^!)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=0273e81)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=0273e81)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0273e81)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=0273e81)  
[test/mjsunit/getter-in-prototype.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/getter-in-prototype.js?cl=0273e81)  
...  
  

---   

## **regress-1119.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1119)**  
**[Commit: 1) Return failure if any of property sets failed;](https://chromium.googlesource.com/v8/v8/+/da8b72f)**  
  
Date(Commit): Tue Feb 08 19:04:17 2011  
Code Review: [http://codereview.chromium.org/6454011](http://codereview.chromium.org/6454011)  
Regress: [mjsunit/regress/regress-1119.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1119.js)  
```javascript
this.__defineSetter__("x", function() { hasBeenInvoked = true; });
this.__defineSetter__("y", function() { throw 'exception'; });

var hasBeenInvoked = false;
eval("try { } catch (e) { var x = false; }");
assertTrue(hasBeenInvoked);

try {
  eval("try { } catch (e) { var y = false; }");
  assertUnreachable();
} catch (e) {
  assertEquals('exception', e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da8b72f^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=da8b72f)  
[test/mjsunit/regress/regress-1119.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1119.js?cl=da8b72f)  
  

---   

## **regress-1110.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1110)**  
**[Commit: Fix wrong assumption in parser that parsing a function literal cannot throw an exception.](https://chromium.googlesource.com/v8/v8/+/096c215)**  
  
Date(Commit): Tue Feb 08 18:46:13 2011  
Code Review: [http://codereview.chromium.org/6453009](http://codereview.chromium.org/6453009)  
Regress: [mjsunit/regress/regress-1110.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1110.js)  
```javascript
try {
  eval("function Crash() { assertUnreachable(); continue;if (Crash) {  } }");
  Crash();
  assertUnreachable();
} catch (e) {
  assertTrue(e instanceof SyntaxError);
  assertTrue(/continue/.test(e.message));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/096c215^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=096c215)  
[test/mjsunit/regress/regress-1110.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1110.js?cl=096c215)  
  

---   

## **regress-1112.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1112)**  
**[Commit: Fix issues with using defineProperty on the global proxy object.](https://chromium.googlesource.com/v8/v8/+/8c6c273)**  
  
Date(Commit): Tue Feb 08 16:31:58 2011  
Code Review: [http://codereview.chromium.org/6452004](http://codereview.chromium.org/6452004)  
Regress: [mjsunit/regress/regress-1112.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1112.js)  
```javascript
Object.defineProperty(this,
                      1,
                      { configurable: true, enumerable: true, value: 3 });
assertEquals(3, this[1]);
assertTrue(this.hasOwnProperty("1"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c6c273^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=8c6c273)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=8c6c273)  
[test/mjsunit/regress/regress-1112.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1112.js?cl=8c6c273)  
  

---   

## **regress-1117.js (v8 issue)**  
   
**[When crankshaft is enabled we might do wrong integer multiplication when the left operand is constant 0](https://crbug.com/v8/1117)**  
**[Commit: x64: Add MulI and DivI to lithium instructions.](https://chromium.googlesource.com/v8/v8/+/f649660)**  
  
Date(Commit): Tue Feb 08 14:37:50 2011  
Code Review: [http://codereview.chromium.org/6448001](http://codereview.chromium.org/6448001)  
Regress: [mjsunit/regress/regress-1117.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1117.js)  
```javascript
function foo(y) {
  return 0 * y;
};
%PrepareFunctionForOptimization(foo);
assertEquals(1 / foo(-42), -Infinity);
assertEquals(1 / foo(-42), -Infinity);
%OptimizeFunctionOnNextCall(foo);
assertEquals(1 / foo(-42), -Infinity);

function bar(x) {
  return x * 0;
};
%PrepareFunctionForOptimization(bar);
assertEquals(Infinity, 1 / bar(5));
assertEquals(Infinity, 1 / bar(5));
%OptimizeFunctionOnNextCall(bar);
assertEquals(-Infinity, 1 / bar(-5));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f649660^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=f649660)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=f649660)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=f649660)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=f649660)  
[src/x64/assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/assembler-x64.cc?cl=f649660)  
...  
  

---   

## **regress-1170.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1170)**  
**[Commit: Correct propagation of exceptions from setters.](https://chromium.googlesource.com/v8/v8/+/2f32f27)**  
  
Date(Commit): Tue Feb 08 14:04:27 2011  
Code Review: [http://codereview.chromium.org/6451003](http://codereview.chromium.org/6451003)  
Regress: [mjsunit/regress/regress-1170.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1170.js)  
```javascript
var setter_value = 0;

this.__defineSetter__("a", function(v) { setter_value = v; });
eval("var a = 1");
assertEquals(1, setter_value);
assertFalse("value" in Object.getOwnPropertyDescriptor(this, "a"));

eval("with({}) { eval('var a = 2') }");
assertTrue("get" in Object.getOwnPropertyDescriptor(this, "a"));
assertFalse("value" in Object.getOwnPropertyDescriptor(this, "a"));
assertEquals(2, setter_value);

this.__defineSetter__("a", function(v) { assertUnreachable(); });
eval("function a() {}");
assertTrue("value" in Object.getOwnPropertyDescriptor(this, "a"));

this.__defineSetter__("b", function(v) { setter_value = v; });
try {
  eval("const b = 3");
} catch(e) { }
assertEquals(2, setter_value);

try {
  eval("with({}) { eval('const b = 23') }");
} catch(e) {
  assertInstanceof(e, TypeError);
}

this.__defineSetter__("c", function(v) { throw 42; });
try {
  eval("var c = 1");
  assertUnreachable();
} catch(e) {
  assertEquals(42, e);
  assertFalse("value" in Object.getOwnPropertyDescriptor(this, "c"));
}




__proto__.__defineSetter__("aa", function(v) { assertUnreachable(); });
eval("var aa = 1");
assertTrue(this.hasOwnProperty("aa"));

__proto__.__defineSetter__("bb", function(v) { assertUnreachable(); });
eval("with({}) { eval('var bb = 2') }");
assertTrue(this.hasOwnProperty("bb"));

__proto__.__defineSetter__("cc", function(v) { assertUnreachable(); });
eval("function cc() {}");
assertTrue(this.hasOwnProperty("cc"));

__proto__.__defineSetter__("dd", function(v) { assertUnreachable(); });
try {
  eval("const dd = 23");
} catch(e) {
  assertUnreachable();
}

try {
  eval("with({}) { eval('const dd = 23') }");
} catch(e) {
  assertInstanceof(e, TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f32f27^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=2f32f27)  
[test/mjsunit/regress/regress-1105.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1105.js?cl=2f32f27)  
  

---   

## **regress-1104.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1104)**  
**[Commit: Fix a possible duplicate AST ID for deoptimization.](https://chromium.googlesource.com/v8/v8/+/bf3c3eb)**  
  
Date(Commit): Tue Feb 08 14:00:22 2011  
Code Review: [http://codereview.chromium.org/6453004](http://codereview.chromium.org/6453004)  
Regress: [mjsunit/regress/regress-1104.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1104.js)  
```javascript
function test(f) {
  function f() {}
  function f() {}
  return arguments;
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf3c3eb^!)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=bf3c3eb)  
[test/mjsunit/regress/regress-1104.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1104.js?cl=bf3c3eb)  
  

---   

## **regress-1120.js (v8 issue)**  
   
**[Object.isExtensible called on the global object when this has been frozen returns true](https://crbug.com/v8/1120)**  
**[Commit: Make sure that we do not call is_extensible on the global proxy.](https://chromium.googlesource.com/v8/v8/+/20f2c1c)**  
  
Date(Commit): Tue Feb 08 13:09:07 2011  
Code Review: [http://codereview.chromium.org/6450003](http://codereview.chromium.org/6450003)  
Regress: [mjsunit/regress/regress-1120.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1120.js)  
```javascript
var obj = this;
Object.freeze(obj);
assertFalse(Object.isExtensible(obj));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/20f2c1c^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=20f2c1c)  
[test/mjsunit/regress/regress-1120.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1120.js?cl=20f2c1c)  
  

---   

## **regress-1103.js (v8 issue)**  
   
**[Permission denied](https://crbug.com/v8/1103)**  
**[Commit: Make sure that we never call prevent extension on the global proxy,](https://chromium.googlesource.com/v8/v8/+/81787f9)**  
  
Date(Commit): Tue Feb 08 12:41:16 2011  
Code Review: [http://codereview.chromium.org/6454001](http://codereview.chromium.org/6454001)  
Regress: [mjsunit/regress/regress-1103.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1103.js)  
```javascript
var obj = this;
obj = Object.freeze(obj);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/81787f9^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=81787f9)  
[test/mjsunit/regress/regress-1103.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1103.js?cl=81787f9)  
  

---   

## **regress-1099.js (v8 issue)**  
   
**[Crash in JSObject::LocalLookup](https://crbug.com/v8/1099)**  
**[Commit: Restore context after LApplyArguments.](https://chromium.googlesource.com/v8/v8/+/10f715e)**  
  
Date(Commit): Fri Feb 04 15:42:02 2011  
Code Review: [http://codereview.chromium.org/6246106](http://codereview.chromium.org/6246106)  
Regress: [mjsunit/regress/regress-1099.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1099.js)  
```javascript
function X() {
  var slot = "foo"; return function (a) { return slot === a; }
}

function Y(x) {
  var slot = "bar";
  return function (a) {
    x.apply(this, arguments);
    return slot === 'bar';
  };
}

var y = Y(X());
%PrepareFunctionForOptimization(y);

for (var i = 0; i < 5; i++) {
  assertTrue(y("foo"));
}

%OptimizeFunctionOnNextCall(y);
assertTrue(y("foo"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10f715e^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=10f715e)  
[test/mjsunit/regress/regress-1099.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1099.js?cl=10f715e)  
  

---   

## **regress-1092.js (v8 issue)**  
   
**[More than 3 assignments to 'this' properties at global scope will break Heap invariants.](https://crbug.com/v8/1092)**  
**[Commit: Fix bugs 992, 1083 and 1092](https://chromium.googlesource.com/v8/v8/+/c894b1f)**  
  
Date(Commit): Thu Feb 03 19:29:10 2011  
Code Review: [http://codereview.chromium.org/6286060](http://codereview.chromium.org/6286060)  
Regress: [mjsunit/regress/regress-1092.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1092.js)  
```javascript
this.w = 0;
this.x = 1;
this.y = 2;
this.z = 3;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c894b1f^!)  
[.gitignore](https://cs.chromium.org/chromium/src/v8/.gitignore?cl=c894b1f)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=c894b1f)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c894b1f)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=c894b1f)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=c894b1f)  
...  
  

---   

## **regress-deopt-gc.js (other issue)**  
   
**[Commit: Add regression test for the deoptimizer immediately followed by gc bug.](https://chromium.googlesource.com/v8/v8/+/a2aa848)**  
  
Date(Commit): Thu Feb 03 13:47:27 2011  
Code Review: [http://codereview.chromium.org/6312118](http://codereview.chromium.org/6312118)  
Regress: [mjsunit/regress/regress-deopt-gc.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-gc.js)  
```javascript
(function() { var a = 10; a++; })();

function opt_me() {
  deopt();
}

function deopt() {
  // Make sure we don't inline this function
  try { var a = 42; } catch(o) {};
  %DeoptimizeFunction(opt_me);
  gc();
}


opt_me();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2aa848^!)  
[src/extensions/gc-extension.cc](https://cs.chromium.org/chromium/src/v8/src/extensions/gc-extension.cc?cl=a2aa848)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=a2aa848)  
[test/mjsunit/regress/regress-deopt-gc.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-deopt-gc.js?cl=a2aa848)  
  
  
---   

## **regress-3408144.js (other issue)**  
   
**[Commit: Fix code generation bug on ARM in classic codegen.](https://chromium.googlesource.com/v8/v8/+/0097f00)**  
  
Date(Commit): Wed Feb 02 14:14:55 2011  
Code Review: [http://codereview.chromium.org/6246045](http://codereview.chromium.org/6246045)  
Regress: [mjsunit/regress/regress-3408144.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3408144.js)  
```javascript
function foo() {
  return (0 > ("10"||10) - 1);
}

assertFalse(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0097f00^!)  
[src/arm/jump-target-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/jump-target-arm.cc?cl=0097f00)  
[src/arm/virtual-frame-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/virtual-frame-arm.cc?cl=0097f00)  
[test/mjsunit/regress/regress-3408144.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3408144.js?cl=0097f00)  
  
  
---   

## **regress-1079.js (v8 issue)**  
   
**[Handling of optimized frames which have called without deoptimization index is unsafe](https://crbug.com/v8/1079)**  
**[Commit: Partial fix for V8 issue 1079.](https://chromium.googlesource.com/v8/v8/+/f114973)**  
  
Date(Commit): Wed Feb 02 13:55:29 2011  
Code Review: [http://codereview.chromium.org/6250105](http://codereview.chromium.org/6250105)  
Regress: [mjsunit/regress/regress-1079.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1079.js)  
```javascript
function optimized() {
  return unoptimized.apply(null, arguments);
}
%PrepareFunctionForOptimization(optimized);

function unoptimized() {
  with ({}) {
    return optimized.arguments;
  }
}

for (var i = 0; i < 5; ++i) {
  assertEquals(3, optimized(1, 2, 3).length);
}
%OptimizeFunctionOnNextCall(optimized);
assertEquals(3, optimized(1, 2, 3).length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f114973^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=f114973)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=f114973)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=f114973)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=f114973)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=f114973)  
...  
  

---   

## **regress-71647.js (chromium issue)**  
   
**[Chrome: Crash Report - Stack Signature: v8::internal::Runtime_Typeof-1B8EFED](https://crbug.com/71647)**  
**[Commit: Require typed input representation for HTypeof hydrogen instruction.](https://chromium.googlesource.com/v8/v8/+/6751627)**  
  
Date(Commit): Wed Feb 02 09:52:57 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "ReleaseBlock-Beta", "M-10", "Crash-TopCrasher", "crash-Reproducible", "bulkmove"]  
Code Review: [http://codereview.chromium.org/6410025](http://codereview.chromium.org/6410025)  
Regress: [mjsunit/regress/regress-71647.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-71647.js)  
```javascript
var qe = 'object';

function g() {
  for (var i = 0; i < 10000; i++) typeof i === qe;
}

g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6751627^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=6751627)  
[test/mjsunit/regress/regress-71647.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-71647.js?cl=6751627)  
  

---   

## **regress-1083.js (v8 issue)**  
   
**[Object.defineProperty(this, "Object", {enumerable:true}); breaks invariant](https://crbug.com/v8/1083)**  
**[Commit: Fix bugs 992 and 1083](https://chromium.googlesource.com/v8/v8/+/9c89aa6)**  
  
Date(Commit): Tue Feb 01 17:08:14 2011  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@6561](http://v8.googlecode.com/svn/branches/bleeding_edge@6561)  
Regress: [mjsunit/regress/regress-1083.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1083.js)  
```javascript
Object.defineProperty(this, 'Object', {enumerable:true});

var desc = Object.getOwnPropertyDescriptor(this, 'Object');
assertTrue(desc.enumerable);
assertTrue(desc.configurable);
assertFalse(desc.hasOwnProperty('get'));
assertFalse(desc.hasOwnProperty('set'));
assertTrue(desc.hasOwnProperty('value'));
assertTrue(desc.writable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c89aa6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=9c89aa6)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9c89aa6)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=9c89aa6)  
[test/mjsunit/object-define-property.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-define-property.js?cl=9c89aa6)  
[test/mjsunit/regress/regress-1083.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083.js?cl=9c89aa6)  
...  
  

---   

## **regress-992.js (v8 issue)**  
   
**[Object.defineProperty does not behave correctly when passed a generic descriptor](https://crbug.com/v8/992)**  
**[Commit: Fix bugs 992 and 1083](https://chromium.googlesource.com/v8/v8/+/9c89aa6)**  
  
Date(Commit): Tue Feb 01 17:08:14 2011  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@6561](http://v8.googlecode.com/svn/branches/bleeding_edge@6561)  
Regress: [mjsunit/regress/regress-992.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-992.js)  
```javascript
var obj =  { get p() { return 42; }  };
var desc = Object.getOwnPropertyDescriptor(obj, 'p');
var getter = desc.get;

Object.defineProperty(obj, 'p', {enumerable: false });
assertEquals(obj.p, 42);
desc = Object.getOwnPropertyDescriptor(obj, 'p');
assertFalse(desc.enumerable);
assertTrue(desc.configurable);
assertEquals(desc.get, getter);
assertEquals(desc.set, undefined);
assertFalse(desc.hasOwnProperty('value'));
assertFalse(desc.hasOwnProperty('writable'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c89aa6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=9c89aa6)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9c89aa6)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=9c89aa6)  
[test/mjsunit/object-define-property.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-define-property.js?cl=9c89aa6)  
[test/mjsunit/regress/regress-1083.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083.js?cl=9c89aa6)  
...  
  

---   

## **regress-1085.js (v8 issue)**  
   
**[Minus infinity treated incorrectly in optimized division.](https://crbug.com/v8/1085)**  
**[Commit: Fix a bug in the placement of minus-zero checks and in GVN.](https://chromium.googlesource.com/v8/v8/+/4e7ddab)**  
  
Date(Commit): Mon Jan 31 12:36:54 2011  
Code Review: [http://codereview.chromium.org/6260035](http://codereview.chromium.org/6260035)  
Regress: [mjsunit/compiler/regress-1085.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1085.js)  
```javascript
function f(x) { return 1 / Math.min(1, x); }

%PrepareFunctionForOptimization(f);
for (var i = 0; i < 5; ++i) f(1);
%OptimizeFunctionOnNextCall(f);

assertEquals(-Infinity, f(-0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e7ddab^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=4e7ddab)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=4e7ddab)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=4e7ddab)  
[test/mjsunit/compiler/regress-1085.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1085.js?cl=4e7ddab)  
  

---   

## **regress-70066.js (chromium issue)**  
   
**[with + delete statements cause unexpected delete of global variables](https://crbug.com/70066)**  
**[Commit: Fix a bug in delete for lookup slots.](https://chromium.googlesource.com/v8/v8/+/9c2d52e)**  
  
Date(Commit): Mon Jan 24 14:03:30 2011  
Components: Blink>JavaScriptBlink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/6280013](http://codereview.chromium.org/6280013)  
Regress: [mjsunit/regress/regress-70066.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-70066.js)  
```javascript
x = 0;

function test1() {
  var value = 1;
  var status;
  with ({}) { status = delete value; }
  return value + ":" + status;
}

assertEquals("1:false", test1(), "test1");
assertEquals(0, x, "test1");  // Global x is undisturbed.


function test2() {
  function f() {
    with ({}) { return delete value; }
  }
  var value = 2;
  var status = f();
  return value + ":" + status;
}

assertEquals("2:false", test2(), "test2");
assertEquals(0, x, "test2");  // Global x is undisturbed.


function test3(value) {
  var status;
  with ({}) { status = delete value; }
  return value + ":" + status;
}

assertEquals("3:false", test3(3), "test3");
assertEquals(0, x, "test3");  // Global x is undisturbed.


function test4(value) {
  function f() {
    with ({}) { return delete value; }
  }
  var status = f();
  return value + ":" + status;
}

assertEquals("4:false", test4(4), "test4");
assertEquals(0, x, "test4");  // Global x is undisturbed.


function test5(value) {
  var status;
  with ({}) { status = delete value; }
  return arguments[0] + ":" + status;
}

assertEquals("5:false", test5(5), "test5");
assertEquals(0, x, "test5");  // Global x is undisturbed.

function test6(value) {
  function f() {
    with ({}) { return delete value; }
  }
  var status = f();
  return arguments[0] + ":" + status;
}

assertEquals("6:false", test6(6), "test6");
assertEquals(0, x, "test6");  // Global x is undisturbed.


function test7(object) {
  with (object) { return delete value; }
}

var o = {value: 7};
assertEquals(true, test7(o), "test7");
assertEquals(void 0, o.value, "test7");
assertEquals(0, x, "test7");  // Global x is undisturbed.


function test8() {
  with ({}) { return delete x; }
}

assertEquals(true, test8(), "test8");
assertThrows("x");  // Global x should be deleted.


function test9() {
  with ({}) { return delete x; }
}

assertThrows("x");  // Make sure it's not there.
assertEquals(true, test9(), "test9");


var y = 10;
function test10() {
  with ({}) { return delete y; }
}

assertEquals(false, test10(), "test10");
assertEquals(10, y, "test10");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c2d52e^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=9c2d52e)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=9c2d52e)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=9c2d52e)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=9c2d52e)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9c2d52e)  
...  
  

---   

## **regress-1060.js (v8 issue)**  
   
**[ASSERT in compiler](https://crbug.com/v8/1060)**  
**[Commit: Fix an assertion failure in the full code generator.](https://chromium.googlesource.com/v8/v8/+/70910af)**  
  
Date(Commit): Wed Jan 19 15:26:54 2011  
Code Review: [http://codereview.chromium.org/6368007](http://codereview.chromium.org/6368007)  
Regress: [mjsunit/regress/regress-1060.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1060.js)  
```javascript
function f(x) { arguments; return x() + x(); }

assertEquals("hesthest", f(function () { return "hest"; }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70910af^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=70910af)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=70910af)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=70910af)  
[test/mjsunit/regress/regress-1060.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1060.js?cl=70910af)  
  

---   

## **regress-serialized-slots.js (other issue)**  
   
**[Commit: Properly create variables to access outer arguments and function names.](https://chromium.googlesource.com/v8/v8/+/49144ee)**  
  
Date(Commit): Wed Jan 19 08:16:17 2011  
Code Review: [http://codereview.chromium.org/6266007](http://codereview.chromium.org/6266007)  
Regress: [mjsunit/compiler/regress-serialized-slots.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-serialized-slots.js)  
```javascript
function runner(f, expected) {
  for (var i = 0; i < 10000; i++) {  // Loop to trigger optimization.
    assertEquals(expected, f.call(this, 10));
  }
}

Function.prototype.bind = function(thisObject)
{
    var func = this;
    var args = Array.prototype.slice.call(arguments, 1);
    function bound()
    {
      // Note outer function parameter access (|thisObject|).
      return func.apply(
          thisObject,
          args.concat(Array.prototype.slice.call(arguments, 0)));
    }
    return bound;
}

function sum(x, y) {
  return x + y;
}

function test(n) {
  runner(sum.bind(this, n), n + 10);
}

test(1);
test(42);
test(239);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49144ee^!)  
[src/scopeinfo.cc](https://cs.chromium.org/chromium/src/v8/src/scopeinfo.cc?cl=49144ee)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=49144ee)  
[test/mjsunit/compiler/regress-serialized-slots.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-serialized-slots.js?cl=49144ee)  
  
  
---   

## **regress-closures-with-eval.js (other issue)**  
   
**[Commit: Make closures optimizable by Crankshaft compiler.](https://chromium.googlesource.com/v8/v8/+/fae90d4)**  
  
Date(Commit): Mon Jan 17 08:11:03 2011  
Code Review: [http://codereview.chromium.org/5753005](http://codereview.chromium.org/5753005)  
Regress: [mjsunit/compiler/regress-closures-with-eval.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-closures-with-eval.js)  
```javascript
function withEval(expr, filter) {
  %PrepareFunctionForOptimization(filter);
  function walk(v) {
    for (var i in v) {
      for (var i in v) {}
    }
    %OptimizeFunctionOnNextCall(filter);
    return filter(v);
  }

  var o = eval(expr);
  return walk(o);
}

function makeTagInfoJSON(n) {
  var a = new Array(n);
  for (var i = 0; i < n; i++) a.push('{}');
  return a;
}

var expr = '([' + makeTagInfoJSON(128).join(', ') + '])';

%PrepareFunctionForOptimization(withEval);
for (var n = 0; n < 5; n++) {
  withEval(expr, function(a) { return a; });
}
%OptimizeFunctionOnNextCall(withEval);
withEval(expr, function(a) { return a; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fae90d4^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=fae90d4)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=fae90d4)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=fae90d4)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=fae90d4)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=fae90d4)  
...  
  
  
---   

## **regress-1020.js (v8 issue)**  
   
**[Optimized instanceof can return 0](https://crbug.com/v8/1020)**  
**[Commit: Fix bug in instanceof stub](https://chromium.googlesource.com/v8/v8/+/9bc3a16)**  
  
Date(Commit): Wed Jan 05 14:19:12 2011  
Code Review: [http://codereview.chromium.org/6014013](http://codereview.chromium.org/6014013)  
Regress: [mjsunit/regress/regress-1020.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1020.js)  
```javascript
function isObject(o) {
  return o instanceof Object;
}

assertTrue(isObject(Object));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bc3a16^!)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=9bc3a16)  
[test/mjsunit/regress/regress-1020.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1020.js?cl=9bc3a16)  
  

---   

## **regress-900.js (v8 issue)**  
   
**[__definegetter__, __definesetter__ and Object.defineProperty does not work correctly on array indices.](https://crbug.com/v8/900)**  
**[Commit: Allow getters and setters on JSArray elements.](https://chromium.googlesource.com/v8/v8/+/aa396c5)**  
  
Date(Commit): Tue Jan 04 13:59:34 2011  
Code Review: [http://codereview.chromium.org/5959009](http://codereview.chromium.org/5959009)  
Regress: [mjsunit/regress/regress-900.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900.js)  
```javascript
var a = [];
var b = {}
Object.defineProperty(a, "1", {get: function() {return "foo";}});
Object.defineProperty(
    b, "1", {get: function() {return "bar";}, set: function() {this.x = 42;}});
assertEquals(a[1], 'foo');
assertEquals(b[1], 'bar');
b[1] = 'foobar';
assertEquals(b[1], 'bar');
assertEquals(b.x, 42);

var desc = Object.getOwnPropertyDescriptor(b, "1");
assertEquals(desc['writable'], undefined);
assertFalse(desc['enumerable']);
assertFalse(desc['configurable']);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa396c5^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=aa396c5)  
[test/mjsunit/indexed-accessors.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/indexed-accessors.js?cl=aa396c5)  
[test/mjsunit/regress/regress-900.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-900.js?cl=aa396c5)  
[test/mozilla/mozilla.status](https://cs.chromium.org/chromium/src/v8/test/mozilla/mozilla.status?cl=aa396c5)  
  

---   

## **regress-1015.js (v8 issue)**  
   
**[Object literals incorrectly invoke inherited setters for initialization](https://crbug.com/v8/1015)**  
**[Commit: Don't let JSON parsed objects hit inherited setters.](https://chromium.googlesource.com/v8/v8/+/e7ecb74)**  
  
Date(Commit): Tue Jan 04 12:19:55 2011  
Code Review: [http://codereview.chromium.org/6101001](http://codereview.chromium.org/6101001)  
Regress: [mjsunit/regress/regress-1015.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1015.js)  
```javascript
function mkFail(message) {
  return function () { assertUnreachable(message); }
}

Object.defineProperty(Object.prototype, "foo",
                      {get: mkFail("oget"), set: mkFail("oset")});
Object.defineProperty(Array.prototype, "2",
                      {get: mkFail("aget"), set: mkFail("aset")});

function inFunction() {
  for (var i = 0; i < 10; i++) {
    // in loop.
    var ja = JSON.parse('[1,2,3,4]');
    var jo = JSON.parse('{"bar": 10, "foo": 20}')
    var jop = JSON.parse('{"bar": 10, "__proto__": { }, "foo": 20}')
    var a = [1,2,3,4];
    var o = { bar: 10, foo: 20 };
    var op = { __proto__: { set bar(v) { assertUnreachable("bset"); } },
               bar: 10 };
  }
}

for (var i = 0; i < 10; i++) {
  // In global scope.
  var ja = JSON.parse('[1,2,3,4]');
  var jo = JSON.parse('{"bar": 10, "foo": 20}')
  var jop = JSON.parse('{"bar": 10, "__proto__": { }, "foo": 20}')
  var a = [1,2,3,4];
  var o = { bar: 10, foo: 20 };
  var op = { __proto__: { set bar(v) { assertUnreachable("bset"); } },
             bar: 10 };
  // In function scope.
  inFunction();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e7ecb74^!)  
[src/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap-inl.h?cl=e7ecb74)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e7ecb74)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=e7ecb74)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=e7ecb74)  
[test/mjsunit/bugs/bug-1015.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-1015.js?cl=e7ecb74)  
  

---   

## **regress-1017.js (v8 issue)**  
   
**[String parsing crash when parsing a uc16 character when current size is exactly 32](https://crbug.com/v8/1017)**  
**[Commit: Fix bug that happens when the first non-ASCII character of a literal is at a power-of-two position.](https://chromium.googlesource.com/v8/v8/+/59aea66)**  
  
Date(Commit): Tue Jan 04 11:25:59 2011  
Code Review: [http://codereview.chromium.org/6044009](http://codereview.chromium.org/6044009)  
Regress: [mjsunit/regress/regress-1017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1017.js)  
```javascript
assertEquals(33, "12345678901234567890123456789012\u2028".length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/59aea66^!)  
[src/scanner-base.h](https://cs.chromium.org/chromium/src/v8/src/scanner-base.h?cl=59aea66)  
[test/mjsunit/regress/regress-1017.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1017.js?cl=59aea66)  
  

---   
