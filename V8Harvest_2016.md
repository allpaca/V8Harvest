# V8Harvest  
The Harvest of V8 regress in 2016.  
  

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
