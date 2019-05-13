# V8Harvest  
The Harvest of V8 regress in 2017.  
  

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
