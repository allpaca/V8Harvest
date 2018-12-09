# V8Harvest  
The Harvest of V8 regress.  
  

### **crbug:909614**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo:ia32,ignition_turbo](https://crbug.com/909614)**  
**[Commit: [bigint] Make kMaxLength platform-independent.](https://chromium.googlesource.com/v8/v8/+/9d51166)**  
  
Closed: Nov 30 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:908309**  
   
**[Issue: No Permission](https://crbug.com/908309)**  
**[Commit: [turbofan] Fix types of Promise#catch() and Promise#finally().](https://chromium.googlesource.com/v8/v8/+/1bfb024)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-908309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-908309.js)  
```javascript
const p = Object.defineProperty(Promise.resolve(), 'then', {
  value() { return 0; }
});

(function() {
  function foo() { return p.catch().catch(); }

  assertThrows(foo, TypeError);
  assertThrows(foo, TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo, TypeError);
})();

(function() {
  function foo() { return p.finally().finally(); }

  assertThrows(foo, TypeError);
  assertThrows(foo, TypeError);
  %OptimizeFunctionOnNextCall(foo);
  assertThrows(foo, TypeError);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1bfb024^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=1bfb024)  
[test/mjsunit/regress/regress-crbug-908309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-908309.js?cl=1bfb024)  
  

### **crbug:906870**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/906870)**  
**[Commit: [turbofan] Properly turn `Number.min(-0,+0)` into `-0`.](https://chromium.googlesource.com/v8/v8/+/154cb3f)**  
  
Closed: Nov 21 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-906870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906870.js)  
```javascript
(function() {
  function foo() {
    return Infinity / Math.max(-0, +0);
  }

  assertEquals(+Infinity, foo());
  assertEquals(+Infinity, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(+Infinity, foo());
})();

(function() {
  function foo() {
    return Infinity / Math.max(+0, -0);
  }

  assertEquals(+Infinity, foo());
  assertEquals(+Infinity, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(+Infinity, foo());
})();

(function() {
  function foo() {
    return Infinity / Math.min(-0, +0);
  }

  assertEquals(-Infinity, foo());
  assertEquals(-Infinity, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-Infinity, foo());
})();

(function() {
  function foo() {
    return Infinity / Math.min(+0, -0);
  }

  assertEquals(-Infinity, foo());
  assertEquals(-Infinity, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(-Infinity, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/154cb3f^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=154cb3f)  
[test/mjsunit/regress/regress-crbug-906870.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-906870.js?cl=154cb3f)  
  

### **crbug:906220**  
   
**[Issue: No Permission](https://crbug.com/906220)**  
**[Commit: [turbofan] Fix negative offset handling in escape analysis.](https://chromium.googlesource.com/v8/v8/+/2bc9d01)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-906220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906220.js)  
```javascript
function foo() { new Array().pop(); }

assertEquals(undefined, foo());
assertEquals(undefined, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2bc9d01^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=2bc9d01)  
[test/mjsunit/regress/regress-crbug-906220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-906220.js?cl=2bc9d01)  
  

### **crbug:906043**  
   
**[Issue: No Permission](https://crbug.com/906043)**  
**[Commit: [runtime] Reduce spread/apply call max arguments](https://chromium.googlesource.com/v8/v8/+/4e3a17d)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-906043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-906043.js)  
```javascript
function fun(arg) {
  let x = arguments.length;
  a1 = new Array(0x10);
  a1[0] = 1.1;
  a2 = new Array(0x10);
  a2[0] = 1.1;
  a1[(x >> 16) * 21] = 1.39064994160909e-309;  
  a1[(x >> 16) * 41] = 8.91238232205e-313;  
}

var a1, a2;
var a3 = [1.1, 2.2];
a3.length = 0x11000;
a3.fill(3.3);

var a4 = [1.1];

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
[test/mjsunit/regress/regress-358090.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-358090.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-732836.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-732836.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-803750.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-803750.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-869735.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-869735.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-crbug-614727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614727.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-crbug-813450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-813450.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-crbug-906043.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-906043.js?cl=4e3a17d)  
[test/mjsunit/regress/regress-v8-6716.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-v8-6716.js?cl=4e3a17d)  
[test/mjsunit/string-indexof-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/string-indexof-1.js?cl=4e3a17d)  
[test/mozilla/mozilla.status](https://cs.chromium.org/chromium/src/v8/test/mozilla/mozilla.status?cl=4e3a17d)  
[test/webkit/fast/js/function-apply-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/js/function-apply-expected.txt?cl=4e3a17d)  
[test/webkit/fast/js/function-apply.js](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/js/function-apply.js?cl=4e3a17d)  
  

### **crbug:905457**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/905457)**  
**[Commit: [turbofan] Preserve NaN properly for NumberMin and NumberMax.](https://chromium.googlesource.com/v8/v8/+/a2f7867)**  
  
Closed: Nov 15 2018  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-905457.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-905457.js)  
```javascript
(function() {
  function foo(x) {
    return Math.abs(Math.min(+x, 0));
  }

  assertEquals(NaN, foo());
  assertEquals(NaN, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(NaN, foo());
})();

(function() {
  function foo(x) {
    return Math.abs(Math.min(-x, 0));
  }

  assertEquals(NaN, foo());
  assertEquals(NaN, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(NaN, foo());
})();

(function() {
  function foo(x) {
    return Math.abs(Math.max(0, +x));
  }

  assertEquals(NaN, foo());
  assertEquals(NaN, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(NaN, foo());
})();

(function() {
  function foo(x) {
    return Math.abs(Math.max(0, -x));
  }

  assertEquals(NaN, foo());
  assertEquals(NaN, foo());
  %OptimizeFunctionOnNextCall(foo);
  assertEquals(NaN, foo());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2f7867^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=a2f7867)  
[test/mjsunit/regress/regress-crbug-905457.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-905457.js?cl=a2f7867)  
  

### **crbug:903043**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/903043)**  
**[Commit: [turbofan] Fix -0 check for subnormals.](https://chromium.googlesource.com/v8/v8/+/56f6a76)**  
  
Closed: Nov 9 2018  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-903043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-903043.js)  
```javascript
(function() {
  function foo() {
    const x = 1e-1;
    return Object.is(-0, x * (-1e-308));
  }

  assertFalse(foo());
  assertFalse(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertFalse(foo());
})();

(function() {
  function foo(x) {
    return Object.is(-0, x * (-1e-308));
  }

  assertFalse(foo(1e-1));
  assertFalse(foo(1e-1));
  %OptimizeFunctionOnNextCall(foo);
  assertFalse(foo(1e-1));
})();

(function() {
  function foo(x) {
    return Object.is(-0, x);
  }

  assertFalse(foo(1e-1 * (-1e-308)));
  assertFalse(foo(1e-1 * (-1e-308)));
  %OptimizeFunctionOnNextCall(foo);
  assertFalse(foo(1e-1 * (-1e-308)));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56f6a76^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=56f6a76)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=56f6a76)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=56f6a76)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=56f6a76)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=56f6a76)  
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=56f6a76)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=56f6a76)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=56f6a76)  
[test/mjsunit/regress/regress-crbug-903043.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-903043.js?cl=56f6a76)  
  

### **crbug:902672**  
   
**[Issue: No Permission](https://crbug.com/902672)**  
**[Commit: [builtin] Array.p.join throws on invalid Array lengths.](https://chromium.googlesource.com/v8/v8/+/0dd0af7)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-902672.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902672.js)  
```javascript
var a = this;
var b = {};
a.length = 4294967296; 
assertThrows(() => Array.prototype.join.call(a,b), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0dd0af7^!)  
[src/builtins/array-join.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-join.tq?cl=0dd0af7)  
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=0dd0af7)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=0dd0af7)  
[test/mjsunit/regress/regress-crbug-902672.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-902672.js?cl=0dd0af7)  
  

### **crbug:902610**  
   
**[Issue: No Permission](https://crbug.com/902610)**  
**[Commit: [parser] Fix off-by-one in parameter count check](https://chromium.googlesource.com/v8/v8/+/36e1e46)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-902610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902610.js)  
```javascript
assertThrows(() => {
  
  
  var f_with_65535_args =
      eval("(function(" + Array(65535).fill("x").join(",") + "){})");
  f_with_65535_args();
}, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/36e1e46^!)  
[src/message-template.h](https://cs.chromium.org/chromium/src/v8/src/message-template.h?cl=36e1e46)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=36e1e46)  
[test/mjsunit/regress/regress-crbug-902610.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-902610.js?cl=36e1e46)  
  

### **crbug:902395**  
   
**[Issue: No Permission](https://crbug.com/902395)**  
**[Commit: [ignition] More accurate dead statement elision](https://chromium.googlesource.com/v8/v8/+/7412593)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-902395.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-902395.js)  
```javascript
function opt() {
  try{
    Object.seal({})
  }finally{
    try{
      
      
      (
        {
          toString(){
          }
        }
      ).apply(-1).x(  )
    }
    finally{
      if(2.2)
      {
        return
      }
      
      try{
        Reflect.construct
      }finally{
      }
    }
  }
}

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
  

### **crbug:900674**  
   
**[Issue: DCHECK failure in IsNumber() in objects-inl.h](https://crbug.com/900674)**  
**[Commit: [async-hooks] Fix Promise.resolve optimization with async hooks enabled](https://chromium.googlesource.com/v8/v8/+/607033a)**  
  
Closed: Nov 14 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-900674.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-900674.js)  
```javascript
function foo() {
  let val = Promise.resolve().then();
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/607033a^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=607033a)  
[test/mjsunit/regress/regress-crbug-900674.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-900674.js?cl=607033a)  
  

### **crbug:899535**  
   
**[Issue: Check failed: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray())](https://crbug.com/899535)**  
**[Commit: [elements] fix wrong cast of empty FixedArray in Array.prototype.includes](https://chromium.googlesource.com/v8/v8/+/f942791)**  
  
Closed: Oct 29 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-899535.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899535.js)  
```javascript
let a = [1.1, 2.2, 3.3];
a.includes(4.4, { toString: () => a.length = 0 });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f942791^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=f942791)  
[test/mjsunit/es7/array-includes.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/array-includes.js?cl=f942791)  
[test/mjsunit/regress/regress-crbug-899535.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899535.js?cl=f942791)  
  

### **crbug:899524**  
   
**[Issue: No Permission](https://crbug.com/899524)**  
**[Commit: [turbofan] Fix LoadElement with variable index scalar replacement.](https://chromium.googlesource.com/v8/v8/+/104d752)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-899524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899524.js)  
```javascript
function empty() { }

function baz(expected, found) {
  var start = "";
  found.length, start + 'x';
  if (expected.length === found.length) {
    for (var i = 0; i < expected.length; ++i) {
      empty(found[i]);
    }
  }
}

baz([1], new (class A extends Array {}));

(function () {
  "use strict";
  function bar() {
    baz([1,2], arguments);
  }
  function foo() {
    bar(2147483648,-[]);
  }
  foo();
  foo();
  %OptimizeFunctionOnNextCall(foo);
  foo();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/104d752^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=104d752)  
[test/mjsunit/regress/regress-crbug-899524.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899524.js?cl=104d752)  
  

### **crbug:899464**  
   
**[Issue: No Permission](https://crbug.com/899464)**  
**[Commit: [regexp] Ensure FastFlagGetter returns either 0 or 1](https://chromium.googlesource.com/v8/v8/+/6397149)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-899464.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-899464.js)  
```javascript
''.matchAll(/./u);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6397149^!)  
[src/builtins/builtins-regexp-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.cc?cl=6397149)  
[src/builtins/builtins-regexp-gen.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-regexp-gen.h?cl=6397149)  
[src/objects/js-regexp.h](https://cs.chromium.org/chromium/src/v8/src/objects/js-regexp.h?cl=6397149)  
[test/mjsunit/regress/regress-crbug-899464.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-899464.js?cl=6397149)  
  

### **crbug:898974**  
   
**[Issue: CHECK failure: !unit.failed() in module-compiler.cc](https://crbug.com/898974)**  
**[Commit: [asm.js] Fix storing float32 value into float64 heap view.](https://chromium.googlesource.com/v8/v8/+/545fa6e)**  
  
Closed: Oct 26 2018  
Type: Bug  
Components: Blink>JavaScript>WebAssembly  
Labels: ["Reproducible", "Security_Impact-None", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:898785**  
   
**[Issue: No Permission](https://crbug.com/898785)**  
**[Commit: [builtins] Fix out-of-bounds in Array#lastIndexOf().](https://chromium.googlesource.com/v8/v8/+/b8a9113)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:897514**  
   
**[Issue: No Permission](https://crbug.com/897514)**  
**[Commit: [ic] Respect PropertyDetails::KindField when following transitions](https://chromium.googlesource.com/v8/v8/+/6c703ff)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  
  spread({ a:0 });
  spread("abc");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c703ff^!)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=6c703ff)  
[test/mjsunit/regress/regress-crbug-897514.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-897514.js?cl=6c703ff)  
  

### **crbug:897406**  
   
**[Issue: CHECK failure: generator_object->is_executing() in isolate.cc](https://crbug.com/897406)**  
**[Commit: [async] Gracefully handle suspended generators.](https://chromium.googlesource.com/v8/v8/+/2a08adb)**  
  
Closed: Oct 22 2018  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-72", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:897404**  
   
**[Issue: No Permission](https://crbug.com/897404)**  
**[Commit: [builtins] Fix Array.p.join length overflow and invalid string length handling](https://chromium.googlesource.com/v8/v8/+/ec969ea)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
[test/mjsunit/array-join-invalid-string-length.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-join-invalid-string-length.js?cl=ec969ea)  
[test/mjsunit/messages.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/messages.js?cl=ec969ea)  
[test/mjsunit/regress/regress-crbug-897404.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-897404.js?cl=ec969ea)  
  

### **crbug:897098**  
   
**[Issue: DCHECK failure in !is_the_hole(index) in fixed-array-inl.h](https://crbug.com/897098)**  
**[Commit: [elements] handle OOB-holes in Array.prototype.includes fast-path](https://chromium.googlesource.com/v8/v8/+/5b92f91)**  
  
Closed: Oct 23 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "M-71", "Target-71"]  
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
  

### **crbug:896700**  
   
**[Issue: Ill in v8::internal::CaptureAsyncStackTrace](https://crbug.com/896700)**  
**[Commit: [async] Gracefully handle exceptions in async_hooks.](https://chromium.googlesource.com/v8/v8/+/e650b9e)**  
  
Closed: Oct 20 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:896181**  
   
**[Issue: Ill in v8::internal::JSArray::ArrayJoinConcatToSequentialString](https://crbug.com/896181)**  
**[Commit: [builtins] Fix Array.p.join handling of an index getter with side effects](https://chromium.googlesource.com/v8/v8/+/7cb6c81)**  
  
Closed: Oct 18 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-CC"]  
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
  

### **crbug:895199**  
   
**[Issue: No Permission](https://crbug.com/895199)**  
**[Commit: [turbofan] Fix representation selection of CheckFloat64Hole.](https://chromium.googlesource.com/v8/v8/+/63f92a9)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-895199.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-895199.js)  
```javascript
function foo() {
  var a = new Array(2);
  a[0] = 23.1234;
  a[1] = 25.1234;
  %DeoptimizeNow();
  return a[2];
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63f92a9^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=63f92a9)  
[test/mjsunit/regress/regress-crbug-895199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-895199.js?cl=63f92a9)  
  

### **crbug:892472**  
   
**[Issue: No Permission](https://crbug.com/892472)**  
**[Commit: [async] Only try to peak into async functions/generators.](https://chromium.googlesource.com/v8/v8/+/4111c98)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:891627**  
   
**[Issue: No Permission](https://crbug.com/891627)**  
**[Commit: [turbofan] Fix Word32 (Signed32OrMinusZero) conversions that identify zeros.](https://chromium.googlesource.com/v8/v8/+/513a5bd)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-891627.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-891627.js)  
```javascript
function bar(x) { return x % 2; }
bar(0.1);


(function() {
  function foo(x) {
    
    return bar(x | -1) == 4294967295;
  }

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
  makeFoo(0);  
  const foo = makeFoo(1);

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
  

### **crbug:890243**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/890243)**  
**[Commit: [turbofan] Add missing Word64->Bit support.](https://chromium.googlesource.com/v8/v8/+/852a8d3)**  
  
Closed: Oct 1 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-890243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-890243.js)  
```javascript
function bar(x) { return x + x; }
bar(0.1);





function baz(y) { return {y}; }
baz(null); baz(0);



function foo(o) {
  return !baz(bar(o.x)).y;
}

assertFalse(foo({x:1}));
assertFalse(foo({x:1}));
%OptimizeFunctionOnNextCall(foo);
assertFalse(foo({x:1}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/852a8d3^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=852a8d3)  
[test/mjsunit/regress/regress-crbug-890243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-890243.js?cl=852a8d3)  
  

### **crbug:888825**  
   
**[Issue: No Permission](https://crbug.com/888825)**  
**[Commit: [parser] Don't resolve preparser variables for arrow functions](https://chromium.googlesource.com/v8/v8/+/55ecf51)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-888825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-888825.js)  
```javascript
eval("((a=function g() { function g() {}}) => {})();");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55ecf51^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=55ecf51)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=55ecf51)  
[test/mjsunit/regress/regress-crbug-888825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-888825.js?cl=55ecf51)  
  

### **crbug:887891**  
   
**[Issue: No Permission](https://crbug.com/887891)**  
**[Commit: [es2015] Setup JSTypedArray after allocating the JSArrayBuffer.](https://chromium.googlesource.com/v8/v8/+/129f770)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:885404**  
   
**[Issue: CHECK failure: byte_length() <= JSArrayBuffer::kMaxByteLength in objects-debug.cc](https://crbug.com/885404)**  
**[Commit: [es2015] Clear JSTypedArray raw fields in the constructor.](https://chromium.googlesource.com/v8/v8/+/984048e)**  
  
Closed: Sep 19 2018  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:884933**  
   
**[Issue: Ill in v8::internal::compiler::RepresentationChanger::TypeError](https://crbug.com/884933)**  
**[Commit: [turbofan] Add missing Word8/16 -> Word64 representation changes.](https://chromium.googlesource.com/v8/v8/+/1210d0c)**  
  
Closed: Sep 18 2018  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  }

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
  }

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
  }

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
  }

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
  

### **crbug:882233**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/882233)**  
**[Commit: [array] Consistently throw TypeError for zero-length arrays](https://chromium.googlesource.com/v8/v8/+/e365bc2)**  
  
Closed: Sep 10 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:881247**  
   
**[Issue: No Permission](https://crbug.com/881247)**  
**[Commit: [CloneObjectIC] Avoid FieldType confusions](https://chromium.googlesource.com/v8/v8/+/78e5763)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:880207**  
   
**[Issue: No Permission](https://crbug.com/880207)**  
**[Commit: [turbofan] Fix incorrect typing rule for NumberExpm1.](https://chromium.googlesource.com/v8/v8/+/56f7dda)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-880207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-880207.js)  
```javascript
(function TestOptimizedFastExpm1MinusZero() {
  function foo() {
    return Object.is(Math.expm1(-0), -0);
  }

  assertTrue(foo());
  %OptimizeFunctionOnNextCall(foo);
  assertTrue(foo());
})();

(function TestOptimizedExpm1MinusZeroSlowPath() {
  function f(x) {
    return Object.is(Math.expm1(x), -0);
  }

  function g() {
    return f(-0);
  }

  f(0);
  
  
  %OptimizeFunctionOnNextCall(f);
  
  
  f("0");
  
  assertTrue(g());
  %OptimizeFunctionOnNextCall(g);
  assertTrue(g());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56f7dda^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=56f7dda)  
[test/mjsunit/regress/regress-crbug-880207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-880207.js?cl=56f7dda)  
  

### **crbug:879898**  
   
**[Issue: No Permission](https://crbug.com/879898)**  
**[Commit: [turbofan] Improve typing of ToNumeric and ToNumber.](https://chromium.googlesource.com/v8/v8/+/b898112)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-879898.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-879898.js)  
```javascript
function foo() {
  return Symbol.toPrimitive++;
}
assertThrows(foo);
%OptimizeFunctionOnNextCall(foo);
assertThrows(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b898112^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=b898112)  
[src/compiler/operation-typer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.h?cl=b898112)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=b898112)  
[test/mjsunit/regress/regress-crbug-879898.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-879898.js?cl=b898112)  
  

### **crbug:879560**  
   
**[Issue: DCHECK failure in (pointer_) != nullptr in utils.h](https://crbug.com/879560)**  
**[Commit: [turbofan] Fix typo flushed out by recent CL.](https://chromium.googlesource.com/v8/v8/+/b1bd6be)**  
  
Closed: Aug 31 2018  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-879560.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-879560.js)  
```javascript
function foo() {
  var x = 1;
  x = undefined;
  while (x--) ;
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1bd6be^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b1bd6be)  
[test/mjsunit/regress/regress-crbug-879560.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-879560.js?cl=b1bd6be)  
  

### **crbug:878845**  
   
**[Issue: CHECK failure: Type cast failed in CAST(p_o) at ../../src/code-stub-assembler.h:351 in code-ass](https://crbug.com/878845)**  
**[Commit: [array] Fix side-effect for 'from' argument in Array.p.lastIndexOf](https://chromium.googlesource.com/v8/v8/+/b9540d4)**  
  
Closed: Aug 30 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "Test-Predator-Auto-Components"]  
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
  

### **crbug:876443**  
   
**[Issue: CHECK failure: Type cast failed in CAST(p_o) at ../../src/code-stub-assembler.h:351 in code-ass](https://crbug.com/876443)**  
**[Commit: [builtins] Enable Torque Array.prototype.splice](https://chromium.googlesource.com/v8/v8/+/fd334b3)**  
  
Closed: Aug 23 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "M-71", "Target-71", "M71"]  
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
[src/builtins/base.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/base.tq?cl=fd334b3)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=fd334b3)  
[src/builtins/builtins-definitions.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-definitions.h?cl=fd334b3)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=fd334b3)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=fd334b3)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=fd334b3)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=fd334b3)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=fd334b3)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=fd334b3)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=fd334b3)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=fd334b3)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=fd334b3)  
[test/mjsunit/array-functions-prototype-misc.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-functions-prototype-misc.js?cl=fd334b3)  
[test/mjsunit/array-splice.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-splice.js?cl=fd334b3)  
[test/mjsunit/regress/regress-crbug-876443.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-876443.js?cl=fd334b3)  
[test/mjsunit/regress/regress-splice-large-index.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-splice-large-index.js?cl=fd334b3)  
[test/mozilla/mozilla.status](https://cs.chromium.org/chromium/src/v8/test/mozilla/mozilla.status?cl=fd334b3)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=fd334b3)  
[test/webkit/array-splice.js](https://cs.chromium.org/chromium/src/v8/test/webkit/array-splice.js?cl=fd334b3)  
  

### **crbug:871886**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadElements(object)) at ../../src/code-stub-assembler.](https://crbug.com/871886)**  
**[Commit: [csa] avoid FixedDoubleArray CAST on empty FixedArray](https://chromium.googlesource.com/v8/v8/+/5b74a7e)**  
  
Closed: Aug 10 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner", "M-70"]  
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
  

### **crbug:869313**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadObjectField(data_view, JSDataView::kByteLengthOffse](https://crbug.com/869313)**  
**[Commit: [dataview] Fix too tight TNode type in DataView getters](https://chromium.googlesource.com/v8/v8/+/3656b46)**  
  
Closed: Aug 3 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components", "merge-merged-6.9"]  
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
  

### **crbug:867776**  
   
**[Issue: V8 OOB write BigInt64Array.of and BigInt64Array.from side effect neuter](https://crbug.com/867776)**  
**[Commit: [csa] Fix is-neutered check in EmitBigTypedArrayElementStore](https://chromium.googlesource.com/v8/v8/+/a24d5ad)**  
  
Closed: Jul 30 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["reward-5000", "Security_Impact-Stable", "reward-decline", "Security_Severity-High", "allpublic", "reward-inprocess", "M-68", "RegressedIn-68", "Target-68", "CVE_description-missing", "merge-merged-6.8", "merge-merged-6.9", "Release-0-M69", "CVE-2018-16065"]  
Regress: [mjsunit/regress/regress-crbug-867776.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-867776.js)  
```javascript
for (var i = 0; i < 3; i++) {
  var array = new BigInt64Array(200);

  function evil_callback() {
    %ArrayBufferNeuter(array.buffer);
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
  

### **crbug:866315**  
   
**[Issue: Ill in v8::Utils::ReportApiFailure](https://crbug.com/866315)**  
**[Commit: [async] Fix a crash when AsyncHooks is used in the proto of an object](https://chromium.googlesource.com/v8/v8/+/2d0a764)**  
  
Closed: Jul 23 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:865892**  
   
**[Issue: CHECK failure: !isolate->has_scheduled_exception() in builtins-console.cc](https://crbug.com/865892)**  
**[Commit: [async] Improve error handling when running async hooks](https://chromium.googlesource.com/v8/v8/+/4a28271)**  
  
Closed: Jul 23 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:865312**  
   
**[Issue: DCHECK failure in end <= array->length_value() in elements.cc](https://crbug.com/865312)**  
**[Commit: [array] Only use fast-path in Array.p.fill for JSArrays](https://chromium.googlesource.com/v8/v8/+/b87e762)**  
  
Closed: Jul 19 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "Test-Predator-Auto-Owner", "Target-69"]  
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
  

### **crbug:862538**  
   
**[Issue: Ill in v8::internal::ScannerStream::For](https://crbug.com/862538)**  
**[Commit: [scanner] Fix scanner stream creation: Sliced strings can have an underlying thin string.](https://chromium.googlesource.com/v8/v8/+/ae044d69)**  
  
Closed: Jul 12 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:860788**  
   
**[Issue: CHECK failure: !isolate->has_scheduled_exception() in builtins-console.cc](https://crbug.com/860788)**  
**[Commit: [async] Implement error handling when running async hooks](https://chromium.googlesource.com/v8/v8/+/614c807)**  
  
Closed: Jul 10 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "Test-Predator-Auto-Owner"]  
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
  assertThrows("nonexistant(obj)");
} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/614c807^!)  
[src/async-hooks-wrapper.cc](https://cs.chromium.org/chromium/src/v8/src/async-hooks-wrapper.cc?cl=614c807)  
[test/mjsunit/regress/regress-crbug-860788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-860788.js?cl=614c807)  
  

### **crbug:859809**  
   
**[Issue: DCHECK failure in !object->IsFiller() in mark-compact.cc](https://crbug.com/859809)**  
**[Commit: [array] Add regression test that causes left trimming while sorting](https://chromium.googlesource.com/v8/v8/+/26ac072)**  
  
Closed: Jul 3 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:856095**  
   
**[Issue: No Permission](https://crbug.com/856095)**  
**[Commit: Fix overzealous assert in CallOrConstructVarArgs](https://chromium.googlesource.com/v8/v8/+/34225a6)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=34225a6)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=34225a6)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=34225a6)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=34225a6)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=34225a6)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=34225a6)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=34225a6)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=34225a6)  
[src/mips/macro-assembler-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.h?cl=34225a6)  
[src/mips64/macro-assembler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/macro-assembler-mips64.cc?cl=34225a6)  
[src/mips64/macro-assembler-mips64.h](https://cs.chromium.org/chromium/src/v8/src/mips64/macro-assembler-mips64.h?cl=34225a6)  
[src/x64/macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.cc?cl=34225a6)  
[src/x64/macro-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.h?cl=34225a6)  
[test/mjsunit/regress/regress-crbug-856095.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-856095.js?cl=34225a6)  
  

### **crbug:854299**  
   
**[Issue: No Permission](https://crbug.com/854299)**  
**[Commit: [array] Change Array.p.sort bailout behavior from fast- to slow-path](https://chromium.googlesource.com/v8/v8/+/3bcf2b8)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:852592**  
   
**[Issue: No Permission](https://crbug.com/852592)**  
**[Commit: [array] Fix OOB load/stores when underlying FixedArray changed](https://chromium.googlesource.com/v8/v8/+/ce3c006)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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

    array.length = 1; 
    array.length = 0; 
    array.length = kArraySize; 
  }
}

array.sort(compareFn);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ce3c006^!)  
[src/builtins/array-sort.tq](https://cs.chromium.org/chromium/src/v8/src/builtins/array-sort.tq?cl=ce3c006)  
[test/mjsunit/regress/regress-crbug-852592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-852592.js?cl=ce3c006)  
  

### **crbug:851393**  
   
**[Issue: Ill in v8::internal::Runtime_SetDataProperties](https://crbug.com/851393)**  
**[Commit: [builtins] Relax type check in a slow path of Object.assign.](https://chromium.googlesource.com/v8/v8/+/412ec75)**  
  
Closed: Jun 18 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-MemorySanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "M-69", "M-68", "Test-Predator-Auto-Owner", "merge-merged-6.8"]  
Regress: [mjsunit/regress/regress-crbug-851393.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-851393.js)  
```javascript
var proxy = new Proxy({}, {});

Object.assign(proxy, { b: "boom", 060: "ah", o: "ouch" });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/412ec75^!)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=412ec75)  
[test/mjsunit/regress/regress-crbug-851393.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-851393.js?cl=412ec75)  
  

### **crbug:850005**  
   
**[Issue: CHECK failure: Type cast failed in CAST(var_elements.value()) at ../../src/builtins/builtins-ca](https://crbug.com/850005)**  
**[Commit: [CSA] Fix assertion in CallOrConstructDoubleVarargs with empty FixedArray](https://chromium.googlesource.com/v8/v8/+/cb29d62)**  
  
Closed: Jun 7 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:849024**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/849024)**  
**[Commit: [builtins] Add reference error for global object property access](https://chromium.googlesource.com/v8/v8/+/d8f0237)**  
  
Closed: Jul 5 2018  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "M-67", "M-66", "M-68", "Test-Predator-Auto-CC"]  
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
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=d8f0237)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=d8f0237)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=d8f0237)  
[test/mjsunit/regress/regress-crbug-849024.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-849024.js?cl=d8f0237)  
  

### **crbug:848165**  
   
**[Issue: enumeration_index out-of-bound](https://crbug.com/848165)**  
**[Commit: Properly set enumeration order for accessor properties in class literals.](https://chromium.googlesource.com/v8/v8/+/e602c90)**  
  
Closed: Jun 18 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["allpublic", "ClusterFuzz-Verified", "Via-Wizard-Security", "M-67", "M-68", "Target-67"]  
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
  

### **crbug:843022**  
   
**[Issue: Security: OOB access in RegExpBuiltinsAssembler::LoadRegExpResultFirstMatch](https://crbug.com/843022)**  
**[Commit: [regexp] Do not assume fast regexp results are non-empty](https://chromium.googlesource.com/v8/v8/+/5999f8f)**  
  
Closed: May 2018  
Type: Bug-Security  
Components: Blink>JavaScript>Regexp  
Labels: ["Hotlist-Merge-Review", "reward-2000", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "reward-inprocess", "FoundIn-66", "merge-merged-6.7", "CVE_description-missing", "Release-0-M67", "CVE-2018-6143", "Hotlist-Torque"]  
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
  

### **crbug:841592**  
   
**[Issue: Crash in IntToSmi<31>](https://crbug.com/841592)**  
**[Commit: [elements] Avoid NOP operation when shrinking HashTables](https://chromium.googlesource.com/v8/v8/+/0b4b14b)**  
  
Closed: May 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Severity-High", "allpublic", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:840220**  
   
**[Issue: CHECK failure: Type cast failed in CAST(TypedArraySpeciesConstructor(context, exemplar)) at ../](https://crbug.com/840220)**  
**[Commit: [CSA] Remove overzealous type check](https://chromium.googlesource.com/v8/v8/+/7235c851)**  
  
Closed: May 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:837939**  
   
**[Issue: Security: [v8] Information Leak in Map constructor](https://crbug.com/837939)**  
**[Commit: Do not throw if the array is empty in Map constructor](https://chromium.googlesource.com/v8/v8/+/c77c869)**  
  
Closed: May 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-Medium", "reward-4500", "allpublic", "reward-inprocess", "M-67", "FoundIn-66", "merge-merged-6.7", "CVE_description-missing", "Release-0-M67", "CVE-2018-6142", "Hotlist-Torque"]  
Regress: [mjsunit/es6/regress/regress-crbug-837939.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-837939.js)  
```javascript
const iterable = [123.123];
assertTrue(%HasDoubleElements(iterable))

iterable.length = 0;
assertTrue(%HasDoubleElements(iterable))


let map = new Map(iterable);
assertEquals(0, map.size);
new WeakMap(iterable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c77c869^!)  
[src/builtins/builtins-collections-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-collections-gen.cc?cl=c77c869)  
[test/mjsunit/es6/regress/regress-crbug-837939.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-crbug-837939.js?cl=c77c869)  
  

### **crbug:831984**  
   
**[Issue: Ill in v8::internal::FullEvacuationVerifier::VerifyPointers](https://crbug.com/831984)**  
**[Commit: [keys] Don't keep chain of OrderedHashSets in KeyAccumulator](https://chromium.googlesource.com/v8/v8/+/7bb79b9)**  
  
Closed: Apr 2018  
Type: Bug-Security  
Components: Blink>JavaScript>GC  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Rejected", "M-65", "M-66", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components", "merge-merged-6.6", "merge-merged-6.7", "Release-1-M66", "Hotlist-Torque"]  
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
  

### **crbug:831943**  
   
**[Issue: Security: Crash with JavaScript RegExp subclassing](https://crbug.com/831943)**  
**[Commit: [builtins] Fix missing ToString in RegExp.p.match](https://chromium.googlesource.com/v8/v8/+/7bdbe77)**  
  
Closed: Apr 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "reward-1500", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "reward-inprocess", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "merge-merged-6.7", "CVE_description-missing", "Release-0-M67", "CVE-2018-6136"]  
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
  

### **crbug:830565**  
   
**[Issue: Promise never resolves/rejects when thenable throws before callback](https://crbug.com/830565)**  
**[Commit: [builtins] Properly reject immediately throwing thenables.](https://chromium.googlesource.com/v8/v8/+/7f8e83b)**  
  
Closed: May 2018  
Type: Bug-Regression  
Components: Blink>JavaScript>Runtime  
Labels: ["Hotlist-Merge-Review", "Hotlist-Merge-Approved", "NodeJS-Backport-Rejected", "Via-Wizard-Javascript", "merge-merged-6.6", "merge-merged-6.7"]  
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
  

### **crbug:827013**  
   
**[Issue: CHECK failure: Type cast failed in CAST(LoadFixedArrayElement( descriptors, DescriptorArray::To](https://crbug.com/827013)**  
**[Commit: [builtins] Fix fast path of Function.prototype.bind.](https://chromium.googlesource.com/v8/v8/+/ef01379)**  
  
Closed: Apr 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-67", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-827013.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-827013.js)  
```javascript
(function Test() {
  var f = () => 42;
  delete f.length;
  delete f.name;

  var g = Object.create(f);
  for (var i = 0; i < 5; i++) {
    g.dummy;
  }
  assertTrue(%HasFastProperties(f));

  var h = f.bind(this);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef01379^!)  
[src/builtins/builtins-function-gen.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function-gen.cc?cl=ef01379)  
[test/mjsunit/regress/regress-crbug-827013.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-827013.js?cl=ef01379)  
  

### **crbug:825045**  
   
**[Issue: DCHECK failure in descriptor_number < number_of_descriptors() in objects-inl.h](https://crbug.com/825045)**  
**[Commit: [turbofan] Properly test number of descriptors.](https://chromium.googlesource.com/v8/v8/+/aa30205)**  
  
Closed: Apr 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner", "merge-merged-6.6", "Release-0-M66"]  
Regress: [mjsunit/regress/regress-crbug-825045.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-825045.js)  
```javascript
const obj = new class A extends (async function (){}.constructor) {};
delete obj.name;
Number.prototype.__proto__ = obj;
function foo() { return obj.bind(); }
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa30205^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=aa30205)  
[test/mjsunit/regress/regress-crbug-825045.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-825045.js?cl=aa30205)  
  

### **crbug:823130**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path](https://crbug.com/823130)**  
**[Commit: Fix "x is not iterable" error message consistency](https://chromium.googlesource.com/v8/v8/+/45a2d9c)**  
  
Closed: May 2018  
Type: Bug  
Components: Blink>JavaScript>GC  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:823069**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,slow_path_opt](https://crbug.com/823069)**  
**[Commit: [builtins] Throw on pop()/shift() when JSArray's length is not writable.](https://chromium.googlesource.com/v8/v8/+/75e04cd)**  
  
Closed: Apr 2018  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:822284**  
   
**[Issue: ThinStrings are incompatible with TurboFan SeqString types](https://crbug.com/822284)**  
**[Commit: [turbofan] NumberToString can return non-sequential strings.](https://chromium.googlesource.com/v8/v8/+/c65f0a7)**  
  
Closed: Mar 2018  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Security", "Security_Severity-Medium", "Arch-All", "allpublic", "merge-merged-6.6"]  
Regress: [mjsunit/regress/regress-crbug-822284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-822284.js)  
```javascript
function foo(a) {
  a = "" + Math.abs(a);
  return a.charCodeAt(0);
}


String.fromCharCode(49);


const o = {};
o[(1).toString()] = 1;

assertEquals(49, foo(1));
assertEquals(49, foo(1));
%OptimizeFunctionOnNextCall(foo);
assertEquals(49, foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c65f0a7^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=c65f0a7)  
[test/mjsunit/regress/regress-crbug-822284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-822284.js?cl=c65f0a7)  
  

### **crbug:821159**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:ia32,ignition](https://crbug.com/821159)**  
**[Commit: [es2015] Properly deal with fast-path results from IterableToList.](https://chromium.googlesource.com/v8/v8/+/631629a)**  
  
Closed: Mar 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:820820**  
   
**[Issue: Null-dereference READ in type](https://crbug.com/820820)**  
**[Commit: [turbofan] Properly deal with killed nodes in LoadElimination.](https://chromium.googlesource.com/v8/v8/+/022e1a5)**  
  
Closed: Mar 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Needs-Feedback", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/022e1a5^!)  
[src/compiler/load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.cc?cl=022e1a5)  
[src/compiler/load-elimination.h](https://cs.chromium.org/chromium/src/v8/src/compiler/load-elimination.h?cl=022e1a5)  
[test/mjsunit/regress/regress-crbug-820820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-820820.js?cl=022e1a5)  
  

### **crbug:820596**  
   
**[Issue: DCHECK failure in static_cast<unsigned>(length_) > static_cast<unsigned>(i) in zone.h](https://crbug.com/820596)**  
**[Commit: [esnext] fix OOB read in ASTPrinter::VisistTemplateLiteral](https://chromium.googlesource.com/v8/v8/+/0802e2b)**  
  
Closed: Mar 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/es6/regress/regress-crbug-820596.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-820596.js)  
```javascript
var x;
`Crashes if OOB read with --print-ast ${x}`;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0802e2b^!)  
[src/ast/prettyprinter.cc](https://cs.chromium.org/chromium/src/v8/src/ast/prettyprinter.cc?cl=0802e2b)  
[test/mjsunit/es6/regress/regress-crbug-820596.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-crbug-820596.js?cl=0802e2b)  
  

### **crbug:820312**  
   
**[Issue: Security: V8: PromiseAllResolveElementClosure can cause elements kind confusion](https://crbug.com/820312)**  
**[Commit: [builtins] Properly handle DICTIONARY_ELEMENTS in Promise.all closures.](https://chromium.googlesource.com/v8/v8/+/fd29e1d)**  
  
Closed: Mar 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Security_Impact-Stable", "Security_Severity-High", "allpublic", "ClusterFuzz-Verified"]  
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
  

### **crbug:819298**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/819298)**  
**[Commit: [turbofan] Fix invalid SpeculativeToNumber optimization.](https://chromium.googlesource.com/v8/v8/+/e583fc8)**  
  
Closed: Mar 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-819298.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-819298.js)  
```javascript
var a = new Int32Array(2);

function foo(base) {
  a[base - 91] = 1;
}

foo("");
foo("");
%OptimizeFunctionOnNextCall(foo);
foo(NaN);
assertEquals(0, a[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e583fc8^!)  
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=e583fc8)  
[test/mjsunit/regress/regress-crbug-819298.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-819298.js?cl=e583fc8)  
  

### **crbug:819086**  
   
**[Issue: CHECK failure: Node::New() Error: #392:DeoptimizeIf[1] is nullptr in node.cc](https://crbug.com/819086)**  
**[Commit: [turbofan] Only store after all checks are done.](https://chromium.googlesource.com/v8/v8/+/6196dd0)**  
  
Closed: Mar 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-819086.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-819086.js)  
```javascript
function foo() {
  return [...[, -Infinity]];
}

foo()[0];
foo()[0];
%OptimizeFunctionOnNextCall(foo);
foo()[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6196dd0^!)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=6196dd0)  
[test/mjsunit/regress/regress-crbug-819086.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-819086.js?cl=6196dd0)  
  

### **crbug:816961**  
   
**[Issue: No Permission](https://crbug.com/816961)**  
**[Commit: Fix buffer-detached check in TypedArray.of/from](https://chromium.googlesource.com/v8/v8/+/c94df3c)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:813630**  
   
**[Issue: DCHECK failure in !has_rest_ in scopes.cc](https://crbug.com/813630)**  
**[Commit: [parser] Fix aborting preparsing of a function with a rest param.](https://chromium.googlesource.com/v8/v8/+/4f506db)**  
  
Closed: Mar 2018  
Type: Bug  
Components: Blink>JavaScript>Language  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components"]  
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
  

### **crbug:813450**  
   
**[Issue: Ill in v8::internal::Runtime_AllocateInNewSpace](https://crbug.com/813450)**  
**[Commit: [proxies] Use write barriers for Proxy [[Construct]] arguments](https://chromium.googlesource.com/v8/v8/+/c7d01c4)**  
  
Closed: Feb 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:813427**  
   
**[Issue: CHECK failure: constructor_initial_map->instance_size() <= instance_size in objects.cc](https://crbug.com/813427)**  
**[Commit: [runtime] Fix overzealous check for derived constructor instance size](https://chromium.googlesource.com/v8/v8/+/da83b61)**  
  
Closed: Feb 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner", "merge-merged-6.5", "merge-merged-65"]  
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
  

### **crbug:808192**  
   
**[Issue: Security: V8 Integer overflow in object allocation size](https://crbug.com/808192)**  
**[Commit: [runtime] Harden JSFunction::CalculateInstanceSizeHelper(...)](https://chromium.googlesource.com/v8/v8/+/7b27040)**  
  
Closed: Feb 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65", "Merge-Rejected-64", "merge-merged-6.5", "Release-0-M65", "CVE-2018-6065", "CVE_description-submitted"]  
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
[test/mjsunit/regress/regress-crbug-808192.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-808192.js?cl=7b27040)  
  

### **crbug:807096**  
   
**[Issue: Security: Arrow function scope fixing bug](https://crbug.com/807096)**  
**[Commit: [parser] More carefully handle destructuring in arrow params](https://chromium.googlesource.com/v8/v8/+/f1a5518)**  
  
Closed: Feb 2018  
Type: Bug-Security  
Components: Blink>JavaScript>Parser  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "ClusterFuzz-Verified", "M-65", "Merge-Rejected-64", "merge-merged-65", "Release-0-M65"]  
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
  

### **crbug:806388**  
   
**[Issue: Security: A bug in JSFunction::GetDerivedMap](https://crbug.com/806388)**  
**[Commit: [runtime] Fix derived class instantiation](https://chromium.googlesource.com/v8/v8/+/8361fa5)**  
  
Closed: Feb 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "NodeJS-Backport-Done", "M-65", "merge-merged-6.4", "merge-merged-6.5", "CVE-2018-6056", "Release-2-M64", "CVE_description-missing"]  
Regress: [mjsunit/regress/regress-crbug-806388.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-806388.js)  
```javascript
class Derived extends Array {
    constructor(a) {
      
      const a = 1;
    }
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
  

### **crbug:806200**  
   
**[Issue: DCHECK failure in !spread_pos.IsValid() in parser-base.h](https://crbug.com/806200)**  
**[Commit: [parser] Throw syntax error for %Foo(...spread)](https://chromium.googlesource.com/v8/v8/+/3249b16)**  
  
Closed: Jan 2018  
Type: Bug  
Components: Blink>JavaScript>Parser  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-806200.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-806200.js)  
```javascript
assertThrows("%Foo(...spread)", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3249b16^!)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=3249b16)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=3249b16)  
[test/mjsunit/regress/regress-crbug-806200.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-806200.js?cl=3249b16)  
  

### **crbug:805765**  
   
**[Issue: CHECK failure: (location_) != nullptr in handles.h](https://crbug.com/805765)**  
**[Commit: [ignition] Fix wide suspends to also return](https://chromium.googlesource.com/v8/v8/+/830e39a)**  
  
Closed: Jan 2018  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-Components", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=830e39a)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=830e39a)  
[test/mjsunit/regress/regress-crbug-805765.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-805765.js?cl=830e39a)  
  

### **crbug:802333**  
   
**[Issue: Security: V8: A bug in the ObjectDescriptor class](https://crbug.com/802333)**  
**[Commit: [runtime] Fix Class Literals](https://chromium.googlesource.com/v8/v8/+/e416e3c)**  
  
Closed: Jan 2018  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Security_Severity-Medium", "ReleaseBlock-Stable", "allpublic", "Merge-Rejected-65"]  
Regress: [mjsunit/regress/regress-crbug-802333.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-802333.js)  
```javascript
function deferred_func() {
    class C {
        method1() {

        }
    }
}

let bound = (a => a).bind(this, 0);

function opt() {
    deferred_func.prototype;  

    return bound();
}

assertEquals(0, opt());
%OptimizeFunctionOnNextCall(opt);

assertEquals(0, opt());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e416e3c^!)  
[src/objects/literal-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.cc?cl=e416e3c)  
[test/mjsunit/regress/regress-crbug-802333.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-802333.js?cl=e416e3c)  
  

### **crbug:801627**  
   
**[Issue: Security: V8: JIT: Type confusion in NodeProperties::InferReceiverMaps](https://crbug.com/801627)**  
**[Commit: [turbofan] Fix type confusion in NodeProperties::InferReceiverMaps.](https://chromium.googlesource.com/v8/v8/+/e272a2f)**  
  
Closed: Jan 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Security_Severity-Medium", "Arch-All", "allpublic", "merge-merged-6.4"]  
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




Reflect.construct(Derived, [], Object.bind());
%OptimizeFunctionOnNextCall(Derived);
new Derived();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e272a2f^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=e272a2f)  
[test/mjsunit/regress/regress-crbug-801627.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-801627.js?cl=e272a2f)  
  

### **crbug:800810**  
   
**[Issue: DCHECK failure in receiver->map() == *original_map in elements.cc](https://crbug.com/800810)**  
**[Commit: [elements] Fix overzealous DCHECK in Array.prototype.includes](https://chromium.googlesource.com/v8/v8/+/b785d2a)**  
  
Closed: Jan 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:800077**  
   
**[Issue: CHECK failure: Type cast failed in CAST(key) at ../../src/code-stub-assembler.cc:7137 in code-a](https://crbug.com/800077)**  
**[Commit: [csa] Fix type casing in GetProperty](https://chromium.googlesource.com/v8/v8/+/8643720)**  
  
Closed: Jan 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "ClusterFuzz-Top-Crash", "Test-Predator-Auto-CC"]  
Regress: [mjsunit/regress/regress-crbug-800077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-800077.js)  
```javascript
var sample = new Float64Array(1);
Reflect.has(sample, undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8643720^!)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=8643720)  
[test/mjsunit/regress/regress-crbug-800077.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-800077.js?cl=8643720)  
  

### **crbug:800032**  
   
**[Issue: Security: V8: Bugs in Genesis::InitializeGlobal](https://crbug.com/800032)**  
**[Commit: [Runtime] Set expected_nof_properties when creating Constructors](https://chromium.googlesource.com/v8/v8/+/42e8ca9)**  
  
Closed: Feb 2018  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65", "merge-merged-6.4", "Release-1-M64"]  
Regress: [mjsunit/regress/regress-crbug-800032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-800032.js)  
```javascript
class Derived extends RegExp {
  constructor(a) {
    
    const a = 1;
  }
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
  

### **crbug:798644**  
   
**[Issue: Security: V8: Type confusion in ElementsAccessorBase::CollectValuesOrEntriesImpl](https://crbug.com/798644)**  
**[Commit: [elements] Fix Object.entries/values with changing elements](https://chromium.googlesource.com/v8/v8/+/be9c5fd)**  
  
Closed: Jan 2018  
Type: Bug-Security  
Components: Blink>JavaScript>Runtime  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "ClusterFuzz-Verified", "M-65", "Merge-Rejected-64", "Release-0-M65", "CVE-2018-6064", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-798644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-798644.js)  
```javascript
let arr = [];

arr[1000] = 0x1234;

arr.__defineGetter__(256, function () {
    
    delete arr[256];
    
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
  

### **crbug:798026**  
   
**[Issue: No Permission](https://crbug.com/798026)**  
**[Commit: [Builtins] Eliminate the fast path in constructor entries](https://chromium.googlesource.com/v8/v8/+/a10689d)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:795922**  
   
**[Issue: DCHECK failure in !has_null_prototype() in ast.cc](https://crbug.com/795922)**  
**[Commit: [ignition] Move object/array literal init to bytecode gen](https://chromium.googlesource.com/v8/v8/+/9128e8b)**  
  
Closed: Dec 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-795922.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-795922.js)  
```javascript
assertThrows(
  
  "({ __proto__: null, __proto__: 1 })",
  SyntaxError
);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9128e8b^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=9128e8b)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=9128e8b)  
[test/mjsunit/regress/regress-crbug-795922.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-795922.js?cl=9128e8b)  
  

### **crbug:791256**  
   
**[Issue: DCHECK failure in kNoSourcePosition != start_position() in scopes.cc](https://crbug.com/791256)**  
**[Commit: [parser] Fix NaryOperation positions.](https://chromium.googlesource.com/v8/v8/+/10d9c31)**  
  
Closed: Dec 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Language  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-CC", "Test-Predator-Auto-Components", "merge-merged-6.4", "merge-merged-64"]  
Regress: [mjsunit/regress/regress-crbug-791256.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-791256.js)  
```javascript
(function* (name = (eval(foo), foo, prototype)) { });


(function (name = (foo, bar, baz) ) { });


(function (param = (0, 1, 2)) { assertEquals(2, param); })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10d9c31^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=10d9c31)  
[test/mjsunit/regress/regress-crbug-791256.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-791256.js?cl=10d9c31)  
  

### **crbug:791245**  
   
**[Issue: Security: V8: JIT: Simplified-lowererer IrOpcode::kStoreField, IrOpcode::kStoreElement optimization bug](https://crbug.com/791245)**  
**[Commit: [turbofan] Properly type the OrderedHashTableHealIndex builtin result.](https://chromium.googlesource.com/v8/v8/+/3ef6e45)**  
  
Closed: Dec 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "M-65", "Merge-Rejected-63", "merge-merged-6.4", "Release-0-M64"]  
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
  

### **crbug:789764**  
   
**[Issue: Crash in v8::internal::Script::FindSharedFunctionInfo](https://crbug.com/789764)**  
**[Commit: [parser] Fix func numbering inside for in.](https://chromium.googlesource.com/v8/v8/+/0394b71)**  
  
Closed: Dec 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Interpreter  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner", "Release-0-M65"]  
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
  

### **crbug:786723**  
   
**[Issue: DCHECK failure in !compilation_info()->dependencies() || !compilation_info()->dependencies()->HasA](https://crbug.com/786723)**  
**[Commit: [turbofan] Fix prototype mutation in Object.create lowering.](https://chromium.googlesource.com/v8/v8/+/4a7eec5)**  
  
Closed: Dec 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner", "merge-merged-6.4", "Release-0-M64"]  
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
  

### **crbug:786020**  
   
**[Issue: CHECK failure: !descriptors->GetKey(i)->IsInterestingSymbol() in objects-debug.cc](https://crbug.com/786020)**  
**[Commit: [objects] Fix flag in {Map::AddMissingTransitions}.](https://chromium.googlesource.com/v8/v8/+/4ad9430)**  
  
Closed: Nov 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
Regress: [mjsunit/regress/regress-crbug-786020.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-786020.js)  
```javascript
%SetAllocationTimeout(1000, 90);
(new constructor)[0x40000000] = null;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ad9430^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4ad9430)  
[test/mjsunit/regress/regress-crbug-786020.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-786020.js?cl=4ad9430)  
  

### **crbug:784835**  
   
**[Issue: page content not display](https://crbug.com/784835)**  
**[Commit: [ic] Properly handle negative indices.](https://chromium.googlesource.com/v8/v8/+/3dddc2b)**  
  
Closed: Nov 2017  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Arch-x86_64", "ReleaseBlock-Stable", "hasbisect-per-revision", "Via-Wizard-Content", "M-64", "Triaged-ET", "Needs-Triage-M64"]  
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
  

### **crbug:783902**  
   
**[Issue: CHECK failure: method->map()->instance_descriptors()->GetKey(kHomeObjectPropertyIndex) == isola](https://crbug.com/783902)**  
**[Commit: Reland^2 "[runtime] Slightly optimize creation of class literals."](https://chromium.googlesource.com/v8/v8/+/cc9e77a)**  
  
Closed: Nov 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
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
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=cc9e77a)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=cc9e77a)  
[src/interpreter/bytecode-generator.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.h?cl=cc9e77a)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=cc9e77a)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=cc9e77a)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=cc9e77a)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=cc9e77a)  
[src/objects/dictionary.h](https://cs.chromium.org/chromium/src/v8/src/objects/dictionary.h?cl=cc9e77a)  
[src/objects/literal-objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects-inl.h?cl=cc9e77a)  
[src/objects/literal-objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.cc?cl=cc9e77a)  
[src/objects/literal-objects.h](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.h?cl=cc9e77a)  
[src/objects/shared-function-info.h](https://cs.chromium.org/chromium/src/v8/src/objects/shared-function-info.h?cl=cc9e77a)  
[src/runtime/runtime-classes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-classes.cc?cl=cc9e77a)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=cc9e77a)  
[src/v8.gyp](https://cs.chromium.org/chromium/src/v8/src/v8.gyp?cl=cc9e77a)  
[test/cctest/interpreter/bytecode_expectations/ClassDeclarations.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ClassDeclarations.golden?cl=cc9e77a)  
[test/cctest/interpreter/bytecode_expectations/Modules.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Modules.golden?cl=cc9e77a)  
[test/cctest/interpreter/bytecode_expectations/NewAndSpread.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/NewAndSpread.golden?cl=cc9e77a)  
[test/mjsunit/es6/class-computed-property-names-super.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/class-computed-property-names-super.js?cl=cc9e77a)  
[test/mjsunit/es6/classes.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/classes.js?cl=cc9e77a)  
[test/mjsunit/regress/regress-crbug-783902.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-783902.js?cl=cc9e77a)  
[tools/v8heapconst.py](https://cs.chromium.org/chromium/src/v8/tools/v8heapconst.py?cl=cc9e77a)  
  

### **crbug:783132**  
   
**[Issue: CHECK failure: is_transitionable_fast_elements_kind implies !Map::IsInplaceGeneralizableField(d](https://crbug.com/783132)**  
**[Commit: [runtime] Ensure elements transitions don't interfere with field type tracking.](https://chromium.googlesource.com/v8/v8/+/00a781d)**  
  
Closed: Nov 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.3"]  
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
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=00a781d)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=00a781d)  
[src/objects/map-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects/map-inl.h?cl=00a781d)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=00a781d)  
[test/cctest/test-field-type-tracking.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-field-type-tracking.cc?cl=00a781d)  
[test/mjsunit/regress/regress-crbug-783132.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-783132.js?cl=00a781d)  
[test/mjsunit/unbox-double-arrays.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/unbox-double-arrays.js?cl=00a781d)  
  

### **crbug:781583**  
   
**[Issue: Stack-overflow in v8::internal::KeyAccumulator::CollectOwnElementIndices](https://crbug.com/781583)**  
**[Commit: [builtins] Add stack check during generator resumption.](https://chromium.googlesource.com/v8/v8/+/2bc09c9)**  
  
Closed: Nov 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Stability-UndefinedBehaviorSanitizer", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "Test-Predator-Auto-Owner"]  
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
[src/builtins/ppc/builtins-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ppc/builtins-ppc.cc?cl=2bc09c9)  
[src/builtins/s390/builtins-s390.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/s390/builtins-s390.cc?cl=2bc09c9)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=2bc09c9)  
[test/mjsunit/regress/regress-crbug-781583.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-781583.js?cl=2bc09c9)  
  

### **crbug:781506**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/781506)**  
**[Commit: [turbofan] Generate the correct bounds when the array protector isn't valid.](https://chromium.googlesource.com/v8/v8/+/fd150c7)**  
  
Closed: Nov 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Owner", "Hotlist-Torque"]  
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
  

### **crbug:781116**  
   
**[Issue: DCHECK failure in false == cell_reports_intact in isolate.cc](https://crbug.com/781116)**  
**[Commit: [turbofan] Properly handle Array.prototype and Object.prototype in the runtime.](https://chromium.googlesource.com/v8/v8/+/82b3ac9)**  
  
Closed: Nov 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:779457**  
   
**[Issue: DCHECK failure in outer_scope_ == scope->outer_scope() in bytecode-generator.cc](https://crbug.com/779457)**  
**[Commit: [parser] RewritableExpressions should keep track of their Scope directly](https://chromium.googlesource.com/v8/v8/+/082009f)**  
  
Closed: Nov 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Parser  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Stability-Libfuzzer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Release-0-M64"]  
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
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=082009f)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=082009f)  
[test/mjsunit/regress/regress-crbug-779457.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-779457.js?cl=082009f)  
  

### **crbug:779367**  
   
**[Issue: Null-dereference READ in v8::internal::CallOptimization::IsCrossContextLazyAccessorPair](https://crbug.com/779367)**  
**[Commit: Check is_simple_api_call before IsCrossContextLazyAccessorPair, accessor could be null](https://chromium.googlesource.com/v8/v8/+/b976b30)**  
  
Closed: Nov 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:779344**  
   
**[Issue: Stack-overflow in v8::internal::CheckObjectType](https://crbug.com/779344)**  
**[Commit: Perform stack check on Proxy call trap.](https://chromium.googlesource.com/v8/v8/+/1e77461)**  
  
Closed: Nov 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
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
[src/interpreter/interpreter-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter-generator.cc?cl=1e77461)  
[test/mjsunit/regress/regress-crbug-779344.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-779344.js?cl=1e77461)  
  

### **crbug:778952**  
   
**[Issue: DCHECK failure in raw_properties_or_hash()->IsDictionary() == map()->is_dictionary_map() in object](https://crbug.com/778952)**  
**[Commit: Fix DCHECK in HasFastProperties](https://chromium.googlesource.com/v8/v8/+/a5b0d64)**  
  
Closed: Oct 2017  
Type: Bug-Regression  
Components: Blink>JavaScript>Runtime  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-778952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-778952.js)  
```javascript
assertThrows(function() {
  const p = new Proxy({}, {});
  (new Set).add(p);  
  null[p] = 0;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5b0d64^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=a5b0d64)  
[test/mjsunit/regress/regress-crbug-778952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-778952.js?cl=a5b0d64)  
  

### **crbug:776511**  
   
**[Issue: DCHECK failure in BackingStore::get(backing_store, i, isolate)->IsSmi() || (IsHoleyElementsKind(Ki](https://crbug.com/776511)**  
**[Commit: [Turbofan] Reland Array.prototype.filter inlining.](https://chromium.googlesource.com/v8/v8/+/b3d8499)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
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
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=b3d8499)  
[test/mjsunit/filter-element-kinds.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/filter-element-kinds.js?cl=b3d8499)  
[test/mjsunit/optimized-filter.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/optimized-filter.js?cl=b3d8499)  
[test/mjsunit/regress/regress-crbug-747062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-747062.js?cl=b3d8499)  
[test/mjsunit/regress/regress-crbug-766635.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-766635.js?cl=b3d8499)  
[test/mjsunit/regress/regress-crbug-776511.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-776511.js?cl=b3d8499)  
  

### **crbug:774994**  
   
**[Issue: CHECK failure: args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/774994)**  
**[Commit: [parser] Skipping inner funcs: accurately record NeedsHomeObject](https://chromium.googlesource.com/v8/v8/+/94a71d7)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Security_Severity-Low", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-63", "ClusterFuzz-Top-Crash", "merge-merged-6.3"]  
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
[test/mjsunit/regress/regress-crbug-774994.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-774994.js?cl=94a71d7)  
  

### **crbug:774860**  
   
**[Issue: CHECK failure: map->IsMap() in spaces.cc](https://crbug.com/774860)**  
**[Commit: Fix slack tracking for function subclasses.](https://chromium.googlesource.com/v8/v8/+/b0fc245)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-774860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-774860.js)  
```javascript
(function () {
  class F extends Function {}
  let f = new F("'use strict';");
  
  for (let i = 0; i < 20; i++) {
    new F();
  }
  gc();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0fc245^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b0fc245)  
[test/mjsunit/regress/regress-crbug-774860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-774860.js?cl=b0fc245)  
  

### **crbug:774459**  
   
**[Issue: Inbox won't load in 32 bit builds](https://crbug.com/774459)**  
**[Commit: [turbofan] Re-enable FindOrderedHashMapEntryForInt32Key optimization.](https://chromium.googlesource.com/v8/v8/+/49e87d2)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["ReleaseBlock-Dev", "M-63", "merge-merged-6.3"]  
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
  

### **crbug:772897**  
   
**[Issue: DCHECK failure in !has_pending_exception() in isolate.cc](https://crbug.com/772897)**  
**[Commit: [proxy] Properly handle exceptions from Object::ToName().](https://chromium.googlesource.com/v8/v8/+/ef45d78)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Hotlist-Recharge-BouncingOwner", "M-65", "merge-merged-6.3", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:772720**  
   
**[Issue: CHECK failure: NodeProperties::GetType(val)->Is(NodeProperties::GetType(node)) in verifier.cc](https://crbug.com/772720)**  
**[Commit: [turbofan] Fix type of inline cons-string allocation.](https://chromium.googlesource.com/v8/v8/+/93f855c)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "M-65", "merge-merged-6.3", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:772689**  
   
**[Issue: CHECK failure: 0 == field_count_ in deoptimizer.cc](https://crbug.com/772689)**  
**[Commit: [deoptimizer] Properly handle in-object properties on JSArrays.](https://chromium.googlesource.com/v8/v8/+/bed8853)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:772672**  
   
**[Issue: Null-dereference WRITE in raise](https://crbug.com/772672)**  
**[Commit: Fix JSArray::kInitialMaxFastElementArray to make sense for 32-bit platforms.](https://chromium.googlesource.com/v8/v8/+/2bb704e)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:772610**  
   
**[Issue: Null-dereference READ in Relaxed_Load](https://crbug.com/772610)**  
**[Commit: [deoptimizer] Fix JSFunction materialization instance size.](https://chromium.googlesource.com/v8/v8/+/c34a295)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:772056**  
   
**[Issue: DCHECK failure in new_len >= old_len in heap.cc](https://crbug.com/772056)**  
**[Commit: [wasm] Fix undefined behavior in WebAssembly.Table.grow.](https://chromium.googlesource.com/v8/v8/+/158dbb8)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript>WebAssembly  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Wrong-CLs", "Test-Predator-Wrong-Components", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
Regress: [mjsunit/regress/regress-crbug-772056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-772056.js)  
```javascript
load("test/mjsunit/wasm/wasm-constants.js");
load("test/mjsunit/wasm/wasm-module-builder.js");

var builder = new WasmModuleBuilder();
builder.addImportedTable("x", "table", 1, 10000000);
let module = new WebAssembly.Module(builder.toBuffer());
let table = new WebAssembly.Table({element: "anyfunc",
  initial: 1, maximum:1000000});
let instance = new WebAssembly.Instance(module, {x: {table:table}});

assertThrows(() => table.grow(Infinity), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/158dbb8^!)  
[src/wasm/wasm-js.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-js.cc?cl=158dbb8)  
[test/mjsunit/regress/regress-crbug-772056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-772056.js?cl=158dbb8)  
  

### **crbug:771971**  
   
**[Issue: DCHECK failure in index < GetJSCallArity() in js-builtin-reducer.cc](https://crbug.com/771971)**  
**[Commit: [turbofan] Properly check call arity for Object.is(o,o).](https://chromium.googlesource.com/v8/v8/+/c77dfda)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
  

### **crbug:771428**  
   
**[Issue: Infinite loop in asm.js parser](https://crbug.com/771428)**  
**[Commit: [asm.js] Fix infinite loop in parser on parse error.](https://chromium.googlesource.com/v8/v8/+/4f8a70a)**  
  
Closed: Oct 2017  
Type: Bug-Regression  
Components: Blink>JavaScript>WebAssembly  
Labels: ["Hotlist-Merge-Review", "NodeJS-Backport-Done", "M-61", "Via-Wizard-Javascript", "merge-merged-6.1", "merge-merged-6.2"]  
Regress: [mjsunit/regress/regress-crbug-771428.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-771428.js)  
```javascript
function Module() {
  "use asm";
  function f(i) {
    i = i | 0;
    switch (i | 0) {
      case 2:
        
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
  

### **crbug:770581**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/770581)**  
**[Commit: [turbofan] Unify error message on non-callable callback.](https://chromium.googlesource.com/v8/v8/+/bb46a59)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:770543**  
   
**[Issue: Null-dereference READ in v8::internal::TranslatedFrame::begin](https://crbug.com/770543)**  
**[Commit: [deoptimizer] Fix TranslatedState inline frame indexing.](https://chromium.googlesource.com/v8/v8/+/631489b)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "Test-Predator-Auto-Components"]  
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
  

### **crbug:769852**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/769852)**  
**[Commit: [deoptimizer] Materialize objects with top-most stub frame.](https://chromium.googlesource.com/v8/v8/+/17d86d7)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Components"]  
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
  

### **crbug:768875**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/768875)**  
**[Commit: [ic] Introduce proper slow stub for StoreGlobalIC.](https://chromium.googlesource.com/v8/v8/+/3384a79)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "Test-Predator-Auto-Components", "Test-Predator-Auto-Owner"]  
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
[test/mjsunit/regress/regress-crbug-768875.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-768875.js?cl=3384a79)  
  

### **crbug:768367**  
   
**[Issue: DCHECK failure in kMaxUInt32 != index_ in lookup.h](https://crbug.com/768367)**  
**[Commit: [turbofan] Fix off-by-one in constant-folding of frozen elements.](https://chromium.googlesource.com/v8/v8/+/adfaf74)**  
  
Closed: Sep 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65"]  
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
  

### **crbug:768158**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/768158)**  
**[Commit: [parser] Ensure for-in/of loop variables are marked maybe_assigned](https://chromium.googlesource.com/v8/v8/+/0717ff3)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-62", "v8-foozzie-failure", "M-63", "merge-merged-6.2", "merge-merged-62"]  
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
  

### **crbug:768080**  
   
**[Issue: CHECK failure: args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/768080)**  
**[Commit: [turbofan] Fix new.target check in Reflect.construct.](https://chromium.googlesource.com/v8/v8/+/afd2f58)**  
  
Closed: Oct 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "ClusterFuzz-Top-Crash", "merge-merged-6.3", "Release-0-M63"]  
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
  new C();  
  assertThrows(f, TypeError);
  assertThrows(f, TypeError);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f, TypeError);
})();

(function TestReflectConstructBogusNewTarget2() {
  class C {}
  
  
  
  function g() {
    Reflect.construct(C, arguments, unescape);
  }
  function f() {
    return new g();
  }
  new C();  
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
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=afd2f58)  
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=afd2f58)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=afd2f58)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=afd2f58)  
[test/mjsunit/regress/regress-crbug-768080.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-768080.js?cl=afd2f58)  
  

### **crbug:766635**  
   
**[Issue: No Permission](https://crbug.com/766635)**  
**[Commit: [Turbofan] Array.prototype.filter inlining.](https://chromium.googlesource.com/v8/v8/+/9fd029e)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9fd029e)  
[test/mjsunit/optimized-filter.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/optimized-filter.js?cl=9fd029e)  
[test/mjsunit/regress/regress-crbug-747062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-747062.js?cl=9fd029e)  
[test/mjsunit/regress/regress-crbug-766635.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-766635.js?cl=9fd029e)  
  

### **crbug:764219**  
   
**[Issue: Ill in v8::internal::__RT_impl_Runtime_AbortJS](https://crbug.com/764219)**  
**[Commit: [ic] Do access checks when storing via JSGlobalProxy.](https://chromium.googlesource.com/v8/v8/+/5ea95fe)**  
  
Closed: Oct 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "ReleaseBlock-Stable", "Clusterfuzz", "M-63", "ClusterFuzz-Top-Crash", "merge-merged-6.3", "Test-Predator-Auto-Components"]  
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
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=5ea95fe)  
[test/mjsunit/regress/regress-crbug-764219.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-764219.js?cl=5ea95fe)  
  

### **crbug:763683**  
   
**[Issue: DCHECK failure in !__isolate__->has_pending_exception() in runtime-proxy.cc](https://crbug.com/763683)**  
**[Commit: Improve error handling of proxies get property](https://chromium.googlesource.com/v8/v8/+/8a568bd)**  
  
Closed: Sep 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.2"]  
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
  

### **crbug:762874**  
   
**[Issue: Security: off by one in TurboFan range optimization for String.indexOf](https://crbug.com/762874)**  
**[Commit: [turbofan] Fix type of String#indexOf and String#lastIndexOf.](https://chromium.googlesource.com/v8/v8/+/b8f144e)**  
  
Closed: Sep 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "NodeJS-Backport-Done", "merge-merged-6.1", "merge-merged-6.2", "Release-2-M61"]  
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
  

### **crbug:762472**  
   
**[Issue: DCHECK failure in !isolate->has_pending_exception() in asm-js.cc](https://crbug.com/762472)**  
**[Commit: [asm.js] Gracefully handle stack overflow in start function.](https://chromium.googlesource.com/v8/v8/+/54a3027)**  
  
Closed: Sep 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-65", "merge-merged-6.2", "Release-0-M62"]  
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
  

### **crbug:759327**  
   
**[Issue: <no crash state available>](https://crbug.com/759327)**  
**[Commit: [asm.js] Correctly set minimum memory size to zero.](https://chromium.googlesource.com/v8/v8/+/89f839e)**  
  
Closed: Aug 2017  
Type: Bug  
Components: Blink>JavaScript>WebAssembly  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
[test/mjsunit/regress/regress-crbug-759327.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-759327.js?cl=89f839e)  
  

### **crbug:758773**  
   
**[Issue: DCHECK failure in result_map_->is_dictionary_map() in map-updater.cc](https://crbug.com/758773)**  
**[Commit: Don't look at abandoned prototype maps when looking for root maps](https://chromium.googlesource.com/v8/v8/+/8a7ce92)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-758773.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-758773.js)  
```javascript
(0).__defineGetter__(0, function() { });
Number.prototype[0] = "string";  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a7ce92^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=8a7ce92)  
[test/mjsunit/regress/regress-crbug-757199.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-757199.js?cl=8a7ce92)  
[test/mjsunit/regress/regress-crbug-758773.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-758773.js?cl=8a7ce92)  
  

### **crbug:757199**  
   
**[Issue: DCHECK failure in result->owns_descriptors() in objects.cc](https://crbug.com/757199)**  
**[Commit: [runtime] Deprecate old prototype maps](https://chromium.googlesource.com/v8/v8/+/8974b75)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62", "Release-0-M62"]  
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
  

### **crbug:756332**  
   
**[Issue: DCHECK failure in !node->is_rewritten() in pattern-rewriter.cc](https://crbug.com/756332)**  
**[Commit: [pattern-rewriter] Handle already-rewritten RewritableExpressions as before](https://chromium.googlesource.com/v8/v8/+/cec289e)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Needs-Feedback", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
  

### **crbug:755044**  
   
**[Issue: DCHECK failure in AllowHeapAllocation::IsAllowed() in heap-inl.h](https://crbug.com/755044)**  
**[Commit: Handlify FrameFunctionIterator to allow for GCs.](https://chromium.googlesource.com/v8/v8/+/6dd1251)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
  

### **crbug:754177**  
   
**[Issue: CHECK failure: args[0]->IsJSFunction() in runtime-test.cc](https://crbug.com/754177)**  
**[Commit: [tests] Make %NeverOptimizeFunction ClusterFuzz safe](https://chromium.googlesource.com/v8/v8/+/89e5792)**  
  
Closed: Aug 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
  

### **crbug:754175**  
   
**[Issue: CHECK failure: memory->byte_length()->ToUint32(&mem_size) in module-compiler.cc](https://crbug.com/754175)**  
**[Commit: [asm.js] Fail gracefully on overly large buffers.](https://chromium.googlesource.com/v8/v8/+/8d2a8e0)**  
  
Closed: Aug 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "External-Fuzzer-Contribution", "reward-0", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
Regress: [mjsunit/regress/regress-crbug-754175.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-754175.js)  
```javascript
function Module(stdlib, foreign, buffer) {
  "use asm";
  var heap = new stdlib.Int8Array(buffer);
  function foo() { return heap[23] | 0 }
  return { foo:foo };
}
function instantiate() {
  
  var buffer = new ArrayBuffer(0x100000000);
  
  var module = Module(this, {}, buffer);
}
assertThrows(instantiate, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d2a8e0^!)  
[src/asmjs/asm-js.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-js.cc?cl=8d2a8e0)  
[test/mjsunit/regress/regress-crbug-754175.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-754175.js?cl=8d2a8e0)  
  

### **crbug:752846**  
   
**[Issue: CHECK failure: args[2]->IsJSReceiver() in runtime-proxy.cc](https://crbug.com/752846)**  
**[Commit: Reland^2 "[builtins] Port getting property from Proxy to CSA"](https://chromium.googlesource.com/v8/v8/+/e86c066)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=e86c066)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=e86c066)  
[src/ic/handler-configuration-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration-inl.h?cl=e86c066)  
[src/ic/handler-configuration.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.h?cl=e86c066)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=e86c066)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=e86c066)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e86c066)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=e86c066)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=e86c066)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=e86c066)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=e86c066)  
[src/v8.gyp](https://cs.chromium.org/chromium/src/v8/src/v8.gyp?cl=e86c066)  
[test/mjsunit/es6/proxies-get.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/proxies-get.js?cl=e86c066)  
[test/mjsunit/regress/regress-crbug-752712.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752712.js?cl=e86c066)  
[test/mjsunit/regress/regress-crbug-752846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752846.js?cl=e86c066)  
  

### **crbug:752826**  
   
**[Issue: Fatal error: Tried to combine incompatible truncations](https://crbug.com/752826)**  
**[Commit: [turbofan] Fix introduction of contradicting {TypeGuard}.](https://chromium.googlesource.com/v8/v8/+/d929cc7)**  
  
Closed: Aug 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
  

### **crbug:752712**  
   
**[Issue: Crash in v8::internal::Invoke](https://crbug.com/752712)**  
**[Commit: Reland^2 "[builtins] Port getting property from Proxy to CSA"](https://chromium.googlesource.com/v8/v8/+/e86c066)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=e86c066)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=e86c066)  
[src/ic/handler-configuration-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration-inl.h?cl=e86c066)  
[src/ic/handler-configuration.h](https://cs.chromium.org/chromium/src/v8/src/ic/handler-configuration.h?cl=e86c066)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=e86c066)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=e86c066)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e86c066)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=e86c066)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=e86c066)  
[src/runtime/runtime-proxy.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-proxy.cc?cl=e86c066)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=e86c066)  
[src/v8.gyp](https://cs.chromium.org/chromium/src/v8/src/v8.gyp?cl=e86c066)  
[test/mjsunit/es6/proxies-get.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/proxies-get.js?cl=e86c066)  
[test/mjsunit/regress/regress-crbug-752712.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752712.js?cl=e86c066)  
[test/mjsunit/regress/regress-crbug-752846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752846.js?cl=e86c066)  
  

### **crbug:752481**  
   
**[Issue: CHECK failure: args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/752481)**  
**[Commit: [turbofan] Properly check new.target parameter in inlined Reflect.construct.](https://chromium.googlesource.com/v8/v8/+/cb9402a)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-62"]  
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
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=cb9402a)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=cb9402a)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=cb9402a)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=cb9402a)  
[test/mjsunit/regress/regress-crbug-752481.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-752481.js?cl=cb9402a)  
  

### **crbug:751715**  
   
**[Issue: DCHECK failure in !is_constructor in frames.cc](https://crbug.com/751715)**  
**[Commit: [deoptimizer] Fix bogus DCHECK in OptimizedFrame::Summarize.](https://chromium.googlesource.com/v8/v8/+/c8ee3fb)**  
  
Closed: Aug 2017  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
  

### **crbug:751109**  
   
**[Issue: CHECK failure: !descriptors->GetKey(i)->IsInterestingSymbol() in objects-debug.cc](https://crbug.com/751109)**  
**[Commit: [runtime] Properly forward the "interesting symbol" bit.](https://chromium.googlesource.com/v8/v8/+/7101248)**  
  
Closed: Aug 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-751109.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-751109.js)  
```javascript
(new constructor)[0] = null;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7101248^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7101248)  
[test/mjsunit/regress/regress-crbug-751109.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-751109.js?cl=7101248)  
  

### **crbug:748539**  
   
**[Issue: CHECK failure: is_transitionable_fast_elements_kind implies !Map::IsInplaceGeneralizableField(d](https://crbug.com/748539)**  
**[Commit: [runtime] Don't create class field types for arrays' fields.](https://chromium.googlesource.com/v8/v8/+/10e4fe3)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "M-62", "merge-merged-6.0", "merge-merged-6.1"]  
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
[test/mjsunit/regress/regress-crbug-748539.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-748539.js?cl=10e4fe3)  
  

### **crbug:747979**  
   
**[Issue: DCHECK failure in !IsInplaceGeneralizableField(details.constness(), details.representation(), desc](https://crbug.com/747979)**  
**[Commit: [runtime] Don't create "class" field types for arrays' fields.](https://chromium.googlesource.com/v8/v8/+/c558369a)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-0", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "merge-merged-6.0", "merge-merged-6.1"]  
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
  

### **crbug:747062**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/747062)**  
**[Commit: [turbofan] Fix missing callability check on Array callbacks](https://chromium.googlesource.com/v8/v8/+/44f88dc)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=44f88dc)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=44f88dc)  
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=44f88dc)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=44f88dc)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=44f88dc)  
[test/mjsunit/regress/regress-crbug-747062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-747062.js?cl=44f88dc)  
  

### **crbug:746835**  
   
**[Issue: Crash in v8::internal::Heap::MergeAllocationSitePretenuringFeedback](https://crbug.com/746835)**  
**[Commit: [literals] Introduce CreateEmptyArrayLiteral Bytecode](https://chromium.googlesource.com/v8/v8/+/0392eb2)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Fracas", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-61", "FoundIn-M-61"]  
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
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=0392eb2)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=0392eb2)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=0392eb2)  
[src/compiler/js-create-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.h?cl=0392eb2)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=0392eb2)  
[src/compiler/js-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.cc?cl=0392eb2)  
[src/compiler/js-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-operator.h?cl=0392eb2)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=0392eb2)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=0392eb2)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=0392eb2)  
[src/interpreter/bytecode-array-builder.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.cc?cl=0392eb2)  
[src/interpreter/bytecode-array-builder.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-array-builder.h?cl=0392eb2)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=0392eb2)  
[src/interpreter/bytecodes.h](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecodes.h?cl=0392eb2)  
[src/interpreter/interpreter-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/interpreter-generator.cc?cl=0392eb2)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=0392eb2)  
[src/objects/literal-objects.h](https://cs.chromium.org/chromium/src/v8/src/objects/literal-objects.h?cl=0392eb2)  
[src/runtime/runtime-literals.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-literals.cc?cl=0392eb2)  
[test/cctest/heap/test-heap.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/heap/test-heap.cc?cl=0392eb2)  
[test/cctest/test-code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-code-stub-assembler.cc?cl=0392eb2)  
[test/mjsunit/array-literal-feedback.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-literal-feedback.js?cl=0392eb2)  
[test/mjsunit/array-literal-transitions.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-literal-transitions.js?cl=0392eb2)  
[test/mjsunit/regress/regress-crbug-746835.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-746835.js?cl=0392eb2)  
[test/unittests/interpreter/bytecode-array-builder-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/interpreter/bytecode-array-builder-unittest.cc?cl=0392eb2)  
  

### **crbug:743154**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:arm,ignition](https://crbug.com/743154)**  
**[Commit: [builtins] Array.prototype.sort bug](https://chromium.googlesource.com/v8/v8/+/c7854ed)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:741078**  
   
**[Issue: CHECK failure: map->IsMap() in spaces.cc](https://crbug.com/741078)**  
**[Commit: [turbofan] Fix inline JSGeneratorObject allocation.](https://chromium.googlesource.com/v8/v8/+/0a4ad44)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-741078.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-741078.js)  
```javascript
function* gen() {}

(function warmup() {
  for (var i = 0; i < 100; ++i) {
    var g = gen();
    g.p = 42;
  }
})();

gc();   
gen();  
%OptimizeFunctionOnNextCall(gen);
gen();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a4ad44^!)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=0a4ad44)  
[test/mjsunit/regress/regress-crbug-741078.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-741078.js?cl=0a4ad44)  
  

### **crbug:740803**  
   
**[Issue: Security: Use After Free  in v8](https://crbug.com/740803)**  
**[Commit: [scope] Null out rare_data_ when aborting preparsing](https://chromium.googlesource.com/v8/v8/+/b56c0f7)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "reward-3000", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "reward-inprocess", "M-59", "M-60", "M-61", "merge-merged-6.0", "Merge-Merged-60", "Release-0-M60", "CVE-2017-5098", "CVE_description-submitted"]  
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
  

### **crbug:740591**  
   
**[Issue: Function expressions in initializers of for-of/in loops are incorrectly scoped](https://crbug.com/740591)**  
**[Commit: Rewrite scopes of initializers in for-in/of destructured declarations](https://chromium.googlesource.com/v8/v8/+/f1f2285)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-61", "merge-merged-6.1", "Merge-Merged-61"]  
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
[test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/AsyncGenerators.golden?cl=f1f2285)  
[test/cctest/interpreter/bytecode_expectations/ForAwaitOf.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForAwaitOf.golden?cl=f1f2285)  
[test/cctest/interpreter/bytecode_expectations/ForOfLoop.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ForOfLoop.golden?cl=f1f2285)  
[test/cctest/interpreter/bytecode_expectations/Generators.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/Generators.golden?cl=f1f2285)  
[test/mjsunit/regress/regress-crbug-740591.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-740591.js?cl=f1f2285)  
  

### **crbug:740398**  
   
**[Issue: CHECK failure: (location_) != nullptr in handles.h](https://crbug.com/740398)**  
**[Commit: Propagate exceptions from JSFunction::SetName as needed](https://chromium.googlesource.com/v8/v8/+/873d516)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
[test/mjsunit/regress/regress-crbug-740398.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-740398.js?cl=873d516)  
  

### **crbug:740116**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/740116)**  
**[Commit: [turbofan] Fix Reflect.getPrototypeOf on primitives.](https://chromium.googlesource.com/v8/v8/+/933a874)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:738763**  
   
**[Issue: CHECK failure: !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/738763)**  
**[Commit: [runtime] Add shortcuts for elements kinds transitions.](https://chromium.googlesource.com/v8/v8/+/b90e83f)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-59", "M-60", "ClusterFuzz-Verified", "NodeJS-Backport-Done", "M-61", "merge-merged-6.0", "merge-merged-6.1"]  
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
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=b90e83f)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b90e83f)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=b90e83f)  
[src/objects/map.h](https://cs.chromium.org/chromium/src/v8/src/objects/map.h?cl=b90e83f)  
[src/transitions-inl.h](https://cs.chromium.org/chromium/src/v8/src/transitions-inl.h?cl=b90e83f)  
[src/transitions.cc](https://cs.chromium.org/chromium/src/v8/src/transitions.cc?cl=b90e83f)  
[src/transitions.h](https://cs.chromium.org/chromium/src/v8/src/transitions.h?cl=b90e83f)  
[test/cctest/test-field-type-tracking.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-field-type-tracking.cc?cl=b90e83f)  
[test/mjsunit/regress/regress-crbug-738763.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-738763.js?cl=b90e83f)  
  

### **crbug:737645**  
   
**[Issue: V8 correctness failure in configs: x64,ignition_turbo:ia32,ignition_turbo](https://crbug.com/737645)**  
**[Commit: [runtime] Fix Array.prototype.sort for large entries](https://chromium.googlesource.com/v8/v8/+/78c74e6)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-737645.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-737645.js)  
```javascript
for (let i = 0; i < 100; i++) {
  
  
  
  let key = 1073741800 + i;
  var a = { length: 12, 1: 0xFA, [key]: 0xFB };
  %HeapObjectVerify(a);
  assertEquals(["1", ""+key, "length"], Object.keys(a));
  
  Array.prototype.sort.call(a);
  %HeapObjectVerify(a);
  assertEquals(["0", ""+key, "length"], Object.keys(a));
  
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
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=78c74e6)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=78c74e6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=78c74e6)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=78c74e6)  
[src/runtime/runtime-intl.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-intl.cc?cl=78c74e6)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=78c74e6)  
[test/mjsunit/regress/regress-crbug-737645.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-737645.js?cl=78c74e6)  
  

### **crbug:736633**  
   
**[Issue: Use-after-poison in v8::internal::compiler::InstructionSelector::EmitTableSwitch](https://crbug.com/736633)**  
**[Commit: [turbofan] Introduce upper limit for table switch size.](https://chromium.googlesource.com/v8/v8/+/4a4bcda)**  
  
Closed: Jul 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
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
[src/compiler/mips64/instruction-selector-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips64/instruction-selector-mips64.cc?cl=4a4bcda)  
[src/compiler/ppc/instruction-selector-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ppc/instruction-selector-ppc.cc?cl=4a4bcda)  
[src/compiler/s390/instruction-selector-s390.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/s390/instruction-selector-s390.cc?cl=4a4bcda)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=4a4bcda)  
[src/compiler/x87/instruction-selector-x87.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x87/instruction-selector-x87.cc?cl=4a4bcda)  
[test/mjsunit/regress/regress-crbug-736633.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-736633.js?cl=4a4bcda)  
  

### **crbug:736575**  
   
**[Issue: No Permission](https://crbug.com/736575)**  
**[Commit: [turbofan] Fix type for HOLEY_DOUBLE_ELEMENTS loads.](https://chromium.googlesource.com/v8/v8/+/533f0e3)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:736451**  
   
**[Issue: CHECK failure: ONE_BYTE == state_ in string.h](https://crbug.com/736451)**  
**[Commit: [string] Handle two-byte contents in String.p.toLowerCase](https://chromium.googlesource.com/v8/v8/+/3c26076)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "M-61", "merge-merged-6.0"]  
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
  

### **crbug:734162**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:ia32,ignition](https://crbug.com/734162)**  
**[Commit: [literals] Perform a deep boilerplate copy for MutableHeapNumber fields](https://chromium.googlesource.com/v8/v8/+/7dcd046)**  
  
Closed: Jun 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:734051**  
   
**[Issue: No Permission](https://crbug.com/734051)**  
**[Commit: [literals] Perform a deep boilerplate copy for MutableHeapNumber fields](https://chromium.googlesource.com/v8/v8/+/7dcd046)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:732169**  
   
**[Issue: Ill in v8::internal::TranslatedState::MaterializeCapturedObjectAt](https://crbug.com/732169)**  
**[Commit: [deoptimizer] Add support for materializing Generator objects.](https://chromium.googlesource.com/v8/v8/+/f555a69)**  
  
Closed: Jun 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-61"]  
Regress: [mjsunit/regress/regress-crbug-732169.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-732169.js)  
```javascript
(function TestGeneratorMaterialization() {
  function* f([x]) { yield x }
  
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
  
  for (var i = 0; i < 100; ++i) { g(); h(); }
  %OptimizeFunctionOnNextCall(h);
  h();  
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f555a69^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=f555a69)  
[test/mjsunit/regress/regress-crbug-732169.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-732169.js?cl=f555a69)  
  

### **crbug:731193**  
   
**[Issue: undefined prototypal inherited properties [OpenStreetMap iD editor]](https://crbug.com/731193)**  
**[Commit: [ic] Fix stub-cached access to use the dereffed thin-string.](https://chromium.googlesource.com/v8/v8/+/2325ef5)**  
  
Closed: Jun 2017  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Via-Wizard-Javascript"]  
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
  

### **crbug:729597**  
   
**[Issue: Null-dereference READ in heap](https://crbug.com/729597)**  
**[Commit: [heap-verify] Relax arguments verification](https://chromium.googlesource.com/v8/v8/+/66fe2d4)**  
  
Closed: Jun 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-60", "Merge-Rejected-60"]  
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
  

### **crbug:729573**  
   
**[Issue: No Permission](https://crbug.com/729573)**  
**[Commit: [deoptimizer] Teach the Deoptimizer about bound functions.](https://chromium.googlesource.com/v8/v8/+/337bb36)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:728813**  
   
**[Issue: Ill in v8::Utils::ReportApiFailure](https://crbug.com/728813)**  
**[Commit: Fix Array.indexOf for Proxies that throw](https://chromium.googlesource.com/v8/v8/+/8bc98b5)**  
  
Closed: Jun 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
  

### **crbug:725537**  
   
**[Issue: CHECK failure: map()->is_callable() in objects-debug.cc](https://crbug.com/725537)**  
**[Commit: [runtime] Set proper initial map for AsyncFunction constructor.](https://chromium.googlesource.com/v8/v8/+/397afc6)**  
  
Closed: May 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz"]  
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
  

### **crbug:725201**  
   
**[Issue: CHECK failure: fixed_array->IsDictionary() in objects-inl.h](https://crbug.com/725201)**  
**[Commit: [literals] Set the proper Map on the elements store for object literals](https://chromium.googlesource.com/v8/v8/+/106226e)**  
  
Closed: May 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "allpublic", "Clusterfuzz"]  
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
  

### **crbug:724608**  
   
**[Issue: CHECK failure: !map->is_deprecated() in compilation-dependencies.cc](https://crbug.com/724608)**  
**[Commit: [turbofan] Try to update deprecated maps first.](https://chromium.googlesource.com/v8/v8/+/468446d)**  
  
Closed: Jun 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Beta", "allpublic", "Clusterfuzz"]  
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
  

### **crbug:724153**  
   
**[Issue: CHECK failure: val <= std::min(static_cast<size_t>(std::numeric_limits<N>::max()), static_cast<](https://crbug.com/724153)**  
**[Commit: [turbofan] Fix value output count range on Operator.](https://chromium.googlesource.com/v8/v8/+/f7f03da)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
[test/mjsunit/regress/regress-crbug-724153.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-724153.js?cl=f7f03da)  
  

### **crbug:723455**  
   
**[Issue: CHECK failure: !map->is_stable() in access-info.cc](https://crbug.com/723455)**  
**[Commit: [turbofan][crankshaft] Don't generate elements kind transitions from stable maps.](https://chromium.googlesource.com/v8/v8/+/ea55b87)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:723132**  
   
**[Issue: Inconsistent binding of "this" in inline arrow function within a generator.](https://crbug.com/723132)**  
**[Commit: [parser] Stop treating generators as "top level" for preparsing purposes](https://chromium.googlesource.com/v8/v8/+/0439100)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
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
  

### **crbug:722871**  
   
**[Issue: Data race in v8::internal::ElementsAccessorBase<v8::internal::TypedElementsAccessor<](https://crbug.com/722871)**  
**[Commit: Add TSAN annotations for TypedArray accesses](https://chromium.googlesource.com/v8/v8/+/181c03e)**  
  
Closed: Sep 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-ThreadSanitizer", "Clusterfuzz", "ClusterFuzz-Verified", "M-63"]  
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
  

### **crbug:722783**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/722783)**  
**[Commit: [ic] Properly handle reconfiguring of a global property to 'readonly'.](https://chromium.googlesource.com/v8/v8/+/b30ea16)**  
  
Closed: Jul 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:722756**  
   
**[Issue: Type Confusion In Chrome Lead to RCE](https://crbug.com/722756)**  
**[Commit: [crankshaft] Fix HAliasAnalyzer for constants](https://chromium.googlesource.com/v8/v8/+/e33fd30)**  
  
Closed: May 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Security_Impact-Stable", "Arch-x86_64", "Security_Severity-High", "reward-7500", "allpublic", "reward-inprocess", "M-59", "Merge-Rejected-58", "Via-Wizard-Security", "merge-merged-5.9", "Release-0-M59", "CVE-2017-5070", "CVE_description-submitted"]  
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
  try {} catch(e) {}  
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
  

### **crbug:722348**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_asm](https://crbug.com/722348)**  
**[Commit: [asm.js] Properly handle unused function imports.](https://chromium.googlesource.com/v8/v8/+/d813f46)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:721835**  
   
**[Issue: CHECK failure: !failed_ in asm-parser.cc](https://crbug.com/721835)**  
**[Commit: [asm.js] Fix evaluation of first for-statement expression.](https://chromium.googlesource.com/v8/v8/+/f2b9c50)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
  

### **crbug:719479**  
   
**[Issue: CHECK failure: LoadElement of kRepFloat64 (NumberOrHole) cannot be changed to kRepTagged in rep](https://crbug.com/719479)**  
**[Commit: [turbofan] Don't mix element accesses with incompatible representations.](https://chromium.googlesource.com/v8/v8/+/d412cad)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
  

### **crbug:719384**  
   
**[Issue: CHECK failure: !isolate->has_pending_exception() in compiler.cc](https://crbug.com/719384)**  
**[Commit: [asm.js] Ensure lookups of imports are non-observable.](https://chromium.googlesource.com/v8/v8/+/ea48d83)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
[test/mjsunit/wasm/asm-wasm.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/wasm/asm-wasm.js?cl=ea48d83)  
  

### **crbug:718779**  
   
**[Issue: CHECK failure: !new_map->IsUnboxedDoubleField(index) in objects.cc](https://crbug.com/718779)**  
**[Commit: [runtime] MigrateFastToFast: fix check for unboxed inobject doubles](https://chromium.googlesource.com/v8/v8/+/ceba405)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
  

### **crbug:716912**  
   
**[Issue: Crash in IsFlagSet](https://crbug.com/716912)**  
**[Commit: Move delete-last-fast-property code from CSA to C++](https://chromium.googlesource.com/v8/v8/+/6cb995b)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
  

### **crbug:716804**  
   
**[Issue: CHECK failure: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/716804)**  
**[Commit: [ic] Fix handling of JSArray.length accessor info.](https://chromium.googlesource.com/v8/v8/+/26cf06b)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:716520**  
   
**[Issue: Crash in v8::internal::JSObject::FastPropertyAt](https://crbug.com/716520)**  
**[Commit: Fix FastAssign for self-assignment](https://chromium.googlesource.com/v8/v8/+/1f51f66)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
  

### **crbug:715862**  
   
**[Issue: CHECK failure: !map->is_stable() in hydrogen.cc](https://crbug.com/715862)**  
**[Commit: [ic] Filter out deprecated maps from polymorphic keyed ICs.](https://chromium.googlesource.com/v8/v8/+/0655ee8)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:715455**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_asm](https://crbug.com/715455)**  
**[Commit: [asm.js] Fix excessive function table sizes.](https://chromium.googlesource.com/v8/v8/+/a621462)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:715404**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/715404)**  
**[Commit: [turbofan] Fix lowering of Array constructor with one argument.](https://chromium.googlesource.com/v8/v8/+/d06d4ce)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:715151**  
   
**[Issue: CHECK failure: (map()->has_fast_smi_or_object_elements() || (elements() == GetHeap()->empty_fix](https://crbug.com/715151)**  
**[Commit: [turbofan] Fix buggy implicit coercion in GetMapWitness.](https://chromium.googlesource.com/v8/v8/+/e913f9e)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:714981**  
   
**[Issue: CHECK failure: map()->unused_property_fields() == actual_unused_property_fields - JSObject::kFi](https://crbug.com/714981)**  
**[Commit: Move delete-last-fast-property code from CSA to C++](https://chromium.googlesource.com/v8/v8/+/6cb995b)**  
  
Closed: May 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-60", "Test-Predator-Wrong"]  
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
  

### **crbug:714971**  
   
**[Issue: Unreachable code in asm-types.cc](https://crbug.com/714971)**  
**[Commit: [asm.js] Fix failure propagation of heap access validation.](https://chromium.googlesource.com/v8/v8/+/54818a6)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:714872**  
   
**[Issue: 6.5%-107.1% regression in page_cycler_v2.intl_es_fr_pt-BR at 465627:465860](https://crbug.com/714872)**  
**[Commit: Revert behavioral part of 84dc8ed4c3e6c8c1e3005b2d2445c64328b139a4](https://chromium.googlesource.com/v8/v8/+/86aa796)**  
  
Closed: Jul 2017  
Type: Bug-Regression  
Components: None  
Labels: ["Performance-Sheriff", "M-60"]  
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
  

### **crbug:714696**  
   
**[Issue: Fatal error in v8::FromJust](https://crbug.com/714696)**  
**[Commit: [d8] console methods must not throw.](https://chromium.googlesource.com/v8/v8/+/87b5b53)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:712802**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/712802)**  
**[Commit: [turbofan] Fix typing rule for JSCreateArguments.](https://chromium.googlesource.com/v8/v8/+/b89ddcf)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:711166**  
   
**[Issue: CHECK failure: context->IsContext() in contexts-inl.h](https://crbug.com/711166)**  
**[Commit: [turbofan] Fix translation containing arguments elements.](https://chromium.googlesource.com/v8/v8/+/e6590a3)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:709753**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/709753)**  
**[Commit: [turbofan] Properly represent the float64 hole.](https://chromium.googlesource.com/v8/v8/+/8c0c5e8)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "NodeJS-Backport-Rejected", "v8-foozzie-failure", "merge-merged-5.8", "merge-merged-58", "merge-merged-5.9"]  
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
[src/compiler/typed-optimization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.cc?cl=8c0c5e8)  
[src/compiler/typed-optimization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/typed-optimization.h?cl=8c0c5e8)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=8c0c5e8)  
[src/compiler/types.h](https://cs.chromium.org/chromium/src/v8/src/compiler/types.h?cl=8c0c5e8)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=8c0c5e8)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=8c0c5e8)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=8c0c5e8)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8c0c5e8)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=8c0c5e8)  
[test/mjsunit/regress/regress-crbug-709753.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-709753.js?cl=8c0c5e8)  
  

### **crbug:709537**  
   
**[Issue: CHECK failure: object.is_null() || *object == scope_site->transition_info() in allocation-site-](https://crbug.com/709537)**  
**[Commit: [turbofan] Fix traversal order of boilerplate objects.](https://chromium.googlesource.com/v8/v8/+/1f3a863)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
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
  

### **crbug:708050**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/708050)**  
**[Commit: [turbofan] Fix typing rule for CheckBounds.](https://chromium.googlesource.com/v8/v8/+/483812d)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure", "merge-merged-5.9"]  
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
  

### **crbug:707580**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:arm,ignition](https://crbug.com/707580)**  
**[Commit: [builtins] Make sure to perform ToPrimitive(key, hint string) in hasOwnProperty even if the receiver is a smi.](https://chromium.googlesource.com/v8/v8/+/fe04841)**  
  
Closed: Jun 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure", "merge-merged-6.0"]  
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
  

### **crbug:706642**  
   
**[Issue: Deoptimizing a derived constructor leaks the hole](https://crbug.com/706642)**  
**[Commit: [turbofan] Disable inlining of derived class constructors.](https://chromium.googlesource.com/v8/v8/+/c019e53)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Approved", "allpublic", "merge-merged-5.8", "merge-merged-58"]  
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
  

### **crbug:703610**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/703610)**  
**[Commit: [turbofan] Fix lowering of Function.prototype accesses.](https://chromium.googlesource.com/v8/v8/+/37b9d65)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:702798**  
   
**[Issue: CHECK failure: cell->value()->IsTheHole(isolate) in objects.cc](https://crbug.com/702798)**  
**[Commit: [ic] Fix 'prototype chain checks' where the holder is the receiver](https://chromium.googlesource.com/v8/v8/+/6f52dfd)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
Regress: [mjsunit/regress/regress-crbug-702798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-702798.js)  
```javascript
this.__defineGetter__("Object", ()=>0);
__proto__ = Realm.global(Realm.create());
Object;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f52dfd^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=6f52dfd)  
[test/mjsunit/regress/regress-crbug-702798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-702798.js?cl=6f52dfd)  
  

### **crbug:702793**  
   
**[Issue: No Permission](https://crbug.com/702793)**  
**[Commit: Fix LoadGlobalIC for cleared WeakCells](https://chromium.googlesource.com/v8/v8/+/f89db5d)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
  

### **crbug:702058**  
   
**[Issue: Security: ZDI-CAN-4587 - chrome OOB read (pwn2own 2017)](https://crbug.com/702058)**  
**[Commit: [csa] Bailout to the runtime for ToInteger conversion in Array.p.indexOf.](https://chromium.googlesource.com/v8/v8/+/9224d5d)**  
  
Closed: Mar 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Needs-Feedback", "Fracas", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "M-57", "M-58", "NodeJS-Backport-Rejected", "FoundIn-M-57", "merge-merged-5.7", "FoundIn-M-58", "merge-merged-5.8", "FoundIn-M-59", "Release-1-M57", "CVE-2017-5053", "CVE_description-submitted"]  
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
  

### **crbug:700733**  
   
**[Issue: !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/700733)**  
**[Commit: [ic] Fix handling of elements kind transitions in polymorphic keyed ICs.](https://chromium.googlesource.com/v8/v8/+/2d85654)**  
  
Closed: Apr 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-59", "Test-Predator-Wrong"]  
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
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=2d85654)  
[test/mjsunit/regress/regress-crbug-700733.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-700733.js?cl=2d85654)  
  

### **crbug:700678**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/700678)**  
**[Commit: [runtime] Fix KeyAccumulator for non-internalized keys.](https://chromium.googlesource.com/v8/v8/+/3b597bb)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:699282**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/699282)**  
**[Commit: [turbofan] Revert invalid optimization of flooring division.](https://chromium.googlesource.com/v8/v8/+/18be5d7)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:698607**  
   
**[Issue: Encountered unaccounted use by #635 (ObjectIsNaN) in escape-analysis.cc](https://crbug.com/698607)**  
**[Commit: [turbofan] Teach escape analysis about ObjectIsNaN.](https://chromium.googlesource.com/v8/v8/+/1e4a272)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:697017**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSFunction()) in objects-i](https://crbug.com/697017)**  
**[Commit: [runtime] Properly handle null constructor case when feeding back normalization.](https://chromium.googlesource.com/v8/v8/+/e003d21)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:696622**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/696622)**  
**[Commit: [turbofan] Fix lowering of %_GetSuperConstructor intrinsic.](https://chromium.googlesource.com/v8/v8/+/09a0703)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:694709**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/694709)**  
**[Commit: [turbofan] Fix Object.prototype.__proto__ getter reduction.](https://chromium.googlesource.com/v8/v8/+/beb94c5)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:694416**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/694416)**  
**[Commit: [turbofan] Fix missing name check for keyed global load.](https://chromium.googlesource.com/v8/v8/+/875ccb4)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:691687**  
   
**[Issue: new_it.done() == old_it.done() in objects-debug.cc](https://crbug.com/691687)**  
**[Commit: Accurately record eval calls in arrow parameter lists](https://chromium.googlesource.com/v8/v8/+/fc02366)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=fc02366)  
[test/mjsunit/regress/regress-crbug-691687.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-691687.js?cl=fc02366)  
  

### **crbug:691323**  
   
**[Issue: Security: Information Leak in Array indexOf](https://crbug.com/691323)**  
**[Commit: [elements] Check if the backing store has been neutered for indexOf](https://chromium.googlesource.com/v8/v8/+/3a43be9)**  
  
Closed: Feb 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Runtime  
Labels: ["reward-2000", "Security", "Security_Impact-Stable", "Security_Severity-Medium", "Arch-All", "allpublic", "reward-inprocess", "M-57", "merge-merged-57", "Release-0-M57", "CVE-2017-5040", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-691323.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-691323.js)  
```javascript
var buffer = new ArrayBuffer(0x100);
var array = new Uint8Array(buffer).fill(55);
var tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferNeuter(array.buffer)
  return 0;
};


assertEquals(-1, Array.prototype.indexOf.call(array, 0x00, tmp));

buffer = new ArrayBuffer(0x100);
array = new Uint8Array(buffer).fill(55);
tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferNeuter(array.buffer)
  return 0;
};


assertEquals(false, Array.prototype.includes.call(array, 0x00, tmp));

buffer = new ArrayBuffer(0x100);
array = new Uint8Array(buffer).fill(55);
tmp = {};
tmp[Symbol.toPrimitive] = function () {
  %ArrayBufferNeuter(array.buffer)
  return 0;
};
assertEquals(true, Array.prototype.includes.call(array, undefined, tmp));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3a43be9^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=3a43be9)  
[test/mjsunit/regress/regress-crbug-691323.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-691323.js?cl=3a43be9)  
  

### **crbug:688734**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/688734)**  
**[Commit: [ic] KeyedStoreIC should use a slow stub when a prototype chain contains dictionary elements.](https://chromium.googlesource.com/v8/v8/+/9760851)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:688567**  
   
**[Issue: Sloppy block function hoisting does not maintain declaration order](https://crbug.com/688567)**  
**[Commit: Retain source order when hoisting sloppy block functions](https://chromium.googlesource.com/v8/v8/+/fb16583)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript>Language  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
      
      function j(){}
    }
  }
  function i(){}

  
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
  

### **crbug:687990**  
   
**[Issue: We should never get here - unexpected deopt info in deoptimizer.cc](https://crbug.com/687990)**  
**[Commit: [turbofan] Properly ensure that deoptimization is enabled.](https://chromium.googlesource.com/v8/v8/+/c21d128)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
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
  

### **crbug:687063**  
   
**[Issue: V8 correctness failure in configs: x64,fullcode:x64,ignition_staging](https://crbug.com/687063)**  
**[Commit: [es2015] Fix ToPrimitive conversions in relational comparisons.](https://chromium.googlesource.com/v8/v8/+/08aec7d)**  
  
Closed: Sep 21 2018  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Recharge-Cold", "Clusterfuzz", "ClusterFuzz-Verified", "Project-Ignition", "v8-foozzie-failure"]  
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
}

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
  

### **crbug:687029**  
   
**[Issue: No Permission](https://crbug.com/687029)**  
**[Commit: [turbofan] Introduce ChangeTaggedToTaggedSigned operator.](https://chromium.googlesource.com/v8/v8/+/68ae57c)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=68ae57c)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=68ae57c)  
[test/mjsunit/regress/regress-crbug-687029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-687029.js?cl=68ae57c)  
  

### **crbug:686737**  
   
**[Issue: V8 correctness failure in configs: x64,fullcode:x64,ignition_turbo](https://crbug.com/686737)**  
**[Commit: [turbofan] Don't eliminate unused CheckFloat64Hole.](https://chromium.googlesource.com/v8/v8/+/b8df954)**  
  
Closed: Feb 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "v8-foozzie-failure"]  
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
  

### **crbug:686427**  
   
**[Issue: No Permission](https://crbug.com/686427)**  
**[Commit: [crankshaft] Fix Smi overflow in {HMaybeGrowElements}.](https://chromium.googlesource.com/v8/v8/+/6c12d57)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
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
[test/mjsunit/regress/regress-crbug-686427.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-686427.js?cl=6c12d57)  
  

### **crbug:686102**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/686102)**  
**[Commit: [turbofan] Don't constant-fold ACCESSOR properties.](https://chromium.googlesource.com/v8/v8/+/b912851)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "v8-foozzie-failure", "merge-merged-5.7"]  
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
  

### **crbug:685965**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo_opt](https://crbug.com/685965)**  
**[Commit: ThinStrings: fix CodeStubAssembler::SubString](https://chromium.googlesource.com/v8/v8/+/9ea3e56)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:685680**  
   
**[Issue: Encountered unaccounted use by #350 (Call) in escape-analysis.cc](https://crbug.com/685680)**  
**[Commit: [turbofan] Introduce dedicated StringIndexOf operator.](https://chromium.googlesource.com/v8/v8/+/b975441)**  
  
Closed: Jan 2017  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "ReleaseBlock-Dev", "HasTestcase", "Clusterfuzz", "M-58", "Test-Predator-Wrong"]  
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
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=b975441)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b975441)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=b975441)  
[src/compiler/simplified-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.h?cl=b975441)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=b975441)  
[src/compiler/verifier.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/verifier.cc?cl=b975441)  
[test/mjsunit/regress/regress-crbug-685680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-685680.js?cl=b975441)  
  

### **crbug:685634**  
   
**[Issue: !AreAliased(args_reg, scratch1, scratch2, scratch3) in code-generator-x64.cc](https://crbug.com/685634)**  
**[Commit: [turbofan] Don't try to optimize tail calls to .apply.](https://chromium.googlesource.com/v8/v8/+/7be3b4c)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:685506**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/685506)**  
**[Commit: [turbofan] Ensure {CheckMaps} is not used accross mutations.](https://chromium.googlesource.com/v8/v8/+/e752bcc)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:685504**  
   
**[Issue: str->IsSeqString() || str->IsExternalString() in factory.cc](https://crbug.com/685504)**  
**[Commit: ThinStrings: fix Factory::NewProperSubString](https://chromium.googlesource.com/v8/v8/+/7438304)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:685050**  
   
**[Issue: STANDARD_STORE == store_mode in js-native-context-specialization.cc](https://crbug.com/685050)**  
**[Commit: [turbofan] Remove over-restrictive DCHECKs.](https://chromium.googlesource.com/v8/v8/+/64eae6e)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
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
  

### **crbug:684208**  
   
**[Issue: V8 correctness failure in configs: x64,ignition:x64,ignition_turbo](https://crbug.com/684208)**  
**[Commit: [deoptimizer] Preserve double bit patterns correctly.](https://chromium.googlesource.com/v8/v8/+/7376e12)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "NodeJS-Backport-Rejected", "v8-foozzie-failure", "merge-merged-5.8", "merge-merged-5.9"]  
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
[src/mips/deoptimizer-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/deoptimizer-mips.cc?cl=7376e12)  
[src/mips64/deoptimizer-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/deoptimizer-mips64.cc?cl=7376e12)  
[src/ppc/deoptimizer-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/deoptimizer-ppc.cc?cl=7376e12)  
[src/s390/deoptimizer-s390.cc](https://cs.chromium.org/chromium/src/v8/src/s390/deoptimizer-s390.cc?cl=7376e12)  
[src/x64/deoptimizer-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/deoptimizer-x64.cc?cl=7376e12)  
[src/x87/deoptimizer-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/deoptimizer-x87.cc?cl=7376e12)  
[test/mjsunit/regress/regress-crbug-684208.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-684208.js?cl=7376e12)  
  

### **crbug:683667**  
   
**[Issue: !field_type->NowStable() || field_type->NowContains(value) || (!FLAG_use_allocat](https://crbug.com/683667)**  
**[Commit: [runtime] Mark old JSGlobalProxy's map as unstable when an iframe navigates away.](https://chromium.googlesource.com/v8/v8/+/1c7f839)**  
  
Closed: Mar 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "M-57", "NodeJS-Backport-Rejected", "Test-Predator-Wrong", "merge-merged-5.7", "merge-merged-5.8"]  
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
  

### **crbug:683581**  
   
**[Issue: V8 correctness failure in configs: x64,fullcode:x64,ignition_turbo](https://crbug.com/683581)**  
**[Commit: [turbofan] Fix accumulator use in bytecode analysis.](https://chromium.googlesource.com/v8/v8/+/efc8cb1)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:682194**  
   
**[Issue: Security: Out-of-bounds read in V8 Array.concat](https://crbug.com/682194)**  
**[Commit: [runtime] Fix Array.prototype.concat with complex @@species](https://chromium.googlesource.com/v8/v8/+/e560815)**  
  
Closed: Jan 2017  
Type: Bug-Security  
Components: Blink>JavaScript>Runtime  
Labels: ["Hotlist-Merge-Review", "M-56", "Security_Impact-Stable", "Security_Severity-High", "reward-7500", "allpublic", "reward-inprocess", "NodeJS-Backport-Done", "merge-merged-5.6", "merge-merged-5.7", "Release-0-M57", "CVE-2017-5030", "CVE_description-submitted", "Hotlist-Torque"]  
Regress: [mjsunit/regress/regress-crbug-682194.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-682194.js)  
```javascript
var proxy = new Proxy([], {
  defineProperty() {
    w.length = 1;  
    gc();          
    return Object.defineProperty.apply(this, arguments);
  }
});

class MyArray extends Array {
  
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
  

### **crbug:681983**  
   
**[Issue: V8 correctness failure in configs: x64,fullcode:x64,ignition_staging](https://crbug.com/681983)**  
**[Commit: [turbofan] Fix translation of uint32 deopt immediates.](https://chromium.googlesource.com/v8/v8/+/7682837)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "v8-foozzie-failure"]  
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
  

### **crbug:679841**  
   
**[Issue: Stack-buffer-overflow in v8::internal::DoubleToRadixCString](https://crbug.com/679841)**  
**[Commit: Add test case for Number.prototype.toString (r42364).](https://chromium.googlesource.com/v8/v8/+/d33dc16)**  
  
Closed: Jan 2017  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-3500", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "reward-inprocess", "M-57"]  
Regress: [mjsunit/regress/regress-crbug-679841.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-679841.js)  
```javascript
(-1e-301).toString(2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d33dc16^!)  
[test/mjsunit/regress/regress-crbug-679841.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-679841.js?cl=d33dc16)  
  

### **crbug:679378**  
   
**[Issue: Crash in heap](https://crbug.com/679378)**  
**[Commit: [turbofan] Don't merge PropertyAccessInfos with different field maps.](https://chromium.googlesource.com/v8/v8/+/64963e1)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
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
  

### **crbug:679202**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/679202)**  
**[Commit: [crankshaft] Properly deal with null prototype.](https://chromium.googlesource.com/v8/v8/+/5f418c8)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Merge-Rejected-55", "Test-Predator-Wrong", "merge-merged-56"]  
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
  

### **crbug:677757**  
   
**[Issue: Fatal error in](https://crbug.com/677757)**  
**[Commit: [turbofan] Teach escape analysis about StringCharAt](https://chromium.googlesource.com/v8/v8/+/5662f99)**  
  
Closed: Jan 2017  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Test-Predator-Wrong"]  
Regress: [mjsunit/regress/regress-crbug-677757.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-677757.js)  
```javascript
for (var i = 0; i < 50000; i++) {
 ("Foo"[0] + "barbarbarbarbarbar")[0];
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5662f99^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=5662f99)  
[test/mjsunit/regress/regress-crbug-677757.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-677757.js?cl=5662f99)  
  

### **crbug:673008**  
   
**[Issue: hasOwnProperty returning inconsistent results](https://crbug.com/673008)**  
**[Commit: [stubs] Fix negative index lookup in hasOwnProperty](https://chromium.googlesource.com/v8/v8/+/bb753b6)**  
  
Closed: Dec 2016  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-55", "Needs-Feedback", "Arch-x86_64", "ReleaseBlock-Stable", "NodeJS-Backport-Rejected", "merge-merged-5.5", "Via-Wizard-Javascript", "merge-merged-5.6", "TE-Verified-M57", "prestable-55.0.2883.87", "TE-Verified-57.0.2951.0"]  
Regress: [mjsunit/regress/regress-crbug-673008.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-673008.js)  
```javascript
var a = {
  "33": true,
  "-1": true
};

var strkeys = Object.keys(a).map(function(k) { return "" + k });
var numkeys = Object.keys(a).map(function(k) { return +k });
var keys = strkeys.concat(numkeys);

keys.forEach(function(k) {
  assertTrue(a.hasOwnProperty(k),
             "property not found: " + k + "(" + (typeof k) + ")");
});

var b = {};
b.__proto__ = a;
keys.forEach(function(k) {
  assertTrue(k in b, "property not found: " + k + "(" + (typeof k) + ")");
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb753b6^!)  
[src/builtins/builtins-object.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object.cc?cl=bb753b6)  
[src/code-stub-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.cc?cl=bb753b6)  
[test/mjsunit/regress/regress-crbug-673008.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-673008.js?cl=bb753b6)  
  

### **crbug:672792**  
   
**[Issue: size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/672792)**  
**[Commit: Fix usage of literal cloning for large double arrays.](https://chromium.googlesource.com/v8/v8/+/6c620e5)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-672792.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-672792.js)  
```javascript
eval("function f() { return [" + String("0.1,").repeat(65535) + "] }");


assertEquals(65535, f().length);


assertEquals(65535, f().length);


%OptimizeFunctionOnNextCall(f);
assertEquals(65535, f().length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c620e5^!)  
[src/ast/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast.cc?cl=6c620e5)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=6c620e5)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=6c620e5)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=6c620e5)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=6c620e5)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=6c620e5)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=6c620e5)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=6c620e5)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=6c620e5)  
[test/mjsunit/regress/regress-crbug-672792.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-672792.js?cl=6c620e5)  
  

### **crbug:671576**  
   
**[Issue: map_index >= Context::TYPED_ARRAY_KEY_ITERATOR_MAP_INDEX in js-builtin-reducer.c](https://crbug.com/671576)**  
**[Commit: Fix assertion failure in JSBuiltinReducer::ReduceArrayIterator.](https://chromium.googlesource.com/v8/v8/+/a610155)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-671576.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-671576.js)  
```javascript
function f() {
  for (var i of [NaN].keys());
}

f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a610155^!)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=a610155)  
[test/mjsunit/regress/regress-crbug-671576.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-671576.js?cl=a610155)  
  

### **crbug:669850**  
   
**[Issue: size <= kMaxRegularHeapObjectSize in runtime-internal.cc](https://crbug.com/669850)**  
**[Commit: [turbofan] Workaround for unknown array literal length.](https://chromium.googlesource.com/v8/v8/+/f8fec66)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-669850.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669850.js)  
```javascript
eval('function f(a) { return [' + new Array(1 << 17) + ',a] }');
assertEquals(23, f(23)[1 << 17]);
assertEquals(42, f(42)[1 << 17]);
%OptimizeFunctionOnNextCall(f);
assertEquals(65, f(65)[1 << 17]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f8fec66^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=f8fec66)  
[test/mjsunit/regress/regress-crbug-669850.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669850.js?cl=f8fec66)  
  

### **crbug:669540**  
   
**[Issue: Missing hole check in computed property names](https://crbug.com/669540)**  
**[Commit: [ignition] Fix hole check for dynamic local variables](https://chromium.googlesource.com/v8/v8/+/f6ee3b5)**  
  
Closed: Dec 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["reward-topanel", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-669540.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669540.js)  
```javascript
function f({
    [
        (function g() {
            assertThrows(function(){
                print(eval("p"));
            }, ReferenceError);
        })()
    ]: p
}) {};

f({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6ee3b5^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=f6ee3b5)  
[test/mjsunit/regress/regress-crbug-669540.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669540.js?cl=f6ee3b5)  
  

### **crbug:669451**  
   
**[Issue: Fatal error in ../../v8/src/compiler/escape-analysis.cc, line 824](https://crbug.com/669451)**  
**[Commit: [turbofan] Teach escape analysis about ConvertTaggedHoleToUndefined.](https://chromium.googlesource.com/v8/v8/+/d6752d9)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-669451.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669451.js)  
```javascript
function foo() {
  var a = [,];
  a[0] = {}
  a[0].toString = FAIL;
}
try { foo(); } catch (e) {}
try { foo(); } catch (e) {}
%OptimizeFunctionOnNextCall(foo);
try { foo(); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6752d9^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=d6752d9)  
[test/mjsunit/regress/regress-crbug-669451.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669451.js?cl=d6752d9)  
  

### **crbug:669411**  
   
**[Issue: Crash in v8::internal::Factory::NewTuple2](https://crbug.com/669411)**  
**[Commit: [ic] Use validity cells to protect keyed element stores against object's prototype chain modifications.](https://chromium.googlesource.com/v8/v8/+/39e6f2ca)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-669411.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-669411.js)  
```javascript
function f(o) {
  o[5000000] = 0;
}
var o = Object.create(null);
f(o);
f(o);
f(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39e6f2ca^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=39e6f2ca)  
[src/ast/ast-types.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-types.cc?cl=39e6f2ca)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=39e6f2ca)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=39e6f2ca)  
[src/compiler/types.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/types.cc?cl=39e6f2ca)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=39e6f2ca)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=39e6f2ca)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=39e6f2ca)  
[src/ic/accessor-assembler-impl.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler-impl.h?cl=39e6f2ca)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=39e6f2ca)  
[src/ic/ic-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic-compiler.cc?cl=39e6f2ca)  
[src/ic/ic-compiler.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic-compiler.h?cl=39e6f2ca)  
[src/ic/ic-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic-inl.h?cl=39e6f2ca)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=39e6f2ca)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=39e6f2ca)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=39e6f2ca)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=39e6f2ca)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=39e6f2ca)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=39e6f2ca)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=39e6f2ca)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=39e6f2ca)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=39e6f2ca)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=39e6f2ca)  
[src/value-serializer.cc](https://cs.chromium.org/chromium/src/v8/src/value-serializer.cc?cl=39e6f2ca)  
[test/mjsunit/regress/regress-crbug-662907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662907.js?cl=39e6f2ca)  
[test/mjsunit/regress/regress-crbug-669411.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-669411.js?cl=39e6f2ca)  
  

### **crbug:668795**  
   
**[Issue: length == previously_materialized_objects->length() in deoptimizer.cc](https://crbug.com/668795)**  
**[Commit: [deoptimizer] Fix deoptimization in {TranslatedState}.](https://chromium.googlesource.com/v8/v8/+/204babf)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-668795.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668795.js)  
```javascript
function g() {
  return g.arguments;
}

function f() {
  var result = "R:";
  for (var i = 0; i < 3; ++i) {
    if (i == 1) %OptimizeOsr();
    result += g([1])[0];
    result += g([2])[0];
  }
  return result;
}

assertEquals("R:121212", f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/204babf^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=204babf)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=204babf)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=204babf)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=204babf)  
[test/mjsunit/regress/regress-crbug-668795.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668795.js?cl=204babf)  
  

### **crbug:668414**  
   
**[Issue: Difference between x64 and ia32: array concat](https://crbug.com/668414)**  
**[Commit: [runtime] Add missing @@IsConcatSpreadable check for FAST_DOUBLE_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/a09e5ed)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-668414.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668414.js)  
```javascript
(function testSmiArrayConcat() {
  var result = [].concat([-12]);

  assertEquals(1, result.length);
  assertEquals([-12], result);
})();

(function testDoubleArrayConcat() {
  var result = [].concat([-1073741825]);

  assertEquals(1, result.length);
  assertEquals([-1073741825], result);
})();

(function testSmiArrayNonConcatSpreadable() {
  var array = [-10];
  array[Symbol.isConcatSpreadable] = false;
  var result = [].concat(array);

  assertEquals(1, result.length);
  assertEquals(1, result[0].length);
  assertEquals([-10], result[0]);
})();

(function testDoubleArrayNonConcatSpreadable() {
  var array = [-1073741825];
  array[Symbol.isConcatSpreadable] = false;
  var result = [].concat(array);

  assertEquals(1, result.length);
  assertEquals(1, result[0].length);
  assertEquals([-1073741825], result[0]);
})();

Array.prototype[Symbol.isConcatSpreadable] = false;


(function testSmiArray() {
  var result = [].concat([-12]);

  assertEquals(2, result.length);
  assertEquals(0, result[0].length);
  assertEquals(1, result[1].length);
  assertEquals([-12], result[1]);
})();

(function testDoubleArray() {
  var result = [].concat([-1073741825]);

  assertEquals(2, result.length);
  assertEquals(0, result[0].length);
  assertEquals(1, result[1].length);
  assertEquals([-1073741825], result[1]);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a09e5ed^!)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=a09e5ed)  
[test/mjsunit/regress/regress-crbug-668414.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668414.js?cl=a09e5ed)  
  

### **crbug:668101**  
   
**[Issue: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/668101)**  
**[Commit: [stubs] Fix AccessorInfo mixup in KeyedStoreGeneric](https://chromium.googlesource.com/v8/v8/+/2661b3e)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-668101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-668101.js)  
```javascript
function f(a, i, v) {
  a[i] = v;
}

f("make it generic", 0, 0);

var a = new Array(3);

f(a, "length", 2);
assertEquals(2, a.length);


%OptimizeObjectForAddingMultipleProperties(a, 1);
f(a, "length", 1);
assertEquals(1, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2661b3e^!)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=2661b3e)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=2661b3e)  
[test/mjsunit/regress/regress-crbug-668101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-668101.js?cl=2661b3e)  
  

### **crbug:667689**  
   
**[Issue: Difference between fullcode and ignition_turbo: instanceof and Symbol.hasInstance](https://crbug.com/667689)**  
**[Commit: [turbofan] Fix broken effect chain for instanceof.](https://chromium.googlesource.com/v8/v8/+/84c9360)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Arch-All", "M-57", "NodeJS-Backport-Rejected", "merge-merged-5.5", "merge-merged-5.6", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-667689.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-667689.js)  
```javascript
function foo() {}
foo.__defineGetter__(undefined, function() {})

function bar() {}
function baz(x) { return x instanceof bar };
%OptimizeFunctionOnNextCall(baz);
baz();
Object.setPrototypeOf(bar, null);
bar[Symbol.hasInstance] = function() { return true };
assertTrue(baz());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84c9360^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=84c9360)  
[test/mjsunit/regress/regress-crbug-667689.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-667689.js?cl=84c9360)  
  

### **crbug:666742**  
   
**[Issue: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/666742)**  
**[Commit: [ic] Ensure prototype validity cell guards global object's prototype changes for LoadGlobalIC.](https://chromium.googlesource.com/v8/v8/+/8ca50a8)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-666742.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-666742.js)  
```javascript
var p = {x:1};
__proto__ = p;
assertEquals(x, 1);
__proto__ = {x:13};
assertEquals(x, 13);
__proto__ = {x:42};
p = null;
gc();
assertEquals(x, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ca50a8^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8ca50a8)  
[test/mjsunit/regress/regress-crbug-666742.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-666742.js?cl=8ca50a8)  
  

### **crbug:666308**  
   
**[Issue: Difference between fullcode and crankshaft_opt: prototype and instanceof](https://crbug.com/666308)**  
**[Commit: [crankshaft] Don't inline the fast path for instanceof if the function has a non-instance .prototype](https://chromium.googlesource.com/v8/v8/+/0c70f37)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-666308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-666308.js)  
```javascript
function foo() {}
foo.prototype = 1;
v = new foo();
function bar() { return v instanceof foo; }
assertThrows(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c70f37^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=0c70f37)  
[test/mjsunit/regress/regress-crbug-666308.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-666308.js?cl=0c70f37)  
  

### **crbug:665886**  
   
**[Issue: pc_->Mask(ExceptionMask) == HLT in simulator-arm64.cc](https://crbug.com/665886)**  
**[Commit: [ic] Invalidate prototype validity cell when a slow prototype becomes fast.](https://chromium.googlesource.com/v8/v8/+/f718cd1)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.6"]  
Regress: [mjsunit/regress/regress-crbug-665886.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665886.js)  
```javascript
[1].toLocaleString();
delete Number.prototype.toLocaleString;
[1].toLocaleString();
var o = {};
o.__proto__ = { toString: Array.prototype.toString };
o.toString();
Number.prototype.arrayToString = Array.prototype.toString;
(42).arrayToString();
var a = [7];
a.toLocaleString();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f718cd1^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=f718cd1)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f718cd1)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=f718cd1)  
[test/mjsunit/regress/regress-crbug-665886.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665886.js?cl=f718cd1)  
  

### **crbug:665793**  
   
**[Issue: [crankshaft] OOB string access returns wrong value](https://crbug.com/665793)**  
**[Commit: [crankshaft] Properly handle OOB string accesses.](https://chromium.googlesource.com/v8/v8/+/576a46f)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-665793.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665793.js)  
```javascript
function foo() {
  return 'x'[1];
}
assertEquals(undefined, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(undefined, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/576a46f^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=576a46f)  
[test/mjsunit/regress/regress-crbug-665793.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665793.js?cl=576a46f)  
  

### **crbug:665587**  
   
**[Issue: Crash in v8::internal::Simulator::DecodeType2](https://crbug.com/665587)**  
**[Commit: [turbofan] Fix bogus representation for {kCheckTaggedHole}.](https://chromium.googlesource.com/v8/v8/+/31a8ec7)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Merge-Rejected-56"]  
Regress: [mjsunit/regress/regress-crbug-665587.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-665587.js)  
```javascript
var a = new (function() { this[0] = 1 });
function f() {
  for (var i = 0; i < 4; ++i) {
    var x = a[0];
    (function() { return x });
    if (i == 1) %OptimizeOsr();
    gc();
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/31a8ec7^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=31a8ec7)  
[test/mjsunit/regress/regress-crbug-665587.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-665587.js?cl=31a8ec7)  
  

### **crbug:664974**  
   
**[Issue: NameDictionary::kNotFound == current->property_dictionary()->FindEntry(name) in](https://crbug.com/664974)**  
**[Commit: [ic] Don't check full prototype chain if name is a private symbol.](https://chromium.googlesource.com/v8/v8/+/4513532)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "M-56", "Hotlist-Merge-Approved", "ReleaseBlock-Stable", "Clusterfuzz", "merge-merged-5.6"]  
Regress: [mjsunit/regress/regress-crbug-664974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664974.js)  
```javascript
for (var i = 0; i < 2000; i++) {
  Object.prototype['X'+i] = true;
}

var m = new Map();
m.set(Object.prototype, 23);

var o = {};
m.set(o, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4513532^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=4513532)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664802.js?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664974.js?cl=4513532)  
  

### **crbug:664942**  
   
**[Issue: Difference between fullcode and ignition_staging_turbo: string vs. array access](https://crbug.com/664942)**  
**[Commit: [turbofan] Properly allocate constant-folded string.](https://chromium.googlesource.com/v8/v8/+/5667280)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-664942.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664942.js)  
```javascript
function foo() {
  return 'x'[0];
}
foo();
%OptimizeFunctionOnNextCall(foo);
assertEquals("x", foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5667280^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=5667280)  
[test/mjsunit/regress/regress-crbug-664942.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664942.js?cl=5667280)  
  

### **crbug:664802**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in objects-inl](https://crbug.com/664802)**  
**[Commit: [ic] Don't check full prototype chain if name is a private symbol.](https://chromium.googlesource.com/v8/v8/+/4513532)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Test-Predator-Wrong", "merge-merged-5.6"]  
Regress: [mjsunit/regress/regress-crbug-664802.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664802.js)  
```javascript
var o = {};
o.__proto__ = new Proxy({}, {});

var m = new Map();
m.set({});
m.set(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4513532^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=4513532)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664802.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664802.js?cl=4513532)  
[test/mjsunit/regress/regress-crbug-664974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664974.js?cl=4513532)  
  

### **crbug:664506**  
   
**[Issue: Difference between fullcode and ignition_staging: array toString and gc](https://crbug.com/664506)**  
**[Commit: [builtins] Fix pointer comparison in ToString builtin.](https://chromium.googlesource.com/v8/v8/+/79aee39)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Proj-Ignition", "merge-merged-5.6", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-664506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664506.js)  
```javascript
gc();
gc();
assertEquals("[object Object]", Object.prototype.toString.call({}));
gc();
assertEquals("[object Array]", Object.prototype.toString.call([]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79aee39^!)  
[src/builtins/builtins-object.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-object.cc?cl=79aee39)  
[test/mjsunit/regress/regress-crbug-664506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664506.js?cl=79aee39)  
  

### **crbug:664469**  
   
**[Issue: Crash in v8::internal::Simulator::DecodeType3](https://crbug.com/664469)**  
**[Commit: [ic] Fix elements conversion in KeyedStoreGeneric](https://chromium.googlesource.com/v8/v8/+/567904f)**  
  
Closed: Nov 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-56", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-664469.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664469.js)  
```javascript
function f(a, i) {
  a[i] = "object";
}

f("make it generic", 0);


var kLength = 500000 / 8;
var kValue = 0.1;
var a = new Array(kLength);
for (var i = 0; i < kLength; i++) {
  a[i] = kValue;
}
f(a, 0);
for (var i = 1; i < kLength; i++) {
  assertEquals(kValue, a[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/567904f^!)  
[src/ic/keyed-store-generic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/keyed-store-generic.cc?cl=567904f)  
[test/mjsunit/regress/regress-crbug-664469.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664469.js?cl=567904f)  
  

### **crbug:664084**  
   
**[Issue: Missing ToNumber conversion in Crankshaft.](https://crbug.com/664084)**  
**[Commit: [crankshaft] Not all HAdd instructions produce a number.](https://chromium.googlesource.com/v8/v8/+/6d53340)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-664084.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-664084.js)  
```javascript
function foo() {
  return +({} + 1);
}

assertEquals(NaN, foo());
assertEquals(NaN, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(NaN, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6d53340^!)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=6d53340)  
[test/mjsunit/regress/regress-crbug-664084.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-664084.js?cl=6d53340)  
  

### **crbug:663750**  
   
**[Issue: Object.freeze doesn't deoptimize properly](https://crbug.com/663750)**  
**[Commit: [runtime] Ensure Object.freeze() deoptimizes code that depends on global property cells.](https://chromium.googlesource.com/v8/v8/+/6aa16ed)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Arch-All", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-663750.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663750.js)  
```javascript
var v = 0;
function foo(a) {
  v = a;
}
this.x = 0;
delete x;

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
assertEquals(undefined, v);

Object.freeze(this);

foo(4);
foo(5);
%OptimizeFunctionOnNextCall(foo);
foo(6);
assertEquals(undefined, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6aa16ed^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=6aa16ed)  
[test/mjsunit/regress/regress-crbug-663750.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663750.js?cl=6aa16ed)  
  

### **crbug:663410**  
   
**[Issue: Function arugment evasion](https://crbug.com/663410)**  
**[Commit: Fix 'combo breaker' in CreateDynamicFunction to handle template literals.](https://chromium.googlesource.com/v8/v8/+/e0d608a)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Needs-Feedback", "Via-Wizard-Javascript"]  
Regress: [mjsunit/regress/regress-crbug-663410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663410.js)  
```javascript
function alert(x) {};
assertThrows(
  'Function("a=`","`,xss=1){alert(xss)")'
);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0d608a^!)  
[src/builtins/builtins-function.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-function.cc?cl=e0d608a)  
[test/mjsunit/regress-crbug-663410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-663410.js?cl=e0d608a)  
  

### **crbug:663402**  
   
**[Issue: Security: [arm] OOB r/w due to size computation bug in MacroAssembler::Allocate](https://crbug.com/663402)**  
**[Commit: [arm] Fix custom addition in MacroAssembler::[Fast]Allocate](https://chromium.googlesource.com/v8/v8/+/87332fd)**  
  
Closed: Nov 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Security_Impact-Stable", "M-54", "Security_Severity-High", "allpublic", "merge-merged-5.5"]  
Regress: [mjsunit/regress/regress-crbug-663402.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663402.js)  
```javascript
var g_eval = eval;
function emit_f(size) {
  var body = "function f(x) {" +
             "  if (x < 0) return x;" +
             "  var a = [1];" +
             "  if (x > 0) return [";
  for (var i = 0; i < size; i++) {
    body += "0.1, ";
  }
  body += "  ];" +
          "  return a;" +
          "}";
  g_eval(body);
}



var kLength = 701;
emit_f(kLength);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
var a = f(1);


var b = new Object();
for (var i = 0; i < kLength; i++) {
  assertEquals(0.1, a[i]);
}


for (var i = 0; i < 300; i++) {
  f(1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87332fd^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=87332fd)  
[test/mjsunit/regress/regress-crbug-663402.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663402.js?cl=87332fd)  
  

### **crbug:663340**  
   
**[Issue: Difference between fullcode and ignition_staging_turbo_opt: array shift](https://crbug.com/663340)**  
**[Commit: [crankshaft] Ensure that we use inlined Array.prototype.shift only when there's no elements in the prototype chain.](https://chromium.googlesource.com/v8/v8/+/faf80b4)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-663340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-663340.js)  
```javascript
var expected = undefined;

function foo() {
  var a = [0,,{}];
  a.shift();
  assertEquals(expected, a[0]);
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();

expected = 42;
Array.prototype[0] = 153;
Array.prototype[1] = expected;
foo();

function bar() {
  var a = [0,,{}];
  a.shift();
  assertEquals(expected, a[0]);
}
bar();
bar();
%OptimizeFunctionOnNextCall(bar);
bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/faf80b4^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=faf80b4)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=faf80b4)  
[src/prototype.h](https://cs.chromium.org/chromium/src/v8/src/prototype.h?cl=faf80b4)  
[test/mjsunit/regress/regress-crbug-663340.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-663340.js?cl=faf80b4)  
  

### **crbug:662907**  
   
**[Issue: Difference between fullcode and crankshaft_opt: array prototype setter](https://crbug.com/662907)**  
**[Commit: [ic] Use validity cells to protect keyed element stores against object's prototype chain modifications.](https://chromium.googlesource.com/v8/v8/+/a39522f)**  
  
Closed: Dec 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-662907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662907.js)  
```javascript
(function() {
  function foo() {
    var a = new Array();
    a[0] = 10;
    return a;
  }

  assertEquals(1, foo().length);

  gc();
  gc();
  gc();
  gc();

  
  
  
  Array.prototype.__defineSetter__("0", function() {});

  assertEquals(0, foo().length);
})();


(function() {
  function foo() {
    var a = new Array();
    a[0] = 10;
    return a;
  }

  
  Array.prototype[123456789] = 42;
  Array.prototype.length = 0;

  assertEquals(1, foo().length);

  gc();
  gc();
  gc();
  gc();

  
  
  Array.prototype.__defineSetter__("0", function() {});

  assertEquals(0, foo().length);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a39522f^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=a39522f)  
[src/ast/ast-types.cc](https://cs.chromium.org/chromium/src/v8/src/ast/ast-types.cc?cl=a39522f)  
[src/builtins/builtins-array.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins-array.cc?cl=a39522f)  
[src/code-stub-assembler.h](https://cs.chromium.org/chromium/src/v8/src/code-stub-assembler.h?cl=a39522f)  
[src/compiler/types.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/types.cc?cl=a39522f)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=a39522f)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=a39522f)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=a39522f)  
[src/ic/accessor-assembler-impl.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler-impl.h?cl=a39522f)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=a39522f)  
[src/ic/ic-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic-compiler.cc?cl=a39522f)  
[src/ic/ic-compiler.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic-compiler.h?cl=a39522f)  
[src/ic/ic-inl.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic-inl.h?cl=a39522f)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=a39522f)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=a39522f)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=a39522f)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=a39522f)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=a39522f)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=a39522f)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a39522f)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=a39522f)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=a39522f)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=a39522f)  
[src/value-serializer.cc](https://cs.chromium.org/chromium/src/v8/src/value-serializer.cc?cl=a39522f)  
[test/mjsunit/regress/regress-crbug-662907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662907.js?cl=a39522f)  
  

### **crbug:662854**  
   
**[Issue: Difference between fullcode and crankshaft_opt: missing reference error](https://crbug.com/662854)**  
**[Commit: [ic] Support data handlers in LoadGlobalIC.](https://chromium.googlesource.com/v8/v8/+/937b8cb)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["merge-merged-5.6", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-662854.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662854.js)  
```javascript
function f() {
  typeof boom;
  boom;
}

assertThrows(()=>f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/937b8cb^!)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=937b8cb)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=937b8cb)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=937b8cb)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=937b8cb)  
[src/ic/accessor-assembler-impl.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler-impl.h?cl=937b8cb)  
[src/ic/accessor-assembler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.cc?cl=937b8cb)  
[src/ic/accessor-assembler.h](https://cs.chromium.org/chromium/src/v8/src/ic/accessor-assembler.h?cl=937b8cb)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=937b8cb)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=937b8cb)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=937b8cb)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=937b8cb)  
[test/mjsunit/regress/regress-crbug-662854.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662854.js?cl=937b8cb)  
  

### **crbug:662830**  
   
**[Issue: fixed_size_above_fp + (stack_slots * kPointerSize) - CommonFrameConstants::kFixe](https://crbug.com/662830)**  
**[Commit: [interpreter] Fix stack unwinding of deoptimized frames.](https://chromium.googlesource.com/v8/v8/+/a90671f)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "Test-Predator-Wrong", "merge-merged-5.6"]  
Regress: [mjsunit/regress/regress-crbug-662830.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662830.js)  
```javascript
function f() {
  %_DeoptimizeNow();
  throw 1;
}

function g() {
  try { f(); } catch(e) { }
  for (var i = 0; i < 3; ++i) if (i === 1) %OptimizeOsr();
  %_DeoptimizeNow();
}

%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a90671f^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=a90671f)  
[test/mjsunit/regress/regress-crbug-662830.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662830.js?cl=a90671f)  
  

### **crbug:662410**  
   
**[Issue: Crash in v8::internal::Invoke](https://crbug.com/662410)**  
**[Commit: [turbofan] Properly rename receiver on CheckHeapObject.](https://chromium.googlesource.com/v8/v8/+/a758c19)**  
  
Closed: Nov 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-56", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-662410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662410.js)  
```javascript
function g(v) { return v.constructor; }

g({});
g({});

function f() {
  var i = 0;
  do {
    i = i + 1;
    g(i);
  } while (i < 1);
}

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a758c19^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=a758c19)  
[src/compiler/js-call-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-call-reducer.cc?cl=a758c19)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=a758c19)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=a758c19)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=a758c19)  
[test/mjsunit/regress/regress-crbug-662410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662410.js?cl=a758c19)  
  

### **crbug:662367**  
   
**[Issue: Different division results with Infinity and NaN between default and ignition](https://crbug.com/662367)**  
**[Commit: [crankshaft] Fix constant folding of HDiv instruction.](https://chromium.googlesource.com/v8/v8/+/9906b3e)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-662367.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-662367.js)  
```javascript
var zero = 0;

(function ConstantFoldZeroDivZero() {
  function f() {
    return 0 / zero;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();

(function ConstantFoldMinusZeroDivZero() {
  function f() {
    return -0 / zero;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();

(function ConstantFoldNaNDivZero() {
  function f() {
    return NaN / 0;
  }
  assertTrue(isNaN(f()));
  assertTrue(isNaN(f()));
  %OptimizeFunctionOnNextCall(f);
  assertTrue(isNaN(f()));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9906b3e^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=9906b3e)  
[test/mjsunit/regress/regress-crbug-662367.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-662367.js?cl=9906b3e)  
  

### **crbug:661949**  
   
**[Issue: We should never get here - unexpected deopt info in deoptimizer.cc](https://crbug.com/661949)**  
**[Commit: [turbofan] CheckBounds cannot be used within asm.js.](https://chromium.googlesource.com/v8/v8/+/1003374)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-661949.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-661949.js)  
```javascript
var foo = (function() {
  'use asm';
  function foo() { ''[0]; }
  return foo;
})();

foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1003374^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=1003374)  
[src/compiler/node-matchers.h](https://cs.chromium.org/chromium/src/v8/src/compiler/node-matchers.h?cl=1003374)  
[test/mjsunit/regress/regress-crbug-661949.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-661949.js?cl=1003374)  
  

### **crbug:660379**  
   
**[Issue: Difference between default and ignition with natives](https://crbug.com/660379)**  
**[Commit: [turbofan] Advance bytecode offset after lazy deopt.](https://chromium.googlesource.com/v8/v8/+/93c6595)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Proj-Ignition", "v8-foozzie-failure"]  
Regress: [mjsunit/regress/regress-crbug-660379.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-660379.js)  
```javascript
(function InlinedThrowAtEndOfTry() {
  function g() {
    %DeoptimizeFunction(f);
    throw "boom";
  }
  function f() {
    try {
      g();  
    } catch (e) {
      assertEquals("boom", e)
    }
  }
  assertDoesNotThrow(f);
  assertDoesNotThrow(f);
  %OptimizeFunctionOnNextCall(f);
  assertDoesNotThrow(f);
})();

(function InlinedThrowInFrontOfTry() {
  function g() {
    %DeoptimizeFunction(f);
    throw "boom";
  }
  function f() {
    g();  
    try {
      Math.random();
    } catch (e) {
      assertUnreachable();
    }
  }
  assertThrows(f);
  assertThrows(f);
  %OptimizeFunctionOnNextCall(f);
  assertThrows(f);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93c6595^!)  
[src/builtins/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm/builtins-arm.cc?cl=93c6595)  
[src/builtins/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/arm64/builtins-arm64.cc?cl=93c6595)  
[src/builtins/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins/builtins.h?cl=93c6595)  
[src/builtins/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/ia32/builtins-ia32.cc?cl=93c6595)  
[src/builtins/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips/builtins-mips.cc?cl=93c6595)  
[src/builtins/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/mips64/builtins-mips64.cc?cl=93c6595)  
[src/builtins/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/builtins/x64/builtins-x64.cc?cl=93c6595)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=93c6595)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=93c6595)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=93c6595)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=93c6595)  
[src/runtime/runtime-interpreter.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-interpreter.cc?cl=93c6595)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=93c6595)  
[test/debugger/debugger.status](https://cs.chromium.org/chromium/src/v8/test/debugger/debugger.status?cl=93c6595)  
[test/mjsunit/regress/regress-crbug-660379.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-660379.js?cl=93c6595)  
  

### **crbug:659967**  
   
**[Issue: right->IsUndefined(isolate_) || right->IsBoolean() in ic-state.cc](https://crbug.com/659967)**  
**[Commit: [ic] Properly deal with all oddballs when updating BinaryOpIC state.](https://chromium.googlesource.com/v8/v8/+/305948f)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-659967.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659967.js)  
```javascript
function f() { null >> arguments; }

f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/305948f^!)  
[src/ic/ic-state.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic-state.cc?cl=305948f)  
[test/mjsunit/regress/regress-crbug-659967.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659967.js?cl=305948f)  
  

### **crbug:659915**  
   
**[Issue: Bug with let variable in Chrome](https://crbug.com/659915)**  
**[Commit: [turbofan] Disable usage of {maybe_assigned} variable flag.](https://chromium.googlesource.com/v8/v8/+/b02e7fb)**  
  
Closed: Nov 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["M-54", "Arch-All"]  
Regress: [mjsunit/regress/regress-crbug-659915a.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659915a.js), [mjsunit/regress/regress-crbug-659915b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659915b.js)  
```javascript
let x;
function f(a) {
  x += a;
}
function g(a) {
  f(a); return x;
}
function h(a) {
  x = a; return x;
}

function boom() { return g(1) }

assertEquals(1, h(1));
assertEquals(2, boom());
assertEquals(3, boom());
%OptimizeFunctionOnNextCall(boom);
assertEquals(4, boom());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b02e7fb^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=b02e7fb)  
[test/mjsunit/regress/regress-crbug-659915a.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659915a.js?cl=b02e7fb)  
[test/mjsunit/regress/regress-crbug-659915b.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659915b.js?cl=b02e7fb)  
  

### **crbug:659475**  
   
**[Issue: Pwn2Own: V8 OOB Bug.](https://crbug.com/659475)**  
**[Commit: [compiler] Properly validate stable map assumption for globals.](https://chromium.googlesource.com/v8/v8/+/3aa57eb)**  
  
Closed: Oct 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Memory-AddressSanitizer", "Security_Impact-Stable", "Hotlist-Merge-Approved", "Security_Severity-High", "allpublic", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Release-2-M54", "CVE-2016-5198", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-659475-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659475-1.js), [mjsunit/regress/regress-crbug-659475-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-659475-2.js)  
```javascript
var n;

function Ctor() {
  n = new Set();
}

function Check() {
  n.xyz = 0x826852f4;
}

Ctor();
Ctor();
%OptimizeFunctionOnNextCall(Ctor);
Ctor();

Check();
Check();
%OptimizeFunctionOnNextCall(Check);
Check();

Ctor();
Check();

parseInt('AAAAAAAA');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3aa57eb^!)  
[src/compiler/js-global-object-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-global-object-specialization.cc?cl=3aa57eb)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=3aa57eb)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3aa57eb)  
[src/runtime/runtime-utils.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-utils.h?cl=3aa57eb)  
[test/mjsunit/regress/regress-crbug-659475-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659475-1.js?cl=3aa57eb)  
[test/mjsunit/regress/regress-crbug-659475-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-659475-2.js?cl=3aa57eb)  
  

### **crbug:658691**  
   
**[Issue: Stack-overflow in v8::internal::Invoke](https://crbug.com/658691)**  
**[Commit: [turbofan] Disable bogus lowering of builtin tail-calls.](https://chromium.googlesource.com/v8/v8/+/2ab2ec2)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-658691.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658691.js)  
```javascript
function f(a, b, c) {
  "use strict";
  return Reflect.set({});
}




function g() {
  return f() + "-no-tail";
}

assertEquals("true-no-tail", g());
%OptimizeFunctionOnNextCall(f);
assertEquals("true-no-tail", g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ab2ec2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=2ab2ec2)  
[test/mjsunit/regress/regress-crbug-658691.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658691.js?cl=2ab2ec2)  
  

### **crbug:658528**  
   
**[Issue: variable->is_this() && variable->mode() == CONST && op == Token::INIT in bytecod](https://crbug.com/658528)**  
**[Commit: [ignition] Use more-targeted check for CONST-this-initialization hole check](https://chromium.googlesource.com/v8/v8/+/56626f3)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-658528.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658528.js)  
```javascript
function f() {
  eval("var x = 1");
  const x = 2;
}

assertThrows(f, SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56626f3^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=56626f3)  
[test/mjsunit/regress/regress-crbug-658528.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658528.js?cl=56626f3)  
  

### **crbug:658185**  
   
**[Issue: !escape_analysis_->IsVirtual(node) in escape-analysis-reducer.cc](https://crbug.com/658185)**  
**[Commit: [turbofan] Properly deal with out-of-bounds fields in EscapeAnalysis.](https://chromium.googlesource.com/v8/v8/+/7201bad)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-658185.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-658185.js)  
```javascript
var t = 0;
function foo() {
  var o = {x:1};
  var p = {y:2.5, x:0};
  o = [];
  for (var i = 0; i < 2; ++i) {
    t = o.x;
    o = p;
  }
}
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7201bad^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=7201bad)  
[test/mjsunit/regress/regress-crbug-658185.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-658185.js?cl=7201bad)  
  

### **crbug:657478**  
   
**[Issue: Phi of kRepFloat64 ((Unsigned32 | Negative31)) cannot be changed to kRepWord32 i](https://crbug.com/657478)**  
**[Commit: [turbofan] Fix typed lowering of JSToLength.](https://chromium.googlesource.com/v8/v8/+/a58d790)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.5"]  
Regress: [mjsunit/regress/regress-crbug-657478.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-657478.js)  
```javascript
function foo(o) { return %_ToLength(o.length); }

foo(new Array(4));
foo(new Array(Math.pow(2, 32) - 1));
foo({length: 10});
%OptimizeFunctionOnNextCall(foo);
foo({length: 10});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a58d790^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=a58d790)  
[test/mjsunit/regress/regress-crbug-657478.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-657478.js?cl=a58d790)  
  

### **crbug:656275**  
   
**[Issue: NumberFround of kRepFloat32 (Number) cannot be changed to kRepTaggedSigned in re](https://crbug.com/656275)**  
**[Commit: [turbofan] Add missing Float32 -> TaggedSigned conversion.](https://chromium.googlesource.com/v8/v8/+/96f1327)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-656275.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-656275.js)  
```javascript
var a = 1;

function foo(x) { a = Math.fround(x + 1); }

foo(1);
foo(1);
%OptimizeFunctionOnNextCall(foo);
foo(1.3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96f1327^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=96f1327)  
[test/mjsunit/regress/regress-crbug-656275.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-656275.js?cl=96f1327)  
  

### **crbug:656037**  
   
**[Issue: Array.push sometimes returns the wrong thing](https://crbug.com/656037)**  
**[Commit: [turbofan] Fix return value of Array.prototype.push.](https://chromium.googlesource.com/v8/v8/+/8584442)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Via-Wizard", "Needs-Feedback", "M-54", "Hotlist-Merge-Approved", "Arch-All", "merge-merged-5.4", "Merge-Merged-54", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup", "Merge-Merged-55", "pre-stable-54.0.2840.59"]  
Regress: [mjsunit/regress/regress-crbug-656037.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-656037.js)  
```javascript
function foo(a) {
  return a.push(true);
}

var a = [];
assertEquals(1, foo(a));
assertEquals(2, foo(a));
%OptimizeFunctionOnNextCall(foo);
assertEquals(3, foo(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8584442^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=8584442)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=8584442)  
[test/mjsunit/regress/regress-crbug-656037.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-656037.js?cl=8584442)  
  

### **crbug:655004**  
   
**[Issue: map == GetHeap()->fixed_array_map() || map == GetHeap()->fixed_cow_array_map() i](https://crbug.com/655004)**  
**[Commit: [turbofan] Fix effect chain for polymorphic array access.](https://chromium.googlesource.com/v8/v8/+/edfe391)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "findit-wrong", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup"]  
Regress: [mjsunit/regress/regress-crbug-655004.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-655004.js)  
```javascript
function foo(a) {
  a.x = 0;
  if (a.x === 0) a[1] = 0.1;
  a.x = {};
}
foo(new Array(1));
foo(new Array(1));
%OptimizeFunctionOnNextCall(foo);
foo(new Array(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/edfe391^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=edfe391)  
[test/mjsunit/regress/regress-crbug-655004.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-655004.js?cl=edfe391)  
  

### **crbug:654723**  
   
**[Issue: this->first()->IsSeqString() || this->first()->IsExternalString() in objects-deb](https://crbug.com/654723)**  
**[Commit: [turbofan] Respect ConsString invariant.](https://chromium.googlesource.com/v8/v8/+/a4f37da)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Hotlist-Merge-Approved", "Clusterfuzz", "ClusterFuzz-Verified", "merge-merged-5.5"]  
Regress: [mjsunit/regress/regress-crbug-654723.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-654723.js)  
```javascript
var k = "0101010101010101" + "01010101";

function foo(s) {
  return k + s;
}

foo("a");
foo("a");
%OptimizeFunctionOnNextCall(foo);
var x = foo("");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4f37da^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=a4f37da)  
[test/mjsunit/regress/regress-crbug-654723.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-654723.js?cl=a4f37da)  
  

### **crbug:652186**  
   
**[Issue: No Permission](https://crbug.com/652186)**  
**[Commit: [ignition] Fix building lookup graph when search depth is 0](https://chromium.googlesource.com/v8/v8/+/4ad3579)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-652186-global.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-652186-global.js), [mjsunit/regress/regress-crbug-652186-local.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-652186-local.js)  
```javascript
x = 1;
print(eval("eval('var x = 2'); x;"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ad3579^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=4ad3579)  
[test/mjsunit/regress/regress-crbug-652186-global.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-652186-global.js?cl=4ad3579)  
[test/mjsunit/regress/regress-crbug-652186-local.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-652186-local.js?cl=4ad3579)  
  

### **crbug:651403**  
   
**[Issue: Unreachable code in verifier.cc](https://crbug.com/651403)**  
**[Commit: [ignition] BytecodeGraphBuilder: Merge correct environment in try block](https://chromium.googlesource.com/v8/v8/+/537c855)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-651403-global.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-651403-global.js), [mjsunit/regress/regress-crbug-651403.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-651403.js)  
```javascript
x = "";

function f () {
  function g() {
    try {
      eval('');
      return x;
    } catch(e) {
    }
  }
  return g();
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/537c855^!)  
[src/compiler/bytecode-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/bytecode-graph-builder.cc?cl=537c855)  
[test/mjsunit/regress/regress-crbug-651403-global.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-651403-global.js?cl=537c855)  
[test/mjsunit/regress/regress-crbug-651403.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-651403.js?cl=537c855)  
  

### **crbug:650973**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/650973)**  
**[Commit: [ic][mips][mips64] Ensure store handlers return value in proper register.](https://chromium.googlesource.com/v8/v8/+/8d8c134)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-650973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650973.js)  
```javascript
var v = {p:0};

v.__defineGetter__("p", function() { return 13; });

function f() {
  var boom = (v.foo = v);
  assertEquals(v, boom.foo);
}

f();
f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d8c134^!)  
[src/ic/mips/handler-compiler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/handler-compiler-mips.cc?cl=8d8c134)  
[src/ic/mips/ic-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/ic-mips.cc?cl=8d8c134)  
[src/ic/mips64/handler-compiler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/handler-compiler-mips64.cc?cl=8d8c134)  
[src/ic/mips64/ic-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/ic-mips64.cc?cl=8d8c134)  
[test/mjsunit/regress/regress-crbug-650973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650973.js?cl=8d8c134)  
  

### **crbug:650933**  
   
**[Issue: length_obj->IsSmi() in runtime-typedarray.cc](https://crbug.com/650933)**  
**[Commit: [typedarray] Properly initialize JSTypedArray::length with Smi.](https://chromium.googlesource.com/v8/v8/+/15a449b)**  
  
Closed: Sep 2016  
Type: Bug-Regression  
Components: Blink>JavaScript>Runtime  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "M-55", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-650933.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650933.js)  
```javascript
var a = [0, 1, 2, 3, 4, 5, 6, 7, 8];
var o = {length: 1e40};
try { new Uint8Array(o); } catch (e) { }
new Float64Array(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15a449b^!)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=15a449b)  
[test/mjsunit/regress/regress-crbug-650933.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650933.js?cl=15a449b)  
  

### **crbug:650404**  
   
**[Issue: Security: OOB read/write in V8 using TypedArrays+Crankshaft+Turbofan](https://crbug.com/650404)**  
**[Commit: [crankshaft] TypedArrayInitialize: force length to be a Smi](https://chromium.googlesource.com/v8/v8/+/142f9df)**  
  
Closed: Sep 2016  
Type: Bug-Security  
Components: Blink>JavaScript>Compiler  
Labels: ["Security_Impact-Head", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-650404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-650404.js)  
```javascript
function c4(w, h) {
  var size = w * h;
  if (size < 0) size = 0;
  return new Uint32Array(size);
}

for (var i = 0; i < 3; i++) {
  
  
  c4(0, -1);
}

for (var i = 0; i < 1000; i++) c4(2, 2);


var bomb = c4(2, 2);

function reader(o, i) {
  
  try {} catch(e) {}
  return o[i];
}

for (var i = 0; i < 3; i++) reader(bomb, 0);
%OptimizeFunctionOnNextCall(reader);
reader(bomb, 0);

for (var i = bomb.length; i < 100; i++) {
  assertEquals(undefined, reader(bomb, i));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/142f9df^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=142f9df)  
[test/mjsunit/regress/regress-crbug-650404.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-650404.js?cl=142f9df)  
  

### **crbug:648740**  
   
**[Issue: arguments() != nullptr == migrate_to->arguments() != nullptr in scopes.cc](https://crbug.com/648740)**  
**[Commit: Add regression test for crbug.com/648740.](https://chromium.googlesource.com/v8/v8/+/68ee0a4)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-648740.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648740.js)  
```javascript
(function () {
  function foo() {
    const arguments = 42;
  }
})()  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68ee0a4^!)  
[test/mjsunit/regress/regress-crbug-648740.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-648740.js?cl=68ee0a4)  
  

### **crbug:648737**  
   
**[Issue: JSFunction::kCodeEntryOffset == FieldAccessOf(node->op()).offset in escape-analy](https://crbug.com/648737)**  
**[Commit: [turbofan] Support for ConsString by escape analysis.](https://chromium.googlesource.com/v8/v8/+/b097c6c)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-648737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648737.js)  
```javascript
function f(str) {
  var s = "We turn {" + str + "} into a ConsString now";
  return s.length;
}
assertEquals(33, f("a"));
assertEquals(33, f("b"));
%OptimizeFunctionOnNextCall(f);
assertEquals(33, f("c"));

function g(str) {
  var s = "We also try to materalize {" + str + "} when deopting";
  %DeoptimizeNow();
  return s.length;
}
assertEquals(43, g("a"));
assertEquals(43, g("b"));
%OptimizeFunctionOnNextCall(g);
assertEquals(43, g("c"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b097c6c^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=b097c6c)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=b097c6c)  
[test/mjsunit/regress/regress-crbug-648737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-648737.js?cl=b097c6c)  
  

### **crbug:648539**  
   
**[Issue: Crash in v8::internal::Factory::NewTypeError](https://crbug.com/648539)**  
**[Commit: [turbofan] Remove bogus constant materialization from frame.](https://chromium.googlesource.com/v8/v8/+/81f4342)**  
  
Closed: Sep 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-54", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.4", "Merge-Merged-54"]  
Regress: [mjsunit/regress/regress-crbug-648539.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-648539.js)  
```javascript
function f() {
  "use strict";
  return undefined(0, 0);
}
function g() {
  return f();
}
assertThrows(g, TypeError);
assertThrows(g, TypeError);
%OptimizeFunctionOnNextCall(g);
assertThrows(g, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/81f4342^!)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=81f4342)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=81f4342)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=81f4342)  
[src/compiler/code-generator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.h?cl=81f4342)  
[src/compiler/ia32/code-generator-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/code-generator-ia32.cc?cl=81f4342)  
[src/compiler/mips/code-generator-mips.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips/code-generator-mips.cc?cl=81f4342)  
[src/compiler/mips64/code-generator-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/mips64/code-generator-mips64.cc?cl=81f4342)  
[src/compiler/ppc/code-generator-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ppc/code-generator-ppc.cc?cl=81f4342)  
[src/compiler/s390/code-generator-s390.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/s390/code-generator-s390.cc?cl=81f4342)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=81f4342)  
[src/compiler/x87/code-generator-x87.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x87/code-generator-x87.cc?cl=81f4342)  
[test/mjsunit/regress/regress-crbug-648539.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-648539.js?cl=81f4342)  
  

### **crbug:647887**  
   
**[Issue: Iterating object properties in a function that has a try...catch produces incorrect result](https://crbug.com/647887)**  
**[Commit: [turbofan] Fix loop assignment analysis on ForInStatements.](https://chromium.googlesource.com/v8/v8/+/4dab7b5)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Hotlist-Merge-Review", "Via-Wizard", "M-53", "M-54", "Hotlist-Merge-Approved", "Arch-All", "ReleaseBlock-Stable", "merge-merged-5.3", "merge-merged-5.4", "TE-Verified-M55", "Merge-Merged-54", "TE-Verified-55.0.2868.0", "NodeJS-Backport-Rejected"]  
Regress: [mjsunit/regress/regress-crbug-647887.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-647887.js)  
```javascript
function f(obj) {
  var key;
  for (key in obj) { }
  return key === undefined;
}

%OptimizeFunctionOnNextCall(f);
assertFalse(f({ foo:0 }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4dab7b5^!)  
[src/compiler/ast-loop-assignment-analyzer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-loop-assignment-analyzer.cc?cl=4dab7b5)  
[test/mjsunit/regress/regress-crbug-647887.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-647887.js?cl=4dab7b5)  
  

### **crbug:647217**  
   
**[Issue: !info_->isolate()->has_pending_exception() in js-inlining.cc](https://crbug.com/647217)**  
**[Commit: [turbofan] Handle stack overflow during inlining.](https://chromium.googlesource.com/v8/v8/+/c2cf8b1)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-647217.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-647217.js)  
```javascript
var source = "return 1" + new Array(2048).join(' + a') + "";
eval("function g(a) {" + source + "}");

function f(a) { return g(a) }
%OptimizeFunctionOnNextCall(f);
try { f(0) } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c2cf8b1^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c2cf8b1)  
[test/mjsunit/regress/regress-crbug-647217.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-647217.js?cl=c2cf8b1)  
  

### **crbug:645888**  
   
**[Issue: IrOpcode::kLoop == GetControlDependency()->opcode() in bytecode-graph-builder.cc](https://crbug.com/645888)**  
**[Commit: [interpreter] Add regression test for bogus OSR entry.](https://chromium.googlesource.com/v8/v8/+/8528974)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-645888.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645888.js)  
```javascript
function f() {
  for (var i = 0; i < 3; ++i) {
    if (i == 1) {
      %OptimizeOsr();
      break;  
    }
  }
  while (true) {
    throw "no loop, thank you";
  }
}
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8528974^!)  
[test/mjsunit/regress/regress-crbug-645888.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645888.js?cl=8528974)  
  

### **crbug:645438**  
   
**[Issue: Security: js input crashes Chrome on FPE; seemingly non-deterministic in the v8 shell?](https://crbug.com/645438)**  
**[Commit: [crankshaft] Range analysis should not rely on overflowed ranges.](https://chromium.googlesource.com/v8/v8/+/9a0109d)**  
  
Closed: Oct 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "M-55", "M-53", "M-54", "Hotlist-Merge-Approved", "allpublic", "merge-merged-5.4", "NodeJS-Backport-Done", "merge-merged-5.5", "Postmortem-Followup"]  
Regress: [mjsunit/regress/regress-crbug-645438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645438.js)  
```javascript
function n(x,y){
  y = (y-(0x80000000|0)|0);
  return (x/y)|0;
};
var x = -0x80000000;
var y = 0x7fffffff;
n(x,y);
n(x,y);
%OptimizeFunctionOnNextCall(n);
assertEquals(x, n(x,y));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a0109d^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=9a0109d)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=9a0109d)  
[test/mjsunit/regress/regress-crbug-645438.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645438.js?cl=9a0109d)  
  

### **crbug:645103**  
   
**[Issue: args[1]->IsJSReceiver() in runtime-object.cc](https://crbug.com/645103)**  
**[Commit: [interpreter] Fix destroyed new.target register use.](https://chromium.googlesource.com/v8/v8/+/0681deb)**  
  
Closed: Sep 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-645103.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-645103.js)  
```javascript
class Base {}
class Subclass extends Base {
  constructor() {
    %DeoptimizeNow();
    super();
  }
}
new Subclass();
new Subclass();
%OptimizeFunctionOnNextCall(Subclass);
new Subclass();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0681deb^!)  
[src/interpreter/bytecode-generator.cc](https://cs.chromium.org/chromium/src/v8/src/interpreter/bytecode-generator.cc?cl=0681deb)  
[test/cctest/interpreter/bytecode_expectations/ClassAndSuperClass.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/ClassAndSuperClass.golden?cl=0681deb)  
[test/cctest/interpreter/bytecode_expectations/NewTarget.golden](https://cs.chromium.org/chromium/src/v8/test/cctest/interpreter/bytecode_expectations/NewTarget.golden?cl=0681deb)  
[test/mjsunit/regress/regress-crbug-645103.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-645103.js?cl=0681deb)  
  

### **crbug:644689**  
   
**[Issue: map->is_stable() in compilation-dependencies.cc](https://crbug.com/644689)**  
**[Commit: [turbofan] Ensure that all prototypes are stable for push/pop.](https://chromium.googlesource.com/v8/v8/+/4ed27fc)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.4", "Postmortem-Followup"]  
Regress: [mjsunit/regress/regress-crbug-644689-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644689-1.js), [mjsunit/regress/regress-crbug-644689-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644689-2.js)  
```javascript
for (var i = 0; i < 1024; ++i) Object.prototype["i" + i] = i;

function foo() { [].push(1); }

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ed27fc^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=4ed27fc)  
[test/mjsunit/regress/regress-crbug-644689-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644689-1.js?cl=4ed27fc)  
[test/mjsunit/regress/regress-crbug-644689-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644689-2.js?cl=4ed27fc)  
  

### **crbug:644631**  
   
**[Issue: Inline calls to runtime functions in TurboFan that throw exceptions result in missing frame state](https://crbug.com/644631)**  
**[Commit: [turbofan] Switch from a whitelist to a blacklist for NeedsFrameStateInput](https://chromium.googlesource.com/v8/v8/+/58325e6)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-644631.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644631.js)  
```javascript
function f() {
  var obj = Object.freeze({});
  %_CreateDataProperty(obj, "foo", "bar");
}


assertThrows(f, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58325e6^!)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=58325e6)  
[test/mjsunit/regress/regress-crbug-644631.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644631.js?cl=58325e6)  
  

### **crbug:644245**  
   
**[Issue: isolate->context() == nullptr || isolate->context()->IsContext() in runtime-comp](https://crbug.com/644245)**  
**[Commit: [deoptimizer] Support materialization of ContextExtension.](https://chromium.googlesource.com/v8/v8/+/9984d6f)**  
  
Closed: Sep 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-644245.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644245.js)  
```javascript
function f() {
  try {
    throw "boom";
  } catch(e) {
    %_DeoptimizeNow();
  }
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9984d6f^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=9984d6f)  
[test/mjsunit/regress/regress-crbug-644245.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644245.js?cl=9984d6f)  
  

### **crbug:644215**  
   
**[Issue: Security: ILL_ILLOPN/Segfault; crashes Chrome Dev/Beta/Stable](https://crbug.com/644215)**  
**[Commit: Properly handle holes following spreads in array literals](https://chromium.googlesource.com/v8/v8/+/e427300)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Security_Impact-Stable", "M-54", "allpublic", "merge-merged-5.4", "Merge-Merged-54", "Release-0-M54"]  
Regress: [mjsunit/regress/regress-crbug-644215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644215.js)  
```javascript
var arr = [...[],,];
assertTrue(%HasHoleyElements(arr));
assertEquals(1, arr.length);
assertFalse(arr.hasOwnProperty(0));
assertEquals(undefined, arr[0]);

assertThrows(() => arr[0][0], TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e427300^!)  
[src/ast/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast-value-factory.h?cl=e427300)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=e427300)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=e427300)  
[test/mjsunit/regress/regress-crbug-644215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644215.js?cl=e427300)  
  

### **crbug:644111**  
   
**[Issue: info->shared_info()->HasBytecodeArray() in compiler.cc](https://crbug.com/644111)**  
**[Commit: [compiler] Bytecode preparation fails for asm.js modules.](https://chromium.googlesource.com/v8/v8/+/cc1249b)**  
  
Closed: Sep 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-644111.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-644111.js)  
```javascript
function Module() {
  "use asm";
  return {};
}
var m = Module();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc1249b^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=cc1249b)  
[test/mjsunit/regress/regress-crbug-644111.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-644111.js?cl=cc1249b)  
  

### **crbug:643073**  
   
**[Issue: TypeGuard of kRepWord32 (None) cannot be changed to kRepBit in representation-ch](https://crbug.com/643073)**  
**[Commit: [turbofan] Only check semantic axis for Type::None.](https://chromium.googlesource.com/v8/v8/+/432790c)**  
  
Closed: Sep 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-643073.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-643073.js)  
```javascript
for (i in [0,0]) {}
function foo() {
  i = 0;
  return i < 0;
}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/432790c^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=432790c)  
[test/mjsunit/regress/regress-crbug-643073.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-643073.js?cl=432790c)  
  

### **crbug:642056**  
   
**[Issue: No Permission](https://crbug.com/642056)**  
**[Commit: [test] Add regression test for http://crbug.com/642056.](https://chromium.googlesource.com/v8/v8/+/86af343)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-642056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-642056.js)  
```javascript
function f(o) {
  return o.x instanceof Array;
}

var o = { x : 1.5 };
o.x = 0;

f(o);
f(o);
%OptimizeFunctionOnNextCall(f);
f(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/86af343^!)  
[test/mjsunit/regress/regress-crbug-642056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-642056.js?cl=86af343)  
  

### **crbug:640497**  
   
**[Issue: HeapConstant of kRepTagged (Constant(1CNUMBER <FixedArray[0]>)) cannot be change](https://crbug.com/640497)**  
**[Commit: [turbofan] Add proper type guards to escape analysis.](https://chromium.googlesource.com/v8/v8/+/4b2c6d0)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "M-54", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-640497.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-640497.js)  
```javascript
function g(v) { return v.length; }
assertEquals(1, g("x"));
assertEquals(2, g("xy"));
assertEquals(1, g([1]));
assertEquals(2, g([1,2]));


function f() { assertEquals(0, g([])); }
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b2c6d0^!)  
[src/compiler/escape-analysis-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis-reducer.cc?cl=4b2c6d0)  
[test/mjsunit/regress/regress-crbug-640497.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-640497.js?cl=4b2c6d0)  
[test/unittests/compiler/escape-analysis-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/escape-analysis-unittest.cc?cl=4b2c6d0)  
  

### **crbug:640369**  
   
**[Issue: IrOpcode::kLoop == loop->opcode() in verifier.cc](https://crbug.com/640369)**  
**[Commit: [turbofan] Use ObjectIsReceiver directly for inlining.](https://chromium.googlesource.com/v8/v8/+/6646d73)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-640369.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-640369.js)  
```javascript
function A() {
  this.x = 0;
  for (var i = 0; i < max; ) {}
}
function foo() {
  for (var i = 0; i < 1; i = 2) %OptimizeOsr();
  return new A();
}
try { foo(); } catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6646d73^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=6646d73)  
[src/compiler/js-inlining.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.h?cl=6646d73)  
[test/mjsunit/regress/regress-crbug-640369.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-640369.js?cl=6646d73)  
  

### **crbug:638551**  
   
**[Issue: !shared->HasBytecodeArray() in compiler.cc](https://crbug.com/638551)**  
**[Commit: [interpreter] Fix canonicalization when preserving bytecode.](https://chromium.googlesource.com/v8/v8/+/8ab555c)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-638551.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-638551.js)  
```javascript
function f() {
  for (var i = 0; i < 10; i++) if (i == 5) %OptimizeOsr();
  function g() {}
  %OptimizeFunctionOnNextCall(g);
  g();
}
f();
gc();  
gc();  
gc();  
gc();  
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ab555c^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=8ab555c)  
[test/mjsunit/regress/regress-crbug-638551.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-638551.js?cl=8ab555c)  
  

### **crbug:635923**  
   
**[Issue: !info->shared_info()->is_compiled() in compiler.cc](https://crbug.com/635923)**  
**[Commit: [compiler] Make Compiler::EnsureBytecode not switch tiers.](https://chromium.googlesource.com/v8/v8/+/b52aeca)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified", "Proj-Ignition"]  
Regress: [mjsunit/regress/regress-crbug-635923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-635923.js)  
```javascript
function f(x) { return x + 23 }
function g(x) { return f(x) + 42 }

assertEquals(23, f(0));
assertEquals(24, f(1));
assertEquals(67, g(2));
assertEquals(68, g(3));


%OptimizeFunctionOnNextCall(g);
assertEquals(65, g(0));


%OptimizeFunctionOnNextCall(f);
assertEquals(23, f(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b52aeca^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=b52aeca)  
[test/mjsunit/regress/regress-crbug-635923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-635923.js?cl=b52aeca)  
  

### **crbug:635798**  
   
**[Issue: Crash in v8::internal::Context::native_context](https://crbug.com/635798)**  
**[Commit: [runtime] %GrowArrayElements doesn't have a native context in TurboFan.](https://chromium.googlesource.com/v8/v8/+/78727d4)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "findit-wrong", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-635798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-635798.js)  
```javascript
function foo() {
  var x = [];
  var y = [];
  x.__proto__ = y;
  for (var i = 0; i < 10000; ++i) {
    y[i] = 1;
  }
}
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78727d4^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=78727d4)  
[test/mjsunit/regress/regress-crbug-635798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-635798.js?cl=78727d4)  
  

### **crbug:633884**  
   
**[Issue: !value->IsTheHole(isolate) in runtime-scopes.cc](https://crbug.com/633884)**  
**[Commit: Properly pass InitializationFlag back from ScriptContextTable lookups](https://chromium.googlesource.com/v8/v8/+/e6d2c9b)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-633884.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-633884.js)  
```javascript
try {
  
  Realm.eval(Realm.current(), "throw Error(); let blarg");
} catch (e) { }


assertThrows(function() {
  
  eval("var x = 5");
  blarg;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6d2c9b^!)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=e6d2c9b)  
[test/mjsunit/regress/regress-crbug-633884.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-633884.js?cl=e6d2c9b)  
  

### **crbug:633585**  
   
**[Issue: Crash in v8::internal::InnerPointerToCodeCache::GcSafeFindCodeForInnerPointer](https://crbug.com/633585)**  
**[Commit: [turbofan] Fix missing bailout for accessors in literals.](https://chromium.googlesource.com/v8/v8/+/667d8ad)**  
  
Closed: Aug 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-53", "Security_Impact-Stable", "Security_Severity-Medium", "Hotlist-Merge-Approved", "allpublic", "Clusterfuzz", "Release-0-M53", "merge-merged-5.3", "Merge-Merged-53", "CVE-2016-5167", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-633585.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-633585.js)  
```javascript
function f() { this.x = this.x.x; }
gc();
f.prototype.x = { x:1 }
new f();
new f();

function g() {
  function h() {};
  h.prototype = { set x(value) { } };
  new f();
}
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/667d8ad^!)  
[src/ast/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast/ast.h?cl=667d8ad)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=667d8ad)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=667d8ad)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=667d8ad)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=667d8ad)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=667d8ad)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=667d8ad)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=667d8ad)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=667d8ad)  
[src/full-codegen/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x64/full-codegen-x64.cc?cl=667d8ad)  
[src/full-codegen/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x87/full-codegen-x87.cc?cl=667d8ad)  
[test/mjsunit/regress/regress-crbug-633585.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-633585.js?cl=667d8ad)  
  

### **crbug:632800**  
   
**[Issue: next_tier == Compiler::OPTIMIZED in runtime-profiler.cc](https://crbug.com/632800)**  
**[Commit: [interpreter] Fix profiler when hitting OSR frame.](https://chromium.googlesource.com/v8/v8/+/f00b42a)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-632800.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-632800.js)  
```javascript
function osr() {
  for (var i = 0; i < 50000; ++i) Math.random();
}
osr();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f00b42a^!)  
[src/runtime-profiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime-profiler.cc?cl=f00b42a)  
[src/runtime-profiler.h](https://cs.chromium.org/chromium/src/v8/src/runtime-profiler.h?cl=f00b42a)  
[test/mjsunit/regress/regress-crbug-632800.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-632800.js?cl=f00b42a)  
  

### **crbug:631917**  
   
**[Issue: args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/631917)**  
**[Commit: [fullcode][mips][mips64][ppc][s390] Avoid trashing of a home object when doing a count operation with keyed load/store to a super.](https://chromium.googlesource.com/v8/v8/+/fc66694)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "findit-wrong", "M-54", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-631917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631917.js)  
```javascript
var b = { toString: function() { return "b"; } };
var c = { toString: function() { return "c"; } };

(function() {
  var expected_receiver;
  var obj1 = {
    a: 100,
    b_: 200,
    get b() { assertEquals(expected_receiver, this); return this.b_; },
    set b(v) { assertEquals(expected_receiver, this); this.b_ = v; },
    c_: 300,
    get c() { assertEquals(expected_receiver, this); return this.c_; },
    set c(v) { assertEquals(expected_receiver, this); this.c_ = v; },
  };
  var obj2 = {
    boom() {
      super.a++;
      super[b]++;
      super[c]++;
    },
  }
  Object.setPrototypeOf(obj2, obj1);

  expected_receiver = obj2;
  obj2.boom();
  assertEquals(101, obj2.a);
  assertEquals(201, obj2[b]);
  assertEquals(301, obj2[c]);

  expected_receiver = obj1;
  assertEquals(100, obj1.a);
  assertEquals(200, obj1[b]);
  assertEquals(300, obj1[c]);
}());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc66694^!)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=fc66694)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=fc66694)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=fc66694)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=fc66694)  
[test/mjsunit/regress/regress-crbug-631917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631917.js?cl=fc66694)  
  

### **crbug:631318**  
   
**[Issue: Dead-code elimination during representation selection is too aggressive](https://crbug.com/631318)**  
**[Commit: [turbofan] Fix overly aggressive dead code elimination.](https://chromium.googlesource.com/v8/v8/+/32346aa)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Arch-All"]  
Regress: [mjsunit/regress/regress-crbug-631318-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-1.js), [mjsunit/regress/regress-crbug-631318-10.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-10.js), [mjsunit/regress/regress-crbug-631318-11.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-11.js), [mjsunit/regress/regress-crbug-631318-12.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-12.js), [mjsunit/regress/regress-crbug-631318-13.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-13.js), [mjsunit/regress/regress-crbug-631318-14.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-14.js), [mjsunit/regress/regress-crbug-631318-15.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-15.js), [mjsunit/regress/regress-crbug-631318-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-2.js), [mjsunit/regress/regress-crbug-631318-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-3.js), [mjsunit/regress/regress-crbug-631318-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-4.js), [mjsunit/regress/regress-crbug-631318-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-5.js), [mjsunit/regress/regress-crbug-631318-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-6.js), [mjsunit/regress/regress-crbug-631318-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-7.js), [mjsunit/regress/regress-crbug-631318-8.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-8.js), [mjsunit/regress/regress-crbug-631318-9.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631318-9.js)  
```javascript
function foo(x) { return x < x; }
foo(1);
foo(2);

function bar(x) { foo(x); }
%OptimizeFunctionOnNextCall(bar);

assertThrows(() => bar(Symbol.toPrimitive));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/32346aa^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=32346aa)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-1.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-10.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-10.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-11.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-11.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-12.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-12.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-13.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-13.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-14.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-14.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-15.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-15.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-2.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-3.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-4.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-5.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-5.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-6.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-6.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-7.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-7.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-8.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-8.js?cl=32346aa)  
[test/mjsunit/regress/regress-crbug-631318-9.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631318-9.js?cl=32346aa)  
  

### **crbug:631027**  
   
**[Issue: Unreachable code in escape-analysis.cc](https://crbug.com/631027)**  
**[Commit: [turbofan] Handle ObjectIsReceiver in escape analysis.](https://chromium.googlesource.com/v8/v8/+/553d504)**  
  
Closed: Sep 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-631027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-631027.js)  
```javascript
function f() {
  with ({ value:"foo" }) { return value; }
}
assertEquals("foo", f());
%OptimizeFunctionOnNextCall(f);
assertEquals("foo", f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/553d504^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=553d504)  
[test/mjsunit/regress/regress-crbug-631027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-631027.js?cl=553d504)  
  

### **crbug:630952**  
   
**[Issue: IsUseLessGeneral(input_use_infos_[index], use_info) in simplified-lowering.cc](https://crbug.com/630952)**  
**[Commit: [Turbofan] IsUseLessGeneral shouldn't consider machine representation.](https://chromium.googlesource.com/v8/v8/+/480f155)**  
  
Closed: Jul 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-630952.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630952.js)  
```javascript
try {
function __f_4(sign_bit,
                          mantissa_29_bits) {
}
__f_4.prototype.returnSpecial = function() {
                     this.mantissa_29_bits * mantissa_29_shift;
}
__f_4.prototype.toSingle = function() {
  if (-65535) return this.toSingleSubnormal();
}
__f_4.prototype.toSingleSubnormal = function() {
  if (__v_15) {
      var __v_7 = this.mantissa_29_bits == -1 &&
                 (__v_13 & __v_10 ) == 0;
    }
  __v_8 >>= __v_7;
}
__v_14 = new __f_4();
__v_14.toSingle();
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/480f155^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=480f155)  
[test/mjsunit/regress/regress-crbug-630952.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630952.js?cl=480f155)  
  

### **crbug:630951**  
   
**[Issue: Unreachable code in verifier.cc](https://crbug.com/630951)**  
**[Commit: [turbofan] Avoid introducing machine operators during typed lowering.](https://chromium.googlesource.com/v8/v8/+/5bed151)**  
  
Closed: Jul 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-630951.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630951.js)  
```javascript
function foo() {
  "use asm";
  var o = new Int32Array(64 * 1024);
  return () => { o[i1 >> 2] | 0; }
}
assertThrows(foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bed151^!)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=5bed151)  
[src/compiler/js-intrinsic-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.h?cl=5bed151)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=5bed151)  
[src/compiler/js-typed-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.h?cl=5bed151)  
[test/mjsunit/regress/regress-crbug-630951.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630951.js?cl=5bed151)  
[test/unittests/compiler/js-intrinsic-lowering-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-intrinsic-lowering-unittest.cc?cl=5bed151)  
[test/unittests/compiler/js-typed-lowering-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-typed-lowering-unittest.cc?cl=5bed151)  
  

### **crbug:630923**  
   
**[Issue: 0 == node->op()->ControlInputCount() in simplified-lowering.cc](https://crbug.com/630923)**  
**[Commit: [turbofan] Remove overly restrictive DCHECK.](https://chromium.googlesource.com/v8/v8/+/e3e347b)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-630923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630923.js)  
```javascript
var o = {};
function bar(o) {
  return 1 + (o.t ? 1 : 2);
}
function foo() {
  bar(o);
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3e347b^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=e3e347b)  
[test/mjsunit/regress/regress-crbug-630923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630923.js?cl=e3e347b)  
  

### **crbug:630561**  
   
**[Issue: collector->heap()->Contains(obj) in mark-compact.cc](https://crbug.com/630561)**  
**[Commit: [elements] Omit fast path in PrependElementIndices](https://chromium.googlesource.com/v8/v8/+/7ede61e)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-630561.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630561.js)  
```javascript
var dict_elements = {};

for (var i= 0; i< 100; i++) {
  dict_elements[2147483648 + i] = i;
}

var keys = Object.keys(dict_elements);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ede61e^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=7ede61e)  
[test/mjsunit/regress/regress-crbug-630561.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630561.js?cl=7ede61e)  
  

### **crbug:630559**  
   
**[Issue: Token::CATCH == tok in parser.cc](https://crbug.com/630559)**  
**[Commit: Native try-catch syntax parsing should not crash.](https://chromium.googlesource.com/v8/v8/+/9868142)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-630559.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-630559.js)  
```javascript
assertThrows("try{}%");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9868142^!)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=9868142)  
[test/mjsunit/regress/regress-crbug-630559.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-630559.js?cl=9868142)  
  

### **crbug:629823**  
   
**[Issue: args[0]->IsJSArray() in runtime-array.cc](https://crbug.com/629823)**  
**[Commit: [runtime] %TransitionElementsKind works for any kind of JSObject.](https://chromium.googlesource.com/v8/v8/+/f793cb1)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-629823.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629823.js)  
```javascript
var o = {}
function bar() {
  o[0] = +o[0];
  o = /\u23a1|__v_4/;
}
bar();
bar();
bar();
function foo() { bar(); }
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f793cb1^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=f793cb1)  
[test/mjsunit/regress/regress-crbug-629823.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629823.js?cl=f793cb1)  
  

### **crbug:629435**  
   
**[Issue: Crash in v8::internal::Invoke](https://crbug.com/629435)**  
**[Commit: [turbofan] Fix typing rule for number addition.](https://chromium.googlesource.com/v8/v8/+/e0b8707)**  
  
Closed: Jul 2016  
Type: Bug-Security  
Components: None  
Labels: ["Reproducible", "Stability-Memory-AddressSanitizer", "M-52", "Security", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-629435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629435.js)  
```javascript
function bar(v) {
  v.constructor;
}

bar([]);
bar([]);

function foo() {
  var x = -0;
  bar(x + 1);
}
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0b8707^!)  
[src/compiler/operation-typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operation-typer.cc?cl=e0b8707)  
[test/mjsunit/regress/regress-crbug-629435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629435.js?cl=e0b8707)  
  

### **crbug:629062**  
   
**[Issue: NumberEqual of kRepBit (Constant(ADDRESS <false>)) cannot be changed to kRepFloa](https://crbug.com/629062)**  
**[Commit: [turbofan] Properly handle bit->float64 representation changes.](https://chromium.googlesource.com/v8/v8/+/15f99cd)**  
  
Closed: Jul 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-629062.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-629062.js)  
```javascript
function foo(x) {
  return 1 + ((1 == 0) && undefined);
}

foo(false);
foo(false);
%OptimizeFunctionOnNextCall(foo);
foo(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15f99cd^!)  
[src/compiler/representation-change.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.cc?cl=15f99cd)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=15f99cd)  
[test/mjsunit/regress/regress-crbug-629062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-629062.js?cl=15f99cd)  
  

### **crbug:628573**  
   
**[Issue: Crash in v8::internal::Context::native_context](https://crbug.com/628573)**  
**[Commit: [fullcode] Restore context after calling ToNumber builtin.](https://chromium.googlesource.com/v8/v8/+/5d66a7f)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-628573.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-628573.js)  
```javascript
var z = {valueOf: function() { return 3; }};

(function() {
  try {
    var tmp = { x: 12 };
    with (tmp) {
      z++;
    }
    throw new Error("boom");
  } catch(e) {}
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d66a7f^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=5d66a7f)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=5d66a7f)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=5d66a7f)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=5d66a7f)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=5d66a7f)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=5d66a7f)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=5d66a7f)  
[src/full-codegen/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x64/full-codegen-x64.cc?cl=5d66a7f)  
[src/full-codegen/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x87/full-codegen-x87.cc?cl=5d66a7f)  
[test/mjsunit/regress/regress-crbug-628573.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-628573.js?cl=5d66a7f)  
  

### **crbug:627935**  
   
**[Issue: args[0]->IsString() in runtime-strings.cc](https://crbug.com/627935)**  
**[Commit: [i18n] Ensure [[ToString]] conversion of time zone names.](https://chromium.googlesource.com/v8/v8/+/8226c88)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-627935.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627935.js)  
```javascript
if (this.Intl) {
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: 0})", RangeError);
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: true})", RangeError);
  assertThrows("Intl.DateTimeFormat('en-US', {timeZone: null})", RangeError);

  var object = { toString: function() { return "UTC" } };
  assertDoesNotThrow("Intl.DateTimeFormat('en-US', {timeZone: object})");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8226c88^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=8226c88)  
[test/mjsunit/regress/regress-crbug-627935.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627935.js?cl=8226c88)  
  

### **crbug:627934**  
   
**[Issue: args[1]->IsString() in runtime-strings.cc](https://crbug.com/627934)**  
**[Commit: [stubs] Properly handle length overflow in StringAddStub.](https://chromium.googlesource.com/v8/v8/+/6530a16)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-627934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627934.js)  
```javascript
var x = "1".repeat(32 * 1024 * 1024);
for (var z = x;;) {
  try {
    z += {toString: function() { return x; }};
  } catch (e) {
    break;
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6530a16^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=6530a16)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=6530a16)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=6530a16)  
[test/mjsunit/regress/regress-crbug-627934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627934.js?cl=6530a16)  
  

### **crbug:627828**  
   
**[Issue: args[1]->IsName() in runtime-object.cc](https://crbug.com/627828)**  
**[Commit: [turbofan] Fix deopt point for [[ToName]] lazy bailout.](https://chromium.googlesource.com/v8/v8/+/a2f1519)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-627828.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-627828.js)  
```javascript
(function TestDeoptFromComputedNameInObjectLiteral() {
  function f() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(f);
        return "x";
      }
    };
    return { [o]() { return 23 } };
  }
  assertEquals(23, f().x());
  assertEquals(23, f().x());
  %OptimizeFunctionOnNextCall(f);
  assertEquals(23, f().x());
})();

(function TestDeoptFromComputedNameInObjectLiteralWithModifiedPrototype() {
  
  

  Object.defineProperty(Object.prototype, 'x_proto', {
    get: function () {
      return 21;
    },
    set: function () {
    }
  });


  function f() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(f);
        return "x_proto";
      }
    };
    return { [o]() { return 23 } };
  }
  assertEquals(23, f().x_proto());
  assertEquals(23, f().x_proto());
  %OptimizeFunctionOnNextCall(f);
  assertEquals(23, f().x_proto());

  delete Object.prototype.c;

})();

(function TestDeoptFromComputedNameInClassLiteral() {
  function g() {
    var o = {
      toString: function() {
        %DeoptimizeFunction(g);
        return "y";
      }
    };
    class C {
      [o]() { return 42 };
    }
    return new C();
  }
  assertEquals(42, g().y());
  assertEquals(42, g().y());
  %OptimizeFunctionOnNextCall(g);
  assertEquals(42, g().y());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a2f1519^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=a2f1519)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=a2f1519)  
[test/mjsunit/regress/regress-crbug-627828.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-627828.js?cl=a2f1519)  
  

### **crbug:626715**  
   
**[Issue: No Permission](https://crbug.com/626715)**  
**[Commit: [runtime] Follow-up fix for "Better encapsulation of dictionary objects handling in lookup iterator."](https://chromium.googlesource.com/v8/v8/+/b030a6f)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-626715.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-626715.js)  
```javascript
var body = "";
for (var i = 0; i < 100; i++) {
  body += `this.a${i} = 0;\n`;
}
var Proto = new Function(body);

function A() {}
A.prototype = new Proto();





var o = new A();
for (var i = 0; i < 100; i++) {
  o["a" + i] = i;
}


var names = Object.getOwnPropertyNames(o);
for (var i = 0; i < 100; i++) {
  assertEquals("a" + i, names[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b030a6f^!)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=b030a6f)  
[test/mjsunit/regress/regress-crbug-626715.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-626715.js?cl=b030a6f)  
  

### **crbug:625590**  
   
**[Issue: RUNTIME_ASSERT in args[1]->IsJSObject() in runtime-classes.cc](https://crbug.com/625590)**  
**[Commit: [fullcode][mips][mips64][ppc][s390] Avoid trashing of a home object when doing a keyed store to a super.](https://chromium.googlesource.com/v8/v8/+/43aee03)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-625590.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-625590.js)  
```javascript
var obj = {};
function f() {}
f.prototype = {
  mSloppy() {
    super[obj] = 15;
  }
};
new f().mSloppy();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/43aee03^!)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=43aee03)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=43aee03)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=43aee03)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=43aee03)  
[test/mjsunit/regress/regress-crbug-625590.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-625590.js?cl=43aee03)  
  

### **crbug:625547**  
   
**[Issue: ho->GetHeap()->Contains(ho) in objects-debug.cc](https://crbug.com/625547)**  
**[Commit: [crankshaft] Use canonical nan_value or minus_zero_value objects instead of constant heap numbers with NaN or -0.0 values.](https://chromium.googlesource.com/v8/v8/+/acd674d)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "M-52", "M-53", "Clusterfuzz", "M-51"]  
Regress: [mjsunit/regress/regress-crbug-625547.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-625547.js)  
```javascript
var v1 = {};
v1 = 0;
var v2 = {};
v2 = 0;
gc();

var minus_zero = {z:-0.0}.z;
var nan = undefined + 1;
function f() {
  v1 = minus_zero;
  v2 = nan;
};
%OptimizeFunctionOnNextCall(f);
f();
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/acd674d^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=acd674d)  
[test/mjsunit/regress/regress-crbug-625547.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-625547.js?cl=acd674d)  
  

### **crbug:624919**  
   
**[Issue: V8 Crash - unable to find pc offset during deoptimization](https://crbug.com/624919)**  
**[Commit: [turbofan] Fix eager bailout point after comma expression.](https://chromium.googlesource.com/v8/v8/+/920bc17)**  
  
Closed: Jul 2016  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-624919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-624919.js)  
```javascript
function f(a, b, c, d, e) {
  if (a && (b, c ? d() : e())) return 0;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/920bc17^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=920bc17)  
[test/mjsunit/regress/regress-crbug-624919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-624919.js?cl=920bc17)  
  

### **crbug:624747**  
   
**[Issue: IrOpcode::kFrameState == state->op()->opcode() in instruction-selector.cc](https://crbug.com/624747)**  
**[Commit: [turbofan] Broaden checkpoint elimination on returns.](https://chromium.googlesource.com/v8/v8/+/a757a62)**  
  
Closed: Jul 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-624747.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-624747.js)  
```javascript
"use strict";

function bar() {
  try {
    unref;
  } catch (e) {
    return (1 instanceof TypeError) && unref();  
  }
}

function foo() {
  return bar();  
}

%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a757a62^!)  
[src/compiler/checkpoint-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/checkpoint-elimination.cc?cl=a757a62)  
[test/mjsunit/regress/regress-crbug-624747.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-624747.js?cl=a757a62)  
  

### **crbug:621868**  
   
**[Issue: Crash in v8::internal::NewSpace::Verify](https://crbug.com/621868)**  
**[Commit: [crankshaft] Disable further folding already folded allocations.](https://chromium.googlesource.com/v8/v8/+/7b79224)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript>GC  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "merge-merged-5.4"]  
Regress: [mjsunit/regress/regress-crbug-621868.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621868.js)  
```javascript
function f(a) {  
  var n = 1 + a;
}

function g() {
  f();
  var d = {x : f()};
  return [d];
}

g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b79224^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=7b79224)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=7b79224)  
[test/mjsunit/regress/regress-crbug-621868.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-621868.js?cl=7b79224)  
  

### **crbug:621816**  
   
**[Issue: safe_to_deopt_topmost_optimized_code in deoptimizer.cc](https://crbug.com/621816)**  
**[Commit: [turbofan] Fix missing lazy deopt in object literals.](https://chromium.googlesource.com/v8/v8/+/4af8029)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-621816.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621816.js)  
```javascript
function f() {
  var o = {};
  o.a = 1;
}
function g() {
  var o = { ['a']: function(){} };
  f();
}
f();
f();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4af8029^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=4af8029)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=4af8029)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=4af8029)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=4af8029)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=4af8029)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=4af8029)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=4af8029)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=4af8029)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=4af8029)  
[src/full-codegen/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x64/full-codegen-x64.cc?cl=4af8029)  
[src/full-codegen/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x87/full-codegen-x87.cc?cl=4af8029)  
[test/mjsunit/regress/regress-crbug-621816.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-621816.js?cl=4af8029)  
  

### **crbug:621611**  
   
**[Issue: v8 regression or user land bug around rounding numbers / exp form](https://crbug.com/621611)**  
**[Commit: [builtins] Make sure the Math functions and constants agree.](https://chromium.googlesource.com/v8/v8/+/7877dde)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Hotlist-Fixit-PE2016"]  
Regress: [mjsunit/regress/regress-crbug-621611.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-621611.js)  
```javascript
assertEquals(Math.E, Math.exp(1));
assertEquals(Math.LN10, Math.log(10));
assertEquals(Math.LN2, Math.log(2));
assertEquals(Math.LOG10E, Math.log10(Math.E));
assertEquals(Math.LOG2E, Math.log2(Math.E));
assertEquals(Math.SQRT1_2, Math.sqrt(0.5));
assertEquals(Math.SQRT2, Math.sqrt(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7877dde^!)  
[src/base/ieee754.cc](https://cs.chromium.org/chromium/src/v8/src/base/ieee754.cc?cl=7877dde)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=7877dde)  
[src/js/math.js](https://cs.chromium.org/chromium/src/v8/src/js/math.js?cl=7877dde)  
[test/mjsunit/regress/regress-crbug-621611.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-621611.js?cl=7877dde)  
[test/unittests/base/ieee754-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/base/ieee754-unittest.cc?cl=7877dde)  
  

### **crbug:621496**  
   
**[Issue: Fatal error in v8::internal::Parser::PatternRewriter::VisitBinaryOperation()](https://crbug.com/621496)**  
**[Commit: Fix bug with illegal spread as single arrow parameter](https://chromium.googlesource.com/v8/v8/+/b9f682b)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "Stability-Libfuzzer", "Clusterfuzz"]  
Regress: [mjsunit/harmony/regress/regress-crbug-621496.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-621496.js)  
```javascript
(function testIllegalSpreadAsSingleArrowParameter() {
  assertThrows("(...[42]) => 42)", SyntaxError)  
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9f682b^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=b9f682b)  
[test/mjsunit/harmony/regress/regress-crbug-621496.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-621496.js?cl=b9f682b)  
  

### **crbug:621111**  
   
**[Issue: Fatal error in v8::internal::List<T, P>::Add()](https://crbug.com/621111)**  
**[Commit: Fix classifier related bug](https://chromium.googlesource.com/v8/v8/+/2cabc86)**  
  
Closed: Jun 2016  
Type: Bug-Security  
Components: Blink>JavaScript>Language  
Labels: ["Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "M-53", "Security_Severity-Medium", "Security_Impact-Head", "Stability-Libfuzzer", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/harmony/regress/regress-crbug-621111.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-621111.js)  
```javascript
(y = 1[1, [...[]]]) => 1;   
(y = 1[1, [...[]]]) => {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2cabc86^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=2cabc86)  
[test/mjsunit/harmony/regress/regress-crbug-621111.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-621111.js?cl=2cabc86)  
  

### **crbug:620650**  
   
**[Issue: (value & V8_UINT64_C(ADDRESS)) != unexpected || (value & V8_UINT64_C(ADDRESS)) =](https://crbug.com/620650)**  
**[Commit: [mips] Fix using signaling NaN for holes in fixed double arrays.](https://chromium.googlesource.com/v8/v8/+/a81c665)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-620650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-620650.js)  
```javascript
(function() {
  function f(src, dst, i) {
    dst[i] = src[i];
  }
  var buf = new ArrayBuffer(16);
  var view_int32 = new Int32Array(buf);
  view_int32[1] = 0xFFF7FFFF;
  var view_f64 = new Float64Array(buf);
  var arr = [,0.1];
  f(view_f64, arr, -1);
  f(view_f64, arr, 0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a81c665^!)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=a81c665)  
[src/mips/macro-assembler-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.h?cl=a81c665)  
[test/mjsunit/regress/regress-crbug-620650.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-620650.js?cl=a81c665)  
  

### **crbug:620253**  
   
**[Issue: Fatal error in v8::FromJust](https://crbug.com/620253)**  
**[Commit: [d8] Make exception reporting more resilient.](https://chromium.googlesource.com/v8/v8/+/e55384b)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-620253.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-620253.js)  
```javascript
load("test/mjsunit/regress/regress-crbug-620253.js");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e55384b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=e55384b)  
[test/mjsunit/regress/regress-crbug-620253.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-620253.js?cl=e55384b)  
  

### **crbug:620119**  
   
**[Issue: No Permission](https://crbug.com/620119)**  
**[Commit: Rewrite scopes in computed properties in destructured parameters](https://chromium.googlesource.com/v8/v8/+/f795a79)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-620119.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-620119.js)  
```javascript
assertEquals(0, ((x, {[(x = function() { y = 0 }, "foo")]: y = eval(1)}) => { x(); return y })(42, {}));
assertEquals(0, (function (x, {[(x = function() { y = 0 }, "foo")]: y = eval(1)}) { x(); return y })(42, {}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f795a79^!)  
[src/parsing/parser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.h?cl=f795a79)  
[src/parsing/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/pattern-rewriter.cc?cl=f795a79)  
[test/mjsunit/regress/regress-crbug-620119.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-620119.js?cl=f795a79)  
  

### **crbug:619476**  
   
**[Issue: reported_errors_end_ <= i in expression-classifier.h](https://crbug.com/619476)**  
**[Commit: Remove erroneous DCHECK related to expression classifiers](https://chromium.googlesource.com/v8/v8/+/cdec5e8)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-619476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-619476.js)  
```javascript
var x = {};

eval, x[eval];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cdec5e8^!)  
[src/parsing/expression-classifier.h](https://cs.chromium.org/chromium/src/v8/src/parsing/expression-classifier.h?cl=cdec5e8)  
[test/mjsunit/regress-crbug-619476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-619476.js?cl=cdec5e8)  
  

### **crbug:618845**  
   
**[Issue: No Permission](https://crbug.com/618845)**  
**[Commit: Fix stale IC::receiver_map_ after prototype fastification](https://chromium.googlesource.com/v8/v8/+/3b87e9a)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-618845.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-618845.js)  
```javascript
function Foo() {}
Object.defineProperty(Foo.prototype, "name",
                      {get: function() { return "FooName"; }});

function ic(f) {
  return f.prototype.name;
}

assertEquals("FooName", ic(Foo));
assertEquals("FooName", ic(Foo));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b87e9a^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=3b87e9a)  
[test/mjsunit/regress/regress-crbug-618845.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-618845.js?cl=3b87e9a)  
  

### **crbug:618788**  
   
**[Issue: !array->HasFixedTypedArrayElements() in runtime-array.cc](https://crbug.com/618788)**  
**[Commit: Array.prototype.slice should only normalize result if it's an array](https://chromium.googlesource.com/v8/v8/+/56ea2f9)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Clusterfuzz", "Unreproducible"]  
Regress: [mjsunit/regress/regress-crbug-618788.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-618788.js)  
```javascript
Object.defineProperty(Int32Array.prototype, 'length', { set(v) { } });

(function testSlice() {
  var a = new Array();
  a.constructor = Int32Array;
  a.length = 1000; 
  assertTrue(a.slice() instanceof Int32Array);
})();

(function testSplice() {
  var a = new Array();
  a.constructor = Int32Array;
  a.length = 1000; 
  assertTrue(a.splice(1) instanceof Int32Array);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56ea2f9^!)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=56ea2f9)  
[test/mjsunit/regress/regress-crbug-618788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-618788.js?cl=56ea2f9)  
  

### **crbug:617567**  
   
**[Issue: 1 == effect->op()->EffectInputCount() in node-properties.cc](https://crbug.com/617567)**  
**[Commit: [turbofan] Make FindFrameStateBefore handle dead paths.](https://chromium.googlesource.com/v8/v8/+/826627d)**  
  
Closed: Jun 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-617567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617567.js)  
```javascript
var v1 = {};
function g() {
  v1 = [];
  for (var i = 0; i < 1; i++) {
    v1[i]();
  }
}

var v2 = {};
var v3 = {};
function f() {
  v3 = v2;
  g();
}

assertThrows(g);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/826627d^!)  
[src/compiler/node-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/node-properties.cc?cl=826627d)  
[test/mjsunit/regress/regress-crbug-617567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617567.js?cl=826627d)  
  

### **crbug:617527**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSReceiver()) in objects-i](https://crbug.com/617527)**  
**[Commit: Add test case for 85b8c2dc (fix observable array access in messages.js).](https://chromium.googlesource.com/v8/v8/+/ada6fa1)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "Stability-Crash", "Reproducible", "Merge-Merged", "Hotlist-Merge-Approved", "Clusterfuzz", "merge-merged-5.1", "merge-merged-5.2", "NodeJS-Backport-Done"]  
Regress: [mjsunit/regress/regress-crbug-617527.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617527.js)  
```javascript
Object.defineProperty(Array.prototype, "1", { get: toLocaleString });
assertThrows(_ => new RegExp(0, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ada6fa1^!)  
[test/mjsunit/regress/regress-crbug-617527.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617527.js?cl=ada6fa1)  
  

### **crbug:617524**  
   
**[Issue: value->IsMutableHeapNumber() in objects-debug.cc](https://crbug.com/617524)**  
**[Commit: [runtime] Don't use ElementsTransitionAndStoreStub for transitions that involve instance rewriting.](https://chromium.googlesource.com/v8/v8/+/3e0be8d)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0", "merge-merged-5.1", "merge-merged-5.2", "NodeJS-Backport-Done"]  
Regress: [mjsunit/regress/regress-crbug-617524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-617524.js)  
```javascript
function f(a,b,c) {
  a.a = b;
  a[1] = c;
  return a;
}

f(new Array(5),.5,0);
var o1 = f(new Array(5),0,.5);
gc();
var o2 = f(new Array(5),0,0);
var o3 = f(new Array(5),0);
assertEquals(0, o3.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e0be8d^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=3e0be8d)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=3e0be8d)  
[test/mjsunit/regress/regress-crbug-617524.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-617524.js?cl=3e0be8d)  
  

### **crbug:616709**  
   
**[Issue: map->is_stable() in src/compilation-dependencies.cc](https://crbug.com/616709)**  
**[Commit: [turbofan] Remove unnecessary prototype checks for element access.](https://chromium.googlesource.com/v8/v8/+/cad5b29)**  
  
Closed: Aug 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Arch-All", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-616709-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-616709-1.js), [mjsunit/regress/regress-crbug-616709-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-616709-2.js)  
```javascript
for (var i = 0; i < 2000; i++) {
  Object.prototype['X'+i] = true;
}

function boom(a1) {
  return a1[0];
}

var a = new Array(1);
a[0] = 0.1;
boom(a);
boom(a);
%OptimizeFunctionOnNextCall(boom);
boom(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cad5b29^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=cad5b29)  
[src/compiler/access-info.h](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.h?cl=cad5b29)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=cad5b29)  
[src/compiler/js-native-context-specialization.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.h?cl=cad5b29)  
[test/mjsunit/regress/regress-crbug-616709-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-616709-1.js?cl=cad5b29)  
[test/mjsunit/regress/regress-crbug-616709-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-616709-2.js?cl=cad5b29)  
  

### **crbug:615774**  
   
**[Issue: index < GetInternalFieldCount() && index >= 0 in src/objects-inl.h](https://crbug.com/615774)**  
**[Commit: Check CallSite arguments more rigorously](https://chromium.googlesource.com/v8/v8/+/25c2203)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-615774.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-615774.js)  
```javascript
Error.prepareStackTrace = (e,s) => s;
var CallSiteConstructor = Error().stack[0].constructor;

try {
  (new CallSiteConstructor(CallSiteConstructor, 6)).toString();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/25c2203^!)  
[src/js/messages.js](https://cs.chromium.org/chromium/src/v8/src/js/messages.js?cl=25c2203)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=25c2203)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=25c2203)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=25c2203)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=25c2203)  
[src/wasm/wasm-module.cc](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.cc?cl=25c2203)  
[src/wasm/wasm-module.h](https://cs.chromium.org/chromium/src/v8/src/wasm/wasm-module.h?cl=25c2203)  
[test/mjsunit/regress/regress-crbug-615774.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-615774.js?cl=25c2203)  
  

### **crbug:614727**  
   
**[Issue: RUNTIME_ASSERT in size <= Page::kMaxRegularHeapObjectSize in src/runtime/runtime-internal.cc](https://crbug.com/614727)**  
**[Commit: Fix arguments object stubs for large arrays.](https://chromium.googlesource.com/v8/v8/+/e95cfaf)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-614727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614727.js)  
```javascript
"use strict";

function f(a, b, c) { return arguments }
function g(...args) { return args }




var length = Math.pow(2, 15) * 3;
var args = new Array(length);
assertEquals(length, f.apply(null, args).length);
assertEquals(length, g.apply(null, args).length);



var length = Math.pow(2, 16) * 3;
var args = new Array(length);
try { f.apply(null, args) } catch(e) {}
try { g.apply(null, args) } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e95cfaf^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=e95cfaf)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=e95cfaf)  
[src/compiler/js-create-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-create-lowering.cc?cl=e95cfaf)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=e95cfaf)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=e95cfaf)  
[src/mips64/code-stubs-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/code-stubs-mips64.cc?cl=e95cfaf)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=e95cfaf)  
[test/mjsunit/regress/regress-crbug-614727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614727.js?cl=e95cfaf)  
  

### **crbug:614644**  
   
**[Issue: Fatal error in v8::HandleScope::CreateHandle](https://crbug.com/614644)**  
**[Commit: [crankshaft] Guard against side effects in Array.prototype.shift lowering.](https://chromium.googlesource.com/v8/v8/+/173313e)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-614644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614644.js)  
```javascript
function f(a, x) {
  a.shift(2, a.length = 2);
  a[0] = x;
}

f([ ], 1.1);
f([1], 1.1);
%OptimizeFunctionOnNextCall(f);
f([1], 1.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/173313e^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=173313e)  
[test/mjsunit/regress/regress-crbug-614644.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614644.js?cl=173313e)  
  

### **crbug:614292**  
   
**[Issue: IrOpcode::kMerge == control->opcode() in src/compiler/memory-optimizer.cc](https://crbug.com/614292)**  
**[Commit: [turbofan] MemoryOptimizer cannot deal with dead nodes in use lists.](https://chromium.googlesource.com/v8/v8/+/5e0cd38)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-614292.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-614292.js)  
```javascript
function foo() {
  return [] | 0 && values[0] || false;
}

%OptimizeFunctionOnNextCall(foo);
try {
  foo();
} catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5e0cd38^!)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=5e0cd38)  
[test/mjsunit/regress/regress-crbug-614292.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-614292.js?cl=5e0cd38)  
  

### **crbug:613919**  
   
**[Issue: Unreachable code in src/objects-inl.h](https://crbug.com/613919)**  
**[Commit: [deoptimizer] Fix materialization of sloppy arguments.](https://chromium.googlesource.com/v8/v8/+/3cc2adb)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-613919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613919.js)  
```javascript
function g(a) {
  if (a) return arguments;
  %DeoptimizeNow();
  return 23;
}
function f() {
  return g(false);
}
assertEquals(23, f());
assertEquals(23, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(23, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cc2adb^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=3cc2adb)  
[test/mjsunit/regress/regress-crbug-613919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613919.js?cl=3cc2adb)  
  

### **crbug:613905**  
   
**[Issue: Crash in v8::base::NoBarrier_Load](https://crbug.com/613905)**  
**[Commit: [runtime] Don't crash when trying to access manually constructed CallSite object.](https://chromium.googlesource.com/v8/v8/+/a7a14fd)**  
  
Closed: May 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-613905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613905.js)  
```javascript
Error.prepareStackTrace = (e,s) => s;
var CallSiteConstructor = Error().stack[0].constructor;

try {
  (new CallSiteConstructor(3, 6)).toString();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7a14fd^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=a7a14fd)  
[test/mjsunit/regress/regress-crbug-613905.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613905.js?cl=a7a14fd)  
  

### **crbug:613570**  
   
**[Issue: AllowHeapAllocation::IsAllowed() in src/heap/heap-inl.h](https://crbug.com/613570)**  
**[Commit: [json] fix encoding change for two-byte gap strings.](https://chromium.googlesource.com/v8/v8/+/46aeb2a)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-613570.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613570.js)  
```javascript
assertEquals("[\n\u26031,\n\u26032\n]",
             JSON.stringify([1, 2], null, "\u2603"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/46aeb2a^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=46aeb2a)  
[test/mjsunit/regress/regress-crbug-613570.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613570.js?cl=46aeb2a)  
  

### **crbug:613494**  
   
**[Issue: Int64Constant of kRepWord64 (Internal) cannot be changed to kRepTagged in src/co](https://crbug.com/613494)**  
**[Commit: [turbofan] Skip data-flow analysis of code entry field.](https://chromium.googlesource.com/v8/v8/+/dbd7d5a)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-613494.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-613494.js)  
```javascript
function f() {
  var bound = 0;
  function g() { return bound }
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dbd7d5a^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=dbd7d5a)  
[src/compiler/escape-analysis.h](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.h?cl=dbd7d5a)  
[test/mjsunit/regress/regress-crbug-613494.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-613494.js?cl=dbd7d5a)  
  

### **crbug:612142**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (IsDereferenceAllowed(INCLUDE_DEFERRE](https://crbug.com/612142)**  
**[Commit: [turbofan] Kill type Guard nodes during effect/control linearization.](https://chromium.googlesource.com/v8/v8/+/33e571f)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-612142.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-612142.js)  
```javascript
var thrower = {[Symbol.toPrimitive]: function(e) { throw e }};
try {
  for (var i = 0; i < 10; i++) { }
  for (var i = 0.5; i < 100000; ++i) { }
  for (var i = 16 | 0 || 0 || this || 1; i;) { String.fromCharCode(thrower); }
} catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/33e571f^!)  
[src/compiler/effect-control-linearizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.cc?cl=33e571f)  
[src/compiler/effect-control-linearizer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/effect-control-linearizer.h?cl=33e571f)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=33e571f)  
[src/compiler/instruction-selector.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.h?cl=33e571f)  
[test/mjsunit/regress/regress-crbug-612142.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-612142.js?cl=33e571f)  
  

### **crbug:612109**  
   
**[Issue: Crash in v8::internal::Heap::CreateFillerObjectAt](https://crbug.com/612109)**  
**[Commit: [builtins] Rewrite uri.js as builtin functions.](https://chromium.googlesource.com/v8/v8/+/8c31bd8)**  
  
Closed: May 2016  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "M-52", "findit-wrong", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-612109.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-612109.js)  
```javascript
s = "string for triggering osr in __f_0";
for (var i = 0; i < 16; i++) s = s + s;
decodeURI(encodeURI(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c31bd8^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=8c31bd8)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=8c31bd8)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=8c31bd8)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=8c31bd8)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=8c31bd8)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=8c31bd8)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=8c31bd8)  
[src/full-codegen/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.h?cl=8c31bd8)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=8c31bd8)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=8c31bd8)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=8c31bd8)  
[src/full-codegen/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ppc/full-codegen-ppc.cc?cl=8c31bd8)  
[src/full-codegen/s390/full-codegen-s390.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/s390/full-codegen-s390.cc?cl=8c31bd8)  
[src/full-codegen/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x64/full-codegen-x64.cc?cl=8c31bd8)  
[src/full-codegen/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/x87/full-codegen-x87.cc?cl=8c31bd8)  
[src/js/uri.js](https://cs.chromium.org/chromium/src/v8/src/js/uri.js?cl=8c31bd8)  
[src/runtime/runtime-strings.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-strings.cc?cl=8c31bd8)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=8c31bd8)  
[src/uri.cc](https://cs.chromium.org/chromium/src/v8/src/uri.cc?cl=8c31bd8)  
[src/uri.h](https://cs.chromium.org/chromium/src/v8/src/uri.h?cl=8c31bd8)  
[test/cctest/compiler/test-run-intrinsics.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-intrinsics.cc?cl=8c31bd8)  
[test/mjsunit/lithium/SeqStringSetChar.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/lithium/SeqStringSetChar.js?cl=8c31bd8)  
[test/mjsunit/regress/regress-crbug-320922.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-320922.js?cl=8c31bd8)  
[test/mjsunit/regress/regress-crbug-612109.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-612109.js?cl=8c31bd8)  
[test/mjsunit/regress/regress-seqstrsetchar-ex1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-seqstrsetchar-ex1.js?cl=8c31bd8)  
[test/mjsunit/regress/regress-seqstrsetchar-ex3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-seqstrsetchar-ex3.js?cl=8c31bd8)  
[test/mjsunit/regress/string-set-char-deopt.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/string-set-char-deopt.js?cl=8c31bd8)  
[test/mjsunit/string-natives.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/string-natives.js?cl=8c31bd8)  
  

### **crbug:610207**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsSharedFunctionInfo()) in s](https://crbug.com/610207)**  
**[Commit: Don't crash when load eval origin of a call site.](https://chromium.googlesource.com/v8/v8/+/8758245)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-610207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-610207.js)  
```javascript
Error.prepareStackTrace = function(exception, frames) {
  return frames[0].getEvalOrigin();
}

try {
  Realm.eval(0, "throw new Error('boom');");
} catch(e) {
  print(e.stack);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8758245^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=8758245)  
[test/mjsunit/regress/regress-crbug-610207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-610207.js?cl=8758245)  
  

### **crbug:609029**  
   
**[Issue: data()->IsUndefined() || data()->IsFixedArray() in v8/src/objects-debug.cc](https://crbug.com/609029)**  
**[Commit: [turbofan] Implement %_NewObject using FastNewObjectStub.](https://chromium.googlesource.com/v8/v8/+/c321837)**  
  
Closed: May 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-609029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-609029.js)  
```javascript
"xxx".match();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c321837^!)  
[src/compiler/js-intrinsic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-intrinsic-lowering.cc?cl=c321837)  
[test/mjsunit/regress/regress-crbug-609029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-609029.js?cl=c321837)  
  

### **crbug:608279**  
   
**[Issue: !info->shared_info()->feedback_vector()->metadata()->SpecDiffersFrom( info->lite](https://crbug.com/608279)**  
**[Commit: Don't treat catch scopes as possibly-shadowing for sloppy eval](https://chromium.googlesource.com/v8/v8/+/75f2d65)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-608279.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-608279.js)  
```javascript
function __f_38() {
  try {
    throw 0;
  } catch (e) {
    eval();
    var __v_38 = { a: 'hest' };
    __v_38.m = function () { return __v_38.a; };
  }
  return __v_38;
}
var __v_40 = __f_38();
__v_40.m();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/75f2d65^!)  
[src/ast/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/ast/scopes.cc?cl=75f2d65)  
[test/mjsunit/regress/regress-crbug-608279.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-608279.js?cl=75f2d65)  
  

### **crbug:608278**  
   
**[Issue: 1 == translation_size in src/crankshaft/lithium-codegen.cc](https://crbug.com/608278)**  
**[Commit: [es6] Properly handle the case when an inlined getter/setter/constructor does a tail call.](https://chromium.googlesource.com/v8/v8/+/e17a283)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.1"]  
Regress: [mjsunit/regress/regress-crbug-608278.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-608278.js)  
```javascript
"use strict";

function h() {
  var stack = (new Error("boom")).stack;
  print(stack);
  %DeoptimizeFunction(f1);
  %DeoptimizeFunction(f2);
  %DeoptimizeFunction(f3);
  %DeoptimizeFunction(g);
  %DeoptimizeFunction(h);
  return 1;
}
%NeverOptimizeFunction(h);

function g(v) {
  return h();
}


function f1() {
  var o = {};
  o.__defineGetter__('p', g);
  o.p;
}

f1();
f1();
%OptimizeFunctionOnNextCall(f1);
f1();


function f2() {
  var o = {};
  o.__defineSetter__('q', g);
  o.q = 1;
}

f2();
f2();
%OptimizeFunctionOnNextCall(f2);
f2();


function A() {
  return h();
}

function f3() {
  new A();
}

f3();
f3();
%OptimizeFunctionOnNextCall(f3);
f3();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e17a283^!)  
[src/crankshaft/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-arm.cc?cl=e17a283)  
[src/crankshaft/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-arm64.cc?cl=e17a283)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=e17a283)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=e17a283)  
[src/crankshaft/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/lithium.cc?cl=e17a283)  
[src/crankshaft/lithium.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/lithium.h?cl=e17a283)  
[src/crankshaft/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips/lithium-mips.cc?cl=e17a283)  
[src/crankshaft/mips64/lithium-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips64/lithium-mips64.cc?cl=e17a283)  
[src/crankshaft/ppc/lithium-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ppc/lithium-ppc.cc?cl=e17a283)  
[src/crankshaft/s390/lithium-s390.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/s390/lithium-s390.cc?cl=e17a283)  
[src/crankshaft/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-x64.cc?cl=e17a283)  
[src/crankshaft/x87/lithium-x87.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x87/lithium-x87.cc?cl=e17a283)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=e17a283)  
[test/mjsunit/es6/tail-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/tail-call.js?cl=e17a283)  
[test/mjsunit/regress/regress-crbug-608278.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-608278.js?cl=e17a283)  
  

### **crbug:605862**  
   
**[Issue: Unreachable code in src/regexp/jsregexp.h](https://crbug.com/605862)**  
**[Commit: [regexp] Fix non-match and max match length in RegExpCharacterClass.](https://chromium.googlesource.com/v8/v8/+/6f67d17)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-605862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-605862.js)  
```javascript
/[]*1/u.exec("\u1234");
/[^\u0000-\u{10ffff}]*1/u.exec("\u1234");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6f67d17^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=6f67d17)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=6f67d17)  
[test/mjsunit/regress/regress-crbug-605862.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-605862.js?cl=6f67d17)  
  

### **crbug:605060**  
   
**[Issue: map->is_stable() in v8/src/compilation-dependencies.cc](https://crbug.com/605060)**  
**[Commit: Make sure we always try to make prototypes fast again when transitioning accessors](https://chromium.googlesource.com/v8/v8/+/4a6a0f5)**  
  
Closed: Apr 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-605060.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-605060.js)  
```javascript
Array.prototype.__defineGetter__('map', function(){});
Array.prototype.__defineGetter__('map', function(){});
Array.prototype.__defineGetter__('map', function(){});
assertTrue(%HasFastProperties(Array.prototype));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a6a0f5^!)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=4a6a0f5)  
[test/mjsunit/regress/regress-crbug-605060.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-605060.js?cl=4a6a0f5)  
  

### **crbug:604680**  
   
**[Issue: !removed || frame->LookupCode()->marked_for_deoptimization() in src/isolate.cc](https://crbug.com/604680)**  
**[Commit: [deoptimizer] Do not modify stack_fp which is used as a key for lookup of previously materialized objects.](https://chromium.googlesource.com/v8/v8/+/b4dbb2f)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "M-51", "merge-merged-5.1"]  
Regress: [mjsunit/regress/regress-crbug-604680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-604680.js)  
```javascript
function h() {
  var res = g.arguments;
  return res;
}

function g(o) {
  var res = h();
  return res;
}

function f1() {
  var o = { x : 42 };
  var res = g(o);
  return 1;
}

function f0(a, b)  {
  "use strict";
  return f1(5);
}

function boom(b) {
  if (b) throw new Error("boom!");
}

%NeverOptimizeFunction(h);
f0();
f0();
%OptimizeFunctionOnNextCall(f0);

boom(false);
boom(false);
%OptimizeFunctionOnNextCall(boom);

try {
  f0(1, 2, 3);
  boom(true, 1, 2, 3);
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4dbb2f^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=b4dbb2f)  
[test/mjsunit/regress/regress-crbug-604680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-604680.js?cl=b4dbb2f)  
  

### **crbug:604299**  
   
**[Issue: RUNTIME_ASSERT in args[0]->IsString() in src/runtime/runtime-regexp.cc](https://crbug.com/604299)**  
**[Commit: Use InternalArrays from certain Intl code](https://chromium.googlesource.com/v8/v8/+/4f374bb)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-604299.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-604299.js)  
```javascript
Array.prototype.__defineSetter__(0,function(value){});

if (this.Intl) {
  var o = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Katmandu'})
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f374bb^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4f374bb)  
[src/js/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/js/typedarray.js?cl=4f374bb)  
[test/mjsunit/regress/regress-crbug-604299.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-604299.js?cl=4f374bb)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=4f374bb)  
  

### **crbug:603463**  
   
**[Issue: marker->IsSmi() in src/frames.cc](https://crbug.com/603463)**  
**[Commit: Fix polymorphic keyed load handler selection for proxies.](https://chromium.googlesource.com/v8/v8/+/2811388)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "M-53", "Clusterfuzz", "MovedFrom-52", "ClusterFuzz-Verified"]  
Regress: [mjsunit/regress/regress-crbug-603463.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-603463.js)  
```javascript
function load(a, i) {
  return a[i];
}

function f() {
  return load(new Proxy({}, {}), undefined);
}

f();
f();
load([11, 22, 33], 0);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2811388^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=2811388)  
[test/mjsunit/regress/regress-crbug-603463.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-603463.js?cl=2811388)  
  

### **crbug:602595**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsSmi()) in src/objects-inl.](https://crbug.com/602595)**  
**[Commit: [turbofan] Escape analysis treats guard nodes as escaping.](https://chromium.googlesource.com/v8/v8/+/7cef559)**  
  
Closed: May 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-602595.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-602595.js)  
```javascript
function f(a) { return [a] }

assertEquals([23], f(23));
assertEquals([42], f(42));
%OptimizeFunctionOnNextCall(f);
assertEquals([65], f(65));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7cef559^!)  
[src/compiler/escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/escape-analysis.cc?cl=7cef559)  
[test/mjsunit/regress/regress-crbug-602595.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-602595.js?cl=7cef559)  
  

### **crbug:602184**  
   
**[Issue: DICTIONARY_ELEMENTS == elements_kind in src/code-stubs.h](https://crbug.com/602184)**  
**[Commit: [tests] Add testcase for r35397](https://chromium.googlesource.com/v8/v8/+/f4a9a50)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "M-50", "Reproducible", "Clusterfuzz", "M-51"]  
Regress: [mjsunit/regress/regress-crbug-602184.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-602184.js)  
```javascript
function f(test, a) {
  var v;
  if (test) {
    v = v|0;
  }
  a[v] = 1;
}
var v = new String();
f(false, v);
f(false, v);

v = new Int32Array(10);
f(true, v);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f4a9a50^!)  
[test/mjsunit/regress/regress-crbug-602184.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-602184.js?cl=f4a9a50)  
  

### **crbug:601617**  
   
**[Issue: frames_[0].kind() == TranslatedFrame::kFunction || frames_[0].kind() == Translat](https://crbug.com/601617)**  
**[Commit: [deoptimizer] Extend assert to also expect kTailCallerFunction as bottommost frame when accessing arguments for inlined function.](https://chromium.googlesource.com/v8/v8/+/26c480d)**  
  
Closed: Apr 2016  
Type: Bug  
Components: None  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz", "M-51", "merge-merged-5.1"]  
Regress: [mjsunit/regress/regress-crbug-601617.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-601617.js)  
```javascript
function h() {
  var res = g.arguments[0].x;
  return res;
}

function g(o) {
  var res = h();
  return res;
}

function f1() {
  var o = { x : 1 };
  var res = g(o);
  return res;
}

function f0() {
  "use strict";
  return f1(5);
}

%NeverOptimizeFunction(h);
f0();
f0();
%OptimizeFunctionOnNextCall(f0);
assertEquals(1, f0());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/26c480d^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=26c480d)  
[test/mjsunit/regress/regress-crbug-601617.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-601617.js?cl=26c480d)  
  

### **crbug:600257**  
   
**[Issue: unicode() in src/regexp/regexp-parser.cc](https://crbug.com/600257)**  
**[Commit: [regexp] fix assertion failure when parsing close to stack overflow.](https://chromium.googlesource.com/v8/v8/+/93135d8)**  
  
Closed: Apr 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-600257.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-600257.js)  
```javascript
(function rec() {
  try {
    rec();
  } catch (e) {
    /{/;
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93135d8^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=93135d8)  
[test/mjsunit/regress/regress-crbug-600257.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-600257.js?cl=93135d8)  
  

### **crbug:599714**  
   
**[Issue: code->kind() == Code::OPTIMIZED_FUNCTION in src/frames.cc](https://crbug.com/599714)**  
**[Commit: [frames] Also properly deal with TF builtins in OptimizedFrame::GetFunctions().](https://chromium.googlesource.com/v8/v8/+/e5724d9)**  
  
Closed: Apr 2016  
Type: Bug  
Components: None  
Labels: ["Te-Logged", "Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-599714.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599714.js)  
```javascript
var custom_toString = function() {
  var boom = custom_toString.caller;
  return boom;
}

var object = {};
object.toString = custom_toString;

try { Object.hasOwnProperty(object); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5724d9^!)  
[src/frames.cc](https://cs.chromium.org/chromium/src/v8/src/frames.cc?cl=e5724d9)  
[test/mjsunit/regress/regress-crbug-599714.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599714.js?cl=e5724d9)  
  

### **crbug:599073**  
   
**[Issue: args.receiver()->IsJSReceiver() in src/builtins.cc](https://crbug.com/599073)**  
**[Commit: [ic] Use the CallFunction builtin to invoke accessors.](https://chromium.googlesource.com/v8/v8/+/6df9a22)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Stability-Crash", "Reproducible", "Arch-All", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-599073-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-1.js), [mjsunit/regress/regress-crbug-599073-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-2.js), [mjsunit/regress/regress-crbug-599073-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-3.js), [mjsunit/regress/regress-crbug-599073-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599073-4.js)  
```javascript
Object.defineProperty(Boolean.prototype, "v", {get:constructor});

function foo(b) { return b.v; }

foo(true);
foo(true);
foo(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6df9a22^!)  
[src/ic/arm/handler-compiler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm/handler-compiler-arm.cc?cl=6df9a22)  
[src/ic/arm64/handler-compiler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm64/handler-compiler-arm64.cc?cl=6df9a22)  
[src/ic/ia32/handler-compiler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ia32/handler-compiler-ia32.cc?cl=6df9a22)  
[src/ic/mips/handler-compiler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/handler-compiler-mips.cc?cl=6df9a22)  
[src/ic/mips64/handler-compiler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/handler-compiler-mips64.cc?cl=6df9a22)  
[src/ic/x64/handler-compiler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/x64/handler-compiler-x64.cc?cl=6df9a22)  
[test/mjsunit/regress/regress-crbug-599073-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599073-1.js?cl=6df9a22)  
[test/mjsunit/regress/regress-crbug-599073-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599073-2.js?cl=6df9a22)  
[test/mjsunit/regress/regress-crbug-599073-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599073-3.js?cl=6df9a22)  
[test/mjsunit/regress/regress-crbug-599073-4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599073-4.js?cl=6df9a22)  
  

### **crbug:599067**  
   
**[Issue: RUNTIME_ASSERT in args[0]->IsJSObject() in src/runtime/runtime-internal.cc](https://crbug.com/599067)**  
**[Commit: Display a meaningfull error message when trying to capture a stack trace to a proxy.](https://chromium.googlesource.com/v8/v8/+/c7ff576)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Security_Impact-Stable", "Security_Severity-High", "M-49", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-599067.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599067.js)  
```javascript
try {
  var o = {};
  var p = new Proxy({}, o);
  Error.captureStackTrace(p);
} catch(e) {
  assertEquals("invalid_argument", e.message);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c7ff576^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=c7ff576)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=c7ff576)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=c7ff576)  
[test/mjsunit/regress/regress-crbug-599067.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599067.js?cl=c7ff576)  
  

### **crbug:599003**  
   
**[Issue: RUNTIME_ASSERT in map->IsMap() in src/heap/spaces.cc](https://crbug.com/599003)**  
**[Commit: Properly complete in-object slack tracking.](https://chromium.googlesource.com/v8/v8/+/4598356)**  
  
Closed: Apr 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Hotlist-Merge-Review", "M-50", "Reproducible", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "Release-0-M50", "merge-merged-5.0"]  
Regress: [mjsunit/regress/regress-crbug-599003.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-599003.js)  
```javascript
function A() {}

function g1() {
  var obj = new A();
  obj.v0 = 0;
  obj.v1 = 0;
  obj.v2 = 0;
  obj.v3 = 0;
  obj.v4 = 0;
  obj.v5 = 0;
  obj.v6 = 0;
  obj.v7 = 0;
  obj.v8 = 0;
  obj.v9 = 0;
  return obj;
}

function g2() {
  return new A();
}

var o = g1();
%OptimizeFunctionOnNextCall(g2);
g2();
o = null;
gc();

for (var i = 0; i < 20; i++) {
  var o = new A();
}
g2();

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4598356^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4598356)  
[test/mjsunit/regress/regress-crbug-599003.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-599003.js?cl=4598356)  
  

### **crbug:598998**  
   
**[Issue: output_count_ - 1 != frame_index in src/deoptimizer.cc](https://crbug.com/598998)**  
**[Commit: [crankshaft] [turbofan] Fix environment handling when generating a tail call from inlined function.](https://chromium.googlesource.com/v8/v8/+/ecb8fcf)**  
  
Closed: Apr 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-598998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-598998.js)  
```javascript
"use strict";

function deopt_function(func) {
  %DeoptimizeFunction(func);
}

function f(x) {
  return deopt_function(h);
}

function g(x) {
  return f(x, 1);
}

function h(x) {
  g(x, 1);
}

%NeverOptimizeFunction(deopt_function);

h(1);
h(1);
%OptimizeFunctionOnNextCall(h);
h(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecb8fcf^!)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=ecb8fcf)  
[src/crankshaft/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm/lithium-arm.cc?cl=ecb8fcf)  
[src/crankshaft/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/arm64/lithium-arm64.cc?cl=ecb8fcf)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=ecb8fcf)  
[src/crankshaft/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips/lithium-mips.cc?cl=ecb8fcf)  
[src/crankshaft/mips64/lithium-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/mips64/lithium-mips64.cc?cl=ecb8fcf)  
[src/crankshaft/ppc/lithium-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ppc/lithium-ppc.cc?cl=ecb8fcf)  
[src/crankshaft/s390/lithium-s390.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/s390/lithium-s390.cc?cl=ecb8fcf)  
[src/crankshaft/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-x64.cc?cl=ecb8fcf)  
[src/crankshaft/x87/lithium-x87.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x87/lithium-x87.cc?cl=ecb8fcf)  
[test/mjsunit/regress/regress-crbug-598998.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-598998.js?cl=ecb8fcf)  
  

### **crbug:596394**  
   
**[Issue: has_property_ in src/lookup.h](https://crbug.com/596394)**  
**[Commit: Ensure CreateDataProperty works correctly on TypedArrays](https://chromium.googlesource.com/v8/v8/+/7a38462)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-596394.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-596394.js)  
```javascript
__v_0 = new Uint8Array(100);
array = new Array(10);
array.__proto__ = __v_0;
assertThrows(() => Array.prototype.concat.call(array), TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7a38462^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7a38462)  
[test/mjsunit/regress/regress-crbug-596394.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-596394.js?cl=7a38462)  
  

### **crbug:595738**  
   
**[Issue: JSON.stringify() throws error about circular structures in a Backbone Model](https://crbug.com/595738)**  
**[Commit: [json] Allow any callable object for toJSON.](https://chromium.googlesource.com/v8/v8/+/cc04776)**  
  
Closed: Mar 2016  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-50", "has-Bisect", "Via-Wizard", "Arch-All", "M-49", "M-51"]  
Regress: [mjsunit/regress/regress-crbug-595738.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595738.js)  
```javascript
function foo() { return 1; }
var x = {toJSON: foo.bind()};
assertEquals("1", JSON.stringify(x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc04776^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=cc04776)  
[test/mjsunit/regress/regress-crbug-595738.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595738.js?cl=cc04776)  
  

### **crbug:595657**  
   
**[Issue: index <= know_captures in src/regexp/regexp-parser.cc](https://crbug.com/595657)**  
**[Commit: [regexp] catch stack overflow when parsing back references.](https://chromium.googlesource.com/v8/v8/+/1e2d0e1)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0"]  
Regress: [mjsunit/regress/regress-crbug-595657.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595657.js)  
```javascript
function test() {
  try {
    test();
  } catch(e) {
    /(\2)(a)/.test("");
  }
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e2d0e1^!)  
[src/regexp/regexp-parser.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-parser.cc?cl=1e2d0e1)  
[test/mjsunit/regress/regress-crbug-595657.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595657.js?cl=1e2d0e1)  
  

### **crbug:595615**  
   
**[Issue: *deopt_index != Safepoint::kNoDeoptimizationIndex in src/frames.cc](https://crbug.com/595615)**  
**[Commit: [crankshaft] Check if the function is callable before generating a tail call via Call builtin.](https://chromium.googlesource.com/v8/v8/+/e6dca37)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-595615.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-595615.js)  
```javascript
"use strict";

function f(o) {
  return o.x();
}
try { f({ x: 1 }); } catch(e) {}
try { f({ x: 1 }); } catch(e) {}
%OptimizeFunctionOnNextCall(f);
try { f({ x: 1 }); } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6dca37^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=e6dca37)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=e6dca37)  
[test/mjsunit/regress/regress-crbug-595615.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-595615.js?cl=e6dca37)  
  

### **crbug:594955**  
   
**[Issue: elements_kind == DICTIONARY_ELEMENTS in src/ic/handler-compiler.cc](https://crbug.com/594955)**  
**[Commit: Fix polymorphic keyed load handler selection for string elements](https://chromium.googlesource.com/v8/v8/+/84dd29b)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-594955.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594955.js)  
```javascript
function g(s, key) { return s[key]; }

assertEquals(g(new String("a"), "length"), 1);
assertEquals(g(new String("a"), "length"), 1);
assertEquals(g("a", 32), undefined);
assertEquals(g("a", "length"), 1);
assertEquals(g(new String("a"), "length"), 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84dd29b^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=84dd29b)  
[test/mjsunit/regress/regress-crbug-594955.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-594955.js?cl=84dd29b)  
  

### **crbug:594574**  
   
**[Issue: Security: v8 Array.concat OOB access writeup](https://crbug.com/594574)**  
**[Commit: [builtins] Fix Array.prototype.concat bug](https://chromium.googlesource.com/v8/v8/+/96a2bd8)**  
  
Closed: Mar 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Security_Impact-Stable", "merge-merged-4.9", "Hotlist-Merge-Approved", "Security_Severity-High", "M-49", "reward-7500", "allpublic", "reward-inprocess", "merge-merged-50", "merge-merged-5.0", "Release-2-M49", "CVE-2016-1646", "CVE_description-submitted", "Hotlist-Torque"]  
Regress: [mjsunit/regress/regress-crbug-594574-concat-leak-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594574-concat-leak-1.js), [mjsunit/regress/regress-crbug-594574-concat-leak-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594574-concat-leak-2.js)  
```javascript
array = new Array(10);
array[0] = 0.1;

array[2] = 2.1;
array[3] = 3.1;

var copy = array.slice(0, array.length);


var proto = {};
array.__proto__ = proto;


Object.defineProperty(
  proto, 1, {
    get() {
      
      array.length = 1;
      
      gc();
      return "value from proto";
    },
    set(new_value) { }
});

var concatted_array = Array.prototype.concat.call(array);
assertEquals(concatted_array[0], 0.1);
assertEquals(concatted_array[1], "value from proto");
assertEquals(concatted_array[2], undefined);
assertEquals(concatted_array[3], undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96a2bd8^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=96a2bd8)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=96a2bd8)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=96a2bd8)  
[test/mjsunit/array-concat.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-concat.js?cl=96a2bd8)  
[test/mjsunit/es6/array-concat.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/array-concat.js?cl=96a2bd8)  
[test/mjsunit/regress/regress-crbug-594574-concat-leak-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-594574-concat-leak-1.js?cl=96a2bd8)  
[test/mjsunit/regress/regress-crbug-594574-concat-leak-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-594574-concat-leak-2.js?cl=96a2bd8)  
  

### **crbug:594183**  
   
**[Issue: Performance Regression: 5-20% WebGLAquarium 1000 Fishes time across most boards](https://crbug.com/594183)**  
**[Commit: [ic] Restore PROPERTY key tracking in keyed ICs](https://chromium.googlesource.com/v8/v8/+/9bebebd)**  
  
Closed: Apr 2016  
Type: Bug-Regression  
Components: Blink>JavaScript>Runtime  
Labels: ["Hotlist-Merge-Review", "M-50", "Performance", "Cros-Perf-Monitor", "Hotlist-Merge-Approved", "ReleaseBlock-Stable", "merge-merged-50", "merge-merged-5.0", "merge-merged-5.1", "Merge-merged-51", "VerifyIn-53", "VerifyIn-54", "Bulk-Verified"]  
Regress: [mjsunit/regress/regress-crbug-594183.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-594183.js)  
```javascript
var global = {}

var fish = [
  {'name': 'foo'},
  {'name': 'bar'},
];

for (var i = 0; i < fish.length; i++) {
  global[fish[i].name] = 1;
}

function load() {
  var sum = 0;
  for (var i = 0; i < fish.length; i++) {
    var name = fish[i].name;
    sum += global[name];
  }
  return sum;
}

load();
load();
%OptimizeFunctionOnNextCall(load);
load();
assertOptimized(load);

function store() {
  for (var i = 0; i < fish.length; i++) {
    var name = fish[i].name;
    global[name] = 1;
  }
}

store();
store();
%OptimizeFunctionOnNextCall(store);
store();
assertOptimized(store);



function store_element(obj, key) {
  obj[key] = 0;
}

var o1 = new Array(3);
var o2 = new Array(3);
o2.o2 = "o2";
var o3 = new Array(3);
o3.o3 = "o3";
var o4 = new Array(3);
o4.o4 = "o4";
var o5 = new Array(3);
o5.o5 = "o5";

store_element(o1, 0);  
store_element(o1, 0);  
store_element(o2, 0);  
store_element(o3, 0);  
store_element(o4, 0);  
store_element(o5, 0);  

function inferrable_store(key) {
  store_element(o5, key);
}

inferrable_store(0);
inferrable_store(0);
%OptimizeFunctionOnNextCall(inferrable_store);
inferrable_store(0);
assertOptimized(inferrable_store);



inferrable_store("deopt");


if (!isTurboFanned(inferrable_store)) {
  assertUnoptimized(inferrable_store);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bebebd^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=9bebebd)  
[src/ic/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic/ic.h?cl=9bebebd)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=9bebebd)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=9bebebd)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=9bebebd)  
[test/mjsunit/regress/regress-crbug-594183.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-594183.js?cl=9bebebd)  
  

### **crbug:593697**  
   
**[Issue: !is_bottommost || stack_fp_ == fp_value in src/deoptimizer.cc](https://crbug.com/593697)**  
**[Commit: [turbofan] Avoid dereferencing empty handle when inlining a tail call.](https://chromium.googlesource.com/v8/v8/+/690c7a8)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-593697-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-593697-2.js)  
```javascript
"use strict";

var f5 = (function f6(stdlib) {
  "use asm";
  var cos = stdlib.Math.cos;
  function f5() {
    return cos();
  }
  return { f5: f5 };
})(this, {}).f5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/690c7a8^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=690c7a8)  
[test/mjsunit/regress/regress-crbug-593697-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-593697-2.js?cl=690c7a8)  
  

### **crbug:593282**  
   
**[Issue: 0 <= from && to <= String::kMaxCodePoint in src/regexp/regexp-ast.h](https://crbug.com/593282)**  
**[Commit: [regexp] fix bogus assertion in CharacterRange constructor.](https://chromium.googlesource.com/v8/v8/+/d1f68f7)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Cr-Blink-JavaScript", "merge-merged-5.0"]  
Regress: [mjsunit/regress/regress-crbug-593282.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-593282.js)  
```javascript
var __v_11 = {};
function __f_2(depth) {
  try {
    __f_5(depth, __v_11);
    return true;
  } catch (e) {
    gc();
  }
}
function __f_5(n, __v_4) {
  if (--n == 0) {
    __f_1(__v_4);
    return;
  }
  __f_5(n, __v_4);
}
function __f_1(__v_4) {
  var __v_5 = new RegExp(__v_4);
}
function __f_4() {
  var __v_1 = 100;
  var __v_8 = 100000;
  while (__v_1 < __v_8 - 1) {
    var __v_3 = Math.floor((__v_1 + __v_8) / 2);
    if (__f_2(__v_3)) {
      __v_1 = __v_3;
    } else {
      __v_8 = __v_3;
    }
  }
}
__f_4();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d1f68f7^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=d1f68f7)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=d1f68f7)  
[test/cctest/test-regexp.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-regexp.cc?cl=d1f68f7)  
[test/mjsunit/regress/regress-crbug-593282.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-593282.js?cl=d1f68f7)  
  

### **crbug:592343**  
   
**[Issue: entry->to() + 1 > current.from() in src/regexp/jsregexp.cc](https://crbug.com/592343)**  
**[Commit: [regexp] Fix off-by-one in CharacterRange::Negate.](https://chromium.googlesource.com/v8/v8/+/f9d7c71)**  
  
Closed: Mar 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "merge-merged-5.0"]  
Regress: [mjsunit/regress/regress-crbug-592343.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-592343.js)  
```javascript
var r = /[^\u{1}-\u{1000}\u{1002}-\u{2000}]/u;
assertTrue(r.test("\u{0}"));
assertFalse(r.test("\u{1}"));
assertFalse(r.test("\u{1000}"));
assertTrue(r.test("\u{1001}"));
assertFalse(r.test("\u{1002}"));
assertFalse(r.test("\u{2000}"));
assertTrue(r.test("\u{2001}"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f9d7c71^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=f9d7c71)  
[src/regexp/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.h?cl=f9d7c71)  
[src/regexp/regexp-ast.h](https://cs.chromium.org/chromium/src/v8/src/regexp/regexp-ast.h?cl=f9d7c71)  
[test/mjsunit/regress/regress-crbug-592343.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-592343.js?cl=f9d7c71)  
  

### **crbug:592340**  
   
**[Issue: Crash in v8::internal::LookupIterator::UpdateProtector](https://crbug.com/592340)**  
**[Commit: Ensure appropriate bounds checking for Array subclass concat](https://chromium.googlesource.com/v8/v8/+/ca5deb1)**  
  
Closed: Mar 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Stability-Memory-AddressSanitizer", "Clusterfuzz", "Unreproducible", "Cr-Blink-JavaScript"]  
Regress: [mjsunit/regress/regress-crbug-592340.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-592340.js)  
```javascript
class MyArray extends Array { }
Object.prototype[Symbol.species] = MyArray;
delete Array[Symbol.species];
__v_1 = Math.pow(2, 31);
__v_2 = [];
__v_2[__v_1] = 31;
__v_4 = [];
__v_4[__v_1 - 2] = 33;
assertThrows(() => __v_2.concat(__v_4), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ca5deb1^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=ca5deb1)  
[test/mjsunit/regress/regress-crbug-592340.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-592340.js?cl=ca5deb1)  
  

### **crbug:590989**  
   
**[Issue: callback function not called any more after called lots of times and in different place.](https://crbug.com/590989)**  
**[Commit: [crankshaft] Fix invalid ToNumber optimization.](https://chromium.googlesource.com/v8/v8/+/0c35579)**  
  
Closed: Mar 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["ReleaseBlock-Beta", "M-50", "Via-Wizard", "Arch-All", "merge-merged-5.0"]  
Regress: [mjsunit/regress/regress-crbug-590989-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-590989-1.js), [mjsunit/regress/regress-crbug-590989-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-590989-2.js)  
```javascript
var o = {}
var p = {foo: 1.5}

function g(x) { return x.foo === +x.foo; }

assertEquals(false, g(o));
assertEquals(false, g(o));
%OptimizeFunctionOnNextCall(g);
assertEquals(false, g(o));  
assertEquals(true, g(p));
%OptimizeFunctionOnNextCall(g);
assertEquals(false, g(o));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c35579^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=0c35579)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=0c35579)  
[test/mjsunit/regress/regress-crbug-590989-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-590989-1.js?cl=0c35579)  
[test/mjsunit/regress/regress-crbug-590989-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-590989-2.js?cl=0c35579)  
  

### **crbug:589792**  
   
**[Issue: Security: [v8] Out of bound(??) memory write with asm.js](https://crbug.com/589792)**  
**[Commit: [turbofan] Bailout if LoadBuffer typing assumption doesn't hold.](https://chromium.googlesource.com/v8/v8/+/58ab990)**  
  
Closed: Mar 2016  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["M-50", "reward-5000", "Nag", "Security_Impact-Stable", "merge-merged-4.9", "Security_Severity-High", "M-49", "allpublic", "Release-0-M50", "merge-merged-5.0", "CVE-2016-1653", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-589792.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-589792.js)  
```javascript
var boom = (function(stdlib, foreign, heap) {
  "use asm";
  var MEM8 = new stdlib.Uint8Array(heap);
  var MEM32 = new stdlib.Int32Array(heap);
  function foo(i, j) {
    j = MEM8[256];
    
    MEM32[j >> 10] = 0xabcdefaa;
    return MEM32[j >> 2] + j
  }
  return foo
})(this, 0, new ArrayBuffer(256));
%OptimizeFunctionOnNextCall(boom);
boom(0, 0x1000);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58ab990^!)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=58ab990)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=58ab990)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=58ab990)  
[test/cctest/cctest.gyp](https://cs.chromium.org/chromium/src/v8/test/cctest/cctest.gyp?cl=58ab990)  
[test/cctest/compiler/test-run-properties.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-properties.cc?cl=58ab990)  
[test/mjsunit/regress/regress-crbug-589792.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-589792.js?cl=58ab990)  
  

### **crbug:589472**  
   
**[Issue: operand_stack_depth_ >= count in src/full-codegen/full-codegen.cc](https://crbug.com/589472)**  
**[Commit: [fullcodegen] Fix assert for operand stack depth tracking.](https://chromium.googlesource.com/v8/v8/+/3baa290)**  
  
Closed: Feb 2016  
Type: Bug  
Components: None  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz", "Cr-Blink-JavaScript"]  
Regress: [mjsunit/regress/regress-crbug-589472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-589472.js)  
```javascript
try { f() } catch(e) { assertInstanceof(e, RangeError) }

function f() {
  return Math.max(
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" + "a" +
    "boom", 1, 2, 3, 4, 5, 6, 7, 8, 9);
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3baa290^!)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=3baa290)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=3baa290)  
[test/mjsunit/regress/regress-crbug-589472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-589472.js?cl=3baa290)  
  

### **crbug:587068**  
   
**[Issue: 10.6%-15.6% regression in webrtc.datachannel at 375196:375247](https://crbug.com/587068)**  
**[Commit: [crankshaft] Fix deopt loop in String.fromCharCode on non-int32 inputs.](https://chromium.googlesource.com/v8/v8/+/6cc5c60)**  
  
Closed: Feb 2016  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-50", "TE-Triaged", "Arch-All", "Performance-Sheriff"]  
Regress: [mjsunit/regress/regress-crbug-587068.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-587068.js)  
```javascript
function foo(i) { return String.fromCharCode(i); }
foo(33);
foo(33);
%OptimizeFunctionOnNextCall(foo);
foo(33.3);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6cc5c60^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=6cc5c60)  
[test/mjsunit/regress/regress-crbug-587068.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-587068.js?cl=6cc5c60)  
  

### **crbug:584188**  
   
**[Issue: object->HasFastSmiOrObjectElements() || object->HasFastDoubleElements() in src/o](https://crbug.com/584188)**  
**[Commit: Fix Array.prototype.sort for *_STRING_WRAPPER_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/5d2c09a)**  
  
Closed: Feb 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-584188.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-584188.js)  
```javascript
var x = {};
try {
Object.defineProperty(String.prototype, "3", { x: function() { x = v; }});
string = "bla";
} catch(e) {; }
assertThrows("Array.prototype.sort.call(string);", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d2c09a^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=5d2c09a)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=5d2c09a)  
[test/mjsunit/regress/regress-crbug-584188.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-584188.js?cl=5d2c09a)  
  

### **crbug:583257**  
   
**[Issue: transition_map->has_dictionary_elements() || transition_map->has_fixed_typed_arr](https://crbug.com/583257)**  
**[Commit: More *_STRING_WRAPPER_ELEMENTS fixes](https://chromium.googlesource.com/v8/v8/+/d582d2b)**  
  
Closed: Feb 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-583257.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-583257.js)  
```javascript
Object.defineProperty(String.prototype, "0", { __v_1: 1});
Object.defineProperty(String.prototype, "3", { __v_1: 1});

(function () {
  var s = new String();
  function set(object, index, value) { object[index] = value; }
  set(s, 10, "value");
  set(s, 1073741823, "value");
})();

function __f_11() {
  Object.preventExtensions(new String());
}
__f_11();
__f_11();

(function() {
  var i = 10;
  var a = new String("foo");
  for (var j = 0; j < i; j++) {
    a[j] = {};
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d582d2b^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=d582d2b)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d582d2b)  
[test/mjsunit/regress/regress-crbug-583257.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-583257.js?cl=d582d2b)  
  

### **crbug:581577**  
   
**[Issue: RegExp flag/source getters cause app.jiff.com to fail](https://crbug.com/581577)**  
**[Commit: ES2015 web compat workaround: RegExp.prototype.flags => ""](https://chromium.googlesource.com/v8/v8/+/b22b258)**  
  
Closed: Feb 2016  
Type: Bug  
Components: Blink>JavaScript>Language  
Labels: ["M-48", "hasbisect", "Via-Wizard", "merge-merged-4.9"]  
Regress: [mjsunit/regress/regress-crbug-581577.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-581577.js)  
```javascript
assertEquals("", RegExp.prototype.flags);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b22b258^!)  
[src/js/harmony-unicode-regexps.js](https://cs.chromium.org/chromium/src/v8/src/js/harmony-unicode-regexps.js?cl=b22b258)  
[src/js/regexp.js](https://cs.chromium.org/chromium/src/v8/src/js/regexp.js?cl=b22b258)  
[test/mjsunit/es6/regexp-flags.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regexp-flags.js?cl=b22b258)  
[test/mjsunit/regress/regress-crbug-581577.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-581577.js?cl=b22b258)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=b22b258)  
[test/webkit/fast/regex/toString-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/regex/toString-expected.txt?cl=b22b258)  
  

### **crbug:580934**  
   
**[Issue: Block-scope (let) sibling functions becoming undefined / garbage-collected](https://crbug.com/580934)**  
**[Commit: Ensure arrow functions can close over lexically-scoped variables](https://chromium.googlesource.com/v8/v8/+/953bb41)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Merge-Merged-49", "M-48", "Via-Wizard", "merge-merged-4.8", "merge-merged-4.9", "M-49", "merge-merged-48"]  
Regress: [mjsunit/regress/regress-crbug-580934.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580934.js)  
```javascript
"use strict";
{
  let one = () => {
    return "example.com";
  };

  let two = () => {
    return one();
  };

  assertEquals("example.com", two());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/953bb41^!)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=953bb41)  
[test/mjsunit/regress/regress-crbug-580934.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-580934.js?cl=953bb41)  
  

### **crbug:580584**  
   
**[Issue: Chr-48 regression, w bisect: Object.defineProperty broken for window.localStorage, sessionStorage, etc](https://crbug.com/580584)**  
**[Commit: [api] Default native data property setter to replace the setter if the property is writable.](https://chromium.googlesource.com/v8/v8/+/997cd3d)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-580584.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580584.js)  
```javascript
function f() { return arguments }


Object.defineProperty(f, "name", {
  writable: true, configurable: true, value: 10});
assertEquals({value: 10, writable: true, enumerable: false, configurable: true},
             Object.getOwnPropertyDescriptor(f, "name"));

var args = f();



args[Symbol.iterator] = 10;
assertEquals({value: 10, writable: true, configurable: true, enumerable: false},
             Object.getOwnPropertyDescriptor(args, Symbol.iterator));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/997cd3d^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=997cd3d)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=997cd3d)  
[src/api-natives.cc](https://cs.chromium.org/chromium/src/v8/src/api-natives.cc?cl=997cd3d)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=997cd3d)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=997cd3d)  
[src/lookup.h](https://cs.chromium.org/chromium/src/v8/src/lookup.h?cl=997cd3d)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=997cd3d)  
[src/snapshot/serialize.cc](https://cs.chromium.org/chromium/src/v8/src/snapshot/serialize.cc?cl=997cd3d)  
[test/mjsunit/regress/regress-crbug-580584.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-580584.js?cl=997cd3d)  
  

### **crbug:580506**  
   
**[Issue: memcmp(fresh->address(), new_map->address(), Map::kCodeCacheOffset) == 0 in src/](https://crbug.com/580506)**  
**[Commit: Also check new_target_is_base() bit when comparing two maps for equivalence.](https://chromium.googlesource.com/v8/v8/+/ac03ef0)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-580506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-580506.js)  
```javascript
(function() {
  'use strict';
  class A extends Function {
    constructor(...args) {
      super(...args);
      this.a = 42;
    }
  }
  var v1 = new A("'use strict';");
  function f(func) {
    func.__defineSetter__('a', function() { });
  }
  var v2 = new A();
  f(v2);
  f(v1);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac03ef0^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ac03ef0)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=ac03ef0)  
[test/mjsunit/regress/regress-crbug-580506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-580506.js?cl=ac03ef0)  
  

### **crbug:578039**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsMap()) in src/objects-inl.](https://crbug.com/578039)**  
**[Commit: [proxy] Reload the initial map after prototype lookup on constructable Proxy.](https://chromium.googlesource.com/v8/v8/+/ec30425)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-4.9", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js)  
```javascript
function target() {};

var proxy = new Proxy(target, {
  get() {
    
    target.prototype = 123;
  }});

new proxy();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec30425^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ec30425)  
[test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-578039-Proxy_construct_prototype_change.js?cl=ec30425)  
  

### **crbug:577112**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/577112)**  
**[Commit: [crankshaft] Don't inline array indexOf operations if receiver's proto is not a JSObject.](https://chromium.googlesource.com/v8/v8/+/1bb7cfd)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-577112.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-577112.js)  
```javascript
Array.prototype.__proto__ = null;
var prototype = Array.prototype;
function f() {
  prototype.lastIndexOf({});
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1bb7cfd^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=1bb7cfd)  
[test/mjsunit/regress/regress-crbug-577112.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-577112.js?cl=1bb7cfd)  
  

### **crbug:575314**  
   
**[Issue: How to find the cause of the new " promiseCapability.[[Resolve]] is not a function" error](https://crbug.com/575314)**  
**[Commit: Partial rollback of Promise error checking](https://chromium.googlesource.com/v8/v8/+/ee9d7ac)**  
  
Closed: Mar 2016  
Type: Bug  
Components: Blink>JavaScript>Language  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-575314.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575314.js)  
```javascript
var test = new Promise(function(){});
test.constructor = function(){};
Promise.resolve(test).catch(e => %AbortJS(e + " FAILED!"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee9d7ac^!)  
[src/js/promise.js](https://cs.chromium.org/chromium/src/v8/src/js/promise.js?cl=ee9d7ac)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=ee9d7ac)  
[test/mjsunit/regress/regress-crbug-575314.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575314.js?cl=ee9d7ac)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=ee9d7ac)  
  

### **crbug:575082**  
   
**[Issue: DaysFromYearMonth(*year, 0) + days == save_days in src/date.cc](https://crbug.com/575082)**  
**[Commit: [date] Date parser says true even for wrong dates, check twice.](https://chromium.googlesource.com/v8/v8/+/b0d0d57)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-575082.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575082.js)  
```javascript
var y = new Date("-1073741824");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0d0d57^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=b0d0d57)  
[src/date.cc](https://cs.chromium.org/chromium/src/v8/src/date.cc?cl=b0d0d57)  
[test/mjsunit/regress/regress-crbug-575082.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575082.js?cl=b0d0d57)  
  

### **crbug:575080**  
   
**[Issue: tagged_expected == tagged_actual in src/layout-descriptor.cc](https://crbug.com/575080)**  
**[Commit: Generalize all representations when reconfiguring a property of a strict Function subclass.](https://chromium.googlesource.com/v8/v8/+/405c7a6)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-575080.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-575080.js)  
```javascript
class A extends Function {
  constructor(...args) {
    super(...args);
    this.a = 42;
    this.d = 4.2;
    this.o = 0;
  }
}
var obj = new A("'use strict';");
obj.o = 0.1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/405c7a6^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=405c7a6)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=405c7a6)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=405c7a6)  
[test/mjsunit/regress/regress-crbug-575080.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-575080.js?cl=405c7a6)  
  

### **crbug:573858**  
   
**[Issue: constructor->shared()->construct_stub() == isolate()->builtins()->builtin(Builti](https://crbug.com/573858)**  
**[Commit: ThrowTypeError should not be constructable, so shouldn't have a prototype.](https://chromium.googlesource.com/v8/v8/+/09c41d9)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-573858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-573858.js)  
```javascript
var throw_type_error = Object.getOwnPropertyDescriptor(
    (function() {"use strict"}).__proto__, "caller").get;

function create_initial_map() { this instanceof throw_type_error }
%OptimizeFunctionOnNextCall(create_initial_map);
assertThrows(create_initial_map);

function test() { new throw_type_error }
%OptimizeFunctionOnNextCall(test);
assertThrows(test);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09c41d9^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=09c41d9)  
[test/mjsunit/regress/regress-crbug-573858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-573858.js?cl=09c41d9)  
[test/mjsunit/strict-mode.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/strict-mode.js?cl=09c41d9)  
  

### **crbug:573857**  
   
**[Issue: p->IsSmi() in src/objects-debug.cc](https://crbug.com/573857)**  
**[Commit: Use JSObjectVerify instead of trying to reimplement parts of it.](https://chromium.googlesource.com/v8/v8/+/fed2c41)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-573857.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-573857.js)  
```javascript
function f() {}
f = f.bind();
f.x = f.name;
f.__defineGetter__('name', function() { return f.x; });
function g() {}
g.prototype = f;
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fed2c41^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=fed2c41)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=fed2c41)  
[test/mjsunit/regress/regress-crbug-573857.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-573857.js?cl=fed2c41)  
  

### **crbug:572590**  
   
**[Issue: p->IsSmi() in src/objects-debug.cc](https://crbug.com/572590)**  
**[Commit: Only verify in-object fields in fast properties case.](https://chromium.googlesource.com/v8/v8/+/2fcf3aa)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-572590.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-572590.js)  
```javascript
function g() { }
var f = g.bind();
f.__defineGetter__('length', g);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2fcf3aa^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=2fcf3aa)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=2fcf3aa)  
[test/mjsunit/regress/regress-crbug-572590.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-572590.js?cl=2fcf3aa)  
  

### **crbug:571517**  
   
**[Issue: Crash in v8::internal::JSObject::UnregisterPrototypeUser](https://crbug.com/571517)**  
**[Commit: [prototype user tracking] Don't skip JSGlobalProxies](https://chromium.googlesource.com/v8/v8/+/b4583c0)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "M-49", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-571517.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-571517.js)  
```javascript
function Receiver() { this.receiver = "receiver"; }
function Proto() { this.proto = "proto"; }

function f(a) {
  return a.foo;
}

var rec = new Receiver();




var proto = rec.__proto__;


assertEquals(undefined, f(rec));
assertEquals(undefined, f(rec));


var p2 = new Proto();
p2.__proto__ = null;
proto.__proto__ = p2;


assertEquals(undefined, f(rec));


p2.foo = "bar";
assertEquals("bar", f(rec));



delete p2.foo;
p2.secret = "GAME OVER";
assertEquals(undefined, f(rec));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4583c0^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=b4583c0)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b4583c0)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=b4583c0)  
[test/mjsunit/regress/regress-crbug-571517.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-571517.js?cl=b4583c0)  
  

### **crbug:571370**  
   
**[Issue: properties()->IsDictionary() == map()->is_dictionary_map() in src/objects-inl.h](https://crbug.com/571370)**  
**[Commit: [ic] Fixed receiver_map register trashing in KeyedStoreIC megamorphic.](https://chromium.googlesource.com/v8/v8/+/c1aded3)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-571370.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-571370.js)  
```javascript
var val = [0.5];
var arr = [0.5];
for (var i = -1; i < 1; i++) {
  arr[i] = val;
}
assertEquals(val, arr[-1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c1aded3^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=c1aded3)  
[src/ic/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm/ic-arm.cc?cl=c1aded3)  
[src/ic/mips/ic-mips.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips/ic-mips.cc?cl=c1aded3)  
[src/ic/mips64/ic-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/mips64/ic-mips64.cc?cl=c1aded3)  
[src/ic/ppc/ic-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ppc/ic-ppc.cc?cl=c1aded3)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=c1aded3)  
[src/mips/macro-assembler-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.h?cl=c1aded3)  
[src/mips64/macro-assembler-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/macro-assembler-mips64.cc?cl=c1aded3)  
[src/mips64/macro-assembler-mips64.h](https://cs.chromium.org/chromium/src/v8/src/mips64/macro-assembler-mips64.h?cl=c1aded3)  
[src/ppc/macro-assembler-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/macro-assembler-ppc.cc?cl=c1aded3)  
[src/ppc/macro-assembler-ppc.h](https://cs.chromium.org/chromium/src/v8/src/ppc/macro-assembler-ppc.h?cl=c1aded3)  
[test/mjsunit/regress/regress-crbug-571370.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-571370.js?cl=c1aded3)  
  

### **crbug:571149**  
   
**[Issue: Uninitialized closed over variable within a function with rest parameters causes assertion failure in ToBoolean IC.](https://crbug.com/571149)**  
**[Commit: Don't pre-initialise block contexts with holes](https://chromium.googlesource.com/v8/v8/+/92e6f7a)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Via-Wizard", "merge-merged-4.9"]  
Regress: [mjsunit/harmony/regress/regress-crbug-571149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-571149.js)  
```javascript
(function(a = 0){
  var x;  
  (function() { return !x })();
})();

(function({a}){
  var x;  
  (function() { return !x })();
})({});

(function(...a){
  var x;  
  (function() { return !x })();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/92e6f7a^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=92e6f7a)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=92e6f7a)  
[test/mjsunit/harmony/regress/regress-crbug-571149.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-571149.js?cl=92e6f7a)  
  

### **crbug:571064**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/571064)**  
**[Commit: [crankshaft] Don't inline array resize operations if receiver's proto is not a JSObject.](https://chromium.googlesource.com/v8/v8/+/bae0d6c)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-571064.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-571064.js)  
```javascript
Array.prototype.__proto__ = null;
var func = Array.prototype.push;
var prototype = Array.prototype;
function CallFunc(a) {
  func.call(a);
}
function CallFuncWithPrototype() {
  CallFunc(prototype);
}
CallFunc([]);
CallFunc([]);
%OptimizeFunctionOnNextCall(CallFuncWithPrototype);
CallFuncWithPrototype();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bae0d6c^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=bae0d6c)  
[test/mjsunit/regress/regress-crbug-571064.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-571064.js?cl=bae0d6c)  
  

### **crbug:570241**  
   
**[Issue: Stack-buffer-underflow in v8::internal::QuickCheckDetails::Advance](https://crbug.com/570241)**  
**[Commit: [regexp] clear QuickCheckDetails for backward reads.](https://chromium.googlesource.com/v8/v8/+/65d3009)**  
  
Closed: Dec 2015  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "M-48", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-570241.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-570241.js)  
```javascript
assertTrue(/(?<=12345123451234512345)/.test("12345123451234512345"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65d3009^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=65d3009)  
[src/regexp/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.h?cl=65d3009)  
[test/mjsunit/regress/regress-crbug-570241.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-570241.js?cl=65d3009)  
  

### **crbug:569534**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray()) in src](https://crbug.com/569534)**  
**[Commit: Fix^3 cast in HasEnumerableElements](https://chromium.googlesource.com/v8/v8/+/a0d03d7)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-569534.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-569534.js)  
```javascript
var array = [,0.5];
array.length = 0;
for (var i in array) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0d03d7^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a0d03d7)  
[test/mjsunit/regress/regress-crbug-569534.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-569534.js?cl=a0d03d7)  
  

### **crbug:568525**  
   
**[Issue: object->IsJSArray() in src/objects.cc](https://crbug.com/568525)**  
**[Commit: Fix mix-up in HasEnumerableElements()](https://chromium.googlesource.com/v8/v8/+/989f44f)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-568525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-568525.js)  
```javascript
var a = /a/;
a[4] = 1.5;
for (var x in a) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/989f44f^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=989f44f)  
[test/mjsunit/regress/regress-crbug-568525.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-568525.js?cl=989f44f)  
  

### **crbug:565917**  
   
**[Issue: 2 == args.length() in src/builtins.cc](https://crbug.com/565917)**  
**[Commit: [es6] Unify ArrayBuffer and SharedArrayBuffer constructors.](https://chromium.googlesource.com/v8/v8/+/cb21144)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-565917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-565917.js)  
```javascript
try {
} catch(e) {; }
new ArrayBuffer();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb21144^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=cb21144)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=cb21144)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=cb21144)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=cb21144)  
[src/js/arraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/js/arraybuffer.js?cl=cb21144)  
[src/js/harmony-sharedarraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/js/harmony-sharedarraybuffer.js?cl=cb21144)  
[src/runtime/runtime-typedarray.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-typedarray.cc?cl=cb21144)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=cb21144)  
[test/mjsunit/regress/regress-crbug-565917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-565917.js?cl=cb21144)  
  

### **crbug:563929**  
   
**[Issue: is_int8(disp) in src/x64/assembler-x64.cc](https://crbug.com/563929)**  
**[Commit: [x86] Sane default for Label::Distance on JumpIfRoot/JumpIfNotRoot.](https://chromium.googlesource.com/v8/v8/+/c83db2d)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-563929.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-563929.js)  
```javascript
var x = 0;
function a() {
  eval("");
  return (function() {
    eval("");
    return (function() {
      eval("");
      return (function() {
        eval("");
        return (function() {
          eval("");
          return (function() {
            eval("");
            return (function() {
              eval("");
              return (function() {
                eval("");
                return x;
              })();
            }) ();
          })();
        })();
      })();
    })();
  })();
}
assertEquals(a(), 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c83db2d^!)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=c83db2d)  
[src/x64/macro-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.h?cl=c83db2d)  
[test/mjsunit/regress/regress-crbug-563929.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-563929.js?cl=c83db2d)  
  

### **crbug:561973**  
   
**[Issue: result == value >= kMinValue && value <= kMaxValue in src/objects.h](https://crbug.com/561973)**  
**[Commit: Fix UTC offset computation in date parser.](https://chromium.googlesource.com/v8/v8/+/37b5ebc)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-561973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-561973.js)  
```javascript
Date.parse('Sat, 01 Jan 100 08:00:00 UT-59011430400000');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37b5ebc^!)  
[src/dateparser.cc](https://cs.chromium.org/chromium/src/v8/src/dateparser.cc?cl=37b5ebc)  
[test/mjsunit/regress/regress-crbug-561973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-561973.js?cl=37b5ebc)  
  

### **crbug:557807**  
   
**[Issue: map->is_stable() in src/compilation-dependencies.cc](https://crbug.com/557807)**  
**[Commit: [turbofan] Unstable prototype maps are not supported currently.](https://chromium.googlesource.com/v8/v8/+/3c9ac97)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-557807.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-557807.js)  
```javascript
function bar() { return { __proto__: this }; }
function foo(a) { a[0] = 0.3; }
foo(bar());
%OptimizeFunctionOnNextCall(foo);
foo(bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c9ac97^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=3c9ac97)  
[test/mjsunit/regress/regress-crbug-557807.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-557807.js?cl=3c9ac97)  
  

### **crbug:554946**  
   
**[Issue: Security: Pwn2Own mobile case, out-of-bound access in json stringifier](https://crbug.com/554946)**  
**[Commit: [JSON stringifier] Correctly load array elements.](https://chromium.googlesource.com/v8/v8/+/6df9a1d)**  
  
Closed: Nov 2015  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Release-0-M47", "M-47", "CVE-2015-6764", "merge-merged-46", "Security_Impact-Stable", "Security_Severity-High", "reward-7500", "Merge-merged-47", "allpublic", "merge-merged-48", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-554946.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-554946.js)  
```javascript
var array = [];
var funky = {
  toJSON: function() { array.length = 1; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[0] = funky;
assertEquals('["funky",null,null,null,null,null,null,null,null,null]',
             JSON.stringify(array));

array = [];
funky = {
  get value() { array.length = 1; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},null,null,null,null,null,null]',
             JSON.stringify(array));

array = [];
funky = {
  get value() { array.pop(); return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},4,5,6,7,8,null]', JSON.stringify(array));

array = [];
funky = {
  get value() { delete array[9]; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},4,5,6,7,8,null]', JSON.stringify(array));

array = [];
funky = {
  get value() { delete array[6]; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},4,5,null,7,8,9]', JSON.stringify(array));

array = [];
funky = {
  get value() { array[12] = 12; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},4,5,6,7,8,9]', JSON.stringify(array));

array = [];
funky = {
  get value() { array[10000000] = 12; return "funky"; }
};
for (var i = 0; i < 10; i++) array[i] = i;
array[3] = funky;
assertEquals('[0,1,2,{"value":"funky"},4,5,6,7,8,9]', JSON.stringify(array));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6df9a1d^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=6df9a1d)  
[test/mjsunit/regress/regress-crbug-554946.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-554946.js?cl=6df9a1d)  
  

### **crbug:554831**  
   
**[Issue: No Permission](https://crbug.com/554831)**  
**[Commit: [crankshaft] only compile string index access with element key.](https://chromium.googlesource.com/v8/v8/+/5bcddae)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-554831.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-554831.js)  
```javascript
(function() {
  var key = "s";
  function f(object) { return object[key]; };
  f("");
  f("");
  %OptimizeFunctionOnNextCall(f);
  f("");
  assertOptimized(f);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bcddae^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=5bcddae)  
[test/mjsunit/regress/regress-crbug-554831.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-554831.js?cl=5bcddae)  
  

### **crbug:552304**  
   
**[Issue: -1 <= index && index < param_count in src/frames-inl.h](https://crbug.com/552304)**  
**[Commit: [turbofan] Fix wrong parameter indices in JSFrameSpecialization.](https://chromium.googlesource.com/v8/v8/+/925a200)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-552304.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-552304.js)  
```javascript
(function() {
  "use asm";
  return function() {
    for (var i = 0; i < 100000; delete unresolved_name) {++i}
    return 0;
  }
})()(this + "i");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/925a200^!)  
[src/compiler/js-frame-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-frame-specialization.cc?cl=925a200)  
[test/mjsunit/regress/regress-crbug-552304.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-552304.js?cl=925a200)  
  

### **crbug:551287**  
   
**[Issue: block->IsFinished() in src/crankshaft/hydrogen.cc](https://crbug.com/551287)**  
**[Commit: [crankshaft] Fix crash when case labels inline endless loops](https://chromium.googlesource.com/v8/v8/+/3cb3a6f)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-551287.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-551287.js)  
```javascript
function f() { do { } while (true); }

function boom(x) {
  switch(x) {
    case 1:
    case f(): return;
  }
}

%OptimizeFunctionOnNextCall(boom)
boom(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cb3a6f^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=3cb3a6f)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3cb3a6f)  
[test/mjsunit/regress/regress-crbug-551287.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-551287.js?cl=3cb3a6f)  
  

### **crbug:549162**  
   
**[Issue: index >= 0 && index < this->length() in src/objects-inl.h](https://crbug.com/549162)**  
**[Commit: Fix cached EnumLength retrieval in JSObject::NumberOfOwnProperties](https://chromium.googlesource.com/v8/v8/+/70a2f53)**  
  
Closed: Oct 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-549162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-549162.js)  
```javascript
var s = Symbol("foo");
var __v_13 = {}
Object.defineProperty( __v_13, s, {value: {}, enumerable: true});
for (var __v_14 in __v_13) {}
__v_13 = {}
Object.defineProperty( __v_13, s, {value: {}, enumerable: true});
var __v_14 = Object.create(Object.prototype, __v_13)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70a2f53^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=70a2f53)  
[test/mjsunit/regress/regress-crbug-549162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-549162.js?cl=70a2f53)  
  

### **crbug:548580**  
   
**[Issue: value->IsHeapObject() in src/objects-debug.cc](https://crbug.com/548580)**  
**[Commit: Regression test for JSRegExp literals sharing.](https://chromium.googlesource.com/v8/v8/+/37a9be5)**  
  
Closed: Nov 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-548580.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-548580.js)  
```javascript
function store(v) {
  var re = /(?=[d#.])/;
  re.a = v;
  return re;
}

var re1 = store(undefined);
var re2 = store(42);

assertEquals(re1.a, undefined);
assertEquals(re2.a, 42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37a9be5^!)  
[test/mjsunit/regress/regress-crbug-548580.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-548580.js?cl=37a9be5)  
  

### **crbug:545364**  
   
**[Issue: AllowHeapAllocation::IsAllowed() in src/objects.cc](https://crbug.com/545364)**  
**[Commit: [turbofan] We cannot unconditionally flatten cons strings in the JSGraph.](https://chromium.googlesource.com/v8/v8/+/d168a1e)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-545364.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-545364.js)  
```javascript
(function() {
  "use asm";
  return function(x) {
    for (var i = 0; i < 100000; ++i) {}
    return x;
  }
})()(this + "i");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d168a1e^!)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=d168a1e)  
[test/mjsunit/regress/regress-crbug-545364.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-545364.js?cl=d168a1e)  
  

### **crbug:542101**  
   
**[Issue: !HasFastProperties() in src/objects-inl.h](https://crbug.com/542101)**  
**[Commit: Fix Error object value lookups.](https://chromium.googlesource.com/v8/v8/+/1a94bc2)**  
  
Closed: Oct 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-542101.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-542101.js)  
```javascript
(function() {
  Error.prototype.toString.call({
    get name() { return { __proto__: this }; },
    get message() { }
  });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a94bc2^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=1a94bc2)  
[test/mjsunit/regress/regress-crbug-542101.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-542101.js?cl=1a94bc2)  
  

### **crbug:540593**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsScript()) in src/objects-i](https://crbug.com/540593)**  
**[Commit: [turbofan] Don't try to inline non-inlineable functions.](https://chromium.googlesource.com/v8/v8/+/a916059)**  
  
Closed: Oct 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/compiler/regress-crbug-540593.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-crbug-540593.js)  
```javascript
var __f_2 = (function(stdlib) {
  "use asm";
  var __v_3 = stdlib.Symbol;
  function __f_2() { return __v_3(); }
  return __f_2;
})(this);
%OptimizeFunctionOnNextCall(__f_2);
__f_2();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a916059^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=a916059)  
[test/mjsunit/compiler/regress-crbug-540593.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-crbug-540593.js?cl=a916059)  
  

### **crbug:538086**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedArray()) in src/objec](https://crbug.com/538086)**  
**[Commit: Fix FixedArrayBase cast in NumberOfOwnElements](https://chromium.googlesource.com/v8/v8/+/ecf2327)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-538086.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-538086.js)  
```javascript
this[1]++;
for (var x in this) {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecf2327^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ecf2327)  
[test/mjsunit/regress/regress-crbug-538086.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-538086.js?cl=ecf2327)  
  

### **crbug:537444**  
   
**[Issue: No Permission](https://crbug.com/537444)**  
**[Commit: [crankshaft] Fix environment handling after leaving inlined tail call.](https://chromium.googlesource.com/v8/v8/+/792bf2a)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-537444.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-537444.js)  
```javascript
"use strict";

function f(x) {
  return x;
}

function g(x) {
  return false ? 0 : f(x, 1);
}

function h(x) {
  var z = g(x, 1);
  return z + 1;
}


h(1);
h(1);
%OptimizeFunctionOnNextCall(h);
h("a");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/792bf2a^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=792bf2a)  
[src/crankshaft/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.h?cl=792bf2a)  
[src/crankshaft/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/lithium.cc?cl=792bf2a)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=792bf2a)  
[test/mjsunit/regress/regress-crbug-537444.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-537444.js?cl=792bf2a)  
  

### **crbug:530598**  
   
**[Issue: 0u != values.size() in src/compiler/js-inlining.cc](https://crbug.com/530598)**  
**[Commit: [turbofan] Fix JSInliner to handle non-returning bodies.](https://chromium.googlesource.com/v8/v8/+/9e47ec6)**  
  
Closed: Sep 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-530598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-530598.js)  
```javascript
var f1 = (function() {
  "use asm";
  function g() { throw 0; }
  function f() { return g(); }
  return f;
})();
assertThrows("f1()");
%OptimizeFunctionOnNextCall(f1);
assertThrows("f1()");

var f2 = (function() {
  "use asm";
  function g() { for (;;); }
  function f(a) { return a || g(); }
  return f;
})();
assertTrue(f2(true));
%OptimizeFunctionOnNextCall(f2);
assertTrue(f2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e47ec6^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=9e47ec6)  
[test/mjsunit/regress/regress-crbug-530598.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-530598.js?cl=9e47ec6)  
  

### **crbug:528379**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSFunction()) in src/objec](https://crbug.com/528379)**  
**[Commit: Check for validity when accessing call site objects in runtime.](https://chromium.googlesource.com/v8/v8/+/82b3082)**  
  
Closed: Oct 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Recharge", "Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-528379.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-528379.js)  
```javascript
Error.prepareStackTrace = function(e, frames) { return frames; }
assertThrows(function() { new Error().stack[0].getMethodName.call({}); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/82b3082^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=82b3082)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=82b3082)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=82b3082)  
[test/mjsunit/regress-crbug-528379.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-crbug-528379.js?cl=82b3082)  
  

### **crbug:527364**  
   
**[Issue: !isolate->has_pending_exception() in src/compiler.cc](https://crbug.com/527364)**  
**[Commit: [turbofan] Handle stack overflow exceptions in JSInliner.](https://chromium.googlesource.com/v8/v8/+/c505907)**  
  
Closed: Sep 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-527364.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-527364.js)  
```javascript
function module() {
  "use asm";
  var abs = Math.abs;
  function f() {
    return +abs();
  }
  return { f:f };
}

function run_close_to_stack_limit(f) {
  try {
    run_close_to_stack_limit(f);
    f();
  } catch(e) {
  }
}

var boom = module().f;
%OptimizeFunctionOnNextCall(boom)
run_close_to_stack_limit(boom);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c505907^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c505907)  
[test/mjsunit/regress/regress-crbug-527364.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-527364.js?cl=c505907)  
  

### **crbug:523919**  
   
**[Issue: No Permission](https://crbug.com/523919)**  
**[Commit: Serializer: attach alignment to deferred objects.](https://chromium.googlesource.com/v8/v8/+/ee9020d)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-523919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523919.js)  
```javascript
[1.5,
[2.5,
[3.5,
[4.5,
[5.5,
[6.5,
[7.5,
[8.5,
[9.5,
[10.5,
[11.5,
[12.5,
[13.5,
[14.5,
[15.5,
[16.5,
[17.5,
[18.5,
[19.5,
[20.5,
[21.5,
[22.5,
[23.5,
[24.5,
[25.5]]]]]]]]]]]]]]]]]]]]]]]]];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee9020d^!)  
[src/snapshot/serialize.cc](https://cs.chromium.org/chromium/src/v8/src/snapshot/serialize.cc?cl=ee9020d)  
[src/snapshot/serialize.h](https://cs.chromium.org/chromium/src/v8/src/snapshot/serialize.h?cl=ee9020d)  
[test/mjsunit/regress/regress-crbug-523919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523919.js?cl=ee9020d)  
  

### **crbug:523308**  
   
**[Issue: IsFound() in src/lookup.h](https://crbug.com/523308)**  
**[Commit: Message formatting: handle unexpected case of failing property lookup.](https://chromium.googlesource.com/v8/v8/+/2454469)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-4.6", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-523308.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523308.js)  
```javascript
var error;
try { reference_error(); } catch (e) { error = e; }
toString = error.toString;
error.__proto__ = [];
assertEquals("Error: reference_error is not defined", toString.call(error));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2454469^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=2454469)  
[test/mjsunit/regress/regress-crbug-523308.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523308.js?cl=2454469)  
  

### **crbug:523307**  
   
**[Issue: !found in src/lithium-allocator.cc](https://crbug.com/523307)**  
**[Commit: [arm64] Don't try convert binary operation to shifted form when both operands are the same.](https://chromium.googlesource.com/v8/v8/+/85f6e16)**  
  
Closed: Sep 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-523307.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523307.js)  
```javascript
function f(x) {
  var c = x * x << 366;
  var a = c + c;
  return a;
}

f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85f6e16^!)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=85f6e16)  
[test/mjsunit/regress/regress-crbug-523307.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523307.js?cl=85f6e16)  
  

### **crbug:523213**  
   
**[Issue: DescriptorArray::kNotFound != number in src/hydrogen.cc](https://crbug.com/523213)**  
**[Commit: Do not inline array resize operations for outdated prototype maps.](https://chromium.googlesource.com/v8/v8/+/590b3be)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-523213.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523213.js)  
```javascript
var v1 = [];
var v2 = [];
v1.__proto__ = v2;

function f(){
  var a = [];
  for(var i=0; i<2; i++){
    a.push([]);
    a = v2;
  }
}

f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/590b3be^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=590b3be)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=590b3be)  
[test/mjsunit/regress/regress-crbug-523213.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523213.js?cl=590b3be)  
  

### **crbug:522895**  
   
**[Issue: old_target == new_target in src/objects-debug.cc](https://crbug.com/522895)**  
**[Commit: Fix bug in Code::VerifyRecompiledCode.](https://chromium.googlesource.com/v8/v8/+/a683f83)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-522895.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-522895.js)  
```javascript
var body =
  "function bar1(  )  {" +
  "  var i = 35;       " +
  "  while (i-- > 31) {" +
  "    %OptimizeOsr(); " +
  "    j = 9;          " +
  "    while (j-- > 7);" +
  "  }                 " +
  "  return i;         " +
  "}";

function gen() {
  return eval("(" + body + ")");
}

gen()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a683f83^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=a683f83)  
[test/mjsunit/regress/regress-crbug-522895.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-522895.js?cl=a683f83)  
  

### **crbug:522496**  
   
**[Issue: data != NULL in src/api.cc](https://crbug.com/522496)**  
**[Commit: [api] Relax CHECK for ArrayBuffer API abuse](https://chromium.googlesource.com/v8/v8/+/de26ce0)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "merge-merged-46", "M-46", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-522496.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-522496.js)  
```javascript
if (this.Worker) {
  var worker = new Worker("onmessage = function(){}", {type: 'string'});
  var buf = new ArrayBuffer();
  worker.postMessage(buf, [buf]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/de26ce0^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=de26ce0)  
[test/mjsunit/regress/regress-crbug-522496.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-522496.js?cl=de26ce0)  
  

### **crbug:522380**  
   
**[Issue: Stack-overflow in v8::base::Mutex::Lock](https://crbug.com/522380)**  
**[Commit: Make Simulator respect C stack limits as well.](https://chromium.googlesource.com/v8/v8/+/7fb31bd)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Stability-Memory-AddressSanitizer", "merge-merged-4.6", "M-46", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-522380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-522380.js)  
```javascript
var global = this;
global.__defineSetter__('x', function(v) { x = v; });
assertThrows("global.x = 0", RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7fb31bd^!)  
[src/arm/simulator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/simulator-arm.cc?cl=7fb31bd)  
[src/arm/simulator-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/simulator-arm.h?cl=7fb31bd)  
[src/arm64/simulator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/simulator-arm64.cc?cl=7fb31bd)  
[src/arm64/simulator-arm64.h](https://cs.chromium.org/chromium/src/v8/src/arm64/simulator-arm64.h?cl=7fb31bd)  
[src/execution.cc](https://cs.chromium.org/chromium/src/v8/src/execution.cc?cl=7fb31bd)  
[src/execution.h](https://cs.chromium.org/chromium/src/v8/src/execution.h?cl=7fb31bd)  
[src/mips/simulator-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/simulator-mips.cc?cl=7fb31bd)  
[src/mips/simulator-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/simulator-mips.h?cl=7fb31bd)  
[src/mips64/simulator-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/simulator-mips64.cc?cl=7fb31bd)  
[src/mips64/simulator-mips64.h](https://cs.chromium.org/chromium/src/v8/src/mips64/simulator-mips64.h?cl=7fb31bd)  
[test/mjsunit/regress/regress-crbug-522380.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-522380.js?cl=7fb31bd)  
  

### **crbug:518747**  
   
**[Issue: Fatal error in v8::Object::SetAlignedPointerInInternalField](https://crbug.com/518747)**  
**[Commit: [d8 Workers] Make Worker prototype read-only](https://chromium.googlesource.com/v8/v8/+/cd92934)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-518747.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-518747.js)  
```javascript
if (this.Worker) {
  Worker.prototype = 12;
  var __v_6 = new Worker('', {type: 'string'});
  __v_6.postMessage([]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd92934^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=cd92934)  
[test/mjsunit/regress/regress-crbug-518747.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-518747.js?cl=cd92934)  
  

### **crbug:516775**  
   
**[Issue: IsNumber() in src/objects-inl.h:1105](https://crbug.com/516775)**  
**[Commit: Fix Array.prototype.concat for arguments object with getter.](https://chromium.googlesource.com/v8/v8/+/2e0d55a)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-516775.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-516775.js)  
```javascript
function arguments_with_length_getter(f) {
  arguments.__defineGetter__('length', f);
  return arguments;
}

var count = 0;
function increment_count_return() { count++; return "boom"; }
function increment_count_throw() { count++; throw "boom"; }



var a1 = [];
%NormalizeElements(a1);
a1.__proto__ = arguments_with_length_getter(increment_count_return);
[].concat(a1);
assertEquals(0, count);

var a2 = [];
%NormalizeElements(a2);
a2.__proto__ = arguments_with_length_getter(increment_count_throw);
[].concat(a2);
assertEquals(0, count);


var a3 = arguments_with_length_getter(increment_count_return);
a3[Symbol.isConcatSpreadable] = true;
[].concat(a3);
assertEquals(1, count);

var a4 = arguments_with_length_getter(increment_count_throw);
a4[Symbol.isConcatSpreadable] = true;
assertThrows(function() { [].concat(a4); });
assertEquals(2, count);



var a5 = {};
a5.__proto__ = arguments_with_length_getter(increment_count_return);
a5[Symbol.isConcatSpreadable] = true;
[].concat(a5);
assertEquals(3, count);

var a6 = {};
a6.__proto__ = arguments_with_length_getter(increment_count_throw);
a6[Symbol.isConcatSpreadable] = true;
assertThrows(function() { [].concat(a6); });
assertEquals(4, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2e0d55a^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=2e0d55a)  
[test/mjsunit/regress/regress-crbug-516775.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-516775.js?cl=2e0d55a)  
  

### **crbug:516592**  
   
**[Issue: kMaxUInt32 != index_ in src/lookup.h:110](https://crbug.com/516592)**  
**[Commit: Fix off-by-one in Array.concat's max index check](https://chromium.googlesource.com/v8/v8/+/087ae1b)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Reproducible", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-516592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-516592.js)  
```javascript
var i = Math.pow(2, 31);
var a = [];
a[i] = 31;
var b = [];
b[i - 2] = 33;
try {
  
  var c = a.concat(b);
  
  Object.observe(c, function() {});
  c.length = 1;
} catch(e) {
  assertTrue(e instanceof RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/087ae1b^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=087ae1b)  
[test/mjsunit/regress/regress-581.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-581.js?cl=087ae1b)  
[test/mjsunit/regress/regress-crbug-516592.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-516592.js?cl=087ae1b)  
  

### **crbug:515897**  
   
**[Issue: Wrong escaping of forward slashes in RegExp.source](https://crbug.com/515897)**  
**[Commit: [regexp] Fix regexp source escaping with preceding backslashes.](https://chromium.googlesource.com/v8/v8/+/bbb2159)**  
  
Closed: Jul 2016  
Type: Bug  
Components: Blink>JavaScript>Language  
Labels: ["M-47", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-515897.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-515897.js)  
```javascript
var r1 = new RegExp("\\\\/");
assertTrue(r1.test("\\/"));
var r2 = eval("/" + r1.source + "/");
assertEquals("\\\\\\/", r1.source);
assertEquals("\\\\\\/", r2.source);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bbb2159^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=bbb2159)  
[test/mjsunit/regress/regress-3229.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3229.js?cl=bbb2159)  
[test/mjsunit/regress/regress-crbug-515897.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-515897.js?cl=bbb2159)  
[test/webkit/fast/regex/toString-expected.txt](https://cs.chromium.org/chromium/src/v8/test/webkit/fast/regex/toString-expected.txt?cl=bbb2159)  
  

### **crbug:514081**  
   
**[Issue: IsNumber() in src/objects-inl.h:1117](https://crbug.com/514081)**  
**[Commit: [d8 worker] Fix regression when serializing very large arraybuffer](https://chromium.googlesource.com/v8/v8/+/df1f72b)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-514081.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-514081.js)  
```javascript
if (this.Worker) {
  var __v_7 = new Worker('onmessage = function() {};', {type: 'string'});
  var e;
  var ab = new ArrayBuffer(2 * 1000 * 1000);
  try {
    __v_7.postMessage(ab);
    threw = false;
  } catch (e) {
    
    assertContains('cloned', e.message);
    threw = true;
  }
  assertTrue(threw, 'Should throw when trying to serialize large message.');
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df1f72b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=df1f72b)  
[test/mjsunit/regress/regress-crbug-514081.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-514081.js?cl=df1f72b)  
  

### **crbug:513602**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/513602)**  
**[Commit: Fix prototype registration upon SlowToFast migration](https://chromium.googlesource.com/v8/v8/+/c906efd)**  
  
Closed: Jul 2015  
Type: Bug-Security  
Components: Blink>JavaScript>Runtime  
Labels: ["Release-0-M45", "Stability-Crash", "M-45", "M-44", "reward-0", "merge-merged-4.4", "merge-merged-4.5", "Security_Impact-Stable", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-513602.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-513602.js)  
```javascript
function Parent() {}

function Child() {}
Child.prototype = new Parent();
var child = new Child();

function crash() {
  return child.__proto__;
}

crash();
crash();


Parent.prototype.__defineSetter__("foo", function() { print("A"); });
Parent.prototype.__defineSetter__("foo", function() { print("B"); });

crash();


delete Object.prototype.__proto__;
crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c906efd^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c906efd)  
[test/mjsunit/regress/regress-crbug-513602.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-513602.js?cl=c906efd)  
  

### **crbug:513507**  
   
**[Issue: V8: Check failed: new_code_map->get(i + kContextOffset)->IsNativeContext().](https://crbug.com/513507)**  
**[Commit: Introduce safe interface to "copy and grow" FixedArray.](https://chromium.googlesource.com/v8/v8/+/bcad9b5)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-46", "ReleaseBlock-Stable"]  
Regress: [mjsunit/regress/regress-crbug-513507.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-513507.js)  
```javascript
function makeFun() {
  function fun(osr_fuse) {
    for (var i = 0; i < 3; ++i) {
      if (i == osr_fuse) %OptimizeOsr();
    }
    for (var i = 3; i < 6; ++i) {
      if (i == osr_fuse) %OptimizeOsr();
    }
  }
  return fun;
}

makeFun()(7);  
makeFun()(4);  
makeFun()(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bcad9b5^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=bcad9b5)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=bcad9b5)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=bcad9b5)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=bcad9b5)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=bcad9b5)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=bcad9b5)  
[test/cctest/test-heap.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-heap.cc?cl=bcad9b5)  
[test/mjsunit/regress/regress-crbug-513507.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-513507.js?cl=bcad9b5)  
  

### **crbug:513472**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSGlobalObject()) in src/o](https://crbug.com/513472)**  
**[Commit: Rewrite Error.prototype.toString in C++.](https://chromium.googlesource.com/v8/v8/+/2e2765a)**  
  
Closed: Oct 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Recharge", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-513472.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-513472.js)  
```javascript
"use strict";
this.__proto__ = Error();
assertThrows(function() { NaN = 1; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2e2765a^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=2e2765a)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=2e2765a)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=2e2765a)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=2e2765a)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=2e2765a)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=2e2765a)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=2e2765a)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=2e2765a)  
[test/mjsunit/error-constructors.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/error-constructors.js?cl=2e2765a)  
[test/mjsunit/regress/regress-crbug-513472.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-513472.js?cl=2e2765a)  
  

### **crbug:513471**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsOddball()) in src/objects-](https://crbug.com/513471)**  
**[Commit: Fix resuming generator marked for optimization.](https://chromium.googlesource.com/v8/v8/+/6ab9c18)**  
  
Closed: Apr 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Recharge", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-513471.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-513471.js)  
```javascript
var g = (function*(){});
var f = g();
%OptimizeFunctionOnNextCall(g);
f.next();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6ab9c18^!)  
[src/runtime/runtime-generator.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-generator.cc?cl=6ab9c18)  
[test/mjsunit/regress/regress-crbug-513471.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-513471.js?cl=6ab9c18)  
  

### **crbug:511880**  
   
**[Issue: Fatal error in v8::External::Cast](https://crbug.com/511880)**  
**[Commit: [d8 Workers] Fix bug creating Worker during main thread termination](https://chromium.googlesource.com/v8/v8/+/a87db3d)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-511880.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-511880.js)  
```javascript
if (this.Worker) {
  var __v_8 =
    `var __v_9 = new Worker('postMessage(42)', {type: 'string'});
     onmessage = function(parentMsg) {
       __v_9.postMessage(parentMsg);
     };`;
  var __v_9 = new Worker(__v_8, {type: 'string'});
  __v_9.postMessage(9);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a87db3d^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=a87db3d)  
[src/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8.h?cl=a87db3d)  
[test/mjsunit/regress/regress-crbug-511880.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-511880.js?cl=a87db3d)  
  

### **crbug:510738**  
   
**[Issue: length() >= kReservedIndexCount in src/type-feedback-vector.h:102](https://crbug.com/510738)**  
**[Commit: Crankshaft part of the 'loads and stores to global vars through property cell shortcuts' feature.](https://chromium.googlesource.com/v8/v8/+/cc66a1c)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-510738.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-510738.js)  
```javascript
function check(f, result) {
  %OptimizeFunctionOnNextCall(f);
  assertEquals(result, f());
}

var x = 17;
function generic_load() { return x; }
check(generic_load, 17);

function generic_store() { x = 13; return x; }
check(generic_store, 13);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cc66a1c^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=cc66a1c)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=cc66a1c)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=cc66a1c)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=cc66a1c)  
[src/arm64/lithium-arm64.h](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.h?cl=cc66a1c)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=cc66a1c)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=cc66a1c)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=cc66a1c)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=cc66a1c)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=cc66a1c)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=cc66a1c)  
[src/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.h?cl=cc66a1c)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=cc66a1c)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=cc66a1c)  
[src/mips/lithium-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.h?cl=cc66a1c)  
[src/mips64/lithium-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/lithium-codegen-mips64.cc?cl=cc66a1c)  
[src/mips64/lithium-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/lithium-mips64.cc?cl=cc66a1c)  
[src/mips64/lithium-mips64.h](https://cs.chromium.org/chromium/src/v8/src/mips64/lithium-mips64.h?cl=cc66a1c)  
[src/ppc/lithium-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/lithium-codegen-ppc.cc?cl=cc66a1c)  
[src/ppc/lithium-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/lithium-ppc.cc?cl=cc66a1c)  
[src/ppc/lithium-ppc.h](https://cs.chromium.org/chromium/src/v8/src/ppc/lithium-ppc.h?cl=cc66a1c)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=cc66a1c)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=cc66a1c)  
[src/x64/lithium-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.h?cl=cc66a1c)  
[src/x87/lithium-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/lithium-codegen-x87.cc?cl=cc66a1c)  
[src/x87/lithium-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/lithium-x87.cc?cl=cc66a1c)  
[src/x87/lithium-x87.h](https://cs.chromium.org/chromium/src/v8/src/x87/lithium-x87.h?cl=cc66a1c)  
[test/mjsunit/regress/regress-crbug-510738.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-510738.js?cl=cc66a1c)  
  

### **crbug:510426**  
   
**[Issue: ContainsOnlyValidKeys(content) in src/objects.cc:6159](https://crbug.com/510426)**  
**[Commit: Fix element enumeration on String wrappers with dictionary elements](https://chromium.googlesource.com/v8/v8/+/e6cb6bb)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-510426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-510426.js)  
```javascript
var s = new String('a');
s[10000000] = 'bente';
assertEquals(['0', '10000000'], Object.keys(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6cb6bb^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e6cb6bb)  
[test/mjsunit/regress/regress-crbug-510426.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-510426.js?cl=e6cb6bb)  
  

### **crbug:506956**  
   
**[Issue: !isolate->has_pending_exception() in src/compiler.cc:857](https://crbug.com/506956)**  
**[Commit: Fixed a couple of proxies-related unhandled exceptions.](https://chromium.googlesource.com/v8/v8/+/52b3e41)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-506956.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-506956.js)  
```javascript
try {
  var p = new Proxy({}, {
      getPropertyDescriptor: function() { throw "boom"; }
  });
  var o = Object.create(p);
  with (o) { delete unresolved_name; }
} catch(e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52b3e41^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=52b3e41)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=52b3e41)  
[test/mjsunit/regress/regress-crbug-505907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505907.js?cl=52b3e41)  
[test/mjsunit/regress/regress-crbug-506956.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-506956.js?cl=52b3e41)  
  

### **crbug:506549**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (static_cast<unsigned>(i) < static_ca](https://crbug.com/506549)**  
**[Commit: Fix cluster-fuzz found regression with d8 Workers](https://chromium.googlesource.com/v8/v8/+/54920cd)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-506549.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-506549.js)  
```javascript
if (this.Worker) {
  var __v_5 = {};
  __v_5.__defineGetter__('byteLength', function() {foo();});
  var __v_8 = new Worker('onmessage = function() {};', {type: 'string'});
  assertThrows(function() { __v_8.postMessage(__v_5); });
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/54920cd^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=54920cd)  
[test/mjsunit/regress/regress-crbug-506549.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-506549.js?cl=54920cd)  
  

### **crbug:505907**  
   
**[Issue: !isolate->has_pending_exception() in src/contexts.cc:273](https://crbug.com/505907)**  
**[Commit: Fixed a couple of proxies-related unhandled exceptions.](https://chromium.googlesource.com/v8/v8/+/52b3e41)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-505907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-505907.js)  
```javascript
try {
  var p = new Proxy({}, {
      getPropertyDescriptor: function() { return [] }
    });
  var o = Object.create(p);
  with (o) { unresolved_name() }
} catch(e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52b3e41^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=52b3e41)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=52b3e41)  
[test/mjsunit/regress/regress-crbug-505907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505907.js?cl=52b3e41)  
[test/mjsunit/regress/regress-crbug-506956.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-506956.js?cl=52b3e41)  
  

### **crbug:505778**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (static_cast<unsigned>(i) < static_ca](https://crbug.com/505778)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers](https://chromium.googlesource.com/v8/v8/+/abaa094)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-505778.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-505778.js)  
```javascript
if (this.Worker) {
  var __v_7 = new Worker('onmessage = function() {}', {type: 'string'});
  __v_7.postMessage("");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/abaa094^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=abaa094)  
[test/mjsunit/regress/regress-crbug-505778.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505778.js?cl=abaa094)  
  

### **crbug:505370**  
   
**[Issue: !name->AsArrayIndex(&index) in src/lookup.h:65](https://crbug.com/505370)**  
**[Commit: Use correct LookupIterator in CallSite::GetMethodName.](https://chromium.googlesource.com/v8/v8/+/4f9cf2b)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-505370.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-505370.js)  
```javascript
var o = {
  get 0() { reference_error;  },
  get length() { return 1; }
};

var method_name;

try {
  o[0];
} catch (e) {
  thrown = true;
  Error.prepareStackTrace = function(exception, frames) { return frames; };
  var frames = e.stack;
  Error.prepareStackTrace = undefined;
  method_name = frames[0].getMethodName();
}

assertEquals("0", method_name);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f9cf2b^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=4f9cf2b)  
[test/mjsunit/regress/regress-crbug-505370.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505370.js?cl=4f9cf2b)  
  

### **crbug:505354**  
   
**[Issue: UNKNOWN in v8::internal::Execution::ToObject](https://crbug.com/505354)**  
**[Commit: [turbofan] Fix exit control flow in TryCatchBuilder.](https://chromium.googlesource.com/v8/v8/+/df06f1c)**  
  
Closed: Jun 2015  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "M-45", "Stability-Memory-AddressSanitizer", "findit-wrong", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-505354.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-505354.js)  
```javascript
function f() {
  "use strict";
  try {
    for (let i = 0; i < 10; i++) {}
  } catch(e) {}
}
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df06f1c^!)  
[src/compiler/control-builders.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-builders.cc?cl=df06f1c)  
[test/mjsunit/regress/regress-crbug-505354.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505354.js?cl=df06f1c)  
  

### **crbug:504787**  
   
**[Issue: script->FindSharedFunctionInfo(literal).is_null() in src/compiler.cc:1339](https://crbug.com/504787)**  
**[Commit: Mark function info as compiled after EnsureDeoptimizationSupport.](https://chromium.googlesource.com/v8/v8/+/8c72792)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-504787.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-504787.js)  
```javascript
function f() {
  "use asm";
  function g() {
    function f() {};
  }
  return g;
}

f()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c72792^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=8c72792)  
[src/compiler.h](https://cs.chromium.org/chromium/src/v8/src/compiler.h?cl=8c72792)  
[test/mjsunit/regress/regress-crbug-504787.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-504787.js?cl=8c72792)  
  

### **crbug:504729**  
   
**[Issue: UNKNOWN in strlen](https://crbug.com/504729)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers.](https://chromium.googlesource.com/v8/v8/+/e291b78)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-504729.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-504729.js)  
```javascript
if (this.Worker) {
  Function.prototype.toString = "foo";
  function __f_7() {}
  assertThrows(function() { var __v_5 = new Worker(__f_7.toString(), {type: 'string'}) });
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e291b78^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=e291b78)  
[test/mjsunit/regress/regress-crbug-504729.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-504729.js?cl=e291b78)  
  

### **crbug:504727**  
   
**[Issue: UNKNOWN in v8::internal::Object::GetProperty](https://crbug.com/504727)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers.](https://chromium.googlesource.com/v8/v8/+/93c4352)**  
  
Closed: Jun 2015  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["ReleaseBlock-Beta", "Merge-na", "M-45", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-504727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-504727.js)  
```javascript
if (this.Worker) {
  var __v_2 = new Worker('', {type: 'string'});
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93c4352^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=93c4352)  
[test/mjsunit/regress/regress-crbug-504727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-504727.js?cl=93c4352)  
  

### **crbug:504136**  
   
**[Issue: UNKNOWN in RtlEncodePointer+0x1eb](https://crbug.com/504136)**  
**[Commit: Fix cluster-fuzz regression when getting message from Worker](https://chromium.googlesource.com/v8/v8/+/28b0129)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "M-45", "Stability-Memory-AddressSanitizer", "Findit-for-crash", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-504136.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-504136.js)  
```javascript
if (this.Worker) {
  var __v_10 = new Worker('', {type: 'string'});
  __v_10.terminate();
 __v_10.getMessage();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/28b0129^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=28b0129)  
[src/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8.h?cl=28b0129)  
[test/mjsunit/d8-worker-spawn-worker.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/d8-worker-spawn-worker.js?cl=28b0129)  
[test/mjsunit/d8-worker.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/d8-worker.js?cl=28b0129)  
[test/mjsunit/regress/regress-crbug-504136.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-504136.js?cl=28b0129)  
  

### **crbug:503991**  
   
**[Issue: Fatal error in ../../src/vector.h,](https://crbug.com/503991)**  
**[Commit: Fix cluster-fuzz regression with Workers when serializing empty string](https://chromium.googlesource.com/v8/v8/+/b3bd728)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-503991.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-503991.js)  
```javascript
if (this.Worker) {
  __v_3 = "";
  var __v_6 = new Worker('', {type: 'string'});
  __v_6.postMessage(__v_3);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b3bd728^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=b3bd728)  
[test/mjsunit/regress/regress-crbug-503991.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-503991.js?cl=b3bd728)  
  

### **crbug:503968**  
   
**[Issue: UNKNOWN in v8::internal::MemoryChunk::heap](https://crbug.com/503968)**  
**[Commit: Fix cluster-fuzz regression with Workers and recursive serialization](https://chromium.googlesource.com/v8/v8/+/5023335)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-503968.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-503968.js)  
```javascript
if (this.Worker) {
  function __f_0() { this.s = new Object(); }
  function __f_1() {
    this.l = [new __f_0, new __f_0];
  }
  __v_6 = new __f_1;
  var __v_9 = new Worker('', {type: 'string'});
  __v_9.postMessage(__v_6);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5023335^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=5023335)  
[test/mjsunit/regress/regress-crbug-503968.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-503968.js?cl=5023335)  
  

### **crbug:503698**  
   
**[Issue: Fatal error in ../../src/isolate.h,](https://crbug.com/503698)**  
**[Commit: Fix cluster-fuzz regression with Workers on mips.debug](https://chromium.googlesource.com/v8/v8/+/627627b)**  
  
Closed: Aug 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-503698.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-503698.js)  
```javascript
if (this.Worker) {
  var __v_6 = new Worker('', {type: 'string'});
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/627627b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=627627b)  
[test/mjsunit/regress/regress-crbug-503698.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-503698.js?cl=627627b)  
  

### **crbug:503578**  
   
**[Issue: Fatal error in ../../src/d8.cc,](https://crbug.com/503578)**  
**[Commit: Fix cluster-fuzz found regression in d8 when deserializing ArrayBuffer](https://chromium.googlesource.com/v8/v8/+/10b6af7)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-503578.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-503578.js)  
```javascript
if (this.Worker) {
  function __f_0(byteLength) {
    var __v_1 = new ArrayBuffer(byteLength);
    var __v_5 = new Uint32Array(__v_1);
    return __v_5;
  }
  var __v_6 = new Worker('onmessage = function() {}', {type: 'string'});
  var __v_3 = __f_0(16);
  __v_6.postMessage(__v_3);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10b6af7^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=10b6af7)  
[test/mjsunit/regress/regress-crbug-503578.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-503578.js?cl=10b6af7)  
  

### **crbug:502930**  
   
**[Issue: Fatal error in ../../src/objects-debug.cc,](https://crbug.com/502930)**  
**[Commit: Map::ReconfigureProperty() should mark map as unstable when it returns a different map.](https://chromium.googlesource.com/v8/v8/+/4742176)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-43", "M-44", "merge-merged-4.4", "Merge-Merged-4.3", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-502930.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-502930.js)  
```javascript
var accessor_to_data_case = (function() {
  var v = {};
  Object.defineProperty(v, "foo", { get: function() { return 42; }, configurable: true});

  var obj = {};
  obj["boom"] = v;

  Object.defineProperty(v, "foo", { value: 0, writable: true, configurable: true });
  return obj;
})();


var data_to_accessor_case = (function() {
  var v = {};
  Object.defineProperty(v, "bar", { value: 0, writable: true, configurable: true });

  var obj = {};
  obj["bam"] = v;

  Object.defineProperty(v, "bar", { get: function() { return 42; }, configurable: true});
  return obj;
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4742176^!)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=4742176)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4742176)  
[test/cctest/test-migrations.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-migrations.cc?cl=4742176)  
[test/mjsunit/regress/regress-crbug-502930.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-502930.js?cl=4742176)  
  

### **crbug:501809**  
   
**[Issue: UNKNOWN in int v8::internal::CompareExchangeSeqCst<int>](https://crbug.com/501809)**  
**[Commit: Fix cluster-fuzz bug introduced in refs/heads/master@{#28796}](https://chromium.googlesource.com/v8/v8/+/e6fed5e)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-501809.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-501809.js)  
```javascript
var sab = new SharedArrayBuffer(8);
var ta = new Int32Array(sab);
ta.__defineSetter__('length', function() {;});
assertThrows(function() {
  Atomics.compareExchange(ta, 4294967295, 0, 0);
}, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6fed5e^!)  
[src/runtime/runtime-atomics.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-atomics.cc?cl=e6fed5e)  
[test/mjsunit/regress/regress-crbug-501809.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-501809.js?cl=e6fed5e)  
  

### **crbug:501808**  
   
**[Issue: Direct-leak in uprv_malloc_54](https://crbug.com/501808)**  
**[Commit: Global handle leak in Realm.create() fixed.](https://chromium.googlesource.com/v8/v8/+/5c4aae3)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-LeakSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-501808.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-501808.js)  
```javascript
var r = Realm.create();
assertEquals(0, "a".localeCompare("a"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c4aae3^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=5c4aae3)  
[test/mjsunit/regress/regress-crbug-501808.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-501808.js?cl=5c4aae3)  
  

### **crbug:501711**  
   
**[Issue: Fatal error in ../../src/compiler.cc,](https://crbug.com/501711)**  
**[Commit: Fixed exception handling in Realm.create().](https://chromium.googlesource.com/v8/v8/+/bcb276c)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-501711.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-501711.js)  
```javascript
function f() {
  try {
    f();
  } catch(e) {
    try {
      Realm.create();
    } catch (e) {
      quit();
    }
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bcb276c^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=bcb276c)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=bcb276c)  
[test/mjsunit/regress/regress-crbug-501711.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-501711.js?cl=bcb276c)  
  

### **crbug:500824**  
   
**[Issue: Fatal error in ../../src/zone.h,](https://crbug.com/500824)**  
**[Commit: [turbofan] Work around negative parameter count.](https://chromium.googlesource.com/v8/v8/+/21a1975)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-500824.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-500824.js)  
```javascript
function get_thrower() {
  "use strict";
  return Object.getOwnPropertyDescriptor(arguments, "callee").get;
}

var f = (function(v) {
  "use asm";
  function fun() {
    switch (v) {}
  }
  return {
    fun: fun
  };
})(get_thrower()).fun;

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21a1975^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=21a1975)  
[test/mjsunit/regress/regress-crbug-500824.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500824.js?cl=21a1975)  
  

### **crbug:500497**  
   
**[Issue: No Permission](https://crbug.com/500497)**  
**[Commit: Hydrogen object literals: always initialize in-object properties](https://chromium.googlesource.com/v8/v8/+/5fca394)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-500497.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-500497.js)  
```javascript
var global = [];  

function Ctor() {
  var result = {a: {}, b: {}, c: {}, d: {}, e: {}, f: {}, g: {}};
  return result;
}

gc();

for (var i = 0; i < 120; i++) {
  
  global.push(Ctor().a);
  (function FillNewSpace() { new Array(10000); })();
}


assertUnoptimized(Ctor);





%OptimizeFunctionOnNextCall(Ctor);
for (var i = 0; i < 10000; i++) {
  
  
  Ctor();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5fca394^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=5fca394)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=5fca394)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=5fca394)  
[test/mjsunit/regress/regress-crbug-500497.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500497.js?cl=5fca394)  
  

### **crbug:500435**  
   
**[Issue: Regression: 'Letterless' app is not working properly.](https://crbug.com/500435)**  
**[Commit: [crankshaft] Fix wrong bailout points in for-in loop body.](https://chromium.googlesource.com/v8/v8/+/45439b9)**  
  
Closed: Jun 2015  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-45", "Needs-Feedback", "ReleaseBlock-Stable"]  
Regress: [mjsunit/regress/regress-crbug-500435.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-500435.js)  
```javascript
function bar(a) {
  delete a[1];
}

function foo(a) {
  var d;
  for (d in a) {
    assertFalse(d === undefined);
    bar(a);
  }
}

foo([1,2]);
foo([2,3]);
%OptimizeFunctionOnNextCall(foo);
foo([1,2]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/45439b9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=45439b9)  
[test/mjsunit/regress/regress-crbug-500435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500435.js?cl=45439b9)  
  

### **crbug:498811**  
   
**[Issue: Unresolved "this" reference.](https://crbug.com/498811)**  
**[Commit: Add script context with context-allocated "const this"](https://chromium.googlesource.com/v8/v8/+/103fcfa)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-498811.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-498811.js)  
```javascript
"use strict";
let l = 0;
eval("eval('this')");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/103fcfa^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=103fcfa)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=103fcfa)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=103fcfa)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=103fcfa)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=103fcfa)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=103fcfa)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=103fcfa)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=103fcfa)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=103fcfa)  
[src/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/full-codegen-ppc.cc?cl=103fcfa)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=103fcfa)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=103fcfa)  
[src/scopeinfo.cc](https://cs.chromium.org/chromium/src/v8/src/scopeinfo.cc?cl=103fcfa)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=103fcfa)  
[src/snapshot/snapshot-common.cc](https://cs.chromium.org/chromium/src/v8/src/snapshot/snapshot-common.cc?cl=103fcfa)  
[src/variables.h](https://cs.chromium.org/chromium/src/v8/src/variables.h?cl=103fcfa)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=103fcfa)  
[src/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/full-codegen-x87.cc?cl=103fcfa)  
[test/cctest/test-serialize.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-serialize.cc?cl=103fcfa)  
[test/mjsunit/regress/regress-crbug-498811.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-498811.js?cl=103fcfa)  
[test/unittests/compiler/js-type-feedback-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-type-feedback-unittest.cc?cl=103fcfa)  
  

### **crbug:498022**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/498022)**  
**[Commit: Fix clobbered register when setting this_function variable.](https://chromium.googlesource.com/v8/v8/+/bf2bbc8)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "M-45", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-498022.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-498022.js)  
```javascript
"use strict";
class Base {
}
class Derived extends Base {
  constructor() {
    eval();
  }
}
assertThrows("new Derived()", ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf2bbc8^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=bf2bbc8)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=bf2bbc8)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=bf2bbc8)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=bf2bbc8)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=bf2bbc8)  
[src/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/full-codegen-ppc.cc?cl=bf2bbc8)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=bf2bbc8)  
[src/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/full-codegen-x87.cc?cl=bf2bbc8)  
[test/mjsunit/regress/regress-crbug-498022.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-498022.js?cl=bf2bbc8)  
  

### **crbug:495493**  
   
**[Issue: UNKNOWN in v8::internal::Context::global_object](https://crbug.com/495493)**  
**[Commit: [crankshaft] do not sign-extend int32 immediate in DoMathMinMax.](https://chromium.googlesource.com/v8/v8/+/7c3cad2)**  
  
Closed: Jun 2016  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-495493.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-495493.js)  
```javascript
function foo(p) {
  for (var i = 0; i < 100000; ++i) {
    p = Math.min(-1, 0);
  }
}
foo(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c3cad2^!)  
[src/crankshaft/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-codegen-x64.cc?cl=7c3cad2)  
[test/mjsunit/regress/regress-crbug-495493.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-495493.js?cl=7c3cad2)  
  

### **crbug:493779**  
   
**[Issue: UNKNOWN in v8::internal::Heap::CreateFillerObjectAt](https://crbug.com/493779)**  
**[Commit: Fix bogus insertion of filler in LO-space by String#replace.](https://chromium.googlesource.com/v8/v8/+/d207fce)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-493779.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-493779.js)  
```javascript
var s = "\u1234-------";
for (var i = 0; i < 17; i++) {
  s += s;
}
s.replace(/[\u1234]/g, "");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d207fce^!)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=d207fce)  
[test/mjsunit/regress/regress-crbug-493779.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-493779.js?cl=d207fce)  
  

### **crbug:493290**  
   
**[Issue: Fatal error in ../../src/objects-inl.h,](https://crbug.com/493290)**  
**[Commit: Drop computed handler count and index from AST.](https://chromium.googlesource.com/v8/v8/+/c14ba5e)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-493290.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-493290.js)  
```javascript
function f() {
  throw "boom";
  try {} catch (e) {}
}
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c14ba5e^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c14ba5e)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=c14ba5e)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=c14ba5e)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=c14ba5e)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=c14ba5e)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c14ba5e)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=c14ba5e)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=c14ba5e)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=c14ba5e)  
[src/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/full-codegen-ppc.cc?cl=c14ba5e)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=c14ba5e)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=c14ba5e)  
[src/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/full-codegen-x87.cc?cl=c14ba5e)  
[test/mjsunit/regress/regress-crbug-493290.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-493290.js?cl=c14ba5e)  
  

### **crbug:493284**  
   
**[Issue: Direct-leak in uprv_malloc_54](https://crbug.com/493284)**  
**[Commit: Fixed memory-leak in d8. It did not clean evaluation context used for executing shell commands.](https://chromium.googlesource.com/v8/v8/+/405844b)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-LeakSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-493284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-493284.js)  
```javascript
if (this.Intl) {
  var coll = new Intl.Collator();
  assertEquals(-1, coll.compare('a', 'c'));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/405844b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=405844b)  
[src/d8.h](https://cs.chromium.org/chromium/src/v8/src/d8.h?cl=405844b)  
[test/mjsunit/regress/regress-crbug-493284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-493284.js?cl=405844b)  
  

### **crbug:491062**  
   
**[Issue: Fatal error in ../../v8/src/isolate.cc,](https://crbug.com/491062)**  
**[Commit: Fixed a couple of failing DCHECK(has_pending_exception()).](https://chromium.googlesource.com/v8/v8/+/62b5650)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-491062.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-491062.js)  
```javascript
function g() {}

var count = 0;
function f() {
  try {
    f();
  } catch(e) {
    print(e.stack);
  }
  if (count < 100) {
    count++;
    %DebugGetLoadedScriptIds();
  }
}
f();
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/62b5650^!)  
[src/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug.cc?cl=62b5650)  
[src/runtime/runtime-debug.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-debug.cc?cl=62b5650)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=62b5650)  
[test/mjsunit/regress/regress-crbug-491062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-491062.js?cl=62b5650)  
  

### **crbug:490680**  
   
**[Issue: toString() is called on values when they are thrown](https://crbug.com/490680)**  
**[Commit: Do not eagerly convert exception to string when creating a message object](https://chromium.googlesource.com/v8/v8/+/36d8363)**  
  
Closed: Dec 2015  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Hotlist-Slow", "M-43", "Hotlist-V8-Recharge", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-490680.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-490680.js)  
```javascript
var sentinel = null;

try {
  throw { toString: function() { sentinel = "observed"; } };
} catch (e) {
}

L: try {
  throw { toString: function() { sentinel = "observed"; } };
} finally {
  break L;
}

assertNull(sentinel);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/36d8363^!)  
[src/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug.cc?cl=36d8363)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=36d8363)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=36d8363)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=36d8363)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=36d8363)  
[test/mjsunit/regress/regress-crbug-490680.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-490680.js?cl=36d8363)  
  

### **crbug:490021**  
   
**[Issue: Fatal error in ../../v8/src/lithium-codegen.cc,](https://crbug.com/490021)**  
**[Commit: [arm64] Fixed unnecessary environment assignment to LSmiTag instruction.](https://chromium.googlesource.com/v8/v8/+/b625d4d)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-490021.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-490021.js)  
```javascript
var global = new Object(3);
function f() {
  global[0] = global[0] >>> 15.5;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b625d4d^!)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=b625d4d)  
[test/mjsunit/regress/regress-crbug-490021.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-490021.js?cl=b625d4d)  
  

### **crbug:489597**  
   
**[Issue: Fatal error in ../../v8/src/ic/ic.cc,](https://crbug.com/489597)**  
**[Commit: Fixed DCHECK in StoreIC::CompileHandler().](https://chromium.googlesource.com/v8/v8/+/1c673a5)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-489597.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-489597.js), [mjsunit/regress/regress-crbug-489597.js-script](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-489597.js-script)  
```javascript
try {
  load("test/mjsunit/regress/regress-crbug-489597.js-script");
} catch (e) {
}

var o = this;
Error.captureStackTrace(o);
o.stack;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1c673a5^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=1c673a5)  
[test/mjsunit/regress/regress-crbug-489597.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-489597.js?cl=1c673a5)  
[test/mjsunit/regress/regress-crbug-489597.js-script](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-489597.js-script?cl=1c673a5)  
  

### **crbug:489293**  
   
**[Issue: Fatal error in ../../v8/src/compiler/code-generator.cc,](https://crbug.com/489293)**  
**[Commit: [turbofan] Fix over-restictive assertion in code generator.](https://chromium.googlesource.com/v8/v8/+/7bd2d3e)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-489293.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-489293.js)  
```javascript
function f() {
  var x = 0;
  for (var y = 0; y < 0; ++y) {
    x = (x + y) | 0;
  }
  return unbound;
}
%OptimizeFunctionOnNextCall(f);
assertThrows(f, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bd2d3e^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=7bd2d3e)  
[test/mjsunit/regress/regress-crbug-489293.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-489293.js?cl=7bd2d3e)  
  

### **crbug:487608**  
   
**[Issue: WebGL three.js example runs into V8 exception](https://crbug.com/487608)**  
**[Commit: Fix harmless HGraph verification failure after hoisting inlined bounds checks](https://chromium.googlesource.com/v8/v8/+/f817520)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-487608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-487608.js)  
```javascript
function inlined(a, i) {
  return a[i + 1];
}

function foo(index) {
  var a = [0, 1, 2, 3];
  var result = 0;
  result += a[index];
  result += inlined(a, index);
  return result;
}

foo(0);
foo(0);
%OptimizeFunctionOnNextCall(foo);
foo(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f817520^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=f817520)  
[test/mjsunit/regress/regress-crbug-487608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487608.js?cl=f817520)  
  

### **crbug:487322**  
   
**[Issue: DateTimeFormat does not resolve IANA time zone](https://crbug.com/487322)**  
**[Commit: Timezone name check fix](https://chromium.googlesource.com/v8/v8/+/4e18190)**  
  
Closed: Dec 2015  
Type: Bug  
Components: Blink>JavaScript>Internationalization  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-487322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-487322.js)  
```javascript
if (this.Intl) {
  
  
  
  
  
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Katmandu'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Kathmandu'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  
  
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ulaanbaatar'})
  assertEquals('Asia/Ulaanbaatar', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ulan_Bator'})
  assertEquals('Asia/Ulaanbaatar', df.resolvedOptions().timeZone);

  
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'Aurope/Paris'}));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e18190^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4e18190)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=4e18190)  
[test/mjsunit/regress/regress-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-487322.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-364374.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-364374.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487322.js?cl=4e18190)  
  

### **crbug:487105**  
   
**[Issue: Fatal error in ../../v8/src/x64/full-codegen-x64.cc,](https://crbug.com/487105)**  
**[Commit: Another regression test for resolving references to "this" in strict mode.](https://chromium.googlesource.com/v8/v8/+/18b6059)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-487105.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-487105.js)  
```javascript
"use strict";
function assertThrows() {
  eval();
};
eval("delete this;")  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18b6059^!)  
[test/mjsunit/regress/regress-crbug-487105.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487105.js?cl=18b6059)  
  

### **crbug:485548**  
   
**[Issue: Fatal error in ../../v8/src/objects-debug.cc,](https://crbug.com/485548)**  
**[Commit: Map::ReconfigureProperty() should mark map as unstable when there is an element kind transition somewhere in the middle of the transition tree.](https://chromium.googlesource.com/v8/v8/+/3c1487d)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-43", "M-44", "merge-merged-4.4", "Merge-Merged-4.3", "Clusterfuzz", "M-42"]  
Regress: [mjsunit/regress/regress-crbug-485548-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485548-1.js), [mjsunit/regress/regress-crbug-485548-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485548-2.js)  
```javascript
var inner = new Array();
inner.a = {x:1};
inner[0] = 1.5;
inner.b = {x:2};
assertTrue(%HasDoubleElements(inner));

function foo(o) {
  return o.field.a.x;
}

var outer = {};
outer.field = inner;
foo(outer);
foo(outer);
foo(outer);
%OptimizeFunctionOnNextCall(foo);
foo(outer);


var v = { get x() { return 0x7fffffff; } };
inner.a = v;

gc();

var boom = foo(outer);
print(boom);
assertEquals(0x7fffffff, boom);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c1487d^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=3c1487d)  
[test/mjsunit/regress/regress-crbug-485548-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-485548-1.js?cl=3c1487d)  
[test/mjsunit/regress/regress-crbug-485548-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-485548-2.js?cl=3c1487d)  
  

### **crbug:485410**  
   
**[Issue: Google Sheets crashes when scrolling with "illegal access"](https://crbug.com/485410)**  
**[Commit: Let Runtime_GrowArrayElements accept non-Smi numbers as |key|.](https://chromium.googlesource.com/v8/v8/+/f10b992)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript>Compiler  
Labels: ["M-44", "Via-Wizard", "Hotlist-Google", "ReleaseBlock-Stable", "Hotlist-Partner-GSuite"]  
Regress: [mjsunit/regress/regress-crbug-485410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485410.js)  
```javascript
var doubles = new Float64Array(1);
function ToHeapNumber(i) {
  doubles[0] = i;
  return doubles[0];
}
for (var i = 0; i < 3; i++) ToHeapNumber(i);
%OptimizeFunctionOnNextCall(ToHeapNumber);
ToHeapNumber(1);

function Fail(a, i, v) {
  a[i] = v;
}

for (var i = 0; i < 3; i++) Fail(new Array(1), 1, i);
%OptimizeFunctionOnNextCall(Fail);

Fail(new Array(1), ToHeapNumber(1050), 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f10b992^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=f10b992)  
[test/mjsunit/regress/regress-crbug-485410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-485410.js?cl=f10b992)  
  

### **crbug:484077**  
   
**[Issue: Bogus expectation for inspector/sources/debugger/properties-special.html](https://crbug.com/484077)**  
**[Commit: Set inferred name of bound function to empty string.](https://chromium.googlesource.com/v8/v8/+/f42544b)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-484077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-484077.js)  
```javascript
assertEquals("", %FunctionGetInferredName((function(){}).bind({})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f42544b^!)  
[src/runtime/runtime-function.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-function.cc?cl=f42544b)  
[test/mjsunit/regress/regress-crbug-484077.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-484077.js?cl=f42544b)  
  

### **crbug:482998**  
   
**[Issue: Regexp performance tanks on unicode inputs](https://crbug.com/482998)**  
**[Commit: Reland II of 'Optimize trivial regexp disjunctions' CL 1176453002](https://chromium.googlesource.com/v8/v8/+/05507cc)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript>Runtime  
Labels: ["Performance", "Via-Wizard", "Arch-x86_64"]  
Regress: [mjsunit/regress/regress-crbug-482998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-482998.js)  
```javascript
function collapse(flags) {
  var src = "(?:";
  for (var i = 128; i < 0x1000; i++) {
    src += String.fromCharCode(96 + i % 26) + String.fromCharCode(i) + "|";
  }
  src += "aa)";
  var collapsible = new RegExp(src, flags);
  var subject = "zzzzzzz" + String.fromCharCode(3000);
  for (var i = 0; i < 1000; i++) {
    subject += "xxxxxxx";
  }
  for (var i = 0; i < 2000; i++) {
    assertFalse(collapsible.test(subject));
  }
}

collapse("i");
collapse("");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/05507cc^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=05507cc)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=05507cc)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=05507cc)  
[src/list-inl.h](https://cs.chromium.org/chromium/src/v8/src/list-inl.h?cl=05507cc)  
[src/list.h](https://cs.chromium.org/chromium/src/v8/src/list.h?cl=05507cc)  
[src/vector.h](https://cs.chromium.org/chromium/src/v8/src/vector.h?cl=05507cc)  
[test/cctest/test-heap.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-heap.cc?cl=05507cc)  
[test/mjsunit/regress/regress-crbug-482998.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-482998.js?cl=05507cc)  
  

### **crbug:480819**  
   
**[Issue: Fatal error in ../../v8/src/compiler/verifier.cc,](https://crbug.com/480819)**  
**[Commit: [turbofan] Fix frame state for class literal definition.](https://chromium.googlesource.com/v8/v8/+/6b60f19)**  
  
Closed: Apr 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-480819.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-480819.js)  
```javascript
(function() {
  "use strict";
  class C1 {}
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6b60f19^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=6b60f19)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=6b60f19)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=6b60f19)  
[test/mjsunit/regress/regress-crbug-480819.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-480819.js?cl=6b60f19)  
  

### **crbug:480807**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::mark](https://crbug.com/480807)**  
**[Commit: [turbofan] Ignore dead cached nodes in the JSGraph.](https://chromium.googlesource.com/v8/v8/+/4f9bc2d)**  
  
Closed: Apr 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-480807.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-480807.js)  
```javascript
function foo() {
  var c = 0;
  for (var e = 0; e < 1; ++e) {
    for (var a = 1; a > 0; a--) {
      c += 1;
    }
    for (var b = 1; b > 0; b--) {
      %OptimizeOsr();
    }
  }
  return c;
}
try {
  foo();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f9bc2d^!)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=4f9bc2d)  
[test/mjsunit/regress/regress-crbug-480807.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-480807.js?cl=4f9bc2d)  
  

### **crbug:478612**  
   
**[Issue: Fatal error in ../../v8/src/x64/lithium-codegen-x64.cc,](https://crbug.com/478612)**  
**[Commit: [x64] Fix handling of Smi constants in LSubI and LBitI](https://chromium.googlesource.com/v8/v8/+/5379d8b)**  
  
Closed: Jul 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["merge-merged-4.4", "merge-merged-4.5", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-478612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-478612.js)  
```javascript
var z = {valueOf: function() { return 3; }};


function f() {
  var y = -2;
  return (1 & z) - y++;
}

assertEquals(3, f());
assertEquals(3, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f());


function g() {
  var y = 2;
  return (1 & z) | y++;
}

assertEquals(3, g());
assertEquals(3, g());
%OptimizeFunctionOnNextCall(g);
assertEquals(3, g());


function h() {
  var y = 3;
  return (3 & z) & y++;
}

assertEquals(3, h());
assertEquals(3, h());
%OptimizeFunctionOnNextCall(h);
assertEquals(3, h());


function i() {
  var y = 2;
  return (1 & z) ^ y++;
}

assertEquals(3, i());
assertEquals(3, i());
%OptimizeFunctionOnNextCall(i);
assertEquals(3, i());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5379d8b^!)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=5379d8b)  
[test/mjsunit/regress/regress-crbug-478612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-478612.js?cl=5379d8b)  
  

### **crbug:478011**  
   
**[Issue: Fatal error in ../../v8/src/handles.h,](https://crbug.com/478011)**  
**[Commit: Throw when attaching a stack trace to an object fails.](https://chromium.googlesource.com/v8/v8/+/8cf289c)**  
  
Closed: Apr 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-478011.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-478011.js)  
```javascript
var e = {};
Object.preventExtensions(e);
assertThrows(function() { Error.captureStackTrace(e) });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8cf289c^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=8cf289c)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=8cf289c)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=8cf289c)  
[test/mjsunit/regress/regress-crbug-478011.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-478011.js?cl=8cf289c)  
  

### **crbug:477924**  
   
**[Issue: UNKNOWN in v8::internal::MemoryChunk::heap](https://crbug.com/477924)**  
**[Commit: Don't use normalized map cache for prototype maps](https://chromium.googlesource.com/v8/v8/+/4204c72)**  
  
Closed: Apr 2015  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "Stability-Memory-AddressSanitizer", "M-44", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-477924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-477924.js)  
```javascript
var dummy = new ReferenceError("dummy");

constructors = [ ReferenceError, TypeError];

for (var i in constructors) {
  constructors[i].prototype.__defineGetter__("stack", function() {});
}

for (var i in constructors) {
  var obj = new constructors[i];
  obj.toString();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4204c72^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=4204c72)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4204c72)  
[test/mjsunit/regress/regress-crbug-477924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-477924.js?cl=4204c72)  
  

### **crbug:476477**  
   
**[Issue: Renderer crashed in V8 generated code, arm64 only, 100% reproducible](https://crbug.com/476477)**  
**[Commit: [crankshaft] Address the deoptimization loops of Math.floor, Math.round and Math.ceil.](https://chromium.googlesource.com/v8/v8/+/978ad03)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript>API  
Labels: ["Stability-Crash", "M-43", "Via-Wizard", "Merge-Merged-4.3"]  
Regress: [mjsunit/regress/regress-crbug-476477-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-476477-1.js), [mjsunit/regress/regress-crbug-476477-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-476477-2.js)  
```javascript
var obj = {
  _leftTime: 12345678,
  _divider: function() {
      var s = Math.floor(this._leftTime / 3600);
      var e = Math.floor(s / 24);
      var i = s % 24;
      return {
            s: s,
            e: e,
            i: i,
          }
    }
}

for (var i = 0; i < 1000; i++) {
  obj._divider();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/978ad03^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=978ad03)  
[src/crankshaft/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.h?cl=978ad03)  
[src/crankshaft/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-codegen-ia32.cc?cl=978ad03)  
[src/crankshaft/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.cc?cl=978ad03)  
[src/crankshaft/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/ia32/lithium-ia32.h?cl=978ad03)  
[src/crankshaft/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-codegen-x64.cc?cl=978ad03)  
[src/crankshaft/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-x64.cc?cl=978ad03)  
[src/crankshaft/x64/lithium-x64.h](https://cs.chromium.org/chromium/src/v8/src/crankshaft/x64/lithium-x64.h?cl=978ad03)  
[test/mjsunit/regress/regress-crbug-476477-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-476477-1.js?cl=978ad03)  
[test/mjsunit/regress/regress-crbug-476477-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-476477-2.js?cl=978ad03)  
  

### **crbug:471659**  
   
**[Issue: Fatal error in ../../v8/src/runtime/runtime-strings.cc,](https://crbug.com/471659)**  
**[Commit: A couple of other "stack overflow" vs. "has_pending_exception()" issues fixed.](https://chromium.googlesource.com/v8/v8/+/050e888)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-471659.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-471659.js)  
```javascript
var s = "0123456789ABCDEF";
for (var i = 0; i < 16; i++) s += s;

var count = 0;
function f() {
  try {
    f();
    if (count < 10) {
      f();
    }
  } catch(e) {
      s.replace("+", "-");
  }
  count++;
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/050e888^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=050e888)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=050e888)  
[src/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug.cc?cl=050e888)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=050e888)  
[src/runtime/runtime-strings.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-strings.cc?cl=050e888)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=050e888)  
[test/mjsunit/regress/regress-crbug-450960.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-450960.js?cl=050e888)  
[test/mjsunit/regress/regress-crbug-471659.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-471659.js?cl=050e888)  
[test/mjsunit/regress/regress-crbug-491062.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-491062.js?cl=050e888)  
  

### **crbug:469768**  
   
**[Issue: Crash with signature v8::internal::Invoke](https://crbug.com/469768)**  
**[Commit: JSEntryTrampoline: check for stack space before pushing arguments](https://chromium.googlesource.com/v8/v8/+/146598f)**  
  
Closed: May 2015  
Type: Bug  
Components: Blink  
Labels: ["M-43"]  
Regress: [mjsunit/regress/regress-crbug-469768.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-469768.js)  
```javascript
try {
  Array.prototype.concat.apply([], new Array(100000));
} catch (e) {
  
}


try {
  Array.prototype.concat.apply([], new Array(150000));
} catch (e) {
  
}


try {
  Array.prototype.concat.apply([], new Array(200000));
} catch (e) {
  
}


try {
  Array.prototype.concat.apply([], new Array(248000));
} catch (e) {
  
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/146598f^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=146598f)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=146598f)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=146598f)  
[src/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/builtins-x64.cc?cl=146598f)  
[test/mjsunit/regress/regress-crbug-469768.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-469768.js?cl=146598f)  
  

### **crbug:467531**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::AppendUse](https://crbug.com/467531)**  
**[Commit: [turbofan] Fix C++ evaluation order in AstGraphBuilder.](https://chromium.googlesource.com/v8/v8/+/7e8a62e)**  
  
Closed: Mar 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-467531.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-467531.js)  
```javascript
assertThrows(function() {
  "use strict";
  try {
    x = ref_error;
    let x = 0;
  } catch (e) {
    throw e;
  }
}, ReferenceError);

assertThrows(function() {
  "use strict";
  try {
    x = ref_error;
    let x = 0;
  } finally {
    
  }
}, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e8a62e^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=7e8a62e)  
[test/mjsunit/regress/regress-crbug-467531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-467531.js?cl=7e8a62e)  
  

### **crbug:467047**  
   
**[Issue: Fatal error in ../../v8/src/frames.cc,](https://crbug.com/467047)**  
**[Commit: Delegate throwing in RegExpExecStub to CEntryStub.](https://chromium.googlesource.com/v8/v8/+/86b391e)**  
  
Closed: Mar 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-467047.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-467047.js)  
```javascript
function captureMatch(re) {
  var local_variable = 0;
  "abcd".replace(re, function() { });
  assertEquals("abcd", RegExp.input);
  assertEquals("a", RegExp.leftContext);
  assertEquals("bc", RegExp.lastMatch);
  assertEquals("d", RegExp.rightContext);
  assertEquals("foo", captureMatch(/^bar/));
}

assertThrows(function() { captureMatch(/(bc)/) }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/86b391e^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=86b391e)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=86b391e)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=86b391e)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=86b391e)  
[src/mips64/code-stubs-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/code-stubs-mips64.cc?cl=86b391e)  
[src/ppc/code-stubs-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/code-stubs-ppc.cc?cl=86b391e)  
[src/runtime/runtime-regexp.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-regexp.cc?cl=86b391e)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=86b391e)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=86b391e)  
[src/x87/code-stubs-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/code-stubs-x87.cc?cl=86b391e)  
[test/mjsunit/regress/regress-crbug-467047.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-467047.js?cl=86b391e)  
  

### **crbug:465671**  
   
**[Issue: Fatal error in ../../v8/src/parser.cc,](https://crbug.com/465671)**  
**[Commit: Parser: Fix crash on stack overflow when lazy-parsing arrow functions](https://chromium.googlesource.com/v8/v8/+/3c3ce1b)**  
  
Closed: Jun 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/es6/regress/regress-crbug-465671-null.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-465671-null.js), [mjsunit/es6/regress/regress-crbug-465671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-465671.js)  
```javascript
function f() {
  var a = [10];
  try {
    f();
  } catch(e) {
    a.map((v) => v + 1);
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c3ce1b^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=3c3ce1b)  
[test/mjsunit/regress/regress-crbug-465671-null.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-465671-null.js?cl=3c3ce1b)  
[test/mjsunit/regress/regress-crbug-465671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-465671.js?cl=3c3ce1b)  
  

### **crbug:465564**  
   
**[Issue: UNKNOWN in v8::internal::ExternalReferenceEncoder::Encode](https://crbug.com/465564)**  
**[Commit: Add test case for serializing external references to runtime functions.](https://chromium.googlesource.com/v8/v8/+/3ed5dea)**  
  
Closed: Mar 2015  
Type: Bug  
Components: None  
Labels: ["Te-Logged", "M-43", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-465564.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-465564.js)  
```javascript
assertTrue(%StringLessThan("a", "b"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ed5dea^!)  
[test/mjsunit/regress/regress-crbug-465564.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-465564.js?cl=3ed5dea)  
  

### **crbug:461520**  
   
**[Issue: Fatal error in ../../v8/src/runtime/runtime-scopes.cc,](https://crbug.com/461520)**  
**[Commit: Remove effectful assertion](https://chromium.googlesource.com/v8/v8/+/68c8073)**  
  
Closed: Feb 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/es6/regress/regress-crbug-461520.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-461520.js)  
```javascript
var fuse = 1;

var handler = {
  get: function() { return function() {} },
  has() { return true },
  getOwnPropertyDescriptor: function() {
    if (fuse-- == 0) throw "please die";
    return {value: function() {}, configurable: true};
  }
};

var p = new Proxy({}, handler);
var o = Object.create(p);
with (o) { f() }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68c8073^!)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=68c8073)  
[test/mjsunit/harmony/regress/regress-crbug-461520.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-crbug-461520.js?cl=68c8073)  
  

### **crbug:455644**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::AppendUse](https://crbug.com/455644)**  
**[Commit: Fix try-finally for dead AST-branches in TurboFan.](https://chromium.googlesource.com/v8/v8/+/df986d0)**  
  
Closed: Feb 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-455644.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-455644.js)  
```javascript
(function f() {
  do { return 23; } while(false);
  with (0) {
    try {
      return 42;
    } finally {}
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df986d0^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=df986d0)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=df986d0)  
[src/compiler/control-builders.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-builders.cc?cl=df986d0)  
[src/compiler/control-builders.h](https://cs.chromium.org/chromium/src/v8/src/compiler/control-builders.h?cl=df986d0)  
[test/mjsunit/regress/regress-crbug-455644.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-455644.js?cl=df986d0)  
  

### **crbug:454091**  
   
**[Issue: Fatal error in ../../v8/src/handles.h,](https://crbug.com/454091)**  
**[Commit: Check global object behind global proxy for extensibility](https://chromium.googlesource.com/v8/v8/+/1de7dff)**  
  
Closed: Feb 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-454091.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-454091.js)  
```javascript
this.__proto__ = Array.prototype;
Object.freeze(this);
this.length = 1;
assertThrows('this.__proto__ = {}');
assertEquals(Array.prototype, this.__proto__);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1de7dff^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=1de7dff)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1de7dff)  
[test/mjsunit/regress/regress-crbug-454091.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-454091.js?cl=1de7dff)  
  

### **crbug:451770**  
   
**[Issue: UNKNOWN in v8::internal::SharedFunctionInfo::code](https://crbug.com/451770)**  
**[Commit: Add missing FrameState to JSToName nodes.](https://chromium.googlesource.com/v8/v8/+/c5833e8)**  
  
Closed: Jan 2015  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-451770.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-451770.js)  
```javascript
assertThrows(function f() {
  var t = { toString: function() { throw new Error(); } };
  var o = { [t]: 23 };
}, Error);

assertThrows(function f() {
  var t = { toString: function() { throw new Error(); } };
  class C { [t]() { return 23; } };
}, Error);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c5833e8^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c5833e8)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=c5833e8)  
[src/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast-numbering.cc?cl=c5833e8)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=c5833e8)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=c5833e8)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=c5833e8)  
[src/compiler/operator-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator-properties.cc?cl=c5833e8)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=c5833e8)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=c5833e8)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c5833e8)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=c5833e8)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=c5833e8)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=c5833e8)  
[src/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/full-codegen-x87.cc?cl=c5833e8)  
[test/mjsunit/regress/regress-crbug-451770.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-451770.js?cl=c5833e8)  
[test/unittests/compiler/js-operator-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-operator-unittest.cc?cl=c5833e8)  
  

### **crbug:451016**  
   
**[Issue: CHECK(IsBootstrappingOrGlobalObject(this->GetIsolate(), result)) failed: ..](https://crbug.com/451016)**  
**[Commit: Avoid unintentional optimization of hot builtins by TurboFan.](https://chromium.googlesource.com/v8/v8/+/d2e424a)**  
  
Closed: Jan 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-451016.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-451016.js)  
```javascript
var value = NaN;
for (i = 0; i < 256; i++) {
  value === "A" || value === "B";
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2e424a^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=d2e424a)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=d2e424a)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=d2e424a)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=d2e424a)  
[test/mjsunit/regress/regress-crbug-451016.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-451016.js?cl=d2e424a)  
  

### **crbug:451013**  
   
**[Issue: CHECK(*deopt_index != Safepoint::kNoDeoptimizationIndex) failed: ../../v8/s](https://crbug.com/451013)**  
**[Commit: Add missing FrameState for Runtime_CreateArrayLiteral.](https://chromium.googlesource.com/v8/v8/+/00f3f99)**  
  
Closed: Jan 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-451013.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-451013.js)  
```javascript
assertThrows(function testDeepArrayLiteral() {
  testDeepArrayLiteral([], [], [[]]);
}, RangeError);

assertThrows(function testDeepObjectLiteral() {
  testDeepObjectLiteral({}, {}, {x:[[]]});
}, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/00f3f99^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=00f3f99)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=00f3f99)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=00f3f99)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=00f3f99)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=00f3f99)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=00f3f99)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=00f3f99)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=00f3f99)  
[src/ppc/full-codegen-ppc.cc](https://cs.chromium.org/chromium/src/v8/src/ppc/full-codegen-ppc.cc?cl=00f3f99)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=00f3f99)  
[src/x87/full-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/full-codegen-x87.cc?cl=00f3f99)  
[test/mjsunit/regress/regress-crbug-451013.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-451013.js?cl=00f3f99)  
  

### **crbug:450960**  
   
**[Issue: CHECK(!isolate->has_pending_exception()) failed: ../../v8/src/compiler.cc(9](https://crbug.com/450960)**  
**[Commit: Clear pending exception on stack overflow in the parser](https://chromium.googlesource.com/v8/v8/+/9cce4ff)**  
  
Closed: Feb 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-450960.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-450960.js)  
```javascript
"a".replace(/a/g, "");

var count = 0;
function test() {
   try {
     test();
   } catch(e) {
     if (count < 50) {
       count++;
       "b".replace(/(b)/g, new []);
     }
   }
}

try {
  test();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9cce4ff^!)  
[src/runtime/runtime-internal.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-internal.cc?cl=9cce4ff)  
[test/mjsunit/regress/regress-crbug-450960.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-450960.js?cl=9cce4ff)  
  

### **crbug:450642**  
   
**[Issue: UNKNOWN in v8::internal::Code::deoptimization_data](https://crbug.com/450642)**  
**[Commit: Add missing BailoutId and FrameState to with statements.](https://chromium.googlesource.com/v8/v8/+/558efe2)**  
  
Closed: Jan 2015  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-450642.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-450642.js)  
```javascript
assertThrows(function() { with (undefined) {} }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/558efe2^!)  
[src/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast-numbering.cc?cl=558efe2)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=558efe2)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=558efe2)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=558efe2)  
[src/compiler/operator-properties.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/operator-properties.cc?cl=558efe2)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=558efe2)  
[test/mjsunit/regress/regress-crbug-450642.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-450642.js?cl=558efe2)  
[test/unittests/compiler/js-operator-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-operator-unittest.cc?cl=558efe2)  
  

### **crbug:448730**  
   
**[Issue: CHECK(field_type_.IsHeapObject()) failed: ../../v8/src/hydrogen.cc(6092)](https://crbug.com/448730)**  
**[Commit: Add proper support for proxies to HType.](https://chromium.googlesource.com/v8/v8/+/e1d878d)**  
  
Closed: Jan 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/es6/regress/regress-crbug-448730.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-448730.js)  
```javascript
function bar() {}
bar({ a: new Proxy({}, {}) });
function foo(x) { x.a.b == ""; }
var x = {a: {b: "" }};
foo(x);
foo(x);
%OptimizeFunctionOnNextCall(foo);
foo(x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1d878d^!)  
[src/hydrogen-types.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-types.cc?cl=e1d878d)  
[src/hydrogen-types.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-types.h?cl=e1d878d)  
[test/mjsunit/regress/regress-crbug-448730.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-448730.js?cl=e1d878d)  
  

### **crbug:436820**  
   
**[Issue: CHECK(value->IsMutableHeapNumber()) failed: ../../v8/src/objects.cc(2135)](https://crbug.com/436820)**  
**[Commit: Map::CopyGeneralizeAllRepresentations() left incorrect layout descriptor in a new map.](https://chromium.googlesource.com/v8/v8/+/1a2e4b2)**  
  
Closed: Nov 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Arch-x86_64", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-436820.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-436820.js)  
```javascript
function c(p) {
  return {__proto__: p};
}
var p = {};
var o = c(p);
p.x = 0.6;
Object.defineProperty(p, "x", { writable: false });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a2e4b2^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1a2e4b2)  
[test/mjsunit/regress/regress-crbug-436820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-436820.js?cl=1a2e4b2)  
  

### **crbug:435825**  
   
**[Issue: UNKNOWN in v8::internal::String::length](https://crbug.com/435825)**  
**[Commit: Fix RegExp.source for uncompiled regexp.](https://chromium.googlesource.com/v8/v8/+/14a3b91)**  
  
Closed: Nov 2014  
Type: Bug-Security  
Components: None  
Labels: ["ReleaseBlock-Beta", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "Security_Severity-Low", "M-41", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-435825.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-435825.js)  
```javascript
Error.prepareStackTrace = function (a,b) { return b; };

try {
  /(invalid regexp/;
} catch (e) {
  assertEquals("[object global]", e.stack[0].getThis().toString());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14a3b91^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=14a3b91)  
[test/mjsunit/regress/regress-crbug-435825.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-435825.js?cl=14a3b91)  
  

### **crbug:433766**  
   
**[Issue: CHECK failure in CHECK(entry.is_valid()) failed: ../../v8/src/parser.cc(3823)](https://crbug.com/433766)**  
**[Commit: Fix one more missing c0_ < 0 check in scanner](https://chromium.googlesource.com/v8/v8/+/bf22724)**  
  
Closed: Nov 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-433766.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-433766.js)  
```javascript
var filler = "


eval(
  "'use strict';" +
  "var x = 23;" +
  "var f = function bozo1() {" +
  "  return x;" +
  "};" +
  "f;" +
  filler
)();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf22724^!)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=bf22724)  
[test/mjsunit/regress/regress-crbug-433766.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-433766.js?cl=bf22724)  
  

### **crbug:433332**  
   
**[Issue: CHECK failure in CHECK(lower->Is(upper)) failed: ../../v8/src/types.h(1025)](https://crbug.com/433332)**  
**[Commit: Fix lower bound violation](https://chromium.googlesource.com/v8/v8/+/4f63564)**  
  
Closed: Apr 2015  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-433332.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-433332.js)  
```javascript
function f(foo) {
  var g;
  true ? (g = 0.1) : g |=  null;
  if (null != g) {}
};

f(1.4);
f(1.4);
%OptimizeFunctionOnNextCall(f);
f(1.4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f63564^!)  
[src/types.h](https://cs.chromium.org/chromium/src/v8/src/types.h?cl=4f63564)  
[test/mjsunit/regress/regress-crbug-433332.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-433332.js?cl=4f63564)  
  

### **crbug:431602**  
   
**[Issue: UNKNOWN in v8::internal::RootMarkingVisitor::MarkObjectByPointer](https://crbug.com/431602)**  
**[Commit: Fix has_constant_parameter_count() confusion in LReturn](https://chromium.googlesource.com/v8/v8/+/d3b68cf)**  
  
Closed: Nov 2014  
Type: Bug-Security  
Components: None  
Labels: ["ReleaseBlock-Beta", "Merge-na", "Stability-Memory-AddressSanitizer", "M-41", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-431602.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-431602.js)  
```javascript
var heap_number_producer = {y:1.5};
heap_number_producer.y = 0;
var heap_number_zero = heap_number_producer.y;
var non_constant_eight = {};
non_constant_eight = 8;

function BreakIt() {
  return heap_number_zero | (1 | non_constant_eight);
}

function expose(a, b, c) {
  return b;
}

assertEquals(9, expose(8, 9, 10));
assertEquals(9, expose(8, BreakIt(), 10));
assertEquals(9, BreakIt());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d3b68cf^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=d3b68cf)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=d3b68cf)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=d3b68cf)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=d3b68cf)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=d3b68cf)  
[src/mips64/lithium-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/lithium-codegen-mips64.cc?cl=d3b68cf)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=d3b68cf)  
[src/x87/lithium-codegen-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/lithium-codegen-x87.cc?cl=d3b68cf)  
[test/mjsunit/regress/regress-crbug-431602.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-431602.js?cl=d3b68cf)  
  

### **crbug:430846**  
   
**[Issue: Hangouts fails on Linux Debug after V8 roll.](https://crbug.com/430846)**  
**[Commit: Fix for an assertion failure in Map::FindTransitionToField(...). Appeared after r25136.](https://chromium.googlesource.com/v8/v8/+/e1f93a8)**  
  
Closed: Nov 2014  
Type: Bug  
Components: Blink  
Labels: ["M-40"]  
Regress: [mjsunit/regress/regress-crbug-430846.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-430846.js)  
```javascript
function foo() { return 1; };
var o1 = {};
o1.foo = foo;

var json = '{"foo": {"x": 1}}';
var o2 = JSON.parse(json);
var o3 = JSON.parse(json);
assertTrue(%HaveSameMap(o2, o3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1f93a8^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=e1f93a8)  
[test/mjsunit/regress/regress-crbug-430846.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-430846.js?cl=e1f93a8)  
  

### **crbug:429159**  
   
**[Issue: CHECK failure in CHECK(stack_height() > 0) failed: .././src/compiler/ast-graph-builder.h(252](https://crbug.com/429159)**  
**[Commit: Properly handle stack overflows in the AST graph builder.](https://chromium.googlesource.com/v8/v8/+/cd3273b)**  
  
Closed: Oct 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-429159.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-429159.js)  
```javascript
try {
  var src = "return " + Array(12000).join("src,") + "src";
  var fun = Function(src);
  assertEquals(src, fun());
} catch (e) {
  
  assertInstanceof(e, RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd3273b^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=cd3273b)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=cd3273b)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=cd3273b)  
[test/mjsunit/regress/regress-crbug-429159.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-429159.js?cl=cd3273b)  
  

### **crbug:425585**  
   
**[Issue: Use-of-uninitialized-value in v8::internal::Decoder<v8::internal::Simulator>::DecodeBranchSystemException](https://crbug.com/425585)**  
**[Commit: ARM64: Fix stack manipulation.](https://chromium.googlesource.com/v8/v8/+/ecbfc43)**  
  
Closed: Oct 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["ReleaseBlock-Beta", "Merge-na", "reward-2500", "External-Fuzzer-Contribution", "M-40", "Security_Impact-Head", "Stability-Memory-MemorySanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-425585.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-425585.js)  
```javascript
var correct_result = "This is the correct result.";

function foo(recursion_depth) {
   if (recursion_depth > 0) return foo(recursion_depth - 1);
   return new String(correct_result, 1, 2, 3, 4, 5, 6);
}


function test(i) {
   var actual = foo(i);
   if (correct_result != actual) {
     var msg = "Expected \"" + correct_result + "\", found " + actual;
     throw new MjsUnitAssertionError(msg);
   }
}

test(1);
test(1);
test(10);
test(100);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecbfc43^!)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=ecbfc43)  
[test/mjsunit/regress/regress-crbug-425585.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-425585.js?cl=ecbfc43)  
  

### **crbug:425519**  
   
**[Issue: UNKNOWN in v8::internal::HEnvironment::Pop](https://crbug.com/425519)**  
**[Commit: The issue is that by handling strings with map/handler pairs instead of a special](https://chromium.googlesource.com/v8/v8/+/8330178)**  
  
Closed: Oct 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-425519.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-425519.js)  
```javascript
function load(a, i) {
  return a[i];
}

load([]);
load(0);
load("x", 0);
%OptimizeFunctionOnNextCall(load);
load([], 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8330178^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=8330178)  
[test/mjsunit/regress/regress-crbug-425519.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-425519.js?cl=8330178)  
  

### **crbug:423687**  
   
**[Issue: JSON.parse sometimes crashes chrome 38 ('Aw, snap')](https://crbug.com/423687)**  
**[Commit: Fixed mutable heap numbers leak in JSON parser.](https://chromium.googlesource.com/v8/v8/+/5509cc2)**  
  
Closed: Oct 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["merge-merged-3.29", "merge-merged-3.28", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-423687.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-423687.js)  
```javascript
var json = '{"a":{"c":2.1,"d":0},"b":{"c":7,"1024":8}}';
var data = JSON.parse(json);

data.b.c++;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5509cc2^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=5509cc2)  
[test/mjsunit/regress/regress-crbug-423687.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-423687.js?cl=5509cc2)  
  

### **crbug:422858**  
   
**[Issue: Date.toLocalString() and new Date("str") are not complementary](https://crbug.com/422858)**  
**[Commit: Accept time zones like GMT-8 in the legacy date parser](https://chromium.googlesource.com/v8/v8/+/acbd64b)**  
  
Closed: Jan 2016  
Type: Bug  
Components: Blink>JavaScript>Internationalization  
Labels: ["Via-Wizard", "Hotlist-Google"]  
Regress: [mjsunit/regress/regress-crbug-422858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-422858.js)  
```javascript
var date = new Date("2016/01/02 10:00 GMT-8")
assertEquals(0, date.getMinutes());
assertEquals(18, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-12")
assertEquals(0, date.getMinutes());
assertEquals(22, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-123")
assertEquals(23, date.getMinutes());
assertEquals(11, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-0856")
assertEquals(56, date.getMinutes());
assertEquals(18, date.getUTCHours());

date = new Date("2016/01/02 10:00 GMT-08000")
assertEquals(NaN, date.getMinutes());
assertEquals(NaN, date.getUTCHours());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/acbd64b^!)  
[src/dateparser-inl.h](https://cs.chromium.org/chromium/src/v8/src/dateparser-inl.h?cl=acbd64b)  
[test/mjsunit/regress/regress-crbug-422858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-422858.js?cl=acbd64b)  
  

### **crbug:417508**  
   
**[Issue: No Permission](https://crbug.com/417508)**  
**[Commit: Fix Hydrogen's BuildStore()](https://chromium.googlesource.com/v8/v8/+/1bb52d0)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-417508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-417508.js)  
```javascript
function foo(x) {
  var k = "value";
  return x[k] = 1;
}
var obj = {};
Object.defineProperty(obj, "value", {set: function(x) { throw "nope"; }});
try { foo(obj); } catch(e) {}
try { foo(obj); } catch(e) {}
%OptimizeFunctionOnNextCall(foo);
try { foo(obj); } catch(e) {}

function bar(x) {
  var k = "value";
  return (x[k] = 1) ? "ok" : "nope";
}
var obj2 = {};
Object.defineProperty(obj2, "value",
    {set: function(x) { throw "nope"; return true; } });

try { bar(obj2); } catch(e) {}
try { bar(obj2); } catch(e) {}
%OptimizeFunctionOnNextCall(bar);
try { bar(obj2); } catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1bb52d0^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=1bb52d0)  
[test/mjsunit/regress/regress-crbug-417508.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-417508.js?cl=1bb52d0)  
  

### **crbug:416558**  
   
**[Issue: UNKNOWN in v8::internal::Map::instance_type](https://crbug.com/416558)**  
**[Commit: Non-JSArrays must always have holey elements.](https://chromium.googlesource.com/v8/v8/+/1903e56)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Te-Logged", "M-39", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-416558.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-416558.js)  
```javascript
(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = /x/;
  store(c);
  function get_hole() {
    var b = /x/;
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new Date();
  store(c);
  function get_hole() {
    var b = new Date();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new Number(1);
  store(c);
  function get_hole() {
    var b = new Number(1);
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new Boolean();
  store(c);
  function get_hole() {
    var b = new Boolean();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new Map();
  store(c);
  function get_hole() {
    var b = new Map();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new Set();
  store(c);
  function get_hole() {
    var b = new Set();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new WeakMap();
  store(c);
  function get_hole() {
    var b = new WeakMap();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();

(function() {
  function store(x) { x[0] = 0; }
  store([]);
  var c = new WeakSet();
  store(c);
  function get_hole() {
    var b = new WeakSet();
    store(b);
    return b[1];
  }
  assertEquals(undefined, get_hole());
  assertEquals(undefined, get_hole());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1903e56^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=1903e56)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=1903e56)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=1903e56)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=1903e56)  
[test/mjsunit/regress/regress-crbug-416558.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-416558.js?cl=1903e56)  
  

### **crbug:412319**  
   
**[Issue: CHECK failure in CHECK(receiver_map->is_extensible()) failed: ../../v8/src/hydrogen.cc(8293)](https://crbug.com/412319)**  
**[Commit: Don't inline Array functions if receiver map is not extensible.](https://chromium.googlesource.com/v8/v8/+/d66ed11)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-412319.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-412319.js)  
```javascript
function __f_6() {
 var __v_7 = [0];
 Object.preventExtensions(__v_7);
 for (var __v_6 = -2; __v_6 < 19; __v_6++) __v_7.shift();
 __f_7(__v_7);
}
__f_6();
__f_6();
%OptimizeFunctionOnNextCall(__f_6);
__f_6();
function __f_7(__v_7) {
  __v_7.pop();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d66ed11^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=d66ed11)  
[test/mjsunit/regress/regress-crbug-412319.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-412319.js?cl=d66ed11)  
  

### **crbug:412215**  
   
**[Issue: CHECK failure in CHECK(value->IsHeapObject()) failed: ../../v8/src/objects-debug.cc(271)](https://crbug.com/412215)**  
**[Commit: Fix Smi vs. HeapObject confusion in HConstants.](https://chromium.googlesource.com/v8/v8/+/b4375b7)**  
  
Closed: Sep 2014  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-39", "External-Fuzzer-Contribution", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-412215.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-412215.js)  
```javascript
var dummy = {foo: "true"};

var a = {y:0.5};
a.y = 357;
var b = a.y;

var d;
function f(  )  {
  d = 357;
  return {foo: b};
}
f();
f();
%OptimizeFunctionOnNextCall(f);
var x = f();




function g(obj) {
  return obj.foo.length;
}

g(dummy);
g(dummy);
%OptimizeFunctionOnNextCall(g);
g(x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4375b7^!)  
[src/conversions.h](https://cs.chromium.org/chromium/src/v8/src/conversions.h?cl=b4375b7)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=b4375b7)  
[src/hydrogen-types.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-types.cc?cl=b4375b7)  
[test/mjsunit/regress/regress-crbug-412215.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-412215.js?cl=b4375b7)  
  

### **crbug:412210**  
   
**[Issue: CHECK failure in CHECK(right_type->Is(Type::String())) failed: ../../v8/src/hydrogen.cc(1028](https://crbug.com/412210)**  
**[Commit: Fix inaccurate type condition in Hydrogen](https://chromium.googlesource.com/v8/v8/+/fc71f7f)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-412210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-412210.js)  
```javascript
function f(x) {
  return (x ? "" >> 0 : "") + /a/;
};

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc71f7f^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=fc71f7f)  
[test/mjsunit/regress/regress-crbug-412210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-412210.js?cl=fc71f7f)  
  

### **crbug:412208**  
   
**[Issue: CHECK failure in CHECK(ast_context()->IsEffect()) failed: ../../v8/src/hydrogen.cc(6760)](https://crbug.com/412208)**  
**[Commit: Hydrogen: bailout when there is a throw statement in a non-effect context.](https://chromium.googlesource.com/v8/v8/+/fd3e505)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-412208.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-412208.js)  
```javascript
var non_const_true = true;

function f() {
  return non_const_true || (f() = this);
}

assertTrue(f());
assertTrue(f());
%OptimizeFunctionOnNextCall(f);
assertTrue(f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd3e505^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=fd3e505)  
[test/mjsunit/regress/regress-crbug-412208.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-412208.js?cl=fd3e505)  
  

### **crbug:412203**  
   
**[Issue: Unreachable code in ../../v8/src/runtime.cc(10277)](https://crbug.com/412203)**  
**[Commit: Fix ElementsKind handling of prototypes in Array.concat](https://chromium.googlesource.com/v8/v8/+/11f7584)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-39", "External-Fuzzer-Contribution", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-412203.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-412203.js)  
```javascript
var b = [];
b[10000] = 1;

assertTrue(%HasDictionaryElements(b));

var a1 = [1.5];
b.__proto__ = a1;
assertEquals(1.5, ([].concat(b))[0]);

var a2 = new Int32Array(2);
a2[0] = 3;
b.__proto__ = a2
assertEquals(3, ([].concat(b))[0]);

function foo(x, y) {
  var a = [];
  a[10000] = 1;
  assertTrue(%HasDictionaryElements(a));

  a.__proto__ = arguments;
  var c = [].concat(a);
  for (var i = 0; i < arguments.length; i++) {
    assertEquals(i + 2, c[i]);
  }
  assertEquals(undefined, c[arguments.length]);
  assertEquals(undefined, c[arguments.length + 1]);
}
foo(2);
foo(2, 3);
foo(2, 3, 4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11f7584^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=11f7584)  
[test/mjsunit/regress/regress-crbug-412203.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-412203.js?cl=11f7584)  
  

### **crbug:407946**  
   
**[Issue: array indexOf unreliable](https://crbug.com/407946)**  
**[Commit: Enforce correct number comparisons when inlining Array.indexOf.](https://chromium.googlesource.com/v8/v8/+/0baf275)**  
  
Closed: Sep 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-38", "Via-Wizard", "Merge-Questions-Applied"]  
Regress: [mjsunit/regress/regress-crbug-407946.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-407946.js)  
```javascript
function f(n) { return [0].indexOf((n - n) + 0); }

assertEquals(0, f(.1));
assertEquals(0, f(.1));
%OptimizeFunctionOnNextCall(f);
assertEquals(0, f(.1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0baf275^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=0baf275)  
[test/mjsunit/regress/regress-crbug-407946.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-407946.js?cl=0baf275)  
  

### **crbug:405517**  
   
**[Issue: CHECK failure in CHECK(receiver_map->is_extensible()) failed: ../../v8/src/hydrogen.cc(8338)](https://crbug.com/405517)**  
**[Commit: Don't inline Array.shift() if receiver map is not extensible.](https://chromium.googlesource.com/v8/v8/+/0142786)**  
  
Closed: Dec 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-405517.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-405517.js)  
```javascript
function f() {
 var e = [0];
 Object.preventExtensions(e);
 for (var i = 0; i < 4; i++) e.shift();
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0142786^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=0142786)  
[test/mjsunit/regress/regress-crbug-405517.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-405517.js?cl=0142786)  
  

### **crbug:403409**  
   
**[Issue: V8 Runtime_ArrayConcat uninitialized memory leak](https://crbug.com/403409)**  
**[Commit: Correctly handle holes when concat()ing double arrays](https://chromium.googlesource.com/v8/v8/+/dacca11)**  
  
Closed: Aug 2014  
Type: Bug-Security  
Components: None  
Labels: ["M-38", "Via-Wizard", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "CVE-2014-3195", "reward-4500", "allpublic", "Release-0-M38", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-403409.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-403409.js)  
```javascript
Array.prototype[0] = 777;
var kElements = 10;

var input_array = [];
for (var i = 1; i < kElements; i++) {
  input_array[i] = 0.5;
}
var output_array = input_array.concat(0.5);

assertEquals(kElements + 1, output_array.length);
assertEquals(777, output_array[0]);
for (var j = 1; j < kElements; j++) {
  assertEquals(0.5, output_array[j]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dacca11^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=dacca11)  
[test/mjsunit/regress/regress-crbug-403409.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-403409.js?cl=dacca11)  
  

### **crbug:393988**  
   
**[Issue: TypeError: Cannot redefine property: stack](https://crbug.com/393988)**  
**[Commit: Error.captureStackTrace should define "stack" property as configurable.](https://chromium.googlesource.com/v8/v8/+/49ae308)**  
  
Closed: Jul 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-Netflix"]  
Regress: [mjsunit/regress/regress-crbug-393988.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-393988.js)  
```javascript
var o = {};
Error.captureStackTrace(o);
Object.defineProperty(o, "stack", { value: 1 });
assertEquals(1, o.stack);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49ae308^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=49ae308)  
[test/mjsunit/regress/regress-crbug-393988.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-393988.js?cl=49ae308)  
  

### **crbug:390918**  
   
**[Issue: CHECK failure in CHECK_EQ(map()->unused_property_fields(), (map()->inobject_properties() + p](https://crbug.com/390918)**  
**[Commit: One of the fast cases in JSObject::MigrateFastToFast() should not be taken if the number of fields did not change.](https://chromium.googlesource.com/v8/v8/+/2fba190)**  
  
Closed: Jul 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-390918.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-390918.js)  
```javascript
function f(scale) {
  var arr = {a: 1.1};

  for (var i = 0; i < 2; i++) {
    arr[2 * scale] = 0;
  }
}

f({});
f({});
%OptimizeFunctionOnNextCall(f);
f(1004);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2fba190^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=2fba190)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=2fba190)  
[test/mjsunit/regress/regress-crbug-390918.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-390918.js?cl=2fba190)  
  

### **crbug:387636**  
   
**[Issue: CHECK failure in CHECK(!right->IsConstant() || (!HConstant::cast(right)->HasInteger32Value()](https://crbug.com/387636)**  
**[Commit: Remove bogus assertions in HCompareObjectEqAndBranch.](https://chromium.googlesource.com/v8/v8/+/58bf19e)**  
  
Closed: Jun 2014  
Type: Bug  
Components: Blink  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-387636.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-387636.js)  
```javascript
function f() {
  [].indexOf(0x40000000);
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58bf19e^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=58bf19e)  
[test/mjsunit/regress/regress-crbug-387636.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-387636.js?cl=58bf19e)  
  

### **crbug:387031**  
   
**[Issue: Security: V8 Array length getter override](https://crbug.com/387031)**  
**[Commit: Array.concat: properly go to dictionary mode when required](https://chromium.googlesource.com/v8/v8/+/1d35d6d)**  
  
Closed: Jun 2014  
Type: Bug-Security  
Components: None  
Labels: ["Merge-Merged", "Release-0-M36", "Security_Impact-Stable", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "M-36"]  
Regress: [mjsunit/regress/regress-crbug-387031.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-387031.js)  
```javascript
a = [1];
b = [];
a.__defineGetter__(0, function () {
  b.length = 0xffffffff;
});
c = a.concat(b);
for (var i = 0; i < 20; i++) {
  assertEquals(undefined, (c[i]));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d35d6d^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=1d35d6d)  
[test/mjsunit/regress/regress-crbug-387031.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-387031.js?cl=1d35d6d)  
  

### **crbug:385002**  
   
**[Issue: Heap-buffer-overflow in v8::internal::Simulator::HandleRList](https://crbug.com/385002)**  
**[Commit: Interrupts must not mask stack overflow.](https://chromium.googlesource.com/v8/v8/+/11368af)**  
  
Closed: Jun 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-385002.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-385002.js)  
```javascript
%ScheduleBreak(); 

function f() { f(); }
assertThrows(f, RangeError);

var locals = "";
for (var i = 0; i < 1024; i++) locals += "var v" + i + ";";
eval("function g() {" + locals + "f();}");
assertThrows("g()", RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11368af^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=11368af)  
[src/arm/regexp-macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/regexp-macro-assembler-arm.cc?cl=11368af)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=11368af)  
[src/arm64/regexp-macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/regexp-macro-assembler-arm64.cc?cl=11368af)  
[src/execution.cc](https://cs.chromium.org/chromium/src/v8/src/execution.cc?cl=11368af)  
[src/execution.h](https://cs.chromium.org/chromium/src/v8/src/execution.h?cl=11368af)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=11368af)  
[src/ia32/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/regexp-macro-assembler-ia32.cc?cl=11368af)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=11368af)  
[src/isolate.h](https://cs.chromium.org/chromium/src/v8/src/isolate.h?cl=11368af)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=11368af)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=11368af)  
[src/x64/regexp-macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/regexp-macro-assembler-x64.cc?cl=11368af)  
[test/mjsunit/regress/regress-crbug-385002.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-385002.js?cl=11368af)  
  

### **crbug:382513**  
   
**[Issue: UNKNOWN in v8::internal::Simulator::DecodeType2](https://crbug.com/382513)**  
**[Commit: Fix missing smi check in inlined indexOf/lastIndexOf.](https://chromium.googlesource.com/v8/v8/+/7eea77b)**  
  
Closed: Jun 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-382513.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-382513.js)  
```javascript
function foo() { return [+0,false].indexOf(-(4/3)); }
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7eea77b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=7eea77b)  
[test/mjsunit/regress/regress-crbug-382513.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-382513.js?cl=7eea77b)  
  

### **crbug:382143**  
   
**[Issue: [REGRESSION] Methods become non-enumerable if first introduced via a parent constructor that defines a property](https://crbug.com/382143)**  
**[Commit: Fix invalid attributes when generalizing because of incompatible map change.](https://chromium.googlesource.com/v8/v8/+/0fcd891)**  
  
Closed: Jun 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-382143.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-382143.js)  
```javascript
function A() {
  Object.defineProperty(this, "x", { set: function () {}, get: function () {}});
  this.a = function () { return 1; }
}

function B() {
  A.apply( this );
  this.a = function () { return 2; }
}

var b = new B();
assertTrue(Object.getOwnPropertyDescriptor(b, "a").enumerable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0fcd891^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0fcd891)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=0fcd891)  
[test/mjsunit/regress/regress-crbug-382143.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-382143.js?cl=0fcd891)  
  

### **crbug:381534**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/381534)**  
**[Commit: Bugfix in inlined versions of Array.indexOf() and Array.lastIndexOf() with a regression test.](https://chromium.googlesource.com/v8/v8/+/6dc967e)**  
  
Closed: Jun 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-381534.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-381534.js)  
```javascript
var obj = {};

function f(v) {
  var v1 = -(4/3);
  var v2 = 1;
  var arr = new Array(+0, true, 0, -0, false, undefined, null, "0", obj, v1, -(4/3), -1.3333333333333, "str", v2, 1, false);
  assertEquals(10, arr.lastIndexOf(-(4/3)));
  assertEquals(9, arr.indexOf(-(4/3)));

  assertEquals(10, arr.lastIndexOf(v));
  assertEquals(9, arr.indexOf(v));

  assertEquals(8, arr.lastIndexOf(obj));
  assertEquals(8, arr.indexOf(obj));
}

function g(v, x, index) {
  var arr = new Array({}, x-1.1, x-2, x-3.1);
  assertEquals(index, arr.indexOf(0));
  assertEquals(index, arr.lastIndexOf(0));

  assertEquals(index, arr.indexOf(v));
  assertEquals(index, arr.lastIndexOf(v));
}

f(-(4/3));
f(-(4/3));
%OptimizeFunctionOnNextCall(f);
f(-(4/3));

g(0, 2, 2);
g(0, 3.1, 3);
%OptimizeFunctionOnNextCall(g);
g(0, 1.1, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6dc967e^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=6dc967e)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6dc967e)  
[test/mjsunit/regress/regress-crbug-381534.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-381534.js?cl=6dc967e)  
  

### **crbug:380512**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/380512)**  
**[Commit: Fix invalid loop condition for Array.lastIndexOf().](https://chromium.googlesource.com/v8/v8/+/9244429)**  
  
Closed: Jun 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-380512.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-380512.js)  
```javascript
function f() { [].lastIndexOf(42); }

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9244429^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9244429)  
[test/mjsunit/regress/regress-crbug-380512.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-380512.js?cl=9244429)  
  

### **crbug:374838**  
   
**[Issue: No Permission](https://crbug.com/374838)**  
**[Commit: Fix ArrayShift hydrogen support](https://chromium.googlesource.com/v8/v8/+/58661c1)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-374838.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-374838.js)  
```javascript
function foo() {
  var a = [0];
  result = 0;
  for (var i = 0; i < 4; i++) {
    result += a.length;
    a.shift();
  }
  return result;
}

assertEquals(1, foo());
assertEquals(1, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/58661c1^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=58661c1)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=58661c1)  
[test/mjsunit/regress/regress-crbug-374838.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-374838.js?cl=58661c1)  
  

### **crbug:364374**  
   
**[Issue: Some official time zone IDs  are not accepted by Intl.DateTimeFormat.](https://crbug.com/364374)**  
**[Commit: Timezone name check fix](https://chromium.googlesource.com/v8/v8/+/4e18190)**  
  
Closed: None  
Type: Bug  
Components: Blink>JavaScript>Internationalization  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-364374.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-364374.js)  
```javascript
if (this.Intl) {
  

  
  
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'eUrope/isLe_OF_man'})
  assertEquals('Europe/Isle_of_Man', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'africa/Dar_eS_salaam'})
  assertEquals('Africa/Dar_es_Salaam', df.resolvedOptions().timeZone);

  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/port_of_spain'})
  assertEquals('America/Port_of_Spain', df.resolvedOptions().timeZone);

  
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/north_Dakota/new_salem'})
  assertEquals('America/North_Dakota/New_Salem', df.resolvedOptions().timeZone);

  
  
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/aRgentina/buenos_aIres'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Argentina/Buenos_Aires'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Buenos_Aires'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Indiana/Indianapolis'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/Indianapolis'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  
  
  


  
  df = new Intl.DateTimeFormat('en-US', {'timeZone': 'America/port-aU-pRince'})
  assertEquals('America/Port-au-Prince', df.resolvedOptions().timeZone);

  
  df1 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Ho_Chi_Minh'})
  df2 = new Intl.DateTimeFormat('en-US', {'timeZone': 'Asia/Saigon'})
  assertEquals(df1.resolvedOptions().timeZone, df2.resolvedOptions().timeZone);

  
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'Europe/_Paris'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New__York'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New_York_'}));
  assertThrows(() => Intl.DateTimeFormat(undefined, {timeZone: 'America/New_Y0rk'}));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e18190^!)  
[src/js/i18n.js](https://cs.chromium.org/chromium/src/v8/src/js/i18n.js?cl=4e18190)  
[src/messages.h](https://cs.chromium.org/chromium/src/v8/src/messages.h?cl=4e18190)  
[test/mjsunit/regress/regress-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-487322.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-364374.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-364374.js?cl=4e18190)  
[test/mjsunit/regress/regress-crbug-487322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487322.js?cl=4e18190)  
  

### **crbug:357330**  
   
**[Issue: CHECK fail in ../../v8/src/list-inl.h while watching Youtube video.](https://crbug.com/357330)**  
**[Commit: Fix Type::Intersect to skip uninhabited bitsets](https://chromium.googlesource.com/v8/v8/+/282a7ca)**  
  
Closed: Apr 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-35", "Merge-Merged", "ReleaseBlock-Stable"]  
Regress: [mjsunit/regress/regress-crbug-357330.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-357330.js)  
```javascript
function f(foo) {
  var g;
  true ? (g = foo + 0) : g = null;
  if (null != g) {}
};

f(1.4);
f(1.4);
%OptimizeFunctionOnNextCall(f);
f(1.4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/282a7ca^!)  
[src/types.cc](https://cs.chromium.org/chromium/src/v8/src/types.cc?cl=282a7ca)  
[test/mjsunit/regress/regress-crbug-357330.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-357330.js?cl=282a7ca)  
  

### **crbug:357137**  
   
**[Issue: No Permission](https://crbug.com/357137)**  
**[Commit: Do not check for interrupt when allocating stack locals.](https://chromium.googlesource.com/v8/v8/+/c0fa861)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-357137.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-357137.js)  
```javascript
var locals = "";
for (var i = 0; i < 1024; i++) locals += "var v" + i + ";";
eval("function f() {" + locals + "f();}");
assertThrows("f()", RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c0fa861^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c0fa861)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=c0fa861)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c0fa861)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=c0fa861)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=c0fa861)  
[test/cctest/test-heap.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-heap.cc?cl=c0fa861)  
[test/mjsunit/regress/regress-crbug-357137.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-357137.js?cl=c0fa861)  
  

### **crbug:357052**  
   
**[Issue: CHECK failure in CHECK(!CpuFeatures::IsSupported(SSE2)) failed: ../../v8/src/ic.cc(2318)](https://crbug.com/357052)**  
**[Commit: Fix HGraphBuilder::BuildAddStringLengths](https://chromium.googlesource.com/v8/v8/+/511edab)**  
  
Closed: Apr 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-357052.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-357052.js)  
```javascript
function f() {
  var str = "";
  for (var i = 0; i < 30; i++) {
    str += "abcdefgh12345678" + str;
  }
  return str;
}
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/511edab^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=511edab)  
[test/mjsunit/regress/regress-crbug-357052.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-357052.js?cl=511edab)  
  

### **crbug:354391**  
   
**[Issue: CHECK failure in CHECK(IsFastElementsKind(elements_kind) || IsExternalArrayElementsKind(elements_kind)) failed: ../sr](https://crbug.com/354391)**  
**[Commit: Fix polymorphic hydrogen handling of SLOPPY_ARGUMENTS_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/2b722b6)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-354391.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-354391.js)  
```javascript
function load(a, i) {
  return a[i];
}

function f2(a, b, c, d, index) {
  return load(arguments, index);
}

f2(1, 2, 3, 4, "foo");
f2(1, 2, 3, 4, "foo");
load([11, 22, 33], 0);
assertEquals(11, f2(11, 22, 33, 44, 0));

%OptimizeFunctionOnNextCall(load);
assertEquals(11, f2(11, 22, 33, 44, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2b722b6^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2b722b6)  
[test/mjsunit/regress/regress-crbug-354391.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-354391.js?cl=2b722b6)  
  

### **crbug:352929**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/352929)**  
**[Commit: Fix typo in r19923 (bounds check offset propagation)](https://chromium.googlesource.com/v8/v8/+/dc45852)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["M-35", "Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-352929.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-352929.js)  
```javascript
var dummy = new Int32Array(100);
array = new Int32Array(100);
var dummy2 = new Int32Array(100);

array[-17] = 0;
function fun(base,cond) {
  array[base - 1] = 1;
  array[base - 2] = 2;
  if (cond) {
    array[base - 4] = 3;
    array[base - 5] = 4;
  } else {
    array[base - 6] = 5;
    array[base - 100] = 777;
  }
}
fun(5,true);
fun(7,false);
%OptimizeFunctionOnNextCall(fun);
fun(7,false);

for (var i = 0; i < dummy.length; i++) {
  assertEquals(0, dummy[i]);
}
for (var i = 0; i < dummy2.length; i++) {
  assertEquals(0, dummy2[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc45852^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=dc45852)  
[test/mjsunit/regress/regress-crbug-352929.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-352929.js?cl=dc45852)  
  

### **crbug:352586**  
   
**[Issue: CHECK failure in CHECK((check == ENABLE_INLINED_SMI_CHECK) ? (*jmp_address == Assembler::kJncShortOpcode || *jmp_addr](https://crbug.com/352586)**  
**[Commit: Fix ASSERT violation when BinaryOpIC::Transition recurses into itself](https://chromium.googlesource.com/v8/v8/+/e4a18df)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-352586.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-352586.js)  
```javascript
var a = {};

function getter() {
  do {
    return a + 1;
  } while (false);
}

a.__proto__ = Error("");
a.__defineGetter__('message', getter);
assertThrows(()=>a.message, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e4a18df^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=e4a18df)  
[src/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic.h?cl=e4a18df)  
[test/mjsunit/regress/regress-crbug-352586.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-352586.js?cl=e4a18df)  
  

### **crbug:352058**  
   
**[Issue: CHECK failure in CHECK(old_entry->maps_->size() > 0) failed: ../src/hydrogen-check-elimination.cc(156)](https://crbug.com/352058)**  
**[Commit: Check elimination now sets known successor branch of HCompareObjectEqAndBranch (correctness fix).](https://chromium.googlesource.com/v8/v8/+/f77c51b)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-352058.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-352058.js)  
```javascript
var v0 = this;
var v2 = this;
function f() {
  v2 = [1.2, 2.3];
  v0 = [12, 23];
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f77c51b^!)  
[src/hydrogen-check-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-check-elimination.cc?cl=f77c51b)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=f77c51b)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=f77c51b)  
[test/mjsunit/regress/regress-crbug-352058.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-352058.js?cl=f77c51b)  
  

### **crbug:351787**  
   
**[Issue: Pwnium 4: v8 OOB read/write with __defineGetter__ and bytesLength](https://crbug.com/351787)**  
**[Commit: Use intrinsics for builtin ArrayBuffer property accesses](https://chromium.googlesource.com/v8/v8/+/f9ee4f1)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Release-3-M33", "ZDI-CAN-2233", "CVE-2014-1705", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Regress: [mjsunit/regress/regress-crbug-351787.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-351787.js)  
```javascript
var ab1 = new ArrayBuffer(8);
ab1.__defineGetter__("byteLength", function() { return 1000000; });
var ab2 = ab1.slice(800000, 900000);
var array = new Uint8Array(ab2);
for (var i = 0; i < array.length; i++) {
  assertEquals(0, array[i]);
}
assertEquals(0, array.length);


var ab3 = new ArrayBuffer(8);
ab3.__defineGetter__("byteLength", function() { return 0xFFFFFFFC; });
var aaa = new DataView(ab3);

for (var i = 10; i < aaa.length; i++) {
  aaa.setInt8(i, 0xcc);
}
assertEquals(8, aaa.byteLength);


var a = new Int8Array(4);
a.__defineGetter__("length", function() { return 0xFFFF; });
var b = new Int8Array(a);
for (var i = 0; i < b.length; i++) {
  assertEquals(0, b[i]);
}


var ab4 = new ArrayBuffer(8);
ab4.__defineGetter__("byteLength", function() { return 0xFFFFFFFC; });
var aaaa = new Uint32Array(ab4);

for (var i = 10; i < aaaa.length; i++) {
  aaaa[i] = 0xcccccccc;
}
assertEquals(2, aaaa.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f9ee4f1^!)  
[src/arraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/arraybuffer.js?cl=f9ee4f1)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=f9ee4f1)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=f9ee4f1)  
[test/mjsunit/regress/regress-crbug-351787.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-351787.js?cl=f9ee4f1)  
  

### **crbug:351658**  
   
**[Issue: CHECK failure in CHECK(op < BinaryOpIC::State::FIRST_TOKEN || op > BinaryOpIC::State::LAST_TOKEN) failed: ../src/type](https://crbug.com/351658)**  
**[Commit: Make invalid LHSs a parse-time (reference) error](https://chromium.googlesource.com/v8/v8/+/c3c185c)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-351658.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-351658.js)  
```javascript
try {
  var f = eval("(function(){0 = y + y})");
  %OptimizeFunctionOnNextCall(f);
  f();
  assertUnreachable();
} catch(e) {
  assertTrue(e instanceof ReferenceError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3c185c^!)  
[src/a64/full-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/full-codegen-a64.cc?cl=c3c185c)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c3c185c)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=c3c185c)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=c3c185c)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=c3c185c)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c3c185c)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=c3c185c)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=c3c185c)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=c3c185c)  
[src/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/preparser.cc?cl=c3c185c)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=c3c185c)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=c3c185c)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=c3c185c)  
[test/mjsunit/invalid-lhs.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/invalid-lhs.js?cl=c3c185c)  
[test/mjsunit/regress/regress-crbug-351658.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-351658.js?cl=c3c185c)  
  

### **crbug:351320**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/351320)**  
**[Commit: Fix HIsSmiAndBranch::KnownSuccessorBlock() by deleting it](https://chromium.googlesource.com/v8/v8/+/105c1e0)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["M-35", "Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-351320.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-351320.js)  
```javascript
var result = 0;
var o1 = {};
o2 = {y:1.5};
o2.y = 0;
o3 = o2.y;

function crash() {
  for (var i = 0; i < 10; i++) {
    result += o1.x + o3.foo;
  }
}

crash();
%OptimizeFunctionOnNextCall(crash);
crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/105c1e0^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=105c1e0)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=105c1e0)  
[test/mjsunit/regress/regress-crbug-351320.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-351320.js?cl=105c1e0)  
  

### **crbug:351262**  
   
**[Issue: CHECK failure in CHECK(!curr->IsAccessCheckNeeded()) failed: ../src/objects.cc(5919)](https://crbug.com/351262)**  
**[Commit: fix bad access check check](https://chromium.googlesource.com/v8/v8/+/62fc099)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-351262.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-351262.js)  
```javascript
for (var x in this) {};
JSON.stringify(this);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/62fc099^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=62fc099)  
[test/mjsunit/regress/regress-crbug-351262.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-351262.js?cl=62fc099)  
  

### **crbug:350890**  
   
**[Issue: CHECK failure in CHECK(debug_lookup.IsPropertyCallbacks() && !debug_lookup.IsReadOnly()) failed: ../src/ic.cc(1808)](https://crbug.com/350890)**  
**[Commit: Fixed spec violation of storing to length of a frozen object.](https://chromium.googlesource.com/v8/v8/+/3b257c3)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-350890.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-350890.js)  
```javascript
function set_length(a, l) {
  a.length = l;
}

function test1() {
  var l = {};
  var a = Array(l);
  set_length(a, 3);
  set_length(a, 3);
  assertEquals(3, a.length);
}

function test2() {
  var a = [];
  set_length(a, 10);
  set_length(a, 10);
  Object.freeze(a);
  set_length(a, 3);
  set_length(a, 3);
  assertEquals(10, a.length);
}

function test3() {
  var a = [2];
  Object.defineProperty(a, "length", {value:2, writable: false});
  %ToFastProperties(a);
  set_length([], 10);
  set_length([], 10);
  set_length(a, 10);
  set_length(a, 10);
  assertEquals(2, a.length);
}

test1();
test2();
test3();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b257c3^!)  
[src/a64/code-stubs-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/code-stubs-a64.cc?cl=3b257c3)  
[src/a64/stub-cache-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/stub-cache-a64.cc?cl=3b257c3)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=3b257c3)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=3b257c3)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=3b257c3)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=3b257c3)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=3b257c3)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=3b257c3)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=3b257c3)  
[src/stub-cache.h](https://cs.chromium.org/chromium/src/v8/src/stub-cache.h?cl=3b257c3)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=3b257c3)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=3b257c3)  
[test/mjsunit/regress/regress-crbug-350890.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-350890.js?cl=3b257c3)  
  

### **crbug:350867**  
   
**[Issue: CHECK failure in CHECK(elements_kind == DICTIONARY_ELEMENTS) failed: ../src/stub-cache.cc(1309)](https://crbug.com/350867)**  
**[Commit: Fix polymorphic keyed loads for SLOPPY_ARGUMENTS_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/d9b6b64)**  
  
Closed: Mar 2014  
Type: Bug  
Components: None  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-350867.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-350867.js)  
```javascript
function f1(a, i) {
  return a[i];
}
function f2(a, b, c, index) {
  return f1(arguments, index);
}

f2(2, 3, 4, "foo");
f2(2, 3, 4, "foo");
assertEquals(11, f1([11, 22, 33], 0));
assertEquals(22, f2(22, 33, 44, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9b6b64^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=d9b6b64)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=d9b6b64)  
[test/mjsunit/regress/regress-crbug-350867.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-350867.js?cl=d9b6b64)  
  

### **crbug:350864**  
   
**[Issue: CHECK failure in CHECK(index >= 0 && index < this->length()) failed: ../src/objects-inl.h(2124)](https://crbug.com/350864)**  
**[Commit: Fix issue with getOwnPropertySymbols and hidden properties](https://chromium.googlesource.com/v8/v8/+/85800ef)**  
  
Closed: Mar 2014  
Type: Bug  
Components: None  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-350864.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-350864.js)  
```javascript
var v0 = new WeakMap;
var v1 = {};
v0.set(v1, 1);
var sym = Symbol();
v1[sym] = 1;
var symbols = Object.getOwnPropertySymbols(v1);
assertArrayEquals([sym], symbols);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85800ef^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=85800ef)  
[test/mjsunit/regress/regress-crbug-350864.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-350864.js?cl=85800ef)  
  

### **crbug:350434**  
   
**[Issue: [LangFuzz] Crash with jump to invalid address](https://crbug.com/350434)**  
**[Commit: Fix lazy deopt after tagged binary ops](https://chromium.googlesource.com/v8/v8/+/8a1812f)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["reward-2000", "Release-0-M34", "M-33", "Via-Wizard", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "CVE-2014-1721", "allpublic", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-350434.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-350434.js)  
```javascript
function Ctor() {
  this.foo = 1;
}

var o = new Ctor();
var p = new Ctor();


function crash(o, timeout) {
  var s = "4000111222";  
  %SetAllocationTimeout(100000, timeout);
  
  var end = s >>> 0;
  s = s.substring(0, end);
  
  
  o.bar = 2;
}

crash(o, 100000);
crash(o, 100000);
crash(p, 100000);
%OptimizeFunctionOnNextCall(crash);
crash(o, 100000);
o = null;
p = null;
crash({}, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8a1812f^!)  
[src/a64/lithium-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-a64.cc?cl=8a1812f)  
[src/a64/lithium-a64.h](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-a64.h?cl=8a1812f)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=8a1812f)  
[src/a64/lithium-codegen-a64.h](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.h?cl=8a1812f)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=8a1812f)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=8a1812f)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=8a1812f)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=8a1812f)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=8a1812f)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=8a1812f)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=8a1812f)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=8a1812f)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8a1812f)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=8a1812f)  
[src/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.h?cl=8a1812f)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=8a1812f)  
[src/mips/lithium-codegen-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.h?cl=8a1812f)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=8a1812f)  
[src/mips/lithium-mips.h](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.h?cl=8a1812f)  
[src/safepoint-table.h](https://cs.chromium.org/chromium/src/v8/src/safepoint-table.h?cl=8a1812f)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=8a1812f)  
[src/x64/lithium-codegen-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.h?cl=8a1812f)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=8a1812f)  
[src/x64/lithium-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.h?cl=8a1812f)  
[test/mjsunit/regress/regress-crbug-350434.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-350434.js?cl=8a1812f)  
  

### **crbug:349878**  
   
**[Issue: No Permission](https://crbug.com/349878)**  
**[Commit: Fix HConstants with Smi-ranged HeapNumber values](https://chromium.googlesource.com/v8/v8/+/1cc0baf)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-349878.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-349878.js)  
```javascript
function f(a, b) {
  a == b;
}

f({}, {});

var a = { y: 1.5 };
a.y = 777;
var b = a.y;

function h() {
  var d = 1;
  var e = 777;
  while (d-- > 0) e++;
  f(1, e);
}

var global;
function g() {
  global = b;
  return h(b);
}

g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1cc0baf^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=1cc0baf)  
[test/mjsunit/regress/regress-crbug-349878.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-349878.js?cl=1cc0baf)  
  

### **crbug:349853**  
   
**[Issue: CHECK failure in CHECK(!found) failed: ../src/lithium-allocator.cc(1381)](https://crbug.com/349853)**  
**[Commit: Let HTransitionElementsKind take part in RestoreActualValues phase](https://chromium.googlesource.com/v8/v8/+/5ea3f00)**  
  
Closed: Mar 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-349853.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-349853.js)  
```javascript
var a = ["string"];
function funky(array) { return array[0] = 1; }
funky(a);

function crash() {
  var q = [0];
  
  for (var i = 0; i < 100000; i++) {
    funky(q);
  }
  q[0] = 0;
  funky(q)
}

crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5ea3f00^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=5ea3f00)  
[test/mjsunit/regress/regress-crbug-349853.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-349853.js?cl=5ea3f00)  
  

### **crbug:349465**  
   
**[Issue: UNKNOWN in v8::internal::JSFunction::context](https://crbug.com/349465)**  
**[Commit: Fix for failing asserts in HBoundsCheck code generation on x64: use proper cmp operation width instead of asserting that Integer32 values should be zero extended. Similar to chromium:345820.](https://chromium.googlesource.com/v8/v8/+/997ce05)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-349465.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-349465.js)  
```javascript
function f(a, base) {
  a[base] = 1;
  a[base + 4] = 2;
  a[base] = 3;
}
var a1 = new Array(1024);
var a2 = new Array(128);
f(a1, 1);
f(a2, -2);
%OptimizeFunctionOnNextCall(f);
f(a1, -2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/997ce05^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=997ce05)  
[test/mjsunit/regress/regress-crbug-349465.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-349465.js?cl=997ce05)  
  

### **crbug:349079**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/349079)**  
**[Commit: x64: Fix LMathMinMax for constant Smi right-hand operands](https://chromium.googlesource.com/v8/v8/+/3df5573)**  
  
Closed: Mar 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["CVE-2014-1704", "Stability-Memory-AddressSanitizer", "Release-2-M33", "M-33", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-349079.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-349079.js)  
```javascript
function assertEquals(expected, found) {
  return found === expected;
};
%NeverOptimizeFunction(assertEquals);

function crash() {
  var a = 1;
  var b = -0;
  var c = 1.5;
  assertEquals(b, Math.max(b++, c++));
  assertEquals(c, Math.min(b++, c++));
  assertEquals(b, Math.max(b++, a++));
}
crash();
crash();
%OptimizeFunctionOnNextCall(crash);
crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3df5573^!)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=3df5573)  
[test/mjsunit/regress/regress-crbug-349079.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-349079.js?cl=3df5573)  
  

### **crbug:347903**  
   
**[Issue: CHECK failure in CHECK((IsFastSmiOrObjectElementsKind(kind) && (map == GetHeap()->fixed_array_map() || map == GetHeap](https://crbug.com/347903)**  
**[Commit: A JSArray may have a filler map in the elements pointer.](https://chromium.googlesource.com/v8/v8/+/b1ffc79)**  
  
Closed: Feb 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-347903.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-347903.js)  
```javascript
function f() {
  var a = new Array(84632);
  
  
  var b = new Array(84632);
  var c = new Array(84632);
  return [a, b, c];
}
f(); f();
%OptimizeFunctionOnNextCall(f);
for(var i = 0; i < 10; i++) {
  f();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1ffc79^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=b1ffc79)  
[test/mjsunit/regress/regress-crbug-347903.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-347903.js?cl=b1ffc79)  
  

### **crbug:347528**  
   
**[Issue: CHECK failure in CHECK(IsNativeContext()) failed: ../src/contexts.h(462)](https://crbug.com/347528)**  
**[Commit: Get array_function from NativeContext](https://chromium.googlesource.com/v8/v8/+/98d1ced)**  
  
Closed: Feb 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Security_severity-None", "M-34", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/harmony/regress/regress-crbug-347528.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-crbug-347528.js)  
```javascript
"use strict";
let unused_var = 1;
function __f_12() { new Array(); }
__f_12();
__f_12();
%OptimizeFunctionOnNextCall(__f_12);
__f_12();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/98d1ced^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=98d1ced)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=98d1ced)  
[test/mjsunit/regress/regress-crbug-347528.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-347528.js?cl=98d1ced)  
  

### **crbug:346636**  
   
**[Issue: UNKNOWN in v8::internal::Range::CanBeZero](https://crbug.com/346636)**  
**[Commit: Mark HCompareMap as having Tagged representation](https://chromium.googlesource.com/v8/v8/+/e7e93cd)**  
  
Closed: Feb 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-346636.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-346636.js)  
```javascript
function assertSame(expected, found) {
  if (found === expected) {
    if (expected !== 0 || (1 / expected) == (1 / found)) return;
  }
  return;
};

function foo(x) {
  return x.bar;
}

function getter1() {
  assertSame(this, this);
}
var o1 = Object.defineProperty({}, "bar", { get: getter1 });
foo(o1);
foo(o1);

function getter2() {
  assertSame(this, this);
}
var o2 = Object.defineProperty({}, "bar", { get: getter2 });
foo(o2);
%OptimizeFunctionOnNextCall(foo);
foo(o2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e7e93cd^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=e7e93cd)  
[test/mjsunit/regress/regress-crbug-346636.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-346636.js?cl=e7e93cd)  
  

### **crbug:346141**  
   
**[Issue: Global-buffer-overflow in GetVisitor](https://crbug.com/346141)**  
**[Commit: Fix crasher in Object.getOwnPropertySymbols](https://chromium.googlesource.com/v8/v8/+/63f1970)**  
  
Closed: Feb 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Security_severity-None", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/es6/regress/regress-crbug-346141.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-346141.js)  
```javascript
var s = Symbol()
var o = {}
o[s] = 2
o[""] = 3
Object.getOwnPropertySymbols(o)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63f1970^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=63f1970)  
[test/mjsunit/regress/regress-crbug-346141.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-346141.js?cl=63f1970)  
  

### **crbug:345820**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/345820)**  
**[Commit: Fix for failing asserts in HBoundsCheck code generation on x64: index register should be zero extended.](https://chromium.googlesource.com/v8/v8/+/1ae7e8a)**  
  
Closed: Feb 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Release-0-M34", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-345820.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-345820.js)  
```javascript
var __v_6 = {};
__v_6 = new Int32Array(5);
for (var i = 0; i < __v_6.length; i++) __v_6[i] = 0;

function __f_7(N) {
  for (var i = -1; i < N; i++) {
    __v_6[i] = i;
  }
}
__f_7(1);
%OptimizeFunctionOnNextCall(__f_7);
__f_7(__v_6.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ae7e8a^!)  
[src/x64/disasm-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/disasm-x64.cc?cl=1ae7e8a)  
[src/x64/lithium-gap-resolver-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-gap-resolver-x64.cc?cl=1ae7e8a)  
[test/mjsunit/regress/regress-crbug-345820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-345820.js?cl=1ae7e8a)  
  

### **crbug:345715**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/345715)**  
**[Commit: Fix for a smi stores optimization on x64 with a regression test.](https://chromium.googlesource.com/v8/v8/+/6c1659b)**  
  
Closed: Feb 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["CVE-2014-1704", "Stability-Memory-AddressSanitizer", "Release-2-M33", "M-33", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-345715.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-345715.js)  
```javascript
a = {y:1.5};
a.y = 0;
b = a.y;
c = {y:{}};

function f() {
  return 1;
}

function g() {
  var e = {y: b};
  var d = {x:f()};
  var d = {x:f()};
  return [e, d];
}

g();
g();
%OptimizeFunctionOnNextCall(g);
assertEquals(1, g()[1].x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c1659b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6c1659b)  
[test/mjsunit/regress/regress-crbug-345715.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-345715.js?cl=6c1659b)  
  

### **crbug:344186**  
   
**[Issue: OOB write due to invalid bounds check in v8](https://crbug.com/344186)**  
**[Commit: Fix Hydrogen bounds check elimination](https://chromium.googlesource.com/v8/v8/+/6e3b81a)**  
  
Closed: Feb 2014  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "M-33", "M-34", "Merge-Merged", "Security_Impact-Stable", "Release-1-M33", "CVE-2013-6668", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-344186.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-344186.js)  
```javascript
var dummy = new Int32Array(100);
var array = new Int32Array(128);
function fun(base) {
  array[base - 95] = 1;
  array[base - 99] = 2;
  array[base + 4] = 3;
}
fun(100);
%OptimizeFunctionOnNextCall(fun);
fun(0);

for (var i = 0; i < dummy.length; i++) {
  assertEquals(0, dummy[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6e3b81a^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=6e3b81a)  
[test/mjsunit/regress/regress-crbug-344186.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-344186.js?cl=6e3b81a)  
  

### **crbug:340064**  
   
**[Issue: Chrome: Crash Report - Magic Signature: v8::internal::HOptimizedGraphBuilder::Prope...](https://crbug.com/340064)**  
**[Commit: Return a valid map for PropertyAccessInfos with Boolean type.](https://chromium.googlesource.com/v8/v8/+/db7124d)**  
  
Closed: Feb 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-34"]  
Regress: [mjsunit/regress/regress-crbug-340064.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-340064.js)  
```javascript
function f(v) {
  return v.length;
}

assertEquals(4, f("test"));
assertEquals(4, f("test"));
assertEquals(undefined, f(true));
%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db7124d^!)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=db7124d)  
[test/mjsunit/regress/regress-crbug-340064.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-340064.js?cl=db7124d)  
  

### **crbug:336148**  
   
**[Issue: TypeError as a result of undefined variable from a RegEx exec](https://crbug.com/336148)**  
**[Commit: Fix short-circuiting logical and/or in HOptimizedGraphBuilder.](https://chromium.googlesource.com/v8/v8/+/9e70f6a)**  
  
Closed: Feb 2014  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-33", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-336148.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-336148.js)  
```javascript
function f(o) {
  var a = 1;
  if (true) return o.v && a;
}

f({});
f({});
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f({ v: 1 }));


function f1() { return 1 && 2; };
function f2() { return 1 || 2; };
function f3() { return 0 && 2; };
function f4() { return 0 || 2; };

function test() {
  assertEquals(2, f1());
  assertEquals(1, f2());
  assertEquals(0, f3());
  assertEquals(2, f4());
}

test();
test();
[f1, f2, f3, f4].forEach(function(f) { %OptimizeFunctionOnNextCall(f); });
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e70f6a^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=9e70f6a)  
[test/mjsunit/regress/regress-crbug-336148.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-336148.js?cl=9e70f6a)  
  

### **crbug:329709**  
   
**[Issue: Maps test crashing on GPU bots after V8 roll](https://crbug.com/329709)**  
**[Commit: Fix switch statements with non-Smi integer labels and no type feedback](https://chromium.googlesource.com/v8/v8/+/3c76ecd)**  
  
Closed: Dec 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-329709.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-329709.js)  
```javascript
function boom(x) {
  switch(x) {
    case 1: return "one";
    case 1500000000: return "non-smi int32";
    default: return "default";
  }
}

assertEquals("one", boom(1));
assertEquals("one", boom(1));
%OptimizeFunctionOnNextCall(boom)
assertEquals("non-smi int32", boom(1500000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c76ecd^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3c76ecd)  
[test/mjsunit/regress/regress-crbug-329709.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-329709.js?cl=3c76ecd)  
  

### **crbug:325225**  
   
**[Issue: Crash on keyed load invocation](https://crbug.com/325225)**  
**[Commit: Check whether the receiver to a keyed-call is actually a heapobject.](https://chromium.googlesource.com/v8/v8/+/d4eaae3)**  
  
Closed: Dec 2013  
Type: Bug-Security  
Components: None  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "M-33", "Security_Severity-Medium", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-325225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-325225.js)  
```javascript
function f1(a) {
  a[0](0);
}

function do1() {
  f1([f1]);
}

assertThrows(do1, TypeError);

function f2(a) {
  a[0](true);
}

function do2() {
  f2([function(a) { return f2("undefined", typeof f2(42, 0)); }]);
}

assertThrows(do2, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d4eaae3^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=d4eaae3)  
[test/mjsunit/regress/regress-crbug-325225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-325225.js?cl=d4eaae3)  
  

### **crbug:323942**  
   
**[Issue: No Permission](https://crbug.com/323942)**  
**[Commit: Fix bug in inlining Function.apply.](https://chromium.googlesource.com/v8/v8/+/f235194)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-323942.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-323942.js)  
```javascript
"use strict";


var holder = { f: function() { return 42; } };
var receiver = { };
receiver.__proto__ = { };
receiver.__proto__.__proto__ = holder;


function h(o) { return o.f.apply(this, arguments); }
function g(o) { return h(o); }


assertEquals(42, g(receiver));
assertEquals(42, g(receiver));



receiver.__proto__.__proto__ = {};


%OptimizeFunctionOnNextCall(g);

assertThrows(function() { g(receiver); });


receiver.__proto__.__proto__ = holder;
assertEquals(42, g(receiver));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f235194^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=f235194)  
[test/mjsunit/regress/regress-crbug-323942.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-323942.js?cl=f235194)  
  

### **crbug:319860**  
   
**[Issue: OOB read in V8](https://crbug.com/319860)**  
**[Commit: Limit size of dehoistable array indices](https://chromium.googlesource.com/v8/v8/+/c9b41c6)**  
  
Closed: Nov 2013  
Type: Bug-Security  
Components: None  
Labels: ["Release-1-M31", "Stability-ThreadSanitizer", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "CVE-2013-6640", "M-31", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Regress: [mjsunit/regress/regress-crbug-319860.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-319860.js)  
```javascript
function read(a, index) {
  var offset = 0x2000000;
  var result;
  for (var i = 0; i < 1; i++) {
    result = a[index + offset];
  }
  return result;
}

var a = new Int8Array(0x2000001);
read(a, 0);
read(a, 0);
%OptimizeFunctionOnNextCall(read);


for (var i = 0; i > -1000000; --i) {
  read(a, i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9b41c6^!)  
[src/elements-kind.cc](https://cs.chromium.org/chromium/src/v8/src/elements-kind.cc?cl=c9b41c6)  
[src/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/elements-kind.h?cl=c9b41c6)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=c9b41c6)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=c9b41c6)  
[src/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/lithium.cc?cl=c9b41c6)  
[src/lithium.h](https://cs.chromium.org/chromium/src/v8/src/lithium.h?cl=c9b41c6)  
[test/mjsunit/regress/regress-crbug-319835.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-319835.js?cl=c9b41c6)  
[test/mjsunit/regress/regress-crbug-319860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-319860.js?cl=c9b41c6)  
  

### **crbug:319835**  
   
**[Issue: OOB write in V8 (only 64bit)](https://crbug.com/319835)**  
**[Commit: Limit size of dehoistable array indices](https://chromium.googlesource.com/v8/v8/+/c9b41c6)**  
  
Closed: Nov 2013  
Type: Bug-Security  
Components: None  
Labels: ["Release-1-M31", "Stability-ThreadSanitizer", "Merge-Merged", "Security_Impact-Stable", "Arch-x86_64", "CVE-2013-6639", "M-31", "Security_Severity-High", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Regress: [mjsunit/regress/regress-crbug-319835.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-319835.js)  
```javascript
try {} catch(e) {}  

var size = 0x20000;
var a = new Float64Array(size);
var training = new Float64Array(10);
function store(a, index) {
  var offset = 0x20000000;
  for (var i = 0; i < 1; i++) {
    a[index + offset] = 0xcc;
  }
}

store(training, -0x20000000);
store(training, -0x20000000 + 1);
store(training, -0x20000000);
store(training, -0x20000000 + 1);
%OptimizeFunctionOnNextCall(store);


for (var i = -0x20000000; i < -0x20000000 + size; i++) {
  store(a, i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9b41c6^!)  
[src/elements-kind.cc](https://cs.chromium.org/chromium/src/v8/src/elements-kind.cc?cl=c9b41c6)  
[src/elements-kind.h](https://cs.chromium.org/chromium/src/v8/src/elements-kind.h?cl=c9b41c6)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=c9b41c6)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=c9b41c6)  
[src/lithium.cc](https://cs.chromium.org/chromium/src/v8/src/lithium.cc?cl=c9b41c6)  
[src/lithium.h](https://cs.chromium.org/chromium/src/v8/src/lithium.h?cl=c9b41c6)  
[test/mjsunit/regress/regress-crbug-319835.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-319835.js?cl=c9b41c6)  
[test/mjsunit/regress/regress-crbug-319860.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-319860.js?cl=c9b41c6)  
  

### **crbug:318671**  
   
**[Issue: Weird illegal access exception](https://crbug.com/318671)**  
**[Commit: Fix missing type feedback check for Generic*String addition.](https://chromium.googlesource.com/v8/v8/+/2ee5aa9)**  
  
Closed: Nov 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-318671.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-318671.js)  
```javascript
function add(x, y) { return x + y; }

print(add({ a: 1 }, "a"));
print(add({ b: 1 }, "b"));
print(add({ c: 1 }, "c"));

%OptimizeFunctionOnNextCall(add);

print(add("a", 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ee5aa9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2ee5aa9)  
[test/mjsunit/regress/regress-crbug-318671.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-318671.js?cl=2ee5aa9)  
  

### **crbug:315252**  
   
**[Issue: No Permission](https://crbug.com/315252)**  
**[Commit: Turn Runtime_MigrateInstance into Runtime_TryMigrateInstance](https://chromium.googlesource.com/v8/v8/+/1ed94ac)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-315252.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-315252.js)  
```javascript
function f(a, b, c) {
 this.a = a;
 this.b = b;
 this.c = c;
}
var o3 = new f(1, 2, 3.5);
var o4 = new f(1, 2.5, 3);
var o1 = new f(1.5, 2, 3);
var o2 = new f(1.5, 2, 3);
function migrate(o) {
 return o.a;
}

migrate(o4);
migrate(o1);
migrate(o2);
function store_transition(o) {
 o.d = 1;
}



store_transition(o4);
store_transition(o1);
store_transition(o2);
%OptimizeFunctionOnNextCall(store_transition);





store_transition(o3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1ed94ac^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=1ed94ac)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=1ed94ac)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=1ed94ac)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=1ed94ac)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=1ed94ac)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=1ed94ac)  
[test/mjsunit/regress/regress-crbug-315252.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-315252.js?cl=1ed94ac)  
  

### **crbug:309623**  
   
**[Issue: Wrong WebGL display](https://crbug.com/309623)**  
**[Commit: Fix uint32-to-smi conversion in Lithium](https://chromium.googlesource.com/v8/v8/+/316271f)**  
  
Closed: Oct 2013  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["M-30", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-309623.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-309623.js)  
```javascript
var u = new Uint32Array(2);
u[0] = 1;
u[1] = 0xEE6B2800;

var a = [0, 1, 2];
a[0] = 0;  
assertTrue(%HasSmiElements(a));

function foo(i) {
  a[0] = u[i];
  return a[0];
}

assertEquals(u[0], foo(0));
assertEquals(u[0], foo(0));
%OptimizeFunctionOnNextCall(foo);
assertEquals(u[1], foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/316271f^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=316271f)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=316271f)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=316271f)  
[src/hydrogen-uint32-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-uint32-analysis.cc?cl=316271f)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=316271f)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=316271f)  
[src/ia32/lithium-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.h?cl=316271f)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=316271f)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=316271f)  
[src/x64/lithium-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.h?cl=316271f)  
[test/mjsunit/regress/regress-crbug-309623.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-309623.js?cl=316271f)  
  

### **crbug:306851**  
   
**[Issue: Object.defineProperty with postfix increment or decrement operator stalls after 132 iterations.](https://crbug.com/306851)**  
**[Commit: Add regression test for optimized count operation.](https://chromium.googlesource.com/v8/v8/+/0a2b4ec)**  
  
Closed: Sep 2015  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-306851.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-306851.js)  
```javascript
function Counter() {
  this.value = 0;
};

Object.defineProperty(Counter.prototype, 'count', {
  get: function() { return this.value; },
  set: function(value) { this.value = value; }
});

var obj = new Counter();

function bummer() {
  obj.count++;
  return obj.count;
}

assertEquals(1, bummer());
assertEquals(2, bummer());
assertEquals(3, bummer());
%OptimizeFunctionOnNextCall(bummer);
assertEquals(4, bummer());
assertEquals(5, bummer());
assertEquals(6, bummer());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a2b4ec^!)  
[test/mjsunit/regress/regress-crbug-306851.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-306851.js?cl=0a2b4ec)  
  

### **crbug:306220**  
   
**[Issue: Error.prototype.toString calls message getter with wrong this object](https://crbug.com/306220)**  
**[Commit: Correctly load message from an Error object.](https://chromium.googlesource.com/v8/v8/+/a5ed9a7)**  
  
Closed: Nov 2013  
Type: Bug  
Components: None  
Labels: ["Via-Wizard", "Needs-Feedback"]  
Regress: [mjsunit/regress/regress-crbug-306220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-306220.js)  
```javascript
var CustomError = function(x) { this.x = x; };
CustomError.prototype = new Error();
CustomError.prototype.x = "prototype";

Object.defineProperties(CustomError.prototype, {
   'message': {
      'get': function() { return this.x; }
   }
});

assertEquals("Error: instance", String(new CustomError("instance")));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a5ed9a7^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=a5ed9a7)  
[test/mjsunit/regress/regress-crbug-306220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-306220.js?cl=a5ed9a7)  
  

### **crbug:305309**  
   
**[Issue: Property lookups sometimes return the wrong property's value](https://crbug.com/305309)**  
**[Commit: Fix HObjectAccess for loads from migrating prototypes](https://chromium.googlesource.com/v8/v8/+/8259439)**  
  
Closed: Nov 2013  
Type: Bug-Regression  
Components: None  
Labels: ["M-30", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-305309.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-305309.js)  
```javascript
function BadProto() {
  this.constant_function = function() {};
  this.one = 1;
  this.two = 2;
}
var b1 = new BadProto();
var b2 = new BadProto();

function Ctor() {}
Ctor.prototype = b1;
var a = new Ctor();

function Two(x) {
  return x.two;
}
assertEquals(2, Two(a));
assertEquals(2, Two(a));
b2.constant_function = "no longer constant!";
%OptimizeFunctionOnNextCall(Two);
assertEquals(2, Two(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8259439^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=8259439)  
[test/mjsunit/regress/regress-crbug-305309.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-305309.js?cl=8259439)  
  

### **crbug:285355**  
   
**[Issue: Chrome_Linux: Crash Report - Magic Signature: v8::internal::Runtime_LazyRecompile](https://crbug.com/285355)**  
**[Commit: Fix bitwise negation on x64](https://chromium.googlesource.com/v8/v8/+/daee0d8)**  
  
Closed: Sep 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-285355.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-285355.js)  
```javascript
function inverted_index() {
  return ~1;
}

%NeverOptimizeFunction(inverted_index);

function crash(array) {
  return array[~inverted_index()] = 2;
}

assertEquals(2, crash(new Array(1)));
assertEquals(2, crash(new Array(1)));
%OptimizeFunctionOnNextCall(crash)
assertEquals(2, crash(new Array(1)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/daee0d8^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=daee0d8)  
[test/mjsunit/regress/regress-crbug-285355.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-285355.js?cl=daee0d8)  
  

### **crbug:280333**  
   
**[Issue: No Permission](https://crbug.com/280333)**  
**[Commit: Always visit branches during HGraph building](https://chromium.googlesource.com/v8/v8/+/2c9ac9c)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-280333.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-280333.js)  
```javascript
function funky() { return false; }
var global;

function foo(x, fun) {
  var a = x + 1;
  var b = x + 2;  
  global = true;  
  if (fun()) {
    return a;
  }
  return 0;
}

assertEquals(0, foo(1, funky));
assertEquals(0, foo(1, funky));
%OptimizeFunctionOnNextCall(foo);
assertEquals(0, foo(1, funky));
assertEquals(2, foo(1, function() { return true; }));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2c9ac9c^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2c9ac9c)  
[test/mjsunit/regress/regress-crbug-280333.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-280333.js?cl=2c9ac9c)  
  

### **crbug:274438**  
   
**[Issue: Chrome: Crash Report - Magic Signature: V8_Fatal](https://crbug.com/274438)**  
**[Commit: Mark HStringCompareAndBranch as potentially causing GCs.](https://chromium.googlesource.com/v8/v8/+/3e4fbd0)**  
  
Closed: Aug 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-274438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-274438.js)  
```javascript
function f(a, b) {
  var x = { a:a };
  switch(b) { case "string": }
  var y = { b:b };
  return y;
}

f("a", "b");
f("a", "b");
%OptimizeFunctionOnNextCall(f);
f("a", "b");
%SetAllocationTimeout(100, 0);
var killer = f("bang", "bo" + "om");
assertEquals("boom", killer.b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e4fbd0^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=3e4fbd0)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=3e4fbd0)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=3e4fbd0)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3e4fbd0)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=3e4fbd0)  
[test/mjsunit/regress/regress-crbug-274438.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-274438.js?cl=3e4fbd0)  
  

### **crbug:272564**  
   
**[Issue: Chrome_Mac: Crash Report - Magic Signature: v8::internal::TypeInfo::FromValue](https://crbug.com/272564)**  
**[Commit: Fix Math.round/floor that had bogus Smi representation](https://chromium.googlesource.com/v8/v8/+/e71a91c)**  
  
Closed: Aug 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-272564.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-272564.js)  
```javascript
function Bb(w) {
  this.width = w;
}

function ce(a, b) {
  "number" == typeof a && (a = (b ? Math.round(a) : a) + "px");
  return a
}

function pe(a, b, c) {
  if (b instanceof Bb) b = b.width;
  a.width = ce(b, !0);
}

var a = new Bb(1);
var b = new Bb(5);
pe(a, b, 0);
pe(a, b, 0);
%OptimizeFunctionOnNextCall(pe);
pe(a, b, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e71a91c^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=e71a91c)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=e71a91c)  
[test/mjsunit/regress/regress-crbug-272564.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-272564.js?cl=e71a91c)  
  

### **crbug:263276**  
   
**[Issue: No Permission](https://crbug.com/263276)**  
**[Commit: Fix JSArray-specific length lookup in polymorphic array handling](https://chromium.googlesource.com/v8/v8/+/32e2e37)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-263276.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-263276.js)  
```javascript
var array1 = [];
array1.foo = true;

var array2 = [];
array2.bar = true;

function bad(array) {
  array[array.length] = 1;
}

bad(array1);
bad(array1);
bad(array2);  
bad(array2);  
%OptimizeFunctionOnNextCall(bad);
bad(array2);  
assertEquals(3, array2.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/32e2e37^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=32e2e37)  
[test/mjsunit/regress/regress-crbug-263276.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-263276.js?cl=32e2e37)  
  

### **crbug:260345**  
   
**[Issue: [Crash] v8::internal::StackFrameIterator::AdvanceWithHandler](https://crbug.com/260345)**  
**[Commit: Synchronize Compare-Literal behavior in FullCodegen and Hydrogen](https://chromium.googlesource.com/v8/v8/+/22f2fd8)**  
  
Closed: Jul 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Hotlist-GoogleApps"]  
Regress: [mjsunit/regress/regress-crbug-260345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-260345.js)  
```javascript
var steps = 100000;
var undefined_values = [undefined, "go on"];
var null_values = [null, "go on"];

function get_undefined_object(i) {
  return undefined_values[(i / steps) | 0];
}

function test_undefined() {
  var objects = 0;
  for (var i = 0; i < 2 * steps; i++) {
    undefined == get_undefined_object(i) && objects++;
  }
  return objects;
}

assertEquals(steps, test_undefined());


function get_null_object(i) {
  return null_values[(i / steps) | 0];
}

function test_null() {
  var objects = 0;
  for (var i = 0; i < 2 * steps; i++) {
    null == get_null_object(i) && objects++;
  }
  return objects;
}

assertEquals(steps, test_null());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22f2fd8^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=22f2fd8)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=22f2fd8)  
[test/mjsunit/regress/regress-crbug-260345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-260345.js?cl=22f2fd8)  
  

### **crbug:258519**  
   
**[Issue: Impossible "Cannot call method 'l$' of undefined" error](https://crbug.com/258519)**  
**[Commit: Add regression test for recently fixed bug](https://chromium.googlesource.com/v8/v8/+/3619dcf)**  
  
Closed: Aug 2013  
Type: Bug-Regression  
Components: None  
Labels: ["Via-Wizard", "Needs-Feedback", "Hotlist-GoogleApps"]  
Regress: [mjsunit/regress/regress-crbug-258519.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-258519.js)  
```javascript
var a = {
  compare_null: function(x) { return null != x; },
  kaboom: function() {}
}

function crash(x) {
  var b = a;
  b.compare_null(x) && b.kaboom();
  return "ok";
}

assertEquals("ok", crash(null));
assertEquals("ok", crash(null));
%OptimizeFunctionOnNextCall(crash);

assertEquals("ok", crash(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3619dcf^!)  
[test/mjsunit/regress/regress-crbug-258519.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-258519.js?cl=3619dcf)  
  

### **crbug:248025**  
   
**[Issue: No Permission](https://crbug.com/248025)**  
**[Commit: Fix crasher when checking for "of", but next token has no literal buffer](https://chromium.googlesource.com/v8/v8/+/f68d6a1)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/es6/regress/regress-crbug-248025.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-248025.js)  
```javascript
var filler = "



try {
  eval(filler + "\nfunction f() { for (x : y) { } }");
  throw "not reached";
} catch (e) {
  if (!(e instanceof SyntaxError)) throw e;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f68d6a1^!)  
[src/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/preparser.cc?cl=f68d6a1)  
[src/scanner.h](https://cs.chromium.org/chromium/src/v8/src/scanner.h?cl=f68d6a1)  
[test/mjsunit/regress/regress-crbug-248025.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-248025.js?cl=f68d6a1)  
  

### **crbug:245480**  
   
**[Issue: No Permission](https://crbug.com/245480)**  
**[Commit: Fix for bug 245480. Calling new Array(a) with a single argument could result in creating a holey array with a packed elements kind.](https://chromium.googlesource.com/v8/v8/+/75afb8c)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-245480.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-245480.js)  
```javascript
function isHoley(obj) {
  if (%HasHoleyElements(obj)) return true;
  return false;
}

function assertHoley(obj, name_opt) {
  assertEquals(true, isHoley(obj), name_opt);
}

function create_array(arg) {
  return new Array(arg);
}

obj = create_array(0);
assertHoley(obj);
create_array(0);
%OptimizeFunctionOnNextCall(create_array);
obj = create_array(10);
assertHoley(obj);


function f(length) {
  return new Array(length)
}

f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
var a = f(10);

function g(a) {
  return a[0];
}

var b = [0];
g(b);
g(b);
assertEquals(undefined, g(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/75afb8c^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=75afb8c)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=75afb8c)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=75afb8c)  
[test/mjsunit/regress/regress-crbug-245480.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-245480.js?cl=75afb8c)  
  

### **crbug:245424**  
   
**[Issue: Chrome: Crash Report - Stack Signature: v8::internal::FlexibleBodyVisitor<v8::internal::IncrementalMarkingMarking](https://crbug.com/245424)**  
**[Commit: Fast literals: fixed initialization of non-copied in-object property fields](https://chromium.googlesource.com/v8/v8/+/b4058a3)**  
  
Closed: May 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-29"]  
Regress: [mjsunit/regress/regress-crbug-245424.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-245424.js)  
```javascript
function boom() {
  var a = {
    foo: "bar",
    foo: "baz"
  };
  return a;
}

assertEquals("baz", boom().foo);
assertEquals("baz", boom().foo);
%OptimizeFunctionOnNextCall(boom);
assertEquals("baz", boom().foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4058a3^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=b4058a3)  
[test/mjsunit/regress/regress-crbug-245424.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-245424.js?cl=b4058a3)  
  

### **crbug:244461**  
   
**[Issue: Chrome: Crash Report - Stack Signature: v8::internal::HOptimizedGraphBuilder::TryInlineBuiltinFunctionCall...](https://crbug.com/244461)**  
**[Commit: Special Array constructor type feedback erroneously recorded when Array](https://chromium.googlesource.com/v8/v8/+/3d3c6b1)**  
  
Closed: May 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-29", "ReleaseBlock-Dev"]  
Regress: [mjsunit/regress/regress-crbug-244461.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-244461.js)  
```javascript
function foo(arg) {
  var a = arg();
  return a;
}


foo(Array);
foo(Array);
%OptimizeFunctionOnNextCall(foo);

foo(Array);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d3c6b1^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=3d3c6b1)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=3d3c6b1)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=3d3c6b1)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=3d3c6b1)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=3d3c6b1)  
[test/mjsunit/allocation-site-info.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/allocation-site-info.js?cl=3d3c6b1)  
[test/mjsunit/regress/regress-crbug-244461.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-244461.js?cl=3d3c6b1)  
  

### **crbug:243868**  
   
**[Issue: Chrome: Crash Report - Stack Signature: v8::internal::HOptimizedGraphBuilder::VisitLogicalExpression...](https://crbug.com/243868)**  
**[Commit: Fix IfBuilder::Deopt to clear the current block.](https://chromium.googlesource.com/v8/v8/+/3b114cd)**  
  
Closed: May 2013  
Type: Bug-Regression  
Components: Blink>JavaScript  
Labels: ["ReleaseBlock-Beta", "M-29"]  
Regress: [mjsunit/regress/regress-crbug-243868.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-243868.js)  
```javascript
var non_const_true = true;

function f(o) {
  return (non_const_true && (o.val == null || false));
}


var realm = Realm.create();
var realmObject = Realm.eval(realm, "function g() {}; var o = { val:g }; o;")


assertFalse(f(realmObject));
assertFalse(f(realmObject));


%OptimizeFunctionOnNextCall(f);
assertFalse(f(realmObject));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3b114cd^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3b114cd)  
[test/mjsunit/regress/regress-crbug-243868.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-243868.js?cl=3b114cd)  
  

### **crbug:242924**  
   
**[Issue: [LangFuzz] Crash at v8::internal::HeapObject::Size() on 64 bit with invalid read](https://crbug.com/242924)**  
**[Commit: Fix bogus deopt in BuildEmitDeepCopy for holey arrays.](https://chromium.googlesource.com/v8/v8/+/b704cb9)**  
  
Closed: May 2013  
Type: Bug-Security  
Components: None  
Labels: ["Reward-1000", "Security-Code28", "Via-Wizard", "Merge-Merged", "M-28", "Security_Severity-High", "Security_Impact-Beta", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-242924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242924.js)  
```javascript
function f() {
  return [,{}];
}

assertEquals([,{}], f());
assertEquals([,{}], f());
%OptimizeFunctionOnNextCall(f);
assertEquals([,{}], f());
gc();

function g() {
  return [[,1.5],{}];
}

assertEquals([[,1.5],{}], g());
assertEquals([[,1.5],{}], g());
%OptimizeFunctionOnNextCall(g);
assertEquals([[,1.5],{}], g());
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b704cb9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=b704cb9)  
[test/mjsunit/regress/regress-crbug-242924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242924.js?cl=b704cb9)  
  

### **crbug:242870**  
   
**[Issue: UNKNOWN in v8::internal::HOptimizedGraphBuilder::VisitLogicalExpression](https://crbug.com/242870)**  
**[Commit: Fix VisitLogicalExpression for empty blocks on RHS.](https://chromium.googlesource.com/v8/v8/+/bf413b5)**  
  
Closed: May 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-242870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242870.js)  
```javascript
var non_const_true = true;

function f() {
  return (non_const_true || true && g());
}

function g() {
  for (;;) {}
}

assertTrue(f());
assertTrue(f());
%OptimizeFunctionOnNextCall(f);
assertTrue(f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bf413b5^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=bf413b5)  
[test/mjsunit/regress/regress-crbug-242870.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242870.js?cl=bf413b5)  
  

### **crbug:242502**  
   
**[Issue: UNKNOWN in v8::internal::TypeFeedbackOracle::CanRetainOtherContext](https://crbug.com/242502)**  
**[Commit: Add regression test for fix from r14732.](https://chromium.googlesource.com/v8/v8/+/db4a770)**  
  
Closed: May 2013  
Type: Bug-Security  
Components: None  
Labels: ["M-27", "Stability-Memory-AddressSanitizer", "Security-Code28", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "Release-1", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-242502.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-242502.js)  
```javascript
function f() {
  return 23;
}

function call(o) {
  return o['']();
}

function test() {
  var o1 = %ToFastProperties(Object.create({ foo:1 }, { '': { value:f }}));
  var o2 = %ToFastProperties(Object.create({ bar:1 }, { '': { value:f }}));
  var o3 = %ToFastProperties(Object.create({ baz:1 }, { '': { value:f }}));
  var o4 = %ToFastProperties(Object.create({ qux:1 }, { '': { value:f }}));
  var o5 = %ToFastProperties(Object.create({ loo:1 }, { '': { value:f }}));
  
  assertEquals(23, call(o1));
  assertEquals(23, call(o1));
  
  assertEquals(23, call(o2));
  assertEquals(23, call(o3));
  assertEquals(23, call(o4));
  assertEquals(23, call(o5));
  return o1;
}


test();


gc();


var oboom = test();


%OptimizeFunctionOnNextCall(call);
assertEquals(23, call(oboom));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db4a770^!)  
[test/mjsunit/regress/regress-crbug-242502.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-242502.js?cl=db4a770)  
  

### **crbug:240032**  
   
**[Issue: Security: chrome_70ee0000!v8::internal::ScavengingVisitor<1,1>::EvacuateShortcutCandidate crash](https://crbug.com/240032)**  
**[Commit: Fix embedded new-space pointer in LCmpObjectEqAndBranch.](https://chromium.googlesource.com/v8/v8/+/8fb2086)**  
  
Closed: May 2013  
Type: Bug-Security  
Components: Blink>JavaScript  
Labels: ["reward-500", "Merge-Merged", "Security_Severity-Medium", "M-28", "Security_Impact-Beta", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-240032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-240032.js)  
```javascript
function mk() {
  return function() {};
}
assertInstanceof(mk(), Function);
assertInstanceof(mk(), Function);


var o = {};
o.func = mk();


function cmp(o, f) {
  return f === o.func;
}
assertTrue(cmp(o, o.func));
assertTrue(cmp(o, o.func));
%OptimizeFunctionOnNextCall(cmp);
assertTrue(cmp(o, o.func));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8fb2086^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8fb2086)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=8fb2086)  
[src/ia32/macro-assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.h?cl=8fb2086)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=8fb2086)  
[src/x64/macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.cc?cl=8fb2086)  
[src/x64/macro-assembler-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/macro-assembler-x64.h?cl=8fb2086)  
[test/mjsunit/regress/regress-crbug-240032.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-240032.js?cl=8fb2086)  
  

### **crbug:233737**  
   
**[Issue: JavaScript performs a calculation incorrectly](https://crbug.com/233737)**  
**[Commit: Fix missing hole check for loads from Smi arrays when all uses are changes](https://chromium.googlesource.com/v8/v8/+/7636fde)**  
  
Closed: May 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["M-29", "MovedFrom-28"]  
Regress: [mjsunit/regress/regress-crbug-233737.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-233737.js)  
```javascript
var a = new Array(2);
a[0] = 1;
assertTrue(%HasSmiElements(a));
assertTrue(%HasHoleyElements(a));

function hole(i) {
  return a[i] << 0;
}

assertEquals(1, hole(0));
assertEquals(1, hole(0));
%OptimizeFunctionOnNextCall(hole);
assertEquals(0, hole(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7636fde^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=7636fde)  
[test/mjsunit/regress/regress-crbug-233737.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-233737.js?cl=7636fde)  
  

### **crbug:229923**  
   
**[Issue: Crash while loading pages due to V8 roll](https://crbug.com/229923)**  
**[Commit: Fix JSON.stringify's slow path wrt sliced strings.](https://chromium.googlesource.com/v8/v8/+/da5c11a)**  
  
Closed: Apr 2013  
Type: Bug  
Components: Blink>JavaScript  
Labels: ["Stability-Crash", "Performance", "ReleaseBlock-Dev", "M-28", "TE-Verified-28.0.1474.0"]  
Regress: [mjsunit/regress/regress-crbug-229923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-229923.js)  
```javascript
var slice = "slow path of JSON.stringify for sliced string".substring(1);
assertEquals('"' + slice + '"', JSON.stringify(slice, null, 0));

var parent = "external string turned into two byte";
var slice_of_external = parent.substring(1);
try {
  
  
  externalizeString(parent, true);
} catch (e) { }
assertEquals('"' + slice_of_external + '"',
             JSON.stringify(slice_of_external, null, 0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da5c11a^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=da5c11a)  
[test/mjsunit/regress/regress-crbug-229923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-229923.js?cl=da5c11a)  
  

### **crbug:217858**  
   
**[Issue: [LangFuzz] Crash on Heap with invalid read (possibly due to uninitialized value) on 64 bit](https://crbug.com/217858)**  
**[Commit: Add test case for missing deopt sequence after forced deopt.](https://chromium.googlesource.com/v8/v8/+/a942fcd)**  
  
Closed: Mar 2013  
Type: Bug-Security  
Components: Blink  
Labels: ["M-27", "Reward-1000", "Via-Wizard", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-217858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-217858.js)  
```javascript
var r = /r/;
function f() {
  r[r] = function() {};
}

for (var i = 0; i < 300; i++) {
  f();
  if (i == 150) %OptimizeOsr();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a942fcd^!)  
[test/mjsunit/regress/regress-crbug-217858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-217858.js?cl=a942fcd)  
  

### **crbug:196583**  
   
**[Issue: No Permission](https://crbug.com/196583)**  
**[Commit: Fix detection of |handle_smi| case in HOptimizedGraphBuilder::HandlePolymorphicCallNamed](https://chromium.googlesource.com/v8/v8/+/e2cd7aa)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-196583.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-196583.js)  
```javascript
var a = 1;
a.__proto__.f = 1;
a.__proto__.f = function() { return 1; }


function B() {}
B.prototype = {f: function() { return 2; }};
var b = new B();
function C() {}
C.prototype = {g: "foo", f: function() { return 3; }};
var c = new C();

function crash(obj) {
  return obj.f();
}

for (var i = 0; i < 2; i++) {
  crash(a);
  crash(b);
  crash(c);
}
%OptimizeFunctionOnNextCall(crash);
assertEquals(1, crash(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e2cd7aa^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e2cd7aa)  
[test/mjsunit/regress/regress-crbug-196583.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-196583.js?cl=e2cd7aa)  
  

### **crbug:181422**  
   
**[Issue: Special character \s in RegExp does not match \u00a0 (NBSP)](https://crbug.com/181422)**  
**[Commit: Fix white space matching in latin-1 strings wrt \u00a0.](https://chromium.googlesource.com/v8/v8/+/b85237a)**  
  
Closed: Mar 2013  
Type: Bug  
Components: Blink  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-181422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-181422.js)  
```javascript
assertArrayEquals(["\u00a0"], "ab\u00a0cd".match(/\s/));
assertArrayEquals(["a", "b", "c", "d"], "ab\u00a0cd".match(/\S/g));

assertArrayEquals(["\u00a0"], "\u2604b\u00a0cd".match(/\s/));
assertArrayEquals(["\u2604", "b", "c", "d"], "\u2604b\u00a0cd".match(/\S/g));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b85237a^!)  
[src/arm/regexp-macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/regexp-macro-assembler-arm.cc?cl=b85237a)  
[src/ia32/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/regexp-macro-assembler-ia32.cc?cl=b85237a)  
[src/x64/regexp-macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/regexp-macro-assembler-x64.cc?cl=b85237a)  
[test/mjsunit/regress/regress-crbug-181422.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-181422.js?cl=b85237a)  
  

### **crbug:178790**  
   
**[Issue: Regex causing browser to become unresponsive](https://crbug.com/178790)**  
**[Commit: Limit EatAtLeast recursion by a budget.](https://chromium.googlesource.com/v8/v8/+/358311e)**  
  
Closed: Mar 2013  
Type: Bug  
Components: Blink  
Labels: ["Performance"]  
Regress: [mjsunit/regress/regress-crbug-178790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-178790.js)  
```javascript
var r1 = "";
for (var i = 0; i < 1000; i++) {
  r1 += "a?";
}
"test".match(RegExp(r1));

var r2 = "";
for (var i = 0; i < 100; i++) {
  r2 += "(a?|b?|c?|d?|e?|f?|g?)";
}
"test".match(RegExp(r2));





var r3 = "a";
for (var i = 0; i < 1000; i++) {
  r3 = "(" + r3 + ")a";
}
"test".match(RegExp(r3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/358311e^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=358311e)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=358311e)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=358311e)  
[test/mjsunit/regress/regress-crbug-178790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-178790.js?cl=358311e)  
  

### **crbug:173974**  
   
**[Issue: Type-feedback oracle in V8 gets confused aoubt monomorphic IC](https://crbug.com/173974)**  
**[Commit: Tag stubs that rely on instance types as MEGAMORPHIC.](https://chromium.googlesource.com/v8/v8/+/aca87c2)**  
  
Closed: Feb 2013  
Type: Bug  
Components: Blink  
Labels: ["Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-173974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173974.js)  
```javascript
function f() {
  var count = "";
  count[0] --;
}
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aca87c2^!)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=aca87c2)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=aca87c2)  
[test/mjsunit/regress/regress-crbug-173974.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-173974.js?cl=aca87c2)  
  

### **crbug:173907**  
   
**[Issue: CryptoJS show different behavior when debugged](https://crbug.com/173907)**  
**[Commit: Add regression test for r13617](https://chromium.googlesource.com/v8/v8/+/e83ff19)**  
  
Closed: Feb 2013  
Type: Bug-Regression  
Components: Blink  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-173907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173907.js), [mjsunit/regress/regress-crbug-173907b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-173907b.js)  
```javascript
var X = 1.1;
var K = 0.5;

var O = 0;
var result = new Float64Array(2);

function spill() {
  try { } catch (e) { }
}

function buggy() {
  var v = X;
  var phi1 = v + K;
  var phi2 = v - K;

  spill();  

  var xmm1 = v;
  var xmm2 = v*v*v;
  var xmm3 = v*v*v*v;
  var xmm4 = v*v*v*v*v;
  var xmm5 = v*v*v*v*v*v;
  var xmm6 = v*v*v*v*v*v*v;
  var xmm7 = v*v*v*v*v*v*v*v;
  var xmm8 = v*v*v*v*v*v*v*v*v;

  
  
  

  for (var x = 0; x < 2; x++) {
    xmm1 += xmm1 * xmm6;
    xmm2 += xmm1 * xmm5;
    xmm3 += xmm1 * xmm4;
    xmm4 += xmm1 * xmm3;
    xmm5 += xmm1 * xmm2;

    
    var t = phi1;
    phi1 = phi2;
    phi2 = t;
  }

  
  
  
  result[0] = (O === 0) ? phi1 : phi2;
  result[1] = (O !== 0) ? phi1 : phi2;
}

function test() {
  buggy();
  assertArrayEquals([X + K, X - K], result);
}

test();
test();
%OptimizeFunctionOnNextCall(buggy);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e83ff19^!)  
[test/mjsunit/regress/regress-crbug-173907.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-173907.js?cl=e83ff19)  
  

### **crbug:172345**  
   
**[Issue: No Permission](https://crbug.com/172345)**  
**[Commit: Do not try to collect the map if the monomorphic IC stub has no map.](https://chromium.googlesource.com/v8/v8/+/c8636a2)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-172345.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-172345.js)  
```javascript
function f(a,i) {
  return a[i];
}

f([1,2,3], "length");
f([1,2,3], "length");
f([1,2,3], 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c8636a2^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=c8636a2)  
[test/mjsunit/regress/regress-crbug-172345.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-172345.js?cl=c8636a2)  
  

### **crbug:170856**  
   
**[Issue: Chrome Canary snaps when rendering Google AppScript pages](https://crbug.com/170856)**  
**[Commit: Correctly reset lastIndex in an RegExp object.](https://chromium.googlesource.com/v8/v8/+/9296975)**  
  
Closed: Feb 2013  
Type: Bug-Regression  
Components: Blink  
Labels: ["M-25", "Via-Wizard", "Merge-Merged", "Hotlist-GoogleApps", "ReleaseBlock-Stable"]  
Regress: [mjsunit/regress/regress-crbug-170856.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-170856.js)  
```javascript
r = new RegExp("a");
for (var i = 0; i < 100; i++) {
  r["abc" + i] = i;
}
"zzzz".replace(r, "");
assertEquals(0, r.lastIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9296975^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=9296975)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=9296975)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9296975)  
[test/mjsunit/regress/regress-crbug-170856.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-170856.js?cl=9296975)  
  

### **crbug:168545**  
   
**[Issue: No Permission](https://crbug.com/168545)**  
**[Commit: Fix missing exception check in typed array constructor.](https://chromium.googlesource.com/v8/v8/+/0e46919)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-168545.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-168545.js)  
```javascript
var o = {};
Object.defineProperty(o, "length", { get: function() { throw "bail"; }});
assertThrows("new Int16Array(o);");

var a = [];
Object.defineProperty(a, "0", { get: function() { throw "bail"; }});
assertThrows("new Int16Array(a);");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0e46919^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=0e46919)  
[test/mjsunit/regress/regress-crbug-168545.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-168545.js?cl=0e46919)  
  

### **crbug:163530**  
   
**[Issue: arguments occasionally contains arguments of callee.](https://crbug.com/163530)**  
**[Commit: Fix materialization of arguments objects with unknown values.](https://chromium.googlesource.com/v8/v8/+/ea5e9ed)**  
  
Closed: Feb 2013  
Type: Bug  
Components: Blink  
Labels: ["M-26", "Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-163530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-163530.js)  
```javascript
(function() {
  var deoptimize = { deopt:true };
  var object = {};

  object.a = function A(x, y, z) {
    assertSame(0, arguments.length);
    return this.b();
  };

  object.b = function B() {
    assertSame(0, arguments.length);
    deoptimize.deopt;
    return arguments.length;
  };

  assertSame(0, object.a());
  assertSame(0, object.a());
  %OptimizeFunctionOnNextCall(object.a);
  assertSame(0, object.a());
  delete deoptimize.deopt;
  assertSame(0, object.a());
})();




(function() {
  'use strict';
  var deoptimize = { deopt:true };
  var object = {};

  object.a = function A(x, y, z) {
    assertSame(0, arguments.length);
    return this.b(1, 2, 3, 4, 5, 6, 7, 8);
  };

  object.b = function B(a, b, c, d, e, f, g, h) {
    assertSame(8, arguments.length);
    deoptimize.deopt;
    return arguments.length;
  };

  assertSame(8, object.a());
  assertSame(8, object.a());
  %OptimizeFunctionOnNextCall(object.a);
  assertSame(8, object.a());
  delete deoptimize.deopt;
  assertSame(8, object.a());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea5e9ed^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=ea5e9ed)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=ea5e9ed)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=ea5e9ed)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=ea5e9ed)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=ea5e9ed)  
[src/ia32/lithium-codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.h?cl=ea5e9ed)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ea5e9ed)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=ea5e9ed)  
[src/x64/lithium-codegen-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.h?cl=ea5e9ed)  
[test/mjsunit/regress/regress-crbug-163530.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-163530.js?cl=ea5e9ed)  
  

### **crbug:162085**  
   
**[Issue: Google Calendar leaks tons of memory](https://crbug.com/162085)**  
**[Commit: Ensure double arrays are filled with holes when extended from variations of empty arrays.](https://chromium.googlesource.com/v8/v8/+/ebeaad6)**  
  
Closed: Nov 2012  
Type: Bug-Regression  
Components: Blink  
Labels: ["M-25", "ReleaseBlock-Dev"]  
Regress: [mjsunit/regress/regress-crbug-162085.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-162085.js)  
```javascript
var a = [1,2,3];
a.length = 0;
a[0] = 1.4;
assertEquals(1.4, a[0]);
assertEquals(undefined, a[1]);
assertEquals(undefined, a[2]);
assertEquals(undefined, a[3]);


function grow_store(a,i,v) {
  a[i] = v;
}

var a2 = [1.3];
grow_store(a2,1,1.4);
a2.length = 0;
grow_store(a2,0,1.5);
assertEquals(1.5, a2[0]);
assertEquals(undefined, a2[1]);
assertEquals(undefined, a2[2]);
assertEquals(undefined, a2[3]);


var a3 = [1.3];
var o = {};
grow_store(a3, 1, o);
assertEquals(1.3, a3[0]);
assertEquals(o, a3[1]);


function grow_store2(a,i,v) {
  a[i] = v;
}

var a4 = [1.3];
grow_store2(a4,1,1.4);
a4.length = 0;
grow_store2(a4,0,1);
assertEquals(1, a4[0]);
assertEquals(undefined, a4[1]);
assertEquals(undefined, a4[2]);
assertEquals(undefined, a4[3]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ebeaad6^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=ebeaad6)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=ebeaad6)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=ebeaad6)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=ebeaad6)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=ebeaad6)  
[test/mjsunit/array-natives-elements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-natives-elements.js?cl=ebeaad6)  
[test/mjsunit/regress/regress-crbug-162085.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-162085.js?cl=ebeaad6)  
  

### **crbug:160010**  
   
**[Issue: [LangFuzz] Crash at v8::internal::BasicJsonStringifier::SerializeString](https://crbug.com/160010)**  
**[Commit: Fix length check in JSON.stringify.](https://chromium.googlesource.com/v8/v8/+/ef1b3d3)**  
  
Closed: Nov 2012  
Type: Bug-Security  
Components: None  
Labels: ["M-25", "Reward-1000", "Via-Wizard", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-160010.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-160010.js)  
```javascript
var str = "a";
for (var i = 0; i < 28; i++) {
  str += str;
  %FlattenString(str);  
}
JSON.stringify(str);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef1b3d3^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=ef1b3d3)  
[test/mjsunit/regress/regress-crbug-160010.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-160010.js?cl=ef1b3d3)  
  

### **crbug:158185**  
   
**[Issue: JSON.parse](https://crbug.com/158185)**  
**[Commit: Treat leading zeros in JSON.parse correctly.](https://chromium.googlesource.com/v8/v8/+/8ed2e56)**  
  
Closed: Oct 2012  
Type: Bug  
Components: Blink  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-158185.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-158185.js)  
```javascript
assertEquals("0023456",
             Object.keys(JSON.parse('{"0023456": 1}'))[0]);
assertEquals("1234567890123",
             Object.keys(JSON.parse('{"1234567890123": 1}'))[0]);
assertEquals("123456789ABCD",
             Object.keys(JSON.parse('{"123456789ABCD": 1}'))[0]);
assertEquals("12A",
             Object.keys(JSON.parse('{"12A": 1}'))[0]);

assertEquals(1, JSON.parse('{"0":1}')[0]);
assertEquals(undefined, JSON.parse('{"00":1}')[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ed2e56^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=8ed2e56)  
[test/mjsunit/regress/regress-crbug-158185.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-158185.js?cl=8ed2e56)  
  

### **crbug:157520**  
   
**[Issue: Arguments collection not updated intermittently](https://crbug.com/157520)**  
**[Commit: Fix ugly typo in GenerateNewNonStrictFast.](https://chromium.googlesource.com/v8/v8/+/e363cd3)**  
  
Closed: Oct 2012  
Type: Bug  
Components: Blink  
Labels: ["Via-Wizard"]  
Regress: [mjsunit/regress/regress-crbug-157520.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-157520.js)  
```javascript
(function(){
  var f = function(arg) {
    arg = 2;
    return arguments[0];
  };
  for (var i = 0; i < 50000; i++) {
    assertSame(2, f(1));
  }
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e363cd3^!)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=e363cd3)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=e363cd3)  
[test/mjsunit/regress/regress-crbug-157520.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-157520.js?cl=e363cd3)  
  

### **crbug:157019**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/157019)**  
**[Commit: Fix slack tracking when instance prototype changes.](https://chromium.googlesource.com/v8/v8/+/a31889e)**  
  
Closed: Dec 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-24"]  
Regress: [mjsunit/regress/regress-crbug-157019.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-157019.js)  
```javascript
function makeConstructor() {
  return function() {
    this.a = 1;
    this.b = 2;
  };
}

var c1 = makeConstructor();
var o1 = new c1();

c1.prototype = {};

for (var i = 0; i < 10; i++) {
  var o = new c1();
  for (var j = 0; j < 8; j++) {
    o["x" + j] = 0;
  }
}

var c2 = makeConstructor();
var o2 = new c2();

for (var i = 0; i < 50000; i++) {
  new c2();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a31889e^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=a31889e)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=a31889e)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=a31889e)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=a31889e)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a31889e)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=a31889e)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=a31889e)  
[test/mjsunit/regress/regress-crbug-157019.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-157019.js?cl=a31889e)  
  

### **crbug:150729**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/150729)**  
**[Commit: Fix LBoundsCheck on x64 to handle (stack slot + constant) correctly](https://chromium.googlesource.com/v8/v8/+/a8e502f)**  
  
Closed: Dec 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["reward-1500", "Merge-Merged", "M-23", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-150729.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-150729.js)  
```javascript
var t = 0;
function burn() {
  i = [t, 1];
  var M = [i[0], Math.cos(t) + i[7074959]];
  t += .05;
}
for (var j = 0; j < 5; j++) {
  if (j == 2) %OptimizeFunctionOnNextCall(burn);
  burn();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8e502f^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=a8e502f)  
[test/mjsunit/regress/regress-crbug-150729.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-150729.js?cl=a8e502f)  
  

### **crbug:150545**  
   
**[Issue: UNKNOWN in v8::internal::RootMarkingVisitor::MarkObjectByPointer](https://crbug.com/150545)**  
**[Commit: Fix lost arguments dropping in HLeaveInlined.](https://chromium.googlesource.com/v8/v8/+/f0dcaf9)**  
  
Closed: Dec 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["Release-0", "Stability-Memory-AddressSanitizer", "CVE-2013-0836", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-24", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-150545.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-150545.js)  
```javascript
(function() {
  "use strict";

  var instantReturn = false;
  function inner() {
    if (instantReturn) return;
    assertSame(3, arguments.length);
    assertSame(1, arguments[0]);
    assertSame(2, arguments[1]);
    assertSame(3, arguments[2]);
  }

  function outer() {
    inner(1,2,3);
    for (var i = 0; i < 3; i++) %OptimizeOsr();
  }

  outer();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0dcaf9^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=f0dcaf9)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=f0dcaf9)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=f0dcaf9)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=f0dcaf9)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=f0dcaf9)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=f0dcaf9)  
[test/mjsunit/regress/regress-crbug-150545.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-150545.js?cl=f0dcaf9)  
  

### **crbug:148376**  
   
**[Issue: [LangFuzz] Crash at v8::internal::MarkCompactCollector::EvacuateNewSpace with invalid read](https://crbug.com/148376)**  
**[Commit: Ensure correct enumeration indices in the dict](https://chromium.googlesource.com/v8/v8/+/1d1adaf)**  
  
Closed: Sep 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["Reward-1000", "M-23", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-148376.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-148376.js)  
```javascript
function defineSetter(o) {
  o.__defineSetter__('property', function() {});
}

defineSetter(Object.prototype);
property = 0;
defineSetter(this);
var keys = Object.keys(this);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d1adaf^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1d1adaf)  
[test/mjsunit/regress/regress-crbug-148376.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-148376.js?cl=1d1adaf)  
  

### **crbug:147475**  
   
**[Issue: UNKNOWN in v8::internal::Deoptimizer::DoComputeOutputFrames](https://crbug.com/147475)**  
**[Commit: Fix deoptimizer for shared optimized code.](https://chromium.googlesource.com/v8/v8/+/f6cd240)**  
  
Closed: Dec 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-Stable", "M-22", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-147475.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-147475.js)  
```javascript
function worker1(ignored) {
  return 100;
}

function factory(worker) {
  return function(call_depth) {
    if (call_depth == 0) return 10;
    return 1 + worker(call_depth - 1);
  }
}

var f1 = factory(worker1);
var f2 = factory(f1);
assertEquals(11, f2(1));
%OptimizeFunctionOnNextCall(f1);
assertEquals(10, f1(0));
%OptimizeFunctionOnNextCall(f2);
assertEquals(102, f2(2));
assertEquals(102, f2(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6cd240^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=f6cd240)  
[test/mjsunit/regress/regress-crbug-147475.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-147475.js?cl=f6cd240)  
  

### **crbug:146910**  
   
**[Issue: setting an array's .length can delete non-configurable properties](https://crbug.com/146910)**  
**[Commit: Fix setting array length to zero for slow elements.](https://chromium.googlesource.com/v8/v8/+/c012afb)**  
  
Closed: Sep 2012  
Type: Bug  
Components: Blink  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-146910.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-146910.js)  
```javascript
var x = [];
assertSame(0, x.length);
assertSame(undefined, x[0]);

Object.defineProperty(x, '0', { value: 7, configurable: false });
assertSame(1, x.length);
assertSame(7, x[0]);

x.length = 0;
assertSame(1, x.length);
assertSame(7, x[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c012afb^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=c012afb)  
[test/mjsunit/regress/regress-crbug-146910.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-146910.js?cl=c012afb)  
  

### **crbug:145961**  
   
**[Issue: Trix / google spreadsheets have selection issues on ChromeOS](https://crbug.com/145961)**  
**[Commit: Support register as right operand in min/max support.](https://chromium.googlesource.com/v8/v8/+/a8638c1)**  
  
Closed: Sep 2012  
Type: Bug  
Components: Blink  
Labels: ["Hotlist-GoogleApps"]  
Regress: [mjsunit/regress/regress-crbug-145961.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-145961.js)  
```javascript
function test() {
  var a = new Int32Array(2);
  var x = a[0];
  return Math.min(x, x);
}

assertEquals(0, test());
assertEquals(0, test());
%OptimizeFunctionOnNextCall(test);
assertEquals(0, test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8638c1^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=a8638c1)  
[test/mjsunit/regress/regress-crbug-145961.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-145961.js?cl=a8638c1)  
  

### **crbug:142218**  
   
**[Issue: LLVM cPython no longer works](https://crbug.com/142218)**  
**[Commit: Check that index and length are Smi in bounds check.](https://chromium.googlesource.com/v8/v8/+/5df5eea)**  
  
Closed: Aug 2012  
Type: Bug  
Components: Blink  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-142218.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-142218.js)  
```javascript
length = 1 << 16;
a = new Array(length);

function insert_element(key) {
  a[key] = 42;
}

insert_element(1);
%OptimizeFunctionOnNextCall(insert_element);
insert_element(new Object());
count = 0;
for (var i = 0; i < length; i++) {
  if (a[i] != undefined) count++;
}
assertEquals(1, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5df5eea^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=5df5eea)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=5df5eea)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=5df5eea)  
[src/ia32/lithium-codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.h?cl=5df5eea)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=5df5eea)  
[src/x64/lithium-codegen-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.h?cl=5df5eea)  
[test/mjsunit/regress/regress-crbug-142218.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-142218.js?cl=5df5eea)  
  

### **crbug:142087**  
   
**[Issue: UNKNOWN in void v8::internal::String::WriteToFlat<char>](https://crbug.com/142087)**  
**[Commit: Fix wrong indexing in global regexp.](https://chromium.googlesource.com/v8/v8/+/960b1af)**  
  
Closed: Aug 2012  
Type: Bug-Security  
Components: Blink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Memory-AddressSanitizer", "M-22", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Regress: [mjsunit/regress/regress-crbug-142087.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-142087.js)  
```javascript
var string = "What are you looking for?";

var expected_match = [""];
for (var i = 0; i < string.length; i++) {
  expected_match.push("");
}

string.replace(/(_)|(_|)/g, "");
assertArrayEquals(expected_match, string.match(/(_)|(_|)/g, ""));

'***************************************'.match(/((\\)|(\*)|(\$))/g, ".");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/960b1af^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=960b1af)  
[test/mjsunit/regress/regress-crbug-142087.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-142087.js?cl=960b1af)  
  

### **crbug:140083**  
   
**[Issue: [LangFuzz] Crash on heap trying to execute address 0x0000000200000000.](https://crbug.com/140083)**  
**[Commit: Fixed compound/count operations with getter-only accessor properties.](https://chromium.googlesource.com/v8/v8/+/83fc420)**  
  
Closed: Aug 2012  
Type: Bug-Security  
Components: None  
Labels: ["Reward-1000", "M-22", "Security_Severity-High", "Security_Impact-Beta", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-140083.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-140083.js)  
```javascript
Object.defineProperty(Object.prototype, "foo",
                      { get: function() { return 123; } });

function bar(o) {
  o.foo += 42;
  o.foo++;
}

var baz = {};
bar(baz);
bar(baz);
%OptimizeFunctionOnNextCall(bar)
bar(baz);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/83fc420^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=83fc420)  
[test/mjsunit/regress/regress-crbug-140083.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-140083.js?cl=83fc420)  
  

### **crbug:138887**  
   
**[Issue: AirAsia.com is broken: Chrome is throwing an "Uncaught RangeError: Maximum call stack size exceeded" since build 143896](https://crbug.com/138887)**  
**[Commit: Always set the callee's context when calling a function from optimized code.](https://chromium.googlesource.com/v8/v8/+/80c35c6)**  
  
Closed: Aug 2012  
Type: Compat  
Components: Blink  
Labels: ["Hotlist-Japan", "Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-138887.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-138887.js)  
```javascript
function worker1(ignored) {
  return 100;
}

function factory(worker) {
  return function(call_depth) {
    if (call_depth == 0) return 10;
    return 1 + worker(call_depth - 1);
  }
}

var f1 = factory(worker1);
var f2 = factory(f1);
assertEquals(11, f2(1));  
assertEquals(11, f2(1));
%OptimizeFunctionOnNextCall(f1);
assertEquals(10, f1(0));  
%OptimizeFunctionOnNextCall(f2);
assertEquals(102, f2(1000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/80c35c6^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=80c35c6)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=80c35c6)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=80c35c6)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=80c35c6)  
[test/mjsunit/regress/regress-crbug-138887.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-138887.js?cl=80c35c6)  
  

### **crbug:137689**  
   
**[Issue: REGRESSION: about:gpu broken](https://crbug.com/137689)**  
**[Commit: When following an accessor transition for an already existing accessor, don't load the last added descriptor but the same descriptor as we already found previously.](https://chromium.googlesource.com/v8/v8/+/90c7cb1)**  
  
Closed: Jul 2012  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-EditIssue", "WebKit-ID-91571-ASSIGNED", "Stability-Crash", "ReleaseBlock-Dev", "M-22", "WebKit-Rev-122930"]  
Regress: [mjsunit/regress/regress-crbug-137689.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-137689.js)  
```javascript
function getter() { return 10; }
function setter(v) { }
function getter2() { return 20; }

var o = {};
var o2 = {};

Object.defineProperty(o, "foo", { get: getter, configurable: true });
Object.defineProperty(o2, "foo", { get: getter, configurable: true });
assertTrue(%HaveSameMap(o, o2));

Object.defineProperty(o, "bar", { get: getter2 });
Object.defineProperty(o2, "bar", { get: getter2 });
assertTrue(%HaveSameMap(o, o2));

Object.defineProperty(o, "foo", { set: setter, configurable: true });
Object.defineProperty(o2, "foo", { set: setter, configurable: true });
assertTrue(%HaveSameMap(o, o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90c7cb1^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=90c7cb1)  
[src/property.h](https://cs.chromium.org/chromium/src/v8/src/property.h?cl=90c7cb1)  
[test/mjsunit/regress/regress-crbug-137689.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-137689.js?cl=90c7cb1)  
  

### **crbug:135066**  
   
**[Issue: No Permission](https://crbug.com/135066)**  
**[Commit: Fix lazy compilation for strict eval scopes.](https://chromium.googlesource.com/v8/v8/+/7da6d2b)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-135066.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-135066.js)  
```javascript
var filler = "


assertEquals(23, eval(
  "'use strict';" +
  "var x = 23;" +
  "var f = function bozo1() {" +
  "  return x;" +
  "};" +
  "assertSame(23, f());" +
  "f;" +
  filler
)());


assertEquals(42, (function() {
  "use strict";
  return eval(
    "var y = 42;" +
    "var g = function bozo2() {" +
    "  return y;" +
    "};" +
    "assertSame(42, g());" +
    "g;" +
    filler
  )();
})());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7da6d2b^!)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=7da6d2b)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=7da6d2b)  
[test/mjsunit/regress/regress-crbug-135066.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-135066.js?cl=7da6d2b)  
  

### **crbug:135008**  
   
**[Issue: Chrome: Crash Report -         Stack Signature: v8::internal::AstVisitor::VisitDeclarations...](https://crbug.com/135008)**  
**[Commit: Fix lazy parsing heuristics to respect outer scope.](https://chromium.googlesource.com/v8/v8/+/a691c69)**  
  
Closed: Sep 2012  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-135008.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-135008.js)  
```javascript
var filler = "

var scope = { x:23 };

with(scope) {
  eval(
    "scope.f = (function outer() {" +
    "  function inner() {" +
    "    return x;" +
    "  }" +
    "  return inner;" +
    "})();" +
    filler
  );
};

assertSame(23, scope.f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a691c69^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=a691c69)  
[test/mjsunit/regress/regress-crbug-135008.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-135008.js?cl=a691c69)  
  

### **crbug:134609**  
   
**[Issue: Chrome: Crash Report -         Stack Signature: v8::internal::JSReceiver::Lookup(v8::intern...](https://crbug.com/134609)**  
**[Commit: Fixed deoptimization of inlined getters.](https://chromium.googlesource.com/v8/v8/+/7af6883)**  
  
Closed: Sep 2012  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "ReleaseBlock-Beta", "M-22"]  
Regress: [mjsunit/regress/regress-crbug-134609.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-134609.js)  
```javascript
var forceDeopt = {x:0};

var objectWithGetterProperty = (function (value) {
  var obj = {};
  Object.defineProperty(obj, "getterProperty", {
    get: function foo() {
      forceDeopt.x;
      return value;
    },
  });
  return obj;
})("bad");

function test() {
  var iAmContextAllocated = "good";
  objectWithGetterProperty.getterProperty;
  return iAmContextAllocated;

  
  function unused() { iAmContextAllocated; }
}

assertEquals("good", test());
assertEquals("good", test());
%OptimizeFunctionOnNextCall(test);
assertEquals("good", test());


delete forceDeopt.x;
assertEquals("good", test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7af6883^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=7af6883)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=7af6883)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=7af6883)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=7af6883)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=7af6883)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=7af6883)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=7af6883)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=7af6883)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=7af6883)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=7af6883)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=7af6883)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=7af6883)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=7af6883)  
[src/ia32/deoptimizer-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/deoptimizer-ia32.cc?cl=7af6883)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=7af6883)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=7af6883)  
[src/mips/deoptimizer-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/deoptimizer-mips.cc?cl=7af6883)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=7af6883)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=7af6883)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7af6883)  
[src/stub-cache.h](https://cs.chromium.org/chromium/src/v8/src/stub-cache.h?cl=7af6883)  
[src/x64/deoptimizer-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/deoptimizer-x64.cc?cl=7af6883)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=7af6883)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=7af6883)  
[test/mjsunit/regress/regress-crbug-134609.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-134609.js?cl=7af6883)  
  

### **crbug:134055**  
   
**[Issue: No Permission](https://crbug.com/134055)**  
**[Commit: Make near-jump check more strict in LoadNamedFieldPolymorphic on ia32/x64](https://chromium.googlesource.com/v8/v8/+/9ce4133)**  
  
Closed: None  
Type: None  
Components: None  
Labels: "No Permission"  
Regress: [mjsunit/regress/regress-crbug-134055.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-134055.js)  
```javascript
function crash(obj) {
  return obj.foo;
}

function base(number_of_properties) {
  var result = new Array();
  for (var i = 0; i < number_of_properties; i++) {
    result["property" + i] = "value" + i;
  }
  result.foo = number_of_properties;
  return result;
}

var a = base(12);
var b = base(13);
var c = base(14);
var d = base(15);

crash(a);  
crash(a);
crash(b);
crash(c);
crash(d);  


var x = base(13);
x[0] = "object";
x = base(14);
x[0] = "object";
x = base(15);
x[0] = "object";

%OptimizeFunctionOnNextCall(crash);
crash(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9ce4133^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=9ce4133)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=9ce4133)  
[test/mjsunit/regress/regress-crbug-134055.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-134055.js?cl=9ce4133)  
  

### **crbug:126414**  
   
**[Issue: [LangFuzz] Crash on heap with invalid read from random address (32 bit)](https://crbug.com/126414)**  
**[Commit: Fix unsigned-Smi check in MappedArgumentsLookup](https://chromium.googlesource.com/v8/v8/+/63263a9)**  
  
Closed: Dec 2012  
Type: Bug-Security  
Components: Internals  
Labels: ["CVE-2011-3111", "reward-500", "M-19", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "CVE_description-submitted"]  
Regress: [mjsunit/regress/regress-crbug-126414.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-126414.js)  
```javascript
function foo(bar)  {
  return arguments[bar];
}
foo(0);           
foo(-536870912);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63263a9^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=63263a9)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=63263a9)  
[src/mips/ic-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/ic-mips.cc?cl=63263a9)  
[test/mjsunit/regress/regress-crbug-126414.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-126414.js?cl=63263a9)  
  

### **crbug:125148**  
   
**[Issue: Javascript engine sometimes calls incorrect constructor](https://crbug.com/125148)**  
**[Commit: Perform HasFastProperties check on prototypes when computing call targets in Crankshaft.](https://chromium.googlesource.com/v8/v8/+/432576b)**  
  
Closed: Jul 2012  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-125148.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-125148.js)  
```javascript
function ToDictionaryMode(x) {
  %OptimizeObjectForAddingMultipleProperties(x, 100);
}

var A, B, C;


A = {};
Object.defineProperty(A, "foo", { value: function() { assertUnreachable(); }});

B = Object.create(A);
Object.defineProperty(B, "foo", { value: function() { return 111; }});

C = Object.create(B);

function bar(x) { return x.foo(); }

assertEquals(111, bar(C));
assertEquals(111, bar(C));
ToDictionaryMode(B);
%OptimizeFunctionOnNextCall(bar);
assertEquals(111, bar(C));


A = {};
Object.defineProperty(A, "baz", { get: function() { assertUnreachable(); }});

B = Object.create(A);
Object.defineProperty(B, "baz", { get: function() { return 111; }});

C = Object.create(B);

function boo(x) { return x.baz; }

assertEquals(111, boo(C));
assertEquals(111, boo(C));
ToDictionaryMode(B);
%OptimizeFunctionOnNextCall(boo);
assertEquals(111, boo(C));


A = {};
Object.defineProperty(A, "huh", { set: function(x) { assertUnreachable(); }});

B = Object.create(A);
var setterValue;
Object.defineProperty(B, "huh", { set: function(x) { setterValue = x; }});

C = Object.create(B);

function fuu(x) {
  setterValue = 222;
  x.huh = 111;
  return setterValue;
}

assertEquals(111, fuu(C));
assertEquals(111, fuu(C));
ToDictionaryMode(B);
%OptimizeFunctionOnNextCall(fuu);
assertEquals(111, fuu(C));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/432576b^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=432576b)  
[test/mjsunit/regress/regress-crbug-125148.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-125148.js?cl=432576b)  
  

### **crbug:122271**  
   
**[Issue: Page starts rendering incorrectly after V8 roll](https://crbug.com/122271)**  
**[Commit: Fix regular and ElementsKind transitions interfering with each other](https://chromium.googlesource.com/v8/v8/+/14e1817)**  
  
Closed: Apr 2012  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-19", "ReleaseBlock-Stable"]  
Regress: [mjsunit/regress/regress-crbug-122271.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-122271.js)  
```javascript
var a = [0, 0, 0, 1];
var b = [0, 0, 0, "one"];
var c = [0, 0, 0, 1];
c.foo = "baz";

function foo(array) {
  array.foo = "bar";
}

assertTrue(%HasSmiElements(a));
assertTrue(%HasObjectElements(b));

foo(a);
foo(b);

assertTrue(%HasSmiElements(a));
assertTrue(%HasObjectElements(b));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14e1817^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=14e1817)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=14e1817)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=14e1817)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=14e1817)  
[test/mjsunit/regress/regress-crbug-122271.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-122271.js?cl=14e1817)  
  

### **crbug:119926**  
   
**[Issue: Use after free in v8::internal::IncrementalMarking::Step](https://crbug.com/119926)**  
**[Commit: Fix missing write barrier in CopyObjectToObjectElements.](https://chromium.googlesource.com/v8/v8/+/4e405b6)**  
  
Closed: Mar 2012  
Type: Bug-Security  
Components: Internals  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-19", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-119926.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-119926.js)  
```javascript
var a = new Array(500);
for (var i = 0; i < 100000; i++) {
  a[i] = new Object();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e405b6^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=4e405b6)  
[src/elements.h](https://cs.chromium.org/chromium/src/v8/src/elements.h?cl=4e405b6)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4e405b6)  
[test/mjsunit/regress/regress-crbug-119926.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-119926.js?cl=4e405b6)  
  

### **crbug:109362**  
   
**[Issue: Line number mismatch in functions created with Function constructor](https://crbug.com/109362)**  
**[Commit: Correctly compute line numbers in functions from the function constructor.](https://chromium.googlesource.com/v8/v8/+/1dbd636)**  
  
Closed: None  
Type: Bug  
Components: Platform>DevTools  
Labels: ["Hotlist-Polish"]  
Regress: [mjsunit/regress/regress-crbug-109362.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-109362.js)  
```javascript
function test(expectation, f) {
  var stack;
  try {
    f();
  } catch (e) {
    stack = e.stack;
  }
  assertTrue(stack.indexOf("at eval (evaltest:" + expectation + ")") > 0);
}

/*
(function(
) {
1 + reference_error 
})
*/
test("3:5", new Function(
    '1 + reference_error 
/*
(function(x
) {

 1 + reference_error 
})
*/
test("4:6", new Function(
    'x', '\n 1 + reference_error 
/*
(function(x

,z
,y
) {

 1 + reference_error 
})
*/
test("7:6", new Function(
    'x\n\n', "z
/*
(function(x/\*,z
,y*\/
) {
1 + reference_error 
})
*/
test("4:5", new Function(
    'x/*', "z
/*
(function () {
 1 + reference_error 
})
*/
test("2:6", eval(
    '(function () {\n 1 + reference_error  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1dbd636^!)  
[src/cpu-profiler.cc](https://cs.chromium.org/chromium/src/v8/src/cpu-profiler.cc?cl=1dbd636)  
[src/generator.js](https://cs.chromium.org/chromium/src/v8/src/generator.js?cl=1dbd636)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=1dbd636)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1dbd636)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=1dbd636)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=1dbd636)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=1dbd636)  
[test/message/single-function-literal.js](https://cs.chromium.org/chromium/src/v8/test/message/single-function-literal.js?cl=1dbd636)  
[test/mjsunit/regress/regress-crbug-109362.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-109362.js?cl=1dbd636)  
  

### **crbug:100859**  
   
**[Issue: [LangFuzz] Crash on Heap with null deref](https://crbug.com/100859)**  
**[Commit: Fix crash in d8 when external array ctor hits stack overflow](https://chromium.googlesource.com/v8/v8/+/91efb31)**  
  
Closed: Feb 2015  
Type: Bug  
Components: None  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-100859.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-100859.js)  
```javascript
function setx() {
  setx(typeof new Uint16Array('x') === 'object');
}
var exception = false;
try {
  setx();
} catch (ex) {
  assertTrue(ex instanceof RangeError);
  exception = true;
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91efb31^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=91efb31)  
[test/mjsunit/regress/regress-crbug-100859.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-100859.js?cl=91efb31)  
  

### **crbug:90771**  
   
**[Issue: DOMUI: Sync errors surfacing on NTP, Preferences and Bookmarks Bar are completely out of sync](https://crbug.com/90771)**  
**[Commit: Fix Reflect.construct with constructors without a prototype slot](https://chromium.googlesource.com/v8/v8/+/7a3cb59)**  
  
Closed: Aug 2011  
Type: Bug  
Components: Services>Sync  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-14", "Merge-Merged-835", "ReleaseBlock-Stable"]  
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
  

### **crbug:87478**  
   
**[Issue: [LangFuzz] Crash on heap with invalid read](https://crbug.com/87478)**  
**[Commit: Fix receiver check in arguments ICs.](https://chromium.googlesource.com/v8/v8/+/89cc886)**  
  
Closed: Jun 2011  
Type: Bug-Security  
Components: None  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-14", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Regress: [mjsunit/regress/regress-crbug-87478.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-87478.js)  
```javascript
function f(array) { return array[0]; }
function args(a) { return arguments; }

for (var i = 0; i < 10; i++) {
  f(args(1));
}
f('123');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/89cc886^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=89cc886)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=89cc886)  
[src/x64/ic-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/ic-x64.cc?cl=89cc886)  
[test/mjsunit/regress/regress-crbug-87478.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-87478.js?cl=89cc886)  
  

### **crbug:72736**  
   
**[Issue: Object.defineProperty does not override existing properties](https://crbug.com/72736)**  
**[Commit: Use ForceSetObjectProperty in DefineOrRedefineDataProperty (fixes crbug 72736).](https://chromium.googlesource.com/v8/v8/+/34eeb88)**  
  
Closed: Oct 2012  
Type: Bug  
Components: Blink  
Labels: []  
Regress: [mjsunit/regress/regress-crbug-72736.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-72736.js)  
```javascript
var obj = {};
Object.defineProperty(obj, 'foo', { value: 10, configurable: true });
assertEquals(obj.foo, 10);
Object.defineProperty(obj, 'foo', { value: 20, configurable: true });
assertEquals(obj.foo, 20);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/34eeb88^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=34eeb88)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=34eeb88)  
[test/mjsunit/regress/regress-crbug-72736.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-72736.js?cl=34eeb88)  
  

### **crbug:40931**  
   
**[Issue: Javascript split method of string object returns array with some additional strange keys](https://crbug.com/40931)**  
**[Commit: Added regression test for crbug 40931 http://crbug.com/40931](https://chromium.googlesource.com/v8/v8/+/f066a9a)**  
  
Closed: Apr 2010  
Type: Bug  
Components: None  
Labels: ["Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-40931.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-40931.js)  
```javascript
var names = "a,b,c,d";

for(var i = 0; i < 10; i++) {
  var splitNames = names.split(/,/);
  var forInNames = [];
  var count = 0;
  for (name in splitNames) {
    forInNames[count++] = name;
  }
  forInNames.sort();
  assertEquals("0,1,2,3", forInNames.join());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f066a9a^!)  
[test/mjsunit/regress/regress-crbug-40931.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-40931.js?cl=f066a9a)  
  

### **crbug:39160**  
   
**[Issue: Reliability failure in V8 generated code](https://crbug.com/39160)**  
**[Commit: Re-apply "Inline floating point compare"](https://chromium.googlesource.com/v8/v8/+/6a63910)**  
  
Closed: May 2010  
Type: Bug  
Components: None  
Labels: ["Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-39160.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-39160.js)  
```javascript
function f(a) {
  for (var i = 1073741820; i < 1073741822; i++) {
    if (a < i) {
      a += i;
    }
  }
}

f(5)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6a63910^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=6a63910)  
[src/codegen.h](https://cs.chromium.org/chromium/src/v8/src/codegen.h?cl=6a63910)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=6a63910)  
[src/ia32/codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.h?cl=6a63910)  
[src/jump-target.cc](https://cs.chromium.org/chromium/src/v8/src/jump-target.cc?cl=6a63910)  
[src/jump-target.h](https://cs.chromium.org/chromium/src/v8/src/jump-target.h?cl=6a63910)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=6a63910)  
[test/mjsunit/regress/regress-crbug-39160.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-39160.js?cl=6a63910)  
  

### **crbug:37853**  
   
**[Issue: CHECK fail when editing a sites page](https://crbug.com/37853)**  
**[Commit: Fix code cache lookup for keyed IC's](https://chromium.googlesource.com/v8/v8/+/b0c9738)**  
  
Closed: Mar 2010  
Type: Bug  
Components: Blink  
Labels: ["Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-37853.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-37853.js)  
```javascript
function f(o, k) { return o[k]; }
a = {'a':1, 1:'a'}
f(a, 'a')
f(a, 'a')
f(a, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0c9738^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=b0c9738)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b0c9738)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=b0c9738)  
[test/mjsunit/regress/regress-crbug-37853.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-37853.js?cl=b0c9738)  
  

### **crbug:18639**  
   
**[Issue: Crash [@ 0xffffffff]](https://crbug.com/18639)**  
**[Commit: Add regression test case for http://crbug.com/18639 which](https://chromium.googlesource.com/v8/v8/+/6621a43)**  
  
Closed: Aug 2009  
Type: Bug-Security  
Components: None  
Labels: ["Area-MISC", "reward-topanel", "Security-High", "Security", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-18639.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-18639.js)  
```javascript
try {
  toString = toString;
  __defineGetter__("z", (0).toLocaleString);
  z;
  z;
  ((0).toLocaleString)();
} catch (e) {
  assertInstanceof(e, TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6621a43^!)  
[test/mjsunit/regress/regress-246.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-246.js?cl=6621a43)  
[test/mjsunit/regress/regress-254.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-254.js?cl=6621a43)  
[test/mjsunit/regress/regress-crbug-18639.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-18639.js?cl=6621a43)  
  

### **crbug:7907**  
   
**[Issue: Crash - WebCore::FrameView::contentsResized()](https://crbug.com/7907)**  
**[Commit: [array] Fix read-only property in NumberDictionary fast-path](https://chromium.googlesource.com/v8/v8/+/327668d)**  
  
Closed: Feb 2009  
Type: Bug-Regression  
Components: Blink  
Labels: ["Crash-2.0.164.0", "Stable", "bulkmove", "Restrict-AddIssueComment-Commit"]  
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
  

### **crbug:3867**  
   
**[Issue: Objects not created or called in order!!](https://crbug.com/3867)**  
**[Commit: Handle insertion order for simple constructors](https://chromium.googlesource.com/v8/v8/+/1091039)**  
  
Closed: Feb 2010  
Type: Bug  
Components: Blink  
Labels: ["NeedsEngReview", "v-154.9", "Size-Small", "Has-Reductions", "Mstone-2.1", "javascript", "chrome-specific", "Restrict-AddIssueComment-Commit"]  
Regress: [mjsunit/regress/regress-crbug-3867.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-3867.js)  
```javascript
function props(x) {
  var result = [];
  for (var p in x) result.push(p);
  return result;
}

function A() {
  this.a1 = 1234;
  this.a2 = "D";
  this.a3 = false;
}

function B() {
  this.b3 = false;
  this.b2 = "D";
  this.b1 = 1234;
}

function C() {
  this.c3 = false;
  this.c1 = 1234;
  this.c2 = "D";
}

assertArrayEquals(["a1", "a2", "a3"], props(new A()));
assertArrayEquals(["b3", "b2", "b1"], props(new B()));
assertArrayEquals(["c3", "c1", "c2"], props(new C()));
assertArrayEquals(["s1", "s2", "s3"], props({s1: 0, s2: 0, s3: 0}));
assertArrayEquals(["s3", "s2", "s1"], props({s3: 0, s2: 0, s1: 0}));
assertArrayEquals(["s3", "s1", "s2"], props({s3: 0, s1: 0, s2: 0}));

var a = new A()
a.a0 = 0;
a.a4 = 0;
assertArrayEquals(["a1", "a2", "a3", "a0", "a4"], props(a));

var b = new B()
b.b4 = 0;
b.b0 = 0;
assertArrayEquals(["b3", "b2", "b1", "b4", "b0"], props(b));

var o1 = {s1: 0, s2: 0, s3: 0}
o1.s0 = 0;
o1.s4 = 0;
assertArrayEquals(["s1", "s2", "s3", "s0", "s4"], props(o1));

var o2 = {s3: 0, s2: 0, s1: 0}
o2.s4 = 0;
o2.s0 = 0;
assertArrayEquals(["s3", "s2", "s1", "s4", "s0"], props(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1091039^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=1091039)  
[test/mjsunit/regress/regress-crbug-3867.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-3867.js?cl=1091039)  
  

### **crbug:3184**  
   
**[Issue: apply and call does not applied on this object](https://crbug.com/3184)**  
**[Commit: Ensure correct boxing of values when calling functions on them](https://chromium.googlesource.com/v8/v8/+/562f90d)**  
  
Closed: Jan 2010  
Type: Bug  
Components: Blink  
Labels: ["M-5", "Restrict-AddIssueComment-EditIssue", "Has-reduction", "chrome-specific"]  
Regress: [mjsunit/regress/regress-crbug-3184.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-3184.js)  
```javascript
Object.extend = function (dest, source) {
  for (property in source) dest[property] = source[property];
  return dest;
};

Object.extend ( Function.prototype,
{
  wrap : function (wrapper) {
    var method = this;
    var bmethod = (function(_method) {
      return function () {
        this.$$$parentMethodStore$$$ = this.$proceed;
        this.$proceed = function() { return _method.apply(this, arguments); };
      };
    })(method);
    var amethod = function () {
      this.$proceed = this.$$$parentMethodStore$$$;
      if (this.$proceed == undefined) delete this.$proceed;
      delete this.$$$parentMethodStore$$$;
    };
    var value = function() { bmethod.call(this); retval = wrapper.apply(this, arguments); amethod.call(this); return retval; };
    return value;
  }
});

String.prototype.cap = function() {
  return this.charAt(0).toUpperCase() + this.substring(1).toLowerCase();
};

String.prototype.cap = String.prototype.cap.wrap(
  function(each) {
    if (each && this.indexOf(" ") != -1) {
      return this.split(" ").map(
        function (value) {
          return value.cap();
        }
      ).join(" ");
    } else {
      return this.$proceed();
  }
});

Object.extend( Array.prototype,
{
  map : function(fun) {
    if (typeof fun != "function") throw new TypeError();
    var len = this.length;
    var res = new Array(len);
    var thisp = arguments[1];
    for (var i = 0; i < len; i++) { if (i in this) res[i] = fun.call(thisp, this[i], i, this); }
    return res;
  }
});
assertEquals("Test1 test1", "test1 test1".cap());
assertEquals("Test2 Test2", "test2 test2".cap(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/562f90d^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=562f90d)  
[src/arm/codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.h?cl=562f90d)  
[src/arm/fast-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/fast-codegen-arm.cc?cl=562f90d)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=562f90d)  
[src/codegen.h](https://cs.chromium.org/chromium/src/v8/src/codegen.h?cl=562f90d)  
[src/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug.cc?cl=562f90d)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=562f90d)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=562f90d)  
[src/ia32/codegen-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.h?cl=562f90d)  
[src/ia32/fast-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/fast-codegen-ia32.cc?cl=562f90d)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=562f90d)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=562f90d)  
[src/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic.h?cl=562f90d)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=562f90d)  
[src/x64/codegen-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.h?cl=562f90d)  
[src/x64/fast-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/fast-codegen-x64.cc?cl=562f90d)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=562f90d)  
[test/mjsunit/bugs/bug-223.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-223.js?cl=562f90d)  
[test/mjsunit/regress/regress-crbug-3184.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-3184.js?cl=562f90d)  
[test/mjsunit/value-wrapper.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/value-wrapper.js?cl=562f90d)  
  
