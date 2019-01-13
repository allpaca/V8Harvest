# V8Harvest  
The Harvest of V8 regress in 2009.  
  

## **regress-524.js (v8 issue)**  
   
**[Issue: Creating a million live slow-case objects exhausts map space.](https://crbug.com/v8/524)**  
**[Commit: Extend the maximum size map space](https://chromium.googlesource.com/v8/v8/+/44b7c59)**  
  
Date(Commit): Thu Dec 17 08:53:18 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/504026](http://codereview.chromium.org/504026)  
Regress: [mjsunit/regress/regress-524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-524.js)  
```javascript
var i = 500000
var a = new Array(i)
for (var j = 0; j < i; j++) { var o = {}; o.x = 42; delete o.x; a[j] = o; }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/44b7c59^!)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=44b7c59)  
[src/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap-inl.h?cl=44b7c59)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=44b7c59)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=44b7c59)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=44b7c59)  
...  
  

---   

## **regress-545.js (v8 issue)**  
   
**[Issue: Fast compiler crash when using "this" in different contexts.](https://crbug.com/v8/545)**  
**[Commit: Fix for issue 545: don't reuse this VariableProxy.](https://chromium.googlesource.com/v8/v8/+/5bbb1d7)**  
  
Date(Commit): Tue Dec 08 09:43:51 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/464069](http://codereview.chromium.org/464069)  
Regress: [mjsunit/regress/regress-545.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-545.js)  
```javascript
try {
 new IsPrimitive(load())?this.join():String('&#10;').charCodeAt((!this>Math));
} catch (e) {}


this + !this;

this + (this ? 1 : 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bbb1d7^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=5bbb1d7)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=5bbb1d7)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=5bbb1d7)  
[test/mjsunit/regress/regress-545.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-545.js?cl=5bbb1d7)  
  

---   

## **regress-540.js (v8 issue)**  
   
**[Issue: ASSERT in triggered in DeclareContextSlot](https://crbug.com/v8/540)**  
**[Commit: Fix issue 540 by handling the case that a declaration is in the](https://chromium.googlesource.com/v8/v8/+/7266bd0)**  
  
Date(Commit): Fri Dec 04 11:59:09 2009  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=540](http://code.google.com/p/v8/issues/detail?id=540)  
Regress: [mjsunit/regress/regress-540.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-540.js)  
```javascript
function f(x, y) { eval(x); return y(); }
var result = f("function y() { return 1; }", function () { return 0; })
assertEquals(1, result);

result =
    (function (x) {
      function x() { return 3; }
      return x();
    })(function () { return 2; });
assertEquals(3, result);

result =
    (function (x) {
      function x() { return 5; }
      return arguments[0]();
    })(function () { return 4; });
assertEquals(5, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7266bd0^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=7266bd0)  
[test/mjsunit/regress/regress-540.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-540.js?cl=7266bd0)  
  

---   

## **regress-r3391.js (other issue)**  
   
**[Commit: Fix toLocaleString-related breakage on buildbot.](https://chromium.googlesource.com/v8/v8/+/a0e12a3)**  
  
Date(Commit): Tue Dec 01 14:19:23 2009  
Code Review: [http://codereview.chromium.org/449055](http://codereview.chromium.org/449055)  
Regress: [mjsunit/regress/regress-r3391.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-r3391.js)  
```javascript
var evil_called = 0;
var evil_locale_called = 0;
var exception_thrown = 0;

function evil_to_string() {
  evil_called++;
  return this;
}

function evil_to_locale_string() {
  evil_locale_called++;
  return this;
}

var o = {toString: evil_to_string, toLocaleString: evil_to_locale_string};

try {
  [o].toLocaleString();
} catch(e) {
  exception_thrown++;
}

assertEquals(1, evil_called, "evil1");
assertEquals(1, evil_locale_called, "local1");
assertEquals(1, exception_thrown, "exception1");

try {
  [o].toString();
} catch(e) {
  exception_thrown++;
}

assertEquals(2, evil_called, "evil2");
assertEquals(1, evil_locale_called, "local2");
assertEquals(2, exception_thrown, "exception2");

try {
  [o].join(o);
} catch(e) {
  exception_thrown++;
}

assertEquals(3, evil_called, "evil3");
assertEquals(1, evil_locale_called, "local3");
assertEquals(3, exception_thrown, "exception3");
print("ok");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0e12a3^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=a0e12a3)  
[test/mjsunit/regress/regress-r3391.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-r3391.js?cl=a0e12a3)  
  
  
---   

## **regress-526.js (v8 issue)**  
   
**[Issue: Fast top-level compiler causes DevToolsSanityCheck tests to fail.](https://crbug.com/v8/526)**  
**[Commit: Fix bug in the fast compiler's object literal code](https://chromium.googlesource.com/v8/v8/+/1c90793)**  
  
Date(Commit): Thu Nov 26 21:13:20 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/444001](http://codereview.chromium.org/444001)  
Regress: [mjsunit/regress/regress-526.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-526.js)  
```javascript
var o = { foo: function() { }, get bar() { return {x:42} } };

assertEquals(42, o.bar.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1c90793^!)  
[src/arm/fast-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/fast-codegen-arm.cc?cl=1c90793)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=1c90793)  
[src/ia32/fast-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/fast-codegen-ia32.cc?cl=1c90793)  
[src/x64/fast-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/fast-codegen-x64.cc?cl=1c90793)  
[test/mjsunit/compiler/objectliterals.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/objectliterals.js?cl=1c90793)  
...  
  

---   

## **regress-515.js (v8 issue)**  
   
**[Issue: String replace with regexp crash](https://crbug.com/v8/515)**  
**[Commit: Fix crash in string replace with regexp.  If the suffix of the subject](https://chromium.googlesource.com/v8/v8/+/3cf9ce4)**  
  
Date(Commit): Wed Nov 18 18:48:04 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/402056](http://codereview.chromium.org/402056)  
Regress: [mjsunit/regress/regress-515.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-515.js)  
```javascript
var length = 2048;
var s = "";
for (var i = 0; i < 2048; i++) {
  s += '.';
}

var string = s + 'x' + s + 'x' + s;

string.replace(/x/g, "")  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cf9ce4^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3cf9ce4)  
[test/mjsunit/regress/regress-515.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-515.js?cl=3cf9ce4)  
  

---   

## **regress-503.js (v8 issue)**  
   
**[Issue: undefined <= undefined on ARM](https://crbug.com/v8/503)**  
**[Commit: Fix bug 503: undefined <= undefined should return false on ARM.](https://chromium.googlesource.com/v8/v8/+/cc3896d)**  
  
Date(Commit): Mon Nov 16 14:12:27 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/399001](http://codereview.chromium.org/399001)  
Regress: [mjsunit/regress/regress-503.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-503.js)  
```javascript
assertTrue(undefined == undefined, 1);
assertFalse(undefined <= undefined, 2);
assertFalse(undefined >= undefined, 3);
assertFalse(undefined < undefined, 4);
assertFalse(undefined > undefined, 5);

assertTrue(null == null, 6);
assertTrue(null <= null, 7);
assertTrue(null >= null, 8);
assertFalse(null < null, 9);
assertFalse(null > null, 10);

assertTrue(void 0 == void 0, 11);
assertFalse(void 0 <= void 0, 12);
assertFalse(void 0 >= void 0, 13);
assertFalse(void 0 < void 0, 14);
assertFalse(void 0 > void 0, 15);

var x = void 0;

assertTrue(x == x, 16);
assertFalse(x <= x, 17);
assertFalse(x >= x, 18);
assertFalse(x < x, 19);
assertFalse(x > x, 20);

var not_undefined = [null, 0, 1, 1/0, -1/0, "", true, false];
for (var i = 0; i < not_undefined.length; i++) {
  x = not_undefined[i];

  assertTrue(x == x, "" + 21 + x);
  assertTrue(x <= x, "" + 22 + x);
  assertTrue(x >= x, "" + 23 + x);
  assertFalse(x < x, "" + 24 + x);
  assertFalse(x > x, "" + 25 + x);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc3896d^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=cc3896d)  
[test/mjsunit/regress/regress-503.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-503.js?cl=cc3896d)  
  

---   

## **regress-2249423.js (other issue)**  
   
**[Commit: Add a regression test that exposes a stack corruption problem.](https://chromium.googlesource.com/v8/v8/+/2e3e770)**  
  
Date(Commit): Fri Nov 13 13:58:48 2009  
Code Review: [http://code.google.com/p/chromium/issues/detail?id=27227](http://code.google.com/p/chromium/issues/detail?id=27227)  
Regress: [mjsunit/regress/regress-2249423.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2249423.js)  
```javascript
function top() {
 function g(a, b) {}
 function t() {
   for (var i=0; i<1; ++i) {
     g(32768, g());
   }
 }
 t();
}
top();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2e3e770^!)  
[test/mjsunit/regress/regress-2249423.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2249423.js?cl=2e3e770)  
  
  
---   

## **regress-502.js (v8 issue)**  
   
**[Issue: Missing bailout in inline constructor code](https://crbug.com/v8/502)**  
**[Commit: Fix inline constructor code bailout.](https://chromium.googlesource.com/v8/v8/+/2252cc1)**  
  
Date(Commit): Wed Nov 11 09:00:09 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/392001](http://codereview.chromium.org/392001)  
Regress: [mjsunit/regress/regress-502.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-502.js)  
```javascript
var X = 'x';
function C() { this[X] = 42; }
var a = new C();
var b = new C();
assertEquals(42, a.x);
assertEquals(42, b.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2252cc1^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=2252cc1)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=2252cc1)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=2252cc1)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=2252cc1)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=2252cc1)  
...  
  

---   

## **regress-486.js (v8 issue)**  
   
**[Issue: case-insensive regex matching does not work for Cyrillic (and perhaps Greek, either)](https://crbug.com/v8/486)**  
**[Commit: Fix bug 486, Cyrillic character ranges in case independent regexps.](https://chromium.googlesource.com/v8/v8/+/57c919e)**  
  
Date(Commit): Fri Nov 06 11:15:20 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/361033](http://codereview.chromium.org/361033)  
Regress: [mjsunit/regress/regress-486.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-486.js)  
```javascript
var st = "\u0422\u0435\u0441\u0442";  // Test in Cyrillic characters.
var cyrillicMatch = /^[\u0430-\u044fa-z]+$/i.test(st);  // a-ja a-z.
assertTrue(cyrillicMatch);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/57c919e^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=57c919e)  
[test/mjsunit/cyrillic.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/cyrillic.js?cl=57c919e)  
[test/mjsunit/regress/regress-486.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-486.js?cl=57c919e)  
  

---   

## **regress-496.js (v8 issue)**  
   
**[Issue: Unaliased eval call treated as aliased.](https://crbug.com/v8/496)**  
**[Commit: Fix case where we treat an unaliased call to eval as an aliased call to eval.](https://chromium.googlesource.com/v8/v8/+/f39fbb2)**  
  
Date(Commit): Thu Nov 05 11:19:37 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/366027](http://codereview.chromium.org/366027)  
Regress: [mjsunit/regress/regress-496.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-496.js)  
```javascript
function h() {
  function f() { return eval; }
  function g() { var x = 44; return eval("x"); }
  assertEquals(44, g());
}

h();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f39fbb2^!)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=f39fbb2)  
[test/mjsunit/regress/regress-496.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-496.js?cl=f39fbb2)  
  

---   

## **regress-491.js (v8 issue)**  
   
**[Issue: constantpool dump violates ARM debugger assertion for return point](https://crbug.com/v8/491)**  
**[Commit: Fix issue 491: constantpool dump violates ARM debugger assertion for return point](https://chromium.googlesource.com/v8/v8/+/77a71c9)**  
  
Date(Commit): Wed Nov 04 14:45:50 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/362003](http://codereview.chromium.org/362003)  
Regress: [mjsunit/regress/regress-491.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-491.js)  
```javascript
function function_with_n_strings(n) {
  var source = '(function f(){';
  for (var i = 0; i < n; i++) {
    if (i != 0) source += ';';
    source += '"x"';
  }
  source += '})()';
  eval(source);
}

var i;
for (i = 500; i < 600; i++) {
  function_with_n_strings(i);
}
for (i = 1100; i < 1200; i++) {
  function_with_n_strings(i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77a71c9^!)  
[src/arm/assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.cc?cl=77a71c9)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=77a71c9)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=77a71c9)  
[src/arm/codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.h?cl=77a71c9)  
[src/arm/debug-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/debug-arm.cc?cl=77a71c9)  
...  
  

---   

## **regress-492.js (v8 issue)**  
   
**[Issue: ARM debug crash: mozilla/ecma/FunctionObjects/15.3.1.1-3](https://crbug.com/v8/492)**  
**[Commit: Fix xssue 492: ARM debug crash: mozilla/ecma/FunctionObjects/15.3.1.1-3](https://chromium.googlesource.com/v8/v8/+/54ec6c0)**  
  
Date(Commit): Wed Nov 04 10:04:22 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/354028](http://codereview.chromium.org/354028)  
Regress: [mjsunit/regress/regress-492.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-492.js)  
```javascript
function function_with_n_args(n) {
  var source = '(function f' + n + '(';
  for (var arg = 0; arg < n; arg++) {
    if (arg != 0) source += ',';
    source += 'arg' + arg;
  }
  source += ') { })()';
  eval(source);
}

var args;
for (args = 250; args < 270; args++) {
  function_with_n_args(args);
}

for (args = 500; args < 520; args++) {
  function_with_n_args(args);
}

for (args = 1019; args < 1041; args++) {
  function_with_n_args(args);
}


function foo(
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x
) {}

for (var i = 0; i < 10000; ++i) foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54ec6c0^!)  
[src/arm/assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.cc?cl=54ec6c0)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=54ec6c0)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=54ec6c0)  
[test/mjsunit/regress/regress-492.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-492.js?cl=54ec6c0)  
  

---   

## **regress-490.js (v8 issue)**  
   
**[Issue: Crash in RegExp with change in rr3153](https://crbug.com/v8/490)**  
**[Commit: Don't use string slices when processing RexExp replace (re-apply r3153)](https://chromium.googlesource.com/v8/v8/+/b4c11d0)**  
  
Date(Commit): Mon Nov 02 12:21:43 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/342015.](http://codereview.chromium.org/342015.)  
Regress: [mjsunit/regress/regress-490.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-490.js)  
```javascript
var kXXX = 11
var a = '';
while (a.length < (2 << 11)) { a+= 'x'; }

a.replace(/^(.*)/, '$1$1$1');

for (var i = 0; i < 10; i++) {
  var  b = '';
  for (var j = 0; j < 10; j++) {
    b += '$1';

    // TODO(machenbach): Do we need all these replacements? Wouldn't corner
    // cases like smallest and biggest suffice?
    a.replace(/^(.*)/, b);
  }
  a += a;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4c11d0^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=b4c11d0)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=b4c11d0)  
[test/mjsunit/regress/regress-490.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-490.js?cl=b4c11d0)  
  

---   

## **regress-485.js (v8 issue)**  
   
**[Issue: Function.prototype.call leaks builtins object.](https://crbug.com/v8/485)**  
**[Commit: Issue 485: Fix leak of builtins object through call and apply functions.](https://chromium.googlesource.com/v8/v8/+/0aecc29)**  
  
Date(Commit): Wed Oct 28 13:51:30 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/345007](http://codereview.chromium.org/345007)  
Regress: [mjsunit/regress/regress-485.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-485.js)  
```javascript
var global = this;
var global2 = (function(){return this;})();
assertEquals(global, global2, "direct call to local function returns global");

var builtin2 = Object.prototype.toString;

assertTrue("[object builtins]" != builtin2(), "Direct call to toString");
assertTrue("[object builtins]" != builtin2.call(), "call() to toString");
assertTrue("[object builtins]" != builtin2.apply(), "call() to toString");
assertTrue("[object builtins]" != builtin2.call.call(builtin2),
           "call.call() to toString");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0aecc29^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=0aecc29)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=0aecc29)  
[src/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/builtins-x64.cc?cl=0aecc29)  
[test/mjsunit/regress/regress-485.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-485.js?cl=0aecc29)  
  

---   

## **regress-483.js (v8 issue)**  
   
**[Issue: Failure running some constructors with only this.x = ... assignments](https://crbug.com/v8/483)**  
**[Commit: Fix issue with running some constructors having only this.x = ... assignments.](https://chromium.googlesource.com/v8/v8/+/7a509f2)**  
  
Date(Commit): Fri Oct 23 12:18:47 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/332007](http://codereview.chromium.org/332007)  
Regress: [mjsunit/regress/regress-483.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-483.js)  
```javascript
function X() {
  this.x = this.x.x;
}

X.prototype.x = {x:1}

new X()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a509f2^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=7a509f2)  
[test/mjsunit/regress/regress-483.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-483.js?cl=7a509f2)  
  

---   

## **regress-475.js (v8 issue)**  
   
**[Issue: Crash is IC after revision 3041](https://crbug.com/v8/475)**  
**[Commit: Fix issue 475](https://chromium.googlesource.com/v8/v8/+/a637f45)**  
  
Date(Commit): Tue Oct 20 12:13:31 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/307002](http://codereview.chromium.org/307002)  
Regress: [mjsunit/regress/regress-475.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-475.js)  
```javascript
assertEquals(1, (function (){return 1|-1%1})());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a637f45^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=a637f45)  
[test/mjsunit/regress/regress-475.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-475.js?cl=a637f45)  
  

---   

## **regress-crbug-18639.js (chromium issue)**  
   
**[Issue: Crash [@ 0xffffffff]](https://crbug.com/18639)**  
**[Commit: Add regression test case for http://crbug.com/18639 which](https://chromium.googlesource.com/v8/v8/+/6621a43)**  
  
Date(Commit): Tue Sep 08 07:22:35 2009  
Components/Type: None/Bug-Security  
Labels: ["Area-MISC", "reward-topanel", "Security-High", "Security", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Restrict-AddIssueComment-Commit"]  
Code Review: [http://crbug.com/18639](http://crbug.com/18639)  
Regress: [mjsunit/regress/regress-crbug-18639.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-18639.js)  
```javascript
try {
  toString = toString;
  __defineGetter__("z", (0).toLocaleString);
  z;
  z;
  ((0).toLocaleString)();
} catch (e) {
  assertInstanceof(e, TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6621a43^!)  
[test/mjsunit/regress/regress-246.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-246.js?cl=6621a43)  
[test/mjsunit/regress/regress-254.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-254.js?cl=6621a43)  
[test/mjsunit/regress/regress-crbug-18639.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-18639.js?cl=6621a43)  
  

---   

## **regress-416.js (v8 issue)**  
   
**[Issue: CHECK(!( sizeof (time) == sizeof(float ) ? __inline_isnanf((float)(time)) : sizeof (time) == sizeof(double) ? __inline_isnand((double)(time)) : __inline_isnan ((long double)(time)))) failed](https://crbug.com/v8/416)**  
**[Commit: Add safe handling of NaN to Posix platform-dependent time functions.](https://chromium.googlesource.com/v8/v8/+/3703231)**  
  
Date(Commit): Tue Aug 04 09:41:18 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/160580](http://codereview.chromium.org/160580)  
Regress: [mjsunit/regress/regress-416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-416.js)  
```javascript
assertTrue(isNaN(new Date(1e81).getTime()), "new Date(1e81)");
assertTrue(isNaN(new Date(-1e81).getTime()), "new Date(-1e81)");
assertTrue(isNaN(new Date(1e81, "").getTime()), "new Date(1e81, \"\")");
assertTrue(isNaN(new Date(-1e81, "").getTime()), "new Date(-1e81, \"\")");
assertTrue(isNaN(new Date(Number.NaN).getTime()), "new Date(Number.NaN)");
assertTrue(isNaN(new Date(Number.NaN, "").getTime()),
           "new Date(Number.NaN, \"\")");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3703231^!)  
[src/date-delay.js](https://cs.chromium.org/chromium/src/v8/src/date-delay.js?cl=3703231)  
[src/platform-posix.cc](https://cs.chromium.org/chromium/src/v8/src/platform-posix.cc?cl=3703231)  
[test/mjsunit/regress/regress-416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-416.js?cl=3703231)  
  

---   

## **regress-155924.js (chromium issue)**  
   
**[Issue: Provide an easier way to migrate wifi credentials](https://crbug.com/155924)**  
**[Commit: Fix an error in a keyed lookup stub - HeapNumbers treated as strings.](https://chromium.googlesource.com/v8/v8/+/18c6337)**  
  
Date(Commit): Thu Jul 23 13:01:17 2009  
Components/Type: Enterprise/Feature  
Labels: ["Hotlist-Recharge", "MovedFrom-25", "Hotlist-Enterprise", "MovedFrom-26"]  
Code Review: [http://codereview.chromium.org/155924](http://codereview.chromium.org/155924)  
Regress: [mjsunit/regress/regress-155924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-155924.js)  
```javascript
A = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ];

function foo() {
  x = 1 << 26;
  x = x * x;
  // The following floating-point heap number has a second word similar
  // to that of the string "5":
  // 2^52 + index << cached_index_shift + cached_index_tag
  x = x + (5 << 2) + (1 << 1);
  return A[x];
}

assertEquals(undefined, foo(), "First lookup A[bad_float]");
assertEquals(undefined, foo(), "Second lookup A[bad_float]");
assertEquals(undefined, foo(), "Third lookup A[bad_float]");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18c6337^!)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=18c6337)  
[test/mjsunit/regress/regress-155924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-155924.js?cl=18c6337)  
  

---   

## **regress-406.js (v8 issue)**  
   
**[Issue: (ARM) Constant folding crashes short-circuit boolean expressions](https://crbug.com/v8/406)**  
**[Commit: Fix ARM compiler crash in short-circuited boolean expressions.](https://chromium.googlesource.com/v8/v8/+/1ca19c3)**  
  
Date(Commit): Thu Jul 23 11:40:14 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/160006](http://codereview.chromium.org/160006)  
Regress: [mjsunit/regress/regress-406.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-406.js)  
```javascript
assertFalse(typeof(0) == "zero");
assertTrue(typeof(0) != "zero");

assertFalse(typeof(0) == "zero" && typeof(0) == "zero");
assertFalse(typeof(0) == "zero" && typeof(0) != "zero");
assertFalse(typeof(0) != "zero" && typeof(0) == "zero");
assertTrue(typeof(0) != "zero" && typeof(0) != "zero");

assertFalse(typeof(0) == "zero" || typeof(0) == "zero");
assertTrue(typeof(0) == "zero" || typeof(0) != "zero");
assertTrue(typeof(0) != "zero" || typeof(0) == "zero");
assertTrue(typeof(0) != "zero" || typeof(0) != "zero");

function one() { return 1; }

assertFalse(typeof(0) == "zero" && one() < 0);
assertFalse(typeof(0) == "zero" && one() > 0);
assertFalse(typeof(0) != "zero" && one() < 0);
assertTrue(typeof(0) != "zero" && one() > 0);

assertFalse(typeof(0) == "zero" || one() < 0);
assertTrue(typeof(0) == "zero" || one() > 0);
assertTrue(typeof(0) != "zero" || one() < 0);
assertTrue(typeof(0) != "zero" || one() > 0);

assertFalse(one() < 0 && typeof(0) == "zero");
assertFalse(one() < 0 && typeof(0) != "zero");
assertFalse(one() > 0 && typeof(0) == "zero");
assertTrue(one() > 0 && typeof(0) != "zero");

assertFalse(one() < 0 || typeof(0) == "zero");
assertTrue(one() < 0 || typeof(0) != "zero");
assertTrue(one() > 0 || typeof(0) == "zero");
assertTrue(one() > 0 || typeof(0) != "zero");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ca19c3^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=1ca19c3)  
[test/mjsunit/regress/regress-406.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-406.js?cl=1ca19c3)  
  

---   

## **regress-345.js (v8 issue)**  
   
**[Issue: CHECK(count >= 0) failed](https://crbug.com/v8/345)**  
**[Commit: Fix issue 345 by avoiding duplicates in the list of escaping labels](https://chromium.googlesource.com/v8/v8/+/6443cb9)**  
  
Date(Commit): Wed Jul 15 08:57:25 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/149670](http://codereview.chromium.org/149670)  
Regress: [mjsunit/regress/regress-345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-345.js)  
```javascript
do {
  try {
    continue;
  } catch (e) {
    continue;
  } finally {
  }
} while (false);


L: {
  try {
    break L;
  } catch (e) {
    break L;
  } finally {
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6443cb9^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=6443cb9)  
[test/mjsunit/regress/regress-345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-345.js?cl=6443cb9)  
  

---   

## **regress-399.js (v8 issue)**  
   
**[Issue: Wrong year calculated for Antipodean time zones near New Year's Day.](https://crbug.com/v8/399)**  
**[Commit: Fix issue 397 and issue 399. Review URL: http://codereview.chromium.org/149247](https://chromium.googlesource.com/v8/v8/+/b0f411c)**  
  
Date(Commit): Tue Jul 07 11:57:09 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/149247](http://codereview.chromium.org/149247)  
Regress: [mjsunit/regress/regress-399.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-399.js)  
```javascript
var date = new Date(1.009804e12);
var year = Number(String(date).match(/.*(200\d)/)[1]);
assertEquals(year, date.getFullYear());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0f411c^!)  
[src/date-delay.js](https://cs.chromium.org/chromium/src/v8/src/date-delay.js?cl=b0f411c)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=b0f411c)  
[test/mjsunit/regress/regress-397.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-397.js?cl=b0f411c)  
[test/mjsunit/regress/regress-399.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-399.js?cl=b0f411c)  
  

---   

## **regress-397.js (v8 issue)**  
   
**[Issue: Math.pow(-Infinity, 0.5) gives NaN, not Infinity.](https://crbug.com/v8/397)**  
**[Commit: Fix issue 397 and issue 399. Review URL: http://codereview.chromium.org/149247](https://chromium.googlesource.com/v8/v8/+/b0f411c)**  
  
Date(Commit): Tue Jul 07 11:57:09 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/149247](http://codereview.chromium.org/149247)  
Regress: [mjsunit/regress/regress-397.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-397.js)  
```javascript
function test() {
  assertEquals("Infinity", String(Math.pow(Infinity, 0.5)));
  assertEquals(0, Math.pow(Infinity, -0.5));

  assertEquals("Infinity", String(Math.pow(-Infinity, 0.5)));
  assertEquals(0, Math.pow(-Infinity, -0.5));
}

test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0f411c^!)  
[src/date-delay.js](https://cs.chromium.org/chromium/src/v8/src/date-delay.js?cl=b0f411c)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=b0f411c)  
[test/mjsunit/regress/regress-397.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-397.js?cl=b0f411c)  
[test/mjsunit/regress/regress-399.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-399.js?cl=b0f411c)  
  

---   

## **regress-396.js (v8 issue)**  
   
**[Issue: REGRESSION: Illegal access exception](https://crbug.com/v8/396)**  
**[Commit: Add regression test case for issue 396.](https://chromium.googlesource.com/v8/v8/+/f0053e8)**  
  
Date(Commit): Thu Jul 02 09:08:15 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/150215](http://codereview.chromium.org/150215)  
Regress: [mjsunit/regress/regress-396.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-396.js)  
```javascript
function DateYear(date) {
  var string = date.getYear() + '';
  if (string.length < 4) {
    string = '' + (string - 0 + 1900);
  }
  return string;
}

assertEquals('1995', DateYear(new Date('Dec 25, 1995')));
assertEquals('2005', DateYear(new Date('Dec 25, 2005')));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0053e8^!)  
[test/mjsunit/regress/regress-396.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-396.js?cl=f0053e8)  
  

---   

## **regress-394.js (v8 issue)**  
   
**[Issue: Assessors on global object not handled correctly](https://crbug.com/v8/394)**  
**[Commit: Handle JavaScript accessors on the global object.](https://chromium.googlesource.com/v8/v8/+/25405dd)**  
  
Date(Commit): Wed Jul 01 11:20:33 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/150162](http://codereview.chromium.org/150162)  
Regress: [mjsunit/regress/regress-394.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-394.js)  
```javascript
function setx(){
  x=1;
}

function getx(){
  return x;
}

setx()
setx()
this.__defineSetter__('x',function(){});
this.__defineGetter__('x',function(){return 2;});
setx()
assertEquals(2, x);

assertEquals(2, getx());
assertEquals(2, getx());
assertEquals(2, getx());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/25405dd^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=25405dd)  
[test/mjsunit/regress/regress-394.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-394.js?cl=25405dd)  
  

---   

## **regress-392.js (v8 issue)**  
   
**[Issue: Crash [@ v8::internal::JSObject::LocalLookup] and CHECK(object->IsString() || object->IsNumber() || object->IsBoolean()) failed](https://crbug.com/v8/392)**  
**[Commit: Fix issue 392 by disabling the TakeValue optimization for](https://chromium.googlesource.com/v8/v8/+/3ae01ab)**  
  
Date(Commit): Mon Jun 29 06:20:52 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/150016](http://codereview.chromium.org/150016)  
Regress: [mjsunit/regress/regress-392.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-392.js)  
```javascript
assertTrue(isNaN((function(){return arguments++})()));
assertTrue(isNaN((function(){return ++arguments})()));
assertTrue(isNaN((function(){return arguments--})()));
assertTrue(isNaN((function(){return --arguments})()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ae01ab^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=3ae01ab)  
[test/mjsunit/regress/regress-392.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-392.js?cl=3ae01ab)  
  

---   

## **regress-1919169.js (other issue)**  
   
**[Commit: Fix bug in static type inference for loops.](https://chromium.googlesource.com/v8/v8/+/2dd9717)**  
  
Date(Commit): Mon Jun 22 12:36:01 2009  
Code Review: [http://codereview.chromium.org/140058](http://codereview.chromium.org/140058)  
Regress: [mjsunit/regress/regress-1919169.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1919169.js)  
```javascript
function test() {
 var s2 = "s2";
 for (var i = 0; i < 2; i++) {
   // Crashes in round i==1 with IllegalAccess in %StringAdd(x,y)
   var res = 1 + s2;
   s2 = 2;
 }
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2dd9717^!)  
[src/ia32/virtual-frame-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/virtual-frame-ia32.cc?cl=2dd9717)  
[test/mjsunit/regress/regress-1919169.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1919169.js?cl=2dd9717)  
  
  
---   

## **regress-386.js (v8 issue)**  
   
**[Issue: Assertion failed: t != INTERCEPTOR](https://crbug.com/v8/386)**  
**[Commit: Fix issue 386, a bug in JSObject::ReplaceSlowProperty with constant transitions.](https://chromium.googlesource.com/v8/v8/+/74ddab9)**  
  
Date(Commit): Mon Jun 22 07:41:15 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/141031](http://codereview.chromium.org/141031)  
Regress: [mjsunit/regress/regress-386.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-386.js)  
```javascript
function A() {
 for (var i = 0; i < 13; i++) {
   this['a' + i] = i;
 }
 this.i = function(){};
};

new A();
new A();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/74ddab9^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=74ddab9)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=74ddab9)  
[test/mjsunit/regress/regress-386.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-386.js?cl=74ddab9)  
  

---   

## **regress-6-9-regexp.js (other issue)**  
   
**[Commit: Fix regexp bug reported by Ian where [6-9] would match any digit.](https://chromium.googlesource.com/v8/v8/+/e2a01ed)**  
  
Date(Commit): Sat Jun 20 17:57:09 2009  
Code Review: [http://codereview.chromium.org/140021](http://codereview.chromium.org/140021)  
Regress: [mjsunit/regress/regress-6-9-regexp.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6-9-regexp.js)  
```javascript
assertFalse(/[6-9]/.test('2'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2a01ed^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=e2a01ed)  
[test/mjsunit/regress/regress-6-9-regexp.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6-9-regexp.js?cl=e2a01ed)  
  
  
---   

## **regress-351.js (v8 issue)**  
   
**[Issue: String#lastIndexOf should return 0 for negative position values.](https://crbug.com/v8/351)**  
**[Commit: Fix for issue 351 - lastIndexOf.](https://chromium.googlesource.com/v8/v8/+/9452453)**  
  
Date(Commit): Tue May 26 15:42:06 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/113838](http://codereview.chromium.org/113838)  
Regress: [mjsunit/regress/regress-351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-351.js)  
```javascript
assertEquals(0, "test".lastIndexOf("test", -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9452453^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=9452453)  
[test/mjsunit/regress/regress-351.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-351.js?cl=9452453)  
[test/mjsunit/string-lastindexof.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/string-lastindexof.js?cl=9452453)  
  

---   

## **regress-349.js (v8 issue)**  
   
**[Issue: String search hits assert in debug mode](https://crbug.com/v8/349)**  
**[Commit: Fix for issue 349: Make initial boundary check for BM text search.](https://chromium.googlesource.com/v8/v8/+/2ff3901)**  
  
Date(Commit): Tue May 19 09:01:03 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/113575](http://codereview.chromium.org/113575)  
Regress: [mjsunit/regress/regress-349.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-349.js)  
```javascript
var str = "bbaabbbbbbbbabbaaaabbaaabbbaaaabbaaabbabaaabb";
assertEquals(str, str.replace(/aabab/g, "foo"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ff3901^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=2ff3901)  
[test/mjsunit/regress/regress-349.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-349.js?cl=2ff3901)  
  

---   

## **regress-341.js (v8 issue)**  
   
**[Issue: CRASH when running LayoutTests/fast/js/instance-of-immediates.html](https://crbug.com/v8/341)**  
**[Commit: Fix for issue 341.  In the stub for instanceof, we could try to read](https://chromium.googlesource.com/v8/v8/+/18f69a7)**  
  
Date(Commit): Tue May 12 11:40:14 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/115236](http://codereview.chromium.org/115236)  
Regress: [mjsunit/regress/regress-341.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-341.js)  
```javascript
function F() {}

F.prototype = 1;
var o = {};

assertThrows("o instanceof F");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18f69a7^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=18f69a7)  
[test/mjsunit/regress/regress-341.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-341.js?cl=18f69a7)  
  

---   

## **regress-334.js (v8 issue)**  
   
**[Issue: Setting a property resets attributes.](https://crbug.com/v8/334)**  
**[Commit: Added test for issue 334.](https://chromium.googlesource.com/v8/v8/+/b11b61c)**  
  
Date(Commit): Tue May 05 11:52:37 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/109009](http://codereview.chromium.org/109009)  
Regress: [mjsunit/regress/regress-334.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-334.js)  
```javascript
var READ_ONLY   = 1;
var DONT_ENUM   = 2;
var DONT_DELETE = 4;

function AddNamedProperty(object, name, value, attrs) {
  Object.defineProperty(object, name, {
      value,
      configurable: (attrs & DONT_DELETE) === 0,
      enumerable: (attrs & DONT_ENUM) === 0,
      writable: (attrs & READ_ONLY) === 0
  });
}

function func1(){}
function func2(){}

var object = {__proto__:{}};
AddNamedProperty(object, "foo", func1, DONT_ENUM | DONT_DELETE);
AddNamedProperty(object, "bar", func1, DONT_ENUM | READ_ONLY);
AddNamedProperty(object, "baz", func1, DONT_DELETE | READ_ONLY);
AddNamedProperty(object.__proto__, "bif", func1, DONT_ENUM | DONT_DELETE);
object.bif = func2;

function enumerable(obj) {
  var res = [];
  for (var i in obj) {
    res.push(i);
  }
  res.sort();
  return res;
}

assertArrayEquals(["baz", "bif"], enumerable(object), "enum0");
assertFalse(delete object.foo, "delete foo");
assertFalse(delete object.baz, "delete baz");
assertEquals(func1, object.foo, "read foo");
assertEquals(func1, object.bar, "read bar");
assertEquals(func1, object.baz, "read baz");
assertEquals(func2, object.bif, "read bif");

object.bar = "NO WAY";
assertEquals(func1, object.bar, "read bar 2");
assertArrayEquals(["baz", "bif"], enumerable(object), "enum1");

object.foo = func2;
assertArrayEquals(["baz", "bif"], enumerable(object), "enum2");
assertFalse(delete object.foo, "delete foo 2");

assertTrue(delete object.bar, "delete bar");
assertFalse("bar" in object, "has bar");
object.bar = func2;
assertTrue("bar" in object, "has bar 2");
assertEquals(func2, object.bar, "read bar 3");

assertArrayEquals(["bar", "baz", "bif"], enumerable(object), "enum3");

assertTrue(delete object.bif, "delete bif");
assertArrayEquals(["bar", "baz"], enumerable(object), "enum4");
assertEquals(func1, object.bif, "read bif 2");
assertTrue(delete object.bif, "delete bif 2");
assertArrayEquals(["bar", "baz"], enumerable(object), "enum5");
assertEquals(func1, object.bif, "read bif3");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b11b61c^!)  
[test/mjsunit/bugs/bug-334.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-334.js?cl=b11b61c)  
  

---   

## **regress-326.js (v8 issue)**  
   
**[Issue: Array sort on non-array objects doesn't preserve missing elements](https://crbug.com/v8/326)**  
**[Commit: Fix Issue 326. Handle sorting of non-array objects correctly.](https://chromium.googlesource.com/v8/v8/+/889eac7)**  
  
Date(Commit): Mon Apr 27 11:16:59 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/92123](http://codereview.chromium.org/92123)  
Regress: [mjsunit/regress/regress-326.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-326.js)  
```javascript
var nonArray = { length: 4, 0: 42, 2: 37, 3: undefined, 4: 0 };
Array.prototype.sort.call(nonArray);

assertEquals(4, nonArray.length, "preserve length");
assertEquals(37, nonArray[0], "sort smallest first");
assertEquals(42, nonArray[1], "sort largest last");
assertTrue(2 in nonArray, "don't delete undefined");
assertEquals(undefined, nonArray[2], "sort undefined after largest");
assertFalse(3 in nonArray, "don't create non-existing");
assertEquals(0, nonArray[4], "don't affect after length.");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/889eac7^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=889eac7)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=889eac7)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=889eac7)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=889eac7)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=889eac7)  
...  
  

---   

## **regress-318.js (v8 issue)**  
   
**[Issue: Inferred static type of elements is not safely set when merge code is not generated](https://crbug.com/v8/318)**  
**[Commit: When merging a frame to an expected on at block entry, the static type](https://chromium.googlesource.com/v8/v8/+/b39f438)**  
  
Date(Commit): Wed Apr 22 13:19:38 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/92009](http://codereview.chromium.org/92009)  
Regress: [mjsunit/regress/regress-318.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-318.js)  
```javascript
function test(value) {
  if (typeof(value) == 'boolean') value = value + '';
  if (typeof(value) == 'number') value = value + '';
}

assertDoesNotThrow('test(0)');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b39f438^!)  
[src/virtual-frame-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/virtual-frame-ia32.cc?cl=b39f438)  
[src/virtual-frame.cc](https://cs.chromium.org/chromium/src/v8/src/virtual-frame.cc?cl=b39f438)  
[test/mjsunit/regress/regress-318.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-318.js?cl=b39f438)  
  

---   

## **regress-317.js (v8 issue)**  
   
**[Issue: String replace of string doesn't allow dollars in replace string.](https://crbug.com/v8/317)**  
**[Commit: Fix for Issue 317 - bug in string.replace(string, "$foo").](https://chromium.googlesource.com/v8/v8/+/bfb33b1)**  
  
Date(Commit): Wed Apr 22 11:43:05 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/94002](http://codereview.chromium.org/94002)  
Regress: [mjsunit/regress/regress-317.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-317.js)  
```javascript
assertEquals("a$ec", "abc".replace("b", "$e"), "$e isn't meaningful");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bfb33b1^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=bfb33b1)  
[test/mjsunit/regress/regress-317.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-317.js?cl=bfb33b1)  
[test/mjsunit/string-replace.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/string-replace.js?cl=bfb33b1)  
  

---   

## **regress-312.js (v8 issue)**  
   
**[Issue: Function name collector triggers ASSERT when expressions contain multiple anonymous functions.](https://crbug.com/v8/312)**  
**[Commit: Change the function name collector to tolerate expressions that contain](https://chromium.googlesource.com/v8/v8/+/22896c8)**  
  
Date(Commit): Wed Apr 15 13:14:23 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/67165](http://codereview.chromium.org/67165)  
Regress: [mjsunit/regress/regress-312.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-312.js)  
```javascript
var o = { f: "x" ? function () {} : function () {} };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22896c8^!)  
[src/func-name-inferrer.h](https://cs.chromium.org/chromium/src/v8/src/func-name-inferrer.h?cl=22896c8)  
[test/mjsunit/regress/regress-312.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-312.js?cl=22896c8)  
  

---   

## **regress-294.js (v8 issue)**  
   
**[Issue: Failure to preserve is_copied flag on virtual frame elements](https://crbug.com/v8/294)**  
**[Commit: Fix issue 294 by ensuring that we don't lose the copy flag on memory](https://chromium.googlesource.com/v8/v8/+/c80b013)**  
  
Date(Commit): Tue Mar 31 14:01:25 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/57053](http://codereview.chromium.org/57053)  
Regress: [mjsunit/regress/regress-294.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-294.js)  
```javascript
function f() { return false; }

function test(x) {
  var y = x;
  if (x == "kat") x = "kat";
  else {
    x = "hund";
    var z = f();
    if (!z) x = "kat";
  }
}

test("hund");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c80b013^!)  
[src/virtual-frame-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/virtual-frame-ia32.cc?cl=c80b013)  
[test/mjsunit/regress/regress-294.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-294.js?cl=c80b013)  
  

---   

## **regress-286.js (v8 issue)**  
   
**[Issue: Compound assignments to variables in registers do not work](https://crbug.com/v8/286)**  
**[Commit: Fix issue 286.  Ensure frame elements are invalidated by InvalidateFrameSlotAt.](https://chromium.googlesource.com/v8/v8/+/1ba34bf)**  
  
Date(Commit): Tue Mar 24 12:42:28 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/53008](http://codereview.chromium.org/53008)  
Regress: [mjsunit/regress/regress-286.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-286.js)  
```javascript
function test() {
  var o = [1];
  var a = o[o ^= 1];
  return a;
};

assertEquals(1, test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ba34bf^!)  
[src/virtual-frame-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/virtual-frame-ia32.cc?cl=1ba34bf)  
[test/mjsunit/regress/regress-286.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-286.js?cl=1ba34bf)  
  

---   

## **regress-284.js (v8 issue)**  
   
**[Issue: Assigning and continuing within a for-in loop behaves unexpectedly](https://crbug.com/v8/284)**  
**[Commit: Added test case for issue 284.](https://chromium.googlesource.com/v8/v8/+/cff1d27)**  
  
Date(Commit): Mon Mar 23 23:49:58 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/52031](http://codereview.chromium.org/52031)  
Regress: [mjsunit/regress/regress-284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-284.js)  
```javascript
function continueWithinLoop() {
  var result;
  for (var key in [0]) {
    result = "hopla";
    continue;
  }
  return result;
};

assertEquals("hopla", continueWithinLoop());

function breakWithinLoop() {
  var result;
  for (var key in [0]) {
    result = "hopla";
    break;
  }
  return result;
};

assertEquals("hopla", continueWithinLoop());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cff1d27^!)  
[test/mjsunit/bugs/bug-284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-284.js?cl=cff1d27)  
  

---   

## **regress-279.js (v8 issue)**  
   
**[Issue: new array literals inside object literals returned from body of a function are recycled](https://crbug.com/v8/279)**  
**[Commit: Revert 1432, 1433, 1469 and 1472 due to a bug with literal objects.](https://chromium.googlesource.com/v8/v8/+/3aa57f7)**  
  
Date(Commit): Sun Mar 15 16:18:20 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/46088](http://codereview.chromium.org/46088)  
Regress: [mjsunit/regress/regress-279.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-279.js)  
```javascript
function makeArrayInObject() {
  return { foo: [] };
}

var a = makeArrayInObject();
a.foo.push(5);
var b = makeArrayInObject();
assertEquals(0, b.foo.length, "Array in object");

function makeObjectInObject() {
  return { foo: {} };
}

a = makeObjectInObject();
a.foo.bar = 1;
b = makeObjectInObject();
assertEquals('undefined', typeof(b.foo.bar), "Object in object");

function makeObjectInArray() {
  return [ {} ];
}

a = makeObjectInArray();
a[0].bar = 1;
b = makeObjectInArray();
assertEquals('undefined', typeof(b[0].bar), "Object in array");

function makeArrayInArray() {
  return [ [] ];
}

a = makeArrayInArray();
a[0].push(5);
b = makeArrayInArray();
assertEquals(0, b[0].length, "Array in array");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3aa57f7^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=3aa57f7)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=3aa57f7)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=3aa57f7)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=3aa57f7)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=3aa57f7)  
...  
  

---   

## **regress-267.js (v8 issue)**  
   
**[Issue: Indirect eval fails in eval-tainted scope.](https://crbug.com/v8/267)**  
**[Commit: Issue 267: Calls to arguments in eval-tainted function scope uses global object as receiver.](https://chromium.googlesource.com/v8/v8/+/34db0ff)**  
  
Date(Commit): Tue Mar 10 12:28:34 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1476](http://v8.googlecode.com/svn/branches/bleeding_edge@1476)  
Regress: [mjsunit/regress/regress-267.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-267.js)  
```javascript
var global = (function(){ return this; })();
function taint(fn){var v = fn(); eval("taint"); return v; }
function getThis(){ return this; }
var obj = taint(getThis);

assertEquals(global, obj, "Should be the global object.");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34db0ff^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=34db0ff)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=34db0ff)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=34db0ff)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=34db0ff)  
[test/mjsunit/regress/regress-267.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-267.js?cl=34db0ff)  
  

---   

## **regress-244.js (v8 issue)**  
   
**[Issue: Uri encode/decode don't handle malformed sequences correctly](https://crbug.com/v8/244)**  
**[Commit: Implemented invalid UTF8 detection in decodeURI.  That is, detection](https://chromium.googlesource.com/v8/v8/+/782b537)**  
  
Date(Commit): Tue Mar 10 09:08:05 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1471](http://v8.googlecode.com/svn/branches/bleeding_edge@1471)  
Regress: [mjsunit/regress/regress-244.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-244.js)  
```javascript
var kLegalPairs = [
  [0x00, '%00'],
  [0x01, '%01'],
  [0x7f, '%7F'],
  [0x80, '%C2%80'],
  [0x81, '%C2%81'],
  [0x7ff, '%DF%BF'],
  [0x800, '%E0%A0%80'],
  [0x801, '%E0%A0%81'],
  [0xd7ff, '%ED%9F%BF'],
  [0xffff, '%EF%BF%BF']
];

var kIllegalEncoded = [
  '%80', '%BF', '%80%BF', '%80%BF%80', '%C0%22', '%DF',
  '%EF%BF', '%F7BFBF', '%FE', '%FF', '%FE%FE%FF%FF',
  '%C0%AF', '%E0%9F%BF', '%F0%8F%BF%BF', '%C0%80',
  '%E0%80%80'
];

function run() {
  for (var i = 0; i < kLegalPairs.length; i++) {
    var decoded = String.fromCharCode(kLegalPairs[i][0]);
    var encoded = kLegalPairs[i][1];
    assertEquals(decodeURI(encoded), decoded);
    assertEquals(encodeURI(decoded), encoded);
  }
  for (var i = 0; i < kIllegalEncoded.length; i++) {
    var value = kIllegalEncoded[i];
    var exception = false;
    try {
      decodeURI(value);
    } catch (e) {
      exception = true;
      assertInstanceof(e, URIError);
    }
    assertTrue(exception);
  }
}

run();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/782b537^!)  
[src/regexp-delay.js](https://cs.chromium.org/chromium/src/v8/src/regexp-delay.js?cl=782b537)  
[src/uri.js](https://cs.chromium.org/chromium/src/v8/src/uri.js?cl=782b537)  
[test/mjsunit/regress/regress-244.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-244.js?cl=782b537)  
  

---   

## **regress-265.js (v8 issue)**  
   
**[Issue: Assertion failure: CHECK(0 <= i && i < length_) failed](https://crbug.com/v8/265)**  
**[Commit: Add a failing test case for issue 265:](https://chromium.googlesource.com/v8/v8/+/8eea2af)**  
  
Date(Commit): Mon Mar 09 17:21:28 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/39349](http://codereview.chromium.org/39349)  
Regress: [mjsunit/regress/regress-265.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-265.js)  
```javascript
function test0() {
  try {
    try {
      return 0;
    } finally {
      try {
        return 0;
      } finally {
      }
    }
  } finally {
  }
}

test0();

function test1() {
L0:
  try {
    try {
      break L0;
    } finally {
      try {
        break L0;
      } finally {
      }
    }
  } finally {
  }
}

test1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8eea2af^!)  
[test/mjsunit/bugs/bug-265.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-265.js?cl=8eea2af)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=8eea2af)  
  

---   

## **regress-1493017.js (other issue)**  
   
**[Commit: Fix garbage collection of unused maps.  Null descriptors, created](https://chromium.googlesource.com/v8/v8/+/7977c6c6)**  
  
Date(Commit): Mon Mar 09 16:24:46 2009  
Code Review: [http://codereview.chromium.org/40218](http://codereview.chromium.org/40218)  
Regress: [mjsunit/regress/regress-1493017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1493017.js)  
```javascript
function C() {}


var o = new C();
o.x = 42;
o = null;

gc();

o = new C();

for (var p in o) {
  assertTrue(false);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7977c6c6^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=7977c6c6)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=7977c6c6)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=7977c6c6)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7977c6c6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=7977c6c6)  
...  
  
  
---   

## **regress-260.js (v8 issue)**  
   
**[Issue: Assertion failure: CHECK(cgen_ == __null) failed](https://crbug.com/v8/260)**  
**[Commit: Work around issue 260 for now by disabling duplication of the loop](https://chromium.googlesource.com/v8/v8/+/34af9f2)**  
  
Date(Commit): Mon Mar 09 14:12:20 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/40294](http://codereview.chromium.org/40294)  
Regress: [mjsunit/regress/regress-260.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-260.js)  
```javascript
function test() { eval("while(!function () { var x; });"); }
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34af9f2^!)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=34af9f2)  
[test/mjsunit/regress/regress-260.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-260.js?cl=34af9f2)  
  

---   

## **regress-263.js (v8 issue)**  
   
**[Issue: Properly adjust stack height when returning out of for/in and try/finally](https://crbug.com/v8/263)**  
**[Commit: Fix issue 263:](https://chromium.googlesource.com/v8/v8/+/ece2c03)**  
  
Date(Commit): Mon Mar 09 10:51:57 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/39331](http://codereview.chromium.org/39331)  
Regress: [mjsunit/regress/regress-263.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-263.js)  
```javascript
function test0() { with({}) for(var x in {}) return; }
test0();


function test1() { with({}) try { } finally { with({}) return; } }
test1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ece2c03^!)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=ece2c03)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=ece2c03)  
[test/mjsunit/regress/regress-263.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-263.js?cl=ece2c03)  
  

---   

## **regress-259.js (v8 issue)**  
   
**[Issue: Assertion failure: CHECK(0 <= i && i < length_) failed](https://crbug.com/v8/259)**  
**[Commit: Fix issue 259.](https://chromium.googlesource.com/v8/v8/+/b638d5c)**  
  
Date(Commit): Fri Mar 06 10:18:33 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/39251](http://codereview.chromium.org/39251)  
Regress: [mjsunit/regress/regress-259.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-259.js)  
```javascript
assertThrows("try { while (true) { throw 0; }} finally {}");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b638d5c^!)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=b638d5c)  
[test/mjsunit/regress/regress-259.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-259.js?cl=b638d5c)  
  

---   

## **regress-254.js (v8 issue)**  
   
**[Issue: RegExp.prototype.test doesn't update lastIndex on a global regexp correctly.](https://crbug.com/v8/254)**  
**[Commit: Issue 254 - now correctly updates lastIndexof when using the test method.](https://chromium.googlesource.com/v8/v8/+/21fb24e)**  
  
Date(Commit): Wed Mar 04 12:29:37 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1418](http://v8.googlecode.com/svn/branches/bleeding_edge@1418)  
Regress: [mjsunit/regress/regress-254.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-254.js)  
```javascript
var re = /x/g;

assertEquals(0, re.lastIndex, "Global, initial lastIndex");

assertTrue(re.test("x"), "Global, test 1");
assertEquals(1, re.lastIndex, "Global, lastIndex after test 1");
assertFalse(re.test("x"), "Global, test 2");
assertEquals(0, re.lastIndex, "Global, lastIndex after test 2");

assertEquals(["x"], re.exec("x"), "Global, exec 1");
assertEquals(1, re.lastIndex, "Global, lastIndex after exec 1");
assertEquals(null, re.exec("x"), "Global, exec 2");
assertEquals(0, re.lastIndex, "Global, lastIndex after exec 2");

var re2 = /x/;

assertEquals(0, re2.lastIndex, "Non-global, initial lastIndex");

assertTrue(re2.test("x"), "Non-global, test 1");
assertEquals(0, re2.lastIndex, "Non-global, lastIndex after test 1");
assertTrue(re2.test("x"), "Non-global, test 2");
assertEquals(0, re2.lastIndex, "Non-global, lastIndex after test 2");

assertEquals(["x"], re2.exec("x"), "Non-global, exec 1");
assertEquals(0, re2.lastIndex, "Non-global, lastIndex after exec 1");
assertEquals(["x"], re2.exec("x"), "Non-global, exec 2");
assertEquals(0, re2.lastIndex, "Non-global, lastIndex after exec 2");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21fb24e^!)  
[src/regexp-delay.js](https://cs.chromium.org/chromium/src/v8/src/regexp-delay.js?cl=21fb24e)  
[test/mjsunit/regress/regress-254.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-254.js?cl=21fb24e)  
  

---   

## **regress-253.js (v8 issue)**  
   
**[Issue: Illegal access when toring multiple elements on heap numbers.](https://crbug.com/v8/253)**  
**[Commit: Fixed issue 253. No longer assuming that the target of a property lookup is a JSObject.](https://chromium.googlesource.com/v8/v8/+/7bd50d0)**  
  
Date(Commit): Wed Mar 04 11:57:24 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/39126](http://codereview.chromium.org/39126)  
Regress: [mjsunit/regress/regress-253.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-253.js)  
```javascript
var x = 0;
x[0] = 0;
x[0] = 1;
x[0] = 2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bd50d0^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=7bd50d0)  
[test/mjsunit/regress/regress-253.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-253.js?cl=7bd50d0)  
  

---   

## **regress-246.js (v8 issue)**  
   
**[Issue: Matching deeply parenthesized regexps fails](https://crbug.com/v8/246)**  
**[Commit: Issue 246 - wait until regexp is parsed to detect whether it's simple.](https://chromium.googlesource.com/v8/v8/+/4852bef)**  
  
Date(Commit): Wed Mar 04 09:52:01 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1412](http://v8.googlecode.com/svn/branches/bleeding_edge@1412)  
Regress: [mjsunit/regress/regress-246.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-246.js)  
```javascript
assertTrue(/(?:text)/.test("text"));
assertEquals(["text"], /(?:text)/.exec("text"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4852bef^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=4852bef)  
[test/mjsunit/regress/regress-246.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-246.js?cl=4852bef)  
  

---   

## **regress-233.js (v8 issue)**  
   
**[Issue: Unhandled empty handle in glboal regexp.](https://crbug.com/v8/233)**  
**[Commit: Missing handle check. Triggers bug if the runtime stack overflows and it is detected by a global regexp.](https://chromium.googlesource.com/v8/v8/+/80bb2cc)**  
  
Date(Commit): Fri Feb 13 09:40:15 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1263](http://v8.googlecode.com/svn/branches/bleeding_edge@1263)  
Regress: [mjsunit/regress/regress-233.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-233.js)  
```javascript
function loop(s) {
  loop(s.replace(/\s/g, ""));
}
try {
  loop("No");
} catch(e) {
  // Stack overflow caught.
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/80bb2cc^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=80bb2cc)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=80bb2cc)  
[test/mjsunit/regress/regress-233.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-233.js?cl=80bb2cc)  
  

---   

## **regress-231.js (v8 issue)**  
   
**[Issue: RegExp look-ahead restores bad stack-pointer after stack growth.](https://crbug.com/v8/231)**  
**[Commit: Issue 231 - Irregexp backtracking stack pointer could become corrupted.](https://chromium.googlesource.com/v8/v8/+/0b1f3f2)**  
  
Date(Commit): Thu Feb 12 13:07:58 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1257](http://v8.googlecode.com/svn/branches/bleeding_edge@1257)  
Regress: [mjsunit/regress/regress-231.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-231.js)  
```javascript
var re = /Ggcy\b[^D]*D((?:(?=([^G]+))\2|G(?!gcy\b[^D]*D))*?)GIgcyD/;

var str = 'GgcyDGgcy.saaaa.aDGaaa.aynaaaaaaaaacaaaaagcaaaaaaaancaDGgnayr' +
    '.aryycnaaaataaaa.aryyacnaaataaaa.aaaaraaaaa.aaagaaaaaaaaDGgaaaaDGga' +
    '.aaagaaaaaaaaDGga.nyataaaaragraa.anyataaagaca.agayraaarataga.aaacaa' +
    '.aaagaa.aaacaaaDGaaa.aynaaaaaaaaacaaaaagcaaaaaacaagaa.agayraaaGgaaa' +
    '.trgaaaaaagaatGanyara.caagaaGaD.araaaa_aat_aayDDaaDGaaa.aynaaaaaaaa' +
    'acaaaaagcaaaaaacaaaaa.agayraaaGgaaa.trgaaaaaaatGanyaraDDaaDGacna.ay' +
    'naaaaaaaaacaaaaagcaaaaaacaaaraGgaaa.naaaaagaaaaaaraynaaGanyaraDDaaD' +
    'aGgaaa.saaangaaaaraaaGgaaa.trgaaaragaaaarGanyaraDDDaGIacnaDGIaaaDGI' +
    'aaaDGIgaDGga.anyataaagaca.agayraaaaagaa.aaaaa.cnaaaata.aca.aca.aca.' +
    'acaaaDGgnayr.aaaaraaaaa.aaagaaaaaaaaDGgaaaaDGgaDGga.aayacnaaaaa.ayn' +
    'aaaaaaaaacaaaaagcaaaaaanaraDGaDacaaaaag_anaraGIaDGIgaDGIgaDGgaDGga.' +
    'aayacnaaaaa.aaagaaaaaaaaDGaa.aaagaaaaaaaa.aaaraaaa.aaaanaraaaa.IDGI' +
    'gaDGIgaDGgaDGga.aynaaaaaaaaacaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaa' +
    'a.trgGragaaaacgGaaaaaaaG_aaaaa_Gaaaaaaaaa,.aGanar.anaraDDaaGIgaDGga' +
    '.aynaaaaaaaaacaaaaagcaaaaaaanyara.anyataaagacaDGaDaaaaag_caaaaag_an' +
    'araGIaDGIgaDGIgaDGgaDGga.aynaaaaaaaaacaaaaagcaaaaaaaraaaa.anyataaag' +
    'acaDaGgaaa.trgGragaaaacgGaaaaaaaG_aaaaa_aaaaaa,.aaaaacaDDaaGIgaDGga' +
    '.aynaaaaaaaaacaaaaagcaaaaaaanyara.anyataaagacaDGaDataaac_araaaaGIaD' +
    'GIgaDGIgaDaagcyaaGgaDGga.aayacnaaaaa.aaagaaaaaaaaDGaa.aaagaaaaaaaa.' +
    'aaaraaaa.aaaanaraaaa.IDGIgaDGIgaDGgcy.asaadanyaga.aa.aaaDGgaDGga.ay' +
    'naaaaaaaaacaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaa' +
    'aaaaaG_aaaaa_DaaaaGaa,.aDanyagaaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaa' +
    'aaaaanyara.anyataaagacaDGaDadanyagaaGIaDGIgaDGIgaDGIgcyDGgcy.asaaga' +
    'cras.agra_yratga.aa.aaaarsaaraa.aa.agra_yratga.aa.aaaDGgaDGga.aynaa' +
    'aaaaaaacaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaa' +
    'aaG_aaaaa_aGaaaaaaGaa,.aaratgaaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaa' +
    'aaaanyara.anyataaagacaDGaDaagra_yratgaaGIaDGIgaDGIgaDGIgcyDGgcy.asa' +
    'agacras.aratag.aa.aaaarsaaraa.aa.aratag.aa.aaaDGgaDGga.aynaaaaaaaaa' +
    'caaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaaaaG_aaa' +
    'aa_aaaaaGa,.aaratagaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaaaaaanyara.a' +
    'nyataaagacaDGaDaaratagaGIaDGIgaDGIgaDGIgcyDGgcy.asaagacras.gaaax_ar' +
    'atag.aa.aaaarsaaraa.aa.gaaax_aratag.aa.aaaDGgaDGga.aynaaaaaaaaacaaa' +
    'aagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaaaaG_aaaaa_G' +
    'aaaaaaaaaGa,.aaratagaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaaaaaanyara.' +
    'anyataaagacaDGaDagaaax_aratagaGIaDGIgaDGIgaDGIgcyDGgcy.asaagacras.c' +
    'ag_aaar.aa.aaaarsaaraa.aa.cag_aaar.aa.aaaDGgaDGga.aynaaaaaaaaacaaaa' +
    'agcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaaaaG_aaaaa_aa' +
    'Gaaaaa,.aaagaaaraDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaaaaaanyara.anya' +
    'taaagacaDGaDacag_aaaraGIaDGIgaDGIgaDGIgcyDGgcy.asaagacras.aaggaata_' +
    'aa_cynaga_cc.aa.aaaarsaaraa.aa.aaggaata_aa_cynaga_cc.aa.aaaDGgaDGga' +
    '.aynaaaaaaaaacaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacg' +
    'GaaaaaaaG_aaaaa_aaGGaaaa_aa_aaaaGa_aaa,.aaynagaIcagaDDaaGIgaDGga.ay' +
    'naaaaaaaaacaaaaagcaaaaaaanyara.anyataaagacaDGaDaaaggaata_aa_cynaga_' +
    'ccaGIaDGIgaDGIgaDGIgcyDGgcy.asaagacras.syaara_aanargra.aa.aaaarsaar' +
    'aa.aa.syaara_aanargra.aa.aaaDGgaDGga.aynaaaaaaaaacaaaaagcaaaaaaaraa' +
    'aa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaaaaG_aaaaa_aaaaaaaaaaaGaaa' +
    ',.aaanargraaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaaaaaanyara.anyataaag' +
    'acaDGaDasyaara_aanargraaGIaDGIgaDGIgaDGIgcyDGgcy.asaagacras.cynag_a' +
    'anargra.aa.aaaarsaaraa.aa.cynag_aanargra.aa.aaaDGgaDGga.aynaaaaaaaa' +
    'acaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaacgGaaaaaaaG_aa' +
    'aaa_aaaaGaaaaaGaaa,.aaanargraaDDaaGIgaDGga.aynaaaaaaaaacaaaaagcaaaa' +
    'aaanyara.anyataaagacaDGaDacynag_aanargraaGIaDGIgaDGIgaDGIgcyDGgaDGg' +
    'a.aynaaaaaaaaacaaaaagcaaaaaaaraaaa.anyataaagacaDaGgaaa.trgGragaaaac' +
    'gGaaaaaaaG';


var res = re.test(str);
assertTrue(res);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b1f3f2^!)  
[src/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/regexp-macro-assembler-ia32.cc?cl=0b1f3f2)  
[src/regexp-macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/regexp-macro-assembler-ia32.h?cl=0b1f3f2)  
[test/mjsunit/regress/regress-231.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-231.js?cl=0b1f3f2)  
  

---   

## **regress-87.js (v8 issue)**  
   
**[Issue: We should handle character escapes used as regexp flags](https://crbug.com/v8/87)**  
**[Commit: Regular Expression literal flags may contain unicode escapes. If these escape any of the](https://chromium.googlesource.com/v8/v8/+/396fa22)**  
  
Date(Commit): Thu Feb 12 09:09:28 2009  
Type: Defect  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1253](http://v8.googlecode.com/svn/branches/bleeding_edge@1253)  
Regress: [mjsunit/regress/regress-87.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-87.js)  
```javascript
assertThrows("/x/\\u0067");
assertThrows("/x/\\u0069");
assertThrows("/x/\\u006d");

assertThrows("/x/\\u0067i");
assertThrows("/x/\\u0069m");
assertThrows("/x/\\u006dg");

assertThrows("/x/m\\u0067");
assertThrows("/x/g\\u0069");
assertThrows("/x/i\\u006d");

assertThrows("/x/m\\u0067i");
assertThrows("/x/g\\u0069m");
assertThrows("/x/i\\u006dg");

assertThrows("/x/\\u0068");
assertThrows("/x/\\u0020");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/396fa22^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=396fa22)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=396fa22)  
[test/mjsunit/regress/regress-87.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-87.js?cl=396fa22)  
  

---   

## **regress-227.js (v8 issue)**  
   
**[Issue: Irregexp ignores high bits in choice nodes on ASCII strings](https://crbug.com/v8/227)**  
**[Commit: Issue 227 Fixed. Properly handles non-ASCII characters in quick-check on ASCII strings.](https://chromium.googlesource.com/v8/v8/+/c621bbb)**  
  
Date(Commit): Wed Feb 11 11:54:30 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1248](http://v8.googlecode.com/svn/branches/bleeding_edge@1248)  
Regress: [mjsunit/regress/regress-227.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-227.js)  
```javascript
var re = /\u23a1|x/;
var res = re.exec("!");
assertEquals(null, res, "Throwing away high bits on ASCII string");

res = re.exec("!x");
assertEquals(["x"], res, "Throwing away high bits on ASCII string");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c621bbb^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=c621bbb)  
[test/mjsunit/regress/regress-227.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-227.js?cl=c621bbb)  
  

---   

## **regress-225.js (v8 issue)**  
   
**[Issue: Search and replace with zero length match where replaceValue is a function.](https://crbug.com/v8/225)**  
**[Commit: Fix bug 225 in regexp replace with function.](https://chromium.googlesource.com/v8/v8/+/b0e3ee6)**  
  
Date(Commit): Thu Feb 05 13:24:13 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1232](http://v8.googlecode.com/svn/branches/bleeding_edge@1232)  
Regress: [mjsunit/regress/regress-225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-225.js)  
```javascript
assertEquals("foo", "foo".replace(/(?:)/g, function() { return ""; }));

assertEquals("foo", "foo".replace(/(?:)/g, ""));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0e3ee6^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=b0e3ee6)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=b0e3ee6)  
[test/mjsunit/regress/regress-225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-225.js?cl=b0e3ee6)  
  

---   

## **regress-219.js (v8 issue)**  
   
**[Issue: Ignore dupes in regexp flags](https://crbug.com/v8/219)**  
**[Commit: Allow duplicate flags in regexps to match other browsers.](https://chromium.googlesource.com/v8/v8/+/0730ada)**  
  
Date(Commit): Fri Jan 30 12:36:40 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1193](http://v8.googlecode.com/svn/branches/bleeding_edge@1193)  
Regress: [mjsunit/regress/regress-219.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-219.js)  
```javascript
function assertFlags(re, global, multiline, ignoreCase) {
  var name = re + " flag: ";
  (global ? assertTrue : assertFalse)(re.global, name + "g");
  (multiline ? assertTrue : assertFalse)(re.multiline, name + "m");
  (ignoreCase ? assertTrue : assertFalse)(re.ignoreCase, name + "i");
}

var re = /a/;
assertFlags(re, false, false, false)

re = /a/gim;
assertFlags(re, true, true, true)

re = RegExp("a","");
assertFlags(re, false, false, false)

re = RegExp("a", "gim");
assertFlags(re, true, true, true)


assertThrows("/a/ii");

assertThrows("/a/gii");

assertThrows("/a/igi");

assertThrows("/a/iig");

assertThrows("/a/gimi");

assertThrows("/a/giim");

assertThrows("/a/igim");

assertThrows(function(){ return RegExp("a", "ii"); })

assertThrows(function(){ return RegExp("a", "gii"); })

assertThrows(function(){ return RegExp("a", "igi"); })

assertThrows(function(){ return RegExp("a", "iig"); })

assertThrows(function(){ return RegExp("a", "gimi"); })

assertThrows(function(){ return RegExp("a", "giim"); })

assertThrows(function(){ return RegExp("a", "igim"); })


assertThrows("/a/iii");

assertThrows("/a/giii");

assertThrows("/a/igii");

assertThrows("/a/iigi");

assertThrows("/a/iiig");

assertThrows("/a/miiig");

assertThrows(function(){ return RegExp("a", "iii"); })

assertThrows(function(){ return RegExp("a", "giii"); })

assertThrows(function(){ return RegExp("a", "igii"); })

assertThrows(function(){ return RegExp("a", "iigi"); })

assertThrows(function(){ return RegExp("a", "iiig"); })

assertThrows(function(){ return RegExp("a", "miiig"); })


assertThrows("/a/arglebargleglopglyf");

assertThrows("/a/arglebargleglopglif");

assertThrows("/a/arglebargleglopglym");

assertThrows("/a/arglebargleglopglim");


var re = /a/gmi;
assertFlags(re, true, true, true)

assertThrows("/a/Gmi");

assertThrows("/a/gMi");

assertThrows("/a/gmI");

assertThrows("/a/GMi");

assertThrows("/a/GmI");

assertThrows("/a/gMI");

assertThrows("/a/GMI");


assertThrows("/a/\\u0067");
assertThrows("/a/\\u0069");
assertThrows("/a/\\u006d");
assertThrows("/a/\\u006D");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0730ada^!)  
[src/regexp-delay.js](https://cs.chromium.org/chromium/src/v8/src/regexp-delay.js?cl=0730ada)  
[test/mjsunit/regress/regress-219.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-219.js?cl=0730ada)  
  

---   

## **regress-192.js (v8 issue)**  
   
**[Issue: CHECK(result != Top::has_pending_exception()) failed](https://crbug.com/v8/192)**  
**[Commit: Fix issue 192 by propagating out exceptions from object literal](https://chromium.googlesource.com/v8/v8/+/524e34b)**  
  
Date(Commit): Mon Jan 26 13:10:26 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/18749](http://codereview.chromium.org/18749)  
Regress: [mjsunit/regress/regress-192.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-192.js)  
```javascript
Object.prototype.__defineGetter__("x", function() {});
Object.prototype.__defineSetter__("y",
                                  function() { assertUnreachable("setter"); });

var x = ({ x: 42, y: 37 });
assertEquals(42, x.x);
assertEquals(37, x.y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/524e34b^!)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=524e34b)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=524e34b)  
[test/mjsunit/regress/regress-192.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-192.js?cl=524e34b)  
  

---   

## **regress-201.js (v8 issue)**  
   
**[Issue: array.sort() crash](https://crbug.com/v8/201)**  
**[Commit: Do not violate the assumption that fast-case arrays have Smi length](https://chromium.googlesource.com/v8/v8/+/39842ba)**  
  
Date(Commit): Fri Jan 23 13:08:29 2009  
Type: Bug  
Code Review: [http://code.google.com/p/v8/issues/detail?id=201](http://code.google.com/p/v8/issues/detail?id=201)  
Regress: [mjsunit/regress/regress-201.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-201.js)  
```javascript
function testsort(n) {
  n=1*n;
  var numbers=new Array(n);
  for (var i=0;i<n;i++) numbers[i]=i;
  numbers.sort();
}

testsort("5001")  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39842ba^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=39842ba)  
[test/mjsunit/regress/regress-201.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-201.js?cl=39842ba)  
  

---   

## **regress-193.js (v8 issue)**  
   
**[Issue: Fatal error in src/builtins.cc, line 127 - unreachable code](https://crbug.com/v8/193)**  
**[Commit: Make sure that eval and try-catch introduced context extension objects](https://chromium.googlesource.com/v8/v8/+/8a73135)**  
  
Date(Commit): Fri Jan 23 12:16:03 2009  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=193.](http://code.google.com/p/v8/issues/detail?id=193.)  
Regress: [mjsunit/regress/regress-193.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-193.js)  
```javascript
function f() {
  return eval("var x; constructor");
}

f()();

assertEquals(constructor, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a73135^!)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=8a73135)  
[test/mjsunit/regress/regress-193.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-193.js?cl=8a73135)  
  

---   

## **regress-189.js (v8 issue)**  
   
**[Issue: Crash [@ v8::internal::Runtime_InitializeConstContextSlot] and CHECK(attributes != ABSENT && (attributes & READ_ONLY) != 0) failed](https://crbug.com/v8/189)**  
**[Commit: Fix handling of const initialization.  We did not handle the fact that](https://chromium.googlesource.com/v8/v8/+/c23dbc1)**  
  
Date(Commit): Thu Jan 22 13:53:06 2009  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=189](http://code.google.com/p/v8/issues/detail?id=189)  
Regress: [mjsunit/regress/regress-189.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-189.js)  
```javascript
function f() {
  eval("delete x; const x = 32");
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c23dbc1^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=c23dbc1)  
[test/mjsunit/const-eval-init.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/const-eval-init.js?cl=c23dbc1)  
[test/mjsunit/const.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/const.js?cl=c23dbc1)  
[test/mjsunit/regress/regress-189.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-189.js?cl=c23dbc1)  
  

---   

## **regress-74.js (v8 issue)**  
   
**[Issue: Caught variables should be DontDelete](https://crbug.com/v8/74)**  
**[Commit: Change the handling of catch blocks to use context extension objects](https://chromium.googlesource.com/v8/v8/+/47d1298)**  
  
Date(Commit): Fri Jan 16 09:42:08 2009  
Type: Defect  
Code Review: [http://codereview.chromium.org/18143](http://codereview.chromium.org/18143)  
Regress: [mjsunit/regress/regress-74.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-74.js)  
```javascript
function test() {
  try {
    throw 42;
  } catch(e) {
    assertFalse(delete e, "deleting catch variable");
    assertEquals(42, e);
  }
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/47d1298^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=47d1298)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=47d1298)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=47d1298)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=47d1298)  
[src/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/prettyprinter.cc?cl=47d1298)  
...  
  

---   

## **regress-191.js (v8 issue)**  
   
**[Issue: CHECK(context_ext->GetLocalPropertyAttribute(*name) == mode) failed](https://crbug.com/v8/191)**  
**[Commit: Fix issue 191:](https://chromium.googlesource.com/v8/v8/+/384b0a54)**  
  
Date(Commit): Thu Jan 15 11:31:08 2009  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=191](http://code.google.com/p/v8/issues/detail?id=191)  
Regress: [mjsunit/regress/regress-191.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-191.js)  
```javascript
var setterCalled = false;

Object.prototype.__defineSetter__("x", function() { setterCalled = true; });

function test() {
  eval("var x = 42");
  assertFalse(setterCalled, "accessor setter call on context object");
  assertEquals(42, eval("x"));
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/384b0a54^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=384b0a54)  
[test/mjsunit/regress/regress-191.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-191.js?cl=384b0a54)  
  

---   

## **regress-187.js (v8 issue)**  
   
**[Issue: Captures made in regexp lookahead should be cleared on match failure](https://crbug.com/v8/187)**  
**[Commit: Added clearing of captures before entering the body of a loop.  This](https://chromium.googlesource.com/v8/v8/+/d6e6508)**  
  
Date(Commit): Wed Jan 14 11:32:23 2009  
Type: Bug  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@1070](http://v8.googlecode.com/svn/branches/bleeding_edge@1070)  
Regress: [mjsunit/regress/regress-187.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-187.js)  
```javascript
assertEquals(["f", undefined], "foo".match(/(?:(?=(f)o)fx|)./));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6e6508^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=d6e6508)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=d6e6508)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=d6e6508)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=d6e6508)  
[src/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/regexp-macro-assembler-ia32.cc?cl=d6e6508)  
...  
  

---   

## **regress-186.js (v8 issue)**  
   
**[Issue: CHECK(!context_ext->HasLocalProperty(*name)) failed](https://crbug.com/v8/186)**  
**[Commit: Add failing test for issue 186:](https://chromium.googlesource.com/v8/v8/+/cd1afea)**  
  
Date(Commit): Wed Jan 14 09:20:13 2009  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=186](http://code.google.com/p/v8/issues/detail?id=186)  
Regress: [mjsunit/regress/regress-186.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-186.js)  
```javascript
var setterCalled = false;

var o = {};
o.__defineSetter__("x", function() { setterCalled = true; });

function runTest(test) {
  setterCalled = false;
  test();
}

function testLocal() {
  // Add property called __proto__ to the extension object.
  eval("var __proto__ = o");
  // Check that the extension object's prototype did not change.
  eval("var x = 27");
  assertFalse(setterCalled, "prototype of extension object changed");
  assertEquals(o, eval("__proto__"));
}

function testGlobal() {
  // Assign to the global __proto__ property.
  eval("__proto__ = o");
  // Check that the prototype of the global object changed.
  eval("x = 27");
  assertTrue(setterCalled, "prototype of global object did not change");
  setterCalled = false;
  assertEquals(o, eval("__proto__"));
}

runTest(testLocal);
runTest(testGlobal);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd1afea^!)  
[test/mjsunit/bugs/bug-186.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-186.js?cl=cd1afea)  
  

---   

## **regress-171.js (v8 issue)**  
   
**[Issue: ARM function call with string value and then string object fails.](https://crbug.com/v8/171)**  
**[Commit: Fix for issue 171:](https://chromium.googlesource.com/v8/v8/+/f3da5ff)**  
  
Date(Commit): Wed Jan 07 23:26:31 2009  
Type: Bug  
Code Review: [http://codereview.chromium.org/16594](http://codereview.chromium.org/16594)  
Regress: [mjsunit/regress/regress-171.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-171.js)  
```javascript
function f(s) { return s.length; }
function g(s, key) { return s[key]; }

assertEquals(f(new String("a")), 1);
assertEquals(f(new String("a")), 1);
assertEquals(f(new String("a")), 1);
assertEquals(f("a"), 1);
assertEquals(f(new String("a")), 1);

assertEquals(g(new String("a"), "length"), 1);
assertEquals(g(new String("a"), "length"), 1);
assertEquals(g(new String("a"), "length"), 1);
assertEquals(g("a", "length"), 1);
assertEquals(g(new String("a"), "length"), 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f3da5ff^!)  
[src/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache-arm.cc?cl=f3da5ff)  
[test/mjsunit/regress/regress-171.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-171.js?cl=f3da5ff)  
  

---   

## **regress-91.js (v8 issue)**  
   
**[Issue: WebKit test for nan dates fails](https://crbug.com/v8/91)**  
**[Commit: Fix for issue 91 (http://code.google.com/p/v8/issues/detail?id=91)](https://chromium.googlesource.com/v8/v8/+/726aa85)**  
  
Date(Commit): Wed Jan 07 09:58:58 2009  
Type: ----  
Code Review: [http://codereview.chromium.org/17232](http://codereview.chromium.org/17232)  
Regress: [mjsunit/regress/regress-91.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-91.js)  
```javascript
var date = new Date();
var year = date.getYear();
date.setMilliseconds(Number.NaN);
date.setYear(1900 + year);
assertEquals(year, date.getYear());
assertEquals(0, date.getMonth());
assertEquals(1, date.getDate());
assertEquals(0, date.getHours());
assertEquals(0, date.getMinutes());
assertEquals(0, date.getSeconds());
assertEquals(0, date.getMilliseconds());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/726aa85^!)  
[src/date-delay.js](https://cs.chromium.org/chromium/src/v8/src/date-delay.js?cl=726aa85)  
[test/mjsunit/regress/regress-91.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-91.js?cl=726aa85)  
  

---   
