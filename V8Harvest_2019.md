# V8Harvest  
The Harvest of V8 regress in 2019.  
  

## **regress-crbug-934166.js (chromium issue)**  
   
**[No Permission](https://crbug.com/934166)**  
**[Commit: [ignition] Skip binding dead labels](https://chromium.googlesource.com/v8/v8/+/35269f7)**  
  
Date(Commit): Thu Feb 28 12:17:34 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1488763](https://chromium-review.googlesource.com/c/1488763)  
Regress: [mjsunit/regress/regress-crbug-934166.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-934166.js)  
```javascript
{
  for(let i = 0; i < 10; ++i){
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
  function foo() { return obj[0]; };
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

## **regress-crbug-936302.js (chromium issue)**  
   
**[No Permission](https://crbug.com/936302)**  
**[Commit: [turbofan] Always pass the right arity to calls.](https://chromium.googlesource.com/v8/v8/+/834c4b3)**  
  
Date(Commit): Wed Feb 27 08:40:58 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
  }

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
   
**[Issue 8913:
 String.prototype.concat deopt loop](https://crbug.com/v8/8913)**  
**[Commit: [turbofan] Properly thread through the feedback for HeapObject checks.](https://chromium.googlesource.com/v8/v8/+/066e2a2)**  
  
Date(Commit): Tue Feb 26 14:19:49 2019  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/c/1488770](https://chromium-review.googlesource.com/c/1488770)  
Regress: [mjsunit/regress/regress-8913.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-8913.js)  
```javascript
function foo(t) { return 'a'.concat(t); }

foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo(1);
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
   
**[No Permission](https://crbug.com/935138)**  
**[Commit: [wasm] Fix {StreamingDecoder} to reject multiple code sections.](https://chromium.googlesource.com/v8/v8/+/85b4ec5)**  
  
Date(Commit): Tue Feb 26 09:59:44 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1486471](https://chromium-review.googlesource.com/c/1486471)  
Regress: [mjsunit/regress/wasm/regress-935138.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-935138.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function TestAsyncCompileMultipleCodeSections() {
  let binary = new Binary();
  binary.emit_header();
  binary.push(kTypeSectionCode, 4, 1, kWasmFunctionTypeForm, 0, 0);
  binary.push(kFunctionSectionCode, 2, 1, 0);
  binary.push(kCodeSectionCode, 6, 1, 4, 0, kExprGetLocal, 0, kExprEnd);
  binary.push(kCodeSectionCode, 6, 1, 4, 0, kExprGetLocal, 0, kExprEnd);
  let buffer = Uint8Array.from(binary).buffer;
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

## **regress-934175.js (chromium issue)**  
   
**[No Permission](https://crbug.com/934175)**  
**[Commit: [turbofan] Re-type JSAdd("", prim) reduction to ToString.](https://chromium.googlesource.com/v8/v8/+/6660639)**  
  
Date(Commit): Fri Feb 22 09:24:53 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1481632](https://chromium-review.googlesource.com/c/1481632)  
Regress: [mjsunit/compiler/regress-934175.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-934175.js)  
```javascript
(function ShortcutEmptyStringAddRight() {
  let ar = new Float32Array(1);
  function opt(i){
    return ar[i] + (NaN ? 0 : '');
  }
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

## **regress-932392.js (chromium issue)**  
   
**[No Permission](https://crbug.com/932392)**  
**[Commit: [turbofan] Handle -0 truncation in word32->tagged rep change.](https://chromium.googlesource.com/v8/v8/+/64bad45)**  
  
Date(Commit): Wed Feb 20 12:48:25 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1478192](https://chromium-review.googlesource.com/c/1478192)  
Regress: [mjsunit/compiler/regress-932392.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-932392.js)  
```javascript
function opt(flag){
  ((flag||(Math.max(-0,0)))==0)
}

try{opt(false)}catch{}
%OptimizeFunctionOnNextCall(opt)
try{opt(false)}catch{}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64bad45^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=64bad45)  
[test/mjsunit/compiler/regress-932392.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-932392.js?cl=64bad45)  
  

---   

## **regress-crbug-932034.js (chromium issue)**  
   
**[No Permission](https://crbug.com/932034)**  
**[Commit: Reland "[builtins]: Optimize CreateTypedArray to use element size log 2 for calculations."](https://chromium.googlesource.com/v8/v8/+/02b9847)**  
  
Date(Commit): Wed Feb 20 12:06:53 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/933179)**  
**[Commit: Remove incorrect dcheck from map updater.](https://chromium.googlesource.com/v8/v8/+/f23712f)**  
  
Date(Commit): Tue Feb 19 19:04:55 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-932953.js (chromium issue)**  
   
**[No Permission](https://crbug.com/932953)**  
**[Commit: Fix accessor update of non-extensible maps.](https://chromium.googlesource.com/v8/v8/+/1a3a2bc)**  
  
Date(Commit): Tue Feb 19 04:59:36 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1477067](https://chromium-review.googlesource.com/c/1477067)  
Regress: [mjsunit/regress/regress-932953.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-932953.js)  
```javascript
(function NonExtensibleBetweenSetterAndGetter() {
  o = {};
  o.x = 42;
  o.__defineGetter__("y", function() { });
  Object.preventExtensions(o);
  o.__defineSetter__("y", function() { });
  o.x = 0.1;
})();

(function InterleavedIntegrityLevel() {
  o = {};
  o.x = 42;
  o.__defineSetter__("y", function() { });
  Object.preventExtensions(o);
  o.__defineGetter__("y", function() { return 44; });
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
   
**[No Permission](https://crbug.com/932101)**  
**[Commit: Relax a too-strict DCHECKs.](https://chromium.googlesource.com/v8/v8/+/0f6f064)**  
  
Date(Commit): Fri Feb 15 07:44:11 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/926651)**  
**[Commit: [ast] Always visit all AST nodes, even dead nodes](https://chromium.googlesource.com/v8/v8/+/9439a1d)**  
  
Date(Commit): Wed Feb 13 15:24:28 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/930948)**  
**[Commit: [array] Fix Array#map storing signaling NaNs](https://chromium.googlesource.com/v8/v8/+/82faa6d)**  
  
Date(Commit): Wed Feb 13 10:23:19 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-crbug-930580.js (chromium issue)**  
   
**[No Permission](https://crbug.com/930580)**  
**[Commit: [parser] Reset expression_scope_ stack to nullptr when parsing a function body](https://chromium.googlesource.com/v8/v8/+/486ec80)**  
  
Date(Commit): Mon Feb 11 09:22:57 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/930045)**  
**[Commit: Fix map updater for non-extensible maps with private symbols.](https://chromium.googlesource.com/v8/v8/+/154bb50)**  
  
Date(Commit): Sat Feb 09 09:09:02 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-crbug-926856.js (chromium issue)**  
   
**[No Permission](https://crbug.com/926856)**  
**[Commit: [Builtins]: Array.prototype.map out of memory error](https://chromium.googlesource.com/v8/v8/+/183b857)**  
  
Date(Commit): Fri Feb 01 12:33:19 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-924843.js (chromium issue)**  
   
**[No Permission](https://crbug.com/924843)**  
**[Commit: [Liftoff] Correctly unuse Labels](https://chromium.googlesource.com/v8/v8/+/3af3c9d)**  
  
Date(Commit): Tue Jan 29 15:18:48 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1442645](https://chromium-review.googlesource.com/c/1442645)  
Regress: [mjsunit/regress/wasm/regress-924843.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-924843.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
const sig = builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, sig)
  .addBody([
    kExprGetLocal, 2,
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
   
**[No Permission](https://crbug.com/925671)**  
**[Commit: [wasm] Distinguish requested tier and executed tier](https://chromium.googlesource.com/v8/v8/+/185922d)**  
  
Date(Commit): Tue Jan 29 12:36:48 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/926036)**  
**[Commit: [parser] Make pattern DCHECK dependent on !has_error](https://chromium.googlesource.com/v8/v8/+/b0e1c2b)**  
  
Date(Commit): Tue Jan 29 11:03:09 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/924905)**  
**[Commit: [wasm][arm] Fix {Word32Shr} instruction selection.](https://chromium.googlesource.com/v8/v8/+/8a3c4d9)**  
  
Date(Commit): Fri Jan 25 13:08:10 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-922933.js (chromium issue)**  
   
**[No Permission](https://crbug.com/922933)**  
**[Commit: [Liftoff][arm] Avoid use of temp registers](https://chromium.googlesource.com/v8/v8/+/ce2bfb8)**  
  
Date(Commit): Mon Jan 21 13:09:13 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
      kExprSetLocal, 0x09,
      kExprI32Const, 0x00,
      kExprIf, kWasmStmt,
        kExprBlock, kWasmStmt,
          kExprI32Const, 0x00,
          kExprSetLocal, 0x0a,
          kExprBr, 0x00,
          kExprEnd,
        kExprBlock, kWasmStmt,
          kExprBlock, kWasmStmt,
            kExprGetLocal, 0x00,
            kExprSetLocal, 0x12,
            kExprBr, 0x00,
            kExprEnd,
          kExprGetLocal, 0x16,
          kExprSetLocal, 0x0f,
          kExprGetLocal, 0x0f,
          kExprSetLocal, 0x17,
          kExprGetLocal, 0x0f,
          kExprSetLocal, 0x18,
          kExprGetLocal, 0x17,
          kExprGetLocal, 0x18,
          kExprI64ShrS,
          kExprSetLocal, 0x19,
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

## **regress-crbug-923264.js (chromium issue)**  
   
**[No Permission](https://crbug.com/923264)**  
**[Commit: [heap] Allow PreparseData in large object space](https://chromium.googlesource.com/v8/v8/+/c45a2ef)**  
  
Date(Commit): Mon Jan 21 11:18:02 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-922432.js (chromium issue)**  
   
**[No Permission](https://crbug.com/922432)**  
**[Commit: [wasm] Fix {OpcodeLength} for invalid br-on-exn opcodes.](https://chromium.googlesource.com/v8/v8/+/30882a5)**  
  
Date(Commit): Wed Jan 16 14:50:13 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
          kExprGetLocal, 0,
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
   
**[No Permission](https://crbug.com/921382)**  
**[Commit: [parser] Clear parenthesized flag on collapsing nary expressions](https://chromium.googlesource.com/v8/v8/+/5f8a3e1)**  
  
Date(Commit): Tue Jan 15 13:26:23 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/917755)**  
**[Commit: [parser] Give hoisting sloppy block functions a valid position](https://chromium.googlesource.com/v8/v8/+/8436715)**  
  
Date(Commit): Tue Jan 15 11:52:28 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-918284.js (chromium issue)**  
   
**[No Permission](https://crbug.com/918284)**  
**[Commit: [Liftoff][arm] Leave scratch register to the assembler](https://chromium.googlesource.com/v8/v8/+/f59d6d9)**  
  
Date(Commit): Fri Jan 11 08:27:16 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
    kExprTeeLocal, 0,
    kExprI32Popcnt
]);
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f59d6d9^!)  
[src/wasm/baseline/arm/liftoff-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/arm/liftoff-assembler-arm.h?cl=f59d6d9)  
[test/mjsunit/regress/wasm/regress-918284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-918284.js?cl=f59d6d9)  
  

---   

## **regress-919754.js (chromium issue)**  
   
**[No Permission](https://crbug.com/919754)**  
**[Commit: [turbofan] Fix invocation frequency computation with NaN.](https://chromium.googlesource.com/v8/v8/+/ef12b47)**  
  
Date(Commit): Thu Jan 10 19:04:05 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef12b47^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=ef12b47)  
[test/mjsunit/compiler/regress-919754.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-919754.js?cl=ef12b47)  
  

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

## **regress-919533.js (chromium issue)**  
   
**[No Permission](https://crbug.com/919533)**  
**[Commit: [Liftoff] Fix reloading register spilled multiple times](https://chromium.googlesource.com/v8/v8/+/24a43b3)**  
  
Date(Commit): Wed Jan 09 16:12:50 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1403124](https://chromium-review.googlesource.com/c/1403124)  
Regress: [mjsunit/regress/wasm/regress-919533.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919533.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_v).addBody([]);
builder.addFunction(undefined, kSig_i_i)
  .addBody([
    kExprGetLocal, 0,
    kExprGetLocal, 0,
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

## **regress-920076.js (chromium issue)**  
   
**[No Permission](https://crbug.com/920076)**  
**[Commit: [asm.js] Fix semicolon insertion in presence of Unicode.](https://chromium.googlesource.com/v8/v8/+/082bfec)**  
  
Date(Commit): Wed Jan 09 12:38:41 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-918917.js (chromium issue)**  
   
**[No Permission](https://crbug.com/918917)**  
**[Commit: [Liftoff] Fix corner case of register moves](https://chromium.googlesource.com/v8/v8/+/f1fb7bc)**  
  
Date(Commit): Tue Jan 08 10:57:05 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1398225](https://chromium-review.googlesource.com/c/1398225)  
Regress: [mjsunit/regress/wasm/regress-918917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918917.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction(undefined, kSig_v_v)
  .addLocals({i32_count: 1}).addLocals({f32_count: 1}).addLocals({f64_count: 1})
  .addBody([
kExprGetLocal, 1,
kExprGetLocal, 2,
kExprGetLocal, 0,
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
   
**[No Permission](https://crbug.com/919340)**  
**[Commit: [turbofan] Restrict redundancy elimination from widening types](https://chromium.googlesource.com/v8/v8/+/5a9fa8f)**  
  
Date(Commit): Tue Jan 08 09:48:28 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1397709](https://chromium-review.googlesource.com/c/1397709)  
Regress: [mjsunit/regress/regress-919340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-919340.js)  
```javascript
var E = 'Σ';
var PI = 123;
function f() {
    print(E = 2, /b/.test(E) || /b/.test(E = 2));
    ((E = 3) * PI);
}

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
   
**[No Permission](https://crbug.com/918763)**  
**[Commit: [turbofan] Add missing heap object check](https://chromium.googlesource.com/v8/v8/+/426312c)**  
  
Date(Commit): Mon Jan 07 14:38:50 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1397707](https://chromium-review.googlesource.com/c/1397707)  
Regress: [mjsunit/regress-918763.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress-918763.js)  
```javascript
function C() {}
C.__proto__ = null;

function f(c) { return 0 instanceof c; }

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
   
**[No Permission](https://crbug.com/918149)**  
**[Commit: [Liftoff][ia32] Fix i64 sign extension on non-byte register](https://chromium.googlesource.com/v8/v8/+/5ed7dff)**  
  
Date(Commit): Fri Jan 04 10:12:06 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/917412)**  
**[Commit: [Liftoff] Keep consistent register mapping in non-merged regions](https://chromium.googlesource.com/v8/v8/+/20b6330)**  
  
Date(Commit): Thu Jan 03 14:37:48 2019  
Components/Type: None/None  
Labels: "No Permission"  
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

## **regress-917588.js (chromium issue)**  
   
**[No Permission](https://crbug.com/917588)**  
**[Commit: [Liftoff] Fix moving stack values](https://chromium.googlesource.com/v8/v8/+/14faced)**  
  
Date(Commit): Thu Jan 03 14:25:47 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
   
**[No Permission](https://crbug.com/917988)**  
**[Commit: Set the correct scope when initializing parameters.](https://chromium.googlesource.com/v8/v8/+/fa844bd)**  
  
Date(Commit): Thu Jan 03 10:18:11 2019  
Components/Type: None/None  
Labels: "No Permission"  
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
