# V8Harvest  
The Harvest of V8 regress in 2020.  
  

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

## **regress-1055692.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1055692)**  
**[Commit: [wasm-simd] Fix OpcodeLength of load splat/extend ops](https://chromium.googlesource.com/v8/v8/+/a67a16a)**  
  
Date(Commit): Wed Feb 26 02:57:20 2020  
Components: None  
Labels: None  
Code Review: [https://chromium-review.googlesource.com/c/v8/v8/+/2071401](https://chromium-review.googlesource.com/c/v8/v8/+/2071401)  
Regress: [mjsunit/regress/wasm/regress-1055692.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-1055692.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addMemory(16, 32, false);
builder.addType(makeSig([kWasmI32, kWasmI32, kWasmI32], [kWasmI32]));
builder.addFunction(undefined, 0 /* sig */)
  .addBodyWithEnd([
kExprI32Const, 0x75,  // i32.const
kExprI32Const, 0x74,  // i32.const
kExprI32Const, 0x18,  // i32.const
kSimdPrefix, kExprS8x16LoadSplat,  // s8x16.load_splat
kExprUnreachable,  // unreachable
kExprUnreachable,  // unreachable
kExprI32Const, 0x6f,  // i32.const
kExprI32Const, 0x7f,  // i32.const
kExprI32Const, 0x6f,  // i32.const
kExprDrop,
kExprDrop,
kExprDrop,
kExprDrop,
kExprDrop,
kExprEnd,  // end @18
]);
builder.addExport('main', 0);
const instance = builder.instantiate();
print(instance.exports.main(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a67a16a^!)  
[src/wasm/wasm-opcodes.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-opcodes.h?cl=a67a16a)  
[test/mjsunit/regress/wasm/regress-1055692.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-1055692.js?cl=a67a16a)  
[test/mjsunit/wasm/wasm-module-builder.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-module-builder.js?cl=a67a16a)  
  

---   

## **regress-1054466.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1054466)**  
**[Commit: [liftoff] Check fp_pair when looking up register for reuse](https://chromium.googlesource.com/v8/v8/+/548fda4)**  
  
Date(Commit): Mon Feb 24 12:24:06 2020  
Components: None  
Labels: None  
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
kSimdPrefix, kExprI32x4Add,  // i32x4.add
kSimdPrefix, kExprI32x4Add,  // i32x4.add
kSimdPrefix, kExprS1x8AnyTrue,  // s1x8.any_true
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
   
**[Issue: Permission denied](https://crbug.com/1050046)**  
**[Commit: [keys] Make sure we don't leak the enum cache in slow-mode for/in](https://chromium.googlesource.com/v8/v8/+/4b0916a)**  
  
Date(Commit): Thu Feb 20 16:44:41 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1051912)**  
**[Commit: [wasm] Fix streaming compilation prefix hash](https://chromium.googlesource.com/v8/v8/+/80c7ab4)**  
  
Date(Commit): Thu Feb 13 20:53:17 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1051017)**  
**[Commit: [turbofan] Fix bug in Typer::TypeInductionVariablePhi](https://chromium.googlesource.com/v8/v8/+/a2e971c)**  
  
Date(Commit): Thu Feb 13 13:29:25 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1048241)**  
**[Commit: [liftoff][ia32] Fix AtomicStore register spilling](https://chromium.googlesource.com/v8/v8/+/0e2e50d)**  
  
Date(Commit): Tue Feb 04 09:39:54 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1047368)**  
**[Commit: [ast] Flatten Wasm function names](https://chromium.googlesource.com/v8/v8/+/6abbfe2)**  
  
Date(Commit): Fri Jan 31 11:25:45 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1045225)**  
**[Commit: [Liftoff] Clean up implementation of AtomicStore](https://chromium.googlesource.com/v8/v8/+/d8bb229)**  
  
Date(Commit): Fri Jan 31 08:54:44 2020  
Components: None  
Labels: None  
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
      'WebAssembly.Module(): Compiling function #0 failed: type error in ' +
          'merge[0] (expected <bot>, got i32) @+57');
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
   
**[Issue: Permission denied](https://crbug.com/1038178)**  
**[Commit: Add missing test for optional chains with undefined receiver](https://chromium.googlesource.com/v8/v8/+/0bc9e52)**  
  
Date(Commit): Tue Jan 14 20:11:57 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1041616)**  
**[Commit: [parser] Fix cache scope recursion for `with`](https://chromium.googlesource.com/v8/v8/+/a85d74a)**  
  
Date(Commit): Tue Jan 14 13:57:47 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1041210)**  
**[Commit: [parser] Fix caching dynamic vars on wrong scope](https://chromium.googlesource.com/v8/v8/+/304e97d)**  
  
Date(Commit): Mon Jan 13 15:06:15 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1040403)**  
**[Commit: [turbofan] Allow handle deferences when compiling non concurrently](https://chromium.googlesource.com/v8/v8/+/36cb5f4)**  
  
Date(Commit): Thu Jan 09 12:54:51 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1038588)**  
**[Commit: [parser] Fix conflict detection loop early exit](https://chromium.googlesource.com/v8/v8/+/2a6c0f4)**  
  
Date(Commit): Tue Jan 07 13:15:05 2020  
Components: None  
Labels: None  
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
   
**[Issue: Permission denied](https://crbug.com/1033966)**  
**[Commit: [protectors] Remove invalid DCHECK in protectors.](https://chromium.googlesource.com/v8/v8/+/643ae46)**  
  
Date(Commit): Thu Jan 02 15:49:54 2020  
Components: None  
Labels: None  
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
