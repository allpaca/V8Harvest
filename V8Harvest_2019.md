# V8Harvest  
The Harvest of V8 regress in 2019.  
  

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
