# V8Harvest  
The Harvest of V8 other regress.  
  

## **regress file:regress-regexp-functional-replace-slow.js**  
   
**[Commit: Reland "[regexp] Introduce species constructor protector for regexps."](https://chromium.googlesource.com/v8/v8/+/a27a42f)**  
  
Date(Commit): Mon Nov 19 10:58:01 2018  
Code Review: [https://chromium-review.googlesource.com/c/1335696](https://chromium-review.googlesource.com/c/1335696)  
Regress: [mjsunit/regress-regexp-functional-replace-slow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-regexp-functional-replace-slow.js)  
```javascript
/a/.constructor = "";

assertEquals("b", "a".replace(/a/, () => "b"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a27a42f^!)  
---   

## **regress file:regress-directive.js**  
   
**[Commit: [parser] Optimize directive parsing especially for preparser](https://chromium.googlesource.com/v8/v8/+/9d34fa0)**  
  
Date(Commit): Fri Nov 02 16:09:46 2018  
Code Review: [https://chromium-review.googlesource.com/c/1314570](https://chromium-review.googlesource.com/c/1314570)  
Regress: [mjsunit/regress/regress-directive.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-directive.js)  
```javascript
function f() {
  'use strict'
  in Number
}

f.arguments  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9d34fa0^!)  
---   

## **regress file:regress-preparse-inner-arrow-duplicate-parameter.js**  
   
**[Commit: [parser] Rewrite duplicate formal detection](https://chromium.googlesource.com/v8/v8/+/e874d6a)**  
  
Date(Commit): Tue Oct 09 09:17:42 2018  
Code Review: [https://chromium-review.googlesource.com/c/1270578](https://chromium-review.googlesource.com/c/1270578)  
Regress: [mjsunit/regress/regress-preparse-inner-arrow-duplicate-parameter.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-preparse-inner-arrow-duplicate-parameter.js)  
```javascript
assertThrows("()=>{ (x,x)=>1 }", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e874d6a^!)  
---   

## **regress file:regress-arrow-single-expression-eval.js**  
   
**[Commit: [parser] Fix single-expression arrow function scoping](https://chromium.googlesource.com/v8/v8/+/af34c6c)**  
  
Date(Commit): Mon Oct 08 13:43:21 2018  
Code Review: [https://chromium-review.googlesource.com/c/1268236](https://chromium-review.googlesource.com/c/1268236)  
Regress: [mjsunit/regress/regress-arrow-single-expression-eval.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arrow-single-expression-eval.js)  
```javascript
((x=1) => eval("var x = 10"))();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/af34c6c^!)  
---   

## **regress file:regress-tonumbercode.js**  
   
**[Commit: [turbofan] Inline Number constructor in certain cases](https://chromium.googlesource.com/v8/v8/+/9eca23e)**  
  
Date(Commit): Mon Jul 16 10:02:42 2018  
Code Review: [https://chromium-review.googlesource.com/1118557](https://chromium-review.googlesource.com/1118557)  
Regress: [mjsunit/harmony/bigint/regress-tonumbercode.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/bigint/regress-tonumbercode.js)  
```javascript
function f(x, b) {
    if (b) return Math.trunc(+(x))
    else return Math.trunc(Number(x))
}

f("1", true);
f("2", true);
f("2", false);
%OptimizeFunctionOnNextCall(f);
f(3n);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9eca23e^!)  
---   

## **regress file:regress-store-transition-dict.js**  
   
**[Commit: [ic] Use Map as transition handlers instead of StoreHandler objects.](https://chromium.googlesource.com/v8/v8/+/78c6bbd)**  
  
Date(Commit): Fri Mar 23 15:37:40 2018  
Code Review: [https://chromium-review.googlesource.com/931701](https://chromium-review.googlesource.com/931701)  
Regress: [mjsunit/regress/regress-store-transition-dict.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-transition-dict.js)  
```javascript
(function() {
  function SetX(o, v) {
    o.x = v;
  }

  function SetY(o, v) {
    o.y = v;
  }

  var p = {};

  function Create() {
    var o = {__proto__:p, b:1, a:2};
    delete o.b;
    assertFalse(%HasFastProperties(o));
    return o;
  }

  for (var i = 0; i < 10; i++) {
    var o = Create();
    SetX(o, 13);
    SetY(o, 13);
  }

  Object.defineProperty(p, "x", {value:42, configurable: true, writable: false});

  for (var i = 0; i < 10; i++) {
    var o = Create();
    SetY(o, 13);
  }

  var o = Create();
  assertEquals(42, o.x);
  SetX(o, 13);
  assertEquals(42, o.x);
})();


(function() {
  var p1 = {a:10};
  Object.defineProperty(p1, "x", {value:42, configurable: true, writable: false});

  var p2 = {__proto__: p1, x:153};
  for (var i = 0; i < 2000; i++) {
    p1["p" + i] = 0;
    p2["p" + i] = 0;
  }
  assertFalse(%HasFastProperties(p1));
  assertFalse(%HasFastProperties(p2));

  function GetX(o) {
    return o.x;
  }
  function SetX(o, v) {
    o.x = v;
  }

  function Create() {
    var o = {__proto__:p2, b:1, a:2};
    return o;
  }

  for (var i = 0; i < 10; i++) {
    var o = Create();
    assertEquals(153, GetX(o));
    SetX(o, 13);
    assertEquals(13, GetX(o));
  }

  delete p2.x;
  assertFalse(%HasFastProperties(p1));
  assertFalse(%HasFastProperties(p2));

  var o = Create();
  assertEquals(42, GetX(o));
  SetX(o, 13);
  assertEquals(42, GetX(o));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78c6bbd^!)  
---   

## **regress file:regress-charat-empty.js**  
   
**[Commit: [turbofan] Speculate on bounds checks for String#char[Code]At](https://chromium.googlesource.com/v8/v8/+/ee2d85a)**  
  
Date(Commit): Fri Jan 26 12:00:58 2018  
Code Review: [https://chromium-review.googlesource.com/873382](https://chromium-review.googlesource.com/873382)  
Regress: [mjsunit/regress/regress-charat-empty.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-charat-empty.js)  
```javascript
(() => {
  function f(s) {
    return s.charAt();
  }
  f("");
  f("");
  %OptimizeFunctionOnNextCall(f);
  f("");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee2d85a^!)  
---   

## **regress file:regress-stringAt-boundsCheck.js**  
   
**[Commit: [turbofan] Add effects to StringAt operators](https://chromium.googlesource.com/v8/v8/+/90e50cc2)**  
  
Date(Commit): Wed Jan 24 12:12:27 2018  
Code Review: [https://chromium-review.googlesource.com/881781](https://chromium-review.googlesource.com/881781)  
Regress: [mjsunit/regress/regress-stringAt-boundsCheck.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-stringAt-boundsCheck.js)  
```javascript
(() => {
  function f(u) {
    for (var j = 0; j < 20; ++j) {
        print("" + u.codePointAt());
    }
  }

  f("test");
  f("foo");
  %OptimizeFunctionOnNextCall(f);
  f("");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90e50cc2^!)  
---   

## **regress file:regress-refreeze-same-map.js**  
   
**[Commit: [runtime] PreventExtensionsWithTransition: before adding the new](https://chromium.googlesource.com/v8/v8/+/6681949)**  
  
Date(Commit): Thu Jun 22 12:19:26 2017  
Code Review: [https://codereview.chromium.org/2915863004](https://codereview.chromium.org/2915863004)  
Regress: [mjsunit/regress/regress-refreeze-same-map.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-refreeze-same-map.js)  
```javascript
assertTrue(%HaveSameMap(Object.freeze({}),     Object.freeze({})));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.freeze({a: 1})));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.freeze([])));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.freeze([1,2])));

assertTrue(%HaveSameMap(Object.seal({}),     Object.seal({})));
assertTrue(%HaveSameMap(Object.seal({a: 1}), Object.seal({a: 1})));
assertTrue(%HaveSameMap(Object.seal([]),     Object.seal([])));
assertTrue(%HaveSameMap(Object.seal([1,2]),  Object.seal([1,2])));


assertTrue(%HaveSameMap(Object.freeze({}),     Object.freeze( Object.freeze({}) )));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.freeze( Object.freeze({a: 1}) )));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.freeze( Object.freeze([]) )));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.freeze( Object.freeze([1,2]) )));


assertTrue(%HaveSameMap(Object.seal({}),     Object.seal( Object.seal({}) )));
assertTrue(%HaveSameMap(Object.seal({a: 1}), Object.seal( Object.seal({a: 1}) )));
assertTrue(%HaveSameMap(Object.seal([]),     Object.seal( Object.seal([]) )));
assertTrue(%HaveSameMap(Object.seal([1,2]),  Object.seal( Object.seal([1,2]) )));


assertTrue(%HaveSameMap(Object.freeze({}),     Object.seal( Object.freeze({}) )));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.seal( Object.freeze({a: 1}) )));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.seal( Object.freeze([]) )));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.seal( Object.freeze([1,2]) )));


assertTrue(%HaveSameMap(Object.freeze(Object.seal({})), Object.seal({})));


assertTrue(%HaveSameMap(Object.seal(Object.preventExtensions({})), Object.preventExtensions({})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6681949^!)  
---   

## **regress file:regress-generators-resume.js**  
   
**[Commit: This is a first step towards reducing the number of stores/loads when suspending/resuming a generator.](https://chromium.googlesource.com/v8/v8/+/f064561)**  
  
Date(Commit): Fri Jun 02 11:55:48 2017  
Code Review: [https://codereview.chromium.org/2894293003](https://codereview.chromium.org/2894293003)  
Regress: [mjsunit/harmony/regress-generators-resume.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress-generators-resume.js)  
```javascript
function* foo() {
  for (let i = 0; i < 10; i++) {
    yield 1;
  }
  return 0;
}

g = foo();
%OptimizeFunctionOnNextCall(foo);
g.next();
g.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f064561^!)  
---   

## **regress file:regress-unlink-closures-on-deopt.js**  
   
**[Commit: [deoptimizer] When deoptimizing code, unlink all functions referring to that code.](https://chromium.googlesource.com/v8/v8/+/fe70447)**  
  
Date(Commit): Mon Mar 06 12:22:05 2017  
Code Review: [https://codereview.chromium.org/2730323002](https://codereview.chromium.org/2730323002)  
Regress: [mjsunit/regress/regress-unlink-closures-on-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-unlink-closures-on-deopt.js)  
```javascript
function foo() {
  function g(o) {
    return o.f;
  }
  return g;
}

let g1 = foo();
let g2 = foo();

g1({ f : 1});
g1({ f : 2});
g2({ f : 2});
g2({ f : 2});

%OptimizeFunctionOnNextCall(g1);
%OptimizeFunctionOnNextCall(g2);

g1({ f : 1});
g2({ f : 2});
g1({});

assertUnoptimized(g1);





assertUnoptimized(g2);

g2({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe70447^!)  
---   

## **regress file:regress-private-enumerable.js**  
   
**[Commit: Add test for making private symbols non-enumerable](https://chromium.googlesource.com/v8/v8/+/942604d)**  
  
Date(Commit): Mon Nov 14 09:17:07 2016  
Code Review: [https://codereview.chromium.org/2498963002](https://codereview.chromium.org/2498963002)  
Regress: [mjsunit/regress/regress-private-enumerable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-private-enumerable.js)  
```javascript
class A {}
class B {}
Object.assign(B, A);
assertEquals("class B {}", B.toString());

(function() {
  function f(a, i, v) {
    a[i] = v;
  }

  f("make it generic", 0, 0);

  var o = {foo: "foo"};
  %OptimizeObjectForAddingMultipleProperties(o, 10);

  var s = %CreatePrivateSymbol("priv");
  f(o, s, "private");
  %ToFastProperties(o);

  var desc = Object.getOwnPropertyDescriptor(o, s);
  assertEquals("private", desc.value);
  assertTrue(desc.writable);
  assertFalse(desc.enumerable);
  assertTrue(desc.configurable);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/942604d^!)  
---   

## **regress file:regress-trap-allocation-memento.js**  
   
**[Commit: [stubs] Fix CodeStubAssembler::TrapAllocationMemento](https://chromium.googlesource.com/v8/v8/+/cc2a277)**  
  
Date(Commit): Thu Nov 10 13:47:41 2016  
Code Review: [https://codereview.chromium.org/2487943005](https://codereview.chromium.org/2487943005)  
Regress: [mjsunit/regress/regress-trap-allocation-memento.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-trap-allocation-memento.js)  
```javascript
var elements_kind = {
  fast_smi_only            :  'fast smi only elements',
  fast                     :  'fast elements',
  fast_double              :  'fast double elements',
  dictionary               :  'dictionary elements',
}

function getKind(obj) {
  if (%HasSmiElements(obj)) return elements_kind.fast_smi_only;
  if (%HasObjectElements(obj)) return elements_kind.fast;
  if (%HasDoubleElements(obj)) return elements_kind.fast_double;
  if (%HasDictionaryElements(obj)) return elements_kind.dictionary;
}

function assertKind(expected, obj, name_opt) {
  assertEquals(expected, getKind(obj), name_opt);
}

(function() {
  function make1() { return new Array(); }
  function make2() { return new Array(); }
  function make3() { return new Array(); }
  function foo(a, i) { a[0] = i; }

  function run_test(maker_function) {
    var one = maker_function();
    assertKind(elements_kind.fast_smi_only, one);
    
    foo(one, 1.5);
    
    var two = maker_function();
    assertKind(elements_kind.fast_double, two);
  }

  
  
  run_test(make1);
  
  
  
  run_test(make2);
  
  run_test(make3);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc2a277^!)  
---   

## **regress file:regress-abort-context-allocate-params.js**  
   
**[Commit: Mark param as used when we force context allocation due to implement access through arguments](https://chromium.googlesource.com/v8/v8/+/9feab2d)**  
  
Date(Commit): Mon Oct 03 17:21:20 2016  
Code Review: [https://codereview.chromium.org/2386623002](https://codereview.chromium.org/2386623002)  
Regress: [mjsunit/regress/regress-abort-context-allocate-params.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-abort-context-allocate-params.js)  
```javascript
function f(getter) {
  arguments = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9feab2d^!)  
---   

## **regress file:regress-escape-analysis-indirect.js**  
   
**[Commit: [turbofan] Fix indirect escapes in escape analysis.](https://chromium.googlesource.com/v8/v8/+/437a33e)**  
  
Date(Commit): Tue Sep 27 14:53:17 2016  
Code Review: [https://codereview.chromium.org/2369953002](https://codereview.chromium.org/2369953002)  
Regress: [mjsunit/compiler/regress-escape-analysis-indirect.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-escape-analysis-indirect.js)  
```javascript
function f(apply) {
  var value = 23;
  apply(function bogeyman() { value = 42 });
  return value;
}
function apply(fun) { fun() }
assertEquals(42, f(apply));
assertEquals(42, f(apply));
%NeverOptimizeFunction(apply);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f(apply));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/437a33e^!)  
---   

## **regress file:regress-abort-preparsing-params.js**  
   
**[Commit: Don't reset parameters if we aborted preparsing, rebuild them from the params_ list](https://chromium.googlesource.com/v8/v8/+/c0ded71)**  
  
Date(Commit): Tue Sep 27 13:05:32 2016  
Code Review: [https://codereview.chromium.org/2372703004](https://codereview.chromium.org/2372703004)  
Regress: [mjsunit/regress/regress-abort-preparsing-params.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-abort-preparsing-params.js)  
```javascript
var outer_a;

function f(a, b, a) {
  outer_a = a;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
}
f(1, 2, 1);
assertEquals(1, outer_a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c0ded71^!)  
---   

## **regress file:regress-arguments-liveness-analysis.js**  
   
**[Commit: Remove ARGUMENTS_VARIABLE and fix crankshaft to properly detect the arguments object and keep it alive when inlining .apply](https://chromium.googlesource.com/v8/v8/+/7f025eb)**  
  
Date(Commit): Fri Sep 23 14:27:02 2016  
Code Review: [https://codereview.chromium.org/2367483003](https://codereview.chromium.org/2367483003)  
Regress: [mjsunit/regress/regress-arguments-liveness-analysis.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arguments-liveness-analysis.js)  
```javascript
function r(v) { return v.f }
function h() { }
function y(v) {
  var x = arguments;
  h.apply(r(v), x);
};

y({f:3});
y({f:3});
y({f:3});

%OptimizeFunctionOnNextCall(y);

y({ f : 3, u : 4 });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7f025eb^!)  
---   

## **regress file:regress-compare-negate.js**  
   
**[Commit: [arm64] Use CMN for cmp(a,sub(0,b)) only when checking equality/inequality.](https://chromium.googlesource.com/v8/v8/+/fdb0f07)**  
  
Date(Commit): Wed Sep 07 12:43:00 2016  
Code Review: [https://codereview.chromium.org/2318043002](https://codereview.chromium.org/2318043002)  
Regress: [mjsunit/compiler/regress-compare-negate.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-compare-negate.js)  
```javascript
function CompareNegate(a,b) {
 a = a|0;
 b = b|0;
 var sub = 0 - b;
 return a < (sub|0);
}

var x = CompareNegate(1,0x80000000);
%OptimizeFunctionOnNextCall(CompareNegate);
CompareNegate(1,0x80000000);
assertOptimized(CompareNegate);
assertEquals(x, CompareNegate(1,0x80000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fdb0f07^!)  
---   

## **regress file:regress-math-sign-nan-type.js**  
   
**[Commit: [turbofan] Fix typing rule for Math.sign.](https://chromium.googlesource.com/v8/v8/+/25504a2)**  
  
Date(Commit): Thu Sep 01 20:06:27 2016  
Code Review: [https://codereview.chromium.org/2306583002](https://codereview.chromium.org/2306583002)  
Regress: [mjsunit/compiler/regress-math-sign-nan-type.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-math-sign-nan-type.js)  
```javascript
function f(a) {
  return Math.sign(+a) < 2;
}

f(NaN);
f(NaN);
%OptimizeFunctionOnNextCall(f);
assertFalse(f(NaN));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/25504a2^!)  
---   

## **regress file:regress-loop-variable-if.js**  
   
**[Commit: [turbofan] Fix silly bug in loop variable analysis.](https://chromium.googlesource.com/v8/v8/+/ad8e0e2)**  
  
Date(Commit): Mon Aug 08 15:50:57 2016  
Code Review: [https://codereview.chromium.org/2222953003](https://codereview.chromium.org/2222953003)  
Regress: [mjsunit/compiler/regress-loop-variable-if.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-loop-variable-if.js)  
```javascript
function f() {
  for (var i = 0; i != 10; i++) {
    if (i < 8) print("x");
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad8e0e2^!)  
---   

## **regress file:regress-loop-variable-unsigned.js**  
   
**[Commit: [turbofan] Insert sigma nodes for loop variable backedge.](https://chromium.googlesource.com/v8/v8/+/e144335)**  
  
Date(Commit): Fri Aug 05 14:34:05 2016  
Code Review: [https://codereview.chromium.org/2222513002](https://codereview.chromium.org/2222513002)  
Regress: [mjsunit/compiler/regress-loop-variable-unsigned.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-loop-variable-unsigned.js)  
```javascript
(function() {
  function f() {
    for (var i = 0; i < 4294967295; i += 2) {
      if (i === 10) break;
    }
  }
  f();
})();

(function() {
  function f() {
    for (var i = 0; i < 4294967293; i += 2) {
      if (i === 10) break;
    }
  }
  f();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e144335^!)  
---   

## **regress file:regress-double-canonicalization.js**  
   
**[Commit: Fix double canonicalization](https://chromium.googlesource.com/v8/v8/+/c17b44b)**  
  
Date(Commit): Thu Jun 30 15:18:16 2016  
Code Review: [https://codereview.chromium.org/2106393002](https://codereview.chromium.org/2106393002)  
Regress: [mjsunit/regress/regress-double-canonicalization.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-double-canonicalization.js)  
```javascript
var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper()
i_view[1] = %GetHoleNaNLower();
var hole_nan = (new Float64Array(ab))[0];

var array = [];

function write() {
  array[0] = hole_nan;
}

write();
%OptimizeFunctionOnNextCall(write);
write();
array[1] = undefined;
assertTrue(isNaN(array[0]));
assertEquals("number", typeof array[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c17b44b^!)  
---   

## **regress file:regress-string-to-number-add.js**  
   
**[Commit: [turbofan] Mark side-effect-free calls to string ops as kEliminatable.](https://chromium.googlesource.com/v8/v8/+/14a1a7e)**  
  
Date(Commit): Wed Jun 15 11:39:40 2016  
Code Review: [https://codereview.chromium.org/2063373003](https://codereview.chromium.org/2063373003)  
Regress: [mjsunit/compiler/regress-string-to-number-add.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-string-to-number-add.js)  
```javascript
function f(x) {
  var s = x ? "0" : "1";
  return 1 + Number(s);
}

f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
assertEquals(2, f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14a1a7e^!)  
---   

## **regress file:regress-store-holey-double-array.js**  
   
**[Commit: [turbofan] Prevent storing signalling NaNs into holey double arrays.](https://chromium.googlesource.com/v8/v8/+/6470dda)**  
  
Date(Commit): Tue Jun 14 08:24:43 2016  
Code Review: [https://codereview.chromium.org/2060233002](https://codereview.chromium.org/2060233002)  
Regress: [mjsunit/compiler/regress-store-holey-double-array.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-store-holey-double-array.js)  
```javascript
(function StoreHoleBitPattern() {
  function g(src, dst, i) {
    dst[i] = src[i];
  }

  var b = new ArrayBuffer(16);
  var i32 = new Int32Array(b);
  i32[0] = 0xFFF7FFFF;
  i32[1] = 0xFFF7FFFF;
  i32[3] = 0xFFF7FFFF;
  i32[4] = 0xFFF7FFFF;
  var f64 = new Float64Array(b);

  var a = [,0.1];

  g(f64, a, 1);
  g(f64, a, 1);
  %OptimizeFunctionOnNextCall(g);
  g(f64, a, 0);

  assertTrue(Number.isNaN(a[0]));
})();


(function ConvertHoleToNumberAndStore() {
  function g(a, i) {
    var x = a[i];
    a[i] = +x;
  }

  var a=[,0.1];
  g(a, 1);
  g(a, 1);
  %OptimizeFunctionOnNextCall(g);
  g(a, 0);
  assertTrue(Number.isNaN(a[0]));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6470dda^!)  
---   

## **regress file:regress-truncate-number-or-undefined-to-float64.js**  
   
**[Commit: [turbofan] Distinguish between change- and truncate-tagged-to-float64.](https://chromium.googlesource.com/v8/v8/+/5e96f47)**  
  
Date(Commit): Tue May 31 12:01:40 2016  
Code Review: [https://codereview.chromium.org/2027593002](https://codereview.chromium.org/2027593002)  
Regress: [mjsunit/compiler/regress-truncate-number-or-undefined-to-float64.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-truncate-number-or-undefined-to-float64.js)  
```javascript
function g(a, b) {
  a = +a;
  if (b) {
    a = undefined;
  }
  print(a);
  return +a;
}

g(0);
g(0);
%OptimizeFunctionOnNextCall(g);
assertTrue(Number.isNaN(g(0, true)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e96f47^!)  
---   

## **regress file:regress-string-from-char-code-tonumber.js**  
   
**[Commit: [builtins] Migrate String.fromCharCode to TurboFan code stub.](https://chromium.googlesource.com/v8/v8/+/7554360)**  
  
Date(Commit): Tue May 31 11:39:05 2016  
Code Review: [https://codereview.chromium.org/2021143003](https://codereview.chromium.org/2021143003)  
Regress: [mjsunit/regress/regress-string-from-char-code-tonumber.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-string-from-char-code-tonumber.js)  
```javascript
var thrower = { [Symbol.toPrimitive]: function() { FAIL } };

function testTrace(func) {
  try {
    func(thrower);
    assertUnreachable();
  } catch (e) {
    assertTrue(e.stack.indexOf("fromCharCode") >= 0);
  }
}

testTrace(String.fromCharCode);

function foo(x) { return String.fromCharCode(x); }

foo(1);
foo(2);
testTrace(foo);
%OptimizeFunctionOnNextCall(foo);
testTrace(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7554360^!)  
---   

## **regress file:regress-number-is-hole-nan.js**  
   
**[Commit: [turbofan] Fix NumberIsHoleNaN to check the upper word.](https://chromium.googlesource.com/v8/v8/+/496aecb)**  
  
Date(Commit): Mon May 30 11:48:07 2016  
Code Review: [https://codereview.chromium.org/2022753002](https://codereview.chromium.org/2022753002)  
Regress: [mjsunit/compiler/regress-number-is-hole-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-number-is-hole-nan.js)  
```javascript
var a = [, 2.121736758e-314];

function foo() { return a[1]; }

assertEquals(2.121736758e-314, foo());
assertEquals(2.121736758e-314, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(2.121736758e-314, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/496aecb^!)  
---   

## **regress file:regress-recurse-patch-binary-op.js**  
   
**[Commit: Check the state of the current binary op IC before patching smi code](https://chromium.googlesource.com/v8/v8/+/f1cc6e6)**  
  
Date(Commit): Wed Apr 27 09:19:15 2016  
Code Review: [https://codereview.chromium.org/1925583002](https://codereview.chromium.org/1925583002)  
Regress: [mjsunit/regress/regress-recurse-patch-binary-op.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-recurse-patch-binary-op.js)  
```javascript
var i = 0
function valueOf() {
  while (true) return i++ < 4 ? 1 + this : 2
}

1 + ({valueOf})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1cc6e6^!)  
---   

## **regress file:regress-object-assign-deprecated-2.js**  
   
**[Commit: MigrateInstance(target) before Object.assign(target, ...)](https://chromium.googlesource.com/v8/v8/+/1678bb5)**  
  
Date(Commit): Mon Apr 25 15:41:21 2016  
Code Review: [https://codereview.chromium.org/1905933002](https://codereview.chromium.org/1905933002)  
Regress: [mjsunit/regress/regress-object-assign-deprecated-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-object-assign-deprecated-2.js)  
```javascript
var x = {a:1, b:2};
Object.defineProperty(x, "c", {set(v) {}})
var y = {get c() { return {a:1, b:2.5} }};
Object.assign(x, y, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1678bb5^!)  
---   

## **regress file:regress-object-assign-deprecated.js**  
   
**[Commit: MigrateInstance(target) before Object.assign(target, ...)](https://chromium.googlesource.com/v8/v8/+/1678bb5)**  
  
Date(Commit): Mon Apr 25 15:41:21 2016  
Code Review: [https://codereview.chromium.org/1905933002](https://codereview.chromium.org/1905933002)  
Regress: [mjsunit/regress/regress-object-assign-deprecated.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-object-assign-deprecated.js)  
```javascript
var x = {a:1, b:2};
var y = {a:1, b:2.5};
Object.assign(x, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1678bb5^!)  
---   

## **regress file:regress-opt-typeof-null.js**  
   
**[Commit: Fix 'typeof null' canonicalization in crankshaft](https://chromium.googlesource.com/v8/v8/+/7dfb5be)**  
  
Date(Commit): Thu Apr 21 11:24:31 2016  
Code Review: [https://codereview.chromium.org/1912553002](https://codereview.chromium.org/1912553002)  
Regress: [mjsunit/regress/regress-opt-typeof-null.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-opt-typeof-null.js)  
```javascript
function f() {
  return typeof null === "object";
};

%OptimizeFunctionOnNextCall(f);
assertTrue(f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7dfb5be^!)  
---   

## **regress file:regress-dead-throw-inlining.js**  
   
**[Commit: [turbofan] Reducers should revisit end after merging to it.](https://chromium.googlesource.com/v8/v8/+/52f2dbc)**  
  
Date(Commit): Fri Feb 05 11:01:44 2016  
Code Review: [https://codereview.chromium.org/1675433003](https://codereview.chromium.org/1675433003)  
Regress: [mjsunit/compiler/regress-dead-throw-inlining.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-dead-throw-inlining.js)  
```javascript
function g() { if (false) throw 0; }
function f() { g(); }

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52f2dbc^!)  
---   

## **regress file:regress-integer-indexed-element.js**  
   
**[Commit: [runtime] Fix integer indexed property handling](https://chromium.googlesource.com/v8/v8/+/621bdd6)**  
  
Date(Commit): Tue Feb 02 17:02:23 2016  
Code Review: [https://codereview.chromium.org/1651913005](https://codereview.chromium.org/1651913005)  
Regress: [mjsunit/regress/regress-integer-indexed-element.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-integer-indexed-element.js)  
```javascript
var o = {__proto__:new Int32Array(100)};
Object.prototype[1.3] = 10;
assertEquals(undefined, o[1.3]);

var o = new Int32Array(100);
var o2 = new Int32Array(200);
o.__proto__ = o2;
assertEquals(undefined, Reflect.get(o, 1.3, o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/621bdd6^!)  
---   

## **regress file:regress-new-target-context.js**  
   
**[Commit: [fullcode] Switch passing of new.target to register.](https://chromium.googlesource.com/v8/v8/+/440a42b)**  
  
Date(Commit): Thu Dec 03 10:04:35 2015  
Code Review: [https://codereview.chromium.org/1492793002](https://codereview.chromium.org/1492793002)  
Regress: [mjsunit/es6/regress/regress-new-target-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-new-target-context.js)  
```javascript
function makeFun(n) {
  var source = "(function f" + n + "() { ";
  for (var i = 0; i < n; ++i) source += "var v" + i + "; ";
  source += "(function() { 0 ";
  for (var i = 0; i < n; ++i) source += "+ v" + i + " ";
  source += "})(); return { value: new.target }; })";
  return eval(source);
}


var a = makeFun(4);
assertEquals(a, new a().value);
assertEquals(undefined, a().value);


var b = makeFun(128);
assertEquals(b, new b().value);
assertEquals(undefined, b().value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/440a42b^!)  
---   

## **regress file:regress-ensure-initial-map.js**  
   
**[Commit: Don't EnsureHasInitialMap on non-constructors.](https://chromium.googlesource.com/v8/v8/+/9bee675)**  
  
Date(Commit): Wed Dec 02 10:39:46 2015  
Code Review: [https://codereview.chromium.org/1490003003](https://codereview.chromium.org/1490003003)  
Regress: [mjsunit/regress/regress-ensure-initial-map.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-ensure-initial-map.js)  
```javascript
var x = Object.getOwnPropertyDescriptor({get x() {}}, "x").get;
function f(o, b) {
  if (b) {
    return o instanceof x;
  }
}

%OptimizeFunctionOnNextCall(f);
f();

function g() {
  return new x();
}

%OptimizeFunctionOnNextCall(g);
assertThrows(()=>g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bee675^!)  
---   

## **regress file:regress-inlined-new-target.js**  
   
**[Commit: [crankshaft] Prevent inlining of new.target functions.](https://chromium.googlesource.com/v8/v8/+/8c793fe)**  
  
Date(Commit): Tue Dec 01 14:19:43 2015  
Code Review: [https://codereview.chromium.org/1484163003](https://codereview.chromium.org/1484163003)  
Regress: [mjsunit/es6/regress/regress-inlined-new-target.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-inlined-new-target.js)  
```javascript
function g() { return { val: new.target }; }
function f() { return (new g()).val; }

assertEquals(g, f());
assertEquals(g, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(g, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c793fe^!)  
---   

## **regress file:regress-inline-arrow-as-construct.js**  
   
**[Commit: Install ConstructNonConstructable as construct stub for non-constructables.](https://chromium.googlesource.com/v8/v8/+/8e28e85)**  
  
Date(Commit): Tue Nov 24 17:17:00 2015  
Code Review: [https://codereview.chromium.org/1467473002](https://codereview.chromium.org/1467473002)  
Regress: [mjsunit/regress/regress-inline-arrow-as-construct.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-arrow-as-construct.js)  
```javascript
var g = () => {}

function f() {
  return new g();
}

assertThrows(f);
assertThrows(f);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8e28e85^!)  
---   

## **regress file:regress-deopt-in-array-literal-spread.js**  
   
**[Commit: [turbofan] Fix deoptimization from array literal spread.](https://chromium.googlesource.com/v8/v8/+/279f2aa)**  
  
Date(Commit): Wed Nov 18 11:45:41 2015  
Code Review: [https://codereview.chromium.org/1455953002](https://codereview.chromium.org/1455953002)  
Regress: [mjsunit/regress/regress-deopt-in-array-literal-spread.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-in-array-literal-spread.js)  
```javascript
function f(a,b,c,d) { return [a, ...(%DeoptimizeNow(), [b,c]), d]; }

assertEquals([1,2,3,4], f(1,2,3,4));
assertEquals([1,2,3,4], f(1,2,3,4));
%OptimizeFunctionOnNextCall(f);
assertEquals([1,2,3,4], f(1,2,3,4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/279f2aa^!)  
---   

## **regress file:regress-f64-w32-change.js**  
   
**[Commit: [turbofan] Only infer signedness for Float64->Word32 representation change from the input type.](https://chromium.googlesource.com/v8/v8/+/a9fa049)**  
  
Date(Commit): Wed Nov 18 10:02:33 2015  
Code Review: [https://codereview.chromium.org/1455103002](https://codereview.chromium.org/1455103002)  
Regress: [mjsunit/compiler/regress-f64-w32-change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-f64-w32-change.js)  
```javascript
var f = (function () {
  "use asm";
  var f64use = 0;
  function f(x, b) {
    x = x|0;
    b = b >>> 0;
    var f64 = x ? -1 : b;
    f64use = f64 + 0.5;
    var w32 = x ? 1 : f64;
    return (w32 + 1)|0;
  }

  return f;
})();

%OptimizeFunctionOnNextCall(f);
assertEquals(0, f(0, -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9fa049^!)  
---   

## **regress file:regress-arguments-slice.js**  
   
**[Commit: Fix Array.prototype.slice with arguments object with negative length.](https://chromium.googlesource.com/v8/v8/+/2ebd5fc)**  
  
Date(Commit): Wed Nov 11 11:50:38 2015  
Code Review: [https://codereview.chromium.org/1436813002](https://codereview.chromium.org/1436813002)  
Regress: [mjsunit/regress/regress-arguments-slice.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arguments-slice.js)  
```javascript
function f() { return arguments; }
var o = f();
o.length = -100;
Array.prototype.slice.call(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ebd5fc^!)  
---   

## **regress file:regress-inline-class-constructor.js**  
   
**[Commit: Ensure we never inline class constructors in Crankshaft, as it currently is entirely unsupported.](https://chromium.googlesource.com/v8/v8/+/f464f12)**  
  
Date(Commit): Thu Oct 22 14:39:07 2015  
Code Review: [https://codereview.chromium.org/1415723005](https://codereview.chromium.org/1415723005)  
Regress: [mjsunit/regress/regress-inline-class-constructor.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-class-constructor.js)  
```javascript
"use strict";

var B = class extends Int32Array { }

function f(b) {
  if (b) {
    null instanceof B;
  }
}

f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();

function f2() {
  return new B();
}

%OptimizeFunctionOnNextCall(f2);
f2();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f464f12^!)  
---   

## **regress file:regress-variable-liveness-let.js**  
   
**[Commit: [turbofan] Fix liveness analysis for let variable in TDZ.](https://chromium.googlesource.com/v8/v8/+/d9a5add)**  
  
Date(Commit): Wed Oct 21 12:23:06 2015  
Code Review: [https://codereview.chromium.org/1420573002](https://codereview.chromium.org/1420573002)  
Regress: [mjsunit/compiler/regress-variable-liveness-let.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-variable-liveness-let.js)  
```javascript
"use strict";

function f() {
  %DeoptimizeNow();
  let x = 23;
}

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9a5add^!)  
---   

## **regress file:regress-arm64-spillslots.js**  
   
**[Commit: [arm64] Fix jssp based spill slot accesses in Crankshaft](https://chromium.googlesource.com/v8/v8/+/102e3e8)**  
  
Date(Commit): Thu Oct 15 13:34:15 2015  
Code Review: [https://codereview.chromium.org/1401703003](https://codereview.chromium.org/1401703003)  
Regress: [mjsunit/regress/regress-arm64-spillslots.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arm64-spillslots.js)  
```javascript
"use strict";

function Message(message) {
  this.message = message;
}

function Inlined(input) {
  var dummy = arguments[1] === undefined;
  if (input instanceof Message) {
    return input;
  }
  print("unreachable, but we must create register allocation complexity");
  return [];
}

function Process(input) {
  var ret = [];
  ret.push(Inlined(input[0], 1, 2));
  return ret;
}

var input = [new Message("TEST PASS")];

Process(input);
Process(input);
%OptimizeFunctionOnNextCall(Process);
var result = Process(input);
assertEquals("TEST PASS", result[0].message);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/102e3e8^!)  
---   

## **regress file:regress-osr-context.js**  
   
**[Commit: [turbofan] Fix bogus materialization from frame with OSR.](https://chromium.googlesource.com/v8/v8/+/b8ecd94)**  
  
Date(Commit): Mon Jul 06 03:40:29 2015  
Code Review: [https://codereview.chromium.org/1220013003](https://codereview.chromium.org/1220013003)  
Regress: [mjsunit/regress/regress-osr-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-osr-context.js)  
```javascript
(function() {
  "use strict";
  var a = 23;
  function f() {
    for (let i = 0; i < 5; ++i) {
      a--;  
      function g() { return i }  
      if (i == 2) %OptimizeOsr();
    }
    return a;
  }
  assertEquals(18, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8ecd94^!)  
---   

## **regress file:regress-shift-right-logical.js**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-right-logical.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-right-logical.js)  
```javascript
(function ShiftRightLogicalWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 >>> tmp1;
  }

  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();


(function ShiftRightLogicalWithCallUsage() {
  var f = (function() {
    "use asm"
    
    

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 >>> tmp1, tmp1);
    }

    return f;
  })();

  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
---   

## **regress file:regress-shift-right.js**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-right.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-right.js)  
```javascript
(function ShiftRightWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 >> tmp1;
  }

  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();


(function ShiftRightWithCallUsage() {
  var f = (function() {
    "use asm"
    
    

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 >> tmp1, tmp1);
    }

    return f;
  })();

  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
---   

## **regress file:regress-shift-left.js**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-left.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-left.js)  
```javascript
(function ShiftLeftWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 << tmp1;
  }

  %OptimizeFunctionOnNextCall(f);
  assertEquals(512, f());
})();


(function ShiftLeftWithCallUsage() {
  var f = (function() {
    "use asm"
    
    

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 << tmp1, tmp1);
    }

    return f;
  })();

  %OptimizeFunctionOnNextCall(f);
  assertEquals(512, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
---   

## **regress file:regress-existing-shared-function-info.js**  
   
**[Commit: Reland 2 "Keep a canonical list of shared function infos."](https://chromium.googlesource.com/v8/v8/+/6434ec3)**  
  
Date(Commit): Thu Jun 25 12:20:06 2015  
Code Review: [https://codereview.chromium.org/1211803002](https://codereview.chromium.org/1211803002)  
Regress: [mjsunit/regress/regress-existing-shared-function-info.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-existing-shared-function-info.js)  
```javascript
function f() {
  return function g() {
    return function h() {}
  }
}

var h = f()();


for (var i of Array(10)) gc();

f()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6434ec3^!)  
---   

## **regress file:regress-arrow-duplicate-params.js**  
   
**[Commit: Support rest parameters in arrow functions](https://chromium.googlesource.com/v8/v8/+/e7a62c2)**  
  
Date(Commit): Wed Jun 10 16:11:31 2015  
Code Review: [https://codereview.chromium.org/1178523002](https://codereview.chromium.org/1178523002)  
Regress: [mjsunit/es6/regress/regress-arrow-duplicate-params.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-arrow-duplicate-params.js)  
```javascript
assertThrows("(x, x, y) => 10;", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e7a62c2^!)  
---   

## **regress file:regress-eval-context.js**  
   
**[Commit: [turbofan] Fix one mean typo in kResolvePossiblyDirectEval.](https://chromium.googlesource.com/v8/v8/+/f45f24d)**  
  
Date(Commit): Tue Jun 09 17:14:52 2015  
Code Review: [https://codereview.chromium.org/1169853006](https://codereview.chromium.org/1169853006)  
Regress: [mjsunit/regress/regress-eval-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-eval-context.js)  
```javascript
(function() {
  'use strict';
  var x = 0;

  {
    let x = 1;
    assertEquals(1, eval("x"));
  }

  {
    let y = 2;
    assertEquals(0, eval("x"));
  }

  assertEquals(0, eval("x"));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f45f24d^!)  
---   

## **regress file:regress-variable-liveness.js**  
   
**[Commit: [turbofan] Fix variable liveness control structure creation.](https://chromium.googlesource.com/v8/v8/+/d2ca18d)**  
  
Date(Commit): Thu May 21 09:57:11 2015  
Code Review: [https://codereview.chromium.org/1148133002](https://codereview.chromium.org/1148133002)  
Regress: [mjsunit/compiler/regress-variable-liveness.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-variable-liveness.js)  
```javascript
function foo(x) {
  %DeoptimizeFunction(run);
  return x;
}

function run() {
  var line = new Array(2);
  for (var n = 3; n > 0; n = n - 1) {
    if (n < foo(line.length)) line = new Array(n);
    line[0] = n;
  }
}

assertEquals(void 0, run());
%OptimizeFunctionOnNextCall(run);
assertEquals(void 0, run());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2ca18d^!)  
---   

## **regress file:regress-smi-scanning.js**  
   
**[Commit: Fix smi scanning](https://chromium.googlesource.com/v8/v8/+/f21ea06)**  
  
Date(Commit): Mon May 04 15:02:30 2015  
Code Review: [https://codereview.chromium.org/1114073003](https://codereview.chromium.org/1114073003)  
Regress: [mjsunit/regress/regress-smi-scanning.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-scanning.js)  
```javascript
x = 2
3;
assertEquals(2, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f21ea06^!)  
---   

## **regress file:regress-indirect-push-unchecked.js**  
   
**[Commit: Fix indirect push](https://chromium.googlesource.com/v8/v8/+/434b456)**  
  
Date(Commit): Mon Apr 13 16:25:33 2015  
Code Review: [https://codereview.chromium.org/1087463003](https://codereview.chromium.org/1087463003)  
Regress: [mjsunit/regress/regress-indirect-push-unchecked.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-indirect-push-unchecked.js)  
```javascript
var a = [1.5];

function p() {
  Array.prototype.push.call(a, 1.7);
}

p();
p();
p();
%OptimizeFunctionOnNextCall(p);
p();
a.push({});
p();
assertEquals(1.7, a[a.length - 1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/434b456^!)  
---   

## **regress file:regress-typedarray-length.js**  
   
**[Commit: Fix speedup of typedarray-length loading in the ICs as well as Crankshaft](https://chromium.googlesource.com/v8/v8/+/87eef73)**  
  
Date(Commit): Mon Mar 30 11:50:23 2015  
Code Review: [https://codereview.chromium.org/1034393002](https://codereview.chromium.org/1034393002)  
Regress: [mjsunit/regress/regress-typedarray-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-typedarray-length.js)  
```javascript
var a = new Int32Array(100);
a.__proto__ = null;

function get(a) {
  return a.length;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteLength;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteOffset;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

(function() {
  "use strict";

  class MyTypedArray extends Int32Array {
    get length() {
      return "length";
    }
  }

  a = new MyTypedArray();

  get = function(a) {
    return a.length;
  }

  assertEquals("length", get(a));
  assertEquals("length", get(a));
  assertEquals("length", get(a));
  %OptimizeFunctionOnNextCall(get);
  assertEquals("length", get(a));

  a.__proto__ = null;

  get = function(a) {
    return a.length;
  }

  assertEquals(undefined, get(a));
  assertEquals(undefined, get(a));
  assertEquals(undefined, get(a));
  %OptimizeFunctionOnNextCall(get);
  assertEquals(undefined, get(a));
})();

(function() {
  "use strict";

  class MyTypedArray extends Int32Array {
    constructor(length) {
      super(length);
    }
  }

  a = new MyTypedArray(1024);

  get = function(a) {
    return a.length;
  }

  assertEquals(1024, get(a));
  assertEquals(1024, get(a));
  assertEquals(1024, get(a));
  %OptimizeFunctionOnNextCall(get);
  assertEquals(1024, get(a));
})();

(function() {
  "use strict";
  var a = new Uint8Array(4);
  Object.defineProperty(a, "length", {get: function() { return "blah"; }});
  get = function(a) {
    return a.length;
  }

  assertEquals("blah", get(a));
  assertEquals("blah", get(a));
  assertEquals("blah", get(a));
  %OptimizeFunctionOnNextCall(get);
  assertEquals("blah", get(a));
})();


assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("length"));
assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("byteOffset"));
assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("byteLength"));
assertTrue(delete Int32Array.prototype.__proto__.length);
assertTrue(delete Int32Array.prototype.__proto__.byteOffset);
assertTrue(delete Int32Array.prototype.__proto__.byteLength);

a = new Int32Array(100);

get = function(a) {
  return a.length;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteLength;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteOffset;
}

assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87eef73^!)  
---   

## **regress file:regress-filter-contexts.js**  
   
**[Commit: Properly handle non-JSFunction constructors in CanRetainOtherContext](https://chromium.googlesource.com/v8/v8/+/1b16678)**  
  
Date(Commit): Mon Mar 23 19:24:58 2015  
Code Review: [https://codereview.chromium.org/1017263003](https://codereview.chromium.org/1017263003)  
Regress: [mjsunit/regress/regress-filter-contexts.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-filter-contexts.js)  
```javascript
function f() { return f.x; }
f.__proto__ = null;
f.prototype = "";

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1b16678^!)  
---   

## **regress file:regress-bce-underflow.js**  
   
**[Commit: Ensure we don't overflow in BCE](https://chromium.googlesource.com/v8/v8/+/0f57346)**  
  
Date(Commit): Fri Mar 20 16:43:05 2015  
Code Review: [https://codereview.chromium.org/1023123003](https://codereview.chromium.org/1023123003)  
Regress: [mjsunit/regress/regress-bce-underflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-bce-underflow.js)  
```javascript
function f(a, i, bool) {
  var result;
  if (bool) {
    
    
    result = f2(a, 0x7fffffff, i, i, -0x80000000);
  } else {
    result = f2(a, -3, 4, i, 0);
  }
  return result;
}

function f2(a, c, x, i, d) {
  return a[x + c] + a[x - 0] + a[i - d];
}


var a = [];
var i = 0;
a.push(i++);
a.push(i++);
a.push(i++);
a.push(i++);
a.push(i++);
f(a, 0, false);
f(a, 0, false);
f(a, 0, false);
%OptimizeFunctionOnNextCall(f);
%DebugPrint(f(a, -0x7fffffff, true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f57346^!)  
---   

## **regress file:regress-arg-materialize-store.js**  
   
**[Commit: During arguments materialization, do not store materialized objects without lazy deopt.](https://chromium.googlesource.com/v8/v8/+/0a4047a)**  
  
Date(Commit): Tue Feb 17 15:24:34 2015  
Code Review: [https://codereview.chromium.org/919173003](https://codereview.chromium.org/919173003)  
Regress: [mjsunit/regress/regress-arg-materialize-store.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arg-materialize-store.js)  
```javascript
function f() {
  return f.arguments;
}

function g(deopt) {
  var o = { x : 2 };
  f();
  o.x = 1;
  deopt + 0;
  return o.x;
}

g(0);
g(0);
%OptimizeFunctionOnNextCall(g);
assertEquals(1, g({}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a4047a^!)  
---   

## **regress file:regress-to-number-binop-deopt.js**  
   
**[Commit: [turbofan] Avoid ToNumber conversions if they could deoptimize.](https://chromium.googlesource.com/v8/v8/+/0b8063c)**  
  
Date(Commit): Mon Feb 16 12:59:20 2015  
Code Review: [https://codereview.chromium.org/922623002](https://codereview.chromium.org/922623002)  
Regress: [mjsunit/compiler/regress-to-number-binop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-to-number-binop-deopt.js)  
```javascript
function deopt(f) {
  return { valueOf : function() { %DeoptimizeFunction(f); return 1.1; } };
}

function or_zero(o) {
  return o|0;
}

function multiply_one(o) {
  return +o;
}

function multiply_one_symbol() {
  return +Symbol();
}

assertThrows(multiply_one_symbol, TypeError);
assertEquals(1, or_zero(deopt(or_zero)));
assertEquals(1.1, multiply_one(deopt(multiply_one)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b8063c^!)  
---   

## **regress file:regress-typedarray-out-of-bounds.js**  
   
**[Commit: fix transition of typedarrays in ics](https://chromium.googlesource.com/v8/v8/+/9896fab)**  
  
Date(Commit): Mon Feb 09 09:50:15 2015  
Code Review: [https://codereview.chromium.org/904423002](https://codereview.chromium.org/904423002)  
Regress: [mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js)  
```javascript
var a = new Int32Array(10);
function f(a) { a["-1"] = 15; }
for (var i = 0; i < 3; i++) {
  f(a);
}
assertEquals(undefined, a[-1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9896fab^!)  
---   

## **regress file:regress-deoptimize-constant-keyed-load.js**  
   
**[Commit: Always emit bailout id for inlining property access (even for keyed access).](https://chromium.googlesource.com/v8/v8/+/da90aab)**  
  
Date(Commit): Fri Jan 30 14:35:43 2015  
Code Review: [https://codereview.chromium.org/887023003](https://codereview.chromium.org/887023003)  
Regress: [mjsunit/regress/regress-deoptimize-constant-keyed-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deoptimize-constant-keyed-load.js)  
```javascript
var o = { };
o.__defineGetter__("progressChanged", function() { %DeoptimizeFunction(f); return 10; })

function g(a, b, c) {
  return a + b + c;
}

function f() {
  var t="progressChanged";
  return g(1, o[t], 100);
}

f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(111, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da90aab^!)  
---   

## **regress file:regress-undefined-nan3.js**  
   
**[Commit: Double field values need sNaN -> qNaN canonicalization.](https://chromium.googlesource.com/v8/v8/+/0381acf)**  
  
Date(Commit): Thu Jan 22 08:36:12 2015  
Code Review: [https://codereview.chromium.org/863153002](https://codereview.chromium.org/863153002)  
Regress: [mjsunit/regress/regress-undefined-nan3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan3.js)  
```javascript
var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper()
i_view[1] = %GetHoleNaNLower();
var f_view = new Float64Array(ab);

var fixed_double_elements = new Float64Array(1);
fixed_double_elements[0] = f_view[0];

function A(src) { this.x = src[0]; }

new A(fixed_double_elements);
new A(fixed_double_elements);

%OptimizeFunctionOnNextCall(A);

var obj = new A(fixed_double_elements);

function move_x(dst, obj) { dst[0] = obj.x; }

var doubles = [0.5];
move_x(doubles, obj);
move_x(doubles, obj);
%OptimizeFunctionOnNextCall(move_x);
move_x(doubles, obj);
assertTrue(doubles[0] !== undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0381acf^!)  
---   

## **regress file:regress-undefined-nan2.js**  
   
**[Commit: [arm] Fix sNaN quietening in the ARM simulator on IA-32.](https://chromium.googlesource.com/v8/v8/+/ee86227)**  
  
Date(Commit): Wed Jan 21 13:01:23 2015  
Code Review: [https://codereview.chromium.org/802243004](https://codereview.chromium.org/802243004)  
Regress: [mjsunit/regress/regress-undefined-nan2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan2.js)  
```javascript
function foo(a, i) {
  var o = [0.5,,1];
  a[i] = o[i];
}
var a1 = [0.1,0.1];
foo(a1, 0);
foo(a1, 1);
assertEquals(undefined, a1[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee86227^!)  
---   

## **regress file:regress-undefined-nan.js**  
   
**[Commit: Use signaling NaN for holes in fixed double arrays.](https://chromium.googlesource.com/v8/v8/+/9eace97)**  
  
Date(Commit): Wed Jan 21 08:52:25 2015  
Code Review: [https://codereview.chromium.org/863633002](https://codereview.chromium.org/863633002)  
Regress: [mjsunit/regress/regress-undefined-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan.js)  
```javascript
function loader(dst, src, i) {
  dst[i] = src[i];
}

var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper()
i_view[1] = %GetHoleNaNLower();
var f_view = new Float64Array(ab);

var fixed_double_elements = new Float64Array(1);

function opt_store() { fixed_double_elements[0] = f_view[0]; }

opt_store();
opt_store();
%OptimizeFunctionOnNextCall(opt_store);
opt_store();

var i32 = new Int32Array(fixed_double_elements.buffer);
assertEquals(i_view[0], i32[0]);
assertEquals(i_view[1], i32[1]);

var doubles = [0.5];
loader(doubles, fixed_double_elements, 0);
loader(doubles, fixed_double_elements, 0);
%OptimizeFunctionOnNextCall(loader);
loader(doubles, fixed_double_elements, 0);
assertTrue(doubles[0] !== undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9eace97^!)  
---   

## **regress file:regress-bit-number-constant.js**  
   
**[Commit: [turbofan] Fix bit representation of NumberConstant.](https://chromium.googlesource.com/v8/v8/+/d1c1a3c)**  
  
Date(Commit): Wed Jan 07 15:44:22 2015  
Code Review: [https://codereview.chromium.org/839813002](https://codereview.chromium.org/839813002)  
Regress: [mjsunit/compiler/regress-bit-number-constant.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-bit-number-constant.js)  
```javascript
var stdlib = this;
var buffer = new ArrayBuffer(64 * 1024);
var foreign = {}

var foo = (function Module(stdlib, foreign, heap) {
  "use asm";
  function foo(i) {
    return !(i ? 1 : false);
  }
  return {foo:foo};
})(stdlib, foreign, buffer).foo;

assertFalse(foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d1c1a3c^!)  
---   

## **regress file:regress-ntl-effect.js**  
   
**[Commit: Do not reduce effect phis for loops.](https://chromium.googlesource.com/v8/v8/+/bdf446f)**  
  
Date(Commit): Sat Jan 03 12:46:00 2015  
Code Review: [https://codereview.chromium.org/830923002](https://codereview.chromium.org/830923002)  
Regress: [mjsunit/compiler/regress-ntl-effect.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-ntl-effect.js)  
```javascript
function g() {
  throw 0;
}

function f() {
  g();
  while (1) {}
}

assertThrows(function () { f(); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bdf446f^!)  
---   

## **regress file:regress-int32array-outofbounds-nan.js**  
   
**[Commit: [turbofan] Quickfix for invalid number truncation of typed array loads.](https://chromium.googlesource.com/v8/v8/+/14409ab)**  
  
Date(Commit): Fri Dec 12 10:45:38 2014  
Code Review: [https://codereview.chromium.org/803483002](https://codereview.chromium.org/803483002)  
Regress: [mjsunit/compiler/regress-int32array-outofbounds-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-int32array-outofbounds-nan.js)  
```javascript
function Module(stdlib, foreign, heap) {
  "use asm";
  var MEM32 = new stdlib.Int32Array(heap);
  function foo(i) {
    i = i|0;
    return +MEM32[i >> 2];
  }
  return {foo: foo};
}

var foo = Module(this, {}, new ArrayBuffer(4)).foo;
assertEquals(NaN, foo(-4));
assertEquals(NaN, foo(4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14409ab^!)  
---   

## **regress file:regress-ntl.js**  
   
**[Commit: [turbofan] Fix control reducer bug with NTLs.](https://chromium.googlesource.com/v8/v8/+/aeda76c)**  
  
Date(Commit): Tue Dec 09 15:09:59 2014  
Code Review: [https://codereview.chromium.org/789773002](https://codereview.chromium.org/789773002)  
Regress: [mjsunit/regress/regress-ntl.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-ntl.js)  
```javascript
function mod1() {
  var v_1 = 1;
  var v_2 = 1;
  v_1++;
  v_2 = {valueOf: function() { throw "gagh"; }};

  function bug1() {
    for (var i = 0; i < 1; v_2++) {
      if (v_1 == 1) ;
    }
  }

  return bug1;
}

var f = mod1();
assertThrows(f);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);


var v_3 = 1;
var v_4 = 1;
v_3++;
v_4 = {valueOf: function() { throw "gagh"; }};

function bug2() {
  for (var i = 0; i < 1; v_4++) {
    if (v_3 == 1) ;
  }
}

assertThrows(bug2);
%OptimizeFunctionOnNextCall(bug2);
assertThrows(bug2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aeda76c^!)  
---   

## **regress file:regress-lea-matching.js**  
   
**[Commit: [turbofan] Fix matching of the lea instruction.](https://chromium.googlesource.com/v8/v8/+/d9cabb9)**  
  
Date(Commit): Mon Nov 24 17:45:33 2014  
Code Review: [https://codereview.chromium.org/756643002](https://codereview.chromium.org/756643002)  
Regress: [mjsunit/regress/regress-lea-matching.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lea-matching.js)  
```javascript
function f(a, b, c) {
  a = a|0;
  b = b|0;
  c = c|0;
  var r = 0;
  r = a + ((b << 1) + c) | 0;
  return r|0;
}

assertEquals(8, f(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9cabb9^!)  
---   

## **regress file:regress-unsigned-mul-add.js**  
   
**[Commit: [turbofan] Remove int32 narrowing during typed lowering.](https://chromium.googlesource.com/v8/v8/+/c3af691)**  
  
Date(Commit): Mon Nov 17 09:04:52 2014  
Code Review: [https://codereview.chromium.org/721723004](https://codereview.chromium.org/721723004)  
Regress: [mjsunit/regress/regress-unsigned-mul-add.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-unsigned-mul-add.js)  
```javascript
function f(a) {
  var x = a >>> 0;
  return (x * 1.0 + x * 1.0) << 0;
}

assertEquals(-2, f(-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3af691^!)  
---   

## **regress file:regress-weakening-multiplication.js**  
   
**[Commit: [turbofan] Weakening of types must weaken ranges inside unions.](https://chromium.googlesource.com/v8/v8/+/4c1f4b7)**  
  
Date(Commit): Thu Nov 13 05:31:47 2014  
Code Review: [https://codereview.chromium.org/712623002](https://codereview.chromium.org/712623002)  
Regress: [mjsunit/regress/regress-weakening-multiplication.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-weakening-multiplication.js)  
```javascript
function f() {
  for (var j = 1; j < 1; j *= -8) {
  }
  for (var i = 1; i < 1; j += 2) {
    j * -1;
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c1f4b7^!)  
---   

## **regress file:regress-register-allocator3.js**  
   
**[Commit: [turbofan] phis cannot take registers as inputs](https://chromium.googlesource.com/v8/v8/+/a350f0d)**  
  
Date(Commit): Thu Nov 06 12:56:44 2014  
Code Review: [https://codereview.chromium.org/704153002](https://codereview.chromium.org/704153002)  
Regress: [mjsunit/compiler/regress-register-allocator3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator3.js)  
```javascript
function Module() {
  "use asm";
  function f() {
   var $0 = 0, $25 = 0, $i$014$i = 0, $sum$013$i = 0, $v_0$01$i = 0, $v_1$02$i = 0, $v_10$011$i = 0, $v_11$012$i = 0, $v_2$03$i = 0, $v_3$04$i = 0, $v_4$05$i = 0, $v_5$06$i = 0, $v_6$07$i = 0, $v_7$08$i = 0, $v_8$09$i = 0, $v_9$010$i = 0;
   $i$014$i = 0;
   $sum$013$i = 0;
   $v_0$01$i = 8;
   $v_1$02$i = 9;
   $v_10$011$i = 18;
   $v_11$012$i = 19;
   $v_2$03$i = 10;
   $v_3$04$i = 11;
   $v_4$05$i = 12;
   $v_5$06$i = 13;
   $v_6$07$i = 14;
   $v_7$08$i = 15;
   $v_8$09$i = 16;
   $v_9$010$i = 17;
   do {
    $v_0$01$i = $v_3$04$i + $v_9$010$i + $v_0$01$i | 0;
    $v_1$02$i = $v_4$05$i + $v_10$011$i + $v_1$02$i | 0;
    $v_2$03$i = $v_5$06$i + $v_11$012$i + $v_2$03$i | 0;
    $v_3$04$i = $v_3$04$i + $v_6$07$i + $v_0$01$i | 0;
    $v_4$05$i = $v_4$05$i + $v_7$08$i + $v_1$02$i | 0;
    $v_5$06$i = $v_5$06$i + $v_8$09$i + $v_2$03$i | 0;
    $v_6$07$i = $v_6$07$i + $v_9$010$i + $v_3$04$i | 0;
    $v_7$08$i = $v_7$08$i + $v_10$011$i + $v_4$05$i | 0;
    $v_8$09$i = $v_8$09$i + $v_11$012$i + $v_5$06$i | 0;
    $v_9$010$i = $v_0$01$i + $v_9$010$i + $v_6$07$i | 0;
    $v_10$011$i = $v_1$02$i + $v_10$011$i + $v_7$08$i | 0;
    $v_11$012$i = $v_2$03$i + $v_11$012$i + $v_8$09$i | 0;
    $25 = $v_0$01$i + $v_1$02$i | 0;
    $sum$013$i = $v_2$03$i + $sum$013$i + $v_5$06$i + $v_4$05$i + $v_8$09$i + $v_3$04$i + $25 + $v_7$08$i + $v_11$012$i + $v_6$07$i + $v_10$011$i + $v_9$010$i | 0;
    $i$014$i = $i$014$i + 1 | 0;
   } while (($i$014$i | 0) <= 0);
   return $sum$013$i - ($v_5$06$i + $v_2$03$i + $v_4$05$i + $v_8$09$i + $25 + $v_3$04$i + $v_7$08$i + $v_11$012$i + $v_6$07$i + $v_10$011$i + $v_9$010$i);
  }
  return { f: f };
}

Module().f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a350f0d^!)  
---   

## **regress file:regress-assignment-in-test-context.js**  
   
**[Commit: [turbofan] Fix deopt for assignments in non-effect context.](https://chromium.googlesource.com/v8/v8/+/91eeae5)**  
  
Date(Commit): Wed Nov 05 13:09:14 2014  
Code Review: [https://codereview.chromium.org/701853002](https://codereview.chromium.org/701853002)  
Regress: [mjsunit/regress/regress-assignment-in-test-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-assignment-in-test-context.js)  
```javascript
function assertEquals() {}

function f(o) {
  if (o.setterProperty = 0) {
    return 1;
  }
  return 2;
}

function deopt() { %DeoptimizeFunction(f); }

assertEquals(2,
             f(Object.defineProperty({}, "setterProperty", { set: deopt })));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91eeae5^!)  
---   

## **regress file:regress-register-allocator2.js**  
   
**[Commit: [x86] Fix register constraints for multiply high and modulus.](https://chromium.googlesource.com/v8/v8/+/017c518)**  
  
Date(Commit): Mon Nov 03 06:28:12 2014  
Code Review: [https://codereview.chromium.org/697053002](https://codereview.chromium.org/697053002)  
Regress: [mjsunit/compiler/regress-register-allocator2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator2.js)  
```javascript
function f() {
  var x = 0;
  var y = 0;
  x ^= undefined;
  assertEquals(x /= 1);
  assertEquals(NaN, y %= 1);
  assertEquals(y = 1);
  f();
  y = -2;
  assertEquals(x >>= 1);
  assertEquals(0, ((y+(y+(y+((y^(x%5))+y)))+(y+y))>>y)+y);
}
try { f(); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/017c518^!)  
---   

## **regress file:regress-eval-cache.js**  
   
**[Commit: Use shared function info for eval cache key.](https://chromium.googlesource.com/v8/v8/+/0dfbf83)**  
  
Date(Commit): Tue Oct 28 10:01:44 2014  
Code Review: [https://codereview.chromium.org/678843004](https://codereview.chromium.org/678843004)  
Regress: [mjsunit/regress/regress-eval-cache.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-eval-cache.js)  
```javascript
(function f() {
  try {
    throw 1;
  } catch (e) {
    var a = 0;
    var b = 0;
    var c = 0;
    var x = 1;
    var result = eval('eval("x")').toString();
    assertEquals("1", result);
  }
  var x = 2;
  var result = eval('eval("x")').toString();
  assertEquals("2", result);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0dfbf83^!)  
---   

## **regress file:regress-shift-enumerable.js**  
   
**[Commit: Widen definition of %HasComplexElements() to include non-enumerability](https://chromium.googlesource.com/v8/v8/+/02d37b8)**  
  
Date(Commit): Fri Oct 24 18:04:13 2014  
Code Review: [https://codereview.chromium.org/672323003](https://codereview.chromium.org/672323003)  
Regress: [mjsunit/regress/regress-shift-enumerable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-shift-enumerable.js)  
```javascript
var arr = [1, 2];
Object.defineProperty(arr, 0xfffe, {
  value: 3,
  configurable: true,
  writable: true,
  enumerable: false
});
arr[0xffff] = 4;
arr.shift();
var desc = Object.getOwnPropertyDescriptor(arr, 0xfffe);
assertEquals(4, desc.value);
assertFalse(desc.enumerable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/02d37b8^!)  
---   

## **regress file:regress-register-allocator.js**  
   
**[Commit: [x86] Fix register constraints for multiply-high.](https://chromium.googlesource.com/v8/v8/+/548fb46)**  
  
Date(Commit): Fri Oct 24 09:36:40 2014  
Code Review: [https://codereview.chromium.org/671393002](https://codereview.chromium.org/671393002)  
Regress: [mjsunit/compiler/regress-register-allocator.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator.js)  
```javascript
function Module(stdlib, foreign, buffer) {
  "use asm";
  var HEAP32 = new stdlib.Int32Array(buffer);
  function g(a) {
    HEAP32[a] = 9982 * 100;
    return a;
  }
  function f(i1) {
    i1 = i1 | 0;
    var i2 = HEAP32[i1 >> 2] | 0;
    g(i1);
    L2909: {
      L2: {
        if (0) {
          if (0) break L2;
          g(i2);
          break L2909;
        }
      }
      var r = (HEAP32[1] | 0) / 100 | 0;
      g(r);
      return r;
    }
  }
  return {f: f};
}

var f = Module(this, {}, new ArrayBuffer(64 * 1024)).f;
assertEquals(9982, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/548fb46^!)  
---   

## **regress file:regress-json-parse-index.js**  
   
**[Commit: Fix escaped index JSON parsing](https://chromium.googlesource.com/v8/v8/+/83f64e8)**  
  
Date(Commit): Mon Sep 22 15:21:19 2014  
Code Review: [https://codereview.chromium.org/592813002](https://codereview.chromium.org/592813002)  
Regress: [mjsunit/regress/regress-json-parse-index.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-json-parse-index.js)  
```javascript
var o = JSON.parse('{"\\u0030":100}');
assertEquals(100, o[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/83f64e8^!)  
---   

## **regress file:regress-reset-dictionary-elements.js**  
   
**[Commit: Allocate a new empty number dictionary when resetting elements](https://chromium.googlesource.com/v8/v8/+/1dddf69)**  
  
Date(Commit): Fri Sep 05 11:38:22 2014  
Code Review: [https://codereview.chromium.org/545773003](https://codereview.chromium.org/545773003)  
Regress: [mjsunit/regress/regress-reset-dictionary-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-reset-dictionary-elements.js)  
```javascript
var a = [];
a[10000] = 1;
a.length = 0;
a[1] = 1;
a.length = 0;
assertEquals(undefined, a[1]);

var o = {};
Object.freeze(o);
assertEquals(undefined, o[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1dddf69^!)  
---   

## **regress file:regress-inline-constant-load.js**  
   
**[Commit: Fix loading non-configurable non-writable value from a constant with mismatching type feedback](https://chromium.googlesource.com/v8/v8/+/03b0237)**  
  
Date(Commit): Wed Sep 03 12:13:46 2014  
Code Review: [https://codereview.chromium.org/534093003](https://codereview.chromium.org/534093003)  
Regress: [mjsunit/regress/regress-inline-constant-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-constant-load.js)  
```javascript
var o1 = {};
var o2 = {};

function foo(x) {
  return x.bar;
}

Object.defineProperty(o1, "bar", {value:200});
foo(o1);
foo(o1);

function f(b) {
  var o = o2;
  if (b) { return foo(o) }
}

f(false);
%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f(false));
Object.defineProperty(o2, "bar", {value: 100});
assertEquals(100, f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/03b0237^!)  
---   

## **regress file:regress-force-constant-representation.js**  
   
**[Commit: Fixed inlining of constant values](https://chromium.googlesource.com/v8/v8/+/2a37ab7)**  
  
Date(Commit): Tue Aug 26 11:34:25 2014  
Code Review: [https://codereview.chromium.org/507613002](https://codereview.chromium.org/507613002)  
Regress: [mjsunit/regress/regress-force-constant-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-force-constant-representation.js)  
```javascript
var a = [{}];
function f(a) {
  a.push(Infinity);
}

f(a);
f(a);
f(a);
%OptimizeFunctionOnNextCall(f);
f(a);
assertEquals([{}, Infinity, Infinity, Infinity, Infinity], a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a37ab7^!)  
---   

## **regress file:regress-update-field-type-attributes.js**  
   
**[Commit: Fix Object.freeze with field type tracking.](https://chromium.googlesource.com/v8/v8/+/f08d269)**  
  
Date(Commit): Tue Jul 29 13:30:29 2014  
Code Review: [https://codereview.chromium.org/424093002](https://codereview.chromium.org/424093002)  
Regress: [mjsunit/regress/regress-update-field-type-attributes.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-update-field-type-attributes.js)  
```javascript
function test(){
  function InnerClass(){}
  var container = {field: new InnerClass()};
  return Object.freeze(container);
};

assertTrue(Object.isFrozen(test()));
assertTrue(Object.isFrozen(test()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f08d269^!)  
---   

## **regress file:regress-grow-deopt.js**  
   
**[Commit: In GrowMode, force the value to the right representation to avoid deopts between storing the length and storing the value.](https://chromium.googlesource.com/v8/v8/+/60df9da)**  
  
Date(Commit): Fri Jul 25 11:48:25 2014  
Code Review: [https://codereview.chromium.org/419683004](https://codereview.chromium.org/419683004)  
Regress: [mjsunit/regress/regress-grow-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-grow-deopt.js)  
```javascript
function f(a, v) {
  a[a.length] = v;
}

var a = [1.4];
f(a, 1);
f(a, 2);
%OptimizeFunctionOnNextCall(f);
f(a, {});
assertEquals(4, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/60df9da^!)  
---   

## **regress file:regress-mask-array-length.js**  
   
**[Commit: Fix ArrayLengthSetter to not throw on non-extensible receivers.](https://chromium.googlesource.com/v8/v8/+/6798779)**  
  
Date(Commit): Wed Jul 23 20:27:32 2014  
Code Review: [https://codereview.chromium.org/411983003](https://codereview.chromium.org/411983003)  
Regress: [mjsunit/regress/regress-mask-array-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mask-array-length.js)  
```javascript
var a = [];
var o = {
  __proto__: a
};
Object.preventExtensions(o);
o.length = 'abc';  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6798779^!)  
---   

## **regress file:regress-regexp-nocase.js**  
   
**[Commit: ARM64: always restore regexp register cache after a C function call.](https://chromium.googlesource.com/v8/v8/+/56ec59b)**  
  
Date(Commit): Thu Jul 17 09:55:48 2014  
Code Review: [https://codereview.chromium.org/392403002](https://codereview.chromium.org/392403002)  
Regress: [mjsunit/regress/regress-regexp-nocase.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-regexp-nocase.js)  
```javascript
var s = "('')x\nx\uF670";

assertEquals(s.match(/\((').*\1\)/i), ["('')", "'"]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56ec59b^!)  
---   

## **regress file:regress-double-property.js**  
   
**[Commit: Fix computed properties on object literals with a double as propertyname.](https://chromium.googlesource.com/v8/v8/+/ad6202d)**  
  
Date(Commit): Mon Jul 07 17:08:54 2014  
Code Review: [https://codereview.chromium.org/371973002](https://codereview.chromium.org/371973002)  
Regress: [mjsunit/regress/regress-double-property.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-double-property.js)  
```javascript
function f(a) {
  return {0.1: a};
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad6202d^!)  
---   

## **regress file:regress-sliced-external-cons-regexp.js**  
   
**[Commit: Turn old space cons strings into regular external strings (not short).](https://chromium.googlesource.com/v8/v8/+/6574f33)**  
  
Date(Commit): Thu Jul 03 11:46:31 2014  
Code Review: [https://codereview.chromium.org/368223002](https://codereview.chromium.org/368223002)  
Regress: [mjsunit/regress/regress-sliced-external-cons-regexp.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sliced-external-cons-regexp.js)  
```javascript
var re = /(B)/;
var cons1 = "0123456789" + "ABCDEFGHIJ";
var cons2 = "0123456789\u1234" + "ABCDEFGHIJ";
gc();
gc();  

try { externalizeString(cons1, false); } catch (e) { }
try { externalizeString(cons2, true); } catch (e) { }

var slice1 = cons1.slice(1,-1);
var slice2 = cons2.slice(1,-1);
for (var i = 0; i < 10; i++) {
  assertEquals(["B", "B"], re.exec(slice1));
  assertEquals(["B", "B"], re.exec(slice2));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6574f33^!)  
---   

## **regress file:regress-gvn-ftt.js**  
   
**[Commit: GVN fix, preventing loads hoisting above stores to the same field when HObjectAccess's representation is not the same.](https://chromium.googlesource.com/v8/v8/+/41e9d91)**  
  
Date(Commit): Fri Jun 13 07:51:45 2014  
Code Review: [https://codereview.chromium.org/331493006](https://codereview.chromium.org/331493006)  
Regress: [mjsunit/regress/regress-gvn-ftt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-gvn-ftt.js)  
```javascript
function A(id) {
  this.id = id;
}

var a1 = new A(1);
var a2 = new A(2);


var g;
function f(o, value) {
  g = o.o;
  o.o = value;
  return o.o;
}

var obj = {o: a1};

f(obj, a1);
f(obj, a1);
%OptimizeFunctionOnNextCall(f);
assertEquals(a2.id, f(obj, a2).id);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/41e9d91^!)  
---   

## **regress file:regress-set-flags-stress-compact.js**  
   
**[Commit: Fix %SetFlags("--stress-compaction")](https://chromium.googlesource.com/v8/v8/+/c3cd2f0)**  
  
Date(Commit): Mon May 12 10:39:08 2014  
Code Review: [https://codereview.chromium.org/261253006](https://codereview.chromium.org/261253006)  
Regress: [mjsunit/regress/regress-set-flags-stress-compact.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-set-flags-stress-compact.js)  
```javascript
var a = [];
for (var i = 0; i < 10000; i++) { a[i * 100] = 0; }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3cd2f0^!)  
---   

## **regress file:regress-builtinbust-7.js**  
   
**[Commit: Bugfix: internationalization routines fail on monkeypatching.](https://chromium.googlesource.com/v8/v8/+/0c3e70a)**  
  
Date(Commit): Wed Apr 30 07:36:12 2014  
Code Review: [https://codereview.chromium.org/253903003](https://codereview.chromium.org/253903003)  
Regress: [mjsunit/regress/regress-builtinbust-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-7.js)  
```javascript
if ("Intl" in this) {
  function overflow() {
    return overflow() + 1;
  }
  Object.defineProperty = overflow;
  assertDoesNotThrow(function() { Intl.Collator.supportedLocalesOf("en"); });

  var date = new Date(Date.UTC(2004, 12, 25, 3, 0, 0));
  var options = {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric"
  };

  Object.apply = overflow;
  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE", options); });

  var options_incomplete = {};
  assertDoesNotThrow(function() {
    date.toLocaleDateString("de-DE", options_incomplete);
  });
  assertFalse(options_incomplete.hasOwnProperty("year"));

  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE", undefined); });
  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE"); });
  assertThrows(function() { date.toLocaleDateString("de-DE", null); }, TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c3e70a^!)  
---   

## **regress file:regress-escape-preserve-smi-representation.js**  
   
**[Commit: Preserve Smi representation of non-escaping fields.](https://chromium.googlesource.com/v8/v8/+/d557425)**  
  
Date(Commit): Fri Apr 25 11:29:02 2014  
Code Review: [https://codereview.chromium.org/251493004](https://codereview.chromium.org/251493004)  
Regress: [mjsunit/regress/regress-escape-preserve-smi-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-escape-preserve-smi-representation.js)  
```javascript
function deepEquals(a, b) {
  if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; }
  if (typeof a != typeof b) return false;
  if (typeof a == "number") return isNaN(a) && isNaN(b);
  if (typeof a !== "object" && typeof a !== "function") return false;
  if (objectClass === "RegExp") { return (a.toString() === b.toString()); }
  if (objectClass === "Function") return false;
  if (objectClass === "Array") {
    var elementsCount = 0;
    if (a.length != b.length) { return false; }
    for (var i = 0; i < a.length; i++) {
      if (!deepEquals(a[i], b[i])) return false;
    }
    return true;
  }
}


function __f_1(){
  var __v_0 = [];
  for(var i=0; i<2; i++){
    __v_0.push([])
    deepEquals(2, __v_0.length);
  }
}
__f_1();
%OptimizeFunctionOnNextCall(__f_1);
__f_1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d557425^!)  
---   

## **regress file:regress-lazy-deopt-inlining2.js**  
   
**[Commit: Don't adopt the AST id from previous if id is none, since previous may have mismatching expected stack height.](https://chromium.googlesource.com/v8/v8/+/d2179f2)**  
  
Date(Commit): Fri Apr 25 09:52:11 2014  
Code Review: [https://codereview.chromium.org/252583004](https://codereview.chromium.org/252583004)  
Regress: [mjsunit/regress/regress-lazy-deopt-inlining2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lazy-deopt-inlining2.js)  
```javascript
"use strict";
function f1(d) {
  return 1 + f2(1, f3(d), d);
}

function f2(v0, v1, v2) { return v1; }

function f3(d) {
  if (d) %DeoptimizeFunction(f1);
  return 2;
}

%NeverOptimizeFunction(f3);

f1(false);
f1(false);
%OptimizeFunctionOnNextCall(f1);
assertEquals(3, f1(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2179f2^!)  
---   

## **regress file:regress-lazy-deopt-inlining.js**  
   
**[Commit: Mark the simulate before EnterInlined with BailoutId::None(), and set ReturnId on EnterInlined. When merging simulates into the simulate before enter-inlined, adopt the last AST id that gets merged into it.](https://chromium.googlesource.com/v8/v8/+/a55821e)**  
  
Date(Commit): Thu Apr 24 15:20:53 2014  
Code Review: [https://codereview.chromium.org/257583004](https://codereview.chromium.org/257583004)  
Regress: [mjsunit/regress/regress-lazy-deopt-inlining.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lazy-deopt-inlining.js)  
```javascript
"use strict";
function f1(d) {
  return 1 + f2(f3(d));
}

function f2(v) { return v; }

function f3(d) {
  if (d) %DeoptimizeFunction(f1);
  return 2;
}

%NeverOptimizeFunction(f3);

f1(false);
f1(false);
%OptimizeFunctionOnNextCall(f1);
assertEquals(3, f1(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a55821e^!)  
---   

## **regress file:regress-empty-fixed-double-array.js**  
   
**[Commit: Fix C++ type of Factory::NewFixedDoubleArray.](https://chromium.googlesource.com/v8/v8/+/8c57b45)**  
  
Date(Commit): Thu Apr 24 05:29:00 2014  
Code Review: [https://codereview.chromium.org/249593002](https://codereview.chromium.org/249593002)  
Regress: [mjsunit/regress/regress-empty-fixed-double-array.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-empty-fixed-double-array.js)  
```javascript
function f(a, x) {
  a.shift();
  a[0] = x;
}

f([1], 1.1);
f([1], 1.1);
%OptimizeFunctionOnNextCall(f);
f([1], 1.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c57b45^!)  
---   

## **regress file:regress-builtinbust-6.js**  
   
**[Commit: Fix ToObject and Object.isSealed in four Array builtins.](https://chromium.googlesource.com/v8/v8/+/66ec299)**  
  
Date(Commit): Wed Apr 23 12:48:32 2014  
Code Review: [https://codereview.chromium.org/240223006](https://codereview.chromium.org/240223006)  
Regress: [mjsunit/regress/regress-builtinbust-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-6.js)  
```javascript
var values = [ 23, 4.2, true, false, 0/0 ];
for (var i = 0; i < values.length; ++i) {
  var v = values[i];
  Array.prototype.join.call(v);
  Array.prototype.pop.call(v);
  Array.prototype.push.call(v);
  Array.prototype.reverse.call(v);
  Array.prototype.shift.call(v);
  Array.prototype.slice.call(v);
  Array.prototype.splice.call(v);
  Array.prototype.unshift.call(v);
}


var length_receiver, element_receiver;
function length() { length_receiver = this; return 2; }
function element() { element_receiver = this; return "x"; }
Object.defineProperty(Number.prototype, "length", { get:length, set:length });
Object.defineProperty(Number.prototype, "0", { get:element, set:element });
Object.defineProperty(Number.prototype, "1", { get:element, set:element });
Object.defineProperty(Number.prototype, "2", { get:element, set:element });
function test_receiver(expected, call_string) {
  assertDoesNotThrow(call_string);
  assertEquals(new Number(expected), length_receiver);
  assertSame(length_receiver, element_receiver);
}

test_receiver(11, "Array.prototype.join.call(11)")
test_receiver(23, "Array.prototype.pop.call(23)");
test_receiver(42, "Array.prototype.push.call(42, 'y')");
test_receiver(49, "Array.prototype.reverse.call(49)");
test_receiver(65, "Array.prototype.shift.call(65)");
test_receiver(77, "Array.prototype.slice.call(77, 1)");
test_receiver(88, "Array.prototype.splice.call(88, 1, 1)");
test_receiver(99, "Array.prototype.unshift.call(99, 'z')");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/66ec299^!)  
---   

## **regress file:regress-builtinbust-5.js**  
   
**[Commit: Fix bogus call to Object.hasOwnProperty in Array builtin.](https://chromium.googlesource.com/v8/v8/+/e51d646)**  
  
Date(Commit): Tue Apr 15 12:52:41 2014  
Code Review: [https://codereview.chromium.org/239033002](https://codereview.chromium.org/239033002)  
Regress: [mjsunit/regress/regress-builtinbust-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-5.js)  
```javascript
var a = [ 1, 2, 3 ];
var was_called = false;
function poison() { was_called = true; }
a.hasOwnProperty = poison;
Object.freeze(a);

assertThrows("a.unshift()", TypeError);
assertEquals(3, a.length);
assertFalse(was_called);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e51d646^!)  
---   

## **regress file:regress-builtinbust-4.js**  
   
**[Commit: Fix bogus Object.isSealed check in some Array builtins.](https://chromium.googlesource.com/v8/v8/+/39137c8)**  
  
Date(Commit): Tue Apr 15 08:25:42 2014  
Code Review: [https://codereview.chromium.org/237253002](https://codereview.chromium.org/237253002)  
Regress: [mjsunit/regress/regress-builtinbust-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-4.js)  
```javascript
var o = { __proto__:Array.prototype, 0:"x" };
function boomer() { return 0; }
Object.defineProperty(o, "length", { get:boomer, set:boomer });
Object.seal(o);

assertDoesNotThrow(function() { o.push(1); });
assertEquals(0, o.length);
assertEquals(1, o[0]);

assertDoesNotThrow(function() { o.unshift(2); });
assertEquals(0, o.length);
assertEquals(2, o[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39137c8^!)  
---   

## **regress file:regress-no-dummy-use-for-arguments-object.js**  
   
**[Commit: There is no definition for HArgumentsObject, so LDummyUse confuses the register allocator. I have recently made similar fix for HCapturedObject (see https://codereview.chromium.org/222283002/).](https://chromium.googlesource.com/v8/v8/+/fd98833)**  
  
Date(Commit): Fri Apr 11 06:29:51 2014  
Code Review: [https://codereview.chromium.org/222283002/](https://codereview.chromium.org/222283002/)  
Regress: [mjsunit/regress/regress-no-dummy-use-for-arguments-object.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-no-dummy-use-for-arguments-object.js)  
```javascript
function g() {
  arguments.length;
}

var global = "";

function f() {
  global.dummy = this;
  g({});
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd98833^!)  
---   

## **regress file:regress-parseint.js**  
   
**[Commit: Fix argument expectation Runtime_StringParseInt.](https://chromium.googlesource.com/v8/v8/+/4df132a)**  
  
Date(Commit): Wed Apr 09 12:33:51 2014  
Code Review: [https://codereview.chromium.org/230693002](https://codereview.chromium.org/230693002)  
Regress: [mjsunit/regress/regress-parseint.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-parseint.js)  
```javascript
function f(string, radix) {
  
  radix = (radix == 0) ? radix : (radix >> 0);
  if (radix != 2) return NaN;
  return %StringParseInt(string, radix);
}

assertEquals(2, (-4294967294) >> 0);
assertEquals(3, f("11", -4294967294));
assertEquals(NaN, f("11", -2147483650));
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f("11", -4294967294));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4df132a^!)  
---   

## **regress file:regress-builtinbust-3.js**  
   
**[Commit: Fix return value of push() and unshift() on Array.prototype.](https://chromium.googlesource.com/v8/v8/+/e3aec7a)**  
  
Date(Commit): Wed Apr 09 09:14:56 2014  
Code Review: [https://codereview.chromium.org/230453002](https://codereview.chromium.org/230453002)  
Regress: [mjsunit/regress/regress-builtinbust-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-3.js)  
```javascript
function produce_object() {
  var real_length = 1;
  function set_length() { real_length = "boom"; }
  function get_length() { return real_length; }
  var o = { __proto__:Array.prototype , 0:"x" };
  Object.defineProperty(o, "length", { set:set_length, get:get_length })
  return o;
}

assertEquals(2, produce_object().push("y"));
assertEquals(2, produce_object().unshift("y"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3aec7a^!)  
---   

## **regress file:regress-inline-getter-near-stack-limit.js**  
   
**[Commit: Add stack overflow check for inlined property getter](https://chromium.googlesource.com/v8/v8/+/05670b6)**  
  
Date(Commit): Wed Apr 09 07:35:12 2014  
Code Review: [https://codereview.chromium.org/220813003](https://codereview.chromium.org/220813003)  
Regress: [mjsunit/regress/regress-inline-getter-near-stack-limit.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-getter-near-stack-limit.js)  
```javascript
function runNearStackLimit(f) {
  function t() {
    try { t(); } catch(e) { f(); }
  };
  try { t(); } catch(e) {}
}

function g(x) { return x.bar; }
function f1() { }
function f2() { }

var x = Object.defineProperty({}, "bar", { get: f1 });
g(x);
g(x);
var y = Object.defineProperty({}, "bar", { get: f2 });
g(y);
%OptimizeFunctionOnNextCall(g);
runNearStackLimit(function() { g(y); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/05670b6^!)  
---   

## **regress file:regress-builtinbust-1.js**  
   
**[Commit: Use premordial Object.isSealed/isFrozen in builtins.](https://chromium.googlesource.com/v8/v8/+/775d9b0)**  
  
Date(Commit): Thu Apr 03 12:23:35 2014  
Code Review: [https://codereview.chromium.org/223473002](https://codereview.chromium.org/223473002)  
Regress: [mjsunit/regress/regress-builtinbust-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-1.js)  
```javascript
function nope() { return false; }
var a = [ 1, 2, 3 ];
Object.seal(a);
Object.isSealed = nope;

assertThrows(function() { a.pop(); }, TypeError);
assertThrows(function() { a.push(5); }, TypeError);
assertThrows(function() { a.shift(); }, TypeError);
assertThrows(function() { a.unshift(5); }, TypeError);
assertThrows(function() { a.splice(0, 1); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/775d9b0^!)  
---   

## **regress file:regress-freeze-setter.js**  
   
**[Commit: When freezing global object, go through the property cell](https://chromium.googlesource.com/v8/v8/+/fe37026)**  
  
Date(Commit): Thu Apr 03 10:43:56 2014  
Code Review: [https://codereview.chromium.org/223613002](https://codereview.chromium.org/223613002)  
Regress: [mjsunit/regress/regress-freeze-setter.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-freeze-setter.js)  
```javascript
Object.defineProperty(this, 'x', {set: function() { }});
Object.freeze(this);
eval('"use strict"; x = 20;');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe37026^!)  
---   

## **regress file:regress-captured-object-no-dummy-use.js**  
   
**[Commit: Do not generate LDummyUse instruction for HCapturedObject](https://chromium.googlesource.com/v8/v8/+/42d2d3c)**  
  
Date(Commit): Thu Apr 03 07:35:13 2014  
Code Review: [https://codereview.chromium.org/222283002](https://codereview.chromium.org/222283002)  
Regress: [mjsunit/regress/regress-captured-object-no-dummy-use.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-captured-object-no-dummy-use.js)  
```javascript
var global = "10.1";
function f() { }
function g(a) { this.d = a; }
function h() {
  var x = new f();
  global.dummy = this;
  var y = new g(x);
}
h();
h();
%OptimizeFunctionOnNextCall(h);
h();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/42d2d3c^!)  
---   

## **regress file:regress-alloc-smi-check.js**  
   
**[Commit: Check in Lithium that allocation size in Smi range.](https://chromium.googlesource.com/v8/v8/+/0b53ed2)**  
  
Date(Commit): Thu Apr 03 07:04:46 2014  
Code Review: [https://codereview.chromium.org/221743005](https://codereview.chromium.org/221743005)  
Regress: [mjsunit/regress/regress-alloc-smi-check.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-alloc-smi-check.js)  
```javascript
var x = {};

function f(a) {
  a[200000000] = x;
}

f(new Array(100000));
f([]);
%OptimizeFunctionOnNextCall(f);
f([]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b53ed2^!)  
---   

## **regress file:regress-load-field-by-index.js**  
   
**[Commit: Fix LoadFieldByIndex to take mutable heap-numbers into account.](https://chromium.googlesource.com/v8/v8/+/55a6318)**  
  
Date(Commit): Mon Mar 31 11:59:29 2014  
Code Review: [https://codereview.chromium.org/213213002](https://codereview.chromium.org/213213002)  
Regress: [mjsunit/regress/regress-load-field-by-index.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-load-field-by-index.js)  
```javascript
var o = {a:1.5, b:{}};

function f(o) {
  var result = [];
  for (var k in o) {
    result[result.length] = o[k];
  }
  return result;
}

f(o);
f(o);
%OptimizeFunctionOnNextCall(f);
var array = f(o);
o.a = 1.7;
assertEquals(1.5, array[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55a6318^!)  
---   

## **regress file:regress-enum-prop-keys-cache-size.js**  
   
**[Commit: With this fix, we only create the enum cache for own property descriptors (originally we cached all descriptors in the map).  The problem was that the size of all descriptors could be trimmed during GC triggered by allocating the storage for the cache, so we could have ended up with a wrong storage size.](https://chromium.googlesource.com/v8/v8/+/4608bde)**  
  
Date(Commit): Thu Mar 27 15:33:06 2014  
Code Review: [https://codereview.chromium.org/212673011](https://codereview.chromium.org/212673011)  
Regress: [mjsunit/regress/regress-enum-prop-keys-cache-size.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-enum-prop-keys-cache-size.js)  
```javascript
%SetAllocationTimeout(100000, 100000);

var x = {};
x.a = 1;
x.b = 2;
x = {};

var y = {};
y.a = 1;

%SetAllocationTimeout(100000, 0);

for (var z in y) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4608bde^!)  
---   

## **regress file:regress-is-smi-repr.js**  
   
**[Commit: Fix missing representation for the result of HIsSmiAndBranch.](https://chromium.googlesource.com/v8/v8/+/10606aa)**  
  
Date(Commit): Wed Mar 26 13:14:08 2014  
Code Review: [https://codereview.chromium.org/211273010](https://codereview.chromium.org/211273010)  
Regress: [mjsunit/regress/regress-is-smi-repr.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-is-smi-repr.js)  
```javascript
"use strict";

var global;

function g() { global = this; }
Object.defineProperty(Number.prototype, "prop", { get: g });
function f(s) { s.prop; }

f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10606aa^!)  
---   

## **regress file:regress-dictionary-to-fast-arguments.js**  
   
**[Commit: Don't convert dictionary sloppy arguments to fast double mode.](https://chromium.googlesource.com/v8/v8/+/c432f71)**  
  
Date(Commit): Tue Mar 25 14:14:58 2014  
Code Review: [https://codereview.chromium.org/207683006](https://codereview.chromium.org/207683006)  
Regress: [mjsunit/regress/regress-dictionary-to-fast-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-dictionary-to-fast-arguments.js)  
```javascript
function f(a, b) {
  for (var i = 10000; i > 0; i--) {
    arguments[i] = 0;
  }
}

f(1.5, 2.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c432f71^!)  
---   

## **regress file:regress-store-heapobject.js**  
   
**[Commit: Ensure the constant operand for heap-object store-named-field is not a smi.](https://chromium.googlesource.com/v8/v8/+/e18e650)**  
  
Date(Commit): Mon Mar 24 16:25:48 2014  
Code Review: [https://codereview.chromium.org/210193002](https://codereview.chromium.org/210193002)  
Regress: [mjsunit/regress/regress-store-heapobject.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-heapobject.js)  
```javascript
var o = {a: undefined};

function store(o, v) {
  o.a = v;
}

store(o, undefined);
store(o, undefined);

function f(bool) {
  var o = {a: undefined};
  if (bool) {
    store(o, 1);
  }
  return o;
}

f(false);
f(false);
%OptimizeFunctionOnNextCall(f);
f(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e18e650^!)  
---   

## **regress file:regress-keyed-store-global.js**  
   
**[Commit: Don't generate keyed store ICs for global proxies.](https://chromium.googlesource.com/v8/v8/+/5aaa513)**  
  
Date(Commit): Mon Mar 17 17:19:39 2014  
Code Review: [https://codereview.chromium.org/197873025](https://codereview.chromium.org/197873025)  
Regress: [mjsunit/regress/regress-keyed-store-global.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-store-global.js)  
```javascript
function f(a) {
  for (var i = 0; i < 256; i++) a[i] = i;
}

f([]);
f([]);
f(this);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5aaa513^!)  
---   

## **regress file:regress-migrate-callbacks.js**  
   
**[Commit: Fix generalization with callbacks.](https://chromium.googlesource.com/v8/v8/+/0f2a324)**  
  
Date(Commit): Fri Mar 14 14:17:49 2014  
Code Review: [https://codereview.chromium.org/200173003](https://codereview.chromium.org/200173003)  
Regress: [mjsunit/regress/regress-migrate-callbacks.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-migrate-callbacks.js)  
```javascript
var o1 = {};
o1.x = 1
o1.y = 1.5
var o2 = {}
o2.x = 1.5;
o2.__defineSetter__('y', function(v) { });
o1.y;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f2a324^!)  
---   

## **regress file:regress-sort-arguments.js**  
   
**[Commit: Don't fast RemoveArrayHoles in case of arguments arrays.](https://chromium.googlesource.com/v8/v8/+/8735adb)**  
  
Date(Commit): Wed Mar 12 13:42:18 2014  
Code Review: [https://codereview.chromium.org/197043004](https://codereview.chromium.org/197043004)  
Regress: [mjsunit/regress/regress-sort-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sort-arguments.js)  
```javascript
function f(a) { return arguments; }
var a = f(1,2,3);
delete a[1];
Array.prototype.sort.apply(a);
a[10000000] = 4;
Array.prototype.sort.apply(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8735adb^!)  
---   

## **regress file:regress-store-global-proxy.js**  
   
**[Commit: Reland and fix "Allow ICs to be generated for own global proxy."](https://chromium.googlesource.com/v8/v8/+/1180803)**  
  
Date(Commit): Mon Mar 10 12:23:05 2014  
Code Review: [https://codereview.chromium.org/176793003](https://codereview.chromium.org/176793003)  
Regress: [mjsunit/regress/regress-store-global-proxy.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-global-proxy.js)  
```javascript
delete Object.prototype.__proto__;

function f() {
  this.toString = 1;
}

f.apply({});
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1180803^!)  
---   

## **regress file:regress-keyed-store-non-strict-arguments.js**  
   
**[Commit: Only use the non-strict-arguments-stub if the store site is non-strict.](https://chromium.googlesource.com/v8/v8/+/cd6f3ef)**  
  
Date(Commit): Thu Mar 06 12:19:06 2014  
Code Review: [https://codereview.chromium.org/176843018](https://codereview.chromium.org/176843018)  
Regress: [mjsunit/regress/regress-keyed-store-non-strict-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-store-non-strict-arguments.js)  
```javascript
function args(arg) { return arguments; }
var a = args(false);

(function () {
  "use strict";
  a["const" + 0] = 0;
})();

(function () {
  "use strict";
  a[0] = 0;
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd6f3ef^!)  
---   

## **regress file:regress-force-representation.js**  
   
**[Commit: Also delete force representations that have no uses.](https://chromium.googlesource.com/v8/v8/+/f913c3b)**  
  
Date(Commit): Thu Mar 06 09:47:27 2014  
Code Review: [https://codereview.chromium.org/187773002](https://codereview.chromium.org/187773002)  
Regress: [mjsunit/regress/regress-force-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-force-representation.js)  
```javascript
function optimize(crankshaft_test) {
  crankshaft_test();
  crankshaft_test();
  %OptimizeFunctionOnNextCall(crankshaft_test);
  crankshaft_test();
}

function f() {
  var v1 = 0;
  var v2 = -0;
  var t = v2++;
  v2++;
  return Math.max(v2++, v1++);
}

optimize(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f913c3b^!)  
---   

## **regress file:regress-sync-optimized-lists.js**  
   
**[Commit: Evict from optimized code map in sync with removing from optimized functions list.](https://chromium.googlesource.com/v8/v8/+/5c186bb)**  
  
Date(Commit): Fri Feb 28 12:27:31 2014  
Code Review: [https://codereview.chromium.org/184443002](https://codereview.chromium.org/184443002)  
Regress: [mjsunit/regress/regress-sync-optimized-lists.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sync-optimized-lists.js)  
```javascript
function Ctor() {
  this.a = 1;
}

function get_closure() {
  return function add_field(obj, osr) {
    obj.c = 3;
    var x = 0;
    if (osr) %OptimizeOsr();
    for (var i = 0; i < 10; i++) {
      x = i + 1;
    }
    return x;
  }
}

var f1 = get_closure();
f1(new Ctor(), false);
f1(new Ctor(), false);

%OptimizeFunctionOnNextCall(f1, "concurrent");


var o = new Ctor();
f1(o, true);


%NotifyContextDisposed();


o.c = 2.2;

var f2 = get_closure();
f2(new Ctor(), true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c186bb^!)  
---   

## **regress file:regress-put-prototype-transition.js**  
   
**[Commit: Fix putting of prototype transitions. The length is also subject to GC, just like entry.](https://chromium.googlesource.com/v8/v8/+/aa14020)**  
  
Date(Commit): Thu Feb 27 16:07:44 2014  
Code Review: [https://codereview.chromium.org/183193003](https://codereview.chromium.org/183193003)  
Regress: [mjsunit/regress/regress-put-prototype-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-put-prototype-transition.js)  
```javascript
function deepEquals(a, b) { if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(a) && isNaN(b); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(a); if (objectClass !== classOf(b)) return false; if (objectClass === "RegExp") { return (a.toString() === b.toString()); } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (!deepEquals(a[i], b[i])) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf() !== b.valueOf()) return false; } return deepObjectEquals(a, b); }
assertSame = function assertSame(expected, found, name_opt) { if (found === expected) { if (expected !== 0 || (1 / expected) == (1 / found)) return; } else if ((expected !== expected) && (found !== found)) { return; } fail(PrettyPrint(expected), found, name_opt); }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected), found, name_opt); } };
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) { assertTrue(Math.abs(expected - found) <= delta, name_opt); }; assertArrayEquals = function assertArrayEquals(expected, found, name_opt) { var start = ""; if (name_opt) { start = name_opt + " - "; } assertEquals(expected.length, found.length, start + "array length"); if (expected.length == found.length) { for (var i = 0; i < expected.length; ++i) { assertEquals(expected[i], found[i], start + "array element at index " + i); } } };
assertTrue = function assertTrue(value, name_opt) { assertEquals(true, value, name_opt); };
assertFalse = function assertFalse(value, name_opt) { assertEquals(false, value, name_opt); };


var __v_0 = {};
var __v_1 = {};
function __f_3() { }
function __f_4(obj) {
  for (var __v_2 = 0; __v_2 < 26; __v_2++) {
    obj["__v_5" + __v_2] = 0;
  }
}
function __f_0(__v_1, __v_6) {
    (new __f_3()).__proto__ = __v_1;
}
%DebugPrint(undefined);
function __f_1(__v_4, add_first, __v_6, same_map_as) {
  var __v_1 = __v_4 ? new __f_3() : {};
  assertTrue(__v_4 || %HasFastProperties(__v_1));
  if (add_first) {
    __f_4(__v_1);
    assertFalse(%HasFastProperties(__v_1));
    __f_0(__v_1, __v_6);
    assertFalse(%HasFastProperties(__v_1));
  } else {
    __f_0(__v_1, __v_6);
    assertTrue(__v_4 || !%HasFastProperties(__v_1));
    __f_4(__v_1);
    assertTrue(__v_4 || !%HasFastProperties(__v_1));
  }
}
gc();
for (var __v_2 = 0; __v_2 < 4; __v_2++) {
  var __v_6 = ((__v_2 & 2) != 7);
  var __v_4 = ((__v_2 & 2) != 0);
  __f_1(__v_4, true, __v_6);
  var __v_0 = __f_1(__v_4, false, __v_6);
  __f_1(__v_4, false, __v_6, __v_0);
}
__v_5 = {a: 1, b: 2, c: 3};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa14020^!)  
---   

## **regress file:regress-fast-empty-string.js**  
   
**[Commit: Don't turn objects with empty-string properties into fast-mode.](https://chromium.googlesource.com/v8/v8/+/84b3665)**  
  
Date(Commit): Thu Feb 20 16:11:48 2014  
Code Review: [https://codereview.chromium.org/165743003](https://codereview.chromium.org/165743003)  
Regress: [mjsunit/regress/regress-fast-empty-string.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fast-empty-string.js)  
```javascript
var o = {};
o[""] = 1;
var x = {__proto__:o};
for (i = 0; i < 3; i++) {
  o[""];
}
for (i = 0; i < 3; i++) {
  assertEquals(undefined, o.x);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84b3665^!)  
---   

## **regress file:regress-lookup-transition.js**  
   
**[Commit: Directly store the transition target on LookupResult in TransitionResult.](https://chromium.googlesource.com/v8/v8/+/60c08a8)**  
  
Date(Commit): Tue Feb 18 12:19:32 2014  
Code Review: [https://codereview.chromium.org/170343003](https://codereview.chromium.org/170343003)  
Regress: [mjsunit/es6/regress/regress-lookup-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-lookup-transition.js)  
```javascript
var proxy = new Proxy({}, { getOwnPropertyDescriptor:function() {
  gc();
}});

function f() { this.x = 23; }
f.prototype = proxy;
new f();
new f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/60c08a8^!)  
---   

## **regress file:regress-check-eliminate-loop-phis.js**  
   
**[Commit: Don't propagate information through phis in loop headers.](https://chromium.googlesource.com/v8/v8/+/7b7e365)**  
  
Date(Commit): Wed Feb 12 18:30:41 2014  
Code Review: [https://codereview.chromium.org/147023005](https://codereview.chromium.org/147023005)  
Regress: [mjsunit/regress/regress-check-eliminate-loop-phis.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-check-eliminate-loop-phis.js)  
```javascript
function f() {
  var o = {x:1};
  var y = {y:2.5, x:0};
  var result;
  for (var i = 0; i < 2; i++) {
    result = o.x + 3;
    o = y;
  }
  return result;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b7e365^!)  
---   

## **regress file:regress-keyed-access-string-length.js**  
   
**[Commit: Fix regression caused by supporting inlining accesses to non-JSObjects](https://chromium.googlesource.com/v8/v8/+/bef13f7)**  
  
Date(Commit): Fri Jan 31 00:29:04 2014  
Code Review: [https://codereview.chromium.org/150983002](https://codereview.chromium.org/150983002)  
Regress: [mjsunit/regress/regress-keyed-access-string-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-access-string-length.js)  
```javascript
function f(i) {
  return "abc"[i];
}

f("length");
f("length");
%OptimizeFunctionOnNextCall(f);
f("length");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bef13f7^!)  
---   

## **regress file:regress-array-pop-deopt.js**  
   
**[Commit: Reland (and fix) "Add hydrogen support for ArrayPop, and remove the handwritten call stubs."](https://chromium.googlesource.com/v8/v8/+/f303303)**  
  
Date(Commit): Wed Jan 22 13:22:58 2014  
Code Review: [https://codereview.chromium.org/144913003](https://codereview.chromium.org/144913003)  
Regress: [mjsunit/regress/regress-array-pop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-array-pop-deopt.js)  
```javascript
var o = [6,7,8,9];

function f(b) {
  var v = o.pop() + b;
  return v;
}

assertEquals(10, f(1));
assertEquals(9, f(1));
assertEquals(8, f(1));
%OptimizeFunctionOnNextCall(f);
assertEquals("61", f("1"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f303303^!)  
---   

## **regress file:regress-is-contextual.js**  
   
**[Commit: Fix logic error in assert in IsUndeclaredGlobal()](https://chromium.googlesource.com/v8/v8/+/155ef10)**  
  
Date(Commit): Fri Jan 17 11:08:24 2014  
Code Review: [https://codereview.chromium.org/140943002](https://codereview.chromium.org/140943002)  
Regress: [mjsunit/regress/regress-is-contextual.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-is-contextual.js)  
```javascript
function foo(index) {
  return text.charAt(index);
}

var text = "hi there";
foo(0);
foo(0);
foo(100);     
text = false;


assertThrows(function () { foo(); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/155ef10^!)  
---   

## **regress file:regress-calls-with-migrating-prototypes.js**  
   
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
var o2 = {__proto__: new C1(f) }
function foo(o) {
 return o.f();
}
foo(o1);
foo(o1);
foo(o2);
foo(o1);
var o3 = new C1(function() { return 2; });
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo(o2));
o2.__proto__.f = function() { return 3; };
assertEquals(3, foo(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/48ff79a^!)  
---   

## **regress file:regress-param-local-type.js**  
   
**[Commit: Fix off-by-one error in AstTyper.](https://chromium.googlesource.com/v8/v8/+/5bc64b9)**  
  
Date(Commit): Wed Dec 11 11:34:09 2013  
Code Review: [https://codereview.chromium.org/105943007](https://codereview.chromium.org/105943007)  
Regress: [mjsunit/regress/regress-param-local-type.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-param-local-type.js)  
```javascript
function f(a) {  
  var s = '';    
  var n = 0;
  var i = 1;
  n = i + a;
}

f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);
assertOptimized(f);


function g() {  
  var s = '';   
  var n = 0;
  var i = 1;
  n = i + this;
}

g.call(1);
g.call(1);
%OptimizeFunctionOnNextCall(g);
g.call(1);
assertOptimized(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bc64b9^!)  
---   

## **regress file:regress-context-osr.js**  
   
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
---   

## **regress file:regress-clobbered-fp-regs.js**  
   
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
  var sum = (f1 + f2 + f3 + f4 + f5 + f6 + f7 + f8);
  assertEquals(1, y);
  var expected = 3.6;
  if (Math.abs(expected - sum) > 0.01) {
    assertEquals(expected, sum);
  }
}


store([1], 1, 1);
store([1], 1.1, 1);
store([1], 1.1, 1);
%OptimizeFunctionOnNextCall(store);

store([1], 1, 1)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21fb140^!)  
---   

## **regress file:regress-add-minus-zero.js**  
   
**[Commit: Do not remove HAdd with zero if the other operand is a double.](https://chromium.googlesource.com/v8/v8/+/3f1a833)**  
  
Date(Commit): Wed Oct 30 10:22:52 2013  
Code Review: [https://codereview.chromium.org/52173003](https://codereview.chromium.org/52173003)  
Regress: [mjsunit/regress/regress-add-minus-zero.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-add-minus-zero.js)  
```javascript
var o = { a: 0 };

function f(x) { return -o.a + 0; };

assertEquals("Infinity", String(1/f()));
assertEquals("Infinity", String(1/f()));
%OptimizeFunctionOnNextCall(f);
assertEquals("Infinity", String(1/f()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f1a833^!)  
---   

## **regress file:regress-compare-constant-doubles.js**  
   
**[Commit: ia32: Fix comparisons of two constant double operands when exactly one of them is in new space.](https://chromium.googlesource.com/v8/v8/+/9e88c23)**  
  
Date(Commit): Tue Oct 29 14:34:07 2013  
Code Review: [https://codereview.chromium.org/46883008](https://codereview.chromium.org/46883008)  
Regress: [mjsunit/regress/regress-compare-constant-doubles.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-compare-constant-doubles.js)  
```javascript
var left = 1.5;
var right;

var keepalive;

function foo() {
  
  var a1 = Math.sin(1) + 10;
  var a2 = a1 + 1;
  var a3 = a2 + 1;
  var a4 = a3 + 1;
  var a5 = a4 + 1;
  var a6 = a5 + 1;
  keepalive = [a1, a2, a3, a4, a5, a6];

  
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
---   

## **regress file:regress-array-pop-nonconfigurable.js**  
   
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
---   

## **regress file:regress-parse-use-strict.js**  
   
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
---   

## **regress file:regress-polymorphic-load.js**  
   
**[Commit: Only fold polymorphic into monomorphic load if all load from either receiver or same prototype.](https://chromium.googlesource.com/v8/v8/+/7e0ea6a)**  
  
Date(Commit): Wed Oct 02 13:24:08 2013  
Code Review: [https://chromiumcodereview.appspot.com/25718002](https://chromiumcodereview.appspot.com/25718002)  
Regress: [mjsunit/regress/regress-polymorphic-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-polymorphic-load.js)  
```javascript
function f(o) {
  return o.x;
}

var o1 = {x:1};
var o2 = {__proto__: {x:2}};

f(o2);
f(o2);
f(o2);
f(o1);
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f(o1));
assertEquals(2, f(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e0ea6a^!)  
---   

## **regress file:regress-binop.js**  
   
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
---   

## **regress file:regress-regexp-construct-result.js**  
   
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
---   

## **regress file:regress-store-uncacheable.js**  
   
**[Commit: Allow uncacheable identifiers to go generic.](https://chromium.googlesource.com/v8/v8/+/3f70c3b)**  
  
Date(Commit): Mon Sep 02 16:32:11 2013  
Code Review: [https://chromiumcodereview.appspot.com/23453019](https://chromiumcodereview.appspot.com/23453019)  
Regress: [mjsunit/regress/regress-store-uncacheable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-uncacheable.js)  
```javascript
function f() {
  var o = {};
  o["<abc>"] = 123;
}

f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();
assertOptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f70c3b^!)  
---   

## **regress file:regress-merge-descriptors.js**  
   
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
---   

## **regress file:regress-shared-deopt.js**  
   
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

  f4(9);
  f4(11);
  %OptimizeFunctionOnNextCall(f4);
  f4(12);

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
---   

## **regress file:regress-x87.js**  
   
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
---   

## **regress file:regress-convert-function-to-double.js**  
   
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
---   

## **regress file:regress-convert-hole2.js**  
   
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
---   

## **regress file:regress-phi-truncation.js**  
   
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
---   

## **regress file:regress-et-clobbers-doubles.js**  
   
**[Commit: Store doubles before calling into the elements transition stub on ARM](https://chromium.googlesource.com/v8/v8/+/145f240)**  
  
Date(Commit): Tue Aug 13 15:06:17 2013  
Code Review: [https://chromiumcodereview.appspot.com/22854011](https://chromiumcodereview.appspot.com/22854011)  
Regress: [mjsunit/regress/regress-et-clobbers-doubles.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-et-clobbers-doubles.js)  
```javascript
function t_smi(a) {
  a[0] = 1.5;
}

t_smi([1,,3]);
t_smi([1,,3]);
t_smi([1,,3]);
%OptimizeFunctionOnNextCall(t_smi);
var ta = [1,,3];
t_smi(ta);
assertEquals([1.5,,3], ta);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/145f240^!)  
---   

## **regress file:regress-map-invalidation-2.js**  
   
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
---   

## **regress file:regress-map-invalidation-1.js**  
   
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
---   

## **regress file:regress-hoist-load-named-field.js**  
   
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
---   

## **regress file:regress-smi-math-floor-round.js**  
   
**[Commit: Fix smi-based math floor.](https://chromium.googlesource.com/v8/v8/+/1965964)**  
  
Date(Commit): Fri Aug 09 11:21:03 2013  
Code Review: [https://chromiumcodereview.appspot.com/22623007](https://chromiumcodereview.appspot.com/22623007)  
Regress: [mjsunit/regress/regress-smi-math-floor-round.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-math-floor-round.js)  
```javascript
function f(o) {
  return Math.floor(o.x_smi) + 1;
}

assertEquals(2, f({x_smi:1}));
assertEquals(2, f({x_smi:1}));
%OptimizeFunctionOnNextCall(f);
assertEquals(2, f({x_smi:1}));

function f2(o) {
  return Math.floor(o.x_tagged) + 1;
}

var o = {x_tagged:{}};
o.x_tagged = 1.4;
assertEquals(2, f2(o));
assertEquals(2, f2(o));
%OptimizeFunctionOnNextCall(f2);
assertEquals(2, f2(o));

function f3(o) {
  return Math.round(o.x_smi) + 1;
}

assertEquals(2, f3({x_smi:1}));
assertEquals(2, f3({x_smi:1}));
%OptimizeFunctionOnNextCall(f3);
assertEquals(2, f3({x_smi:1}));

function f4(o) {
  return Math.round(o.x_tagged) + 1;
}

assertEquals(2, f4(o));
assertEquals(2, f4(o));
%OptimizeFunctionOnNextCall(f4);
assertEquals(2, f4(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1965964^!)  
---   

## **regress file:regress-freeze.js**  
   
**[Commit: Fix Object.freeze, Object.observe wrt CountOperation and CompoundAssignment.](https://chromium.googlesource.com/v8/v8/+/e5afd32)**  
  
Date(Commit): Wed Aug 07 18:45:41 2013  
Code Review: [https://chromiumcodereview.appspot.com/22562004](https://chromiumcodereview.appspot.com/22562004)  
Regress: [mjsunit/regress/regress-freeze.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-freeze.js)  
```javascript
function f(o) { o.x++ };
var o = {x: 5};
Object.freeze(o);
f(o);
f(o);
%OptimizeFunctionOnNextCall(f);
f(o);
assertEquals(5, o.x);


function f2(o) { o.x+=3 };
f2(o);
f2(o);
%OptimizeFunctionOnNextCall(f2);
f2(o);
assertEquals(5, o.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5afd32^!)  
---   

## **regress file:regress-omit-checks.js**  
   
**[Commit: Mark maps as unstable if their instances potentially transition away.](https://chromium.googlesource.com/v8/v8/+/2af164f)**  
  
Date(Commit): Tue Jul 30 16:33:58 2013  
Code Review: [https://chromiumcodereview.appspot.com/21095005](https://chromiumcodereview.appspot.com/21095005)  
Regress: [mjsunit/regress/regress-omit-checks.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-omit-checks.js)  
```javascript
var a = {x:1};
var a_deprecate = {x:1};
a_deprecate.x = 1.5;
function create() {
  return {__proto__:a, y:1};
}
var b1 = create();
var b2 = create();
var b3 = create();
var b4 = create();

function set(b) {
  b.x = 5;
  b.z = 10;
}

set(b1);
set(b2);
%OptimizeFunctionOnNextCall(set);
set(b3);
var called = false;
a.x = 1.5;
Object.defineProperty(a, "z", {set:function(v) { called = true; }});
set(b4);
assertTrue(called);
assertEquals(undefined, b4.z);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2af164f^!)  
---   

## **regress file:regress-deopt-store-effect.js**  
   
**[Commit: Fix deopt in store with effect context.](https://chromium.googlesource.com/v8/v8/+/88a4b0d)**  
  
Date(Commit): Fri Jul 19 13:45:26 2013  
Code Review: [https://chromiumcodereview.appspot.com/19693004](https://chromiumcodereview.appspot.com/19693004)  
Regress: [mjsunit/regress/regress-deopt-store-effect.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-store-effect.js)  
```javascript
var pro = { x : 1 }
var a = {}
a.__proto__ = pro
delete pro.x

function g(o) {
  return 7 + (o.z = 1, 20);
}

g(a);
g(a);
%OptimizeFunctionOnNextCall(g);
Object.defineProperty(pro, "z", {
    set: function(v) { %DeoptimizeFunction(g); },
    get: function() { return 20; }
});

assertEquals(27, g(a));



var i = { z : 2, r : 1 }
var j = { z : 2 }
var p = { a : 10 }
var pp = { a : 20, b : 1 }

function bar(o, p) {
  return 7 + (o.z = 1, p.a);
}

bar(i, p);
bar(i, p);
bar(j, p);
%OptimizeFunctionOnNextCall(bar);
assertEquals(27, bar(i, pp));



var i = { r : 1, z : 2 }
var j = { z : 2 }
var p = { a : 10 }
var pp = { a : 20, b : 1 }

function bar1(o, p) {
  return 7 + (o.z = 1, p.a);
}

bar1(i, p);
bar1(i, p);
bar1(j, p);
%OptimizeFunctionOnNextCall(bar1);
assertEquals(27, bar1(i, pp));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88a4b0d^!)  
---   

## **regress file:regress-mul-canoverflowb.js**  
   
**[Commit: Added %NeverOptimize runtime call that can disable optimizations for a method for tests.](https://chromium.googlesource.com/v8/v8/+/9e7819f)**  
  
Date(Commit): Thu Jul 11 14:17:56 2013  
Code Review: [https://codereview.chromium.org/18214005](https://codereview.chromium.org/18214005)  
Regress: [mjsunit/regress/regress-mul-canoverflowb.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mul-canoverflowb.js)  
```javascript
function boom(a) {
  return ((a | 0) * (a | 0)) | 0;
}
%NeverOptimizeFunction(boom_unoptimized);
function boom_unoptimized(a) {
  return ((a | 0) * (a | 0)) | 0;
}

boom(1, 1);
boom(2, 2);

%OptimizeFunctionOnNextCall(boom);
var big_int = 0x5F00000F;
var expected = boom_unoptimized(big_int);
var actual = boom(big_int)
assertEquals(expected, actual);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e7819f^!)  
---   

## **regress file:regress-deopt-gcb.js**  
   
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
---   

## **regress file:regress-embedded-cons-string.js**  
   
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
  
  %OptimizeFunctionOnNextCall(fun, "concurrent");
  
  fun();
  
  gc();
  
  assertUnoptimized(fun, "no sync");
  
  %UnblockConcurrentRecompilation();
  
  assertOptimized(fun, "sync");
  
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
---   

## **regress file:regress-polymorphic-store.js**  
   
**[Commit: Fix using monomorphic store instruction for polymorphic stores.](https://chromium.googlesource.com/v8/v8/+/2ca5c6c)**  
  
Date(Commit): Wed Jun 19 18:07:35 2013  
Code Review: [https://chromiumcodereview.appspot.com/16875008](https://chromiumcodereview.appspot.com/16875008)  
Regress: [mjsunit/regress/regress-polymorphic-store.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-polymorphic-store.js)  
```javascript
var o1 = {};
o1.f1 = function() { return 10; };
o1.x = 5;
o1.y = 2;
var o2 = {};
o2.x = 5;
o2.y = 5;

function store(o, v) {
  o.y = v;
}

store(o2, 0);
store(o1, 0);
store(o2, 0);
%OptimizeFunctionOnNextCall(store);
store(o1, 10);
assertEquals(5, o1.x);
assertEquals(10, o1.y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ca5c6c^!)  
---   

## **regress file:regress-function-length-strict.js**  
   
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
---   

## **regress file:regress-int32-truncation.js**  
   
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
}

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
}

f2(false);
f2(true);
%OptimizeFunctionOnNextCall(f2);
global = 2.5;
assertEquals(global, f2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3588aa4^!)  
---   

## **regress file:regress-convert-hole.js**  
   
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
---   

## **regress file:regress-copy-hole-to-field.js**  
   
**[Commit: Don't allow copying holes to fields.](https://chromium.googlesource.com/v8/v8/+/b353b1d)**  
  
Date(Commit): Wed May 22 15:33:53 2013  
Code Review: [https://chromiumcodereview.appspot.com/15745006](https://chromiumcodereview.appspot.com/15745006)  
Regress: [mjsunit/regress/regress-copy-hole-to-field.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-copy-hole-to-field.js)  
```javascript
var a = [1.5,,1.7];
var o = {a:1.8};

function f1(o,a,i) {
  o.a = a[i];
}

f1(o,a,0);
f1(o,a,0);
assertEquals(1.5, o.a);
%OptimizeFunctionOnNextCall(f1);
f1(o,a,1);
assertEquals(undefined, o.a);


var a = [1,,3];
var o = {ab:5};

function f2(o,a,i) {
  o.ab = a[i];
}

f2(o,a,0);
f2(o,a,0);
%OptimizeFunctionOnNextCall(f2);
f2(o,a,1);
assertEquals(undefined, o.ab);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b353b1d^!)  
---   

## **regress file:regress-mul-canoverflow.js**  
   
**[Commit: Fix overflow check in mul-i which was missing since r14322](https://chromium.googlesource.com/v8/v8/+/6288754)**  
  
Date(Commit): Thu Apr 25 07:36:59 2013  
Code Review: [https://codereview.chromium.org/14471012](https://codereview.chromium.org/14471012)  
Regress: [mjsunit/regress/regress-mul-canoverflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mul-canoverflow.js)  
```javascript
function boom(a) {
  return ((a | 0) * (a | 0)) | 0;
}
function boom_unoptimized(a) {
  try {} catch(_) {}
  return ((a | 0) * (a | 0)) | 0;
}

boom(1, 1);
boom(2, 2);

%OptimizeFunctionOnNextCall(boom);
var big_int = 0x5F00000F;
var expected = boom_unoptimized(big_int);
var actual = boom(big_int)
assertEquals(expected, actual);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6288754^!)  
---   

## **regress file:regress-grow-store-smi-check.js**  
   
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
        c = -306674912 ^ ((c >> 1) & 0x7fffffff);
      } else {
        c = (c >> 1) & 0x7fffffff;
      }
    }
    crc32[i] = c;
  }
}

var a = [0.5];
for (var i = 0; i < 256; ++i) a[i] = i;

test([0.5]);
test(a);
%OptimizeFunctionOnNextCall(test);
test(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/adf9afc^!)  
---   

## **regress file:regress-latin-1.js**  
   
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
---   

## **regress file:regress-delete-empty-double.js**  
   
**[Commit: Ensure reducing the length of an array doesn't make it go holey.](https://chromium.googlesource.com/v8/v8/+/14abf05)**  
  
Date(Commit): Fri Nov 02 10:24:56 2012  
Code Review: [https://chromiumcodereview.appspot.com/11358011](https://chromiumcodereview.appspot.com/11358011)  
Regress: [mjsunit/regress/regress-delete-empty-double.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-delete-empty-double.js)  
```javascript
a = [1.1,2.2,3.3];
a.length = 1;
delete a[1];

assertTrue(%HasDoubleElements(a));
assertFalse(%HasHoleyElements(a));

delete a[0];

assertTrue(%HasDoubleElements(a));
assertTrue(%HasHoleyElements(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14abf05^!)  
---   

## **regress file:regress-json-stringify-gc.js**  
   
**[Commit: Reland JSON.stringify reimplementation.](https://chromium.googlesource.com/v8/v8/+/e50ee08)**  
  
Date(Commit): Mon Oct 22 14:22:58 2012  
Code Review: [https://chromiumcodereview.appspot.com/11189112](https://chromiumcodereview.appspot.com/11189112)  
Regress: [mjsunit/regress/regress-json-stringify-gc.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-json-stringify-gc.js)  
```javascript
var a = [];
var new_space_string = "a";
for (var i = 0; i < 8; i++) {
  new_space_string += new_space_string;
}
for (var i = 0; i < 10000; i++) a.push(new_space_string);




json1 = JSON.stringify(a);
json2 = JSON.stringify(a);
assertTrue(json1 == json2, "GC caused JSON.stringify to fail.");


for (var i = 0; i < 10000; i++) {
  var s = i.toString();
  assertEquals('"' + s + '"', JSON.stringify(s, null, 0));
}

for (var i = 0; i < 10000; i++) {
  var s = i.toString() + "\u2603";
  assertEquals('"' + s + '"', JSON.stringify(s, null, 0));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e50ee08^!)  
---   

## **regress file:regress-builtin-array-op.js**  
   
**[Commit: Always invoke the default Array.sort functions from builtin functions.](https://chromium.googlesource.com/v8/v8/+/971e834)**  
  
Date(Commit): Thu Oct 18 11:18:08 2012  
Code Review: [https://chromiumcodereview.appspot.com/10559005](https://chromiumcodereview.appspot.com/10559005)  
Regress: [mjsunit/regress/regress-builtin-array-op.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtin-array-op.js)  
```javascript
var foo =  "hest";
Array.prototype.sort = function(fn) { foo = "fisk"; };
Function.prototype.call = function() { foo = "caramel"; };
var a = [2,3,1];
a[100000] = 0;
a.join();
assertEquals("hest", foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/971e834^!)  
---   

## **regress file:regress-convert-enum2.js**  
   
**[Commit: Invalidate the enum cache when converting a transition across which the descriptors are shared.](https://chromium.googlesource.com/v8/v8/+/7c28995)**  
  
Date(Commit): Mon Oct 15 08:38:51 2012  
Code Review: [https://chromiumcodereview.appspot.com/11145017](https://chromiumcodereview.appspot.com/11145017)  
Regress: [mjsunit/regress/regress-convert-enum2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-enum2.js)  
```javascript
var o = {};
o.a = 1;
o.b = function() { return 1; };
o.d = 2;

for (var x in o) { }

var o1 = {};
o1.a = 1;
o1.b = 10;
o1.c = 20;

var keys = ["a", "b", "c"];

var i = 0;
for (var y in o1) {
  assertEquals(keys[i], y);
  i += 1;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c28995^!)  
---   

## **regress file:regress-convert-enum.js**  
   
**[Commit: Don't clear EnumLength but rather copy the enum cache. Added regression test for crashes from chromecrash.](https://chromium.googlesource.com/v8/v8/+/b75705f)**  
  
Date(Commit): Thu Oct 11 15:33:34 2012  
Code Review: [https://chromiumcodereview.appspot.com/11103036](https://chromiumcodereview.appspot.com/11103036)  
Regress: [mjsunit/regress/regress-convert-enum.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-enum.js)  
```javascript
var o = {};
o.a = 1;
o.c = 2;



var o1 = {};
o1.a = 1;


for (var x in o1) { }
o1.b = function() { return 1; };


o = null;
gc();



var o2 = {};
o2.a = 1;
o2.b = 10;


var o3 = {};
o3.a = 1;

for (var y in o3) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b75705f^!)  
---   

## **regress file:regress-convert-transition.js**  
   
**[Commit: Fix transition conversion from CONSTANT_FUNCTION to FIELD.](https://chromium.googlesource.com/v8/v8/+/dde1cdf)**  
  
Date(Commit): Wed Oct 10 12:31:50 2012  
Code Review: [https://chromiumcodereview.appspot.com/11094044](https://chromiumcodereview.appspot.com/11094044)  
Regress: [mjsunit/regress/regress-convert-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-transition.js)  
```javascript
var input = '{ "a1":1, "a2":1, "a3":1, "a4":1, "a5":1, "a6":1, "a7":1,\
               "a8":1, "a9":1, "a10":1, "a11":1, "a12":1, "a13":1}';
var a = JSON.parse(input);
a.a = function() { return 10; };


var b = JSON.parse(input);
b.a = 10;


var c = JSON.parse(input);
c.x = 10;
assertEquals(undefined, c.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dde1cdf^!)  
---   

## **regress file:regress-cnlt-elements.js**  
   
**[Commit: Fix CNLT regression.](https://chromium.googlesource.com/v8/v8/+/55e924c)**  
  
Date(Commit): Wed Oct 10 12:29:44 2012  
Code Review: [https://chromiumcodereview.appspot.com/11017054](https://chromiumcodereview.appspot.com/11017054)  
Regress: [mjsunit/regress/regress-cnlt-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cnlt-elements.js)  
```javascript
var a = JSON.parse('{"b":1,"c":2,"d":3,"e":4}');
var b = JSON.parse('{"12040200":1, "a":2, "b":2}');
var c = JSON.parse('{"24050300":1}');
b = null;
gc();
gc();
c.a1 = 2;
c.a2 = 2;
c.a3 = 2;
c.a4 = 2;
c.a5 = 2;
c.a6 = 2;
c.a7 = 2;
c.a8 = 2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55e924c^!)  
---   

## **regress file:regress-cnlt-enum-indices.js**  
   
**[Commit: Fix CNLT for enum indices.](https://chromium.googlesource.com/v8/v8/+/083ee63)**  
  
Date(Commit): Thu Sep 20 15:18:00 2012  
Code Review: [https://chromiumcodereview.appspot.com/10958015](https://chromiumcodereview.appspot.com/10958015)  
Regress: [mjsunit/regress/regress-cnlt-enum-indices.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cnlt-enum-indices.js)  
```javascript
var o = {};
var o2 = {};

o.a = 1;
o2.a = 1;
function f() { return 10; }

Object.defineProperty(o, "b", { get: f, enumerable: true });
Object.defineProperty(o2, "b", { get: f, enumerable: true });
assertTrue(%HaveSameMap(o, o2));
o.c = 2;

for (var x in o) { }
o = null;

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/083ee63^!)  
---   

## **regress file:regress-undefined-store-keyed-fast-element.js**  
   
**[Commit: Deopt on storing undefined into double elements.](https://chromium.googlesource.com/v8/v8/+/ea31f86)**  
  
Date(Commit): Thu Sep 20 13:41:00 2012  
Code Review: [https://chromiumcodereview.appspot.com/10963010](https://chromiumcodereview.appspot.com/10963010)  
Regress: [mjsunit/regress/regress-undefined-store-keyed-fast-element.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-store-keyed-fast-element.js)  
```javascript
function f(v) {
  return [0.0, 0.1, 0.2, v];
}

assertEquals([0.0, 0.1, 0.2, NaN], f(NaN));
assertEquals([0.0, 0.1, 0.2, NaN], f(NaN));
%OptimizeFunctionOnNextCall(f);
assertEquals([0.0, 0.1, 0.2, undefined], f(undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea31f86^!)  
---   

## **regress file:regress-cntl-descriptors-enum.js**  
   
**[Commit: CNLT with descriptors but no valid enum fields has to clear the EnumCache.](https://chromium.googlesource.com/v8/v8/+/ad4746c)**  
  
Date(Commit): Fri Sep 14 13:15:43 2012  
Code Review: [https://chromiumcodereview.appspot.com/10928204](https://chromiumcodereview.appspot.com/10928204)  
Regress: [mjsunit/regress/regress-cntl-descriptors-enum.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cntl-descriptors-enum.js)  
```javascript
var o = {};
Object.defineProperty(o, "a", {
    value: 0, configurable: true, writable: true, enumerable: false
});

var o2 = {};
Object.defineProperty(o2, "a", {
    value: 0, configurable: true, writable: true, enumerable: false
});


assertTrue(%HaveSameMap(o, o2));

o.y = 2;

for (var v in o) { print(v); }
o = {};
gc();

for (var v in o2) { print(v); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad4746c^!)  
---   

## **regress file:regress-load-elements.js**  
   
**[Commit: Elements load depends on the type of the receiver.](https://chromium.googlesource.com/v8/v8/+/90db487)**  
  
Date(Commit): Thu Aug 30 17:31:32 2012  
Code Review: [https://chromiumcodereview.appspot.com/10918005](https://chromiumcodereview.appspot.com/10918005)  
Regress: [mjsunit/regress/regress-load-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-load-elements.js)  
```javascript
function bad_func(o,a) {
  for (var i = 0; i < 1; ++i) {
    o.prop = 0;
    var x = a[0];
  }
}

o = new Object();
a = {};
a[0] = 1;
bad_func(o, a);

o = new Object();
bad_func(o, a);




%OptimizeFunctionOnNextCall(bad_func);
bad_func(o, "");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90db487^!)  
---   

## **regress file:regress-deep-proto.js**  
   
**[Commit: Fix r11780 to avoid bugs where near branches are used to labels that are out of range.](https://chromium.googlesource.com/v8/v8/+/5eb4bae)**  
  
Date(Commit): Wed Jun 13 09:54:34 2012  
Code Review: [http://codereview.chromium.org/10542137](http://codereview.chromium.org/10542137)  
Regress: [mjsunit/regress/regress-deep-proto.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deep-proto.js)  
```javascript
function poly(x) {
  return x.foo;
}

var one = {foo: 0};
var two = {foo: 0, bar: 1};
var three = {bar: 0};
three.__proto__ = {};
three.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__.__proto__.__proto__ = {};

poly(one);
poly(two);
poly(three);

%OptimizeFunctionOnNextCall(poly);

poly(one);
poly(two);
poly(three);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5eb4bae^!)  
---   

## **regress file:regress-iteration-order.js**  
   
**[Commit: Fix bug in __proto__ assignment transition cache where we forget the next enumeration index resulting in wrong iteration order.](https://chromium.googlesource.com/v8/v8/+/0a856e0)**  
  
Date(Commit): Mon Jun 04 12:07:46 2012  
Code Review: [https://chromiumcodereview.appspot.com/10515006](https://chromiumcodereview.appspot.com/10515006)  
Regress: [mjsunit/regress/regress-iteration-order.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-iteration-order.js)  
```javascript
var x = {a: 1, b: 2, c: 3};

x.__proto__ = {};

delete x.b;

x.d = 4;

s = "";

for (key in x) {
    s += x[key];
}

assertEquals("134", s);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a856e0^!)  
---   

## **regress file:regress-fast-literal-transition.js**  
   
**[Commit: Fix LFastLiteral to check boilerplate elements kind.](https://chromium.googlesource.com/v8/v8/+/b54ca31)**  
  
Date(Commit): Mon Apr 30 14:59:13 2012  
Code Review: [https://chromiumcodereview.appspot.com/10254006](https://chromiumcodereview.appspot.com/10254006)  
Regress: [mjsunit/regress/regress-fast-literal-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fast-literal-transition.js)  
```javascript
function f(x) {
  switch(x) {
    case 1: return 1.4;
    case 2: return 1.5;
    case 3: return {};
    default: gc();
  }
}

function g(x) {
  return [1.1, 1.2, 1.3, f(x)];
}


assertEquals([1.1, 1.2, 1.3, 1.4], g(1));
assertEquals([1.1, 1.2, 1.3, 1.5], g(2));
%OptimizeFunctionOnNextCall(g);


assertEquals([1.1, 1.2, 1.3, {}], g(3));



assertEquals([1.1, 1.2, 1.3, undefined], g(4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b54ca31^!)  
---   

## **regress file:regress-sqrt.js**  
   
**[Commit: Ensure consistency of Math.sqrt on Intel platforms.](https://chromium.googlesource.com/v8/v8/+/7659bea)**  
  
Date(Commit): Mon Mar 12 14:56:04 2012  
Code Review: [https://chromiumcodereview.appspot.com/9690010](https://chromiumcodereview.appspot.com/9690010)  
Regress: [mjsunit/regress/regress-sqrt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sqrt.js)  
```javascript
function f(x) {
  return Math.sqrt(x);
}

var x = 7.0506280066499245e-233;

var a = f(x);

f(0.1);
f(0.2);
%OptimizeFunctionOnNextCall(f);

var b = f(x);

assertEquals(a, b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7659bea^!)  
---   

## **regress file:regress-transcendental.js**  
   
**[Commit: Ensure consistent result of transcendental functions.](https://chromium.googlesource.com/v8/v8/+/12f2099)**  
  
Date(Commit): Fri Mar 02 14:33:15 2012  
Code Review: [https://chromiumcodereview.appspot.com/9572009](https://chromiumcodereview.appspot.com/9572009)  
Regress: [mjsunit/regress/regress-transcendental.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-transcendental.js)  
```javascript
function test(f, x, name) {
  
  gc();
  
  var runtime_result = f(x);
  
  for (var i = 0; i < 100000; i++) f(i);
  
  var gencode_result = f(x);
  print(name + " runtime function: " + runtime_result);
  print(name + " generated code  : " + gencode_result);
  assertEquals(gencode_result, runtime_result);
}

test(Math.tan, -1.57079632679489660000, "Math.tan");
test(Math.sin, 6.283185307179586, "Math.sin");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/12f2099^!)  
---   

## **regress file:regress-toint32.js**  
   
**[Commit: Fix a register assignment bug in typed array stores without SSE3 available.](https://chromium.googlesource.com/v8/v8/+/1e40f7a)**  
  
Date(Commit): Thu Mar 01 12:45:46 2012  
Code Review: [https://chromiumcodereview.appspot.com/9565007](https://chromiumcodereview.appspot.com/9565007)  
Regress: [mjsunit/compiler/regress-toint32.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-toint32.js)  
```javascript
var a = new Int32Array(1);
var G = 0x80000000;

function f(x) {
  var v = x;
  v = v + 1;
  a[0] = v;
  v = v - 1;
  return v;
}

assertEquals(G, f(G));
assertEquals(G, f(G));
%OptimizeFunctionOnNextCall(f);
assertEquals(G, f(G));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e40f7a^!)  
---   

## **regress file:regress-inlining-function-literal-context.js**  
   
**[Commit: On ia32 LFunctionLiteral instruction should get context from esi register instead of stack slot.](https://chromium.googlesource.com/v8/v8/+/f5c8ac9)**  
  
Date(Commit): Tue Feb 21 12:10:04 2012  
Code Review: [https://chromiumcodereview.appspot.com/9425061](https://chromiumcodereview.appspot.com/9425061)  
Regress: [mjsunit/regress/regress-inlining-function-literal-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inlining-function-literal-context.js)  
```javascript
function mkbaz(x) {
  function baz() {
    return function () {
      return [x];
    }
  }
  return baz;
}

var baz = mkbaz(1);

function foo() {
  var f = baz();
  return f();
}


gc();
gc();

assertArrayEquals([1], foo());
assertArrayEquals([1], foo());
%OptimizeFunctionOnNextCall(foo);
assertArrayEquals([1], foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f5c8ac9^!)  
---   

## **regress file:regress-smi-only-concat.js**  
   
**[Commit: Fix elements transition bug related to array.concat.](https://chromium.googlesource.com/v8/v8/+/3e58827)**  
  
Date(Commit): Wed Feb 08 09:50:13 2012  
Code Review: [https://chromiumcodereview.appspot.com/9358018](https://chromiumcodereview.appspot.com/9358018)  
Regress: [mjsunit/regress/regress-smi-only-concat.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-only-concat.js)  
```javascript
var fast_array = ['a', 'b'];
var array = fast_array.concat(fast_array);

assertTrue(%HasObjectElements(fast_array));
assertTrue(%HasObjectElements(array));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e58827^!)  
---   

## **regress file:regress-lazy-deopt.js**  
   
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

f(42);
f(42);
assertEquals(42, f(42));
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f(42));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8480569^!)  
---   

## **regress file:regress-inline-callfunctionstub.js**  
   
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

main(o.g);
main(o.g);
main(o.g);
%OptimizeFunctionOnNextCall(main);
main(o.g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d4bb18^!)  
---   

## **regress file:regress-deopt-call-as-function.js**  
   
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
---   

## **regress file:regress-bind-receiver.js**  
   
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
---   

## **regress file:regress-fundecl.js**  
   
**[Commit: Introduce local function declarations in Crankshaft and fix issue 1647.](https://chromium.googlesource.com/v8/v8/+/ffc6c7e)**  
  
Date(Commit): Wed Aug 31 13:26:08 2011  
Code Review: [http://codereview.chromium.org/7776009](http://codereview.chromium.org/7776009)  
Regress: [mjsunit/regress/regress-fundecl.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fundecl.js)  
```javascript
function h(a, b) {
  var r = a + b;
  function X() { return 42; }
  return r + X();
}

for (var i = 0; i < 5; i++) h(1,2);

%OptimizeFunctionOnNextCall(h);

assertEquals(45, h(1,2));
assertEquals("foo742", h("foo", 7));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ffc6c7e^!)  
---   

## **regress file:regress-lbranch-double.js**  
   
**[Commit: Fixed code generation for LBranch on ARM when the operand's representation is double.](https://chromium.googlesource.com/v8/v8/+/6f6c882)**  
  
Date(Commit): Tue Aug 02 15:14:12 2011  
Code Review: [http://codereview.chromium.org/7553010](http://codereview.chromium.org/7553010)  
Regress: [mjsunit/compiler/regress-lbranch-double.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-lbranch-double.js)  
```javascript
function foo() {
  return Math.sqrt(2.6415) ? 88 : 99;
}

assertEquals(88, foo());
assertEquals(88, foo());
%OptimizeFunctionOnNextCall(foo)
assertEquals(88, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f6c882^!)  
---   

## **regress file:regress-regexp-codeflush.js**  
   
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
---   

## **regress file:osr-regress-max-locals.js**  
   
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
---   

## **regress file:regress-const.js**  
   
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

for (var i = 0; i < 5; i++) {
  f();
  g();
}

%OptimizeFunctionOnNextCall(f);
%OptimizeFunctionOnNextCall(g);

assertEquals(1, f());
assertEquals(42, g());


function h(a, b) {
  var r = a + b;
  const X = 42;
  return r + X;
}

for (var i = 0; i < 5; i++) h(1,2);

%OptimizeFunctionOnNextCall(h);

assertEquals(45, h(1,2));
assertEquals("foo742", h("foo", 7));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e098588^!)  
---   

## **regress file:regress-loadfield.js**  
   
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
---   

## **regress file:regress-lazy-deopt-reloc.js**  
   
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
---   

## **regress file:regress-deopt-gc.js**  
   
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
  
  try { var a = 42; } catch(o) {};
  %DeoptimizeFunction(opt_me);
  gc();
}


opt_me();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2aa848^!)  
---   

## **regress file:regress-3408144.js**  
   
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
---   

## **regress file:regress-serialized-slots.js**  
   
**[Commit: Properly create variables to access outer arguments and function names.](https://chromium.googlesource.com/v8/v8/+/49144ee)**  
  
Date(Commit): Wed Jan 19 08:16:17 2011  
Code Review: [http://codereview.chromium.org/6266007](http://codereview.chromium.org/6266007)  
Regress: [mjsunit/compiler/regress-serialized-slots.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-serialized-slots.js)  
```javascript
function runner(f, expected) {
  for (var i = 0; i < 10000; i++) {  
    assertEquals(expected, f.call(this, 10));
  }
}

Function.prototype.bind = function(thisObject)
{
    var func = this;
    var args = Array.prototype.slice.call(arguments, 1);
    function bound()
    {
      
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
---   

## **regress file:regress-closures-with-eval.js**  
   
**[Commit: Make closures optimizable by Crankshaft compiler.](https://chromium.googlesource.com/v8/v8/+/fae90d4)**  
  
Date(Commit): Mon Jan 17 08:11:03 2011  
Code Review: [http://codereview.chromium.org/5753005](http://codereview.chromium.org/5753005)  
Regress: [mjsunit/compiler/regress-closures-with-eval.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-closures-with-eval.js)  
```javascript
function withEval(expr, filter) {
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

var expr = '([' + makeTagInfoJSON(128).join(', ') + '])'

for (var n = 0; n < 5; n++) {
  withEval(expr, function(a) { return a; });
}
%OptimizeFunctionOnNextCall(withEval);
withEval(expr, function(a) { return a; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fae90d4^!)  
---   

## **regress file:regress-intoverflow.js**  
   
**[Commit: Fix bugs in the range analysis for integers.](https://chromium.googlesource.com/v8/v8/+/73737fc)**  
  
Date(Commit): Thu Dec 16 18:01:36 2010  
Code Review: [http://codereview.chromium.org/5860009](http://codereview.chromium.org/5860009)  
Regress: [mjsunit/compiler/regress-intoverflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-intoverflow.js)  
```javascript
function testMul(a, b) {
  a *= 2;
  b *= 2;
  if (a < 1 && b < 1) {
    return a * b;
  }
}

for (var i=0; i<5; i++) testMul(0,0);
%OptimizeFunctionOnNextCall(testMul);
assertEquals(4611686018427388000, testMul(-0x40000000, -0x40000000));

function testAdd(a, b) {
  a *= 2;
  b *= 2;
  if (a < 1 && b < 1) {
    return a + b;
  }
}

for (var i=0; i<5; i++) testAdd(0,0);
%OptimizeFunctionOnNextCall(testAdd);
assertEquals(-4294967296, testAdd(-0x40000000, -0x40000000));


function testSub(a, b) {
  a *= 2;
  b *= 2;
  if (b == 2) {print(a); print(b);}
  if (a < 1 && b < 3) {
    return a - b;
  }
}

for (var i=0; i<5; i++) testSub(0,0);
%OptimizeFunctionOnNextCall(testSub);
assertEquals(-2147483650, testSub(-0x40000000, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73737fc^!)  
---   

## **regress file:regress-swapelements.js**  
   
**[Commit: Add array bound checks to code generated for SwapElements. This fixes a bug that lead to a segfault when an array was modified while it was sorted.](https://chromium.googlesource.com/v8/v8/+/5f962f2)**  
  
Date(Commit): Wed Dec 15 09:52:58 2010  
Code Review: [http://codereview.chromium.org/5686006](http://codereview.chromium.org/5686006)  
Regress: [mjsunit/regress/regress-swapelements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-swapelements.js)  
```javascript
function Item(val) {
  this.value = val;
}


var size = 23;
var array1 = new Array(size);


function myToString() {
  array1.splice(0, 1);
  return this.value.toString();
}


function test() {
  for (var i = 0; i < size; i++) {
    array1[i] = new Item(i);
    array1[i].toString = myToString;
  }
  array1.sort();
}


test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f962f2^!)  
---   

## **regress file:regress-3260426.js**  
   
**[Commit: Be more careful about exiting inlined functions in a test context.](https://chromium.googlesource.com/v8/v8/+/e0d3f6a)**  
  
Date(Commit): Tue Dec 07 12:07:40 2010  
Code Review: [http://codereview.chromium.org/5566005](http://codereview.chromium.org/5566005)  
Regress: [mjsunit/compiler/regress-3260426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3260426.js)  
```javascript
function always_false() {}
function test() { return always_false() ? 0 : 1; }

assertEquals(1, test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0d3f6a^!)  
---   

## **regress file:regress-stacktrace-methods.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-stacktrace-methods.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-stacktrace-methods.js)  
```javascript
function Hest() {}
function Svin() {}

Svin.prototype.two = function() { /* xxxxxxx */ o.three(); }

Hest.prototype.one = function(x) { x.two(); }

Hest.prototype.three = function() { if (v == 42) throw new Error("urg"); }

var o = new Hest();
var s = new Svin();
var v = 0;

for (var i = 0; i < 5; i++) {
  o.one(s);
}
%OptimizeFunctionOnNextCall(Hest.prototype.one);
%OptimizeFunctionOnNextCall(Hest.prototype.three);
o.one(s);

v = 42;

try {
  o.one(s);
} catch (e) {
  var stack = e.stack.toString();
  var p3 = stack.indexOf("at Hest.three");
  var p2 = stack.indexOf("at Svin.two");
  var p1 = stack.indexOf("at Hest.one");
  assertTrue(p3 != -1);
  assertTrue(p2 != -1);
  assertTrue(p1 != -1);
  assertTrue(p3 < p2);
  assertTrue(p2 < p1);
  assertTrue(stack.indexOf("38:56") != -1);
  assertTrue(stack.indexOf("34:51") != -1);
  assertTrue(stack.indexOf("36:38") != -1);
  assertTrue(stack.indexOf("54:5") != -1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-stacktrace.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-stacktrace.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-stacktrace.js)  
```javascript
eval("function two() { /* xxxxxxx */ three(); }");

function one() {
  two();
}

function three() {
  throw new Error("urg");
}

try {
 one();
} catch (e) {
  var stack = e.stack.toString();
  var p3 = stack.indexOf("at three");
  var p2 = stack.indexOf("at two");
  var p1 = stack.indexOf("at one");
  assertTrue(p3 != -1);
  assertTrue(p2 != -1);
  assertTrue(p1 != -1);
  assertTrue(p3 < p2);
  assertTrue(p2 < p1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-rep-change.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-rep-change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-rep-change.js)  
```javascript
function test(start) {
  if (true) {
    for (var i = start; i < 10; i++) { }
  }
  for (var i = start; i < 10; i++) { }
}

var n = 3;

for (var i = 0; i < n; ++i) {
  test(0);
}

%OptimizeFunctionOnNextCall(test);
test(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-or.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-or.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-or.js)  
```javascript
function f1(x) {
  var c = "fail";
  if (!x || g1()) {
    c = ~x;
  }
  return c;
}

function g1() { try { return 1; } finally {} }

for (var i = 0; i < 5; i++) f1(42);
%OptimizeFunctionOnNextCall(f1);

assertEquals(-1, f1(0));
assertEquals(-43, f1(42));
assertEquals(-1, f1(""));

function f2(x) {
  var c = "fail";
  if (!x || !g2()) {
    c = ~x;
  }
  return c;
}

function g2() { try { return 0; } finally {} }

for (var i = 0; i < 5; i++) f2(42);
%OptimizeFunctionOnNextCall(f2);

assertEquals(-1, f2(""));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-max.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-max.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-max.js)  
```javascript
for (var i = 0; i < 5; i++) Math.max(0, 0);
Math.max(0, 0);

var r = Math.max(-0, -0);
assertEquals(-Infinity, 1 / r);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-loop-deopt.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-loop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-loop-deopt.js)  
```javascript
function h() {
  var i = 3, j = 0;
  while(--i >= 0) {
    var x = i & 1;
    if(x > 0) {
      continue;
    }
    j++;
  }
  return j;
}

assertEquals(2, h());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-gvn.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-gvn.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-gvn.js)  
```javascript
function test(a) {
  var res = a[0] + a[0];
  if (res == 0) {
    a[0] = 1;
  }
  return a[0];
}

var a = new Array();

var n = 100;

var result = 0;
for (var i = 0; i < n; ++i) {
  if (i == 10) %OptimizeFunctionOnNextCall(test);
  a[0] = 0;
  result += test(a);
}


assertEquals(n, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-gap.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-gap.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-gap.js)  
```javascript
function small_select(n, v1, v2) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = tmp;
  }
  return v1;
}

function select(n, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = v6;
    v6 = v7;
    v7 = v8;
    v8 = v9;
    v9 = v10;
    v10 = tmp;
  }
  return v1;
}

function select_while(n, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10) {
  var i = 0;
  while (i < n) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = v6;
    v6 = v7;
    v7 = v8;
    v8 = v9;
    v9 = v10;
    v10 = tmp;
    i++;
  }
  return v1;
}

function two_cycles(n, v1, v2, v3, v4, v5, x1, x2, x3, x4, x5) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = tmp;
    tmp = x1;
    x1 = x2;
    x2 = x3;
    x3 = x4;
    x4 = x5;
    x5 = tmp;
  }
  return v1 + x1;
}

function two_cycles_while(n, v1, v2, v3, v4, v5, x1, x2, x3, x4, x5) {
  var i = 0;
  while (i < n) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = tmp;
    tmp = x1;
    x1 = x2;
    x2 = x3;
    x3 = x4;
    x4 = x5;
    x5 = tmp;
    i++;
  }
  return v1 + x1;
}
assertEquals(1, small_select(0, 1, 2));
assertEquals(2, small_select(1, 1, 2));
assertEquals(1, small_select(10, 1, 2));

assertEquals(1, select(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4, select(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(10, select(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1 + 6, two_cycles(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4 + 9, two_cycles(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(5 + 10, two_cycles(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1, select_while(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4, select_while(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(10, select_while(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1 + 6, two_cycles_while(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4 + 9, two_cycles_while(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(5 + 10, two_cycles_while(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-funcaller.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-funcaller.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-funcaller.js)  
```javascript
function A() {}

function fun(x) {
  if (x == 0) return fun.caller;
  if (x == 1) return gee.caller;
  return 42;
}
function gee(x) { return this.f(x); }

A.prototype.f = fun;
A.prototype.g = gee;

var o = new A();

for (var i=0; i<5; i++) {
  o.g(i);
}
%OptimizeFunctionOnNextCall(o.g);
assertEquals(gee, o.g(0));
assertEquals(null, o.g(1));


function hej(x) {
  if (x == 0) return o.g(x);
  if (x == 1) return o.g(x);
  return o.g(x);
}

for (var j=0; j<5; j++) {
  hej(j);
}
%OptimizeFunctionOnNextCall(hej);
assertEquals(gee, hej(0));
assertEquals(hej, hej(1));


function from_eval(x) {
  if (x == 0) return eval("o.g(x);");
  if (x == 1) return eval("o.g(x);");
  return o.g(x);
}

for (var j=0; j<5; j++) {
  from_eval(j);
}
%OptimizeFunctionOnNextCall(from_eval);
assertEquals(gee, from_eval(0));
assertEquals(from_eval, from_eval(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-funarguments.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-funarguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-funarguments.js)  
```javascript
function A() {}
function B() {}

function fee(x, y) {
  if (x == 1) return fee["arg" + "uments"];
  if (x == 2) return gee["arg" + "uments"];
  return 42;
}

function gee(x) { return this.f(2 - x, "f"); }

function foo(x, y) {
  if (x == 0) return foo["arg" + "uments"];
  if (x == 1) return goo["arg" + "uments"];
  return 42;
}

function goo(x) { return this.f(x, "f"); }

A.prototype.f = fee;
A.prototype.g = gee;

B.prototype.f = foo;
B.prototype.g = goo;

var o = new A();

function hej(x) {
  if (x == 0) return o.g(x, "h");
  if (x == 1) return o.g(x, "h");
  return o.g(x, "z");
}

function opt() {
  for (var k=0; k<2; k++) {
    for (var i=0; i<5; i++) o.g(i, "g");
    for (var j=0; j<5; j++) hej(j);
  }
  %OptimizeFunctionOnNextCall(o.g);
  %OptimizeFunctionOnNextCall(hej);
}

opt();
assertArrayEquals([0, "g"], o.g(0, "g"));
assertArrayEquals([1, "f"], o.g(1, "g"));
assertArrayEquals([0, "h"], hej(0));
assertArrayEquals([1, "f"], hej(1));

o = new B();

opt();
assertArrayEquals([0, "f"], o.g(0, "g"));
assertArrayEquals([1, "g"], o.g(1, "g"));
assertArrayEquals([0, "f"], hej(0));
assertArrayEquals([1, "h"], hej(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-arrayliteral.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-arrayliteral.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-arrayliteral.js)  
```javascript
var G = 41;
var H = 42;
function f() { var v = [G,H]; return v[1]; }
assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-arguments.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-arguments.js)  
```javascript
function f() { return this.foo; }

function g() { return f.apply(null, arguments); }
function h() { return f.apply(void 0, arguments); }

var foo = 42;

for (var i = 0; i < 3; i++) assertEquals(42, g());
%OptimizeFunctionOnNextCall(g);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, g());

for (var i = 0; i < 3; i++) assertEquals(42, h());
%OptimizeFunctionOnNextCall(h);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, h());

var G1 = 21;
var G2 = 22;

function u() {
 var v = G1 + G2;
 return f.apply(v, arguments);
}

Number.prototype.foo = 42;
delete Number.prototype.foo;

for (var i = 0; i < 3; i++) assertEquals(void 0, u());
%OptimizeFunctionOnNextCall(u);
%OptimizeFunctionOnNextCall(f);
assertEquals(void 0, u());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3252443.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3252443.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3252443.js)  
```javascript
var document = new Object();
document.getElementById = function(s) { return { style: {}}};
function x(p0, p1, p2, p3) {
  document.getElementById(p1+p0).style.display='';
  document.getElementById(p1+''+p0).style.backgroundColor = "";
  document.getElementById(p1+''+p0).style.color="";
  document.getElementById(p1+''+p0).style.borderBottomColor = "";
  for (var i = p3; i <= p2; ++i) {
    if (i != p0) {
      document.getElementById(p1+i).style.display='';
      document.getElementById(p1+''+i).style.backgroundColor = "";
      document.getElementById(p1+''+i).style.color="";
      document.getElementById(p1+''+i).style.borderBottomColor = "";
    }
  }
}

x(1, "xxx", 10000, 1)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3249650.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3249650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3249650.js)  
```javascript
function f0(x) { try { } catch (e) {}}
function f1(x) { try { } catch (e) {}}
function f2(x) { try { } catch (e) {}}
function f3(x) { try { } catch (e) {}}

var object = { a: "", b: false, c: {}};
object.f = function(x) { return this; }


function test(x) {
  f0(x);
  f1(x);
  f2(x);
  f3(x);
  x.a.b == "";
  object.f("A").b = true;
  object.f("B").a = "";
  object.f("C").c.display = "A";
  object.f("D").c.display = "A";
}

var x = {a: {b: "" }};
for (var i = 0; i < 20000; i++) test(x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3247124.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3247124.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3247124.js)  
```javascript
var foo = unescape("%E0%E2%EA%F4%FB%E3%F5%E1%E9%ED%F3%FA%E7%FC%C0%C2%CA%D4%DB%C3%D5%C1%C9%CD%D3%DA%C7%DC");

function bar(x) {
  var s = new String(x);
  var a = new String(foo);
  var b = new String('aaeouaoaeioucuAAEOUAOAEIOUCU');

  var i = new Number();
  var j = new Number();
  var c = new String();
  var r = '';

  for (i = 0; i < s.length; i++) {
    c = s.substring(i, i + 1);
    for (j = 0; j < a.length; j++) {
      if (a.substring(j, j + 1) == c) {
        c = b.substring(j, j + 1);
      }
    }
    r += c;
  }

  return r.toLowerCase();
}

for (var i = 0; i < 100; i++) bar(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3230771.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3230771.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3230771.js)  
```javascript
function f() {
  for (var h = typeof arguments[0] == "object" ? 0 : arguments; false; ) { }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3218915.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3218915.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3218915.js)  
```javascript
function withCommaExpressionInConditional(x) {
  if (x > 1000) { for (var i = 0; i < 10000; i++) { } }
  var y;
  if (y = x, y > 1) {
    return 'big';
  }
  return (y = x + 1, y > 1) ? 'medium' : 'small';
}

for (var i = 0; i < 5; i++) {
  withCommaExpressionInConditional(i);
}
%OptimizeFunctionOnNextCall(withCommaExpressionInConditional);
withCommaExpressionInConditional(i);
withCommaExpressionInConditional("1")  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3218915.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3218915.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3218915.js)  
```javascript
function side_effect() { try {} finally {} return "wrong"; }


function observe(x, y) { try {} finally {} return x; }




function test(x) { return observe(this, ((0, side_effect()), x + 1)); }


for (var i = 0; i < 5; ++i) test(0);
%OptimizeFunctionOnNextCall(test);
test(0);




assertFalse(test("a") === "wrong");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3218530.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3218530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3218530.js)  
```javascript
var m = Math;
var p = "floor";

function test() {
  var bignumber = 31363200000;
  assertDoesNotThrow(assertEquals(m[p](Math.round(bignumber/864E5)/7)+1, 52));
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3199913.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3199913.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3199913.js)  
```javascript
var y = {
  'a' : function (x, y) { return 'called a(' + x + ', ' + y + ')' },
  'b' : function (x, y) { return 'called b(' + x + ', ' + y + ')' }
}

function C() {
}

C.prototype.f = function () {
  return y[(this.a == 1 ? "a" : "b")](0, 1);
}

obj = new C()
assertEquals('called b(0, 1)', obj.f())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3185905.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3185905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3185905.js)  
```javascript
function test1(x) {
  var a = arguments.callee;
  x = 1;
  x = 2;
  assertEquals(2, x);
}
test1(0)

function test2(x) {
  var a = arguments.callee;
  x++;
  x++;
  assertEquals(2, x);
}
test2(0)

function test3(x) {
  var a = arguments.callee;
  x += 1;
  x += 1;
  assertEquals(2, x);
}
test3(0)

function test4(x) {
  var arguments = { 0 : 3, 'x' : 4 };
  x += 1;
  x += 1;
  assertEquals(2, x);
  assertEquals(3, arguments[0])
  assertEquals(4, arguments['x'])
}
test4(0)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3185901.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3185901.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3185901.js)  
```javascript
var x;

function f() { if (g()) { } }
function g() { if (x) { return true; } }

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3136962.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3136962.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3136962.js)  
```javascript
var height = 267;

var count = 0;
function inner() { height = 0; ++count; }
function outer() {}

function test() {
  for (var i = 0; i < height; ++i) {
    for (var j = -6; j < 7; ++j) {
      if (i + j < 0 || i + j >= height) continue;
      for (var k = -6; k < 7; ++k) {
        inner();
      }
    }
    outer();
  }
}

test();

assertEquals(13, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3006390.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3006390.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3006390.js)  
```javascript
function X() { }
X.prototype.valueOf = function () { return 7; }

function f(x, y) { return x % y; }

assertEquals(1, f(8, new X()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-8.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-8.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-8.js)  
```javascript
var gp = "";
var yE = "";
var W = "";
var LA = "";
var zE = "";
var Fp = "";
var AE = "";
var Gob = "";
var Hob = "";
var Iob = "";
var Job = "";
var Kob = "";
var Lob = "";
var Mob = "";
var p = "";
function O() { this.append = function(a,b,c,d,e) { return a + b + c + d + e; } }

function Nob(b,a) {
 var c;
 if (b==2) {
   c=new O;
   c.append(gp,
            yE,
            W,
            LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(AE+(LA+(a.total+Gob))))))))),
            p);
   c=c.toString();
 } else {
   if (b==1) {
     if(a.total>=2E6) {
       c=new O;
       c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Hob))))),p);
       c=c.toString();
     } else {
       if(a.total>=2E5) {
         c=new O;
         c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Iob))))),p);
         c=c.toString();
       } else {
         if(a.total>=2E4) {
           c=new O;
           c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Job))))),p);
           c=c.toString();
         } else {
           if(a.total>=2E3) {
             c=new O;
             c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Kob))))),p);
             c=c.toString();
           } else {
             if(a.total>=200) {
               c=new O;
               c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Lob))))),p);
               c=c.toString();
             } else {
               c=new O;
               c.append(gp,yE,W,
                        LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(Mob+(LA+(a.total+zE))))))))),
                        p);
               c=c.toString();
             }
             c=c;
           }
           c=c;
         }
         c=c;
       }
       c=c;
     }
     c=c;
   } else {
     c=new O;
     c.append(gp,yE,W,
              LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(AE+(LA+(a.total+zE))))))))),
              p);
     c=c.toString();
   }
   c=c;
 }
 return c;
}
Nob(2, { Un: "" , Im: "" , total: 42});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-7.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-7.js)  
```javascript
var G = 42;

function f() {
  var v = G;
  var w = v >> 0;
  return w;
}

for(var i=0; i<10000; i++) f();

assertEquals(G, f());
G = 2000000000;
assertEquals(G, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-6.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-6.js)  
```javascript
function f(a, b, c) {
  if (a == 0 || b == 0) return a;
  return a + c;
}

assertEquals(0, f(0, 0, 0));
assertEquals(0, f(0, 1, 0));
assertEquals(1, f(1, 0, 0));
assertEquals(2, f(2, 1, 0));




assertEquals(1.5, f(1, 1, 0.5));
assertEquals(2.5, f(2, 1, 0.5));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-5.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5.js)  
```javascript
function f(y) {
  var x = 0;

  foo: {
    x++;
    bar: {
       if (y == 0) break bar; else break foo;
    }
    x++;
  }
  return x;
}

assertEquals(2, f(0));
assertEquals(1, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-4.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4.js)  
```javascript
function f(p) {
  var y=0;
  for (var x=0; x<10; x++) {
    if (x > 5) { y=y+p; break;}
  }
  return y+x;
}

for (var i=0; i<100000; i++) f(42);

var result = f("foo");
assertEquals("0foo6", result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-3.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3.js)  
```javascript
function fib(n) {
  var f0 = 0, f1 = 1;
  for (; n > 0; n = n -1) {
    var f2 = f0 + f1;
    f0 = f1; f1 = f2;
  }
  return f0;
}

assertEquals(2111485077978050, fib(75));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-2.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-2.js)  
```javascript
function TestCreateString(n)
{
  var l = n * 1;
  var r = 'r';
  while (r.length < n)
  {
    r = r + r;
  }
  return r;
}

assertEquals("r", TestCreateString(1));
assertEquals("rr", TestCreateString(2));
assertEquals("rrrr", TestCreateString(3));
assertEquals("rrrrrrrr", TestCreateString(6));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-1.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1.js)  
```javascript
function DaysInYear(y) {
  if (y % 4 != 0) return 365;
  if (y % 4 == 0 && y % 100 != 0) return 366;
  if (y % 100 == 0 && y % 400 != 0) return 365;
  if (y % 400 == 0) return 366;
}
assertEquals(365, DaysInYear(1999));
assertEquals(366, DaysInYear(2000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-0.js**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-0.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-0.js)  
```javascript
function TestNestedLoops() {
  var sum = 0;
  for (var i = 0; i < 200; i = i + 1) {
    for (var j = 0; j < 200; j = j + 1) {
      sum = sum + 1;
    }
  }
  return sum;
}
assertEquals(200 * 200, TestNestedLoops());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
---   

## **regress file:regress-conditional-position.js**  
   
**[Commit: Improve positions recording for calls.](https://chromium.googlesource.com/v8/v8/+/746d724)**  
  
Date(Commit): Thu Nov 04 15:12:03 2010  
Code Review: [http://codereview.chromium.org/4469002](http://codereview.chromium.org/4469002)  
Regress: [mjsunit/regress/regress-conditional-position.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-conditional-position.js)  
```javascript
var functionToCatch;
var lineNumber;

function catchLineNumber () {
  var x = {};

  Error.prepareStackTrace = function (error, stackTrace) {
    stackTrace.some(function (frame) {
      if (frame.getFunction() == functionToCatch) {
        lineNumber = frame.getLineNumber();
        return true;
      }
      return false;
    });
    return lineNumber;
  };

  Error.captureStackTrace(x);
  return x.stack;
}

function log() {
  catchLineNumber();
}

function foo() {}

function test1() {
  log(foo() == foo()
      ? 'a'
      : 'b');
}

function test2() {
  var o = { foo: function () {}}
  log(o.foo() == o.foo()
      ? 'a'
      : 'b');
}

function test3() {
  var o = { log: log, foo: function() { } };
  o.log(o.foo() == o.foo()
      ? 'a'
      : 'b');

}

function test(f, expectedLineNumber) {
  functionToCatch = f;
  f();

  assertEquals(expectedLineNumber, lineNumber);
}

test(test1, 58);
test(test2, 65);
test(test3, 72);

eval(test1.toString() + "
eval(test2.toString() + "
eval(test3.toString() + "

test(test1, 2);
test(test2, 3);
test(test3, 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/746d724^!)  
---   

## **regress file:regress-create-exception.js**  
   
**[Commit: Fix creation of an exception to avoid rare GC corner case.](https://chromium.googlesource.com/v8/v8/+/d22965c)**  
  
Date(Commit): Fri Oct 15 07:54:20 2010  
Code Review: [http://codereview.chromium.org/3782009](http://codereview.chromium.org/3782009)  
Regress: [mjsunit/regress/regress-create-exception.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-create-exception.js)  
```javascript
"use strict";


var v = [1, 2, 3, 4]

Object.preventExtensions(v);

function foo() {
  var re = /2147483647/;  
  for  (var i = 0; i < 10000; i++) {
    var ok = false;
    try {
      var j = 1;
      
      
      
      for (var j = 0; j < i % 93; j++) {
        j *= 1.123567;  
      }
      v[0x7fffffff] = 0;  
      assertTrue(false);
      return j;  
    } catch(e) {
      ok = true;
      assertTrue(re.test(e), 'e: ' + e);
    }
    assertTrue(ok);
  }
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d22965c^!)  
---   

## **regress file:regress-push-args-twice.js**  
   
**[Commit: Avoid pushing arguments twice in GenericBinaryOpStub.](https://chromium.googlesource.com/v8/v8/+/73c0239)**  
  
Date(Commit): Tue Sep 07 13:33:40 2010  
Code Review: [http://codereview.chromium.org/3290012](http://codereview.chromium.org/3290012)  
Regress: [mjsunit/regress/regress-push-args-twice.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-push-args-twice.js)  
```javascript
try {
  for (var key = 0; key != 10; key++) {
    var x = 1 + undefined;
  }
} catch(e) {
  fail("no exception", e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73c0239^!)  
---   

## **regress file:regress-r4998.js**  
   
**[Commit: Fix error in x64 fast smi loops, change 4998.](https://chromium.googlesource.com/v8/v8/+/cb1eedd)**  
  
Date(Commit): Wed Jul 14 13:22:47 2010  
Code Review: [http://codereview.chromium.org/2925012](http://codereview.chromium.org/2925012)  
Regress: [mjsunit/regress/regress-r4998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-r4998.js)  
```javascript
function foo() {
  return;
}

function bar() {
  var x1 = 3;
  var x2 = 3;
  var x3 = 3;
  var x4 = 3;
  var x5 = 3;
  var x6 = 3;
  var x7 = 3;
  var x8 = 3;
  var x9 = 3;
  var x10 = 3;
  var x11 = 3;
  var x12 = 3;
  var x13 = 3;

  foo();

  x1 = 257;
  x2 = 258;
  x3 = 259;
  x4 = 260;
  x5 = 261;
  x6 = 262;
  x7 = 263;
  x8 = 264;
  x9 = 265;
  x10 = 266;
  x11 = 267;
  x12 = 268;
  x13 = 269;

  
  
  
  
  
  
  
  for (x7 = 3; x7 < 10; ++x7) {
    foo();
  }
}

bar();

function aliasing() {
  var x = 3;
  var j;
  for (j = 7; j < 11; ++j) {
    x = j;
  }

  assertEquals(10, x);
  assertEquals(11, j);
}

aliasing();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb1eedd^!)  
---   

## **regress file:regress-r3391.js**  
   
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
---   

## **regress file:regress-2249423.js**  
   
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
---   

## **regress file:regress-1919169.js**  
   
**[Commit: Fix bug in static type inference for loops.](https://chromium.googlesource.com/v8/v8/+/2dd9717)**  
  
Date(Commit): Mon Jun 22 12:36:01 2009  
Code Review: [http://codereview.chromium.org/140058](http://codereview.chromium.org/140058)  
Regress: [mjsunit/regress/regress-1919169.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1919169.js)  
```javascript
function test() {
 var s2 = "s2";
 for (var i = 0; i < 2; i++) {
   
   var res = 1 + s2;
   s2 = 2;
 }
}


test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2dd9717^!)  
---   

## **regress file:regress-6-9-regexp.js**  
   
**[Commit: Fix regexp bug reported by Ian where [6-9] would match any digit.](https://chromium.googlesource.com/v8/v8/+/e2a01ed)**  
  
Date(Commit): Sat Jun 20 17:57:09 2009  
Code Review: [http://codereview.chromium.org/140021](http://codereview.chromium.org/140021)  
Regress: [mjsunit/regress/regress-6-9-regexp.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6-9-regexp.js)  
```javascript
assertFalse(/[6-9]/.test('2'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2a01ed^!)  
---   

## **regress file:regress-1493017.js**  
   
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
---   

## **regress file:regress-1439135.js**  
   
**[Commit: Added failing test case for bug 1439135.](https://chromium.googlesource.com/v8/v8/+/3d4d596)**  
  
Date(Commit): Tue Oct 21 07:39:53 2008  
Code Review: [http://codereview.chromium.org/7808](http://codereview.chromium.org/7808)  
Regress: [mjsunit/regress/regress-1439135.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1439135.js)  
```javascript
function Test() {
  var left  = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
  var right = "YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY";
  for (var i = 0; i < 100000; i++) {
    var cons = left + right;
    var substring = cons.substring(20, 80);
    var index = substring.indexOf('Y');
    assertEquals(34, index);
  }
}

Test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d4d596^!)  
---   

## **regress file:regress-1134697.js**  
   
**[Commit: - Added some "special" tests that were left out before.](https://chromium.googlesource.com/v8/v8/+/472ae34)**  
  
Date(Commit): Wed Sep 03 07:31:19 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@116](http://v8.googlecode.com/svn/branches/bleeding_edge@116)  
Regress: [mjsunit/regress/regress-1134697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1134697.js)  
```javascript
(-90).toPrecision(6);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/472ae34^!)  
---   

## **regress file:regress-1346700.js**  
   
**[Commit: Add a test that access a property name in its unicode escaped form.](https://chromium.googlesource.com/v8/v8/+/69b74a9)**  
  
Date(Commit): Thu Aug 28 18:32:52 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@30](http://v8.googlecode.com/svn/branches/bleeding_edge@30)  
Regress: [mjsunit/regress/regress-1346700.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1346700.js)  
```javascript
var o = {"\u59cb\u53d1\u7ad9": 1};
assertEquals(1, o.\u59cb\u53d1\u7ad9);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/69b74a9^!)  
---   

## **regress file:regress-20070207.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-20070207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-20070207.js)  
```javascript
function f(s) {
  arguments.length;
  return (s += 10) < 0;
}

assertTrue(f(-100));
assertTrue(f(-20));
assertFalse(f(-10));
assertFalse(f(-5));
assertFalse(f(0));
assertFalse(f(10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1327557.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1327557.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1327557.js)  
```javascript
var x = { valueOf: function() { throw "x"; } };
var y = { valueOf: function() { throw "y"; } };

var exception = false;
try {
  x * -y;
} catch (e) {
  exception = true;
  assertEquals("y", e);
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1254366.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1254366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1254366.js)  
```javascript
function gee() {};

Object.prototype.findOrStore = function() {
  var z = this.vvv = gee;
  return z;
};

var a =  new Object();
assertEquals(gee, a.findOrStore());
assertEquals(gee, a.findOrStore());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1215653.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1215653.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1215653.js)  
```javascript
var caught = false;
try {
  OverflowParserStack();
  assertTrue(false);
} catch (e) {
  assertTrue(e instanceof RangeError);
  caught = true;
}
assertTrue(caught);


function OverflowParserStack() {
  var s =
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((";
  eval(s);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1213516.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1213516.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1213516.js)  
```javascript
function run() {
 var a = 0;
 L: try {
   throw "x";
 } catch(x) {
   break L;
 } finally {
   a = 1;
 }
 assertEquals(1, a);
}

run();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1203459.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1203459.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1203459.js)  
```javascript
var obj = { 0.2 : 'a' }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1200351.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1200351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1200351.js)  
```javascript
var enums = "";
for (var k in this) enums += (k + '|');
assertEquals(-1, enums.split('|').indexOf("constructor"));


new this.constructor;
new this.constructor();
new this.constructor(1,2,3,4,5,6);

var x = 0;
try {
  eval("SetValueOf(typeof(break.prototype.name), Math.max(typeof(break)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join((void), false.className(), null instanceof continue, return 'a', 0.__defineGetter__(x,function(){native}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ void&&null.push(goto NaN) : Math.max(undef).toText }) { {-1/null,1.isNull} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new break>>>=native.charCodeAt(-1.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(this > native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {native,0.2}?continue+undef:IsSmi(0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = break.toString()&&return continue")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (-1==continue.toJSONProtocol, GetFunctionFor(break.call(NaN)), (!new RegExp).prototype.new Object()<<void) { debugger.__defineSetter__(null,function(){continue})>>>=GetFunctionFor(-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat(NaN).splice() in null.add(1).className()) { true[0.2]<<x.splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (debugger.constructor.valueOf()) { this.sort().true.splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("unescape(break.toObject()).prototype.new RegExp.continue.__lookupGetter__(x.slice(1, NaN)) = typeof(null.push(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Iterator(continue.pop()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new RegExp.shift().concat({debugger,continue}) }; X(return goto 0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(0.add(break)&&x > null)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ eval(Array(x)) : 1.call('a').superConstructor }) { debugger.lastIndex.toLocaleString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = return true.__defineGetter__(this,function(){0.2})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new typeof(0)&this.lastIndex")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("String(new RegExp.call(1)).prototype.unescape(parseFloat(-1)) = false<<true.x.lastIndexOf(1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 1+debugger.valueOf() : continue.join().name() }) { parseInt(true)==undef.sort() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new RegExp>>0.2.superConstructor.prototype.eval(void).className() = false.join().prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export (new Object()?undef:native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new null.isNull.slice(x.prototype.value, Iterator(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export function () { 0.2 }.unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.max(continue.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = return debugger.toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (-1.length+new Object().prototype.name) { case (debugger.constructor.sort()): IsPrimitive(undef.__defineSetter__(undef,function(){native})); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete (!new Object().toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(0<<'a'>>>=new RegExp['a'])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {unescape(true),new RegExp.isNull}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1.lastIndexOf(false)?parseFloat(void):Join(null, continue, new Object(), x, break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label null/void-break.__lookupGetter__(native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2.join().constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label function () { false }.__lookupGetter__(this==1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(-1.prototype.0.2.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new return goto -1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {Number(debugger)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (parseInt(break) instanceof 0.length) { this.(!0.2) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(break.superConstructor[throw new false(true)], this.~x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(function () { IsSmi(-1) }, unescape(IsPrimitive(void)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (new RegExp.join().className() in new Object().length()>>true.toObject()) { parseFloat(escape(debugger)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new String(debugger).toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(1.indexOf('a')<<break.__lookupGetter__('a'), new Object().null.prototype.new RegExp.charCodeAt(-1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new {parseInt(0)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(void.join().add(escape(undef)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native parseFloat(false.charAt(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(~Iterator(void))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(NaN.shift().toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(native-debugger<<continue.slice(x, new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = parseFloat(~new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (null.size/true.add(void) in 0+continue&true.null) { continue.toObject()/throw new true(debugger) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Iterator(native+break) in debugger.superConstructor.constructor) { Math.max(0.add(undef)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {-1.add(native),true.sort()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {IsSmi(break),throw new 'a'(null)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(0).length()) { case ('a'.toObject().__defineSetter__(GetFunctionFor(null),function(){(!x)})): IsSmi(void).constructor; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 0.lastIndexOf(NaN).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0>>>=this.lastIndex : new Object().lastIndexOf(true).toObject() }) { x.lastIndex > 1.__defineSetter__(false,function(){this}) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ throw new false(0.2).prototype.name : parseFloat(false)+(!debugger) }) { escape(undef.lastIndex) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Math.pow(0.2).toJSONProtocol.prototype.break.superConstructor.slice(NaN.exec(undef), -1.lastIndexOf(NaN)) = true.splice().length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native continue.className().constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (0.2.isNull&undef.toString()) { continue/void+parseInt(null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Math.pow(break==this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(continue.__lookupGetter__(null).constructor, debugger.filter(0.2)>>>=this.'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2.unshift() > true.size : return Math.max(new RegExp) }) { void.splice().toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new unescape(false).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return this.true?'a'==this:0.2.__lookupGetter__(void) }; X(Iterator(false).length)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = function () { null }.__defineSetter__(0.charCodeAt(new Object()),function(){null>>>=new Object()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import goto 'a'.charAt(native.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import 0.2.isNull.__lookupGetter__(debugger.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~new Object().push(Array(null)) in new RegExp>>>=void.prototype.name) { goto break.lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete String(x).slice(String('a'), parseFloat(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(continue.__defineGetter__(0.2,function(){1}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(true.concat(undef)==0.2.new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return NaN['a']?-1.exec(0):NaN.prototype.this }; X(native.prototype.name.toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (debugger==continue.toObject(), Array(NaN.className()), Math.max(new RegExp).prototype.value) { GetFunctionFor('a').prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new parseInt(break)==Array(x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseInt(0.2.charCodeAt(this)), this.continue.prototype.name, native.superConstructor.superConstructor) { Join(0.__defineGetter__(continue,function(){undef}), {1}, parseFloat(0), undef.__defineSetter__(break,function(){null}), x?-1:-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join(debugger.splice(), parseInt(NaN), new RegExp.pop(), this.false, x.-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Math.max(native).charCodeAt(continue==break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (void==NaN.sort(), new Object()==new RegExp.toObject(), -1/NaN.unshift()) { GetFunctionFor(true).name() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ((!'a'.join()), ~NaN.__defineGetter__(undef,function(){this}), Math.pow(NaN).__lookupGetter__(typeof(false))) { throw new debugger.toObject()(Math.max(-1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (NaN.shift()&&undef&&continue in throw new x(NaN).prototype.-1&x) { return native.toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (0).charAt(this.charCodeAt(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return x.valueOf().size }; X(0.2.unshift().unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (eval(new Object().valueOf())) { break.prototype.name.__defineGetter__(eval(NaN),function(){Math.max(native)}) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Math.pow(1).isNull in Iterator(continue.length())) { Join(true, 0.2, null, x, new Object()).length }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(0>>>=void.unshift(), void.exec('a').undef.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete throw new this(0.2).pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Iterator(unescape(continue))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return unescape(goto debugger) }; X(new RegExp.push(break).name())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = undef/'a'.indexOf(-1.exec(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.isNull.filter(this.toText), function () { throw new 'a'(0.2) }, native?break:undef.prototype.return continue) { Array(void.toText) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.slice(new Object(), 1).isNull")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (0.2.className().call((!debugger)), native.__defineGetter__(0,function(){x}).name(), null.splice().splice()) { NaN.charCodeAt(new Object()) > true.toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native false.length?new RegExp instanceof this:Array(undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new ~0.2.call(typeof(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(0.2.sort())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new x.join().shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (~new Object().toText) { case (new RegExp.unshift().exec(new RegExp<<debugger)): -1.length.exec(this.isNull); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(~true)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new unescape(debugger.call(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new GetFunctionFor(0.2).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete IsPrimitive(null.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (eval(0.2) instanceof debugger.splice() in null.superConstructor==new Object()&void) { Number(0+x) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ('a'-continue?null.length():escape(continue)) { return undef.push(false.shift()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Array(x.length) in 'a'.length().sort()) { goto (new Object()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (NaN==true.length) { IsPrimitive(0.2).prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(return true&&void, new RegExp.toObject().length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.pow(void).length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(void.add(continue).charCodeAt(this.toObject()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join(break.toObject(), 0.2.isNull, false.call(0), break.filter(break), 1.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (1/NaN.__lookupGetter__(undef.prototype.value)) { escape(eval(this)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(Join(unescape(x), new RegExp.__defineGetter__(debugger,function(){NaN}), 'a'.indexOf(0.2), false.prototype.name, (this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Math.pow(native).indexOf(1>>>=-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new RegExp?native:continue.join().prototype.Math.max(x.__defineSetter__(1,function(){continue})) = parseFloat(parseInt(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native function () { new RegExp }.new RegExp.pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import typeof(new RegExp.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (0.2.size>>NaN-continue) { case ('a'.push(true).indexOf(NaN.lastIndexOf(-1))): {0.2,x}.toObject(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (IsSmi(new Object())/false.filter('a')) { function () { Iterator(debugger) } }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = break.lastIndex.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(new Object() > 0.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native IsPrimitive(continue)==break.charCodeAt(new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new break.true<<'a'-NaN")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(-1?'a':-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat('a'.exec(continue)) in (!new RegExp)&&0.2.toObject()) { {true,x}.add(void.prototype.NaN) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (-1.prototype.value.join()) { (!1.prototype.name) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new GetFunctionFor(continue).toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Math.pow(continue.slice(null, native)), goto (!0), native?1:this.charAt(String(debugger))) { parseFloat(~this) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(debugger.pop().length, new RegExp.isNull.toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (typeof(new RegExp.slice(new RegExp, 0)) in native.toLocaleString().lastIndexOf(0.2.length())) { native>>>=new RegExp.length() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native x.join().className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0?0:true.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = IsPrimitive(0).concat(new Object().name())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new parseFloat(x)?this.valueOf():IsSmi(x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 'a'.slice(null, -1).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 'a'+void.concat('a'>>>=-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(escape(0.length))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = parseInt(0.lastIndexOf(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(null&debugger.valueOf(), 0[false].push(false.add(debugger)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = parseInt(new RegExp.__lookupGetter__(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(~false&&break>>0, new RegExp.lastIndex.add({this}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Join(break, continue, 0, debugger, NaN).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import new Object().sort().superConstructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsSmi(goto -1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Iterator(null).toObject() }; X(-1==new Object()==0.__lookupGetter__(native))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native void.join().add(parseFloat(continue))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (function () { -1 }.shift()) { escape(1.unshift()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(new RegExp.indexOf(1).filter(continue instanceof break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (NaN?continue:NaN.shift()) { native.push(null).add(new Object().superConstructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new Object().length().toText }; X(debugger.indexOf(this).toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Object().call('a').charCodeAt(native.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new function () { continue }.add(true.slice(continue, new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x[native] instanceof -1.join().prototype.this.null.size = 0.2.prototype.x+0.2.indexOf(false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (this instanceof new RegExp.splice() in null>>>=new RegExp.valueOf()) { function () { unescape(1) } }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (true.shift()/native.null in undef.call(NaN).isNull) { native+this-x.size }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return false.pop()<<Join(continue, false, break, NaN, -1) }; X(IsSmi(debugger>>x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if ({parseFloat(null),Math.max(native)}) { 0.2-new Object().__lookupGetter__(eval(new Object())) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(Array(1).toLocaleString(), null.name().exec(undef.filter(false)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(true.filter(this).pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (break.lastIndex.superConstructor) { new Object().toString().length() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label (!0.2/debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ NaN.concat(new RegExp)+Join(1, false, new Object(), new Object(), x) : unescape(x).concat(Iterator(-1)) }) { 'a'.isNull.__lookupGetter__(this+native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.name()/IsPrimitive(this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {null}.prototype.value")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new true+false.__lookupGetter__(null&continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (-1.push(new RegExp)[void.valueOf()]) { new RegExp.className().__lookupGetter__(Array(0)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export NaN.__lookupGetter__(undef).__lookupGetter__(void.isNull)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ ~new RegExp.filter(undef&&this) : String(continue)<<NaN.toText }) { this.exec(this).length }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (true&void.exec(void.exec(continue)) in Join('a', undef, new Object(), continue, x) instanceof {undef}) { unescape(-1.prototype.name) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import void.push(true).join()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({break}&x.name(), 1.charAt(false).slice(continue.superConstructor, this&&break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (this.call(this) > Iterator(continue)) { new Object().prototype.value.slice(1.slice(native, -1), (!false)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export parseInt(new RegExp>>>=x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (escape(x==debugger), NaN.shift()&debugger?false:0.2, (!new RegExp)&goto break) { unescape(x.toText) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(throw new NaN.toObject()(this?break:true))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new (typeof(this))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape('a'/0) in ~new Object().lastIndex) { IsSmi(0).push(0.concat(0.2)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("(!new RegExp)[0.2 > new Object()].prototype.Number(debugger.join()) = native&-1.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new false.toJSONProtocol&&0.2.constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~0?0.2:undef in new RegExp.charCodeAt(0).prototype.name) { NaN.toLocaleString().splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~IsPrimitive(new RegExp), true.toString().size, null.charCodeAt('a') > null.concat(0)) { break.toJSONProtocol/IsPrimitive(break) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(new Object()).lastIndexOf(NaN > void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.splice()&&-1.prototype.new Object()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{{true,0}}.prototype.break.length.splice() = 'a'.toText.superConstructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (debugger>>>=continue > break.exec(1)) { Math.pow(new RegExp)==NaN>>>=0.2 }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2==0.2/goto true : IsSmi(native).isNull }) { throw new {x,null}(false.className()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {false.concat(null),Math.pow(NaN)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Array(null).add(NaN.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat(new Object()==true) in GetFunctionFor('a'&false)) { native&undef.toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new {eval(null),(debugger)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import {this.0,debugger.filter(NaN)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import break.charAt(-1)<<false.__defineSetter__(0,function(){x})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = goto false > new Object()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.superConstructor[debugger.isNull].prototype.Math.max('a').shift() = parseInt(0).size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native eval(void.add(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(x > void.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {this.toObject()} : Number(NaN).toJSONProtocol }) { 0.2.className().prototype.name }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (false.__defineGetter__(undef,function(){undef}).exec(NaN.splice())) { typeof(Join(void, new RegExp, break, -1, -1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (false.splice().toObject(), continue.name().size, Join(void?debugger:this, new RegExp.__defineSetter__(NaN,function(){NaN}), x.unshift(), this.true, parseInt(break))) { undef<<continue.toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (this.0.indexOf(break)) { break.charAt(this).unshift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Join(new Object().splice(), this instanceof 1, parseFloat(NaN), undef.concat(x), void.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(goto NaN.toString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 'a'<<break.shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Iterator(continue)[new Object()>>NaN]")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Join(new RegExp, 'a', this, void, true)>>>=continue>>native")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import new Object().toJSONProtocol.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return undef.__defineSetter__(native,function(){void}).toJSONProtocol }; X(eval(x).charCodeAt('a'.concat(true)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(throw new 0.2.__defineGetter__(NaN,function(){-1})(void&&new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 0.unshift() > IsSmi(NaN)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label x.call(null).lastIndex")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(IsSmi(0.2.add(0)), x.add(break).this.__defineGetter__(undef,function(){new RegExp}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native Number(this).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new NaN.shift().add(String(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new null.name().splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 1.undef.push(new Object().call(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(parseInt(1).size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = this.x.sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(continue.valueOf().prototype.new RegExp.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(this.charAt(continue)?undef+'a':unescape(1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({throw new 'a'(0.2),void.lastIndexOf(NaN)}, Math.pow(new Object().className()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (1.slice(new Object(), this).valueOf()) { parseInt(true).pop() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2.superConstructor.lastIndex : goto debugger<<Join(undef, 1, true, undef, debugger) }) { function () { NaN }.prototype.name }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("-1.exec(debugger).length.prototype.debugger > null.slice(Iterator(void), continue.concat(0)) = parseInt(throw new 1(1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(new Object().constructor.call(Number(1)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new null.unshift().call(escape(x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (Math.pow(native).toLocaleString()) { case (false instanceof native.join()): Math.pow(NaN).size; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label function () { new Object() }.prototype.true.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Join('a', 0.2, false, new Object(), void).continue.className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = IsPrimitive(break.__lookupGetter__(-1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Object()>>0.2.prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsPrimitive(new Object()).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (Array(parseInt(break))) { 'a'.toString().unshift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = return 0.2>>>=-1?undef:undef")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Object().splice().unshift().prototype.null&&native.__lookupGetter__(undef>>>=NaN) = (1<<break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete NaN.charAt(1).concat(NaN.0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(new RegExp.sort().toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return GetFunctionFor(false).lastIndexOf(1.shift()) }; X(this.0.2.charCodeAt(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto NaN.toObject(), ~true.'a', parseInt(debugger)+eval(false)) { eval(0.2.constructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(debugger).pop()) { case (this.push(true).valueOf()): Join(continue, debugger, native, native, debugger).filter(Array(continue)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new debugger.sort() instanceof this>>1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ parseFloat(false).prototype.(!new Object()) : {unescape(-1)} }) { Math.max(new RegExp.superConstructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate({Math.pow(break)})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import typeof(break.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Math.pow(-1[new RegExp]))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native IsPrimitive(1).concat({x,null})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("NaN.length.prototype.value.prototype.function () { null==new Object() } = break.name()&IsPrimitive(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete NaN.prototype.-1.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new continue.unshift()+parseFloat(undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new NaN-break.call(false.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native new RegExp.exec(break).pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({'a',null}.prototype.value, 1.shift() instanceof {'a',0})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (debugger.valueOf().size, function () { x.unshift() }, IsSmi(1)&&true==native) { new Object().__defineGetter__(this,function(){'a'})&&eval(native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export 'a'.pop().charCodeAt(x.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export String(IsSmi(debugger))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("typeof(debugger).valueOf().prototype.(1).lastIndexOf(this.break) = x.prototype.name.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native Array(typeof(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(1.__defineGetter__(1,function(){1}).null.constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = 1.charAt(0).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Math.max('a'.filter(new Object())))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(void.prototype.name.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (-1.toJSONProtocol.call(-1.size) in ~x.sort()) { eval(0&debugger) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ('a'==undef.join() in Math.pow(IsSmi(false))) { undef > this>>goto x }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate('a'.constructor.isNull)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (GetFunctionFor(this.slice(0.2, this)), this.prototype.void?null.unshift():native.className(), Number(new Object().call(-1))) { 0.splice() > debugger&&this }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {goto new RegExp,Join(new Object(), native, continue, -1, x)} : NaN&x/{0,break} }) { this.lastIndexOf(new RegExp).join() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (typeof(break.length())) { native&&false.sort() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new parseFloat(-1 instanceof break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label throw new continue.unshift()(null.shift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Math.max(0.2.toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return false.unshift().className() }; X(escape(NaN&NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Join(native.toText, goto x, 0.2.splice(), Join('a', 0, void, NaN, 1), eval(native)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (GetFunctionFor(true.prototype.name)) { parseInt(NaN).toLocaleString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new escape(native).__defineSetter__(return native,function(){undef > native})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new typeof(true > 'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (debugger.prototype.0.2<<new RegExp+false) { case (native.splice().filter({x})): false&true.indexOf(1.__defineGetter__(native,function(){continue})); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label true-NaN.prototype.native.shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new typeof(new RegExp.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (function () { this.NaN }) { case (this.continue.prototype.parseFloat(false)): IsPrimitive(new Object()-'a'); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.__lookupGetter__(debugger).indexOf(native.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (GetFunctionFor(NaN.lastIndex)) { case (new RegExp.lastIndex.toLocaleString()): NaN.join().indexOf(eval(-1)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {void.charAt(true)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new Object()==NaN.join()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(typeof(Array(new Object())))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label throw new (false)(eval(x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new RegExp.size.charAt(true > -1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger.toObject().charAt(this<<undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 'a'.valueOf()+parseInt(undef) : IsPrimitive(null).lastIndex }) { NaN.toObject().isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new new Object()&&void.lastIndexOf(0.2.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 1+1.name() : Join(Math.pow(debugger), new RegExp-1, x > 1, x<<-1, new RegExp.size) }) { undef[undef].size }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete native.call(-1).isNull")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (new Object()>>>=break==Math.pow(debugger)) { IsPrimitive(this).lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ((!x&&new RegExp) in undef.toLocaleString().slice(new RegExp.indexOf(NaN), IsPrimitive(-1))) { false.size+debugger[x] }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import 0.length.__defineGetter__(0.2.shift(),function(){'a'.className()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(goto new Object().push(void))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Array(this.0) : parseFloat(void).pop() }) { escape(true).slice(continue.lastIndex, false.toObject()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new native==true.filter({NaN,-1})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ('a'.__defineSetter__(continue,function(){-1}).unshift(), Array(undef).toLocaleString(), undef.__lookupGetter__(void).toLocaleString()) { parseInt(false/native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.x<<false.prototype.true.toLocaleString()==NaN.pop() = this.superConstructor>>Math.max(true)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return this.prototype.name.splice() }; X(unescape(x).__lookupGetter__(Number(debugger)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (!NaN).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(escape(Iterator(this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Number(new RegExp)<<this?true:-1 }; X(Number(null).lastIndex)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export this.void.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (this.prototype.null.sort() in -1.className()&void.filter(new Object())) { GetFunctionFor(new Object()).pop() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 0[break].sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (null.length().toString(), eval(-1).toObject(), (!continue.concat(continue))) { true.name()/native<<new RegExp }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape(null).sort(), Number(undef).charCodeAt(IsPrimitive(NaN)), null>>true/null.join()) { 0.2.toObject() > IsPrimitive(new RegExp) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date({NaN,native}&&1+undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(IsPrimitive(undef>>>=1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Join(true, 'a', true, 1, NaN).add({1}), GetFunctionFor(new Object().push(new Object())), goto 1.length) { Math.pow(GetFunctionFor(native)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return break.isNull > parseInt(continue) }; X((new RegExp instanceof 1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Number(false).indexOf(x instanceof new Object()) : function () { x.toString() } }) { false.name().indexOf(GetFunctionFor(null)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date('a'.constructor.prototype.name)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("GetFunctionFor(void&new Object()).prototype.debugger.add(null)[void.unshift()] = new RegExp.isNull.Iterator(this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete false?break:undef.constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ (native.filter(1)) : eval(this&&0.2) }) { undef.length instanceof new Object().toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export String(break.lastIndexOf(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label (!Iterator(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(String(null==-1), {1&0})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(parseInt('a' > 0))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(debugger.toJSONProtocol.indexOf(escape(0)), this.filter(null).__defineSetter__(continue.break,function(){debugger>>null}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.name().length().prototype.goto false.exec(true.charCodeAt(continue)) = Join(-1-false, undef.superConstructor, 'a'.shift(), (!x), NaN.this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(typeof(new RegExp).sort())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0.2.concat(x).splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto void.indexOf(throw new x(1)), typeof(return new RegExp), IsPrimitive(-1).add(void.lastIndexOf(debugger))) { null.indexOf(void).toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("return new RegExp.pop().prototype.String(x.toObject()) = 1.superConstructor.charCodeAt(new RegExp.charCodeAt(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new null&true.prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1>>>=NaN.indexOf((debugger))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new parseFloat(null).splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import -1.lastIndexOf(new RegExp) instanceof throw new void(0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if ((0.shift())) { Join(IsPrimitive(-1), break.__defineSetter__(true,function(){break}), parseInt(null), parseFloat(break), true/null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new escape(1 > continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(undef)>>false.filter(continue)) { case (this.undef/new Object()): 'a'.toJSONProtocol.__defineGetter__(new RegExp-undef,function(){parseFloat(new RegExp)}); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{void}.shift().prototype.this.Array(new Object()) = {0.2,new RegExp}.lastIndexOf(break.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new continue&&new Object().lastIndexOf(new Object() instanceof 1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (throw new 'a'.exec(x)(return false), native/void.constructor, {native}==true.toLocaleString()) { goto 1 instanceof 1.isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (break.concat(break) > native>>>=-1, (debugger.x), Join(x, void, void, new RegExp, null).name()) { void.charCodeAt(true).valueOf() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new 'a'>>0 instanceof new Object().push(new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (return ~break) { break.__defineGetter__(break,function(){-1}).shift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Join(null, -1, undef, null, 0).toString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ({new RegExp,void}.slice(break.isNull, false.shift())) { eval(debugger.slice(this, 1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return {GetFunctionFor(0)} }; X('a'.prototype.debugger.concat(void.constructor))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (~true instanceof continue) { escape(new RegExp.toObject()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("escape(0[native]).prototype.debugger.add(1).unshift() = (true.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape(void).length, undef.toObject() instanceof x.toObject(), 0.2+true.concat(true.__lookupGetter__(this))) { (x).toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(escape(null).__lookupGetter__(undef.size))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label Array(continue[false])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Number(this&&false) }; X(NaN.toJSONProtocol.toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.toString().shift().prototype.Array(x).__lookupGetter__('a'.prototype.x) = {1.length,break.join()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 1.charCodeAt(break)+IsSmi(false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(String(this) > 0.2.toText, new RegExp.length.lastIndexOf(1<<0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (new RegExp.pop().charAt(IsSmi(new RegExp))) { case (native.indexOf(this)/native.lastIndex): this.debugger.indexOf(debugger); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Number(x)[debugger.prototype.break])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new RegExp>>>=x.unshift() }; X(Math.max(continue.name()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(IsSmi(null.size))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = native?0.2:1+GetFunctionFor(void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (IsPrimitive(-1)>>>=break.valueOf() in String(0 > 0.2)) { Math.max(true.length()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (escape(unescape(NaN))) { case (Math.pow(eval(undef))): true.charAt(null)&new RegExp.pop(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete Join(new RegExp, 1, false, new Object(), this).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label return x.filter(x.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new RegExp.pop().shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (!debugger.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label Math.max(debugger.__lookupGetter__(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(eval(debugger[debugger]))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0.2.filter(true)&throw new true(debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(continue.exec(debugger) > Math.pow(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("void.prototype.value.name().prototype.Number(undef&NaN) = false.__lookupGetter__(-1).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(null.__defineGetter__(native,function(){continue}).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {new Object()[continue],native.length()} : undef.name().superConstructor }) { Math.pow(break).indexOf(0.toJSONProtocol) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (Iterator(native.call(new RegExp))) { case (String(new RegExp).isNull): goto new RegExp.pop(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new x.constructor instanceof undef.indexOf(-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(this.~null, continue.pop()&0&'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (GetFunctionFor(~0)) { case ('a'.'a'<<undef.__defineGetter__(false,function(){true})): (!1).lastIndex; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return debugger.unshift().0.toString() }; X(Number(break).0.2>>>=false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Iterator(x)/undef.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(undef.join().toLocaleString(), null.add(false).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("IsSmi(x).toString().prototype.0>>continue.indexOf(NaN.__lookupGetter__(new Object())) = ~-1&typeof(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.__lookupGetter__(new RegExp).toObject(), false-0.toString(), return native.sort()) { new RegExp.name().className() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (escape(new RegExp).toString()) { case (goto eval(1)): this.filter(new Object()).call(new RegExp.slice(null, this)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger-false.toText")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Number(null>>new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete this&native.indexOf('a'.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(~Math.max(break), 0.2.valueOf().length)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Number(native.charCodeAt(x)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new goto continue.add(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete typeof(debugger).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("'a'<<false.toText.prototype.throw new true(1).lastIndex = 'a'.name().length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native 'a'.indexOf(debugger).charAt(NaN.add(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(break>>false.toString(), (false.indexOf(this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete goto NaN==(!debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(0.2.join().superConstructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.void.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(x.exec(debugger)[GetFunctionFor(0)], native.toObject().exec(new RegExp.sort()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(0.2.valueOf().toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(-1.toJSONProtocol.prototype.name)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Array(-1.shift()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.concat(undef).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native parseFloat(-1)?NaN.toText:debugger.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (void-continue/continue.prototype.undef in String(break.toText)) { parseInt(false).isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(true.isNull.toObject())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ typeof(debugger).toObject() : x.constructor>>>=null.__defineGetter__(native,function(){debugger}) }) { unescape(undef.lastIndexOf(false)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export unescape(continue)<<native[0]")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (String(0).unescape(debugger)) { {break.pop(),0.2.constructor} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("String({true}).prototype.break.length.call(false > 0.2) = GetFunctionFor(0.prototype.new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ false.push(0.2).indexOf(Math.max(debugger)) : x&x.prototype.name }) { goto 1.lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2.lastIndex&0.2?break:NaN)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1.prototype.value.toText")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import native.toLocaleString()-1.prototype.0")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export debugger[-1].indexOf(Join(new Object(), 0, x, new Object(), 0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return (!true).lastIndexOf(true.splice()) }; X(NaN.toString().prototype.value)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return continue.slice(-1, 1).prototype.true.name() }; X('a'.push(void).prototype.value)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto new RegExp.length(), x.sort().className(), Math.max(new RegExp.toJSONProtocol)) { (IsSmi(-1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 0.splice()&&-1.sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (Math.max(-1>>1)) { break.toLocaleString().toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {void.prototype.break,new RegExp.toString()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new IsSmi(debugger).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 'a'.concat(undef).sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new {debugger.toObject(),'a' > false}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (goto 1.concat(Join(x, undef, native, x, new Object()))) { new RegExp.prototype.name==new RegExp.superConstructor }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new Object().__defineGetter__(0.2,function(){0.2}).length() }; X(void.isNull<<parseFloat(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete continue.toJSONProtocol.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.constructor.toObject() in true&&undef.toJSONProtocol) { String(0+break) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import true.call(continue)>>break.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label escape(this) > Math.pow(new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {void}/IsSmi(new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (native==null?debugger.prototype.name:null.toLocaleString()) { case (NaN.push(this).join()): (break instanceof continue); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Math.pow(x.push(0))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new (Array(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label IsSmi(new RegExp).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label NaN.push(1).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{escape(undef),debugger.filter(0.2)}.prototype.-1 > new RegExp[0.2.valueOf()] = new RegExp.prototype.value.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Join(0.2, x, continue, debugger, new Object()).size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Number(null).name() : Math.pow(true).__defineGetter__(debugger.toString(),function(){false+0.2}) }) { this.{x,break} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.pow(goto debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = IsPrimitive(void.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Object().toString().toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(this.String(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ({-1.call(new RegExp)}) { break.length().splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import null.size.__defineGetter__(void.filter(x),function(){null.pop()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsPrimitive(null.superConstructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new eval(-1.prototype.continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (typeof(Iterator('a'))) { case (0.constructor>>~1): void.__defineGetter__(void,function(){1})/GetFunctionFor(0); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (false instanceof x.add(true.charAt(new RegExp)) in Join(undef.lastIndexOf(break), 0.2.add(new Object()), Iterator(1), {'a',x}, Array(new Object()))) { function () { null }/1&&-1 }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new escape('a'.concat(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(Math.pow(NaN).toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new throw new 0(NaN).className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete String(GetFunctionFor(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Iterator(new Object()).charAt((0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Number(undef.charAt(1)).prototype.undef.lastIndexOf(true).slice(1.className(), undef.filter(-1)) = null<<null.push(parseInt('a'))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {Math.max(1),IsSmi(new Object())}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (new Object().exec(0).isNull) { case (escape(IsSmi(false))): false.toObject()-null.size; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 'a'.__defineSetter__(debugger,function(){false}).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger?-1:0+true.prototype.1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {false instanceof continue,native.size}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("GetFunctionFor(continue.__lookupGetter__(0.2)).prototype.Math.max(1.splice()) = true.__defineGetter__(undef,function(){NaN}).filter(String(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.size-1.toLocaleString().prototype.(this).shift() = GetFunctionFor(native.charAt(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate((!null.indexOf(-1)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {break.sort()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new throw new debugger.splice()(this.__lookupGetter__(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("unescape(x[native]).prototype.0.splice().-1.prototype.true = x.prototype.value.className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export x+true.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export debugger.indexOf(-1).indexOf(true.constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ({break}.exec(new Object().continue) in eval(0.2.charAt(new Object()))) { throw new null.length(null?break:-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = NaN.toLocaleString().toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Math.pow(break+false) }; X(Join(true.add(new Object()), null[-1], new RegExp[true], NaN&&debugger, x.charAt(undef)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch ((break).add(true.sort())) { case (undef.charAt(native).__defineGetter__(IsPrimitive(1),function(){NaN<<new RegExp})): -1.__defineSetter__(null,function(){-1}) > this.charCodeAt(this); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import return 0.2.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("continue.join().toText.prototype.Number(debugger).slice(new RegExp.-1, (NaN)) = function () { (!null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Number(break.__lookupGetter__(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(return null/x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Number(undef).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 1[native]/this&true")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete typeof(debugger.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import x.charAt(false)&-1>>x")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (null.toText.superConstructor) { typeof(-1).toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (parseFloat(continue.superConstructor)) { 0.2.toText.prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label parseInt(IsSmi(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete new Object().valueOf().indexOf(true-x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new unescape(1.__defineGetter__(new Object(),function(){x}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (undef.size.splice()) { 1.constructor.charCodeAt(0+'a') }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.new RegExp.pop().prototype.eval(debugger).toJSONProtocol = unescape(continue).valueOf()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.new RegExp.indexOf(unescape(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new break instanceof false instanceof native.length()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(parseFloat(x).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label {escape(true),Math.max(null)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("'a'>>>=void.prototype.value.prototype.break.prototype.break.indexOf(0.className()) = (!this&native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Number(NaN).push(IsSmi(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export true.exec(void).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function({'a',true}/eval(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label null.concat(null).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {0.2.length,new RegExp.lastIndexOf(-1)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Math.max({0.2}) }; X(true.charCodeAt(null).add(new RegExp.name()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete -1.lastIndex.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2[1].call(true > break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate('a'.toLocaleString().splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = typeof(void&&void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1199637.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1199637.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1199637.js)  
```javascript
var NONE = 0;
var READ_ONLY = 1;

function AddNamedProperty(object, name, value, attrs) {
  Object.defineProperty(object, name, {
      value,
      configurable: true,
      enumerable: true,
      writable: (attrs & READ_ONLY) === 0
  });
}


AddNamedProperty(this.__proto__, "a", 1234, NONE);
assertEquals(1234, a);
eval("var a = 5678;");
assertEquals(5678, a);

AddNamedProperty(this.__proto__, "b", 1234, NONE);
assertEquals(1234, b);
eval("var b = 5678;");
assertEquals(5678, b);

AddNamedProperty(this.__proto__, "c", 1234, READ_ONLY);
assertEquals(1234, c);
eval("var c = 5678;");
assertEquals(5678, c);

AddNamedProperty(this.__proto__, "d", 1234, READ_ONLY);
assertEquals(1234, d);
eval("var d = 5678;");
assertEquals(5678, d);


AddNamedProperty(this.__proto__, "x", 1234, NONE);
assertEquals(1234, x);
eval("with({}) { var x = 5678; }");
assertEquals(5678, x);

AddNamedProperty(this.__proto__, "y", 1234, NONE);
assertEquals(1234, y);
eval("with({}) { var y = 5678; }");
assertEquals(5678, y);

AddNamedProperty(this.__proto__, "z", 1234, READ_ONLY);
assertEquals(1234, z);
eval("with({}) { var z = 5678; }");
assertEquals(5678, z);

AddNamedProperty(this.__proto__, "w", 1234, READ_ONLY);
assertEquals(1234, w);
eval("with({}) { var w = 5678; }");
assertEquals(5678, w);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1199401.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1199401.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1199401.js)  
```javascript
var ranges = [{min: -1073741824, max: 1073741823, bits: 31},
              {min: -2147483648, max: 2147483647, bits: 32}];

for (var i = 0; i < ranges.length; i++) {
  var range = ranges[i];
  var min_smi = range.min;
  var max_smi = range.max;
  var bits = range.bits;
  var name = bits + "-bit";

  var result = max_smi + 1;

  
  assertEquals(result, eval(min_smi + " * -1"), name + "-litconmult");
  assertEquals(result, eval(min_smi + " / -1"), name + "-litcondiv");
  assertEquals(result, eval("-(" + min_smi + ")"), name + "-litneg");
  assertEquals(result, eval("0 - (" + min_smi + ")")), name + "-conlitsub";

  
  assertEquals(result, min_smi * -1, name + "-varconmult");
  assertEquals(result, min_smi / -1, name + "-varcondiv");
  assertEquals(result, -min_smi, name + "-varneg");
  assertEquals(result, 0 - min_smi, name + "-convarsub");

  
  var zero = 0;
  var minus_one = -1;

  assertEquals(result, min_smi * minus_one, name + "-varvarmult");
  assertEquals(result, min_smi / minus_one, name + "-varvardiv");
  assertEquals(result, zero - min_smi, name + "-varvarsub");

  
  assertEquals(result, eval(min_smi + " * minus_one"), name + "-litvarmult");
  assertEquals(result, eval(min_smi + " / minus_one"), name + "-litvarmdiv");
  assertEquals(result, eval("0 - (" + min_smi + ")"), name + "-varlitsub");

  var half_min_smi = -(1 << (bits >> 1));
  var half_max_smi = 1 << ((bits - 1) >> 1);

  assertEquals(max_smi + 1, -half_min_smi * half_max_smi, name + "-half1");
  assertEquals(max_smi + 1, half_min_smi * -half_max_smi, name + "-half2");
  assertEquals(max_smi + 1, half_max_smi * -half_min_smi, name + "-half3");
  assertEquals(max_smi + 1, -half_max_smi * half_min_smi, name + "-half4");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1187524.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1187524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1187524.js)  
```javascript
assertEquals(undefined, ""[0x40000000]);
assertEquals(undefined, ""[0x80000000]);
assertEquals(undefined, ""[-1]);
assertEquals(undefined, ""[-0x40000001]);
assertEquals(undefined, ""[-0x80000000]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1178598.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1178598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1178598.js)  
```javascript
var value = (function() {
  var result;
  try {
    throw 42;
  } catch (e) {
    result = eval("e");
  }
  return result;
})();

assertEquals(42, value);






var value = (function() {
  var result;
  try {
    throw 87;
  } catch(e) {
    
    
    (function() { e; });
    result = eval("e");
  }

  
  
  try {
    eval("e");
    assertTrue(false);  
  } catch(exception) {
    assertTrue(exception instanceof ReferenceError);
    return result;
  }
})();

assertEquals(87, value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1177809.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1177809.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1177809.js)  
```javascript
String.fromCharCode(48,48,48,59,32,102,111,110,116,45,119,101,105,103,104,116,58,98,111,108,100,59,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,32,99,111,108,111,114,61,34,35,70,70,48,48,48,48,34,62,70,79,82,69,88,47,80,65,82,38,35,51,48,52,59,60,119,98,114,32,47,62,84,69,32,38,35,51,48,52,59,38,35,51,53,48,59,76,69,77,76,69,82,38,35,51,48,52,59,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,115,112,97,110,32,105,100,61,34,97,99,95,100,101,115,99,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,49,112,120,59,32,99,111,108,111,114,58,35,48,48,48,48,48,48,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,38,112,111,117,110,100,59,47,36,32,50,32,112,105,112,44,32,89,84,76,32,49,50,32,112,105,112,44,65,108,116,38,35,51,48,53,59,110,32,51,32,99,101,110,116,46,32,83,97,98,105,116,32,83,112,114,101,97,100,45,84,38,117,117,109,108,59,114,60,119,98,114,32,47,62,107,32,66,97,110,107,97,115,38,35,51,48,53,59,32,65,86,65,78,84,65,74,73,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,100,105,118,32,105,100,61,34,97,99,95,117,114,108,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,48,112,120,59,32,99,111,108,111,114,58,35,70,70,54,54,57,57,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,119,119,119,46,104,101,100,101,102,111,60,119,98,114,32,47,62,110,108,105,110,101,46,99,111,109,60,47,102,111,110,116,62,60,47,100,105,118,62,60,47,116,100,62,60,47,116,114,62,60,47,116,97,98,108,101,62,60,47,116,100,62,60,47,116,114,62,60,116,114,62,10,60,116,100,32,99,108,97,115,115,61,34,97,99,95,107,97,114,105,109,34,32,104,101,105,103,104,116,61,34,50,48,37,34,32,98,103,99,111,108,111,114,61,34,35,70,70,70,70,70,70,34,32,105,100,61,34,116,97,119,52,34,32,97,108,105,103,110,61,34,108,101,102,116,34,32,118,97,108,105,103,110,61,34,109,105,100,100,108,101,34,32,111,110,70,111,99,117,115,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,39,44,39,97,119,52,39,41,34,32,111,110,77,111,117,115,101,79,118,101,114,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,39,44,39,97,119,52,39,41,34,32,32,111,110,77,111,117,115,101,79,117,116,61,34,99,115,40,41,34,32,111,110,67,108,105,99,107,61,34,103,97,40,39,104,116,116,112,58,47,47,97,100,115,101,114,118,101,114,46,109,121,110,101,116,46,99,111,109,47,65,100,83,101,114,118,101,114,47,99,108,105,99,107,46,106,115,112,63,117,114,108,61,56,56,49,48,48,50,53,49,50,49,55,54,51,57,52,54,50,51,49,56,52,52,48,51,57,54,48,48,54,51,49,51,54,54,52,52,56,50,56,54,50,48,49,49,49,52,55,51,55,54,52,51,50,57,50,52,50,56,51,53,56,51,54,53,48,48,48,48,53,56,49,55,50,56,57,53,48,48,52,49,57,48,54,56,56,55,50,56,49,55,48,55,53,48,57,50,55,53,55,57,57,51,54,53,50,52,54,49,51,56,49,57,53,55,52,53,50,49,52,50,55,54,48,57,53,57,56,52,55,50,55,48,56,52,51,49,54,52,49,54,57,53,48,56,57,50,54,54,54,48,57,49,54,53,55,57,48,57,49,55,57,52,55,52,55,57,50,48,55,50,55,51,51,53,51,50,55,53,50,54,55,50,56,48,51,57,49,56,54,50,56,55,49,51,55,48,52,51,49,51,52,55,56,51,54,51,52,53,50,54,55,53,57,48,57,48,56,54,57,49,52,53,49,49,52,55,53,50,120,49,57,50,88,49,54,56,88,51,56,88,52,49,88,56,48,56,48,88,65,39,41,34,32,115,116,121,108,101,61,34,99,117,114,115,111,114,58,112,111,105,110,116,101,114,34,62,10,60,116,97,98,108,101,32,119,105,100,116,104,61,34,49,53,54,34,32,98,111,114,100,101,114,61,34,48,34,32,99,101,108,108,115,112,97,99,105,110,103,61,34,49,34,32,99,101,108,108,112,97,100,100,105,110,103,61,34,49,34,62,10,60,116,114,62,10,32,32,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,32,62,60,115,112,97,110,32,105,100,61,34,97,99,95,116,105,116,108,101,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,50,112,120,59,32,99,111,108,111,114,58,35,70,70,48,48,48,48,59,32,102,111,110,116,45,119,101,105,103,104,116,58,98,111,108,100,59,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,32,99,111,108,111,114,61,34,35,70,70,48,48,48,48,34,62,66,108,117,101,32,72,111,117,115,101,32,77,105,107,115,101,114,39,100,101,32,38,35,51,53,48,59,111,107,33,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,115,112,97,110,32,105,100,61,34,97,99,95,100,101,115,99,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,49,112,120,59,32,99,111,108,111,114,58,35,48,48,48,48,48,48,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,66,108,117,101,32,72,111,117,115,101,32,77,105,107,115,101,114,39,100,101,32,65,110,110,101,108,101,114,101,32,38,79,117,109,108,59,122,101,108,32,70,105,121,97,116,32,83,65,68,69,67,69,32,50,57,44,57,54,32,89,84,76,33,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,100,105,118,32,105,100,61,34,97,99,95,117,114,108,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,48,112,120,59,32,99,111,108,111,114,58,35,70,70,54,54,57,57,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,60,47,102,111,110,116,62,60,47,100,105,118,62,60,47,116,100,62,60,47,116,114,62,60,47,116,97,98,108,101,62,60,47,116,100,62,60,47,116,114,62,60,116,114,62,10,60,116,100,32,99,108,97,115,115,61,34,97,99,95,107,97,114,105,109,34,32,104,101,105,103,104,116,61,34,50,48,37,34,32,98,103,99,111,108,111,114,61,34,35,70,70,70,70,70,70,34,32,105,100,61,34,116,97,119,53,34,32,97,108,105,103,110,61,34,108,101,102,116,34,32,118,97,108,105,103,110,61,34,109,105,100,100,108,101,34,32,111,110,70,111,99,117,115,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,98,105,116,109,101,100,60,119,98,114,32,47,62,101,110,46,99,111,109,39,44,39,97,119,53,39,41,34,32,111,110,77,111,117,115,101,79,118,101,114,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,98,105,116,109,101,100,60,119,98,114,32,47,62,101,110,46,99,111,109,39,44,39,97,119,53,39,41,34,32,32,111,110,77,111,117,115,101,79,117,116,61,34,99,115,40,41,34,32,111,110,67,108,105,99,107,61,34,103,97,40,39,104,116,116,112,58,47,47,97,100,115,101,114,118,101,114,46,109,121,110,101,116,46,99,111,109,47,65,100,83,101,114,118,101,114,47,99,108,105,99,107,46,106,115,112,63,117,114,108,61,51,51,54,49,55,53,56,50,56,51,56,50,53,52,57,55,54,49,48)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1177518.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1177518.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1177518.js)  
```javascript
isFinite = 0;
Math.floor = 0;
Math.abs = 0;


assertEquals(4, parseInt(4.5));


assertEquals('string', typeof (new Date(9999)).toString());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1175390.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1175390.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1175390.js)  
```javascript
a = 0;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1173979.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1173979.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1173979.js)  
```javascript
var null_var = null;
var undef_var = [][0];
var boolean_var = false;
var number_var = 0;
var string_var = "";
var object_var = { foo : 0 };

assertTrue(null_var == null_var);
assertTrue(null_var == undef_var);
assertTrue(null_var != boolean_var);
assertTrue(null_var != number_var);
assertTrue(null_var != string_var);
assertTrue(null_var != object_var);

assertTrue(undef_var == null_var);
assertTrue(boolean_var != null_var);
assertTrue(number_var != null_var);
assertTrue(string_var != null_var);
assertTrue(object_var != null_var);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1114040.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1114040.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1114040.js)  
```javascript
function TestBreak() {
  var sequence = "";
  for (var a in [0,1]) {
    L: {
      for (var b in [2,3,4]) {
        break L;
      }
    }
    sequence += a;
  }
  return sequence;
}


function TestContinue() {
  var sequence = "";
  for (var a in [0,1]) {
    L: do {
      for (var b in [2,3,4]) {
        continue L;
      }
    } while (false);
    sequence += a;
  }
  return sequence;
}


assertEquals("01", TestBreak());
assertEquals("01", TestContinue());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1112051.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1112051.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1112051.js)  
```javascript
function f() { }
assertThrows("f.call.apply()");
assertThrows("f.call.apply(null)");
assertThrows("f.call.apply(null, [], 0)");
assertThrows("f.call.apply(null, [1,2,3,4,5,6,7,8,9], 0)");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1110164.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1110164.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1110164.js)  
```javascript
var o = { x: 0, f: function() { return 42; } };
delete o.x;  

function CallF(o) {
  return o.f();
}


for (var i = 0; i < 10; i++) assertEquals(42, CallF(o));

var caught = false;
o.f = 87;
try {
  CallF(o);
} catch (e) {
  caught = true;
  assertTrue(e instanceof TypeError);
}
assertTrue(caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1102760.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1102760.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1102760.js)  
```javascript
function F() {
  return arguments.length;
}

assertEquals(0, F.apply(), "no receiver or args");
assertEquals(0, F.apply(this), "no args");
assertEquals(0, F.apply(this, []), "empty args");
assertEquals(0, F.apply(this, [], 0), "empty args, extra argument");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1066899.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1066899.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1066899.js)  
```javascript
function Crash() {
  for (var key in [0]) {
    try { } finally { continue; }
  }
}

Crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1062422.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1062422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1062422.js)  
```javascript
Number.prototype.__proto__ = String.prototype;
assertEquals((123).length, 0)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1050043.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1050043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1050043.js)  
```javascript
function unsignedShiftRight(val, shift) {
  return val >>> shift;
}

assertEquals(        15, unsignedShiftRight(15, 0), "15 >>> 0");
assertEquals(         7, unsignedShiftRight(15, 1), "15 >>> 1");
assertEquals(         3, unsignedShiftRight(15, 2), "15 >>> 2");

assertEquals(4294967288, unsignedShiftRight(-8, 0), "-8 >>> 0");
assertEquals(2147483644, unsignedShiftRight(-8, 1), "-8 >>> 1");
assertEquals(1073741822, unsignedShiftRight(-8, 2), "-8 >>> 2");

assertEquals(         1, unsignedShiftRight(-8, 31), "-8 >>> 31");
assertEquals(4294967288, unsignedShiftRight(-8, 32), "-8 >>> 32");
assertEquals(2147483644, unsignedShiftRight(-8, 33), "-8 >>> 33");
assertEquals(1073741822, unsignedShiftRight(-8, 34), "-8 >>> 34");

assertEquals(2147483648, unsignedShiftRight(0x80000000, 0), "0x80000000 >>> 0");
assertEquals(1073741824, unsignedShiftRight(0x80000000, 1), "0x80000000 >>> 1");
assertEquals( 536870912, unsignedShiftRight(0x80000000, 2), "0x80000000 >>> 2");

assertEquals(1073741824, unsignedShiftRight(0x40000000, 0), "0x40000000 >>> 0");
assertEquals( 536870912, unsignedShiftRight(0x40000000, 1), "0x40000000 >>> 1");
assertEquals( 268435456, unsignedShiftRight(0x40000000, 2), "0x40000000 >>> 2");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1039610.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1039610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1039610.js)  
```javascript
assertTrue(typeof(Debug) === 'undefined');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1036894.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1036894.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1036894.js)  
```javascript
assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");

function foo() {
  assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");
}
foo();

assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");

xeval = function(s) { eval(s); }
xeval('$=function(){L: {break L;break L;}};');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-1030466.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1030466.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1030466.js)  
```javascript
var result = (function outer() {
 with ({}) { }
 var x = 10;
 function inner() {
   return x;
 };
 return inner();
})();

assertEquals(10, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-996542.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-996542.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996542.js)  
```javascript
var zero = 0;
var one = 1;
var minus_one = -1;

assertEquals(-Infinity, 1 / (0 / -1));
assertEquals(-Infinity, one / (zero / minus_one));
assertEquals(Infinity, 1 / (0 / 1));
assertEquals(Infinity, one / (zero / one));

assertEquals(-Infinity, 1 / (-1 % 1));
assertEquals(-Infinity, one / (minus_one % one))
assertEquals(Infinity, 1 / (1 % 1));
assertEquals(Infinity, one / (one % one));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-992733.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-992733.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-992733.js)  
```javascript
assertEquals("object", typeof this);
var threw = false;
try {
  this();
} catch (e) {
  threw = true;
}
assertTrue(threw);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-990205.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-990205.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-990205.js)  
```javascript
function f() {
  
  
  return eval("while(0) function x() { break; }; 42");
};

assertThrows("f()");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-937896.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-937896.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-937896.js)  
```javascript
function f() {
  try {
    for (var i = 0; i < 2; i++) {
      continue;
      try {
        continue;
        continue;
      } catch (ex) {
        
      }
    }
  } catch (e) {
    
  }
  return 42;
}


assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   

## **regress file:regress-925537.js**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-925537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-925537.js)  
```javascript
function assertClose(expected, actual) {
  var delta = 0.00001;
  if (Math.abs(expected - actual) > delta) {
    print('Failure: Expected <' + actual + '> to be close to <' +
          expected + '>');
  }
}

assertEquals(1, Math.pow(NaN, 0));
var pinf = Number.POSITIVE_INFINITY, ninf = Number.NEGATIVE_INFINITY;
assertClose( Math.PI / 4, Math.atan2(pinf, pinf));
assertClose(-Math.PI / 4, Math.atan2(ninf, pinf));
assertClose( 3 * Math.PI / 4, Math.atan2(pinf, ninf));
assertClose(-3 * Math.PI / 4, Math.atan2(ninf, ninf));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
---   
