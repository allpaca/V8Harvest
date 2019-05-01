# V8Harvest  
The Harvest of V8 regress in 2016.  
  

## **regress-5783.js (v8 issue)**  
   
**[Issue 5783:
 Some runtime functions erroneously use SealHandleScope](https://crbug.com/v8/5783)**  
**[Commit: Fix SealHandleScope usage in runtime-classes.cc](https://chromium.googlesource.com/v8/v8/+/2454737)**  
  
Date(Commit): Tue Dec 27 18:55:16 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2603783003](https://codereview.chromium.org/2603783003)  
Regress: [mjsunit/regress/regress-5783.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5783.js)  
```javascript
class C {}
class D extends C { constructor(...args) { super(...args, 75) } }
D.__proto__ = null;
assertThrows(() => new D(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2454737^!)  
[src/runtime/runtime-classes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-classes.cc?cl=2454737)  
[test/mjsunit/regress/regress-5783.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5783.js?cl=2454737)  
  

---   

## **regress-5780.js (v8 issue)**  
   
**[Issue 5780:
 Object.prototype.toString doesn't look up @@toStringTag in some cases](https://crbug.com/v8/5780)**  
**[Commit: Object.prototype.toString must reflect mutated @@toStringTag values for primitives](https://chromium.googlesource.com/v8/v8/+/23019c4)**  
  
Date(Commit): Tue Dec 27 17:57:38 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2597323002](https://codereview.chromium.org/2597323002)  
Regress: [mjsunit/regress/regress-5780.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5780.js)  
```javascript
function testMutatedPrimitiveToStringTag(primitive) {
  Object.defineProperty(
    primitive.__proto__, Symbol.toStringTag,
    {value: "bogus", configurable: true, writable: false, enumerable: false});
  assertEquals("[object bogus]", Object.prototype.toString.call(primitive));
}

testMutatedPrimitiveToStringTag('');
testMutatedPrimitiveToStringTag(true);
testMutatedPrimitiveToStringTag(42);
testMutatedPrimitiveToStringTag(42.42);
testMutatedPrimitiveToStringTag(Symbol());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/23019c4^!)  
[src/builtins/builtins-object.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object.cc?cl=23019c4)  
[test/mjsunit/regress/regress-5780.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5780.js?cl=23019c4)  
  

---   

## **regress-4870.js (v8 issue)**  
   
**[Issue 4870:
 Runtime error accessible from Intl code](https://crbug.com/v8/4870)**  
**[Commit: [intl] Add new semantics + compat fallback to Intl constructor](https://chromium.googlesource.com/v8/v8/+/b0a09d7)**  
  
Date(Commit): Fri Dec 23 14:32:16 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2582993002](https://codereview.chromium.org/2582993002)  
Regress: [mjsunit/regress/regress-4870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4870.js)  
```javascript
if (this.Intl) {
  assertThrows(() => Object.getOwnPropertyDescriptor(Intl.Collator.prototype,
                                                     'compare')
                     .get.call(new Intl.DateTimeFormat())('a', 'b'),
               TypeError)
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0a09d7^!)  
[src/heap-symbols.h](https://cs.chromium.org/chromium/src/v8/src/heap-symbols.h?cl=b0a09d7)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=b0a09d7)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=b0a09d7)  
[test/cctest/interpreter/bytecode_expectations/ForOf.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForOf.golden?cl=b0a09d7)  
[test/cctest/interpreter/bytecode_expectations/Generators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Generators.golden?cl=b0a09d7)  
...  
  

---   

## **regress-v8-5756.js (v8 issue)**  
   
**[Issue 5756:
 Illegal instruction with testcase](https://crbug.com/v8/5756)**  
**[Commit: [turbofan] Optimize store to typed arrays only if the value is plain primitive.](https://chromium.googlesource.com/v8/v8/+/e92118b)**  
  
Date(Commit): Fri Dec 23 14:29:00 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2596843002](https://codereview.chromium.org/2596843002)  
Regress: [mjsunit/compiler/regress-v8-5756.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-5756.js)  
```javascript
z = {};
t = new Uint8Array(3);
var m = 0;
var x = 10;

function k() {
  ++m;
  if (m % 10 != 9) {
    if (m > 20) // This if is to just force it to deoptimize.
      x = '1';  // this deoptimizes during the second invocation of k.
                // This causes two deopts, one eager at x = 1 and the
                // other lazy at t[2] = z.
     t[2] = z; // since we are assigning to Uint8Array, ToNumber
              // is called which calls this funciton again.
  }
}

function f1() {
  %PrepareFunctionForOptimization(k);
  z.toString = k;
  z.toString();
  z.toString();
  %OptimizeFunctionOnNextCall(k);
  z.toString();
}
f1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e92118b^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=e92118b)  
[test/mjsunit/compiler/regress-v8-5756.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-v8-5756.js?cl=e92118b)  
[test/unittests/compiler/js-typed-lowering-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-typed-lowering-unittest.cc?cl=e92118b)  
  

---   

## **regress-5772.js (v8 issue)**  
   
**[Issue 5772:
 Check failed: !value->IsTheHole(isolate).](https://crbug.com/v8/5772)**  
**[Commit: [ic] Always use generic ICs for growing element stores on arguments](https://chromium.googlesource.com/v8/v8/+/f739730)**  
  
Date(Commit): Thu Dec 22 14:10:51 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2597013004](https://codereview.chromium.org/2597013004)  
Regress: [mjsunit/regress/regress-5772.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5772.js)  
```javascript
(function sloppyPackedArguments() {
  function f(a) {
    for (var i = 0; i < 2; i++) {
      a[i] = 0;
    }
  }
  var boom;
  function g() {
    var a = arguments;
    f(a);
    boom = a[5];
    assertEquals(undefined, boom);
  }

  f([]);
  g(1);
})();

(function strictPackedArguments() {
  "use strict";
  function f(a) {
    for (var i = 0; i < 2; i++) {
      a[i] = 0;
    }
  }
  var boom;
  function g() {
    var a = arguments;
    f(a);
    boom = a[5];
    assertEquals(undefined, boom);
  }

  f([]);
  g(1);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f739730^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=f739730)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=f739730)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=f739730)  
[test/mjsunit/regress/regress-5772.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5772.js?cl=f739730)  
  

---   

## **regress-648719.js (chromium issue)**  
   
**[Issue 648719:
 eval, const, closure causes function decl to fail](https://crbug.com/648719)**  
**[Commit: Use a different map to distinguish eval contexts](https://chromium.googlesource.com/v8/v8/+/53fdf9d)**  
  
Date(Commit): Tue Dec 20 16:23:19 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Via-Wizard", "Arch-All", "Merge-Rejected-56", "NodeJS-Backport-Review"]  
Code Review: [https://codereview.chromium.org/2435023002](https://codereview.chromium.org/2435023002)  
Regress: [mjsunit/regress/regress-648719.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-648719.js)  
```javascript
assertEquals('function', typeof eval('const xz = {};function yz(){xz}yz'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53fdf9d^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=53fdf9d)  
[src/code-factory.cc](https://cs.chromium.org/chromium/src/v8/src/code-factory.cc?cl=53fdf9d)  
[src/code-factory.h](https://cs.chromium.org/chromium/src/v8/src/code-factory.h?cl=53fdf9d)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=53fdf9d)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=53fdf9d)  
...  
  

---   

## **regress-5295-2.js (v8 issue)**  
   
**[Issue 5295:
 Possible reference bug](https://crbug.com/v8/5295)**  
**[Commit: Use a different map to distinguish eval contexts](https://chromium.googlesource.com/v8/v8/+/53fdf9d)**  
  
Date(Commit): Tue Dec 20 16:23:19 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2435023002](https://codereview.chromium.org/2435023002)  
Regress: [mjsunit/regress/regress-5295-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5295-2.js), [mjsunit/regress/regress-5295.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5295.js)  
```javascript
source = "var x;";
for (var i = 0; i < 11; i++) {
  source += "  let a_" + i + " = 0;\n";
}
source += "  (function () {\n"
for (var i = 0; i < 11; i++) {
  source += "a_" + i + "++;\n";
}
source += "})();\n"

eval(source);
assertEquals(undefined, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53fdf9d^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=53fdf9d)  
[src/code-factory.cc](https://cs.chromium.org/chromium/src/v8/src/code-factory.cc?cl=53fdf9d)  
[src/code-factory.h](https://cs.chromium.org/chromium/src/v8/src/code-factory.h?cl=53fdf9d)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=53fdf9d)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=53fdf9d)  
...  
  

---   

## **regress-674232.js (chromium issue)**  
   
**[Issue 674232:
 Unreachable code in objects-inl.h](https://crbug.com/674232)**  
**[Commit: Implement header size calculation for array iterators.](https://chromium.googlesource.com/v8/v8/+/cbd3b3d)**  
  
Date(Commit): Tue Dec 20 10:38:17 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2592633002](https://codereview.chromium.org/2592633002)  
Regress: [mjsunit/regress/regress-674232.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-674232.js)  
```javascript
function getRandomProperty(v, rand) { var properties = Object.getOwnPropertyNames(v); var proto = Object.getPrototypeOf(v); if (proto) {; } if ("constructor" && v.constructor.hasOwnProperty()) {; } if (properties.length == 0) { return "0"; } return properties[rand % properties.length]; }
function __f_11() {
  var __v_8 = new Array();
  var __v_9 = __v_8.entries();
  __v_9.__p_118574531 = __v_9[ 118574531];
  __v_9.__defineGetter__(getRandomProperty(__v_9, 1442724132), function() {; __v_0[getRandomProperty()] = __v_1[getRandomProperty()]; return __v_9.__p_118574531; });
}
function __f_10() {
  __f_11();
}
__f_10();
__f_10();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cbd3b3d^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=cbd3b3d)  
[test/mjsunit/regress/regress-674232.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-674232.js?cl=cbd3b3d)  
  

---   

## **regress-crbug-663340.js (chromium issue)**  
   
**[Issue 663340:
 Difference between fullcode and ignition_staging_turbo_opt: array shift](https://crbug.com/663340)**  
**[Commit: [crankshaft] Ensure that we use inlined Array.prototype.shift only when there's no elements in the prototype chain.](https://chromium.googlesource.com/v8/v8/+/faf80b4)**  
  
Date(Commit): Tue Dec 20 10:18:02 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2593553002](https://codereview.chromium.org/2593553002)  
Regress: [mjsunit/regress/regress-crbug-663340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663340.js)  
```javascript
var expected = undefined;

function foo() {
  var a = [0,,{}];
  a.shift();
  assertEquals(expected, a[0]);
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();

expected = 42;
Array.prototype[0] = 153;
Array.prototype[1] = expected;
foo();

function bar() {
  var a = [0,,{}];
  a.shift();
  assertEquals(expected, a[0]);
}
bar();
bar();
%OptimizeFunctionOnNextCall(bar);
bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/faf80b4^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=faf80b4)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=faf80b4)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=faf80b4)  
[test/mjsunit/regress/regress-crbug-663340.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663340.js?cl=faf80b4)  
  

---   

## **regress-crbug-665793.js (chromium issue)**  
   
**[Issue 665793:
 [crankshaft] OOB string access returns wrong value](https://crbug.com/665793)**  
**[Commit: [crankshaft] Properly handle OOB string accesses.](https://chromium.googlesource.com/v8/v8/+/576a46f)**  
  
Date(Commit): Tue Dec 20 10:01:59 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2589823003](https://codereview.chromium.org/2589823003)  
Regress: [mjsunit/regress/regress-crbug-665793.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665793.js)  
```javascript
function foo() {
  return 'x'[1];
}
assertEquals(undefined, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/576a46f^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=576a46f)  
[test/mjsunit/regress/regress-crbug-665793.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665793.js?cl=576a46f)  
  

---   

## **regress-674447.js (chromium issue)**  
   
**[Issue 674447:
 Autocomplete/datalist popup gets detached by scrolling](https://crbug.com/674447)**  
**[Commit: [serializer] do not serialize script wrappers.](https://chromium.googlesource.com/v8/v8/+/07fa0f4)**  
  
Date(Commit): Mon Dec 19 10:53:02 2016  
Components/Type: Blink>Forms>Datalist/Bug  
Labels: ["M-69"]  
Code Review: [https://codereview.chromium.org/2586943003](https://codereview.chromium.org/2586943003)  
Regress: [mjsunit/regress/wasm/regress-674447.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-674447.js)  
```javascript
(function() {
  "use asm";
  return function f() {}
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/07fa0f4^!)  
[src/snapshot/code-serializer.cc](https://cs.chromium.org/chromium/src/v8/src/snapshot/code-serializer.cc?cl=07fa0f4)  
[test/mjsunit/regress/wasm/regression-674447.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-674447.js?cl=07fa0f4)  
  

---   

## **regress-5749.js (v8 issue)**  
   
**[Issue 5749:
 Check failed: call->arguments()->length() == 1](https://crbug.com/v8/5749)**  
**[Commit: [crankshaft] Fix IsClassOfTest helper method](https://chromium.googlesource.com/v8/v8/+/d449322)**  
  
Date(Commit): Mon Dec 19 10:45:48 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2586933002](https://codereview.chromium.org/2586933002)  
Regress: [mjsunit/regress/regress-5749.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5749.js)  
```javascript
function f(x) {
    (x ** 1) === '';
}
f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();

function g(x) {
    '' === (x ** 1);
}
g();
g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d449322^!)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=d449322)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=d449322)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=d449322)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=d449322)  
[test/mjsunit/regress/regress-5749.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5749.js?cl=d449322)  
  

---   

## **regress-673242.js (chromium issue)**  
   
**[Issue 673242:
 shared->is_compiled() in compiler.cc](https://crbug.com/673242)**  
**[Commit: [Complier] Only optimize a function marked for tier-up if it is compiled.](https://chromium.googlesource.com/v8/v8/+/cb9d0fe)**  
  
Date(Commit): Fri Dec 16 10:44:50 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2575333003](https://codereview.chromium.org/2575333003)  
Regress: [mjsunit/regress/regress-673242.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-673242.js)  
```javascript
function foo() {
  function bar() {
  }
  return bar;
}

var bar = foo();
%OptimizeFunctionOnNextCall(bar);

%OptimizeFunctionOnNextCall(foo);

bar = null;
for (var i = 0; i < 6; i++) {
  gc();
}

foo()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb9d0fe^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=cb9d0fe)  
[test/mjsunit/regress/regress-673242.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-673242.js?cl=cb9d0fe)  
  

---   

## **regress-5736.js (v8 issue)**  
   
**[Issue 5736:
 eval is broken in several ways](https://crbug.com/v8/5736)**  
**[Commit: Disable lazy parsing inside eval (see bug).](https://chromium.googlesource.com/v8/v8/+/ed080e6)**  
  
Date(Commit): Thu Dec 15 14:26:58 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2435023002/](https://codereview.chromium.org/2435023002/)  
Regress: [mjsunit/regress/regress-5736.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5736.js)  
```javascript
var my_global = 0;

eval(`let foo = 1;
      let maybe_lazy = function() { foo = 2; }
      maybe_lazy();
      my_global = foo;`);
assertEquals(2, my_global);

(function TestVarInStrictEval() {
  "use strict";
  eval(`var foo = 3;
        let maybe_lazy = function() { foo = 4; }
        maybe_lazy();
        my_global = foo;`);
  assertEquals(4, my_global);
})();

eval("let foo = 1; function lazy() { foo = 2; } lazy(); my_global = foo;");
assertEquals(my_global, 2);

eval(`{ let foo = 5;
        function not_lazy() { foo = 6; }
        not_lazy();
        my_global = foo;
      }`);
assertEquals(my_global, 6);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ed080e6^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=ed080e6)  
[test/mjsunit/bugs/bug-2728.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-2728.js?cl=ed080e6)  
[test/mjsunit/regress/regress-5736.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5736.js?cl=ed080e6)  
  

---   

## **regress-674469.js (chromium issue)**  
   
**[Issue 674469:
 CanBeTaggedPointer(type.representation()) in code-generator.cc](https://crbug.com/674469)**  
**[Commit: [turbofan] Handle the impossible value representation mismatch in instruction selector.](https://chromium.googlesource.com/v8/v8/+/01de216)**  
  
Date(Commit): Thu Dec 15 12:13:06 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2579743002](https://codereview.chromium.org/2579743002)  
Regress: [mjsunit/compiler/regress-674469.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-674469.js)  
```javascript
global = -1073741824;
global = 2;
function foo() {
  global = "a";
  global = global;
  var o = global;
  while (o < 2) {
  }
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/01de216^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=01de216)  
[test/mjsunit/compiler/regress-674469.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-674469.js?cl=01de216)  
  

---   

## **regress-672027.js (chromium issue)**  
   
**[Issue 672027:
 Ignition memory leak at http://moxielogic.org](https://crbug.com/672027)**  
**[Commit: [Interpreter] Allocate registers used as call arguments on-demand.](https://chromium.googlesource.com/v8/v8/+/ae741d0)**  
  
Date(Commit): Thu Dec 15 10:59:57 2016  
Components/Type: None/Bug  
Labels: ["M-56", "Proj-Ignition", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2557173004](https://codereview.chromium.org/2557173004)  
Regress: [mjsunit/ignition/regress-672027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-672027.js)  
```javascript
(function() {
  var source = "[]"
  for (var i = 0; i < 300; i++) {
    source += ".concat([";
    for (var j = 0; j < 1000; j++) {
      source += "0,";
    }
    source += "0])"
  }
  eval(source);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae741d0^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=ae741d0)  
[src/interpreter/bytecode-generator.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.h?cl=ae741d0)  
[src/interpreter/bytecode-register-allocator.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register-allocator.h?cl=ae741d0)  
[src/interpreter/bytecode-register.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register.h?cl=ae741d0)  
[test/cctest/interpreter/bytecode_expectations/ForOf.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForOf.golden?cl=ae741d0)  
...  
  

---   

## **regress-crbug-672792.js (chromium issue)**  
   
**[Issue 672792:
 size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/672792)**  
**[Commit: Fix usage of literal cloning for large double arrays.](https://chromium.googlesource.com/v8/v8/+/6c620e5)**  
  
Date(Commit): Thu Dec 15 10:29:47 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2570843002](https://codereview.chromium.org/2570843002)  
Regress: [mjsunit/regress/regress-crbug-672792.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-672792.js)  
```javascript
eval("function f() { return [" + String("0.1,").repeat(65535) + "] }");

assertEquals(65535, f().length);

assertEquals(65535, f().length);

%OptimizeFunctionOnNextCall(f);
assertEquals(65535, f().length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c620e5^!)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=6c620e5)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=6c620e5)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=6c620e5)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=6c620e5)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=6c620e5)  
...  
  

---   

## **regress-643595.js (chromium issue)**  
   
**[Issue 643595:
 !obj->IsMap() in code-serializer.cc](https://crbug.com/643595)**  
**[Commit: [wasm] disable serialization for asm-wasm](https://chromium.googlesource.com/v8/v8/+/77b50a8)**  
  
Date(Commit): Thu Dec 15 05:06:54 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2573193002](https://codereview.chromium.org/2573193002)  
Regress: [mjsunit/regress/wasm/regress-643595.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-643595.js)  
```javascript
(function f() {
  "use asm";
  function g() { }
  return { g: g };
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77b50a8^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=77b50a8)  
[test/mjsunit/regress/wasm/regression-643595.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-643595.js?cl=77b50a8)  
  

---   

## **regress-crbug-669540.js (chromium issue)**  
   
**[Issue 669540:
 Missing hole check in computed property names](https://crbug.com/669540)**  
**[Commit: [ignition] Fix hole check for dynamic local variables](https://chromium.googlesource.com/v8/v8/+/f6ee3b5)**  
  
Date(Commit): Tue Dec 13 14:29:07 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["reward-topanel", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://codereview.chromium.org/2551023004](https://codereview.chromium.org/2551023004)  
Regress: [mjsunit/regress/regress-crbug-669540.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669540.js)  
```javascript
function f({
    [
        (function g() {
            assertThrows(function(){
                print(eval("p"));
            }, ReferenceError);
        })()
    ]: p
}) {};

f({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6ee3b5^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=f6ee3b5)  
[test/mjsunit/regress/regress-crbug-669540.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669540.js?cl=f6ee3b5)  
  

---   

## **regress-crbug-673008.js (chromium issue)**  
   
**[Issue 673008:
 hasOwnProperty returning inconsistent results](https://crbug.com/673008)**  
**[Commit: [stubs] Fix negative index lookup in hasOwnProperty](https://chromium.googlesource.com/v8/v8/+/bb753b6)**  
  
Date(Commit): Mon Dec 12 20:13:07 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-55", "Needs-Feedback", "Arch-x86_64", "ReleaseBlock-Stable", "NodeJS-Backport-Rejected", "merge-merged-5.5", "Via-Wizard-Javascript", "merge-merged-5.6", "TE-Verified-M57", "prestable-55.0.2883.87", "TE-Verified-57.0.2951.0"]  
Code Review: [https://codereview.chromium.org/2568943002](https://codereview.chromium.org/2568943002)  
Regress: [mjsunit/regress/regress-crbug-673008.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-673008.js)  
```javascript
var a = {
  "33": true,
  "-1": true
};

var strkeys = Object.keys(a).map(function(k) { return "" + k });
var numkeys = Object.keys(a).map(function(k) { return +k });
var keys = strkeys.concat(numkeys);

keys.forEach(function(k) {
  assertTrue(a.hasOwnProperty(k),
             "property not found: " + k + "(" + (typeof k) + ")");
});

var b = {};
b.__proto__ = a;
keys.forEach(function(k) {
  assertTrue(k in b, "property not found: " + k + "(" + (typeof k) + ")");
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb753b6^!)  
[src/builtins/builtins-object.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object.cc?cl=bb753b6)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=bb753b6)  
[test/mjsunit/regress/regress-crbug-673008.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-673008.js?cl=bb753b6)  
  

---   

## **regress-672041.js (chromium issue)**  
   
**[Issue 672041:
 chunk->Contains(slot_addr) in remembered-set.h](https://crbug.com/672041)**  
**[Commit: [heap] Initialize the owner on each page after lospace allocation](https://chromium.googlesource.com/v8/v8/+/9b6808b)**  
  
Date(Commit): Mon Dec 12 13:19:07 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2565713002](https://codereview.chromium.org/2565713002)  
Regress: [mjsunit/regress/regress-672041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-672041.js)  
```javascript
const min_ptr_size = 4;
const max_regular_heap_object_size = 507136;
const num_iterations = max_regular_heap_object_size / min_ptr_size;

const RegExpPrototypeExec = RegExp.prototype.exec;

let i = 0;

RegExp.prototype.__defineGetter__("global", () => true);
RegExp.prototype.exec = function(str) {
  return (i++ < num_iterations) ? RegExpPrototypeExec.call(this, str) : null;
};

"a".match();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9b6808b^!)  
[src/heap/spaces-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap/spaces-inl.h?cl=9b6808b)  
[src/heap/spaces.h](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.h?cl=9b6808b)  
[test/mjsunit/regress/regress-672041.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-672041.js?cl=9b6808b)  
  

---   

## **regress-673241.js (chromium issue)**  
   
**[Issue 673241:
 Crash in callee_pc](https://crbug.com/673241)**  
**[Commit: [wasm] Handle potentially null callee-pc](https://chromium.googlesource.com/v8/v8/+/c69b48a)**  
  
Date(Commit): Mon Dec 12 12:30:39 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2562333002](https://codereview.chromium.org/2562333002)  
Regress: [mjsunit/regress/regress-673241.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-673241.js)  
```javascript
function generateAsmJs() {
  'use asm';
  function fun() { fun(); }
  return fun;
}

assertThrows(generateAsmJs());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c69b48a^!)  
[src/frames.h](https://cs.chromium.org/chromium/src/v8/src/frames.h?cl=c69b48a)  
[test/mjsunit/regress/regress-673241.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-673241.js?cl=c69b48a)  
  

---   

## **regress-673244.js (chromium issue)**  
   
**[Issue 673244:
 Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/673244)**  
**[Commit: [turbofan] Fix representation change from bit to tagged pointer.](https://chromium.googlesource.com/v8/v8/+/d024df4)**  
  
Date(Commit): Mon Dec 12 09:36:47 2016  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-56", "External-Fuzzer-Contribution", "reward-3000", "Security_Severity-Medium", "Hotlist-Merge-Approved", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "reward-inprocess", "M-57", "ClusterFuzz-Verified", "reward_to-decoder.oh_at_googlemail.com", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2568053002](https://codereview.chromium.org/2568053002)  
Regress: [mjsunit/compiler/regress-673244.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-673244.js)  
```javascript
function f() {
  var accumulator = false;
  for (var i = 0; i < 4; i++) {
    accumulator = accumulator.hasOwnProperty(3);
    if (i === 1) %OptimizeOsr();
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d024df4^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=d024df4)  
[test/mjsunit/compiler/regress-673244.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-673244.js?cl=d024df4)  
  

---   

## **regress-672045.js (chromium issue)**  
   
**[Issue 672045:
 VariableLocation::PARAMETER == expr->obj()->AsVariableProxy()->var()->location()](https://crbug.com/672045)**  
**[Commit: [wasm][asm.js] Fail sooner if eval is present.](https://chromium.googlesource.com/v8/v8/+/6deb99c)**  
  
Date(Commit): Thu Dec 08 14:44:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2558813004](https://codereview.chromium.org/2558813004)  
Regress: [mjsunit/asm/regress-672045.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-672045.js)  
```javascript
function Module(stdlib, env) {
  "use asm";
  var x = env.bar|0;
  return { foo: function(y) { return eval(1); } };
}
Module(this, {bar:0});
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6deb99c^!)  
[src/asmjs/asm-typer.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.cc?cl=6deb99c)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=6deb99c)  
[test/mjsunit/asm/regress-672045.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-672045.js?cl=6deb99c)  
  

---   

## **regress-671574.js (chromium issue)**  
   
**[Issue 671574:
 false in code-generator.cc](https://crbug.com/671574)**  
**[Commit: [turbofan] Fix skipping of translations for lazy deopt return value stores.](https://chromium.googlesource.com/v8/v8/+/da2529a)**  
  
Date(Commit): Wed Dec 07 08:31:40 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2560743002](https://codereview.chromium.org/2560743002)  
Regress: [mjsunit/compiler/regress-671574.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-671574.js)  
```javascript
"use strict";
function f() {
  try {
    try {
       eval('--');
    } catch (e) { }
  } catch(e) { }
  try {
    function get() { }
    dummy = {};
  } catch(e) {"Caught: " + e; }
}

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da2529a^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=da2529a)  
[test/mjsunit/compiler/regress-671574.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-671574.js?cl=da2529a)  
  

---   

## **regress-670671.js (chromium issue)**  
   
**[Issue 670671:
 size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/670671)**  
**[Commit: [stubs] Add option to allow LO space allocation](https://chromium.googlesource.com/v8/v8/+/9c9c8d7)**  
  
Date(Commit): Tue Dec 06 14:08:57 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "Reproducible", "ReleaseBlock-Dev", "Fracas", "Clusterfuzz", "M-57", "FoundIn-M-55", "FoundIn-M-57", "TE-Verified-57.0.2944.0", "TE-Verified-57"]  
Code Review: [https://codereview.chromium.org/2555703003](https://codereview.chromium.org/2555703003)  
Regress: [mjsunit/regress/regress-670671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-670671.js)  
```javascript
const min_ptr_size = 4;
const max_regular_heap_object_size = 507136;
const num_iterations = max_regular_heap_object_size / min_ptr_size;

let i = 0;

const re = /foo.bar/;
const RegExpPrototypeExec = RegExp.prototype.exec;
re.exec = (str) => {
  return (i++ < num_iterations) ? RegExpPrototypeExec.call(re, str) : null;
};
re.__defineGetter__("global", () => true);  // Triggers infinite loop.

"foo*bar".match(re);  // Should not crash.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c9c8d7^!)  
[src/builtins/builtins-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp.cc?cl=9c9c8d7)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=9c9c8d7)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=9c9c8d7)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=9c9c8d7)  
[test/mjsunit/regress/regress-670671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-670671.js?cl=9c9c8d7)  
  

---   

## **regress-crbug-671576.js (chromium issue)**  
   
**[Issue 671576:
 map_index >= Context::TYPED_ARRAY_KEY_ITERATOR_MAP_INDEX in js-builtin-reducer.c](https://crbug.com/671576)**  
**[Commit: Fix assertion failure in JSBuiltinReducer::ReduceArrayIterator.](https://chromium.googlesource.com/v8/v8/+/a610155)**  
  
Date(Commit): Tue Dec 06 13:10:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2550143004](https://codereview.chromium.org/2550143004)  
Regress: [mjsunit/regress/regress-crbug-671576.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-671576.js)  
```javascript
function f() {
  for (var i of [NaN].keys());
}

f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a610155^!)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=a610155)  
[test/mjsunit/regress/regress-crbug-671576.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-671576.js?cl=a610155)  
  

---   

## **regress-670808.js (chromium issue)**  
   
**[Issue 670808:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSFunction()) in objects-i](https://crbug.com/670808)**  
**[Commit: [wasm] Implement location from stack trace for asm.js frames](https://chromium.googlesource.com/v8/v8/+/6a8dccb)**  
  
Date(Commit): Mon Dec 05 19:30:16 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2548323002](https://codereview.chromium.org/2548323002)  
Regress: [mjsunit/regress/regress-670808.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-670808.js)  
```javascript
var sym = Symbol();
function asm(stdlib, ffi) {
  "use asm";
  var get_sym = ffi.get_sym;
  function crash() {
    get_sym()|0;
  }
  return {crash: crash};
}
function get_sym() {
  return sym;
}
try {
  asm(null, {get_sym: get_sym}).crash();
} catch (e) {
  if (!(e instanceof TypeError))
    throw e;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6a8dccb^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=6a8dccb)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=6a8dccb)  
[test/mjsunit/regress/regress-670808.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-670808.js?cl=6a8dccb)  
  

---   

## **regress-v8-5697.js (v8 issue)**  
   
**[Issue 5697:
 Deopt loop when LoadIC on null/undefined](https://crbug.com/v8/5697)**  
**[Commit: [ic] Ensure state of load/store ICs always progresses.](https://chromium.googlesource.com/v8/v8/+/e7a51ff)**  
  
Date(Commit): Fri Dec 02 15:07:31 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2548753003](https://codereview.chromium.org/2548753003)  
Regress: [mjsunit/regress/regress-v8-5697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5697.js)  
```javascript
function load(o) { return o.x; }

for (var x = 0; x < 1000; ++x) {
  load({x});
  load({x});
  %OptimizeFunctionOnNextCall(load);
  try { load(); } catch (e) { }
}

assertOptimized(load);


function store(o) { o.x = -1; }

for (var x = 0; x < 1000; ++x) {
  store({x});
  store({x});
  %OptimizeFunctionOnNextCall(store);
  try { store(); } catch (e) { }
}

assertOptimized(store);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e7a51ff^!)  
[src/counters.h](https://cs.chromium.org/chromium/src/v8/src/counters.h?cl=e7a51ff)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=e7a51ff)  
[test/mjsunit/regress/regress-v8-5697.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5697.js?cl=e7a51ff)  
  

---   

## **regress-crbug-669411.js (chromium issue)**  
   
**[Issue 669411:
 Crash in v8::internal::Factory::NewTuple2](https://crbug.com/669411)**  
**[Commit: [ic] Use validity cells to protect keyed element stores against object's prototype chain modifications.](https://chromium.googlesource.com/v8/v8/+/39e6f2ca)**  
  
Date(Commit): Fri Dec 02 10:03:33 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://crrev.com/a39522f44f7e0be4686831688917e9675255dcaf](https://crrev.com/a39522f44f7e0be4686831688917e9675255dcaf)  
Regress: [mjsunit/regress/regress-crbug-669411.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669411.js)  
```javascript
function f(o) {
  o[5000000] = 0;
}
var o = Object.create(null);
f(o);
f(o);
f(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39e6f2ca^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=39e6f2ca)  
[src/ast/ast-types.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-types.cc?cl=39e6f2ca)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=39e6f2ca)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=39e6f2ca)  
[src/compiler/types.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/types.cc?cl=39e6f2ca)  
...  
  

---   

## **regress-crbug-669850.js (chromium issue)**  
   
**[Issue 669850:
 size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/669850)**  
**[Commit: [turbofan] Workaround for unknown array literal length.](https://chromium.googlesource.com/v8/v8/+/f8fec66)**  
  
Date(Commit): Thu Dec 01 12:01:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2542633002](https://codereview.chromium.org/2542633002)  
Regress: [mjsunit/regress/regress-crbug-669850.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669850.js)  
```javascript
eval('function f(a) { return [' + new Array(1 << 17) + ',a] }');
assertEquals(23, f(23)[1 << 17]);
assertEquals(42, f(42)[1 << 17]);
%OptimizeFunctionOnNextCall(f);
assertEquals(65, f(65)[1 << 17]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f8fec66^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=f8fec66)  
[test/mjsunit/regress/regress-crbug-669850.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669850.js?cl=f8fec66)  
  

---   

## **regress-669899.js (chromium issue)**  
   
**[Issue 669899:
 (indices) != nullptr in asm-wasm-builder.cc](https://crbug.com/669899)**  
**[Commit: [wasm] [asm.js] Ignore unused function tables in AsmWasmBuilder.](https://chromium.googlesource.com/v8/v8/+/00ec483)**  
  
Date(Commit): Thu Dec 01 02:27:30 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2546553002](https://codereview.chromium.org/2546553002)  
Regress: [mjsunit/asm/regress-669899.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-669899.js)  
```javascript
try {
(function () {
})();
} catch(e) {; }
  function __f_113() {
  }
(function () {
function __f_89() {
  "use asm";
  function __f_63(__v_26, __v_28) {
    __v_26 = __v_26|0;
    __v_28 = __v_28|0;
  }
  function __f_21(table_id, fun_id, arg1, arg2) {
    table_id = table_id|0;
    fun_id = fun_id|0;
    arg1 = arg1|0;
    arg2 = arg2|0;
  }
  var __v_17 = [];
}
var module = __f_89();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00ec483^!)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=00ec483)  
[test/mjsunit/asm/regress-669899.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-669899.js?cl=00ec483)  
  

---   

## **regress-669024.js (chromium issue)**  
   
**[Issue 669024:
 Difference between x64 and ia32: arguments and undefined](https://crbug.com/669024)**  
**[Commit: [crankshaft] Disable escape analysis of nested objects.](https://chromium.googlesource.com/v8/v8/+/e19f43d)**  
  
Date(Commit): Wed Nov 30 15:07:16 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2531163006](https://codereview.chromium.org/2531163006)  
Regress: [mjsunit/regress/regress-669024.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-669024.js)  
```javascript
function h(y) { return y.u; }

function g() { return h.apply(0, arguments); }

function f(x) {
  var o = { u : x };
  return g(o);
}

f(42);
f(0.1);

%OptimizeFunctionOnNextCall(f);

assertEquals(undefined, f(undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e19f43d^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=e19f43d)  
[test/mjsunit/regress/regress-669024.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-669024.js?cl=e19f43d)  
  

---   

## **regress-669517.js (chromium issue)**  
   
**[Issue 669517:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/669517)**  
**[Commit: [Turbofan] Disable JSFrameSpecialization for interpreted frames.](https://chromium.googlesource.com/v8/v8/+/6d90507)**  
  
Date(Commit): Wed Nov 30 14:03:51 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2538823002](https://codereview.chromium.org/2538823002)  
Regress: [mjsunit/compiler/regress-669517.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-669517.js)  
```javascript
(function() {
  "use asm";
  return function() {
    for (var i = 0; i < 10; i++) {
      if (i == 5) {
        %OptimizeOsr();
      }
    }
    with (Object());
  }
})()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d90507^!)  
[src/compiler/js-frame-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-frame-specialization.cc?cl=6d90507)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=6d90507)  
[test/mjsunit/compiler/regress-669517.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-669517.js?cl=6d90507)  
  

---   

## **regress-crbug-669451.js (chromium issue)**  
   
**[Issue 669451:
 Fatal error in ../../v8/src/compiler/escape-analysis.cc, line 824](https://crbug.com/669451)**  
**[Commit: [turbofan] Teach escape analysis about ConvertTaggedHoleToUndefined.](https://chromium.googlesource.com/v8/v8/+/d6752d9)**  
  
Date(Commit): Tue Nov 29 13:13:55 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2533263002](https://codereview.chromium.org/2533263002)  
Regress: [mjsunit/regress/regress-crbug-669451.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669451.js)  
```javascript
function foo() {
  var a = [,];
  a[0] = {}
  a[0].toString = FAIL;
}
try { foo(); } catch (e) {}
try { foo(); } catch (e) {}
%OptimizeFunctionOnNextCall(foo);
try { foo(); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6752d9^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=d6752d9)  
[test/mjsunit/regress/regress-crbug-669451.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669451.js?cl=d6752d9)  
  

---   

## **regress-5664.js (v8 issue)**  
   
**[Issue 5664:
 mjsunit/es6/destructuring fails with --nolazy](https://crbug.com/v8/5664)**  
**[Commit: [scopes] Propagate inner-scope-calls-eval to make sure we context allocate in inserted scopes](https://chromium.googlesource.com/v8/v8/+/73a2d63)**  
  
Date(Commit): Tue Nov 29 12:01:34 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2536153002](https://codereview.chromium.org/2536153002)  
Regress: [mjsunit/regress/regress-5664.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5664.js)  
```javascript
var f = (x, y=()=>eval("x")) => y();
assertEquals(100, f(100));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73a2d63^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=73a2d63)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=73a2d63)  
[test/mjsunit/regress/regress-5664.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5664.js?cl=73a2d63)  
  

---   

## **regress-crbug-668795.js (chromium issue)**  
   
**[Issue 668795:
 length == previously_materialized_objects->length() in deoptimizer.cc](https://crbug.com/668795)**  
**[Commit: [deoptimizer] Fix deoptimization in {TranslatedState}.](https://chromium.googlesource.com/v8/v8/+/204babf)**  
  
Date(Commit): Tue Nov 29 11:34:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2534143002](https://codereview.chromium.org/2534143002)  
Regress: [mjsunit/regress/regress-crbug-668795.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668795.js)  
```javascript
function g() {
  return g.arguments;
}

function f() {
  var result = "R:";
  for (var i = 0; i < 3; ++i) {
    if (i == 1) %OptimizeOsr();
    result += g([1])[0];
    result += g([2])[0];
  }
  return result;
}

assertEquals("R:121212", f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/204babf^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=204babf)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=204babf)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=204babf)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=204babf)  
[test/mjsunit/regress/regress-crbug-668795.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668795.js?cl=204babf)  
  

---   

## **regress-crbug-662907.js (chromium issue)**  
   
**[Issue 662907:
 Difference between fullcode and crankshaft_opt: array prototype setter](https://crbug.com/662907)**  
**[Commit: [ic] Use validity cells to protect keyed element stores against object's prototype chain modifications.](https://chromium.googlesource.com/v8/v8/+/a39522f)**  
  
Date(Commit): Mon Nov 28 22:56:52 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2534613002](https://codereview.chromium.org/2534613002)  
Regress: [mjsunit/regress/regress-crbug-662907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662907.js)  
```javascript
(function() {
  function foo() {
    var a = new Array();
    a[0] = 10;
    return a;
  }

  assertEquals(1, foo().length);

  gc();
  gc();
  gc();
  gc();

  // Change prototype elements from fast smi to slow elements dictionary.
  // The validity cell is invalidated by the change of Array.prototype's
  // map.
  Array.prototype.__defineSetter__("0", function() {});

  assertEquals(0, foo().length);
})();


(function() {
  function foo() {
    var a = new Array();
    a[0] = 10;
    return a;
  }

  // Change prototype elements from fast smi to dictionary.
  Array.prototype[123456789] = 42;
  Array.prototype.length = 0;

  assertEquals(1, foo().length);

  gc();
  gc();
  gc();
  gc();

  // Change prototype elements from dictionary to slow elements dictionary.
  // The validity cell is invalidated by making the elements dictionary slow.
  Array.prototype.__defineSetter__("0", function() {});

  assertEquals(0, foo().length);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a39522f^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=a39522f)  
[src/ast/ast-types.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-types.cc?cl=a39522f)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=a39522f)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=a39522f)  
[src/compiler/types.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/types.cc?cl=a39522f)  
...  
  

---   

## **regress-666046.js (chromium issue)**  
   
**[Issue 666046:
 Tab crashes every time for unknown reason (recent versions of Chrome and Chrome only)](https://crbug.com/666046)**  
**[Commit: [heap] Clear recorded slots for inobject properties when migrating fast object to slow mode.](https://chromium.googlesource.com/v8/v8/+/a814b8a)**  
  
Date(Commit): Mon Nov 28 20:11:30 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "NodeJS-Backport-Done", "merge-merged-5.5", "Via-Wizard-Crashes", "merge-merged-5.6", "Merge-rejected-5.4"]  
Code Review: [https://codereview.chromium.org/2539493002](https://codereview.chromium.org/2539493002)  
Regress: [mjsunit/regress/regress-666046.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-666046.js)  
```javascript
function P() {
  this.a0 = {};
  this.a1 = {};
  this.a2 = {};
  this.a3 = {};
  this.a4 = {};
}

function A() {
}

var proto = new P();
A.prototype = proto;

function foo(o) {
  return o.a0;
}

gc();
gc();
gc();

var o = new A();
foo(o);
foo(o);
foo(o);
assertTrue(%HasFastProperties(proto));

var buffer = new ArrayBuffer(8);
var int32view = new Int32Array(buffer);
var float64view = new Float64Array(buffer);
int32view[0] = int32view[1] = 0x40000001;
var boom = float64view[0];


proto.a4 = {a: 0};
delete proto.a4;

assertTrue(%HasFastProperties(proto));

proto.boom = boom;

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a814b8a^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a814b8a)  
[test/mjsunit/regress/regress-666046.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-666046.js?cl=a814b8a)  
  

---   

## **regress-crbug-663410.js (chromium issue)**  
   
**[Issue 663410:
 Function arugment evasion](https://crbug.com/663410)**  
**[Commit: Fix 'combo breaker' in CreateDynamicFunction to handle template literals.](https://chromium.googlesource.com/v8/v8/+/e0d608a)**  
  
Date(Commit): Mon Nov 28 14:44:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Needs-Feedback", "Via-Wizard-Javascript"]  
Code Review: [https://codereview.chromium.org/2533463002](https://codereview.chromium.org/2533463002)  
Regress: [mjsunit/regress/regress-crbug-663410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663410.js)  
```javascript
function alert(x) {};
assertThrows(
  'Function("a=`","`,xss=1){alert(xss)")'
);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0d608a^!)  
[src/builtins/builtins-function.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function.cc?cl=e0d608a)  
[test/mjsunit/regress-crbug-663410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-663410.js?cl=e0d608a)  
  

---   

## **regress-668760.js (chromium issue)**  
   
**[Issue 668760:
 catch_handler_frame_index < count in deoptimizer.cc](https://crbug.com/668760)**  
**[Commit: [deoptimizer] Use the correct function for handler lookup for bytecode.](https://chromium.googlesource.com/v8/v8/+/72b5a0d)**  
  
Date(Commit): Mon Nov 28 12:45:29 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2530403002](https://codereview.chromium.org/2530403002)  
Regress: [mjsunit/compiler/regress-668760.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-668760.js)  
```javascript
function f() {
  try {
    o;
  } catch (e) {
    return e;
  }
  return 0;
}

function deopt() {
  %DeoptimizeFunction(f);
  throw 42;
}

%NeverOptimizeFunction(deopt);

this.__defineGetter__("o", deopt );

%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/72b5a0d^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=72b5a0d)  
[test/mjsunit/compiler/regress-668760.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-668760.js?cl=72b5a0d)  
  

---   

## **regress-crbug-668414.js (chromium issue)**  
   
**[Issue 668414:
 Difference between x64 and ia32: array concat](https://crbug.com/668414)**  
**[Commit: [runtime] Add missing @@IsConcatSpreadable check for FAST_DOUBLE_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/a09e5ed)**  
  
Date(Commit): Mon Nov 28 10:06:17 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2527173002](https://codereview.chromium.org/2527173002)  
Regress: [mjsunit/regress/regress-crbug-668414.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668414.js)  
```javascript
(function testSmiArrayConcat() {
  var result = [].concat([-12]);

  assertEquals(1, result.length);
  assertEquals([-12], result);
})();

(function testDoubleArrayConcat() {
  var result = [].concat([-1073741825]);

  assertEquals(1, result.length);
  assertEquals([-1073741825], result);
})();

(function testSmiArrayNonConcatSpreadable() {
  var array = [-10];
  array[Symbol.isConcatSpreadable] = false;
  var result = [].concat(array);

  assertEquals(1, result.length);
  assertEquals(1, result[0].length);
  assertEquals([-10], result[0]);
})();

(function testDoubleArrayNonConcatSpreadable() {
  var array = [-1073741825];
  array[Symbol.isConcatSpreadable] = false;
  var result = [].concat(array);

  assertEquals(1, result.length);
  assertEquals(1, result[0].length);
  assertEquals([-1073741825], result[0]);
})();

Array.prototype[Symbol.isConcatSpreadable] = false;


(function testSmiArray() {
  var result = [].concat([-12]);

  assertEquals(2, result.length);
  assertEquals(0, result[0].length);
  assertEquals(1, result[1].length);
  assertEquals([-12], result[1]);
})();

(function testDoubleArray() {
  var result = [].concat([-1073741825]);

  assertEquals(2, result.length);
  assertEquals(0, result[0].length);
  assertEquals(1, result[1].length);
  assertEquals([-1073741825], result[1]);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a09e5ed^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=a09e5ed)  
[test/mjsunit/regress/regress-crbug-668414.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668414.js?cl=a09e5ed)  
  

---   

## **regress-crbug-668101.js (chromium issue)**  
   
**[Issue 668101:
 pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/668101)**  
**[Commit: [stubs] Fix AccessorInfo mixup in KeyedStoreGeneric](https://chromium.googlesource.com/v8/v8/+/2661b3e)**  
  
Date(Commit): Wed Nov 23 13:27:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2525913002](https://codereview.chromium.org/2525913002)  
Regress: [mjsunit/regress/regress-crbug-668101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668101.js)  
```javascript
function f(a, i, v) {
  a[i] = v;
}

f("make it generic", 0, 0);

var a = new Array(3);
f(a, "length", 2);
assertEquals(2, a.length);

%OptimizeObjectForAddingMultipleProperties(a, 1);
f(a, "length", 1);
assertEquals(1, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2661b3e^!)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=2661b3e)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=2661b3e)  
[test/mjsunit/regress/regress-crbug-668101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668101.js?cl=2661b3e)  
  

---   

## **regress-5648.js (v8 issue)**  
   
**[Issue 5648:
 Segmentation fault in for..of/generators](https://crbug.com/v8/5648)**  
**[Commit: [parser] Fix scopes in rewriting of for-of and destructuring assignments.](https://chromium.googlesource.com/v8/v8/+/f385268)**  
  
Date(Commit): Wed Nov 23 13:25:35 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2520883002](https://codereview.chromium.org/2520883002)  
Regress: [mjsunit/regress/regress-5648.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5648.js)  
```javascript
var iter = {}
iter[Symbol.iterator] = () => ({
  next: () => ({}),
  return: () => {throw 666}
});


function* foo() {
  for (let x of iter) {throw 42}
}
assertThrowsEquals(() => foo().next(), 42);


function* bar() {
  let x;
  { let gaga = () => {x};
    [[x]] = iter;
  }
}
assertThrows(() => bar().next(), TypeError);


function baz() {
  let x;
  { let gaga = () => {x};
    let gugu = () => {gaga};
    [[x]] = iter;
  }
}
assertThrows(baz, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f385268^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=f385268)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=f385268)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=f385268)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=f385268)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=f385268)  
...  
  

---   

## **regress-667745.js (chromium issue)**  
   
**[Issue 667745:
 Crash in v8::internal::compiler::RegisterAllocatorVerifier::ValidatePendingAssessment](https://crbug.com/667745)**  
**[Commit: [turbofan] Regalloc validator: support same block pending assessment](https://chromium.googlesource.com/v8/v8/+/7a1ad0c)**  
  
Date(Commit): Tue Nov 22 17:31:06 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "Test-Predator-Correct-CLs"]  
Code Review: [https://codereview.chromium.org/2519973004](https://codereview.chromium.org/2519973004)  
Regress: [mjsunit/regress/wasm/regress-667745.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-667745.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(0, 0, false);
  builder.addFunction("test", kSig_i_iii)
    .addBody([
kExprI32Const, 0x0b,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x67,
kExprI32Const, 0x07,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Eq,
kExprI32RemU,
kExprI32Clz,
kExprI32Const, 0x25,
kExprI32Const, 0x82, 0x6c,
kExprI32Add,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Const, 0x70,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x70,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x67,
kExprI32Clz,
kExprI32Clz,
kExprI32GeS,
kExprI32Const, 0x67,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprDrop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Const, 0x01,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprSelect,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x0e,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Const, 0x01,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprSelect,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x0e,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Const, 0x01,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprSelect,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x0e,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Const, 0x01,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprSelect,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x41,
kExprI32Const, 0x0e,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x4a,
kExprI32Const, 0x41,
kExprI32LtU,
kExprI32Const, 0x67,
kExprI32Clz,
kExprI32GtS,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Ne,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x1a,
kExprI32Const, 0x71,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32ShrS,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprI32Clz,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Const, 0x4a,
kExprI32Const, 0x41,
kExprI32LtU,
kExprI32Const, 0x67,
kExprI32Clz,
kExprI32GtS,
kExprI32Const, 0x41,
kExprI32Const, 0x41,
kExprI32Ne,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Const, 0x41,
kExprI32Const, 0x1a,
kExprI32Const, 0x71,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32And,
kExprI32ShrS,
kExprI32Clz,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprI32Clz,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprI32Clz,
kExprUnreachable,
kExprCallFunction, 0x00, // function #0
kExprCallFunction, 0x00, // function #0
kExprNop,
kExprNop,
kExprNop,
kExprNop,
kExprReturn
            ])
            .exportFunc();
  var module = builder.instantiate();
  assertTrue(module != undefined);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a1ad0c^!)  
[src/compiler/register-allocator-verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/register-allocator-verifier.cc?cl=7a1ad0c)  
[test/mjsunit/regress/wasm/regression-667745.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-667745.js?cl=7a1ad0c)  
  

---   

## **regress-667603.js (chromium issue)**  
   
**[Issue 667603:
 Crash in v8::ShellArrayBufferAllocator::Allocate](https://crbug.com/667603)**  
**[Commit: [d8] Do not try to verify zero-ness of failed virtual memory allocation.](https://chromium.googlesource.com/v8/v8/+/5a1fbe2)**  
  
Date(Commit): Tue Nov 22 12:36:37 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Test-Predator-Wrong-CLs"]  
Code Review: [https://codereview.chromium.org/2519363002](https://codereview.chromium.org/2519363002)  
Regress: [mjsunit/regress/regress-667603.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-667603.js)  
```javascript
try {
  var v65 = new ArrayBuffer(2147483647);
} catch(e) {
  assertTrue(e instanceof RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5a1fbe2^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=5a1fbe2)  
[test/mjsunit/regress/regress-667603.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-667603.js?cl=5a1fbe2)  
  

---   

## **regress-664117.js (chromium issue)**  
   
**[Issue 664117:
 Difference between fullcode and ignition_turbo: array length overflow](https://crbug.com/664117)**  
**[Commit: [turbofan] Fix representation changes for unsigned values used as checked-signed values.](https://chromium.googlesource.com/v8/v8/+/d7aae40)**  
  
Date(Commit): Tue Nov 22 12:07:45 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2522883002](https://codereview.chromium.org/2522883002)  
Regress: [mjsunit/compiler/regress-664117.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-664117.js)  
```javascript
function foo() {
  return v.length + 1;
}

%PrepareFunctionForOptimization(foo);
var v = [];
foo();
v.length = 0xFFFFFFFF;

%OptimizeFunctionOnNextCall(foo);
assertEquals(4294967296, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d7aae40^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=d7aae40)  
[test/mjsunit/compiler/regress-664117.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-664117.js?cl=d7aae40)  
  

---   

## **regress-crbug-662830.js (chromium issue)**  
   
**[Issue 662830:
 fixed_size_above_fp + (stack_slots * kPointerSize) - CommonFrameConstants::kFixe](https://crbug.com/662830)**  
**[Commit: [interpreter] Fix stack unwinding of deoptimized frames.](https://chromium.googlesource.com/v8/v8/+/a90671f)**  
  
Date(Commit): Tue Nov 22 11:28:45 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "Test-Predator-Wrong", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2517203003](https://codereview.chromium.org/2517203003)  
Regress: [mjsunit/regress/regress-crbug-662830.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662830.js)  
```javascript
function f() {
  %_DeoptimizeNow();
  throw 1;
}

function g() {
  try { f(); } catch(e) { }
  for (var i = 0; i < 3; ++i) if (i === 1) %OptimizeOsr();
  %_DeoptimizeNow();
}

%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a90671f^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=a90671f)  
[test/mjsunit/regress/regress-crbug-662830.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662830.js?cl=a90671f)  
  

---   

## **regress-crbug-667689.js (chromium issue)**  
   
**[Issue 667689:
 Difference between fullcode and ignition_turbo: instanceof and Symbol.hasInstance](https://crbug.com/667689)**  
**[Commit: [turbofan] Fix broken effect chain for instanceof.](https://chromium.googlesource.com/v8/v8/+/84c9360)**  
  
Date(Commit): Tue Nov 22 11:05:35 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Arch-All", "M-57", "NodeJS-Backport-Rejected", "merge-merged-5.5", "merge-merged-5.6", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2518313002](https://codereview.chromium.org/2518313002)  
Regress: [mjsunit/regress/regress-crbug-667689.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-667689.js)  
```javascript
function foo() {}
foo.__defineGetter__(undefined, function() {})

function bar() {}
function baz(x) { return x instanceof bar };
%OptimizeFunctionOnNextCall(baz);
baz();
Object.setPrototypeOf(bar, null);
bar[Symbol.hasInstance] = function() { return true };
assertTrue(baz());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84c9360^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=84c9360)  
[test/mjsunit/regress/regress-crbug-667689.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-667689.js?cl=84c9360)  
  

---   

## **regress-666741.js (chromium issue)**  
   
**[Issue 666741:
 (location_) != nullptr in handles.cc](https://crbug.com/666741)**  
**[Commit: [wasm] Throw a RangeError if Wasm memory could not be allocated.](https://chromium.googlesource.com/v8/v8/+/d0fe942)**  
  
Date(Commit): Mon Nov 21 21:58:53 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://bugs.chromium.org/p/chromium/issues/detail?id=666741](https://bugs.chromium.org/p/chromium/issues/detail?id=666741)  
Regress: [mjsunit/regress/wasm/regress-666741.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-666741.js)  
```javascript
(function __f_7() {
  assertThrows(() => new WebAssembly.Memory({initial: 59199}), RangeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d0fe942^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=d0fe942)  
[test/mjsunit/regress/wasm/regression-666741.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-666741.js?cl=d0fe942)  
  

---   

## **regress-crbug-666742.js (chromium issue)**  
   
**[Issue 666742:
 pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/666742)**  
**[Commit: [ic] Ensure prototype validity cell guards global object's prototype changes for LoadGlobalIC.](https://chromium.googlesource.com/v8/v8/+/8ca50a8)**  
  
Date(Commit): Mon Nov 21 12:46:44 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2512183002](https://codereview.chromium.org/2512183002)  
Regress: [mjsunit/regress/regress-crbug-666742.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-666742.js)  
```javascript
var p = {x:1};
__proto__ = p;
assertEquals(x, 1);
__proto__ = {x:13};
assertEquals(x, 13);
__proto__ = {x:42};
p = null;
gc();
assertEquals(x, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ca50a8^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8ca50a8)  
[test/mjsunit/regress/regress-crbug-666742.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-666742.js?cl=8ca50a8)  
  

---   

## **regress-crbug-664974.js (chromium issue)**  
   
**[Issue 664974:
 NameDictionary::kNotFound == current->property_dictionary()->FindEntry(name) in](https://crbug.com/664974)**  
**[Commit: [ic] Don't check full prototype chain if name is a private symbol.](https://chromium.googlesource.com/v8/v8/+/4513532)**  
  
Date(Commit): Mon Nov 21 11:21:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "M-56", "Hotlist-Merge-Approved", "ReleaseBlock-Stable", "Clusterfuzz", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2513893003](https://codereview.chromium.org/2513893003)  
Regress: [mjsunit/regress/regress-crbug-664974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664974.js)  
```javascript
for (var i = 0; i < 2000; i++) {
  Object.prototype['X'+i] = true;
}

var m = new Map();
m.set(Object.prototype, 23);

var o = {};
m.set(o, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4513532^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=4513532)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664802.js?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664974.js?cl=4513532)  
  

---   

## **regress-crbug-664802.js (chromium issue)**  
   
**[Issue 664802:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/664802)**  
**[Commit: [ic] Don't check full prototype chain if name is a private symbol.](https://chromium.googlesource.com/v8/v8/+/4513532)**  
  
Date(Commit): Mon Nov 21 11:21:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Test-Predator-Wrong", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2513893003](https://codereview.chromium.org/2513893003)  
Regress: [mjsunit/regress/regress-crbug-664802.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664802.js)  
```javascript
var o = {};
o.__proto__ = new Proxy({}, {});

var m = new Map();
m.set({});
m.set(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4513532^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=4513532)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664802.js?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664974.js?cl=4513532)  
  

---   

## **regress-cr-658267.js (chromium issue)**  
   
**[Issue 658267:
 Use-after-poison in v8::internal::List<v8::internal::FuncNameInferrer::Name, v8::internal::ZoneAlloc](https://crbug.com/658267)**  
**[Commit: Fix function name inference corruption for async functions](https://chromium.googlesource.com/v8/v8/+/06f8e87)**  
  
Date(Commit): Fri Nov 18 18:31:54 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "M-55", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "NodeJS-Backport-Rejected", "merge-merged-5.5", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2514893002](https://codereview.chromium.org/2514893002)  
Regress: [mjsunit/regress/regress-cr-658267.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cr-658267.js)  
```javascript
assertThrows("class D extends async() =>", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/06f8e87^!)  
[src/parsing/func-name-inferrer.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/func-name-inferrer.cc?cl=06f8e87)  
[test/mjsunit/regress/regress-cr-658267.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-cr-658267.js?cl=06f8e87)  
  

---   

## **regress-666622.js (chromium issue)**  
   
**[Issue 666622:
 OperatorProperties::GetTotalInputCount(node->op()) == node->InputCount() in veri](https://crbug.com/666622)**  
**[Commit: [builtins] add context input to users of CreateKeyValueArray opcode](https://chromium.googlesource.com/v8/v8/+/e84f0ad)**  
  
Date(Commit): Fri Nov 18 18:17:27 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Merge-Merged", "Hotlist-Merge-Approved", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2515683002](https://codereview.chromium.org/2515683002)  
Regress: [mjsunit/es6/regress/regress-666622.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-666622.js)  
```javascript
function iterateArray() {
  var array = new Array();
  var it = array.entries();
  it.next();
}

function iterateTypedArray() {
  var array = new Uint8Array();
  var it = array.entries();
  it.next();
}

function testArray() {
  iterateArray();
  try {
  } catch (e) {
  }
}
%PrepareFunctionForOptimization(testArray);
testArray();
testArray();
%OptimizeFunctionOnNextCall(testArray);
testArray();

function testTypedArray() {
  iterateTypedArray();
  try {
  } catch (e) {
  }
}
%PrepareFunctionForOptimization(testTypedArray);
testTypedArray();
testTypedArray();
%OptimizeFunctionOnNextCall(testTypedArray);
testTypedArray();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e84f0ad^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=e84f0ad)  
[test/mjsunit/es6/regress/regress-666622.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-666622.js?cl=e84f0ad)  
  

---   

## **regress-crbug-666308.js (chromium issue)**  
   
**[Issue 666308:
 Difference between fullcode and crankshaft_opt: prototype and instanceof](https://crbug.com/666308)**  
**[Commit: [crankshaft] Don't inline the fast path for instanceof if the function has a non-instance .prototype](https://chromium.googlesource.com/v8/v8/+/0c70f37)**  
  
Date(Commit): Fri Nov 18 12:57:37 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2516603002](https://codereview.chromium.org/2516603002)  
Regress: [mjsunit/regress/regress-crbug-666308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-666308.js)  
```javascript
function foo() {}
foo.prototype = 1;
v = new foo();
function bar() { return v instanceof foo; }
assertThrows(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c70f37^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=0c70f37)  
[test/mjsunit/regress/regress-crbug-666308.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-666308.js?cl=0c70f37)  
  

---   

## **regress-crbug-662854.js (chromium issue)**  
   
**[Issue 662854:
 Difference between fullcode and crankshaft_opt: missing reference error](https://crbug.com/662854)**  
**[Commit: [ic] Support data handlers in LoadGlobalIC.](https://chromium.googlesource.com/v8/v8/+/937b8cb)**  
  
Date(Commit): Thu Nov 17 12:18:40 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["merge-merged-5.6", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2511603002](https://codereview.chromium.org/2511603002)  
Regress: [mjsunit/regress/regress-crbug-662854.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662854.js)  
```javascript
function f() {
  typeof boom;
  boom;
}

assertThrows(()=>f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/937b8cb^!)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=937b8cb)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=937b8cb)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=937b8cb)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=937b8cb)  
[src/ic/accessor-assembler-impl.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler-impl.h?cl=937b8cb)  
...  
  

---   

## **regress-crbug-665886.js (chromium issue)**  
   
**[Issue 665886:
 pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/665886)**  
**[Commit: [ic] Invalidate prototype validity cell when a slow prototype becomes fast.](https://chromium.googlesource.com/v8/v8/+/f718cd1)**  
  
Date(Commit): Wed Nov 16 17:45:33 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2502393002](https://codereview.chromium.org/2502393002)  
Regress: [mjsunit/regress/regress-crbug-665886.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665886.js)  
```javascript
[1].toLocaleString();
delete Number.prototype.toLocaleString;
[1].toLocaleString();
var o = {};
o.__proto__ = { toString: Array.prototype.toString };
o.toString();
Number.prototype.arrayToString = Array.prototype.toString;
(42).arrayToString();
var a = [7];
a.toLocaleString();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f718cd1^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=f718cd1)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f718cd1)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=f718cd1)  
[test/mjsunit/regress/regress-crbug-665886.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665886.js?cl=f718cd1)  
  

---   

## **regress-665680.js (chromium issue)**  
   
**[Issue 665680:
 Crash in v8::internal::compiler::AstGraphBuilder::VisitCall](https://crbug.com/665680)**  
**[Commit: [Turbofan] Fix missing break on AstGraphBuilder VisitCall.](https://chromium.googlesource.com/v8/v8/+/94e8417)**  
  
Date(Commit): Wed Nov 16 13:46:42 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Merge-Merged", "Clusterfuzz", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2509643002](https://codereview.chromium.org/2509643002)  
Regress: [mjsunit/compiler/regress-665680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-665680.js)  
```javascript
function foo() {}

var invalidAsmFunction = (function() {
  "use asm";
  return function() {
    with (foo) foo();
  }
})();

%PrepareFunctionForOptimization(invalidAsmFunction);
invalidAsmFunction();
%OptimizeFunctionOnNextCall(invalidAsmFunction);
invalidAsmFunction();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/94e8417^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=94e8417)  
[test/mjsunit/compiler/regress-665680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-665680.js?cl=94e8417)  
  

---   

## **regress-crbug-665587.js (chromium issue)**  
   
**[Issue 665587:
 Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/665587)**  
**[Commit: [turbofan] Fix bogus representation for {kCheckTaggedHole}.](https://chromium.googlesource.com/v8/v8/+/31a8ec7)**  
  
Date(Commit): Wed Nov 16 12:53:47 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Merge-Rejected-56"]  
Code Review: [https://codereview.chromium.org/2504183002](https://codereview.chromium.org/2504183002)  
Regress: [mjsunit/regress/regress-crbug-665587.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665587.js)  
```javascript
var a = new (function() { this[0] = 1 });
function f() {
  for (var i = 0; i < 4; ++i) {
    var x = a[0];
    (function() { return x });
    if (i == 1) %OptimizeOsr();
    gc();
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/31a8ec7^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=31a8ec7)  
[test/mjsunit/regress/regress-crbug-665587.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665587.js?cl=31a8ec7)  
  

---   

## **regress-626986.js (chromium issue)**  
   
**[Issue 626986:
 "maps_pixel_test (with patch)" recently became flaky on Mac, Windows, Linux and Android](https://crbug.com/626986)**  
**[Commit: [turbofan] Fix deopt check for storing into constant field.](https://chromium.googlesource.com/v8/v8/+/1900760)**  
  
Date(Commit): Tue Nov 15 13:17:13 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["M-55", "M-56", "M-54", "Hotlist-Merge-Approved", "ReleaseBlock-Stable", "Via-TryFlakes", "Hotlist-PixelWrangler", "hotlist-infra-opportunity", "NodeJS-Backport-Rejected", "merge-merged-5.5", "merge-merged-5.6"]  
Code Review: [https://codereview.chromium.org/2503863002](https://codereview.chromium.org/2503863002)  
Regress: [mjsunit/compiler/regress-626986.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-626986.js)  
```javascript
function g() {
  return 42;
}

var o = {};

function f(o, x) {
  o.f = x;
}

%PrepareFunctionForOptimization(f);

f(o, g);
f(o, g);
f(o, g);
assertEquals(42, o.f());
%OptimizeFunctionOnNextCall(f);
f(o, function() { return 0; });
assertEquals(0, o.f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1900760^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=1900760)  
[test/mjsunit/compiler/regress-626986.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-626986.js?cl=1900760)  
  

---   

## **regress-660925.js (chromium issue)**  
   
**[Issue 660925:
 pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/660925)**  
**[Commit: [builtins] Take fast path in Array.prototype.keys() only if length is an Smi](https://chromium.googlesource.com/v8/v8/+/2a350ed)**  
  
Date(Commit): Mon Nov 14 18:52:25 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2496323002](https://codereview.chromium.org/2496323002)  
Regress: [mjsunit/es6/regress/regress-660925.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-660925.js)  
```javascript
let array = new Array(0xFFFFFFFF);
let it = array.keys();
assertEquals({ value: 0, done: false }, it.next());

it = array.entries();
assertEquals({ value: [0, undefined], done: false }, it.next());

it = array[Symbol.iterator]();
assertEquals({ value: undefined, done: false }, it.next());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a350ed^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=2a350ed)  
[test/mjsunit/es6/regress/regress-660925.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-660925.js?cl=2a350ed)  
  

---   

## **regress-crbug-664506.js (chromium issue)**  
   
**[Issue 664506:
 Difference between fullcode and ignition_staging: array toString and gc](https://crbug.com/664506)**  
**[Commit: [builtins] Fix pointer comparison in ToString builtin.](https://chromium.googlesource.com/v8/v8/+/79aee39)**  
  
Date(Commit): Mon Nov 14 12:44:29 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Proj-Ignition", "merge-merged-5.6", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2496043003](https://codereview.chromium.org/2496043003)  
Regress: [mjsunit/regress/regress-crbug-664506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664506.js)  
```javascript
gc();
gc();
assertEquals("[object Object]", Object.prototype.toString.call({}));
gc();
assertEquals("[object Array]", Object.prototype.toString.call([]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79aee39^!)  
[src/builtins/builtins-object.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object.cc?cl=79aee39)  
[test/mjsunit/regress/regress-crbug-664506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664506.js?cl=79aee39)  
  

---   

## **regress-crbug-664942.js (chromium issue)**  
   
**[Issue 664942:
 Difference between fullcode and ignition_staging_turbo: string vs. array access](https://crbug.com/664942)**  
**[Commit: [turbofan] Properly allocate constant-folded string.](https://chromium.googlesource.com/v8/v8/+/5667280)**  
  
Date(Commit): Mon Nov 14 11:58:09 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2503433002](https://codereview.chromium.org/2503433002)  
Regress: [mjsunit/regress/regress-crbug-664942.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664942.js)  
```javascript
function foo() {
  return 'x'[0];
}
foo();
%OptimizeFunctionOnNextCall(foo);
assertEquals("x", foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5667280^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=5667280)  
[test/mjsunit/regress/regress-crbug-664942.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664942.js?cl=5667280)  
  

---   

## **regress-664490.js (chromium issue)**  
   
**[Issue 664490:
 Difference between fullcode and turbofan: osr](https://crbug.com/664490)**  
**[Commit: [turbofan] Fix deoptimization of boolean bit constants.](https://chromium.googlesource.com/v8/v8/+/297a969)**  
  
Date(Commit): Mon Nov 14 09:30:19 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["merge-merged-5.6", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2495243002](https://codereview.chromium.org/2495243002)  
Regress: [mjsunit/compiler/regress-664490.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-664490.js)  
```javascript
var foo = function(msg) {};

foo = function (value) {
  assertEquals(false, value);
}

function f() {
  foo(undefined == 0);
}

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/297a969^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=297a969)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=297a969)  
[test/mjsunit/compiler/regress-664490.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-664490.js?cl=297a969)  
  

---   

## **regress-private-enumerable.js (other issue)**  
   
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
[test/mjsunit/regress/regress-private-enumerable.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-private-enumerable.js?cl=942604d)  
  
  
---   

## **regress-crbug-664469.js (chromium issue)**  
   
**[Issue 664469:
 Crash in v8::internal::Simulator::DecodeType3](https://crbug.com/664469)**  
**[Commit: [ic] Fix elements conversion in KeyedStoreGeneric](https://chromium.googlesource.com/v8/v8/+/567904f)**  
  
Date(Commit): Fri Nov 11 13:02:10 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-56", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2492783004](https://codereview.chromium.org/2492783004)  
Regress: [mjsunit/regress/regress-crbug-664469.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664469.js)  
```javascript
function f(a, i) {
  a[i] = "object";
}

f("make it generic", 0);

var kLength = 500000 / 8;
var kValue = 0.1;
var a = new Array(kLength);
for (var i = 0; i < kLength; i++) {
  a[i] = kValue;
}
f(a, 0);
for (var i = 1; i < kLength; i++) {
  assertEquals(kValue, a[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/567904f^!)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=567904f)  
[test/mjsunit/regress/regress-crbug-664469.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664469.js?cl=567904f)  
  

---   

## **regress-664146.js (chromium issue)**  
   
**[Issue 664146:
  Difference between fullcode and ignition_staging: Missing referece error](https://crbug.com/664146)**  
**[Commit: [Interpreter] Fix logical-or/and to ensure it always visits the lhs.](https://chromium.googlesource.com/v8/v8/+/f50f19e)**  
  
Date(Commit): Thu Nov 10 16:31:00 2016  
Components/Type: None/Bug  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2495543002](https://codereview.chromium.org/2495543002)  
Regress: [mjsunit/ignition/regress-664146.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-664146.js)  
```javascript
var foo_call_count = 0;
function foo() { foo_call_count++; }

(true || foo()) ? 1 : 2;
assertTrue(foo_call_count == 0);
(false && foo()) ? 1 : 2;
assertTrue(foo_call_count == 0);

(foo() || true) ? 1 : 2;
assertTrue(foo_call_count == 1);
(false || foo()) ? 1 : 2;
assertTrue(foo_call_count == 2);
(foo() || false) ? 1 : 2;
assertTrue(foo_call_count == 3);

(true && foo()) ? 1 : 2;
assertTrue(foo_call_count == 4);
(foo() && true) ? 1 : 2;
assertTrue(foo_call_count == 5);
(foo() && false) ? 1 : 2;
assertTrue(foo_call_count == 6);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f50f19e^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=f50f19e)  
[test/mjsunit/ignition/regress-664146.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/ignition/regress-664146.js?cl=f50f19e)  
  

---   

## **regress-664087.js (chromium issue)**  
   
**[Issue 664087:
 Difference between fullcode and crankshaft_opt: Missing reference error with valueOf](https://crbug.com/664087)**  
**[Commit: [crankshaft] Always force number representation for increment.](https://chromium.googlesource.com/v8/v8/+/c71e5e1)**  
  
Date(Commit): Thu Nov 10 14:51:18 2016  
Components/Type: None/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2491333002](https://codereview.chromium.org/2491333002)  
Regress: [mjsunit/regress/regress-664087.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-664087.js)  
```javascript
function g() {
  throw 1;
}

var v = { valueOf : g };

function foo(v) {
  v++;
}

%NeverOptimizeFunction(g);
assertThrows(function () { foo(v); });
assertThrows(function () { foo(v); });
%OptimizeFunctionOnNextCall(foo);
assertThrows(function () { foo(v); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c71e5e1^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=c71e5e1)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=c71e5e1)  
[test/mjsunit/regress/regress-664087.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-664087.js?cl=c71e5e1)  
  

---   

## **regress-trap-allocation-memento.js (other issue)**  
   
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
    // Use memento to pre-transition allocation site to DOUBLE elements.
    foo(one, 1.5);
    // Newly created arrays should now have DOUBLE elements right away.
    var two = maker_function();
    assertKind(elements_kind.fast_double, two);
  }

  // Initialize the KeyedStoreIC in foo; the actual operation will be done
  // in the runtime.
  run_test(make1);
  // Run again; the IC optimistically assumed to only see the transitioned
  // (double-elements) map again, so this will make it polymorphic.
  // The actual operation will again be done in the runtime.
  run_test(make2);
  // Finally, check if the initialized IC honors the allocation memento.
  run_test(make3);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc2a277^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=cc2a277)  
[test/mjsunit/regress/regress-trap-allocation-memento.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-trap-allocation-memento.js?cl=cc2a277)  
  
  
---   

## **regress-crbug-664084.js (chromium issue)**  
   
**[Issue 664084:
 Missing ToNumber conversion in Crankshaft.](https://crbug.com/664084)**  
**[Commit: [crankshaft] Not all HAdd instructions produce a number.](https://chromium.googlesource.com/v8/v8/+/6d53340)**  
  
Date(Commit): Thu Nov 10 13:11:28 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2494703002](https://codereview.chromium.org/2494703002)  
Regress: [mjsunit/regress/regress-crbug-664084.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664084.js)  
```javascript
function foo() {
  return +({} + 1);
}

assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d53340^!)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=6d53340)  
[test/mjsunit/regress/regress-crbug-664084.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664084.js?cl=6d53340)  
  

---   

## **regress-crbug-660379.js (chromium issue)**  
   
**[Issue 660379:
 Difference between default and ignition with natives](https://crbug.com/660379)**  
**[Commit: [turbofan] Advance bytecode offset after lazy deopt.](https://chromium.googlesource.com/v8/v8/+/93c6595)**  
  
Date(Commit): Thu Nov 10 11:35:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2487173002](https://codereview.chromium.org/2487173002)  
Regress: [mjsunit/regress/regress-crbug-660379.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-660379.js)  
```javascript
(function InlinedThrowAtEndOfTry() {
  function g() {
    %DeoptimizeFunction(f);
    throw "boom";
  }
  function f() {
    try {
      g();  // Right at the end of try.
    } catch (e) {
      assertEquals("boom", e)
    }
  }
  assertDoesNotThrow(f);
  assertDoesNotThrow(f);
  %OptimizeFunctionOnNextCall(f);
  assertDoesNotThrow(f);
})();

(function InlinedThrowInFrontOfTry() {
  function g() {
    %DeoptimizeFunction(f);
    throw "boom";
  }
  function f() {
    g();  // Right in front of try.
    try {
      Math.random();
    } catch (e) {
      assertUnreachable();
    }
  }
  assertThrows(f);
  assertThrows(f);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93c6595^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=93c6595)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=93c6595)  
[src/builtins/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.h?cl=93c6595)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=93c6595)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=93c6595)  
...  
  

---   

## **regress-crbug-663750.js (chromium issue)**  
   
**[Issue 663750:
 Object.freeze doesn't deoptimize properly](https://crbug.com/663750)**  
**[Commit: [runtime] Ensure Object.freeze() deoptimizes code that depends on global property cells.](https://chromium.googlesource.com/v8/v8/+/6aa16ed)**  
  
Date(Commit): Thu Nov 10 10:37:26 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2488223002](https://codereview.chromium.org/2488223002)  
Regress: [mjsunit/regress/regress-crbug-663750.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663750.js)  
```javascript
var v = 0;
function foo(a) {
  v = a;
}
this.x = 0;
delete x;

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
assertEquals(undefined, v);

Object.freeze(this);

foo(4);
foo(5);
%OptimizeFunctionOnNextCall(foo);
foo(6);
assertEquals(undefined, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6aa16ed^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=6aa16ed)  
[test/mjsunit/regress/regress-crbug-663750.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663750.js?cl=6aa16ed)  
  

---   

## **regress-660813.js (chromium issue)**  
   
**[Issue 660813:
 return_type_->IsReturnType() in asm-typer.cc](https://crbug.com/660813)**  
**[Commit: [wasm] [asm.js] Don't allow bad return types from a global constant](https://chromium.googlesource.com/v8/v8/+/3f2db58)**  
  
Date(Commit): Tue Nov 08 23:32:04 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2481103002](https://codereview.chromium.org/2481103002)  
Regress: [mjsunit/asm/regress-660813.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-660813.js)  
```javascript
function Module() {
  "use asm";
  const i = 0xffffffff;
  function foo() {
    return i;
  }
}
Module();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f2db58^!)  
[src/asmjs/asm-typer.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.cc?cl=3f2db58)  
[test/cctest/asmjs/test-asm-typer.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/asmjs/test-asm-typer.cc?cl=3f2db58)  
[test/mjsunit/asm/asm-validation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/asm-validation.js?cl=3f2db58)  
[test/mjsunit/asm/regress-660813.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-660813.js?cl=3f2db58)  
  

---   

## **regress-crbug-663402.js (chromium issue)**  
   
**[Issue 663402:
 Security: [arm] OOB r/w due to size computation bug in MacroAssembler::Allocate](https://crbug.com/663402)**  
**[Commit: [arm] Fix custom addition in MacroAssembler::[Fast]Allocate](https://chromium.googlesource.com/v8/v8/+/87332fd)**  
  
Date(Commit): Tue Nov 08 18:19:30 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_Impact-Stable", "M-54", "Security_Severity-High", "allpublic", "merge-merged-5.5"]  
Code Review: [https://codereview.chromium.org/2484283002](https://codereview.chromium.org/2484283002)  
Regress: [mjsunit/regress/regress-crbug-663402.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663402.js)  
```javascript
var g_eval = eval;
function emit_f(size) {
  var body = "function f(x) {" +
             "  if (x < 0) return x;" +
             "  var a = [1];" +
             "  if (x > 0) return [";
  for (var i = 0; i < size; i++) {
    body += "0.1, ";
  }
  body += "  ];" +
          "  return a;" +
          "}";
  g_eval(body);
}

var kLength = 701;
emit_f(kLength);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
var a = f(1);

var b = new Object();
for (var i = 0; i < kLength; i++) {
  assertEquals(0.1, a[i]);
}

for (var i = 0; i < 300; i++) {
  f(1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87332fd^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=87332fd)  
[test/mjsunit/regress/regress-crbug-663402.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663402.js?cl=87332fd)  
  

---   

## **regress-662418.js (chromium issue)**  
   
**[Issue 662418:
 Difference between default and ignition: valueOf](https://crbug.com/662418)**  
**[Commit: [Interpreter] Ensure ValueOf is only called once for post-increment operations.](https://chromium.googlesource.com/v8/v8/+/ba5885c)**  
  
Date(Commit): Tue Nov 08 17:03:16 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2473223004](https://codereview.chromium.org/2473223004)  
Regress: [mjsunit/ignition/regress-662418.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-662418.js)  
```javascript
var valueof_calls = 0;

var v = {
  toString: function() {
    var z = w++;
  }
};
var w = {
  valueOf: function() {
    valueof_calls++;
  }
};
var x = { [v]: 'B' };
assertTrue(valueof_calls == 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ba5885c^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=ba5885c)  
[test/cctest/interpreter/bytecode_expectations/AssignmentsInBinaryExpression.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AssignmentsInBinaryExpression.golden?cl=ba5885c)  
[test/cctest/interpreter/bytecode_expectations/CountOperators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/CountOperators.golden?cl=ba5885c)  
[test/cctest/interpreter/bytecode_expectations/GlobalCountOperators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/GlobalCountOperators.golden?cl=ba5885c)  
[test/mjsunit/ignition/regress-662418.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/ignition/regress-662418.js?cl=ba5885c)  
  

---   

## **regress-662904.js (chromium issue)**  
   
**[Issue 662904:
 Difference between fullcode and crankshaft_opt: mock_arguments](https://crbug.com/662904)**  
**[Commit: [crankshaft] FIx for in deopt at the end of the loop.](https://chromium.googlesource.com/v8/v8/+/5d89844)**  
  
Date(Commit): Tue Nov 08 12:33:56 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2476423003](https://codereview.chromium.org/2476423003)  
Regress: [mjsunit/regress/regress-662904.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-662904.js)  
```javascript
function foo(a) {
  var sum = 0;
  var a = [0, "a"];
  for (var i in a) {
    sum += a[i];
  }
  return sum;
}

assertEquals("0a", foo());
assertEquals("0a", foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals("0a", foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d89844^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=5d89844)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=5d89844)  
[test/mjsunit/regress/regress-662904.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-662904.js?cl=5d89844)  
  

---   

## **regress-662845.js (chromium issue)**  
   
**[Issue 662845:
 Difference between fullcode and crankshaft_opt: plain arguments](https://crbug.com/662845)**  
**[Commit: [crankshaft] Do not optimize argument access if any parameter is context-allocated.](https://chromium.googlesource.com/v8/v8/+/7f801ff)**  
  
Date(Commit): Mon Nov 07 19:10:15 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2484723002](https://codereview.chromium.org/2484723002)  
Regress: [mjsunit/regress/regress-662845.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-662845.js)  
```javascript
function foo(x) {
  (function() { x = 1; })()
  return arguments[0];
}

assertEquals(1, foo(42));
assertEquals(1, foo(42));
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo(42));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7f801ff^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=7f801ff)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=7f801ff)  
[test/mjsunit/regress/regress-662845.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-662845.js?cl=7f801ff)  
  

---   

## **regress-crbug-662410.js (chromium issue)**  
   
**[Issue 662410:
 Crash in v8::internal::Invoke](https://crbug.com/662410)**  
**[Commit: [turbofan] Properly rename receiver on CheckHeapObject.](https://chromium.googlesource.com/v8/v8/+/a758c19)**  
  
Date(Commit): Mon Nov 07 08:41:34 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-56", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2485563002](https://codereview.chromium.org/2485563002)  
Regress: [mjsunit/regress/regress-crbug-662410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662410.js)  
```javascript
function g(v) { return v.constructor; }

g({});
g({});

function f() {
  var i = 0;
  do {
    i = i + 1;
    g(i);
  } while (i < 1);
}

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a758c19^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=a758c19)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=a758c19)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=a758c19)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=a758c19)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=a758c19)  
...  
  

---   

## **regress-crbug-662367.js (chromium issue)**  
   
**[Issue 662367:
 Different division results with Infinity and NaN between default and ignition](https://crbug.com/662367)**  
**[Commit: [crankshaft] Fix constant folding of HDiv instruction.](https://chromium.googlesource.com/v8/v8/+/9906b3e)**  
  
Date(Commit): Fri Nov 04 15:08:12 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2472413002](https://codereview.chromium.org/2472413002)  
Regress: [mjsunit/regress/regress-crbug-662367.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662367.js)  
```javascript
var zero = 0;

(function ConstantFoldZeroDivZero() {
  function f() {
    return 0 / zero;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();

(function ConstantFoldMinusZeroDivZero() {
  function f() {
    return -0 / zero;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();

(function ConstantFoldNaNDivZero() {
  function f() {
    return NaN / 0;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9906b3e^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=9906b3e)  
[test/mjsunit/regress/regress-crbug-662367.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662367.js?cl=9906b3e)  
  

---   

## **regress-5598.js (v8 issue)**  
   
**[Issue 5598:
 V8 crashes with --turbo-escape on string destructuring](https://crbug.com/v8/5598)**  
**[Commit: [builtins] fix Allocate() call in ReduceStringIterator()](https://chromium.googlesource.com/v8/v8/+/cbadac5)**  
  
Date(Commit): Fri Nov 04 05:45:15 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2478643002](https://codereview.chromium.org/2478643002)  
Regress: [mjsunit/es6/regress/regress-5598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-5598.js)  
```javascript
function fn(a) {
  var [b] = a;
  return b;
}

%PrepareFunctionForOptimization(fn);
fn('a');
fn('a');
%OptimizeFunctionOnNextCall(fn);

fn('a');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cbadac5^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=cbadac5)  
[test/mjsunit/es6/regress/regress-5598.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-5598.js?cl=cbadac5)  
  

---   

## **regress-crbug-661949.js (chromium issue)**  
   
**[Issue 661949:
 We should never get here - unexpected deopt info in deoptimizer.cc](https://crbug.com/661949)**  
**[Commit: [turbofan] CheckBounds cannot be used within asm.js.](https://chromium.googlesource.com/v8/v8/+/1003374)**  
  
Date(Commit): Thu Nov 03 12:35:04 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2474013002](https://codereview.chromium.org/2474013002)  
Regress: [mjsunit/regress/regress-crbug-661949.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-661949.js)  
```javascript
var foo = (function() {
  'use asm';
  function foo() { ''[0]; }
  return foo;
})();

foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1003374^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=1003374)  
[src/compiler/node-matchers.h](https://cs.chromium.org/chromium/src/v8/src/compiler/node-matchers.h?cl=1003374)  
[test/mjsunit/regress/regress-crbug-661949.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-661949.js?cl=1003374)  
  

---   

## **regress-crbug-659915a.js (chromium issue)**  
   
**[Issue 659915:
 Bug with let variable in Chrome](https://crbug.com/659915)**  
**[Commit: [turbofan] Disable usage of {maybe_assigned} variable flag.](https://chromium.googlesource.com/v8/v8/+/b02e7fb)**  
  
Date(Commit): Thu Nov 03 10:24:06 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["M-54", "Arch-All"]  
Code Review: [https://codereview.chromium.org/2471523005](https://codereview.chromium.org/2471523005)  
Regress: [mjsunit/regress/regress-crbug-659915a.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659915a.js), [mjsunit/regress/regress-crbug-659915b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659915b.js)  
```javascript
let x;
function f(a) {
  x += a;
}
function g(a) {
  f(a); return x;
}
function h(a) {
  x = a; return x;
}

function boom() { return g(1) }

assertEquals(1, h(1));
assertEquals(2, boom());
assertEquals(3, boom());
%OptimizeFunctionOnNextCall(boom);
assertEquals(4, boom());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b02e7fb^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=b02e7fb)  
[test/mjsunit/regress/regress-crbug-659915a.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659915a.js?cl=b02e7fb)  
[test/mjsunit/regress/regress-crbug-659915b.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659915b.js?cl=b02e7fb)  
  

---   

## **regress-crbug-658185.js (chromium issue)**  
   
**[Issue 658185:
 !escape_analysis_->IsVirtual(node) in escape-analysis-reducer.cc](https://crbug.com/658185)**  
**[Commit: [turbofan] Properly deal with out-of-bounds fields in EscapeAnalysis.](https://chromium.googlesource.com/v8/v8/+/7201bad)**  
  
Date(Commit): Mon Oct 31 06:43:25 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2459273002](https://codereview.chromium.org/2459273002)  
Regress: [mjsunit/regress/regress-crbug-658185.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658185.js)  
```javascript
var t = 0;
function foo() {
  var o = {x:1};
  var p = {y:2.5, x:0};
  o = [];
  for (var i = 0; i < 2; ++i) {
    t = o.x;
    o = p;
  }
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7201bad^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=7201bad)  
[test/mjsunit/regress/regress-crbug-658185.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658185.js?cl=7201bad)  
  

---   

## **regress-v8-5573.js (v8 issue)**  
   
**[Issue 5573:
 Representation check in simplified-lowering fails](https://crbug.com/v8/5573)**  
**[Commit: [turbofan] Relax a too-strict dcheck.](https://chromium.googlesource.com/v8/v8/+/21d55e2)**  
  
Date(Commit): Thu Oct 27 12:33:19 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2458623002](https://codereview.chromium.org/2458623002)  
Regress: [mjsunit/compiler/regress-v8-5573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-5573.js)  
```javascript
var global = true;
global = false;

function f() {
  return !global;
}

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
assertTrue(f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21d55e2^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=21d55e2)  
[test/mjsunit/compiler/regress-v8-5573.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-v8-5573.js?cl=21d55e2)  
  

---   

## **regress-crbug-659967.js (chromium issue)**  
   
**[Issue 659967:
 right->IsUndefined(isolate_) || right->IsBoolean() in ic-state.cc](https://crbug.com/659967)**  
**[Commit: [ic] Properly deal with all oddballs when updating BinaryOpIC state.](https://chromium.googlesource.com/v8/v8/+/305948f)**  
  
Date(Commit): Thu Oct 27 12:16:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2453633005](https://codereview.chromium.org/2453633005)  
Regress: [mjsunit/regress/regress-crbug-659967.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659967.js)  
```javascript
function f() { null >> arguments; }

f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/305948f^!)  
[src/ic/ic-state.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic-state.cc?cl=305948f)  
[test/mjsunit/regress/regress-crbug-659967.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659967.js?cl=305948f)  
  

---   

## **regress-5566.js (v8 issue)**  
   
**[Issue 5566:
 Some RegExp static properties are mistakenly non-configurable](https://crbug.com/v8/5566)**  
**[Commit: [regexp] Set static property attributes as in spec proposal](https://chromium.googlesource.com/v8/v8/+/88c5a30)**  
  
Date(Commit): Thu Oct 27 08:26:05 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2452913002](https://codereview.chromium.org/2452913002)  
Regress: [mjsunit/regress/regress-5566.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5566.js)  
```javascript
const props = [ "input", "$_"
              , "lastMatch", "$&"
              , "lastParen", "$+"
              , "leftContext", "$`"
              , "rightContext", "$'"
              , "$1", "$2", "$3", "$4", "$5", "$6", "$7", "$8", "$9"
              ];

for (let i = 0; i < props.length; i++) {
  const prop = props[i];
  const desc = Object.getOwnPropertyDescriptor(RegExp, prop);
  assertTrue(desc.configurable, prop);
  assertFalse(desc.enumerable, prop);
  assertTrue(desc.get !== undefined, prop);

  // TODO(jgruber): Although the spec proposal specifies setting setters to
  // undefined, we are not sure that this change would be web-compatible, and
  // we are intentionally sticking with the old behavior for now.
  assertTrue(desc.set !== undefined, prop);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88c5a30^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=88c5a30)  
[test/mjsunit/regress/regress-5566.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5566.js?cl=88c5a30)  
  

---   

## **regress-crbug-658691.js (chromium issue)**  
   
**[Issue 658691:
 Stack-overflow in v8::internal::Invoke](https://crbug.com/658691)**  
**[Commit: [turbofan] Disable bogus lowering of builtin tail-calls.](https://chromium.googlesource.com/v8/v8/+/2ab2ec2)**  
  
Date(Commit): Wed Oct 26 12:49:06 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2447383002](https://codereview.chromium.org/2447383002)  
Regress: [mjsunit/regress/regress-crbug-658691.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658691.js)  
```javascript
function f(a, b, c) {
  "use strict";
  return Reflect.set({});
}

function g() {
  return f() + "-no-tail";
}

assertEquals("true-no-tail", g());
%OptimizeFunctionOnNextCall(f);
assertEquals("true-no-tail", g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ab2ec2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=2ab2ec2)  
[test/mjsunit/regress/regress-crbug-658691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658691.js?cl=2ab2ec2)  
  

---   

## **regress-crbug-659475-1.js (chromium issue)**  
   
**[Issue 659475:
 Pwn2Own: V8 OOB Bug.](https://crbug.com/659475)**  
**[Commit: [compiler] Properly validate stable map assumption for globals.](https://chromium.googlesource.com/v8/v8/+/3aa57eb)**  
  
Date(Commit): Wed Oct 26 08:55:10 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Release-2-M54", "CVE-2016-5198", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/2444233004](https://codereview.chromium.org/2444233004)  
Regress: [mjsunit/regress/regress-crbug-659475-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659475-1.js), [mjsunit/regress/regress-crbug-659475-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659475-2.js)  
```javascript
var n;

function Ctor() {
  n = new Set();
}

function Check() {
  n.xyz = 0x826852f4;
}

Ctor();
Ctor();
%OptimizeFunctionOnNextCall(Ctor);
Ctor();

Check();
Check();
%OptimizeFunctionOnNextCall(Check);
Check();

Ctor();
Check();

parseInt('AAAAAAAA');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3aa57eb^!)  
[src/compiler/js-global-object-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-global-object-specialization.cc?cl=3aa57eb)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=3aa57eb)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3aa57eb)  
[src/runtime/runtime-utils.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-utils.h?cl=3aa57eb)  
[test/mjsunit/regress/regress-crbug-659475-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659475-1.js?cl=3aa57eb)  
...  
  

---   

## **regress-crbug-658528.js (chromium issue)**  
   
**[Issue 658528:
 variable->is_this() && variable->mode() == CONST && op == Token::INIT in bytecod](https://crbug.com/658528)**  
**[Commit: [ignition] Use more-targeted check for CONST-this-initialization hole check](https://chromium.googlesource.com/v8/v8/+/56626f3)**  
  
Date(Commit): Tue Oct 25 11:08:06 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2447783002](https://codereview.chromium.org/2447783002)  
Regress: [mjsunit/regress/regress-crbug-658528.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658528.js)  
```javascript
function f() {
  eval("var x = 1");
  const x = 2;
}

assertThrows(f, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56626f3^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=56626f3)  
[test/mjsunit/regress/regress-crbug-658528.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658528.js?cl=56626f3)  
  

---   

## **regress-655573.js (chromium issue)**  
   
**[Issue 655573:
 Check failed: size <= Page::kMaxRegularHeapObjectSize.](https://crbug.com/655573)**  
**[Commit: Don't call FastNewFunctionContextStub if context is bigger than kMaxRegularHeapObjectSize.](https://chromium.googlesource.com/v8/v8/+/381b543)**  
  
Date(Commit): Mon Oct 24 17:23:21 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "hasbisect", "M-56", "Via-Wizard", "M-54", "ReleaseBlock-Stable", "merge-merged-5.4", "TE-Verified-M55", "NodeJS-Backport-Done", "merge-merged-5.5", "pre-stable-54.0.2840.59", "TE-Verified-55.0.2883.34"]  
Code Review: [https://codereview.chromium.org/2177273002](https://codereview.chromium.org/2177273002)  
Regress: [mjsunit/regress/regress-655573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-655573.js)  
```javascript
source = "(function() {\n"
for (var i = 0; i < 65000; i++) {
  source += "  var a_" + i + " = 0;\n";
}
source += "  return function() {\n"
for (var i = 0; i < 65000; i++) {
  source += "a_" + i + "++;\n";
}
source += "}})();\n"

eval(source);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/381b543^!)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=381b543)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=381b543)  
[src/crankshaft/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-codegen-arm.cc?cl=381b543)  
[src/crankshaft/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-codegen-arm64.cc?cl=381b543)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=381b543)  
...  
  

---   

## **regress-5434.js (v8 issue)**  
   
**[Issue 5434:
 [regexp] Additional 'exec' property access violates spec](https://crbug.com/v8/5434)**  
**[Commit: [regexp] Add regression test for v8:5434](https://chromium.googlesource.com/v8/v8/+/f87d73c)**  
  
Date(Commit): Mon Oct 24 10:39:01 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2440413002](https://codereview.chromium.org/2440413002)  
Regress: [mjsunit/regress/regress-5434.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5434.js)  
```javascript
var lastIndexHasBeenSet = false;
var countOfExecGets = 0;

class ObservableExecRegExp extends RegExp {
  constructor(pattern, flags) {
    super(pattern, flags);
    this.lastIndex = 42;

    const re = this;
    Object.defineProperty(this, "exec", {
      get: function() {
        // Ensure exec is first accessed after lastIndex has been reset to
        // satisfy the spec.
        assertTrue(re.lastIndex != 42);
        countOfExecGets++;
        return RegExp.prototype.exec;
      }
    });
  }
}



var re = new ObservableExecRegExp(/x/);

assertEquals(42, re.lastIndex);
assertEquals(0, countOfExecGets);

var result = "axbxc".split(re);

assertEquals(5, countOfExecGets);
assertEquals(["a", "b", "c"], result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f87d73c^!)  
[test/mjsunit/regress/regress-5434.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5434.js?cl=f87d73c)  
  

---   

## **regress-crbug-657478.js (chromium issue)**  
   
**[Issue 657478:
 Phi of kRepFloat64 ((Unsigned32 | Negative31)) cannot be changed to kRepWord32 i](https://crbug.com/657478)**  
**[Commit: [turbofan] Fix typed lowering of JSToLength.](https://chromium.googlesource.com/v8/v8/+/a58d790)**  
  
Date(Commit): Mon Oct 24 06:37:22 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.5"]  
Code Review: [https://codereview.chromium.org/2440333002](https://codereview.chromium.org/2440333002)  
Regress: [mjsunit/regress/regress-crbug-657478.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-657478.js)  
```javascript
function foo(o) { return %_ToLength(o.length); }

foo(new Array(4));
foo(new Array(Math.pow(2, 32) - 1));
foo({length: 10});
%OptimizeFunctionOnNextCall(foo);
foo({length: 10});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a58d790^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=a58d790)  
[test/mjsunit/regress/regress-crbug-657478.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-657478.js?cl=a58d790)  
  

---   

## **regress-5531.js (v8 issue)**  
   
**[Issue 5531:
 v8_wasm_code_fuzzer: Check failed: val <= std::numeric_limits<N>::max() (1238476 vs. 65535).](https://crbug.com/v8/5531)**  
**[Commit: [turbofan] Use uint32 to store the number of control outputs instead of uint16.](https://chromium.googlesource.com/v8/v8/+/2f3ca96)**  
  
Date(Commit): Wed Oct 19 07:25:51 2016  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/2425983002](https://chromiumcodereview.appspot.com/2425983002)  
Regress: [mjsunit/regress/wasm/regress-5531.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-5531.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(1, 1, false);
  builder.addFunction("foo", kSig_i_v)
    .addBody([
              kExprI32Const, 0x00,
      kExprI32Const, 0x0b,
      kExprI32Const, 0x0f,
      kExprBrTable, 0xcb, 0xcb, 0xcb, 0x00, 0x00, 0xcb, 0x00 // entries=1238475
              ])
              .exportFunc();
  assertThrows(function() { builder.instantiate(); });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f3ca96^!)  
[src/compiler/operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.cc?cl=2f3ca96)  
[src/compiler/operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.h?cl=2f3ca96)  
[test/mjsunit/regress/wasm/regression-5531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-5531.js?cl=2f3ca96)  
  

---   

## **regress-5538.js (v8 issue)**  
   
**[Issue 5538:
 Invalid ToInt32 truncation for inlined Number.parseInt code](https://crbug.com/v8/5538)**  
**[Commit: [turbofan] Fix invalid Number.parseInt inlining.](https://chromium.googlesource.com/v8/v8/+/3a7eac1)**  
  
Date(Commit): Wed Oct 19 05:17:52 2016  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/2432143002](https://chromiumcodereview.appspot.com/2432143002)  
Regress: [mjsunit/compiler/regress-5538.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5538.js)  
```javascript
(function() {
  function foo(x) {
    x = x | 0;
    return Number.parseInt(x + 1);
  }

  %PrepareFunctionForOptimization(foo);
  assertEquals(1, foo(0));
  assertEquals(2, foo(1));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(Math.pow(2, 31), foo(Math.pow(2, 31) - 1));
})();

(function() {
  function foo(x) {
    x = x | 0;
    return Number.parseInt(x + 1, 0);
  }

  %PrepareFunctionForOptimization(foo);
  assertEquals(1, foo(0));
  assertEquals(2, foo(1));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(Math.pow(2, 31), foo(Math.pow(2, 31) - 1));
})();

(function() {
  function foo(x) {
    x = x | 0;
    return Number.parseInt(x + 1, 10);
  }

  %PrepareFunctionForOptimization(foo);
  assertEquals(1, foo(0));
  assertEquals(2, foo(1));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(Math.pow(2, 31), foo(Math.pow(2, 31) - 1));
})();

(function() {
  function foo(x) {
    x = x | 0;
    return Number.parseInt(x + 1, undefined);
  }

  %PrepareFunctionForOptimization(foo);
  assertEquals(1, foo(0));
  assertEquals(2, foo(1));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(Math.pow(2, 31), foo(Math.pow(2, 31) - 1));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3a7eac1^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=3a7eac1)  
[test/mjsunit/compiler/regress-5538.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5538.js?cl=3a7eac1)  
[test/unittests/compiler/js-builtin-reducer-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-builtin-reducer-unittest.cc?cl=3a7eac1)  
  

---   

## **regress-653407.js (chromium issue)**  
   
**[Issue 653407:
 'this is undefined' in a subclass super call.](https://crbug.com/653407)**  
**[Commit: [turbofan] When inlining JSCallConstruct receiver should be set to the hole.](https://chromium.googlesource.com/v8/v8/+/cad3665)**  
  
Date(Commit): Tue Oct 18 11:48:15 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Via-Wizard", "Arch-x86_64", "Proj-Ignition"]  
Code Review: [https://codereview.chromium.org/2424123002](https://codereview.chromium.org/2424123002)  
Regress: [mjsunit/regress/regress-653407.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-653407.js)  
```javascript
class superClass {
  constructor () {}
}

class subClass extends superClass {
  constructor () {
    super();
  }
}

function f() {
 new subClass();
}

f(); // We need this to collect feedback, so that subClass gets inlined in f.
%OptimizeFunctionOnNextCall(f)
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cad3665^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=cad3665)  
[test/mjsunit/regress/regress-653407.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-653407.js?cl=cad3665)  
  

---   

## **regress-crbug-656037.js (chromium issue)**  
   
**[Issue 656037:
 Array.push sometimes returns the wrong thing](https://crbug.com/656037)**  
**[Commit: [turbofan] Fix return value of Array.prototype.push.](https://chromium.googlesource.com/v8/v8/+/8584442)**  
  
Date(Commit): Tue Oct 18 08:02:25 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Via-Wizard", "Needs-Feedback", "M-54", "Hotlist-Merge-Approved", "Arch-All", "merge-merged-5.4", "Merge-Merged-54", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup", "Merge-Merged-55", "pre-stable-54.0.2840.59"]  
Code Review: [https://codereview.chromium.org/2425903002](https://codereview.chromium.org/2425903002)  
Regress: [mjsunit/regress/regress-crbug-656037.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-656037.js)  
```javascript
function foo(a) {
  return a.push(true);
}

var a = [];
assertEquals(1, foo(a));
assertEquals(2, foo(a));
%OptimizeFunctionOnNextCall(foo);
assertEquals(3, foo(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8584442^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=8584442)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=8584442)  
[test/mjsunit/regress/regress-crbug-656037.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-656037.js?cl=8584442)  
  

---   

## **regress-crbug-656275.js (chromium issue)**  
   
**[Issue 656275:
 NumberFround of kRepFloat32 (Number) cannot be changed to kRepTaggedSigned in re](https://crbug.com/656275)**  
**[Commit: [turbofan] Add missing Float32 -> TaggedSigned conversion.](https://chromium.googlesource.com/v8/v8/+/96f1327)**  
  
Date(Commit): Mon Oct 17 05:41:09 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2421193002](https://codereview.chromium.org/2421193002)  
Regress: [mjsunit/regress/regress-crbug-656275.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-656275.js)  
```javascript
var a = 1;

function foo(x) { a = Math.fround(x + 1); }

foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo(1.3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96f1327^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=96f1327)  
[test/mjsunit/regress/regress-crbug-656275.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-656275.js?cl=96f1327)  
  

---   

## **regress-654377.js (chromium issue)**  
   
**[Issue 654377:
 Crash in v8::internal::wasm::WasmFullDecoder::CreateOrMergeIntoPhi](https://crbug.com/654377)**  
**[Commit: [wasm] Do not create TF nodes during verification](https://chromium.googlesource.com/v8/v8/+/0e1f6d8)**  
  
Date(Commit): Thu Oct 13 08:21:47 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "findit-wrong", "Clusterfuzz", "ClusterFuzz-Verified", "Stability-AFL"]  
Code Review: [https://codereview.chromium.org/2403013002](https://codereview.chromium.org/2403013002)  
Regress: [mjsunit/regress/wasm/regress-654377.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-654377.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(1, 1, false);
  builder.addFunction("foo", kSig_i_v)
    .addBody([
              kExprI32Const, 00,
              kExprMemorySize,
              kExprBrIf, 00,
              kExprMemorySize,
              kExprBr, 0xe7, 0xd2, 0xf2, 0xff, 0x1d
              ])
              .exportFunc();
  assertThrows(function() { builder.instantiate(); });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0e1f6d8^!)  
[src/wasm/ast-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/ast-decoder.cc?cl=0e1f6d8)  
[test/mjsunit/regress/wasm/regression-654377.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-654377.js?cl=0e1f6d8)  
  

---   

## **regress-crbug-645438.js (chromium issue)**  
   
**[Issue 645438:
 Security: js input crashes Chrome on FPE; seemingly non-deterministic in the v8 shell?](https://crbug.com/645438)**  
**[Commit: [crankshaft] Range analysis should not rely on overflowed ranges.](https://chromium.googlesource.com/v8/v8/+/9a0109d)**  
  
Date(Commit): Wed Oct 12 09:06:32 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "M-55", "M-53", "M-54", "Hotlist-Merge-Approved", "allpublic", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup"]  
Code Review: [https://codereview.chromium.org/2412853002](https://codereview.chromium.org/2412853002)  
Regress: [mjsunit/regress/regress-crbug-645438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645438.js)  
```javascript
function n(x,y){
  y = (y-(0x80000000|0)|0);
  return (x/y)|0;
};
var x = -0x80000000;
var y = 0x7fffffff;
n(x,y);
n(x,y);
%OptimizeFunctionOnNextCall(n);
assertEquals(x, n(x,y));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a0109d^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=9a0109d)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=9a0109d)  
[test/mjsunit/regress/regress-crbug-645438.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645438.js?cl=9a0109d)  
  

---   

## **regress-crbug-655004.js (chromium issue)**  
   
**[Issue 655004:
 map == GetHeap()->fixed_array_map() || map == GetHeap()->fixed_cow_array_map() i](https://crbug.com/655004)**  
**[Commit: [turbofan] Fix effect chain for polymorphic array access.](https://chromium.googlesource.com/v8/v8/+/edfe391)**  
  
Date(Commit): Wed Oct 12 08:31:55 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "findit-wrong", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup"]  
Code Review: [https://codereview.chromium.org/2411273002](https://codereview.chromium.org/2411273002)  
Regress: [mjsunit/regress/regress-crbug-655004.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-655004.js)  
```javascript
function foo(a) {
  a.x = 0;
  if (a.x === 0) a[1] = 0.1;
  a.x = {};
}
foo(new Array(1));
foo(new Array(1));
%OptimizeFunctionOnNextCall(foo);
foo(new Array(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/edfe391^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=edfe391)  
[test/mjsunit/regress/regress-crbug-655004.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-655004.js?cl=edfe391)  
  

---   

## **regress-crbug-654723.js (chromium issue)**  
   
**[Issue 654723:
 this->first()->IsSeqString() || this->first()->IsExternalString() in objects-deb](https://crbug.com/654723)**  
**[Commit: [turbofan] Respect ConsString invariant.](https://chromium.googlesource.com/v8/v8/+/a4f37da)**  
  
Date(Commit): Wed Oct 12 07:00:52 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-5.5"]  
Code Review: [https://codereview.chromium.org/2410893003](https://codereview.chromium.org/2410893003)  
Regress: [mjsunit/regress/regress-crbug-654723.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-654723.js)  
```javascript
var k = "0101010101010101" + "01010101";

function foo(s) {
  return k + s;
}

foo("a");
foo("a");
%OptimizeFunctionOnNextCall(foo);
var x = foo("");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4f37da^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=a4f37da)  
[test/mjsunit/regress/regress-crbug-654723.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-654723.js?cl=a4f37da)  
  

---   

## **regress-648079.js (chromium issue)**  
   
**[Issue 648079:
 <no crash state available> Fatal error in afl_v8_wasm_fuzzer](https://crbug.com/648079)**  
**[Commit: [wasm] Simd128 types should not be available in asmjs modules.](https://chromium.googlesource.com/v8/v8/+/19dab88)**  
  
Date(Commit): Fri Oct 07 07:52:19 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Stability-AFL"]  
Code Review: [https://codereview.chromium.org/2400863003](https://codereview.chromium.org/2400863003)  
Regress: [mjsunit/regress/wasm/regress-648079.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-648079.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let kSig_s_v = makeSig([], [kWasmS128]);
let kExprS128LoadMem = 0xc0;

(function() {
"use asm";
var builder = new WasmModuleBuilder();
builder.addFunction("regression_648079", kSig_s_v)
  .addBody([
    // locals:
    0x00,
    // body:
    kExprI64RemU,
    kExprI64Ctz,
    kExprI64LeU,
    kExprUnreachable,
    kExprUnreachable,
    kExprUnreachable,
    kExprUnreachable,
    kExprI64Ctz,
    kExprI64Ne,
    kExprI64ShrS,
    kExprI64GtS,
    kExprI64RemU,
    kExprUnreachable,
    kExprI64RemU,
    kExprI32Eqz,
    kExprI64LeU,
    kExprDrop,
    kExprF32Add,
    kExprI64Ior,
    kExprF32CopySign,
    kExprI64Ne,
    kExprI64GeS,
    kExprUnreachable,
    kExprF32Trunc,
    kExprF32Trunc,
    kExprUnreachable,
    kExprIf, 10,   // @32
      kExprBlock, 00,   // @34
        kExprBr,   // depth=109
        kExprI64Shl,
        kExprI64LeU,
        kExprI64GeS,
        kExprI64Clz,
        kExprF32Min,
        kExprF32Eq,
        kExprF32Trunc,
        kExprF32Trunc,
        kExprF32Trunc,
        kExprUnreachable,
        kExprI32Const,
        kExprUnreachable,
        kExprBr,   // depth=101
        kExprF32Div,
        kExprI64GtU,
        kExprI64GeS,
        kExprI64Clz,
        kExprSelect,
        kExprI64GtS,
        kExprI64RemU,
        kExprI64LeU,
        kExprI64Shl,
        kExprI64Ctz,
        kExprLoop, 01,   // @63 i32
        kExprElse,   // @65
          kExprI64LeU,
          kExprI64RemU,
          kExprI64Ne,
          kExprI64GeS,
          kExprI32Const,
          kExprI64GtS,
          kExprI64LoadMem32U,
          kExprI64Clz,
          kExprI64Shl,
          kExprI64Ne,
          kExprI64ShrS,
          kExprI64GtS,
          kExprI64DivU,
          kExprI64Ne,
          kExprI64GtS,
          kExprI64Ne,
          kExprI64Popcnt,
          kExprI64DivU,
          kExprI64DivU,
          kExprSelect,
          kExprI64Ctz,
          kExprI64Popcnt,
          kExprI64RemU,
          kExprI64Clz,
          kExprF64Sub,
          kExprF32Trunc,
          kExprF32Trunc,
          kExprI64RemU,
          kExprI64Ctz,
          kExprI64LeU,
          kExprUnreachable,
          kExprUnreachable,
          kExprUnreachable,
          kExprBrIf,   // depth=116
          kExprF32Min,
          kExprI64GtU,
          kExprBlock, 01,   // @107 i32
            kExprTeeLocal,
            kExprBlock, 01,   // @111 i32
              kExprBlock, 01,   // @113 i32
                kExprBlock, 01,   // @115 i32
                  kExprBlock, 01,   // @117 i32
                    kExprBlock, 01,   // @119 i32
                      kExprBlock, 01,   // @121 i32
                        kExprBlock, 01,   // @123 i32
                          kExprBlock, 88,   // @125
                            kExprF32Trunc,
                            kExprF32Trunc,
                            kExprF32Trunc,
                            kExprUnreachable,
                            kExprLoop, 40,   // @131
                              kExprUnreachable,
                              kExprUnreachable,
                              kExprI32Add,
                              kExprBlock, 05,   // @136
                                kExprUnreachable,
                                kExprIf, 02,   // @139 i64
                                  kExprBlock, 01,   // @141 i32
                                    kExprBrIf,   // depth=16
                                    kExprLoop, 00,   // @145
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprReturn,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprI64LoadMem16U,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprNop,
                                      kExprBr,   // depth=1
                                    kExprElse,   // @164
                                      kExprF32Trunc,
                                      kExprI32Add,
                                      kExprCallIndirect,   // sig #1
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprUnreachable,
                                      kExprBlock, 00,   // @172
                                        kExprI64RemU,
                                        kExprI64Ctz,
                                        kExprI64LeU,
                                        kExprUnreachable,
                                        kExprUnreachable,
                                        kExprUnreachable,
                                        kExprUnreachable,
                                        kExprUnreachable,
                                        kExprDrop,
                                        kExprI64Popcnt,
                                        kExprF32Min,
                                        kExprUnreachable,
                                        kExprF64Sub,
                                        kExprI32Const,
                                        kExprUnreachable,
                                        kExprGetLocal,
                                        kExprI64LoadMem32U,
                                        kExprUnreachable,
                                        kExprI64RemU,
                                        kExprI32Eqz,
                                        kExprI64LeU,
                                        kExprDrop,
                                        kExprF32Add,
                                        kExprI64Ior,
                                        kExprF32CopySign,
                                        kExprI64Ne,
                                        kExprI64GeS,
                                        kExprUnreachable,
                                        kExprF32Trunc,
                                        kExprF32Trunc,
                                        kExprUnreachable,
                                        kExprIf, 10,   // @216
                                          kExprBlock, 00,   // @218
                                            kExprBr,   // depth=109
                                            kExprI64Shl,
                                            kExprI64LeU,
                                            kExprI64GeS,
                                            kExprI64Clz,
                                            kExprF32Min,
                                            kExprF32Eq,
                                            kExprF32Trunc,
                                            kExprF32Trunc,
                                            kExprF32Trunc,
                                            kExprUnreachable,
                                            kExprF64Min,
                                            kExprI32Const,
                                            kExprBr,   // depth=101
                                            kExprF32Div,
                                            kExprI64GtU,
                                            kExprI64GeS,
                                            kExprI64Clz,
                                            kExprI64Popcnt,
                                            kExprF64Lt,
                                            kExprF32Trunc,
                                            kExprF32Trunc,
                                            kExprF32Trunc,
                                            kExprUnreachable,
                                            kExprLoop, 01,   // @247 i32
                                            kExprElse,   // @249
                                              kExprI64LeU,
                                              kExprI64RemU,
                                              kExprI64Ne,
                                              kExprI64GeS,
                                              kExprI32Const,
                                              kExprBlock, 01,   // @256 i32
                                                kExprBlock, 01,   // @258 i32
                                                  kExprBlock, 01,   // @260 i32
                                                    kExprBlock, 01,   // @262 i32
                                                      kExprBlock, 01,   // @264 i32
                                                        kExprF32Ge,
                                                        kExprF32Trunc,
                                                        kExprF32Trunc,
                                                        kExprF32Trunc,
                                                        kExprUnreachable,
                                                        kExprLoop, 40,   // @271
                                                          kExprUnreachable,
                                                          kExprUnreachable,
                                                          kExprI32Add,
                                                          kExprBlock, 01,   // @276 i32
                                                            kExprUnreachable,
                                                            kExprIf, 02,   // @279 i64
                                                              kExprBlock, 00,   // @281
                                                                kExprBrIf,   // depth=16
                                                                kExprLoop, 00,   // @285
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprReturn,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprI64LoadMem16U,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprNop,
                                                                  kExprBr,   // depth=1
                                                                kExprElse,   // @304
                                                                  kExprF32Trunc,
                                                                  kExprI32Add,
                                                                  kExprCallIndirect,   // sig #1
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprUnreachable,
                                                                  kExprBlock, 00,   // @312
                                                                    kExprI64RemU,
                                                                    kExprI64Ctz,
                                                                    kExprI64LeU,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprDrop,
                                                                    kExprI64Popcnt,
                                                                    kExprF32Min,
                                                                    kExprUnreachable,
                                                                    kExprF64Sub,
                                                                    kExprI32Const,
                                                                    kExprUnreachable,
                                                                    kExprGetLocal,
                                                                    kExprI64LoadMem32U,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprNop,
                                                                    kExprBr,   // depth=1
                                                                  kExprElse,   // @348
                                                                    kExprF32Trunc,
                                                                    kExprI32Add,
                                                                    kExprCallIndirect,   // sig #1
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprBlock, 00,   // @356
                                                                    kExprI64RemU,
                                                                    kExprI64Ctz,
                                                                    kExprI64LeU,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprUnreachable,
                                                                    kExprDrop,
                                                                    kExprI64Popcnt,
                                                                    kExprF32Min,
                                                                    kExprUnreachable,
                                                                    kExprF64Sub,
                                                                    kExprI32Const,
                                                                    kExprUnreachable,
                                                                    kExprGetLocal,
                                                                    kExprI64LoadMem32U,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF32Trunc,
                                                                    kExprF32Trunc,
                                                                    kExprF32Trunc,
                                                                    kExprUnreachable,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
                                                                    kExprF64Min,
            ])
            .exportFunc();
assertThrows(function() { builder.instantiate(); });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19dab88^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=19dab88)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=19dab88)  
[src/wasm/ast-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/ast-decoder.cc?cl=19dab88)  
[src/wasm/ast-decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/ast-decoder.h?cl=19dab88)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=19dab88)  
...  
  

---   

## **regress-5476.js (v8 issue)**  
   
**[Issue 5476:
 internal deferred object leaks while subclassing Promise](https://crbug.com/v8/5476)**  
**[Commit: [promises] fix deferred object leak](https://chromium.googlesource.com/v8/v8/+/9d836ec)**  
  
Date(Commit): Thu Oct 06 18:29:35 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2399053003](https://codereview.chromium.org/2399053003)  
Regress: [mjsunit/regress/regress-5476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5476.js)  
```javascript
'use strict'

class LeakyPromise extends Promise {
  constructor(executor) {
    super((resolve, reject) => { resolve();});
    this.resolve = function() {assertEquals(this, undefined); };
    this.reject = function() {assertEquals(this, undefined); };
    executor(this.resolve, this.reject);
  }
}

const p1 = new LeakyPromise((r) => r());
const p2 = new LeakyPromise((_, r) => r());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9d836ec^!)  
[src/js/promise.js](https://cs.chromium.org/chromium/src/v8/src/js/promise.js?cl=9d836ec)  
[test/mjsunit/regress/regress-5476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5476.js?cl=9d836ec)  
  

---   

## **regress-651961.js (chromium issue)**  
   
**[Issue 651961:
 Disposing the isolate that is entered by a thread in wasm-code.cc](https://crbug.com/651961)**  
**[Commit: [wasm] Call a runtime function for a MemorySize instruction.](https://chromium.googlesource.com/v8/v8/+/2c12a9a)**  
  
Date(Commit): Wed Oct 05 06:06:58 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-54", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2386183004](https://codereview.chromium.org/2386183004)  
Regress: [mjsunit/regress/wasm/regress-651961.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-651961.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(1, 32, false);
  builder.addFunction("foo", kSig_i_v)
    .addBody([
              kExprMemorySize, kMemoryZero,
              kExprI32Const, 0x10,
              kExprMemoryGrow, kMemoryZero,
              kExprI32Mul,
              ])
              .exportFunc();
  var module = builder.instantiate();
  var result = module.exports.foo();
  assertEquals(1, result);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2c12a9a^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=2c12a9a)  
[src/runtime/runtime-wasm.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-wasm.cc?cl=2c12a9a)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=2c12a9a)  
[test/cctest/wasm/test-run-wasm-module.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/wasm/test-run-wasm-module.cc?cl=2c12a9a)  
[test/cctest/wasm/test-run-wasm.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/wasm/test-run-wasm.cc?cl=2c12a9a)  
...  
  

---   

## **regress-625966.js (chromium issue)**  
   
**[Issue 625966:
 InputCountField::is_valid(input_count) in instruction.h](https://crbug.com/625966)**  
**[Commit: [turbofan] Check instruction input/output count limits in instruction selector.](https://chromium.googlesource.com/v8/v8/+/a974970)**  
  
Date(Commit): Wed Oct 05 05:43:35 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2390303002](https://codereview.chromium.org/2390303002)  
Regress: [mjsunit/compiler/regress-625966.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-625966.js)  
```javascript
"use strict";
var s = "";
for (var i = 0; i < 65535; i++) {
  s += ("var a" + i + ";");
}
eval(s);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a974970^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=a974970)  
[src/compiler/instruction-selector.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.h?cl=a974970)  
[src/compiler/instruction.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction.h?cl=a974970)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=a974970)  
[test/mjsunit/compiler/regress-625966.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-625966.js?cl=a974970)  
  

---   

## **regress-02256.js (chromium issue)**  
   
**[Issue 2256:
 Right-click not working for context menus](https://crbug.com/2256)**  
**[Commit: [wasm] explicitly mark off unlinked wasm module instances](https://chromium.googlesource.com/v8/v8/+/c938f0d)**  
  
Date(Commit): Tue Oct 04 21:23:24 2016  
Components/Type: None/Bug  
Labels: ["Area-Unknown", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://codereview.chromium.org/2393443003](https://codereview.chromium.org/2393443003)  
Regress: [mjsunit/regress/wasm/regress-02256.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-02256.js), [mjsunit/regress/wasm/regress-02256b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-02256b.js)  
```javascript
function MjsUnitAssertionError(message) {}
MjsUnitAssertionError.prototype.toString = function() {
    return this.message;
};
var assertSame;
var assertEquals;
var assertEqualsDelta;
var assertArrayEquals;
var assertPropertiesEqual;
var assertToStringEquals;
var assertTrue;
var assertFalse;
var triggerAssertFalse;
var assertNull;
var assertNotNull;
var assertThrows;
var assertDoesNotThrow;
var assertInstanceof;
var assertUnreachable;
var assertOptimized;
var assertUnoptimized;

function classOf(object) {
    var string = Object.prototype.toString.call(object);
    return string.substring(8, string.length - 1);
}

function PrettyPrint(value) {
    return "";
}

function PrettyPrintArrayElement(value, index, array) {
    return "";
}

function fail(expectedText, found, name_opt) {}

function deepObjectEquals(a, b) {
    var aProps = Object.keys(a);
    aProps.sort();
    var bProps = Object.keys(b);
    bProps.sort();
    if (!deepEquals(aProps, bProps)) {
        return false;
    }
    for (var i = 0; i < aProps.length; i++) {
        if (!deepEquals(a[aProps[i]], b[aProps[i]])) {
            return false;
        }
    }
    return true;
}

function deepEquals(a, b) {
    if (a === b) {
        if (a === 0) return (1 / a) === (1 / b);
        return true;
    }
    if (typeof a != typeof b) return false;
    if (typeof a == "number") return isNaN(a) && isNaN(b);
    if (typeof a !== "object" && typeof a !== "function") return false;
    var objectClass = classOf(a);
    if (objectClass !== classOf(b)) return false;
    if (objectClass === "RegExp") {
        return (a.toString() === b.toString());
    }
    if (objectClass === "Function") return false;
    if (objectClass === "Array") {
        var elementCount = 0;
        if (a.length != b.length) {
            return false;
        }
        for (var i = 0; i < a.length; i++) {
            if (!deepEquals(a[i], b[i])) return false;
        }
        return true;
    }
    if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") {
        if (a.valueOf() !== b.valueOf()) return false;
    }
    return deepObjectEquals(a, b);
}
assertSame = function assertSame(expected, found, name_opt) {
    if (found === expected) {
        if (expected !== 0 || (1 / expected) == (1 / found)) return;
    } else if ((expected !== expected) && (found !== found)) {
        return;
    }
    fail(PrettyPrint(expected), found, name_opt);
};
assertEquals = function assertEquals(expected, found, name_opt) {
    if (!deepEquals(found, expected)) {
        fail(PrettyPrint(expected), found, name_opt);
    }
};
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) {
    assertTrue(Math.abs(expected - found) <= delta, name_opt);
};
assertArrayEquals = function assertArrayEquals(expected, found, name_opt) {
    var start = "";
    if (name_opt) {
        start = name_opt + " - ";
    }
    assertEquals(expected.length, found.length, start + "array length");
    if (expected.length == found.length) {
        for (var i = 0; i < expected.length; ++i) {
            assertEquals(expected[i], found[i], start + "array element at index " + i);
        }
    }
};
assertPropertiesEqual = function assertPropertiesEqual(expected, found, name_opt) {
    if (!deepObjectEquals(expected, found)) {
        fail(expected, found, name_opt);
    }
};
assertToStringEquals = function assertToStringEquals(expected, found, name_opt) {
    if (expected != String(found)) {
        fail(expected, found, name_opt);
    }
};
assertTrue = function assertTrue(value, name_opt) {
    assertEquals(true, value, name_opt);
};
assertFalse = function assertFalse(value, name_opt) {
    assertEquals(false, value, name_opt);
};
assertNull = function assertNull(value, name_opt) {
    if (value !== null) {
        fail("null", value, name_opt);
    }
};
assertNotNull = function assertNotNull(value, name_opt) {
    if (value === null) {
        fail("not null", value, name_opt);
    }
};
assertThrows = function assertThrows(code, type_opt, cause_opt) {
    var threwException = true;
    try {
        if (typeof code == 'function') {
            code();
        } else {
            eval(code);
        }
        threwException = false;
    } catch (e) {
        if (typeof type_opt == 'function') {
            assertInstanceof(e, type_opt);
        }
        if (arguments.length >= 3) {
            assertEquals(e.type, cause_opt);
        }
        return;
    }
};
assertInstanceof = function assertInstanceof(obj, type) {
    if (!(obj instanceof type)) {
        var actualTypeName = null;
        var actualConstructor = Object.getPrototypeOf(obj).constructor;
        if (typeof actualConstructor == "function") {
            actualTypeName = actualConstructor.name || String(actualConstructor);
        }
        fail("Object <" + PrettyPrint(obj) + "> is not an instance of <" + (type.name || type) + ">" + (actualTypeName ? " but of < " + actualTypeName + ">" : ""));
    }
};
assertDoesNotThrow = function assertDoesNotThrow(code, name_opt) {
    try {
        if (typeof code == 'function') {
            code();
        } else {
            eval(code);
        }
    } catch (e) {
        fail("threw an exception: ", e.message || e, name_opt);
    }
};
assertUnreachable = function assertUnreachable(name_opt) {
    var message = "Fail" + "ure: unreachable";
    if (name_opt) {
        message += " - " + name_opt;
    }
};
var OptimizationStatus = function() {}
assertUnoptimized = function assertUnoptimized(fun, sync_opt, name_opt) {
    if (sync_opt === undefined) sync_opt = "";
    assertTrue(OptimizationStatus(fun, sync_opt) != 1, name_opt);
}
assertOptimized = function assertOptimized(fun, sync_opt, name_opt) {
    if (sync_opt === undefined) sync_opt = "";
    assertTrue(OptimizationStatus(fun, sync_opt) != 2, name_opt);
}
triggerAssertFalse = function() {}
try {
    console.log;
    print = console.log;
    alert = console.log;
} catch (e) {}

function runNearStackLimit(f) {
    function t() {
        try {
            t();
        } catch (e) {
            f();
        }
    };
    try {
        t();
    } catch (e) {}
}

function quit() {}

function nop() {}
try {
    gc;
} catch (e) {
    gc = nop;
}

function getRandomProperty(v, rand) {
    var properties = Object.getOwnPropertyNames(v);
    var proto = Object.getPrototypeOf(v);
    if (proto) {
        properties = properties.concat(Object.getOwnPropertyNames(proto));
    }
    if (properties.includes("constructor") && v.constructor.hasOwnProperty("__proto__")) {
        properties = properties.concat(Object.getOwnPropertyNames(v.constructor.__proto__));
    }
    if (properties.length == 0) {
        return "0";
    }
    return properties[rand % properties.length];
}

var __v_0 = {};
var __v_1 = {};
var __v_2 = {};
var __v_3 = {};
var __v_4 = -1073741824;
var __v_5 = {};
var __v_6 = 1;
var __v_7 = 1073741823;
var __v_8 = {};
var __v_9 = {};
var __v_10 = 4294967295;
var __v_11 = this;
var __v_12 = {};
var __v_13 = {};
try {
    load("test/mjsunit/wasm/wasm-module-__v_1.js");
    __v_2 = 0x10000;
} catch (e) {
    print("Caught: " + e);
}

function __f_16() {
    var __v_1 = new WasmModuleBuilder();
    __v_1.addFunction("grow_memory", kSig_i_i)
        .addBody([kExprGetLocal, 0, kExprMemoryGrow])
        .exportFunc();
    __v_1.addFunction("load", kSig_i_i)
        .addBody([kExprGetLocal, 0, kExprI32LoadMem, 0, 0])
        .exportFunc();
    __v_1.addFunction("store", kSig_i_ii)
        .addBody([kExprGetLocal, 0, kExprGetLocal, 1, kExprI32StoreMem, 0, 0, kExprGetLocal, 1])
        .exportFunc();
    __v_1.addFunction("load16", kSig_i_i)
        .addBody([kExprGetLocal, 0, kExprI32LoadMem16U, 0, 0])
        .exportFunc();
    __v_1.addFunction("store16", kSig_i_ii)
        .addBody([kExprGetLocal, 0, kExprGetLocal, 1, kExprI32StoreMem16, 0, 0, kExprGetLocal, 1])
        .exportFunc();
    __v_1.__p_1551105852 = __v_1[getRandomProperty(__v_1, 1551105852)];
    __v_1.__defineGetter__(getRandomProperty(__v_1, 348910887), function() {
        gc();
        __v_9[getRandomProperty(__v_9, 1894652048)] = __v_13[getRandomProperty(__v_13, 1352929371)];
        return __v_1.__p_1551105852;
    });
    __v_1.addFunction("load8", kSig_i_i)
        .addBody([kExprGetLocal, 0, kExprI32LoadMem8U, 0, 0])
        .exportFunc();
    __v_1.addFunction("store8", kSig_i_ii)
        .addBody([kExprGetLocal, 0, kExprGetLocal, 1, kExprI32StoreMem8, 0, 0, kExprGetLocal, 1])
        .exportFunc();
    return __v_1;
}

function __f_14() {
    var __v_4 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    var __v_3;

    function __f_1() {
        return module.exports.load(__v_3);
    }

    function __f_2(value) {
        return module.exports.store(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    for (__v_3 = 0; __v_3 <= (__v_2 - 4); __v_3 += 4) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = __v_2 - 3; __v_3 < __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(1, __f_8(3));
    for (__v_3 = __v_2; __v_3 <= 4 * __v_2 - 4; __v_3 += 4) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 4 * __v_2 - 3; __v_3 < 4 * __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(4, __f_8(15));
    for (__v_3 = 4 * __v_2 - 3; __v_3 <= 4 * __v_2 + 4; __v_3 += 4) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 19 * __v_2 - 10; __v_3 <= 19 * __v_2 - 4; __v_3 += 4) {
        __f_2(20);
        gc();
        assertEquals(12, __f_1());
    }
    for (__v_3 = 19 * __v_2 - 3; __v_3 < 19 * __v_2 + 5; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
}
try {
    __f_14();
} catch (e) {
    print("Caught: " + e);
}

function __f_13() {
    var __v_1 = __f_16();
    __v_1.__defineGetter__(getRandomProperty(__v_1, 1322348896), function() {
        gc();
        return __f_28(__v_1);
    });
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    assertEquals(0, __f_30(0));
    var __v_3;

    function __f_1() {
        return module.exports.load16(__v_3);
    }

    function __f_2(value) {
        return module.exports.store16(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    for (__v_3 = 0; __v_3 <= (__v_2 - 2); __v_3 += 2) {
        __f_2(20);
        assertEquals(20, __f_1());
        __f_19();
    }
    for (__v_3 = __v_2 - 1; __v_3 < __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(65535, __f_8(0));
    for (__v_3 = __v_2; __v_3 <= 4 * __v_2 - 2; __v_3 += 2) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 4 * __v_2 - 1; __v_3 < 4 * __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(4, __f_8(15));
    for (__v_3 = 4 * __v_2 - 2; __v_3 <= 4 * __v_2 + 4; __v_3 += 2) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_1 = 19 * __v_11 - 10; __v_13 <= 19 * __v_2 - 2; __v_9 += 2) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 19 * __v_2 - 1; __v_3 < 19 * __v_2 + 5; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
}
try {
    __f_13();
} catch (e) {
    print("Caught: " + e);
}

function __f_10() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    var __v_3;

    function __f_1() {
        return module.exports.load8(__v_3);
    }

    function __f_2(value) {
        return module.exports.store8(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    for (__v_3 = 0; __v_3 <= __v_2 - 1; __v_3++) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = __v_2; __v_3 < __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(1, __f_8(3));
    for (__v_3 = __v_2; __v_3 <= 4 * __v_2 - 1; __v_3++) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 4 * __v_2; __v_3 < 4 * __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
    assertEquals(4, __f_8(15));
    for (__v_3 = 4 * __v_2; __v_3 <= 4 * __v_2 + 4; __v_3++) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 19 * __v_2 - 10; __v_3 <= 19 * __v_2 - 1; __v_3++) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = 19 * __v_2; __v_3 < 19 * __v_2 + 5; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
}
try {
    __f_10();
} catch (e) {
    print("Caught: " + e);
}

function __f_5() {
    var __v_1 = __f_16();
    var module = __v_1.instantiate();
    var __v_3;

    function __f_1() {
        return module.exports.load(__v_3);
    }

    function __f_2(value) {
        return module.exports.store(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    assertTraps(kTrapMemOutOfBounds, __f_1);
    assertTraps(kTrapMemOutOfBounds, __f_2);
    assertEquals(0, __f_8(1));
    for (__v_3 = 0; __v_3 <= __v_2 - 4; __v_3++) {
        __f_2(20);
        assertEquals(20, __f_1());
    }
    for (__v_3 = __v_2; __v_3 <= __v_2 + 5; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_1);
    }
}
try {
    __f_5();
} catch (e) {
    print("Caught: " + e);
}

function __f_9() {
    var __v_1 = __f_16();
    var module = __v_1.instantiate();
    var __v_4 = 16385;

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    assertEquals(-1, __f_8(__v_13));
}
try {
    __f_9();
} catch (e) {
    print("Caught: " + e);
}

function __f_12() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_9.instantiate();
    __v_4.__p_1905062277 = __v_4[getRandomProperty(__v_4, 1905062277)];
    __v_4.__defineGetter__(getRandomProperty(__v_4, 1764398743), function() {
        gc();
        __v_0[getRandomProperty(__v_0, 1011363961)] = __v_8[getRandomProperty(__v_8, 1946768258)];
        return __v_4.__p_1905062277;
    });
    var __v_4 = 16384;

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    assertEquals(-1, __f_8(__v_4));
}
try {
    __f_12();
} catch (e) {
    print("Caught: " + e);
}

function __f_0() {
    var __v_1 = __f_16();
    var module = __v_1.instantiate();

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    assertEquals(-1, __f_8(-1));
};
try {
    __f_0();
} catch (e) {
    print("Caught: " + e);
}

function __f_4() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    __v_1.addFunction("memory_size", kSig_i_v)
        .addBody([kExprMemorySize])
        .exportFunc();
    var module = __v_1.instantiate();

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }

    function __f_7() {
        return module.exports.memory_size();
    }
    assertEquals(1, __f_7());
    assertEquals(1, __f_8(1));
    assertEquals(2, __f_7());
}
try {
    __f_4();
    gc();
} catch (e) {
    print("Caught: " + e);
}

function __f_6() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    var __v_3, __v_0;
    gc();

    function __f_1() {
        return module.exports.load(__v_3);
    }

    function __f_2(value) {
        return module.exports.store(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    gc();
    for (__v_3 = 0; __v_3 <= (__v_2 - 4); __v_3 += 4) {
        __f_2(100000 - __v_3);
        __v_3.__defineGetter__(getRandomProperty(__v_3, 764734523), function() {
            gc();
            return __f_16(__v_3);
        });
        assertEquals(100000 - __v_3, __f_1());
    }
    assertEquals(1, __f_8(3));
    for (__v_3 = 0; __v_3 <= (__v_2 - 4); __v_3 += 4) {
        assertEquals(100000 - __v_3, __f_1());
    }
}
try {
    __f_6();
    gc();
} catch (e) {
    print("Caught: " + e);
}

function __f_11() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_2.instantiate();
    var __v_3, __v_0;

    function __f_1() {
        return module.exports.load16(__v_3);
    }

    function __f_2(value) {
        return module.exports.store16(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    for (__v_3 = 0; __v_3 <= (__v_2 - 2); __v_3 += 2) {
        __f_2(65535 - __v_3);
        assertEquals(65535 - __v_3, __f_1());
    }
    assertEquals(1, __f_8(3));
    for (__v_3 = 0; __v_3 <= (__v_2 - 2); __v_3 += 2) {
        assertEquals(65535 - __v_3, __f_1());
    }
}
try {
    __f_11();
} catch (e) {
    print("Caught: " + e);
}

function __f_15() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    var __v_3, __v_0 = 0;

    function __f_1() {
        return module.exports.load8(__v_10);
    }

    function __f_2(value) {
        return module.exports.store8(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    for (__v_3 = 0; __v_3 <= (__v_2 - 1); __v_3++, __v_0++) {
        __f_2(__v_0);
        assertEquals(__v_0, __f_1());
        if (__v_0 == 255) __v_0 = 0;
    }
    assertEquals(1, __f_8(3));
    __v_0 = 0;
    for (__v_10 = 0; __v_4 <= (__v_0 - 1); __v_11++, __v_5++) {
        assertEquals(__v_0, __f_1());
        if (__v_10 == 255) __v_5 = 0;
    }
}
try {
    __f_15();
} catch (e) {
    print("Caught: " + e);
}

function __f_3() {
    var __v_1 = __f_16();
    __v_1.addMemory(1, 1, false);
    var module = __v_1.instantiate();
    var __v_3, __v_0;

    function __f_1() {
        return module.exports.load(__v_3);
    }

    function __f_2(value) {
        return module.exports.store(__v_3, value);
    }

    function __f_8(pages) {
        return module.exports.grow_memory(pages);
    }
    gc();
    __v_3 = 3 * __v_2 + 4;
    assertTraps(kTrapMemOutOfBounds, __f_2);
    assertEquals(1, __f_8(1));
    assertTraps(kTrapMemOutOfBounds, __f_2);
    assertEquals(2, __f_8(1));
    assertTraps(kTrapMemOutOfBounds, __f_2);
    assertEquals(3, __f_8(1));
    for (__v_3 = 3 * __v_2; __v_3 <= 4 * __v_2 - 4; __v_3++) {
        __f_2(0xaced);
        assertEquals(0xaced, __f_1());
    }
    for (__v_3 = 4 * __v_2 - 3; __v_3 <= 4 * __v_2 + 4; __v_3++) {
        assertTraps(kTrapMemOutOfBounds, __f_2);
    }
}
try {
    __f_3();
} catch (e) {
    print("Caught: " + e);
}

function __f_18(__f_17, y) {
    eval(__f_17);
    return y();
}
try {
    var __v_17 = __f_18("function y() { return 1; }", function() {
        return 0;
    })
    assertEquals(1, __v_17);
    gc();
    __v_17 =
        (function(__f_17) {
            function __f_17() {
                return 3;
            }
            return __f_17();
        })(function() {
            return 2;
        });
    assertEquals(3, __v_17);
    __v_17 =
        (function(__f_17) {
            function __f_17() {
                return 5;
            }
            return arguments[0]();
        })(function() {
            return -1073741825;
        });
    assertEquals(5, __v_17);
} catch (e) {
    print("Caught: " + e);
}

function __f_27() {}
try {
    var __v_24 = {};
    var __v_21 = {};
    var __v_22 = {};
    var __v_20 = {};
    __v_58 = {
        instantiateModuleFromAsm: function(text, ffi, heap) {
            var __v_21 = eval('(' + text + ')');
            if (__f_27()) {
                throw "validate failure";
            }
            var __v_20 = __v_21();
            if (__f_27()) {
                throw "bad module args";
            }
        }
    };
    __f_21 = function __f_21() {
        if (found === expected) {
            if (1 / expected) return;
        } else if ((expected !== expected) && (found !== found)) {
            return;
        };
    };
    __f_28 = function __f_28() {
        if (!__f_23()) {
            __f_125(__f_69(), found, name_opt);
        }
    };
    __f_24 = function __f_24(code, type_opt, cause_opt) {
        var __v_24 = true;
        try {
            if (typeof code == 'function') {
                code();
            } else {
                eval();
            }
            __v_24 = false;
        } catch (e) {
            if (typeof type_opt == 'function') {
                __f_22();
            }
            if (arguments.length >= 3) {
                __f_28();
            }
            return;
        }
    };
    __f_22 = function __f_22() {
        if (obj instanceof type) {
            obj.constructor;
            if (typeof __v_57 == "function") {;
            };
        }
    };
    try {
        __f_28();
        __v_82.__p_750895751 = __v_82[getRandomProperty()];
    } catch (e) {
        "Caught: " + e;
    }
    __f_19();
    gc();
    __f_19(19, __f_24);
    __f_19();
    __f_19();
    __f_24(function() {
        __v_58.instantiateModuleFromAsm(__f_28.toString()).__f_20();
    });
} catch (e) {
    print("Caught: " + e);
}

function __f_19() {
    "use asm";

    function __f_20() {}
    return {
        __f_20: __f_20
    };
}
try {
    __f_19();
    __f_19();
    __f_19();
} catch (e) {
    print("Caught: " + e);
}

function __f_29() {}
try {
    __f_19();
    try {
        __f_19();
        gc();
        __f_25();
    } catch (e) {
        "Caught: " + e;
    }
    __f_19();
    __f_19();
    __f_19();
} catch (e) {
    print("Caught: " + e);
}

function __f_23() {
    "use asm";

    function __f_20() {}
    return {
        __f_20: __f_20
    };
}
try {
    __f_19();
    __f_19();
    __f_19();
    __f_19();
    gc();
    __f_19();
    __f_19();
    __f_19();
} catch (e) {
    print("Caught: " + e);
}

function __f_26(stdlib) {
    "use asm";
    var __v_2 = new stdlib.Int32Array();
    __v_22[4294967295] | 14 + 1 | 14;
    return {
        __f_20: __f_20
    };
}

function __f_25() {
    var __v_19 = new ArrayBuffer();
    var __v_23 = new Int32Array(__v_19);
    var module = __v_58.instantiateModuleFromAsm(__f_26.toString());
    __f_28();
    gc();
}
try {
    (function() {})();
    (function() {})();
    try {
        (function() {
            __v_23.__defineGetter__(getRandomProperty(__v_23, 580179357), function() {
                gc();
                return __f_25(__v_23);
            });
            var __v_23 = 0x87654321;
            __v_19.__f_89();
        })();
    } catch (e) {;
    }
} catch (e) {
    print("Caught: " + e);
}

function __f_30(x) {
    var __v_30 = x + 1;
    var __v_31 = x + 2;
    if (x != 0) {
        if (x > 0 & x < 100) {
            return __v_30;
        }
    }
    return 0;
}
try {
    assertEquals(0, __f_30(0));
    assertEquals(0, __f_30(0));
    %OptimizeFunctionOnNextCall(__f_30);
    assertEquals(3, __f_30(2));
} catch (e) {
    print("Caught: " + e);
}

function __f_31() {
    __f_32.arguments;
}

function __f_32(x) {
    __f_31();
}

function __f_33() {
    __f_32({});
}
try {
    __f_33();
    __f_33();
    __f_33();
    %OptimizeFunctionOnNextCall(__f_33);
    __f_33();
    gc();
} catch (e) {
    print("Caught: " + e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c938f0d^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=c938f0d)  
[test/mjsunit/regress/wasm/regression-02256.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-02256.js?cl=c938f0d)  
  

---   

## **regress-abort-context-allocate-params.js (other issue)**  
   
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
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=9feab2d)  
[test/mjsunit/regress/regress-abort-context-allocate-params.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-abort-context-allocate-params.js?cl=9feab2d)  
  
  
---   

## **regress-crbug-651403-global.js (chromium issue)**  
   
**[Issue 651403:
 Unreachable code in verifier.cc](https://crbug.com/651403)**  
**[Commit: [ignition] BytecodeGraphBuilder: Merge correct environment in try block](https://chromium.googlesource.com/v8/v8/+/537c855)**  
  
Date(Commit): Thu Sep 29 15:18:06 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2379043002](https://codereview.chromium.org/2379043002)  
Regress: [mjsunit/regress/regress-crbug-651403-global.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-651403-global.js), [mjsunit/regress/regress-crbug-651403.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-651403.js)  
```javascript
x = "";

function f () {
  function g() {
    try {
      eval('');
      return x;
    } catch(e) {
    }
  }
  return g();
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/537c855^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=537c855)  
[test/mjsunit/regress/regress-crbug-651403-global.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-651403-global.js?cl=537c855)  
[test/mjsunit/regress/regress-crbug-651403.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-651403.js?cl=537c855)  
  

---   

## **regress-651327.js (chromium issue)**  
   
**[Issue 651327:
 *isolate->global_object() == *receiver in ic.cc](https://crbug.com/651327)**  
**[Commit: Readd default function variables upon scope reset for preparse abort](https://chromium.googlesource.com/v8/v8/+/fecd09c)**  
  
Date(Commit): Thu Sep 29 13:29:15 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2380993003](https://codereview.chromium.org/2380993003)  
Regress: [mjsunit/regress/regress-651327.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-651327.js)  
```javascript
function __f_1(a) {
  __v_1 = a;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = -1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 0;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  __f_3();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = -1073741825;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = -7;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  __f_3();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 17;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 0;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  gc();
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 65535;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = -13;
  x = 1;
  x = 1;
  this.mapHeight * Math.round();
}
__f_1();
function __f_2(initialX, initialY) {
}
function __f_3() {
}
gc();
__f_1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fecd09c^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=fecd09c)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=fecd09c)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=fecd09c)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=fecd09c)  
[test/mjsunit/regress/regress-651327.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-651327.js?cl=fecd09c)  
  

---   

## **regress-02862.js (chromium issue)**  
   
**[Issue 2862:
 Regression: Navigation Back and Forth problems from History page](https://crbug.com/2862)**  
**[Commit: [wasm] fix for GC during instantiation.](https://chromium.googlesource.com/v8/v8/+/aff5ab1)**  
  
Date(Commit): Thu Sep 29 04:24:42 2016  
Components/Type: None/Bug  
Labels: ["Mstone-2.0", "v-153.1", "stable", "Area-BrowserUI", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://codereview.chromium.org/2371403003](https://codereview.chromium.org/2371403003)  
Regress: [mjsunit/regress/wasm/regress-02862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-02862.js)  
```javascript
function nop() {}
var __v_42 = {};
var __v_49 = {};
var __v_70 = {};
var __v_79 = {};
__v_58 = {
  instantiateModuleFromAsm: function(text, ffi, heap) {
    var __v_49 = eval('(' + text + ')');
    if (nop()) {
      throw "validate failure";
    }
    var __v_79 = __v_49();
    if (nop()) {
      throw "bad module args";
    }
  }};
__f_140 = function __f_140() {
  if (found === expected) {
    if (1 / expected) return;
  } else if ((expected !== expected) && (found !== found)) { return; };
};
__f_128 = function __f_128() { if (!__f_105()) { __f_125(__f_69(), found, name_opt); } };
__f_136 = function __f_136(code, type_opt, cause_opt) {
  var __v_42 = true;
  try {
    if (typeof code == 'function') { code(); }
    else { eval(); }
    __v_42 = false;
  } catch (e) {
    if (typeof type_opt == 'function') { __f_101(); }
    if (arguments.length >= 3) { __f_128(); }
    return;
  }
};
__f_101 = function __f_101() { if (obj instanceof type) {obj.constructor; if (typeof __v_57 == "function") {; }; } };
try {
__f_128();
__v_82.__p_750895751 = __v_82[getRandomProperty()];
} catch(e) {"Caught: " + e; }
__f_119();
gc();
__f_119(19, __f_136);
__f_119();
__f_119();
__f_136(function() {
  __v_58.instantiateModuleFromAsm(__f_128.toString()).__f_108();
});
function __f_119() {
  "use asm";
  function __f_108() {
  }
  return {__f_108: __f_108};
}
__f_119();
__f_119();
__f_119();
function __f_95() {
}
__f_119();
try {
__f_119();
__f_135();
} catch(e) {"Caught: " + e; }
__f_119();
__f_119();
__f_119();
function __f_105() {
  "use asm";
  function __f_108() {
  }
  return {__f_108: __f_108};
}
__f_119();
__f_119();
__f_119();
__f_119();
__f_119();
__f_119();
__f_119();
function __f_93(stdlib) {
  "use asm";
  var __v_70 = new stdlib.Int32Array();
__v_70[4294967295]|14 + 1 | 14;
  return {__f_108: __f_108};
}
function __f_135() {
  var __v_66 = new ArrayBuffer();
  var __v_54 = new Int32Array(__v_66);
  var module = __v_58.instantiateModuleFromAsm( __f_93.toString());
  __f_128();
}
(function () {
})();
(function () {
})();
try {
(function() {
      var __v_54 = 0x87654321;
 __v_66.__f_89();
})();
} catch(e) {; }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aff5ab1^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=aff5ab1)  
[test/mjsunit/regress/wasm/regression-02862.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-02862.js?cl=aff5ab1)  
  

---   

## **regress-647649.js (chromium issue)**  
   
**[Issue 647649:
 old_size == 0 || wasm_memory_size_reference() <= old_size in assembler.cc](https://crbug.com/647649)**  
**[Commit: [wasm] Fix for cloning module heap size value](https://chromium.googlesource.com/v8/v8/+/df490c3)**  
  
Date(Commit): Thu Sep 29 00:48:28 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2347333002](https://codereview.chromium.org/2347333002)  
Regress: [mjsunit/regress/wasm/regress-647649.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-647649.js)  
```javascript
function getRandomProperty(v, rand) {
  var properties = Object.getOwnPropertyNames(v);
  var proto = Object.getPrototypeOf(v);
  if (proto) {; }
  if ("constructor" && v.constructor.hasOwnProperty()) {; }
  if (properties.length == 0) { return "0"; }
  return properties[rand % properties.length];
}

var __v_11 = {};

function __f_1(stdlib, foreign, buffer) {
  "use asm";
  var __v_3 = new stdlib.Float64Array(buffer);
  function __f_0() {
    var __v_1 = 6.0;
    __v_3[2] = __v_1 + 1.0;
  }
  return {__f_0: __f_0};
}
try {
  var __v_0 = new ArrayBuffer(207222809);
  var module = __f_1(this, null, __v_0);
( {
})();
} catch(e) {; }
__v_13 = '@3'
Array.prototype.__proto__ = {3: __v_13};
Array.prototype.__proto__.__proto__ = {7: __v_11};
__v_9 = [0, 1, , , 4, 5, , , , 9]
__v_12 = __v_9.splice(4, 1)
__v_9.__defineGetter__(getRandomProperty(__v_9, 1689439720), function() { return {}; });
 __v_9[8]
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df490c3^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=df490c3)  
[test/mjsunit/regress/wasm/regression-647649.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-647649.js?cl=df490c3)  
  

---   

## **regress-crbug-650973.js (chromium issue)**  
   
**[Issue 650973:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/650973)**  
**[Commit: [ic][mips][mips64] Ensure store handlers return value in proper register.](https://chromium.googlesource.com/v8/v8/+/8d8c134)**  
  
Date(Commit): Wed Sep 28 11:46:44 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2374003002](https://codereview.chromium.org/2374003002)  
Regress: [mjsunit/regress/regress-crbug-650973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650973.js)  
```javascript
var v = {p:0};
v.__defineGetter__("p", function() { return 13; });

function f() {
  var boom = (v.foo = v);
  assertEquals(v, boom.foo);
}

f();
f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d8c134^!)  
[src/ic/mips/handler-compiler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/handler-compiler-mips.cc?cl=8d8c134)  
[src/ic/mips/ic-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/ic-mips.cc?cl=8d8c134)  
[src/ic/mips64/handler-compiler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/handler-compiler-mips64.cc?cl=8d8c134)  
[src/ic/mips64/ic-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/ic-mips64.cc?cl=8d8c134)  
[test/mjsunit/regress/regress-crbug-650973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650973.js?cl=8d8c134)  
  

---   

## **regress-5440.js (v8 issue)**  
   
**[Issue 5440:
 One more string-related TF crash](https://crbug.com/v8/5440)**  
**[Commit: Allow empty first parts of ConsStrings](https://chromium.googlesource.com/v8/v8/+/da27e0c)**  
  
Date(Commit): Wed Sep 28 09:46:56 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2377983002](https://codereview.chromium.org/2377983002)  
Regress: [mjsunit/regress/regress-5440.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5440.js)  
```javascript
eval(" " + ("" + "try {;} catch (_) {}"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da27e0c^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=da27e0c)  
[test/mjsunit/regress/regress-5440.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5440.js?cl=da27e0c)  
  

---   

## **regress-crbug-650933.js (chromium issue)**  
   
**[Issue 650933:
 length_obj->IsSmi() in runtime-typedarray.cc](https://crbug.com/650933)**  
**[Commit: [typedarray] Properly initialize JSTypedArray::length with Smi.](https://chromium.googlesource.com/v8/v8/+/15a449b)**  
  
Date(Commit): Wed Sep 28 05:49:37 2016  
Components/Type: Blink>JavaScript>Runtime/Bug-Regression  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "M-55", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2371963002](https://codereview.chromium.org/2371963002)  
Regress: [mjsunit/regress/regress-crbug-650933.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650933.js)  
```javascript
var a = [0, 1, 2, 3, 4, 5, 6, 7, 8];
var o = {length: 1e40};
try { new Uint8Array(o); } catch (e) { }
new Float64Array(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15a449b^!)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=15a449b)  
[test/mjsunit/regress/regress-crbug-650933.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650933.js?cl=15a449b)  
  

---   

## **regress-escape-analysis-indirect.js (other issue)**  
   
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
%PrepareFunctionForOptimization(f);
function apply(fun) { fun() }
assertEquals(42, f(apply));
assertEquals(42, f(apply));
%NeverOptimizeFunction(apply);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f(apply));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/437a33e^!)  
[src/compiler/escape-analysis-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis-reducer.cc?cl=437a33e)  
[test/mjsunit/compiler/regress-escape-analysis-indirect.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-escape-analysis-indirect.js?cl=437a33e)  
  
  
---   

## **regress-5351.js (v8 issue)**  
   
**[Issue 5351:
 [regexp] Incorrect RegExp.prototype.split subclass implementation](https://crbug.com/v8/5351)**  
**[Commit: [regexp] Don't cache exec method in Regexp.proto[@@split]](https://chromium.googlesource.com/v8/v8/+/515994b)**  
  
Date(Commit): Tue Sep 27 14:02:33 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2370733003](https://codereview.chromium.org/2370733003)  
Regress: [mjsunit/regress/regress-5351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5351.js)  
```javascript
var re = /[bc]/;
var str = "baba";

assertEquals(["", "a", "a"], str.split(re));

re.exec = (string) => RegExp.prototype.exec.call(re, string);
assertEquals(["", "a", "a"], str.split(re));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/515994b^!)  
[src/js/regexp.js](https://cs.chromium.org/chromium/src/v8/src/js/regexp.js?cl=515994b)  
[test/mjsunit/regress/regress-5351.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5351.js?cl=515994b)  
  

---   

## **regress-abort-preparsing-params.js (other issue)**  
   
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
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=c0ded71)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=c0ded71)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=c0ded71)  
[test/mjsunit/regress/regress-abort-preparsing-params.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-abort-preparsing-params.js?cl=c0ded71)  
  
  
---   

## **regress-650172.js (chromium issue)**  
   
**[Issue 650172:
 Stack-overflow in v8::internal::Invoke](https://crbug.com/650172)**  
**[Commit: [builtins] adapt arguments for Builtins::kIteratorPrototypeIterator](https://chromium.googlesource.com/v8/v8/+/8fea775)**  
  
Date(Commit): Tue Sep 27 11:05:42 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2368323002](https://codereview.chromium.org/2368323002)  
Regress: [mjsunit/es6/regress/regress-650172.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-650172.js)  
```javascript
var iterator = [].entries().__proto__.__proto__[Symbol.iterator];
print(1/iterator(-1E-300));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8fea775^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=8fea775)  
[test/mjsunit/es6/regress/regress-650172.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-650172.js?cl=8fea775)  
  

---   

## **regress-crbug-650404.js (chromium issue)**  
   
**[Issue 650404:
 Security: OOB read/write in V8 using TypedArrays+Crankshaft+Turbofan](https://crbug.com/650404)**  
**[Commit: [crankshaft] TypedArrayInitialize: force length to be a Smi](https://chromium.googlesource.com/v8/v8/+/142f9df)**  
  
Date(Commit): Mon Sep 26 23:00:45 2016  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Security_Impact-Head", "Security_Severity-High", "allpublic"]  
Code Review: [https://codereview.chromium.org/2371963002](https://codereview.chromium.org/2371963002)  
Regress: [mjsunit/regress/regress-crbug-650404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650404.js)  
```javascript
function c4(w, h) {
  var size = w * h;
  if (size < 0) size = 0;
  return new Uint32Array(size);
}

for (var i = 0; i < 3; i++) {
  // Computing -0 as the result makes the "size = w * h" multiplication IC
  // go into double mode.
  c4(0, -1);
}
for (var i = 0; i < 1000; i++) c4(2, 2);

var bomb = c4(2, 2);

function reader(o, i) {
  // Dummy try-catch, so that TurboFan is used to optimize this.
  try {} catch(e) {}
  return o[i];
}
for (var i = 0; i < 3; i++) reader(bomb, 0);
%OptimizeFunctionOnNextCall(reader);
reader(bomb, 0);

for (var i = bomb.length; i < 100; i++) {
  assertEquals(undefined, reader(bomb, i));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/142f9df^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=142f9df)  
[test/mjsunit/regress/regress-crbug-650404.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650404.js?cl=142f9df)  
  

---   

## **regress-650215.js (chromium issue)**  
   
**[Issue 650215:
 restriction_type->Is(info->restriction_type()) in simplified-lowering.cc](https://crbug.com/650215)**  
**[Commit: [turbofan] Fix restriction type for modulus in representation inference.](https://chromium.googlesource.com/v8/v8/+/3218ef3)**  
  
Date(Commit): Mon Sep 26 11:45:07 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2373453002](https://codereview.chromium.org/2373453002)  
Regress: [mjsunit/compiler/regress-650215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-650215.js)  
```javascript
function f() {
  var x = 0;
  for (var i = 0; i < 10; i++) {
    x = (2 % x) | 0;
    if (i === 5) %OptimizeOsr();
  }
  return x;
}

assertEquals(0, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3218ef3^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=3218ef3)  
[test/mjsunit/compiler/regress-650215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-650215.js?cl=3218ef3)  
  

---   

## **regress-arguments-liveness-analysis.js (other issue)**  
   
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
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=7f025eb)  
[src/ast/variables.h](https://cs.chromium.org/chromium/src/v8/src/ast/variables.h?cl=7f025eb)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=7f025eb)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=7f025eb)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=7f025eb)  
...  
  
  
---   

## **regress-crbug-640497.js (chromium issue)**  
   
**[Issue 640497:
 HeapConstant of kRepTagged (Constant(1CNUMBER <FixedArray[0]>)) cannot be change](https://crbug.com/640497)**  
**[Commit: [turbofan] Add proper type guards to escape analysis.](https://chromium.googlesource.com/v8/v8/+/4b2c6d0)**  
  
Date(Commit): Fri Sep 23 11:02:13 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "M-54", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2363573003](https://codereview.chromium.org/2363573003)  
Regress: [mjsunit/regress/regress-crbug-640497.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-640497.js)  
```javascript
function g(v) { return v.length; }
assertEquals(1, g("x"));
assertEquals(2, g("xy"));
assertEquals(1, g([1]));
assertEquals(2, g([1,2]));

function f() { assertEquals(0, g([])); }
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b2c6d0^!)  
[src/compiler/escape-analysis-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis-reducer.cc?cl=4b2c6d0)  
[test/mjsunit/regress/regress-crbug-640497.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-640497.js?cl=4b2c6d0)  
[test/unittests/compiler/escape-analysis-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/escape-analysis-unittest.cc?cl=4b2c6d0)  
  

---   

## **regress-crbug-648740.js (chromium issue)**  
   
**[Issue 648740:
 arguments() != nullptr == migrate_to->arguments() != nullptr in scopes.cc](https://crbug.com/648740)**  
**[Commit: Add regression test for crbug.com/648740.](https://chromium.googlesource.com/v8/v8/+/68ee0a4)**  
  
Date(Commit): Thu Sep 22 20:44:05 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2362563002](https://codereview.chromium.org/2362563002)  
Regress: [mjsunit/regress/regress-crbug-648740.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648740.js)  
```javascript
(function () {
  function foo() {
    const arguments = 42;
  }
})()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68ee0a4^!)  
[test/mjsunit/regress/regress-crbug-648740.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-648740.js?cl=68ee0a4)  
  

---   

## **regress-649067.js (chromium issue)**  
   
**[Issue 649067:
 variable->IsContextSlot() || variable->IsStackAllocated() in bytecode-generator.](https://crbug.com/649067)**  
**[Commit: Declare the arguments object before creating the function var, to make sure it masks it](https://chromium.googlesource.com/v8/v8/+/df7ecd1)**  
  
Date(Commit): Thu Sep 22 19:16:42 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2362463003](https://codereview.chromium.org/2362463003)  
Regress: [mjsunit/regress/regress-649067.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-649067.js)  
```javascript
assertEquals(1, (function arguments() { return eval("arguments"); })(1)[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df7ecd1^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=df7ecd1)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=df7ecd1)  
[test/mjsunit/regress/regress-649067.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-649067.js?cl=df7ecd1)  
  

---   

## **regress-649078.js (chromium issue)**  
   
**[Issue 649078:
 args[1]->IsJSFunction() in runtime-internal.cc](https://crbug.com/649078)**  
**[Commit: [promises] PromiseResolveThenableJob: change then to be a JSReceiver](https://chromium.googlesource.com/v8/v8/+/ba41697)**  
  
Date(Commit): Wed Sep 21 23:56:20 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2362503003](https://codereview.chromium.org/2362503003)  
Regress: [mjsunit/regress/regress-649078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-649078.js)  
```javascript
let p = Promise.resolve();
Object.defineProperty(p, 'then', {
  get: () => new Proxy(function() {}, p)
});

new Promise((r) => r(p));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ba41697^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=ba41697)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=ba41697)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=ba41697)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=ba41697)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=ba41697)  
...  
  

---   

## **regress-crbug-648737.js (chromium issue)**  
   
**[Issue 648737:
 JSFunction::kCodeEntryOffset == FieldAccessOf(node->op()).offset in escape-analy](https://crbug.com/648737)**  
**[Commit: [turbofan] Support for ConsString by escape analysis.](https://chromium.googlesource.com/v8/v8/+/b097c6c)**  
  
Date(Commit): Wed Sep 21 12:30:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2357153002](https://codereview.chromium.org/2357153002)  
Regress: [mjsunit/regress/regress-crbug-648737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648737.js)  
```javascript
function f(str) {
  var s = "We turn {" + str + "} into a ConsString now";
  return s.length;
}
assertEquals(33, f("a"));
assertEquals(33, f("b"));
%OptimizeFunctionOnNextCall(f);
assertEquals(33, f("c"));

function g(str) {
  var s = "We also try to materalize {" + str + "} when deopting";
  %DeoptimizeNow();
  return s.length;
}
assertEquals(43, g("a"));
assertEquals(43, g("b"));
%OptimizeFunctionOnNextCall(g);
assertEquals(43, g("c"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b097c6c^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=b097c6c)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=b097c6c)  
[test/mjsunit/regress/regress-crbug-648737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-648737.js?cl=b097c6c)  
  

---   

## **regress-648373-sloppy-arguments-includesValues.js (chromium issue)**  
   
**[Issue 648373:
 Crash in v8::internal::SloppyArgumentsElementsAccessor<v8::internal::SlowSloppyArgumentsE](https://crbug.com/648373)**  
**[Commit: [elements] Handlify raw parameter_map pointers for SloppyArgumentsAccessor](https://chromium.googlesource.com/v8/v8/+/2fd6d60)**  
  
Date(Commit): Wed Sep 21 10:22:53 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "M-55", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2354773006](https://codereview.chromium.org/2354773006)  
Regress: [mjsunit/regress/regress-648373-sloppy-arguments-includesValues.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-648373-sloppy-arguments-includesValues.js)  
```javascript
function getRandomProperty(v, rand) { var properties = Object.getOwnPropertyNames(v); var proto = Object.getPrototypeOf(v); if (proto) {; } if ("constructor" && v.constructor.hasOwnProperty()) {; } if (properties.length == 0) { return "0"; } return properties[rand % properties.length]; }
var __v_4 = {};

__v_2 = {
    PACKED_ELEMENTS() {
          return {
            get 0() {
            }          };
    }  ,
  Arguments: {
    FAST_SLOPPY_ARGUMENTS_ELEMENTS() {
      var __v_11 = (function( b) { return arguments; })("foo", NaN, "bar");
      __v_11.__p_2006760047 = __v_11[getRandomProperty( 2006760047)];
      __v_11.__defineGetter__(getRandomProperty( 1698457573), function() {  gc(); __v_4[ 1486458228] = __v_2[ 1286067691]; return __v_11.__p_2006760047; });
;
Array.prototype.includes.call(__v_11);
    },
    Detached_Float64Array() {
    }  }
};
function __f_3(suites) {
  Object.keys(suites).forEach(suite => __f_4(suites[suite]));
  function __f_4(suite) {
    Object.keys(suite).forEach(test => suite[test]());
  }
}
__f_3(__v_2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2fd6d60^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=2fd6d60)  
[test/mjsunit/regress/regress-648373-sloppy-arguments-includesValues.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-648373-sloppy-arguments-includesValues.js?cl=2fd6d60)  
  

---   

## **regress-crbug-648539.js (chromium issue)**  
   
**[Issue 648539:
 Crash in v8::internal::Factory::NewTypeError](https://crbug.com/648539)**  
**[Commit: [turbofan] Remove bogus constant materialization from frame.](https://chromium.googlesource.com/v8/v8/+/81f4342)**  
  
Date(Commit): Wed Sep 21 09:31:32 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-54", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.4", "Merge-Merged-54"]  
Code Review: [https://codereview.chromium.org/2357583003](https://codereview.chromium.org/2357583003)  
Regress: [mjsunit/regress/regress-crbug-648539.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648539.js)  
```javascript
function f() {
  "use strict";
  return undefined(0, 0);
}
function g() {
  return f();
}
assertThrows(g, TypeError);
assertThrows(g, TypeError);
%OptimizeFunctionOnNextCall(g);
assertThrows(g, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/81f4342^!)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=81f4342)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=81f4342)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=81f4342)  
[src/compiler/code-generator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.h?cl=81f4342)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=81f4342)  
...  
  

---   

## **regress-5405.js (v8 issue)**  
   
**[Issue 5405:
 Unexpected meta-programming facilities: .promise and .new.target catchable by with-scope](https://crbug.com/v8/5405)**  
**[Commit: Remove synthetic unresolved variables from async/await desugaring](https://chromium.googlesource.com/v8/v8/+/bd07819)**  
  
Date(Commit): Tue Sep 20 21:31:32 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2359513002](https://codereview.chromium.org/2359513002)  
Regress: [mjsunit/regress/regress-5405.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5405.js)  
```javascript
let log = [];

(async function() {
  with ({get ['.promise']() { log.push('async') }}) {
    return 10;
  }
})();
%PerformMicrotaskCheckpoint();

(function() {
  with ({get ['.new.target']() { log.push('new.target') }}) {
    return new.target;
  }
})();

(function() {
  with ({get ['this']() { log.push('this') }}) {
    return this;
  }
})();

assertArrayEquals([], log);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd07819^!)  
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=bd07819)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=bd07819)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=bd07819)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=bd07819)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=bd07819)  
...  
  

---   

## **regress-crbug-647887.js (chromium issue)**  
   
**[Issue 647887:
 Iterating object properties in a function that has a try...catch produces incorrect result](https://crbug.com/647887)**  
**[Commit: [turbofan] Fix loop assignment analysis on ForInStatements.](https://chromium.googlesource.com/v8/v8/+/4dab7b5)**  
  
Date(Commit): Tue Sep 20 12:37:33 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Review", "Via-Wizard", "M-53", "M-54", "Hotlist-Merge-Approved", "Arch-All", "ReleaseBlock-Stable", "merge-merged-5.3", "merge-merged-5.4", "TE-Verified-M55", "Merge-Merged-54", "TE-Verified-55.0.2868.0", "NodeJS-Backport-Rejected"]  
Code Review: [https://codereview.chromium.org/2356733002](https://codereview.chromium.org/2356733002)  
Regress: [mjsunit/regress/regress-crbug-647887.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-647887.js)  
```javascript
function f(obj) {
  var key;
  for (key in obj) { }
  return key === undefined;
}

%OptimizeFunctionOnNextCall(f);
assertFalse(f({ foo:0 }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4dab7b5^!)  
[src/compiler/ast-loop-assignment-analyzer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-loop-assignment-analyzer.cc?cl=4dab7b5)  
[test/mjsunit/regress/regress-crbug-647887.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-647887.js?cl=4dab7b5)  
  

---   

## **regress-5404.js (v8 issue)**  
   
**[Issue 5404:
 Crankshaft deoptimizes unconditionally on string add range overflow](https://crbug.com/v8/5404)**  
**[Commit: [crankshaft] Protect against deopt loops from string length overflows.](https://chromium.googlesource.com/v8/v8/+/cb19257)**  
  
Date(Commit): Mon Sep 19 21:01:30 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2348293002](https://codereview.chromium.org/2348293002)  
Regress: [mjsunit/regress/regress-5404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5404.js)  
```javascript
function foo(a, b) {
  return a + "0123456789012";
}

foo("a");
foo("a");
%OptimizeFunctionOnNextCall(foo);
foo("a");

var a = "a".repeat(%StringMaxLength());
assertThrows(function() { foo(a); }, RangeError);

%OptimizeFunctionOnNextCall(foo);
assertThrows(function() { foo(a); }, RangeError);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb19257^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=cb19257)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=cb19257)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=cb19257)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=cb19257)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=cb19257)  
...  
  

---   

## **regress-crbug-647217.js (chromium issue)**  
   
**[Issue 647217:
 !info_->isolate()->has_pending_exception() in js-inlining.cc](https://crbug.com/647217)**  
**[Commit: [turbofan] Handle stack overflow during inlining.](https://chromium.googlesource.com/v8/v8/+/c2cf8b1)**  
  
Date(Commit): Thu Sep 15 14:05:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2339383002](https://codereview.chromium.org/2339383002)  
Regress: [mjsunit/regress/regress-crbug-647217.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-647217.js)  
```javascript
var source = "return 1" + new Array(2048).join(' + a') + "";
eval("function g(a) {" + source + "}");

function f(a) { return g(a) }
%OptimizeFunctionOnNextCall(f);
try { f(0) } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c2cf8b1^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c2cf8b1)  
[test/mjsunit/regress/regress-crbug-647217.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-647217.js?cl=c2cf8b1)  
  

---   

## **regress-crbug-645888.js (chromium issue)**  
   
**[Issue 645888:
 IrOpcode::kLoop == GetControlDependency()->opcode() in bytecode-graph-builder.cc](https://crbug.com/645888)**  
**[Commit: [interpreter] Add regression test for bogus OSR entry.](https://chromium.googlesource.com/v8/v8/+/8528974)**  
  
Date(Commit): Tue Sep 13 13:23:21 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2337033002](https://codereview.chromium.org/2337033002)  
Regress: [mjsunit/regress/regress-crbug-645888.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645888.js)  
```javascript
function f() {
  for (var i = 0; i < 3; ++i) {
    if (i == 1) {
      %OptimizeOsr();
      break;  // Trigger next loop.
    }
  }
  while (true) {
    throw "no loop, thank you";
  }
}
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8528974^!)  
[test/mjsunit/regress/regress-crbug-645888.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645888.js?cl=8528974)  
  

---   

## **regress-645680.js (chromium issue)**  
   
**[Issue 645680:
 Crash in v8::internal::SloppyArgumentsElementsAccessor<v8::internal::SlowSloppyArgumentsE](https://crbug.com/645680)**  
**[Commit: [elements] Handlify SloppyArguments IndexOfValueImpl](https://chromium.googlesource.com/v8/v8/+/621f4af)**  
  
Date(Commit): Mon Sep 12 17:32:09 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2332503002](https://codereview.chromium.org/2332503002)  
Regress: [mjsunit/regress/regress-645680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-645680.js)  
```javascript
function getRandomProperty(v, rand) {
  var properties = Object.getOwnPropertyNames(v);
  if ("constructor" && v.constructor.hasOwnProperty()) {; }
  if (properties.length == 0) { return "0"; }
  return properties[rand % properties.length];
}

var args = (function( b) { return arguments; })("foo", NaN, "bar");
args.__p_293850326 = "foo";
%HeapObjectVerify(args);
args.__defineGetter__(getRandomProperty( 990787501), function() {
  gc();
  return args.__p_293850326;
});
%HeapObjectVerify(args);
Array.prototype.indexOf.call(args)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/621f4af^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=621f4af)  
[test/mjsunit/array-indexing-receiver.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-indexing-receiver.js?cl=621f4af)  
[test/mjsunit/regress/regress-645680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-645680.js?cl=621f4af)  
  

---   

## **regress-645851.js (chromium issue)**  
   
**[Issue 645851:
 previous->Is(current) in typer.cc](https://crbug.com/645851)**  
**[Commit: [turbofan] Another fix for induction variable typing monotonicity.](https://chromium.googlesource.com/v8/v8/+/e031451)**  
  
Date(Commit): Mon Sep 12 17:05:11 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2332633002](https://codereview.chromium.org/2332633002)  
Regress: [mjsunit/compiler/regress-645851.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-645851.js)  
```javascript
function f() {
  var sum = 0;
  while (1) {
    for (var j = 0; j < 200; j -=  j) {
      sum = sum + 1;
      %OptimizeOsr();
      if (sum == 2) return;
    }
  }
  return sum;
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e031451^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=e031451)  
[test/mjsunit/compiler/regress-645851.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-645851.js?cl=e031451)  
  

---   

## **regress-crbug-644631.js (chromium issue)**  
   
**[Issue 644631:
 Inline calls to runtime functions in TurboFan that throw exceptions result in missing frame state](https://crbug.com/644631)**  
**[Commit: [turbofan] Switch from a whitelist to a blacklist for NeedsFrameStateInput](https://chromium.googlesource.com/v8/v8/+/58325e6)**  
  
Date(Commit): Mon Sep 12 16:12:57 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2331543002](https://codereview.chromium.org/2331543002)  
Regress: [mjsunit/regress/regress-crbug-644631.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644631.js)  
```javascript
function f() {
  var obj = Object.freeze({});
  %_CreateDataProperty(obj, "foo", "bar");
}

assertThrows(f, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58325e6^!)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=58325e6)  
[test/mjsunit/regress/regress-crbug-644631.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644631.js?cl=58325e6)  
  

---   

## **regress-crbug-645103.js (chromium issue)**  
   
**[Issue 645103:
 args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/645103)**  
**[Commit: [interpreter] Fix destroyed new.target register use.](https://chromium.googlesource.com/v8/v8/+/0681deb)**  
  
Date(Commit): Fri Sep 09 12:20:20 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2325133002](https://codereview.chromium.org/2325133002)  
Regress: [mjsunit/regress/regress-crbug-645103.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645103.js)  
```javascript
class Base {}
class Subclass extends Base {
  constructor() {
    %DeoptimizeNow();
    super();
  }
}
new Subclass();
new Subclass();
%OptimizeFunctionOnNextCall(Subclass);
new Subclass();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0681deb^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=0681deb)  
[test/cctest/interpreter/bytecode_expectations/ClassAndSuperClass.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ClassAndSuperClass.golden?cl=0681deb)  
[test/cctest/interpreter/bytecode_expectations/NewTarget.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/NewTarget.golden?cl=0681deb)  
[test/mjsunit/regress/regress-crbug-645103.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645103.js?cl=0681deb)  
  

---   

## **regress-crbug-644215.js (chromium issue)**  
   
**[Issue 644215:
 Security: ILL_ILLOPN/Segfault; crashes Chrome Dev/Beta/Stable](https://crbug.com/644215)**  
**[Commit: Properly handle holes following spreads in array literals](https://chromium.googlesource.com/v8/v8/+/e427300)**  
  
Date(Commit): Thu Sep 08 18:50:41 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Security_Impact-Stable", "M-54", "allpublic", "merge-merged-5.4", "Merge-Merged-54", "Release-0-M54"]  
Code Review: [https://codereview.chromium.org/2321533003](https://codereview.chromium.org/2321533003)  
Regress: [mjsunit/regress/regress-crbug-644215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644215.js)  
```javascript
var arr = [...[],,];
assertTrue(%HasHoleyElements(arr));
assertEquals(1, arr.length);
assertFalse(arr.hasOwnProperty(0));
assertEquals(undefined, arr[0]);
assertThrows(() => arr[0][0], TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e427300^!)  
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=e427300)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=e427300)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=e427300)  
[test/mjsunit/regress/regress-crbug-644215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644215.js?cl=e427300)  
  

---   

## **regress-644682.js (chromium issue)**  
   
**[Issue 644682:
 Crash in v8::internal::compiler::WasmGraphBuilder::AppendToPhi](https://crbug.com/644682)**  
**[Commit: [wasm] Do not produce code for br_if if its condition does not validate.](https://chromium.googlesource.com/v8/v8/+/853892a)**  
  
Date(Commit): Thu Sep 08 14:41:04 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Stability-AFL"]  
Code Review: [https://codereview.chromium.org/2319213003](https://codereview.chromium.org/2319213003)  
Regress: [mjsunit/regress/wasm/regress-644682.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-644682.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
var builder = new WasmModuleBuilder();
builder.addFunction("regression_644682", kSig_i_v)
  .addBody([
            kExprBlock,   // @1
            kExprI32Const, 0x3b,
            kExprI32LoadMem, 0x00, 0x00,
            kExprI32Const, 0x10,
            kExprBrIf, 0x01, 0x00,   // arity=1 depth0
            kExprI32Const, 0x45,
            kExprI32Const, 0x3b,
            kExprI64LoadMem16S, 0x00, 0x3b,
            kExprBrIf, 0x01, 0x00   // arity=1 depth0
            ])
            .exportFunc();
assertThrows(function() { builder.instantiate(); });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/853892a^!)  
[src/wasm/ast-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/ast-decoder.cc?cl=853892a)  
[test/mjsunit/wasm/regression-644682.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/regression-644682.js?cl=853892a)  
  

---   

## **regress-crbug-644245.js (chromium issue)**  
   
**[Issue 644245:
 isolate->context() == nullptr || isolate->context()->IsContext() in runtime-comp](https://crbug.com/644245)**  
**[Commit: [deoptimizer] Support materialization of ContextExtension.](https://chromium.googlesource.com/v8/v8/+/9984d6f)**  
  
Date(Commit): Thu Sep 08 10:33:20 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2317353003](https://codereview.chromium.org/2317353003)  
Regress: [mjsunit/regress/regress-crbug-644245.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644245.js)  
```javascript
function f() {
  try {
    throw "boom";
  } catch(e) {
    %_DeoptimizeNow();
  }
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9984d6f^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9984d6f)  
[test/mjsunit/regress/regress-crbug-644245.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644245.js?cl=9984d6f)  
  

---   

## **regress-crbug-644689-1.js (chromium issue)**  
   
**[Issue 644689:
 map->is_stable() in compilation-dependencies.cc](https://crbug.com/644689)**  
**[Commit: [turbofan] Ensure that all prototypes are stable for push/pop.](https://chromium.googlesource.com/v8/v8/+/4ed27fc)**  
  
Date(Commit): Thu Sep 08 08:48:32 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.4", "Postmortem-Followup"]  
Code Review: [https://codereview.chromium.org/2319533005](https://codereview.chromium.org/2319533005)  
Regress: [mjsunit/regress/regress-crbug-644689-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644689-1.js), [mjsunit/regress/regress-crbug-644689-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644689-2.js)  
```javascript
for (var i = 0; i < 1024; ++i) Object.prototype["i" + i] = i;

function foo() { [].push(1); }

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ed27fc^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=4ed27fc)  
[test/mjsunit/regress/regress-crbug-644689-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644689-1.js?cl=4ed27fc)  
[test/mjsunit/regress/regress-crbug-644689-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644689-2.js?cl=4ed27fc)  
  

---   

## **regress-5357.js (v8 issue)**  
   
**[Issue 5357:
 Take into account input restrictions for feedback typing](https://crbug.com/v8/5357)**  
**[Commit: [turbofan] Revert "Avoid overflow checks on SpeculativeNumberAdd/Subtract/Multiply."](https://chromium.googlesource.com/v8/v8/+/91ed540)**  
  
Date(Commit): Thu Sep 08 04:20:31 2016  
Type: FeatureRequest  
Code Review: [https://codereview.chromium.org/2320013002](https://codereview.chromium.org/2320013002)  
Regress: [mjsunit/regress/regress-5357.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5357.js)  
```javascript
function foo(a) {
  a++;
  a = Math.max(0, a);
  a++;
  return a;
}

foo(0);
foo(0);
%OptimizeFunctionOnNextCall(foo);
assertEquals(2147483648, foo(2147483646));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91ed540^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=91ed540)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=91ed540)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=91ed540)  
[test/mjsunit/regress/regress-5357.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5357.js?cl=91ed540)  
  

---   

## **regress-644633.js (chromium issue)**  
   
**[Issue 644633:
 previous->Is(current) in typer.cc](https://crbug.com/644633)**  
**[Commit: [turbofan] Ensure monotonicity for induction variable typing.](https://chromium.googlesource.com/v8/v8/+/b4f8a7c)**  
  
Date(Commit): Thu Sep 08 03:51:11 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2320803002](https://codereview.chromium.org/2320803002)  
Regress: [mjsunit/compiler/regress-644633.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-644633.js)  
```javascript
var g = -1073741824;

function f() {
  var x = g*g*g*g*g*g*g;
  for (var i = g; i < 1; ) {
    i += i * x;
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4f8a7c^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=b4f8a7c)  
[test/mjsunit/compiler/regress-644633.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-644633.js?cl=b4f8a7c)  
  

---   

## **regress-compare-negate.js (other issue)**  
   
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

%PrepareFunctionForOptimization(CompareNegate);
var x = CompareNegate(1,0x80000000);
%OptimizeFunctionOnNextCall(CompareNegate);
CompareNegate(1,0x80000000);
assertOptimized(CompareNegate);
assertEquals(x, CompareNegate(1,0x80000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fdb0f07^!)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=fdb0f07)  
[test/mjsunit/compiler/regress-compare-negate.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-compare-negate.js?cl=fdb0f07)  
[test/unittests/compiler/arm64/instruction-selector-arm64-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/arm64/instruction-selector-arm64-unittest.cc?cl=fdb0f07)  
  
  
---   

## **regress-crbug-631027.js (chromium issue)**  
   
**[Issue 631027:
 Unreachable code in escape-analysis.cc](https://crbug.com/631027)**  
**[Commit: [turbofan] Handle ObjectIsReceiver in escape analysis.](https://chromium.googlesource.com/v8/v8/+/553d504)**  
  
Date(Commit): Tue Sep 06 11:59:31 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2317623003](https://codereview.chromium.org/2317623003)  
Regress: [mjsunit/regress/regress-crbug-631027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631027.js)  
```javascript
function f() {
  with ({ value:"foo" }) { return value; }
}
assertEquals("foo", f());
%OptimizeFunctionOnNextCall(f);
assertEquals("foo", f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/553d504^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=553d504)  
[test/mjsunit/regress/regress-crbug-631027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631027.js?cl=553d504)  
  

---   

## **regress-642409.js (chromium issue)**  
   
**[Issue 642409:
 Instance of ES6 subclass can be constructed with wrong prototype](https://crbug.com/642409)**  
**[Commit: [Turbofan] Fix CallSuper argument order in BytecodeGraphBuilder.](https://chromium.googlesource.com/v8/v8/+/c950256)**  
  
Date(Commit): Tue Sep 06 11:53:19 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Polymer", "Proj-Ignition"]  
Code Review: [https://codereview.chromium.org/2312103002](https://codereview.chromium.org/2312103002)  
Regress: [mjsunit/regress/regress-642409.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-642409.js)  
```javascript
class SuperClass {
}

class SubClass extends SuperClass {
  constructor() {
    super();
    this.doSomething();
  }
  doSomething() {
  }
}

new SubClass();
new SubClass();
%OptimizeFunctionOnNextCall(SubClass);
new SubClass();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c950256^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=c950256)  
[src/interpreter/interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter.cc?cl=c950256)  
[src/interpreter/interpreter.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter.h?cl=c950256)  
[test/mjsunit/regress/regress-642409.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-642409.js?cl=c950256)  
  

---   

## **regress-crbug-644111.js (chromium issue)**  
   
**[Issue 644111:
 info->shared_info()->HasBytecodeArray() in compiler.cc](https://crbug.com/644111)**  
**[Commit: [compiler] Bytecode preparation fails for asm.js modules.](https://chromium.googlesource.com/v8/v8/+/cc1249b)**  
  
Date(Commit): Mon Sep 05 23:03:07 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2309853002](https://codereview.chromium.org/2309853002)  
Regress: [mjsunit/regress/regress-crbug-644111.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644111.js)  
```javascript
function Module() {
  "use asm";
  return {};
}
var m = Module();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc1249b^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=cc1249b)  
[test/mjsunit/regress/regress-crbug-644111.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644111.js?cl=cc1249b)  
  

---   

## **regress-644048.js (chromium issue)**  
   
**[Issue 644048:
 v8: inputs cause SIGILL/conditional jump or move depends on uninitialized value](https://crbug.com/644048)**  
**[Commit: [turbofan] Don't propagate truncations if output is tagged.](https://chromium.googlesource.com/v8/v8/+/8af781e)**  
  
Date(Commit): Mon Sep 05 20:54:56 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/2311903002](https://codereview.chromium.org/2311903002)  
Regress: [mjsunit/compiler/regress-644048.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-644048.js)  
```javascript
function foo(x) {
  (x
      ? (!0 / 0)
         : x) | 0
}

%PrepareFunctionForOptimization(foo);
foo(1);
foo(2);
%OptimizeFunctionOnNextCall(foo);
foo(3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8af781e^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=8af781e)  
[test/mjsunit/compiler/regress-644048.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-644048.js?cl=8af781e)  
  

---   

## **regress-5342.js (v8 issue)**  
   
**[Issue 5342:
 Error.captureStackTrace now adds its own call to the stack](https://crbug.com/v8/5342)**  
**[Commit: Do not include Error.captureStackTrace in the trace](https://chromium.googlesource.com/v8/v8/+/64c518d)**  
  
Date(Commit): Fri Sep 02 09:51:42 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2307783002](https://codereview.chromium.org/2307783002)  
Regress: [mjsunit/regress/regress-5342.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5342.js)  
```javascript
var o = {}
Error.captureStackTrace(o);
assertEquals(-1, o.stack.indexOf("captureStackTrace"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64c518d^!)  
[src/builtins/builtins-error.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-error.cc?cl=64c518d)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=64c518d)  
[test/mjsunit/regress/regress-5342.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5342.js?cl=64c518d)  
  

---   

## **regress-math-sign-nan-type.js (other issue)**  
   
**[Commit: [turbofan] Fix typing rule for Math.sign.](https://chromium.googlesource.com/v8/v8/+/25504a2)**  
  
Date(Commit): Thu Sep 01 20:06:27 2016  
Code Review: [https://codereview.chromium.org/2306583002](https://codereview.chromium.org/2306583002)  
Regress: [mjsunit/compiler/regress-math-sign-nan-type.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-math-sign-nan-type.js)  
```javascript
function f(a) {
  return Math.sign(+a) < 2;
}

%PrepareFunctionForOptimization(f);
f(NaN);
f(NaN);
%OptimizeFunctionOnNextCall(f);
assertFalse(f(NaN));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/25504a2^!)  
[src/compiler/type-cache.h](https://cs.chromium.org/chromium/src/v8/src/compiler/type-cache.h?cl=25504a2)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=25504a2)  
[test/mjsunit/compiler/regress-math-sign-nan-type.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-math-sign-nan-type.js?cl=25504a2)  
  
  
---   

## **regress-crbug-643073.js (chromium issue)**  
   
**[Issue 643073:
 TypeGuard of kRepWord32 (None) cannot be changed to kRepBit in representation-ch](https://crbug.com/643073)**  
**[Commit: [turbofan] Only check semantic axis for Type::None.](https://chromium.googlesource.com/v8/v8/+/432790c)**  
  
Date(Commit): Thu Sep 01 07:11:21 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2299903002](https://codereview.chromium.org/2299903002)  
Regress: [mjsunit/regress/regress-crbug-643073.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-643073.js)  
```javascript
for (i in [0,0]) {}
function foo() {
  i = 0;
  return i < 0;
}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/432790c^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=432790c)  
[test/mjsunit/regress/regress-crbug-643073.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-643073.js?cl=432790c)  
  

---   

## **regress-5332.js (v8 issue)**  
   
**[Issue 5332:
 Turbofan doesn't initialize holey-double arrays correctly in Debug mode](https://crbug.com/v8/5332)**  
**[Commit: [turbofan] Don't treat the hole NaN as constant inside the compiler.](https://chromium.googlesource.com/v8/v8/+/64a7bd3)**  
  
Date(Commit): Thu Sep 01 06:02:19 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2303643002](https://codereview.chromium.org/2303643002)  
Regress: [mjsunit/regress/regress-5332.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5332.js)  
```javascript
(function() {
  function foo() {
    var a = new Array(2);
    a[1] = 1.5;
    return a;
  }

  assertEquals(undefined, foo()[0]);
  assertEquals(undefined, foo()[0]);
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(undefined, foo()[0]);
})();

(function() {
  function foo() {
    var a = Array(2);
    a[1] = 1.5;
    return a;
  }

  assertEquals(undefined, foo()[0]);
  assertEquals(undefined, foo()[0]);
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(undefined, foo()[0]);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64a7bd3^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=64a7bd3)  
[src/compiler/access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.cc?cl=64a7bd3)  
[src/compiler/access-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.h?cl=64a7bd3)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=64a7bd3)  
[test/mjsunit/regress/regress-5332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5332.js?cl=64a7bd3)  
  

---   

## **regress-crbug-621868.js (chromium issue)**  
   
**[Issue 621868:
 Crash in v8::internal::NewSpace::Verify](https://crbug.com/621868)**  
**[Commit: [crankshaft] Disable further folding already folded allocations.](https://chromium.googlesource.com/v8/v8/+/7b79224)**  
  
Date(Commit): Wed Aug 31 09:48:45 2016  
Components/Type: Blink>JavaScript>GC/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "merge-merged-5.4"]  
Code Review: [https://codereview.chromium.org/2297983003](https://codereview.chromium.org/2297983003)  
Regress: [mjsunit/regress/regress-crbug-621868.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621868.js)  
```javascript
function f(a) {  // First parameter is tagged.
  var n = 1 + a;
}

function g() {
  f();
  var d = {x : f()};
  return [d];
}

g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b79224^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=7b79224)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=7b79224)  
[test/mjsunit/regress/regress-crbug-621868.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-621868.js?cl=7b79224)  
  

---   

## **regress-5320.js (v8 issue)**  
   
**[Issue 5320:
 Deopt loops on 32-bit architectures](https://crbug.com/v8/5320)**  
**[Commit: [turbofan] Treat the INT32 state of a truncating binary op IC as number or oddball on 32-bit machines.](https://chromium.googlesource.com/v8/v8/+/bdf5566)**  
  
Date(Commit): Tue Aug 30 14:13:34 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2292873002](https://codereview.chromium.org/2292873002)  
Regress: [mjsunit/compiler/regress-5320.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5320.js)  
```javascript
function OptimizeTruncatingBinaryOp(func) {
  %PrepareFunctionForOptimization(func);
  func(42, -2);
  func(31, undefined);
  %OptimizeFunctionOnNextCall(func);
  func(-1, 2.1);
  assertOptimized(func);
}

OptimizeTruncatingBinaryOp(function(a, b) { return a >> b; });
OptimizeTruncatingBinaryOp(function(a, b) { return a >>> b; });
OptimizeTruncatingBinaryOp(function(a, b) { return a << b; });
OptimizeTruncatingBinaryOp(function(a, b) { return a & b; });
OptimizeTruncatingBinaryOp(function(a, b) { return a | b; });
OptimizeTruncatingBinaryOp(function(a, b) { return a ^ b; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bdf5566^!)  
[src/compiler/type-hint-analyzer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/type-hint-analyzer.cc?cl=bdf5566)  
[test/mjsunit/compiler/regress-5320.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5320.js?cl=bdf5566)  
  

---   

## **regress-639270.js (chromium issue)**  
   
**[Issue 639270:
 IrOpcode::kFrameState == state->op()->opcode() in instruction-selector.cc](https://crbug.com/639270)**  
**[Commit: Disallow tail calls from async functions and generators](https://chromium.googlesource.com/v8/v8/+/5af4cd9)**  
  
Date(Commit): Mon Aug 29 18:31:35 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-5.3", "merge-merged-5.4", "NodeJS-Backport-Rejected"]  
Code Review: [https://codereview.chromium.org/2278413003](https://codereview.chromium.org/2278413003)  
Regress: [mjsunit/regress/regress-639270.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-639270.js)  
```javascript
"use strict";

var g = (async () => { return JSON.stringify() });

g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5af4cd9^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=5af4cd9)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=5af4cd9)  
[test/message/syntactic-tail-call-generator.js](https://cs.chromium.org/chromium/src/v8/test/message/syntactic-tail-call-generator.js?cl=5af4cd9)  
[test/message/syntactic-tail-call-generator.out](https://cs.chromium.org/chromium/src/v8/test/message/syntactic-tail-call-generator.out?cl=5af4cd9)  
[test/mjsunit/es8/syntactic-tail-call-parsing.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es8/syntactic-tail-call-parsing.js?cl=5af4cd9)  
...  
  

---   

## **regress-638132.js (chromium issue)**  
   
**[Issue 638132:
 Word32Or of kRepWord32 (None) cannot be changed to kRepBit in representation-cha](https://crbug.com/638132)**  
**[Commit: [turbofan] Insert dummy values when changing from None type.](https://chromium.googlesource.com/v8/v8/+/c83b21a)**  
  
Date(Commit): Thu Aug 25 06:06:58 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2266823002](https://codereview.chromium.org/2266823002)  
Regress: [mjsunit/compiler/regress-638132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-638132.js)  
```javascript
function g(x, y) {
  return x | y;
}

function f(b) {
  if (b) {
    var s = g("a", "b") && true;
    return s;
  }
}

g(1, 2);
g(1, 2);

%PrepareFunctionForOptimization(f);
f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c83b21a^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=c83b21a)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=c83b21a)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=c83b21a)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=c83b21a)  
[src/compiler/instruction-codes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-codes.h?cl=c83b21a)  
...  
  

---   

## **regress-639210.js (chromium issue)**  
   
**[Issue 639210:
 Regression: Shockwave plugin for Bond-Breaker-2 game is not working.](https://crbug.com/639210)**  
**[Commit: [turbofan] Fix merging of empty and non-empty state in load elimination.](https://chromium.googlesource.com/v8/v8/+/dc330f2)**  
  
Date(Commit): Wed Aug 24 17:14:24 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["ReleaseBlock-Beta", "hasbisect", "M-54"]  
Code Review: [https://codereview.chromium.org/2270793004](https://codereview.chromium.org/2270793004)  
Regress: [mjsunit/compiler/regress-639210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-639210.js)  
```javascript
var m = (function m() {
  "use asm"
  var i32 = new Int32Array(4);
  var f64 = new Float64Array(4);

  function init() {
    i32[0] = 1;
    f64[0] = 0.1;
  }

  function load(b) {
    return (b ? 0 : i32[0]) + i32[0];
  }

  function store(b) {
    if (b|0) {
    } else {
      f64[0] = 42;
    }
    return f64[0];
  }

  return { init : init, load : load, store : store };
})();

m.init();

%PrepareFunctionForOptimization(m.load);
%OptimizeFunctionOnNextCall(m.load);
assertEquals(2, m.load());

%PrepareFunctionForOptimization(m.store);
%OptimizeFunctionOnNextCall(m.store);
assertEquals(0.1, m.store(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc330f2^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=dc330f2)  
[test/mjsunit/compiler/regress-639210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-639210.js?cl=dc330f2)  
  

---   

## **regress-crbug-635923.js (chromium issue)**  
   
**[Issue 635923:
 !info->shared_info()->is_compiled() in compiler.cc](https://crbug.com/635923)**  
**[Commit: [compiler] Make Compiler::EnsureBytecode not switch tiers.](https://chromium.googlesource.com/v8/v8/+/b52aeca)**  
  
Date(Commit): Wed Aug 24 14:09:59 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Proj-Ignition"]  
Code Review: [https://codereview.chromium.org/2278543002](https://codereview.chromium.org/2278543002)  
Regress: [mjsunit/regress/regress-crbug-635923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-635923.js)  
```javascript
function f(x) { return x + 23 }
function g(x) { return f(x) + 42 }

assertEquals(23, f(0));
assertEquals(24, f(1));
assertEquals(67, g(2));
assertEquals(68, g(3));

%OptimizeFunctionOnNextCall(g);
assertEquals(65, g(0));

%OptimizeFunctionOnNextCall(f);
assertEquals(23, f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b52aeca^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=b52aeca)  
[test/mjsunit/regress/regress-crbug-635923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-635923.js?cl=b52aeca)  
  

---   

## **regress-crbug-640369.js (chromium issue)**  
   
**[Issue 640369:
 IrOpcode::kLoop == loop->opcode() in verifier.cc](https://crbug.com/640369)**  
**[Commit: [turbofan] Use ObjectIsReceiver directly for inlining.](https://chromium.googlesource.com/v8/v8/+/6646d73)**  
  
Date(Commit): Wed Aug 24 11:09:32 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2271973003](https://codereview.chromium.org/2271973003)  
Regress: [mjsunit/regress/regress-crbug-640369.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-640369.js)  
```javascript
function A() {
  this.x = 0;
  for (var i = 0; i < max; ) {}
}
function foo() {
  for (var i = 0; i < 1; i = 2) %OptimizeOsr();
  return new A();
}
try { foo(); } catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6646d73^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=6646d73)  
[src/compiler/js-inlining.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.h?cl=6646d73)  
[test/mjsunit/regress/regress-crbug-640369.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-640369.js?cl=6646d73)  
  

---   

## **regress-crbug-638551.js (chromium issue)**  
   
**[Issue 638551:
 !shared->HasBytecodeArray() in compiler.cc](https://crbug.com/638551)**  
**[Commit: [interpreter] Fix canonicalization when preserving bytecode.](https://chromium.googlesource.com/v8/v8/+/8ab555c)**  
  
Date(Commit): Thu Aug 18 10:42:40 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2245263006](https://codereview.chromium.org/2245263006)  
Regress: [mjsunit/regress/regress-crbug-638551.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-638551.js)  
```javascript
function f() {
  for (var i = 0; i < 10; i++) if (i == 5) %OptimizeOsr();
  function g() {}
  %OptimizeFunctionOnNextCall(g);
  g();
}
f();
gc();  // Make sure that ...
gc();  // ... code flushing ...
gc();  // ... clears code ...
gc();  // ... attached to {g}.
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ab555c^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=8ab555c)  
[test/mjsunit/regress/regress-crbug-638551.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-638551.js?cl=8ab555c)  
  

---   

## **regress-638134.js (chromium issue)**  
   
**[Issue 638134:
 slice->AllElementsAreUnique() in constant-array-builder.cc](https://crbug.com/638134)**  
**[Commit: [Parser] Track ContainsDot for SMI values.](https://chromium.googlesource.com/v8/v8/+/477495c)**  
  
Date(Commit): Thu Aug 18 08:15:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2248693005](https://codereview.chromium.org/2248693005)  
Regress: [mjsunit/regress/regress-638134.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-638134.js)  
```javascript
function foo() {
  // Generates a forward branch that puts 200 in the constant pool.
  var i = 0;
  if (i) {
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
    i = 0; i = 0; i = 0; i = 0; i = 0; i = 0;
  }
  // Emit a 200 literal which also ends up in the constant pool.
  var j = 0.2e3;
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/477495c^!)  
[src/ast/ast-value-factory.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.cc?cl=477495c)  
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=477495c)  
[test/cctest/interpreter/bytecode_expectations/PrimitiveReturnStatements.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/PrimitiveReturnStatements.golden?cl=477495c)  
[test/cctest/interpreter/bytecode_expectations/UnaryOperators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/UnaryOperators.golden?cl=477495c)  
[test/mjsunit/regress/regress-638134.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-638134.js?cl=477495c)  
  

---   

## **regress-633497.js (chromium issue)**  
   
**[Issue 633497:
 Crash in asm.js emulator application](https://crbug.com/633497)**  
**[Commit: [turbofan] Only do value numbering when types are compatible.](https://chromium.googlesource.com/v8/v8/+/b190d13)**  
  
Date(Commit): Wed Aug 17 08:45:26 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/2251833002](https://codereview.chromium.org/2251833002)  
Regress: [mjsunit/compiler/regress-633497.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-633497.js)  
```javascript
function f(a) {
  var x;
  a = a|0;
  var dummy;
  if (a === 1) {
    x = 277.5;
  } else if (a === 2) {
    x = 0;
  } else {
    dummy = 527.5;
    dummy = 958.5;
    dummy = 1143.5;
    dummy = 1368.5;
    dummy = 1558.5;
    x = 277.5;
  }
  return +x;
}

%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(277.5, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b190d13^!)  
[src/compiler/value-numbering-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/value-numbering-reducer.cc?cl=b190d13)  
[test/mjsunit/compiler/regress-633497.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-633497.js?cl=b190d13)  
  

---   

## **regress-5286.js (v8 issue)**  
   
**[Issue 5286:
 TurboFan produces wrong result for speculative int32 modulus](https://crbug.com/v8/5286)**  
**[Commit: [turbofan] Fix CheckedInt32Mod lowering for -0 case with negative left hand side.](https://chromium.googlesource.com/v8/v8/+/665f0e4)**  
  
Date(Commit): Fri Aug 12 12:13:51 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2243803002](https://codereview.chromium.org/2243803002)  
Regress: [mjsunit/regress/regress-5286.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5286.js)  
```javascript
(function() {
  function foo(x, y) { return x % y; }

  assertEquals(0, foo(2, 2));
  assertEquals(0, foo(4, 4));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-0, foo(-8, 8));
})();

(function() {
  function foo(x, y) { return x % y; }

  assertEquals(0, foo(1, 1));
  assertEquals(0, foo(2, 2));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-0, foo(-3, 3));
})();

(function() {
  function foo(x, y) { return x % y; }

  assertEquals(0, foo(1, 1));
  assertEquals(0, foo(2, 2));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-0, foo(-2147483648, -1));
})();

(function() {
  function foo(x, y) { return x % y; }

  assertEquals(0, foo(1, 1));
  assertEquals(0, foo(2, 2));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-0, foo(-2147483648, -2147483648));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/665f0e4^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=665f0e4)  
[test/mjsunit/regress/regress-5286.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5286.js?cl=665f0e4)  
  

---   

## **regress-5278.js (v8 issue)**  
   
**[Issue 5278:
 Wrong handling of <negative integer> % 1 in TurboFan.](https://crbug.com/v8/5278)**  
**[Commit: [turbofan] Fix CheckedInt32Mod lowering.](https://chromium.googlesource.com/v8/v8/+/9e14155)**  
  
Date(Commit): Wed Aug 10 09:24:59 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2233713002](https://codereview.chromium.org/2233713002)  
Regress: [mjsunit/compiler/regress-5278.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5278.js)  
```javascript
function foo(a, b) {
  return a % b;
}
%PrepareFunctionForOptimization(foo);
foo(2, 1);
foo(2, 1);
%OptimizeFunctionOnNextCall(foo);
assertEquals(-0, foo(-2, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e14155^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=9e14155)  
[test/mjsunit/compiler/regress-5278.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5278.js?cl=9e14155)  
  

---   

## **regress-5275-1.js (v8 issue)**  
   
**[Issue 5275:
 webkit/fast/js/array-bad-time flakily failing under --turbo](https://crbug.com/v8/5275)**  
**[Commit: [turbofan] Properly guard keyed stores wrt. setters in the prototype chain.](https://chromium.googlesource.com/v8/v8/+/7060bab8)**  
  
Date(Commit): Wed Aug 10 06:30:22 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2231683002](https://codereview.chromium.org/2231683002)  
Regress: [mjsunit/regress/regress-5275-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5275-1.js), [mjsunit/regress/regress-5275-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5275-2.js)  
```javascript
function foo(x) {
  var a = new Array(1);
  a[0] = x;
  return a;
}

assertEquals([1], foo(1));
assertEquals([1], foo(1));
%OptimizeFunctionOnNextCall(foo);
assertEquals([1], foo(1));
Array.prototype.__defineSetter__("0", function() {});
assertEquals([undefined], foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7060bab8^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=7060bab8)  
[test/mjsunit/regress/regress-5275-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5275-1.js?cl=7060bab8)  
[test/mjsunit/regress/regress-5275-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5275-2.js?cl=7060bab8)  
[test/webkit/webkit.status](https://cs.chromium.org/chromium/src/v8/test/webkit/webkit.status?cl=7060bab8)  
  

---   

## **regress-crbug-635798.js (chromium issue)**  
   
**[Issue 635798:
 Crash in v8::internal::Context::native_context](https://crbug.com/635798)**  
**[Commit: [runtime] %GrowArrayElements doesn't have a native context in TurboFan.](https://chromium.googlesource.com/v8/v8/+/78727d4)**  
  
Date(Commit): Tue Aug 09 13:03:07 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "findit-wrong", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2224253003](https://codereview.chromium.org/2224253003)  
Regress: [mjsunit/regress/regress-crbug-635798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-635798.js)  
```javascript
function foo() {
  var x = [];
  var y = [];
  x.__proto__ = y;
  for (var i = 0; i < 10000; ++i) {
    y[i] = 1;
  }
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78727d4^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=78727d4)  
[test/mjsunit/regress/regress-crbug-635798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-635798.js?cl=78727d4)  
  

---   

## **regress-635429.js (chromium issue)**  
   
**[Issue 635429:
 slice->AllElementsAreUnique() in constant-array-builder.cc](https://crbug.com/635429)**  
**[Commit: [Interpreter] Don't try to create bytecode array if HasStackOverflow().](https://chromium.googlesource.com/v8/v8/+/c1ae15d)**  
  
Date(Commit): Tue Aug 09 07:24:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2228503004](https://codereview.chromium.org/2228503004)  
Regress: [mjsunit/regress/regress-635429.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-635429.js)  
```javascript
function foo() {
 "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "boom"};

try {
  foo()
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c1ae15d^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=c1ae15d)  
[test/mjsunit/regress/regress-635429.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-635429.js?cl=c1ae15d)  
  

---   

## **regress-loop-variable-if.js (other issue)**  
   
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
[src/compiler/loop-variable-optimizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/loop-variable-optimizer.cc?cl=ad8e0e2)  
[test/mjsunit/compiler/regress-loop-variable-if.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-loop-variable-if.js?cl=ad8e0e2)  
  
  
---   

## **regress-loop-variable-unsigned.js (other issue)**  
   
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
[src/compiler/common-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.cc?cl=e144335)  
[src/compiler/common-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.h?cl=e144335)  
[src/compiler/loop-variable-optimizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/loop-variable-optimizer.cc?cl=e144335)  
[src/compiler/loop-variable-optimizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/loop-variable-optimizer.h?cl=e144335)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=e144335)  
...  
  
  
---   

## **regress-crbug-632800.js (chromium issue)**  
   
**[Issue 632800:
 next_tier == Compiler::OPTIMIZED in runtime-profiler.cc](https://crbug.com/632800)**  
**[Commit: [interpreter] Fix profiler when hitting OSR frame.](https://chromium.googlesource.com/v8/v8/+/f00b42a)**  
  
Date(Commit): Fri Aug 05 08:47:48 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2208603005](https://codereview.chromium.org/2208603005)  
Regress: [mjsunit/regress/regress-crbug-632800.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-632800.js)  
```javascript
function osr() {
  for (var i = 0; i < 50000; ++i) Math.random();
}
osr();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f00b42a^!)  
[src/runtime-profiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime-profiler.cc?cl=f00b42a)  
[src/runtime-profiler.h](https://cs.chromium.org/chromium/src/v8/src/runtime-profiler.h?cl=f00b42a)  
[test/mjsunit/regress/regress-crbug-632800.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-632800.js?cl=f00b42a)  
  

---   

## **regress-5262.js (v8 issue)**  
   
**[Issue 5262:
 OSR code generated from bytecode might trigger deopt after underlying function was tiered up](https://crbug.com/v8/5262)**  
**[Commit: [interpreter] Avoid tier-up when there is an OSR activation.](https://chromium.googlesource.com/v8/v8/+/5671b66)**  
  
Date(Commit): Fri Aug 05 07:55:03 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2206363004](https://codereview.chromium.org/2206363004)  
Regress: [mjsunit/regress/regress-5262.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5262.js)  
```javascript
function g() { return 23 }
function h() { return 42 }
function boom(o) { o.g = h }
function f(osr_and_recurse) {
  if (osr_and_recurse) {
    for (var i = 0; i < 3; ++i) {
      if (i == 1) %OptimizeOsr();
    }
    %OptimizeFunctionOnNextCall(f);
    f(false);     // Trigger tier-up due to recursive call.
    boom(this);   // Causes a deopt due to below dependency.
    var x = g();  // Install dependency on the {g} function.
    return x;
  }
  return 65;
}
assertEquals(65, f(false));
assertEquals(65, f(false));
assertEquals(42, f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5671b66^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=5671b66)  
[test/mjsunit/regress/regress-5262.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5262.js?cl=5671b66)  
  

---   

## **regress-crbug-616709-1.js (chromium issue)**  
   
**[Issue 616709:
 map->is_stable() in src/compilation-dependencies.cc](https://crbug.com/616709)**  
**[Commit: [turbofan] Remove unnecessary prototype checks for element access.](https://chromium.googlesource.com/v8/v8/+/cad5b29)**  
  
Date(Commit): Fri Aug 05 04:55:03 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Arch-All", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2198833002](https://codereview.chromium.org/2198833002)  
Regress: [mjsunit/regress/regress-crbug-616709-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-616709-1.js), [mjsunit/regress/regress-crbug-616709-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-616709-2.js)  
```javascript
for (var i = 0; i < 2000; i++) {
  Object.prototype['X'+i] = true;
}

function boom(a1) {
  return a1[0];
}

var a = new Array(1);
a[0] = 0.1;
boom(a);
boom(a);
%OptimizeFunctionOnNextCall(boom);
boom(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cad5b29^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=cad5b29)  
[src/compiler/access-info.h](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.h?cl=cad5b29)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=cad5b29)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=cad5b29)  
[test/mjsunit/regress/regress-crbug-616709-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-616709-1.js?cl=cad5b29)  
...  
  

---   

## **regress-634357.js (chromium issue)**  
   
**[Issue 634357:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsSeededNumberDictionary())](https://crbug.com/634357)**  
**[Commit: [elements] update Dictionary in IncludesValue if own elements change](https://chromium.googlesource.com/v8/v8/+/9977a2c)**  
  
Date(Commit): Thu Aug 04 19:09:30 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2212963002](https://codereview.chromium.org/2212963002)  
Regress: [mjsunit/es7/regress/regress-634357.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es7/regress/regress-634357.js)  
```javascript
array = new Array({}, {}, {});
Object.defineProperty(array, 1, {
  get: function() {
    array.length = 0;
    array[0] = -2147483648;
  }
});
result = array.includes(new Array());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9977a2c^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=9977a2c)  
[test/mjsunit/es7/regress/regress-634273.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-634273.js?cl=9977a2c)  
[test/mjsunit/es7/regress/regress-634357.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-634357.js?cl=9977a2c)  
  

---   

## **regress-634273.js (chromium issue)**  
   
**[Issue 634273:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsSmi()) in objects-inl.h](https://crbug.com/634273)**  
**[Commit: [elements] update Dictionary in IncludesValue if own elements change](https://chromium.googlesource.com/v8/v8/+/9977a2c)**  
  
Date(Commit): Thu Aug 04 19:09:30 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2212963002](https://codereview.chromium.org/2212963002)  
Regress: [mjsunit/es7/regress/regress-634273.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es7/regress/regress-634273.js)  
```javascript
array = new Array(undefined, undefined, undefined);
Object.defineProperty(array, 0, {
  get: function() {
    array.push(undefined, undefined);
  }
});
array[0x80000] = 1;
result = array.includes(new WeakMap());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9977a2c^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=9977a2c)  
[test/mjsunit/es7/regress/regress-634273.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-634273.js?cl=9977a2c)  
[test/mjsunit/es7/regress/regress-634357.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-634357.js?cl=9977a2c)  
  

---   

## **regress-crbug-633884.js (chromium issue)**  
   
**[Issue 633884:
 !value->IsTheHole(isolate) in runtime-scopes.cc](https://crbug.com/633884)**  
**[Commit: Properly pass InitializationFlag back from ScriptContextTable lookups](https://chromium.googlesource.com/v8/v8/+/e6d2c9b)**  
  
Date(Commit): Thu Aug 04 16:13:41 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2203213003](https://codereview.chromium.org/2203213003)  
Regress: [mjsunit/regress/regress-crbug-633884.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-633884.js)  
```javascript
try {
  // Leave "blarg" as the hole in a new ScriptContext.
  Realm.eval(Realm.current(), "throw Error(); let blarg");
} catch (e) { }

assertThrows(function() {
  // Prevent full-codegen from optimizing away the %LoadLookupSlot call.
  eval("var x = 5");
  blarg;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6d2c9b^!)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=e6d2c9b)  
[test/mjsunit/regress/regress-crbug-633884.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-633884.js?cl=e6d2c9b)  
  

---   

## **regress-634269.js (chromium issue)**  
   
**[Issue 634269:
 (index >= 0) && (index < this->length()) in objects-inl.h](https://crbug.com/634269)**  
**[Commit: [elements] limit TypedElementsAccessor::IncludesValue to backing store length](https://chromium.googlesource.com/v8/v8/+/0d7f7dc)**  
  
Date(Commit): Thu Aug 04 15:54:55 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2209273002](https://codereview.chromium.org/2209273002)  
Regress: [mjsunit/es7/regress/regress-634269.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es7/regress/regress-634269.js)  
```javascript
__v_1 = new Uint8Array();
Object.defineProperty(__v_1.__proto__, 'length', {value: 42});
Array.prototype.includes.call(new Uint8Array(), 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0d7f7dc^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=0d7f7dc)  
[test/mjsunit/es7/regress/regress-634269.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-634269.js?cl=0d7f7dc)  
  

---   

## **regress-crbug-633585.js (chromium issue)**  
   
**[Issue 633585:
 Crash in v8::internal::InnerPointerToCodeCache::GcSafeFindCodeForInnerPointer](https://crbug.com/633585)**  
**[Commit: [turbofan] Fix missing bailout for accessors in literals.](https://chromium.googlesource.com/v8/v8/+/667d8ad)**  
  
Date(Commit): Thu Aug 04 10:28:46 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-53", "Security_Impact-Stable", "Security_Severity-Medium", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "Release-0-M53", "merge-merged-5.3", "Merge-Merged-53", "CVE-2016-5167", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/2207413002](https://codereview.chromium.org/2207413002)  
Regress: [mjsunit/regress/regress-crbug-633585.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-633585.js)  
```javascript
function f() { this.x = this.x.x; }
gc();
f.prototype.x = { x:1 }
new f();
new f();

function g() {
  function h() {};
  h.prototype = { set x(value) { } };
  new f();
}
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/667d8ad^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=667d8ad)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=667d8ad)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=667d8ad)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=667d8ad)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=667d8ad)  
...  
  

---   

## **regress-633998.js (chromium issue)**  
   
**[Issue 633998:
 Stack-overflow in v8::internal::PerThreadAssertScope<](https://crbug.com/633998)**  
**[Commit: Handle stack overflows in NoSideEffectToString](https://chromium.googlesource.com/v8/v8/+/ea6b960)**  
  
Date(Commit): Thu Aug 04 07:45:11 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2206313002](https://codereview.chromium.org/2206313002)  
Regress: [mjsunit/regress/regress-633998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-633998.js)  
```javascript
var err_str_1 = "apply was called on , which is a object and not a function";
var err_str_2 =
  "apply was called on Error, which is a object and not a function";

var reached = false;
var error = new Error();
error.name = error;
try {
  Reflect.apply(error);
  reached = true;
} catch (e) {
  assertTrue(e.stack.indexOf(err_str_1) != -1);
} finally {
  assertFalse(reached);
}

reached = false;
error = new Error();
error.msg = error;
try {
  Reflect.apply(error);
  reached = true;
} catch (e) {
  assertTrue(e.stack.indexOf(err_str_2) != -1);
} finally {
  assertFalse(reached);
}

reached = false;
error = new Error();
error.name = error;
error.msg = error;
try {
  Reflect.apply(error);
  reached = true;
} catch (e) {
  assertTrue(e.stack.indexOf(err_str_1) != -1);
} finally {
  assertFalse(reached);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea6b960^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ea6b960)  
[test/mjsunit/regress/regress-633998.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-633998.js?cl=ea6b960)  
  

---   

## **regress-632289.js (chromium issue)**  
   
**[Issue 632289:
 !info->shared_info()->is_compiled() in compiler.cc](https://crbug.com/632289)**  
**[Commit: [Crankshaft] Move don't crankshaft check before EnsureDeoptimizationSupport.](https://chromium.googlesource.com/v8/v8/+/aacbdac)**  
  
Date(Commit): Wed Aug 03 15:02:38 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2194453002](https://codereview.chromium.org/2194453002)  
Regress: [mjsunit/regress/regress-632289.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-632289.js)  
```javascript
try {
} catch(e) {; }
(function __f_12() {
})();
(function __f_6() {
  function __f_3() {
  }
  function __f_4() {
    try {
    } catch (e) {
    }
  }
 __f_4();
  %OptimizeFunctionOnNextCall(__f_4);
 __f_4();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aacbdac^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=aacbdac)  
[test/mjsunit/regress/regress-632289.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-632289.js?cl=aacbdac)  
  

---   

## **regress-633883.js (chromium issue)**  
   
**[Issue 633883:
 args[1]->IsString() in runtime-strings.cc](https://crbug.com/633883)**  
**[Commit: [builtins] fix mapcheck in Array.includes fast-case when searching for String](https://chromium.googlesource.com/v8/v8/+/c4ee3d9)**  
  
Date(Commit): Wed Aug 03 14:27:38 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2207903002](https://codereview.chromium.org/2207903002)  
Regress: [mjsunit/es7/regress/regress-633883.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es7/regress/regress-633883.js)  
```javascript
v5 = new Array();
v17 = encodeURIComponent(v5);
v19 = isFinite();
v34 = new Array(v19);
v47 = v34.includes(v17);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c4ee3d9^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=c4ee3d9)  
[test/mjsunit/es7/regress/regress-633883.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-633883.js?cl=c4ee3d9)  
  

---   

## **regress-v8-5255-1.js (v8 issue)**  
   
**[Issue 5255:
 Representation selection is wrong for Word32/Float64 truncations on non-Number/non-Oddball values](https://crbug.com/v8/5255)**  
**[Commit: [turbofan] Fix invalid representation selection for Phis/Selects.](https://chromium.googlesource.com/v8/v8/+/c9324fe)**  
  
Date(Commit): Tue Aug 02 12:11:09 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2206553002](https://codereview.chromium.org/2206553002)  
Regress: [mjsunit/regress/regress-v8-5255-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5255-1.js), [mjsunit/regress/regress-v8-5255-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5255-2.js), [mjsunit/regress/regress-v8-5255-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5255-3.js)  
```javascript
function foo(x) {
  return (x ? true : "7") >> 0;
}

assertEquals(1, foo(1));
assertEquals(1, foo(1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(7, foo(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9324fe^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=c9324fe)  
[test/mjsunit/regress/regress-v8-5255-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5255-1.js?cl=c9324fe)  
[test/mjsunit/regress/regress-v8-5255-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5255-2.js?cl=c9324fe)  
[test/mjsunit/regress/regress-v8-5255-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5255-3.js?cl=c9324fe)  
  

---   

## **regress-5252.js (v8 issue)**  
   
**[Issue 5252:
 Running benchmark Octane21.-TF.json crashes](https://crbug.com/v8/5252)**  
**[Commit: [interpreter] Elide OSR polling from fake loops.](https://chromium.googlesource.com/v8/v8/+/962fd4a)**  
  
Date(Commit): Tue Aug 02 09:16:59 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2200733002](https://codereview.chromium.org/2200733002)  
Regress: [mjsunit/regress/regress-5252.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5252.js)  
```javascript
(function TestNonLoopyLoop() {
  function f() {
    do {
      %OptimizeOsr();
      return 23;
    } while(false)
  }
  assertEquals(23, f());
  assertEquals(23, f());
})();

(function TestNonLoopyGenerator() {
  function* g() {
    do {
      %OptimizeOsr();
      yield 23;
      yield 42;
    } while(false)
    return 999;
  }
  var gen = g();
  assertEquals({ value:23, done:false }, gen.next());
  assertEquals({ value:42, done:false }, gen.next());
  assertEquals({ value:999, done:true }, gen.next());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/962fd4a^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=962fd4a)  
[src/interpreter/control-flow-builders.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/control-flow-builders.cc?cl=962fd4a)  
[src/interpreter/control-flow-builders.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/control-flow-builders.h?cl=962fd4a)  
[test/cctest/interpreter/bytecode_expectations/RemoveRedundantLdar.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/RemoveRedundantLdar.golden?cl=962fd4a)  
[test/mjsunit/regress/regress-5252.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5252.js?cl=962fd4a)  
  

---   

## **regress-v8-5254-1.js (v8 issue)**  
   
**[Issue 5254:
 Narrowing comparison opcodes is wrong in TurboFan](https://crbug.com/v8/5254)**  
**[Commit: [turbofan] Fix invalid comparison operator narrowing.](https://chromium.googlesource.com/v8/v8/+/a758144)**  
  
Date(Commit): Tue Aug 02 07:46:15 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2202803003](https://codereview.chromium.org/2202803003)  
Regress: [mjsunit/regress/regress-v8-5254-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5254-1.js), [mjsunit/regress/regress-v8-5254-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5254-2.js)  
```javascript
var foo = (function() {
  "use asm";
  var a = new Uint16Array(2);
  a[0] = 32815;
  a[1] = 32114;

  function foo() {
    var x = a[0]|0;
    var y = a[1]|0;
    if (x < 0) x = 4294967296 + x|0;
    if (y < 0) y = 4294967296 + y|0;
    return x >= y;
  }

  return foo;
})();

assertTrue(foo());
assertTrue(foo());
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a758144^!)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=a758144)  
[src/compiler/instruction-selector-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector-impl.h?cl=a758144)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=a758144)  
[test/mjsunit/regress/regress-v8-5254-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5254-1.js?cl=a758144)  
[test/mjsunit/regress/regress-v8-5254-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-5254-2.js?cl=a758144)  
  

---   

## **regress-5245.js (v8 issue)**  
   
**[Issue 5245:
 Error.capturStackTrace(a, Error) now installs readOnly stack property](https://crbug.com/v8/5245)**  
**[Commit: Set Error.stack property writable](https://chromium.googlesource.com/v8/v8/+/1c7c052)**  
  
Date(Commit): Fri Jul 29 08:15:26 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2190313002](https://codereview.chromium.org/2190313002)  
Regress: [mjsunit/regress/regress-5245.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5245.js)  
```javascript
"use strict";


var a = {};
Error.captureStackTrace(a, Error);
a.stack = 1;  // Should not throw, stack should be writable.


var b = new Error();
b.stack = 1;  // Should not throw, stack should be writable.
b.stack = 1;  // Still writable.


var c = new Error();
var old_stack = c.stack;
c.stack = 1;  // Should not throw, stack should be writable.
c.stack = 1;  // Still writable.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1c7c052^!)  
[src/builtins/builtins-error.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-error.cc?cl=1c7c052)  
[test/mjsunit/regress/regress-5245.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5245.js?cl=1c7c052)  
[test/mjsunit/stack-traces.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/stack-traces.js?cl=1c7c052)  
  

---   

## **regress-629792-source-position-on-jump.js (chromium issue)**  
   
**[Issue 629792:
 CanElideLastBasedOnSourcePosition(node) in bytecode-peephole-optimizer.cc](https://crbug.com/629792)**  
**[Commit: [interpreter] Fix peephole rule on eliding last before jump.](https://chromium.googlesource.com/v8/v8/+/02b0985)**  
  
Date(Commit): Thu Jul 28 14:41:26 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2185123003](https://codereview.chromium.org/2185123003)  
Regress: [mjsunit/ignition/regress-629792-source-position-on-jump.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-629792-source-position-on-jump.js)  
```javascript
function f(t) {
  var f = t || this;
  for (var i in t) {
    for (var j in t) {
      (j);
      continue;
    }
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/02b0985^!)  
[src/interpreter/bytecode-peephole-optimizer.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-peephole-optimizer.cc?cl=02b0985)  
[test/mjsunit/ignition/regress-629792-source-position-on-jump.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/ignition/regress-629792-source-position-on-jump.js?cl=02b0985)  
  

---   

## **regress-crbug-631917.js (chromium issue)**  
   
**[Issue 631917:
 args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/631917)**  
**[Commit: [fullcode][mips][mips64][ppc][s390] Avoid trashing of a home object when doing a count operation with keyed load/store to a super.](https://chromium.googlesource.com/v8/v8/+/fc66694)**  
  
Date(Commit): Thu Jul 28 14:31:07 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "findit-wrong", "M-54", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2191663004](https://codereview.chromium.org/2191663004)  
Regress: [mjsunit/regress/regress-crbug-631917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631917.js)  
```javascript
var b = { toString: function() { return "b"; } };
var c = { toString: function() { return "c"; } };

(function() {
  var expected_receiver;
  var obj1 = {
    a: 100,
    b_: 200,
    get b() { assertEquals(expected_receiver, this); return this.b_; },
    set b(v) { assertEquals(expected_receiver, this); this.b_ = v; },
    c_: 300,
    get c() { assertEquals(expected_receiver, this); return this.c_; },
    set c(v) { assertEquals(expected_receiver, this); this.c_ = v; },
  };
  var obj2 = {
    boom() {
      super.a++;
      super[b]++;
      super[c]++;
    },
  }
  Object.setPrototypeOf(obj2, obj1);

  expected_receiver = obj2;
  obj2.boom();
  assertEquals(101, obj2.a);
  assertEquals(201, obj2[b]);
  assertEquals(301, obj2[c]);

  expected_receiver = obj1;
  assertEquals(100, obj1.a);
  assertEquals(200, obj1[b]);
  assertEquals(300, obj1[c]);
}());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc66694^!)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=fc66694)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=fc66694)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=fc66694)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=fc66694)  
[test/mjsunit/regress/regress-crbug-631917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631917.js?cl=fc66694)  
  

---   

## **regress-631050.js (chromium issue)**  
   
**[Issue 631050:
 Crash in v8::internal::JSObject::UpdateAllocationSite](https://crbug.com/631050)**  
**[Commit: [heap] Don't consider mementos on pages below age mark](https://chromium.googlesource.com/v8/v8/+/e97b868)**  
  
Date(Commit): Wed Jul 27 12:18:16 2016  
Components/Type: Blink>JavaScript>GC/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "M-54", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2179033005](https://codereview.chromium.org/2179033005)  
Regress: [mjsunit/regress/regress-631050.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-631050.js)  
```javascript
function __f_3(x, expected) {
  var __v_3 = [];
  __v_3.length = x;
  __f_3(true, 1);
}

try {
  __f_3(2147483648, 2147483648);
} catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e97b868^!)  
[src/heap/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap-inl.h?cl=e97b868)  
[test/mjsunit/regress/regress-631050.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-631050.js?cl=e97b868)  
  

---   

## **regress-crbug-631318-1.js (chromium issue)**  
   
**[Issue 631318:
 Dead-code elimination during representation selection is too aggressive](https://crbug.com/631318)**  
**[Commit: [turbofan] Fix overly aggressive dead code elimination.](https://chromium.googlesource.com/v8/v8/+/32346aa)**  
  
Date(Commit): Tue Jul 26 07:09:58 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Arch-All"]  
Code Review: [https://codereview.chromium.org/2182003002](https://codereview.chromium.org/2182003002)  
Regress: [mjsunit/regress/regress-crbug-631318-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-1.js), [mjsunit/regress/regress-crbug-631318-10.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-10.js), [mjsunit/regress/regress-crbug-631318-11.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-11.js), [mjsunit/regress/regress-crbug-631318-12.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-12.js), [mjsunit/regress/regress-crbug-631318-13.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-13.js), [mjsunit/regress/regress-crbug-631318-14.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-14.js), [mjsunit/regress/regress-crbug-631318-15.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-15.js), [mjsunit/regress/regress-crbug-631318-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-2.js), [mjsunit/regress/regress-crbug-631318-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-3.js), [mjsunit/regress/regress-crbug-631318-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-4.js), [mjsunit/regress/regress-crbug-631318-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-5.js), [mjsunit/regress/regress-crbug-631318-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-6.js), [mjsunit/regress/regress-crbug-631318-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-7.js), [mjsunit/regress/regress-crbug-631318-8.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-8.js), [mjsunit/regress/regress-crbug-631318-9.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-9.js)  
```javascript
function foo(x) { return x < x; }
foo(1);
foo(2);

function bar(x) { foo(x); }
%OptimizeFunctionOnNextCall(bar);

assertThrows(() => bar(Symbol.toPrimitive));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/32346aa^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=32346aa)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-1.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-10.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-10.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-11.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-11.js?cl=32346aa)  
...  
  

---   

## **regress-crbug-630952.js (chromium issue)**  
   
**[Issue 630952:
 IsUseLessGeneral(input_use_infos_[index], use_info) in simplified-lowering.cc](https://crbug.com/630952)**  
**[Commit: [Turbofan] IsUseLessGeneral shouldn't consider machine representation.](https://chromium.googlesource.com/v8/v8/+/480f155)**  
  
Date(Commit): Mon Jul 25 12:01:54 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2177193002](https://codereview.chromium.org/2177193002)  
Regress: [mjsunit/regress/regress-crbug-630952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630952.js)  
```javascript
try {
function __f_4(sign_bit,
                          mantissa_29_bits) {
}
__f_4.prototype.returnSpecial = function() {
                     this.mantissa_29_bits * mantissa_29_shift;
}
__f_4.prototype.toSingle = function() {
  if (-65535) return this.toSingleSubnormal();
}
__f_4.prototype.toSingleSubnormal = function() {
  if (__v_15) {
      var __v_7 = this.mantissa_29_bits == -1 &&
                 (__v_13 & __v_10 ) == 0;
    }
  __v_8 >>= __v_7;
}
__v_14 = new __f_4();
__v_14.toSingle();
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/480f155^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=480f155)  
[test/mjsunit/regress/regress-crbug-630952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630952.js?cl=480f155)  
  

---   

## **regress-crbug-630951.js (chromium issue)**  
   
**[Issue 630951:
 Unreachable code in verifier.cc](https://crbug.com/630951)**  
**[Commit: [turbofan] Avoid introducing machine operators during typed lowering.](https://chromium.googlesource.com/v8/v8/+/5bed151)**  
  
Date(Commit): Mon Jul 25 10:38:00 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2182453002](https://codereview.chromium.org/2182453002)  
Regress: [mjsunit/regress/regress-crbug-630951.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630951.js)  
```javascript
function foo() {
  "use asm";
  var o = new Int32Array(64 * 1024);
  return () => { o[i1 >> 2] | 0; }
}
assertThrows(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bed151^!)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=5bed151)  
[src/compiler/js-intrinsic-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.h?cl=5bed151)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=5bed151)  
[src/compiler/js-typed-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.h?cl=5bed151)  
[test/mjsunit/regress/regress-crbug-630951.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630951.js?cl=5bed151)  
...  
  

---   

## **regress-crbug-630559.js (chromium issue)**  
   
**[Issue 630559:
 Token::CATCH == tok in parser.cc](https://crbug.com/630559)**  
**[Commit: Native try-catch syntax parsing should not crash.](https://chromium.googlesource.com/v8/v8/+/9868142)**  
  
Date(Commit): Mon Jul 25 05:32:28 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2176613002](https://codereview.chromium.org/2176613002)  
Regress: [mjsunit/regress/regress-crbug-630559.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630559.js)  
```javascript
assertThrows("try{}%");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9868142^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=9868142)  
[test/mjsunit/regress/regress-crbug-630559.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630559.js?cl=9868142)  
  

---   

## **regress-crbug-630923.js (chromium issue)**  
   
**[Issue 630923:
 0 == node->op()->ControlInputCount() in simplified-lowering.cc](https://crbug.com/630923)**  
**[Commit: [turbofan] Remove overly restrictive DCHECK.](https://chromium.googlesource.com/v8/v8/+/e3e347b)**  
  
Date(Commit): Mon Jul 25 05:22:19 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2177133002](https://codereview.chromium.org/2177133002)  
Regress: [mjsunit/regress/regress-crbug-630923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630923.js)  
```javascript
var o = {};
function bar(o) {
  return 1 + (o.t ? 1 : 2);
}
function foo() {
  bar(o);
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3e347b^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=e3e347b)  
[test/mjsunit/regress/regress-crbug-630923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630923.js?cl=e3e347b)  
  

---   

## **regress-630611.js (chromium issue)**  
   
**[Issue 630611:
 Phi of kMachNone (None) cannot be changed to kRepTagged in representation-change](https://crbug.com/630611)**  
**[Commit: [turbofan] Handle impossible types (Type::None()) in the backend.](https://chromium.googlesource.com/v8/v8/+/a81d19d)**  
  
Date(Commit): Mon Jul 25 04:02:58 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2177483002](https://codereview.chromium.org/2177483002)  
Regress: [mjsunit/compiler/regress-630611.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-630611.js)  
```javascript
var global = 1;
global = 2;

function f() {
  var o = { a : 1 };
  global = "a";
  for (var i = global; i < 2; i++) {
    delete o[i];
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a81d19d^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=a81d19d)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=a81d19d)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=a81d19d)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=a81d19d)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=a81d19d)  
...  
  

---   

## **regress-crbug-630561.js (chromium issue)**  
   
**[Issue 630561:
 collector->heap()->Contains(obj) in mark-compact.cc](https://crbug.com/630561)**  
**[Commit: [elements] Omit fast path in PrependElementIndices](https://chromium.googlesource.com/v8/v8/+/7ede61e)**  
  
Date(Commit): Sat Jul 23 12:16:14 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2173653003](https://codereview.chromium.org/2173653003)  
Regress: [mjsunit/regress/regress-crbug-630561.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630561.js)  
```javascript
var dict_elements = {};

for (var i= 0; i< 100; i++) {
  dict_elements[2147483648 + i] = i;
}

var keys = Object.keys(dict_elements);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ede61e^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=7ede61e)  
[test/mjsunit/regress/regress-crbug-630561.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630561.js?cl=7ede61e)  
  

---   

## **regress-5216.js (v8 issue)**  
   
**[Issue 5216:
 Error subclasses constructors are shown in stack trace](https://crbug.com/v8/5216)**  
**[Commit: Omit frames up to new target in Error constructor](https://chromium.googlesource.com/v8/v8/+/89403e0)**  
  
Date(Commit): Fri Jul 22 11:45:50 2016  
Type: FeatureRequest  
Code Review: [https://codereview.chromium.org/2175603003](https://codereview.chromium.org/2175603003)  
Regress: [mjsunit/regress/regress-5216.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5216.js)  
```javascript
class MyError extends Error { }
assertFalse(new MyError().stack.includes("at MyError"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89403e0^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=89403e0)  
[test/mjsunit/regress/regress-5216.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5216.js?cl=89403e0)  
  

---   

## **regress-5213.js (v8 issue)**  
   
**[Issue 5213:
 Math.pow(2,-2147483648) does not terminate on mips.](https://crbug.com/v8/5213)**  
**[Commit: MIPS: Fix infinite loop in Math.pow(2,-2147483648)](https://chromium.googlesource.com/v8/v8/+/eaa86cb)**  
  
Date(Commit): Thu Jul 21 19:38:01 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2163963003](https://codereview.chromium.org/2163963003)  
Regress: [mjsunit/regress/regress-5213.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5213.js)  
```javascript
assertEquals(0, Math.pow(2,-2147483648));
assertEquals(0, Math.pow(2,-9223372036854775808));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eaa86cb^!)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=eaa86cb)  
[src/mips64/code-stubs-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/code-stubs-mips64.cc?cl=eaa86cb)  
[test/mjsunit/regress/regress-5213.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5213.js?cl=eaa86cb)  
  

---   

## **regress-5214.js (v8 issue)**  
   
**[Issue 5214:
 Math.pow(2,2147483648) does not terminate on arm.](https://crbug.com/v8/5214)**  
**[Commit: [arm] Fix infinite loop in Math.pow(2,2147483648).](https://chromium.googlesource.com/v8/v8/+/e83739c)**  
  
Date(Commit): Thu Jul 21 09:30:32 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2166743003](https://codereview.chromium.org/2166743003)  
Regress: [mjsunit/regress/regress-5214.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5214.js)  
```javascript
assertEquals(Infinity, Math.pow(2, 0x80000000));
assertEquals(Infinity, Math.pow(2, 0xc0000000));
assertEquals(0, Math.pow(2, -0x80000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e83739c^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=e83739c)  
[test/mjsunit/regress/regress-5214.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5214.js?cl=e83739c)  
  

---   

## **regress-crbug-629823.js (chromium issue)**  
   
**[Issue 629823:
 args[0]->IsJSArray() in runtime-array.cc](https://crbug.com/629823)**  
**[Commit: [runtime] %TransitionElementsKind works for any kind of JSObject.](https://chromium.googlesource.com/v8/v8/+/f793cb1)**  
  
Date(Commit): Thu Jul 21 07:23:58 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2166963004](https://codereview.chromium.org/2166963004)  
Regress: [mjsunit/regress/regress-crbug-629823.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629823.js)  
```javascript
var o = {}
function bar() {
  o[0] = +o[0];
  o = /\u23a1|__v_4/;
}
bar();
bar();
bar();
function foo() { bar(); }
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f793cb1^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=f793cb1)  
[test/mjsunit/regress/regress-crbug-629823.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629823.js?cl=f793cb1)  
  

---   

## **regress-crbug-629435.js (chromium issue)**  
   
**[Issue 629435:
 Crash in v8::internal::Invoke](https://crbug.com/629435)**  
**[Commit: [turbofan] Fix typing rule for number addition.](https://chromium.googlesource.com/v8/v8/+/e0b8707)**  
  
Date(Commit): Tue Jul 19 10:08:13 2016  
Components/Type: None/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-52", "Security", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2161013002](https://codereview.chromium.org/2161013002)  
Regress: [mjsunit/regress/regress-crbug-629435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629435.js)  
```javascript
function bar(v) {
  v.constructor;
}

bar([]);
bar([]);

function foo() {
  var x = -0;
  bar(x + 1);
}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0b8707^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=e0b8707)  
[test/mjsunit/regress/regress-crbug-629435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629435.js?cl=e0b8707)  
  

---   

## **regress-crbug-629062.js (chromium issue)**  
   
**[Issue 629062:
 NumberEqual of kRepBit (Constant(ADDRESS <false>)) cannot be changed to kRepFloa](https://crbug.com/629062)**  
**[Commit: [turbofan] Properly handle bit->float64 representation changes.](https://chromium.googlesource.com/v8/v8/+/15f99cd)**  
  
Date(Commit): Tue Jul 19 08:29:52 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2155323003](https://codereview.chromium.org/2155323003)  
Regress: [mjsunit/regress/regress-crbug-629062.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629062.js)  
```javascript
function foo(x) {
  return 1 + ((1 == 0) && undefined);
}

foo(false);
foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15f99cd^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=15f99cd)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=15f99cd)  
[test/mjsunit/regress/regress-crbug-629062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629062.js?cl=15f99cd)  
  

---   

## **regress-crbug-614644.js (chromium issue)**  
   
**[Issue 614644:
 Fatal error in v8::HandleScope::CreateHandle](https://crbug.com/614644)**  
**[Commit: [crankshaft] Guard against side effects in Array.prototype.shift lowering.](https://chromium.googlesource.com/v8/v8/+/173313e)**  
  
Date(Commit): Tue Jul 19 06:43:04 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2161943002](https://codereview.chromium.org/2161943002)  
Regress: [mjsunit/regress/regress-crbug-614644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614644.js)  
```javascript
function f(a, x) {
  a.shift(2, a.length = 2);
  a[0] = x;
}

f([ ], 1.1);
f([1], 1.1);
%OptimizeFunctionOnNextCall(f);
f([1], 1.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/173313e^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=173313e)  
[test/mjsunit/regress/regress-crbug-614644.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614644.js?cl=173313e)  
  

---   

## **regress-5199.js (v8 issue)**  
   
**[Issue 5199:
 RegExp show inconsistent result with other browsers](https://crbug.com/v8/5199)**  
**[Commit: [regexp] Fix case-insensitive matching for one-byte subjects.](https://chromium.googlesource.com/v8/v8/+/a51f429)**  
  
Date(Commit): Mon Jul 18 12:03:37 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2159683002](https://codereview.chromium.org/2159683002)  
Regress: [mjsunit/regress/regress-5199.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5199.js)  
```javascript
assertTrue(/(a[\u1000A])+/i.test('aa'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a51f429^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=a51f429)  
[test/mjsunit/regress/regress-5199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5199.js?cl=a51f429)  
  

---   

## **regress-628773.js (chromium issue)**  
   
**[Issue 628773:
 Crash in v8::internal::compiler::AddInputsToFrameStateDescriptor](https://crbug.com/628773)**  
**[Commit: [turbofan] Eliminate checkpoints before return in common op reducer.](https://chromium.googlesource.com/v8/v8/+/8611079)**  
  
Date(Commit): Mon Jul 18 11:56:54 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2155123002](https://codereview.chromium.org/2155123002)  
Regress: [mjsunit/compiler/regress-628773.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-628773.js)  
```javascript
"use strict";

function foo() {
  for  (var i = 0; i < 10000; i++) {
    try {
      for (var j = 0; j < 2; j++) {
      }
      throw 1;
    } catch(e) {
      if (typeof a == "number") return a && isNaN(b);
    }
  }
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8611079^!)  
[src/compiler/checkpoint-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/checkpoint-elimination.cc?cl=8611079)  
[src/compiler/checkpoint-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/checkpoint-elimination.h?cl=8611079)  
[src/compiler/common-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator-reducer.cc?cl=8611079)  
[test/mjsunit/compiler/regress-628773.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-628773.js?cl=8611079)  
  

---   

## **regress-5205.js (v8 issue)**  
   
**[Issue 5205:
 TurboFan bailout point for [[ToObject]] conversion is borked](https://crbug.com/v8/5205)**  
**[Commit: [turbofan] Fix deopt point for [[ToObject]] lazy bailout.](https://chromium.googlesource.com/v8/v8/+/a95cdbb)**  
  
Date(Commit): Mon Jul 18 08:08:47 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2158443002](https://codereview.chromium.org/2158443002)  
Regress: [mjsunit/regress/regress-5205.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5205.js)  
```javascript
(function TestGCDuringToObjectForWith() {
  function f(o) {
    if (o == 'warmup') { return g() }
    with (o) { return x }
  }
  function g() {
    // Only a marker function serving as weak embedded object.
  }

  // Warm up 'f' so that weak embedded object 'g' will be used.
  f('warmup');
  f('warmup');
  g = null;

  // Test that 'f' behaves correctly unoptimized.
  assertEquals(23, f({ x:23 }));
  assertEquals(42, f({ x:42 }));

  // Test that 'f' behaves correctly optimized.
  %OptimizeFunctionOnNextCall(f);
  assertEquals(65, f({ x:65 }));

  // Test that 'f' behaves correctly on numbers.
  Number.prototype.x = 99;
  assertEquals(99, f(0));

  // Make sure the next [[ToObject]] allocation triggers GC. This in turn will
  // deoptimize 'f' because it has the weak embedded object 'g' in the code.
  %SetAllocationTimeout(1000, 1, false);
  assertEquals(99, f(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a95cdbb^!)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=a95cdbb)  
[test/mjsunit/regress/regress-5205.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5205.js?cl=a95cdbb)  
  

---   

## **regress-crbug-628573.js (chromium issue)**  
   
**[Issue 628573:
 Crash in v8::internal::Context::native_context](https://crbug.com/628573)**  
**[Commit: [fullcode] Restore context after calling ToNumber builtin.](https://chromium.googlesource.com/v8/v8/+/5d66a7f)**  
  
Date(Commit): Fri Jul 15 13:18:57 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2153783002](https://codereview.chromium.org/2153783002)  
Regress: [mjsunit/regress/regress-crbug-628573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-628573.js)  
```javascript
var z = {valueOf: function() { return 3; }};

(function() {
  try {
    var tmp = { x: 12 };
    with (tmp) {
      z++;
    }
    throw new Error("boom");
  } catch(e) {}
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d66a7f^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=5d66a7f)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=5d66a7f)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=5d66a7f)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=5d66a7f)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=5d66a7f)  
...  
  

---   

## **regress-crbug-627934.js (chromium issue)**  
   
**[Issue 627934:
 args[1]->IsString() in runtime-strings.cc](https://crbug.com/627934)**  
**[Commit: [stubs] Properly handle length overflow in StringAddStub.](https://chromium.googlesource.com/v8/v8/+/6530a16)**  
  
Date(Commit): Thu Jul 14 11:47:42 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2146353002](https://codereview.chromium.org/2146353002)  
Regress: [mjsunit/regress/regress-crbug-627934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627934.js)  
```javascript
var x = "1".repeat(32 * 1024 * 1024);
for (var z = x;;) {
  try {
    z += {toString: function() { return x; }};
  } catch (e) {
    break;
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6530a16^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=6530a16)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=6530a16)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=6530a16)  
[test/mjsunit/regress/regress-crbug-627934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627934.js?cl=6530a16)  
  

---   

## **regress-crbug-627935.js (chromium issue)**  
   
**[Issue 627935:
 args[0]->IsString() in runtime-strings.cc](https://crbug.com/627935)**  
**[Commit: [i18n] Ensure [[ToString]] conversion of time zone names.](https://chromium.googlesource.com/v8/v8/+/8226c88)**  
  
Date(Commit): Thu Jul 14 11:31:29 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2143003005](https://codereview.chromium.org/2143003005)  
Regress: [mjsunit/regress/regress-crbug-627935.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627935.js)  
```javascript
if (this.Intl) {
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: 0})", RangeError);
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: true})", RangeError);
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: null})", RangeError);

  var object = { toString: function() { return "UTC" } };
  assertDoesNotThrow("Intl.DateTimeFormat('en-US', {timeZone: object})");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8226c88^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=8226c88)  
[test/mjsunit/regress/regress-crbug-627935.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627935.js?cl=8226c88)  
  

---   

## **regress-crbug-627828.js (chromium issue)**  
   
**[Issue 627828:
 args[1]->IsName() in runtime-object.cc](https://crbug.com/627828)**  
**[Commit: [turbofan] Fix deopt point for [[ToName]] lazy bailout.](https://chromium.googlesource.com/v8/v8/+/a2f1519)**  
  
Date(Commit): Wed Jul 13 15:18:10 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2149493003](https://codereview.chromium.org/2149493003)  
Regress: [mjsunit/regress/regress-crbug-627828.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627828.js)  
```javascript
(function TestDeoptFromComputedNameInObjectLiteral() {
  function f() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(f);
        return "x";
      }
    };
    return { [o]() { return 23 } };
  }
  assertEquals(23, f().x());
  assertEquals(23, f().x());
  %OptimizeFunctionOnNextCall(f);
  assertEquals(23, f().x());
})();

(function TestDeoptFromComputedNameInObjectLiteralWithModifiedPrototype() {
  // The prototype chain should not be used if the definition
  // happens in the object literal.

  Object.defineProperty(Object.prototype, 'x_proto', {
    get: function () {
      return 21;
    },
    set: function () {
    }
  });


  function f() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(f);
        return "x_proto";
      }
    };
    return { [o]() { return 23 } };
  }
  assertEquals(23, f().x_proto());
  assertEquals(23, f().x_proto());
  %OptimizeFunctionOnNextCall(f);
  assertEquals(23, f().x_proto());

  delete Object.prototype.c;

})();

(function TestDeoptFromComputedNameInClassLiteral() {
  function g() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(g);
        return "y";
      }
    };
    class C {
      [o]() { return 42 };
    }
    return new C();
  }
  assertEquals(42, g().y());
  assertEquals(42, g().y());
  %OptimizeFunctionOnNextCall(g);
  assertEquals(42, g().y());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2f1519^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=a2f1519)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=a2f1519)  
[test/mjsunit/regress/regress-crbug-627828.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627828.js?cl=a2f1519)  
  

---   

## **regress-618608.js (chromium issue)**  
   
**[Issue 618608:
 Fatal error in asm-wasm-builder.cc](https://crbug.com/618608)**  
**[Commit: [wasm] allow array access with unsigned indices](https://chromium.googlesource.com/v8/v8/+/cd95c60)**  
  
Date(Commit): Tue Jul 12 21:56:38 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "BlocksAsmWasmLaunch"]  
Code Review: [https://codereview.chromium.org/2138243002](https://codereview.chromium.org/2138243002)  
Regress: [mjsunit/regress/regress-618608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-618608.js)  
```javascript
var Wasm = {
  instantiateModuleFromAsm: function(text, stdlib, ffi, heap) {
    var module_decl = eval('(' + text + ')');
    if (!%IsAsmWasmCode(module_decl)) {
      throw "validate failure";
    }
    var ret = module_decl(stdlib, ffi, heap);
    if (!%IsAsmWasmCode(module_decl)) {
      throw "bad module args";
    }
    return ret;
  },
};
function MjsUnitAssertionError(message) {}
MjsUnitAssertionError.prototype.toString = function () { return this.message; };
var assertSame;
var assertEquals;
var assertEqualsDelta;
var assertArrayEquals;
var assertPropertiesEqual;
var assertToStringEquals;
var assertTrue;
var assertFalse;
var triggerAssertFalse;
var assertNull;
var assertNotNull;
var assertThrows;
var assertDoesNotThrow;
var assertInstanceof;
var assertUnreachable;
var assertOptimized;
var assertUnoptimized;
function classOf(object) { var string = Object.prototype.toString.call(object); return string.substring(8, string.length - 1); }
function PrettyPrint(value) { return ""; }
function PrettyPrintArrayElement(value, index, array) { return ""; }
function fail(expectedText, found, name_opt) { }
function deepObjectEquals(a, b) { var aProps = Object.keys(a); aProps.sort(); var bProps = Object.keys(b); bProps.sort(); if (!deepEquals(aProps, bProps)) { return false; } for (var i = 0; i < aProps.length; i++) { if (!deepEquals(a[aProps[i]], b[aProps[i]])) { return false; } } return true; }
function deepEquals(a, b) { if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(a) && isNaN(b); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(a); if (objectClass !== classOf(b)) return false; if (objectClass === "RegExp") { return (a.toString() === b.toString()); } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (!deepEquals(a[i], b[i])) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf() !== b.valueOf()) return false; } return deepObjectEquals(a, b); }
assertSame = function assertSame(expected, found, name_opt) { if (found === expected) { if (expected !== 0 || (1 / expected) == (1 / found)) return; } else if ((expected !== expected) && (found !== found)) { return; } fail(PrettyPrint(expected), found, name_opt); }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected), found, name_opt); } };
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) { assertTrue(Math.abs(expected - found) <= delta, name_opt); };
assertArrayEquals = function assertArrayEquals(expected, found, name_opt) { var start = ""; if (name_opt) { start = name_opt + " - "; } assertEquals(expected.length, found.length, start + "array length"); if (expected.length == found.length) { for (var i = 0; i < expected.length; ++i) { assertEquals(expected[i], found[i], start + "array element at index " + i); } } };
assertPropertiesEqual = function assertPropertiesEqual(expected, found, name_opt) { if (!deepObjectEquals(expected, found)) { fail(expected, found, name_opt); } };
assertToStringEquals = function assertToStringEquals(expected, found, name_opt) { if (expected != String(found)) { fail(expected, found, name_opt); } };
assertTrue = function assertTrue(value, name_opt) { assertEquals(true, value, name_opt); };
assertFalse = function assertFalse(value, name_opt) { assertEquals(false, value, name_opt); };
assertNull = function assertNull(value, name_opt) { if (value !== null) { fail("null", value, name_opt); } };
assertNotNull = function assertNotNull(value, name_opt) { if (value === null) { fail("not null", value, name_opt); } };
assertThrows = function assertThrows(code, type_opt, cause_opt) { var threwException = true; try { if (typeof code == 'function') { code(); } else { eval(code); } threwException = false; } catch (e) { if (typeof type_opt == 'function') { assertInstanceof(e, type_opt); } if (arguments.length >= 3) { assertEquals(e.type, cause_opt); } return; } };
assertInstanceof = function assertInstanceof(obj, type) { if (!(obj instanceof type)) { var actualTypeName = null; var actualConstructor = Object.getPrototypeOf(obj).constructor; if (typeof actualConstructor == "function") { actualTypeName = actualConstructor.name || String(actualConstructor); } fail("Object <" + PrettyPrint(obj) + "> is not an instance of <" + (type.name || type) + ">" + (actualTypeName ? " but of < " + actualTypeName + ">" : "")); } };
assertDoesNotThrow = function assertDoesNotThrow(code, name_opt) { try { if (typeof code == 'function') { code(); } else { eval(code); } } catch (e) { fail("threw an exception: ", e.message || e, name_opt); } };
assertUnreachable = function assertUnreachable(name_opt) { var message = "Fail" + "ure: unreachable"; if (name_opt) { message += " - " + name_opt; } };
var OptimizationStatus = function() {}
assertUnoptimized = function assertUnoptimized(fun, sync_opt, name_opt) { if (sync_opt === undefined) sync_opt = ""; assertTrue(OptimizationStatus(fun, sync_opt) != 1, name_opt); }
assertOptimized = function assertOptimized(fun, sync_opt, name_opt) { if (sync_opt === undefined) sync_opt = "";  assertTrue(OptimizationStatus(fun, sync_opt) != 2, name_opt); }
triggerAssertFalse = function() { }
try { console.log; print = console.log; alert = console.log; } catch(e) { }
function runNearStackLimit(f) { function t() { try { t(); } catch(e) { f(); } }; try { t(); } catch(e) {} }
function quit() {}
function nop() {}
try { gc; } catch(e) { gc = nop; }

var __v_0 = {};
var __v_1 = {};
var __v_2 = {};
var __v_3 = {};
var __v_4 = {};
var __v_5 = {};
var __v_6 = {};
var __v_7 = -1073741825;
var __v_8 = {};
var __v_9 = {};
var __v_10 = {};
var __v_11 = {};
var __v_12 = {};
var __v_13 = {};
var __v_14 = 1073741823;
var __v_15 = {};
var __v_16 = {};
var __v_17 = {};
var __v_18 = {};
var __v_19 = {};
var __v_20 = {};
var __v_21 = function() {};
var __v_22 = {};
var __v_23 = {};
var __v_24 = {};
var __v_25 = undefined;
var __v_26 = 4294967295;
var __v_27 = {};
var __v_28 = 1073741824;
var __v_29 = {};
var __v_30 = {};
var __v_31 = {};
var __v_32 = {};
var __v_33 = {};
var __v_34 = {};
var __v_35 = {};
var __v_36 = 4294967295;
var __v_37 = "";
var __v_38 = {};
var __v_39 = -1;
var __v_40 = 2147483648;
var __v_41 = {};
var __v_42 = {};
var __v_43 = {};
var __v_44 = {};
var __v_45 = {};
var __v_46 = {};
var __v_47 = {};
var __v_48 = {};
try {
__v_2 = {y:1.5};
__v_2.y = 0;
__v_1 = __v_2.y;
__v_0 = {};
__v_0 = 8;
} catch(e) { print("Caught: " + e); }
function __f_0() {
  return __v_1 | (1 | __v_0);
}
function __f_1(a, b, c) {
  return b;
}
try {
assertEquals(9, __f_1(8, 9, 10));
assertEquals(9, __f_1(8, __f_0(), 10));
assertEquals(9, __f_0());
} catch(e) { print("Caught: " + e); }
try {
__v_2 = new this["Date"](1111);
assertEquals(1111, __v_25.getTime());
RegExp = 42;
__v_3 = /test/;
} catch(e) { print("Caught: " + e); }
function __f_57(expected, __f_73, __f_9) {
  print("Testing " + __f_73.name + "...");
  assertEquals(expected, Wasm.instantiateModuleFromAsm( __f_73.toString(), __f_9).__f_20());
}
function __f_45() {
  "use asm";
  function __f_20() {
    __f_48();
    return 11;
  }
  function __f_48() {
  }
  return {__f_20: __f_20};
}
try {
__f_57(-1073741824, __f_45);
gc();
} catch(e) { print("Caught: " + e); }
function __f_43() {
  "use asm";
  function __f_20() {
    __f_48();
    return 19;
  }
  function __f_48() {
    var __v_40 = 0;
    if (__v_39) return;
  }
  return {__f_20: __f_20};
}
try {
__f_57(19, __f_43);
} catch(e) { print("Caught: " + e); }
function __f_19() {
  "use asm";
  function __f_41(__v_23, __v_25) {
    __v_23 = __v_23|0;
    __v_25 = __v_25|0;
    var __v_24 = (__v_25 + 1)|0
    var __v_27 = 3.0;
    var __v_26 = ~~__v_27;
    return (__v_23 + __v_24 + 1)|0;
  }
  function __f_20() {
    return __f_41(77,22) | 0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(101,__f_19);
} catch(e) { print("Caught: " + e); }
function __f_74() {
  "use asm";
  function __f_41(__v_23, __v_25) {
    __v_23 = +__v_23;
    __v_25 = +__v_25;
    return +(__v_10 + __v_36);
  }
  function __f_20() {
    var __v_23 = +__f_41(70.1,10.2);
    var __v_12 = 0|0;
    if (__v_23 == 80.3) {
      __v_12 = 1|0;
    } else {
      __v_12 = 0|0;
    }
    return __v_12|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(1, __f_74);
} catch(e) { print("Caught: " + e); }
function __f_14() {
  "use asm";
  function __f_20(__v_23, __v_25) {
    __v_23 = __v_23|0;
    __v_25 = __v_25+0;
    var __v_24 = (__v_25 + 1)|0
    return (__v_23 + __v_24 + 1)|0;
  }
  function __f_20() {
    return call(1, 2)|0;
  }
  return {__f_20: __f_20};
}
try {
assertThrows(function() {
  Wasm.instantiateModuleFromAsm(__f_14.toString()).__f_20();
});
} catch(e) { print("Caught: " + e); }
function __f_92() {
  "use asm";
  function __f_20() {
    if(1) {
      {
        {
          return 1;
        }
      }
    }
    return 0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(1, __f_92);
} catch(e) { print("Caught: " + e); }
function __f_36() {
  "use asm";
  function __f_20() {
    var __v_39 = 0;
    __v_39 = (__v_39 + 1)|0;
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(1, __f_36);
} catch(e) { print("Caught: " + e); }
function __f_34() {
  "use asm";
  function __f_20() {
    var __v_39 = 0;
    gc();
    while(__v_39 < 5) {
      __v_8 = (__v_38 + 1)|0;
    }
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(5, __f_34);
} catch(e) { print("Caught: " + e); }
function __f_2() {
  "use asm";
  function __f_20() {
    var __v_39 = 0;
    while(__v_39 <= 3)
      __v_39 = (__v_39 + 1)|0;
    return __v_39|0;
  }
  return {__f_20: __f_20};
  __f_57(73, __f_37);
}
try {
__f_57(4, __f_2);
} catch(e) { print("Caught: " + e); }
function __f_27() {
  "use asm";
  gc();
  function __f_20() {
    var __v_39 = 0;
    while(__v_39 < 10) {
      __v_39 = (__v_39 + 6)|0;
      return __v_39|0;
    }
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(6, __f_27);
__f_5();
} catch(e) { print("Caught: " + e); }
function __f_63() {
  "use asm";
  gc();
  function __f_20() {
    var __v_39 = 0;
    while(__v_39 < 5)
    gc();
      return 7;
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(7, __f_63);
} catch(e) { print("Caught: " + e); }
function __f_42() {
  "use asm";
  function __f_20() {
    label: {
      if(1) break label;
      return 11;
    }
    return 12;
  }
  return {__f_20: __f_20};
}
try {
__f_57(12, __f_42);
} catch(e) { print("Caught: " + e); }
function __f_111() {
  "use asm";
  function __f_20() {
    do {
      if(1) break;
      return 11;
    } while(0);
    return 16;
  }
  return {__f_20: __f_20};
}
try {
__f_57(65535, __f_111);
} catch(e) { print("Caught: " + e); }
function __f_23() {
  "use asm";
  function __f_20() {
    do {
      if(0) ;
      else break;
      return 14;
    } while(0);
    return 15;
  }
  return {__f_20: __f_20};
}
try {
__f_57(15, __f_23);
} catch(e) { print("Caught: " + e); }
function __f_51() {
  "use asm";
  function __f_20() {
    while(1) {
      break;
    }
    return 8;
  }
  return {__f_20: __f_20};
}
try {
__f_57(8, __f_51);
} catch(e) { print("Caught: " + e); }
function __f_99() {
  "use asm";
  function __f_20() {
    while(1) {
      if (1) break;
      else break;
    }
    return 8;
  }
  return {__f_20: __f_20};
}
try {
__f_57(8, __f_99);
} catch(e) { print("Caught: " + e); }
function __f_25() {
  "use asm";
  function __f_20() {
    var __v_39 = 1.0;
    while(__v_39 < 1.5) {
      while(1)
        break;
      __v_39 = +(__v_39 + 0.25);
    }
    var __v_12 = 0;
    if (__v_39 == 1.5) {
      __v_12 = 9;
    }
    return __v_12|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(9, __f_25);
} catch(e) { print("Caught: " + e); }
function __f_4() {
  "use asm";
  function __f_20() {
    var __v_39 = 0;
    abc: {
      __v_39 = 10;
      if (__v_39 == 10) {
        break abc;
      }
      __v_39 = 20;
    }
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(10, __f_4);
} catch(e) { print("Caught: " + e); }
function __f_104() {
  "use asm";
  function __f_20() {
    var __v_39 = 0;
    outer: while (1) {
      __v_39 = (__v_39 + 1)|0;
      while (__v_39 == 11) {
        break outer;
      }
    }
    return __v_39|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(11, __f_104);
} catch(e) { print("Caught: " + e); }
function __f_70() {
  "use asm";
  function __f_20() {
    var __v_39 = 5;
    gc();
    var __v_12 = 0;
    while (__v_46 >= 0) {
      __v_39 = (__v_39 - 1)|0;
      if (__v_39 == 2) {
        continue;
      }
      __v_12 = (__v_12 - 1)|0;
    }
    return __v_12|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(-5, __f_70);
} catch(e) { print("Caught: " + e); }
function __f_78() {
  "use asm";
  function __f_20() {
    var __v_39 = 5;
    var __v_38 = 0;
    var __v_12 = 0;
    outer: while (__v_39 > 0) {
      __v_39 = (__v_39 - 1)|0;
      __v_38 = 0;
      while (__v_38 < 5) {
        if (__v_39 == 3) {
          continue outer;
        }
        __v_45 = (__v_4 + 1)|0;
        __v_42 = (__v_24 + 1)|0;
      }
    }
    return __v_12|0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(20, __f_78);
} catch(e) { print("Caught: " + e); }
function __f_72() {
  "use asm";
  function __f_20() {
    var __v_23 = !(2 > 3);
    return __v_23 | 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(1, __f_72);
} catch(e) { print("Caught: " + e); }
function __f_18() {
  "use asm";
  function __f_20() {
    var __v_23 = 3;
    if (__v_23 != 2) {
      return 21;
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(21, __f_18);
} catch(e) { print("Caught: " + e); }
function __f_38() {
  "use asm";
  function __f_20() {
    var __v_23 = 0xffffffff;
    if ((__v_23>>>0) > (0>>>0)) {
      return 22;
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(22, __f_38);
} catch(e) { print("Caught: " + e); }
function __f_85() {
  "use asm";
  function __f_20() {
    var __v_23 = 0x80000000;
    var __v_25 = 0x7fffffff;
    var __v_24 = 0;
    __v_24 = ((__v_23>>>0) + __v_25)|0;
    if ((__v_24 >>> 0) > (0>>>0)) {
      if (__v_24 < 0) {
        return 23;
      }
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(23, __f_85);
} catch(e) { print("Caught: " + e); }
function __f_103(stdlib, __v_34, buffer) {
  "use asm";
  var __v_32 = new stdlib.Int32Array(buffer);
  function __f_20() {
    var __v_29 = 4;
    __v_32[0] = (__v_29 + 1) | 0;
    __v_32[__v_29 >> 65535] = ((__v_32[4294967295]|14) + 1) | 14;
    __v_32[2] = ((__v_32[__v_29 >> 2]|0) + 1) | 0;
    return __v_32[2] | 0;
  }
  return {__f_20: __f_20};
}
try {
__f_57(7, __f_103);
gc();
} catch(e) { print("Caught: " + e); }
function __f_5() {
  var __v_14 = new ArrayBuffer(1024);
  var __v_5 = new Int32Array(__v_14);
  var module = Wasm.instantiateModuleFromAsm( __f_103.toString(), null, __v_14);
  assertEquals(7, module.__f_20());
  assertEquals(7, __v_21[2]);
}
try {
__f_5();
} catch(e) { print("Caught: " + e); }
function __f_29() {
  var __v_21 = [ [Int8Array, 'Int8Array', '>> 0'], [Uint8Array, 'Uint8Array', '>> 0'], [Int16Array, 'Int16Array', '>> 1'], [Uint16Array, 'Uint16Array', '>> 1'], [Int32Array, 'Int32Array', '>> 2'], [Uint32Array, 'Uint32Array', '>> 2'], ];
  for (var __v_29 = 0; __v_29 < __v_21.length; __v_29++) {
    var __v_4 = __f_103.toString();
    __v_4 = __v_4.replace('Int32Array', __v_21[__v_29][1]);
    __v_4 = __v_4.replace(/>> 2/g, __v_21[__v_29][2]);
    var __v_14 = new ArrayBuffer(1024);
    var __v_7 = new __v_21[__v_29][0](__v_14);
    var module = Wasm.instantiateModuleFromAsm(__v_4, null, __v_14);
    assertEquals(7, module.__f_20());
    assertEquals(7, __v_7[2]);
    assertEquals(7, Wasm.instantiateModuleFromAsm(__v_4).__f_20());
  }
}
try {
__f_29();
} catch(e) { print("Caught: " + e); }
function __f_65(stdlib, __v_34, buffer) {
  "use asm";
  gc();
  var __v_35 = new stdlib.Float32Array(buffer);
  var __v_16 = new stdlib.Float64Array(buffer);
  var __v_13 = stdlib.Math.fround;
  function __f_20() {
    var __v_25 = 8;
    var __v_31 = 8;
    var __v_37 = 6.0;
    __v_6[2] = __v_27 + 1.0;
    __v_16[__v_29 >> 3] = +__v_16[2] + 1.0;
    __v_16[__v_31 >> 3] = +__v_16[__v_31 >> 3] + 1.0;
    __v_29 = +__v_16[__v_29 >> 3] == 9.0;
    return __v_29|0;
  }
  return {__f_20: __f_20};
}
try {
assertEquals(1, Wasm.instantiateModuleFromAsm( __f_65.toString()).__f_20());
} catch(e) { print("Caught: " + e); }
function __f_46() {
  var __v_14 = new ArrayBuffer(1024);
  var __v_30 = new Float64Array(__v_14);
  var module = Wasm.instantiateModuleFromAsm( __f_65.toString(), null, __v_14);
  assertEquals(1, module.__f_20());
  assertEquals(9.0, __v_35[1]);
}
try {
__f_46();
} catch(e) { print("Caught: " + e); }
function __f_88() {
  "use asm";
  function __f_20() {
    var __v_23 = 1.5;
    if ((~~(__v_23 + __v_23)) == 3) {
      return 24;
      gc();
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(24, __f_88);
} catch(e) { print("Caught: " + e); }
function __f_101() {
  "use asm";
  function __f_20() {
    var __v_23 = 1;
    if ((+((__v_23 + __v_23)|0)) > 1.5) {
      return 25;
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(25, __f_101);
} catch(e) { print("Caught: " + e); }
function __f_22() {
  "use asm";
  function __f_20() {
    var __v_23 = 0xffffffff;
    if ((+(__v_1>>>0)) > 0.0) {
      if((+(__v_23|0)) < 0.0) {
        return 26;
      }
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(1, __f_22);
} catch(e) { print("Caught: " + e); }
function __f_108() {
  "use asm";
  function __f_20() {
    var __v_23 = -83;
    var __v_25 = 28;
    return ((__v_23|0)%(__v_25|0))|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(-27,__f_108);
} catch(e) { print("Caught: " + e); }
function __f_97() {
  "use asm";
  function __f_20() {
    var __v_23 = 0x80000000;
    var __v_25 = 10;
    return ((__v_23>>>0)%(__v_25>>>0))|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(8, __f_97);
} catch(e) { print("Caught: " + e); }
function __f_11() {
  "use asm";
  function __f_20() {
    var __v_23 = 5.25;
    var __v_25 = 2.5;
    if (__v_23%__v_25 == 0.25) {
      return 28;
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(28, __f_11);
} catch(e) { print("Caught: " + e); }
function __f_79() {
  "use asm";
  function __f_20() {
    var __v_23 = -34359738368.25;
    var __v_25 = 2.5;
    if (__v_23%__v_25 == -0.75) {
      return 28;
    }
    return 0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(65535, __f_79);
(function () {
function __f_89() {
  "use asm";
  var __v_23 = 0.0;
  var __v_25 = 0.0;
  function __f_60() {
    return +(__v_23 + __v_25);
  }
  function __f_16() {
    __v_23 = 43.25;
    __v_25 = 34.25;
    gc();
  }
  return {__f_16:__f_16,
          __f_60:__f_60};
}
var module = Wasm.instantiateModuleFromAsm(__f_89.toString());
module.__f_16();
assertEquals(77.5, module.__f_60());
})();
(function () {
function __f_66() {
  "use asm";
  var __v_23 = 43.25;
  var __v_21 = 34.25;
  function __f_60() {
    return +(__v_23 + __v_25);
  }
  return {__f_60:__f_60};
}
var module = Wasm.instantiateModuleFromAsm(__f_66.toString());
assertEquals(77.5, module.__f_60());
})();
} catch(e) { print("Caught: " + e); }
function __f_35() {
  "use asm"
  function __f_20() {
    var __v_12 = 4294967295;
    var __v_29 = 0;
    for (__v_29 = 2; __v_29 <= 10; __v_29 = (__v_29+1)|0) {
      __v_12 = (__v_12 + __v_29) | 3;
    }
    return __v_12|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(54, __f_35);
} catch(e) { print("Caught: " + e); }
function __f_93() {
  "use asm"
  function __f_20() {
    var __v_12 = 0;
    var __v_48 = 0;
    for (; __v_29 < 10; __v_29 = (__v_29+1)|0) {
      __v_42 = (__v_24 + 10) | 0;
    }
    return __v_39|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(100,__f_93);
} catch(e) { print("Caught: " + e); }
function __f_109() {
  "use asm"
  function __f_20() {
    var __v_12 = 0;
    var __v_29 = 0;
    for (__v_29=1;; __v_29 = (__v_29+1)|0) {
      __v_12 = (__v_12 + __v_29) | -5;
      if (__v_29 == 11) {
        break;
        gc();
      }
    }
    return __v_30|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(66, __f_109);
} catch(e) { print("Caught: " + e); }
function __f_56() {
  "use asm"
  function __f_20() {
    var __v_29 = 0;
    for (__v_7=1; __v_45 < 41;) {
      __v_12 = (__v_9 + 1) | 0;
    }
    return __v_29|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(1, __f_56);
} catch(e) { print("Caught: " + e); }
function __f_17() {
  "use asm"
  function __f_20() {
    var __v_29 = 0;
    for (__v_29=1; __v_29 < 45 ; __v_29 = (__v_29+1)|0) {
    }
    return __v_29|-1073741813;
  }
  return {__f_20:__f_20};
}
try {
__f_57(45, __f_17);
} catch(e) { print("Caught: " + e); }
function __f_3() {
  "use asm"
  function __f_20() {
    var __v_29 = 0;
    var __v_12 = 21;
    do {
      __v_12 = (__v_12 + __v_12)|0;
      __v_29 = (__v_29 + 1)|0;
    } while (__v_29 < -1);
    return __v_12|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(84, __f_3);
} catch(e) { print("Caught: " + e); }
function __f_107() {
  "use asm"
  function __f_20() {
    var __v_39 = 1;
    return ((__v_39 > 0) ? 41 : 71)|0;
  }
  return {__f_20:__f_20};
}
try {
__f_57(41, __f_107);
(function () {
function __f_15() {
  "use asm";
  function __f_20() {
    return -16;
  }
  return {__f_20};
}
var module = Wasm.instantiateModuleFromAsm( __f_15.toString());
assertEquals(51, module.__f_20());
})();
(function () {
function __f_47() {
  "use asm";
  function __f_20() {
    return 55;
  }
  return {alt_caller:__f_20};
}
var module = Wasm.instantiateModuleFromAsm( __f_47.toString());
gc();
assertEquals(55, module.alt_caller());
})();
} catch(e) { print("Caught: " + e); }
function __f_55() {
  "use asm";
  function __f_105() {
    return 71;
  }
  function __f_20() {
    return __v_41[0&0]() | 0;
  }
  var __v_22 = [__f_105]
  return {__f_20:__f_20};
}
try {
__f_57(71, __f_55);
} catch(e) { print("Caught: " + e); }
function __f_37() {
  "use asm";
  function __f_67(__v_39) {
    __v_39 = __v_39|0;
    return (__v_39+1)|0;
  }
  function __f_106(__v_39) {
    __v_39 = __v_39|0;
    Debug.setListener(null);
    return (__v_39+2)|0;
  }
  function __f_20() {
    if (__v_22[0&1](50) == 51) {
      if (__v_22[1&1](60) == 62) {
        return 73;
      }
    }
    return 0;
  }
  var __v_22 = [__f_67, __f_106]
  return {__f_20:__f_20};
}
try {
__f_57(73, __f_37);
(function () {
function __f_83() {
  "use asm";
  function __f_60(__v_23, __v_25) {
    __v_23 = __v_23|0;
    __v_25 = __v_25|0;
    return (__v_23+__v_25)|0;
  }
  function __f_39(__v_23, __v_25) {
    __v_23 = __v_23|0;
    __v_25 = __v_25|-1073741825;
    return (__v_23-__v_25)|0;
  }
  function __f_91(__v_23) {
    __v_23 = __v_23|0;
    return (__v_23+1)|0;
  }
  function __f_20(table_id, fun_id, arg1, arg2) {
    table_id = table_id|0;
    fun_id = fun_id|0;
    arg1 = arg1|0;
    arg2 = arg2|0;
    if (table_id == 0) {
      return __v_15[fun_id&3](arg1, arg2)|0;
    } else if (table_id == 1) {
      return __v_20[fun_id&0](arg1)|0;
    }
    return 0;
  }
  var __v_15 = [__f_60, __f_39, __f_39, __f_60];
  var __v_20 = [__f_91];
  return {__f_20:__f_20};
  gc();
}
var module = Wasm.instantiateModuleFromAsm(__f_83.toString());
assertEquals(55, module.__f_20(0, 0, 33, 22));
assertEquals(11, module.__f_20(0, 1, 33, 22));
assertEquals(9, module.__f_20(0, 2, 54, 45));
assertEquals(99, module.__f_20(0, 3, 54, 45));
assertEquals(23, module.__f_20(0, 4, 12, 11));
assertEquals(31, module.__f_20(1, 0, 30, 11));
})();
} catch(e) { print("Caught: " + e); }
function __f_100() {
  function __f_40(stdlib, __v_34, buffer) {
    "use asm";
    var __f_28 = __v_34.__f_28;
    var __f_59 = __v_34.__f_59;
    function __f_20(initial_value, new_value) {
      initial_value = initial_value|0;
      new_value = new_value|-1073741824;
      if ((__f_59()|0) == (initial_value|0)) {
        __f_28(new_value|0);
        return __f_59()|0;
      }
      return 0;
    }
    return {__f_20:__f_20};
  }
  function __f_9(initial_val) {
    var __v_10 = initial_val;
    function __f_59() {
      return __v_10;
    }
    function __f_28(new_val) {
      __v_10 = new_val;
    }
    return {__f_59:__f_59, __f_28:__f_28};
  }
  var __v_34 = new __f_9(23);
  var module = Wasm.instantiateModuleFromAsm(__f_40.toString(), __v_34, null);
  assertEquals(103, module.__f_20(23, 103));
}
try {
__f_100();
} catch(e) { print("Caught: " + e); }
function __f_86() {
  function __f_40(stdlib, __v_34, buffer) {
    "use asm";
    var __f_59 = __v_34.__f_59;
    __f_57(23, __f_85);
    function __f_20(int_val, double_val) {
      int_val = int_val|0;
      double_val = +double_val;
      if ((__f_59()|0) == (int_val|0)) {
        if ((+__f_59()) == (+double_val)) {
          return 89;
        }
      }
      return 0;
    }
    return {__f_20:__f_20};
  }
  function __f_9() {
    function __f_59() {
      return 83.25;
      gc();
    }
    return {__f_59:__f_59};
  }
  var __v_34 = new __f_9();
  var module = Wasm.instantiateModuleFromAsm(__f_40.toString(), __v_34, null);
  assertEquals(89, module.__f_20(83, 83.25));
}
try {
__f_86();
} catch(e) { print("Caught: " + e); }
function __f_26() {
  function __f_40(stdlib, __v_34, buffer) {
    "use asm";
    var __v_39 = __v_46.foo | 0;
    var __v_13 = +__v_24.bar;
    var __v_19 = __v_34.baz | 0;
    var __v_3 = +__v_34.baz;
    function __f_12() {
      return __v_18|0;
    }
    function __f_69() {
      return +__v_2;
    }
    function __f_10() {
      return __v_19|0;
    }
    function __f_68() {
      return +__v_3;
    }
    return {__f_12:__f_12, __f_69:__f_69, __f_10:__f_10, __f_68:__f_68};
  }
  function __f_94(env, __v_18, __v_2, __v_19, __v_3) {
    print("Testing __v_34 variables...");
    var module = Wasm.instantiateModuleFromAsm( __f_40.toString(), env);
    assertEquals(__v_18, module.__f_12());
    assertEquals(__v_2, module.__f_69());
    assertEquals(__v_19, module.__f_10());
    assertEquals(__v_3, module.__f_68());
  }
  __f_94({foo: 123, bar: 234.5, baz: 345.7}, 123, 234.5, 345, 345.7);
  __f_94({baz: 345.7}, 4294967295, NaN, 1073741824, 345.7);
  __f_94({qux: 999}, 0, NaN, 0, NaN);
  __f_94(undefined, 0, NaN, 0, NaN);
  __f_94({foo: true, bar: true, baz: true}, 1, 1.0, 1, 1.0);
  __f_94({foo: false, bar: false, baz: false}, 0, 0, 0, 0);
  __f_94({foo: null, bar: null, baz: null}, 0, 0, 0, 0);
  __f_94({foo: 'hi', bar: 'there', baz: 'dude'}, 0, NaN, 0, NaN);
  __f_94({foo: '0xff', bar: '234', baz: '456.1'}, 255, 234, 456, 456.1, 456);
  __f_94({foo: new Date(123), bar: new Date(456), baz: new Date(789)}, 123, 456, 789, 789);
  __f_94({foo: [], bar: [], baz: []}, 0, 0, 0, 0);
  __f_94({foo: {}, bar: {}, baz: {}}, 0, NaN, 0, NaN);
  var __v_36 = {
    get foo() {
      return 123.4;
    }
  };
  __f_94({foo: __v_33.foo, bar: __v_33.foo, baz: __v_33.foo}, 123, 123.4, 123, 123.4);
  var __v_33 = {
    get baz() {
      return 123.4;
    }
  };
  __f_94(__v_33, 0, NaN, 123, 123.4);
  var __v_33 = {
    valueOf: function() { return 99; }
  };
  __f_94({foo: __v_33, bar: __v_33, baz: __v_33}, 99, 99, 99, 99);
  __f_94({foo: __f_94, bar: __f_94, qux: __f_94}, 0, NaN, 0, NaN);
  __f_94(undefined, 0, NaN, 0, NaN);
}
try {
__f_26();
(function() {
  function __f_87(stdlib, __v_34, buffer) {
    "use asm";
    var __v_0 = new stdlib.Uint8Array(buffer);
    var __v_8 = new stdlib.Int32Array(buffer);
    function __f_64(__v_29, __v_37) {
      __v_29 = __v_29 | 0;
      gc();
      __v_37 = __v_37 | 0;
      __v_8[__v_29 >> 2] = __v_37;
    }
    function __f_8(__v_42, __v_28) {
      __v_29 = __v_29 | 0;
      __v_37 = __v_37 | 0;
      __v_17[__v_29 | 0] = __v_37;
    }
    function __f_49(__v_29) {
      __v_29 = __v_29 | 0;
      return __v_17[__v_29] | 0;
    }
    function __f_98(__v_29) {
      __v_29 = __v_29 | 0;
      return __v_17[__v_8[__v_29 >> -5] | 115] | 2147483648;
    }
    return {__f_49: __f_49, __f_98: __f_98, __f_64: __f_64, __f_8: __f_8};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm( __f_87.toString());
  __v_32.__f_64(0, 20);
  __v_32.__f_64(4, 21);
  __v_32.__f_64(8, 22);
  __v_32.__f_8(20, 123);
  __v_32.__f_8(21, 42);
  __v_32.__f_8(22, 77);
  assertEquals(123, __v_32.__f_49(20));
  assertEquals(42, __v_32.__f_49(21));
  assertEquals(-1073, __v_32.__f_49(21));
  assertEquals(123, __v_32.__f_98(0));
  assertEquals(42, __v_32.__f_98(4));
  assertEquals(77, __v_32.__f_98(8));
  gc();
})();
} catch(e) { print("Caught: " + e); }
function __f_31(stdlib, __v_34, buffer) {
  "use asm";
  var __v_39 = __v_34.x | 0, __v_38 = __v_34.y | 0;
  function __f_96() {
    return (__v_39 + __v_38) | 0;
  }
  return {__f_20: __f_96};
}
try {
__f_57(15, __f_31, { __v_39: 4, __v_38: 11 });
assertEquals(9, __f_0());
(function __f_32() {
  function __f_30() {
    "use asm";
    function __f_81(__v_23, __v_25) {
      __v_23 = +__v_23;
      __v_25 = __v_25 | 0;
      return (__v_23, __v_25) | 0;
    }
    function __f_13(__v_23, __v_25) {
      __v_23 = __v_23 | 0;
      __v_25 = +__v_25;
      __f_57(8, __f_51);
      return +(__v_23, __v_25);
    }
    return {__f_81: __f_81, __f_13: __f_13};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_30.toString());
  assertEquals(123, __v_32.__f_81(456.7, 123));
  assertEquals(123.4, __v_32.__f_13(456, 123.4));
})();
} catch(e) { print("Caught: " + e); }
function __f_82(stdlib) {
  "use asm";
  var __v_13 = stdlib.Math.fround;
  __f_57(11, __f_45);
  function __f_73() {
    var __v_39 = __v_13(1.0);
    return +__v_13(__v_39);
  }
  return {__f_20: __f_73};
}
try {
__f_57(1, __f_82);
} catch(e) { print("Caught: " + e); }
function __f_24() {
  "use asm";
  function __f_73() {
    var __v_39 = 1;
    var __v_38 = 2;
    return (__v_39 | __v_38) | 0;
  }
  return {__f_20: __f_73};
}
try {
__f_57(3, __f_24);
} catch(e) { print("Caught: " + e); }
function __f_7() {
  "use asm";
  function __f_73() {
    var __v_39 = 3;
    gc();
    var __v_21 = 2;
    return (__v_39 & __v_38) | 0;
  }
  return {__f_20: __f_73};
}
try {
__f_57(2, __f_7);
} catch(e) { print("Caught: " + e); }
function __f_102() {
  "use asm";
  function __f_73() {
    var __v_0 = 3;
    var __v_38 = 2;
    return (__v_39 ^ __v_38) | -1;
  }
  return {__f_20: __f_73};
}
try {
__f_57(1, __f_102);
gc();
(function __f_58() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    var __v_8 = new stdlib.Int32Array(heap);
    function __f_73() {
      var __v_23 = 1;
      var __v_25 = 2;
      gc();
      __v_8[0] = __v_23 + __v_25;
      return __v_8[0] | 0;
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(3, __v_32.__f_73());
})();
(function __f_62() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    var __v_9 = new stdlib.Float32Array(heap);
    var __v_13 = stdlib.Math.fround;
    function __f_73() {
      var __v_23 = __v_13(1.0);
      var __v_25 = __v_13(2.0);
      __v_9[0] = __v_23 + __v_25;
      gc();
      return +__v_9[0];
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(3, __v_32.__f_73());
})();
(function __f_53() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    var __v_32 = new stdlib.Float32Array(heap);
    var __v_13 = stdlib.Math.fround;
    function __f_73() {
      var __v_23 = 1.23;
      __v_9[0] = __v_23;
      return +__v_9[0];
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(1.23, __v_32.__f_73());
});
(function __f_90() {
  function __f_110(stdlib, __v_16, heap) {
    "use asm";
    function __f_73() {
      var __v_23 = 1;
      return ((__v_23 * 3) + (4 * __v_23)) | 0;
    }
    return {__f_73: __f_73};
  }
  var __v_42 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  gc();
  assertEquals(7, __v_32.__f_73());
})();
(function __f_71() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    function __f_73() {
      var __v_23 = 1;
      var __v_25 = 3.0;
      __v_25 = __v_23;
    }
    return {__f_73: __f_73};
  }
  assertThrows(function() {
    Wasm.instantiateModuleFromAsm(__f_110.toString());
  });
})();
(function __f_44() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    function __f_73() {
      var __v_23 = 1;
      var __v_25 = 3.0;
      __v_23 = __v_25;
    }
    return {__f_73: __f_73};
  }
  assertThrows(function() {
    Wasm.instantiateModuleFromAsm(__f_110.toString());
  });
})();
(function __f_21() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    function __f_73() {
      var __v_23 = 1;
      return ((__v_23 + __v_23) * 4) | 0;
    }
    return {__f_73: __f_73};
  }
  assertThrows(function() {
    Wasm.instantiateModuleFromAsm(__f_110.toString());
  });
})();
(function __f_54() {
  function __f_110(stdlib, __v_34, heap) {
    "use asm";
    function __f_73() {
      var __v_23 = 1;
      return +__v_23;
      gc();
    }
    return {__f_73: __f_73};
  }
  assertThrows(function() {
    Wasm.instantiateModuleFromAsm(__f_110.toString());
  });
})();
(function __f_80() {
  function __f_110() {
    "use asm";
    function __f_73() {
      var __v_39 = 1;
      var __v_38 = 2;
      var __v_40 = 0;
      __v_40 = __v_39 + __v_38 & -1;
      return __v_40 | 0;
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(3, __v_32.__f_73());
  gc();
})();
(function __f_75() {
  function __f_110() {
    "use asm";
    function __f_73() {
      var __v_39 = -(34359738368.25);
      var __v_38 = -2.5;
      return +(__v_39 + __v_38);
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(-34359738370.75, __v_32.__f_73());
})();
(function __f_6() {
  function __f_110() {
    "use asm";
    function __f_73() {
      var __v_39 = 1.0;
      var __v_38 = 2.0;
      return (__v_39 & __v_38) | 0;
    }
    return {__f_73: __f_73};
  }
  assertThrows(function() {
    Wasm.instantiateModuleFromAsm(__f_110.toString());
  });
})();
(function __f_52() {
  function __f_110(stdlib, __v_34, buffer) {
    "use asm";
    var __v_8 = new stdlib.Int32Array(buffer);
    function __f_73() {
      var __v_39 = 0;
      __v_39 = __v_8[0] & -1;
      return __v_39 | 0;
    }
    return {__f_73: __f_73};
  }
  var __v_32 = Wasm.instantiateModuleFromAsm(__f_110.toString());
  assertEquals(0, __v_32.__f_73());
})();
(function __f_33() {
  function __f_61($__v_23,$__v_25,$__v_24){'use asm';
    function __f_77() {
      var __v_28 = 0.0;
      var __v_23 = 0;
      __v_28 = 5616315000.000001;
      __v_23 = ~~__v_28 >>>0;
      __v_0 = {};
      return __v_23 | 0;
    }
    return { main : __f_77 };
  }
  var __v_40 = Wasm.instantiateModuleFromAsm(__f_61.toString());
  assertEquals(1321347704, __v_2.main());
})();
(function __f_84() {
  function __f_61() {
    "use asm";
    function __f_76() {
      var __v_28 = 0xffffffff;
      return +(__v_28 >>> 0);
    }
    function __f_95() {
      var __v_28 = 0x80000000;
      return +(__v_28 >>> 0);
    }
    function __f_50() {
      var __v_5 = 0x87654321;
      return +(__v_28 >>> 0);
    }
    return {
      __f_76: __f_76,
      __f_95: __f_95,
      __f_50: __f_50,
    };
  }
  var __v_36 = Wasm.instantiateModuleFromAsm(__f_61.toString());
  assertEquals(0xffffffff, __v_36.__f_76());
  assertEquals(0x80000000, __v_36.__f_95());
  assertEquals(0x87654321, __v_30.__f_50());
})();
} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd95c60^!)  
[src/asmjs/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/typing-asm.cc?cl=cd95c60)  
[test/cctest/test-asm-validator.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-asm-validator.cc?cl=cd95c60)  
[test/mjsunit/regress/regress-618608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-618608.js?cl=cd95c60)  
  

---   

## **regress-crbug-515897.js (chromium issue)**  
   
**[Issue 515897:
 Wrong escaping of forward slashes in RegExp.source](https://crbug.com/515897)**  
**[Commit: [regexp] Fix regexp source escaping with preceding backslashes.](https://chromium.googlesource.com/v8/v8/+/bbb2159)**  
  
Date(Commit): Tue Jul 12 05:36:17 2016  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["M-47", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/2137033002](https://codereview.chromium.org/2137033002)  
Regress: [mjsunit/regress/regress-crbug-515897.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-515897.js)  
```javascript
var r1 = new RegExp("\\\\/");
assertTrue(r1.test("\\/"));
var r2 = eval("/" + r1.source + "/");
assertEquals("\\\\\\/", r1.source);
assertEquals("\\\\\\/", r2.source);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bbb2159^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=bbb2159)  
[test/mjsunit/regress/regress-3229.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3229.js?cl=bbb2159)  
[test/mjsunit/regress/regress-crbug-515897.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-515897.js?cl=bbb2159)  
[test/webkit/fast/regex/toString-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/regex/toString-expected.txt?cl=bbb2159)  
  

---   

## **regress-624300.js (chromium issue)**  
   
**[Issue 624300:
 peek_any_identifier() || peek() == Token::LPAREN in parser.cc](https://crbug.com/624300)**  
**[Commit: Narrowly address async function stack overflow parsing case](https://chromium.googlesource.com/v8/v8/+/77cbe276)**  
  
Date(Commit): Mon Jul 11 19:33:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2135503002](https://codereview.chromium.org/2135503002)  
Regress: [mjsunit/es8/regress/regress-624300.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es8/regress/regress-624300.js)  
```javascript
(function f() {
  try {
    f();
  } catch (e) {
    (async() => await 1).length;
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77cbe276^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=77cbe276)  
[test/mjsunit/harmony/regress/regress-624300.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-624300.js?cl=77cbe276)  
  

---   

## **regress-613928.js (chromium issue)**  
   
**[Issue 613928:
 !v->IsParameter() in src/wasm/asm-wasm-builder.cc](https://crbug.com/613928)**  
**[Commit: [wasm] throw in case of assignment to module parameters](https://chromium.googlesource.com/v8/v8/+/8474f24)**  
  
Date(Commit): Mon Jul 11 17:41:30 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "BlocksAsmWasmLaunch"]  
Code Review: [https://codereview.chromium.org/2123283007](https://codereview.chromium.org/2123283007)  
Regress: [mjsunit/regress/regress-613928.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-613928.js)  
```javascript
(function __f_54() {
  function __f_41(stdlib, __v_35) {
    "use asm";
    __v_35 = __v_35;
    function __f_21(int_val, double_val) {
      int_val = int_val|0;
      double_val = +double_val;
    }
    return {__f_21:__f_21};
  }
  __f_41();
  assertFalse(%IsAsmWasmCode(__f_41));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8474f24^!)  
[src/asmjs/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/typing-asm.cc?cl=8474f24)  
[test/mjsunit/regress/regress-613928.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-613928.js?cl=8474f24)  
  

---   

## **regress-5174.js (v8 issue)**  
   
**[Issue 5174:
 Wrong behavior of Object.keys on Proxy on built in objects](https://crbug.com/v8/5174)**  
**[Commit: [keys] propagate PropertyFilter to proxy targets in KeyAccumulator](https://chromium.googlesource.com/v8/v8/+/08d0012)**  
  
Date(Commit): Mon Jul 11 10:39:35 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2129193003](https://codereview.chromium.org/2129193003)  
Regress: [mjsunit/regress/regress-5174.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5174.js)  
```javascript
assertEquals([], Object.keys(new Proxy([], {})));
assertEquals([], Object.keys(new Proxy(/regex/, {})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/08d0012^!)  
[src/keys.cc](https://cs.chromium.org/chromium/src/v8/src/keys.cc?cl=08d0012)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=08d0012)  
[test/mjsunit/es6/proxies-keys.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/proxies-keys.js?cl=08d0012)  
[test/mjsunit/regress/regress-5174.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5174.js?cl=08d0012)  
  

---   

## **regress-5173.js (v8 issue)**  
   
**[Issue 5173:
 Stack traces do not show assembler builtin methods](https://crbug.com/v8/5173)**  
**[Commit: [builtins] Construct builtin frame in String/Number ctors](https://chromium.googlesource.com/v8/v8/+/d49d386)**  
  
Date(Commit): Fri Jul 08 06:38:19 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2118283003](https://codereview.chromium.org/2118283003)  
Regress: [mjsunit/regress/regress-5173.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5173.js)  
```javascript
var thrower = { [Symbol.toPrimitive]: () => FAIL };

function testTraceNativeConversion(nativeFunc) {
  var nativeFuncName = nativeFunc.name;
  try {
    nativeFunc(thrower);
    assertUnreachable(nativeFuncName);
  } catch (e) {
    assertTrue(e.stack.indexOf(nativeFuncName) >= 0, nativeFuncName);
  }
}

testTraceNativeConversion(Math.max);
testTraceNativeConversion(Math.min);

function testBuiltinInStackTrace(script, expectedString) {
  try {
    eval(script);
    assertUnreachable(expectedString);
  } catch (e) {
    assertTrue(e.stack.indexOf(expectedString) >= 0, expectedString);
  }
}

testBuiltinInStackTrace("Date.prototype.getDate.call('')", "at String.getDate");
testBuiltinInStackTrace("Date.prototype.getUTCDate.call('')",
                        "at String.getUTCDate");
testBuiltinInStackTrace("Date.prototype.getTime.call('')", "at String.getTime");

testBuiltinInStackTrace("Number(thrower);", "at Number");
testBuiltinInStackTrace("new Number(thrower);", "at new Number");
testBuiltinInStackTrace("String(thrower);", "at String");
testBuiltinInStackTrace("new String(thrower);", "at new String");

testBuiltinInStackTrace("Math.acos(thrower);", "at Math.acos");
testBuiltinInStackTrace("Math.asin(thrower);", "at Math.asin");
testBuiltinInStackTrace("Math.fround(thrower);", "at Math.fround");
testBuiltinInStackTrace("Math.imul(thrower);", "at Math.imul");

testBuiltinInStackTrace("((f, x) => f(x))(Math.acos, thrower);", "at acos");
testBuiltinInStackTrace("((f, x) => f(x))(Math.asin, thrower);", "at asin");
testBuiltinInStackTrace("((f, x) => f(x))(Math.fround, thrower);", "at fround");
testBuiltinInStackTrace("((f, x) => f(x))(Math.imul, thrower);", "at imul");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d49d386^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=d49d386)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=d49d386)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=d49d386)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=d49d386)  
[src/arm64/macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.cc?cl=d49d386)  
...  
  

---   

## **regress-5181.js (v8 issue)**  
   
**[Issue 5181:
 DCHECK fails when Proxy target has a null prototype](https://crbug.com/v8/5181)**  
**[Commit: [ForIn] Fix HasEnumerableProperty for Proxies with null prototype](https://chromium.googlesource.com/v8/v8/+/b36237b)**  
  
Date(Commit): Thu Jul 07 10:12:06 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2129563002](https://codereview.chromium.org/2129563002)  
Regress: [mjsunit/regress/regress-5181.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5181.js)  
```javascript
var target = Object.create(null);
var proxy = new Proxy(target, {
  ownKeys: function() {
    return ['a'];
  }
});
for (var key in proxy) ;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b36237b^!)  
[src/runtime/runtime-forin.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-forin.cc?cl=b36237b)  
[test/mjsunit/regress/regress-5181.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5181.js?cl=b36237b)  
  

---   

## **regress-5178.js (v8 issue)**  
   
**[Issue 5178:
 Crash with missing TDZ-check in catch-node](https://crbug.com/v8/5178)**  
**[Commit: [parser] Fix bug in destructuring binding for catch.](https://chromium.googlesource.com/v8/v8/+/4a4f717)**  
  
Date(Commit): Thu Jul 07 07:31:16 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2123953002](https://codereview.chromium.org/2123953002)  
Regress: [mjsunit/regress/regress-5178.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5178.js)  
```javascript
assertThrows(() => {
  try { throw {} } catch({a=b, b}) { a+b }
}, ReferenceError);

try { throw {a: 42} } catch({a, b=a}) { assertEquals(42, b) };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a4f717^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=4a4f717)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=4a4f717)  
[test/mjsunit/regress/regress-5178.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5178.js?cl=4a4f717)  
  

---   

## **regress-crbug-625590.js (chromium issue)**  
   
**[Issue 625590:
 RUNTIME_ASSERT in args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/625590)**  
**[Commit: [fullcode][mips][mips64][ppc][s390] Avoid trashing of a home object when doing a keyed store to a super.](https://chromium.googlesource.com/v8/v8/+/43aee03)**  
  
Date(Commit): Mon Jul 04 11:42:39 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2120963002](https://codereview.chromium.org/2120963002)  
Regress: [mjsunit/regress/regress-crbug-625590.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-625590.js)  
```javascript
var obj = {};
function f() {}
f.prototype = {
  mSloppy() {
    super[obj] = 15;
  }
};
new f().mSloppy();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/43aee03^!)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=43aee03)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=43aee03)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=43aee03)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=43aee03)  
[test/mjsunit/regress/regress-crbug-625590.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-625590.js?cl=43aee03)  
  

---   

## **regress-crbug-625547.js (chromium issue)**  
   
**[Issue 625547:
 ho->GetHeap()->Contains(ho) in objects-debug.cc](https://crbug.com/625547)**  
**[Commit: [crankshaft] Use canonical nan_value or minus_zero_value objects instead of constant heap numbers with NaN or -0.0 values.](https://chromium.googlesource.com/v8/v8/+/acd674d)**  
  
Date(Commit): Mon Jul 04 09:59:26 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "M-52", "M-53", "Clusterfuzz", "M-51"]  
Code Review: [https://codereview.chromium.org/2115413002](https://codereview.chromium.org/2115413002)  
Regress: [mjsunit/regress/regress-crbug-625547.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-625547.js)  
```javascript
var v1 = {};
v1 = 0;
var v2 = {};
v2 = 0;
gc();

var minus_zero = {z:-0.0}.z;
var nan = undefined + 1;
function f() {
  v1 = minus_zero;
  v2 = nan;
};
%OptimizeFunctionOnNextCall(f);
f();
gc();  // Boom!  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/acd674d^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=acd674d)  
[test/mjsunit/regress/regress-crbug-625547.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-625547.js?cl=acd674d)  
  

---   

## **regress-625558.js (chromium issue)**  
   
**[Issue 625558:
 !type->Is(Type::None()) in simplified-lowering.cc](https://crbug.com/625558)**  
**[Commit: [turbofan] Better handling of empty type in simplified lowering.](https://chromium.googlesource.com/v8/v8/+/9fdacb9)**  
  
Date(Commit): Mon Jul 04 08:43:12 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2120243002](https://codereview.chromium.org/2120243002)  
Regress: [mjsunit/compiler/regress-625558.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-625558.js)  
```javascript
for (var global = 0; global <= 256; global++) { }

function f() {
  global = "luft";
  global += ++global;
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9fdacb9^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=9fdacb9)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=9fdacb9)  
[test/mjsunit/compiler/regress-625558.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-625558.js?cl=9fdacb9)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=9fdacb9)  
  

---   

## **regress-crbug-624747.js (chromium issue)**  
   
**[Issue 624747:
 IrOpcode::kFrameState == state->op()->opcode() in instruction-selector.cc](https://crbug.com/624747)**  
**[Commit: [turbofan] Broaden checkpoint elimination on returns.](https://chromium.googlesource.com/v8/v8/+/a757a62)**  
  
Date(Commit): Fri Jul 01 13:53:45 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2118793002](https://codereview.chromium.org/2118793002)  
Regress: [mjsunit/regress/regress-crbug-624747.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-624747.js)  
```javascript
"use strict";

function bar() {
  try {
    unref;
  } catch (e) {
    return (1 instanceof TypeError) && unref();  // Call in tail position!
  }
}

function foo() {
  return bar();  // Call in tail position!
}

%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a757a62^!)  
[src/compiler/checkpoint-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/checkpoint-elimination.cc?cl=a757a62)  
[test/mjsunit/regress/regress-crbug-624747.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-624747.js?cl=a757a62)  
  

---   

## **regress-625121.js (chromium issue)**  
   
**[Issue 625121:
 Unexpected operator #115:NumberSinh @ node #291 in instruction-selector.cc](https://crbug.com/625121)**  
**[Commit: [turbofan] Properly lower NumberSinh, NumberCosh and NumberTanh.](https://chromium.googlesource.com/v8/v8/+/9c281f2)**  
  
Date(Commit): Fri Jul 01 12:53:04 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Hotlist-Fixit-PE2016"]  
Code Review: [https://codereview.chromium.org/2116533004](https://codereview.chromium.org/2116533004)  
Regress: [mjsunit/regress/regress-625121.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-625121.js)  
```javascript
function test(f) {
  f(0);
  f(NaN);
  %OptimizeFunctionOnNextCall(f);
  f(1.0);
}

test(x => Math.cosh(+x));
test(x => Math.sinh(+x));
test(x => Math.tanh(+x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c281f2^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=9c281f2)  
[test/mjsunit/regress/regress-625121.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-625121.js?cl=9c281f2)  
  

---   

## **regress-crbug-624919.js (chromium issue)**  
   
**[Issue 624919:
 V8 Crash - unable to find pc offset during deoptimization](https://crbug.com/624919)**  
**[Commit: [turbofan] Fix eager bailout point after comma expression.](https://chromium.googlesource.com/v8/v8/+/920bc17)**  
  
Date(Commit): Fri Jul 01 09:51:50 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: []  
Code Review: [https://codereview.chromium.org/2113893004](https://codereview.chromium.org/2113893004)  
Regress: [mjsunit/regress/regress-crbug-624919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-624919.js)  
```javascript
function f(a, b, c, d, e) {
  if (a && (b, c ? d() : e())) return 0;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/920bc17^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=920bc17)  
[test/mjsunit/regress/regress-crbug-624919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-624919.js?cl=920bc17)  
  

---   

## **regress-double-canonicalization.js (other issue)**  
   
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
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=c17b44b)  
[test/mjsunit/regress/regress-double-canonicalization.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-double-canonicalization.js?cl=c17b44b)  
  
  
---   

## **regress-5106.js (v8 issue)**  
   
**[Issue 5106:
 Illegal access with yield in extends clause in destructuring catch-node](https://crbug.com/v8/5106)**  
**[Commit: Do all parsing for try/catch destructuring inside the appropriate scopes](https://chromium.googlesource.com/v8/v8/+/7166503)**  
  
Date(Commit): Thu Jun 30 06:52:13 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2110193002](https://codereview.chromium.org/2110193002)  
Regress: [mjsunit/regress/regress-5106.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5106.js)  
```javascript
function* g1() {
  try {
    throw {};
  } catch ({a = class extends (yield) {}}) {
  }
}
g1().next();  // crashes without fix

function* g2() {
  let x = function(){};
  try {
    throw {};
  } catch ({b = class extends x {}}) {
  }
}
g2().next();  // crashes without fix

function* g3() {
  let x = 42;
  try {
    throw {};
  } catch ({c = (function() { return x })()}) {
  }
}
g3().next();  // throws a ReferenceError without fix  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7166503^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=7166503)  
[test/mjsunit/regress/regress-5106.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5106.js?cl=7166503)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=7166503)  
  

---   

## **regress-617526.js (chromium issue)**  
   
**[Issue 617526:
 Unreachable code in asm-wasm-builder.cc](https://crbug.com/617526)**  
**[Commit: [wasm] fix loops and if-else to take int type instead of signed](https://chromium.googlesource.com/v8/v8/+/fa5cb20)**  
  
Date(Commit): Wed Jun 29 00:40:32 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "BlocksAsmWasmLaunch"]  
Code Review: [https://codereview.chromium.org/2101923003](https://codereview.chromium.org/2101923003)  
Regress: [mjsunit/regress/regress-617526.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-617526.js)  
```javascript
function __f_109() {
  "use asm";
  function __f_18() {
    var a = 0;
    while(2147483648) {
      a = 1;
      break;
    }
    return a|0;
  }
  return {__f_18: __f_18};
}

var wasm = __f_109();
assertTrue(%IsAsmWasmCode(__f_109));
assertEquals(1, wasm.__f_18());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa5cb20^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=fa5cb20)  
[test/mjsunit/regress/regress-617526.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-617526.js?cl=fa5cb20)  
[test/mjsunit/wasm/asm-wasm.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm.js?cl=fa5cb20)  
  

---   

## **regress-wasm-crbug-599413.js (chromium issue)**  
   
**[Issue 599413:
 (left_index == right_index) || (ignore_sign && (left_index <= 1) && (right_index](https://crbug.com/599413)**  
**[Commit: [wasm] Making compare and conditionals more correct.](https://chromium.googlesource.com/v8/v8/+/e42983d)**  
  
Date(Commit): Tue Jun 28 23:50:14 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "BlocksAsmWasmLaunch"]  
Code Review: [https://codereview.chromium.org/2106683003](https://codereview.chromium.org/2106683003)  
Regress: [mjsunit/regress/regress-wasm-crbug-599413.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-wasm-crbug-599413.js)  
```javascript
function __f_100() {
  "use asm";
  function __f_76() {
    var __v_39 = 0;
    outer: while (1) {
      while (__v_39 == 4294967295) {
      }
    }
  }
  return {__f_76: __f_76};
}
assertFalse(%IsAsmWasmCode(__f_100));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e42983d^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=e42983d)  
[src/typing-asm.h](https://cs.chromium.org/chromium/src/v8/src/typing-asm.h?cl=e42983d)  
[test/cctest/test-asm-validator.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-asm-validator.cc?cl=e42983d)  
[test/mjsunit/regress/regress-wasm-crbug-599413.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-wasm-crbug-599413.js?cl=e42983d)  
[test/mjsunit/wasm/asm-wasm.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm.js?cl=e42983d)  
  

---   

## **regress-wasm-crbug-618602.js (chromium issue)**  
   
**[Issue 618602:
 (left_index == right_index) || (ignore_sign && (left_index <= 1) && (right_index](https://crbug.com/618602)**  
**[Commit: [wasm] Forbid sign mismatch in asm typer.](https://chromium.googlesource.com/v8/v8/+/c585677)**  
  
Date(Commit): Tue Jun 28 21:01:36 2016  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "BlocksAsmWasmLaunch"]  
Code Review: [https://codereview.chromium.org/2107683002](https://codereview.chromium.org/2107683002)  
Regress: [mjsunit/regress/regress-wasm-crbug-618602.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-wasm-crbug-618602.js)  
```javascript
function __f_1() {
  'use asm';
  function __f_3() {
    var __v_11 = 1, __v_10 = 0, __v_12 = 0;
    __v_12 = (__v_10 | 12) % 4294967295 | -1073741824;
  }
  return { __f_3: __f_3 };
}
assertFalse(%IsAsmWasmCode(__f_1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c585677^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=c585677)  
[test/cctest/test-asm-validator.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-asm-validator.cc?cl=c585677)  
[test/mjsunit/regress/regress-wasm-crbug-618602.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-wasm-crbug-618602.js?cl=c585677)  
[test/mjsunit/wasm/asm-wasm-i32.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm-i32.js?cl=c585677)  
  

---   

## **regress-622663.js (chromium issue)**  
   
**[Issue 622663:
 reg >= first_temporary_register() && reg <= last_temporary_register() in bytecod](https://crbug.com/622663)**  
**[Commit: Fix bug with re-scoping arrow function parameter initializers](https://chromium.googlesource.com/v8/v8/+/61c137c)**  
  
Date(Commit): Tue Jun 28 15:10:17 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.3"]  
Code Review: [https://codereview.chromium.org/2083083007](https://codereview.chromium.org/2083083007)  
Regress: [mjsunit/regress/regress-622663.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-622663.js)  
```javascript
+// Copyright 2016 the V8 project authors. All rights reserved.
+// Use of this source code is governed by a BSD-style license that can be
+// found in the LICENSE file.
+
+// Flags: --no-lazy

(function() {
  try { (y = [...[]]) => {} } catch(_) {}  // will core dump, if not fixed
})();

(function() {
  try { ((y = [...[]]) => {})(); } catch(_) {}  // will core dump, if not fixed,
                                                // even without --no-lazy
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/61c137c^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=61c137c)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=61c137c)  
[src/parsing/parameter-initializer-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parameter-initializer-rewriter.cc?cl=61c137c)  
[test/mjsunit/regress/regress-622663.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-622663.js?cl=61c137c)  
  

---   

## **regress-5158.js (v8 issue)**  
   
**[Issue 5158:
 ARM64 deopt from Int32SubWithOverflow is wrong](https://crbug.com/v8/5158)**  
**[Commit: [arm64] We must not overwrite registers for binop results that are used in frame states.](https://chromium.googlesource.com/v8/v8/+/29da546)**  
  
Date(Commit): Tue Jun 28 10:11:13 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2103793002](https://codereview.chromium.org/2103793002)  
Regress: [mjsunit/compiler/regress-5158.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5158.js)  
```javascript
function foo(x) {
  x = +x;
  return (x > 0) ? x : 0 - x;
}

%PrepareFunctionForOptimization(foo);
foo(1);
foo(-1);
foo(0);
%OptimizeFunctionOnNextCall(foo);
assertEquals(2147483648, foo(-2147483648));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/29da546^!)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=29da546)  
[test/mjsunit/compiler/regress-5158.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5158.js?cl=29da546)  
  

---   

## **regress-crbug-621816.js (chromium issue)**  
   
**[Issue 621816:
 safe_to_deopt_topmost_optimized_code in deoptimizer.cc](https://crbug.com/621816)**  
**[Commit: [turbofan] Fix missing lazy deopt in object literals.](https://chromium.googlesource.com/v8/v8/+/4af8029)**  
  
Date(Commit): Mon Jun 27 13:56:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2099133003](https://codereview.chromium.org/2099133003)  
Regress: [mjsunit/regress/regress-crbug-621816.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621816.js)  
```javascript
function f() {
  var o = {};
  o.a = 1;
}
function g() {
  var o = { ['a']: function(){} };
  f();
}
f();
f();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4af8029^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=4af8029)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=4af8029)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=4af8029)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=4af8029)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=4af8029)  
...  
  

---   

## **regress-crbug-620650.js (chromium issue)**  
   
**[Issue 620650:
 (value & V8_UINT64_C(ADDRESS)) != unexpected || (value & V8_UINT64_C(ADDRESS)) =](https://crbug.com/620650)**  
**[Commit: [mips] Fix using signaling NaN for holes in fixed double arrays.](https://chromium.googlesource.com/v8/v8/+/a81c665)**  
  
Date(Commit): Thu Jun 23 08:27:54 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2086343002](https://codereview.chromium.org/2086343002)  
Regress: [mjsunit/regress/regress-crbug-620650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-620650.js)  
```javascript
(function() {
  function f(src, dst, i) {
    dst[i] = src[i];
  }
  var buf = new ArrayBuffer(16);
  var view_int32 = new Int32Array(buf);
  view_int32[1] = 0xFFF7FFFF;
  var view_f64 = new Float64Array(buf);
  var arr = [,0.1];
  f(view_f64, arr, -1);
  f(view_f64, arr, 0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a81c665^!)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=a81c665)  
[src/mips/macro-assembler-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.h?cl=a81c665)  
[test/mjsunit/regress/regress-crbug-620650.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-620650.js?cl=a81c665)  
  

---   

## **regress-616386.js (chromium issue)**  
   
**[Issue 616386:
 Security: Arbitrary Memory Read in v8](https://crbug.com/616386)**  
**[Commit: Rewrite scopes of non-simple default arguments](https://chromium.googlesource.com/v8/v8/+/0e14baf)**  
  
Date(Commit): Wed Jun 22 18:22:18 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "reward-5000", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "reward-inprocess", "M-51", "merge-merged-5.2", "Release-0-M52", "CVE-2016-5172", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/2077283004](https://codereview.chromium.org/2077283004)  
Regress: [mjsunit/regress/regress-616386.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-616386.js)  
```javascript
assertEquals(0, ((y = (function(a2) { bbbb = a2 }), bbbb = eval('1')) => {y(0); return bbbb})())
assertEquals(0, (({y = (function(a2) { bbbb = a2 }), bbbb = eval('1')} = {}) => {y(0); return bbbb})())
assertEquals(0, (function (y = (function(a2) { bbbb = a2 }), bbbb = eval('1')) {y(0); return bbbb})())
assertEquals(0, (function ({y = (function(a2) { bbbb = a2 }), bbbb = eval('1')} = {}) {y(0); return bbbb})())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0e14baf^!)  
[src/parsing/parameter-initializer-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parameter-initializer-rewriter.cc?cl=0e14baf)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=0e14baf)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=0e14baf)  
[test/mjsunit/regress/regress-616386.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-616386.js?cl=0e14baf)  
  

---   

## **regress-crbug-621496.js (chromium issue)**  
   
**[Issue 621496:
 Fatal error in v8::internal::Parser::PatternRewriter::VisitBinaryOperation()](https://crbug.com/621496)**  
**[Commit: Fix bug with illegal spread as single arrow parameter](https://chromium.googlesource.com/v8/v8/+/b9f682b)**  
  
Date(Commit): Wed Jun 22 18:07:46 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2084703005](https://codereview.chromium.org/2084703005)  
Regress: [mjsunit/harmony/regress/regress-crbug-621496.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-621496.js)  
```javascript
(function testIllegalSpreadAsSingleArrowParameter() {
  assertThrows("(...[42]) => 42)", SyntaxError)  // will core dump, if not fixed
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9f682b^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=b9f682b)  
[test/mjsunit/harmony/regress/regress-crbug-621496.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-621496.js?cl=b9f682b)  
  

---   

## **regress-621869.js (chromium issue)**  
   
**[Issue 621869:
 heap->AllowedToBeMigrated(object, NEW_SPACE) in scavenger.cc](https://crbug.com/621869)**  
**[Commit: [heap] Filter out stale left-trimmed handles for scavenges](https://chromium.googlesource.com/v8/v8/+/7a88ff3)**  
  
Date(Commit): Wed Jun 22 12:22:46 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.2"]  
Code Review: [https://codereview.chromium.org/2078403002/](https://codereview.chromium.org/2078403002/)  
Regress: [mjsunit/regress/regress-621869.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-621869.js)  
```javascript
var o0 = [];
var o1 = [];
var cnt = 0;
var only_scavenge = true;
o1.__defineGetter__(0, function() {
  if (cnt++ > 2) return;
  o0.shift();
  gc(only_scavenge);
  o0.push((64));
  o0.concat(o1);
});
o1[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a88ff3^!)  
[src/heap/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap-inl.h?cl=7a88ff3)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=7a88ff3)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=7a88ff3)  
[src/heap/scavenger.cc](https://cs.chromium.org/chromium/src/v8/src/heap/scavenger.cc?cl=7a88ff3)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=7a88ff3)  
...  
  

---   

## **regress-620750.js (chromium issue)**  
   
**[Issue 620750:
 Crash in v8::internal::Heap::AllocateHeapNumber](https://crbug.com/620750)**  
**[Commit: [heap] Fix check in AdvancePage](https://chromium.googlesource.com/v8/v8/+/21b55c4)**  
  
Date(Commit): Wed Jun 22 09:10:09 2016  
Components/Type: Blink>JavaScript>GC/Bug-Security  
Labels: ["Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2090013002](https://codereview.chromium.org/2090013002)  
Regress: [mjsunit/regress/regress-620750.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-620750.js)  
```javascript
function push_a_lot(arr) {
  for (var i = 0; i < 2e4; i++) {
    arr.push(i);
  }
  return arr;
}

__v_13 = push_a_lot([]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21b55c4^!)  
[src/heap/spaces.h](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.h?cl=21b55c4)  
[test/mjsunit/regress/regress-620750.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-620750.js?cl=21b55c4)  
  

---   

## **regress-5129.js (v8 issue)**  
   
**[Issue 5129:
 Turbofan changes x - y < 0 to x < y which is not equivalent when (x - y) overflows](https://crbug.com/v8/5129)**  
**[Commit: [turbofan] x - y < 0 is not equivalent to x < y.](https://chromium.googlesource.com/v8/v8/+/488d6e5)**  
  
Date(Commit): Wed Jun 22 05:38:36 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2090493002](https://codereview.chromium.org/2090493002)  
Regress: [mjsunit/compiler/regress-5129.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5129.js)  
```javascript
function foo($a,$b) {
 $a = $a|0;
 $b = $b|0;
 var $sub = $a - $b;
 return ($sub|0) < 0;
}

%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(0x7fffffff,-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/488d6e5^!)  
[src/compiler/machine-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/machine-operator-reducer.cc?cl=488d6e5)  
[test/cctest/compiler/test-machine-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-machine-operator-reducer.cc?cl=488d6e5)  
[test/mjsunit/compiler/regress-5129.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5129.js?cl=488d6e5)  
  

---   

## **regress-crbug-621111.js (chromium issue)**  
   
**[Issue 621111:
 Fatal error in v8::internal::List<T, P>::Add()](https://crbug.com/621111)**  
**[Commit: Fix classifier related bug](https://chromium.googlesource.com/v8/v8/+/2cabc86)**  
  
Date(Commit): Tue Jun 21 16:41:00 2016  
Components/Type: Blink>JavaScript>Language/Bug-Security  
Labels: ["Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "M-53", "Security_Severity-Medium", "Security_Impact-Head", "Stability-Libfuzzer", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2086513002](https://codereview.chromium.org/2086513002)  
Regress: [mjsunit/harmony/regress/regress-crbug-621111.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-621111.js)  
```javascript
(y = 1[1, [...[]]]) => 1;   // will core dump, if not fixed
(y = 1[1, [...[]]]) => {};  // will core dump, if not fixed  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2cabc86^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=2cabc86)  
[test/mjsunit/harmony/regress/regress-crbug-621111.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-621111.js?cl=2cabc86)  
  

---   

## **regress-crbug-614292.js (chromium issue)**  
   
**[Issue 614292:
 IrOpcode::kMerge == control->opcode() in src/compiler/memory-optimizer.cc](https://crbug.com/614292)**  
**[Commit: [turbofan] MemoryOptimizer cannot deal with dead nodes in use lists.](https://chromium.googlesource.com/v8/v8/+/5e0cd38)**  
  
Date(Commit): Tue Jun 21 10:40:44 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2080703003](https://codereview.chromium.org/2080703003)  
Regress: [mjsunit/regress/regress-crbug-614292.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614292.js)  
```javascript
function foo() {
  return [] | 0 && values[0] || false;
}

%OptimizeFunctionOnNextCall(foo);
try {
  foo();
} catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e0cd38^!)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=5e0cd38)  
[test/mjsunit/regress/regress-crbug-614292.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614292.js?cl=5e0cd38)  
  

---   

## **regress-crbug-621611.js (chromium issue)**  
   
**[Issue 621611:
 v8 regression or user land bug around rounding numbers / exp form](https://crbug.com/621611)**  
**[Commit: [builtins] Make sure the Math functions and constants agree.](https://chromium.googlesource.com/v8/v8/+/7877dde)**  
  
Date(Commit): Tue Jun 21 07:02:16 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Hotlist-Fixit-PE2016"]  
Code Review: [https://codereview.chromium.org/2079233005](https://codereview.chromium.org/2079233005)  
Regress: [mjsunit/regress/regress-crbug-621611.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621611.js)  
```javascript
assertEquals(Math.E, Math.exp(1));
assertEquals(Math.LN10, Math.log(10));
assertEquals(Math.LN2, Math.log(2));
assertEquals(Math.LOG10E, Math.log10(Math.E));
assertEquals(Math.LOG2E, Math.log2(Math.E));
assertEquals(Math.SQRT1_2, Math.sqrt(0.5));
assertEquals(Math.SQRT2, Math.sqrt(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7877dde^!)  
[src/base/ieee754.cc](https://cs.chromium.org/chromium/src/v8/src/base/ieee754.cc?cl=7877dde)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=7877dde)  
[src/js/math.js](https://cs.chromium.org/chromium/src/v8/src/js/math.js?cl=7877dde)  
[test/mjsunit/regress/regress-crbug-621611.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-621611.js?cl=7877dde)  
[test/unittests/base/ieee754-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/base/ieee754-unittest.cc?cl=7877dde)  
  

---   

## **regress-620553.js (chromium issue)**  
   
**[Issue 620553:
 Security: V8 OOB Read(?) in GC with Array Object.](https://crbug.com/620553)**  
**[Commit: [heap] Filter out stale left-trimmed handles](https://chromium.googlesource.com/v8/v8/+/d800a65)**  
  
Date(Commit): Mon Jun 20 14:32:15 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "reward-5000", "M-52", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "reward-inprocess", "M-51", "merge-merged-5.2", "Release-0-M52", "CVE-2016-5129", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/2078403002](https://codereview.chromium.org/2078403002)  
Regress: [mjsunit/regress/regress-620553.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-620553.js)  
```javascript
var o0 = [];
var o1 = [];
var cnt = 0;
o1.__defineGetter__(0, function() {
  if (cnt++ > 2) return;
  o0.shift();
  gc();
  o0.push(0);
  o0.concat(o1);
});
o1[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d800a65^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=d800a65)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=d800a65)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=d800a65)  
[test/mjsunit/regress/regress-620553.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-620553.js?cl=d800a65)  
  

---   

## **regress-621423.js (chromium issue)**  
   
**[Issue 621423:
 TypeError: node #235:CheckBounds(input @1 = NumberConstant:NumberConstant) type](https://crbug.com/621423)**  
**[Commit: [turbofan] Only consider inhabited types for constant folding in typed lowering.](https://chromium.googlesource.com/v8/v8/+/50d6837)**  
  
Date(Commit): Mon Jun 20 07:56:29 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2084483002](https://codereview.chromium.org/2084483002)  
Regress: [mjsunit/compiler/regress-621423.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-621423.js)  
```javascript
var a = [0, ""];
a[0] = 0;

function g(array) {
  array[1] = undefined;
}

function f() {
  g(function() {});
  g(a);
}

%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/50d6837^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=50d6837)  
[test/mjsunit/compiler/regress-621423.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-621423.js?cl=50d6837)  
  

---   

## **regress-4815.js (v8 issue)**  
   
**[Issue 4815:
 Stack traces do not show builtin methods implemented in C++](https://crbug.com/v8/4815)**  
**[Commit: [builtins] Introduce a proper BUILTIN frame type.](https://chromium.googlesource.com/v8/v8/+/f47b9e9)**  
  
Date(Commit): Fri Jun 17 07:41:34 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2069423002](https://codereview.chromium.org/2069423002)  
Regress: [mjsunit/regress/regress-4815.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4815.js)  
```javascript
var thrower = { [Symbol.toPrimitive]: () => FAIL };

function testTraceNativeConversion(nativeFunc) {
  var nativeFuncName = nativeFunc.name;
  try {
    nativeFunc(thrower);
    assertUnreachable(nativeFuncName);
  } catch (e) {
    assertTrue(e.stack.indexOf(nativeFuncName) >= 0, nativeFuncName);
  }
}

testTraceNativeConversion(Math.acos);
testTraceNativeConversion(Math.asin);
testTraceNativeConversion(Math.fround);
testTraceNativeConversion(Math.imul);


function testBuiltinInStackTrace(script, expectedString) {
  try {
    eval(script);
    assertUnreachable(expectedString);
  } catch (e) {
    assertTrue(e.stack.indexOf(expectedString) >= 0, expectedString);
  }
}

testBuiltinInStackTrace("Boolean.prototype.toString.call(thrower);",
                        "at Object.toString");

testBuiltinInStackTrace("new Date(thrower);", "at new Date");

testBuiltinInStackTrace("Math.acos(thrower);", "at Math.acos");
testBuiltinInStackTrace("Math.asin(thrower);", "at Math.asin");
testBuiltinInStackTrace("Math.fround(thrower);", "at Math.fround");
testBuiltinInStackTrace("Math.imul(thrower);", "at Math.imul");

testBuiltinInStackTrace("((f, x) => f(x))(Math.acos, thrower);", "at acos");
testBuiltinInStackTrace("((f, x) => f(x))(Math.asin, thrower);", "at asin");
testBuiltinInStackTrace("((f, x) => f(x))(Math.fround, thrower);", "at fround");
testBuiltinInStackTrace("((f, x) => f(x))(Math.imul, thrower);", "at imul");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f47b9e9^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=f47b9e9)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=f47b9e9)  
[src/frames-inl.h](https://cs.chromium.org/chromium/src/v8/src/frames-inl.h?cl=f47b9e9)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=f47b9e9)  
[src/frames.h](https://cs.chromium.org/chromium/src/v8/src/frames.h?cl=f47b9e9)  
...  
  

---   

## **regress-crbug-620253.js (chromium issue)**  
   
**[Issue 620253:
 Fatal error in v8::FromJust](https://crbug.com/620253)**  
**[Commit: [d8] Make exception reporting more resilient.](https://chromium.googlesource.com/v8/v8/+/e55384b)**  
  
Date(Commit): Thu Jun 16 10:14:08 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2069543007](https://codereview.chromium.org/2069543007)  
Regress: [mjsunit/regress/regress-crbug-620253.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-620253.js)  
```javascript
load("test/mjsunit/regress/regress-crbug-620253.js");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e55384b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=e55384b)  
[test/mjsunit/regress/regress-crbug-620253.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-620253.js?cl=e55384b)  
  

---   

## **regress-5004.js (v8 issue)**  
   
**[Issue 5004:
 Illegal access in proxied promise subclass](https://crbug.com/v8/5004)**  
**[Commit: Promises: Add regression test for promise resolution with proxy](https://chromium.googlesource.com/v8/v8/+/3624a5e)**  
  
Date(Commit): Thu Jun 16 02:00:26 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2070213002](https://codereview.chromium.org/2070213002)  
Regress: [mjsunit/regress/regress-5004.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5004.js)  
```javascript
function assertAsync(b, s) {
  if (!b) {
    %AbortJS(" FAILED!")
  }
}

class P extends Promise {
  constructor() {
    super(...arguments)
    return new Proxy(this, {
      get: (_, key) => {
        return key == 'then' ?
            this.then.bind(this) :
            this.constructor.resolve(20)
      }
    })
  }
}

let p = P.resolve(10)
p.key.then(v => assertAsync(v === 20));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3624a5e^!)  
[test/mjsunit/regress/regress-5004.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5004.js?cl=3624a5e)  
  

---   

## **regress-string-to-number-add.js (other issue)**  
   
**[Commit: [turbofan] Mark side-effect-free calls to string ops as kEliminatable.](https://chromium.googlesource.com/v8/v8/+/14a1a7e)**  
  
Date(Commit): Wed Jun 15 11:39:40 2016  
Code Review: [https://codereview.chromium.org/2063373003](https://codereview.chromium.org/2063373003)  
Regress: [mjsunit/compiler/regress-string-to-number-add.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-string-to-number-add.js)  
```javascript
function f(x) {
  var s = x ? "0" : "1";
  return 1 + Number(s);
}

%PrepareFunctionForOptimization(f);
f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
assertEquals(2, f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14a1a7e^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=14a1a7e)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=14a1a7e)  
[test/mjsunit/compiler/regress-string-to-number-add.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-string-to-number-add.js?cl=14a1a7e)  
  
  
---   

## **regress-619382.js (chromium issue)**  
   
**[Issue 619382:
 Use-of-uninitialized-value in long v8::internal::Simulator::AddWithCarry<long>](https://crbug.com/619382)**  
**[Commit: [Heap] Fix comparing against new space top pointer](https://chromium.googlesource.com/v8/v8/+/d6473f5)**  
  
Date(Commit): Tue Jun 14 13:52:01 2016  
Components/Type: Blink>JavaScript>GC/Bug-Security  
Labels: ["Reproducible", "M-52", "Security_Impact-Stable", "Security_Severity-Medium", "Stability-Memory-MemorySanitizer", "allpublic", "Clusterfuzz", "M-51", "merge-merged-5.2", "Release-0-M52"]  
Code Review: [https://codereview.chromium.org/2065063002](https://codereview.chromium.org/2065063002)  
Regress: [mjsunit/regress/regress-619382.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-619382.js)  
```javascript
(function __f_9() {
})();
function __f_16(ctor_desc) {
  var __v_22 = 5;
  var __v_25 = [];
  gc(); gc(); gc();
  for (var __v_18 = 0; __v_18 < __v_22; __v_18++) {
    __v_25[__v_18] = ctor_desc.ctor.apply();
  }
}
var __v_28 = [
  {
    ctor: function(__v_27) { return {a: __v_27}; },
    args: function() { return [1.5 + __v_18]; }  },
  {
    ctor: function(__v_27) { var __v_21 = []; __v_21[1] = __v_27; __v_21[200000] = __v_27; return __v_21; },
    args: function() { return [1.5 + __v_18]; }  },
  {
    ctor: function() {
    }  }
];
var __v_26 = [
  {
  }];
  __v_26.forEach(function(__v_16) {
    __v_28.forEach(function(ctor) {
      __f_16(ctor);
    });
  });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6473f5^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=d6473f5)  
[src/arm64/macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.cc?cl=d6473f5)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=d6473f5)  
[src/mips64/macro-assembler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/macro-assembler-mips64.cc?cl=d6473f5)  
[test/mjsunit/regress/regress-619382.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-619382.js?cl=d6473f5)  
  

---   

## **regress-crbug-618788.js (chromium issue)**  
   
**[Issue 618788:
 !array->HasFixedTypedArrayElements() in runtime-array.cc](https://crbug.com/618788)**  
**[Commit: Array.prototype.slice should only normalize result if it's an array](https://chromium.googlesource.com/v8/v8/+/56ea2f9)**  
  
Date(Commit): Tue Jun 14 09:39:23 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Clusterfuzz", "Unreproducible"]  
Code Review: [https://codereview.chromium.org/2058013002](https://codereview.chromium.org/2058013002)  
Regress: [mjsunit/regress/regress-crbug-618788.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-618788.js)  
```javascript
Object.defineProperty(Int32Array.prototype, 'length', { set(v) { } });

(function testSlice() {
  var a = new Array();
  a.constructor = Int32Array;
  a.length = 1000; // Make the length >= 1000 so UseSparseVariant returns true.
  assertTrue(a.slice() instanceof Int32Array);
})();

(function testSplice() {
  var a = new Array();
  a.constructor = Int32Array;
  a.length = 1000; // Make the length >= 1000 so UseSparseVariant returns true.
  assertTrue(a.splice(1) instanceof Int32Array);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56ea2f9^!)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=56ea2f9)  
[test/mjsunit/regress/regress-crbug-618788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-618788.js?cl=56ea2f9)  
  

---   

## **regress-store-holey-double-array.js (other issue)**  
   
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

  %PrepareFunctionForOptimization(g);
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

  %PrepareFunctionForOptimization(g);
  g(a, 1);
  g(a, 1);
  %OptimizeFunctionOnNextCall(g);
  g(a, 0);
  assertTrue(Number.isNaN(a[0]));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6470dda^!)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=6470dda)  
[src/compiler/arm/instruction-codes-arm.h](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-codes-arm.h?cl=6470dda)  
[src/compiler/arm/instruction-scheduler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-scheduler-arm.cc?cl=6470dda)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=6470dda)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=6470dda)  
...  
  
  
---   

## **regress-5100.js (v8 issue)**  
   
**[Issue 5100:
 Introduce dedicated CheckBounds operator for TurboFan](https://crbug.com/v8/5100)**  
**[Commit: [turbofan] Introduce a dedicated CheckBounds operator.](https://chromium.googlesource.com/v8/v8/+/85e5567)**  
  
Date(Commit): Tue Jun 14 06:12:06 2016  
Type: FeatureRequest  
Code Review: [https://codereview.chromium.org/2035893004](https://codereview.chromium.org/2035893004)  
Regress: [mjsunit/compiler/regress-5100.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5100.js)  
```javascript
var a = [0, 1];
a["true"] = "true";
a["false"] = "false";
a["null"] = "null";
a["undefined"] = "undefined";

(function() {
  function f(x) { return a[x]; }

  %PrepareFunctionForOptimization(f);
  assertEquals(0, f(0));
  assertEquals(0, f(0));
  %OptimizeFunctionOnNextCall(f);
  assertEquals("true", f(true));
})();

(function() {
  function f( x) { return a[x]; }

  %PrepareFunctionForOptimization(f);
  assertEquals(0, f(0));
  assertEquals(0, f(0));
  %OptimizeFunctionOnNextCall(f);
  assertEquals("false", f(false));
})();

(function() {
  function f( x) { return a[x]; }

  %PrepareFunctionForOptimization(f);
  assertEquals(0, f(0));
  assertEquals(0, f(0));
  %OptimizeFunctionOnNextCall(f);
  assertEquals("null", f(null));
})();

(function() {
  function f( x) { return a[x]; }

  %PrepareFunctionForOptimization(f);
  assertEquals(0, f(0));
  assertEquals(0, f(0));
  %OptimizeFunctionOnNextCall(f);
  assertEquals("undefined", f(undefined));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85e5567^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=85e5567)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=85e5567)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=85e5567)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=85e5567)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=85e5567)  
...  
  

---   

## **regress-618603.js (chromium issue)**  
   
**[Issue 618603:
 OperandIsValid(bytecode, operand_scale, 0, operand0) in bytecode-array-builder.c](https://crbug.com/618603)**  
**[Commit: [interpreter] support async functions in Ignition](https://chromium.googlesource.com/v8/v8/+/1a30866)**  
  
Date(Commit): Mon Jun 13 17:21:19 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2051423003](https://codereview.chromium.org/2051423003)  
Regress: [mjsunit/es8/regress/regress-618603.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es8/regress/regress-618603.js)  
```javascript
try {
} catch(e) {; }
function __f_7(expected, run) {
  var __v_10 = run();
};
__f_7("[1,2,3]", () => (function() {
      return (async () => {[...await arguments] })();
      })());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a30866^!)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=1a30866)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=1a30866)  
[test/mjsunit/harmony/regress/regress-618603.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-618603.js?cl=1a30866)  
  

---   

## **regress-crbug-619476.js (chromium issue)**  
   
**[Issue 619476:
 reported_errors_end_ <= i in expression-classifier.h](https://crbug.com/619476)**  
**[Commit: Remove erroneous DCHECK related to expression classifiers](https://chromium.googlesource.com/v8/v8/+/cdec5e8)**  
  
Date(Commit): Mon Jun 13 12:34:19 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2058413002](https://codereview.chromium.org/2058413002)  
Regress: [mjsunit/regress/regress-crbug-619476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-619476.js)  
```javascript
var x = {};
eval, x[eval];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cdec5e8^!)  
[src/parsing/expression-classifier.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-classifier.h?cl=cdec5e8)  
[test/mjsunit/regress-crbug-619476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-619476.js?cl=cdec5e8)  
  

---   

## **regress-crbug-614727.js (chromium issue)**  
   
**[Issue 614727:
 RUNTIME_ASSERT in size <= Page::kMaxRegularHeapObjectSize in src/runtime/runtime-internal.cc](https://crbug.com/614727)**  
**[Commit: Fix arguments object stubs for large arrays.](https://chromium.googlesource.com/v8/v8/+/e95cfaf)**  
  
Date(Commit): Mon Jun 13 08:25:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2054853002](https://codereview.chromium.org/2054853002)  
Regress: [mjsunit/regress/regress-crbug-614727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614727.js)  
```javascript
"use strict";

function f(a, b, c) { return arguments }
function g(...args) { return args }

var length = Math.pow(2, 15) * 3;
var args = new Array(length);
assertEquals(length, f.apply(null, args).length);
assertEquals(length, g.apply(null, args).length);

var length = Math.pow(2, 16) * 3;
var args = new Array(length);
try { f.apply(null, args) } catch(e) {}
try { g.apply(null, args) } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e95cfaf^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=e95cfaf)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=e95cfaf)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=e95cfaf)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=e95cfaf)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=e95cfaf)  
...  
  

---   

## **regress-618657.js (chromium issue)**  
   
**[Issue 618657:
 HasBytecodeArray() in objects-inl.h](https://crbug.com/618657)**  
**[Commit: [generators] Make runtime functions more robust.](https://chromium.googlesource.com/v8/v8/+/54b405c)**  
  
Date(Commit): Thu Jun 09 14:20:58 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2052873002](https://codereview.chromium.org/2052873002)  
Regress: [mjsunit/regress/regress-618657.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-618657.js)  
```javascript
function* foo() { yield 42 }
function* goo() { yield 42 }
var f = foo();
var g = goo();
assertEquals(42, f.next().value);
assertEquals(42, g.next().value);
assertEquals(true, f.next().done);
assertEquals(true, g.next().done);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54b405c^!)  
[src/runtime/runtime-generator.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-generator.cc?cl=54b405c)  
[test/mjsunit/regress/regress-618657.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-618657.js?cl=54b405c)  
  

---   

## **regress-5085.js (v8 issue)**  
   
**[Issue 5085:
 ({}) instanceof Proxy is returning true unexpectedly](https://crbug.com/v8/5085)**  
**[Commit: [es6] Fix prototype chain walk for instanceof.](https://chromium.googlesource.com/v8/v8/+/eb1c9e2)**  
  
Date(Commit): Thu Jun 09 06:26:03 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2041103007](https://codereview.chromium.org/2041103007)  
Regress: [mjsunit/regress/regress-5085.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5085.js)  
```javascript
g = async function () {
  await 10;
}
assertEquals(undefined, g.prototype)
g();
assertEquals(undefined, g.prototype)

gen = function* () {
  yield 10;
}
assertTrue(gen.prototype != undefined && gen.prototype != null)
gen()
assertTrue(gen.prototype != undefined && gen.prototype != null)

async_gen = async function* () {
  yield 10;
}
assertTrue(async_gen.prototype != undefined && async_gen.prototype != null)
async_gen()
assertTrue(async_gen.prototype != undefined && async_gen.prototype != null)

function foo(x) {
  return x instanceof Proxy;
}

function test_for_exception() {
  caught_exception = false;
  try {
    foo({});
  } catch (e) {
    caught_exception = true;
    assertEquals(
        'Function has non-object prototype \'undefined\' in instanceof check',
        e.message);
  } finally {
    assertTrue(caught_exception)
  }
}

test_for_exception();
test_for_exception();
%OptimizeFunctionOnNextCall(foo);
test_for_exception();

Proxy.__proto__.prototype = Function.prototype;
assertTrue((() => {}) instanceof Proxy);

assertEquals(
    new Proxy({}, {
      get(o, s) {
        return s
      }
    }).test,
    'test');

Proxy.__proto__ = {
  prototype: {b: 2},
  a: 1
};
assertEquals(Proxy.prototype, {b: 2});

(function testProxyCreationContext() {
  let realm = Realm.create();
  let p1 = new Proxy({}, {});
  let p2 = Realm.eval(realm, "new Proxy({}, {})");
  assertEquals(0, Realm.owner(p1));
  assertEquals(1, Realm.owner(p2));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eb1c9e2^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=eb1c9e2)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=eb1c9e2)  
[src/crankshaft/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-codegen-arm.cc?cl=eb1c9e2)  
[src/crankshaft/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-codegen-arm64.cc?cl=eb1c9e2)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=eb1c9e2)  
...  
  

---   

## **regress-crbug-495493.js (chromium issue)**  
   
**[Issue 495493:
 UNKNOWN in v8::internal::Context::global_object](https://crbug.com/495493)**  
**[Commit: [crankshaft] do not sign-extend int32 immediate in DoMathMinMax.](https://chromium.googlesource.com/v8/v8/+/7c3cad2)**  
  
Date(Commit): Wed Jun 08 10:12:16 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2044353002](https://codereview.chromium.org/2044353002)  
Regress: [mjsunit/regress/regress-crbug-495493.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-495493.js)  
```javascript
function foo(p) {
  for (var i = 0; i < 100000; ++i) {
    p = Math.min(-1, 0);
  }
}
foo(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c3cad2^!)  
[src/crankshaft/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-codegen-x64.cc?cl=7c3cad2)  
[test/mjsunit/regress/regress-crbug-495493.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-495493.js?cl=7c3cad2)  
  

---   

## **regress-crbug-617527.js (chromium issue)**  
   
**[Issue 617527:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/617527)**  
**[Commit: Add test case for 85b8c2dc (fix observable array access in messages.js).](https://chromium.googlesource.com/v8/v8/+/ada6fa1)**  
  
Date(Commit): Wed Jun 08 07:54:26 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Merge-Merged", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.1", "merge-merged-5.2", "NodeJS-Backport-Done"]  
Code Review: [https://codereview.chromium.org/2045153002](https://codereview.chromium.org/2045153002)  
Regress: [mjsunit/regress/regress-crbug-617527.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617527.js)  
```javascript
Object.defineProperty(Array.prototype, "1", { get: toLocaleString });
assertThrows(_ => new RegExp(0, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ada6fa1^!)  
[test/mjsunit/regress/regress-crbug-617527.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617527.js?cl=ada6fa1)  
  

---   

## **regress-5074.js (v8 issue)**  
   
**[Issue 5074:
 Crankshaft deopts with truncated value](https://crbug.com/v8/5074)**  
**[Commit: [crankshaft] Fix invalid number truncation assumption on HAdd inputs.](https://chromium.googlesource.com/v8/v8/+/f576e29)**  
  
Date(Commit): Wed Jun 08 03:56:22 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2048643002](https://codereview.chromium.org/2048643002)  
Regress: [mjsunit/compiler/regress-5074.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5074.js)  
```javascript
var s = [,0.1];

function foo(a, b) {
  var x = s[a];
  s[1] = 0.1;
  return x + b;
}

%PrepareFunctionForOptimization(foo);
assertEquals(2.1, foo(1, 2));
assertEquals(2.1, foo(1, 2));
%OptimizeFunctionOnNextCall(foo);
assertEquals("undefined2", foo(0, "2"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f576e29^!)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=f576e29)  
[test/mjsunit/compiler/regress-5074.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-5074.js?cl=f576e29)  
  

---   

## **regress-crbug-617524.js (chromium issue)**  
   
**[Issue 617524:
 value->IsMutableHeapNumber() in objects-debug.cc](https://crbug.com/617524)**  
**[Commit: [runtime] Don't use ElementsTransitionAndStoreStub for transitions that involve instance rewriting.](https://chromium.googlesource.com/v8/v8/+/3e0be8d)**  
  
Date(Commit): Tue Jun 07 09:50:04 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0", "merge-merged-5.1", "merge-merged-5.2", "NodeJS-Backport-Done"]  
Code Review: [https://codereview.chromium.org/2044003003](https://codereview.chromium.org/2044003003)  
Regress: [mjsunit/regress/regress-crbug-617524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617524.js)  
```javascript
function f(a,b,c) {
  a.a = b;
  a[1] = c;
  return a;
}

f(new Array(5),.5,0);
var o1 = f(new Array(5),0,.5);
gc();
var o2 = f(new Array(5),0,0);
var o3 = f(new Array(5),0);
assertEquals(0, o3.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e0be8d^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=3e0be8d)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=3e0be8d)  
[test/mjsunit/regress/regress-crbug-617524.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617524.js?cl=3e0be8d)  
  

---   

## **regress-617525.js (chromium issue)**  
   
**[Issue 617525:
 !locals_.has_sig() in encoder.cc](https://crbug.com/617525)**  
**[Commit: [asmjs] Validator should reject modules with repeated functions.](https://chromium.googlesource.com/v8/v8/+/0b91952)**  
  
Date(Commit): Mon Jun 06 13:40:42 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2040983002](https://codereview.chromium.org/2040983002)  
Regress: [mjsunit/regress/regress-617525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-617525.js)  
```javascript
function __f_14() {
  "use asm";
  function __f_15() { return 0; }
  function __f_15() { return 137; }  // redeclared function
  return {};
}
__f_14();
assertFalse(%IsAsmWasmCode(__f_14));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b91952^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=0b91952)  
[test/cctest/test-asm-validator.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-asm-validator.cc?cl=0b91952)  
[test/mjsunit/regress/regress-617525.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-617525.js?cl=0b91952)  
  

---   

## **regress-617529.js (chromium issue)**  
   
**[Issue 617529:
 kAstStmt != var_type in asm-wasm-builder.cc](https://crbug.com/617529)**  
**[Commit: [asmjs] Validator should reject assignments to heap variables in functions.](https://chromium.googlesource.com/v8/v8/+/dc98fab)**  
  
Date(Commit): Mon Jun 06 13:30:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2041843002](https://codereview.chromium.org/2041843002)  
Regress: [mjsunit/regress/regress-617529.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-617529.js)  
```javascript
function __f_71(stdlib, buffer) {
  "use asm";
  var __v_22 = new stdlib.Float64Array(buffer);
  function __f_26() {
    __v_22 = __v_22;
  }
  return {__f_26: __f_26};
}

__f_71(this);
assertFalse(%IsAsmWasmCode(__f_71));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc98fab^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=dc98fab)  
[test/mjsunit/regress/regress-617529.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-617529.js?cl=dc98fab)  
  

---   

## **regress-crbug-617567.js (chromium issue)**  
   
**[Issue 617567:
 1 == effect->op()->EffectInputCount() in node-properties.cc](https://crbug.com/617567)**  
**[Commit: [turbofan] Make FindFrameStateBefore handle dead paths.](https://chromium.googlesource.com/v8/v8/+/826627d)**  
  
Date(Commit): Mon Jun 06 12:34:53 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2041833002](https://codereview.chromium.org/2041833002)  
Regress: [mjsunit/regress/regress-crbug-617567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617567.js)  
```javascript
var v1 = {};
function g() {
  v1 = [];
  for (var i = 0; i < 1; i++) {
    v1[i]();
  }
}

var v2 = {};
var v3 = {};
function f() {
  v3 = v2;
  g();
}

assertThrows(g);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/826627d^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=826627d)  
[test/mjsunit/regress/regress-crbug-617567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617567.js?cl=826627d)  
  

---   

## **regress-616064.js (chromium issue)**  
   
**[Issue 616064:
 Crash in v8::internal::interpreter::BytecodeRegisterOptimizer::OutputRegisterTransfer](https://crbug.com/616064)**  
**[Commit: [Interpreter] Don't try to eliminate dead-code in bytecode-array-builder](https://chromium.googlesource.com/v8/v8/+/2fd3f9d)**  
  
Date(Commit): Wed Jun 01 22:55:10 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-53", "Fracas", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2030583002](https://codereview.chromium.org/2030583002)  
Regress: [mjsunit/ignition/regress-616064.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-616064.js)  
```javascript
function foo() {
  if (this.Worker) {
    function __f_0() { this.s = a; }
    function __f_1() {
      this.l = __f_0;
    }

    with ( 'source' , Object ) throw function __f_0(__f_0) {
      return Worker.__f_0(-2147483648, __f_0);
    };

    var __v_9 = new Worker('', {type: 'string'});
    __f_1 = {s: Math.s, __f_1: true};
  }
}

try {
  foo();
} catch(e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2fd3f9d^!)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=2fd3f9d)  
[src/interpreter/bytecode-array-builder.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.h?cl=2fd3f9d)  
[src/interpreter/bytecode-peephole-optimizer.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-peephole-optimizer.h?cl=2fd3f9d)  
[src/interpreter/bytecode-register-optimizer.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register-optimizer.cc?cl=2fd3f9d)  
[src/interpreter/bytecode-register-optimizer.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register-optimizer.h?cl=2fd3f9d)  
...  
  

---   

## **regress-615776.js (chromium issue)**  
   
**[Issue 615776:
 RUNTIME_ASSERT in args[0]->IsNumber() in src/runtime/runtime-maths.cc](https://crbug.com/615776)**  
**[Commit: math.js: Use %_TypedArrayGetLength to get length](https://chromium.googlesource.com/v8/v8/+/a7d091f)**  
  
Date(Commit): Wed Jun 01 18:44:30 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2020203006](https://codereview.chromium.org/2020203006)  
Regress: [mjsunit/regress/regress-615776.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-615776.js)  
```javascript
Object.defineProperty(Int32Array.prototype.__proto__, 'length', {
  get: function() { throw new Error('Custom length property'); }
});

var a = Math.random();

var v0 = new Set();
var v1 = new Object();
v0.add(v1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7d091f^!)  
[src/js/math.js](https://cs.chromium.org/chromium/src/v8/src/js/math.js?cl=a7d091f)  
[test/mjsunit/regress/regress-615776.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-615776.js?cl=a7d091f)  
  

---   

## **regress-v8-5009.js (v8 issue)**  
   
**[Issue 5009:
 StoreIC sometimes does not store](https://crbug.com/v8/5009)**  
**[Commit: [runtime] Ensure that all elements kind transitions are chained to the root map.](https://chromium.googlesource.com/v8/v8/+/9fa206e)**  
  
Date(Commit): Wed Jun 01 15:55:11 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2015513002](https://codereview.chromium.org/2015513002)  
Regress: [mjsunit/regress/regress-v8-5009.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5009.js)  
```javascript
function fn1() {
}

function fn2() {
}

function fn3() {
}

function create(id) {
  // Just some `FunctionTemplate` to hang on
  var o = new version();

  o.id = id;
  o[0] = null;

  return o;
}

function setM1(o) {
  o.m1 = fn1;
}

function setM2(o) {
  o.m2 = fn2;
}

function setAltM2(o) {
  // Failing StoreIC happens here
  o.m2 = fn3;
}

function setAltM1(o) {
  o.m1 = null;
}

function test(o) {
  o.m2();
  o.m1();
}

var p0 = create(0);
var p1 = create(1);
var p2 = create(2);

setM1(p0);
setM1(p1);
setM1(p2);

setM2(p0);
setAltM2(p0);
setAltM1(p0);

setAltM2(p1);

setAltM2(p2);
test(p2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9fa206e^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=9fa206e)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=9fa206e)  
[src/ic/ic-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic-compiler.cc?cl=9fa206e)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=9fa206e)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=9fa206e)  
...  
  

---   

## **regress-612146.js (chromium issue)**  
   
**[Issue 612146:
 Renderer crash on zhytomyr.dozor-gps.com.ua](https://crbug.com/612146)**  
**[Commit: [crankshaft] Only exclude explicit 'arguments' (and 'this') from liveness analysis.](https://chromium.googlesource.com/v8/v8/+/1428fbe)**  
  
Date(Commit): Wed Jun 01 12:04:35 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "Reproducible"]  
Code Review: [https://codereview.chromium.org/2026173003](https://codereview.chromium.org/2026173003)  
Regress: [mjsunit/regress/regress-612146.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-612146.js)  
```javascript
function f() {
  var arguments_ = arguments;
  if (undefined) {
    while (true) {
      arguments_[0];
    }
  } else {
    %DeoptimizeNow();
    return arguments_[0];
  }
};

f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f(1));

function g() {
    var a = arguments;
    %DeoptimizeNow();
    return a.length;
}

g(1);
g(1);
%OptimizeFunctionOnNextCall(g);
assertEquals(1, g(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1428fbe^!)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=1428fbe)  
[test/mjsunit/regress/regress-612146.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-612146.js?cl=1428fbe)  
  

---   

## **regress-truncate-number-or-undefined-to-float64.js (other issue)**  
   
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

%PrepareFunctionForOptimization(g);
g(0);
g(0);
%OptimizeFunctionOnNextCall(g);
assertTrue(Number.isNaN(g(0, true)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e96f47^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=5e96f47)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=5e96f47)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=5e96f47)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=5e96f47)  
[src/compiler/simplified-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator-reducer.cc?cl=5e96f47)  
...  
  
  
---   

## **regress-string-from-char-code-tonumber.js (other issue)**  
   
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
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=7554360)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=7554360)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=7554360)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=7554360)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=7554360)  
...  
  
  
---   

## **regress-number-is-hole-nan.js (other issue)**  
   
**[Commit: [turbofan] Fix NumberIsHoleNaN to check the upper word.](https://chromium.googlesource.com/v8/v8/+/496aecb)**  
  
Date(Commit): Mon May 30 11:48:07 2016  
Code Review: [https://codereview.chromium.org/2022753002](https://codereview.chromium.org/2022753002)  
Regress: [mjsunit/compiler/regress-number-is-hole-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-number-is-hole-nan.js)  
```javascript
var a = [, 2.121736758e-314];

function foo() { return a[1]; }

%PrepareFunctionForOptimization(foo);
assertEquals(2.121736758e-314, foo());
assertEquals(2.121736758e-314, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(2.121736758e-314, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/496aecb^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=496aecb)  
[test/mjsunit/compiler/regress-number-is-hole-nan.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-number-is-hole-nan.js?cl=496aecb)  
  
  
---   

## **regress-crbug-615774.js (chromium issue)**  
   
**[Issue 615774:
 index < GetInternalFieldCount() && index >= 0 in src/objects-inl.h](https://crbug.com/615774)**  
**[Commit: Check CallSite arguments more rigorously](https://chromium.googlesource.com/v8/v8/+/25c2203)**  
  
Date(Commit): Mon May 30 10:30:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2006603002](https://codereview.chromium.org/2006603002)  
Regress: [mjsunit/regress/regress-crbug-615774.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-615774.js)  
```javascript
Error.prepareStackTrace = (e,s) => s;
var CallSiteConstructor = Error().stack[0].constructor;

try {
  (new CallSiteConstructor(CallSiteConstructor, 6)).toString();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/25c2203^!)  
[src/js/messages.js](https://cs.chromium.org/chromium/src/v8/src/js/messages.js?cl=25c2203)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=25c2203)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=25c2203)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=25c2203)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=25c2203)  
...  
  

---   

## **regress-crbug-612109.js (chromium issue)**  
   
**[Issue 612109:
 Crash in v8::internal::Heap::CreateFillerObjectAt](https://crbug.com/612109)**  
**[Commit: [builtins] Rewrite uri.js as builtin functions.](https://chromium.googlesource.com/v8/v8/+/8c31bd8)**  
  
Date(Commit): Fri May 27 09:57:07 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-52", "findit-wrong", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1994733003](https://codereview.chromium.org/1994733003)  
Regress: [mjsunit/regress/regress-crbug-612109.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-612109.js)  
```javascript
s = "string for triggering osr in __f_0";
for (var i = 0; i < 16; i++) s = s + s;
decodeURI(encodeURI(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c31bd8^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=8c31bd8)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=8c31bd8)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=8c31bd8)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=8c31bd8)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=8c31bd8)  
...  
  

---   

## **regress-5036.js (v8 issue)**  
   
**[Issue 5036:
 regexp with unicode and ignore case flag does not match correctly](https://crbug.com/v8/5036)**  
**[Commit: [regexp] fix /ui regexp desugaring for text nodes.](https://chromium.googlesource.com/v8/v8/+/5d93296)**  
  
Date(Commit): Mon May 23 22:23:43 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2005753003](https://codereview.chromium.org/2005753003)  
Regress: [mjsunit/regress/regress-5036.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5036.js)  
```javascript
assertEquals(["1\u212a"], /\d\w/ui.exec("1\u212a"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d93296^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=5d93296)  
[test/mjsunit/regress/regress-5036.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5036.js?cl=5d93296)  
  

---   

## **regress-612412.js (chromium issue)**  
   
**[Issue 612412:
 OperatorProperties::GetTotalInputCount(node->op()) == node->InputCount() in src/](https://crbug.com/612412)**  
**[Commit: [turbofan] Correctly call ArrayNoArgumentConstructor stub from TF code](https://chromium.googlesource.com/v8/v8/+/f43aa0b)**  
  
Date(Commit): Mon May 23 16:44:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/1999783004](https://codereview.chromium.org/1999783004)  
Regress: [mjsunit/regress/regress-612412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-612412.js)  
```javascript
function counter() { return {x: 0} || this }

var f = (function() {
  "use asm";
  return function g(c1, c2) {
    for (var x = 0 ; x < 10; ++x) {
      if (x == 5) %OptimizeOsr();
      c1();
    }
  }
})();

g = (function() { f((Array), counter()); });
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f43aa0b^!)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=f43aa0b)  
[test/mjsunit/regress/regress-612412.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-612412.js?cl=f43aa0b)  
  

---   

## **regress-crbug-613919.js (chromium issue)**  
   
**[Issue 613919:
 Unreachable code in src/objects-inl.h](https://crbug.com/613919)**  
**[Commit: [deoptimizer] Fix materialization of sloppy arguments.](https://chromium.googlesource.com/v8/v8/+/3cc2adb)**  
  
Date(Commit): Mon May 23 13:52:35 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2001133002](https://codereview.chromium.org/2001133002)  
Regress: [mjsunit/regress/regress-crbug-613919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613919.js)  
```javascript
function g(a) {
  if (a) return arguments;
  %DeoptimizeNow();
  return 23;
}
function f() {
  return g(false);
}
assertEquals(23, f());
assertEquals(23, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(23, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cc2adb^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=3cc2adb)  
[test/mjsunit/regress/regress-crbug-613919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613919.js?cl=3cc2adb)  
  

---   

## **regress-crbug-613494.js (chromium issue)**  
   
**[Issue 613494:
 Int64Constant of kRepWord64 (Internal) cannot be changed to kRepTagged in src/co](https://crbug.com/613494)**  
**[Commit: [turbofan] Skip data-flow analysis of code entry field.](https://chromium.googlesource.com/v8/v8/+/dbd7d5a)**  
  
Date(Commit): Mon May 23 10:40:29 2016  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1997353002](https://codereview.chromium.org/1997353002)  
Regress: [mjsunit/regress/regress-crbug-613494.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613494.js)  
```javascript
function f() {
  var bound = 0;
  function g() { return bound }
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dbd7d5a^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=dbd7d5a)  
[src/compiler/escape-analysis.h](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.h?cl=dbd7d5a)  
[test/mjsunit/regress/regress-crbug-613494.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613494.js?cl=dbd7d5a)  
  

---   

## **regress-crbug-613570.js (chromium issue)**  
   
**[Issue 613570:
 AllowHeapAllocation::IsAllowed() in src/heap/heap-inl.h](https://crbug.com/613570)**  
**[Commit: [json] fix encoding change for two-byte gap strings.](https://chromium.googlesource.com/v8/v8/+/46aeb2a)**  
  
Date(Commit): Mon May 23 09:18:58 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1997003002](https://codereview.chromium.org/1997003002)  
Regress: [mjsunit/regress/regress-crbug-613570.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613570.js)  
```javascript
assertEquals("[\n\u26031,\n\u26032\n]",
             JSON.stringify([1, 2], null, "\u2603"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/46aeb2a^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=46aeb2a)  
[test/mjsunit/regress/regress-crbug-613570.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613570.js?cl=46aeb2a)  
  

---   

## **regress-crbug-613905.js (chromium issue)**  
   
**[Issue 613905:
 Crash in v8::base::NoBarrier_Load](https://crbug.com/613905)**  
**[Commit: [runtime] Don't crash when trying to access manually constructed CallSite object.](https://chromium.googlesource.com/v8/v8/+/a7a14fd)**  
  
Date(Commit): Mon May 23 09:01:29 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2006603002](https://codereview.chromium.org/2006603002)  
Regress: [mjsunit/regress/regress-crbug-613905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613905.js)  
```javascript
Error.prepareStackTrace = (e,s) => s;
var CallSiteConstructor = Error().stack[0].constructor;

try {
  (new CallSiteConstructor(3, 6)).toString();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7a14fd^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=a7a14fd)  
[test/mjsunit/regress/regress-crbug-613905.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613905.js?cl=a7a14fd)  
  

---   

## **regress-5033.js (v8 issue)**  
   
**[Issue 5033:
 Assertion failure in TypeFeedbackOracle::CollectReceiverTypes](https://crbug.com/v8/5033)**  
**[Commit: [crankshaft] Don't inline "dont_crankshaft" functions](https://chromium.googlesource.com/v8/v8/+/43547df)**  
  
Date(Commit): Fri May 20 15:20:15 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/2000703002](https://codereview.chromium.org/2000703002)  
Regress: [mjsunit/regress/regress-5033.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5033.js)  
```javascript
var test = function() {
  var t = Date.now();  // Just any non-constant double value.
  var o = {
    ['p']: 1,
    t
  };
};

function caller() {
  test();
}
caller();
caller();
%OptimizeFunctionOnNextCall(caller);
caller();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/43547df^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=43547df)  
[test/mjsunit/regress/regress-5033.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5033.js?cl=43547df)  
  

---   

## **regress-606021.js (chromium issue)**  
   
**[Issue 606021:
 deqp/functional/gles3/texturefiltering/cube-sizes-*.html flaky on a bunch of platforms](https://crbug.com/606021)**  
**[Commit: Bugfix: Crankshaft array literals with incorrect values.](https://chromium.googlesource.com/v8/v8/+/b71f1cc)**  
  
Date(Commit): Fri May 20 13:07:52 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "merge-merged-5.2", "Merge-rejected-5.1"]  
Code Review: [https://codereview.chromium.org/2000673002](https://codereview.chromium.org/2000673002)  
Regress: [mjsunit/regress/regress-606021.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-606021.js)  
```javascript
function foo() {
  return function(c) {
    var double_var = [3.0, 3.5][0];
    var literal = c ? [1, double_var] : [double_var, 3.5];
    return literal[0];
  };
}

var f1 = foo();
var f2 = foo();

f1(false);
f2(false);

%OptimizeFunctionOnNextCall(f1);
f1(false);

f2(true);

var l = f1(true);
assertEquals(1, l);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b71f1cc^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=b71f1cc)  
[test/mjsunit/regress/regress-606021.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-606021.js?cl=b71f1cc)  
  

---   

## **regress-5018.js (v8 issue)**  
   
**[Issue 5018:
 DataView accessor fast path incorrect after ES2015 prototype hierarchy changes](https://crbug.com/v8/5018)**  
**[Commit: Remove now-incorrect DataView accessor optimization](https://chromium.googlesource.com/v8/v8/+/de7d47e)**  
  
Date(Commit): Thu May 19 19:49:35 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1984043002](https://codereview.chromium.org/1984043002)  
Regress: [mjsunit/regress/regress-5018.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5018.js)  
```javascript
var dv = new DataView(new ArrayBuffer(4), 2);

function getByteLength(a) {
  return a.byteLength;
}

assertEquals(2, getByteLength(dv));
assertEquals(2, getByteLength(dv));

Object.defineProperty(dv.__proto__, 'byteLength', {value: 42});

assertEquals(42, dv.byteLength);
assertEquals(42, getByteLength(dv));

function getByteOffset(a) {
  return a.byteOffset;
}

assertEquals(2, getByteOffset(dv));
assertEquals(2, getByteOffset(dv));

Object.defineProperty(dv.__proto__, 'byteOffset', {value: 42});

assertEquals(42, dv.byteOffset);
assertEquals(42, getByteOffset(dv));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/de7d47e^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=de7d47e)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=de7d47e)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=de7d47e)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=de7d47e)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=de7d47e)  
...  
  

---   

## **regress-crbug-612142.js (chromium issue)**  
   
**[Issue 612142:
 !v8::internal::FLAG_enable_slow_asserts || (IsDereferenceAllowed(INCLUDE_DEFERRE](https://crbug.com/612142)**  
**[Commit: [turbofan] Kill type Guard nodes during effect/control linearization.](https://chromium.googlesource.com/v8/v8/+/33e571f)**  
  
Date(Commit): Wed May 18 05:38:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1980383002](https://codereview.chromium.org/1980383002)  
Regress: [mjsunit/regress/regress-crbug-612142.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-612142.js)  
```javascript
var thrower = {[Symbol.toPrimitive]: function(e) { throw e }};
try {
  for (var i = 0; i < 10; i++) { }
  for (var i = 0.5; i < 100000; ++i) { }
  for (var i = 16 | 0 || 0 || this || 1; i;) { String.fromCharCode(thrower); }
} catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/33e571f^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=33e571f)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=33e571f)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=33e571f)  
[src/compiler/instruction-selector.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.h?cl=33e571f)  
[test/mjsunit/regress/regress-crbug-612142.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-612142.js?cl=33e571f)  
  

---   

## **regress-crbug-602595.js (chromium issue)**  
   
**[Issue 602595:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsSmi()) in src/objects-inl.](https://crbug.com/602595)**  
**[Commit: [turbofan] Escape analysis treats guard nodes as escaping.](https://chromium.googlesource.com/v8/v8/+/7cef559)**  
  
Date(Commit): Tue May 17 15:47:35 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1972323004](https://codereview.chromium.org/1972323004)  
Regress: [mjsunit/regress/regress-crbug-602595.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-602595.js)  
```javascript
function f(a) { return [a] }

assertEquals([23], f(23));
assertEquals([42], f(42));
%OptimizeFunctionOnNextCall(f);
assertEquals([65], f(65));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7cef559^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=7cef559)  
[test/mjsunit/regress/regress-crbug-602595.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-602595.js?cl=7cef559)  
  

---   

## **regress-5006.js (v8 issue)**  
   
**[Issue 5006:
 Elliptic.js crashes on V8 5.2.290](https://crbug.com/v8/5006)**  
**[Commit: [turbofan] Fix optimized lowering of Math.imul.](https://chromium.googlesource.com/v8/v8/+/fa7460a)**  
  
Date(Commit): Thu May 12 18:43:32 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1971163002](https://codereview.chromium.org/1971163002)  
Regress: [mjsunit/regress/regress-5006.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5006.js)  
```javascript
function foo(x) { return Math.imul(x|0, 2); }
print(foo(1));
print(foo(1));
%OptimizeFunctionOnNextCall(foo);
print(foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa7460a^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=fa7460a)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=fa7460a)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=fa7460a)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=fa7460a)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=fa7460a)  
...  
  

---   

## **regress-4971.js (v8 issue)**  
   
**[Issue 4971:
 Deoptimizer::GetOutputInfo failure when handling stack overflow with Proxies/super constructor calls](https://crbug.com/v8/4971)**  
**[Commit: [fullcodegen] Add missing bailout points for super calls.](https://chromium.googlesource.com/v8/v8/+/afb69f7)**  
  
Date(Commit): Mon May 09 13:44:40 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1960083002](https://codereview.chromium.org/1960083002)  
Regress: [mjsunit/regress/regress-4971.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4971.js)  
```javascript
(function TestDeoptInNamedSuperGetter() {
  class C { m() { return 23 } }
  class D extends C { f() { return super.boom() } }

  var should_deoptimize_caller = false;
  Object.defineProperty(C.prototype, "boom", { get: function() {
    if (should_deoptimize_caller) %DeoptimizeFunction(D.prototype.f);
    return this.m
  }})

  assertEquals(23, new D().f());
  assertEquals(23, new D().f());
  %OptimizeFunctionOnNextCall(D.prototype.f);
  assertEquals(23, new D().f());
  should_deoptimize_caller = true;
  assertEquals(23, new D().f());
})();

(function TestDeoptInKeyedSuperGetter() {
  class C { m() { return 23 } }
  class D extends C { f(name) { return super[name]() } }

  var should_deoptimize_caller = false;
  Object.defineProperty(C.prototype, "boom", { get: function() {
    if (should_deoptimize_caller) %DeoptimizeFunction(D.prototype.f);
    return this.m
  }})

  assertEquals(23, new D().f("boom"));
  assertEquals(23, new D().f("boom"));
  %OptimizeFunctionOnNextCall(D.prototype.f);
  assertEquals(23, new D().f("boom"));
  should_deoptimize_caller = true;
  assertEquals(23, new D().f("boom"));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afb69f7^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=afb69f7)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=afb69f7)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=afb69f7)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=afb69f7)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=afb69f7)  
...  
  

---   

## **regress-crbug-610207.js (chromium issue)**  
   
**[Issue 610207:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsSharedFunctionInfo()) in s](https://crbug.com/610207)**  
**[Commit: Don't crash when load eval origin of a call site.](https://chromium.googlesource.com/v8/v8/+/8758245)**  
  
Date(Commit): Mon May 09 09:00:52 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1958043002](https://codereview.chromium.org/1958043002)  
Regress: [mjsunit/regress/regress-crbug-610207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-610207.js)  
```javascript
Error.prepareStackTrace = function(exception, frames) {
  return frames[0].getEvalOrigin();
}

try {
  Realm.eval(0, "throw new Error('boom');");
} catch(e) {
  print(e.stack);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8758245^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=8758245)  
[test/mjsunit/regress/regress-crbug-610207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-610207.js?cl=8758245)  
  

---   

## **regress-crbug-608278.js (chromium issue)**  
   
**[Issue 608278:
 1 == translation_size in src/crankshaft/lithium-codegen.cc](https://crbug.com/608278)**  
**[Commit: [es6] Properly handle the case when an inlined getter/setter/constructor does a tail call.](https://chromium.googlesource.com/v8/v8/+/e17a283)**  
  
Date(Commit): Fri May 06 12:37:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.1"]  
Code Review: [https://codereview.chromium.org/1936043002](https://codereview.chromium.org/1936043002)  
Regress: [mjsunit/regress/regress-crbug-608278.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-608278.js)  
```javascript
"use strict";

function h() {
  var stack = (new Error("boom")).stack;
  print(stack);
  %DeoptimizeFunction(f1);
  %DeoptimizeFunction(f2);
  %DeoptimizeFunction(f3);
  %DeoptimizeFunction(g);
  %DeoptimizeFunction(h);
  return 1;
}
%NeverOptimizeFunction(h);

function g(v) {
  return h();
}


function f1() {
  var o = {};
  o.__defineGetter__('p', g);
  o.p;
}

f1();
f1();
%OptimizeFunctionOnNextCall(f1);
f1();


function f2() {
  var o = {};
  o.__defineSetter__('q', g);
  o.q = 1;
}

f2();
f2();
%OptimizeFunctionOnNextCall(f2);
f2();


function A() {
  return h();
}

function f3() {
  new A();
}

f3();
f3();
%OptimizeFunctionOnNextCall(f3);
f3();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e17a283^!)  
[src/crankshaft/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-arm.cc?cl=e17a283)  
[src/crankshaft/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-arm64.cc?cl=e17a283)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=e17a283)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=e17a283)  
[src/crankshaft/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/lithium.cc?cl=e17a283)  
...  
  

---   

## **regress-crbug-608279.js (chromium issue)**  
   
**[Issue 608279:
 !info->shared_info()->feedback_vector()->metadata()->SpecDiffersFrom( info->lite](https://crbug.com/608279)**  
**[Commit: Don't treat catch scopes as possibly-shadowing for sloppy eval](https://chromium.googlesource.com/v8/v8/+/75f2d65)**  
  
Date(Commit): Wed May 04 21:36:13 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1950803002](https://codereview.chromium.org/1950803002)  
Regress: [mjsunit/regress/regress-crbug-608279.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-608279.js)  
```javascript
function __f_38() {
  try {
    throw 0;
  } catch (e) {
    eval();
    var __v_38 = { a: 'hest' };
    __v_38.m = function () { return __v_38.a; };
  }
  return __v_38;
}
var __v_40 = __f_38();
__v_40.m();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/75f2d65^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=75f2d65)  
[test/mjsunit/regress/regress-crbug-608279.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-608279.js?cl=75f2d65)  
  

---   

## **regress-608630.js (chromium issue)**  
   
**[Issue 608630:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/608630)**  
**[Commit: [wasm] Fix for 608630: allow proxies as FFI.](https://chromium.googlesource.com/v8/v8/+/f82b337)**  
  
Date(Commit): Wed May 04 08:54:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/1943313002](https://codereview.chromium.org/1943313002)  
Regress: [mjsunit/regress/regress-608630.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-608630.js)  
```javascript
var __v_5 = {};
var __v_35 = {};
var __v_44 = {};
var __v_43 = {};

try {
__v_1 = 1;
__v_2 = {
  get: function() { return function() {} },
  has() { return true },
  getOwnPropertyDescriptor: function() {
    if (__v_1-- == 0) throw "please die";
    return {value: function() {}, configurable: true};
  }
};
__v_3 = new Proxy({}, __v_2);
__v_30 = Object.create(__v_35);
with (__v_5) { f() }
} catch(e) { print("Caught: " + e); }

function __f_1(asmfunc, expect) {
  var __v_1 = asmfunc.toString();
  var __v_2 = __v_1.replace(new RegExp("use asm"), "");
  var __v_39 = {Math: Math};
  var __v_4 = eval("(" + __v_2 + ")")(__v_3);
  print("Testing " + asmfunc.name + " (js)...");
  __v_44.valueOf = __v_43;
  expect(__v_4);
  print("Testing " + asmfunc.name + " (asm.js)...");
  var __v_5 = asmfunc(__v_3);
  expect(__v_5);
  print("Testing " + asmfunc.name + " (wasm)...");
  var module_func = eval(__v_1);
  var __v_6 = module_func({}, __v_3);
  assertTrue(%IsAsmWasmCode(module_func));
  expect(__v_6);
}
function __f_2() {
  "use asm";
  function __f_3() { return 0; }
  function __f_4() { return 1; }
  function __f_5() { return 4; }
  function __f_6() { return 64; }
  function __f_7() { return 137; }
  function __f_8() { return 128; }
  function __f_9() { return -1; }
  function __f_10() { return 1000; }
  function __f_11() { return 2000000; }
  function __f_12() { return 2147483647; }
  return {__f_3: __f_3, __f_4: __f_4, __f_5: __f_5, __f_6: __f_6, __f_7: __f_7, __f_8: __f_8,
          __f_9: __f_9, __f_10: __f_10, __f_11, __f_12: __f_12};
}
try {
__f_1(__f_2, function(module) {
  assertEquals(0, module.__f_3());
  assertEquals(1, module.__f_4());
  assertEquals(4, module.__f_5());
  assertEquals(64, module.__f_6());
  assertEquals(128, module.__f_8());
  assertEquals(-1, module.__f_9());
  assertEquals(1000, module.__f_10());
  assertEquals(2000000, module.__f_11());
  assertEquals(2147483647, module.__f_12());
});
} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f82b337^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=f82b337)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=f82b337)  
[src/wasm/wasm-module.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.h?cl=f82b337)  
[test/mjsunit/regress/regress-608630.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-608630.js?cl=f82b337)  
  

---   

## **regress-crbug-609029.js (chromium issue)**  
   
**[Issue 609029:
 data()->IsUndefined() || data()->IsFixedArray() in v8/src/objects-debug.cc](https://crbug.com/609029)**  
**[Commit: [turbofan] Implement %_NewObject using FastNewObjectStub.](https://chromium.googlesource.com/v8/v8/+/c321837)**  
  
Date(Commit): Wed May 04 07:35:22 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1943403004](https://codereview.chromium.org/1943403004)  
Regress: [mjsunit/regress/regress-crbug-609029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-609029.js)  
```javascript
"xxx".match();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c321837^!)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=c321837)  
[test/mjsunit/regress/regress-crbug-609029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-609029.js?cl=c321837)  
  

---   

## **regress-4970.js (v8 issue)**  
   
**[Issue 4970:
 Check failed: is_sloppy(scope->language_mode()) || is_strict(info->language_mode()).](https://crbug.com/v8/4970)**  
**[Commit: Fix 'eval' in class extends clauses to be always-strict](https://chromium.googlesource.com/v8/v8/+/c8a342a)**  
  
Date(Commit): Tue May 03 22:36:29 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1931003003](https://codereview.chromium.org/1931003003)  
Regress: [mjsunit/regress/regress-4970.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4970.js)  
```javascript
function g() {
  var f;
  class C extends eval("f = () => delete C; Array") {}
  f();
}

assertThrows(g, SyntaxError);
%OptimizeFunctionOnNextCall(g);
assertThrows(g, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8a342a^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=c8a342a)  
[src/full-codegen/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.h?cl=c8a342a)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=c8a342a)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=c8a342a)  
[test/mjsunit/regress/regress-4970.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4970.js?cl=c8a342a)  
  

---   

## **regress-592352.js (chromium issue)**  
   
**[Issue 592352:
 Unreachable code in src/wasm/asm-wasm-builder.cc](https://crbug.com/592352)**  
**[Commit: [wasm] Disallow runtime calls in asm.js modules.](https://chromium.googlesource.com/v8/v8/+/d622c3a)**  
  
Date(Commit): Tue May 03 15:57:23 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1943373002](https://codereview.chromium.org/1943373002)  
Regress: [mjsunit/regress/regress-592352.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-592352.js)  
```javascript
function __f_76() {
  "use asm";
  function __f_72() {
    %OptimizeFunctionOnNextCall();
  }
  return {__f_72:__f_72};
}

try {
  assertTrue(%IsAsmWasmCode(__f_76));
  assertTrue(false);
} catch (e) {
  print("Caught: " + e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d622c3a^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=d622c3a)  
[test/mjsunit/regress/regress-592352.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-592352.js?cl=d622c3a)  
  

---   

## **regress-607493.js (chromium issue)**  
   
**[Issue 607493:
 Unreachable code in src/compiler/verifier.cc](https://crbug.com/607493)**  
**[Commit: [turbofan] Fix OSR environment in for-in.](https://chromium.googlesource.com/v8/v8/+/2da181b)**  
  
Date(Commit): Tue May 03 13:41:03 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1949433002](https://codereview.chromium.org/1949433002)  
Regress: [mjsunit/compiler/regress-607493.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-607493.js)  
```javascript
(function ForInTryCatchContrinueOsr() {
  var a = [1];

  function g() {
    for (var x in a) {
      try {
        for (var i = 0; i < 10; i++) { %OptimizeOsr(); }
        return;
      } catch(e) {
        continue;
      }
    }
  }

  g();
})();

(function ForInContinueNestedOsr() {
  var a = [1];

  function g() {
    for (var x in a) {
      if (x) {
        for (var i = 0; i < 10; i++) { %OptimizeOsr(); }
      }
      continue;
    }
  }

  g();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2da181b^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=2da181b)  
[test/mjsunit/compiler/regress-607493.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-607493.js?cl=2da181b)  
  

---   

## **regress-crbug-604299.js (chromium issue)**  
   
**[Issue 604299:
 RUNTIME_ASSERT in args[0]->IsString() in src/runtime/runtime-regexp.cc](https://crbug.com/604299)**  
**[Commit: Use InternalArrays from certain Intl code](https://chromium.googlesource.com/v8/v8/+/4f374bb)**  
  
Date(Commit): Mon May 02 18:19:25 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1923803002](https://codereview.chromium.org/1923803002)  
Regress: [mjsunit/regress/regress-crbug-604299.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-604299.js)  
```javascript
Array.prototype.__defineSetter__(0,function(value){});

if (this.Intl) {
  var o = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Katmandu'})
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f374bb^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4f374bb)  
[src/js/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/js/typedarray.js?cl=4f374bb)  
[test/mjsunit/regress/regress-crbug-604299.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-604299.js?cl=4f374bb)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=4f374bb)  
  

---   

## **regress-4967.js (v8 issue)**  
   
**[Issue 4967:
 Check failed: !(!HasStackOverflow()) || (operand_stack_depth_ >= count)](https://crbug.com/v8/4967)**  
**[Commit: [full-codegen] Fix stack depth tracking when reporting unsupported super usages](https://chromium.googlesource.com/v8/v8/+/567aa1b)**  
  
Date(Commit): Mon May 02 17:28:54 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1929213002](https://codereview.chromium.org/1929213002)  
Regress: [mjsunit/regress/regress-4967.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4967.js)  
```javascript
assertThrows(() => {
  new class extends Object {
    constructor() { (() => delete super[super()])(); }
  }
}, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/567aa1b^!)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=567aa1b)  
[test/mjsunit/regress/regress-4967.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4967.js?cl=567aa1b)  
  

---   

## **regress-v8-4972.js (v8 issue)**  
   
**[Issue 4972:
 Check failed: (value & kHeapObjectTag) == 0.](https://crbug.com/v8/4972)**  
**[Commit: [runtime] Don't crash when creating an instance of a class inherited from a Proxy.](https://chromium.googlesource.com/v8/v8/+/b83edcc)**  
  
Date(Commit): Fri Apr 29 15:07:35 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1925803005](https://codereview.chromium.org/1925803005)  
Regress: [mjsunit/regress/regress-v8-4972.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-4972.js)  
```javascript
new class extends new Proxy(class {},{}) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b83edcc^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b83edcc)  
[test/mjsunit/regress/regress-v8-4972.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-4972.js?cl=b83edcc)  
  

---   

## **regress-4964.js (v8 issue)**  
   
**[Issue 4964:
 Illegal access with ArrayBuffer.prototype.slice and detached buffer](https://crbug.com/v8/4964)**  
**[Commit: Add checks for detached ArrayBuffers to ArrayBuffer.prototype.slice](https://chromium.googlesource.com/v8/v8/+/3d66e5d)**  
  
Date(Commit): Thu Apr 28 22:50:56 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1929123002](https://codereview.chromium.org/1929123002)  
Regress: [mjsunit/regress/regress-4964.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4964.js)  
```javascript
var ab = new ArrayBuffer(10);
ab.constructor = { get [Symbol.species]() { %ArrayBufferDetach(ab); return ArrayBuffer; } };
assertThrows(() => ab.slice(0), TypeError);

class DetachedArrayBuffer extends ArrayBuffer {
  constructor(...args) {
    super(...args);
    %ArrayBufferDetach(this);
  }
}

var ab2 = new ArrayBuffer(10);
ab2.constructor = DetachedArrayBuffer;
assertThrows(() => ab2.slice(0), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d66e5d^!)  
[src/js/arraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/js/arraybuffer.js?cl=3d66e5d)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=3d66e5d)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=3d66e5d)  
[test/cctest/interpreter/bytecode_expectations/ForOf.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForOf.golden?cl=3d66e5d)  
[test/cctest/interpreter/bytecode_expectations/Generators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Generators.golden?cl=3d66e5d)  
...  
  

---   

## **regress-recurse-patch-binary-op.js (other issue)**  
   
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
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=f1cc6e6)  
[test/mjsunit/regress/regress-recurse-patch-binary-op.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-recurse-patch-binary-op.js?cl=f1cc6e6)  
  
  
---   

## **regress-4945.js (v8 issue)**  
   
**[Issue 4945:
 Fix "in" operator parsing.](https://crbug.com/v8/4945)**  
**[Commit: Forward accept_IN to ParseYieldExpression](https://chromium.googlesource.com/v8/v8/+/967a046)**  
  
Date(Commit): Tue Apr 26 17:24:49 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1916183003](https://codereview.chromium.org/1916183003)  
Regress: [mjsunit/regress/regress-4945.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4945.js)  
```javascript
function* g(o) {
  yield 'x' in o;
}

assertTrue(g({x: 1}).next().value);
assertFalse(g({}).next().value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/967a046^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=967a046)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=967a046)  
[test/mjsunit/regress/regress-4945.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4945.js?cl=967a046)  
  

---   

## **regress-object-assign-deprecated-2.js (other issue)**  
   
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
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=1678bb5)  
[test/mjsunit/regress/regress-object-assign-deprecated-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-object-assign-deprecated-2.js?cl=1678bb5)  
[test/mjsunit/regress/regress-object-assign-deprecated.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-object-assign-deprecated.js?cl=1678bb5)  
  
  
---   

## **regress-object-assign-deprecated.js (other issue)**  
   
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
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=1678bb5)  
[test/mjsunit/regress/regress-object-assign-deprecated-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-object-assign-deprecated-2.js?cl=1678bb5)  
[test/mjsunit/regress/regress-object-assign-deprecated.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-object-assign-deprecated.js?cl=1678bb5)  
  
  
---   

## **regress-crbug-605862.js (chromium issue)**  
   
**[Issue 605862:
 Unreachable code in src/regexp/jsregexp.h](https://crbug.com/605862)**  
**[Commit: [regexp] Fix non-match and max match length in RegExpCharacterClass.](https://chromium.googlesource.com/v8/v8/+/6f67d17)**  
  
Date(Commit): Mon Apr 25 13:32:14 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1916763002](https://codereview.chromium.org/1916763002)  
Regress: [mjsunit/regress/regress-crbug-605862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-605862.js)  
```javascript
/[]*1/u.exec("\u1234");
/[^\u0000-\u{10ffff}]*1/u.exec("\u1234");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f67d17^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=6f67d17)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=6f67d17)  
[test/mjsunit/regress/regress-crbug-605862.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-605862.js?cl=6f67d17)  
  

---   

## **regress-605470.js (chromium issue)**  
   
**[Issue 605470:
 Crash in v8::internal::Invoke](https://crbug.com/605470)**  
**[Commit: [Interpreter] Fix incorrect Register OperandSize calculation for ExtraWide.](https://chromium.googlesource.com/v8/v8/+/11e3ba3)**  
  
Date(Commit): Fri Apr 22 10:32:14 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-3500", "M-52", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "reward-inprocess"]  
Code Review: [https://codereview.chromium.org/1908033002](https://codereview.chromium.org/1908033002)  
Regress: [mjsunit/regress/regress-605470.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-605470.js)  
```javascript
function function_with_m_args(m) {
  var source = '(function f() { return; })(';
  for (var arg = 0; arg < m ; arg++) {
    if (arg != 0) source += ',';
    source += arg;
  }
  source += ')';
  return eval(source);
}

function_with_m_args(0x7FFF);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11e3ba3^!)  
[src/interpreter/bytecodes.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecodes.cc?cl=11e3ba3)  
[test/mjsunit/regress/regress-605470.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-605470.js?cl=11e3ba3)  
  

---   

## **regress-crbug-602184.js (chromium issue)**  
   
**[Issue 602184:
 DICTIONARY_ELEMENTS == elements_kind in src/code-stubs.h](https://crbug.com/602184)**  
**[Commit: [tests] Add testcase for r35397](https://chromium.googlesource.com/v8/v8/+/f4a9a50)**  
  
Date(Commit): Fri Apr 22 09:08:46 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "M-50", "Reproducible", "Clusterfuzz", "M-51"]  
Code Review: [https://codereview.chromium.org/1912443004](https://codereview.chromium.org/1912443004)  
Regress: [mjsunit/regress/regress-crbug-602184.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-602184.js)  
```javascript
function f(test, a) {
  var v;
  if (test) {
    v = v|0;
  }
  a[v] = 1;
}
var v = new String();
f(false, v);
f(false, v);

v = new Int32Array(10);
f(true, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f4a9a50^!)  
[test/mjsunit/regress/regress-crbug-602184.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-602184.js?cl=f4a9a50)  
  

---   

## **regress-crbug-594183.js (chromium issue)**  
   
**[Issue 594183:
 Performance Regression: 5-20% WebGLAquarium 1000 Fishes time across most boards](https://crbug.com/594183)**  
**[Commit: [ic] Restore PROPERTY key tracking in keyed ICs](https://chromium.googlesource.com/v8/v8/+/9bebebd)**  
  
Date(Commit): Thu Apr 21 13:18:28 2016  
Components/Type: Blink>JavaScript>Runtime/Bug-Regression  
Labels: ["Hotlist-Merge-Review", "M-50", "Performance", "Cros-Perf-Monitor", "Hotlist-Merge-Approved", "ReleaseBlock-Stable", "merge-merged-50", "merge-merged-5.0", "merge-merged-5.1", "Merge-merged-51", "VerifyIn-53", "VerifyIn-54", "Bulk-Verified"]  
Code Review: [https://codereview.chromium.org/1912593002](https://codereview.chromium.org/1912593002)  
Regress: [mjsunit/regress/regress-crbug-594183.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594183.js)  
```javascript
var global = {}

var fish = [
  {'name': 'foo'},
  {'name': 'bar'},
];

for (var i = 0; i < fish.length; i++) {
  global[fish[i].name] = 1;
}

function load() {
  var sum = 0;
  for (var i = 0; i < fish.length; i++) {
    var name = fish[i].name;
    sum += global[name];
  }
  return sum;
}

load();
load();
%OptimizeFunctionOnNextCall(load);
load();
assertOptimized(load);

function store() {
  for (var i = 0; i < fish.length; i++) {
    var name = fish[i].name;
    global[name] = 1;
  }
}

store();
store();
%OptimizeFunctionOnNextCall(store);
store();
assertOptimized(store);


function store_element(obj, key) {
  obj[key] = 0;
}

var o1 = new Array(3);
var o2 = new Array(3);
o2.o2 = "o2";
var o3 = new Array(3);
o3.o3 = "o3";
var o4 = new Array(3);
o4.o4 = "o4";
var o5 = new Array(3);
o5.o5 = "o5";
store_element(o1, 0);  // Premonomorphic
store_element(o1, 0);  // Monomorphic
store_element(o2, 0);  // 2-way polymorphic.
store_element(o3, 0);  // 3-way polymorphic.
store_element(o4, 0);  // 4-way polymorphic.
store_element(o5, 0);  // Megamorphic.

function inferrable_store(key) {
  store_element(o5, key);
}

inferrable_store(0);
inferrable_store(0);
%OptimizeFunctionOnNextCall(inferrable_store);
inferrable_store(0);
assertOptimized(inferrable_store);
inferrable_store("deopt");

if (!isTurboFanned(inferrable_store)) {
  assertUnoptimized(inferrable_store);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bebebd^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=9bebebd)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=9bebebd)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=9bebebd)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=9bebebd)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=9bebebd)  
...  
  

---   

## **regress-opt-typeof-null.js (other issue)**  
   
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
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=7dfb5be)  
[test/mjsunit/regress/regress-opt-typeof-null.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-opt-typeof-null.js?cl=7dfb5be)  
  
  
---   

## **regress-crbug-605060.js (chromium issue)**  
   
**[Issue 605060:
 map->is_stable() in v8/src/compilation-dependencies.cc](https://crbug.com/605060)**  
**[Commit: Make sure we always try to make prototypes fast again when transitioning accessors](https://chromium.googlesource.com/v8/v8/+/4a6a0f5)**  
  
Date(Commit): Thu Apr 21 11:18:08 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1907953002](https://codereview.chromium.org/1907953002)  
Regress: [mjsunit/regress/regress-crbug-605060.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-605060.js)  
```javascript
Array.prototype.__defineGetter__('map', function(){});
Array.prototype.__defineGetter__('map', function(){});
Array.prototype.__defineGetter__('map', function(){});
assertTrue(%HasFastProperties(Array.prototype));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a6a0f5^!)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=4a6a0f5)  
[test/mjsunit/regress/regress-crbug-605060.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-605060.js?cl=4a6a0f5)  
  

---   

## **regress-crbug-604680.js (chromium issue)**  
   
**[Issue 604680:
 !removed || frame->LookupCode()->marked_for_deoptimization() in src/isolate.cc](https://crbug.com/604680)**  
**[Commit: [deoptimizer] Do not modify stack_fp which is used as a key for lookup of previously materialized objects.](https://chromium.googlesource.com/v8/v8/+/b4dbb2f)**  
  
Date(Commit): Thu Apr 21 09:54:33 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-51", "merge-merged-5.1"]  
Code Review: [https://codereview.chromium.org/1904663003](https://codereview.chromium.org/1904663003)  
Regress: [mjsunit/regress/regress-crbug-604680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-604680.js)  
```javascript
function h() {
  var res = g.arguments;
  return res;
}

function g(o) {
  var res = h();
  return res;
}

function f1() {
  var o = { x : 42 };
  var res = g(o);
  return 1;
}

function f0(a, b)  {
  "use strict";
  return f1(5);
}

function boom(b) {
  if (b) throw new Error("boom!");
}

%NeverOptimizeFunction(h);
f0();
f0();
%OptimizeFunctionOnNextCall(f0);

boom(false);
boom(false);
%OptimizeFunctionOnNextCall(boom);

try {
  f0(1, 2, 3);
  boom(true, 1, 2, 3);
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4dbb2f^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=b4dbb2f)  
[test/mjsunit/regress/regress-crbug-604680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-604680.js?cl=b4dbb2f)  
  

---   

## **regress-4908.js (v8 issue)**  
   
**[Issue 4908:
 Crash related to arrow function parameters](https://crbug.com/v8/4908)**  
**[Commit: More accurately record an end position for default parameters in arrows](https://chromium.googlesource.com/v8/v8/+/e96cbdc)**  
  
Date(Commit): Wed Apr 20 20:49:16 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1900343002](https://codereview.chromium.org/1900343002)  
Regress: [mjsunit/regress/regress-4908.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4908.js)  
```javascript
(function() { ((s = 17, y = s) => s)() })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e96cbdc^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=e96cbdc)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=e96cbdc)  
[test/mjsunit/regress/regress-4908.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4908.js?cl=e96cbdc)  
  

---   

## **regress-599719.js (chromium issue)**  
   
**[Issue 599719:
 result == fixed_size_from_fp + (stack_slots * kPointerSize) - CommonFrameConstan](https://crbug.com/599719)**  
**[Commit: [turbofan] Length and index2 are unsigned in CheckedLoad/CheckedStore.](https://chromium.googlesource.com/v8/v8/+/b994ad4)**  
  
Date(Commit): Wed Apr 20 09:35:06 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1858323003](https://codereview.chromium.org/1858323003)  
Regress: [mjsunit/regress/regress-599719.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599719.js)  
```javascript
function __f_7() {
    %DeoptimizeFunction(__f_5);
}
function __f_8(global, env) {
  "use asm";
  var __f_7 = env.__f_7;
  function __f_9(i4, i5) {
    i4 = i4 | 0;
    i5 = i5 | 0;
    __f_7();
  }
  return {__f_9: __f_9}
}
function __f_5() {
  var __v_5 = __f_8({}, {'__f_7': __f_7});
  assertTrue(%IsAsmWasmCode(__f_8));
  __v_5.__f_9(0, 0, 0);
}
__f_5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b994ad4^!)  
[src/compiler/code-generator-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator-impl.h?cl=b994ad4)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=b994ad4)  
[test/cctest/cctest.gyp](https://cs.chromium.org/chromium/src/v8/test/cctest/cctest.gyp?cl=b994ad4)  
[test/cctest/compiler/test-run-load-store.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-load-store.cc?cl=b994ad4)  
[test/cctest/compiler/test-run-machops.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-machops.cc?cl=b994ad4)  
...  
  

---   

## **regress-599717.js (chromium issue)**  
   
**[Issue 599717:
 index2 <= length in src/compiler/x64/code-generator-x64.cc](https://crbug.com/599717)**  
**[Commit: [turbofan] Length and index2 are unsigned in CheckedLoad/CheckedStore.](https://chromium.googlesource.com/v8/v8/+/b994ad4)**  
  
Date(Commit): Wed Apr 20 09:35:06 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1858323003](https://codereview.chromium.org/1858323003)  
Regress: [mjsunit/regress/regress-599717.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599717.js)  
```javascript
function __f_61(stdlib, foreign, buffer) {
  "use asm";
  var __v_14 = new stdlib.Float64Array(buffer);
  function __f_74() {
    var __v_35 = 6.0;
    __v_14[2] = __v_35 + 1.0;
  }
  return {__f_74: __f_74};
}
var ok = false;
try {
  var __v_12 = new ArrayBuffer(2147483648);
  ok = true;
} catch (e) {
  // Can happen on 32 bit systems.
}
if (ok) {
  var module = __f_61(this, null, __v_12);
  assertTrue(%IsAsmWasmCode(__f_61));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b994ad4^!)  
[src/compiler/code-generator-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator-impl.h?cl=b994ad4)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=b994ad4)  
[test/cctest/cctest.gyp](https://cs.chromium.org/chromium/src/v8/test/cctest/cctest.gyp?cl=b994ad4)  
[test/cctest/compiler/test-run-load-store.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-load-store.cc?cl=b994ad4)  
[test/cctest/compiler/test-run-machops.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-machops.cc?cl=b994ad4)  
...  
  

---   

## **regress-604044.js (chromium issue)**  
   
**[Issue 604044:
 [v8] Segmentation fault from preparser](https://crbug.com/604044)**  
**[Commit: Prevent un-parsed LiteralFunction reaching the compiler.](https://chromium.googlesource.com/v8/v8/+/ed9b7d9)**  
  
Date(Commit): Wed Apr 20 09:35:05 2016  
Components/Type: None/Bug  
Labels: ["Stability-Memory-AddressSanitizer"]  
Code Review: [https://codereview.chromium.org/1895123002](https://codereview.chromium.org/1895123002)  
Regress: [mjsunit/regress/regress-604044.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-604044.js)  
```javascript
(function(_ = function() {}){})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ed9b7d9^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=ed9b7d9)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=ed9b7d9)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=ed9b7d9)  
[test/mjsunit/regress-604044.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-604044.js?cl=ed9b7d9)  
  

---   

## **regress-crbug-603463.js (chromium issue)**  
   
**[Issue 603463:
 marker->IsSmi() in src/frames.cc](https://crbug.com/603463)**  
**[Commit: Fix polymorphic keyed load handler selection for proxies.](https://chromium.googlesource.com/v8/v8/+/2811388)**  
  
Date(Commit): Tue Apr 19 08:58:43 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "M-53", "Clusterfuzz", "MovedFrom-52", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/1894203002](https://codereview.chromium.org/1894203002)  
Regress: [mjsunit/regress/regress-crbug-603463.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-603463.js)  
```javascript
function load(a, i) {
  return a[i];
}

function f() {
  return load(new Proxy({}, {}), undefined);
}

f();
f();
load([11, 22, 33], 0);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2811388^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=2811388)  
[test/mjsunit/regress/regress-crbug-603463.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-603463.js?cl=2811388)  
  

---   

## **regress-602970.js (chromium issue)**  
   
**[Issue 602970:
 Security: type confusion lead to information leak in decodeURI](https://crbug.com/602970)**  
**[Commit: Security: type confusion lead to information leak in decodeURI](https://chromium.googlesource.com/v8/v8/+/4014504)**  
  
Date(Commit): Fri Apr 15 13:09:45 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-50", "Security_Impact-Stable", "Security_Severity-Medium", "reward-7500", "allpublic", "reward-inprocess", "M-51", "merge-merged-5.1", "Release-0-M51", "CVE-2016-1677", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/1889133003](https://codereview.chromium.org/1889133003)  
Regress: [mjsunit/regress/regress-602970.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-602970.js)  
```javascript
var num = new Number(10);
Array.prototype.__defineGetter__(0,function(){
    return num;
})
Array.prototype.__defineSetter__(0,function(value){
})
var str=decodeURI("%E7%9A%84");
assertEquals(0x7684, str.charCodeAt(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4014504^!)  
[src/js/uri.js](https://cs.chromium.org/chromium/src/v8/src/js/uri.js?cl=4014504)  
[test/mjsunit/regress/regress-602970.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-602970.js?cl=4014504)  
  

---   

## **regress-crbug-600257.js (chromium issue)**  
   
**[Issue 600257:
 unicode() in src/regexp/regexp-parser.cc](https://crbug.com/600257)**  
**[Commit: [regexp] fix assertion failure when parsing close to stack overflow.](https://chromium.googlesource.com/v8/v8/+/93135d8)**  
  
Date(Commit): Thu Apr 14 14:44:28 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1884143002](https://codereview.chromium.org/1884143002)  
Regress: [mjsunit/regress/regress-crbug-600257.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-600257.js)  
```javascript
(function rec() {
  try {
    rec();
  } catch (e) {
    /{/;
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93135d8^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=93135d8)  
[test/mjsunit/regress/regress-crbug-600257.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-600257.js?cl=93135d8)  
  

---   

## **regress-crbug-601617.js (chromium issue)**  
   
**[Issue 601617:
 frames_[0].kind() == TranslatedFrame::kFunction || frames_[0].kind() == Translat](https://crbug.com/601617)**  
**[Commit: [deoptimizer] Extend assert to also expect kTailCallerFunction as bottommost frame when accessing arguments for inlined function.](https://chromium.googlesource.com/v8/v8/+/26c480d)**  
  
Date(Commit): Mon Apr 11 12:20:37 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz", "M-51", "merge-merged-5.1"]  
Code Review: [https://codereview.chromium.org/1876753002](https://codereview.chromium.org/1876753002)  
Regress: [mjsunit/regress/regress-crbug-601617.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-601617.js)  
```javascript
function h() {
  var res = g.arguments[0].x;
  return res;
}

function g(o) {
  var res = h();
  return res;
}

function f1() {
  var o = { x : 1 };
  var res = g(o);
  return res;
}

function f0() {
  "use strict";
  return f1(5);
}

%NeverOptimizeFunction(h);
f0();
f0();
%OptimizeFunctionOnNextCall(f0);
assertEquals(1, f0());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/26c480d^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=26c480d)  
[test/mjsunit/regress/regress-crbug-601617.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-601617.js?cl=26c480d)  
  

---   

## **regress-599068-func-bindings.js (chromium issue)**  
   
**[Issue 599068:
 !is_strict(language_mode()) in src/interpreter/bytecode-generator.cc](https://crbug.com/599068)**  
**[Commit: [Interpreter] Handles legacy constants in strict mode.](https://chromium.googlesource.com/v8/v8/+/8982cb5)**  
  
Date(Commit): Mon Apr 11 12:04:01 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1845223006](https://codereview.chromium.org/1845223006)  
Regress: [mjsunit/regress/regress-599068-func-bindings.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599068-func-bindings.js)  
```javascript
(function f() {
  function assignSloppy() {
    f = 0;
  }
  assertDoesNotThrow(assignSloppy);

  function assignStrict() {
    'use strict';
    f = 0;
  }
  assertThrows(assignStrict, TypeError);

  function assignStrictLookup() {
    eval("'use strict'; f = 1;");
  }
  assertThrows(assignStrictLookup, TypeError);
})();

(function f() {
  function assignSloppy() {
    f += "x";
  }
  assertDoesNotThrow(assignSloppy);
  assertDoesNotThrow(assignSloppy);
  %OptimizeFunctionOnNextCall(assignSloppy);
  assertDoesNotThrow(assignSloppy);

  function assignStrict() {
    'use strict';
    f += "x";
  }
  assertThrows(assignStrict, TypeError);
  assertThrows(assignStrict, TypeError);
  %OptimizeFunctionOnNextCall(assignStrict);
  assertThrows(assignStrict, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8982cb5^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=8982cb5)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=8982cb5)  
[test/mjsunit/regress/regress-599068-func-bindings.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-599068-func-bindings.js?cl=8982cb5)  
  

---   

## **regress-600593.js (chromium issue)**  
   
**[Issue 600593:
 Crash in v8::internal::Invoke](https://crbug.com/600593)**  
**[Commit: [turbofan] Remove some clever-but-wrong bits from select lowering.](https://chromium.googlesource.com/v8/v8/+/03975be)**  
  
Date(Commit): Fri Apr 08 08:26:13 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "M-50", "Reproducible", "Stability-Memory-AddressSanitizer", "Hotlist-Merge-Approved", "Clusterfuzz", "M-51", "merge-merged-5.1"]  
Code Review: [https://codereview.chromium.org/1870763003](https://codereview.chromium.org/1870763003)  
Regress: [mjsunit/compiler/regress-600593.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-600593.js)  
```javascript
"use strict"

function f(c) {
  if (c) { throw new Error(); }
  throw new Error();
};

function Error()  {
  return arguments.length;
}

%PrepareFunctionForOptimization(f);
assertThrows(function() { f(true); });
assertThrows(function() { f(false); });
%OptimizeFunctionOnNextCall(f);
assertThrows(function() { f(true); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/03975be^!)  
[src/compiler/select-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/select-lowering.cc?cl=03975be)  
[src/compiler/select-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/select-lowering.h?cl=03975be)  
[test/mjsunit/compiler/regress-600593.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-600593.js?cl=03975be)  
[test/unittests/compiler/select-lowering-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/select-lowering-unittest.cc?cl=03975be)  
[test/unittests/unittests.gyp](https://cs.chromium.org/chromium/src/v8/test/unittests/unittests.gyp?cl=03975be)  
  

---   

## **regress-585041.js (chromium issue)**  
   
**[Issue 585041:
 Smi::IsValid(value) in src/objects.h](https://crbug.com/585041)**  
**[Commit: Bugfix: assert in lithium compile for LMaybeGrowElements](https://chromium.googlesource.com/v8/v8/+/ce1fe78)**  
  
Date(Commit): Thu Apr 07 11:41:39 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1854423003](https://codereview.chromium.org/1854423003)  
Regress: [mjsunit/regress/regress-585041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-585041.js)  
```javascript
function f(arr, i) {
  arr[i] = 50;
}

function boom(dummy) {
  var arr = new Array(10);
  f(arr, 10);
  if (dummy) {
    f(arr, -2147483648);
  }
}

boom(false);
%OptimizeFunctionOnNextCall(boom);
boom(false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce1fe78^!)  
[src/crankshaft/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-codegen-arm.cc?cl=ce1fe78)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=ce1fe78)  
[src/crankshaft/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips/lithium-codegen-mips.cc?cl=ce1fe78)  
[src/crankshaft/mips64/lithium-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips64/lithium-codegen-mips64.cc?cl=ce1fe78)  
[test/mjsunit/regress/regress-585041.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-585041.js?cl=ce1fe78)  
  

---   

## **regress-599710.js (chromium issue)**  
   
**[Issue 599710:
 right_block->HasPredecessor() in v8/src/crankshaft/hydrogen.cc](https://crbug.com/599710)**  
**[Commit: [crankshaft] Make infinite loops preserve control flow.](https://chromium.googlesource.com/v8/v8/+/3df0a8c)**  
  
Date(Commit): Thu Apr 07 05:36:44 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.1"]  
Code Review: [https://codereview.chromium.org/1863123002](https://codereview.chromium.org/1863123002)  
Regress: [mjsunit/regress/regress-599710.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599710.js)  
```javascript
var f1 = function() { while (1) { } }

function g1() {
  var s = "hey";
  f1 = function() { return true; }
  if (f1()) { return s; }
}

%OptimizeFunctionOnNextCall(g1);
assertEquals("hey", g1());

var f2 = function() { do { } while (1); }

function g2() {
  var s = "hey";
  f2 = function() { return true; }
  if (f2()) { return s; }
}

%OptimizeFunctionOnNextCall(g2);
assertEquals("hey", g2());

var f3 = function() { for (;;); }

function g3() {
  var s = "hey";
  f3 = function() { return true; }
  if (f3()) { return s; }
}

%OptimizeFunctionOnNextCall(g3);
assertEquals("hey", g3());

var f4 = function() { for (;;); }

function g4() {
  var s = "hey";
  f4 = function() { return true; }
  while (f4()) { return s; }
}

%OptimizeFunctionOnNextCall(g4);
assertEquals("hey", g4());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3df0a8c^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3df0a8c)  
[test/mjsunit/regress/regress-599710.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-599710.js?cl=3df0a8c)  
  

---   

## **regress-594084.js (chromium issue)**  
   
**[Issue 594084:
 Crash in v8::internal::AstExpressionVisitor::VisitStatements](https://crbug.com/594084)**  
**[Commit: [destructuring] don't attempt to visit contents of FunctionLiterals](https://chromium.googlesource.com/v8/v8/+/f60048c)**  
  
Date(Commit): Tue Apr 05 18:43:17 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Stability-Crash", "M-50", "Reproducible", "Stability-Memory-AddressSanitizer", "findit-wrong", "M-49", "Clusterfuzz", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1864553002](https://codereview.chromium.org/1864553002)  
Regress: [mjsunit/es6/regress/regress-594084.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-594084.js)  
```javascript
(function() {
  function CRASH(defaultParameter =
      (function() { function functionDeclaration() { return 0; } }())) {
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f60048c^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=f60048c)  
[test/mjsunit/es6/regress/regress-594084.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-594084.js?cl=f60048c)  
  

---   

## **regress-599825.js (chromium issue)**  
   
**[Issue 599825:
 Crash in v8::internal::AsmTyper::VisitHeapAccess](https://crbug.com/599825)**  
**[Commit: [asm.js] Fix typing bug for non-literals in heap access.](https://chromium.googlesource.com/v8/v8/+/77a8c2e)**  
  
Date(Commit): Tue Apr 05 17:24:03 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/1858263002](https://codereview.chromium.org/1858263002)  
Regress: [mjsunit/regress/regress-599825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599825.js)  
```javascript
function __f_97(stdlib, buffer) {
  "use asm";
  var __v_30 = new stdlib.Int32Array(buffer);
  function __f_74() {
    var __v_27 = 4;
    __v_30[__v_27 >> __v_2] = ((__v_30[-1073741825]|-10) + 2) | 0;
  }
}
var module = __f_97(this);
assertFalse(%IsAsmWasmCode(__f_97));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77a8c2e^!)  
[src/typing-asm.cc](https://cs.chromium.org/chromium/src/v8/src/typing-asm.cc?cl=77a8c2e)  
[test/mjsunit/regress/regress-599825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-599825.js?cl=77a8c2e)  
  

---   

## **regress-crbug-596394.js (chromium issue)**  
   
**[Issue 596394:
 has_property_ in src/lookup.h](https://crbug.com/596394)**  
**[Commit: Ensure CreateDataProperty works correctly on TypedArrays](https://chromium.googlesource.com/v8/v8/+/7a38462)**  
  
Date(Commit): Tue Apr 05 16:56:12 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1821723004](https://codereview.chromium.org/1821723004)  
Regress: [mjsunit/regress/regress-crbug-596394.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-596394.js)  
```javascript
__v_0 = new Uint8Array(100);
array = new Array(10);
array.__proto__ = __v_0;
assertThrows(() => Array.prototype.concat.call(array), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a38462^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7a38462)  
[test/mjsunit/regress/regress-crbug-596394.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-596394.js?cl=7a38462)  
  

---   

## **regress-599414-array-concat-fast-path.js (chromium issue)**  
   
**[Issue 599414:
 result_len <= FixedDoubleArray::kMaxLength in src/elements.cc](https://crbug.com/599414)**  
**[Commit: [elements] Fix length bounds precheck for Array.prototype.concat](https://chromium.googlesource.com/v8/v8/+/823224f)**  
  
Date(Commit): Tue Apr 05 15:35:27 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1863553003](https://codereview.chromium.org/1863553003)  
Regress: [mjsunit/regress/regress-599414-array-concat-fast-path.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599414-array-concat-fast-path.js)  
```javascript
var largeArray = 'x'.repeat(999).split('');
var a = largeArray;

assertThrows(() => {
  for (;;) {
    a = a.concat(a, a, a, a, a);
  }}, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/823224f^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=823224f)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=823224f)  
[test/mjsunit/regress/regress-599414-array-concat-fast-path.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-599414-array-concat-fast-path.js?cl=823224f)  
  

---   

## **regress-599412.js (chromium issue)**  
   
**[Issue 599412:
 RUNTIME_ASSERT in RepresentationChangerError: node #195:HeapConstant of kRepTagged (Constant(ADDRE](https://crbug.com/599412)**  
**[Commit: [turbofan] Restrict types in load elimination.](https://chromium.googlesource.com/v8/v8/+/4142bc6)**  
  
Date(Commit): Tue Apr 05 12:30:14 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1857133003](https://codereview.chromium.org/1857133003)  
Regress: [mjsunit/regress/regress-599412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-599412.js)  
```javascript
function h(a) {
  if (!a) return false;
  print();
}

function g(a) { return a.length; }
g('0');
g('1');

function f() {
  h(g([]));
}

f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4142bc6^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=4142bc6)  
[src/compiler/load-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.h?cl=4142bc6)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=4142bc6)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=4142bc6)  
[test/mjsunit/regress/regress-599412.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-599412.js?cl=4142bc6)  
...  
  

---   

## **regress-4759.js (v8 issue)**  
   
**[Issue 4759:
 Rest element does not accept valid user-defined iterables](https://crbug.com/v8/4759)**  
**[Commit: Fix treatment of rest pattern in array destructuring.](https://chromium.googlesource.com/v8/v8/+/4edf16d)**  
  
Date(Commit): Tue Apr 05 08:56:51 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1852703002](https://codereview.chromium.org/1852703002)  
Regress: [mjsunit/es6/regress/regress-4759.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4759.js)  
```javascript
function iterable(done) {
  return {
    [Symbol.iterator]: function() {
      return {
        next: function() {
          if (done) return { done: true };
          done = true;
          return { value: 42, done: false };
        }
      }
    }
  }
}

var [...result] = iterable(true);
assertEquals([], result);

var [...result] = iterable(false);
assertEquals([42], result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4edf16d^!)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=4edf16d)  
[src/js/runtime.js](https://cs.chromium.org/chromium/src/v8/src/js/runtime.js?cl=4edf16d)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=4edf16d)  
[test/cctest/interpreter/bytecode_expectations/CallRuntime.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/CallRuntime.golden?cl=4edf16d)  
[test/mjsunit/es6/regress/regress-4759.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-4759.js?cl=4edf16d)  
  

---   

## **regress-crbug-599714.js (chromium issue)**  
   
**[Issue 599714:
 code->kind() == Code::OPTIMIZED_FUNCTION in src/frames.cc](https://crbug.com/599714)**  
**[Commit: [frames] Also properly deal with TF builtins in OptimizedFrame::GetFunctions().](https://chromium.googlesource.com/v8/v8/+/e5724d9)**  
  
Date(Commit): Tue Apr 05 06:41:20 2016  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1861583002](https://codereview.chromium.org/1861583002)  
Regress: [mjsunit/regress/regress-crbug-599714.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599714.js)  
```javascript
var custom_toString = function() {
  var boom = custom_toString.caller;
  return boom;
}

var object = {};
object.toString = custom_toString;

try { Object.hasOwnProperty(object); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5724d9^!)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=e5724d9)  
[test/mjsunit/regress/regress-crbug-599714.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599714.js?cl=e5724d9)  
  

---   

## **regress-crbug-513471.js (chromium issue)**  
   
**[Issue 513471:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsOddball()) in src/objects-](https://crbug.com/513471)**  
**[Commit: Fix resuming generator marked for optimization.](https://chromium.googlesource.com/v8/v8/+/6ab9c18)**  
  
Date(Commit): Mon Apr 04 11:52:09 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Recharge", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1856683002](https://codereview.chromium.org/1856683002)  
Regress: [mjsunit/regress/regress-crbug-513471.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-513471.js)  
```javascript
var g = (function*(){});
var f = g();
%OptimizeFunctionOnNextCall(g);
f.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6ab9c18^!)  
[src/runtime/runtime-generator.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-generator.cc?cl=6ab9c18)  
[test/mjsunit/regress/regress-crbug-513471.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-513471.js?cl=6ab9c18)  
  

---   

## **regress-crbug-599003.js (chromium issue)**  
   
**[Issue 599003:
 RUNTIME_ASSERT in map->IsMap() in src/heap/spaces.cc](https://crbug.com/599003)**  
**[Commit: Properly complete in-object slack tracking.](https://chromium.googlesource.com/v8/v8/+/4598356)**  
  
Date(Commit): Mon Apr 04 10:00:44 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "M-50", "Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "Release-0-M50", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1856653002](https://codereview.chromium.org/1856653002)  
Regress: [mjsunit/regress/regress-crbug-599003.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599003.js)  
```javascript
function A() {}

function g1() {
  var obj = new A();
  obj.v0 = 0;
  obj.v1 = 0;
  obj.v2 = 0;
  obj.v3 = 0;
  obj.v4 = 0;
  obj.v5 = 0;
  obj.v6 = 0;
  obj.v7 = 0;
  obj.v8 = 0;
  obj.v9 = 0;
  return obj;
}

function g2() {
  return new A();
}

var o = g1();
%OptimizeFunctionOnNextCall(g2);
g2();
o = null;
gc();

for (var i = 0; i < 20; i++) {
  var o = new A();
}
g2();

gc();  // Boom!  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4598356^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4598356)  
[test/mjsunit/regress/regress-crbug-599003.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599003.js?cl=4598356)  
  

---   

## **regress-crbug-599067.js (chromium issue)**  
   
**[Issue 599067:
 RUNTIME_ASSERT in args[0]->IsJSObject() in src/runtime/runtime-internal.cc](https://crbug.com/599067)**  
**[Commit: Display a meaningfull error message when trying to capture a stack trace to a proxy.](https://chromium.googlesource.com/v8/v8/+/c7ff576)**  
  
Date(Commit): Mon Apr 04 08:37:30 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "M-49", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1844223004](https://codereview.chromium.org/1844223004)  
Regress: [mjsunit/regress/regress-crbug-599067.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599067.js)  
```javascript
try {
  var o = {};
  var p = new Proxy({}, o);
  Error.captureStackTrace(p);
} catch(e) {
  assertEquals("invalid_argument", e.message);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7ff576^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=c7ff576)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=c7ff576)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=c7ff576)  
[test/mjsunit/regress/regress-crbug-599067.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599067.js?cl=c7ff576)  
  

---   

## **regress-599001-verifyheap.js (chromium issue)**  
   
**[Issue 599001:
 object->IsCode() || object->IsSeqString() || object->IsExternalString() || objec](https://crbug.com/599001)**  
**[Commit: [Interpreter] Handles BytecodeArrays when scanning objects in heap.](https://chromium.googlesource.com/v8/v8/+/8a9ada4)**  
  
Date(Commit): Fri Apr 01 13:14:33 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1850443006](https://codereview.chromium.org/1850443006)  
Regress: [mjsunit/ignition/regress-599001-verifyheap.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-599001-verifyheap.js)  
```javascript
var s = "";
for (var i = 0; i < 65535; i++) {
  s += ("var a" + i + ";");
}

(function() { eval(s); })();
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a9ada4^!)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=8a9ada4)  
[test/mjsunit/ignition/regress-599001-verifyheap.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/ignition/regress-599001-verifyheap.js?cl=8a9ada4)  
  

---   

## **regress-crbug-598998.js (chromium issue)**  
   
**[Issue 598998:
 output_count_ - 1 != frame_index in src/deoptimizer.cc](https://crbug.com/598998)**  
**[Commit: [crankshaft] [turbofan] Fix environment handling when generating a tail call from inlined function.](https://chromium.googlesource.com/v8/v8/+/ecb8fcf)**  
  
Date(Commit): Fri Apr 01 07:22:47 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1849503002](https://codereview.chromium.org/1849503002)  
Regress: [mjsunit/regress/regress-crbug-598998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-598998.js)  
```javascript
"use strict";

function deopt_function(func) {
  %DeoptimizeFunction(func);
}

function f(x) {
  return deopt_function(h);
}

function g(x) {
  return f(x, 1);
}

function h(x) {
  g(x, 1);
}

%NeverOptimizeFunction(deopt_function);

h(1);
h(1);
%OptimizeFunctionOnNextCall(h);
h(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecb8fcf^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=ecb8fcf)  
[src/crankshaft/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-arm.cc?cl=ecb8fcf)  
[src/crankshaft/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-arm64.cc?cl=ecb8fcf)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=ecb8fcf)  
[src/crankshaft/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips/lithium-mips.cc?cl=ecb8fcf)  
...  
  

---   

## **regress-crbug-599073-1.js (chromium issue)**  
   
**[Issue 599073:
 args.receiver()->IsJSReceiver() in src/builtins.cc](https://crbug.com/599073)**  
**[Commit: [ic] Use the CallFunction builtin to invoke accessors.](https://chromium.googlesource.com/v8/v8/+/6df9a22)**  
  
Date(Commit): Fri Apr 01 06:37:57 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Stability-Crash", "Reproducible", "Arch-All", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1845243005](https://codereview.chromium.org/1845243005)  
Regress: [mjsunit/regress/regress-crbug-599073-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-1.js), [mjsunit/regress/regress-crbug-599073-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-2.js), [mjsunit/regress/regress-crbug-599073-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-3.js), [mjsunit/regress/regress-crbug-599073-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-4.js)  
```javascript
Object.defineProperty(Boolean.prototype, "v", {get:constructor});

function foo(b) { return b.v; }

foo(true);
foo(true);
foo(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6df9a22^!)  
[src/ic/arm/handler-compiler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm/handler-compiler-arm.cc?cl=6df9a22)  
[src/ic/arm64/handler-compiler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm64/handler-compiler-arm64.cc?cl=6df9a22)  
[src/ic/ia32/handler-compiler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ia32/handler-compiler-ia32.cc?cl=6df9a22)  
[src/ic/mips/handler-compiler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/handler-compiler-mips.cc?cl=6df9a22)  
[src/ic/mips64/handler-compiler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/handler-compiler-mips64.cc?cl=6df9a22)  
...  
  

---   

## **regress-597565-double-to-object-transition.js (chromium issue)**  
   
**[Issue 597565:
 ASAN failure on ignition.](https://crbug.com/597565)**  
**[Commit: [Interpreter] Changes GenerateDoubleToObject to push and pop rsi value.](https://chromium.googlesource.com/v8/v8/+/e6b6e55)**  
  
Date(Commit): Thu Mar 31 13:45:48 2016  
Components/Type: None/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/1848473002](https://codereview.chromium.org/1848473002)  
Regress: [mjsunit/ignition/regress-597565-double-to-object-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/ignition/regress-597565-double-to-object-transition.js)  
```javascript
function __f_2(b, value) {
  b[1] = value;
}
function __f_9() {
 var arr = [1.5, 0, 0];
  // Call with a double, so the expected element type is double.
  __f_2(1.5);
  // Call with an object, which triggers transition from FAST_double
  // to Object for the elements type.
  __f_2(arr);
}
__f_9();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6b6e55^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=e6b6e55)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=e6b6e55)  
[test/mjsunit/ignition/regress-597565-double-to-object-transition.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/ignition/regress-597565-double-to-object-transition.js?cl=e6b6e55)  
  

---   

## **regress-crbug-476477-1.js (chromium issue)**  
   
**[Issue 476477:
 Renderer crashed in V8 generated code, arm64 only, 100% reproducible](https://crbug.com/476477)**  
**[Commit: [crankshaft] Address the deoptimization loops of Math.floor, Math.round and Math.ceil.](https://chromium.googlesource.com/v8/v8/+/978ad03)**  
  
Date(Commit): Tue Mar 29 10:24:54 2016  
Components/Type: Blink>JavaScript>API/Bug  
Labels: ["Stability-Crash", "M-43", "Via-Wizard", "Merge-Merged-4.3"]  
Code Review: [https://codereview.chromium.org/1841513003](https://codereview.chromium.org/1841513003)  
Regress: [mjsunit/regress/regress-crbug-476477-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-476477-1.js), [mjsunit/regress/regress-crbug-476477-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-476477-2.js)  
```javascript
var obj = {
  _leftTime: 12345678,
  _divider: function() {
      var s = Math.floor(this._leftTime / 3600);
      var e = Math.floor(s / 24);
      var i = s % 24;
      return {
            s: s,
            e: e,
            i: i,
          }
    }
}

for (var i = 0; i < 1000; i++) {
  obj._divider();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/978ad03^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=978ad03)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=978ad03)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=978ad03)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=978ad03)  
[src/crankshaft/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.h?cl=978ad03)  
...  
  

---   

## **regress-596718.js (chromium issue)**  
   
**[Issue 596718:
 call_site.IsValid() in src/runtime/runtime-internal.cc](https://crbug.com/596718)**  
**[Commit: Check for proper types from error handling code](https://chromium.googlesource.com/v8/v8/+/97fce62)**  
  
Date(Commit): Fri Mar 25 02:10:02 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1831053003](https://codereview.chromium.org/1831053003)  
Regress: [mjsunit/regress/regress-596718.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-596718.js)  
```javascript
Error.prepareStackTrace = function(e, frames) { return frames; }
assertThrows(() => new Error().stack[0].getMethodName.call({}), TypeError);

Error.prepareStackTrace = function(e, frames) { return frames.map(frame => new Proxy(frame, {})); }
assertThrows(() => new Error().stack[0].getMethodName(), TypeError);

Error.prepareStackTrace = function(e, frames) { return frames; }
assertEquals(null, new Error().stack[0].getMethodName());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/97fce62^!)  
[src/js/messages.js](https://cs.chromium.org/chromium/src/v8/src/js/messages.js?cl=97fce62)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=97fce62)  
[test/cctest/interpreter/bytecode_expectations/ForOf.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForOf.golden?cl=97fce62)  
[test/mjsunit/regress/regress-596718.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-596718.js?cl=97fce62)  
  

---   

## **regress-crbug-595615.js (chromium issue)**  
   
**[Issue 595615:
 *deopt_index != Safepoint::kNoDeoptimizationIndex in src/frames.cc](https://crbug.com/595615)**  
**[Commit: [crankshaft] Check if the function is callable before generating a tail call via Call builtin.](https://chromium.googlesource.com/v8/v8/+/e6dca37)**  
  
Date(Commit): Mon Mar 21 19:24:28 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1818003002](https://codereview.chromium.org/1818003002)  
Regress: [mjsunit/regress/regress-crbug-595615.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595615.js)  
```javascript
"use strict";

function f(o) {
  return o.x();
}
try { f({ x: 1 }); } catch(e) {}
try { f({ x: 1 }); } catch(e) {}
%OptimizeFunctionOnNextCall(f);
try { f({ x: 1 }); } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6dca37^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=e6dca37)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=e6dca37)  
[test/mjsunit/regress/regress-crbug-595615.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595615.js?cl=e6dca37)  
  

---   

## **regress-crbug-595738.js (chromium issue)**  
   
**[Issue 595738:
 JSON.stringify() throws error about circular structures in a Backbone Model](https://crbug.com/595738)**  
**[Commit: [json] Allow any callable object for toJSON.](https://chromium.googlesource.com/v8/v8/+/cc04776)**  
  
Date(Commit): Sun Mar 20 19:35:28 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-50", "has-Bisect", "Via-Wizard", "Arch-All", "M-49", "M-51"]  
Code Review: [https://codereview.chromium.org/1824533002](https://codereview.chromium.org/1824533002)  
Regress: [mjsunit/regress/regress-crbug-595738.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595738.js)  
```javascript
function foo() { return 1; }
var x = {toJSON: foo.bind()};
assertEquals("1", JSON.stringify(x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc04776^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=cc04776)  
[test/mjsunit/regress/regress-crbug-595738.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595738.js?cl=cc04776)  
  

---   

## **regress-crbug-595657.js (chromium issue)**  
   
**[Issue 595657:
 index <= know_captures in src/regexp/regexp-parser.cc](https://crbug.com/595657)**  
**[Commit: [regexp] catch stack overflow when parsing back references.](https://chromium.googlesource.com/v8/v8/+/1e2d0e1)**  
  
Date(Commit): Fri Mar 18 14:52:41 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1811913006](https://codereview.chromium.org/1811913006)  
Regress: [mjsunit/regress/regress-crbug-595657.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595657.js)  
```javascript
function test() {
  try {
    test();
  } catch(e) {
    /(\2)(a)/.test("");
  }
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e2d0e1^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=1e2d0e1)  
[test/mjsunit/regress/regress-crbug-595657.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595657.js?cl=1e2d0e1)  
  

---   

## **regress-595319.js (chromium issue)**  
   
**[Issue 595319:
 !isolate->has_pending_exception() in src/compiler.cc](https://crbug.com/595319)**  
**[Commit: Throw the right exceptions from setting elements in Array.prototype.concat](https://chromium.googlesource.com/v8/v8/+/7acee1e)**  
  
Date(Commit): Thu Mar 17 22:42:00 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1814933002](https://codereview.chromium.org/1814933002)  
Regress: [mjsunit/regress/regress-595319.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-595319.js)  
```javascript
class MyException extends Error { }
class NoDefinePropertyArray extends Array {
  constructor(...args) {
    super(...args);
    return new Proxy(this, {
      defineProperty() { throw new MyException(); }
    });
  }
}
assertThrows(() => new NoDefinePropertyArray().concat([1]), MyException);

class ZeroGetterArray extends Array { get 0() {} };
assertArrayEquals([1], new ZeroGetterArray().concat(1));


class FrozenArray extends Array {
  constructor(...args) { super(...args); Object.freeze(this); }
}
assertThrows(() => new FrozenArray().concat([1]), TypeError);

class ZeroFrozenArray extends Array {
  constructor(...args) {
    super(...args);
    Object.defineProperty(this, 0, {value: 1});
  }
}
assertThrows(() => new ZeroFrozenArray().concat([1]), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7acee1e^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=7acee1e)  
[test/mjsunit/regress/regress-595319.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-595319.js?cl=7acee1e)  
  

---   

## **regress-v8-4839.js (v8 issue)**  
   
**[Issue 4839:
 Ember.js 2.4.x (only on v8) crash: May be inlining related](https://crbug.com/v8/4839)**  
**[Commit: [crankshaft] Fix inlining to always connect both branches of test context.](https://chromium.googlesource.com/v8/v8/+/c2e82d6)**  
  
Date(Commit): Thu Mar 17 10:00:21 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1811693002](https://codereview.chromium.org/1811693002)  
Regress: [mjsunit/regress/regress-v8-4839.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-4839.js)  
```javascript
function dummy() { }

(function InlinedFunctionTestContext() {
  var f = function() { }

  function g() {
   var s = "hey";
   dummy();  // Force a deopt point.
   if (f()) return s;
  }

  g();
  g();
  g();
  %OptimizeFunctionOnNextCall(g);
  f = function() { return true; }
  assertEquals("hey", g());
})();

(function InlinedConstructorReturnTestContext() {
  function c() { return 1; }

  var f = function() { return !(new c());  }

  function g() {
   var s = "hey";
   dummy();  // Force a deopt point.
   if (f()) return s;
  }

  g();
  g();
  g();
  %OptimizeFunctionOnNextCall(g);
  f = function() { return true; }
  assertEquals("hey", g());
})();

(function InlinedConstructorNoReturnTestContext() {
  function c() { }

  var f = function() { return !(new c());  }

  function g() {
   var s = "hey";
   dummy();  // Force a deopt point.
   if (f()) return s;
  }

  g();
  g();
  g();
  %OptimizeFunctionOnNextCall(g);
  f = function() { return true; }
  assertEquals("hey", g());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c2e82d6^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=c2e82d6)  
[test/mjsunit/regress/regress-v8-4839.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-4839.js?cl=c2e82d6)  
  

---   

## **regress-crbug-594955.js (chromium issue)**  
   
**[Issue 594955:
 elements_kind == DICTIONARY_ELEMENTS in src/ic/handler-compiler.cc](https://crbug.com/594955)**  
**[Commit: Fix polymorphic keyed load handler selection for string elements](https://chromium.googlesource.com/v8/v8/+/84dd29b)**  
  
Date(Commit): Wed Mar 16 10:56:16 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1806543002](https://codereview.chromium.org/1806543002)  
Regress: [mjsunit/regress/regress-crbug-594955.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594955.js)  
```javascript
function g(s, key) { return s[key]; }

assertEquals(g(new String("a"), "length"), 1);
assertEquals(g(new String("a"), "length"), 1);
assertEquals(g("a", 32), undefined);
assertEquals(g("a", "length"), 1);
assertEquals(g(new String("a"), "length"), 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84dd29b^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=84dd29b)  
[test/mjsunit/regress/regress-crbug-594955.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-594955.js?cl=84dd29b)  
  

---   

## **regress-592353.js (chromium issue)**  
   
**[Issue 592353:
 scope_contains_with_ bit not set consistently for arrow functions](https://crbug.com/592353)**  
**[Commit: Remove Scope::scope_contains_with_ bit](https://chromium.googlesource.com/v8/v8/+/108efd7)**  
  
Date(Commit): Tue Mar 15 22:41:59 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Cr-Blink-JavaScript"]  
Code Review: [https://codereview.chromium.org/1804783002](https://codereview.chromium.org/1804783002)  
Regress: [mjsunit/regress/regress-592353.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-592353.js)  
```javascript
with ({}) {}
f = ({x}) => { };
%OptimizeFunctionOnNextCall(f);
f({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/108efd7^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=108efd7)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=108efd7)  
[src/debug/debug-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-scopes.cc?cl=108efd7)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=108efd7)  
[test/cctest/interpreter/bytecode_expectations/WithStatement.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/WithStatement.golden?cl=108efd7)  
...  
  

---   

## **regress-crbug-594574-concat-leak-1.js (chromium issue)**  
   
**[Issue 594574:
 Security: v8 Array.concat OOB access writeup](https://crbug.com/594574)**  
**[Commit: [builtins] Fix Array.prototype.concat bug](https://chromium.googlesource.com/v8/v8/+/96a2bd8)**  
  
Date(Commit): Tue Mar 15 20:29:28 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_Impact-Stable", "merge-merged-4.9", "Hotlist-Merge-Approved", "Security_Severity-High", "M-49", "reward-7500", "allpublic", "reward-inprocess", "merge-merged-50", "merge-merged-5.0", "Release-2-M49", "CVE-2016-1646", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/1804963002](https://codereview.chromium.org/1804963002)  
Regress: [mjsunit/regress/regress-crbug-594574-concat-leak-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594574-concat-leak-1.js), [mjsunit/regress/regress-crbug-594574-concat-leak-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594574-concat-leak-2.js)  
```javascript
array = new Array(10);
array[0] = 0.1;
array[2] = 2.1;
array[3] = 3.1;

var copy = array.slice(0, array.length);

var proto = {};
array.__proto__ = proto;

Object.defineProperty(
  proto, 1, {
    get() {
      // Alter the array.
      array.length = 1;
      // Force gc to move the array.
      gc();
      return "value from proto";
    },
    set(new_value) { }
});

var concatted_array = Array.prototype.concat.call(array);
assertEquals(concatted_array[0], 0.1);
assertEquals(concatted_array[1], "value from proto");
assertEquals(concatted_array[2], undefined);
assertEquals(concatted_array[3], undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96a2bd8^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=96a2bd8)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=96a2bd8)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=96a2bd8)  
[test/mjsunit/array-concat.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-concat.js?cl=96a2bd8)  
[test/mjsunit/es6/array-concat.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/array-concat.js?cl=96a2bd8)  
...  
  

---   

## **regress-4825.js (v8 issue)**  
   
**[Issue 4825:
 ElementsAccessor::CollectElementIndices() gets wrong results for SLOW_SLOPPY_ARGUMENTS_ELEMENTS](https://crbug.com/v8/4825)**  
**[Commit: [runtime] fix getting element keys from SLOW_SLOPPY_ARGUMENTS_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/3088979)**  
  
Date(Commit): Mon Mar 14 17:10:50 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1778023004](https://codereview.chromium.org/1778023004)  
Regress: [mjsunit/regress/regress-4825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4825.js)  
```javascript
function enumerate(o) {
  var keys = [];
  for (var key in o) keys.push(key);
  return keys;
}

(function testSlowSloppyArgumentsElements()  {
  function slowSloppyArguments(a, b, c) {
    %HeapObjectVerify(arguments);
    arguments[10000] = "last";
    arguments[4000] = "first";
    arguments[6000] = "second";
    arguments[5999] = "x";
    arguments[3999] = "y";
    %HeapObjectVerify(arguments);
    return arguments;
  }
  assertEquals(["0", "1", "2", "3999", "4000", "5999", "6000", "10000"],
               Object.keys(slowSloppyArguments(1, 2, 3)));

  assertEquals(["0", "1", "2", "3999", "4000", "5999", "6000", "10000"],
               enumerate(slowSloppyArguments(1,2,3)));
})();

(function testSlowSloppyArgumentsElementsNotEnumerable() {
  function slowSloppyArguments(a, b, c) {
    Object.defineProperty(arguments, 10000, {
      enumerable: false, configurable: false, value: "NOPE"
    });
    %HeapObjectVerify(arguments);
    arguments[4000] = "first";
    arguments[6000] = "second";
    arguments[5999] = "x";
    arguments[3999] = "y";
    %HeapObjectVerify(arguments);
    return arguments;
  }

  assertEquals(["0", "1", "2", "3999", "4000", "5999", "6000"],
               Object.keys(slowSloppyArguments(1, 2, 3)));

  assertEquals(["0", "1", "2", "3999", "4000", "5999", "6000"],
                enumerate(slowSloppyArguments(1,2,3)));
})();


(function testFastSloppyArgumentsElements()  {
  function fastSloppyArguments(a, b, c) {
    arguments[5] = 1;
    arguments[7] = 0;
    arguments[3] = 2;
    %HeapObjectVerify(arguments);
    return arguments;
  }
  assertEquals(["0", "1", "2", "3", "5", "7"],
               Object.keys(fastSloppyArguments(1, 2, 3)));

  assertEquals(
      ["0", "1", "2", "3", "5", "7"], enumerate(fastSloppyArguments(1, 2, 3)));

  function fastSloppyArguments2(a, b, c) {
    delete arguments[0];
    %DebugPrint(arguments);
    %HeapObjectVerify(arguments);
    arguments[0] = "test";
    %DebugPrint(arguments);
    %HeapObjectVerify(arguments);
    return arguments;
  }

  assertEquals(["0", "1", "2"], Object.keys(fastSloppyArguments2(1, 2, 3)));
  assertEquals(["0", "1", "2"], enumerate(fastSloppyArguments2(1, 2, 3)));
})();

(function testFastSloppyArgumentsElementsNotEnumerable() {
  function fastSloppyArguments(a, b, c) {
    Object.defineProperty(arguments, 5, {
      enumerable: false, configurable: false, value: "NOPE"
    });
    %HeapObjectVerify(arguments);
    arguments[7] = 0;
    arguments[3] = 2;
    %HeapObjectVerify(arguments);
    return arguments;
  }
  assertEquals(
      ["0", "1", "2", "3", "7"], Object.keys(fastSloppyArguments(1, 2, 3)));

  assertEquals(
      ["0", "1", "2", "3", "7"], enumerate(fastSloppyArguments(1,2,3)));

  function fastSloppyArguments2(a, b, c) {
    delete arguments[0];
    %HeapObjectVerify(arguments);
    Object.defineProperty(arguments, 1, {
      enumerable: false, configurable: false, value: "NOPE"
    });
    arguments[0] = "test";
    %HeapObjectVerify(arguments);
    return arguments;
  }

  assertEquals(["0", "2"], Object.keys(fastSloppyArguments2(1, 2, 3)));
  assertEquals(["0", "2"], enumerate(fastSloppyArguments2(1, 2, 3)));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3088979^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=3088979)  
[test/mjsunit/regress/regress-4825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4825.js?cl=3088979)  
  

---   

## **regress-593299.js (chromium issue)**  
   
**[Issue 593299:
 Crash in v8::internal::Isolate::native_context](https://crbug.com/593299)**  
**[Commit: [turbofan] Fix operand calculation for constant materialization from frame.](https://chromium.googlesource.com/v8/v8/+/4d4bd54)**  
  
Date(Commit): Sun Mar 13 20:21:41 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Cr-Blink-JavaScript"]  
Code Review: [https://codereview.chromium.org/1781393002](https://codereview.chromium.org/1781393002)  
Regress: [mjsunit/regress/regress-593299.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-593299.js)  
```javascript
"use strict";

function h(global) { return global.boom(); }
function g() { var r = h({}); return r; }
function f() {
  var o = {};
  o.__defineGetter__('prop1', g);
  o.prop1;
}

assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4d4bd54^!)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=4d4bd54)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=4d4bd54)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=4d4bd54)  
[src/compiler/code-generator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.h?cl=4d4bd54)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=4d4bd54)  
...  
  

---   

## **regress-crbug-593697-2.js (chromium issue)**  
   
**[Issue 593697:
 !is_bottommost || stack_fp_ == fp_value in src/deoptimizer.cc](https://crbug.com/593697)**  
**[Commit: [turbofan] Avoid dereferencing empty handle when inlining a tail call.](https://chromium.googlesource.com/v8/v8/+/690c7a8)**  
  
Date(Commit): Fri Mar 11 11:41:29 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1781303002](https://codereview.chromium.org/1781303002)  
Regress: [mjsunit/regress/regress-crbug-593697-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-593697-2.js)  
```javascript
"use strict";

var f5 = (function f6(stdlib) {
  "use asm";
  var cos = stdlib.Math.cos;
  function f5() {
    return cos();
  }
  return { f5: f5 };
})(this, {}).f5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/690c7a8^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=690c7a8)  
[test/mjsunit/regress/regress-crbug-593697-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-593697-2.js?cl=690c7a8)  
  

---   

## **regress-crbug-592340.js (chromium issue)**  
   
**[Issue 592340:
 Crash in v8::internal::LookupIterator::UpdateProtector](https://crbug.com/592340)**  
**[Commit: Ensure appropriate bounds checking for Array subclass concat](https://chromium.googlesource.com/v8/v8/+/ca5deb1)**  
  
Date(Commit): Wed Mar 09 18:54:44 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Unreproducible", "Cr-Blink-JavaScript"]  
Code Review: [https://codereview.chromium.org/1782443002](https://codereview.chromium.org/1782443002)  
Regress: [mjsunit/regress/regress-crbug-592340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-592340.js)  
```javascript
class MyArray extends Array { }
Object.prototype[Symbol.species] = MyArray;
delete Array[Symbol.species];
__v_1 = Math.pow(2, 31);
__v_2 = [];
__v_2[__v_1] = 31;
__v_4 = [];
__v_4[__v_1 - 2] = 33;
assertThrows(() => __v_2.concat(__v_4), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca5deb1^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=ca5deb1)  
[test/mjsunit/regress/regress-crbug-592340.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-592340.js?cl=ca5deb1)  
  

---   

## **regress-4800.js (v8 issue)**  
   
**[Issue 4800:
 ARM/ARM64: some double registers are corrupted after deopt or stub failure](https://crbug.com/v8/4800)**  
**[Commit: [arm/arm64][stubs] Fix d16-d31 preservation on stub failure](https://chromium.googlesource.com/v8/v8/+/32b3d3e)**  
  
Date(Commit): Wed Mar 09 17:36:07 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1769833006](https://codereview.chromium.org/1769833006)  
Regress: [mjsunit/regress/regress-4800.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4800.js)  
```javascript
function f(x, len) {
  var distraction = [];
  var result = new Array(25);

  // Create a bunch of double values with long live ranges.
  var d0 = x + 0.5;
  var d1 = x + 1.5;
  var d2 = x + 2.5;
  var d3 = x + 3.5;
  var d4 = x + 4.5;
  var d5 = x + 5.5;
  var d6 = x + 6.5;
  var d7 = x + 7.5;
  var d8 = x + 8.5;
  var d9 = x + 9.5;
  var d10 = x + 10.5;
  var d11 = x + 11.5;
  var d12 = x + 12.5;
  var d13 = x + 13.5;
  var d14 = x + 14.5;
  var d15 = x + 15.5;
  var d16 = x + 16.5;
  var d17 = x + 17.5;
  var d18 = x + 18.5;
  var d19 = x + 19.5;
  var d20 = x + 20.5;
  var d21 = x + 21.5;
  var d22 = x + 22.5;
  var d23 = x + 23.5;
  var d24 = x + 24.5;

  // Trigger a stub failure when the array grows too big.
  distraction[len] = 0;

  // Write the long-lived doubles to memory and verify them.
  result[0] = d0;
  result[1] = d1;
  result[2] = d2;
  result[3] = d3;
  result[4] = d4;
  result[5] = d5;
  result[6] = d6;
  result[7] = d7;
  result[8] = d8;
  result[9] = d9;
  result[10] = d10;
  result[11] = d11;
  result[12] = d12;
  result[13] = d13;
  result[14] = d14;
  result[15] = d15;
  result[16] = d16;
  result[17] = d17;
  result[18] = d18;
  result[19] = d19;
  result[20] = d20;
  result[21] = d21;
  result[22] = d22;
  result[23] = d23;
  result[24] = d24;

  for (var i = 0; i < result.length; i++) {
    assertEquals(x + i + 0.5, result[i]);
  }
}

f(0, 10);
f(0, 10);
%OptimizeFunctionOnNextCall(f);
f(0, 80000);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/32b3d3e^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=32b3d3e)  
[src/arm64/deoptimizer-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/deoptimizer-arm64.cc?cl=32b3d3e)  
[test/mjsunit/regress/regress-4800.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4800.js?cl=32b3d3e)  
  

---   

## **regress-crbug-593282.js (chromium issue)**  
   
**[Issue 593282:
 0 <= from && to <= String::kMaxCodePoint in src/regexp/regexp-ast.h](https://crbug.com/593282)**  
**[Commit: [regexp] fix bogus assertion in CharacterRange constructor.](https://chromium.googlesource.com/v8/v8/+/d1f68f7)**  
  
Date(Commit): Wed Mar 09 15:55:38 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Cr-Blink-JavaScript", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1776013003](https://codereview.chromium.org/1776013003)  
Regress: [mjsunit/regress/regress-crbug-593282.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-593282.js)  
```javascript
var __v_11 = {};
function __f_2(depth) {
  try {
    __f_5(depth, __v_11);
    return true;
  } catch (e) {
    gc();
  }
}
function __f_5(n, __v_4) {
  if (--n == 0) {
    __f_1(__v_4);
    return;
  }
  __f_5(n, __v_4);
}
function __f_1(__v_4) {
  var __v_5 = new RegExp(__v_4);
}
function __f_4() {
  var __v_1 = 100;
  var __v_8 = 100000;
  while (__v_1 < __v_8 - 1) {
    var __v_3 = Math.floor((__v_1 + __v_8) / 2);
    if (__f_2(__v_3)) {
      __v_1 = __v_3;
    } else {
      __v_8 = __v_3;
    }
  }
}
__f_4();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d1f68f7^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=d1f68f7)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=d1f68f7)  
[test/cctest/test-regexp.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-regexp.cc?cl=d1f68f7)  
[test/mjsunit/regress/regress-crbug-593282.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-593282.js?cl=d1f68f7)  
  

---   

## **regress-592341.js (chromium issue)**  
   
**[Issue 592341:
 result == fixed_size + (stack_slots * kPointerSize) - StandardFrameConstants::kF](https://crbug.com/592341)**  
**[Commit: [turbofan] Fix deoptimization stack layout for fast literal comparisons.](https://chromium.googlesource.com/v8/v8/+/69c84fe)**  
  
Date(Commit): Wed Mar 09 12:36:09 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1776013002](https://codereview.chromium.org/1776013002)  
Regress: [mjsunit/regress/regress-592341.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-592341.js)  
```javascript
function id(a) {
  return a;
}

(function LiteralCompareNullDeopt() {
  function f() {
   return id(null == %DeoptimizeNow());
  }

  %OptimizeFunctionOnNextCall(f);
  assertTrue(f());
})();

(function LiteralCompareUndefinedDeopt() {
  function f() {
   return id(undefined == %DeoptimizeNow());
  }

  %OptimizeFunctionOnNextCall(f);
  assertTrue(f());
})();

(function LiteralCompareTypeofDeopt() {
  function f() {
   return id("undefined" == typeof(%DeoptimizeNow()));
  }

  %OptimizeFunctionOnNextCall(f);
  assertTrue(f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/69c84fe^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=69c84fe)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=69c84fe)  
[test/mjsunit/regress/regress-592341.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-592341.js?cl=69c84fe)  
  

---   

## **regress-590074.js (chromium issue)**  
   
**[Issue 590074:
 Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/590074)**  
**[Commit: [turbofan] Fix register constraint for memory barrier.](https://chromium.googlesource.com/v8/v8/+/9867a8a)**  
  
Date(Commit): Wed Mar 09 09:39:51 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Cr-Blink-JavaScript"]  
Code Review: [https://codereview.chromium.org/1775083002](https://codereview.chromium.org/1775083002)  
Regress: [mjsunit/regress/regress-590074.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-590074.js)  
```javascript
var __v_5 = {};

function __f_10() {
  var __v_2 = [0, 0, 0];
  __v_2[0] = 0;
  gc();
  return __v_2;
}

function __f_2(array) {
  array[1] = undefined;
}

function __f_9() {
  var __v_4 = __f_10();
  __f_2(__f_10());
  __v_5 = __f_10();
  __v_4 = __f_10();
  __f_2(__v_5);
}
__f_9();
%OptimizeFunctionOnNextCall(__f_9);
__f_9();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9867a8a^!)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=9867a8a)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=9867a8a)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=9867a8a)  
[src/compiler/mips/instruction-selector-mips.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips/instruction-selector-mips.cc?cl=9867a8a)  
[src/compiler/mips64/instruction-selector-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips64/instruction-selector-mips64.cc?cl=9867a8a)  
...  
  

---   

## **regress-crbug-592343.js (chromium issue)**  
   
**[Issue 592343:
 entry->to() + 1 > current.from() in src/regexp/jsregexp.cc](https://crbug.com/592343)**  
**[Commit: [regexp] Fix off-by-one in CharacterRange::Negate.](https://chromium.googlesource.com/v8/v8/+/f9d7c71)**  
  
Date(Commit): Mon Mar 07 11:00:01 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1768093002](https://codereview.chromium.org/1768093002)  
Regress: [mjsunit/regress/regress-crbug-592343.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-592343.js)  
```javascript
var r = /[^\u{1}-\u{1000}\u{1002}-\u{2000}]/u;
assertTrue(r.test("\u{0}"));
assertFalse(r.test("\u{1}"));
assertFalse(r.test("\u{1000}"));
assertTrue(r.test("\u{1001}"));
assertFalse(r.test("\u{1002}"));
assertFalse(r.test("\u{2000}"));
assertTrue(r.test("\u{2001}"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f9d7c71^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=f9d7c71)  
[src/regexp/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.h?cl=f9d7c71)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=f9d7c71)  
[test/mjsunit/regress/regress-crbug-592343.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-592343.js?cl=f9d7c71)  
  

---   

## **regress-crbug-590989-1.js (chromium issue)**  
   
**[Issue 590989:
 callback function not called any more after called lots of times and in different place.](https://crbug.com/590989)**  
**[Commit: [crankshaft] Fix invalid ToNumber optimization.](https://chromium.googlesource.com/v8/v8/+/0c35579)**  
  
Date(Commit): Wed Mar 02 19:28:04 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["ReleaseBlock-Beta", "M-50", "Via-Wizard", "Arch-All", "merge-merged-5.0"]  
Code Review: [https://codereview.chromium.org/1757013002](https://codereview.chromium.org/1757013002)  
Regress: [mjsunit/regress/regress-crbug-590989-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-590989-1.js), [mjsunit/regress/regress-crbug-590989-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-590989-2.js)  
```javascript
var o = {}
var p = {foo: 1.5}

function g(x) { return x.foo === +x.foo; }

assertEquals(false, g(o));
assertEquals(false, g(o));
%OptimizeFunctionOnNextCall(g);
assertEquals(false, g(o));  // Still fine here.
assertEquals(true, g(p));
%OptimizeFunctionOnNextCall(g);
assertEquals(false, g(o));  // Confused by type feedback.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c35579^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=0c35579)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=0c35579)  
[test/mjsunit/regress/regress-crbug-590989-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-590989-1.js?cl=0c35579)  
[test/mjsunit/regress/regress-crbug-590989-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-590989-2.js?cl=0c35579)  
  

---   

## **regress-4769.js (v8 issue)**  
   
**[Issue 4769:
 Builtins that should not rely on iteration do.](https://crbug.com/v8/4769)**  
**[Commit: [json] Fix iteration over object keys in InternalizeJSONProperty.](https://chromium.googlesource.com/v8/v8/+/0ad4459)**  
  
Date(Commit): Tue Mar 01 11:53:28 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1748633002](https://codereview.chromium.org/1748633002)  
Regress: [mjsunit/regress/regress-4769.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4769.js)  
```javascript
Object.getPrototypeOf([])[Symbol.iterator] = () => assertUnreachable();

JSON.stringify({foo: [42]});
JSON.stringify({foo: [42]}, []);
JSON.stringify({foo: [42]}, undefined, ' ');
JSON.stringify({foo: [42]}, [], ' ');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0ad4459^!)  
[src/js/json.js](https://cs.chromium.org/chromium/src/v8/src/js/json.js?cl=0ad4459)  
[test/mjsunit/regress/regress-4769.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4769.js?cl=0ad4459)  
  

---   

## **regress-crbug-589792.js (chromium issue)**  
   
**[Issue 589792:
 Security: [v8] Out of bound(??) memory write with asm.js](https://crbug.com/589792)**  
**[Commit: [turbofan] Bailout if LoadBuffer typing assumption doesn't hold.](https://chromium.googlesource.com/v8/v8/+/58ab990)**  
  
Date(Commit): Fri Feb 26 11:06:30 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-50", "reward-5000", "Nag", "Security_Impact-Stable", "merge-merged-4.9", "Security_Severity-High", "M-49", "allpublic", "Release-0-M50", "merge-merged-5.0", "CVE-2016-1653", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/1740123002](https://codereview.chromium.org/1740123002)  
Regress: [mjsunit/regress/regress-crbug-589792.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-589792.js)  
```javascript
var boom = (function(stdlib, foreign, heap) {
  "use asm";
  var MEM8 = new stdlib.Uint8Array(heap);
  var MEM32 = new stdlib.Int32Array(heap);
  function foo(i, j) {
    j = MEM8[256];
    // This following value '10' determines the value of 'rax'
    MEM32[j >> 10] = 0xabcdefaa;
    return MEM32[j >> 2] + j
  }
  return foo
})(this, 0, new ArrayBuffer(256));
%OptimizeFunctionOnNextCall(boom);
boom(0, 0x1000);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58ab990^!)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=58ab990)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=58ab990)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=58ab990)  
[test/cctest/cctest.gyp](https://cs.chromium.org/chromium/src/v8/test/cctest/cctest.gyp?cl=58ab990)  
[test/cctest/compiler/test-run-properties.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-properties.cc?cl=58ab990)  
...  
  

---   

## **regress-crbug-589472.js (chromium issue)**  
   
**[Issue 589472:
 operand_stack_depth_ >= count in src/full-codegen/full-codegen.cc](https://crbug.com/589472)**  
**[Commit: [fullcodegen] Fix assert for operand stack depth tracking.](https://chromium.googlesource.com/v8/v8/+/3baa290)**  
  
Date(Commit): Wed Feb 24 16:29:47 2016  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Cr-Blink-JavaScript"]  
Code Review: [https://codereview.chromium.org/1732903002](https://codereview.chromium.org/1732903002)  
Regress: [mjsunit/regress/regress-crbug-589472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-589472.js)  
```javascript
try { f() } catch(e) { assertInstanceof(e, RangeError) }

function f() {
  return Math.max(
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "boom", 1, 2, 3, 4, 5, 6, 7, 8, 9);
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3baa290^!)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=3baa290)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=3baa290)  
[test/mjsunit/regress/regress-crbug-589472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-589472.js?cl=3baa290)  
  

---   

## **regress-crbug-587068.js (chromium issue)**  
   
**[Issue 587068:
 10.6%-15.6% regression in webrtc.datachannel at 375196:375247](https://crbug.com/587068)**  
**[Commit: [crankshaft] Fix deopt loop in String.fromCharCode on non-int32 inputs.](https://chromium.googlesource.com/v8/v8/+/6cc5c60)**  
  
Date(Commit): Wed Feb 24 10:59:55 2016  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-50", "TE-Triaged", "Arch-All", "Performance-Sheriff"]  
Code Review: [https://codereview.chromium.org/1727873003](https://codereview.chromium.org/1727873003)  
Regress: [mjsunit/regress/regress-crbug-587068.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-587068.js)  
```javascript
function foo(i) { return String.fromCharCode(i); }
foo(33);
foo(33);
%OptimizeFunctionOnNextCall(foo);
foo(33.3);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6cc5c60^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=6cc5c60)  
[test/mjsunit/regress/regress-crbug-587068.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-587068.js?cl=6cc5c60)  
  

---   

## **regress-crbug-581577.js (chromium issue)**  
   
**[Issue 581577:
 RegExp flag/source getters cause app.jiff.com to fail](https://crbug.com/581577)**  
**[Commit: ES2015 web compat workaround: RegExp.prototype.flags => ""](https://chromium.googlesource.com/v8/v8/+/b22b258)**  
  
Date(Commit): Tue Feb 23 01:49:03 2016  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["M-48", "hasbisect", "Via-Wizard", "merge-merged-4.9"]  
Code Review: [https://codereview.chromium.org/1640803003](https://codereview.chromium.org/1640803003)  
Regress: [mjsunit/regress/regress-crbug-581577.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-581577.js)  
```javascript
assertEquals("", RegExp.prototype.flags);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b22b258^!)  
[src/js/harmony-unicode-regexps.js](https://cs.chromium.org/chromium/src/v8/src/js/harmony-unicode-regexps.js?cl=b22b258)  
[src/js/regexp.js](https://cs.chromium.org/chromium/src/v8/src/js/regexp.js?cl=b22b258)  
[test/mjsunit/es6/regexp-flags.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regexp-flags.js?cl=b22b258)  
[test/mjsunit/regress/regress-crbug-581577.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-581577.js?cl=b22b258)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=b22b258)  
...  
  

---   

## **regress-585775.js (chromium issue)**  
   
**[Issue 585775:
 [Regression]: Snapdeal page doesn't render its content fully](https://crbug.com/585775)**  
**[Commit: Return undefined from RegExp.prototype.compile](https://chromium.googlesource.com/v8/v8/+/cdec6d2)**  
  
Date(Commit): Sat Feb 20 00:35:57 2016  
Components/Type: Blink>JavaScript>Language/Bug-Regression  
Labels: ["hasbisect", "TE-Verified-M50", "merge-merged-4.9", "M-49", "ReleaseBlock-Stable", "TE-Verified-50.0.2656.0"]  
Code Review: [https://codereview.chromium.org/1714903004](https://codereview.chromium.org/1714903004)  
Regress: [mjsunit/regress/regress-585775.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-585775.js)  
```javascript
var pattern = /foo/;
assertEquals(pattern, pattern.compile(pattern));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cdec6d2^!)  
[src/js/regexp.js](https://cs.chromium.org/chromium/src/v8/src/js/regexp.js?cl=cdec6d2)  
[test/mjsunit/regress/regress-585775.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-585775.js?cl=cdec6d2)  
  

---   

## **regress-4659.js (v8 issue)**  
   
**[Issue 4659:
 AstValueFactory::GetString() should handle String::FlatContent::NON_FLAT](https://crbug.com/v8/4659)**  
**[Commit: Force SharedFunctionInfo::name() to be a flat string](https://chromium.googlesource.com/v8/v8/+/58a9bc5)**  
  
Date(Commit): Thu Feb 11 18:53:02 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1686193003](https://codereview.chromium.org/1686193003)  
Regress: [mjsunit/regress/regress-4659.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4659.js)  
```javascript
var obj = {
  get longerName(){
    return 42;
  }
};
assertEquals(42, obj.longerName);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58a9bc5^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=58a9bc5)  
[test/mjsunit/regress/regress-4659.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4659.js?cl=58a9bc5)  
  

---   

## **regress-crbug-584188.js (chromium issue)**  
   
**[Issue 584188:
 object->HasFastSmiOrObjectElements() || object->HasFastDoubleElements() in src/o](https://crbug.com/584188)**  
**[Commit: Fix Array.prototype.sort for *_STRING_WRAPPER_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/5d2c09a)**  
  
Date(Commit): Fri Feb 05 13:36:51 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1670153002](https://codereview.chromium.org/1670153002)  
Regress: [mjsunit/regress/regress-crbug-584188.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-584188.js)  
```javascript
var x = {};
try {
Object.defineProperty(String.prototype, "3", { x: function() { x = v; }});
string = "bla";
} catch(e) {; }
assertThrows("Array.prototype.sort.call(string);", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d2c09a^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=5d2c09a)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=5d2c09a)  
[test/mjsunit/regress/regress-crbug-584188.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-584188.js?cl=5d2c09a)  
  

---   

## **regress-dead-throw-inlining.js (other issue)**  
   
**[Commit: [turbofan] Reducers should revisit end after merging to it.](https://chromium.googlesource.com/v8/v8/+/52f2dbc)**  
  
Date(Commit): Fri Feb 05 11:01:44 2016  
Code Review: [https://codereview.chromium.org/1675433003](https://codereview.chromium.org/1675433003)  
Regress: [mjsunit/compiler/regress-dead-throw-inlining.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-dead-throw-inlining.js)  
```javascript
function g() { if (false) throw 0; }
function f() { g(); }

%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52f2dbc^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=52f2dbc)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=52f2dbc)  
[src/compiler/js-global-object-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-global-object-specialization.cc?cl=52f2dbc)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=52f2dbc)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=52f2dbc)  
...  
  
  
---   

## **regress-583260.js (chromium issue)**  
   
**[Issue 583260:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/583260)**  
**[Commit: Expect JSReceiver in Runtime_DeleteLookupSlot, not just JSObject.](https://chromium.googlesource.com/v8/v8/+/a973f73)**  
  
Date(Commit): Wed Feb 03 09:49:22 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-4.9", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1664683002](https://codereview.chromium.org/1664683002)  
Regress: [mjsunit/regress/regress-583260.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-583260.js)  
```javascript
__v_1 = {
  has() { return true }
};
__v_2 = new Proxy({}, __v_1);
function __f_5(object) {
  with (object) { return delete __v_3; }
}
 __f_5(__v_2)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a973f73^!)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=a973f73)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=a973f73)  
[test/mjsunit/regress/regress-583260.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-583260.js?cl=a973f73)  
  

---   

## **regress-integer-indexed-element.js (other issue)**  
   
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
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=621bdd6)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=621bdd6)  
[src/lookup.h](https://cs.chromium.org/chromium/src/v8/src/lookup.h?cl=621bdd6)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=621bdd6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=621bdd6)  
...  
  
  
---   

## **regress-crbug-583257.js (chromium issue)**  
   
**[Issue 583257:
 transition_map->has_dictionary_elements() || transition_map->has_fixed_typed_arr](https://crbug.com/583257)**  
**[Commit: More *_STRING_WRAPPER_ELEMENTS fixes](https://chromium.googlesource.com/v8/v8/+/d582d2b)**  
  
Date(Commit): Tue Feb 02 13:51:00 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1651253003](https://codereview.chromium.org/1651253003)  
Regress: [mjsunit/regress/regress-crbug-583257.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-583257.js)  
```javascript
Object.defineProperty(String.prototype, "0", { __v_1: 1});
Object.defineProperty(String.prototype, "3", { __v_1: 1});

(function () {
  var s = new String();
  function set(object, index, value) { object[index] = value; }
  set(s, 10, "value");
  set(s, 1073741823, "value");
})();

function __f_11() {
  Object.preventExtensions(new String());
}
__f_11();
__f_11();

(function() {
  var i = 10;
  var a = new String("foo");
  for (var j = 0; j < i; j++) {
    a[j] = {};
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d582d2b^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=d582d2b)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d582d2b)  
[test/mjsunit/regress/regress-crbug-583257.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-583257.js?cl=d582d2b)  
  

---   

## **regress-4654.js (v8 issue)**  
   
**[Issue 4654:
 Unicode normalization omits NUL characters and anything after them in the string](https://crbug.com/v8/4654)**  
**[Commit: Fix Unicode string normalization with null bytes](https://chromium.googlesource.com/v8/v8/+/f3e41d9)**  
  
Date(Commit): Fri Jan 29 17:00:46 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1645223003](https://codereview.chromium.org/1645223003)  
Regress: [mjsunit/regress/regress-4654.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4654.js)  
```javascript
assertEquals('hello\u0000foobar', 'hello\u0000foobar'.normalize('NFC'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f3e41d9^!)  
[src/runtime/runtime-i18n.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-i18n.cc?cl=f3e41d9)  
[test/mjsunit/regress/regress-4654.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4654.js?cl=f3e41d9)  
  

---   

## **regress-4715.js (v8 issue)**  
   
**[Issue 4715:
 For-In is broken in Crankshaft](https://crbug.com/v8/4715)**  
**[Commit: [crankshaft] Make the for-in slow path compatible with the other compilers.](https://chromium.googlesource.com/v8/v8/+/3251a03)**  
  
Date(Commit): Fri Jan 29 07:50:51 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1650493002](https://codereview.chromium.org/1650493002)  
Regress: [mjsunit/regress/regress-4715.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4715.js)  
```javascript
var training = {};
training.a = "nop";
training.slow = "nop";
delete training.slow;  // Dictionary-mode properties => slow-mode for-in.

var keepalive = {};
keepalive.a = "nop";  // Keep a map early in the transition chain alive.

function GetReal() {
  var r = {};
  r.a = "nop";
  r.b = "nop";
  r.c = "dictionarize",
  r.d = "gc";
  r.e = "result";
  return r;
};

function SideEffect(object, action) {
  if (action === "dictionarize") {
    delete object.a;
  } else if (action === "gc") {
    gc();
  }
}

function foo(object) {
  for (var key in object) {
    SideEffect(object, object[key]);
  }
  return key;
}

foo(training);
SideEffect({a: 0}, "dictionarize");
SideEffect({}, "gc");

%OptimizeFunctionOnNextCall(foo);
assertEquals("e", foo(GetReal()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3251a03^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3251a03)  
[test/mjsunit/regress/regress-4715.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4715.js?cl=3251a03)  
  

---   

## **regress-crbug-580584.js (chromium issue)**  
   
**[Issue 580584:
 Chr-48 regression, w bisect: Object.defineProperty broken for window.localStorage, sessionStorage, etc](https://crbug.com/580584)**  
**[Commit: [api] Default native data property setter to replace the setter if the property is writable.](https://chromium.googlesource.com/v8/v8/+/997cd3d)**  
  
Date(Commit): Wed Jan 27 13:22:18 2016  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/1632603002](https://codereview.chromium.org/1632603002)  
Regress: [mjsunit/regress/regress-crbug-580584.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580584.js)  
```javascript
function f() { return arguments }

Object.defineProperty(f, "name", {
  writable: true, configurable: true, value: 10});
assertEquals({value: 10, writable: true, enumerable: false, configurable: true},
             Object.getOwnPropertyDescriptor(f, "name"));

var args = f();

args[Symbol.iterator] = 10;
assertEquals({value: 10, writable: true, configurable: true, enumerable: false},
             Object.getOwnPropertyDescriptor(args, Symbol.iterator));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/997cd3d^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=997cd3d)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=997cd3d)  
[src/api-natives.cc](https://cs.chromium.org/chromium/src/v8/src/api-natives.cc?cl=997cd3d)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=997cd3d)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=997cd3d)  
...  
  

---   

## **regress-crbug-580934.js (chromium issue)**  
   
**[Issue 580934:
 Block-scope (let) sibling functions becoming undefined / garbage-collected](https://crbug.com/580934)**  
**[Commit: Ensure arrow functions can close over lexically-scoped variables](https://chromium.googlesource.com/v8/v8/+/953bb41)**  
  
Date(Commit): Tue Jan 26 23:11:10 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Merge-Merged-49", "M-48", "Via-Wizard", "merge-merged-4.8", "merge-merged-4.9", "M-49", "merge-merged-48"]  
Code Review: [https://codereview.chromium.org/1630823006](https://codereview.chromium.org/1630823006)  
Regress: [mjsunit/regress/regress-crbug-580934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580934.js)  
```javascript
"use strict";
{
  let one = () => {
    return "example.com";
  };

  let two = () => {
    return one();
  };

  assertEquals("example.com", two());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/953bb41^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=953bb41)  
[test/mjsunit/regress/regress-crbug-580934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-580934.js?cl=953bb41)  
  

---   

## **regress-crbug-580506.js (chromium issue)**  
   
**[Issue 580506:
 memcmp(fresh->address(), new_map->address(), Map::kCodeCacheOffset) == 0 in src/](https://crbug.com/580506)**  
**[Commit: Also check new_target_is_base() bit when comparing two maps for equivalence.](https://chromium.googlesource.com/v8/v8/+/ac03ef0)**  
  
Date(Commit): Mon Jan 25 16:44:01 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1631673002](https://codereview.chromium.org/1631673002)  
Regress: [mjsunit/regress/regress-crbug-580506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580506.js)  
```javascript
(function() {
  'use strict';
  class A extends Function {
    constructor(...args) {
      super(...args);
      this.a = 42;
    }
  }
  var v1 = new A("'use strict';");
  function f(func) {
    func.__defineSetter__('a', function() { });
  }
  var v2 = new A();
  f(v2);
  f(v1);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac03ef0^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ac03ef0)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=ac03ef0)  
[test/mjsunit/regress/regress-crbug-580506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-580506.js?cl=ac03ef0)  
  

---   

## **regress-4693.js (v8 issue)**  
   
**[Issue 4693:
 Block-scoped sloppy mode function declaration issue with Cisco Spark](https://crbug.com/v8/4693)**  
**[Commit: Sloppy mode webcompat: allow conflicting function declarations in blocks](https://chromium.googlesource.com/v8/v8/+/8aeb608)**  
  
Date(Commit): Sat Jan 23 00:40:53 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1622723003](https://codereview.chromium.org/1622723003)  
Regress: [mjsunit/regress/regress-4693.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4693.js)  
```javascript
(function() {
  assertEquals(undefined, f);  // Annex B
  if (true) {
    assertEquals(2, f());
    function f() { return 1 }
    assertEquals(2, f());
    function f() { return 2 }
    assertEquals(2, f());
  }
  assertEquals(2, f());  // Annex B
})();

assertThrows(`
  (function() {
    "use strict";
    if (true) {
      function f() { return 1 }
      function f() { return 2 }
    }
  })();
`, SyntaxError);

assertThrows(`
  (function() {
    if (true) {
      let f;
      function f() { return 2 }
    }
  })();
`, SyntaxError);

assertThrows(`
  (function() {
    if (true) {
      function f() { return 2 }
      let f;
    }
  })();
`, SyntaxError);

assertThrows(`
  (function() {
    if (true) {
      const f;
      function f() { return 2 }
    }
  })();
`, SyntaxError);

assertThrows(`
  (function() {
    if (true) {
      function f() { return 2 }
      const f;
    }
  })();
`, SyntaxError);

(function() {
  assertEquals(undefined, f);  // Annex B
  if (true) {
    assertEquals(undefined, f);
    { function f() { return 1 } }
    assertEquals(1, f());
    { function f() { return 2 } }
    assertEquals(2, f());
  }
  assertEquals(2, f());  // Annex B
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8aeb608^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=8aeb608)  
[test/mjsunit/regress/regress-4693.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4693.js?cl=8aeb608)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=8aeb608)  
  

---   

## **regress-3650-1.js (v8 issue)**  
   
**[Issue 3650:
 for x in y de-optimizations](https://crbug.com/v8/3650)**  
**[Commit: [crankshaft] For-in index increment cannot overflow.](https://chromium.googlesource.com/v8/v8/+/c7d2adc)**  
  
Date(Commit): Fri Jan 22 07:55:11 2016  
Type: FeatureRequest  
Code Review: [https://codereview.chromium.org/1621623002](https://codereview.chromium.org/1621623002)  
Regress: [mjsunit/regress/regress-3650-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3650-1.js), [mjsunit/regress/regress-3650-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3650-2.js), [mjsunit/regress/regress-3650-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3650-3.js)  
```javascript
function f(t) {
  var result = [];
  for (var i in t) {
    for (var j in t) {
      result.push(i + j + t[i] + t[j]);
      continue;
    }
  }
  return result.join('');
}

var t = {a: "1", b: "2"};
assertEquals("aa11ab12ba21bb22", f(t));
%OptimizeFunctionOnNextCall(f);
assertEquals("aa11ab12ba21bb22", f(t));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7d2adc^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=c7d2adc)  
[test/mjsunit/regress/regress-3650-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3650-1.js?cl=c7d2adc)  
  

---   

## **regress-4267.js (v8 issue)**  
   
**[Issue 4267:
 Array length reduction should throw in strict mode if it can't delete an element](https://crbug.com/v8/4267)**  
**[Commit: Array length reduction should throw in strict mode if it can't delete an element.](https://chromium.googlesource.com/v8/v8/+/1d3e837)**  
  
Date(Commit): Thu Jan 21 14:23:09 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1587073003](https://codereview.chromium.org/1587073003)  
Regress: [mjsunit/regress/regress-4267.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4267.js)  
```javascript
"use strict";

var a = [];
Object.defineProperty(a, "0", {configurable: false, value: 10});
assertEquals(1, a.length);
var setter = ()=>{ a.length = 0; };
assertThrows(setter);
assertThrows(setter);
%OptimizeFunctionOnNextCall(setter);
assertThrows(setter);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d3e837^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=1d3e837)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=1d3e837)  
[src/arguments.cc](https://cs.chromium.org/chromium/src/v8/src/arguments.cc?cl=1d3e837)  
[src/arguments.h](https://cs.chromium.org/chromium/src/v8/src/arguments.h?cl=1d3e837)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=1d3e837)  
...  
  

---   

## **regress-4696.js (v8 issue)**  
   
**[Issue 4696:
 Spread rewriting doesn't work well](https://crbug.com/v8/4696)**  
**[Commit: Fix bug with spread rewriting](https://chromium.googlesource.com/v8/v8/+/52a01ae)**  
  
Date(Commit): Thu Jan 21 12:16:20 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1617713002](https://codereview.chromium.org/1617713002)  
Regress: [mjsunit/harmony/regress/regress-4696.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-4696.js)  
```javascript
(function testSpreadIndex() {
  var result = [...[17, 42]][1];
  assertEquals(result, 42);
})();

(function testSpreadProperty() {
  var result = [...[17, 42]].length;
  assertEquals(result, 2);
})();

(function testSpreadMethodCall() {
  var result = [...[17, 42]].join("+");
  assertEquals(result, "17+42");
})();

(function testSpreadSavedMethodCall() {
  var x = [...[17, 42]];
  var method = x.join;
  var result = method.call(x, "+");
  assertEquals(result, "17+42");
})();

(function testSpreadAsTemplateTag() {
  assertThrows(function() { [...[17, 42]] `foo`; }, TypeError)
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52a01ae^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=52a01ae)  
[test/mjsunit/harmony/regress/regress-4696.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4696.js?cl=52a01ae)  
  

---   

## **regress-crbug-578039-Proxy_construct_prototype_change.js (chromium issue)**  
   
**[Issue 578039:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsMap()) in src/objects-inl.](https://crbug.com/578039)**  
**[Commit: [proxy] Reload the initial map after prototype lookup on constructable Proxy.](https://chromium.googlesource.com/v8/v8/+/ec30425)**  
  
Date(Commit): Mon Jan 18 12:49:29 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-4.9", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1586203003](https://codereview.chromium.org/1586203003)  
Regress: [mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js)  
```javascript
function target() {};

var proxy = new Proxy(target, {
  get() {
    // Reset the initial map of the target.
    target.prototype = 123;
  }});

new proxy();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec30425^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ec30425)  
[test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js?cl=ec30425)  
  

---   

## **regress-578775.js (chromium issue)**  
   
**[Issue 578775:
 *deopt_index != Safepoint::kNoDeoptimizationIndex in src/frames.cc](https://crbug.com/578775)**  
**[Commit: Remove premature crankshaft optimization of HasInPrototypeChain.](https://chromium.googlesource.com/v8/v8/+/107db2c)**  
  
Date(Commit): Mon Jan 18 12:12:32 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-4.9", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1605483002](https://codereview.chromium.org/1605483002)  
Regress: [mjsunit/regress/regress-578775.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-578775.js)  
```javascript
var __v_9 = {};
for (var __v_0 = 0; __v_0 < 1000; __v_0++) {
}
__v_2 = { __v_2: 1 };
__v_12 = new Proxy({}, {});
function f() {
  var __v_10 = new Proxy({}, __v_2);
  __v_9.__proto__ = __v_10;
  __v_2.getPrototypeOf = function () { return __v_9 };
  Object.prototype.isPrototypeOf.call(__v_0, __v_10);
};
assertThrows(f, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/107db2c^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=107db2c)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=107db2c)  
[src/js/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/js/v8natives.js?cl=107db2c)  
[test/mjsunit/regress/regress-578775.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-578775.js?cl=107db2c)  
  

---   

## **regress-4509-Class-constructor-typeerror-realm.js (v8 issue)**  
   
**[Issue 4509:
 Classes: type error in derived constructor returning non-undefined primitive value created in callee rather than caller context (and hence realm)](https://crbug.com/v8/4509)**  
**[Commit: [runtime] Throw exception for derived constructors in correct context.](https://chromium.googlesource.com/v8/v8/+/c86f189)**  
  
Date(Commit): Fri Jan 15 15:31:28 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1593553002](https://codereview.chromium.org/1593553002)  
Regress: [mjsunit/regress/regress-4509-Class-constructor-typeerror-realm.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4509-Class-constructor-typeerror-realm.js)  
```javascript
"use strict";
var realm = Realm.create();
var OtherTypeError = Realm.eval(realm, 'TypeError');

class Derived extends Object {
  constructor() {
    return null;
  }
}

assertThrows(() => { new Derived() }, TypeError);

var OtherDerived = Realm.eval(realm,
   "'use strict';" +
   "class Derived extends Object {" +
      "constructor() {" +
        "return null;" +
      "}};");

assertThrows(() => { new OtherDerived() }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c86f189^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=c86f189)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=c86f189)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=c86f189)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=c86f189)  
[src/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/builtins-mips.cc?cl=c86f189)  
...  
  

---   

## **regress-crbug-577112.js (chromium issue)**  
   
**[Issue 577112:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/577112)**  
**[Commit: [crankshaft] Don't inline array indexOf operations if receiver's proto is not a JSObject.](https://chromium.googlesource.com/v8/v8/+/1bb7cfd)**  
  
Date(Commit): Fri Jan 15 10:19:59 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1584303002](https://codereview.chromium.org/1584303002)  
Regress: [mjsunit/regress/regress-crbug-577112.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-577112.js)  
```javascript
Array.prototype.__proto__ = null;
var prototype = Array.prototype;
function f() {
  prototype.lastIndexOf({});
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1bb7cfd^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=1bb7cfd)  
[test/mjsunit/regress/regress-crbug-577112.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-577112.js?cl=1bb7cfd)  
  

---   

## **regress-4665.js (v8 issue)**  
   
**[Issue 4665:
 "Uncaught TypeError: this is not a typed array" thrown when modifying prototype](https://crbug.com/v8/4665)**  
**[Commit: Construct instances of base class from TypedArray.prototype.subarray](https://chromium.googlesource.com/v8/v8/+/e13f2ff)**  
  
Date(Commit): Thu Jan 14 19:23:26 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1583773005](https://codereview.chromium.org/1583773005)  
Regress: [mjsunit/regress/regress-4665.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4665.js)  
```javascript
function FirstBuffer () {}
FirstBuffer.prototype.__proto__ = Uint8Array.prototype
FirstBuffer.__proto__ = Uint8Array

var buf = new Uint8Array(10)
buf.__proto__ = FirstBuffer.prototype

assertThrows(() => buf.subarray(2), TypeError);


let seen_args = [];

function SecondBuffer (arg) {
  seen_args.push(arg);
  var arr = new Uint8Array(arg)
  arr.__proto__ = SecondBuffer.prototype
  return arr
}
SecondBuffer.prototype.__proto__ = Uint8Array.prototype
SecondBuffer.__proto__ = Uint8Array

var buf3 = new SecondBuffer(10)
assertEquals([10], seen_args);

var buf4 = buf3.subarray(2)

assertEquals(10, buf4.length);
assertEquals([10, buf3.buffer], seen_args);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e13f2ff^!)  
[src/js/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/js/typedarray.js?cl=e13f2ff)  
[test/mjsunit/es6/legacy-subclassing.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/legacy-subclassing.js?cl=e13f2ff)  
[test/mjsunit/regress/regress-4665.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4665.js?cl=e13f2ff)  
  

---   

## **regress-572589.js (chromium issue)**  
   
**[Issue 572589:
 old_target->kind() == new_target->kind() in src/objects-debug.cc](https://crbug.com/572589)**  
**[Commit: Propagate the "calls eval" bit from ScopeInfo to lazily-compiled arrow functions](https://chromium.googlesource.com/v8/v8/+/bcde4e2)**  
  
Date(Commit): Thu Jan 14 19:21:24 2016  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "merge-merged-4.9", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1575333004](https://codereview.chromium.org/1575333004)  
Regress: [mjsunit/regress/regress-572589.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-572589.js)  
```javascript
"use strict";
eval();
var f = ({x}) => { };
%OptimizeFunctionOnNextCall(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bcde4e2^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=bcde4e2)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=bcde4e2)  
[test/mjsunit/regress/regress-572589.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-572589.js?cl=bcde4e2)  
  

---   

## **regress-crbug-571149.js (chromium issue)**  
   
**[Issue 571149:
 Uninitialized closed over variable within a function with rest parameters causes assertion failure in ToBoolean IC.](https://crbug.com/571149)**  
**[Commit: Don't pre-initialise block contexts with holes](https://chromium.googlesource.com/v8/v8/+/92e6f7a)**  
  
Date(Commit): Thu Jan 14 18:04:35 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Via-Wizard", "merge-merged-4.9"]  
Code Review: [https://codereview.chromium.org/1584243002](https://codereview.chromium.org/1584243002)  
Regress: [mjsunit/harmony/regress/regress-crbug-571149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-571149.js)  
```javascript
(function(a = 0){
  var x;  // allocated in a var block, due to use of default parameter
  (function() { return !x })();
})();

(function({a}){
  var x;  // allocated in a var block, due to use of parameter destructuring
  (function() { return !x })();
})({});

(function(...a){
  var x;  // allocated in a var block, due to use of rest parameter
  (function() { return !x })();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92e6f7a^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=92e6f7a)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=92e6f7a)  
[test/mjsunit/harmony/regress/regress-crbug-571149.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-571149.js?cl=92e6f7a)  
  

---   

## **regress-crbug-575080.js (chromium issue)**  
   
**[Issue 575080:
 tagged_expected == tagged_actual in src/layout-descriptor.cc](https://crbug.com/575080)**  
**[Commit: Generalize all representations when reconfiguring a property of a strict Function subclass.](https://chromium.googlesource.com/v8/v8/+/405c7a6)**  
  
Date(Commit): Thu Jan 14 10:45:34 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1579603002](https://codereview.chromium.org/1579603002)  
Regress: [mjsunit/regress/regress-crbug-575080.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575080.js)  
```javascript
class A extends Function {
  constructor(...args) {
    super(...args);
    this.a = 42;
    this.d = 4.2;
    this.o = 0;
  }
}
var obj = new A("'use strict';");
obj.o = 0.1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/405c7a6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=405c7a6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=405c7a6)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=405c7a6)  
[test/mjsunit/regress/regress-crbug-575080.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575080.js?cl=405c7a6)  
  

---   

## **regress-576662.js (chromium issue)**  
   
**[Issue 576662:
 JSPROXY != state_ in src/lookup.cc](https://crbug.com/576662)**  
**[Commit: Gracefully handle proxies in AllCanWrite().](https://chromium.googlesource.com/v8/v8/+/863bf39)**  
  
Date(Commit): Tue Jan 12 14:56:54 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1580723003](https://codereview.chromium.org/1580723003)  
Regress: [mjsunit/es6/regress/regress-576662.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-576662.js)  
```javascript
Realm.create();
this.__proto__ = new Proxy({},{});
assertThrows(() => Realm.eval(1, "Realm.global(0).bla = 1"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/863bf39^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=863bf39)  
[test/mjsunit/harmony/regress/regress-576662.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-576662.js?cl=863bf39)  
  

---   

## **regress-crbug-575314.js (chromium issue)**  
   
**[Issue 575314:
 How to find the cause of the new " promiseCapability.[[Resolve]] is not a function" error](https://crbug.com/575314)**  
**[Commit: Partial rollback of Promise error checking](https://chromium.googlesource.com/v8/v8/+/ee9d7ac)**  
  
Date(Commit): Mon Jan 11 22:42:11 2016  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/1578893002](https://codereview.chromium.org/1578893002)  
Regress: [mjsunit/regress/regress-crbug-575314.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575314.js)  
```javascript
var test = new Promise(function(){});
test.constructor = function(){};
Promise.resolve(test).catch(e => %AbortJS(e + " FAILED!"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee9d7ac^!)  
[src/js/promise.js](https://cs.chromium.org/chromium/src/v8/src/js/promise.js?cl=ee9d7ac)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=ee9d7ac)  
[test/mjsunit/regress/regress-crbug-575314.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575314.js?cl=ee9d7ac)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=ee9d7ac)  
  

---   

## **regress-4577.js (v8 issue)**  
   
**[Issue 4577:
 Lexical variables named "arguments" erroneously disallowed in sloppy mode](https://crbug.com/v8/4577)**  
**[Commit: Add test showing broken-ness of non-simple parameter named 'arguments'](https://chromium.googlesource.com/v8/v8/+/067c27b)**  
  
Date(Commit): Fri Jan 08 20:29:46 2016  
Type: Bug  
Code Review: [https://codereview.chromium.org/1576503002](https://codereview.chromium.org/1576503002)  
Regress: [mjsunit/regress/regress-4577.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4577.js)  
```javascript
function f(...arguments) {
  return Array.isArray(arguments);
}
assertTrue(f());

function g({arguments}) {
  return arguments === 42;
}
assertTrue(g({arguments: 42}));

function foo() {
  let arguments = 2;
  return arguments;
}
assertEquals(2, foo());

assertThrows(function(x = arguments, arguments) {}, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/067c27b^!)  
[test/mjsunit/bugs/bug-4577.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-4577.js?cl=067c27b)  
  

---   

## **regress-crbug-569534.js (chromium issue)**  
   
**[Issue 569534:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray()) in src](https://crbug.com/569534)**  
**[Commit: Fix^3 cast in HasEnumerableElements](https://chromium.googlesource.com/v8/v8/+/a0d03d7)**  
  
Date(Commit): Thu Jan 07 14:47:27 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1568863002](https://codereview.chromium.org/1568863002)  
Regress: [mjsunit/regress/regress-crbug-569534.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-569534.js)  
```javascript
var array = [,0.5];
array.length = 0;
for (var i in array) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0d03d7^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a0d03d7)  
[test/mjsunit/regress/regress-crbug-569534.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-569534.js?cl=a0d03d7)  
  

---   

## **regress-crbug-575082.js (chromium issue)**  
   
**[Issue 575082:
 DaysFromYearMonth(*year, 0) + days == save_days in src/date.cc](https://crbug.com/575082)**  
**[Commit: [date] Date parser says true even for wrong dates, check twice.](https://chromium.googlesource.com/v8/v8/+/b0d0d57)**  
  
Date(Commit): Thu Jan 07 09:30:46 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1566973002](https://codereview.chromium.org/1566973002)  
Regress: [mjsunit/regress/regress-crbug-575082.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575082.js)  
```javascript
var y = new Date("-1073741824");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0d0d57^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=b0d0d57)  
[src/date.cc](https://cs.chromium.org/chromium/src/v8/src/date.cc?cl=b0d0d57)  
[test/mjsunit/regress/regress-crbug-575082.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575082.js?cl=b0d0d57)  
  

---   

## **regress-crbug-571517.js (chromium issue)**  
   
**[Issue 571517:
 Crash in v8::internal::JSObject::UnregisterPrototypeUser](https://crbug.com/571517)**  
**[Commit: [prototype user tracking] Don't skip JSGlobalProxies](https://chromium.googlesource.com/v8/v8/+/b4583c0)**  
  
Date(Commit): Tue Jan 05 16:15:48 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "M-49", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1559323002](https://codereview.chromium.org/1559323002)  
Regress: [mjsunit/regress/regress-crbug-571517.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-571517.js)  
```javascript
function Receiver() { this.receiver = "receiver"; }
function Proto() { this.proto = "proto"; }

function f(a) {
  return a.foo;
}

var rec = new Receiver();

var proto = rec.__proto__;

assertEquals(undefined, f(rec));
assertEquals(undefined, f(rec));

var p2 = new Proto();
p2.__proto__ = null;
proto.__proto__ = p2;

assertEquals(undefined, f(rec));

p2.foo = "bar";
assertEquals("bar", f(rec));

delete p2.foo;
p2.secret = "GAME OVER";
assertEquals(undefined, f(rec));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4583c0^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=b4583c0)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b4583c0)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=b4583c0)  
[test/mjsunit/regress/regress-crbug-571517.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-571517.js?cl=b4583c0)  
  

---   

## **regress-crbug-422858.js (chromium issue)**  
   
**[Issue 422858:
 Date.toLocalString() and new Date("str") are not complementary](https://crbug.com/422858)**  
**[Commit: Accept time zones like GMT-8 in the legacy date parser](https://chromium.googlesource.com/v8/v8/+/acbd64b)**  
  
Date(Commit): Mon Jan 04 23:25:57 2016  
Components/Type: Blink>JavaScript>Internationalization/Bug  
Labels: ["Via-Wizard", "Hotlist-Google"]  
Code Review: [https://codereview.chromium.org/1557053002](https://codereview.chromium.org/1557053002)  
Regress: [mjsunit/regress/regress-crbug-422858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-422858.js)  
```javascript
var date = new Date("2016/01/02 10:00 GMT-8")
assertEquals(0, date.getMinutes());
assertEquals(18, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-12")
assertEquals(0, date.getMinutes());
assertEquals(22, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-123")
assertEquals(23, date.getMinutes());
assertEquals(11, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-0856")
assertEquals(56, date.getMinutes());
assertEquals(18, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-08000")
assertEquals(NaN, date.getMinutes());
assertEquals(NaN, date.getUTCHours());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/acbd64b^!)  
[src/dateparser-inl.h](https://cs.chromium.org/chromium/src/v8/src/dateparser-inl.h?cl=acbd64b)  
[test/mjsunit/regress/regress-crbug-422858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-422858.js?cl=acbd64b)  
  

---   

## **regress-crbug-487322.js (chromium issue)**  
   
**[Issue 487322:
 DateTimeFormat does not resolve IANA time zone](https://crbug.com/487322)**  
**[Commit: Timezone name check fix](https://chromium.googlesource.com/v8/v8/+/4e18190)**  
  
Date(Commit): Mon Jan 04 21:48:04 2016  
Components/Type: Blink>JavaScript>Internationalization/Bug  
Labels: []  
Code Review: [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)  
Regress: [mjsunit/regress/regress-crbug-487322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-487322.js)  
```javascript
if (this.Intl) {
  // Normalizes Kat{h,}mandu (chromium:487322)
  // According to the IANA timezone db, Kathmandu is the current canonical
  // name, but ICU got it backward. To make this test robust against a future
  // ICU change ( http://bugs.icu-project.org/trac/ticket/12044 ),
  // just check that Kat(h)mandu is resolved identically.
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Katmandu'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Kathmandu'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  // Normalizes Ulan_Bator to Ulaanbaatar. Unlike Kat(h)mandu, ICU got this
  // right so that we make sure that Ulan_Bator is resolved to Ulaanbaatar.
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ulaanbaatar'})
  assertEquals('Asia/Ulaanbaatar', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ulan_Bator'})
  assertEquals('Asia/Ulaanbaatar', df.resolvedOptions().timeZone);

  // Throws for unsupported time zones.
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'Aurope/Paris'}));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e18190^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4e18190)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=4e18190)  
[test/mjsunit/regress/regress-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-487322.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-364374.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-364374.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487322.js?cl=4e18190)  
  

---   

## **regress-crbug-364374.js (chromium issue)**  
   
**[Issue 364374:
 Some official time zone IDs  are not accepted by Intl.DateTimeFormat.](https://crbug.com/364374)**  
**[Commit: Timezone name check fix](https://chromium.googlesource.com/v8/v8/+/4e18190)**  
  
Date(Commit): Mon Jan 04 21:48:04 2016  
Components/Type: Blink>JavaScript>Internationalization/Bug  
Labels: []  
Code Review: [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)  
Regress: [mjsunit/regress/regress-crbug-364374.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-364374.js)  
```javascript
if (this.Intl) {
  // chromium:364374

  // Locations with 2 underscores are accepted and normalized.
  // 'of' and 'es' are always lowercased.
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'eUrope/isLe_OF_man'})
  assertEquals('Europe/Isle_of_Man', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'africa/Dar_eS_salaam'})
  assertEquals('Africa/Dar_es_Salaam', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/port_of_spain'})
  assertEquals('America/Port_of_Spain', df.resolvedOptions().timeZone);

  // Zone ids with more than 2 parts are accepted and normalized.
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/north_Dakota/new_salem'})
  assertEquals('America/North_Dakota/New_Salem', df.resolvedOptions().timeZone);

  // 3-part zone IDs are accepted and normalized.
  // Two Buenose Aires aliases are identical.
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/aRgentina/buenos_aIres'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Argentina/Buenos_Aires'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Buenos_Aires'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Indiana/Indianapolis'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Indianapolis'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  // ICU does not recognize East-Indiana. Add later when it does.
  // df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/East-Indiana'})
  // assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);


  // Zone IDs with hyphens. 'au' has to be in lowercase.
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/port-aU-pRince'})
  assertEquals('America/Port-au-Prince', df.resolvedOptions().timeZone);

  // Accepts Ho_Chi_Minh and treats it as identical to Saigon
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ho_Chi_Minh'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Saigon'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  // Throws for invalid timezone ids.
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'Europe/_Paris'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New__York'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America//New_York'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New_York_'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New_Y0rk'}));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e18190^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4e18190)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=4e18190)  
[test/mjsunit/regress/regress-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-487322.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-364374.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-364374.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487322.js?cl=4e18190)  
  

---   

## **regress-crbug-573857.js (chromium issue)**  
   
**[Issue 573857:
 p->IsSmi() in src/objects-debug.cc](https://crbug.com/573857)**  
**[Commit: Use JSObjectVerify instead of trying to reimplement parts of it.](https://chromium.googlesource.com/v8/v8/+/fed2c41)**  
  
Date(Commit): Mon Jan 04 13:50:06 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1551333002](https://codereview.chromium.org/1551333002)  
Regress: [mjsunit/regress/regress-crbug-573857.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-573857.js)  
```javascript
function f() {}
f = f.bind();
f.x = f.name;
f.__defineGetter__('name', function() { return f.x; });
function g() {}
g.prototype = f;
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fed2c41^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=fed2c41)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=fed2c41)  
[test/mjsunit/regress/regress-crbug-573857.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-573857.js?cl=fed2c41)  
  

---   

## **regress-crbug-573858.js (chromium issue)**  
   
**[Issue 573858:
 constructor->shared()->construct_stub() == isolate()->builtins()->builtin(Builti](https://crbug.com/573858)**  
**[Commit: ThrowTypeError should not be constructable, so shouldn't have a prototype.](https://chromium.googlesource.com/v8/v8/+/09c41d9)**  
  
Date(Commit): Mon Jan 04 13:33:09 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1552223002](https://codereview.chromium.org/1552223002)  
Regress: [mjsunit/regress/regress-crbug-573858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-573858.js)  
```javascript
var throw_type_error = Object.getOwnPropertyDescriptor(
    (function() {"use strict"}).__proto__, "caller").get;

function create_initial_map() { this instanceof throw_type_error }
%OptimizeFunctionOnNextCall(create_initial_map);
assertThrows(create_initial_map);

function test() { new throw_type_error }
%OptimizeFunctionOnNextCall(test);
assertThrows(test);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09c41d9^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=09c41d9)  
[test/mjsunit/regress/regress-crbug-573858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-573858.js?cl=09c41d9)  
[test/mjsunit/strict-mode.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/strict-mode.js?cl=09c41d9)  
  

---   

## **regress-572409.js (chromium issue)**  
   
**[Issue 572409:
 Crash in v8::internal::InnerPointerToCodeCache::GcSafeFindCodeForInnerPointer](https://crbug.com/572409)**  
**[Commit: [turbofan] Add deopt point for InternalSetPrototype in VisitObjectLiteral.](https://chromium.googlesource.com/v8/v8/+/140f69d)**  
  
Date(Commit): Mon Jan 04 09:54:51 2016  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "M-49", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/1555023002](https://codereview.chromium.org/1555023002)  
Regress: [mjsunit/compiler/regress-572409.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-572409.js)  
```javascript
var o = function() {};
function f() {
  var lit = { __proto__: o  };
  o instanceof RegExp;
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/140f69d^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=140f69d)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=140f69d)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=140f69d)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=140f69d)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=140f69d)  
...  
  

---   

## **regress-crbug-565917.js (chromium issue)**  
   
**[Issue 565917:
 2 == args.length() in src/builtins.cc](https://crbug.com/565917)**  
**[Commit: [es6] Unify ArrayBuffer and SharedArrayBuffer constructors.](https://chromium.googlesource.com/v8/v8/+/cb21144)**  
  
Date(Commit): Fri Jan 01 07:13:16 2016  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://crrev.com/3235ccbb7826ceec2188f6ebab98fc851b54f60e](https://crrev.com/3235ccbb7826ceec2188f6ebab98fc851b54f60e)  
Regress: [mjsunit/regress/regress-crbug-565917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-565917.js)  
```javascript
try {
} catch(e) {; }
new ArrayBuffer();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb21144^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=cb21144)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=cb21144)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=cb21144)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=cb21144)  
[src/js/arraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/js/arraybuffer.js?cl=cb21144)  
...  
  

---   
