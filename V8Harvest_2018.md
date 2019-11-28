# V8Harvest  
The Harvest of V8 regress in 2018.  
  

## **regress-crbug-917980.js (chromium issue)**  
   
**[Issue: Security: Heap-use-after-free in TypedArray.join](https://crbug.com/917980)**  
**[Commit: [typedarray] Check for a detached buffer before each iteration of TypedArray.p.join.](https://chromium.googlesource.com/v8/v8/+/75ca843)**  
  
Date(Commit): Mon Dec 31 18:27:51 2018  
Components: Blink>JavaScript  
Labels: reward-5000, Security_Impact-Head, Security_Severity-High, allpublic, reward-inprocess, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1392070](https://chromium-review.googlesource.com/c/1392070)  
Regress: [mjsunit/regress/regress-crbug-917980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-917980.js)  
```javascript
const constructors = [
  [Uint8Array, [0, 1]],
  [Int8Array, [0, 1]],
  [Uint16Array, [0, 1]],
  [Int16Array, [0, 1]],
  [Uint32Array, [0, 1]],
  [Int32Array, [0, 1]],
  [Float32Array, [0, 1]],
  [Float64Array, [0, 1]],
  [Uint8ClampedArray, [0, 1]],
  [BigInt64Array, [0n, 1n]],
  [BigUint64Array, [0n, 1n]]
];

let typedArray;

const separator = {
  toString() {
    %ArrayBufferDetach(typedArray.buffer);
    return '*';
  }
};

constructors.forEach(([constructor, arr]) => {
  typedArray = new constructor(arr);
  assertSame(typedArray.join(separator), '*');
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/75ca843^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=75ca843)  
[test/mjsunit/regress/regress-crbug-917980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-917980.js?cl=75ca843)  
  

---   

## **regress-905815.js (chromium issue)**  
   
**[Issue: DCHECK failure in pc <= end_ in decoder.h](https://crbug.com/905815)**  
**[Commit: [wasm] Validate prefixed opcode reads](https://chromium.googlesource.com/v8/v8/+/29c1c5d6)**  
  
Date(Commit): Fri Dec 28 07:07:11 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1390927](https://chromium-review.googlesource.com/c/1390927)  
Regress: [mjsunit/regress/wasm/regress-905815.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-905815.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  const builder = new WasmModuleBuilder();
  builder.addType(makeSig([], []));
  builder.addType(makeSig([kWasmI32], [kWasmI32]));
  builder.addFunction(undefined, 0 /* sig */)
    .addBodyWithEnd([
        kExprEnd,   // @1
    ]);
  builder.addFunction(undefined, 1 /* sig */)
    .addLocals({i32_count: 65})
    .addBodyWithEnd([
        kExprLoop, kWasmStmt,   // @3
        kSimdPrefix,
        kExprF32x4Min,
        kExprI64UConvertI32,
        kExprI64RemS,
        kExprUnreachable,
        kExprLoop, 0x02,   // @10
    ]);
})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/29c1c5d6^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=29c1c5d6)  
[test/mjsunit/regress/wasm/regress-905815.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-905815.js?cl=29c1c5d6)  
[test/mjsunit/wasm/wasm-constants.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-constants.js?cl=29c1c5d6)  
  

---   

## **regress-910824.js (chromium issue)**  
   
**[Issue: DCHECK failure in *available != 0 in assembler-arm.cc](https://crbug.com/910824)**  
**[Commit: [liftoff][arm] GetUnusedRegister before Acquire](https://chromium.googlesource.com/v8/v8/+/491eff8)**  
  
Date(Commit): Fri Dec 21 14:57:18 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-0, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Wrong, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1387498](https://chromium-review.googlesource.com/c/1387498)  
Regress: [mjsunit/regress/wasm/regress-910824.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-910824.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addGlobal(kWasmI32, 1);
builder.addGlobal(kWasmF32, 1);
builder.addType(makeSig([kWasmI32, kWasmF32, kWasmF32, kWasmF64], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */)
  .addLocals({i32_count: 504})
  .addBody([
kExprGlobalGet, 0x00,
kExprLocalSet, 0x04,
kExprLocalGet, 0x04,
kExprI32Const, 0x01,
kExprI32Sub,
kExprGlobalGet, 0x00,
kExprI32Const, 0x00,
kExprI32Eqz,
kExprGlobalGet, 0x00,
kExprI32Const, 0x01,
kExprI32Const, 0x01,
kExprI32Sub,
kExprGlobalGet, 0x00,
kExprI32Const, 0x00,
kExprI32Eqz,
kExprGlobalGet, 0x00,
kExprI32Const, 0x00,
kExprI32Const, 0x01,
kExprI32Sub,
kExprGlobalGet, 0x01,
kExprUnreachable,
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/491eff8^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=491eff8)  
[test/mjsunit/regress/wasm/regress-910824.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-910824.js?cl=491eff8)  
  

---   

## **regress-916869.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::wasm::fuzzer::WasmExecutionFuzzer::FuzzWasmModule](https://crbug.com/916869)**  
**[Commit: [wasm] Fix i8 to i32 sign extension on ia32](https://chromium.googlesource.com/v8/v8/+/f328613)**  
  
Date(Commit): Thu Dec 20 12:28:54 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Stable, Stability-Libfuzzer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, M-71, merge-merged-7.2, Release-0-M72  
Code Review: [https://chromium-review.googlesource.com/c/1386110](https://chromium-review.googlesource.com/c/1386110)  
Regress: [mjsunit/regress/wasm/regress-916869.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-916869.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction('main', sig)
    .addBody([kExprI32Const, 0x01, kExprI32SExtendI8])
    .exportFunc();
const instance = builder.instantiate();
assertEquals(1, instance.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f328613^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=f328613)  
[test/mjsunit/regress/wasm/regress-916869.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-916869.js?cl=f328613)  
[test/mjsunit/wasm/wasm-constants.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-constants.js?cl=f328613)  
  

---   

## **regress-crbug-916288.js (chromium issue)**  
   
**[Issue: DCHECK failure in IsAssignmentContext() in pattern-rewriter.cc](https://crbug.com/916288)**  
**[Commit: [parser] Eagerly throw pattern error even if we lazily throw lhs error for calls](https://chromium.googlesource.com/v8/v8/+/89a64f0)**  
  
Date(Commit): Wed Dec 19 11:39:30 2018  
Components: Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Stability-Libfuzzer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-73, Target-73  
Code Review: [https://chromium-review.googlesource.com/c/1384084](https://chromium-review.googlesource.com/c/1384084)  
Regress: [mjsunit/regress/regress-crbug-916288.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-916288.js)  
```javascript
assertThrows("(a()=0)=>0", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89a64f0^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=89a64f0)  
[test/mjsunit/regress/regress-crbug-916288.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-916288.js?cl=89a64f0)  
  

---   

## **regress-8607.js (v8 issue)**  
   
**[Issue: Dcheck on jsfunfuzz](https://crbug.com/v8/8607)**  
**[Commit: [parser] Fix late-checked destructuring pattern followed by property](https://chromium.googlesource.com/v8/v8/+/81a11c1)**  
  
Date(Commit): Tue Dec 18 17:52:10 2018  
Code Review: [https://chromium-review.googlesource.com/c/1382744](https://chromium-review.googlesource.com/c/1382744)  
Regress: [mjsunit/regress/regress-8607.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8607.js)  
```javascript
assertThrows("[({ p: this }), [][0]] = x", SyntaxError);
assertThrows("[...a, [][0]] = []", SyntaxError);
assertThrows("[...o=1,[][0]] = []", SyntaxError);
assertThrows("({x(){},y:[][0]} = {})", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/81a11c1^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=81a11c1)  
[test/mjsunit/regress/regress-8607.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8607.js?cl=81a11c1)  
  

---   

## **regress-907479.js (chromium issue)**  
   
**[Issue: Use-of-uninitialized-value in v8::internal::CopyDoubleToObjectElements](https://crbug.com/907479)**  
**[Commit: Reland "Use CopyElements (which uses memcpy) to copy FixedDoubleArray."](https://chromium.googlesource.com/v8/v8/+/63ce4ba)**  
  
Date(Commit): Tue Dec 18 16:34:49 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-Medium, Security_Impact-Head, Stability-Memory-MemorySanitizer, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1280308](https://chromium-review.googlesource.com/c/1280308)  
Regress: [mjsunit/regress/regress-907479.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-907479.js)  
```javascript
'use strict';

{
  const x = [42];
  x.splice(0, 0, 23);
  assertEquals([23, 42], x);
  x.length++;
  assertEquals([23, 42, ,], x);
  assertFalse(x.hasOwnProperty(2));
}

{
  const x = [4.2];
  x.splice(0, 0, 23);
  assertEquals([23, 4.2], x);
  x.length++;
  assertEquals([23, 4.2, ,], x);
  assertFalse(x.hasOwnProperty(2));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63ce4ba^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=63ce4ba)  
[test/mjsunit/regress/regress-907479.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-907479.js?cl=63ce4ba)  
  

---   

## **regress-crbug-915783.js (chromium issue)**  
   
**[Issue: Security: Heap-use-after-free in TypedArray.toLocaleString](https://crbug.com/915783)**  
**[Commit: [typedarray] Add TA.p.toLocaleString check for a detached buffer.](https://chromium.googlesource.com/v8/v8/+/682db78)**  
  
Date(Commit): Tue Dec 18 15:06:15 2018  
Components: Blink>JavaScript  
Labels: reward-5000, Security_Impact-Head, Security_Severity-High, allpublic, reward-inprocess, M-73  
Code Review: [https://chromium-review.googlesource.com/c/1382553](https://chromium-review.googlesource.com/c/1382553)  
Regress: [mjsunit/regress/regress-crbug-915783.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-915783.js)  
```javascript
const constructors = [
  [Uint8Array, [0, 1]],
  [Int8Array, [0, 1]],
  [Uint16Array, [0, 1]],
  [Int16Array, [0, 1]],
  [Uint32Array, [0, 1]],
  [Int32Array, [0, 1]],
  [Float32Array, [0, 1]],
  [Float64Array, [0, 1]],
  [Uint8ClampedArray, [0, 1]],
  [BigInt64Array, [0n, 1n]],
  [BigUint64Array, [0n, 1n]]
];

let typedArray;
function detachBuffer() {
  %ArrayBufferDetach(typedArray.buffer);
  return 'a';
}
Number.prototype.toString = detachBuffer;
BigInt.prototype.toString = detachBuffer;
Number.prototype.toLocaleString = detachBuffer;
BigInt.prototype.toLocaleString = detachBuffer;

constructors.forEach(([constructor, arr]) => {
  typedArray = new constructor(arr);
  assertSame(typedArray.join(), '0,1');
  assertSame(typedArray.toLocaleString(), 'a,');
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/682db78^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=682db78)  
[test/mjsunit/regress/regress-crbug-915783.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-915783.js?cl=682db78)  
  

---   

## **regress-7773.js (v8 issue)**  
   
**[Issue: Hard Crash: Check Failure in Runtime_InternalSetPrototype](https://crbug.com/v8/7773)**  
**[Commit: [runtime] Fix Runtime_InternalSetPrototype](https://chromium.googlesource.com/v8/v8/+/fb434f1)**  
  
Date(Commit): Fri Dec 14 12:06:04 2018  
Code Review: [https://chromium-review.googlesource.com/c/1375912](https://chromium-review.googlesource.com/c/1375912)  
Regress: [mjsunit/regress/regress-7773.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7773.js)  
```javascript
(function testFunctionNames() {
  let descriptor = {
    value: '',
    writable: false,
    enumerable: false,
    configurable: true
  };
  // Functions have a "name" property by default.
  assertEquals(
      descriptor, Object.getOwnPropertyDescriptor(function(){}, 'name'));
  let a = { fn: function(){} };
  assertSame('fn', a.fn.name);
  descriptor.value = 'fn';
  assertEquals(descriptor, Object.getOwnPropertyDescriptor(a.fn, 'name'));

  let b = { __proto__: function(){} };
  assertSame('', b.__proto__.name);
  descriptor.value = '';
  assertEquals(
      descriptor, Object.getOwnPropertyDescriptor(b.__proto__, 'name'));

  let c = { fn: function F(){} };
  assertSame('F', c.fn.name);
  descriptor.value = 'F';
  assertEquals(descriptor, Object.getOwnPropertyDescriptor(c.fn, 'name'));

  let d = { __proto__: function E(){} };
  assertSame('E', d.__proto__.name);
  descriptor.value = 'E';
  assertEquals(
      descriptor, Object.getOwnPropertyDescriptor(d.__proto__, 'name'));
})();

(function testClassNames() {
  let descriptor = {
    value: '',
    writable: false,
    enumerable: false,
    configurable: true
  };

  // Anonymous classes do not have a "name" property by default.
  assertSame(undefined, Object.getOwnPropertyDescriptor(class {}, 'name'));
  descriptor.value = 'C';
  assertEquals(descriptor, Object.getOwnPropertyDescriptor(class C {}, 'name'));

  let a = { fn: class {} };
  assertSame('fn', a.fn.name);
  descriptor.value = 'fn';
  assertEquals(descriptor, Object.getOwnPropertyDescriptor(a.fn, 'name'));

  let b = { __proto__: class {} };
  assertSame('', b.__proto__.name);
  assertSame(
      undefined, Object.getOwnPropertyDescriptor(b.__proto__, 'name'));

  let c = { fn: class F {} };
  assertSame('F', c.fn.name);
  descriptor.value = 'F';
  assertEquals(descriptor, Object.getOwnPropertyDescriptor(c.fn, 'name'));

  let d = { __proto__: class F {} };
  assertSame('F', d.__proto__.name);
  descriptor.value = 'F';
  assertEquals(
      descriptor, Object.getOwnPropertyDescriptor(d.__proto__, 'name'));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb434f1^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=fb434f1)  
[test/mjsunit/es6/classes.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/classes.js?cl=fb434f1)  
[test/mjsunit/regress/regress-7773.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7773.js?cl=fb434f1)  
  

---   

## **regress-913822.js (chromium issue)**  
   
**[Issue: DCHECK failure in !failed_ in asm-parser.cc](https://crbug.com/913822)**  
**[Commit: [asm.js] Fix semicolon insertion in presence of comments.](https://chromium.googlesource.com/v8/v8/+/5f8cd45)**  
  
Date(Commit): Wed Dec 12 14:43:05 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, merge-merged-7.2  
Code Review: [https://chromium-review.googlesource.com/c/1373551](https://chromium-review.googlesource.com/c/1373551)  
Regress: [mjsunit/asm/regress-913822.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-913822.js)  
```javascript
(function TestNewlineInCPPComment() {
  function Module() {
    "use asm" // Crash by comment!
    function f() {}
    return f
  }
  Module();
  assertTrue(%IsAsmWasmCode(Module));
})();

(function TestNewlineInCComment() {
  function Module() {
    "use asm" /* Crash by
    comment! */ function f() {}
    return f
  }
  Module();
  assertTrue(%IsAsmWasmCode(Module));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f8cd45^!)  
[src/asmjs/asm-scanner.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.cc?cl=5f8cd45)  
[test/mjsunit/asm/regress-913822.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-913822.js?cl=5f8cd45)  
  

---   

## **regress-crbug-913212.js (chromium issue)**  
   
**[Issue: DCHECK failure in index >= 0 && index < this->length() in fixed-array-inl.h](https://crbug.com/913212)**  
**[Commit: [ic] do not expose global object](https://chromium.googlesource.com/v8/v8/+/e5fcd33)**  
  
Date(Commit): Tue Dec 11 16:01:48 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73, Target-73, Release-0-M73  
Code Review: [https://chromium-review.googlesource.com/c/1371605](https://chromium-review.googlesource.com/c/1371605)  
Regress: [mjsunit/regress/regress-crbug-913212.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-913212.js)  
```javascript
const globalThis = this;
Object.setPrototypeOf(this, new Proxy({}, {
  has() { return true; },
  getOwnPropertyDescriptor() {
    assertUnreachable("getOwnPropertyDescriptor shouldn't be called."); },
  get(target, prop, receiver) {
    assertTrue(receiver === globalThis);
  }
}));
undefined_name_access  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5fcd33^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e5fcd33)  
[test/mjsunit/regress/regress-crbug-913212.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-913212.js?cl=e5fcd33)  
  

---   

## **regress-912504.js (chromium issue)**  
   
**[Issue: CHECK failure: fixed_size_above_fp + in deoptimizer.cc](https://crbug.com/912504)**  
**[Commit: [esnext] use variadic arguments for Object.fromEntries](https://chromium.googlesource.com/v8/v8/+/5c77970)**  
  
Date(Commit): Tue Dec 11 15:58:52 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-73  
Code Review: [https://chromium-review.googlesource.com/c/1366397](https://chromium-review.googlesource.com/c/1366397)  
Regress: [mjsunit/harmony/regress/regress-912504.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-912504.js)  
```javascript
function test() {
  Object.fromEntries([[]]);
  %DeoptimizeNow();
}
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c77970^!)  
[src/builtins/object-fromentries.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/object-fromentries.tq?cl=5c77970)  
[test/mjsunit/harmony/regress/regress-912504.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-912504.js?cl=5c77970)  
  

---   

## **regress-913232.js (chromium issue)**  
   
**[Issue: DCHECK failure in HasIncomingBackEdges(block) implies block_effects.For(block->PredecessorAt(0), b](https://crbug.com/913232)**  
**[Commit: [compiler] Relax too strict debug assert.](https://chromium.googlesource.com/v8/v8/+/dc6eed6)**  
  
Date(Commit): Tue Dec 11 15:51:53 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1372065](https://chromium-review.googlesource.com/c/1372065)  
Regress: [mjsunit/compiler/regress-913232.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-913232.js)  
```javascript
function* E(b) {
  while (true) {
    for (yield* 0; b; yield* 0) {}
  }
}

%PrepareFunctionForOptimization(E);
%OptimizeFunctionOnNextCall(E);
E();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc6eed6^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=dc6eed6)  
[test/mjsunit/compiler/regress-913232.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-913232.js?cl=dc6eed6)  
  

---   

## **regress-913804.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::compiler::Node::AppendUse](https://crbug.com/913804)**  
**[Commit: [wasm] Fix return from unreachable code](https://chromium.googlesource.com/v8/v8/+/573e412)**  
  
Date(Commit): Tue Dec 11 12:01:10 2018  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Stability-AFL, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1371566](https://chromium-review.googlesource.com/c/1371566)  
Regress: [mjsunit/regress/wasm/regress-913804.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-913804.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('main', kSig_v_v).addBody([
  kExprLoop, kWasmStmt,        // loop
  /**/ kExprBr, 0x01,          //   br depth=1
  /**/ kExprBlock, kWasmStmt,  //   block
  /**/ /**/ kExprBr, 0x02,     //     br depth=2
  /**/ /**/ kExprEnd,          //     end [block]
  /**/ kExprEnd                //   end [loop]
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/573e412^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=573e412)  
[test/mjsunit/regress/wasm/regress-913804.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-913804.js?cl=573e412)  
  

---   

## **regress-crbug-913296.js (chromium issue)**  
   
**[Issue: Security: V8: Incorrect type information on SpeculativeSafeIntegerSubtract](https://crbug.com/913296)**  
**[Commit: [turbofan] Fix wrong typing of SpeculativeSafeIntegerSubtract.](https://chromium.googlesource.com/v8/v8/+/e3c9239)**  
  
Date(Commit): Tue Dec 11 10:21:35 2018  
Components: Blink>JavaScript>Compiler  
Labels: reward-5000, Security_Impact-Stable, Security_Severity-High, allpublic, reward-inprocess, M-72, Merge-Rejected-71, CVE_description-submitted, merge-merged-7.2, Release-0-M72, CVE-2019-5755  
Code Review: [https://chromium-review.googlesource.com/c/1370042](https://chromium-review.googlesource.com/c/1370042)  
Regress: [mjsunit/regress/regress-crbug-913296.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-913296.js)  
```javascript
function foo(trigger) {
  return Object.is((trigger ? -0 : 0) - 0, -0);
};
%PrepareFunctionForOptimization(foo);
assertFalse(foo(false));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3c9239^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=e3c9239)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=e3c9239)  
[test/mjsunit/regress/regress-crbug-913296.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-913296.js?cl=e3c9239)  
  

---   

## **regress-crbug-911416.js (chromium issue)**  
   
**[Issue: Security: SEGV_ACCERR in Symbol.prototype.description hash calc](https://crbug.com/911416)**  
**[Commit: Add test case for RO-space string used as property key.](https://chromium.googlesource.com/v8/v8/+/4233ec0)**  
  
Date(Commit): Mon Dec 10 08:55:45 2018  
Components: Blink>JavaScript  
Labels: reward-0, Hotlist-Merge-Approved, Security_Severity-High, Security_Impact-Beta, ReleaseBlock-Stable, allpublic, ClusterFuzz-Verified, M-72, Test-Predator-Auto-CC, Test-Predator-Auto-Components, Target-72, merge-merged-7.2  
Code Review: [https://chromium-review.googlesource.com/c/1362952](https://chromium-review.googlesource.com/c/1362952)  
Regress: [mjsunit/regress/regress-crbug-911416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-911416.js)  
```javascript
assertEquals(7, ({[Symbol.hasInstance.description]:7})["Symbol.hasInstance"]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4233ec0^!)  
[test/mjsunit/regress/regress-crbug-911416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-911416.js?cl=4233ec0)  
  

---   

## **regress-910838.js (chromium issue)**  
   
**[Issue: Unknown signal in Builtins_ArgumentsAdaptorTrampoline](https://crbug.com/910838)**  
**[Commit: [turbofan] Pin pure unreachable values to effect chain (in rep selection)](https://chromium.googlesource.com/v8/v8/+/f27ac28)**  
  
Date(Commit): Thu Dec 06 10:35:13 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1365274](https://chromium-review.googlesource.com/c/1365274)  
Regress: [mjsunit/compiler/regress-910838.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-910838.js)  
```javascript
function f(b, s, x) {
  if (!b) {
    return (x ? b : s * undefined) ? 1 : 42;
  }
}

function g(b, x) {
  return f(b, 'abc', x);
}

%PrepareFunctionForOptimization(g);
f(false, 0, 0);
g(true, 0);
%OptimizeFunctionOnNextCall(g);
assertEquals(42, g(false, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f27ac28^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=f27ac28)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=f27ac28)  
[test/mjsunit/compiler/regress-910838.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-910838.js?cl=f27ac28)  
  

---   

## **regress-8533.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/8533)**  
**[Commit: [wasm] Load thread-in-wasm flag from the isolate](https://chromium.googlesource.com/v8/v8/+/148ef60)**  
  
Date(Commit): Wed Dec 05 15:10:11 2018  
Code Review: [https://chromium-review.googlesource.com/c/1358518](https://chromium-review.googlesource.com/c/1358518)  
Regress: [mjsunit/regress/wasm/regress-8533.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8533.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


const sync_address = 12;
(function TestPostModule() {
  let builder = new WasmModuleBuilder();
  let sig_index = builder.addType(kSig_v_v);
  let import_id = builder.addImport('m', 'func', sig_index);
  builder.addFunction('wait', kSig_v_v)
      .addBody([
        // Calling the imported function sets the thread-in-wasm flag of the
        // main thread.
        kExprCallFunction, import_id,  // --
        kExprLoop, kWasmStmt,          // --
        kExprI32Const, sync_address,   // --
        kExprI32LoadMem, 0, 0,         // --
        kExprI32Eqz,
        kExprBrIf, 0,                  // --
        kExprEnd,
      ])
      .exportFunc();

  builder.addFunction('signal', kSig_v_v)
      .addBody([
        kExprI32Const, sync_address,  // --
        kExprI32Const, 1,             // --
        kExprI32StoreMem, 0, 0,       // --
        ])
      .exportFunc();
  builder.addImportedMemory("m", "imported_mem", 0, 1, "shared");

  let module = builder.toModule();
  let memory = new WebAssembly.Memory({initial: 1, maximum: 1, shared: true});

  let workerScript = `
    onmessage = function(msg) {
      try {
        let worker_instance = new WebAssembly.Instance(msg.module,
            {m: {imported_mem: msg.memory,
                 func: _ => 5}});
        postMessage("start running");
        worker_instance.exports.wait();
        postMessage("finished");
      } catch(e) {
        postMessage('ERROR: ' + e);
      }
    }
  `;

  let worker = new Worker(workerScript, {type: 'string'});
  worker.postMessage({module: module, memory: memory});

  let main_instance = new WebAssembly.Instance(
      module, {m: {imported_mem: memory, func: _ => 7}});

  let counter = 0;
  function CheckThreadNotInWasm() {
    // We check the thread-in-wasm flag many times and reschedule ourselves in
    // between to increase the chance that we read the flag set by the worker.
    assertFalse(%IsThreadInWasm());
    counter++;
    if (counter < 100) {
      setTimeout(CheckThreadNotInWasm, 0);
    } else {
      main_instance.exports.signal(sync_address);
      assertEquals('finished', worker.getMessage());
      worker.terminate();
    }
  }

  assertFalse(%IsThreadInWasm());
  assertEquals('start running', worker.getMessage());
  CheckThreadNotInWasm();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/148ef60^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=148ef60)  
[src/external-reference.cc](https://cs.chromium.org/chromium/src/v8/src/external-reference.cc?cl=148ef60)  
[src/external-reference.h](https://cs.chromium.org/chromium/src/v8/src/external-reference.h?cl=148ef60)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=148ef60)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=148ef60)  
...  
  

---   

## **regress-crbug-909614.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo:ia32,ignition_turbo](https://crbug.com/909614)**  
**[Commit: [bigint] Make kMaxLength platform-independent.](https://chromium.googlesource.com/v8/v8/+/9d51166)**  
  
Date(Commit): Fri Nov 30 23:43:29 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1355625](https://chromium-review.googlesource.com/c/1355625)  
Regress: [mjsunit/regress/regress-crbug-909614.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-909614.js)  
```javascript
let just_under = 2n ** 30n - 1n;
let just_above = 2n ** 30n;

assertDoesNotThrow(() => { var dummy = 2n ** just_under; });
assertThrows(() => { var dummy = 2n ** just_above; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9d51166^!)  
[src/objects/bigint.h](https://cs.chromium.org/chromium/src/v8/src/objects/bigint.h?cl=9d51166)  
[test/mjsunit/harmony/bigint/regressions.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/bigint/regressions.js?cl=9d51166)  
[test/mjsunit/regress/regress-crbug-909614.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-909614.js?cl=9d51166)  
  

---   

## **regress-908975.js (chromium issue)**  
   
**[Issue: DCHECK failure in outer_scope_ == scope->outer_scope() in bytecode-generator.cc](https://crbug.com/908975)**  
**[Commit: [parser] Set rewritable_length to the correct length rather than 0](https://chromium.googlesource.com/v8/v8/+/bd114da)**  
  
Date(Commit): Wed Nov 28 08:53:26 2018  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1352286](https://chromium-review.googlesource.com/c/1352286)  
Regress: [mjsunit/regress/regress-908975.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-908975.js)  
```javascript
[] = [];
a => 0  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd114da^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=bd114da)  
[test/mjsunit/regress/regress-908975.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-908975.js?cl=bd114da)  
  

---   

## **regress-904167.js (chromium issue)**  
   
**[Issue: DCHECK failure in !IsSmi() == Internals::HasHeapObjectTag(ptr()) in objects.h](https://crbug.com/904167)**  
**[Commit: [cloneobjectic] initialize property array before filling it](https://chromium.googlesource.com/v8/v8/+/3729410)**  
  
Date(Commit): Tue Nov 27 17:24:21 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1350282](https://chromium-review.googlesource.com/c/1350282)  
Regress: [mjsunit/es9/regress/regress-904167.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-904167.js)  
```javascript
var src, clone;
for (var i = 0; i < 40000; i++) {
    src = { ...i, x: -9007199254740991 };
    clone = { ...src };
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3729410^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=3729410)  
[test/mjsunit/es9/regress/regress-904167.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-904167.js?cl=3729410)  
  

---   

## **regress-crbug-90771.js (chromium issue)**  
   
**[Issue: DOMUI: Sync errors surfacing on NTP, Preferences and Bookmarks Bar are completely out of sync](https://crbug.com/90771)**  
**[Commit: Fix Reflect.construct with constructors without a prototype slot](https://chromium.googlesource.com/v8/v8/+/7a3cb59)**  
  
Date(Commit): Tue Nov 27 11:52:41 2018  
Components: UI, Services>Sync  
Labels: Restrict-AddIssueComment-EditIssue, M-14, Merge-Merged-835, ReleaseBlock-Stable  
Code Review: [https://chromium-review.googlesource.com/c/1351023](https://chromium-review.googlesource.com/c/1351023)  
Regress: [mjsunit/regress/regress-crbug-90771.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-90771.js)  
```javascript
function target() {};

for (let key of Object.getOwnPropertyNames(this)) {
  try {
    let newTarget = this[key];
    let arg = target;
    Reflect.construct(target, arg, newTarget);
  } catch {}
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a3cb59^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7a3cb59)  
[test/mjsunit/regress/regress-crbug-90771.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-90771.js?cl=7a3cb59)  
  

---   

## **regress-8505.js (v8 issue)**  
   
**[Issue: Intrinsified Math.pow is slightly incorrect in WASM and asm.js](https://crbug.com/v8/8505)**  
**[Commit: [wasm] Intrinsify math imports](https://chromium.googlesource.com/v8/v8/+/99484e2)**  
  
Date(Commit): Mon Nov 26 15:17:51 2018  
Code Review: [https://chromium-review.googlesource.com/c/1349279](https://chromium-review.googlesource.com/c/1349279)  
Regress: [mjsunit/regress/wasm/regress-8505.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8505.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

function verbose(args) {
  // print(...args);
}

let numFailures = 0;

function reportFailure(name, vals, m, w) {
  print("  error: " + name + "(" + vals + ") == " + w + ", expected " + m);
  numFailures++;
}

let global_imports = {Math: Math};

let inputs = [
  1 / 0,
  -1 / 0,
  0 / 0,
  -2.70497e+38,
  -1.4698e+37,
  -1.22813e+35,
  -1.34584e+34,
  -1.0079e+32,
  -6.49364e+26,
  -3.06077e+25,
  -1.46821e+25,
  -1.17658e+23,
  -1.9617e+22,
  -2.7357e+20,
  -9223372036854775808.0,  // INT64_MIN
  -1.48708e+13,
  -1.89633e+12,
  -4.66622e+11,
  -2.22581e+11,
  -1.45381e+10,
  -2147483904.0,  // First float32 after INT32_MIN
  -2147483648.0,  // INT32_MIN
  -2147483520.0,  // Last float32 before INT32_MIN
  -1.3956e+09,
  -1.32951e+09,
  -1.30721e+09,
  -1.19756e+09,
  -9.26822e+08,
  -5.09256e+07,
  -964300.0,
  -192446.0,
  -28455.0,
  -27194.0,
  -20575.0,
  -17069.0,
  -9167.0,
  -960.178,
  -113.0,
  -62.0,
  -15.0,
  -7.0,
  -1.0,
  -0.0256635,
  -4.60374e-07,
  -3.63759e-10,
  -4.30175e-14,
  -5.27385e-15,
  -1.5707963267948966,
  -1.48084e-15,
  -2.220446049250313e-16,
  -1.05755e-19,
  -3.2995e-21,
  -1.67354e-23,
  -1.11885e-23,
  -1.78506e-30,
  -1.43718e-34,
  -1.27126e-38,
  -0.0,
  3e-88,
  -2e66,
  0.0,
  2e66,
  1.17549e-38,
  1.56657e-37,
  4.08512e-29,
  6.25073e-22,
  4.1723e-13,
  1.44343e-09,
  1.5707963267948966,
  5.27004e-08,
  9.48298e-08,
  5.57888e-07,
  4.89988e-05,
  0.244326,
  1.0,
  12.4895,
  19.0,
  47.0,
  106.0,
  538.324,
  564.536,
  819.124,
  7048.0,
  12611.0,
  19878.0,
  20309.0,
  797056.0,
  1.77219e+09,
  2147483648.0,  // INT32_MAX + 1
  4294967296.0,  // UINT32_MAX + 1
  1.51116e+11,
  4.18193e+13,
  3.59167e+16,
  9223372036854775808.0,   // INT64_MAX + 1
  18446744073709551616.0,  // UINT64_MAX + 1
  3.38211e+19,
  2.67488e+20,
  1.78831e+21,
  9.20914e+21,
  8.35654e+23,
  1.4495e+24,
  5.94015e+25,
  4.43608e+30,
  2.44502e+33,
  1.38178e+37,
  1.71306e+37,
  3.31899e+38,
  3.40282e+38,
];

function assertBinop(name, math_func, wasm_func) {
  let inputs2 = [ 1, 0.5, -1, -0.5,  0, -0, 1/0, -1/0, 0/0 ];
  for (val of inputs) {
    verbose("  ", val);
    for (val2 of inputs2) {
      verbose("    ", val2);
      let m = math_func(val, val2);
      let w = wasm_func(val, val2);
      if (!deepEquals(m, w)) reportFailure(name, [val, val2], m, w);
      m = math_func(val2, val);
      w = wasm_func(val2, val);
      if (!deepEquals(m, w)) reportFailure(name, [val2, val], m, w);
    }
  }
}

let stdlib = this;
function Module_pow(stdlib) {
  "use asm";

  var Stdlib = stdlib.Math.pow;

  function pow(a, b) {
    a = +a;
    b = +b;
    return +Stdlib(a, b);
  }

  return {pow: pow};
}

function wasmBinop(name, sig) {
  var builder = new WasmModuleBuilder();

  var sig_index = builder.addType(sig);
  builder.addImport('Math', name, sig_index);
  builder.addFunction('main', sig_index)
      .addBody([
        kExprLocalGet, 0,  // --
        kExprLocalGet, 1,  // --
        kExprCallFunction, 0
      ])  // --
      .exportAs('main');

  return builder.instantiate(global_imports).exports.main;
}

function asmBinop(name) {
  let instance = Module_pow(stdlib);
  assertTrue(%IsAsmWasmCode(Module_pow));

  let asm_func = instance[name];
  if (typeof asm_func != "function") throw "asm[" + full_name + "] not found";
  return asm_func;
}

(function TestF64() {
  let name = 'pow';
  let math_func = Math[name];

  let wasm_func = wasmBinop(name, kSig_d_dd);
  assertBinop("(f64)" + name, math_func, wasm_func);

  let asm_func = asmBinop(name);
  assertBinop("(f64)" + name, math_func, asm_func);
})();

assertEquals(0, numFailures);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/99484e2^!)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=99484e2)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=99484e2)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=99484e2)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=99484e2)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=99484e2)  
...  
  

---   

## **regress-crbug-908309.js (chromium issue)**  
   
**[Issue: Unknown signal in Builtins_InterpreterEntryTrampoline](https://crbug.com/908309)**  
**[Commit: [turbofan] Fix types of Promise#catch() and Promise#finally().](https://chromium.googlesource.com/v8/v8/+/1bfb024)**  
  
Date(Commit): Mon Nov 26 14:04:09 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1350789](https://chromium-review.googlesource.com/c/1350789)  
Regress: [mjsunit/regress/regress-crbug-908309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-908309.js)  
```javascript
const p = Object.defineProperty(Promise.resolve(), 'then', {
  value() {
    return 0;
  }
});

(function() {
function foo() {
  return p.catch().catch();
};
%PrepareFunctionForOptimization(foo);
assertThrows(foo, TypeError);
assertThrows(foo, TypeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);
})();

(function() {
function foo() {
  return p.finally().finally();
};
%PrepareFunctionForOptimization(foo);
assertThrows(foo, TypeError);
assertThrows(foo, TypeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1bfb024^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=1bfb024)  
[test/mjsunit/regress/regress-crbug-908309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-908309.js?cl=1bfb024)  
  

---   

## **regress-908231.js (chromium issue)**  
   
**[Issue: DCHECK failure in parse_lazily() implies allow_lazy_ in parser.cc](https://crbug.com/908231)**  
**[Commit: [parser] Relax DCHECK in has_error() case](https://chromium.googlesource.com/v8/v8/+/536f62c)**  
  
Date(Commit): Mon Nov 26 10:06:28 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1350116](https://chromium-review.googlesource.com/c/1350116)  
Regress: [mjsunit/regress/regress-908231.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-908231.js)  
```javascript
assertThrows(`
    class C {
      get [(function() { function lazy() { Syntax Error } })()]() {}
    }`, SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/536f62c^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=536f62c)  
[test/mjsunit/regress/regress-908231.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-908231.js?cl=536f62c)  
  

---   

## **regress-908250.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::AstTraversalVisitor<v8::internal::InitializerRewriter>::VisitNoSta](https://crbug.com/908250)**  
**[Commit: [parser] Don't rewrite parameters if has_error()](https://chromium.googlesource.com/v8/v8/+/0b48031)**  
  
Date(Commit): Mon Nov 26 09:19:05 2018  
Components: Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1350117](https://chromium-review.googlesource.com/c/1350117)  
Regress: [mjsunit/regress/regress-908250.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-908250.js)  
```javascript
assertThrows("(al,al,e={}=e)=>l", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b48031^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=0b48031)  
[test/mjsunit/regress/regress-908250.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-908250.js?cl=0b48031)  
  

---   

## **regress-907575.js (chromium issue)**  
   
**[Issue: DCHECK failure in binop->op() == Token::COMMA in parser.cc](https://crbug.com/907575)**  
**[Commit: [parser] Drop ExpressionClassifier::ArrowFormalsParameterProduction and BP_to_AFP](https://chromium.googlesource.com/v8/v8/+/71f59a2)**  
  
Date(Commit): Thu Nov 22 15:13:41 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1348078](https://chromium-review.googlesource.com/c/1348078)  
Regress: [mjsunit/regress/regress-907575.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-907575.js)  
```javascript
assertThrows("0 || () =>", SyntaxError);
assertThrows("++(a) =>", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/71f59a2^!)  
[src/parsing/expression-classifier.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-classifier.h?cl=71f59a2)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=71f59a2)  
[test/mjsunit/regress/regress-907575.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-907575.js?cl=71f59a2)  
  

---   

## **regress-907669.js (chromium issue)**  
   
**[Issue: DCHECK failure in !has_error() implies !next_arrow_formals_parenthesized_ in parser-base.h](https://crbug.com/907669)**  
**[Commit: [parser] Don't re-preparse when trying to find an unidentifiable error](https://chromium.googlesource.com/v8/v8/+/23e99a9)**  
  
Date(Commit): Thu Nov 22 13:00:32 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1347486](https://chromium-review.googlesource.com/c/1347486)  
Regress: [mjsunit/regress/regress-907669.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-907669.js)  
```javascript
assertThrows("function f() { function g() { (); ", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/23e99a9^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=23e99a9)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=23e99a9)  
[test/mjsunit/regress/regress-907669.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-907669.js?cl=23e99a9)  
  

---   

## **regress-crbug-906043.js (chromium issue)**  
   
**[Issue: Security: Tianfu CUP RCE](https://crbug.com/906043)**  
**[Commit: [runtime] Reduce spread/apply call max arguments](https://chromium.googlesource.com/v8/v8/+/4e3a17d)**  
  
Date(Commit): Thu Nov 22 12:08:17 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, M-72, CVE_description-submitted, M-71, Target-71, Target-72, merge-merged-7.1, merge-merged-7.2, Release-0-M72, CVE-2019-5782  
Code Review: [https://chromium-review.googlesource.com/c/1346115](https://chromium-review.googlesource.com/c/1346115)  
Regress: [mjsunit/regress/regress-crbug-906043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906043.js)  
```javascript
function fun(arg) {
  let x = arguments.length;
  a1 = new Array(0x10);
  a1[0] = 1.1;
  a2 = new Array(0x10);
  a2[0] = 1.1;
  a1[(x >> 16) * 21] = 1.39064994160909e-309;  // 0xffff00000000
  a1[(x >> 16) * 41] = 8.91238232205e-313;  // 0x2a00000000
}

var a1, a2;
var a3 = [1.1, 2.2];
a3.length = 0x11000;
a3.fill(3.3);

var a4 = [1.1];

%PrepareFunctionForOptimization(fun);
for (let i = 0; i < 3; i++) fun(...a4);
%OptimizeFunctionOnNextCall(fun);
fun(...a4);

res = fun(...a3);

assertEquals(16, a2.length);
for (let i = 8; i < 32; i++) {
  assertEquals(undefined, a2[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e3a17d^!)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=4e3a17d)  
[src/message-template.h](https://cs.chromium.org/chromium/src/v8/src/message-template.h?cl=4e3a17d)  
[test/mjsunit/apply.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/apply.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-3027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3027.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-331444.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-331444.js?cl=4e3a17d)  
...  
  

---   

## **regress-906406.js (chromium issue)**  
   
**[Issue: Null-dereference READ in opcode](https://crbug.com/906406)**  
**[Commit: [turbofan] Apply duct-tape to load elimination](https://chromium.googlesource.com/v8/v8/+/b28637b)**  
  
Date(Commit): Wed Nov 21 15:23:01 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1346491](https://chromium-review.googlesource.com/c/1346491)  
Regress: [mjsunit/regress/regress-906406.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-906406.js)  
```javascript
for (x = 0; x < 10000; ++x) {
    [(x) => x, [, 4294967295].find((x) => x), , 2].includes('x', -0);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b28637b^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=b28637b)  
[test/mjsunit/regress/regress-906406.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-906406.js?cl=b28637b)  
  

---   

## **regress-crbug-906870.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/906870)**  
**[Commit: [turbofan] Properly turn `Number.min(-0,+0)` into `-0`.](https://chromium.googlesource.com/v8/v8/+/154cb3f)**  
  
Date(Commit): Tue Nov 20 11:00:41 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1342920](https://chromium-review.googlesource.com/c/1342920)  
Regress: [mjsunit/regress/regress-crbug-906870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906870.js)  
```javascript
(function() {
function foo() {
  return Infinity / Math.max(-0, +0);
};
%PrepareFunctionForOptimization(foo);
assertEquals(+Infinity, foo());
assertEquals(+Infinity, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(+Infinity, foo());
})();

(function() {
function foo() {
  return Infinity / Math.max(+0, -0);
};
%PrepareFunctionForOptimization(foo);
assertEquals(+Infinity, foo());
assertEquals(+Infinity, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(+Infinity, foo());
})();

(function() {
function foo() {
  return Infinity / Math.min(-0, +0);
};
%PrepareFunctionForOptimization(foo);
assertEquals(-Infinity, foo());
assertEquals(-Infinity, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(-Infinity, foo());
})();

(function() {
function foo() {
  return Infinity / Math.min(+0, -0);
};
%PrepareFunctionForOptimization(foo);
assertEquals(-Infinity, foo());
assertEquals(-Infinity, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(-Infinity, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/154cb3f^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=154cb3f)  
[test/mjsunit/regress/regress-crbug-906870.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-906870.js?cl=154cb3f)  
  

---   

## **regress-906893.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: IsFastRegExpWithOriginalExec(context, regexp)](https://crbug.com/906893)**  
**[Commit: [turbofan] Fix RegExp.p.exec modification test.](https://chromium.googlesource.com/v8/v8/+/86894d9)**  
  
Date(Commit): Tue Nov 20 06:36:53 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1342919](https://chromium-review.googlesource.com/c/1342919)  
Regress: [mjsunit/regress-906893.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-906893.js)  
```javascript
const r = /x/;
let counter = 0;

r.exec = () => { counter++; return null; }

function f() {
  r.test("ABcd");
}

%PrepareFunctionForOptimization(f);
f();
assertEquals(1, counter);
%OptimizeFunctionOnNextCall(f);

f();
assertEquals(2, counter);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/86894d9^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=86894d9)  
[test/mjsunit/regress-906893.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-906893.js?cl=86894d9)  
  

---   

## **regress-crbug-906220.js (chromium issue)**  
   
**[Issue: DCHECK failure in index >= 0 in escape-analysis.cc](https://crbug.com/906220)**  
**[Commit: [turbofan] Fix negative offset handling in escape analysis.](https://chromium.googlesource.com/v8/v8/+/2bc9d01)**  
  
Date(Commit): Mon Nov 19 11:07:38 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1340292](https://chromium-review.googlesource.com/c/1340292)  
Regress: [mjsunit/regress/regress-crbug-906220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906220.js)  
```javascript
function foo() {
  new Array().pop();
};
%PrepareFunctionForOptimization(foo);
assertEquals(undefined, foo());
assertEquals(undefined, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2bc9d01^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=2bc9d01)  
[test/mjsunit/regress/regress-crbug-906220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-906220.js?cl=2bc9d01)  
  

---   

## **regress-regexp-functional-replace-slow.js (other issue)**  
   
**[Commit: Reland "[regexp] Introduce species constructor protector for regexps."](https://chromium.googlesource.com/v8/v8/+/a27a42f)**  
  
Date(Commit): Mon Nov 19 10:58:01 2018  
Code Review: [https://chromium-review.googlesource.com/c/1335696](https://chromium-review.googlesource.com/c/1335696)  
Regress: [mjsunit/regress-regexp-functional-replace-slow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-regexp-functional-replace-slow.js)  
```javascript
/a/.constructor = "";

assertEquals("b", "a".replace(/a/, () => "b"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a27a42f^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=a27a42f)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=a27a42f)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=a27a42f)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=a27a42f)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=a27a42f)  
...  
  
  
---   

## **regress-905555-2.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/905555)**  
**[Commit: [turbofan] Fix property cell dependencies.](https://chromium.googlesource.com/v8/v8/+/7b7e61c)**  
  
Date(Commit): Mon Nov 19 10:24:42 2018  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, ClusterFuzz-Wrong, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/c/1339863](https://chromium-review.googlesource.com/c/1339863)  
Regress: [mjsunit/compiler/regress-905555-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-905555-2.js), [mjsunit/compiler/regress-905555.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-905555.js)  
```javascript
global = 1;

function boom(value) {
  return global;
}

%PrepareFunctionForOptimization(boom);
assertEquals(1, boom());
assertEquals(1, boom());
%OptimizeFunctionOnNextCall(boom, "concurrent");
assertEquals(1, boom());

delete this.global;

%UnblockConcurrentRecompilation();

assertUnoptimized(boom, "sync");

assertThrows(boom);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b7e61c^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=7b7e61c)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=7b7e61c)  
[test/mjsunit/compiler/regress-905555-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-905555-2.js?cl=7b7e61c)  
[test/mjsunit/compiler/regress-905555.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-905555.js?cl=7b7e61c)  
  

---   

## **regress-905587.js (chromium issue)**  
   
**[Issue: DCHECK failure in token.invalid_template_escape_message == MessageTemplate::kNone in scanner.cc](https://crbug.com/905587)**  
**[Commit: [scanner] Reset invalid_template_escape_message during Bookmark::Apply](https://chromium.googlesource.com/v8/v8/+/c8cbf23)**  
  
Date(Commit): Fri Nov 16 10:43:24 2018  
Components: Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Stability-Libfuzzer, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1340040](https://chromium-review.googlesource.com/c/1340040)  
Regress: [mjsunit/regress/regress-905587.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-905587.js)  
```javascript
assertThrows("function test() { '\\u`''\\u' }", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8cbf23^!)  
[src/parsing/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.cc?cl=c8cbf23)  
[test/mjsunit/regress/regress-905587.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-905587.js?cl=c8cbf23)  
  

---   

## **regress-905907.js (chromium issue)**  
   
**[Issue: DCHECK failure in (function_) == nullptr in scopes.cc](https://crbug.com/905907)**  
**[Commit: [parser] Declare scope-info deserialized function var on the cache scope](https://chromium.googlesource.com/v8/v8/+/7762b23)**  
  
Date(Commit): Fri Nov 16 10:12:21 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1339862](https://chromium-review.googlesource.com/c/1339862)  
Regress: [mjsunit/regress/regress-905907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-905907.js)  
```javascript
var g = function f(a = 3) {
  var context_allocated = undefined;
  function inner() { f(); f(context_allocated) };
  inner();
};
assertThrows("g()", RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7762b23^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=7762b23)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=7762b23)  
[test/mjsunit/regress/regress-905907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-905907.js?cl=7762b23)  
  

---   

## **regress-v8-8445-2.js (v8 issue)**  
   
**[Issue: Spec violation: RegExp.constructor not respected](https://crbug.com/v8/8445)**  
**[Commit: [regexp] Introduce species constructor protector for regexps.](https://chromium.googlesource.com/v8/v8/+/3ca32e9)**  
  
Date(Commit): Fri Nov 16 10:07:03 2018  
Code Review: [https://chromium-review.googlesource.com/c/1335696](https://chromium-review.googlesource.com/c/1335696)  
Regress: [mjsunit/regress-v8-8445-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-v8-8445-2.js), [mjsunit/regress-v8-8445.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-v8-8445.js)  
```javascript
class MyRegExp {
  exec() { return null; }
}

var r = /c/g;

assertEquals(["ab", ""], "abc".split(r));
assertEquals([["c"]], [..."c".matchAll(r)]);

r.constructor =  { [Symbol.species] : MyRegExp };

assertEquals(["abc"], "abc".split(r));
assertEquals([], [..."c".matchAll(r)]);

assertEquals(["ab", ""], "abc".split(/c/g));
assertEquals([["c"]], [..."c".matchAll(/c/g)]);

RegExp.prototype.constructor =  { [Symbol.species] : MyRegExp };

assertEquals(["abc"], "abc".split(/c/g));
assertEquals([], [..."c".matchAll(/c/g)]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ca32e9^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=3ca32e9)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=3ca32e9)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=3ca32e9)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=3ca32e9)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=3ca32e9)  
...  
  

---   

## **regress-894307.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/894307)**  
**[Commit: [Liftoff] Fix 64bit shift on ia32](https://chromium.googlesource.com/v8/v8/+/59a8eba)**  
  
Date(Commit): Thu Nov 15 16:43:34 2018  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/1337580](https://chromium-review.googlesource.com/c/1337580)  
Regress: [mjsunit/regress/wasm/regress-894307.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-894307.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = makeSig([kWasmI32, kWasmI64, kWasmI64], [kWasmI64]);
builder.addFunction(undefined, sig)
  .addBody([
    kExprLocalGet, 2,
    kExprLocalGet, 1,
    kExprI64Shl,
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/59a8eba^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=59a8eba)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=59a8eba)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=59a8eba)  
[test/mjsunit/regress/wasm/regress-894307.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-894307.js?cl=59a8eba)  
  

---   

## **regress-crbug-905457.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/905457)**  
**[Commit: [turbofan] Preserve NaN properly for NumberMin and NumberMax.](https://chromium.googlesource.com/v8/v8/+/a2f7867)**  
  
Date(Commit): Thu Nov 15 12:32:03 2018  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1337576](https://chromium-review.googlesource.com/c/1337576)  
Regress: [mjsunit/regress/regress-crbug-905457.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-905457.js)  
```javascript
(function() {
function foo(x) {
  return Math.abs(Math.min(+x, 0));
};
%PrepareFunctionForOptimization(foo);
assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());
})();

(function() {
function foo(x) {
  return Math.abs(Math.min(-x, 0));
};
%PrepareFunctionForOptimization(foo);
assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());
})();

(function() {
function foo(x) {
  return Math.abs(Math.max(0, +x));
};
%PrepareFunctionForOptimization(foo);
assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());
})();

(function() {
function foo(x) {
  return Math.abs(Math.max(0, -x));
};
%PrepareFunctionForOptimization(foo);
assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2f7867^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=a2f7867)  
[test/mjsunit/regress/regress-crbug-905457.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-905457.js?cl=a2f7867)  
  

---   

## **regress-crbug-900674.js (chromium issue)**  
   
**[Issue: DCHECK failure in IsNumber() in objects-inl.h](https://crbug.com/900674)**  
**[Commit: [async-hooks] Fix Promise.resolve optimization with async hooks enabled](https://chromium.googlesource.com/v8/v8/+/607033a)**  
  
Date(Commit): Wed Nov 14 15:29:09 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1335693](https://chromium-review.googlesource.com/c/1335693)  
Regress: [mjsunit/regress/regress-crbug-900674.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-900674.js)  
```javascript
function foo() {
  let val = Promise.resolve().then();
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/607033a^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=607033a)  
[test/mjsunit/regress/regress-crbug-900674.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-900674.js?cl=607033a)  
  

---   

## **regress-904417.js (chromium issue)**  
   
**[Issue: CHECK failure: serialized_prototype_ in js-heap-broker.cc](https://crbug.com/904417)**  
**[Commit: [turbofan] Serialize more prototypes.](https://chromium.googlesource.com/v8/v8/+/312dbdd)**  
  
Date(Commit): Wed Nov 14 09:13:25 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1331478](https://chromium-review.googlesource.com/c/1331478)  
Regress: [mjsunit/regress/regress-904417.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-904417.js)  
```javascript
function bar(o) {
  return o.hello, Object.getPrototypeOf(o);
};
%PrepareFunctionForOptimization(bar);
var y = { __proto__: {}, hello: 44 };
var z = { hello: 45 };

bar(y);
bar(z);
bar(y);
%OptimizeFunctionOnNextCall(bar);
bar(y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/312dbdd^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=312dbdd)  
[test/mjsunit/regress/regress-904417.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-904417.js?cl=312dbdd)  
  

---   

## **regress-904707.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::TypedElementsAccessor<](https://crbug.com/904707)**  
**[Commit: [typed-array] Fix CopyElements.](https://chromium.googlesource.com/v8/v8/+/04af85c)**  
  
Date(Commit): Tue Nov 13 11:47:00 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1333409](https://chromium-review.googlesource.com/c/1333409)  
Regress: [mjsunit/regress/regress-904707.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-904707.js)  
```javascript
delete Float64Array.prototype.__proto__[Symbol.iterator];

let a = new Float64Array(9);
Object.defineProperty(a, "length", {
  get: function () { %ArrayBufferDetach(a.buffer); return 6; }
});

Float64Array.from(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/04af85c^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=04af85c)  
[test/mjsunit/regress/regress-904707.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-904707.js?cl=04af85c)  
  

---   

## **regress-8449.js (v8 issue)**  
   
**[Issue: Array iteration should read through holes](https://crbug.com/v8/8449)**  
**[Commit: Fix ArrayIteratorPrototypeNext for holes.](https://chromium.googlesource.com/v8/v8/+/a377c9a)**  
  
Date(Commit): Tue Nov 13 10:09:31 2018  
Code Review: [https://chromium-review.googlesource.com/c/1332232](https://chromium-review.googlesource.com/c/1332232)  
Regress: [mjsunit/regress/regress-8449.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8449.js)  
```javascript
{
  const x = [, 1];
  x.__proto__ = [42];
  const y = [...x];
  assertEquals([42, 1], y);
  assertTrue(y.hasOwnProperty(0));
}

{
  const x = [, 1];
  x.__proto__ = [42];
  assertEquals(42, x[Symbol.iterator]().next().value);
}

{
  const array_prototype = [].__proto__;
  array_prototype[0] = 42;
  const x = [, 1];
  assertEquals(42, x[Symbol.iterator]().next().value);
  delete array_prototype[0];
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a377c9a^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=a377c9a)  
[test/mjsunit/regress/regress-8449.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8449.js?cl=a377c9a)  
  

---   

## **regress-crbug-902672.js (chromium issue)**  
   
**[Issue: CSA_ASSERT in Array.p.join](https://crbug.com/902672)**  
**[Commit: [builtin] Array.p.join throws on invalid Array lengths.](https://chromium.googlesource.com/v8/v8/+/0dd0af7)**  
  
Date(Commit): Tue Nov 13 09:46:01 2018  
Components: Blink>JavaScript  
Labels: reward-0, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, M-72, Via-Wizard-Security  
Code Review: [https://chromium-review.googlesource.com/c/1330921](https://chromium-review.googlesource.com/c/1330921)  
Regress: [mjsunit/regress/regress-crbug-902672.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902672.js)  
```javascript
var a = this;
var b = {};
a.length = 4294967296; // 2 ^ 32 (max array length + 1)
assertThrows(() => Array.prototype.join.call(a,b), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0dd0af7^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=0dd0af7)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=0dd0af7)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=0dd0af7)  
[test/mjsunit/regress/regress-crbug-902672.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-902672.js?cl=0dd0af7)  
  

---   

## **regress-904255.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::interpreter::BytecodeArrayBuilder::Local](https://crbug.com/904255)**  
**[Commit: [parser] Restore reparenting of temporaries](https://chromium.googlesource.com/v8/v8/+/4235fc0)**  
  
Date(Commit): Mon Nov 12 09:44:56 2018  
Components: Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1329685](https://chromium-review.googlesource.com/c/1329685)  
Regress: [mjsunit/regress/regress-904255.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-904255.js)  
```javascript
assertThrows("((__v_0 = __v_0.replace(...new Array(), '0').slice(...new Int32Array(), '0')) => print())()", ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4235fc0^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=4235fc0)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=4235fc0)  
[test/mjsunit/regress/regress-904255.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-904255.js?cl=4235fc0)  
  

---   

## **regress-903527.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::Literal::ToBooleanIsTrue](https://crbug.com/903527)**  
**[Commit: [parser] Cook invalid template literals if we've thrown](https://chromium.googlesource.com/v8/v8/+/65ab5bb)**  
  
Date(Commit): Mon Nov 12 09:34:22 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/1329691](https://chromium-review.googlesource.com/c/1329691)  
Regress: [mjsunit/regress/regress-903527.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-903527.js)  
```javascript
assertThrows("e*!`\\2`", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65ab5bb^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=65ab5bb)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=65ab5bb)  
[test/mjsunit/regress/regress-903527.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-903527.js?cl=65ab5bb)  
  

---   

## **regress-904275.js (chromium issue)**  
   
**[Issue: Unreachable code in ast-traversal-visitor.h](https://crbug.com/904275)**  
**[Commit: [parser] Don't reindex function literals if there's a parser error](https://chromium.googlesource.com/v8/v8/+/cdae5af)**  
  
Date(Commit): Mon Nov 12 09:16:50 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1329686](https://chromium-review.googlesource.com/c/1329686)  
Regress: [mjsunit/regress/regress-904275.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-904275.js)  
```javascript
function __isPropertyOfType(obj, name) {
    Object.getOwnPropertyDescriptor(obj, name)
}
function __getProperties(obj, type) {
  for (let name of Object.getOwnPropertyNames(obj)) {
    __isPropertyOfType(obj, name);
  }
}
function __getRandomProperty(obj) {
  let properties = __getProperties(obj);
}
function __f_6776(__v_33890, __v_33891) {
  var __v_33896 = __v_33891();
 __getRandomProperty([])
}
(function __f_6777() {
  var __v_33906 = async () => { };
    __f_6776(1, () => __v_33906())
})();
(function __f_6822() {
  try {
    __f_6776(1, () => __f_6822());
  } catch (e) {}
  var __v_34059 = async (__v_34079 = () => eval()) => { };
    delete __v_34059[__getRandomProperty(__v_34059)];
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cdae5af^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=cdae5af)  
[test/mjsunit/regress/regress-904275.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-904275.js?cl=cdae5af)  
  

---   

## **regress-903874.js (chromium issue)**  
   
**[Issue: Stack-overflow in v8::internal::ParserBase<v8::internal::PreParser>::ParseObjectPropertyDefinition](https://crbug.com/903874)**  
**[Commit: [parser] Check stackoverflow in ParseBindingPattern](https://chromium.googlesource.com/v8/v8/+/bc53445)**  
  
Date(Commit): Mon Nov 12 09:15:45 2018  
Components: Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Needs-Feedback, Clusterfuzz, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/1329688](https://chromium-review.googlesource.com/c/1329688)  
Regress: [mjsunit/regress/regress-903874.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-903874.js)  
```javascript
var code = "function f(" + ("{o(".repeat(10000));
assertThrows(code, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc53445^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=bc53445)  
[test/message/regress/fail/regress-903874.js](https://cs.chromium.org/chromium/src/v8/test/message/regress/fail/regress-903874.js?cl=bc53445)  
[test/message/regress/fail/regress-903874.out](https://cs.chromium.org/chromium/src/v8/test/message/regress/fail/regress-903874.out?cl=bc53445)  
  

---   

## **regress-903697.js (chromium issue)**  
   
**[Issue: CHECK failure: heap_->Contains(object) in heap.cc](https://crbug.com/903697)**  
**[Commit: [turbofan] Install code dependencies atomically.](https://chromium.googlesource.com/v8/v8/+/5751278)**  
  
Date(Commit): Mon Nov 12 08:27:51 2018  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1329162](https://chromium-review.googlesource.com/c/1329162)  
Regress: [mjsunit/regress/regress-903697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-903697.js)  
```javascript
function f() {
  C = class {};
  for (var i = 0; i < 5; ++i) {
    gc();
    if (i == 2) %OptimizeOsr();
    C.prototype.foo = i + 9000000000000000;
  }
}
%PrepareFunctionForOptimization(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5751278^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=5751278)  
[test/mjsunit/regress/regress-903697.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-903697.js?cl=5751278)  
  

---   

## **regress-896326.js (chromium issue)**  
   
**[Issue: Crash in MemoryWrite<unsigned](https://crbug.com/896326)**  
**[Commit: Check for stack overflow when pushing arguments in JSConstructStubGeneric](https://chromium.googlesource.com/v8/v8/+/d056294)**  
  
Date(Commit): Fri Nov 09 14:56:51 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-70, M-70, merge-merged-7.1, Release-0-M71  
Code Review: [https://chromium-review.googlesource.com/c/1305934](https://chromium-review.googlesource.com/c/1305934)  
Regress: [mjsunit/regress/regress-896326.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-896326.js)  
```javascript
function f() {
}

var large_array = Array(150 * 1024);
assertThrows('new f(... large_array)', RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d056294^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=d056294)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=d056294)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=d056294)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=d056294)  
[test/mjsunit/regress/regress-896326.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-896326.js?cl=d056294)  
  

---   

## **regress-crbug-903043.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/903043)**  
**[Commit: [turbofan] Fix -0 check for subnormals.](https://chromium.googlesource.com/v8/v8/+/56f6a76)**  
  
Date(Commit): Fri Nov 09 12:04:30 2018  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1328802](https://chromium-review.googlesource.com/c/1328802)  
Regress: [mjsunit/regress/regress-crbug-903043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-903043.js)  
```javascript
(function() {
function foo() {
  const x = 1e-1;
  return Object.is(-0, x * -1e-308);
};
%PrepareFunctionForOptimization(foo);
assertFalse(foo());
assertFalse(foo());
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo());
})();

(function() {
function foo(x) {
  return Object.is(-0, x * -1e-308);
};
%PrepareFunctionForOptimization(foo);
assertFalse(foo(1e-1));
assertFalse(foo(1e-1));
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo(1e-1));
})();

(function() {
function foo(x) {
  return Object.is(-0, x);
};
%PrepareFunctionForOptimization(foo);
assertFalse(foo(1e-1 * -1e-308));
assertFalse(foo(1e-1 * -1e-308));
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo(1e-1 * -1e-308));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56f6a76^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=56f6a76)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=56f6a76)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=56f6a76)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=56f6a76)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=56f6a76)  
...  
  

---   

## **regress-903070.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: IsStrong(object)](https://crbug.com/903070)**  
**[Commit: [CloneObjectIC] clone MutableHeapNumbers only if !FLAG_unbox_double_fields](https://chromium.googlesource.com/v8/v8/+/3e010af)**  
  
Date(Commit): Thu Nov 08 19:14:11 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1323911](https://chromium-review.googlesource.com/c/1323911)  
Regress: [mjsunit/es9/regress/regress-903070.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-903070.js)  
```javascript
function clone(src) {
  return { ...src };
}

function inobjectDoubles() {
  "use strict";
  this.p0 = -6400510997704731;
}

assertEquals({ p0: -6400510997704731 }, clone(new inobjectDoubles()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e010af^!)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=3e010af)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=3e010af)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=3e010af)  
[test/mjsunit/es9/regress/regress-902965.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-902965.js?cl=3e010af)  
[test/mjsunit/es9/regress/regress-903070.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-903070.js?cl=3e010af)  
  

---   

## **regress-902965.js (chromium issue)**  
   
**[Issue: Null-dereference READ in Builtins_CloneObjectIC](https://crbug.com/902965)**  
**[Commit: [CloneObjectIC] clone MutableHeapNumbers only if !FLAG_unbox_double_fields](https://chromium.googlesource.com/v8/v8/+/3e010af)**  
  
Date(Commit): Thu Nov 08 19:14:11 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1323911](https://chromium-review.googlesource.com/c/1323911)  
Regress: [mjsunit/es9/regress/regress-902965.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-902965.js)  
```javascript
function inobjectDouble() {
  "use strict";
  this.x = -3.9;
}
const instance = new inobjectDouble();
const clone = { ...instance, };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e010af^!)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=3e010af)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=3e010af)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=3e010af)  
[test/mjsunit/es9/regress/regress-902965.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-902965.js?cl=3e010af)  
[test/mjsunit/es9/regress/regress-903070.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-903070.js?cl=3e010af)  
  

---   

## **regress-crbug-902610.js (chromium issue)**  
   
**[Issue: Crash in Builtins_MovExtraWideHandler](https://crbug.com/902610)**  
**[Commit: [parser] Fix off-by-one in parameter count check](https://chromium.googlesource.com/v8/v8/+/36e1e46)**  
  
Date(Commit): Thu Nov 08 14:52:30 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1326029](https://chromium-review.googlesource.com/c/1326029)  
Regress: [mjsunit/regress/regress-crbug-902610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902610.js)  
```javascript
assertThrows(() => {
  // Make a function with 65535 args. This should throw a SyntaxError because -1
  // is reserved for the "don't adapt arguments" sentinel.
  var f_with_65535_args =
      eval("(function(" + Array(65535).fill("x").join(",") + "){})");
  f_with_65535_args();
}, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/36e1e46^!)  
[src/message-template.h](https://cs.chromium.org/chromium/src/v8/src/message-template.h?cl=36e1e46)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=36e1e46)  
[test/mjsunit/regress/regress-crbug-902610.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-902610.js?cl=36e1e46)  
  

---   

## **regress-902810.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::Isolate::PushStackTraceAndDie](https://crbug.com/902810)**  
**[Commit: [parser] Fix cover-grammar initializer positions](https://chromium.googlesource.com/v8/v8/+/5bf9e47)**  
  
Date(Commit): Thu Nov 08 14:42:35 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1326022](https://chromium-review.googlesource.com/c/1326022)  
Regress: [mjsunit/regress/regress-902810.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-902810.js)  
```javascript
assertThrows("((__v_4 = __v_4, __v_0) => eval(__v_4))()", ReferenceError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bf9e47^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=5bf9e47)  
[test/mjsunit/regress/regress-902810.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-902810.js?cl=5bf9e47)  
  

---   

## **regress-crbug-902395.js (chromium issue)**  
   
**[Issue: Security: bytecode-graph-builder values_[index] != builder()->jsgraph()->OptimizedOutConstant()](https://crbug.com/902395)**  
**[Commit: [ignition] More accurate dead statement elision](https://chromium.googlesource.com/v8/v8/+/7412593)**  
  
Date(Commit): Thu Nov 08 10:48:09 2018  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>Interpreter  
Labels: reward-0, Security_Impact-Head, Security_Severity-High, allpublic, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/c/1322951](https://chromium-review.googlesource.com/c/1322951)  
Regress: [mjsunit/regress/regress-crbug-902395.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902395.js)  
```javascript
function opt() {
  try {
    Object.seal({});
  } finally {
    try {
      // Carefully crafted by clusterfuzz to alias the temporary object literal
      // register with the below dead try block's context register.
      ({toString() {}})
          .

          apply(-1)
          .x();
    } finally {
      if (2.2) {
        return;
      }
      // This code should be dead.
      try {
        Reflect.construct;
      } finally {
      }
    }
  }
};
%PrepareFunctionForOptimization(opt);
opt();
%OptimizeFunctionOnNextCall(opt);
opt();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7412593^!)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=7412593)  
[src/interpreter/bytecode-array-builder.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.h?cl=7412593)  
[src/interpreter/bytecode-array-writer.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-writer.h?cl=7412593)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=7412593)  
[test/mjsunit/regress/regress-crbug-902395.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-902395.js?cl=7412593)  
  

---   

## **regress-902608.js (chromium issue)**  
   
**[Issue: Crash in GetValueByObjectIndex](https://crbug.com/902608)**  
**[Commit: [interpreter] Store CreateObjectLiteral's result into the accumulator.](https://chromium.googlesource.com/v8/v8/+/60c0edc)**  
  
Date(Commit): Thu Nov 08 10:31:45 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1325901](https://chromium-review.googlesource.com/c/1325901)  
Regress: [mjsunit/compiler/regress-902608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-902608.js)  
```javascript
async function f() {
  var a = [...new Int8Array([, ...new Uint8Array(65536)])];
  var p = new Proxy([f], {
    set: function () { },
    done: undefined.prototype
  });
}

f()
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/60c0edc^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=60c0edc)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=60c0edc)  
[src/interpreter/bytecode-array-builder.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.h?cl=60c0edc)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=60c0edc)  
[src/interpreter/bytecodes.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecodes.h?cl=60c0edc)  
...  
  

---   

## **regress-902552.js (chromium issue)**  
   
**[Issue: DCHECK failure in AllowCodeDependencyChange::IsAllowed() in objects.cc](https://crbug.com/902552)**  
**[Commit: Allow code-dependency changes in OptimizedCompilationJob::FinalizeJob](https://chromium.googlesource.com/v8/v8/+/f460315)**  
  
Date(Commit): Thu Nov 08 08:46:44 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1322450](https://chromium-review.googlesource.com/c/1322450)  
Regress: [mjsunit/regress/regress-902552.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-902552.js)  
```javascript
function f() {
  var C = class {};
  for (var i = 0; i < 4; ++i) {
    if (i == 2) %OptimizeOsr();
    C.prototype.foo = 42;
  }
}
%PrepareFunctionForOptimization(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f460315^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=f460315)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=f460315)  
[test/mjsunit/regress/regress-902552.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-902552.js?cl=f460315)  
  

---   

## **regress-901798.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/901798)**  
**[Commit: [turbofan] Don't loose checked Uint32 -> Int32 conversion](https://chromium.googlesource.com/v8/v8/+/201a0c6)**  
  
Date(Commit): Tue Nov 06 15:16:48 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1319760](https://chromium-review.googlesource.com/c/1319760)  
Regress: [mjsunit/regress/regress-901798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-901798.js)  
```javascript
function f(a) {
  return (a >>> 1073741824) + -3;
};
%PrepareFunctionForOptimization(f);
assertEquals(-3, f(0));
assertEquals(-2, f(1));
%OptimizeFunctionOnNextCall(f);
assertEquals(4294967291, f(-2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/201a0c6^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=201a0c6)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=201a0c6)  
[test/mjsunit/regress/regress-901798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-901798.js?cl=201a0c6)  
  

---   

## **regress-901633.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Torque assert 'srcPos <= GetReceiverLengthProperty(sortState)](https://crbug.com/901633)**  
**[Commit: [array] Weaken bounds checks in Array.p.sort](https://chromium.googlesource.com/v8/v8/+/1444beb)**  
  
Date(Commit): Tue Nov 06 14:04:38 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1317814](https://chromium-review.googlesource.com/c/1317814)  
Regress: [mjsunit/regress/regress-901633.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-901633.js)  
```javascript
const magic0 = 2396;
const magic1 = 1972;

const xs = [];
for (let j = 0; j < magic0; ++j) {
  xs[j] = [j + 0.1];
}

let cmp_calls = 0;
xs.sort((lhs, rhs) => {
  lhs = lhs || [0];
  rhs = rhs || [0];
  if (cmp_calls++ == magic1) xs.length = 1;
  return lhs[0] - rhs[0];
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1444beb^!)  
[test/mjsunit/regress/regress-901633.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-901633.js?cl=1444beb)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=1444beb)  
  

---   

## **regress-directive.js (other issue)**  
   
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
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=9d34fa0)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=9d34fa0)  
[src/parsing/scanner.h](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.h?cl=9d34fa0)  
[test/message/fail/directive.js](https://cs.chromium.org/chromium/src/v8/test/message/fail/directive.js?cl=9d34fa0)  
[test/message/fail/directive.out](https://cs.chromium.org/chromium/src/v8/test/message/fail/directive.out?cl=9d34fa0)  
...  
  
  
---   

## **regress-900786.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::FunctionLiteral::kind](https://crbug.com/900786)**  
**[Commit: [parser] Simplify Scope::DeclareVariable](https://chromium.googlesource.com/v8/v8/+/9884930)**  
  
Date(Commit): Fri Nov 02 10:27:23 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1314331](https://chromium-review.googlesource.com/c/1314331)  
Regress: [mjsunit/regress/regress-900786.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900786.js)  
```javascript
assertThrows("{function g(){}function g(){+", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9884930^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=9884930)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=9884930)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=9884930)  
[src/ast/variables.h](https://cs.chromium.org/chromium/src/v8/src/ast/variables.h?cl=9884930)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=9884930)  
...  
  

---   

## **regress-crbug-898785.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: SmiBelow(effective_index, LoadFixedArrayBaseLength(array))](https://crbug.com/898785)**  
**[Commit: [builtins] Fix out-of-bounds in Array#lastIndexOf().](https://chromium.googlesource.com/v8/v8/+/b8a9113)**  
  
Date(Commit): Fri Nov 02 07:42:50 2018  
Components: Blink>JavaScript>Runtime  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1314329](https://chromium-review.googlesource.com/c/1314329)  
Regress: [mjsunit/regress/regress-crbug-898785.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-898785.js)  
```javascript
var a = [0, 1];
var o = { [Symbol.toPrimitive]() { a.length = 1; return 2; } };

a.push(2);
a.lastIndexOf(5, o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8a9113^!)  
[src/builtins/array-lastindexof.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-lastindexof.tq?cl=b8a9113)  
[test/mjsunit/regress/regress-crbug-898785.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-898785.js?cl=b8a9113)  
  

---   

## **regress-900585.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::PatternRewriter::VisitArrayLiteral](https://crbug.com/900585)**  
**[Commit: [parser] Don't rewrite if we're in error state](https://chromium.googlesource.com/v8/v8/+/9bd6e60)**  
  
Date(Commit): Wed Oct 31 18:39:42 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1310294](https://chromium-review.googlesource.com/c/1310294)  
Regress: [mjsunit/regress/regress-900585.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900585.js)  
```javascript
assertThrows("/*for..in*/for(var [x5, functional] = this = function(id) { return id } in false) var x2, x;", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bd6e60^!)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=9bd6e60)  
[test/mjsunit/regress/regress-900585.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-900585.js?cl=9bd6e60)  
  

---   

## **regress-8384.js (v8 issue)**  
   
**[Issue: TryMatchLoadWord64AndShiftRight causes invalid load/store reordering](https://crbug.com/v8/8384)**  
**[Commit: [instruction-selector-x64] Add missing CanCover check](https://chromium.googlesource.com/v8/v8/+/4dff27e)**  
  
Date(Commit): Wed Oct 31 08:08:40 2018  
Code Review: [https://chromium-review.googlesource.com/c/1307419](https://chromium-review.googlesource.com/c/1307419)  
Regress: [mjsunit/regress/regress-8384.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8384.js)  
```javascript
function assert(cond) {
  if (!cond) throw 'Assert';
}

function Constructor() {
  this.padding1 = null;
  this.padding2 = null;
  this.padding3 = null;
  this.padding4 = null;
  this.padding5 = null;
  this.padding6 = null;
  this.padding7 = null;
  this.padding8 = null;
  this.padding9 = null;
  this.padding10 = null;
  this.padding11 = null;
  this.padding12 = null;
  this.padding13 = null;
  this.padding14 = null;
  this.padding15 = null;
  this.padding16 = null;
  this.padding17 = null;
  this.padding18 = null;
  this.padding19 = null;
  this.padding20 = null;
  this.padding21 = null;
  this.padding22 = null;
  this.padding23 = null;
  this.padding24 = null;
  this.padding25 = null;
  this.padding26 = null;
  this.padding27 = null;
  this.padding28 = null;
  this.padding29 = null;
  this.array = null;
  this.accumulator = 0;
}

function f(k) {
  var c = k.accumulator | 0;
  k.accumulator = k.array[k.accumulator + 1 | 0] | 0;
  k.array[c + 1 | 0] = -1;
  var head = k.accumulator;
  assert(head + c & 1);
  while (head >= 0) {
    head = k.array[head + 1 | 0];
  }
  return;
};
%PrepareFunctionForOptimization(f);
const tmp = new Constructor();
tmp.array = new Int32Array(5);
for (var i = 1; i < 5; i++) tmp.array[i] = i | 0;
tmp.accumulator = 0;

f(tmp);
f(tmp);
%OptimizeFunctionOnNextCall(f);
f(tmp);  // This must not trigger the {assert}.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4dff27e^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=4dff27e)  
[src/compiler/instruction-selector.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.h?cl=4dff27e)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=4dff27e)  
[test/mjsunit/regress/regress-8384.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8384.js?cl=4dff27e)  
  

---   

## **regress-899537.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::interpreter::BytecodeGenerator::BuildVariableAssignment](https://crbug.com/899537)**  
**[Commit: [class] Rewrite destructuring assignment in class field initializers](https://chromium.googlesource.com/v8/v8/+/c65dbd5)**  
  
Date(Commit): Tue Oct 30 16:34:04 2018  
Components: Blink>JavaScript>Interpreter  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, Stability-Libfuzzer, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1304538](https://chromium-review.googlesource.com/c/1304538)  
Regress: [mjsunit/regress/regress-899537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-899537.js)  
```javascript
[ class  { c = [ c ] = c } ]  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c65dbd5^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=c65dbd5)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=c65dbd5)  
[test/mjsunit/regress/regress-899537.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-899537.js?cl=c65dbd5)  
  

---   

## **regress-898932.js (chromium issue)**  
   
**[Issue: WebAssembly app start-up is slower with Liftoff](https://crbug.com/898932)**  
**[Commit: [wasm] Fix memory limit checks](https://chromium.googlesource.com/v8/v8/+/fac176d)**  
  
Date(Commit): Tue Oct 30 13:44:48 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/1305274](https://chromium-review.googlesource.com/c/1305274)  
Regress: [mjsunit/regress/wasm/regress-898932.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-898932.js)  
```javascript
let mem = new WebAssembly.Memory({initial: 1});
try {
  mem.grow(49151);
} catch (e) {
  // This can fail on 32-bit systems if we cannot make such a big reservation.
  if (!(e instanceof RangeError)) throw e;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fac176d^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=fac176d)  
[src/wasm/compilation-environment.h](https://cs.chromium.org/chromium/src/v8/src/wasm/compilation-environment.h?cl=fac176d)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=fac176d)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=fac176d)  
[src/wasm/wasm-engine.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-engine.cc?cl=fac176d)  
...  
  

---   

## **regress-8380.js (v8 issue)**  
   
**[Issue: Assertion failure during one-line array swap](https://crbug.com/v8/8380)**  
**[Commit: Fix typing of binary operators on BigInt.](https://chromium.googlesource.com/v8/v8/+/c5c6b8b)**  
  
Date(Commit): Tue Oct 30 13:33:55 2018  
Code Review: [https://chromium-review.googlesource.com/c/1307418](https://chromium-review.googlesource.com/c/1307418)  
Regress: [mjsunit/compiler/regress-8380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-8380.js)  
```javascript
function reduceLHS() {
  for (var i = 0; i < 2 ;i++) {
    let [q, r] = [1n, 1n];
    r = r - 1n;
    q += 1n;
    q = r;
  }
}

%PrepareFunctionForOptimization(reduceLHS);
reduceLHS();
%OptimizeFunctionOnNextCall(reduceLHS);
reduceLHS();


function reduceRHS() {
  for (var i = 0; i < 2 ;i++) {
    let [q, r] = [1n, 1n];
    r = 1n - r;
    q += 1n;
    q = r;
  }
}

%PrepareFunctionForOptimization(reduceRHS);
reduceRHS();
%OptimizeFunctionOnNextCall(reduceRHS);
reduceRHS();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c5c6b8b^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=c5c6b8b)  
[test/mjsunit/compiler/regress-8380.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-8380.js?cl=c5c6b8b)  
  

---   

## **regress-900085.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::ParserBase<v8::internal::Parser>::ParseForAwaitStatement](https://crbug.com/900085)**  
**[Commit: [parser] Restore RETURN_IF_PARSE_ERROR in for/await](https://chromium.googlesource.com/v8/v8/+/e0c6671)**  
  
Date(Commit): Tue Oct 30 10:11:00 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1306438](https://chromium-review.googlesource.com/c/1306438)  
Regress: [mjsunit/regress/regress-900085.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900085.js)  
```javascript
assertThrows(
    "async function f() { let v = 1; for await (var v of {}) { }",
    SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0c6671^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=e0c6671)  
[test/mjsunit/regress/regress-900085.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-900085.js?cl=e0c6671)  
  

---   

## **regress-crbug-899535.js (chromium issue)**  
   
**[Issue: Check failed: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray())](https://crbug.com/899535)**  
**[Commit: [elements] fix wrong cast of empty FixedArray in Array.prototype.includes](https://chromium.googlesource.com/v8/v8/+/f942791)**  
  
Date(Commit): Mon Oct 29 20:37:03 2018  
Components: Blink>JavaScript  
Labels: ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/c/1304296](https://chromium-review.googlesource.com/c/1304296)  
Regress: [mjsunit/regress/regress-crbug-899535.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899535.js)  
```javascript
let a = [1.1, 2.2, 3.3];
a.includes(4.4, { toString: () => a.length = 0 });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f942791^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=f942791)  
[test/mjsunit/es7/array-includes.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/array-includes.js?cl=f942791)  
[test/mjsunit/regress/regress-crbug-899535.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899535.js?cl=f942791)  
  

---   

## **regress-8377.js (v8 issue)**  
   
**[Issue: Invalid asm.js: Expected ;](https://crbug.com/v8/8377)**  
**[Commit: [asm.js] Fix fall-back case in MultiplicativeExpression.](https://chromium.googlesource.com/v8/v8/+/9195ca9)**  
  
Date(Commit): Mon Oct 29 12:59:01 2018  
Code Review: [https://chromium-review.googlesource.com/c/1304317](https://chromium-review.googlesource.com/c/1304317)  
Regress: [mjsunit/regress/regress-8377.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8377.js)  
```javascript
function Module(global, env, buffer) {
  "use asm";
  function test1() {
    var x = 0;
    x = -1 / 1 | 0;
    return x | 0;
  }
  function test2() {
    var x = 0;
    x = (-1 / 1) | 0;
    return x | 0;
  }
  return { test1: test1, test2: test2 };
};
let module = Module(this);
assertEquals(-1, module.test1());
assertEquals(-1, module.test2());
assertTrue(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9195ca9^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=9195ca9)  
[test/mjsunit/regress/regress-8377.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8377.js?cl=9195ca9)  
  

---   

## **regress-crbug-899464.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Word32Or(Word32Equal(var_unicode.value(), zero), Word32Equal(](https://crbug.com/899464)**  
**[Commit: [regexp] Ensure FastFlagGetter returns either 0 or 1](https://chromium.googlesource.com/v8/v8/+/6397149)**  
  
Date(Commit): Mon Oct 29 09:54:43 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1304315](https://chromium-review.googlesource.com/c/1304315)  
Regress: [mjsunit/regress/regress-crbug-899464.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899464.js)  
```javascript
''.matchAll(/./ug);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6397149^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=6397149)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=6397149)  
[src/objects/js-regexp.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-regexp.h?cl=6397149)  
[test/mjsunit/regress/regress-crbug-899464.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899464.js?cl=6397149)  
  

---   

## **regress-crbug-899524.js (chromium issue)**  
   
**[Issue: CHECK failure: JSNegate of kRepTagged (Numeric) cannot be changed to kRepWord32 in representati](https://crbug.com/899524)**  
**[Commit: [turbofan] Fix LoadElement with variable index scalar replacement.](https://chromium.googlesource.com/v8/v8/+/104d752)**  
  
Date(Commit): Mon Oct 29 09:38:23 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-71, Target-71, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1303721](https://chromium-review.googlesource.com/c/1303721)  
Regress: [mjsunit/regress/regress-crbug-899524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899524.js)  
```javascript
function empty() {}

function baz(expected, found) {
  var start = "";
  found.length, start + 'x';
  if (expected.length === found.length) {
    for (var i = 0; i < expected.length; ++i) {
      empty(found[i]);
    }
  }
}

baz([1], new class A extends Array {}());

(function () {
  "use strict";
  function bar() {
    baz([1, 2], arguments);
  }
  function foo() {
    bar(2147483648, -[]);
  };
  %PrepareFunctionForOptimization(foo);
  foo();
  foo();
  %OptimizeFunctionOnNextCall(foo);
  foo();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/104d752^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=104d752)  
[test/mjsunit/regress/regress-crbug-899524.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899524.js?cl=104d752)  
  

---   

## **regress-899474.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::AstNode::position](https://crbug.com/899474)**  
**[Commit: [parser] Only throw spread class property error if it's the first error](https://chromium.googlesource.com/v8/v8/+/dc70cb6)**  
  
Date(Commit): Mon Oct 29 09:26:04 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Libfuzzer, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1304314](https://chromium-review.googlesource.com/c/1304314)  
Regress: [mjsunit/regress/regress-899474.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-899474.js)  
```javascript
assertThrows("class A {...", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc70cb6^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=dc70cb6)  
[test/mjsunit/regress/regress-899474.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-899474.js?cl=dc70cb6)  
  

---   

## **regress-899133.js (chromium issue)**  
   
**[Issue: DCHECK failure in success in pattern-rewriter.cc](https://crbug.com/899133)**  
**[Commit: [parser] Temporarily restore RETURN_IF_PARSE_ERROR guarding DCHECK](https://chromium.googlesource.com/v8/v8/+/da024b5)**  
  
Date(Commit): Fri Oct 26 16:43:57 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1301482](https://chromium-review.googlesource.com/c/1301482)  
Regress: [mjsunit/regress/regress-899133.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-899133.js)  
```javascript
assertThrows("let fun = ({a} = {a: 30}) => {", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da024b5^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=da024b5)  
[test/mjsunit/regress/regress-899133.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-899133.js?cl=da024b5)  
  

---   

## **regress-v8-8357.js (v8 issue)**  
   
**[Issue: MaybeCallFunctionAtSymbol spec violation](https://crbug.com/v8/8357)**  
**[Commit: [string] Remove invalid optimization in MaybeCallFunctionAtSymbol](https://chromium.googlesource.com/v8/v8/+/6f08b64)**  
  
Date(Commit): Fri Oct 26 14:39:57 2018  
Code Review: [https://chromium-review.googlesource.com/c/1301498](https://chromium-review.googlesource.com/c/1301498)  
Regress: [mjsunit/regress/regress-v8-8357.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-8357.js)  
```javascript
const s = "Umbridge has been reading your mail, Harry."

{
  let monkey_called = false;
  s.__proto__.__proto__[Symbol.replace] =
    () => { monkey_called = true; };
  s.replace(s);
  assertTrue(monkey_called);
}

{
  let monkey_called = false;
  s.__proto__.__proto__[Symbol.search] =
    () => { monkey_called = true; };
  s.search(s);
  assertTrue(monkey_called);
}

{
  let monkey_called = false;
  s.__proto__.__proto__[Symbol.match] =
    () => { monkey_called = true; };
  s.match(s);
  assertTrue(monkey_called);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f08b64^!)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=6f08b64)  
[test/mjsunit/regress/regress-v8-8357.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-8357.js?cl=6f08b64)  
  

---   

## **regress-899115.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::MapRef::prototype](https://crbug.com/899115)**  
**[Commit: [turbofan] Serialize receiver prototypes more often.](https://chromium.googlesource.com/v8/v8/+/cd629c0)**  
  
Date(Commit): Fri Oct 26 14:10:45 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1301475](https://chromium-review.googlesource.com/c/1301475)  
Regress: [mjsunit/regress/regress-899115.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-899115.js)  
```javascript
function foo() {
  Object.getPrototypeOf([]).includes();
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd629c0^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=cd629c0)  
[test/mjsunit/regress/regress-899115.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-899115.js?cl=cd629c0)  
  

---   

## **regress-crbug-898974.js (chromium issue)**  
   
**[Issue: CHECK failure: !unit.failed() in module-compiler.cc](https://crbug.com/898974)**  
**[Commit: [asm.js] Fix storing float32 value into float64 heap view.](https://chromium.googlesource.com/v8/v8/+/545fa6e)**  
  
Date(Commit): Fri Oct 26 11:33:23 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-None, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1301499](https://chromium-review.googlesource.com/c/1301499)  
Regress: [mjsunit/regress/regress-crbug-898974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-898974.js)  
```javascript
function Module(global, env, buffer) {
  "use asm";
  var HEAPF64 = new global.Float64Array(buffer);
  var HEAPF32 = new global.Float32Array(buffer);
  var Math_fround = global.Math.fround;
  function main_d_f() {
    HEAPF64[0] = Math_fround(+HEAPF64[0]);
  }
  function main_d_fq() {
    HEAPF64[1] = HEAPF32[4096];
  }
  function main_f_dq() {
    HEAPF32[4] = HEAPF64[4096];
  }
  return {main_d_f: main_d_f, main_d_fq: main_d_fq, main_f_dq: main_f_dq};
};
let buffer = new ArrayBuffer(4096);
let module = Module(this, undefined, buffer);
let view64 = new Float64Array(buffer);
let view32 = new Float32Array(buffer);
assertEquals(view64[0] = 2.3, view64[0]);
module.main_d_f();
module.main_d_fq();
module.main_f_dq();
assertTrue(%IsAsmWasmCode(Module));
assertEquals(Math.fround(2.3), view64[0]);
assertTrue(isNaN(view64[1]));
assertTrue(isNaN(view32[4]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/545fa6e^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=545fa6e)  
[test/mjsunit/regress/regress-crbug-898974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-898974.js?cl=545fa6e)  
  

---   

## **regress-898936.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_async implies classifier()->is_valid_async_arrow_formal_parameters() in parse](https://crbug.com/898936)**  
**[Commit: [parser] Only validate async params of valid arrow functions](https://chromium.googlesource.com/v8/v8/+/69f370b)**  
  
Date(Commit): Fri Oct 26 07:55:49 2018  
Components: Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Stability-Libfuzzer, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1300133](https://chromium-review.googlesource.com/c/1300133)  
Regress: [mjsunit/regress/regress-898936.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-898936.js)  
```javascript
assertThrows("async(...x=e)()=>");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/69f370b^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=69f370b)  
[test/mjsunit/regress/regress-898936.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-898936.js?cl=69f370b)  
  

---   

## **regress-898812.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::FuncNameInferrer::RemoveAsyncKeywordFromEnd](https://crbug.com/898812)**  
**[Commit: [parser] Only parse async parenthesized arrow if current_token == ASYNC](https://chromium.googlesource.com/v8/v8/+/1efaf46)**  
  
Date(Commit): Fri Oct 26 07:54:44 2018  
Components: Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Stability-AFL, Test-Predator-Auto-CC, Test-Predator-Auto-Components, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1300134](https://chromium-review.googlesource.com/c/1300134)  
Regress: [mjsunit/regress/regress-898812.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-898812.js)  
```javascript
assertThrows("(async)(a)=>{}", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1efaf46^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=1efaf46)  
[test/mjsunit/regress/regress-898812.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-898812.js?cl=1efaf46)  
  

---   

## **regress-897512.js (chromium issue)**  
   
**[Issue: Security:  assert 'srcPos <= GetReceiverLengthProperty(sortState) - length' at array-sort.tq:613:](https://crbug.com/897512)**  
**[Commit: [array] Ensure PrepareElementsForSort returns a legal value](https://chromium.googlesource.com/v8/v8/+/0855fb1)**  
  
Date(Commit): Thu Oct 25 12:02:47 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reward-1000, Security_Impact-Stable, Security_Severity-Medium, allpublic, reward-inprocess, CVE_description-submitted, M-71, Target-71, merge-merged-7.0, merge-merged-7.1, Release-2-M70, CVE-2018-17478  
Code Review: [https://chromium-review.googlesource.com/c/1297958](https://chromium-review.googlesource.com/c/1297958)  
Regress: [mjsunit/regress/regress-897512.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-897512.js)  
```javascript
for (let i = 0; i < 100; i++) Array.prototype.unshift(3.14);

const o31 = [1.1];
o31[37] = 2.2;

const o51 = o31.concat(false);

o51[0] = undefined;

assertEquals(o51.length, 39);

o51.sort();

assertEquals(o51.length, 39);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0855fb1^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=0855fb1)  
[test/mjsunit/regress/regress-897512.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-897512.js?cl=0855fb1)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=0855fb1)  
  

---   

## **regress-897436.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: TaggedDoesntHaveInstanceType(value, JS_PROMISE_TYPE)](https://crbug.com/897436)**  
**[Commit: [async-await] remove CSA_SLOW_ASSERT in AsyncGeneratorResolve](https://chromium.googlesource.com/v8/v8/+/9867aa3)**  
  
Date(Commit): Wed Oct 24 00:04:55 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1296909](https://chromium-review.googlesource.com/c/1296909)  
Regress: [mjsunit/harmony/regress/regress-897436.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-897436.js)  
```javascript
async function* gen() {
  const alwaysPending = new Promise(() => {});
  alwaysPending.then = "non-callable then";
  yield alwaysPending;
}
gen().next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9867aa3^!)  
[src/builtins/builtins-async-generator-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-async-generator-gen.cc?cl=9867aa3)  
[test/mjsunit/harmony/regress/regress-897436.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-897436.js?cl=9867aa3)  
  

---   

## **regress-crbug-897404.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: IntPtrOrSmiGreaterThan(capacity, IntPtrOrSmiConstant(0, mode)](https://crbug.com/897404)**  
**[Commit: [builtins] Fix Array.p.join length overflow and invalid string length handling](https://chromium.googlesource.com/v8/v8/+/ec969ea)**  
  
Date(Commit): Tue Oct 23 15:04:24 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1293070](https://chromium-review.googlesource.com/c/1293070)  
Regress: [mjsunit/regress/regress-crbug-897404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-897404.js)  
```javascript
function TestError() {}

const a = new Array(2**32 - 1);

a[0] = {
  toString() { throw new TestError(); }
};

assertThrows(() => a.join(), TestError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec969ea^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=ec969ea)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=ec969ea)  
[src/builtins/builtins-array-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.h?cl=ec969ea)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=ec969ea)  
[src/torque/csa-generator.cc](https://cs.chromium.org/chromium/src/v8/src/torque/csa-generator.cc?cl=ec969ea)  
...  
  

---   

## **regress-897815.js (chromium issue)**  
   
**[Issue: CHECK failure: start_position == start_position_from_data in preparsed-scope-data.cc](https://crbug.com/897815)**  
**[Commit: [scanner] Fix apply for bookmarks and usage of scope_data within an error context.](https://chromium.googlesource.com/v8/v8/+/e91e180)**  
  
Date(Commit): Tue Oct 23 14:39:19 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, ReleaseBlock-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner, Target-72  
Code Review: [https://chromium-review.googlesource.com/c/1296469](https://chromium-review.googlesource.com/c/1296469)  
Regress: [mjsunit/regress/regress-897815.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-897815.js)  
```javascript
(function __f_19350() {
  function __f_19351() {
    function __f_19352() {
    }
  };
  %PrepareFunctionForOptimization(__f_19351);
  try {
    __f_19350();
  } catch (e) {
  }
  %OptimizeFunctionOnNextCall(__f_19351);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e91e180^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=e91e180)  
[src/parsing/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.cc?cl=e91e180)  
[src/parsing/scanner.h](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.h?cl=e91e180)  
[test/mjsunit/regress/regress-897815.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-897815.js?cl=e91e180)  
  

---   

## **regress-897366.js (chromium issue)**  
   
**[Issue: DCHECK failure in *p != to_check_ in heap.cc](https://crbug.com/897366)**  
**[Commit: [array] Fix left-trimming in Array.p.sort](https://chromium.googlesource.com/v8/v8/+/d31a5b6)**  
  
Date(Commit): Tue Oct 23 13:58:54 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, NodeJS-Backport-Done, Test-Predator-Auto-Owner, Target-70, M-70, merge-merged-7.0, merge-merged-7.1, Release-2-M70  
Code Review: [https://chromium-review.googlesource.com/c/1292066](https://chromium-review.googlesource.com/c/1292066)  
Regress: [mjsunit/regress/regress-897366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-897366.js)  
```javascript
let xs = [];
for (let i = 0; i < 205; ++i) {
  xs.push(i);
}
xs.sort((a, b) => {
  xs.shift();
  xs[xs.length] = -246;
  return a - b;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d31a5b6^!)  
[test/mjsunit/regress/regress-897366.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-897366.js?cl=d31a5b6)  
[third_party/v8/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/third_party/v8/builtins/array-sort.tq?cl=d31a5b6)  
  

---   

## **regress-8241.js (v8 issue)**  
   
**[Issue: Destructuring in the arguments of CallExpression is incorrect](https://crbug.com/v8/8241)**  
**[Commit: [parser] Validate destructuring assignment pattern in correct classifier](https://chromium.googlesource.com/v8/v8/+/cd21f71)**  
  
Date(Commit): Tue Oct 23 09:26:19 2018  
Code Review: [https://chromium-review.googlesource.com/c/1296229](https://chromium-review.googlesource.com/c/1296229)  
Regress: [mjsunit/regress/regress-8241.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8241.js)  
```javascript
function f(x) { }
f(x=>x, [x,y] = [1,2]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd21f71^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=cd21f71)  
[test/mjsunit/regress/regress-8241.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8241.js?cl=cd21f71)  
  

---   

## **regress-crbug-897098.js (chromium issue)**  
   
**[Issue: DCHECK failure in !is_the_hole(index) in fixed-array-inl.h](https://crbug.com/897098)**  
**[Commit: [elements] handle OOB-holes in Array.prototype.includes fast-path](https://chromium.googlesource.com/v8/v8/+/5b92f91)**  
  
Date(Commit): Tue Oct 23 09:07:37 2018  
Components: Blink>JavaScript  
Labels: ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-71, Target-71  
Code Review: [https://chromium-review.googlesource.com/c/1293571](https://chromium-review.googlesource.com/c/1293571)  
Regress: [mjsunit/regress/regress-crbug-897098.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-897098.js)  
```javascript
const arr = [1.1,2.2,3.3];
arr.pop();
const start = {toString: function() {arr.pop();}}
arr.includes(0, start);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5b92f91^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=5b92f91)  
[test/mjsunit/regress/regress-crbug-897098.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-897098.js?cl=5b92f91)  
  

---   

## **regress-crbug-897514.js (chromium issue)**  
   
**[Issue: ASSERT: CSA_ASSERT failed: Word32Equal(DecodeWord32<PropertyDetails::KindField>(details)](https://crbug.com/897514)**  
**[Commit: [ic] Respect PropertyDetails::KindField when following transitions](https://chromium.googlesource.com/v8/v8/+/6c703ff)**  
  
Date(Commit): Mon Oct 22 18:46:28 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-71, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1293955](https://chromium-review.googlesource.com/c/1293955)  
Regress: [mjsunit/regress/regress-crbug-897514.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-897514.js)  
```javascript
let o = {};
Object.defineProperty(o, 'a', {
    enumerable: true,
    configurable: true,
    get: function() { return 7 }
});

function spread(o) {
  let result = { ...o };
  %HeapObjectVerify(result);
  return result;
}

for (let i = 0; i<3; i++) {
  spread([]);
  // Use different transition => 'a'.
  spread({ a:0 });
  spread("abc");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c703ff^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=6c703ff)  
[test/mjsunit/regress/regress-crbug-897514.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-897514.js?cl=6c703ff)  
  

---   

## **regress-crbug-897406.js (chromium issue)**  
   
**[Issue: CHECK failure: generator_object->is_executing() in isolate.cc](https://crbug.com/897406)**  
**[Commit: [async] Gracefully handle suspended generators.](https://chromium.googlesource.com/v8/v8/+/2a08adb)**  
  
Date(Commit): Mon Oct 22 07:06:22 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, M-72, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1292057](https://chromium-review.googlesource.com/c/1292057)  
Regress: [mjsunit/regress/regress-crbug-897406.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-897406.js)  
```javascript
async_hooks.createHook({
  after() { throw new Error(); }
}).enable();

(async function() {
  await 1;
  await 1;
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a08adb^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=2a08adb)  
[test/mjsunit/regress/regress-crbug-897406.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-897406.js?cl=2a08adb)  
  

---   

## **regress-crbug-881247.js (chromium issue)**  
   
**[Issue: Fatal error related to field tracking](https://crbug.com/881247)**  
**[Commit: [CloneObjectIC] Avoid FieldType confusions](https://chromium.googlesource.com/v8/v8/+/78e5763)**  
  
Date(Commit): Fri Oct 19 11:03:21 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-70, Fuzz-Blocker, M-71, merge-merged-7.0, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1288637](https://chromium-review.googlesource.com/c/1288637)  
Regress: [mjsunit/regress/regress-crbug-881247.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-881247.js)  
```javascript
const resolvedPromise = Promise.resolve();

function spread() {
  const result = { ...resolvedPromise };
  %HeapObjectVerify(result);
  return result;
}

resolvedPromise[undefined] =  {a:1};
%HeapObjectVerify(resolvedPromise);

spread();

resolvedPromise[undefined] = undefined;
%HeapObjectVerify(resolvedPromise);

spread();
%HeapObjectVerify(resolvedPromise);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78e5763^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=78e5763)  
[test/mjsunit/regress/regress-crbug-881247.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-881247.js?cl=78e5763)  
  

---   

## **regress-crbug-896700.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::CaptureAsyncStackTrace](https://crbug.com/896700)**  
**[Commit: [async] Gracefully handle exceptions in async_hooks.](https://chromium.googlesource.com/v8/v8/+/e650b9e)**  
  
Date(Commit): Fri Oct 19 08:25:27 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1290550](https://chromium-review.googlesource.com/c/1290550)  
Regress: [mjsunit/regress/regress-crbug-896700.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-896700.js)  
```javascript
async_hooks.createHook({
  after() { throw new Error(); }
}).enable();
Promise.resolve().then();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e650b9e^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=e650b9e)  
[test/mjsunit/regress/regress-crbug-896700.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-896700.js?cl=e650b9e)  
  

---   

## **regress-crbug-896181.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSArray::ArrayJoinConcatToSequentialString](https://crbug.com/896181)**  
**[Commit: [builtins] Fix Array.p.join handling of an index getter with side effects](https://chromium.googlesource.com/v8/v8/+/7cb6c81)**  
  
Date(Commit): Thu Oct 18 10:46:23 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/c/1286957](https://chromium-review.googlesource.com/c/1286957)  
Regress: [mjsunit/regress/regress-crbug-896181.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-896181.js)  
```javascript
var a = new Array();
a[0] = 0.1;
a[2] = 0.2;
Object.defineProperty(a, 1, {
  get: function() {
    a[4] = 0.3;
  },
});

assertSame('0.1,,0.2', a.join());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7cb6c81^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=7cb6c81)  
[test/mjsunit/array-join-index-getter-side-effects.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-join-index-getter-side-effects.js?cl=7cb6c81)  
[test/mjsunit/regress/regress-crbug-896181.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-896181.js?cl=7cb6c81)  
  

---   

## **regress-cr895860.js (chromium issue)**  
   
**[Issue: Ill in __RT_impl_Runtime_AllocateInNewSpace](https://crbug.com/895860)**  
**[Commit: Use slow path in IterableToList for big input strings.](https://chromium.googlesource.com/v8/v8/+/779d102)**  
  
Date(Commit): Thu Oct 18 08:44:21 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1286337](https://chromium-review.googlesource.com/c/1286337)  
Regress: [mjsunit/es6/regress/regress-cr895860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-cr895860.js)  
```javascript
(function() {
  var s = "f";

  // 2^18 length, enough to ensure an array (of pointers) bigger than 500KB.
  for (var i = 0; i < 18; i++) {
    s += s;
  }

  var ss = [...s];
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/779d102^!)  
[src/builtins/builtins-iterator-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-iterator-gen.cc?cl=779d102)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=779d102)  
[src/builtins/builtins-string-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.h?cl=779d102)  
[test/mjsunit/es6/regress/regress-cr895860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-cr895860.js?cl=779d102)  
  

---   

## **regress-895799.js (chromium issue)**  
   
**[Issue: DCHECK failure in isolate->context() == nullptr || isolate->context()->IsContext() in runtime-inte](https://crbug.com/895799)**  
**[Commit: [deoptimizer] Materialize context properly for construct stub frame.](https://chromium.googlesource.com/v8/v8/+/2d11dda)**  
  
Date(Commit): Wed Oct 17 10:27:04 2018  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-71, Target-71, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1286336](https://chromium-review.googlesource.com/c/1286336)  
Regress: [mjsunit/compiler/regress-895799.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-895799.js)  
```javascript
class C extends Object {
  constructor() {
    try { super(); } catch (e) { };
    return 1;
  }
}

class A extends C {
  constructor() {
    super();
    throw new Error();
    return { get: () => this };
  }
}

%PrepareFunctionForOptimization(A);

var D = new Proxy(A, { get() { %DeoptimizeFunction(A); } });

try { Reflect.construct(A, [], D); } catch(e) {}
%OptimizeFunctionOnNextCall(A);
try { Reflect.construct(A, [], D); } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d11dda^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=2d11dda)  
[src/compiler/instruction.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction.h?cl=2d11dda)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=2d11dda)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=2d11dda)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=2d11dda)  
...  
  

---   

## **regress-895691.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/895691)**  
**[Commit: [turbofan] Allow converting word64 to float32 if value is safe integer.](https://chromium.googlesource.com/v8/v8/+/a8cb521)**  
  
Date(Commit): Tue Oct 16 11:31:39 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, ReleaseBlock-Stable, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-71, M71, merge-merged-7.1  
Code Review: [https://chromium-review.googlesource.com/c/1282990](https://chromium-review.googlesource.com/c/1282990)  
Regress: [mjsunit/regress/regress-895691.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-895691.js)  
```javascript
const n = 2 ** 32;
const x = new Float32Array();

function f() {
  for (var i = 96; i < 100; i += 4) {
    x[i] = i + n;
  }
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8cb521^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=a8cb521)  
[test/mjsunit/regress/regress-895691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-895691.js?cl=a8cb521)  
  

---   

## **regress-crbug-895199.js (chromium issue)**  
   
**[Issue: DCHECK failure in restriction_type.Is(info->restriction_type()) in simplified-lowering.cc](https://crbug.com/895199)**  
**[Commit: [turbofan] Fix representation selection of CheckFloat64Hole.](https://chromium.googlesource.com/v8/v8/+/63f92a9)**  
  
Date(Commit): Mon Oct 15 07:11:58 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1278804](https://chromium-review.googlesource.com/c/1278804)  
Regress: [mjsunit/regress/regress-crbug-895199.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-895199.js)  
```javascript
function foo() {
  var a = new Array(2);
  a[0] = 23.1234;
  a[1] = 25.1234;
  %DeoptimizeNow();
  return a[2];
};
%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63f92a9^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=63f92a9)  
[test/mjsunit/regress/regress-crbug-895199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-895199.js?cl=63f92a9)  
  

---   

## **regress-894374.js (chromium issue)**  
   
**[Issue: [liftoff] [ia32] Debug check failed: !unpinned.is_empty()](https://crbug.com/894374)**  
**[Commit: [Liftoff] Fewer pinned registers on store](https://chromium.googlesource.com/v8/v8/+/56b8ab5)**  
  
Date(Commit): Fri Oct 12 08:11:52 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Security_Severity-Low, Security_Impact-Head, allpublic, M-71  
Code Review: [https://chromium-review.googlesource.com/c/1275819](https://chromium-review.googlesource.com/c/1275819)  
Regress: [mjsunit/regress/wasm/regress-894374.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-894374.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false);
const sig = makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
builder.addFunction(undefined, sig)
  .addBodyWithEnd([
    kExprMemorySize, 0,
    kExprI32Const, 0,
    kExprI64Const, 0,
    kExprI64StoreMem8, 0, 0,
    kExprEnd,
  ]);
builder.addExport('main', 0);
builder.instantiate();  // shouldn't crash  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56b8ab5^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=56b8ab5)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=56b8ab5)  
[src/wasm/baseline/liftoff-register.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-register.h?cl=56b8ab5)  
[src/wasm/baseline/mips/liftoff-assembler-mips.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/mips/liftoff-assembler-mips.h?cl=56b8ab5)  
[src/wasm/baseline/mips64/liftoff-assembler-mips64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/mips64/liftoff-assembler-mips64.h?cl=56b8ab5)  
...  
  

---   

## **regress-preparse-inner-arrow-duplicate-parameter.js (other issue)**  
   
**[Commit: [parser] Rewrite duplicate formal detection](https://chromium.googlesource.com/v8/v8/+/e874d6a)**  
  
Date(Commit): Tue Oct 09 09:17:42 2018  
Code Review: [https://chromium-review.googlesource.com/c/1270578](https://chromium-review.googlesource.com/c/1270578)  
Regress: [mjsunit/regress/regress-preparse-inner-arrow-duplicate-parameter.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-preparse-inner-arrow-duplicate-parameter.js)  
```javascript
assertThrows("()=>{ (x,x)=>1 }", SyntaxError)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e874d6a^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=e874d6a)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=e874d6a)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=e874d6a)  
[src/parsing/duplicate-finder.h](https://cs.chromium.org/chromium/src/v8/src/parsing/duplicate-finder.h?cl=e874d6a)  
[src/parsing/expression-classifier.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-classifier.h?cl=e874d6a)  
...  
  
  
---   

## **regress-arrow-single-expression-eval.js (other issue)**  
   
**[Commit: [parser] Fix single-expression arrow function scoping](https://chromium.googlesource.com/v8/v8/+/af34c6c)**  
  
Date(Commit): Mon Oct 08 13:43:21 2018  
Code Review: [https://chromium-review.googlesource.com/c/1268236](https://chromium-review.googlesource.com/c/1268236)  
Regress: [mjsunit/regress/regress-arrow-single-expression-eval.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arrow-single-expression-eval.js)  
```javascript
((x=1) => eval("var x = 10"))();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/af34c6c^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=af34c6c)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=af34c6c)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=af34c6c)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=af34c6c)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=af34c6c)  
...  
  
  
---   

## **regress-892858.js (chromium issue)**  
   
**[Issue: Global-buffer-overflow in MemoryRead<unsigned](https://crbug.com/892858)**  
**[Commit: [async-await] Fix global-buffer-overflow issue when loading flag](https://chromium.googlesource.com/v8/v8/+/890fd9c)**  
  
Date(Commit): Mon Oct 08 09:16:14 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1267936](https://chromium-review.googlesource.com/c/1267936)  
Regress: [mjsunit/regress/regress-892858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-892858.js)  
```javascript
async function foo() {
  await Promise.resolve(42);
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/890fd9c^!)  
[src/builtins/builtins-async-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-async-gen.cc?cl=890fd9c)  
[test/mjsunit/regress/regress-892858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-892858.js?cl=890fd9c)  
  

---   

## **regress-8265.js (v8 issue)**  
   
**[Issue: Math.random distribution is skewed towards zero when --random_seed is used](https://crbug.com/v8/8265)**  
**[Commit: Warm up RNG when --random_seed is used](https://chromium.googlesource.com/v8/v8/+/88c5da0)**  
  
Date(Commit): Fri Oct 05 15:34:58 2018  
Code Review: [https://chromium-review.googlesource.com/c/1263816](https://chromium-review.googlesource.com/c/1263816)  
Regress: [mjsunit/regress/regress-8265.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8265.js)  
```javascript
for (let i = 0; i < 54; ++i) Math.random();
let sum = 0;
for (let i = 0; i < 10; ++i)
  sum += Math.floor(Math.random() * 50);

assertNotEquals(0, sum);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88c5da0^!)  
[src/base/utils/random-number-generator.h](https://cs.chromium.org/chromium/src/v8/src/base/utils/random-number-generator.h?cl=88c5da0)  
[src/math-random.cc](https://cs.chromium.org/chromium/src/v8/src/math-random.cc?cl=88c5da0)  
[test/mjsunit/regress/regress-8265.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8265.js?cl=88c5da0)  
  

---   

## **regress-crbug-892472-1.js (chromium issue)**  
   
**[Issue: DCHECK failure in code->kind() == Code::OPTIMIZED_FUNCTION in frames.cc](https://crbug.com/892472)**  
**[Commit: [async] Only try to peak into async functions/generators.](https://chromium.googlesource.com/v8/v8/+/4111c98)**  
  
Date(Commit): Fri Oct 05 06:36:27 2018  
Components: Platform>DevTools>JavaScript, Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [http://bit.ly/v8-zero-cost-async-stack-traces](http://bit.ly/v8-zero-cost-async-stack-traces)  
Regress: [mjsunit/regress/regress-crbug-892472-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-892472-1.js), [mjsunit/regress/regress-crbug-892472-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-892472-2.js)  
```javascript
const a = /x/;
a.exec = RegExp.prototype.test;
assertThrows(() => RegExp.prototype.test.call(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4111c98^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=4111c98)  
[test/mjsunit/regress/regress-crbug-892472-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-892472-1.js?cl=4111c98)  
[test/mjsunit/regress/regress-crbug-892472-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-892472-2.js?cl=4111c98)  
  

---   

## **regress-crbug-891627.js (chromium issue)**  
   
**[Issue: CHECK failure: NumberModulus of kRepWord32 ((MinusZero | Range(-1, 0))) cannot be changed to kR](https://crbug.com/891627)**  
**[Commit: [turbofan] Fix Word32 (Signed32OrMinusZero) conversions that identify zeros.](https://chromium.googlesource.com/v8/v8/+/513a5bd)**  
  
Date(Commit): Thu Oct 04 09:13:18 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/c/1258902](https://chromium-review.googlesource.com/c/1258902)  
Regress: [mjsunit/regress/regress-crbug-891627.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-891627.js)  
```javascript
function bar(x) { return x % 2; }
bar(0.1);

(function() {
  function foo(x) {
    // The NumberEqual identifies 0 and -0.
    return bar(x | -1) == 4294967295;
  }

  %PrepareFunctionForOptimization(foo);
  assertFalse(foo(1));
  assertFalse(foo(0));
  %OptimizeFunctionOnNextCall(foo);
  assertFalse(foo(1));
  assertFalse(foo(0));
})();

(function() {
  function makeFoo(y) {
    return function foo(x) {
      return bar(x | -1) == y;
    }
  }
  makeFoo(0);  // Defeat the function context specialization.
  const foo = makeFoo(1);

  %PrepareFunctionForOptimization(foo);
  assertFalse(foo(1));
  assertFalse(foo(0));
  %OptimizeFunctionOnNextCall(foo);
  assertFalse(foo(1));
  assertFalse(foo(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/513a5bd^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=513a5bd)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=513a5bd)  
[test/mjsunit/regress/regress-crbug-891627.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-891627.js?cl=513a5bd)  
  

---   

## **regress-890553.js (chromium issue)**  
   
**[Issue: DCHECK failure in (function_) == nullptr in scopes.cc](https://crbug.com/890553)**  
**[Commit: [parser] Fix function name variable tracking](https://chromium.googlesource.com/v8/v8/+/563eeec)**  
  
Date(Commit): Mon Oct 01 13:14:33 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-71  
Code Review: [https://chromium-review.googlesource.com/1254061](https://chromium-review.googlesource.com/1254061)  
Regress: [mjsunit/regress/regress-890553.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-890553.js)  
```javascript
"use strict";
var s = "function __f_9(func, testName) {" +
  "var __v_0 = function __f_10(__v_14, __v_14) {" +
  "  return __v_16;" +
  "}; " +
"}"
assertThrows(function() { eval(s); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/563eeec^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=563eeec)  
[test/mjsunit/regress/regress-890553.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-890553.js?cl=563eeec)  
  

---   

## **regress-890620.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::MapRef::AsElementsKind](https://crbug.com/890620)**  
**[Commit: [turbofan] Make sure we use only serialized elements kind transitions.](https://chromium.googlesource.com/v8/v8/+/56b6b6a)**  
  
Date(Commit): Mon Oct 01 08:44:23 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1253105](https://chromium-review.googlesource.com/1253105)  
Regress: [mjsunit/compiler/regress-890620.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-890620.js)  
```javascript
var a = 42;

function g(n) {
  while (n > 0) {
    a = new Array(n);
    n--;
  }
}

g(1);

function f() {
  g();
}

%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();
assertEquals(1, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56b6b6a^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=56b6b6a)  
[src/compiler/js-create-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.h?cl=56b6b6a)  
[test/mjsunit/compiler/regress-890620.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-890620.js?cl=56b6b6a)  
  

---   

## **regress-crbug-890243.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/890243)**  
**[Commit: [turbofan] Add missing Word64->Bit support.](https://chromium.googlesource.com/v8/v8/+/852a8d3)**  
  
Date(Commit): Sun Sep 30 09:07:15 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1251551](https://chromium-review.googlesource.com/1251551)  
Regress: [mjsunit/regress/regress-crbug-890243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-890243.js)  
```javascript
function bar(x) {
  return x + x;
}
bar(0.1);

function baz(y) {
  return {y};
}
baz(null);
baz(0);

function foo(o) {
  return !baz(bar(o.x)).y;
};
%PrepareFunctionForOptimization(foo);
assertFalse(foo({x: 1}));
assertFalse(foo({x: 1}));
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo({x: 1}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/852a8d3^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=852a8d3)  
[test/mjsunit/regress/regress-crbug-890243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-890243.js?cl=852a8d3)  
  

---   

## **regress-890057.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSFunction::ComputeInstanceSizeWithMinSlack](https://crbug.com/890057)**  
**[Commit: [turbofan] Fail slack tracking dependency if initial map disappears.](https://chromium.googlesource.com/v8/v8/+/e693c69)**  
  
Date(Commit): Fri Sep 28 08:20:42 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1250481](https://chromium-review.googlesource.com/1250481)  
Regress: [mjsunit/compiler/regress-890057.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-890057.js)  
```javascript
function f() {}
function g() {
  f.prototype = undefined;
  f();
  new f();
}

for (let i = 0; i < 10000; i++) g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e693c69^!)  
[src/compiler/compilation-dependencies.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/compilation-dependencies.cc?cl=e693c69)  
[test/mjsunit/compiler/regress-890057.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-890057.js?cl=e693c69)  
  

---   

## **regress-889722.js (chromium issue)**  
   
**[Issue: CHECK failure: (data_) != nullptr in js-heap-broker.h](https://crbug.com/889722)**  
**[Commit: [turbofan] Prepare broker for the next steps.](https://chromium.googlesource.com/v8/v8/+/bcbb6d9)**  
  
Date(Commit): Thu Sep 27 10:22:51 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1245425](https://chromium-review.googlesource.com/1245425)  
Regress: [mjsunit/regress/regress-889722.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-889722.js)  
```javascript
function getRandomProperty(v, rand) {
  var properties = Object.getOwnPropertyNames(v);
  return properties[rand % properties.length];
}
r = Realm.create();
o = Realm.eval(r, "() => { return Realm.global(-10) instanceof Object }");
o.__p_211203344 = o[getRandomProperty(o, 211203344)];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bcbb6d9^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=bcbb6d9)  
[src/compiler/js-heap-broker.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.h?cl=bcbb6d9)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=bcbb6d9)  
[test/mjsunit/regress/regress-889722.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-889722.js?cl=bcbb6d9)  
[test/unittests/compiler/graph-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/graph-unittest.cc?cl=bcbb6d9)  
...  
  

---   

## **regress-888923.js (chromium issue)**  
   
**[Issue: Security: Chrome RCE](https://crbug.com/888923)**  
**[Commit: [turbofan] Fix ObjectCreate's side effect annotation.](https://chromium.googlesource.com/v8/v8/+/52a9e67)**  
  
Date(Commit): Wed Sep 26 12:02:11 2018  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, reward-0, Security_Impact-Stable, Security_Severity-High, allpublic, NodeJS-Backport-Done, M-69, M-70, CVE_description-submitted, merge-merged-6.9, merge-merged-7.0, Release-0-M70, CVE-2018-17463  
Code Review: [https://chromium-review.googlesource.com/1245763](https://chromium-review.googlesource.com/1245763)  
Regress: [mjsunit/compiler/regress-888923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-888923.js)  
```javascript
(function() {
  function f(o) {
    o.x;
    Object.create(o);
    return o.y.a;
  }

  %PrepareFunctionForOptimization(f);
  f({ x : 0, y : { a : 1 } });
  f({ x : 0, y : { a : 2 } });
  %OptimizeFunctionOnNextCall(f);
  assertEquals(3, f({ x : 0, y : { a : 3 } }));
})();

(function() {
  function f(o) {
    let a = o.y;
    Object.create(o);
    return o.x + a;
  }

  %PrepareFunctionForOptimization(f);
  f({ x : 42, y : 21 });
  f({ x : 42, y : 21 });
  %OptimizeFunctionOnNextCall(f);
  assertEquals(63, f({ x : 42, y : 21 }));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52a9e67^!)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=52a9e67)  
[test/mjsunit/compiler/regress-888923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-888923.js?cl=52a9e67)  
  

---   

## **regress-crbug-888825.js (chromium issue)**  
   
**[Issue: DCHECK failure in byte_data_->size() % ByteData::kSkippableFunctionDataSize == ByteData::kPlacehol](https://crbug.com/888825)**  
**[Commit: [parser] Don't resolve preparser variables for arrow functions](https://chromium.googlesource.com/v8/v8/+/55ecf51)**  
  
Date(Commit): Tue Sep 25 15:28:18 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-71, Target-71  
Code Review: [https://chromium-review.googlesource.com/1243383](https://chromium-review.googlesource.com/1243383)  
Regress: [mjsunit/regress/regress-crbug-888825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-888825.js)  
```javascript
eval("((a=function g() { function g() {}}) => {})();");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55ecf51^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=55ecf51)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=55ecf51)  
[test/mjsunit/regress/regress-crbug-888825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-888825.js?cl=55ecf51)  
  

---   

## **regress-crbug-887891.js (chromium issue)**  
   
**[Issue: CHECK failure: byte_length() <= JSArrayBuffer::kMaxByteLength in objects-debug.cc](https://crbug.com/887891)**  
**[Commit: [es2015] Setup JSTypedArray after allocating the JSArrayBuffer.](https://chromium.googlesource.com/v8/v8/+/129f770)**  
  
Date(Commit): Fri Sep 21 12:02:12 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1238494](https://chromium-review.googlesource.com/1238494)  
Regress: [mjsunit/regress/regress-crbug-887891.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-887891.js)  
```javascript
const l = 1000000000;
const a = [];
function foo() { var x = new Int32Array(l); }
try { foo(); } catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/129f770^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=129f770)  
[test/mjsunit/regress/regress-crbug-887891.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-887891.js?cl=129f770)  
  

---   

## **regress-crbug-687063.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,fullcode:x64,ignition_staging](https://crbug.com/687063)**  
**[Commit: [es2015] Fix ToPrimitive conversions in relational comparisons.](https://chromium.googlesource.com/v8/v8/+/08aec7d)**  
  
Date(Commit): Fri Sep 21 10:53:06 2018  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>Interpreter  
Labels: Stability-Crash, Reproducible, Hotlist-Recharge-Cold, Clusterfuzz, ClusterFuzz-Verified, Project-Ignition, v8-foozzie-failure  
Code Review: [https://chromium-review.googlesource.com/1236557](https://chromium-review.googlesource.com/1236557)  
Regress: [mjsunit/regress/regress-crbug-687063.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-687063.js)  
```javascript
const actual = [];

function foo() {
  actual.length = 0;
  const lhs = Symbol();
  const rhs = new Proxy({}, {
    get: function(target, property, receiver) {
      actual.push(property);
      return undefined;
    }
  });

  return lhs < rhs;
};
%PrepareFunctionForOptimization(foo);
assertThrows(foo, TypeError);
assertEquals([Symbol.toPrimitive, 'valueOf', 'toString'], actual);
assertThrows(foo, TypeError);
assertEquals([Symbol.toPrimitive, 'valueOf', 'toString'], actual);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);
assertEquals([Symbol.toPrimitive, 'valueOf', 'toString'], actual);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/08aec7d^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=08aec7d)  
[test/mjsunit/regress/regress-crbug-687063.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-687063.js?cl=08aec7d)  
  

---   

## **regress-crbug-885404.js (chromium issue)**  
   
**[Issue: CHECK failure: byte_length() <= JSArrayBuffer::kMaxByteLength in objects-debug.cc](https://crbug.com/885404)**  
**[Commit: [es2015] Clear JSTypedArray raw fields in the constructor.](https://chromium.googlesource.com/v8/v8/+/984048e)**  
  
Date(Commit): Wed Sep 19 09:28:11 2018  
Components: Blink>JavaScript>Runtime  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1233257](https://chromium-review.googlesource.com/1233257)  
Regress: [mjsunit/regress/regress-crbug-885404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-885404.js)  
```javascript
var ab = new ArrayBuffer(2);
try { new Int32Array(ab); } catch (e) { }
assertEquals(2, ab.byteLength);
gc();
assertEquals(2, ab.byteLength);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/984048e^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=984048e)  
[test/mjsunit/regress/regress-crbug-885404.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-885404.js?cl=984048e)  
  

---   

## **regress-884052.js (chromium issue)**  
   
**[Issue: DCHECK failure in RegionObservability::kObservable == region_observability_ in effect-control-line](https://crbug.com/884052)**  
**[Commit: [turbofan] Fix dead value insertion in simplified lowering.](https://chromium.googlesource.com/v8/v8/+/b6bdd74)**  
  
Date(Commit): Tue Sep 18 09:30:26 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-71  
Code Review: [https://chromium-review.googlesource.com/1228125](https://chromium-review.googlesource.com/1228125)  
Regress: [mjsunit/compiler/regress-884052.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-884052.js)  
```javascript
function foo() {
  var a = new Array(2);
  for (var i = 1; i > -1; i = i - 2) {
    if (i < a.length) a = new Array(i);
  }
}

%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b6bdd74^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b6bdd74)  
[test/mjsunit/compiler/regress-884052.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-884052.js?cl=b6bdd74)  
  

---   

## **regress-crbug-884933.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/884933)**  
**[Commit: [turbofan] Add missing Word8/16 -> Word64 representation changes.](https://chromium.googlesource.com/v8/v8/+/1210d0c)**  
  
Date(Commit): Tue Sep 18 08:51:27 2018  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1229953](https://chromium-review.googlesource.com/1229953)  
Regress: [mjsunit/regress/regress-crbug-884933.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-884933.js)  
```javascript
(function() {
function bar(x, y) {
  return x + y;
}

bar(0.1, 0.2);
bar(0.1, 0.2);

function foo(dv) {
  return bar(dv.getUint8(0, true), 0xFFFFFFFF);
};
%PrepareFunctionForOptimization(foo);
const dv = new DataView(new ArrayBuffer(8));
assertEquals(0xFFFFFFFF, foo(dv));
assertEquals(0xFFFFFFFF, foo(dv));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0xFFFFFFFF, foo(dv));
})();

(function() {
function bar(x, y) {
  return x + y;
}

bar(0.1, 0.2);
bar(0.1, 0.2);

function foo(dv) {
  return bar(dv.getInt8(0, true), 0xFFFFFFFF);
};
%PrepareFunctionForOptimization(foo);
const dv = new DataView(new ArrayBuffer(8));
assertEquals(0xFFFFFFFF, foo(dv));
assertEquals(0xFFFFFFFF, foo(dv));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0xFFFFFFFF, foo(dv));
})();

(function() {
function bar(x, y) {
  return x + y;
}

bar(0.1, 0.2);
bar(0.1, 0.2);

function foo(dv) {
  return bar(dv.getUint16(0, true), 0xFFFFFFFF);
};
%PrepareFunctionForOptimization(foo);
const dv = new DataView(new ArrayBuffer(8));
assertEquals(0xFFFFFFFF, foo(dv));
assertEquals(0xFFFFFFFF, foo(dv));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0xFFFFFFFF, foo(dv));
})();

(function() {
function bar(x, y) {
  return x + y;
}

bar(0.1, 0.2);
bar(0.1, 0.2);

function foo(dv) {
  return bar(dv.getInt16(0, true), 0xFFFFFFFF);
};
%PrepareFunctionForOptimization(foo);
const dv = new DataView(new ArrayBuffer(8));
assertEquals(0xFFFFFFFF, foo(dv));
assertEquals(0xFFFFFFFF, foo(dv));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0xFFFFFFFF, foo(dv));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1210d0c^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=1210d0c)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=1210d0c)  
[test/mjsunit/regress/regress-crbug-884933.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-884933.js?cl=1210d0c)  
  

---   

## **regress-883059.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_resolved() in ast.h](https://crbug.com/883059)**  
**[Commit: Reland "[preparser] Refactor VariableProxies to use ThreadedLists interface"](https://chromium.googlesource.com/v8/v8/+/d970749)**  
  
Date(Commit): Wed Sep 12 15:13:29 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript, Blink>JavaScript>Interpreter  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1206292](https://chromium-review.googlesource.com/1206292)  
Regress: [mjsunit/regress/regress-883059.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-883059.js)  
```javascript
var __v_47 = ({[__v_46]: __f_52}) => { var __v_46 = 'b'; return __f_52; };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d970749^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=d970749)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=d970749)  
[src/ast/scopes-inl.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes-inl.h?cl=d970749)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=d970749)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=d970749)  
...  
  

---   

## **regress-crbug-882233-1.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/882233)**  
**[Commit: [array] Consistently throw TypeError for zero-length arrays](https://chromium.googlesource.com/v8/v8/+/e365bc2)**  
  
Date(Commit): Mon Sep 10 09:50:52 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1215164](https://chromium-review.googlesource.com/1215164)  
Regress: [mjsunit/regress/regress-crbug-882233-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-882233-1.js), [mjsunit/regress/regress-crbug-882233-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-882233-2.js)  
```javascript
let array = [];
Object.defineProperty(array, 'length', {writable: false});

assertEquals(array.length, 0);
assertThrows(() => array.shift(), TypeError);

let object = { length: 0 };
Object.defineProperty(object, 'length', {writable: false});

assertEquals(object.length, 0);
assertThrows(() => Array.prototype.shift.call(object));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e365bc2^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=e365bc2)  
[test/mjsunit/regress/regress-crbug-882233-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-882233-1.js?cl=e365bc2)  
[test/mjsunit/regress/regress-crbug-882233-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-882233-2.js?cl=e365bc2)  
  

---   

## **regress-8133-1.js (v8 issue)**  
   
**[Issue: Current IterableToList implementation can lead to spec violations ](https://crbug.com/v8/8133)**  
**[Commit: [typedarray] Properly convert hole to undefined in TypedArray.from](https://chromium.googlesource.com/v8/v8/+/ece86ad)**  
  
Date(Commit): Mon Sep 10 09:31:55 2018  
Code Review: [https://chromium-review.googlesource.com/1213204](https://chromium-review.googlesource.com/1213204)  
Regress: [mjsunit/regress/regress-8133-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8133-1.js), [mjsunit/regress/regress-8133-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8133-2.js)  
```javascript
const arr = [1, , 3];

function mapper(x) {
  Array.prototype[1] = 2;
  return x + 1;
}

assertArrayEquals([2, 0, 4], Uint16Array.from(arr, mapper));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ece86ad^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=ece86ad)  
[test/mjsunit/regress/regress-8133-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8133-1.js?cl=ece86ad)  
[test/mjsunit/regress/regress-8133-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-8133-2.js?cl=ece86ad)  
  

---   

## **regress-crbug-880207.js (chromium issue)**  
   
**[Issue: Security: incorrect type information on Math.expm1](https://crbug.com/880207)**  
**[Commit: [turbofan] Fix incorrect typing rule for NumberExpm1.](https://chromium.googlesource.com/v8/v8/+/56f7dda)**  
  
Date(Commit): Wed Sep 05 16:07:16 2018  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, M-70, M-71, merge-merged-7.1, Release-0-M71  
Code Review: [https://chromium-review.googlesource.com/1205072](https://chromium-review.googlesource.com/1205072)  
Regress: [mjsunit/regress/regress-crbug-880207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-880207.js)  
```javascript
(function TestOptimizedFastExpm1MinusZero() {
  function foo() {
    return Object.is(Math.expm1(-0), -0);
  };
  %PrepareFunctionForOptimization(foo);
  assertTrue(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertTrue(foo());
})();

(function TestOptimizedExpm1MinusZeroSlowPath() {
  function f(x) {
    return Object.is(Math.expm1(x), -0);
  };
  %PrepareFunctionForOptimization(f);
  function g() {
    return f(-0);
  };
  %PrepareFunctionForOptimization(g);
  f(0);
  // Compile function optimistically for numbers (with fast inlined
  // path for Math.expm1).
  %OptimizeFunctionOnNextCall(f);
  // Invalidate the optimistic assumption, deopting and marking non-number
  // input feedback in the call IC.
  f("0");
  // Optimize again, now with non-lowered call to Math.expm1.
  assertTrue(g());
  %OptimizeFunctionOnNextCall(g);
  assertTrue(g());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56f7dda^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=56f7dda)  
[test/mjsunit/regress/regress-crbug-880207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-880207.js?cl=56f7dda)  
  

---   

## **regress-crbug-876443.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(p_o) at ../../src/code-stub-assembler.h:351 in code-ass](https://crbug.com/876443)**  
**[Commit: [builtins] Enable Torque Array.prototype.splice](https://chromium.googlesource.com/v8/v8/+/fd334b3)**  
  
Date(Commit): Tue Sep 04 13:18:23 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-71, Target-71, M71  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/1199403](https://chromium-review.googlesource.com/c/v8/v8/+/1199403)  
Regress: [mjsunit/regress/regress-crbug-876443.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-876443.js)  
```javascript
var a = [5.65];
a.splice(0);
var b = a.splice(-4, 9, 10);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd334b3^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=fd334b3)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=fd334b3)  
[src/builtins/array-reverse.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-reverse.tq?cl=fd334b3)  
[src/builtins/array-splice.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-splice.tq?cl=fd334b3)  
[src/builtins/array.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array.tq?cl=fd334b3)  
...  
  

---   

## **regress-8094.js (v8 issue)**  
   
**[Issue: [wasm] Exception object instantiation is observable](https://crbug.com/v8/8094)**  
**[Commit: [wasm] Make exception creation non-observable by JS.](https://chromium.googlesource.com/v8/v8/+/e8d79f0)**  
  
Date(Commit): Tue Sep 04 10:37:27 2018  
Code Review: [https://chromium-review.googlesource.com/1203951](https://chromium-review.googlesource.com/1203951)  
Regress: [mjsunit/regress/wasm/regress-8094.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8094.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

var builder = new WasmModuleBuilder();
builder.addException(kSig_v_v);
builder.addFunction("propel", kSig_v_v)
       .addBody([kExprThrow, 0])
       .exportFunc();
var instance = builder.instantiate();

var exception;
try {
  instance.exports.propel();
} catch (e) {
  exception = e;
}

assertInstanceof(exception, WebAssembly.RuntimeError);
assertArrayEquals(["stack", "message"], Object.getOwnPropertyNames(exception));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e8d79f0^!)  
[src/heap-symbols.h](https://cs.chromium.org/chromium/src/v8/src/heap-symbols.h?cl=e8d79f0)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=e8d79f0)  
[src/runtime/runtime-wasm.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-wasm.cc?cl=e8d79f0)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=e8d79f0)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=e8d79f0)  
...  
  

---   

## **regress-crbug-879898.js (chromium issue)**  
   
**[Issue: CHECK failure: TypeError: node #28:JSToNumber type Numeric is not Number in verifier.cc](https://crbug.com/879898)**  
**[Commit: [turbofan] Improve typing of ToNumeric and ToNumber.](https://chromium.googlesource.com/v8/v8/+/b898112)**  
  
Date(Commit): Mon Sep 03 19:14:09 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1201522](https://chromium-review.googlesource.com/1201522)  
Regress: [mjsunit/regress/regress-crbug-879898.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-879898.js)  
```javascript
function foo() {
  return Symbol.toPrimitive++;
};
%PrepareFunctionForOptimization(foo);
assertThrows(foo);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b898112^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=b898112)  
[src/compiler/operation-typer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.h?cl=b898112)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=b898112)  
[test/mjsunit/regress/regress-crbug-879898.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-879898.js?cl=b898112)  
  

---   

## **regress-crbug-879560.js (chromium issue)**  
   
**[Issue: DCHECK failure in (pointer_) != nullptr in utils.h](https://crbug.com/879560)**  
**[Commit: [turbofan] Fix typo flushed out by recent CL.](https://chromium.googlesource.com/v8/v8/+/b1bd6be)**  
  
Date(Commit): Fri Aug 31 14:58:25 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1199742](https://chromium-review.googlesource.com/1199742)  
Regress: [mjsunit/regress/regress-crbug-879560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-879560.js)  
```javascript
function foo() {
  var x = 1;
  x = undefined;
  while (x--)
    ;
};
%PrepareFunctionForOptimization(foo);
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1bd6be^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b1bd6be)  
[test/mjsunit/regress/regress-crbug-879560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-879560.js?cl=b1bd6be)  
  

---   

## **regress-crbug-878845.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(p_o) at ../../src/code-stub-assembler.h:351 in code-ass](https://crbug.com/878845)**  
**[Commit: [array] Fix side-effect for 'from' argument in Array.p.lastIndexOf](https://chromium.googlesource.com/v8/v8/+/b9540d4)**  
  
Date(Commit): Thu Aug 30 13:34:25 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-69, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/1196507](https://chromium-review.googlesource.com/1196507)  
Regress: [mjsunit/regress/regress-crbug-878845.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-878845.js)  
```javascript
let arr = [, 0.1];

Array.prototype.lastIndexOf.call(arr, 100, {
  valueOf() {
    arr.length = 0;
  }
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9540d4^!)  
[src/builtins/array-lastindexof.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-lastindexof.tq?cl=b9540d4)  
[test/mjsunit/regress/regress-crbug-878845.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-878845.js?cl=b9540d4)  
  

---   

## **regress-8095.js (v8 issue)**  
   
**[Issue: [wasm] Stack unwinding crashes during exception handler lookup](https://crbug.com/v8/8095)**  
**[Commit: [wasm] Fix crash during exception stack unwinding.](https://chromium.googlesource.com/v8/v8/+/dd40b33)**  
  
Date(Commit): Tue Aug 28 13:02:44 2018  
Code Review: [https://chromium-review.googlesource.com/1193445](https://chromium-review.googlesource.com/1193445)  
Regress: [mjsunit/regress/wasm/regress-8095.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8095.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

var error = new Error("my error");
error.__proto__ = new Proxy(new Error(), {
  has(target, property, receiver) {
    assertUnreachable();
  }
});

var builder = new WasmModuleBuilder();
builder.addImport('mod', 'fun', kSig_v_v);
builder.addFunction("funnel", kSig_v_v)
       .addBody([kExprCallFunction, 0])
       .exportFunc();
var instance = builder.instantiate({ mod: {fun: function() { throw error }}});
assertThrows(instance.exports.funnel, Error);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dd40b33^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=dd40b33)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=dd40b33)  
[test/mjsunit/regress/wasm/regress-8095.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-8095.js?cl=dd40b33)  
  

---   

## **regress-v8-8070.js (v8 issue)**  
   
**[Issue: for of loop still slower than a traditional for loop](https://crbug.com/v8/8070)**  
**[Commit: [es2015] Use [[ArrayIteratorNextIndex]] to indicate exhaustion.](https://chromium.googlesource.com/v8/v8/+/6031f17)**  
  
Date(Commit): Tue Aug 21 11:26:00 2018  
Code Review: [https://chromium-review.googlesource.com/1181902](https://chromium-review.googlesource.com/1181902)  
Regress: [mjsunit/regress/regress-v8-8070.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-8070.js)  
```javascript
function bar(iterator) {
  for (const entry of iterator) {}
}

%NeverOptimizeFunction(bar);

function foo(a) {
  const iterator = a.values();
  bar(iterator);
  return iterator.next().done;
}

const a = [1, 2, 3];
%PrepareFunctionForOptimization(foo);
assertTrue(foo(a));
assertTrue(foo(a));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6031f17^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=6031f17)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=6031f17)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=6031f17)  
[src/compiler/access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.cc?cl=6031f17)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=6031f17)  
...  
  

---   

## **regress-875556.js (chromium issue)**  
   
**[Issue: Heap-buffer-overflow in int v8::internal::wasm::Decoder::read_leb_tail<int,](https://crbug.com/875556)**  
**[Commit: [wasm] Abort decoding of BlockTypeImmediate after an error was detected](https://chromium.googlesource.com/v8/v8/+/af4cf8d)**  
  
Date(Commit): Mon Aug 20 12:09:11 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Security_Impact-Head, allpublic, Clusterfuzz, ClusterFuzz-Verified, Stability-AFL, Test-Predator-Auto-CC, Test-Predator-Auto-Components, Target-70, M-70  
Code Review: [https://chromium-review.googlesource.com/1180964](https://chromium-review.googlesource.com/1180964)  
Regress: [mjsunit/regress/wasm/regress-875556.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-875556.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  const builder = new WasmModuleBuilder();
  // Generate function 1 (out of 2).
  sig1 = makeSig([kWasmI32], []);
  builder.addFunction("main", sig1).addBodyWithEnd([
    // signature: v_i
    // body:
    kExprBlock,
  ]);
  assertThrows(function() { builder.instantiate(); }, WebAssembly.CompileError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/af4cf8d^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=af4cf8d)  
[test/mjsunit/regress/wasm/regress-875556.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-875556.js?cl=af4cf8d)  
  

---   

## **regress-875493.js (chromium issue)**  
   
**[Issue: CHECK failure: !ResultAreIdentical(args); RegExpBuiltinsFuzzerHash=decNUMBER in regexp-builtins](https://crbug.com/875493)**  
**[Commit: [regexp] Fix invalid lastIndex handling in RegExp.p[@@replace]](https://chromium.googlesource.com/v8/v8/+/d74a9fd)**  
  
Date(Commit): Mon Aug 20 10:25:39 2018  
Components: Blink>JavaScript, Blink>JavaScript>Regexp  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/1180889](https://chromium-review.googlesource.com/1180889)  
Regress: [mjsunit/regress/regress-875493.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-875493.js)  
```javascript
function test() {
  const re = /./y;
  re.lastIndex = 3;
  const str = 'fg';
  return re[Symbol.replace](str, '$');
}

%SetForceSlowPath(false);
const fast = test();
%SetForceSlowPath(true);
const slow = test();
%SetForceSlowPath(false);

assertEquals(slow, fast);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d74a9fd^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=d74a9fd)  
[test/mjsunit/regress/regress-875493.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-875493.js?cl=d74a9fd)  
  

---   

## **regress-8059.js (v8 issue)**  
   
**[Issue: Serialization of WebAssembly.Module assertion failure](https://crbug.com/v8/8059)**  
**[Commit: [wasm] Fix {IsWebAssemblyCompiledModule} predicate.](https://chromium.googlesource.com/v8/v8/+/62b894b)**  
  
Date(Commit): Mon Aug 20 09:17:08 2018  
Code Review: [https://chromium-review.googlesource.com/1180886](https://chromium-review.googlesource.com/1180886)  
Regress: [mjsunit/regress/wasm/regress-8059.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-8059.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestPostModule() {
  let builder = new WasmModuleBuilder();
  builder.addFunction("add", kSig_i_ii)
    .addBody([kExprLocalGet, 0, kExprLocalGet, 1, kExprI32Add])
    .exportFunc();

  let module = builder.toModule();

  let workerScript = `
    onmessage = function(module) {
      try {
        let instance = new WebAssembly.Instance(module);
        let result = instance.exports.add(40, 2);
        postMessage(result);
      } catch(e) {
        postMessage('ERROR: ' + e);
      }
    }
  `;

  let realm = Realm.create();
  Realm.shared = { m:module, s:workerScript };

  let realmScript = `
    let worker = new Worker(Realm.shared.s, {type: 'string'});
    worker.postMessage(Realm.shared.m);
    let message = worker.getMessage();
    worker.terminate();
    message;
  `;
  let message = Realm.eval(realm, realmScript);
  assertEquals(42, message);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/62b894b^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=62b894b)  
[test/mjsunit/regress/wasm/regress-8059.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-8059.js?cl=62b894b)  
  

---   

## **regress-873600.js (chromium issue)**  
   
**[Issue: CHECK failure: mem_size <= wasm::kV8MaxWasmMemoryBytes in wasm-objects.cc](https://crbug.com/873600)**  
**[Commit: [asmjs] Properly validate asm.js heap sizes](https://chromium.googlesource.com/v8/v8/+/5d69010)**  
  
Date(Commit): Thu Aug 16 14:02:02 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-0, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, M-70  
Code Review: [https://chromium-review.googlesource.com/1174411](https://chromium-review.googlesource.com/1174411)  
Regress: [mjsunit/regress/wasm/regress-873600.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-873600.js)  
```javascript
(function DoTest() {

  var stdlib = this;
  try {
    var buffer = new ArrayBuffer((2097120) * 1024);
  } catch (e) {
    // Out of memory: soft pass because 2GiB is actually a lot!
    print("OOM: soft pass");
    return;
  }
  var foreign = {}

  var m = (function Module(stdlib, foreign, heap) {
    "use asm";
    var MEM16 = new stdlib.Int16Array(heap);
    function load(i) {
      i = i|0;
      i = MEM16[i >> 1]|0;
      return i | 0;
    }
    function store(i, v) {
      i = i|0;
      v = v|0;
      MEM16[i >> 1] = v;
    }
    function load8(i) {
      i = i|0;
      i = MEM16[i + 8 >> 1]|0;
      return i | 0;
    }
    function store8(i, v) {
      i = i|0;
      v = v|0;
      MEM16[i + 8 >> 1] = v;
    }
    return { load: load, store: store, load8: load8, store8: store8 };
  })(stdlib, foreign, buffer);

  assertEquals(0, m.load(-8));
  assertEquals(0, m.load8(-16));
  m.store(2014, 2, 30, 1, 0);
  assertEquals(0, m.load8(-8));
  m.store8(-8, 99);
  assertEquals(99, m.load(0));
  assertEquals(99, m.load8(-8));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d69010^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=5d69010)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=5d69010)  
[test/message/asm-linking-bogus-heap.out](https://cs.chromium.org/chromium/src/v8/test/message/asm-linking-bogus-heap.out?cl=5d69010)  
[test/mjsunit/asm/asm-heap.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/asm-heap.js?cl=5d69010)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=5d69010)  
...  
  

---   

## **regress-8033.js (v8 issue)**  
   
**[Issue: Use of Label in a false IfStatement](https://crbug.com/v8/8033)**  
**[Commit: [parsing] Fix detection of invalid continue targets.](https://chromium.googlesource.com/v8/v8/+/260af11)**  
  
Date(Commit): Tue Aug 14 08:30:47 2018  
Code Review: [https://chromium-review.googlesource.com/1172292](https://chromium-review.googlesource.com/1172292)  
Regress: [mjsunit/regress/regress-8033.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8033.js)  
```javascript
assertThrows("foo: if (true) do { continue foo } while (false)", SyntaxError);
assertThrows("foo: if (true) while (false) { continue foo }", SyntaxError);
assertThrows("foo: if (true) for (; false; ) { continue foo }", SyntaxError);
assertThrows("foo: if (true) for (let x of []) { continue foo }", SyntaxError);
assertThrows("foo: if (true) for (let x in []) { continue foo }", SyntaxError);

assertThrows("foo: if (true) { do { continue foo } while (false) }", SyntaxError);
assertThrows("foo: if (true) { while (false) { continue foo } }", SyntaxError);
assertThrows("foo: if (true) { for (; false; ) { continue foo } }", SyntaxError);
assertThrows("foo: if (true) { for (let x of []) { continue foo } }", SyntaxError);
assertThrows("foo: if (true) { for (let x in []) { continue foo } }", SyntaxError);

assertThrows("foo: goo: if (true) do { continue foo } while (false)", SyntaxError);
assertThrows("foo: goo: if (true) while (false) { continue foo }", SyntaxError);
assertThrows("foo: goo: if (true) for (; false; ) { continue foo }", SyntaxError);
assertThrows("foo: goo: if (true) for (let x of []) { continue foo }", SyntaxError);
assertThrows("foo: goo: if (true) for (let x in []) { continue foo }", SyntaxError);

assertThrows("foo: goo: if (true) { do { continue foo } while (false) }", SyntaxError);
assertThrows("foo: goo: if (true) { while (false) { continue foo } }", SyntaxError);
assertThrows("foo: goo: if (true) { for (; false; ) { continue foo } }", SyntaxError);
assertThrows("foo: goo: if (true) { for (let x of []) { continue foo } }", SyntaxError);
assertThrows("foo: goo: if (true) { for (let x in []) { continue foo } }", SyntaxError);

assertDoesNotThrow("if (true) foo: goo: do { continue foo } while (false)");
assertDoesNotThrow("if (true) foo: goo: while (false) { continue foo }");
assertDoesNotThrow("if (true) foo: goo: for (; false; ) { continue foo }");
assertDoesNotThrow("if (true) foo: goo: for (let x of []) { continue foo }");
assertDoesNotThrow("if (true) foo: goo: for (let x in []) { continue foo }");

assertThrows("if (true) foo: goo: { do { continue foo } while (false) }", SyntaxError);
assertThrows("if (true) foo: goo: { while (false) { continue foo } }", SyntaxError);
assertThrows("if (true) foo: goo: { for (; false; ) { continue foo } }", SyntaxError);
assertThrows("if (true) foo: goo: { for (let x of []) { continue foo } }", SyntaxError);
assertThrows("if (true) foo: goo: { for (let x in []) { continue foo } }", SyntaxError);

assertDoesNotThrow("if (true) { foo: goo: do { continue foo } while (false) }");
assertDoesNotThrow("if (true) { foo: goo: while (false) { continue foo } }");
assertDoesNotThrow("if (true) { foo: goo: for (; false; ) { continue foo } }");
assertDoesNotThrow("if (true) { foo: goo: for (let x of []) { continue foo } }");
assertDoesNotThrow("if (true) { foo: goo: for (let x in []) { continue foo } }");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/260af11^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=260af11)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=260af11)  
[src/ast/prettyprinter.h](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.h?cl=260af11)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=260af11)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=260af11)  
...  
  

---   

## **regress-crbug-871886.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadElements(object)) at ../../src/code-stub-assembler.](https://crbug.com/871886)**  
**[Commit: [csa] avoid FixedDoubleArray CAST on empty FixedArray](https://chromium.googlesource.com/v8/v8/+/5b74a7e)**  
  
Date(Commit): Thu Aug 09 10:00:25 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-70  
Code Review: [https://chromium-review.googlesource.com/1166906](https://chromium-review.googlesource.com/1166906)  
Regress: [mjsunit/regress/regress-crbug-871886.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-871886.js)  
```javascript
let arr = [1.5, 2.5];
arr.slice(0,
  { valueOf: function () {
      arr.length = 0;
      return 2;
    }
  });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5b74a7e^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=5b74a7e)  
[test/mjsunit/regress/regress-crbug-871886.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-871886.js?cl=5b74a7e)  
  

---   

## **regress-869735.js (chromium issue)**  
   
**[Issue: DCHECK failure in InOldSpace(object) || InNewSpace(object) || (lo_space()->Contains(object) && obj](https://crbug.com/869735)**  
**[Commit: [heap] Relax NotifyObjectLayoutChange DCHECK to allow ByteArrays changes in LO space](https://chromium.googlesource.com/v8/v8/+/a56d747)**  
  
Date(Commit): Mon Aug 06 06:42:35 2018  
Components: Blink>JavaScript  
Labels: Reproducible, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-70  
Code Review: [https://chromium-review.googlesource.com/1158065](https://chromium-review.googlesource.com/1158065)  
Regress: [mjsunit/regress/regress-869735.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-869735.js)  
```javascript
function f() {
  return arguments.length;
};
%PrepareFunctionForOptimization(f);
var a = [];
%OptimizeFunctionOnNextCall(f);
a.length = 81832;
f(...a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a56d747^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=a56d747)  
[test/mjsunit/regress/regress-869735.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-869735.js?cl=a56d747)  
  

---   

## **regress-866229.js (chromium issue)**  
   
**[Issue: CHECK failure: !descriptors->GetKey(i)->IsInterestingSymbol() in objects-debug.cc](https://crbug.com/866229)**  
**[Commit: [CloneObjectIC] copy may_have_interesting_symbols bit to fast result map](https://chromium.googlesource.com/v8/v8/+/7098f35)**  
  
Date(Commit): Sat Aug 04 16:48:18 2018  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-70, M-70  
Code Review: [https://chromium-review.googlesource.com/1162278](https://chromium-review.googlesource.com/1162278)  
Regress: [mjsunit/es9/regress/regress-866229.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-866229.js)  
```javascript
var obj = { length: 1, 0: "spread" };
obj[Symbol.toStringTag] = "foo";
obj[Symbol.hasInstance] = function() { return true; }
obj[Symbol.isConcatSpreadable] = true;

var obj2 = { ...obj };

%HeapObjectVerify(obj2);

assertEquals("[object foo]", Object.prototype.toString.call(obj2));
assertTrue(Uint8Array instanceof obj2);
assertEquals(["spread"], [].concat(obj2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7098f35^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=7098f35)  
[test/mjsunit/es9/regress/regress-866229.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-866229.js?cl=7098f35)  
  

---   

## **regress-crbug-869313.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadObjectField(data_view, JSDataView::kByteLengthOffse](https://crbug.com/869313)**  
**[Commit: [dataview] Fix too tight TNode type in DataView getters](https://chromium.googlesource.com/v8/v8/+/3656b46)**  
  
Date(Commit): Fri Aug 03 13:21:16 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-69, Test-Predator-Auto-CC, Test-Predator-Auto-Components, merge-merged-6.9  
Code Review: [https://chromium-review.googlesource.com/1158582](https://chromium-review.googlesource.com/1158582)  
Regress: [mjsunit/regress/regress-crbug-869313.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-869313.js)  
```javascript
function f() {
  try {
    var a = new ArrayBuffer(1073741824);
    var d = new DataView(a);
    return d.getUint8() === 0;
  } catch(e) {
    return true;
  }
}

!f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3656b46^!)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=3656b46)  
[src/builtins/builtins-data-view-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-data-view-gen.h?cl=3656b46)  
[src/builtins/data-view.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/data-view.tq?cl=3656b46)  
[test/mjsunit/regress/regress-crbug-869313.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-869313.js?cl=3656b46)  
  

---   

## **regress-869342.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::FeedbackNexus::ConfigureCloneObject](https://crbug.com/869342)**  
**[Commit: Reland "Reland [CloneObjectIC] overwrite monomorphic/polymorphic feedback if deprecated"](https://chromium.googlesource.com/v8/v8/+/5caee70)**  
  
Date(Commit): Wed Aug 01 00:30:11 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1154143](https://chromium-review.googlesource.com/1154143)  
Regress: [mjsunit/es9/regress/regress-869342.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-869342.js)  
```javascript
function spread(o) { return { ...o }; }

(function setupPolymorphicFeedback() {
  function C1() { this.p0 = 1; }
  function C2() { this.p1 = 2; this.p2 = 3; }
  assertEquals({ p0: 1 }, spread(new C1));
  assertEquals({ p1: 2, p2: 3 }, spread(new C2));
})();

gc(); // Clobber cached map in feedback[0], and check that we don't crash
function C3() { this.p0 = 3; }
assertEquals({ p0: 3 }, spread(new C3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5caee70^!)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=5caee70)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=5caee70)  
[test/mjsunit/es9/regress/regress-867958.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-867958.js?cl=5caee70)  
[test/mjsunit/es9/regress/regress-869342.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-869342.js?cl=5caee70)  
  

---   

## **regress-crbug-867776.js (chromium issue)**  
   
**[Issue: V8 OOB write BigInt64Array.of and BigInt64Array.from side effect neuter](https://crbug.com/867776)**  
**[Commit: [csa] Fix is-neutered check in EmitBigTypedArrayElementStore](https://chromium.googlesource.com/v8/v8/+/a24d5ad)**  
  
Date(Commit): Fri Jul 27 21:40:03 2018  
Components: Blink>JavaScript  
Labels: reward-5000, Security_Impact-Stable, reward-decline, Security_Severity-High, allpublic, reward-inprocess, NodeJS-Backport-Rejected, M-68, RegressedIn-68, Target-68, CVE_description-submitted, merge-merged-6.8, merge-merged-6.9, Release-0-M69, CVE-2018-16065  
Code Review: [https://chromium-review.googlesource.com/1153553](https://chromium-review.googlesource.com/1153553)  
Regress: [mjsunit/regress/regress-crbug-867776.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-867776.js)  
```javascript
for (var i = 0; i < 3; i++) {
  var array = new BigInt64Array(200);

  function evil_callback() {
    %ArrayBufferDetach(array.buffer);
    gc();
    return 1094795585n;
  }

  var evil_object = {valueOf: evil_callback};
  var root;
  try {
    root = BigInt64Array.of.call(function() { return array }, evil_object);
  } catch(e) {}
  gc();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a24d5ad^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=a24d5ad)  
[test/mjsunit/regress/regress-crbug-867776.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-867776.js?cl=a24d5ad)  
  

---   

## **regress-867958.js (chromium issue)**  
   
**[Issue: DCHECK failure in *source_map != *feedback in feedback-vector.cc](https://crbug.com/867958)**  
**[Commit: [CloneObjectIC] overwrite monomorphic/polymorphic feedback if deprecated](https://chromium.googlesource.com/v8/v8/+/670fa86)**  
  
Date(Commit): Fri Jul 27 19:37:39 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1152414](https://chromium-review.googlesource.com/1152414)  
Regress: [mjsunit/es9/regress/regress-867958.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-867958.js)  
```javascript
var obj1 = { x: 1 };
var obj2 = { x: 2 }; // same map
obj2.x = null; // deprecate map

function f() { return { ...obj1 } };
assertEquals({ x: 1 }, f()); // missed, object migrated to cached new map
assertEquals({ x: 1 }, f()); // monomorphic cache-hit  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/670fa86^!)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=670fa86)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=670fa86)  
[test/mjsunit/es9/regress/regress-867958.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es9/regress/regress-867958.js?cl=670fa86)  
  

---   

## **regress-866861.js (chromium issue)**  
   
**[Issue: DCHECK failure in PropertyConstness::kConst == details.constness() in objects.cc](https://crbug.com/866861)**  
**[Commit: [runtime] fix ClusterFuzz regressions (and remaining nits) in CloneObject](https://chromium.googlesource.com/v8/v8/+/d6efcbf)**  
  
Date(Commit): Wed Jul 25 21:23:05 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1146297](https://chromium-review.googlesource.com/1146297)  
Regress: [mjsunit/es9/regress/regress-866861.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-866861.js)  
```javascript
var o = {};
var toString = o.toString = function() {};
try {
assertEquals({ toString }, o = { ...o });
} catch (e) {}
o.toString = [];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6efcbf^!)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=d6efcbf)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=d6efcbf)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=d6efcbf)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d6efcbf)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=d6efcbf)  
...  
  

---   

## **regress-866727.js (chromium issue)**  
   
**[Issue: DCHECK failure in 2 == subnode->op()->ControlOutputCount() in js-inlining.cc](https://crbug.com/866727)**  
**[Commit: [runtime] fix ClusterFuzz regressions (and remaining nits) in CloneObject](https://chromium.googlesource.com/v8/v8/+/d6efcbf)**  
  
Date(Commit): Wed Jul 25 21:23:05 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, Target-70, M-70  
Code Review: [https://chromium-review.googlesource.com/1146297](https://chromium-review.googlesource.com/1146297)  
Regress: [mjsunit/es9/regress/regress-866727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-866727.js)  
```javascript
function test() {
  var spread = function(value) { return { ...value }; }
  try {
    assertEquals({}, spread());
  } catch (e) {}
};

%PrepareFunctionForOptimization(test);
test();
test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6efcbf^!)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=d6efcbf)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=d6efcbf)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=d6efcbf)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d6efcbf)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=d6efcbf)  
...  
  

---   

## **regress-866357.js (chromium issue)**  
   
**[Issue: DCHECK failure in UnusedPropertyFields() == map->UnusedPropertyFields() in map-inl.h](https://crbug.com/866357)**  
**[Commit: [runtime] fix ClusterFuzz regressions (and remaining nits) in CloneObject](https://chromium.googlesource.com/v8/v8/+/d6efcbf)**  
  
Date(Commit): Wed Jul 25 21:23:05 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner, M-70  
Code Review: [https://chromium-review.googlesource.com/1146297](https://chromium-review.googlesource.com/1146297)  
Regress: [mjsunit/es9/regress/regress-866357.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-866357.js)  
```javascript
var p = Promise.resolve();
var then = p.then = () => {};

function spread() { return { ...p }; }

%PrepareFunctionForOptimization(spread);
assertEquals({ then }, spread());
assertEquals({ then }, spread());
assertEquals({ then }, spread());
%OptimizeFunctionOnNextCall(spread);
assertEquals({ then }, spread());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6efcbf^!)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=d6efcbf)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=d6efcbf)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=d6efcbf)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d6efcbf)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=d6efcbf)  
...  
  

---   

## **regress-866282.js (chromium issue)**  
   
**[Issue: CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/866282)**  
**[Commit: [runtime] fix ClusterFuzz regressions (and remaining nits) in CloneObject](https://chromium.googlesource.com/v8/v8/+/d6efcbf)**  
  
Date(Commit): Wed Jul 25 21:23:05 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, Target-70, M-70  
Code Review: [https://chromium-review.googlesource.com/1146297](https://chromium-review.googlesource.com/1146297)  
Regress: [mjsunit/es9/regress/regress-866282.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es9/regress/regress-866282.js)  
```javascript
function spread(o) { return { ...o }; }

assertEquals({}, spread(new function C1() {}));
assertEquals({}, spread(new function C2() {}));
assertEquals({}, spread(new function C3() {}));
assertEquals({}, spread(new function C4() {}));
assertEquals({}, spread(new function C5() {}));

assertEquals({}, spread(undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6efcbf^!)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=d6efcbf)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=d6efcbf)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=d6efcbf)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d6efcbf)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=d6efcbf)  
...  
  

---   

## **regress-crbug-866315.js (chromium issue)**  
   
**[Issue: Ill in v8::Utils::ReportApiFailure](https://crbug.com/866315)**  
**[Commit: [async] Fix a crash when AsyncHooks is used in the proto of an object](https://chromium.googlesource.com/v8/v8/+/2d0a764)**  
  
Date(Commit): Mon Jul 23 14:34:59 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1146645](https://chromium-review.googlesource.com/1146645)  
Regress: [mjsunit/regress/regress-crbug-866315.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-866315.js)  
```javascript
let num = 42;
let ah = async_hooks.createHook({});

num.__proto__.__proto__ = ah;
assertThrows('num.enable()');
assertThrows('num.disable()');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d0a764^!)  
[src/async-hooks-wrapper.cc](https://cs.chromium.org/chromium/src/v8/src/async-hooks-wrapper.cc?cl=2d0a764)  
[src/async-hooks-wrapper.h](https://cs.chromium.org/chromium/src/v8/src/async-hooks-wrapper.h?cl=2d0a764)  
[test/mjsunit/regress/regress-crbug-866315.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-866315.js?cl=2d0a764)  
  

---   

## **regress-crbug-865892.js (chromium issue)**  
   
**[Issue: CHECK failure: !isolate->has_scheduled_exception() in builtins-console.cc](https://crbug.com/865892)**  
**[Commit: [async] Improve error handling when running async hooks](https://chromium.googlesource.com/v8/v8/+/4a28271)**  
  
Date(Commit): Mon Jul 23 13:34:50 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, M-69, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1146561](https://chromium-review.googlesource.com/1146561)  
Regress: [mjsunit/regress/regress-crbug-865892.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-865892.js)  
```javascript
let ah = async_hooks.createHook(
{
  init(asyncId, type) {
    if (type !== 'PROMISE') { return; }
    assertThrows('asyncIds.push(asyncId);');
  }
});
ah.enable();

async function foo() {
  let x = { toString() { return 'modules-skip-1.js' } };
  assertThrows('await import(x);');
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a28271^!)  
[src/async-hooks-wrapper.cc](https://cs.chromium.org/chromium/src/v8/src/async-hooks-wrapper.cc?cl=4a28271)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=4a28271)  
[test/mjsunit/regress/regress-crbug-865892.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-865892.js?cl=4a28271)  
  

---   

## **regress-865310.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,trusted](https://crbug.com/865310)**  
**[Commit: Reland "[turbofan] Inline Number constructor in certain cases"](https://chromium.googlesource.com/v8/v8/+/a2d6159)**  
  
Date(Commit): Mon Jul 23 13:17:19 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner, merge-merged-6.9  
Code Review: [https://chromium-review.googlesource.com/1118557](https://chromium-review.googlesource.com/1118557)  
Regress: [mjsunit/regress/regress-865310.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-865310.js)  
```javascript
check = function() {
  assertEquals(null, check.caller);
};

var obj = {};
obj.valueOf = check;

function f() {
  Number(obj);
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2d6159^!)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=a2d6159)  
[src/builtins/builtins-conversion-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-conversion-gen.cc?cl=a2d6159)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=a2d6159)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=a2d6159)  
[src/builtins/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.cc?cl=a2d6159)  
...  
  

---   

## **regress-crbug-865312.js (chromium issue)**  
   
**[Issue: DCHECK failure in end <= array->length_value() in elements.cc](https://crbug.com/865312)**  
**[Commit: [array] Only use fast-path in Array.p.fill for JSArrays](https://chromium.googlesource.com/v8/v8/+/b87e762)**  
  
Date(Commit): Thu Jul 19 12:15:42 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-69, Test-Predator-Auto-Owner, Target-69  
Code Review: [https://chromium-review.googlesource.com/1142772](https://chromium-review.googlesource.com/1142772)  
Regress: [mjsunit/regress/regress-crbug-865312.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-865312.js)  
```javascript
const intArrayConstructors = [
  Uint8Array,
  Int8Array,
  Uint16Array,
  Int16Array,
  Uint32Array,
  Int32Array,
  Uint8ClampedArray
];

const floatArrayConstructors = [
  Float32Array,
  Float64Array
];

const typedArrayConstructors = [...intArrayConstructors,
                                ...floatArrayConstructors];

for (let constructor of typedArrayConstructors) {
  // Shadowing the length of a TypedArray should work for Array.p.fill,
  // but not crash it.
  let array = new constructor([2, 2]);
  assertEquals(2, array.length);

  Object.defineProperty(array, 'length', {value: 5});
  Array.prototype.fill.call(array, 5);

  assertArrayEquals([5, 5], [array[0], array[1]]);
  assertEquals(undefined, array[2]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b87e762^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=b87e762)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=b87e762)  
[test/mjsunit/regress/regress-crbug-865312.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-865312.js?cl=b87e762)  
  

---   

## **regress-864509.js (chromium issue)**  
   
**[Issue: Liftoff must ensure that i32 stack parameters are zero extended](https://crbug.com/864509)**  
**[Commit: [Liftoff] Zero-extend i32 stack parameters](https://chromium.googlesource.com/v8/v8/+/16af1ba)**  
  
Date(Commit): Tue Jul 17 16:59:14 2018  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>WebAssembly  
Labels: Security_Severity-Medium, Security_Impact-Head, allpublic, M-69  
Code Review: [https://chromium-review.googlesource.com/1140168](https://chromium-review.googlesource.com/1140168)  
Regress: [mjsunit/regress/wasm/regress-864509.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-864509.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(1, 1);
builder.addFunction(undefined, kSig_v_i).addBody([
  kExprLocalGet, 0,        // get_local 0
  kExprI32Const, 0,        // i32.const 0
  kExprI32StoreMem, 0, 0,  // i32.store offset=0
]);
builder.addFunction(undefined, makeSig(new Array(6).fill(kWasmI32), []))
    .addBody([
      kExprLocalGet, 5,     // get_local 5
      kExprCallFunction, 0  // call 0
    ]);
const gen_i32_code = [
  kExprLocalTee, 0,  // tee_local 0
  kExprLocalGet, 0,  // get_local 0
  kExprI32Const, 1,  // i32.const 1
  kExprI32Add        // i32.add     --> 2nd param
];
builder.addFunction(undefined, kSig_v_v).addLocals({i32_count: 1}).addBody([
  // Generate six values on the stack, then six more to force the other six on
  // the stack.
  ...wasmI32Const(0),    // i32.const 0
  ...wasmI32Const(1),    // i32.const 1
  kExprI32Add,           // i32.add --> 1st param
  ...gen_i32_code,       // --> 2nd param
  ...gen_i32_code,       // --> 3rd param
  ...gen_i32_code,       // --> 4th param
  ...gen_i32_code,       // --> 5th param
  ...gen_i32_code,       // --> 6th param
  ...gen_i32_code,       // --> garbage
  ...gen_i32_code,       // --> garbage
  ...gen_i32_code,       // --> garbage
  ...gen_i32_code,       // --> garbage
  ...gen_i32_code,       // --> garbage
  ...gen_i32_code,       // --> garbage
  kExprDrop,             // drop garbage
  kExprDrop,             // drop garbage
  kExprDrop,             // drop garbage
  kExprDrop,             // drop garbage
  kExprDrop,             // drop garbage
  kExprDrop,             // drop garbage
  kExprCallFunction,  1  // call 1
]).exportAs('three');
const instance = builder.instantiate();
instance.exports.three();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/16af1ba^!)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=16af1ba)  
[test/mjsunit/regress/wasm/regress-864509.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-864509.js?cl=16af1ba)  
  

---   

## **regress-863810.js (chromium issue)**  
   
**[Issue: [turbofan] TruncateInt64ToInt32 must generate zero-extended value](https://crbug.com/863810)**  
**[Commit: [turbofan] lea32 must create zero-extended value](https://chromium.googlesource.com/v8/v8/+/b2b2583)**  
  
Date(Commit): Tue Jul 17 13:30:04 2018  
Components: Blink>JavaScript>Compiler, Blink>JavaScript>WebAssembly  
Labels: Security_Severity-Medium, Security_Impact-Head, allpublic, M-69  
Code Review: [https://chromium-review.googlesource.com/1137825](https://chromium-review.googlesource.com/1137825)  
Regress: [mjsunit/regress/regress-863810.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-863810.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('main', kSig_i_v)
  .addBody([
    kExprI64Const, 0xa3, 0x82, 0x83, 0x86, 0x8c, 0xd8, 0xae, 0xb5, 0x40,
    kExprI32ConvertI64,
    kExprI32Const, 0x00,
    kExprI32Sub,
  ]).exportFunc();
const instance = builder.instantiate();
print(instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b2b2583^!)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=b2b2583)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=b2b2583)  
[test/mjsunit/regress/regress-863810.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-863810.js?cl=b2b2583)  
  

---   

## **regress-tonumbercode.js (other issue)**  
   
**[Commit: [turbofan] Inline Number constructor in certain cases](https://chromium.googlesource.com/v8/v8/+/9eca23e)**  
  
Date(Commit): Mon Jul 16 10:02:42 2018  
Code Review: [https://chromium-review.googlesource.com/1118557](https://chromium-review.googlesource.com/1118557)  
Regress: [mjsunit/harmony/bigint/regress-tonumbercode.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/bigint/regress-tonumbercode.js)  
```javascript
function f(x, b) {
    if (b) return Math.trunc(+(x))
    else return Math.trunc(Number(x))
}

%PrepareFunctionForOptimization(f);
f("1", true);
f("2", true);
f("2", false);
%OptimizeFunctionOnNextCall(f);
f(3n);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9eca23e^!)  
[src/builtins/builtins-conversion-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-conversion-gen.cc?cl=9eca23e)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=9eca23e)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=9eca23e)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=9eca23e)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=9eca23e)  
...  
  
  
---   

## **regress-852765.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::PatternRewriter::VisitImportCallExpression](https://crbug.com/852765)**  
**[Commit: [parser] Fix import in arrow function parameters.](https://chromium.googlesource.com/v8/v8/+/f128ace)**  
  
Date(Commit): Mon Jul 16 07:57:19 2018  
Components: Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Libfuzzer, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, ClusterFuzz-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/1128875](https://chromium-review.googlesource.com/1128875)  
Regress: [mjsunit/regress/regress-852765.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-852765.js)  
```javascript
assertThrows("(import(foo)) =>", SyntaxError, "Invalid destructuring assignment target");

assertThrows("import(foo) =>", SyntaxError, "Malformed arrow function parameter list");
assertThrows("(a, import(foo)) =>", SyntaxError, "Invalid destructuring assignment target");
assertThrows("(1, import(foo)) =>", SyntaxError, "Invalid destructuring assignment target");
assertThrows("(super(foo)) =>", SyntaxError, "'super' keyword unexpected here");
assertThrows("(bar(foo)) =>", SyntaxError, "Invalid destructuring assignment target");

assertThrows("[import(foo).then] = [1];", ReferenceError, "foo is not defined");
assertThrows("[[import(foo).then]] = [[1]];", ReferenceError, "foo is not defined");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f128ace^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=f128ace)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=f128ace)  
[test/mjsunit/regress/regress-852765.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-852765.js?cl=f128ace)  
  

---   

## **regress-863155.js (chromium issue)**  
   
**[Issue: DCHECK failure in AllowHandleAllocation::IsAllowed() in handles-inl.h](https://crbug.com/863155)**  
**[Commit: [turbofan] Add a few missing AllowHandleAllocation scopes.](https://chromium.googlesource.com/v8/v8/+/1319680)**  
  
Date(Commit): Fri Jul 13 12:51:04 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1136444](https://chromium-review.googlesource.com/1136444)  
Regress: [mjsunit/regress/regress-863155.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-863155.js)  
```javascript
for (let i = 0; i < 5; i++) {
  try { typeof x } catch (e) {};
  let x;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1319680^!)  
[src/compiler/js-heap-broker.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-heap-broker.cc?cl=1319680)  
[test/mjsunit/regress/regress-863155.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-863155.js?cl=1319680)  
  

---   

## **regress-862433.js (chromium issue)**  
   
**[Issue: CHECK failure: object->IsAbstractCode() || object->IsSeqString() || object->IsExternalString() ](https://crbug.com/862433)**  
**[Commit: [runtime] Allow FeedbackMetadata objects in old space for verification](https://chromium.googlesource.com/v8/v8/+/a0dbaf5)**  
  
Date(Commit): Thu Jul 12 12:55:28 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, M-67, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1134995](https://chromium-review.googlesource.com/1134995)  
Regress: [mjsunit/regress/regress-862433.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-862433.js)  
```javascript
var arr = [];
for (var i = 1; i != 390000; ++i) {
  arr.push("f()");
}
new Function(arr.join());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0dbaf5^!)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=a0dbaf5)  
[test/mjsunit/regress/regress-862433.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-862433.js?cl=a0dbaf5)  
  

---   

## **regress-crbug-862538.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::ScannerStream::For](https://crbug.com/862538)**  
**[Commit: [scanner] Fix scanner stream creation: Sliced strings can have an underlying thin string.](https://chromium.googlesource.com/v8/v8/+/ae044d69)**  
  
Date(Commit): Thu Jul 12 10:32:47 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1133980](https://chromium-review.googlesource.com/1133980)  
Regress: [mjsunit/regress/regress-crbug-862538.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-862538.js)  
```javascript
var uvwxyzundefined;

function __f_5778(__v_29973) {
  var __v_29975 = __v_29973 + undefined;
  var __v_29976 = __v_29975.substring( 20);
  ({})[__v_29975];
  return eval(__v_29976);
}
__f_5778("abcdefghijklmnopqrstuvwxyz");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae044d69^!)  
[src/parsing/scanner-character-streams.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner-character-streams.cc?cl=ae044d69)  
[test/mjsunit/regress/regress-crbug-862538.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-862538.js?cl=ae044d69)  
  

---   

## **regress-crbug-860788.js (chromium issue)**  
   
**[Issue: CHECK failure: !isolate->has_scheduled_exception() in builtins-console.cc](https://crbug.com/860788)**  
**[Commit: [async] Implement error handling when running async hooks](https://chromium.googlesource.com/v8/v8/+/614c807)**  
  
Date(Commit): Tue Jul 10 08:12:09 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-69, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1128750](https://chromium-review.googlesource.com/1128750)  
Regress: [mjsunit/regress/regress-crbug-860788.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-860788.js)  
```javascript
try {
Object.prototype.__defineGetter__(0, function(){});
assertThrows("x");
} catch(e) { print("Caught: " + e); }

try {
(function() {
  let asyncIds = [], triggerIds = [];
  let ah = async_hooks.createHook({
    init(asyncId, type, triggerAsyncId, resource) {
      if (type !== 'PROMISE') { return; }
      assertThrows("asyncIds.push(asyncId);");
      assertThrows("triggerIds.push(triggerAsyncId)");
    },
  });
  ah.enable();
  async function foo() {}
  foo();
})();
} catch(e) { print("Caught: " + e); }
try {
  var obj = {prop: 7};
  assertThrows("nonexistent(obj)");
} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/614c807^!)  
[src/async-hooks-wrapper.cc](https://cs.chromium.org/chromium/src/v8/src/async-hooks-wrapper.cc?cl=614c807)  
[test/mjsunit/regress/regress-crbug-860788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-860788.js?cl=614c807)  
  

---   

## **regress-crbug-849024.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/849024)**  
**[Commit: [builtins] Add reference error for global object property access](https://chromium.googlesource.com/v8/v8/+/d8f0237)**  
  
Date(Commit): Thu Jul 05 09:52:48 2018  
Components: Blink>JavaScript>Runtime  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, M-67, M-66, M-68, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/1124446](https://chromium-review.googlesource.com/1124446)  
Regress: [mjsunit/regress/regress-crbug-849024.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-849024.js)  
```javascript
this.__proto__ = new Proxy({}, {});

function test1() {
  eval("bla");
}

assertThrows(test1, ReferenceError);
assertThrows(test1, ReferenceError);
assertThrows(test1, ReferenceError);

function test2() {
  gc();
  eval("bla");
}

assertThrows(test2, ReferenceError);
assertThrows(test2, ReferenceError);
assertThrows(test2, ReferenceError);

function foo() {
  try {
    eval("bla");
  } catch(e) {
    return;
  }
  throw 1337;
}

function test3() {
  gc();
  foo();
  foo();
}

assertDoesNotThrow(test3);
assertDoesNotThrow(test3);
assertDoesNotThrow(test3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8f0237^!)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=d8f0237)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=d8f0237)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=d8f0237)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=d8f0237)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d8f0237)  
...  
  

---   

## **regress-854011.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::wasm::fuzzer::WasmExecutionFuzzer::FuzzWasmModule](https://crbug.com/854011)**  
**[Commit: [Liftoff][arm64] Fix i64 constants passed via stack](https://chromium.googlesource.com/v8/v8/+/720218c)**  
  
Date(Commit): Tue Jul 03 17:04:49 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/1124683](https://chromium-review.googlesource.com/1124683)  
Regress: [mjsunit/regress/wasm/regress-854011.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-854011.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('main', kSig_d_d)
    .addBody([
      // Call with param 0 (converted to i64), to fill the stack with non-zero
      // values.
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 0
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 1
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 2
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 3
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 4
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 5
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 6
      kExprLocalGet, 0, kExprI64SConvertF64,  // arg 7
      kExprCallFunction, 1,                   // call #1
      // Now call with 0 constants.
      // The bug was that they were written out as i32 values, thus the upper 32
      // bit were the previous values on that stack memory.
      kExprI64Const, 0,      // i64.const 0  [0]
      kExprI64Const, 0,      // i64.const 0  [1]
      kExprI64Const, 0,      // i64.const 0  [2]
      kExprI64Const, 0,      // i64.const 0  [3]
      kExprI64Const, 0,      // i64.const 0  [4]
      kExprI64Const, 0,      // i64.const 0  [5]
      kExprI64Const, 0,      // i64.const 0  [6]
      kExprI64Const, 0,      // i64.const 0  [7]
      kExprCallFunction, 1,  // call #1
      // Return the sum of the two returned values.
      kExprF64Add
    ])
    .exportFunc();
builder.addFunction(undefined, makeSig(new Array(8).fill(kWasmI64), [kWasmF64]))
    .addBody([
      kExprLocalGet, 7,     // get_local 7 (last parameter)
      kExprF64SConvertI64,  // f64.convert_s/i64
    ]);
const instance = builder.instantiate();
const big_num_1 = 2 ** 48;
const big_num_2 = 2 ** 56 / 3;
assertEquals(big_num_1, instance.exports.main(big_num_1));
assertEquals(big_num_2, instance.exports.main(big_num_2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/720218c^!)  
[src/wasm/baseline/arm64/liftoff-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm64/liftoff-assembler-arm64.h?cl=720218c)  
[test/mjsunit/regress/wasm/regress-854011.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-854011.js?cl=720218c)  
  

---   

## **regress-7914.js (v8 issue)**  
   
**[Issue: [Liftoff][arm64] i32.popcnt produces wrong result](https://crbug.com/v8/7914)**  
**[Commit: [wasm] Add regression test for issue 7914](https://chromium.googlesource.com/v8/v8/+/ca4a8f9)**  
  
Date(Commit): Tue Jul 03 17:03:37 2018  
Code Review: [https://chromium-review.googlesource.com/1124687](https://chromium-review.googlesource.com/1124687)  
Regress: [mjsunit/regress/wasm/regress-7914.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7914.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false);
builder.addFunction('main', kSig_i_v)
    .addBody([
      ...wasmI32Const(10000),  // i32.const 10000
      kExprMemoryGrow, 0,      // grow_memory --> -1
      kExprI32Popcnt,          // i32.popcnt  --> 32
    ])
    .exportFunc();
const instance = builder.instantiate();
assertEquals(32, instance.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca4a8f9^!)  
[test/mjsunit/regress/wasm/regress-7914.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7914.js?cl=ca4a8f9)  
  

---   

## **regress-crbug-859809.js (chromium issue)**  
   
**[Issue: DCHECK failure in !object->IsFiller() in mark-compact.cc](https://crbug.com/859809)**  
**[Commit: [array] Add regression test that causes left trimming while sorting](https://chromium.googlesource.com/v8/v8/+/26ac072)**  
  
Date(Commit): Tue Jul 03 14:16:14 2018  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1124475](https://chromium-review.googlesource.com/1124475)  
Regress: [mjsunit/regress/regress-crbug-859809.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-859809.js)  
```javascript
let xs = [];
const kSize = 200;
for (let i = 0; i < kSize; ++i) {
  xs.push(i);
}

let counter = 0;
xs.sort((a, b) => {
  if (counter++ % 10 == 0) {
    xs.shift();
    gc();
  }

  return a - b;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/26ac072^!)  
[test/mjsunit/regress/regress-crbug-859809.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-859809.js?cl=26ac072)  
  

---   

## **regress-crbug-856095.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/856095)**  
**[Commit: Fix overzealous assert in CallOrConstructVarArgs](https://chromium.googlesource.com/v8/v8/+/34225a6)**  
  
Date(Commit): Tue Jul 03 03:42:20 2018  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/1117922](https://chromium-review.googlesource.com/1117922)  
Regress: [mjsunit/regress/regress-crbug-856095.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-856095.js)  
```javascript
function f(a, b, c) { }
function a() {
    let o1;
    o1 = new Array();
    f(...o1);
    o1[1000] = Infinity;
}

a();
a();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34225a6^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=34225a6)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=34225a6)  
[src/arm64/macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.cc?cl=34225a6)  
[src/arm64/macro-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.h?cl=34225a6)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=34225a6)  
...  
  

---   

## **regress-7893.js (v8 issue)**  
   
**[Issue: Asm.js numeric literal parsing bug](https://crbug.com/v8/7893)**  
**[Commit: [asmjs] Fix parsing hex numeric literals ending with 'e'.](https://chromium.googlesource.com/v8/v8/+/d683fd7)**  
  
Date(Commit): Mon Jul 02 11:52:18 2018  
Code Review: [https://chromium-review.googlesource.com/1116551](https://chromium-review.googlesource.com/1116551)  
Regress: [mjsunit/regress/regress-7893.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7893.js)  
```javascript
function Module(stdlib, imports, buffer) {
  "use asm";
  function f() {
    var bar = 0;
    return 0x1e+bar|0;
  }
  return f;
}
var f = Module(this);
assertTrue(%IsWasmCode(f));
assertTrue(%IsAsmWasmCode(Module));
assertEquals(0x1e, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d683fd7^!)  
[AUTHORS](https://cs.chromium.org/chromium/src/v8/AUTHORS?cl=d683fd7)  
[src/asmjs/asm-scanner.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.cc?cl=d683fd7)  
[test/mjsunit/regress/regress-7893.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7893.js?cl=d683fd7)  
  

---   

## **regress-crbug-7907.js (v8 issue)**  
   
**[Issue: Array.p.sort crashing on read-only elements](https://crbug.com/v8/7907)**  
**[Commit: [array] Fix read-only property in NumberDictionary fast-path](https://chromium.googlesource.com/v8/v8/+/327668d)**  
  
Date(Commit): Fri Jun 29 10:40:35 2018  
Code Review: [https://chromium-review.googlesource.com/1119910](https://chromium-review.googlesource.com/1119910)  
Regress: [mjsunit/regress/regress-crbug-7907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-7907.js)  
```javascript
let arr = new Array(10);
Object.defineProperty(arr, 0, {value: 10, writable: false});
Object.defineProperty(arr, 9, {value: 1, writable: false});

assertThrows(() => arr.sort(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/327668d^!)  
[src/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-sort.tq?cl=327668d)  
[test/mjsunit/regress/regress-crbug-7907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-7907.js?cl=327668d)  
  

---   

## **regress-854050.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_used(reg) in liftoff-assembler.h](https://crbug.com/854050)**  
**[Commit: [Liftoff] Fix register use count](https://chromium.googlesource.com/v8/v8/+/ada6480)**  
  
Date(Commit): Fri Jun 22 15:40:52 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1111958](https://chromium-review.googlesource.com/1111958)  
Regress: [mjsunit/regress/wasm/regress-854050.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-854050.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, makeSig([kWasmI32, kWasmF32], []))
    .addLocals({i32_count: 7})
    .addBody([
      kExprLocalGet,    0,          // get_local
      kExprI32Const,    0,          // i32.const 0
      kExprIf,          kWasmStmt,  // if
      kExprUnreachable,             // unreachable
      kExprEnd,                     // end if
      kExprLocalGet,    4,          // get_local
      kExprLocalTee,    8,          // tee_local
      kExprBrIf,        0,          // br_if depth=0
      kExprLocalTee,    7,          // tee_local
      kExprLocalTee,    0,          // tee_local
      kExprLocalTee,    2,          // tee_local
      kExprLocalTee,    8,          // tee_local
      kExprDrop,                    // drop
      kExprLoop,        kWasmStmt,  // loop
      kExprEnd,                     // end loop
    ]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ada6480^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=ada6480)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=ada6480)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=ada6480)  
[test/mjsunit/regress/wasm/regress-854050.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-854050.js?cl=ada6480)  
  

---   

## **regress-853453.js (chromium issue)**  
   
**[Issue: DCHECK failure in (has_shared_memory) != nullptr in module-decoder.cc](https://crbug.com/853453)**  
**[Commit: [wasm] Catch invalid flags correctly](https://chromium.googlesource.com/v8/v8/+/f2b90bd)**  
  
Date(Commit): Fri Jun 22 15:06:39 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-0, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1104976](https://chromium-review.googlesource.com/1104976)  
Regress: [mjsunit/regress/wasm/regress-853453.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-853453.js)  
```javascript
assertThrows(() => new WebAssembly.Module(
    new Uint8Array([
      0x00, 0x61, 0x73, 0x6d,     // wasm magic
      0x01, 0x00, 0x00, 0x00,     // wasm version
      0x04,                       // section code
      0x04,                       // section length
      /* Section: Table */
      0x01,                       // table count
      0x70,                       // table type
      0x03,                       // resizable limits flags
      0x00])),
    WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f2b90bd^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=f2b90bd)  
[test/mjsunit/regress/wasm/regress-853453.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-853453.js?cl=f2b90bd)  
  

---   

## **regress-854066-2.js (chromium issue)**  
   
**[Issue: Security: OOB read in TypedArray.from](https://crbug.com/854066)**  
**[Commit: [typedarray] Use slow case more aggressively in CopyElementsHandleImpl](https://chromium.googlesource.com/v8/v8/+/bededee)**  
  
Date(Commit): Thu Jun 21 12:14:18 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, M-67, Merge-Rejected-67, merge-merged-6.8, Hotlist-Torque, Release-0-M68  
Code Review: [https://chromium-review.googlesource.com/1108203](https://chromium-review.googlesource.com/1108203)  
Regress: [mjsunit/regress/regress-854066-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-854066-2.js), [mjsunit/regress/regress-854066.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-854066.js)  
```javascript
oobArray = [];
delete oobArray.__proto__[Symbol.iterator];
for (let i = 0; i < 1e5; ++i) {
  oobArray[i] = 1.1;
}
floatArray = new Float64Array(oobArray.length);
Float64Array.from.call(function(length) {
  oobArray.length = 0;
  return floatArray;
}, oobArray);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bededee^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=bededee)  
[test/mjsunit/regress/regress-854066-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-854066-2.js?cl=bededee)  
[test/mjsunit/regress/regress-854066.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-854066.js?cl=bededee)  
  

---   

## **regress-crbug-854299.js (chromium issue)**  
   
**[Issue: Security: OOB read in Array.prototype.sort](https://crbug.com/854299)**  
**[Commit: [array] Change Array.p.sort bailout behavior from fast- to slow-path](https://chromium.googlesource.com/v8/v8/+/3bcf2b8)**  
  
Date(Commit): Wed Jun 20 15:38:18 2018  
Components: Blink>JavaScript  
Labels: reward-4000, Security_Impact-Head, Security_Severity-High, allpublic, reward-inprocess, ClusterFuzz-Verified, M-69, Target-69, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1107702](https://chromium-review.googlesource.com/1107702)  
Regress: [mjsunit/regress/regress-crbug-854299.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-854299.js)  
```javascript
let rand = n => Math.floor(Math.random() * n);

for (let i = 0; i < 1000; ++i) {
  array = [];
  let len = rand(30);
  for(let i = 0; i < len; ++i) {
    array[i] = [i + 0.1];
  }

  let counter = 0;
  array.sort((a, b) => {
    a = a || [0];
    b = b || [0];

    if (counter++ == rand(30)) {
      array.length = 1;
      gc();
    }
    return a[0] - b[0];
  });
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3bcf2b8^!)  
[src/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-sort.tq?cl=3bcf2b8)  
[test/mjsunit/regress/regress-crbug-854299.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-854299.js?cl=3bcf2b8)  
  

---   

## **regress-crbug-852592.js (chromium issue)**  
   
**[Issue: Security: OOB read/write in Array.prototype.sort](https://crbug.com/852592)**  
**[Commit: [array] Fix OOB load/stores when underlying FixedArray changed](https://chromium.googlesource.com/v8/v8/+/ce3c006)**  
  
Date(Commit): Tue Jun 19 05:19:44 2018  
Components: Blink>JavaScript  
Labels: Security_Impact-Head, Security_Severity-High, reward-7500, allpublic, reward-inprocess, M-69, Target-69, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1104158](https://chromium-review.googlesource.com/1104158)  
Regress: [mjsunit/regress/regress-crbug-852592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-852592.js)  
```javascript
const kArraySize = 1024;

let array = [];
for (let i = 1; i < kArraySize; ++i) {
  array[i] = i + 0.1;
}

assertEquals(array.length, kArraySize);

let executed = false;
compareFn = _ => {
  if (!executed) {
    executed = true;

    array.length = 1; // shrink
    array.length = 0; // replace
    array.length = kArraySize; // restore the original length
  }
}

array.sort(compareFn);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce3c006^!)  
[src/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-sort.tq?cl=ce3c006)  
[test/mjsunit/regress/regress-crbug-852592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-852592.js?cl=ce3c006)  
  

---   

## **regress-crbug-851393.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::Runtime_SetDataProperties](https://crbug.com/851393)**  
**[Commit: [builtins] Relax type check in a slow path of Object.assign.](https://chromium.googlesource.com/v8/v8/+/412ec75)**  
  
Date(Commit): Mon Jun 18 14:37:38 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-MemorySanitizer, Clusterfuzz, ClusterFuzz-Verified, M-69, M-68, Test-Predator-Auto-Owner, merge-merged-6.8  
Code Review: [https://chromium-review.googlesource.com/1104317](https://chromium-review.googlesource.com/1104317)  
Regress: [mjsunit/regress/regress-crbug-851393.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-851393.js)  
```javascript
var proxy = new Proxy({}, {});

Object.assign(proxy, { b: "boom", 060: "ah", o: "ouch" });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/412ec75^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=412ec75)  
[test/mjsunit/regress/regress-crbug-851393.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-851393.js?cl=412ec75)  
  

---   

## **regress-crbug-848165.js (chromium issue)**  
   
**[Issue: enumeration_index out-of-bound](https://crbug.com/848165)**  
**[Commit: Properly set enumeration order for accessor properties in class literals.](https://chromium.googlesource.com/v8/v8/+/e602c90)**  
  
Date(Commit): Mon Jun 18 12:45:02 2018  
Components: Blink>JavaScript  
Labels: allpublic, ClusterFuzz-Verified, Via-Wizard-Security, M-67, M-68, Target-67  
Code Review: [https://chromium-review.googlesource.com/1104175](https://chromium-review.googlesource.com/1104175)  
Regress: [mjsunit/regress/regress-crbug-848165.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-848165.js)  
```javascript
class cls0 {
  static get length(){ return 42; };
  static get [1](){ return 21; };
};
Object.defineProperty(cls0, "length", {value:'1'});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e602c90^!)  
[src/objects/literal-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.cc?cl=e602c90)  
[test/mjsunit/regress/regress-crbug-848165.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-848165.js?cl=e602c90)  
  

---   

## **regress-852258.js (chromium issue)**  
   
**[Issue: JSTypedArray ByteLength out of bounds](https://crbug.com/852258)**  
**[Commit: [typedarray] Fix incorrect access to typed array byte offset.](https://chromium.googlesource.com/v8/v8/+/d69df91)**  
  
Date(Commit): Fri Jun 15 08:26:41 2018  
Components: Blink>JavaScript  
Labels: reward-0, Security_Severity-Low, Security_Impact-None, allpublic, Via-Wizard-Security, M-67, Target-67, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1100886](https://chromium-review.googlesource.com/1100886)  
Regress: [mjsunit/regress/regress-852258.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-852258.js)  
```javascript
try {
  let ta0 = new Int16Array(0x24924925);
  let ta2 = ta0.slice(1);
  let ta1 = ta0.slice(0x24924924);
} catch (e) {
  // Allocation failed, that's fine.
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d69df91^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=d69df91)  
[test/mjsunit/regress/regress-852258.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-852258.js?cl=d69df91)  
  

---   

## **regress-crbug-850005.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(var_elements.value()) at ../../src/builtins/builtins-ca](https://crbug.com/850005)**  
**[Commit: [CSA] Fix assertion in CallOrConstructDoubleVarargs with empty FixedArray](https://chromium.googlesource.com/v8/v8/+/cb29d62)**  
  
Date(Commit): Wed Jun 06 11:01:11 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1088608](https://chromium-review.googlesource.com/1088608)  
Regress: [mjsunit/regress/regress-crbug-850005.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-850005.js)  
```javascript
let args = [3.34, ];
function f(a, b, c) {};
f(...args);
args = args.splice();
f(...args);
args = [];
f(...args);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb29d62^!)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=cb29d62)  
[test/mjsunit/regress/regress-crbug-850005.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-850005.js?cl=cb29d62)  
  

---   

## **regress-849663.js (chromium issue)**  
   
**[Issue: DCHECK failure in x <= INT_MAX in conversions.h](https://crbug.com/849663)**  
**[Commit: [date] Fix double-to-int conversion in MakeDay](https://chromium.googlesource.com/v8/v8/+/eca6c5b)**  
  
Date(Commit): Tue Jun 05 16:15:20 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/1087063](https://chromium-review.googlesource.com/1087063)  
Regress: [mjsunit/regress/regress-849663.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-849663.js)  
```javascript
const v1 = 0xFFFFFFFF;
const v3 = new Float64Array();
new Date(v3, v3, 0xFFFFFFFF,);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eca6c5b^!)  
[src/builtins/builtins-date.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-date.cc?cl=eca6c5b)  
[test/mjsunit/regress/regress-849663.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-849663.js?cl=eca6c5b)  
  

---   

## **regress-799952.js (chromium issue)**  
   
**[Issue: Ill in v8::Utils::ReportApiFailure](https://crbug.com/799952)**  
**[Commit: [wasm] Add missing WebAssembly.instantiate regression test.](https://chromium.googlesource.com/v8/v8/+/f4b2323)**  
  
Date(Commit): Tue May 29 10:37:32 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Wrong-CLs, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1075967](https://chromium-review.googlesource.com/1075967)  
Regress: [mjsunit/regress/wasm/regress-799952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-799952.js)  
```javascript
var sentinel = {};
Object.defineProperty(Promise, Symbol.species, {
  value: function(f) {
    f(function() {}, function() {})
    return sentinel;
  }
});

var promise = WebAssembly.instantiate(new ArrayBuffer());
assertInstanceof(promise, Promise);
assertNotSame(promise, sentinel);

var monkey = promise.then(r => { print(r) }, e => { print(e) });
assertSame(monkey, sentinel);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f4b2323^!)  
[test/mjsunit/regress/wasm/regress-799952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-799952.js?cl=f4b2323)  
  

---   

## **regress-7791.js (v8 issue)**  
   
**[Issue: Declaring a data property with the same name as a previously-declared accessor property creates broken property](https://crbug.com/v8/7791)**  
**[Commit: Fix bug in object literals with redeclarations.](https://chromium.googlesource.com/v8/v8/+/21eb202)**  
  
Date(Commit): Mon May 28 13:00:07 2018  
Code Review: [https://chromium-review.googlesource.com/1075054](https://chromium-review.googlesource.com/1075054)  
Regress: [mjsunit/regress/regress-7791.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7791.js)  
```javascript
"use strict";



{
  const o = {
    get foo() { return 666 },
    foo: 42,
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').value);
}

{
  const o = {
    set foo(_) { },
    foo: 42,
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').value);
}

{
  const o = {
    get foo() { return 666 },
    set foo(_) { },
    foo: 42,
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').value);
}

{
  const o = {
    get foo() { return 666 },
    set ['foo'.slice()](_) { },
    foo: 42,
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').value);
}

{
  const o = {
    get ['foo'.slice()]() { return 666 },
    set ['foo'.slice()](_) { },
    foo: 42,
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').value);
}



{
  const o = {
    foo: 666,
    get foo() { return 42 },
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').get());
}

{
  const o = {
    foo: 666,
    set foo(_) { },
  };
  assertEquals(undefined, Object.getOwnPropertyDescriptor(o, 'foo').get);
  assertEquals(undefined, Object.getOwnPropertyDescriptor(o, 'foo').value);
}

{
  const o = {
    foo: 666,
    get foo() { return 42 },
    set foo(_) { },
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').get());
}

{
  const o = {
    foo: 666,
    get ['foo'.slice()]() { return 42 },
    set foo(_) { },
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').get());
}

{
  const o = {
    foo: 666,
    get ['foo'.slice()]() { return 42 },
    set ['foo'](_) { },
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').get());
}



{
  const o = {
    get foo() { return 42 },
    foo: 666,
    set foo(_) { },
  };
  assertEquals(undefined, Object.getOwnPropertyDescriptor(o, 'foo').get);
  assertEquals(undefined, Object.getOwnPropertyDescriptor(o, 'foo').set());
}

{
  const o = {
    set foo(_) { },
    foo: 666,
    get foo() { return 42 },
  };
  assertEquals(42, Object.getOwnPropertyDescriptor(o, 'foo').get());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21eb202^!)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=21eb202)  
[test/mjsunit/regress/regress-7791.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7791.js?cl=21eb202)  
  

---   

## **regress-7785.js (v8 issue)**  
   
**[Issue: Wasm serialization/deserialization breaks {null} references](https://crbug.com/v8/7785)**  
**[Commit: [wasm] Avoid embedding {null} values in WasmCode.](https://chromium.googlesource.com/v8/v8/+/fabb514)**  
  
Date(Commit): Fri May 25 08:33:06 2018  
Code Review: [https://chromium-review.googlesource.com/1070981](https://chromium-review.googlesource.com/1070981)  
Regress: [mjsunit/regress/wasm/regress-7785.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7785.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function testAnyRefNull() {
  const builder = new WasmModuleBuilder();
  builder.addFunction('main', kSig_r_v)
      .addBody([kExprRefNull])
      .exportFunc();

  var wire_bytes = builder.toBuffer();
  var module = new WebAssembly.Module(wire_bytes);
  var buffer = %SerializeWasmModule(module);
  module = %DeserializeWasmModule(buffer, wire_bytes);
  var instance = new WebAssembly.Instance(module);

  assertEquals(null, instance.exports.main());
})();

(function testAnyRefIsNull() {
  const builder = new WasmModuleBuilder();
  builder.addFunction('main', kSig_i_r)
      .addBody([kExprLocalGet, 0, kExprRefIsNull])
      .exportFunc();

  var wire_bytes = builder.toBuffer();
  var module = new WebAssembly.Module(wire_bytes);
  var buffer = %SerializeWasmModule(module);
  module = %DeserializeWasmModule(buffer, wire_bytes);
  var instance = new WebAssembly.Instance(module);

  assertEquals(0, instance.exports.main({'hello' : 'world'}));
  assertEquals(0, instance.exports.main(1234));
  assertEquals(0, instance.exports.main(0));
  assertEquals(0, instance.exports.main(123.4));
  assertEquals(0, instance.exports.main(undefined));
  assertEquals(1, instance.exports.main(null));
  assertEquals(0, instance.exports.main(print));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fabb514^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=fabb514)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=fabb514)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=fabb514)  
[src/wasm/wasm-objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects-inl.h?cl=fabb514)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=fabb514)  
...  
  

---   

## **regress-843062-1.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/843062)**  
**[Commit: [runtime] Do not shrink fixed arrays to length 0.](https://chromium.googlesource.com/v8/v8/+/5a0ebc8)**  
  
Date(Commit): Thu May 24 09:41:00 2018  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/1064052](https://chromium-review.googlesource.com/1064052)  
Regress: [mjsunit/regress/regress-843062-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-843062-1.js), [mjsunit/regress/regress-843062-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-843062-2.js), [mjsunit/regress/regress-843062-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-843062-3.js)  
```javascript
var sparse_array = [];

sparse_array[100] = 3;
sparse_array[200] = undefined;
sparse_array[300] = 4;
sparse_array[400] = 5;
sparse_array[500] = 6;
sparse_array[600] = 5;
sparse_array[700] = 4;
sparse_array[800] = undefined;
sparse_array[900] = 3
sparse_array[41999] = "filler";

sparse_array.lastIndexOf(3, 99);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5a0ebc8^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=5a0ebc8)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=5a0ebc8)  
[src/debug/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug.cc?cl=5a0ebc8)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=5a0ebc8)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=5a0ebc8)  
...  
  

---   

## **regress-843563.js (chromium issue)**  
   
**[Issue: [wasm] Shared js-to-wasm wrappers call to instance-specific wasm-to-js wrapper](https://crbug.com/843563)**  
**[Commit: [wasm] Call imports via import table in js-to-wasm wrappers](https://chromium.googlesource.com/v8/v8/+/71c0545)**  
  
Date(Commit): Fri May 18 12:56:26 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Security_Impact-Head, Security_Severity-High, allpublic, ClusterFuzz-Verified, M-68  
Code Review: [https://chromium-review.googlesource.com/1064610](https://chromium-review.googlesource.com/1064610)  
Regress: [mjsunit/regress/wasm/regress-843563.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-843563.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
sig1 = makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
const imp_idx = builder.addImport('q', 'imp', kSig_i_i);
builder.addExport('exp', imp_idx);
const module = builder.toModule();

function bad(a, b, c, d, e, f, g, h) {
    print(JSON.stringify([a, b, c, d, e, f, g, h]));
}
const instance1 = new WebAssembly.Instance(module, {q: {imp: bad}});
const instance2 = new WebAssembly.Instance(module, {q: {imp: i => i}});

print(instance1.exports.exp(5));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/71c0545^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=71c0545)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=71c0545)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=71c0545)  
[test/mjsunit/regress/wasm/regress-843563.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-843563.js?cl=71c0545)  
  

---   

## **regress-843543.js (chromium issue)**  
   
**[Issue: Security: OOB reads due to missing map check](https://crbug.com/843543)**  
**[Commit: [turbofan] Add missing check in JSCallReducer](https://chromium.googlesource.com/v8/v8/+/f651409)**  
  
Date(Commit): Wed May 16 14:01:30 2018  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security, Security_Severity-Medium, Security_Impact-Beta, allpublic, M-67, merge-merged-6.7, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1061463](https://chromium-review.googlesource.com/1061463)  
Regress: [mjsunit/regress/regress-843543.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-843543.js)  
```javascript
const o = {
  x: 9
};
o.__proto__ = Array.prototype;

function foo(o) {
  return o.indexOf(undefined);
};
%PrepareFunctionForOptimization(foo);
assertEquals(-1, foo(o));
assertEquals(-1, foo(o));
%OptimizeFunctionOnNextCall(foo);
assertEquals(-1, foo(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f651409^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=f651409)  
[test/mjsunit/regress/regress-843543.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-843543.js?cl=f651409)  
  

---   

## **regress-crbug-843022.js (chromium issue)**  
   
**[Issue: Security: OOB access in RegExpBuiltinsAssembler::LoadRegExpResultFirstMatch](https://crbug.com/843022)**  
**[Commit: [regexp] Do not assume fast regexp results are non-empty](https://chromium.googlesource.com/v8/v8/+/5999f8f)**  
  
Date(Commit): Wed May 16 13:06:14 2018  
Components: Blink>JavaScript>Regexp  
Labels: Hotlist-Merge-Review, reward-2000, Security_Impact-Stable, Security_Severity-Medium, allpublic, reward-inprocess, FoundIn-66, merge-merged-6.7, CVE_description-submitted, Release-0-M67, CVE-2018-6143, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1061455](https://chromium-review.googlesource.com/1061455)  
Regress: [mjsunit/regress/regress-crbug-843022.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-843022.js)  
```javascript
const fast_regexp_result = /./g.exec("a");
fast_regexp_result.length = 0;
class RegExpWithFastResult extends RegExp {
  constructor() { super(".", "g"); this.number_of_runs = 0; }
  exec(str) { return (this.number_of_runs++ == 0) ? fast_regexp_result : null; }
}

const slow_regexp_result = [];
class RegExpWithSlowResult extends RegExp {
  constructor() { super(".", "g"); this.number_of_runs = 0; }
  exec(str) { return (this.number_of_runs++ == 0) ? slow_regexp_result : null; }
}

assertEquals(["undefined"], "a".match(new RegExpWithFastResult()));
assertEquals(["undefined"], "a".match(new RegExpWithSlowResult()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5999f8f^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=5999f8f)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=5999f8f)  
[test/mjsunit/regress/regress-crbug-843022.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-843022.js?cl=5999f8f)  
  

---   

## **regress-842612.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/842612)**  
**[Commit: Fix array.indexOf for negative fromIndex](https://chromium.googlesource.com/v8/v8/+/be5cfb2)**  
  
Date(Commit): Wed May 16 07:31:46 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner, merge-merged-6.7  
Code Review: [https://chromium-review.googlesource.com/1059628](https://chromium-review.googlesource.com/1059628)  
Regress: [mjsunit/regress/regress-842612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-842612.js)  
```javascript
var arr = [undefined];

function f() {
  assertEquals(0, arr.indexOf(undefined, -1));
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/be5cfb2^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=be5cfb2)  
[test/mjsunit/regress/regress-842612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-842612.js?cl=be5cfb2)  
  

---   

## **regress-842501.js (chromium issue)**  
   
**[Issue: Stack-buffer-overflow in v8::internal::compiler::VisitBinop](https://crbug.com/842501)**  
**[Commit: [turbofan] Binop Instructions can have up to 5 input operands](https://chromium.googlesource.com/v8/v8/+/1b11d98)**  
  
Date(Commit): Mon May 14 10:38:47 2018  
Components: Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Stability-Libfuzzer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-68, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/1056989](https://chromium-review.googlesource.com/1056989)  
Regress: [mjsunit/regress/wasm/regress-842501.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-842501.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  const builder = new WasmModuleBuilder();
  builder.addMemory(16, 32);
  // Generate function 1 (out of 1).
  sig1 = makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
  builder.addFunction(undefined, sig1)
    .addBodyWithEnd([
      // signature: i_iii
      // body:
      kExprI32Const, 0xe1, 0xc8, 0xd5, 0x01,
      kExprI32Const, 0xe2, 0xe4, 0x00,
      kExprI32Sub,
      kExprF32Const, 0x00, 0x00, 0x00, 0x00,
      kExprF32Const, 0xc9, 0xc9, 0xc9, 0x00,
      kExprF32Eq,
      kExprI32LoadMem, 0x01, 0xef, 0xec, 0x95, 0x93, 0x07,
      kExprI32Add,
      kExprIf, kWasmStmt,   // @30
      kExprEnd,             // @32
      kExprI32Const, 0xc9, 0x93, 0xdf, 0xcc, 0x7c,
      kExprEnd,             // @39
    ]);
  builder.addExport('main', 0);
  const instance = builder.instantiate();
  assertTraps(kTrapMemOutOfBounds, _ => instance.exports.main(1, 2, 3));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1b11d98^!)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=1b11d98)  
[test/mjsunit/regress/wasm/regress-842501.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-842501.js?cl=1b11d98)  
  

---   

## **regress-7740.js (v8 issue)**  
   
**[Issue: hitting int3 in optimized code](https://crbug.com/v8/7740)**  
**[Commit: [compiler] Fix bug in representation changer.](https://chromium.googlesource.com/v8/v8/+/fc36cac)**  
  
Date(Commit): Mon May 14 10:16:22 2018  
Code Review: [https://chromium-review.googlesource.com/1054670](https://chromium-review.googlesource.com/1054670)  
Regress: [mjsunit/regress/regress-7740.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7740.js)  
```javascript
var x = 0;
x = 42;

function foo(a, b) {
  let y = a < a;
  if (b) x = y;
};
%PrepareFunctionForOptimization(foo);
foo(1, false);
foo(1, false);
%OptimizeFunctionOnNextCall(foo);
foo(1, true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc36cac^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=fc36cac)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=fc36cac)  
[test/mjsunit/regress/regress-7740.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7740.js?cl=fc36cac)  
  

---   

## **regress-842078.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::String::MakeExternal](https://crbug.com/842078)**  
**[Commit: [objects] Disallow externalizing RO_SPACE 2-byte strings](https://chromium.googlesource.com/v8/v8/+/fad99f5)**  
  
Date(Commit): Fri May 11 12:37:55 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Stability-Memory-MemorySanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1054290](https://chromium-review.googlesource.com/1054290)  
Regress: [mjsunit/regress/regress-842078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-842078.js)  
```javascript
assertThrows(() => {
  externalizeString("1", false)
});
assertThrows(() => {
  externalizeString("1", true)
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fad99f5^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=fad99f5)  
[test/mjsunit/regress/regress-842078.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-842078.js?cl=fad99f5)  
  

---   

## **regress-crbug-841592.js (chromium issue)**  
   
**[Issue: Crash in IntToSmi<31>](https://crbug.com/841592)**  
**[Commit: [elements] Avoid NOP operation when shrinking HashTables](https://chromium.googlesource.com/v8/v8/+/0b4b14b)**  
  
Date(Commit): Thu May 10 11:09:59 2018  
Components: Blink>JavaScript>API, Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1052693](https://chromium-review.googlesource.com/1052693)  
Regress: [mjsunit/regress/regress-crbug-841592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-841592.js)  
```javascript
a = [];

a.length = 0xFFFFFFF;

a.length = 0;

a.length = 0xFFFFFFF;

a.length = 1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b4b14b^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=0b4b14b)  
[test/mjsunit/regress/regress-crbug-841592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-841592.js?cl=0b4b14b)  
  

---   

## **regress-v8-7725.js (v8 issue)**  
   
**[Issue: Object.assign seems to freeze when assigning to a proxy with a set trap](https://crbug.com/v8/7725)**  
**[Commit: [builtins] Properly handle non-simple target in Object.assign.](https://chromium.googlesource.com/v8/v8/+/09d4ba0)**  
  
Date(Commit): Wed May 09 13:44:00 2018  
Code Review: [https://chromium-review.googlesource.com/1051653](https://chromium-review.googlesource.com/1051653)  
Regress: [mjsunit/regress/regress-v8-7725.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-7725.js)  
```javascript
var proxy = new Proxy({}, {});

Object.assign(proxy, { b: "boom", a: "ah", o: "ouch" });
assertEquals(["b", "a", "o"], Object.getOwnPropertyNames(proxy));
assertEquals("boom", proxy.b);
assertEquals("ah", proxy.a);
assertEquals("ouch", proxy.o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09d4ba0^!)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=09d4ba0)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=09d4ba0)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=09d4ba0)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=09d4ba0)  
[src/ic/keyed-store-generic.h](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.h?cl=09d4ba0)  
...  
  

---   

## **regress-841117.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,trusted](https://crbug.com/841117)**  
**[Commit: [turbofan] Fix NumberFloor typing.](https://chromium.googlesource.com/v8/v8/+/d520ebb)**  
  
Date(Commit): Wed May 09 07:32:46 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1051228](https://chromium-review.googlesource.com/1051228)  
Regress: [mjsunit/compiler/regress-841117.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-841117.js)  
```javascript
var v = 1e9;
function f() { return Math.floor(v / 10); }
%PrepareFunctionForOptimization(f);
assertEquals(1e8, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(1e8, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d520ebb^!)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=d520ebb)  
[test/mjsunit/compiler/regress-841117.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-841117.js?cl=d520ebb)  
  

---   

## **regress-840757.js (chromium issue)**  
   
**[Issue: Segfault for --code-comments with --print-wasm-code](https://crbug.com/840757)**  
**[Commit: Fix SourcePositionInfo for wasm](https://chromium.googlesource.com/v8/v8/+/e084eea)**  
  
Date(Commit): Tue May 08 18:23:04 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/1049632](https://chromium-review.googlesource.com/1049632)  
Regress: [mjsunit/regress/wasm/regress-840757.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-840757.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


const builder = new WasmModuleBuilder();
builder.addFunction('foo', kSig_v_v).addBody([]);
builder.addFunction('test', kSig_v_v).addBody([kExprCallFunction, 0]);

builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e084eea^!)  
[src/perf-jit.cc](https://cs.chromium.org/chromium/src/v8/src/perf-jit.cc?cl=e084eea)  
[src/profiler/profiler-listener.cc](https://cs.chromium.org/chromium/src/v8/src/profiler/profiler-listener.cc?cl=e084eea)  
[src/source-position.cc](https://cs.chromium.org/chromium/src/v8/src/source-position.cc?cl=e084eea)  
[src/source-position.h](https://cs.chromium.org/chromium/src/v8/src/source-position.h?cl=e084eea)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=e084eea)  
...  
  

---   

## **regress-7716.js (v8 issue)**  
   
**[Issue: Stack overflow: Reflect.set using proxy chained object](https://crbug.com/v8/7716)**  
**[Commit: [proxies] Add missing stack overflow check.](https://chromium.googlesource.com/v8/v8/+/e91cd3c)**  
  
Date(Commit): Mon May 07 18:50:09 2018  
Code Review: [https://chromium-review.googlesource.com/1047612](https://chromium-review.googlesource.com/1047612)  
Regress: [mjsunit/regress/regress-7716.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7716.js)  
```javascript
let proxy = new Proxy(function(){}, {});
for (let i = 0; i < 100000; i++) {
  proxy = new Proxy(proxy, {});
}

try { Reflect.apply(proxy, {}, []) } catch(_) {}
try { Reflect.construct(proxy, []) } catch(_) {}
try { Reflect.defineProperty(proxy, "x", {}) } catch(_) {}
try { Reflect.deleteProperty(proxy, "x") } catch(_) {}
try { Reflect.get(proxy, "x") } catch(_) {}
try { Reflect.getOwnPropertyDescriptor(proxy, "x") } catch(_) {}
try { Reflect.getPrototypeOf(proxy) } catch(_) {}
try { Reflect.has(proxy, "x") } catch(_) {}
try { Reflect.isExtensible(proxy) } catch(_) {}
try { Reflect.ownKeys(proxy) } catch(_) {}
try { Reflect.preventExtensions(proxy) } catch(_) {}
try { Reflect.setPrototypeOf(proxy, {}) } catch(_) {}
try { Reflect.set(proxy, "x", {}) } catch(_) {}



function run(trap, ...args) {
  let handler = {};
  const proxy = new Proxy(function(){}, handler);
  handler[trap] = (target, ...args) => Reflect[trap](proxy, ...args);
  return Reflect[trap](proxy, ...args);
}

assertThrows(() => run("apply", {}, []), RangeError);
assertThrows(() => run("construct", []), RangeError);
assertThrows(() => run("defineProperty", "x", {}), RangeError);
assertThrows(() => run("deleteProperty", "x"), RangeError);
assertThrows(() => run("get", "x"), RangeError);
assertThrows(() => run("getOwnPropertyDescriptor", "x"), RangeError);
assertThrows(() => run("has", "x"), RangeError);
assertThrows(() => run("isExtensible"), RangeError);
assertThrows(() => run("ownKeys"), RangeError);
assertThrows(() => run("preventExtensions"), RangeError);
assertThrows(() => run("setPrototypeOf", {}), RangeError);
assertThrows(() => run("set", "x", {}), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e91cd3c^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=e91cd3c)  
[test/mjsunit/regress/regress-7716.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7716.js?cl=e91cd3c)  
[test/mjsunit/regress/regress-crbug-779344.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-779344.js?cl=e91cd3c)  
  

---   

## **regress-840106.js (chromium issue)**  
   
**[Issue: Security: heap-use-after-free in TypedArrayBuiltinsAssembler::ConstructByArrayLike](https://crbug.com/840106)**  
**[Commit: [typedarrays] Throw on construction of a detached typed array.](https://chromium.googlesource.com/v8/v8/+/645efbf)**  
  
Date(Commit): Mon May 07 15:30:48 2018  
Components: Blink>JavaScript  
Labels: Security_Impact-Head, Security_Severity-High, reward-7500, allpublic, reward-inprocess, M-68, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1046827](https://chromium-review.googlesource.com/1046827)  
Regress: [mjsunit/regress/regress-840106.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-840106.js)  
```javascript
var buffer = new ArrayBuffer(1024 * 1024);
buffer.constructor = {
  [Symbol.species]: new Proxy(function() {}, {
    get: _ => {
      %ArrayBufferDetach(buffer);
    }
  })
};
var array1 = new Uint8Array(buffer, 0, 1024);
assertThrows(() => new Uint8Array(array1));
assertThrows(() => new Int8Array(array1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/645efbf^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=645efbf)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=645efbf)  
[test/mjsunit/es6/typedarray-construct-by-array-like.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/typedarray-construct-by-array-like.js?cl=645efbf)  
[test/mjsunit/regress/regress-707410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-707410.js?cl=645efbf)  
[test/mjsunit/regress/regress-840106.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-840106.js?cl=645efbf)  
...  
  

---   

## **regress-crbug-840220.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(TypedArraySpeciesConstructor(context, exemplar)) at ../](https://crbug.com/840220)**  
**[Commit: [CSA] Remove overzealous type check](https://chromium.googlesource.com/v8/v8/+/7235c851)**  
  
Date(Commit): Mon May 07 11:20:56 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1046066](https://chromium-review.googlesource.com/1046066)  
Regress: [mjsunit/regress/regress-crbug-840220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-840220.js)  
```javascript
var boundFunction = (function(){}).bind();

var instance = new Uint8Array()
instance.constructor = {
  [Symbol.species]: boundFunction
};

assertThrows(() => instance.map(each => each * 2), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7235c851^!)  
[src/builtins/builtins-typed-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typed-array-gen.cc?cl=7235c851)  
[test/mjsunit/regress/regress-crbug-840220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-840220.js?cl=7235c851)  
  

---   

## **regress-crbug-823130.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/823130)**  
**[Commit: Fix "x is not iterable" error message consistency](https://chromium.googlesource.com/v8/v8/+/45a2d9c)**  
  
Date(Commit): Thu May 03 23:13:21 2018  
Components: Blink>JavaScript>GC  
Labels: Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1042951](https://chromium-review.googlesource.com/1042951)  
Regress: [mjsunit/regress/regress-crbug-823130.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-823130.js)  
```javascript
var __v_1 = new Array();
var __v_2 = 0x30;
var __v_4 = "abc";
var __v_3 = "def";

function __f_2(b) {
  [...b];
}
__f_2([1]);
__f_2([3.3]);
__f_2([{}]);

var vars = [__v_1, __v_2, __v_3, __v_4];

for (var j = 0; j < vars.length && j < 7; j++) {
  for (var k = j; k < vars.length && k < 7 + j; k++) {
    var v1 = vars[j];
    var e1, e2;
    try {
      __f_2(v1);
      __f_2();
    } catch (e) {
      e1 = "" + e;
    }
    gc();
    try {
      __f_2(v1);
      __f_2();
    } catch (e) {
      e2 = "" + e;
    }
    assertEquals(e1, e2);
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/45a2d9c^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=45a2d9c)  
[test/mjsunit/regress/regress-crbug-823130.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-823130.js?cl=45a2d9c)  
  

---   

## **regress-838766.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/838766)**  
**[Commit: [turbofan] Fix wrong optimization of Number.parseInt](https://chromium.googlesource.com/v8/v8/+/d9c9b00)**  
  
Date(Commit): Wed May 02 12:24:07 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1039197](https://chromium-review.googlesource.com/1039197)  
Regress: [mjsunit/regress/regress-838766.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-838766.js)  
```javascript
function foo(x) {
  x = x | 2147483648;
  return Number.parseInt(x + 65535, 8);
};
%PrepareFunctionForOptimization(foo);
assertEquals(-72161, foo());
assertEquals(-72161, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(-72161, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9c9b00^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=d9c9b00)  
[src/compiler/type-cache.h](https://cs.chromium.org/chromium/src/v8/src/compiler/type-cache.h?cl=d9c9b00)  
[test/mjsunit/regress/regress-838766.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-838766.js?cl=d9c9b00)  
  

---   

## **regress-crbug-837939.js (chromium issue)**  
   
**[Issue: Security: [v8] Information Leak in Map constructor](https://crbug.com/837939)**  
**[Commit: Do not throw if the array is empty in Map constructor](https://chromium.googlesource.com/v8/v8/+/c77c869)**  
  
Date(Commit): Wed May 02 12:03:26 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-Medium, reward-4500, allpublic, reward-inprocess, M-67, FoundIn-66, merge-merged-6.7, CVE_description-submitted, Release-0-M67, CVE-2018-6142, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1034043](https://chromium-review.googlesource.com/1034043)  
Regress: [mjsunit/es6/regress/regress-crbug-837939.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-837939.js)  
```javascript
const iterable = [123.123];
assertTrue(%HasDoubleElements(iterable))

iterable.length = 0;
assertTrue(%HasDoubleElements(iterable))

let map = new Map(iterable);
assertEquals(0, map.size);
new WeakMap(iterable); // WeakMap does not have a size  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c77c869^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=c77c869)  
[test/mjsunit/es6/regress/regress-crbug-837939.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-crbug-837939.js?cl=c77c869)  
  

---   

## **regress-834624.js (chromium issue)**  
   
**[Issue: DCHECK failure in !trap_handler::IsThreadInWasm() in wasm-interpreter.cc](https://crbug.com/834624)**  
**[Commit: [wasm][interpreter] Clear thread in wasm flag on exceptional return](https://chromium.googlesource.com/v8/v8/+/9286358)**  
  
Date(Commit): Mon Apr 30 17:13:19 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-68, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1019570](https://chromium-review.googlesource.com/1019570)  
Regress: [mjsunit/regress/wasm/regress-834624.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-834624.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let instance;
(function DoTest() {
  function call_main() {
    instance.exports.main();
  }
  let module = new WasmModuleBuilder();
  module.addImport('mod', 'func', kSig_v_i);
  module.addFunction('main', kSig_v_i)
    .addBody([kExprLocalGet, 0, kExprCallFunction, 0])
    .exportFunc();
  instance = module.instantiate({
    mod: {
      func: call_main
    }
  });
  try {
    instance.exports.main();
  } catch (e) {
    // ignore
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9286358^!)  
[src/wasm/wasm-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-interpreter.cc?cl=9286358)  
[test/mjsunit/regress/wasm/regress-834624.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-834624.js?cl=9286358)  
  

---   

## **regress-7706.js (v8 issue)**  
   
**[Issue: Accessing symbol on primitive wrapper subclass changes its apparent toStringTag](https://crbug.com/v8/7706)**  
**[Commit: [objects] fix forced slow path in MigrateSlowToFast](https://chromium.googlesource.com/v8/v8/+/a7e6b0e)**  
  
Date(Commit): Sun Apr 29 11:59:57 2018  
Code Review: [https://chromium-review.googlesource.com/1034210](https://chromium-review.googlesource.com/1034210)  
Regress: [mjsunit/es6/regress/regress-7706.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-7706.js)  
```javascript
function toString(o) {
  %ToFastProperties(o.__proto__);
  return Object.prototype.toString.call(o);
}

class TestNumber extends Number {}
TestNumber.prototype[Symbol.toStringTag] = "TestNumber";
assertEquals("[object TestNumber]", toString(new TestNumber), "Try #1");
assertEquals("[object TestNumber]", toString(new TestNumber), "Try #2");

class TestBoolean extends Boolean {}
TestBoolean.prototype[Symbol.toStringTag] = "TestBoolean";
assertEquals("[object TestBoolean]", toString(new TestBoolean), "Try #1");
assertEquals("[object TestBoolean]", toString(new TestBoolean), "Try #2");

class TestString extends String {}
TestString.prototype[Symbol.toStringTag] = "TestString";
assertEquals("[object TestString]", toString(new TestString), "Try #1");
assertEquals("[object TestString]", toString(new TestString), "Try #2");

class base {}
class TestBigInt extends base {}
TestBigInt.prototype[Symbol.toStringTag] = 'TestBigInt';
var b = new TestBigInt();
b.__proto__.__proto__ = BigInt.prototype;
assertEquals("[object TestBigInt]", toString(b), "Try #1");
assertEquals("[object TestBigInt]", toString(b), "Try #2");

class TestSymbol extends base {}
TestSymbol.prototype[Symbol.toStringTag] = 'TestSymbol';
var sym = new TestSymbol();
sym.__proto__.__proto__ = Symbol.prototype;
assertEquals("[object TestSymbol]", toString(sym), "Try #1");
assertEquals("[object TestSymbol]", toString(sym), "Try #2");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7e6b0e^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a7e6b0e)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=a7e6b0e)  
[test/mjsunit/es6/regress/regress-7706.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-7706.js?cl=a7e6b0e)  
  

---   

## **regress-837417.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::wasm::InstantiateToInstanceObject](https://crbug.com/837417)**  
**[Commit: [wasm] Do an additional IsWasmModuleObject check during instantiation](https://chromium.googlesource.com/v8/v8/+/441e6d4)**  
  
Date(Commit): Fri Apr 27 17:34:05 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, NodeJS-Backport-Approved, Test-Predator-Auto-Components, merge-merged-6.6, merge-merged-6.7, Release-2-M66  
Code Review: [https://chromium-review.googlesource.com/1032392](https://chromium-review.googlesource.com/1032392)  
Regress: [mjsunit/regress/wasm/regress-837417.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-837417.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addFunction("test", kSig_i_v).addBody([
  kExprI32Const, 12,         // i32.const 12
]);

WebAssembly.Module.prototype.then = resolve => {
  assertUnreachable();
};

WebAssembly.instantiate(builder.toBuffer());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/441e6d4^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=441e6d4)  
[test/mjsunit/regress/wasm/regress-836141.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-836141.js?cl=441e6d4)  
[test/mjsunit/regress/wasm/regress-837417.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-837417.js?cl=441e6d4)  
  

---   

## **regress-834619.js (chromium issue)**  
   
**[Issue: DCHECK failure in func_index == code->index() in wasm-code-manager.cc](https://crbug.com/834619)**  
**[Commit: [wasm] Fix target instance for indirect calls to imports](https://chromium.googlesource.com/v8/v8/+/903d873)**  
  
Date(Commit): Fri Apr 27 08:27:56 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-67, Test-Predator-Auto-Owner, merge-merged-6.7  
Code Review: [https://chromium-review.googlesource.com/1030174](https://chromium-review.googlesource.com/1030174)  
Regress: [mjsunit/regress/wasm/regress-834619.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-834619.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function ExportedFunctionsImportedOrder() {
  print(arguments.callee.name);

  let i1 = (() => {
    let builder = new WasmModuleBuilder();
    builder.addFunction("f1", kSig_i_v)
      .addBody(
        [kExprI32Const, 1])
      .exportFunc();
    builder.addFunction("f2", kSig_i_v)
      .addBody(
        [kExprI32Const, 2])
      .exportFunc();
    return builder.instantiate();
  })();

  let i2 = (() => {
    let builder = new WasmModuleBuilder();
    builder.addImport("q", "f2", kSig_i_v);
    builder.addImport("q", "f1", kSig_i_v);
    builder.addTable(kWasmAnyFunc, 4);
    builder.addFunction("main", kSig_i_i)
      .addBody([
        kExprLocalGet, 0,
        kExprCallIndirect, 0, kTableZero
      ])
      .exportFunc();
    builder.addElementSegment(0, 0, false, [0, 1, 1, 0]);

    return builder.instantiate({q: {f2: i1.exports.f2, f1: i1.exports.f1}});
  })();

  print("--->calling 0");
  assertEquals(2, i2.exports.main(0));
  print("--->calling 1");
  assertEquals(1, i2.exports.main(1));
  print("--->calling 2");
  assertEquals(1, i2.exports.main(2));
  print("--->calling 3");
  assertEquals(2, i2.exports.main(3));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/903d873^!)  
[src/wasm/function-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-compiler.cc?cl=903d873)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=903d873)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=903d873)  
[test/mjsunit/regress/wasm/regress-834619.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-834619.js?cl=903d873)  
[test/mjsunit/wasm/indirect-tables.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/indirect-tables.js?cl=903d873)  
  

---   

## **regress-836141.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::wasm::InstantiateToInstanceObject](https://crbug.com/836141)**  
**[Commit: [wasm] Call AsyncInstantiate directly when instantiating a module object](https://chromium.googlesource.com/v8/v8/+/49712d8)**  
  
Date(Commit): Tue Apr 24 13:01:18 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, NodeJS-Backport-Done, M-67, M-66, M-68, merge-merged-6.6, merge-merged-6.7, CVE_description-missing, Release-1-M66, CVE-2018-6122  
Code Review: [https://chromium-review.googlesource.com/1025774](https://chromium-review.googlesource.com/1025774)  
Regress: [mjsunit/regress/wasm/regress-836141.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-836141.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addFunction("test", kSig_i_v).addBody([
  kExprI32Const, 12,         // i32.const 0
]);

let module = new WebAssembly.Module(builder.toBuffer());
module.then = () => {
  // Use setTimeout to get out of the promise chain.
  setTimeout(assertUnreachable);
};

WebAssembly.instantiate(module);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49712d8^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=49712d8)  
[test/mjsunit/regress/wasm/regress-836141.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-836141.js?cl=49712d8)  
  

---   

## **regress-crbug-830565.js (chromium issue)**  
   
**[Issue: Promise never resolves/rejects when thenable throws before callback](https://crbug.com/830565)**  
**[Commit: [builtins] Properly reject immediately throwing thenables.](https://chromium.googlesource.com/v8/v8/+/7f8e83b)**  
  
Date(Commit): Tue Apr 24 07:55:00 2018  
Components: Blink>JavaScript>Runtime  
Labels: Hotlist-Merge-Review, Hotlist-Merge-Approved, NodeJS-Backport-Rejected, Via-Wizard-Javascript, merge-merged-6.6, merge-merged-6.7  
Code Review: [https://chromium-review.googlesource.com/1023400](https://chromium-review.googlesource.com/1023400)  
Regress: [mjsunit/regress/regress-crbug-830565.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-830565.js)  
```javascript
load('test/mjsunit/test-async.js');

testAsync(assert => {
  assert.plan(1);
  const error = new TypeError('Throwing');
  Promise.resolve({ then(resolve, reject) {
    throw error;
  }}).then(v => {
    assert.unreachable();
  }, e => {
    assert.equals(error, e);
  });
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7f8e83b^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=7f8e83b)  
[test/mjsunit/regress/regress-crbug-830565.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-830565.js?cl=7f8e83b)  
[test/webkit/fast/js/Promise-resolve-with-then-exception-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/js/Promise-resolve-with-then-exception-expected.txt?cl=7f8e83b)  
  

---   

## **regress-834693.js (chromium issue)**  
   
**[Issue: Crash in Call](https://crbug.com/834693)**  
**[Commit: [wasm] Register trap handler data for lazily compiled functions](https://chromium.googlesource.com/v8/v8/+/94139bc)**  
  
Date(Commit): Mon Apr 23 18:30:24 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-68, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1020361](https://chromium-review.googlesource.com/1020361)  
Regress: [mjsunit/regress/wasm/regress-834693.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-834693.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

var module = new WasmModuleBuilder();
module.addMemory();
module.addFunction("main", kSig_v_v)
  .addBody([
    kExprI32Const, 20,
    kExprI32Const, 29,
    kExprMemoryGrow, kMemoryZero,
    kExprI32StoreMem, 0, 0xFF, 0xFF, 0x7A])
  .exportAs("main");
var instance = module.instantiate();
assertTraps(kTrapMemOutOfBounds, instance.exports.main);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/94139bc^!)  
[src/runtime/runtime-wasm.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-wasm.cc?cl=94139bc)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=94139bc)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=94139bc)  
[src/wasm/wasm-code-manager.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.h?cl=94139bc)  
[test/mjsunit/regress/wasm/regress-834693.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-834693.js?cl=94139bc)  
  

---   

## **regress-v8-7682.js (v8 issue)**  
   
**[Issue: Array.prototype.sort with an Array-like object which has accessor properties does not work.](https://crbug.com/v8/7682)**  
**[Commit: Add regression test for crbug.com/v8/7682](https://chromium.googlesource.com/v8/v8/+/7b4286b)**  
  
Date(Commit): Mon Apr 23 10:58:15 2018  
Code Review: [https://chromium-review.googlesource.com/1023394](https://chromium-review.googlesource.com/1023394)  
Regress: [mjsunit/regress/regress-v8-7682.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-7682.js)  
```javascript
const impl = Symbol();
class MyArrayLike {
  constructor() {
    this[impl] = [2, 1];
    Object.freeze(this);
  }
  get 0() { return this[impl][0]; }
  set 0(value) { this[impl][0] = value; }
  get 1() { return this[impl][1]; }
  set 1(value) { this[impl][1] = value; }
  get length() { return 2; }
}

const xs = new MyArrayLike();
Array.prototype.sort.call(xs);

assertEquals(1, xs[0]);
assertEquals(2, xs[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b4286b^!)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=7b4286b)  
[test/mjsunit/regress/regress-v8-7682.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-7682.js?cl=7b4286b)  
  

---   

## **regress-7677.js (v8 issue)**  
   
**[Issue: Array.prototype.fill has an extra check which prevents to use for Array-like objects](https://crbug.com/v8/7677)**  
**[Commit: Remove incorrect receiver checks from some array methods.](https://chromium.googlesource.com/v8/v8/+/021e9b0)**  
  
Date(Commit): Mon Apr 23 08:57:35 2018  
Code Review: [https://chromium-review.googlesource.com/1021870](https://chromium-review.googlesource.com/1021870)  
Regress: [mjsunit/regress/regress-7677.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7677.js)  
```javascript
"use strict";

function arraylike(freeze) {
  let x;
  const obj = {length: 42};
  Object.defineProperty(obj, 5, {get() {return x}, set(y) {x = y}});
  return freeze ? Object.freeze(obj) : Object.seal(obj);
}

{
  const sealed = arraylike(false);
  Array.prototype.fill.call(sealed, "foo", 5, 6);
  assertEquals("foo", sealed[5]);
  assertThrows(() => Array.prototype.fill.call(sealed, "foo"), TypeError);
}{
  const frozen = arraylike(true);
  Array.prototype.fill.call(frozen, "foo", 5, 6);
  assertEquals("foo", frozen[5]);
  assertThrows(() => Array.prototype.fill.call(frozen, "foo"), TypeError);
}

{
  const sealed = Object.seal({length: 0});
  assertEquals(undefined, Array.prototype.shift.call(sealed));
}{
  const sealed = Object.seal({length: 42});
  assertEquals(undefined, Array.prototype.shift.call(sealed));
}{
  let x;
  let obj = {length: 42, [1]: "foo"};
  Object.defineProperty(obj, 0, {get() {return x}, set(y) {x = y}});
  const sealed = Object.seal(obj);
  assertThrows(() => Array.prototype.shift.call(sealed), TypeError);
  assertEquals("foo", sealed[0]);
}{
  const frozen = Object.freeze({length: 0});
  assertThrows(() => Array.prototype.shift.call(frozen), TypeError);
}

{
  const sealed = arraylike(false);
  assertEquals([undefined], Array.prototype.splice.call(sealed, 5, 1, "foo"));
  assertEquals("foo", sealed[5]);
  assertThrows(() => Array.prototype.splice.call(sealed, 5, 0, "bar"),
      TypeError);
  assertEquals("foo", sealed[5]);
}{
  const frozen = arraylike(true);
  assertThrows(() => Array.prototype.splice.call(frozen, 5, 1, "foo"),
      TypeError);
  assertEquals("foo", frozen[5]);
  assertThrows(() => Array.prototype.splice.call(frozen, 5, 0, "bar"),
      TypeError);
  assertEquals("foo", frozen[5]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/021e9b0^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=021e9b0)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=021e9b0)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=021e9b0)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=021e9b0)  
[test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden?cl=021e9b0)  
...  
  

---   

## **regress-crbug-831984.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::FullEvacuationVerifier::VerifyPointers](https://crbug.com/831984)**  
**[Commit: [keys] Don't keep chain of OrderedHashSets in KeyAccumulator](https://chromium.googlesource.com/v8/v8/+/7bb79b9)**  
  
Date(Commit): Mon Apr 16 21:07:06 2018  
Components: Blink>JavaScript>GC  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-Medium, Hotlist-Merge-Approved, allpublic, Clusterfuzz, ClusterFuzz-Verified, NodeJS-Backport-Rejected, M-65, M-66, Test-Predator-Auto-CC, Test-Predator-Auto-Components, merge-merged-6.6, merge-merged-6.7, Release-1-M66, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/1012874](https://chromium-review.googlesource.com/1012874)  
Regress: [mjsunit/regress/regress-crbug-831984.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-831984.js)  
```javascript
let arr = [...Array(9000)];
for (let j = 0; j < 40; j++) {
  Reflect.ownKeys(arr).shift();
  Array(64386);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bb79b9^!)  
[src/keys.cc](https://cs.chromium.org/chromium/src/v8/src/keys.cc?cl=7bb79b9)  
[test/mjsunit/regress/regress-crbug-831984.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-831984.js?cl=7bb79b9)  
  

---   

## **regress-7652.js (v8 issue)**  
   
**[Issue: Several array methods fail to check for new array length exceeding 2^53-1](https://crbug.com/v8/7652)**  
**[Commit: Check new length in array splice and unshift.](https://chromium.googlesource.com/v8/v8/+/00a3bfa)**  
  
Date(Commit): Mon Apr 16 16:26:33 2018  
Code Review: [https://chromium-review.googlesource.com/1013563](https://chromium-review.googlesource.com/1013563)  
Regress: [mjsunit/regress/regress-7652.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7652.js)  
```javascript
const splice = Array.prototype.splice;
const unshift = Array.prototype.unshift;

{
  let a = {length: 2**53 - 1};
  assertThrows(() => unshift.call(a, 42), TypeError);
  assertEquals(unshift.call(a), 2**53 - 1);
}

{
  let a = {length: 2**53 - 1};
  assertThrows(() => splice.call(a, 0, 0, 42), TypeError);
  assertEquals(splice.call(a, 0, 1, 42).length, 1);
  assertEquals(a[0], 42);
}

{
  let a = {length: 2**53 - 1, [Symbol.isConcatSpreadable]: true};
  assertThrows(() => [42].concat(a), TypeError);
  assertThrows(() => [, ].concat(a), TypeError);
  assertThrows(() => [].concat(42, a), TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00a3bfa^!)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=00a3bfa)  
[test/mjsunit/regress/regress-7652.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7652.js?cl=00a3bfa)  
  

---   

## **regress-7642.js (v8 issue)**  
   
**[Issue: spread super call doesn't trigger InitializeInstanceFields](https://crbug.com/v8/7642)**  
**[Commit: [interpreter] Move desugaring of spread super call to bytecode generator](https://chromium.googlesource.com/v8/v8/+/42049b4)**  
  
Date(Commit): Fri Apr 13 18:25:31 2018  
Code Review: [https://chromium-review.googlesource.com/1009907](https://chromium-review.googlesource.com/1009907)  
Regress: [mjsunit/regress/regress-7642.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7642.js)  
```javascript
const a = [2];
const b = [4];

let log;

class C {
  constructor(...args) {
    log = args;
  }
}

class D extends C {
  field = 42;
  constructor() { super(1) };
}
assertEquals(42, (new D).field);
assertEquals([1], log);

class E extends C {
  field = 42;
  constructor() { super(...a) };
}
assertEquals(42, (new E).field);
assertEquals([2], log);

class F extends C {
  field = 42;
  constructor() { super(1, ...a) };
}
assertEquals(42, (new F).field);
assertEquals([1, 2], log);

class G extends C {
  field = 42;
  constructor() { super(1, ...a, 3) };
}
assertEquals(42, (new G).field);
assertEquals([1, 2, 3], log);

class H extends C {
  field = 42;
  constructor() { super(1, ...a, 3, ...b) };
}
assertEquals(42, (new H).field);
assertEquals([1, 2, 3, 4], log);

class I extends C {
  field = 42;
  constructor() { super(1, ...a, 3, ...b, 5) };
}
assertEquals(42, (new I).field);
assertEquals([1, 2, 3, 4, 5], log);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/42049b4^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=42049b4)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=42049b4)  
[test/cctest/interpreter/bytecode_expectations/SuperCallAndSpread.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/SuperCallAndSpread.golden?cl=42049b4)  
[test/mjsunit/regress/regress-7642.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7642.js?cl=42049b4)  
  

---   

## **regress-crbug-831943.js (chromium issue)**  
   
**[Issue: Security: Crash with JavaScript RegExp subclassing](https://crbug.com/831943)**  
**[Commit: [builtins] Fix missing ToString in RegExp.p.match](https://chromium.googlesource.com/v8/v8/+/7bdbe77)**  
  
Date(Commit): Thu Apr 12 14:54:54 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, reward-1500, Security_Impact-Stable, Security_Severity-Medium, allpublic, reward-inprocess, ClusterFuzz-Verified, Test-Predator-Auto-Components, merge-merged-6.7, CVE_description-submitted, Release-0-M67, CVE-2018-6136  
Code Review: [https://chromium-review.googlesource.com/1009325](https://chromium-review.googlesource.com/1009325)  
Regress: [mjsunit/regress/regress-crbug-831943.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-831943.js)  
```javascript
class MyRegExp extends RegExp {
  exec(str) {
    const r = super.exec.call(this, str);
    if (r) r[0] = 0;
    return r;
  }
}

const result = 'a'.match(new MyRegExp('.', 'g'));
assertArrayEquals(result, ['0']);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bdbe77^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=7bdbe77)  
[test/mjsunit/regress/regress-crbug-831943.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-831943.js?cl=7bdbe77)  
  

---   

## **regress-831463.js (chromium issue)**  
   
**[Issue: CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsWasmInstanceObject()) in w](https://crbug.com/831463)**  
**[Commit: [wasm][interpreter] Check signature before getting code](https://chromium.googlesource.com/v8/v8/+/be1a231)**  
  
Date(Commit): Wed Apr 11 09:52:19 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1005005](https://chromium-review.googlesource.com/1005005)  
Regress: [mjsunit/regress/wasm/regress-831463.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-831463.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

const builder = new WasmModuleBuilder();
const sig = builder.addType(kSig_i_i);
builder.addFunction('call', kSig_i_v)
    .addBody([
      kExprI32Const, 0, kExprI32Const, 0, kExprCallIndirect, sig, kTableZero
    ])
    .exportAs('call');
builder.addImportedTable('imp', 'table');
const table = new WebAssembly.Table({element: 'anyfunc', initial: 1});
const instance = builder.instantiate({imp: {table: table}});
assertThrows(
    () => instance.exports.call(), WebAssembly.RuntimeError,
    /function signature mismatch/);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/be1a231^!)  
[src/wasm/wasm-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-interpreter.cc?cl=be1a231)  
[test/mjsunit/regress/wasm/regress-831463.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-831463.js?cl=be1a231)  
  

---   

## **regress-crbug-823069.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/823069)**  
**[Commit: [builtins] Throw on pop()/shift() when JSArray's length is not writable.](https://chromium.googlesource.com/v8/v8/+/75e04cd)**  
  
Date(Commit): Tue Apr 10 08:51:07 2018  
Components: Blink>JavaScript>Runtime  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/1002560](https://chromium-review.googlesource.com/1002560)  
Regress: [mjsunit/regress/regress-crbug-823069.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-823069.js)  
```javascript
var v = [];
Object.defineProperty(v, "length", {value: 3, writable: false});
assertThrows(()=>{v.pop();});
assertThrows(()=>{v.shift();});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/75e04cd^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=75e04cd)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=75e04cd)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=75e04cd)  
[src/objects/js-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array.h?cl=75e04cd)  
[test/mjsunit/regress/regress-crbug-823069.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-823069.js?cl=75e04cd)  
  

---   

## **regress-827806.js (chromium issue)**  
   
**[Issue: Heap-use-after-free in v8::internal::Isolate::UnregisterFromReleaseAtTeardown](https://crbug.com/827806)**  
**[Commit: [wasm] Add regression test for chromium:827806](https://chromium.googlesource.com/v8/v8/+/ccde646)**  
  
Date(Commit): Thu Apr 05 18:49:23 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-67, M-66, merge-merged-6.6, Release-2-M66  
Code Review: [https://crrev.com/c/995796,](https://crrev.com/c/995796,)  
Regress: [mjsunit/regress/wasm/regress-827806.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-827806.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

try {
  (function () {
    let m = new WasmModuleBuilder();
    m.addFunction("sub", kSig_i_ii)
    m.instantiate();
  })();
} catch (e) {
  console.info("caught exception");
  console.info(e);
}
for (let i = 0; i < 150; i++) {
  var m = new WasmModuleBuilder();
  m.addMemory(2);
  m.instantiate();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ccde646^!)  
[test/mjsunit/regress/wasm/regress-827806.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-827806.js?cl=ccde646)  
  

---   

## **regress-crbug-827013.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadFixedArrayElement( descriptors, DescriptorArray::To](https://crbug.com/827013)**  
**[Commit: [builtins] Fix fast path of Function.prototype.bind.](https://chromium.googlesource.com/v8/v8/+/ef01379)**  
  
Date(Commit): Tue Apr 03 17:49:05 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-67, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/993053](https://chromium-review.googlesource.com/993053)  
Regress: [mjsunit/regress/regress-crbug-827013.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-827013.js)  
```javascript
(function Test() {
  var f = () => 42;
  function modify_f() {
    delete f.length;
    delete f.name;

    var g = Object.create(f);
    for (var i = 0; i < 5; i++) {
      g.dummy;
    }
  }
  %EnsureFeedbackVectorForFunction(f);
  assertTrue(%HasFastProperties(f));

  var h = f.bind(this);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef01379^!)  
[src/builtins/builtins-function-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function-gen.cc?cl=ef01379)  
[test/mjsunit/regress/regress-crbug-827013.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-827013.js?cl=ef01379)  
  

---   

## **regress-crbug-825045.js (chromium issue)**  
   
**[Issue: DCHECK failure in descriptor_number < number_of_descriptors() in objects-inl.h](https://crbug.com/825045)**  
**[Commit: [turbofan] Properly test number of descriptors.](https://chromium.googlesource.com/v8/v8/+/aa30205)**  
  
Date(Commit): Tue Apr 03 07:30:47 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Owner, merge-merged-6.6, Release-0-M66  
Code Review: [https://chromium-review.googlesource.com/991812](https://chromium-review.googlesource.com/991812)  
Regress: [mjsunit/regress/regress-crbug-825045.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-825045.js)  
```javascript
const obj = new class A extends async function
() {}
.constructor {}
();
delete obj.name;
Number.prototype.__proto__ = obj;
function foo() {
  return obj.bind();
};
%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa30205^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=aa30205)  
[test/mjsunit/regress/regress-crbug-825045.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-825045.js?cl=aa30205)  
  

---   

## **regress-808848.js (chromium issue)**  
   
**[Issue: Error when transferring a wasm instance between AudioWorkletNode and AudioWorkletProcessor](https://crbug.com/808848)**  
**[Commit: [wasm] Fix crash serializing modules w/ big frames](https://chromium.googlesource.com/v8/v8/+/fae1ab0)**  
  
Date(Commit): Tue Mar 27 18:34:06 2018  
Components: Blink>WebAudio, Blink>JavaScript>WebAssembly  
Labels: Needs-Feedback, Via-Wizard-Javascript, M-65  
Code Review: [https://chromium-review.googlesource.com/981687](https://chromium-review.googlesource.com/981687)  
Regress: [mjsunit/regress/wasm/regress-808848.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-808848.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const kNumLocals = 128;

function varuint32(val) {
  let bytes = [];
  for (let i = 0; i < 4; ++i) {
    bytes.push(0x80 | ((val >> (7 * i)) & 0x7f));
  }
  bytes.push((val >> (7 * 4)) & 0x7f);
  return bytes;
}

let body = [];

for (let i = 0; i < kNumLocals; ++i) {
  body.push(kExprCallFunction, 0, kExprLocalSet, ...varuint32(i));
}

for (let i = 0; i < kNumLocals; ++i) {
  body.push(kExprLocalGet, ...varuint32(i), kExprCallFunction, 1);
}

let builder = new WasmModuleBuilder();
builder.addImport('mod', 'get', kSig_i_v);
builder.addImport('mod', 'call', kSig_v_i);
builder.
  addFunction('main', kSig_v_v).
  addLocals({i32_count: kNumLocals}).
  addBody(body).
  exportAs('main');
let m1_bytes = builder.toBuffer();
let m1 = new WebAssembly.Module(m1_bytes);

let serialized_m1 = %SerializeWasmModule(m1);

let worker_onmessage = function(msg) {
  let {serialized_m1, m1_bytes} = msg;

  let m1_clone = %DeserializeWasmModule(serialized_m1, m1_bytes);
  let imports = {mod: {get: () => 3, call: () => {}}};
  let i2 = new WebAssembly.Instance(m1_clone, imports);
  i2.exports.main();
  postMessage('done');
}
let workerScript = "onmessage = " + worker_onmessage.toString();

let worker = new Worker(workerScript, {type: 'string'});
worker.postMessage({serialized_m1, m1_bytes});

print(worker.getMessage());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fae1ab0^!)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=fae1ab0)  
[test/mjsunit/regress/wasm/regress-808848.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-808848.js?cl=fae1ab0)  
  

---   

## **regress-825087a.js (chromium issue)**  
   
**[Issue: DCHECK failure in is_wasm_memory == GetIsolate()->wasm_engine()->memory_tracker()->IsWasmMemory( b](https://crbug.com/825087)**  
**[Commit: [wasm] clear is_wasm_memory flag when neutering ArrayBuffers](https://chromium.googlesource.com/v8/v8/+/ff43bbe)**  
  
Date(Commit): Sat Mar 24 00:30:23 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/978840](https://chromium-review.googlesource.com/978840)  
Regress: [mjsunit/regress/wasm/regress-825087a.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-825087a.js), [mjsunit/regress/wasm/regress-825087b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-825087b.js)  
```javascript
PAGES = 10;
memory = new WebAssembly.Memory({initial: PAGES});
buffer = memory.buffer;
memory.grow(0);
WebAssembly.validate(buffer);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ff43bbe^!)  
[src/objects/js-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array.h?cl=ff43bbe)  
[src/wasm/wasm-memory.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-memory.cc?cl=ff43bbe)  
[test/mjsunit/regress/wasm/regress-825087a.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-825087a.js?cl=ff43bbe)  
[test/mjsunit/regress/wasm/regress-825087b.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-825087b.js?cl=ff43bbe)  
  

---   

## **regress-store-transition-dict.js (other issue)**  
   
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
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=78c6bbd)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=78c6bbd)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=78c6bbd)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=78c6bbd)  
[src/ic/handler-configuration-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration-inl.h?cl=78c6bbd)  
...  
  
  
---   

## **regress-7582.js (v8 issue)**  
   
**[Issue: [Liftoff] Float comparisons might spill conditionally](https://crbug.com/v8/7582)**  
**[Commit: [Liftoff] Fix conditional spilling](https://chromium.googlesource.com/v8/v8/+/2589ea0)**  
  
Date(Commit): Thu Mar 22 18:45:17 2018  
Code Review: [https://chromium-review.googlesource.com/973961](https://chromium-review.googlesource.com/973961)  
Regress: [mjsunit/regress/wasm/regress-7582.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7582.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addGlobal(kWasmI32, 1);
sig0 = makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
builder.addFunction(undefined, sig0)
  .addBody([
kExprF32Const, 0x01, 0x00, 0x00, 0x00,
kExprF32Const, 0x00, 0x00, 0x00, 0x00,
kExprF32Eq,  // --> i32:0
kExprF32Const, 0xc9, 0xc9, 0x69, 0xc9,
kExprF32Const, 0xc9, 0xc9, 0xc9, 0x00,
kExprF32Eq,  // --> i32:0 i32:0
kExprIf, kWasmF32,
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
kExprElse,   // @32
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
  kExprEnd,   // --> i32:0 f32:0
kExprF32Const, 0xc9, 0x00, 0x00, 0x00,
kExprF32Const, 0xc9, 0xc9, 0xc9, 0x00,
kExprF32Const, 0xc9, 0xc9, 0xa0, 0x00, // --> i32:0 f32:0 f32 f32 f32
kExprF32Eq,  // --> i32:0 f32:0 f32 i32:0
kExprIf, kWasmF32,
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
kExprElse,
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
  kExprEnd,  // --> i32:0 f32:0 f32 f32:0
kExprF32Eq,  // --> i32:0 f32:0 i32:0
kExprIf, kWasmF32,
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
kExprElse,
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,
  kExprEnd,   // --> i32:0 f32:0 f32:0
kExprF32Const, 0xc9, 0xc9, 0xff, 0xff,  // --> i32:0 f32:0 f32:0 f32
kExprF32Eq,  // --> i32:0 f32:0 i32:0
kExprDrop,
kExprDrop, // --> i32:0
kExprI32Const, 1, // --> i32:0 i32:1
kExprI32GeU,  // --> i32:0
          ]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(0, instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2589ea0^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=2589ea0)  
[test/mjsunit/regress/wasm/regress-7582.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7582.js?cl=2589ea0)  
  

---   

## **regress-824681.js (chromium issue)**  
   
**[Issue: Wasm async compilation might never finish](https://crbug.com/824681)**  
**[Commit: [wasm] Fix deadlock on async compilation](https://chromium.googlesource.com/v8/v8/+/be1b2d6)**  
  
Date(Commit): Thu Mar 22 11:57:21 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Hotlist-Merge-Approved, M-65, M-67, M-66, merge-merged-6.7  
Code Review: [https://chromium-review.googlesource.com/975301](https://chromium-review.googlesource.com/975301)  
Regress: [mjsunit/regress/wasm/regress-824681.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-824681.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let chain = Promise.resolve();
const builder = new WasmModuleBuilder();
for (let i = 0; i < 50; ++i) {
  builder.addFunction('fun' + i, kSig_i_v)
      .addBody([...wasmI32Const(i)])
      .exportFunc();
}
const buffer = builder.toBuffer();
for (let i = 0; i < 100; ++i) {
  chain = chain.then(() => WebAssembly.instantiate(buffer));
}
chain.then(({module, instance}) => instance.exports.fun1155())
    .then(res => print('Result of executing fun1155: ' + res));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/be1b2d6^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=be1b2d6)  
[test/mjsunit/regress/wasm/regress-824681.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-824681.js?cl=be1b2d6)  
  

---   

## **regress-7579.js (v8 issue)**  
   
**[Issue: [Liftoff] Stack slot overwrite on ia32](https://crbug.com/v8/7579)**  
**[Commit: [Liftoff] Fix stack slot overwrite](https://chromium.googlesource.com/v8/v8/+/8bb41e8)**  
  
Date(Commit): Wed Mar 21 15:38:39 2018  
Code Review: [https://chromium-review.googlesource.com/973301](https://chromium-review.googlesource.com/973301)  
Regress: [mjsunit/regress/wasm/regress-7579.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7579.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
sig0 = makeSig([], [kWasmI32]);
builder.addFunction(undefined, sig0)
  .addBody([
    kExprI64Const, 0xc8, 0xda, 0x9c, 0xbc, 0xf8, 0xf0, 0xe1, 0xc3, 0x87, 0x7f,
    kExprLoop, kWasmF64,
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      ...wasmF64Const(0),
      kExprCallFunction, 0x01,
      ...wasmF64Const(0),
      kExprEnd,
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    ...wasmF64Const(0),
    kExprCallFunction, 0x01,
    kExprI64Const, 0xb9, 0xf2, 0xe4, 0x01,
    kExprI64LtS]);
sig1 = makeSig(new Array(12).fill(kWasmF64), []);
builder.addFunction(undefined, sig1).addBody([]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(1, instance.exports.main());

const builder2 = new WasmModuleBuilder();
sig0 = makeSig([], [kWasmI32]);
builder2.addFunction(undefined, sig0).addLocals({i64_count: 1}).addBody([
  kExprLoop, kWasmI32,     // loop i32
  kExprLocalGet, 0,        // get_local 3
  kExprF32SConvertI64,     // f32.sconvert/i64
  kExprI32ReinterpretF32,  // i32.reinterpret/f32
  kExprEnd                 // end
]);
builder2.addExport('main', 0);
const instance2 = builder2.instantiate();
assertEquals(0, instance2.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8bb41e8^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=8bb41e8)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=8bb41e8)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=8bb41e8)  
[test/mjsunit/regress/wasm/regress-7579.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7579.js?cl=8bb41e8)  
  

---   

## **regress-crbug-813630.js (chromium issue)**  
   
**[Issue: DCHECK failure in !has_rest_ in scopes.cc](https://crbug.com/813630)**  
**[Commit: [parser] Fix aborting preparsing of a function with a rest param.](https://chromium.googlesource.com/v8/v8/+/4f506db)**  
  
Date(Commit): Wed Mar 21 09:04:07 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/972901](https://chromium-review.googlesource.com/972901)  
Regress: [mjsunit/regress/regress-crbug-813630.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-813630.js)  
```javascript
function g(...args) {
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f506db^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=4f506db)  
[test/mjsunit/regress/regress-crbug-813630.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-813630.js?cl=4f506db)  
  

---   

## **regress-7254.js (v8 issue)**  
   
**[Issue: Deopt loop in a store that transitions elements kind](https://crbug.com/v8/7254)**  
**[Commit: [compiler] Don't infer receiver maps for stores.](https://chromium.googlesource.com/v8/v8/+/c94dcb2)**  
  
Date(Commit): Fri Mar 16 13:10:24 2018  
Code Review: [https://chromium-review.googlesource.com/966065](https://chromium-review.googlesource.com/966065)  
Regress: [mjsunit/regress/regress-7254.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7254.js)  
```javascript
function foo(a) {
  a[0];
  a[1] = "";
}

%PrepareFunctionForOptimization(foo);
foo([0,0].map(x => x));
foo([0,0].map(x => x));
%OptimizeFunctionOnNextCall(foo);
foo([0,0].map(x => x));
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c94dcb2^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=c94dcb2)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=c94dcb2)  
[test/mjsunit/regress/regress-7254.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7254.js?cl=c94dcb2)  
  

---   

## **regress-7565.js (v8 issue)**  
   
**[Issue: [Liftoff] Miscompilation on x64](https://crbug.com/v8/7565)**  
**[Commit: [Liftoff][x64] Fix and optimize spilling i64 constants](https://chromium.googlesource.com/v8/v8/+/27e3625)**  
  
Date(Commit): Fri Mar 16 11:05:11 2018  
Code Review: [https://chromium-review.googlesource.com/965062](https://chromium-review.googlesource.com/965062)  
Regress: [mjsunit/regress/wasm/regress-7565.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7565.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
sig0 = makeSig([], [kWasmI32]);
builder.addFunction(undefined, sig0).addLocals({i64_count: 1}).addBody([
  kExprLoop, kWasmI32,                    // loop i32
  kExprF32Const, 0x00, 0x00, 0x00, 0x00,  // f32.const 0      --> f32:0
  kExprLocalGet, 0x00,                    // get_local 0      --> i64:0
  kExprF32SConvertI64,                    // f32.sconvert/i64 --> f32:0
  kExprF32Ge,                             // f32.ge           --> i32:1
  kExprEnd,                               // end
]);
builder.addExport('main', 0);
const module = builder.instantiate();
assertEquals(1, module.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/27e3625^!)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=27e3625)  
[test/mjsunit/regress/wasm/regress-7565.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7565.js?cl=27e3625)  
  

---   

## **regress-821368.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSArrayBuffer::Neuter](https://crbug.com/821368)**  
**[Commit: Mark neteured ArrayBuffers as not neuterable](https://chromium.googlesource.com/v8/v8/+/dfe7eb8)**  
  
Date(Commit): Thu Mar 15 18:19:32 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/963601](https://chromium-review.googlesource.com/963601)  
Regress: [mjsunit/regress/regress-821368.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-821368.js)  
```javascript
const worker = new Worker("onmessage = function(){}", {type: 'string'});
const buffer = new ArrayBuffer();
worker.postMessage(buffer, [buffer]);
try {
  worker.postMessage(buffer, [buffer]);
} catch (e) {
  if (e != "ArrayBuffer could not be transferred") {
    throw e;
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dfe7eb8^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=dfe7eb8)  
[test/mjsunit/regress/regress-821368.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-821368.js?cl=dfe7eb8)  
  

---   

## **regress-crbug-822284.js (chromium issue)**  
   
**[Issue: ThinStrings are incompatible with TurboFan SeqString types](https://crbug.com/822284)**  
**[Commit: [turbofan] NumberToString can return non-sequential strings.](https://chromium.googlesource.com/v8/v8/+/c65f0a7)**  
  
Date(Commit): Thu Mar 15 17:52:12 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security, Security_Severity-Medium, Arch-All, allpublic, merge-merged-6.6  
Code Review: [https://chromium-review.googlesource.com/964523](https://chromium-review.googlesource.com/964523)  
Regress: [mjsunit/regress/regress-crbug-822284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-822284.js)  
```javascript
function foo(a) {
  a = "" + Math.abs(a);
  return a.charCodeAt(0);
}

%PrepareFunctionForOptimization(foo);
String.fromCharCode(49);
const o = {};
o[1..toString()] = 1;

assertEquals(49, foo(1));
assertEquals(49, foo(1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(49, foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c65f0a7^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=c65f0a7)  
[test/mjsunit/regress/regress-crbug-822284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-822284.js?cl=c65f0a7)  
  

---   

## **regress-821137.js (chromium issue)**  
   
**[Issue: OOB read/write using Array.prototype.from](https://crbug.com/821137)**  
**[Commit: [builtins] Fix OOB read/write using Array.from](https://chromium.googlesource.com/v8/v8/+/b5da57a)**  
  
Date(Commit): Wed Mar 14 11:31:42 2018  
Components: Blink>JavaScript>Compiler  
Labels: Security_Severity-High, Security_Impact-Beta, allpublic, M-66, merge-merged-6.6, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/962222](https://chromium-review.googlesource.com/962222)  
Regress: [mjsunit/regress/regress-821137.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-821137.js)  
```javascript
let oobArray = [];
let maxSize = 1028 * 8;
Array.from.call(function() { return oobArray }, {[Symbol.iterator] : _ => (
  {
    counter : 0,
    next() {
      let result = this.counter++;
      if (this.counter > maxSize) {
        oobArray.length = 0;
        return {done: true};
      } else {
        return {value: result, done: false};
      }
    }
  }
) });
assertEquals(oobArray.length, maxSize);

oobArray[oobArray.length - 1] = 0x41414141;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b5da57a^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=b5da57a)  
[test/mjsunit/regress/regress-821137.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-821137.js?cl=b5da57a)  
  

---   

## **regress-820802.js (chromium issue)**  
   
**[Issue: Stack-overflow in v8::internal::Invoke](https://crbug.com/820802)**  
**[Commit: [Liftoff] Fix stack pointer corruption](https://chromium.googlesource.com/v8/v8/+/cc862e6)**  
  
Date(Commit): Wed Mar 14 08:13:12 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Stability-AFL, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/959064](https://chromium-review.googlesource.com/959064)  
Regress: [mjsunit/regress/wasm/regress-820802.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-820802.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addGlobal(kWasmI32, 0);
const sig0 = makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
builder.addFunction(undefined, sig0).addBody([
  kExprI32Const, 1,  // i32.const 1
  kExprI32Const, 0,  // i32.const 0
  kExprI32Const, 3,  // i32.const 3
  kExprI32GeU,       // i32.ge_u
  kExprI32Rol,       // i32.rol
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
assertEquals(1, instance.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc862e6^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=cc862e6)  
[src/wasm/baseline/arm64/liftoff-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm64/liftoff-assembler-arm64.h?cl=cc862e6)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=cc862e6)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=cc862e6)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=cc862e6)  
...  
  

---   

## **regress-crbug-821159-1.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:ia32,ignition](https://crbug.com/821159)**  
**[Commit: [es2015] Properly deal with fast-path results from IterableToList.](https://chromium.googlesource.com/v8/v8/+/631629a)**  
  
Date(Commit): Tue Mar 13 07:23:57 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/959323](https://chromium-review.googlesource.com/959323)  
Regress: [mjsunit/regress/regress-crbug-821159-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-821159-1.js), [mjsunit/regress/regress-crbug-821159-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-821159-2.js), [mjsunit/regress/regress-crbug-821159-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-821159-3.js), [mjsunit/regress/regress-crbug-821159-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-821159-4.js)  
```javascript
Array.prototype[0] = 0;
Math.max(...[3]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/631629a^!)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=631629a)  
[test/mjsunit/regress/regress-crbug-821159-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-821159-1.js?cl=631629a)  
[test/mjsunit/regress/regress-crbug-821159-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-821159-2.js?cl=631629a)  
[test/mjsunit/regress/regress-crbug-821159-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-821159-3.js?cl=631629a)  
[test/mjsunit/regress/regress-crbug-821159-4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-821159-4.js?cl=631629a)  
  

---   

## **regress-crbug-820820.js (chromium issue)**  
   
**[Issue: Null-dereference READ in type](https://crbug.com/820820)**  
**[Commit: [turbofan] Properly deal with killed nodes in LoadElimination.](https://chromium.googlesource.com/v8/v8/+/022e1a5)**  
  
Date(Commit): Tue Mar 13 06:27:13 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/958843](https://chromium-review.googlesource.com/958843)  
Regress: [mjsunit/regress/regress-crbug-820820.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-820820.js)  
```javascript
function* generator() {
  yield undefined;
}
function bar(x) {
  const objects = [];
  for (let obj of generator()) {
  }
  return objects[0];
}
function foo() {
  try { undefined[0] = bar(); } catch (e) { }
  Math.min(bar(), bar(), bar());
}
%PrepareFunctionForOptimization(foo);
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/022e1a5^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=022e1a5)  
[src/compiler/load-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.h?cl=022e1a5)  
[test/mjsunit/regress/regress-crbug-820820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-820820.js?cl=022e1a5)  
  

---   

## **regress-813440.js (chromium issue)**  
   
**[Issue: Direct-leak in uprv_malloc_60](https://crbug.com/813440)**  
**[Commit: [intl] Store the collator as a Managed](https://chromium.googlesource.com/v8/v8/+/825d017)**  
  
Date(Commit): Mon Mar 12 16:46:42 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Memory-LeakSanitizer, Clusterfuzz  
Code Review: [https://chromium-review.googlesource.com/956069](https://chromium-review.googlesource.com/956069)  
Regress: [mjsunit/regress/regress-813440.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-813440.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

const builder = new WasmModuleBuilder();
builder.addFunction('f', kSig_i_v).addBody([kExprI32Const, 42]);
const buffer = builder.toBuffer();
const module = WebAssembly.compile(buffer);

'퓛'.localeCompare();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/825d017^!)  
[src/objects/intl-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/intl-objects.cc?cl=825d017)  
[src/objects/intl-objects.h](https://cs.chromium.org/chromium/src/v8/src/objects/intl-objects.h?cl=825d017)  
[src/runtime/runtime-intl.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-intl.cc?cl=825d017)  
[test/mjsunit/regress/regress-813440.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-813440.js?cl=825d017)  
  

---   

## **regress-crbug-820596.js (chromium issue)**  
   
**[Issue: DCHECK failure in static_cast<unsigned>(length_) > static_cast<unsigned>(i) in zone.h](https://crbug.com/820596)**  
**[Commit: [esnext] fix OOB read in ASTPrinter::VisistTemplateLiteral](https://chromium.googlesource.com/v8/v8/+/0802e2b)**  
  
Date(Commit): Sat Mar 10 01:13:50 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/957883](https://chromium-review.googlesource.com/957883)  
Regress: [mjsunit/es6/regress/regress-crbug-820596.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-820596.js)  
```javascript
var x;
`Crashes if OOB read with --print-ast ${x}`;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0802e2b^!)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=0802e2b)  
[test/mjsunit/es6/regress/regress-crbug-820596.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-crbug-820596.js?cl=0802e2b)  
  

---   

## **regress-crbug-820312.js (chromium issue)**  
   
**[Issue: Security: V8: PromiseAllResolveElementClosure can cause elements kind confusion](https://crbug.com/820312)**  
**[Commit: [builtins] Properly handle DICTIONARY_ELEMENTS in Promise.all closures.](https://chromium.googlesource.com/v8/v8/+/fd29e1d)**  
  
Date(Commit): Fri Mar 09 14:25:34 2018  
Components: Blink>JavaScript  
Labels: Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified  
Code Review: [https://chromium-review.googlesource.com/957090](https://chromium-review.googlesource.com/957090)  
Regress: [mjsunit/regress/regress-crbug-820312.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-820312.js)  
```javascript
let arr = new Array(0x10000);
let resolve_element_closures = new Array(0x10000);

for (let i = 0; i < arr.length; i++) {
    arr[i] = new Promise(() => {});
    arr[i].then = ((idx, resolve) => {
        resolve_element_closures[idx] = resolve;
    }).bind(null, i);
}

Promise.all(arr);

resolve_element_closures[0xffff]();

resolve_element_closures[100]();

resolve_element_closures[0xfffe]();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd29e1d^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=fd29e1d)  
[test/mjsunit/regress/regress-crbug-820312.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-820312.js?cl=fd29e1d)  
  

---   

## **regress-819869.js (chromium issue)**  
   
**[Issue: Security: Integer Overflow when Processing WebAssembly Locals](https://crbug.com/819869)**  
**[Commit: [wasm] Avoid integer overflow on function locals check](https://chromium.googlesource.com/v8/v8/+/a71e5f9)**  
  
Date(Commit): Thu Mar 08 17:00:55 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Hotlist-Merge-Approved, Security_Severity-High, allpublic, Arch-ARM, Merge-Rejected-65, merge-merged-6.6, Release-0-M66, CVE-2018-6092, CVE_description-submitted  
Code Review: [https://chromium-review.googlesource.com/955025](https://chromium-review.googlesource.com/955025)  
Regress: [mjsunit/regress/wasm/regress-819869.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-819869.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_i)
    .addLocals({i32_count: 0xffffffff})
    .addBody([]);
assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a71e5f9^!)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=a71e5f9)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=a71e5f9)  
[test/mjsunit/regress/wasm/regress-819869.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-819869.js?cl=a71e5f9)  
  

---   

## **regress-crbug-819298.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/819298)**  
**[Commit: [turbofan] Fix invalid SpeculativeToNumber optimization.](https://chromium.googlesource.com/v8/v8/+/e583fc8)**  
  
Date(Commit): Thu Mar 08 12:38:29 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/955423](https://chromium-review.googlesource.com/955423)  
Regress: [mjsunit/regress/regress-crbug-819298.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-819298.js)  
```javascript
var a = new Int32Array(2);

function foo(base) {
  a[base - 91] = 1;
};
%PrepareFunctionForOptimization(foo);
foo("");
foo("");
%OptimizeFunctionOnNextCall(foo);
foo(NaN);
assertEquals(0, a[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e583fc8^!)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=e583fc8)  
[test/mjsunit/regress/regress-crbug-819298.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-819298.js?cl=e583fc8)  
  

---   

## **regress-crbug-819086.js (chromium issue)**  
   
**[Issue: CHECK failure: Node::New() Error: #392:DeoptimizeIf[1] is nullptr in node.cc](https://crbug.com/819086)**  
**[Commit: [turbofan] Only store after all checks are done.](https://chromium.googlesource.com/v8/v8/+/6196dd0)**  
  
Date(Commit): Tue Mar 06 09:09:11 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/950723](https://chromium-review.googlesource.com/950723)  
Regress: [mjsunit/regress/regress-crbug-819086.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-819086.js)  
```javascript
function foo() {
  return [...[, -Infinity]];
};
%PrepareFunctionForOptimization(foo);
foo()[0];
foo()[0];
%OptimizeFunctionOnNextCall(foo);
foo()[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6196dd0^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=6196dd0)  
[test/mjsunit/regress/regress-crbug-819086.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-819086.js?cl=6196dd0)  
  

---   

## **regress-818070.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/818070)**  
**[Commit: [turbofan] Don't drop arguments in fast-path](https://chromium.googlesource.com/v8/v8/+/0d5588d)**  
  
Date(Commit): Mon Mar 05 15:19:11 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/948523](https://chromium-review.googlesource.com/948523)  
Regress: [mjsunit/regress/regress-818070.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-818070.js)  
```javascript
function f(a) {
  Math.imul(a);
}

x = { [Symbol.toPrimitive]: () => FAIL };
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
assertThrows(() => f(x), ReferenceError);

function f(a) {
  Math.pow(a);
}

x = { [Symbol.toPrimitive]: () => FAIL };
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
assertThrows(() => f(x), ReferenceError);

function f(a) {
  Math.atan2(a);
}

x = { [Symbol.toPrimitive]: () => FAIL };
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
assertThrows(() => f(x), ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0d5588d^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=0d5588d)  
[test/mjsunit/regress/regress-818070.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-818070.js?cl=0d5588d)  
  

---   

## **regress-7510.js (v8 issue)**  
   
**[Issue: Array iteration defeated by polymorphism in TurboFan](https://crbug.com/v8/7510)**  
**[Commit: [es2015] Refactor the JSArrayIterator.](https://chromium.googlesource.com/v8/v8/+/06ee127)**  
  
Date(Commit): Mon Mar 05 11:57:28 2018  
Code Review: [https://chromium-review.googlesource.com/946098](https://chromium-review.googlesource.com/946098)  
Regress: [mjsunit/regress/regress-7510.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7510.js)  
```javascript
function foo(a) {
  for (const x of a) {
    a[100] = 1;
  }
}

%PrepareFunctionForOptimization(foo);
foo([1]);
foo([1]);
%OptimizeFunctionOnNextCall(foo);
foo([1]);
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo([1]);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/06ee127^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=06ee127)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=06ee127)  
[src/builtins/builtins-typedarray-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typedarray-gen.cc?cl=06ee127)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=06ee127)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=06ee127)  
...  
  

---   

## **regress-818438.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::FeedbackNexus::GetKeyedAccessStoreMode](https://crbug.com/818438)**  
**[Commit: [ic] Relax a CHECK.](https://chromium.googlesource.com/v8/v8/+/c895a23)**  
  
Date(Commit): Mon Mar 05 10:09:01 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/947950](https://chromium-review.googlesource.com/947950)  
Regress: [mjsunit/regress/regress-818438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-818438.js)  
```javascript
function foo(b) {
  [,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
   ...b];
}
Array.prototype.__defineSetter__("0", () => {});
foo([3.3, 3.3, 3.3]);
foo([{}, 3.3]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c895a23^!)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=c895a23)  
[test/mjsunit/regress/regress-818438.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-818438.js?cl=c895a23)  
  

---   

## **regress-817225.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::Simulator::LoadStoreHelper](https://crbug.com/817225)**  
**[Commit: [turbofan] remove type-widening NaN-addition folding](https://chromium.googlesource.com/v8/v8/+/b8abd27)**  
  
Date(Commit): Fri Mar 02 14:19:59 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Hotlist-Merge-Approved, Clusterfuzz, ClusterFuzz-Verified, M-66, merge-merged-6.6  
Code Review: [https://chromium-review.googlesource.com/946129](https://chromium-review.googlesource.com/946129)  
Regress: [mjsunit/compiler/regress-817225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-817225.js)  
```javascript
function inlined(abort, n, a, b) {
  if (abort) return;
  var x = a ? true : "" + a;
  if (!a) {
    var y = n + y + 10;
    if(!b) {
      x = y;
    }
    if (x) {
      x = false;
    }
  }
  return x + 1;
}
inlined();
function optimized(abort, a, b) {
  return inlined(abort, "abc", a, b);
}
%PrepareFunctionForOptimization(optimized);
optimized(true);
%OptimizeFunctionOnNextCall(optimized);
optimized();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8abd27^!)  
[src/compiler/machine-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/machine-operator-reducer.cc?cl=b8abd27)  
[test/mjsunit/compiler/regress-817225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-817225.js?cl=b8abd27)  
  

---   

## **regress-817380.js (chromium issue)**  
   
**[Issue: DCHECK failure in code->kind() == wasm::WasmCode::kFunction || code->kind() == wasm::WasmCode::kWa](https://crbug.com/817380)**  
**[Commit: [wasm] Fix DCHECK for lazy compilation](https://chromium.googlesource.com/v8/v8/+/6195ebe)**  
  
Date(Commit): Fri Mar 02 09:48:11 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-67, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/943107](https://chromium-review.googlesource.com/943107)  
Regress: [mjsunit/regress/wasm/regress-817380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-817380.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder1 = new WasmModuleBuilder();
builder1.addFunction('mul', kSig_i_ii)
    .addBody([kExprLocalGet, 0, kExprLocalGet, 1, kExprI32Mul])
    .exportFunc();
const mul = builder1.instantiate().exports.mul;
const table = new WebAssembly.Table({
  element: 'anyfunc',
  initial: 10,
});
const builder2 = new WasmModuleBuilder();
const mul_import = builder2.addImport('q', 'wasm_mul', kSig_i_ii);
builder2.addImportedTable('q', 'table');
const glob_import = builder2.addImportedGlobal('q', 'glob', kWasmI32);
builder2.addElementSegment(0, glob_import, true, [mul_import]);
builder2.instantiate(
    {q: {glob: 0, js_div: i => i, wasm_mul: mul, table: table}});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6195ebe^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=6195ebe)  
[test/mjsunit/regress/wasm/regress-817380.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-817380.js?cl=6195ebe)  
  

---   

## **regress-7508.js (v8 issue)**  
   
**[Issue: [Liftoff] Debug check failed: is_gp() || is_fp() on ia32](https://crbug.com/v8/7508)**  
**[Commit: [Liftoff] Fix get_use_count for register pairs](https://chromium.googlesource.com/v8/v8/+/08a9e3e)**  
  
Date(Commit): Thu Mar 01 13:06:17 2018  
Code Review: [https://chromium-review.googlesource.com/942821](https://chromium-review.googlesource.com/942821)  
Regress: [mjsunit/regress/wasm/regress-7508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7508.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_v).addLocals({i64_count: 1}).addBody([
  kExprI64Const, 0xeb,     0xd7, 0xaf, 0xdf,
  0xbe,          0xfd,     0xfa, 0xf5, 0x6b,  // i64.const
  kExprI32Const, 0,                           // i32.const
  kExprIf,       kWasmI32,                    // if i32
  kExprI32Const, 0,                           // i32.const
  kExprElse,                                  // else
  kExprI32Const, 0,                           // i32.const
  kExprEnd,                                   // end
  kExprBrIf,     0,                           // br_if depth=0
  kExprLocalSet, 0,                           // set_local 0
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/08a9e3e^!)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=08a9e3e)  
[test/mjsunit/regress/wasm/regress-7508.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7508.js?cl=08a9e3e)  
  

---   

## **regress-crbug-816961.js (chromium issue)**  
   
**[Issue: Security: Use-after-free in TypedArrayOf and TypedArrayFrom](https://crbug.com/816961)**  
**[Commit: Fix buffer-detached check in TypedArray.of/from](https://chromium.googlesource.com/v8/v8/+/c94df3c)**  
  
Date(Commit): Wed Feb 28 20:52:55 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Approved, Security_Severity-High, Security_Impact-Beta, reward-7500, allpublic, reward-inprocess, M-66, merge-merged-6.6, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/939767](https://chromium-review.googlesource.com/939767)  
Regress: [mjsunit/regress/regress-crbug-816961.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-816961.js)  
```javascript
assertThrows(function() {
  var memory = new WebAssembly.Memory({initial: 64 * 1024 * 1024 / 0x10000});
  var array = new Uint8Array(memory.buffer);
  Uint8Array.of.call(function() { return array },
                    {valueOf() { memory.grow(1); } });
}, TypeError);

assertThrows(function() {
  var memory = new WebAssembly.Memory({initial: 64 * 1024 * 1024 / 0x10000});
  var array = new Uint8Array(memory.buffer);
  Uint8Array.from.call(function() { return array },
                       [{valueOf() { memory.grow(1); } }],
                       x => x);
}, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c94df3c^!)  
[src/builtins/builtins-typedarray-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typedarray-gen.cc?cl=c94df3c)  
[src/builtins/builtins-typedarray-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typedarray-gen.h?cl=c94df3c)  
[test/mjsunit/regress/regress-crbug-816961.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-816961.js?cl=c94df3c)  
  

---   

## **regress-816226.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::JSArrayBuffer::FreeBackingStore](https://crbug.com/816226)**  
**[Commit: [typed arrays] GetBuffer returns old buffer for guarded buffers](https://chromium.googlesource.com/v8/v8/+/c137eb5)**  
  
Date(Commit): Tue Feb 27 20:36:54 2018  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/938968](https://chromium-review.googlesource.com/938968)  
Regress: [mjsunit/regress/wasm/regress-816226.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-816226.js)  
```javascript
(new Int8Array((new WebAssembly.Memory({initial: 0})).buffer)).buffer;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c137eb5^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c137eb5)  
[test/mjsunit/regress/wasm/regress-816226.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-816226.js?cl=c137eb5)  
  

---   

## **regress-7499.js (v8 issue)**  
   
**[Issue: [Liftoff] Debug check failed: offset_imm <= max_offset on ia32](https://crbug.com/v8/7499)**  
**[Commit: [Liftoff][ia32] Handle overflow in memory offset](https://chromium.googlesource.com/v8/v8/+/a0e66bc)**  
  
Date(Commit): Tue Feb 27 15:06:24 2018  
Code Review: [https://chromium-review.googlesource.com/939389](https://chromium-review.googlesource.com/939389)  
Regress: [mjsunit/regress/wasm/regress-7499.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7499.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addFunction(undefined, kSig_v_v).addBody([
  kExprI32Const, 0,  // i32.const 0
  kExprI64LoadMem, 0, 0xff, 0xff, 0xff, 0xff,
  0x0f,       // i64.load align=0 offset=0xffffffff
  kExprDrop,  // drop
]);
builder.addExport('main', 0);
const module = builder.instantiate();
assertThrows(
    () => module.exports.main(), WebAssembly.RuntimeError, /out of bounds/);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0e66bc^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=a0e66bc)  
[test/mjsunit/regress/wasm/regress-7499.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7499.js?cl=a0e66bc)  
  

---   

## **regress-crbug-813450.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::Runtime_AllocateInNewSpace](https://crbug.com/813450)**  
**[Commit: [proxies] Use write barriers for Proxy [[Construct]] arguments](https://chromium.googlesource.com/v8/v8/+/c7d01c4)**  
  
Date(Commit): Tue Feb 27 14:41:08 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/939174](https://chromium-review.googlesource.com/939174)  
Regress: [mjsunit/regress/regress-crbug-813450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-813450.js)  
```javascript
var constructorArgs = new Array(0x10100);
var constructor = function() {};
var target = new Proxy(constructor, {
  construct: function() {
  }
});
var proxy = new Proxy(target, {
  construct: function(newTarget, args) {
    return Reflect.construct(constructor, []);
  }
});
var instance = new proxy();
var instance2 = Reflect.construct(proxy, constructorArgs);
%HeapObjectVerify(target);
%HeapObjectVerify(proxy);
%HeapObjectVerify(instance);
%HeapObjectVerify(instance2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7d01c4^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=c7d01c4)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=c7d01c4)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=c7d01c4)  
[test/mjsunit/regress/regress-crbug-813450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-813450.js?cl=c7d01c4)  
  

---   

## **regress-815392.js (chromium issue)**  
   
**[Issue: Null-dereference READ in Get](https://crbug.com/815392)**  
**[Commit: [turbofan] Bailout from optimizations for large bytecode sizes (>128kB).](https://chromium.googlesource.com/v8/v8/+/8c12348)**  
  
Date(Commit): Tue Feb 27 13:22:53 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/938421](https://chromium-review.googlesource.com/938421)  
Regress: [mjsunit/compiler/regress-815392.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-815392.js)  
```javascript
const __f_1 = eval(`(function __f_1() {
    class Derived extends Object {
      constructor() {
        ${"this.a=1;".repeat(0x3fffe-8)}
      }
    }
    return Derived;
})`);
assertThrows(() => new (__f_1())());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c12348^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=8c12348)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=8c12348)  
[test/mjsunit/compiler/regress-815392.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-815392.js?cl=8c12348)  
  

---   

## **regress-816289.js (chromium issue)**  
   
**[Issue: Fatal error in Runtime_TypedArrayCopyElements](https://crbug.com/816289)**  
**[Commit: [typedarray] Extend ElementsAccessor::CopyElements to all Object types](https://chromium.googlesource.com/v8/v8/+/6b25ab2)**  
  
Date(Commit): Mon Feb 26 15:51:31 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/937461](https://chromium-review.googlesource.com/937461)  
Regress: [mjsunit/regress/regress-816289.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-816289.js)  
```javascript
delete String.prototype[Symbol.iterator];
Int8Array.from("anything");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b25ab2^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=6b25ab2)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=6b25ab2)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=6b25ab2)  
[test/mjsunit/regress/regress-816289.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-816289.js?cl=6b25ab2)  
  

---   

## **regress-816317.js (chromium issue)**  
   
**[Issue: DCHECK failure in source->length_value() <= destination->length_value() - offset in elements.cc](https://crbug.com/816317)**  
**[Commit: [typedarray] Fix failing DCHECK for TA.from with a length getter.](https://chromium.googlesource.com/v8/v8/+/ec5c342)**  
  
Date(Commit): Mon Feb 26 13:42:23 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Severity-Low, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/937209](https://chromium-review.googlesource.com/937209)  
Regress: [mjsunit/regress/regress-816317.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-816317.js)  
```javascript
let a = new Float64Array(15);
Object.defineProperty(a, "length", {
  get: function () {
    return 6;
  }
});
delete a.__proto__.__proto__[Symbol.iterator];
Float64Array.from(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec5c342^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=ec5c342)  
[test/mjsunit/regress/regress-816317.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-816317.js?cl=ec5c342)  
  

---   

## **regress-814643.js (chromium issue)**  
   
**[Issue: Ill in __RT_impl_Runtime_IterableToListCanBeElided](https://crbug.com/814643)**  
**[Commit: [typedarray] Fix IterableToList when Number has an iterator](https://chromium.googlesource.com/v8/v8/+/aaa78c3)**  
  
Date(Commit): Thu Feb 22 10:23:32 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/930962](https://chromium-review.googlesource.com/930962)  
Regress: [mjsunit/regress/regress-814643.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-814643.js)  
```javascript
Number.prototype.__proto__ = String.prototype;
Uint8Array.from(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aaa78c3^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=aaa78c3)  
[test/mjsunit/es6/typedarray-from.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/typedarray-from.js?cl=aaa78c3)  
[test/mjsunit/regress/regress-814643.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-814643.js?cl=aaa78c3)  
  

---   

## **regress-808472.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::compiler::CFGBuilder::BuildBlockForNode](https://crbug.com/808472)**  
**[Commit: [turbofan] simplified lowering: process DeadValue input](https://chromium.googlesource.com/v8/v8/+/07abe39)**  
  
Date(Commit): Tue Feb 20 15:13:28 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Wrong, M-66  
Code Review: [https://chromium-review.googlesource.com/919362](https://chromium-review.googlesource.com/919362)  
Regress: [mjsunit/compiler/regress-808472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-808472.js)  
```javascript
function opt() {
    let opt, arr = [...[...[...[...new Uint8Array(0x10000)]]]];
    while (arr--)  {
        opt = ((typeof opt) === 'undefined') ? /a/ : arr;
    }
}
opt();
opt();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/07abe39^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=07abe39)  
[test/mjsunit/compiler/regress-808472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-808472.js?cl=07abe39)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=07abe39)  
  

---   

## **regress-crbug-813427.js (chromium issue)**  
   
**[Issue: CHECK failure: constructor_initial_map->instance_size() <= instance_size in objects.cc](https://crbug.com/813427)**  
**[Commit: [runtime] Fix overzealous check for derived constructor instance size](https://chromium.googlesource.com/v8/v8/+/da83b61)**  
  
Date(Commit): Tue Feb 20 13:28:37 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Owner, merge-merged-6.5, merge-merged-65  
Code Review: [https://chromium-review.googlesource.com/926003](https://chromium-review.googlesource.com/926003)  
Regress: [mjsunit/regress/regress-crbug-813427.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-813427.js)  
```javascript
function createPropertiesAssignment(count) {
  let result = "";
  for (let i = 0; i < count; i++) {
    result += "this.p"+i+" = undefined;";
  }
  return result;
}

function testSubclassProtoProperties(count) {
  const MyClass = eval(`(class MyClass {
    constructor() {
      ${createPropertiesAssignment(count)}
    }
  });`);

  class BaseClass {};
  class SubClass extends BaseClass {
    constructor() {
        super()
    }
  };

  const boundMyClass = MyClass.bind();
  %HeapObjectVerify(boundMyClass);

  SubClass.__proto__ = boundMyClass;
  var instance = new SubClass();

  %HeapObjectVerify(instance);
  // Create some more instances to complete in-object slack tracking.
  let results = [];
  for (let i = 0; i < 4000; i++) {
    results.push(new SubClass());
  }
  var instance = new SubClass();
  %HeapObjectVerify(instance);
}


for (let count = 0; count < 10; count++) {
  testSubclassProtoProperties(count);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da83b61^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=da83b61)  
[test/mjsunit/regress/regress-crbug-813427.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-813427.js?cl=da83b61)  
  

---   

## **regress-812005.js (chromium issue)**  
   
**[Issue: DCHECK failure in dst.type() == src.type() in liftoff-assembler.cc](https://crbug.com/812005)**  
**[Commit: [Liftoff] Fix result type of f64 binops](https://chromium.googlesource.com/v8/v8/+/6ac2579)**  
  
Date(Commit): Mon Feb 19 16:12:30 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Security_Impact-None, Clusterfuzz, ClusterFuzz-Verified, M-66, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/924025](https://chromium-review.googlesource.com/924025)  
Regress: [mjsunit/regress/wasm/regress-812005.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-812005.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_d_v).addBody([
  ...wasmF64Const(0),  // f64.const 0
  ...wasmF64Const(0),  // f64.const 0
  ...wasmI32Const(0),  // i32.const 0
  kExprBrIf, 0x00,     // br_if depth=0
  kExprF64Add          // f64.add
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6ac2579^!)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=6ac2579)  
[test/mjsunit/regress/wasm/regress-812005.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-812005.js?cl=6ac2579)  
  

---   

## **regress-812451.js (chromium issue)**  
   
**[Issue: Crash in /build/eglibc-ripdx6/eglibc-NUMBER/string/../sysdeps/x86_64/multiarch/memcpy-sse](https://crbug.com/812451)**  
**[Commit: Reland "[ic] EmitElementStore: don't miss when hitting new space limit."](https://chromium.googlesource.com/v8/v8/+/a50bc8a)**  
  
Date(Commit): Thu Feb 15 12:27:18 2018  
Components: Blink>JavaScript>GC, Blink>JavaScript  
Labels: Reproducible, Security_Severity-Medium, Security_Impact-Head, allpublic, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/918723](https://chromium-review.googlesource.com/918723)  
Regress: [mjsunit/regress/regress-812451.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-812451.js)  
```javascript
var x = [];
function foo(x, p) {
  x[p] = 5.3;
}
foo(x, 1);
foo(x, 2);
foo(x, -1);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a50bc8a^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=a50bc8a)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=a50bc8a)  
[test/mjsunit/regress/regress-812451.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-812451.js?cl=a50bc8a)  
  

---   

## **regress-crbug-808192.js (chromium issue)**  
   
**[Issue: Security: V8 Integer overflow in object allocation size](https://crbug.com/808192)**  
**[Commit: [runtime] Harden JSFunction::CalculateInstanceSizeHelper(...)](https://chromium.googlesource.com/v8/v8/+/7b27040)**  
  
Date(Commit): Mon Feb 12 20:54:29 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, M-65, Merge-Rejected-64, merge-merged-6.5, Release-0-M65, CVE-2018-6065, CVE_description-submitted  
Code Review: [https://chromium-review.googlesource.com/902103](https://chromium-review.googlesource.com/902103)  
Regress: [mjsunit/regress/regress-crbug-808192.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-808192.js)  
```javascript
const f = eval(`(function f(i) {
  if (i == 0) {
    class Derived extends Object {
      constructor() {
        super();
        ${"this.a=1;".repeat(0x3fffe-8)}
      }
    }
    return Derived;
  }

  class DerivedN extends f(i-1) {
    constructor() {
      super();
      ${"this.a=1;".repeat(0x40000-8)}
    }
  }

  return DerivedN;
})`);

let a = new (f(0x7ff))();
a.a = 1;
gc();
assertEquals(1, a.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b27040^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7b27040)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=7b27040)  
[test/cctest/test-inobject-slack-tracking.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-inobject-slack-tracking.cc?cl=7b27040)  
[test/mjsunit/es6/destructuring-assignment.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/destructuring-assignment.js?cl=7b27040)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=7b27040)  
...  
  

---   

## **regress-810973.js (chromium issue)**  
   
**[Issue: CHECK failure: !result.failed() in wasm-engine.cc](https://crbug.com/810973)**  
**[Commit: [asm.js] Enforce maximum number of parameters for asm.js.](https://chromium.googlesource.com/v8/v8/+/73d6072)**  
  
Date(Commit): Mon Feb 12 19:42:12 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-66, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/914330](https://chromium-review.googlesource.com/914330)  
Regress: [mjsunit/regress/wasm/regress-810973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-810973.js), [mjsunit/regress/wasm/regress-810973b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-810973b.js)  
```javascript
this.WScript = new Proxy({}, {
    get() {
      switch (name) {
      }
    }
  });
function MjsUnitAssertionError() {
};
let __v_692 = `(function module() { "use asm";function foo(`;
const __v_693 =
1005;
for (let __v_695 = 0; __v_695 < __v_693; ++__v_695) {
    __v_692 += `arg${__v_695},`;
}
try {
  __v_692 += `arg${__v_693}){`;
} catch (e) {}
for (let __v_696 = 0; __v_696 <= __v_693; ++__v_696) {
    __v_692 += `arg${__v_696}=+arg${__v_696};`;
}
  __v_692 += "return 10;}function bar(){return foo(";
for (let __v_697 = 0; __v_697 < __v_693; ++__v_697) {
    __v_692 += "0.0,";
}
  __v_692 += "1.0)|0;}";

__v_692 += "return bar})()()";

const __v_694 = eval(__v_692);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73d6072^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=73d6072)  
[test/mjsunit/regress/wasm/regress-810973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-810973.js?cl=73d6072)  
  

---   

## **regress-7364.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/7364)**  
**[Commit: [wasm] Reexported wasm functions should be identical to imports](https://chromium.googlesource.com/v8/v8/+/384ac3c)**  
  
Date(Commit): Mon Feb 12 14:27:18 2018  
Code Review: [https://chromium-review.googlesource.com/901626](https://chromium-review.googlesource.com/901626)  
Regress: [mjsunit/regress/wasm/regress-7364.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7364.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const exportingModuleBinary = (() => {
  const builder = new WasmModuleBuilder();
  builder.addFunction('f', kSig_i_v).addBody([kExprI32Const, 42]).exportFunc();
  return builder.toBuffer();
})();

const exportingModule = new WebAssembly.Module(exportingModuleBinary);
const exportingInstance = new WebAssembly.Instance(exportingModule);

const reExportingModuleBinary = (() => {
  const builder = new WasmModuleBuilder();
  const gIndex = builder.addImport('a', 'g', kSig_i_v);
  builder.addExport('y', gIndex);
  return builder.toBuffer();
})();

const module = new WebAssembly.Module(reExportingModuleBinary);
const imports = {
  a: {g: exportingInstance.exports.f},
};
const instance = new WebAssembly.Instance(module, imports);

assertEquals(instance.exports.y, exportingInstance.exports.f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/384ac3c^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=384ac3c)  
[test/mjsunit/regress/wasm/regress-7364.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7364.js?cl=384ac3c)  
[test/mjsunit/wasm/jsapi-harness.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/jsapi-harness.js?cl=384ac3c)  
  

---   

## **regress-7422.js (v8 issue)**  
   
**[Issue: [Liftoff] Unity-Liftoff broken on ia32](https://crbug.com/v8/7422)**  
**[Commit: [Liftoff] Fix caller frame slots generated from stack values](https://chromium.googlesource.com/v8/v8/+/3c47499)**  
  
Date(Commit): Thu Feb 08 13:47:20 2018  
Code Review: [https://chromium-review.googlesource.com/908554](https://chromium-review.googlesource.com/908554)  
Regress: [mjsunit/regress/wasm/regress-7422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7422.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
sig = makeSig([kWasmI32, kWasmI32, kWasmI32, kWasmI32, kWasmI32], [kWasmI32]);
builder.addFunction(undefined, sig).addBody([kExprLocalGet, 4]);
builder.addMemory(16, 32);
builder.addFunction('main', sig)
    .addBody([
      kExprI32Const, 0, kExprLocalSet, 0,
      // Compute five arguments to the function call.
      kExprI32Const, 0, kExprI32Const, 0, kExprI32Const, 0, kExprI32Const, 0,
      kExprLocalGet, 4, kExprI32Const, 1, kExprI32Add,
      // Now some intermediate computation to force the arguments to be spilled
      // to the stack:
      kExprLocalGet, 0, kExprI32Const, 1, kExprI32Add, kExprLocalGet, 1,
      kExprLocalGet, 1, kExprI32Add, kExprI32Add, kExprDrop,
      // Now call the function.
      kExprCallFunction, 0
    ])
    .exportFunc();
var instance = builder.instantiate();
assertEquals(11, instance.exports.main(2, 4, 6, 8, 10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c47499^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=3c47499)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=3c47499)  
[test/mjsunit/regress/wasm/regress-7422.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7422.js?cl=3c47499)  
  

---   

## **regress-crbug-807096.js (chromium issue)**  
   
**[Issue: Security: Arrow function scope fixing bug](https://crbug.com/807096)**  
**[Commit: [parser] More carefully handle destructuring in arrow params](https://chromium.googlesource.com/v8/v8/+/f1a5518)**  
  
Date(Commit): Wed Feb 07 18:14:28 2018  
Components: Blink>JavaScript>Parser  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified, M-65, Merge-Rejected-64, merge-merged-65, Release-0-M65  
Code Review: [https://chromium-review.googlesource.com/900168](https://chromium-review.googlesource.com/900168)  
Regress: [mjsunit/regress/regress-crbug-807096.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-807096.js)  
```javascript
load('test/mjsunit/test-async.js');


let f = ({a = (({b = {a = c} = {
  a: 0x1234
}}) => 1)({})}, c) => 1;

assertThrows(() => f({}), ReferenceError);

let g = ({a = (async ({b = {a = c} = {
  a: 0x1234
}}) => 1)({})}, c) => a;

testAsync(assert => {
  assert.plan(1);
  g({}).catch(e => {
    assert.equals("ReferenceError", e.name);
  });
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1a5518^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=f1a5518)  
[test/mjsunit/regress/regress-crbug-807096.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-807096.js?cl=f1a5518)  
  

---   

## **regress-808012.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::wasm::NativeModuleSerializer::MeasureCode](https://crbug.com/808012)**  
**[Commit: [wasm] Ensure WasmCode always has protected instructions.](https://chromium.googlesource.com/v8/v8/+/61391f3)**  
  
Date(Commit): Mon Feb 05 22:01:56 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/899122](https://chromium-review.googlesource.com/899122)  
Regress: [mjsunit/regress/wasm/regress-808012.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-808012.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_i).addBody([kExprUnreachable]);
let module = new WebAssembly.Module(builder.toBuffer());
var worker = new Worker('onmessage = function() {};', {type: 'string'});
worker.postMessage(module);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/61391f3^!)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=61391f3)  
[src/wasm/wasm-code-manager.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.h?cl=61391f3)  
[test/mjsunit/regress/wasm/regress-808012.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-808012.js?cl=61391f3)  
  

---   

## **regress-808980.js (chromium issue)**  
   
**[Issue: [v8] Uninitialized wasm_compiled_module for deserialized module](https://crbug.com/808980)**  
**[Commit: [wasm] Set wasm_compiled_module for script of deserialized module](https://chromium.googlesource.com/v8/v8/+/34a8204)**  
  
Date(Commit): Mon Feb 05 16:48:00 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: reward-3500, Security_Impact-Stable, Security_Severity-Medium, allpublic, reward-inprocess, Via-Wizard-Security, M-65, merge-merged-6.5  
Code Review: [https://chromium-review.googlesource.com/901306](https://chromium-review.googlesource.com/901306)  
Regress: [mjsunit/regress/wasm/regress-808980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-808980.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');
let kTableSize = 3;

var builder = new WasmModuleBuilder();
var sig_index1 = builder.addType(kSig_i_v);
builder.addFunction('main', kSig_i_ii).addBody([
    kExprLocalGet,
    0,
    kExprCallIndirect,
    sig_index1,
    kTableZero
]).exportAs('main');
builder.setTableBounds(kTableSize, kTableSize);
var m1_bytes = builder.toBuffer();
var m1 = new WebAssembly.Module(m1_bytes);

var serialized_m1 = %SerializeWasmModule(m1);
var m1_clone = %DeserializeWasmModule(serialized_m1, m1_bytes);
var i1 = new WebAssembly.Instance(m1_clone);

i1.exports.main(123123);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34a8204^!)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=34a8204)  
[test/mjsunit/regress/wasm/regress-808980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-808980.js?cl=34a8204)  
  

---   

## **regress-6703.js (v8 issue)**  
   
**[Issue: Character mapping across Latin1 and non-Latin1 not properly handled in TextNode](https://crbug.com/v8/6703)**  
**[Commit: [regexp] fix Latin1 ignore-case bug.](https://chromium.googlesource.com/v8/v8/+/8e9eba3)**  
  
Date(Commit): Mon Feb 05 09:00:20 2018  
Code Review: [https://chromium-review.googlesource.com/897523](https://chromium-review.googlesource.com/897523)  
Regress: [mjsunit/regress/regress-6703.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6703.js)  
```javascript
assertTrue(/(\u039C)/i.test("\xB5"));
assertTrue(/(\u039C)+/i.test("\xB5"));
assertTrue(/(\u039C)/ui.test("\xB5"));
assertTrue(/(\u039C)+/ui.test("\xB5"));

assertTrue(/(\u03BC)/i.test("\xB5"));
assertTrue(/(\u03BC)+/i.test("\xB5"));
assertTrue(/(\u03BC)/ui.test("\xB5"));
assertTrue(/(\u03BC)+/ui.test("\xB5"));

assertTrue(/(\u03BC)/i.test("\u039C"));
assertTrue(/(\u03BC)+/i.test("\u039C"));
assertTrue(/(\u03BC)/ui.test("\u039C"));
assertTrue(/(\u03BC)+/ui.test("\u039C"));

assertTrue(/(\u0178)/i.test("\xFF"));
assertTrue(/(\u0178)+/i.test("\xFF"));
assertTrue(/(\u0178)/ui.test("\xFF"));
assertTrue(/(\u0178)+/ui.test("\xFF"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8e9eba3^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=8e9eba3)  
[src/unicode-decoder.h](https://cs.chromium.org/chromium/src/v8/src/unicode-decoder.h?cl=8e9eba3)  
[test/mjsunit/regress/regress-6703.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6703.js?cl=8e9eba3)  
  

---   

## **regress-802060.js (chromium issue)**  
   
**[Issue: DCHECK failure in op->IsAnyLocationOperand() in instruction.h](https://crbug.com/802060)**  
**[Commit: Fix bug in x64 immediate operand handling for smi-converting loads](https://chromium.googlesource.com/v8/v8/+/9ef2ed3)**  
  
Date(Commit): Thu Feb 01 14:44:19 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-CC, Test-Predator-Auto-Components, merge-merged-6.5  
Code Review: [https://chromium-review.googlesource.com/897723](https://chromium-review.googlesource.com/897723)  
Regress: [mjsunit/regress/regress-802060.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-802060.js)  
```javascript
function assertEquals(expected, found) {
  found.length !== expected.length;
}
assertEquals([], []);
assertEquals("a", "a");
assertEquals([], []);
function f() {
  assertEquals(0, undefined);
};
%PrepareFunctionForOptimization(f);
try {
  f();
} catch (e) {
}
%OptimizeFunctionOnNextCall(f);
try {
  f();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9ef2ed3^!)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=9ef2ed3)  
[test/mjsunit/regress/regress-802060.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-802060.js?cl=9ef2ed3)  
  

---   

## **regress-800651.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/800651)**  
**[Commit: [promise] Remove incorrect fast path](https://chromium.googlesource.com/v8/v8/+/0f6eafe)**  
  
Date(Commit): Wed Jan 31 19:19:56 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, ClusterFuzz-Wrong, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/895416](https://chromium-review.googlesource.com/895416)  
Regress: [mjsunit/regress/regress-800651.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-800651.js)  
```javascript
var list = [];
function log(item) { list.push(item); }
async function f() {
  try {
    let namespace = await import(/a/);
  } catch(e) {
    log(1);
  }
}
f();

async function importUndefined() {
  try {
    await import({ get toString() { return undefined; }})
  } catch(e) {
    log(2);
  }
}

function g() {
  let namespace = Promise.resolve().then(importUndefined);
}
g();
%PerformMicrotaskCheckpoint();
assertEquals(list, [1,2]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f6eafe^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=0f6eafe)  
[test/mjsunit/regress/regress-5691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5691.js?cl=0f6eafe)  
[test/mjsunit/regress/regress-800651.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-800651.js?cl=0f6eafe)  
  

---   

## **regress-5691.js (v8 issue)**  
   
**[Issue: Promise subclass resolved too early](https://crbug.com/v8/5691)**  
**[Commit: [promise] Remove incorrect fast path](https://chromium.googlesource.com/v8/v8/+/0f6eafe)**  
  
Date(Commit): Wed Jan 31 19:19:56 2018  
Code Review: [https://chromium-review.googlesource.com/895416](https://chromium-review.googlesource.com/895416)  
Regress: [mjsunit/regress/regress-5691.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5691.js)  
```javascript
var log = "";
var result;
Promise.resolve()
  .then(() => log += "|turn1")
  .then(() => log += "|turn2")
  .then(() => log += "|turn3")
  .then(() => log += "|turn4")
  .then(() => result = "|start|turn1|fast-resolve|turn2|turn3|slow-resolve|turn4\n"+log)
  .catch(e => print("ERROR", e));

Promise.resolve(Promise.resolve()).then(() => log += "|fast-resolve");
(class extends Promise {}).resolve(Promise.resolve()).then(() => log += "|slow-resolve");

log += "|start";
%PerformMicrotaskCheckpoint();
assertEquals("|start|turn1|fast-resolve|turn2|turn3|slow-resolve|turn4\n\
|start|turn1|fast-resolve|turn2|turn3|slow-resolve|turn4", result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f6eafe^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=0f6eafe)  
[test/mjsunit/regress/regress-5691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5691.js?cl=0f6eafe)  
[test/mjsunit/regress/regress-800651.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-800651.js?cl=0f6eafe)  
  

---   

## **regress-crbug-806388.js (chromium issue)**  
   
**[Issue: Security: A bug in JSFunction::GetDerivedMap](https://crbug.com/806388)**  
**[Commit: [runtime] Fix derived class instantiation](https://chromium.googlesource.com/v8/v8/+/8361fa5)**  
  
Date(Commit): Wed Jan 31 12:07:56 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, NodeJS-Backport-Done, M-65, merge-merged-6.4, merge-merged-6.5, CVE-2018-6056, Release-2-M64, CVE_description-submitted  
Code Review: [https://chromium-review.googlesource.com/894942](https://chromium-review.googlesource.com/894942)  
Regress: [mjsunit/regress/regress-crbug-806388.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-806388.js)  
```javascript
class Derived extends Array {
    constructor(a) { throw "error" }
}

let o = Reflect.construct(RegExp, [], Derived);
o.lastIndex = 0x1234;
%HeapObjectVerify(o);

gc();
%HeapObjectVerify(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8361fa5^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8361fa5)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=8361fa5)  
[test/mjsunit/regress/regress-crbug-806388.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-806388.js?cl=8361fa5)  
  

---   

## **regress-805729.js (chromium issue)**  
   
**[Issue: Security: V8: AwaitedPromise update bug](https://crbug.com/805729)**  
**[Commit: Fix bug in async generators.](https://chromium.googlesource.com/v8/v8/+/9c4c717)**  
  
Date(Commit): Wed Jan 31 07:43:28 2018  
Components: Blink>JavaScript  
Labels: Security_Impact-Stable, Security_Severity-Medium, allpublic, M-66, Release-0-M66, CVE-2018-6106, CVE_description-submitted, Hotlist-Torque  
Code Review: [https://chromium-review.googlesource.com/891231](https://chromium-review.googlesource.com/891231)  
Regress: [mjsunit/regress/regress-805729.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-805729.js)  
```javascript
async function* asyncGenerator() {};
let gen = asyncGenerator();
gen.return({ get then() { delete this.then; gen.next(); } });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c4c717^!)  
[src/builtins/builtins-async-generator-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-async-generator-gen.cc?cl=9c4c717)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=9c4c717)  
[src/compiler/access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.cc?cl=9c4c717)  
[src/compiler/access-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.h?cl=9c4c717)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=9c4c717)  
...  
  

---   

## **regress-crbug-805765.js (chromium issue)**  
   
**[Issue: CHECK failure: (location_) != nullptr in handles.h](https://crbug.com/805765)**  
**[Commit: [ignition] Fix wide suspends to also return](https://chromium.googlesource.com/v8/v8/+/830e39a)**  
  
Date(Commit): Mon Jan 29 12:38:33 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Wrong-Components, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/887082](https://chromium-review.googlesource.com/887082)  
Regress: [mjsunit/regress/regress-crbug-805765.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-805765.js)  
```javascript
var code = "(function* gen() {"
for (var i = 0; i < 256; ++i) {
  code += `var v_${i} = 0;`
}
code += `yield; })`

var gen = eval(code);
var g = gen();
g.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/830e39a^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=830e39a)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=830e39a)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=830e39a)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=830e39a)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=830e39a)  
...  
  

---   

## **regress-crbug-806200.js (chromium issue)**  
   
**[Issue: DCHECK failure in !spread_pos.IsValid() in parser-base.h](https://crbug.com/806200)**  
**[Commit: [parser] Throw syntax error for %Foo(...spread)](https://chromium.googlesource.com/v8/v8/+/3249b16)**  
  
Date(Commit): Mon Jan 29 09:57:39 2018  
Components: Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/888922](https://chromium-review.googlesource.com/888922)  
Regress: [mjsunit/regress/regress-crbug-806200.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-806200.js)  
```javascript
assertThrows("%Foo(...spread)", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3249b16^!)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=3249b16)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=3249b16)  
[test/mjsunit/regress/regress-crbug-806200.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-806200.js?cl=3249b16)  
  

---   

## **regress-7369.js (v8 issue)**  
   
**[Issue: parseInt(-0.9) returns 0 instead of -0](https://crbug.com/v8/7369)**  
**[Commit: Fix parseInt fast-path to return -0 when needed](https://chromium.googlesource.com/v8/v8/+/b6e6843)**  
  
Date(Commit): Fri Jan 26 18:17:03 2018  
Code Review: [https://chromium-review.googlesource.com/887428](https://chromium-review.googlesource.com/887428)  
Regress: [mjsunit/regress/regress-7369.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7369.js)  
```javascript
assertEquals(-Infinity, 1/parseInt(-0.9));
assertEquals(-Infinity, 1/parseInt("-0.9"));
assertEquals(-Infinity, 1/parseInt(-0.09));
assertEquals(-Infinity, 1/parseInt(-0.009));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b6e6843^!)  
[src/builtins/builtins-number-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-number-gen.cc?cl=b6e6843)  
[test/mjsunit/regress/regress-7369.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7369.js?cl=b6e6843)  
  

---   

## **regress-crbug-802333.js (chromium issue)**  
   
**[Issue: Security: V8: A bug in the ObjectDescriptor class](https://crbug.com/802333)**  
**[Commit: [runtime] Fix Class Literals](https://chromium.googlesource.com/v8/v8/+/e416e3c)**  
  
Date(Commit): Fri Jan 26 12:21:15 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript>Compiler  
Labels: Security_Severity-Medium, ReleaseBlock-Stable, allpublic, Merge-Rejected-65  
Code Review: [https://chromium-review.googlesource.com/873032](https://chromium-review.googlesource.com/873032)  
Regress: [mjsunit/regress/regress-crbug-802333.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-802333.js)  
```javascript
function deferred_func() {
  class C {
    method1() {}
  }
}

let bound = (a => a).bind(this, 0);

function opt() {
  deferred_func.prototype;  // ReduceJSLoadNamed

  return bound();
};
%PrepareFunctionForOptimization(opt);
assertEquals(0, opt());
%OptimizeFunctionOnNextCall(opt);

assertEquals(0, opt());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e416e3c^!)  
[src/objects/literal-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.cc?cl=e416e3c)  
[test/mjsunit/regress/regress-crbug-802333.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-802333.js?cl=e416e3c)  
  

---   

## **regress-charat-empty.js (other issue)**  
   
**[Commit: [turbofan] Speculate on bounds checks for String#char[Code]At](https://chromium.googlesource.com/v8/v8/+/ee2d85a)**  
  
Date(Commit): Fri Jan 26 12:00:58 2018  
Code Review: [https://chromium-review.googlesource.com/873382](https://chromium-review.googlesource.com/873382)  
Regress: [mjsunit/regress/regress-charat-empty.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-charat-empty.js)  
```javascript
(() => {
  function f(s) {
    return s.charAt();
  };
  %PrepareFunctionForOptimization(f);
  f('');
  f("");
  %OptimizeFunctionOnNextCall(f);
  f("");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee2d85a^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=ee2d85a)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=ee2d85a)  
[test/mjsunit/constant-folding-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/constant-folding-2.js?cl=ee2d85a)  
[test/mjsunit/regress/regress-charat-empty.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-charat-empty.js?cl=ee2d85a)  
  
  
---   

## **regress-805768.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/805768)**  
**[Commit: Reland "[ic] Improve performance of KeyedStoreIC on literal-based arrays."](https://chromium.googlesource.com/v8/v8/+/024d349)**  
  
Date(Commit): Fri Jan 26 11:11:03 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/876014](https://chromium-review.googlesource.com/876014)  
Regress: [mjsunit/regress/regress-805768.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-805768.js)  
```javascript
function foo() {
  var a = [''];
  print(a[0]);
  return a;
}

function bar(a) {
  a[0] = 'bazinga!';
};
%PrepareFunctionForOptimization(bar);
for (var i = 0; i < 5; i++) bar([]);

%OptimizeFunctionOnNextCall(bar);
bar(foo());
assertEquals([''], foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/024d349^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=024d349)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=024d349)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=024d349)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=024d349)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=024d349)  
...  
  

---   

## **regress-804188.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/804188)**  
**[Commit: [builtins] Fix Collection constructor when entries have custom iteration.](https://chromium.googlesource.com/v8/v8/+/55efb6c)**  
  
Date(Commit): Thu Jan 25 11:11:29 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, v8-foozzie-failure, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/883065](https://chromium-review.googlesource.com/883065)  
Regress: [mjsunit/regress/regress-804188.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804188.js)  
```javascript
Object.defineProperty(Array.prototype, Symbol.iterator, {
  value: function* () {}
});
const arrayIteratorProto = Object.getPrototypeOf([][Symbol.iterator]());
arrayIteratorProto.next = function() {};

assertThrows(() => new Map([[{}, 1], [{}, 2]]), TypeError);
assertThrows(() => new WeakMap([[{}, 1], [{}, 2]]), TypeError);
assertThrows(() => new Set([{}]), TypeError);
assertThrows(() => new WeakSet([{}]), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55efb6c^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=55efb6c)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=55efb6c)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=55efb6c)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=55efb6c)  
[src/compiler/code-assembler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-assembler.h?cl=55efb6c)  
...  
  

---   

## **regress-804176.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/804176)**  
**[Commit: [builtins] Fix Collection constructor when entries have custom iteration.](https://chromium.googlesource.com/v8/v8/+/55efb6c)**  
  
Date(Commit): Thu Jan 25 11:11:29 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/883065](https://chromium-review.googlesource.com/883065)  
Regress: [mjsunit/regress/regress-804176.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804176.js)  
```javascript
const set_entries = [{}];
set_entries[Symbol.iterator] = function() {};
assertThrows(() => new Set(set_entries), TypeError);
assertThrows(() => new WeakSet(set_entries), TypeError);

const map_entries = [[{}, 1]];
map_entries[Symbol.iterator] = function() {};
assertThrows(() => new Set(map_entries), TypeError);
assertThrows(() => new WeakSet(map_entries), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55efb6c^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=55efb6c)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=55efb6c)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=55efb6c)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=55efb6c)  
[src/compiler/code-assembler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-assembler.h?cl=55efb6c)  
...  
  

---   

## **regress-7366.js (v8 issue)**  
   
**[Issue: [Liftoff] Stack transfers reuse stack slots](https://crbug.com/v8/7366)**  
**[Commit: [Liftoff] Fix register spilling on stack transfer](https://chromium.googlesource.com/v8/v8/+/ad98ba7)**  
  
Date(Commit): Wed Jan 24 19:42:48 2018  
Code Review: [https://chromium-review.googlesource.com/883802](https://chromium-review.googlesource.com/883802)  
Regress: [mjsunit/regress/wasm/regress-7366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7366.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_i_iii).addBody([
  // Return the sum of all arguments.
  kExprLocalGet, 0, kExprLocalGet, 1, kExprLocalGet, 2, kExprI32Add, kExprI32Add
]);
const sig = builder.addType(kSig_i_iii);
builder.addFunction(undefined, kSig_i_iii)
    .addBody([
      ...wasmI32Const(1),         // i32.const 0x1
      kExprLocalSet, 0,           // set_local 0
      ...wasmI32Const(4),         // i32.const 0x1
      kExprLocalSet, 1,           // set_local 1
      ...wasmI32Const(16),        // i32.const 0x1
      kExprLocalSet, 2,           // set_local 2
      kExprLoop, kWasmStmt,       // loop
      kExprEnd,                   // end
      kExprLocalGet, 0,           // get_local 0
      kExprLocalGet, 1,           // get_local 1
      kExprLocalGet, 2,           // get_local 2
      kExprI32Const, 0,           // i32.const 0 (func index)
      kExprCallIndirect, sig, 0,  // call indirect
    ])
    .exportAs('main');
builder.appendToTable([0]);
const instance = builder.instantiate();
assertEquals(21, instance.exports.main());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad98ba7^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=ad98ba7)  
[test/mjsunit/regress/wasm/regress-7366.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7366.js?cl=ad98ba7)  
  

---   

## **regress-stringAt-boundsCheck.js (other issue)**  
   
**[Commit: [turbofan] Add effects to StringAt operators](https://chromium.googlesource.com/v8/v8/+/90e50cc2)**  
  
Date(Commit): Wed Jan 24 12:12:27 2018  
Code Review: [https://chromium-review.googlesource.com/881781](https://chromium-review.googlesource.com/881781)  
Regress: [mjsunit/regress/regress-stringAt-boundsCheck.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-stringAt-boundsCheck.js)  
```javascript
(() => {
  function f(u) {
    for (var j = 0; j < 20; ++j) {
      print('' + u.codePointAt());
    }
  };
  %PrepareFunctionForOptimization(f);
  f("test");
  f("foo");
  %OptimizeFunctionOnNextCall(f);
  f("");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90e50cc2^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=90e50cc2)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=90e50cc2)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=90e50cc2)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=90e50cc2)  
[test/mjsunit/regress/regress-stringAt-boundsCheck.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-stringAt-boundsCheck.js?cl=90e50cc2)  
  
  
---   

## **regress-804801.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(add_func) at ../../src/builtins/builtins-collections-ge](https://crbug.com/804801)**  
**[Commit: [builtins] Allow bound function / proxy `add` in collection ctors](https://chromium.googlesource.com/v8/v8/+/c0a6e85)**  
  
Date(Commit): Wed Jan 24 09:49:14 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Stable, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/883121](https://chromium-review.googlesource.com/883121)  
Regress: [mjsunit/regress/regress-804801.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804801.js)  
```javascript
function f() { return 42; }
const bound_function = f.bind();
const callable_proxy = new Proxy(function(){}.__proto__, {});

function testSet(ctor) {
  new ctor([]);
  new ctor([{},{}]);
}

function testMap(ctor) {
  new ctor([]);
  new ctor([[{},{}],[{},{}]]);
}

function testAllVariants(set_or_add_function) {
  Set.prototype.add = set_or_add_function;
  testSet(Set);

  WeakSet.prototype.add = set_or_add_function;
  testSet(WeakSet);

  Map.prototype.set = set_or_add_function;
  testMap(Map);

  WeakMap.prototype.set = set_or_add_function;
  testMap(WeakMap);
}

testAllVariants(bound_function);
testAllVariants(callable_proxy);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c0a6e85^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=c0a6e85)  
[test/mjsunit/regress/regress-804801.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-804801.js?cl=c0a6e85)  
  

---   

## **regress-804177.js (chromium issue)**  
   
**[Issue: DCHECK failure in map() != GetHeap()->fixed_cow_array_map() in fixed-array-inl.h](https://crbug.com/804177)**  
**[Commit: [builtins] Fix Array.of crashes by setting length correctly](https://chromium.googlesource.com/v8/v8/+/d5dca89)**  
  
Date(Commit): Tue Jan 23 21:59:16 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-66, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/880922](https://chromium-review.googlesource.com/880922)  
Regress: [mjsunit/regress/regress-804177.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804177.js)  
```javascript
(function testUnshift() {
  a = [1];
  function f() {
    return a;
  }
  b = Array.of.call(f);
  b.unshift(2);
  assertEquals(b, [2]);
})();

(function testInsertionPastEnd() {
  a = [9,9,9,9];
  function f() {
    return a;
  }
  b = Array.of.call(f,1,2);
  b[4] = 1;
  assertEquals(b, [1, 2, undefined, undefined, 1]);
})();

(function testFrozenArrayThrows() {
  function f() {
    return Object.freeze([1,2,3]);
  }
  assertThrows(function() { Array.of.call(f); }, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5dca89^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=d5dca89)  
[test/mjsunit/regress/regress-804177.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-804177.js?cl=d5dca89)  
  

---   

## **regress-804837.js (chromium issue)**  
   
**[Issue: CHECK failure: LoadElement of kRepFloat64 (NumberOrHole) cannot be changed to kRepTagged in rep](https://crbug.com/804837)**  
**[Commit: [turbofan] Fix typer bug in Array.p.reduce[Right]](https://chromium.googlesource.com/v8/v8/+/a9796a1)**  
  
Date(Commit): Tue Jan 23 17:20:17 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/880967](https://chromium-review.googlesource.com/880967)  
Regress: [mjsunit/regress/regress-804837.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804837.js)  
```javascript
var __v_25662 = [, 1.8];
function __f_6214(__v_25668) {
  __v_25662.reduce(() => {
    1;
  });
};
%PrepareFunctionForOptimization(__f_6214);
__f_6214();
__f_6214();
%OptimizeFunctionOnNextCall(__f_6214);
__f_6214();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9796a1^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=a9796a1)  
[test/mjsunit/regress/regress-804837.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-804837.js?cl=a9796a1)  
  

---   

## **regress-803022.js (chromium issue)**  
   
**[Issue: DCHECK failure in current_ == next_ in node.h](https://crbug.com/803022)**  
**[Commit: [turbofan] Fix dead loop exit removal.](https://chromium.googlesource.com/v8/v8/+/b711332)**  
  
Date(Commit): Tue Jan 23 17:07:57 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-3500, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, reward-inprocess, ClusterFuzz-Verified, M-66, Test-Predator-Auto-CC, Test-Predator-Auto-Components, Merge-Rejected-66, Release-0-M66  
Code Review: [https://chromium-review.googlesource.com/878329](https://chromium-review.googlesource.com/878329)  
Regress: [mjsunit/compiler/regress-803022.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-803022.js)  
```javascript
function foo() {
  for (var a = 0; a < 2; a++) {
    if (a === 1) %OptimizeOsr();
    while (0 && 1) {
      for (var j = 1; j < 2; j++) { }
    }
  }
}

%PrepareFunctionForOptimization(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b711332^!)  
[src/compiler/dead-code-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.cc?cl=b711332)  
[test/mjsunit/compiler/regress-803022.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-803022.js?cl=b711332)  
  

---   

## **regress-7353.js (v8 issue)**  
   
**[Issue: [Liftoff] 32 bit values contain garbage in the upper bits after loading from stack](https://crbug.com/v8/7353)**  
**[Commit: [Liftoff] Fill registers as the right type](https://chromium.googlesource.com/v8/v8/+/ecb3afc)**  
  
Date(Commit): Tue Jan 23 11:45:15 2018  
Code Review: [https://chromium-review.googlesource.com/880682](https://chromium-review.googlesource.com/880682)  
Regress: [mjsunit/regress/wasm/regress-7353.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7353.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addFunction('grow', kSig_i_i).addBody([
  kExprLocalGet, 0,
  kExprMemoryGrow, 0,
]).exportFunc();
builder.addFunction('main', kSig_i_i).addBody([
  ...wasmI32Const(0x41),
  kExprLocalSet, 0,
  // Enter loop, such that values are spilled to the stack.
  kExprLoop, kWasmStmt,
  kExprEnd,
  // Reload value. This must be loaded as 32 bit value.
  kExprLocalGet, 0,
  kExprI32LoadMem, 0, 0,
]).exportFunc();
const instance = builder.instantiate();
instance.exports.grow(1);
instance.exports.main();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecb3afc^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=ecb3afc)  
[src/wasm/baseline/arm64/liftoff-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm64/liftoff-assembler-arm64.h?cl=ecb3afc)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=ecb3afc)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=ecb3afc)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=ecb3afc)  
...  
  

---   

## **regress-803427.js (chromium issue)**  
   
**[Issue: DCHECK failure in (native_module_->lazy_builtin_) == nullptr in wasm-serialization.cc](https://crbug.com/803427)**  
**[Commit: [wasm] Remove {NativeModule::lazy_builtin} field.](https://chromium.googlesource.com/v8/v8/+/e11c57f)**  
  
Date(Commit): Mon Jan 22 17:27:15 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-66, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/878581](https://chromium-review.googlesource.com/878581)  
Regress: [mjsunit/regress/wasm/regress-803427.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-803427.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
let module = new WebAssembly.Module(builder.toBuffer());
var worker = new Worker('onmessage = function() {};', {type: 'string'});
worker.postMessage(module)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e11c57f^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=e11c57f)  
[src/wasm/wasm-code-manager.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.cc?cl=e11c57f)  
[src/wasm/wasm-code-manager.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-manager.h?cl=e11c57f)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=e11c57f)  
[test/mjsunit/regress/wasm/regress-803427.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-803427.js?cl=e11c57f)  
  

---   

## **regress-804288.js (chromium issue)**  
   
**[Issue: DCHECK failure in IsNativeContext() in contexts-inl.h](https://crbug.com/804288)**  
**[Commit: [typedarray] Use native context in elements accessor.](https://chromium.googlesource.com/v8/v8/+/2cfacb7)**  
  
Date(Commit): Mon Jan 22 14:27:22 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Low, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/878324](https://chromium-review.googlesource.com/878324)  
Regress: [mjsunit/regress/regress-804288.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804288.js)  
```javascript
var arr = [{}];
Object.setPrototypeOf(arr, {});
var ta = new Uint8Array(arr);

let kDeclNoLocals = 0;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2cfacb7^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=2cfacb7)  
[test/mjsunit/regress/regress-804288.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-804288.js?cl=2cfacb7)  
  

---   

## **regress-801785.js (chromium issue)**  
   
**[Issue: Unreachable code in objects.cc](https://crbug.com/801785)**  
**[Commit: [wasm] Fix printing of reloc info on the native heap](https://chromium.googlesource.com/v8/v8/+/d414d80)**  
  
Date(Commit): Mon Jan 22 13:49:21 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/875271](https://chromium-review.googlesource.com/875271)  
Regress: [mjsunit/regress/wasm/regress-801785.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-801785.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


const builder = new WasmModuleBuilder();
builder.addMemory(8, 16);
builder.addFunction(undefined, kSig_i_i).addBody([
  // wasm to wasm call.
  kExprLocalGet, 0, kExprCallFunction, 0x1
]);
builder.addFunction(undefined, kSig_i_i).addBody([
  // load from <get_local 0> to create trap code.
  kExprLocalGet, 0, kExprI32LoadMem, 0,
  // unreachable to create a runtime call.
  kExprUnreachable
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d414d80^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=d414d80)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=d414d80)  
[test/mjsunit/regress/wasm/regress-801785.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-801785.js?cl=d414d80)  
  

---   

## **regress-803788.js (chromium issue)**  
   
**[Issue: DCHECK failure in wasm::WasmCode::kLazyStub == code->kind() in module-compiler.cc](https://crbug.com/803788)**  
**[Commit: [wasm] Fix lazy compilation with native-heap code.](https://chromium.googlesource.com/v8/v8/+/f30a86c)**  
  
Date(Commit): Mon Jan 22 13:11:11 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/878262](https://chromium-review.googlesource.com/878262)  
Regress: [mjsunit/regress/wasm/regress-803788.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-803788.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
let q_table = builder.addImportedTable("q", "table")
let q_base = builder.addImportedGlobal("q", "base", kWasmI32);
let q_fun = builder.addImport("q", "fun", kSig_v_v);
builder.addType(kSig_i_ii);
builder.addElementSegment(0, q_base, true, [ q_fun ])
let module = new WebAssembly.Module(builder.toBuffer());
let table = new WebAssembly.Table({
  element: "anyfunc",
  initial: 10,
});
let instance = new WebAssembly.Instance(module, {
  q: {
    base: 0,
    table: table,
    fun: () => (0)
  }
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f30a86c^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=f30a86c)  
[test/mjsunit/regress/wasm/regress-803788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-803788.js?cl=f30a86c)  
  

---   

## **regress-804096.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::Sweeper::EnsurePageIsIterable](https://crbug.com/804096)**  
**[Commit: [turbofan] Fix deoptimization framestate in A.p.reduce[Right]](https://chromium.googlesource.com/v8/v8/+/9e47513)**  
  
Date(Commit): Mon Jan 22 12:14:06 2018  
Components: Blink>JavaScript>GC  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-Medium, Arch-All, Security_Impact-Beta, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-66, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/878120](https://chromium-review.googlesource.com/878120)  
Regress: [mjsunit/regress/regress-804096.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-804096.js)  
```javascript
for (let i = 0; i < 5000; i++) {
  try {
    [].reduce(function() {});
  } catch (x) {
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e47513^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=9e47513)  
[src/deoptimize-reason.h](https://cs.chromium.org/chromium/src/v8/src/deoptimize-reason.h?cl=9e47513)  
[test/mjsunit/regress/regress-804096.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-804096.js?cl=9e47513)  
  

---   

## **regress-803750.js (chromium issue)**  
   
**[Issue: CHECK failure: size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/803750)**  
**[Commit: Fix Array.of crashing when called with lots of parameters](https://chromium.googlesource.com/v8/v8/+/08b0ff2)**  
  
Date(Commit): Fri Jan 19 16:11:18 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/876011](https://chromium-review.googlesource.com/876011)  
Regress: [mjsunit/regress/regress-803750.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-803750.js)  
```javascript
assertEquals(Array.isArray(Array.of.apply(Array, Array(65536))), true);
assertEquals(Array.isArray(Array.of.apply(null, Array(65536))), true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/08b0ff2^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=08b0ff2)  
[test/mjsunit/regress/regress-803750.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-803750.js?cl=08b0ff2)  
  

---   

## **regress-802244.js (chromium issue)**  
   
**[Issue: DCHECK failure in dst == src in liftoff-assembler.cc](https://crbug.com/802244)**  
**[Commit: [Liftoff] Fix registers spilling](https://chromium.googlesource.com/v8/v8/+/cb903d8)**  
  
Date(Commit): Wed Jan 17 09:41:04 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-CC, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/868022](https://chromium-review.googlesource.com/868022)  
Regress: [mjsunit/regress/wasm/regress-802244.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-802244.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_iii).addBody([
  kExprI32Const, 0x41,  // i32.const 0x41
  kExprLoop, 0x7c,      // loop f64
  kExprLocalGet, 0x00,  // get_local 0
  kExprLocalGet, 0x01,  // get_local 1
  kExprBrIf, 0x01,      // br_if depth=1
  kExprLocalGet, 0x00,  // get_local 0
  kExprI32Rol,          // i32.rol
  kExprBrIf, 0x00,      // br_if depth=0
  kExprUnreachable,     // unreachable
  kExprEnd,             // end
  kExprUnreachable,     // unreachable
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb903d8^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=cb903d8)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=cb903d8)  
[test/mjsunit/regress/wasm/regress-802244.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-802244.js?cl=cb903d8)  
  

---   

## **regress-801772.js (chromium issue)**  
   
**[Issue: DCHECK failure in scope_data_->ReadUint32() == static_cast<uint32_t>(name->length()) in preparsed-](https://crbug.com/801772)**  
**[Commit: [parser] Fix declaration order of "arguments" and func name.](https://chromium.googlesource.com/v8/v8/+/9bc4e56)**  
  
Date(Commit): Wed Jan 17 08:29:20 2018  
Components: Blink>JavaScript, Blink>JavaScript>Parser  
Labels: Reproducible, Stability-Memory-AddressSanitizer, External-Fuzzer-Contribution, reward-0, Security_Severity-Low, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/865900](https://chromium-review.googlesource.com/865900)  
Regress: [mjsunit/regress/regress-801772.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-801772.js)  
```javascript
function foo(f) { f(); }

foo(function arguments() {
    function skippable() { }
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bc4e56^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=9bc4e56)  
[test/mjsunit/regress/regress-801772.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-801772.js?cl=9bc4e56)  
  

---   

## **regress-801850.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::wasm::NativeModule::GetCode](https://crbug.com/801850)**  
**[Commit: [wasm] Fix serialization of empty modules.](https://chromium.googlesource.com/v8/v8/+/0465c76)**  
  
Date(Commit): Mon Jan 15 14:25:18 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/866773](https://chromium-review.googlesource.com/866773)  
Regress: [mjsunit/regress/wasm/regress-801850.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-801850.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
let module = new WebAssembly.Module(builder.toBuffer());
var worker = new Worker('onmessage = function() {};', {type: 'string'});
worker.postMessage(module)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0465c76^!)  
[src/wasm/wasm-serialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-serialization.cc?cl=0465c76)  
[test/mjsunit/regress/wasm/regress-801850.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-801850.js?cl=0465c76)  
  

---   

## **regress-crbug-801627.js (chromium issue)**  
   
**[Issue: Security: V8: JIT: Type confusion in NodeProperties::InferReceiverMaps](https://crbug.com/801627)**  
**[Commit: [turbofan] Fix type confusion in NodeProperties::InferReceiverMaps.](https://chromium.googlesource.com/v8/v8/+/e272a2f)**  
  
Date(Commit): Mon Jan 15 06:56:47 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Security_Severity-Medium, Arch-All, allpublic, merge-merged-6.4  
Code Review: [https://chromium-review.googlesource.com/866493](https://chromium-review.googlesource.com/866493)  
Regress: [mjsunit/regress/regress-crbug-801627.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-801627.js)  
```javascript
class Base {
  constructor() {
    this.x = 1;
  }
}

class Derived extends Base {
  constructor() {
    super();
  }
}

%PrepareFunctionForOptimization(Derived);
Reflect.construct(Derived, [], Object.bind());
%OptimizeFunctionOnNextCall(Derived);
new Derived();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e272a2f^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=e272a2f)  
[test/mjsunit/regress/regress-crbug-801627.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-801627.js?cl=e272a2f)  
  

---   

## **regress-801097.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::compiler::PrintCode](https://crbug.com/801097)**  
**[Commit: [TurboFan] Fix null-dereference on code-gen failure.](https://chromium.googlesource.com/v8/v8/+/5637889)**  
  
Date(Commit): Fri Jan 12 14:40:08 2018  
Components: Blink>JavaScript, Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/861882](https://chromium-review.googlesource.com/861882)  
Regress: [mjsunit/compiler/regress-801097.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-801097.js)  
```javascript
function GetFunction() {
  var source = "return ((dividend | 0) / ((";
  for (var i = 0; i < 0x8000; i++) {
    source += "a,"
  }
  source += "a) | 0)) | 0";
  return Function("dividend", source);
}

var func = GetFunction();
%PrepareFunctionForOptimization(func);
assertThrows("func();");
%OptimizeFunctionOnNextCall(func);
assertThrows("func()");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5637889^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=5637889)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=5637889)  
[test/mjsunit/compiler/regress-801097.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-801097.js?cl=5637889)  
  

---   

## **regress-crbug-800810.js (chromium issue)**  
   
**[Issue: DCHECK failure in receiver->map() == *original_map in elements.cc](https://crbug.com/800810)**  
**[Commit: [elements] Fix overzealous DCHECK in Array.prototype.includes](https://chromium.googlesource.com/v8/v8/+/b785d2a)**  
  
Date(Commit): Fri Jan 12 14:07:44 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/860927](https://chromium-review.googlesource.com/860927)  
Regress: [mjsunit/regress/regress-crbug-800810.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-800810.js)  
```javascript
var array = [];
Object.defineProperty(array , 506519, {});
Object.defineProperty(array , 3, {
  get: function () {
      Object.defineProperty(array , undefined, {
   })
  }
});
array.includes(61301);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b785d2a^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=b785d2a)  
[test/mjsunit/regress/regress-crbug-800810.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-800810.js?cl=b785d2a)  
  

---   

## **regress-800538.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/800538)**  
**[Commit: [regexp] Fix fast/slow-path dispatch in RegExp.p.get flags](https://chromium.googlesource.com/v8/v8/+/4e14a2a)**  
  
Date(Commit): Fri Jan 12 14:06:09 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/859767](https://chromium-review.googlesource.com/859767)  
Regress: [mjsunit/regress/regress-800538.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-800538.js)  
```javascript
RegExp.prototype.__defineGetter__("global", () => true);
assertEquals("/()/g", /()/.toString());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e14a2a^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=4e14a2a)  
[test/mjsunit/regress/regress-800538.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-800538.js?cl=4e14a2a)  
  

---   

## **regress-801171.js (chromium issue)**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/801171)**  
**[Commit: [regexp] Fix spec ordering issue in @@split](https://chromium.googlesource.com/v8/v8/+/557e79c)**  
  
Date(Commit): Fri Jan 12 13:00:39 2018  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz, ClusterFuzz-Verified, v8-foozzie-failure, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/863644](https://chromium-review.googlesource.com/863644)  
Regress: [mjsunit/regress/regress-801171.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-801171.js)  
```javascript
let called_custom_unicode_getter = false;
const re = /./;

function f() {
  re.__defineGetter__("unicode", function() {
    called_custom_unicode_getter = true;
  });
  return 2;
}

assertEquals(["","",], re[Symbol.split]("abc", { valueOf: f }));

assertFalse(called_custom_unicode_getter);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/557e79c^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=557e79c)  
[test/mjsunit/regress/regress-801171.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-801171.js?cl=557e79c)  
  

---   

## **regress-crbug-800032.js (chromium issue)**  
   
**[Issue: Security: V8: Bugs in Genesis::InitializeGlobal](https://crbug.com/800032)**  
**[Commit: [Runtime] Set expected_nof_properties when creating Constructors](https://chromium.googlesource.com/v8/v8/+/42e8ca9)**  
  
Date(Commit): Fri Jan 12 10:51:11 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, M-65, merge-merged-6.4, Release-1-M64  
Code Review: [https://chromium-review.googlesource.com/861886](https://chromium-review.googlesource.com/861886)  
Regress: [mjsunit/regress/regress-crbug-800032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-800032.js)  
```javascript
class Derived extends RegExp {
  constructor(a) { throw "error" }
}

let o = Reflect.construct(RegExp, [], Derived);
%HeapObjectVerify(o);
assertEquals(o.lastIndex, 0);
o.lastIndex = 1;
assertEquals(o.lastIndex, 1);
o.lastIndex = 0;
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/42e8ca9^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=42e8ca9)  
[test/mjsunit/regress/regress-crbug-800032.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-800032.js?cl=42e8ca9)  
  

---   

## **regress-797481.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::Simulator::LoadStorePairHelper](https://crbug.com/797481)**  
**[Commit: [regexp] Add stack check to RegExpExec](https://chromium.googlesource.com/v8/v8/+/e1f676e)**  
  
Date(Commit): Thu Jan 11 15:39:34 2018  
Components: Blink>JavaScript>Regexp  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-Stable, Stability-Memory-MemorySanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Release-0-M65  
Code Review: [https://chromium-review.googlesource.com/856777](https://chromium-review.googlesource.com/856777)  
Regress: [mjsunit/regress/regress-797481.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-797481.js)  
```javascript
const a = /x/;

a.exec = RegExp.prototype.test;
assertThrows(() => RegExp.prototype.test.call(a), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1f676e^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=e1f676e)  
[test/mjsunit/regress/regress-797481.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-797481.js?cl=e1f676e)  
  

---   

## **regress-800756.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/800756)**  
**[Commit: [Liftoff] Fix i32.eqz on ia32](https://chromium.googlesource.com/v8/v8/+/29e4696)**  
  
Date(Commit): Thu Jan 11 14:55:24 2018  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/860640](https://chromium-review.googlesource.com/860640)  
Regress: [mjsunit/regress/wasm/regress-800756.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-800756.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32);
builder.addFunction(undefined, kSig_i_iii).addBody([
  kExprI32Const, 0,         // i32.const 0
  kExprI32LoadMem8S, 0, 0,  // i32.load8_s offset=0 align=0
  kExprI32Eqz,              // i32.eqz
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/29e4696^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=29e4696)  
[test/mjsunit/regress/wasm/regress-800756.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-800756.js?cl=29e4696)  
  

---   

## **regress-crbug-798644.js (chromium issue)**  
   
**[Issue: Security: V8: Type confusion in ElementsAccessorBase::CollectValuesOrEntriesImpl](https://crbug.com/798644)**  
**[Commit: [elements] Fix Object.entries/values with changing elements](https://chromium.googlesource.com/v8/v8/+/be9c5fd)**  
  
Date(Commit): Wed Jan 10 13:50:20 2018  
Components: Blink>JavaScript>Runtime  
Labels: Hotlist-Merge-Review, Security_Impact-Stable, Security_Severity-High, allpublic, ClusterFuzz-Verified, M-65, Merge-Rejected-64, Release-0-M65, CVE-2018-6064, CVE_description-submitted  
Code Review: [https://chromium-review.googlesource.com/856816](https://chromium-review.googlesource.com/856816)  
Regress: [mjsunit/regress/regress-crbug-798644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-798644.js)  
```javascript
let arr = [];
arr[1000] = 0x1234;

arr.__defineGetter__(256, function () {
    // Remove the getter so we can compact the array.
    delete arr[256];
    // Trigger compaction.
    arr.unshift(1.1);
});

let results = Object.entries(arr);
%HeapObjectVerify(results);
%HeapObjectVerify(arr);
let str = results.toString();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/be9c5fd^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=be9c5fd)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=be9c5fd)  
[test/mjsunit/es8/object-entries.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es8/object-entries.js?cl=be9c5fd)  
[test/mjsunit/regress/regress-crbug-798644.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-798644.js?cl=be9c5fd)  
  

---   

## **regress-797581.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::AstNumberingVisitor::VisitEmptyParentheses](https://crbug.com/797581)**  
**[Commit: [parser] Fix: disallow "export default ()".](https://chromium.googlesource.com/v8/v8/+/15eb10b)**  
  
Date(Commit): Wed Jan 10 09:32:50 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/856937](https://chromium-review.googlesource.com/856937)  
Regress: [mjsunit/regress/regress-797581.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-797581.js)  
```javascript
function TryToLoadModule(filename, expect_error, token) {
  let caught_error;

  function SetError(e) {
    caught_error = e;
  }

  import(filename).catch(SetError);
  %PerformMicrotaskCheckpoint();

  if (expect_error) {
    assertTrue(caught_error instanceof SyntaxError);
    assertEquals("Unexpected token '" + token + "'", caught_error.message);
  } else {
    assertEquals(undefined, caught_error);
  }
}

TryToLoadModule("modules-skip-regress-797581-1.js", true, ")");
TryToLoadModule("modules-skip-regress-797581-2.js", true, ")");
TryToLoadModule("modules-skip-regress-797581-3.js", true, "...");
TryToLoadModule("modules-skip-regress-797581-4.js", true, ",");
TryToLoadModule("modules-skip-regress-797581-5.js", false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15eb10b^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=15eb10b)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-1.js?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-2.js?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-3.js?cl=15eb10b)  
...  
  

---   

## **modules-skip-regress-797581-1.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::AstNumberingVisitor::VisitEmptyParentheses](https://crbug.com/797581)**  
**[Commit: [parser] Fix: disallow "export default ()".](https://chromium.googlesource.com/v8/v8/+/15eb10b)**  
  
Date(Commit): Wed Jan 10 09:32:50 2018  
Components: Blink>JavaScript>Language, Blink>JavaScript>Parser  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Stability-Libfuzzer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [https://chromium-review.googlesource.com/856937](https://chromium-review.googlesource.com/856937)  
Regress: [mjsunit/regress/modules-skip-regress-797581-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/modules-skip-regress-797581-1.js), [mjsunit/regress/modules-skip-regress-797581-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/modules-skip-regress-797581-2.js), [mjsunit/regress/modules-skip-regress-797581-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/modules-skip-regress-797581-3.js), [mjsunit/regress/modules-skip-regress-797581-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/modules-skip-regress-797581-4.js), [mjsunit/regress/modules-skip-regress-797581-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/modules-skip-regress-797581-5.js)  
```javascript
export default ()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15eb10b^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=15eb10b)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-1.js?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-2.js?cl=15eb10b)  
[test/mjsunit/regress/modules-skip-regress-797581-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/modules-skip-regress-797581-3.js?cl=15eb10b)  
...  
  

---   

## **regress-799690.js (chromium issue)**  
   
**[Issue: DCHECK failure in total_offset == offset_table->get_int(kOTESize * left) in wasm-objects.cc](https://crbug.com/799690)**  
**[Commit: [asm] Store source position for all loops](https://chromium.googlesource.com/v8/v8/+/54cb64a)**  
  
Date(Commit): Tue Jan 09 13:56:28 2018  
Components: Blink>JavaScript>WebAssembly  
Labels: Hotlist-Merge-Review, Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, Test-Predator-Auto-Owner, merge-merged-6.4  
Code Review: [https://chromium-review.googlesource.com/856338](https://chromium-review.googlesource.com/856338)  
Regress: [mjsunit/regress/regress-799690.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-799690.js)  
```javascript
function asm() {
  "use asm";
  function f(a) {
    a = a | 0;
    while (1) return 1;
    return 0;
  }
  return { f: f};
}
const mod = asm();
function call_f() {
  mod.f();
  call_f();
}
assertThrows(call_f, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54cb64a^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=54cb64a)  
[test/mjsunit/regress/regress-799690.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-799690.js?cl=54cb64a)  
  

---   

## **regress-797846.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::Shell::CreateRealm](https://crbug.com/797846)**  
**[Commit: [d8] Run the message loop in the same RealmScope as the script](https://chromium.googlesource.com/v8/v8/+/1016e62)**  
  
Date(Commit): Tue Jan 09 13:51:41 2018  
Components: Blink>JavaScript, Blink>JavaScript>WebAssembly  
Labels: Stability-Crash, Reproducible, Needs-Feedback, Clusterfuzz, Stability-UndefinedBehaviorSanitizer, ClusterFuzz-Verified, Test-Predator-Wrong-CLs, Test-Predator-Auto-Owner  
Code Review: [https://chromium-review.googlesource.com/856497](https://chromium-review.googlesource.com/856497)  
Regress: [mjsunit/regress/wasm/regress-797846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-797846.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_v_v).addBody([]);

const buffer = builder.toBuffer();
assertPromiseResult(
    WebAssembly.compile(buffer), _ => Realm.createAllowCrossRealmAccess());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1016e62^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=1016e62)  
[test/mjsunit/regress/wasm/regress-797846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-797846.js?cl=1016e62)  
  

---   

## **regress-crbug-800077.js (chromium issue)**  
   
**[Issue: CHECK failure: Type cast failed in CAST(key) at ../../src/code-stub-assembler.cc:7137 in code-a](https://crbug.com/800077)**  
**[Commit: [csa] Fix type casing in GetProperty](https://chromium.googlesource.com/v8/v8/+/8643720)**  
  
Date(Commit): Tue Jan 09 12:56:07 2018  
Components: Blink>JavaScript  
Labels: Reproducible, Security_Impact-Stable, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, M-65, ClusterFuzz-Top-Crash, Test-Predator-Auto-CC  
Code Review: [https://chromium-review.googlesource.com/855138](https://chromium-review.googlesource.com/855138)  
Regress: [mjsunit/regress/regress-crbug-800077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-800077.js)  
```javascript
var sample = new Float64Array(1);
Reflect.has(sample, undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8643720^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=8643720)  
[test/mjsunit/regress/regress-crbug-800077.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-800077.js?cl=8643720)  
  

---   

## **regress-799813.js (chromium issue)**  
   
**[Issue: DCHECK failure in index >= 0 && index < length() in string-inl.h](https://crbug.com/799813)**  
**[Commit: [regexp] Properly handle large values in AdvanceStringIndex](https://chromium.googlesource.com/v8/v8/+/3f8d6f6)**  
  
Date(Commit): Tue Jan 09 12:03:55 2018  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Hotlist-Merge-Review, Reproducible, Stability-Memory-AddressSanitizer, Security_Severity-High, allpublic, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components, Test-Predator-Auto-Owner, merge-merged-6.4  
Code Review: [https://chromium-review.googlesource.com/854272](https://chromium-review.googlesource.com/854272)  
Regress: [mjsunit/regress/regress-799813.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-799813.js)  
```javascript
function testAdvanceLastIndex(initial_last_index_value,
                              expected_final_last_index_value) {
  let exec_call_count = 0;
  let last_index_setter_call_count = 0;
  let final_last_index_value;

  var customRegexp = {
    get global() { return true; },
    get unicode() { return true; },
    get lastIndex() {
      return initial_last_index_value;
    },
    set lastIndex(v) {
      last_index_setter_call_count++;
      final_last_index_value = v;
    },
    exec() {
      return (exec_call_count++ == 0) ? [""] : null;
    }
  };

  RegExp.prototype[Symbol.replace].call(customRegexp);

  assertEquals(2, exec_call_count);
  assertEquals(2, last_index_setter_call_count);
  assertEquals(expected_final_last_index_value, final_last_index_value);
}

testAdvanceLastIndex(-1, 1);
testAdvanceLastIndex( 0, 1);
testAdvanceLastIndex(2**31 - 2, 2**31 - 1);
testAdvanceLastIndex(2**31 - 1, 2**31 - 0);
testAdvanceLastIndex(2**32 - 3, 2**32 - 2);
testAdvanceLastIndex(2**32 - 2, 2**32 - 1);
testAdvanceLastIndex(2**32 - 1, 2**32 - 0);
testAdvanceLastIndex(2**53 - 2, 2**53 - 1);
testAdvanceLastIndex(2**53 - 1, 2**53 - 0);
testAdvanceLastIndex(2**53 - 0, 2**53 - 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f8d6f6^!)  
[src/conversions-inl.h](https://cs.chromium.org/chromium/src/v8/src/conversions-inl.h?cl=3f8d6f6)  
[src/conversions.h](https://cs.chromium.org/chromium/src/v8/src/conversions.h?cl=3f8d6f6)  
[src/regexp/regexp-utils.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-utils.cc?cl=3f8d6f6)  
[src/regexp/regexp-utils.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-utils.h?cl=3f8d6f6)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=3f8d6f6)  
...  
  

---   

## **regress-v8-7245.js (v8 issue)**  
   
**[Issue: "name" property of Proxy revocation functions set incorrectly](https://crbug.com/v8/7245)**  
**[Commit: [builtins] Port Proxy.revocable() to CSA](https://chromium.googlesource.com/v8/v8/+/ddfbbc5)**  
  
Date(Commit): Sun Jan 07 10:20:13 2018  
Code Review: [https://chromium-review.googlesource.com/844065](https://chromium-review.googlesource.com/844065)  
Regress: [mjsunit/regress/regress-v8-7245.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-7245.js)  
```javascript
const { revoke } = Proxy.revocable({}, {});
assertEquals("", revoke.name);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ddfbbc5^!)  
[AUTHORS](https://cs.chromium.org/chromium/src/v8/AUTHORS?cl=ddfbbc5)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=ddfbbc5)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=ddfbbc5)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=ddfbbc5)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=ddfbbc5)  
...  
  

---   

## **regress-799263.js (chromium issue)**  
   
**[Issue: Security: V8: JIT: A bug in LoadElimination::ReduceTransitionElementsKind](https://crbug.com/799263)**  
**[Commit: [turbofan] Kill transition-kind source map in load elimination.](https://chromium.googlesource.com/v8/v8/+/6b30393)**  
  
Date(Commit): Fri Jan 05 10:53:41 2018  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Merge-Review, Security_Severity-High, allpublic, merge-merged-6.4  
Code Review: [https://chromium-review.googlesource.com/852072](https://chromium-review.googlesource.com/852072)  
Regress: [mjsunit/compiler/regress-799263.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-799263.js)  
```javascript
function opt(a, b) {
  b[0] = 0;

  a.length;

  // TransitionElementsKind
  for (let i = 0; i < 1; i++)
      a[0] = 0;

  b[0] = 9.431092e-317;
}

%PrepareFunctionForOptimization(opt);

let arr1 = new Array(1);
arr1[0] = 'a';
opt(arr1, [0]);

let arr2 = [0.1];
opt(arr2, arr2);

%OptimizeFunctionOnNextCall(opt);

opt(arr2, arr2);
assertEquals(9.431092e-317, arr2[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b30393^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=6b30393)  
[test/mjsunit/compiler/regress-799263.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-799263.js?cl=6b30393)  
  

---   

## **regress-crbug-798026.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/798026)**  
**[Commit: [Builtins] Eliminate the fast path in constructor entries](https://chromium.googlesource.com/v8/v8/+/a10689d)**  
  
Date(Commit): Thu Jan 04 15:29:00 2018  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/850356](https://chromium-review.googlesource.com/850356)  
Regress: [mjsunit/regress/regress-crbug-798026.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-798026.js)  
```javascript
array = new Array(4 * 1024 * 1024);
Set.prototype.add = value => {
  if (array.length != 1) {
    array.length = 1;
    gc();
  }
}
new Set(array);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a10689d^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=a10689d)  
[test/mjsunit/regress/regress-crbug-798026.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-798026.js?cl=a10689d)  
  

---   

## **regress-796041.js (chromium issue)**  
   
**[Issue: Null-dereference READ in v8::internal::Invoke](https://crbug.com/796041)**  
**[Commit: [turbofan] add regression test for chromium:796041](https://chromium.googlesource.com/v8/v8/+/dbc377e)**  
  
Date(Commit): Thu Jan 04 00:36:09 2018  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Fracas, Clusterfuzz, ClusterFuzz-Wrong, M-65, M-64, Merge-Rejected-64, FoundIn-M-64, Test-Predator-Auto-CC, Test-Predator-Auto-Components, FoundIn-M-65  
Code Review: [https://chromium-review.googlesource.com/848995](https://chromium-review.googlesource.com/848995)  
Regress: [mjsunit/compiler/regress-796041.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-796041.js)  
```javascript
'use strict';

function f(abort, n, a, b) {
  if (abort) return;
  var x = a ? true : "" + a;
  if (!a) {
    var dead = n + 1 + 1;
    if(!b) {
      x = dead;
    }
    if (x) {
      x = false;
    }
    if (b) {
      x = false;
    }
  }
  return x + 1;
}
f(false, 5); f(false, 6); f(false, 7); f(false, 8);

function g(abort, a, b) {
  return f(abort, "abc", a, b);
}

%PrepareFunctionForOptimization(g);
g(true); g(true); g(true); g(true);

%OptimizeFunctionOnNextCall(g);
g(false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dbc377e^!)  
[test/mjsunit/compiler/regress-796041.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-796041.js?cl=dbc377e)  
  

---   
