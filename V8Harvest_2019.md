# V8Harvest  
The Harvest of V8 regress in 2019.  
  

## **regress-sealedarray-store-outofbounds.js (other issue)**  
   
**[Commit: Sealed array should handle store out of bounds in optimized code](https://chromium.googlesource.com/v8/v8/+/e69460e)**  
  
Date(Commit): Wed May 08 17:19:02 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1597116](https://chromium-review.googlesource.com/c/v8/v8/+/1597116)  
Regress: [mjsunit/compiler/regress-sealedarray-store-outofbounds.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-sealedarray-store-outofbounds.js)  
```javascript
const v3 = [0,"symbol"];
const v5 = 0 - 1;
const v6 = Object.seal(v3);
let v9 = 0;
function f1() {
  v6[119090556] = v5;
}
%PrepareFunctionForOptimization(f1);
f1();
%OptimizeFunctionOnNextCall(f1);
f1();
assertOptimized(f1);
assertEquals(v6.length, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e69460e^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=e69460e)  
[test/mjsunit/compiler/regress-sealedarray-store-outofbounds.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-sealedarray-store-outofbounds.js?cl=e69460e)  
  
  
---   

## **regress-v8-9113.js (v8 issue)**  
   
**[Issue 9113:
 Constant field tracking ignores field change from 0 to -0](https://crbug.com/v8/9113)**  
**[Commit: [turbofan] Switch equality check for constant fields to SameValue.](https://chromium.googlesource.com/v8/v8/+/42b90af)**  
  
Date(Commit): Thu Apr 11 11:59:24 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1561317](https://chromium-review.googlesource.com/c/v8/v8/+/1561317)  
Regress: [mjsunit/compiler/regress-v8-9113.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-9113.js)  
```javascript
let dummy = {x: 0.1};

let o = {x: 0};

function f(o, v) {
  o.x = v;
};
%PrepareFunctionForOptimization(f);
f(o, 0);
f(o, 0);
assertEquals(Infinity, 1 / o.x);
%OptimizeFunctionOnNextCall(f);
f(o, -0);
assertEquals(-Infinity, 1 / o.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/42b90af^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=42b90af)  
[test/mjsunit/compiler/regress-v8-9113.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-v8-9113.js?cl=42b90af)  
  

---   

## **regress-v8-9106.js (v8 issue)**  
   
**[Issue 9106:
 [wasm] [bulk memory] DCHECK when passive data segment is at end of module](https://crbug.com/v8/9106)**  
**[Commit: [wasm] Fix DCHECK with empty passive data segment](https://chromium.googlesource.com/v8/v8/+/b29993f)**  
  
Date(Commit): Wed Apr 10 18:10:58 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1560405](https://chromium-review.googlesource.com/c/v8/v8/+/1560405)  
Regress: [mjsunit/regress/regress-v8-9106.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9106.js)  
```javascript
let bytes = new Uint8Array([
  0,   97,  115, 109, 1,   0,   0,   0,   1,   132, 128, 128, 128, 0,   1,
  96,  0,   0,   3,   133, 128, 128, 128, 0,   4,   0,   0,   0,   0,   5,
  131, 128, 128, 128, 0,   1,   0,   1,   7,   187, 128, 128, 128, 0,   4,
  12,  100, 114, 111, 112, 95,  112, 97,  115, 115, 105, 118, 101, 0,   0,
  12,  105, 110, 105, 116, 95,  112, 97,  115, 115, 105, 118, 101, 0,   1,
  11,  100, 114, 111, 112, 95,  97,  99,  116, 105, 118, 101, 0,   2,   11,
  105, 110, 105, 116, 95,  97,  99,  116, 105, 118, 101, 0,   3,   12,  129,
  128, 128, 128, 0,   2,   10,  183, 128, 128, 128, 0,   4,   133, 128, 128,
  128, 0,   0,   252, 9,   0,   11,  140, 128, 128, 128, 0,   0,   65,  0,
  65,  0,   65,  0,   252, 8,   0,   0,   11,  133, 128, 128, 128, 0,   0,
  252, 9,   1,   11,  140, 128, 128, 128, 0,   0,   65,  0,   65,  0,   65,
  0,   252, 8,   1,   0,   11,  11,  136, 128, 128, 128, 0,   2,   1,   0,
  0,   65,  0,   11,  0
]);
new WebAssembly.Instance(new WebAssembly.Module(bytes));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b29993f^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=b29993f)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=b29993f)  
[test/mjsunit/regress/regress-v8-9106.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9106.js?cl=b29993f)  
  

---   

## **regress-947822.js (chromium issue)**  
   
**[Issue 947822:
 V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/947822)**  
**[Commit: [regexp] Ensure ToString(replaceValue) is called once in @@replace](https://chromium.googlesource.com/v8/v8/+/f8d1169)**  
  
Date(Commit): Wed Apr 10 07:12:14 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1559739](https://chromium-review.googlesource.com/c/v8/v8/+/1559739)  
Regress: [mjsunit/regress/regress-947822.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-947822.js)  
```javascript
let cnt = 0;
const re = /x/y;
const replacement = {
  toString: () => {
    cnt++;
    if (cnt == 2) {
      re.lastIndex = { valueOf: () => { re.x = -1073741825; return 7; }};
    }
    return 'y$';
  }
};

const str = re[Symbol.replace]("x", replacement);
assertEquals(str, "y$");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f8d1169^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=f8d1169)  
[test/mjsunit/regress/regress-947822.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-947822.js?cl=f8d1169)  
  

---   

## **regress-9087.js (v8 issue)**  
   
**[Issue 9087:
 mjsunit/regress/polymorphic-accessor-test-context starts flaking on gc fuzzer](https://crbug.com/v8/9087)**  
**[Commit: [turbofan] Add a regression test](https://chromium.googlesource.com/v8/v8/+/d97bc8d)**  
  
Date(Commit): Fri Apr 05 13:57:56 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1554690](https://chromium-review.googlesource.com/c/v8/v8/+/1554690)  
Regress: [mjsunit/compiler/regress-9087.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9087.js)  
```javascript
function constructor() {}
const obj = Object.create(constructor.prototype);

for (let i = 0; i < 1020; ++i) {
  constructor.prototype["x" + i] = 42;
}

function foo() {
  return obj instanceof constructor;
};
%PrepareFunctionForOptimization(foo);
assertTrue(foo());
assertTrue(foo());
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d97bc8d^!)  
[test/mjsunit/compiler/regress-9087.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-9087.js?cl=d97bc8d)  
  

---   

## **regress-9041.js (v8 issue)**  
   
**[Issue 9041:
 instanceof optimization doesn't add proper stability dependencies](https://crbug.com/v8/9041)**  
**[Commit: [turbofan] Fix bug in InferHasInPrototypeChain](https://chromium.googlesource.com/v8/v8/+/4c35194)**  
  
Date(Commit): Mon Apr 01 12:13:48 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1541107](https://chromium-review.googlesource.com/c/v8/v8/+/1541107)  
Regress: [mjsunit/compiler/regress-9041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9041.js)  
```javascript
(function() {
class A {}

function foo(a, fn) {
  const C = a.constructor;
  fn(a);
  return a instanceof C;
};
%PrepareFunctionForOptimization(foo);
assertTrue(foo(new A(), a => {}));
assertTrue(foo(new A(), a => {}));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(new A(), a => {}));
assertFalse(foo(new A(), a => {
  a.__proto__ = {};
}));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c35194^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=4c35194)  
[src/compiler/compilation-dependencies.h](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.h?cl=4c35194)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=4c35194)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=4c35194)  
[test/mjsunit/compiler/regress-9041.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-9041.js?cl=4c35194)  
  

---   

## **regress-9036-1.js (v8 issue)**  
   
**[Issue 9036:
 Proxy implements getPrototypeOf uncorrectly](https://crbug.com/v8/9036)**  
**[Commit: [csa] Fix instanceof for LHS with proxy in prototype chain](https://chromium.googlesource.com/v8/v8/+/b9076b4)**  
  
Date(Commit): Tue Mar 26 19:35:25 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1539497](https://chromium-review.googlesource.com/c/v8/v8/+/1539497)  
Regress: [mjsunit/regress/regress-9036-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9036-1.js), [mjsunit/regress/regress-9036-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9036-2.js), [mjsunit/regress/regress-9036-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9036-3.js)  
```javascript
function C() {};

const p = new Proxy({}, { getPrototypeOf() { return C.prototype } });

assertTrue(p instanceof C);
assertTrue(p instanceof C);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9076b4^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=b9076b4)  
[test/mjsunit/regress/regress-9036-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9036-1.js?cl=b9076b4)  
[test/mjsunit/regress/regress-9036-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9036-2.js?cl=b9076b4)  
[test/mjsunit/regress/regress-9036-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9036-3.js?cl=b9076b4)  
  

---   

## **regress-9022.js (v8 issue)**  
   
**[Issue 9022:
 Wrong break target in asm.js](https://crbug.com/v8/9022)**  
**[Commit: [asm.js] Fix break depth calculation for named blocks.](https://chromium.googlesource.com/v8/v8/+/080fa87)**  
  
Date(Commit): Mon Mar 25 14:00:58 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1538126](https://chromium-review.googlesource.com/c/v8/v8/+/1538126)  
Regress: [mjsunit/regress/regress-9022.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9022.js)  
```javascript
function Module(stdlib, ffi) {
  "use asm";
  var log = ffi.log;
  function boom() {
    while (1) {
      label: {
        break;
      }
      log(1);
      break;
    }
    log(2);
  }
  return { boom: boom }
}

var log_value = 0;
function log(i) {
  log_value += i;
}

Module({}, { log: log }).boom();
assertTrue(%IsAsmWasmCode(Module));
assertEquals(2, log_value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/080fa87^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=080fa87)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=080fa87)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=080fa87)  
[test/mjsunit/regress/regress-9022.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9022.js?cl=080fa87)  
  

---   

## **regress-9002.js (v8 issue)**  
   
**[Issue 9002:
 Small functions shouldn't be inlined if they are not inlineable](https://crbug.com/v8/9002)**  
**[Commit: [turbofan] Fix wrongly inlined small functions](https://chromium.googlesource.com/v8/v8/+/07a711a)**  
  
Date(Commit): Wed Mar 20 08:46:41 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1528233](https://chromium-review.googlesource.com/c/v8/v8/+/1528233)  
Regress: [mjsunit/regress/regress-9002.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9002.js)  
```javascript
function f() {
  return 42;
}

function g() {
  return 52;
}

%NeverOptimizeFunction(f);

function foo(cond) {
  let func;
  if (cond) {
    func = f;
  } else {
    func = g;
  }
  func();
}

%PrepareFunctionForOptimization(foo);
foo(true);
foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(true);
foo(false);

assertUnoptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/07a711a^!)  
[src/compiler/js-inlining-heuristic.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining-heuristic.cc?cl=07a711a)  
[test/mjsunit/regress/regress-9002.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9002.js?cl=07a711a)  
  

---   

## **regress-crbug-942068.js (chromium issue)**  
   
**[Issue 942068:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/942068)**  
**[Commit: [turbofan] Fix HasProperty for OOB access on polymorphic ICs](https://chromium.googlesource.com/v8/v8/+/1e2aa78)**  
  
Date(Commit): Fri Mar 15 22:09:16 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-CC"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1525876](https://chromium-review.googlesource.com/c/v8/v8/+/1525876)  
Regress: [mjsunit/regress/regress-crbug-942068.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-942068.js)  
```javascript
function foo(index, array) {
  return index in array;
};
%PrepareFunctionForOptimization(foo);
let arr = [];
arr.__proto__ = [0];
assertFalse(foo(0, {}));
assertTrue(foo(0, arr));
assertFalse(foo(0, {}));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(0, arr));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e2aa78^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=1e2aa78)  
[test/mjsunit/regress/regress-crbug-942068.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-942068.js?cl=1e2aa78)  
  

---   

## **regress-940722.js (chromium issue)**  
   
**[Issue 940722:
 DCHECK failure in AllowHeapAllocation::IsAllowed() in heap-inl.h](https://crbug.com/940722)**  
**[Commit: [regexp] Allow heap allocation on stack overflows](https://chromium.googlesource.com/v8/v8/+/0793bb8)**  
  
Date(Commit): Tue Mar 12 15:01:59 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1518174](https://chromium-review.googlesource.com/c/v8/v8/+/1518174)  
Regress: [mjsunit/regress/regress-940722.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-940722.js)  
```javascript
var __v_27278 = "x";
for (var __v_27279 = 0; __v_27279 != 13; __v_27279++) {
  try { __v_27278 += __v_27278; } catch (e) {}
}

try { /(xx|x)*/.exec(__v_27278); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0793bb8^!)  
[src/regexp/interpreter-irregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/interpreter-irregexp.cc?cl=0793bb8)  
[test/mjsunit/regress/regress-940722.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-940722.js?cl=0793bb8)  
  

---   

## **regress-crbug-937618.js (chromium issue)**  
   
**[Issue 937618:
 V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/937618)**  
**[Commit: [proxy] fix has trap check for indices](https://chromium.googlesource.com/v8/v8/+/11d8358)**  
  
Date(Commit): Mon Mar 11 20:53:47 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-CC"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1510333](https://chromium-review.googlesource.com/c/v8/v8/+/1510333)  
Regress: [mjsunit/regress/regress-crbug-937618.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-937618.js)  
```javascript
let target = {0: 42, a: 42};

let proxy = new Proxy(target, {
  has: function() {
    return false;
  }
});

Object.preventExtensions(target);

function testLookupElementInProxy() {
  0 in proxy;
}

;
%PrepareFunctionForOptimization(testLookupElementInProxy);
assertThrows(testLookupElementInProxy, TypeError);
assertThrows(testLookupElementInProxy, TypeError);
%OptimizeFunctionOnNextCall(testLookupElementInProxy);
assertThrows(testLookupElementInProxy, TypeError);

function testLookupPropertyInProxy() {
  "a" in proxy;
};
%PrepareFunctionForOptimization(testLookupPropertyInProxy);
assertThrows(testLookupPropertyInProxy, TypeError);
assertThrows(testLookupPropertyInProxy, TypeError);
%OptimizeFunctionOnNextCall(testLookupPropertyInProxy);
assertThrows(testLookupPropertyInProxy, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11d8358^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=11d8358)  
[test/mjsunit/regress/regress-crbug-937618.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-937618.js?cl=11d8358)  
  

---   

## **regress-937681.js (chromium issue)**  
   
**[Issue 937681:
 CHECK failure: !ResultAreIdentical(args); RegExpBuiltinsFuzzerHash=c087ca96 in regexp-builtins.](https://crbug.com/937681)**  
**[Commit: [regexp] Fix sticky callable replace with OOB lastIndex](https://chromium.googlesource.com/v8/v8/+/dd580e8)**  
  
Date(Commit): Mon Mar 11 16:09:47 2019  
Components/Type: Blink>JavaScript>Regexp/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1514679](https://chromium-review.googlesource.com/c/v8/v8/+/1514679)  
Regress: [mjsunit/regress/regress-937681.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-937681.js)  
```javascript
const str = 'aaaa';

const re0 = /./y;

re0.lastIndex = 9;
assertEquals(str, re0[Symbol.replace](str, () => 42));
re0.lastIndex = 9;
assertEquals(str, re0[Symbol.replace](str, () => 42));
re0.lastIndex = 9;
assertEquals(str, re0[Symbol.replace](str, "42"));
re0.lastIndex = 9;
assertEquals(str, re0[Symbol.replace](str, "42"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dd580e8^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=dd580e8)  
[test/mjsunit/regress/regress-937681.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-937681.js?cl=dd580e8)  
  

---   

## **regress-935092.js (chromium issue)**  
   
**[Issue 935092:
 Security: Debug check failed: op->opcode() == IrOpcode::kDeoptimize || op->opcode() == IrOpcode::kDeoptimizeIf || op->opcode() == IrOpcode::kDeoptimizeUnless](https://crbug.com/935092)**  
**[Commit: [turbofan] Check for dead control in branch elimination.](https://chromium.googlesource.com/v8/v8/+/ac8e98e)**  
  
Date(Commit): Mon Mar 11 06:30:00 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-74"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1488769](https://chromium-review.googlesource.com/c/v8/v8/+/1488769)  
Regress: [mjsunit/compiler/regress-935092.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-935092.js)  
```javascript
function opt(g) {
  for (var X = 0; X < 1; X++) {
    new function() {
      this.y;
    }().x;
    (g || g && (g || -N)(g && 0)).y = 0;
  }
  (function() {
    g;
  });
};
%PrepareFunctionForOptimization(opt);
opt({});
%OptimizeFunctionOnNextCall(opt);
opt({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac8e98e^!)  
[src/compiler/branch-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/branch-elimination.cc?cl=ac8e98e)  
[test/mjsunit/compiler/regress-935092.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-935092.js?cl=ac8e98e)  
  

---   

## **regress-937650.js (chromium issue)**  
   
**[Issue 937650:
 Float-cast-overflow in v8::internal::wasm::AsmJsParser::ValidateModuleVarFromGlobal](https://crbug.com/937650)**  
**[Commit: [asm.js] Fix undefined behavior with float32 constants.](https://chromium.googlesource.com/v8/v8/+/b60d567)**  
  
Date(Commit): Thu Mar 07 08:56:37 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1505458](https://chromium-review.googlesource.com/c/v8/v8/+/1505458)  
Regress: [mjsunit/asm/regress-937650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-937650.js)  
```javascript
function Module(stdlib) {
  "use asm";
  var fround = stdlib.Math.fround;
  // The below constant is outside the range of representable {float} values.
  const infinity = fround(1.7976931348623157e+308);
  function f() {
    return infinity;
  }
  return { f: f };
}

var m = Module(this);
assertEquals(Infinity, m.f());
assertTrue(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b60d567^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=b60d567)  
[test/mjsunit/asm/regress-937650.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-937650.js?cl=b60d567)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=b60d567)  
  

---   

## **regress-ubsan.js (other issue)**  
   
**[Commit: [ubsan] Fix various ClusterFuzz-found issues](https://chromium.googlesource.com/v8/v8/+/91f0cd0)**  
  
Date(Commit): Thu Mar 07 00:09:20 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1495911](https://chromium-review.googlesource.com/c/v8/v8/+/1495911)  
Regress: [mjsunit/regress/wasm/regress-ubsan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-ubsan.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addImportedGlobal("mod", "i32", kWasmI32);
  builder.addImportedGlobal("mod", "f32", kWasmF32);
  var module = new WebAssembly.Module(builder.toBuffer());
  return new WebAssembly.Instance(module, {
    mod: {
      i32: 1e12,
      f32: 1e300,
    }
  });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91f0cd0^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=91f0cd0)  
[src/builtins/builtins-string.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string.cc?cl=91f0cd0)  
[src/builtins/builtins-typed-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array.cc?cl=91f0cd0)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=91f0cd0)  
[src/objects/bigint.cc](https://cs.chromium.org/chromium/src/v8/src/objects/bigint.cc?cl=91f0cd0)  
...  
  
  
---   

## **regress-8947.js (v8 issue)**  
   
**[Issue 8947:
 Calling wasm-re-exported-then-imported API function crashes](https://crbug.com/v8/8947)**  
**[Commit: [wasm] Fix import of reexported API function](https://chromium.googlesource.com/v8/v8/+/15925e5)**  
  
Date(Commit): Tue Mar 05 14:34:57 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1503632](https://chromium-review.googlesource.com/c/v8/v8/+/1503632)  
Regress: [mjsunit/regress/regress-8947.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8947.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function testCallReexportedJSFunc() {
 print(arguments.callee.name);

  function dothrow() {
    throw "exception";
  }

  var builder = new WasmModuleBuilder();
  const imp_index = builder.addImport("w", "m", kSig_i_v);
  builder.addExport("exp", imp_index);
  var exp = builder.instantiate({w: {m: dothrow}}).exports.exp;

  builder.addImport("w", "m", kSig_i_v);
  builder.addFunction("main", kSig_i_v)
    .addBody([
      kExprCallFunction, 0, // --
    ])                             // --
    .exportFunc();

  var main = builder.instantiate({w: {m: exp}}).exports.main;
  assertThrowsEquals(main, "exception");
})();

(function testCallReexportedAPIFunc() {
 print(arguments.callee.name);

  var builder = new WasmModuleBuilder();
  const imp_index = builder.addImport("w", "m", kSig_i_v);
  builder.addExport("exp", imp_index);
  var exp = builder.instantiate({w: {m: WebAssembly.Module}}).exports.exp;

  builder.addImport("w", "m", kSig_i_v);
  builder.addFunction("main", kSig_i_v)
    .addBody([
      kExprCallFunction, 0, // --
    ])                             // --
    .exportFunc();

  var main = builder.instantiate({w: {m: exp}}).exports.main;
  assertThrows(main, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15925e5^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=15925e5)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=15925e5)  
[test/mjsunit/regress/regress-8947.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8947.js?cl=15925e5)  
  

---   

## **regress-crbug-935932.js (chromium issue)**  
   
**[Issue 935932:
 Ill in v8::internal::compiler::JSNativeContextSpecialization::ReduceGlobalAccess](https://crbug.com/935932)**  
**[Commit: Reland "Optimize `in` operator"](https://chromium.googlesource.com/v8/v8/+/803ad32)**  
  
Date(Commit): Fri Mar 01 09:01:18 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/c/1493132](https://chromium-review.googlesource.com/c/1493132)  
Regress: [mjsunit/regress/regress-crbug-935932.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-935932.js)  
```javascript
function test(func, expect) {
    %PrepareFunctionForOptimization(func);
    assertTrue(func() == expect);
    %OptimizeFunctionOnNextCall(func);
    assertTrue(func() == expect);
}

var v0 = 10;
function check_v0() { return "v0" in this; }
test(check_v0, true);

v0 = 0;
test(check_v0, true);

function check_v1() { return "v1" in this; }
test(check_v1, false);
this.v1 = 3;
test(check_v1, true);
delete this.v1;
test(check_v1, false);

var v2;
function check_v2() { return "v2" in this; }
test(check_v2, true);

var v3 = {};
function check_v3() { return "v3" in this; }
test(check_v3, true);
v3 = [];
test(check_v3, true);

Object.defineProperty(this, "v4", { value: {}, configurable: false});
function check_v4() { return "v4" in this; }
test(check_v4, true);

(function() {
  function testIn(index, array) {
    return index in array;
  }
  %PrepareFunctionForOptimization(testIn);

  let a = [];
  a.__proto__ = [0,1,2];
  a[1] = 3;

  // First load will set IC to Load handle with allow hole to undefined conversion false.
  assertTrue(testIn(0, a));
  // Second load will hit ICMiss when hole is loaded. Seeing the same map twice, the IC will be set megamorphic.
  assertTrue(testIn(0, a));
  %OptimizeFunctionOnNextCall(testIn);
  // Test JIT to ensure proper handling.
  assertTrue(testIn(0, a));

  %ClearFunctionFeedback(testIn);
  %DeoptimizeFunction(testIn);
  %PrepareFunctionForOptimization(testIn);

  // First load will set IC to Load handle with allow hole to undefined conversion false.
  assertTrue(testIn(0, a));
  %OptimizeFunctionOnNextCall(testIn);
  // Test JIT to ensure proper handling if hole is loaded.
  assertTrue(testIn(0, a));

  // Repeat the same testing for access out-of-bounds of the array, but in bounds of it's prototype.
  %ClearFunctionFeedback(testIn);
  %DeoptimizeFunction(testIn);
  %PrepareFunctionForOptimization(testIn);

  assertTrue(testIn(2, a));
  assertTrue(testIn(2, a));
  %OptimizeFunctionOnNextCall(testIn);
  assertTrue(testIn(2, a));

  %ClearFunctionFeedback(testIn);
  %DeoptimizeFunction(testIn);
  %PrepareFunctionForOptimization(testIn);

  assertTrue(testIn(2, a));
  %OptimizeFunctionOnNextCall(testIn);
  assertTrue(testIn(2, a));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/803ad32^!)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=803ad32)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=803ad32)  
[src/builtins/builtins-ic-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-ic-gen.cc?cl=803ad32)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=803ad32)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=803ad32)  
...  
  

---   

## **regress-936077.js (chromium issue)**  
   
**[Issue 936077:
 Ill in v8::internal::compiler::AccessInfoFactory::ConsolidateElementLoad](https://crbug.com/936077)**  
**[Commit: [turbofan] Don't assume we have receiver maps in preprocessed feedback](https://chromium.googlesource.com/v8/v8/+/9c5cd06)**  
  
Date(Commit): Wed Feb 27 18:46:20 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1491222](https://chromium-review.googlesource.com/c/1491222)  
Regress: [mjsunit/regress/regress-936077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-936077.js)  
```javascript
function main() {
  var obj = {};
  function foo() {
    return obj[0];
  };
  %PrepareFunctionForOptimization(foo);
  ;
  gc();
  obj.x = 10;
  %OptimizeFunctionOnNextCall(foo);
  foo();
}
main();
main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c5cd06^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=9c5cd06)  
[test/mjsunit/regress/regress-936077.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-936077.js?cl=9c5cd06)  
  

---   

## **regress-8896.js (v8 issue)**  
   
**[Issue 8896:
 Calls to runtime functions in wasm code should be serializable](https://crbug.com/v8/8896)**  
**[Commit: [wasm] Support runtime functions in (de)serializer.](https://chromium.googlesource.com/v8/v8/+/4c60e6b)**  
  
Date(Commit): Wed Feb 27 11:32:42 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1491220](https://chromium-review.googlesource.com/c/1491220)  
Regress: [mjsunit/regress/wasm/regress-8896.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8896.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestSerializeDeserializeRuntimeCall() {
  var builder = new WasmModuleBuilder();
  var except = builder.addException(kSig_v_v);
  builder.addFunction("f", kSig_v_v)
      .addBody([
        kExprThrow, except,
      ]).exportFunc();
  var wire_bytes = builder.toBuffer();
  var module = new WebAssembly.Module(wire_bytes);
  var instance1 = new WebAssembly.Instance(module);
  var serialized = %SerializeWasmModule(module);
  module = %DeserializeWasmModule(serialized, wire_bytes);
  var instance2 = new WebAssembly.Instance(module);
  assertThrows(() => instance2.exports.f(), WebAssembly.RuntimeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c60e6b^!)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=4c60e6b)  
[test/mjsunit/regress/wasm/regress-8896.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-8896.js?cl=4c60e6b)  
  

---   

## **regress-8913.js (v8 issue)**  
   
**[Issue 8913:
 String.prototype.concat deopt loop](https://crbug.com/v8/8913)**  
**[Commit: [turbofan] Properly thread through the feedback for HeapObject checks.](https://chromium.googlesource.com/v8/v8/+/066e2a2)**  
  
Date(Commit): Tue Feb 26 14:19:49 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1488770](https://chromium-review.googlesource.com/c/1488770)  
Regress: [mjsunit/regress/regress-8913.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8913.js)  
```javascript
function foo(t) { return 'a'.concat(t); }

%PrepareFunctionForOptimization(foo);
foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo(1);
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo(1);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/066e2a2^!)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=066e2a2)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=066e2a2)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=066e2a2)  
[test/mjsunit/regress/regress-8913.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8913.js?cl=066e2a2)  
  

---   

## **regress-v8-8799.js (v8 issue)**  
   
**[Issue 8799:
 Bytecode flushing breaks template literals test](https://crbug.com/v8/8799)**  
**[Commit: [Runtime] Ensure template objects are retained if bytecode is flushed.](https://chromium.googlesource.com/v8/v8/+/ec9aef3)**  
  
Date(Commit): Mon Feb 25 11:20:06 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1477746](https://chromium-review.googlesource.com/c/1477746)  
Regress: [mjsunit/regress/regress-v8-8799.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-8799.js)  
```javascript
var f = (x) => eval`a${x}b`;
var a = f();
gc();
assertSame(a, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec9aef3^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=ec9aef3)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=ec9aef3)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=ec9aef3)  
[src/compiler/bytecode-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.h?cl=ec9aef3)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=ec9aef3)  
...  
  

---   

## **regress-crbug-913222.js (chromium issue)**  
   
**[Issue 913222:
 Stack-overflow in v8::internal::PreParser::ParseFunctionLiteral](https://crbug.com/913222)**  
**[Commit: [parser] Fix stackoverflow on function expressions](https://chromium.googlesource.com/v8/v8/+/4b0c2b3)**  
  
Date(Commit): Mon Feb 25 10:44:26 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1480379](https://chromium-review.googlesource.com/c/1480379)  
Regress: [mjsunit/regress/regress-crbug-913222.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-913222.js)  
```javascript
__v_0 = '(function() {\n';
for (var __v_1 = 0; __v_1 < 10000; __v_1++) {
    __v_0 += '  return function() {\n';
}
assertThrows(()=>eval(__v_0), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b0c2b3^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=4b0c2b3)  
[test/mjsunit/regress/regress-crbug-913222.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-913222.js?cl=4b0c2b3)  
  

---   

## **regress-crbug-933214.js (chromium issue)**  
   
**[Issue 933214:
 Null-dereference READ in v8::internal::DeclarationScope::HoistSloppyBlockFunctions](https://crbug.com/933214)**  
**[Commit: [parser] Always return a valid var from DeclareVariableName](https://chromium.googlesource.com/v8/v8/+/e14a24d)**  
  
Date(Commit): Mon Feb 25 10:31:26 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1481219](https://chromium-review.googlesource.com/c/1481219)  
Regress: [mjsunit/regress/regress-crbug-933214.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-933214.js)  
```javascript
assertThrows(`
    function __v_0() {
        function __v_2() {
            try {
                function* __v_0() {}
                function __v_0() {}
            }
        }
    }`, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e14a24d^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=e14a24d)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=e14a24d)  
[test/mjsunit/regress/regress-crbug-933214.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-933214.js?cl=e14a24d)  
  

---   

## **regress-crbug-934138.js (chromium issue)**  
   
**[Issue 934138:
 V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/934138)**  
**[Commit: [asm.js] Fix handling of bogus code after export statement.](https://chromium.googlesource.com/v8/v8/+/cc787e1)**  
  
Date(Commit): Thu Feb 21 14:37:37 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1481216](https://chromium-review.googlesource.com/c/1481216)  
Regress: [mjsunit/regress/regress-crbug-934138.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-934138.js)  
```javascript
(function TestTrailingJunkAfterExport() {
  function Module() {
    "use asm";
    function f() {}
    return {f: f}
    %kaboom;
  }
  assertThrows(() => Module(), ReferenceError);
  assertFalse(%IsAsmWasmCode(Module));
})();

(function TestExportWithSemicolon() {
  function Module() {
    "use asm";
    function f() {}
    return {f: f};
    // appreciate the semicolon
  }
  assertDoesNotThrow(() => Module());
  assertTrue(%IsAsmWasmCode(Module));
})();

(function TestExportWithoutSemicolon() {
  function Module() {
    "use asm";
    function f() {}
    return {f: f}
    // appreciate the nothingness
  }
  assertDoesNotThrow(() => Module());
  assertTrue(%IsAsmWasmCode(Module));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc787e1^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=cc787e1)  
[test/message/asm-function-undefined.out](https://cs.chromium.org/chromium/src/v8/test/message/asm-function-undefined.out?cl=cc787e1)  
[test/message/asm-table-undefined.out](https://cs.chromium.org/chromium/src/v8/test/message/asm-table-undefined.out?cl=cc787e1)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=cc787e1)  
[test/mjsunit/regress/regress-crbug-934138.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-934138.js?cl=cc787e1)  
  

---   

## **regress-933776.js (chromium issue)**  
   
**[Issue 933776:
 DCHECK failure in array_builder_.capacity() > array_builder_.length() in string-builder.cc](https://crbug.com/933776)**  
**[Commit: Remove invalid DCHECK in ReplacementStringBuilder](https://chromium.googlesource.com/v8/v8/+/c54bbd2)**  
  
Date(Commit): Thu Feb 21 09:41:06 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1479955](https://chromium-review.googlesource.com/c/1479955)  
Regress: [mjsunit/regress/regress-933776.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-933776.js)  
```javascript
__v_51351 = /[^]$/gm;
"a\nb\rc\n\rd\r\ne".replace(__v_51351, "*$1");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c54bbd2^!)  
[src/string-builder.cc](https://cs.chromium.org/chromium/src/v8/src/string-builder.cc?cl=c54bbd2)  
[test/mjsunit/regress/regress-933776.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-933776.js?cl=c54bbd2)  
  

---   

## **regress-8846.js (v8 issue)**  
   
**[Issue 8846:
 [wasm] Unordered module sections are not properly accepted during async compilation](https://crbug.com/v8/8846)**  
**[Commit: [wasm] Fix section order checking in {StreamingDecoder}.](https://chromium.googlesource.com/v8/v8/+/d7a5e5b)**  
  
Date(Commit): Tue Feb 19 16:57:23 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1477211](https://chromium-review.googlesource.com/c/1477211)  
Regress: [mjsunit/regress/wasm/regress-8846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8846.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestAsyncCompileExceptionSection() {
  print(arguments.callee.name);
  let builder = new WasmModuleBuilder();
  let except = builder.addException(kSig_v_v);
  builder.addFunction("thrw", kSig_v_v)
      .addBody([
        kExprThrow, except,
      ]).exportFunc();
  function step1(buffer) {
    assertPromiseResult(WebAssembly.compile(buffer), module => step2(module));
  }
  function step2(module) {
    assertPromiseResult(WebAssembly.instantiate(module), inst => step3(inst));
  }
  function step3(instance) {
    assertThrows(() => instance.exports.thrw(), WebAssembly.RuntimeError);
  }
  step1(builder.toBuffer());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d7a5e5b^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=d7a5e5b)  
[src/wasm/streaming-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/streaming-decoder.cc?cl=d7a5e5b)  
[src/wasm/streaming-decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/streaming-decoder.h?cl=d7a5e5b)  
[test/mjsunit/regress/wasm/regress-8846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-8846.js?cl=d7a5e5b)  
[test/unittests/wasm/streaming-decoder-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/wasm/streaming-decoder-unittest.cc?cl=d7a5e5b)  
  

---   

## **regress-crbug-931664.js (chromium issue)**  
   
**[Issue 931664:
 Security: v8: unreachable code in OddballToNumber](https://crbug.com/931664)**  
**[Commit: [turbofan] Handle all oddballs in OddballToNumber](https://chromium.googlesource.com/v8/v8/+/68ed2f1)**  
  
Date(Commit): Mon Feb 18 10:46:37 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://chromium-review.googlesource.com/c/1477050](https://chromium-review.googlesource.com/c/1477050)  
Regress: [mjsunit/regress/regress-crbug-931664.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-931664.js)  
```javascript
function opt(){
  for(l in('a')){
    try{
      for(a in('')) {
        for(let arg2 in(+(arg2)));
      }
    }
    finally{}
  }
}
%PrepareFunctionForOptimization(opt);
opt();
%OptimizeFunctionOnNextCall(opt);
opt();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68ed2f1^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=68ed2f1)  
[src/compiler/js-heap-broker.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.h?cl=68ed2f1)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=68ed2f1)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=68ed2f1)  
[test/mjsunit/regress/regress-crbug-931664.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-931664.js?cl=68ed2f1)  
  

---   

## **regress-8808.js (v8 issue)**  
   
**[Issue 8808:
 Segmentation fault when trying to declare a class with destructuring private class field](https://crbug.com/v8/8808)**  
**[Commit: [parser] don't accept PRIVATE_NAME for object literal property names](https://chromium.googlesource.com/v8/v8/+/1483561)**  
  
Date(Commit): Mon Feb 11 18:17:32 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1461161](https://chromium-review.googlesource.com/c/1461161)  
Regress: [mjsunit/harmony/regress/regress-8808.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-8808.js)  
```javascript
assertThrows(() => eval(`
  class Foo {
    #x = 1;
    destructureX() {
      const { #x: x } = this;
      return x;
    }
  }
`), SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1483561^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=1483561)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=1483561)  
[test/message/fail/destructuring-object-private-name.js](https://cs.chromium.org/chromium/src/v8/test/message/fail/destructuring-object-private-name.js?cl=1483561)  
[test/message/fail/destructuring-object-private-name.out](https://cs.chromium.org/chromium/src/v8/test/message/fail/destructuring-object-private-name.out?cl=1483561)  
[test/mjsunit/harmony/regress/regress-8808.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-8808.js?cl=1483561)  
  

---   

## **regress-930486.js (chromium issue)**  
   
**[Issue 930486:
 Ill in v8::internal::Map::EquivalentToForTransition](https://crbug.com/930486)**  
**[Commit: Fix map equivalence check.](https://chromium.googlesource.com/v8/v8/+/a953f8d)**  
  
Date(Commit): Mon Feb 11 16:31:35 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1462004](https://chromium-review.googlesource.com/c/1462004)  
Regress: [mjsunit/regress/regress-930486.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-930486.js)  
```javascript
var __v_49026 = function () {};

__v_49026.prototype = undefined;
__v_49026.x = 23;
__v_49026.prototype = new ArrayBuffer();
__v_49026.x = 2147483649;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a953f8d^!)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=a953f8d)  
[test/mjsunit/regress/regress-930486.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-930486.js?cl=a953f8d)  
  

---   

## **regress-912162.js (chromium issue)**  
   
**[Issue 912162:
 Ill in v8::internal::JSObject::JSObjectVerify](https://crbug.com/912162)**  
**[Commit: Constant field tracking for arrays.](https://chromium.googlesource.com/v8/v8/+/ea86509)**  
  
Date(Commit): Wed Feb 06 14:44:43 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/c/1454606](https://chromium-review.googlesource.com/c/1454606)  
Regress: [mjsunit/regress/regress-912162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-912162.js)  
```javascript
var a = new Array();
a.prototype = a;

function f() {
 a.length = 0x2000001;
 a.push();
}

({}).__proto__ = a;

f()
f()

a.length = 1;
a.fill(-255);

%HeapObjectVerify(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea86509^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=ea86509)  
[src/compiler/compilation-dependencies.h](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.h?cl=ea86509)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=ea86509)  
[src/compiler/property-access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/property-access-builder.cc?cl=ea86509)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=ea86509)  
...  
  

---   

## **regress-v8-5848.js (v8 issue)**  
   
**[Issue 5848:
 Exponentiation operator ** has different results for numbers and variables from 50 upwards](https://crbug.com/v8/5848)**  
**[Commit: [builtins] [turbofan] Refactor Float64Pow to use single implementation](https://chromium.googlesource.com/v8/v8/+/595aafe)**  
  
Date(Commit): Thu Jan 31 09:42:25 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1403018](https://chromium-review.googlesource.com/c/1403018)  
Regress: [mjsunit/regress/regress-v8-5848.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5848.js)  
```javascript
const inlineFromParser = 50 ** 50;

const i = 50;
const fromRuntimePowOp = i ** i;
const fromRuntimeMath = Math.pow(i, i);


assertEquals(inlineFromParser, fromRuntimePowOp);
assertEquals(inlineFromParser - fromRuntimePowOp, 0);

assertEquals(inlineFromParser, fromRuntimeMath);
assertEquals(inlineFromParser - fromRuntimeMath, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/595aafe^!)  
[src/base/ieee754.cc](https://cs.chromium.org/chromium/src/v8/src/base/ieee754.cc?cl=595aafe)  
[src/base/ieee754.h](https://cs.chromium.org/chromium/src/v8/src/base/ieee754.h?cl=595aafe)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=595aafe)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=595aafe)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=595aafe)  
...  
  

---   

## **regress-crbug-926819.js (chromium issue)**  
   
**[Issue 926819:
 Null-dereference READ in v8::internal::DeclarationScope::HoistSloppyBlockFunctions](https://crbug.com/926819)**  
**[Commit: [parser] Don't hoist sloppy block functions on error](https://chromium.googlesource.com/v8/v8/+/3ef9af8)**  
  
Date(Commit): Wed Jan 30 11:54:28 2019  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/c/1445891](https://chromium-review.googlesource.com/c/1445891)  
Regress: [mjsunit/regress/regress-crbug-926819.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-926819.js)  
```javascript
assertThrows("a(function(){{let f;function f}})", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ef9af8^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=3ef9af8)  
[test/mjsunit/regress/regress-crbug-926819.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-926819.js?cl=3ef9af8)  
  

---   

## **regress-924151.js (chromium issue)**  
   
**[Issue 924151:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/924151)**  
**[Commit: [turbofan] Handle exceptional edges when inserting unreachable node.](https://chromium.googlesource.com/v8/v8/+/ec4d45a)**  
  
Date(Commit): Thu Jan 24 12:43:46 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1429860](https://chromium-review.googlesource.com/c/1429860)  
Regress: [mjsunit/compiler/regress-924151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-924151.js)  
```javascript
function g(code) {
  try {
    if (typeof code === 'function') {
      +Symbol();
    } else {
      eval();
    }
  } catch (e) {
    return;
  }
  dummy();
}

function f() {
  g(g);
}

try { g(); } catch(e) {; }

%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec4d45a^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=ec4d45a)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=ec4d45a)  
[test/mjsunit/compiler/regress-924151.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-924151.js?cl=ec4d45a)  
  

---   

## **regress-6711.js (v8 issue)**  
   
**[Issue 6711:
 Applying delete operator on non-initialized |this| should throw ReferenceError](https://crbug.com/v8/6711)**  
**[Commit: Check for "SuperNotCalled" on "delete this" in a constructor](https://chromium.googlesource.com/v8/v8/+/1e5b235)**  
  
Date(Commit): Tue Jan 22 18:58:42 2019  
Type: Bug  
Code Review: [https://bugs.chromium.org/p/v8/issues/detail?id=6711](https://bugs.chromium.org/p/v8/issues/detail?id=6711)  
Regress: [mjsunit/regress/regress-6711.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6711.js)  
```javascript
assertThrows(()=>{
  new class extends Object {
    constructor() {
      delete this;
      super();
    }
  }
}, ReferenceError);

new class extends Object {
  constructor() {
    super();
    delete this;
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e5b235^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=1e5b235)  
[test/cctest/interpreter/bytecode_expectations/Delete.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Delete.golden?cl=1e5b235)  
[test/cctest/interpreter/test-bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/test-bytecode-generator.cc?cl=1e5b235)  
[test/mjsunit/regress/regress-6711.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6711.js?cl=1e5b235)  
  

---   

## **regress-crbug-923705.js (chromium issue)**  
   
**[Issue 923705:
 Ill in v8::internal::BaseConsumedPreparseData<v8::internal::PreparseData>::GetDataForSk](https://crbug.com/923705)**  
**[Commit: [parser] Fix storing has_data bit for inner function preparse data](https://chromium.googlesource.com/v8/v8/+/c3722aa)**  
  
Date(Commit): Mon Jan 21 18:04:34 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1425201](https://chromium-review.googlesource.com/c/1425201)  
Regress: [mjsunit/regress/regress-crbug-923705.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-923705.js)  
```javascript
function __f_5() {
  function __f_1() {
    function __f_0() {
      ({y = eval()}) => assertEquals()();
    }
  }
}

__f_5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3722aa^!)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=c3722aa)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=c3722aa)  
[src/parsing/preparse-data-impl.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparse-data-impl.h?cl=c3722aa)  
[src/parsing/preparse-data.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparse-data.cc?cl=c3722aa)  
[test/mjsunit/regress/regress-crbug-923705.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-923705.js?cl=c3722aa)  
  

---   

## **regress-922670.js (chromium issue)**  
   
**[Issue 922670:
 Fatal error in liftoff-register.h](https://crbug.com/922670)**  
**[Commit: [Liftoff] Fix DCHECK error](https://chromium.googlesource.com/v8/v8/+/f77299e)**  
  
Date(Commit): Mon Jan 21 11:52:17 2019  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Clusterfuzz", "Unreproducible"]  
Code Review: [https://chromium-review.googlesource.com/c/1421921](https://chromium-review.googlesource.com/c/1421921)  
Regress: [mjsunit/regress/wasm/regress-922670.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-922670.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI32], []));
builder.addFunction(undefined, sig)
  .addLocals({i64_count: 1})
  .addBody([
    kExprLoop, kWasmI32,
      kExprGetLocal, 1,
      kExprI64Const, 1,
      kExprLoop, kWasmI32,
        kExprGetLocal, 0,
        kExprI32Const, 1,
        kExprI32Const, 1,
        kExprIf, kWasmI32,
          kExprI32Const, 1,
        kExprElse,
          kExprUnreachable,
          kExprEnd,
        kExprSelect,
        kExprUnreachable,
        kExprEnd,
      kExprUnreachable,
      kExprEnd,
    kExprUnreachable
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f77299e^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=f77299e)  
[test/mjsunit/regress/wasm/regress-922670.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-922670.js?cl=f77299e)  
  

---   

## **regress-8708.js (v8 issue)**  
   
**[Issue 8708:
 Uncaught stack overflow in Array.prototype.flat](https://crbug.com/v8/8708)**  
**[Commit: [array] Add stack overflow check for Array#flat](https://chromium.googlesource.com/v8/v8/+/bf17cd2)**  
  
Date(Commit): Mon Jan 21 10:39:45 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1424858](https://chromium-review.googlesource.com/c/1424858)  
Regress: [mjsunit/regress/regress-8708.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8708.js)  
```javascript
let array = new Array(1);
array.splice(1, 0, array);

assertThrows(() => array.flat(Infinity), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf17cd2^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=bf17cd2)  
[test/mjsunit/regress/regress-8708.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8708.js?cl=bf17cd2)  
  

---   

## **regress-923723.js (chromium issue)**  
   
**[Issue 923723:
 Null-dereference READ in v8::internal::ParserBase<v8::internal::Parser>::ParseArrowFunctionLiteral](https://crbug.com/923723)**  
**[Commit: [parser] Reparsing arrow function head upon failure can overflow the stack](https://chromium.googlesource.com/v8/v8/+/b4e7d11)**  
  
Date(Commit): Mon Jan 21 10:12:10 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1424857](https://chromium-review.googlesource.com/c/1424857)  
Regress: [mjsunit/regress/regress-923723.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-923723.js)  
```javascript
function __f_3() {
  try {
    __f_3();
  } catch(e) {
    eval("let fun = ({a} = {a: 30}) => {");
  }
}
assertThrows(__f_3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4e7d11^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=b4e7d11)  
[test/mjsunit/regress/regress-923723.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-923723.js?cl=b4e7d11)  
  

---   

## **regress-crbug-923265.js (chromium issue)**  
   
**[Issue 923265:
 Ill in v8::internal::RemoveArrayHolesGeneric](https://crbug.com/923265)**  
**[Commit: [array] Remove CHECK_LE from RemoveArrayHolesGeneric](https://chromium.googlesource.com/v8/v8/+/e38faab)**  
  
Date(Commit): Fri Jan 18 10:01:37 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1420679](https://chromium-review.googlesource.com/c/1420679)  
Regress: [mjsunit/regress/regress-crbug-923265.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-923265.js)  
```javascript
let a = {0: 5, 1: 4, 2: 3, length: 2};
Object.freeze(a);

assertThrows(() => Array.prototype.sort.call(a));
assertPropertiesEqual({0: 5, 1: 4, 2: 3, length: 2}, a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e38faab^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=e38faab)  
[test/mjsunit/regress/regress-crbug-923265.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-923265.js?cl=e38faab)  
  

---   

## **regress-sloppy-block-function-hoisting-dynamic.js (other issue)**  
   
**[Commit: [parser] Use cached kDynamic variable for eval-introduced vars](https://chromium.googlesource.com/v8/v8/+/f2303d9)**  
  
Date(Commit): Wed Jan 16 14:18:33 2019  
Code Review: [https://chromium-review.googlesource.com/c/1408890](https://chromium-review.googlesource.com/c/1408890)  
Regress: [mjsunit/regress/regress-sloppy-block-function-hoisting-dynamic.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sloppy-block-function-hoisting-dynamic.js)  
```javascript
with({}) { eval("{function f(){f}}") };
f()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f2303d9^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=f2303d9)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=f2303d9)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=f2303d9)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=f2303d9)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=f2303d9)  
...  
  
  
---   

## **regress-917215.js (chromium issue)**  
   
**[Issue 917215:
 False SyntaxError on Sibling JS-Labels](https://crbug.com/917215)**  
**[Commit: [parser] Allow same-named labelled blocks in if/else statements](https://chromium.googlesource.com/v8/v8/+/469754d)**  
  
Date(Commit): Fri Jan 11 17:40:18 2019  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Arch-x86_64", "Via-Wizard-Javascript", "Triaged-ET", "M-73", "Target-73", "FoundIn-71", "FoundIn-72", "FoundIn-73", "Needs-Triage-M71"]  
Code Review: [https://chromium-review.googlesource.com/c/1404176](https://chromium-review.googlesource.com/c/1404176)  
Regress: [mjsunit/regress/regress-917215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-917215.js)  
```javascript
a: if (true) b: { break a; break b; }
else b: { break a; break b; }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/469754d^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=469754d)  
[test/mjsunit/regress/regress-917215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-917215.js?cl=469754d)  
  

---   

## **regress-8630.js (v8 issue)**  
   
**[Issue 8630:
 Dcheck on jsfunfuzz](https://crbug.com/v8/8630)**  
**[Commit: [parser] Check assignment LHS for paren errors](https://chromium.googlesource.com/v8/v8/+/df6f5f6)**  
  
Date(Commit): Fri Jan 11 12:56:38 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1404452](https://chromium-review.googlesource.com/c/1404452)  
Regress: [mjsunit/regress/regress-8630.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8630.js)  
```javascript
assertThrows("( ({x: 1}) ) => {};", SyntaxError);
assertThrows("( (x) ) => {}", SyntaxError);
assertThrows("( ({x: 1}) = y ) => {}", ReferenceError);
assertThrows("( (x) = y ) => {}", SyntaxError);

assertThrows("let [({x: 1})] = [];", SyntaxError);
assertThrows("let [(x)] = [];", SyntaxError);
assertThrows("let [({x: 1}) = y] = [];", SyntaxError);
assertThrows("let [(x) = y] = [];", SyntaxError);
assertThrows("var [({x: 1})] = [];", SyntaxError);
assertThrows("var [(x)] = [];", SyntaxError);
assertThrows("var [({x: 1}) = y] = [];", SyntaxError);
assertThrows("var [(x) = y] = [];", SyntaxError);

assertThrows("[({x: 1}) = y] = [];", ReferenceError);
assertThrows("({a,b}) = {a:2,b:3}", ReferenceError);

var x;
[(x)] = [2];
assertEquals(x, 2);
[(x) = 3] = [];
assertEquals(x, 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df6f5f6^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=df6f5f6)  
[test/mjsunit/regress/regress-8630.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8630.js?cl=df6f5f6)  
  

---   

## **regress-919308.js (chromium issue)**  
   
**[Issue 919308:
 Ill in v8::internal::wasm::fuzzer::WasmExecutionFuzzer::FuzzWasmModule](https://crbug.com/919308)**  
**[Commit: [Liftoff] Fix sub of the same register](https://chromium.googlesource.com/v8/v8/+/8518d12)**  
  
Date(Commit): Fri Jan 11 10:57:09 2019  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Stability-AFL", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1405029](https://chromium-review.googlesource.com/c/1405029)  
Regress: [mjsunit/regress/wasm/regress-919308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919308.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_i)
  .addLocals({i32_count: 5})
  .addBody([
    kExprGetLocal, 0,    // --> 1
    kExprIf, kWasmI32,
      kExprGetLocal, 0,  // --> 1
    kExprElse,
      kExprUnreachable,
      kExprEnd,
    kExprIf, kWasmI32,
      kExprGetLocal, 0,  // --> 1
    kExprElse,
      kExprUnreachable,
      kExprEnd,
    kExprIf, kWasmI32,
      kExprI32Const, 0,
      kExprGetLocal, 0,
      kExprI32Sub,       // --> -1
      kExprGetLocal, 0,
      kExprGetLocal, 0,
      kExprI32Sub,       // --> 0
      kExprI32Sub,       // --> -1
    kExprElse,
      kExprUnreachable,
      kExprEnd
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(-1, instance.exports.main(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8518d12^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=8518d12)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=8518d12)  
[test/mjsunit/regress/wasm/regress-919308.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-919308.js?cl=8518d12)  
  

---   

## **regress-crbug-920184.js (chromium issue)**  
   
**[Issue 920184:
 Ill in v8::internal::JSObject::JSObjectVerify](https://crbug.com/920184)**  
**[Commit: [Builtins] Array.prototype.filter species creation error](https://chromium.googlesource.com/v8/v8/+/72d8307)**  
  
Date(Commit): Thu Jan 10 18:09:36 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/c/1405035](https://chromium-review.googlesource.com/c/1405035)  
Regress: [mjsunit/regress/regress-crbug-920184.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-920184.js)  
```javascript
var Ctor = function() {
  return [];
};
var a = ["one", "two", "three"];
a.constructor = {};
a.constructor[Symbol.species] = Ctor;

a.filter(function() { return true; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/72d8307^!)  
[src/builtins/array-filter.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-filter.tq?cl=72d8307)  
[test/mjsunit/regress/regress-crbug-920184.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-920184.js?cl=72d8307)  
  

---   

## **regress-loop-var-assign-without-block-scope.js (other issue)**  
   
**[Commit: [parser] Change and fix how we MarkLoopVariableAsAssigned](https://chromium.googlesource.com/v8/v8/+/6c2cc58)**  
  
Date(Commit): Thu Jan 10 15:56:49 2019  
Code Review: [https://chromium-review.googlesource.com/c/1405037](https://chromium-review.googlesource.com/c/1405037)  
Regress: [mjsunit/regress/regress-loop-var-assign-without-block-scope.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-loop-var-assign-without-block-scope.js)  
```javascript
function f() {
  // Loop with a body that's not wrapped in a block.
  for (i = 0; i < 2; i++)
    var x = i, // var x that's assigned on each iteration
        y = y||(()=>x), // single arrow function that returns x
        z = (%OptimizeFunctionOnNextCall(y), y()); // optimize y on first iteration
  return y()
};
assertEquals(1, f())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c2cc58^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=6c2cc58)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=6c2cc58)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=6c2cc58)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=6c2cc58)  
[test/mjsunit/regress/regress-loop-var-assign-without-block-scope.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-loop-var-assign-without-block-scope.js?cl=6c2cc58)  
  
  
---   

## **regress-913844.js (chromium issue)**  
   
**[Issue 913844:
 DCHECK failure in block->predecessors().empty() || block->successors().empty() in unwinding-info-w](https://crbug.com/913844)**  
**[Commit: Remove invalid DCHECKS in unwinding-info-writer](https://chromium.googlesource.com/v8/v8/+/49a526a)**  
  
Date(Commit): Wed Jan 09 15:52:08 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/c/1373770](https://chromium-review.googlesource.com/c/1373770)  
Regress: [mjsunit/regress/regress-913844.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-913844.js)  
```javascript
for (var x = 0; x < 1000000; x++)
;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49a526a^!)  
[src/compiler/backend/arm/unwinding-info-writer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm/unwinding-info-writer-arm.cc?cl=49a526a)  
[src/compiler/backend/arm64/unwinding-info-writer-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm64/unwinding-info-writer-arm64.cc?cl=49a526a)  
[src/compiler/backend/x64/unwinding-info-writer-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/x64/unwinding-info-writer-x64.cc?cl=49a526a)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=49a526a)  
[test/mjsunit/regress/regress-913844.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-913844.js?cl=49a526a)  
  

---   

## **regress-8659.js (v8 issue)**  
   
**[Issue 8659:
 Dcheck on jsfunfuzz](https://crbug.com/v8/8659)**  
**[Commit: [parser] Parenthesized identifiers are invalid as part of a declaration](https://chromium.googlesource.com/v8/v8/+/5b4d4c2)**  
  
Date(Commit): Wed Jan 09 11:02:55 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1402547](https://chromium-review.googlesource.com/c/1402547)  
Regress: [mjsunit/regress/regress-8659.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8659.js)  
```javascript
assertThrows("const [(x)] = []", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5b4d4c2^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=5b4d4c2)  
[test/mjsunit/regress/regress-8659.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8659.js?cl=5b4d4c2)  
  

---   

## **regress-919710.js (chromium issue)**  
   
**[Issue 919710:
 Null-dereference READ in v8::internal::Scope::zone](https://crbug.com/919710)**  
**[Commit: [parser] Reparse arrow functions with unidentified syntax errors in the correct scope](https://chromium.googlesource.com/v8/v8/+/7c3595e)**  
  
Date(Commit): Tue Jan 08 14:46:07 2019  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "ClusterFuzz-Auto-CC"]  
Code Review: [https://chromium-review.googlesource.com/c/1400419](https://chromium-review.googlesource.com/c/1400419)  
Regress: [mjsunit/regress/regress-919710.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-919710.js)  
```javascript
assertThrows("( let ) => { 'use strict';  let }", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c3595e^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=7c3595e)  
[test/mjsunit/regress/regress-919710.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-919710.js?cl=7c3595e)  
  

---   

## **regress-918763.js (chromium issue)**  
   
**[Issue 918763:
 Null-dereference READ in Builtins_ArgumentsAdaptorTrampoline](https://crbug.com/918763)**  
**[Commit: [turbofan] Add missing heap object check](https://chromium.googlesource.com/v8/v8/+/426312c)**  
  
Date(Commit): Mon Jan 07 14:38:50 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1397707](https://chromium-review.googlesource.com/c/1397707)  
Regress: [mjsunit/regress-918763.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-918763.js)  
```javascript
function C() {}
C.__proto__ = null;

function f(c) { return 0 instanceof c; }

%PrepareFunctionForOptimization(f);
f(C);
%OptimizeFunctionOnNextCall(f);
assertThrows(() => f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/426312c^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=426312c)  
[test/mjsunit/regress-918763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-918763.js?cl=426312c)  
  

---   

## **regress-crbug-917076.js (chromium issue)**  
   
**[Issue 917076:
 V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/917076)**  
**[Commit: [async] The Promise.all() fast-path must check @@species protector.](https://chromium.googlesource.com/v8/v8/+/b6bcf32)**  
  
Date(Commit): Mon Jan 07 08:22:56 2019  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1396426](https://chromium-review.googlesource.com/c/1396426)  
Regress: [mjsunit/regress/regress-crbug-917076.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-917076.js)  
```javascript
let speciesCounter = 0;

Object.defineProperty(Promise, Symbol.species, {
  value: function(...args) {
    speciesCounter++;
    return new Promise(...args);
  }
});

async function foo() { }

assertPromiseResult(Promise.all([foo()]), () => {
  assertEquals(3, speciesCounter);
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b6bcf32^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=b6bcf32)  
[test/mjsunit/regress/regress-crbug-917076.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-917076.js?cl=b6bcf32)  
  

---   

## **regress-918149.js (chromium issue)**  
   
**[Issue 918149:
 DCHECK failure in src.is_reg_only() implies src.reg().is_byte_register() in assembler-ia32.cc](https://crbug.com/918149)**  
**[Commit: [Liftoff][ia32] Fix i64 sign extension on non-byte register](https://chromium.googlesource.com/v8/v8/+/5ed7dff)**  
  
Date(Commit): Fri Jan 04 10:12:06 2019  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "M-73", "Target-73", "Release-0-M73"]  
Code Review: [https://chromium-review.googlesource.com/c/1394555](https://chromium-review.googlesource.com/c/1394555)  
Regress: [mjsunit/regress/wasm/regress-918149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918149.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig =
    builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI64]));
builder.addFunction('main', sig).addBody([kExprI64Const, 1, kExprI64SExtendI8]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ed7dff^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=5ed7dff)  
[test/mjsunit/regress/wasm/regress-918149.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-918149.js?cl=5ed7dff)  
  

---   

## **regress-917412.js (chromium issue)**  
   
**[Issue 917412:
 DCHECK failure in !move_dst_regs_.has(dst) in liftoff-assembler.cc](https://crbug.com/917412)**  
**[Commit: [Liftoff] Keep consistent register mapping in non-merged regions](https://chromium.googlesource.com/v8/v8/+/20b6330)**  
  
Date(Commit): Thu Jan 03 14:37:48 2019  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner", "M-73"]  
Code Review: [https://chromium-review.googlesource.com/c/1392190](https://chromium-review.googlesource.com/c/1392190)  
Regress: [mjsunit/regress/wasm/regress-917412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917412.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI32, kWasmI64], []));
builder.addFunction(undefined, sig)
  .addBody([
kExprI32Const, 0,
kExprIf, kWasmI32,
  kExprI32Const, 0,
kExprElse,
  kExprI32Const, 1,
  kExprEnd,
kExprTeeLocal, 0,
kExprGetLocal, 0,
kExprLoop, kWasmStmt,
  kExprI64Const, 0x80, 0x80, 0x80, 0x70,
  kExprSetLocal, 0x01,
  kExprI32Const, 0x00,
  kExprIf, kWasmI32,
    kExprI32Const, 0x00,
  kExprElse,
    kExprI32Const, 0x00,
    kExprEnd,
  kExprBrIf, 0x00,
  kExprUnreachable,
  kExprEnd,
kExprUnreachable,
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/20b6330^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=20b6330)  
[src/wasm/baseline/liftoff-register.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-register.h?cl=20b6330)  
[test/mjsunit/regress/wasm/regress-917412.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-917412.js?cl=20b6330)  
  

---   

## **regress-917988.js (chromium issue)**  
   
**[Issue 917988:
 DCHECK failure in outer_scope_ == scope->outer_scope() in bytecode-generator.cc](https://crbug.com/917988)**  
**[Commit: Set the correct scope when initializing parameters.](https://chromium.googlesource.com/v8/v8/+/fa844bd)**  
  
Date(Commit): Thu Jan 03 10:18:11 2019  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1392240](https://chromium-review.googlesource.com/c/1392240)  
Regress: [mjsunit/regress/regress-917988.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-917988.js)  
```javascript
function v_2(
v_3 = class v_4 {
    get [[] = ';']() { }
}
) { }
v_2();

(function f(
v_3 = class v_4 {
    get [{} = ';']() { }
}
) { })();

(function f( {p, q} = class C { get [[] = ';']() {} } ) {})();

class C {};
C[Symbol.iterator] = function() {
  return {
    next: function() { return { done: true }; },
    _first: true
  };
};
(function f1([p, q] = class D extends C { get [[]]() {} }) { })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa844bd^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=fa844bd)  
[test/mjsunit/regress/regress-917988.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-917988.js?cl=fa844bd)  
  

---   
