# V8Harvest  
The Harvest of V8 regress in 2017.  
  

## **regress-797596.js (chromium issue)**  
   
**[Issue 797596:
 DCHECK failure in IrOpcode::kMerge == control->opcode() in node-properties.cc](https://crbug.com/797596)**  
**[Commit: [turbofan] handle dead effect-phi control op in InferReceiverMaps](https://chromium.googlesource.com/v8/v8/+/007f90b)**  
  
Date(Commit): Wed Dec 27 22:14:41 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/844978](https://chromium-review.googlesource.com/844978)  
Regress: [mjsunit/compiler/regress-797596.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-797596.js)  
```javascript
var notCallable;
function inferReceiverMapsInDeadCode() {
  var obj = { func() {} };
  gc();
  function wrappedCode() { try { code(); } catch (e) {} }
  function code() {
    obj.a;
    try {
      Object.defineProperty(obj, "func", { get() {} });
    } catch (neverCaught) {}
    for (var i = 0; i < 1; i++) {
      try {
        notCallable(arguments[i]);
      } catch (alwaysCaught) {}
    }
  }
  wrappedCode();
  try {
    %OptimizeFunctionOnNextCall(wrappedCode);
    wrappedCode();
  } catch (e) {}
}
inferReceiverMapsInDeadCode();
inferReceiverMapsInDeadCode();
inferReceiverMapsInDeadCode();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/007f90b^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=007f90b)  
[test/mjsunit/compiler/regress-797596.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-797596.js?cl=007f90b)  
  

---   

## **regress-796427.js (chromium issue)**  
   
**[Issue 796427:
 Stack-overflow in v8::internal::Object::IsDescriptorArray](https://crbug.com/796427)**  
**[Commit: [builtins] Add Object#toLocaleString stack check](https://chromium.googlesource.com/v8/v8/+/bd1f805)**  
  
Date(Commit): Thu Dec 21 14:24:02 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/838854](https://chromium-review.googlesource.com/838854)  
Regress: [mjsunit/regress/regress-796427.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-796427.js)  
```javascript
assertThrows(() => "" + { toString: Object.prototype.toLocaleString }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd1f805^!)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=bd1f805)  
[test/mjsunit/regress/regress-796427.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-796427.js?cl=bd1f805)  
  

---   

## **regress-794744.js (chromium issue)**  
   
**[Issue 794744:
 Null-dereference READ in v8::internal::FrameFunctionIterator::next](https://crbug.com/794744)**  
**[Commit: [builtins] abort FrameFunctionIterator::next if frame summary empty](https://chromium.googlesource.com/v8/v8/+/18dc491)**  
  
Date(Commit): Wed Dec 20 00:08:35 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/833266](https://chromium-review.googlesource.com/833266)  
Regress: [mjsunit/es8/regress/regress-794744.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es8/regress/regress-794744.js)  
```javascript
Promise.resolve(function () {}).then(Object.getOwnPropertyDescriptors);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18dc491^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=18dc491)  
[test/mjsunit/es8/regress/regress-794744.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es8/regress/regress-794744.js?cl=18dc491)  
  

---   

## **regress-crbug-795922.js (chromium issue)**  
   
**[Issue 795922:
 DCHECK failure in !has_null_prototype() in ast.cc](https://crbug.com/795922)**  
**[Commit: [ignition] Move object/array literal init to bytecode gen](https://chromium.googlesource.com/v8/v8/+/9128e8b)**  
  
Date(Commit): Tue Dec 19 14:50:19 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/833882](https://chromium-review.googlesource.com/833882)  
Regress: [mjsunit/regress/regress-crbug-795922.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-795922.js)  
```javascript
assertThrows(
  // Should throw a syntax error, but not crash.
  "({ __proto__: null, __proto__: 1 })",
  SyntaxError
);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9128e8b^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=9128e8b)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=9128e8b)  
[test/mjsunit/regress/regress-crbug-795922.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-795922.js?cl=9128e8b)  
  

---   

## **regress-794825.js (chromium issue)**  
   
**[Issue 794825:
 Security: V8: Empty BytecodeJumpTable may lead to OOB read](https://crbug.com/794825)**  
**[Commit: [Turbofan] Fix instruction selector to handle switch with no case](https://chromium.googlesource.com/v8/v8/+/f2d85ff)**  
  
Date(Commit): Mon Dec 18 10:17:08 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/830013](https://chromium-review.googlesource.com/830013)  
Regress: [mjsunit/regress/regress-794825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-794825.js)  
```javascript
function* opt() {
  // The for loop to generate a SwitchOnSmiNoFeedback with holes
  // at the end since yield will be eliminated.
  for (;;)
    if (true) {
    } else {
      yield;
    }

  // Another loop to force more holes in the constant pool to
  // verify if bounds checks works when iterating over the jump
  // table.
  for (;;)
    if (true) {
    } else {
      yield;
    }
}

opt();
%OptimizeFunctionOnNextCall(opt);
opt();
assertOptimized(opt);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f2d85ff^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=f2d85ff)  
[test/mjsunit/regress/regress-794825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-794825.js?cl=f2d85ff)  
  

---   

## **regress-793588.js (chromium issue)**  
   
**[Issue 793588:
 Use-of-uninitialized-value in v8::internal::TextNode::GetQuickCheckDetails](https://crbug.com/793588)**  
**[Commit: [regexp] Preserve invariant of non-empty character class](https://chromium.googlesource.com/v8/v8/+/52b4fb0)**  
  
Date(Commit): Mon Dec 18 08:50:39 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-Medium", "Stability-Memory-MemorySanitizer", "Stability-Libfuzzer", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/827010](https://chromium-review.googlesource.com/827010)  
Regress: [mjsunit/regress/regress-793588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-793588.js)  
```javascript
assertNull(/a\P{Any}a/u.exec("a\u{d83d}a"));
assertEquals(["a\u{d83d}a"], /a\p{Any}a/u.exec("a\u{d83d}a"));
assertEquals(["a\u{d83d}a"], /(?:a\P{Any}a|a\p{Any}a)/u.exec("a\u{d83d}a"));
assertNull(/a[\P{Any}]a/u.exec("a\u{d83d}a"));
assertEquals(["a\u{d83d}a"], /a[^\P{Any}]a/u.exec("a\u{d83d}a"));
assertEquals(["a\u{d83d}a"], /a[^\P{Any}x]a/u.exec("a\u{d83d}a"));
assertNull(/a[^\P{Any}x]a/u.exec("axa"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52b4fb0^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=52b4fb0)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=52b4fb0)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=52b4fb0)  
[test/mjsunit/regress/regress-793588.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-793588.js?cl=52b4fb0)  
  

---   

## **regress-793793.js (chromium issue)**  
   
**[Issue 793793:
 Use-after-poison in v8::internal::RegExpParser::GetCapture](https://crbug.com/793793)**  
**[Commit: [regexp] Restrict unicode property value expressions](https://chromium.googlesource.com/v8/v8/+/0da56e7)**  
  
Date(Commit): Fri Dec 15 14:17:34 2017  
Components/Type: Blink>JavaScript>Regexp/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "merge-merged-6.4"]  
Code Review: [https://chromium-review.googlesource.com/824272](https://chromium-review.googlesource.com/824272)  
Regress: [mjsunit/regress/regress-793793.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-793793.js)  
```javascript
assertThrows(() => new RegExp("\\1(\\P{P\0[}()/", "u"), SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0da56e7^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=0da56e7)  
[test/mjsunit/regress/regress-793793.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-793793.js?cl=0da56e7)  
  

---   

## **regress-794822.js (chromium issue)**  
   
**[Issue 794822:
 Security: V8: JIT: Type confusion in GetSpecializationContext](https://crbug.com/794822)**  
**[Commit: [compiler] Don't assume a HeapConstant context input is a Context.](https://chromium.googlesource.com/v8/v8/+/649ab06)**  
  
Date(Commit): Fri Dec 15 13:47:14 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/828954](https://chromium-review.googlesource.com/828954)  
Regress: [mjsunit/regress/regress-794822.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-794822.js)  
```javascript
function* opt(arg = () => arg) {
  let tmp = opt.x;  // LdaNamedProperty
  for (;;) {
    arg;
    yield;
    function inner() { tmp }
    break;
  }
}

opt();
%OptimizeFunctionOnNextCall(opt);
opt();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/649ab06^!)  
[src/compiler/js-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-context-specialization.cc?cl=649ab06)  
[test/mjsunit/regress/regress-794822.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-794822.js?cl=649ab06)  
  

---   

## **regress-crbug-786723.js (chromium issue)**  
   
**[Issue 786723:
 DCHECK failure in !compilation_info()->dependencies() || !compilation_info()->dependencies()->HasA](https://crbug.com/786723)**  
**[Commit: [turbofan] Fix prototype mutation in Object.create lowering.](https://chromium.googlesource.com/v8/v8/+/4a7eec5)**  
  
Date(Commit): Fri Dec 15 12:36:34 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/827066](https://chromium-review.googlesource.com/827066)  
Regress: [mjsunit/regress/regress-crbug-786723.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-786723.js)  
```javascript
function f() {
  var o = {};
  function g() {
    o.x = 1;
    return Object.create(o);
  };
  gc();
  o.x = 10;
  %OptimizeFunctionOnNextCall(g);
  g();
}
f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a7eec5^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=4a7eec5)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4a7eec5)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=4a7eec5)  
[test/mjsunit/regress/regress-crbug-786723.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-786723.js?cl=4a7eec5)  
  

---   

## **regress-crbug-791256.js (chromium issue)**  
   
**[Issue 791256:
 DCHECK failure in kNoSourcePosition != start_position() in scopes.cc](https://crbug.com/791256)**  
**[Commit: [parser] Fix NaryOperation positions.](https://chromium.googlesource.com/v8/v8/+/10d9c31)**  
  
Date(Commit): Tue Dec 12 18:54:03 2017  
Components/Type: Blink>JavaScript>Language/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components", "merge-merged-6.4", "merge-merged-64"]  
Code Review: [https://chromium-review.googlesource.com/822093](https://chromium-review.googlesource.com/822093)  
Regress: [mjsunit/regress/regress-crbug-791256.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-791256.js)  
```javascript
(function* (name = (eval(foo), foo, prototype)) { });

(function (name = (foo, bar, baz) ) { });

(function (param = (0, 1, 2)) { assertEquals(2, param); })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10d9c31^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=10d9c31)  
[test/mjsunit/regress/regress-crbug-791256.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791256.js?cl=10d9c31)  
  

---   

## **regress-793863.js (chromium issue)**  
   
**[Issue 793863:
 CHECK failure: arg_elements == isolate->heap()->empty_fixed_array() in objects-debug.cc](https://crbug.com/793863)**  
**[Commit: [deoptimizer] Use empty fixed array when materializing empty arguments elements.](https://chromium.googlesource.com/v8/v8/+/bee8c16)**  
  
Date(Commit): Tue Dec 12 12:56:13 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/822071](https://chromium-review.googlesource.com/822071)  
Regress: [mjsunit/compiler/regress-793863.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-793863.js)  
```javascript
function f(a) {
  return arguments[0];
}

%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bee8c16^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=bee8c16)  
[test/mjsunit/compiler/regress-793863.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-793863.js?cl=bee8c16)  
  

---   

## **regress-791334.js (chromium issue)**  
   
**[Issue 791334:
 `this` in top level Arrow Function in Module Context should be `undefined`](https://crbug.com/791334)**  
**[Commit: Fix "this" value in lazily-parsed module functions.](https://chromium.googlesource.com/v8/v8/+/c3bd741)**  
  
Date(Commit): Tue Dec 12 12:09:49 2017  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Via-Wizard-Javascript"]  
Code Review: [https://chromium-review.googlesource.com/808938](https://chromium-review.googlesource.com/808938)  
Regress: [mjsunit/regress/regress-791334.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-791334.js)  
```javascript
let foo = () => { return this };
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3bd741^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=c3bd741)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=c3bd741)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=c3bd741)  
[test/cctest/interpreter/bytecode_expectations/Modules.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Modules.golden?cl=c3bd741)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=c3bd741)  
...  
  

---   

## **regress-793551.js (chromium issue)**  
   
**[Issue 793551:
 DCHECK failure in !move_dst_regs.has(dst) in liftoff-assembler.cc](https://crbug.com/793551)**  
**[Commit: [Liftoff] Fix redundant register moves](https://chromium.googlesource.com/v8/v8/+/9678c53)**  
  
Date(Commit): Mon Dec 11 13:47:02 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Security_Impact-None", "Clusterfuzz", "ClusterFuzz-Verified", "M-64"]  
Code Review: [https://chromium-review.googlesource.com/819231](https://chromium-review.googlesource.com/819231)  
Regress: [mjsunit/regress/wasm/regress-793551.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-793551.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_i)
    .addBody([
      // body:
      kExprGetLocal, 0,      // get_local 0
      kExprGetLocal, 0,      // get_local 0
      kExprLoop, kWasmStmt,  // loop
      kExprBr, 0,            // br depth=0
      kExprEnd,              // end
      kExprUnreachable,      // unreachable
    ])
    .exportFunc();
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9678c53^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=9678c53)  
[test/mjsunit/regress/wasm/regress-793551.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-793551.js?cl=9678c53)  
  

---   

## **regress-786784.js (chromium issue)**  
   
**[Issue 786784:
 Crash in v8::internal::Invoke](https://crbug.com/786784)**  
**[Commit: [coverage] Do not reset JSFunction::code post-deoptimization](https://chromium.googlesource.com/v8/v8/+/8303dc5)**  
  
Date(Commit): Thu Dec 07 13:48:31 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Rejected", "M-65", "merge-merged-6.3", "merge-merged-6.4"]  
Code Review: [https://crrev.com/f0acede9bb05155c25ee87e81b4b587e8a76f690](https://crrev.com/f0acede9bb05155c25ee87e81b4b587e8a76f690)  
Regress: [mjsunit/regress/regress-786784.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-786784.js)  
```javascript
function f() {
  function g(arg) { return arg; }
  // The closure contains a call IC slot.
  return function() { return g(42); };
}

const a = Realm.create();
const b = Realm.create();

const x = Realm.eval(a, f.toString() + " f()");
const y = Realm.eval(b, f.toString() + " f()");

x();


%DebugToggleBlockCoverage(true);

y();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8303dc5^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=8303dc5)  
[test/mjsunit/regress/regress-786784.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-786784.js?cl=8303dc5)  
  

---   

## **regress-791810.js (chromium issue)**  
   
**[Issue 791810:
 DCHECK failure in rc == kGpReg || rc == kFpReg in liftoff-assembler.h](https://crbug.com/791810)**  
**[Commit: [Liftoff] Fix cache state initialization](https://chromium.googlesource.com/v8/v8/+/9a91669)**  
  
Date(Commit): Thu Dec 07 10:51:46 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Security_Impact-None", "Clusterfuzz", "ClusterFuzz-Verified", "M-64", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/811188](https://chromium-review.googlesource.com/811188)  
Regress: [mjsunit/regress/wasm/regress-791810.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-791810.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

const builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_i)
    .addBody([
      kExprGetLocal, 0x00,    // get_local 0
      kExprBlock, kWasmStmt,  // block
      kExprBr, 0x00,          // br depth=0
      kExprEnd,               // end
      kExprBlock, kWasmStmt,  // block
      kExprBr, 0x00,          // br depth=0
      kExprEnd,               // end
      kExprBr, 0x00,          // br depth=0
    ])
    .exportFunc();
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a91669^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=9a91669)  
[test/mjsunit/regress/wasm/regress-791810.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-791810.js?cl=9a91669)  
  

---   

## **regress-786521.js (chromium issue)**  
   
**[Issue 786521:
 Breakpoint in v8::internal::Invoke](https://crbug.com/786521)**  
**[Commit: [turbofan] do not remove speculative Number operations when they can deopt](https://chromium.googlesource.com/v8/v8/+/2290ad8)**  
  
Date(Commit): Wed Dec 06 09:16:58 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/793043](https://chromium-review.googlesource.com/793043)  
Regress: [mjsunit/compiler/regress-786521.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-786521.js)  
```javascript
function inlined(b, x) {
  if (b) {
    x * 2 * 2
  }
}

inlined(true, 1);
inlined(true, 2);
inlined(false, 1);

function foo(b) { inlined(b, "") }
foo(false); foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2290ad8^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=2290ad8)  
[test/mjsunit/compiler/regress-786521.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-786521.js?cl=2290ad8)  
  

---   

## **regress-791345.js (chromium issue)**  
   
**[No Permission](https://crbug.com/791345)**  
**[Commit: Fix OOB access in Array.prototype.slice](https://chromium.googlesource.com/v8/v8/+/6f6ca73)**  
  
Date(Commit): Tue Dec 05 14:34:17 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/806097](https://chromium-review.googlesource.com/806097)  
Regress: [mjsunit/regress/regress-791345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-791345.js)  
```javascript
(function(a) {
    var len = 0x80000000;
    arguments.length = len;
    Array.prototype.slice.call(arguments, len - 1, len);
}('a'));

(function(a) {
    var len = 0x40000000;
    arguments.length = len;
    Array.prototype.slice.call(arguments, len - 1, len);
}('a'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f6ca73^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=6f6ca73)  
[test/mjsunit/regress/regress-791345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-791345.js?cl=6f6ca73)  
  

---   

## **regress-791958.js (chromium issue)**  
   
**[Issue 791958:
 Ill in v8::internal::compiler::CodeGenerator::AddTranslationForOperand](https://crbug.com/791958)**  
**[Commit: [compiler] Add regression test exhibiting int64 deopt literals.](https://chromium.googlesource.com/v8/v8/+/7ffc331)**  
  
Date(Commit): Tue Dec 05 14:04:41 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/808804](https://chromium-review.googlesource.com/808804)  
Regress: [mjsunit/regress/regress-791958.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-791958.js)  
```javascript
obj = {m: print};
function foo() {
  for (var x = -536870912; x != -536870903; ++x) {
    obj.m(-x >= 1000000 ? x % 1000000 : y);
  }
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ffc331^!)  
[test/mjsunit/regress/regress-791958.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-791958.js?cl=7ffc331)  
  

---   

## **regress-crbug-791245-1.js (chromium issue)**  
   
**[Issue 791245:
 Security: V8: JIT: Simplified-lowererer IrOpcode::kStoreField, IrOpcode::kStoreElement optimization bug](https://crbug.com/791245)**  
**[Commit: [turbofan] Properly type the OrderedHashTableHealIndex builtin result.](https://chromium.googlesource.com/v8/v8/+/3ef6e45)**  
  
Date(Commit): Tue Dec 05 09:51:15 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65", "Merge-Rejected-63", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/808104](https://chromium-review.googlesource.com/808104)  
Regress: [mjsunit/regress/regress-crbug-791245-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-791245-1.js), [mjsunit/regress/regress-crbug-791245-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-791245-2.js)  
```javascript
const s = new Map;

function foo(s) {
  const i = s[Symbol.iterator]();
  i.next();
  return i;
}

console.log(foo(s));
console.log(foo(s));
%OptimizeFunctionOnNextCall(foo);
console.log(foo(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ef6e45^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=3ef6e45)  
[test/mjsunit/regress/regress-crbug-791245-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791245-1.js?cl=3ef6e45)  
[test/mjsunit/regress/regress-crbug-791245-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791245-2.js?cl=3ef6e45)  
  

---   

## **regress-791245.js (chromium issue)**  
   
**[Issue 791245:
 Security: V8: JIT: Simplified-lowererer IrOpcode::kStoreField, IrOpcode::kStoreElement optimization bug](https://crbug.com/791245)**  
**[Commit: [turbofan] Properly type the OrderedHashTableHealIndex builtin result.](https://chromium.googlesource.com/v8/v8/+/3ef6e45)**  
  
Date(Commit): Tue Dec 05 09:51:15 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65", "Merge-Rejected-63", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/808104](https://chromium-review.googlesource.com/808104)  
Regress: [mjsunit/compiler/regress-791245.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-791245.js)  
```javascript
var a, b;  // Global variables that will end up with number map.

for (var i = 0; i < 100000; i++) {
  b = 1;
  a = i + -0;  // -0 is a number, so this will make "a" a heap object.
  b = a;
}

assertTrue(a === b);
gc();
assertTrue(a === b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ef6e45^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=3ef6e45)  
[test/mjsunit/regress/regress-crbug-791245-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791245-1.js?cl=3ef6e45)  
[test/mjsunit/regress/regress-crbug-791245-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791245-2.js?cl=3ef6e45)  
  

---   

## **compiler-regress-787301.js (chromium issue)**  
   
**[Issue 787301:
 Stack-overflow in v8::internal::TranslatedState::MaterializeAt](https://crbug.com/787301)**  
**[Commit: [deoptimizer] Fix materialization of iterators.](https://chromium.googlesource.com/v8/v8/+/9a6f442)**  
  
Date(Commit): Mon Dec 04 17:57:45 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-65", "Merge-Rejected-63", "Test-Predator-Auto-Owner", "merge-merged-6.4", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/806223](https://chromium-review.googlesource.com/806223)  
Regress: [mjsunit/compiler-regress-787301.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler-regress-787301.js)  
```javascript
function opt(b) {
    let iterator = new Set().values();
    iterator.x = 0;

    let arr = [iterator, iterator];
    if (b)
        return arr.slice();
}

opt(false);
opt(false);
%OptimizeFunctionOnNextCall(opt);

let res = opt(true);
let a = res[0];
let b = res[1];

assertTrue(a === b);
a.x = 7;
assertEquals(7, b.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a6f442^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9a6f442)  
[test/mjsunit/compiler-regress-787301.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler-regress-787301.js?cl=9a6f442)  
  

---   

## **regress-crbug-789764.js (chromium issue)**  
   
**[Issue 789764:
 Crash in v8::internal::Script::FindSharedFunctionInfo](https://crbug.com/789764)**  
**[Commit: [parser] Fix func numbering inside for in.](https://chromium.googlesource.com/v8/v8/+/0394b71)**  
  
Date(Commit): Fri Dec 01 14:12:12 2017  
Components/Type: Blink>JavaScript>Interpreter/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "Release-0-M65"]  
Code Review: [https://chromium-review.googlesource.com/803217](https://chromium-review.googlesource.com/803217)  
Regress: [mjsunit/regress/regress-crbug-789764.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-789764.js)  
```javascript
_v3 = ({ _v7 = (function outer() {
        for ([...[]][function inner() {}] in []) {
        }
      })} = {}) => {
};
_v3();

a = (b = !function outer() { for (function inner() {}.foo in []) {} }) => {};
a();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0394b71^!)  
[src/ast/ast-traversal-visitor.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-traversal-visitor.h?cl=0394b71)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0394b71)  
[test/mjsunit/regress/regress-crbug-789764.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-789764.js?cl=0394b71)  
  

---   

## **regress-789952.js (chromium issue)**  
   
**[Issue 789952:
 Security: NCSC Vulnerability Report - Google Chrome - V8 JavaScript Engine](https://crbug.com/789952)**  
**[Commit: [wasm] Gracefully handle malformed custom sections in WebAssembly.Module.customSections().](https://chromium.googlesource.com/v8/v8/+/163c1c8)**  
  
Date(Commit): Thu Nov 30 18:25:09 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "reward-2000", "Security_Impact-Stable", "Security_Severity-Medium", "reward-decline", "allpublic", "M-65", "Merge-Rejected-63", "merge-merged-6.4", "Release-0-M64", "CVE-2018-6036", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/800941](https://chromium-review.googlesource.com/800941)  
Regress: [mjsunit/regress/wasm/regress-789952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-789952.js)  
```javascript
let module_size = 19;
let string_len = 0x00fffff0 - module_size;

print("Allocating backing store: " + (string_len + module_size));
let backing = new ArrayBuffer(string_len + module_size);

print("Allocating typed array buffer");
let buffer = new Uint8Array(backing);

print("Filling...");
buffer.fill(0x41);

print("Setting up array buffer");
buffer.set([0x00, 0x61, 0x73, 0x6D], 0);
buffer.set([0x01, 0x00, 0x00, 0x00], 4);
buffer.set([0], 8);
buffer.set([0x80, 0x80, 0x80, 0x80, 0x00],  9);
let x = string_len + 1;
let b1 = ((x >> 0) & 0x7F) | 0x80;
let b2 = ((x >> 7) & 0x7F) | 0x80;
let b3 = ((x >> 14) & 0x7F) | 0x80;
let b4 = ((x >> 21) & 0x7F);
 buffer.set([b1, b2, b3, b4], 14);

print("Parsing module...");
let m = new WebAssembly.Module(buffer);

print("Triggering!");
let c = WebAssembly.Module.customSections(m, "A".repeat(string_len + 1));
assertEquals(0, c.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/163c1c8^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=163c1c8)  
[test/mjsunit/regress/wasm/regress-789952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-789952.js?cl=163c1c8)  
  

---   

## **regress-7135.js (v8 issue)**  
   
**[Issue 7135:
 Ignition records incorrect feedback for unary ops](https://crbug.com/v8/7135)**  
**[Commit: [interpreter] Fix feedback collection for negation.](https://chromium.googlesource.com/v8/v8/+/64030c6)**  
  
Date(Commit): Tue Nov 28 13:46:23 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/793039](https://chromium-review.googlesource.com/793039)  
Regress: [mjsunit/regress/regress-7135.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7135.js)  
```javascript
function foo() { return -"0" }
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
assertOptimized(foo);

function bar() { return -"1" }
bar();
%OptimizeFunctionOnNextCall(bar);
bar();
assertOptimized(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64030c6^!)  
[src/interpreter/interpreter-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter-generator.cc?cl=64030c6)  
[test/mjsunit/regress/regress-7135.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7135.js?cl=64030c6)  
  

---   

## **regress-7115.js (v8 issue)**  
   
**[Issue 7115:
 accessing length of a instance of a class extending typed array is not optimized](https://crbug.com/v8/7115)**  
**[Commit: [runtime] Properly deal with prototype setup mode during class literal instantiation.](https://chromium.googlesource.com/v8/v8/+/888acb2)**  
  
Date(Commit): Tue Nov 28 09:11:59 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/790992](https://chromium-review.googlesource.com/790992)  
Regress: [mjsunit/regress/regress-7115.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7115.js)  
```javascript
function TestBuiltinSubclassing(Builtin) {
  assertTrue(%HasFastProperties(Builtin));
  assertTrue(%HasFastProperties(Builtin.prototype));
  assertTrue(%HasFastProperties(Builtin.prototype.__proto__));

  class SubClass extends Builtin {}

  assertTrue(%HasFastProperties(Builtin));
  assertTrue(%HasFastProperties(Builtin.prototype));
  assertTrue(%HasFastProperties(Builtin.prototype.__proto__));
}

let TypedArray = Uint8Array.__proto__;

TestBuiltinSubclassing(RegExp);
TestBuiltinSubclassing(Promise);
TestBuiltinSubclassing(Array);
TestBuiltinSubclassing(TypedArray);
TestBuiltinSubclassing(Uint8Array);
TestBuiltinSubclassing(Int8Array);
TestBuiltinSubclassing(Uint16Array);
TestBuiltinSubclassing(Int16Array);
TestBuiltinSubclassing(Uint32Array);
TestBuiltinSubclassing(Int32Array);
TestBuiltinSubclassing(Float32Array);
TestBuiltinSubclassing(Float64Array);
TestBuiltinSubclassing(Uint8ClampedArray);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/888acb2^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=888acb2)  
[src/js/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/js/typedarray.js?cl=888acb2)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=888acb2)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=888acb2)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=888acb2)  
...  
  

---   

## **regress-788539.js (chromium issue)**  
   
**[Issue 788539:
 CHECK failure: frame_state->opcode() == IrOpcode::kFrameState || (node->opcode() == IrOpcode::k](https://crbug.com/788539)**  
**[Commit: [turbofan] fix dead code elimination: propagate DeadValue along FrameState inputs](https://chromium.googlesource.com/v8/v8/+/904c3a1)**  
  
Date(Commit): Tue Nov 28 09:09:09 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/792050](https://chromium-review.googlesource.com/792050)  
Regress: [mjsunit/compiler/regress-788539.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-788539.js)  
```javascript
function f1() {
  return this;
}

function f2(x, value, type) {
  x instanceof type
}

function f3(a) {
  a.x = 0;
  if (a.x === 0) {
    a[1] = 0.1;
  }
  class B {
  }
  class C extends B {
    bar() {
      return super.foo()
    }
  }
  B.prototype.foo = f1;
  f2(new C().bar.call(), Object(), String);
}

f3(new Array(1));
f3(new Array(1));
%OptimizeFunctionOnNextCall(f3);
f3(new Array(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/904c3a1^!)  
[src/compiler/dead-code-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.cc?cl=904c3a1)  
[test/mjsunit/compiler/regress-788539.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-788539.js?cl=904c3a1)  
  

---   

## **regress-crbug-786020.js (chromium issue)**  
   
**[Issue 786020:
 CHECK failure: !descriptors->GetKey(i)->IsInterestingSymbol() in objects-debug.cc](https://crbug.com/786020)**  
**[Commit: [objects] Fix flag in {Map::AddMissingTransitions}.](https://chromium.googlesource.com/v8/v8/+/4ad9430)**  
  
Date(Commit): Mon Nov 27 12:49:01 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/789839](https://chromium-review.googlesource.com/789839)  
Regress: [mjsunit/regress/regress-crbug-786020.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-786020.js)  
```javascript
%SetAllocationTimeout(1000, 90);
(new constructor)[0x40000000] = null;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ad9430^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4ad9430)  
[test/mjsunit/regress/regress-crbug-786020.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-786020.js?cl=4ad9430)  
  

---   

## **regress-7121.js (v8 issue)**  
   
**[Issue 7121:
 DCHECK failure in Turbofan typer](https://crbug.com/v8/7121)**  
**[Commit: [compiler] Make typer deal with conversions that return empty type.](https://chromium.googlesource.com/v8/v8/+/74184d5)**  
  
Date(Commit): Thu Nov 23 11:37:09 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/785130](https://chromium-review.googlesource.com/785130)  
Regress: [mjsunit/compiler/regress-7121.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-7121.js)  
```javascript
function foo() { %_ToLength(42n) }
assertThrows(foo, TypeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/74184d5^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=74184d5)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=74184d5)  
[test/mjsunit/compiler/regress-7121.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-7121.js?cl=74184d5)  
  

---   

## **regress-crbug-783132.js (chromium issue)**  
   
**[Issue 783132:
 CHECK failure: is_transitionable_fast_elements_kind implies !Map::IsInplaceGeneralizableField(d](https://crbug.com/783132)**  
**[Commit: [runtime] Ensure elements transitions don't interfere with field type tracking.](https://chromium.googlesource.com/v8/v8/+/00a781d)**  
  
Date(Commit): Wed Nov 22 16:51:47 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.3"]  
Code Review: [https://chromium-review.googlesource.com/785190](https://chromium-review.googlesource.com/785190)  
Regress: [mjsunit/regress/regress-crbug-783132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-783132.js)  
```javascript
function f(o, v) {
  try {
    f(o, v + 1);
  } catch (e) {
  }
  o[v] = 43.35 + v * 5.3;
}

f(Array.prototype, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00a781d^!)  
[src/api-natives.cc](https://cs.chromium.org/chromium/src/v8/src/api-natives.cc?cl=00a781d)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=00a781d)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=00a781d)  
[src/heap/setup-heap-internal.cc](https://cs.chromium.org/chromium/src/v8/src/heap/setup-heap-internal.cc?cl=00a781d)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=00a781d)  
...  
  

---   

## **regress-784080.js (chromium issue)**  
   
**[Issue 784080:
 Crash in v8::internal::Simulator::DecodeType3](https://crbug.com/784080)**  
**[Commit: Fix hole handling in fast arguments slice](https://chromium.googlesource.com/v8/v8/+/4d70aa0)**  
  
Date(Commit): Wed Nov 22 12:32:37 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "reward-1500", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "reward-inprocess", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/785210](https://chromium-review.googlesource.com/785210)  
Regress: [mjsunit/regress/regress-784080.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-784080.js)  
```javascript
(function() {
  function f(a, b, a) {
    return Array.prototype.slice.call(arguments);
  }
  let result = f(456, 789, 111112);
  assertEquals(result[0], 456);
  assertEquals(result[1], 789);
  assertEquals(result[2], 111112);
  assertEquals(result.length, 3);
})();

(function() {
  function f(a, b, a) {
    return Array.prototype.slice.call(arguments);
  }
  let result = f(456, 789, 111112, 543, 654);
  assertEquals(result[0], 456);
  assertEquals(result[1], 789);
  assertEquals(result[2], 111112);
  assertEquals(result[3], 543);
  assertEquals(result[4], 654);
  assertEquals(result.length, 5);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4d70aa0^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=4d70aa0)  
[test/mjsunit/regress/regress-784080.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-784080.js?cl=4d70aa0)  
  

---   

## **regress-786573.js (chromium issue)**  
   
**[Issue 786573:
 Security: V8: Integer overflow in Runtime_RegExpReplace](https://crbug.com/786573)**  
**[Commit: [regexp] Avoid integer overflow in callable @@replace](https://chromium.googlesource.com/v8/v8/+/71b9018)**  
  
Date(Commit): Tue Nov 21 12:09:13 2017  
Components/Type: Blink>JavaScript>Regexp/Bug-Security  
Labels: ["Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/779001](https://chromium-review.googlesource.com/779001)  
Regress: [mjsunit/regress/regress-786573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-786573.js)  
```javascript
let cnt = 0;
let reg = /./g;
reg.exec = () => {
  // Note: it's still possible to trigger OOM by passing huge values here, since
  // the spec requires building a list of all captures in
  // https://tc39.github.io/ecma262/#sec-regexp.prototype-@@replace
  if (cnt++ == 0) return {length: 2 ** 16};
  cnt = 0;
  return null;
};

assertThrows(() => ''.replace(reg, () => {}), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/71b9018^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=71b9018)  
[test/mjsunit/regress/regress-707187.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-707187.js?cl=71b9018)  
[test/mjsunit/regress/regress-786573.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-786573.js?cl=71b9018)  
  

---   

## **regress-785804.js (chromium issue)**  
   
**[Issue 785804:
 DCHECK failure in !IsSmi() == Internals::HasHeapObjectTag(this) in objects.h](https://crbug.com/785804)**  
**[Commit: Fix bug in length handling of Array.prototype.slice fast-path](https://chromium.googlesource.com/v8/v8/+/f0ceb9f)**  
  
Date(Commit): Mon Nov 20 11:53:13 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/776696](https://chromium-review.googlesource.com/776696)  
Regress: [mjsunit/regress/regress-785804.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-785804.js)  
```javascript
let __v_25059 = {
  valueOf: function () {
    let __v_25062 = __v_25055.length;
    __v_25055.length = 1;
    return __v_25062;
  }
};
let __v_25060 = [];
for (let __v_25063 = 0; __v_25063 < 1500; __v_25063++) {
  __v_25060.push("" + 0.1);
}
for (let __v_25064 = 0; __v_25064 < 75; __v_25064++) {
  __v_25055 = __v_25060.slice();
  __v_25056 = __v_25055.slice(0, __v_25059);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0ceb9f^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=f0ceb9f)  
[test/mjsunit/regress/regress-785804.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-785804.js?cl=f0ceb9f)  
  

---   

## **regress-crbug-783902.js (chromium issue)**  
   
**[Issue 783902:
 CHECK failure: method->map()->instance_descriptors()->GetKey(kHomeObjectPropertyIndex) == isola](https://crbug.com/783902)**  
**[Commit: Reland^2 "[runtime] Slightly optimize creation of class literals."](https://chromium.googlesource.com/v8/v8/+/cc9e77a)**  
  
Date(Commit): Fri Nov 17 18:15:34 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/764067](https://chromium-review.googlesource.com/764067)  
Regress: [mjsunit/regress/regress-crbug-783902.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-783902.js)  
```javascript
class A {}

class B extends A {
  *gf() {
    yield super.f();
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc9e77a^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=cc9e77a)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=cc9e77a)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=cc9e77a)  
[src/compiler/code-assembler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-assembler.h?cl=cc9e77a)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=cc9e77a)  
...  
  

---   

## **regress-784050.js (chromium issue)**  
   
**[Issue 784050:
 DCHECK failure in dst == src in liftoff-assembler.cc](https://crbug.com/784050)**  
**[Commit: [Liftoff] Don't force unrelated stack slots into registers](https://chromium.googlesource.com/v8/v8/+/1cec66d)**  
  
Date(Commit): Thu Nov 16 17:34:17 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "M-64", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/766353](https://chromium-review.googlesource.com/766353)  
Regress: [mjsunit/regress/wasm/regress-784050.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-784050.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_v_v)
    .addBodyWithEnd([
      kExprI32Const,    0x0,   // const 0
      kExprI32Const,    0x0,   // const 0
      kExprBrIf,        0x00,  // br depth=0
      kExprLoop,        0x7f,  // loop i32
      kExprBlock,       0x7f,  // block i32
      kExprI32Const,    0x0,   // const 0
      kExprBr,          0x00,  // br depth=0
      kExprEnd,                // end
      kExprBr,          0x00,  // br depth=0
      kExprEnd,                // end
      kExprUnreachable,        // unreachable
      kExprEnd,                // end
    ])
    .exportFunc();
builder.instantiate();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1cec66d^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=1cec66d)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=1cec66d)  
[test/mjsunit/regress/wasm/regress-784050.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-784050.js?cl=1cec66d)  
  

---   

## **regress-784863.js (chromium issue)**  
   
**[Issue 784863:
 CHECK failure: nof_elements <= array_length in objects-debug.cc](https://crbug.com/784863)**  
**[Commit: Fix hole escape in dictionary mode Array.prototype.slice()](https://chromium.googlesource.com/v8/v8/+/4002bf9)**  
  
Date(Commit): Thu Nov 16 12:17:58 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Hotlist-Torque"]  
Code Review: [https://chromium-review.googlesource.com/774263](https://chromium-review.googlesource.com/774263)  
Regress: [mjsunit/regress/regress-784863.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-784863.js)  
```javascript
var __v_18522 = [ 4.2, true, false];
Object.defineProperty(__v_18522, 2, {
  get: function () {
    return false;
  },
});
__v_18522.shift();
__v_18522.slice();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4002bf9^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=4002bf9)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4002bf9)  
[test/mjsunit/regress/regress-784863.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-784863.js?cl=4002bf9)  
  

---   

## **regress-784990.js (chromium issue)**  
   
**[Issue 784990:
 DCHECK failure in nod == removed_holes_index in objects.cc](https://crbug.com/784990)**  
**[Commit: [collections] Handle holes in collection constructor fast paths](https://chromium.googlesource.com/v8/v8/+/007203a)**  
  
Date(Commit): Thu Nov 16 06:59:25 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/771387](https://chromium-review.googlesource.com/771387)  
Regress: [mjsunit/regress/regress-784990.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-784990.js)  
```javascript
const key1 = {};
const key2 = {};

const set = new Set([, 1]);
assertEquals(set.has(undefined), true);
assertEquals(set.has(1), true);

const doubleSet = new Set([,1.234]);
assertEquals(doubleSet.has(undefined), true);
assertEquals(doubleSet.has(1.234), true);

const map = new Map([[, key1], [key2, ]]);
assertEquals(map.get(undefined), key1);
assertEquals(map.get(key2), undefined);

const doublesMap = new Map([[, 1.234]]);
assertEquals(doublesMap.get(undefined), 1.234);

const weakmap = new WeakMap([[key1, ]]);
assertEquals(weakmap.get(key1), undefined);

assertThrows(() => new WeakSet([, {}]));
assertThrows(() => new WeakSet([, 1.234]));
assertThrows(() => new Map([, [, key1]]));
assertThrows(() => new WeakMap([[, key1]]));
assertThrows(() => new WeakMap([, [, key1]]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/007203a^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=007203a)  
[test/mjsunit/regress/regress-784990.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-784990.js?cl=007203a)  
  

---   

## **regress-crbug-784835.js (chromium issue)**  
   
**[Issue 784835:
 page content not display](https://crbug.com/784835)**  
**[Commit: [ic] Properly handle negative indices.](https://chromium.googlesource.com/v8/v8/+/3dddc2b)**  
  
Date(Commit): Thu Nov 16 06:56:25 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Arch-x86_64", "ReleaseBlock-Stable", "hasbisect-per-revision", "Via-Wizard-Content", "M-64", "Triaged-ET", "Needs-Triage-M64"]  
Code Review: [https://chromium-review.googlesource.com/774278](https://chromium-review.googlesource.com/774278)  
Regress: [mjsunit/regress/regress-crbug-784835.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-784835.js)  
```javascript
function foo(o, k) { return o[k]; }

var a = [1,2];
a["-1"] = 42;

assertEquals(1, foo(a, 0));
assertEquals(2, foo(a, 1));
assertEquals(undefined, foo(a, 3));
assertEquals(42, foo(a, -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3dddc2b^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=3dddc2b)  
[test/mjsunit/regress/regress-crbug-784835.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-784835.js?cl=3dddc2b)  
  

---   

## **regress-784862.js (chromium issue)**  
   
**[Issue 784862:
 CHECK failure: size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/784862)**  
**[Commit: [collections] Allocate large collections in large object space](https://chromium.googlesource.com/v8/v8/+/271ffdb)**  
  
Date(Commit): Wed Nov 15 12:08:35 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/771150](https://chromium-review.googlesource.com/771150)  
Regress: [mjsunit/regress/regress-784862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-784862.js)  
```javascript
const array = new Array();
array[0x80000] = 1;
array.unshift({});
assertThrows(() => new WeakMap(array));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/271ffdb^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=271ffdb)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=271ffdb)  
[src/debug/debug-evaluate.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-evaluate.cc?cl=271ffdb)  
[test/mjsunit/regress/regress-784862.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-784862.js?cl=271ffdb)  
  

---   

## **regress-780658.js (chromium issue)**  
   
**[No Permission](https://crbug.com/780658)**  
**[Commit: [turbofan] Escape analysis no longer introduces Dead nodes in unreachable code.](https://chromium.googlesource.com/v8/v8/+/9e92289)**  
  
Date(Commit): Wed Nov 15 11:16:01 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/769507](https://chromium-review.googlesource.com/769507)  
Regress: [mjsunit/compiler/regress-780658.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-780658.js)  
```javascript
function get1(l, b) {
    return l[1];
}

function with_double(x) {
    var o = {a: [x,x,x]};
    o.a.some_property = 1;
    return get1(o.a);
}

function with_tagged(x) {
    var l = [{}, x,x];
    return get1(l);
}

with_double(.5);
with_tagged({});
with_double(.6);
with_tagged(null);
with_double(.5);
with_tagged({});
%OptimizeFunctionOnNextCall(with_double);
with_double(.7);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e92289^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=9e92289)  
[src/compiler/escape-analysis.h](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.h?cl=9e92289)  
[test/mjsunit/compiler/regress-780658.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-780658.js?cl=9e92289)  
  

---   

## **regress-crbug-779457.js (chromium issue)**  
   
**[Issue 779457:
 DCHECK failure in outer_scope_ == scope->outer_scope() in bytecode-generator.cc](https://crbug.com/779457)**  
**[Commit: [parser] RewritableExpressions should keep track of their Scope directly](https://chromium.googlesource.com/v8/v8/+/082009f)**  
  
Date(Commit): Tue Nov 14 20:30:14 2017  
Components/Type: Blink>JavaScript>Parser/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Release-0-M64"]  
Code Review: [https://chromium-review.googlesource.com/767666](https://chromium-review.googlesource.com/767666)  
Regress: [mjsunit/regress/regress-crbug-779457.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-779457.js)  
```javascript
(function testEager() {
  (function({name = [foo] = eval("[]")}) {})({});
  (function([name = [foo] = eval("[]")]) {})([]);
})();

(function testLazy() {
  function f({name = [foo] = eval("[]")}) {}
  function g([name = [foo] = eval("[]")]) {}
  f({});
  g([]);
})();

(function testEagerArrow() {
  (({name = [foo] = eval("[]")}) => {})({});
  (([name = [foo] = eval("[]")]) => {})([]);
})();

(function testLazyArrow() {
  var f = ({name = [foo] = eval("[]")}) => {};
  var g = ([name = [foo] = eval("[]")]) => {};
  f({});
  g([]);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/082009f^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=082009f)  
[src/parsing/expression-scope-reparenter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-scope-reparenter.cc?cl=082009f)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=082009f)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=082009f)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=082009f)  
...  
  

---   

## **regress-crbug-781583.js (chromium issue)**  
   
**[Issue 781583:
 Stack-overflow in v8::internal::KeyAccumulator::CollectOwnElementIndices](https://crbug.com/781583)**  
**[Commit: [builtins] Add stack check during generator resumption.](https://chromium.googlesource.com/v8/v8/+/2bc09c9)**  
  
Date(Commit): Mon Nov 13 14:52:10 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/765953](https://chromium-review.googlesource.com/765953)  
Regress: [mjsunit/regress/regress-crbug-781583.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781583.js)  
```javascript
function* generator(a) {
  a.pop().next();
}

function prepareGenerators(n) {
  var a = [{ next: () => 0 }];
  for (var i = 0; i < n; ++i) {
    a.push(generator(a));
  }
  return a;
}

var gens1 = prepareGenerators(10);
assertDoesNotThrow(() => gens1.pop().next());

%OptimizeFunctionOnNextCall(generator);

var gens2 = prepareGenerators(200000);
assertThrows(() => gens2.pop().next(), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2bc09c9^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=2bc09c9)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=2bc09c9)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=2bc09c9)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=2bc09c9)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=2bc09c9)  
...  
  

---   

## **regress-783119.js (chromium issue)**  
   
**[Issue 783119:
 CHECK failure: nof_elements <= array_length in objects-debug.cc](https://crbug.com/783119)**  
**[Commit: Fix index bug in splicing dictionary element arrays](https://chromium.googlesource.com/v8/v8/+/cecbe26)**  
  
Date(Commit): Mon Nov 13 11:21:40 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/763460](https://chromium-review.googlesource.com/763460)  
Regress: [mjsunit/regress/regress-783119.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-783119.js)  
```javascript
let a = [,,,,,,,,,,,,,,,,,,,,,,,11,12,13,14,15,16,17,18,19];
%NormalizeElements(a);
let b = a.slice(19);
assertEquals(11, b[4]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cecbe26^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=cecbe26)  
[test/mjsunit/regress/regress-783119.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-783119.js?cl=cecbe26)  
  

---   

## **regress-781218.js (chromium issue)**  
   
**[Issue 781218:
 Chrome 62 Race Condition Crashes w/ Angular+PrimeNG](https://crbug.com/781218)**  
**[Commit: Disallow empty PropertyArray as properties backing store](https://chromium.googlesource.com/v8/v8/+/eab2f2e)**  
  
Date(Commit): Mon Nov 13 10:56:53 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Hotlist-Merge-Review", "ReleaseBlock-Stable", "hasbisect-per-revision", "Via-Wizard-Javascript", "M-62", "M-63", "M-64", "Needs-Triage-M62", "TE-Verified-M63", "merge-merged-6.3", "merge-merged-63", "TE-Verified-63.0.3239.59"]  
Code Review: [https://chromium-review.googlesource.com/763230](https://chromium-review.googlesource.com/763230)  
Regress: [mjsunit/regress/regress-781218.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-781218.js)  
```javascript
var m = new Map();

function C() { }

%CompleteInobjectSlackTracking(new C());

function f(o) {
  o.x = true;
}

f(new C());
f(new C());


var o = new C();
%HeapObjectVerify(o);

m.set({}, 3);
m.set(o, 1);

o.x = true;
%HeapObjectVerify(o);
delete o.x;
%HeapObjectVerify(o);


%OptimizeFunctionOnNextCall(f);
f(o);

%HeapObjectVerify(o);
assertEquals(1, m.get(o));

for (let i = 0; i < 1000; i++) {
  let object = {};
  m.set(object, object);
  assertEquals(1, m.get(o));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eab2f2e^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=eab2f2e)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=eab2f2e)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=eab2f2e)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=eab2f2e)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=eab2f2e)  
...  
  

---   

## **regress-778668.js (chromium issue)**  
   
**[Issue 778668:
 Crash in v8::internal::Invoke](https://crbug.com/778668)**  
**[Commit: Fix splice bug in handling of negative arguments length](https://chromium.googlesource.com/v8/v8/+/d5885ca)**  
  
Date(Commit): Fri Nov 10 15:23:28 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/753324](https://chromium-review.googlesource.com/753324)  
Regress: [mjsunit/regress/regress-778668.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-778668.js)  
```javascript
(function () {
  function f() {
    arguments.length = -5;
    Array.prototype.slice.call(arguments);
  }
  f('a')
})();

(function () {
  function f() {
    arguments.length = 2.3;
    Array.prototype.slice.call(arguments);
  }
  f('a')
})();

(function () {
  function f( __v_59960) {
    arguments.length = -5;
    Array.prototype.slice.call(arguments);
  }
  f('a')
})();

(function () {
  function f( __v_59960) {
    arguments.length = 2.3;
    Array.prototype.slice.call(arguments);
  }
  f('a')
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5885ca^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=d5885ca)  
[test/mjsunit/regress/regress-778668.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-778668.js?cl=d5885ca)  
  

---   

## **regress-782280.js (chromium issue)**  
   
**[Issue 782280:
 CHECK failure: interpreter != liftoff (ec35c0be vs ebeNUMBER); WasmCodeFuzzerHash=ee207cda in w](https://crbug.com/782280)**  
**[Commit: [Liftoff] Implement parallel register moves](https://chromium.googlesource.com/v8/v8/+/6c61328)**  
  
Date(Commit): Fri Nov 10 09:47:32 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Memory-LeakSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "ClusterFuzz-Wrong", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/758772](https://chromium-review.googlesource.com/758772)  
Regress: [mjsunit/regress/wasm/regress-782280.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-782280.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_iii)
    .addBodyWithEnd([
      kExprI32Const, 0,          // 0
      kExprI32Const, 0,          // 0, 0
      kExprI32Add,               // 0 + 0 -> 0
      kExprI32Const, 0,          // 0, 0
      kExprI32Const, 0,          // 0, 0, 0
      kExprI32Add,               // 0, 0 + 0 -> 0
      kExprDrop,                 // 0
      kExprDrop,                 // -
      kExprI32Const, 0,          // 0
      kExprI32Const, 0,          // 0, 0
      kExprI32Add,               // 0 + 0 -> 0
      kExprI32Const, 0,          // 0, 0
      kExprI32Const, 1,          // 0, 0, 1
      kExprI32Add,               // 0, 0 + 1 -> 1
      kExprBlock,    kWasmStmt,  // 0, 1
      kExprBr,       0,          // 0, 1
      kExprEnd,                  // 0, 1
      kExprI32Add,               // 0 + 1 -> 1
      kExprEnd
    ])
    .exportFunc();
var module = builder.instantiate();
assertEquals(1, module.exports.test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c61328^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=6c61328)  
[test/mjsunit/regress/wasm/regress-782280.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-782280.js?cl=6c61328)  
  

---   

## **regress-782754.js (chromium issue)**  
   
**[Issue 782754:
 DCHECK failure in this->IsInhabited() in types.cc](https://crbug.com/782754)**  
**[Commit: [compiler] Really do not call Min/Max on empty type.](https://chromium.googlesource.com/v8/v8/+/23496a2)**  
  
Date(Commit): Fri Nov 10 08:37:06 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "ClusterFuzz-Top-Crash"]  
Code Review: [https://chromium-review.googlesource.com/758841](https://chromium-review.googlesource.com/758841)  
Regress: [mjsunit/regress/regress-782754.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-782754.js)  
```javascript
let a = [1,2];
function f(skip) { g(undefined, skip) }
function g(x, skip) {
  if (skip) return;
  return a[x+1];
}
g(0, false);
g(0, false);
f(true);
f(true);
%OptimizeFunctionOnNextCall(f);
f(false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/23496a2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=23496a2)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=23496a2)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=23496a2)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=23496a2)  
[test/mjsunit/regress/regress-782754.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-782754.js?cl=23496a2)  
  

---   

## **regress-783051.js (chromium issue)**  
   
**[Issue 783051:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/783051)**  
**[Commit: [compiler] Fix OperationTyper::NumberAbs.](https://chromium.googlesource.com/v8/v8/+/22d4e6e)**  
  
Date(Commit): Thu Nov 09 12:18:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/758765](https://chromium-review.googlesource.com/758765)  
Regress: [mjsunit/regress/regress-783051.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-783051.js)  
```javascript
function f() { return Math.abs([][0]); }
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22d4e6e^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=22d4e6e)  
[test/mjsunit/regress/regress-783051.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-783051.js?cl=22d4e6e)  
  

---   

## **regress-776309.js (chromium issue)**  
   
**[Issue 776309:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/776309)**  
**[Commit: [deoptimizer] Make sure property arrays don't contain mutable heap numbers.](https://chromium.googlesource.com/v8/v8/+/9eb92da)**  
  
Date(Commit): Thu Nov 09 12:02:47 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.3", "Release-0-M63"]  
Code Review: [https://chromium-review.googlesource.com/759781](https://chromium-review.googlesource.com/759781)  
Regress: [mjsunit/regress/regress-776309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-776309.js)  
```javascript
function C() { }

function f(b) {
  var o = new C();
  // Create out-of-object properties only on one branch so that escape
  // analysis does not analyze the property array away.
  if (b) o.t = 1.1;
  %_DeoptimizeNow();
  return o.t;
}

for (var i = 0; i < 1000; i++) new C();

f(true);
f(true);
f(false);

%OptimizeFunctionOnNextCall(f);

assertEquals(1.1, f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9eb92da^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9eb92da)  
[test/mjsunit/regress/regress-776309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-776309.js?cl=9eb92da)  
  

---   

## **regress-7049.js (v8 issue)**  
   
**[Issue 7049:
 mjsunit/wasm/interpreter-mixed crashing in arm64 GC stress](https://crbug.com/v8/7049)**  
**[Commit: [wasm] Mark C_WASM_ENTRY as no tagged_params](https://chromium.googlesource.com/v8/v8/+/3c483de)**  
  
Date(Commit): Wed Nov 08 12:55:17 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/758245](https://chromium-review.googlesource.com/758245)  
Regress: [mjsunit/regress/wasm/regress-7049.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7049.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


let builder1 = new WasmModuleBuilder();

function call_gc() {
  print('Triggering GC.');
  gc();
  print('Survived GC.');
}
let func1_sig = makeSig(new Array(8).fill(kWasmI32), [kWasmI32]);
let imp = builder1.addImport('q', 'gc', kSig_v_v);
let func1 = builder1.addFunction('func1', func1_sig)
                .addBody([
                  kExprGetLocal, 0,  // -
                  kExprCallFunction, imp
                ])
                .exportFunc();
let instance1 = builder1.instantiate({q: {gc: call_gc}});

let builder2 = new WasmModuleBuilder();

let func1_imp = builder2.addImport('q', 'func1', func1_sig);
let func2 = builder2.addFunction('func2', kSig_i_i)
                .addBody([
                  kExprGetLocal, 0,  // 1
                  kExprGetLocal, 0,  // 2
                  kExprGetLocal, 0,  // 3
                  kExprGetLocal, 0,  // 4
                  kExprGetLocal, 0,  // 5
                  kExprGetLocal, 0,  // 6
                  kExprGetLocal, 0,  // 7
                  kExprGetLocal, 0,  // 8
                  kExprCallFunction, func1_imp
                ])
                .exportFunc();

let instance2 = builder2.instantiate({q: {func1: instance1.exports.func1}});

%RedirectToWasmInterpreter(
        instance2, parseInt(instance2.exports.func2.name));

instance2.exports.func2(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c483de^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=3c483de)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=3c483de)  
[test/mjsunit/regress/wasm/regress-7049.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7049.js?cl=3c483de)  
  

---   

## **regress-782145.js (chromium issue)**  
   
**[Issue 782145:
 Security:V8:Type Confusion Leads To OOB Read Write](https://crbug.com/782145)**  
**[Commit: [string] Fix regexp fast path in MaybeCallFunctionAtSymbol](https://chromium.googlesource.com/v8/v8/+/55a9807)**  
  
Date(Commit): Wed Nov 08 09:49:33 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "reward-3000", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "reward-inprocess", "NodeJS-Backport-Done", "M-65", "merge-merged-6.2", "merge-merged-6.3", "CVE-2017-15428", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/758257](https://chromium-review.googlesource.com/758257)  
Regress: [mjsunit/regress/regress-782145.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-782145.js)  
```javascript
function newFastRegExp() { return new RegExp('.'); }
function toSlowRegExp(re) { re.exec = 42; }

let re = newFastRegExp();
const evil_nonstring = { [Symbol.toPrimitive]: () => toSlowRegExp(re) };
const empty_string = "";

String.prototype.replace.call(evil_nonstring, re, empty_string);

re = newFastRegExp();
String.prototype.match.call(evil_nonstring, re, empty_string);

re = newFastRegExp();
String.prototype.search.call(evil_nonstring, re, empty_string);

re = newFastRegExp();
String.prototype.split.call(evil_nonstring, re, empty_string);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55a9807^!)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=55a9807)  
[src/builtins/builtins-string-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.h?cl=55a9807)  
[test/mjsunit/regress/regress-782145.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-782145.js?cl=55a9807)  
  

---   

## **regress-7035.js (v8 issue)**  
   
**[Issue 7035:
 [Liftoff] Bug in state join on break](https://crbug.com/v8/7035)**  
**[Commit: [Liftoff] Fix register reuse in merge init](https://chromium.googlesource.com/v8/v8/+/c7ad565)**  
  
Date(Commit): Mon Nov 06 17:35:07 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/754816](https://chromium-review.googlesource.com/754816)  
Regress: [mjsunit/regress/wasm/regress-7035.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7035.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_iii)
    .addBodyWithEnd([
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Add,          // i32.add -> 0
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Add,          // i32.add -> 0
      kExprI32Add,          // i32.add -> 0
      kExprI32Const, 0x01,  // i32.const 1
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Add,          // i32.add -> 1
      kExprBlock,    0x7f,  // @39 i32
      kExprI32Const, 0x00,  // i32.const 0
      kExprBr,       0x00,  // depth=0
      kExprEnd,             // @90
      kExprI32Add,          // i32.add -> 1
      kExprI32Add,          // i32.add -> 1
      kExprEnd
    ])
    .exportFunc();
var module = builder.instantiate();
assertEquals(1, module.exports.test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7ad565^!)  
[src/wasm/baseline/liftoff-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.cc?cl=c7ad565)  
[src/wasm/baseline/liftoff-assembler.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-assembler.h?cl=c7ad565)  
[test/mjsunit/regress/wasm/regress-7035.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7035.js?cl=c7ad565)  
  

---   

## **regress-779407.js (chromium issue)**  
   
**[Issue 779407:
 DCHECK failure in !done() || handler_ == nullptr in frames.cc](https://crbug.com/779407)**  
**[Commit: [regexp] Fix incorrect string length check on arm64.](https://chromium.googlesource.com/v8/v8/+/f155445)**  
  
Date(Commit): Mon Nov 06 13:03:45 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Low", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/753584](https://chromium-review.googlesource.com/753584)  
Regress: [mjsunit/regress/regress-779407.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-779407.js)  
```javascript
var s = '\u1234-------';
for (var i = 0; i < 17; i++) {
  try {
    s += s;
    s += s;
  } catch (e) {
  }
}
s.replace(/[a]/g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f155445^!)  
[src/objects/string.h](https://cs.chromium.org/chromium/src/v8/src/objects/string.h?cl=f155445)  
[src/regexp/arm64/regexp-macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/arm64/regexp-macro-assembler-arm64.cc?cl=f155445)  
[test/mjsunit/regress/regress-779407.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-779407.js?cl=f155445)  
  

---   

## **regress-7033.js (v8 issue)**  
   
**[Issue 7033:
 [Liftoff] Register override on i32 binop](https://crbug.com/v8/7033)**  
**[Commit: [Liftoff] Fix binop code generation bug](https://chromium.googlesource.com/v8/v8/+/407cfc0)**  
  
Date(Commit): Mon Nov 06 11:45:44 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/753462](https://chromium-review.googlesource.com/753462)  
Regress: [mjsunit/regress/wasm/regress-7033.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-7033.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('test', kSig_i_iii)
    .addBodyWithEnd([
      kExprI32Const, 0x07,  // i32.const 7
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32Const, 0x00,  // i32.const 0
      kExprI32And,          // i32.and
      kExprI32And,          // i32.and
      kExprEnd,             // -
    ])
    .exportFunc();
var module = builder.instantiate();
assertEquals(0, module.exports.test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/407cfc0^!)  
[src/wasm/baseline/ia32/liftoff-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/ia32/liftoff-assembler-ia32.h?cl=407cfc0)  
[src/wasm/baseline/liftoff-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/liftoff-compiler.cc?cl=407cfc0)  
[src/wasm/baseline/x64/liftoff-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/wasm/baseline/x64/liftoff-assembler-x64.h?cl=407cfc0)  
[test/mjsunit/regress/wasm/regress-7033.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-7033.js?cl=407cfc0)  
  

---   

## **regress-crbug-781506-1.js (chromium issue)**  
   
**[Issue 781506:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/781506)**  
**[Commit: [turbofan] Generate the correct bounds when the array protector isn't valid.](https://chromium.googlesource.com/v8/v8/+/fd150c7)**  
  
Date(Commit): Sat Nov 04 12:06:31 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner", "Hotlist-Torque"]  
Code Review: [https://chromium-review.googlesource.com/753590](https://chromium-review.googlesource.com/753590)  
Regress: [mjsunit/regress/regress-crbug-781506-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781506-1.js), [mjsunit/regress/regress-crbug-781506-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781506-2.js), [mjsunit/regress/regress-crbug-781506-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781506-3.js)  
```javascript
function foo(a) { return a[0]; }

assertEquals(undefined, foo(x => x));
assertEquals(undefined, foo({}));
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo(x => x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd150c7^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=fd150c7)  
[test/mjsunit/regress/regress-crbug-781506-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781506-1.js?cl=fd150c7)  
[test/mjsunit/regress/regress-crbug-781506-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781506-2.js?cl=fd150c7)  
[test/mjsunit/regress/regress-crbug-781506-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781506-3.js?cl=fd150c7)  
  

---   

## **regress-crbug-781116-1.js (chromium issue)**  
   
**[Issue 781116:
 DCHECK failure in false == cell_reports_intact in isolate.cc](https://crbug.com/781116)**  
**[Commit: [turbofan] Properly handle Array.prototype and Object.prototype in the runtime.](https://chromium.googlesource.com/v8/v8/+/82b3ac9)**  
  
Date(Commit): Fri Nov 03 10:38:51 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/753089](https://chromium-review.googlesource.com/753089)  
Regress: [mjsunit/regress/regress-crbug-781116-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781116-1.js), [mjsunit/regress/regress-crbug-781116-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-781116-2.js)  
```javascript
function baz(obj, store) {
  if (store === true) obj[0] = 1;
}
function bar(store) {
  baz(Array.prototype, store);
}
bar(false);
bar(false);
%OptimizeFunctionOnNextCall(bar);
bar(true);

function foo() { [].push(); }
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82b3ac9^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=82b3ac9)  
[test/mjsunit/regress/regress-crbug-781116-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781116-1.js?cl=82b3ac9)  
[test/mjsunit/regress/regress-crbug-781116-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781116-2.js?cl=82b3ac9)  
  

---   

## **regress-7026.js (v8 issue)**  
   
**[Issue 7026:
 Lots of %KeyedGetProperty calls because of SeqString/ConsString keys in the babylon test](https://crbug.com/v8/7026)**  
**[Commit: [ic] Internalize strings on the fly in KeyedLoadICGeneric.](https://chromium.googlesource.com/v8/v8/+/96b1fdb)**  
  
Date(Commit): Thu Nov 02 20:57:10 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/751464](https://chromium-review.googlesource.com/751464)  
Regress: [mjsunit/regress/regress-7026.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7026.js)  
```javascript
function foo(o, k) { return o[k]; }

const a = "a";
foo([1], 0);
foo({a:1}, a);

const p = new Proxy({}, {
  get(target, name) {
    return name;
  }
});

assertEquals(a + "b", foo(p, a + "b"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96b1fdb^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=96b1fdb)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=96b1fdb)  
[test/mjsunit/regress/regress-7026.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-7026.js?cl=96b1fdb)  
  

---   

## **regress-crbug-779367.js (chromium issue)**  
   
**[Issue 779367:
 Null-dereference READ in v8::internal::CallOptimization::IsCrossContextLazyAccessorPair](https://crbug.com/779367)**  
**[Commit: Check is_simple_api_call before IsCrossContextLazyAccessorPair, accessor could be null](https://chromium.googlesource.com/v8/v8/+/b976b30)**  
  
Date(Commit): Thu Nov 02 14:23:32 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/749812](https://chromium-review.googlesource.com/749812)  
Regress: [mjsunit/regress/regress-crbug-779367.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-779367.js)  
```javascript
function g(o) {
  return o.x;
}

Object.defineProperty(g, 'x', {set(v) {}});

g.prototype = 1;
g(g);
g(g);
%OptimizeFunctionOnNextCall(g);
g(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b976b30^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=b976b30)  
[test/mjsunit/regress/regress-crbug-779367.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-779367.js?cl=b976b30)  
  

---   

## **regress-778917.js (chromium issue)**  
   
**[Issue 778917:
 Out-of-memory in v8_wasm_async_fuzzer](https://crbug.com/778917)**  
**[Commit: [wasm] Improve stack check in the interpreter](https://chromium.googlesource.com/v8/v8/+/793c52e)**  
  
Date(Commit): Thu Nov 02 10:10:27 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Libfuzzer", "Clusterfuzz", "Test-Predator-Wrong", "M-64"]  
Code Review: [https://chromium-review.googlesource.com/744003](https://chromium-review.googlesource.com/744003)  
Regress: [mjsunit/regress/wasm/regress-778917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-778917.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");


const builder = new WasmModuleBuilder();

const index = builder.addFunction("huge_frame", kSig_v_v)
    .addBody([kExprCallFunction, 0])
  .addLocals({f64_count: 49555}).exportFunc().index;
assertEquals(0, index);

const module = builder.instantiate();
assertThrows(module.exports.huge_frame, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/793c52e^!)  
[src/wasm/wasm-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-interpreter.cc?cl=793c52e)  
[src/wasm/wasm-limits.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-limits.h?cl=793c52e)  
[test/mjsunit/regress/wasm/regress-778917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-778917.js?cl=793c52e)  
  

---   

## **regress-crbug-779344.js (chromium issue)**  
   
**[Issue 779344:
 Stack-overflow in v8::internal::CheckObjectType](https://crbug.com/779344)**  
**[Commit: Perform stack check on Proxy call trap.](https://chromium.googlesource.com/v8/v8/+/1e77461)**  
  
Date(Commit): Thu Nov 02 07:29:34 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/743782](https://chromium-review.googlesource.com/743782)  
Regress: [mjsunit/regress/regress-crbug-779344.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-779344.js)  
```javascript
var o = {};
var proxy = new Proxy(() => {}, o);
o.apply = proxy;
assertThrows(proxy);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e77461^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=1e77461)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=1e77461)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=1e77461)  
[src/interpreter/interpreter-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter-assembler.cc?cl=1e77461)  
[src/interpreter/interpreter-assembler.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter-assembler.h?cl=1e77461)  
...  
  

---   

## **regress-crbug-778952.js (chromium issue)**  
   
**[Issue 778952:
 DCHECK failure in raw_properties_or_hash()->IsDictionary() == map()->is_dictionary_map() in object](https://crbug.com/778952)**  
**[Commit: Fix DCHECK in HasFastProperties](https://chromium.googlesource.com/v8/v8/+/a5b0d64)**  
  
Date(Commit): Tue Oct 31 18:06:43 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Regression  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/747272](https://chromium-review.googlesource.com/747272)  
Regress: [mjsunit/regress/regress-crbug-778952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-778952.js)  
```javascript
assertThrows(function() {
  const p = new Proxy({}, {});
  (new Set).add(p);  // Compute the hash code for p.
  null[p] = 0;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5b0d64^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=a5b0d64)  
[test/mjsunit/regress/regress-crbug-778952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-778952.js?cl=a5b0d64)  
  

---   

## **regress-7014-1.js (v8 issue)**  
   
**[Issue 7014:
 MEGAMORPHIC KeyedLoadIC's fall back to %KeyedGetProperty to load string characters](https://crbug.com/v8/7014)**  
**[Commit: [ic] Add OOB support to KeyedLoadIC.](https://chromium.googlesource.com/v8/v8/+/6dc35ab)**  
  
Date(Commit): Tue Oct 31 11:25:53 2017  
Type: Bug  
Code Review: [https://github.com/babel/babel/pull/6589](https://github.com/babel/babel/pull/6589)  
Regress: [mjsunit/regress/regress-7014-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7014-1.js), [mjsunit/regress/regress-7014-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-7014-2.js)  
```javascript
function foo(s) {
  return s[5];
}

assertEquals("f", foo("abcdef"));
assertEquals(undefined, foo("a"));
%OptimizeFunctionOnNextCall(foo);
assertEquals("f", foo("abcdef"));
assertEquals(undefined, foo("a"));
assertOptimized(foo);

String.prototype[5] = "5";

assertEquals("f", foo("abcdef"));
assertEquals("5", foo("a"));
%OptimizeFunctionOnNextCall(foo);
assertEquals("f", foo("abcdef"));
assertEquals("5", foo("a"));
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6dc35ab^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=6dc35ab)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=6dc35ab)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=6dc35ab)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=6dc35ab)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=6dc35ab)  
...  
  

---   

## **regress-crbug-772897.js (chromium issue)**  
   
**[Issue 772897:
 DCHECK failure in !has_pending_exception() in isolate.cc](https://crbug.com/772897)**  
**[Commit: [proxy] Properly handle exceptions from Object::ToName().](https://chromium.googlesource.com/v8/v8/+/ef45d78)**  
  
Date(Commit): Mon Oct 30 15:06:38 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Hotlist-Recharge-BouncingOwner", "M-65", "merge-merged-6.3", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/744041](https://chromium-review.googlesource.com/744041)  
Regress: [mjsunit/regress/regress-crbug-772897.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772897.js)  
```javascript
function store(obj, name) {
  return obj[name] = 0;
}

function f(obj) {
  var key = {
    toString() { throw new Error("boom"); }
  };
  store(obj, key);
}

(function() {
  var proxy = new Proxy({}, {});
  store(proxy, 0)
  assertThrows(() => f(proxy), Error);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef45d78^!)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=ef45d78)  
[test/mjsunit/regress/regress-crbug-772897.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772897.js?cl=ef45d78)  
  

---   

## **regress-778574.js (chromium issue)**  
   
**[Issue 778574:
 Null-dereference READ in v8::internal::MemoryChunk::InNewSpace](https://crbug.com/778574)**  
**[Commit: Fix Array.protoype.slice bug in argument object handling](https://chromium.googlesource.com/v8/v8/+/7dd261c)**  
  
Date(Commit): Thu Oct 26 14:32:56 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Needs-Feedback", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/738148](https://chromium-review.googlesource.com/738148)  
Regress: [mjsunit/regress/regress-778574.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-778574.js)  
```javascript
(function () {
  arguments.length = 7;
  Array.prototype.slice.call(arguments);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7dd261c^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=7dd261c)  
[src/objects/arguments-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/arguments-inl.h?cl=7dd261c)  
[src/objects/arguments.h](https://cs.chromium.org/chromium/src/v8/src/objects/arguments.h?cl=7dd261c)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=7dd261c)  
[test/mjsunit/regress/regress-778574.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-778574.js?cl=7dd261c)  
  

---   

## **regress-775710.js (chromium issue)**  
   
**[Issue 775710:
 CHECK failure: !thrower.error() in module-compiler.cc](https://crbug.com/775710)**  
**[Commit: [asm.js] Limit number of local variables](https://chromium.googlesource.com/v8/v8/+/bb56b7e)**  
  
Date(Commit): Wed Oct 25 12:45:36 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-63", "Merge-Rejected-62", "merge-merged-6.3", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/732660](https://chromium-review.googlesource.com/732660)  
Regress: [mjsunit/regress/wasm/regress-775710.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-775710.js)  
```javascript
const kMaxLocals = 50000;
const fn_template = '"use asm";\nfunction f() { LOCALS }\nreturn f;';
for (var num_locals = kMaxLocals; num_locals < kMaxLocals + 2; ++num_locals) {
  const fn_code = fn_template.replace(
      'LOCALS',
      Array(num_locals)
          .fill()
          .map((_, idx) => 'var l' + idx + ' = 0;')
          .join('\n'));
  const asm_fn = new Function(fn_code);
  const f = asm_fn();
  f();
  assertEquals(num_locals <= kMaxLocals, %IsAsmWasmCode(asm_fn));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb56b7e^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=bb56b7e)  
[test/mjsunit/regress/wasm/regress-775710.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-775710.js?cl=bb56b7e)  
  

---   

## **regress-crbug-774459.js (chromium issue)**  
   
**[Issue 774459:
 Inbox won't load in 32 bit builds](https://crbug.com/774459)**  
**[Commit: [turbofan] Re-enable FindOrderedHashMapEntryForInt32Key optimization.](https://chromium.googlesource.com/v8/v8/+/49e87d2)**  
  
Date(Commit): Wed Oct 25 09:36:56 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["ReleaseBlock-Dev", "M-63", "merge-merged-6.3"]  
Code Review: [https://chromium-review.googlesource.com/737871](https://chromium-review.googlesource.com/737871)  
Regress: [mjsunit/regress/regress-crbug-774459.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-774459.js)  
```javascript
(function() {
  const m = new Map();
  const k = Math.pow(2, 31) - 1;
  m.set(k, 1);

  function foo(m, k) {
    return m.get(k | 0);
  }

  assertEquals(1, foo(m, k));
  assertEquals(1, foo(m, k));
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(1, foo(m, k));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49e87d2^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=49e87d2)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=49e87d2)  
[test/mjsunit/regress/regress-crbug-774459.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-774459.js?cl=49e87d2)  
  

---   

## **regress-6970.js (v8 issue)**  
   
**[Issue 6970:
 Destucturing assignment not rewritten in arrow parameters](https://crbug.com/v8/6970)**  
**[Commit: [parser] Fix rewinding logic for destructuring in arrow params](https://chromium.googlesource.com/v8/v8/+/132152f6)**  
  
Date(Commit): Tue Oct 24 14:54:52 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/733950](https://chromium-review.googlesource.com/733950)  
Regress: [mjsunit/regress/regress-6970.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6970.js)  
```javascript
assertEquals(42, (({a = {b} = {b: 42}}) => a.b)({}));
assertEquals(42, b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/132152f6^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=132152f6)  
[test/mjsunit/regress/regress-6970.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6970.js?cl=132152f6)  
  

---   

## **regress-6991.js (v8 issue)**  
   
**[Issue 6991:
 Terrible deopt loop around property access in prettier benchmark](https://crbug.com/v8/6991)**  
**[Commit: [turbofan] Properly handle smis in monomorphic loads/stores.](https://chromium.googlesource.com/v8/v8/+/a9da0ce)**  
  
Date(Commit): Tue Oct 24 09:19:47 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/735342](https://chromium-review.googlesource.com/735342)  
Regress: [mjsunit/regress/regress-6991.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6991.js)  
```javascript
function foo(o) { return o.x; }

assertEquals(undefined, foo({}));
assertEquals(undefined, foo(1));
assertEquals(undefined, foo({}));
assertEquals(undefined, foo(1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo({}));
assertOptimized(foo);
assertEquals(undefined, foo(1));
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9da0ce^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=a9da0ce)  
[test/mjsunit/regress/regress-6991.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6991.js?cl=a9da0ce)  
  

---   

## **regress-6989.js (v8 issue)**  
   
**[Issue 6989:
 Deopt loop due to IC not leaving UNINITIALIZED state for undefined/null receivers.](https://crbug.com/v8/6989)**  
**[Commit: [ic] Fix undefined/null receivers not leaving UNINITIALIZED state.](https://chromium.googlesource.com/v8/v8/+/5e72557)**  
  
Date(Commit): Tue Oct 24 06:40:56 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/735319](https://chromium-review.googlesource.com/735319)  
Regress: [mjsunit/regress/regress-6989.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6989.js)  
```javascript
(function() {
  function foo(o) { o["x"] = 1; }

  assertThrows(() => foo(undefined));
  assertThrows(() => foo(undefined));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(undefined));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { o["x"] = 1; }

  assertThrows(() => foo(null));
  assertThrows(() => foo(null));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(null));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { return o["x"]; }

  assertThrows(() => foo(undefined));
  assertThrows(() => foo(undefined));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(undefined));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { return o["x"]; }

  assertThrows(() => foo(null));
  assertThrows(() => foo(null));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(null));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { o.x = 1; }

  assertThrows(() => foo(undefined));
  assertThrows(() => foo(undefined));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(undefined));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { o.x = 1; }

  assertThrows(() => foo(null));
  assertThrows(() => foo(null));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(null));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { return o.x; }

  assertThrows(() => foo(undefined));
  assertThrows(() => foo(undefined));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(undefined));
  assertOptimized(foo);
})();

(function() {
  function foo(o) { return o.x; }

  assertThrows(() => foo(null));
  assertThrows(() => foo(null));
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(() => foo(null));
  assertOptimized(foo);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e72557^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=5e72557)  
[test/mjsunit/regress/regress-6989.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6989.js?cl=5e72557)  
  

---   

## **regress-crbug-776511.js (chromium issue)**  
   
**[Issue 776511:
 DCHECK failure in BackingStore::get(backing_store, i, isolate)->IsSmi() || (IsHoleyElementsKind(Ki](https://crbug.com/776511)**  
**[Commit: [Turbofan] Reland Array.prototype.filter inlining.](https://chromium.googlesource.com/v8/v8/+/b3d8499)**  
  
Date(Commit): Mon Oct 23 19:29:50 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/733089](https://chromium-review.googlesource.com/733089)  
Regress: [mjsunit/regress/regress-crbug-776511.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-776511.js)  
```javascript
function __getProperties(obj) {
  let properties = [];
  for (let name of Object.getOwnPropertyNames(obj)) {
    properties.push(name);
  }
  return properties;
}
function __getRandomProperty(obj, seed) {
  let properties = __getProperties(obj);
  return properties[seed % properties.length];
}
(function() {
  var __v_59904 = [12, 13, 14, 16, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25];
  var __v_59906 = function(__v_59908) {
    var __v_59909 = function(__v_59910, __v_59911) {
      if (__v_59911 == 13 && __v_59908) {
        __v_59904.abc = 25;
      }
      return true;
    };
    return __v_59904.filter(__v_59909);
  };
  print(__v_59906());
  __v_59904[__getRandomProperty(__v_59904, 366855)] = this, gc();
  print(__v_59906());
  %OptimizeFunctionOnNextCall(__v_59906);
  var __v_59907 = __v_59906(true);
  print(__v_59907);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b3d8499^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=b3d8499)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=b3d8499)  
[src/builtins/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.cc?cl=b3d8499)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=b3d8499)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=b3d8499)  
...  
  

---   

## **regress-776677.js (chromium issue)**  
   
**[Issue 776677:
 Security: V8:Use After Free Leads to Remote Code Execution](https://crbug.com/776677)**  
**[Commit: [wasm] Fix Memory.grow when shared with asm.js modules](https://chromium.googlesource.com/v8/v8/+/5f960df)**  
  
Date(Commit): Mon Oct 23 15:49:03 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "reward-7500", "allpublic", "reward-inprocess", "NodeJS-Backport-Done", "M-65", "merge-merged-6.2", "merge-merged-6.3", "Release-2-M62", "CVE-2017-15399", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/731844](https://chromium-review.googlesource.com/731844)  
Regress: [mjsunit/regress/wasm/regress-776677.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-776677.js)  
```javascript
function module(stdlib,foreign,buffer) {
  "use asm";
  var fl = new stdlib.Uint32Array(buffer);
  function f1(x) {
    x = x | 0;
    fl[0] = x;
    fl[0x10000] = x;
    fl[0x100000] = x;
  }
  return f1;
}

var global = {Uint32Array:Uint32Array};
var env = {};
memory = new WebAssembly.Memory({initial:128});
var buffer = memory.buffer;
evil_f = module(global,env,buffer);

zz = {};
zz.toString = function() {
  Array.prototype.slice.call([]);
  return 0xffffffff;
}
evil_f(3);
assertThrows(() => memory.grow(1), RangeError);
evil_f(zz);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f960df^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=5f960df)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=5f960df)  
[src/objects/js-array-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array-inl.h?cl=5f960df)  
[src/objects/js-array.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-array.h?cl=5f960df)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=5f960df)  
...  
  

---   

## **regress-777182.js (chromium issue)**  
   
**[Issue 777182:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray()) in obj](https://crbug.com/777182)**  
**[Commit: [typedarrays] Fix a wrong type casting in TA.p.set](https://chromium.googlesource.com/v8/v8/+/6241e81)**  
  
Date(Commit): Mon Oct 23 10:34:11 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/732865](https://chromium-review.googlesource.com/732865)  
Regress: [mjsunit/es6/regress/regress-777182.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-777182.js)  
```javascript
var __v_65159 = [1.3];
__v_65159.length = 0;
new Int8Array(10).set(__v_65159);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6241e81^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=6241e81)  
[test/mjsunit/es6/regress/regress-777182.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-777182.js?cl=6241e81)  
  

---   

## **regress-776338.js (chromium issue)**  
   
**[Issue 776338:
 Proxies of an object which has a programmatic getter call the getter erroneously](https://crbug.com/776338)**  
**[Commit: [proxy] Fix invalid call to getter in [[Get/Set]]](https://chromium.googlesource.com/v8/v8/+/14165a4)**  
  
Date(Commit): Mon Oct 23 07:48:22 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Hotlist-Merge-Review", "hasbisect", "Hotlist-Interop", "NodeJS-Backport-Done", "Via-Wizard-API", "M-62", "Needs-Triage-M62", "merge-merged-6.2", "Triaged-ET", "merge-merged-6.3"]  
Code Review: [https://chromium-review.googlesource.com/731123](https://chromium-review.googlesource.com/731123)  
Regress: [mjsunit/regress/regress-776338.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-776338.js)  
```javascript
const obj = {};
Object.defineProperty(obj, 'value', {
  enumerable: true,
  configurable: true,
  get: assertUnreachable,
  set: assertUnreachable,
});

let called_get = false;
let called_has = false;
let called_set = false;

const has = function(target, prop) {
  assertEquals('value', prop);
  called_has = true;
  return false;  // Need to return false to trigger GetOwnProperty call.
};

const get = function(target, prop) {
  assertEquals('value', prop);
  called_get = true;
  return 'yep';
};

const set = function(target, prop, value) {
  assertEquals('value', prop);
  called_set = true;
  return true;    // Need to return true to trigger GetOwnProperty call.
};

const proxy = new Proxy(obj, { has, get, set });

assertFalse(Reflect.has(proxy, 'value'));
assertTrue(called_has);

assertEquals('nope', proxy.value = 'nope');
assertTrue(called_set);

assertEquals('yep', proxy.value);
assertTrue(called_get);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14165a4^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=14165a4)  
[test/mjsunit/regress/regress-776338.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-776338.js?cl=14165a4)  
  

---   

## **regress-775366.js (chromium issue)**  
   
**[Issue 775366:
 Null-dereference READ in v8::internal::Signature<v8::internal::MachineRepresentation>::return_count](https://crbug.com/775366)**  
**[Commit: g# Enter a description of the change.](https://chromium.googlesource.com/v8/v8/+/b2199fa)**  
  
Date(Commit): Fri Oct 20 14:00:34 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/730752](https://chromium-review.googlesource.com/730752)  
Regress: [mjsunit/regress/wasm/regress-775366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-775366.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function BadTypeSection() {
  var data = bytes(
    kWasmH0,
    kWasmH1,
    kWasmH2,
    kWasmH3,

    kWasmV0,
    kWasmV1,
    kWasmV2,
    kWasmV3,

    kTypeSectionCode,
    5,
    2,
    0x60,
    0,
    0,
    13
  );

  assertFalse(WebAssembly.validate(data));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b2199fa^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=b2199fa)  
[test/mjsunit/regress/wasm/regress-775366.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-775366.js?cl=b2199fa)  
  

---   

## **regress-6948.js (v8 issue)**  
   
**[Issue 6948:
 Investigate potential deoptimization loop with KeyedLoadIC feedback in the jshint test](https://crbug.com/v8/6948)**  
**[Commit: [ic] Ensure that we make progress on KeyedLoadIC polymorphic name.](https://chromium.googlesource.com/v8/v8/+/d5c19aa)**  
  
Date(Commit): Fri Oct 20 12:16:10 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/730224](https://chromium-review.googlesource.com/730224)  
Regress: [mjsunit/regress/regress-6948.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6948.js)  
```javascript
var o = {};

function foo(s) { return o[s]; }

var s = 'c' + 'c';
foo(s);
foo(s);
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo(s));
assertOptimized(foo);
assertEquals(undefined, foo('c' + 'c'));
assertOptimized(foo);
assertEquals(undefined, foo('ccddeeffgg'.slice(0, 2)));
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5c19aa^!)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=d5c19aa)  
[src/builtins/builtins-ic-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-ic-gen.cc?cl=d5c19aa)  
[src/compiler/c-linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/c-linkage.cc?cl=d5c19aa)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=d5c19aa)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=d5c19aa)  
...  
  

---   

## **regress-775888.js (chromium issue)**  
   
**[Issue 775888:
 DCHECK failure in array->map() != fixed_cow_array_map() in heap.cc](https://crbug.com/775888)**  
**[Commit: Ensure inlined Array.protoype.shift() calls return non-COW arrays](https://chromium.googlesource.com/v8/v8/+/0454a84)**  
  
Date(Commit): Thu Oct 19 15:05:44 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Hotlist-Torque"]  
Code Review: [https://chromium-review.googlesource.com/727900](https://chromium-review.googlesource.com/727900)  
Regress: [mjsunit/regress/regress-775888.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-775888.js)  
```javascript
function __f_7586(__v_27535) {
  let a = __v_27535.shift();
  return a;
}

function __f_7587() {
  var __v_27536 = [ 1, 15, 16];
  __f_7586(__v_27536);
  __v_27536.unshift(__v_27536);
}
__f_7587();
__f_7587();

%OptimizeFunctionOnNextCall(__f_7586);
__f_7587();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0454a84^!)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=0454a84)  
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=0454a84)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=0454a84)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=0454a84)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=0454a84)  
...  
  

---   

## **regress-6931.js (v8 issue)**  
   
**[Issue 6931:
 [wasm] Missing out-of-bounds trap on "wrapping" offset](https://crbug.com/v8/6931)**  
**[Commit: [wasm] add a test for accidental sign extension](https://chromium.googlesource.com/v8/v8/+/ef2036a)**  
  
Date(Commit): Thu Oct 19 04:09:21 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/727022](https://chromium-review.googlesource.com/727022)  
Regress: [mjsunit/regress/wasm/regress-6931.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-6931.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');




(function() {
  let builder = new WasmModuleBuilder();
  builder.addMemory(1, 1, false);
  builder.addFunction('test', kSig_v_v)
      .addBody([
        kExprI32Const, 0x7c, // address = -4
        kExprI32Const, 0,
        kExprI32StoreMem, 0, 8, // align = 0, offset = 8
      ])
      .exportFunc();
  let module = builder.instantiate();

  assertTraps(kTrapMemOutOfBounds, module.exports.test);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef2036a^!)  
[test/mjsunit/regress/wasm/regress-6931.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-6931.js?cl=ef2036a)  
  

---   

## **regress-crbug-766635.js (chromium issue)**  
   
**[No Permission](https://crbug.com/766635)**  
**[Commit: [Turbofan] Array.prototype.filter inlining.](https://chromium.googlesource.com/v8/v8/+/9fd029e)**  
  
Date(Commit): Wed Oct 18 17:09:27 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/721119](https://chromium-review.googlesource.com/721119)  
Regress: [mjsunit/regress/regress-crbug-766635.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-766635.js)  
```javascript
function classOf() {; }
function PrettyPrint(value) { return ""; }
function fail() { }
function deepEquals(a, b) { if (a === b) { if (a === 0)1 / b; return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(); if (b) return false; if (objectClass === "RegExp") {; } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (a[i][i]) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf()) return false; }; }
assertSame = function assertSame() { if (found === expected) { if (1 / found) return; } else if ((expected !== expected) && (found !== found)) { return; }; }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected),); } };
var __v_3 = {};
function __f_0() {
  assertEquals();
}
try {
  __f_0();
} catch(e) {; }
__v_2 = 0;
o2 = {y:1.5};
o2.y = 0;
o3 = o2.y;
function __f_1() {
  for (var __v_1 = 0; __v_1 < 10; __v_1++) {
    __v_2 += __v_3.x + o3.foo;
    [ 3].filter(__f_9);
  }
}
__f_1();
%OptimizeFunctionOnNextCall(__f_1);
__f_1();
function __f_9(){ "use __f_9"; assertEquals( this); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9fd029e^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=9fd029e)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=9fd029e)  
[src/builtins/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.cc?cl=9fd029e)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=9fd029e)  
[src/compiler/js-call-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.h?cl=9fd029e)  
...  
  

---   

## **regress-v8-6940.js (v8 issue)**  
   
**[Issue 6940:
 Bug on matching diacritic characters.](https://crbug.com/v8/6940)**  
**[Commit: [regexp] Fix a bug causing early aborts from AddCaseEquivalents](https://chromium.googlesource.com/v8/v8/+/8016f30)**  
  
Date(Commit): Wed Oct 18 12:18:59 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/725430](https://chromium-review.googlesource.com/725430)  
Regress: [mjsunit/regress/regress-v8-6940.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6940.js)  
```javascript
assertTrue(/[ŸŶ]/i.test('ÿ'));
assertTrue(/[ŸY]/i.test('ÿ'));

assertTrue(/[YÝŸŶỲ]/i.test('ÿ'));
assertTrue(/[YÝŸŶỲ]/iu.test('ÿ'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8016f30^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=8016f30)  
[test/mjsunit/regress/regress-v8-6940.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6940.js?cl=8016f30)  
  

---   

## **regress-v8-6906.js (v8 issue)**  
   
**[Issue 6906:
 Deopt crash: test/mjsunit/regress/regress-330046](https://crbug.com/v8/6906)**  
**[Commit: [disassembler] Handle the case of optimized code object with unlinked deopt data.](https://chromium.googlesource.com/v8/v8/+/54f7cd6)**  
  
Date(Commit): Wed Oct 18 10:46:01 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/725343](https://chromium-review.googlesource.com/725343)  
Regress: [mjsunit/regress/regress-v8-6906.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6906.js)  
```javascript
function f() {}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();
%DeoptimizeFunction(f);

%DisassembleFunction(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54f7cd6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=54f7cd6)  
[test/mjsunit/regress/regress-v8-6906.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6906.js?cl=54f7cd6)  
  

---   

## **regress-774824.js (chromium issue)**  
   
**[Issue 774824:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedArray()) in objects-i](https://crbug.com/774824)**  
**[Commit: [deoptimizer] Remove incorrect cast for materialized property array.](https://chromium.googlesource.com/v8/v8/+/57c6c97)**  
  
Date(Commit): Wed Oct 18 08:13:51 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/725339](https://chromium-review.googlesource.com/725339)  
Regress: [mjsunit/regress/regress-774824.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-774824.js)  
```javascript
function f() {
  var a = new Set().values();
  a.outOfObjectProperty = undefined;
  %DeoptimizeNow();
  return !a;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/57c6c97^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=57c6c97)  
[test/mjsunit/regress/regress-774824.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-774824.js?cl=57c6c97)  
  

---   

## **regress-773954.js (chromium issue)**  
   
**[Issue 773954:
 DCHECK failure in 0 == node->op()->EffectOutputCount() in memory-optimizer.cc](https://crbug.com/773954)**  
**[Commit: Reland^4 "[turbofan] eagerly prune None types and deadness from the graph"](https://chromium.googlesource.com/v8/v8/+/1cee0e0)**  
  
Date(Commit): Wed Oct 18 05:24:17 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://bugs.chromium.org/p/chromium/issues/detail?id=773954.](https://bugs.chromium.org/p/chromium/issues/detail?id=773954.)  
Regress: [mjsunit/compiler/regress-773954.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-773954.js)  
```javascript
'use strict';
var a = { x:0 };
var b = {};
Object.defineProperty(b, "x", {get: function () {}});

function f(o) {
  return 5 + o.x++;
}

try {
  f(a);
  f(b);
} catch (e) {}
%OptimizeFunctionOnNextCall(f);
f(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1cee0e0^!)  
[src/compiler/branch-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/branch-elimination.cc?cl=1cee0e0)  
[src/compiler/common-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.cc?cl=1cee0e0)  
[src/compiler/common-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.h?cl=1cee0e0)  
[src/compiler/dead-code-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.cc?cl=1cee0e0)  
[src/compiler/dead-code-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.h?cl=1cee0e0)  
...  
  

---   

## **regress-772420.js (chromium issue)**  
   
**[Issue 772420:
 DCHECK failure in right_type()->Is(Type::PlainPrimitive()) in js-typed-lowering.cc](https://crbug.com/772420)**  
**[Commit: [TurboFan] Fix type checks for lowering SpeculativeNumberBinop.](https://chromium.googlesource.com/v8/v8/+/3118f47)**  
  
Date(Commit): Tue Oct 17 16:12:49 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/715717](https://chromium-review.googlesource.com/715717)  
Regress: [mjsunit/compiler/regress-772420.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-772420.js)  
```javascript
function foo(arg) {
  var value;
  // None of the branches of this switch are ever taken, but
  // the sequence means value could be the hole.
  switch (arg) {
    case 1:
      let let_var = 1;
    case 2:
      value = let_var;
  }
  // Speculative number binop with NumberOrOddball feedback.
  // Shouldn't be optimized to pure operator since value's phi
  // could theoretically be the hole (we would have already thrown a
  // reference error in case 2 above if so, but TF typing still
  // thinks it could be the hole).
  return value * undefined;
}

foo(3);
foo(3);
%OptimizeFunctionOnNextCall(foo);
foo(3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3118f47^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=3118f47)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=3118f47)  
[test/mjsunit/compiler/regress-772420.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-772420.js?cl=3118f47)  
  

---   

## **regress-772332.js (chromium issue)**  
   
**[Issue 772332:
 DCHECK failure in !target->function->imported in wasm-interpreter.cc](https://crbug.com/772332)**  
**[Commit: [wasm] Add regression tests for some recently fixed WasmInterpreter issues.](https://chromium.googlesource.com/v8/v8/+/bff42d3)**  
  
Date(Commit): Tue Oct 17 12:04:40 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/721666](https://chromium-review.googlesource.com/721666)  
Regress: [mjsunit/regress/wasm/regress-772332.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-772332.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

assertThrows(() => {
let __v_50315 = 0;
function __f_15356(__v_50316, __v_50317) {
  let __v_50318 = new WasmModuleBuilder();
  if (__v_50317) {
    let __v_50319 = __v_50318.addImport('import_module', 'other_module_fn', kSig_i_i);
  }
      __v_50318.addMemory();
      __v_50318.addFunction('load', kSig_i_i).addBody([ 0, 0, 0]).exportFunc();
  return __v_50318;
}
  (function __f_15357() {
    let __v_50320 = __f_15356(__v_50350 = false, __v_50351 = kSig_i_i);
      __v_50320.addFunction('plus_one', kSig_i_i).addBody([kExprGetLocal, 0, kExprCallFunction, __v_50315, kExprI32Const, kExprI32Add, kExprReturn]).exportFunc();
    let __v_50321 = __f_15356();
    let __v_50324 = __v_50321.instantiate();
    let __v_50325 = __v_50320.instantiate({
      import_module: {
        other_module_fn: __v_50324.exports.load
      }
    });
 __v_50325.exports.plus_one();
  })();
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bff42d3^!)  
[test/mjsunit/regress/wasm/regress-766003.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-766003.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-771243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-771243.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-772332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-772332.js?cl=bff42d3)  
  

---   

## **regress-771243.js (chromium issue)**  
   
**[Issue 771243:
 Null-dereference READ in v8::internal::wasm::ThreadImpl::PushFrame](https://crbug.com/771243)**  
**[Commit: [wasm] Add regression tests for some recently fixed WasmInterpreter issues.](https://chromium.googlesource.com/v8/v8/+/bff42d3)**  
  
Date(Commit): Tue Oct 17 12:04:40 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/721666](https://chromium-review.googlesource.com/721666)  
Regress: [mjsunit/regress/wasm/regress-771243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-771243.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

assertThrows(() => {
 __v_29 = 0;
function __f_1() {
 __v_19 = new WasmModuleBuilder();
  if (__v_25) {
 __v_23 = __v_19.addImport('__v_24', '__v_30', __v_25);
  }
  if (__v_18) {
 __v_19.addMemory();
 __v_19.addFunction('load', kSig_i_i)
        .addBody([ 0])
        .exportFunc();
  }
 return __v_19;
}
 (function TestExternalCallBetweenTwoWasmModulesWithoutAndWithMemory() {
 __v_21 = __f_1(__v_18 = false, __v_25 = kSig_i_i);
 __v_21.addFunction('plus_one', kSig_i_i)
      .addBody([
        kExprGetLocal, 0,                   // -
        kExprCallFunction, __v_29      ])
      .exportFunc();
 __v_32 =
      __f_1(__v_18 = true, __v_25 = undefined);
 __v_31 = __v_32.instantiate(); try { __v_32[__getRandomProperty()] = __v_0; delete __v_18[__getRandomProperty()]; delete __v_34[__getRandomProperty()]; } catch(e) {; };
 __v_20 = __v_21.instantiate(
      {__v_24: {__v_30: __v_31.exports.load}});
 __v_20.exports.plus_one(); __v_33 = __v_43;
})();
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bff42d3^!)  
[test/mjsunit/regress/wasm/regress-766003.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-766003.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-771243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-771243.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-772332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-772332.js?cl=bff42d3)  
  

---   

## **regress-766003.js (chromium issue)**  
   
**[Issue 766003:
 DCHECK failure in !it.done() in wasm-module.cc](https://crbug.com/766003)**  
**[Commit: [wasm] Add regression tests for some recently fixed WasmInterpreter issues.](https://chromium.googlesource.com/v8/v8/+/bff42d3)**  
  
Date(Commit): Tue Oct 17 12:04:40 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/721666](https://chromium-review.googlesource.com/721666)  
Regress: [mjsunit/regress/wasm/regress-766003.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-766003.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

 __v_6 = new WasmModuleBuilder();
__v_6.addFunction('exp1', kSig_i_i).addBody([kExprUnreachable]).exportFunc();
 __v_7 = new WasmModuleBuilder();
 __v_7.addImport('__v_11', '__v_11', kSig_i_i);
try {
; } catch(e) {; }
 __v_8 = __v_6.instantiate().exports.exp1;
 __v_9 = __v_7.instantiate({__v_11: {__v_11: __v_8}}).exports.call_imp;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bff42d3^!)  
[test/mjsunit/regress/wasm/regress-766003.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-766003.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-771243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-771243.js?cl=bff42d3)  
[test/mjsunit/regress/wasm/regress-772332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-772332.js?cl=bff42d3)  
  

---   

## **regress-crbug-774860.js (chromium issue)**  
   
**[Issue 774860:
 CHECK failure: map->IsMap() in spaces.cc](https://crbug.com/774860)**  
**[Commit: Fix slack tracking for function subclasses.](https://chromium.googlesource.com/v8/v8/+/b0fc245)**  
  
Date(Commit): Tue Oct 17 10:37:04 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/721551](https://chromium-review.googlesource.com/721551)  
Regress: [mjsunit/regress/regress-crbug-774860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-774860.js)  
```javascript
(function () {
  class F extends Function {}
  let f = new F("'use strict';");
  // Create enough objects to complete slack tracking.
  for (let i = 0; i < 20; i++) {
    new F();
  }
  gc();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0fc245^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b0fc245)  
[test/mjsunit/regress/regress-crbug-774860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-774860.js?cl=b0fc245)  
  

---   

## **regress-crbug-774994.js (chromium issue)**  
   
**[Issue 774994:
 CHECK failure: args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/774994)**  
**[Commit: [parser] Skipping inner funcs: accurately record NeedsHomeObject](https://chromium.googlesource.com/v8/v8/+/94a71d7)**  
  
Date(Commit): Tue Oct 17 01:49:36 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Security_Severity-Low", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-63", "ClusterFuzz-Top-Crash", "merge-merged-6.3"]  
Code Review: [https://chromium-review.googlesource.com/721879](https://chromium-review.googlesource.com/721879)  
Regress: [mjsunit/regress/regress-crbug-774994.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-774994.js)  
```javascript
function f() {
  new class extends Object {
    constructor() {
      eval("super(); super.__f_10();");
    }
  }
}
assertThrows(f, TypeError);

function g() {
  let obj = {
    m() {
      eval("super.foo()");
    }
  }
  obj.m();
}
assertThrows(g, TypeError);

function h() {
  let obj = {
    get m() {
      eval("super.foo()");
    }
  }
  obj.m;
}
assertThrows(h, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/94a71d7^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=94a71d7)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=94a71d7)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=94a71d7)  
[src/parsing/preparsed-scope-data.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparsed-scope-data.cc?cl=94a71d7)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=94a71d7)  
...  
  

---   

## **regress-6941.js (v8 issue)**  
   
**[Issue 6941:
 CodeStubAssembler::Equal can forget type feedback](https://crbug.com/v8/6941)**  
**[Commit: Fix type feedback recording in CodeStubAssembler::Equal.](https://chromium.googlesource.com/v8/v8/+/7a7639e)**  
  
Date(Commit): Mon Oct 16 14:41:20 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/721020](https://chromium-review.googlesource.com/721020)  
Regress: [mjsunit/regress/regress-6941.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6941.js)  
```javascript
function foo(x) {
  return Symbol.iterator == x;
}

function main() {
  foo(Symbol());
  foo({valueOf() { return Symbol.toPrimitive}});
}

%NeverOptimizeFunction(main);
main();
%OptimizeFunctionOnNextCall(foo);
main();
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a7639e^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=7a7639e)  
[test/mjsunit/regress/regress-6941.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6941.js?cl=7a7639e)  
  

---   

## **regress-774475.js (chromium issue)**  
   
**[Issue 774475:
 DCHECK failure in (function_) == nullptr in scopes.cc](https://crbug.com/774475)**  
**[Commit: [parser] Skipping inner funcs: fix related to aborting preparsing.](https://chromium.googlesource.com/v8/v8/+/d69159d)**  
  
Date(Commit): Mon Oct 16 13:31:49 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-0", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-6.3", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/720920](https://chromium-review.googlesource.com/720920)  
Regress: [mjsunit/regress/regress-774475.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-774475.js)  
```javascript
var o = function f3() {
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
  x = 1;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d69159d^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=d69159d)  
[test/mjsunit/regress/regress-774475.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-774475.js?cl=d69159d)  
  

---   

## **regress-crbug-768080.js (chromium issue)**  
   
**[Issue 768080:
 CHECK failure: args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/768080)**  
**[Commit: [turbofan] Fix new.target check in Reflect.construct.](https://chromium.googlesource.com/v8/v8/+/afd2f58)**  
  
Date(Commit): Fri Oct 13 13:13:12 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "ClusterFuzz-Top-Crash", "merge-merged-6.3", "Release-0-M63"]  
Code Review: [https://chromium-review.googlesource.com/718100](https://chromium-review.googlesource.com/718100)  
Regress: [mjsunit/regress/regress-crbug-768080.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-768080.js)  
```javascript
(function TestReflectConstructBogusNewTarget1() {
  class C {}
  function g() {
    Reflect.construct(C, arguments, 23);
  }
  function f() {
    return new g();
  }
  new C();  // Warm-up!
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();

(function TestReflectConstructBogusNewTarget2() {
  class C {}
  // Note that {unescape} is an example of a non-constructable function. If that
  // ever changes and this test needs to be adapted, make sure to choose another
  // non-constructable {JSFunction} object instead.
  function g() {
    Reflect.construct(C, arguments, unescape);
  }
  function f() {
    return new g();
  }
  new C();  // Warm-up!
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();

(function TestReflectConstructBogusTarget() {
  function g() {
    Reflect.construct(23, arguments);
  }
  function f() {
    return new g();
  }
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();

(function TestReflectApplyBogusTarget() {
  function g() {
    Reflect.apply(23, this, arguments);
  }
  function f() {
    return g();
  }
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afd2f58^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=afd2f58)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=afd2f58)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=afd2f58)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=afd2f58)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=afd2f58)  
...  
  

---   

## **regress-crbug-768875.js (chromium issue)**  
   
**[Issue 768875:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/768875)**  
**[Commit: [ic] Introduce proper slow stub for StoreGlobalIC.](https://chromium.googlesource.com/v8/v8/+/3384a79)**  
  
Date(Commit): Thu Oct 12 14:07:41 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/715802](https://chromium-review.googlesource.com/715802)  
Regress: [mjsunit/regress/regress-crbug-768875.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-768875.js)  
```javascript
this.__defineGetter__('x', function() { return 0; });
function store_x() {
  x = 23;
}
store_x();
store_x();
assertEquals(0, x);
Realm.eval(Realm.current(), "let x = 42");
assertEquals(42, x);
store_x();
assertEquals(23, x);


this.__defineGetter__('y', function() { return 0; });
function store_y() {
  y = 23;
}
store_y();
store_y();
assertEquals(0, y);
Realm.eval(Realm.current(), "const y = 42");
assertEquals(42, y);
assertThrows(store_y, TypeError);
assertEquals(42, y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3384a79^!)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=3384a79)  
[src/builtins/builtins-handler-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-handler-gen.cc?cl=3384a79)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=3384a79)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=3384a79)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=3384a79)  
...  
  

---   

## **regress-crbug-772720.js (chromium issue)**  
   
**[Issue 772720:
 CHECK failure: NodeProperties::GetType(val)->Is(NodeProperties::GetType(node)) in verifier.cc](https://crbug.com/772720)**  
**[Commit: [turbofan] Fix type of inline cons-string allocation.](https://chromium.googlesource.com/v8/v8/+/93f855c)**  
  
Date(Commit): Thu Oct 12 10:02:29 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-65", "merge-merged-6.3", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/708796](https://chromium-review.googlesource.com/708796)  
Regress: [mjsunit/regress/regress-crbug-772720.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772720.js)  
```javascript
var global;
function f() {
  var local = 'abcdefghijklmnopqrst';
  local += 'abcdefghijkl' + (0 + global);
  global += 'abcdefghijkl';
}
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93f855c^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=93f855c)  
[test/mjsunit/regress/regress-crbug-772720.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772720.js?cl=93f855c)  
  

---   

## **regress-crbug-764219.js (chromium issue)**  
   
**[Issue 764219:
 Ill in v8::internal::__RT_impl_Runtime_AbortJS](https://crbug.com/764219)**  
**[Commit: [ic] Do access checks when storing via JSGlobalProxy.](https://chromium.googlesource.com/v8/v8/+/5ea95fe)**  
  
Date(Commit): Thu Oct 12 09:10:55 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "ReleaseBlock-Stable", "Clusterfuzz", "M-63", "ClusterFuzz-Top-Crash", "merge-merged-6.3", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/712454](https://chromium-review.googlesource.com/712454)  
Regress: [mjsunit/regress/regress-crbug-764219.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-764219.js)  
```javascript
(function() {
  function f(o) {
    o.x = 42;
  };

  f({});
  f(this);
  f(this);
})();

(function() {
  function f(o) {
    o.y = 153;
  };

  Object.setPrototypeOf(this, new Proxy({}, {}));
  f({});
  f(this);
  f(this);
})();

(function() {
  function f(o) {
    o.z = 153;
  };

  Object.setPrototypeOf(this, new Proxy({get z(){}}, {}));
  f({});
  f(this);
  f(this);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ea95fe^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=5ea95fe)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=5ea95fe)  
[src/ic/handler-configuration-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration-inl.h?cl=5ea95fe)  
[src/ic/handler-configuration.cc](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.cc?cl=5ea95fe)  
[src/ic/handler-configuration.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.h?cl=5ea95fe)  
...  
  

---   

## **regress-772872.js (chromium issue)**  
   
**[Issue 772872:
 Ill in v8::internal::compiler::Verifier::Visitor::Check](https://crbug.com/772872)**  
**[Commit: Reland^3 "[turbofan] eagerly prune None types and deadness from the graph"](https://chromium.googlesource.com/v8/v8/+/4cf4764)**  
  
Date(Commit): Wed Oct 11 16:23:00 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://bugs.chromium.org/p/chromium/issues/detail?id=772872.](https://bugs.chromium.org/p/chromium/issues/detail?id=772872.)  
Regress: [mjsunit/compiler/regress-772872.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-772872.js)  
```javascript
function f() {
  for (var x = 10; x > 5; x -= 16) {}
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4cf4764^!)  
[src/compiler/branch-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/branch-elimination.cc?cl=4cf4764)  
[src/compiler/common-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.cc?cl=4cf4764)  
[src/compiler/common-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.h?cl=4cf4764)  
[src/compiler/dead-code-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.cc?cl=4cf4764)  
[src/compiler/dead-code-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/dead-code-elimination.h?cl=4cf4764)  
...  
  

---   

## **regress-772649.js (chromium issue)**  
   
**[Issue 772649:
 Ill in v8::internal::TranslatedState::MaterializeCapturedObjectAt](https://crbug.com/772649)**  
**[Commit: [esnext] fix MaterializeCapturedObjectAt for async generator objects](https://chromium.googlesource.com/v8/v8/+/9f0bdf0)**  
  
Date(Commit): Tue Oct 10 11:18:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/706780](https://chromium-review.googlesource.com/706780)  
Regress: [mjsunit/harmony/regress/regress-772649.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-772649.js)  
```javascript
async function* gen([[notIterable]] = [null]) {}
assertThrows(() => gen(), TypeError);
assertThrows(() => gen(), TypeError);
%OptimizeFunctionOnNextCall(gen);
assertThrows(() => gen(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9f0bdf0^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9f0bdf0)  
[test/mjsunit/harmony/regress/regress-772649.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-772649.js?cl=9f0bdf0)  
  

---   

## **regress-6907.js (v8 issue)**  
   
**[Issue 6907:
 Deoptimizer misses to materialize values in builtin continuation registers ...](https://crbug.com/v8/6907)**  
**[Commit: [deoptimizer] Fix materialization of builtin stub registers.](https://chromium.googlesource.com/v8/v8/+/0a7fcd0)**  
  
Date(Commit): Tue Oct 10 07:50:09 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/707245](https://chromium-review.googlesource.com/707245)  
Regress: [mjsunit/regress/regress-6907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6907.js)  
```javascript
(function TestDematerializedContextInBuiltin() {
  var f = function() {
    var b = [1,2,3];
    var callback = function(v,i,o) {
      %_DeoptimizeNow();
    };
    try { throw 0 } catch(e) {
      return b.forEach(callback);
    }
  }
  f();
  f();
  %OptimizeFunctionOnNextCall(f);
  f();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a7fcd0^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=0a7fcd0)  
[test/mjsunit/regress/regress-6907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6907.js?cl=0a7fcd0)  
  

---   

## **regress-crbug-772610.js (chromium issue)**  
   
**[Issue 772610:
 Null-dereference READ in Relaxed_Load](https://crbug.com/772610)**  
**[Commit: [deoptimizer] Fix JSFunction materialization instance size.](https://chromium.googlesource.com/v8/v8/+/c34a295)**  
  
Date(Commit): Mon Oct 09 14:01:05 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/707109](https://chromium-review.googlesource.com/707109)  
Regress: [mjsunit/regress/regress-crbug-772610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772610.js)  
```javascript
function f() {
  var o = [{
    [Symbol.toPrimitive]() {}
  }];
  %_DeoptimizeNow();
  return o.length;
}
assertEquals(1, f());
assertEquals(1, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f());
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c34a295^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=c34a295)  
[test/mjsunit/regress/regress-crbug-772610.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772610.js?cl=c34a295)  
  

---   

## **regress-crbug-770581.js (chromium issue)**  
   
**[Issue 770581:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/770581)**  
**[Commit: [turbofan] Unify error message on non-callable callback.](https://chromium.googlesource.com/v8/v8/+/bb46a59)**  
  
Date(Commit): Mon Oct 09 12:05:28 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/702378](https://chromium-review.googlesource.com/702378)  
Regress: [mjsunit/regress/regress-crbug-770581.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-770581.js)  
```javascript
function f(callback) {
  [Object].forEach(callback);
}

function message_of_f() {
  try {
    f("a teapot");
  } catch(e) {
    return String(e);
  }
}

assertEquals("TypeError: a teapot is not a function", message_of_f());
assertEquals("TypeError: a teapot is not a function", message_of_f());
%OptimizeFunctionOnNextCall(f);
assertEquals("TypeError: a teapot is not a function", message_of_f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb46a59^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=bb46a59)  
[test/mjsunit/regress/regress-crbug-770581.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-770581.js?cl=bb46a59)  
  

---   

## **regress-crbug-772689.js (chromium issue)**  
   
**[Issue 772689:
 CHECK failure: 0 == field_count_ in deoptimizer.cc](https://crbug.com/772689)**  
**[Commit: [deoptimizer] Properly handle in-object properties on JSArrays.](https://chromium.googlesource.com/v8/v8/+/bed8853)**  
  
Date(Commit): Mon Oct 09 08:28:51 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/706781](https://chromium-review.googlesource.com/706781)  
Regress: [mjsunit/regress/regress-crbug-772689.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772689.js)  
```javascript
const A = class A extends Array {
  constructor() {
    super();
    this.y = 1;
  }
}

function foo(x) {
  var a = new A();
  if (x) return a.y;
}

assertEquals(undefined, foo(false));
assertEquals(undefined, foo(false));
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo(false));
assertEquals(1, foo(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bed8853^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=bed8853)  
[test/mjsunit/regress/regress-crbug-772689.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772689.js?cl=bed8853)  
  

---   

## **regress-crbug-772672.js (chromium issue)**  
   
**[Issue 772672:
 Null-dereference WRITE in raise](https://crbug.com/772672)**  
**[Commit: Fix JSArray::kInitialMaxFastElementArray to make sense for 32-bit platforms.](https://chromium.googlesource.com/v8/v8/+/2bb704e)**  
  
Date(Commit): Mon Oct 09 07:49:41 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/706782](https://chromium-review.googlesource.com/706782)  
Regress: [mjsunit/regress/regress-crbug-772672.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772672.js)  
```javascript
function foo() { return new Array(120 * 1024); }

foo()[0] = 0.1;
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2bb704e^!)  
[src/builtins/builtins-constructor.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor.h?cl=2bb704e)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=2bb704e)  
[test/mjsunit/regress/regress-crbug-772672.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772672.js?cl=2bb704e)  
  

---   

## **regress-crbug-772056.js (chromium issue)**  
   
**[Issue 772056:
 DCHECK failure in new_len >= old_len in heap.cc](https://crbug.com/772056)**  
**[Commit: [wasm] Fix undefined behavior in WebAssembly.Table.grow.](https://chromium.googlesource.com/v8/v8/+/158dbb8)**  
  
Date(Commit): Fri Oct 06 12:54:53 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "Test-Predator-Wrong-Components", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/704582](https://chromium-review.googlesource.com/704582)  
Regress: [mjsunit/regress/regress-crbug-772056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772056.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

var builder = new WasmModuleBuilder();
builder.addImportedTable("x", "table", 1, 10000000);
let module = new WebAssembly.Module(builder.toBuffer());
let table = new WebAssembly.Table({element: "anyfunc",
  initial: 1, maximum:1000000});
let instance = new WebAssembly.Instance(module, {x: {table:table}});

assertThrows(() => table.grow(Infinity), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/158dbb8^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=158dbb8)  
[test/mjsunit/regress/regress-crbug-772056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772056.js?cl=158dbb8)  
  

---   

## **regress-772190.js (chromium issue)**  
   
**[Issue 772190:
 Ill in v8::internal::Isolate::PushCodeObjectsAndDie](https://crbug.com/772190)**  
**[Commit: [turbofan] Don't try to constant-fold properties from the_hole.](https://chromium.googlesource.com/v8/v8/+/4824da8)**  
  
Date(Commit): Fri Oct 06 09:56:41 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/704578](https://chromium-review.googlesource.com/704578)  
Regress: [mjsunit/regress/regress-772190.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-772190.js)  
```javascript
assertThrows(function() {
  __v_13383[4];
  let __v_13383 = {};
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4824da8^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=4824da8)  
[test/mjsunit/regress/regress-772190.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-772190.js?cl=4824da8)  
  

---   

## **regress-crbug-771971.js (chromium issue)**  
   
**[Issue 771971:
 DCHECK failure in index < GetJSCallArity() in js-builtin-reducer.cc](https://crbug.com/771971)**  
**[Commit: [turbofan] Properly check call arity for Object.is(o,o).](https://chromium.googlesource.com/v8/v8/+/c77dfda)**  
  
Date(Commit): Fri Oct 06 07:56:31 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Code Review: [https://chromium-review.googlesource.com/702834](https://chromium-review.googlesource.com/702834)  
Regress: [mjsunit/regress/regress-crbug-771971.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-771971.js)  
```javascript
function f() { Object.is(); }

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c77dfda^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=c77dfda)  
[test/mjsunit/regress/regress-crbug-771971.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-771971.js?cl=c77dfda)  
  

---   

## **regress-crbug-768158.js (chromium issue)**  
   
**[Issue 768158:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/768158)**  
**[Commit: [parser] Ensure for-in/of loop variables are marked maybe_assigned](https://chromium.googlesource.com/v8/v8/+/0717ff3)**  
  
Date(Commit): Thu Oct 05 19:16:29 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-62", "v8-foozzie-failure", "M-63", "merge-merged-6.2", "merge-merged-62"]  
Code Review: [https://chromium-review.googlesource.com/701624](https://chromium-review.googlesource.com/701624)  
Regress: [mjsunit/regress/regress-crbug-768158.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-768158.js)  
```javascript
(function testOriginalRepro() {
  var result;
  var dict = { toString() { result = v;} };
  for (var v of ['fontsize', 'sup']) {
    String.prototype[v].call(dict);
    assertEquals(v, result);
  }
})();

(function testSimpler() {
  var result;
  function setResult() { result = v; }
  for (var v of ['hello', 'world']) {
    setResult();
    assertEquals(v, result);
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0717ff3^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=0717ff3)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=0717ff3)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=0717ff3)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=0717ff3)  
[test/mjsunit/regress/regress-crbug-768158.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-768158.js?cl=0717ff3)  
  

---   

## **regress-771470.js (chromium issue)**  
   
**[Issue 771470:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/771470)**  
**[Commit: [esnext] initialize native_context()->initial_async_generator_prototype](https://chromium.googlesource.com/v8/v8/+/f3fb1b7)**  
  
Date(Commit): Wed Oct 04 16:15:59 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/700261](https://chromium-review.googlesource.com/700261)  
Regress: [mjsunit/harmony/regress/regress-771470.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-771470.js)  
```javascript
async function* gen() { };
gen.prototype = 1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f3fb1b7^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=f3fb1b7)  
[test/mjsunit/harmony/regress/regress-771470.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-771470.js?cl=f3fb1b7)  
  

---   

## **regress-crbug-771428.js (chromium issue)**  
   
**[Issue 771428:
 Infinite loop in asm.js parser](https://crbug.com/771428)**  
**[Commit: [asm.js] Fix infinite loop in parser on parse error.](https://chromium.googlesource.com/v8/v8/+/4f8a70a)**  
  
Date(Commit): Wed Oct 04 16:13:39 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Regression  
Labels: ["Hotlist-Merge-Review", "NodeJS-Backport-Done", "M-61", "Via-Wizard-Javascript", "merge-merged-6.1", "merge-merged-6.2"]  
Code Review: [https://chromium-review.googlesource.com/699994](https://chromium-review.googlesource.com/699994)  
Regress: [mjsunit/regress/regress-crbug-771428.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-771428.js)  
```javascript
function Module() {
  "use asm";
  function f(i) {
    i = i | 0;
    switch (i | 0) {
      case 2:
        // Exceeds value range.
        i = 0x1ffffffff;
        break;
    }
    return i | 0;
  }
  return f;
}
var f = Module();
assertEquals(0, f(0));
assertEquals(1, f(1));
assertEquals(-1, f(2));
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f8a70a^!)  
[AUTHORS](https://cs.chromium.org/chromium/src/v8/AUTHORS?cl=4f8a70a)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=4f8a70a)  
[test/mjsunit/regress/regress-crbug-771428.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-771428.js?cl=4f8a70a)  
  

---   

## **regress-crbug-770543.js (chromium issue)**  
   
**[Issue 770543:
 Null-dereference READ in v8::internal::TranslatedFrame::begin](https://crbug.com/770543)**  
**[Commit: [deoptimizer] Fix TranslatedState inline frame indexing.](https://chromium.googlesource.com/v8/v8/+/631489b)**  
  
Date(Commit): Mon Oct 02 14:14:30 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/695123](https://chromium-review.googlesource.com/695123)  
Regress: [mjsunit/regress/regress-crbug-770543.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-770543.js)  
```javascript
(function FunctionCallerFromInlinedBuiltin() {
  function f() {
    function g() {
      Object.getOwnPropertyDescriptor(g, "caller");
    };
    [0].forEach(g);
  }
  f();
  f();
  %OptimizeFunctionOnNextCall(f);
  f();
})();

(function FunctionArgumentsFromInlinedBuiltin() {
  function g() {
    g.arguments;
  }
  function f() {
    [0].forEach(g);
  }
  f();
  f();
  %OptimizeFunctionOnNextCall(f);
  f();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/631489b^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=631489b)  
[test/mjsunit/regress/regress-crbug-770543.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-770543.js?cl=631489b)  
  

---   

## **regress-crbug-769852.js (chromium issue)**  
   
**[Issue 769852:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/769852)**  
**[Commit: [deoptimizer] Materialize objects with top-most stub frame.](https://chromium.googlesource.com/v8/v8/+/17d86d7)**  
  
Date(Commit): Mon Oct 02 13:23:45 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/691996](https://chromium-review.googlesource.com/691996)  
Regress: [mjsunit/regress/regress-crbug-769852.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-769852.js)  
```javascript
function f(o) {
  function g() {}
  Object.keys(o).forEach(suite => g());
}
assertDoesNotThrow(() => f({}));
assertDoesNotThrow(() => f({ x:0 }));
%OptimizeFunctionOnNextCall(f);
assertDoesNotThrow(() => f({ x:0 }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/17d86d7^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=17d86d7)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=17d86d7)  
[test/mjsunit/regress/regress-crbug-769852.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-769852.js?cl=17d86d7)  
  

---   

## **regress-769846.js (chromium issue)**  
   
**[Issue 769846:
 DCHECK failure in !IsThreadInWasm() in trap-handler.h](https://crbug.com/769846)**  
**[Commit: [wasm] set thread-in-wasm flag after converting arguments](https://chromium.googlesource.com/v8/v8/+/025e3ab)**  
  
Date(Commit): Sat Sep 30 01:07:08 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/693414](https://chromium-review.googlesource.com/693414)  
Regress: [mjsunit/regress/wasm/regress-769846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-769846.js)  
```javascript
function Module() {
  "use asm";
  function div_(__v_6) {
    __v_6 = __v_6 | 0;
  }
  return { f: div_}
};
var __f_0 = Module().f;
__v_8 = [0];
__v_8.__defineGetter__(0, function() { return __f_0(__v_8); });
__v_8[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/025e3ab^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=025e3ab)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=025e3ab)  
[test/mjsunit/regress/wasm/regression-769846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-769846.js?cl=025e3ab)  
  

---   

## **regress-769637.js (chromium issue)**  
   
**[Issue 769637:
 Crash in v8::internal::Invoke](https://crbug.com/769637)**  
**[Commit: [wasm] always allocate memory when guard regions are needed](https://chromium.googlesource.com/v8/v8/+/1f99c66)**  
  
Date(Commit): Fri Sep 29 20:24:04 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Needs-Feedback", "Security_Severity-Medium", "Security_Impact-Beta", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "M-63", "Merge-Rejected-62", "ClusterFuzz-Top-Crash", "Test-Predator-Auto-Components"]  
Code Review: [https://chromium-review.googlesource.com/693137](https://chromium-review.googlesource.com/693137)  
Regress: [mjsunit/regress/wasm/regress-769637.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-769637.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder
    .addMemory()
    .addFunction("main", kSig_v_v)
    .addBody([kExprI32Const, 4,
              kExprI32Const, 8,
              kExprI32StoreMem, 0, 16])
    .exportAs("main");
let instance = builder.instantiate();
assertTraps(kTrapMemOutOfBounds, instance.exports.main);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f99c66^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=1f99c66)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=1f99c66)  
[test/mjsunit/regress/wasm/regression-769637.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-769637.js?cl=1f99c66)  
  

---   

## **regress-crbug-768367.js (chromium issue)**  
   
**[Issue 768367:
 DCHECK failure in kMaxUInt32 != index_ in lookup.h](https://crbug.com/768367)**  
**[Commit: [turbofan] Fix off-by-one in constant-folding of frozen elements.](https://chromium.googlesource.com/v8/v8/+/adfaf74)**  
  
Date(Commit): Wed Sep 27 05:43:25 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Code Review: [https://chromium-review.googlesource.com/685240](https://chromium-review.googlesource.com/685240)  
Regress: [mjsunit/regress/regress-crbug-768367.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-768367.js)  
```javascript
const o = {};

function foo() { return o[4294967295]; }

assertEquals(undefined, foo());
assertEquals(undefined, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/adfaf74^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=adfaf74)  
[test/mjsunit/regress/regress-crbug-768367.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-768367.js?cl=adfaf74)  
  

---   

## **regress-6838-1.js (v8 issue)**  
   
**[Issue 6838:
 asm.js spec bugs: Math.min/max, -0, Math,abs](https://crbug.com/v8/6838)**  
**[Commit: [asm.js] Fix Math.min/max signatures to take signed.](https://chromium.googlesource.com/v8/v8/+/63f9ee1)**  
  
Date(Commit): Mon Sep 25 12:58:57 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/681357](https://chromium-review.googlesource.com/681357)  
Regress: [mjsunit/regress/regress-6838-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6838-1.js), [mjsunit/regress/regress-6838-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6838-2.js), [mjsunit/regress/regress-6838-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6838-3.js)  
```javascript
(function TestMathMaxOnLargeInt() {
  function Module(stdlib) {
    "use asm";
    var max = stdlib.Math.max;
    function f() {
      return max(42,0xffffffff);
    }
    return f;
  }
  var f = Module(this);
  assertEquals(0xffffffff, f());
  assertFalse(%IsAsmWasmCode(Module));
})();

(function TestMathMinOnLargeInt() {
  function Module(stdlib) {
    "use asm";
    var min = stdlib.Math.min;
    function f() {
      return min(42,0xffffffff);
    }
    return f;
  }
  var f = Module(this);
  assertEquals(42, f());
  assertFalse(%IsAsmWasmCode(Module));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63f9ee1^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=63f9ee1)  
[test/mjsunit/asm/math-max.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/math-max.js?cl=63f9ee1)  
[test/mjsunit/asm/math-min.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/math-min.js?cl=63f9ee1)  
[test/mjsunit/regress/regress-6838-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6838-1.js?cl=63f9ee1)  
  

---   

## **regress-763439.js (chromium issue)**  
   
**[Issue 763439:
 Null-dereference READ in v8::internal::Invoke](https://crbug.com/763439)**  
**[Commit: [wasm] Fix memory initialization on instantiate](https://chromium.googlesource.com/v8/v8/+/327df0b)**  
  
Date(Commit): Wed Sep 20 22:52:31 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-63", "merge-merged-6.2"]  
Code Review: [https://chromium-review.googlesource.com/674707](https://chromium-review.googlesource.com/674707)  
Regress: [mjsunit/regress/wasm/regress-763439.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-763439.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addMemory(0, 1234, false);
builder.addFunction('f', kSig_i_v)
    .addBody([
      kExprI32Const, 0x1d,                       // --
      kExprMemoryGrow, 0x00,                     // --
      kExprI32LoadMem, 0x00, 0xff, 0xff, 0x45,  // --
    ])
    .exportFunc();

var module = new WebAssembly.Module(builder.toBuffer());
var instance1 = new WebAssembly.Instance(module);
instance1.exports.f();
var instance2 = new WebAssembly.Instance(module);
instance2.exports.f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/327df0b^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=327df0b)  
[test/mjsunit/regress/wasm/regression-763439.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-763439.js?cl=327df0b)  
[test/mjsunit/wasm/import-memory.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/import-memory.js?cl=327df0b)  
  

---   

## **regress-crbug-763683.js (chromium issue)**  
   
**[Issue 763683:
 DCHECK failure in !__isolate__->has_pending_exception() in runtime-proxy.cc](https://crbug.com/763683)**  
**[Commit: Improve error handling of proxies get property](https://chromium.googlesource.com/v8/v8/+/8a568bd)**  
  
Date(Commit): Tue Sep 12 16:42:12 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.2"]  
Code Review: [https://chromium-review.googlesource.com/663218](https://chromium-review.googlesource.com/663218)  
Regress: [mjsunit/regress/regress-crbug-763683.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-763683.js)  
```javascript
assertThrows(function test() {
  var proxy = new Proxy({}, {});
  var key_or_proxy = 0;

  function failing_get() {
    return proxy[key_or_proxy];
  }

  failing_get();

  key_or_proxy = new Proxy({
    toString() {
      throw new TypeError('error');
    }
  }, {});

  failing_get();
}, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a568bd^!)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=8a568bd)  
[test/mjsunit/regress/regress-crbug-763683.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-763683.js?cl=8a568bd)  
  

---   

## **regress-763697.js (chromium issue)**  
   
**[Issue 763697:
 Ill in v8::internal::wasm::ThreadImpl::InitLocals](https://crbug.com/763697)**  
**[Commit: [wasm] Simd locals are not allowed without --experimental-wasm-simd](https://chromium.googlesource.com/v8/v8/+/07f93af)**  
  
Date(Commit): Mon Sep 11 13:09:30 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-63"]  
Code Review: [https://chromium-review.googlesource.com/660117](https://chromium-review.googlesource.com/660117)  
Regress: [mjsunit/regress/wasm/regress-763697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-763697.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
    builder.addFunction("main", kSig_i_i)
      .addBody([kExprGetLocal, 0])
      .addLocals({s128_count: 1});

  assertFalse(WebAssembly.validate(builder.toBuffer()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/07f93af^!)  
[src/wasm/function-body-decoder-impl.h](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder-impl.h?cl=07f93af)  
[test/mjsunit/regress/wasm/regression-648079.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-648079.js?cl=07f93af)  
[test/mjsunit/regress/wasm/regression-702460.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-702460.js?cl=07f93af)  
[test/mjsunit/regress/wasm/regression-763697.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-763697.js?cl=07f93af)  
[test/mjsunit/wasm/wasm-constants.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-constants.js?cl=07f93af)  
...  
  

---   

## **regress-crbug-762874-1.js (chromium issue)**  
   
**[Issue 762874:
 Security: off by one in TurboFan range optimization for String.indexOf](https://crbug.com/762874)**  
**[Commit: [turbofan] Fix type of String#indexOf and String#lastIndexOf.](https://chromium.googlesource.com/v8/v8/+/b8f144e)**  
  
Date(Commit): Mon Sep 11 12:05:29 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "NodeJS-Backport-Done", "merge-merged-6.1", "merge-merged-6.2", "Release-2-M61"]  
Code Review: [https://chromium-review.googlesource.com/660000](https://chromium-review.googlesource.com/660000)  
Regress: [mjsunit/regress/regress-crbug-762874-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-762874-1.js), [mjsunit/regress/regress-crbug-762874-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-762874-2.js)  
```javascript
const maxLength = %StringMaxLength();
const s = 'A'.repeat(maxLength);

function foo(s) {
  let x = s.indexOf("", maxLength);
  return x === maxLength;
}

assertTrue(foo(s));
assertTrue(foo(s));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8f144e^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=b8f144e)  
[test/mjsunit/regress/regress-crbug-762874-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-762874-1.js?cl=b8f144e)  
[test/mjsunit/regress/regress-crbug-762874-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-762874-2.js?cl=b8f144e)  
  

---   

## **regress-2435.js (v8 issue)**  
   
**[Issue 2435:
 String.fromcharCode.apply(undefined, uint8Array) is super-slow](https://crbug.com/v8/2435)**  
**[Commit: [builtins] Add fast-path for JSTypedArray to CreateListFromArrayLike.](https://chromium.googlesource.com/v8/v8/+/f31bae0)**  
  
Date(Commit): Mon Sep 11 06:21:58 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/657405](https://chromium-review.googlesource.com/657405)  
Regress: [mjsunit/regress/regress-2435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2435.js)  
```javascript
function arrayLikeToString(a) {
  return String.fromCharCode.apply(this, a);
}

const klasses = [
    Int8Array,
    Uint8Array,
    Uint8ClampedArray,
    Int16Array,
    Uint16Array,
    Int32Array,
    Uint32Array,
    Float32Array,
    Float64Array
];
const string = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';

for (const klass of klasses) {
  const array = klass.from(string, s => s.charCodeAt(0));
  assertEquals(string, arrayLikeToString(array));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f31bae0^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=f31bae0)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=f31bae0)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f31bae0)  
[test/mjsunit/regress/regress-2435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2435.js?cl=f31bae0)  
  

---   

## **regress-crbug-722871.js (chromium issue)**  
   
**[Issue 722871:
 Data race in v8::internal::ElementsAccessorBase<v8::internal::TypedElementsAccessor<](https://crbug.com/722871)**  
**[Commit: Add TSAN annotations for TypedArray accesses](https://chromium.googlesource.com/v8/v8/+/181c03e)**  
  
Date(Commit): Fri Sep 08 18:35:17 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-ThreadSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "M-63"]  
Code Review: [https://chromium-review.googlesource.com/656509](https://chromium-review.googlesource.com/656509)  
Regress: [mjsunit/regress/regress-crbug-722871.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-722871.js)  
```javascript
let sab = new SharedArrayBuffer(10 * 4);
let memory = new Int32Array(sab);
let workers = [];
let runningWorkers = 0;

function startWorker(script) {
  let worker = new Worker(script, {type: 'string'});
  worker.done = false;
  worker.idx = workers.length;
  workers.push(worker);
  worker.postMessage(memory);
  ++runningWorkers;
};

let shared = `
  function wait(memory, index, waitCondition, wakeCondition) {
    while (memory[index] == waitCondition) {
      var result = Atomics.wait(memory, index, waitCondition);
      switch (result) {
        case 'not-equal':
        case 'ok':
          break;
        default:
          postMessage('Error: bad result from wait: ' + result);
          break;
      }
      var value = memory[index];
      if (value != wakeCondition) {
        postMessage(
            'Error: wait returned not-equal but the memory has a bad value: ' +
            value);
      }
    }
    var value = memory[index];
    if (value != wakeCondition) {
      postMessage(
          'Error: done waiting but the memory has a bad value: ' + value);
    }
  }

  function wake(memory, index) {
    var result = Atomics.wake(memory, index, 1);
    if (result != 0 && result != 1) {
      postMessage('Error: bad result from wake: ' + result);
    }
  }
`;

let worker1 = startWorker(shared + `
  onmessage = function(msg) {
    let memory = msg;
    const didStartIdx = 0;
    const shouldGoIdx = 1;
    const didEndIdx = 2;

    postMessage("started");
    postMessage("memory: " + memory);
    wait(memory, didStartIdx, 0, 1);
    memory[shouldGoIdx] = 1;
    wake(memory, shouldGoIdx);
    wait(memory, didEndIdx, 0, 1);
    postMessage("memory: " + memory);
    postMessage("done");
  };
`);

let worker2 = startWorker(shared + `
  onmessage = function(msg) {
    let memory = msg;
    const didStartIdx = 0;
    const shouldGoIdx = 1;
    const didEndIdx = 2;

    postMessage("started");
    postMessage("memory: " + memory);
    Atomics.store(memory, didStartIdx, 1);
    wake(memory, didStartIdx);
    wait(memory, shouldGoIdx, 0, 1);
    Atomics.store(memory, didEndIdx, 1);
    wake(memory, didEndIdx, 1);
    postMessage("memory: " + memory);
    postMessage("done");
  };
`);

let running = true;
while (running) {
  for (let worker of workers) {
    if (worker.done) continue;

    let msg = worker.getMessage();
    if (msg) {
      switch (msg) {
        case "done":
          if (worker.done === false) {
            print("worker #" + worker.idx + " done.");
            worker.done = true;
            if (--runningWorkers === 0) {
              running = false;
            }
          }
          break;

        default:
          print("msg from worker #" + worker.idx + ": " + msg);
          break;
      }
    }
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/181c03e^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=181c03e)  
[src/base/tsan.h](https://cs.chromium.org/chromium/src/v8/src/base/tsan.h?cl=181c03e)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=181c03e)  
[src/v8.gyp](https://cs.chromium.org/chromium/src/v8/src/v8.gyp?cl=181c03e)  
[test/mjsunit/regress/regress-crbug-722871.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-722871.js?cl=181c03e)  
  

---   

## **regress-762057.js (chromium issue)**  
   
**[No Permission](https://crbug.com/762057)**  
**[Commit: Introduce an Abort bytecode and turbofan operator.](https://chromium.googlesource.com/v8/v8/+/6e8c00f)**  
  
Date(Commit): Fri Sep 08 12:16:23 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/657025](https://chromium-review.googlesource.com/657025)  
Regress: [mjsunit/compiler/regress-762057.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-762057.js)  
```javascript
function* foo() {
  yield;
  new Set();
  for (let x of []) {
    for (let y of []) {
      yield;
    }
  }
}

let gaga = foo();
gaga.next();
%OptimizeFunctionOnNextCall(foo);
gaga.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6e8c00f^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=6e8c00f)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=6e8c00f)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=6e8c00f)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=6e8c00f)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=6e8c00f)  
...  
  

---   

## **regress-761831.js (chromium issue)**  
   
**[Issue 761831:
 DCHECK failure in !already_resolved_ in scopes.cc](https://crbug.com/761831)**  
**[Commit: [parser] Fix arrow funcs w/ destructuring params again. [Alternative fix]](https://chromium.googlesource.com/v8/v8/+/138fbdb)**  
  
Date(Commit): Thu Sep 07 13:06:44 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.2", "merge-merged-62"]  
Code Review: [https://chromium-review.googlesource.com/650297](https://chromium-review.googlesource.com/650297)  
Regress: [mjsunit/regress/regress-761831.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-761831.js)  
```javascript
function OrigReproCase() {
  assertThrows('var f = ([x=[a=undefined]=[]]) => {}; f();', TypeError);
}
OrigReproCase();

function SimpleReproCase() {
  assertThrows('var f = ([x=[]=[]]) => {}; f()', TypeError);
}
SimpleReproCase();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/138fbdb^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=138fbdb)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=138fbdb)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=138fbdb)  
[test/mjsunit/regress/regress-761831.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-761831.js?cl=138fbdb)  
  

---   

## **regress-crbug-762472.js (chromium issue)**  
   
**[Issue 762472:
 DCHECK failure in !isolate->has_pending_exception() in asm-js.cc](https://crbug.com/762472)**  
**[Commit: [asm.js] Gracefully handle stack overflow in start function.](https://chromium.googlesource.com/v8/v8/+/54a3027)**  
  
Date(Commit): Wed Sep 06 15:03:13 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.2", "Release-0-M62"]  
Code Review: [https://chromium-review.googlesource.com/652478](https://chromium-review.googlesource.com/652478)  
Regress: [mjsunit/regress/regress-crbug-762472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-762472.js)  
```javascript
function Module() {
  "use asm";
  function f() {}
  return { f:f }
}

function InstantiateNearStackLimit() {
  try {
    var fuse = InstantiateNearStackLimit();
    if (fuse == 0) Module();
    return fuse - 1;
  } catch(e) {
    return init_fuse;
  }
}

var init_fuse = 0;
for (init_fuse = 0; init_fuse < 10; init_fuse++) {
  InstantiateNearStackLimit();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54a3027^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=54a3027)  
[test/mjsunit/regress/regress-crbug-762472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-762472.js?cl=54a3027)  
  

---   

## **regress-761892.js (chromium issue)**  
   
**[Issue 761892:
 Fatal error in ../../src/compiler/representation-change.cc](https://crbug.com/761892)**  
**[Commit: [turbofan] Fix truncation for number feedback.](https://chromium.googlesource.com/v8/v8/+/4bce250)**  
  
Date(Commit): Tue Sep 05 14:48:08 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["reward-0", "Arch-x86_64", "ClusterFuzz-Verified", "Via-Wizard-Security"]  
Code Review: [https://chromium-review.googlesource.com/649513](https://chromium-review.googlesource.com/649513)  
Regress: [mjsunit/compiler/regress-761892.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-761892.js)  
```javascript
function f(x) {
  var x0 = (0 != Math.min(1, 1)) && 1;
  1.1!=(x||x0)
}

f(1.1);
f(1.1);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4bce250^!)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=4bce250)  
[test/mjsunit/compiler/regress-761892.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-761892.js?cl=4bce250)  
  

---   

## **regress-761639.js (chromium issue)**  
   
**[Issue 761639:
 DCHECK failure in !receiver_map->IsJSGlobalObjectMap() in ic.cc](https://crbug.com/761639)**  
**[Commit: Remove unnecessary check in StoreProxy](https://chromium.googlesource.com/v8/v8/+/affdc80)**  
  
Date(Commit): Tue Sep 05 10:58:18 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-63"]  
Code Review: [https://chromium-review.googlesource.com/647550](https://chromium-review.googlesource.com/647550)  
Regress: [mjsunit/regress/regress-761639.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-761639.js)  
```javascript
for (var i = 0; i < 10; i++) {
  __proto__ = new Proxy({}, { getPrototypeOf() { } });
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/affdc80^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=affdc80)  
[test/mjsunit/regress/regress-761639.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-761639.js?cl=affdc80)  
  

---   

## **regress-758096.js (chromium issue)**  
   
**[Issue 758096:
 CHECK failure: Representation inference: unsupported opcode 59 (Dead), node #5 in simplified-lo](https://crbug.com/758096)**  
**[Commit: [turbofan] Reland^2 "Polymorphic inlining - try merge map check dispatch with function call dispatch."](https://chromium.googlesource.com/v8/v8/+/8cf4aaf)**  
  
Date(Commit): Tue Sep 05 07:32:16 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-63"]  
Code Review: [https://chromium-review.googlesource.com/628169](https://chromium-review.googlesource.com/628169)  
Regress: [mjsunit/compiler/regress-758096.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-758096.js)  
```javascript
(function() {
  var x = 1;
  x.__proto__.f = function() { return 1; }

  function g() {}
  g.prototype.f =  function() { return 3; };
  var y = new g();

  function f(obj) {
    return obj.f();
  }

  f(x);
  f(y);
  f(x);
  f(y);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(1, f(x));
  assertEquals(3, f(y));
})();

(function() {
  function f() { return 1; }
  function g() { return 2; }

  var global;

  function h(s) {
    var fg;
    var a = 0;
    if (s) {
      global = 0;
      a = 1;
      fg = f;
    } else {
      global = 1
      fg = g;
    }
    return fg() + a;
  }

  h(0);
  h(0);
  h(1);
  h(1);
  %OptimizeFunctionOnNextCall(h);
  assertEquals(2, h(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8cf4aaf^!)  
[src/compiler/js-inlining-heuristic.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining-heuristic.cc?cl=8cf4aaf)  
[src/compiler/js-inlining-heuristic.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining-heuristic.h?cl=8cf4aaf)  
[test/mjsunit/compiler/regress-758096.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-758096.js?cl=8cf4aaf)  
  

---   

## **regress-760790.js (chromium issue)**  
   
**[Issue 760790:
 Ill in v8::internal::__RT_impl_Runtime_AbortJS](https://crbug.com/760790)**  
**[Commit: [csa] Canonicalize empty elements in AllocateJSArray](https://chromium.googlesource.com/v8/v8/+/2859dba)**  
  
Date(Commit): Fri Sep 01 16:56:53 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/645959](https://chromium-review.googlesource.com/645959)  
Regress: [mjsunit/regress/regress-760790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-760790.js)  
```javascript
function g() {
  var a = Array(0);
  a[0]++;
}
g();
g();
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2859dba^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=2859dba)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=2859dba)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=2859dba)  
[test/mjsunit/regress/regress-760790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-760790.js?cl=2859dba)  
  

---   

## **regress-739902.js (chromium issue)**  
   
**[Issue 739902:
 VEYRON_* platform cannot view Scribd.com content after updating from M58 to M59](https://crbug.com/739902)**  
**[Commit: [turbofan] Fix arm backend matching of (x >>> 24) & 0xffff.](https://chromium.googlesource.com/v8/v8/+/b1c1228)**  
  
Date(Commit): Thu Aug 31 13:50:07 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Regression  
Labels: ["M-59", "M-60", "NodeJS-Backport-Done", "merge-merged-6.1", "merge-merged-6.2"]  
Code Review: [https://chromium-review.googlesource.com/645848](https://chromium-review.googlesource.com/645848)  
Regress: [mjsunit/compiler/regress-739902.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-739902.js)  
```javascript
(function() {
  function f(x) {
    return String.fromCharCode(x >>> 24);
  };

  var e = 0x41000001;

  f(e);
  %OptimizeFunctionOnNextCall(f);
  assertEquals("A", f(e));
})();

(function() {
  function f(x) {
    return (x >>> 24) & 0xffff;
  };

  f(1);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f(1));
  assertEquals(100, f((100 << 24) + 42));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1c1228^!)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=b1c1228)  
[test/mjsunit/compiler/regress-739902.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-739902.js?cl=b1c1228)  
[test/unittests/compiler/arm/instruction-selector-arm-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/arm/instruction-selector-arm-unittest.cc?cl=b1c1228)  
  

---   

## **regress-760268.js (chromium issue)**  
   
**[Issue 760268:
 DCHECK failure in __isolate__->has_scheduled_exception() in runtime-proxy.cc](https://crbug.com/760268)**  
**[Commit: Fix wrongly handled exception in CheckProxyHasTrap](https://chromium.googlesource.com/v8/v8/+/68eabce)**  
  
Date(Commit): Wed Aug 30 08:36:28 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/642798](https://chromium-review.googlesource.com/642798)  
Regress: [mjsunit/regress/regress-760268.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-760268.js)  
```javascript
var obj = this;
var handler = {
  has: function() { return false; }
}
var proxy = new Proxy(obj, handler);
Object.defineProperty(obj, "nonconf", {});
assertThrows("'nonconf' in proxy");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68eabce^!)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=68eabce)  
[test/mjsunit/regress/regress-760268.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-760268.js?cl=68eabce)  
  

---   

## **regress-758983.js (chromium issue)**  
   
**[Issue 758983:
 Pages that were working in 59 crash Chrome 60 (geogebra.org applets)](https://crbug.com/758983)**  
**[Commit: [turbofan] Retype ConvertTaggedHoleToUndefined in representation selection.](https://chromium.googlesource.com/v8/v8/+/a529f12)**  
  
Date(Commit): Tue Aug 29 08:56:07 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Regression  
Labels: ["Stability-Crash", "Arch-x86_64", "M-60", "Via-Wizard-Javascript"]  
Code Review: [https://chromium-review.googlesource.com/640382](https://chromium-review.googlesource.com/640382)  
Regress: [mjsunit/compiler/regress-758983.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-758983.js)  
```javascript
var holey = [ 2.2,,"x" ];

function f(b) {
  holey[0] = 1.1;
  var r = holey[0];
  r = b ? r : 0;
  return r < 0;
}

f(true);
f(true);
%OptimizeFunctionOnNextCall(f);
assertFalse(f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a529f12^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=a529f12)  
[src/compiler/operation-typer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.h?cl=a529f12)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=a529f12)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=a529f12)  
[test/mjsunit/compiler/regress-758983.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-758983.js?cl=a529f12)  
  

---   

## **regress-6708.js (v8 issue)**  
   
**[Issue 6708:
 Array.prototype.concat doesn't set new length](https://crbug.com/v8/6708)**  
**[Commit: [builtins] Array.prototype.concat should set length on return value](https://chromium.googlesource.com/v8/v8/+/fafc3d5)**  
  
Date(Commit): Mon Aug 28 18:02:48 2017  
Type: Bug  
Code Review: [https://tc39.github.io/ecma262/#sec-array.prototype.concat,](https://tc39.github.io/ecma262/#sec-array.prototype.concat,)  
Regress: [mjsunit/regress/regress-6708.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6708.js)  
```javascript
var a = [0, 1];
a.constructor = {
  [Symbol.species]: function(len) {
    var arr = Array(20);
    return arr;
  }
};
assertEquals([0, 1], Array.prototype.concat.call(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fafc3d5^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=fafc3d5)  
[test/mjsunit/es6/array-species.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/array-species.js?cl=fafc3d5)  
[test/mjsunit/regress/regress-6707.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6707.js?cl=fafc3d5)  
[test/mjsunit/regress/regress-6708.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6708.js?cl=fafc3d5)  
  

---   

## **regress-6707.js (v8 issue)**  
   
**[Issue 6707:
 Array.prototype.concat should throw TypeError when length property is non-writable](https://crbug.com/v8/6707)**  
**[Commit: [builtins] Array.prototype.concat should set length on return value](https://chromium.googlesource.com/v8/v8/+/fafc3d5)**  
  
Date(Commit): Mon Aug 28 18:02:48 2017  
Type: Bug  
Code Review: [https://tc39.github.io/ecma262/#sec-array.prototype.concat,](https://tc39.github.io/ecma262/#sec-array.prototype.concat,)  
Regress: [mjsunit/regress/regress-6707.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6707.js)  
```javascript
var a = [0, 1];
a.constructor = {
  [Symbol.species]: function(len) {
    var arr = {length: 0};
    Object.defineProperty(arr, "length", {writable: false});
    return arr;
  }
}
assertThrows(() => { Array.prototype.concat.call(a) }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fafc3d5^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=fafc3d5)  
[test/mjsunit/es6/array-species.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/array-species.js?cl=fafc3d5)  
[test/mjsunit/regress/regress-6707.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6707.js?cl=fafc3d5)  
[test/mjsunit/regress/regress-6708.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6708.js?cl=fafc3d5)  
  

---   

## **regress-crbug-759327.js (chromium issue)**  
   
**[Issue 759327:
 <no crash state available>](https://crbug.com/759327)**  
**[Commit: [asm.js] Correctly set minimum memory size to zero.](https://chromium.googlesource.com/v8/v8/+/89f839e)**  
  
Date(Commit): Mon Aug 28 15:01:30 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/637912](https://chromium-review.googlesource.com/637912)  
Regress: [mjsunit/regress/regress-crbug-759327.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-759327.js)  
```javascript
function Module(stdlib, env, heap) {
  "use asm";
  var MEM = new stdlib.Int32Array(heap);
  function f() {
    MEM[0] = 0;
  }
  return { f: f };
}
function instantiate() {
  var buffer = new ArrayBuffer(4096);
  Module(this, {}, buffer).f();
  try {} finally {}
  gc();
  Module(this, {}, buffer).f();
}
instantiate();
assertTrue(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89f839e^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=89f839e)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=89f839e)  
[src/wasm/module-decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.h?cl=89f839e)  
[src/wasm/wasm-module-builder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module-builder.cc?cl=89f839e)  
[src/wasm/wasm-module-builder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module-builder.h?cl=89f839e)  
...  
  

---   

## **regress-758763.js (chromium issue)**  
   
**[Issue 758763:
 Ill in v8::internal::__RT_impl_Runtime_AbortJS](https://crbug.com/758763)**  
**[Commit: [regexp] Pass correct limit to Runtime::kRegExpSplit](https://chromium.googlesource.com/v8/v8/+/c7a7bf6)**  
  
Date(Commit): Fri Aug 25 13:59:43 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/635588](https://chromium-review.googlesource.com/635588)  
Regress: [mjsunit/regress/regress-758763.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-758763.js)  
```javascript
const re = /./g;
function toSlowMode() { re.slow = true; }
re[Symbol.split]("abc", { valueOf: toSlowMode });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7a7bf6^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=c7a7bf6)  
[test/mjsunit/regress/regress-758763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-758763.js?cl=c7a7bf6)  
  

---   

## **regress-crbug-758773.js (chromium issue)**  
   
**[Issue 758773:
 DCHECK failure in result_map_->is_dictionary_map() in map-updater.cc](https://crbug.com/758773)**  
**[Commit: Don't look at abandoned prototype maps when looking for root maps](https://chromium.googlesource.com/v8/v8/+/8a7ce92)**  
  
Date(Commit): Fri Aug 25 10:44:29 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/635223](https://chromium-review.googlesource.com/635223)  
Regress: [mjsunit/regress/regress-crbug-758773.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-758773.js)  
```javascript
(0).__defineGetter__(0, function() { });
Number.prototype[0] = "string";  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a7ce92^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=8a7ce92)  
[test/mjsunit/regress/regress-crbug-757199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-757199.js?cl=8a7ce92)  
[test/mjsunit/regress/regress-crbug-758773.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-758773.js?cl=8a7ce92)  
  

---   

## **regress-crbug-754175.js (chromium issue)**  
   
**[Issue 754175:
 CHECK failure: memory->byte_length()->ToUint32(&mem_size) in module-compiler.cc](https://crbug.com/754175)**  
**[Commit: [asm.js] Fail gracefully on overly large buffers.](https://chromium.googlesource.com/v8/v8/+/8d2a8e0)**  
  
Date(Commit): Fri Aug 25 09:52:58 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "External-Fuzzer-Contribution", "reward-0", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/632618](https://chromium-review.googlesource.com/632618)  
Regress: [mjsunit/regress/regress-crbug-754175.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-754175.js)  
```javascript
function Module(stdlib, foreign, buffer) {
  "use asm";
  var heap = new stdlib.Int8Array(buffer);
  function foo() { return heap[23] | 0 }
  return { foo:foo };
}
function instantiate() {
  // On 32-bit architectures buffer allocation will throw.
  var buffer = new ArrayBuffer(0x100000000);
  // On 64-bit architectures instantiation will throw.
  var module = Module(this, {}, buffer);
}
assertThrows(instantiate, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d2a8e0^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=8d2a8e0)  
[test/mjsunit/regress/regress-crbug-754175.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-754175.js?cl=8d2a8e0)  
  

---   

## **regress-crbug-757199.js (chromium issue)**  
   
**[Issue 757199:
 DCHECK failure in result->owns_descriptors() in objects.cc](https://crbug.com/757199)**  
**[Commit: [runtime] Deprecate old prototype maps](https://chromium.googlesource.com/v8/v8/+/8974b75)**  
  
Date(Commit): Thu Aug 24 16:55:13 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62", "Release-0-M62"]  
Code Review: [https://chromium-review.googlesource.com/632317](https://chromium-review.googlesource.com/632317)  
Regress: [mjsunit/regress/regress-crbug-757199.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-757199.js)  
```javascript
var obj1 = {};
var obj2 = {};

function h() {}

h.prototype = obj2;

function g(v) {
  v.constructor;
}
function f() {
  g(obj1);
}

obj1.x = 0;
f();

obj1.__defineGetter__("x", function() {});

g(obj2);

obj2.y = 0;

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8974b75^!)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=8974b75)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8974b75)  
[test/mjsunit/regress/regress-crbug-757199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-757199.js?cl=8974b75)  
  

---   

## **regress-757217.js (chromium issue)**  
   
**[Issue 757217:
 DCHECK failure in !it.done() in module-compiler.cc](https://crbug.com/757217)**  
**[Commit: [wasm] Test and fix for module with no functions](https://chromium.googlesource.com/v8/v8/+/172d6f5)**  
  
Date(Commit): Thu Aug 24 00:10:52 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/630097](https://chromium-review.googlesource.com/630097)  
Regress: [mjsunit/regress/wasm/regress-757217.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-757217.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder.addImport('','f', kSig_v_v);
builder.addExport('a', 0);
builder.addExport('b', 0);
var bytes = builder.toBuffer();

var m = new WebAssembly.Module(bytes);
assertTrue(m instanceof WebAssembly.Module);

assertPromiseResult(
  WebAssembly.compile(bytes)
  .then(async_result => assertTrue(async_result instanceof WebAssembly.Module),
        assertUnreachable));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/172d6f5^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=172d6f5)  
[test/mjsunit/regress/wasm/regression-757217.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-757217.js?cl=172d6f5)  
  

---   

## **regress-6733.js (v8 issue)**  
   
**[Issue 6733:
 completion value of deleting let-variable seems inconsistent](https://crbug.com/v8/6733)**  
**[Commit: [ignition] Fix return value of delete on global lexical variables](https://chromium.googlesource.com/v8/v8/+/ac0a2df)**  
  
Date(Commit): Wed Aug 23 16:17:48 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/627636](https://chromium-review.googlesource.com/627636)  
Regress: [mjsunit/regress/regress-6733.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6733.js)  
```javascript
let x;
Realm.eval(Realm.current(), "let y");
assertFalse(delete x);
assertFalse(delete y);
assertFalse(eval("delete x"));
assertFalse(eval("delete y"));

(function() {
  let z;
  assertFalse(delete x);
  assertFalse(delete y);
  assertFalse(delete z);
  assertFalse(eval("delete x"));
  assertFalse(eval("delete y"));
  assertFalse(eval("delete z"));
})();

assertFalse(eval("let z; delete z"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac0a2df^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=ac0a2df)  
[test/cctest/interpreter/bytecode_expectations/GlobalDelete.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/GlobalDelete.golden?cl=ac0a2df)  
[test/mjsunit/regress/regress-6733.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6733.js?cl=ac0a2df)  
  

---   

## **regress-v8-6631.js (v8 issue)**  
   
**[Issue 6631:
 RepresentationChangerError: node #150:HeapConstant of kRepTaggedPointer (Boolean) cannot be changed to kRepWord32](https://crbug.com/v8/6631)**  
**[Commit: [turbofan] Work around lowering uninhabited ReferenceEqual.](https://chromium.googlesource.com/v8/v8/+/cf65162)**  
  
Date(Commit): Wed Aug 23 10:45:26 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/627916](https://chromium-review.googlesource.com/627916)  
Regress: [mjsunit/compiler/regress-v8-6631.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-6631.js)  
```javascript
function h(x) {
  return (x|0) && (1>>x == {})
}

function g() {
  return h(1)
};

function f() {
  return g(h({}))
};

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf65162^!)  
[src/compiler/new-escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/new-escape-analysis.cc?cl=cf65162)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=cf65162)  
[test/mjsunit/compiler/regress-v8-6631.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-v8-6631.js?cl=cf65162)  
  

---   

## **regress-v8-6706.js (v8 issue)**  
   
**[Issue 6706:
 RegExp.prototype[@@split] with sticky regexp not handled correctly](https://crbug.com/v8/6706)**  
**[Commit: [regexp] Send sticky @@splits to the slow path](https://chromium.googlesource.com/v8/v8/+/27fd52a)**  
  
Date(Commit): Wed Aug 23 09:55:21 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/626065](https://chromium-review.googlesource.com/626065)  
Regress: [mjsunit/regress/regress-v8-6706.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6706.js)  
```javascript
const str = "a-b-c";

function test(re) {
  assertArrayEquals(["a", "b", "c"], re[Symbol.split](str));
}

!function() {
  const re = /-/y;
  re.lastIndex = 1;
  test(re);
}();

!function() {
  const re = /-/y;
  re.lastIndex = 3;
  test(re);
}();

!function() {
  const re = /-/y;
  re.lastIndex = -1;
  test(re);
}();

!function() {
  const re = /-/y;
  test(re);
}();

!function() {
  const re = /-/g;
  test(re);
}();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/27fd52a^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=27fd52a)  
[test/mjsunit/regress/regress-v8-6706.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6706.js?cl=27fd52a)  
  

---   

## **regress-crbug-755044.js (chromium issue)**  
   
**[Issue 755044:
 DCHECK failure in AllowHeapAllocation::IsAllowed() in heap-inl.h](https://crbug.com/755044)**  
**[Commit: Handlify FrameFunctionIterator to allow for GCs.](https://chromium.googlesource.com/v8/v8/+/6dd1251)**  
  
Date(Commit): Tue Aug 22 15:00:03 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/625878](https://chromium-review.googlesource.com/625878)  
Regress: [mjsunit/regress/regress-crbug-755044.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-755044.js)  
```javascript
function foo(f){
  f.caller;
}
function bar(f) {
  new foo(f);
}
bar(function() {});
%OptimizeFunctionOnNextCall(bar);
bar(function() {});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6dd1251^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=6dd1251)  
[test/mjsunit/regress/regress-crbug-755044.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-755044.js?cl=6dd1251)  
  

---   

## **regress-6700.js (v8 issue)**  
   
**[Issue 6700:
 [asm.js] Validation failure for left-right-shift expression without parenthesis](https://crbug.com/v8/6700)**  
**[Commit: [asm.js] Fix heap access validation of shift expressions.](https://chromium.googlesource.com/v8/v8/+/313f8d3)**  
  
Date(Commit): Tue Aug 22 08:50:26 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/623811](https://chromium-review.googlesource.com/623811)  
Regress: [mjsunit/regress/regress-6700.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6700.js)  
```javascript
let kMinHeapSize = 4096;

(function TestLeftRight() {
  function Module(stdlib, foreign, heap) {
    "use asm";
    var HEAP32 = new stdlib.Int32Array(heap);
    function f(i) {
      i = i | 0;
      return HEAP32[i << 2 >> 2] | 0;
    }
    return { f:f }
  }
  var buffer = new ArrayBuffer(kMinHeapSize);
  var module = new Module(this, {}, buffer);
  assertTrue(%IsAsmWasmCode(Module));
  new Int32Array(buffer)[42] = 23;
  assertEquals(23, module.f(42));
})();

(function TestRightRight() {
  function Module(stdlib, foreign, heap) {
    "use asm";
    var HEAP32 = new stdlib.Int32Array(heap);
    function f(i) {
      i = i | 0;
      return HEAP32[i >> 2 >> 2] | 0;
    }
    return { f:f }
  }
  var buffer = new ArrayBuffer(kMinHeapSize);
  var module = new Module(this, {}, buffer)
  assertTrue(%IsAsmWasmCode(Module));
  new Int32Array(buffer)[42 >> 4] = 23;
  assertEquals(23, module.f(42));
})();

(function TestRightLeft() {
  function Module(stdlib, foreign, heap) {
    "use asm";
    var HEAP32 = new stdlib.Int32Array(heap);
    function f(i) {
      i = i | 0;
      return HEAP32[i >> 2 << 2] | 0;
    }
    return { f:f }
  }
  var buffer = new ArrayBuffer(kMinHeapSize);
  var module = new Module(this, {}, buffer)
  assertFalse(%IsAsmWasmCode(Module));
  new Int32Array(buffer)[42 & 0xfc] = 23;
  assertEquals(23, module.f(42));
})();

(function TestRightButNotImmediate() {
  function Module(stdlib, foreign, heap) {
    "use asm";
    var HEAP32 = new stdlib.Int32Array(heap);
    function f(i) {
      i = i | 0;
      return HEAP32[i >> 2 + 1] | 0;
    }
    return { f:f }
  }
  var buffer = new ArrayBuffer(kMinHeapSize);
  var module = new Module(this, {}, buffer)
  assertFalse(%IsAsmWasmCode(Module));
  new Int32Array(buffer)[42 >> 3] = 23;
  assertEquals(23, module.f(42));
})();

(function TestLeftOnly() {
  function Module(stdlib, foreign, heap) {
    "use asm";
    var HEAP32 = new stdlib.Int32Array(heap);
    function f(i) {
      i = i | 0;
      return HEAP32[i << 2] | 0;
    }
    return { f:f }
  }
  var buffer = new ArrayBuffer(kMinHeapSize);
  var module = new Module(this, {}, buffer)
  assertFalse(%IsAsmWasmCode(Module));
  new Int32Array(buffer)[42 << 2] = 23;
  assertEquals(23, module.f(42));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/313f8d3^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=313f8d3)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=313f8d3)  
[test/mjsunit/regress/regress-6700.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6700.js?cl=313f8d3)  
  

---   

## **regress-v8-6716.js (v8 issue)**  
   
**[Issue 6716:
 Check failed: size <= kMaxRegularHeapObjectSize](https://crbug.com/v8/6716)**  
**[Commit: [csa] Fix two cases where allocations could go into LO space](https://chromium.googlesource.com/v8/v8/+/1b5df68)**  
  
Date(Commit): Tue Aug 22 08:20:54 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/623808](https://chromium-review.googlesource.com/623808)  
Regress: [mjsunit/regress/regress-v8-6716.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6716.js)  
```javascript
function f() {}
var a = Array(2 ** 16);  // Elements in large-object-space.
f.bind(...a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1b5df68^!)  
[src/builtins/builtins-function-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function-gen.cc?cl=1b5df68)  
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=1b5df68)  
[test/mjsunit/regress/regress-v8-6716.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6716.js?cl=1b5df68)  
  

---   

## **regress-crbug-756332.js (chromium issue)**  
   
**[Issue 756332:
 DCHECK failure in !node->is_rewritten() in pattern-rewriter.cc](https://crbug.com/756332)**  
**[Commit: [pattern-rewriter] Handle already-rewritten RewritableExpressions as before](https://chromium.googlesource.com/v8/v8/+/cec289e)**  
  
Date(Commit): Fri Aug 18 16:16:24 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Needs-Feedback", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/619506](https://chromium-review.googlesource.com/619506)  
Regress: [mjsunit/regress/regress-crbug-756332.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-756332.js)  
```javascript
{
  let z = ({x: {y} = {y: 42}} = {}) => y;
  assertEquals(42, z());
}

{
  let z = ({x: [y] = [42]} = {}) => y;
  assertEquals(42, z());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cec289e^!)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=cec289e)  
[test/mjsunit/regress/regress-crbug-756332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-756332.js?cl=cec289e)  
  

---   

## **regress-756608.js (chromium issue)**  
   
**[Issue 756608:
 ProxyHasProperty stub crashes when trap is a Smi](https://crbug.com/756608)**  
**[Commit: [builtins] Fix crash in ProxyHasProperty stub](https://chromium.googlesource.com/v8/v8/+/03285ec)**  
  
Date(Commit): Fri Aug 18 13:56:18 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-3500", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "reward-inprocess", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/620647](https://chromium-review.googlesource.com/620647)  
Regress: [mjsunit/regress/regress-756608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-756608.js)  
```javascript
assertThrows(function() {
  'foo' in new Proxy({}, {has: 0});
}, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/03285ec^!)  
[src/builtins/builtins-proxy-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-gen.cc?cl=03285ec)  
[test/mjsunit/regress/regress-756608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-756608.js?cl=03285ec)  
  

---   

## **regress-v8-6712.js (v8 issue)**  
   
**[Issue 6712:
 Function.prototype.bind doesn't search prototype chain for "name" property](https://crbug.com/v8/6712)**  
**[Commit: Fix spec violation in Function.prototype.bind.](https://chromium.googlesource.com/v8/v8/+/d5a398e)**  
  
Date(Commit): Thu Aug 17 08:42:03 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/616727](https://chromium-review.googlesource.com/616727)  
Regress: [mjsunit/regress/regress-v8-6712.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-6712.js)  
```javascript
var log = [];

function f() {}
Object.defineProperty(Function.prototype, "name", {
  get() { log.push("getter"); return "ok"; }
});
delete f.name;
var b = f.bind();
assertEquals("bound ok", b.name);
assertEquals("bound ok", b.name);
assertEquals("bound ok", b.name);
assertEquals(["getter"], log);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5a398e^!)  
[src/builtins/builtins-function.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function.cc?cl=d5a398e)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=d5a398e)  
[src/lookup.h](https://cs.chromium.org/chromium/src/v8/src/lookup.h?cl=d5a398e)  
[test/mjsunit/regress/regress-v8-6712.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6712.js?cl=d5a398e)  
  

---   

## **regress-746909.js (chromium issue)**  
   
**[Issue 746909:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsString()) in string-inl.h](https://crbug.com/746909)**  
**[Commit: [modules] Fix dynamic import in eval](https://chromium.googlesource.com/v8/v8/+/f6e20fc)**  
  
Date(Commit): Mon Aug 14 23:21:49 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Low", "Security_Impact-None", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/606701](https://chromium-review.googlesource.com/606701)  
Regress: [mjsunit/regress/regress-746909.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-746909.js)  
```javascript
eval(`import('modules-skip-2.js');`);
eval(`eval(import('modules-skip-2.js'));`);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6e20fc^!)  
[src/runtime/runtime-module.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-module.cc?cl=f6e20fc)  
[test/mjsunit/harmony/modules-import-16.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/modules-import-16.js?cl=f6e20fc)  
[test/mjsunit/regress/regress-746909.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-746909.js?cl=f6e20fc)  
  

---   

## **regress-crbug-754177.js (chromium issue)**  
   
**[Issue 754177:
 CHECK failure: args[0]->IsJSFunction() in runtime-test.cc](https://crbug.com/754177)**  
**[Commit: [tests] Make %NeverOptimizeFunction ClusterFuzz safe](https://chromium.googlesource.com/v8/v8/+/89e5792)**  
  
Date(Commit): Fri Aug 11 14:56:45 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/610706](https://chromium-review.googlesource.com/610706)  
Regress: [mjsunit/regress/regress-crbug-754177.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-754177.js)  
```javascript
%NeverOptimizeFunction(undefined);
%NeverOptimizeFunction(true);
%NeverOptimizeFunction(1);
%NeverOptimizeFunction({});
assertThrows("%NeverOptimizeFunction()", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89e5792^!)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=89e5792)  
[test/mjsunit/regress/regress-crbug-754177.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-754177.js?cl=89e5792)  
  

---   

## **regress-753496.js (chromium issue)**  
   
**[Issue 753496:
 Null-dereference WRITE in v8::internal::Invoke](https://crbug.com/753496)**  
**[Commit: [wasm] Correctly reconstitute ModuleEnv from runtime data](https://chromium.googlesource.com/v8/v8/+/1ca0eea)**  
  
Date(Commit): Thu Aug 10 14:52:50 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/609376](https://chromium-review.googlesource.com/609376)  
Regress: [mjsunit/regress/wasm/regress-753496.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-753496.js)  
```javascript
function Module(stdlib, foreign, heap) {
  "use asm";
  var MEM64 = new stdlib.Float64Array(heap);
  function foo(i) {
    i = i | 0;
    MEM64[032 ] = +(i >>> 7 ) / 2.;
    return +MEM64[0];
  }
  return { foo: foo };
}

var foo = Module(this, {}, new ArrayBuffer( "" ? this : this)).foo;
assertEquals(NaN, foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ca0eea^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=1ca0eea)  
[test/mjsunit/regress/wasm/regression-753496.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-753496.js?cl=1ca0eea)  
  

---   

## **regress-751789.js (chromium issue)**  
   
**[Issue 751789:
 DCHECK failure in !is_async_function() in parser-base.h](https://crbug.com/751789)**  
**[Commit: [parser] Check if async function before throwing error](https://chromium.googlesource.com/v8/v8/+/58bbc6b)**  
  
Date(Commit): Wed Aug 09 23:43:52 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/607356](https://chromium-review.googlesource.com/607356)  
Regress: [mjsunit/regress/regress-751789.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-751789.js)  
```javascript
assertThrows(() => eval('async A=>{s.await i}'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58bbc6b^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=58bbc6b)  
[test/mjsunit/regress/regress-751789.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-751789.js?cl=58bbc6b)  
  

---   

## **regress-752764.js (chromium issue)**  
   
**[Issue 752764:
 DCHECK failure in size <= SeqOneByteString::kMaxSize in heap.cc](https://crbug.com/752764)**  
**[Commit: [runtime] Align Seq{One,Two}ByteString::kMaxSize.](https://chromium.googlesource.com/v8/v8/+/06f5f84)**  
  
Date(Commit): Wed Aug 09 14:48:54 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Low", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/607935](https://chromium-review.googlesource.com/607935)  
Regress: [mjsunit/regress/regress-752764.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-752764.js)  
```javascript
a = "a".repeat(%StringMaxLength() - 3);
assertThrows(() => new RegExp("a" + a), SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/06f5f84^!)  
[src/objects/string.h](https://cs.chromium.org/chromium/src/v8/src/objects/string.h?cl=06f5f84)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=06f5f84)  
[test/mjsunit/regress/regress-752764.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-752764.js?cl=06f5f84)  
  

---   

## **regress-752423.js (chromium issue)**  
   
**[Issue 752423:
 [wasm] OOB access in v8 wasm after Symbol.toPrimitive overwrite](https://crbug.com/752423)**  
**[Commit: [wasm] Fix patching of table sizes.](https://chromium.googlesource.com/v8/v8/+/f6d5504)**  
  
Date(Commit): Wed Aug 09 14:44:33 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["reward-3000", "Security_Impact-Stable", "Arch-x86_64", "Security_Severity-High", "allpublic", "reward-inprocess", "Via-Wizard-Security", "M-62", "merge-merged-6.1", "Release-2-M61", "CVE-2017-5122", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/606587](https://chromium-review.googlesource.com/606587)  
Regress: [mjsunit/regress/wasm/regress-752423.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-752423.js)  
```javascript
'use strict';

load("test/mjsunit/wasm/wasm-module-builder.js");

var builder = new WasmModuleBuilder();
builder.addImportedTable("x", "table", 1, 10000000);
builder.addFunction("main", kSig_i_i)
  .addBody([
    kExprI32Const, 0,
    kExprGetLocal, 0,
    kExprCallIndirect, 0, kTableZero])
  .exportAs("main");
let module = new WebAssembly.Module(builder.toBuffer());
let table = new WebAssembly.Table({element: "anyfunc",
  initial: 1, maximum:1000000});
let instance = new WebAssembly.Instance(module, {x: {table:table}});

table.grow(0x40001);

let instance2 = new WebAssembly.Instance(module, {x: {table:table}});

try {
  instance2.exports.main(402982); // should be OOB
} catch (e) {
  print("Correctly caught: ", e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6d5504^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=f6d5504)  
[test/mjsunit/regress/wasm/regress-752423.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-752423.js?cl=f6d5504)  
  

---   

## **regress-6681.js (v8 issue)**  
   
**[Issue 6681:
 ValidateAndApplyPropertyDescriptor does not always honor its ShouldThrow mode](https://crbug.com/v8/6681)**  
**[Commit: Make ValidateAndApplyPropertyDescriptor pass on its ShouldThrow mode.](https://chromium.googlesource.com/v8/v8/+/b7227dc)**  
  
Date(Commit): Wed Aug 09 09:01:39 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/606027](https://chromium-review.googlesource.com/606027)  
Regress: [mjsunit/regress/regress-6681.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6681.js)  
```javascript
import * as ns from "./regress-6681.js";
export var foo;

assertEquals(false, Reflect.defineProperty(ns, 'foo', {value: 123}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b7227dc^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b7227dc)  
[test/mjsunit/regress/regress-6681.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6681.js?cl=b7227dc)  
  

---   

## **regress-crbug-752846.js (chromium issue)**  
   
**[Issue 752846:
 CHECK failure: args[2]->IsJSReceiver() in runtime-proxy.cc](https://crbug.com/752846)**  
**[Commit: Reland^2 "[builtins] Port getting property from Proxy to CSA"](https://chromium.googlesource.com/v8/v8/+/e86c066)**  
  
Date(Commit): Wed Aug 09 07:59:48 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/603796](https://chromium-review.googlesource.com/603796)  
Regress: [mjsunit/regress/regress-crbug-752846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-752846.js)  
```javascript
var values = [
  10,
  false,
  "test"
];

for (let val of values) {
  var proto = Object.getPrototypeOf(val);

  var proxy = new Proxy({}, {});
  Object.setPrototypeOf(proto, proxy);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e86c066^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=e86c066)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=e86c066)  
[src/builtins/builtins-proxy-helpers-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-helpers-gen.cc?cl=e86c066)  
[src/builtins/builtins-proxy-helpers-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-helpers-gen.h?cl=e86c066)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=e86c066)  
...  
  

---   

## **regress-crbug-752712.js (chromium issue)**  
   
**[Issue 752712:
 Crash in v8::internal::Invoke](https://crbug.com/752712)**  
**[Commit: Reland^2 "[builtins] Port getting property from Proxy to CSA"](https://chromium.googlesource.com/v8/v8/+/e86c066)**  
  
Date(Commit): Wed Aug 09 07:59:48 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/603796](https://chromium-review.googlesource.com/603796)  
Regress: [mjsunit/regress/regress-crbug-752712.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-752712.js)  
```javascript
var number = 1;

(function testFailingInvariant() {
  var obj = {};
  var handler = {
    get: function() {}
  };
  var proxy = new Proxy(obj, handler);
  Object.defineProperty(handler, 'get', {
    get: function() {
      return number;
    }
  });
  assertThrows(function(){ proxy.property; }, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e86c066^!)  
[BUILD.gn](https://cs.chromium.org/chromium/src/v8/BUILD.gn?cl=e86c066)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=e86c066)  
[src/builtins/builtins-proxy-helpers-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-helpers-gen.cc?cl=e86c066)  
[src/builtins/builtins-proxy-helpers-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-proxy-helpers-gen.h?cl=e86c066)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=e86c066)  
...  
  

---   

## **regress-crbug-752826.js (chromium issue)**  
   
**[Issue 752826:
 Fatal error: Tried to combine incompatible truncations](https://crbug.com/752826)**  
**[Commit: [turbofan] Fix introduction of contradicting {TypeGuard}.](https://chromium.googlesource.com/v8/v8/+/d929cc7)**  
  
Date(Commit): Tue Aug 08 11:54:51 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/605547](https://chromium-review.googlesource.com/605547)  
Regress: [mjsunit/regress/regress-crbug-752826.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-752826.js)  
```javascript
function h(a) {
  return a[1];
}
assertEquals(0, h(new Int8Array(10)));
assertEquals(0, h(new Int8Array(10)));

function g() {
  return h(arguments);
}
function f() {
  return g(23, 42);
}
assertEquals(42, f());
assertEquals(42, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d929cc7^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=d929cc7)  
[test/mjsunit/regress/regress-crbug-752826.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752826.js?cl=d929cc7)  
[test/unittests/compiler/load-elimination-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/load-elimination-unittest.cc?cl=d929cc7)  
  

---   

## **regress-6677.js (v8 issue)**  
   
**[Issue 6677:
 Writing to `const`-declared variable does not throw when passing through a `with` scope](https://crbug.com/v8/6677)**  
**[Commit: Throw errors when assigning to const variables inside `with`](https://chromium.googlesource.com/v8/v8/+/a9846ad)**  
  
Date(Commit): Tue Aug 08 02:00:22 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/602690](https://chromium-review.googlesource.com/602690)  
Regress: [mjsunit/regress/regress-6677.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6677.js)  
```javascript
const x = 0;
assertThrows(() => { with ({}) { x = 1; } }, TypeError);
assertEquals(0, x);

assertThrows(() => { with ({}) { eval("x = 1"); } }, TypeError);
assertEquals(0, x);

assertEquals('function', function f() {
  with ({}) { f = 1 }
  return typeof f;
}());

assertEquals('function', function f() {
  with ({}) {
    assertThrows(function() { "use strict"; f = 1 }, TypeError);
  }
  return typeof f;
}());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9846ad^!)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=a9846ad)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=a9846ad)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=a9846ad)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=a9846ad)  
[test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden?cl=a9846ad)  
...  
  

---   

## **regress-crbug-752481.js (chromium issue)**  
   
**[Issue 752481:
 CHECK failure: args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/752481)**  
**[Commit: [turbofan] Properly check new.target parameter in inlined Reflect.construct.](https://chromium.googlesource.com/v8/v8/+/cb9402a)**  
  
Date(Commit): Mon Aug 07 18:15:30 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/604187](https://chromium-review.googlesource.com/604187)  
Regress: [mjsunit/regress/regress-crbug-752481.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-752481.js)  
```javascript
const A = class A {}

function test(foo) {
  assertThrows(foo);
  assertThrows(foo);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo);
}

test((...args) => Reflect.construct(A, args, 0));
test((...args) => Reflect.construct(A, args, true));
test((...args) => Reflect.construct(A, args, false));
test((...args) => Reflect.construct(A, args, ""));
test((...args) => Reflect.construct(A, args, null));
test((...args) => Reflect.construct(A, args, undefined));
test((...args) => Reflect.construct(A, args, Symbol.species));

test(function() { Reflect.construct(A, arguments, 0); });
test(function() { Reflect.construct(A, arguments, true); });
test(function() { Reflect.construct(A, arguments, false); });
test(function() { Reflect.construct(A, arguments, ""); });
test(function() { Reflect.construct(A, arguments, null); });
test(function() { Reflect.construct(A, arguments, undefined); });
test(function() { Reflect.construct(A, arguments, Symbol.species); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb9402a^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=cb9402a)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=cb9402a)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=cb9402a)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=cb9402a)  
[src/builtins/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.h?cl=cb9402a)  
...  
  

---   

## **regress-6657.js (v8 issue)**  
   
**[Issue 6657:
 Incorrect realization of Array.prototype.filter in latest Chrome v8](https://crbug.com/v8/6657)**  
**[Commit: [builtins] Fix missing check in Array.prototype.filter.](https://chromium.googlesource.com/v8/v8/+/b329b24)**  
  
Date(Commit): Fri Aug 04 08:55:15 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/600467](https://chromium-review.googlesource.com/600467)  
Regress: [mjsunit/regress/regress-6657.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6657.js)  
```javascript
(function TestArrayNonEmptySpecies() {
  class MyArray extends Array {
    constructor() { return [1, 2, 3]; }
  }
  var a = [5, 4];
  a.__proto__ = MyArray.prototype;
  var o = a.filter(() => true);
  assertEquals([5, 4, 3], o);
  assertEquals(3, o.length);
})();

(function TestArrayLeakingSpeciesInsertInCallback() {
  var my_array = [];
  class MyArray extends Array {
    constructor() { return my_array; }
  }
  var a = [5, 4];
  a.__proto__ = MyArray.prototype;
  var o = a.filter(() => (my_array[2] = 3, true));
  assertEquals([5, 4, 3], o);
  assertEquals(3, o.length);
})();

(function TestArrayLeakingSpeciesRemoveInCallback() {
  var my_array = [];
  class MyArray extends Array {
    constructor() { return my_array; }
  }
  var a = [5, 4, 3, 2, 1];
  a.__proto__ = MyArray.prototype;
  var o = a.filter(() => (my_array.length = 0, true));
  assertEquals([,,,,1], o);
  assertEquals(5, o.length);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b329b24^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=b329b24)  
[test/mjsunit/regress/regress-6657.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6657.js?cl=b329b24)  
  

---   

## **regress-crbug-751715.js (chromium issue)**  
   
**[Issue 751715:
 DCHECK failure in !is_constructor in frames.cc](https://crbug.com/751715)**  
**[Commit: [deoptimizer] Fix bogus DCHECK in OptimizedFrame::Summarize.](https://chromium.googlesource.com/v8/v8/+/c8ee3fb)**  
  
Date(Commit): Thu Aug 03 07:34:42 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/598227](https://chromium-review.googlesource.com/598227)  
Regress: [mjsunit/regress/regress-crbug-751715.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-751715.js)  
```javascript
class Base {}
class Derived extends Base {
  constructor() { super(); }
}
var proxy = new Proxy(Base, { get() {} });
assertDoesNotThrow(() => Reflect.construct(Derived, []));
assertThrows(() => Reflect.construct(Derived, [], proxy), TypeError);
assertThrows(() => Reflect.construct(Derived, [], proxy), TypeError);
%OptimizeFunctionOnNextCall(Derived);
assertThrows(() => Reflect.construct(Derived, [], proxy), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8ee3fb^!)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=c8ee3fb)  
[test/mjsunit/regress/regress-crbug-751715.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-751715.js?cl=c8ee3fb)  
  

---   

## **regress-crbug-751109.js (chromium issue)**  
   
**[Issue 751109:
 CHECK failure: !descriptors->GetKey(i)->IsInterestingSymbol() in objects-debug.cc](https://crbug.com/751109)**  
**[Commit: [runtime] Properly forward the "interesting symbol" bit.](https://chromium.googlesource.com/v8/v8/+/7101248)**  
  
Date(Commit): Wed Aug 02 11:08:38 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/597669](https://chromium-review.googlesource.com/597669)  
Regress: [mjsunit/regress/regress-crbug-751109.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-751109.js)  
```javascript
(new constructor)[0] = null;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7101248^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7101248)  
[test/mjsunit/regress/regress-crbug-751109.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-751109.js?cl=7101248)  
  

---   

## **regress-crbug-747062.js (chromium issue)**  
   
**[Issue 747062:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/747062)**  
**[Commit: [turbofan] Fix missing callability check on Array callbacks](https://chromium.googlesource.com/v8/v8/+/44f88dc)**  
  
Date(Commit): Thu Jul 27 14:49:58 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/588893](https://chromium-review.googlesource.com/588893)  
Regress: [mjsunit/regress/regress-crbug-747062.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-747062.js)  
```javascript
(function TestNonCallableForEach() {
  function foo() { [].forEach(undefined) }
  assertThrows(foo, TypeError);
  assertThrows(foo, TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo, TypeError);
})();

(function TestNonCallableForEachCaught() {
  function foo() { try { [].forEach(undefined) } catch(e) { return e } }
  assertInstanceof(foo(), TypeError);
  assertInstanceof(foo(), TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertInstanceof(foo(), TypeError);
})();

(function TestNonCallableMap() {
  function foo() { [].map(undefined); }
  assertThrows(foo, TypeError);
  assertThrows(foo, TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo, TypeError);
})();

(function TestNonCallableMapCaught() {
  function foo() { try { [].map(undefined) } catch(e) { return e } }
  assertInstanceof(foo(), TypeError);
  assertInstanceof(foo(), TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertInstanceof(foo(), TypeError);
})();

(function TestNonCallableFilter() {
  function foo() { [].filter(undefined); }
  assertThrows(foo, TypeError);
  assertThrows(foo, TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo, TypeError);
})();

(function TestNonCallableFilterCaught() {
  function foo() { try { [].filter(undefined) } catch(e) { return e } }
  assertInstanceof(foo(), TypeError);
  assertInstanceof(foo(), TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertInstanceof(foo(), TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/44f88dc^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=44f88dc)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=44f88dc)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=44f88dc)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=44f88dc)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=44f88dc)  
...  
  

---   

## **regress-crbug-748539.js (chromium issue)**  
   
**[Issue 748539:
 CHECK failure: is_transitionable_fast_elements_kind implies !Map::IsInplaceGeneralizableField(d](https://crbug.com/748539)**  
**[Commit: [runtime] Don't create class field types for arrays' fields.](https://chromium.googlesource.com/v8/v8/+/10e4fe3)**  
  
Date(Commit): Thu Jul 27 07:11:05 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "M-62", "merge-merged-6.0", "merge-merged-6.1"]  
Code Review: [https://chromium-review.googlesource.com/586709](https://chromium-review.googlesource.com/586709)  
Regress: [mjsunit/regress/regress-crbug-748539.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-748539.js)  
```javascript
function f1() {}
function f2() {}

var o1 = [];
o1.a = 0;
o1.f = f1;
%HeapObjectVerify(o1);

var o2 = [];
o2.a = 4.2;
o2.f = f2;
%HeapObjectVerify(o2);

o1.a;
%HeapObjectVerify(o1);
%HeapObjectVerify(o2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10e4fe3^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=10e4fe3)  
[src/map-updater.h](https://cs.chromium.org/chromium/src/v8/src/map-updater.h?cl=10e4fe3)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=10e4fe3)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=10e4fe3)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=10e4fe3)  
...  
  

---   

## **regress-748069.js (chromium issue)**  
   
**[Issue 748069:
 Crash in Append](https://crbug.com/748069)**  
**[Commit: [runtime] Check for overflow when serializing Strings for JSON.](https://chromium.googlesource.com/v8/v8/+/8315422)**  
  
Date(Commit): Wed Jul 26 11:40:56 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Code Review: [https://chromium-review.googlesource.com/584611](https://chromium-review.googlesource.com/584611)  
Regress: [mjsunit/regress/regress-748069.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-748069.js)  
```javascript
try {
  var a = 'a'.repeat(1 << 28);
} catch (e) {
  // If the allocation fails, we don't care, because we can't cause the
  // overflow.
}
JSON.stringify(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8315422^!)  
[src/json-stringifier.cc](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.cc?cl=8315422)  
[src/string-builder.h](https://cs.chromium.org/chromium/src/v8/src/string-builder.h?cl=8315422)  
[test/mjsunit/regress/regress-748069.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-748069.js?cl=8315422)  
  

---   

## **regress-crbug-740591.js (chromium issue)**  
   
**[Issue 740591:
 Function expressions in initializers of for-of/in loops are incorrectly scoped](https://crbug.com/740591)**  
**[Commit: Rewrite scopes of initializers in for-in/of destructured declarations](https://chromium.googlesource.com/v8/v8/+/f1f2285)**  
  
Date(Commit): Tue Jul 25 18:26:16 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-61", "merge-merged-6.1", "Merge-Merged-61"]  
Code Review: [https://chromium-review.googlesource.com/583531](https://chromium-review.googlesource.com/583531)  
Regress: [mjsunit/regress/regress-crbug-740591.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-740591.js)  
```javascript
(function regressionCaseOne() {
  var c;
  for (let [a, b = c = function() { return a + b }] of [[0]]) {
    function f() { return a };
  }
  c();
})();

(function testForInFunction() {
  for (const {length: a, b = function() { return a, b }} in {foo: 42}) {
    assertSame(b, (function() { return b() })());
  }
})();

(function testForOfFunction() {
  for (const [a, b = function() { return a, b }] of [[42]]) {
    assertSame(b, (function() { return b() })());
  }
})();

(function testForInVariableProxy() {
  for (const {length: a, b = a} in {foo: 42}) {
    assertEquals(3, a);
    assertEquals(a, b);
  }
})();

(function testForOfVariableProxy() {
  for (const [a, b = a] of [[42]]) {
    assertEquals(42, a);
    assertEquals(a, b);
  }
})();

(function testClassLiteral() {
  for (let { a, b = class c { static f() { return a, b } } } of [{}]) {
    assertSame(b, (function() { return b.f() })());
  }
})();

(function testClassLiteralMethod() {
  for (let { a, b = class c { m() { return c } } } of [{}]) {
    assertSame(b, (function() { return (new b).m() })());
  }
})();

(function testClassLiteralComputedName() {
  let d;
  for (let { a, b = class c { [d = function() { return c }]() { } } } of [{}]) {
    assertSame(b, (function() { return b, d() })());
  }
})();

(function testClassLiteralComputedName() {
  let d;
  for (let { a, b = class c extends (d = function() { return c }, Object) { } } of [{}]) {
    assertSame(b, (function() { return b, d() })());
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1f2285^!)  
[src/parsing/parameter-initializer-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parameter-initializer-rewriter.cc?cl=f1f2285)  
[src/parsing/parameter-initializer-rewriter.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parameter-initializer-rewriter.h?cl=f1f2285)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=f1f2285)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=f1f2285)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=f1f2285)  
...  
  

---   

## **regress-crbug-746835.js (chromium issue)**  
   
**[Issue 746835:
 Crash in v8::internal::Heap::MergeAllocationSitePretenuringFeedback](https://crbug.com/746835)**  
**[Commit: [literals] Introduce CreateEmptyArrayLiteral Bytecode](https://chromium.googlesource.com/v8/v8/+/0392eb2)**  
  
Date(Commit): Tue Jul 25 14:30:43 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Fracas", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-61", "FoundIn-M-61"]  
Code Review: [https://chromium-review.googlesource.com/580932](https://chromium-review.googlesource.com/580932)  
Regress: [mjsunit/regress/regress-crbug-746835.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-746835.js)  
```javascript
function __getProperties() {
    return [];
    let properties = [];
    for (let name of Object.getOwnPropertyNames()) {;
    }
    return properties;
}

function __getRandomProperty() {
    let properties = __getProperties();
    if (!properties.length)
        return undefined;
    return properties[seed % properties.length];
}
var kWasmH0 = 0;
var kWasmH1 = 0x61;
var kWasmH2 = 0x73;
var kWasmH3 = 0x6d;
var kWasmV0 = 0x1;
var kWasmV1 = 0;
var kWasmV2 = 0;
var kWasmV3 = 0;
class Binary extends Array {
    emit_header() {
        this.push(kWasmH0, kWasmH1, kWasmH2, kWasmH3,
            kWasmV0, kWasmV1, kWasmV2, kWasmV3);
    }
}
class WasmModuleBuilder {
    constructor() {
        this.exports = [];
    }
    addImportedMemory() {}
    setFunctionTableLength() {}
    toArray() {
        let binary = new Binary;
        let wasm = this;
        binary.emit_header();
        "emitting imports @ " + binary.length;
        section => {};
        var mem_export = (wasm.memory !== undefined && wasm.memory.exp);
        var exports_count = wasm.exports.length + (mem_export ? 1 : 0);
        return binary;
    }
    toBuffer() {
        let bytes = this.toArray();
        let buffer = new ArrayBuffer(bytes.length);
        let view = new Uint8Array(buffer);
        for (let i = 0; i < bytes.length; i++) {
            let val = bytes[i];
            view[i] = val | 0;
        }
        return buffer;
    }
    instantiate(ffi) {
        let module = new WebAssembly.Module(this.toBuffer());
        let instance = new WebAssembly.Instance(module);
    }
}

var v_40 = 0;
var v_43 = NaN;

try {
    v_23 = new WasmModuleBuilder();
} catch (e) {
    print("Caught: " + e);
}

try {
    v_31 = [0xff];
    v_29 = [v_31];
} catch (e) {
    print("Caught: " + e);
}

try {
    v_25 = ["main"];
    gc();
} catch (e) {
    print("Caught: " + e);
}
for (var v_28 of [[2]]) {
    try {
        gc();
    } catch (e) {
        print("Caught: " + e);
    }
}
try {
    module = v_23.instantiate();
} catch (e) {
    print("Caught: " + e);
}
try {
    v_41 = [];
} catch (e) {;
}
for (var v_43 = 0; v_43 < 100000; v_43++) try {
    v_41[v_43] = [];
} catch (e) {
    "Caught: " + e;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0392eb2^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=0392eb2)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=0392eb2)  
[src/builtins/builtins-constructor-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.h?cl=0392eb2)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=0392eb2)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=0392eb2)  
...  
  

---   

## **regress-crbug-743154.js (chromium issue)**  
   
**[Issue 743154:
 V8 correctness failure in configs: x64,ignition:arm,ignition](https://crbug.com/743154)**  
**[Commit: [builtins] Array.prototype.sort bug](https://chromium.googlesource.com/v8/v8/+/c7854ed)**  
  
Date(Commit): Tue Jul 25 13:26:03 2017  
Components/Type: Blink>JavaScript>Runtime/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/580928](https://chromium-review.googlesource.com/580928)  
Regress: [mjsunit/regress/regress-crbug-743154.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-743154.js)  
```javascript
Object.prototype[1] = 1.5;

var v = { length: 12, [1073741824]: 0 };

assertEquals(['1073741824', 'length'], Object.keys(v));
assertEquals(undefined, v[0]);
assertEquals(1.5, v[1]);
assertEquals(0, v[1073741824]);

Array.prototype.sort.call(v);

assertEquals(['0', '1073741824', 'length'], Object.keys(v));
assertTrue(v.hasOwnProperty(0));
assertEquals(1.5, v[0]);
assertFalse(v.hasOwnProperty(1));
assertEquals(1.5, v[1]);
assertEquals(0, v[1073741824]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7854ed^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=c7854ed)  
[test/mjsunit/regress/regress-crbug-743154.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-743154.js?cl=c7854ed)  
  

---   

## **regress-crbug-747979.js (chromium issue)**  
   
**[Issue 747979:
 DCHECK failure in !IsInplaceGeneralizableField(details.constness(), details.representation(), desc](https://crbug.com/747979)**  
**[Commit: [runtime] Don't create "class" field types for arrays' fields.](https://chromium.googlesource.com/v8/v8/+/c558369a)**  
  
Date(Commit): Tue Jul 25 06:54:46 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-0", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "merge-merged-6.0", "merge-merged-6.1"]  
Code Review: [https://chromium-review.googlesource.com/583647](https://chromium-review.googlesource.com/583647)  
Regress: [mjsunit/regress/regress-crbug-747979.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-747979.js)  
```javascript
function f(a) {
  %HeapObjectVerify(a);
  a[1] = 0;
  %HeapObjectVerify(a);
}

function foo() {}

var arr1 = [0];
var arr2 = [0];
var arr3 = [0];

arr1.f = foo;
arr1[0] = 4.2;

arr2.f = foo;

arr3.f = foo;
arr3[0] = 4.2;
arr3.f = f;

f(arr1);
f(arr2);
f(arr3);
%OptimizeFunctionOnNextCall(f);
f(arr3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c558369a^!)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=c558369a)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=c558369a)  
[test/mjsunit/regress/regress-crbug-747979.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-747979.js?cl=c558369a)  
  

---   

## **regress-747075.js (chromium issue)**  
   
**[Issue 747075:
 array map function returns incorrect values](https://crbug.com/747075)**  
**[Commit: [TurboFan] Array.prototype.map inlining error](https://chromium.googlesource.com/v8/v8/+/d9b98f3)**  
  
Date(Commit): Tue Jul 25 04:05:56 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Hotlist-Merge-Approved", "ReleaseBlock-Stable", "M-61", "Via-Wizard-Javascript", "merge-merged-6.1"]  
Code Review: [https://chromium-review.googlesource.com/583090](https://chromium-review.googlesource.com/583090)  
Regress: [mjsunit/regress/regress-747075.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-747075.js)  
```javascript
r = [
  14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14,
  14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14
];


function f() {
  r2 = r.map(function(y) {return y/64} );
  assertTrue(r2[0] < 1);
}

for (let i = 0; i < 1000; ++i) f();
for (let i = 0; i < 1000; ++i) f();
%OptimizeFunctionOnNextCall(f);
for (let i = 0; i < 1000; ++i) f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9b98f3^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=d9b98f3)  
[test/mjsunit/regress/regress-747075.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-747075.js?cl=d9b98f3)  
  

---   

## **regress-747825.js (chromium issue)**  
   
**[Issue 747825:
 Ill in v8::internal::TranslatedState::MaterializeCapturedObjectAt](https://crbug.com/747825)**  
**[Commit: [regexp] Teach deoptimizer to materialize JSRegExp objects](https://chromium.googlesource.com/v8/v8/+/d8e1477)**  
  
Date(Commit): Mon Jul 24 10:36:25 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Hotlist-Merge-Approved", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-6.1"]  
Code Review: [https://chromium-review.googlesource.com/582609](https://chromium-review.googlesource.com/582609)  
Regress: [mjsunit/regress/regress-747825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-747825.js)  
```javascript
var g = 0;
g = function() {}

function f() {
  var r = /[abc]/i;  // Optimized out.
  g(r);
}

f(); f(); %OptimizeFunctionOnNextCall(f);  // Warm-up.

var re;
g = function(r) { re = r; }
f();  // Lazy deopt is forced here.

assertNotEquals(undefined, re);
assertEquals("[abc]", re.source);
assertEquals("i", re.flags);
assertEquals(0, re.lastIndex);
assertArrayEquals(["a"], re.exec("a"));
assertArrayEquals(["A"], re.exec("A"));
assertNull(re.exec("d"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d8e1477^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=d8e1477)  
[test/mjsunit/regress/regress-747825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-747825.js?cl=d8e1477)  
  

---   

## **regress-crbug-722783.js (chromium issue)**  
   
**[Issue 722783:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/722783)**  
**[Commit: [ic] Properly handle reconfiguring of a global property to 'readonly'.](https://chromium.googlesource.com/v8/v8/+/b30ea16)**  
  
Date(Commit): Thu Jul 20 13:39:23 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/579268](https://chromium-review.googlesource.com/579268)  
Regress: [mjsunit/regress/regress-crbug-722783.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-722783.js)  
```javascript
function set_x(v) {
  x = v;
}

var o = {};
set_x(o);
set_x(o);
assertEquals(o, x);
Object.defineProperty(this, "x", { value:5, writable:false, configurable:true });
assertEquals(5, x);
set_x(o);
set_x(o);
assertEquals(5, x);
Object.defineProperty(this, "x", { value:42, writable:true, configurable:true });
assertEquals(42, x);
set_x(o);
assertEquals(o, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b30ea16^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b30ea16)  
[test/mjsunit/regress/regress-crbug-722783.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-722783.js?cl=b30ea16)  
  

---   

## **regress-6607-1.js (v8 issue)**  
   
**[Issue 6607:
 Array protector checks too restrictive in TurboFan](https://crbug.com/v8/6607)**  
**[Commit: [turbofan] Fix CanTreatHoleAsUndefined check.](https://chromium.googlesource.com/v8/v8/+/e1e35df)**  
  
Date(Commit): Tue Jul 18 16:29:29 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/574849](https://chromium-review.googlesource.com/574849)  
Regress: [mjsunit/regress/regress-6607-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6607-1.js), [mjsunit/regress/regress-6607-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6607-2.js)  
```javascript
function get(a, i) {
  return a[i];
}

get([1,,3], 0);
get([1,,3], 2);
%OptimizeFunctionOnNextCall(get);
get([1,,3], 0);
assertOptimized(get);

Array.prototype.unrelated = 1;
assertOptimized(get);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1e35df^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=e1e35df)  
[test/mjsunit/regress/regress-6607-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6607-1.js?cl=e1e35df)  
[test/mjsunit/regress/regress-6607-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6607-2.js?cl=e1e35df)  
  

---   

## **regress-744292.js (chromium issue)**  
   
**[Issue 744292:
 DCHECK failure in __isolate__->has_pending_exception() in runtime-module.cc](https://crbug.com/744292)**  
**[Commit: [modules] Propogate scheduled exception on ToString failure](https://chromium.googlesource.com/v8/v8/+/c45b229)**  
  
Date(Commit): Mon Jul 17 22:07:41 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Low", "Security_Impact-None", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/575394](https://chromium-review.googlesource.com/575394)  
Regress: [mjsunit/regress/regress-744292.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-744292.js)  
```javascript
__v_1 = {
};
function __f_8() {
  try {
    __f_8();
  } catch(e) {
      import(__v_1);
  }
}
__f_8();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c45b229^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=c45b229)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=c45b229)  
[test/mjsunit/regress/regress-744292.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-744292.js?cl=c45b229)  
  

---   

## **regress-743622.js (chromium issue)**  
   
**[Issue 743622:
 DCHECK failure in HasLength() in shared-function-info-inl.h](https://crbug.com/743622)**  
**[Commit: [Compiler] Fix setting shared function info flags from literal for asm_wasm.](https://chromium.googlesource.com/v8/v8/+/259bf74)**  
  
Date(Commit): Mon Jul 17 16:08:17 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/574604](https://chromium-review.googlesource.com/574604)  
Regress: [mjsunit/regress/regress-743622.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-743622.js)  
```javascript
function Module(stdlib, foreign, heap) {
  "use asm";
  var a = stdlib.Math.PI;
  function f() { return a }
  return { f:f };
}
Module.length  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/259bf74^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=259bf74)  
[test/mjsunit/regress/regress-743622.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-743622.js?cl=259bf74)  
  

---   

## **regress-740784.js (chromium issue)**  
   
**[Issue 740784:
 CHECK failure: dependent_code()->IsEmpty(DependentCode::kPrototypeCheckGroup) in objects-debug.](https://crbug.com/740784)**  
**[Commit: Don't add dependencies on prototype chain when inlining forEach](https://chromium.googlesource.com/v8/v8/+/9a0403a)**  
  
Date(Commit): Mon Jul 17 12:54:32 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/574175](https://chromium-review.googlesource.com/574175)  
Regress: [mjsunit/regress/regress-740784.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-740784.js)  
```javascript
"".stack;;var isNeverOptimize;var isAlwaysOptimize;var isInterpreted;var isOptimized;var isCrankshafted;var isTurboFanned;var failWithMessage;(function(){{;}
function PrettyPrint(){switch(typeof value){case"string":return JSON.stringify();case"number":if(1/value<0);case"object":if(value===null);switch(objectClass){case"Number":case"String":case"Boolean":case"Date":return objectClass+"("+PrettyPrint();return objectClass+"(["+joined+"])";case"Object":break;default:return objectClass+"()";}
var name=value.constructor.name;if(name)return name+"()";return"Object()";default:return"-- unknown value --";}}
function fail(){var message="Fail"+"ure";if(name_opt){message+=" ("+name_opt+")";}
return true;}
assertSame=function assertSame(){if(found===expected){return;}else if((expected!==expected)&&(found!==found)){return;}
};assertThrows=function assertThrows(code){try{if(typeof code==='function'){code();}else{;}}catch(e){if(typeof type_opt==='function'){;}else if(type_opt!==void 0){;}
return;}
;;}
isTurboFanned=function isTurboFanned(){opt_status&V8OptimizationStatus.kOptimized!==0;}})();
function __isPropertyOfType(){let desc;try{;}catch(e){return false;}
return false;return typeof type==='undefined'||typeof desc.value===type;}
function __getProperties(obj){if(typeof obj==="undefined"||obj===null)
return[];let properties=[];for(let name of Object.getOwnPropertyNames(obj)){
properties.push(name);}
let proto=Object.getPrototypeOf(obj);while(proto&&proto!=Object.prototype){Object.getOwnPropertyNames(proto).forEach(name=>{if(name!=='constructor'){__isPropertyOfType()
;}});proto=Object.getPrototypeOf(proto);}
return properties;}
function*__getObjects(root=this,level=0){if(level>4)
return;let obj_names=__getProperties(root);for(let obj_name of obj_names){let obj=root[obj_name];if(obj===root)
continue;yield obj;yield*__getObjects();}}
function __getRandomObject(){let objects=[];for(let obj of __getObjects()){;}
return objects[seed%objects.length];}
for (var __v_0 = 0; __v_0 < 2000; __v_0++) {
 Object.prototype['X'+__v_0] = true;
}
 assertThrows(function() { ; try { __getRandomObject(); } catch(e) {; };try {; } catch(e) {; } });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a0403a^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=9a0403a)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=9a0403a)  
[test/mjsunit/regress/regress-740784.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-740784.js?cl=9a0403a)  
  

---   

## **regress-739768.js (chromium issue)**  
   
**[Issue 739768:
 CHECK failure: *instance == wasm::GetOwningWasmInstance(*caller_code) in wasm-module.cc](https://crbug.com/739768)**  
**[Commit: [wasm] Fix wrong DCHECK](https://chromium.googlesource.com/v8/v8/+/485786b)**  
  
Date(Commit): Thu Jul 13 09:35:36 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/568494](https://chromium-review.googlesource.com/568494)  
Regress: [mjsunit/regress/wasm/regress-739768.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-739768.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


let builder0 = new WasmModuleBuilder();
builder0.setName('module_0');
let sig_index = builder0.addType(kSig_i_v);
builder0.addFunction('main', kSig_i_i)
    .addBody([
      kExprGetLocal, 0,  // --
      kExprCallIndirect, sig_index, kTableZero
    ])  // --
    .exportAs('main');
builder0.setTableBounds(3, 3);
builder0.addExportOfKind('table', kExternalTable);
let module0 = new WebAssembly.Module(builder0.toBuffer());
let instance0 = new WebAssembly.Instance(module0);

let builder1 = new WasmModuleBuilder();
builder1.setName('module_1');
builder1.addFunction('main', kSig_i_v).addBody([kExprUnreachable]);
builder1.addImportedTable('z', 'table');
builder1.addElementSegment(0, false, [0], true);
let module1 = new WebAssembly.Module(builder1.toBuffer());
let instance1 =
    new WebAssembly.Instance(module1, {z: {table: instance0.exports.table}});
assertThrows(
    () => instance0.exports.main(0), WebAssembly.RuntimeError, 'unreachable');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/485786b^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=485786b)  
[test/mjsunit/regress/wasm/regression-739768.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-739768.js?cl=485786b)  
  

---   

## **regress-crbug-738763.js (chromium issue)**  
   
**[Issue 738763:
 CHECK failure: !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/738763)**  
**[Commit: [runtime] Add shortcuts for elements kinds transitions.](https://chromium.googlesource.com/v8/v8/+/b90e83f)**  
  
Date(Commit): Thu Jul 13 09:16:56 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-59", "M-60", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "M-61", "merge-merged-6.0", "merge-merged-6.1"]  
Code Review: [https://chromium-review.googlesource.com/567992](https://chromium-review.googlesource.com/567992)  
Regress: [mjsunit/regress/regress-crbug-738763.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-738763.js)  
```javascript
let constant = { a: 1 };

function update_array(array) {
  array.x = constant;
  %HeapObjectVerify(array);
  array[0] = undefined;
  %HeapObjectVerify(array);
  return array;
}

let ar1 = [1];
let ar2 = [2];
let ar3 = [3];
gc();
gc();

update_array(ar1);
constant = update_array(ar2);
update_array(ar3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b90e83f^!)  
[src/heap-symbols.h](https://cs.chromium.org/chromium/src/v8/src/heap-symbols.h?cl=b90e83f)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=b90e83f)  
[src/map-updater.cc](https://cs.chromium.org/chromium/src/v8/src/map-updater.cc?cl=b90e83f)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=b90e83f)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=b90e83f)  
...  
  

---   

## **regress-crbug-736575.js (chromium issue)**  
   
**[No Permission](https://crbug.com/736575)**  
**[Commit: [turbofan] Fix type for HOLEY_DOUBLE_ELEMENTS loads.](https://chromium.googlesource.com/v8/v8/+/533f0e3)**  
  
Date(Commit): Thu Jul 13 09:04:10 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/567180](https://chromium-review.googlesource.com/567180)  
Regress: [mjsunit/regress/regress-crbug-736575.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-736575.js)  
```javascript
function f() {
  return [...[/*hole*/, 2.3]];
}

assertEquals(undefined, f()[0]);
assertEquals(undefined, f()[0]);
%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f()[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/533f0e3^!)  
[src/compiler/access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.cc?cl=533f0e3)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=533f0e3)  
[test/mjsunit/es6/array-iterator-turbo.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/array-iterator-turbo.js?cl=533f0e3)  
[test/mjsunit/regress/regress-crbug-736575.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-736575.js?cl=533f0e3)  
  

---   

## **regress-740694.js (chromium issue)**  
   
**[Issue 740694:
 Ill in v8::Utils::ReportApiFailure](https://crbug.com/740694)**  
**[Commit: [d8] Fix stack overflow when importing modules](https://chromium.googlesource.com/v8/v8/+/ea63271)**  
  
Date(Commit): Wed Jul 12 23:39:51 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/568222](https://chromium-review.googlesource.com/568222)  
Regress: [mjsunit/regress/regress-740694.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-740694.js)  
```javascript
function __f_0() {
  try {
    return __f_0();
  } catch(e) {
    return import('no-such-file');
  }
}

var done = false;
var error;
var promise = __f_0();
promise.then(assertUnreachable,
             err => { done = true; error = err });
%PerformMicrotaskCheckpoint();
assertTrue(error.startsWith('Error reading'));
assertTrue(done);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea63271^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=ea63271)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=ea63271)  
[src/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8.h?cl=ea63271)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=ea63271)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=ea63271)  
...  
  

---   

## **regress-crbug-740803.js (chromium issue)**  
   
**[Issue 740803:
 Security: Use After Free  in v8](https://crbug.com/740803)**  
**[Commit: [scope] Null out rare_data_ when aborting preparsing](https://chromium.googlesource.com/v8/v8/+/b56c0f7)**  
  
Date(Commit): Wed Jul 12 20:26:10 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "reward-3000", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "reward-inprocess", "M-59", "M-60", "M-61", "merge-merged-6.0", "Merge-Merged-60", "Release-0-M60", "CVE-2017-5098", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/568784](https://chromium-review.googlesource.com/568784)  
Regress: [mjsunit/regress/regress-crbug-740803.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-740803.js)  
```javascript
({
   m() {
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x; x;
     x;
   }
})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b56c0f7^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=b56c0f7)  
[test/mjsunit/regress/regress-crbug-740803.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-740803.js?cl=b56c0f7)  
  

---   

## **regress-crbug-740398.js (chromium issue)**  
   
**[Issue 740398:
 CHECK failure: (location_) != nullptr in handles.h](https://crbug.com/740398)**  
**[Commit: Propagate exceptions from JSFunction::SetName as needed](https://chromium.googlesource.com/v8/v8/+/873d516)**  
  
Date(Commit): Wed Jul 12 18:32:39 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/566092](https://chromium-review.googlesource.com/566092)  
Regress: [mjsunit/regress/regress-crbug-740398.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-740398.js)  
```javascript
var longString = (function() {
  return "a".repeat(%StringMaxLength());
})();

assertThrows(() => { return { get[longString]() { } } }, RangeError);
assertThrows(() => { return { set[longString](v) { } } }, RangeError);
assertThrows(() => { return { [Symbol(longString)]: () => {} } }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/873d516^!)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=873d516)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=873d516)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=873d516)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=873d516)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=873d516)  
...  
  

---   

## **regress-crbug-741078.js (chromium issue)**  
   
**[Issue 741078:
 CHECK failure: map->IsMap() in spaces.cc](https://crbug.com/741078)**  
**[Commit: [turbofan] Fix inline JSGeneratorObject allocation.](https://chromium.googlesource.com/v8/v8/+/0a4ad44)**  
  
Date(Commit): Wed Jul 12 12:47:22 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/567982](https://chromium-review.googlesource.com/567982)  
Regress: [mjsunit/regress/regress-crbug-741078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-741078.js)  
```javascript
function* gen() {}

(function warmup() {
  for (var i = 0; i < 100; ++i) {
    var g = gen();
    g.p = 42;
  }
})();

gc();   // Ensure no instance alive.
gen();  // Still has unused fields.
%OptimizeFunctionOnNextCall(gen);
gen();  // Was shrunk, boom!  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a4ad44^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=0a4ad44)  
[test/mjsunit/regress/regress-crbug-741078.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-741078.js?cl=0a4ad44)  
  

---   

## **regress-crbug-736633.js (chromium issue)**  
   
**[Issue 736633:
 Use-after-poison in v8::internal::compiler::InstructionSelector::EmitTableSwitch](https://crbug.com/736633)**  
**[Commit: [turbofan] Introduce upper limit for table switch size.](https://chromium.googlesource.com/v8/v8/+/4a4bcda)**  
  
Date(Commit): Wed Jul 12 08:35:26 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/566829](https://chromium-review.googlesource.com/566829)  
Regress: [mjsunit/regress/regress-crbug-736633.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-736633.js)  
```javascript
function f(x) {
  switch (x | 0) {
    case 0:
    case 1:
    case 2:
    case -2147483644:
    case 2147483647:
      return x + 1;
  }
  return 0;
}
assertEquals(1, f(0));
assertEquals(2, f(1));
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a4bcda^!)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=4a4bcda)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=4a4bcda)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=4a4bcda)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=4a4bcda)  
[src/compiler/mips/instruction-selector-mips.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips/instruction-selector-mips.cc?cl=4a4bcda)  
...  
  

---   

## **regress-crbug-736451.js (chromium issue)**  
   
**[Issue 736451:
 CHECK failure: ONE_BYTE == state_ in string.h](https://crbug.com/736451)**  
**[Commit: [string] Handle two-byte contents in String.p.toLowerCase](https://chromium.googlesource.com/v8/v8/+/3c26076)**  
  
Date(Commit): Wed Jul 12 06:25:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "M-61", "merge-merged-6.0"]  
Code Review: [https://chromium-review.googlesource.com/565559](https://chromium-review.googlesource.com/565559)  
Regress: [mjsunit/regress/regress-crbug-736451.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-736451.js)  
```javascript
!function() {
  const s0 = "external string turned into two byte";
  const s1 = s0.substring(1);
  externalizeString(s0, true);

  s1.toLowerCase();
}();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c26076^!)  
[src/intl.cc](https://cs.chromium.org/chromium/src/v8/src/intl.cc?cl=3c26076)  
[test/mjsunit/regress/regress-crbug-736451.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-736451.js?cl=3c26076)  
  

---   

## **regress-crbug-740116.js (chromium issue)**  
   
**[Issue 740116:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/740116)**  
**[Commit: [turbofan] Fix Reflect.getPrototypeOf on primitives.](https://chromium.googlesource.com/v8/v8/+/933a874)**  
  
Date(Commit): Tue Jul 11 12:45:12 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/565295](https://chromium-review.googlesource.com/565295)  
Regress: [mjsunit/regress/regress-crbug-740116.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-740116.js)  
```javascript
(function TestReflectGetPrototypeOfOnPrimitive() {
  function f() { return Reflect.getPrototypeOf(""); }
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();

(function TestObjectGetPrototypeOfOnPrimitive() {
  function f() { return Object.getPrototypeOf(""); }
  assertSame(String.prototype, f());
  assertSame(String.prototype, f());
  %OptimizeFunctionOnNextCall(f);
  assertSame(String.prototype, f());
})();

(function TestDunderProtoOnPrimitive() {
  function f() { return "".__proto__; }
  assertSame(String.prototype, f());
  assertSame(String.prototype, f());
  %OptimizeFunctionOnNextCall(f);
  assertSame(String.prototype, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/933a874^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=933a874)  
[test/mjsunit/regress/regress-crbug-740116.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-740116.js?cl=933a874)  
  

---   

## **regress-740325.js (chromium issue)**  
   
**[Issue 740325:
 CHECK failure: is_api_object in objects.cc](https://crbug.com/740325)**  
**[Commit: [wasm] Improve precision of slow DCHECK for WebAssembly-constructed internal objects.](https://chromium.googlesource.com/v8/v8/+/11484e7)**  
  
Date(Commit): Mon Jul 10 13:49:34 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://codereview.chromium.org/2972353002](https://codereview.chromium.org/2972353002)  
Regress: [mjsunit/asm/regress-740325.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-740325.js)  
```javascript
assertTrue = function assertTrue() { }
assertFalse = function assertFalse() { }

__v_3 = [];
__v_2 = [];
__v_0 = 0;
__v_2.__defineGetter__(0, function() {
  if (__v_0++ > 2) return;
  gc();
  __v_3.concat(__v_2);
});
__v_2[0];


function __f_2() {
}

(function __f_1() {
  print("1...");
  function __f_5(stdlib, imports) {
    "use asm";
    var __f_2 = imports.__f_2;
    function __f_3(a) {
      a = a | 0;
    }
    return { __f_3:__f_3 };
  }
  var __v_2 = __f_5(this, { __f_2:__f_2 });
;
})();

(function __f_10() {
  print("2...");
  function __f_5() {
    "use asm";
    function __f_3(a) {
    }
  }
  var __v_2 = __f_5();
  assertFalse();
})();

(function __f_11() {
  print("3...");
  let m = (function __f_6() {
    function __f_5() {
      "use asm";
      function __f_3() {
      }
      return { __f_3:__f_3 };
    }
    var __v_2 = __f_5( { __f_2:__f_2 });
  });
  for (var i = 0; i < 30; i++) {
    print("  i = " + i);
    var x = m();
    for (var j = 0; j < 200; j++) {
      try {
        __f_5;
      } catch (e) {
      }
    }
    x;
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11484e7^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=11484e7)  
[test/mjsunit/asm/regress-740325.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-740325.js?cl=11484e7)  
  

---   

## **regress-crbug-737645.js (chromium issue)**  
   
**[Issue 737645:
 V8 correctness failure in configs: x64,ignition_turbo:ia32,ignition_turbo](https://crbug.com/737645)**  
**[Commit: [runtime] Fix Array.prototype.sort for large entries](https://chromium.googlesource.com/v8/v8/+/78c74e6)**  
  
Date(Commit): Thu Jul 06 10:45:52 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/558868](https://chromium-review.googlesource.com/558868)  
Regress: [mjsunit/regress/regress-crbug-737645.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-737645.js)  
```javascript
for (let i = 0; i < 100; i++) {
  // - length > 2 to trigger sorting.
  // - key > kRequiresSlowElementsLimit required to set the according bit on the
  //   dictionary elements store.
  let key = 1073741800 + i;
  var a = { length: 12, 1: 0xFA, [key]: 0xFB };
  %HeapObjectVerify(a);
  assertEquals(["1", ""+key, "length"], Object.keys(a));
  // Sort, everything > length is ignored.
  Array.prototype.sort.call(a);
  %HeapObjectVerify(a);
  assertEquals(["0", ""+key, "length"], Object.keys(a));
  // Sorting again to trigger bug caused by not setting requires_slow_elements
  Array.prototype.sort.call(a);
  %HeapObjectVerify(a);
  assertEquals(["0", ""+key, "length"], Object.keys(a));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78c74e6^!)  
[src/builtins/builtins-intl.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-intl.cc?cl=78c74e6)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=78c74e6)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=78c74e6)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=78c74e6)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=78c74e6)  
...  
  

---   

## **regress-6509.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6509)**  
**[Commit: [ast] AstTraversalVisitor should visit the Declarations of Block scopes](https://chromium.googlesource.com/v8/v8/+/4c79544)**  
  
Date(Commit): Thu Jun 29 17:51:22 2017  
Type: None  
Code Review: [https://chromium-review.googlesource.com/556239](https://chromium-review.googlesource.com/556239)  
Regress: [mjsunit/regress/regress-6509.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6509.js)  
```javascript
(function testSloppy() {
  var arrow = (sth = (function f() {
    {
      function f2() { }
    }
  })()) => 0;

  assertEquals(0, arrow());
})();

(function testStrict() {
  "use strict";
  var arrow = (sth = (function f() {
    {
      function f2() { }
    }
  })()) => 0;

  assertEquals(0, arrow());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c79544^!)  
[src/ast/ast-traversal-visitor.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-traversal-visitor.h?cl=4c79544)  
[test/mjsunit/regress/regress-6509.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6509.js?cl=4c79544)  
  

---   

## **regress-737069.js (chromium issue)**  
   
**[Issue 737069:
 Security: Heap-buffer-overflow in v8::wasm](https://crbug.com/737069)**  
**[Commit: [wasm] Check that a function body exists before verifying it.](https://chromium.googlesource.com/v8/v8/+/a150303)**  
  
Date(Commit): Wed Jun 28 12:35:36 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reward-1000", "Security_Impact-Head", "Security_Severity-High", "allpublic", "reward-inprocess", "M-61", "Merge-Rejected-59", "merge-merged-6.0"]  
Code Review: [https://chromium-review.googlesource.com/552145](https://chromium-review.googlesource.com/552145)  
Regress: [mjsunit/regress/wasm/regress-737069.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-737069.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let binary = new Binary;

binary.emit_header();
binary.emit_section(kTypeSectionCode, section => {
  section.emit_u32v(1); // number of types
  section.emit_u8(kWasmFunctionTypeForm);
  section.emit_u32v(0); // number of parameters
  section.emit_u32v(0); // number of returns
});
binary.emit_section(kFunctionSectionCode, section => {
  section.emit_u32v(1); // number of functions
  section.emit_u32v(0); // type index
});

binary.emit_u8(kCodeSectionCode);
binary.emit_u8(0x02); // section length
binary.emit_u8(0x01); // number of functions
binary.emit_u8(0x40); // function body size

let buffer = new ArrayBuffer(binary.length);
let view = new Uint8Array(buffer);
for (let i = 0; i < binary.length; i++) {
  view[i] = binary[i] | 0;
}
WebAssembly.validate(buffer);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a150303^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=a150303)  
[test/mjsunit/regress/wasm/regression-737069.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-737069.js?cl=a150303)  
  

---   

## **regress-733181.js (chromium issue)**  
   
**[Issue 733181:
 Tab crashes every time for unknown reason (or could be a regression: 666046)](https://crbug.com/733181)**  
**[Commit: [turbofan] Add toLowerCase, toUpperCase operators to the infamous escape analysis list.](https://chromium.googlesource.com/v8/v8/+/e14c4c9)**  
  
Date(Commit): Wed Jun 28 11:12:24 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Regression  
Labels: ["Stability-Crash", "Fracas", "M-60", "NodeJS-Backport-Rejected", "Via-Wizard-Crashes", "Needs-Triage-M59", "merge-merged-5.9", "FoundIn-M-60", "merge-merged-59", "merge-merged-6.0", "Merge-Merged-60"]  
Code Review: [https://codereview.chromium.org/2962853002](https://codereview.chromium.org/2962853002)  
Regress: [mjsunit/compiler/regress-733181.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-733181.js)  
```javascript
function l(s) {
  return ("xxxxxxxxxxxxxxxxxxxxxxx" + s).toLowerCase();
}

l("abcd");
l("abcd");
%OptimizeFunctionOnNextCall(l);
l("abcd");

function u(s) {
  return ("xxxxxxxxxxxxxxxxxxxxxxx" + s).toUpperCase();
}

u("abcd");
u("abcd");
%OptimizeFunctionOnNextCall(u);
u("abcd");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e14c4c9^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=e14c4c9)  
[test/mjsunit/compiler/regress-733181.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-733181.js?cl=e14c4c9)  
  

---   

## **regress-736584.js (chromium issue)**  
   
**[Issue 736584:
 CHECK failure: mem_size == 0 implies mem_start == nullptr in wasm-module.cc](https://crbug.com/736584)**  
**[Commit: [wasm] Fix wrong implication](https://chromium.googlesource.com/v8/v8/+/08fc24b)**  
  
Date(Commit): Mon Jun 26 14:36:13 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/548635](https://chromium-review.googlesource.com/548635)  
Regress: [mjsunit/regress/wasm/regress-736584.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-736584.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let mem = new WebAssembly.Memory({initial: 0});
let builder = new WasmModuleBuilder();
builder.addImportedMemory("mod", "imported_mem");
builder.addFunction('mem_size', kSig_i_v)
    .addBody([kExprMemorySize, kMemoryZero])
    .exportFunc();
let instance = builder.instantiate({mod: {imported_mem: mem}});
instance.exports.mem_size();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/08fc24b^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=08fc24b)  
[test/mjsunit/regress/wasm/regression-736584.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-736584.js?cl=08fc24b)  
  

---   

## **regress-736567.js (chromium issue)**  
   
**[Issue 736567:
 CHECK failure: MachineRepresentation::kNone == input_info->representation() in simplified-lower](https://crbug.com/736567)**  
**[Commit: [turbofan] Fix an assertion in representation selection for BooleanNot.](https://chromium.googlesource.com/v8/v8/+/bdf1b0a)**  
  
Date(Commit): Mon Jun 26 13:49:06 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://codereview.chromium.org/2962503002](https://codereview.chromium.org/2962503002)  
Regress: [mjsunit/compiler/regress-736567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-736567.js)  
```javascript
function f(b, x) {
  var o = b ? { a : 1 } : undefined;
  return o.a + !(x & 1);
}

f(1);

function g() {
  f(0, "s");
}

assertThrows(g);
%OptimizeFunctionOnNextCall(g);
assertThrows(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bdf1b0a^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=bdf1b0a)  
[test/mjsunit/compiler/regress-736567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-736567.js?cl=bdf1b0a)  
  

---   

## **regress-6373.js (v8 issue)**  
   
**[Issue 6373:
 JSInstanceOf lowering misses ToBoolean conversion on deopt](https://crbug.com/v8/6373)**  
**[Commit: Fix deoptmization of inlined TF instanceOf to call ToBoolean](https://chromium.googlesource.com/v8/v8/+/e2544f6)**  
  
Date(Commit): Thu Jun 22 15:43:35 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2890363002](https://codereview.chromium.org/2890363002)  
Regress: [mjsunit/regress/regress-6373.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6373.js)  
```javascript
var A = {}

A[Symbol.hasInstance] = function(x) {
  %DeoptimizeFunction(foo);
  return 1;
}

var a = {}

function foo(o) {
  return o instanceof A;
}

foo(a);
foo(a);
assertTrue(foo(a) !== 1);
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(a) !== 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2544f6^!)  
[src/builtins/builtins-conversion-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-conversion-gen.cc?cl=e2544f6)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=e2544f6)  
[src/compiler/frame-states.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/frame-states.cc?cl=e2544f6)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=e2544f6)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=e2544f6)  
...  
  

---   

## **regress-refreeze-same-map.js (other issue)**  
   
**[Commit: [runtime] PreventExtensionsWithTransition: before adding the new](https://chromium.googlesource.com/v8/v8/+/6681949)**  
  
Date(Commit): Thu Jun 22 12:19:26 2017  
Code Review: [https://codereview.chromium.org/2915863004](https://codereview.chromium.org/2915863004)  
Regress: [mjsunit/regress/regress-refreeze-same-map.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-refreeze-same-map.js)  
```javascript
assertTrue(%HaveSameMap(Object.freeze({}),     Object.freeze({})));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.freeze({a: 1})));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.freeze([])));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.freeze([1,2])));

assertTrue(%HaveSameMap(Object.seal({}),     Object.seal({})));
assertTrue(%HaveSameMap(Object.seal({a: 1}), Object.seal({a: 1})));
assertTrue(%HaveSameMap(Object.seal([]),     Object.seal([])));
assertTrue(%HaveSameMap(Object.seal([1,2]),  Object.seal([1,2])));

assertTrue(%HaveSameMap(Object.freeze({}),     Object.freeze( Object.freeze({}) )));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.freeze( Object.freeze({a: 1}) )));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.freeze( Object.freeze([]) )));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.freeze( Object.freeze([1,2]) )));

assertTrue(%HaveSameMap(Object.seal({}),     Object.seal( Object.seal({}) )));
assertTrue(%HaveSameMap(Object.seal({a: 1}), Object.seal( Object.seal({a: 1}) )));
assertTrue(%HaveSameMap(Object.seal([]),     Object.seal( Object.seal([]) )));
assertTrue(%HaveSameMap(Object.seal([1,2]),  Object.seal( Object.seal([1,2]) )));

assertTrue(%HaveSameMap(Object.freeze({}),     Object.seal( Object.freeze({}) )));
assertTrue(%HaveSameMap(Object.freeze({a: 1}), Object.seal( Object.freeze({a: 1}) )));
assertTrue(%HaveSameMap(Object.freeze([]),     Object.seal( Object.freeze([]) )));
assertTrue(%HaveSameMap(Object.freeze([1,2]),  Object.seal( Object.freeze([1,2]) )));

assertTrue(%HaveSameMap(Object.freeze(Object.seal({})), Object.seal({})));

assertTrue(%HaveSameMap(Object.seal(Object.preventExtensions({})), Object.preventExtensions({})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6681949^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=6681949)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=6681949)  
[test/mjsunit/harmony/private.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/private.js?cl=6681949)  
[test/mjsunit/regress/regress-refreeze-same-map.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-refreeze-same-map.js?cl=6681949)  
  
  
---   

## **regress-720247.js (chromium issue)**  
   
**[Issue 720247:
 When eval() an with statement, VariableEnvironment is wrong.](https://crbug.com/720247)**  
**[Commit: [scopes] Fix sloppy-mode block-scoped function hoisting edge case](https://chromium.googlesource.com/v8/v8/+/d54ffad)**  
  
Date(Commit): Thu Jun 22 08:18:55 2017  
Components/Type: Blink>JavaScript>Language/Bug-Regression  
Labels: ["hasbisect", "Via-Wizard-Javascript"]  
Code Review: [https://chromium-review.googlesource.com/529230](https://chromium-review.googlesource.com/529230)  
Regress: [mjsunit/regress/regress-720247.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-720247.js)  
```javascript
assertEquals('function', typeof (function() {
    return eval('with ({a: 1}) { function a() {} }; a')
})());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d54ffad^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=d54ffad)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=d54ffad)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=d54ffad)  
[src/compiler/bytecode-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.h?cl=d54ffad)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=d54ffad)  
...  
  

---   

## **regress-734108.js (chromium issue)**  
   
**[Issue 734108:
 CHECK failure: !IsSmi() == Internals::HasHeapObjectTag(this) in objects.h](https://crbug.com/734108)**  
**[Commit: [wasm] Reopen CEntryStub handle in deferred scope when async compiling.](https://chromium.googlesource.com/v8/v8/+/045c40d)**  
  
Date(Commit): Tue Jun 20 22:22:56 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/541624](https://chromium-review.googlesource.com/541624)  
Regress: [mjsunit/regress/wasm/regress-734108.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-734108.js)  
```javascript
__v_0 = new Uint8Array([
  0x00, 0x61, 0x73, 0x6d, 0x01, 0x00, 0x00, 0x00, 0x01, 0x05, 0x01,
  0x60, 0x00, 0x01, 0x7f, 0x03, 0x02, 0x01, 0x00, 0x05, 0x03, 0x01,
  0x00, 0x01, 0x07, 0x11, 0x02, 0x04, 0x67, 0x72, 0x6f, 0x77, 0x00,
  0x00, 0x06, 0x6d, 0x65, 0x6d, 0x6f, 0x72, 0x79, 0x02, 0x00, 0x0a,
  0x08, 0x01, 0x06, 0x00, 0x41, 0x01, 0x40, 0x00, 0x0b
]);
assertPromiseResult(
  WebAssembly.compile(__v_0)
);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/045c40d^!)  
[src/compiler/wasm-compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.h?cl=045c40d)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=045c40d)  
[test/mjsunit/regress/wasm/regression-734108.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-734108.js?cl=045c40d)  
  

---   

## **regress-726625.js (chromium issue)**  
   
**[Issue 726625:
 Unicode non-character treated as whitespace](https://crbug.com/726625)**  
**[Commit: [parser] Treat \ufffe as non-whitespace.](https://chromium.googlesource.com/v8/v8/+/79324c4)**  
  
Date(Commit): Tue Jun 20 16:44:51 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-60", "Via-Wizard-Javascript"]  
Code Review: [https://chromium-review.googlesource.com/530849](https://chromium-review.googlesource.com/530849)  
Regress: [mjsunit/regress/regress-726625.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-726625.js)  
```javascript
function abc() { return; }
assertThrows("abc" + String.fromCharCode(65534) + "(1)");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79324c4^!)  
[src/parsing/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/scanner.cc?cl=79324c4)  
[test/mjsunit/regress/regress-726625.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-726625.js?cl=79324c4)  
[test/mozilla/mozilla.status](https://cs.chromium.org/chromium/src/v8/test/mozilla/mozilla.status?cl=79324c4)  
  

---   

## **regress-734345.js (chromium issue)**  
   
**[Issue 734345:
 CHECK failure: (owning_instance) != nullptr in runtime-wasm.cc](https://crbug.com/734345)**  
**[Commit: [wasm] Keep instances of imported code alive](https://chromium.googlesource.com/v8/v8/+/ebc76f6)**  
  
Date(Commit): Tue Jun 20 16:23:09 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "allpublic", "Clusterfuzz", "M-59", "M-60", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/539738](https://chromium-review.googlesource.com/539738)  
Regress: [mjsunit/regress/wasm/regress-734345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-734345.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

builder1 = new WasmModuleBuilder();
builder1.addFunction('exp1', kSig_v_v).addBody([kExprUnreachable]).exportFunc();

builder2 = new WasmModuleBuilder();
builder2.addImport('imp', 'imp', kSig_v_v);
builder2.addFunction('call_imp', kSig_v_v)
    .addBody([kExprCallFunction, 0])
    .exportFunc();

export1 = builder1.instantiate().exports.exp1;
export2 = builder2.instantiate({imp: {imp: export1}}).exports.call_imp;
export1 = undefined;

let a = [0];
for (i = 0; i < 10; ++i) {
  a = a.concat(new Array(i).fill(i));
  assertThrows(() => export2(), WebAssembly.RuntimeError);
  gc();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ebc76f6^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=ebc76f6)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=ebc76f6)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=ebc76f6)  
[test/mjsunit/regress/wasm/regression-734345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-734345.js?cl=ebc76f6)  
  

---   

## **regress-6431.js (v8 issue)**  
   
**[Issue 6431:
 [asm.js] Coercion of global imports values causes observable side-effect.](https://crbug.com/v8/6431)**  
**[Commit: [asm.js] Ensure coercion of imports is non-observable.](https://chromium.googlesource.com/v8/v8/+/21cbc914)**  
  
Date(Commit): Tue Jun 20 13:55:35 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/512544](https://chromium-review.googlesource.com/512544)  
Regress: [mjsunit/regress/regress-6431.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6431.js)  
```javascript
(function TestImportSymbolValue() {
  function Module(stdlib, foreign) {
    "use asm";
    var x = +foreign.x;
    function f() {}
    return { f:f };
  }
  var foreign = { x : Symbol("boom") };
  assertThrows(() => Module(this, foreign));
  assertFalse(%IsAsmWasmCode(Module));
})();

(function TestImportMutatingObject() {
  function Module(stdlib, foreign) {
    "use asm";
    var x = +foreign.x;
    var PI = stdlib.Math.PI;
    function f() { return +(PI + x) }
    return { f:f };
  }
  var stdlib = { Math : { PI : Math.PI } };
  var foreign = { x : { valueOf : () => (stdlib.Math.PI = 23, 42) } };
  var m = Module(stdlib, foreign);
  assertFalse(%IsAsmWasmCode(Module));
  assertEquals(65, m.f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21cbc914^!)  
[src/wasm/module-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-compiler.cc?cl=21cbc914)  
[test/mjsunit/asm/global-imports.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/global-imports.js?cl=21cbc914)  
[test/mjsunit/regress/regress-6431.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6431.js?cl=21cbc914)  
[test/mjsunit/wasm/asm-wasm.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm.js?cl=21cbc914)  
  

---   

## **regress-734246.js (chromium issue)**  
   
**[Issue 734246:
 CHECK failure: offset_ <= offset_ + length_ in wasm-module.h](https://crbug.com/734246)**  
**[Commit: [wasm] Avoid constructing overflowing WireBytesRefs](https://chromium.googlesource.com/v8/v8/+/6269b2b)**  
  
Date(Commit): Tue Jun 20 13:48:44 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "Test-Predator-Correct-CLs", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/539402](https://chromium-review.googlesource.com/539402)  
Regress: [mjsunit/regress/wasm/regress-734246.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-734246.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder.addExplicitSection([
  kUnknownSectionCode,
  // section length
  0x0f,
  // name length: 0xffffffff
  0xf9, 0xff, 0xff, 0xff, 0x0f
]);
assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6269b2b^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=6269b2b)  
[test/mjsunit/regress/wasm/regression-734246.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-734246.js?cl=6269b2b)  
  

---   

## **regress-crbug-734162.js (chromium issue)**  
   
**[Issue 734162:
 V8 correctness failure in configs: x64,ignition:ia32,ignition](https://crbug.com/734162)**  
**[Commit: [literals] Perform a deep boilerplate copy for MutableHeapNumber fields](https://chromium.googlesource.com/v8/v8/+/7dcd046)**  
  
Date(Commit): Tue Jun 20 10:24:00 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/541415](https://chromium-review.googlesource.com/541415)  
Regress: [mjsunit/regress/regress-crbug-734162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-734162.js)  
```javascript
(function TestSmi() {
  var v_0 = {};
  function f_0(constructor, closure) {
    var v_2 = { value: 0 };
    v_4 = closure(constructor, 1073741823, v_0, v_2);
    assertEquals(1, v_2.value);
  }
  function f_1(constructor, val, deopt, v_2) {
    if (!new constructor(val, deopt, v_2)) {
    }
  }
  function f_10(constructor) {
    f_0(constructor, f_1);
    f_0(constructor, f_1);
    f_0(constructor, f_1);
  }
  function f_12(val, deopt, v_2) {
    v_2.value++;
  }
  f_10(f_12);
})();

(function TestHeapNumber() {
  var v_0 = {};
  function f_0(constructor, closure) {
    var v_2 = { value: 1.5 };
    v_4 = closure(constructor, 1073741823, v_0, v_2);
    assertEquals(2.5, v_2.value);
  }
  function f_1(constructor, val, deopt, v_2) {
    if (!new constructor(val, deopt, v_2)) {
    }
  }
  function f_10(constructor) {
    f_0(constructor, f_1);
    f_0(constructor, f_1);
    f_0(constructor, f_1);
  }
  function f_12(val, deopt, v_2) {
    v_2.value++;
  }
  f_10(f_12);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7dcd046^!)  
[src/runtime/runtime-literals.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-literals.cc?cl=7dcd046)  
[test/mjsunit/object-literal.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-literal.js?cl=7dcd046)  
[test/mjsunit/regress/regress-crbug-734051.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-734051.js?cl=7dcd046)  
[test/mjsunit/regress/regress-crbug-734162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-734162.js?cl=7dcd046)  
  

---   

## **regress-crbug-734051.js (chromium issue)**  
   
**[No Permission](https://crbug.com/734051)**  
**[Commit: [literals] Perform a deep boilerplate copy for MutableHeapNumber fields](https://chromium.googlesource.com/v8/v8/+/7dcd046)**  
  
Date(Commit): Tue Jun 20 10:24:00 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/541415](https://chromium-review.googlesource.com/541415)  
Regress: [mjsunit/regress/regress-crbug-734051.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-734051.js)  
```javascript
function TestMutableHeapNumberLiteral() {
    var data = { a: 0, b: 0 };
    data.a += 0.1;
    assertEquals(0.1, data.a);
    assertEquals(0, data.b);
};
TestMutableHeapNumberLiteral();
TestMutableHeapNumberLiteral();
TestMutableHeapNumberLiteral();
TestMutableHeapNumberLiteral();
TestMutableHeapNumberLiteral();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7dcd046^!)  
[src/runtime/runtime-literals.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-literals.cc?cl=7dcd046)  
[test/mjsunit/object-literal.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-literal.js?cl=7dcd046)  
[test/mjsunit/regress/regress-crbug-734051.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-734051.js?cl=7dcd046)  
[test/mjsunit/regress/regress-crbug-734162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-734162.js?cl=7dcd046)  
  

---   

## **regress-crbug-731193.js (chromium issue)**  
   
**[Issue 731193:
 undefined prototypal inherited properties [OpenStreetMap iD editor]](https://crbug.com/731193)**  
**[Commit: [ic] Fix stub-cached access to use the dereffed thin-string.](https://chromium.googlesource.com/v8/v8/+/2325ef5)**  
  
Date(Commit): Mon Jun 19 13:33:19 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Via-Wizard-Javascript"]  
Code Review: [https://chromium-review.googlesource.com/539596](https://chromium-review.googlesource.com/539596)  
Regress: [mjsunit/regress/regress-crbug-731193.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-731193.js)  
```javascript
function f() {
}

for (var i = 0; i < 10000; i++) {
  f.prototype["b" + i] = 1;
}

var o = new f();

function access(o, k) {
  return o[k];
}

var p = "b";
p += 10001;

assertEquals(undefined, access(o, p));
assertEquals(undefined, access(o, p));
assertEquals(undefined, access(o, p));
f.prototype[p] = 100;
assertEquals(100, access(o, p));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2325ef5^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=2325ef5)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=2325ef5)  
[test/mjsunit/regress/regress-crbug-731193.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-731193.js?cl=2325ef5)  
  

---   

## **regress-732836.js (chromium issue)**  
   
**[Issue 732836:
 CHECK failure: size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/732836)**  
**[Commit: [builtins] Allow large allocations when unboxing double arrays.](https://chromium.googlesource.com/v8/v8/+/a1baf26)**  
  
Date(Commit): Mon Jun 19 11:08:01 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/535563](https://chromium-review.googlesource.com/535563)  
Regress: [mjsunit/regress/regress-732836.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-732836.js)  
```javascript
function boom() {
  var args = [];
  for (var i = 0; i < 125000; i++)
    args.push(1.1);
  return Array.apply(Array, args);
}
var array = boom();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a1baf26^!)  
[src/builtins/builtins-call-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-call-gen.cc?cl=a1baf26)  
[test/mjsunit/regress/regress-732836.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-732836.js?cl=a1baf26)  
  

---   

## **regress-733059.js (chromium issue)**  
   
**[Issue 733059:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (!owned || FindObject(address)->IsHea](https://crbug.com/733059)**  
**[Commit: [heap] Fix adjusting of area end when shrinking large pages](https://chromium.googlesource.com/v8/v8/+/2138950)**  
  
Date(Commit): Wed Jun 14 15:18:01 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/535596](https://chromium-review.googlesource.com/535596)  
Regress: [mjsunit/regress/regress-733059.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-733059.js)  
```javascript
a = new Proxy([], {
  defineProperty() {
    b.length = 1; gc();
    return Object.defineProperty.apply(this, arguments);
  }
});

class MyArray extends Array {
  static get[Symbol.species](){
    return function() {
      return a;
    }
  };
}

b = new MyArray(65535);
b[1] = 0.1;
c = Array.prototype.concat.call(b);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2138950^!)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=2138950)  
[src/heap/spaces.h](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.h?cl=2138950)  
[test/mjsunit/regress/regress-733059.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-733059.js?cl=2138950)  
  

---   

## **regress-crbug-729597.js (chromium issue)**  
   
**[Issue 729597:
 Null-dereference READ in heap](https://crbug.com/729597)**  
**[Commit: [heap-verify] Relax arguments verification](https://chromium.googlesource.com/v8/v8/+/66fe2d4)**  
  
Date(Commit): Wed Jun 14 07:19:20 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-60", "Merge-Rejected-60"]  
Code Review: [https://chromium-review.googlesource.com/532900](https://chromium-review.googlesource.com/532900)  
Regress: [mjsunit/regress/regress-crbug-729597.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-729597.js)  
```javascript
function __f_3(f) {
  arguments.__defineGetter__('length', f);
  return arguments;
}
function __f_4() { return "boom"; }

__v_4 = [];
__v_13 = "";

for (var i = 0; i < 12800; ++i) {
  __v_13 +=  __v_4.__proto__ = __f_3(__f_4);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/66fe2d4^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=66fe2d4)  
[test/mjsunit/regress/regress-crbug-729597.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-729597.js?cl=66fe2d4)  
  

---   

## **regress-731351.js (chromium issue)**  
   
**[Issue 731351:
 Crash in v8::internal::Invoke](https://crbug.com/731351)**  
**[Commit: [wasm] Correctly reset memory size to default instead of 0.](https://chromium.googlesource.com/v8/v8/+/5db4364)**  
  
Date(Commit): Tue Jun 13 16:39:52 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "M-60", "ClusterFuzz-Verified", "merge-merged-6.0", "Release-0-M60"]  
Code Review: [https://chromium-review.googlesource.com/532634](https://chromium-review.googlesource.com/532634)  
Regress: [mjsunit/regress/wasm/regress-731351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-731351.js)  
```javascript
gc();
function asm(stdlib, foreign, buffer) {
  "use asm";
  var HEAP32 = new stdlib.Uint32Array(buffer);
  function load(a) {
    a = a | 0;
    return +(HEAP32[a >> 2] >>> 0);
  }
  return {load: load};
}

function RunAsmJsTest() {
  buffer = new ArrayBuffer(65536);
  var asm_module = asm({Uint32Array: Uint32Array}, {}, buffer);
  asm_module.load(buffer.byteLength);
}
RunAsmJsTest();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5db4364^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=5db4364)  
[test/mjsunit/regress/wasm/regression-731351.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-731351.js?cl=5db4364)  
  

---   

## **regress-725858.js (chromium issue)**  
   
**[No Permission](https://crbug.com/725858)**  
**[Commit: [arm64] Fix pre-shifted immediate generation involving csp.](https://chromium.googlesource.com/v8/v8/+/849a08b)**  
  
Date(Commit): Tue Jun 13 15:04:13 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2922173004](https://codereview.chromium.org/2922173004)  
Regress: [mjsunit/regress/regress-725858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-725858.js)  
```javascript
function f() {}
var src = 'f(' + '0,'.repeat(0x201f) + ')';
var boom = new Function(src);
%OptimizeFunctionOnNextCall(boom);
boom();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/849a08b^!)  
[src/arm64/macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.cc?cl=849a08b)  
[src/arm64/macro-assembler-arm64.h](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.h?cl=849a08b)  
[test/cctest/test-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-assembler-arm64.cc?cl=849a08b)  
[test/mjsunit/regress/regress-725858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-725858.js?cl=849a08b)  
  

---   

## **regress-crbug-732169.js (chromium issue)**  
   
**[Issue 732169:
 Ill in v8::internal::TranslatedState::MaterializeCapturedObjectAt](https://crbug.com/732169)**  
**[Commit: [deoptimizer] Add support for materializing Generator objects.](https://chromium.googlesource.com/v8/v8/+/f555a69)**  
  
Date(Commit): Mon Jun 12 11:30:22 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/530847](https://chromium-review.googlesource.com/530847)  
Regress: [mjsunit/regress/regress-crbug-732169.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-732169.js)  
```javascript
(function TestGeneratorMaterialization() {
  function* f([x]) { yield x }
  // No warm-up of {f} to trigger soft deopt.
  %OptimizeFunctionOnNextCall(f);
  var gen = f([23]);
  assertEquals("[object Generator]", gen.toString());
  assertEquals({ done:false, value:23 }, gen.next());
  assertEquals({ done:true, value:undefined }, gen.next());
})();

(function TestGeneratorMaterializationWithProperties() {
  function* f(x = (%_DeoptimizeNow(), 23)) { yield x }
  function g() {
    var gen = f();
    gen.p = 42;
    return gen;
  }
  function h() { f() }
  // Enough warm-up to make {p} an in-object property.
  for (var i = 0; i < 100; ++i) { g(); h(); }
  %OptimizeFunctionOnNextCall(h);
  h();  // In {h} the generator does not escape.
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f555a69^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=f555a69)  
[test/mjsunit/regress/regress-crbug-732169.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-732169.js?cl=f555a69)  
  

---   

## **regress-731495.js (chromium issue)**  
   
**[Issue 731495:
 CHECK failure: args[0]->IsString() in runtime-strings.cc](https://crbug.com/731495)**  
**[Commit: [TurboFan] Fix typing of INTERNALIZED_STRING_TYPE for new EmptyString type.](https://chromium.googlesource.com/v8/v8/+/fc826e3)**  
  
Date(Commit): Fri Jun 09 15:10:56 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Code Review: [https://chromium-review.googlesource.com/528275](https://chromium-review.googlesource.com/528275)  
Regress: [mjsunit/compiler/regress-731495.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-731495.js)  
```javascript
function foo() {
  global = "";
  global = global + "bar";
  return global;
};

assertEquals(foo(), "bar");
%OptimizeFunctionOnNextCall(foo);
assertEquals(foo(), "bar");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc826e3^!)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=fc826e3)  
[test/mjsunit/compiler/regress-731495.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-731495.js?cl=fc826e3)  
  

---   

## **regress-crbug-707580.js (chromium issue)**  
   
**[Issue 707580:
 V8 correctness failure in configs: x64,ignition:arm,ignition](https://crbug.com/707580)**  
**[Commit: [builtins] Make sure to perform ToPrimitive(key, hint string) in hasOwnProperty even if the receiver is a smi.](https://chromium.googlesource.com/v8/v8/+/fe04841)**  
  
Date(Commit): Thu Jun 08 15:12:31 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "merge-merged-6.0"]  
Code Review: [https://chromium-review.googlesource.com/528077](https://chromium-review.googlesource.com/528077)  
Regress: [mjsunit/regress/regress-crbug-707580.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-707580.js)  
```javascript
var thrower = { [Symbol.toPrimitive] : function() { throw "I was called!" } };
var heap_number = 4.2;
var smi_number = 23;

assertThrows(() => heap_number.hasOwnProperty(thrower));
assertThrows(() => smi_number.hasOwnProperty(thrower));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe04841^!)  
[src/builtins/builtins-object-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object-gen.cc?cl=fe04841)  
[test/mjsunit/regress/regress-crbug-707580.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-707580.js?cl=fe04841)  
  

---   

## **regress-729369.js (chromium issue)**  
   
**[Issue 729369:
 Null-dereference READ in v8::internal::interpreter::BytecodeRegisterOptimizer::RegisterInfo::register_val](https://crbug.com/729369)**  
**[Commit: [interpreter] Make sure allocated registers are always materialized in the register optimizer.](https://chromium.googlesource.com/v8/v8/+/b543c2d)**  
  
Date(Commit): Wed Jun 07 15:39:56 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2926063002](https://codereview.chromium.org/2926063002)  
Regress: [mjsunit/compiler/regress-729369.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-729369.js)  
```javascript
function* f() {
  x.__defineGetter__();
  var x = 0;
  for (let y of iterable) {
    yield y;
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b543c2d^!)  
[src/interpreter/bytecode-register-optimizer.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register-optimizer.cc?cl=b543c2d)  
[src/interpreter/bytecode-register-optimizer.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-register-optimizer.h?cl=b543c2d)  
[test/cctest/interpreter/bytecode_expectations/ObjectLiterals.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ObjectLiterals.golden?cl=b543c2d)  
[test/mjsunit/compiler/regress-729369.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-729369.js?cl=b543c2d)  
  

---   

## **regress-crbug-728813.js (chromium issue)**  
   
**[Issue 728813:
 Ill in v8::Utils::ReportApiFailure](https://crbug.com/728813)**  
**[Commit: Fix Array.indexOf for Proxies that throw](https://chromium.googlesource.com/v8/v8/+/8bc98b5)**  
  
Date(Commit): Wed Jun 07 12:33:50 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/527173](https://chromium-review.googlesource.com/527173)  
Regress: [mjsunit/regress/regress-crbug-728813.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-728813.js)  
```javascript
var p = new Proxy({}, {
  has: function () { throw "nope"; }
});
p.length = 2;
assertThrows(() => Array.prototype.indexOf.call(p));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8bc98b5^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=8bc98b5)  
[test/mjsunit/regress/regress-crbug-728813.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-728813.js?cl=8bc98b5)  
  

---   

## **regress-730254.js (chromium issue)**  
   
**[Issue 730254:
 Null-dereference READ in v8::internal::compiler::Node::opcode](https://crbug.com/730254)**  
**[Commit: [Turbofan] Fix to not leak holes on any edges.](https://chromium.googlesource.com/v8/v8/+/66218e4)**  
  
Date(Commit): Wed Jun 07 12:07:24 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/509613](https://chromium-review.googlesource.com/509613)  
Regress: [mjsunit/regress/regress-730254.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-730254.js)  
```javascript
var __v_0 = {};
__v_0 = new Map();
function __f_0() {
  __v_0[0] --;
}
__f_0();
%OptimizeFunctionOnNextCall(__f_0);
__f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/66218e4^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=66218e4)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=66218e4)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=66218e4)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=66218e4)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=66218e4)  
...  
  

---   

## **regress-729671.js (chromium issue)**  
   
**[Issue 729671:
 Stack-overflow in JSONParser](https://crbug.com/729671)**  
**[Commit: [json] Handle stack overflows in JSON.parse](https://chromium.googlesource.com/v8/v8/+/84a54c5)**  
  
Date(Commit): Wed Jun 07 07:47:13 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/525812](https://chromium-review.googlesource.com/525812)  
Regress: [mjsunit/regress/regress-729671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-729671.js)  
```javascript
var o = { 0: 11, 1: 9};
assertThrows(() => JSON.parse('[0,0]', function() { this[1] = o; }), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84a54c5^!)  
[src/json-parser.cc](https://cs.chromium.org/chromium/src/v8/src/json-parser.cc?cl=84a54c5)  
[test/mjsunit/regress/regress-729671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-729671.js?cl=84a54c5)  
  

---   

## **regress-crbug-729573-1.js (chromium issue)**  
   
**[No Permission](https://crbug.com/729573)**  
**[Commit: [deoptimizer] Teach the Deoptimizer about bound functions.](https://chromium.googlesource.com/v8/v8/+/337bb36)**  
  
Date(Commit): Wed Jun 07 06:25:26 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2931483003](https://codereview.chromium.org/2931483003)  
Regress: [mjsunit/regress/regress-crbug-729573-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-729573-1.js), [mjsunit/regress/regress-crbug-729573-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-729573-2.js)  
```javascript
(function() {
  function foo() {
    var a = foo.bind(this);
    %DeoptimizeNow();
    if (!a) return a;
    return 0;
  }

  assertEquals(0, foo());
  assertEquals(0, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(0, foo());
})();

(function() {
  "use strict";

  function foo() {
    var a = foo.bind(this);
    %DeoptimizeNow();
    if (!a) return a;
    return 0;
  }

  assertEquals(0, foo());
  assertEquals(0, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(0, foo());
})();

(function() {
  function foo() {
    var a = foo.bind(this);
    %DeoptimizeNow();
    if (!a) return a;
    return 0;
  }
  foo.prototype = {custom: "prototype"};

  assertEquals(0, foo());
  assertEquals(0, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(0, foo());
})();

(function() {
  "use strict";

  function foo() {
    var a = foo.bind(this);
    %DeoptimizeNow();
    if (!a) return a;
    return 0;
  }
  foo.prototype = {custom: "prototype"};

  assertEquals(0, foo());
  assertEquals(0, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(0, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/337bb36^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=337bb36)  
[test/mjsunit/regress/regress-crbug-729573-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-729573-1.js?cl=337bb36)  
[test/mjsunit/regress/regress-crbug-729573-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-729573-2.js?cl=337bb36)  
  

---   

## **regress-729991.js (chromium issue)**  
   
**[Issue 729991:
 Security: Information Disclosure Issue in v8::wasm](https://crbug.com/729991)**  
**[Commit: [wasm] Add regression test](https://chromium.googlesource.com/v8/v8/+/fa0d5be)**  
  
Date(Commit): Tue Jun 06 15:55:02 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "reward-4000", "Arch-All", "Security_Severity-High", "allpublic", "reward-inprocess", "M-59", "merge-merged-5.9", "Release-1-M59", "CVE-2017-5088", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/525538](https://chromium-review.googlesource.com/525538)  
Regress: [mjsunit/regress/wasm/regress-729991.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-729991.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder.addCustomSection('BBBB', []);
builder.addCustomSection('AAAA', new Array(32).fill(0));
let buffer = builder.toBuffer();
buffer = buffer.slice(0, buffer.byteLength - 30);
assertThrows(() => new WebAssembly.Module(buffer), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa0d5be^!)  
[test/mjsunit/regress/wasm/regression-729991.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-729991.js?cl=fa0d5be)  
  

---   

## **regress-726554.js (chromium issue)**  
   
**[Issue 726554:
 CHECK failure: LoadElement of kRepFloat64 (NumberOrHole) cannot be changed to kRepTagged in rep](https://crbug.com/726554)**  
**[Commit: [turbofan] Improve representation selection for type guard.](https://chromium.googlesource.com/v8/v8/+/5005fae)**  
  
Date(Commit): Tue Jun 06 14:45:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2920193004](https://codereview.chromium.org/2920193004)  
Regress: [mjsunit/compiler/regress-726554.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-726554.js)  
```javascript
function h(a,b){
  for(var i=0; i<a.length; i++) {h(a[i],b[i]); }
}

function g() {
  h(arguments.length, 2);
}

function f() {
  return g(1, 2);
}

b = [1,,];
b[1] = 3.5;

h(b, [1073741823, 2147483648, -12]);

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5005fae^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=5005fae)  
[test/mjsunit/compiler/regress-726554.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-726554.js?cl=5005fae)  
  

---   

## **regress-crbug-724608.js (chromium issue)**  
   
**[Issue 724608:
 CHECK failure: !map->is_deprecated() in compilation-dependencies.cc](https://crbug.com/724608)**  
**[Commit: [turbofan] Try to update deprecated maps first.](https://chromium.googlesource.com/v8/v8/+/468446d)**  
  
Date(Commit): Tue Jun 06 12:10:40 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Beta", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2930433003](https://codereview.chromium.org/2930433003)  
Regress: [mjsunit/regress/regress-crbug-724608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-724608.js)  
```javascript
function foo(x) {
  return {['p']: 0, x};
}
foo();
var a = {['p']: ''};
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/468446d^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=468446d)  
[test/mjsunit/regress/regress-crbug-724608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-724608.js?cl=468446d)  
  

---   

## **regress-generators-resume.js (other issue)**  
   
**[Commit: This is a first step towards reducing the number of stores/loads when suspending/resuming a generator.](https://chromium.googlesource.com/v8/v8/+/f064561)**  
  
Date(Commit): Fri Jun 02 11:55:48 2017  
Code Review: [https://codereview.chromium.org/2894293003](https://codereview.chromium.org/2894293003)  
Regress: [mjsunit/harmony/regress-generators-resume.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress-generators-resume.js)  
```javascript
function* foo() {
  for (let i = 0; i < 10; i++) {
    yield 1;
  }
  return 0;
}

g = foo();
%OptimizeFunctionOnNextCall(foo);
g.next();
g.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f064561^!)  
[src/compiler/bytecode-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-analysis.cc?cl=f064561)  
[src/compiler/bytecode-analysis.h](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-analysis.h?cl=f064561)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=f064561)  
[src/compiler/js-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.h?cl=f064561)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=f064561)  
...  
  
  
---   

## **regress-727560.js (chromium issue)**  
   
**[Issue 727560:
 CHECK failure: memory_->byte_length()->ToUint32(&mem_size) in wasm-module.cc](https://crbug.com/727560)**  
**[Commit: [wasm] Fix WasmMemoryObject constructor for when a module has no initial memory](https://chromium.googlesource.com/v8/v8/+/5c0baf7)**  
  
Date(Commit): Thu Jun 01 17:08:02 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2917603002](https://codereview.chromium.org/2917603002)  
Regress: [mjsunit/regress/wasm/regress-727560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-727560.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

{
  let builder = new WasmModuleBuilder();
  builder.addMemory();
  builder.exportMemoryAs("exported_mem");
  i1 = builder.instantiate();
}
{
  let builder = new WasmModuleBuilder();
  builder.addImportedMemory("fil", "imported_mem");
  i2 = builder.instantiate({fil: {imported_mem: i1.exports.exported_mem}});
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c0baf7^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=5c0baf7)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=5c0baf7)  
[test/mjsunit/regress/wasm/regression-724972.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-724972.js?cl=5c0baf7)  
[test/mjsunit/regress/wasm/regression-727560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-727560.js?cl=5c0baf7)  
  

---   

## **regress-724972.js (chromium issue)**  
   
**[Issue 724972:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSArrayBuffer()) in object](https://crbug.com/724972)**  
**[Commit: [wasm] Fix WasmMemoryObject constructor for when a module has no initial memory](https://chromium.googlesource.com/v8/v8/+/5c0baf7)**  
  
Date(Commit): Thu Jun 01 17:08:02 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2917603002](https://codereview.chromium.org/2917603002)  
Regress: [mjsunit/regress/wasm/regress-724972.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-724972.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addMemory(0, 0, true);
var instance = builder.instantiate();
instance.exports.memory.buffer;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c0baf7^!)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=5c0baf7)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=5c0baf7)  
[test/mjsunit/regress/wasm/regression-724972.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-724972.js?cl=5c0baf7)  
[test/mjsunit/regress/wasm/regression-727560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-727560.js?cl=5c0baf7)  
  

---   

## **regress-725743.js (chromium issue)**  
   
**[Issue 725743:
 CHECK failure: interrupt_address == isolate->builtins()->InterruptCheck()->entry() in full-code](https://crbug.com/725743)**  
**[Commit: [arm] Clean up disabling of sharing code target entries.](https://chromium.googlesource.com/v8/v8/+/6a99238)**  
  
Date(Commit): Thu Jun 01 13:18:21 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2922433002](https://codereview.chromium.org/2922433002)  
Regress: [mjsunit/compiler/regress-725743.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-725743.js)  
```javascript
function f() {
  var n = a.length;
  for (var i = 0; i < n; i++) {
  }
  for (var i = 0; i < n; i++) {
  }
}
var a = "xxxxxxxxxxxxxxxxxxxxxxxxx";
while (a.length < 100000) a = a + a;
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6a99238^!)  
[src/arm/assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.cc?cl=6a99238)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=6a99238)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=6a99238)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=6a99238)  
[test/mjsunit/compiler/regress-725743.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-725743.js?cl=6a99238)  
  

---   

## **regress-719866.js (chromium issue)**  
   
**[Issue 719866:
 asm.js code generation regression in Chrome Canary (60.0.3094.0)](https://crbug.com/719866)**  
**[Commit: [asm.js] Fix associativity of multiplicative expressions.](https://chromium.googlesource.com/v8/v8/+/1569175)**  
  
Date(Commit): Thu Jun 01 13:03:03 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: []  
Code Review: [https://chromium-review.googlesource.com/520945](https://chromium-review.googlesource.com/520945)  
Regress: [mjsunit/asm/regress-719866.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-719866.js)  
```javascript
function Module(stdlib) {
  "use asm";
  function f(a,b,c) {
    a = +a;
    b = +b;
    c = +c;
    var r = 0.0;
    r = a / b * c;
    return +r;
  }
  return { f:f }
}
var m = Module(this);
assertEquals(16, m.f(32, 4, 2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1569175^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=1569175)  
[test/mjsunit/asm/regress-719866.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-719866.js?cl=1569175)  
  

---   

## **regress-727662.js (chromium issue)**  
   
**[No Permission](https://crbug.com/727662)**  
**[Commit: [turbofan] Mark SeqStringCharCodeAt return type as Word32, not Tagged.](https://chromium.googlesource.com/v8/v8/+/ad3724e)**  
  
Date(Commit): Wed May 31 10:51:28 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/519302](https://chromium-review.googlesource.com/519302)  
Regress: [mjsunit/regress/regress-727662.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-727662.js)  
```javascript
(function() {
  function thingo(i, b) {
    var s = b ? "ac" : "abcd";
    i = i >>> 0;
    if (i < s.length) {
      var c = s.charCodeAt(i);
      gc();
      return c;
    }
  }
  thingo(0, true);
  thingo(0, true);
  %OptimizeFunctionOnNextCall(thingo);
  thingo(0, true);

})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad3724e^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=ad3724e)  
[test/mjsunit/regress/regress-727662.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-727662.js?cl=ad3724e)  
  

---   

## **regress-727218.js (chromium issue)**  
   
**[Issue 727218:
 CHECK failure: is_resolved() in ast.h](https://crbug.com/727218)**  
**[Commit: [parser] Disable aborting preparsing for arrow functions.](https://chromium.googlesource.com/v8/v8/+/36de919)**  
  
Date(Commit): Tue May 30 14:00:54 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-60", "Merge-Merged-60"]  
Code Review: [https://chromium-review.googlesource.com/518145](https://chromium-review.googlesource.com/518145)  
Regress: [mjsunit/regress/regress-727218.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-727218.js)  
```javascript
var f = ({ x } = { x: y }) => {
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/36de919^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=36de919)  
[test/mjsunit/regress/regress-727218.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-727218.js?cl=36de919)  
  

---   

## **regress-723366.js (chromium issue)**  
   
**[Issue 723366:
 CHECK failure: 1 <= target_receiver_maps.size() in ic.cc](https://crbug.com/723366)**  
**[Commit: [ic] Properly handle the case when all receiver maps are deprecated.](https://chromium.googlesource.com/v8/v8/+/8820a79)**  
  
Date(Commit): Tue May 30 09:38:48 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/517952](https://chromium-review.googlesource.com/517952)  
Regress: [mjsunit/regress/regress-723366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-723366.js)  
```javascript
var o = {foo: 0, 0: 0, 2: 2, 3: 3};
o.__defineSetter__("1", function(v) { this.foo = 0.1; });

for(var i = 0; i < 4; i++) {
  switch (i) {
    case 0: o.p1 = 0; break;
    case 1: o.p2 = 0; break;
  }
  o[i] = i;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8820a79^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=8820a79)  
[test/mjsunit/regress/regress-723366.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-723366.js?cl=8820a79)  
  

---   

## **regress-727222.js (chromium issue)**  
   
**[Issue 727222:
 CHECK failure: (module_->instance) != nullptr in wasm-compiler.cc](https://crbug.com/727222)**  
**[Commit: [wasm] Remove more obsolete DCHECKs](https://chromium.googlesource.com/v8/v8/+/b5203e8)**  
  
Date(Commit): Tue May 30 08:58:09 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/517947](https://chromium-review.googlesource.com/517947)  
Regress: [mjsunit/regress/wasm/regress-727222.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-727222.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addMemory(0, 0, false);
builder.addFunction('f', kSig_i_v)
    .addBody([kExprMemorySize, kMemoryZero])
    .exportFunc();
var instance = builder.instantiate();
instance.exports.f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b5203e8^!)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=b5203e8)  
[test/mjsunit/regress/wasm/regression-727222.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-727222.js?cl=b5203e8)  
  

---   

## **regress-crbug-724153.js (chromium issue)**  
   
**[Issue 724153:
 CHECK failure: val <= std::min(static_cast<size_t>(std::numeric_limits<N>::max()), static_cast<](https://crbug.com/724153)**  
**[Commit: [turbofan] Fix value output count range on Operator.](https://chromium.googlesource.com/v8/v8/+/f7f03da)**  
  
Date(Commit): Mon May 29 15:49:06 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/517489](https://chromium-review.googlesource.com/517489)  
Regress: [mjsunit/regress/regress-crbug-724153.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-724153.js)  
```javascript
(function TestParameterLimit() {
  var src = '(function f(a,';
  for (var i = 0; i < 65534 - 2; i++) {
    src += 'b' + i + ',';
  }
  src += 'c) { return a + c })';
  var f = eval(src);
  assertEquals(NaN, f(1));
  assertEquals(NaN, f(2));
  %OptimizeFunctionOnNextCall(f);
  assertEquals(NaN, f(3));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f7f03da^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=f7f03da)  
[src/compiler/bytecode-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.h?cl=f7f03da)  
[src/compiler/operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.cc?cl=f7f03da)  
[src/compiler/operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.h?cl=f7f03da)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=f7f03da)  
...  
  

---   

## **regress-727219.js (chromium issue)**  
   
**[Issue 727219:
 CHECK failure: deopt_data->get(this_idx)->IsUndefined(isolate) in wasm-module.cc](https://crbug.com/727219)**  
**[Commit: [asm] Fix reusing code with annotated export info](https://chromium.googlesource.com/v8/v8/+/14fae58)**  
  
Date(Commit): Mon May 29 12:33:57 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/517945](https://chromium-review.googlesource.com/517945)  
Regress: [mjsunit/regress/wasm/regress-727219.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-727219.js)  
```javascript
function asm() {
  "use asm";
  function f(a) {
    a = a | 0;
    tab[a & 0]() | 0;
  }
  function unused() {
    return 0;
  }
  var tab = [ unused ];
  return f;
}

asm();
gc();
asm();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14fae58^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=14fae58)  
[test/mjsunit/regress/wasm/regression-727219.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-727219.js?cl=14fae58)  
  

---   

## **regress-crbug-725537.js (chromium issue)**  
   
**[Issue 725537:
 CHECK failure: map()->is_callable() in objects-debug.cc](https://crbug.com/725537)**  
**[Commit: [runtime] Set proper initial map for AsyncFunction constructor.](https://chromium.googlesource.com/v8/v8/+/397afc6)**  
  
Date(Commit): Fri May 26 21:06:48 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/517087](https://chromium-review.googlesource.com/517087)  
Regress: [mjsunit/regress/regress-crbug-725537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-725537.js)  
```javascript
const AsyncFunction = async function(){}.constructor;
class MyAsync extends AsyncFunction {}
var af = new MyAsync();
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/397afc6^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=397afc6)  
[test/mjsunit/regress/regress-crbug-725537.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-725537.js?cl=397afc6)  
  

---   

## **regress-726636.js (chromium issue)**  
   
**[Issue 726636:
 Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/726636)**  
**[Commit: [Promise] Add smi check for species constructor](https://chromium.googlesource.com/v8/v8/+/6b31174)**  
  
Date(Commit): Fri May 26 11:18:37 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-59", "M-60", "NodeJS-Backport-Rejected", "merge-merged-5.9", "merge-merged-6.0", "Release-1-M59"]  
Code Review: [https://chromium-review.googlesource.com/516734](https://chromium-review.googlesource.com/516734)  
Regress: [mjsunit/regress/regress-726636.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-726636.js)  
```javascript
Object.defineProperty(Promise, Symbol.species, { value: 0 });
var p = new Promise(function() {});
try {
  p.then();
  assertUnreachable();
} catch(e) {
  assertTrue(e instanceof TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b31174^!)  
[src/builtins/builtins-promise-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-promise-gen.cc?cl=6b31174)  
[test/mjsunit/regress/regress-726636.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-726636.js?cl=6b31174)  
  

---   

## **regress-6322.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6322)**  
**[Commit: [test] add mjsunit regression tests for v8:6322](https://chromium.googlesource.com/v8/v8/+/cd778f1)**  
  
Date(Commit): Wed May 24 19:06:26 2017  
Type: None  
Code Review: [https://chromium-review.googlesource.com/514230](https://chromium-review.googlesource.com/514230)  
Regress: [mjsunit/harmony/regress/regress-6322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-6322.js)  
```javascript
(async function() { for await (let { a = class b { } } of [{}]) { } })();
(async function() { var a; for await ({ a = class b { } } of [{}]) { } })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd778f1^!)  
[test/mjsunit/es6/regress/regress-6322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-6322.js?cl=cd778f1)  
[test/mjsunit/harmony/regress/regress-6322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-6322.js?cl=cd778f1)  
  

---   

## **regress-6322.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6322)**  
**[Commit: [test] add mjsunit regression tests for v8:6322](https://chromium.googlesource.com/v8/v8/+/cd778f1)**  
  
Date(Commit): Wed May 24 19:06:26 2017  
Type: None  
Code Review: [https://chromium-review.googlesource.com/514230](https://chromium-review.googlesource.com/514230)  
Regress: [mjsunit/es6/regress/regress-6322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-6322.js)  
```javascript
(function*() { for (let { a = class b { } } of [{}]) { } })().next();
(function() { for (let { a = class b { } } of [{}]) { } })();
(function() { var a; for ({ a = class b { } } of [{}]) { } })();

(function() { for (let [a = class b { } ] = [[]]; ;) break; })();
(function() { var a; for ([a = class b { } ] = [[]]; ;) break; })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd778f1^!)  
[test/mjsunit/es6/regress/regress-6322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-6322.js?cl=cd778f1)  
[test/mjsunit/harmony/regress/regress-6322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-6322.js?cl=cd778f1)  
  

---   

## **regress-crbug-725201.js (chromium issue)**  
   
**[Issue 725201:
 CHECK failure: fixed_array->IsDictionary() in objects-inl.h](https://crbug.com/725201)**  
**[Commit: [literals] Set the proper Map on the elements store for object literals](https://chromium.googlesource.com/v8/v8/+/106226e)**  
  
Date(Commit): Wed May 24 14:44:13 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/514004](https://chromium-review.googlesource.com/514004)  
Regress: [mjsunit/regress/regress-crbug-725201.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-725201.js)  
```javascript
function __f_1() {
  function __f_2() {
    Array.prototype.__proto__ = { 77e4  : null };
  }
  __f_2();
  %OptimizeFunctionOnNextCall(__f_2);
  __f_2();
}
try {
__f_1();
} catch(e) {; }
for (var __v_6 in [(1.2)]) {  }

%HeapObjectVerify([(1.2)]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/106226e^!)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=106226e)  
[test/mjsunit/regress/regress-crbug-725201.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-725201.js?cl=106226e)  
  

---   

## **regress-crbug-719384.js (chromium issue)**  
   
**[Issue 719384:
 CHECK failure: !isolate->has_pending_exception() in compiler.cc](https://crbug.com/719384)**  
**[Commit: [asm.js] Ensure lookups of imports are non-observable.](https://chromium.googlesource.com/v8/v8/+/ea48d83)**  
  
Date(Commit): Tue May 23 10:42:43 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/509552](https://chromium-review.googlesource.com/509552)  
Regress: [mjsunit/regress/regress-crbug-719384.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-719384.js)  
```javascript
(function TestThrowingObserver() {
  function Module(stdlib, foreign) {
    "use asm";
    var x = foreign.x | 0;
    function f() {}
    return { f:f };
  }
  var observer = { get x() { throw new Error() } };
  assertThrows(() => Module(this, observer));
  assertFalse(%IsAsmWasmCode(Module));
})();

(function TestMutatingObserver() {
  function Module(stdlib, foreign) {
    "use asm";
    var x = +foreign.x;
    var PI = stdlib.Math.PI;
    function f() {
      return +(PI + x);
    }
    return { f:f };
  }
  var stdlib = { Math : { PI : Math.PI } };
  var observer = { get x() { stdlib.Math.PI = 23; return 42; } };
  var m = Module(stdlib, observer);
  assertFalse(%IsAsmWasmCode(Module));
  assertEquals(65, m.f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea48d83^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=ea48d83)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=ea48d83)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=ea48d83)  
[test/mjsunit/asm/global-imports.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/global-imports.js?cl=ea48d83)  
[test/mjsunit/regress/regress-crbug-719384.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-719384.js?cl=ea48d83)  
...  
  

---   

## **regress-724851.js (chromium issue)**  
   
**[Issue 724851:
 CHECK failure: !thrower.error() in wasm-module.cc](https://crbug.com/724851)**  
**[Commit: [wasm] Validate function bodies for lazy compilation](https://chromium.googlesource.com/v8/v8/+/70a43f4)**  
  
Date(Commit): Tue May 23 09:47:56 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/509574](https://chromium-review.googlesource.com/509574)  
Regress: [mjsunit/regress/wasm/regress-724851.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-724851.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let builder = new WasmModuleBuilder();
builder.addFunction('f', kSig_i_v).addBody([kExprReturn]);
assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70a43f4^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=70a43f4)  
[test/mjsunit/regress/wasm/regression-724851.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-724851.js?cl=70a43f4)  
  

---   

## **regress-724846.js (chromium issue)**  
   
**[Issue 724846:
 CHECK failure: new_memory->byte_length()->ToUint32(&mem_size) in wasm-debug.cc](https://crbug.com/724846)**  
**[Commit: [wasm] Stricter max memory check](https://chromium.googlesource.com/v8/v8/+/a5449b0)**  
  
Date(Commit): Mon May 22 14:28:11 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/509255](https://chromium-review.googlesource.com/509255)  
Regress: [mjsunit/regress/wasm/regress-724846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-724846.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');


let builder = new WasmModuleBuilder();
const num_pages = 49152;
builder.addMemory(num_pages, num_pages);
assertThrows(() => builder.instantiate(), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5449b0^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=a5449b0)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=a5449b0)  
[test/mjsunit/regress/wasm/regression-724846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-724846.js?cl=a5449b0)  
  

---   

## **regress-crbug-722348.js (chromium issue)**  
   
**[Issue 722348:
 V8 correctness failure in configs: x64,ignition:x64,ignition_asm](https://crbug.com/722348)**  
**[Commit: [asm.js] Properly handle unused function imports.](https://chromium.googlesource.com/v8/v8/+/d813f46)**  
  
Date(Commit): Mon May 22 11:41:07 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/509450](https://chromium-review.googlesource.com/509450)  
Regress: [mjsunit/regress/regress-crbug-722348.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-722348.js)  
```javascript
function Module(global, env) {
  "use asm";
  var unused_fun = env.fun;
  function f() {}
  return { f:f }
}
assertThrows(() => Module(), TypeError);
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d813f46^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=d813f46)  
[test/mjsunit/regress/regress-crbug-722348.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-722348.js?cl=d813f46)  
  

---   

## **regress-crbug-715455.js (chromium issue)**  
   
**[Issue 715455:
 V8 correctness failure in configs: x64,ignition:x64,ignition_asm](https://crbug.com/715455)**  
**[Commit: [asm.js] Fix excessive function table sizes.](https://chromium.googlesource.com/v8/v8/+/a621462)**  
  
Date(Commit): Fri May 19 14:14:17 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/509530](https://chromium-review.googlesource.com/509530)  
Regress: [mjsunit/regress/regress-crbug-715455.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-715455.js)  
```javascript
function MODULE() {
  "use asm";
  function f() {
    bogus_function_table[0 & LIMIT]();
  }
  return { f:f };
}

var bogus_function_table = [ Object ];
var test_set = [ 0x3fffffff, 0x7fffffff, 0xffffffff ];
for (var i = 0; i < test_set.length; ++i) {
  bogus_function_table[i] = Object;
  var src = MODULE.toString();
  src = src.replace(/MODULE/g, "Module" + i);
  src = src.replace(/LIMIT/g, test_set[i]);
  var module = eval("(" + src + ")");
  assertDoesNotThrow(module(this).f());
  assertFalse(%IsAsmWasmCode(module));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a621462^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=a621462)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=a621462)  
[src/wasm/wasm-module-builder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module-builder.cc?cl=a621462)  
[test/mjsunit/regress/regress-crbug-715455.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-715455.js?cl=a621462)  
  

---   

## **regress-crbug-723132.js (chromium issue)**  
   
**[Issue 723132:
 Inconsistent binding of "this" in inline arrow function within a generator.](https://crbug.com/723132)**  
**[Commit: [parser] Stop treating generators as "top level" for preparsing purposes](https://chromium.googlesource.com/v8/v8/+/0439100)**  
  
Date(Commit): Thu May 18 16:24:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://chromium-review.googlesource.com/508055](https://chromium-review.googlesource.com/508055)  
Regress: [mjsunit/regress/regress-crbug-723132.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-723132.js)  
```javascript
function outer() {
  function* generator() {
    let arrow = () => {
      assertSame(expectedReceiver, this);
      assertEquals(42, arguments[0]);
    };
    arrow();
  }
  generator.call(this, 42).next();
}
let expectedReceiver = {};
outer.call(expectedReceiver);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0439100^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=0439100)  
[test/mjsunit/regress/regress-crbug-723132.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-723132.js?cl=0439100)  
  

---   

## **regress-crbug-723455.js (chromium issue)**  
   
**[Issue 723455:
 CHECK failure: !map->is_stable() in access-info.cc](https://crbug.com/723455)**  
**[Commit: [turbofan][crankshaft] Don't generate elements kind transitions from stable maps.](https://chromium.googlesource.com/v8/v8/+/ea55b87)**  
  
Date(Commit): Wed May 17 21:58:44 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/483442](https://chromium-review.googlesource.com/483442)  
Regress: [mjsunit/regress/regress-crbug-723455.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-723455.js)  
```javascript
function f(a) {
  a.x = 0;
  a[0] = 0.1;
  a.x = {};
}

f(new Array(1));
f(new Array(1));
f(new Array());

%OptimizeFunctionOnNextCall(f);
f(new Array(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea55b87^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=ea55b87)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=ea55b87)  
[test/mjsunit/regress/regress-crbug-723455.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-723455.js?cl=ea55b87)  
  

---   

## **regress-crbug-722756.js (chromium issue)**  
   
**[Issue 722756:
 Type Confusion In Chrome Lead to RCE](https://crbug.com/722756)**  
**[Commit: [crankshaft] Fix HAliasAnalyzer for constants](https://chromium.googlesource.com/v8/v8/+/e33fd30)**  
  
Date(Commit): Wed May 17 13:11:02 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["Security_Impact-Stable", "Arch-x86_64", "Security_Severity-High", "reward-7500", "allpublic", "reward-inprocess", "M-59", "Merge-Rejected-58", "Via-Wizard-Security", "merge-merged-5.9", "Release-0-M59", "CVE-2017-5070", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/507209](https://chromium-review.googlesource.com/507209)  
Regress: [mjsunit/regress/regress-crbug-722756.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-722756.js)  
```javascript
var array = [[{}], [1.1]];

function transition() {
  for(var i = 0; i < array.length; i++){
    var arr = array[i];
    arr[0] = {};
  }
}

var double_arr2 = [1.1,2.2];

var flag = 0;
function swap() {
  try {} catch(e) {}  // Prevent Crankshaft from inlining this.
  if (flag == 1) {
    array[1] = double_arr2;
  }
}

var expected = 6.176516726456e-312;
function f(){
  swap();
  double_arr2[0] = 1;
  transition();
  double_arr2[1] = expected;
}

for(var i = 0; i < 3; i++) {
  f();
}
%OptimizeFunctionOnNextCall(f);
flag = 1;
f();
assertEquals(expected, double_arr2[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e33fd30^!)  
[src/crankshaft/hydrogen-alias-analysis.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-alias-analysis.h?cl=e33fd30)  
[test/mjsunit/regress/regress-crbug-722756.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-722756.js?cl=e33fd30)  
  

---   

## **regress-722978.js (chromium issue)**  
   
**[Issue 722978:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/722978)**  
**[Commit: Reland "[compiler] Delay allocation of heap numbers for deoptimization literals."](https://chromium.googlesource.com/v8/v8/+/789b604)**  
  
Date(Commit): Wed May 17 12:20:38 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/507207](https://chromium-review.googlesource.com/507207)  
Regress: [mjsunit/regress/regress-722978.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-722978.js)  
```javascript
var __v_3 = {};
function __f_0() {
  var __v_30 = -0;
  __v_30.__defineGetter__("0", function() { return undefined; });
  __v_30 = 0;
  __v_3 = 0;
  assertTrue(Object.is(0, __v_30));
}
__f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/789b604^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=789b604)  
[src/compiler/code-generator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.h?cl=789b604)  
[test/mjsunit/regress/regress-722978.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-722978.js?cl=789b604)  
  

---   

## **regress-722445.js (chromium issue)**  
   
**[Issue 722445:
 CHECK failure: AllowHeapAllocation::IsAllowed() in heap.cc](https://crbug.com/722445)**  
**[Commit: [wasm] Check for illegal br table count](https://chromium.googlesource.com/v8/v8/+/74519c4)**  
  
Date(Commit): Wed May 17 09:46:46 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "M-60", "Test-Predator-Wrong-CLs"]  
Code Review: [https://chromium-review.googlesource.com/505496](https://chromium-review.googlesource.com/505496)  
Regress: [mjsunit/regress/wasm/regress-722445.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-722445.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var builder = new WasmModuleBuilder();
builder.addFunction('f', kSig_v_v).addBody([
  kExprI32Const, 0, kExprBrTable,
  // 0x80000000 in LEB:
  0x80, 0x80, 0x80, 0x80, 0x08,
  // First break target. Creation of this node triggered the bug.
  0
]);
assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/74519c4^!)  
[src/compiler/operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator.cc?cl=74519c4)  
[src/wasm/function-body-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder.cc?cl=74519c4)  
[test/mjsunit/regress/wasm/regression-722445.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-722445.js?cl=74519c4)  
  

---   

## **regress-719175.js (chromium issue)**  
   
**[Issue 719175:
 CHECK failure: Unknown or unimplemented opcode #204:f64.mod in wasm-interpreter.cc](https://crbug.com/719175)**  
**[Commit: [wasm] Don't try to interpret asm.js modules](https://chromium.googlesource.com/v8/v8/+/a68b75d)**  
  
Date(Commit): Wed May 17 09:38:06 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/506160](https://chromium-review.googlesource.com/506160)  
Regress: [mjsunit/regress/wasm/regress-719175.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-719175.js)  
```javascript
function asm() {
  'use asm';
  function f() {
    if (1.0 % 2.5 == -0.75) {
    }
    return 0;
  }
  return {f: f};
}
asm().f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a68b75d^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=a68b75d)  
[test/mjsunit/regress/wasm/regression-719175.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-719175.js?cl=a68b75d)  
  

---   

## **regress-crbug-721835.js (chromium issue)**  
   
**[Issue 721835:
 CHECK failure: !failed_ in asm-parser.cc](https://crbug.com/721835)**  
**[Commit: [asm.js] Fix evaluation of first for-statement expression.](https://chromium.googlesource.com/v8/v8/+/f2b9c50)**  
  
Date(Commit): Mon May 15 13:19:49 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/506148](https://chromium-review.googlesource.com/506148)  
Regress: [mjsunit/regress/regress-crbug-721835.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-721835.js)  
```javascript
(function TestValidationFailureInForStatement() {
  function Module() {
    "use asm"
    function f() {
      var a = 0;
      for (a = b; 0; 0) {};
      return 0;
    }
    return { f:f };
  }
  assertThrows(() => Module().f(), ReferenceError);
  assertFalse(%IsAsmWasmCode(Module));
})();

(function TestForStatementInVoidFunction() {
  function Module() {
    "use asm"
    function f() {
      for (1; 0; 0) {};
    }
    return { f:f };
  }
  assertDoesNotThrow(() => Module().f());
  assertTrue(%IsAsmWasmCode(Module));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f2b9c50^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=f2b9c50)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=f2b9c50)  
[test/mjsunit/regress/regress-crbug-721835.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-721835.js?cl=f2b9c50)  
  

---   

## **regress-719380.js (chromium issue)**  
   
**[Issue 719380:
 CHECK failure: !isolate_->external_caught_exception() in api.cc](https://crbug.com/719380)**  
**[Commit: [error] Clear external_caught_exception in Error formatting](https://chromium.googlesource.com/v8/v8/+/f9c4fc0)**  
  
Date(Commit): Thu May 11 06:35:53 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2870423002](https://codereview.chromium.org/2870423002)  
Regress: [mjsunit/regress/regress-719380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-719380.js)  
```javascript
TypeError.prototype.__defineGetter__("name", () => { throw 42; });
try { console.log({ toString: () => { throw new TypeError() }}); } catch (e) {}
try { new WebAssembly.Table({}); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f9c4fc0^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=f9c4fc0)  
[test/mjsunit/regress/regress-719380.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-719380.js?cl=f9c4fc0)  
  

---   

## **regress-718891.js (chromium issue)**  
   
**[No Permission](https://crbug.com/718891)**  
**[Commit: Reland: [TypeFeedbackVector] Store optimized code in the vector](https://chromium.googlesource.com/v8/v8/+/11a211f)**  
  
Date(Commit): Wed May 10 15:04:35 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/500134](https://chromium-review.googlesource.com/500134)  
Regress: [mjsunit/regress/regress-718891.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-718891.js)  
```javascript
function Data() {
}
Data.prototype = { x: 1 };

function TriggerDeopt() {
  Data.prototype = { x: 2 };
}

function TestDontSelfHealWithDeoptedCode(run_unoptimized, ClosureFactory) {
  // Create some function closures which don't have
  // optimized code.
  var unoptimized_closure = ClosureFactory();
  if (run_unoptimized) {
    unoptimized_closure();
  }

  // Run and optimize the code (do this in a separate function
  // so that the closure doesn't leak in a dead register).
  (() => {
    var optimized_closure = ClosureFactory();
    // Use .call to avoid the CallIC retaining the JSFunction in the
    // feedback vector via a weak map, which would mean it wouldn't be
    // collected in the minor gc below.
    optimized_closure.call(undefined);
    %OptimizeFunctionOnNextCall(optimized_closure);
    optimized_closure.call(undefined);
  })();

  // Optimize a dummy function, just so it gets linked into the
  // Contexts optimized_functions list head, which is in the old
  // space, and the link from to the optimized_closure's JSFunction
  // moves to the inline link in dummy's JSFunction in the new space,
  // otherwise optimized_closure's JSFunction will be retained by the
  // old->new remember set.
  (() => {
    var dummy = function() { return 1; };
    %OptimizeFunctionOnNextCall(dummy);
    dummy();
  })();

  // GC the optimized closure with a minor GC - the optimized
  // code will remain in the feedback vector.
  gc(true);

  // Trigger deoptimization by changing the prototype of Data. This
  // will mark the code for deopt, but since no live JSFunction has
  // optimized code, we won't clear the feedback vector.
  TriggerDeopt();

  // Call pre-existing functions, these will try to self-heal with the
  // optimized code in the feedback vector op, but should bail-out
  // since the code is marked for deoptimization.
  unoptimized_closure();
}

TestDontSelfHealWithDeoptedCode(false,
        () => { return () => { return new Data() }});
TestDontSelfHealWithDeoptedCode(true,
        () => { return () => { return new Data() }});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11a211f^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=11a211f)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=11a211f)  
[src/builtins/builtins-constructor-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-constructor-gen.cc?cl=11a211f)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=11a211f)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=11a211f)  
...  
  

---   

## **regress-crbug-719479.js (chromium issue)**  
   
**[Issue 719479:
 CHECK failure: LoadElement of kRepFloat64 (NumberOrHole) cannot be changed to kRepTagged in rep](https://crbug.com/719479)**  
**[Commit: [turbofan] Don't mix element accesses with incompatible representations.](https://chromium.googlesource.com/v8/v8/+/d412cad)**  
  
Date(Commit): Tue May 09 10:16:13 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2866233002](https://codereview.chromium.org/2866233002)  
Regress: [mjsunit/regress/regress-crbug-719479.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-719479.js)  
```javascript
function baz(a, b) {
  for (var i = 0; i < a.length; i++) {
    if (a[i], b[i]) return false;
  }
}
function bar(expected, found) {
  if (!baz(found, expected)) {
  }
};
bar([{}, 6, NaN], [1.8, , NaN]);
function foo() {
  var a = [1,2,3,4];
  bar(a.length, a.length);
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d412cad^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=d412cad)  
[src/compiler/load-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.h?cl=d412cad)  
[test/mjsunit/regress/regress-crbug-719479.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-719479.js?cl=d412cad)  
  

---   

## **regress-crbug-718779.js (chromium issue)**  
   
**[Issue 718779:
 CHECK failure: !new_map->IsUnboxedDoubleField(index) in objects.cc](https://crbug.com/718779)**  
**[Commit: [runtime] MigrateFastToFast: fix check for unboxed inobject doubles](https://chromium.googlesource.com/v8/v8/+/ceba405)**  
  
Date(Commit): Fri May 05 22:23:04 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2861093004](https://codereview.chromium.org/2861093004)  
Regress: [mjsunit/regress/regress-crbug-718779.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-718779.js)  
```javascript
function __f_1()
{
    __v_1.p2 = 2147483648;
    __v_1.p3 = 3;
    __v_1.p4 = 4;
    __v_1.p5 = 2147483648;
    __v_1.p6 = 6;
}
function __f_2()
{
    delete __v_1.p6;
    delete __v_1.p5;
}
var __v_1 = { };
__f_1(__v_1);
__f_2(__v_1);
__f_1(__v_1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ceba405^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ceba405)  
[test/mjsunit/regress/regress-crbug-718779.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-718779.js?cl=ceba405)  
  

---   

## **regress-718745.js (chromium issue)**  
   
**[Issue 718745:
 <unreachable> in AsmJsParser::GetVarInfo (asm-parser.cc:228)](https://crbug.com/718745)**  
**[Commit: [asm.js] Fix checking of "fround" in parameter annotation.](https://chromium.googlesource.com/v8/v8/+/2ed278f)**  
  
Date(Commit): Fri May 05 12:45:53 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/497469](https://chromium-review.googlesource.com/497469)  
Regress: [mjsunit/asm/regress-718745.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-718745.js)  
```javascript
function Module(stdlib) {
  "use asm";
  var fround = stdlib.Math.fround;
  function f(a) {
    a = (fround(a));
  }
  return { f:f };
}
Module(this).f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ed278f^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=2ed278f)  
[test/mjsunit/asm/regress-718745.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-718745.js?cl=2ed278f)  
  

---   

## **regress-718285.js (chromium issue)**  
   
**[Issue 718285:
 CHECK failure: dest_data + dest_byte_length <= source_data || source_data + source_byte_length](https://crbug.com/718285)**  
**[Commit: [builtins] Use the byte_length for byte length, not byte_offset.](https://chromium.googlesource.com/v8/v8/+/4d611d1)**  
  
Date(Commit): Fri May 05 09:57:17 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "M-60", "ClusterFuzz-Verified", "Test-Predator-Correct-CLs"]  
Code Review: [https://chromium-review.googlesource.com/497727](https://chromium-review.googlesource.com/497727)  
Regress: [mjsunit/regress/regress-718285.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-718285.js)  
```javascript
function foo_reference(n) {
  var array = new Int32Array(n + 1);
  for (var i = 0; i < n; ++i) {
    array[i] = i;
  }
  var array2 = new Int32Array(array);
  array2.set(new Uint8Array(array.buffer, 0, n), 1);
  return array2;
}

function foo(n) {
  var array = new Int32Array(n + 1);
  for (var i = 0; i < n; ++i) {
    array[i] = i;
  }
  array.set(new Uint8Array(array.buffer, 0, n), 1);
  return array;
}

function bar_reference(n) {
  var array = new Int32Array(n + 1);
  for (var i = 0; i < n; ++i) {
    array[i] = i;
  }
  var array2 = new Int32Array(array);
  array2.set(new Uint8Array(array.buffer, 34), 0);
  return array2;
}

function bar(n) {
  var array = new Int32Array(n + 1);
  for (var i = 0; i < n; ++i) {
    array[i] = i;
  }
  array.set(new Uint8Array(array.buffer, 34), 0);
  return array;
}

foo(10);
foo_reference(10);
bar(10);
bar_reference(10);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4d611d1^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=4d611d1)  
[test/mjsunit/regress/regress-718285.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-718285.js?cl=4d611d1)  
  

---   

## **regress-6288.js (v8 issue)**  
   
**[Issue 6288:
 Locale reported as "und" for pt-BR and en-GB](https://crbug.com/v8/6288)**  
**[Commit: [intl] Use a service-dependent default locale](https://chromium.googlesource.com/v8/v8/+/5228af6)**  
  
Date(Commit): Thu May 04 18:46:00 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/492886](https://chromium-review.googlesource.com/492886)  
Regress: [mjsunit/regress/regress-6288.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6288.js)  
```javascript
if (this.Intl) {
  assertEquals('pt', Intl.Collator().resolvedOptions().locale);
  assertEquals('pt-BR', Intl.DateTimeFormat().resolvedOptions().locale);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5228af6^!)  
[src/js/intl.js](https://cs.chromium.org/chromium/src/v8/src/js/intl.js?cl=5228af6)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=5228af6)  
[test/mjsunit/regress/regress-6288.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6288.js?cl=5228af6)  
  

---   

## **regress-717194.js (chromium issue)**  
   
**[No Permission](https://crbug.com/717194)**  
**[Commit: [wasm] Avoid js-typed-lowering optimization for wasm Memory objects](https://chromium.googlesource.com/v8/v8/+/82503e9)**  
  
Date(Commit): Thu May 04 17:21:56 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2862763002](https://codereview.chromium.org/2862763002)  
Regress: [mjsunit/regress/wasm/regress-717194.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-717194.js)  
```javascript
PAGE_SIZE = 0x10000;
PAGES = 10;

memory = new WebAssembly.Memory({initial: PAGES});
buffer = memory.buffer;

var func = (function (stdlib, env, heap) {
    "use asm";

    var array = new stdlib.Int32Array(heap);

    return function () {
        array[0] = 0x41424344;
        array[1] = 0x45464748;
    }
}({Int32Array: Int32Array}, {}, buffer));

for (var i = 0; i < 1000; ++i)
    func();

memory.grow(1);

func();

for(var i = 0; i < 2; ++i)
    new ArrayBuffer(PAGE_SIZE * PAGES);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82503e9^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=82503e9)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=82503e9)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=82503e9)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=82503e9)  
[test/mjsunit/regress/wasm/regression-717194.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-717194.js?cl=82503e9)  
  

---   

## **regress-crbug-716520.js (chromium issue)**  
   
**[Issue 716520:
 Crash in v8::internal::JSObject::FastPropertyAt](https://crbug.com/716520)**  
**[Commit: Fix FastAssign for self-assignment](https://chromium.googlesource.com/v8/v8/+/1f51f66)**  
  
Date(Commit): Thu May 04 13:41:08 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2855133006](https://codereview.chromium.org/2855133006)  
Regress: [mjsunit/regress/regress-crbug-716520.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-716520.js)  
```javascript
var __v_0 = {};
var __v_8 = this;
var __v_11 = -1073741825;
__v_1 = this;
try {
} catch(e) {; }
  function __f_4() {}
  __f_4.prototype = __v_0;
  function __f_9() { return new __f_4().v; }
  __f_9(); __f_9();
try {
(function() {
})();
} catch(e) {; }
  Object.assign(__v_0, __v_1, __v_0);
(function() {
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f51f66^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1f51f66)  
[test/mjsunit/regress/regress-crbug-716520.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-716520.js?cl=1f51f66)  
  

---   

## **regress-crbug-716912.js (chromium issue)**  
   
**[Issue 716912:
 Crash in IsFlagSet](https://crbug.com/716912)**  
**[Commit: Move delete-last-fast-property code from CSA to C++](https://chromium.googlesource.com/v8/v8/+/6cb995b)**  
  
Date(Commit): Wed May 03 15:50:50 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2854373002](https://codereview.chromium.org/2854373002)  
Regress: [mjsunit/regress/regress-crbug-716912.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-716912.js)  
```javascript
function __f_6() {
this.a4 = {};
}
__v_6 = new __f_6();
__v_6.prototype = __v_6;
__v_6 = new __f_6();
gc();
gc();

buf = new ArrayBuffer(8);
__v_8 = new Int32Array(buf);
__v_9 = new Float64Array(buf);

__v_8[0] = 1;
__v_6.a4 = {a: 0};
delete __v_6.a4;
__v_6.boom = __v_9[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6cb995b^!)  
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=6cb995b)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=6cb995b)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=6cb995b)  
[test/mjsunit/regress/regress-crbug-714981.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-714981.js?cl=6cb995b)  
[test/mjsunit/regress/regress-crbug-716912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-716912.js?cl=6cb995b)  
  

---   

## **regress-crbug-714981.js (chromium issue)**  
   
**[Issue 714981:
 CHECK failure: map()->unused_property_fields() == actual_unused_property_fields - JSObject::kFi](https://crbug.com/714981)**  
**[Commit: Move delete-last-fast-property code from CSA to C++](https://chromium.googlesource.com/v8/v8/+/6cb995b)**  
  
Date(Commit): Wed May 03 15:50:50 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-60", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2854373002](https://codereview.chromium.org/2854373002)  
Regress: [mjsunit/regress/regress-crbug-714981.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-714981.js)  
```javascript
function addProperties(o)
{
    o.p1 = 1;
    o.p2 = 2;
    o.p3 = 3;
    o.p4 = 4;
    o.p5 = 5;
    o.p6 = 6;
    o.p7 = 7;
    o.p8 = 8;
}
function removeProperties(o)
{
    delete o.p8;
    delete o.p7;
    delete o.p6;
    delete o.p5;
}
function makeO()
{
    var o = { };
    addProperties(o);
    removeProperties(o);
    addProperties(o);
}
for (var i = 0; i < 3; ++i) {
    o = makeO();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6cb995b^!)  
[src/builtins/builtins-internal-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-internal-gen.cc?cl=6cb995b)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=6cb995b)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=6cb995b)  
[test/mjsunit/regress/regress-crbug-714981.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-714981.js?cl=6cb995b)  
[test/mjsunit/regress/regress-crbug-716912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-716912.js?cl=6cb995b)  
  

---   

## **regress-716044.js (chromium issue)**  
   
**[Issue 716044:
 V8: OOB write in Array.prototype.map builtin](https://crbug.com/716044)**  
**[Commit: Array.prototype.map write error.](https://chromium.googlesource.com/v8/v8/+/192984e)**  
  
Date(Commit): Wed May 03 14:11:44 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_Impact-Head", "Security_Severity-High", "allpublic", "M-60", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/2846963003](https://codereview.chromium.org/2846963003)  
Regress: [mjsunit/regress/regress-716044.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-716044.js)  
```javascript
class Array1 extends Array {
  constructor(len) {
      super(1);
    }
};

class MyArray extends Array {
  static get [Symbol.species]() {
      return Array1;
    }
}

a = new MyArray();

for (var i = 0; i < 100000; i++) {
  a.push(1);
}

a.map(function(x) { return 42; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/192984e^!)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=192984e)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=192984e)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=192984e)  
[test/mjsunit/regress/regress-716044.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-716044.js?cl=192984e)  
  

---   

## **regress-715216a.js (chromium issue)**  
   
**[Issue 715216:
 CHECK failure: (code_to_relocate.Find(old_code)) == nullptr in wasm-debug.cc](https://crbug.com/715216)**  
**[Commit: [wasm] Disallow lazy compilation with --wasm-interpret-all](https://chromium.googlesource.com/v8/v8/+/9c62795)**  
  
Date(Commit): Wed May 03 08:05:42 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-60", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/494486](https://chromium-review.googlesource.com/494486)  
Regress: [mjsunit/regress/wasm/regress-715216a.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-715216a.js), [mjsunit/regress/wasm/regress-715216b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-715216b.js)  
```javascript
function asm() {
  "use asm";
  function f() {}
  return {};
}
asm();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c62795^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=9c62795)  
[src/wasm/wasm-code-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-specialization.cc?cl=9c62795)  
[test/mjsunit/regress/wasm/regression-715216-a.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-715216-a.js?cl=9c62795)  
[test/mjsunit/regress/wasm/regression-715216-b.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-715216-b.js?cl=9c62795)  
  

---   

## **regress-6337.js (v8 issue)**  
   
**[Issue 6337:
 Fatal error "unreachable code" with spread in class](https://crbug.com/v8/6337)**  
**[Commit: [parser] Fix fatal error on spread in class properties](https://chromium.googlesource.com/v8/v8/+/e393093)**  
  
Date(Commit): Tue May 02 18:03:13 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/493786](https://chromium-review.googlesource.com/493786)  
Regress: [mjsunit/regress/regress-6337.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6337.js)  
```javascript
assertThrows(function() { eval(`class C { ...[] }`); } )  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e393093^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=e393093)  
[test/message/class-spread-property.js](https://cs.chromium.org/chromium/src/v8/test/message/class-spread-property.js?cl=e393093)  
[test/message/class-spread-property.out](https://cs.chromium.org/chromium/src/v8/test/message/class-spread-property.out?cl=e393093)  
[test/mjsunit/regress/regress-6337.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6337.js?cl=e393093)  
  

---   

## **regress-717056.js (chromium issue)**  
   
**[Issue 717056:
 Ill in v8::internal::wasm::ErrorThrower::Reify](https://crbug.com/717056)**  
**[Commit: [wasm] Fix usages of ErrorThrower::Reify](https://chromium.googlesource.com/v8/v8/+/24a0987)**  
  
Date(Commit): Tue May 02 15:11:36 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-60"]  
Code Review: [https://chromium-review.googlesource.com/493506](https://chromium-review.googlesource.com/493506)  
Regress: [mjsunit/regress/wasm/regress-717056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-717056.js)  
```javascript
function asm() {
  'use asm';
  return {};
}

function rec() {
  asm();
  rec();
}
assertThrows(() => rec(), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/24a0987^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=24a0987)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=24a0987)  
[src/wasm/wasm-result.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-result.cc?cl=24a0987)  
[src/wasm/wasm-result.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-result.h?cl=24a0987)  
[test/mjsunit/regress/wasm/regression-717056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-717056.js?cl=24a0987)  
  

---   

## **regress-crbug-716804.js (chromium issue)**  
   
**[Issue 716804:
 CHECK failure: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/716804)**  
**[Commit: [ic] Fix handling of JSArray.length accessor info.](https://chromium.googlesource.com/v8/v8/+/26cf06b)**  
  
Date(Commit): Tue May 02 08:55:51 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/493146](https://chromium-review.googlesource.com/493146)  
Regress: [mjsunit/regress/regress-crbug-716804.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-716804.js)  
```javascript
var v = [];
v.__proto__ = function() {};
v.prototype;

var v = [];
v.__proto__ = new Error();
v.stack;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/26cf06b^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=26cf06b)  
[test/mjsunit/regress/regress-crbug-716804.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-716804.js?cl=26cf06b)  
  

---   

## **regress-709782.js (chromium issue)**  
   
**[Issue 709782:
 A math operation is giving an NaN result -- but only when run a certain number of times within a loop.](https://crbug.com/709782)**  
**[Commit: [turbofan] Avoid going through ArgumentsAdaptorTrampoline for select CSA/C++ builtins](https://chromium.googlesource.com/v8/v8/+/6803562)**  
  
Date(Commit): Sat Apr 29 07:36:10 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Regression  
Labels: ["M-59", "has-bisect-per-revision", "TE-Verified-M59", "merge-merged-5.9", "TE-Verified-59.0.3071.9"]  
Code Review: [https://codereview.chromium.org/2829093004](https://codereview.chromium.org/2829093004)  
Regress: [mjsunit/regress/regress-709782.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-709782.js)  
```javascript
var a = [0];
function bar(x) { return x; }
function foo() { return a.reduce(bar); }

assertEquals(0, foo());
assertEquals(0, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(0, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6803562^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=6803562)  
[src/builtins/builtins-array-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array-gen.cc?cl=6803562)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=6803562)  
[src/code-factory.cc](https://cs.chromium.org/chromium/src/v8/src/code-factory.cc?cl=6803562)  
[src/code-factory.h](https://cs.chromium.org/chromium/src/v8/src/code-factory.h?cl=6803562)  
...  
  

---   

## **regress-crbug-715862.js (chromium issue)**  
   
**[Issue 715862:
 CHECK failure: !map->is_stable() in hydrogen.cc](https://crbug.com/715862)**  
**[Commit: [ic] Filter out deprecated maps from polymorphic keyed ICs.](https://chromium.googlesource.com/v8/v8/+/0655ee8)**  
  
Date(Commit): Fri Apr 28 10:02:20 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/490106](https://chromium-review.googlesource.com/490106)  
Regress: [mjsunit/regress/regress-crbug-715862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-715862.js)  
```javascript
function f(a) {
  a.x = 0;
  a[1] = 0.1;
  a.x = {};
}

f(new Array(1));
f(new Array());

%OptimizeFunctionOnNextCall(f);
f(new Array(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0655ee8^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=0655ee8)  
[test/mjsunit/regress/regress-crbug-715862.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-715862.js?cl=0655ee8)  
  

---   

## **regress-715651.js (chromium issue)**  
   
**[No Permission](https://crbug.com/715651)**  
**[Commit: [turbofan] Fix impossible type handling for TypeGuard and BooleanNot.](https://chromium.googlesource.com/v8/v8/+/ff2109d)**  
  
Date(Commit): Thu Apr 27 11:35:15 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2848583002](https://codereview.chromium.org/2848583002)  
Regress: [mjsunit/compiler/regress-715651.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-715651.js)  
```javascript
function f() {
  this.x = 1;
}

var a = [];

for (let i = 0; i < 100; i++) {
  new f();
}

function h() {
  // Create a new object and add an out-of-object property 'y'.
  var o = new f();
  o.y = 1.5;
  return o;
}

function g(o) {
  // Add more properties so that we trigger extension of out-ot-object
  // property store.
  o.u = 1.1;
  o.v = 1.2;
  o.z = 1.3;
  // Return a field from the out-of-object-property store.
  return o.y;
}

g(h());
g(h());
%OptimizeFunctionOnNextCall(g);
assertEquals(1.5, g(h()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ff2109d^!)  
[src/compiler/access-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-builder.cc?cl=ff2109d)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=ff2109d)  
[test/mjsunit/compiler/regress-715204.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-715204.js?cl=ff2109d)  
[test/mjsunit/compiler/regress-715651.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-715651.js?cl=ff2109d)  
  

---   

## **regress-715582.js (chromium issue)**  
   
**[Issue 715582:
 Security: Out of bound read in FindSharedFunctionInfo (V8)](https://crbug.com/715582)**  
**[Commit: Add missing early-bailouts in ast traversal visitors](https://chromium.googlesource.com/v8/v8/+/4e78b5a)**  
  
Date(Commit): Thu Apr 27 10:47:37 2017  
Components/Type: Blink>JavaScript>Compiler/Bug-Security  
Labels: ["reward-3000", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "reward-inprocess", "M-59", "merge-merged-5.9", "Release-0-M59", "CVE-2017-5071", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/487983](https://chromium-review.googlesource.com/487983)  
Regress: [mjsunit/regress/regress-715582.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-715582.js)  
```javascript
this.__defineGetter__(
  "x", (a = (function f() { return; (function() {}); })()) => { });
x;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e78b5a^!)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=4e78b5a)  
[src/ast/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-numbering.cc?cl=4e78b5a)  
[test/mjsunit/regress/regress-715582.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-715582.js?cl=4e78b5a)  
  

---   

## **regress-crbug-714872.js (chromium issue)**  
   
**[Issue 714872:
 6.5%-107.1% regression in page_cycler_v2.intl_es_fr_pt-BR at 465627:465860](https://crbug.com/714872)**  
**[Commit: Revert behavioral part of 84dc8ed4c3e6c8c1e3005b2d2445c64328b139a4](https://chromium.googlesource.com/v8/v8/+/86aa796)**  
  
Date(Commit): Wed Apr 26 20:56:30 2017  
Components/Type: None/Bug-Regression  
Labels: ["Performance-Sheriff", "M-60"]  
Code Review: [https://chromium-review.googlesource.com/486130](https://chromium-review.googlesource.com/486130)  
Regress: [mjsunit/regress/regress-crbug-714872.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-714872.js)  
```javascript
function f() {}
f.prototype = 1;
f.foo = 1;
f.prototype = {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/86aa796^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=86aa796)  
[test/mjsunit/regress/regress-crbug-714872.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-714872.js?cl=86aa796)  
  

---   

## **regress-6298.js (v8 issue)**  
   
**[Issue 6298:
 [asm.js] Scanner truncates all numeric literals to 32-bit](https://crbug.com/v8/6298)**  
**[Commit: [asm.js] Fix numeric literal bounds checking.](https://chromium.googlesource.com/v8/v8/+/e2accb4)**  
  
Date(Commit): Wed Apr 26 13:45:45 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/486881](https://chromium-review.googlesource.com/486881)  
Regress: [mjsunit/regress/regress-6298.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6298.js)  
```javascript
function Module(stdlib, imports, buffer) {
  "use asm";
  function f() {
    return (281474976710655 * 1048575) | 0;
  }
  return { f:f };
}
var m = Module(this);
assertEquals(-1048576, m.f());
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2accb4^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=e2accb4)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=e2accb4)  
[src/asmjs/asm-scanner.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.cc?cl=e2accb4)  
[src/asmjs/asm-scanner.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.h?cl=e2accb4)  
[test/mjsunit/regress/regress-6298.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6298.js?cl=e2accb4)  
...  
  

---   

## **regress-crbug-715404.js (chromium issue)**  
   
**[Issue 715404:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/715404)**  
**[Commit: [turbofan] Fix lowering of Array constructor with one argument.](https://chromium.googlesource.com/v8/v8/+/d06d4ce)**  
  
Date(Commit): Wed Apr 26 12:02:12 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2838123004](https://codereview.chromium.org/2838123004)  
Regress: [mjsunit/regress/regress-crbug-715404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-715404.js)  
```javascript
function foo() { Array(-1); }
assertThrows(foo, RangeError);
assertThrows(foo, RangeError);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d06d4ce^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=d06d4ce)  
[test/mjsunit/regress/regress-crbug-715404.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-715404.js?cl=d06d4ce)  
  

---   

## **regress-715204.js (chromium issue)**  
   
**[Issue 715204:
 CHECK failure: CanBeTaggedPointer(input_info->representation()) in simplified-lowering.cc](https://crbug.com/715204)**  
**[Commit: [turbofan] Fix impossible type handling for TypeGuard and BooleanNot.](https://chromium.googlesource.com/v8/v8/+/9c47a06)**  
  
Date(Commit): Wed Apr 26 10:27:12 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "M-60", "ClusterFuzz-Verified", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2836203004](https://codereview.chromium.org/2836203004)  
Regress: [mjsunit/compiler/regress-715204.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-715204.js)  
```javascript
var global = true;
global = false;

function f() {
  global = 1;
  return !global;
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c47a06^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=9c47a06)  
[test/mjsunit/compiler/regress-715204.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-715204.js?cl=9c47a06)  
  

---   

## **regress-crbug-715151.js (chromium issue)**  
   
**[Issue 715151:
 CHECK failure: (map()->has_fast_smi_or_object_elements() || (elements() == GetHeap()->empty_fix](https://crbug.com/715151)**  
**[Commit: [turbofan] Fix buggy implicit coercion in GetMapWitness.](https://chromium.googlesource.com/v8/v8/+/e913f9e)**  
  
Date(Commit): Wed Apr 26 09:57:36 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2839873004](https://codereview.chromium.org/2839873004)  
Regress: [mjsunit/regress/regress-crbug-715151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-715151.js)  
```javascript
function foo() {
  var a = [0];
  Object.preventExtensions(a);
  return a.pop();
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e913f9e^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=e913f9e)  
[test/mjsunit/regress/regress-crbug-715151.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-715151.js?cl=e913f9e)  
  

---   

## **regress-713367.js (chromium issue)**  
   
**[No Permission](https://crbug.com/713367)**  
**[Commit: [turbofan] escape analysis: patch for wrong deopt info](https://chromium.googlesource.com/v8/v8/+/f431b59)**  
  
Date(Commit): Tue Apr 25 14:20:57 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/486360](https://chromium-review.googlesource.com/486360)  
Regress: [mjsunit/compiler/regress-713367.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-713367.js)  
```javascript
var mp = Object.getPrototypeOf(0);

function getRandomProperty(v) {
  var properties;
  if (mp) { properties = Object.getOwnPropertyNames(mp); }
  if (properties.includes("constructor") && v.constructor.hasOwnProperty()) {; }
  if (properties.length == 0) { return "0"; }
  return properties[NaN];
}

var c = 0;

function f() {
  c++;
  if (c === 3) %OptimizeFunctionOnNextCall(f);
  if (c > 4) throw 42;
  for (var x of ["x"]) {
    getRandomProperty(0) ;
    f();
    %_DeoptimizeNow();
  }
}

assertThrowsEquals(f, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f431b59^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=f431b59)  
[src/compiler/escape-analysis.h](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.h?cl=f431b59)  
[test/mjsunit/compiler/regress-713367.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-713367.js?cl=f431b59)  
  

---   

## **regress-crbug-714696.js (chromium issue)**  
   
**[Issue 714696:
 Fatal error in v8::FromJust](https://crbug.com/714696)**  
**[Commit: [d8] console methods must not throw.](https://chromium.googlesource.com/v8/v8/+/87b5b53)**  
  
Date(Commit): Tue Apr 25 13:47:33 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2838143002](https://codereview.chromium.org/2838143002)  
Regress: [mjsunit/regress/regress-crbug-714696.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-714696.js)  
```javascript
if (this.Intl) {
  new Intl.v8BreakIterator();
  new Intl.DateTimeFormat();
  try { console.log({ toString: function() { throw 1; }}); } catch (e) {}
  new Intl.v8BreakIterator();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87b5b53^!)  
[src/builtins/builtins-console.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-console.cc?cl=87b5b53)  
[src/d8-console.cc](https://cs.chromium.org/chromium/src/v8/src/d8-console.cc?cl=87b5b53)  
[test/message/console.js](https://cs.chromium.org/chromium/src/v8/test/message/console.js?cl=87b5b53)  
[test/message/console.out](https://cs.chromium.org/chromium/src/v8/test/message/console.out?cl=87b5b53)  
[test/mjsunit/regress/regress-crbug-714696.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-714696.js?cl=87b5b53)  
  

---   

## **regress-crbug-714971.js (chromium issue)**  
   
**[Issue 714971:
 Unreachable code in asm-types.cc](https://crbug.com/714971)**  
**[Commit: [asm.js] Fix failure propagation of heap access validation.](https://chromium.googlesource.com/v8/v8/+/54818a6)**  
  
Date(Commit): Tue Apr 25 12:58:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/486801](https://chromium-review.googlesource.com/486801)  
Regress: [mjsunit/regress/regress-crbug-714971.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-714971.js)  
```javascript
function Module(stdlib, foreign, heap) {
  "use asm";
  var a = new stdlib.Int16Array(heap);
  function f() {
    return a[23 >> -1];
  }
  return { f:f };
}
var b = new ArrayBuffer(1024);
var m = Module(this, {}, b);
new Int16Array(b)[0] = 42;
assertEquals(42, m.f());
assertFalse(%IsAsmWasmCode(Module));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54818a6^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=54818a6)  
[test/mjsunit/regress/regress-crbug-714971.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-714971.js?cl=54818a6)  
  

---   

## **regress-714483.js (chromium issue)**  
   
**[Issue 714483:
 Fatal error in ../../v8/src/compiler/schedule.cc, line 254](https://crbug.com/714483)**  
**[Commit: [turbofan] Make sure an inlined call is not resurrected and inlined again.](https://chromium.googlesource.com/v8/v8/+/d081a6f)**  
  
Date(Commit): Tue Apr 25 08:10:32 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Arch-All", "Security_Severity-High", "merge-merged-5.9"]  
Code Review: [https://codereview.chromium.org/2833423004](https://codereview.chromium.org/2833423004)  
Regress: [mjsunit/compiler/regress-714483.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-714483.js)  
```javascript
function C1() { }
C1.prototype.f = function () { return 1; }

function C2() { }
C2.prototype.f = function () { throw 2; }

var o1 = new C1();
var o2 = new C2();

function foo(o) {
  return o.f();
}

foo(o1);
try { foo(o2); } catch(e) {}
foo(o1);
try { foo(o2); } catch(e) {}
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo(o1));
assertThrows(() => foo(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d081a6f^!)  
[src/compiler/js-inlining-heuristic.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining-heuristic.cc?cl=d081a6f)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=d081a6f)  
[test/mjsunit/compiler/regress-714483.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-714483.js?cl=d081a6f)  
  

---   

## **regress-6280.js (v8 issue)**  
   
**[Issue 6280:
 [asm.js] Typed array constructors are not considered stdlib uses](https://crbug.com/v8/6280)**  
**[Commit: [asm.js] Treat typed array constructors as stdlib uses.](https://chromium.googlesource.com/v8/v8/+/f06db79)**  
  
Date(Commit): Mon Apr 24 13:33:35 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/485521](https://chromium-review.googlesource.com/485521)  
Regress: [mjsunit/regress/regress-6280.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6280.js)  
```javascript
function Module(stdlib, imports, buffer) {
  "use asm";
  var x = new stdlib.Int8Array(buffer);
  function f() {
    return x[0] | 0;
  }
  return { f:f };
}

var b = new ArrayBuffer(1024);
var m1 = Module({ Int8Array:Int8Array }, {}, b);
assertEquals(0, m1.f());

var was_called = 0;
function observer() { was_called++; return [23]; }
var m2 = Module({ Int8Array:observer }, {}, b);
assertEquals(1, was_called);
assertEquals(23, m2.f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f06db79^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=f06db79)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=f06db79)  
[src/asmjs/asm-typer.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.h?cl=f06db79)  
[test/mjsunit/regress/regress-6280.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6280.js?cl=f06db79)  
[test/mjsunit/regress/wasm/regression-647649.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-647649.js?cl=f06db79)  
  

---   

## **regress-crbug-700733.js (chromium issue)**  
   
**[Issue 700733:
 !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/700733)**  
**[Commit: [ic] Fix handling of elements kind transitions in polymorphic keyed ICs.](https://chromium.googlesource.com/v8/v8/+/2d85654)**  
  
Date(Commit): Fri Apr 21 15:14:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/483442](https://chromium-review.googlesource.com/483442)  
Regress: [mjsunit/regress/regress-crbug-700733.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-700733.js)  
```javascript
(function test_keyed_load() {
  var smi_arr = [0];
  smi_arr.load = 42;

  var double_arr = [0.5];
  double_arr.load = 42;

  var obj_arr = [{}];
  obj_arr.load = 42;

  var arrs = [smi_arr, double_arr, obj_arr];

  var tmp;
  function do_keyed_load(arrs) {
    for (var i = 0; i < arrs.length; i++) {
      var arr = arrs[i];
      tmp = arr[0];
    }
  }

  var obj = {};
  obj.load_boom = smi_arr;

  do_keyed_load(arrs);
  do_keyed_load(arrs);
  %OptimizeFunctionOnNextCall(do_keyed_load);
  do_keyed_load(arrs);

  gc();
})();

(function test_keyed_store() {
  var smi_arr = [0];
  smi_arr.store = 42;

  var double_arr = [0.5];
  double_arr.store = 42;

  var obj_arr = [{}];
  obj_arr.store = 42;

  var arrs = [smi_arr, double_arr, obj_arr];

  function do_keyed_store(arrs) {
    for (var i = 0; i < arrs.length; i++) {
      var arr = arrs[i];
      arr[0] = 0;
    }
  }

  var obj = {};
  obj.store_boom = smi_arr;

  do_keyed_store(arrs);
  do_keyed_store(arrs);
  %OptimizeFunctionOnNextCall(do_keyed_store);
  do_keyed_store(arrs);

  gc();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d85654^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=2d85654)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=2d85654)  
[src/ic/handler-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/handler-compiler.cc?cl=2d85654)  
[src/ic/handler-compiler.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-compiler.h?cl=2d85654)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=2d85654)  
...  
  

---   

## **regress-711203.js (chromium issue)**  
   
**[Issue 711203:
 CHECK failure: Disposing the isolate that is entered by a thread in wasm-compile.cc](https://crbug.com/711203)**  
**[Commit: Restrict range for int64_t to immediate conversions](https://chromium.googlesource.com/v8/v8/+/ec772a4)**  
  
Date(Commit): Thu Apr 20 21:03:31 2017  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "M-59", "Test-Predator-Wrong-CLs"]  
Code Review: [https://chromium-review.googlesource.com/482448](https://chromium-review.googlesource.com/482448)  
Regress: [mjsunit/regress/wasm/regress-711203.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-711203.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addFunction("test", kSig_i_iii)
      .addBodyWithEnd([
        // body:
        kExprI64Const, 0,
        kExprI64Const, 0x1,
        kExprI64Clz,
        kExprI64Sub,
        kExprI64Const, 0x10,
        kExprI64Const, 0x1b,
        kExprI64Shl,
        kExprI64Sub,
        kExprI64Popcnt,
        kExprI32ConvertI64,
        kExprEnd,   // @207
      ])
            .exportFunc();
  var module = builder.instantiate();
  const result = module.exports.test(1, 2, 3);
  assertEquals(58, result);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec772a4^!)  
[src/compiler/graph-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-reducer.cc?cl=ec772a4)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=ec772a4)  
[test/mjsunit/regress/wasm/regression-711203.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-711203.js?cl=ec772a4)  
  

---   

## **regress-crbug-712802.js (chromium issue)**  
   
**[Issue 712802:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/712802)**  
**[Commit: [turbofan] Fix typing rule for JSCreateArguments.](https://chromium.googlesource.com/v8/v8/+/b89ddcf)**  
  
Date(Commit): Wed Apr 19 07:38:20 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2828573004](https://codereview.chromium.org/2828573004)  
Regress: [mjsunit/regress/regress-crbug-712802.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-712802.js)  
```javascript
function foo(...args) { return Array.isArray(args); }

assertTrue(foo());
assertTrue(foo());
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b89ddcf^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=b89ddcf)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=b89ddcf)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=b89ddcf)  
[test/mjsunit/regress/regress-crbug-712802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-712802.js?cl=b89ddcf)  
  

---   

## **regress-712569.js (chromium issue)**  
   
**[Issue 712569:
 CHECK failure: i_isolate->has_pending_exception() in wasm-js.cc](https://crbug.com/712569)**  
**[Commit: [wasm] Fix DCHECK handiling pending exceptions.](https://chromium.googlesource.com/v8/v8/+/9cc6729)**  
  
Date(Commit): Tue Apr 18 19:15:12 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2825073002](https://codereview.chromium.org/2825073002)  
Regress: [mjsunit/regress/wasm/regress-712569.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-712569.js)  
```javascript
var v11 = {};
Object.defineProperty(v11.__proto__, 0, {
  get: function() {
  },
  set: function() {
    try {
      WebAssembly.instantiate();
      v11[0] = 0;
    } catch (e) {
      assertTrue(e instanceof RangeError);
    }
  }
});
v66 = new Array();
cv = v66; cv[0] = 0.1; cv[2] = 0.2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9cc6729^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=9cc6729)  
[test/mjsunit/regress/wasm/regress-712569.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-712569.js?cl=9cc6729)  
  

---   

## **regress-crbug-711166.js (chromium issue)**  
   
**[Issue 711166:
 CHECK failure: context->IsContext() in contexts-inl.h](https://crbug.com/711166)**  
**[Commit: [turbofan] Fix translation containing arguments elements.](https://chromium.googlesource.com/v8/v8/+/e6590a3)**  
  
Date(Commit): Tue Apr 18 14:44:01 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/476631](https://chromium-review.googlesource.com/476631)  
Regress: [mjsunit/regress/regress-crbug-711166.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-711166.js)  
```javascript
'use strict'
function g() {
  var x = 1;
  try { undefined.x } catch (e) { x = e; }
  (function() { x });
  return x;
}
function f(a) {
  var args = arguments;
  assertInstanceof(g(), TypeError);
  return args.length;
}
assertEquals(1, f(0));
assertEquals(1, f(0));
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6590a3^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=e6590a3)  
[test/mjsunit/regress/regress-crbug-711166.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-711166.js?cl=e6590a3)  
  

---   

## **regress-710844.js (chromium issue)**  
   
**[Issue 710844:
 Fatal error in v8::Isolate::Disposev8::Utils::ReportApiFailure](https://crbug.com/710844)**  
**[Commit: [wasm] Handle no initial memory case correctly when memory is exported](https://chromium.googlesource.com/v8/v8/+/78b8d7e)**  
  
Date(Commit): Tue Apr 18 06:34:16 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Memory-LeakSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "M-59", "Test-Predator-Wrong-CLs"]  
Code Review: [https://codereview.chromium.org/2820223002](https://codereview.chromium.org/2820223002)  
Regress: [mjsunit/regress/wasm/regress-710844.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-710844.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  "use asm";
  var builder = new WasmModuleBuilder();
  builder.addMemory(0, 5, true);
  builder.addFunction("regression_710844", kSig_v_v)
    .addBody([
        kExprI32Const, 0x03,
        kExprNop,
        kExprMemoryGrow, 0x00,
        kExprI32Const, 0x13,
        kExprNop,
        kExprI32StoreMem8, 0x00, 0x10
        ]).exportFunc();
 let instance = builder.instantiate();
 instance.exports.regression_710844();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78b8d7e^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=78b8d7e)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=78b8d7e)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=78b8d7e)  
[test/mjsunit/regress/wasm/regression-699485.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-699485.js?cl=78b8d7e)  
[test/mjsunit/regress/wasm/regression-710844.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-710844.js?cl=78b8d7e)  
  

---   

## **regress-707066.js (chromium issue)**  
   
**[Issue 707066:
 CHECK failure: next == token in parser-base.h](https://crbug.com/707066)**  
**[Commit: fix assertion failure with --harmony CreateDynamicFunction() in stack overflow conditions](https://chromium.googlesource.com/v8/v8/+/1236335)**  
  
Date(Commit): Mon Apr 17 20:06:15 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/474990](https://chromium-review.googlesource.com/474990)  
Regress: [mjsunit/regress/regress-707066.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-707066.js)  
```javascript
function test(api) {
  function f() {
    try {
      // induce a stack overflow
      f();
    } catch(e) {
      // this might result in even more stack overflows
      api();
    }
  }
  f();
}

test((      function (){}).constructor); // Function
test((      function*(){}).constructor); // GeneratorFunction
test((async function (){}).constructor); // AsyncFunction  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1236335^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=1236335)  
[test/mjsunit/regress/regress-707066.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-707066.js?cl=1236335)  
  

---   

## **regress-711165.js (chromium issue)**  
   
**[Issue 711165:
 Direct-leak in InitializeModuleEmbedderData](https://crbug.com/711165)**  
**[Commit: [d8] Fix leak in IntializeModuleEmbedderData](https://chromium.googlesource.com/v8/v8/+/484d25d)**  
  
Date(Commit): Thu Apr 13 21:52:28 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Memory-LeakSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://chromium-review.googlesource.com/476970](https://chromium-review.googlesource.com/476970)  
Regress: [mjsunit/regress/regress-711165.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-711165.js)  
```javascript
try {
  Realm.navigate(0);
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/484d25d^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=484d25d)  
[test/mjsunit/regress/regress-711165.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-711165.js?cl=484d25d)  
  

---   

## **regress-707223.js (chromium issue)**  
   
**[Issue 707223:
 CHECK failure: position > 0 in feedback-vector.cc](https://crbug.com/707223)**  
**[Commit: [type feedback] Allow position 0.](https://chromium.googlesource.com/v8/v8/+/b305033)**  
  
Date(Commit): Thu Apr 13 09:55:14 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/476630](https://chromium-review.googlesource.com/476630)  
Regress: [mjsunit/type-profile/regress-707223.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/type-profile/regress-707223.js)  
```javascript
let e;
eval("e");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b305033^!)  
[src/feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/feedback-vector.cc?cl=b305033)  
[src/list-inl.h](https://cs.chromium.org/chromium/src/v8/src/list-inl.h?cl=b305033)  
[test/mjsunit/type-profile/regress-707223.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/type-profile/regress-707223.js?cl=b305033)  
  

---   

## **regress-6248.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6248)**  
**[Commit: [turbofan] Fix lowering of JSGetSuperConstructor.](https://chromium.googlesource.com/v8/v8/+/68b047d)**  
  
Date(Commit): Thu Apr 13 08:34:22 2017  
Type: None  
Code Review: [https://chromium-review.googlesource.com/476570](https://chromium-review.googlesource.com/476570)  
Regress: [mjsunit/regress/regress-6248.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6248.js)  
```javascript
var sentinelObject = {};
var evaluatedArg = false;
class C extends Object {
  constructor() {
    try {
      super(evaluatedArg = true);
    } catch (e) {
      assertInstanceof(e, TypeError);
      return sentinelObject;
    }
  }
}
Object.setPrototypeOf(C, parseInt);
assertSame(sentinelObject, new C());
assertSame(sentinelObject, new C());
%OptimizeFunctionOnNextCall(C)
assertSame(sentinelObject, new C());
assertFalse(evaluatedArg);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68b047d^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=68b047d)  
[test/mjsunit/regress/regress-6248.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6248.js?cl=68b047d)  
  

---   

## **regress-crbug-709753.js (chromium issue)**  
   
**[Issue 709753:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/709753)**  
**[Commit: [turbofan] Properly represent the float64 hole.](https://chromium.googlesource.com/v8/v8/+/8c0c5e8)**  
  
Date(Commit): Wed Apr 12 10:10:48 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "NodeJS-Backport-Rejected", "v8-foozzie-failure", "merge-merged-5.8", "merge-merged-58", "merge-merged-5.9"]  
Code Review: [https://codereview.chromium.org/2814013003](https://codereview.chromium.org/2814013003)  
Regress: [mjsunit/regress/regress-crbug-709753.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-709753.js)  
```javascript
function foo(a, i) { a[i].x; }

var a = [,0.1];
foo(a, 1);
foo(a, 1);
%OptimizeFunctionOnNextCall(foo);
assertThrows(() => foo(a, 0), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c0c5e8^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=8c0c5e8)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=8c0c5e8)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=8c0c5e8)  
[src/compiler/operation-typer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.h?cl=8c0c5e8)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=8c0c5e8)  
...  
  

---   

## **regress-641091.js (chromium issue)**  
   
**[Issue 641091:
 Unicode regular expression using alternation/OR returns null](https://crbug.com/641091)**  
**[Commit: [regexp] Consider surrogate pairs when optimizing disjunctions](https://chromium.googlesource.com/v8/v8/+/4635572)**  
  
Date(Commit): Wed Apr 12 09:09:12 2017  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/2813893002](https://codereview.chromium.org/2813893002)  
Regress: [mjsunit/regress/regress-641091.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-641091.js)  
```javascript
assertEquals(["🍤", "🍤"],
             '🍤🍦🍋ππ🍋🍦🍤'.match(/🍤/ug));

assertEquals(["🍤", "🍦", "🍦", "🍤"],
             '🍤🍦🍋ππ🍋🍦🍤'.match(/🍤|🍦/ug));

assertEquals(["🍤", "🍦", "🍋", "🍋", "🍦", "🍤"],
             '🍤🍦🍋ππ🍋🍦🍤'.match(/🍤|🍦|🍋/ug));

assertEquals(["🍤", "🍦", "🍋", "π", "π", "🍋", "🍦", "🍤"],
             '🍤🍦🍋ππ🍋🍦🍤'.match(/🍤|🍦|π|🍋/ug));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4635572^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=4635572)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=4635572)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=4635572)  
[test/mjsunit/regress/regress-641091.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-641091.js?cl=4635572)  
  

---   

## **regress-crbug-708050-1.js (chromium issue)**  
   
**[Issue 708050:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/708050)**  
**[Commit: [turbofan] Fix typing rule for CheckBounds.](https://chromium.googlesource.com/v8/v8/+/483812d)**  
  
Date(Commit): Wed Apr 12 09:02:28 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure", "merge-merged-5.9"]  
Code Review: [https://codereview.chromium.org/2812013006](https://codereview.chromium.org/2812013006)  
Regress: [mjsunit/regress/regress-crbug-708050-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-708050-1.js), [mjsunit/regress/regress-crbug-708050-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-708050-2.js)  
```javascript
var v = {}

function foo() {
  v[0] = 5;
  v[-0] = 27;
  return v[0];
}

assertEquals(27, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(27, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/483812d^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=483812d)  
[test/mjsunit/regress/regress-crbug-708050-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-708050-1.js?cl=483812d)  
[test/mjsunit/regress/regress-crbug-708050-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-708050-2.js?cl=483812d)  
  

---   

## **regress-crbug-709537.js (chromium issue)**  
   
**[Issue 709537:
 CHECK failure: object.is_null() || *object == scope_site->transition_info() in allocation-site-](https://crbug.com/709537)**  
**[Commit: [turbofan] Fix traversal order of boilerplate objects.](https://chromium.googlesource.com/v8/v8/+/1f3a863)**  
  
Date(Commit): Tue Apr 11 11:42:52 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/474028](https://chromium-review.googlesource.com/474028)  
Regress: [mjsunit/regress/regress-crbug-709537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-709537.js)  
```javascript
function foo() {
  return { 0: {}, x: {} };
}
var ref = foo();
assertEquals(ref, foo());
assertEquals(ref, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(ref, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1f3a863^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=1f3a863)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=1f3a863)  
[test/mjsunit/regress/regress-crbug-709537.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-709537.js?cl=1f3a863)  
  

---   

## **regress-707675.js (chromium issue)**  
   
**[Issue 707675:
 CHECK failure: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/707675)**  
**[Commit: [runtime] Filter out non-JSObject prototypes when eliding iteration.](https://chromium.googlesource.com/v8/v8/+/e00dd8e)**  
  
Date(Commit): Mon Apr 10 15:37:11 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/472907](https://chromium-review.googlesource.com/472907)  
Regress: [mjsunit/regress/regress-707675.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-707675.js)  
```javascript
Array.prototype.__proto__ = null;
new Uint8Array(Array.prototype);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e00dd8e^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e00dd8e)  
[test/mjsunit/regress/regress-707675.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-707675.js?cl=e00dd8e)  
  

---   

## **regress-709029.js (chromium issue)**  
   
**[Issue 709029:
 CHECK failure: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/709029)**  
**[Commit: [regexp] Avoid side effects between map load and fast path check](https://chromium.googlesource.com/v8/v8/+/db61537)**  
  
Date(Commit): Mon Apr 10 14:57:55 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "allpublic", "Clusterfuzz", "M-59", "ClusterFuzz-Verified", "Test-Predator-Wrong", "merge-merged-5.8", "merge-merged-58"]  
Code Review: [https://codereview.chromium.org/2807153002](https://codereview.chromium.org/2807153002)  
Regress: [mjsunit/regress/regress-709029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-709029.js)  
```javascript
function mutateObjectOnStringConversion(obj) {
  return { toString: () => { obj.x = 42;return "";}};
}

{
  const re = /./;
  re.exec(mutateObjectOnStringConversion(re));
}

{
  const re = /./;
  re.test(mutateObjectOnStringConversion(re));
}

{
  const re = /./;
  re[Symbol.match](mutateObjectOnStringConversion(re));
}

{
  const re = /./;
  re[Symbol.search](mutateObjectOnStringConversion(re));
}

{
  const re = /./;
  re[Symbol.split](mutateObjectOnStringConversion(re));
}

{
  const re = /./;
  re[Symbol.replace](mutateObjectOnStringConversion(re));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db61537^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=db61537)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=db61537)  
[test/mjsunit/regress/regress-709029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-709029.js?cl=db61537)  
  

---   

## **regress-709684.js (chromium issue)**  
   
**[Issue 709684:
 [wasm] WebAssembly.compile broken by histogram scopes](https://crbug.com/709684)**  
**[Commit: Fixed accounting issues due to code table containing imports as well as wasm funcs.](https://chromium.googlesource.com/v8/v8/+/85b1f10)**  
  
Date(Commit): Mon Apr 10 14:03:59 2017  
Components/Type: None/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/2807013002](https://codereview.chromium.org/2807013002)  
Regress: [mjsunit/regress/wasm/regress-709684.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-709684.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let importingModuleBinary1 = (() => {
  var builder = new WasmModuleBuilder();
  builder.addImport('', 'f', kSig_i_v);
  return new Int8Array(builder.toBuffer());
})();

let importingModuleBinary2 = (() => {
  var builder = new WasmModuleBuilder();
  builder.addImport('', 'f', kSig_i_v);
  builder.addFunction('g', kSig_v_v)
    .addBody([kExprNop]);
  return new Int8Array(builder.toBuffer());
})();

let importingModuleBinary3 = (() => {
  var builder = new WasmModuleBuilder();
  builder.addImport('', 'f', kSig_i_v);
  builder.addImport('', 'f2', kSig_i_v);
  builder.addFunction('g', kSig_v_v)
    .addBody([kExprNop]);
  return new Int8Array(builder.toBuffer());
})();

let importingModuleBinary4 = (() => {
  var builder = new WasmModuleBuilder();
  builder.addFunction('g', kSig_v_v)
    .addBody([kExprNop]);
  return new Int8Array(builder.toBuffer());
})();

const complexImportingModuleBinary1 = (() => {
  let builder = new WasmModuleBuilder();

  builder.addImport('a', 'b', kSig_v_v);
  builder.addImportedMemory('c', 'd', 1);
  builder.addImportedTable('e', 'f', 1);
  builder.addImportedGlobal('g', '⚡', kWasmI32);
  builder.addFunction('g', kSig_v_v)
    .addBody([kExprNop]);
  return builder.toBuffer();
})();

const complexImportingModuleBinary2 = (() => {
  let builder = new WasmModuleBuilder();

  builder.addImport('a', 'b', kSig_v_v);
  builder.addImportedMemory('c', 'd', 1);
  builder.addImportedTable('e', 'f', 1);
  builder.addImportedGlobal('g', '⚡', kWasmI32);
  builder.addFunction('g', kSig_v_v)
    .addBody([kExprNop]);
  return builder.toBuffer();
})();

var tests = [
  importingModuleBinary1,
  importingModuleBinary2,
  importingModuleBinary3,
  importingModuleBinary4,
  complexImportingModuleBinary1,
  complexImportingModuleBinary2
];

for (var index in tests) {
  assertPromiseResult(
    WebAssembly.compile(tests[index]),
    m => assertTrue(m instanceof WebAssembly.Module),
    assertUnreachable);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85b1f10^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=85b1f10)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=85b1f10)  
[test/mjsunit/regress/wasm/regress-709684.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-709684.js?cl=85b1f10)  
  

---   

## **regress-708714.js (chromium issue)**  
   
**[Issue 708714:
 Fatal error in v8::Isolate::Disposev8::Utils::ReportApiFailure](https://crbug.com/708714)**  
**[Commit: [wasm] Stop decoding sections once an error occured](https://chromium.googlesource.com/v8/v8/+/88e169d)**  
  
Date(Commit): Mon Apr 10 13:00:50 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/472847](https://chromium-review.googlesource.com/472847)  
Regress: [mjsunit/regress/wasm/regress-708714.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-708714.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

var builder = new WasmModuleBuilder();

builder.addExplicitSection([kFunctionSectionCode,
  // length
  7,
  // functions count
  1,
  // signature index (invalid LEB)
  0xff, 0xff, 0xff, 0xff, 0xff]);
builder.addExplicitSection([kStartSectionCode,
  // length
  1,
  // index
  0]);

assertThrows(() => builder.instantiate(), WebAssembly.CompileError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88e169d^!)  
[src/wasm/decoder.h](https://cs.chromium.org/chromium/src/v8/src/wasm/decoder.h?cl=88e169d)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=88e169d)  
[test/mjsunit/regress/wasm/regression-708714.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-708714.js?cl=88e169d)  
  

---   

## **regress-6223.js (v8 issue)**  
   
**[Issue 6223:
 Spec violation in TypedArray.prototype.slice (+ UB)](https://crbug.com/v8/6223)**  
**[Commit: [runtime] Fix TypedArray slice when sharing the underlying buffer](https://chromium.googlesource.com/v8/v8/+/186bfbb)**  
  
Date(Commit): Mon Apr 10 12:57:21 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/471409](https://chromium-review.googlesource.com/471409)  
Regress: [mjsunit/regress/regress-6223.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6223.js)  
```javascript
var ab = new Int8Array(20).map((v, i) => i).buffer;
var ta = new Int8Array(ab, 0, 10);
var seen_length = -1;
ta.constructor = {
  [Symbol.species]: function(len) {
    seen_length = len;
    return new Int8Array(ab, 1, len);
  }
};

assertEquals(-1, seen_length);
assertArrayEquals([0,1,2,3,4,5,6,7,8,9], ta);
var tb = ta.slice();
assertEquals(10, seen_length);
assertArrayEquals([0,0,0,0,0,0,0,0,0,0], ta);
assertArrayEquals([0,0,0,0,0,0,0,0,0,0], tb);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/186bfbb^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=186bfbb)  
[test/mjsunit/es6/typedarray-slice.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/typedarray-slice.js?cl=186bfbb)  
[test/mjsunit/regress-6223.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-6223.js?cl=186bfbb)  
  

---   

## **regress-708598.js (chromium issue)**  
   
**[Issue 708598:
 CHECK failure: shared()->is_compiled() in objects.cc](https://crbug.com/708598)**  
**[Commit: [parser] Skipping inner funcs: Fix untrue DCHECK.](https://chromium.googlesource.com/v8/v8/+/930174c)**  
  
Date(Commit): Mon Apr 10 11:03:30 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/461827](https://chromium-review.googlesource.com/461827)  
Regress: [mjsunit/regress/regress-708598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-708598.js)  
```javascript
assertThrows(`function lazy_does_not_compile(x) {
                break;
              }
              new lazy_does_not_compile();`, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/930174c^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=930174c)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=930174c)  
[test/mjsunit/regress/regress-708598.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-708598.js?cl=930174c)  
  

---   

## **regress-6209.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6209)**  
**[Commit: [regexp] Properly handle HeapNumbers in AdvanceStringIndex](https://chromium.googlesource.com/v8/v8/+/ed5496f)**  
  
Date(Commit): Thu Apr 06 18:43:09 2017  
Type: None  
Code Review: [https://codereview.chromium.org/2798933003](https://codereview.chromium.org/2798933003)  
Regress: [mjsunit/regress/regress-6209.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6209.js)  
```javascript
function testAdvanceStringIndex(lastIndex, expectedLastIndex) {
  let exec_count = 0;
  let last_last_index = -1;

  let fake_re = {
    exec: () => { return (exec_count++ == 0) ? [""] : null },
    get lastIndex() { return lastIndex; },
    set lastIndex(value) { last_last_index = value },
    get global() { return true; },
    get flags() { return "g"; }
  };

  assertEquals([""], RegExp.prototype[Symbol.match].call(fake_re, "abc"));
  assertEquals(expectedLastIndex, last_last_index);
}

testAdvanceStringIndex(new Number(42), 43);  // Value wrapper.
testAdvanceStringIndex(%AllocateHeapNumber(), 1);  // HeapNumber.
testAdvanceStringIndex(4294967295, 4294967296);  // HeapNumber.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ed5496f^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=ed5496f)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=ed5496f)  
[test/mjsunit/regress/regress-6209.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6209.js?cl=ed5496f)  
  

---   

## **regress-6210.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6210)**  
**[Commit: [regexp] Fix two more possible shape changes on fast path](https://chromium.googlesource.com/v8/v8/+/1ccf6c0)**  
  
Date(Commit): Thu Apr 06 15:52:21 2017  
Type: None  
Code Review: [https://codereview.chromium.org/2803603005](https://codereview.chromium.org/2803603005)  
Regress: [mjsunit/regress/regress-6210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6210.js)  
```javascript
const str = '2016-01-02';

function testToUint32InSplit() {
  var re;
  function toDictMode() {
    re.x = 42;
    delete re.x;
    return "def";
  }

  re = /./g;  // Needs to be global to trigger lastIndex accesses.
  return re[Symbol.replace]("abc", { valueOf: toDictMode });
}

function testToStringInReplace() {
  var re;
  function toDictMode() {
    re.x = 42;
    delete re.x;
    return 42;
  }

  re = /./g;  // Needs to be global to trigger lastIndex accesses.
  return re[Symbol.split]("abc", { valueOf: toDictMode });
}

testToUint32InSplit();
testToStringInReplace();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ccf6c0^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=1ccf6c0)  
[test/mjsunit/regress/regress-6210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6210.js?cl=1ccf6c0)  
  

---   

## **regress-708247.js (chromium issue)**  
   
**[Issue 708247:
 Security:  OOB access in RegExp Stubs](https://crbug.com/708247)**  
**[Commit: [regexp] Ensure there are no shape changes on the fast path](https://chromium.googlesource.com/v8/v8/+/ae45935)**  
  
Date(Commit): Thu Apr 06 08:12:56 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Security  
Labels: ["Security_Severity-High", "Security_Impact-Beta", "allpublic", "M-58", "merge-merged-5.8", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/2797993002](https://codereview.chromium.org/2797993002)  
Regress: [mjsunit/regress/regress-708247.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-708247.js)  
```javascript
const str = '2016-01-02';

function t() {
  var re;
  function toDictMode() {
    for (var i = 0; i < 100; i++) {  // Loop is required.
      re.x = 42;
      delete re.x;
    }
    return 0;
  }

  re = /-/g;  // Needs to be global to trigger lastIndex accesses.
  re.lastIndex = { valueOf : toDictMode };
  return re.exec(str);
}

for (var q = 0; q < 10000; q++) {
  t();  // Needs repetitions to trigger a crash.
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae45935^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=ae45935)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=ae45935)  
[src/builtins/builtins-string-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string-gen.cc?cl=ae45935)  
[src/regexp/regexp-utils.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-utils.cc?cl=ae45935)  
[test/mjsunit/regress/regress-708247.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-708247.js?cl=ae45935)  
  

---   

## **regress-6203.js (v8 issue)**  
   
**[Issue 6203:
 [asm.js] Error reporting of missing import is borked](https://crbug.com/v8/6203)**  
**[Commit: [asm.js] Prevent throwing of asm.js warning messages.](https://chromium.googlesource.com/v8/v8/+/5e8eb62)**  
  
Date(Commit): Wed Apr 05 14:41:52 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/468808](https://chromium-review.googlesource.com/468808)  
Regress: [mjsunit/regress/regress-6203.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6203.js)  
```javascript
function Module(stdlib, imports, buffer) {
  "use asm";
  var a = imports.x | 0;
  function f() {
    return a | 0;
  }
  return { f:f };
}
try {
  Module(this).f();
} catch(e) {
  assertInstanceof(e, TypeError);
  // The following print is needed to cross the API boundary and thereby flush
  // out any leftover scheduled exceptions. Any other API function would do.
  print(e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e8eb62^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=5e8eb62)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=5e8eb62)  
[test/mjsunit/regress/regress-6203.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6203.js?cl=5e8eb62)  
  

---   

## **regress-706234-2.js (chromium issue)**  
   
**[Issue 706234:
 Use-after-poison in v8::internal::interpreter::BytecodeRegisterOptimizer::RegisterInfo::materialized](https://crbug.com/706234)**  
**[Commit: [parser] don't rewrite destructuring assignments in params for lazy top level arrow functions](https://chromium.googlesource.com/v8/v8/+/5f782db)**  
  
Date(Commit): Tue Apr 04 20:35:03 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "Stability-Memory-MemorySanitizer", "allpublic", "Clusterfuzz", "M-58", "M-59", "merge-merged-5.8", "Release-0-M58"]  
Code Review: [https://chromium-review.googlesource.com/c/464769/](https://chromium-review.googlesource.com/c/464769/)  
Regress: [mjsunit/regress/regress-706234-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-706234-2.js), [mjsunit/regress/regress-706234.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-706234.js)  
```javascript
var f = ({ x } = { x: 1 }) => {
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
  x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;x;
};

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f782db^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=5f782db)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=5f782db)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=5f782db)  
[test/mjsunit/regress/regress-706234-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-706234-2.js?cl=5f782db)  
[test/mjsunit/regress/regress-706234.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-706234.js?cl=5f782db)  
  

---   

## **regress-6186.js (v8 issue)**  
   
**[Issue 6186:
 CHECK fail when a Proxy function is paased for String.replace](https://crbug.com/v8/6186)**  
**[Commit: [regexp] Handle a function Proxy passed to String.prototype.replace](https://chromium.googlesource.com/v8/v8/+/8b8295d)**  
  
Date(Commit): Tue Apr 04 18:48:56 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/466549](https://chromium-review.googlesource.com/466549)  
Regress: [mjsunit/regress/regress-6186.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6186.js)  
```javascript
assertEquals("b", "a".replace(/a/, new Proxy(() => "b", {})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8b8295d^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=8b8295d)  
[test/mjsunit/regress/regress-6186.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6186.js?cl=8b8295d)  
  

---   

## **regress-6196.js (v8 issue)**  
   
**[Issue 6196:
 [asm.js] Nested function table calls are borked](https://crbug.com/v8/6196)**  
**[Commit: [asm.js] Fix nested function table calls.](https://chromium.googlesource.com/v8/v8/+/ce06d1f)**  
  
Date(Commit): Tue Apr 04 10:28:06 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/467327](https://chromium-review.googlesource.com/467327)  
Regress: [mjsunit/regress/regress-6196.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6196.js)  
```javascript
function Module(stdlib, imports, buffer) {
  "use asm";
  function f(a,b,c) {
    a = a | 0;
    b = b | 0;
    c = c | 0;
    var x = 0;
    x = funTable[a & 1](funTable[b & 1](c) | 0) | 0;
    return x | 0;
  }
  function g(a) {
    a = a | 0;
    return (a + 23) | 0;
  }
  function h(a) {
    a = a | 0;
    return (a + 42) | 0;
  }
  var funTable = [ g, h ];
  return f;
}
var f = Module(this);
assertTrue(%IsWasmCode(f));
assertTrue(%IsAsmWasmCode(Module));
assertEquals(165, f(0, 1, 100));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce06d1f^!)  
[src/asmjs/asm-parser.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.cc?cl=ce06d1f)  
[src/asmjs/asm-parser.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-parser.h?cl=ce06d1f)  
[test/mjsunit/regress/regress-6196.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6196.js?cl=ce06d1f)  
  

---   

## **regress-707410.js (chromium issue)**  
   
**[Issue 707410:
 Heap-use-after-free in v8::internal::libc_memcpy](https://crbug.com/707410)**  
**[Commit: [builtins] Use length field in TypedArrayConstructByArrayLike.](https://chromium.googlesource.com/v8/v8/+/c5ad59f)**  
  
Date(Commit): Mon Apr 03 12:45:22 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-59"]  
Code Review: [https://chromium-review.googlesource.com/465987](https://chromium-review.googlesource.com/465987)  
Regress: [mjsunit/regress/regress-707410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-707410.js)  
```javascript
var a = new Uint8Array(1024*1024);
%ArrayBufferDetach(a.buffer);
assertThrows(() => new Uint8Array(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c5ad59f^!)  
[src/builtins/builtins-typedarray-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typedarray-gen.cc?cl=c5ad59f)  
[test/mjsunit/regress-707410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-707410.js?cl=c5ad59f)  
  

---   

## **regress-702460.js (chromium issue)**  
   
**[Issue 702460:
 CHECK failure: Unknown or unimplemented opcode #192:s128.load128 in wasm-interpreter.cc](https://crbug.com/702460)**  
**[Commit: [wasm] Gate SIMD load/store opcodes with the --wasm-simd-prototype flag.](https://chromium.googlesource.com/v8/v8/+/0f9680c)**  
  
Date(Commit): Fri Mar 31 22:52:59 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "M-59", "Stability-AFL", "Test-Predator-Wrong-CLs"]  
Code Review: [https://codereview.chromium.org/2794693002](https://codereview.chromium.org/2794693002)  
Regress: [mjsunit/regress/wasm/regress-702460.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-702460.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

let kSig_s_v = makeSig([], [kWasmS128]);
let kExprS128LoadMem = 0xc0;

(function() {
"use asm";
  var builder = new WasmModuleBuilder();
  builder.addFunction("regression_702460", kSig_i_v)
    .addBody([
        kExprI32Const, 0x52,
        kExprI32Const, 0x41,
        kExprI32Const, 0x3c,
        kExprI32Const, 0xdc, 0x01,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprSetLocal, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprMemoryGrow, 0x00,
        kExprS128LoadMem, 0x00, 0x40,
        kExprUnreachable,
        kExprMemoryGrow, 0x00
        ]).exportFunc();
  assertThrows(() => builder.instantiate());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f9680c^!)  
[src/wasm/function-body-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder.cc?cl=0f9680c)  
[test/mjsunit/regress/wasm/regression-702460.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-702460.js?cl=0f9680c)  
[test/mjsunit/wasm/wasm-constants.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/wasm-constants.js?cl=0f9680c)  
  

---   

## **regress-707187.js (chromium issue)**  
   
**[Issue 707187:
 V8 correctness failure in configs: x64,ignition:x64,ignition_eager](https://crbug.com/707187)**  
**[Commit: [regexp] Revert to ZoneList usage in @@replace](https://chromium.googlesource.com/v8/v8/+/686c378)**  
  
Date(Commit): Fri Mar 31 14:38:36 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2787343002](https://codereview.chromium.org/2787343002)  
Regress: [mjsunit/regress/regress-707187.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-707187.js)  
```javascript
let i = 0;
let re = /./g;
re.exec = () => {
  if (i++ == 0) return { length: 2 ** 16 };
  return null;
};

"".replace(re);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/686c378^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=686c378)  
[test/mjsunit/regress/regress-707187.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-707187.js?cl=686c378)  
  

---   

## **regress-crbug-706642.js (chromium issue)**  
   
**[Issue 706642:
 Deoptimizing a derived constructor leaks the hole](https://crbug.com/706642)**  
**[Commit: [turbofan] Disable inlining of derived class constructors.](https://chromium.googlesource.com/v8/v8/+/c019e53)**  
  
Date(Commit): Thu Mar 30 10:17:10 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Hotlist-Merge-Approved", "allpublic", "merge-merged-5.8", "merge-merged-58"]  
Code Review: [https://codereview.chromium.org/2788603002](https://codereview.chromium.org/2788603002)  
Regress: [mjsunit/regress/regress-crbug-706642.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-706642.js)  
```javascript
class A extends Object {
  constructor(arg) {
    super();
    superclass_counter++;
    if (superclass_counter === 3) {
      return 1;
    }
  }
}

class B extends A {
  constructor() {
    let x = super(123);
    return x.a;
  }
}

var superclass_counter = 0;
var observer = new Proxy(A, {
  get(target, property, receiver) {
    if (property === 'prototype') {
      %DeoptimizeFunction(B);
    }
    return Reflect.get(target, property, receiver);
  }
});

Reflect.construct(B, [], observer);
Reflect.construct(B, [], observer);
%OptimizeFunctionOnNextCall(B);
assertThrows(() => Reflect.construct(B, [], observer), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c019e53^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c019e53)  
[test/mjsunit/regress/regress-crbug-706642.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-706642.js?cl=c019e53)  
  

---   

## **regress-6164.js (v8 issue)**  
   
**[Issue 6164:
 WebAssembly Crash on ia32 for functions with i64 return value and unreachable.](https://crbug.com/v8/6164)**  
**[Commit: [wasm] Consider void returns in the int64-lowering](https://chromium.googlesource.com/v8/v8/+/151cad8)**  
  
Date(Commit): Wed Mar 29 13:51:33 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/461941](https://chromium-review.googlesource.com/461941)  
Regress: [mjsunit/regress/wasm/regress-6164.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-6164.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(31, 31, false);
  builder.addFunction('test', kSig_l_v)
      .addBodyWithEnd([
kExprUnreachable,
kExprEnd,   // @374
      ])
      .exportFunc();
  var module = builder.instantiate();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/151cad8^!)  
[src/compiler/int64-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/int64-lowering.cc?cl=151cad8)  
[test/mjsunit/regress/wasm/regression-6164.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-6164.js?cl=151cad8)  
  

---   

## **regress-705934.js (chromium issue)**  
   
**[Issue 705934:
 CHECK failure: value != Smi::FromInt(JSRegExp::kUninitializedValue) in objects-inl.h](https://crbug.com/705934)**  
**[Commit: [regexp] Properly handle failed RegExp compilations](https://chromium.googlesource.com/v8/v8/+/e2858f2)**  
  
Date(Commit): Wed Mar 29 07:18:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2778953004](https://codereview.chromium.org/2778953004)  
Regress: [mjsunit/regress/regress-705934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-705934.js)  
```javascript
function call_replace_close_to_stack_overflow() {
  try {
    call_replace_close_to_stack_overflow();
  } catch {
    "b".replace(/(b)/g);
  }
}

call_replace_close_to_stack_overflow();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2858f2^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=e2858f2)  
[test/mjsunit/regress/regress-705934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-705934.js?cl=e2858f2)  
  

---   

## **regress-704811.js (chromium issue)**  
   
**[Issue 704811:
 Simple invalid javascript crashes tab](https://crbug.com/704811)**  
**[Commit: [parser] Fix crash when lazy arrow func params contain destructuring assignments.](https://chromium.googlesource.com/v8/v8/+/bc39a51)**  
  
Date(Commit): Tue Mar 28 08:22:46 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "M-57", "Via-Wizard-Javascript", "merge-merged-5.8"]  
Code Review: [https://chromium-review.googlesource.com/459618](https://chromium-review.googlesource.com/459618)  
Regress: [mjsunit/regress/regress-704811.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-704811.js)  
```javascript
(({x = {} = {}}) => {})({});

let a0 = ({x = {} = {}}) => {};
a0({});

let called = false;
let global_side_assignment = undefined;
(({x = {myprop: global_side_assignment} = {myprop: 2115}}) => {
  assertTrue('myprop' in x);
  assertEquals(2115, x.myprop);
  called = true;
})({});
assertTrue(called);
assertEquals(2115, global_side_assignment);

called = false;
global_side_assignment = undefined;
(({x = {myprop: global_side_assignment} = {myprop: 2115}}) => {
  assertEquals(3000, x);
  called = true;
})({x: 3000});
assertTrue(called);
assertEquals(undefined, global_side_assignment);

called = false;
global_side_assignment = undefined;
let a1 = ({x = {myprop: global_side_assignment} = {myprop: 2115}}) => {
  assertTrue('myprop' in x);
  assertEquals(2115, x.myprop);
  called = true;
}
a1({});
assertTrue(called);
assertEquals(2115, global_side_assignment);

called = false;
global_side_assignment = undefined;
let a2 = ({x = {myprop: global_side_assignment} = {myprop: 2115}}) => {
  assertEquals(3000, x);
  called = true;
}
a2({x: 3000});
assertTrue(called);
assertEquals(undefined, global_side_assignment);

called = false;
global_side_assignment = undefined;
function f1({x = {myprop: global_side_assignment} = {myprop: 2115}}) {
  assertTrue('myprop' in x);
  assertEquals(2115, x.myprop);
  assertEquals(2115, global_side_assignment);
  called = true;
}
f1({});
assertTrue(called);
assertEquals(2115, global_side_assignment);

called = false;
global_side_assignment = undefined;
function f2({x = {myprop: global_side_assignment} = {myprop: 2115}}) {
  assertEquals(3000, x);
  called = true;
}
f2({x: 3000});
assertTrue(called);
assertEquals(undefined, global_side_assignment);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc39a51^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=bc39a51)  
[test/mjsunit/regress/regress-704811.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-704811.js?cl=bc39a51)  
  

---   

## **regress-699485.js (chromium issue)**  
   
**[Issue 699485:
 Crash in NumberToSize](https://crbug.com/699485)**  
**[Commit: [wasm] Detach memory buffer only when GrowMemory is called from the JS API](https://chromium.googlesource.com/v8/v8/+/c8b2656)**  
  
Date(Commit): Mon Mar 27 22:59:55 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "M-59", "Stability-AFL", "Test-Predator-Wrong", "merge-merged-5.8"]  
Code Review: [https://codereview.chromium.org/2772973002](https://codereview.chromium.org/2772973002)  
Regress: [mjsunit/regress/wasm/regress-699485.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-699485.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
"use asm";
var builder = new WasmModuleBuilder();
builder.addMemory(0, 5, false);
builder.addFunction("regression_699485", kSig_i_v)
  .addBody([
      kExprI32Const, 0x04,
      kExprNop,
      kExprMemoryGrow, 0x00,
      ]).exportFunc();
let module = builder.instantiate();
assertEquals(0, module.exports.regression_699485());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8b2656^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=c8b2656)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=c8b2656)  
[src/wasm/wasm-module.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.h?cl=c8b2656)  
[test/mjsunit/regress/wasm/regression-699485.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-699485.js?cl=c8b2656)  
  

---   

## **regress-6142.js (v8 issue)**  
   
**[Issue 6142:
 Bad error message for continuing a non-loop label](https://crbug.com/v8/6142)**  
**[Commit: [parser] Use better error message to continue a non IterationStatement](https://chromium.googlesource.com/v8/v8/+/cd86861)**  
  
Date(Commit): Sat Mar 25 22:04:15 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/459436](https://chromium-review.googlesource.com/459436)  
Regress: [mjsunit/regress/regress-6142.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6142.js)  
```javascript
try {
  eval('a: { continue a; }');
  assertUnreachable();
} catch(e) {
  assertTrue(e instanceof SyntaxError);
  assertEquals('Illegal continue statement: \'a\' does not denote an iteration statement', e.message);
}

try {
  eval('continue;');
  assertUnreachable();
} catch(e) {
  assertTrue(e instanceof SyntaxError);
  assertEquals('Illegal continue statement: no surrounding iteration statement', e.message);
}

try {
  eval('a: { continue b;}');
  assertUnreachable();
} catch(e) {
  assertTrue(e instanceof SyntaxError);
  assertEquals("Undefined label 'b'", e.message);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd86861^!)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=cd86861)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=cd86861)  
[test/mjsunit/regress/regress-6142.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6142.js?cl=cd86861)  
[test/webkit/fast/js/basic-strict-mode-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/js/basic-strict-mode-expected.txt?cl=cd86861)  
[test/webkit/js-continue-break-restrictions-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/js-continue-break-restrictions-expected.txt?cl=cd86861)  
  

---   

## **regress-6098.js (v8 issue)**  
   
**[Issue 6098:
 Destructuring assignment not handled correctly inside ternary operator](https://crbug.com/v8/6098)**  
**[Commit: [parser] allow patterns within left/right branches of ConditionalExpr](https://chromium.googlesource.com/v8/v8/+/ff1a155)**  
  
Date(Commit): Wed Mar 22 21:39:29 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/455221](https://chromium-review.googlesource.com/455221)  
Regress: [mjsunit/es6/regress/regress-6098.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-6098.js)  
```javascript
const fn = (c) => {
  let d = [1, 2], x = [3, 4],
      e = null,
      f = null;
  0 < c.getIn(['a']) ? [e, f] = d : [e, f] = x;
  return [e, f];
};

assertEquals([3, 4], fn({ getIn(x) { return false; } }));
assertEquals([1, 2], fn({ getIn(x) { return true; } }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ff1a155^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=ff1a155)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=ff1a155)  
[test/mjsunit/es6/regress/regress-6098.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-6098.js?cl=ff1a155)  
  

---   

## **regress-703568.js (chromium issue)**  
   
**[Issue 703568:
 CHECK failure: (location_) != nullptr in handles.h](https://crbug.com/703568)**  
**[Commit: [wasm] [asm.js] Store function start position also for init function](https://chromium.googlesource.com/v8/v8/+/a2807f2)**  
  
Date(Commit): Wed Mar 22 17:02:16 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-58", "ClusterFuzz-Verified", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/458437](https://chromium-review.googlesource.com/458437)  
Regress: [mjsunit/regress/wasm/regress-703568.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-703568.js)  
```javascript
function asm() {
  'use asm';
  return {};
}
function f() {
  asm();
  f();
}
assertThrows(() => f(), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2807f2^!)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=a2807f2)  
[test/mjsunit/regress/wasm/regression-703568.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-703568.js?cl=a2807f2)  
  

---   

## **regress-v8-6077.js (v8 issue)**  
   
**[Issue 6077:
 Random "undefined behaviour" when using Ignition/TurboFan](https://crbug.com/v8/6077)**  
**[Commit: [deoptimizer] Fill the single precision registers in the deoptimizer entry stub.](https://chromium.googlesource.com/v8/v8/+/798ffc9)**  
  
Date(Commit): Wed Mar 22 16:56:03 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2765323002](https://codereview.chromium.org/2765323002)  
Regress: [mjsunit/compiler/regress-v8-6077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-v8-6077.js)  
```javascript
var f32 = new Float32Array(40);

function foo(f32, deopt) {
  var v0 = f32[0];
  var v1 = f32[1];
  var v2 = f32[2];
  var v3 = f32[3];
  var v4 = f32[4];
  var v5 = f32[5];
  var v6 = f32[6];
  var v7 = f32[7];
  var v8 = f32[8];
  var v9 = f32[9];
  var v10 = f32[10];
  var v11 = f32[11];
  var v12 = f32[12];
  var v13 = f32[13];
  var v14 = f32[14];
  var v15 = f32[15];
  var v16 = f32[16];
  var v17 = f32[17];
  var v18 = f32[18];
  var v19 = f32[19];
  var v20 = f32[20];
  var v21 = f32[21];
  var v22 = f32[22];
  var v23 = f32[23];
  var v24 = f32[24];
  var v25 = f32[25];
  var v26 = f32[26];
  var v27 = f32[27];
  var v28 = f32[28];
  var v29 = f32[29];
  var v30 = f32[30];
  var v31 = f32[31];
  var v32 = f32[32];
  var v33 = f32[33];
  var v34 = f32[34];
  var v35 = f32[35];
  var v36 = f32[36];
  var v37 = f32[37];
  var v38 = f32[38];
  var v39 = f32[39];
  // Side effect to force the deopt after the store.
  f32[0] = v1 - 1;
  // Here we deopt once we warm up with numbers, but then we
  // pass a string as {deopt}.
  return deopt + v0 + v1 + v2 + v3 + v4 + v5 + v6 + v7 + v8 + v9 + v10 + v11 +
      v12 + v13 + v14 + v15 + v16 + v17 + v18 + v19 + v20 + v21 + v22 + v23 +
      v24 + v25 + v26 + v27 + v28 + v29 + v30 + v31 + v32 + v33 + v34 + v35 +
      v36 + v37 + v38 + v39;
}

var s = "";
for (var i = 0; i < f32.length; i++) {
  f32[i] = i;
  s += i;
}

foo(f32, 0);
foo(f32, 0);
%OptimizeFunctionOnNextCall(foo);
assertEquals("x" + s, foo(f32, "x"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/798ffc9^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=798ffc9)  
[src/arm64/deoptimizer-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/deoptimizer-arm64.cc?cl=798ffc9)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=798ffc9)  
[src/ia32/deoptimizer-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/deoptimizer-ia32.cc?cl=798ffc9)  
[src/register-configuration.h](https://cs.chromium.org/chromium/src/v8/src/register-configuration.h?cl=798ffc9)  
...  
  

---   

## **regress-crbug-703610.js (chromium issue)**  
   
**[Issue 703610:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/703610)**  
**[Commit: [turbofan] Fix lowering of Function.prototype accesses.](https://chromium.googlesource.com/v8/v8/+/37b9d65)**  
  
Date(Commit): Wed Mar 22 10:12:23 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/458396](https://chromium-review.googlesource.com/458396)  
Regress: [mjsunit/regress/regress-crbug-703610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-703610.js)  
```javascript
function fun() {};
fun.prototype = 42;
new fun();
function f() {
  return fun.prototype;
}
assertEquals(42, f());
assertEquals(42, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37b9d65^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=37b9d65)  
[test/mjsunit/regress/regress-crbug-703610.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-703610.js?cl=37b9d65)  
  

---   

## **regress-702839.js (chromium issue)**  
   
**[Issue 702839:
 CHECK failure: !it.done() in wasm-code-specialization.cc](https://crbug.com/702839)**  
**[Commit: [wasm] Identify interpreter entry as direct call target](https://chromium.googlesource.com/v8/v8/+/198bab4)**  
  
Date(Commit): Mon Mar 20 14:58:55 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/456282](https://chromium-review.googlesource.com/456282)  
Regress: [mjsunit/regress/wasm/regress-702839.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-702839.js)  
```javascript
function __f_0() {
  "use asm";
  function __f_1() { }
  return {__f_1: __f_1};
}
__f_0();
__f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/198bab4^!)  
[src/wasm/wasm-code-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-specialization.cc?cl=198bab4)  
[test/mjsunit/regress/wasm/regression-702839.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-702839.js?cl=198bab4)  
  

---   

## **regress-crbug-702798.js (chromium issue)**  
   
**[Issue 702798:
 CHECK failure: cell->value()->IsTheHole(isolate) in objects.cc](https://crbug.com/702798)**  
**[Commit: [ic] Fix 'prototype chain checks' where the holder is the receiver](https://chromium.googlesource.com/v8/v8/+/6f52dfd)**  
  
Date(Commit): Mon Mar 20 13:55:33 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/457369](https://chromium-review.googlesource.com/457369)  
Regress: [mjsunit/regress/regress-crbug-702798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702798.js)  
```javascript
this.__defineGetter__("Object", ()=>0);
__proto__ = Realm.global(Realm.create());
Object;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f52dfd^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=6f52dfd)  
[test/mjsunit/regress/regress-crbug-702798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702798.js?cl=6f52dfd)  
  

---   

## **regress-6054.js (v8 issue)**  
   
**[Issue 6054:
 Crashed while compiling wasm on Android](https://crbug.com/v8/6054)**  
**[Commit: [wasm][arm] Emit MaybeCheckConstPool in the trap code generation](https://chromium.googlesource.com/v8/v8/+/ab97fd7)**  
  
Date(Commit): Mon Mar 20 09:52:04 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2738683003](https://codereview.chromium.org/2738683003)  
Regress: [mjsunit/regress/wasm/regress-6054.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-6054.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addFunction('test', kSig_i_i)
      .addBodyWithEnd([
  kExprI32Const, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
  kExprI32LoadMem8S, 0x00, 0x00,
kExprEnd,   // @44
      ])
      .exportFunc();
  var module = builder.instantiate();
  assertEquals(0, module.exports.test(1));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ab97fd7^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=ab97fd7)  
[test/mjsunit/regress/wasm/regression-6054.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-6054.js?cl=ab97fd7)  
  

---   

## **regress-6121.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/6121)**  
**[Commit: [turbofan] Properly handle IfException projections on JSForInNext.](https://chromium.googlesource.com/v8/v8/+/a93e522)**  
  
Date(Commit): Mon Mar 20 06:32:28 2017  
Type: None  
Code Review: [https://codereview.chromium.org/2761703002](https://codereview.chromium.org/2761703002)  
Regress: [mjsunit/regress/regress-6121.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6121.js)  
```javascript
function foo(o) {
  try {
    for (var x in o) {}
    return false;
  } catch (e) {
    return true;
  }
}

var o = new Proxy({a:1},{
  getOwnPropertyDescriptor(target, property) { throw target; }
});

assertTrue(foo(o));
assertTrue(foo(o));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a93e522^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=a93e522)  
[test/mjsunit/regress/regress-6121.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6121.js?cl=a93e522)  
  

---   

## **regress-crbug-702793.js (chromium issue)**  
   
**[No Permission](https://crbug.com/702793)**  
**[Commit: Fix LoadGlobalIC for cleared WeakCells](https://chromium.googlesource.com/v8/v8/+/f89db5d)**  
  
Date(Commit): Sat Mar 18 00:52:09 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/456280](https://chromium-review.googlesource.com/456280)  
Regress: [mjsunit/regress/regress-crbug-702793.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702793.js)  
```javascript
prop = "property";

function f(o) {
  return o.prop;
}

f(this);
f(this);

delete this.prop;

gc();
assertEquals(undefined, f(this));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f89db5d^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=f89db5d)  
[test/mjsunit/regress/regress-crbug-702793.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702793.js?cl=f89db5d)  
  

---   

## **regress-crbug-702058-1.js (chromium issue)**  
   
**[Issue 702058:
 Security: ZDI-CAN-4587 - chrome OOB read (pwn2own 2017)](https://crbug.com/702058)**  
**[Commit: [csa] Bailout to the runtime for ToInteger conversion in Array.p.indexOf.](https://chromium.googlesource.com/v8/v8/+/9224d5d)**  
  
Date(Commit): Thu Mar 16 06:53:09 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Needs-Feedback", "Fracas", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "M-57", "M-58", "NodeJS-Backport-Rejected", "FoundIn-M-57", "merge-merged-5.7", "FoundIn-M-58", "merge-merged-5.8", "FoundIn-M-59", "Release-1-M57", "CVE-2017-5053", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/2756663002](https://codereview.chromium.org/2756663002)  
Regress: [mjsunit/regress/regress-crbug-702058-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702058-1.js), [mjsunit/regress/regress-crbug-702058-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702058-2.js), [mjsunit/regress/regress-crbug-702058-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702058-3.js)  
```javascript
var arr = [];
for (var i = 0; i < 100000; i++) arr[i] = 0;
var fromIndex = {valueOf: function() { arr.length = 0; }};
arr.indexOf(1, fromIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9224d5d^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=9224d5d)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=9224d5d)  
[test/mjsunit/regress/regress-crbug-702058-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702058-1.js?cl=9224d5d)  
[test/mjsunit/regress/regress-crbug-702058-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702058-2.js?cl=9224d5d)  
[test/mjsunit/regress/regress-crbug-702058-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702058-3.js?cl=9224d5d)  
  

---   

## **regress-6100.js (v8 issue)**  
   
**[Issue 6100:
 Harmony template escapes not handled properly in preparser](https://crbug.com/v8/6100)**  
**[Commit: [parser] Fix template escapes in preparser](https://chromium.googlesource.com/v8/v8/+/bb927eb)**  
  
Date(Commit): Wed Mar 15 12:44:41 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/455876](https://chromium-review.googlesource.com/455876)  
Regress: [mjsunit/harmony/regress/regress-6100.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-6100.js)  
```javascript
function check({cooked, raw, exprs}) {
  return function(strs, ...args) {
    assertArrayEquals(cooked, strs);
    assertArrayEquals(raw, strs.raw);
    assertArrayEquals(exprs, args);
  };
}


function lazy() {
  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\01'
    ],
    'exprs': []
  })`\01`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\01',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\01${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\01'
    ],
    'exprs': [
      0
    ]
  })`left${0}\01`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\01',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\01${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\1'
    ],
    'exprs': []
  })`\1`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\1',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\1${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\1'
    ],
    'exprs': [
      0
    ]
  })`left${0}\1`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\1',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\1${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\xg'
    ],
    'exprs': []
  })`\xg`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\xg',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\xg${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\xg'
    ],
    'exprs': [
      0
    ]
  })`left${0}\xg`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\xg',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\xg${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\xAg'
    ],
    'exprs': []
  })`\xAg`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\xAg',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\xAg${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\xAg'
    ],
    'exprs': [
      0
    ]
  })`left${0}\xAg`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\xAg',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\xAg${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u0'
    ],
    'exprs': []
  })`\u0`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u0',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u0${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u0'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u0`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u0',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u0${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u0g'
    ],
    'exprs': []
  })`\u0g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u0g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u0g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u0g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u0g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u0g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u0g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u00g'
    ],
    'exprs': []
  })`\u00g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u00g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u00g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u00g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u00g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u00g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u00g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u000g'
    ],
    'exprs': []
  })`\u000g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u000g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u000g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u000g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u000g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u000g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u000g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{}'
    ],
    'exprs': []
  })`\u{}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{-0}'
    ],
    'exprs': []
  })`\u{-0}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{-0}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{-0}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{-0}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{-0}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{-0}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{-0}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{g}'
    ],
    'exprs': []
  })`\u{g}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{g}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{g}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{g}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{g}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{g}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{g}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{0'
    ],
    'exprs': []
  })`\u{0`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{0',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{0${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{0'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{0`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{0',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{0${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{\\u{0}'
    ],
    'exprs': []
  })`\u{\u{0}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{\\u{0}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{\u{0}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{\\u{0}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{\u{0}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{\\u{0}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{\u{0}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{110000}'
    ],
    'exprs': []
  })`\u{110000}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{110000}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{110000}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{110000}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{110000}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{110000}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{110000}${1}right`;



  function checkMultiple(expectedArray) {
    let results = [];
    return function consume(strs, ...args) {
      if (typeof strs === 'undefined') {
        assertArrayEquals(expectedArray, results);
      } else {
        results.push({cooked: strs, raw: strs.raw, exprs: args});
        return consume;
      }
    };
  }


  checkMultiple([{
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }, {
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }])`\u``\u`();

  checkMultiple([{
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }, {
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }])` ``\u`();

  checkMultiple([{
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }, {
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }])`\u`` `();

  checkMultiple([{
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }, {
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }])` `` `();
}

(function eager() {
  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\01'
    ],
    'exprs': []
  })`\01`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\01',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\01${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\01'
    ],
    'exprs': [
      0
    ]
  })`left${0}\01`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\01',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\01${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\1'
    ],
    'exprs': []
  })`\1`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\1',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\1${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\1'
    ],
    'exprs': [
      0
    ]
  })`left${0}\1`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\1',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\1${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\xg'
    ],
    'exprs': []
  })`\xg`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\xg',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\xg${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\xg'
    ],
    'exprs': [
      0
    ]
  })`left${0}\xg`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\xg',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\xg${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\xAg'
    ],
    'exprs': []
  })`\xAg`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\xAg',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\xAg${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\xAg'
    ],
    'exprs': [
      0
    ]
  })`left${0}\xAg`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\xAg',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\xAg${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u0'
    ],
    'exprs': []
  })`\u0`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u0',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u0${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u0'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u0`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u0',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u0${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u0g'
    ],
    'exprs': []
  })`\u0g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u0g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u0g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u0g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u0g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u0g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u0g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u00g'
    ],
    'exprs': []
  })`\u00g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u00g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u00g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u00g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u00g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u00g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u00g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u000g'
    ],
    'exprs': []
  })`\u000g`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u000g',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u000g${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u000g'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u000g`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u000g',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u000g${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{}'
    ],
    'exprs': []
  })`\u{}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{-0}'
    ],
    'exprs': []
  })`\u{-0}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{-0}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{-0}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{-0}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{-0}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{-0}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{-0}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{g}'
    ],
    'exprs': []
  })`\u{g}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{g}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{g}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{g}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{g}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{g}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{g}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{0'
    ],
    'exprs': []
  })`\u{0`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{0',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{0${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{0'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{0`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{0',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{0${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{\\u{0}'
    ],
    'exprs': []
  })`\u{\u{0}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{\\u{0}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{\u{0}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{\\u{0}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{\u{0}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{\\u{0}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{\u{0}${1}right`;

  check({
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u{110000}'
    ],
    'exprs': []
  })`\u{110000}`;

  check({
    'cooked': [
      undefined,
      'right'
    ],
    'raw': [
      '\\u{110000}',
      'right'
    ],
    'exprs': [
      0
    ]
  })`\u{110000}${0}right`;

  check({
    'cooked': [
      'left',
      undefined
    ],
    'raw': [
      'left',
      '\\u{110000}'
    ],
    'exprs': [
      0
    ]
  })`left${0}\u{110000}`;

  check({
    'cooked': [
      'left',
      undefined,
      'right'
    ],
    'raw': [
      'left',
      '\\u{110000}',
      'right'
    ],
    'exprs': [
      0,
      1
    ]
  })`left${0}\u{110000}${1}right`;



  function checkMultiple(expectedArray) {
    let results = [];
    return function consume(strs, ...args) {
      if (typeof strs === 'undefined') {
        assertArrayEquals(expectedArray, results);
      } else {
        results.push({cooked: strs, raw: strs.raw, exprs: args});
        return consume;
      }
    };
  }


  checkMultiple([{
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }, {
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }])`\u``\u`();

  checkMultiple([{
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }, {
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }])` ``\u`();

  checkMultiple([{
    'cooked': [
      undefined
    ],
    'raw': [
      '\\u',
    ],
    'exprs': []
  }, {
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }])`\u`` `();

  checkMultiple([{
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }, {
    'cooked': [
      ' '
    ],
    'raw': [
      ' ',
    ],
    'exprs': []
  }])` `` `();
})();

lazy();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb927eb^!)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=bb927eb)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=bb927eb)  
[test/mjsunit/harmony/regress/regress-6100.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-6100.js?cl=bb927eb)  
  

---   

## **regress-700883.js (chromium issue)**  
   
**[No Permission](https://crbug.com/700883)**  
**[Commit: [turbofan] Fix typing for NumberMin and NumberMax to handle uninhabited types.](https://chromium.googlesource.com/v8/v8/+/5790aad)**  
  
Date(Commit): Wed Mar 15 07:46:25 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2751513006](https://codereview.chromium.org/2751513006)  
Regress: [mjsunit/compiler/regress-700883.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-700883.js)  
```javascript
function add(x) {
 return x + x;
}

add(0);
add(1);

var min = Math.min;
function foo(x) {
 x = x|0;
 let y = add(x ? 800000000000 : NaN);
 return min(y, x);
}

foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5790aad^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=5790aad)  
[test/mjsunit/compiler/regress-700883.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-700883.js?cl=5790aad)  
  

---   

## **regress-693425.js (chromium issue)**  
   
**[Issue 693425:
 LoadField of kRepTaggedSigned (Signed32) cannot be changed to kRepFloat32 in rep](https://crbug.com/693425)**  
**[Commit: [turbofan] Handle Smi -> Float32 conversion in representation changer.](https://chromium.googlesource.com/v8/v8/+/8c114d1)**  
  
Date(Commit): Wed Mar 15 07:44:59 2017  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-57", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2749193003](https://codereview.chromium.org/2749193003)  
Regress: [mjsunit/compiler/regress-693425.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-693425.js)  
```javascript
var g = 0;
g++;
var f32 = new Float32Array();
function foo() {
  f32[0 >> 2] = g;
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c114d1^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=8c114d1)  
[test/mjsunit/compiler/regress-693425.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-693425.js?cl=8c114d1)  
  

---   

## **regress-crbug-700678.js (chromium issue)**  
   
**[Issue 700678:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/700678)**  
**[Commit: [runtime] Fix KeyAccumulator for non-internalized keys.](https://chromium.googlesource.com/v8/v8/+/3b597bb)**  
  
Date(Commit): Tue Mar 14 11:19:28 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/452979](https://chromium-review.googlesource.com/452979)  
Regress: [mjsunit/regress/regress-crbug-700678.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-700678.js)  
```javascript
(function testNonConfigurableProperty() {
  function ownKeys(x) { return ["23", "length"]; }
  var target = [];
  var proxy = new Proxy(target, {ownKeys:ownKeys});
  Object.defineProperty(target, "23", {value:true});
  assertEquals(["23", "length"], Object.getOwnPropertyNames(proxy));
})();

(function testPreventedExtension() {
  function ownKeys(x) { return ["42", "length"]; }
  var target = [];
  var proxy = new Proxy(target, {ownKeys:ownKeys});
  target[42] = true;
  Object.preventExtensions(target);
  assertEquals(["42", "length"], Object.getOwnPropertyNames(proxy));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b597bb^!)  
[src/keys.cc](https://cs.chromium.org/chromium/src/v8/src/keys.cc?cl=3b597bb)  
[test/mjsunit/regress/regress-crbug-700678.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-700678.js?cl=3b597bb)  
  

---   

## **regress-6082.js (v8 issue)**  
   
**[Issue 6082:
 Number.isNaN inlining crashes](https://crbug.com/v8/6082)**  
**[Commit: [turbofan] Fix lowering of Number.isNaN().](https://chromium.googlesource.com/v8/v8/+/9bee8f1)**  
  
Date(Commit): Mon Mar 13 07:00:59 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2743183003](https://codereview.chromium.org/2743183003)  
Regress: [mjsunit/regress/regress-6082.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6082.js)  
```javascript
function foo() { return Number.isNaN(); }
assertFalse(foo());
assertFalse(foo());
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bee8f1^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=9bee8f1)  
[test/mjsunit/regress/regress-6082.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6082.js?cl=9bee8f1)  
  

---   

## **regress-crbug-699282.js (chromium issue)**  
   
**[Issue 699282:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/699282)**  
**[Commit: [turbofan] Revert invalid optimization of flooring division.](https://chromium.googlesource.com/v8/v8/+/18be5d7)**  
  
Date(Commit): Thu Mar 09 13:41:39 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2743673002](https://codereview.chromium.org/2743673002)  
Regress: [mjsunit/regress/regress-crbug-699282.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-699282.js)  
```javascript
var v = 1;
function foo() { return Math.floor(-v / 125); }
assertEquals(-1, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(-1, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18be5d7^!)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=18be5d7)  
[test/mjsunit/regress/regress-crbug-699282.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-699282.js?cl=18be5d7)  
  

---   

## **regress-698790.js (chromium issue)**  
   
**[Issue 698790:
 IsFlat() in objects-inl.h](https://crbug.com/698790)**  
**[Commit: [regexp] Properly flatten string during initialization](https://chromium.googlesource.com/v8/v8/+/5002a4a)**  
  
Date(Commit): Thu Mar 09 12:25:19 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2736383003](https://codereview.chromium.org/2736383003)  
Regress: [mjsunit/regress/regress-698790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-698790.js)  
```javascript
var cons_string = %ConstructConsString("", "aaaaaaaaaaaaaa");
new RegExp(cons_string);


function make_cons_string(s) { return s + "aaaaaaaaaaaaaa"; }
make_cons_string("");
%OptimizeFunctionOnNextCall(make_cons_string);
var cons_str = make_cons_string("");
new RegExp(cons_str);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5002a4a^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=5002a4a)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=5002a4a)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=5002a4a)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=5002a4a)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=5002a4a)  
...  
  

---   

## **regress-6063.js (v8 issue)**  
   
**[Issue 6063:
 gpu test failing "gpu_tests.maps_integration_test.MapsIntegrationTest.Maps_maps_004"](https://crbug.com/v8/6063)**  
**[Commit: [ia32] Fix invalid DCHECK on cmpw with immediate.](https://chromium.googlesource.com/v8/v8/+/6560707)**  
  
Date(Commit): Wed Mar 08 13:04:45 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2735363002](https://codereview.chromium.org/2735363002)  
Regress: [mjsunit/regress/regress-6063.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-6063.js)  
```javascript
var U16 = new Uint16Array(2);
U16[0] = 0xffff;

function foo(a, i) {
  return U16[0] === 0xffff;
}

assertTrue(foo());
assertTrue(foo());
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6560707^!)  
[src/ia32/assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.cc?cl=6560707)  
[test/mjsunit/regress/regress-6063.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-6063.js?cl=6560707)  
  

---   

## **regress-698587.js (chromium issue)**  
   
**[Issue 698587:
 Crash in v8::internal::Invoke](https://crbug.com/698587)**  
**[Commit: [wasm] Fix code specialization for empty memory buffer](https://chromium.googlesource.com/v8/v8/+/7d8a302)**  
  
Date(Commit): Mon Mar 06 13:39:54 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.8"]  
Code Review: [https://chromium-review.googlesource.com/450257](https://chromium-review.googlesource.com/450257)  
Regress: [mjsunit/regress/wasm/regress-698587.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-698587.js)  
```javascript
var heap = new ArrayBuffer();
function asm(stdlib, ffi, heap) {
  "use asm";
  return {};
}
asm({}, {}, heap);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d8a302^!)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=7d8a302)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=7d8a302)  
[src/wasm/wasm-code-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-code-specialization.cc?cl=7d8a302)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=7d8a302)  
[test/cctest/compiler/test-run-wasm-machops.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-wasm-machops.cc?cl=7d8a302)  
...  
  

---   

## **regress-crbug-698607.js (chromium issue)**  
   
**[Issue 698607:
 Encountered unaccounted use by #635 (ObjectIsNaN) in escape-analysis.cc](https://crbug.com/698607)**  
**[Commit: [turbofan] Teach escape analysis about ObjectIsNaN.](https://chromium.googlesource.com/v8/v8/+/1e4a272)**  
  
Date(Commit): Mon Mar 06 12:55:28 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2735633003](https://codereview.chromium.org/2735633003)  
Regress: [mjsunit/regress/regress-crbug-698607.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-698607.js)  
```javascript
function assertSame(expected, found) {
  if (found === expected) {
  } else if ((expected !== expected) && (found !== found)) {
  }
}

function foo() {
  var x = {var: 0.5};
  assertSame(x, x.val);
  return () => x;
}

foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e4a272^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=1e4a272)  
[test/mjsunit/regress/regress-crbug-698607.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-698607.js?cl=1e4a272)  
  

---   

## **regress-unlink-closures-on-deopt.js (other issue)**  
   
**[Commit: [deoptimizer] When deoptimizing code, unlink all functions referring to that code.](https://chromium.googlesource.com/v8/v8/+/fe70447)**  
  
Date(Commit): Mon Mar 06 12:22:05 2017  
Code Review: [https://codereview.chromium.org/2730323002](https://codereview.chromium.org/2730323002)  
Regress: [mjsunit/regress/regress-unlink-closures-on-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-unlink-closures-on-deopt.js)  
```javascript
function foo() {
  function g(o) {
    return o.f;
  }
  return g;
}

let g1 = foo();
let g2 = foo();

g1({ f : 1});
g1({ f : 2});
g2({ f : 2});
g2({ f : 2});

%OptimizeFunctionOnNextCall(g1);
%OptimizeFunctionOnNextCall(g2);

g1({ f : 1});
g2({ f : 2});
g1({});

assertUnoptimized(g1);

assertUnoptimized(g2);

g2({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe70447^!)  
[src/debug/liveedit.cc](https://cs.chromium.org/chromium/src/v8/src/debug/liveedit.cc?cl=fe70447)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=fe70447)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=fe70447)  
[src/log.cc](https://cs.chromium.org/chromium/src/v8/src/log.cc?cl=fe70447)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=fe70447)  
...  
  
  
---   

## **regress-crbug-688567.js (chromium issue)**  
   
**[Issue 688567:
 Sloppy block function hoisting does not maintain declaration order](https://crbug.com/688567)**  
**[Commit: Retain source order when hoisting sloppy block functions](https://chromium.googlesource.com/v8/v8/+/fb16583)**  
  
Date(Commit): Thu Mar 02 21:06:00 2017  
Components/Type: Blink>JavaScript>Language/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/448701](https://chromium-review.googlesource.com/448701)  
Regress: [mjsunit/regress/regress-crbug-688567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-688567.js)  
```javascript
{
  function a(){}
  function b(){}
  function c(){}
  function d(){}
  function e(){}
  function f(){}
  function g(){}
  function h(){}
}

var names = Object.getOwnPropertyNames(this);
names = names.filter(n => Array.prototype.includes.call("abcdefgh", n));
assertEquals("a,b,c,d,e,f,g,h", names.join());

{
  {
    let j;
    {
      // This j will not be hoisted
      function j(){}
    }
  }
  function i(){}

  // but this j will be.
  function j(){}
}

var names = Object.getOwnPropertyNames(this);
names = names.filter(n => Array.prototype.includes.call("ij", n));
assertEquals("i,j", names.join());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb16583^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=fb16583)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=fb16583)  
[test/mjsunit/regress/regress-crbug-688567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-688567.js?cl=fb16583)  
  

---   

## **regress-696651.js (chromium issue)**  
   
**[No Permission](https://crbug.com/696651)**  
**[Commit: [runtime] Fix flattening of ConsStrings with empty first parts.](https://chromium.googlesource.com/v8/v8/+/c776223)**  
  
Date(Commit): Wed Mar 01 12:50:32 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://chromium-review.googlesource.com/448457](https://chromium-review.googlesource.com/448457)  
Regress: [mjsunit/regress/regress-696651.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-696651.js)  
```javascript
function get_a() { return "aaaaaaaaaaaaaa"; }
function get_b() { return "bbbbbbbbbbbbbb"; }

function get_string() {
  return get_a() + get_b();
}

function prefix(s) {
  return s + get_string();
}

prefix("");
prefix("");
%OptimizeFunctionOnNextCall(prefix);
var s = prefix("");
assertFalse(s === "aaaaaaaaaaaaaabbbbbbbbbbbbbc");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c776223^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c776223)  
[test/mjsunit/regress/regress-696651.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-696651.js?cl=c776223)  
  

---   

## **regress-crbug-697017.js (chromium issue)**  
   
**[Issue 697017:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSFunction()) in objects-i](https://crbug.com/697017)**  
**[Commit: [runtime] Properly handle null constructor case when feeding back normalization.](https://chromium.googlesource.com/v8/v8/+/e003d21)**  
  
Date(Commit): Wed Mar 01 10:02:14 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/448217](https://chromium-review.googlesource.com/448217)  
Regress: [mjsunit/regress/regress-crbug-697017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-697017.js)  
```javascript
for (var i = 0; i < 100; i++) {
  print(i);
  (Int32Array)["abc" + i] = i;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e003d21^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e003d21)  
[test/mjsunit/regress/regress-crbug-697017.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-697017.js?cl=e003d21)  
  

---   

## **regress-696332.js (chromium issue)**  
   
**[Issue 696332:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/696332)**  
**[Commit: [ast] Fix bug in deserialization of catch scopes.](https://chromium.googlesource.com/v8/v8/+/78d9d5b)**  
  
Date(Commit): Wed Mar 01 08:45:46 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/447680](https://chromium-review.googlesource.com/447680)  
Regress: [mjsunit/regress/regress-696332.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-696332.js)  
```javascript
try {
  throw 1
} catch (v) {
  function foo() { return v; }
  foo();
  v = 2
}
assertEquals(2, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78d9d5b^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=78d9d5b)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=78d9d5b)  
[test/mjsunit/regress/regress-696332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-696332.js?cl=78d9d5b)  
  

---   

## **regress-crbug-691687.js (chromium issue)**  
   
**[Issue 691687:
 new_it.done() == old_it.done() in objects-debug.cc](https://crbug.com/691687)**  
**[Commit: Accurately record eval calls in arrow parameter lists](https://chromium.googlesource.com/v8/v8/+/fc02366)**  
  
Date(Commit): Tue Feb 28 19:15:09 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://chromium-review.googlesource.com/446094](https://chromium-review.googlesource.com/446094)  
Regress: [mjsunit/regress/regress-crbug-691687.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-691687.js)  
```javascript
function g() { eval() }
with ({}) { }
f = ({x}) => x;
assertEquals(42, f({x: 42}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc02366^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=fc02366)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=fc02366)  
[src/parsing/parse-info.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parse-info.cc?cl=fc02366)  
[src/parsing/parse-info.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parse-info.h?cl=fc02366)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=fc02366)  
...  
  

---   

## **regress-crbug-683667.js (chromium issue)**  
   
**[Issue 683667:
 !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/683667)**  
**[Commit: [runtime] Mark old JSGlobalProxy's map as unstable when an iframe navigates away.](https://chromium.googlesource.com/v8/v8/+/1c7f839)**  
  
Date(Commit): Tue Feb 28 17:05:51 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "M-57", "NodeJS-Backport-Rejected", "Test-Predator-Wrong", "merge-merged-5.7", "merge-merged-5.8"]  
Code Review: [https://chromium-review.googlesource.com/447638](https://chromium-review.googlesource.com/447638)  
Regress: [mjsunit/regress/regress-crbug-683667.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-683667.js)  
```javascript
var realm = Realm.create();
var g = Realm.global(realm);
var obj = {x: 0, g: g};

Realm.navigate(realm);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1c7f839^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=1c7f839)  
[src/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8.h?cl=1c7f839)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=1c7f839)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=1c7f839)  
[test/mjsunit/regress/regress-crbug-683667.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-683667.js?cl=1c7f839)  
  

---   

## **regress-crbug-696622.js (chromium issue)**  
   
**[Issue 696622:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/696622)**  
**[Commit: [turbofan] Fix lowering of %_GetSuperConstructor intrinsic.](https://chromium.googlesource.com/v8/v8/+/09a0703)**  
  
Date(Commit): Tue Feb 28 12:47:37 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/447716](https://chromium-review.googlesource.com/447716)  
Regress: [mjsunit/regress/regress-crbug-696622.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-696622.js)  
```javascript
class C {}
class D extends C { constructor() { super(...unresolved, 75) } }
D.__proto__ = null;

assertThrows(() => new D(), TypeError);
assertThrows(() => new D(), TypeError);
%OptimizeFunctionOnNextCall(D);
assertThrows(() => new D(), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09a0703^!)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=09a0703)  
[src/interpreter/bytecode-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-decoder.cc?cl=09a0703)  
[test/mjsunit/regress/regress-crbug-696622.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-696622.js?cl=09a0703)  
  

---   

## **regress-694088.js (chromium issue)**  
   
**[Issue 694088:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/694088)**  
**[Commit: [turbofan] Fix handling of typed array loads in load elimination.](https://chromium.googlesource.com/v8/v8/+/3c36aac)**  
  
Date(Commit): Tue Feb 28 12:20:19 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2725593002](https://codereview.chromium.org/2725593002)  
Regress: [mjsunit/compiler/regress-694088.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-694088.js)  
```javascript
function is_little_endian() {
  var buffer = new ArrayBuffer(4);
  HEAP32 = new Int32Array(buffer);
  HEAPU8 = new Uint8Array(buffer);
  HEAP32[0] = 255;
  if (HEAPU8[0] === 255 && HEAPU8[3] === 0)
    return true;
  else
    return false;
}

(function () {
  var buffer = new ArrayBuffer(8);
  var i32 = new Int32Array(buffer);
  var f64 = new Float64Array(buffer);

  function foo() {
    f64[0] = 1;
    var x = f64[0];
    if (is_little_endian())
      return i32[0];
    else
      return i32[1];
  }
  assertEquals(0, foo());
})();

(function () {
  var buffer = new ArrayBuffer(8);
  var i16 = new Int16Array(buffer);
  var i32 = new Int32Array(buffer);

  function foo() {
    i32[0] = 0x20001;
    var x = i32[0];
    if (is_little_endian())
      return i16[0];
    else
      return i16[1];
  }
  assertEquals(1, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c36aac^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=3c36aac)  
[test/mjsunit/compiler/regress-694088.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-694088.js?cl=3c36aac)  
  

---   

## **regress-crbug-688734.js (chromium issue)**  
   
**[Issue 688734:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/688734)**  
**[Commit: [ic] KeyedStoreIC should use a slow stub when a prototype chain contains dictionary elements.](https://chromium.googlesource.com/v8/v8/+/9760851)**  
  
Date(Commit): Mon Feb 27 13:41:11 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/446845](https://chromium-review.googlesource.com/446845)  
Regress: [mjsunit/regress/regress-crbug-688734.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-688734.js)  
```javascript
function foo(a) {
  a[0] = 3;
}
var v = [,6];
v.__proto__ = [];
foo(v);
delete v[0];
var count = 0;
v.__proto__.__defineSetter__(0, function() { count++; });
foo([1,,,2]);
foo(v);
assertEquals(1, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9760851^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=9760851)  
[test/mjsunit/regress/regress-crbug-688734.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-688734.js?cl=9760851)  
  

---   

## **regress-696251.js (chromium issue)**  
   
**[Issue 696251:
 Heap-buffer-overflow in v8::internal::Invoke](https://crbug.com/696251)**  
**[Commit: [typedarrays] Fix Out of Bound Access in TypedArraySortFast](https://chromium.googlesource.com/v8/v8/+/cd3a76d)**  
  
Date(Commit): Mon Feb 27 11:41:25 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "reward-1500", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "Merge-Merged", "Security_Severity-Medium", "Security_Impact-Head", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "reward-inprocess", "M-58", "ClusterFuzz-Verified", "merge-merged-5.8"]  
Code Review: [https://chromium-review.googlesource.com/447036](https://chromium-review.googlesource.com/447036)  
Regress: [mjsunit/regress/regress-696251.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-696251.js)  
```javascript
var a = new Uint8Array(1000);
a.fill(255);
a.sort();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd3a76d^!)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=cd3a76d)  
[test/mjsunit/regress/regress-696251.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-696251.js?cl=cd3a76d)  
  

---   

## **regress-crbug-694709.js (chromium issue)**  
   
**[Issue 694709:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/694709)**  
**[Commit: [turbofan] Fix Object.prototype.__proto__ getter reduction.](https://chromium.googlesource.com/v8/v8/+/beb94c5)**  
  
Date(Commit): Wed Feb 22 15:07:49 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/445818](https://chromium-review.googlesource.com/445818)  
Regress: [mjsunit/regress/regress-crbug-694709.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-694709.js)  
```javascript
function f(primitive) {
  return primitive.__proto__;
}
assertEquals(Symbol.prototype, f(Symbol()));
assertEquals(Symbol.prototype, f(Symbol()));
%OptimizeFunctionOnNextCall(f);
assertEquals(Symbol.prototype, f(Symbol()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/beb94c5^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=beb94c5)  
[test/mjsunit/regress/regress-crbug-694709.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-694709.js?cl=beb94c5)  
  

---   

## **regress-694433.js (chromium issue)**  
   
**[Issue 694433:
 length == 0 || (length > 0 && data != __null) in vector.h](https://crbug.com/694433)**  
**[Commit: [wasm] Enforce module size limit early enough](https://chromium.googlesource.com/v8/v8/+/cc805e4)**  
  
Date(Commit): Tue Feb 21 18:13:02 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2705233002](https://codereview.chromium.org/2705233002)  
Regress: [mjsunit/regress/wasm/regress-694433.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-694433.js)  
```javascript
var size = Math.floor(0xFFFFFFFF / 4) + 1;
(function() {
  // Note: On 32 bit, this throws in the Uint16Array constructor (size does not
  // fit in a Smi). On 64 bit, it throws in WebAssembly.validate, because the
  // size exceeds the internal module size limit.
  assertThrows(() => WebAssembly.validate(new Uint16Array(size)), RangeError);
})();
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc805e4^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=cc805e4)  
[test/mjsunit/regress/wasm/regression-694433.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-694433.js?cl=cc805e4)  
  

---   

## **regress-crbug-694416.js (chromium issue)**  
   
**[Issue 694416:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/694416)**  
**[Commit: [turbofan] Fix missing name check for keyed global load.](https://chromium.googlesource.com/v8/v8/+/875ccb4)**  
  
Date(Commit): Tue Feb 21 14:51:07 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://chromium-review.googlesource.com/445224](https://chromium-review.googlesource.com/445224)  
Regress: [mjsunit/regress/regress-crbug-694416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-694416.js)  
```javascript
var good = 23;
var boom = 42;

function foo(name) {
  return this[name];
}

assertEquals(23, foo('good'));
assertEquals(23, foo('good'));
%OptimizeFunctionOnNextCall(foo);
assertEquals(42, foo('boom'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/875ccb4^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=875ccb4)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=875ccb4)  
[test/mjsunit/regress/regress-crbug-694416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-694416.js?cl=875ccb4)  
  

---   

## **regress-5986.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/5986)**  
**[Commit: [builtins] fix incorrect return value in ArrayIncludes](https://chromium.googlesource.com/v8/v8/+/6746227)**  
  
Date(Commit): Mon Feb 20 14:41:25 2017  
Type: None  
Code Review: [https://chromium-review.googlesource.com/445006](https://chromium-review.googlesource.com/445006)  
Regress: [mjsunit/es7/regress/regress-5986.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es7/regress/regress-5986.js)  
```javascript
var array = [1.7, 1.7, 1.7];
var mutator = {
  [Symbol.toPrimitive]() {
    Object.defineProperties(array, {
      0: { get() { } },
      1: { get() { } },
      2: { get() { } },
    });
    return 0;
  }
};

assertTrue(array.includes(undefined, mutator));

function search(array, searchElement, startIndex) {
  return array.includes(searchElement, startIndex);
}

array = [1.7, 1.7, 1.7];
var not_mutator = { [Symbol.toPrimitive]() { return 0; } };
assertFalse(search(array, undefined, not_mutator));
assertFalse(search(array, undefined, not_mutator));
%OptimizeFunctionOnNextCall(search);
assertTrue(search(array, undefined, mutator));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6746227^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=6746227)  
[test/mjsunit/es7/regress/regress-5986.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/regress/regress-5986.js?cl=6746227)  
  

---   

## **regress-693500.js (chromium issue)**  
   
**[Issue 693500:
 Crash in v8::internal::Accessors::ErrorStackGetter](https://crbug.com/693500)**  
**[Commit: [regexp] Fix smi receiver in stack accessors](https://chromium.googlesource.com/v8/v8/+/3acc00a)**  
  
Date(Commit): Mon Feb 20 11:48:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.7"]  
Code Review: [https://codereview.chromium.org/2706833002](https://codereview.chromium.org/2706833002)  
Regress: [mjsunit/regress/regress-693500.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-693500.js)  
```javascript
Reflect.get(new Error(), "stack", 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3acc00a^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=3acc00a)  
[test/mjsunit/regress/regress-693500.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-693500.js?cl=3acc00a)  
  

---   

## **regress-5927.js (v8 issue)**  
   
**[Issue 5927:
 Ignition doesn't always use proper language mode.](https://crbug.com/v8/5927)**  
**[Commit: [interpreter] When generating bytecode, properly track current scope.](https://chromium.googlesource.com/v8/v8/+/8686368)**  
  
Date(Commit): Sun Feb 19 13:08:19 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/444785](https://chromium-review.googlesource.com/444785)  
Regress: [mjsunit/regress/regress-5927.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5927.js)  
```javascript
let a = Object.freeze({});
assertThrows(() => class C {[a.b = "foo"]() {}}, TypeError);
assertThrows(() => class C extends (a.c = null) {}, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8686368^!)  
[src/ast/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-numbering.cc?cl=8686368)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=8686368)  
[src/interpreter/bytecode-generator.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.h?cl=8686368)  
[test/mjsunit/regress/regress-5927.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5927.js?cl=8686368)  
  

---   

## **regress-5972.js (v8 issue)**  
   
**[Issue 5972:
 Optimized code might report `typeof document.all === 'function'` as true](https://crbug.com/v8/5972)**  
**[Commit: Fix typeof optimization for undetectable](https://chromium.googlesource.com/v8/v8/+/6302753)**  
  
Date(Commit): Sat Feb 18 12:43:37 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2697063002](https://codereview.chromium.org/2697063002)  
Regress: [mjsunit/regress/regress-5972.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5972.js)  
```javascript
var undetectable = %GetUndetectable();

function foo(a) {
  const o = a ? foo : undetectable;
  return typeof o === 'function';
}

assertFalse(foo(false));
assertFalse(foo(false));
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo(false));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6302753^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=6302753)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=6302753)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=6302753)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=6302753)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=6302753)  
...  
  

---   

## **regress-crbug-691323.js (chromium issue)**  
   
**[Issue 691323:
 Security: Information Leak in Array indexOf](https://crbug.com/691323)**  
**[Commit: [elements] Check if the backing store has been neutered for indexOf](https://chromium.googlesource.com/v8/v8/+/3a43be9)**  
  
Date(Commit): Fri Feb 17 12:49:21 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Security  
Labels: ["reward-2000", "Security", "Security_Impact-Stable", "Security_Severity-Medium", "Arch-All", "allpublic", "reward-inprocess", "M-57", "merge-merged-57", "Release-0-M57", "CVE-2017-5040", "CVE_description-submitted"]  
Code Review: [https://chromium-review.googlesource.com/441964](https://chromium-review.googlesource.com/441964)  
Regress: [mjsunit/regress/regress-crbug-691323.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-691323.js)  
```javascript
var buffer = new ArrayBuffer(0x100);
var array = new Uint8Array(buffer).fill(55);
var tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferDetach(array.buffer)
  return 0;
};


assertEquals(-1, Array.prototype.indexOf.call(array, 0x00, tmp));

buffer = new ArrayBuffer(0x100);
array = new Uint8Array(buffer).fill(55);
tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferDetach(array.buffer)
  return 0;
};


assertEquals(false, Array.prototype.includes.call(array, 0x00, tmp));

buffer = new ArrayBuffer(0x100);
array = new Uint8Array(buffer).fill(55);
tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferDetach(array.buffer)
  return 0;
};
assertEquals(true, Array.prototype.includes.call(array, undefined, tmp));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3a43be9^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=3a43be9)  
[test/mjsunit/regress/regress-crbug-691323.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-691323.js?cl=3a43be9)  
  

---   

## **regress-689450.js (chromium issue)**  
   
**[Issue 689450:
 Disposing the isolate that is entered by a thread in wasm-call.cc](https://crbug.com/689450)**  
**[Commit: [turbofan] For Word32Shl optimizations only consider the last 5 bits of the shift](https://chromium.googlesource.com/v8/v8/+/5f1661a)**  
  
Date(Commit): Thu Feb 16 12:09:32 2017  
Components/Type: None/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong"]  
Code Review: [https://chromium-review.googlesource.com/439312](https://chromium-review.googlesource.com/439312)  
Regress: [mjsunit/regress/wasm/regress-689450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-689450.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addFunction('test', kSig_i_i)
      .addBodyWithEnd([
              kExprGetLocal, 0x00,
              kExprI32Const, 0x29,
            kExprI32Shl,
            kExprI32Const, 0x18,
          kExprI32ShrS,
          kExprI32Const, 0x18,
        kExprI32Shl,
        kExprEnd,
      ])
      .exportFunc();
  var module = builder.instantiate();
  assertEquals(0, module.exports.test(16));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f1661a^!)  
[src/compiler/machine-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/machine-operator-reducer.cc?cl=5f1661a)  
[test/mjsunit/regress/wasm/regression-689450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-689450.js?cl=5f1661a)  
  

---   

## **regress-5974.js (v8 issue)**  
   
**[Issue 5974:
 Spread calls do not convert hole to undefined](https://crbug.com/v8/5974)**  
**[Commit: [builtins] Convert the hole to undefined when pushing spread arguments.](https://chromium.googlesource.com/v8/v8/+/817e035)**  
  
Date(Commit): Wed Feb 15 18:32:16 2017  
Type: Bug  
Code Review: [https://chromium-review.googlesource.com/443247](https://chromium-review.googlesource.com/443247)  
Regress: [mjsunit/regress/regress-5974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5974.js)  
```javascript
(function() {
  var a = Array(...Array(5)).map(() => 1);

  assertEquals([1, 1, 1, 1, 1], a);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/817e035^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=817e035)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=817e035)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=817e035)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=817e035)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=817e035)  
...  
  

---   

## **regress-5929-1.js (v8 issue)**  
   
**[Issue 5929:
 Slow TypedArray builtins](https://crbug.com/v8/5929)**  
**[Commit: Reland [typedarrays] move %TypedArray%.prototype.copyWithin to C++](https://chromium.googlesource.com/v8/v8/+/dc302c7)**  
  
Date(Commit): Wed Feb 15 14:21:18 2017  
Type: FeatureRequest  
Code Review: [https://codereview.chromium.org/2697593002](https://codereview.chromium.org/2697593002)  
Regress: [mjsunit/es6/regress/regress-5929-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-5929-1.js)  
```javascript
var buf = new ArrayBuffer(0x10000);
var arr = new Uint8Array(buf).fill(55);
var tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferDetach(arr.buffer);
  return 50;
}
arr.copyWithin(tmp);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc302c7^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=dc302c7)  
[src/builtins/builtins-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-typedarray.cc?cl=dc302c7)  
[src/builtins/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.h?cl=dc302c7)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=dc302c7)  
[src/js/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/js/typedarray.js?cl=dc302c7)  
...  
  

---   

## **regress-5692.js (v8 issue)**  
   
**[Issue 5692:
 "let" with Unicode escape sequence not supported as label](https://crbug.com/v8/5692)**  
**[Commit: ParserBase should accept ESCAPED_STRICT_RESERVED_WORD as an identifier](https://chromium.googlesource.com/v8/v8/+/e3d761d)**  
  
Date(Commit): Wed Feb 15 02:35:12 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2695973003](https://codereview.chromium.org/2695973003)  
Regress: [mjsunit/regress/regress-5692.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5692.js)  
```javascript
var wasTouched = false;
l\u0065t:
do {
  break l\u0065t;
  wasTouched = true;
} while (false);
assertFalse(wasTouched);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3d761d^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=e3d761d)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=e3d761d)  
[test/mjsunit/regress/regress-5692.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5692.js?cl=e3d761d)  
  

---   

## **regress-v8-5958.js (v8 issue)**  
   
**[Issue 5958:
 instanceof protector cell is fundamentally wrong](https://crbug.com/v8/5958)**  
**[Commit: [es2015] Remove the @@hasInstance protector cell.](https://chromium.googlesource.com/v8/v8/+/1a23620)**  
  
Date(Commit): Mon Feb 13 07:16:27 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2684033012](https://codereview.chromium.org/2684033012)  
Regress: [mjsunit/regress/regress-v8-5958.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-v8-5958.js)  
```javascript
class A {}
class B {}

A.__proto__ = new Proxy(A.__proto__, {
  get: function (target, property, receiver) {
    if (property === Symbol.hasInstance) throw new B;
  }
});

assertThrows(() => (new A) instanceof A, B);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a23620^!)  
[src/builtins/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.h?cl=1a23620)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=1a23620)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=1a23620)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=1a23620)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=1a23620)  
...  
  

---   

## **regress-crbug-686427.js (chromium issue)**  
   
**[No Permission](https://crbug.com/686427)**  
**[Commit: [crankshaft] Fix Smi overflow in {HMaybeGrowElements}.](https://chromium.googlesource.com/v8/v8/+/6c12d57)**  
  
Date(Commit): Fri Feb 10 14:20:55 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2686263002](https://codereview.chromium.org/2686263002)  
Regress: [mjsunit/regress/regress-crbug-686427.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-686427.js)  
```javascript
function f(a, base) {
  a[base + 4] = 23;
  return a;
}
var i = 1073741824;
assertEquals(23, f({}, 1)[1 + 4]);
assertEquals(23, f([], 2)[2 + 4]);
%OptimizeFunctionOnNextCall(f);
assertEquals(23, f({}, i)[i + 4]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c12d57^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=6c12d57)  
[src/crankshaft/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-codegen-arm.cc?cl=6c12d57)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=6c12d57)  
[src/crankshaft/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips/lithium-codegen-mips.cc?cl=6c12d57)  
[src/crankshaft/mips64/lithium-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips64/lithium-codegen-mips64.cc?cl=6c12d57)  
...  
  

---   

## **regress-5943.js (v8 issue)**  
   
**[Issue 5943:
 Regexp hits assertion](https://crbug.com/v8/5943)**  
**[Commit: [regexp] Ensure IrregexpExecRaw is passed a flat string](https://chromium.googlesource.com/v8/v8/+/1d3317f)**  
  
Date(Commit): Thu Feb 09 11:24:08 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2681123002](https://codereview.chromium.org/2681123002)  
Regress: [mjsunit/regress/regress-5943.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5943.js)  
```javascript
function createHTML() {
  return '' + '<div><div><di';
}

createHTML();
%OptimizeFunctionOnNextCall(createHTML);

/./.test(createHTML());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d3317f^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=1d3317f)  
[test/mjsunit/regress/regress-5943.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5943.js?cl=1d3317f)  
  

---   

## **regress-5938.js (v8 issue)**  
   
**[Issue 5938:
 Function fails to close over variable in declared in the same block](https://crbug.com/v8/5938)**  
**[Commit: [parsing] Produce same Scopes in Parser and PreParser when the params are not simple.](https://chromium.googlesource.com/v8/v8/+/9b35d8f)**  
  
Date(Commit): Wed Feb 08 17:14:30 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2638333002](https://codereview.chromium.org/2638333002)  
Regress: [mjsunit/regress/regress-5938.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5938.js)  
```javascript
let global = 0;
{
  let confusing = 13;
  function lazy_func(b = confusing) { let confusing = 0; global = b; }
  lazy_func();
}

assertEquals(13, global);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9b35d8f^!)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=9b35d8f)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=9b35d8f)  
[test/mjsunit/regress/regress-5938.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5938.js?cl=9b35d8f)  
  

---   

## **regress-688876.js (chromium issue)**  
   
**[Issue 688876:
 Crash in v8::internal::Invoke](https://crbug.com/688876)**  
**[Commit: [x64] Consider both operands when emitting the REX prefix for testb.](https://chromium.googlesource.com/v8/v8/+/59bb188)**  
  
Date(Commit): Wed Feb 08 10:27:45 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "M-58", "ClusterFuzz-Verified", "Stability-AFL"]  
Code Review: [https://chromium-review.googlesource.com/439145](https://chromium-review.googlesource.com/439145)  
Regress: [mjsunit/regress/wasm/regress-688876.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-688876.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addFunction('test', kSig_i_i)
      .addBodyWithEnd([
  kExprI32Const, 0x41,
      kExprI32Const, 0x45,
      kExprI32Const, 0x41,
     kExprI32DivU,
    kExprI32LoadMem8S, 0x00, 0x3a,
      kExprI32Const, 0x75,
       kExprI32Const, 0x75,
         kExprI32Const, 0x6e,
        kExprI32Eqz,
       kExprI32LoadMem8S, 0x00, 0x3a,
      kExprI32Add,
     kExprI32DivU,
    kExprI32LoadMem8S, 0x00, 0x74,
   kExprI32And,
  kExprI32Eqz,
 kExprI32And,
kExprMemoryGrow, 0x00,
   kExprI32Const, 0x55,
  kExprI32LoadMem8S, 0x00, 0x3a,
   kExprI32LoadMem16U, 0x00, 0x71,
   kExprI32Const, 0x00,
  kExprI32RemU,
 kExprI32And,
kExprI32Eqz,
kExprEnd,   // @44
      ])
      .exportFunc();
  var module = builder.instantiate();
  assertThrows(() => {module.exports.test(1);});
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/59bb188^!)  
[src/x64/assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/assembler-x64.cc?cl=59bb188)  
[test/mjsunit/regress/wasm/regression-688876.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-688876.js?cl=59bb188)  
  

---   

## **regress-5636-1.js (v8 issue)**  
   
**[Issue 5636:
 The variable assignment analysis (aka. maybe_assigned) is broken for several language features](https://crbug.com/v8/5636)**  
**[Commit: [parsing] Fix maybe-assigned for loop variables.](https://chromium.googlesource.com/v8/v8/+/a33fcd6)**  
  
Date(Commit): Tue Feb 07 11:45:09 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2673403003](https://codereview.chromium.org/2673403003)  
Regress: [mjsunit/regress/regress-5636-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5636-1.js), [mjsunit/regress/regress-5636-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5636-2.js)  
```javascript
function f(n) {
  var a = [];
  function g() { return x }
  for (var i = 0; i < n; ++i) {
    var x = i;
    a[i] = g;
    %OptimizeFunctionOnNextCall(g);
    g();
  }
  return a;
}
var a = f(3);
assertEquals(3, a.length);
assertEquals(2, a[0]());
assertEquals(2, a[1]());
assertEquals(2, a[2]());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a33fcd6^!)  
[src/ast/scopes.h](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.h?cl=a33fcd6)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=a33fcd6)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=a33fcd6)  
[src/parsing/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.cc?cl=a33fcd6)  
[test/cctest/parsing/test-preparser.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/parsing/test-preparser.cc?cl=a33fcd6)  
...  
  

---   

## **regress-689016.js (chromium issue)**  
   
**[No Permission](https://crbug.com/689016)**  
**[Commit: [builtins] Fix crash on stack overflow in CheckSpreadAndPushToStack.](https://chromium.googlesource.com/v8/v8/+/f4739ea)**  
  
Date(Commit): Tue Feb 07 09:58:19 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2681643004](https://codereview.chromium.org/2681643004)  
Regress: [mjsunit/regress/regress-689016.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-689016.js)  
```javascript
(function() {
  function f() {}

  assertThrows(function() {
    f(...Array(1000000));
  }, RangeError);

})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f4739ea^!)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=f4739ea)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=f4739ea)  
[src/builtins/x87/builtins-x87.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x87/builtins-x87.cc?cl=f4739ea)  
[test/mjsunit/regress/regress-689016.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-689016.js?cl=f4739ea)  
  

---   

## **regress-5638.js (v8 issue)**  
   
**[Issue 5638:
 JSCreate needs a proper frame state and IfException edges need to be rewired](https://crbug.com/v8/5638)**  
**[Commit: [turbofan] Mark {JSCreate} as potentially throwing.](https://chromium.googlesource.com/v8/v8/+/e34f536)**  
  
Date(Commit): Tue Feb 07 09:00:18 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2671203003](https://codereview.chromium.org/2671203003)  
Regress: [mjsunit/regress/regress-5638.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5638.js), [mjsunit/regress/regress-5638b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5638b.js)  
```javascript
class MyErrorA {}

class MyErrorB {}

class A {}

class B extends A {
  constructor() {
    try {
      super();
    } catch (e) {
      throw new MyErrorB();
    }
  }
}

var thrower = new Proxy(A, {
  get(target, property, receiver) {
    if (property === 'prototype') throw new MyErrorA();
  }
});

assertThrows(() => Reflect.construct(B, [], thrower), MyErrorB);
assertThrows(() => Reflect.construct(B, [], thrower), MyErrorB);
%OptimizeFunctionOnNextCall(B);
assertThrows(() => Reflect.construct(B, [], thrower), MyErrorB);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e34f536^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=e34f536)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=e34f536)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=e34f536)  
[test/mjsunit/regress/regress-5638.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5638.js?cl=e34f536)  
[test/unittests/compiler/js-create-lowering-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-create-lowering-unittest.cc?cl=e34f536)  
...  
  

---   

## **regress-688690.js (chromium issue)**  
   
**[Issue 688690:
 args[0]->IsString() in runtime-scopes.cc](https://crbug.com/688690)**  
**[Commit: [string] Don't tail-call into runtime with adaptor frames](https://chromium.googlesource.com/v8/v8/+/9576d08)**  
  
Date(Commit): Mon Feb 06 09:47:55 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2675133003](https://codereview.chromium.org/2675133003)  
Regress: [mjsunit/regress/regress-688690.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-688690.js)  
```javascript
var foo = "01234567";

foo += foo;
foo += foo;
foo += foo;
foo += foo;
foo += foo;  // foo.length = 256;

var bar = foo.replace('x', 'y', 'z');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9576d08^!)  
[src/builtins/builtins-string.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string.cc?cl=9576d08)  
[test/mjsunit/regress/regress-688690.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-688690.js?cl=9576d08)  
  

---   

## **regress-crbug-687990.js (chromium issue)**  
   
**[Issue 687990:
 We should never get here - unexpected deopt info in deoptimizer.cc](https://crbug.com/687990)**  
**[Commit: [turbofan] Properly ensure that deoptimization is enabled.](https://chromium.googlesource.com/v8/v8/+/c21d128)**  
  
Date(Commit): Fri Feb 03 14:06:06 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2675793002](https://codereview.chromium.org/2675793002)  
Regress: [mjsunit/regress/regress-crbug-687990.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-687990.js)  
```javascript
var x = 1;

var foo = (function() {
  "use asm";
  var o = this;
  return function() { o.x = null; }
})();

%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c21d128^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=c21d128)  
[test/mjsunit/regress/regress-crbug-687990.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-687990.js?cl=c21d128)  
  

---   

## **regress-5902.js (v8 issue)**  
   
**[Issue 5902:
 regexp constructor map is in dictionary mode.](https://crbug.com/v8/5902)**  
**[Commit: Add test case for built-in objects' property mode.](https://chromium.googlesource.com/v8/v8/+/b963cb2)**  
  
Date(Commit): Fri Feb 03 12:19:21 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2669423002](https://codereview.chromium.org/2669423002)  
Regress: [mjsunit/regress/regress-5902.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5902.js)  
```javascript
var log = [];

function check(predicate, item) {
  if (!predicate) log.push(item);
}

var global = this;

Object.getOwnPropertyNames(global).forEach(function(name) {
  // Only check for global properties with uppercase names.
  if (name[0] != name[0].toUpperCase()) return;

  var obj = global[name];

  // Skip non-receivers.
  if (!%IsJSReceiver(obj)) return;

  // Skip non-natives.
  if (!obj.toString().includes('native')) return;

  // Construct an instance.
  try {
    new obj();
  } catch (e) {
  }

  // Check the object.
  check(%HasFastProperties(obj), `${name}`);

  // Check the constructor.
  var constructor = obj.constructor;
  if (!%IsJSReceiver(constructor)) return;
  check(%HasFastProperties(constructor), `${name}.constructor`);

  // Check the prototype.
  var prototype = obj.prototype;
  if (!%IsJSReceiver(prototype)) return;
  check(%HasFastProperties(prototype), `${name}.prototype`);

  // Check the prototype.constructor.
  var prototype_constructor = prototype.constructor;
  if (!%IsJSReceiver(prototype_constructor)) return;
  check(
      %HasFastProperties(prototype_constructor),
      `${name}.prototype.constructor`);
});

assertEquals([], log);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b963cb2^!)  
[test/mjsunit/regress/regress-5902.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5902.js?cl=b963cb2)  
  

---   

## **regress-5911.js (v8 issue)**  
   
**[Issue 5911:
 Crash in the register allocator verifier on ia32](https://crbug.com/v8/5911)**  
**[Commit: [turbofan] more regalloc fixes](https://chromium.googlesource.com/v8/v8/+/b0e58a9)**  
  
Date(Commit): Thu Feb 02 22:33:40 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2667963004](https://codereview.chromium.org/2667963004)  
Regress: [mjsunit/regress/regress-5911.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5911.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
    var builder = new WasmModuleBuilder();
    builder.addMemory(32, 32, false);
    builder.addFunction("test", kSig_i_iii)
        .addBodyWithEnd([
            // body:
            kExprI64Const, 0x42,
            kExprI64Const, 0x7a,
            kExprI64RemU,
            kExprI64Const, 0x42,
            kExprI64Const, 0x37,
            kExprI64Mul,
            kExprI64Const, 0x36,
            kExprI64Mul,
            kExprI64Const, 0x42,
            kExprI64Ctz,
            kExprI64Ctz,
            kExprI64Shl,
            kExprF32SConvertI64,
            kExprUnreachable,
            kExprEnd,   // @21
        ])
        .exportFunc();
    var module = new WebAssembly.Module(builder.toBuffer());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0e58a9^!)  
[src/compiler/register-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/register-allocator.cc?cl=b0e58a9)  
[test/mjsunit/regress/regress-5911.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5911.js?cl=b0e58a9)  
  

---   

## **regress-682349.js (chromium issue)**  
   
**[Issue 682349:
 mashable.com crashing on linux perf waterfall](https://crbug.com/682349)**  
**[Commit: [promises] Fix .arguments on builtin function.](https://chromium.googlesource.com/v8/v8/+/5020db7)**  
  
Date(Commit): Wed Feb 01 14:06:38 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/2672453002](https://codereview.chromium.org/2672453002)  
Regress: [mjsunit/regress/regress-682349.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-682349.js)  
```javascript
let success = false;
function f() {
  success = (f.caller === null);
}
Promise.resolve().then(f);
%PerformMicrotaskCheckpoint();
assertTrue(success);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5020db7^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=5020db7)  
[test/mjsunit/regress/regress-682349.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-682349.js?cl=5020db7)  
  

---   

## **regress-crbug-685050.js (chromium issue)**  
   
**[Issue 685050:
 STANDARD_STORE == store_mode in js-native-context-specialization.cc](https://crbug.com/685050)**  
**[Commit: [turbofan] Remove over-restrictive DCHECKs.](https://chromium.googlesource.com/v8/v8/+/64eae6e)**  
  
Date(Commit): Tue Jan 31 09:00:55 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2668643002](https://codereview.chromium.org/2668643002)  
Regress: [mjsunit/regress/regress-crbug-685050.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685050.js)  
```javascript
function bar(a) {
  a[0] = 0;
  a[1] = 0;
}

var a = new Int32Array(2);
bar([1, 2, 3]);
function foo() {
  bar([1, 2, 3]);
  bar(a);
}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64eae6e^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=64eae6e)  
[test/mjsunit/regress/regress-crbug-685050.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685050.js?cl=64eae6e)  
  

---   

## **regress-crbug-687029.js (chromium issue)**  
   
**[No Permission](https://crbug.com/687029)**  
**[Commit: [turbofan] Introduce ChangeTaggedToTaggedSigned operator.](https://chromium.googlesource.com/v8/v8/+/68ae57c)**  
  
Date(Commit): Tue Jan 31 08:55:56 2017  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/2666863002](https://codereview.chromium.org/2666863002)  
Regress: [mjsunit/regress/regress-crbug-687029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-687029.js)  
```javascript
function foo(x) {
  x = Math.clz32(x);
  return "a".indexOf("a", x);
}
foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68ae57c^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=68ae57c)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=68ae57c)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=68ae57c)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=68ae57c)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=68ae57c)  
...  
  

---   

## **regress-crbug-686737.js (chromium issue)**  
   
**[Issue 686737:
 V8 correctness failure in configs: x64,fullcode:x64,ignition_turbo](https://crbug.com/686737)**  
**[Commit: [turbofan] Don't eliminate unused CheckFloat64Hole.](https://chromium.googlesource.com/v8/v8/+/b8df954)**  
  
Date(Commit): Tue Jan 31 08:30:55 2017  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2665123002](https://codereview.chromium.org/2665123002)  
Regress: [mjsunit/regress/regress-crbug-686737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-686737.js)  
```javascript
Object.prototype.__defineGetter__(0, () => { throw Error() });
var a = [,0.1];
function foo(i) { a[i]; }
foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
assertThrows(() => foo(0), Error);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8df954^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b8df954)  
[test/mjsunit/regress/regress-crbug-686737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-686737.js?cl=b8df954)  
  

---   

## **regress-crbug-685504.js (chromium issue)**  
   
**[Issue 685504:
 str->IsSeqString() || str->IsExternalString() in factory.cc](https://crbug.com/685504)**  
**[Commit: ThinStrings: fix Factory::NewProperSubString](https://chromium.googlesource.com/v8/v8/+/7438304)**  
  
Date(Commit): Mon Jan 30 18:24:16 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2660823002](https://codereview.chromium.org/2660823002)  
Regress: [mjsunit/regress/regress-crbug-685504.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685504.js)  
```javascript
var v2 = 1073741823;
var v13 = {};
function f1(a, b) {
  var v4 = a + b;
  var v1 = v4.substring(20);
  v2[v4];
  return v1;
}

v5 = f1("abcdefghijklmnopqrstuvwxyz", "abcdefghijklmnopqrstuvwxyz");
function f8(name, input, regexp) {
  var v14 = input.match(regexp);
  RegExp["$'"]}
f8("CaptureGlobal", v5, v13, []["anama"]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7438304^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=7438304)  
[test/mjsunit/regress/regress-crbug-685504.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685504.js?cl=7438304)  
  

---   

## **regress-crbug-685965.js (chromium issue)**  
   
**[Issue 685965:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/685965)**  
**[Commit: ThinStrings: fix CodeStubAssembler::SubString](https://chromium.googlesource.com/v8/v8/+/9ea3e56)**  
  
Date(Commit): Mon Jan 30 18:17:52 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2660123002](https://codereview.chromium.org/2660123002)  
Regress: [mjsunit/regress/regress-crbug-685965.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685965.js)  
```javascript
function __f_0(a) {
  var __v_3 = a + undefined;
  var __v_0 = __v_3.substring(0, 20);
  var __v_1 = {};
  __v_1[__v_3];
  return __v_0;
}
__v_4 = __f_0( "abcdefghijklmnopqrstuvwxyz");
assertEquals("bcdefg", __v_4.substring(7, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9ea3e56^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=9ea3e56)  
[test/mjsunit/regress/regress-crbug-685965.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685965.js?cl=9ea3e56)  
  

---   

## **regress-crbug-686102.js (chromium issue)**  
   
**[Issue 686102:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/686102)**  
**[Commit: [turbofan] Don't constant-fold ACCESSOR properties.](https://chromium.googlesource.com/v8/v8/+/b912851)**  
  
Date(Commit): Mon Jan 30 11:15:02 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "v8-foozzie-failure", "merge-merged-5.7"]  
Code Review: [https://codereview.chromium.org/2662793002](https://codereview.chromium.org/2662793002)  
Regress: [mjsunit/regress/regress-crbug-686102.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-686102.js)  
```javascript
var a = [];
Object.freeze(a);
function foo() {
  return a.length;
}
assertEquals(0, foo());
assertEquals(0, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(0, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b912851^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=b912851)  
[test/mjsunit/regress/regress-crbug-686102.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-686102.js?cl=b912851)  
  

---   

## **regress-675704.js (chromium issue)**  
   
**[Issue 675704:
 IsSmiDouble(constant.ToFloat64()) in code-generator.cc](https://crbug.com/675704)**  
**[Commit: [turbofan] Only use Tagged machine representation for tagged state values.](https://chromium.googlesource.com/v8/v8/+/6cd2d4b)**  
  
Date(Commit): Sat Jan 28 17:25:46 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2656243004](https://codereview.chromium.org/2656243004)  
Regress: [mjsunit/compiler/regress-675704.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-675704.js)  
```javascript
function foo(a) {
  this.a = a;
  // Note that any call would do, it doesn't need to be %MaxSmi()
  this.x = this.a + %MaxSmi();
}

function g(x) {
  new foo(2);

  if (x) {
    for (var i = 0.1; i < 1.1; i++) {
      new foo(i);
    }
  }
}

g(false);
g(false);
%OptimizeFunctionOnNextCall(g);
g(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6cd2d4b^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=6cd2d4b)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=6cd2d4b)  
[test/mjsunit/compiler/regress-675704.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-675704.js?cl=6cd2d4b)  
  

---   

## **regress-crbug-685680.js (chromium issue)**  
   
**[Issue 685680:
 Encountered unaccounted use by #350 (Call) in escape-analysis.cc](https://crbug.com/685680)**  
**[Commit: [turbofan] Introduce dedicated StringIndexOf operator.](https://chromium.googlesource.com/v8/v8/+/b975441)**  
  
Date(Commit): Fri Jan 27 12:02:42 2017  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["Stability-Crash", "Reproducible", "ReleaseBlock-Dev", "HasTestcase", "Clusterfuzz", "M-58", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2657243002](https://codereview.chromium.org/2657243002)  
Regress: [mjsunit/regress/regress-crbug-685680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685680.js)  
```javascript
function foo(s) {
  s = s + '0123456789012';
  return s.indexOf('0');
}

assertEquals(0, foo('0'));
assertEquals(0, foo('0'));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0, foo('0'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b975441^!)  
[src/builtins/builtins-string.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-string.cc?cl=b975441)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=b975441)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=b975441)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=b975441)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=b975441)  
...  
  

---   

## **regress-crbug-685634.js (chromium issue)**  
   
**[Issue 685634:
 !AreAliased(args_reg, scratch1, scratch2, scratch3) in code-generator-x64.cc](https://crbug.com/685634)**  
**[Commit: [turbofan] Don't try to optimize tail calls to .apply.](https://chromium.googlesource.com/v8/v8/+/7be3b4c)**  
  
Date(Commit): Thu Jan 26 20:52:21 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2658853002](https://codereview.chromium.org/2658853002)  
Regress: [mjsunit/regress/regress-crbug-685634.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685634.js)  
```javascript
"use strict";

function foo(f) { return f.apply(this, arguments); }
function bar() {}

foo(bar);
%OptimizeFunctionOnNextCall(foo);
foo(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7be3b4c^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=7be3b4c)  
[test/mjsunit/regress/regress-crbug-685634.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685634.js?cl=7be3b4c)  
  

---   

## **regress-5888.js (v8 issue)**  
   
**[Issue 5888:
 Crash in the register allocator verifier](https://crbug.com/v8/5888)**  
**[Commit: [turbofan] Correct regalloc blocked register behavior](https://chromium.googlesource.com/v8/v8/+/ca779b2)**  
  
Date(Commit): Thu Jan 26 15:51:47 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2652153005](https://codereview.chromium.org/2652153005)  
Regress: [mjsunit/regress/regress-5888.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5888.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(32, 32, false);
  builder.addFunction("test", kSig_i_iii)
    .addBodyWithEnd([
kExprI64Const, 0xb4, 0x42,
kExprI64Const, 0x7a,
kExprI64Const, 0x42,
kExprI64Const, 0x7a,
kExprI64Ior,
kExprI64Ctz,
kExprI64Ctz,
kExprI64Shl,
kExprI64Mul,
kExprI64Const, 0x41,
kExprI64Ctz,
kExprI64Ctz,
kExprI64Shl,
kExprF32SConvertI64,
kExprI64Const, 0x42,
kExprI64Const, 0x02,
kExprI64Const, 0x7a,
kExprI64Mul,
kExprI64Const, 0x42,
kExprI64Ctz,
kExprI64Shl,
kExprI64Const, 0x7a,
kExprI64Ctz,
kExprI64Shl,
kExprI64Mul,
kExprI64Const, 0x41,
kExprI64Ctz,
kExprI64Ctz,
kExprI64Shl,
kExprF32SConvertI64,
kExprUnreachable,
kExprEnd,   // @65
            ])
            .exportFunc();
  var module = new WebAssembly.Module(builder.toBuffer());
})();

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(16, 32, false);
  builder.addFunction("test", kSig_i_iii)
    .addBodyWithEnd([
      // body:
      kExprI64Const, 0x42,
      kExprI64Const, 0x7a,
      kExprI64Ctz,
      kExprI64Mul,
      kExprI64Ctz,
      kExprI64Const, 0x41,
      kExprI64Ctz,
      kExprI64Ctz,
      kExprI64Shl,
      kExprI64Const, 0x41,
      kExprI64Ctz,
      kExprI64Ctz,
      kExprI64Shl,
      kExprF32SConvertI64,
      kExprUnreachable,
      kExprEnd,   // @20
    ])
    .exportFunc();
  var module = new WebAssembly.Module(builder.toBuffer());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca779b2^!)  
[src/compiler/register-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/register-allocator.cc?cl=ca779b2)  
[test/mjsunit/regress/regress-5888.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5888.js?cl=ca779b2)  
  

---   

## **regress-crbug-685506.js (chromium issue)**  
   
**[Issue 685506:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/685506)**  
**[Commit: [turbofan] Ensure {CheckMaps} is not used accross mutations.](https://chromium.googlesource.com/v8/v8/+/e752bcc)**  
  
Date(Commit): Thu Jan 26 12:57:04 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2653273004](https://codereview.chromium.org/2653273004)  
Regress: [mjsunit/regress/regress-crbug-685506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-685506.js)  
```javascript
var a = {};

function init() {
  a = [];
  for (var __v_1 = 0; __v_1 < 10016; __v_1++) {
    a.push({});
  }
  a.map(function() {}) + "";
}
init();

function foo() {
  a.push((a + "!", 23));
  return a;
}
assertEquals(23, foo()[10016]);
assertEquals(23, foo()[10017]);
assertEquals(23, foo()[10018]);
%OptimizeFunctionOnNextCall(foo);
assertEquals(23, foo()[10019]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e752bcc^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=e752bcc)  
[test/mjsunit/regress/regress-crbug-685506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685506.js?cl=e752bcc)  
  

---   

## **regress-5884.js (v8 issue)**  
   
**[Issue 5884:
 Locally found fuzzer issue on ia32 #2](https://crbug.com/v8/5884)**  
**[Commit: [wasm] Do the default int64-lowering for all non-i64 stores.](https://chromium.googlesource.com/v8/v8/+/a5e7382)**  
  
Date(Commit): Thu Jan 26 09:38:13 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2656563003](https://codereview.chromium.org/2656563003)  
Regress: [mjsunit/regress/wasm/regress-5884.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-5884.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

(function() {
  var builder = new WasmModuleBuilder();
  builder.addMemory(31, 31, false);
  builder.addFunction('test', kSig_i_iii)
      .addBodyWithEnd([
        // body:
        kExprI64Const, 0x41, kExprI64Const, 0x41, kExprI64LtS, kExprI32Const,
        0x01, kExprI32StoreMem, 0x00, 0x41, kExprUnreachable,
        kExprEnd,  // @60
      ])
      .exportFunc();
  var module = builder.instantiate();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5e7382^!)  
[src/compiler/int64-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/int64-lowering.cc?cl=a5e7382)  
[test/mjsunit/regress/wasm/regression-5884.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-5884.js?cl=a5e7382)  
  

---   

## **regress-crbug-684208.js (chromium issue)**  
   
**[Issue 684208:
 V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/684208)**  
**[Commit: [deoptimizer] Preserve double bit patterns correctly.](https://chromium.googlesource.com/v8/v8/+/7376e12)**  
  
Date(Commit): Thu Jan 26 09:25:59 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "NodeJS-Backport-Rejected", "v8-foozzie-failure", "merge-merged-5.8", "merge-merged-5.9"]  
Code Review: [https://codereview.chromium.org/2652303002](https://codereview.chromium.org/2652303002)  
Regress: [mjsunit/regress/regress-crbug-684208.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-684208.js)  
```javascript
function foo() {
  var a = [1, 2.3, /*hole*/, 4.2];
  %_DeoptimizeNow();
  return a[2];
}
assertSame(undefined, foo());
assertSame(undefined, foo());
%OptimizeFunctionOnNextCall(foo)
assertSame(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7376e12^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=7376e12)  
[src/arm64/deoptimizer-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/deoptimizer-arm64.cc?cl=7376e12)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=7376e12)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=7376e12)  
[src/ia32/deoptimizer-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/deoptimizer-ia32.cc?cl=7376e12)  
...  
  

---   

## **regress-685086.js (chromium issue)**  
   
**[Issue 685086:
 Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/685086)**  
**[Commit: [Builtins] Smi-check the spread and go to runtime in CheckSpreadAndPushToStack.](https://chromium.googlesource.com/v8/v8/+/bf782ec)**  
  
Date(Commit): Wed Jan 25 13:55:58 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "M-58"]  
Code Review: [https://codereview.chromium.org/2655013002](https://codereview.chromium.org/2655013002)  
Regress: [mjsunit/regress/regress-685086.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-685086.js)  
```javascript
try {
  Math.max(...0);
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf782ec^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=bf782ec)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=bf782ec)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=bf782ec)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=bf782ec)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=bf782ec)  
...  
  

---   

## **regress-684858.js (chromium issue)**  
   
**[Issue 684858:
 Crash in v8::internal::wasm::ModuleWireBytes::GetNameOrNull](https://crbug.com/684858)**  
**[Commit: [wasm] Fix check failure on invalid name section](https://chromium.googlesource.com/v8/v8/+/0ec3a26)**  
  
Date(Commit): Wed Jan 25 11:37:48 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "M-57", "Test-Predator-Correct", "merge-merged-5.7"]  
Code Review: [https://codereview.chromium.org/2656713003](https://codereview.chromium.org/2656713003)  
Regress: [mjsunit/regress/wasm/regress-684858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-684858.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

var name = 'regression_684858';

function patchNameLength(buffer) {
  var count = 0;
  var view = new Uint8Array(buffer);
  for (var i = 0, e = view.length - name.length; i <= e; ++i) {
    var subs = String.fromCharCode.apply(null, view.slice(i, i + name.length));
    if (subs != name) continue;
    ++count;
    // One byte before this name, its length is encoded.
    // Patch this to 127, making it out of bounds.
    if (view.length >= 127) throw Error('cannot patch reliably');
    if (view[i - 1] != name.length) throw Error('unexpected length');
    view[i - 1] = 0x7f;
  }
  if (count != 1) throw Error('did not find name');
}

var builder = new WasmModuleBuilder();
builder.addFunction(name, kSig_i_v)
    .addBody([kExprI32Const, 2, kExprI32Const, 0, kExprI32DivU])
    .exportAs('main');
var buffer = builder.toBuffer();
patchNameLength(buffer);
var module = new WebAssembly.Module(buffer);
var instance = new WebAssembly.Instance(module);
assertThrows(() => instance.exports.main(), WebAssembly.RuntimeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0ec3a26^!)  
[src/wasm/module-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/module-decoder.cc?cl=0ec3a26)  
[test/mjsunit/regress/wasm/regression-684858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-684858.js?cl=0ec3a26)  
  

---   

## **regress-crbug-683581.js (chromium issue)**  
   
**[Issue 683581:
 V8 correctness failure in configs: x64,fullcode:x64,ignition_turbo](https://crbug.com/683581)**  
**[Commit: [turbofan] Fix accumulator use in bytecode analysis.](https://chromium.googlesource.com/v8/v8/+/efc8cb1)**  
  
Date(Commit): Wed Jan 25 09:14:41 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2651653005](https://codereview.chromium.org/2651653005)  
Regress: [mjsunit/regress/regress-crbug-683581.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-683581.js)  
```javascript
var v = 0;
function foo() {
  for (var i = 0; i < 70000; i++) {
    v += i;
  }
  eval();
}
foo()
assertEquals(2449965000, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/efc8cb1^!)  
[src/compiler/bytecode-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-analysis.cc?cl=efc8cb1)  
[test/mjsunit/regress/regress-crbug-683581.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-683581.js?cl=efc8cb1)  
  

---   

## **regress-crbug-682194.js (chromium issue)**  
   
**[Issue 682194:
 Security: Out-of-bounds read in V8 Array.concat](https://crbug.com/682194)**  
**[Commit: [runtime] Fix Array.prototype.concat with complex @@species](https://chromium.googlesource.com/v8/v8/+/e560815)**  
  
Date(Commit): Wed Jan 25 04:37:04 2017  
Components/Type: Blink>JavaScript>Runtime/Bug-Security  
Labels: ["Hotlist-Merge-Review", "M-56", "Security_Impact-Stable", "Security_Severity-High", "reward-7500", "allpublic", "reward-inprocess", "NodeJS-Backport-Done", "merge-merged-5.6", "merge-merged-5.7", "Release-0-M57", "CVE-2017-5030", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/2655623004](https://codereview.chromium.org/2655623004)  
Regress: [mjsunit/regress/regress-crbug-682194.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-682194.js)  
```javascript
var proxy = new Proxy([], {
  defineProperty() {
    w.length = 1;  // shorten the array so the backstore pointer is relocated
    gc();          // force gc to move the array's elements backstore
    return Object.defineProperty.apply(this, arguments);
  }
});

class MyArray extends Array {
  // custom constructor which returns a proxy object
  static get[Symbol.species](){
    return function() {
      return proxy;
    }
  };
}

var w = new MyArray(100);
w[1] = 0.1;
w[2] = 0.1;

var result = Array.prototype.concat.call(w);

assertEquals(undefined, result[0]);
assertEquals(0.1, result[1]);

for (var i = 2; i < 200; i++) {
  assertEquals(undefined, result[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e560815^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=e560815)  
[test/mjsunit/regress/regress-crbug-682194.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-682194.js?cl=e560815)  
  

---   

## **regress-5860.js (v8 issue)**  
   
**[No Permission](https://crbug.com/v8/5860)**  
**[Commit: [wasm] Do not patch memory references in imported functions.](https://chromium.googlesource.com/v8/v8/+/e9b22dd)**  
  
Date(Commit): Tue Jan 24 09:43:57 2017  
Type: None  
Code Review: [https://codereview.chromium.org/2653533003](https://codereview.chromium.org/2653533003)  
Regress: [mjsunit/regress/wasm/regress-5860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-5860.js)  
```javascript
load('test/mjsunit/wasm/wasm-module-builder.js');

let module1 = (() => {
  let builder = new WasmModuleBuilder();
  builder.addMemory(1, 1);
  builder.addFunction('load', kSig_i_i)
      .addBody([kExprI32Const, 0, kExprI32LoadMem, 0, 0])
      .exportAs('load');
  return new WebAssembly.Module(builder.toBuffer());
})();

let module2 = (() => {
  let builder = new WasmModuleBuilder();
  builder.addMemory(1, 1);
  builder.addImport('A', 'load', kSig_i_i);
  builder.addExportOfKind('load', kExternalFunction, 0);
  return new WebAssembly.Module(builder.toBuffer());
})();

let instance1 = new WebAssembly.Instance(module1);
let instance2 = new WebAssembly.Instance(module2, {A: instance1.exports});

assertEquals(0, instance2.exports.load());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e9b22dd^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=e9b22dd)  
[src/wasm/wasm-objects.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.cc?cl=e9b22dd)  
[src/wasm/wasm-objects.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-objects.h?cl=e9b22dd)  
[test/mjsunit/regress/wasm/regress-5860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regress-5860.js?cl=e9b22dd)  
  

---   

## **regress-678917.js (chromium issue)**  
   
**[Issue 678917:
 Making long string occurs crash](https://crbug.com/678917)**  
**[Commit: [crankshaft] Fix string addition to check for max length of cons string.](https://chromium.googlesource.com/v8/v8/+/dd310b4)**  
  
Date(Commit): Tue Jan 24 09:35:56 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["reward-0", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "M-57", "Via-Wizard-Security", "merge-merged-5.7", "Release-0-M57"]  
Code Review: [https://codereview.chromium.org/2653623002](https://codereview.chromium.org/2653623002)  
Regress: [mjsunit/regress/regress-678917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-678917.js)  
```javascript
s1 = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx';
s1 += s1;
s1 += s1;
s1 += s1;
s1 += s1;

s0 = 'a';

function g() {
  for (var j = 0; j < 1000000; j++) {
    s0 += s1;
  }
}

try {
  g();
} catch (e) {
}

assertEquals('x', s0[10]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dd310b4^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=dd310b4)  
[test/mjsunit/regress/regress-678917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-678917.js?cl=dd310b4)  
  

---   

## **regress-683617.js (chromium issue)**  
   
**[Issue 683617:
 Unreachable code in deoptimizer.cc](https://crbug.com/683617)**  
**[Commit: [deoptimizer] Materialize string iterators.](https://chromium.googlesource.com/v8/v8/+/6d1894e)**  
  
Date(Commit): Mon Jan 23 16:46:42 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2651553003](https://codereview.chromium.org/2651553003)  
Regress: [mjsunit/regress/regress-683617.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-683617.js)  
```javascript
global = 'n';
function f(deopt) {
  let it = global[Symbol.iterator]();
  if (deopt) {
    return it.next().value;
  }
}
f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals('n', f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d1894e^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=6d1894e)  
[test/mjsunit/regress/regress-683617.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-683617.js?cl=6d1894e)  
  

---   

## **regress-682242.js (chromium issue)**  
   
**[Issue 682242:
 TailCallMode::kDisallow == expr->tail_call_mode() in bytecode-generator.cc](https://crbug.com/682242)**  
**[Commit: [Ignition/turbo] Add a CallWithSpread bytecode.](https://chromium.googlesource.com/v8/v8/+/9622073)**  
  
Date(Commit): Mon Jan 23 09:03:35 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2629363002](https://codereview.chromium.org/2629363002)  
Regress: [mjsunit/regress/regress-682242.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-682242.js)  
```javascript
class BaseClass {
  method() {
    return 1;
  }
}
class SubClass extends BaseClass {
  method(...args) {
    return super.method(...args);
  }
}
var a = new SubClass();
a.method();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9622073^!)  
[src/ast/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-numbering.cc?cl=9622073)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=9622073)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=9622073)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=9622073)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=9622073)  
...  
  

---   

## **regress-681984.js (chromium issue)**  
   
**[Issue 681984:
 AllowExceptions::IsAllowed(this) in isolate.cc](https://crbug.com/681984)**  
**[Commit: Also suppress exception messages thrown by native scripts](https://chromium.googlesource.com/v8/v8/+/8b8c8df)**  
  
Date(Commit): Fri Jan 20 08:57:42 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2640983006](https://codereview.chromium.org/2640983006)  
Regress: [mjsunit/regress/regress-681984.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681984.js)  
```javascript
function __f_0() {
  try {
    __f_0();
  } catch(e) {

      Realm.create();
  }
}
__f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8b8c8df^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=8b8c8df)  
[test/mjsunit/regress/regress-681984.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-681984.js?cl=8b8c8df)  
  

---   

## **regress-crbug-681983.js (chromium issue)**  
   
**[Issue 681983:
 V8 correctness failure in configs: x64,fullcode:x64,ignition_staging](https://crbug.com/681983)**  
**[Commit: [turbofan] Fix translation of uint32 deopt immediates.](https://chromium.googlesource.com/v8/v8/+/7682837)**  
  
Date(Commit): Thu Jan 19 09:11:47 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Code Review: [https://codereview.chromium.org/2646463002](https://codereview.chromium.org/2646463002)  
Regress: [mjsunit/regress/regress-crbug-681983.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-681983.js)  
```javascript
function g(a) {
  a = a >>> 0;
  %_DeoptimizeNow();
  return a;
}

function f() {
  return g(-1);
}

%OptimizeFunctionOnNextCall(f);
assertEquals(4294967295, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7682837^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=7682837)  
[test/mjsunit/regress/regress-crbug-681983.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-681983.js?cl=7682837)  
  

---   

## **regress-5800.js (v8 issue)**  
   
**[Issue 5800:
 179.art gets OOB error on ia32](https://crbug.com/v8/5800)**  
**[Commit: [wasm] Fix codegen issue for i64.add and i64.sub on ia32](https://chromium.googlesource.com/v8/v8/+/037200e)**  
  
Date(Commit): Thu Jan 19 01:16:19 2017  
Type: Bug  
Code Review: [https://bugs.chromium.org/p/v8/issues/detail?id=5800](https://bugs.chromium.org/p/v8/issues/detail?id=5800)  
Regress: [mjsunit/regress/wasm/regress-5800.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-5800.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function AddTest() {
  let builder = new WasmModuleBuilder();

  builder.addFunction("main", kSig_i_v)
    .addBody([
      kExprBlock, kWasmStmt,
        kExprI64Const, 0,
        // 0x80 ... 0x10 is the LEB encoding of 0x100000000. This is chosen so
        // that the 64-bit constant has a non-zero top half. In this bug, the
        // top half was clobbering eax, leading to the function return 1 rather
        // than 0.
        kExprI64Const, 0x80, 0x80, 0x80, 0x80, 0x10,
        kExprI64Add,
        kExprI64Eqz,
        kExprBrIf, 0,
        kExprI32Const, 0,
        kExprReturn,
      kExprEnd,
      kExprI32Const, 0
    ])
    .exportFunc();
  let module = builder.instantiate();
  assertEquals(0, module.exports.main());
})();

(function SubTest() {
  let builder = new WasmModuleBuilder();

  builder.addFunction("main", kSig_i_v)
    .addBody([
      kExprBlock, kWasmStmt,
        kExprI64Const, 0,
        // 0x80 ... 0x10 is the LEB encoding of 0x100000000. This is chosen so
        // that the 64-bit constant has a non-zero top half. In this bug, the
        // top half was clobbering eax, leading to the function return 1 rather
        // than 0.
        kExprI64Const, 0x80, 0x80, 0x80, 0x80, 0x10,
        kExprI64Sub,
        kExprI64Eqz,
        kExprBrIf, 0,
        kExprI32Const, 0,
        kExprReturn,
      kExprEnd,
      kExprI32Const, 0
    ])
    .exportFunc();
  let module = builder.instantiate();
  assertEquals(0, module.exports.main());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/037200e^!)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=037200e)  
[test/cctest/wasm/test-run-wasm-64.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/wasm/test-run-wasm-64.cc?cl=037200e)  
[test/mjsunit/regress/wasm/regression-5800.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-5800.js?cl=037200e)  
  

---   

## **regress-681383.js (chromium issue)**  
   
**[Issue 681383:
 unreachable in deoptimizer.cc](https://crbug.com/681383)**  
**[Commit: [deoptimizer] Materialize array iterators in the deoptimizer.](https://chromium.googlesource.com/v8/v8/+/9091eb1)**  
  
Date(Commit): Wed Jan 18 10:55:22 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.7"]  
Code Review: [https://codereview.chromium.org/2646433002](https://codereview.chromium.org/2646433002)  
Regress: [mjsunit/regress/regress-681383.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681383.js)  
```javascript
function f(deopt) {
  let array = [42];
  let it = array[Symbol.iterator]();
  if (deopt) {
    %_DeoptimizeNow();
    return it.next().value;
  }
}

f(false);
f(false);
%OptimizeFunctionOnNextCall(f);
assertEquals(f(true), 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9091eb1^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9091eb1)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=9091eb1)  
[test/mjsunit/regress/regress-681383.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-681383.js?cl=9091eb1)  
  

---   

## **regress-674089.js (chromium issue)**  
   
**[Issue 674089:
 !info->isolate()->has_pending_exception() in asm-js.cc](https://crbug.com/674089)**  
**[Commit: [wasm][asm.js] Cancel exception and rethrow on parse failure.](https://chromium.googlesource.com/v8/v8/+/5c8022e)**  
  
Date(Commit): Wed Jan 18 09:23:13 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong", "Hotlist-Asm"]  
Code Review: [https://codereview.chromium.org/2614563002](https://codereview.chromium.org/2614563002)  
Regress: [mjsunit/asm/regress-674089.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-674089.js)  
```javascript
function outer() {
  "use asm";
  function inner() {
    switch (1) {
      case 0:
        break foo;
    }
  }
}
outer();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c8022e^!)  
[src/asmjs/asm-typer.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.cc?cl=5c8022e)  
[src/asmjs/asm-typer.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.h?cl=5c8022e)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=5c8022e)  
[test/mjsunit/asm/regress-674089.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-674089.js?cl=5c8022e)  
  

---   

## **regress-5845.js (v8 issue)**  
   
**[Issue 5845:
 Incorrect "Invalid quantifier" error in regexp parser](https://crbug.com/v8/5845)**  
**[Commit: [regexp] Implement regexp groups as wrapper.](https://chromium.googlesource.com/v8/v8/+/92acec5)**  
  
Date(Commit): Wed Jan 18 08:14:59 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2636883002](https://codereview.chromium.org/2636883002)  
Regress: [mjsunit/regress/regress-5845.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5845.js)  
```javascript
assertDoesNotThrow('/(?:(?=(foo)))?/u.exec("foo")');
assertThrows('/(?=(foo))?/u.exec("foo")');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92acec5^!)  
[src/regexp/regexp-ast.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.cc?cl=92acec5)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=92acec5)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=92acec5)  
[test/cctest/test-regexp.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-regexp.cc?cl=92acec5)  
[test/mjsunit/regress/regress-5845.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5845.js?cl=92acec5)  
  

---   

## **regress-681707.js (chromium issue)**  
   
**[Issue 681707:
 Crash in v8::internal::String::length](https://crbug.com/681707)**  
**[Commit: [wasm][asm.js] Check if a property key is a PropertyName before assumming it.](https://chromium.googlesource.com/v8/v8/+/2f08919)**  
  
Date(Commit): Wed Jan 18 06:49:21 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "reward-NA", "Clusterfuzz", "ClusterFuzz-Verified", "Hotlist-Asm"]  
Code Review: [https://codereview.chromium.org/2641513003](https://codereview.chromium.org/2641513003)  
Regress: [mjsunit/asm/regress-681707.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-681707.js)  
```javascript
var foo = (function(stdlib) {
  "use asm";
  var bar = (stdlib[0]);
  function foo() { return bar ("lala"); }
  return foo;
})(this);

try {
  nop(foo);
  foo();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f08919^!)  
[src/asmjs/asm-typer.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.cc?cl=2f08919)  
[test/mjsunit/asm/regress-681707.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-681707.js?cl=2f08919)  
  

---   

## **regress-680938.js (chromium issue)**  
   
**[Issue 680938:
 Crash in v8::internal::MemoryChunk::heap](https://crbug.com/680938)**  
**[Commit: [wasm] WebAssembly.Memory.grow() should handle the no instance case](https://chromium.googlesource.com/v8/v8/+/6934db7)**  
  
Date(Commit): Wed Jan 18 04:45:07 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "M-57", "ClusterFuzz-Verified", "merge-merged-5.7", "Hotlist-Wasm"]  
Code Review: [https://codereview.chromium.org/2638243002](https://codereview.chromium.org/2638243002)  
Regress: [mjsunit/regress/wasm/regress-680938.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-680938.js)  
```javascript
var v17 = 42;
var v32 = { initial: 1 };
v39 = new WebAssembly.Memory(v32);
v49 = v39.grow(v17);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6934db7^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=6934db7)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=6934db7)  
[src/wasm/wasm-module.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.h?cl=6934db7)  
[test/mjsunit/regress/wasm/regression-680938.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-680938.js?cl=6934db7)  
[test/mjsunit/wasm/js-api.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/js-api.js?cl=6934db7)  
...  
  

---   

## **regress-681171-1.js (chromium issue)**  
   
**[Issue 681171:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsSmi()) in objects-inl.h](https://crbug.com/681171)**  
**[Commit: [generators] Always call function with closure context when resuming.](https://chromium.googlesource.com/v8/v8/+/c5948b9)**  
  
Date(Commit): Tue Jan 17 13:44:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2639533002](https://codereview.chromium.org/2639533002)  
Regress: [mjsunit/regress/regress-681171-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681171-1.js), [mjsunit/regress/regress-681171-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681171-2.js), [mjsunit/regress/regress-681171-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681171-3.js)  
```javascript
bar = function() { }

try {
  (function() {
     bar(...(function*(){ yield 1; yield 2; yield 3; })());
   })();
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c5948b9^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=c5948b9)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=c5948b9)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=c5948b9)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=c5948b9)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=c5948b9)  
...  
  

---   

## **regress-crbug-679841.js (chromium issue)**  
   
**[Issue 679841:
 Stack-buffer-overflow in v8::internal::DoubleToRadixCString](https://crbug.com/679841)**  
**[Commit: Add test case for Number.prototype.toString (r42364).](https://chromium.googlesource.com/v8/v8/+/d33dc16)**  
  
Date(Commit): Mon Jan 16 13:49:00 2017  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-3500", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "reward-inprocess", "M-57"]  
Code Review: [https://codereview.chromium.org/2631163002](https://codereview.chromium.org/2631163002)  
Regress: [mjsunit/regress/regress-crbug-679841.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-679841.js)  
```javascript
(-1e-301).toString(2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d33dc16^!)  
[test/mjsunit/regress/regress-crbug-679841.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-679841.js?cl=d33dc16)  
  

---   

## **regress-crbug-679378.js (chromium issue)**  
   
**[Issue 679378:
 Crash in heap](https://crbug.com/679378)**  
**[Commit: [turbofan] Don't merge PropertyAccessInfos with different field maps.](https://chromium.googlesource.com/v8/v8/+/64963e1)**  
  
Date(Commit): Mon Jan 16 11:47:47 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2634953002](https://codereview.chromium.org/2634953002)  
Regress: [mjsunit/regress/regress-crbug-679378.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-679378.js)  
```javascript
var x = {};
x.__defineGetter__('0', () => 0);
x.a = {v: 1.51};

var y = {};
y.a = {u:"OK"};

function foo(o) { return o.a.u; }
foo(y);
foo(y);
foo(x);
%OptimizeFunctionOnNextCall(foo);
%DebugPrint(foo(x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/64963e1^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=64963e1)  
[test/mjsunit/regress/regress-crbug-679378.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-679378.js?cl=64963e1)  
  

---   

## **regress-5836.js (v8 issue)**  
   
**[Issue 5836:
 String.prototype.anchor and similar cause side effects](https://crbug.com/v8/5836)**  
**[Commit: String.prototype.anchor and others should not cause side effects.](https://chromium.googlesource.com/v8/v8/+/865b5e5)**  
  
Date(Commit): Fri Jan 13 08:38:23 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2628963005](https://codereview.chromium.org/2628963005)  
Regress: [mjsunit/regress/regress-5836.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5836.js)  
```javascript
var previous = RegExp.lastMatch;
'hello world'.anchor('"hi"');
assertEquals(previous, RegExp.lastMatch);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/865b5e5^!)  
[src/js/string.js](https://cs.chromium.org/chromium/src/v8/src/js/string.js?cl=865b5e5)  
[test/mjsunit/regress/regress-5836.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5836.js?cl=865b5e5)  
  

---   

## **regress-673297.js (chromium issue)**  
   
**[Issue 673297:
 [wasm] Illegal reuse of contexts](https://crbug.com/673297)**  
**[Commit: [wasm] Patch the native context embedded in compiled code](https://chromium.googlesource.com/v8/v8/+/ddbfbef)**  
  
Date(Commit): Thu Jan 12 18:30:17 2017  
Components/Type: Blink>JavaScript>WebAssembly/Bug-Security  
Labels: ["M-56", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Release-0-M56"]  
Code Review: [https://codereview.chromium.org/2623203003](https://codereview.chromium.org/2623203003)  
Regress: [mjsunit/regress/regress-673297.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-673297.js)  
```javascript
function generateAsmJs() {
  'use asm';
  function fun() { fun(); }
  return fun;
}

assertThrows(generateAsmJs(), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ddbfbef^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=ddbfbef)  
[test/mjsunit/regress/regress-673297.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-673297.js?cl=ddbfbef)  
[test/mjsunit/wasm/asm-wasm-stack.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm-stack.js?cl=ddbfbef)  
  

---   

## **regress-679727.js (chromium issue)**  
   
**[Issue 679727:
 Crash in v8::internal::AstNumberingVisitor::VisitAssignment](https://crbug.com/679727)**  
**[Commit: Parser: Fix InitializerRewriter.](https://chromium.googlesource.com/v8/v8/+/aff64e9)**  
  
Date(Commit): Thu Jan 12 15:52:00 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "M-55", "hasbisect-per-revision", "NodeJS-Backport-Review", "Via-Wizard-Crashes"]  
Code Review: [https://codereview.chromium.org/2629623002](https://codereview.chromium.org/2629623002)  
Regress: [mjsunit/regress/regress-679727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-679727.js)  
```javascript
f = (e=({} = {} = 1)) => {}; f(1);
((e = ({} = {} = {})) => {})(1)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aff64e9^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=aff64e9)  
[test/mjsunit/regress/regress-679727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-679727.js?cl=aff64e9)  
  

---   

## **regress-5669.js (v8 issue)**  
   
**[Issue 5669:
 KeyedStoreGeneric does not check for writable array length](https://crbug.com/v8/5669)**  
**[Commit: Fix: KeyedStoreGeneric must check for writable array length](https://chromium.googlesource.com/v8/v8/+/93a357c)**  
  
Date(Commit): Wed Jan 11 11:37:44 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2610343002](https://codereview.chromium.org/2610343002)  
Regress: [mjsunit/regress/regress-5669.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5669.js)  
```javascript
function f(a, i, v) { a[i] = v; }
f("make it generic", 0, 0);

var a = new Array();
Object.defineProperty(a, "length", {value: 3, writable: false});
print(JSON.stringify(Object.getOwnPropertyDescriptor(a, "length")));
assertEquals(3, a.length);
f(a, 3, 3);
assertFalse(Object.getOwnPropertyDescriptor(a, "length").writable);
assertEquals(3, a.length);

var b = new Array();
b.length = 3;
Object.freeze(b);
assertEquals(3, b.length);
f(b, 3, 3);
assertEquals(3, b.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93a357c^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=93a357c)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=93a357c)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=93a357c)  
[test/mjsunit/regress/regress-5669.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5669.js?cl=93a357c)  
  

---   

## **regress-676573.js (chromium issue)**  
   
**[Issue 676573:
 !retval.is_null() in asm-js.cc](https://crbug.com/676573)**  
**[Commit: [wasm][asm.js] Ensure final validation phase runs.](https://chromium.googlesource.com/v8/v8/+/b1cfa64)**  
  
Date(Commit): Tue Jan 10 17:47:21 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2620893002](https://codereview.chromium.org/2620893002)  
Regress: [mjsunit/asm/regress-676573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-676573.js)  
```javascript
function baz() {
  "use asm";
}
function B(stdlib, env) {
  "use asm";
  var x = env.foo | 0;
}
var bar = {
  get foo() {
  }
};
bar.__defineGetter__('foo', function() { return baz(); });
B(this, bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1cfa64^!)  
[src/asmjs/asm-typer.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.cc?cl=b1cfa64)  
[src/asmjs/asm-typer.h](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-typer.h?cl=b1cfa64)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=b1cfa64)  
[test/mjsunit/asm/regress-676573.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-676573.js?cl=b1cfa64)  
  

---   

## **regress-641885.js (chromium issue)**  
   
**[Issue 641885:
 right_val != 0 in instruction-selector-arm64.cc](https://crbug.com/641885)**  
**[Commit: [wasm][asm.js] Exclude zero left hand side in arm64 isel.](https://chromium.googlesource.com/v8/v8/+/e8188a2)**  
  
Date(Commit): Tue Jan 10 17:46:11 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Code Review: [https://codereview.chromium.org/2620953002](https://codereview.chromium.org/2620953002)  
Regress: [mjsunit/asm/regress-641885.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-641885.js)  
```javascript
var __f_2 = (function __f_4() {
  "use asm";
  function __f_2(i) {
    i = i|0;
    i = i << -2147483648 >> -1073741824;
    return i|0;
  }
  return { __f_2: __f_2 };
})().__f_2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e8188a2^!)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=e8188a2)  
[test/mjsunit/asm/regress-641885.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-641885.js?cl=e8188a2)  
  

---   

## **regress-663994.js (chromium issue)**  
   
**[Issue 663994:
 Crash in v8::internal::Isolate::PushStackTraceAndDie](https://crbug.com/663994)**  
**[Commit: [wasm] The exports property of a wasm instance should always exist](https://chromium.googlesource.com/v8/v8/+/a2081b2)**  
  
Date(Commit): Tue Jan 10 09:55:10 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs"]  
Code Review: [https://codereview.chromium.org/2622563002](https://codereview.chromium.org/2622563002)  
Regress: [mjsunit/regress/wasm/regress-663994.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/wasm/regress-663994.js)  
```javascript
load("test/mjsunit/wasm/wasm-module-builder.js");

(function() {
  var builder = new WasmModuleBuilder();
  var module = builder.instantiate();
  assertTrue(typeof(module.exports) != "undefined");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2081b2^!)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=a2081b2)  
[test/mjsunit/regress/wasm/regression-663994.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/wasm/regression-663994.js?cl=a2081b2)  
  

---   

## **regress-677685.js (chromium issue)**  
   
**[Issue 677685:
 left < right in wasm-objects.cc](https://crbug.com/677685)**  
**[Commit: [asm.js] [wasm] Store function start position for stack check](https://chromium.googlesource.com/v8/v8/+/fc327e2)**  
  
Date(Commit): Mon Jan 09 09:43:04 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "ReleaseBlock-Beta", "Reproducible", "allpublic", "Clusterfuzz", "M-57", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2609363004](https://codereview.chromium.org/2609363004)  
Regress: [mjsunit/regress/regress-677685.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-677685.js)  
```javascript
function Module(stdlib) {
  "use asm";

  var fround = stdlib.Math.fround;

  // f: double -> float
  function f(a) {
    a = +a;
    return fround(a);
  }

  return { f: f };
}

var f = Module({ Math: Math }).f;

function runNearStackLimit()  {
  function g() { try { g(); } catch(e) { f(); } };
  g();
}

(function () {
  function g() {}

  runNearStackLimit(g);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc327e2^!)  
[src/asmjs/asm-wasm-builder.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-wasm-builder.cc?cl=fc327e2)  
[src/compiler/wasm-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/wasm-compiler.cc?cl=fc327e2)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=fc327e2)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=fc327e2)  
[src/wasm/function-body-decoder.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/function-body-decoder.cc?cl=fc327e2)  
...  
  

---   

## **regress-crbug-679202.js (chromium issue)**  
   
**[Issue 679202:
 !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/679202)**  
**[Commit: [crankshaft] Properly deal with null prototype.](https://chromium.googlesource.com/v8/v8/+/5f418c8)**  
  
Date(Commit): Mon Jan 09 08:47:43 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Merge-Rejected-55", "Test-Predator-Wrong", "merge-merged-56"]  
Code Review: [https://codereview.chromium.org/2621583002](https://codereview.chromium.org/2621583002)  
Regress: [mjsunit/regress/regress-crbug-679202.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-679202.js)  
```javascript
var x = Object.prototype;

function f() { return x <= x; }

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f418c8^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=5f418c8)  
[test/mjsunit/regress/regress-crbug-679202.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-679202.js?cl=5f418c8)  
  

---   

## **regress-677055.js (chromium issue)**  
   
**[Issue 677055:
 Bad-cast to icu_58::DateFormat from icu_58::DecimalFormat;__RT_impl_Runtime_InternalDateFormatToParts;v8::internal::Runtime_InternalDateFormatToParts](https://crbug.com/677055)**  
**[Commit: [intl] Remove redundant type checking system](https://chromium.googlesource.com/v8/v8/+/aa8a2d2)**  
  
Date(Commit): Sat Jan 07 02:54:48 2017  
Components/Type: Blink>JavaScript>Internationalization/Bug-Security  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "M-57"]  
Code Review: [https://codereview.chromium.org/2600913002](https://codereview.chromium.org/2600913002)  
Regress: [mjsunit/regress/regress-677055.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-677055.js)  
```javascript
if (this.Intl) {
  v5 = new Intl.NumberFormat();
  v9 = new Intl.DateTimeFormat();
  v52 = v9["formatToParts"];
  var v55 = {};
  assertThrows(() => Reflect.apply(v52, v5, v55), TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa8a2d2^!)  
[src/i18n.cc](https://cs.chromium.org/chromium/src/v8/src/i18n.cc?cl=aa8a2d2)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=aa8a2d2)  
[src/runtime/runtime-i18n.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-i18n.cc?cl=aa8a2d2)  
[test/intl/bad-target.js](https://cs.chromium.org/chromium/src/v8/test/intl/bad-target.js?cl=aa8a2d2)  
[test/mjsunit/regress/regress-4962.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4962.js?cl=aa8a2d2)  
...  
  

---   

## **regress-4962.js (v8 issue)**  
   
**[Issue 4962:
 Illegal access when calling Intl constructor with proxy as this-value](https://crbug.com/v8/4962)**  
**[Commit: [intl] Remove redundant type checking system](https://chromium.googlesource.com/v8/v8/+/aa8a2d2)**  
  
Date(Commit): Sat Jan 07 02:54:48 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2600913002](https://codereview.chromium.org/2600913002)  
Regress: [mjsunit/regress/regress-4962.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4962.js)  
```javascript
if (this.Intl) {
  assertInstanceof(Intl.NumberFormat.call(new Proxy({},{})), Intl.NumberFormat);
  assertThrows(() =>
               Intl.DateTimeFormat.prototype.formatToParts.call(
                   new Proxy({}, {})),
               TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa8a2d2^!)  
[src/i18n.cc](https://cs.chromium.org/chromium/src/v8/src/i18n.cc?cl=aa8a2d2)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=aa8a2d2)  
[src/runtime/runtime-i18n.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-i18n.cc?cl=aa8a2d2)  
[test/intl/bad-target.js](https://cs.chromium.org/chromium/src/v8/test/intl/bad-target.js?cl=aa8a2d2)  
[test/mjsunit/regress/regress-4962.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4962.js?cl=aa8a2d2)  
...  
  

---   

## **regress-5802.js (v8 issue)**  
   
**[Issue 5802:
 Crankshaft doesn't handle abstract equality correctly for receivers](https://crbug.com/v8/5802)**  
**[Commit: [crankshaft] Fix abstract equality for receivers.](https://chromium.googlesource.com/v8/v8/+/0957241)**  
  
Date(Commit): Thu Jan 05 09:26:30 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2612213002](https://codereview.chromium.org/2612213002)  
Regress: [mjsunit/regress/regress-5802.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5802.js)  
```javascript
(function() {
  function eq(a, b) { return a == b; }

  var o = { [Symbol.toPrimitive]: () => "o" };

  assertTrue(eq(o, o));
  assertTrue(eq(o, o));
  %OptimizeFunctionOnNextCall(eq);
  assertTrue(eq(o, o));
  assertTrue(eq("o", o));
  assertTrue(eq(o, "o"));
  %OptimizeFunctionOnNextCall(eq);
  assertTrue(eq(o, o));
  assertTrue(eq("o", o));
  assertTrue(eq(o, "o"));
  assertOptimized(eq);
})();

(function() {
  function ne(a, b) { return a != b; }

  var o = { [Symbol.toPrimitive]: () => "o" };

  assertFalse(ne(o, o));
  assertFalse(ne(o, o));
  %OptimizeFunctionOnNextCall(ne);
  assertFalse(ne(o, o));
  assertFalse(ne("o", o));
  assertFalse(ne(o, "o"));
  %OptimizeFunctionOnNextCall(ne);
  assertFalse(ne(o, o));
  assertFalse(ne("o", o));
  assertFalse(ne(o, "o"));
  assertOptimized(ne);
})();

(function() {
  function eq(a, b) { return a == b; }

  var a = {};
  var b = {b};
  var u = %GetUndetectable();

  assertTrue(eq(a, a));
  assertTrue(eq(b, b));
  assertFalse(eq(a, b));
  assertFalse(eq(b, a));
  assertTrue(eq(a, a));
  assertTrue(eq(b, b));
  assertFalse(eq(a, b));
  assertFalse(eq(b, a));
  %OptimizeFunctionOnNextCall(eq);
  assertTrue(eq(a, a));
  assertTrue(eq(b, b));
  assertFalse(eq(a, b));
  assertFalse(eq(b, a));
  assertTrue(eq(null, u));
  assertTrue(eq(undefined, u));
  assertTrue(eq(u, null));
  assertTrue(eq(u, undefined));
  %OptimizeFunctionOnNextCall(eq);
  assertTrue(eq(a, a));
  assertTrue(eq(b, b));
  assertFalse(eq(a, b));
  assertFalse(eq(b, a));
  assertTrue(eq(null, u));
  assertTrue(eq(undefined, u));
  assertTrue(eq(u, null));
  assertTrue(eq(u, undefined));
  assertOptimized(eq);
})();

(function() {
  function ne(a, b) { return a != b; }

  var a = {};
  var b = {b};
  var u = %GetUndetectable();

  assertFalse(ne(a, a));
  assertFalse(ne(b, b));
  assertTrue(ne(a, b));
  assertTrue(ne(b, a));
  assertFalse(ne(a, a));
  assertFalse(ne(b, b));
  assertTrue(ne(a, b));
  assertTrue(ne(b, a));
  %OptimizeFunctionOnNextCall(ne);
  assertFalse(ne(a, a));
  assertFalse(ne(b, b));
  assertTrue(ne(a, b));
  assertTrue(ne(b, a));
  assertFalse(ne(null, u));
  assertFalse(ne(undefined, u));
  assertFalse(ne(u, null));
  assertFalse(ne(u, undefined));
  %OptimizeFunctionOnNextCall(ne);
  assertFalse(ne(a, a));
  assertFalse(ne(b, b));
  assertTrue(ne(a, b));
  assertTrue(ne(b, a));
  assertFalse(ne(null, u));
  assertFalse(ne(undefined, u));
  assertFalse(ne(u, null));
  assertFalse(ne(u, undefined));
  assertOptimized(ne);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0957241^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=0957241)  
[test/mjsunit/regress/regress-5802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5802.js?cl=0957241)  
  

---   

## **regress-crbug-677757.js (chromium issue)**  
   
**[Issue 677757:
 Fatal error in](https://crbug.com/677757)**  
**[Commit: [turbofan] Teach escape analysis about StringCharAt](https://chromium.googlesource.com/v8/v8/+/5662f99)**  
  
Date(Commit): Wed Jan 04 12:01:38 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Test-Predator-Wrong"]  
Code Review: [https://codereview.chromium.org/2606383005](https://codereview.chromium.org/2606383005)  
Regress: [mjsunit/regress/regress-crbug-677757.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-677757.js)  
```javascript
for (var i = 0; i < 50000; i++) {
 ("Foo"[0] + "barbarbarbarbarbar")[0];
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5662f99^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=5662f99)  
[test/mjsunit/regress/regress-crbug-677757.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-677757.js?cl=5662f99)  
  

---   

## **regress-670981-array-push.js (chromium issue)**  
   
**[Issue 670981:
 Crash in v8::internal::JSObject::AddDataElement](https://crbug.com/670981)**  
**[Commit: Fix empty push bug in Array.push](https://chromium.googlesource.com/v8/v8/+/fcffcba)**  
  
Date(Commit): Wed Jan 04 10:57:26 2017  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/2609973002](https://codereview.chromium.org/2609973002)  
Regress: [mjsunit/regress/regress-670981-array-push.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-670981-array-push.js)  
```javascript
var array = [];
array.length = .6e+7;
array.push( );
assertEquals(array.length, .6e+7);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fcffcba^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=fcffcba)  
[test/mjsunit/regress/regress-670981-array-push.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-670981-array-push.js?cl=fcffcba)  
  

---   

## **regress-5790.js (v8 issue)**  
   
**[Issue 5790:
 Crankshaft disables optimization for uninitialized arguments access](https://crbug.com/v8/5790)**  
**[Commit: [crankshaft] Don't bailout on uninitialized access to arguments object.](https://chromium.googlesource.com/v8/v8/+/380a020)**  
  
Date(Commit): Mon Jan 02 06:52:04 2017  
Type: Bug  
Code Review: [https://codereview.chromium.org/2607303002](https://codereview.chromium.org/2607303002)  
Regress: [mjsunit/regress/regress-5790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-5790.js)  
```javascript
function foo(a) {
  "use strict";
  if (a) return arguments[1];
}

foo(false);
foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(true, 1);
foo(true, 1);
%OptimizeFunctionOnNextCall(foo);
foo(false);
foo(true, 1);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/380a020^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=380a020)  
[test/mjsunit/regress/regress-5790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-5790.js?cl=380a020)  
  

---   
