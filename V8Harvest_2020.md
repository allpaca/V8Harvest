# V8Harvest  
The Harvest of V8 regress in 2020.  
  

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

## **regress-v8-10072.js (chromium issue)**  
   
**[Issue: 3 Failing Layout Tests](https://crbug.com/10072)**  
**[Commit: [regexp] Fix CP advancement in all SKIP_* bytecodes](https://chromium.googlesource.com/v8/v8/+/aedc824)**  
  
Date(Commit): Thu Jan 16 13:10:34 2020  
Components: Blink  
Labels: LayoutTests, Restrict-AddIssueComment-Commit  
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
