# V8Harvest  
The Harvest of V8 regress in 2019.  
  

## **regress-918149.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/918149)**  
**[Commit: [Liftoff][ia32] Fix i64 sign extension on non-byte register](https://chromium.googlesource.com/v8/v8/+/5ed7dff)**  
  
Date(Commit): Fri Jan 04 10:12:06 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1394555](https://chromium-review.googlesource.com/c/1394555)  
Regress: [mjsunit/regress/wasm/regress-918149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918149.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/917412)**  
**[Commit: [Liftoff] Keep consistent register mapping in non-merged regions](https://chromium.googlesource.com/v8/v8/+/20b6330)**  
  
Date(Commit): Thu Jan 03 14:37:48 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1392190](https://chromium-review.googlesource.com/c/1392190)  
Regress: [mjsunit/regress/wasm/regress-917412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917412.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/917588)**  
**[Commit: [Liftoff] Fix moving stack values](https://chromium.googlesource.com/v8/v8/+/14faced)**  
  
Date(Commit): Thu Jan 03 14:25:47 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1391755](https://chromium-review.googlesource.com/c/1391755)  
Regress: [mjsunit/regress/wasm/regress-917588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917588.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/917988)**  
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
