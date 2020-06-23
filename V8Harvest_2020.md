# V8Harvest  
The Harvest of V8 regress in 2020.  
  

## **regress-crbug-1053364.js (chromium issue)**  
   
**[Issue: Bytecode mismatch for anonymous function](https://crbug.com/1053364)**  
**[Commit: [parser] Don't mark sloppy block functions as assigned](https://chromium.googlesource.com/v8/v8/+/92cd4d1)**  
  
Date(Commit): Thu Jun 18 16:24:48 2020  
Components: Blink>JavaScript>Interpreter  
Labels: ClusterFuzz-Verified, Via-Wizard-Security, bytecode-mismatch  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2241511](https://chromium-review.googlesource.com/c/v8/v8/+/2241511)  
Regress: [mjsunit/regress/regress-crbug-1053364.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1053364.js)  
```javascript
function main() {
  function g() {
    function h() {
      f;
    }
    {
      function f() {}
    }
    f;
    throw new Error();
  }
  g();
}
main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92cd4d1^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=92cd4d1)  
[test/mjsunit/regress/regress-crbug-1053364.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1053364.js?cl=92cd4d1)  
  

---   

## **regress-v8-10604.js (v8 issue)**  
   
**[Issue: d8 crash in in ReportApiFailure () at v8/src/api/api.cc:488](https://crbug.com/v8/10604)**  
**[Commit: [d8] Fix Realm.eval script origin](https://chromium.googlesource.com/v8/v8/+/10e713b)**  
  
Date(Commit): Tue Jun 16 16:45:13 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2246564](https://chromium-review.googlesource.com/c/v8/v8/+/2246564)  
Regress: [mjsunit/regress/regress-v8-10604.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10604.js)  
```javascript
(async function test() {
  try {
    import('does_not_exist.mjs');
    assertUnreachable();
  } catch {};

  try {
    await eval("import('does_not_exist.mjs')");
    assertUnreachable();
  } catch {};

  try {
    await Realm.eval(Realm.create(), "import('does_not_exist.mjs')");
    assertUnreachable();
  } catch {};
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10e713b^!)  
[src/d8/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8/d8.cc?cl=10e713b)  
[test/mjsunit/regress/regress-v8-10604.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10604.js?cl=10e713b)  
  

---   

## **regress-1094226.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1094226)**  
**[Commit: [scanner] Update outdated DCHECK](https://chromium.googlesource.com/v8/v8/+/9aa3c60)**  
  
Date(Commit): Mon Jun 15 07:21:43 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2245592](https://chromium-review.googlesource.com/c/v8/v8/+/2245592)  
Regress: [mjsunit/regress/regress-1094226.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1094226.js)  
```javascript
assertThrows(() => eval('<' + '!'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9aa3c60^!)  
[src/parsing/scanner.h](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.h?cl=9aa3c60)  
[test/mjsunit/regress/regress-1094226.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1094226.js?cl=9aa3c60)  
  

---   

## **regress-1094132.js (chromium issue)**  
   
**[Issue: CHECK failure: previously_materialized_objects->get(i) == *value in deoptimizer.cc](https://crbug.com/1094132)**  
**[Commit: [deoptimizer] Relax a CHECK](https://chromium.googlesource.com/v8/v8/+/92012d0)**  
  
Date(Commit): Fri Jun 12 09:40:39 2020  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2241517](https://chromium-review.googlesource.com/c/v8/v8/+/2241517)  
Regress: [mjsunit/compiler/regress-1094132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1094132.js)  
```javascript
function prettyPrinted() {}

function formatFailureText() {
  if (expectedText.length <= 40 && foundText.length <= 40) {
    message += ": expected <" + expectedText + "> found <" + foundText + ">";
    message += ":\nexpected:\n" + expectedText + "\nfound:\n" + foundText;
  }
}

function fail(expectedText, found, name_opt) {
  formatFailureText(expectedText, found, name_opt);
  if (!a[aProps[i]][aProps[i]]) { }
}

function deepEquals(a, b) {
  if (a === 0) return 1 / a === 1 / b;
  if (typeof a !== typeof a) return false;
  if (typeof a !== "object" && typeof a !== "function") return false;
  if (objectClass !== classOf()) return false;
  if (objectClass === "RegExp") { }
}

function assertEquals() {
  if (!deepEquals()) {
    fail(prettyPrinted(), undefined, undefined);
  }
}

({y: {}, x: 0.42});

function gaga() {
  return {gx: bar.arguments[0], hx: baz.arguments[0]};
}

function baz() {
  return gaga();
}

function bar(obj) {
  return baz(obj.y);
}

function foo() {
  bar({y: {}, x: 42});
  try { assertEquals() } catch (e) {}
  try { assertEquals() } catch (e) {}
  assertEquals();
}

%PrepareFunctionForOptimization(prettyPrinted);
%PrepareFunctionForOptimization(formatFailureText);
%PrepareFunctionForOptimization(fail);
%PrepareFunctionForOptimization(deepEquals);
%PrepareFunctionForOptimization(assertEquals);
%PrepareFunctionForOptimization(gaga);
%PrepareFunctionForOptimization(baz);
%PrepareFunctionForOptimization(bar);
%PrepareFunctionForOptimization(foo);
try { foo() } catch (e) {}
%OptimizeFunctionOnNextCall(foo);
try { foo() } catch (e) {}
%PrepareFunctionForOptimization(prettyPrinted);
%PrepareFunctionForOptimization(formatFailureText);
%PrepareFunctionForOptimization(fail);
%PrepareFunctionForOptimization(deepEquals);
%PrepareFunctionForOptimization(assertEquals);
%PrepareFunctionForOptimization(gaga);
%PrepareFunctionForOptimization(baz);
%PrepareFunctionForOptimization(bar);
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
try { foo() } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92012d0^!)  
[src/deoptimizer/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimizer.cc?cl=92012d0)  
[test/mjsunit/compiler/regress-1094132.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1094132.js?cl=92012d0)  
  

---   

## **regress-1092011.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1092011)**  
**[Commit: [runtime] Fix reentrancy bug in JSFunction::EnsureHasInitialMap](https://chromium.googlesource.com/v8/v8/+/0817d7e)**  
  
Date(Commit): Wed Jun 10 13:43:07 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2238570](https://chromium-review.googlesource.com/c/v8/v8/+/2238570)  
Regress: [mjsunit/compiler/regress-1092011.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1092011.js)  
```javascript
var __caught = 0;

(function main() {
  function foo(f) {
    const pr = new Promise(function executor() {
      f(function resolvefun() {
        try {
          throw 42;
        } catch (e) {
          __caught++;
        }
      }, function rejectfun() {});
    });
    pr.__proto__ = foo.prototype;
    return pr;
  }
  foo.__proto__ = Promise;
  foo.prototype.then = function thenfun() {};
  new foo();
  foo.prototype = undefined;
  foo.all([foo.resolve()]);
})();

assertEquals(2, __caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0817d7e^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=0817d7e)  
[src/objects/js-objects.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.h?cl=0817d7e)  
[test/mjsunit/compiler/regress-1092011.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1092011.js?cl=0817d7e)  
  

---   

## **regress-v8-10568.js (v8 issue)**  
   
**[Issue: Incorrect CharacterRange  when UTF16 surrogate pair is present](https://crbug.com/v8/10568)**  
**[Commit: [regexp] Fix integer overflows in TextNode::GetQuickCheckDetails](https://chromium.googlesource.com/v8/v8/+/a305d2d)**  
  
Date(Commit): Wed Jun 10 12:22:47 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2230527](https://chromium-review.googlesource.com/c/v8/v8/+/2230527)  
Regress: [mjsunit/regress/regress-v8-10568.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10568.js)  
```javascript
assertEquals(/[--𐀾]/u.exec("Hr3QoS3KCWXQ2yjBoDIK")[0], "H");
assertEquals(/[0-\u{10000}]/u.exec("A0")[0], "A");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a305d2d^!)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=a305d2d)  
[src/regexp/regexp-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.cc?cl=a305d2d)  
[src/regexp/regexp-compiler.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.h?cl=a305d2d)  
[src/regexp/regexp-dotprinter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-dotprinter.cc?cl=a305d2d)  
[test/mjsunit/regress/regress-v8-10568.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10568.js?cl=a305d2d)  
  

---   

## **regress-1092896.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1092896)**  
**[Commit: [string] Don't skip GetMethod on Smis in String builtins](https://chromium.googlesource.com/v8/v8/+/b527305)**  
  
Date(Commit): Wed Jun 10 09:47:10 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2238030](https://chromium-review.googlesource.com/c/v8/v8/+/2238030)  
Regress: [mjsunit/regress/regress-1092896.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1092896.js)  
```javascript
let count = 0
Object.prototype[Symbol.replace] = function () {
  count++
}

"".replace(0);
assertEquals(count, 1);

"".replace(0.1);
assertEquals(count, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b527305^!)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=b527305)  
[test/mjsunit/regress/regress-1092896.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1092896.js?cl=b527305)  
  

---   

## **regress-1092650.js (chromium issue)**  
   
**[Issue: CHECK failure: object->IsHeapObject() in deoptimizer.cc](https://crbug.com/1092650)**  
**[Commit: [deoptimizer] Add missing HeapNumber allocation](https://chromium.googlesource.com/v8/v8/+/ebfb877)**  
  
Date(Commit): Tue Jun 09 15:04:07 2020  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Clusterfuzz, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2237145](https://chromium-review.googlesource.com/c/v8/v8/+/2237145)  
Regress: [mjsunit/compiler/regress-1092650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1092650.js)  
```javascript
({a: 2**30});

function foo() {
  return foo.arguments[0];
}

function main() {
  foo({a: 42});
}

%PrepareFunctionForOptimization(foo);
%PrepareFunctionForOptimization(main);
main();
main();
%OptimizeFunctionOnNextCall(main);
main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ebfb877^!)  
[src/deoptimizer/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimizer.cc?cl=ebfb877)  
[test/mjsunit/compiler/regress-1092650.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1092650.js?cl=ebfb877)  
  

---   

## **regress-1091461.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1091461)**  
**[Commit: [turbofan] Make BigInt operations kNoThrow again](https://chromium.googlesource.com/v8/v8/+/a99ca6e)**  
  
Date(Commit): Tue Jun 09 11:24:09 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2235113](https://chromium-review.googlesource.com/c/v8/v8/+/2235113)  
Regress: [mjsunit/regress/regress-1091461.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1091461.js)  
```javascript
function foo(a, b) {
  let x = a + b;
}
function test() {
  try {
    foo(1n, 1n);
  } catch (e) {}
}

%PrepareFunctionForOptimization(foo);
%PrepareFunctionForOptimization(test);
test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a99ca6e^!)  
[src/compiler/js-type-hint-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-type-hint-lowering.h?cl=a99ca6e)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=a99ca6e)  
[test/mjsunit/regress/regress-1091461.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1091461.js?cl=a99ca6e)  
  

---   

## **regress-1084820.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1084820)**  
**[Commit: [deoptimizer] Fix bug in object materialization](https://chromium.googlesource.com/v8/v8/+/d8bc336)**  
  
Date(Commit): Mon Jun 08 15:48:41 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2228330](https://chromium-review.googlesource.com/c/v8/v8/+/2228330)  
Regress: [mjsunit/compiler/regress-1084820.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1084820.js)  
```javascript
const dummy_obj = {};
dummy_obj.my_property = 'some HeapObject';
dummy_obj.my_property = 'some other HeapObject';

function gaga() {
  const obj = {};
  // Store a HeapNumber and then a Smi.
  // This must happen in a loop, even if it's only 2 iterations:
  for (let j = -3_000_000_000; j <= -1_000_000_000; j += 2_000_000_000) {
    obj.my_property = j;
  }
  // Trigger (soft) deopt.
  if (!%IsBeingInterpreted()) obj + obj;
}

%PrepareFunctionForOptimization(gaga);
gaga();
gaga();
%OptimizeFunctionOnNextCall(gaga);
gaga();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8bc336^!)  
[src/deoptimizer/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimizer.cc?cl=d8bc336)  
[src/deoptimizer/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimizer.h?cl=d8bc336)  
[test/mjsunit/compiler/regress-1084820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1084820.js?cl=d8bc336)  
  

---   

## **regress-1073440.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1073440)**  
**[Commit: [turbofan] Fix lost exception on BigInt ops](https://chromium.googlesource.com/v8/v8/+/ca54b83)**  
  
Date(Commit): Thu Jun 04 15:32:29 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2228493](https://chromium-review.googlesource.com/c/v8/v8/+/2228493)  
Regress: [mjsunit/regress/regress-1073440.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1073440.js)  
```javascript
function foo(n) {
  try {
    let v = 0n;
    for (let i = 0; i < n; ++i) {
      v = 3n + v;
      v = i;
    }
  } catch (e) {
    return 1;
  }
  return 0;
}

%PrepareFunctionForOptimization(foo);
assertEquals(foo(1), 0);
assertEquals(foo(1), 0);
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(1), 0);
assertOptimized(foo);
%PrepareFunctionForOptimization(foo);
assertEquals(foo(2), 1);
assertUnoptimized(foo);
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(1), 0);
assertOptimized(foo);
assertEquals(foo(2), 1);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca54b83^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=ca54b83)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=ca54b83)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=ca54b83)  
[src/deoptimizer/deoptimize-reason.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimize-reason.h?cl=ca54b83)  
[src/ic/binary-op-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/binary-op-assembler.cc?cl=ca54b83)  
...  
  

---   

## **regress-crbug-1082293.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_no_ic](https://crbug.com/1082293)**  
**[Commit: [ic] Fix a bug in StoreOwnIC when storing NaN values](https://chromium.googlesource.com/v8/v8/+/b613355)**  
  
Date(Commit): Thu Jun 04 09:35:22 2020  
Components: Blink>JavaScript>Runtime, Blink>JavaScript>Compiler  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2228886](https://chromium-review.googlesource.com/c/v8/v8/+/2228886)  
Regress: [mjsunit/regress/regress-crbug-1082293.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1082293.js)  
```javascript
function f() {
  let buffer = new ArrayBuffer(8);
  let a32 = new Float32Array(buffer);
  let a8 = new Uint32Array(buffer);
  let a = { value: NaN };
  Object.defineProperty(a32, 0, { value: NaN });
  return a8[0];
}

let value = f();
%EnsureFeedbackVectorForFunction(f);
assertEquals(value, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b613355^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=b613355)  
[test/mjsunit/regress/regress-crbug-1082293.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1082293.js?cl=b613355)  
  

---   

## **regress-9441.js (v8 issue)**  
   
**[Issue: Loss of feedback when throwing in Generate_AddWithFeedback](https://crbug.com/v8/9441)**  
**[Commit: Fix feedback loss when builtins throw](https://chromium.googlesource.com/v8/v8/+/fd5cc88)**  
  
Date(Commit): Thu May 28 12:20:37 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2202904](https://chromium-review.googlesource.com/c/v8/v8/+/2202904)  
Regress: [mjsunit/regress/regress-9441.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9441.js)  
```javascript
function foo(a, b) {
    return a - b;
}

%PrepareFunctionForOptimization(foo);
assertEquals(-1n, foo(1n, 2n));
%OptimizeFunctionOnNextCall(foo);
assertEquals(1n, foo(2n, 1n));
assertOptimized(foo);
assertThrows(() => foo(2n, undefined));
assertUnoptimized(foo);
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
assertEquals(-1n, foo(1n, 2n));
assertOptimized(foo);
assertThrows(() => foo(undefined, 2n));
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd5cc88^!)  
[src/ic/binary-op-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/binary-op-assembler.cc?cl=fd5cc88)  
[test/mjsunit/regress/regress-9441.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9441.js?cl=fd5cc88)  
  

---   

## **regress-1086470.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1086470)**  
**[Commit: Fix assert caused by SloppyArgumentsElements introduction](https://chromium.googlesource.com/v8/v8/+/fbb8dc4)**  
  
Date(Commit): Tue May 26 18:01:44 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2215059](https://chromium-review.googlesource.com/c/v8/v8/+/2215059)  
Regress: [mjsunit/regress/regress-1086470.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1086470.js)  
```javascript
function __f_1( __v_9) {
  return arguments;
}

var __v_48 = [];
var __v_49 = __f_1();
var __v_50 = 3;
Object.preventExtensions(__v_49);
function __f_7(__v_52, __v_53, __v_54) {
  __v_52[__v_53] =
  __v_54;
}
__v_48.__proto__ = __v_49;
for (var __v_51 = 0; __v_51 < __v_50; __v_51++) {
  __f_7(__v_48, __v_51);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fbb8dc4^!)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=fbb8dc4)  
[test/mjsunit/regress/regress-1086470.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1086470.js?cl=fbb8dc4)  
  

---   

## **regress-1084953.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1084953)**  
**[Commit: [TurboProp] Don't try to rewire unreachable blocks to end.](https://chromium.googlesource.com/v8/v8/+/f34771f)**  
  
Date(Commit): Mon May 25 10:42:52 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2209061](https://chromium-review.googlesource.com/c/v8/v8/+/2209061)  
Regress: [mjsunit/regress/regress-1084953.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1084953.js)  
```javascript
function foo() {
  try {
    +Symbol();
  } catch {
  }
}
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f34771f^!)  
[src/compiler/backend/instruction-selector-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/instruction-selector-impl.h?cl=f34771f)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=f34771f)  
[src/compiler/graph-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.cc?cl=f34771f)  
[test/mjsunit/regress/regress-1083272.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083272.js?cl=f34771f)  
[test/mjsunit/regress/regress-1083763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083763.js?cl=f34771f)  
...  
  

---   

## **regress-1083763.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1083763)**  
**[Commit: [TurboProp] Don't try to rewire unreachable blocks to end.](https://chromium.googlesource.com/v8/v8/+/f34771f)**  
  
Date(Commit): Mon May 25 10:42:52 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2209061](https://chromium-review.googlesource.com/c/v8/v8/+/2209061)  
Regress: [mjsunit/regress/regress-1083763.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1083763.js)  
```javascript
function bar() {}

function foo() {
  try {
    bar( "abc".charAt(4));
  } catch (e) {}
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f34771f^!)  
[src/compiler/backend/instruction-selector-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/instruction-selector-impl.h?cl=f34771f)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=f34771f)  
[src/compiler/graph-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.cc?cl=f34771f)  
[test/mjsunit/regress/regress-1083272.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083272.js?cl=f34771f)  
[test/mjsunit/regress/regress-1083763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083763.js?cl=f34771f)  
...  
  

---   

## **regress-1083272.js (chromium issue)**  
   
**[Issue: Crash in EffectControlLinearizer::UpdateBlockControl](https://crbug.com/1083272)**  
**[Commit: [TurboProp] Don't try to rewire unreachable blocks to end.](https://chromium.googlesource.com/v8/v8/+/f34771f)**  
  
Date(Commit): Mon May 25 10:42:52 2020  
Components: Blink>JavaScript>Compiler  
Labels: ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2209061](https://chromium-review.googlesource.com/c/v8/v8/+/2209061)  
Regress: [mjsunit/regress/regress-1083272.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1083272.js)  
```javascript
function foo(e, t) {
    for (var n = [e], s = e.length; s > 0; --s) {}
    for (var s = 0; s < n.length; s++) { t() }
}

var e = 'abc';
function t() {};

%PrepareFunctionForOptimization(foo);
foo(e, t);
foo(e, t);
%OptimizeFunctionOnNextCall(foo);
foo(e, t);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f34771f^!)  
[src/compiler/backend/instruction-selector-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/instruction-selector-impl.h?cl=f34771f)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=f34771f)  
[src/compiler/graph-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.cc?cl=f34771f)  
[test/mjsunit/regress/regress-1083272.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083272.js?cl=f34771f)  
[test/mjsunit/regress/regress-1083763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083763.js?cl=f34771f)  
...  
  

---   

## **regress-1084872.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/1084872)**  
**[Commit: [regexp] Fix signed/unsigned confusion in regexp interpreter](https://chromium.googlesource.com/v8/v8/+/1372e35)**  
  
Date(Commit): Wed May 20 13:44:21 2020  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://crrev.com/c/2207137.](https://crrev.com/c/2207137.)  
Regress: [mjsunit/regress/regress-1084872.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1084872.js)  
```javascript
const re = new RegExp("(?<=(.)\\2.*(T))");
re.exec(undefined);
assertEquals(re.exec("bTaLTT")[1], "b");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1372e35^!)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=1372e35)  
[test/mjsunit/regress/regress-1084872.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1084872.js?cl=1372e35)  
  

---   

## **regress-1084151.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1084151)**  
**[Commit: [liftoff][mv] Fix merge issue in multi-value loops](https://chromium.googlesource.com/v8/v8/+/9d06369)**  
  
Date(Commit): Tue May 19 15:43:50 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2208851](https://chromium-review.googlesource.com/c/v8/v8/+/2208851)  
Regress: [mjsunit/regress/wasm/regress-1084151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1084151.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let builder = new WasmModuleBuilder();
let sig_i_iii = builder.addType(kSig_i_iii);

builder.addFunction("main", sig_i_iii)
  .addBody([
    kExprLocalGet, 1,
    kExprLocalGet, 1,
    kExprI32Const, 5,
    kExprLoop, sig_i_iii,
      kExprLocalGet, 1,
      kExprBlock, sig_i_iii,
        kExprLocalGet, 1,
        kExprLocalGet, 2,
        kExprBrIf, 1,
        kExprDrop,
        kExprDrop,
        kExprDrop,
      kExprEnd,
      kExprDrop,
    kExprEnd])
  .exportAs("main");

let module = new WebAssembly.Module(builder.toBuffer());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9d06369^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=9d06369)  
[test/mjsunit/regress/wasm/regress-1084151.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1084151.js?cl=9d06369)  
  

---   

## **regress-1083450.js (chromium issue)**  
   
**[Issue: Security: Assertion failure in RegExpBytecodeGenerator](https://crbug.com/1083450)**  
**[Commit: [regexp] Specify signedness when accessing packed arguments](https://chromium.googlesource.com/v8/v8/+/508569f)**  
  
Date(Commit): Tue May 19 05:25:15 2020  
Components: Blink>JavaScript>Regexp  
Labels: Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2207137](https://chromium-review.googlesource.com/c/v8/v8/+/2207137)  
Regress: [mjsunit/regress/regress-1083450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1083450.js)  
```javascript
const source = "(?<=(?=ab)(|)abc)"
const re = new RegExp(source);
assertNotNull(re.exec("abc"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/508569f^!)  
[src/regexp/regexp-bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-bytecode-generator.cc?cl=508569f)  
[src/regexp/regexp-bytecodes.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-bytecodes.h?cl=508569f)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=508569f)  
[test/mjsunit/regress/regress-1083450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1083450.js?cl=508569f)  
  

---   

## **regress-1082704.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1082704)**  
**[Commit: [turbofan] Make GraphAssembler branching respect typing](https://chromium.googlesource.com/v8/v8/+/349e4ee)**  
  
Date(Commit): Fri May 15 12:50:11 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2202978](https://chromium-review.googlesource.com/c/v8/v8/+/2202978)  
Regress: [mjsunit/compiler/regress-1082704.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1082704.js)  
```javascript
var array = [[]];
function foo() {
  const x = array[0];
  const y = [][0];
  return x == y;
}
%PrepareFunctionForOptimization(foo);
assertFalse(foo());
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/349e4ee^!)  
[src/compiler/graph-assembler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.h?cl=349e4ee)  
[test/mjsunit/compiler/regress-1082704.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1082704.js?cl=349e4ee)  
  

---   

## **regress-1079446.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1079446)**  
**[Commit: [Turboprop] Allow removal of multiple unreachable blocks that merge.](https://chromium.googlesource.com/v8/v8/+/d9828e4)**  
  
Date(Commit): Thu May 14 21:22:35 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2196354](https://chromium-review.googlesource.com/c/v8/v8/+/2196354)  
Regress: [mjsunit/regress/regress-1079446.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1079446.js)  
```javascript
arr = new Int16Array();
function foo() {
  arr.__defineGetter__('a', function() { });
  arr[0] = "123.12";
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9828e4^!)  
[src/compiler/graph-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.cc?cl=d9828e4)  
[test/mjsunit/regress/regress-1079446.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1079446.js?cl=d9828e4)  
  

---   

## **regress-1081030.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: CHECK failure: interpreter_result.result() == result_compiled in wasm-fuzzer-common.cc](https://crbug.com/1081030)**  
**[Commit: [wasm-simd][ia32] Fix f32x4.min AVX implementation](https://chromium.googlesource.com/v8/v8/+/6a6ec7a)**  
  
Date(Commit): Wed May 13 22:54:53 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2194471](https://chromium-review.googlesource.com/c/v8/v8/+/2194471)  
Regress: [mjsunit/regress/wasm/regress-1081030.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1081030.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */).addBodyWithEnd([
  // signature: i_iii
  // body:
  kExprF32Const, 0xf8, 0xf8, 0xf8, 0xf8,
  kSimdPrefix, kExprF32x4Splat,         // f32x4.splat
  kExprF32Const, 0xf8, 0xf8, 0xf8, 0xf8,
  kSimdPrefix, kExprF32x4Splat,         // f32x4.splat
  kSimdPrefix, kExprF32x4Min, 0x01,     // f32x4.min
  kSimdPrefix, kExprV32x4AnyTrue, 0x01,  // i32x4.any_true
  kExprEnd,                             // end @16
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(1, instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6a6ec7a^!)  
[src/compiler/backend/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/ia32/code-generator-ia32.cc?cl=6a6ec7a)  
[test/mjsunit/regress/wasm/regress-1081030.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1081030.js?cl=6a6ec7a)  
[test/mjsunit/wasm/wasm-module-builder.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-module-builder.js?cl=6a6ec7a)  
  

---   

## **regress-crbug-1069530.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1069530)**  
**[Commit: [ic] Properly handle store mode generalization in KeyedStoreIC](https://chromium.googlesource.com/v8/v8/+/bf25184)**  
  
Date(Commit): Wed May 13 15:14:21 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2196353](https://chromium-review.googlesource.com/c/v8/v8/+/2196353)  
Regress: [mjsunit/regress/regress-crbug-1069530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1069530.js)  
```javascript
function store(ar, index) {
  ar[index] = "a";
}

let growable_array = [];

store(growable_array, 0);
store(growable_array, 1);
store(growable_array, 2);
store(growable_array, 3);

var array = [];
Object.defineProperty(array, "length", { value: 3, writable: false });

store(array, 0);
store(array, 1);

store(array, 3);
assertEquals(undefined, array[3]);
assertEquals(3, array.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf25184^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=bf25184)  
[src/objects/js-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array.h?cl=bf25184)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=bf25184)  
[test/mjsunit/regress/regress-crbug-1069530.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1069530.js?cl=bf25184)  
  

---   

## **regress-v8-10513.js (v8 issue)**  
   
**[Issue: RegExp.prototype[@@replace] with named captures does not call proxied Get for undefined names](https://crbug.com/v8/10513)**  
**[Commit: [regexp] Unconditionally get named capture in GetSubstitution](https://chromium.googlesource.com/v8/v8/+/4d53833)**  
  
Date(Commit): Tue May 12 08:45:05 2020  
Code Review: [https://tc39.es/ecma262/#sec-getsubstitution.](https://tc39.es/ecma262/#sec-getsubstitution.)  
Regress: [mjsunit/regress/regress-v8-10513.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10513.js)  
```javascript
const access_log = [];
const handler = {
  get: function(obj, prop) {
    access_log.push(prop);
    return prop in obj ? obj[prop] : "z";
  }
};

class ProxiedGroupRegExp extends RegExp {
  exec(s) {
    var result = super.exec(s);
    if (result) {
      result.groups = new Proxy(result.groups, handler);
    }
    return result;
  }
}

let re = new ProxiedGroupRegExp("(?<x>.)");
assertEquals("a z", "a".replace(re, "$<x> $<y>"));
assertEquals(["x", "y"], access_log);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4d53833^!)  
[src/objects/string.cc](https://cs.chromium.org/chromium/src/v8/src/objects/string.cc?cl=4d53833)  
[src/objects/string.h](https://cs.chromium.org/chromium/src/v8/src/objects/string.h?cl=4d53833)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=4d53833)  
[test/mjsunit/regress/regress-v8-10513.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10513.js?cl=4d53833)  
  

---   

## **regress-crbug-1055138-1.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo:arm,ignition_turbo](https://crbug.com/1055138)**  
**[Commit: [ic] Fix stores to holey elements](https://chromium.googlesource.com/v8/v8/+/ae6c58c)**  
  
Date(Commit): Mon May 11 16:42:19 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Security_Impact-Stable, Clusterfuzz, ClusterFuzz-Verified, ClusterFuzz-Wrong, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2172090](https://chromium-review.googlesource.com/c/v8/v8/+/2172090)  
Regress: [mjsunit/regress/regress-crbug-1055138-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1055138-1.js), [mjsunit/regress/regress-crbug-1055138-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1055138-2.js), [mjsunit/regress/regress-crbug-1055138-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1055138-3.js)  
```javascript
Object.prototype[1] = 153;
Object.freeze(Object.prototype);

(function TestSloppyStoreToReadOnlyProperty() {
  function foo() {
    let ar = [];
    for (let i = 0; i < 3; i++) {
      ar[i] = 42;

      if (i == 1) {
        // Attempt to overwrite read-only element should not change
        // array length.
        assertEquals(1, ar.length);
      } else {
        assertEquals(i + 1, ar.length);
      }
    }
    return ar;
  }

  assertEquals([42,153,42], foo());
  assertEquals([42,153,42], foo());
  assertEquals([42,153,42], foo());
  %PrepareFunctionForOptimization(foo);
  assertEquals([42,153,42], foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals([42,153,42], foo());
})();

(function StrictStoreToReadOnlyProperty() {
  function foo() {
    "use strict";
    let ar = [];
    let threw_exception = false;
    for (let i = 0; i < 3; i++) {
      try {
        ar[i] = 42;
      } catch(e) {
        // Attempt to overwrite read-only element should throw and
        // should not change array length.
        assertTrue(i == 1);
        assertEquals(1, ar.length);
        assertInstanceof(e, TypeError);
        threw_exception = true;
      }
    }
    assertTrue(threw_exception);
    return ar;
  }

  assertEquals([42,153,42], foo());
  assertEquals([42,153,42], foo());
  assertEquals([42,153,42], foo());
  %PrepareFunctionForOptimization(foo);
  assertEquals([42,153,42], foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals([42,153,42], foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae6c58c^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=ae6c58c)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=ae6c58c)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=ae6c58c)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=ae6c58c)  
[src/objects/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/objects/elements-kind.h?cl=ae6c58c)  
...  
  

---   

## **regress-10508.js (v8 issue)**  
   
**[Issue: d8 crash in src/objects/contexts-inl.h, line 170](https://crbug.com/v8/10508)**  
**[Commit: [runtime] Return undefined as CallSite::getFunction for scripts](https://chromium.googlesource.com/v8/v8/+/7e05ebe)**  
  
Date(Commit): Mon May 11 13:06:11 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2193716](https://chromium-review.googlesource.com/c/v8/v8/+/2193716)  
Regress: [mjsunit/regress/regress-10508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-10508.js)  
```javascript
Error.prepareStackTrace = (error, frames) => {
  // JSON.stringify executes the replacer, triggering the relevant
  // code in Invoke().
  JSON.stringify({}, frames[0].getFunction());
};
let v0;
try {
  throw new Error();
} catch (e) {
  e.stack
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e05ebe^!)  
[src/builtins/builtins-callsite.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-callsite.cc?cl=7e05ebe)  
[test/mjsunit/regress/regress-10508.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-10508.js?cl=7e05ebe)  
  

---   

## **regress-1079449.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1079449)**  
**[Commit: [wasm][liftoff][arm] Fix register allocation in I64AtomicCompareExchange](https://chromium.googlesource.com/v8/v8/+/a76f2cb)**  
  
Date(Commit): Mon May 11 10:16:46 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2192652](https://chromium-review.googlesource.com/c/v8/v8/+/2192652)  
Regress: [mjsunit/regress/wasm/regress-1079449.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1079449.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false, true);
const sig = builder.addType(makeSig(
    [
      kWasmI64, kWasmI32, kWasmI64, kWasmI32, kWasmI32, kWasmI32, kWasmI32,
      kWasmI32, kWasmI32, kWasmI64, kWasmI64, kWasmI64
    ],
    [kWasmI64]));
builder.addFunction(undefined, sig)
    .addLocals({f32_count: 10})
    .addLocals({i32_count: 4})
    .addLocals({f64_count: 1})
    .addLocals({i32_count: 15})
    .addBodyWithEnd([
      // signature: v_liliiiiiilll
      // body:
      kExprI32Const, 0x00,  // i32.const
      kExprI64Const, 0x00,  // i64.const
      kExprI64Const, 0x00,  // i64.const
      kAtomicPrefix, kExprI64AtomicCompareExchange, 0x00,
      0x8,      // i64.atomic.cmpxchng64
      kExprEnd,  // end @124
    ]);

builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(
    0n, instance.exports.main(1n, 2, 3n, 4, 5, 6, 7, 8, 9, 10n, 11n, 12n, 13n));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a76f2cb^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=a76f2cb)  
[test/mjsunit/regress/wasm/regress-1079449.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1079449.js?cl=a76f2cb)  
  

---   

## **regress-crbug-1074737.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1074737)**  
**[Commit: [parser] Treat var initializers in masking catch as assigning](https://chromium.googlesource.com/v8/v8/+/f5818c6)**  
  
Date(Commit): Fri May 08 14:25:50 2020  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Wrong-CLs, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2187272](https://chromium-review.googlesource.com/c/v8/v8/+/2187272)  
Regress: [mjsunit/regress/regress-crbug-1074737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1074737.js)  
```javascript
try {
  throw 42
} catch (e) {
  function foo() { return e };
  %PrepareFunctionForOptimization(foo);
  %OptimizeFunctionOnNextCall(foo);
  foo();
  var e = "expected";
}
assertEquals("expected", foo());

try {
  throw 42
} catch (f) {
  function foo2() { return f };
  %PrepareFunctionForOptimization(foo2);
  %OptimizeFunctionOnNextCall(foo2);
  foo2();
  with ({}) {
    var f = "expected";
  }
}
assertEquals("expected", foo2());

(function () {
  function foo3() { return g };
  %PrepareFunctionForOptimization(foo3);
  %OptimizeFunctionOnNextCall(foo3);
  foo3();
  with ({}) {
    var g = "expected";
  }
  assertEquals("expected", foo3());
})()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f5818c6^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=f5818c6)  
[test/mjsunit/regress/regress-crbug-1074737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1074737.js?cl=f5818c6)  
  

---   

## **regress-1078913.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1078913)**  
**[Commit: [compiler] Skip interpreter trampoline copy for asm.js](https://chromium.googlesource.com/v8/v8/+/7bd4c13)**  
  
Date(Commit): Fri May 08 11:44:50 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2190412](https://chromium-review.googlesource.com/c/v8/v8/+/2190412)  
Regress: [mjsunit/regress/regress-1078913.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1078913.js)  
```javascript
function func() {
  return;
}

function asm_func() {
  "use asm";
  function f(){}
  return {f:f};
}

function failed_asm_func() {
  "use asm";
  // This should fail validation
  [x,y,z] = [1,2,3];
  return;
}

func();
asm_func();
failed_asm_func();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bd4c13^!)  
[src/codegen/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/compiler.cc?cl=7bd4c13)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=7bd4c13)  
[test/mjsunit/regress/regress-1078913.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1078913.js?cl=7bd4c13)  
  

---   

## **regress-crbug-1078825.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Torque assert 'Is<A>(o)' failed](https://crbug.com/1078825)**  
**[Commit: [Promise.any] Fix crash if "then" is not callble](https://chromium.googlesource.com/v8/v8/+/ef2f167)**  
  
Date(Commit): Thu May 07 15:09:08 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2187495](https://chromium-review.googlesource.com/c/v8/v8/+/2187495)  
Regress: [mjsunit/regress-crbug-1078825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-crbug-1078825.js)  
```javascript
load('test/mjsunit/test-async.js');

(function() {
  const p1 = Promise.reject(1);
  const p2 = Promise.resolve(1);
  Object.defineProperty(p2, "then", {});

  testAsync(assert => {
    assert.plan(1);
    Promise.any([p1, p2]).then(
      assert.unreachable,
      (e) => { assert.equals(true, e instanceof TypeError); });
    });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef2f167^!)  
[src/builtins/promise-any.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/promise-any.tq?cl=ef2f167)  
[test/mjsunit/regress-crbug-1078825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-1078825.js?cl=ef2f167)  
  

---   

## **regress-5660.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/5660)**  
**[Commit: [turbofan] Improve equality on NumberOrOddball](https://chromium.googlesource.com/v8/v8/+/6204768)**  
  
Date(Commit): Thu May 07 11:58:09 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2162854](https://chromium-review.googlesource.com/c/v8/v8/+/2162854)  
Regress: [mjsunit/regress/regress-5660.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5660.js)  
```javascript
(function() {
  function test(a, b) {
    return a === b;
  }

  %PrepareFunctionForOptimization(test);
  assertTrue(test(undefined, undefined));
  assertTrue(test(undefined, undefined));
  %OptimizeFunctionOnNextCall(test);
  assertTrue(test(undefined, undefined));
})();

(function() {
  function test(a, b) {
    return a === b;
  }

  %PrepareFunctionForOptimization(test);
  assertTrue(test(true, true));
  assertTrue(test(true, true));
  %OptimizeFunctionOnNextCall(test);
  assertFalse(test(true, 1));
})();

(function() {
  function test(a, b) {
      return a == b;
  }

  %PrepareFunctionForOptimization(test);
  assertTrue(test(true, true));
  assertTrue(test(true, true));
  %OptimizeFunctionOnNextCall(test);
  assertTrue(test(true, 1));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6204768^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=6204768)  
[src/common/globals.h](https://cs.chromium.org/chromium/src/v8/src/common/globals.h?cl=6204768)  
[src/compiler/code-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-assembler.cc?cl=6204768)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=6204768)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=6204768)  
...  
  

---   

## **regress-crbug-1077508.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/1077508)**  
**[Commit: Move to slow-path in Array#sort if the array is no longer a FastJSArray](https://chromium.googlesource.com/v8/v8/+/a40e093)**  
  
Date(Commit): Thu May 07 08:08:39 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2187171](https://chromium-review.googlesource.com/c/v8/v8/+/2187171)  
Regress: [mjsunit/regress/regress-crbug-1077508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1077508.js)  
```javascript
const array = [, , , 0, 1, 2];
const comparefn = () => {
  Array.prototype.__defineSetter__("0", function () {});
  Array.prototype.__defineSetter__("1", function () {});
  Array.prototype.__defineSetter__("2", function () {});
};

array.sort(comparefn);

assertArrayEquals([, , , , , , ], array);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a40e093^!)  
[test/mjsunit/regress/regress-crbug-1077508.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1077508.js?cl=a40e093)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=a40e093)  
  

---   

## **regress-1076569.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1076569)**  
**[Commit: [Tests] Add mjsunit test for issue 1076569.](https://chromium.googlesource.com/v8/v8/+/f19c759)**  
  
Date(Commit): Wed May 06 18:34:28 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2185130](https://chromium-review.googlesource.com/c/v8/v8/+/2185130)  
Regress: [mjsunit/regress/regress-1076569.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1076569.js)  
```javascript
var array = new Int16Array();

function foo() {
  array[0] = "123.12";
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f19c759^!)  
[test/mjsunit/regress/regress-1076569.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1076569.js?cl=f19c759)  
  

---   

## **regress-v8-10484-1.js (v8 issue)**  
   
**[Issue: Unreachable due to Object.freeze and Array Pop](https://crbug.com/v8/10484)**  
**[Commit: [builtins] Fix handling of read-only length in Array.prototype.pop](https://chromium.googlesource.com/v8/v8/+/d914a9a)**  
  
Date(Commit): Wed May 06 14:14:47 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2182452](https://chromium-review.googlesource.com/c/v8/v8/+/2182452)  
Regress: [mjsunit/regress/regress-v8-10484-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10484-1.js), [mjsunit/regress/regress-v8-10484-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10484-2.js)  
```javascript
var ar;
Object.defineProperty(Array.prototype, 3,
    { get() { Object.freeze(ar); } });

function foo() {
  ar = [1, 2, 3];
  ar.length = 4;
  ar.pop();
}

assertThrows(foo, TypeError);
assertThrows(foo, TypeError);
assertThrows(foo, TypeError);
assertThrows(foo, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d914a9a^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=d914a9a)  
[test/mjsunit/regress/regress-v8-10484-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10484-1.js?cl=d914a9a)  
[test/mjsunit/regress/regress-v8-10484-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10484-2.js?cl=d914a9a)  
  

---   

## **regress-1077804.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1077804)**  
**[Commit: [turbofan] Fixes undefined in BigInt operations](https://chromium.googlesource.com/v8/v8/+/adc2b64)**  
  
Date(Commit): Wed May 06 14:07:07 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2182455](https://chromium-review.googlesource.com/c/v8/v8/+/2182455)  
Regress: [mjsunit/regress/regress-1077804.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1077804.js)  
```javascript
function foo() {
  return bar();
}

function bar(a, b) {
  return a + b;
}

%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
%PrepareFunctionForOptimization(bar);
%OptimizeFunctionOnNextCall(bar);
bar(2n, 2n);
assertTrue(Number.isNaN(foo()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/adc2b64^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=adc2b64)  
[test/mjsunit/regress/regress-1077804.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1077804.js?cl=adc2b64)  
  

---   

## **regress-crbug-1063796.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1063796)**  
**[Commit: [ic] Fix KeyedHasIC_SloppyArguments implementation](https://chromium.googlesource.com/v8/v8/+/0d44905)**  
  
Date(Commit): Mon May 04 14:22:51 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2174504](https://chromium-review.googlesource.com/c/v8/v8/+/2174504)  
Regress: [mjsunit/regress/regress-crbug-1063796.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1063796.js)  
```javascript
Object.prototype[1] = 1;
function foo(baz) {
  return 1 in arguments;
}
assertTrue(foo(0));
%PrepareFunctionForOptimization(foo);
assertTrue(foo(0));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0d44905^!)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=0d44905)  
[test/mjsunit/regress/regress-crbug-1063796.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1063796.js?cl=0d44905)  
  

---   

## **regress-crbug-1072947.js (chromium issue)**  
   
**[Issue: Security: V8: Bugs in FastInitializeDerivedMap](https://crbug.com/1072947)**  
**[Commit: [runtime] Fix miscalculated number of properties for derived class](https://chromium.googlesource.com/v8/v8/+/a4cf332)**  
  
Date(Commit): Thu Apr 30 15:22:27 2020  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, M-81, Merge-Request-81, Merge-Review-83  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2172964](https://chromium-review.googlesource.com/c/v8/v8/+/2172964)  
Regress: [mjsunit/regress/regress-crbug-1072947.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1072947.js)  
```javascript
(function() {
  class reg extends RegExp {}

  let r;
  function trigger() {
    try {
      trigger();
    } catch {
      Reflect.construct(RegExp,[],reg);
    }
  }
  trigger();
})();

(function() {
  class reg extends Function {}

  let r;
  function trigger() {
    try {
      trigger();
    } catch {
      Reflect.construct(RegExp,[],reg);
    }
  }
  trigger();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4cf332^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=a4cf332)  
[test/mjsunit/regress/regress-crbug-1072947.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1072947.js?cl=a4cf332)  
  

---   

## **regress-1075953.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1075953)**  
**[Commit: [wasm][liftoff][arm] Guarantee scratch register for spilling](https://chromium.googlesource.com/v8/v8/+/0e1ac4e)**  
  
Date(Commit): Thu Apr 30 11:05:25 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2172424](https://chromium-review.googlesource.com/c/v8/v8/+/2172424)  
Regress: [mjsunit/regress/wasm/regress-1075953.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1075953.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(1, 1, false, true);
const sig = builder.addType(makeSig([], [kWasmI32]));

builder.addFunction(undefined, sig)
  .addLocals({i32_count: 1002}).addLocals({i64_count: 3})
  .addBodyWithEnd([
  kExprLocalGet, 0xec, 0x07,  // local.get
  kExprLocalGet, 0xea, 0x07,  // local.set
  kExprLocalGet, 0x17,  // local.set
  kExprLocalGet, 0xb5, 0x01,  // local.set
  kExprI32Const, 0x00,  // i32.const
  kExprIf, kWasmI32,  // if @39 i32
    kExprI32Const, 0x91, 0xe8, 0x7e,  // i32.const
  kExprElse,  // else @45
    kExprI32Const, 0x00,  // i32.const
    kExprEnd,  // end @48
  kExprIf, kWasmStmt,  // if @49
    kExprI32Const, 0x00,  // i32.const
    kExprI32Const, 0x00,  // i32.const
    kAtomicPrefix, kExprI32AtomicSub, 0x01, 0x04,  // i32.atomic.sub
    kExprDrop,
  kExprEnd,
  kExprUnreachable,
kExprEnd
]);

const instance = builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0e1ac4e^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=0e1ac4e)  
[test/mjsunit/regress/wasm/regress-1075953.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1075953.js?cl=0e1ac4e)  
  

---   

## **regress-1071190.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1071190)**  
**[Commit: [turbofan] Fixes incorrect DataView setters](https://chromium.googlesource.com/v8/v8/+/84cff42)**  
  
Date(Commit): Tue Apr 28 15:47:55 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2170091](https://chromium-review.googlesource.com/c/v8/v8/+/2170091)  
Regress: [mjsunit/regress/regress-1071190.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1071190.js)  
```javascript
function test() {
  const a = new DataView(new ArrayBuffer(32));
  const b = new DataView(new ArrayBuffer(32));
  a.setFloat64(0);
  b.setFloat64(0, undefined);

  for(let i = 0; i < 8; ++i) {
    assertEquals(a.getUint8(i), b.getUint8(i));
  }
}

%PrepareFunctionForOptimization(test);
test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84cff42^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=84cff42)  
[test/mjsunit/regress/regress-1071190.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1071190.js?cl=84cff42)  
  

---   

## **regress-1074586-b.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1074586)**  
**[Commit: [wasm][liftoff][arm] Avoid double allocation of register is AtomicOp64](https://chromium.googlesource.com/v8/v8/+/980037c)**  
  
Date(Commit): Tue Apr 28 15:08:42 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2170088](https://chromium-review.googlesource.com/c/v8/v8/+/2170088)  
Regress: [mjsunit/regress/wasm/regress-1074586-b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1074586-b.js), [mjsunit/regress/wasm/regress-1074586.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1074586.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false, true);
const sig = builder.addType(makeSig(
    [kWasmI32, kWasmI32, kWasmI32, kWasmI32, kWasmI32, kWasmI32, kWasmI32],
    []));
builder.addFunction(undefined, sig).addBodyWithEnd([
  // signature: v_iiiiifidi
  // body:
  kExprI32Const, 0x00,                             // i32.const
  kExprI64Const, 0x00,                             // i64.const
  kAtomicPrefix, kExprI64AtomicStore, 0x00, 0x00,  // i64.atomic.store64
  kExprEnd,                                        // end @9
]);
builder.addExport('main', 0);
assertDoesNotThrow(() => builder.instantiate());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/980037c^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=980037c)  
[test/mjsunit/regress/wasm/regress-1074586-b.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1074586-b.js?cl=980037c)  
  

---   

## **regress-1074736.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1074736)**  
**[Commit: [turbofan] Fix bug in typed array iteration](https://chromium.googlesource.com/v8/v8/+/0188a33)**  
  
Date(Commit): Tue Apr 28 13:36:26 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2168874](https://chromium-review.googlesource.com/c/v8/v8/+/2168874)  
Regress: [mjsunit/compiler/regress-1074736.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1074736.js)  
```javascript
var arr = new Uint8Array();
%ArrayBufferDetach(arr.buffer);

function foo() {
  return arr[Symbol.iterator]();
}

%PrepareFunctionForOptimization(foo);
assertThrows(foo, TypeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0188a33^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=0188a33)  
[test/mjsunit/compiler/regress-1074736.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1074736.js?cl=0188a33)  
  

---   

## **regress-1069964.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1069964)**  
**[Commit: [protectors] Move regexp species protector back to the isolate](https://chromium.googlesource.com/v8/v8/+/af45cf6)**  
  
Date(Commit): Tue Apr 28 06:40:42 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2157382](https://chromium-review.googlesource.com/c/v8/v8/+/2157382)  
Regress: [mjsunit/regress/regress-1069964.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1069964.js)  
```javascript
Realm.createAllowCrossRealmAccess();
const c = Realm.global(1);
Realm.detachGlobal(1);
try { c.constructor = () => {}; } catch {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/af45cf6^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=af45cf6)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=af45cf6)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=af45cf6)  
[src/execution/protectors-inl.h](https://cs.chromium.org/chromium/src/v8/src/execution/protectors-inl.h?cl=af45cf6)  
[src/execution/protectors.cc](https://cs.chromium.org/chromium/src/v8/src/execution/protectors.cc?cl=af45cf6)  
...  
  

---   

## **regress-1071743.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1071743)**  
**[Commit: [turbofan] Distinguish two further modes of CheckBounds](https://chromium.googlesource.com/v8/v8/+/53c1525)**  
  
Date(Commit): Mon Apr 27 19:45:35 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2157365](https://chromium-review.googlesource.com/c/v8/v8/+/2157365)  
Regress: [mjsunit/compiler/regress-1071743.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1071743.js)  
```javascript
function foo(v) {
  let x = Math.floor(v);
  Number.prototype[v] = 42;
  return x + Math.floor(v);
}

%PrepareFunctionForOptimization(foo);
assertSame(foo(-0), -0);
assertSame(foo(-0), -0);
%OptimizeFunctionOnNextCall(foo);
assertSame(foo(-0), -0);


function bar(v) {
  v = v ? (v|0) : -0;  // v has now type Integral32OrMinusZero.
  let x = Math.floor(v);
  Number.prototype[v] = 42;
  return x + Math.floor(v);
}

%PrepareFunctionForOptimization(bar);
assertSame(2, bar(1));
assertSame(2, bar(1));
%OptimizeFunctionOnNextCall(bar);
assertSame(-0, bar(-0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53c1525^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=53c1525)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=53c1525)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=53c1525)  
[src/compiler/redundancy-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/redundancy-elimination.cc?cl=53c1525)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=53c1525)  
...  
  

---   

## **regress-1070892.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1070892)**  
**[Commit: [turbofan] Distinguish two further modes of CheckBounds](https://chromium.googlesource.com/v8/v8/+/53c1525)**  
  
Date(Commit): Mon Apr 27 19:45:35 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2157365](https://chromium-review.googlesource.com/c/v8/v8/+/2157365)  
Regress: [mjsunit/compiler/regress-1070892.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1070892.js)  
```javascript
var v = {0: 0, 1: 1, '01': 7};
function foo(index) {
    return [v[index], v[index + 1], index + 1];
};

%PrepareFunctionForOptimization(foo);
assertEquals(foo(0), [0, 1, 1]);
assertEquals(foo(0), [0, 1, 1]);
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(0), [0, 1, 1]);
assertEquals(foo('0'), [0, 7, '01']);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/53c1525^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=53c1525)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=53c1525)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=53c1525)  
[src/compiler/redundancy-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/redundancy-elimination.cc?cl=53c1525)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=53c1525)  
...  
  

---   

## **regress-1070078.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: CHECK failure: iterator != current_assessments->map().end() in register-allocator-verifier.cc](https://crbug.com/1070078)**  
**[Commit: [arm] Change fp_fixed registers to be allocatable registers](https://chromium.googlesource.com/v8/v8/+/390ed4b)**  
  
Date(Commit): Fri Apr 24 17:00:36 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2161565](https://chromium-review.googlesource.com/c/v8/v8/+/2161565)  
Regress: [mjsunit/regress/wasm/regress-1070078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1070078.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false);
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */).addBodyWithEnd([
  // signature: i_iii
  // body:
  kExprI32Const, 0x00,  // i32.const
  kExprMemoryGrow, 0x00,  // memory.grow
  kExprI32Const, 0xd3, 0xe7, 0x03,  // i32.const
  kSimdPrefix, kExprI8x16Splat,  // i8x16.splat
  kExprI32Const, 0x84, 0x80, 0xc0, 0x05,  // i32.const
  kSimdPrefix, kExprI8x16Splat,  // i8x16.splat
  kExprI32Const, 0x84, 0x81, 0x80, 0xc8, 0x01,  // i32.const
  kSimdPrefix, kExprI8x16Splat,  // i8x16.splat
  kExprI32Const, 0x19,  // i32.const
  kSimdPrefix, kExprI8x16Splat,  // i8x16.splat
  kSimdPrefix, kExprS8x16Shuffle,
  0x00, 0x00, 0x17, 0x00, 0x04, 0x04, 0x04, 0x04,
  0x04, 0x10, 0x01, 0x00, 0x04, 0x04, 0x04, 0x04,  // s8x16.shuffle
  kSimdPrefix, kExprS8x16Shuffle,
  0x04, 0x00, 0x10, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // s8x16.shuffle
  kSimdPrefix, kExprI8x16LeU,  // i8x16.le_u
  kSimdPrefix, kExprV8x16AnyTrue,  // v8x16.any_true
  kExprMemoryGrow, 0x00,  // memory.grow
  kExprDrop,
  kExprEnd,  // end @233
]);
builder.addExport('main', 0);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/390ed4b^!)  
[src/codegen/arm64/register-arm64.h](https://cs.chromium.org/chromium/src/v8/src/codegen/arm64/register-arm64.h?cl=390ed4b)  
[test/mjsunit/regress/wasm/regress-1070078.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1070078.js?cl=390ed4b)  
[test/mjsunit/wasm/wasm-module-builder.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-module-builder.js?cl=390ed4b)  
  

---   

## **regress-1073553.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1073553)**  
**[Commit: Validate reading prefixed opcodes](https://chromium.googlesource.com/v8/v8/+/4681371)**  
  
Date(Commit): Fri Apr 24 16:56:11 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2161404](https://chromium-review.googlesource.com/c/v8/v8/+/2161404)  
Regress: [mjsunit/regress/wasm/regress-1073553.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1073553.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(1);
builder.addFunction(undefined, kSig_v_i) .addBodyWithEnd([
    kExprI32Const, 1, kExprMemoryGrow, kMemoryZero, kNumericPrefix]);

const b = builder.toBuffer();
WebAssembly.compile(b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4681371^!)  
[src/wasm/decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/decoder.h?cl=4681371)  
[test/mjsunit/regress/wasm/regress-1073553.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1073553.js?cl=4681371)  
  

---   

## **regress-1072171.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1072171)**  
**[Commit: [turbofan] Fix bug in Number.Min/Max typings](https://chromium.googlesource.com/v8/v8/+/4158af8)**  
  
Date(Commit): Mon Apr 20 17:05:50 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2157028](https://chromium-review.googlesource.com/c/v8/v8/+/2157028)  
Regress: [mjsunit/compiler/regress-1072171.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1072171.js)  
```javascript
function testMax1(b) {
  const max = Math.max(-1, b ? -0 : 1);
  return Object.is(max, -0);
}
%PrepareFunctionForOptimization(testMax1);
assertTrue(testMax1(true));
assertTrue(testMax1(true));
%OptimizeFunctionOnNextCall(testMax1);
assertTrue(testMax1(true));

function testMax2(b) {
  const max = Math.max(b ? -0 : 1, -1);
  return Object.is(max, -0);
}
%PrepareFunctionForOptimization(testMax2);
assertTrue(testMax2(true));
assertTrue(testMax2(true));
%OptimizeFunctionOnNextCall(testMax2);
assertTrue(testMax2(true));

function testMin1(b) {
  const min = Math.min(1, b ? -0 : -1);
  return Object.is(min, -0);
}
%PrepareFunctionForOptimization(testMin1);
assertTrue(testMin1(true));
assertTrue(testMin1(true));
%OptimizeFunctionOnNextCall(testMin1);
assertTrue(testMin1(true));

function testMin2(b) {
  const min = Math.min(b ? -0 : -1, 1);
  return Object.is(min, -0);
}
%PrepareFunctionForOptimization(testMin2);
assertTrue(testMin2(true));
assertTrue(testMin2(true));
%OptimizeFunctionOnNextCall(testMin2);
assertTrue(testMin2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4158af8^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=4158af8)  
[test/mjsunit/compiler/regress-1072171.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1072171.js?cl=4158af8)  
[test/unittests/compiler/typer-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/typer-unittest.cc?cl=4158af8)  
  

---   

## **regress-crbug-1070560.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1070560)**  
**[Commit: [builtins] When creating new elements array initialize with holes](https://chromium.googlesource.com/v8/v8/+/a46d8d1)**  
  
Date(Commit): Thu Apr 16 15:59:37 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2150599](https://chromium-review.googlesource.com/c/v8/v8/+/2150599)  
Regress: [mjsunit/regress/regress-crbug-1070560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1070560.js)  
```javascript
function f() {
 // Create a FixedDoubleArray
 var arr = [5.65];
 // Force the elements to be EmptyFixedArray
 arr.splice(0);
 // This should create a FixedDoubleArray initialized with holes.
 arr.splice(-4, 9, 10, 20);
 // If the earlier spice didn't create a holes this would fail.
 assertFalse(2 in arr);
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a46d8d1^!)  
[src/builtins/array-splice.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-splice.tq?cl=a46d8d1)  
[test/mjsunit/regress/regress-crbug-1070560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1070560.js?cl=a46d8d1)  
  

---   

## **regress-crbug-1067757.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1067757)**  
**[Commit: [ic] Use slow stub when storing non-existent properties to global object](https://chromium.googlesource.com/v8/v8/+/d11292f)**  
  
Date(Commit): Wed Apr 15 15:00:29 2020  
Components: Blink>JavaScript>Runtime  
Labels: Stability-Crash, Reproducible, Security_Impact-Stable, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2142255](https://chromium-review.googlesource.com/c/v8/v8/+/2142255)  
Regress: [mjsunit/regress/regress-crbug-1067757.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1067757.js)  
```javascript
"use strict";

function foo() {
  let count = 0;
  try {
    for (p of v) {
      count += 1;
    }
  } catch (e) { }
  assertEquals(count, 0);
}

var v = [ "0", {}];

foo();
Reflect.deleteProperty(v, '0');

let count_loop = 0;
try {
 for (p of v) { count_loop += 1; }
} catch (e) {}
assertEquals(count_loop, 0);

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d11292f^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=d11292f)  
[test/mjsunit/regress/regress-crbug-1067757.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1067757.js?cl=d11292f)  
  

---   

## **regress-1067621.js (chromium issue)**  
   
**[Issue: Timing issues with Wasm threads (SAB and/or Atomics)](https://crbug.com/1067621)**  
**[Commit: [wasm] Fix return value of concurrent memory.grow](https://chromium.googlesource.com/v8/v8/+/401190b)**  
  
Date(Commit): Tue Apr 14 21:37:32 2020  
Components: Platform>DevTools, Blink>JavaScript>WebAssembly  
Labels: allpublic  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2144113](https://chromium-review.googlesource.com/c/v8/v8/+/2144113)  
Regress: [mjsunit/regress/wasm/regress-1067621.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1067621.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const kNumberOfWorker = 4;

const workerOnMessage = function(msg) {
  if (msg.module) {
    let module = msg.module;
    let mem = msg.mem;
    this.instance = new WebAssembly.Instance(module, {m: {memory: mem}});
    postMessage({instantiated: true});
  } else {
    const kNumberOfRuns = 20;
    let result = new Array(kNumberOfRuns);
    for (let i = 0; i < kNumberOfRuns; ++i) {
      result[i] = instance.exports.grow();
    }
    postMessage({result: result});
  }
};

function spawnWorkers() {
  let workers = [];
  for (let i = 0; i < kNumberOfWorker; i++) {
    let worker = new Worker(
        'onmessage = ' + workerOnMessage.toString(), {type: 'string'});
    workers.push(worker);
  }
  return workers;
}

function instantiateModuleInWorkers(workers, module, shared_memory) {
  for (let worker of workers) {
    worker.postMessage({module: module, mem: shared_memory});
    let msg = worker.getMessage();
    if (!msg.instantiated) throw 'Worker failed to instantiate';
  }
}

function triggerWorkers(workers) {
  for (i = 0; i < workers.length; i++) {
    let worker = workers[i];
    worker.postMessage({});
  }
}

(function TestConcurrentGrowMemoryResult() {
  let builder = new WasmModuleBuilder();
  builder.addImportedMemory('m', 'memory', 1, 500, 'shared');
  builder.addFunction('grow', kSig_i_v)
      .addBody([kExprI32Const, 1, kExprMemoryGrow, kMemoryZero])
      .exportFunc();

  const module = builder.toModule();
  const shared_memory =
      new WebAssembly.Memory({initial: 1, maximum: 500, shared: true});

  // Spawn off the workers and run the sequences.
  let workers = spawnWorkers();
  instantiateModuleInWorkers(workers, module, shared_memory);
  triggerWorkers(workers);
  let all_results = [];
  for (let worker of workers) {
    let msg = worker.getMessage();
    all_results = all_results.concat(msg.result);
  }

  all_results.sort((a, b) => a - b);
  for (let i = 1; i < all_results.length; ++i) {
    assertEquals(all_results[i - 1] + 1, all_results[i]);
  }

  // Terminate all workers.
  for (let worker of workers) {
    worker.terminate();
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/401190b^!)  
[src/objects/backing-store.cc](https://cs.chromium.org/chromium/src/v8/src/objects/backing-store.cc?cl=401190b)  
[src/objects/backing-store.h](https://cs.chromium.org/chromium/src/v8/src/objects/backing-store.h?cl=401190b)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=401190b)  
[test/mjsunit/regress/wasm/regress-1067621.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1067621.js?cl=401190b)  
[test/unittests/objects/backing-store-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/objects/backing-store-unittest.cc?cl=401190b)  
  

---   

## **regress-1068494.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1068494)**  
**[Commit: [turbofan] Fix bug in reduction of StoreDataPropertyInLiteral](https://chromium.googlesource.com/v8/v8/+/fbdb473)**  
  
Date(Commit): Wed Apr 08 11:50:28 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2139581](https://chromium-review.googlesource.com/c/v8/v8/+/2139581)  
Regress: [mjsunit/compiler/regress-1068494.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1068494.js)  
```javascript
function foo() {
  return { ['bar']: class {} };
}
%PrepareFunctionForOptimization(foo);
assertEquals('bar', foo().bar.name);
assertEquals('bar', foo().bar.name);
%OptimizeFunctionOnNextCall(foo);
assertEquals('bar', foo().bar.name);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fbdb473^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=fbdb473)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=fbdb473)  
[test/mjsunit/compiler/regress-1068494.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1068494.js?cl=fbdb473)  
  

---   

## **regress-1067270.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1067270)**  
**[Commit: [regexp] Reserve space for all registers in interpreter](https://chromium.googlesource.com/v8/v8/+/30658b6)**  
  
Date(Commit): Mon Apr 06 14:34:34 2020  
Components: None  
Labels: None  
Code Review: [https://crrev.com/c/2135642](https://crrev.com/c/2135642)  
Regress: [mjsunit/regress/regress-1067270.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1067270.js)  
```javascript
const needle = Array(1802).join(" +") + Array(16884).join("A");
const string = "A";

assertEquals(string.search(needle), -1);
assertEquals(string.search(needle), -1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/30658b6^!)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=30658b6)  
[test/mjsunit/regress/regress-1067270.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1067270.js?cl=30658b6)  
  

---   

## **regress-1067544.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1067544)**  
**[Commit: [turbofan] Fix bug in reduction of typed array iteration](https://chromium.googlesource.com/v8/v8/+/7cd01ed)**  
  
Date(Commit): Mon Apr 06 13:18:10 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2135632](https://chromium-review.googlesource.com/c/v8/v8/+/2135632)  
Regress: [mjsunit/compiler/regress-1067544.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1067544.js)  
```javascript
const v = [];
function foo() {
  Int8Array.prototype.values.call([v]);
}

%PrepareFunctionForOptimization(foo);
assertThrows(foo, TypeError);
assertThrows(foo, TypeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7cd01ed^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=7cd01ed)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=7cd01ed)  
[test/mjsunit/compiler/regress-1067544.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1067544.js?cl=7cd01ed)  
  

---   

## **regress-1065599.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: Divide-by-zero in Builtins_JSEntry](https://crbug.com/1065599)**  
**[Commit: [wasm-simd][x64][ia32] Do not overwrite input register](https://chromium.googlesource.com/v8/v8/+/7d955fa)**  
  
Date(Commit): Fri Apr 03 17:57:31 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2125941](https://chromium-review.googlesource.com/c/v8/v8/+/2125941)  
Regress: [mjsunit/regress/wasm/regress-1065599.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1065599.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false);
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */).addBodyWithEnd([
  // signature: i_iii
  // body:
  kExprI32Const, 0xba, 0x01,       // i32.const
  kSimdPrefix, kExprI16x8Splat,    // i16x8.splat
  kExprMemorySize, 0x00,           // memory.size
  kSimdPrefix, kExprI16x8ShrS,     // i16x8.shr_s
  kSimdPrefix, kExprV8x16AnyTrue,  // v8x16.any_true
  kExprMemorySize, 0x00,           // memory.size
  kExprI32RemS,                    // i32.rem_s
  kExprEnd,                        // end @15
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
instance.exports.main(1, 2, 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d955fa^!)  
[src/compiler/backend/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/ia32/code-generator-ia32.cc?cl=7d955fa)  
[src/compiler/backend/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/ia32/instruction-selector-ia32.cc?cl=7d955fa)  
[src/compiler/backend/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/x64/code-generator-x64.cc?cl=7d955fa)  
[src/compiler/backend/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/x64/instruction-selector-x64.cc?cl=7d955fa)  
[test/mjsunit/regress/wasm/regress-1055692.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1055692.js?cl=7d955fa)  
...  
  

---   

## **regress-crbug-1060023.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1060023)**  
**[Commit: [parser] Already break the expression scope chain for function parameters](https://chromium.googlesource.com/v8/v8/+/4561500)**  
  
Date(Commit): Thu Apr 02 13:16:55 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2134006](https://chromium-review.googlesource.com/c/v8/v8/+/2134006)  
Regress: [mjsunit/regress/regress-crbug-1060023.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1060023.js)  
```javascript
class b extends RegExp {
  exec() {
    (function() { (a = (function({} = this) {})) => {} })
  }
}
assertThrows(()=>'a'.match(new b), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4561500^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=4561500)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=4561500)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=4561500)  
[test/mjsunit/regress/regress-crbug-1060023.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1060023.js?cl=4561500)  
  

---   

## **regress-crbug-1053939-1.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1053939)**  
**[Commit: [ic] Use the existing prototype validity cell when recomputing handlers](https://chromium.googlesource.com/v8/v8/+/800c294)**  
  
Date(Commit): Thu Apr 02 12:36:45 2020  
Components: Blink>JavaScript>Runtime  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Security_Impact-Stable, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Merge-Request-80, Merge-Review-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2122032](https://chromium-review.googlesource.com/c/v8/v8/+/2122032)  
Regress: [mjsunit/regress/regress-crbug-1053939-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1053939-1.js), [mjsunit/regress/regress-crbug-1053939.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1053939.js)  
```javascript
v = {};
v.__proto__ = new Int32Array(1);
function foo() {
  for (var i = 0; i < 2; i++) {
    v[i] = 0;
  }
}
foo();
assertEquals(Object.keys(v).length, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/800c294^!)  
[src/ic/handler-configuration.cc](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.cc?cl=800c294)  
[src/ic/handler-configuration.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.h?cl=800c294)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=800c294)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=800c294)  
[src/objects/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/objects/feedback-vector.cc?cl=800c294)  
...  
  

---   

## **regress-1065635.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/1065635)**  
**[Commit: [asm] Fix double literals without dots](https://chromium.googlesource.com/v8/v8/+/7bb686a)**  
  
Date(Commit): Wed Apr 01 13:59:24 2020  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2126922](https://chromium-review.googlesource.com/c/v8/v8/+/2126922)  
Regress: [mjsunit/regress/wasm/regress-1065635.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1065635.js)  
```javascript
function foo() {
  'use asm';
  function bar() {
    return -1e-15;
  }
  return {bar: bar};
}

assertEquals(-1e-15, foo().bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bb686a^!)  
[src/asmjs/asm-scanner.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.cc?cl=7bb686a)  
[test/mjsunit/regress/wasm/regress-1065635.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1065635.js?cl=7bb686a)  
  

---   

## **regress-crbug-1065741.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1065741)**  
**[Commit: [turbofan] Add a type check to String.prototype.startsWith](https://chromium.googlesource.com/v8/v8/+/6ee457b)**  
  
Date(Commit): Wed Apr 01 13:57:44 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2129639](https://chromium-review.googlesource.com/c/v8/v8/+/2129639)  
Regress: [mjsunit/regress/regress-crbug-1065741.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1065741.js)  
```javascript
function bar() {
  String.prototype.startsWith.apply();
}

%PrepareFunctionForOptimization(bar);
assertThrows(bar, TypeError);
assertThrows(bar, TypeError);
%OptimizeFunctionOnNextCall(bar);
assertThrows(bar, TypeError);
%PrepareFunctionForOptimization(bar);
%OptimizeFunctionOnNextCall(bar);
assertThrows(bar, TypeError);
assertOptimized(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6ee457b^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=6ee457b)  
[test/mjsunit/regress/regress-crbug-1065741.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1065741.js?cl=6ee457b)  
  

---   

## **regress-1065737.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1065737)**  
**[Commit: [turbofan] Mark JSStoreGlobal as NeedsExactContext](https://chromium.googlesource.com/v8/v8/+/2f0e62e)**  
  
Date(Commit): Wed Apr 01 12:18:14 2020  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2130280](https://chromium-review.googlesource.com/c/v8/v8/+/2130280)  
Regress: [mjsunit/compiler/regress-1065737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1065737.js)  
```javascript
function foo() {
  class c {
    static get [v = 0]() {}
  }
}

%PrepareFunctionForOptimization(foo);
assertThrows(foo, ReferenceError);
assertThrows(foo, ReferenceError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f0e62e^!)  
[src/compiler/operator-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator-properties.cc?cl=2f0e62e)  
[test/mjsunit/compiler/regress-1065737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1065737.js?cl=2f0e62e)  
  

---   

## **regress-1065852.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/1065852)**  
**[Commit: [asm] Avoid instantiation as resumable function](https://chromium.googlesource.com/v8/v8/+/ee498c1)**  
  
Date(Commit): Wed Apr 01 09:50:34 2020  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2126920](https://chromium-review.googlesource.com/c/v8/v8/+/2126920)  
Regress: [mjsunit/regress/wasm/regress-1065852.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1065852.js)  
```javascript
function* asm() {
  "use asm";
  function x(v) {
    v = v | 0;
  }
  return x;
}

asm().next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee498c1^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=ee498c1)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=ee498c1)  
[test/mjsunit/regress/wasm/regress-1065852.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1065852.js?cl=ee498c1)  
[test/mjsunit/wasm/asm-wasm.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm.js?cl=ee498c1)  
  

---   

## **regress-1065094.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1065094)**  
**[Commit: Make CreateDynamicFunction throw if disallowed](https://chromium.googlesource.com/v8/v8/+/2aac556)**  
  
Date(Commit): Mon Mar 30 10:59:49 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2124837](https://chromium-review.googlesource.com/c/v8/v8/+/2124837)  
Regress: [mjsunit/regress-1065094.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-1065094.js)  
```javascript
function f(fnConstructor) {
    return Object.is(new fnConstructor(), undefined);
}

const realmIndex = Realm.createAllowCrossRealmAccess();
const otherFunction = Realm.global(realmIndex).Function;
Realm.detachGlobal(realmIndex);

%PrepareFunctionForOptimization(f);
assertFalse(f(Function));
assertThrows(_ => f(otherFunction));
%OptimizeFunctionOnNextCall(f);
assertThrows(_ => f(otherFunction));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2aac556^!)  
[src/builtins/builtins-function.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function.cc?cl=2aac556)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=2aac556)  
[test/mjsunit/regress-1065094.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-1065094.js?cl=2aac556)  
  

---   

## **regress-1063661.js (chromium issue)**  
   
**[Issue: Security: Fatal error in ../../src/compiler/typer.cc, line 336](https://crbug.com/1063661)**  
**[Commit: [turbofan] Fix NumberMin and NumberMax typings](https://chromium.googlesource.com/v8/v8/+/33306c4)**  
  
Date(Commit): Wed Mar 25 11:19:13 2020  
Components: Blink>JavaScript>Compiler  
Labels: ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2116199](https://chromium-review.googlesource.com/c/v8/v8/+/2116199)  
Regress: [mjsunit/compiler/regress-1063661.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1063661.js)  
```javascript
function main() {
  const v1 = [];
  for (let v11 = 0; v11 < 7; v11++) {
    for (let v16 = 0; v16 != 100; v16++) {}
    for (let v18 = -0.0; v18 < 7; v18 = v18 || 13.37) {
      const v21 = Math.max(-339,v18);
      v1.fill();
      undefined % v21;
    }
  }
}
main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/33306c4^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=33306c4)  
[test/mjsunit/compiler/regress-1063661.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1063661.js?cl=33306c4)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=33306c4)  
[test/unittests/compiler/typer-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/typer-unittest.cc?cl=33306c4)  
  

---   

## **regress-1062916.js (chromium issue)**  
   
**[Issue: DCHECK failure in !found in constant-folding-reducer.cc](https://crbug.com/1062916)**  
**[Commit: [turbofan] Remove bogus DCHECK and add a comment](https://chromium.googlesource.com/v8/v8/+/c25cc4e)**  
  
Date(Commit): Fri Mar 20 08:11:00 2020  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2110027](https://chromium-review.googlesource.com/c/v8/v8/+/2110027)  
Regress: [mjsunit/compiler/regress-1062916.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1062916.js)  
```javascript
function foo(x) {
  var a = [];
  for (var k1 in x) {
    for (var k2 in x) {
      a.k2;
    }
  }
  return a.join();
}

%PrepareFunctionForOptimization(foo);
foo({p: 42});
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c25cc4e^!)  
[src/compiler/constant-folding-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/constant-folding-reducer.cc?cl=c25cc4e)  
[test/mjsunit/compiler/regress-1062916.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1062916.js?cl=c25cc4e)  
  

---   

## **regress-1061803.js (chromium issue)**  
   
**[Issue: Trap in Builtins_CheckNumberInRange](https://crbug.com/1061803)**  
**[Commit: Reland "[turbofan] Clean up ConstantFoldingReducer"](https://chromium.googlesource.com/v8/v8/+/416b0c3)**  
  
Date(Commit): Tue Mar 17 09:49:24 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2098736](https://chromium-review.googlesource.com/c/v8/v8/+/2098736)  
Regress: [mjsunit/compiler/regress-1061803.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1061803.js)  
```javascript
function foo() {
  return arguments[1][0] === arguments[0];
}

%PrepareFunctionForOptimization(foo);
assertFalse(foo(0, 0));
assertFalse(foo(0, 0));
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/416b0c3^!)  
[src/compiler/constant-folding-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/constant-folding-reducer.cc?cl=416b0c3)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=416b0c3)  
[src/compiler/js-graph.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.h?cl=416b0c3)  
[src/compiler/operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.h?cl=416b0c3)  
[test/mjsunit/compiler/regress-1061678.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1061678.js?cl=416b0c3)  
...  
  

---   

## **regress-1061678.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo:x64,ignition_turbo](https://crbug.com/1061678)**  
**[Commit: Reland "[turbofan] Clean up ConstantFoldingReducer"](https://chromium.googlesource.com/v8/v8/+/416b0c3)**  
  
Date(Commit): Tue Mar 17 09:49:24 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2098736](https://chromium-review.googlesource.com/c/v8/v8/+/2098736)  
Regress: [mjsunit/compiler/regress-1061678.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1061678.js)  
```javascript
Array.prototype[10] = 2;

function foo() {
  try {
    [].forEach();
  } catch (e) {
  }
};

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/416b0c3^!)  
[src/compiler/constant-folding-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/constant-folding-reducer.cc?cl=416b0c3)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=416b0c3)  
[src/compiler/js-graph.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.h?cl=416b0c3)  
[src/compiler/operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.h?cl=416b0c3)  
[test/mjsunit/compiler/regress-1061678.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1061678.js?cl=416b0c3)  
...  
  

---   

## **regress-crbug-1059738.js (chromium issue)**  
   
**[Issue: DCHECK failure in !name_->AsIntegerIndex(&index) in lookup-inl.h](https://crbug.com/1059738)**  
**[Commit: Fix one more LookupIterator](https://chromium.googlesource.com/v8/v8/+/ea468d5)**  
  
Date(Commit): Fri Mar 13 13:39:54 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, M-80, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2101005](https://chromium-review.googlesource.com/c/v8/v8/+/2101005)  
Regress: [mjsunit/regress/regress-crbug-1059738.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1059738.js)  
```javascript
var o = {4294967295: ({4294967295: NaN}) + "foo"};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea468d5^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=ea468d5)  
[test/mjsunit/regress/regress-crbug-1059738.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1059738.js?cl=ea468d5)  
  

---   

## **regress-10309.js (v8 issue)**  
   
**[Issue: Experimental SIMD - Issue when using f32.replace_lane and f32.load instructions together.](https://crbug.com/v8/10309)**  
**[Commit: [wasm-simd] Add regression test to validate results on Arm64 HW](https://chromium.googlesource.com/v8/v8/+/37ef629)**  
  
Date(Commit): Fri Mar 13 00:58:01 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2101701](https://chromium-review.googlesource.com/c/v8/v8/+/2101701)  
Regress: [mjsunit/regress/wasm/regress-10309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-10309.js)  
```javascript
let registry = {};

function module(bytes, valid = true) {
  let buffer = new ArrayBuffer(bytes.length);
  let view = new Uint8Array(buffer);
  for (let i = 0; i < bytes.length; ++i) {
    view[i] = bytes.charCodeAt(i);
  }
  let validated;
  try {
    validated = WebAssembly.validate(buffer);
  } catch (e) {
    throw new Error("Wasm validate throws");
  }
  if (validated !== valid) {
    throw new Error("Wasm validate failure" + (valid ? "" : " expected"));
  }
  return new WebAssembly.Module(buffer);
}

function instance(bytes, imports = registry) {
  return new WebAssembly.Instance(module(bytes), imports);
}

function call(instance, name, args) {
  return instance.exports[name](...args);
}

function exports(name, instance) {
  return {[name]: instance.exports};
}

function assert_return(action, expected) {
  let actual = action();
  if (!Object.is(actual, expected)) {
    throw new Error("Wasm return value " + expected + " expected, got " + actual);
  };
}

let f32 = Math.fround;

let $1 = instance("\x00\x61\x73\x6d\x01\x00\x00\x00\x01\x09\x02\x60\x00\x00\x60\x01\x7f\x01\x7d\x03\x04\x03\x00\x00\x01\x05\x03\x01\x00\x01\x07\x1c\x02\x11\x72\x65\x70\x6c\x61\x63\x65\x5f\x6c\x61\x6e\x65\x5f\x74\x65\x73\x74\x00\x01\x04\x72\x65\x61\x64\x00\x02\x08\x01\x00\x0a\x6e\x03\x2a\x00\x41\x10\x43\x00\x00\x80\x3f\x38\x02\x00\x41\x14\x43\x00\x00\x00\x40\x38\x02\x00\x41\x18\x43\x00\x00\x40\x40\x38\x02\x00\x41\x1c\x43\x00\x00\x80\x40\x38\x02\x00\x0b\x39\x01\x01\x7b\x41\x10\x2a\x02\x00\xfd\x13\x21\x00\x20\x00\x41\x10\x2a\x01\x04\xfd\x20\x01\x21\x00\x20\x00\x41\x10\x2a\x01\x08\xfd\x20\x02\x21\x00\x20\x00\x41\x10\x2a\x01\x0c\xfd\x20\x03\x21\x00\x41\x00\x20\x00\xfd\x0b\x02\x00\x0b\x07\x00\x20\x00\x2a\x02\x00\x0b");

call($1, "replace_lane_test", []);

assert_return(() => call($1, "read", [0]), f32(1.0));

assert_return(() => call($1, "read", [4]), f32(2.0));

assert_return(() => call($1, "read", [8]), f32(3.0));

assert_return(() => call($1, "read", [12]), f32(4.0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37ef629^!)  
[test/mjsunit/regress/wasm/regress-10309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-10309.js?cl=37ef629)  
  

---   

## **regress-crbug-1057653.js (chromium issue)**  
   
**[Issue: Security: Segmentation fault   in operator* () at ../../src/handles/handles.h:149](https://crbug.com/1057653)**  
**[Commit: [keys] Handle RangeError in GetKeysWithPrototypeInfoCache](https://chromium.googlesource.com/v8/v8/+/22afaac)**  
  
Date(Commit): Wed Mar 04 13:38:10 2020  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, reward-0, Security_Impact-Head, allpublic, ClusterFuzz-Verified, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2084814](https://chromium-review.googlesource.com/c/v8/v8/+/2084814)  
Regress: [mjsunit/regress/regress-crbug-1057653.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1057653.js)  
```javascript
Object.prototype.length = 3642395160;
const array = new Float32Array(2**27);

assertThrows(() => {for (const key in array) {}}, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22afaac^!)  
[src/handles/maybe-handles.h](https://cs.chromium.org/chromium/src/v8/src/handles/maybe-handles.h?cl=22afaac)  
[src/objects/keys.cc](https://cs.chromium.org/chromium/src/v8/src/objects/keys.cc?cl=22afaac)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=22afaac)  
[test/cctest/compiler/test-run-jsobjects.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-jsobjects.cc?cl=22afaac)  
[test/cctest/heap/test-heap.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/heap/test-heap.cc?cl=22afaac)  
...  
  

---   

## **regress-crbug-1057094.js (chromium issue)**  
   
**[Issue: CHECK failure: maximum_pages <= std::numeric_limits<size_t>::max() / wasm::kWasmPageSize in bac](https://crbug.com/1057094)**  
**[Commit: [wasm] Fix memory limit check with custom flags](https://chromium.googlesource.com/v8/v8/+/27538aa)**  
  
Date(Commit): Tue Mar 03 16:17:24 2020  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-82, Target-82  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2083300](https://chromium-review.googlesource.com/c/v8/v8/+/2083300)  
Regress: [mjsunit/regress/wasm/regress-crbug-1057094.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-crbug-1057094.js)  
```javascript
try {
  var __v_50189 = new WebAssembly.Memory({
    initial: 65536
  });
} catch (e) {
  // 32-bit builds will throw a RangeError, that's okay.
  assertTrue(e instanceof RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/27538aa^!)  
[src/objects/backing-store.cc](https://cs.chromium.org/chromium/src/v8/src/objects/backing-store.cc?cl=27538aa)  
[test/mjsunit/regress/wasm/regress-crbug-1057094.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-crbug-1057094.js?cl=27538aa)  
  

---   

## **regress-crbug-1052647.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1052647)**  
**[Commit: [intl] Fix Intl.NumberFormat constructor](https://chromium.googlesource.com/v8/v8/+/09d1472)**  
  
Date(Commit): Tue Mar 03 11:33:53 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2082560](https://chromium-review.googlesource.com/c/v8/v8/+/2082560)  
Regress: [mjsunit/regress/regress-crbug-1052647.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1052647.js)  
```javascript
let useArgs = undefined;
function f(arg) {
    useArgs = 'result' + arguments[0] + arg;
}

Intl.NumberFormat.__proto__ = { [Symbol.hasInstance]: f };

new Intl.NumberFormat();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09d1472^!)  
[src/builtins/builtins-intl.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-intl.cc?cl=09d1472)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=09d1472)  
[test/mjsunit/regress/regress-crbug-1052647.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1052647.js?cl=09d1472)  
  

---   

## **regress-1054466.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: DCHECK failure in is_fp_pair() == other.is_fp_pair() in liftoff-register.h](https://crbug.com/1054466)**  
**[Commit: [liftoff] Check fp_pair when looking up register for reuse](https://chromium.googlesource.com/v8/v8/+/548fda4)**  
  
Date(Commit): Mon Feb 24 12:24:06 2020  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Head, Stability-Libfuzzer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2070006](https://chromium-review.googlesource.com/c/v8/v8/+/2070006)  
Regress: [mjsunit/regress/wasm/regress-1054466.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1054466.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */)
  .addLocals({i32_count: 2}).addLocals({f32_count: 2})
  .addBodyWithEnd([
kExprI32Const, 0x00,  // i32.const
kExprI32Const, 0x00,  // i32.const
kExprI32Const, 0xf9, 0x00,  // i32.const
kExprI32Ior,  // i32.or
kExprI32Eqz,  // i32.eqz
kExprI32Add,  // i32.Add
kSimdPrefix, kExprI32x4Splat,  // i32x4.splat
kExprF32Const, 0x46, 0x5d, 0x00, 0x00,  // f32.const
kExprI32Const, 0x83, 0x01,  // i32.const
kExprI32Const, 0x83, 0x01,  // i32.const
kExprI32Const, 0x83, 0x01,  // i32.const
kExprI32Add,  // i32.Add
kExprI32Add,  // i32.Add
kExprIf, kWasmI32,  // if @33 i32
  kExprI32Const, 0x00,  // i32.const
kExprElse,  // else @37
  kExprI32Const, 0x00,  // i32.const
  kExprEnd,  // end @40
kExprIf, kWasmI32,  // if @41 i32
  kExprI32Const, 0x00,  // i32.const
kExprElse,  // else @45
  kExprI32Const, 0x00,  // i32.const
  kExprEnd,  // end @48
kExprF32ReinterpretI32,  // f32.reinterpret_i32
kExprF32Max,  // f32.max
kSimdPrefix, kExprF32x4Splat,  // f32x4.splat
kExprI32Const, 0x83, 0x01,  // i32.const
kSimdPrefix, kExprI32x4Splat,  // i32x4.splat
kSimdPrefix, kExprI32x4Eq,  // i32x4.eq
kSimdPrefix, kExprI32x4Eq,  // i32x4.eq
kSimdPrefix, kExprV8x16AnyTrue,  // v8x16.any_true
kExprEnd,  // end @64
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
print(instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/548fda4^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=548fda4)  
[test/mjsunit/regress/wasm/regress-1054466.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1054466.js?cl=548fda4)  
[test/mjsunit/wasm/wasm-module-builder.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-module-builder.js?cl=548fda4)  
  

---   

## **regress-1034449.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1034449)**  
**[Commit: [turbofan] Fixes Array constructor with single string argument](https://chromium.googlesource.com/v8/v8/+/86a6ce4)**  
  
Date(Commit): Fri Feb 21 12:26:09 2020  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Security_Impact-Stable, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner, Merge-Review-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2064985](https://chromium-review.googlesource.com/c/v8/v8/+/2064985)  
Regress: [mjsunit/regress/regress-1034449.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1034449.js)  
```javascript
function f(len) {
  return new Array(len);
}

%PrepareFunctionForOptimization(f);
assertEquals(3, f(3).length);
assertEquals(18, f(18).length);
%OptimizeFunctionOnNextCall(f);
assertEquals(4, f(4).length);
assertOptimized(f);
let a = f("8");
assertUnoptimized(f);
assertEquals(1, a.length);
assertEquals("8", a[0]);

%PrepareFunctionForOptimization(f);
assertEquals(1, f(1).length);
%OptimizeFunctionOnNextCall(f);
assertEquals(8, f(8).length);
assertOptimized(f);
assertEquals(1, f("8").length);
assertOptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/86a6ce4^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=86a6ce4)  
[test/mjsunit/regress/regress-1034449.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1034449.js?cl=86a6ce4)  
  

---   

## **regress-crbug-1050046.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: SmiBelow(effective_index, LoadFixedArrayBaseLength(array))](https://crbug.com/1050046)**  
**[Commit: [keys] Make sure we don't leak the enum cache in slow-mode for/in](https://chromium.googlesource.com/v8/v8/+/4b0916a)**  
  
Date(Commit): Thu Feb 20 16:44:41 2020  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Security_Severity-High, Security_Impact-Beta, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, M-81, Target-81, merge-merged-8.1, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2066959](https://chromium-review.googlesource.com/c/v8/v8/+/2066959)  
Regress: [mjsunit/regress/regress-crbug-1050046.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1050046.js)  
```javascript
var __v_6 = new Boolean();
 __v_6.first = 0;
 __v_6.prop = 1;
for (var __v_2 in __v_6) {
 delete __v_6.prop;
 gc();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b0916a^!)  
[src/objects/keys.cc](https://cs.chromium.org/chromium/src/v8/src/objects/keys.cc?cl=4b0916a)  
[test/mjsunit/regress/regress-crbug-1050046.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1050046.js?cl=4b0916a)  
  

---   

## **regress-10126-streaming.js (v8 issue)**  
   
**[Issue: [wasm] Decoders should throw on invalid custom section size](https://crbug.com/v8/10126)**  
**[Commit: [wasm] The name of a custom section can cause a validation error](https://chromium.googlesource.com/v8/v8/+/03d5a7b)**  
  
Date(Commit): Wed Feb 19 18:39:25 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2041446](https://chromium-review.googlesource.com/c/v8/v8/+/2041446)  
Regress: [mjsunit/regress/wasm/regress-10126-streaming.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-10126-streaming.js), [mjsunit/regress/wasm/regress-10126.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-10126.js)  
```javascript
load('test/mjsunit/regress/wasm/regress-10126.js')  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/03d5a7b^!)  
[src/wasm/decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/decoder.h?cl=03d5a7b)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=03d5a7b)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=03d5a7b)  
[src/wasm/module-decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.h?cl=03d5a7b)  
[src/wasm/wasm-engine.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-engine.cc?cl=03d5a7b)  
...  
  

---   

## **regress-1053604.js (chromium issue)**  
   
**[Issue: Security: Incorrect side effect modelling for JSCreate](https://crbug.com/1053604)**  
**[Commit: [turbofan] Fix bug in receiver maps inference](https://chromium.googlesource.com/v8/v8/+/fb0a60e)**  
  
Date(Commit): Wed Feb 19 10:15:34 2020  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Restrict-AddIssueComment-EditIssue, Security_Impact-Stable, M-80, Security_Severity-High, ReleaseBlock-Stable, allpublic, Arch-ARM, ClusterFuzz-Verified, CVE_description-missing, Merge-Approved-79, Target-80, merge-merged-8.0, merge-merged-8.1, Respin-Cause-Android-80, Respin-Cause-Desktop-80, Release-3-M80, CVE-2020-6418  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2062396](https://chromium-review.googlesource.com/c/v8/v8/+/2062396)  
Regress: [mjsunit/compiler/regress-1053604.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1053604.js)  
```javascript
let a = [0, 1, 2, 3, 4];

function empty() {}

function f(p) {
  a.pop(Reflect.construct(empty, arguments, p));
}

let p = new Proxy(Object, {
    get: () => (a[0] = 1.1, Object.prototype)
});

function main(p) {
  f(p);
}

%PrepareFunctionForOptimization(empty);
%PrepareFunctionForOptimization(f);
%PrepareFunctionForOptimization(main);

main(empty);
main(empty);
%OptimizeFunctionOnNextCall(main);
main(p);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb0a60e^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=fb0a60e)  
[test/mjsunit/compiler/regress-1053604.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1053604.js?cl=fb0a60e)  
  

---   

## **regress-1051912.js (chromium issue)**  
   
**[Issue: DCHECK failure in 1 == map_.count(key) in wasm-engine.cc](https://crbug.com/1051912)**  
**[Commit: [wasm] Fix streaming compilation prefix hash](https://chromium.googlesource.com/v8/v8/+/80c7ab4)**  
  
Date(Commit): Thu Feb 13 20:53:17 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-82, Target-82, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2054099](https://chromium-review.googlesource.com/c/v8/v8/+/2054099)  
Regress: [mjsunit/regress/wasm/regress-1051912.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1051912.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js')

let binary = new Binary();
binary.emit_header();
binary.emit_bytes([kTypeSectionCode, 4, 1, kWasmFunctionTypeForm, 0, 0]);
binary.emit_bytes([kFunctionSectionCode, 2, 1, 0]);
binary.emit_bytes([kCodeSectionCode, 6, 1, 4]);
binary.emit_bytes([kUnknownSectionCode, 2, 1, 0]);
binary.emit_bytes([kUnknownSectionCode, 2, 1, 0]);
binary.emit_bytes([kUnknownSectionCode, 2, 1, 0]);
binary.emit_bytes([ kExprEnd]);
let buffer = binary.trunc_buffer();
WebAssembly.compile(buffer);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/80c7ab4^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=80c7ab4)  
[test/mjsunit/regress/wasm/regress-1051912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1051912.js?cl=80c7ab4)  
  

---   

## **regress-1051017.js (chromium issue)**  
   
**[Issue: Security: Type inference issue in Typer::Visitor::TypeInductionVariablePhi](https://crbug.com/1051017)**  
**[Commit: [turbofan] Fix bug in Typer::TypeInductionVariablePhi](https://chromium.googlesource.com/v8/v8/+/a2e971c)**  
  
Date(Commit): Thu Feb 13 13:29:25 2020  
Components: Blink>JavaScript>Compiler  
Labels: Security_Impact-Stable, M-80, Security_Severity-High, allpublic, ClusterFuzz-Verified, CVE_description-submitted, Target-80, merge-merged-8.0, merge-merged-8.1, Release-2-M80, CVE-2020-6383, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2051957](https://chromium-review.googlesource.com/c/v8/v8/+/2051957)  
Regress: [mjsunit/compiler/regress-1051017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1051017.js)  
```javascript
function foo1() {
  var x = -Infinity;
  var i = 0;
  for (; i < 1; i += x) {
    if (i == -Infinity) x = +Infinity;
  }
  return i;
}

%PrepareFunctionForOptimization(foo1);
assertEquals(NaN, foo1());
assertEquals(NaN, foo1());
%OptimizeFunctionOnNextCall(foo1);
assertEquals(NaN, foo1());


function foo2() {
  var i = -Infinity;
  for (; i <= 42; i += Infinity) { }
  return i;
}

%PrepareFunctionForOptimization(foo2);
assertEquals(NaN, foo2());
assertEquals(NaN, foo2());
%OptimizeFunctionOnNextCall(foo2);
assertEquals(NaN, foo2());


function foo3(b) {
  var k = 0;
  let str = b ? "42" : "0";
  for (var i = str; i < 1 && k++ < 1; i -= 0) { }
  return i;
}

%PrepareFunctionForOptimization(foo3);
assertEquals(0, foo3());
assertEquals(0, foo3());
%OptimizeFunctionOnNextCall(foo3);
assertEquals(0, foo3());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2e971c^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=a2e971c)  
[test/mjsunit/compiler/regress-1051017.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1051017.js?cl=a2e971c)  
  

---   

## **regress-1049982-1.js (chromium issue)**  
   
**[Issue: Array.reduce callback is called with the wrong accumulator](https://crbug.com/1049982)**  
**[Commit: [gasm] Fix deopt frame state in Array.p.reduce and reduceRight](https://chromium.googlesource.com/v8/v8/+/099de33)**  
  
Date(Commit): Tue Feb 11 16:38:33 2020  
Components: Enterprise, Blink>JavaScript  
Labels: Hotlist-Merge-Review, Hotlist-Enterprise, M-80, ReleaseBlock-Stable, Via-Wizard-Javascript, Triaged-ET, Target-80, Postmortem-Needed, M-81, M-82, FoundIn-80, Target-81, Enterprise-TSE, Target-82, Needs-Triage-M80, merge-merged-8.0, merge-merged-8.1, TE-Verified-82.0.4055.2  
Code Review: [https://crrev.com/c/1934329.](https://crrev.com/c/1934329.)  
Regress: [mjsunit/regress/regress-1049982-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1049982-1.js), [mjsunit/regress/regress-1049982-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1049982-2.js)  
```javascript
const xs = [1,2,3,4,5,6,7,8,9];
let deopt = false;

function g(acc, x, i) {
  if (deopt) {
    assertFalse(%IsBeingInterpreted());
    Array.prototype.x = 42;  // Trigger a lazy deopt.
    deopt = false;
  }
  return acc + x;
}

function f() {
  return xs.reduce(g, 0);
}

%PrepareFunctionForOptimization(f);
%PrepareFunctionForOptimization(g);
assertEquals(45, f());
assertEquals(45, f());
%OptimizeFunctionOnNextCall(f);

deopt = true;
assertEquals(45, f());
assertEquals(45, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/099de33^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=099de33)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=099de33)  
[test/mjsunit/regress/regress-1049982-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1049982-1.js?cl=099de33)  
[test/mjsunit/regress/regress-1049982-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1049982-2.js?cl=099de33)  
  

---   

## **regress-1048241.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: Stack-buffer-overflow in v8::internal::wasm::LiftoffAssembler::VarState::is_reg](https://crbug.com/1048241)**  
**[Commit: [liftoff][ia32] Fix AtomicStore register spilling](https://chromium.googlesource.com/v8/v8/+/0e2e50d)**  
  
Date(Commit): Tue Feb 04 09:39:54 2020  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, Stability-Libfuzzer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components, M-81, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2036073](https://chromium-review.googlesource.com/c/v8/v8/+/2036073)  
Regress: [mjsunit/regress/wasm/regress-1048241.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1048241.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false, true);
const sig = makeSig([kWasmF64, kWasmI64, kWasmI32, kWasmF64], []);
builder.addFunction(undefined, sig).addBody([
  kExprI32Const, 0x00,                               // -
  kExprI32Const, 0x00,                               // -
  kExprI32Const, 0x00,                               // -
  kAtomicPrefix, kExprI32AtomicXor16U, 0x01, 0x00,   // -
  kAtomicPrefix, kExprI32AtomicStore8U, 0x00, 0x00,  // -
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0e2e50d^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=0e2e50d)  
[test/mjsunit/regress/wasm/regress-1048241.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1048241.js?cl=0e2e50d)  
  

---   

## **regress-crbug-1047368.js (chromium issue)**  
   
**[Issue: DCHECK failure in name->IsFlat() in factory.cc](https://crbug.com/1047368)**  
**[Commit: [ast] Flatten Wasm function names](https://chromium.googlesource.com/v8/v8/+/6abbfe2)**  
  
Date(Commit): Fri Jan 31 11:25:45 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2030738](https://chromium-review.googlesource.com/c/v8/v8/+/2030738)  
Regress: [mjsunit/regress/regress-crbug-1047368.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1047368.js)  
```javascript
new WebAssembly.Function({
    parameters: [],
    results: []
  }, x => x);
const long_variable = {
  toString: () => {
  }
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6abbfe2^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=6abbfe2)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=6abbfe2)  
[test/mjsunit/regress/regress-crbug-1047368.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1047368.js?cl=6abbfe2)  
  

---   

## **regress-1045225.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: Stack-buffer-overflow in v8::internal::wasm::LiftoffAssembler::VarState::is_reg](https://crbug.com/1045225)**  
**[Commit: [Liftoff] Clean up implementation of AtomicStore](https://chromium.googlesource.com/v8/v8/+/d8bb229)**  
  
Date(Commit): Fri Jan 31 08:54:44 2020  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, Stability-Libfuzzer, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-81, Target-81, merge-merged-8.3  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2029414](https://chromium-review.googlesource.com/c/v8/v8/+/2029414)  
Regress: [mjsunit/regress/wasm/regress-1045225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1045225.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  const builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false, true);
  builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
  // Generate function 1 (out of 1).
  builder.addFunction(undefined, 0 /* sig */)
    .addBodyWithEnd([
kExprI32Const, 0x80, 0x01,
kExprI32Clz,
kExprI32Const, 0x00,
kExprI64Const, 0x00,
kAtomicPrefix, kExprI64AtomicStore8U, 0x00, 0x00,
kExprEnd,   // @13
            ]);
  builder.addExport('main', 0);
  const instance = builder.instantiate();
  print(instance.exports.main(1, 2, 3));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8bb229^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=d8bb229)  
[test/mjsunit/regress/wasm/regress-1045225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1045225.js?cl=d8bb229)  
  

---   

## **regress-1046472.js (chromium issue)**  
   
**[Issue: v8_wasm_code_fuzzer: Fatal error in ](https://crbug.com/1046472)**  
**[Commit: [wasm] Type check brtable if it's not unreachable](https://chromium.googlesource.com/v8/v8/+/8ff14f5)**  
  
Date(Commit): Thu Jan 30 13:46:15 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2027990](https://chromium-review.googlesource.com/c/v8/v8/+/2027990)  
Regress: [mjsunit/regress/wasm/regress-1046472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1046472.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  const builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
  // Generate function 1 (out of 1).
  builder.addFunction(undefined, 0 /* sig */)
    .addBodyWithEnd([
kExprI32Const, 0x20,
kExprI64LoadMem, 0x00, 0xce, 0xf2, 0xff, 0x01,
kExprBlock, kWasmF32,   // @9 f32
  kExprI32Const, 0x04,
  kExprI32Const, 0x01,
  kExprBrTable, 0x01, 0x01, 0x00, // entries=1
  kExprEnd,   // @19
kExprUnreachable,
kExprEnd,   // @21
            ]);
  builder.addExport('main', 0);
  assertThrows(
      () => {builder.toModule()}, WebAssembly.CompileError,
      'WebAssembly.Module(): Compiling function #0:\"main\" failed: ' +
      'inconsistent type in br_table target 1 (previous was i32, ' +
      'this one is f32) @+60');
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ff14f5^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=8ff14f5)  
[test/mjsunit/regress/wasm/regress-1046472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1046472.js?cl=8ff14f5)  
  

---   

## **regress-crbug-1044909.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1044909)**  
**[Commit: Fix one more LookupIterator](https://chromium.googlesource.com/v8/v8/+/efaa34b)**  
  
Date(Commit): Wed Jan 29 16:49:50 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2023558](https://chromium-review.googlesource.com/c/v8/v8/+/2023558)  
Regress: [mjsunit/regress/regress-crbug-1044909.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1044909.js)  
```javascript
function main() {
  const v2 = Object.prototype;
  v2[4294967296] = {};
  const v12 = {get: function() {}};
  Object.defineProperty(v2, 4294967296, v12);
  const v15 = {...v2};
}
%PrepareFunctionForOptimization(main);
main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/efaa34b^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=efaa34b)  
[test/mjsunit/regress/regress-crbug-1044909.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1044909.js?cl=efaa34b)  
  

---   

## **regress-crbug-1041251.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/1041251)**  
**[Commit: [builtins] Fix FastCreateDataProperty](https://chromium.googlesource.com/v8/v8/+/68cc5c6)**  
  
Date(Commit): Wed Jan 29 12:25:03 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2023551](https://chromium-review.googlesource.com/c/v8/v8/+/2023551)  
Regress: [mjsunit/regress/regress-crbug-1041251.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1041251.js)  
```javascript
let v0 = [0, 1];
v0.constructor = {
  [Symbol.species]: function() {
    let v1 = [2];
    Object.defineProperty(v1, "length", {writable: false});
    return v1;
  }
}

assertThrows(() => Array.prototype.map.call(v0, function() {}), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68cc5c6^!)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=68cc5c6)  
[test/mjsunit/regress/regress-crbug-1041251.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1041251.js?cl=68cc5c6)  
  

---   

## **regress-1044919.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1044919)**  
**[Commit: [runtime] Don't invalidate property cell when it becomes read-only](https://chromium.googlesource.com/v8/v8/+/e395871)**  
  
Date(Commit): Wed Jan 29 11:06:42 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2023562](https://chromium-review.googlesource.com/c/v8/v8/+/2023562)  
Regress: [mjsunit/regress/regress-1044919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1044919.js)  
```javascript
(function main() {
  eval();
  function foo() {
    bla = [];
    bla.__proto__ = '';
  }
  %PrepareFunctionForOptimization(foo);
  foo();
  Object.defineProperty(this, 'bla',
      {value: bla, configurable: false, writable: true});
  foo();
  %OptimizeFunctionOnNextCall(foo);
  foo();
  Object.defineProperty(this, 'bla',
      {value: bla, configurable: false, writable: false});
  foo();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e395871^!)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=e395871)  
[test/mjsunit/regress/regress-1044919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1044919.js?cl=e395871)  
  

---   

## **regress-crbug-1044911.js (chromium issue)**  
   
**[Issue:  Fatal error - unreachable code](https://crbug.com/1044911)**  
**[Commit: Fix ArrayLengthSetter for suddenly frozen elements](https://chromium.googlesource.com/v8/v8/+/2d10033)**  
  
Date(Commit): Wed Jan 29 10:52:52 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, TE-NeedsTriageHelp, Needs-Milestone  
Code Review: [https://codereview.chromium.org/2543553002](https://codereview.chromium.org/2543553002)  
Regress: [mjsunit/regress/regress-crbug-1044911.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1044911.js)  
```javascript
let a = [0];
let l = {
  valueOf: function() {
    Object.freeze(a);
    return 1;
  }
};
a.length = l;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d10033^!)  
[src/builtins/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/accessors.cc?cl=2d10033)  
[test/mjsunit/regress/regress-crbug-1044911.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1044911.js?cl=2d10033)  
  

---   

## **regress-1045737.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: Fatal error in ](https://crbug.com/1045737)**  
**[Commit: [wasm][liftoff] Zero-extend result of atomic.add](https://chromium.googlesource.com/v8/v8/+/82b7819)**  
  
Date(Commit): Mon Jan 27 14:02:35 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2020768](https://chromium-review.googlesource.com/c/v8/v8/+/2020768)  
Regress: [mjsunit/regress/wasm/regress-1045737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1045737.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false, true);
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */).addBodyWithEnd([
  // signature: i_iii
  // body:
  kExprI32Const, 0x00, kExprI64Const, 0xc2, 0xe6, 0x00, kAtomicPrefix,
  kExprI64AtomicAdd8U, 0x00, 0xb6, 0x0e, kExprF32SConvertI64,
  kExprI32SConvertF32,
  kExprEnd,  // @14
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(instance.exports.main(1, 2, 3), 0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82b7819^!)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=82b7819)  
[test/mjsunit/regress/wasm/regress-1045737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1045737.js?cl=82b7819)  
  

---   

## **regress-10138.js (v8 issue)**  
   
**[Issue: --interpreted-frames-native-stack crashing with compressed pointers](https://crbug.com/v8/10138)**  
**[Commit: Fix native stacks flag for pointer compression](https://chromium.googlesource.com/v8/v8/+/99641cb)**  
  
Date(Commit): Tue Jan 21 09:40:57 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2011206](https://chromium-review.googlesource.com/c/v8/v8/+/2011206)  
Regress: [mjsunit/regress/regress-10138.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-10138.js)  
```javascript
function f() {
  g();
}

function g() {
  %DeoptimizeFunction(f);
  %DeoptimizeFunction(f);
}

%PrepareFunctionForOptimization(f);
f(); f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/99641cb^!)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=99641cb)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=99641cb)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=99641cb)  
[test/mjsunit/regress/regress-10138.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-10138.js?cl=99641cb)  
  

---   

## **regress-1037771.js (chromium issue)**  
   
**[Issue: CHECK failure: TypeError: node #190:LoopExitValue type HeapConstant(ADDRESS {ADDRESS <undefined](https://crbug.com/1037771)**  
**[Commit: [turbofan] Don't verify context input of Create*Context nodes](https://chromium.googlesource.com/v8/v8/+/e33d633)**  
  
Date(Commit): Mon Jan 20 18:25:04 2020  
Components: Blink>JavaScript>Compiler  
Labels: Security_Impact-Stable, TE-NeedsTriageHelp, Via-Wizard-Javascript, Triaged-ET  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2010792](https://chromium-review.googlesource.com/c/v8/v8/+/2010792)  
Regress: [mjsunit/compiler/regress-1037771.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1037771.js)  
```javascript
async function* r() {
  for (var l = "" in { goo: ()=>{} }) {
    for (let n = 0; n < 500; (t ? -500 : 0)) {
      n++;
      if (n > 1) break;
      try {
        r.blabadfasdfasdfsdafsdsadf();
      } catch (e) {
        for (let n = 0; n < 500; n++);
        for (let n in t) {
          return t[n];
        }
      }
      try { r(n, null) } catch (e) {}
    }
  }
}
let t = r();
t.return({
  get then() {
    let n = r();
    n.next();
  }
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e33d633^!)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=e33d633)  
[test/mjsunit/compiler/regress-1037771.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-1037771.js?cl=e33d633)  
  

---   

## **regress-v8-10072.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/10072)**  
**[Commit: [regexp] Fix CP advancement in all SKIP_* bytecodes](https://chromium.googlesource.com/v8/v8/+/aedc824)**  
  
Date(Commit): Thu Jan 16 13:10:34 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2002543](https://chromium-review.googlesource.com/c/v8/v8/+/2002543)  
Regress: [mjsunit/regress/regress-v8-10072.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-10072.js)  
```javascript
assertNull(/(?<=a[^b]*)./.exec('a'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aedc824^!)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=aedc824)  
[test/mjsunit/regress/regress-v8-10072.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-10072.js?cl=aedc824)  
  

---   

## **regress-crbug-1038178.js (chromium issue)**  
   
**[Issue: Security: Missing deoptimization information for OptimizedFrame::Summarize](https://crbug.com/1038178)**  
**[Commit: Add missing test for optional chains with undefined receiver](https://chromium.googlesource.com/v8/v8/+/0bc9e52)**  
  
Date(Commit): Tue Jan 14 20:11:57 2020  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, reward-0, Security_Impact-Head, M-80, Security_Severity-High, ReleaseBlock-Stable, allpublic, ClusterFuzz-Verified, M-81, Target-81, merge-merged-8.0  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1987914](https://chromium-review.googlesource.com/c/v8/v8/+/1987914)  
Regress: [mjsunit/regress/regress-crbug-1038178.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1038178.js)  
```javascript
function opt(){
    (new (function(){
        try{
            r.c>new class extends(W.y.h){}
            l.g.e._
            function _(){}[]=({[l](){}}),c()
        }catch(x){}
    }));
    (((function(){})())?.v)()
}
%PrepareFunctionForOptimization(opt)
assertThrows(opt());
assertThrows(opt());
%OptimizeFunctionOnNextCall(opt)
assertThrows(opt());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0bc9e52^!)  
[test/mjsunit/regress/regress-crbug-1038178.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1038178.js?cl=0bc9e52)  
  

---   

## **regress-crbug-1041616.js (chromium issue)**  
   
**[Issue: DCHECK failure in cache != this implies cache->outer_scope()->deserialized_scope_uses_external_cac](https://crbug.com/1041616)**  
**[Commit: [parser] Fix cache scope recursion for `with`](https://chromium.googlesource.com/v8/v8/+/a85d74a)**  
  
Date(Commit): Tue Jan 14 13:57:47 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-81, Target-81  
Code Review: [https://crrev.com/c/1997135](https://crrev.com/c/1997135)  
Regress: [mjsunit/regress/regress-crbug-1041616.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1041616.js)  
```javascript
global = 0;

(function foo() {
  function bar() {
    let context_allocated = 0;
    with ({}) {
      f = function() { ++global; }
    }
    function baz() { return foo(context_allocated); };
    f();
  };
  bar();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a85d74a^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=a85d74a)  
[test/mjsunit/regress/regress-crbug-1041210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1041210.js?cl=a85d74a)  
[test/mjsunit/regress/regress-crbug-1041616.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1041616.js?cl=a85d74a)  
  

---   

## **regress-crbug-1041232.js (chromium issue)**  
   
**[Issue: Multi-mapped Mock Allocator sometimes fails to allocate](https://crbug.com/1041232)**  
**[Commit: [test] Fix Multi-Mapped Mock Allocator](https://chromium.googlesource.com/v8/v8/+/bd51a5e)**  
  
Date(Commit): Mon Jan 13 19:44:26 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1997442](https://chromium-review.googlesource.com/c/v8/v8/+/1997442)  
Regress: [mjsunit/regress/regress-crbug-1041232.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1041232.js)  
```javascript
let kSize = 128 * 1024 * 1024;
let kChunkSize = 2 * 1024 * 1024;
let a = new Uint8Array(kSize);

for (let i = 0; i < kChunkSize; i++) {
  a[i] = 42;
}

assertEquals(undefined, a[-kChunkSize - 1]);
assertEquals(undefined, a[-kChunkSize]);
assertEquals(undefined, a[-1]);
assertEquals(42, a[0]);
assertEquals(42, a[1]);
assertEquals(42, a[kChunkSize]);
assertEquals(42, a[kChunkSize + 1]);
assertEquals(42, a[kChunkSize + 1]);
assertEquals(42, a[kSize - kChunkSize]);
assertEquals(42, a[kSize - 1]);
assertEquals(undefined, a[kSize]);
assertEquals(undefined, a[kSize + 1]);
assertEquals(undefined, a[kSize + kChunkSize]);
assertEquals(undefined, a[kSize + kSize]);

assertThrows(() => new ArrayBuffer(Number.MAX_SAFE_INTEGER));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd51a5e^!)  
[src/d8/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8/d8.cc?cl=bd51a5e)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=bd51a5e)  
[test/mjsunit/regress/regress-crbug-1041232.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1041232.js?cl=bd51a5e)  
  

---   

## **regress-crbug-1041210.js (chromium issue)**  
   
**[Issue: CHECK failure: Bytecode mismatch at offset 10 in interpreter.cc](https://crbug.com/1041210)**  
**[Commit: [parser] Fix caching dynamic vars on wrong scope](https://chromium.googlesource.com/v8/v8/+/304e97d)**  
  
Date(Commit): Mon Jan 13 15:06:15 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-81, Target-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1997135](https://chromium-review.googlesource.com/c/v8/v8/+/1997135)  
Regress: [mjsunit/regress/regress-crbug-1041210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1041210.js)  
```javascript
with ({}) {
  (() => eval())();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/304e97d^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=304e97d)  
[test/mjsunit/regress/regress-crbug-1041210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1041210.js?cl=304e97d)  
  

---   

## **regress-chromium-1040238.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Torque assert 'Is<A>(o)' failed](https://crbug.com/1040238)**  
**[Commit: Reland "Reland "Reland "[promises] Port Promise.race to Torque."""](https://chromium.googlesource.com/v8/v8/+/d8fe5b9)**  
  
Date(Commit): Fri Jan 10 13:37:50 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1991954](https://chromium-review.googlesource.com/c/v8/v8/+/1991954)  
Regress: [mjsunit/regress/regress-chromium-1040238.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-chromium-1040238.js)  
```javascript
class MyPromise extends Promise {
  static resolve() {
    return {
      then() {
        throw "then throws";
      }
    };
  }
}

let myIterable = {
  [Symbol.iterator]() {
    return {
      next() {
          return {};
      },
      get return() { return {}; },
    };
  }
};

MyPromise.race(myIterable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8fe5b9^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=d8fe5b9)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=d8fe5b9)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=d8fe5b9)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=d8fe5b9)  
[src/builtins/iterator.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/iterator.tq?cl=d8fe5b9)  
...  
  

---   

## **regress-9209.js (v8 issue)**  
   
**[Issue: D8 crashes on {quit}](https://crbug.com/v8/9209)**  
**[Commit: [verify-heap] Move verification to Heap::StartTearDown](https://chromium.googlesource.com/v8/v8/+/7b6de83)**  
  
Date(Commit): Fri Jan 10 12:06:01 2020  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1993968](https://chromium-review.googlesource.com/c/v8/v8/+/1993968)  
Regress: [mjsunit/regress/regress-9209.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9209.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addImport("d8", "quit", kSig_v_v)
builder.addFunction('do_not_crash', kSig_v_v)
        .addBody([kExprCallFunction, 0])
        .exportFunc();
builder.instantiate({d8: {quit: quit}}).exports.do_not_crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b6de83^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=7b6de83)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=7b6de83)  
[test/mjsunit/regress/regress-9209.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9209.js?cl=7b6de83)  
  

---   

## **regress-1033948.js (chromium issue)**  
   
**[Issue: CHECK failure: !isolate->has_scheduled_exception() in builtins-console.cc](https://crbug.com/1033948)**  
**[Commit: [wasm] Leave Global constructor on error](https://chromium.googlesource.com/v8/v8/+/bc9db9f)**  
  
Date(Commit): Thu Jan 09 17:51:12 2020  
Components: Blink>JavaScript  
Labels: TE-NeedsTriageHelp, Via-Wizard-Other  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1993283](https://chromium-review.googlesource.com/c/v8/v8/+/1993283)  
Regress: [mjsunit/regress/wasm/regress-1033948.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1033948.js)  
```javascript
const desc = {
  get mutable() {
    throw "foo";
  },
  get value() {
    console.trace();
  }
};
assertThrowsEquals(() => new WebAssembly.Global(desc), "foo");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc9db9f^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=bc9db9f)  
[test/mjsunit/regress/wasm/regress-1033948.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1033948.js?cl=bc9db9f)  
  

---   

## **regress-1040403.js (chromium issue)**  
   
**[Issue: DCHECK failure in mode == JSHeapBroker::BrokerMode::kSerialized implies kind == kUnserializedReadO](https://crbug.com/1040403)**  
**[Commit: [turbofan] Allow handle deferences when compiling non concurrently](https://chromium.googlesource.com/v8/v8/+/36cb5f4)**  
  
Date(Commit): Thu Jan 09 12:54:51 2020  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-81, Target-81  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1992433](https://chromium-review.googlesource.com/c/v8/v8/+/1992433)  
Regress: [mjsunit/regress/regress-1040403.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1040403.js)  
```javascript
function bytes() {
}
function __f_4622() {
  var __v_22507 = {
  };
}
%PrepareFunctionForOptimization(__f_4622);
%OptimizeFunctionOnNextCall(__f_4622);
42 |  __f_4622();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/36cb5f4^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=36cb5f4)  
[test/mjsunit/regress/regress-1040403.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1040403.js?cl=36cb5f4)  
  

---   

## **regress-crbug-1020162.js (chromium issue)**  
   
**[Issue: CHECK failure: Bytecode mismatch at offset 12 in interpreter.cc](https://crbug.com/1020162)**  
**[Commit: [parser] Force stable order for variables](https://chromium.googlesource.com/v8/v8/+/c8ab41e)**  
  
Date(Commit): Thu Jan 09 10:25:31 2020  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-79, M-79  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1977865](https://chromium-review.googlesource.com/c/v8/v8/+/1977865)  
Regress: [mjsunit/regress/regress-crbug-1020162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1020162.js)  
```javascript
((get = foo.get(...a, a), b) => 0)();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8ab41e^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=c8ab41e)  
[test/mjsunit/regress/regress-crbug-1020162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1020162.js?cl=c8ab41e)  
  

---   

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

## **regress-crbug-1038140.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Torque assert 'Is<A>(o)' failed](https://crbug.com/1038140)**  
**[Commit: Reland "[promises] Port Promise.race to Torque."](https://chromium.googlesource.com/v8/v8/+/766aeb9)**  
  
Date(Commit): Tue Jan 07 17:46:15 2020  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Security_Impact-Stable, Clusterfuzz, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1965919](https://chromium-review.googlesource.com/c/v8/v8/+/1965919)  
Regress: [mjsunit/regress/regress-crbug-1038140.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1038140.js)  
```javascript
Promise.resolve = function() { return {}; };
Promise.race([function() {}]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/766aeb9^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=766aeb9)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=766aeb9)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=766aeb9)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=766aeb9)  
[src/builtins/iterator.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/iterator.tq?cl=766aeb9)  
...  
  

---   

## **regress-1038588.js (chromium issue)**  
   
**[Issue: Security: Fatal error in ../../src/runtime/runtime-scopes.cc, line 269](https://crbug.com/1038588)**  
**[Commit: [parser] Fix conflict detection loop early exit](https://chromium.googlesource.com/v8/v8/+/2a6c0f4)**  
  
Date(Commit): Tue Jan 07 13:15:05 2020  
Components: Blink>JavaScript  
Labels: M-80, Security_Impact-Beta, allpublic, ClusterFuzz-Verified, Target-80  
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
   
**[Issue: Security: d8 memory corruption with dcheck failed in protectors.cc](https://crbug.com/1033966)**  
**[Commit: [protectors] Remove invalid DCHECK in protectors.](https://chromium.googlesource.com/v8/v8/+/643ae46)**  
  
Date(Commit): Thu Jan 02 15:49:54 2020  
Components: Blink>JavaScript  
Labels: allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-79, M-79  
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
