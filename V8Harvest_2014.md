# V8Harvest  
The Harvest of V8 regress in 2014.  
  

## **regress-445267.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/445267)**  
**[Commit: [turbofan] Fix invalid bounds check with overflowing offset.](https://chromium.googlesource.com/v8/v8/+/ef41f70)**  
  
Date(Commit): Mon Dec 29 10:01:15 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "merge-merged-3.31", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "reward-3500", "M-41", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/825403002](https://codereview.chromium.org/825403002)  
Regress: [mjsunit/compiler/regress-445267.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445267.js)  
```javascript
var foo = (function Module(stdlib, foreign, heap) {
  "use asm";
  var MEM16 = new stdlib.Int16Array(heap);
  function foo(i) {
    i = i|0;
    i = MEM16[i + 2147483650 >> 1]|0;
    return i;
  }
  return { foo: foo };
})(this, {}, new ArrayBuffer(64 * 1024)).foo;

foo(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef41f70^!)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=ef41f70)  
[test/mjsunit/compiler/regress-445267.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-445267.js?cl=ef41f70)  
  

---   

## **regress-3786.js (v8 issue)**  
   
**[Issue: InstructionOperand limits number of parameters in function call to 2^9 and number of virtual registers to 2^18](https://crbug.com/v8/3786)**  
**[Commit: [turbofan] Raise max virtual registers and call parameter limit.](https://chromium.googlesource.com/v8/v8/+/b5e8dd0)**  
  
Date(Commit): Thu Dec 25 18:18:04 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/826673002](https://codereview.chromium.org/826673002)  
Regress: [mjsunit/compiler/regress-3786.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3786.js)  
```javascript
var foo = (function Module(stdlib, foreign, heap) {
  "use asm";
  var f = stdlib.Math.cos;
  function foo() {
    return f(48,48,48,59,32,102,111,110,116,45,119,101,105,103,104,116,58,98,111,108,100,59,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,32,99,111,108,111,114,61,34,35,70,70,48,48,48,48,34,62,70,79,82,69,88,47,80,65,82,38,35,51,48,52,59,60,119,98,114,32,47,62,84,69,32,38,35,51,48,52,59,38,35,51,53,48,59,76,69,77,76,69,82,38,35,51,48,52,59,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,115,112,97,110,32,105,100,61,34,97,99,95,100,101,115,99,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,49,112,120,59,32,99,111,108,111,114,58,35,48,48,48,48,48,48,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,38,112,111,117,110,100,59,47,36,32,50,32,112,105,112,44,32,89,84,76,32,49,50,32,112,105,112,44,65,108,116,38,35,51,48,53,59,110,32,51,32,99,101,110,116,46,32,83,97,98,105,116,32,83,112,114,101,97,100,45,84,38,117,117,109,108,59,114,60,119,98,114,32,47,62,107,32,66,97,110,107,97,115,38,35,51,48,53,59,32,65,86,65,78,84,65,74,73,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,100,105,118,32,105,100,61,34,97,99,95,117,114,108,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,48,112,120,59,32,99,111,108,111,114,58,35,70,70,54,54,57,57,59,32,102,111,110,116,45,102,97114,105,97);
  }
  return { foo: foo };
})(this, {}).foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b5e8dd0^!)  
[src/compiler/instruction.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction.h?cl=b5e8dd0)  
[test/mjsunit/compiler/regress-3786.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-3786.js?cl=b5e8dd0)  
  

---   

## **regress-444695.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/444695)**  
**[Commit: [turbofan] Fix missing ChangeUint32ToUint64 in lowering of LoadBuffer.](https://chromium.googlesource.com/v8/v8/+/3f00ce2)**  
  
Date(Commit): Tue Dec 23 06:54:00 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "CVE-2014-7927", "External-Fuzzer-Contribution", "reward-3500", "M-40", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/824843002](https://codereview.chromium.org/824843002)  
Regress: [mjsunit/compiler/regress-444695.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-444695.js)  
```javascript
var foo = (function(stdlib, foreign, heap) {
  "use asm";
  var MEM = new stdlib.Uint8Array(heap);
  function foo(x) { MEM[x | 0] *= 0; }
  return {foo: foo};
})(this, {}, new ArrayBuffer(1)).foo;
foo(-926416896 * 8 * 1024);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f00ce2^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=3f00ce2)  
[test/mjsunit/compiler/regress-444695.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-444695.js?cl=3f00ce2)  
  

---   

## **regress-444508.js (chromium issue)**  
   
**[Issue: Unreachable code in ../../v8/src/compiler/simplified-operator.cc(49)](https://crbug.com/444508)**  
**[Commit: [turbofan] Correctify lowering of Uint8ClampedArray buffer access.](https://chromium.googlesource.com/v8/v8/+/65e6949)**  
  
Date(Commit): Mon Dec 22 08:27:59 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/794013004](https://codereview.chromium.org/794013004)  
Regress: [mjsunit/compiler/regress-444508.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-444508.js)  
```javascript
(function Module(stdlib, foreign, heap) {
  "use asm";
  // This is not valid asm.js, but should nevertheless work.
  var MEM = new Uint8ClampedArray(heap);
  function foo(i) { MEM[0] = 1; }
  return {foo: foo};
})(this, {}, new ArrayBuffer(64 * 1024)).foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65e6949^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=65e6949)  
[src/compiler/simplified-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-operator.cc?cl=65e6949)  
[test/mjsunit/compiler/regress-444508.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-444508.js?cl=65e6949)  
  

---   

## **regress-443744.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/443744)**  
**[Commit: [turbofan] Fix unsafe out-of-bounds check for checked loads/stores.](https://chromium.googlesource.com/v8/v8/+/f7e4689)**  
  
Date(Commit): Fri Dec 19 12:53:29 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "Stability-Memory-AddressSanitizer", "M-41", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/804993004](https://codereview.chromium.org/804993004)  
Regress: [mjsunit/compiler/regress-443744.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-443744.js)  
```javascript
var m = (function(stdlib, foreign, heap) {
  "use asm";
  var MEM = new stdlib.Uint8Array(heap);
  function f(x) {
    x = x | 0;
    MEM[x] = 0;
  }
  return {f: f};
})(this, {}, new ArrayBuffer(1));
m.f(-926416896 * 32 * 1024);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f7e4689^!)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=f7e4689)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=f7e4689)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=f7e4689)  
[test/mjsunit/compiler/regress-443744.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-443744.js?cl=f7e4689)  
  

---   

## **regress-441099.js (chromium issue)**  
   
**[Issue: Bad-cast to const Operator1<v8::internal::Unique<v8::internal::Object> > from v8::internal::compiler::Operator1<v8::internal::Unique<v8::internal::HeapObject>, std::equal_to<v8::internal::Unique<v8::internal::HeapObject> >, v8::base::hash<v8::internal::Unique<v8::internal::HeapObject> > >;operator.h:172:10](https://crbug.com/441099)**  
**[Commit: More -fsanitize=vptr fixes.](https://chromium.googlesource.com/v8/v8/+/cbf3b0b)**  
  
Date(Commit): Tue Dec 16 14:20:28 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/812563002](https://codereview.chromium.org/812563002)  
Regress: [mjsunit/regress/regress-441099.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-441099.js)  
```javascript
var Module;
if (!Module) Module = eval('(function() { try { return Module || {} } catch(e) { return {} } })()');
else if (ENVIRONMENT_IS_SHELL) {
}
var Runtime = {
  stackSave: function () {
  },
  alignMemory: function (quantum) { var ret = size = Math.ceil()*(quantum ? quantum : 8); return ret; }}
function allocate() {
}
function callRuntimeCallbacks(callbacks) {
    var callback = callbacks.shift();
    var func = callback.func;
    if (typeof func === 'number') {
    } else {
      func();
    }
}
var __ATINIT__    = []; // functions called during startup
function ensureInitRuntime() {
  callRuntimeCallbacks(__ATINIT__);
}
/* global initializers */ __ATINIT__.push({ func: function() { runPostSets() } });
    function __formatString() {
            switch (next) {
            }
    }
  var Browser={mainLoop:{queue:[],pause:function () {
        }},moduleContextCreatedCallbacks:[],workers:[],init:function () {
      }};
var asm = (function() {
  'use asm';
function setThrew() {
}
function runPostSets() {
}
function _main() {
}
function _free() {
}
  return { runPostSets: runPostSets};
})
();
var runPostSets = Module["runPostSets"] = asm["runPostSets"];
var i64Math = (function() { // Emscripten wrapper
  /**
   */
})();
    ensureInitRuntime();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cbf3b0b^!)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=cbf3b0b)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=cbf3b0b)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=cbf3b0b)  
[src/compiler/instruction-selector-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector-impl.h?cl=cbf3b0b)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=cbf3b0b)  
...  
  

---   

## **regress-3756.js (v8 issue)**  
   
**[Issue: Regexps: \u as identity escape behaves differently depending on whether it's at the end of the regexp or not](https://crbug.com/v8/3756)**  
**[Commit: RegExpParser: Fix Reset()ting to the end.](https://chromium.googlesource.com/v8/v8/+/978f41a)**  
  
Date(Commit): Tue Dec 16 12:14:19 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/802313003](https://codereview.chromium.org/802313003)  
Regress: [mjsunit/regress/regress-3756.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3756.js)  
```javascript
(function TestIdentityEscapes() {
  // \u not followed by 4 hex digits is treated as an identity escape.
  var r0 = /\u/;
  assertTrue(r0.test("u"));

  r0 = RegExp("\\u");
  assertTrue(r0.test("u"));

  var r1 = /\usecond/;
  assertTrue(r1.test("usecond"));

  r1 = RegExp("\\usecond");
  assertTrue(r1.test("usecond"));

  var r2 = /first\u/;
  assertTrue(r2.test("firstu"));
  // This used to return true (which was a bug).
  assertFalse(r2.test("first\\u"));

  r2 = RegExp("first\\u");
  assertTrue(r2.test("firstu"));
  // This used to return true (which was a bug).
  assertFalse(r2.test("first\\u"));

  var r3 = /first\usecond/;
  assertTrue(r3.test("firstusecond"));
  assertFalse(r3.test("first\\usecond"));

  r3 = RegExp("first\\usecond");
  assertTrue(r3.test("firstusecond"));
  assertFalse(r3.test("first\\usecond"));

  var r4 = /first\u123second/;
  assertTrue(r4.test("firstu123second"));
  assertFalse(r4.test("first\\u123second"));

  r4 = RegExp("first\\u123second");
  assertTrue(r4.test("firstu123second"));
  assertFalse(r4.test("first\\u123second"));

  // \X where X is not a legal escape character is treated as identity escape
  // too.
  var r5 = /\a/;
  assertTrue(r5.test("a"));

  r5 = RegExp("\\a");
  assertTrue(r5.test("a"));

  var r6 = /\asecond/;
  assertTrue(r6.test("asecond"));

  r6 = RegExp("\\asecond");
  assertTrue(r6.test("asecond"));

  var r7 = /first\a/;
  assertTrue(r7.test("firsta"));
  assertFalse(r7.test("first\\a"));

  r7 = RegExp("first\\a");
  assertTrue(r7.test("firsta"));
  assertFalse(r7.test("first\\a"));

  var r8 = /first\asecond/;
  assertTrue(r8.test("firstasecond"));
  assertFalse(r8.test("first\\asecond"));

  r8 = RegExp("first\\asecond");
  assertTrue(r8.test("firstasecond"));
  assertFalse(r8.test("first\\asecond"));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/978f41a^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=978f41a)  
[test/mjsunit/regress/regress-3756.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3756.js?cl=978f41a)  
  

---   

## **regress-int32array-outofbounds-nan.js (other issue)**  
   
**[Commit: [turbofan] Quickfix for invalid number truncation of typed array loads.](https://chromium.googlesource.com/v8/v8/+/14409ab)**  
  
Date(Commit): Fri Dec 12 10:45:38 2014  
Code Review: [https://codereview.chromium.org/803483002](https://codereview.chromium.org/803483002)  
Regress: [mjsunit/compiler/regress-int32array-outofbounds-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-int32array-outofbounds-nan.js)  
```javascript
function Module(stdlib, foreign, heap) {
  "use asm";
  var MEM32 = new stdlib.Int32Array(heap);
  function foo(i) {
    i = i|0;
    return +MEM32[i >> 2];
  }
  return {foo: foo};
}

var foo = Module(this, {}, new ArrayBuffer(4)).foo;
assertEquals(NaN, foo(-4));
assertEquals(NaN, foo(4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14409ab^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=14409ab)  
[test/mjsunit/compiler/regress-int32array-outofbounds-nan.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-int32array-outofbounds-nan.js?cl=14409ab)  
  
  
---   

## **regress-ntl.js (other issue)**  
   
**[Commit: [turbofan] Fix control reducer bug with NTLs.](https://chromium.googlesource.com/v8/v8/+/aeda76c)**  
  
Date(Commit): Tue Dec 09 15:09:59 2014  
Code Review: [https://codereview.chromium.org/789773002](https://codereview.chromium.org/789773002)  
Regress: [mjsunit/regress/regress-ntl.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-ntl.js)  
```javascript
function mod1() {
  var v_1 = 1;
  var v_2 = 1;
  v_1++;
  v_2 = {valueOf: function() { throw "gagh"; }};

  function bug1() {
    for (var i = 0; i < 1; v_2++) {
      if (v_1 == 1) ;
    }
  }

  return bug1;
}

var f = mod1();
assertThrows(f);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);


var v_3 = 1;
var v_4 = 1;
v_3++;
v_4 = {valueOf: function() { throw "gagh"; }};

function bug2() {
  for (var i = 0; i < 1; v_4++) {
    if (v_3 == 1) ;
  }
}

assertThrows(bug2);
%OptimizeFunctionOnNextCall(bug2);
assertThrows(bug2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aeda76c^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=aeda76c)  
[test/mjsunit/regress-ntl.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-ntl.js?cl=aeda76c)  
  
  
---   

## **regress-439743.js (chromium issue)**  
   
**[Issue: Asm.js-based BPG decoder broken in recent Chrome builds](https://crbug.com/439743)**  
**[Commit: [x86] Disable invalid checked load/store optimization.](https://chromium.googlesource.com/v8/v8/+/48a6766)**  
  
Date(Commit): Tue Dec 09 14:16:34 2014  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["ReleaseBlock-Dev"]  
Code Review: [https://codereview.chromium.org/784153006](https://codereview.chromium.org/784153006)  
Regress: [mjsunit/compiler/regress-439743.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-439743.js)  
```javascript
function module(stdlib, foreign, heap) {
    "use asm";
    var MEM32 = new stdlib.Int32Array(heap);
    function foo(i) {
      i = i|0;
      MEM32[0] = i;
      return MEM32[i + 4 >> 2]|0;
    }
    return { foo: foo };
}

var foo = module(this, {}, new ArrayBuffer(64*1024)).foo;
assertEquals(-4, foo(-4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/48a6766^!)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=48a6766)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=48a6766)  
[test/mjsunit/compiler/regress-lena.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-lena.js?cl=48a6766)  
  

---   

## **regress-3741.js (v8 issue)**  
   
**[Issue: Deopt problems in ES6](https://crbug.com/v8/3741)**  
**[Commit: Fix the order of context binding/simulate insertion for BlockContexts.](https://chromium.googlesource.com/v8/v8/+/bd04e6c)**  
  
Date(Commit): Fri Dec 05 13:06:50 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/762393008](https://codereview.chromium.org/762393008)  
Regress: [mjsunit/es6/regress/regress-3741.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3741.js)  
```javascript
'use strict';
function f24(deopt) {
  let x = 1;
  {
    let x = 2;
    {
      let x = 3;
      assertEquals(3, x);
    }
    deopt + 1;
    assertEquals(2, x);
  }
  assertEquals(1, x);
}


for (var j = 0; j < 10; ++j) {
  f24(12);
}
%OptimizeFunctionOnNextCall(f24);
f24({});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bd04e6c^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=bd04e6c)  
[test/mjsunit/harmony/regress/regress-3741.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-3741.js?cl=bd04e6c)  
  

---   

## **regress-437765.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/lithium-codegen.cc,](https://crbug.com/437765)**  
**[Commit: Fixed environment handling for LFlooringDivI on ARM.](https://chromium.googlesource.com/v8/v8/+/c16b8f6)**  
  
Date(Commit): Tue Dec 02 13:47:19 2014  
Components/Type: None/Bug  
Labels: ["Te-Logged", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/775613002](https://codereview.chromium.org/775613002)  
Regress: [mjsunit/regress/regress-437765.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-437765.js)  
```javascript
function foo(x, y) {
  return Math.floor(x / y);
}

function bar(x, y) {
  return foo(x + 1, y + 1);
}

function baz() {
  bar(64, 2);
}

baz();
baz();
%OptimizeFunctionOnNextCall(baz);
baz();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c16b8f6^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=c16b8f6)  
[test/mjsunit/regress/regress-437765.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-437765.js?cl=c16b8f6)  
  

---   

## **regress-crbug-436820.js (chromium issue)**  
   
**[Issue: CHECK(value->IsMutableHeapNumber()) failed: ../../v8/src/objects.cc(2135)](https://crbug.com/436820)**  
**[Commit: Map::CopyGeneralizeAllRepresentations() left incorrect layout descriptor in a new map.](https://chromium.googlesource.com/v8/v8/+/1a2e4b2)**  
  
Date(Commit): Wed Nov 26 17:37:05 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Arch-x86_64", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/759823004](https://codereview.chromium.org/759823004)  
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
  

---   

## **regress-436893.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!info()->shared_info()->optimization_disabled()) failed: ../../v8/src](https://crbug.com/436893)**  
**[Commit: Abort optimization in corner case.](https://chromium.googlesource.com/v8/v8/+/9da4998)**  
  
Date(Commit): Wed Nov 26 16:57:52 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/760063002](https://codereview.chromium.org/760063002)  
Regress: [mjsunit/regress/regress-436893.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-436893.js)  
```javascript
var x = 11;
function foo() {
  return 42;
}
function g() { return foo.apply(null, x()++); }
%OptimizeFunctionOnNextCall(g);
assertThrows(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9da4998^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=9da4998)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=9da4998)  
[test/mjsunit/regress/regress-436893.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-436893.js?cl=9da4998)  
  

---   

## **regress-410030.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject())) fa](https://crbug.com/410030)**  
**[Commit: Introduce legacy const slots in correct context.](https://chromium.googlesource.com/v8/v8/+/626f110)**  
  
Date(Commit): Wed Nov 26 12:16:30 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Release-0-M40", "M-40", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "merge-merged-3.30"]  
Code Review: [https://codereview.chromium.org/756293004](https://codereview.chromium.org/756293004)  
Regress: [mjsunit/regress/regress-410030.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-410030.js)  
```javascript
try {
  throw 0;
} catch(e) {
  assertSame(3, eval("delete x; const x=3; x"));
}


try {
  throw 0;
} catch(e) {
  assertSame(3, (1,eval)("delete x1; const x1=3; x1"));
}


try {
  throw 0;
} catch(e) {
  with({}) {
    assertSame(3, eval("delete x2; const x2=3; x2"));
  }
}


(function f() {
  try {
    throw 0;
  } catch(e) {
    assertSame(3, eval("delete x; const x=3; x"));
  }
}());


(function f() {
  try {
    throw 0;
  } catch(e) {
    assertSame(3, (1,eval)("delete x4; const x4=3; x4"));
  }
}());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/626f110^!)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=626f110)  
[test/mjsunit/regress/regress-410030.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-410030.js?cl=626f110)  
  

---   

## **regress-2858.js (v8 issue)**  
   
**[Issue: v8 --harmony fails on redeclaration (var) of exception variable](https://crbug.com/v8/2858)**  
**[Commit: harmony-scoping: Catch variable should be VAR, not LET](https://chromium.googlesource.com/v8/v8/+/f1d8668)**  
  
Date(Commit): Tue Nov 25 14:48:39 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/748113003](https://codereview.chromium.org/748113003)  
Regress: [mjsunit/es6/regress/regress-2858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2858.js)  
```javascript
"use strict";

function f() {
    var y = 1;
    var q1;
    var q;
    var z = new Error();
    try {
        throw z;
    } catch (y) {
      assertTrue(z === y);
      q1 = function() { return y; }
      var y = 15;
      q = function() { return y; }
      assertSame(15, y);
    }
    assertSame(1, y);
    assertSame(15, q1());
    assertSame(15, q());
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f1d8668^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=f1d8668)  
[test/mjsunit/harmony/block-conflicts.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/block-conflicts.js?cl=f1d8668)  
[test/mjsunit/harmony/regress/regress-2858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-2858.js?cl=f1d8668)  
  

---   

## **regress-lea-matching.js (other issue)**  
   
**[Commit: [turbofan] Fix matching of the lea instruction.](https://chromium.googlesource.com/v8/v8/+/d9cabb9)**  
  
Date(Commit): Mon Nov 24 17:45:33 2014  
Code Review: [https://codereview.chromium.org/756643002](https://codereview.chromium.org/756643002)  
Regress: [mjsunit/regress/regress-lea-matching.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lea-matching.js)  
```javascript
function f(a, b, c) {
  a = a|0;
  b = b|0;
  c = c|0;
  var r = 0;
  r = a + ((b << 1) + c) | 0;
  return r|0;
}

assertEquals(8, f(1, 2, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9cabb9^!)  
[src/compiler/node-matchers.h](https://cs.chromium.org/chromium/src/v8/src/compiler/node-matchers.h?cl=d9cabb9)  
[test/mjsunit/regress/regress-lea-matching.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-lea-matching.js?cl=d9cabb9)  
[test/unittests/compiler/x64/instruction-selector-x64-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/x64/instruction-selector-x64-unittest.cc?cl=d9cabb9)  
  
  
---   

## **regress-crbug-435825.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::String::length](https://crbug.com/435825)**  
**[Commit: Fix RegExp.source for uncompiled regexp.](https://chromium.googlesource.com/v8/v8/+/14a3b91)**  
  
Date(Commit): Mon Nov 24 11:21:52 2014  
Components/Type: None/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Stability-Memory-AddressSanitizer", "External-Fuzzer-Contribution", "Security_Severity-Low", "M-41", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/753983002](https://codereview.chromium.org/753983002)  
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
  

---   

## **regress-3229.js (v8 issue)**  
   
**[Issue: Slashes in regex.source should be escaped](https://crbug.com/v8/3229)**  
**[Commit: Correctly escape RegExp source.](https://chromium.googlesource.com/v8/v8/+/61bee5c)**  
  
Date(Commit): Fri Nov 21 10:50:24 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/736003002](https://codereview.chromium.org/736003002)  
Regress: [mjsunit/regress/regress-3229.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3229.js)  
```javascript
function testEscapes(expected, regexp) {
  assertEquals(expected, regexp.source);
  assertEquals("/" + expected + "/", regexp.toString());
}

testEscapes("\\/", /\//);
testEscapes("\\/\\/", /\/\//);
testEscapes("\\/", new RegExp("/"));
testEscapes("\\/", new RegExp("\\/"));
testEscapes("\\\\\\/", new RegExp("\\\\/"));
testEscapes("\\/\\/", new RegExp("\\/\\/"));
testEscapes("\\/\\/\\/\\/", new RegExp("////"));
testEscapes("\\/\\/\\/\\/", new RegExp("\\//\\//"));
testEscapes("(?:)", new RegExp(""));

var r = /\/\//;
testEscapes("\\/\\/", r);
r.source = "garbage";
testEscapes("\\/\\/", r);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/61bee5c^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=61bee5c)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=61bee5c)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=61bee5c)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=61bee5c)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=61bee5c)  
...  
  

---   

## **regress-435477.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!map->IsStringMap()) failed: ../../v8/src/hydrogen.cc(7109)](https://crbug.com/435477)**  
**[Commit: Assert to protect against polymorphic string loads fires on valid stores.](https://chromium.googlesource.com/v8/v8/+/cf57269)**  
  
Date(Commit): Fri Nov 21 10:29:08 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/751513002](https://codereview.chromium.org/751513002)  
Regress: [mjsunit/regress/regress-435477.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-435477.js)  
```javascript
var a = new Array(128);

function f(a, base) {
  a[base] = 2;
}

f(a, undefined);
f("r12", undefined);
f(a, 0);
%OptimizeFunctionOnNextCall(f);
f(a, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf57269^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=cf57269)  
[test/mjsunit/regress/regress-435477.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-435477.js?cl=cf57269)  
  

---   

## **regress-435073.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(p->IsSmi()) failed: ../../v8/src/objects-debug.cc(32)](https://crbug.com/435073)**  
**[Commit: Fix for 435073: CHECK failure in CHECK(p->IsSmi()) failed.](https://chromium.googlesource.com/v8/v8/+/3d58b82a)**  
  
Date(Commit): Fri Nov 21 10:14:19 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Release-0-M40", "reward-3500", "CVE-2014-7928", "M-40", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/737383002](https://codereview.chromium.org/737383002)  
Regress: [mjsunit/regress/regress-435073.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-435073.js)  
```javascript
function test(x) { [x,,]; }

test(0);
test(0);
%OptimizeFunctionOnNextCall(test);
test(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d58b82a^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3d58b82a)  
[test/mjsunit/array-methods-read-only-length.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-methods-read-only-length.js?cl=3d58b82a)  
[test/mjsunit/array-shift4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-shift4.js?cl=3d58b82a)  
[test/mjsunit/regress/regress-435073.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-435073.js?cl=3d58b82a)  
  

---   

## **regress-3709.js (v8 issue)**  
   
**[Issue: unexpected deoptimization due to fn.apply(this, arguments);](https://crbug.com/v8/3709)**  
**[Commit: Do not bailout from optimizing functions that use f(x, arguments)](https://chromium.googlesource.com/v8/v8/+/dc88962)**  
  
Date(Commit): Thu Nov 20 17:07:44 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/736043002](https://codereview.chromium.org/736043002)  
Regress: [mjsunit/regress/regress-3709.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3709.js)  
```javascript
function getobj() {
  return { bar : function() { return 0}};
}

function foo() {
  var obj = getobj();
  var length = arguments.length;
  if (length == 0) {
     obj.bar();
  } else {
     obj.bar.apply(obj, arguments);
  }
}

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
assertOptimized(foo);
foo(10);
assertUnoptimized(foo);
%ClearFunctionFeedback(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dc88962^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=dc88962)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=dc88962)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=dc88962)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=dc88962)  
[src/type-info.h](https://cs.chromium.org/chromium/src/v8/src/type-info.h?cl=dc88962)  
...  
  

---   

## **regress-crbug-433332.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(lower->Is(upper)) failed: ../../v8/src/types.h(1025)](https://crbug.com/433332)**  
**[Commit: Fix lower bound violation](https://chromium.googlesource.com/v8/v8/+/4f63564)**  
  
Date(Commit): Thu Nov 20 11:22:49 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/739563002](https://codereview.chromium.org/739563002)  
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
  

---   

## **regress-crbug-433766.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(entry.is_valid()) failed: ../../v8/src/parser.cc(3823)](https://crbug.com/433766)**  
**[Commit: Fix one more missing c0_ < 0 check in scanner](https://chromium.googlesource.com/v8/v8/+/bf22724)**  
  
Date(Commit): Mon Nov 17 09:43:31 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/731953003](https://codereview.chromium.org/731953003)  
Regress: [mjsunit/regress/regress-crbug-433766.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-433766.js)  
```javascript
var filler = "//" + new Array(('@')).join('x');

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
  

---   

## **regress-unsigned-mul-add.js (other issue)**  
   
**[Commit: [turbofan] Remove int32 narrowing during typed lowering.](https://chromium.googlesource.com/v8/v8/+/c3af691)**  
  
Date(Commit): Mon Nov 17 09:04:52 2014  
Code Review: [https://codereview.chromium.org/721723004](https://codereview.chromium.org/721723004)  
Regress: [mjsunit/regress/regress-unsigned-mul-add.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-unsigned-mul-add.js)  
```javascript
function f(a) {
  var x = a >>> 0;
  return (x * 1.0 + x * 1.0) << 0;
}

assertEquals(-2, f(-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3af691^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=c3af691)  
[test/cctest/compiler/test-js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-js-typed-lowering.cc?cl=c3af691)  
[test/mjsunit/regress/regress-unsigned-mul-add.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-unsigned-mul-add.js?cl=c3af691)  
  
  
---   

## **regress-3683.js (v8 issue)**  
   
**[Issue: C-style for-let can't handle continue](https://crbug.com/v8/3683)**  
**[Commit: Fix desugaring of let bindings in for loops to handle continue properly](https://chromium.googlesource.com/v8/v8/+/b17eaaa)**  
  
Date(Commit): Fri Nov 14 19:33:23 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/720863002](https://codereview.chromium.org/720863002)  
Regress: [mjsunit/es6/regress/regress-3683.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3683.js)  
```javascript
"use strict";

var count = 0;
for (let x = 0; x < 10;) {
  x++;
  count++;
  continue;
}
assertEquals(10, count);

count = 0;
label: for (let x = 0; x < 10;) {
  while (true) {
    x++;
    count++;
    continue label;
  }
}
assertEquals(10, count);

count = 0;
label: for (let x = 0; x < 10;) {
  x++;
  count++;
  continue label;
}
assertEquals(10, count);

count = 0;
for (let x = 0; x < 10;) {
  x++;
  count++;
  {
    let x = "hello";
    continue;
  }
}
assertEquals(10, count);

count = 0;
for (let x = 0; x < 10;) {
  x++;
  for (let y = 0; y < 2;) {
    y++;
    count++;
    continue;
  }
}
assertEquals(20, count);

count = 0;
for (let x = 0; x < 10;) {
  x++;
  for (let y = 0; y < 2;) {
    y++;
    count++;
  }
  continue;
}
assertEquals(20, count);

count = 0;
outer: for (let x = 0; x < 10;) {
  x++;
  for (let y = 0; y < 2;) {
    y++;
    count++;
    if (y == 2) continue outer;
  }
}
assertEquals(20, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b17eaaa^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=b17eaaa)  
[test/mjsunit/harmony/regress/regress-3683.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-3683.js?cl=b17eaaa)  
  

---   

## **regress-3687.js (v8 issue)**  
   
**[Issue: Crash in GeneralizeRepresentation](https://crbug.com/v8/3687)**  
**[Commit: Avoid fast short-cut in Map::GeneralizeRepresentation() for literals with non-simple transitions.](https://chromium.googlesource.com/v8/v8/+/bc8c41c)**  
  
Date(Commit): Thu Nov 13 10:56:31 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/715313003](https://codereview.chromium.org/715313003)  
Regress: [mjsunit/regress/regress-3687.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3687.js)  
```javascript
var t1 = { f1: 0 };
var t2 = { f2: 0 };

var z = {
  x: {
    x: t1,
    y: {
      x: {},
      z1: {
        x: t2,
        y: 1
      }
    }
  },
  z2: 0
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bc8c41c^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=bc8c41c)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=bc8c41c)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=bc8c41c)  
[test/mjsunit/regress/regress-3687.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3687.js?cl=bc8c41c)  
  

---   

## **regress-weakening-multiplication.js (other issue)**  
   
**[Commit: [turbofan] Weakening of types must weaken ranges inside unions.](https://chromium.googlesource.com/v8/v8/+/4c1f4b7)**  
  
Date(Commit): Thu Nov 13 05:31:47 2014  
Code Review: [https://codereview.chromium.org/712623002](https://codereview.chromium.org/712623002)  
Regress: [mjsunit/regress/regress-weakening-multiplication.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-weakening-multiplication.js)  
```javascript
function f() {
  for (var j = 1; j < 1; j *= -8) {
  }
  for (var i = 1; i < 1; j += 2) {
    j * -1;
  }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c1f4b7^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=4c1f4b7)  
[src/types.cc](https://cs.chromium.org/chromium/src/v8/src/types.cc?cl=4c1f4b7)  
[src/types.h](https://cs.chromium.org/chromium/src/v8/src/types.h?cl=4c1f4b7)  
[test/cctest/test-types.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-types.cc?cl=4c1f4b7)  
[test/mjsunit/regress/regress-weakening-multiplication.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-weakening-multiplication.js?cl=4c1f4b7)  
  
  
---   

## **regress-crbug-109362.js (chromium issue)**  
   
**[Issue: Line number mismatch in functions created with Function constructor](https://crbug.com/109362)**  
**[Commit: Correctly compute line numbers in functions from the function constructor.](https://chromium.googlesource.com/v8/v8/+/1dbd636)**  
  
Date(Commit): Wed Nov 12 10:06:47 2014  
Components/Type: Platform>DevTools/Bug  
Labels: ["Hotlist-Polish"]  
Code Review: [https://codereview.chromium.org/701093003](https://codereview.chromium.org/701093003)  
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
1 + reference_error //@ sourceURL=evaltest
})
*/
test("3:5", new Function(
    '1 + reference_error //@ sourceURL=evaltest'));
/*
(function(x
) {

 1 + reference_error //@ sourceURL=evaltest
})
*/
test("4:6", new Function(
    'x', '\n 1 + reference_error //@ sourceURL=evaltest'));
/*
(function(x

,z//
,y
) {

 1 + reference_error //@ sourceURL=evaltest
})
*/
test("7:6", new Function(
    'x\n\n', "z//\n", "y", '\n 1 + reference_error //@ sourceURL=evaltest'));
/*
(function(x/\*,z//
,y*\/
) {
1 + reference_error //@ sourceURL=evaltest
})
*/
test("4:5", new Function(
    'x/*', "z//\n", "y*/", '1 + reference_error //@ sourceURL=evaltest'));
/*
(function () {
 1 + reference_error //@ sourceURL=evaltest5
})
*/
test("2:6", eval(
    '(function () {\n 1 + reference_error //@ sourceURL=evaltest\n})'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1dbd636^!)  
[src/cpu-profiler.cc](https://cs.chromium.org/chromium/src/v8/src/cpu-profiler.cc?cl=1dbd636)  
[src/generator.js](https://cs.chromium.org/chromium/src/v8/src/generator.js?cl=1dbd636)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=1dbd636)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1dbd636)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=1dbd636)  
...  
  

---   

## **regress-crbug-431602.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::RootMarkingVisitor::MarkObjectByPointer](https://crbug.com/431602)**  
**[Commit: Fix has_constant_parameter_count() confusion in LReturn](https://chromium.googlesource.com/v8/v8/+/d3b68cf)**  
  
Date(Commit): Mon Nov 10 15:25:50 2014  
Components/Type: None/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "Stability-Memory-AddressSanitizer", "M-41", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/714663002](https://codereview.chromium.org/714663002)  
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
...  
  

---   

## **regress-register-allocator3.js (other issue)**  
   
**[Commit: [turbofan] phis cannot take registers as inputs](https://chromium.googlesource.com/v8/v8/+/a350f0d)**  
  
Date(Commit): Thu Nov 06 12:56:44 2014  
Code Review: [https://codereview.chromium.org/704153002](https://codereview.chromium.org/704153002)  
Regress: [mjsunit/compiler/regress-register-allocator3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator3.js)  
```javascript
function Module() {
  "use asm";
  function f() {
   var $0 = 0, $25 = 0, $i$014$i = 0, $sum$013$i = 0, $v_0$01$i = 0, $v_1$02$i = 0, $v_10$011$i = 0, $v_11$012$i = 0, $v_2$03$i = 0, $v_3$04$i = 0, $v_4$05$i = 0, $v_5$06$i = 0, $v_6$07$i = 0, $v_7$08$i = 0, $v_8$09$i = 0, $v_9$010$i = 0;
   $i$014$i = 0;
   $sum$013$i = 0;
   $v_0$01$i = 8;
   $v_1$02$i = 9;
   $v_10$011$i = 18;
   $v_11$012$i = 19;
   $v_2$03$i = 10;
   $v_3$04$i = 11;
   $v_4$05$i = 12;
   $v_5$06$i = 13;
   $v_6$07$i = 14;
   $v_7$08$i = 15;
   $v_8$09$i = 16;
   $v_9$010$i = 17;
   do {
    $v_0$01$i = $v_3$04$i + $v_9$010$i + $v_0$01$i | 0;
    $v_1$02$i = $v_4$05$i + $v_10$011$i + $v_1$02$i | 0;
    $v_2$03$i = $v_5$06$i + $v_11$012$i + $v_2$03$i | 0;
    $v_3$04$i = $v_3$04$i + $v_6$07$i + $v_0$01$i | 0;
    $v_4$05$i = $v_4$05$i + $v_7$08$i + $v_1$02$i | 0;
    $v_5$06$i = $v_5$06$i + $v_8$09$i + $v_2$03$i | 0;
    $v_6$07$i = $v_6$07$i + $v_9$010$i + $v_3$04$i | 0;
    $v_7$08$i = $v_7$08$i + $v_10$011$i + $v_4$05$i | 0;
    $v_8$09$i = $v_8$09$i + $v_11$012$i + $v_5$06$i | 0;
    $v_9$010$i = $v_0$01$i + $v_9$010$i + $v_6$07$i | 0;
    $v_10$011$i = $v_1$02$i + $v_10$011$i + $v_7$08$i | 0;
    $v_11$012$i = $v_2$03$i + $v_11$012$i + $v_8$09$i | 0;
    $25 = $v_0$01$i + $v_1$02$i | 0;
    $sum$013$i = $v_2$03$i + $sum$013$i + $v_5$06$i + $v_4$05$i + $v_8$09$i + $v_3$04$i + $25 + $v_7$08$i + $v_11$012$i + $v_6$07$i + $v_10$011$i + $v_9$010$i | 0;
    $i$014$i = $i$014$i + 1 | 0;
   } while (($i$014$i | 0) <= 0);
   return $sum$013$i - ($v_5$06$i + $v_2$03$i + $v_4$05$i + $v_8$09$i + $25 + $v_3$04$i + $v_7$08$i + $v_11$012$i + $v_6$07$i + $v_10$011$i + $v_9$010$i);
  }
  return { f: f };
}

Module().f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a350f0d^!)  
[src/compiler/register-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/register-allocator.cc?cl=a350f0d)  
[test/mjsunit/compiler/regress-register-allocator3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-register-allocator3.js?cl=a350f0d)  
[test/unittests/compiler/register-allocator-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/register-allocator-unittest.cc?cl=a350f0d)  
  
  
---   

## **regress-crbug-430846.js (chromium issue)**  
   
**[Issue: Hangouts fails on Linux Debug after V8 roll.](https://crbug.com/430846)**  
**[Commit: Fix for an assertion failure in Map::FindTransitionToField(...). Appeared after r25136.](https://chromium.googlesource.com/v8/v8/+/e1f93a8)**  
  
Date(Commit): Thu Nov 06 11:50:33 2014  
Components/Type: Blink/Bug  
Labels: ["M-40"]  
Code Review: [https://codereview.chromium.org/704183002](https://codereview.chromium.org/704183002)  
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
  

---   

## **regress-assignment-in-test-context.js (other issue)**  
   
**[Commit: [turbofan] Fix deopt for assignments in non-effect context.](https://chromium.googlesource.com/v8/v8/+/91eeae5)**  
  
Date(Commit): Wed Nov 05 13:09:14 2014  
Code Review: [https://codereview.chromium.org/701853002](https://codereview.chromium.org/701853002)  
Regress: [mjsunit/regress/regress-assignment-in-test-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-assignment-in-test-context.js)  
```javascript
function assertEquals() {}

function f(o) {
  if (o.setterProperty = 0) {
    return 1;
  }
  return 2;
}

function deopt() { %DeoptimizeFunction(f); }

assertEquals(2,
             f(Object.defineProperty({}, "setterProperty", { set: deopt })));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91eeae5^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=91eeae5)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=91eeae5)  
[test/mjsunit/regress/regress-assignment-in-test-context.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-assignment-in-test-context.js?cl=91eeae5)  
  
  
---   

## **regress-3483.js (v8 issue)**  
   
**[Issue: `1..isPrototypeOf.call(null)` should return false, not throw TypeError.](https://crbug.com/v8/3483)**  
**[Commit: `1..isPrototypeOf.call(null)` should return false, not throw TypeError.](https://chromium.googlesource.com/v8/v8/+/357882a)**  
  
Date(Commit): Tue Nov 04 16:14:18 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/433413002](https://codereview.chromium.org/433413002)  
Regress: [mjsunit/regress/regress-3483.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3483.js)  
```javascript
assertFalse(Object.prototype.isPrototypeOf.call());
assertFalse(Object.prototype.isPrototypeOf.call(null, 1));
assertFalse(Object.prototype.isPrototypeOf.call(undefined, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/357882a^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=357882a)  
[test/mjsunit/function-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/function-call.js?cl=357882a)  
[test/mjsunit/regress/regress-3483.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3483.js?cl=357882a)  
  

---   

## **regress-register-allocator2.js (other issue)**  
   
**[Commit: [x86] Fix register constraints for multiply high and modulus.](https://chromium.googlesource.com/v8/v8/+/017c518)**  
  
Date(Commit): Mon Nov 03 06:28:12 2014  
Code Review: [https://codereview.chromium.org/697053002](https://codereview.chromium.org/697053002)  
Regress: [mjsunit/compiler/regress-register-allocator2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator2.js)  
```javascript
function f() {
  var x = 0;
  var y = 0;
  x ^= undefined;
  assertEquals(x /= 1);
  assertEquals(NaN, y %= 1);
  assertEquals(y = 1);
  f();
  y = -2;
  assertEquals(x >>= 1);
  assertEquals(0, ((y+(y+(y+((y^(x%5))+y)))+(y+y))>>y)+y);
}
try { f(); } catch (e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/017c518^!)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=017c518)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=017c518)  
[test/mjsunit/compiler/regress-register-allocator2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-register-allocator2.js?cl=017c518)  
  
  
---   

## **regress-crbug-429159.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(stack_height() > 0) failed: .././src/compiler/ast-graph-builder.h(252](https://crbug.com/429159)**  
**[Commit: Properly handle stack overflows in the AST graph builder.](https://chromium.googlesource.com/v8/v8/+/cd3273b)**  
  
Date(Commit): Fri Oct 31 14:02:46 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/697473006](https://codereview.chromium.org/697473006)  
Regress: [mjsunit/regress/regress-crbug-429159.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-429159.js)  
```javascript
try {
  var src = "return " + Array(12000).join("src,") + "src";
  var fun = Function(src);
  assertEquals(src, fun());
} catch (e) {
  // Some architectures throw a RangeError, that is fine.
  assertInstanceof(e, RangeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd3273b^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=cd3273b)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=cd3273b)  
[src/compiler/pipeline.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/pipeline.cc?cl=cd3273b)  
[test/mjsunit/regress/regress-crbug-429159.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-429159.js?cl=cd3273b)  
  

---   

## **regress-eval-cache.js (other issue)**  
   
**[Commit: Use shared function info for eval cache key.](https://chromium.googlesource.com/v8/v8/+/0dfbf83)**  
  
Date(Commit): Tue Oct 28 10:01:44 2014  
Code Review: [https://codereview.chromium.org/678843004](https://codereview.chromium.org/678843004)  
Regress: [mjsunit/regress/regress-eval-cache.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-eval-cache.js)  
```javascript
(function f() {
  try {
    throw 1;
  } catch (e) {
    var a = 0;
    var b = 0;
    var c = 0;
    var x = 1;
    var result = eval('eval("x")').toString();
    assertEquals("1", result);
  }
  var x = 2;
  var result = eval('eval("x")').toString();
  assertEquals("2", result);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0dfbf83^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=0dfbf83)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=0dfbf83)  
[src/compilation-cache.cc](https://cs.chromium.org/chromium/src/v8/src/compilation-cache.cc?cl=0dfbf83)  
[src/compilation-cache.h](https://cs.chromium.org/chromium/src/v8/src/compilation-cache.h?cl=0dfbf83)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=0dfbf83)  
...  
  
  
---   

## **regress-3643.js (v8 issue)**  
   
**[Issue: Array.prototype.slice calls [[Get]] before [[Has]] when generating result array](https://crbug.com/v8/3643)**  
**[Commit: SimpleSlice now calls [[Get]] before [[Has]] when generating copy](https://chromium.googlesource.com/v8/v8/+/c9ea8d6)**  
  
Date(Commit): Fri Oct 24 18:08:13 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/674003002](https://codereview.chromium.org/674003002)  
Regress: [mjsunit/regress/regress-3643.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3643.js)  
```javascript
function newArrayWithGetter() {
  var arr = [1, 2, 3];
  Object.defineProperty(arr, '1', {
    get: function() { delete this[1]; return undefined; },
    configurable: true
  });
  return arr;
}

var a = newArrayWithGetter();
var s = a.slice(1);
assertTrue('0' in s);

a = newArrayWithGetter();
a[0xffff] = 4;
s = a.slice(1);
assertTrue('0' in s);

a = newArrayWithGetter();
a.shift();
assertTrue('0' in a);

a = newArrayWithGetter();
a.unshift(0);
assertTrue('2' in a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c9ea8d6^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=c9ea8d6)  
[test/mjsunit/regress/regress-3643.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3643.js?cl=c9ea8d6)  
  

---   

## **regress-shift-enumerable.js (other issue)**  
   
**[Commit: Widen definition of %HasComplexElements() to include non-enumerability](https://chromium.googlesource.com/v8/v8/+/02d37b8)**  
  
Date(Commit): Fri Oct 24 18:04:13 2014  
Code Review: [https://codereview.chromium.org/672323003](https://codereview.chromium.org/672323003)  
Regress: [mjsunit/regress/regress-shift-enumerable.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-shift-enumerable.js)  
```javascript
var arr = [1, 2];
Object.defineProperty(arr, 0xfffe, {
  value: 3,
  configurable: true,
  writable: true,
  enumerable: false
});
arr[0xffff] = 4;
arr.shift();
var desc = Object.getOwnPropertyDescriptor(arr, 0xfffe);
assertEquals(4, desc.value);
assertFalse(desc.enumerable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/02d37b8^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=02d37b8)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=02d37b8)  
[test/mjsunit/regress/regress-shift-enumerable.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-shift-enumerable.js?cl=02d37b8)  
  
  
---   

## **regress-register-allocator.js (other issue)**  
   
**[Commit: [x86] Fix register constraints for multiply-high.](https://chromium.googlesource.com/v8/v8/+/548fb46)**  
  
Date(Commit): Fri Oct 24 09:36:40 2014  
Code Review: [https://codereview.chromium.org/671393002](https://codereview.chromium.org/671393002)  
Regress: [mjsunit/compiler/regress-register-allocator.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-register-allocator.js)  
```javascript
function Module(stdlib, foreign, buffer) {
  "use asm";
  var HEAP32 = new stdlib.Int32Array(buffer);
  function g(a) {
    HEAP32[a] = 9982 * 100;
    return a;
  }
  function f(i1) {
    i1 = i1 | 0;
    var i2 = HEAP32[i1 >> 2] | 0;
    g(i1);
    L2909: {
      L2: {
        if (0) {
          if (0) break L2;
          g(i2);
          break L2909;
        }
      }
      var r = (HEAP32[1] | 0) / 100 | 0;
      g(r);
      return r;
    }
  }
  return {f: f};
}

var f = Module(this, {}, new ArrayBuffer(64 * 1024)).f;
assertEquals(9982, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/548fb46^!)  
[src/compiler/ia32/instruction-selector-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/instruction-selector-ia32.cc?cl=548fb46)  
[src/compiler/instruction.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction.h?cl=548fb46)  
[src/compiler/x64/instruction-selector-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/instruction-selector-x64.cc?cl=548fb46)  
[test/mjsunit/compiler/regress-register-allocator.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-register-allocator.js?cl=548fb46)  
[test/unittests/compiler/ia32/instruction-selector-ia32-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/ia32/instruction-selector-ia32-unittest.cc?cl=548fb46)  
...  
  
  
---   

## **regress-crbug-423687.js (chromium issue)**  
   
**[Issue: JSON.parse sometimes crashes chrome 38 ('Aw, snap')](https://crbug.com/423687)**  
**[Commit: Fixed mutable heap numbers leak in JSON parser.](https://chromium.googlesource.com/v8/v8/+/5509cc2)**  
  
Date(Commit): Thu Oct 23 14:41:39 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["merge-merged-3.29", "merge-merged-3.28", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/669403002](https://codereview.chromium.org/669403002)  
Regress: [mjsunit/regress/regress-crbug-423687.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-423687.js)  
```javascript
var json = '{"a":{"c":2.1,"d":0},"b":{"c":7,"1024":8}}';
var data = JSON.parse(json);

data.b.c++;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5509cc2^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=5509cc2)  
[test/mjsunit/regress/regress-crbug-423687.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-423687.js?cl=5509cc2)  
  

---   

## **regress-2506.js (v8 issue)**  
   
**[Issue: Missing support for const in for-in loops](https://crbug.com/v8/2506)**  
**[Commit: harmony-scoping: Allow 'const' iteration variables in strict mode.](https://chromium.googlesource.com/v8/v8/+/b54f7d3)**  
  
Date(Commit): Thu Oct 23 11:18:50 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/671913002](https://codereview.chromium.org/671913002)  
Regress: [mjsunit/es6/regress/regress-2506.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2506.js)  
```javascript
'use strict';

let s = 0;
let f = [undefined, undefined, undefined]
for (const x of [1,2,3]) {
  s += x;
  f[x-1] = function() { return x; }
}
assertEquals(6, s);
assertEquals(1, f[0]());
assertEquals(2, f[1]());
assertEquals(3, f[2]());

let x = 1;
s = 0;
for (const z of [x, x+1, x+2]) {
  s += z;
}
assertEquals(6, s);

s = 0;
var q = 1;
for (const x of [q, q+1, q+2]) {
  s += x;
}
assertEquals(6, s);

let z = 1;
s = 0;
for (const x = 1; z < 2; z++) {
  s += x + z;
}
assertEquals(2, s);


s = "";
for (const x in [1,2,3]) {
  s += x;
}
assertEquals("012", s);

assertThrows("'use strict'; for (const x in [1,2,3]) { x++ }", TypeError);

(function() {
  let s = 0;
  for (const x of [1,2,3]) {
    s += x;
  }
  assertEquals(6, s);

  let x = 1;
  s = 0;
  for (const q of [x, x+1, x+2]) {
    s += q;
  }
  assertEquals(6, s);

  s = 0;
  var q = 1;
  for (const x of [q, q+1, q+2]) {
    s += x;
  }
  assertEquals(6, s);

  s = "";
  for (const x in [1,2,3]) {
    s += x;
  }
  assertEquals("012", s);
}());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b54f7d3^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=b54f7d3)  
[src/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/preparser.cc?cl=b54f7d3)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=b54f7d3)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=b54f7d3)  
[test/mjsunit/regress/regress-2506.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2506.js?cl=b54f7d3)  
  

---   

## **regress-crbug-425585.js (chromium issue)**  
   
**[Issue: Use-of-uninitialized-value in v8::internal::Decoder<v8::internal::Simulator>::DecodeBranchSystemException](https://crbug.com/425585)**  
**[Commit: ARM64: Fix stack manipulation.](https://chromium.googlesource.com/v8/v8/+/ecbfc43)**  
  
Date(Commit): Wed Oct 22 18:24:20 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "reward-2500", "External-Fuzzer-Contribution", "M-40", "Security_Impact-Head", "Stability-Memory-MemorySanitizer", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/672623003](https://codereview.chromium.org/672623003)  
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
  

---   

## **regress-425551.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK_EQ(TWO_BYTE, state_) failed: ../../v8/src/objects.h(8728)](https://crbug.com/425551)**  
**[Commit: Flatten the string in StringToDouble function.](https://chromium.googlesource.com/v8/v8/+/b664c12)**  
  
Date(Commit): Wed Oct 22 08:19:05 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/654763003](https://codereview.chromium.org/654763003)  
Regress: [mjsunit/regress/regress-425551.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-425551.js)  
```javascript
var array = new Int8Array(10);
array[/\u007d\u00fc\u0043/] = 1.499
assertEquals(1.499, array[/\u007d\u00fc\u0043/]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b664c12^!)  
[src/conversions.cc](https://cs.chromium.org/chromium/src/v8/src/conversions.cc?cl=b664c12)  
[src/conversions.h](https://cs.chromium.org/chromium/src/v8/src/conversions.h?cl=b664c12)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=b664c12)  
[src/runtime/runtime-numbers.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-numbers.cc?cl=b664c12)  
[test/mjsunit/regress/regress-425551.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-425551.js?cl=b664c12)  
  

---   

## **regress-423633.js (chromium issue)**  
   
**[Issue: Altering Array prototype breaks slice and splice](https://crbug.com/423633)**  
**[Commit: Array.prototype.{slice,splice} should use [[DefineOwnProperty]] to generate return value](https://chromium.googlesource.com/v8/v8/+/b6d0113)**  
  
Date(Commit): Tue Oct 21 17:46:42 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/649063003](https://codereview.chromium.org/649063003)  
Regress: [mjsunit/regress/regress-423633.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-423633.js)  
```javascript
Object.defineProperty(Array.prototype, '0', {
  get: function() { return false; },
});
var a = [1, 2, 3];
assertEquals(a, a.slice());
assertEquals([3], a.splice(2, 1));

a = [1, 2, 3];
a[0xffff] = 4;
a.__proto__ = null;
assertEquals(a, Array.prototype.slice.call(a));
assertEquals([3], Array.prototype.splice.call(a, 2, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b6d0113^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=b6d0113)  
[test/mjsunit/regress/regress-423633.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-423633.js?cl=b6d0113)  
  

---   

## **regress-crbug-425519.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HEnvironment::Pop](https://crbug.com/425519)**  
**[Commit: The issue is that by handling strings with map/handler pairs instead of a special](https://chromium.googlesource.com/v8/v8/+/8330178)**  
  
Date(Commit): Tue Oct 21 13:04:51 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/667923004](https://codereview.chromium.org/667923004)  
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
  

---   

## **regress-2615.js (v8 issue)**  
   
**[Issue: Non-compliant behavior in array functions at MaxUInt32 boundary](https://crbug.com/v8/2615)**  
**[Commit: Remove SmartMove, bringing Array methods further into spec compliance](https://chromium.googlesource.com/v8/v8/+/bb885a7)**  
  
Date(Commit): Wed Oct 15 23:36:58 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/349073002](https://codereview.chromium.org/349073002)  
Regress: [mjsunit/regress/regress-2615.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2615.js)  
```javascript
a = [1];
Object.defineProperty(a, "1", {writable:false, configurable:false, value: 100});
assertThrows("a.unshift(4);", TypeError);
assertEquals([1, 100, 100], a);
var desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.writable);
assertEquals(false, desc.configurable);

a = [1];
var g = function() { return 100; };
Object.defineProperty(a, "1", {get:g});
assertThrows("a.unshift(0);", TypeError);
assertEquals([1, 100, 100], a);
desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.configurable);
assertEquals(g, desc.get);

a = [1];
var c = 0;
var s = function(v) { c += 1; };
Object.defineProperty(a, "1", {set:s});
a.unshift(10);
assertEquals([10, undefined, undefined], a);
assertEquals(1, c);
desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.configurable);
assertEquals(s, desc.set);

a = [1];
Object.defineProperty(a, "1", {configurable:false, value:10});
assertThrows("a.splice(1,1);", TypeError);
assertEquals([1, 10], a);
desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.configurable);

a = [0,1,2,3,4,5,6];
Object.defineProperty(a, "3", {configurable:false, writable:false, value:3});
assertThrows("a.splice(1,4);", TypeError);
assertEquals([0,5,6,3,,,,], a);
desc = Object.getOwnPropertyDescriptor(a, "3");
assertEquals(false, desc.configurable);
assertEquals(false, desc.writable);

a = [0,1,2,3,4,5,6];
Object.defineProperty(a, "5", {configurable:false, value:5});
assertThrows("a.splice(1,4);", TypeError);
assertEquals([0,5,6,3,4,5,,], a);
desc = Object.getOwnPropertyDescriptor(a, "5");
assertEquals(false, desc.configurable);

a = [1,2,3,,5];
Object.defineProperty(a, "1", {configurable:false, writable:true, value:2});
assertEquals(1, a.shift());
assertEquals([2,3,,5], a);
desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.configurable);
assertEquals(true, desc.writable);
assertThrows("a.shift();", TypeError);
assertEquals([3,3,,5], a);
desc = Object.getOwnPropertyDescriptor(a, "1");
assertEquals(false, desc.configurable);
assertEquals(true, desc.writable);

a = [1,2,3];
Object.defineProperty(a, "2", {configurable:false, value:3});
assertThrows("a.pop();", TypeError);
assertEquals([1,2,3], a);
desc = Object.getOwnPropertyDescriptor(a, "2");
assertEquals(false, desc.configurable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb885a7^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=bb885a7)  
[test/mjsunit/array-functions-prototype-misc.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-functions-prototype-misc.js?cl=bb885a7)  
[test/mjsunit/array-natives-elements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-natives-elements.js?cl=bb885a7)  
[test/mjsunit/array-shift2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-shift2.js?cl=bb885a7)  
[test/mjsunit/array-unshift.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/array-unshift.js?cl=bb885a7)  
...  
  

---   

## **regress-385565.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/385565)**  
**[Commit: Optimize Function.prototype.call](https://chromium.googlesource.com/v8/v8/+/23868b4)**  
  
Date(Commit): Wed Oct 15 12:22:15 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/588573002](https://codereview.chromium.org/588573002)  
Regress: [mjsunit/regress/regress-385565.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-385565.js)  
```javascript
var calls = 0;

function callsFReceiver(o) {
    return [].f.call(new Number(o.m), 1, 2, 3);
}

Array.prototype.f = function() {
    calls++;
    return +this;
};


var o1 = {m: 1};
var o2 = {a: 0, m:1};

var r1 = callsFReceiver(o1);
callsFReceiver(o1);
%OptimizeFunctionOnNextCall(callsFReceiver);
var r2 = callsFReceiver(o1);
assertOptimized(callsFReceiver);
callsFReceiver(o2);
assertUnoptimized(callsFReceiver);
var r3 = callsFReceiver(o1);

assertEquals(1, r1);
assertTrue(r1 === r2);
assertTrue(r2 === r3);

r1 = callsFReceiver(o1);
callsFReceiver(o1);
%OptimizeFunctionOnNextCall(callsFReceiver);
r2 = callsFReceiver(o1);
callsFReceiver(o2);
r3 = callsFReceiver(o1);

assertEquals(1, r1);
assertTrue(r1 === r2);
assertTrue(r2 === r3);

assertEquals(10, calls);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/23868b4^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=23868b4)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=23868b4)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=23868b4)  
[test/mjsunit/compiler/deopt-inlined-from-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/deopt-inlined-from-call.js?cl=23868b4)  
[test/mjsunit/compiler/inlined-call-mapcheck.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/inlined-call-mapcheck.js?cl=23868b4)  
...  
  

---   

## **regress-3621.js (v8 issue)**  
   
**[Issue: Sparse variants of Array.prototype.join does not handle side-effects due to getters](https://crbug.com/v8/3621)**  
**[Commit: Add test case for SparseJoin misbehavior with getters](https://chromium.googlesource.com/v8/v8/+/9595c61)**  
  
Date(Commit): Fri Oct 10 17:17:00 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/645703003](https://codereview.chromium.org/645703003)  
Regress: [mjsunit/regress/regress-3621.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3621.js)  
```javascript
var a = [];
var endIndex = 0xffff;
a[endIndex] = 3;
Object.defineProperty(a, 0, { get: function() { this[1] = 2; return 1; } });
assertEquals('123', a.join(''));
delete a[1];  // reset the array
assertEquals('1,2,', a.join().slice(0, 4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9595c61^!)  
[test/mjsunit/bugs/bug-3621.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-3621.js?cl=9595c61)  
  

---   

## **regress-3116.js (v8 issue)**  
   
**[Issue: Date behavior incorrect in Sao Paulo on Sat Oct 18 2014 00:00:00 GMT-0300](https://crbug.com/v8/3116)**  
**[Commit: Fix computation of UTC time from local time at DST change points.](https://chromium.googlesource.com/v8/v8/+/29296d7)**  
  
Date(Commit): Thu Oct 09 14:17:33 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/639383002](https://codereview.chromium.org/639383002)  
Regress: [mjsunit/regress/regress-3116.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3116.js)  
```javascript
function timezone(tz) {
  var str = (new Date(2014, 0, 10)).toString();
  if (tz == "CET") {
    return str == "Fri Jan 10 2014 00:00:00 GMT+0100 (CET)";
  }
  if (tz == "BRT") {
    return str == "Fri Jan 10 2014 00:00:00 GMT-0200 (BRST)";
  }
  if (tz == "PST") {
    return str == "Fri Jan 10 2014 00:00:00 GMT-0800 (PST)";
  }
  return false;
}

if (timezone("CET")) {
  assertEquals("Sat Mar 29 2014 22:59:00 GMT+0100 (CET)",
               (new Date(2014, 2, 29, 22, 59)).toString());
  assertEquals("Sat, 29 Mar 2014 21:59:00 GMT",
               (new Date(2014, 2, 29, 22, 59)).toUTCString());
  assertEquals("Sat Mar 29 2014 23:00:00 GMT+0100 (CET)",
               (new Date(2014, 2, 29, 23, 0)).toString());
  assertEquals("Sat, 29 Mar 2014 22:00:00 GMT",
               (new Date(2014, 2, 29, 23, 0)).toUTCString());
  assertEquals("Sat Mar 29 2014 23:59:00 GMT+0100 (CET)",
               (new Date(2014, 2, 29, 23, 59)).toString());
  assertEquals("Sat, 29 Mar 2014 22:59:00 GMT",
               (new Date(2014, 2, 29, 23, 59)).toUTCString());
  assertEquals("Sun Mar 30 2014 00:00:00 GMT+0100 (CET)",
               (new Date(2014, 2, 30, 0, 0)).toString());
  assertEquals("Sat, 29 Mar 2014 23:00:00 GMT",
               (new Date(2014, 2, 30, 0, 0)).toUTCString());
  assertEquals("Sun Mar 30 2014 00:59:00 GMT+0100 (CET)",
               (new Date(2014, 2, 30, 0, 59)).toString());
  assertEquals("Sat, 29 Mar 2014 23:59:00 GMT",
               (new Date(2014, 2, 30, 0, 59)).toUTCString());
  assertEquals("Sun Mar 30 2014 01:00:00 GMT+0100 (CET)",
               (new Date(2014, 2, 30, 1, 0)).toString());
  assertEquals("Sun, 30 Mar 2014 00:00:00 GMT",
               (new Date(2014, 2, 30, 1, 0)).toUTCString());
  assertEquals("Sun Mar 30 2014 01:59:00 GMT+0100 (CET)",
               (new Date(2014, 2, 30, 1, 59)).toString());
  assertEquals("Sun, 30 Mar 2014 00:59:00 GMT",
               (new Date(2014, 2, 30, 1, 59)).toUTCString());
  assertEquals("Sun Mar 30 2014 03:00:00 GMT+0200 (CEST)",
               (new Date(2014, 2, 30, 2, 0)).toString());
  assertEquals("Sun, 30 Mar 2014 01:00:00 GMT",
               (new Date(2014, 2, 30, 2, 0)).toUTCString());
  assertEquals("Sun Mar 30 2014 03:59:00 GMT+0200 (CEST)",
               (new Date(2014, 2, 30, 2, 59)).toString());
  assertEquals("Sun, 30 Mar 2014 01:59:00 GMT",
               (new Date(2014, 2, 30, 2, 59)).toUTCString());
  assertEquals("Sun Mar 30 2014 03:00:00 GMT+0200 (CEST)",
               (new Date(2014, 2, 30, 3, 0)).toString());
  assertEquals("Sun, 30 Mar 2014 01:00:00 GMT",
               (new Date(2014, 2, 30, 3, 0)).toUTCString());
  assertEquals("Sun Mar 30 2014 03:59:00 GMT+0200 (CEST)",
               (new Date(2014, 2, 30, 3, 59)).toString());
  assertEquals("Sun, 30 Mar 2014 01:59:00 GMT",
               (new Date(2014, 2, 30, 3, 59)).toUTCString());
  assertEquals("Sun Mar 30 2014 04:00:00 GMT+0200 (CEST)",
               (new Date(2014, 2, 30, 4, 0)).toString());
  assertEquals("Sun, 30 Mar 2014 02:00:00 GMT",
               (new Date(2014, 2, 30, 4, 0)).toUTCString());
  assertEquals("Sat Oct 25 2014 22:59:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 25, 22, 59)).toString());
  assertEquals("Sat, 25 Oct 2014 20:59:00 GMT",
               (new Date(2014, 9, 25, 22, 59)).toUTCString());
  assertEquals("Sat Oct 25 2014 23:00:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 25, 23, 0)).toString());
  assertEquals("Sat, 25 Oct 2014 21:00:00 GMT",
               (new Date(2014, 9, 25, 23, 0)).toUTCString());
  assertEquals("Sat Oct 25 2014 23:59:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 25, 23, 59)).toString());
  assertEquals("Sat, 25 Oct 2014 21:59:00 GMT",
               (new Date(2014, 9, 25, 23, 59)).toUTCString());
  assertEquals("Sun Oct 26 2014 00:00:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 0, 0)).toString());
  assertEquals("Sat, 25 Oct 2014 22:00:00 GMT",
               (new Date(2014, 9, 26, 0, 0)).toUTCString());
  assertEquals("Sun Oct 26 2014 00:59:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 0, 59)).toString());
  assertEquals("Sat, 25 Oct 2014 22:59:00 GMT",
               (new Date(2014, 9, 26, 0, 59)).toUTCString());
  assertEquals("Sun Oct 26 2014 01:00:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 1, 0)).toString());
  assertEquals("Sat, 25 Oct 2014 23:00:00 GMT",
               (new Date(2014, 9, 26, 1, 0)).toUTCString());
  assertEquals("Sun Oct 26 2014 01:59:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 1, 59)).toString());
  assertEquals("Sat, 25 Oct 2014 23:59:00 GMT",
               (new Date(2014, 9, 26, 1, 59)).toUTCString());
  assertEquals("Sun Oct 26 2014 02:00:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 2, 0)).toString());
  assertEquals("Sun, 26 Oct 2014 00:00:00 GMT",
               (new Date(2014, 9, 26, 2, 0)).toUTCString());
  assertEquals("Sun Oct 26 2014 02:59:00 GMT+0200 (CEST)",
               (new Date(2014, 9, 26, 2, 59)).toString());
  assertEquals("Sun, 26 Oct 2014 00:59:00 GMT",
               (new Date(2014, 9, 26, 2, 59)).toUTCString());
  assertEquals("Sun Oct 26 2014 03:00:00 GMT+0100 (CET)",
               (new Date(2014, 9, 26, 3, 0)).toString());
  assertEquals("Sun, 26 Oct 2014 02:00:00 GMT",
               (new Date(2014, 9, 26, 3, 0)).toUTCString());
  assertEquals("Sun Oct 26 2014 03:59:00 GMT+0100 (CET)",
               (new Date(2014, 9, 26, 3, 59)).toString());
  assertEquals("Sun, 26 Oct 2014 02:59:00 GMT",
               (new Date(2014, 9, 26, 3, 59)).toUTCString());
  assertEquals("Sun Oct 26 2014 04:00:00 GMT+0100 (CET)",
               (new Date(2014, 9, 26, 4, 0)).toString());
  assertEquals("Sun, 26 Oct 2014 03:00:00 GMT",
               (new Date(2014, 9, 26, 4, 0)).toUTCString());
}

if (timezone("BRT")) {
  assertEquals("Sat Oct 18 2014 22:59:00 GMT-0300 (BRT)",
               (new Date(2014, 9, 18, 22, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 01:59:00 GMT",
               (new Date(2014, 9, 18, 22, 59)).toUTCString());
  assertEquals("Sat Oct 18 2014 23:00:00 GMT-0300 (BRT)",
               (new Date(2014, 9, 18, 23, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 02:00:00 GMT",
               (new Date(2014, 9, 18, 23, 0)).toUTCString());
  assertEquals("Sat Oct 18 2014 23:59:00 GMT-0300 (BRT)",
               (new Date(2014, 9, 18, 23, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 02:59:00 GMT",
               (new Date(2014, 9, 18, 23, 59)).toUTCString());
  assertEquals("Sun Oct 19 2014 01:00:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 0, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 03:00:00 GMT",
               (new Date(2014, 9, 19, 0, 0)).toUTCString());
  assertEquals("Sun Oct 19 2014 01:59:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 0, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 03:59:00 GMT",
               (new Date(2014, 9, 19, 0, 59)).toUTCString());
  assertEquals("Sun Oct 19 2014 01:00:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 1, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 03:00:00 GMT",
               (new Date(2014, 9, 19, 1, 0)).toUTCString());
  assertEquals("Sun Oct 19 2014 01:59:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 1, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 03:59:00 GMT",
               (new Date(2014, 9, 19, 1, 59)).toUTCString());
  assertEquals("Sun Oct 19 2014 02:00:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 2, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 04:00:00 GMT",
               (new Date(2014, 9, 19, 2, 0)).toUTCString());
  assertEquals("Sun Oct 19 2014 02:59:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 2, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 04:59:00 GMT",
               (new Date(2014, 9, 19, 2, 59)).toUTCString());
  assertEquals("Sun Oct 19 2014 03:00:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 3, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 05:00:00 GMT",
               (new Date(2014, 9, 19, 3, 0)).toUTCString());
  assertEquals("Sun Oct 19 2014 03:59:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 3, 59)).toString());
  assertEquals("Sun, 19 Oct 2014 05:59:00 GMT",
               (new Date(2014, 9, 19, 3, 59)).toUTCString());
  assertEquals("Sun Oct 19 2014 04:00:00 GMT-0200 (BRST)",
               (new Date(2014, 9, 19, 4, 0)).toString());
  assertEquals("Sun, 19 Oct 2014 06:00:00 GMT",
               (new Date(2014, 9, 19, 4, 0)).toUTCString());
  assertEquals("Sat Feb 15 2014 22:59:00 GMT-0200 (BRST)",
               (new Date(2014, 1, 15, 22, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 00:59:00 GMT",
               (new Date(2014, 1, 15, 22, 59)).toUTCString());
  assertEquals("Sat Feb 15 2014 23:00:00 GMT-0200 (BRST)",
               (new Date(2014, 1, 15, 23, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 01:00:00 GMT",
               (new Date(2014, 1, 15, 23, 0)).toUTCString());
  assertEquals("Sat Feb 15 2014 23:59:00 GMT-0200 (BRST)",
               (new Date(2014, 1, 15, 23, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 01:59:00 GMT",
               (new Date(2014, 1, 15, 23, 59)).toUTCString());
  assertEquals("Sun Feb 16 2014 00:00:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 0, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 03:00:00 GMT",
               (new Date(2014, 1, 16, 0, 0)).toUTCString());
  assertEquals("Sun Feb 16 2014 00:59:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 0, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 03:59:00 GMT",
               (new Date(2014, 1, 16, 0, 59)).toUTCString());
  assertEquals("Sun Feb 16 2014 01:00:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 1, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 04:00:00 GMT",
               (new Date(2014, 1, 16, 1, 0)).toUTCString());
  assertEquals("Sun Feb 16 2014 01:59:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 1, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 04:59:00 GMT",
               (new Date(2014, 1, 16, 1, 59)).toUTCString());
  assertEquals("Sun Feb 16 2014 02:00:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 2, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 05:00:00 GMT",
               (new Date(2014, 1, 16, 2, 0)).toUTCString());
  assertEquals("Sun Feb 16 2014 02:59:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 2, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 05:59:00 GMT",
               (new Date(2014, 1, 16, 2, 59)).toUTCString());
  assertEquals("Sun Feb 16 2014 03:00:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 3, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 06:00:00 GMT",
               (new Date(2014, 1, 16, 3, 0)).toUTCString());
  assertEquals("Sun Feb 16 2014 03:59:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 3, 59)).toString());
  assertEquals("Sun, 16 Feb 2014 06:59:00 GMT",
               (new Date(2014, 1, 16, 3, 59)).toUTCString());
  assertEquals("Sun Feb 16 2014 04:00:00 GMT-0300 (BRT)",
               (new Date(2014, 1, 16, 4, 0)).toString());
  assertEquals("Sun, 16 Feb 2014 07:00:00 GMT",
               (new Date(2014, 1, 16, 4, 0)).toUTCString());
}

if (timezone("PST")) {
  assertEquals("Sat Mar 08 2014 22:59:00 GMT-0800 (PST)",
               (new Date(2014, 2, 8, 22, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 06:59:00 GMT",
               (new Date(2014, 2, 8, 22, 59)).toUTCString());
  assertEquals("Sat Mar 08 2014 23:00:00 GMT-0800 (PST)",
               (new Date(2014, 2, 8, 23, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 07:00:00 GMT",
               (new Date(2014, 2, 8, 23, 0)).toUTCString());
  assertEquals("Sat Mar 08 2014 23:59:00 GMT-0800 (PST)",
               (new Date(2014, 2, 8, 23, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 07:59:00 GMT",
               (new Date(2014, 2, 8, 23, 59)).toUTCString());
  assertEquals("Sun Mar 09 2014 00:00:00 GMT-0800 (PST)",
               (new Date(2014, 2, 9, 0, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 08:00:00 GMT",
               (new Date(2014, 2, 9, 0, 0)).toUTCString());
  assertEquals("Sun Mar 09 2014 00:59:00 GMT-0800 (PST)",
               (new Date(2014, 2, 9, 0, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 08:59:00 GMT",
               (new Date(2014, 2, 9, 0, 59)).toUTCString());
  assertEquals("Sun Mar 09 2014 01:00:00 GMT-0800 (PST)",
               (new Date(2014, 2, 9, 1, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 09:00:00 GMT",
               (new Date(2014, 2, 9, 1, 0)).toUTCString());
  assertEquals("Sun Mar 09 2014 01:59:00 GMT-0800 (PST)",
               (new Date(2014, 2, 9, 1, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 09:59:00 GMT",
               (new Date(2014, 2, 9, 1, 59)).toUTCString());
  assertEquals("Sun Mar 09 2014 03:00:00 GMT-0700 (PDT)",
               (new Date(2014, 2, 9, 2, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 10:00:00 GMT",
               (new Date(2014, 2, 9, 2, 0)).toUTCString());
  assertEquals("Sun Mar 09 2014 03:59:00 GMT-0700 (PDT)",
               (new Date(2014, 2, 9, 2, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 10:59:00 GMT",
               (new Date(2014, 2, 9, 2, 59)).toUTCString());
  assertEquals("Sun Mar 09 2014 03:00:00 GMT-0700 (PDT)",
               (new Date(2014, 2, 9, 3, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 10:00:00 GMT",
               (new Date(2014, 2, 9, 3, 0)).toUTCString());
  assertEquals("Sun Mar 09 2014 03:59:00 GMT-0700 (PDT)",
               (new Date(2014, 2, 9, 3, 59)).toString());
  assertEquals("Sun, 09 Mar 2014 10:59:00 GMT",
               (new Date(2014, 2, 9, 3, 59)).toUTCString());
  assertEquals("Sun Mar 09 2014 04:00:00 GMT-0700 (PDT)",
               (new Date(2014, 2, 9, 4, 0)).toString());
  assertEquals("Sun, 09 Mar 2014 11:00:00 GMT",
               (new Date(2014, 2, 9, 4, 0)).toUTCString());
  assertEquals("Sat Nov 01 2014 22:59:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 1, 22, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 05:59:00 GMT",
               (new Date(2014, 10, 1, 22, 59)).toUTCString());
  assertEquals("Sat Nov 01 2014 23:00:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 1, 23, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 06:00:00 GMT",
               (new Date(2014, 10, 1, 23, 0)).toUTCString());
  assertEquals("Sat Nov 01 2014 23:59:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 1, 23, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 06:59:00 GMT",
               (new Date(2014, 10, 1, 23, 59)).toUTCString());
  assertEquals("Sun Nov 02 2014 00:00:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 2, 0, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 07:00:00 GMT",
               (new Date(2014, 10, 2, 0, 0)).toUTCString());
  assertEquals("Sun Nov 02 2014 00:59:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 2, 0, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 07:59:00 GMT",
               (new Date(2014, 10, 2, 0, 59)).toUTCString());
  assertEquals("Sun Nov 02 2014 01:00:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 2, 1, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 08:00:00 GMT",
               (new Date(2014, 10, 2, 1, 0)).toUTCString());
  assertEquals("Sun Nov 02 2014 01:59:00 GMT-0700 (PDT)",
               (new Date(2014, 10, 2, 1, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 08:59:00 GMT",
               (new Date(2014, 10, 2, 1, 59)).toUTCString());
  assertEquals("Sun Nov 02 2014 02:00:00 GMT-0800 (PST)",
               (new Date(2014, 10, 2, 2, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 10:00:00 GMT",
               (new Date(2014, 10, 2, 2, 0)).toUTCString());
  assertEquals("Sun Nov 02 2014 02:59:00 GMT-0800 (PST)",
               (new Date(2014, 10, 2, 2, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 10:59:00 GMT",
               (new Date(2014, 10, 2, 2, 59)).toUTCString());
  assertEquals("Sun Nov 02 2014 03:00:00 GMT-0800 (PST)",
               (new Date(2014, 10, 2, 3, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 11:00:00 GMT",
               (new Date(2014, 10, 2, 3, 0)).toUTCString());
  assertEquals("Sun Nov 02 2014 03:59:00 GMT-0800 (PST)",
               (new Date(2014, 10, 2, 3, 59)).toString());
  assertEquals("Sun, 02 Nov 2014 11:59:00 GMT",
               (new Date(2014, 10, 2, 3, 59)).toUTCString());
  assertEquals("Sun Nov 02 2014 04:00:00 GMT-0800 (PST)",
               (new Date(2014, 10, 2, 4, 0)).toString());
  assertEquals("Sun, 02 Nov 2014 12:00:00 GMT",
               (new Date(2014, 10, 2, 4, 0)).toUTCString());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/29296d7^!)  
[src/date.h](https://cs.chromium.org/chromium/src/v8/src/date.h?cl=29296d7)  
[test/mjsunit/regress/regress-3116.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3116.js?cl=29296d7)  
  

---   

## **regress-3612.js (v8 issue)**  
   
**[Issue: Sparse variant of Array.prototype.reverse does not handle side-effects due to getters/setters](https://crbug.com/v8/3612)**  
**[Commit: Add test case demonstrating bug in SparseReverse when combined with getters/setters](https://chromium.googlesource.com/v8/v8/+/a0f80ed)**  
  
Date(Commit): Tue Oct 07 19:22:44 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/628383002](https://codereview.chromium.org/628383002)  
Regress: [mjsunit/regress/regress-3612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3612.js)  
```javascript
var a = [1];
var getterValue = 2;
var endIndex = 0xffff;
Object.defineProperty(a, endIndex, {
  get: function() {
    this[1] = 3;
    return getterValue;
  },
  set: function(val) {
    getterValue = val;
  },
  configurable: true,
  enumerable: true
});
a.reverse();
assertFalse(a.hasOwnProperty(1));
assertEquals(3, a[endIndex-1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a0f80ed^!)  
[test/mjsunit/bugs/bug-3612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-3612.js?cl=a0f80ed)  
  

---   

## **regress-crbug-417508.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/417508)**  
**[Commit: Fix Hydrogen's BuildStore()](https://chromium.googlesource.com/v8/v8/+/1bb52d0)**  
  
Date(Commit): Wed Oct 01 13:17:34 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/612423002](https://codereview.chromium.org/612423002)  
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
  

---   

## **regress-416730.js (chromium issue)**  
   
**[Issue: Aw Snap on WebGL site http://immense-escarpment-6268.herokuapp.com/](https://crbug.com/416730)**  
**[Commit: Disable merging simulates across captured objects.](https://chromium.googlesource.com/v8/v8/+/b11c925)**  
  
Date(Commit): Thu Sep 25 12:16:32 2014  
Components/Type: Blink/Bug  
Labels: ["Stability-Crash", "M-39", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/607453002](https://codereview.chromium.org/607453002)  
Regress: [mjsunit/regress/regress-416730.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-416730.js)  
```javascript
var d = {x: undefined, y: undefined};

function Crash(left, right) {
  var c = {
    x: right.x - left.x,
    y: right.y - left.y
  };
  return c.x * c.y;
}

var a = {x: 0.5, y: 0};
var b = {x: 1, y: 0};

for (var i = 0; i < 3; i++) Crash(a, b);
%OptimizeFunctionOnNextCall(Crash);
Crash(a, b);

Crash({x: 0, y: 0.5}, b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b11c925^!)  
[src/hydrogen-removable-simulates.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-removable-simulates.cc?cl=b11c925)  
[test/mjsunit/regress/regress-416730.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-416730.js?cl=b11c925)  
  

---   

## **regress-crbug-416558.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Map::instance_type](https://crbug.com/416558)**  
**[Commit: Non-JSArrays must always have holey elements.](https://chromium.googlesource.com/v8/v8/+/1903e56)**  
  
Date(Commit): Thu Sep 25 08:25:25 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Te-Logged", "M-39", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/595333002](https://codereview.chromium.org/595333002)  
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
  

---   

## **regress-416416.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!root->IsNull()) failed: ../../v8/src/lookup.cc(49)](https://crbug.com/416416)**  
**[Commit: Fix IC cache confusion on String.prototype.length](https://chromium.googlesource.com/v8/v8/+/b0b5907)**  
  
Date(Commit): Wed Sep 24 09:33:04 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/587363002](https://codereview.chromium.org/587363002)  
Regress: [mjsunit/regress/regress-416416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-416416.js)  
```javascript
function foo() {
  try {
    String.prototype.length.x();
  } catch (e) {
  }
}

foo();
foo();
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b0b5907^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=b0b5907)  
[test/mjsunit/regress/regress-416416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-416416.js?cl=b0b5907)  
  

---   

## **regress-json-parse-index.js (other issue)**  
   
**[Commit: Fix escaped index JSON parsing](https://chromium.googlesource.com/v8/v8/+/83f64e8)**  
  
Date(Commit): Mon Sep 22 15:21:19 2014  
Code Review: [https://codereview.chromium.org/592813002](https://codereview.chromium.org/592813002)  
Regress: [mjsunit/regress/regress-json-parse-index.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-json-parse-index.js)  
```javascript
var o = JSON.parse('{"\\u0030":100}');
assertEquals(100, o[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/83f64e8^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=83f64e8)  
[test/mjsunit/regress/regress-json-parse-index.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-json-parse-index.js?cl=83f64e8)  
  
  
---   

## **regress-3564.js (v8 issue)**  
   
**[Issue: String comparison in TurboFan is borked](https://crbug.com/v8/3564)**  
**[Commit: Fix typed lowering to number comparison.](https://chromium.googlesource.com/v8/v8/+/429924b)**  
  
Date(Commit): Tue Sep 16 11:33:30 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/574653002](https://codereview.chromium.org/574653002)  
Regress: [mjsunit/regress/regress-3564.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3564.js)  
```javascript
function MyWrapper(v) {
  return { valueOf: function() { return v } };
}

function f() {
  assertTrue("a" < "x");
  assertTrue("a" < new String("y"));
  assertTrue("a" < new MyWrapper("z"));

  assertFalse("a" > "x");
  assertFalse("a" > new String("y"));
  assertFalse("a" > new MyWrapper("z"));
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/429924b^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=429924b)  
[test/mjsunit/regress/regress-3564.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3564.js?cl=429924b)  
  

---   

## **regress-crbug-412215.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(value->IsHeapObject()) failed: ../../v8/src/objects-debug.cc(271)](https://crbug.com/412215)**  
**[Commit: Fix Smi vs. HeapObject confusion in HConstants.](https://chromium.googlesource.com/v8/v8/+/b4375b7)**  
  
Date(Commit): Fri Sep 12 08:44:14 2014  
Components/Type: Blink>JavaScript/Bug-Regression  
Labels: ["M-39", "External-Fuzzer-Contribution", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/556563005](https://codereview.chromium.org/556563005)  
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
  

---   

## **regress-crbug-412210.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(right_type->Is(Type::String())) failed: ../../v8/src/hydrogen.cc(1028](https://crbug.com/412210)**  
**[Commit: Fix inaccurate type condition in Hydrogen](https://chromium.googlesource.com/v8/v8/+/fc71f7f)**  
  
Date(Commit): Thu Sep 11 12:13:34 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/550453003](https://codereview.chromium.org/550453003)  
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
  

---   

## **regress-crbug-412203.js (chromium issue)**  
   
**[Issue: Unreachable code in ../../v8/src/runtime.cc(10277)](https://crbug.com/412203)**  
**[Commit: Fix ElementsKind handling of prototypes in Array.concat](https://chromium.googlesource.com/v8/v8/+/11f7584)**  
  
Date(Commit): Thu Sep 11 10:04:13 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-39", "External-Fuzzer-Contribution", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/560463002](https://codereview.chromium.org/560463002)  
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
  

---   

## **regress-crbug-412319.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(receiver_map->is_extensible()) failed: ../../v8/src/hydrogen.cc(8293)](https://crbug.com/412319)**  
**[Commit: Don't inline Array functions if receiver map is not extensible.](https://chromium.googlesource.com/v8/v8/+/d66ed11)**  
  
Date(Commit): Wed Sep 10 09:22:13 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/552333002](https://codereview.chromium.org/552333002)  
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
  

---   

## **regress-411210.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(start <= end) failed: ../../v8/src/heap/spaces.cc(1722)](https://crbug.com/411210)**  
**[Commit: Remove guard page mechanism from promotion queue.](https://chromium.googlesource.com/v8/v8/+/ed37edc)**  
  
Date(Commit): Wed Sep 10 07:51:29 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-38", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "Release-0-M38"]  
Code Review: [https://codereview.chromium.org/557243002](https://codereview.chromium.org/557243002)  
Regress: [mjsunit/regress/regress-411210.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-411210.js)  
```javascript
var __v_3;
function __f_2() {
  var __v_1 = new Array(3);
  __v_1[0] = 10;
  __v_1[1] = 15.5;
  __v_3 = __f_2();
  __v_1[2] = 20;
  return __v_1;
}

try {
  for (var __v_2 = 0; __v_2 < 3; ++__v_2) {
    __v_3 = __f_2();
  }
}
catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ed37edc^!)  
[src/heap/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap-inl.h?cl=ed37edc)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=ed37edc)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=ed37edc)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=ed37edc)  
[test/mjsunit/regress/regress-411210.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-411210.js?cl=ed37edc)  
  

---   

## **regress-412162.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Handle<v8::internal::Object>::operator*](https://crbug.com/412162)**  
**[Commit: Handle non-object constants in HConstant::GetMonomorphicJSObjectMap.](https://chromium.googlesource.com/v8/v8/+/01d63e4)**  
  
Date(Commit): Tue Sep 09 12:58:34 2014  
Components/Type: None/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/552243002](https://codereview.chromium.org/552243002)  
Regress: [mjsunit/regress/regress-412162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-412162.js)  
```javascript
function test() {
  Math.abs(-NaN).toString();
}

test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/01d63e4^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=01d63e4)  
[test/mjsunit/regress/regress-412162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-412162.js?cl=01d63e4)  
  

---   

## **regress-crbug-412208.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(ast_context()->IsEffect()) failed: ../../v8/src/hydrogen.cc(6760)](https://crbug.com/412208)**  
**[Commit: Hydrogen: bailout when there is a throw statement in a non-effect context.](https://chromium.googlesource.com/v8/v8/+/fd3e505)**  
  
Date(Commit): Tue Sep 09 12:16:33 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/558593002](https://codereview.chromium.org/558593002)  
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
  

---   

## **regress-411262.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(function->code()->kind() == Code::FUNCTION || function->code()->kind(](https://crbug.com/411262)**  
**[Commit: Fix more fallout from making OptimizeFunctionOnNextCall work as advertised.](https://chromium.googlesource.com/v8/v8/+/4c53bb0)**  
  
Date(Commit): Fri Sep 05 15:31:33 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/544213002](https://codereview.chromium.org/544213002)  
Regress: [mjsunit/compiler/regress-411262.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-411262.js)  
```javascript
function b() {
}
function f() {
  b.apply(this, arguments);
}

%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c53bb0^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=4c53bb0)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4c53bb0)  
[test/mjsunit/compiler/regress-411262.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-411262.js?cl=4c53bb0)  
  

---   

## **regress-411237.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK_EQ(FUNCTION, kind()) failed: ../../v8/src/objects-inl.h(4701)](https://crbug.com/411237)**  
**[Commit: Harden OptimizeFunctionOnNextCall.](https://chromium.googlesource.com/v8/v8/+/83af12c)**  
  
Date(Commit): Fri Sep 05 15:13:44 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/547553003](https://codereview.chromium.org/547553003)  
Regress: [mjsunit/es6/regress/regress-411237.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-411237.js)  
```javascript
try {
  %OptimizeFunctionOnNextCall(print);
} catch(e) { }

try {
  function* f() {
  }
  %OptimizeFunctionOnNextCall(f);
} catch(e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/83af12c^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=83af12c)  
[test/mjsunit/regress/regress-411237.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-411237.js?cl=83af12c)  
  

---   

## **regress-reset-dictionary-elements.js (other issue)**  
   
**[Commit: Allocate a new empty number dictionary when resetting elements](https://chromium.googlesource.com/v8/v8/+/1dddf69)**  
  
Date(Commit): Fri Sep 05 11:38:22 2014  
Code Review: [https://codereview.chromium.org/545773003](https://codereview.chromium.org/545773003)  
Regress: [mjsunit/regress/regress-reset-dictionary-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-reset-dictionary-elements.js)  
```javascript
var a = [];
a[10000] = 1;
a.length = 0;
a[1] = 1;
a.length = 0;
assertEquals(undefined, a[1]);

var o = {};
Object.freeze(o);
assertEquals(undefined, o[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1dddf69^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=1dddf69)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1dddf69)  
[test/mjsunit/regress/regress-reset-dictionary-elements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-reset-dictionary-elements.js?cl=1dddf69)  
  
  
---   

## **regress-410912.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::MemoryChunk::IsFlagSet](https://crbug.com/410912)**  
**[Commit: Fix EvacuateJSFunction to obtain the target address from the forwarding pointer.](https://chromium.googlesource.com/v8/v8/+/b74fae5)**  
  
Date(Commit): Fri Sep 05 09:38:04 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["ReleaseBlock-Beta", "Merge-na", "M-39", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz", "Owner-Triage"]  
Code Review: [https://codereview.chromium.org/541353003](https://codereview.chromium.org/541353003)  
Regress: [mjsunit/regress/regress-410912.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-410912.js)  
```javascript
var assertDoesNotThrow;
var assertInstanceof;
var assertUnreachable;
var assertOptimized;
var assertUnoptimized;
function classOf(object) { var string = Object.prototype.toString.call(object); return string.substring(8, string.length - 1); }
function PrettyPrint(value) { return ""; }
function PrettyPrintArrayElement(value, index, array) { return ""; }
function fail(expectedText, found, name_opt) { }
function deepObjectEquals(a, b) { var aProps = Object.keys(a); aProps.sort(); var bProps = Object.keys(b); bProps.sort(); if (!deepEquals(aProps, bProps)) { return false; } for (var i = 0; i < aProps.length; i++) { if (!deepEquals(a[aProps[i]], b[aProps[i]])) { return false; } } return true; }
function deepEquals(a, b) { if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(a) && isNaN(b); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(a); if (objectClass !== classOf(b)) return false; if (objectClass === "RegExp") { return (a.toString() === b.toString()); } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (!deepEquals(a[i], b[i])) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf() !== b.valueOf()) return false; } return deepObjectEquals(a, b); }
assertSame = function assertSame(expected, found, name_opt) { if (found === expected) { if (expected !== 0 || (1 / expected) == (1 / found)) return; } else if ((expected !== expected) && (found !== found)) { return; } fail(PrettyPrint(expected), found, name_opt); }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected), found, name_opt); } };
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) { assertTrue(Math.abs(expected - found) <= delta, name_opt); };
assertArrayEquals = function assertArrayEquals(expected, found, name_opt) { var start = ""; if (name_opt) { start = name_opt + " - "; } assertEquals(expected.length, found.length, start + "array length"); if (expected.length == found.length) { for (var i = 0; i < expected.length; ++i) { assertEquals(expected[i], found[i], start + "array element at index " + i); } } };
assertPropertiesEqual = function assertPropertiesEqual(expected, found, name_opt) { if (!deepObjectEquals(expected, found)) { fail(expected, found, name_opt); } };
assertToStringEquals = function assertToStringEquals(expected, found, name_opt) { if (expected != String(found)) { fail(expected, found, name_opt); } };
assertTrue = function assertTrue(value, name_opt) { assertEquals(true, value, name_opt); };
assertFalse = function assertFalse(value, name_opt) { assertEquals(false, value, name_opt); };
assertNull = function assertNull(value, name_opt) { if (value !== null) { fail("null", value, name_opt); } };
assertNotNull = function assertNotNull(value, name_opt) { if (value === null) { fail("not null", value, name_opt); } };
var __v_39 = {};
var __v_40 = {};
var __v_41 = {};
var __v_42 = {};
var __v_43 = {};
var __v_44 = {};
try {
__v_0 = [1.5,,1.7];
__v_1 = {__v_0:1.8};
} catch(e) { print("Caught: " + e); }
function __f_0(__v_1,__v_0,i) {
  __v_1.a = __v_0[i];
  gc();
}
try {
__f_0(__v_1,__v_0,0);
__f_0(__v_1,__v_0,0);
%OptimizeFunctionOnNextCall(__f_0);
__f_0(__v_1,__v_0,1);
assertEquals(undefined, __v_1.a);
__v_0 = [1,,3];
__v_1 = {ab:5};
} catch(e) { print("Caught: " + e); }
function __f_1(__v_1,__v_0,i) {
  __v_1.ab = __v_0[i];
}
try {
__f_1(__v_1,__v_0,1);
} catch(e) { print("Caught: " + e); }
function __f_5(x) {
  return ~x;
}
try {
__f_5(42);
assertEquals(~12, __f_5(12.45));
assertEquals(~46, __f_5(42.87));
__v_2 = 1, __v_4 = 2, __v_3 = 4, __v_6 = 8;
} catch(e) { print("Caught: " + e); }
function __f_4() {
  return __v_2 | (__v_4 | (__v_3 | __v_6));
}
try {
__f_4();
__v_3 = "16";
assertEquals(17 | -13 | 0 | -5, __f_4());
} catch(e) { print("Caught: " + e); }
function __f_6() {
  return __f_4();
}
try {
assertEquals(1 | 2 | 16 | 8, __f_6());
__f_4 = function() { return 42; };
assertEquals(42, __f_6());
__v_5 = {};
__v_5.__f_4 = __f_4;
} catch(e) { print("Caught: " + e); }
function __f_7(o) {
  return o.__f_4();
}
try {
for (var __v_7 = 0; __v_7 < 5; __v_7++) __f_7(__v_5);
%OptimizeFunctionOnNextCall(__f_7);
__f_7(__v_5);
assertEquals(42, __f_7(__v_5));
assertEquals(87, __f_7({__f_4: function() { return 87; }}));
} catch(e) { print("Caught: " + e); }
function __f_8(x,y) {
  x = 42;
  y = 1;
  y = y << "0";
  return x | y;
}
try {
assertEquals(43, __f_8(0,0));
} catch(e) { print("Caught: " + e); }
function __f_2(x) {
  return 'lit[' + (x + ']');
}
try {
assertEquals('lit[-87]', __f_2(-87));
assertEquals('lit[0]', __f_2(0));
assertEquals('lit[42]', __f_2(42));
__v_9 = "abc";
gc();
var __v_8;
} catch(e) { print("Caught: " + e); }
function __f_9(n) { return __v_9.charAt(n); }
try {
for (var __v_7 = 0; __v_7 < 5; __v_7++) {
  __v_8 = __f_9(0);
}
%OptimizeFunctionOnNextCall(__f_9);
__v_8 = __f_9(0);
} catch(e) { print("Caught: " + e); }
function __f_3(__v_2,__v_4,__v_3,__v_6) {
  return __v_2+__v_4+__v_3+__v_6;
}
try {
assertEquals(0x40000003, __f_3(1,1,2,0x3fffffff));
} catch(e) { print("Caught: " + e); }
try {
__v_19 = {
  fast_smi_only             :  'fast smi only elements',
  fast                      :  'fast elements',
  fast_double               :  'fast double elements',
  dictionary                :  'dictionary elements',
  external_int32            :  'external int8 elements',
  external_uint8            :  'external uint8 elements',
  external_int16            :  'external int16 elements',
  external_uint16           :  'external uint16 elements',
  external_int32            :  'external int32 elements',
  external_uint32           :  'external uint32 elements',
  external_float32          :  'external float32 elements',
  external_float64          :  'external float64 elements',
  external_uint8_clamped    :  'external uint8_clamped elements',
  fixed_int32               :  'fixed int8 elements',
  fixed_uint8               :  'fixed uint8 elements',
  fixed_int16               :  'fixed int16 elements',
  fixed_uint16              :  'fixed uint16 elements',
  fixed_int32               :  'fixed int32 elements',
  fixed_uint32              :  'fixed uint32 elements',
  fixed_float32             :  'fixed float32 elements',
  fixed_float64             :  'fixed float64 elements',
  fixed_uint8_clamped       :  'fixed uint8_clamped elements'
}
} catch(e) { print("Caught: " + e); }
function __f_12() {
}
__v_10 = {};
__v_10.dance = 0xD15C0;
__v_10.drink = 0xC0C0A;
__f_12(__v_19.fast, __v_10);
__v_24 = [1,2,3];
__f_12(__v_19.fast_smi_only, __v_24);
__v_24.dance = 0xD15C0;
__v_24.drink = 0xC0C0A;
__f_12(__v_19.fast_smi_only, __v_24);
function __f_18() {
  var __v_27 = new Array();
  __f_12(__v_19.fast_smi_only, __v_27);
  for (var __v_18 = 0; __v_18 < 1337; __v_18++) {
    var __v_16 = __v_18;
    if (__v_18 == 1336) {
      __f_12(__v_19.fast_smi_only, __v_27);
      __v_16 = new Object();
    }
    __v_27[__v_18] = __v_16;
  }
  __f_12(__v_19.fast, __v_27);
  var __v_15 = [];
  __v_15[912570] = 7;
  __f_12(__v_19.dictionary, __v_15);
  var __v_26 = new Array(912561);
  %SetAllocationTimeout(100000000, 10000000);
  for (var __v_18 = 0; __v_18 < 0x20000; __v_18++) {
    __v_26[0] = __v_18 / 2;
  }
  __f_12(__v_19.fixed_int8,    new Int8Array(007));
  __f_12(__v_19.fixed_uint8,   new Uint8Array(007));
  __f_12(__v_19.fixed_int16,   new Int16Array(666));
  __f_12(__v_19.fixed_uint16,  new Uint16Array(42));
  __f_12(__v_19.fixed_int32,   new Int32Array(0xF));
  __f_12(__v_19.fixed_uint32,  new Uint32Array(23));
  __f_12(__v_19.fixed_float32, new Float32Array(7));
  __f_12(__v_19.fixed_float64, new Float64Array(0));
  __f_12(__v_19.fixed_uint8_clamped, new Uint8ClampedArray(512));
  var __v_13 = new ArrayBuffer(128);
  __f_12(__v_19.external_int8,    new Int8Array(__v_13));
  __f_12(__v_37.external_uint8,   new Uint8Array(__v_13));
  __f_12(__v_19.external_int16,   new Int16Array(__v_13));
  __f_12(__v_19.external_uint16,  new Uint16Array(__v_13));
  __f_12(__v_19.external_int32,   new Int32Array(__v_13));
  __f_12(__v_19.external_uint32,  new Uint32Array(__v_13));
  __f_12(__v_19.external_float32, new Float32Array(__v_13));
  __f_12(__v_19.external_float64, new Float64Array(__v_13));
  __f_12(__v_19.external_uint8_clamped, new Uint8ClampedArray(__v_13));
}
try {
__f_18();
} catch(e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b74fae5^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=b74fae5)  
[test/mjsunit/regress/regress-410912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-410912.js?cl=b74fae5)  
  

---   

## **regress-crbug-407946.js (chromium issue)**  
   
**[Issue: array indexOf unreliable](https://crbug.com/407946)**  
**[Commit: Enforce correct number comparisons when inlining Array.indexOf.](https://chromium.googlesource.com/v8/v8/+/0baf275)**  
  
Date(Commit): Thu Sep 04 12:25:57 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-38", "Via-Wizard", "Merge-Questions-Applied"]  
Code Review: [https://codereview.chromium.org/536393003](https://codereview.chromium.org/536393003)  
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
  

---   

## **regress-inline-constant-load.js (other issue)**  
   
**[Commit: Fix loading non-configurable non-writable value from a constant with mismatching type feedback](https://chromium.googlesource.com/v8/v8/+/03b0237)**  
  
Date(Commit): Wed Sep 03 12:13:46 2014  
Code Review: [https://codereview.chromium.org/534093003](https://codereview.chromium.org/534093003)  
Regress: [mjsunit/regress/regress-inline-constant-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-constant-load.js)  
```javascript
var o1 = {};
var o2 = {};

function foo(x) {
  return x.bar;
}

Object.defineProperty(o1, "bar", {value:200});
foo(o1);
foo(o1);

function f(b) {
  var o = o2;
  if (b) { return foo(o) }
}

f(false);
%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f(false));
Object.defineProperty(o2, "bar", {value: 100});
assertEquals(100, f(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/03b0237^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=03b0237)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=03b0237)  
[test/mjsunit/regress/regress-inline-constant-load.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-inline-constant-load.js?cl=03b0237)  
  
  
---   

## **regress-force-constant-representation.js (other issue)**  
   
**[Commit: Fixed inlining of constant values](https://chromium.googlesource.com/v8/v8/+/2a37ab7)**  
  
Date(Commit): Tue Aug 26 11:34:25 2014  
Code Review: [https://codereview.chromium.org/507613002](https://codereview.chromium.org/507613002)  
Regress: [mjsunit/regress/regress-force-constant-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-force-constant-representation.js)  
```javascript
var a = [{}];
function f(a) {
  a.push(Infinity);
}

f(a);
f(a);
f(a);
%OptimizeFunctionOnNextCall(f);
f(a);
assertEquals([{}, Infinity, Infinity, Infinity, Infinity], a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a37ab7^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=2a37ab7)  
[src/property-details-inl.h](https://cs.chromium.org/chromium/src/v8/src/property-details-inl.h?cl=2a37ab7)  
[src/property-details.h](https://cs.chromium.org/chromium/src/v8/src/property-details.h?cl=2a37ab7)  
[test/mjsunit/regress/regress-force-constant-representation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-force-constant-representation.js?cl=2a37ab7)  
  
  
---   

## **regress-crbug-405517.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(receiver_map->is_extensible()) failed: ../../v8/src/hydrogen.cc(8338)](https://crbug.com/405517)**  
**[Commit: Don't inline Array.shift() if receiver map is not extensible.](https://chromium.googlesource.com/v8/v8/+/0142786)**  
  
Date(Commit): Thu Aug 21 06:23:44 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/491863002](https://codereview.chromium.org/491863002)  
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
  

---   

## **regress-404981.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!lo_space()->Contains(object)) failed: ../../v8/src/heap/heap.cc(3324](https://crbug.com/404981)**  
**[Commit: Do not install fillers when right trimming large objects.](https://chromium.googlesource.com/v8/v8/+/91599ff)**  
  
Date(Commit): Tue Aug 19 08:35:39 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/487703002](https://codereview.chromium.org/487703002)  
Regress: [mjsunit/regress/regress-404981.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-404981.js)  
```javascript
var large_object = new Array(5000001);
large_object.length = 23;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91599ff^!)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=91599ff)  
[test/mjsunit/regress/regress-404981.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-404981.js?cl=91599ff)  
  

---   

## **regress-crbug-403409.js (chromium issue)**  
   
**[Issue: V8 Runtime_ArrayConcat uninitialized memory leak](https://crbug.com/403409)**  
**[Commit: Correctly handle holes when concat()ing double arrays](https://chromium.googlesource.com/v8/v8/+/dacca11)**  
  
Date(Commit): Mon Aug 18 08:51:35 2014  
Components/Type: None/Bug-Security  
Labels: ["M-38", "Via-Wizard", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "CVE-2014-3195", "reward-4500", "allpublic", "Release-0-M38", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/468863003](https://codereview.chromium.org/468863003)  
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
  

---   

## **regress-3476.js (v8 issue)**  
   
**[Issue: String addition in Crankshaft is borked](https://crbug.com/v8/3476)**  
**[Commit: Fix handling of potential string additions in hydrogen.](https://chromium.googlesource.com/v8/v8/+/57c315d)**  
  
Date(Commit): Tue Jul 29 14:53:11 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/423083004](https://codereview.chromium.org/423083004)  
Regress: [mjsunit/regress/regress-3476.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3476.js)  
```javascript
function MyWrapper(v) {
  return { valueOf: function() { return v } };
}

function f() {
  assertEquals("truex", true + "x");
  assertEquals("truey", true + new String("y"));
  assertEquals("truez", true + new MyWrapper("z"));

  assertEquals("xtrue", "x" + true);
  assertEquals("ytrue", new String("y") + true);
  assertEquals("ztrue", new MyWrapper("z") + true);
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/57c315d^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=57c315d)  
[test/mjsunit/regress/regress-3476.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3476.js?cl=57c315d)  
  

---   

## **regress-update-field-type-attributes.js (other issue)**  
   
**[Commit: Fix Object.freeze with field type tracking.](https://chromium.googlesource.com/v8/v8/+/f08d269)**  
  
Date(Commit): Tue Jul 29 13:30:29 2014  
Code Review: [https://codereview.chromium.org/424093002](https://codereview.chromium.org/424093002)  
Regress: [mjsunit/regress/regress-update-field-type-attributes.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-update-field-type-attributes.js)  
```javascript
function test(){
  function InnerClass(){}
  var container = {field: new InnerClass()};
  return Object.freeze(container);
};

assertTrue(Object.isFrozen(test()));
assertTrue(Object.isFrozen(test()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f08d269^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f08d269)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=f08d269)  
[test/mjsunit/regress/regress-update-field-type-attributes.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-update-field-type-attributes.js?cl=f08d269)  
  
  
---   

## **regress-grow-deopt.js (other issue)**  
   
**[Commit: In GrowMode, force the value to the right representation to avoid deopts between storing the length and storing the value.](https://chromium.googlesource.com/v8/v8/+/60df9da)**  
  
Date(Commit): Fri Jul 25 11:48:25 2014  
Code Review: [https://codereview.chromium.org/419683004](https://codereview.chromium.org/419683004)  
Regress: [mjsunit/regress/regress-grow-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-grow-deopt.js)  
```javascript
function f(a, v) {
  a[a.length] = v;
}

var a = [1.4];
f(a, 1);
f(a, 2);
%OptimizeFunctionOnNextCall(f);
f(a, {});
assertEquals(4, a.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/60df9da^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=60df9da)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=60df9da)  
[test/mjsunit/regress/regress-grow-deopt.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-grow-deopt.js?cl=60df9da)  
  
  
---   

## **regress-3462.js (v8 issue)**  
   
**[Issue: FunctionPrototypeSetter is not correct](https://crbug.com/v8/3462)**  
**[Commit: Fix issue with setters and their holders in accessors.cc](https://chromium.googlesource.com/v8/v8/+/77a37e4)**  
  
Date(Commit): Thu Jul 24 16:42:54 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/417793002](https://codereview.chromium.org/417793002)  
Regress: [mjsunit/regress/regress-3462.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3462.js)  
```javascript
function TestFunctionPrototypeSetter() {
  var f = function() {};
  var o = {__proto__: f};
  o.prototype = 42;
  assertEquals(42, o.prototype);
  assertTrue(o.hasOwnProperty('prototype'));
}
TestFunctionPrototypeSetter();


function TestFunctionPrototypeSetterOnValue() {
  var f = function() {};
  var fp = f.prototype;
  Number.prototype.__proto__ = f;
  var n = 42;
  var o = {};
  n.prototype = o;
  assertEquals(fp, n.prototype);
  assertEquals(fp, f.prototype);
  assertFalse(Number.prototype.hasOwnProperty('prototype'));
}
TestFunctionPrototypeSetterOnValue();


function TestArrayLengthSetter() {
  var a = [1];
  var o = {__proto__: a};
  o.length = 2;
  assertEquals(2, o.length);
  assertEquals(1, a.length);
  assertTrue(o.hasOwnProperty('length'));
}
TestArrayLengthSetter();


function TestArrayLengthSetterOnValue() {
  Number.prototype.__proto__ = [1];
  var n = 42;
  n.length = 2;
  assertEquals(1, n.length);
  assertFalse(Number.prototype.hasOwnProperty('length'));
}
TestArrayLengthSetterOnValue();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77a37e4^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=77a37e4)  
[test/mjsunit/es7/object-observe.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es7/object-observe.js?cl=77a37e4)  
[test/mjsunit/regress/regress-3462.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3462.js?cl=77a37e4)  
  

---   

## **regress-mask-array-length.js (other issue)**  
   
**[Commit: Fix ArrayLengthSetter to not throw on non-extensible receivers.](https://chromium.googlesource.com/v8/v8/+/6798779)**  
  
Date(Commit): Wed Jul 23 20:27:32 2014  
Code Review: [https://codereview.chromium.org/411983003](https://codereview.chromium.org/411983003)  
Regress: [mjsunit/regress/regress-mask-array-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-mask-array-length.js)  
```javascript
var a = [];
var o = {
  __proto__: a
};
Object.preventExtensions(o);
o.length = 'abc';  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6798779^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=6798779)  
[test/mjsunit/regress/regress-mask-array-length.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-mask-array-length.js?cl=6798779)  
  
  
---   

## **regress-3456.js (v8 issue)**  
   
**[Issue: Prefix operation parsing broken](https://crbug.com/v8/3456)**  
**[Commit: Fix checks to bit flags of PreParserExpression](https://chromium.googlesource.com/v8/v8/+/b5220b2)**  
  
Date(Commit): Wed Jul 23 13:29:24 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/410873004](https://codereview.chromium.org/410873004)  
Regress: [mjsunit/regress/regress-3456.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3456.js)  
```javascript
function f() { ++(this.foo) }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b5220b2^!)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=b5220b2)  
[test/mjsunit/regress-3456.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-3456.js?cl=b5220b2)  
  

---   

## **regress-regexp-nocase.js (other issue)**  
   
**[Commit: ARM64: always restore regexp register cache after a C function call.](https://chromium.googlesource.com/v8/v8/+/56ec59b)**  
  
Date(Commit): Thu Jul 17 09:55:48 2014  
Code Review: [https://codereview.chromium.org/392403002](https://codereview.chromium.org/392403002)  
Regress: [mjsunit/regress/regress-regexp-nocase.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-regexp-nocase.js)  
```javascript
var s = "('')x\nx\uF670";

assertEquals(s.match(/\((').*\1\)/i), ["('')", "'"]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56ec59b^!)  
[src/arm64/regexp-macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/regexp-macro-assembler-arm64.cc?cl=56ec59b)  
[test/mjsunit/regress/regress-regexp-nocase.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-regexp-nocase.js?cl=56ec59b)  
  
  
---   

## **regress-crbug-393988.js (chromium issue)**  
   
**[Issue: TypeError: Cannot redefine property: stack](https://crbug.com/393988)**  
**[Commit: Error.captureStackTrace should define "stack" property as configurable.](https://chromium.googlesource.com/v8/v8/+/49ae308)**  
  
Date(Commit): Wed Jul 16 07:55:05 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Hotlist-Netflix"]  
Code Review: [https://codereview.chromium.org/396063008](https://codereview.chromium.org/396063008)  
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
  

---   

## **regress-381313.js (chromium issue)**  
   
**[Issue: Feature request: Profile icon no longer displayed with Chrome(ium) Icon](https://crbug.com/381313)**  
**[Commit: Fix arm64 deoptimization from double registers (reverts r20613).](https://chromium.googlesource.com/v8/v8/+/457de26)**  
  
Date(Commit): Fri Jul 11 19:30:09 2014  
Components/Type: UI/Feature  
Labels: ["Via-Wizard", "Hotlist-Google"]  
Code Review: [https://codereview.chromium.org/389583003](https://codereview.chromium.org/389583003)  
Regress: [mjsunit/regress/regress-381313.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-381313.js)  
```javascript
var g = 0;

function f(x, deopt) {
  var a0 = x;
  var a1 = 2 * x;
  var a2 = 3 * x;
  var a3 = 4 * x;
  var a4 = 5 * x;
  var a5 = 6 * x;
  var a6 = 7 * x;
  var a7 = 8 * x;
  var a8 = 9 * x;
  var a9 = 10 * x;
  var a10 = 11 * x;
  var a11 = 12 * x;
  var a12 = 13 * x;
  var a13 = 14 * x;
  var a14 = 15 * x;
  var a15 = 16 * x;
  var a16 = 17 * x;
  var a17 = 18 * x;
  var a18 = 19 * x;
  var a19 = 20 * x;

  g = 1;

  deopt + 0;

  return a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 + a8 + a9 +
         a10 + a11 + a12 + a13 + a14 + a15 + a16 + a17 + a18 + a19;
}

f(0.5, 0);
f(0.5, 0);
%OptimizeFunctionOnNextCall(f);
print(f(0.5, ""));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/457de26^!)  
[src/arm64/deoptimizer-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/deoptimizer-arm64.cc?cl=457de26)  
[test/mjsunit/regress/regress-381313.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-381313.js?cl=457de26)  
  

---   

## **regress-3426.js (v8 issue)**  
   
**[Issue: Redeclaration checking wrong for locally strict functions](https://crbug.com/v8/3426)**  
**[Commit: Fix several issues with ES6 redeclaration checks](https://chromium.googlesource.com/v8/v8/+/2375327)**  
  
Date(Commit): Wed Jul 09 11:35:05 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/377513006](https://codereview.chromium.org/377513006)  
Regress: [mjsunit/es6/regress/regress-3426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3426.js)  
```javascript
assertThrows("(function() { 'use strict'; { let f; var f; } })", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2375327^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=2375327)  
[test/mjsunit/harmony/block-conflicts.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/block-conflicts.js?cl=2375327)  
[test/mjsunit/harmony/block-let-declaration.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/block-let-declaration.js?cl=2375327)  
[test/mjsunit/harmony/regress/regress-3426.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-3426.js?cl=2375327)  
[tools/generate-runtime-tests.py](https://cs.chromium.org/chromium/src/v8/tools/generate-runtime-tests.py?cl=2375327)  
  

---   

## **regress-double-property.js (other issue)**  
   
**[Commit: Fix computed properties on object literals with a double as propertyname.](https://chromium.googlesource.com/v8/v8/+/ad6202d)**  
  
Date(Commit): Mon Jul 07 17:08:54 2014  
Code Review: [https://codereview.chromium.org/371973002](https://codereview.chromium.org/371973002)  
Regress: [mjsunit/regress/regress-double-property.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-double-property.js)  
```javascript
function f(a) {
  return {0.1: a};
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad6202d^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=ad6202d)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=ad6202d)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=ad6202d)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=ad6202d)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=ad6202d)  
...  
  
  
---   

## **regress-sliced-external-cons-regexp.js (other issue)**  
   
**[Commit: Turn old space cons strings into regular external strings (not short).](https://chromium.googlesource.com/v8/v8/+/6574f33)**  
  
Date(Commit): Thu Jul 03 11:46:31 2014  
Code Review: [https://codereview.chromium.org/368223002](https://codereview.chromium.org/368223002)  
Regress: [mjsunit/regress/regress-sliced-external-cons-regexp.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sliced-external-cons-regexp.js)  
```javascript
var re = /(B)/;
var cons1 = "0123456789" + "ABCDEFGHIJ";
var cons2 = "0123456789\u1234" + "ABCDEFGHIJ";
gc();
gc();  // Promote cons.

try { externalizeString(cons1, false); } catch (e) { }
try { externalizeString(cons2, true); } catch (e) { }

var slice1 = cons1.slice(1,-1);
var slice2 = cons2.slice(1,-1);
for (var i = 0; i < 10; i++) {
  assertEquals(["B", "B"], re.exec(slice1));
  assertEquals(["B", "B"], re.exec(slice2));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6574f33^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=6574f33)  
[test/mjsunit/regress/regress-sliced-external-cons-regexp.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-sliced-external-cons-regexp.js?cl=6574f33)  
  
  
---   

## **regress-crbug-390918.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK_EQ(map()->unused_property_fields(), (map()->inobject_properties() + p](https://crbug.com/390918)**  
**[Commit: One of the fast cases in JSObject::MigrateFastToFast() should not be taken if the number of fields did not change.](https://chromium.googlesource.com/v8/v8/+/2fba190)**  
  
Date(Commit): Wed Jul 02 19:10:19 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/363073002](https://codereview.chromium.org/363073002)  
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
  

---   

## **regress-3404.js (v8 issue)**  
   
**[Issue: CaptureStackTrace's stack accessors reconfigure unconfigurable properties](https://crbug.com/v8/3404)**  
**[Commit: Fix stack trace accessor behavior.](https://chromium.googlesource.com/v8/v8/+/e1d80e2)**  
  
Date(Commit): Mon Jun 30 11:48:20 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/343563009](https://codereview.chromium.org/343563009)  
Regress: [mjsunit/regress/regress-3404.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3404.js)  
```javascript
function testError(error) {
  // Reconfigure e.stack to be non-configurable
  var desc1 = Object.getOwnPropertyDescriptor(error, "stack");
  Object.defineProperty(error, "stack",
                        {get: desc1.get, set: desc1.set, configurable: false});

  var desc2 = Object.getOwnPropertyDescriptor(error, "stack");
  assertFalse(desc2.configurable);
  assertEquals(desc1.get, desc2.get);
  assertEquals(desc2.get, desc2.get);
}

function stackOverflow() {
  function f() { f(); }
  try { f() } catch (e) { return e; }
}

function referenceError() {
  try { g() } catch (e) { return e; }
}

testError(referenceError());
testError(stackOverflow());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1d80e2^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=e1d80e2)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=e1d80e2)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=e1d80e2)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=e1d80e2)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=e1d80e2)  
...  
  

---   

## **regress-crbug-387636.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!right->IsConstant() || (!HConstant::cast(right)->HasInteger32Value()](https://crbug.com/387636)**  
**[Commit: Remove bogus assertions in HCompareObjectEqAndBranch.](https://chromium.googlesource.com/v8/v8/+/58bf19e)**  
  
Date(Commit): Tue Jun 24 09:33:05 2014  
Components/Type: Blink/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/331863015](https://codereview.chromium.org/331863015)  
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
  

---   

## **regress-386034.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/386034)**  
**[Commit: Add missing map check to optimized f.apply(...)](https://chromium.googlesource.com/v8/v8/+/e56faa9)**  
  
Date(Commit): Mon Jun 23 05:50:06 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-37", "Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-Stable", "Release-1-M36", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "M-36"]  
Code Review: [https://codereview.chromium.org/346473002/,](https://codereview.chromium.org/346473002/,)  
Regress: [mjsunit/regress/regress-386034.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-386034.js)  
```javascript
function f(x) {
  var v = x;
  for (i = 0; i < 1; i++) {
    v.apply(this, arguments);
  }
}

function g() {}

f(g);
f(g);
%OptimizeFunctionOnNextCall(f);
assertThrows(function() { f('----'); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e56faa9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e56faa9)  
[test/mjsunit/regress/regress-386034.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-386034.js?cl=e56faa9)  
  

---   

## **regress-crbug-387031.js (chromium issue)**  
   
**[Issue: Security: V8 Array length getter override](https://crbug.com/387031)**  
**[Commit: Array.concat: properly go to dictionary mode when required](https://chromium.googlesource.com/v8/v8/+/1d35d6d)**  
  
Date(Commit): Fri Jun 20 15:40:21 2014  
Components/Type: None/Bug-Security  
Labels: ["Merge-Merged", "Release-0-M36", "Security_Impact-Stable", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "M-36"]  
Code Review: [https://codereview.chromium.org/342333002](https://codereview.chromium.org/342333002)  
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
  

---   

## **regress-crbug-385002.js (chromium issue)**  
   
**[Issue: Heap-buffer-overflow in v8::internal::Simulator::HandleRList](https://crbug.com/385002)**  
**[Commit: Interrupts must not mask stack overflow.](https://chromium.googlesource.com/v8/v8/+/11368af)**  
  
Date(Commit): Tue Jun 17 13:54:49 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/339883002](https://codereview.chromium.org/339883002)  
Regress: [mjsunit/regress/regress-crbug-385002.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-385002.js)  
```javascript
%ScheduleBreak(); // Schedule an interrupt that does not go away.

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
...  
  

---   

## **regress-385054.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/385054)**  
**[Commit: Do not eliminate bounds checks for "<const> - x".](https://chromium.googlesource.com/v8/v8/+/f69bb7f)**  
  
Date(Commit): Mon Jun 16 13:43:50 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-Stable", "Release-1-M36", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-36"]  
Code Review: [https://codereview.chromium.org/339583003](https://codereview.chromium.org/339583003)  
Regress: [mjsunit/regress/regress-385054.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-385054.js)  
```javascript
function f(x) {
  var a = [1, 2];
  a[x];
  return a[0 - x];
}

f(0);
f(0);
%OptimizeFunctionOnNextCall(f);
assertEquals(undefined, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f69bb7f^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=f69bb7f)  
[test/mjsunit/regress/regress-385054.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-385054.js?cl=f69bb7f)  
  

---   

## **regress-3392.js (v8 issue)**  
   
**[Issue: Miscompilation of numeric comparison](https://crbug.com/v8/3392)**  
**[Commit: Fix representation of Phis for mutable-heapnumber-in-object-literal properties](https://chromium.googlesource.com/v8/v8/+/aae24ae)**  
  
Date(Commit): Mon Jun 16 08:41:29 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/328343004](https://codereview.chromium.org/328343004)  
Regress: [mjsunit/regress/regress-3392.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3392.js)  
```javascript
function foo() {
  var a = {b: -1.5};
  for (var i = 0; i < 1; i++) {
    a.b = 1;
  }
  assertTrue(0 <= a.b);
}

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aae24ae^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=aae24ae)  
[test/mjsunit/regress/regress-3392.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3392.js?cl=aae24ae)  
  

---   

## **regress-gvn-ftt.js (other issue)**  
   
**[Commit: GVN fix, preventing loads hoisting above stores to the same field when HObjectAccess's representation is not the same.](https://chromium.googlesource.com/v8/v8/+/41e9d91)**  
  
Date(Commit): Fri Jun 13 07:51:45 2014  
Code Review: [https://codereview.chromium.org/331493006](https://codereview.chromium.org/331493006)  
Regress: [mjsunit/regress/regress-gvn-ftt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-gvn-ftt.js)  
```javascript
function A(id) {
  this.id = id;
}

var a1 = new A(1);
var a2 = new A(2);


var g;
function f(o, value) {
  g = o.o;
  o.o = value;
  return o.o;
}

var obj = {o: a1};

f(obj, a1);
f(obj, a1);
%OptimizeFunctionOnNextCall(f);
assertEquals(a2.id, f(obj, a2).id);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/41e9d91^!)  
[src/hydrogen-gvn.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-gvn.cc?cl=41e9d91)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=41e9d91)  
[test/mjsunit/regress/regress-gvn-ftt.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-gvn-ftt.js?cl=41e9d91)  
  
  
---   

## **regress-3380.js (v8 issue)**  
   
**[Issue: Bug with x64](https://crbug.com/v8/3380)**  
**[Commit: Fix unsigned comparisons.](https://chromium.googlesource.com/v8/v8/+/2931f09)**  
  
Date(Commit): Wed Jun 11 09:09:15 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/325133004](https://codereview.chromium.org/325133004)  
Regress: [mjsunit/regress/regress-3380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3380.js)  
```javascript
function foo(a) {
  return (a[0] >>> 0) > 0;
}

var a = new Uint32Array([4]);
var b = new Uint32Array([0x80000000]);
assertTrue(foo(a));
assertTrue(foo(a));
%OptimizeFunctionOnNextCall(foo);
assertTrue(foo(b))  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2931f09^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=2931f09)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=2931f09)  
[src/hydrogen-uint32-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-uint32-analysis.cc?cl=2931f09)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=2931f09)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=2931f09)  
...  
  

---   

## **regress-crbug-382143.js (chromium issue)**  
   
**[Issue: [REGRESSION] Methods become non-enumerable if first introduced via a parent constructor that defines a property](https://crbug.com/382143)**  
**[Commit: Fix invalid attributes when generalizing because of incompatible map change.](https://chromium.googlesource.com/v8/v8/+/0fcd891)**  
  
Date(Commit): Tue Jun 10 12:24:54 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/324933003](https://codereview.chromium.org/324933003)  
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
  

---   

## **regress-crbug-381534.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/381534)**  
**[Commit: Bugfix in inlined versions of Array.indexOf() and Array.lastIndexOf() with a regression test.](https://chromium.googlesource.com/v8/v8/+/6dc967e)**  
  
Date(Commit): Tue Jun 10 09:01:45 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/319343002](https://codereview.chromium.org/319343002)  
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
  

---   

## **regress-crbug-382513.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Simulator::DecodeType2](https://crbug.com/382513)**  
**[Commit: Fix missing smi check in inlined indexOf/lastIndexOf.](https://chromium.googlesource.com/v8/v8/+/7eea77b)**  
  
Date(Commit): Tue Jun 10 04:26:15 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/313233005](https://codereview.chromium.org/313233005)  
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
  

---   

## **regress-380092.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(is_valid(value)) failed: ../../v8/src/utils.h(261)](https://crbug.com/380092)**  
**[Commit: Clusterfuzz identified overflow check needed in dehoisting.](https://chromium.googlesource.com/v8/v8/+/7d2d083)**  
  
Date(Commit): Fri Jun 06 09:12:16 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/315593002](https://codereview.chromium.org/315593002)  
Regress: [mjsunit/regress/regress-380092.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-380092.js)  
```javascript
function many_hoist(o, index) {
  return o[index + 33554427];
}

var obj = {};
many_hoist(obj, 0);
%OptimizeFunctionOnNextCall(many_hoist);
many_hoist(obj, 5);

function constant_too_large(o, index) {
  return o[index + 1033554433];
}

constant_too_large(obj, 0);
%OptimizeFunctionOnNextCall(constant_too_large);
constant_too_large(obj, 5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d2d083^!)  
[src/hydrogen-dehoist.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-dehoist.cc?cl=7d2d083)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=7d2d083)  
[test/mjsunit/regress/regress-380092.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-380092.js?cl=7d2d083)  
  

---   

## **regress-crbug-380512.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/380512)**  
**[Commit: Fix invalid loop condition for Array.lastIndexOf().](https://chromium.googlesource.com/v8/v8/+/9244429)**  
  
Date(Commit): Wed Jun 04 08:21:39 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/313073003](https://codereview.chromium.org/313073003)  
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
  

---   

## **regress-379770.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK_EQ((static_cast<int>(loop_depth) <= loop_nesting_level), GetBackEdgeState(isolate, unoptimized](https://crbug.com/379770)**  
**[Commit: When flag --nouse-osr is set, don't allow osr from hidden runtime calls.](https://chromium.googlesource.com/v8/v8/+/adeaedf)**  
  
Date(Commit): Tue Jun 03 07:45:40 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/310773003](https://codereview.chromium.org/310773003)  
Regress: [mjsunit/regress/regress-379770.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-379770.js)  
```javascript
function foo(obj) {
  var counter = 1;
  for (var i = 0; i < obj.length; i++) %OptimizeOsr();
  counter += obj;
  return counter;
}

function bar() {
  var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12];
  for (var i = 0; i < 100; i++ ) {
    foo(a);
  }
}

try {
  bar();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/adeaedf^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=adeaedf)  
[test/mjsunit/regress/regress-379770.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-379770.js?cl=adeaedf)  
  

---   

## **regress-3359.js (v8 issue)**  
   
**[Issue: Fatal error in LCodeGenBase::CheckEnvironmentUsage](https://crbug.com/v8/3359)**  
**[Commit: HRor and HSar can deoptimize.](https://chromium.googlesource.com/v8/v8/+/5cd009a)**  
  
Date(Commit): Fri May 30 16:12:25 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/309483002](https://codereview.chromium.org/309483002)  
Regress: [mjsunit/regress/regress-3359.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3359.js)  
```javascript
function f() {
  return 1 >> Boolean.constructor + 1;
}
assertEquals(1, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(1, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5cd009a^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=5cd009a)  
[test/mjsunit/regress/regress-3359.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3359.js?cl=5cd009a)  
  

---   

## **regress-3334.js (v8 issue)**  
   
**[Issue: Object.defineProperty(fn,"prototype",{ value: .. , writable: false }) fails](https://crbug.com/v8/3334)**  
**[Commit: Changing the attributes of a data property implemented with](https://chromium.googlesource.com/v8/v8/+/8c54a37)**  
  
Date(Commit): Wed May 28 09:58:27 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/262053011](https://codereview.chromium.org/262053011)  
Regress: [mjsunit/regress/regress-3334.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3334.js)  
```javascript
function foo(){}
Object.defineProperty(foo, "prototype", { value: 2 });
assertEquals(2, foo.prototype);

function bar(){}
Object.defineProperty(bar, "prototype", { value: 2, writable: false });
assertEquals(2, bar.prototype);
assertThrows(function() { "use strict"; bar.prototype = 10; }, TypeError);
assertEquals(false, Object.getOwnPropertyDescriptor(bar,"prototype").writable);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c54a37^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=8c54a37)  
[src/accessors.h](https://cs.chromium.org/chromium/src/v8/src/accessors.h?cl=8c54a37)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=8c54a37)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8c54a37)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=8c54a37)  
...  
  

---   

## **regress-377290.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Map::instance_type](https://crbug.com/377290)**  
**[Commit: Reland "Customized support for feedback on calls to Array." and follow-up fixes.](https://chromium.googlesource.com/v8/v8/+/d755611)**  
  
Date(Commit): Mon May 26 13:59:24 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-Head", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/305493003](https://codereview.chromium.org/305493003)  
Regress: [mjsunit/regress/regress-377290.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-377290.js)  
```javascript
Object.prototype.__defineGetter__('constructor', function() { throw 42; });
__v_7 = [
  function() { [].push() },
];
for (var __v_6 = 0; __v_6 < 5; ++__v_6) {
  for (var __v_8 in __v_7) {
    print(__v_8, " -> ", __v_7[__v_8]);
    gc();
    try { __v_7[__v_8](); } catch (e) {};
  }
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d755611^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=d755611)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=d755611)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=d755611)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=d755611)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=d755611)  
...  
  

---   

## **regress-3307.js (v8 issue)**  
   
**[Issue: Incorrect handling of mutable double boxes by allocation-sinking](https://crbug.com/v8/3307)**  
**[Commit: Fix representation inference for mutable double boxes.](https://chromium.googlesource.com/v8/v8/+/cf448aa)**  
  
Date(Commit): Fri May 23 14:02:08 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/298723014](https://codereview.chromium.org/298723014)  
Regress: [mjsunit/regress/regress-3307.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3307.js)  
```javascript
function p(x) {
  this.x = x;
}

function f() {
  var a = new p(1), b = new p(2);
  for (var i = 0; i < 1; i++) {
    a.x += b.x;
  }
  return a.x;
}

new p(0.1);  // make 'x' mutable box double field in p.

assertEquals(3, f());
assertEquals(3, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf448aa^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=cf448aa)  
[test/mjsunit/regress/regress-3307.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3307.js?cl=cf448aa)  
  

---   

## **regress-crbug-374838.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/374838)**  
**[Commit: Fix ArrayShift hydrogen support](https://chromium.googlesource.com/v8/v8/+/58661c1)**  
  
Date(Commit): Wed May 21 08:51:29 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/299713003](https://codereview.chromium.org/299713003)  
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
  

---   

## **regress-cr372788.js (chromium issue)**  
   
**[Issue: Promises: should not cache results of thenables, only promises](https://crbug.com/372788)**  
**[Commit: Drop thenable coercion cache](https://chromium.googlesource.com/v8/v8/+/98849dd)**  
  
Date(Commit): Wed May 14 10:44:34 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/281753004](https://codereview.chromium.org/281753004)  
Regress: [mjsunit/es6/regress/regress-cr372788.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-cr372788.js)  
```javascript
var x = 0;
var y = 0;

var thenable = { then: function(f) { x++; f(); } };

for (var i = 0; i < 3; ++i) {
  Promise.resolve(thenable).then(function() { x++; y++; });
}
assertEquals(0, x);

(function check() {
  Promise.resolve().then(function() {
    // Delay check until all handlers have run.
    if (y < 3) check(); else assertEquals(6, x);
  }).catch(function(e) { %AbortJS("FAILURE: " + e) });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/98849dd^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=98849dd)  
[src/promise.js](https://cs.chromium.org/chromium/src/v8/src/promise.js?cl=98849dd)  
[test/mjsunit/es6/regress/regress-cr372788.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-cr372788.js?cl=98849dd)  
  

---   

## **regress-set-flags-stress-compact.js (other issue)**  
   
**[Commit: Fix %SetFlags("--stress-compaction")](https://chromium.googlesource.com/v8/v8/+/c3cd2f0)**  
  
Date(Commit): Mon May 12 10:39:08 2014  
Code Review: [https://codereview.chromium.org/261253006](https://codereview.chromium.org/261253006)  
Regress: [mjsunit/regress/regress-set-flags-stress-compact.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-set-flags-stress-compact.js)  
```javascript
var a = [];
for (var i = 0; i < 10000; i++) { a[i * 100] = 0; }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c3cd2f0^!)  
[src/spaces-inl.h](https://cs.chromium.org/chromium/src/v8/src/spaces-inl.h?cl=c3cd2f0)  
[test/mjsunit/regress/regress-set-flags-stress-compact.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-set-flags-stress-compact.js?cl=c3cd2f0)  
  
  
---   

## **regress-370827.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(instance_type == CODE_TYPE) failed: ../../v8/src/objects-inl.h(3953)](https://crbug.com/370827)**  
**[Commit: Make new space iterable for --log-gc and --heap-stats options](https://chromium.googlesource.com/v8/v8/+/3976ebe)**  
  
Date(Commit): Fri May 09 09:23:10 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/272503005](https://codereview.chromium.org/272503005)  
Regress: [mjsunit/regress/regress-370827.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-370827.js)  
```javascript
function g(dummy, x) {
  var start = "";
  if (x) { start = x + " - "; }
  start = start + "array length";
};

function f() {
  gc();
  g([0.1]);
}

f();
%OptimizeFunctionOnNextCall(f);
f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3976ebe^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=3976ebe)  
[test/mjsunit/regress/regress-370827.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-370827.js?cl=3976ebe)  
  

---   

## **regress-368243.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/368243)**  
**[Commit: Fix index register assignment in LoadFieldByIndex for arm, arm64, and mips.](https://chromium.googlesource.com/v8/v8/+/8999a00)**  
  
Date(Commit): Thu May 08 08:51:51 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/269273003](https://codereview.chromium.org/269273003)  
Regress: [mjsunit/regress/regress-368243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-368243.js)  
```javascript
function foo(a, c){
  for(var f in c) {
    if ("object" === typeof c[f]) {
      a[f] = c[f];
      foo(a[f], c[f]);
    }
  }
};

c = {
  "one" : { x : 1},
  "two" : { x : 2},
  "thr" : { x : 3, z : 4},
};

foo({}, c);
foo({}, c);
%OptimizeFunctionOnNextCall(foo);
foo({}, c);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8999a00^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=8999a00)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=8999a00)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=8999a00)  
[test/mjsunit/regress/regress-368243.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-368243.js?cl=8999a00)  
  

---   

## **regress-370384.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(is_int8(disp)) failed: ../../v8/src/x64/assembler-x64.cc(346)](https://crbug.com/370384)**  
**[Commit: Fixed jump in non-SSE4.1 implementation of LMathFloor instruction on x64.](https://chromium.googlesource.com/v8/v8/+/9be0c4d)**  
  
Date(Commit): Tue May 06 14:20:46 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/261853009](https://codereview.chromium.org/261853009)  
Regress: [mjsunit/regress/regress-370384.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-370384.js)  
```javascript
function g(f, x, name) {
  var v2 = f(x);
  for (var i = 0; i < 13000; i++) {
    f(i);
  }
  var v1 = f(x);
  assertEquals(v1, v2);
}

g(Math.sin, 6.283185307179586, "Math.sin");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9be0c4d^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=9be0c4d)  
[test/mjsunit/regress/regress-370384.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-370384.js?cl=9be0c4d)  
  

---   

## **regress-369450.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!v8::internal::FLAG_enable_slow_asserts || (object->IsFixedDoubleArray())) failed: ../../v8/sr](https://crbug.com/369450)**  
**[Commit: Checks for empty array case added before casting elements to FixedDoubleArray.](https://chromium.googlesource.com/v8/v8/+/b4c1eda)**  
  
Date(Commit): Fri May 02 11:30:24 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/264973008](https://codereview.chromium.org/264973008)  
Regress: [mjsunit/regress/regress-369450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-369450.js)  
```javascript
var v = [1.3];
v.length = 0;

var json = JSON.stringify(v);
assertEquals("[]", json);

Array.prototype[0] = 5.5;
var arr = [].concat(v, [{}], [2.3]);
assertEquals([{}, 2.3], arr);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4c1eda^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=b4c1eda)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=b4c1eda)  
[test/mjsunit/regress/regress-369450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-369450.js?cl=b4c1eda)  
  

---   

## **regress-362870.js (chromium issue)**  
   
**[Issue: IE Popcorn benchmark slow due to rasterization and time-in-js](https://crbug.com/362870)**  
**[Commit: Object.defineProperty shouldn't be a hint that we're constructing a dictionary.](https://chromium.googlesource.com/v8/v8/+/7bfc426)**  
  
Date(Commit): Fri May 02 06:02:00 2014  
Components/Type: None/Bug  
Labels: ["SlowContent"]  
Code Review: [https://codereview.chromium.org/261583004](https://codereview.chromium.org/261583004)  
Regress: [mjsunit/regress/regress-362870.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-362870.js)  
```javascript
var obj = {};

for (var i = 0; i < 100; i++) {
  Object.defineProperty(obj, "x" + i, { value: 31415 });
  Object.defineProperty(obj, "y" + i, {
    get: function() { return 42; },
    set: function(value) { }
  });
  assertTrue(%HasFastProperties(obj));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bfc426^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7bfc426)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=7bfc426)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=7bfc426)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=7bfc426)  
[test/mjsunit/regress/regress-362870.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-362870.js?cl=7bfc426)  
  

---   

## **regress-builtinbust-7.js (other issue)**  
   
**[Commit: Bugfix: internationalization routines fail on monkeypatching.](https://chromium.googlesource.com/v8/v8/+/0c3e70a)**  
  
Date(Commit): Wed Apr 30 07:36:12 2014  
Code Review: [https://codereview.chromium.org/253903003](https://codereview.chromium.org/253903003)  
Regress: [mjsunit/regress/regress-builtinbust-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-7.js)  
```javascript
if ("Intl" in this) {
  function overflow() {
    return overflow() + 1;
  }
  Object.defineProperty = overflow;
  assertDoesNotThrow(function() { Intl.Collator.supportedLocalesOf("en"); });

  var date = new Date(Date.UTC(2004, 12, 25, 3, 0, 0));
  var options = {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric"
  };

  Object.apply = overflow;
  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE", options); });

  var options_incomplete = {};
  assertDoesNotThrow(function() {
    date.toLocaleDateString("de-DE", options_incomplete);
  });
  assertFalse(options_incomplete.hasOwnProperty("year"));

  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE", undefined); });
  assertDoesNotThrow(function() { date.toLocaleDateString("de-DE"); });
  assertThrows(function() { date.toLocaleDateString("de-DE", null); }, TypeError);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0c3e70a^!)  
[src/i18n.js](https://cs.chromium.org/chromium/src/v8/src/i18n.js?cl=0c3e70a)  
[test/mjsunit/regress/regress-builtinbust-7.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-7.js?cl=0c3e70a)  
  
  
---   

## **regress-3294.js (v8 issue)**  
   
**[Issue: Inconsistent Error.stack behaviour](https://crbug.com/v8/3294)**  
**[Commit: Error stack getter should not overwrite itself with a data property.](https://chromium.googlesource.com/v8/v8/+/1a9649a)**  
  
Date(Commit): Mon Apr 28 12:14:36 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/258933007](https://codereview.chromium.org/258933007)  
Regress: [mjsunit/regress/regress-3294.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3294.js)  
```javascript
var e = new Error('message');
var keys = Object.keys(e);
e.stack;
assertEquals(keys, Object.keys(e));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1a9649a^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=1a9649a)  
[test/mjsunit/regress/regress-3294.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3294.js?cl=1a9649a)  
  

---   

## **regress-359441.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Deoptimizer::MaterializeHeapObjects](https://crbug.com/359441)**  
**[Commit: Fix materialization of accessor frames with captured receivers](https://chromium.googlesource.com/v8/v8/+/ff884e0)**  
  
Date(Commit): Fri Apr 25 12:58:15 2014  
Components/Type: Blink/Bug-Regression  
Labels: ["Te-Logged", "Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/225283006](https://codereview.chromium.org/225283006)  
Regress: [mjsunit/regress/regress-359441.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-359441.js)  
```javascript
function g() {
  this.x = {};
}

function f() {
  new g();
}

function deopt(x) {
  %DeoptimizeFunction(f);
}

f();
f();
%OptimizeFunctionOnNextCall(f);
Object.prototype.__defineSetter__('x', deopt);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ff884e0^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=ff884e0)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=ff884e0)  
[test/mjsunit/regress/regress-359441.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-359441.js?cl=ff884e0)  
  

---   

## **regress-escape-preserve-smi-representation.js (other issue)**  
   
**[Commit: Preserve Smi representation of non-escaping fields.](https://chromium.googlesource.com/v8/v8/+/d557425)**  
  
Date(Commit): Fri Apr 25 11:29:02 2014  
Code Review: [https://codereview.chromium.org/251493004](https://codereview.chromium.org/251493004)  
Regress: [mjsunit/regress/regress-escape-preserve-smi-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-escape-preserve-smi-representation.js)  
```javascript
function deepEquals(a, b) {
  if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; }
  if (typeof a != typeof b) return false;
  if (typeof a == "number") return isNaN(a) && isNaN(b);
  if (typeof a !== "object" && typeof a !== "function") return false;
  if (objectClass === "RegExp") { return (a.toString() === b.toString()); }
  if (objectClass === "Function") return false;
  if (objectClass === "Array") {
    var elementsCount = 0;
    if (a.length != b.length) { return false; }
    for (var i = 0; i < a.length; i++) {
      if (!deepEquals(a[i], b[i])) return false;
    }
    return true;
  }
}


function __f_1(){
  var __v_0 = [];
  for(var i=0; i<2; i++){
    __v_0.push([])
    deepEquals(2, __v_0.length);
  }
}
__f_1();
%OptimizeFunctionOnNextCall(__f_1);
__f_1();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d557425^!)  
[src/hydrogen-escape-analysis.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-escape-analysis.cc?cl=d557425)  
[src/hydrogen-escape-analysis.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-escape-analysis.h?cl=d557425)  
[test/mjsunit/regress/regress-escape-preserve-smi-representation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-escape-preserve-smi-representation.js?cl=d557425)  
  
  
---   

## **regress-lazy-deopt-inlining2.js (other issue)**  
   
**[Commit: Don't adopt the AST id from previous if id is none, since previous may have mismatching expected stack height.](https://chromium.googlesource.com/v8/v8/+/d2179f2)**  
  
Date(Commit): Fri Apr 25 09:52:11 2014  
Code Review: [https://codereview.chromium.org/252583004](https://codereview.chromium.org/252583004)  
Regress: [mjsunit/regress/regress-lazy-deopt-inlining2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lazy-deopt-inlining2.js)  
```javascript
"use strict";
function f1(d) {
  return 1 + f2(1, f3(d), d);
}

function f2(v0, v1, v2) { return v1; }

function f3(d) {
  if (d) %DeoptimizeFunction(f1);
  return 2;
}

%NeverOptimizeFunction(f3);

f1(false);
f1(false);
%OptimizeFunctionOnNextCall(f1);
assertEquals(3, f1(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2179f2^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=d2179f2)  
[src/hydrogen-removable-simulates.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-removable-simulates.cc?cl=d2179f2)  
[test/mjsunit/regress/regress-lazy-deopt-inlining2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-lazy-deopt-inlining2.js?cl=d2179f2)  
  
  
---   

## **regress-lazy-deopt-inlining.js (other issue)**  
   
**[Commit: Mark the simulate before EnterInlined with BailoutId::None(), and set ReturnId on EnterInlined. When merging simulates into the simulate before enter-inlined, adopt the last AST id that gets merged into it.](https://chromium.googlesource.com/v8/v8/+/a55821e)**  
  
Date(Commit): Thu Apr 24 15:20:53 2014  
Code Review: [https://codereview.chromium.org/257583004](https://codereview.chromium.org/257583004)  
Regress: [mjsunit/regress/regress-lazy-deopt-inlining.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-lazy-deopt-inlining.js)  
```javascript
"use strict";
function f1(d) {
  return 1 + f2(f3(d));
}

function f2(v) { return v; }

function f3(d) {
  if (d) %DeoptimizeFunction(f1);
  return 2;
}

%NeverOptimizeFunction(f3);

f1(false);
f1(false);
%OptimizeFunctionOnNextCall(f1);
assertEquals(3, f1(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a55821e^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=a55821e)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=a55821e)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=a55821e)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=a55821e)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=a55821e)  
...  
  
  
---   

## **regress-365172-1.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/365172)**  
**[Commit: Make DescriptorArray::IsMoreGeneralThan() and DescriptorArray::Merge() compatible again.](https://chromium.googlesource.com/v8/v8/+/052f9e9)**  
  
Date(Commit): Thu Apr 24 08:07:14 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/255513005](https://codereview.chromium.org/255513005)  
Regress: [mjsunit/regress/regress-365172-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-365172-1.js), [mjsunit/regress/regress-365172-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-365172-2.js), [mjsunit/regress/regress-365172-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-365172-3.js)  
```javascript
var b1 = {d: 1}; var b2 = {d: 2};
var f1 = {x: 1}; var f2 = {x: 2};
f1.b = b1;
f2.x = {};
b2.d = 4.2;
f2.b = b2;
var x = f1.x;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/052f9e9^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=052f9e9)  
[test/mjsunit/regress/regress-365172-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-365172-1.js?cl=052f9e9)  
[test/mjsunit/regress/regress-365172-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-365172-2.js?cl=052f9e9)  
[test/mjsunit/regress/regress-365172-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-365172-3.js?cl=052f9e9)  
  

---   

## **regress-empty-fixed-double-array.js (other issue)**  
   
**[Commit: Fix C++ type of Factory::NewFixedDoubleArray.](https://chromium.googlesource.com/v8/v8/+/8c57b45)**  
  
Date(Commit): Thu Apr 24 05:29:00 2014  
Code Review: [https://codereview.chromium.org/249593002](https://codereview.chromium.org/249593002)  
Regress: [mjsunit/regress/regress-empty-fixed-double-array.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-empty-fixed-double-array.js)  
```javascript
function f(a, x) {
  a.shift();
  a[0] = x;
}

f([1], 1.1);
f([1], 1.1);
%OptimizeFunctionOnNextCall(f);
f([1], 1.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c57b45^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=8c57b45)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=8c57b45)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=8c57b45)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=8c57b45)  
[test/mjsunit/regress/regress-empty-fixed-double-array.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-empty-fixed-double-array.js?cl=8c57b45)  
  
  
---   

## **regress-builtinbust-6.js (other issue)**  
   
**[Commit: Fix ToObject and Object.isSealed in four Array builtins.](https://chromium.googlesource.com/v8/v8/+/66ec299)**  
  
Date(Commit): Wed Apr 23 12:48:32 2014  
Code Review: [https://codereview.chromium.org/240223006](https://codereview.chromium.org/240223006)  
Regress: [mjsunit/regress/regress-builtinbust-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-6.js)  
```javascript
var values = [ 23, 4.2, true, false, 0/0 ];
for (var i = 0; i < values.length; ++i) {
  var v = values[i];
  Array.prototype.join.call(v);
  Array.prototype.pop.call(v);
  Array.prototype.push.call(v);
  Array.prototype.reverse.call(v);
  Array.prototype.shift.call(v);
  Array.prototype.slice.call(v);
  Array.prototype.splice.call(v);
  Array.prototype.unshift.call(v);
}

var length_receiver, element_receiver;
function length() { length_receiver = this; return 2; }
function element() { element_receiver = this; return "x"; }
Object.defineProperty(Number.prototype, "length", { get:length, set:length });
Object.defineProperty(Number.prototype, "0", { get:element, set:element });
Object.defineProperty(Number.prototype, "1", { get:element, set:element });
Object.defineProperty(Number.prototype, "2", { get:element, set:element });
function test_receiver(expected, call_string) {
  assertDoesNotThrow(call_string);
  assertEquals(new Number(expected), length_receiver);
  assertSame(length_receiver, element_receiver);
}

test_receiver(11, "Array.prototype.join.call(11)")
test_receiver(23, "Array.prototype.pop.call(23)");
test_receiver(42, "Array.prototype.push.call(42, 'y')");
test_receiver(49, "Array.prototype.reverse.call(49)");
test_receiver(65, "Array.prototype.shift.call(65)");
test_receiver(77, "Array.prototype.slice.call(77, 1)");
test_receiver(88, "Array.prototype.splice.call(88, 1, 1)");
test_receiver(99, "Array.prototype.unshift.call(99, 'z')");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/66ec299^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=66ec299)  
[test/mjsunit/regress/regress-builtinbust-6.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-6.js?cl=66ec299)  
  
  
---   

## **regress-363956.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(field_access.representation().IsHeapObject()) failed: ../../v8/src/hydrogen.cc(5418)](https://crbug.com/363956)**  
**[Commit: Clear invalid field maps in PropertyAccessInfo.](https://chromium.googlesource.com/v8/v8/+/63a477b)**  
  
Date(Commit): Wed Apr 16 09:48:32 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/239623005](https://codereview.chromium.org/239623005)  
Regress: [mjsunit/regress/regress-363956.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-363956.js)  
```javascript
function Fuu() { this.x = this.x.x; }
Fuu.prototype.x = {x: 1}
new Fuu();
new Fuu();
%OptimizeFunctionOnNextCall(Fuu);
new Fuu();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63a477b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=63a477b)  
[test/mjsunit/regress/regress-363956.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-363956.js?cl=63a477b)  
  

---   

## **regress-builtinbust-5.js (other issue)**  
   
**[Commit: Fix bogus call to Object.hasOwnProperty in Array builtin.](https://chromium.googlesource.com/v8/v8/+/e51d646)**  
  
Date(Commit): Tue Apr 15 12:52:41 2014  
Code Review: [https://codereview.chromium.org/239033002](https://codereview.chromium.org/239033002)  
Regress: [mjsunit/regress/regress-builtinbust-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-5.js)  
```javascript
var a = [ 1, 2, 3 ];
var was_called = false;
function poison() { was_called = true; }
a.hasOwnProperty = poison;
Object.freeze(a);

assertThrows("a.unshift()", TypeError);
assertEquals(3, a.length);
assertFalse(was_called);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e51d646^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=e51d646)  
[test/mjsunit/regress/regress-builtinbust-5.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-5.js?cl=e51d646)  
  
  
---   

## **regress-builtinbust-4.js (other issue)**  
   
**[Commit: Fix bogus Object.isSealed check in some Array builtins.](https://chromium.googlesource.com/v8/v8/+/39137c8)**  
  
Date(Commit): Tue Apr 15 08:25:42 2014  
Code Review: [https://codereview.chromium.org/237253002](https://codereview.chromium.org/237253002)  
Regress: [mjsunit/regress/regress-builtinbust-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-4.js)  
```javascript
var o = { __proto__:Array.prototype, 0:"x" };
function boomer() { return 0; }
Object.defineProperty(o, "length", { get:boomer, set:boomer });
Object.seal(o);

assertDoesNotThrow(function() { o.push(1); });
assertEquals(0, o.length);
assertEquals(1, o[0]);

assertDoesNotThrow(function() { o.unshift(2); });
assertEquals(0, o.length);
assertEquals(2, o[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39137c8^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=39137c8)  
[test/mjsunit/regress/regress-builtinbust-4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-4.js?cl=39137c8)  
  
  
---   

## **regress-362128.js (chromium issue)**  
   
**[Issue: ProxyResolverV8Test.JavascriptLibrary crashes when built with v8_target_arch=arm64](https://crbug.com/362128)**  
**[Commit: Fix result of LCodeGen::DoWrapReceiver for strict functions and builtins.](https://chromium.googlesource.com/v8/v8/+/8b445aa)**  
  
Date(Commit): Mon Apr 14 11:58:18 2014  
Components/Type: Internals/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/226363007](https://codereview.chromium.org/226363007)  
Regress: [mjsunit/regress/regress-362128.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-362128.js)  
```javascript
function genM() {
  "use strict";
  return function () {
    return this.field;
  };
}

function genR() {
  var x = {
    field: 10
  }
  return x;
}

method = {};
receiver = {};

method = genM("A");
receiver = genR("A");

var foo = (function () {
  return function suspect (name) {
    "use strict";
    return method.apply(receiver, arguments);
  }
})();

foo("a", "b", "c");
foo("a", "b", "c");
foo("a", "b", "c");
%OptimizeFunctionOnNextCall(foo);
foo("a", "b", "c");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8b445aa^!)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=8b445aa)  
[test/mjsunit/regress/regress-362128.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-362128.js?cl=8b445aa)  
  

---   

## **regress-353058.js (chromium issue)**  
   
**[Issue: Heap-buffer-overflow in v8::internal::Simulator::DecodeType2](https://crbug.com/353058)**  
**[Commit: Check stack limit in ArgumentAdaptorTrampoline.](https://chromium.googlesource.com/v8/v8/+/4268ce0)**  
  
Date(Commit): Fri Apr 11 13:39:19 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_severity-None", "Nag", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/215853005](https://codereview.chromium.org/215853005)  
Regress: [mjsunit/regress/regress-353058.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-353058.js)  
```javascript
function runNearStackLimit(f) { function t() { try { t(); } catch(e) { f(); } }; try { t(); } catch(e) {} }
function __f_0(
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x,
  x, x, x, x, x, x, x, x, x, x, x, x, x, x, x, x
) { }
runNearStackLimit(__f_0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4268ce0^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=4268ce0)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=4268ce0)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=4268ce0)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=4268ce0)  
[src/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/builtins-mips.cc?cl=4268ce0)  
...  
  

---   

## **regress-360733.js (chromium issue)**  
   
**[Issue: Heap-buffer-overflow in v8::internal::Simulator::HandleRList](https://crbug.com/360733)**  
**[Commit: Do not call user defined getter of Error.stackTraceLimit.](https://chromium.googlesource.com/v8/v8/+/49d951d)**  
  
Date(Commit): Fri Apr 11 13:16:36 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_severity-None", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/233243005](https://codereview.chromium.org/233243005)  
Regress: [mjsunit/regress/regress-360733.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-360733.js)  
```javascript
function f(a) {
  f(a + 1);
}

Error.__defineGetter__('stackTraceLimit', function() { });
try {
  f(0);
} catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49d951d^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=49d951d)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=49d951d)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=49d951d)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=49d951d)  
[test/mjsunit/regress/regress-360733.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-360733.js?cl=49d951d)  
  

---   

## **regress-no-dummy-use-for-arguments-object.js (other issue)**  
   
**[Commit: There is no definition for HArgumentsObject, so LDummyUse confuses the register allocator. I have recently made similar fix for HCapturedObject (see https://codereview.chromium.org/222283002/).](https://chromium.googlesource.com/v8/v8/+/fd98833)**  
  
Date(Commit): Fri Apr 11 06:29:51 2014  
Code Review: [https://codereview.chromium.org/222283002/](https://codereview.chromium.org/222283002/)  
Regress: [mjsunit/regress/regress-no-dummy-use-for-arguments-object.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-no-dummy-use-for-arguments-object.js)  
```javascript
function g() {
  arguments.length;
}

var global = "";

function f() {
  global.dummy = this;
  g({});
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fd98833^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=fd98833)  
[test/mjsunit/regress/regress-no-dummy-use-for-arguments-object.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-no-dummy-use-for-arguments-object.js?cl=fd98833)  
  
  
---   

## **regress-361608.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/361608)**  
**[Commit: Do not use ranges after range analysis.](https://chromium.googlesource.com/v8/v8/+/5bddec0)**  
  
Date(Commit): Thu Apr 10 09:40:17 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-35", "Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Severity-Medium", "Security_Impact-Beta", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/232553004](https://codereview.chromium.org/232553004)  
Regress: [mjsunit/regress/regress-361608.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-361608.js)  
```javascript
function f() {};
int_array = [1];

function foo() {
  var x;
  for (var i = -1; i < 0; i++) {
    x = int_array[i + 1];
    f(function() { x = i; });
  }
}

foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5bddec0^!)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=5bddec0)  
[test/mjsunit/regress/regress-361608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-361608.js?cl=5bddec0)  
  

---   

## **regress-359491.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!right->IsConstant() || (!HConstant::cast(right)->HasInteger32Value() || HConstant::cast(right](https://crbug.com/359491)**  
**[Commit: Avoid hydrogen compare-objects-equal assertions in dead code](https://chromium.googlesource.com/v8/v8/+/57d70c1)**  
  
Date(Commit): Wed Apr 09 13:08:28 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-33", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/228883005](https://codereview.chromium.org/228883005)  
Regress: [mjsunit/regress/regress-359491.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-359491.js)  
```javascript
(function () {
  function f(a, b, mode) {
    if (mode) {
      return a === b;
    } else {
      return a === b;
    }
  }

  // Gather type feedback for both branches.
  f("a", "b", 1);
  f("c", "d", 1);
  f("a", "b", 0);
  f("c", "d", 0);

  function g(mode) {
    var x = 1e10 | 0;
    f(x, x, mode);
  }

  // Gather type feedback for g, but only on one branch for f.
  g(1);
  g(1);
  %OptimizeFunctionOnNextCall(g);
  // Optimize g, which inlines f. Both branches in f will see the constant.
  g(0);
})();

(function () {
  function f(a, b, mode) {
    if (mode) {
      return a === b;
    } else {
      return a === b;
    }
  }

  // Gather type feedback for both branches.
  f({ a : 1}, {b : 1}, 1);
  f({ c : 1}, {d : 1}, 1);
  f({ a : 1}, {c : 1}, 0);
  f({ b : 1}, {d : 1}, 0);

  function g(mode) {
    var x = 1e10 | 0;
    f(x, x, mode);
  }

  // Gather type feedback for g, but only on one branch for f.
  g(1);
  g(1);
  %OptimizeFunctionOnNextCall(g);
  // Optimize g, which inlines f. Both branches in f will see the constant.
  g(0);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/57d70c1^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=57d70c1)  
[test/mjsunit/regress/regress-359491.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-359491.js?cl=57d70c1)  
  

---   

## **regress-parseint.js (other issue)**  
   
**[Commit: Fix argument expectation Runtime_StringParseInt.](https://chromium.googlesource.com/v8/v8/+/4df132a)**  
  
Date(Commit): Wed Apr 09 12:33:51 2014  
Code Review: [https://codereview.chromium.org/230693002](https://codereview.chromium.org/230693002)  
Regress: [mjsunit/regress/regress-parseint.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-parseint.js)  
```javascript
function f(string, radix) {
  // Use a phi to force radix into heap number representation.
  radix = (radix == 0) ? radix : (radix >> 0);
  if (radix != 2) return NaN;
  return %StringParseInt(string, radix);
}

assertEquals(2, (-4294967294) >> 0);
assertEquals(3, f("11", -4294967294));
assertEquals(NaN, f("11", -2147483650));
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f("11", -4294967294));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4df132a^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4df132a)  
[test/mjsunit/regress/regress-parseint.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-parseint.js?cl=4df132a)  
  
  
---   

## **regress-builtinbust-3.js (other issue)**  
   
**[Commit: Fix return value of push() and unshift() on Array.prototype.](https://chromium.googlesource.com/v8/v8/+/e3aec7a)**  
  
Date(Commit): Wed Apr 09 09:14:56 2014  
Code Review: [https://codereview.chromium.org/230453002](https://codereview.chromium.org/230453002)  
Regress: [mjsunit/regress/regress-builtinbust-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-3.js)  
```javascript
function produce_object() {
  var real_length = 1;
  function set_length() { real_length = "boom"; }
  function get_length() { return real_length; }
  var o = { __proto__:Array.prototype , 0:"x" };
  Object.defineProperty(o, "length", { set:set_length, get:get_length })
  return o;
}

assertEquals(2, produce_object().push("y"));
assertEquals(2, produce_object().unshift("y"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e3aec7a^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=e3aec7a)  
[test/mjsunit/regress/regress-builtinbust-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-3.js?cl=e3aec7a)  
  
  
---   

## **regress-inline-getter-near-stack-limit.js (other issue)**  
   
**[Commit: Add stack overflow check for inlined property getter](https://chromium.googlesource.com/v8/v8/+/05670b6)**  
  
Date(Commit): Wed Apr 09 07:35:12 2014  
Code Review: [https://codereview.chromium.org/220813003](https://codereview.chromium.org/220813003)  
Regress: [mjsunit/regress/regress-inline-getter-near-stack-limit.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-getter-near-stack-limit.js)  
```javascript
function runNearStackLimit(f) {
  function t() {
    try { t(); } catch(e) { f(); }
  };
  try { t(); } catch(e) {}
}

function g(x) { return x.bar; }
function f1() { }
function f2() { }

var x = Object.defineProperty({}, "bar", { get: f1 });
g(x);
g(x);
var y = Object.defineProperty({}, "bar", { get: f2 });
g(y);
%OptimizeFunctionOnNextCall(g);
runNearStackLimit(function() { g(y); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/05670b6^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=05670b6)  
[test/mjsunit/regress/regress-inline-getter-near-stack-limit.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-inline-getter-near-stack-limit.js?cl=05670b6)  
  
  
---   

## **regress-361025.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(p->IsHeapObject()) failed: ../../v8/src/objects-debug.cc(224)](https://crbug.com/361025)**  
**[Commit: Fix invalid local property lookup for transitions.](https://chromium.googlesource.com/v8/v8/+/48e0d81)**  
  
Date(Commit): Tue Apr 08 09:36:04 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/224903023](https://codereview.chromium.org/224903023)  
Regress: [mjsunit/regress/regress-361025.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-361025.js)  
```javascript
var x = new Object();
x.__defineGetter__('a', function() { return 7 });
JSON.parse('{"a":2600753951}');
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/48e0d81^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=48e0d81)  
[test/mjsunit/regress/regress-361025.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-361025.js?cl=48e0d81)  
  

---   

## **regress-3255.js (v8 issue)**  
   
**[Issue: Grow KeyedStoreIC doesn't respect String value wrappers](https://crbug.com/v8/3255)**  
**[Commit: Fix for v8:3255 Grow KeyedStoreIC doesn't respect String value wrappers](https://chromium.googlesource.com/v8/v8/+/eaacd96)**  
  
Date(Commit): Mon Apr 07 07:52:24 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/226053002](https://codereview.chromium.org/226053002)  
Regress: [mjsunit/regress/regress-3255.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3255.js)  
```javascript
var arr = [];
var str = new String('x');

function f(a,b) {
  a[b] = 1;
}

f(arr, 0);
f(str, 0);
f(str, 0);

%SetKeyedProperty(str, 1, 'y');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eaacd96^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=eaacd96)  
[test/mjsunit/regress/regress-3255.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3255.js?cl=eaacd96)  
  

---   

## **regress-359525.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(size_in_bytes <= kMaxBlockSize) failed: ../src/spaces.cc(2378)](https://crbug.com/359525)**  
**[Commit: Make sure value is a heap number when reusing the double box in BinaryOpICStub.](https://chromium.googlesource.com/v8/v8/+/5230d8d)**  
  
Date(Commit): Fri Apr 04 08:46:49 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-35", "M-34", "Release-1-M34", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/216823005](https://codereview.chromium.org/216823005)  
Regress: [mjsunit/regress/regress-359525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-359525.js)  
```javascript
var a;
for (var i = 0; i < 2; i++) {
  var x = 42 + a - {};
  print(x);
  a = "";
}

var b = 1.4;
var val = 0;
var o = {valueOf:function() { val++; return 10; }};
for (var i = 0; i < 2; i++) {
  var x = (b + i) + o;
  b = "";
}
assertEquals(val, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5230d8d^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=5230d8d)  
[test/mjsunit/regress/regress-359525.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-359525.js?cl=5230d8d)  
  

---   

## **regress-builtinbust-1.js (other issue)**  
   
**[Commit: Use premordial Object.isSealed/isFrozen in builtins.](https://chromium.googlesource.com/v8/v8/+/775d9b0)**  
  
Date(Commit): Thu Apr 03 12:23:35 2014  
Code Review: [https://codereview.chromium.org/223473002](https://codereview.chromium.org/223473002)  
Regress: [mjsunit/regress/regress-builtinbust-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtinbust-1.js)  
```javascript
function nope() { return false; }
var a = [ 1, 2, 3 ];
Object.seal(a);
Object.isSealed = nope;

assertThrows(function() { a.pop(); }, TypeError);
assertThrows(function() { a.push(5); }, TypeError);
assertThrows(function() { a.shift(); }, TypeError);
assertThrows(function() { a.unshift(5); }, TypeError);
assertThrows(function() { a.splice(0, 1); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/775d9b0^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=775d9b0)  
[test/mjsunit/regress/regress-builtinbust-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtinbust-1.js?cl=775d9b0)  
  
  
---   

## **regress-freeze-setter.js (other issue)**  
   
**[Commit: When freezing global object, go through the property cell](https://chromium.googlesource.com/v8/v8/+/fe37026)**  
  
Date(Commit): Thu Apr 03 10:43:56 2014  
Code Review: [https://codereview.chromium.org/223613002](https://codereview.chromium.org/223613002)  
Regress: [mjsunit/regress/regress-freeze-setter.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-freeze-setter.js)  
```javascript
Object.defineProperty(this, 'x', {set: function() { }});
Object.freeze(this);
eval('"use strict"; x = 20;');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fe37026^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=fe37026)  
[test/mjsunit/regress/regress-global-freeze-const.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-global-freeze-const.js?cl=fe37026)  
  
  
---   

## **regress-captured-object-no-dummy-use.js (other issue)**  
   
**[Commit: Do not generate LDummyUse instruction for HCapturedObject](https://chromium.googlesource.com/v8/v8/+/42d2d3c)**  
  
Date(Commit): Thu Apr 03 07:35:13 2014  
Code Review: [https://codereview.chromium.org/222283002](https://codereview.chromium.org/222283002)  
Regress: [mjsunit/regress/regress-captured-object-no-dummy-use.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-captured-object-no-dummy-use.js)  
```javascript
var global = "10.1";
function f() { }
function g(a) { this.d = a; }
function h() {
  var x = new f();
  global.dummy = this;
  var y = new g(x);
}
h();
h();
%OptimizeFunctionOnNextCall(h);
h();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/42d2d3c^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=42d2d3c)  
[test/mjsunit/regress/regress-captured-object-no-dummy-use.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-captured-object-no-dummy-use.js?cl=42d2d3c)  
  
  
---   

## **regress-alloc-smi-check.js (other issue)**  
   
**[Commit: Check in Lithium that allocation size in Smi range.](https://chromium.googlesource.com/v8/v8/+/0b53ed2)**  
  
Date(Commit): Thu Apr 03 07:04:46 2014  
Code Review: [https://codereview.chromium.org/221743005](https://codereview.chromium.org/221743005)  
Regress: [mjsunit/regress/regress-alloc-smi-check.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-alloc-smi-check.js)  
```javascript
var x = {};

function f(a) {
  a[200000000] = x;
}

f(new Array(100000));
f([]);
%OptimizeFunctionOnNextCall(f);
f([]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b53ed2^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=0b53ed2)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=0b53ed2)  
[test/mjsunit/regress/regress-alloc-smi-check.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-alloc-smi-check.js?cl=0b53ed2)  
  
  
---   

## **regress-crbug-357052.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!CpuFeatures::IsSupported(SSE2)) failed: ../../v8/src/ic.cc(2318)](https://crbug.com/357052)**  
**[Commit: Fix HGraphBuilder::BuildAddStringLengths](https://chromium.googlesource.com/v8/v8/+/511edab)**  
  
Date(Commit): Wed Apr 02 12:24:42 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/222113002](https://codereview.chromium.org/222113002)  
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
  

---   

## **regress-357054.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(to_kind == DICTIONARY_ELEMENTS) failed: ../../v8/src/objects.cc(3255)](https://crbug.com/357054)**  
**[Commit: Support typed arrays in IsMoreGeneralElementsKindTransition.](https://chromium.googlesource.com/v8/v8/+/19c354b)**  
  
Date(Commit): Tue Apr 01 16:41:35 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-35", "Merge-Merged", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/220403004](https://codereview.chromium.org/220403004)  
Regress: [mjsunit/regress/regress-357054.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-357054.js)  
```javascript
[].__defineSetter__(0, function() { });
function f(a,i,v) { a[i] = v; }
a = [0,0,0];
f(a,0,5);
a = new Float32Array(5);
f(a,2,5.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19c354b^!)  
[src/elements-kind.cc](https://cs.chromium.org/chromium/src/v8/src/elements-kind.cc?cl=19c354b)  
[test/mjsunit/regress/regress-357054.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-357054.js?cl=19c354b)  
  

---   

## **regress-358059.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/358059)**  
**[Commit: Smi immediates are not supported on x64. Do not use it.](https://chromium.googlesource.com/v8/v8/+/6490100)**  
  
Date(Commit): Tue Apr 01 15:32:06 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Release-0-M34", "M-34", "Merge-Merged", "Security_Impact-Stable", "Arch-x86_64", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/217083003](https://codereview.chromium.org/217083003)  
Regress: [mjsunit/regress/regress-358059.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-358059.js)  
```javascript
function f(a, b) { return b + (a.x++); }
var o = {};
o.__defineGetter__('x', function() { return 1; });
assertEquals(4, f(o, 3));
assertEquals(4, f(o, 3));
%OptimizeFunctionOnNextCall(f);
assertEquals(4, f(o, 3));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6490100^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=6490100)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=6490100)  
[test/mjsunit/regress/regress-358059.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-358059.js?cl=6490100)  
  

---   

## **regress-358088.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(STANDARD_STORE == KeyedStoreIC::GetKeyedAccessStoreMode(extra_ic_state)) failed: ../../v8/src/](https://crbug.com/358088)**  
**[Commit: Monomorphic prototype failures should be reserved for already-seen keys.](https://chromium.googlesource.com/v8/v8/+/d93c906)**  
  
Date(Commit): Tue Apr 01 14:16:54 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-34", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/219313002](https://codereview.chromium.org/219313002)  
Regress: [mjsunit/regress/regress-358088.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-358088.js)  
```javascript
function f(a) {
  a[a.length] = 1;
}

function g(a, i, v) {
  a[i] = v;
}

f([]);    // f KeyedStoreIC goes to 1.GROW
o = {};
g(o);     // We've added property "undefined" to o

o = {};   // A transition on property "undefined" exists from {}
f(o);     // Store should go generic.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d93c906^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=d93c906)  
[src/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic.h?cl=d93c906)  
[test/mjsunit/regress/regress-358088.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-358088.js?cl=d93c906)  
  

---   

## **regress-357103.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!target->IsConsString()) failed: ../../v8/src/x64/assembler-x64-inl.h(323)](https://crbug.com/357103)**  
**[Commit: Remove internalized cons string types.](https://chromium.googlesource.com/v8/v8/+/10abff3)**  
  
Date(Commit): Tue Apr 01 11:30:31 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/218993011](https://codereview.chromium.org/218993011)  
Regress: [mjsunit/regress/regress-357103.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-357103.js)  
```javascript
%SetAllocationTimeout(1, 1);

var key = "Huckleberry Finn" + "Tom Sawyer";
var o = {};
function f() { o[key] = "Adventures"; }

f();
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10abff3^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=10abff3)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=10abff3)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=10abff3)  
[src/types.cc](https://cs.chromium.org/chromium/src/v8/src/types.cc?cl=10abff3)  
[test/mjsunit/regress/regress-357103.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-357103.js?cl=10abff3)  
  

---   

## **regress-crbug-357330.js (chromium issue)**  
   
**[Issue: CHECK fail in ../../v8/src/list-inl.h while watching Youtube video.](https://crbug.com/357330)**  
**[Commit: Fix Type::Intersect to skip uninhabited bitsets](https://chromium.googlesource.com/v8/v8/+/282a7ca)**  
  
Date(Commit): Mon Mar 31 15:53:21 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-35", "Merge-Merged", "ReleaseBlock-Stable"]  
Code Review: [https://codereview.chromium.org/219333003](https://codereview.chromium.org/219333003)  
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
  

---   

## **regress-358057.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Simulator::DecodeType3](https://crbug.com/358057)**  
**[Commit: Fix PrepareKeyedOperand on arm.](https://chromium.googlesource.com/v8/v8/+/b3148d9)**  
  
Date(Commit): Mon Mar 31 15:14:28 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-35", "Stability-Memory-AddressSanitizer", "CVE-2014-3152", "Release-0-M35", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "Arch-ARM", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/219473002](https://codereview.chromium.org/219473002)  
Regress: [mjsunit/regress/regress-358057.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-358057.js)  
```javascript
__v_0 = new Uint8ClampedArray(10);
for (var i = 0; i < 10; i++) {
  __v_0[i] = 0xAA;
}
function __f_12(__v_6) {
  if (__v_6 < 0) {
    __v_1 = __v_0[__v_6 + 10];
    return __v_1;
  }
}

assertEquals(0xAA, __f_12(-1));
%OptimizeFunctionOnNextCall(__f_12);
assertEquals(0xAA, __f_12(-1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b3148d9^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=b3148d9)  
[test/mjsunit/regress/regress-358057.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-358057.js?cl=b3148d9)  
  

---   

## **regress-358090.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!heap->lo_space()->Contains(elms)) failed: ../src/builtins.cc(224)](https://crbug.com/358090)**  
**[Commit: Fix left trimming check for large objects](https://chromium.googlesource.com/v8/v8/+/d02e1f2)**  
  
Date(Commit): Mon Mar 31 15:01:46 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["MovedFrom-36", "MovedFrom-35", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/213833008](https://codereview.chromium.org/213833008)  
Regress: [mjsunit/regress/regress-358090.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-358090.js)  
```javascript
var x = Array(100000);
y =  Array.apply(Array, x);
y.unshift(4);
y.shift();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d02e1f2^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=d02e1f2)  
[test/mjsunit/regress/regress-358090.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-358090.js?cl=d02e1f2)  
  

---   

## **regress-crbug-357137.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/357137)**  
**[Commit: Do not check for interrupt when allocating stack locals.](https://chromium.googlesource.com/v8/v8/+/c0fa861)**  
  
Date(Commit): Mon Mar 31 14:14:54 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/219373004](https://codereview.chromium.org/219373004)  
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
...  
  

---   

## **regress-load-field-by-index.js (other issue)**  
   
**[Commit: Fix LoadFieldByIndex to take mutable heap-numbers into account.](https://chromium.googlesource.com/v8/v8/+/55a6318)**  
  
Date(Commit): Mon Mar 31 11:59:29 2014  
Code Review: [https://codereview.chromium.org/213213002](https://codereview.chromium.org/213213002)  
Regress: [mjsunit/regress/regress-load-field-by-index.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-load-field-by-index.js)  
```javascript
var o = {a:1.5, b:{}};

function f(o) {
  var result = [];
  for (var k in o) {
    result[result.length] = o[k];
  }
  return result;
}

f(o);
f(o);
%OptimizeFunctionOnNextCall(f);
var array = f(o);
o.a = 1.7;
assertEquals(1.5, array[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55a6318^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=55a6318)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=55a6318)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=55a6318)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=55a6318)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=55a6318)  
...  
  
  
---   

## **regress-357105.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(safe_to_deopt_topmost_optimized_code) failed: ../../v8/src/deoptimizer.cc(435)](https://crbug.com/357105)**  
**[Commit: Add missing lazy deopt point for the TransitionElementsKind instruction.](https://chromium.googlesource.com/v8/v8/+/d65fe51)**  
  
Date(Commit): Mon Mar 31 11:58:53 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/216963002](https://codereview.chromium.org/216963002)  
Regress: [mjsunit/regress/regress-357105.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-357105.js)  
```javascript
var global = { };

function do_nothing() { }

function f(opt_gc) {
  var x = new Array(3);
  x[0] = 10;
  opt_gc();
  global[1] = 15.5;
  return x;
}

gc();
global = f(gc);
global = f(do_nothing);
%OptimizeFunctionOnNextCall(f);
global = f(do_nothing);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d65fe51^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=d65fe51)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=d65fe51)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=d65fe51)  
[src/arm64/lithium-arm64.h](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.h?cl=d65fe51)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=d65fe51)  
...  
  

---   

## **regress-enum-prop-keys-cache-size.js (other issue)**  
   
**[Commit: With this fix, we only create the enum cache for own property descriptors (originally we cached all descriptors in the map).  The problem was that the size of all descriptors could be trimmed during GC triggered by allocating the storage for the cache, so we could have ended up with a wrong storage size.](https://chromium.googlesource.com/v8/v8/+/4608bde)**  
  
Date(Commit): Thu Mar 27 15:33:06 2014  
Code Review: [https://codereview.chromium.org/212673011](https://codereview.chromium.org/212673011)  
Regress: [mjsunit/regress/regress-enum-prop-keys-cache-size.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-enum-prop-keys-cache-size.js)  
```javascript
%SetAllocationTimeout(100000, 100000);

var x = {};
x.a = 1;
x.b = 2;
x = {};

var y = {};
y.a = 1;

%SetAllocationTimeout(100000, 0);

for (var z in y) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4608bde^!)  
[src/handles.cc](https://cs.chromium.org/chromium/src/v8/src/handles.cc?cl=4608bde)  
[test/mjsunit/regress/regress-enum-prop-keys-cache-size.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-enum-prop-keys-cache-size.js?cl=4608bde)  
  
  
---   

## **regress-357108.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(value->IsUndefined()) failed: ../../v8/src/objects-inl.h(3840)](https://crbug.com/357108)**  
**[Commit: Fix JSObject::SetElement for fixed typed array elements.](https://chromium.googlesource.com/v8/v8/+/4cdfb46)**  
  
Date(Commit): Thu Mar 27 12:54:26 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-35", "Merge-Merged", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/214543003](https://codereview.chromium.org/214543003)  
Regress: [mjsunit/regress/regress-357108.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-357108.js)  
```javascript
function TestArray(constructor) {
  function Check(a) {
    a[0] = "";
    assertEquals(0, a[0]);
    a[0] = {};
    assertEquals(0, a[0]);
    a[0] = { valueOf : function() { return 27; } };
    assertEquals(27, a[0]);
  }
  Check(new constructor(1));
  Check(new constructor(100));
}

TestArray(Uint8Array);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4cdfb46^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4cdfb46)  
[test/mjsunit/regress/regress-357108.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-357108.js?cl=4cdfb46)  
  

---   

## **regress-is-smi-repr.js (other issue)**  
   
**[Commit: Fix missing representation for the result of HIsSmiAndBranch.](https://chromium.googlesource.com/v8/v8/+/10606aa)**  
  
Date(Commit): Wed Mar 26 13:14:08 2014  
Code Review: [https://codereview.chromium.org/211273010](https://codereview.chromium.org/211273010)  
Regress: [mjsunit/regress/regress-is-smi-repr.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-is-smi-repr.js)  
```javascript
"use strict";

var global;

function g() { global = this; }
Object.defineProperty(Number.prototype, "prop", { get: g });
function f(s) { s.prop; }

f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/10606aa^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=10606aa)  
[test/mjsunit/regress/regress-is-smi-repr.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-is-smi-repr.js?cl=10606aa)  
  
  
---   

## **regress-dictionary-to-fast-arguments.js (other issue)**  
   
**[Commit: Don't convert dictionary sloppy arguments to fast double mode.](https://chromium.googlesource.com/v8/v8/+/c432f71)**  
  
Date(Commit): Tue Mar 25 14:14:58 2014  
Code Review: [https://codereview.chromium.org/207683006](https://codereview.chromium.org/207683006)  
Regress: [mjsunit/regress/regress-dictionary-to-fast-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-dictionary-to-fast-arguments.js)  
```javascript
function f(a, b) {
  for (var i = 10000; i > 0; i--) {
    arguments[i] = 0;
  }
}

f(1.5, 2.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c432f71^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c432f71)  
[test/mjsunit/regress/regress-dictionary-to-fast-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-dictionary-to-fast-arguments.js?cl=c432f71)  
  
  
---   

## **regress-355523.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(index >= 0) failed: ../../v8/src/x64/codegen-x64.cc(697)](https://crbug.com/355523)**  
**[Commit: Add index check in DoAccessArgumentsAt.](https://chromium.googlesource.com/v8/v8/+/cb0f49c)**  
  
Date(Commit): Tue Mar 25 13:26:41 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/210053003](https://codereview.chromium.org/210053003)  
Regress: [mjsunit/regress/regress-355523.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-355523.js)  
```javascript
function __f_4(a, b) { }
function __f_8(n) { return __f_4(arguments[13], arguments[-10]); }
function __f_6(a) { return __f_8(0, a); }
__f_8(0);
__f_8(0);
%OptimizeFunctionOnNextCall(__f_8);
__f_8(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb0f49c^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=cb0f49c)  
[test/mjsunit/regress/regress-355523.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-355523.js?cl=cb0f49c)  
  

---   

## **regress-356053.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::OptimizingCompilerThread::Unblock](https://crbug.com/356053)**  
**[Commit: Fix issues when changing FLAG_concurrent_recompilation after init.](https://chromium.googlesource.com/v8/v8/+/793d4cb)**  
  
Date(Commit): Tue Mar 25 09:38:48 2014  
Components/Type: None/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/210363005](https://codereview.chromium.org/210363005)  
Regress: [mjsunit/regress/regress-356053.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-356053.js)  
```javascript
gc();
try { %UnblockConcurrentRecompilation(); } catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/793d4cb^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=793d4cb)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=793d4cb)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=793d4cb)  
[test/mjsunit/regress/regress-356053.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-356053.js?cl=793d4cb)  
  

---   

## **regress-355486.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(argument_count_immediate_ + receiver > 0) failed: ../../v8/src/x64/codegen-x64.cc(705)](https://crbug.com/355486)**  
**[Commit: Fix to get around an assertion that triggers when generating code that happens to be dead because the assertion is checked a bit earlier at runtime.](https://chromium.googlesource.com/v8/v8/+/56f2006)**  
  
Date(Commit): Mon Mar 24 20:51:36 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/201573011](https://codereview.chromium.org/201573011)  
Regress: [mjsunit/regress/regress-355486.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-355486.js)  
```javascript
function f() { var v = arguments[0]; }
function g() { f(); }

g();
g();
%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56f2006^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=56f2006)  
[test/mjsunit/regress/regress-355486.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-355486.js?cl=56f2006)  
  

---   

## **regress-store-heapobject.js (other issue)**  
   
**[Commit: Ensure the constant operand for heap-object store-named-field is not a smi.](https://chromium.googlesource.com/v8/v8/+/e18e650)**  
  
Date(Commit): Mon Mar 24 16:25:48 2014  
Code Review: [https://codereview.chromium.org/210193002](https://codereview.chromium.org/210193002)  
Regress: [mjsunit/regress/regress-store-heapobject.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-heapobject.js)  
```javascript
var o = {a: undefined};

function store(o, v) {
  o.a = v;
}

store(o, undefined);
store(o, undefined);

function f(bool) {
  var o = {a: undefined};
  if (bool) {
    store(o, 1);
  }
  return o;
}

f(false);
f(false);
%OptimizeFunctionOnNextCall(f);
f(true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e18e650^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=e18e650)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=e18e650)  
[test/mjsunit/regress/regress-store-heapobject.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-store-heapobject.js?cl=e18e650)  
  
  
---   

## **regress-355485.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(index >= 0 && index < length() && value <= kMaxOneByteCharCode) failed: ../../v8/src/objects-i](https://crbug.com/355485)**  
**[Commit: Correctly convert micro-sign to its upper case.](https://chromium.googlesource.com/v8/v8/+/9c0f5be)**  
  
Date(Commit): Mon Mar 24 14:16:14 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/209323007](https://codereview.chromium.org/209323007)  
Regress: [mjsunit/regress/regress-355485.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-355485.js)  
```javascript
assertEquals("\u039c", "\u00b5".toUpperCase());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c0f5be^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=9c0f5be)  
[test/mjsunit/regress/regress-355485.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-355485.js?cl=9c0f5be)  
  

---   

## **regress-354357.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!done() || handler_ == __null) failed: ../src/frames.cc(120)](https://crbug.com/354357)**  
**[Commit: Visit return statement of inlined function in value context.](https://chromium.googlesource.com/v8/v8/+/fc2563f)**  
  
Date(Commit): Fri Mar 21 12:14:44 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/206413005](https://codereview.chromium.org/206413005)  
Regress: [mjsunit/regress/regress-354357.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-354357.js)  
```javascript
var v = {};
function inlined() {
  return !(v.bar++);
}
function outer() {
  inlined();
};

outer();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fc2563f^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=fc2563f)  
[test/mjsunit/regress/regress-354357.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-354357.js?cl=fc2563f)  
  

---   

## **regress-354433.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(instr->IsLdrLiteralX()) failed: ../src/a64/assembler-a64-inl.h(582)](https://crbug.com/354433)**  
**[Commit: Ensure that lazy deopt sequence does not override calls.](https://chromium.googlesource.com/v8/v8/+/f20a947)**  
  
Date(Commit): Fri Mar 21 11:02:15 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/198463006](https://codereview.chromium.org/198463006)  
Regress: [mjsunit/regress/regress-354433.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-354433.js)  
```javascript
var __v_0 = {};
var __v_5 = {};
function __f_2() {
  this.__defineGetter__('str', function() { return __f_2(this); });
  this.str = "1";
  this.toString = function() {
    return this.str;
  };
};

__v_5 = new __f_2();
__v_0 = new __f_2();

function __f_5(fun,a,b) {
  __v_5.str = a;
  __v_0.str = b;
  fun(__v_5, __v_0);
}

function __f_8(a,b) { return a%b };

__f_5(__f_8, 1 << 30, 1);
__f_5(__f_8, 1, 1 << 30);
%OptimizeFunctionOnNextCall(__f_8);
__f_5(__f_8, 1, 1 << 30);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f20a947^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=f20a947)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=f20a947)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=f20a947)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=f20a947)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=f20a947)  
...  
  

---   

## **regress-crbug-354391.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(IsFastElementsKind(elements_kind) || IsExternalArrayElementsKind(elements_kind)) failed: ../sr](https://crbug.com/354391)**  
**[Commit: Fix polymorphic hydrogen handling of SLOPPY_ARGUMENTS_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/2b722b6)**  
  
Date(Commit): Thu Mar 20 16:25:24 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/206073008](https://codereview.chromium.org/206073008)  
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
  

---   

## **regress-353551.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(obj->IsHeapObject()) failed: ../src/incremental-marking.cc(84)](https://crbug.com/353551)**  
**[Commit: A64: Fix write barrier input in KeyedStoreIC::GenerateSloppyArguments.](https://chromium.googlesource.com/v8/v8/+/41eab25)**  
  
Date(Commit): Thu Mar 20 08:32:58 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/204453002](https://codereview.chromium.org/204453002)  
Regress: [mjsunit/regress/regress-353551.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-353551.js)  
```javascript
var depth = 0;
function __f_3(x) {
  var __v_1 = arguments;
  __v_1[1000] = 123;
  depth++;
  if (depth > 2200) return;
  function __f_4() {
    ++__v_1[0];
    __f_3(0.5);
  };
  __f_4();
}
__f_3(0.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/41eab25^!)  
[src/a64/ic-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/ic-a64.cc?cl=41eab25)  
[test/mjsunit/regress/regress-353551.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-353551.js?cl=41eab25)  
  

---   

## **regress-crbug-350867.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(elements_kind == DICTIONARY_ELEMENTS) failed: ../src/stub-cache.cc(1309)](https://crbug.com/350867)**  
**[Commit: Fix polymorphic keyed loads for SLOPPY_ARGUMENTS_ELEMENTS](https://chromium.googlesource.com/v8/v8/+/d9b6b64)**  
  
Date(Commit): Wed Mar 19 15:49:29 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/203303010](https://codereview.chromium.org/203303010)  
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
  

---   

## **regress-352982.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(object->map()->IsMap()) failed: ../src/heap-inl.h(818)](https://crbug.com/352982)**  
**[Commit: Fix TransitionElementsKindStub to handle non-JSArray objects correctly.](https://chromium.googlesource.com/v8/v8/+/487ca9e)**  
  
Date(Commit): Tue Mar 18 13:29:29 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-merged-1750", "Merge-Merged-1847", "Release-0-M34", "M-34", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/196343023](https://codereview.chromium.org/196343023)  
Regress: [mjsunit/regress/regress-352982.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-352982.js)  
```javascript
function __f_4(i1) {
  return __v_3[i1] * __v_3[0];
}
function __f_3(i1) {
  __f_4(i1);
  __f_4(i1 + 16);
  __f_4(i1 + 32);
  %OptimizeFunctionOnNextCall(__f_4);
  var x = __f_4(i1 + 993);
  return x;
}
function __f_5() {
  __v_3[0] = +__v_3[0];
  gc();
  __f_3(0) | 0;
  __v_3 = /\u23a1|x/;
  return 0;
}
var __v_3 = new Float32Array(1000);
__f_5();
__f_5();
__f_5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/487ca9e^!)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=487ca9e)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=487ca9e)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=487ca9e)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=487ca9e)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=487ca9e)  
...  
  

---   

## **regress-353004.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/353004)**  
**[Commit: Apply numeric casts correctly in typed arrays and related code.](https://chromium.googlesource.com/v8/v8/+/849187e)**  
  
Date(Commit): Tue Mar 18 10:23:50 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/201873005](https://codereview.chromium.org/201873005)  
Regress: [mjsunit/regress/regress-353004.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-353004.js)  
```javascript
var buffer1 = new ArrayBuffer(100 * 1024);

assertThrows(function() {
  var array1 = new Uint8Array(buffer1, {valueOf : function() {
    %ArrayBufferDetach(buffer1);
    return 0;
  }});
}, TypeError);

var buffer2 = new ArrayBuffer(100 * 1024);

assertThrows(function() {
  var array2 = new Uint8Array(buffer2, 0, {valueOf : function() {
      %ArrayBufferDetach(buffer2);
      return 100 * 1024;
  }});
}, TypeError);

let convertedOffset = false;
let convertedLength = false;
assertThrows(() =>
  new Uint8Array(buffer1, {valueOf : function() {
      convertedOffset = true;
      return 0;
    }}, {valueOf : function() {
      convertedLength = true;
      %ArrayBufferDetach(buffer1);
      return 0;
  }}), TypeError);
assertTrue(convertedOffset);
assertTrue(convertedLength);

var buffer3 = new ArrayBuffer(100 * 1024 * 1024);
var dataView1 = new DataView(buffer3, {valueOf : function() {
  %ArrayBufferDetach(buffer3);
  return 0;
}});

assertEquals(0, dataView1.byteLength);

var buffer4 = new ArrayBuffer(100 * 1024);
assertThrows(function() {
  var dataView2 = new DataView(buffer4, 0, {valueOf : function() {
    %ArrayBufferDetach(buffer4);
    return 100 * 1024 * 1024;
  }});
}, RangeError);


var buffer5 = new ArrayBuffer(100 * 1024);
assertThrows(function() {
  buffer5.slice({valueOf : function() {
    %ArrayBufferDetach(buffer5);
    return 0;
  }}, 100 * 1024 * 1024);
}, TypeError);


var buffer7 = new ArrayBuffer(100 * 1024 * 1024);
assertThrows(function() {
  buffer7.slice(0, {valueOf : function() {
    %ArrayBufferDetach(buffer7);
    return 100 * 1024 * 1024;
  }});
}, TypeError);

var buffer9 = new ArrayBuffer(1024);
var array9 = new Uint8Array(buffer9);
assertThrows(() =>
  array9.subarray({valueOf : function() {
    %ArrayBufferDetach(buffer9);
    return 0;
  }}, 1024), TypeError);
assertEquals(0, array9.length);

var buffer11 = new ArrayBuffer(1024);
var array11 = new Uint8Array(buffer11);
assertThrows(() =>
  array11.subarray(0, {valueOf : function() {
    %ArrayBufferDetach(buffer11);
    return 1024;
  }}), TypeError);
assertEquals(0, array11.length);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/849187e^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=849187e)  
[src/arraybuffer.js](https://cs.chromium.org/chromium/src/v8/src/arraybuffer.js?cl=849187e)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=849187e)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=849187e)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=849187e)  
...  
  

---   

## **regress-keyed-store-global.js (other issue)**  
   
**[Commit: Don't generate keyed store ICs for global proxies.](https://chromium.googlesource.com/v8/v8/+/5aaa513)**  
  
Date(Commit): Mon Mar 17 17:19:39 2014  
Code Review: [https://codereview.chromium.org/197873025](https://codereview.chromium.org/197873025)  
Regress: [mjsunit/regress/regress-keyed-store-global.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-store-global.js)  
```javascript
function f(a) {
  for (var i = 0; i < 256; i++) a[i] = i;
}

f([]);
f([]);
f(this);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5aaa513^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=5aaa513)  
[test/mjsunit/regress/regress-keyed-store-global.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-keyed-store-global.js?cl=5aaa513)  
  
  
---   

## **regress-3220.js (v8 issue)**  
   
**[Issue: Date.toString: ReferenceError: cache is not defined](https://crbug.com/v8/3220)**  
**[Commit: Fix date cache in strict mode.](https://chromium.googlesource.com/v8/v8/+/e1e4071)**  
  
Date(Commit): Mon Mar 17 15:47:58 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/201753002](https://codereview.chromium.org/201753002)  
Regress: [mjsunit/regress/regress-3220.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3220.js)  
```javascript
String(new Date());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1e4071^!)  
[src/date.js](https://cs.chromium.org/chromium/src/v8/src/date.js?cl=e1e4071)  
[test/mjsunit/regress/regress-3220.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3220.js?cl=e1e4071)  
  

---   

## **regress-crbug-350890.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(debug_lookup.IsPropertyCallbacks() && !debug_lookup.IsReadOnly()) failed: ../src/ic.cc(1808)](https://crbug.com/350890)**  
**[Commit: Fixed spec violation of storing to length of a frozen object.](https://chromium.googlesource.com/v8/v8/+/3b257c3)**  
  
Date(Commit): Mon Mar 17 15:43:33 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/196653015](https://codereview.chromium.org/196653015)  
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
...  
  

---   

## **regress-crbug-352586.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK((check == ENABLE_INLINED_SMI_CHECK) ? (*jmp_address == Assembler::kJncShortOpcode || *jmp_addr](https://crbug.com/352586)**  
**[Commit: Fix ASSERT violation when BinaryOpIC::Transition recurses into itself](https://chromium.googlesource.com/v8/v8/+/e4a18df)**  
  
Date(Commit): Mon Mar 17 14:51:31 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/201313002](https://codereview.chromium.org/201313002)  
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
  

---   

## **regress-crbug-351658.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(op < BinaryOpIC::State::FIRST_TOKEN || op > BinaryOpIC::State::LAST_TOKEN) failed: ../src/type](https://crbug.com/351658)**  
**[Commit: Make invalid LHSs a parse-time (reference) error](https://chromium.googlesource.com/v8/v8/+/c3c185c)**  
  
Date(Commit): Mon Mar 17 10:21:01 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/200473003](https://codereview.chromium.org/200473003)  
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
...  
  

---   

## **regress-crbug-352929.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/352929)**  
**[Commit: Fix typo in r19923 (bounds check offset propagation)](https://chromium.googlesource.com/v8/v8/+/dc45852)**  
  
Date(Commit): Mon Mar 17 09:38:01 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-35", "Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/201303002](https://codereview.chromium.org/201303002)  
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
  

---   

## **regress-crbug-352058.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(old_entry->maps_->size() > 0) failed: ../src/hydrogen-check-elimination.cc(156)](https://crbug.com/352058)**  
**[Commit: Check elimination now sets known successor branch of HCompareObjectEqAndBranch (correctness fix).](https://chromium.googlesource.com/v8/v8/+/f77c51b)**  
  
Date(Commit): Mon Mar 17 09:11:38 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/196383018](https://codereview.chromium.org/196383018)  
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
  

---   

## **regress-migrate-callbacks.js (other issue)**  
   
**[Commit: Fix generalization with callbacks.](https://chromium.googlesource.com/v8/v8/+/0f2a324)**  
  
Date(Commit): Fri Mar 14 14:17:49 2014  
Code Review: [https://codereview.chromium.org/200173003](https://codereview.chromium.org/200173003)  
Regress: [mjsunit/regress/regress-migrate-callbacks.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-migrate-callbacks.js)  
```javascript
var o1 = {};
o1.x = 1
o1.y = 1.5
var o2 = {}
o2.x = 1.5;
o2.__defineSetter__('y', function(v) { });
o1.y;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f2a324^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0f2a324)  
[test/mjsunit/regress/regress-migrate-callbacks.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-migrate-callbacks.js?cl=0f2a324)  
  
  
---   

## **regress-351261.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(object_size <= Page::kMaxRegularHeapObjectSize) failed: ../src/ia32/macro-assembler-ia32.cc(15](https://crbug.com/351261)**  
**[Commit: Fix for issue 351261.](https://chromium.googlesource.com/v8/v8/+/11df4b8)**  
  
Date(Commit): Fri Mar 14 10:22:55 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/200113002](https://codereview.chromium.org/200113002)  
Regress: [mjsunit/regress/regress-351261.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-351261.js)  
```javascript
function store(a) {
  a[5000000] = 1;
}

function foo() {
  var __v_8 = new Object;
  var __v_7 = new Array(4999990);
  store(__v_8);
  store(__v_7);
}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/11df4b8^!)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=11df4b8)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=11df4b8)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=11df4b8)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=11df4b8)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=11df4b8)  
...  
  

---   

## **regress-350863.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(object->map()->IsMap()) failed: ../src/heap-inl.h(833)](https://crbug.com/350863)**  
**[Commit: Propagate updated offsets in BoundsCheckBbData.](https://chromium.googlesource.com/v8/v8/+/2c99cba)**  
  
Date(Commit): Fri Mar 14 10:02:25 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-merged-1750", "Merge-Merged-1847", "Release-0-M34", "M-34", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/197823009](https://codereview.chromium.org/197823009)  
Regress: [mjsunit/regress/regress-350863.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-350863.js)  
```javascript
var __v_7 = { };
function __f_8(base, condition) {
  __v_7[base + 3] = 0;
  __v_7[base + 4] = 0;
  if (condition) {
    __v_7[base + 0] = 0;
    __v_7[base + 5] = 0;
  } else {
    __v_7[base + 0] = 0;
    __v_7[base + 18] = 0;
  }
}
__f_8(1, true);
__f_8(1, false);
%OptimizeFunctionOnNextCall(__f_8);
__f_8(5, false);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2c99cba^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=2c99cba)  
[test/mjsunit/regress/regress-350863.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-350863.js?cl=2c99cba)  
  

---   

## **regress-3204.js (v8 issue)**  
   
**[Issue: Users of range analysis results assume SSI, but we have only "adhoc SSI in random places"](https://crbug.com/v8/3204)**  
**[Commit: Add regression test for range analysis bug.](https://chromium.googlesource.com/v8/v8/+/358e176)**  
  
Date(Commit): Fri Mar 14 09:54:26 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/200103002](https://codereview.chromium.org/200103002)  
Regress: [mjsunit/regress/regress-3204.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3204.js)  
```javascript
function SmiTaggingCanOverflow(x) {
 x = x | 0;
 if (x == 0) return;
 return x;
}

SmiTaggingCanOverflow(2147483647);
SmiTaggingCanOverflow(2147483647);
%OptimizeFunctionOnNextCall(SmiTaggingCanOverflow);
assertEquals(2147483647, SmiTaggingCanOverflow(2147483647));


function ModILeftCanBeNegative() {
  var x = 0;
  for (var i = -1; i < 0; ++i) x = i % 2;
  return x;
}

ModILeftCanBeNegative();
%OptimizeFunctionOnNextCall(ModILeftCanBeNegative);
assertEquals(-1, ModILeftCanBeNegative());


function ModIRightCanBeZero() {
  var x = 0;
  for (var i = -1; i <= 0; ++i) x = (2 % i) | 0;
  return x;
}

ModIRightCanBeZero();
%OptimizeFunctionOnNextCall(ModIRightCanBeZero);
ModIRightCanBeZero();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/358e176^!)  
[test/mjsunit/regress/regress-3204.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3204.js?cl=358e176)  
  

---   

## **regress-351624.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/351624)**  
**[Commit: Correctly retain argument value when deopting from Math.round on x64.](https://chromium.googlesource.com/v8/v8/+/0f71a24)**  
  
Date(Commit): Thu Mar 13 13:57:21 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/199013002](https://codereview.chromium.org/199013002)  
Regress: [mjsunit/regress/regress-351624.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-351624.js)  
```javascript
var big = 1e10;
var backup = new Float64Array(1);

function mult0(val){
  var prod = val * big;
  backup[0] = prod;
  var rounded = Math.round(prod);
  assertEquals(prod, backup[0]);
  return rounded;
}

var count = 5;
for (var i = 0; i < count; i++) {
  if (i == count - 1) %OptimizeFunctionOnNextCall(mult0);
  var result = mult0(-1);
  assertEquals(result, -big);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f71a24^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=0f71a24)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=0f71a24)  
[src/x64/lithium-x64.h](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.h?cl=0f71a24)  
[test/mjsunit/regress/regress-351624.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-351624.js?cl=0f71a24)  
  

---   

## **regress-351263.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(HasInteger32Value()) failed: ../src/hydrogen-instructions.h(3512)](https://crbug.com/351263)**  
**[Commit: Check that constant is an integer before getting its value in HGraphBuilder::MatchRotateRight.](https://chromium.googlesource.com/v8/v8/+/c64b78f)**  
  
Date(Commit): Thu Mar 13 11:50:50 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/197803005](https://codereview.chromium.org/197803005)  
Regress: [mjsunit/regress/regress-351263.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-351263.js)  
```javascript
var __v_12 = {};
function __f_30(x, sa) {
  return (x >>> sa) | (x << (__v_12 - sa));
}
__f_30(1.4, 1);
__f_30(1.4, 1);
%OptimizeFunctionOnNextCall(__f_30);
__f_30(1.4, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c64b78f^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=c64b78f)  
[test/mjsunit/regress/regress-351263.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-351263.js?cl=c64b78f)  
  

---   

## **regress-352059.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK_EQ(fixed_right_arg.value, c_right->Integer32Value()) failed: ../src/hydrogen.cc(9256)](https://crbug.com/352059)**  
**[Commit: Make translation of modulus operation '--stress-opt'-proof.](https://chromium.googlesource.com/v8/v8/+/390d3a0)**  
  
Date(Commit): Thu Mar 13 09:37:16 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/198833002](https://codereview.chromium.org/198833002)  
Regress: [mjsunit/regress/regress-352059.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-352059.js)  
```javascript
var foo = false;

function bar() {
  foo = 2;
  return 4 % foo;
}

bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/390d3a0^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=390d3a0)  
[test/mjsunit/regress/regress-352059.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-352059.js?cl=390d3a0)  
  

---   

## **regress-crbug-351787.js (chromium issue)**  
   
**[Issue: Pwnium 4: v8 OOB read/write with __defineGetter__ and bytesLength](https://crbug.com/351787)**  
**[Commit: Use intrinsics for builtin ArrayBuffer property accesses](https://chromium.googlesource.com/v8/v8/+/f9ee4f1)**  
  
Date(Commit): Wed Mar 12 19:25:40 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Release-3-M33", "ZDI-CAN-2233", "CVE-2014-1705", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "CVE_description-submitted", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/197793003](https://codereview.chromium.org/197793003)  
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
  

---   

## **regress-sort-arguments.js (other issue)**  
   
**[Commit: Don't fast RemoveArrayHoles in case of arguments arrays.](https://chromium.googlesource.com/v8/v8/+/8735adb)**  
  
Date(Commit): Wed Mar 12 13:42:18 2014  
Code Review: [https://codereview.chromium.org/197043004](https://codereview.chromium.org/197043004)  
Regress: [mjsunit/regress/regress-sort-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sort-arguments.js)  
```javascript
function f(a) { return arguments; }
var a = f(1,2,3);
delete a[1];
Array.prototype.sort.apply(a);
a[10000000] = 4;
Array.prototype.sort.apply(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8735adb^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=8735adb)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=8735adb)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=8735adb)  
[test/mjsunit/regress/regress-sort-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-sort-arguments.js?cl=8735adb)  
  
  
---   

## **regress-350884.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(store_mode == STANDARD_STORE || store_mode == STORE_AND_GROW_NO_TRANSITION || store_mode == ST](https://crbug.com/350884)**  
**[Commit: 350884: KeyedStoreIC miss didn't handle a transitioning case.](https://chromium.googlesource.com/v8/v8/+/7477bc3)**  
  
Date(Commit): Wed Mar 12 13:35:40 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/194623005](https://codereview.chromium.org/194623005)  
Regress: [mjsunit/regress/regress-350884.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-350884.js)  
```javascript
var obj = new Array(1);
obj[0] = 0;
obj[1] = 0;
function foo(flag_index) {
  obj[flag_index]++;
}

obj[-8] = 3;
foo(1);
foo(2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7477bc3^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=7477bc3)  
[test/mjsunit/regress/regress-350884.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-350884.js?cl=7477bc3)  
  

---   

## **regress-crbug-351320.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/351320)**  
**[Commit: Fix HIsSmiAndBranch::KnownSuccessorBlock() by deleting it](https://chromium.googlesource.com/v8/v8/+/105c1e0)**  
  
Date(Commit): Wed Mar 12 10:14:29 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-35", "Merge-na", "Stability-Memory-AddressSanitizer", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/196943002](https://codereview.chromium.org/196943002)  
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
  

---   

## **regress-351319.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(Smi::IsValid(value)) failed: ../src/objects-inl.h(1199)](https://crbug.com/351319)**  
**[Commit: Fix handling of polymorphic array accesses with constant index](https://chromium.googlesource.com/v8/v8/+/ae1669b)**  
  
Date(Commit): Wed Mar 12 10:11:38 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/196353004](https://codereview.chromium.org/196353004)  
Regress: [mjsunit/regress/regress-351319.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-351319.js)  
```javascript
function __f_0(a, base) {
  a[base] = 1;
  a[base] = -1749557862;
}
var __v_0 = new Array(1024);
var __v_1 = new Array(128);
__f_0(__v_0, 1);
__f_0(__v_1, -2);
%OptimizeFunctionOnNextCall(__f_0);
__f_0(__v_0, -2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae1669b^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=ae1669b)  
[src/property-details-inl.h](https://cs.chromium.org/chromium/src/v8/src/property-details-inl.h?cl=ae1669b)  
[src/property-details.h](https://cs.chromium.org/chromium/src/v8/src/property-details.h?cl=ae1669b)  
[test/mjsunit/regress/regress-351319.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-351319.js?cl=ae1669b)  
  

---   

## **regress-crbug-350434.js (chromium issue)**  
   
**[Issue: [LangFuzz] Crash with jump to invalid address](https://crbug.com/350434)**  
**[Commit: Fix lazy deopt after tagged binary ops](https://chromium.googlesource.com/v8/v8/+/8a1812f)**  
  
Date(Commit): Wed Mar 12 09:59:36 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["reward-2000", "Release-0-M34", "M-33", "Via-Wizard", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "CVE-2014-1721", "allpublic", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/194703008](https://codereview.chromium.org/194703008)  
Regress: [mjsunit/regress/regress-crbug-350434.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-350434.js)  
```javascript
function Ctor() {
  this.foo = 1;
}

var o = new Ctor();
var p = new Ctor();


function crash(o, timeout) {
  var s = "4000111222";  // Outside Smi range.
  %SetAllocationTimeout(100000, timeout);
  // This allocates a heap number, causing a GC, triggering lazy deopt.
  var end = s >>> 0;
  s = s.substring(0, end);
  // This creates a map dependency, which gives the GC a reason to trigger
  // a lazy deopt when that map dies.
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
...  
  

---   

## **regress-crbug-350864.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(index >= 0 && index < this->length()) failed: ../src/objects-inl.h(2124)](https://crbug.com/350864)**  
**[Commit: Fix issue with getOwnPropertySymbols and hidden properties](https://chromium.googlesource.com/v8/v8/+/85800ef)**  
  
Date(Commit): Tue Mar 11 16:46:35 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/192883005](https://codereview.chromium.org/192883005)  
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
  

---   

## **regress-crbug-351262.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!curr->IsAccessCheckNeeded()) failed: ../src/objects.cc(5919)](https://crbug.com/351262)**  
**[Commit: fix bad access check check](https://chromium.googlesource.com/v8/v8/+/62fc099)**  
  
Date(Commit): Tue Mar 11 15:12:47 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/195163002](https://codereview.chromium.org/195163002)  
Regress: [mjsunit/regress/regress-crbug-351262.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-351262.js)  
```javascript
for (var x in this) {};
JSON.stringify(this);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/62fc099^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=62fc099)  
[test/mjsunit/regress/regress-crbug-351262.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-351262.js?cl=62fc099)  
  

---   

## **regress-346587.js (chromium issue)**  
   
**[Issue: Under certain conditions involving new objects and setInterval, string equality breaks](https://crbug.com/346587)**  
**[Commit: Fix bug in constant folding object comparisons.](https://chromium.googlesource.com/v8/v8/+/6e15073)**  
  
Date(Commit): Tue Mar 11 13:34:01 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Merge-Merged-3.23.17.24", "TE-Verified-M35", "TE-Verified-35.0.1897.2", "Via-Wizard", "M-34", "Merge-Merged-3.24.35.16"]  
Code Review: [https://codereview.chromium.org/195063002](https://codereview.chromium.org/195063002)  
Regress: [mjsunit/regress/regress-346587.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-346587.js)  
```javascript
function bar(obj) {
  assertTrue(obj.x === 'baz');
}

function foo() {
  bar({ x : 'baz' });
}

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6e15073^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=6e15073)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=6e15073)  
[test/mjsunit/regress/regress-346587.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-346587.js?cl=6e15073)  
  

---   

## **regress-350887.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(new_length->IsSmi() || new_length->IsUndefined()) failed: ../src/elements.cc(1862)](https://crbug.com/350887)**  
**[Commit: Fix for 350887: CHECK failure on new_length->IsSmi()](https://chromium.googlesource.com/v8/v8/+/819d9f6)**  
  
Date(Commit): Tue Mar 11 10:30:10 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/194803002](https://codereview.chromium.org/194803002)  
Regress: [mjsunit/regress/regress-350887.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-350887.js)  
```javascript
var arr = [];
assertSame(0, arr.length);
assertSame(undefined, arr[0]);
Object.defineProperty(arr, '2501866687', { value: 4, configurable: false });
assertSame(2501866688, arr.length);
assertSame(undefined, arr[0]);
arr.length = 0;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/819d9f6^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=819d9f6)  
[test/mjsunit/regress/regress-350887.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-350887.js?cl=819d9f6)  
  

---   

## **regress-350865.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK('0' <= current() && current() <= '7') failed: ../src/parser.cc(4997)](https://crbug.com/350865)**  
**[Commit: Fix assertion in RegExp parser to correctly expect stack overflow.](https://chromium.googlesource.com/v8/v8/+/1634e7d)**  
  
Date(Commit): Mon Mar 10 15:52:10 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/192773002](https://codereview.chromium.org/192773002)  
Regress: [mjsunit/regress/regress-350865.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-350865.js)  
```javascript
/\2/.test("1");

function rec() {
  try {
    rec();
  } catch(e) {
    /\2/.test("1");
  }
}

rec();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1634e7d^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=1634e7d)  
[test/mjsunit/regress/regress-350865.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-350865.js?cl=1634e7d)  
  

---   

## **regress-store-global-proxy.js (other issue)**  
   
**[Commit: Reland and fix "Allow ICs to be generated for own global proxy."](https://chromium.googlesource.com/v8/v8/+/1180803)**  
  
Date(Commit): Mon Mar 10 12:23:05 2014  
Code Review: [https://codereview.chromium.org/176793003](https://codereview.chromium.org/176793003)  
Regress: [mjsunit/regress/regress-store-global-proxy.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-store-global-proxy.js)  
```javascript
delete Object.prototype.__proto__;

function f() {
  this.toString = 1;
}

f.apply({});
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1180803^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=1180803)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=1180803)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=1180803)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=1180803)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=1180803)  
...  
  
  
---   

## **regress-crbug-349465.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::JSFunction::context](https://crbug.com/349465)**  
**[Commit: Fix for failing asserts in HBoundsCheck code generation on x64: use proper cmp operation width instead of asserting that Integer32 values should be zero extended. Similar to chromium:345820.](https://chromium.googlesource.com/v8/v8/+/997ce05)**  
  
Date(Commit): Thu Mar 06 16:22:47 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-na", "Stability-Memory-AddressSanitizer", "Security_Severity-Medium", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/188703002](https://codereview.chromium.org/188703002)  
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
  

---   

## **regress-crbug-349878.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/349878)**  
**[Commit: Fix HConstants with Smi-ranged HeapNumber values](https://chromium.googlesource.com/v8/v8/+/1cc0baf)**  
  
Date(Commit): Thu Mar 06 16:21:09 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/186123003](https://codereview.chromium.org/186123003)  
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
  

---   

## **regress-349885.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(Smi::IsValid(value)) failed: ../src/objects-inl.h(1192)](https://crbug.com/349885)**  
**[Commit: Bugfix for 349874: we incorrectly believe we saw a growing store](https://chromium.googlesource.com/v8/v8/+/6115a00)**  
  
Date(Commit): Thu Mar 06 13:07:51 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/188643002](https://codereview.chromium.org/188643002)  
Regress: [mjsunit/regress/regress-349885.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-349885.js)  
```javascript
function foo(a) {
  a[292755462] = new Object();
}
foo(new Array(5));
foo(new Array(5));
%OptimizeFunctionOnNextCall(foo);
foo(new Array(10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6115a00^!)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=6115a00)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=6115a00)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=6115a00)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=6115a00)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=6115a00)  
...  
  

---   

## **regress-keyed-store-non-strict-arguments.js (other issue)**  
   
**[Commit: Only use the non-strict-arguments-stub if the store site is non-strict.](https://chromium.googlesource.com/v8/v8/+/cd6f3ef)**  
  
Date(Commit): Thu Mar 06 12:19:06 2014  
Code Review: [https://codereview.chromium.org/176843018](https://codereview.chromium.org/176843018)  
Regress: [mjsunit/regress/regress-keyed-store-non-strict-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-store-non-strict-arguments.js)  
```javascript
function args(arg) { return arguments; }
var a = args(false);

(function () {
  "use strict";
  a["const" + 0] = 0;
})();

(function () {
  "use strict";
  a[0] = 0;
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cd6f3ef^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=cd6f3ef)  
[test/mjsunit/regress-keyed-store-non-strict-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-keyed-store-non-strict-arguments.js?cl=cd6f3ef)  
  
  
---   

## **regress-crbug-349853.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!found) failed: ../src/lithium-allocator.cc(1381)](https://crbug.com/349853)**  
**[Commit: Let HTransitionElementsKind take part in RestoreActualValues phase](https://chromium.googlesource.com/v8/v8/+/5ea3f00)**  
  
Date(Commit): Thu Mar 06 12:13:49 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/183753005](https://codereview.chromium.org/183753005)  
Regress: [mjsunit/regress/regress-crbug-349853.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-349853.js)  
```javascript
var a = ["string"];
function funky(array) { return array[0] = 1; }
funky(a);

function crash() {
  var q = [0];
  // The failing ASSERT was only triggered when compiling for OSR.
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
  

---   

## **regress-force-representation.js (other issue)**  
   
**[Commit: Also delete force representations that have no uses.](https://chromium.googlesource.com/v8/v8/+/f913c3b)**  
  
Date(Commit): Thu Mar 06 09:47:27 2014  
Code Review: [https://codereview.chromium.org/187773002](https://codereview.chromium.org/187773002)  
Regress: [mjsunit/regress/regress-force-representation.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-force-representation.js)  
```javascript
function optimize(crankshaft_test) {
  crankshaft_test();
  crankshaft_test();
  %OptimizeFunctionOnNextCall(crankshaft_test);
  crankshaft_test();
}

function f() {
  var v1 = 0;
  var v2 = -0;
  var t = v2++;
  v2++;
  return Math.max(v2++, v1++);
}

optimize(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f913c3b^!)  
[src/hydrogen-representation-changes.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-representation-changes.cc?cl=f913c3b)  
[test/mjsunit/regress/regress-force-representation.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-force-representation.js?cl=f913c3b)  
  
  
---   

## **regress-348512.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Deoptimizer::MaterializeHeapObjects](https://crbug.com/348512)**  
**[Commit: Fix materialization of captured objects in adapted arguments.](https://chromium.googlesource.com/v8/v8/+/52fd520)**  
  
Date(Commit): Wed Mar 05 12:57:18 2014  
Components/Type: Blink>JavaScript>Compiler/Bug  
Labels: ["Stability-Crash", "MovedFrom-36", "MovedFrom-35", "Hotlist-Google"]  
Code Review: [https://codereview.chromium.org/183063006](https://codereview.chromium.org/183063006)  
Regress: [mjsunit/regress/regress-348512.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-348512.js)  
```javascript
function h(y) { assertEquals(42, y.u); }
function g() { h.apply(0, arguments); }
function f(x) { g({ u : x }); }

f(42);
f(42);
%OptimizeFunctionOnNextCall(f);
f(42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/52fd520^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=52fd520)  
[test/mjsunit/regress/regress-348512.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-348512.js?cl=52fd520)  
  

---   

## **regress-3183.js (v8 issue)**  
   
**[Issue: "Object #<Object> has no method push" on Array after deoptimization](https://crbug.com/v8/3183)**  
**[Commit: Deoptimization fix for HPushArgument.](https://chromium.googlesource.com/v8/v8/+/7ac668f)**  
  
Date(Commit): Wed Mar 05 12:45:46 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/178193026](https://codereview.chromium.org/178193026)  
Regress: [mjsunit/regress/regress-3183.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3183.js)  
```javascript
(function DeoptimizeArgCallFunctionGeneric() {
  var a = [];

  function f1(method, array, elem, deopt) {
    assertEquals('push', method);
  }

  function f2() { }

  function bar(x, deopt, f) {
    f('push', a, [x], deopt + 0);
  }

  function foo() { return bar(arguments[0], arguments[1], arguments[2]); }
  function baz(f, deopt) { return foo("x", deopt, f); }

  baz(f1, 0);
  baz(f2, 0);
  %OptimizeFunctionOnNextCall(baz);
  baz(f1, "deopt");
})();


(function DeoptimizeArgGlobalFunctionGeneric() {
  var a = [];

  var f1;

  f1 = function(method, array, elem, deopt) {
    assertEquals('push', method);
  }

  function bar(x, deopt, f) {
    f1('push', a, [x], deopt + 0);
  }

  function foo() { return bar(arguments[0], arguments[1]); }
  function baz(deopt) { return foo("x", deopt); }

  baz(0);
  baz(0);
  %OptimizeFunctionOnNextCall(baz);
  baz("deopt");
})();


(function DeoptimizeArgCallFunctionRuntime() {
  var a = [];

  var f1;

  f1 = function(method, array, elem, deopt) {
    assertEquals('push', method);
  }

  function bar(x, deopt) {
    %_Call(f1, null, 'push', [x][0], ((deopt + 0), 1));
  }

  function foo() { return bar(arguments[0], arguments[1]); }
  function baz(deopt) { return foo(0, deopt); }

  baz(0);
  baz(0);
  %OptimizeFunctionOnNextCall(baz);
  baz("deopt");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7ac668f^!)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=7ac668f)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=7ac668f)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=7ac668f)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=7ac668f)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=7ac668f)  
...  
  

---   

## **regress-crbug-349079.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/349079)**  
**[Commit: x64: Fix LMathMinMax for constant Smi right-hand operands](https://chromium.googlesource.com/v8/v8/+/3df5573)**  
  
Date(Commit): Wed Mar 05 09:49:07 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["CVE-2014-1704", "Stability-Memory-AddressSanitizer", "Release-2-M33", "M-33", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/186593003](https://codereview.chromium.org/186593003)  
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
  

---   

## **regress-348280.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(IsInitialized() && other.IsInitialized()) failed: ../src/unique.h(94)](https://crbug.com/348280)**  
**[Commit: Fix HCheckValue::Canonicalize wrt uninitialized HConstant unique.](https://chromium.googlesource.com/v8/v8/+/b1a271a)**  
  
Date(Commit): Tue Mar 04 08:08:08 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/183383006](https://codereview.chromium.org/183383006)  
Regress: [mjsunit/regress/regress-348280.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-348280.js)  
```javascript
function baz(f) { f(); }
function goo() {}
baz(goo);
baz(goo);

function bar(p) { if (p == 0) baz(1); }
bar(1);
bar(1);
%OptimizeFunctionOnNextCall(bar);
bar(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b1a271a^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=b1a271a)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=b1a271a)  
[test/mjsunit/regress/regress-348280.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-348280.js?cl=b1a271a)  
  

---   

## **regress-343609.js (chromium issue)**  
   
**[Issue: Chrome_Win: Crash Report: v8::internal::Deoptimizer::VisitAllOptimizedFunctionsForContext](https://crbug.com/343609)**  
**[Commit: Clear optimized code cache in shared function info when code gets deoptimized.](https://chromium.googlesource.com/v8/v8/+/b9e0b87)**  
  
Date(Commit): Mon Mar 03 11:11:39 2014  
Components/Type: Blink/Bug  
Labels: ["M-34"]  
Code Review: [https://codereview.chromium.org/184923002](https://codereview.chromium.org/184923002)  
Regress: [mjsunit/regress/regress-343609.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-343609.js)  
```javascript
function Ctor() {
  this.a = 1;
}

function get_closure() {
  return function add_field(obj) {
    obj.c = 3;
    obj.a = obj.a + obj.c;
    return obj.a;
  }
}
function get_closure2() {
  return function cc(obj) {
    obj.c = 3;
    obj.a = obj.a + obj.c;
  }
}

function dummy() {
  (function () {
    var o = {c: 10};
    var f1 = get_closure2();
    f1(o);
    f1(o);
    %OptimizeFunctionOnNextCall(f1);
    f1(o);
  })();
}

var o = new Ctor();
function opt() {
  (function () {
    var f1 = get_closure();
    f1(new Ctor());
    f1(new Ctor());
    %OptimizeFunctionOnNextCall(f1);
    f1(o);
  })();
}

opt();
opt();
opt();

dummy();
dummy();

for(var i = 0; i < 3; i++) gc(true);

o.c = 2.2;

var f2 = get_closure();
f2(new Ctor());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b9e0b87^!)  
[src/a64/deoptimizer-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/deoptimizer-a64.cc?cl=b9e0b87)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=b9e0b87)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=b9e0b87)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=b9e0b87)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=b9e0b87)  
...  
  

---   

## **regress-347914.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(this_entry->maps_->size() > 0) failed: ../src/hydrogen-check-elimination.cc(277)](https://crbug.com/347914)**  
**[Commit: Check elimination did not mark some dead blocks.](https://chromium.googlesource.com/v8/v8/+/c2601ae)**  
  
Date(Commit): Fri Feb 28 14:16:38 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/180483003](https://codereview.chromium.org/180483003)  
Regress: [mjsunit/regress/regress-347914.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347914.js)  
```javascript
// Copyright 2014 the V8 project authors. All rights reserved.


function MjsUnitAssertionError(message) {}
MjsUnitAssertionError.prototype.toString = function () { return this.message; };
var assertSame;
var assertEquals;
var assertEqualsDelta;
var assertArrayEquals;
var assertPropertiesEqual;
var assertToStringEquals;
var assertTrue;
var assertFalse;
var triggerAssertFalse;
var assertNull;
var assertNotNull;
var assertThrows;
var assertDoesNotThrow;
var assertInstanceof;
var assertUnreachable;
var assertOptimized;
var assertUnoptimized;
function classOf(object) { var string = Object.prototype.toString.call(object); return string.substring(8, string.length - 1); }
function PrettyPrint(value) { return ""; }
function PrettyPrintArrayElement(value, index, array) { return ""; }
function fail(expectedText, found, name_opt) { }
function deepObjectEquals(a, b) { var aProps = Object.keys(a); aProps.sort(); var bProps = Object.keys(b); bProps.sort(); if (!deepEquals(aProps, bProps)) { return false; } for (var i = 0; i < aProps.length; i++) { if (!deepEquals(a[aProps[i]], b[aProps[i]])) { return false; } } return true; }
function deepEquals(a, b) { if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(a) && isNaN(b); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(a); if (objectClass !== classOf(b)) return false; if (objectClass === "RegExp") { return (a.toString() === b.toString()); } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (!deepEquals(a[i], b[i])) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf() !== b.valueOf()) return false; } return deepObjectEquals(a, b); }
assertSame = function assertSame(expected, found, name_opt) { if (found === expected) { if (expected !== 0 || (1 / expected) == (1 / found)) return; } else if ((expected !== expected) && (found !== found)) { return; } fail(PrettyPrint(expected), found, name_opt); }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected), found, name_opt); } };
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) { assertTrue(Math.abs(expected - found) <= delta, name_opt); }; assertArrayEquals = function assertArrayEquals(expected, found, name_opt) { var start = ""; if (name_opt) { start = name_opt + " - "; } assertEquals(expected.length, found.length, start + "array length"); if (expected.length == found.length) { for (var i = 0; i < expected.length; ++i) { assertEquals(expected[i], found[i], start + "array element at index " + i); } } };
assertPropertiesEqual = function assertPropertiesEqual(expected, found, name_opt) { if (!deepObjectEquals(expected, found)) { fail(expected, found, name_opt); } };
assertToStringEquals = function assertToStringEquals(expected, found, name_opt) { if (expected != String(found)) { fail(expected, found, name_opt); } };
assertTrue = function assertTrue(value, name_opt) { assertEquals(true, value, name_opt); };
assertFalse = function assertFalse(value, name_opt) { assertEquals(false, value, name_opt); };

assertNull = function assertNull(value, name_opt) { if (value !== null) { fail("null", value, name_opt); } };
assertNotNull = function assertNotNull(value, name_opt) { if (value === null) { fail("not null", value, name_opt); } };
as1sertThrows = function assertThrows(code, type_opt, cause_opt) { var threwException = true; try { if (typeof code == 'function') { code(); } else { eval(code); } threwException = false; } catch (e) { if (typeof type_opt == 'function') { assertInstanceof(e, type_opt); } if (arguments.length >= 3) { assertEquals(e.type, cause_opt); } return; } };
assertInstanceof = function assertInstanceof(obj, type) { if (!(obj instanceof type)) { var actualTypeName = null; var actualConstructor = Object.getPrototypeOf(obj).constructor; if (typeof actualConstructor == "function") { actualTypeName = actualConstructor.name || String(actualConstructor); } fail("Object <" + PrettyPrint(obj) + "> is not an instance of <" + (type.name || type) + ">" + (actualTypeName ? " but of < " + actualTypeName + ">" : "")); } };
assertDoesNotThrow = function assertDoesNotThrow(code, name_opt) { try { if (typeof code == 'function') { code(); } else { eval(code); } } catch (e) { fail("threw an exception: ", e.message || e, name_opt); } };
assertUnreachable = function assertUnreachable(name_opt) { var message = "Fail" + "ure: unreachable"; if (name_opt) { message += " - " + name_opt; } };
var OptimizationStatus;
try { OptimizationStatus = new Function("fun", "sync", "return %GetOptimizationStatus(fun, sync);"); } catch (e) { OptimizationStatus = function() { } }
assertUnoptimized = function assertUnoptimized(fun, sync_opt, name_opt) { if (sync_opt === undefined) sync_opt = ""; assertTrue(OptimizationStatus(fun, sync_opt) != 1, name_opt); }
assertOptimized = function assertOptimized(fun, sync_opt, name_opt) { if (sync_opt === undefined) sync_opt = "";  assertTrue(OptimizationStatus(fun, sync_opt) != 2, name_opt); }
triggerAssertFalse = function() { }

var __v_1 = {};
var __v_2 = {};
var __v_3 = {};
var __v_4 = {};
var __v_5 = {};
var __v_6 = {};
var __v_7 = {};
var __v_8 = {};
var __v_9 = {};
var __v_10 = {};
var __v_0 = 'fisk';
assertEquals('fisk', __v_0);
var __v_0;
assertEquals('fisk', __v_0);
var __v_6 = 'hest';
assertEquals('hest', __v_0);
this.bar = 'fisk';
assertEquals('fisk', __v_1);
__v_1;
assertEquals('fisk', __v_1);
__v_1 = 'hest';
assertEquals('hest', __v_1);

function __f_0(o) {
  o.g();
  if (!o.g()) {
    assertTrue(false);
  }
}
__v_4 = {};
__v_4.size = function() { return 42; }
__v_4.g = function() { return this.size(); };
__f_0({g: __v_4.g, size:__v_4.size});
for (var __v_0 = 0; __v_0 < 5; __v_0++) __f_0(__v_4);
%OptimizeFunctionOnNextCall(__f_0);
__f_0(__v_4);
__f_0({g: __v_4.g, size:__v_4.size});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c2601ae^!)  
[src/hydrogen-check-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-check-elimination.cc?cl=c2601ae)  
[test/mjsunit/regress/regress-347914.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347914.js?cl=c2601ae)  
  

---   

## **regress-347906.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(HasInteger32Value()) failed: ../src/hydrogen-instructions.h(3474)](https://crbug.com/347906)**  
**[Commit: Fixed constant folding for Math.clz32.](https://chromium.googlesource.com/v8/v8/+/e927333)**  
  
Date(Commit): Fri Feb 28 13:07:10 2014  
Components/Type: None/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/184353002](https://codereview.chromium.org/184353002)  
Regress: [mjsunit/es6/regress/regress-347906.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-347906.js)  
```javascript
function foo() {
  return Math.clz32(12.34);
}

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e927333^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=e927333)  
[test/mjsunit/regress/regress-347906.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347906.js?cl=e927333)  
  

---   

## **regress-crbug-347903.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK((IsFastSmiOrObjectElementsKind(kind) && (map == GetHeap()->fixed_array_map() || map == GetHeap](https://crbug.com/347903)**  
**[Commit: A JSArray may have a filler map in the elements pointer.](https://chromium.googlesource.com/v8/v8/+/b1ffc79)**  
  
Date(Commit): Fri Feb 28 12:29:19 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/184493002](https://codereview.chromium.org/184493002)  
Regress: [mjsunit/regress/regress-crbug-347903.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-347903.js)  
```javascript
function f() {
  var a = new Array(84632);
  // Allocation folding will bail out trying to fold the elements alloc of
  // array "b."
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
  

---   

## **regress-sync-optimized-lists.js (other issue)**  
   
**[Commit: Evict from optimized code map in sync with removing from optimized functions list.](https://chromium.googlesource.com/v8/v8/+/5c186bb)**  
  
Date(Commit): Fri Feb 28 12:27:31 2014  
Code Review: [https://codereview.chromium.org/184443002](https://codereview.chromium.org/184443002)  
Regress: [mjsunit/regress/regress-sync-optimized-lists.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sync-optimized-lists.js)  
```javascript
function Ctor() {
  this.a = 1;
}

function get_closure() {
  return function add_field(obj, osr) {
    obj.c = 3;
    var x = 0;
    if (osr) %OptimizeOsr();
    for (var i = 0; i < 10; i++) {
      x = i + 1;
    }
    return x;
  }
}

var f1 = get_closure();
f1(new Ctor(), false);
f1(new Ctor(), false);

%OptimizeFunctionOnNextCall(f1, "concurrent");

var o = new Ctor();
f1(o, true);

%NotifyContextDisposed();

o.c = 2.2;

var f2 = get_closure();
f2(new Ctor(), true);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c186bb^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=5c186bb)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=5c186bb)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=5c186bb)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=5c186bb)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=5c186bb)  
...  
  
  
---   

## **regress-347912.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(number_of_own_descriptors > 0) failed: ../src/objects.h(6111)](https://crbug.com/347912)**  
**[Commit: Fix JSObject::PrintTransitions.](https://chromium.googlesource.com/v8/v8/+/70242fe)**  
  
Date(Commit): Fri Feb 28 11:41:07 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/183683005](https://codereview.chromium.org/183683005)  
Regress: [mjsunit/regress/regress-347912.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347912.js)  
```javascript
var __v_4 = {};
__v_2 = {};
__v_2[1024] = 0;
%DebugPrint(__v_4);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70242fe^!)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=70242fe)  
[test/mjsunit/regress/regress-347912.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347912.js?cl=70242fe)  
  

---   

## **regress-347909.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(value->IsHeapObject()) failed: ../src/objects-debug.cc(295)](https://crbug.com/347909)**  
**[Commit: Fix representation generalization for doubles.](https://chromium.googlesource.com/v8/v8/+/38ca262)**  
  
Date(Commit): Fri Feb 28 11:07:10 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["M-33", "M-34", "Merge-Merged", "Security_Impact-Stable", "Release-1-M33", "CVE-2013-6668", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/184393002](https://codereview.chromium.org/184393002)  
Regress: [mjsunit/regress/regress-347909.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347909.js)  
```javascript
var a = {y:1.5};
a.y = 0;
var b = a.y;
a.y = {};
var d = 1;
function f() {
  d = 0;
  return {y: b};
}
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/38ca262^!)  
[src/property-details.h](https://cs.chromium.org/chromium/src/v8/src/property-details.h?cl=38ca262)  
[test/mjsunit/regress/regress-347909.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347909.js?cl=38ca262)  
  

---   

## **regress-crbug-347528.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(IsNativeContext()) failed: ../src/contexts.h(462)](https://crbug.com/347528)**  
**[Commit: Get array_function from NativeContext](https://chromium.googlesource.com/v8/v8/+/98d1ced)**  
  
Date(Commit): Fri Feb 28 10:01:27 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_severity-None", "M-34", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/184173003](https://codereview.chromium.org/184173003)  
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
  

---   

## **regress-347904.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!right->IsConstant() || (!HConstant::cast(right)->HasInteger32Value() || HConstant::cast(right](https://crbug.com/347904)**  
**[Commit: Fix handling of constant global variable assignments.](https://chromium.googlesource.com/v8/v8/+/5945f9e)**  
  
Date(Commit): Fri Feb 28 09:40:12 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/184303003](https://codereview.chromium.org/184303003)  
Regress: [mjsunit/regress/regress-347904.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347904.js)  
```javascript
var v = /abc/;
function f() {
  v = 1578221999;
};
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5945f9e^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=5945f9e)  
[test/mjsunit/regress/regress-347904.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347904.js?cl=5945f9e)  
  

---   

## **regress-347542.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(!function->IsOptimized()) failed: ../src/runtime.cc(8623)](https://crbug.com/347542)**  
**[Commit: Removed bogus ASSERT.](https://chromium.googlesource.com/v8/v8/+/c4e90c1)**  
  
Date(Commit): Fri Feb 28 08:45:07 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/183763007](https://codereview.chromium.org/183763007)  
Regress: [mjsunit/regress/regress-347542.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347542.js)  
```javascript
function foo() {}
foo();
%OptimizeFunctionOnNextCall(foo);
foo();
%NeverOptimizeFunction(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c4e90c1^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=c4e90c1)  
[test/mjsunit/regress/regress-347542.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347542.js?cl=c4e90c1)  
  

---   

## **regress-347543.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(object_size <= Page::kMaxRegularHeapObjectSize) failed: ../src/ia32/macro-assembler-ia32.cc(15](https://crbug.com/347543)**  
**[Commit: HAllocate should never generate allocation code if the requested size does not fit into page. Regression test included.](https://chromium.googlesource.com/v8/v8/+/2ab83cf)**  
  
Date(Commit): Thu Feb 27 17:33:25 2014  
Components/Type: None/Bug-Security  
Labels: ["Merge-na", "M-33", "Security_Impact-Head", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/180803005](https://codereview.chromium.org/180803005)  
Regress: [mjsunit/regress/regress-347543.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347543.js)  
```javascript
function f(a) {
  a[5000000] = 256;
  assertEquals(256, a[5000000]);
}

var v1 = new Array(5000001);
var v2 = new Array(10);
f(v1);
f(v2);
f(v2);
%OptimizeFunctionOnNextCall(f);
f(v2);
f(v1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ab83cf^!)  
[src/a64/lithium-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/lithium-codegen-a64.cc?cl=2ab83cf)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=2ab83cf)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=2ab83cf)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=2ab83cf)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=2ab83cf)  
...  
  

---   

## **regress-put-prototype-transition.js (other issue)**  
   
**[Commit: Fix putting of prototype transitions. The length is also subject to GC, just like entry.](https://chromium.googlesource.com/v8/v8/+/aa14020)**  
  
Date(Commit): Thu Feb 27 16:07:44 2014  
Code Review: [https://codereview.chromium.org/183193003](https://codereview.chromium.org/183193003)  
Regress: [mjsunit/regress/regress-put-prototype-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-put-prototype-transition.js)  
```javascript
function deepEquals(a, b) { if (a === b) { if (a === 0) return (1 / a) === (1 / b); return true; } if (typeof a != typeof b) return false; if (typeof a == "number") return isNaN(a) && isNaN(b); if (typeof a !== "object" && typeof a !== "function") return false; var objectClass = classOf(a); if (objectClass !== classOf(b)) return false; if (objectClass === "RegExp") { return (a.toString() === b.toString()); } if (objectClass === "Function") return false; if (objectClass === "Array") { var elementCount = 0; if (a.length != b.length) { return false; } for (var i = 0; i < a.length; i++) { if (!deepEquals(a[i], b[i])) return false; } return true; } if (objectClass == "String" || objectClass == "Number" || objectClass == "Boolean" || objectClass == "Date") { if (a.valueOf() !== b.valueOf()) return false; } return deepObjectEquals(a, b); }
assertSame = function assertSame(expected, found, name_opt) { if (found === expected) { if (expected !== 0 || (1 / expected) == (1 / found)) return; } else if ((expected !== expected) && (found !== found)) { return; } fail(PrettyPrint(expected), found, name_opt); }; assertEquals = function assertEquals(expected, found, name_opt) { if (!deepEquals(found, expected)) { fail(PrettyPrint(expected), found, name_opt); } };
assertEqualsDelta = function assertEqualsDelta(expected, found, delta, name_opt) { assertTrue(Math.abs(expected - found) <= delta, name_opt); }; assertArrayEquals = function assertArrayEquals(expected, found, name_opt) { var start = ""; if (name_opt) { start = name_opt + " - "; } assertEquals(expected.length, found.length, start + "array length"); if (expected.length == found.length) { for (var i = 0; i < expected.length; ++i) { assertEquals(expected[i], found[i], start + "array element at index " + i); } } };
assertTrue = function assertTrue(value, name_opt) { assertEquals(true, value, name_opt); };
assertFalse = function assertFalse(value, name_opt) { assertEquals(false, value, name_opt); };

var __v_0 = {};
var __v_1 = {};
function __f_3() { }
function __f_4(obj) {
  for (var __v_2 = 0; __v_2 < 26; __v_2++) {
    obj["__v_5" + __v_2] = 0;
  }
}
function __f_0(__v_1, __v_6) {
    (new __f_3()).__proto__ = __v_1;
}
%DebugPrint(undefined);
function __f_1(__v_4, add_first, __v_6, same_map_as) {
  var __v_1 = __v_4 ? new __f_3() : {};
  assertTrue(__v_4 || %HasFastProperties(__v_1));
  if (add_first) {
    __f_4(__v_1);
    assertFalse(%HasFastProperties(__v_1));
    __f_0(__v_1, __v_6);
    assertFalse(%HasFastProperties(__v_1));
  } else {
    __f_0(__v_1, __v_6);
    assertTrue(__v_4 || !%HasFastProperties(__v_1));
    __f_4(__v_1);
    assertTrue(__v_4 || !%HasFastProperties(__v_1));
  }
}
gc();
for (var __v_2 = 0; __v_2 < 4; __v_2++) {
  var __v_6 = ((__v_2 & 2) != 7);
  var __v_4 = ((__v_2 & 2) != 0);
  __f_1(__v_4, true, __v_6);
  var __v_0 = __f_1(__v_4, false, __v_6);
  __f_1(__v_4, false, __v_6, __v_0);
}
__v_5 = {a: 1, b: 2, c: 3};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/aa14020^!)  
[src/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/mark-compact.cc?cl=aa14020)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=aa14020)  
[test/mjsunit/regress/regress-put-prototype-transition.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-put-prototype-transition.js?cl=aa14020)  
  
  
---   

## **regress-347262.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Map::instance_descriptors](https://crbug.com/347262)**  
**[Commit: Handle arguments objects in frame when materializing arguments](https://chromium.googlesource.com/v8/v8/+/05b9849)**  
  
Date(Commit): Thu Feb 27 15:12:12 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "M-34", "Merge-Merged", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/177293009](https://codereview.chromium.org/177293009)  
Regress: [mjsunit/regress/regress-347262.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347262.js)  
```javascript
(function ArgumentsObjectWithOtherArgumentsInFrame() {
  function g() {
    return g.arguments;
  }

  function f(x) {
    g();
    return arguments[0];
  }
  f();
  f();
  %OptimizeFunctionOnNextCall(f);
  f();
})();


(function ArgumentsObjectWithOtherArgumentsDeopt() {
  function g(y) {
    y.o2 = 2;
    return g.arguments;
  }

  function f(x) {
    var o1 = { o2 : 1 };
    var a = g(o1);
    o1.o2 = 3;
    return arguments[0] + a[0].o2;
  }
  f(0);
  f(0);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(3, f(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/05b9849^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=05b9849)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=05b9849)  
[test/mjsunit/regress/regress-347262.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347262.js?cl=05b9849)  
  

---   

## **regress-347530.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(object->elements()->IsFixedDoubleArray()) failed: ../src/objects.cc(12468)](https://crbug.com/347530)**  
**[Commit: Fix bogus assertion in SetFastDoubleElements.](https://chromium.googlesource.com/v8/v8/+/6912a24)**  
  
Date(Commit): Thu Feb 27 14:45:53 2014  
Components/Type: Blink>JavaScript/----  
Labels: ["Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/181433016](https://codereview.chromium.org/181433016)  
Regress: [mjsunit/regress/regress-347530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-347530.js)  
```javascript
a = [];
a[1000] = .1;
a.length = 0;
gc();
gc();
a[1000] = .1;
assertEquals(.1, a[1000]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6912a24^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=6912a24)  
[test/mjsunit/regress/regress-347530.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-347530.js?cl=6912a24)  
  

---   

## **regress-crbug-345820.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/345820)**  
**[Commit: Fix for failing asserts in HBoundsCheck code generation on x64: index register should be zero extended.](https://chromium.googlesource.com/v8/v8/+/1ae7e8a)**  
  
Date(Commit): Tue Feb 25 16:33:54 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Release-0-M34", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/180013002](https://codereview.chromium.org/180013002)  
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
  

---   

## **regress-crbug-346636.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Range::CanBeZero](https://crbug.com/346636)**  
**[Commit: Mark HCompareMap as having Tagged representation](https://chromium.googlesource.com/v8/v8/+/e7e93cd)**  
  
Date(Commit): Tue Feb 25 15:09:47 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/176923013](https://codereview.chromium.org/176923013)  
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
  

---   

## **regress-crbug-346141.js (chromium issue)**  
   
**[Issue: Global-buffer-overflow in GetVisitor](https://crbug.com/346141)**  
**[Commit: Fix crasher in Object.getOwnPropertySymbols](https://chromium.googlesource.com/v8/v8/+/63f1970)**  
  
Date(Commit): Tue Feb 25 12:01:34 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Security_severity-None", "Security_Impact-None", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/177883002](https://codereview.chromium.org/177883002)  
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
  

---   

## **regress-346343.js (chromium issue)**  
   
**[Issue: NO STACK](https://crbug.com/346343)**  
**[Commit: Don't eliminate loads with incompatible types or representations.](https://chromium.googlesource.com/v8/v8/+/77f597d)**  
  
Date(Commit): Tue Feb 25 09:55:50 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "M-34", "Merge-Merged", "Security_Severity-High", "Security_Impact-Beta", "allpublic", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/179553002](https://codereview.chromium.org/179553002)  
Regress: [mjsunit/regress/regress-346343.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-346343.js)  
```javascript
function f(o) {
  for (var i = 1; i < 2; ++i) {
    var y = o.y;
  }
}
f({y:1.1});
f({y:1.1});

function g(x) { f({z:x}); }
g(1);
g(2);
%OptimizeFunctionOnNextCall(g);
g(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77f597d^!)  
[src/hydrogen-load-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-load-elimination.cc?cl=77f597d)  
[test/mjsunit/regress/regress-346343.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-346343.js?cl=77f597d)  
  

---   

## **regress-crbug-345715.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HeapObject::map_word](https://crbug.com/345715)**  
**[Commit: Fix for a smi stores optimization on x64 with a regression test.](https://chromium.googlesource.com/v8/v8/+/6c1659b)**  
  
Date(Commit): Tue Feb 25 09:55:02 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["CVE-2014-1704", "Stability-Memory-AddressSanitizer", "Release-2-M33", "M-33", "M-34", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/178833002](https://codereview.chromium.org/178833002)  
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
  

---   

## **regress-cr-344285.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::Shell::RealmEval](https://crbug.com/344285)**  
**[Commit: negative bounds checking on realm calls](https://chromium.googlesource.com/v8/v8/+/cb05cff)**  
  
Date(Commit): Tue Feb 25 09:15:05 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Memory-AddressSanitizer", "MovedFrom-36", "MovedFrom-35", "Clusterfuzz"]  
Code Review: [https://codereview.chromium.org/169393002](https://codereview.chromium.org/169393002)  
Regress: [mjsunit/regress/regress-cr-344285.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cr-344285.js)  
```javascript
function __f_1(g) { return (g/-1) ^ 1; }
var __v_0 = 1 << 31;
var __v_2 = __f_1(__v_0);
caught = false;
try {
  Realm.eval(__v_2, "Realm.global(0).y = 1");
} catch (e) {
  caught = true;
}
assertTrue(caught, "exception not caught");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb05cff^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=cb05cff)  
[test/mjsunit/regress/regress-cr-344285.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-cr-344285.js?cl=cb05cff)  
  

---   

## **regress-3176.js (v8 issue)**  
   
**[Issue: Optimistic bounds check generalization leads to repetetive deoptimization](https://crbug.com/v8/3176)**  
**[Commit: Fix optimistic BCE to back off after deopt](https://chromium.googlesource.com/v8/v8/+/37b6fd0)**  
  
Date(Commit): Mon Feb 24 13:15:31 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/177523002](https://codereview.chromium.org/177523002)  
Regress: [mjsunit/regress/regress-3176.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3176.js)  
```javascript
function foo(a) {
  var sum = 0;
  for (var i = 0; i < 10; i++) {
    sum += a[i];

    if (i > 6) {
      sum -= a[i - 4];
      sum -= a[i - 5];
    }
  }
  return sum;
}

var a = new Int32Array(10);

foo(a);
foo(a);
%OptimizeFunctionOnNextCall(foo);
foo(a);
%OptimizeFunctionOnNextCall(foo);
foo(a);
assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37b6fd0^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=37b6fd0)  
[test/mjsunit/regress/regress-3176.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3176.js?cl=37b6fd0)  
  

---   

## **regress-fast-empty-string.js (other issue)**  
   
**[Commit: Don't turn objects with empty-string properties into fast-mode.](https://chromium.googlesource.com/v8/v8/+/84b3665)**  
  
Date(Commit): Thu Feb 20 16:11:48 2014  
Code Review: [https://codereview.chromium.org/165743003](https://codereview.chromium.org/165743003)  
Regress: [mjsunit/regress/regress-fast-empty-string.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fast-empty-string.js)  
```javascript
var o = {};
o[""] = 1;
var x = {__proto__:o};
for (i = 0; i < 3; i++) {
  o[""];
}
for (i = 0; i < 3; i++) {
  assertEquals(undefined, o.x);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84b3665^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=84b3665)  
[test/mjsunit/regress/regress-fast-empty-string.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-fast-empty-string.js?cl=84b3665)  
  
  
---   

## **regress-crbug-344186.js (chromium issue)**  
   
**[Issue: OOB write due to invalid bounds check in v8](https://crbug.com/344186)**  
**[Commit: Fix Hydrogen bounds check elimination](https://chromium.googlesource.com/v8/v8/+/6e3b81a)**  
  
Date(Commit): Wed Feb 19 10:30:39 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "M-33", "M-34", "Merge-Merged", "Security_Impact-Stable", "Release-1-M33", "CVE-2013-6668", "Security_Severity-High", "allpublic", "Clusterfuzz", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/172093002](https://codereview.chromium.org/172093002)  
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
  

---   

## **regress-lookup-transition.js (other issue)**  
   
**[Commit: Directly store the transition target on LookupResult in TransitionResult.](https://chromium.googlesource.com/v8/v8/+/60c08a8)**  
  
Date(Commit): Tue Feb 18 12:19:32 2014  
Code Review: [https://codereview.chromium.org/170343003](https://codereview.chromium.org/170343003)  
Regress: [mjsunit/es6/regress/regress-lookup-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-lookup-transition.js)  
```javascript
var proxy = new Proxy({}, { getOwnPropertyDescriptor:function() {
  gc();
}});

function f() { this.x = 23; }
f.prototype = proxy;
new f();
new f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/60c08a8^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=60c08a8)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=60c08a8)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=60c08a8)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=60c08a8)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=60c08a8)  
...  
  
  
---   

## **regress-3158.js (v8 issue)**  
   
**[Issue: A64: mjsunit/sparse-array-reverse fails on no_snap](https://crbug.com/v8/3158)**  
**[Commit: Fix dictionary element load to pass correct elements kind.](https://chromium.googlesource.com/v8/v8/+/6744ff6)**  
  
Date(Commit): Fri Feb 14 15:52:24 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/166653005](https://codereview.chromium.org/166653005)  
Regress: [mjsunit/regress/regress-3158.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3158.js)  
```javascript
Array.prototype[0] = 'a';
delete Array.prototype[0];

function foo(a, i) {
  return a[i];
}

var a = new Array(100000);
a[3] = 'x';

foo(a, 3);
foo(a, 3);
foo(a, 3);
%OptimizeFunctionOnNextCall(foo);
foo(a, 3);
Array.prototype[0] = 'a';
var z = foo(a, 0);
assertEquals('a', z);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6744ff6^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=6744ff6)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=6744ff6)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=6744ff6)  
[test/mjsunit/regress/regress-3158.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3158.js?cl=6744ff6)  
  

---   

## **regress-3138.js (v8 issue)**  
   
**[Issue: Empty variable statement ignored in with-statement](https://crbug.com/v8/3138)**  
**[Commit: Fix assignment of function name constant.](https://chromium.googlesource.com/v8/v8/+/68c7523)**  
  
Date(Commit): Fri Feb 14 12:40:47 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/159903008](https://codereview.chromium.org/159903008)  
Regress: [mjsunit/regress/regress-3138.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3138.js)  
```javascript
(function f(){
   assertEquals("function", typeof f);
})();

(function f(){
   var f;  // Variable shadows function name.
   assertEquals("undefined", typeof f);
})();

(function f(){
   var f;
   assertEquals("undefined", typeof f);
   with ({});  // Force context allocation of both variable and function name.
})();

assertEquals("undefined", typeof f);

(function() {
  var o = { a: 1 };
  with (o) {
    var a = 2;
  }
  assertEquals("undefined", typeof a);
  assertEquals(2, o.a);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68c7523^!)  
[src/a64/full-codegen-a64.cc](https://cs.chromium.org/chromium/src/v8/src/a64/full-codegen-a64.cc?cl=68c7523)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=68c7523)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=68c7523)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=68c7523)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=68c7523)  
...  
  

---   

## **regress-3159.js (v8 issue)**  
   
**[Issue: Garbled typed array error messages](https://crbug.com/v8/3159)**  
**[Commit: Fix typed array error message.](https://chromium.googlesource.com/v8/v8/+/a676bc1)**  
  
Date(Commit): Fri Feb 14 09:33:03 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/163293002](https://codereview.chromium.org/163293002)  
Regress: [mjsunit/regress/regress-3159.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3159.js)  
```javascript
try {
  new Uint32Array(new ArrayBuffer(1), 2, 3);
} catch (e) {
  assertEquals("start offset of Uint32Array should be a multiple of 4",
               e.message);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a676bc1^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=a676bc1)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=a676bc1)  
[test/mjsunit/regress/regress-3159.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3159.js?cl=a676bc1)  
  

---   

## **regress-check-eliminate-loop-phis.js (other issue)**  
   
**[Commit: Don't propagate information through phis in loop headers.](https://chromium.googlesource.com/v8/v8/+/7b7e365)**  
  
Date(Commit): Wed Feb 12 18:30:41 2014  
Code Review: [https://codereview.chromium.org/147023005](https://codereview.chromium.org/147023005)  
Regress: [mjsunit/regress/regress-check-eliminate-loop-phis.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-check-eliminate-loop-phis.js)  
```javascript
function f() {
  var o = {x:1};
  var y = {y:2.5, x:0};
  var result;
  for (var i = 0; i < 2; i++) {
    result = o.x + 3;
    o = y;
  }
  return result;
}

f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b7e365^!)  
[src/hydrogen-check-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-check-elimination.cc?cl=7b7e365)  
[test/mjsunit/regress/regress-check-eliminate-loop-phis.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-check-eliminate-loop-phis.js?cl=7b7e365)  
  
  
---   

## **regress-3135.js (v8 issue)**  
   
**[Issue: JSON.stringify() executes getters multiple times when replacer array contains duplicates](https://crbug.com/v8/3135)**  
**[Commit: Fix spec violations in JSON.stringify wrt replacer array.](https://chromium.googlesource.com/v8/v8/+/f78bfaa)**  
  
Date(Commit): Tue Feb 11 10:45:39 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/146623009](https://codereview.chromium.org/146623009)  
Regress: [mjsunit/regress/regress-3135.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3135.js)  
```javascript
assertEquals('{"x":1}', JSON.stringify({ x : 1 }, ["x", 1, "x", 1]));
assertEquals('{"1":1}', JSON.stringify({ 1 : 1 }, ["x", 1, "x", 1]));
assertEquals('{"1":1}', JSON.stringify({ 1 : 1 }, ["1", 1, "1", 1]));
assertEquals('{"1":1}', JSON.stringify({ 1 : 1 }, [1, "1", 1, "1"]));

var fired = 0;
var getter_obj = { get x() { fired++; return 2; } };
assertEquals('{"x":2}', JSON.stringify(getter_obj, ["x", "y", "x"]));
assertEquals(1, fired);

assertEquals('{"y":4,"x":3}', JSON.stringify({ x : 3, y : 4}, ["y", "x"]));
assertEquals('{"y":4,"1":2,"x":3}',
             JSON.stringify({ x : 3, y : 4, 1 : 2 }, ["y", 1, "x"]));

var a = { x : 8 };
assertEquals('{"__proto__":{"__proto__":null},"x":8}',
             JSON.stringify(a, ["__proto__", "x", "__proto__"]));
a.__proto__ = { x : 7 };
assertEquals('{"__proto__":{"__proto__":{"__proto__":null},"x":7},"x":8}',
             JSON.stringify(a, ["__proto__", "x"]));
var b = { __proto__: { x: 9 } };
assertEquals('{}', JSON.stringify(b));
assertEquals('{"x":9}', JSON.stringify(b, ["x"]));
var c = {x: 10};
Object.defineProperty(c, 'x', { enumerable: false });
assertEquals('{}', JSON.stringify(c));
assertEquals('{"x":10}', JSON.stringify(c, ["x"]));

assertEquals("[9,8,7]", JSON.stringify([9, 8, 7], [1, 1]));
var mixed_arr = [11,12,13];
mixed_arr.x = 10;
assertEquals('[11,12,13]', JSON.stringify(mixed_arr, [1, 0, 1]));

var mixed_obj = { x : 3 };
mixed_obj[0] = 6;
mixed_obj[1] = 5;
assertEquals('{"1":5,"0":6}', JSON.stringify(mixed_obj, [1, 0, 1]));

assertEquals('{"z":{"x":3},"x":1}',
             JSON.stringify({ x: 1, y:2, z: {x:3, b:4}}, ["z","x"]));

assertEquals('{}',
             JSON.stringify({ x : 1, "1": 1 }, [{}]));
assertEquals('{}',
             JSON.stringify({ x : 1, "1": 1 }, [true, undefined, null]));
assertEquals('{}',
             JSON.stringify({ x : 1, "1": 1 },
                            [{ toString: function() { return "x";} }]));
assertEquals('{}',
             JSON.stringify({ x : 1, "1": 1 },
                            [{ valueOf: function() { return 1;} }]));

assertEquals('{"toString":42}', JSON.stringify({ toString: 42 }, ["toString"]));

assertEquals('{"1":1,"s":"s"}',
             JSON.stringify({ 1: 1, s: "s" },
                            [new Number(1), new String("s")]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f78bfaa^!)  
[src/json.js](https://cs.chromium.org/chromium/src/v8/src/json.js?cl=f78bfaa)  
[test/mjsunit/regress-3135.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-3135.js?cl=f78bfaa)  
  

---   

## **regress-340125.js (chromium issue)**  
   
**[Issue: CHECK failure in CHECK(is_valid) failed: ../../v8/src/v8conversions.h(107)](https://crbug.com/340125)**  
**[Commit: Check the offset argument of TypedArray.set for fitting into Smi.](https://chromium.googlesource.com/v8/v8/+/a03d313)**  
  
Date(Commit): Tue Feb 04 09:53:05 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Security_severity-None", "Security_Impact-None", "allpublic", "Clusterfuzz", "Hotlist-Torque"]  
Code Review: [https://codereview.chromium.org/145623009](https://codereview.chromium.org/145623009)  
Regress: [mjsunit/regress/regress-340125.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-340125.js)  
```javascript
var a = new Int8Array(2);
var b = a.subarray(2, 4);
assertThrows(function () { a.set(b, 1e10); }, RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a03d313^!)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=a03d313)  
[test/mjsunit/regress/regress-340125.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-340125.js?cl=a03d313)  
  

---   

## **regress-crbug-336148.js (chromium issue)**  
   
**[Issue: TypeError as a result of undefined variable from a RegEx exec](https://crbug.com/336148)**  
**[Commit: Fix short-circuiting logical and/or in HOptimizedGraphBuilder.](https://chromium.googlesource.com/v8/v8/+/9e70f6a)**  
  
Date(Commit): Mon Feb 03 14:29:34 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-33", "Via-Wizard"]  
Code Review: [https://codereview.chromium.org/143263022](https://codereview.chromium.org/143263022)  
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
  

---   

## **regress-crbug-340064.js (chromium issue)**  
   
**[Issue: Chrome: Crash Report - Magic Signature: v8::internal::HOptimizedGraphBuilder::Prope...](https://crbug.com/340064)**  
**[Commit: Return a valid map for PropertyAccessInfos with Boolean type.](https://chromium.googlesource.com/v8/v8/+/db7124d)**  
  
Date(Commit): Mon Feb 03 10:20:32 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["M-34"]  
Code Review: [https://codereview.chromium.org/152603002](https://codereview.chromium.org/152603002)  
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
  

---   

## **regress-2989.js (v8 issue)**  
   
**[Issue: webkit/dfg-inline-arguments-* is flaky](https://crbug.com/v8/2989)**  
**[Commit: Simpler repro for bug 2989.](https://chromium.googlesource.com/v8/v8/+/3c2363f)**  
  
Date(Commit): Fri Jan 31 16:12:58 2014  
Type: Bug  
Code Review: [https://codereview.chromium.org/151403003](https://codereview.chromium.org/151403003)  
Regress: [mjsunit/regress/regress-2989.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2989.js)  
```javascript
if (isNeverOptimizeLiteMode()) {
  print("Warning: skipping test that requires optimization in Lite mode.");
  quit(0);
}

(function ArgumentsObjectChange() {
  function f(x) {
      x = 42;
      return f.arguments[0];
  }

  f(0);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(42, f(0));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c2363f^!)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=3c2363f)  
[test/mjsunit/regress/regress-2989.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2989.js?cl=3c2363f)  
[test/webkit/webkit.status](https://cs.chromium.org/chromium/src/v8/test/webkit/webkit.status?cl=3c2363f)  
  

---   

## **regress-336820.js (chromium issue)**  
   
**[Issue: Chrome_Mac: Crash Report - b4afb2ca_b5c4705e_0cc2522b_a19e9b16_fc5e603f](https://crbug.com/336820)**  
**[Commit: Don't crash in Array.join() if the resulting string exceeds the max string length.](https://chromium.googlesource.com/v8/v8/+/3214cf1)**  
  
Date(Commit): Fri Jan 31 12:21:17 2014  
Components/Type: None/Bug  
Labels: ["Hotlist-Google"]  
Code Review: [https://codereview.chromium.org/144533003](https://codereview.chromium.org/144533003)  
Regress: [mjsunit/regress/regress-336820.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-336820.js)  
```javascript
assertThrows((function() {
  let str = "a".repeat(1e7);
  let arr = new Array(2000);
  for (let i = 0; i < 200; ++i) {
    arr[i*10] = str;
  }
  let res = arr.join(':');
}), RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3214cf1^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=3214cf1)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3214cf1)  
[test/mjsunit/regress/regress-336820.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-336820.js?cl=3214cf1)  
  

---   

## **regress-keyed-access-string-length.js (other issue)**  
   
**[Commit: Fix regression caused by supporting inlining accesses to non-JSObjects](https://chromium.googlesource.com/v8/v8/+/bef13f7)**  
  
Date(Commit): Fri Jan 31 00:29:04 2014  
Code Review: [https://codereview.chromium.org/150983002](https://codereview.chromium.org/150983002)  
Regress: [mjsunit/regress/regress-keyed-access-string-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-keyed-access-string-length.js)  
```javascript
function f(i) {
  return "abc"[i];
}

f("length");
f("length");
%OptimizeFunctionOnNextCall(f);
f("length");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bef13f7^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=bef13f7)  
[test/mjsunit/regress/regress-keyed-access-string-length.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-keyed-access-string-length.js?cl=bef13f7)  
  
  
---   

## **regress-634-debug.js (v8 issue)**  
   
**[Issue: debug-handle test fails on ia32 v8 running on x64 linux machine](https://crbug.com/v8/634)**  
**[Commit: Speed up some mjsunit test cases and clean up test expectations for arm and mips.](https://chromium.googlesource.com/v8/v8/+/cde3ed1)**  
  
Date(Commit): Fri Jan 24 11:36:45 2014  
Type: ----  
Code Review: [https://codereview.chromium.org/138503008](https://codereview.chromium.org/138503008)  
Regress: [mjsunit/regress/regress-634-debug.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-634-debug.js)  
```javascript
function f() {
  %SetAllocationTimeout(1, 0, false);
  a = new Array(0);
  assertEquals(0, a.length);
  assertEquals(0, a.length);
  %SetAllocationTimeout(-1, -1, true);
}

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cde3ed1^!)  
[test/mjsunit/compiler/alloc-number-debug.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/alloc-number-debug.js?cl=cde3ed1)  
[test/mjsunit/compiler/regress-arguments.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-arguments.js?cl=cde3ed1)  
[test/mjsunit/compiler/regress-rep-change.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-rep-change.js?cl=cde3ed1)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=cde3ed1)  
[test/mjsunit/regress/regress-490.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-490.js?cl=cde3ed1)  
...  
  

---   

## **regress-array-pop-deopt.js (other issue)**  
   
**[Commit: Reland (and fix) "Add hydrogen support for ArrayPop, and remove the handwritten call stubs."](https://chromium.googlesource.com/v8/v8/+/f303303)**  
  
Date(Commit): Wed Jan 22 13:22:58 2014  
Code Review: [https://codereview.chromium.org/144913003](https://codereview.chromium.org/144913003)  
Regress: [mjsunit/regress/regress-array-pop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-array-pop-deopt.js)  
```javascript
var o = [6,7,8,9];

function f(b) {
  var v = o.pop() + b;
  return v;
}

assertEquals(10, f(1));
assertEquals(9, f(1));
assertEquals(8, f(1));
%OptimizeFunctionOnNextCall(f);
assertEquals("61", f("1"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f303303^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=f303303)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=f303303)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=f303303)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=f303303)  
[src/stub-cache.h](https://cs.chromium.org/chromium/src/v8/src/stub-cache.h?cl=f303303)  
...  
  
  
---   

## **regress-334708.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/334708)**  
**[Commit: Fixed floor-of-div optimization.](https://chromium.googlesource.com/v8/v8/+/b4949cf)**  
  
Date(Commit): Wed Jan 22 11:54:51 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/143903016](https://codereview.chromium.org/143903016)  
Regress: [mjsunit/regress/regress-334708.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-334708.js)  
```javascript
function foo(x, y) {
  return Math.floor(x / y);
}

function bar(x, y) {
  return foo(x + 1, y + 1);
}

foo(16, "4");

bar(64, 2);
%OptimizeFunctionOnNextCall(bar);
bar(64, 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4949cf^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=b4949cf)  
[test/mjsunit/regress/regress-334708.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-334708.js?cl=b4949cf)  
  

---   

## **regress-333594.js (chromium issue)**  
   
**[Issue: Displaying a PDF with the pdf.js viewer crash Chrome](https://crbug.com/333594)**  
**[Commit: Fix representation requirement in HReturn.](https://chromium.googlesource.com/v8/v8/+/5771b09)**  
  
Date(Commit): Mon Jan 20 19:00:11 2014  
Components/Type: Blink>JavaScript/Bug  
Labels: ["Stability-Crash", "Merge-merged-32", "Via-Wizard", "Merge-merged-33"]  
Code Review: [https://codereview.chromium.org/143523002](https://codereview.chromium.org/143523002)  
Regress: [mjsunit/regress/regress-333594.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-333594.js)  
```javascript
var a = { x: 1.1 };
a.x = 0;
var G = a.x;
var o = { x: {} };

function func() {
  return {x: G};
}

func();
func();
%OptimizeFunctionOnNextCall(func);
assertEquals(0, func().x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5771b09^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=5771b09)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=5771b09)  
[test/mjsunit/regress-333594.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-333594.js?cl=5771b09)  
  

---   

## **regress-is-contextual.js (other issue)**  
   
**[Commit: Fix logic error in assert in IsUndeclaredGlobal()](https://chromium.googlesource.com/v8/v8/+/155ef10)**  
  
Date(Commit): Fri Jan 17 11:08:24 2014  
Code Review: [https://codereview.chromium.org/140943002](https://codereview.chromium.org/140943002)  
Regress: [mjsunit/regress/regress-is-contextual.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-is-contextual.js)  
```javascript
function foo(index) {
  return text.charAt(index);
}

var text = "hi there";
foo(0);
foo(0);
foo(100);     // Accumulate feedback that index is out of bounds.
text = false;

assertThrows(function () { foo(); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/155ef10^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=155ef10)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=155ef10)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=155ef10)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=155ef10)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=155ef10)  
...  
  
  
---   

## **regress-crbug-315252.js (chromium issue)**  
   
**[Issue: No Permission](https://crbug.com/315252)**  
**[Commit: Turn Runtime_MigrateInstance into Runtime_TryMigrateInstance](https://chromium.googlesource.com/v8/v8/+/1ed94ac)**  
  
Date(Commit): Tue Jan 14 13:41:09 2014  
Components/Type: None/None  
Labels: "No Permission"  
Code Review: [https://codereview.chromium.org/131243003](https://codereview.chromium.org/131243003)  
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
...  
  

---   

## **regress-331416.js (chromium issue)**  
   
**[Issue: [LangFuzz] Crash on Heap with Array access/length and invalid read](https://crbug.com/331416)**  
**[Commit: Correctly handle instances without elements in polymorphic keyed load/store.](https://chromium.googlesource.com/v8/v8/+/8db7aaa)**  
  
Date(Commit): Wed Jan 08 09:57:28 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-merged-1750", "Stability-Memory-AddressSanitizer", "Missing_Impact-1", "M-33", "Via-Wizard", "M-34", "reward-3000", "Security_Severity-Medium", "Security_Impact-Beta", "allpublic"]  
Code Review: [https://codereview.chromium.org/121893003](https://codereview.chromium.org/121893003)  
Regress: [mjsunit/regress/regress-331416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-331416.js)  
```javascript
function load(a, i) {
  return a[i];
}
load([1, 2, 3], "length");
load(3);
load([1, 2, 3], 3);
load(0, 0);
%OptimizeFunctionOnNextCall(load);
assertEquals(2, load([1, 2, 3], 1));
assertEquals(undefined, load(0, 0));

function store(a, i, x) {
  a[i] = x;
}
store([1, 2, 3], "length", 3);
store(3);
store([1, 2, 3], 3, 3);
store(0, 0, 1);
%OptimizeFunctionOnNextCall(store);
var a = [1, 2, 3];
store(a, 1, 1);
assertEquals(1, a[1]);
store(0, 0, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8db7aaa^!)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=8db7aaa)  
[test/mjsunit/regress/regress-331416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-331416.js?cl=8db7aaa)  
  

---   

## **regress-331444.js (chromium issue)**  
   
**[Issue: [LangFuzz] Crash at v8::internal::StoreBuffer::Compact with invalid write](https://crbug.com/331444)**  
**[Commit: Fix selection of popular pages in store buffer.](https://chromium.googlesource.com/v8/v8/+/43d1c23)**  
  
Date(Commit): Wed Jan 08 09:49:37 2014  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Merge-Merged-1700", "Merge-merged-1750", "CVE-2013-6650", "Missing_Impact-1", "M-32", "Via-Wizard", "reward-3000", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/125983002](https://codereview.chromium.org/125983002)  
Regress: [mjsunit/regress/regress-331444.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-331444.js)  
```javascript
function boom() {
  var args = [];
  for (var i = 0; i < 125000; i++)
    args.push(i);
  return Array.apply(Array, args);
}
var array = boom();
function fib(n) {
  var f0 = 0, f1 = 1;
  for (; n > 0; n = n - 1) {
    f0 + f1;
    f0 = array;
  }
}
fib(12);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/43d1c23^!)  
[src/store-buffer.cc](https://cs.chromium.org/chromium/src/v8/src/store-buffer.cc?cl=43d1c23)  
[test/mjsunit/regress/regress-331444.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-331444.js?cl=43d1c23)  
  

---   
