# V8Harvest  
The Harvest of V8 regress in 2019.  
  

## **regress-crbug-1017159.js (chromium issue)**  
   
**[Issue: Fatal error in ](https://crbug.com/1017159)**  
**[Commit: [Turbofan]: Fix error in serializer try ranges with generators](https://chromium.googlesource.com/v8/v8/+/d5dd2e6)**  
  
Date(Commit): Wed Nov 13 09:28:17 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1879247](https://chromium-review.googlesource.com/c/v8/v8/+/1879247)  
Regress: [mjsunit/regress/regress-crbug-1017159.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1017159.js)  
```javascript
function* foo() {
  __v_1 = foo.x;   // LdaNamedProperty
  for (;;) {
    try {
      yield;
      try {
        __v_0 == "object";
        __v_1[__getRandomProperty()]();
      } catch(e) {
        print();
      }
    } catch(e) {
      "Caught: " + e;
    }
    break;
  }
};

%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5dd2e6^!)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=d5dd2e6)  
[test/mjsunit/regress/regress-crbug-1017159.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1017159.js?cl=d5dd2e6)  
  

---   

## **regress-1020031.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1020031)**  
**[Commit: [interpreter] Move function-entry stack check to start of bytecode array](https://chromium.googlesource.com/v8/v8/+/cebfde6)**  
  
Date(Commit): Mon Nov 11 15:00:09 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1903440](https://chromium-review.googlesource.com/c/v8/v8/+/1903440)  
Regress: [mjsunit/regress/regress-1020031.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1020031.js)  
```javascript
function* f() {
  try {
    g();
  } catch {}
}

function g() {
  try {
    for (var i of f());
  } catch {
    gc();
  }
}

%PrepareFunctionForOptimization(g);
g();
g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cebfde6^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=cebfde6)  
[test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden?cl=cebfde6)  
[test/cctest/interpreter/bytecode_expectations/AsyncModules.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AsyncModules.golden?cl=cebfde6)  
[test/cctest/interpreter/bytecode_expectations/BreakableBlocks.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/BreakableBlocks.golden?cl=cebfde6)  
[test/cctest/interpreter/bytecode_expectations/CallLookupSlot.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/CallLookupSlot.golden?cl=cebfde6)  
...  
  

---   

## **regress-1021712.js (chromium issue)**  
   
**[Issue: CHECK failure: arguments.size() >= 1 in serializer-for-background-compilation.cc](https://crbug.com/1021712)**  
**[Commit: Fixes argument CHECKs in serializer that are too strict](https://chromium.googlesource.com/v8/v8/+/0fc1f3a)**  
  
Date(Commit): Thu Nov 07 16:51:16 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, M-80, Clusterfuzz, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1903442](https://chromium-review.googlesource.com/c/v8/v8/+/1903442)  
Regress: [mjsunit/regress/regress-1021712.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1021712.js)  
```javascript
function f() {
  Promise.prototype.then.call()
}


%PrepareFunctionForOptimization(f);
try {
  f();
} catch (e) {}
%OptimizeFunctionOnNextCall(f);
assertThrows(() => f(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0fc1f3a^!)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=0fc1f3a)  
[test/mjsunit/regress/regress-1021712.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1021712.js?cl=0fc1f3a)  
  

---   

## **regress-1016703.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1016703)**  
**[Commit: [heap]: Make addition of detached contexts robust for GC](https://chromium.googlesource.com/v8/v8/+/b33a850)**  
  
Date(Commit): Wed Nov 06 17:59:21 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1895573](https://chromium-review.googlesource.com/c/v8/v8/+/1895573)  
Regress: [mjsunit/regress/regress-1016703.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1016703.js)  
```javascript
let realms = [];
for (let i = 0; i < 4; i++) {
  realms.push(Realm.createAllowCrossRealmAccess());
}

for (let i = 0; i < 4; i++) {
  Realm.detachGlobal(realms[i]);
  gc();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b33a850^!)  
[src/execution/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.cc?cl=b33a850)  
[src/objects/fixed-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/fixed-array.h?cl=b33a850)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=b33a850)  
[test/mjsunit/regress/regress-1016703.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1016703.js?cl=b33a850)  
  

---   

## **regress-crbug-1020983.js (chromium issue)**  
   
**[Issue: Security: unreachable code in v8::internal::compiler::BytecodeGraphBuilder::TryGetScopeInfo](https://crbug.com/1020983)**  
**[Commit: [compiler] Fallback to slow path for any unexpected opcode in TryGetScopeInfo](https://chromium.googlesource.com/v8/v8/+/8534e52)**  
  
Date(Commit): Wed Nov 06 09:31:24 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1899989](https://chromium-review.googlesource.com/c/v8/v8/+/1899989)  
Regress: [mjsunit/regress/regress-crbug-1020983.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1020983.js)  
```javascript
function opt() {
    for (let i = 0; i < 5000; i++) {
        try {
                    try {
                        throw 1
                    } finally {
                        E
                    }
        } catch {}
    }
eval()
}
    opt()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8534e52^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=8534e52)  
[test/mjsunit/regress/regress-crbug-1020983.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1020983.js?cl=8534e52)  
  

---   

## **regress-1016450.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1016450)**  
**[Commit: Regression test for word64-lowered BigInt accumulator](https://chromium.googlesource.com/v8/v8/+/ab9cd1a)**  
  
Date(Commit): Mon Nov 04 14:04:22 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1873692](https://chromium-review.googlesource.com/c/v8/v8/+/1873692)  
Regress: [mjsunit/regress/regress-1016450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1016450.js)  
```javascript
let g = 0;

function f(x) {
  let y = BigInt.asUintN(64, 15n);
  // Introduce a side effect to force the construction of a FrameState that
  // captures the value of y.
  g = 42;
  try {
    return x + y;
  } catch(_) {
    return y;
  }
}


%PrepareFunctionForOptimization(f);
assertEquals(16n, f(1n));
assertEquals(17n, f(2n));
%OptimizeFunctionOnNextCall(f);
assertEquals(16n, f(1n));
assertOptimized(f);
assertEquals(15n, f(0));
assertUnoptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ab9cd1a^!)  
[test/mjsunit/regress/regress-1016450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1016450.js?cl=ab9cd1a)  
  

---   

## **regress-1018871.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1018871)**  
**[Commit: [runtime] Handle when JSProxy::HasProperty returns Nothing](https://chromium.googlesource.com/v8/v8/+/9cba7a8)**  
  
Date(Commit): Thu Oct 31 09:57:06 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1886919](https://chromium-review.googlesource.com/c/v8/v8/+/1886919)  
Regress: [mjsunit/regress/regress-1018871.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1018871.js)  
```javascript
var v_this = this;
function f() {
  var obj = {y: 0};
  var proxy = new Proxy(obj, {
    has() { y; },
  });
  Object.setPrototypeOf(v_this, proxy);
  x;
}
assertThrows(f, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9cba7a8^!)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=9cba7a8)  
[test/mjsunit/regress/regress-1018871.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1018871.js?cl=9cba7a8)  
  

---   

## **regress-v8-9534.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/9534)**  
**[Commit: Reland "[compiler] Optionally apply an offset to stack checks"](https://chromium.googlesource.com/v8/v8/+/b875f46)**  
  
Date(Commit): Wed Oct 30 10:23:05 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1762521](https://chromium-review.googlesource.com/c/v8/v8/+/1762521)  
Regress: [mjsunit/regress/regress-v8-9534.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9534.js)  
```javascript
let i = 0;
function f() {
  i++;
  if (i > 10) {
    %PrepareFunctionForOptimization(f);
    %OptimizeFunctionOnNextCall(f);
  }

  new Promise(f);
  return f.x;
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b875f46^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=b875f46)  
[src/compiler/backend/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm/code-generator-arm.cc?cl=b875f46)  
[src/compiler/backend/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm/instruction-selector-arm.cc?cl=b875f46)  
[src/compiler/backend/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm64/code-generator-arm64.cc?cl=b875f46)  
[src/compiler/backend/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm64/instruction-selector-arm64.cc?cl=b875f46)  
...  
  

---   

## **regress-1018592.js (chromium issue)**  
   
**[Issue: Debug check failed: 1 <= capture_ix && capture_ix <= capture_count.](https://crbug.com/1018592)**  
**[Commit: [regexp] Fix invalid DCHECK in named capture logic](https://chromium.googlesource.com/v8/v8/+/5d5a659)**  
  
Date(Commit): Wed Oct 30 07:09:24 2019  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: M-78  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1886810](https://chromium-review.googlesource.com/c/v8/v8/+/1886810)  
Regress: [mjsunit/regress/regress-1018592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1018592.js)  
```javascript
const re = /()()()()(?<aaaab>)1/;
"111a1a".replace(re, () => {}) ;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d5a659^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=5d5a659)  
[test/mjsunit/regress/regress-1018592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1018592.js?cl=5d5a659)  
  

---   

## **regress-crbug-1018611.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1018611)**  
**[Commit: [parser] Add early return for declaration error in arrow head](https://chromium.googlesource.com/v8/v8/+/6d97ac5)**  
  
Date(Commit): Mon Oct 28 14:09:11 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1879906](https://chromium-review.googlesource.com/c/v8/v8/+/1879906)  
Regress: [mjsunit/regress/regress-crbug-1018611.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1018611.js)  
```javascript
assertThrows("(l-(c))=>", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d97ac5^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=6d97ac5)  
[test/mjsunit/regress/regress-crbug-1018611.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1018611.js?cl=6d97ac5)  
  

---   

## **regress-bound-functions.js (other issue)**  
   
**[Commit: [turbofan] Fix memory corruption with VirtualBoundFunctions](https://chromium.googlesource.com/v8/v8/+/48fb778)**  
  
Date(Commit): Mon Oct 28 13:20:16 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1879904](https://chromium-review.googlesource.com/c/v8/v8/+/1879904)  
Regress: [mjsunit/compiler/regress-bound-functions.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-bound-functions.js)  
```javascript
function foo() {
  return Array.prototype.sort.bind([]);
}

function bar() {
  return foo();
}

%PrepareFunctionForOptimization(foo);
%PrepareFunctionForOptimization(bar);
bar();
bar();
%OptimizeFunctionOnNextCall(bar);
bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/48fb778^!)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=48fb778)  
[test/mjsunit/compiler/regress-bound-functions.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-bound-functions.js?cl=48fb778)  
  
  
---   

## **regress-9894.js (v8 issue)**  
   
**[Issue:  V8: missing has_sealed_elements() has_frozen_elements() in Map::GetInitialElements()](https://crbug.com/v8/9894)**  
**[Commit: Handle nonextensible obj in Map::GetInitalElements](https://chromium.googlesource.com/v8/v8/+/65079f1)**  
  
Date(Commit): Mon Oct 28 08:00:48 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1876994](https://chromium-review.googlesource.com/c/v8/v8/+/1876994)  
Regress: [mjsunit/regress/regress-9894.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9894.js)  
```javascript
(function frozen() {
  const ary = [1.1]
  Object.defineProperty(ary, 0, {get:run_it} );

  // v8::internal::Runtime_ArrayIncludes_Slow.
  ary.includes();

  function run_it(el) {
    ary.length = 0;
    ary[0] = 1.1;
    Object.freeze(ary);
    return 2.2;
  }
})();

(function seal() {
  const ary = [1.1]
  Object.defineProperty(ary, 0, {get:run_it} );

  // v8::internal::Runtime_ArrayIncludes_Slow.
  ary.includes();

  function run_it(el) {
    ary.length = 0;
    ary[0] = 1.1;
    Object.seal(ary);
    return 2.2;
  }
})();

(function preventExtensions() {
  const ary = [1.1]
  Object.defineProperty(ary, 0, {get:run_it} );

  // v8::internal::Runtime_ArrayIncludes_Slow.
  ary.includes();

  function run_it(el) {
    ary.length = 0;
    ary[0] = 1.1;
    Object.preventExtensions(ary);
    return 2.2;
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65079f1^!)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=65079f1)  
[test/mjsunit/regress/regress-9894.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9894.js?cl=65079f1)  
  

---   

## **regress-1010272.js (chromium issue)**  
   
**[Issue: AwSnap! with 16 or more WASM threads](https://crbug.com/1010272)**  
**[Commit: [wasm] Fix incorrect check for growing shared WebAssembly.memory](https://chromium.googlesource.com/v8/v8/+/2599d3c)**  
  
Date(Commit): Wed Oct 23 18:14:50 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Needs-Triage-M79  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1869077](https://chromium-review.googlesource.com/c/v8/v8/+/1869077)  
Regress: [mjsunit/regress/wasm/regress-1010272.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1010272.js)  
```javascript
const kNumWorkers = 100;
const kNumMessages = 50;

function AllocMemory(initial, maximum = initial) {
  return new WebAssembly.Memory({initial : initial, maximum : maximum, shared : true});
}

(function RunTest() {
  let worker = [];
  for (let w = 0; w < kNumWorkers; w++) {
    worker[w] = new Worker(
        `onmessage =
        function(msg) {
          msg.memory.grow(1);
        }`, {type : 'string'});
  }

  for (let i = 0; i < kNumMessages; i++) {
    let memory = AllocMemory(1, 128);
    for (let w = 0; w < kNumWorkers; w++) {
      worker[w].postMessage({memory : memory});
    }
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2599d3c^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=2599d3c)  
[test/mjsunit/regress/wasm/regress-1010272.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1010272.js?cl=2599d3c)  
  

---   

## **regress-1016515.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1016515)**  
**[Commit: [wasm] Initialize new jump table correct for lazy compilation](https://chromium.googlesource.com/v8/v8/+/369f1ff)**  
  
Date(Commit): Tue Oct 22 12:44:22 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1873687](https://chromium-review.googlesource.com/c/v8/v8/+/1873687)  
Regress: [mjsunit/regress/wasm/regress-1016515.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1016515.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
var func = builder.addFunction('func', kSig_i_v).addBody([kExprI32Const, 1]);
var body = [];
for (let i = 0; i < 200; ++i) {
    body.push(kExprCallFunction, func.index);
}
for (let i = 1; i < 200; ++i) {
    body.push(kExprI32Add);
}
builder.addFunction('test', kSig_i_v).addBody(body).exportFunc();
var instance = builder.instantiate();
instance.exports.test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/369f1ff^!)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=369f1ff)  
[test/mjsunit/regress/wasm/regress-1016515.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1016515.js?cl=369f1ff)  
  

---   

## **regress-crbug-1015372.js (chromium issue)**  
   
**[Issue: Security: Bytecode mismatch at offset 1](https://crbug.com/1015372)**  
**[Commit: [parser] Push variables of non-arrow parenthesized expression to parent](https://chromium.googlesource.com/v8/v8/+/7ff8299)**  
  
Date(Commit): Tue Oct 22 10:40:05 2019  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1872389](https://chromium-review.googlesource.com/c/v8/v8/+/1872389)  
Regress: [mjsunit/regress/regress-crbug-1015372.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1015372.js)  
```javascript
(function() {
  function opt(flag) {
    function inline() {
      (function () {
        flag()
      })();
      (flag) = 1;
    }
    inline();
  }
  assertThrows(opt, TypeError);
})();

(function() {
  function opt(flag){
      function inline() {
          var f = (function() {
              return flag;
          });
          function g(x) {
            (flag) = x;
          }

          return [f,g];
      }
      return inline();
  }
  [f, g] = opt(1);

  %PrepareFunctionForOptimization(f);
  assertEquals(1, f());
  assertEquals(1, f());
  %OptimizeFunctionOnNextCall(f);
  assertEquals(1, f());

  g(2);

  assertEquals(2, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ff8299^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=7ff8299)  
[test/mjsunit/regress/regress-crbug-1015372.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1015372.js?cl=7ff8299)  
  

---   

## **regress-crbug-1016056.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/1016056)**  
**[Commit: Reland "Reland "[runtime] Remove extension slots from context objects""](https://chromium.googlesource.com/v8/v8/+/392a121)**  
  
Date(Commit): Tue Oct 22 09:12:53 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1863191](https://chromium-review.googlesource.com/c/v8/v8/+/1863191)  
Regress: [mjsunit/regress/regress-crbug-1016056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1016056.js)  
```javascript
function f(args) {
  eval();
  return arguments[0];
}
%PrepareFunctionForOptimization(f);
function g() {
  return f(1);
}
%PrepareFunctionForOptimization(g);
assertEquals(1, g());
%OptimizeFunctionOnNextCall(g);
assertEquals(1, g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/392a121^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=392a121)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=392a121)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=392a121)  
[src/builtins/builtins-arguments-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-arguments-gen.cc?cl=392a121)  
[src/builtins/builtins-async-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-async-gen.cc?cl=392a121)  
...  
  

---   

## **regress-crbug-1015945.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1015945)**  
**[Commit: [async stacks] Fix corner case for async generators.](https://chromium.googlesource.com/v8/v8/+/84cd9a8)**  
  
Date(Commit): Mon Oct 21 07:19:58 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1870230](https://chromium-review.googlesource.com/c/v8/v8/+/1870230)  
Regress: [mjsunit/regress/regress-crbug-1015945.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1015945.js)  
```javascript
async function* foo() {
  await 1;
  throw new Error();
}

(async () => {
  for await (const x of foo()) { }
})();

async_hooks.createHook({
  promiseResolve() {
    throw new Error();
  }
}).enable()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84cd9a8^!)  
[src/execution/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.cc?cl=84cd9a8)  
[test/mjsunit/regress/regress-crbug-1015945.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1015945.js?cl=84cd9a8)  
  

---   

## **regress-crbug-1015567.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1015567)**  
**[Commit: [parser] Accumulate even if we already thought we had an error](https://chromium.googlesource.com/v8/v8/+/94d8fcb)**  
  
Date(Commit): Fri Oct 18 14:30:05 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1869198](https://chromium-review.googlesource.com/c/v8/v8/+/1869198)  
Regress: [mjsunit/regress/regress-crbug-1015567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1015567.js)  
```javascript
assertThrows('a ( { b() {} } [ [ 1 , c.d = 1 ] = 1.1 ] )', SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/94d8fcb^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=94d8fcb)  
[test/mjsunit/regress/regress-crbug-1015567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1015567.js?cl=94d8fcb)  
  

---   

## **regress-1014798.js (chromium issue)**  
   
**[Issue: v8_wasm_compile_fuzzer: Fatal error in ](https://crbug.com/1014798)**  
**[Commit: [Liftoff] Fix stack slot initialization on arm and arm64](https://chromium.googlesource.com/v8/v8/+/7d09b27)**  
  
Date(Commit): Wed Oct 16 14:07:36 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1864933](https://chromium-review.googlesource.com/c/v8/v8/+/1864933)  
Regress: [mjsunit/regress/wasm/regress-1014798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1014798.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('main', kSig_i_iii)
    .addLocals({f32_count: 4})
    .addLocals({i64_count: 1})
    .addLocals({f32_count: 2})
    .addBodyWithEnd([
      kExprI64Const, 0,
      kExprLocalGet, 3,
      kExprI64SConvertF32,
      kExprI64Ne,
      kExprEnd,  // @17
    ]).exportFunc();
const instance = builder.instantiate();
assertEquals(0, instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d09b27^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=7d09b27)  
[src/wasm/baseline/arm64/liftoff-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm64/liftoff-assembler-arm64.h?cl=7d09b27)  
[test/mjsunit/regress/wasm/regress-1014798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1014798.js?cl=7d09b27)  
  

---   

## **regress-1013920.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1013920)**  
**[Commit: [asmjs] Disallow AsmJs instantiation from a SharedArrayBuffer.](https://chromium.googlesource.com/v8/v8/+/9de61eb)**  
  
Date(Commit): Tue Oct 15 12:45:29 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1862563](https://chromium-review.googlesource.com/c/v8/v8/+/1862563)  
Regress: [mjsunit/asm/regress-1013920.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-1013920.js)  
```javascript
function asm(stdlib, foreign, heap) {
  "use asm";
  var heap32 = new stdlib.Uint32Array(heap);
  function f() { return 0; }
  return {f : f};
}

var heap = Reflect.construct(
    SharedArrayBuffer,
    [1024 * 1024],
    ArrayBuffer.prototype.constructor);

asm(this, {}, heap);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9de61eb^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=9de61eb)  
[test/mjsunit/asm/regress-1013920.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-1013920.js?cl=9de61eb)  
  

---   

## **regress-9832.js (v8 issue)**  
   
**[Issue: EH program runs successfully on interpreter but not on turbofan](https://crbug.com/v8/9832)**  
**[Commit: [wasm] Fix bogus uses of {WasmGraphBuilder::Buffer}.](https://chromium.googlesource.com/v8/v8/+/47f3a53)**  
  
Date(Commit): Mon Oct 14 09:32:37 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1856006](https://chromium-review.googlesource.com/c/v8/v8/+/1856006)  
Regress: [mjsunit/regress/regress-9832.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9832.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestRegress9832() {
  let builder = new WasmModuleBuilder();
  let f = builder.addFunction("f", kSig_i_i)
      .addBody([
        kExprLocalGet, 0,
        kExprLocalGet, 0,
        kExprI32Add,
      ]).exportFunc();
  builder.addFunction("main", kSig_i_i)
      .addLocals({except_count: 1})
      .addBody([
        kExprTry, kWasmStmt,
          kExprLocalGet, 0,
          kExprCallFunction, f.index,
          kExprCallFunction, f.index,
          kExprLocalSet, 0,
        kExprCatch,
          kExprDrop,
          kExprLocalGet, 0,
          kExprCallFunction, f.index,
          kExprLocalSet, 0,
          kExprEnd,
        kExprLocalGet, 0,
      ]).exportFunc();
  let instance = builder.instantiate();
  assertEquals(92, instance.exports.main(23));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/47f3a53^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=47f3a53)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=47f3a53)  
[src/wasm/graph-builder-interface.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/graph-builder-interface.cc?cl=47f3a53)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=47f3a53)  
[test/mjsunit/regress/regress-9832.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9832.js?cl=47f3a53)  
  

---   

## **regress-crbug-1012301-1.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1012301)**  
**[Commit: [runtime] Fix CloneObject for all in-place repr changes](https://chromium.googlesource.com/v8/v8/+/947a124)**  
  
Date(Commit): Fri Oct 11 16:09:45 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1856005](https://chromium-review.googlesource.com/c/v8/v8/+/1856005)  
Regress: [mjsunit/regress/regress-crbug-1012301-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1012301-1.js), [mjsunit/regress/regress-crbug-1012301.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1012301.js)  
```javascript
function get() {
  // Update the descriptor array now shared between the Foo map and the
  // (Foo + c) map.
  o1.c = 10;
  // Change the type of the field on the new descriptor array in-place to
  // Tagged. If Object.assign has a cached descriptor array, then it will point
  // to the old Foo map's descriptors, which still have .b as Double.
  o2.b = "string";
  return 1;
}

function Foo() {
  Object.defineProperty(this, "a", {get, enumerable: true});
  // Initialise Foo.b to have Double representation.
  this.b = 1.5;
}

var o1 = new Foo();
var o2 = new Foo();
var target = {};
Object.assign(target, o2);
assertEquals(target.b, "string");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/947a124^!)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=947a124)  
[src/objects/property-details.h](https://cs.chromium.org/chromium/src/v8/src/objects/property-details.h?cl=947a124)  
[test/mjsunit/regress/regress-crbug-1012301.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1012301.js?cl=947a124)  
  

---   

## **regress-v8-9825.mjs (v8 issue)**  
   
**[Issue: Stress opt fails for await in 'next' position of for loop.](https://crbug.com/v8/9825)**  
**[Commit: [async] Fix bug with await in for 'next' position.](https://chromium.googlesource.com/v8/v8/+/f796f86)**  
  
Date(Commit): Thu Oct 10 18:06:07 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1849197](https://chromium-review.googlesource.com/c/v8/v8/+/1849197)  
Regress: [mjsunit/regress/regress-v8-9825.mjs](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9825.mjs)  
```javascript
async function foo() {
  for (;;await[]) {
    break;
  }
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f796f86^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=f796f86)  
[test/mjsunit/regress/regress-v8-9825.mjs](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9825.mjs?cl=f796f86)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=f796f86)  
  

---   

## **regress-1011980.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1011980)**  
**[Commit: [ptr-compr] Remove ChangeTaggedSignedToCompressedSigned optimization](https://chromium.googlesource.com/v8/v8/+/fabfa41)**  
  
Date(Commit): Wed Oct 09 09:58:01 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1847363](https://chromium-review.googlesource.com/c/v8/v8/+/1847363)  
Regress: [mjsunit/regress/regress-1011980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1011980.js)  
```javascript
let hex_b = 0x0b;
let hex_d = 0x0d;
let hex_20 = 0x20;
let hex_52 = 0x52;
let hex_fe = 0xfe;

function f(a) {
  let unused = [ a / 8, ...[ ...[ ...[], a / 8, ...[ 7, hex_fe, a, 0, 0, hex_20,
    6, hex_52, hex_d, 0, hex_b], 0, hex_b], hex_b]];
}

%PrepareFunctionForOptimization(f)
f(64)
f(64);
%OptimizeFunctionOnNextCall(f);
f(64);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fabfa41^!)  
[src/compiler/simplified-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator-reducer.cc?cl=fabfa41)  
[test/mjsunit/regress/regress-1011980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1011980.js?cl=fabfa41)  
  

---   

## **regress-crbug-1007608.js (chromium issue)**  
   
**[Issue: DCHECK failure in CheckType(RootIndex::kException) in roots-inl.h](https://crbug.com/1007608)**  
**[Commit: [wasm] Fix stack args in CWasmEntry stub](https://chromium.googlesource.com/v8/v8/+/f1e5488)**  
  
Date(Commit): Tue Oct 08 13:57:46 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-77, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1847352](https://chromium-review.googlesource.com/c/v8/v8/+/1847352)  
Regress: [mjsunit/regress/wasm/regress-crbug-1007608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-crbug-1007608.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let argc = 7;
let builder = new WasmModuleBuilder();
let types = new Array(argc).fill(kWasmI32);
let sig = makeSig(types, []);
let body = [];
for (let i = 0; i < argc; ++i) {
  body.push(kExprLocalGet, i);
}
body.push(kExprCallFunction, 0);
builder.addImport('', 'f', sig);
builder.addFunction("main", sig).addBody(body).exportAs('main');
let instance = builder.instantiate({
  '': {
    'f': function() { throw "don't crash"; }
  }
});
assertThrows(instance.exports.main);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1e5488^!)  
[src/execution/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.cc?cl=f1e5488)  
[test/mjsunit/regress/wasm/regress-crbug-1007608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-crbug-1007608.js?cl=f1e5488)  
  

---   

## **regress-crbug-1003732.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1003732)**  
**[Commit: [runtime] Don't set sticky bit on empty_slow_element_dictionary](https://chromium.googlesource.com/v8/v8/+/90d161f)**  
  
Date(Commit): Tue Oct 08 11:49:25 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1833686](https://chromium-review.googlesource.com/c/v8/v8/+/1833686)  
Regress: [mjsunit/regress/regress-crbug-1003732.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1003732.js)  
```javascript
function f_1() {
  var v = new Array();
  v[0] = 10;
  return v;
}

function test() {
  var setter_called = false;
  // Turn array to NumberDictionary
  Array.prototype[123456789] = 42;
  assertEquals(f_1().length, 1);

  // Reset to empty_slow_dictionary
  Array.prototype.length = 0;

  // This should reset the prototype validity cell.
  Array.prototype.__defineSetter__("0", function() {setter_called = true});
  f_1();
  assertEquals(setter_called, true);
}
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90d161f^!)  
[src/heap/setup-heap-internal.cc](https://cs.chromium.org/chromium/src/v8/src/heap/setup-heap-internal.cc?cl=90d161f)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=90d161f)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=90d161f)  
[test/mjsunit/regress/regress-crbug-1003732.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1003732.js?cl=90d161f)  
  

---   

## **regress-crbug-1011596-module.mjs (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1011596)**  
**[Commit: [parser] Fix preparsing of modules containing labels](https://chromium.googlesource.com/v8/v8/+/427a2fd)**  
  
Date(Commit): Mon Oct 07 10:18:14 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1844777](https://chromium-review.googlesource.com/c/v8/v8/+/1844777)  
Regress: [mjsunit/regress/regress-crbug-1011596-module.mjs](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1011596-module.mjs), [mjsunit/regress/regress-crbug-1011596.mjs](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1011596.mjs)  
```javascript
export function foo() {
  { label: 1 }
  return 42;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/427a2fd^!)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=427a2fd)  
[test/mjsunit/regress/regress-crbug-1011596-module.mjs](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1011596-module.mjs?cl=427a2fd)  
[test/mjsunit/regress/regress-crbug-1011596.mjs](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1011596.mjs?cl=427a2fd)  
  

---   

## **regress-crbug-1009728.js (chromium issue)**  
   
**[Issue: CHECK failure: Bytecode mismatch at offset 0 in interpreter.cc](https://crbug.com/1009728)**  
**[Commit: [parser] Delete unresolved variables created for labels](https://chromium.googlesource.com/v8/v8/+/5876122)**  
  
Date(Commit): Fri Oct 04 10:41:31 2019  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, Merge-Merged, reward-NA, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-78, M-78  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1836258](https://chromium-review.googlesource.com/c/v8/v8/+/1836258)  
Regress: [mjsunit/regress/regress-crbug-1009728.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1009728.js)  
```javascript
function foo(x) {
  (function bar() {
    {
      x: 1
    }
    function f() {}
  });
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5876122^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=5876122)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=5876122)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=5876122)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=5876122)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=5876122)  
...  
  

---   

## **regress-1006640.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1006640)**  
**[Commit: GCExtension: Properly support exceptions](https://chromium.googlesource.com/v8/v8/+/38c9016)**  
  
Date(Commit): Wed Oct 02 12:14:02 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1835531](https://chromium-review.googlesource.com/c/v8/v8/+/1835531)  
Regress: [mjsunit/regress/regress-1006640.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1006640.js)  
```javascript
function main() {
  const v2 = [1337,1337,1337,1337,1337];
  function v9() {
      const v15 = {get:RegExp};
      Object.defineProperty(v2,501,v15);
      const v18 = RegExp();
      const v19 = 1337 instanceof v18;
  }
  const v30 = {defineProperty:Function,get:v9,getPrototypeOf:Object};
  const v32 = new Proxy(ArrayBuffer,v30);
  const v34 = gc(v32);
}

assertThrows(() => main(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/38c9016^!)  
[src/extensions/gc-extension.cc](https://cs.chromium.org/chromium/src/v8/src/extensions/gc-extension.cc?cl=38c9016)  
[test/mjsunit/regress/regress-1002827.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1002827.js?cl=38c9016)  
[test/mjsunit/regress/regress-1006640.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1006640.js?cl=38c9016)  
  

---   

## **regress-crbug-1008632.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1008632)**  
**[Commit: Modify the DCHECK in when computing KeyedAccessStoreMode.](https://chromium.googlesource.com/v8/v8/+/1e3c387)**  
  
Date(Commit): Mon Sep 30 18:59:48 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1828480](https://chromium-review.googlesource.com/c/v8/v8/+/1828480)  
Regress: [mjsunit/regress/regress-crbug-1008632.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1008632.js)  
```javascript
var __v_9690 = function () {};
try {
    (function () {
        __f_1653();
    })()
} catch (__v_9763) {
}
function __f_1653(__v_9774, __v_9775) {
  try {
  } catch (e) {}
    __v_9774[__v_9775 + 4] = 2;
}
(function () {
    %PrepareFunctionForOptimization(__f_1653);
    __f_1653(__v_9690, true);
    %OptimizeFunctionOnNextCall(__f_1653);
    assertThrows(() => __f_1653(), TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e3c387^!)  
[src/objects/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/objects/feedback-vector.cc?cl=1e3c387)  
[test/mjsunit/regress/regress-crbug-1008632.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1008632.js?cl=1e3c387)  
  

---   

## **regress-9781.js (v8 issue)**  
   
**[Issue: Regression in v8's PRoxy implementation (chrome & node)](https://crbug.com/v8/9781)**  
**[Commit: Add missing null condition in Proxy GetPrototypeof](https://chromium.googlesource.com/v8/v8/+/c721203)**  
  
Date(Commit): Mon Sep 30 17:56:34 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1829897](https://chromium-review.googlesource.com/c/v8/v8/+/1829897)  
Regress: [mjsunit/regress/regress-9781.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9781.js)  
```javascript
var proto = Object.getPrototypeOf(new Proxy(Object.create(null), {
  getPrototypeOf(target) {
    return Reflect.getPrototypeOf(target);
  }
} ));

assertEquals(proto, null);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c721203^!)  
[src/builtins/proxy-get-prototype-of.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/proxy-get-prototype-of.tq?cl=c721203)  
[test/mjsunit/regress/regress-9781.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9781.js?cl=c721203)  
  

---   

## **regress-1008414.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1008414)**  
**[Commit: [parser] Prevent feedback slot merging for dynamic globals](https://chromium.googlesource.com/v8/v8/+/8de672c)**  
  
Date(Commit): Mon Sep 30 11:57:09 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1829270](https://chromium-review.googlesource.com/c/v8/v8/+/1829270)  
Regress: [mjsunit/regress/regress-1008414.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1008414.js)  
```javascript
(function bar() {
  (function foo(
      x = new class B extends A(eval) { }
    ) {
        eval();
    })();
  eval();
})()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8de672c^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=8de672c)  
[test/mjsunit/regress/regress-1008414.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1008414.js?cl=8de672c)  
  

---   

## **regress-crbug-1003403.js (chromium issue)**  
   
**[Issue: CHECK failure: Bytecode mismatch at offset 60 in interpreter.cc](https://crbug.com/1003403)**  
**[Commit: [parser] Fix destructured parameters in arrowheads](https://chromium.googlesource.com/v8/v8/+/f674045)**  
  
Date(Commit): Tue Sep 24 14:11:52 2019  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1815131](https://chromium-review.googlesource.com/c/v8/v8/+/1815131)  
Regress: [mjsunit/regress/regress-crbug-1003403.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1003403.js)  
```javascript
({ x: b = 0 }) => {
  try { b; } catch (e) {}
  function a() { b }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f674045^!)  
[src/ast/variables.h](https://cs.chromium.org/chromium/src/v8/src/ast/variables.h?cl=f674045)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=f674045)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=f674045)  
[test/mjsunit/regress/regress-crbug-1003403.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1003403.js?cl=f674045)  
  

---   

## **regress-1006670.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1006670)**  
**[Commit: [regexp] Adhere to the stack limit in the interpreter](https://chromium.googlesource.com/v8/v8/+/256a816)**  
  
Date(Commit): Tue Sep 24 13:33:09 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1821458](https://chromium-review.googlesource.com/c/v8/v8/+/1821458)  
Regress: [mjsunit/regress/regress-1006670.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1006670.js)  
```javascript
assertThrows(() => /(a?;?){4000000}/.exec("a"), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/256a816^!)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=256a816)  
[src/regexp/regexp-stack.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-stack.h?cl=256a816)  
[test/mjsunit/regress/regress-1006670.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1006670.js?cl=256a816)  
  

---   

## **regress-9759.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/9759)**  
**[Commit: [wasm] Limit number of labels for {br_table} instruction.](https://chromium.googlesource.com/v8/v8/+/cf34210)**  
  
Date(Commit): Tue Sep 24 12:54:49 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1821457](https://chromium-review.googlesource.com/c/v8/v8/+/1821457)  
Regress: [mjsunit/regress/wasm/regress-9759.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-9759.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

const NUM_CASES = 0xfffd;

(function TestBrTableTooLarge() {
  let builder = new WasmModuleBuilder();
  let cases = new Array(NUM_CASES).fill(0);
  builder.addFunction('main', kSig_v_i)
      .addBody([].concat([
        kExprBlock, kWasmStmt,
          kExprLocalGet, 0,
          kExprBrTable], wasmSignedLeb(NUM_CASES),
          cases, [0,
        kExprEnd
      ])).exportFunc();
  assertThrows(() => new WebAssembly.Module(builder.toBuffer()),
    WebAssembly.CompileError, /invalid table count/);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf34210^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=cf34210)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=cf34210)  
[src/wasm/wasm-limits.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-limits.h?cl=cf34210)  
[test/mjsunit/regress/wasm/regress-9759.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-9759.js?cl=cf34210)  
  

---   

## **regress-1006629.js (chromium issue)**  
   
**[Issue: Fatal error in v8_SharedArrayBuffer_Newv8::Utils::ReportApiFailure](https://crbug.com/1006629)**  
**[Commit: Fix construction of empty backing stores for SharedArrayBuffers](https://chromium.googlesource.com/v8/v8/+/39ecc99)**  
  
Date(Commit): Mon Sep 23 13:42:29 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1815244](https://chromium-review.googlesource.com/c/v8/v8/+/1815244)  
Regress: [mjsunit/regress/regress-1006629.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1006629.js)  
```javascript
const workerScript = `
  onmessage = function() {
  };`;
const worker = new Worker(workerScript, {type: 'string'});
const i32a = new Int32Array( new SharedArrayBuffer() );
worker.postMessage([i32a.buffer]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39ecc99^!)  
[src/api/api.cc](https://cs.chromium.org/chromium/src/v8/src/api/api.cc?cl=39ecc99)  
[src/objects/backing-store.cc](https://cs.chromium.org/chromium/src/v8/src/objects/backing-store.cc?cl=39ecc99)  
[src/objects/backing-store.h](https://cs.chromium.org/chromium/src/v8/src/objects/backing-store.h?cl=39ecc99)  
[test/mjsunit/regress/regress-1006629.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1006629.js?cl=39ecc99)  
  

---   

## **regress-crbug-1006592.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/1006592)**  
**[Commit: [asm.js] Fix parsing of float coercion arguments.](https://chromium.googlesource.com/v8/v8/+/d1e9b88)**  
  
Date(Commit): Mon Sep 23 12:26:26 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1815255](https://chromium-review.googlesource.com/c/v8/v8/+/1815255)  
Regress: [mjsunit/regress/regress-crbug-1006592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1006592.js)  
```javascript
function Module(stdlib) {
  "use asm";
  var fround = stdlib.Math.fround;
  function f(a, b) {
    a = +a;
    b = +b;
    return fround(a, b);
  }
  return { f: f };
}

var m = Module(this);
assertEquals(23, m.f(23));
assertEquals(42, m.f(42, 65));
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d1e9b88^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=d1e9b88)  
[test/mjsunit/regress/regress-crbug-1006592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1006592.js?cl=d1e9b88)  
  

---   

## **regress-crbug-1006631.js (chromium issue)**  
   
**[Issue: CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (IsDereferenceAllowed()) in handles.h](https://crbug.com/1006631)**  
**[Commit: [wasm] Load call builtin in JS-to-JS wrappers.](https://chromium.googlesource.com/v8/v8/+/ca02d58)**  
  
Date(Commit): Mon Sep 23 10:43:51 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1815245](https://chromium-review.googlesource.com/c/v8/v8/+/1815245)  
Regress: [mjsunit/regress/wasm/regress-crbug-1006631.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-crbug-1006631.js)  
```javascript
new WebAssembly.Function({ parameters: [], results: [] }, x => x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca02d58^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=ca02d58)  
[test/mjsunit/regress/wasm/regress-crbug-1006631.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-crbug-1006631.js?cl=ca02d58)  
  

---   

## **regress-v8-9758.js (v8 issue)**  
   
**[Issue: Debug check failure: Bytecode Mismatch](https://crbug.com/v8/9758)**  
**[Commit: [parser] Prevent lazy parsing of arrow functions](https://chromium.googlesource.com/v8/v8/+/4921821)**  
  
Date(Commit): Mon Sep 23 08:59:18 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1816510](https://chromium-review.googlesource.com/c/v8/v8/+/1816510)  
Regress: [mjsunit/regress/regress-v8-9758.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9758.js)  
```javascript
((a = ((b = a) => {})()) => 1)();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4921821^!)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=4921821)  
[test/mjsunit/regress/regress-v8-9758.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9758.js?cl=4921821)  
  

---   

## **regress-crbug-1004037.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1004037)**  
**[Commit: [ic] Add support for StoreSlow() in Global Dispatcher](https://chromium.googlesource.com/v8/v8/+/99188fc)**  
  
Date(Commit): Fri Sep 20 17:05:09 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1812657](https://chromium-review.googlesource.com/c/v8/v8/+/1812657)  
Regress: [mjsunit/regress/regress-crbug-1004037.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1004037.js)  
```javascript
__v_1 = {};
__v_1.__defineGetter__('x', function () { });
__proto__ = __v_1;
function __f_4() {
    __v_1 = {};
}
function __f_3() {
    'use strict';
    x = 42;
}
__f_4()
try {
    __f_3();
} catch (e) { }

__proto__ = __v_1;
assertThrows(() => __f_3(), ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/99188fc^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=99188fc)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=99188fc)  
[test/mjsunit/regress/regress-crbug-1002628.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1002628.js?cl=99188fc)  
[test/mjsunit/regress/regress-crbug-1004037.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1004037.js?cl=99188fc)  
  

---   

## **regress-crbug-1002628.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/1002628)**  
**[Commit: [ic] Add support for StoreSlow() in Global Dispatcher](https://chromium.googlesource.com/v8/v8/+/99188fc)**  
  
Date(Commit): Fri Sep 20 17:05:09 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1812657](https://chromium-review.googlesource.com/c/v8/v8/+/1812657)  
Regress: [mjsunit/regress/regress-crbug-1002628.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1002628.js)  
```javascript
"use strict";
var __v_0 = {};
try {
    __v_0 = this;
    Object.freeze(__v_0);
}
catch (e) {
}

function f() {
    x = { [Symbol.toPrimitive]: () => FAIL };
}
try {
    f()
} catch (e) { }
assertThrows(() => f(), ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/99188fc^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=99188fc)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=99188fc)  
[test/mjsunit/regress/regress-crbug-1002628.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1002628.js?cl=99188fc)  
[test/mjsunit/regress/regress-crbug-1004037.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1004037.js?cl=99188fc)  
  

---   

## **regress-crbug-997056.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Word32Equal(DecodeWord32<PropertyDetails::KindField>(details)](https://crbug.com/997056)**  
**[Commit: [ic] Fix accessor to data reconfiguration case](https://chromium.googlesource.com/v8/v8/+/ecafe04)**  
  
Date(Commit): Thu Sep 19 14:35:46 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Merge-Request-76, M-76, M-77, M-78, Merge-Review-77, Merge-Review-78  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1813021](https://chromium-review.googlesource.com/c/v8/v8/+/1813021)  
Regress: [mjsunit/regress/regress-crbug-997056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-997056.js)  
```javascript
for (let i = 0; i < 4; ++i) {
    var obj1 = {
        get [obj1]() {},
        ...obj2,
    };
    var obj2 = { [obj1]: 0 };
    print(obj2);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecafe04^!)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=ecafe04)  
[test/mjsunit/regress/regress-crbug-997056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-997056.js?cl=ecafe04)  
  

---   

## **regress-1005400.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/1005400)**  
**[Commit: [CSA] Ensure we only call ToName once in KeyedLoadICGeneric.](https://chromium.googlesource.com/v8/v8/+/513c751)**  
  
Date(Commit): Thu Sep 19 12:39:46 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1813022](https://chromium-review.googlesource.com/c/v8/v8/+/1813022)  
Regress: [mjsunit/regress/regress-1005400.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1005400.js)  
```javascript
function foo(a, key) {
  a[key];
}

let obj = {};
let count = 0;

var key_obj = {
  toString: function() {
    count++;
    // Force string to be internalized during keyed lookup.
    return 'foo' + count;
  }
};

foo(obj, key_obj);

assertEquals(count, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/513c751^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=513c751)  
[test/mjsunit/regress/regress-1005400.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1005400.js?cl=513c751)  
  

---   

## **regress-1004912.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1004912)**  
**[Commit: [CSA][cleanup] Use Name instead of String type for var_name in KeyedLoadICGeneric.](https://chromium.googlesource.com/v8/v8/+/b946521)**  
  
Date(Commit): Wed Sep 18 11:22:28 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1809370](https://chromium-review.googlesource.com/c/v8/v8/+/1809370)  
Regress: [mjsunit/regress/regress-1004912.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1004912.js)  
```javascript
var key = {
  toString() {
    return Symbol();
  }
};

var obj = {};
obj[key];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b946521^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=b946521)  
[test/mjsunit/regress/regress-1004912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1004912.js?cl=b946521)  
  

---   

## **regress-crbug-1004061.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(Parameter(Descriptor::kElements)) at ../../src/builtins](https://crbug.com/1004061)**  
**[Commit: [csa] Fix parameter casting on empty arrays](https://chromium.googlesource.com/v8/v8/+/bec49d8)**  
  
Date(Commit): Mon Sep 16 13:49:21 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-78, M-78  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1806676](https://chromium-review.googlesource.com/c/v8/v8/+/1806676)  
Regress: [mjsunit/regress/regress-crbug-1004061.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1004061.js)  
```javascript
(function testPackedDoublesIncludes() {
  arr = [1.5, 2.5];
  arr.length = 0;
  function f() {
    return arr.includes(1);
  };
  %PrepareFunctionForOptimization(f);
  assertEquals(f(), false);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(f(), false);
})();

(function testHoleyDoublesIncludes() {
  arr = [1.1];
  arr[3]= 1.5;
  arr.length = 0;
  function f() {
    return arr.includes(1);
  };
  %PrepareFunctionForOptimization(f);
  assertEquals(f(), false);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(f(), false);
})();

(function testPackedDoublesIndexOf() {
  arr = [1.5, 2.5];
  arr.length = 0;
  function f() {
    return arr.indexOf(1);
  };
  %PrepareFunctionForOptimization(f);
  assertEquals(f(), -1);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(f(), -1);
})();

(function testHoleyDoublesIndexOf() {
  arr = [1.1];
  arr[3]= 1.5;
  arr.length = 0;
  function f() {
    return arr.indexOf(1);
  };
  %PrepareFunctionForOptimization(f);
  assertEquals(f(), -1);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(f(), -1);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bec49d8^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=bec49d8)  
[test/mjsunit/regress/regress-crbug-1004061.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1004061.js?cl=bec49d8)  
  

---   

## **regress-1003730.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1003730)**  
**[Commit: [turbofan] Add a missing object to the broker](https://chromium.googlesource.com/v8/v8/+/2f9d2fc)**  
  
Date(Commit): Mon Sep 16 12:39:26 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1806913](https://chromium-review.googlesource.com/c/v8/v8/+/1806913)  
Regress: [mjsunit/regress/regress-1003730.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1003730.js)  
```javascript
function bar(error) {
  try {
    throw "didn't throw TypeError";
  } catch (err) {
    error instanceof error, "didn't throw " + error.prototype.name;
  }
}
function foo(param) {
  bar(TypeError);
}
try {
  bar();
} catch (e) {}
%PrepareFunctionForOptimization(foo);
try {
  foo();
} catch (e) {}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f9d2fc^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=2f9d2fc)  
[test/mjsunit/regress/regress-1003730.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1003730.js?cl=2f9d2fc)  
  

---   

## **regress-1003919.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1003919)**  
**[Commit: [CSA] Update TryLookupProperty to JSReceiver type.](https://chromium.googlesource.com/v8/v8/+/61085f2)**  
  
Date(Commit): Mon Sep 16 12:20:31 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1803651](https://chromium-review.googlesource.com/c/v8/v8/+/1803651)  
Regress: [mjsunit/regress/regress-1003919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1003919.js)  
```javascript
var obj = {foo: 'bar'};
Object.defineProperty(obj, 'foo', {
  get: function () {
  }
});
obj.__proto__ = new Proxy([], {});

function getKey() {
  return 'values'
}

obj[getKey()] = 1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/61085f2^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=61085f2)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=61085f2)  
[test/mjsunit/regress/regress-1003919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1003919.js?cl=61085f2)  
  

---   

## **regress-crbug-1002388.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1002388)**  
**[Commit: [wasm] Fix WebAssembly.Table#get for constructed functions.](https://chromium.googlesource.com/v8/v8/+/7da8f2c)**  
  
Date(Commit): Thu Sep 12 09:40:55 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1795352](https://chromium-review.googlesource.com/c/v8/v8/+/1795352)  
Regress: [mjsunit/regress/wasm/regress-crbug-1002388.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-crbug-1002388.js)  
```javascript
(function TestTableSetAndGetFunction() {
  let func = new WebAssembly.Function({ parameters: [], results: [] }, x => x);
  let table = new WebAssembly.Table({ element: "anyfunc", initial: 1 });
  table.set(0, func);
  table.get(0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7da8f2c^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=7da8f2c)  
[test/mjsunit/regress/wasm/regress-crbug-1002388.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-crbug-1002388.js?cl=7da8f2c)  
[test/mjsunit/wasm/type-reflection-with-anyref.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/type-reflection-with-anyref.js?cl=7da8f2c)  
  

---   

## **regress-1002827.js (chromium issue)**  
   
**[Issue: Fatal error in v8::ToLocalCheckedv8::Utils::ReportApiFailure](https://crbug.com/1002827)**  
**[Commit: [heap] Fix parameter parsing on GC builtin](https://chromium.googlesource.com/v8/v8/+/3569a4f)**  
  
Date(Commit): Wed Sep 11 10:13:16 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1796556](https://chromium-review.googlesource.com/c/v8/v8/+/1796556)  
Regress: [mjsunit/regress/regress-1002827.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1002827.js)  
```javascript
var PI = new Proxy(this, {
  get() {
      PI();
  }
});

assertThrows(() => new gc(PI, {}), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3569a4f^!)  
[src/extensions/gc-extension.cc](https://cs.chromium.org/chromium/src/v8/src/extensions/gc-extension.cc?cl=3569a4f)  
[test/mjsunit/regress/regress-1002827.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1002827.js?cl=3569a4f)  
  

---   

## **regress-996161.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/996161)**  
**[Commit: Fix EmitGenericPropertyStore to bailout on stores to TypedArrays](https://chromium.googlesource.com/v8/v8/+/ecf178a)**  
  
Date(Commit): Tue Sep 10 10:13:38 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1789396](https://chromium-review.googlesource.com/c/v8/v8/+/1789396)  
Regress: [mjsunit/regress/regress-996161.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996161.js)  
```javascript
function checkOwnProperties(v, count) {
  var properties = Object.getOwnPropertyNames(v);
  assertEquals(properties.length, count);
}


function testStoreNoFeedback() {
  arr = new Int32Array(10);
  function f(a) { a["-1"] = 15; }

  for (var i = 0; i < 3; i++) {
    arr.__defineGetter__("x", function() { });
    checkOwnProperties(arr, 11);
    f(arr);
  }
}
testStoreNoFeedback();

function testStoreGeneric() {
  arr = new Int32Array(10);
  var index = "-1";
  function f1(a) { a[index] = 15; }
  %EnsureFeedbackVectorForFunction(f1);

  // Make a[index] in f1 megamorphic
  f1({a: 1});
  f1({b: 1});
  f1({c: 1});
  f1({d: 1});

  for (var i = 0; i < 3; i++) {
    arr.__defineGetter__("x", function() { });
    checkOwnProperties(arr, 11);
    f1(arr);
  }
}
testStoreGeneric();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecf178a^!)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=ecf178a)  
[test/mjsunit/regress/regress-996161.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-996161.js?cl=ecf178a)  
  

---   

## **regress-crbug-1000094.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1000094)**  
**[Commit: [parser] Fix arrowhead parsing in the script scope](https://chromium.googlesource.com/v8/v8/+/6f17f5d)**  
  
Date(Commit): Tue Sep 10 09:11:07 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1789705](https://chromium-review.googlesource.com/c/v8/v8/+/1789705)  
Regress: [mjsunit/regress/regress-crbug-1000094.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1000094.js)  
```javascript
var f = (( {a: b} = {
    a() {
      return b;
    }
}) => b)()();

assertEquals(f, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f17f5d^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=6f17f5d)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=6f17f5d)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=6f17f5d)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=6f17f5d)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=6f17f5d)  
...  
  

---   

## **regress-1000635.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1000635)**  
**[Commit: Correctly handlify two frame {Summarize} methods](https://chromium.googlesource.com/v8/v8/+/fba03ab)**  
  
Date(Commit): Thu Sep 05 15:42:59 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1787429](https://chromium-review.googlesource.com/c/v8/v8/+/1787429)  
Regress: [mjsunit/regress/regress-1000635.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1000635.js)  
```javascript
function add(a, b) {
  throw new Error();
}
for (let i = 0; i < 100; ++i) {
  try {
    add(1, 2);
  } catch (e) {
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fba03ab^!)  
[src/execution/frames.cc](https://cs.chromium.org/chromium/src/v8/src/execution/frames.cc?cl=fba03ab)  
[test/mjsunit/regress/regress-1000635.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1000635.js?cl=fba03ab)  
  

---   

## **regress-997989.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/997989)**  
**[Commit: Reland^2 "[ic] In-place Double -> Tagged transitions""](https://chromium.googlesource.com/v8/v8/+/470e685)**  
  
Date(Commit): Thu Sep 05 15:20:19 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1743973](https://chromium-review.googlesource.com/c/v8/v8/+/1743973)  
Regress: [mjsunit/regress/regress-997989.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-997989.js)  
```javascript
function foo(o) {
  for (var i in o) {
    return o[i];
  }
}

var o = { x: 0.5 };

%PrepareFunctionForOptimization(foo);
assertEquals(foo(o), 0.5);
assertEquals(foo(o), 0.5);
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(o), 0.5);

o.x = "abc";

assertEquals(foo(o), "abc");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/470e685^!)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=470e685)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=470e685)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=470e685)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=470e685)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=470e685)  
...  
  

---   

## **regress-crbug-1000170.js (chromium issue)**  
   
**[Issue: CHECK failure: Bytecode mismatch at offset 1 in interpreter.cc](https://crbug.com/1000170)**  
**[Commit: [parser] Don't mark const variables as assigned](https://chromium.googlesource.com/v8/v8/+/a35a705)**  
  
Date(Commit): Thu Sep 05 14:44:29 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-78, M-78, merge-merged-7.8  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1782170](https://chromium-review.googlesource.com/c/v8/v8/+/1782170)  
Regress: [mjsunit/regress/regress-crbug-1000170.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-1000170.js)  
```javascript
(function a() {
  function b() { a(); }
  function c() { eval(); }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a35a705^!)  
[src/ast/variables.h](https://cs.chromium.org/chromium/src/v8/src/ast/variables.h?cl=a35a705)  
[test/mjsunit/regress/regress-crbug-1000170.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1000170.js?cl=a35a705)  
[test/mjsunit/regress/regress-crbug-999450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-999450.js?cl=a35a705)  
  

---   

## **regress-crbug-999450.js (chromium issue)**  
   
**[Issue: Crash caused by v8 bytecode not match check](https://crbug.com/999450)**  
**[Commit: [parser] Don't mark const variables as assigned](https://chromium.googlesource.com/v8/v8/+/a35a705)**  
  
Date(Commit): Thu Sep 05 14:44:29 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, allpublic, Via-Wizard-Security, merge-merged-7.8  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1782170](https://chromium-review.googlesource.com/c/v8/v8/+/1782170)  
Regress: [mjsunit/regress/regress-crbug-999450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-999450.js)  
```javascript
(function foo() {
    foo = null;
    () => foo;
})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a35a705^!)  
[src/ast/variables.h](https://cs.chromium.org/chromium/src/v8/src/ast/variables.h?cl=a35a705)  
[test/mjsunit/regress/regress-crbug-1000170.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-1000170.js?cl=a35a705)  
[test/mjsunit/regress/regress-crbug-999450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-999450.js?cl=a35a705)  
  

---   

## **regress-crbug-997320.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/997320)**  
**[Commit: [parser] Improve hole check elision in async arrow funcs](https://chromium.googlesource.com/v8/v8/+/afca89f)**  
  
Date(Commit): Wed Sep 04 09:13:03 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1710671](https://chromium-review.googlesource.com/c/v8/v8/+/1710671)  
Regress: [mjsunit/regress/regress-crbug-997320.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-997320.js)  
```javascript
async(a, b = a) => {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afca89f^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=afca89f)  
[test/mjsunit/regress/regress-crbug-997320.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-997320.js?cl=afca89f)  
  

---   

## **regress-v8-9656.js (v8 issue)**  
   
**[Issue: Debug check failed: bytecode->IsBytecodeEqual](https://crbug.com/v8/9656)**  
**[Commit: [coverage] Collect source positions when toggling mode](https://chromium.googlesource.com/v8/v8/+/3e545f3)**  
  
Date(Commit): Fri Aug 30 17:58:30 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1774721](https://chromium-review.googlesource.com/c/v8/v8/+/1774721)  
Regress: [mjsunit/regress/regress-v8-9656.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9656.js)  
```javascript
%DebugToggleBlockCoverage(true);

try {
  throw new Error();
} catch (e) {
  e.stack;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e545f3^!)  
[src/debug/debug-coverage.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-coverage.cc?cl=3e545f3)  
[src/debug/debug-type-profile.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-type-profile.cc?cl=3e545f3)  
[src/execution/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.cc?cl=3e545f3)  
[src/execution/isolate.h](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.h?cl=3e545f3)  
[test/mjsunit/regress/regress-v8-9656.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9656.js?cl=3e545f3)  
  

---   

## **regress-996751.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/996751)**  
**[Commit: [scopes] Push sloppy eval check through eval scopes](https://chromium.googlesource.com/v8/v8/+/f6057ff)**  
  
Date(Commit): Thu Aug 29 14:49:28 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1773247](https://chromium-review.googlesource.com/c/v8/v8/+/1773247)  
Regress: [mjsunit/regress/regress-996751.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996751.js)  
```javascript
eval(`
  eval("");
  (function f() {
    // This undefined should always be known to be the global undefined value,
    // even though there is a sloppy eval call inside the top eval scope.
    return undefined;
  })();
`);

eval(`
  eval(\`
    eval(\\\`
      eval("");
      (function f() {
        return undefined;
      })();
    \\\`);
  \`);
`);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6057ff^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=f6057ff)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=f6057ff)  
[src/debug/debug-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-scopes.cc?cl=f6057ff)  
[src/diagnostics/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/diagnostics/objects-printer.cc?cl=f6057ff)  
[src/objects/scope-info.cc](https://cs.chromium.org/chromium/src/v8/src/objects/scope-info.cc?cl=f6057ff)  
...  
  

---   

## **regress-nonextensiblearray-store-outofbounds.js (other issue)**  
   
**[Commit: Add new nonextensible element kinds](https://chromium.googlesource.com/v8/v8/+/1f4bec2)**  
  
Date(Commit): Wed Aug 28 17:24:49 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1760976](https://chromium-review.googlesource.com/c/v8/v8/+/1760976)  
Regress: [mjsunit/compiler/regress-nonextensiblearray-store-outofbounds.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-nonextensiblearray-store-outofbounds.js)  
```javascript
const v3 = [0,"symbol"];
const v5 = 0 - 1;
const v6 = Object.preventExtensions(v3);
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
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f4bec2^!)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=1f4bec2)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=1f4bec2)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=1f4bec2)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=1f4bec2)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=1f4bec2)  
...  
  
  
---   

## **regress-crbug-992914.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/992914)**  
**[Commit: Add new nonextensible element kinds](https://chromium.googlesource.com/v8/v8/+/1f4bec2)**  
  
Date(Commit): Wed Aug 28 17:24:49 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1760976](https://chromium-review.googlesource.com/c/v8/v8/+/1760976)  
Regress: [mjsunit/regress/regress-crbug-992914.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-992914.js)  
```javascript
function mainFreeze() {
  const v2 = {foo:1.1};
  Object.freeze(v2);
  const v12 = {foo:2.2};
  Object.preventExtensions(v12);
  Object.freeze(v12);
  const v18 = {foo:Object};
  v12.__proto__ = 0;
  v2[5] = 1;
}
mainFreeze();

function mainSeal() {
  const v2 = {foo:1.1};
  Object.seal(v2);
  const v12 = {foo:2.2};
  Object.preventExtensions(v12);
  Object.seal(v12);
  const v18 = {foo:Object};
  v12.__proto__ = 0;
  v2[5] = 1;
}
mainSeal();

function testSeal() {
  a = new RangeError(null, null, null);
  a.toFixed = () => {};
  e = Object.seal(a);
  a = new RangeError(null, null, null);
  a.toFixed = () => {};
  k = Object.preventExtensions(a);
  l = Object.seal(a);
  a.toFixed = () => {};
  assertThrows(() => {
    Array.prototype.unshift.call(l, false, Infinity);
  }, TypeError);
}
testSeal();

function testFreeze() {
  a = new RangeError(null, null, null);
  a.toFixed = () => {};
  e = Object.freeze(a);
  a = new RangeError(null, null, null);
  a.toFixed = () => {};
  k = Object.preventExtensions(a);
  l = Object.freeze(a);
  a.toFixed = () => {};
  assertThrows(() => {
    Array.prototype.unshift.call(l, false, Infinity);
  }, TypeError);
}
testFreeze();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f4bec2^!)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=1f4bec2)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=1f4bec2)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=1f4bec2)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=1f4bec2)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=1f4bec2)  
...  
  

---   

## **regress-crbug-918301.js (chromium issue)**  
   
**[Issue: Javascript OOM in invalid table size](https://crbug.com/918301)**  
**[Commit: [runtime] Throw range error on too many properties](https://chromium.googlesource.com/v8/v8/+/4477097)**  
  
Date(Commit): Wed Aug 28 15:58:04 2019  
Components: Blink>JavaScript>Runtime  
Labels: Arch-x86_64, Via-Wizard-Javascript, Triaged-ET, M-73, Target-73, FoundIn-71, FoundIn-72, FoundIn-73, Needs-Triage-M71  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1545902](https://chromium-review.googlesource.com/c/v8/v8/+/1545902)  
Regress: [mjsunit/regress/regress-crbug-918301.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-918301.js)  
```javascript
assertThrows(() => Object.getOwnPropertyDescriptors(Array(1e9).join('c')), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4477097^!)  
[src/common/globals.h](https://cs.chromium.org/chromium/src/v8/src/common/globals.h?cl=4477097)  
[src/common/message-template.h](https://cs.chromium.org/chromium/src/v8/src/common/message-template.h?cl=4477097)  
[src/heap/factory.cc](https://cs.chromium.org/chromium/src/v8/src/heap/factory.cc?cl=4477097)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=4477097)  
[src/json/json-stringifier.cc](https://cs.chromium.org/chromium/src/v8/src/json/json-stringifier.cc?cl=4477097)  
...  
  

---   

## **regress-996391.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/996391)**  
**[Commit: [regexp] Dont attempt to match '^' before the start of the string](https://chromium.googlesource.com/v8/v8/+/1990b1e)**  
  
Date(Commit): Wed Aug 28 14:23:39 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1771794](https://chromium-review.googlesource.com/c/v8/v8/+/1771794)  
Regress: [mjsunit/regress/regress-996391.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996391.js)  
```javascript
assertArrayEquals(["o"], /.(?<!^.)/m.exec("foobar"));
assertArrayEquals(["o"], /.(?<!\b.)/m.exec("foobar"));
assertArrayEquals(["f"], /.(?<!\B.)/m.exec("foobar"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1990b1e^!)  
[src/regexp/regexp-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.cc?cl=1990b1e)  
[test/mjsunit/regress/regress-996391.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-996391.js?cl=1990b1e)  
  

---   

## **regress-crbug-993980.js (chromium issue)**  
   
**[Issue: json parser assume wrong data structure](https://crbug.com/993980)**  
**[Commit: [json] Don't consume sibling feedback from objects with detached maps](https://chromium.googlesource.com/v8/v8/+/864cacd)**  
  
Date(Commit): Mon Aug 26 15:57:52 2019  
Components: Blink>JavaScript  
Labels: Arch-x86_64, Hotlist-Interop, ReleaseBlock-Stable, hasbisect-per-revision, Via-Wizard-API, Triaged-ET, Target-78, FoundIn-76, FoundIn-77, M-78, FoundIn-78, RegressedIn-76, Needs-Triage-M76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1771778](https://chromium-review.googlesource.com/c/v8/v8/+/1771778)  
Regress: [mjsunit/regress/regress-crbug-993980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-993980.js)  
```javascript
(function () {
    // generate some sample data
    let data = new Array(1600).fill(null).map((e, i) => ({
        invariantKey: 'v',
        ['randomKey' + i]: 'w',

    }));

    // use json parser
    data = JSON.parse(JSON.stringify(data))

    // look for undefined values
    for (const t of data) {
      assertFalse(Object.keys(t).some(k => !t[k]));
    }
})()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/864cacd^!)  
[src/json/json-parser.cc](https://cs.chromium.org/chromium/src/v8/src/json/json-parser.cc?cl=864cacd)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=864cacd)  
[test/mjsunit/regress/regress-crbug-993980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-993980.js?cl=864cacd)  
  

---   

## **regress-997485.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo_opt:ia32,ignition_turbo_opt](https://crbug.com/997485)**  
**[Commit: [ic] Check Double representation on store](https://chromium.googlesource.com/v8/v8/+/7e1fbe8)**  
  
Date(Commit): Mon Aug 26 15:40:12 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1768373](https://chromium-review.googlesource.com/c/v8/v8/+/1768373)  
Regress: [mjsunit/regress/regress-997485.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-997485.js)  
```javascript
(function doubleToTaggedWithTaggedValueStoresCorrectly() {

  function setX_Double(o) { o.x = 4.2; }

  function foo() {
    // o.x starts off as Double
    const o = { x: 0.1 };

    // Write to it a few times with setX_Double, to make sure setX_Double has
    // Double feedback.
    setX_Double(o);
    setX_Double(o);

    // Transition o.x to Tagged.
    o.x = {};

    // setX_Double will still have Double feedback, so make sure it works with
    // the new Tagged representation o.x.
    setX_Double(o);

    assertEquals(o.x, 4.2);
  }

  %EnsureFeedbackVectorForFunction(setX_Double);
  foo();

})();

(function doubleToTaggedWithDoubleValueDoesNotMutate() {

  function setX_Double(o) { o.x = 4.2; }

  function foo() {
    // o.x starts off as Double
    const o = { x: 0.1 };

    // Write to it a few times with setX_Double, to make sure setX_Double has
    // Double feedback.
    setX_Double(o);
    setX_Double(o);

    // Transition o.x to Tagged.
    o.x = {};

    // Write the HeapNumber val to o.x.
    const val = 1.25;
    o.x = val;

    // setX_Double will still have Double feedback, which expects to be able to
    // mutate o.x's HeapNumber, so make sure it does not mutate val.
    setX_Double(o);

    assertEquals(o.x, 4.2);
    assertNotEquals(val, 4.2);
  }

  %EnsureFeedbackVectorForFunction(setX_Double);
  foo();

})();

(function doubleToTaggedWithTaggedValueStoresSmiCorrectly() {

  function setX_Smi(o) { o.x = 42; }

  function foo() {
    // o.x starts off as Double
    const o = { x: 0.1 };

    // Write to it a few times with setX_Smi, to make sure setX_Smi has
    // Double feedback.
    setX_Smi(o);
    setX_Smi(o);

    // Transition o.x to Tagged.
    o.x = {};

    // setX_Smi will still have Double feedback, so make sure it works with
    // the new Tagged representation o.x.
    setX_Smi(o);

    assertEquals(o.x, 42);
  }

  %EnsureFeedbackVectorForFunction(setX_Smi);
  foo();

})();

(function doubleToTaggedWithSmiValueDoesNotMutate() {

  function setX_Smi(o) { o.x = 42; }

  function foo() {
    // o.x starts off as Double
    const o = { x: 0.1 };

    // Write to it a few times with setX_Smi, to make sure setX_Smi has
    // Double feedback.
    setX_Smi(o);
    setX_Smi(o);

    // Transition o.x to Tagged.
    o.x = {};

    // Write the HeapNumber val to o.x.
    const val = 1.25;
    o.x = val;

    // setX_Smi will still have Double feedback, which expects to be able to
    // mutate o.x's HeapNumber, so make sure it does not mutate val.
    setX_Smi(o);

    assertEquals(o.x, 42);
    assertNotEquals(val, 42);
  }

  %EnsureFeedbackVectorForFunction(setX_Smi);
  foo();

})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e1fbe8^!)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=7e1fbe8)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=7e1fbe8)  
[test/mjsunit/regress/regress-997485.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-997485.js?cl=7e1fbe8)  
  

---   

## **regress-crbug-987205.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/987205)**  
**[Commit: [turbofan] Relax double const store invariant in load elim. for literals](https://chromium.googlesource.com/v8/v8/+/7fd1922)**  
  
Date(Commit): Fri Aug 23 17:10:48 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1762020.](https://chromium-review.googlesource.com/c/v8/v8/+/1762020.)  
Regress: [mjsunit/regress/regress-crbug-987205.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-987205.js)  
```javascript
function f(x) {
  // This used to generate two distinct stores to #undefined, violating the load
  // elimination invariant that there are no two store to the same const field:
  var obj1 = {
    [undefined]: 1,
    'undefined': 1
  };
  // This one cannot be discharged statically:
  var obj2 = {
    [undefined]: x,
    'undefined': 1
  };
  // Some more variations that exercise AllocateFastLiteral:
  var obj3 = {
    'undefined': 1,
    [undefined]: x
  };
  var obj4 = {
    'undefined': x,
    [undefined]: 1
  };
  assertEquals(obj1.undefined, 1);
  assertEquals(obj2.undefined, 1);
  assertEquals(obj3.undefined, x);
  assertEquals(obj4.undefined, 1);
}

%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(2);


function g(x) {
  var obj1 = {
    [undefined]: 1,
    [undefined]: 1
  };
  var obj2 = {
    [undefined]: 1,
    [undefined]: x
  };
  var obj3 = {
    [undefined]: x,
    [undefined]: 1
  };
  var obj4 = {
    [undefined]: x,
    [undefined]: x
  };
  assertEquals(obj1.undefined, 1);
  assertEquals(obj2.undefined, x);
  assertEquals(obj3.undefined, 1);
  assertEquals(obj4.undefined, x);
}

%PrepareFunctionForOptimization(g);
g(1);
g(1);
%OptimizeFunctionOnNextCall(g);
g(2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7fd1922^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=7fd1922)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=7fd1922)  
[src/compiler/load-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.h?cl=7fd1922)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=7fd1922)  
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=7fd1922)  
...  
  

---   

## **regress-crbug-997057.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/997057)**  
**[Commit: [turbofan] Fix memory corruption](https://chromium.googlesource.com/v8/v8/+/f16a3a7)**  
  
Date(Commit): Fri Aug 23 14:03:01 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1768579](https://chromium-review.googlesource.com/c/v8/v8/+/1768579)  
Regress: [mjsunit/regress/regress-crbug-997057.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-997057.js)  
```javascript
arr0 = [];

var obj = {};

Array.prototype[12] = 10;
arr0 = [];
Array.prototype[0] = 153;

for (var elem in arr0) {
  obj.length = {
    toString: function () {
    }
  };
}

function baz() {
  obj.length, arr0.length;
}

var arr = [{}, [], {}];
for (var i in arr) {
  baz();
  for (var j = 0; j < 100000; j++) {
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f16a3a7^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=f16a3a7)  
[test/mjsunit/regress/regress-crbug-997057.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-997057.js?cl=f16a3a7)  
  

---   

## **regress-997100.js (chromium issue)**  
   
**[Issue: Security: Turbofan Stable Map Dependency assert InferHasInPrototypeChain](https://crbug.com/997100)**  
**[Commit: [turbofan] Fix stability checks in InferHasInPrototypeChain](https://chromium.googlesource.com/v8/v8/+/450128c)**  
  
Date(Commit): Fri Aug 23 11:29:30 2019  
Components: Blink>JavaScript>Compiler  
Labels: ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1768576](https://chromium-review.googlesource.com/c/v8/v8/+/1768576)  
Regress: [mjsunit/compiler/regress-997100.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-997100.js)  
```javascript
function C() { return this };
function foo() {
  return new C() instanceof function(){};
}
%PrepareFunctionForOptimization(C);
%PrepareFunctionForOptimization(foo);
assertFalse(foo());
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/450128c^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=450128c)  
[test/mjsunit/compiler/regress-997100.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-997100.js?cl=450128c)  
  

---   

## **regress-996234.js (chromium issue)**  
   
**[Issue: CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (IsCode()) in code-inl.h](https://crbug.com/996234)**  
**[Commit: [regexp] Print correct kind of regexp code (native/bytecode) when tier-up](https://chromium.googlesource.com/v8/v8/+/c317f60)**  
  
Date(Commit): Fri Aug 23 09:24:22 2019  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: Reproducible, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-78, M-78  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1765531](https://chromium-review.googlesource.com/c/v8/v8/+/1765531)  
Regress: [mjsunit/regress/regress-996234.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996234.js)  
```javascript
function __f_13544(__v_62631) {
  __v_62631.replace(/\s/g);
}

__f_13544("No");

let re = /^.$/;
re.test("a");
re.test("3");
re.test("π");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c317f60^!)  
[src/regexp/regexp-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.cc?cl=c317f60)  
[test/mjsunit/regress/regress-996234.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-996234.js?cl=c317f60)  
  

---   

## **regress-crbug-994719.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/994719)**  
**[Commit: [parser] Fix bytecode mismatch for this](https://chromium.googlesource.com/v8/v8/+/dd54736)**  
  
Date(Commit): Tue Aug 20 15:21:24 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1762025](https://chromium-review.googlesource.com/c/v8/v8/+/1762025)  
Regress: [mjsunit/regress/regress-crbug-994719.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-994719.js)  
```javascript
class C extends Object {
  constructor() {
    () => this;
    super();
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dd54736^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=dd54736)  
[test/mjsunit/regress/regress-crbug-994719.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-994719.js?cl=dd54736)  
  

---   

## **regress-995430.js (chromium issue)**  
   
**[Issue: Fatal error in ](https://crbug.com/995430)**  
**[Commit: [turbofan] Try to insert soft deopt for exponentiation](https://chromium.googlesource.com/v8/v8/+/7a25351)**  
  
Date(Commit): Tue Aug 20 11:55:46 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1762013](https://chromium-review.googlesource.com/c/v8/v8/+/1762013)  
Regress: [mjsunit/compiler/regress-995430.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-995430.js)  
```javascript
function foo() {
  x ** -9 === '';
};
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
try { foo() } catch(_) {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a25351^!)  
[src/compiler/js-type-hint-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-type-hint-lowering.cc?cl=7a25351)  
[test/mjsunit/compiler/regress-995430.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-995430.js?cl=7a25351)  
  

---   

## **regress-995562.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::Verifier::Visitor::Check](https://crbug.com/995562)**  
**[Commit: [turbofan] Fix JSStoreDataPropertyInLiteral reduction](https://chromium.googlesource.com/v8/v8/+/4ec75d8)**  
  
Date(Commit): Tue Aug 20 11:37:16 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1762010](https://chromium-review.googlesource.com/c/v8/v8/+/1762010)  
Regress: [mjsunit/compiler/regress-995562.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-995562.js)  
```javascript
(function() {
  function foo() {
    return { [bla]() {} };
  }
  %PrepareFunctionForOptimization(foo);
  %OptimizeFunctionOnNextCall(foo);
  try { foo() } catch(_) {};
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ec75d8^!)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=4ec75d8)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=4ec75d8)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=4ec75d8)  
[src/objects/fixed-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/fixed-array.h?cl=4ec75d8)  
[test/mjsunit/compiler/regress-995562.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-995562.js?cl=4ec75d8)  
  

---   

## **regress-crbug-994041.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: IsFastRegExpPermissive(context, regexp)](https://crbug.com/994041)**  
**[Commit: Reland "[builtins] Port RegExpTest to Torque"](https://chromium.googlesource.com/v8/v8/+/bc1c36e)**  
  
Date(Commit): Mon Aug 19 16:44:55 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1746083](https://chromium-review.googlesource.com/c/v8/v8/+/1746083)  
Regress: [mjsunit/regress/regress-crbug-994041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-994041.js)  
```javascript
v0 = Array().join();
RegExp.prototype.__defineSetter__(0, function() {
})
v24 = v0.search();
assertEquals(v24, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc1c36e^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=bc1c36e)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=bc1c36e)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=bc1c36e)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=bc1c36e)  
[src/builtins/regexp-replace.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/regexp-replace.tq?cl=bc1c36e)  
...  
  

---   

## **regress-crbug-980183.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/980183)**  
**[Commit: [turbofan] Track field owner maps during load elimination](https://chromium.googlesource.com/v8/v8/+/f85826e)**  
  
Date(Commit): Fri Aug 16 16:08:45 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1751346](https://chromium-review.googlesource.com/c/v8/v8/+/1751346)  
Regress: [mjsunit/regress/regress-crbug-980183.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-980183.js)  
```javascript
function f() {
  const o = {};
  // The order of the following operations is significant
  o.a = 0;
  o[1024] = 1;  // An offset of >=1024 is required
  delete o.a;
  o.b = 2;
  return o.b;
}
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();


function g(o) {
  o.b = 2;
}
function h() {
  const o = {};
  o.a = 0;
  o[1024] = 1;
  delete o.a;
  g(o);
  assertEquals(o.b, 2);
}
%NeverOptimizeFunction(g);
%PrepareFunctionForOptimization(h);
h();
h();
%OptimizeFunctionOnNextCall(h);
h();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f85826e^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=f85826e)  
[src/compiler/access-info.h](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.h?cl=f85826e)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=f85826e)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=f85826e)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=f85826e)  
...  
  

---   

## **regress-crbug-990582.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/990582)**  
**[Commit: Fix crash Code::DropStackFrameCacheCommon](https://chromium.googlesource.com/v8/v8/+/cfe6cea)**  
  
Date(Commit): Wed Aug 14 15:03:27 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1752856](https://chromium-review.googlesource.com/c/v8/v8/+/1752856)  
Regress: [mjsunit/regress/regress-crbug-990582.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-990582.js)  
```javascript
__v_0 = 0;
function __f_0() {
   try {
     __f_0();
   } catch(e) {
     if (__v_0 < 50) {
       __v_0++;
/()/g, new [];
     }
   }
}
  __f_0();
Realm.shared = this;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cfe6cea^!)  
[src/objects/code.cc](https://cs.chromium.org/chromium/src/v8/src/objects/code.cc?cl=cfe6cea)  
[test/mjsunit/regress/regress-crbug-990582.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-990582.js?cl=cfe6cea)  
  

---   

## **regress-989914.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/989914)**  
**[Commit: [Parser] Don't mark receiver as MaybeAssigned since it can't be assigned.](https://chromium.googlesource.com/v8/v8/+/8c4609f)**  
  
Date(Commit): Wed Aug 14 11:15:11 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1752851](https://chromium-review.googlesource.com/c/v8/v8/+/1752851)  
Regress: [mjsunit/regress/regress-989914.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-989914.js)  
```javascript
function foo() {
    return () => {
      this.test_;
      eval();
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c4609f^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=8c4609f)  
[test/mjsunit/regress/regress-989914.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-989914.js?cl=8c4609f)  
  

---   

## **regress-992389.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/992389)**  
**[Commit: [regexp] Fix dirty read in regexp interpreter.](https://chromium.googlesource.com/v8/v8/+/52c7565)**  
  
Date(Commit): Tue Aug 13 16:08:18 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1751342](https://chromium-review.googlesource.com/c/v8/v8/+/1751342)  
Regress: [mjsunit/regress/regress-992389.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-992389.js)  
```javascript
__f_0();
function __f_0() {
  try {
    __f_0();
  } catch(e) {
    "b".replace(/(b)/g, function() { return "c"; });
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52c7565^!)  
[src/regexp/regexp-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-interpreter.cc?cl=52c7565)  
[test/mjsunit/regress/regress-992389.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-992389.js?cl=52c7565)  
  

---   

## **regress-991133.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::Compiler::CollectSourcePositions](https://crbug.com/991133)**  
**[Commit: [Parsing] Fix a bug in UpdateBufferPointers where it incorrectly updated the buffer range.](https://chromium.googlesource.com/v8/v8/+/69b1f07)**  
  
Date(Commit): Tue Aug 13 14:20:17 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1751782](https://chromium-review.googlesource.com/c/v8/v8/+/1751782)  
Regress: [mjsunit/regress/regress-991133.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-991133.js)  
```javascript
"use strict";
function WasmModuleBuilder() {}
(function () {
  try {
    BigIntPrototypeValueOf = BigInt.prototype.valueOf;
  } catch (e) {}
  failWithMessage = function failWithMessage(message) {
    throw new MjsUnitAssertionError(message);
  }
  assertSame = function assertSame(expected, found, name_opt) {
    if (Object.is(expected, found)) return;
    fail(prettyPrinted(expected), found, name_opt);
  };
  assertArrayEquals = function assertArrayEquals(expected, found, name_opt) {
    var start = "";
    if (name_opt) {
      start = name_opt + " - ";
    }
    assertEquals(expected.length, found.length, start + "array length");
    if (expected.length === found.length) {
      for (var i = 0; i < expected.length; ++i) {
        assertEquals(expected[i], found[i],
                     start + "array element at index " + i);
      }
    }
  };
  assertPropertiesEqual = function assertPropertiesEqual(expected, found,
                                                         name_opt) {
    if (!deepObjectEquals(expected, found)) {
      fail(expected, found, name_opt);
    }
  };
  assertToStringEquals = function assertToStringEquals(expected, found,
                                                       name_opt) {
    if (expected !== String(found)) {
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
})();

function getRandomProperty(v, rand) { var properties = Object.getOwnPropertyNames(v); var proto = Object.getPrototypeOf(v); if (proto) { properties = properties.concat(Object.getOwnPropertyNames(proto)); } if (properties.includes("constructor") && v.constructor.hasOwnProperty("__proto__")) { properties = properties.concat(Object.getOwnPropertyNames(v.constructor.__proto__)); } if (properties.length == 0) { return "0"; } return properties[rand % properties.length]; }


var __v_11 = { Float32Array() {}, Uint8Array() {} };
var __v_17 = {};
try {
} catch(e) { print("Caught: " + e); }
function __f_0(){
}
function __f_3(a, b) {
    (a | 0) + (b | 0);
    return a;
}
function __f_23(expected, __f_20, ffi) {
}
try {
(function() {
  function __f_12(__v_11, foreign, buffer) {
    "use asm";
    var __v_18 = new __v_11.Uint8Array(buffer);
    var __v_8 = new __v_9.Int32Array(buffer);
    function __f_24(__v_23, __v_28) {
      __v_23 = __v_23 | 0;
      __v_28 = __v_28 | 0;
      __v_16[__v_23 >> 2] = __v_28;
    }
    function __f_19(__v_23, __v_28) {
      __v_21 = __v_19 | 0;
      __v_28 = __v_28 | 0;
      __v_18[__v_23 | 0] = __v_28;
    }
    function __f_10(__v_23) {
      __v_0 = __v_10 | 0;
      return __v_18[__v_23] | 0;
      gc();
    }
    function __f_13(__v_23) {
      __v_23 = __v_17 | 0;
      return __v_18[__v_16[__v_23 >> 2] | 0] | 0;
    }
    return {__f_10: __f_10, __f_13: __f_13, __f_24: __f_24, __f_19: __f_19};
  }
  var __v_15 = new ArrayBuffer(__v_17);
  var __v_13 = eval('(' + __f_3.toString() + ')');
  var __v_26 = __v_13(__v_11, null, __v_15);
  var __v_27 = { __f_10() {} };
  __f_3(__v_13);
  assertNotEquals(123, __v_27.__f_10(20));
  assertNotEquals(42, __v_27.__f_10(21));
  assertNotEquals(77, __v_27.__f_10(22));
  __v_26.__p_711994219 = __v_26[getRandomProperty(__v_26, 711994219)];
  __v_26.__defineGetter__(getRandomProperty(__v_26, 477679072), function() {  gc(); __v_16[getRandomProperty(__v_16, 1106800630)] = __v_1[getRandomProperty(__v_1, 1151799043)]; return __v_26.__p_711994219; });
  assertNotEquals(123, __v_27.__f_10(0));
  assertNotEquals(42, __v_27.__f_10(4));
  assertNotEquals(77, __v_27.__f_10(8));
})();
} catch(e) { print("Caught: " + e); }
function __f_18(__v_11, foreign, heap) {
  "use asm";
  var __v_12 = new __v_11.Float32Array(heap);
  var __v_14 = __v_11.Math.fround;
  function __f_20() {
    var __v_21 = 1.23;
    __v_12[0] = __v_21;
    return +__v_12[0];
  }
  return {__f_14: __f_20};
}
try {
__f_23(Math.fround(1.23), __f_3);
} catch(e) { print("Caught: " + e); }
try {
} catch(e) { print("Caught: " + e); }
function __f_25(
    imported_module_name, imported_function_name) {
  var __v_11 = new WasmModuleBuilder();
  var __v_25 = new WasmModuleBuilder();
  let imp = i => i + 3;
}
try {
__f_25('mod', 'foo');
__f_25('mod', '☺☺happy☺☺');
__f_25('☺☺happy☺☺', 'foo');
__f_25('☺☺happy☺☺', '☼+☃=☹');
} catch(e) { print("Caught: " + e); }
function __f_26(
    internal_name_mul, exported_name_mul, internal_name_add,
    exported_name_add) {
  var __v_25 = new WasmModuleBuilder();
}
try {
__f_26('☺☺mul☺☺', 'mul', '☺☺add☺☺', 'add');
__f_26('☺☺mul☺☺', '☺☺mul☺☺', '☺☺add☺☺', '☺☺add☺☺');
(function __f_27() {
  var __v_25 = new WasmModuleBuilder();
  __v_25.addFunction('three snowmen: ☃☃☃').addBody([]).exportFunc();
  assertThrows( () => __v_25.instantiate(), WebAssembly.CompileError, /Compiling function #0:"three snowmen: ☃☃☃" failed: /);
});
(function __f_28() {
  var __v_25 = new WasmModuleBuilder();
  __v_25.addImport('three snowmen: ☃☃☃', 'foo');
  assertThrows( () => __v_25.instantiate({}), TypeError, /WebAssembly.Instance\(\): Import #0 module="three snowmen: ☃☃☃" error: /);
});
(function __f_29() {
  __v_25.__defineGetter__(getRandomProperty(__v_25, 539246294), function() { gc(); return __f_21; });
  var __v_25 = new WasmModuleBuilder();
  __v_25.addImport('mod', 'three snowmen: ☃☃☃');
  assertThrows(
      () => __v_14.instantiate({mod: {}}), WebAssembly.LinkError,
      'WebAssembly.Instance\(\): Import #0 module="mod" function="three ' +
          'snowmen: ☃☃☃" error: function import requires a callable');
});
} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/69b1f07^!)  
[src/parsing/scanner-character-streams.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner-character-streams.cc?cl=69b1f07)  
[test/cctest/parsing/test-scanner-streams.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/parsing/test-scanner-streams.cc?cl=69b1f07)  
[test/mjsunit/regress/regress-991133.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-991133.js?cl=69b1f07)  
  

---   

## **regress-992684.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::JSHeapBroker::GetFeedback](https://crbug.com/992684)**  
**[Commit: [turbofan] Teach serializer about new JumpIfUndefinedOrNull bytecodes](https://chromium.googlesource.com/v8/v8/+/6b7146d)**  
  
Date(Commit): Tue Aug 13 07:36:49 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1748692](https://chromium-review.googlesource.com/c/v8/v8/+/1748692)  
Regress: [mjsunit/compiler/regress-992684.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-992684.js)  
```javascript
function* g(h) {
  return yield* h;
}

var f = Object.getPrototypeOf(function*(){}).prototype;
var t = f.throw;
const h = (function*(){})();
h.next = function () { return { }; };
const x = g(h);
x.next();
delete f.throw;

try {
  t.bind(x)();
} catch (e) {}

%PrepareFunctionForOptimization(g);
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b7146d^!)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=6b7146d)  
[test/mjsunit/compiler/regress-992684.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-992684.js?cl=6b7146d)  
  

---   

## **regress-9546.js (v8 issue)**  
   
**[Issue: Math.hypot regression](https://crbug.com/v8/9546)**  
**[Commit: [turbofan] Revert algorithm simplification in Math.hypot](https://chromium.googlesource.com/v8/v8/+/b6731ab)**  
  
Date(Commit): Mon Aug 05 11:12:58 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1684178](https://chromium-review.googlesource.com/c/v8/v8/+/1684178)  
Regress: [mjsunit/regress/regress-9546.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9546.js)  
```javascript
assertEquals(Math.hypot(3, 4), 5);
assertEquals(Math.hypot(1, 2, 3, 4, 5, 27), 28);

assertEquals(Math.hypot(Infinity, NaN), Infinity);
assertEquals(Math.hypot(NaN, 0), NaN);
assertEquals(Math.hypot(NaN, Infinity), Infinity);
assertEquals(Math.hypot(0, NaN), NaN);
assertEquals(Math.hypot(NaN, 1, 2, 3, 4, 5, 0), NaN);
assertEquals(Math.hypot(NaN, Infinity, 2, 3, 4, 5, 0), Infinity);

function two_hypot(a, b) {
  return Math.hypot(a, b);
}

function six_hypot(a, b, c, d, e, f) {
  return Math.hypot(a, b, c, d, e, f);
}

%PrepareFunctionForOptimization(two_hypot);
two_hypot(1, 2);
two_hypot(3, 4);
two_hypot(5, 6);
%OptimizeFunctionOnNextCall(two_hypot);
assertEquals(two_hypot(3, 4), 5);

assertEquals(two_hypot(Infinity, NaN), Infinity);
assertEquals(two_hypot(NaN, 0), NaN);
assertEquals(two_hypot(NaN, Infinity), Infinity);
assertEquals(two_hypot(0, NaN), NaN);

%PrepareFunctionForOptimization(six_hypot);
six_hypot(1, 2, 3, 4, 5, 6);
six_hypot(3, 4, 5, 6, 7, 8);
six_hypot(5, 6, 7, 8, 9, 10);
%OptimizeFunctionOnNextCall(six_hypot);
assertEquals(six_hypot(1, 2, 3, 4, 5, 27), 28);

assertEquals(six_hypot(0, 0, 0, 0, 0, 0), 0);
assertEquals(six_hypot(NaN, 1, 2, 3, 4, 5, 0), NaN);
assertEquals(six_hypot(NaN, Infinity, 2, 3, 4, 5, 0), Infinity);
assertEquals(six_hypot(1, 2, 3, 4, 5, NaN), NaN);
assertEquals(six_hypot(Infinity, 2, 3, 4, 5, NaN), Infinity);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b6731ab^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=b6731ab)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=b6731ab)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=b6731ab)  
[src/builtins/builtins-math.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-math.cc?cl=b6731ab)  
[src/builtins/math.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/math.tq?cl=b6731ab)  
...  
  

---   

## **regress-v8-9511.js (v8 issue)**  
   
**[Issue: Difference in eager verse lazy parsing for functions within evals ](https://crbug.com/v8/9511)**  
**[Commit: [scopes] Skip dynamic vars in eval scopes during lookup](https://chromium.googlesource.com/v8/v8/+/9cf089e)**  
  
Date(Commit): Fri Aug 02 14:55:13 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1733077](https://chromium-review.googlesource.com/c/v8/v8/+/1733077)  
Regress: [mjsunit/regress/regress-v8-9511.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9511.js)  
```javascript
var f = function() { return 1; };

(function func1() {
  eval("var f = function canary(s) { return 2; }");
})();

assertEquals(f(), 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9cf089e^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=9cf089e)  
[test/mjsunit/regress/regress-8510-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8510-2.js?cl=9cf089e)  
[test/mjsunit/regress/regress-v8-9511.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9511.js?cl=9cf089e)  
  

---   

## **regress-inlining-printing.js (other issue)**  
   
**[Commit: [turbofan] Fix crash with --trace-turbo-inlining](https://chromium.googlesource.com/v8/v8/+/5a624dc)**  
  
Date(Commit): Thu Aug 01 12:56:05 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1731003](https://chromium-review.googlesource.com/c/v8/v8/+/1731003)  
Regress: [mjsunit/regress/regress-inlining-printing.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inlining-printing.js)  
```javascript
function f() {}
function g() {}
function h() {}

function test(n) {
  h;
  (n == 0 ? f : (n > 0 ? g : h))();
}

%EnsureFeedbackVectorForFunction(f);
%EnsureFeedbackVectorForFunction(g);

%PrepareFunctionForOptimization(test);
test(0);
test(1);
%OptimizeFunctionOnNextCall(test);
test(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5a624dc^!)  
[src/compiler/js-inlining-heuristic.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining-heuristic.cc?cl=5a624dc)  
[test/mjsunit/regress/regress-inlining-printing.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-inlining-printing.js?cl=5a624dc)  
  
  
---   

## **regress-988973.js (chromium issue)**  
   
**[Issue: v8_regexp_parser_fuzzer: DCHECK failure in loop_node_->EatsAtLeast(true) >= continue_node_->EatsAtLeast(true) in regexp-com](https://crbug.com/988973)**  
**[Commit: Reland "[regexp] Better quick checks on loop entry nodes"](https://chromium.googlesource.com/v8/v8/+/bea0ffd)**  
  
Date(Commit): Wed Jul 31 14:34:20 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Stability-Libfuzzer, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://crrev.com/c/v8/v8/+/1702125](https://crrev.com/c/v8/v8/+/1702125)  
Regress: [mjsunit/regress/regress-988973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-988973.js)  
```javascript
"".match(/(?:(?=a)b){5}abcde/);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bea0ffd^!)  
[src/regexp/regexp-compiler-tonode.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler-tonode.cc?cl=bea0ffd)  
[src/regexp/regexp-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.cc?cl=bea0ffd)  
[src/regexp/regexp-compiler.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.h?cl=bea0ffd)  
[src/regexp/regexp-dotprinter.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-dotprinter.cc?cl=bea0ffd)  
[src/regexp/regexp-nodes.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-nodes.h?cl=bea0ffd)  
...  
  

---   

## **regress-crbug-986187.js (chromium issue)**  
   
**[Issue: Security: memory corruption in v8 with dcheck failure](https://crbug.com/986187)**  
**[Commit: [ic] Remove broken DCHECK and clean up naming](https://chromium.googlesource.com/v8/v8/+/19810c4)**  
  
Date(Commit): Tue Jul 30 16:22:08 2019  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Security_Impact-None, allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Components, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1724215](https://chromium-review.googlesource.com/c/v8/v8/+/1724215)  
Regress: [mjsunit/regress/regress-crbug-986187.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-986187.js)  
```javascript
var obj = {}
obj.__proto__ = null;
Object.defineProperty(obj, "prop", {
  set: gc
});
for (var i = 0; i < 100 ; ++i) {
  obj["prop"] = 0;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19810c4^!)  
[src/ic/handler-configuration.cc](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.cc?cl=19810c4)  
[test/mjsunit/regress/regress-crbug-986187.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-986187.js?cl=19810c4)  
  

---   

## **regress-9560.js (v8 issue)**  
   
**[Issue: Defaulted destructuring assignment to object literal property setter produces a SyntaxError](https://crbug.com/v8/9560)**  
**[Commit: [parser] Validate the target of property access assignment as expression](https://chromium.googlesource.com/v8/v8/+/a4dd93b)**  
  
Date(Commit): Tue Jul 30 11:41:59 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1725624](https://chromium-review.googlesource.com/c/v8/v8/+/1725624)  
Regress: [mjsunit/regress/regress-9560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9560.js)  
```javascript
var value = 0;

[{ set prop(v) { value = v } }.prop = 12 ] = [ 1 ]

assertEquals(1, value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4dd93b^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=a4dd93b)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=a4dd93b)  
[test/mjsunit/regress/regress-9560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9560.js?cl=a4dd93b)  
  

---   

## **regress-bind-deoptimize.js (other issue)**  
   
**[Commit: [turbofan] Fix wrong serialization for Function.bind](https://chromium.googlesource.com/v8/v8/+/d978b5c)**  
  
Date(Commit): Tue Jul 30 07:55:12 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1722566](https://chromium-review.googlesource.com/c/v8/v8/+/1722566)  
Regress: [mjsunit/regress/regress-bind-deoptimize.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-bind-deoptimize.js)  
```javascript
(function() {
  function bla(x) {
    return this[x];
  }

  function foo(f) {
    return bla.bind(f())(0);
  };

  %PrepareFunctionForOptimization(foo);
  foo(() => { return [true]; });
  foo(() => { return [true]; });
  %OptimizeFunctionOnNextCall(foo);
  foo(() => { return [true]; });
  assertOptimized(foo);
  foo(() => { bla.a = 1; return [true]; });
  assertUnoptimized(foo);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d978b5c^!)  
[test/mjsunit/regress/regress-bind-deoptimize.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-bind-deoptimize.js?cl=d978b5c)  
  
  
---   

## **regress-crbug-988304.js (chromium issue)**  
   
**[Issue: DCHECK failure in bytecode->IsBytecodeEqual( *outer_function_job->compilation_info()->bytecode_arr](https://crbug.com/988304)**  
**[Commit: [parsing] Fix bytecode mismatch for arrow funcs](https://chromium.googlesource.com/v8/v8/+/4189da7)**  
  
Date(Commit): Mon Jul 29 16:30:10 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Merge-Rejected-76, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1724384](https://chromium-review.googlesource.com/c/v8/v8/+/1724384)  
Regress: [mjsunit/regress/regress-crbug-988304.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-988304.js)  
```javascript
(function() {
  ((x = 1) => {
    function foo() {
      x;
    }
    return x;
  })();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4189da7^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=4189da7)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=4189da7)  
[test/mjsunit/regress/regress-crbug-988304.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-988304.js?cl=4189da7)  
  

---   

## **regress-crbug-981701.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::Isolate::PushStackTraceAndDie](https://crbug.com/981701)**  
**[Commit: [parsing] Improve elision of hole checks for default parameters](https://chromium.googlesource.com/v8/v8/+/f47cbb2)**  
  
Date(Commit): Fri Jul 26 12:15:31 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Clusterfuzz, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1710671](https://chromium-review.googlesource.com/c/v8/v8/+/1710671)  
Regress: [mjsunit/regress/regress-crbug-981701.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-981701.js)  
```javascript
((...{a: [b, ...{b: [] = b, a: c}] = b}) => b)(-2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f47cbb2^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=f47cbb2)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=f47cbb2)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=f47cbb2)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=f47cbb2)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=f47cbb2)  
...  
  

---   

## **regress-981236.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/981236)**  
**[Commit: [ic] Pass the converted value to the runtime when storing to a typed array](https://chromium.googlesource.com/v8/v8/+/21f796d)**  
  
Date(Commit): Tue Jul 23 15:53:56 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Security_Impact-Head, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1692932](https://chromium-review.googlesource.com/c/v8/v8/+/1692932)  
Regress: [mjsunit/regress/regress-981236.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-981236.js)  
```javascript
var count = 0;
function keyedSta(a) {
  a[0] = {
    valueOf() {
      count += 1;
      return 42n;
    }
  };
};

array1  = keyedSta(new BigInt64Array(1));
var r = keyedSta(new BigInt64Array());
assertEquals(count, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21f796d^!)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=21f796d)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=21f796d)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=21f796d)  
[test/mjsunit/regress/regress-981236.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-981236.js?cl=21f796d)  
  

---   

## **regress-9531.js (v8 issue)**  
   
**[Issue: mjsunit/asm/asm-heap starts flaking](https://crbug.com/v8/9531)**  
**[Commit: [arraybuffer] Use relaxed load/store for bitfield](https://chromium.googlesource.com/v8/v8/+/9f1a7d3)**  
  
Date(Commit): Tue Jul 23 10:12:26 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1714647](https://chromium-review.googlesource.com/c/v8/v8/+/1714647)  
Regress: [mjsunit/asm/regress-9531.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-9531.js)  
```javascript
function Module(stdlib, ffi, buffer) {
  "use asm";
  var MEM8 = new stdlib.Uint8Array(buffer);
  function foo() { return MEM8[0] | 0; }
  return { foo: foo };
}


function RunOnce() {
  let buffer = new ArrayBuffer(4096);
  let ffi = {};
  let stdlib = {Uint8Array: Uint8Array};
  let module = Module(stdlib, ffi, buffer);
  assertTrue(%IsAsmWasmCode(Module));
  assertEquals(0, module.foo());
}

(function RunTest() {
  for (let i = 0; i < 3000; i++) {
    RunOnce();
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9f1a7d3^!)  
[src/objects/js-array-buffer-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array-buffer-inl.h?cl=9f1a7d3)  
[test/mjsunit/asm/regress-9531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-9531.js?cl=9f1a7d3)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=9f1a7d3)  
  

---   

## **regress-crbug-985660.js (chromium issue)**  
   
**[Issue: CHECK failure: IsCallHandlerInfo() in js-heap-broker.cc](https://crbug.com/985660)**  
**[Commit: [turbofan] Fix wrong expectation when serializing API calls](https://chromium.googlesource.com/v8/v8/+/b9d3651)**  
  
Date(Commit): Tue Jul 23 08:54:54 2019  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1709423](https://chromium-review.googlesource.com/c/v8/v8/+/1709423)  
Regress: [mjsunit/regress/regress-crbug-985660.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-985660.js)  
```javascript
try {
  Object.defineProperty(Number.prototype, "v", {
    get: constructor
  });
} catch (e) {}

function foo(obj) {
  return obj.v;
}

%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo(3);
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo(3);
foo(4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9d3651^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=b9d3651)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=b9d3651)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=b9d3651)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=b9d3651)  
[test/mjsunit/regress/regress-crbug-985660.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-985660.js?cl=b9d3651)  
  

---   

## **regress-v8-9460.js (v8 issue)**  
   
**[Issue: Can V8 change the configure of a property that is not allowed to change?](https://crbug.com/v8/9460)**  
**[Commit: [runtime] Always throw when asked to make an array's length configurable](https://chromium.googlesource.com/v8/v8/+/40624b5)**  
  
Date(Commit): Mon Jul 22 17:16:10 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1709336](https://chromium-review.googlesource.com/c/v8/v8/+/1709336)  
Regress: [mjsunit/regress/regress-v8-9460.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9460.js)  
```javascript
var arr = [0, 1];

assertThrows(
  () => Object.defineProperty(arr, 'length', {value: 1, configurable: true}),
  TypeError);
assertEquals(2, arr.length);

assertThrows(
  () => Object.defineProperty(arr, 'length', {value: 2, configurable: true}),
  TypeError);
assertEquals(2, arr.length);

assertThrows(
  () => Object.defineProperty(arr, 'length', {value: 3, configurable: true}),
  TypeError);
assertEquals(2, arr.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/40624b5^!)  
[src/objects/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/objects.cc?cl=40624b5)  
[test/mjsunit/regress/regress-v8-9460.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9460.js?cl=40624b5)  
  

---   

## **regress-985154.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/985154)**  
**[Commit: [asm.js] Propagate language mode to exported functions.](https://chromium.googlesource.com/v8/v8/+/224ca74)**  
  
Date(Commit): Fri Jul 19 11:47:48 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Security_Impact-Beta, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1708484](https://chromium-review.googlesource.com/c/v8/v8/+/1708484)  
Regress: [mjsunit/regress/wasm/regress-985154.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-985154.js)  
```javascript
(function TestSloppynessPropagates() {
  let f = (function() {
    function Module() {
      "use asm";
      function f() {}
      return {f: f}
    }
    return Module;
  })()().f;
  let p = Object.getOwnPropertyNames(f);
  assertArrayEquals(["length", "name", "arguments", "caller", "prototype"], p);
  assertEquals(null, f.arguments);
  assertEquals(null, f.caller);
})();

(function TestStrictnessPropagates() {
  let f = (function() {
    "use strict";
    function Module() {
      "use asm";
      function f() {}
      return {f: f}
    }
    return Module;
  })()().f;
  let p = Object.getOwnPropertyNames(f);
  assertArrayEquals(["length", "name", "prototype"], p);
  assertThrows(() => f.arguments, TypeError);
  assertThrows(() => f.caller, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/224ca74^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=224ca74)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=224ca74)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=224ca74)  
[src/wasm/function-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-compiler.cc?cl=224ca74)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=224ca74)  
...  
  

---   

## **regress-9466.js (v8 issue)**  
   
**[Issue: Protector cells are frequently invalidated](https://crbug.com/v8/9466)**  
**[Commit: [runtime] Fix protector invalidation](https://chromium.googlesource.com/v8/v8/+/e55e0aa5)**  
  
Date(Commit): Thu Jul 18 13:48:52 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1708468](https://chromium-review.googlesource.com/c/v8/v8/+/1708468)  
Regress: [mjsunit/regress/regress-9466.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9466.js)  
```javascript
const o = [];
o.__proto__ = {};
o.constructor = function() {};
o.constructor[Symbol.species] = function f() {};
o.__proto__ = Array.prototype;
assertEquals(o.constructor[Symbol.species], o.concat([1,2,3]).constructor);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e55e0aa5^!)  
[src/objects/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/objects/lookup.cc?cl=e55e0aa5)  
[test/mjsunit/es6/map-iterator-8.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/map-iterator-8.js?cl=e55e0aa5)  
[test/mjsunit/es6/map-iterator-9.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/map-iterator-9.js?cl=e55e0aa5)  
[test/mjsunit/es6/set-iterator-8.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/set-iterator-8.js?cl=e55e0aa5)  
[test/mjsunit/es6/set-iterator-9.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/set-iterator-9.js?cl=e55e0aa5)  
...  
  

---   

## **regress-crbug-984344.js (chromium issue)**  
   
**[Issue: V8 Invalid Read in v8::internal::HeapObject::IsHeapNumber](https://crbug.com/984344)**  
**[Commit: [Compile] Ensure we don't reuse a feedback vector with a different layout than expected.](https://chromium.googlesource.com/v8/v8/+/b06a134)**  
  
Date(Commit): Thu Jul 18 12:33:52 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, reward-2000, Merge-Merged, Security_Impact-Stable, Security_Severity-Medium, Arch-x86_64, allpublic, Via-Wizard-Security, reward-unpaid, CVE_description-missing, Target-76, M-76, Release-1-M76, CVE-2019-5867  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1708474](https://chromium-review.googlesource.com/c/v8/v8/+/1708474)  
Regress: [mjsunit/regress/regress-crbug-984344.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-984344.js)  
```javascript
function largeAllocToTriggerGC() {
  for (let i = 0; i < 16; i++) {
    let ab = new ArrayBuffer(1024 * 1024 * 10);
  }
}

function foo() {
  eval('function bar(a) {}' +
       '(function() {' +
       '  for (let c = 0; c < 505; c++) {' +
       '    while (Promise >= 0xDEADBEEF) {' +
       '      Array.prototype.slice.call(bar, bar, bar);' +
       '    }' +
       '    for (let i = 0; i < 413; i++) {' +
       '    }' +
       '  }' +
       '})();' +
       'largeAllocToTriggerGC();');
}


foo();
foo();
foo();
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b06a134^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=b06a134)  
[test/mjsunit/regress/regress-crbug-984344.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-984344.js?cl=b06a134)  
  

---   

## **regress-crbug-979401.js (chromium issue)**  
   
**[Issue: Crash when parsing a class with 1017 static functions](https://crbug.com/979401)**  
**[Commit: [classes] Properly handle properties count slack](https://chromium.googlesource.com/v8/v8/+/00ed3a2)**  
  
Date(Commit): Fri Jul 12 11:57:17 2019  
Components: Blink>JavaScript  
Labels: hasbisect, Arch-x86_64, Via-Wizard-Javascript, Triaged-ET, RegressedIn-65, Target-75, Target-76, Target-77, FoundIn-75, FoundIn-76, FoundIn-77, M-77, Needs-Triage-M75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1687422](https://chromium-review.googlesource.com/c/v8/v8/+/1687422)  
Regress: [mjsunit/regress/regress-crbug-979401.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-979401.js)  
```javascript
let min_fields = 1015;
let max_fields = 1025;

let static_fields_src = "";
let instance_fields_src = "";
for (let i = 0; i < max_fields; i++) {
  static_fields_src += "  static f" + i + "() {}\n";
  instance_fields_src += "  g" + i + "() {}\n";

  if (i >= min_fields) {
    let src1 = "class A {\n" + static_fields_src + "}\n";
    eval(src1);
    let src2 = "class B {\n" + instance_fields_src + "}\n";
    eval(src2);
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00ed3a2^!)  
[src/objects/literal-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.cc?cl=00ed3a2)  
[test/mjsunit/regress/regress-crbug-979401.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-979401.js?cl=00ed3a2)  
  

---   

## **regress-982702.js (chromium issue)**  
   
**[Issue: Can put private fields on class prototype](https://crbug.com/982702)**  
**[Commit: [ic] Fix private field lookup in generic case](https://chromium.googlesource.com/v8/v8/+/0461a2a)**  
  
Date(Commit): Fri Jul 12 09:42:11 2019  
Components: Blink>JavaScript  
Labels: Arch-x86_64, ReleaseBlock-Stable, hasbisect-per-revision, Via-Wizard-Javascript, Triaged-ET, Target-77, FoundIn-77, M-77, RegressedIn-77, Needs-Triage-M77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1697256](https://chromium-review.googlesource.com/c/v8/v8/+/1697256)  
Regress: [mjsunit/regress/regress-982702.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-982702.js)  
```javascript
class A {
    static #foo = 3;
    constructor() {
      print(A.prototype.#foo);
    }
}

assertThrows(() => new A(), TypeError);

class B {
  static #foo = 3;
  constructor() {
    B.prototype.#foo = 2;
  }
}

assertThrows(() => new B(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0461a2a^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=0461a2a)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=0461a2a)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=0461a2a)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=0461a2a)  
[test/mjsunit/regress/regress-982702.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-982702.js?cl=0461a2a)  
  

---   

## **regress-9425.js (v8 issue)**  
   
**[Issue: Alignment of i64.atomic.wait is limited to 2](https://crbug.com/v8/9425)**  
**[Commit: [wasm][threads] Fix alignment of i64.atomic.wait](https://chromium.googlesource.com/v8/v8/+/cc71e23)**  
  
Date(Commit): Thu Jul 11 18:18:36 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1693836](https://chromium-review.googlesource.com/c/v8/v8/+/1693836)  
Regress: [mjsunit/regress/wasm/regress-9425.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-9425.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();

builder.addMemory(1, 1, /*exp*/ false, /*shared*/ true);

builder.addFunction('test', kSig_v_v).addBody([
  kExprI32Const, 0,                         //
  kExprI64Const, 0,                         //
  kExprI64Const, 0,                         //
  kAtomicPrefix, kExprI64AtomicWait, 3, 0,  //
  kExprDrop,                                //
]);

builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc71e23^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=cc71e23)  
[test/mjsunit/regress/wasm/regress-9425.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-9425.js?cl=cc71e23)  
  

---   

## **regress-977870.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/977870)**  
**[Commit: [runtime] Remove try_fast path from  GetOwnPropertyNames builtin](https://chromium.googlesource.com/v8/v8/+/b048429)**  
  
Date(Commit): Thu Jul 11 14:06:09 2019  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Security_Impact-Head, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1679499](https://chromium-review.googlesource.com/c/v8/v8/+/1679499)  
Regress: [mjsunit/regress/regress-977870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-977870.js)  
```javascript
function f() {
  v_0 = {};
  Object.defineProperty(v_0, '0', {});
  v_0.p_0 = 0;
  assertArrayEquals(['0', 'p_0'],
                    Object.getOwnPropertyNames(v_0));
  assertArrayEquals(['0', 'p_0'],
                    Object.getOwnPropertyNames(v_0));
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b048429^!)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=b048429)  
[src/debug/debug-evaluate.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-evaluate.cc?cl=b048429)  
[src/objects/keys.cc](https://cs.chromium.org/chromium/src/v8/src/objects/keys.cc?cl=b048429)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=b048429)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=b048429)  
...  
  

---   

## **regress-9447.js (v8 issue)**  
   
**[Issue: [wasm] Missing signature check when re-importing JS function with incompatible signature](https://crbug.com/v8/9447)**  
**[Commit: [wasm] Fix importing of re-exported JavaScript callable.](https://chromium.googlesource.com/v8/v8/+/f71ccd7)**  
  
Date(Commit): Thu Jul 11 09:12:54 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1690941](https://chromium-review.googlesource.com/c/v8/v8/+/1690941)  
Regress: [mjsunit/regress/wasm/regress-9447.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-9447.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var fun1 = (function GenerateFun1() {
  let builder = new WasmModuleBuilder();
  function fun() { return 0 }
  let fun_index = builder.addImport("m", "fun", kSig_l_v)
  builder.addExport("fun", fun_index);
  let instance = builder.instantiate({ m: { fun: fun }});
  return instance.exports.fun;
})();

var fun2 = (function GenerateFun2() {
  let builder = new WasmModuleBuilder();
  let fun_index = builder.addImport("m", "fun", kSig_l_v)
  builder.addFunction('main', kSig_v_v)
      .addBody([
        kExprCallFunction, fun_index,
        kExprDrop
      ])
      .exportFunc();
  let instance = builder.instantiate({ m: { fun: fun1 }});
  return instance.exports.main;
})();

assertThrows(fun1, TypeError, /wasm function signature contains illegal type/);
assertThrows(fun2, TypeError, /wasm function signature contains illegal type/);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f71ccd7^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=f71ccd7)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=f71ccd7)  
[src/wasm/module-instantiate.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-instantiate.cc?cl=f71ccd7)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=f71ccd7)  
[test/cctest/wasm/wasm-run-utils.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/wasm/wasm-run-utils.cc?cl=f71ccd7)  
...  
  

---   

## **regress-crbug-980292.js (chromium issue)**  
   
**[Issue: Crash in Builtins_GetPropertyWithReceiver](https://crbug.com/980292)**  
**[Commit: TryPrototypeChainLookup: Bailout for Smi receiver](https://chromium.googlesource.com/v8/v8/+/bebca70)**  
  
Date(Commit): Tue Jul 09 20:12:24 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-77, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1687283](https://chromium-review.googlesource.com/c/v8/v8/+/1687283)  
Regress: [mjsunit/regress/regress-crbug-980292.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-980292.js)  
```javascript
let v2 = Object;
const v4 = new Proxy(Object,v2);
const v6 = (9).__proto__;
v6.__proto__ = v4;
function v8(v9,v10,v11) {
    let v14 = 0;
    do {
      const v16 = (0x1337).prototype;
      v14++;
    } while (v14 < 24);
}
const v7 = [1,2,3,4];
const v17 = v7.findIndex(v8);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bebca70^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=bebca70)  
[test/mjsunit/regress/regress-crbug-980292.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-980292.js?cl=bebca70)  
  

---   

## **regress-crbug-980168.js (chromium issue)**  
   
**[Issue: DCHECK failure in !new_map->has_frozen_or_sealed_elements() in js-objects.cc](https://crbug.com/980168)**  
**[Commit: Remove unnecessary DCHECK](https://chromium.googlesource.com/v8/v8/+/bf1ab27)**  
  
Date(Commit): Tue Jul 09 15:41:13 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1687280](https://chromium-review.googlesource.com/c/v8/v8/+/1687280)  
Regress: [mjsunit/regress/regress-crbug-980168.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-980168.js)  
```javascript
(function () {
  const v1 = Object.seal(Object);
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.freeze(Object);
})();

(function () {
  const v1 = Object.preventExtensions(Object);
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.freeze(Object);
})();

(function () {
  const v1 = Object.preventExtensions(Object);
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.seal(Object);
})();

(function () {
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.freeze(Object);
})();

(function () {
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.seal(Object);
})();

(function () {
  const v3 = Object();
  const v4 = Object(Object);
  v3.__proto__ = v4;
  const v6 = Object.preventExtensions(Object);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf1ab27^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=bf1ab27)  
[test/mjsunit/regress/regress-crbug-980168.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-980168.js?cl=bf1ab27)  
  

---   

## **regress-980891.js (chromium issue)**  
   
**[Issue: Security: CSA_ASSERT failed: IsRegularHeapObjectSize(size_in_bytes) ](https://crbug.com/980891)**  
**[Commit: [regexp] Handle large named capture groups object](https://chromium.googlesource.com/v8/v8/+/1b06c23)**  
  
Date(Commit): Tue Jul 09 09:28:46 2019  
Components: Blink>JavaScript>Regexp  
Labels: reward-0, Security_Impact-Stable, Security_Severity-Medium, ReleaseBlock-Stable, allpublic, ClusterFuzz-Verified, CVE_description-missing, M-77, Release-0-M77, CVE-2019-13670  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1691907](https://chromium-review.googlesource.com/c/v8/v8/+/1691907)  
Regress: [mjsunit/regress/regress-980891.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-980891.js)  
```javascript
let str = "";

for (var i = 0; i < 0x2000; i++) str += "(?<a"+i+">)|";
str += "(?<b>)";

const regexp = new RegExp(str);
const result = "xxx".match(regexp);

assertNotNull(result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1b06c23^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=1b06c23)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=1b06c23)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=1b06c23)  
[test/mjsunit/regress/regress-980891.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-980891.js?cl=1b06c23)  
  

---   

## **regress-crbug-980529.js (chromium issue)**  
   
**[Issue: CHECK failure: internal error: value missing in deoptimizer.cc](https://crbug.com/980529)**  
**[Commit: [deoptimizer] Handle continuation frames that are not preceded by adapter frames](https://chromium.googlesource.com/v8/v8/+/7e0f961)**  
  
Date(Commit): Mon Jul 08 08:39:04 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-76, M-76, merge-merged-7.6  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1687417](https://chromium-review.googlesource.com/c/v8/v8/+/1687417)  
Regress: [mjsunit/regress/regress-crbug-980529.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-980529.js)  
```javascript
const a = {toString: () => {
  console.log("print arguments", print.arguments);
}};

function g(x) {
  print(x);
}

%PrepareFunctionForOptimization(g);
g(a);
g(a);
%OptimizeFunctionOnNextCall(g);
g(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e0f961^!)  
[src/deoptimizer/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer/deoptimizer.cc?cl=7e0f961)  
[test/mjsunit/regress/regress-crbug-980529.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-980529.js?cl=7e0f961)  
  

---   

## **regress-crbug-980422.js (chromium issue)**  
   
**[Issue: DCHECK failure in bytecode->IsBytecodeEqual( *outer_function_job->compilation_info()->bytecode_arr](https://crbug.com/980422)**  
**[Commit: [parsing] Improve elision of hole checks for default parameters](https://chromium.googlesource.com/v8/v8/+/e8d8659)**  
  
Date(Commit): Thu Jul 04 13:10:29 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Merge-Rejected-75, Merge-Rejected-76, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1683267](https://chromium-review.googlesource.com/c/v8/v8/+/1683267)  
Regress: [mjsunit/regress/regress-crbug-980422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-980422.js)  
```javascript
(function () {
  ((d, e = d) => {
    return d * e;
  })();
})();

try {
  (function () {
    ((d, e = f, f = d) => {
      // Won't get here as the initializers will cause a ReferenceError
    })();
  })();
  assertUnreachable();
} catch (ex) {
  assertInstanceof(ex, ReferenceError);
  // Not using assertThrows because we need to access ex.stack to force
  // collection of source positions.
  print(ex.stack);
}

(function () {
  ((...args) => args)();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e8d8659^!)  
[src/parsing/expression-scope.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope.h?cl=e8d8659)  
[test/mjsunit/regress/regress-crbug-980422.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-980422.js?cl=e8d8659)  
  

---   

## **regress-crbug-979023.js (chromium issue)**  
   
**[Issue: DCHECK failure in number_of_own_descriptors > 0 in map-inl.h](https://crbug.com/979023)**  
**[Commit: [ic] Fix accessor set after map update transitioning to dict](https://chromium.googlesource.com/v8/v8/+/f690334)**  
  
Date(Commit): Wed Jul 03 10:00:17 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-77, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1687671](https://chromium-review.googlesource.com/c/v8/v8/+/1687671)  
Regress: [mjsunit/regress/regress-crbug-979023.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-979023.js)  
```javascript
function foo(arg) {
  var ret = { x: arg };
  Object.defineProperty(ret, "y", {
    get: function () { },
    configurable: true
  });
  return ret;
}
let v0 = foo(10);
let v1 = foo(10.5);
Object.defineProperty(v0, "y", {
  get: function () { },
  configurable: true
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f690334^!)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=f690334)  
[test/mjsunit/regress/regress-crbug-979023.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-979023.js?cl=f690334)  
  

---   

## **regress-980007.js (chromium issue)**  
   
**[Issue: Integer-overflow in v8::internal::compiler::InstructionSelector::VisitInt32Sub](https://crbug.com/980007)**  
**[Commit: [ubsan] Fix integer overflow in compiler](https://chromium.googlesource.com/v8/v8/+/a420d20)**  
  
Date(Commit): Mon Jul 01 14:34:45 2019  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-Stable, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1683993](https://chromium-review.googlesource.com/c/v8/v8/+/1683993)  
Regress: [mjsunit/regress/wasm/regress-980007.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-980007.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_i).addBody([
  kExprI64Const, 0x01,
  kExprI32ConvertI64,
  kExprI32Const, 0x80, 0x80, 0x80, 0x80, 0x78,
  kExprI32Sub,
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a420d20^!)  
[src/compiler/backend/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/x64/instruction-selector-x64.cc?cl=a420d20)  
[test/mjsunit/regress/wasm/regress-980007.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-980007.js?cl=a420d20)  
  

---   

## **regress-crbug-976934.js (chromium issue)**  
   
**[Issue: Float-cast-overflow in v8::internal::wasm::AsmJsParser::ValidateFunctionLocals](https://crbug.com/976934)**  
**[Commit: [asm.js] Fix undefined cast from double to float.](https://chromium.googlesource.com/v8/v8/+/f03430f)**  
  
Date(Commit): Mon Jul 01 14:27:05 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-Stable, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1683999](https://chromium-review.googlesource.com/c/v8/v8/+/1683999)  
Regress: [mjsunit/regress/regress-crbug-976934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-976934.js)  
```javascript
function Module(stdlib, imports, heap) {
  "use asm";

  var fround = stdlib.Math.fround;

  function f() {
    var x = fround(-1.7976931348623157e+308);
    return fround(x);
  }

  return { f: f };
}

var m = Module(this);
assertEquals(-Infinity, m.f());
assertTrue(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f03430f^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=f03430f)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=f03430f)  
[test/mjsunit/regress/regress-crbug-976934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-976934.js?cl=f03430f)  
  

---   

## **regress-v8-8770.js (v8 issue)**  
   
**[Issue: Regex results are not independent; results depend on order of strings tested](https://crbug.com/v8/8770)**  
**[Commit: [regexp] Fix BoyerMooreLookahead behavior at submatches](https://chromium.googlesource.com/v8/v8/+/bc4cbe9)**  
  
Date(Commit): Mon Jul 01 07:14:17 2019  
Code Review: [https://codereview.chromium.org/2777583003,](https://codereview.chromium.org/2777583003,)  
Regress: [mjsunit/regress/regress-v8-8770.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-8770.js)  
```javascript
const re = /^.*?Y((?=X?).)*Y$/s;
const sult = "YABCY";
const result = re.exec(sult);

assertNotNull(result);
assertArrayEquals([sult, "C"], result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc4cbe9^!)  
[src/regexp/regexp-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler.cc?cl=bc4cbe9)  
[test/mjsunit/regress/regress-v8-8770.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-8770.js?cl=bc4cbe9)  
  

---   

## **regress-v8-9394-2.js (v8 issue)**  
   
**[Issue: maybe_assigned is wrong  around with statements](https://crbug.com/v8/9394)**  
**[Commit: [parser] Always mark shadowed vars maybe_assigned](https://chromium.googlesource.com/v8/v8/+/e79751b)**  
  
Date(Commit): Thu Jun 27 08:26:02 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1678365](https://chromium-review.googlesource.com/c/v8/v8/+/1678365)  
Regress: [mjsunit/regress/regress-v8-9394-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9394-2.js), [mjsunit/regress/regress-v8-9394.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9394.js)  
```javascript
function test() {
  function f() {
    with ({}) {
      // This is a non-assigning shadowing access to value. If both f and test
      // are fully parsed or both are preparsed, then this is resolved during
      // scope analysis to the outer value, and the outer value knows it can be
      // shadowed. If test is fully parsed and f is preparsed, value here
      // doesn't resolve to anything during partial analysis, and the outer
      // value does not know it can be shadowed.
      return value;
    }
  }
  var value = 2;
  var status = f();
  return value;
}
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e79751b^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=e79751b)  
[test/mjsunit/regress/regress-v8-9394.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9394.js?cl=e79751b)  
  

---   

## **regress-crbug-976598.js (chromium issue)**  
   
**[Issue: Breakpoint in Builtins_InterpreterEntryTrampoline](https://crbug.com/976598)**  
**[Commit: [objects] Migrate kHoleNanInt64 unboxed doubles to uninitialized values during boilerplate serialization](https://chromium.googlesource.com/v8/v8/+/eaf2a23)**  
  
Date(Commit): Wed Jun 26 15:51:39 2019  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, ClusterFuzz-Wrong, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1674034](https://chromium-review.googlesource.com/c/v8/v8/+/1674034)  
Regress: [mjsunit/regress/regress-crbug-976598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-976598.js)  
```javascript
function f() {
  return { value: NaN };
}

%PrepareFunctionForOptimization(f);
f();
f();

let x = { value: "Y" };

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eaf2a23^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=eaf2a23)  
[test/mjsunit/regress/regress-crbug-976598.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-976598.js?cl=eaf2a23)  
  

---   

## **regress-crbug-977012.js (chromium issue)**  
   
**[Issue: DCHECK failure in descriptor_number < number_of_descriptors() in descriptor-array-inl.h](https://crbug.com/977012)**  
**[Commit: [map] Update map in PrepareForDataProperty](https://chromium.googlesource.com/v8/v8/+/9c1363e)**  
  
Date(Commit): Wed Jun 26 10:17:41 2019  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-77, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1674029](https://chromium-review.googlesource.com/c/v8/v8/+/1674029)  
Regress: [mjsunit/regress/regress-crbug-977012.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-977012.js)  
```javascript
function foo(arg) {
  var ret = { x: arg };
  ret.__defineSetter__("y", function() { });
  return ret;
}

let v1 = foo(10);
let v2 = foo(10.5);

v1.x = 20.5;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c1363e^!)  
[src/objects/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/objects/lookup.cc?cl=9c1363e)  
[src/objects/lookup.h](https://cs.chromium.org/chromium/src/v8/src/objects/lookup.h?cl=9c1363e)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=9c1363e)  
[test/mjsunit/regress/regress-crbug-977012.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-977012.js?cl=9c1363e)  
  

---   

## **regress-976627.js (chromium issue)**  
   
**[Issue: v8 crash on regexp length check](https://crbug.com/976627)**  
**[Commit: [regexp] Allow JSRegExpResult allocations in large object space](https://chromium.googlesource.com/v8/v8/+/4c15693)**  
  
Date(Commit): Wed Jun 26 07:50:33 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, reward-3000, Security_Impact-Stable, Arch-x86_64, Security_Severity-High, allpublic, reward-inprocess, Via-Wizard-Security, CVE_description-missing, M-75, merge-merged-7.6, Release-0-M76, CVE-2019-5853  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1674027](https://chromium-review.googlesource.com/c/v8/v8/+/1674027)  
Regress: [mjsunit/regress/regress-976627.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-976627.js)  
```javascript
function v2() {
    const v8 = Symbol || 9007199254740991;
    function v9(v10,v11,v12) {
    }
    const v16 = String();
    const v100 = String();//add
    const v106 = String();// add
    const v116 = String();// add
    const v17 = Int32Array();
    const v18 = Map();
    const v19 = [];
    const v20 = v18.values();
    function v21(v22,v23,v24,v25,v26) {
    }
    function v28(v29,v30,v31) {
        function v32(v33,v34,v35,v36) {
        }
        let v39 = 0;
        do {
            const v40 = v32();
            function v99() {
            }
        } while (v39 < 8);
    }
    const v41 = Promise();
}
const v46 = ["has",13.37,-9007199254740991,Reflect];
for (let v50 = 64; v50 <= 2000; v50++) {
    v46.push(v50,v2);
}
const v54 = RegExp(v46);
const v55 = v54.exec();

assertTrue(%HasElementsInALargeObjectSpace(v55));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c15693^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=4c15693)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=4c15693)  
[src/codegen/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.h?cl=4c15693)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=4c15693)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=4c15693)  
...  
  

---   

## **regress-crbug-974474.js (chromium issue)**  
   
**[Issue: Turbofan infers wrong information about control flow](https://crbug.com/974474)**  
**[Commit: [turbofan] fix bug in CommonOperatorReducer::ReduceReturn](https://chromium.googlesource.com/v8/v8/+/6254e98)**  
  
Date(Commit): Tue Jun 25 11:00:01 2019  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1669693](https://chromium-review.googlesource.com/c/v8/v8/+/1669693)  
Regress: [mjsunit/compiler/regress-crbug-974474.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-crbug-974474.js)  
```javascript
function foo(x) {
    const y = x == 42;
    () => {y};
    if (y) { Object(); }
    [!!y];
    return y;
}

%PrepareFunctionForOptimization(foo);
foo(42); foo(42);
%OptimizeFunctionOnNextCall(foo);
foo(42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6254e98^!)  
[src/compiler/common-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator-reducer.cc?cl=6254e98)  
[test/mjsunit/compiler/regress-crbug-974474.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-crbug-974474.js?cl=6254e98)  
  

---   

## **regress-977670.js (chromium issue)**  
   
**[Issue: CHECK failure: IsLoad() in js-heap-broker.cc](https://crbug.com/977670)**  
**[Commit: [turbofan] Fix call of ReduceElementAccessOnString](https://chromium.googlesource.com/v8/v8/+/f21537a)**  
  
Date(Commit): Mon Jun 24 11:17:33 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-77, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1672940](https://chromium-review.googlesource.com/c/v8/v8/+/1672940)  
Regress: [mjsunit/compiler/regress-977670.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-977670.js)  
```javascript
function foo() {
  var i;
  for (i in 'xxxxxxxx') {
    try { throw 42 } catch (e) {}
  }
  print(i);
  i['' + 'length'] = 42;
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f21537a^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=f21537a)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=f21537a)  
[test/mjsunit/compiler/regress-977670.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-977670.js?cl=f21537a)  
  

---   

## **regress-crbug-977089.js (chromium issue)**  
   
**[Issue: DCHECK failure in fresh->bit_field3() & ~IsInRetainedMapListBit::kMask == new_map->bit_field3() & ](https://crbug.com/977089)**  
**[Commit: [map] Ignore migration target bit when normalizing](https://chromium.googlesource.com/v8/v8/+/88d2349)**  
  
Date(Commit): Mon Jun 24 10:44:11 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1672936](https://chromium-review.googlesource.com/c/v8/v8/+/1672936)  
Regress: [mjsunit/regress/regress-crbug-977089.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-977089.js)  
```javascript
var foo = function() {

  function f1(arg) {
    var ret = { x: arg };
    ret.__defineGetter__("y", function() { });
    return ret;
  }
  // Create v1 with a map with properties: {x:Smi, y:AccessorPair}
  let v1 = f1(10);
  // Create a map with properties: {x:Double, y:AccessorPair}, deprecating the
  // previous map.
  let v2 = f1(10.5);

  // Access x on v1 to a function that reads x, which triggers it to update its
  // map. This update transitions v1 to slow mode as there is already a "y"
  // transition with a different accessor.
  //
  // Note that the parent function `foo` can't be an IIFE, as then this callsite
  // would use the NoFeedback version of the LdaNamedProperty bytecode, and this
  // doesn't trigger the map update.
  v1.x;

  // Create v3 which overwrites a non-accessor with an accessor, triggering it
  // to normalize, and picking up the same cached normalized map as v1. However,
  // v3's map is not a migration target and v1's is (as it was migrated to when
  // updating v1), so the migration target bit doesn't match. This should be
  // fine and shouldn't trigger any DCHECKs.
  let v3 = { z:1 };
  v3.__defineGetter__("z", function() {});
};

%EnsureFeedbackVectorForFunction(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88d2349^!)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=88d2349)  
[test/cctest/test-field-type-tracking.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-field-type-tracking.cc?cl=88d2349)  
[test/mjsunit/regress/regress-crbug-977089.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-977089.js?cl=88d2349)  
  

---   

## **regress-9383.js (v8 issue)**  
   
**[Issue: Lazy compilation produces different bytecode for object with getters/setters](https://crbug.com/v8/9383)**  
**[Commit: [interpreter] Fix order of bytecode generated for adding getters/setters](https://chromium.googlesource.com/v8/v8/+/fc68d1e)**  
  
Date(Commit): Thu Jun 20 18:41:42 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1669689](https://chromium-review.googlesource.com/c/v8/v8/+/1669689)  
Regress: [mjsunit/regress/regress-9383.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9383.js)  
```javascript
var c = {
  get b() {
  },
  get getter() {
  },
  set a(n) {
  },
  set a(n) {
  },
  set setter1(n) {
  },
  set setter2(n) {
  },
  set setter3(n) {
  },
  set setter4(n) {
  },
  set setter5(n) {
  },
  set setter6(n) {
  },
  set setter7(n) {
  },
  set setter8(n) {
  },
  set setter9(n) {
  },
  set setter10(n) {
  },
  set setter11(n) {
  },
  set setter12(n) {
  },
  set setter12(n) {
  },
};

for (x in c) {
  print(x);
}

throw new Error();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc68d1e^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=fc68d1e)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=fc68d1e)  
[test/mjsunit/regress/regress-9383.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9383.js?cl=fc68d1e)  
  

---   

## **regress-crbug-976256.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/976256)**  
**[Commit: [Turbofan] Fix crash in MapInference::~MapInference](https://chromium.googlesource.com/v8/v8/+/b3ce13f)**  
  
Date(Commit): Wed Jun 19 06:53:38 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1660623](https://chromium-review.googlesource.com/c/v8/v8/+/1660623)  
Regress: [mjsunit/regress/regress-crbug-976256.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-976256.js)  
```javascript
function foo(r) {
  return r.finally();
}

const resolution = Promise.resolve();
foo(resolution);

function bar() {
  try {
    foo(undefined);
  } catch (e) {}
}

%PrepareFunctionForOptimization(bar);
bar();
bar();
%OptimizeFunctionOnNextCall(bar);
bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b3ce13f^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=b3ce13f)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=b3ce13f)  
[test/mjsunit/regress/regress-crbug-976256.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-976256.js?cl=b3ce13f)  
  

---   

## **regress-8510-2.js (v8 issue)**  
   
**[Issue: Omit generating source positions](https://crbug.com/v8/8510)**  
**[Commit: Fix crash when reporting exceptions](https://chromium.googlesource.com/v8/v8/+/d8164d5)**  
  
Date(Commit): Tue Jun 18 20:52:38 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1664334](https://chromium-review.googlesource.com/c/v8/v8/+/1664334)  
Regress: [mjsunit/regress/regress-8510-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8510-2.js), [mjsunit/regress/regress-8510.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8510.js)  
```javascript
try {
  (function () {
    eval(`
      function assertLocation() {}
      (function foo() {
        var x = 1;
        assertLocation();
        throw new Error();
      })();
    `);
  })();
} catch (e) {
  print(e.stack);
}

try {
  (function () {
    var assertLocation = 2;
    (function () {
      eval(`
        function assertLocation() {}
        (function foo() {
          var x = 1;
          assertLocation();
          throw new Error();
        })();
      `);
    })();
  })();
} catch (e) {
  print(e.stack);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8164d5^!)  
[src/execution/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/execution/isolate.cc?cl=d8164d5)  
[test/mjsunit/regress/regress-8510.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8510.js?cl=d8164d5)  
  

---   

## **regress-968078.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/968078)**  
**[Commit: [arm64] Ensure pools are emitted before emitting large branch tables](https://chromium.googlesource.com/v8/v8/+/19eb723)**  
  
Date(Commit): Tue Jun 18 13:42:22 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1664067](https://chromium-review.googlesource.com/c/v8/v8/+/1664067)  
Regress: [mjsunit/regress/wasm/regress-968078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-968078.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  function repeat(value, length) {
    var arr = new Array(length);
    for (let i = 0; i < length; i++) {
      arr[i] = value;
    }
    return arr;
  }
  function br_table(block_index, length, def_block) {
    const bytes = new Binary();
    bytes.emit_bytes([kExprBrTable]);
    // Functions count (different than the count in the functions section.
    bytes.emit_u32v(length);
    bytes.emit_bytes(repeat(block_index, length));
    bytes.emit_bytes([def_block]);
    return Array.from(bytes.trunc_buffer());
  }
  var builder = new WasmModuleBuilder();
  builder.addMemory(12, 12, false);
  builder.addFunction("foo", kSig_v_iii)
    .addBody([].concat([
      kExprBlock, kWasmStmt,
        kExprLocalGet, 0x2,
        kExprI32Const, 0x01,
        kExprI32And,
        // Generate a test branch (which has 32k limited reach).
        kExprIf, kWasmStmt,
          kExprLocalGet, 0x0,
          kExprI32Const, 0x01,
          kExprI32And,
          kExprBrIf, 0x1,
          kExprLocalGet, 0x0,
          // Emit a br_table that is long enough to make the test branch go out of range.
          ], br_table(0x1, 9000, 0x00), [
        kExprEnd,
      kExprEnd,
    ])).exportFunc();
  builder.instantiate();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19eb723^!)  
[src/codegen/arm64/assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/arm64/assembler-arm64.cc?cl=19eb723)  
[src/codegen/arm64/assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/codegen/arm64/assembler-arm64.h?cl=19eb723)  
[src/codegen/constant-pool.h](https://cs.chromium.org/chromium/src/v8/src/codegen/constant-pool.h?cl=19eb723)  
[src/compiler/backend/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm64/code-generator-arm64.cc?cl=19eb723)  
[test/mjsunit/regress/wasm/regress-968078.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-968078.js?cl=19eb723)  
  

---   

## **regress-crbug-974476.js (chromium issue)**  
   
**[Issue: Turbofan EscapeAnalysis fails to remove a node](https://crbug.com/974476)**  
**[Commit: [turbofan] fix escape analysis bug: revisit phis](https://chromium.googlesource.com/v8/v8/+/92fdbc1)**  
  
Date(Commit): Tue Jun 18 12:10:46 2019  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1664063](https://chromium-review.googlesource.com/c/v8/v8/+/1664063)  
Regress: [mjsunit/compiler/regress-crbug-974476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-crbug-974476.js)  
```javascript
function use(x) { return x; }
%NeverOptimizeFunction(use);

function foo() {
  let result = undefined;
  (function () {
    const a = {};
    for (_ of [0]) {
      const empty = [];
      (function () {
        result = 42;
        for (_ of [0]) {
          for (_ of [0]) {
            use(empty);
          }
        }
        result = a;
      })();
    }
  })();
  return result;
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92fdbc1^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=92fdbc1)  
[test/mjsunit/compiler/regress-crbug-974476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-crbug-974476.js?cl=92fdbc1)  
  

---   

## **regress-crbug-971782.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/971782)**  
**[Commit: [turbofan] Properly handle -0 in Word32->Word64 conversion.](https://chromium.googlesource.com/v8/v8/+/523be74)**  
  
Date(Commit): Tue Jun 18 11:17:25 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1664062](https://chromium-review.googlesource.com/c/v8/v8/+/1664062)  
Regress: [mjsunit/regress/regress-crbug-971782.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-971782.js)  
```javascript
function foo(dv) {
  for (let i = -1; i < 1; ++i) {
    dv.setUint16(i % 1);
  }
}

const dv = new DataView(new ArrayBuffer(2));
%PrepareFunctionForOptimization(foo);
foo(dv);
foo(dv);
%OptimizeFunctionOnNextCall(foo);
foo(dv);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/523be74^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=523be74)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=523be74)  
[test/mjsunit/regress/regress-crbug-971782.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-971782.js?cl=523be74)  
  

---   

## **regress-crbug-969368.js (chromium issue)**  
   
**[Issue: CHECK failure: (location_) != nullptr in maybe-handles.h](https://crbug.com/969368)**  
**[Commit: [asm.js] Check that function table indices are intish.](https://chromium.googlesource.com/v8/v8/+/b8474e7)**  
  
Date(Commit): Mon Jun 17 16:59:50 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1662571](https://chromium-review.googlesource.com/c/v8/v8/+/1662571)  
Regress: [mjsunit/regress/regress-crbug-969368.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-969368.js)  
```javascript
function Module() {
  'use asm';
  function f() {}
  function g() {
    var x = 0.0;
    table[x & 3]();
  }
  var table = [f, f, f, f];
  return { g: g };
}
var m = Module();
assertDoesNotThrow(m.g);
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8474e7^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=b8474e7)  
[test/mjsunit/regress/regress-crbug-969368.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-969368.js?cl=b8474e7)  
  

---   

## **regress-v8-6515.js (v8 issue)**  
   
**[Issue: [regexp] Collapse impossible \b\B patterns into failures](https://crbug.com/v8/6515)**  
**[Commit: [regexp] Rewrite certain Assertion sequences](https://chromium.googlesource.com/v8/v8/+/c51e4f3)**  
  
Date(Commit): Mon Jun 17 09:21:58 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1657925](https://chromium-review.googlesource.com/c/v8/v8/+/1657925)  
Regress: [mjsunit/regress/regress-v8-6515.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6515.js)  
```javascript
assertNull(/\b\B\b\B\b\B\b\B\b\B\b\B\b\B\b\B\b\B/.exec(" aa "));
assertNull(/\b\b\b\b\b\b\b\b\b\B\B\B\B\B\B\B\B\B/.exec(" aa "));
assertNull(/\b\B$\b\B$\b\B$\b\B$\b\B$\b\B$\b\B$/.exec(" aa "));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c51e4f3^!)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=c51e4f3)  
[src/regexp/regexp-compiler-tonode.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-compiler-tonode.cc?cl=c51e4f3)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=c51e4f3)  
[test/cctest/test-regexp.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-regexp.cc?cl=c51e4f3)  
[test/mjsunit/regress/regress-v8-6515.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6515.js?cl=c51e4f3)  
  

---   

## **regress-crbug-397662.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/397662)**  
**[Commit: [runtime] Throw RangeError if we try to get too many values or entries](https://chromium.googlesource.com/v8/v8/+/e79e81c)**  
  
Date(Commit): Thu Jun 13 12:28:02 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1657914](https://chromium-review.googlesource.com/c/v8/v8/+/1657914)  
Regress: [mjsunit/regress/regress-crbug-397662.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-397662.js)  
```javascript
var a = new Uint8Array(%MaxSmi() >> 1);
a.x = 1;
assertThrows(()=>Object.entries(a), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e79e81c^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=e79e81c)  
[test/mjsunit/regress/regress-crbug-397662.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-397662.js?cl=e79e81c)  
  

---   

## **regress-v8-7848.js (v8 issue)**  
   
**[Issue: [error] Use prepareStackTrace from the holder's realm](https://crbug.com/v8/7848)**  
**[Commit: [error] Use prepareStackTrace from error's realm](https://chromium.googlesource.com/v8/v8/+/d766d6d)**  
  
Date(Commit): Tue Jun 11 13:02:45 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1113438](https://chromium-review.googlesource.com/c/v8/v8/+/1113438)  
Regress: [mjsunit/regress/regress-v8-7848.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-7848.js)  
```javascript
Error.prepareStackTrace = () => 299792458;

{
  const that_realm = Realm.create();
  const result = Realm.eval(that_realm,
      "() => { Error.prepareStackTrace = () => 42; return new Error(); }")();
  assertEquals(42, result.stack);
}
{
  const that_realm = Realm.create();
  const result = Realm.eval(that_realm,
      "() => { Error.prepareStackTrace = () => 42; " +
      "class MyError extends Error {}; return new MyError(); }")();
  assertEquals(42, result.stack);
}
{
  const that_realm = Realm.create();
  const result = Realm.eval(that_realm,
      "() => { Error.prepareStackTrace = () => 42; return {}; }")();
  assertFalse("stack" in result);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d766d6d^!)  
[src/execution/messages.cc](https://cs.chromium.org/chromium/src/v8/src/execution/messages.cc?cl=d766d6d)  
[test/mjsunit/regress/regress-v8-7848.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-7848.js?cl=d766d6d)  
  

---   

## **regress-crbug-967101.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_no_ic:x64,ignition_turbo_opt](https://crbug.com/967101)**  
**[Commit: Handle IC store with sealed elements](https://chromium.googlesource.com/v8/v8/+/659010e)**  
  
Date(Commit): Mon Jun 10 19:54:17 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1632830](https://chromium-review.googlesource.com/c/v8/v8/+/1632830)  
Regress: [mjsunit/regress/regress-crbug-967101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-967101.js)  
```javascript
function packedStore() {
  let a = Object.seal([""]);
  a[0] = 0;
  assertEquals(a[0], 0);
}

packedStore();
packedStore();

function holeyStore() {
  let a = Object.seal([, ""]);
  a[0] = 0;
  assertEquals(a[0], undefined);
}

holeyStore();
holeyStore();

let a = Object.seal([, ""]);
function foo() {
  a[1] = 0;
}

foo();
foo();
function bar() {
  a[0] = 1;
}
assertEquals(a, [, 0]);
bar();
assertEquals(a, [, 0]);
bar();
assertEquals(a, [, 0]);
function baz() {
  a[2] = 2;
}
assertEquals(a, [, 0]);
baz();
assertEquals(a, [, 0]);
baz();
assertEquals(a, [, 0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/659010e^!)  
[src/codegen/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/codegen/code-stub-assembler.cc?cl=659010e)  
[test/mjsunit/regress/regress-crbug-967101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-967101.js?cl=659010e)  
  

---   

## **regress-crbug-969498.js (chromium issue)**  
   
**[Issue: Float-cast-overflow in v8::WebAssemblyGlobalSetValue](https://crbug.com/969498)**  
**[Commit: [ubsan] Fix a few double-to-float casts](https://chromium.googlesource.com/v8/v8/+/05e3b64)**  
  
Date(Commit): Sat Jun 08 12:38:02 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1648253](https://chromium-review.googlesource.com/c/v8/v8/+/1648253)  
Regress: [mjsunit/regress/regress-crbug-969498.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-969498.js)  
```javascript
let global = new WebAssembly.Global({value: 'f32', mutable: true}, 2e66);
global.value = 2e66;

const kRoundsDown = 3.4028235677973362e+38;
const kRoundsToInf = 3.4028235677973366e+38;
var floats = new Float32Array([kRoundsDown, kRoundsToInf]);
assertNotEquals(Infinity, floats[0]);
assertEquals(Infinity, floats[1]);
floats.set([kRoundsDown, kRoundsToInf]);
assertNotEquals(Infinity, floats[0]);
assertEquals(Infinity, floats[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/05e3b64^!)  
[src/objects/elements.cc](https://cs.chromium.org/chromium/src/v8/src/objects/elements.cc?cl=05e3b64)  
[src/wasm/wasm-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-interpreter.cc?cl=05e3b64)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=05e3b64)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=05e3b64)  
[test/mjsunit/regress/regress-crbug-969498.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-969498.js?cl=05e3b64)  
  

---   

## **regress-crbug-971383.js (chromium issue)**  
   
**[Issue: RegExp handling of /[a-z]/ig  broken for Turkish language, breaks Google Maps](https://crbug.com/971383)**  
**[Commit: Fix character ranges in case insensitive regexp](https://chromium.googlesource.com/v8/v8/+/9bcacf6)**  
  
Date(Commit): Fri Jun 07 00:09:17 2019  
Components: Blink>JavaScript>Regexp  
Labels: Hotlist-Merge-Review, Hotlist-Merge-Approved, ReleaseBlock-Stable, Target-75, FoundIn-75, RegressedIn-75, Hotlist-gTech, Hotlist-gTech-Channel-Stable, merge-merged-7.5, merge-merged-7.6, TE-Verified-75.0.3770.90, Hotlist-gTech-Validate  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1648098](https://chromium-review.googlesource.com/c/v8/v8/+/1648098)  
Regress: [mjsunit/regress/regress-crbug-971383.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-971383.js)  
```javascript
assertEquals(["HIJK"], "HIJK".match(/[a-z]+/gi));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bcacf6^!)  
[src/d8/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8/d8.cc?cl=9bcacf6)  
[src/d8/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8/d8.h?cl=9bcacf6)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=9bcacf6)  
[test/mjsunit/regress/regress-crbug-971383.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-971383.js?cl=9bcacf6)  
  

---   

## **regress-crbug-964833.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/964833)**  
**[Commit: Fix Load Elimination crash involving transitioning const stores in loops](https://chromium.googlesource.com/v8/v8/+/2911a16)**  
  
Date(Commit): Wed Jun 05 10:47:58 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1645315](https://chromium-review.googlesource.com/c/v8/v8/+/1645315)  
Regress: [mjsunit/regress/regress-crbug-964833.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-964833.js)  
```javascript
function f() {
  var n = 3;
  var obj = {};

  var m = n;
  for (;;) {
    m++;

    if (m == 456) {
      break;
    }

    var i = 0;
    var j = 0;
    while (i < 1) {
      j = i;
      i++;
    }
    obj.y = j;
  }
}

%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2911a16^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=2911a16)  
[test/mjsunit/regress/regress-crbug-964833.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-964833.js?cl=2911a16)  
  

---   

## **regress-966460.js (chromium issue)**  
   
**[Issue: DCHECK failure in object->HasSmiOrObjectElements() || object->HasDoubleElements() || object->HasFa](https://crbug.com/966460)**  
**[Commit: Freeze proxy from sealed elements-kind object can normalize elements](https://chromium.googlesource.com/v8/v8/+/211b4e5)**  
  
Date(Commit): Wed May 29 18:05:28 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-76, Merge-M76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1625864](https://chromium-review.googlesource.com/c/v8/v8/+/1625864)  
Regress: [mjsunit/regress-966460.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-966460.js)  
```javascript
PI = [];
PI[250] = PI;
Object.seal(PI);
assertTrue(Object.isSealed(PI));
var proxy = new Proxy(PI, PI);
Object.freeze(proxy);
assertTrue(Object.isFrozen(proxy));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/211b4e5^!)  
[src/objects/elements.cc](https://cs.chromium.org/chromium/src/v8/src/objects/elements.cc?cl=211b4e5)  
[src/objects/js-objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects-inl.h?cl=211b4e5)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=211b4e5)  
[src/objects/js-objects.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.h?cl=211b4e5)  
[test/mjsunit/regress-966460.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-966460.js?cl=211b4e5)  
  

---   

## **regress-crbug-966450.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_no_ic](https://crbug.com/966450)**  
**[Commit: Fix correctness issue in proxy set trap](https://chromium.googlesource.com/v8/v8/+/731a370)**  
  
Date(Commit): Wed May 29 13:16:49 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1632238](https://chromium-review.googlesource.com/c/v8/v8/+/1632238)  
Regress: [mjsunit/regress/regress-crbug-966450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-966450.js)  
```javascript
let prop = "someName";
function foo(a, b, v) { return a[b] = 0 }
try {
  foo("", prop);
} catch(e) {}
var target = {};
var traps = { set() {return 42} };
var proxy = new Proxy(target, traps);
Object.defineProperty(target, prop, { value: 0 });
try {
  foo(proxy, prop);
} catch (e) { }
foo(proxy, prop, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/731a370^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=731a370)  
[src/builtins/proxy-set-property.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/proxy-set-property.tq?cl=731a370)  
[test/mjsunit/regress/regress-crbug-966450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-966450.js?cl=731a370)  
  

---   

## **regress-crbug-967434.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::LoadElimination::ReduceStoreField](https://crbug.com/967434)**  
**[Commit: Weaken representation tracking assertion in load elimination](https://chromium.googlesource.com/v8/v8/+/6e89adc)**  
  
Date(Commit): Tue May 28 13:43:05 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1632169](https://chromium-review.googlesource.com/c/v8/v8/+/1632169)  
Regress: [mjsunit/regress/regress-crbug-967434.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-967434.js)  
```javascript
function f1(h_also_eval) {
  this.x = h_also_eval;
}

function f2(h, h_eval) {
  var o = new f1(h());
  // During the last call to f3 with g2 as an argument, this store is
  // bi-morphic, including a version that refers to the old map (before
  // the replacement of f1's prototype). As a result, during load elimination
  // we see two stores with incompatible representations: One in the
  // constructor, and one in the impossible branch of the bi-morphic store
  // site.
  o.x = h_eval;
};
function f3(h) {
  %PrepareFunctionForOptimization(f2);
  f2(h, h());
  %OptimizeFunctionOnNextCall(f2);
  f2(h, h());
}

function g1() {
  return {};
};
function g2() {
  return 4.2;
};

f3(g1);
f3(g2);

f3(g1);
f1.prototype = {};
f3(g2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6e89adc^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=6e89adc)  
[test/mjsunit/regress/regress-crbug-967434.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-967434.js?cl=6e89adc)  
  

---   

## **regress-966560-1.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::JSHeapBroker::GetGlobalAccessFeedback](https://crbug.com/966560)**  
**[Commit: [turbofan] Fix serialization of resumables](https://chromium.googlesource.com/v8/v8/+/72fbd95)**  
  
Date(Commit): Tue May 28 09:30:23 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1630670](https://chromium-review.googlesource.com/c/v8/v8/+/1630670)  
Regress: [mjsunit/compiler/regress-966560-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-966560-1.js), [mjsunit/compiler/regress-966560-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-966560-2.js)  
```javascript
async function __f_3() {
  return await __f_4();
}
async function __f_4() {
  await x.then();
  throw new Error();
};
%PrepareFunctionForOptimization(__f_4);
async function __f_5(f) {
  try {
    await f();
  } catch (e) {
  }
}
(async () => {
  ;
  %OptimizeFunctionOnNextCall(__f_4);
  await __f_5(__f_3);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/72fbd95^!)  
[src/compiler/serializer-for-background-compilation.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/serializer-for-background-compilation.cc?cl=72fbd95)  
[test/mjsunit/compiler/regress-966560-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-966560-1.js?cl=72fbd95)  
[test/mjsunit/compiler/regress-966560-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-966560-2.js?cl=72fbd95)  
  

---   

## **regress-v8-4153-1.js (v8 issue)**  
   
**[Issue: Remove Typed Array index size limitation](https://crbug.com/v8/4153)**  
**[Commit: Reland "[typedarray] Move external/data pointer to JSTypedArray."](https://chromium.googlesource.com/v8/v8/+/70bd7cf)**  
  
Date(Commit): Mon May 27 17:44:06 2019  
Code Review: [http://doc/1Z-wM2qwvAuxH46e9ivtkYvKzzwYZg8ymm0x0wJaomow](http://doc/1Z-wM2qwvAuxH46e9ivtkYvKzzwYZg8ymm0x0wJaomow)  
Regress: [mjsunit/regress/regress-v8-4153-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-4153-1.js)  
```javascript
var arrays = [
  Int8Array, Uint8Array, Int16Array, Uint16Array, Int32Array, Uint32Array,
  Float32Array, Float64Array, Uint8ClampedArray, BigInt64Array, BigUint64Array
].map(C => {
  new C(1)
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70bd7cf^!)  
[src/api/api.cc](https://cs.chromium.org/chromium/src/v8/src/api/api.cc?cl=70bd7cf)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=70bd7cf)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=70bd7cf)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=70bd7cf)  
[src/builtins/builtins-sharedarraybuffer-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-sharedarraybuffer-gen.cc?cl=70bd7cf)  
...  
  

---   

## **regress-crbug-967151.js (chromium issue)**  
   
**[Issue: CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (IsExternalOneByteString()) in string](https://crbug.com/967151)**  
**[Commit: [json] Strings can lie to us about representation, so check what's underneath](https://chromium.googlesource.com/v8/v8/+/f58b7e1)**  
  
Date(Commit): Mon May 27 10:56:44 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1630683](https://chromium-review.googlesource.com/c/v8/v8/+/1630683)  
Regress: [mjsunit/regress/regress-crbug-967151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-967151.js)  
```javascript
__v_3 = "100                         external string turned into two byte";
__v_2 = __v_3.substring(0, 28);
try {
  externalizeString(__v_3, true);
} catch (e) {}
assertEquals(100, JSON.parse(__v_2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f58b7e1^!)  
[src/builtins/builtins-json.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-json.cc?cl=f58b7e1)  
[test/mjsunit/regress/regress-crbug-967151.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-967151.js?cl=f58b7e1)  
  

---   

## **regress-crbug-967065.js (chromium issue)**  
   
**[Issue: CHECK failure: IsAligned(size, kTaggedSize) in runtime-internal.cc](https://crbug.com/967065)**  
**[Commit: [array] Prevent negative work array capacity when sorting](https://chromium.googlesource.com/v8/v8/+/82f6179)**  
  
Date(Commit): Mon May 27 10:41:44 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1630672](https://chromium-review.googlesource.com/c/v8/v8/+/1630672)  
Regress: [mjsunit/regress/regress-crbug-967065.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-967065.js)  
```javascript
function ThrowingSort() {
  const __v_3 = new Array(2147549152);
  Object.defineProperty(__v_3, 0, {
    get: () => { throw new Error("Do not actually sort!"); }
  });
  __v_3.sort();
}

assertThrows(() => ThrowingSort());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82f6179^!)  
[test/mjsunit/regress/regress-crbug-967065.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-967065.js?cl=82f6179)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=82f6179)  
  

---   

## **regress-crbug-967254.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/967254)**  
**[Commit: [array] Properly handle COW arrays in Array#sort](https://chromium.googlesource.com/v8/v8/+/dbf0262)**  
  
Date(Commit): Mon May 27 08:51:05 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1630675](https://chromium-review.googlesource.com/c/v8/v8/+/1630675)  
Regress: [mjsunit/regress/regress-crbug-967254.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-967254.js)  
```javascript
function COWSort() {
  const array = ["cc", "c", "aa", "bb", "b", "ab", "ac"];
  array.sort();
  return array;
}

assertArrayEquals(["aa", "ab", "ac", "b", "bb", "c", "cc"], COWSort());

Array.prototype.sort = () => {};

assertArrayEquals(["cc", "c", "aa", "bb", "b", "ab", "ac"], COWSort());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dbf0262^!)  
[test/mjsunit/regress/regress-crbug-967254.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-967254.js?cl=dbf0262)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=dbf0262)  
  

---   

## **regress-964607.js (chromium issue)**  
   
**[Issue: Security: WebAssembly duplicate indirect_function_table lead to OOB Write](https://crbug.com/964607)**  
**[Commit: [wasm] Initialize IFT only for table 0](https://chromium.googlesource.com/v8/v8/+/5cf5992)**  
  
Date(Commit): Thu May 23 14:55:46 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: reward-3000, Security_Impact-None, Security_Severity-High, allpublic, reward-inprocess  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1624804](https://chromium-review.googlesource.com/c/v8/v8/+/1624804)  
Regress: [mjsunit/regress/wasm/regress-964607.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-964607.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();

builder.addImportedTable('ffi', 't1', 5, 5, kWasmAnyFunc);
builder.addImportedTable('ffi', 't2', 9, 9, kWasmAnyFunc);

builder.addFunction('foo', kSig_v_v).addBody([]).exportFunc();

let module = builder.toModule();
let table1 =
    new WebAssembly.Table({element: 'anyfunc', initial: 5, maximum: 5});

let table2 =
    new WebAssembly.Table({element: 'anyfunc', initial: 9, maximum: 9});

let instance =
    new WebAssembly.Instance(module, {ffi: {t1: table1, t2: table2}});
let table3 =
    new WebAssembly.Table({element: 'anyfunc', initial: 9, maximum: 9});

table3.set(8, instance.exports.foo);
new WebAssembly.Instance(module, {ffi: {t1: table1, t2: table3}});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5cf5992^!)  
[src/wasm/module-instantiate.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-instantiate.cc?cl=5cf5992)  
[test/mjsunit/regress/wasm/regress-964607.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-964607.js?cl=5cf5992)  
  

---   

## **regress-crbug-965513.js (chromium issue)**  
   
**[Issue: V8 Deopt-Loop when converting Boolean to Number](https://crbug.com/965513)**  
**[Commit: [turbofan] fix deopt-loop for specuative Boolean to Number conversion](https://chromium.googlesource.com/v8/v8/+/5263653)**  
  
Date(Commit): Wed May 22 10:38:39 2019  
Components: Blink>JavaScript>Compiler  
Labels: M-75, M-76, M-74, merge-merged-7.5  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1622119](https://chromium-review.googlesource.com/c/v8/v8/+/1622119)  
Regress: [mjsunit/compiler/regress-crbug-965513.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-crbug-965513.js)  
```javascript
%EnsureFeedbackVectorForFunction(foo);
function foo(x) {
  return x * (x == 1);
};
%PrepareFunctionForOptimization(foo);
foo(0.5);
foo(1.5);
%OptimizeFunctionOnNextCall(foo);
foo(1.5);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5263653^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=5263653)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=5263653)  
[test/mjsunit/compiler/regress-crbug-965513.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-crbug-965513.js?cl=5263653)  
  

---   

## **regress-crbug-964869.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/964869)**  
**[Commit: [runtime] Make sure we don't inplace update None to Double](https://chromium.googlesource.com/v8/v8/+/cdd3c7c)**  
  
Date(Commit): Tue May 21 15:17:27 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1622114](https://chromium-review.googlesource.com/c/v8/v8/+/1622114)  
Regress: [mjsunit/regress/regress-crbug-964869.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-964869.js)  
```javascript
const o = {x: JSON.parse('{"x":1.1}').x};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cdd3c7c^!)  
[src/objects/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map-updater.cc?cl=cdd3c7c)  
[src/objects/property-details.h](https://cs.chromium.org/chromium/src/v8/src/objects/property-details.h?cl=cdd3c7c)  
[test/mjsunit/regress/regress-crbug-964869.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-964869.js?cl=cdd3c7c)  
  

---   

## **regress-963346.js (chromium issue)**  
   
**[Issue: CHECK failure: (map()->has_fast_smi_or_object_elements() || map()->has_frozen_or_sealed_element](https://crbug.com/963346)**  
**[Commit: Elements kind should not change after dictionary elements kind.](https://chromium.googlesource.com/v8/v8/+/8cbb60f)**  
  
Date(Commit): Mon May 20 21:31:24 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1613840](https://chromium-review.googlesource.com/c/v8/v8/+/1613840)  
Regress: [mjsunit/regress-963346.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-963346.js)  
```javascript
var o = ['3'];
function foo(i) { o.x = i; }
foo("string");
Object.preventExtensions(o);
Object.seal(o);
print('foo');
foo(0);
%HeapObjectVerify(o);
assertEquals(o.x, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8cbb60f^!)  
[src/objects/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map-updater.cc?cl=8cbb60f)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=8cbb60f)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=8cbb60f)  
[test/mjsunit/regress-963346.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-963346.js?cl=8cbb60f)  
  

---   

## **regress-v8-9267-1.js (v8 issue)**  
   
**[Issue: TurboFan compilation breaks NormalizedMapCache lookup](https://crbug.com/v8/9267)**  
**[Commit: [map] Move Map::IsInRetainedMapListBit out of Map::bit_field2.](https://chromium.googlesource.com/v8/v8/+/437d710)**  
  
Date(Commit): Mon May 20 14:01:46 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1619747](https://chromium-review.googlesource.com/c/v8/v8/+/1619747)  
Regress: [mjsunit/regress/regress-v8-9267-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9267-1.js), [mjsunit/regress/regress-v8-9267-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9267-2.js)  
```javascript
function bar(a) {
  return Object.defineProperty(a, 'x', {get() { return 1; }});
}

function foo() {
  return Array(1);
}

%NeverOptimizeFunction(bar);
%PrepareFunctionForOptimization(foo);
const o = foo();  // Keep a reference so the GC doesn't kill the map.
bar(o);
const a = bar(foo());
%OptimizeFunctionOnNextCall(foo);
const b = bar(foo());

assertTrue(%HaveSameMap(a, b));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/437d710^!)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=437d710)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=437d710)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=437d710)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=437d710)  
[src/heap/factory.cc](https://cs.chromium.org/chromium/src/v8/src/heap/factory.cc?cl=437d710)  
...  
  

---   

## **regress-crbug-940274.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/940274)**  
**[Commit: [Torque] Array.prototype.shift correctness fix](https://chromium.googlesource.com/v8/v8/+/c9b48e9)**  
  
Date(Commit): Fri May 17 14:38:30 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner, merge-merged-7.5  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1617255](https://chromium-review.googlesource.com/c/v8/v8/+/1617255)  
Regress: [mjsunit/regress/regress-crbug-940274.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-940274.js)  
```javascript
function foo() {
  var a = new Array({});
  a.shift();
  assertFalse(a.hasOwnProperty(0));
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9b48e9^!)  
[src/builtins/array-shift.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-shift.tq?cl=c9b48e9)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=c9b48e9)  
[test/mjsunit/regress/regress-crbug-940274.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-940274.js?cl=c9b48e9)  
  

---   

## **regress-963891.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/963891)**  
**[Commit: [ptr-compr] Adding compressed case to lowering of Boolean Not](https://chromium.googlesource.com/v8/v8/+/d382c2e)**  
  
Date(Commit): Fri May 17 12:45:48 2019  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1617248](https://chromium-review.googlesource.com/c/v8/v8/+/1617248)  
Regress: [mjsunit/regress/regress-963891.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-963891.js)  
```javascript
var bar = true;
bar = false;
function foo() {
  return !bar;
};
%PrepareFunctionForOptimization(foo);
assertEquals(foo(), true);
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(), true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d382c2e^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=d382c2e)  
[test/mjsunit/regress/regress-963891.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-963891.js?cl=d382c2e)  
  

---   

## **regress-crbug-963568.js (chromium issue)**  
   
**[Issue: DCHECK failure in descriptor_number < number_of_descriptors() in descriptor-array-inl.h](https://crbug.com/963568)**  
**[Commit: [json] Use correct index to read details](https://chromium.googlesource.com/v8/v8/+/30bcdca)**  
  
Date(Commit): Thu May 16 10:57:38 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-76, M-76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1615179](https://chromium-review.googlesource.com/c/v8/v8/+/1615179)  
Regress: [mjsunit/regress/regress-crbug-963568.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-963568.js)  
```javascript
JSON.parse('{"0":true,"1":true,"2":true,"3":true,"4":true,"9":true," ":true,"D":true,"B":true,"-1":true,"A":true,"C":true}');
JSON.parse('{"0":true,"1":true,"2":true,"3":true,"4":true,"9":true," ":true,"D":true,"B":true,"-1":true,"A":true,"C":true}');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/30bcdca^!)  
[src/json-parser.cc](https://cs.chromium.org/chromium/src/v8/src/json-parser.cc?cl=30bcdca)  
[test/mjsunit/regress/regress-crbug-963568.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-963568.js?cl=30bcdca)  
  

---   

## **regress-9234.js (v8 issue)**  
   
**[Issue: Crash when setting value to a proxy with a set function](https://crbug.com/v8/9234)**  
**[Commit: Reland of Port Proxy SetProperty trap builtin to Torque](https://chromium.googlesource.com/v8/v8/+/2dd0db1)**  
  
Date(Commit): Tue May 14 18:06:46 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1585269](https://chromium-review.googlesource.com/c/v8/v8/+/1585269)  
Regress: [mjsunit/es6/regress/regress-9234.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-9234.js)  
```javascript
(function returnFalsishStrict() {
  "use strict";

  function trySet(o) {
    o["bla"] = 0;
  }

  var proxy = new Proxy({}, {});
  var proxy2 = new Proxy({}, { set() { return ""; } });

  trySet(proxy);
  trySet(proxy);
  assertThrows(() => trySet(proxy2), TypeError);
})();

(function privateSymbolStrict() {
  "use strict";
  var proxy = new Proxy({}, {});
  var proxy2 = new Proxy({a: 1}, { set() { return true; } });

  function trySet(o) {
    var symbol = o == proxy2 ? %CreatePrivateSymbol("private"): 1;
    o[symbol] = 0;
  }

  trySet(proxy);
  trySet(proxy);
  assertThrows(() => trySet(proxy2), TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2dd0db1^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=2dd0db1)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=2dd0db1)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=2dd0db1)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=2dd0db1)  
[src/builtins/builtins-proxy-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.h?cl=2dd0db1)  
...  
  

---   

## **regress-v8-9243.js (v8 issue)**  
   
**[Issue: Builtin IterResultObjects should use the same map as {value,done}](https://crbug.com/v8/9243)**  
**[Commit: [map] Properly share the map for builtin iterator result objects.](https://chromium.googlesource.com/v8/v8/+/d2ea316)**  
  
Date(Commit): Tue May 14 14:02:29 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1609794](https://chromium-review.googlesource.com/c/v8/v8/+/1609794)  
Regress: [mjsunit/regress/regress-v8-9243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9243.js)  
```javascript
const user = {value:undefined, done:true};

const arrayResult = (new Array())[Symbol.iterator]().next();
assertTrue(%HaveSameMap(user, arrayResult));

const mapResult = (new Map())[Symbol.iterator]().next();
assertTrue(%HaveSameMap(user, mapResult));

const setResult = (new Set())[Symbol.iterator]().next();
assertTrue(%HaveSameMap(user, setResult));

function* generator() {}
const generatorResult = generator().next();
assertTrue(%HaveSameMap(user, setResult));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2ea316^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=d2ea316)  
[src/heap/factory.cc](https://cs.chromium.org/chromium/src/v8/src/heap/factory.cc?cl=d2ea316)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=d2ea316)  
[test/mjsunit/regress/regress-v8-9243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9243.js?cl=d2ea316)  
  

---   

## **regress-v8-9233.js (v8 issue)**  
   
**[Issue: Constness should be invalidated upon field deletion](https://crbug.com/v8/9233)**  
**[Commit: [constant-tracking] Disable `delete` optimization for constant fields.](https://chromium.googlesource.com/v8/v8/+/f0e054c)**  
  
Date(Commit): Tue May 14 13:36:37 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1609796](https://chromium-review.googlesource.com/c/v8/v8/+/1609796)  
Regress: [mjsunit/regress/regress-v8-9233.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-9233.js)  
```javascript
let o1 = { x: 999 };
o1.y = 999;

var o2 = { x: 1 };

function f() {
  return o2.x;
};
%PrepareFunctionForOptimization(f);
assertEquals(1, f());
assertEquals(1, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f());

delete o2.x;
o2.x = 2;
assertEquals(2, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0e054c^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=f0e054c)  
[test/mjsunit/regress/regress-v8-9233.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-9233.js?cl=f0e054c)  
  

---   

## **regress-crbug-961709-1.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/961709)**  
**[Commit: [ic] Disallow growing stores with TypedArrays in the prototype chain.](https://chromium.googlesource.com/v8/v8/+/bd17f12)**  
  
Date(Commit): Tue May 14 07:43:05 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1609790](https://chromium-review.googlesource.com/c/v8/v8/+/1609790)  
Regress: [mjsunit/regress/regress-crbug-961709-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-961709-1.js), [mjsunit/regress/regress-crbug-961709-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-961709-2.js)  
```javascript
function foo() {
  const a = [];
  a[0] = 1;
  return a[0];
}

function bar() {
  const a = new Array(10);
  a[0] = 1;
  return a[0];
}

Object.setPrototypeOf(Array.prototype, new Int8Array());
%EnsureFeedbackVectorForFunction(foo);
assertEquals(undefined, foo());
assertEquals(undefined, foo());

%EnsureFeedbackVectorForFunction(bar);
assertEquals(undefined, bar());
assertEquals(undefined, bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd17f12^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=bd17f12)  
[test/mjsunit/regress/regress-crbug-961709-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-961709-1.js?cl=bd17f12)  
[test/mjsunit/regress/regress-crbug-961709-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-961709-2.js?cl=bd17f12)  
  

---   

## **regress-961709-classes-opt.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/961709)**  
**[Commit: [ic] Disallow growing stores with TypedArrays in the prototype chain.](https://chromium.googlesource.com/v8/v8/+/bd17f12)**  
  
Date(Commit): Tue May 14 07:43:05 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1609790](https://chromium-review.googlesource.com/c/v8/v8/+/1609790)  
Regress: [mjsunit/regress/regress-961709-classes-opt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-961709-classes-opt.js), [mjsunit/regress/regress-961709-classes.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-961709-classes.js)  
```javascript
function foo(a, i) {
  a[i] = 1;
  return a[i];
}

class MyArray extends (class C extends Array {
}){};

o = new MyArray;

%PrepareFunctionForOptimization(foo);
assertEquals(1, foo(o, 0));
assertEquals(1, foo(o, 1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo(o, 2));
assertOptimized(foo);

o.__proto__.__proto__ = new Int32Array(3);


assertEquals(undefined, foo(o, 3));
assertUnoptimized(foo);
%PrepareFunctionForOptimization(foo);
assertEquals(undefined, foo(o, 3));
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo(o, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd17f12^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=bd17f12)  
[test/mjsunit/regress/regress-crbug-961709-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-961709-1.js?cl=bd17f12)  
[test/mjsunit/regress/regress-crbug-961709-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-961709-2.js?cl=bd17f12)  
  

---   

## **regress-961508.js (chromium issue)**  
   
**[Issue: Null-dereference READ in brand](https://crbug.com/961508)**  
**[Commit: Reland "[class] implement private method declarations"](https://chromium.googlesource.com/v8/v8/+/00c7e2a)**  
  
Date(Commit): Mon May 13 20:20:53 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://docs.google.com/document/d/1T-Ql6HOIH2U_8YjWkwK2rTfywwb7b3Qe8d3jkz72KwA/edit#](https://docs.google.com/document/d/1T-Ql6HOIH2U_8YjWkwK2rTfywwb7b3Qe8d3jkz72KwA/edit#)  
Regress: [mjsunit/regress/regress-961508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-961508.js)  
```javascript
const foo = new class bar extends async function () {}.constructor {}();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00c7e2a^!)  
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=00c7e2a)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=00c7e2a)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=00c7e2a)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=00c7e2a)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=00c7e2a)  
...  
  

---   

## **regress-961237.js (chromium issue)**  
   
**[Issue: Security: jit difference on comparison in d8](https://crbug.com/961237)**  
**[Commit: [turbofan] Fix handling of null in -0 == null comparison](https://chromium.googlesource.com/v8/v8/+/2108566)**  
  
Date(Commit): Mon May 13 13:35:03 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: reward-0, Security_Severity-Low, Security_Impact-Stable, Arch-All, allpublic, CVE_description-missing, Release-0-M76, CVE-2019-5857  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1609538](https://chromium-review.googlesource.com/c/v8/v8/+/1609538)  
Regress: [mjsunit/regress/regress-961237.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-961237.js)  
```javascript
const a = 1.1;
const b = null;

function f(x) { return -0 == (x ? a : b); }
%PrepareFunctionForOptimization(f);
assertEquals(false, f(true));
assertEquals(false, f(true));
%OptimizeFunctionOnNextCall(f);
assertEquals(false, f(false));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2108566^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=2108566)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=2108566)  
[test/mjsunit/regress/regress-961237.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-961237.js?cl=2108566)  
  

---   

## **regress-961986.js (chromium issue)**  
   
**[Issue: CHECK failure: feedback->kind() == ProcessedFeedback::kElementAccess in js-heap-broker.cc](https://crbug.com/961986)**  
**[Commit: [turbofan] Handle insufficient feedback in ComputeElementAccessInfos](https://chromium.googlesource.com/v8/v8/+/b1e7cd9)**  
  
Date(Commit): Mon May 13 10:39:01 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1607648](https://chromium-review.googlesource.com/c/v8/v8/+/1607648)  
Regress: [mjsunit/compiler/regress-961986.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-961986.js)  
```javascript
function foo() {
  const proto = [];
  const obj = Object.create(proto);
  obj[1] = "";
  proto[1];
  proto.bla = 42;
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1e7cd9^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=b1e7cd9)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=b1e7cd9)  
[test/mjsunit/compiler/regress-961986.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-961986.js?cl=b1e7cd9)  
  

---   

## **regress-961129.js (chromium issue)**  
   
**[Issue: Null-dereference WRITE in v8::internal::wasm::WasmCode::IncRef](https://crbug.com/961129)**  
**[Commit: [wasm][gc] Fix NativeModule::GetCode for nonexisting code](https://chromium.googlesource.com/v8/v8/+/0975c55)**  
  
Date(Commit): Fri May 10 09:40:23 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1602700](https://chromium-review.googlesource.com/c/v8/v8/+/1602700)  
Regress: [mjsunit/regress/wasm/regress-961129.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-961129.js)  
```javascript
%EnableCodeLoggingForTesting();

function module() {
  "use asm";
  function f() {
    var i = 4;
    return i | 0;
  }
  return {f: f};
}

module().f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0975c55^!)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=0975c55)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=0975c55)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=0975c55)  
[test/mjsunit/regress/wasm/regress-961129.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-961129.js?cl=0975c55)  
  

---   

## **regress-crbug-961522.js (chromium issue)**  
   
**[Issue: CHECK failure: function.has_feedback_vector() in js-inlining.cc](https://crbug.com/961522)**  
**[Commit: [turbofan] Fix wrong assumption in inlining](https://chromium.googlesource.com/v8/v8/+/9df690f)**  
  
Date(Commit): Fri May 10 08:10:58 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1605720](https://chromium-review.googlesource.com/c/v8/v8/+/1605720)  
Regress: [mjsunit/regress/regress-crbug-961522.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-961522.js)  
```javascript
(function () {
  let arr = [, 3];
  function inlined() {
  }
  function foo() {
    arr.reduce(inlined);
  };
  %PrepareFunctionForOptimization(foo);
  foo();
  %OptimizeFunctionOnNextCall(foo);
  foo();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9df690f^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=9df690f)  
[test/mjsunit/regress/regress-crbug-961522.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-961522.js?cl=9df690f)  
  

---   

## **regress-crbug-959727.js (chromium issue)**  
   
**[Issue: DCHECK failure in !IsElement() in lookup.h](https://crbug.com/959727)**  
**[Commit: Fix a DCHECK failure on an exception message](https://chromium.googlesource.com/v8/v8/+/621c5c6)**  
  
Date(Commit): Thu May 09 01:22:13 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-None, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1597372](https://chromium-review.googlesource.com/c/v8/v8/+/1597372)  
Regress: [mjsunit/regress/regress-crbug-959727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-959727.js)  
```javascript
'use strict';
let r = Realm.createAllowCrossRealmAccess();
Realm.detachGlobal(r);
try {
  Realm.global(r)[1] = 0;
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/621c5c6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=621c5c6)  
[test/mjsunit/regress/regress-crbug-959727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-959727.js?cl=621c5c6)  
  

---   

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

## **regress-crbug-941703.js (chromium issue)**  
   
**[Issue: Null-dereference WRITE in v8::internal::ParserBase<class v8::internal::Parser>::ParseAssignmentExpressionC](https://crbug.com/941703)**  
**[Commit: [parser] Clear is_parenthesized on ThisExpression when accessing it](https://chromium.googlesource.com/v8/v8/+/44382e9)**  
  
Date(Commit): Wed May 08 15:44:06 2019  
Components: Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1601260](https://chromium-review.googlesource.com/c/v8/v8/+/1601260)  
Regress: [mjsunit/regress/regress-crbug-941703.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-941703.js)  
```javascript
assertThrows("(this) , this =>", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/44382e9^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=44382e9)  
[test/mjsunit/regress/regress-crbug-941703.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-941703.js?cl=44382e9)  
  

---   

## **regress-crbug-959645-1.js (chromium issue)**  
   
**[Issue: DCHECK failure in value->IsSmi() in objects-debug.cc](https://crbug.com/959645)**  
**[Commit: [map] Make field representation updates work with elements kind transitions.](https://chromium.googlesource.com/v8/v8/+/6564c6d)**  
  
Date(Commit): Tue May 07 13:13:51 2019  
Components: Blink>JavaScript>Runtime  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-75, M-75  
Code Review: [http://bit.ly/v8-in-place-field-representation-changes](http://bit.ly/v8-in-place-field-representation-changes)  
Regress: [mjsunit/regress/regress-crbug-959645-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-959645-1.js), [mjsunit/regress/regress-crbug-959645-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-959645-2.js)  
```javascript
function f(array, x) {
  array.x = x;
  array[0] = 1.1;
  return array;
}

f([1], 1);
f([2], 1);
%HeapObjectVerify(f([3], undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6564c6d^!)  
[src/field-type.h](https://cs.chromium.org/chromium/src/v8/src/field-type.h?cl=6564c6d)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=6564c6d)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=6564c6d)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=6564c6d)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=6564c6d)  
...  
  

---   

## **regress-9105.js (v8 issue)**  
   
**[Issue: cnn.com freezes while loading under emulation](https://crbug.com/v8/9105)**  
**[Commit: Reland "[typedarray] Make JSTypedArray::length authoritative."](https://chromium.googlesource.com/v8/v8/+/330e5ba)**  
  
Date(Commit): Tue May 07 11:46:06 2019  
Code Review: [http://doc/1Z-wM2qwvAuxH46e9ivtkYvKzzwYZg8ymm0x0wJaomow](http://doc/1Z-wM2qwvAuxH46e9ivtkYvKzzwYZg8ymm0x0wJaomow)  
Regress: [mjsunit/regress/regress-9105.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9105.js)  
```javascript
let array = new Uint32Array(32);
array[10] = 10; array[20] = 20;

Array.prototype.sort.call(array);
assertEquals(32, array.length);
assertEquals(10, array[30]);
assertEquals(20, array[31]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/330e5ba^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=330e5ba)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=330e5ba)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=330e5ba)  
[src/builtins/builtins-sharedarraybuffer.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-sharedarraybuffer.cc?cl=330e5ba)  
[src/builtins/builtins-typed-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array.cc?cl=330e5ba)  
...  
  

---   

## **regress-957405.js (chromium issue)**  
   
**[Issue: DCHECK failure in trap_handler::IsTrapHandlerEnabled() == trap_handler::IsThreadInWasm() in runtim](https://crbug.com/957405)**  
**[Commit: [wasm] Disable asan for memory_fill_wrapper](https://chromium.googlesource.com/v8/v8/+/140c1e5)**  
  
Date(Commit): Sat May 04 03:36:36 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Needs-Feedback, Security_Severity-Low, Arch-All, Security_Impact-None, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-76  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1590386](https://chromium-review.googlesource.com/c/v8/v8/+/1590386)  
Regress: [mjsunit/regress/wasm/regress-957405.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-957405.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const memory = new WebAssembly.Memory({initial: 1});

let builder = new WasmModuleBuilder();
builder.addImportedMemory("imports", "mem");
builder.addFunction("fill", kSig_v_iii)
       .addBody([kExprLocalGet, 0, // dst
                 kExprLocalGet, 1, // value
                 kExprLocalGet, 2, // size
                 kNumericPrefix, kExprMemoryFill, 0]).exportAs("fill");
let instance = builder.instantiate({imports: {mem: memory}});
memory.grow(1);
assertTraps(
    kTrapMemOutOfBounds,
    () => instance.exports.fill(kPageSize + 1, 123, kPageSize));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/140c1e5^!)  
[src/wasm/wasm-external-refs.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-external-refs.cc?cl=140c1e5)  
[test/mjsunit/regress/wasm/regress-957405.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-957405.js?cl=140c1e5)  
  

---   

## **regress-958725.js (chromium issue)**  
   
**[Issue: DCHECK failure in 1 == effect->op()->EffectInputCount() in node-properties.cc](https://crbug.com/958725)**  
**[Commit: [turbofan] Handle unreachable code gracefully when searching framestates](https://chromium.googlesource.com/v8/v8/+/6d0078e)**  
  
Date(Commit): Fri May 03 09:51:47 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1594434](https://chromium-review.googlesource.com/c/v8/v8/+/1594434)  
Regress: [mjsunit/regress-958725.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-958725.js)  
```javascript
function f(v3) {
  Symbol[Symbol.replace] = Object;
  const v8 = {};
  let i = 0;
  do {
    const v12 = v3[3];
    for (let v17 = 0; v17 < 100000; v17++) {
    }
    const v18 = Object();
    function v19(v20, v21, v22) {}
    i++;;
  } while (i < 1);
  const v25 = Object.freeze(v8);
};
%PrepareFunctionForOptimization(f);
f(Object);
%OptimizeFunctionOnNextCall(f);
f(Object);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d0078e^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=6d0078e)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=6d0078e)  
[src/compiler/js-type-hint-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-type-hint-lowering.cc?cl=6d0078e)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=6d0078e)  
[src/compiler/node-properties.h](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.h?cl=6d0078e)  
...  
  

---   

## **regress-9017.js (v8 issue)**  
   
**[Issue: Liftoff needs to emit explicit stack checks on Windows](https://crbug.com/v8/9017)**  
**[Commit: Touch guard pages when allocating stack frames](https://chromium.googlesource.com/v8/v8/+/df8548c)**  
  
Date(Commit): Thu May 02 17:46:18 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1570666](https://chromium-review.googlesource.com/c/v8/v8/+/1570666)  
Regress: [mjsunit/regress/wasm/regress-9017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-9017.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();

var func_idx = builder.addFunction('helper', kSig_i_v)
    .addLocals({i32_count: 1})
    .addBody([
        kExprI32Const, 0x01,
    ]).index;

var large_function_body = [];
const num_temporaries = 16 * 1024;
for (let i = 0; i < num_temporaries; ++i) {
  large_function_body.push(kExprCallFunction, func_idx);
}
for (let i = 1; i < num_temporaries; ++i) {
  large_function_body.push(kExprI32Add);
}

builder.addFunction('test', kSig_i_v)
    .addBody(large_function_body)
    .exportFunc();
var module = builder.instantiate();

assertEquals(num_temporaries, module.exports.test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df8548c^!)  
[src/arm/assembler-arm-inl.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm-inl.h?cl=df8548c)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=df8548c)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=df8548c)  
...  
  

---   

## **regress-9017.js (v8 issue)**  
   
**[Issue: Liftoff needs to emit explicit stack checks on Windows](https://crbug.com/v8/9017)**  
**[Commit: Touch guard pages when allocating stack frames](https://chromium.googlesource.com/v8/v8/+/df8548c)**  
  
Date(Commit): Thu May 02 17:46:18 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1570666](https://chromium-review.googlesource.com/c/v8/v8/+/1570666)  
Regress: [mjsunit/regress/regress-9017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9017.js)  
```javascript
const frameSize = 4096 * 5;
const numValues = frameSize / 4;
const arr = new Array(numValues);
let counter = 10;
function f() { --counter; return 1 + (counter > 0 ? bound() : 0); }
const bound = f.bind.apply(f, arr);
bound();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df8548c^!)  
[src/arm/assembler-arm-inl.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm-inl.h?cl=df8548c)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=df8548c)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=df8548c)  
...  
  

---   

## **regress-9017.js (v8 issue)**  
   
**[Issue: Liftoff needs to emit explicit stack checks on Windows](https://crbug.com/v8/9017)**  
**[Commit: Touch guard pages when allocating stack frames](https://chromium.googlesource.com/v8/v8/+/df8548c)**  
  
Date(Commit): Thu May 02 17:46:18 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1570666](https://chromium-review.googlesource.com/c/v8/v8/+/1570666)  
Regress: [mjsunit/compiler/regress-9017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9017.js)  
```javascript
const frame_size = 4096 * 4;       // 4 pages
const num_locals = frame_size / 8; // Assume 8-byte floating point values

function f() { return 0.1; }

let g_text = "if (input === 0) return; if (input > 0) return g(input - 1);";
g_text += " var inc = f(); var a0 = 0;";
for (let i = 1; i < num_locals; ++i) {
  g_text += " var a" + i + " = a" + (i - 1) + " + inc;";
}
g_text += " return f(a0";
for (let i = 1; i < num_locals; ++i) {
  g_text += ", a" + i;
}
g_text += ");";
const g = new Function("input", g_text);

%PrepareFunctionForOptimization(g);
g(1);
g(-1);
%OptimizeFunctionOnNextCall(g);

g(20);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df8548c^!)  
[src/arm/assembler-arm-inl.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm-inl.h?cl=df8548c)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=df8548c)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=df8548c)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=df8548c)  
...  
  

---   

## **regress-958716.js (chromium issue)**  
   
**[Issue: DCHECK failure in map_.is_stable() in compilation-dependencies.cc](https://crbug.com/958716)**  
**[Commit: [turbofan] Fix a bug in DepenOnStablePrototypeChains](https://chromium.googlesource.com/v8/v8/+/87b3416)**  
  
Date(Commit): Thu May 02 14:25:03 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-None, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, merge-merged-7.5  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1593086](https://chromium-review.googlesource.com/c/v8/v8/+/1593086)  
Regress: [mjsunit/compiler/regress-958716.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-958716.js)  
```javascript
for (let i = 0; i < 2; i++) {
  new String().valueOf = Symbol;
}

function foo() {
  Promise.resolve("");
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87b3416^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=87b3416)  
[test/mjsunit/compiler/regress-958716.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958716.js?cl=87b3416)  
  

---   

## **regress-958021.js (chromium issue)**  
   
**[Issue: CHECK failure: UpdateType error for node 69: SpeculativeNumberLessThanOrEqual[Number](14, 66, 3](https://crbug.com/958021)**  
**[Commit: [turbofan] Fix monotonicity of ComparisonOutcome-related typings](https://chromium.googlesource.com/v8/v8/+/d83f023)**  
  
Date(Commit): Thu May 02 14:14:54 2019  
Components: Blink>JavaScript>Compiler  
Labels: Merge-na, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-74, M-74  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1593293](https://chromium-review.googlesource.com/c/v8/v8/+/1593293)  
Regress: [mjsunit/compiler/regress-958021.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-958021.js)  
```javascript
function v0() {
  let v7 = -4294967295;
  try {
    for (let v11 = 0; v11 < 8; v11++) {
      const v13 = Symbol.isConcatSpreadable;
      const v14 = v11 && v13;
      const v15 = v7 <= v14;
      for (var i = 0; i < 10; i++) {}
    }
  } catch (v20) {
  }
};
%PrepareFunctionForOptimization(v0);
v0();
v0();
%OptimizeFunctionOnNextCall(v0);
v0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d83f023^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=d83f023)  
[test/mjsunit/compiler/regress-958021.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958021.js?cl=d83f023)  
  

---   

## **regress-952682.js (chromium issue)**  
   
**[Issue: DCHECK failure in value->IsSmi() in objects-debug.cc](https://crbug.com/952682)**  
**[Commit: Turn off in-place field representation changes](https://chromium.googlesource.com/v8/v8/+/3ce92ce)**  
  
Date(Commit): Thu May 02 11:52:20 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-75, Merge-Merged-75-3770  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1593083](https://chromium-review.googlesource.com/c/v8/v8/+/1593083)  
Regress: [mjsunit/regress-952682.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-952682.js)  
```javascript
function f(array, x) {
  array.x = x;
  array[0] = undefined;
  return array;
}

f([1], 1);
f([2], 1);
%HeapObjectVerify(f([3], undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ce92ce^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=3ce92ce)  
[test/mjsunit/regress-952682.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-952682.js?cl=3ce92ce)  
  

---   

## **regress-958420.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::MapInference::~MapInference](https://crbug.com/958420)**  
**[Commit: [turbofan] Fix two bugs in ReduceArrayIteratorPrototypeNext](https://chromium.googlesource.com/v8/v8/+/053393d)**  
  
Date(Commit): Thu May 02 11:31:30 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1593077](https://chromium-review.googlesource.com/c/v8/v8/+/1593077)  
Regress: [mjsunit/compiler/regress-958420.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-958420.js)  
```javascript
var a = [];

function foo() {
  return a[Symbol.iterator]().next();
};
%PrepareFunctionForOptimization(foo);
a.__proto__.push(5);
a.bla = {};

foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/053393d^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=053393d)  
[test/mjsunit/compiler/regress-958350.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958350.js?cl=053393d)  
[test/mjsunit/compiler/regress-958420.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958420.js?cl=053393d)  
  

---   

## **regress-958350.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::Isolate::PushStackTraceAndDie](https://crbug.com/958350)**  
**[Commit: [turbofan] Fix two bugs in ReduceArrayIteratorPrototypeNext](https://chromium.googlesource.com/v8/v8/+/053393d)**  
  
Date(Commit): Thu May 02 11:31:30 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1593077](https://chromium-review.googlesource.com/c/v8/v8/+/1593077)  
Regress: [mjsunit/compiler/regress-958350.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-958350.js)  
```javascript
function foo(o) {
  for (const x of o) {
    o[100] = 1;
    try { x.push(); } catch (e) {}
  }
}

%PrepareFunctionForOptimization(foo);
foo([1]);
foo([1]);
%OptimizeFunctionOnNextCall(foo);
foo([1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/053393d^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=053393d)  
[test/mjsunit/compiler/regress-958350.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958350.js?cl=053393d)  
[test/mjsunit/compiler/regress-958420.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-958420.js?cl=053393d)  
  

---   

## **regress-957559.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/957559)**  
**[Commit: [turbofan] Handle -0 truncation in word32->float64 rep change.](https://chromium.googlesource.com/v8/v8/+/da6ebfa)**  
  
Date(Commit): Tue Apr 30 13:21:21 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1588428](https://chromium-review.googlesource.com/c/v8/v8/+/1588428)  
Regress: [mjsunit/compiler/regress-957559.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-957559.js)  
```javascript
const v0 = [];
function f(b) {
  for (let v13 = 0; v13 <= 3; v13 = v13 + 2241165261) {
    for (let i = 0; i < 8; i++) {}
    const v23 = Math.max(v13, -0.0, -2523259642);
    const v24 = v0[v23];
  }
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da6ebfa^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=da6ebfa)  
[test/mjsunit/compiler/regress-957559.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-957559.js?cl=da6ebfa)  
  

---   

## **regress-956771.js (chromium issue)**  
   
**[Issue: DCHECK failure in FLAG_wasm_lazy_compilation || (FLAG_asm_wasm_lazy_compilation && module->origin ](https://crbug.com/956771)**  
**[Commit: [wasm] Fix Wasm Lazy Compilation](https://chromium.googlesource.com/v8/v8/+/197b1d9)**  
  
Date(Commit): Tue Apr 30 13:05:20 2019  
Components: Blink>JavaScript  
Labels: Reproducible, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-77  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1585843](https://chromium-review.googlesource.com/c/v8/v8/+/1585843)  
Regress: [mjsunit/regress/wasm/regress-956771.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-956771.js), [mjsunit/regress/wasm/regress-956771b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-956771b.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function testLazyModuleAsyncCompilation() {
  print(arguments.callee.name);
  let builder = new WasmModuleBuilder();
  builder.addFunction("some", kSig_i_ii)
  assertPromiseResult(WebAssembly.compile(builder.toBuffer())
    .then(assertUnreachable,
          error => assertEquals("WebAssembly.compile(): function body must " +
                                "end with \"end\" opcode @+26",
                                error.message)));
})();

(function testLazyModuleSyncCompilation() {
  print(arguments.callee.name);
  let builder = new WasmModuleBuilder();
  builder.addFunction("some", kSig_i_ii)
  assertThrows(() => builder.toModule(),
               WebAssembly.CompileError,
               "WebAssembly.Module(): Compiling function #0:\"some\" failed: " +
               "function body must end with \"end\" opcode @+26");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/197b1d9^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=197b1d9)  
[src/wasm/module-compiler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.h?cl=197b1d9)  
[src/wasm/wasm-code-manager.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.h?cl=197b1d9)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=197b1d9)  
[test/mjsunit/regress/wasm/regress-956771.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-956771.js?cl=197b1d9)  
...  
  

---   

## **regress-9137-1.js (v8 issue)**  
   
**[Issue: Function.prototype.bind reduction can result in deopt loop](https://crbug.com/v8/9137)**  
**[Commit: [turbofan] Avoid raw InferReceiverMaps in JSCallReducer](https://chromium.googlesource.com/v8/v8/+/9284ad5)**  
  
Date(Commit): Tue Apr 30 09:19:56 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1578501](https://chromium-review.googlesource.com/c/v8/v8/+/1578501)  
Regress: [mjsunit/compiler/regress-9137-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9137-1.js), [mjsunit/compiler/regress-9137-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9137-2.js)  
```javascript
function changeMap(obj) {
  obj.blub = 42;
}

function foo(obj) {
  return obj.bind(changeMap(obj));
}

%NeverOptimizeFunction(changeMap);
%PrepareFunctionForOptimization(foo);
foo(function(){});
foo(function(){});
%OptimizeFunctionOnNextCall(foo);
foo(function(){});
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo(function(){});
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9284ad5^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=9284ad5)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=9284ad5)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=9284ad5)  
[test/mjsunit/compiler/regress-9137-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-9137-1.js?cl=9284ad5)  
[test/mjsunit/compiler/regress-9137-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-9137-2.js?cl=9284ad5)  
  

---   

## **regress-956426.js (chromium issue)**  
   
**[Issue: DCHECK failure in old_descriptors_->GetDetails(modified_descriptor_) .representation() .Equals(new](https://crbug.com/956426)**  
**[Commit: Avoid adding integrity level transitions to deprecated maps.](https://chromium.googlesource.com/v8/v8/+/a474dbc)**  
  
Date(Commit): Sun Apr 28 14:11:01 2019  
Components: Blink>JavaScript>Runtime  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-75, M-75, Merge-Merged-75-3770  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1585858](https://chromium-review.googlesource.com/c/v8/v8/+/1585858)  
Regress: [mjsunit/regress-956426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-956426.js)  
```javascript
var b = { x: 0, y: 0, 0: '' };
var a = { x: 0, y: 100000000000, 0: '' };
Object.seal(b);
b.x = '';  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a474dbc^!)  
[src/objects/js-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/js-objects.cc?cl=a474dbc)  
[test/mjsunit/regress-956426.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-956426.js?cl=a474dbc)  
  

---   

## **regress-952342.js (chromium issue)**  
   
**[Issue: DCHECK failure in trap_handler::IsTrapHandlerEnabled() == trap_handler::IsThreadInWasm() in runtim](https://crbug.com/952342)**  
**[Commit: [wasm] Disable asan for memory_copy_wrapper](https://chromium.googlesource.com/v8/v8/+/eb131dc)**  
  
Date(Commit): Fri Apr 26 11:21:21 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Severity-Low, Security_Impact-None, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1584326](https://chromium-review.googlesource.com/c/v8/v8/+/1584326)  
Regress: [mjsunit/regress/wasm/regress-952342.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-952342.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const memory = new WebAssembly.Memory({initial: 1});

let builder = new WasmModuleBuilder();
builder.addImportedMemory("imports", "mem", 1);
builder.addFunction("copy", kSig_v_iii)
       .addBody([kExprLocalGet, 0, // dst
                 kExprLocalGet, 1, // src
                 kExprLocalGet, 2, // size
                 kNumericPrefix, kExprMemoryCopy, 0, 0]).exportAs("copy");
let instance = builder.instantiate({imports: {mem: memory}});
memory.grow(1);
instance.exports.copy(0, kPageSize, 11);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eb131dc^!)  
[src/wasm/wasm-external-refs.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-external-refs.cc?cl=eb131dc)  
[test/mjsunit/regress/wasm/regress-952342.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-952342.js?cl=eb131dc)  
  

---   

## **regress-crbug-9161.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/9161)**  
**[Commit: [typedarray] Fix crash when sorting SharedArrayBuffers](https://chromium.googlesource.com/v8/v8/+/3d84611)**  
  
Date(Commit): Thu Apr 25 09:54:25 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1581641](https://chromium-review.googlesource.com/c/v8/v8/+/1581641)  
Regress: [mjsunit/regress/regress-crbug-9161.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-9161.js)  
```javascript
const lock = new Int32Array(new SharedArrayBuffer(4));

const kIterations = 5000;
const kLength = 2000;

const kStageIndex = 0;
const kStageInit = 0;
const kStageRunning = 1;
const kStageDone = 2;

Atomics.store(lock, kStageIndex, kStageInit);

function WaitUntil(expected) {
  while (true) {
    const value = Atomics.load(lock, kStageIndex);
    if (value === expected) break;
  }
}

const workerScript = `
  onmessage = function([sab, lock]) {
    const i32a = new Int32Array(sab);
    Atomics.store(lock, ${kStageIndex}, ${kStageRunning});

    for (let j = 1; j < ${kIterations}; ++j) {
      for (let i = 0; i < i32a.length; ++i) {
        i32a[i] = j;
      }
    }

    postMessage("done");
    Atomics.store(lock, ${kStageIndex}, ${kStageDone});
  };`;

const worker = new Worker(workerScript, {type: 'string'});

const i32a = new Int32Array(
  new SharedArrayBuffer(Int32Array.BYTES_PER_ELEMENT * kLength)
);

worker.postMessage([i32a.buffer, lock]);
WaitUntil(kStageRunning);

for (let i = 0; i < kIterations; ++i) {
  i32a.sort();
}

WaitUntil(kStageDone);
assertEquals(worker.getMessage(), "done");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d84611^!)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=3d84611)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=3d84611)  
[test/mjsunit/regress/regress-crbug-9161.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-9161.js?cl=3d84611)  
  

---   

## **regress-952586.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/952586)**  
**[Commit: [turbofan] Fix bounds check for the 'in' operator on typed arrays.](https://chromium.googlesource.com/v8/v8/+/d2bfdaf)**  
  
Date(Commit): Wed Apr 24 11:52:17 2019  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, ReleaseBlock-Stable, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-CC, M-75, merge-merged-7.5  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1581179](https://chromium-review.googlesource.com/c/v8/v8/+/1581179)  
Regress: [mjsunit/compiler/regress-952586.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-952586.js)  
```javascript
a = new Int8Array(1);

function f(i) {
  return i in a;
};
%PrepareFunctionForOptimization(f);
assertTrue(f(0));
%OptimizeFunctionOnNextCall(f);
assertFalse(f(-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2bfdaf^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=d2bfdaf)  
[test/mjsunit/compiler/regress-952586.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-952586.js?cl=d2bfdaf)  
  

---   

## **regress-9165.js (v8 issue)**  
   
**[Issue: Overly zealous DCHECK for reference types](https://crbug.com/v8/9165)**  
**[Commit: [wasm] Fix DCHECK in MergeValuesInto for reference types.](https://chromium.googlesource.com/v8/v8/+/0c9c8a9)**  
  
Date(Commit): Wed Apr 24 09:32:17 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1578463](https://chromium-review.googlesource.com/c/v8/v8/+/1578463)  
Regress: [mjsunit/regress/regress-9165.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-9165.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let kSig_r_i = makeSig([kWasmI32], [kWasmAnyRef]);

(function TestMergeOfAnyFuncIntoAnyRef() {
  print(arguments.callee.name);
  let builder = new WasmModuleBuilder();
  builder.addFunction("merge", kSig_r_i)
      .addLocals({anyref_count: 1, anyfunc_count: 1})
      .addBody([
        kExprLocalGet, 0,
        kExprI32Eqz,
        kExprIf, kWasmAnyRef,
          kExprLocalGet, 1,
        kExprElse,
          kExprLocalGet, 2,
        kExprEnd,
      ]).exportFunc();
  let instance = builder.instantiate();
  assertEquals(null, instance.exports.merge(0));
  assertEquals(null, instance.exports.merge(1));
})();

(function TestMergeOfAnyFuncIntoNullRef() {
  print(arguments.callee.name);
  let builder = new WasmModuleBuilder();
  builder.addFunction("merge", kSig_r_i)
      .addLocals({anyfunc_count: 1})
      .addBody([
        kExprLocalGet, 0,
        kExprI32Eqz,
        kExprIf, kWasmAnyRef,
          kExprRefNull,
        kExprElse,
          kExprLocalGet, 1,
        kExprEnd,
      ]).exportFunc();
  let instance = builder.instantiate();
  assertEquals(null, instance.exports.merge(0));
  assertEquals(null, instance.exports.merge(1));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c9c8a9^!)  
[src/wasm/graph-builder-interface.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/graph-builder-interface.cc?cl=0c9c8a9)  
[src/wasm/value-type.h](https://cs.chromium.org/chromium/src/v8/src/wasm/value-type.h?cl=0c9c8a9)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=0c9c8a9)  
[test/mjsunit/regress/regress-9165.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-9165.js?cl=0c9c8a9)  
  

---   

## **regress-crbug-935800.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/935800)**  
**[Commit: [asm.js] Exported functions diverge from wasm js-api spec.](https://chromium.googlesource.com/v8/v8/+/6957e23)**  
  
Date(Commit): Tue Apr 23 11:54:01 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://webassembly.github.io/spec/js-api/index.html#exported-function-exotic-objects](https://webassembly.github.io/spec/js-api/index.html#exported-function-exotic-objects)  
Regress: [mjsunit/regress/regress-crbug-935800.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-935800.js)  
```javascript
function foo() {
  "use asm";
  function bar() {}
  return {bar: bar};
}
var module = foo();
assertTrue(Object.getOwnPropertyNames(module.bar).includes("prototype"));
assertInstanceof(new module.bar(), module.bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6957e23^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=6957e23)  
[test/mjsunit/regress/regress-crbug-935800.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-935800.js?cl=6957e23)  
[tools/clusterfuzz/v8_sanity_checks.js](https://cs.chromium.org/chromium/src/v8/tools/clusterfuzz/v8_sanity_checks.js?cl=6957e23)  
  

---   

## **regress-crbug-951400.js (chromium issue)**  
   
**[Issue: DCHECK failure in AllowHeapAllocation::IsAllowed() in heap-inl.h](https://crbug.com/951400)**  
**[Commit: [test] Fix a regressed DCHECK in JSInliner](https://chromium.googlesource.com/v8/v8/+/c8763dd)**  
  
Date(Commit): Thu Apr 18 16:06:12 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1571614](https://chromium-review.googlesource.com/c/v8/v8/+/1571614)  
Regress: [mjsunit/regress/regress-crbug-951400.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-951400.js)  
```javascript
function foo(arr) {
  gc();
  eval(arr);
};
%PrepareFunctionForOptimization(foo);
try {
  foo("tag`Hello${tag}`");
} catch (e) {}

%OptimizeFunctionOnNextCall(foo);

try {
  foo("tag.prop`${tag}`");
} catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8763dd^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c8763dd)  
[test/mjsunit/regress/regress-crbug-951400.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-951400.js?cl=c8763dd)  
  

---   

## **regress-v8-9139.js (v8 issue)**  
   
**[Issue: Deopt loop when storing same (non-smi) numbers into const tagged field.](https://crbug.com/v8/9139)**  
**[Commit: [turbofan] Use the right comparison for constant field store.](https://chromium.googlesource.com/v8/v8/+/2c5f11f)**  
  
Date(Commit): Thu Apr 18 11:29:22 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1569438](https://chromium-review.googlesource.com/c/v8/v8/+/1569438)  
Regress: [mjsunit/compiler/regress-v8-9139.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-9139.js)  
```javascript
let dummy = { x : {} };

let o = { x : 0.1 };

function f(o, a, b) {
  o.x = a + b;
}

%PrepareFunctionForOptimization(f);
f(o, 0.05, 0.05);
f(o, 0.05, 0.05);
%OptimizeFunctionOnNextCall(f);
f(o, 0.05, 0.05);
assertOptimized(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2c5f11f^!)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=2c5f11f)  
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=2c5f11f)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=2c5f11f)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=2c5f11f)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=2c5f11f)  
...  
  

---   

## **regress-952722.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_resolved() in ast.h](https://crbug.com/952722)**  
**[Commit: [ast] simplify ClassScope::ResolvePrivateNamesPartially](https://chromium.googlesource.com/v8/v8/+/9ace845)**  
  
Date(Commit): Tue Apr 16 11:08:40 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1567709](https://chromium-review.googlesource.com/c/v8/v8/+/1567709)  
Regress: [mjsunit/harmony/regress/regress-952722.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-952722.js)  
```javascript
class A {
  static #a = 1;
  static b = class {
    static get_A() { return val.#a; }
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9ace845^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=9ace845)  
[test/mjsunit/harmony/regress/regress-952722.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-952722.js?cl=9ace845)  
  

---   

## **regress-v8-9113.js (v8 issue)**  
   
**[Issue: Constant field tracking ignores field change from 0 to -0](https://crbug.com/v8/9113)**  
**[Commit: [turbofan] Switch equality check for constant fields to SameValue.](https://chromium.googlesource.com/v8/v8/+/42b90af)**  
  
Date(Commit): Thu Apr 11 11:59:24 2019  
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

## **regress-crbug-950747.js (chromium issue)**  
   
**[Issue: DCHECK: !initializing_store && property_details_.constness() == PropertyConstness::kConst implies IsConstFieldValueEqualTo(*value)](https://crbug.com/950747)**  
**[Commit: [ic] Fix handling of +0/-0 when constant field tracking is enabled](https://chromium.googlesource.com/v8/v8/+/94c87fe)**  
  
Date(Commit): Thu Apr 11 11:28:13 2019  
Components: Blink>JavaScript  
Labels: reward-0, Arch-x86_64, allpublic, ClusterFuzz-Verified, Via-Wizard-Security, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1561319](https://chromium-review.googlesource.com/c/v8/v8/+/1561319)  
Regress: [mjsunit/regress/regress-crbug-950747.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-950747.js)  
```javascript
let o = {};
Reflect.set(o, "a", 0.1);

let o1 = {};
o1.a = {};

Reflect.set(o, "a", 0.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/94c87fe^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=94c87fe)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=94c87fe)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=94c87fe)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=94c87fe)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=94c87fe)  
...  
  

---   

## **regress-v8-9106.js (v8 issue)**  
   
**[Issue: [wasm] [bulk memory] DCHECK when passive data segment is at end of module](https://crbug.com/v8/9106)**  
**[Commit: [wasm] Fix DCHECK with empty passive data segment](https://chromium.googlesource.com/v8/v8/+/b29993f)**  
  
Date(Commit): Wed Apr 10 18:10:58 2019  
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

## **regress-950328.js (chromium issue)**  
   
**[Issue: v8 crash on map-check](https://crbug.com/950328)**  
**[Commit: Avoid making maps unstable in keyed store IC.](https://chromium.googlesource.com/v8/v8/+/5ef8846)**  
  
Date(Commit): Wed Apr 10 14:30:57 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Merge-na, reward-3000, Security_Impact-Stable, Arch-x86_64, Security_Severity-High, allpublic, reward-inprocess, ClusterFuzz-Verified, Test-Predator-Wrong-CLs, Via-Wizard-Security, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, CVE_description-submitted, Target-75, M-75, Release-0-M75, CVE-2019-5831  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1559867](https://chromium-review.googlesource.com/c/v8/v8/+/1559867)  
Regress: [mjsunit/regress/regress-950328.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-950328.js)  
```javascript
(function NoStoreBecauseReadonlyLength() {
  var a = [];
  Object.defineProperty(a, 'length', { writable: false });


  function f() {
    var o = {__proto__: a};
    o.push;
  };
  %PrepareFunctionForOptimization(f);
  f();
  f();
  %OptimizeFunctionOnNextCall(f);

  a[0] = 1.1;
  f();
  assertEquals(undefined, a[0]);
})();

(function NoStoreBecauseTypedArrayProto() {
  const arr_proto = [].__proto__;
  const arr = [];

  function f() {
    const i32arr = new Int32Array();

    const obj = {};
    obj.__proto__ = arr;
    arr_proto.__proto__ = i32arr;
    obj.__proto__ = arr;
    arr_proto.__proto__ = i32arr;
  };
  %PrepareFunctionForOptimization(f);
  f();
  %OptimizeFunctionOnNextCall(f);
  arr[1024] = [];
  f();
  assertEquals(undefined, arr[1024]);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ef8846^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=5ef8846)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=5ef8846)  
[test/mjsunit/regress/regress-950328.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-950328.js?cl=5ef8846)  
  

---   

## **regress-947822.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/947822)**  
**[Commit: [regexp] Ensure ToString(replaceValue) is called once in @@replace](https://chromium.googlesource.com/v8/v8/+/f8d1169)**  
  
Date(Commit): Wed Apr 10 07:12:14 2019  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
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
   
**[Issue: mjsunit/regress/polymorphic-accessor-test-context starts flaking on gc fuzzer](https://crbug.com/v8/9087)**  
**[Commit: [turbofan] Add a regression test](https://chromium.googlesource.com/v8/v8/+/d97bc8d)**  
  
Date(Commit): Fri Apr 05 13:57:56 2019  
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

## **regress-949435.js (chromium issue)**  
   
**[Issue: v8 crash on TryUpdateSlow debug check](https://crbug.com/949435)**  
**[Commit: Fix Map::TryUpdate assertion.](https://chromium.googlesource.com/v8/v8/+/4a68b29)**  
  
Date(Commit): Thu Apr 04 19:27:29 2019  
Components: Blink>JavaScript  
Labels: Arch-x86_64, allpublic, ClusterFuzz-Verified, Via-Wizard-Security, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?l=330&amp;rcl=5671f8b940b0fcdb550e318e449ded0f866e935a](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?l=330&amp;rcl=5671f8b940b0fcdb550e318e449ded0f866e935a)  
Regress: [mjsunit/compiler/regress-949435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-949435.js)  
```javascript
function f() {
  const v6 = new String();
  v6.POSITIVE_INFINITY = 1337;
  const v8 = Object.seal(v6);
  v8.POSITIVE_INFINITY = Object;
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a68b29^!)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=4a68b29)  
[test/mjsunit/compiler/regress-949435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-949435.js?cl=4a68b29)  
  

---   

## **regress-crbug-944865.js (chromium issue)**  
   
**[Issue: DCHECK failure in object->FitsRepresentation(representation) in objects.cc](https://crbug.com/944865)**  
**[Commit: [turbofan] Bail out for accesses to fields with representation None.](https://chromium.googlesource.com/v8/v8/+/acdeb64)**  
  
Date(Commit): Wed Apr 03 15:07:00 2019  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Merge-Rejected-73  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1549167](https://chromium-review.googlesource.com/c/v8/v8/+/1549167)  
Regress: [mjsunit/regress/regress-crbug-944865.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-944865.js)  
```javascript
function foo() {
  const r = {e: NaN, g: undefined, c: undefined};
  const u = {__proto__: {}, e: new Set(), g: 0, c: undefined};
  return r;
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
const o = foo();
Object.defineProperty(o, 'c', {value: 42});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/acdeb64^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=acdeb64)  
[test/mjsunit/regress/regress-crbug-944865.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-944865.js?cl=acdeb64)  
  

---   

## **regress-948228.js (chromium issue)**  
   
**[Issue: DCHECK failure in *isolate->external_caught_exception_address() in wasm-engine.cc](https://crbug.com/948228)**  
**[Commit: [wasm] Remove wrong DCHECK](https://chromium.googlesource.com/v8/v8/+/fe00be4)**  
  
Date(Commit): Wed Apr 03 11:15:53 2019  
Components: Blink>JavaScript  
Labels: Merge-na, Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-75, Release-0-M75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1547855](https://chromium-review.googlesource.com/c/v8/v8/+/1547855)  
Regress: [mjsunit/regress/wasm/regress-948228.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-948228.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var {proxy, revoke} = Proxy.revocable({}, {});
revoke();
let builder = new WasmModuleBuilder();
builder.addImport('m', 'q', kSig_v_v);
WebAssembly.instantiate(builder.toModule(), proxy);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe00be4^!)  
[src/wasm/wasm-engine.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-engine.cc?cl=fe00be4)  
[test/mjsunit/regress/wasm/regress-948228.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-948228.js?cl=fe00be4)  
  

---   

## **regress-948307.js (chromium issue)**  
   
**[Issue: DCHECK failure in ObjectInYoungGeneration(HeapObjectSlot(slot).ToHeapObject()) in heap.cc](https://crbug.com/948307)**  
**[Commit: [heap] Do not {RecordEphemeronKeyWrite} if key is in old-space](https://chromium.googlesource.com/v8/v8/+/50d74d6)**  
  
Date(Commit): Tue Apr 02 13:24:33 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1547858](https://chromium-review.googlesource.com/c/v8/v8/+/1547858)  
Regress: [mjsunit/regress/regress-948307.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-948307.js)  
```javascript
const set = new WeakSet()
const obj = {};
gc();
gc();
const foo = new Int8Array(0x0F000000);
set.add(obj);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/50d74d6^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=50d74d6)  
[test/mjsunit/regress/regress-948307.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-948307.js?cl=50d74d6)  
  

---   

## **regress-948248.js (chromium issue)**  
   
**[Issue: Security: Debug check failed: name->is_one_byte() src/parsing/parser.cc, line 350](https://crbug.com/948248)**  
**[Commit: [parser] Fail early for two-byte intrinsic calls](https://chromium.googlesource.com/v8/v8/+/837e8f5)**  
  
Date(Commit): Tue Apr 02 10:43:12 2019  
Components: Blink>JavaScript>Parser  
Labels: reward-0, Security_Severity-Low, Security_Impact-Stable, allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Release-0-M75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1547857](https://chromium-review.googlesource.com/c/v8/v8/+/1547857)  
Regress: [mjsunit/regress/regress-948248.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-948248.js)  
```javascript
assertThrows("%ಠ_ಠ()", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/837e8f5^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=837e8f5)  
[test/mjsunit/regress/regress-948248.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-948248.js?cl=837e8f5)  
  

---   

## **regress-9041.js (v8 issue)**  
   
**[Issue: instanceof optimization doesn't add proper stability dependencies](https://crbug.com/v8/9041)**  
**[Commit: [turbofan] Fix bug in InferHasInPrototypeChain](https://chromium.googlesource.com/v8/v8/+/4c35194)**  
  
Date(Commit): Mon Apr 01 12:13:48 2019  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1541107](https://chromium-review.googlesource.com/c/v8/v8/+/1541107)  
Regress: [mjsunit/compiler/regress-9041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-9041.js)  
```javascript
(function() {
  class A {};

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
  assertFalse(foo(new A(), a => { a.__proto__ = {}; }));
})();

(function() {
  class A {};
  A.__proto__ = {};
  A.prototype = {};

  function foo() {
    var x = Object.create(Object.create(Object.create(A.prototype)));
    return x instanceof A;
  };

  %PrepareFunctionForOptimization(foo);
  assertTrue(foo());
  assertTrue(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertTrue(foo());
})();

(function() {
  class A {};
  A.prototype = {};
  A.__proto__ = {};
  var a = {__proto__: new A, gaga: 42};

  function foo() {
    A.bla;  // Make A.__proto__ fast again.
    a.gaga;
    return a instanceof A;
  };

  %PrepareFunctionForOptimization(foo);
  assertTrue(foo());
  assertTrue(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertTrue(foo());
})();

(function() {
  class A {};
  A.prototype = {};
  A.__proto__ = {};
  const boundA = Function.prototype.bind.call(A, {});
  boundA.prototype = {};
  boundA.__proto__ = {};
  var a = {__proto__: new boundA, gaga: 42};

  function foo() {
    A.bla;  // Make A.__proto__ fast again.
    boundA.bla;  // Make boundA.__proto__ fast again.
    a.gaga;
    return a instanceof boundA;
  };

  %PrepareFunctionForOptimization(foo);
  assertTrue(foo());
  assertTrue(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertTrue(foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c35194^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=4c35194)  
[src/compiler/compilation-dependencies.h](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.h?cl=4c35194)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=4c35194)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=4c35194)  
[test/mjsunit/compiler/regress-9041.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-9041.js?cl=4c35194)  
  

---   

## **regress-946889.js (chromium issue)**  
   
**[Issue: v8 debug version crash when CreateGraph phase](https://crbug.com/946889)**  
**[Commit: [turbofan] Fix bug in JSStoreInArrayLiteral](https://chromium.googlesource.com/v8/v8/+/8d6da70)**  
  
Date(Commit): Mon Apr 01 11:58:27 2019  
Components: Blink>JavaScript>Compiler  
Labels: reward-0, Security_Severity-Low, Security_Impact-Stable, Arch-x86_64, allpublic, ClusterFuzz-Verified, Via-Wizard-Security, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Release-0-M75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1547655](https://chromium-review.googlesource.com/c/v8/v8/+/1547655)  
Regress: [mjsunit/compiler/regress-946889.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-946889.js)  
```javascript
Object.preventExtensions(Array.prototype);

function foo() {
  var arr = [];
  [...arr, 42, null];
  arr.length = 1;
};
%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d6da70^!)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=8d6da70)  
[src/compiler/js-generic-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.h?cl=8d6da70)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=8d6da70)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=8d6da70)  
[test/mjsunit/compiler/regress-946889.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-946889.js?cl=8d6da70)  
  

---   

## **regress-945644.js (chromium issue)**  
   
**[Issue: Security: Failed Debug Check in src/compiler/verifier.cc, line 121](https://crbug.com/945644)**  
**[Commit: [turbofan] Make sure nodes are killed on replacement](https://chromium.googlesource.com/v8/v8/+/1ec7ffe)**  
  
Date(Commit): Fri Mar 29 08:52:20 2019  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, reward-3000, Security_Impact-Stable, Security_Severity-High, allpublic, reward-inprocess, CVE_description-submitted, Target-74, M-74, Merge-Approved-7.4, Release-0-M74, CVE-2019-5807, VulnerabilityAnalysis-Requested  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1545229](https://chromium-review.googlesource.com/c/v8/v8/+/1545229)  
Regress: [mjsunit/compiler/regress-945644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-945644.js)  
```javascript
function f(v5, v6) {
  const v16 = [1337, 1337, -765470.5051836492];
  let v19 = 0;
  do {
    const v20 = v19 + 1;
    const v22 = Math.fround(v20);
    v19 = v22;
    const v23 = [v20, v22];
    function v24() {
      v20;
      v22;
    }
    const v33 = v16.indexOf(v19);
  } while (v19 < 6);
};
%PrepareFunctionForOptimization(f);
f();
Array.prototype.push(8);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ec7ffe^!)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=1ec7ffe)  
[test/mjsunit/compiler/regress-945644.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-945644.js?cl=1ec7ffe)  
  

---   

## **regress-944945.js (chromium issue)**  
   
**[Issue: CHECK failure: !result.failed() in wasm-engine.cc](https://crbug.com/944945)**  
**[Commit: [asmjs] Check function body size limit](https://chromium.googlesource.com/v8/v8/+/766edfc)**  
  
Date(Commit): Wed Mar 27 17:20:20 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-74  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1541479](https://chromium-review.googlesource.com/c/v8/v8/+/1541479)  
Regress: [mjsunit/regress/regress-944945.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-944945.js)  
```javascript
const E = '"use asm";\nfunction f() { LOCALS }\nreturn f;';
const PI = new Function(E.replace('LOCALS', Array(999995).fill('0.9')));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/766edfc^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=766edfc)  
[src/wasm/wasm-engine.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-engine.cc?cl=766edfc)  
[test/mjsunit/regress/regress-944945.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-944945.js?cl=766edfc)  
  

---   

## **regress-crbug-944971.js (chromium issue)**  
   
**[Issue: Security: OOB memory access in v8 regexp](https://crbug.com/944971)**  
**[Commit: [regexp] Refactor Regexp.prototype[@@replace]](https://chromium.googlesource.com/v8/v8/+/2ee4300)**  
  
Date(Commit): Wed Mar 27 13:15:16 2019  
Components: Blink>JavaScript>GC, Blink>JavaScript>Regexp  
Labels: Hotlist-Merge-Review, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Components, M-73, M-74, Merge-Merged-73, Merge-Merged-74, Release-0-M74  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1541477](https://chromium-review.googlesource.com/c/v8/v8/+/1541477)  
Regress: [mjsunit/regress/regress-crbug-944971.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-944971.js)  
```javascript
let re = /x/y;
let cnt = 0;
let str = re[Symbol.replace]("x", {
  toString: () => {
    cnt++;
    if (cnt == 2) {
      re.lastIndex = {valueOf: () => {
        re.x = 42;
        return 0;
      }};
    }
    return 'y$';
  }
});
assertEquals("y$", str);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ee4300^!)  
[src/regexp/regexp-utils.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-utils.cc?cl=2ee4300)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=2ee4300)  
[test/mjsunit/regress/regress-crbug-944971.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-944971.js?cl=2ee4300)  
  

---   

## **regress-946350.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::Object::Number](https://crbug.com/946350)**  
**[Commit: [wasm] Fix missing GC visit of instance elements](https://chromium.googlesource.com/v8/v8/+/6111c61)**  
  
Date(Commit): Wed Mar 27 13:04:26 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Severity-Medium, Security_Impact-Beta, allpublic, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-75, M-75  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1541237](https://chromium-review.googlesource.com/c/v8/v8/+/1541237)  
Regress: [mjsunit/regress/wasm/regress-946350.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-946350.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
var instance = builder.instantiate();
instance[1] = undefined;
gc();
Object.getOwnPropertyNames(instance);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6111c61^!)  
[src/objects-body-descriptors-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-body-descriptors-inl.h?cl=6111c61)  
[test/mjsunit/regress/wasm/regress-946350.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-946350.js?cl=6111c61)  
  

---   

## **regress-9036-1.js (v8 issue)**  
   
**[Issue: Proxy implements getPrototypeOf uncorrectly](https://crbug.com/v8/9036)**  
**[Commit: [csa] Fix instanceof for LHS with proxy in prototype chain](https://chromium.googlesource.com/v8/v8/+/b9076b4)**  
  
Date(Commit): Tue Mar 26 19:35:25 2019  
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

## **regress-crbug-944435.js (chromium issue)**  
   
**[Issue: CHECK failure: (value & uint64_t{ADDRESS}) != unexpected || (value & uint64_t{ADDRESS}) == uint](https://crbug.com/944435)**  
**[Commit: [Builtins] Make it harder to store signalling NaNs in Torque/CSA](https://chromium.googlesource.com/v8/v8/+/539017b)**  
  
Date(Commit): Tue Mar 26 10:22:50 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-0, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, merge-merged-3729  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1538517](https://chromium-review.googlesource.com/c/v8/v8/+/1538517)  
Regress: [mjsunit/regress/regress-crbug-944435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-944435.js)  
```javascript
function foo(  )  {
  return [
  0,
  1,
  2,
  3,
  4,
  5,
  6,
  7,
  8,
  9,
  10,
  0x1000000,
  0x40000000,
  12,
  60,
  100,
  1000 * 60 * 60 * 24].map(Math.asin);
}

let b = [];
b.constructor = {};
b.constructor[Symbol.species] = function() {};

let a = [];
for (let i = 0; i < 10; i++) {
  a.push(foo());
  gc();
  gc();
  gc();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/539017b^!)  
[src/builtins/array-map.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-map.tq?cl=539017b)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=539017b)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=539017b)  
[test/mjsunit/regress/regress-crbug-944435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-944435.js?cl=539017b)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=539017b)  
  

---   

## **regress-9022.js (v8 issue)**  
   
**[Issue: Wrong break target in asm.js](https://crbug.com/v8/9022)**  
**[Commit: [asm.js] Fix break depth calculation for named blocks.](https://chromium.googlesource.com/v8/v8/+/080fa87)**  
  
Date(Commit): Mon Mar 25 14:00:58 2019  
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

## **regress-945187.js (chromium issue)**  
   
**[Issue: Security: Debug check failed: !it.is_dictionary_holder() in src/compiler/property-access-builder.cc](https://crbug.com/945187)**  
**[Commit: [turbofan] Only lower constant load if feedback agrees with receiver map.](https://chromium.googlesource.com/v8/v8/+/149b822)**  
  
Date(Commit): Mon Mar 25 13:06:04 2019  
Components: Blink>JavaScript  
Labels: allpublic  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1538125](https://chromium-review.googlesource.com/c/v8/v8/+/1538125)  
Regress: [mjsunit/compiler/regress-945187.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-945187.js)  
```javascript
function f() {
  const o = {get: Object};
  Object.defineProperty(Object, 0, o);
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
delete Object.fromEntries;
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/149b822^!)  
[src/compiler/property-access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/property-access-builder.cc?cl=149b822)  
[test/mjsunit/compiler/regress-945187.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-945187.js?cl=149b822)  
  

---   

## **regress-944062-1.js (chromium issue)**  
   
**[Issue: Security: v8: turbofan: JSCallReducer::ReduceArrayIndexOfIncludes fails to insert Map checks](https://crbug.com/944062)**  
**[Commit: [turbofan] Add missing map checks in a reducer](https://chromium.googlesource.com/v8/v8/+/e80082b)**  
  
Date(Commit): Thu Mar 21 21:25:01 2019  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, allpublic, Target-74, M-74, Merge-Merged-74  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1532331](https://chromium-review.googlesource.com/c/v8/v8/+/1532331)  
Regress: [mjsunit/compiler/regress-944062-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-944062-1.js), [mjsunit/compiler/regress-944062-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-944062-2.js)  
```javascript
const array = [42, 2.1];  // non-stable map (PACKED_DOUBLE)
let b = false;

function f() {
  if (b) array[100000] = 4.2;  // go to dictionary mode
  return 42;
};
%NeverOptimizeFunction(f);

function includes() {
  return array.includes(f());
};
%PrepareFunctionForOptimization(includes);
assertTrue(includes());
assertTrue(includes());
%OptimizeFunctionOnNextCall(includes);
assertTrue(includes());
b = true;
assertTrue(includes());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e80082b^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=e80082b)  
[test/mjsunit/compiler/regress-944062-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-944062-1.js?cl=e80082b)  
[test/mjsunit/compiler/regress-944062-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-944062-2.js?cl=e80082b)  
  

---   

## **regress-9002.js (v8 issue)**  
   
**[Issue: Small functions shouldn't be inlined if they are not inlineable](https://crbug.com/v8/9002)**  
**[Commit: [turbofan] Fix wrongly inlined small functions](https://chromium.googlesource.com/v8/v8/+/07a711a)**  
  
Date(Commit): Wed Mar 20 08:46:41 2019  
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

## **regress-939316.js (chromium issue)**  
   
**[Issue: V8: Turbofan may read a Map pointer out-of-bounds when optimizing Reflect.construct](https://crbug.com/939316)**  
**[Commit: [turbofan] Do not call JSFunction::has_initial_map without has_prototype_slot](https://chromium.googlesource.com/v8/v8/+/d62cd2f)**  
  
Date(Commit): Mon Mar 18 13:00:06 2019  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified, CVE_description-missing, M-73, Target-72, Target-73, Merge-Merged-74, Release-0-M74, CVE-2019-5843  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1525942](https://chromium-review.googlesource.com/c/v8/v8/+/1525942)  
Regress: [mjsunit/compiler/regress-939316.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-939316.js)  
```javascript
(function JSCreate() {
  function f(arg) {
    const o = Reflect.construct(Object, arguments, Proxy);
    o.foo = arg;
  }

  function g(i) {
    f(i);
  };
  %PrepareFunctionForOptimization(g);
  g(0);
  g(1);
  %OptimizeFunctionOnNextCall(g);
  g(2);
})();


(function JSCreateArray() {
  function f() {
    try {
      const o = Reflect.construct(Array, arguments, parseInt);
    } catch (e) {
    }
  }

  function g() {
    f();
  };
  %PrepareFunctionForOptimization(g);
  g();
  g();
  %OptimizeFunctionOnNextCall(g);
  g();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d62cd2f^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=d62cd2f)  
[test/mjsunit/compiler/regress-939316.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-939316.js?cl=d62cd2f)  
  

---   

## **regress-crbug-941743.js (chromium issue)**  
   
**[Issue: Security: OOB write in v8::internal::(anonymous namespace)::ElementsAccessorBase](https://crbug.com/941743)**  
**[Commit: [TurboFan] Array.prototype.map wrong ElementsKind for output array.](https://chromium.googlesource.com/v8/v8/+/96de5ee)**  
  
Date(Commit): Mon Mar 18 12:30:42 2019  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified, M-73, Target-73, Merge-Merged-73, merge-merged-3729, Release-1-M73, CVE-2019-5825, CVE-description_missing  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1526018](https://chromium-review.googlesource.com/c/v8/v8/+/1526018)  
Regress: [mjsunit/regress/regress-crbug-941743.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-941743.js)  
```javascript
Array(2 ** 30);

let a = [1, 2, , , , 3];
function mapping(a) {
  return a.map(v => v);
};
%PrepareFunctionForOptimization(mapping);
mapping(a);
mapping(a);
%OptimizeFunctionOnNextCall(mapping);
mapping(a);

a.length = 32 * 1024 * 1024 - 1;
a.fill(1, 0);
a.push(2);
a.length += 500;
mapping(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96de5ee^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=96de5ee)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=96de5ee)  
[test/mjsunit/regress/regress-crbug-941743.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-941743.js?cl=96de5ee)  
  

---   

## **regress-crbug-942068.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/942068)**  
**[Commit: [turbofan] Fix HasProperty for OOB access on polymorphic ICs](https://chromium.googlesource.com/v8/v8/+/1e2aa78)**  
  
Date(Commit): Fri Mar 15 22:09:16 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-CC  
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
   
**[Issue: DCHECK failure in AllowHeapAllocation::IsAllowed() in heap-inl.h](https://crbug.com/940722)**  
**[Commit: [regexp] Allow heap allocation on stack overflows](https://chromium.googlesource.com/v8/v8/+/0793bb8)**  
  
Date(Commit): Tue Mar 12 15:01:59 2019  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
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

## **regress-940296.js (chromium issue)**  
   
**[Issue: Crash in unsigned long v8::base::AsAtomicImpl<long>::Relaxed_Load<unsigned long>](https://crbug.com/940296)**  
**[Commit: [wasm] Fix insufficient bounds check in WebAssembly.get](https://chromium.googlesource.com/v8/v8/+/e162eb4)**  
  
Date(Commit): Tue Mar 12 11:29:02 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Severity-Medium, Security_Impact-Beta, allpublic, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1514496](https://chromium-review.googlesource.com/c/v8/v8/+/1514496)  
Regress: [mjsunit/regress/wasm/regress-940296.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-940296.js)  
```javascript
let table = new WebAssembly.Table({element: "anyfunc", initial: 1});
assertThrows(() => table.get(3612882876), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e162eb4^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=e162eb4)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=e162eb4)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=e162eb4)  
[test/mjsunit/regress/wasm/regress-940296.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-940296.js?cl=e162eb4)  
[test/mjsunit/wasm/js-api.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/js-api.js?cl=e162eb4)  
  

---   

## **regress-crbug-937618.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/937618)**  
**[Commit: [proxy] fix has trap check for indices](https://chromium.googlesource.com/v8/v8/+/11d8358)**  
  
Date(Commit): Mon Mar 11 20:53:47 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-CC, merge-merged-7.4  
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
   
**[Issue: CHECK failure: !ResultAreIdentical(args); RegExpBuiltinsFuzzerHash=c087ca96 in regexp-builtins.](https://crbug.com/937681)**  
**[Commit: [regexp] Fix sticky callable replace with OOB lastIndex](https://chromium.googlesource.com/v8/v8/+/dd580e8)**  
  
Date(Commit): Mon Mar 11 16:09:47 2019  
Components: Blink>JavaScript>Regexp  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified  
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

## **regress-940361.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/940361)**  
**[Commit: [turbofan] Manually serialize descriptors for a field type dependency](https://chromium.googlesource.com/v8/v8/+/708c911)**  
  
Date(Commit): Mon Mar 11 12:45:00 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1514495](https://chromium-review.googlesource.com/c/v8/v8/+/1514495)  
Regress: [mjsunit/regress/regress-940361.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-940361.js)  
```javascript
const re = /abc/;

re.__proto__.__proto__.test = re.__proto__.test;
delete re.__proto__.test;

function foo(s) {
  return re.test(s);
};
%PrepareFunctionForOptimization(foo);
assertTrue(foo('abc'));
assertTrue(foo('abc'));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo('abc'));
assertFalse(foo('ab'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/708c911^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=708c911)  
[test/mjsunit/regress/regress-940361.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-940361.js?cl=708c911)  
  

---   

## **regress-935092.js (chromium issue)**  
   
**[Issue: Security: Debug check failed: op->opcode() == IrOpcode::kDeoptimize || op->opcode() == IrOpcode::kDeoptimizeIf || op->opcode() == IrOpcode::kDeoptimizeUnless](https://crbug.com/935092)**  
**[Commit: [turbofan] Check for dead control in branch elimination.](https://chromium.googlesource.com/v8/v8/+/ac8e98e)**  
  
Date(Commit): Mon Mar 11 06:30:00 2019  
Components: Blink>JavaScript  
Labels: M-74  
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
   
**[Issue: Float-cast-overflow in v8::internal::wasm::AsmJsParser::ValidateModuleVarFromGlobal](https://crbug.com/937650)**  
**[Commit: [asm.js] Fix undefined behavior with float32 constants.](https://chromium.googlesource.com/v8/v8/+/b60d567)**  
  
Date(Commit): Thu Mar 07 08:56:37 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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

## **regress-crbug-937734.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/937734)**  
**[Commit: [turbofan] Use load_mode feedback for HasProperty access](https://chromium.googlesource.com/v8/v8/+/1297c92)**  
  
Date(Commit): Wed Mar 06 19:27:31 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1501975](https://chromium-review.googlesource.com/c/v8/v8/+/1501975)  
Regress: [mjsunit/regress/regress-crbug-937734.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-937734.js)  
```javascript
function foo()
{
    return 1 in [0];
}

%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1297c92^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=1297c92)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=1297c92)  
[test/mjsunit/regress/regress-crbug-937734.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-937734.js?cl=1297c92)  
  

---   

## **regress-8947.js (v8 issue)**  
   
**[Issue: Calling wasm-re-exported-then-imported API function crashes](https://crbug.com/v8/8947)**  
**[Commit: [wasm] Fix import of reexported API function](https://chromium.googlesource.com/v8/v8/+/15925e5)**  
  
Date(Commit): Tue Mar 05 14:34:57 2019  
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

## **regress-crbug-937649.js (chromium issue)**  
   
**[Issue: Unknown signal in Builtins_JSEntryTrampoline](https://crbug.com/937649)**  
**[Commit: [turbofan] representation selection: do not convert from Boolean to Number without truncation](https://chromium.googlesource.com/v8/v8/+/676a020)**  
  
Date(Commit): Tue Mar 05 11:18:00 2019  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-74  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1501052](https://chromium-review.googlesource.com/c/v8/v8/+/1501052)  
Regress: [mjsunit/regress/regress-crbug-937649.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-937649.js)  
```javascript
(function() {
function foo(x) {
  const i = x > 0;
  const dv = new DataView(ab);
  return dv.getUint16(i);
};
%PrepareFunctionForOptimization(foo);
const ab = new ArrayBuffer(2);
foo(0);
foo(0);
%OptimizeFunctionOnNextCall(foo);
foo(0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/676a020^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=676a020)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=676a020)  
[test/mjsunit/regress/regress-crbug-937649.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-937649.js?cl=676a020)  
  

---   

## **regress-crbug-935932.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::JSNativeContextSpecialization::ReduceGlobalAccess](https://crbug.com/935932)**  
**[Commit: Reland "Optimize `in` operator"](https://chromium.googlesource.com/v8/v8/+/803ad32)**  
  
Date(Commit): Fri Mar 01 09:01:18 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
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

## **regress-crbug-934166.js (chromium issue)**  
   
**[Issue: Security: other->values_[index] != builder()->jsgraph()->OptimizedOutConstant() (0x563015eb2cf8 vs. 0x563015eb2cf8).](https://crbug.com/934166)**  
**[Commit: [ignition] Skip binding dead labels](https://chromium.googlesource.com/v8/v8/+/35269f7)**  
  
Date(Commit): Thu Feb 28 12:17:34 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: reward-0, Security_Severity-Low, Security_Impact-Head, allpublic, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/1488763](https://chromium-review.googlesource.com/c/1488763)  
Regress: [mjsunit/regress/regress-crbug-934166.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-934166.js)  
```javascript
{
  function f() {
    for(let i = 0; i < 10; ++i){
      %PrepareFunctionForOptimization(f);
      try{
        // Carefully constructed by a fuzzer to use a new register for s(), whose
        // write is dead due to the unconditional throw after s()=N, but which is
        // read in the ({...g}) call, which therefore must also be marked dead and
        // elided.
        with(f&&g&&(s()=N)({...g})){}
      } catch {}
      %OptimizeOsr();
    }
  }
  %EnsureFeedbackVectorForFunction(f);
  f();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/35269f7^!)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=35269f7)  
[src/interpreter/bytecode-array-builder.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.h?cl=35269f7)  
[src/interpreter/bytecode-array-writer.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-writer.cc?cl=35269f7)  
[src/interpreter/bytecode-array-writer.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-writer.h?cl=35269f7)  
[src/interpreter/bytecode-label.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-label.cc?cl=35269f7)  
...  
  

---   

## **regress-936077.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::AccessInfoFactory::ConsolidateElementLoad](https://crbug.com/936077)**  
**[Commit: [turbofan] Don't assume we have receiver maps in preprocessed feedback](https://chromium.googlesource.com/v8/v8/+/9c5cd06)**  
  
Date(Commit): Wed Feb 27 18:46:20 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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
   
**[Issue: Calls to runtime functions in wasm code should be serializable](https://crbug.com/v8/8896)**  
**[Commit: [wasm] Support runtime functions in (de)serializer.](https://chromium.googlesource.com/v8/v8/+/4c60e6b)**  
  
Date(Commit): Wed Feb 27 11:32:42 2019  
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

## **regress-crbug-936302.js (chromium issue)**  
   
**[Issue: CHECK failure: fixed_size_above_fp + in deoptimizer.cc](https://crbug.com/936302)**  
**[Commit: [turbofan] Always pass the right arity to calls.](https://chromium.googlesource.com/v8/v8/+/834c4b3)**  
  
Date(Commit): Wed Feb 27 08:40:58 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1491191](https://chromium-review.googlesource.com/c/1491191)  
Regress: [mjsunit/regress/regress-crbug-936302.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-936302.js)  
```javascript
(function() {
'use strict';

function baz() {
  'use asm';
  function f() {}
  return {f: f};
}

function foo(x) {
  baz(x);
  %DeoptimizeFunction(foo);
};
%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/834c4b3^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=834c4b3)  
[test/mjsunit/regress/regress-crbug-936302.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-936302.js?cl=834c4b3)  
  

---   

## **regress-8913.js (v8 issue)**  
   
**[Issue: String.prototype.concat deopt loop](https://crbug.com/v8/8913)**  
**[Commit: [turbofan] Properly thread through the feedback for HeapObject checks.](https://chromium.googlesource.com/v8/v8/+/066e2a2)**  
  
Date(Commit): Tue Feb 26 14:19:49 2019  
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

## **regress-935138.js (chromium issue)**  
   
**[Issue: Use-of-uninitialized-value in v8::internal::compiler::TurbofanWasmCompilationUnit::BuildGraphForWasmFunction](https://crbug.com/935138)**  
**[Commit: [wasm] Fix {StreamingDecoder} to reject multiple code sections.](https://chromium.googlesource.com/v8/v8/+/85b4ec5)**  
  
Date(Commit): Tue Feb 26 09:59:44 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-Stable, Security_Severity-Medium, Stability-Memory-MemorySanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1486471](https://chromium-review.googlesource.com/c/1486471)  
Regress: [mjsunit/regress/wasm/regress-935138.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-935138.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestAsyncCompileMultipleCodeSections() {
  let binary = new Binary();
  binary.emit_header();
  binary.emit_bytes([kTypeSectionCode, 4, 1, kWasmFunctionTypeForm, 0, 0]);
  binary.emit_bytes([kFunctionSectionCode, 2, 1, 0]);
  binary.emit_bytes([kCodeSectionCode, 6, 1, 4, 0, kExprLocalGet, 0, kExprEnd]);
  binary.emit_bytes([kCodeSectionCode, 6, 1, 4, 0, kExprLocalGet, 0, kExprEnd]);
  let buffer = binary.trunc_buffer();
  assertPromiseResult(WebAssembly.compile(buffer), assertUnreachable,
                      e => assertInstanceof(e, WebAssembly.CompileError));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85b4ec5^!)  
[src/wasm/streaming-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/streaming-decoder.cc?cl=85b4ec5)  
[src/wasm/streaming-decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/streaming-decoder.h?cl=85b4ec5)  
[test/mjsunit/regress/wasm/regress-935138.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-935138.js?cl=85b4ec5)  
[test/unittests/wasm/streaming-decoder-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/wasm/streaming-decoder-unittest.cc?cl=85b4ec5)  
  

---   

## **regress-v8-8799.js (v8 issue)**  
   
**[Issue: Bytecode flushing breaks template literals test](https://crbug.com/v8/8799)**  
**[Commit: [Runtime] Ensure template objects are retained if bytecode is flushed.](https://chromium.googlesource.com/v8/v8/+/ec9aef3)**  
  
Date(Commit): Mon Feb 25 11:20:06 2019  
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
   
**[Issue: Stack-overflow in v8::internal::PreParser::ParseFunctionLiteral](https://crbug.com/913222)**  
**[Commit: [parser] Fix stackoverflow on function expressions](https://chromium.googlesource.com/v8/v8/+/4b0c2b3)**  
  
Date(Commit): Mon Feb 25 10:44:26 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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
   
**[Issue: Null-dereference READ in v8::internal::DeclarationScope::HoistSloppyBlockFunctions](https://crbug.com/933214)**  
**[Commit: [parser] Always return a valid var from DeclareVariableName](https://chromium.googlesource.com/v8/v8/+/e14a24d)**  
  
Date(Commit): Mon Feb 25 10:31:26 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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

## **regress-934175.js (chromium issue)**  
   
**[Issue: Security: TypeError: node #38:JSToString type NumericOrString is not String](https://crbug.com/934175)**  
**[Commit: [turbofan] Re-type JSAdd("", prim) reduction to ToString.](https://chromium.googlesource.com/v8/v8/+/6660639)**  
  
Date(Commit): Fri Feb 22 09:24:53 2019  
Components: Blink>JavaScript  
Labels: Security_Impact-Head, Security_Severity-High, allpublic  
Code Review: [https://chromium-review.googlesource.com/c/1481632](https://chromium-review.googlesource.com/c/1481632)  
Regress: [mjsunit/compiler/regress-934175.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-934175.js)  
```javascript
(function ShortcutEmptyStringAddRight() {
  let ar = new Float32Array(1);
  function opt(i){
    return ar[i] + (NaN ? 0 : '');
  }
  %PrepareFunctionForOptimization(opt);
  ar[0] = 42;
  opt(1);
  %OptimizeFunctionOnNextCall(opt);
  assertEquals("42", opt(0));
})();

(function ShortcutiEmptyStringAddLeft() {
  let ar = new Float32Array(1);
  function opt(i){
    return (NaN ? 0 : '') + ar[i];
  }
  %PrepareFunctionForOptimization(opt);
  ar[0] = 42;
  opt(1);
  %OptimizeFunctionOnNextCall(opt);
  assertEquals("42", opt(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6660639^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=6660639)  
[test/mjsunit/compiler/regress-934175.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-934175.js?cl=6660639)  
  

---   

## **regress-crbug-934138.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,jitless](https://crbug.com/934138)**  
**[Commit: [asm.js] Fix handling of bogus code after export statement.](https://chromium.googlesource.com/v8/v8/+/cc787e1)**  
  
Date(Commit): Thu Feb 21 14:37:37 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
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
   
**[Issue: DCHECK failure in array_builder_.capacity() > array_builder_.length() in string-builder.cc](https://crbug.com/933776)**  
**[Commit: Remove invalid DCHECK in ReplacementStringBuilder](https://chromium.googlesource.com/v8/v8/+/c54bbd2)**  
  
Date(Commit): Thu Feb 21 09:41:06 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
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

## **regress-932392.js (chromium issue)**  
   
**[Issue: Security: RepresentationChangerError: node #50:NumberMax of kRepWord32 ((MinusZero | Range(0, 0))) cannot be changed to kRepTagged](https://crbug.com/932392)**  
**[Commit: [turbofan] Handle -0 truncation in word32->tagged rep change.](https://chromium.googlesource.com/v8/v8/+/64bad45)**  
  
Date(Commit): Wed Feb 20 12:48:25 2019  
Components: Blink>JavaScript  
Labels: Security_Impact-Stable, allpublic, M-73  
Code Review: [https://chromium-review.googlesource.com/c/1478192](https://chromium-review.googlesource.com/c/1478192)  
Regress: [mjsunit/compiler/regress-932392.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-932392.js)  
```javascript
function opt(flag){
  ((flag||(Math.max(-0,0)))==0)
}

%PrepareFunctionForOptimization(opt);
try{opt(false)}catch{}
%OptimizeFunctionOnNextCall(opt)
try{opt(false)}catch{}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64bad45^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=64bad45)  
[test/mjsunit/compiler/regress-932392.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-932392.js?cl=64bad45)  
  

---   

## **regress-crbug-932034.js (chromium issue)**  
   
**[Issue: Size calculation overflow can lead to heap buffer overflow](https://crbug.com/932034)**  
**[Commit: Reland "[builtins]: Optimize CreateTypedArray to use element size log 2 for calculations."](https://chromium.googlesource.com/v8/v8/+/02b9847)**  
  
Date(Commit): Wed Feb 20 12:06:53 2019  
Components: Blink>JavaScript  
Labels: reward-5000, Security_Impact-Head, Security_Severity-High, allpublic, reward-inprocess, Via-Wizard-Security, M-74, VulnerabilityAnalysis-Requested, VulnerabilityAnalysis-Submitted  
Code Review: [https://chromium-review.googlesource.com/c/1456299](https://chromium-review.googlesource.com/c/1456299)  
Regress: [mjsunit/regress/regress-crbug-932034.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-932034.js)  
```javascript
try {
  new BigInt64Array(%MaxSmi());
} catch(e) {
  assertInstanceof(e, RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/02b9847^!)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=02b9847)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=02b9847)  
[src/builtins/builtins-typed-array-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.h?cl=02b9847)  
[src/builtins/typed-array-createtypedarray.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/typed-array-createtypedarray.tq?cl=02b9847)  
[src/builtins/typed-array.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/typed-array.tq?cl=02b9847)  
...  
  

---   

## **regress-933179.js (chromium issue)**  
   
**[Issue: DCHECK failure in old_map_->is_stable() in map-updater.cc](https://crbug.com/933179)**  
**[Commit: Remove incorrect dcheck from map updater.](https://chromium.googlesource.com/v8/v8/+/f23712f)**  
  
Date(Commit): Tue Feb 19 19:04:55 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-74, M-74  
Code Review: [https://chromium-review.googlesource.com/c/1477275](https://chromium-review.googlesource.com/c/1477275)  
Regress: [mjsunit/regress/regress-933179.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-933179.js)  
```javascript
var o = { ...{ length : 1 } };

o.x = 1;
delete o.x;

o.length = 2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f23712f^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=f23712f)  
[test/mjsunit/regress/regress-933179.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-933179.js?cl=f23712f)  
  

---   

## **regress-8846.js (v8 issue)**  
   
**[Issue: [wasm] Unordered module sections are not properly accepted during async compilation](https://crbug.com/v8/8846)**  
**[Commit: [wasm] Fix section order checking in {StreamingDecoder}.](https://chromium.googlesource.com/v8/v8/+/d7a5e5b)**  
  
Date(Commit): Tue Feb 19 16:57:23 2019  
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

## **regress-932953.js (chromium issue)**  
   
**[Issue: CHECK failure: transitions.SearchSpecial(roots.nonextensible_symbol()) == *old_map_ in map-upda](https://crbug.com/932953)**  
**[Commit: Fix accessor update of non-extensible maps.](https://chromium.googlesource.com/v8/v8/+/1a3a2bc)**  
  
Date(Commit): Tue Feb 19 04:59:36 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-74, M-74  
Code Review: [https://chromium-review.googlesource.com/c/1477067](https://chromium-review.googlesource.com/c/1477067)  
Regress: [mjsunit/regress/regress-932953.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-932953.js)  
```javascript
(function NonExtensibleBetweenSetterAndGetter() {
  o = {};
  o.x = 42;
  o.__defineGetter__('y', function() {});
  Object.preventExtensions(o);
  o.__defineSetter__('y', function() {});
  o.x = 0.1;
})();

(function InterleavedIntegrityLevel() {
  o = {};
  o.x = 42;
  o.__defineSetter__('y', function() {});
  Object.preventExtensions(o);
  o.__defineGetter__('y', function() {
    return 44;
  });
  Object.seal(o);
  o.x = 0.1;
  assertEquals(44, o.y);
})();

(function TryUpdateRepeatedIntegrityLevel() {
  function C() {
    this.x = 0;
    this.x = 1;
    Object.preventExtensions(this);
    Object.seal(this);
  }

  const o1 = new C();
  const o2 = new C();
  const o3 = new C();

  function f(o) {
    return o.x;
  }

  // Warm up the IC.
  ;
  %PrepareFunctionForOptimization(f);
  f(o1);
  f(o1);
  f(o1);

  // Reconfigure to double field.
  o3.x = 0.1;

  // Migrate o2 to the new shape.
  f(o2);

  %OptimizeFunctionOnNextCall(f);
  f(o1);

  assertTrue(%HaveSameMap(o1, o2));
  assertTrue(%HaveSameMap(o1, o3));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a3a2bc^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=1a3a2bc)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=1a3a2bc)  
[src/transitions.cc](https://cs.chromium.org/chromium/src/v8/src/transitions.cc?cl=1a3a2bc)  
[src/transitions.h](https://cs.chromium.org/chromium/src/v8/src/transitions.h?cl=1a3a2bc)  
[test/mjsunit/regress/regress-932953.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-932953.js?cl=1a3a2bc)  
  

---   

## **regress-crbug-931664.js (chromium issue)**  
   
**[Issue: Security: v8: unreachable code in OddballToNumber](https://crbug.com/931664)**  
**[Commit: [turbofan] Handle all oddballs in OddballToNumber](https://chromium.googlesource.com/v8/v8/+/68ed2f1)**  
  
Date(Commit): Mon Feb 18 10:46:37 2019  
Components: Blink>JavaScript  
Labels: None  
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

## **regress-932101.js (chromium issue)**  
   
**[Issue: Security: Debug check failed: to_kind == DICTIONARY_ELEMENTS || IsFixedTypedArrayElementsKind(to_kind)](https://crbug.com/932101)**  
**[Commit: Relax a too-strict DCHECKs.](https://chromium.googlesource.com/v8/v8/+/0f6f064)**  
  
Date(Commit): Fri Feb 15 07:44:11 2019  
Components: Blink>JavaScript  
Labels: allpublic  
Code Review: [https://chromium-review.googlesource.com/c/1475390](https://chromium-review.googlesource.com/c/1475390)  
Regress: [mjsunit/regress-932101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-932101.js)  
```javascript
o = Object("A");
o.x = 1;
Object.seal(o);
o.x = 0.1

o[1] = "b";
assertEquals(undefined, o[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f6f064^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=0f6f064)  
[test/mjsunit/regress-932101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-932101.js?cl=0f6f064)  
  

---   

## **regress-crbug-926651.js (chromium issue)**  
   
**[Issue: Security: [v8] Type Confusion in Builtins_CallUndefinedReceiver1Handler](https://crbug.com/926651)**  
**[Commit: [ast] Always visit all AST nodes, even dead nodes](https://chromium.googlesource.com/v8/v8/+/9439a1d)**  
  
Date(Commit): Wed Feb 13 15:24:28 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, reward-5000, ReleaseBlock-NA, Reward-1000, Merge-Merged, Security_Impact-Stable, Security_Severity-High, allpublic, reward-inprocess, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, CVE_description-submitted, M-73, Target-72, Target-73, Release-0-M73, CVE-2019-5791  
Code Review: [https://chromium-review.googlesource.com/c/1470108](https://chromium-review.googlesource.com/c/1470108)  
Regress: [mjsunit/regress/regress-crbug-926651.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-926651.js)  
```javascript
var asdf = false;

const f =
  (v1 = (function g() {
    if (asdf) { return; } else { return; }
    (function h() {});
  })()) => 1;
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9439a1d^!)  
[src/ast/ast-traversal-visitor.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-traversal-visitor.h?cl=9439a1d)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=9439a1d)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=9439a1d)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=9439a1d)  
[test/mjsunit/regress/regress-crbug-926651.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-926651.js?cl=9439a1d)  
  

---   

## **regress-crbug-930948-base.js (chromium issue)**  
   
**[Issue: CHECK failure: (value & uint64_t{ADDRESS}) != unexpected || (value & uint64_t{ADDRESS}) == uint](https://crbug.com/930948)**  
**[Commit: [array] Fix Array#map storing signaling NaNs](https://chromium.googlesource.com/v8/v8/+/82faa6d)**  
  
Date(Commit): Wed Feb 13 10:23:19 2019  
Components: Blink>JavaScript>Runtime, Blink>JavaScript>Compiler  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-74, M-74  
Code Review: [https://chromium-review.googlesource.com/c/1466503](https://chromium-review.googlesource.com/c/1466503)  
Regress: [mjsunit/regress/regress-crbug-930948-base.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-930948-base.js), [mjsunit/regress/regress-crbug-930948.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-930948.js)  
```javascript
function foo() {
  return [undefined].map(Math.asin);
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82faa6d^!)  
[src/builtins/array-map.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-map.tq?cl=82faa6d)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=82faa6d)  
[src/compiler/graph-assembler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-assembler.h?cl=82faa6d)  
[test/mjsunit/regress/regress-crbug-930948-base.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-930948-base.js?cl=82faa6d)  
[test/mjsunit/regress/regress-crbug-930948.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-930948.js?cl=82faa6d)  
  

---   

## **regress-8808.js (v8 issue)**  
   
**[Issue: Segmentation fault when trying to declare a class with destructuring private class field](https://crbug.com/v8/8808)**  
**[Commit: [parser] don't accept PRIVATE_NAME for object literal property names](https://chromium.googlesource.com/v8/v8/+/1483561)**  
  
Date(Commit): Mon Feb 11 18:17:32 2019  
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
   
**[Issue: Ill in v8::internal::Map::EquivalentToForTransition](https://crbug.com/930486)**  
**[Commit: Fix map equivalence check.](https://chromium.googlesource.com/v8/v8/+/a953f8d)**  
  
Date(Commit): Mon Feb 11 16:31:35 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
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

## **regress-crbug-930580.js (chromium issue)**  
   
**[Issue: DCHECK failure in !var->has_forced_context_allocation() || var->is_used() in scopes.cc](https://crbug.com/930580)**  
**[Commit: [parser] Reset expression_scope_ stack to nullptr when parsing a function body](https://chromium.googlesource.com/v8/v8/+/486ec80)**  
  
Date(Commit): Mon Feb 11 09:22:57 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1462005](https://chromium-review.googlesource.com/c/1462005)  
Regress: [mjsunit/regress/regress-crbug-930580.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-930580.js)  
```javascript
(function outer() {
  (arg = (function inner() {
    return this
  })()) => 0;
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/486ec80^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=486ec80)  
[test/mjsunit/regress/regress-crbug-930580.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-930580.js?cl=486ec80)  
  

---   

## **regress-930045.js (chromium issue)**  
   
**[Issue: CHECK failure: transitions.SearchSpecial(roots.nonextensible_symbol()) == *old_map_ in map-upda](https://crbug.com/930045)**  
**[Commit: Fix map updater for non-extensible maps with private symbols.](https://chromium.googlesource.com/v8/v8/+/154bb50)**  
  
Date(Commit): Sat Feb 09 09:09:02 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1460949](https://chromium-review.googlesource.com/c/1460949)  
Regress: [mjsunit/regress-930045.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-930045.js)  
```javascript
(function CaptureStackTracePrivateSymbol() {
  var o = {};
  Object.preventExtensions(o);

  try { Error.captureStackTrace(o); } catch (e) {}
  try { Error.captureStackTrace(o); } catch (e) {}
})();

(function PrivateFieldAfterPreventExtensions() {
  class C {
    constructor() {
      this.x = 1;
      Object.preventExtensions(this);
    }
  }

  class D extends C {
    #i = 42;

    set(i) { this.#i = i; }
    get(i) { return this.#i; }
  }

  let d = new D();
  d.x = 0.1;
  assertEquals(42, d.get());
  d.set(43);
  assertEquals(43, d.get());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/154bb50^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=154bb50)  
[src/map-updater.h](https://cs.chromium.org/chromium/src/v8/src/map-updater.h?cl=154bb50)  
[src/objects/map.cc](https://cs.chromium.org/chromium/src/v8/src/objects/map.cc?cl=154bb50)  
[test/mjsunit/regress-930045.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-930045.js?cl=154bb50)  
  

---   

## **regress-912162.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSObject::JSObjectVerify](https://crbug.com/912162)**  
**[Commit: Constant field tracking for arrays.](https://chromium.googlesource.com/v8/v8/+/ea86509)**  
  
Date(Commit): Wed Feb 06 14:44:43 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
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

## **regress-crbug-926856.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/926856)**  
**[Commit: [Builtins]: Array.prototype.map out of memory error](https://chromium.googlesource.com/v8/v8/+/183b857)**  
  
Date(Commit): Fri Feb 01 12:33:19 2019  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/1449531](https://chromium-review.googlesource.com/c/1449531)  
Regress: [mjsunit/regress/regress-crbug-926856.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-926856.js)  
```javascript
var size = 63392;
var a = [];
function build() {
  for (let i = 0; i < size; i++) {
    a.push(i);
  }
}

build();

function c(v) { return v + 0.5; }
a.map(c);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/183b857^!)  
[src/builtins/array-map.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-map.tq?cl=183b857)  
[test/mjsunit/regress/regress-crbug-926856.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-926856.js?cl=183b857)  
  

---   

## **regress-v8-5848.js (v8 issue)**  
   
**[Issue: Exponentiation operator ** has different results for numbers and variables from 50 upwards](https://crbug.com/v8/5848)**  
**[Commit: [builtins] [turbofan] Refactor Float64Pow to use single implementation](https://chromium.googlesource.com/v8/v8/+/595aafe)**  
  
Date(Commit): Thu Jan 31 09:42:25 2019  
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
   
**[Issue: Null-dereference READ in v8::internal::DeclarationScope::HoistSloppyBlockFunctions](https://crbug.com/926819)**  
**[Commit: [parser] Don't hoist sloppy block functions on error](https://chromium.googlesource.com/v8/v8/+/3ef9af8)**  
  
Date(Commit): Wed Jan 30 11:54:28 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/1445891](https://chromium-review.googlesource.com/c/1445891)  
Regress: [mjsunit/regress/regress-crbug-926819.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-926819.js)  
```javascript
assertThrows("a(function(){{let f;function f}})", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ef9af8^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=3ef9af8)  
[test/mjsunit/regress/regress-crbug-926819.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-926819.js?cl=3ef9af8)  
  

---   

## **regress-924843.js (chromium issue)**  
   
**[Issue: DCHECK failure in IsAligned(DistanceTo(target), kInstrSize) in instructions-arm64.cc](https://crbug.com/924843)**  
**[Commit: [Liftoff] Correctly unuse Labels](https://chromium.googlesource.com/v8/v8/+/3af3c9d)**  
  
Date(Commit): Tue Jan 29 15:18:48 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-71, Target-71  
Code Review: [https://chromium-review.googlesource.com/c/1442645](https://chromium-review.googlesource.com/c/1442645)  
Regress: [mjsunit/regress/wasm/regress-924843.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-924843.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, sig)
  .addBody([
    kExprLocalGet, 2,
    kExprIf, kWasmStmt,
      kExprBlock, kWasmStmt
  ]);
builder.addExport('main', 0);
assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3af3c9d^!)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=3af3c9d)  
[test/mjsunit/regress/wasm/regress-924843.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-924843.js?cl=3af3c9d)  
  

---   

## **regress-925671.js (chromium issue)**  
   
**[Issue: DCHECK failure in 0 < outstanding_tiering_units_ in module-compiler.cc](https://crbug.com/925671)**  
**[Commit: [wasm] Distinguish requested tier and executed tier](https://chromium.googlesource.com/v8/v8/+/185922d)**  
  
Date(Commit): Tue Jan 29 12:36:48 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73  
Code Review: [https://chromium-review.googlesource.com/c/1442639](https://chromium-review.googlesource.com/c/1442639)  
Regress: [mjsunit/regress/wasm/regress-925671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-925671.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('f0', kSig_v_v).addBody([]);
builder.addFunction('f1', kSig_v_v).addBody([]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/185922d^!)  
[src/wasm/function-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-compiler.cc?cl=185922d)  
[src/wasm/function-compiler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-compiler.h?cl=185922d)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=185922d)  
[src/wasm/wasm-tier.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-tier.h?cl=185922d)  
[test/mjsunit/regress/wasm/regress-925671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-925671.js?cl=185922d)  
  

---   

## **regress-926036.js (chromium issue)**  
   
**[Issue: DCHECK failure in (decl.pattern) != nullptr in parser.cc](https://crbug.com/926036)**  
**[Commit: [parser] Make pattern DCHECK dependent on !has_error](https://chromium.googlesource.com/v8/v8/+/b0e1c2b)**  
  
Date(Commit): Tue Jan 29 11:03:09 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1442635](https://chromium-review.googlesource.com/c/1442635)  
Regress: [mjsunit/regress/regress-926036.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-926036.js)  
```javascript
assertThrows("async() => { for await (var a ;;) {} }", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0e1c2b^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=b0e1c2b)  
[test/mjsunit/regress/regress-926036.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-926036.js?cl=b0e1c2b)  
  

---   

## **regress-924905.js (chromium issue)**  
   
**[Issue: DCHECK failure in lsb == base::bits::CountTrailingZeros32(value) in instruction-selector-arm.cc](https://crbug.com/924905)**  
**[Commit: [wasm][arm] Fix {Word32Shr} instruction selection.](https://chromium.googlesource.com/v8/v8/+/8a3c4d9)**  
  
Date(Commit): Fri Jan 25 13:08:10 2019  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Merge-Merged, Security_Impact-Stable, Stability-Libfuzzer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, M-73, Target-73, Release-0-M73  
Code Review: [https://chromium-review.googlesource.com/c/1435939](https://chromium-review.googlesource.com/c/1435939)  
Regress: [mjsunit/regress/wasm/regress-924905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-924905.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder.addFunction("kaboom", kSig_i_v)
    .addBody([
      kExprI32Const, 0,
      kExprI32Const, 0,
      kExprI32And,
      kExprI32Const, 0,
      kExprI32ShrU,
    ]).exportFunc();
let instance = builder.instantiate();
assertEquals(0, instance.exports.kaboom());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a3c4d9^!)  
[src/compiler/backend/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/backend/arm/instruction-selector-arm.cc?cl=8a3c4d9)  
[test/mjsunit/regress/wasm/regress-924905.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-924905.js?cl=8a3c4d9)  
  

---   

## **regress-924151.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/924151)**  
**[Commit: [turbofan] Handle exceptional edges when inserting unreachable node.](https://chromium.googlesource.com/v8/v8/+/ec4d45a)**  
  
Date(Commit): Thu Jan 24 12:43:46 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
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
   
**[Issue: Applying delete operator on non-initialized |this| should throw ReferenceError](https://crbug.com/v8/6711)**  
**[Commit: Check for "SuperNotCalled" on "delete this" in a constructor](https://chromium.googlesource.com/v8/v8/+/1e5b235)**  
  
Date(Commit): Tue Jan 22 18:58:42 2019  
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
   
**[Issue: Ill in v8::internal::BaseConsumedPreparseData<v8::internal::PreparseData>::GetDataForSk](https://crbug.com/923705)**  
**[Commit: [parser] Fix storing has_data bit for inner function preparse data](https://chromium.googlesource.com/v8/v8/+/c3722aa)**  
  
Date(Commit): Mon Jan 21 18:04:34 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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

## **regress-922933.js (chromium issue)**  
   
**[Issue: DCHECK failure in *available != 0 in assembler-arm.cc](https://crbug.com/922933)**  
**[Commit: [Liftoff][arm] Avoid use of temp registers](https://chromium.googlesource.com/v8/v8/+/ce2bfb8)**  
  
Date(Commit): Mon Jan 21 13:09:13 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Head, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1424862](https://chromium-review.googlesource.com/c/1424862)  
Regress: [mjsunit/regress/wasm/regress-922933.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-922933.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI64], [kWasmI64]));
builder.addFunction(undefined, sig)
  .addLocals({i32_count: 14}).addLocals({i64_count: 17}).addLocals({f32_count: 14})
  .addBody([
    kExprBlock, kWasmStmt,
      kExprBr, 0x00,
      kExprEnd,
    kExprBlock, kWasmStmt,
      kExprI32Const, 0x00,
      kExprLocalSet, 0x09,
      kExprI32Const, 0x00,
      kExprIf, kWasmStmt,
        kExprBlock, kWasmStmt,
          kExprI32Const, 0x00,
          kExprLocalSet, 0x0a,
          kExprBr, 0x00,
          kExprEnd,
        kExprBlock, kWasmStmt,
          kExprBlock, kWasmStmt,
            kExprLocalGet, 0x00,
            kExprLocalSet, 0x12,
            kExprBr, 0x00,
            kExprEnd,
          kExprLocalGet, 0x16,
          kExprLocalSet, 0x0f,
          kExprLocalGet, 0x0f,
          kExprLocalSet, 0x17,
          kExprLocalGet, 0x0f,
          kExprLocalSet, 0x18,
          kExprLocalGet, 0x17,
          kExprLocalGet, 0x18,
          kExprI64ShrS,
          kExprLocalSet, 0x19,
          kExprUnreachable,
          kExprEnd,
        kExprUnreachable,
      kExprElse,
        kExprUnreachable,
        kExprEnd,
      kExprUnreachable,
      kExprEnd,
    kExprUnreachable
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce2bfb8^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=ce2bfb8)  
[test/mjsunit/regress/wasm/regress-922933.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-922933.js?cl=ce2bfb8)  
  

---   

## **regress-922670.js (chromium issue)**  
   
**[Issue: Fatal error in liftoff-register.h](https://crbug.com/922670)**  
**[Commit: [Liftoff] Fix DCHECK error](https://chromium.googlesource.com/v8/v8/+/f77299e)**  
  
Date(Commit): Mon Jan 21 11:52:17 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Clusterfuzz, Unreproducible  
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
      kExprLocalGet, 1,
      kExprI64Const, 1,
      kExprLoop, kWasmI32,
        kExprLocalGet, 0,
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

## **regress-crbug-923264.js (chromium issue)**  
   
**[Issue: CHECK failure: object->IsAbstractCode() || object->IsSeqString() || object->IsExternalString() ](https://crbug.com/923264)**  
**[Commit: [heap] Allow PreparseData in large object space](https://chromium.googlesource.com/v8/v8/+/c45a2ef)**  
  
Date(Commit): Mon Jan 21 11:18:02 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73  
Code Review: [https://chromium-review.googlesource.com/c/1421321](https://chromium-review.googlesource.com/c/1421321)  
Regress: [mjsunit/regress/regress-crbug-923264.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-923264.js)  
```javascript
let paramName = '';
for (let i=0; i < 2**10; i++) {
  paramName += 'a';
}

let params = '';
for (let i = 0; i < 2**10; i++) {
  params += paramName + i + ',';
}

let fn = eval(`(
  class A {
    constructor (${params}) {
      function lazy() {
        return function lazier() { return ${paramName+1} }
      };
      return lazy;
    }
})`);

gc()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c45a2ef^!)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=c45a2ef)  
[test/mjsunit/regress/regress-crbug-923264.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-923264.js?cl=c45a2ef)  
  

---   

## **regress-8708.js (v8 issue)**  
   
**[Issue: Uncaught stack overflow in Array.prototype.flat](https://crbug.com/v8/8708)**  
**[Commit: [array] Add stack overflow check for Array#flat](https://chromium.googlesource.com/v8/v8/+/bf17cd2)**  
  
Date(Commit): Mon Jan 21 10:39:45 2019  
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
   
**[Issue: Null-dereference READ in v8::internal::ParserBase<v8::internal::Parser>::ParseArrowFunctionLiteral](https://crbug.com/923723)**  
**[Commit: [parser] Reparsing arrow function head upon failure can overflow the stack](https://chromium.googlesource.com/v8/v8/+/b4e7d11)**  
  
Date(Commit): Mon Jan 21 10:12:10 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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
   
**[Issue: Ill in v8::internal::RemoveArrayHolesGeneric](https://crbug.com/923265)**  
**[Commit: [array] Remove CHECK_LE from RemoveArrayHolesGeneric](https://chromium.googlesource.com/v8/v8/+/e38faab)**  
  
Date(Commit): Fri Jan 18 10:01:37 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
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

## **regress-922432.js (chromium issue)**  
   
**[Issue: Heap-buffer-overflow in unsigned int v8::internal::wasm::Decoder::read_leb_tail<unsigned int,](https://crbug.com/922432)**  
**[Commit: [wasm] Fix {OpcodeLength} for invalid br-on-exn opcodes.](https://chromium.googlesource.com/v8/v8/+/30882a5)**  
  
Date(Commit): Wed Jan 16 14:50:13 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, Stability-Libfuzzer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1414917](https://chromium-review.googlesource.com/c/1414917)  
Regress: [mjsunit/regress/wasm/regress-922432.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-922432.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestTruncatedBrOnExnInLoop() {
  let builder = new WasmModuleBuilder();
  let fun = builder.addFunction(undefined, kSig_v_v)
      .addLocals({except_count: 1})
      .addBody([
        kExprLoop, kWasmStmt,
          kExprLocalGet, 0,
          kExprBrOnExn  // Bytecode truncated here.
      ]).exportFunc();
  fun.body.pop();  // Pop implicitly added kExprEnd from body.
  assertThrows(() => builder.instantiate(), WebAssembly.CompileError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/30882a5^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=30882a5)  
[test/mjsunit/regress/wasm/regress-922432.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-922432.js?cl=30882a5)  
  

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

## **regress-921382.js (chromium issue)**  
   
**[Issue: Security: Debug check failed: nary->op() == Token::COMMA in V8 parsing](https://crbug.com/921382)**  
**[Commit: [parser] Clear parenthesized flag on collapsing nary expressions](https://chromium.googlesource.com/v8/v8/+/5f8a3e1)**  
  
Date(Commit): Tue Jan 15 13:26:23 2019  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: reward-0, Security_Severity-Medium, Security_Impact-Head, allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Components, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1412172](https://chromium-review.googlesource.com/c/1412172)  
Regress: [mjsunit/regress/regress-921382.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-921382.js)  
```javascript
assertThrows("(d * f * g) * e => 0", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f8a3e1^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=5f8a3e1)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=5f8a3e1)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=5f8a3e1)  
[test/mjsunit/regress/regress-921382.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-921382.js?cl=5f8a3e1)  
  

---   

## **regress-917755.js (chromium issue)**  
   
**[Issue: Security: DCHECK failure in V8 DeclarationScope Analyze phase](https://crbug.com/917755)**  
**[Commit: [parser] Give hoisting sloppy block functions a valid position](https://chromium.googlesource.com/v8/v8/+/8436715)**  
  
Date(Commit): Tue Jan 15 11:52:28 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: reward-0, allpublic, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/1408991](https://chromium-review.googlesource.com/c/1408991)  
Regress: [mjsunit/regress/regress-917755.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-917755.js)  
```javascript
assertThrows(`
{
    function a() {}
}

{
    // Duplicate lexical declarations are only allowed if they are both sloppy
    // block functions (see bug 4693). In this case the sloppy block function
    // conflicts with the lexical variable declaration, causing a syntax error.
    let a;
    function a() {};
}
`, SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8436715^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=8436715)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=8436715)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=8436715)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=8436715)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=8436715)  
...  
  

---   

## **regress-917215.js (chromium issue)**  
   
**[Issue: False SyntaxError on Sibling JS-Labels](https://crbug.com/917215)**  
**[Commit: [parser] Allow same-named labelled blocks in if/else statements](https://chromium.googlesource.com/v8/v8/+/469754d)**  
  
Date(Commit): Fri Jan 11 17:40:18 2019  
Components: Blink>JavaScript>Language  
Labels: Arch-x86_64, Via-Wizard-Javascript, Triaged-ET, M-73, Target-73, FoundIn-71, FoundIn-72, FoundIn-73, Needs-Triage-M71  
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
   
**[Issue: Dcheck on jsfunfuzz](https://crbug.com/v8/8630)**  
**[Commit: [parser] Check assignment LHS for paren errors](https://chromium.googlesource.com/v8/v8/+/df6f5f6)**  
  
Date(Commit): Fri Jan 11 12:56:38 2019  
Code Review: [https://chromium-review.googlesource.com/c/1404452](https://chromium-review.googlesource.com/c/1404452)  
Regress: [mjsunit/regress/regress-8630.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8630.js)  
```javascript
assertThrows("( ({x: 1}) ) => {};", SyntaxError);
assertThrows("( (x) ) => {}", SyntaxError);
assertThrows("( ({x: 1}) = y ) => {}", SyntaxError);
assertThrows("( (x) = y ) => {}", SyntaxError);

assertThrows("let [({x: 1})] = [];", SyntaxError);
assertThrows("let [(x)] = [];", SyntaxError);
assertThrows("let [({x: 1}) = y] = [];", SyntaxError);
assertThrows("let [(x) = y] = [];", SyntaxError);
assertThrows("var [({x: 1})] = [];", SyntaxError);
assertThrows("var [(x)] = [];", SyntaxError);
assertThrows("var [({x: 1}) = y] = [];", SyntaxError);
assertThrows("var [(x) = y] = [];", SyntaxError);

assertThrows("[({x: 1}) = y] = [];", SyntaxError);
assertThrows("({a,b}) = {a:2,b:3}", SyntaxError);

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
   
**[Issue: Ill in v8::internal::wasm::fuzzer::WasmExecutionFuzzer::FuzzWasmModule](https://crbug.com/919308)**  
**[Commit: [Liftoff] Fix sub of the same register](https://chromium.googlesource.com/v8/v8/+/8518d12)**  
  
Date(Commit): Fri Jan 11 10:57:09 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Stability-AFL, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1405029](https://chromium-review.googlesource.com/c/1405029)  
Regress: [mjsunit/regress/wasm/regress-919308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919308.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_i)
  .addLocals({i32_count: 5})
  .addBody([
    kExprLocalGet, 0,    // --> 1
    kExprIf, kWasmI32,
      kExprLocalGet, 0,  // --> 1
    kExprElse,
      kExprUnreachable,
      kExprEnd,
    kExprIf, kWasmI32,
      kExprLocalGet, 0,  // --> 1
    kExprElse,
      kExprUnreachable,
      kExprEnd,
    kExprIf, kWasmI32,
      kExprI32Const, 0,
      kExprLocalGet, 0,
      kExprI32Sub,       // --> -1
      kExprLocalGet, 0,
      kExprLocalGet, 0,
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

## **regress-918284.js (chromium issue)**  
   
**[Issue: DCHECK failure in *available != 0 in assembler-arm.cc](https://crbug.com/918284)**  
**[Commit: [Liftoff][arm] Leave scratch register to the assembler](https://chromium.googlesource.com/v8/v8/+/f59d6d9)**  
  
Date(Commit): Fri Jan 11 08:27:16 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1405036](https://chromium-review.googlesource.com/c/1405036)  
Regress: [mjsunit/regress/wasm/regress-918284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918284.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_i)
  .addLocals({i32_count: 7})
  .addBody([
    kExprI32Const, 0,
    kExprIf, kWasmI32,   // @11 i32
      kExprI32Const, 0,
    kExprElse,   // @15
      kExprI32Const, 1,
      kExprEnd,   // @18
    kExprLocalTee, 0,
    kExprI32Popcnt
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f59d6d9^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=f59d6d9)  
[test/mjsunit/regress/wasm/regress-918284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-918284.js?cl=f59d6d9)  
  

---   

## **regress-919754.js (chromium issue)**  
   
**[Issue: DCHECK failure in !std::isnan(value) in js-operator.h](https://crbug.com/919754)**  
**[Commit: [turbofan] Fix invocation frequency computation with NaN.](https://chromium.googlesource.com/v8/v8/+/ef12b47)**  
  
Date(Commit): Thu Jan 10 19:04:05 2019  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1404445](https://chromium-review.googlesource.com/c/1404445)  
Regress: [mjsunit/compiler/regress-919754.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-919754.js)  
```javascript
function f(get, ...a) {
  for (let i = 0; i < 1000; i++) {
    if (i === 999) %OptimizeOsr();
    a.map(f);
  }
  return get();
}
%PrepareFunctionForOptimization(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef12b47^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=ef12b47)  
[test/mjsunit/compiler/regress-919754.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-919754.js?cl=ef12b47)  
  

---   

## **regress-crbug-920184.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSObject::JSObjectVerify](https://crbug.com/920184)**  
**[Commit: [Builtins] Array.prototype.filter species creation error](https://chromium.googlesource.com/v8/v8/+/72d8307)**  
  
Date(Commit): Thu Jan 10 18:09:36 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
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
        z0 = (%PrepareFunctionForOptimization(y)), // prepare function for optimization
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

## **regress-919533.js (chromium issue)**  
   
**[Issue: DCHECK failure in !load_dst_regs_.has(dst) in liftoff-assembler.cc](https://crbug.com/919533)**  
**[Commit: [Liftoff] Fix reloading register spilled multiple times](https://chromium.googlesource.com/v8/v8/+/24a43b3)**  
  
Date(Commit): Wed Jan 09 16:12:50 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1403124](https://chromium-review.googlesource.com/c/1403124)  
Regress: [mjsunit/regress/wasm/regress-919533.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919533.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_v).addBody([]);
builder.addFunction(undefined, kSig_i_i)
  .addBody([
    kExprLocalGet, 0,
    kExprLocalGet, 0,
    // Stack now contains two copies of the first param register.
    // Start a loop to create a merge point (values still in registers).
    kExprLoop, kWasmStmt,
      // The call spills all values.
      kExprCallFunction, 0,
      // Break to the loop. Now the spilled values need to be loaded back *into
      // the same register*.
      kExprBr, 0,
      kExprEnd,
    kExprDrop
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/24a43b3^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=24a43b3)  
[test/mjsunit/regress/wasm/regress-919533.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-919533.js?cl=24a43b3)  
  

---   

## **regress-913844.js (chromium issue)**  
   
**[Issue: DCHECK failure in block->predecessors().empty() || block->successors().empty() in unwinding-info-w](https://crbug.com/913844)**  
**[Commit: Remove invalid DCHECKS in unwinding-info-writer](https://chromium.googlesource.com/v8/v8/+/49a526a)**  
  
Date(Commit): Wed Jan 09 15:52:08 2019  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
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

## **regress-920076.js (chromium issue)**  
   
**[Issue: DCHECK failure in !failed_ in asm-parser.cc](https://crbug.com/920076)**  
**[Commit: [asm.js] Fix semicolon insertion in presence of Unicode.](https://chromium.googlesource.com/v8/v8/+/082bfec)**  
  
Date(Commit): Wed Jan 09 12:38:41 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1402778](https://chromium-review.googlesource.com/c/1402778)  
Regress: [mjsunit/asm/regress-920076.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-920076.js)  
```javascript
function Module() {
  "use asm";
  function f() {}
  return f
}
eval("(" + Module.toString().replace(/;/, String.fromCharCode(8233)) + ")();");
assertFalse(%IsAsmWasmCode(Module));  // Valid asm.js, but we reject Unicode.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/082bfec^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=082bfec)  
[test/mjsunit/asm/regress-920076.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-920076.js?cl=082bfec)  
  

---   

## **regress-8659.js (v8 issue)**  
   
**[Issue: Dcheck on jsfunfuzz](https://crbug.com/v8/8659)**  
**[Commit: [parser] Parenthesized identifiers are invalid as part of a declaration](https://chromium.googlesource.com/v8/v8/+/5b4d4c2)**  
  
Date(Commit): Wed Jan 09 11:02:55 2019  
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
   
**[Issue: Null-dereference READ in v8::internal::Scope::zone](https://crbug.com/919710)**  
**[Commit: [parser] Reparse arrow functions with unidentified syntax errors in the correct scope](https://chromium.googlesource.com/v8/v8/+/7c3595e)**  
  
Date(Commit): Tue Jan 08 14:46:07 2019  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1400419](https://chromium-review.googlesource.com/c/1400419)  
Regress: [mjsunit/regress/regress-919710.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-919710.js)  
```javascript
assertThrows("( let ) => { 'use strict';  let }", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c3595e^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=7c3595e)  
[test/mjsunit/regress/regress-919710.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-919710.js?cl=7c3595e)  
  

---   

## **regress-918917.js (chromium issue)**  
   
**[Issue: DCHECK failure in HasRegisterMove(dst, src, type) in liftoff-assembler.cc](https://crbug.com/918917)**  
**[Commit: [Liftoff] Fix corner case of register moves](https://chromium.googlesource.com/v8/v8/+/f1fb7bc)**  
  
Date(Commit): Tue Jan 08 10:57:05 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1398225](https://chromium-review.googlesource.com/c/1398225)  
Regress: [mjsunit/regress/wasm/regress-918917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918917.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_v)
  .addLocals({i32_count: 1}).addLocals({f32_count: 1}).addLocals({f64_count: 1})
  .addBody([
kExprLocalGet, 1,
kExprLocalGet, 2,
kExprLocalGet, 0,
kExprIf, kWasmI32,
  kExprI32Const, 1,
kExprElse,
  kExprUnreachable,
  kExprEnd,
kExprUnreachable
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1fb7bc^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=f1fb7bc)  
[test/mjsunit/regress/wasm/regress-918917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-918917.js?cl=f1fb7bc)  
  

---   

## **regress-919340.js (chromium issue)**  
   
**[Issue: CHECK failure: TypeError: node #169:DeadValue[kRepTagged](input @0 = CheckString:CheckString) t](https://crbug.com/919340)**  
**[Commit: [turbofan] Restrict redundancy elimination from widening types](https://chromium.googlesource.com/v8/v8/+/5a9fa8f)**  
  
Date(Commit): Tue Jan 08 09:48:28 2019  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73, Release-0-M73  
Code Review: [https://chromium-review.googlesource.com/c/1397709](https://chromium-review.googlesource.com/c/1397709)  
Regress: [mjsunit/regress/regress-919340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-919340.js)  
```javascript
var E = 'Σ';
var PI = 123;
function f() {
  print(E = 2, /b/.test(E) || /b/.test(E = 2));
  (E = 3) * PI;
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5a9fa8f^!)  
[src/compiler/redundancy-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/redundancy-elimination.cc?cl=5a9fa8f)  
[test/mjsunit/regress/regress-919340.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-919340.js?cl=5a9fa8f)  
  

---   

## **regress-918763.js (chromium issue)**  
   
**[Issue: Null-dereference READ in Builtins_ArgumentsAdaptorTrampoline](https://crbug.com/918763)**  
**[Commit: [turbofan] Add missing heap object check](https://chromium.googlesource.com/v8/v8/+/426312c)**  
  
Date(Commit): Mon Jan 07 14:38:50 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
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
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/917076)**  
**[Commit: [async] The Promise.all() fast-path must check @@species protector.](https://chromium.googlesource.com/v8/v8/+/b6bcf32)**  
  
Date(Commit): Mon Jan 07 08:22:56 2019  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
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
   
**[Issue: DCHECK failure in src.is_reg_only() implies src.reg().is_byte_register() in assembler-ia32.cc](https://crbug.com/918149)**  
**[Commit: [Liftoff][ia32] Fix i64 sign extension on non-byte register](https://chromium.googlesource.com/v8/v8/+/5ed7dff)**  
  
Date(Commit): Fri Jan 04 10:12:06 2019  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Stability-Libfuzzer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, M-73, Target-73, Release-0-M73  
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
   
**[Issue: DCHECK failure in !move_dst_regs_.has(dst) in liftoff-assembler.cc](https://crbug.com/917412)**  
**[Commit: [Liftoff] Keep consistent register mapping in non-merged regions](https://chromium.googlesource.com/v8/v8/+/20b6330)**  
  
Date(Commit): Thu Jan 03 14:37:48 2019  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73  
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
kExprLocalTee, 0,
kExprLocalGet, 0,
kExprLoop, kWasmStmt,
  kExprI64Const, 0x80, 0x80, 0x80, 0x70,
  kExprLocalSet, 0x01,
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

## **regress-917588.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_fp() in liftoff-register.h](https://crbug.com/917588)**  
**[Commit: [Liftoff] Fix moving stack values](https://chromium.googlesource.com/v8/v8/+/14faced)**  
  
Date(Commit): Thu Jan 03 14:25:47 2019  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1391755](https://chromium-review.googlesource.com/c/1391755)  
Regress: [mjsunit/regress/wasm/regress-917588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917588.js), [mjsunit/regress/wasm/regress-917588b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917588b.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([], [kWasmF64]));
builder.addFunction(undefined, sig)
  .addLocals({f32_count: 5}).addLocals({f64_count: 3})
  .addBody([
kExprBlock, kWasmF64,
  kExprF64Const, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xf0, 0x3f,
  kExprF64Const, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  kExprI32Const, 0,
  kExprIf, kWasmI32,
    kExprI32Const, 0,
  kExprElse,
    kExprI32Const, 1,
    kExprEnd,
  kExprBrIf, 0,
  kExprUnreachable,
kExprEnd
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14faced^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=14faced)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=14faced)  
[test/mjsunit/regress/wasm/regress-917588.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-917588.js?cl=14faced)  
  

---   

## **regress-917988.js (chromium issue)**  
   
**[Issue: DCHECK failure in outer_scope_ == scope->outer_scope() in bytecode-generator.cc](https://crbug.com/917988)**  
**[Commit: Set the correct scope when initializing parameters.](https://chromium.googlesource.com/v8/v8/+/fa844bd)**  
  
Date(Commit): Thu Jan 03 10:18:11 2019  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
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
