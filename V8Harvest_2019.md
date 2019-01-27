# V8Harvest  
The Harvest of V8 regress in 2019.  
  

## **regress-924905.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/924905)**  
**[Commit: [wasm][arm] Fix {Word32Shr} instruction selection.](https://chromium.googlesource.com/v8/v8/+/8a3c4d9)**  
  
Date(Commit): Fri Jan 25 13:08:10 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1435939](https://chromium-review.googlesource.com/c/1435939)  
Regress: [mjsunit/regress/wasm/regress-924905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-924905.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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

## **regress-922933.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/922933)**  
**[Commit: [Liftoff][arm] Avoid use of temp registers](https://chromium.googlesource.com/v8/v8/+/ce2bfb8)**  
  
Date(Commit): Mon Jan 21 13:09:13 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1424862](https://chromium-review.googlesource.com/c/1424862)  
Regress: [mjsunit/regress/wasm/regress-922933.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-922933.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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

## **regress-crbug-923264.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/923264)**  
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

## **regress-crbug-923265.js (chromium issue)**  
   
**[Issue: Ill in v8::internal::RemoveArrayHolesGeneric](https://crbug.com/923265)**  
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
   
**[Issue: No Permission](https://crbug.com/922432)**  
**[Commit: [wasm] Fix {OpcodeLength} for invalid br-on-exn opcodes.](https://chromium.googlesource.com/v8/v8/+/30882a5)**  
  
Date(Commit): Wed Jan 16 14:50:13 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1414917](https://chromium-review.googlesource.com/c/1414917)  
Regress: [mjsunit/regress/wasm/regress-922432.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-922432.js)  
```javascript
load("test/mjsunit/wasm/wasm-constants.js");
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
   
**[Issue: No Permission](https://crbug.com/921382)**  
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
   
**[Issue: No Permission](https://crbug.com/917755)**  
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
   
**[Issue: False SyntaxError on Sibling JS-Labels](https://crbug.com/917215)**  
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
   
**[Issue: Dcheck on jsfunfuzz](https://crbug.com/v8/8630)**  
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
   
**[Issue: Ill in v8::internal::wasm::fuzzer::WasmExecutionFuzzer::FuzzWasmModule](https://crbug.com/919308)**  
**[Commit: [Liftoff] Fix sub of the same register](https://chromium.googlesource.com/v8/v8/+/8518d12)**  
  
Date(Commit): Fri Jan 11 10:57:09 2019  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Stability-AFL", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/c/1405029](https://chromium-review.googlesource.com/c/1405029)  
Regress: [mjsunit/regress/wasm/regress-919308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919308.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/918284)**  
**[Commit: [Liftoff][arm] Leave scratch register to the assembler](https://chromium.googlesource.com/v8/v8/+/f59d6d9)**  
  
Date(Commit): Fri Jan 11 08:27:16 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1405036](https://chromium-review.googlesource.com/c/1405036)  
Regress: [mjsunit/regress/wasm/regress-918284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918284.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/919754)**  
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
   
**[Issue: Ill in v8::internal::JSObject::JSObjectVerify](https://crbug.com/920184)**  
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
   
**[Issue: No Permission](https://crbug.com/919533)**  
**[Commit: [Liftoff] Fix reloading register spilled multiple times](https://chromium.googlesource.com/v8/v8/+/24a43b3)**  
  
Date(Commit): Wed Jan 09 16:12:50 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1403124](https://chromium-review.googlesource.com/c/1403124)  
Regress: [mjsunit/regress/wasm/regress-919533.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-919533.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: DCHECK failure in block->predecessors().empty() || block->successors().empty() in unwinding-info-w](https://crbug.com/913844)**  
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
   
**[Issue: No Permission](https://crbug.com/920076)**  
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
   
**[Issue: Dcheck on jsfunfuzz](https://crbug.com/v8/8659)**  
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
   
**[Issue: Null-dereference READ in v8::internal::Scope::zone](https://crbug.com/919710)**  
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
   
**[Issue: No Permission](https://crbug.com/918917)**  
**[Commit: [Liftoff] Fix corner case of register moves](https://chromium.googlesource.com/v8/v8/+/f1fb7bc)**  
  
Date(Commit): Tue Jan 08 10:57:05 2019  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/c/1398225](https://chromium-review.googlesource.com/c/1398225)  
Regress: [mjsunit/regress/wasm/regress-918917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-918917.js)  
```javascript
load('test/mjsunit/wasm/wasm-constants.js');
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
   
**[Issue: No Permission](https://crbug.com/919340)**  
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
   
**[Issue: No Permission](https://crbug.com/918763)**  
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
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/917076)**  
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
Regress: [mjsunit/regress/wasm/regress-917588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917588.js), [mjsunit/regress/wasm/regress-917588b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-917588b.js)  
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
