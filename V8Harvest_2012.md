# V8Harvest  
The Harvest of V8 regress in 2012.  
  

## **regress-166379.js (chromium issue)**  
   
**[Issue 166379:
 [CRASH] LayoutTests/fast/js/integer-division-neg2tothe32-by-neg1.html](https://crbug.com/166379)**  
**[Commit: Deopt on overflow in integer mod.](https://chromium.googlesource.com/v8/v8/+/362218a)**  
  
Date(Commit): Wed Dec 19 12:01:22 2012  
Components/Type: Blink/Bug  
Labels: []  
Code Review: [https://chromiumcodereview.appspot.com/11618017](https://chromiumcodereview.appspot.com/11618017)  
Regress: [mjsunit/regress/regress-166379.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-166379.js)  
```javascript
function mod(a, b) {
  return a % b;
}

;
%PrepareFunctionForOptimization(mod);
assertEquals(0, mod(4, 2));
assertEquals(1, mod(3, 2));
%OptimizeFunctionOnNextCall(mod);

assertEquals(-Infinity, 1 / mod(-2147483648, -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/362218a^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=362218a)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=362218a)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=362218a)  
[test/mjsunit/regress/regress-166379.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-166379.js?cl=362218a)  
  

---   

## **regress-166553.js (chromium issue)**  
   
**[Issue 166553:
 [LangFuzz] Crash at v8::internal::HeapObject::SizeFromMap with invalid read](https://crbug.com/166553)**  
**[Commit: Correctly handle negative codes in String.fromCharCode()](https://chromium.googlesource.com/v8/v8/+/8574054)**  
  
Date(Commit): Tue Dec 18 12:37:57 2012  
Components/Type: None/Bug-Security  
Labels: ["M-25", "Reward-1000", "Via-Wizard", "Merge-Merged", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/11576069](https://chromiumcodereview.appspot.com/11576069)  
Regress: [mjsunit/regress/regress-166553.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-166553.js)  
```javascript
JSON.stringify(String.fromCharCode(1, -11).toString())
gc();
var s = String.fromCharCode(1, -11)
assertEquals(65525, s.charCodeAt(1))  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8574054^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=8574054)  
[test/mjsunit/regress/regress-166553.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-166553.js?cl=8574054)  
  

---   

## **regress-2243.js (v8 issue)**  
   
**[Issue 2243:
 Assigning to function identifier under strict should throw](https://crbug.com/v8/2243)**  
**[Commit: Simplify implementation of assignment-to-const checks.](https://chromium.googlesource.com/v8/v8/+/c6bb497)**  
  
Date(Commit): Tue Dec 18 12:00:50 2012  
Type: Bug  
Code Review: [https://codereview.chromium.org/11607016](https://codereview.chromium.org/11607016)  
Regress: [mjsunit/es6/regress/regress-2243.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2243.js)  
```javascript
assertThrows("'use strict'; (function f() { f = 123; })()", TypeError);
assertThrows("(function f() { 'use strict'; f = 123; })()", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c6bb497^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=c6bb497)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=c6bb497)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=c6bb497)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=c6bb497)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=c6bb497)  
...  
  

---   

## **regress-165637.js (chromium issue)**  
   
**[Issue 165637:
 Array.slice performance drastically reduced](https://crbug.com/165637)**  
**[Commit: Remove over-zealous hole checking in Array.slice()](https://chromium.googlesource.com/v8/v8/+/facad07)**  
  
Date(Commit): Wed Dec 12 15:20:45 2012  
Components/Type: Blink/Bug  
Labels: ["Via-Wizard", "Needs-Feedback"]  
Code Review: [https://codereview.chromium.org/11442054](https://codereview.chromium.org/11442054)  
Regress: [mjsunit/regress/regress-165637.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-165637.js)  
```javascript
var holey_array = [1, 2, 3, 4, 5,,,,,,];
assertEquals([undefined], holey_array.slice(6, 7));
assertEquals(undefined, holey_array.slice(6, 7)[0]);
assertEquals([], holey_array.slice(2, 1));
assertEquals(3, holey_array.slice(2, 3)[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/facad07^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=facad07)  
[test/mjsunit/regress/regress-165637.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-165637.js?cl=facad07)  
  

---   

## **regress-2315.js (v8 issue)**  
   
**[Issue 2315:
 Eval cannot be compiled lazily anymore](https://crbug.com/v8/2315)**  
**[Commit: Allow lazy compilation (and thus optimisation) of functions inside eval.](https://chromium.googlesource.com/v8/v8/+/3348b5c)**  
  
Date(Commit): Fri Dec 07 10:35:50 2012  
Type: ----  
Code Review: [https://codereview.chromium.org/11438042](https://codereview.chromium.org/11438042)  
Regress: [mjsunit/regress/regress-2315.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2315.js)  
```javascript
var foo = (function() {
  return eval("(function bar() { return 1; })");
})();

foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();

assertOptimized(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3348b5c^!)  
[src/contexts.h](https://cs.chromium.org/chromium/src/v8/src/contexts.h?cl=3348b5c)  
[src/lithium.h](https://cs.chromium.org/chromium/src/v8/src/lithium.h?cl=3348b5c)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=3348b5c)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=3348b5c)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=3348b5c)  
...  
  

---   

## **regress-2443.js (v8 issue)**  
   
**[Issue 2443:
 Number.prototype.{toPrecision,toExponential} should accept out-of-range input for NaN/Infinity](https://crbug.com/v8/2443)**  
**[Commit: Fix spec violations in methods of Number.prototype.](https://chromium.googlesource.com/v8/v8/+/3388f92)**  
  
Date(Commit): Fri Dec 07 10:20:35 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11465005](https://chromiumcodereview.appspot.com/11465005)  
Regress: [mjsunit/regress/regress-2443.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2443.js)  
```javascript
assertThrows(function() { Number.prototype.toExponential.call({}) },
             TypeError);

assertThrows(function() { Number.prototype.toPrecision.call({}) },
             TypeError);

assertThrows(function() { Number.prototype.toFixed.call({}) },
             TypeError);

assertThrows(function() { Number.prototype.toString.call({}) },
             TypeError);

assertThrows(function() { Number.prototype.toLocaleString.call({}) },
             TypeError);

assertThrows(function() { Number.prototype.ValueOf.call({}) },
             TypeError);



var x_obj = new Number(1);
x_obj.valueOf = function() { assertUnreachable(); };

assertEquals("1.00e+0",
             Number.prototype.toExponential.call(x_obj, 2));

assertEquals("1.0",
             Number.prototype.toPrecision.call(x_obj, 2));

assertEquals("1.00",
             Number.prototype.toFixed.call(x_obj, 2));

assertEquals("1.00e+0",
             Number.prototype.toExponential.call(1, 2));

assertEquals("1.0",
             Number.prototype.toPrecision.call(1, 2));

assertEquals("1.00",
             Number.prototype.toFixed.call(1, 2));



var f_flag = false;
var f_obj = { valueOf: function() { f_flag = true; return 1000; } };

assertEquals("NaN",
             Number.prototype.toExponential.call(NaN, f_obj));
assertTrue(f_flag);

f_flag = false;
assertEquals("Infinity",
             Number.prototype.toExponential.call(1/0, f_obj));
assertTrue(f_flag);

f_flag = false;
assertEquals("-Infinity",
             Number.prototype.toExponential.call(-1/0, f_obj));
assertTrue(f_flag);

f_flag = false;
assertEquals("NaN",
             Number.prototype.toPrecision.call(NaN, f_obj));
assertTrue(f_flag);

f_flag = false;
assertEquals("Infinity",
             Number.prototype.toPrecision.call(1/0, f_obj));
assertTrue(f_flag);

f_flag = false;
assertEquals("-Infinity",
             Number.prototype.toPrecision.call(-1/0, f_obj));
assertTrue(f_flag);


f_flag = false;
assertThrows(function() { Number.prototype.toFixed.call(NaN, f_obj) },
             RangeError);
assertTrue(f_flag);

f_flag = false;
assertThrows(function() { Number.prototype.toFixed.call(1/0, f_obj) },
             RangeError);
assertTrue(f_flag);

f_flag = false;
assertThrows(function() { Number.prototype.toFixed.call(-1/0, f_obj) },
             RangeError);
assertTrue(f_flag);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3388f92^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3388f92)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=3388f92)  
[test/mjsunit/function-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/function-call.js?cl=3388f92)  
[test/mjsunit/regress/regress-2443.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2443.js?cl=3388f92)  
[test/mjsunit/regress/regress-crbug-18639.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-18639.js?cl=3388f92)  
  

---   

## **regress-2444.js (v8 issue)**  
   
**[Issue 2444:
 Math.{max, min}() must not return after first NaN value](https://crbug.com/v8/2444)**  
**[Commit: Iterate through all arguments for side effects in Math.min/max.](https://chromium.googlesource.com/v8/v8/+/276c790)**  
  
Date(Commit): Thu Dec 06 13:13:38 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/11444030](https://chromiumcodereview.appspot.com/11444030)  
Regress: [mjsunit/regress/regress-2444.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2444.js)  
```javascript
var flags;

function resetFlags(size) {
  flags = Array(size);
  while (size--) flags[size] = 0;
}

function assertFlags(array) {
  assertArrayEquals(array, flags);
}

function object_factory(flag_index, value, expected_flags) {
  var obj = {};
  obj.valueOf = function() {
    assertFlags(expected_flags);
    flags[flag_index]++;
    return value;
  }
  return obj;
}


assertEquals(-Infinity, Math.max());

resetFlags(1);
assertEquals(NaN,
             Math.max(object_factory(0, NaN, [0])));
assertFlags([1]);

resetFlags(2);
assertEquals(NaN,
             Math.max(object_factory(0, NaN, [0, 0]),
                      object_factory(1,   0, [1, 0])));
assertFlags([1, 1]);

resetFlags(3);
assertEquals(NaN,
             Math.max(object_factory(0, NaN, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2,   1, [1, 1, 0])));
assertFlags([1, 1, 1]);

resetFlags(3);
assertEquals(NaN,
             Math.max(object_factory(0,   2, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2, NaN, [1, 1, 0])));
assertFlags([1, 1, 1]);

resetFlags(3);
assertEquals(2,
             Math.max(object_factory(0,   2, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2,   1, [1, 1, 0])));
assertFlags([1, 1, 1]);


assertEquals(+Infinity, Math.min());

resetFlags(1);
assertEquals(NaN,
             Math.min(object_factory(0, NaN, [0])));
assertFlags([1]);

resetFlags(2);
assertEquals(NaN,
             Math.min(object_factory(0, NaN, [0, 0]),
                      object_factory(1,   0, [1, 0])));
assertFlags([1, 1]);

resetFlags(3);
assertEquals(NaN,
             Math.min(object_factory(0, NaN, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2,   1, [1, 1, 0])));
assertFlags([1, 1, 1]);

resetFlags(3);
assertEquals(NaN,
             Math.min(object_factory(0,   2, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2, NaN, [1, 1, 0])));
assertFlags([1, 1, 1]);

resetFlags(3);
assertEquals(0,
             Math.min(object_factory(0,   2, [0, 0, 0]),
                      object_factory(1,   0, [1, 0, 0]),
                      object_factory(2,   1, [1, 1, 0])));
assertFlags([1, 1, 1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/276c790^!)  
[src/math.js](https://cs.chromium.org/chromium/src/v8/src/math.js?cl=276c790)  
[test/mjsunit/regress/regress-2444.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2444.js?cl=276c790)  
  

---   

## **regress-2438.js (v8 issue)**  
   
**[Issue 2438:
 [ES5.1 - 15.10.6.2] RegExp.prototype.exec(string), step 5 "ToInteger(lastIndex)" not always executed](https://crbug.com/v8/2438)**  
**[Commit: Fix spec violations related to regexp.lastIndex](https://chromium.googlesource.com/v8/v8/+/6c92aba)**  
  
Date(Commit): Wed Dec 05 12:32:25 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11451005](https://chromiumcodereview.appspot.com/11451005)  
Regress: [mjsunit/regress/regress-2438.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2438.js)  
```javascript
function testSideEffects(subject, re) {
  var counter = 0;
  var side_effect_object = { valueOf: function() { return counter++; } };
  re.lastIndex = side_effect_object;
  re.exec(subject);

  assertEquals(1, counter);

  re.lastIndex = side_effect_object;
  re.test(subject);

  assertEquals(2, counter);
}

testSideEffects("zzzz", /a/);
testSideEffects("zzzz", /a/g);
testSideEffects("xaxa", /a/);
testSideEffects("xaxa", /a/g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c92aba^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=6c92aba)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=6c92aba)  
[src/regexp.js](https://cs.chromium.org/chromium/src/v8/src/regexp.js?cl=6c92aba)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=6c92aba)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=6c92aba)  
...  
  

---   

## **regress-2437.js (v8 issue)**  
   
**[Issue 2437:
 RegExp.prototype.exec should always update lastIndex if no match was found](https://crbug.com/v8/2437)**  
**[Commit: Fix spec violations related to regexp.lastIndex](https://chromium.googlesource.com/v8/v8/+/6c92aba)**  
  
Date(Commit): Wed Dec 05 12:32:25 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11451005](https://chromiumcodereview.appspot.com/11451005)  
Regress: [mjsunit/regress/regress-2437.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2437.js)  
```javascript
r = /a/;
r.lastIndex = 1;
r.exec("zzzz");
assertEquals(1, r.lastIndex);

r = /a/;
r.lastIndex = 1;
r.test("zzzz");
assertEquals(1, r.lastIndex);

r = /a/;
r.lastIndex = 1;
"zzzz".match(r);
assertEquals(1, r.lastIndex);

r = /a/;
r.lastIndex = 1;
"zzzz".replace(r, "");
assertEquals(1, r.lastIndex);

r = /\d/;
r.lastIndex = 1;
"zzzz".replace(r, "");
assertEquals(1, r.lastIndex);

r = /a/;
r.lastIndex = 1;
"zzzz".replace(r, "a");
assertEquals(1, r.lastIndex);

r = /\d/;
r.lastIndex = 1;
"zzzz".replace(r, "a");
assertEquals(1, r.lastIndex);

r = /a/;
r.lastIndex = 1;
"zzzz".replace(r, function() { return ""; });
assertEquals(1, r.lastIndex);

r = /a/g;
r.lastIndex = -1;
"0123abcd".replace(r, "x");
assertEquals(0, r.lastIndex);

r.lastIndex = -1;
"01234567".replace(r, "x");
assertEquals(0, r.lastIndex);

r.lastIndex = -1;
"0123abcd".match(r);
assertEquals(0, r.lastIndex);

r.lastIndex = -1;
"01234567".match(r);
assertEquals(0, r.lastIndex);

r = /a/;
r.lastIndex = -1;
"0123abcd".replace(r, "x");
assertEquals(-1, r.lastIndex);

r.lastIndex = -1;
"01234567".replace(r, "x");
assertEquals(-1, r.lastIndex);

r.lastIndex = -1;
"0123abcd".match(r);
assertEquals(-1, r.lastIndex);

r.lastIndex = -1;
"01234567".match(r);
assertEquals(-1, r.lastIndex);

r = /a/y;
r.lastIndex = -1;
"0123abcd".replace(r, "x");
assertEquals(0, r.lastIndex);

r.lastIndex = -1;
"01234567".replace(r, "x");
assertEquals(0, r.lastIndex);


r = /a/g;
r.lastIndex = 1;
r.exec("01234567");
assertEquals(0, r.lastIndex);

r.lastIndex = 1;
r.exec("0123abcd");
assertEquals(5, r.lastIndex);

r = /a/;
r.lastIndex = 1;
r.exec("01234567");
assertEquals(1, r.lastIndex);

r.lastIndex = 1;
r.exec("0123abcd");
assertEquals(1, r.lastIndex);

r = /a/g;
r.lastIndex = 1;
r.test("01234567");
assertEquals(0, r.lastIndex);

r.lastIndex = 1;
r.test("0123abcd");
assertEquals(5, r.lastIndex);

r = /a/;
r.lastIndex = 1;
r.test("01234567");
assertEquals(1, r.lastIndex);

r.lastIndex = 1;
r.test("0123abcd");
assertEquals(1, r.lastIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c92aba^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=6c92aba)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=6c92aba)  
[src/regexp.js](https://cs.chromium.org/chromium/src/v8/src/regexp.js?cl=6c92aba)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=6c92aba)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=6c92aba)  
...  
  

---   

## **regress-2433.js (v8 issue)**  
   
**[Issue 2433:
 Array access past length leads to reading uninitialized data after transition to FAST_DOUBLE_ELEMENTS](https://crbug.com/v8/2433)**  
**[Commit: CopyPackedSmiToDoubleElements should fill the FixedDoubleArray with holes](https://chromium.googlesource.com/v8/v8/+/7553f0d)**  
  
Date(Commit): Thu Nov 29 08:34:19 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11280223](https://chromiumcodereview.appspot.com/11280223)  
Regress: [mjsunit/regress/regress-2433.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2433.js)  
```javascript
arr = [];
arr[0] = 0;
arr[0] = 1.1;
assertEquals(undefined, arr[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7553f0d^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=7553f0d)  
[test/mjsunit/regress/regress-2433.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2433.js?cl=7553f0d)  
  

---   

## **regress-crbug-162085.js (chromium issue)**  
   
**[Issue 162085:
 Google Calendar leaks tons of memory](https://crbug.com/162085)**  
**[Commit: Ensure double arrays are filled with holes when extended from variations of empty arrays.](https://chromium.googlesource.com/v8/v8/+/ebeaad6)**  
  
Date(Commit): Mon Nov 26 14:29:21 2012  
Components/Type: Blink/Bug-Regression  
Labels: ["M-25", "ReleaseBlock-Dev"]  
Code Review: [https://chromiumcodereview.appspot.com/11414155](https://chromiumcodereview.appspot.com/11414155)  
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
...  
  

---   

## **regress-2416.js (v8 issue)**  
   
**[Issue 2416:
 According to V8, 2147483647 <= -2147483648 is true.](https://crbug.com/v8/2416)**  
**[Commit: Fix corner case in x64 compare stubs.](https://chromium.googlesource.com/v8/v8/+/a956594)**  
  
Date(Commit): Tue Nov 20 15:57:10 2012  
Type: ----  
Code Review: [https://codereview.chromium.org/11413087](https://codereview.chromium.org/11413087)  
Regress: [mjsunit/regress/regress-2416.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2416.js)  
```javascript
assertFalse(2147483647 < -2147483648)
assertFalse(2147483647 <= -2147483648)
assertFalse(2147483647 == -2147483648)
assertTrue(2147483647 >= -2147483648)
assertTrue(2147483647 > -2147483648)

assertTrue(-2147483648 < 2147483647)
assertTrue(-2147483648 <= 2147483647)
assertFalse(-2147483648 == 2147483647)
assertFalse(-2147483648 >= 2147483647)
assertFalse(-2147483648 > 2147483647)

assertFalse(2147483647 < 2147483647)
assertTrue(2147483647 <= 2147483647)
assertTrue(2147483647 == 2147483647)
assertTrue(2147483647 >= 2147483647)
assertFalse(2147483647 > 2147483647)

assertFalse(-2147483648 < -2147483648)
assertTrue(-2147483648 <= -2147483648)
assertTrue(-2147483648 == -2147483648)
assertTrue(-2147483648 >= -2147483648)
assertFalse(-2147483648 > -2147483648)


assertFalse(1073741823 < -1073741824)
assertFalse(1073741823 <= -1073741824)
assertFalse(1073741823 == -1073741824)
assertTrue(1073741823 >= -1073741824)
assertTrue(1073741823 > -1073741824)

assertTrue(-1073741824 < 1073741823)
assertTrue(-1073741824 <= 1073741823)
assertFalse(-1073741824 == 1073741823)
assertFalse(-1073741824 >= 1073741823)
assertFalse(-1073741824 > 1073741823)

assertFalse(1073741823 < 1073741823)
assertTrue(1073741823 <= 1073741823)
assertTrue(1073741823 == 1073741823)
assertTrue(1073741823 >= 1073741823)
assertFalse(1073741823 > 1073741823)

assertFalse(-1073741824 < -1073741824)
assertTrue(-1073741824 <= -1073741824)
assertTrue(-1073741824 == -1073741824)
assertTrue(-1073741824 >= -1073741824)
assertFalse(-1073741824 > -1073741824)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a956594^!)  
[src/x64/code-stubs-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/code-stubs-x64.cc?cl=a956594)  
[test/mjsunit/regress/regress-2416.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2416.js?cl=a956594)  
  

---   

## **regress-2263.js (v8 issue)**  
   
**[Issue 2263:
 Array.prototype.join must perform ToUint32(length) before ToString(separator)](https://crbug.com/v8/2263)**  
**[Commit: Fix Array.prototype.join evaluation order.](https://chromium.googlesource.com/v8/v8/+/3d1582c)**  
  
Date(Commit): Fri Nov 16 12:45:23 2012  
Type: Bug  
Code Review: [https://codereview.chromium.org/11280025](https://codereview.chromium.org/11280025)  
Regress: [mjsunit/regress/regress-2263.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2263.js)  
```javascript
assertThrows = function assertThrows(code, type_opt, cause_opt) {
  var threwException = true;
  try {
    if (typeof code === 'function') {
      code();
    } else {
      eval(code);
    }
    threwException = false;
  } catch (e) {
    if (typeof type_opt === 'function') {
      assertInstanceof(e, type_opt);
    } else if (type_opt !== void 0) {
      failWithMessage("invalid use of assertThrows, maybe you want assertThrowsEquals");
    }
    if (arguments.length >= 3) {
      assertEquals(e.type, cause_opt);
    }
    // Success.
    return;
  }
  failWithMessage("Did not throw exception");
};

var obj = { length: { valueOf: function(){ throw { type: "length" }}}};
var sep = { toString: function(){ throw { type: "toString" }}};
assertThrows("Array.prototype.join.call(obj, sep)", undefined, "length");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d1582c^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=3d1582c)  
[test/mjsunit/regress/regress-2263.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2263.js?cl=3d1582c)  
  

---   

## **regress-2410.js (v8 issue)**  
   
**[Issue 2410:
 Object.getOwnPropertyNames() should not access Object.prototype](https://crbug.com/v8/2410)**  
**[Commit: When using an Object as a set in Object.getOwnPropertyNames, null out the proto](https://chromium.googlesource.com/v8/v8/+/af824ea)**  
  
Date(Commit): Fri Nov 16 09:32:39 2012  
Type: ----  
Code Review: [https://codereview.chromium.org/11364237](https://codereview.chromium.org/11364237)  
Regress: [mjsunit/regress/regress-2410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2410.js)  
```javascript
Object.defineProperty(Object.prototype,
                      'thrower',
                      { get: function() { throw Error('bug') } });
var obj = { thrower: 'local' };
assertEquals(['thrower'], Object.getOwnPropertyNames(obj));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/af824ea^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=af824ea)  
[test/mjsunit/regress/regress-2410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2410.js?cl=af824ea)  
  

---   

## **regress-2398.js (v8 issue)**  
   
**[Issue 2398:
 Formatting Error.message is observable.](https://crbug.com/v8/2398)**  
**[Commit: Make formatting error message side-effect-free.](https://chromium.googlesource.com/v8/v8/+/4cca6c6)**  
  
Date(Commit): Mon Nov 12 10:33:20 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/11359130](https://chromiumcodereview.appspot.com/11359130)  
Regress: [mjsunit/regress/regress-2398.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2398.js)  
```javascript
"use strict";

var observed = false;

var object = { get toString() { observed = true; } };
Object.defineProperty(object, "ro", { value: 1 });

try {
  object.ro = 2;  // TypeError caused by trying to write to read-only.
} catch (e) {
  e.message;  // Forces formatting of the message object.
}

assertFalse(observed);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4cca6c6^!)  
[src/isolate.cc](https://cs.chromium.org/chromium/src/v8/src/isolate.cc?cl=4cca6c6)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=4cca6c6)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4cca6c6)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=4cca6c6)  
[test/mjsunit/regress/regress-2398.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2398.js?cl=4cca6c6)  
  

---   

## **regress-crbug-160010.js (chromium issue)**  
   
**[Issue 160010:
 [LangFuzz] Crash at v8::internal::BasicJsonStringifier::SerializeString](https://crbug.com/160010)**  
**[Commit: Fix length check in JSON.stringify.](https://chromium.googlesource.com/v8/v8/+/ef1b3d3)**  
  
Date(Commit): Mon Nov 12 10:20:07 2012  
Components/Type: None/Bug-Security  
Labels: ["M-25", "Reward-1000", "Via-Wizard", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/11410031](https://chromiumcodereview.appspot.com/11410031)  
Regress: [mjsunit/regress/regress-crbug-160010.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-160010.js)  
```javascript
var str = "a";
for (var i = 0; i < 28; i++) {
  str += str;
  %FlattenString(str);  // Evil performance hack
}
JSON.stringify(str);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ef1b3d3^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=ef1b3d3)  
[test/mjsunit/regress/regress-crbug-160010.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-160010.js?cl=ef1b3d3)  
  

---   

## **regress-crbug-157019.js (chromium issue)**  
   
**[Issue 157019:
 UNKNOWN in v8::internal::Invoke](https://crbug.com/157019)**  
**[Commit: Fix slack tracking when instance prototype changes.](https://chromium.googlesource.com/v8/v8/+/a31889e)**  
  
Date(Commit): Thu Nov 08 11:56:44 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-24"]  
Code Review: [https://codereview.chromium.org/11293059](https://codereview.chromium.org/11293059)  
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
...  
  

---   

## **regress-delete-empty-double.js (other issue)**  
   
**[Commit: Ensure reducing the length of an array doesn't make it go holey.](https://chromium.googlesource.com/v8/v8/+/14abf05)**  
  
Date(Commit): Fri Nov 02 10:24:56 2012  
Code Review: [https://chromiumcodereview.appspot.com/11358011](https://chromiumcodereview.appspot.com/11358011)  
Regress: [mjsunit/regress/regress-delete-empty-double.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-delete-empty-double.js)  
```javascript
a = [1.1,2.2,3.3];
a.length = 1;
delete a[1];

assertTrue(%HasDoubleElements(a));
assertFalse(%HasHoleyElements(a));

delete a[0];

assertTrue(%HasDoubleElements(a));
assertTrue(%HasHoleyElements(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/14abf05^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=14abf05)  
[test/mjsunit/elements-length-no-holey.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/elements-length-no-holey.js?cl=14abf05)  
[test/mjsunit/regress/regress-delete-empty-double.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-delete-empty-double.js?cl=14abf05)  
  
  
---   

## **regress-crbug-158185.js (chromium issue)**  
   
**[Issue 158185:
 JSON.parse](https://crbug.com/158185)**  
**[Commit: Treat leading zeros in JSON.parse correctly.](https://chromium.googlesource.com/v8/v8/+/8ed2e56)**  
  
Date(Commit): Mon Oct 29 12:01:29 2012  
Components/Type: Blink/Bug  
Labels: []  
Code Review: [https://chromiumcodereview.appspot.com/11273075](https://chromiumcodereview.appspot.com/11273075)  
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
  

---   

## **regress-crbug-157520.js (chromium issue)**  
   
**[Issue 157520:
 Arguments collection not updated intermittently](https://crbug.com/157520)**  
**[Commit: Fix ugly typo in GenerateNewNonStrictFast.](https://chromium.googlesource.com/v8/v8/+/e363cd3)**  
  
Date(Commit): Fri Oct 26 10:55:25 2012  
Components/Type: Blink/Bug  
Labels: ["Via-Wizard"]  
Code Review: [https://codereview.chromium.org/11300008](https://codereview.chromium.org/11300008)  
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
  

---   

## **regress-json-stringify-gc.js (other issue)**  
   
**[Commit: Reland JSON.stringify reimplementation.](https://chromium.googlesource.com/v8/v8/+/e50ee08)**  
  
Date(Commit): Mon Oct 22 14:22:58 2012  
Code Review: [https://chromiumcodereview.appspot.com/11189112](https://chromiumcodereview.appspot.com/11189112)  
Regress: [mjsunit/regress/regress-json-stringify-gc.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-json-stringify-gc.js)  
```javascript
var a = [];
var new_space_string = "a";
for (var i = 0; i < 8; i++) {
  new_space_string += new_space_string;
}
for (var i = 0; i < 10000; i++) a.push(new_space_string);

json1 = JSON.stringify(a);
json2 = JSON.stringify(a);
assertTrue(json1 == json2, "GC caused JSON.stringify to fail.");

for (var i = 0; i < 10000; i++) {
  var s = i.toString();
  assertEquals('"' + s + '"', JSON.stringify(s, null, 0));
}

for (var i = 0; i < 10000; i++) {
  var s = i.toString() + "\u2603";
  assertEquals('"' + s + '"', JSON.stringify(s, null, 0));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e50ee08^!)  
[src/json-stringifier.h](https://cs.chromium.org/chromium/src/v8/src/json-stringifier.h?cl=e50ee08)  
[src/json.js](https://cs.chromium.org/chromium/src/v8/src/json.js?cl=e50ee08)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=e50ee08)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=e50ee08)  
[test/mjsunit/json-recursive.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/json-recursive.js?cl=e50ee08)  
...  
  
  
---   

## **regress-2374.js (v8 issue)**  
   
**[Issue 2374:
 JSON.parse() is broken after r12747](https://crbug.com/v8/2374)**  
**[Commit: Fixed json regression](https://chromium.googlesource.com/v8/v8/+/fa53250)**  
  
Date(Commit): Fri Oct 19 08:23:45 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11186059](https://chromiumcodereview.appspot.com/11186059)  
Regress: [mjsunit/regress/regress-2374.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2374.js)  
```javascript
var msg = '{"result":{"profile":{"head":{"functionName":"(root)","url":"","lineNumber":0,"totalTime":495.7243772462511,"selfTime":0,"numberOfCalls":0,"visible":true,"callUID":2771605942,"children":[{"functionName":"(program)","url":"","lineNumber":0,"totalTime":495.7243772462511,"selfTime":495.7243772462511,"numberOfCalls":0,"visible":true,"callUID":1902715303,"children":[]}]},"bottomUpHead":{"functionName":"(root)","url":"","lineNumber":0,"totalTime":495.7243772462511,"selfTime":0,"numberOfCalls":0,"visible":true,"callUID":2771605942,"children":[{"functionName":"(program)","url":"","lineNumber":0,"totalTime":495.7243772462511,"selfTime":495.7243772462511,"numberOfCalls":0,"visible":true,"callUID":1902715303,"children":[]}]}}},"id":41}';

var obj = JSON.parse(msg);
var obj2 = JSON.parse(msg);

assertEquals(JSON.stringify(obj), JSON.stringify(obj2));
assertEquals(JSON.stringify(obj, null, 0), JSON.stringify(obj2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fa53250^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=fa53250)  
[test/mjsunit/regress/regress-2374.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2374.js?cl=fa53250)  
  

---   

## **regress-2373.js (v8 issue)**  
   
**[Issue 2373:
 JSON.parse() is broken after r12761](https://crbug.com/v8/2373)**  
**[Commit: Fixed error introduced in r12761.](https://chromium.googlesource.com/v8/v8/+/7bc94a9)**  
  
Date(Commit): Thu Oct 18 18:43:19 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/11198068](https://chromiumcodereview.appspot.com/11198068)  
Regress: [mjsunit/regress/regress-2373.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2373.js)  
```javascript
var o = JSON.parse('{"a":2600753951}');
assertEquals(2600753951, o.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bc94a9^!)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=7bc94a9)  
[test/mjsunit/regress/regress-2373.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2373.js?cl=7bc94a9)  
  

---   

## **regress-builtin-array-op.js (other issue)**  
   
**[Commit: Always invoke the default Array.sort functions from builtin functions.](https://chromium.googlesource.com/v8/v8/+/971e834)**  
  
Date(Commit): Thu Oct 18 11:18:08 2012  
Code Review: [https://chromiumcodereview.appspot.com/10559005](https://chromiumcodereview.appspot.com/10559005)  
Regress: [mjsunit/regress/regress-builtin-array-op.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-builtin-array-op.js)  
```javascript
var foo =  "hest";
Array.prototype.sort = function(fn) { foo = "fisk"; };
Function.prototype.call = function() { foo = "caramel"; };
var a = [2,3,1];
a[100000] = 0;
a.join();
assertEquals("hest", foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/971e834^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=971e834)  
[test/mjsunit/regress/regress-builtin-array-op.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-builtin-array-op.js?cl=971e834)  
  
  
---   

## **regress-convert-enum2.js (other issue)**  
   
**[Commit: Invalidate the enum cache when converting a transition across which the descriptors are shared.](https://chromium.googlesource.com/v8/v8/+/7c28995)**  
  
Date(Commit): Mon Oct 15 08:38:51 2012  
Code Review: [https://chromiumcodereview.appspot.com/11145017](https://chromiumcodereview.appspot.com/11145017)  
Regress: [mjsunit/regress/regress-convert-enum2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-enum2.js)  
```javascript
var o = {};
o.a = 1;
o.b = function() { return 1; };
o.d = 2;

for (var x in o) { }

var o1 = {};
o1.a = 1;
o1.b = 10;
o1.c = 20;

var keys = ["a", "b", "c"];

var i = 0;
for (var y in o1) {
  assertEquals(keys[i], y);
  i += 1;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c28995^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7c28995)  
[test/mjsunit/regress/regress-convert-enum2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-convert-enum2.js?cl=7c28995)  
  
  
---   

## **regress-convert-enum.js (other issue)**  
   
**[Commit: Don't clear EnumLength but rather copy the enum cache. Added regression test for crashes from chromecrash.](https://chromium.googlesource.com/v8/v8/+/b75705f)**  
  
Date(Commit): Thu Oct 11 15:33:34 2012  
Code Review: [https://chromiumcodereview.appspot.com/11103036](https://chromiumcodereview.appspot.com/11103036)  
Regress: [mjsunit/regress/regress-convert-enum.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-enum.js)  
```javascript
var o = {};
o.a = 1;
o.c = 2;

var o1 = {};
o1.a = 1;

for (var x in o1) { }
o1.b = function() { return 1; };

o = null;
gc();

var o2 = {};
o2.a = 1;
o2.b = 10;

var o3 = {};
o3.a = 1;

for (var y in o3) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b75705f^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b75705f)  
[test/mjsunit/regress/regress-convert-enum.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-convert-enum.js?cl=b75705f)  
  
  
---   

## **regress-convert-transition.js (other issue)**  
   
**[Commit: Fix transition conversion from CONSTANT_FUNCTION to FIELD.](https://chromium.googlesource.com/v8/v8/+/dde1cdf)**  
  
Date(Commit): Wed Oct 10 12:31:50 2012  
Code Review: [https://chromiumcodereview.appspot.com/11094044](https://chromiumcodereview.appspot.com/11094044)  
Regress: [mjsunit/regress/regress-convert-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-convert-transition.js)  
```javascript
var input = '{ "a1":1, "a2":1, "a3":1, "a4":1, "a5":1, "a6":1, "a7":1,\
               "a8":1, "a9":1, "a10":1, "a11":1, "a12":1, "a13":1}';
var a = JSON.parse(input);
a.a = function() { return 10; };

var b = JSON.parse(input);
b.a = 10;

var c = JSON.parse(input);
c.x = 10;
assertEquals(undefined, c.a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/dde1cdf^!)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=dde1cdf)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=dde1cdf)  
[test/mjsunit/regress/regress-convert-transition.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-convert-transition.js?cl=dde1cdf)  
  
  
---   

## **regress-cnlt-elements.js (other issue)**  
   
**[Commit: Fix CNLT regression.](https://chromium.googlesource.com/v8/v8/+/55e924c)**  
  
Date(Commit): Wed Oct 10 12:29:44 2012  
Code Review: [https://chromiumcodereview.appspot.com/11017054](https://chromiumcodereview.appspot.com/11017054)  
Regress: [mjsunit/regress/regress-cnlt-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cnlt-elements.js)  
```javascript
var a = JSON.parse('{"b":1,"c":2,"d":3,"e":4}');
var b = JSON.parse('{"12040200":1, "a":2, "b":2}');
var c = JSON.parse('{"24050300":1}');
b = null;
gc();
gc();
c.a1 = 2;
c.a2 = 2;
c.a3 = 2;
c.a4 = 2;
c.a5 = 2;
c.a6 = 2;
c.a7 = 2;
c.a8 = 2;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/55e924c^!)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=55e924c)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=55e924c)  
[test/mjsunit/regress/regress-cnlt-elements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-cnlt-elements.js?cl=55e924c)  
  
  
---   

## **regress-143967.js (chromium issue)**  
   
**[Issue 143967:
 Small JavaScript Input Causes Hang in FunctionGetPrototype](https://crbug.com/143967)**  
**[Commit: Fixed Accessors::FunctionGetPrototype's proto chain traversal.](https://chromium.googlesource.com/v8/v8/+/5d11c5e)**  
  
Date(Commit): Mon Oct 08 12:58:46 2012  
Components/Type: Blink/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/11087004](https://codereview.chromium.org/11087004)  
Regress: [mjsunit/regress/regress-143967.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-143967.js)  
```javascript
var functionWithoutProto = [].filter;
var obj = Object.create(functionWithoutProto);
functionWithoutProto.__proto__ = function() {};
assertEquals(functionWithoutProto.prototype, obj.prototype);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d11c5e^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=5d11c5e)  
[test/mjsunit/regress/regress-143967.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-143967.js?cl=5d11c5e)  
  

---   

## **regress-2322.js (v8 issue)**  
   
**[Issue 2322:
 Crash in "let" scope resolution](https://crbug.com/v8/2322)**  
**[Commit: Reject uses of lexical for-loop variable on the RHS.](https://chromium.googlesource.com/v8/v8/+/3f7b5c3)**  
  
Date(Commit): Fri Oct 05 09:07:53 2012  
Type: Bug  
Code Review: [https://codereview.chromium.org/11031045](https://codereview.chromium.org/11031045)  
Regress: [mjsunit/es6/regress/regress-2322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2322.js)  
```javascript
"use strict";

assertThrows("'use strict'; for (let x in x);", ReferenceError);

let s;
for (let pppp in {}) {};
assertThrows(function() { pppp = true }, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f7b5c3^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=3f7b5c3)  
[test/mjsunit/regress/regress-2322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2322.js?cl=3f7b5c3)  
  

---   

## **regress-2339.js (v8 issue)**  
   
**[Issue 2339:
 Wrong handling of constant operand 0 in integer multiplication.](https://crbug.com/v8/2339)**  
**[Commit: Avoid wrong imul deopt on ia32 and x64 (fixes v8 bug 2339).](https://chromium.googlesource.com/v8/v8/+/8fbfad6)**  
  
Date(Commit): Wed Sep 26 09:57:30 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10963032](https://chromiumcodereview.appspot.com/10963032)  
Regress: [mjsunit/regress/regress-2339.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2339.js)  
```javascript
function simple() {
  return simple_two_args(0, undefined);
}

function simple_two_args(always_zero, always_undefined) {
  var always_five = always_undefined || 5;
  return always_zero * always_five * .5;
}
%EnsureFeedbackVectorForFunction(simple_two_args);


%PrepareFunctionForOptimization(simple);
simple();
simple();
%OptimizeFunctionOnNextCall(simple);
simple();
assertOptimized(simple);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8fbfad6^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=8fbfad6)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=8fbfad6)  
[test/mjsunit/regress/regress-1117.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1117.js?cl=8fbfad6)  
[test/mjsunit/regress/regress-2339.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2339.js?cl=8fbfad6)  
  

---   

## **regress-2346.js (v8 issue)**  
   
**[Issue 2346:
 Generic KeyedStoreIC doesn't change length and element_kind atomically](https://crbug.com/v8/2346)**  
**[Commit: x64 and ARM: Fix issue 2346 (order of operations in keyed store](https://chromium.googlesource.com/v8/v8/+/72e9f1b)**  
  
Date(Commit): Tue Sep 25 13:35:42 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10985017](https://chromiumcodereview.appspot.com/10985017)  
Regress: [mjsunit/regress/regress-2346.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2346.js)  
```javascript
function get() { return x; }
function set(x) { this.x = x; }

var obj = {x: 1};
obj.__defineGetter__("accessor", get);
obj.__defineSetter__("accessor", set);
var a = new Array();
a[1] = 42;
obj[1] = 42;

var descIsData = Object.getOwnPropertyDescriptor(obj, 'x');
assertTrue(descIsData.enumerable);
assertTrue(descIsData.writable);
assertTrue(descIsData.configurable);

var descIsAccessor = Object.getOwnPropertyDescriptor(obj, 'accessor');
assertTrue(descIsAccessor.enumerable);
assertTrue(descIsAccessor.configurable);
assertTrue(descIsAccessor.get == get);
assertTrue(descIsAccessor.set == set);

var descIsNotData = Object.getOwnPropertyDescriptor(obj, 'not-x');
assertTrue(descIsNotData == undefined);

var descIsNotAccessor = Object.getOwnPropertyDescriptor(obj, 'not-accessor');
assertTrue(descIsNotAccessor == undefined);

var descArray = Object.getOwnPropertyDescriptor(a, '1');
assertTrue(descArray.enumerable);
assertTrue(descArray.configurable);
assertTrue(descArray.writable);
assertEquals(descArray.value, 42);

var descObjectElement = Object.getOwnPropertyDescriptor(obj, '1');
assertTrue(descObjectElement.enumerable);
assertTrue(descObjectElement.configurable);
assertTrue(descObjectElement.writable);
assertEquals(descObjectElement.value, 42);

var a = new String('foobar');
for (var i = 0; i < a.length; i++) {
  var descStringObject = Object.getOwnPropertyDescriptor(a, i);
  assertTrue(descStringObject.enumerable);
  assertFalse(descStringObject.configurable);
  assertFalse(descStringObject.writable);
  assertEquals(descStringObject.value, a.substring(i, i+1));
}

a.x = 42;
a[10] = 'foo';
var descStringProperty = Object.getOwnPropertyDescriptor(a, 'x');
assertTrue(descStringProperty.enumerable);
assertTrue(descStringProperty.configurable);
assertTrue(descStringProperty.writable);
assertEquals(descStringProperty.value, 42);

var descStringElement = Object.getOwnPropertyDescriptor(a, '10');
assertTrue(descStringElement.enumerable);
assertTrue(descStringElement.configurable);
assertTrue(descStringElement.writable);
assertEquals(descStringElement.value, 'foo');

var proto = {};
proto[10] = 42;

var objWithProto = new Array();
objWithProto.prototype = proto;
objWithProto[0] = 'bar';
var descWithProto = Object.getOwnPropertyDescriptor(objWithProto, '10');
assertEquals(undefined, descWithProto);

var global = (function() { return this; })();

global[42] = 42;

function el_getter() { return 239; };
function el_setter() {};
Object.defineProperty(global, '239', {get: el_getter, set: el_setter});

var descRegularElement = Object.getOwnPropertyDescriptor(global, '42');
assertEquals(42, descRegularElement.value);

var descAccessorElement = Object.getOwnPropertyDescriptor(global, '239');
assertEquals(el_getter, descAccessorElement.get);
assertEquals(el_setter, descAccessorElement.set);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/72e9f1b^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=72e9f1b)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=72e9f1b)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=72e9f1b)  
[src/ic.h](https://cs.chromium.org/chromium/src/v8/src/ic.h?cl=72e9f1b)  
[src/x64/ic-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/ic-x64.cc?cl=72e9f1b)  
...  
  

---   

## **regress-cnlt-enum-indices.js (other issue)**  
   
**[Commit: Fix CNLT for enum indices.](https://chromium.googlesource.com/v8/v8/+/083ee63)**  
  
Date(Commit): Thu Sep 20 15:18:00 2012  
Code Review: [https://chromiumcodereview.appspot.com/10958015](https://chromiumcodereview.appspot.com/10958015)  
Regress: [mjsunit/regress/regress-cnlt-enum-indices.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cnlt-enum-indices.js)  
```javascript
var o = {};
var o2 = {};

o.a = 1;
o2.a = 1;
function f() { return 10; }
Object.defineProperty(o, "b", { get: f, enumerable: true });
Object.defineProperty(o2, "b", { get: f, enumerable: true });
assertTrue(%HaveSameMap(o, o2));
o.c = 2;

for (var x in o) { }
o = null;

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/083ee63^!)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=083ee63)  
[test/mjsunit/regress/regress-cnlt-enum-indices.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-cnlt-enum-indices.js?cl=083ee63)  
  
  
---   

## **regress-undefined-store-keyed-fast-element.js (other issue)**  
   
**[Commit: Deopt on storing undefined into double elements.](https://chromium.googlesource.com/v8/v8/+/ea31f86)**  
  
Date(Commit): Thu Sep 20 13:41:00 2012  
Code Review: [https://chromiumcodereview.appspot.com/10963010](https://chromiumcodereview.appspot.com/10963010)  
Regress: [mjsunit/regress/regress-undefined-store-keyed-fast-element.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-store-keyed-fast-element.js)  
```javascript
function f(v) {
  return [0.0, 0.1, 0.2, v];
};
%PrepareFunctionForOptimization(f);
assertEquals([0.0, 0.1, 0.2, NaN], f(NaN));
assertEquals([0.0, 0.1, 0.2, NaN], f(NaN));
%OptimizeFunctionOnNextCall(f);
assertEquals([0.0, 0.1, 0.2, undefined], f(undefined));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ea31f86^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=ea31f86)  
[test/mjsunit/elements-transition-hoisting.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/elements-transition-hoisting.js?cl=ea31f86)  
[test/mjsunit/regress/regress-undefined-store-keyed-fast-element.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-undefined-store-keyed-fast-element.js?cl=ea31f86)  
  
  
---   

## **regress-crbug-150729.js (chromium issue)**  
   
**[Issue 150729:
 UNKNOWN in v8::internal::Invoke](https://crbug.com/150729)**  
**[Commit: Fix LBoundsCheck on x64 to handle (stack slot + constant) correctly](https://chromium.googlesource.com/v8/v8/+/a8e502f)**  
  
Date(Commit): Thu Sep 20 09:56:24 2012  
Components/Type: Blink/Bug-Security  
Labels: ["reward-1500", "Merge-Merged", "M-23", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic"]  
Code Review: [https://codereview.chromium.org/10959009](https://codereview.chromium.org/10959009)  
Regress: [mjsunit/regress/regress-crbug-150729.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-150729.js)  
```javascript
var t = 0;
function burn() {
  i = [t, 1];
  var M = [i[0], Math.cos(t) + i[7074959]];
  t += .05;
};
%PrepareFunctionForOptimization(burn);
for (var j = 0; j < 5; j++) {
  if (j == 2) %OptimizeFunctionOnNextCall(burn);
  burn();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8e502f^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=a8e502f)  
[test/mjsunit/regress/regress-crbug-150729.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-150729.js?cl=a8e502f)  
  

---   

## **regress-crbug-146910.js (chromium issue)**  
   
**[Issue 146910:
 setting an array's .length can delete non-configurable properties](https://crbug.com/146910)**  
**[Commit: Fix setting array length to zero for slow elements.](https://chromium.googlesource.com/v8/v8/+/c012afb)**  
  
Date(Commit): Wed Sep 19 11:52:33 2012  
Components/Type: Blink/Bug  
Labels: []  
Code Review: [https://codereview.chromium.org/10937026](https://codereview.chromium.org/10937026)  
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
  

---   

## **regress-crbug-150545.js (chromium issue)**  
   
**[Issue 150545:
 UNKNOWN in v8::internal::RootMarkingVisitor::MarkObjectByPointer](https://crbug.com/150545)**  
**[Commit: Fix lost arguments dropping in HLeaveInlined.](https://chromium.googlesource.com/v8/v8/+/f0dcaf9)**  
  
Date(Commit): Wed Sep 19 08:13:46 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Release-0", "Stability-Memory-AddressSanitizer", "CVE-2013-0836", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "Clusterfuzz", "M-24", "CVE_description-submitted"]  
Code Review: [https://codereview.chromium.org/10938016](https://codereview.chromium.org/10938016)  
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
  %EnsureFeedbackVectorForFunction(inner);

  function outer() {
    inner(1,2,3);
    for (var i = 0; i < 3; i++) %OptimizeOsr();
  }
  %PrepareFunctionForOptimization(outer);

  outer();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0dcaf9^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=f0dcaf9)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=f0dcaf9)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=f0dcaf9)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=f0dcaf9)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=f0dcaf9)  
...  
  

---   

## **regress-cntl-descriptors-enum.js (other issue)**  
   
**[Commit: CNLT with descriptors but no valid enum fields has to clear the EnumCache.](https://chromium.googlesource.com/v8/v8/+/ad4746c)**  
  
Date(Commit): Fri Sep 14 13:15:43 2012  
Code Review: [https://chromiumcodereview.appspot.com/10928204](https://chromiumcodereview.appspot.com/10928204)  
Regress: [mjsunit/regress/regress-cntl-descriptors-enum.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-cntl-descriptors-enum.js)  
```javascript
var o = {};
Object.defineProperty(o, "a", {
    value: 0, configurable: true, writable: true, enumerable: false
});

var o2 = {};
Object.defineProperty(o2, "a", {
    value: 0, configurable: true, writable: true, enumerable: false
});


assertTrue(%HaveSameMap(o, o2));

o.y = 2;

for (var v in o) { print(v); }
o = {};
gc();

for (var v in o2) { print(v); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ad4746c^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ad4746c)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=ad4746c)  
[test/mjsunit/regress/regress-cntl-descriptors-enum.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-cntl-descriptors-enum.js?cl=ad4746c)  
  
  
---   

## **regress-2326.js (v8 issue)**  
   
**[Issue 2326:
 Fatal error: CHECK(BailoutId(data->OsrAstId()->value()) == ast_id) failed](https://crbug.com/v8/2326)**  
**[Commit: Fix caching of optimized code for OSR.](https://chromium.googlesource.com/v8/v8/+/77a7d9f)**  
  
Date(Commit): Fri Sep 14 10:41:31 2012  
Type: Bug  
Code Review: [https://codereview.chromium.org/10933088](https://codereview.chromium.org/10933088)  
Regress: [mjsunit/regress/regress-2326.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2326.js)  
```javascript
function makeClosure() {
  function f(mode, iterations) {
    var accumulator = 0;
    if (mode == 1) {
      while (--iterations > 0) accumulator = Math.ceil(accumulator);
      return 1;
    } else {
      while (--iterations > 0) accumulator = Math.floor(accumulator);
      return 2;
    }
  }
  return f;
}

var f1 = makeClosure();
var f2 = makeClosure();

assertSame(1, f1(1, 100000));

assertSame(2, f2(2, 100000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/77a7d9f^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=77a7d9f)  
[test/mjsunit/regress/regress-2326.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2326.js?cl=77a7d9f)  
  

---   

## **regress-crbug-148376.js (chromium issue)**  
   
**[Issue 148376:
 [LangFuzz] Crash at v8::internal::MarkCompactCollector::EvacuateNewSpace with invalid read](https://crbug.com/148376)**  
**[Commit: Ensure correct enumeration indices in the dict](https://chromium.googlesource.com/v8/v8/+/1d1adaf)**  
  
Date(Commit): Thu Sep 13 08:52:55 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Reward-1000", "M-23", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10908216](https://chromiumcodereview.appspot.com/10908216)  
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
  

---   

## **regress-148378.js (chromium issue)**  
   
**[Issue 148378:
 [LangFuzz] Crash due to invalid free in v8::internal::Runtime_RegExpExecMultiple](https://crbug.com/148378)**  
**[Commit: Correctly initialize regexp global cache.](https://chromium.googlesource.com/v8/v8/+/67d0506)**  
  
Date(Commit): Wed Sep 12 15:26:43 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Reward-1000", "M-23", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10905239](https://chromiumcodereview.appspot.com/10905239)  
Regress: [mjsunit/regress/regress-148378.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-148378.js)  
```javascript
"a".replace(/a/g, function() { return "c"; });

function test() {
  try {
    test();
  } catch(e) {
    "b".replace(/(b)/g, function() { return "c"; });
  }
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/67d0506^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=67d0506)  
[test/mjsunit/regress/regress-148378.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-148378.js?cl=67d0506)  
  

---   

## **regress-2261.js (v8 issue)**  
   
**[Issue 2261:
 Materialization of arguments object during deopt in strict mode yields wrong values](https://crbug.com/v8/2261)**  
**[Commit: Fix arguments object materialization during deopt.](https://chromium.googlesource.com/v8/v8/+/f37f504)**  
  
Date(Commit): Wed Sep 12 12:28:42 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10908194](https://chromiumcodereview.appspot.com/10908194)  
Regress: [mjsunit/regress/regress-2261.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2261.js)  
```javascript
(function () {
  var forceDeopt = 0;
  function inner(x) {
    "use strict";
    x = 2;
    // Do not remove this %DebugPrint as it makes sure the deopt happens
    // after the assignment and is not hoisted above the assignment.
    %DebugPrint(arguments[0]);
    forceDeopt + 1;
    return arguments[0];
  };
  %PrepareFunctionForOptimization(inner);
  assertEquals(1, inner(1));
  assertEquals(1, inner(1));
  %OptimizeFunctionOnNextCall(inner);
  assertEquals(1, inner(1));
  forceDeopt = "not a number";
  assertEquals(1, inner(1));
})();



(function () {
  var forceDeopt = 0;
  function inner(x) {
    "use strict";
    x = 2;
    // Do not remove this %DebugPrint as it makes sure the deopt happens
    // after the assignment and is not hoisted above the assignment.
    %DebugPrint(arguments[0]);
    forceDeopt + 1;
    return arguments[0];
  }

  function outer(x) {
    return inner(x);
  };
  %PrepareFunctionForOptimization(outer);
  assertEquals(1, outer(1));
  assertEquals(1, outer(1));
  %OptimizeFunctionOnNextCall(outer);
  assertEquals(1, outer(1));
  forceDeopt = "not a number";
  assertEquals(1, outer(1));
})();



(function () {
  var forceDeopt = 0;
  function inner(x, y, z) {
    "use strict";
    x = 3;
    // Do not remove this %DebugPrint as it makes sure the deopt happens
    // after the assignment and is not hoisted above the assignment.
    %DebugPrint(arguments[0]);
    forceDeopt + 1;
    return arguments[0];
  }

  function middle(x) {
    "use strict";
    x = 2;
    return inner(10 * x, 20 * x, 30 * x) + arguments[0];
  }

  function outer(x) {
    return middle(x);
  };
  %PrepareFunctionForOptimization(outer);
  assertEquals(21, outer(1));
  assertEquals(21, outer(1));
  %OptimizeFunctionOnNextCall(outer);
  assertEquals(21, outer(1));
  forceDeopt = "not a number";
  assertEquals(21, outer(1));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f37f504^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=f37f504)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=f37f504)  
[src/arm/lithium-codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.h?cl=f37f504)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=f37f504)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=f37f504)  
...  
  

---   

## **regress-crbug-147475.js (chromium issue)**  
   
**[Issue 147475:
 UNKNOWN in v8::internal::Deoptimizer::DoComputeOutputFrames](https://crbug.com/147475)**  
**[Commit: Fix deoptimizer for shared optimized code.](https://chromium.googlesource.com/v8/v8/+/f6cd240)**  
  
Date(Commit): Mon Sep 10 11:05:17 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Memory-AddressSanitizer", "Merge-Merged", "Security_Impact-Stable", "M-22", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromiumcodereview.appspot.com/10917162](https://chromiumcodereview.appspot.com/10917162)  
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
  

---   

## **regress-crbug-134609.js (chromium issue)**  
   
**[Issue 134609:
 Chrome: Crash Report -         Stack Signature: v8::internal::JSReceiver::Lookup(v8::intern...](https://crbug.com/134609)**  
**[Commit: Fixed deoptimization of inlined getters.](https://chromium.googlesource.com/v8/v8/+/7af6883)**  
  
Date(Commit): Fri Sep 07 09:01:54 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "ReleaseBlock-Beta", "M-22"]  
Code Review: [https://chromiumcodereview.appspot.com/10910110](https://chromiumcodereview.appspot.com/10910110)  
Regress: [mjsunit/regress/regress-crbug-134609.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-134609.js)  
```javascript
var forceDeopt = {x: 0};

var objectWithGetterProperty = function(value) {
  var obj = {};
  Object.defineProperty(obj, 'getterProperty', {
    get: function foo() {
      forceDeopt.x;
      return value;
    }
  });

  return obj;
}('bad');

function test() {
  var iAmContextAllocated = "good";
  objectWithGetterProperty.getterProperty;
  return iAmContextAllocated;

  // Make sure that the local variable is context allocated.
  function unused() {
    iAmContextAllocated;
  }
};
%PrepareFunctionForOptimization(test);
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
...  
  

---   

## **regress-145201.js (chromium issue)**  
   
**[Issue 145201:
 Security: V8 builtin functions exposed via operators and Function.prototype.caller](https://crbug.com/145201)**  
**[Commit: Fix some corner cases in skipping native methods using caller.](https://chromium.googlesource.com/v8/v8/+/e5df028)**  
  
Date(Commit): Wed Sep 05 08:19:49 2012  
Components/Type: Blink/Bug  
Labels: ["Release-0", "Security_severity-None", "Merge-Merged", "M-24"]  
Code Review: [https://chromiumcodereview.appspot.com/10911063](https://chromiumcodereview.appspot.com/10911063)  
Regress: [mjsunit/regress/regress-145201.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-145201.js)  
```javascript
var net = [];


var x = 0;

function collect () {
  function item(operator) {
    binary(operator, 1, false);
    binary(operator, 1, true);
    binary(operator, '{}', false);
    binary(operator, '{}', true);
    binary(operator, '"x"', false);
    binary(operator, '"x"', true);
    unary(operator, "");
  }

  function unary(op, after) {
    // Capture:
    try {
      eval(op + " custom " + after);
    } catch(e) {
    }
  }

  function binary(op, other_side, inverted) {
    // Capture:
    try {
      if (inverted) {
        eval("custom " + op + " " + other_side);
      } else {
        eval(other_side + " " + op + " custom");
      }
    } catch(e) {
    }
  }

  function catcher() {
    var caller = catcher.caller;
    if (/native/i.test(caller) || /ADD/.test(caller)) {
      net[caller] = 0;
    }
  }

  var custom = Object.create(null, {
    toString: { value: catcher },
    length: { get: catcher }
  });

  item('^');
  item('~');
  item('<<');
  item('<');
  item('==');
  item('>>>');
  item('>>');
  item('|');
  item('-');
  item('*');
  item('&');
  item('%');
  item('+');
  item('in');
  item('instanceof');
  unary('{}[', ']');
  unary('delete {}[', ']');
  unary('(function() {}).apply(null, ', ')');
}

collect();
collect();
collect();

var keys = 0;
for (var key in net) {
  print(key);
  keys++;
}

assertTrue(keys == 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5df028^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=e5df028)  
[test/mjsunit/regress/regress-145201.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-145201.js?cl=e5df028)  
  

---   

## **regress-crbug-145961.js (chromium issue)**  
   
**[Issue 145961:
 Trix / google spreadsheets have selection issues on ChromeOS](https://crbug.com/145961)**  
**[Commit: Support register as right operand in min/max support.](https://chromium.googlesource.com/v8/v8/+/a8638c1)**  
  
Date(Commit): Tue Sep 04 09:35:43 2012  
Components/Type: Blink/Bug  
Labels: ["Hotlist-GoogleApps"]  
Code Review: [https://chromiumcodereview.appspot.com/10914072](https://chromiumcodereview.appspot.com/10914072)  
Regress: [mjsunit/regress/regress-crbug-145961.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-145961.js)  
```javascript
function test() {
  var a = new Int32Array(2);
  var x = a[0];
  return Math.min(x, x);
};
%PrepareFunctionForOptimization(test);
assertEquals(0, test());
assertEquals(0, test());
%OptimizeFunctionOnNextCall(test);
assertEquals(0, test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a8638c1^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=a8638c1)  
[test/mjsunit/regress/regress-crbug-145961.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-145961.js?cl=a8638c1)  
  

---   

## **regress-load-elements.js (other issue)**  
   
**[Commit: Elements load depends on the type of the receiver.](https://chromium.googlesource.com/v8/v8/+/90db487)**  
  
Date(Commit): Thu Aug 30 17:31:32 2012  
Code Review: [https://chromiumcodereview.appspot.com/10918005](https://chromiumcodereview.appspot.com/10918005)  
Regress: [mjsunit/regress/regress-load-elements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-load-elements.js)  
```javascript
function bad_func(o, a) {
  for (var i = 0; i < 1; ++i) {
    o.prop = 0;
    var x = a[0];
  }
};
%PrepareFunctionForOptimization(bad_func);
o = new Object();
a = {};
a[0] = 1;
bad_func(o, a);

o = new Object();
bad_func(o, a);

%OptimizeFunctionOnNextCall(bad_func);
bad_func(o, "");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/90db487^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=90db487)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=90db487)  
[test/mjsunit/regress/regress-load-elements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-load-elements.js?cl=90db487)  
  
  
---   

## **regress-2250.js (v8 issue)**  
   
**[Issue 2250:
 LICM is too aggressive with moving checks across conditional control flow](https://crbug.com/v8/2250)**  
**[Commit: Disable speculative LICM when it may lead to unnecessary deopts](https://chromium.googlesource.com/v8/v8/+/3544e2e)**  
  
Date(Commit): Thu Aug 23 21:08:58 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10867033](https://chromiumcodereview.appspot.com/10867033)  
Regress: [mjsunit/regress/regress-2250.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2250.js)  
```javascript
function eq(a, b) {
  if (typeof b === "object") {
    return b.equals(a);  // (*)
  }
  return a === b;
}

Object.prototype.equals = function (other) {
  return (this === other);
};

function test() {
  for (var i = 0; !eq(i, 10); i++)
    ;
}

eq({}, {});
eq({}, {});
eq(1, 1);
eq(1, 1);
test();
%OptimizeFunctionOnNextCall(test);
test();
%OptimizeFunctionOnNextCall(test);
test();
assertOptimized(test);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3544e2e^!)  
[src/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap.cc?cl=3544e2e)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3544e2e)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=3544e2e)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=3544e2e)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=3544e2e)  
...  
  

---   

## **regress-2294.js (v8 issue)**  
   
**[Issue 2294:
 Apparently incorrect rounding in Uint8ClampedArray implementation in V8](https://crbug.com/v8/2294)**  
**[Commit: Fix rounding in Uint8ClampedArray setter.](https://chromium.googlesource.com/v8/v8/+/efc26f9)**  
  
Date(Commit): Wed Aug 22 14:27:11 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10831409](https://chromiumcodereview.appspot.com/10831409)  
Regress: [mjsunit/regress/regress-2294.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2294.js)  
```javascript
var clampedArray = new Uint8ClampedArray(10);

function test() {
  clampedArray[0] = 0.499;
  assertEquals(0, clampedArray[0]);
  clampedArray[0] = 0.5;
  assertEquals(0, clampedArray[0]);
  clampedArray[0] = 0.501;
  assertEquals(1, clampedArray[0]);
  clampedArray[0] = 1.499;
  assertEquals(1, clampedArray[0]);
  clampedArray[0] = 1.5;
  assertEquals(2, clampedArray[0]);
  clampedArray[0] = 1.501;
  assertEquals(2, clampedArray[0]);
  clampedArray[0] = 2.5;
  assertEquals(2, clampedArray[0]);
  clampedArray[0] = 3.5;
  assertEquals(4, clampedArray[0]);
  clampedArray[0] = 252.5;
  assertEquals(252, clampedArray[0]);
  clampedArray[0] = 253.5;
  assertEquals(254, clampedArray[0]);
  clampedArray[0] = 254.5;
  assertEquals(254, clampedArray[0]);
  clampedArray[0] = 256.5;
  assertEquals(255, clampedArray[0]);
  clampedArray[0] = -0.5;
  assertEquals(0, clampedArray[0]);
  clampedArray[0] = -1.5;
  assertEquals(0, clampedArray[0]);
  clampedArray[0] = 1000000000000;
  assertEquals(255, clampedArray[0]);
  clampedArray[0] = -1000000000000;
  assertEquals(0, clampedArray[0]);
};
%PrepareFunctionForOptimization(test);
test();
test();
%OptimizeFunctionOnNextCall(test);
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/efc26f9^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=efc26f9)  
[src/ia32/assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.cc?cl=efc26f9)  
[src/ia32/assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/ia32/assembler-ia32.h?cl=efc26f9)  
[src/ia32/disasm-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/disasm-ia32.cc?cl=efc26f9)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=efc26f9)  
...  
  

---   

## **regress-crbug-142218.js (chromium issue)**  
   
**[Issue 142218:
 LLVM cPython no longer works](https://crbug.com/142218)**  
**[Commit: Check that index and length are Smi in bounds check.](https://chromium.googlesource.com/v8/v8/+/5df5eea)**  
  
Date(Commit): Tue Aug 21 16:46:25 2012  
Components/Type: Blink/Bug  
Labels: []  
Code Review: [https://chromiumcodereview.appspot.com/10829456](https://chromiumcodereview.appspot.com/10829456)  
Regress: [mjsunit/regress/regress-crbug-142218.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-142218.js)  
```javascript
length = 1 << 16;
a = new Array(length);

function insert_element(key) {
  a[key] = 42;
};
%PrepareFunctionForOptimization(insert_element);
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
...  
  

---   

## **regress-2291.js (v8 issue)**  
   
**[Issue 2291:
 Object equality check error](https://crbug.com/v8/2291)**  
**[Commit: Fix bug in compare IC.  BUG=2291](https://chromium.googlesource.com/v8/v8/+/ee3a66b)**  
  
Date(Commit): Wed Aug 15 15:08:42 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10830334](https://chromiumcodereview.appspot.com/10830334)  
Regress: [mjsunit/regress/regress-2291.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2291.js)  
```javascript
function StrictCompare(x) { return x === Object(x); }

var obj = new Object();
var obj2 = new Object();
obj == obj;  // Populate IC cache with non-strict comparison.

StrictCompare(obj);  // Set IC in StrictCompare from IC cache.

assertFalse(StrictCompare('foo'));  // Use == stub for === operation.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee3a66b^!)  
[src/code-stubs.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs.cc?cl=ee3a66b)  
[src/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap.h?cl=ee3a66b)  
[test/mjsunit/regress/regress-2291.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2291.js?cl=ee3a66b)  
  

---   

## **regress-2289.js (v8 issue)**  
   
**[Issue 2289:
 string.replace crashes for very long strings](https://crbug.com/v8/2289)**  
**[Commit: Ensure capacity when adding parts in String.replace.](https://chromium.googlesource.com/v8/v8/+/28c8929)**  
  
Date(Commit): Tue Aug 14 11:33:12 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10830304](https://chromiumcodereview.appspot.com/10830304)  
Regress: [mjsunit/regress/regress-2289.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2289.js)  
```javascript
var foo = "a";
for (var i = 0; i < 12; i++) foo += foo;
foo = foo + 'b' + foo;

foo.replace(/b/, "a");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/28c8929^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=28c8929)  
[test/mjsunit/regress/regress-2289.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2289.js?cl=28c8929)  
  

---   

## **regress-2286.js (v8 issue)**  
   
**[Issue 2286:
 segfault when calling unknown inline runtime call.](https://crbug.com/v8/2286)**  
**[Commit: Prevent segfault on undefined inline runtime call.](https://chromium.googlesource.com/v8/v8/+/d3733ce)**  
  
Date(Commit): Tue Aug 14 10:06:34 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10828282](https://chromiumcodereview.appspot.com/10828282)  
Regress: [mjsunit/regress/regress-2286.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2286.js)  
```javascript
assertThrows("f()", ReferenceError);
assertThrows("%f()", Error);
assertThrows("%_f()", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d3733ce^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=d3733ce)  
[test/mjsunit/regress-2286.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-2286.js?cl=d3733ce)  
  

---   

## **regress-2284.js (v8 issue)**  
   
**[Issue 2284:
 Calling %constructor() on the builtins global object causes crashes](https://crbug.com/v8/2284)**  
**[Commit: Remove prototype of global builtins object.](https://chromium.googlesource.com/v8/v8/+/e77f24f)**  
  
Date(Commit): Mon Aug 13 15:34:49 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10854116](https://chromiumcodereview.appspot.com/10854116)  
Regress: [mjsunit/regress/regress-2284.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2284.js)  
```javascript
assertThrows("%foobar();", Error);
assertThrows("%constructor();", Error);
assertThrows("%constructor(23);", Error);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e77f24f^!)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=e77f24f)  
[test/mjsunit/regress/regress-2284.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2284.js?cl=e77f24f)  
  

---   

## **regress-crbug-142087.js (chromium issue)**  
   
**[Issue 142087:
 UNKNOWN in void v8::internal::String::WriteToFlat<char>](https://crbug.com/142087)**  
**[Commit: Fix wrong indexing in global regexp.](https://chromium.googlesource.com/v8/v8/+/960b1af)**  
  
Date(Commit): Mon Aug 13 15:26:46 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Memory-AddressSanitizer", "M-22", "Security_Impact-None", "Security_Severity-High", "allpublic", "Clusterfuzz"]  
Code Review: [https://chromiumcodereview.appspot.com/10824278](https://chromiumcodereview.appspot.com/10824278)  
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
  

---   

## **regress-crbug-140083.js (chromium issue)**  
   
**[Issue 140083:
 [LangFuzz] Crash on heap trying to execute address 0x0000000200000000.](https://crbug.com/140083)**  
**[Commit: Fixed compound/count operations with getter-only accessor properties.](https://chromium.googlesource.com/v8/v8/+/83fc420)**  
  
Date(Commit): Fri Aug 03 09:45:08 2012  
Components/Type: None/Bug-Security  
Labels: ["Reward-1000", "M-22", "Security_Severity-High", "Security_Impact-Beta", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10828146](https://chromiumcodereview.appspot.com/10828146)  
Regress: [mjsunit/regress/regress-crbug-140083.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-140083.js)  
```javascript
Object.defineProperty(Object.prototype, 'foo', {
  get: function() {
    return 123;
  }
});

function bar(o) {
  o.foo += 42;
  o.foo++;
};
%PrepareFunctionForOptimization(bar);
var baz = {};
bar(baz);
bar(baz);
%OptimizeFunctionOnNextCall(bar);
bar(baz);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/83fc420^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=83fc420)  
[test/mjsunit/regress/regress-crbug-140083.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-140083.js?cl=83fc420)  
  

---   

## **regress-crbug-138887.js (chromium issue)**  
   
**[Issue 138887:
 AirAsia.com is broken: Chrome is throwing an "Uncaught RangeError: Maximum call stack size exceeded" since build 143896](https://crbug.com/138887)**  
**[Commit: Always set the callee's context when calling a function from optimized code.](https://chromium.googlesource.com/v8/v8/+/80c35c6)**  
  
Date(Commit): Thu Jul 26 12:49:08 2012  
Components/Type: Blink/Compat  
Labels: ["Hotlist-Japan", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10834031](https://chromiumcodereview.appspot.com/10834031)  
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
assertEquals(11, f2(1));  // Result: 1 + f1(0) == 1 + 10.
assertEquals(11, f2(1));
%OptimizeFunctionOnNextCall(f1);
assertEquals(10, f1(0));  // Terminates immediately -> returns 10.
%OptimizeFunctionOnNextCall(f2);
assertEquals(102, f2(1000));  // 1 + f1(999) == 1 + 1 + worker1(998) == 102  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/80c35c6^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=80c35c6)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=80c35c6)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=80c35c6)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=80c35c6)  
[test/mjsunit/regress/regress-crbug-138887.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-138887.js?cl=80c35c6)  
  

---   

## **regress-137768.js (chromium issue)**  
   
**[Issue 137768:
 San Angeles WebGL demo crashes the renderer process, related to V8 roll](https://crbug.com/137768)**  
**[Commit: Add dependency to HLoadKeyed* instructions to prevent invalid hoisting](https://chromium.googlesource.com/v8/v8/+/3667f92)**  
  
Date(Commit): Mon Jul 23 13:59:24 2012  
Components/Type: Blink/Bug  
Labels: ["Stability-Crash", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10802038](https://chromiumcodereview.appspot.com/10802038)  
Regress: [mjsunit/regress/regress-137768.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-137768.js)  
```javascript
function TestConstructor() {
  this[0] = 1;
  this[1] = 2;
  this[2] = 3;
}

function bad_func(o, a) {
  var s = 0;
  for (var i = 0; i < 1; ++i) {
    o.newFileToChangeMap = undefined;
    var x = a[0];
    s += x;
  }
  return s;
};
%PrepareFunctionForOptimization(bad_func);
o = new Object();
a = new TestConstructor();
bad_func(o, a);

o = new Object();
a = new TestConstructor();
bad_func(o, a);

o = new Object();
a = new TestConstructor();
%OptimizeFunctionOnNextCall(bad_func);
bad_func(o, a);

o = new Object();
a = [2.122e-314, 2.122e-314, 2.122e-314];
bad_func(o, a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3667f92^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=3667f92)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=3667f92)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3667f92)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=3667f92)  
[test/mjsunit/regress/regress-137768.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-137768.js?cl=3667f92)  
  

---   

## **regress-2249.js (v8 issue)**  
   
**[Issue 2249:
 Whiteness witness triggers when transforming dictionary to fast elements if the backing store is the empty array.](https://crbug.com/v8/2249)**  
**[Commit: Fix corner case when transforming dictionary to fast elements.](https://chromium.googlesource.com/v8/v8/+/50bf19a)**  
  
Date(Commit): Mon Jul 23 08:41:53 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10802051](https://chromiumcodereview.appspot.com/10802051)  
Regress: [mjsunit/regress/regress-2249.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2249.js)  
```javascript
var o = {};
o[Math.pow(2,30)-1] = 0;
o[Math.pow(2,31)-1] = 0;
o[1] = 0;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/50bf19a^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=50bf19a)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=50bf19a)  
[test/mjsunit/regress-2249.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress-2249.js?cl=50bf19a)  
  

---   

## **regress-crbug-137689.js (chromium issue)**  
   
**[Issue 137689:
 REGRESSION: about:gpu broken](https://crbug.com/137689)**  
**[Commit: When following an accessor transition for an already existing accessor, don't load the last added descriptor but the same descriptor as we already found previously.](https://chromium.googlesource.com/v8/v8/+/90c7cb1)**  
  
Date(Commit): Wed Jul 18 09:20:57 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "WebKit-ID-91571-ASSIGNED", "Stability-Crash", "ReleaseBlock-Dev", "M-22", "WebKit-Rev-122930"]  
Code Review: [https://chromiumcodereview.appspot.com/10808005](https://chromiumcodereview.appspot.com/10808005)  
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
  

---   

## **regress-2234.js (v8 issue)**  
   
**[Issue 2234:
 Math.sin(0) is sometimes incorrect on ARM](https://crbug.com/v8/2234)**  
**[Commit: Fix transcendental cache on ARM in optimized code.](https://chromium.googlesource.com/v8/v8/+/022ba05)**  
  
Date(Commit): Mon Jul 16 09:44:59 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10695205](https://chromiumcodereview.appspot.com/10695205)  
Regress: [mjsunit/regress/regress-2234.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2234.js)  
```javascript
function test(i) {
  // Overwrite random parts of the transcendental cache.
  Math.sin(i / 1779 * Math.PI);
  // Check whether the first cache line has been accidentally overwritten
  // with incorrect key.
  assertEquals(0, Math.sin(0));
};
%PrepareFunctionForOptimization(test);
for (i = 0; i < 10000; ++i) {
  test(i);
  if (i == 0) %OptimizeFunctionOnNextCall(test);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/022ba05^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=022ba05)  
[test/mjsunit/regress/regress-2234.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2234.js?cl=022ba05)  
  

---   

## **regress-crbug-125148.js (chromium issue)**  
   
**[Issue 125148:
 Javascript engine sometimes calls incorrect constructor](https://crbug.com/125148)**  
**[Commit: Perform HasFastProperties check on prototypes when computing call targets in Crankshaft.](https://chromium.googlesource.com/v8/v8/+/432576b)**  
  
Date(Commit): Wed Jul 11 14:27:53 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10735054](https://chromiumcodereview.appspot.com/10735054)  
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
  

---   

## **regress-1591.js (v8 issue)**  
   
**[Issue 1591:
 Reading Error.stack in a __lookupGetter__ causes inf loop](https://crbug.com/v8/1591)**  
**[Commit: Do not use user-defined __lookupGetter__ when generating stack trace.](https://chromium.googlesource.com/v8/v8/+/2a81966)**  
  
Date(Commit): Wed Jul 11 11:35:19 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10736030](https://chromiumcodereview.appspot.com/10736030)  
Regress: [mjsunit/regress/regress-1591.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1591.js)  
```javascript
var stack;
var used_custom_lookup = false;

({
  __lookupGetter__ : function() {
    used_custom_lookup = true;
  },

  test : function() {
    try {
      f();
    } catch (err) {
      stack = err.stack;
    }
  }
}).test();

var expected_message = "ReferenceError: f is not defined";
assertTrue(stack.indexOf(expected_message) >= 0);
assertFalse(used_custom_lookup);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2a81966^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=2a81966)  
[test/mjsunit/regress/regress-1591.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1591.js?cl=2a81966)  
  

---   

## **regress-2225.js (v8 issue)**  
   
**[Issue 2225:
 SharedFunctionInfo::CanGenerateInlineConstructor can't cope with proxies](https://crbug.com/v8/2225)**  
**[Commit: Fix inline constructors for Harmony Proxy prototypes.](https://chromium.googlesource.com/v8/v8/+/09bfdab)**  
  
Date(Commit): Tue Jul 10 11:28:33 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10736009](https://chromiumcodereview.appspot.com/10736009)  
Regress: [mjsunit/es6/regress/regress-2225.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2225.js)  
```javascript
var proxy_has_x = false;
var proxy = new Proxy({}, {
  get(t, key, receiver) {
    assertSame('x', key);
    if (proxy_has_x) { return 19 }
    return 8;
  }
});

assertSame(undefined, Object.prototype.__lookupGetter__.call(proxy, 'foo'));
assertSame(undefined, Object.prototype.__lookupSetter__.call(proxy, 'bar'));
assertSame(undefined, Object.prototype.__lookupGetter__.call(proxy, '123'));
assertSame(undefined, Object.prototype.__lookupSetter__.call(proxy, '456'));

var object = Object.create(proxy);
assertSame(undefined, Object.prototype.__lookupGetter__.call(object, 'foo'));
assertSame(undefined, Object.prototype.__lookupSetter__.call(object, 'bar'));
assertSame(undefined, Object.prototype.__lookupGetter__.call(object, '123'));
assertSame(undefined, Object.prototype.__lookupSetter__.call(object, '456'));

function F() { this.x = 42 }
F.prototype = proxy;
var instance = new F();

proxy_has_x = false;
assertSame(42, instance.x);
delete instance.x;
assertSame(8, instance.x);

proxy_has_x = true;
assertSame(19, instance.x);

function G() { this.x = 42; }
G.prototype.__proto__ = proxy;
instance = new G();

proxy_has_x = false;
assertSame(42, instance.x);
delete instance.x;
assertSame(8, instance.x);

proxy_has_x = true;
assertSame(19, instance.x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/09bfdab^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=09bfdab)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=09bfdab)  
[test/mjsunit/regress/regress-2225.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2225.js?cl=09bfdab)  
  

---   

## **regress-2226.js (v8 issue)**  
   
**[Issue 2226:
 Multiple assignments set wrong values](https://crbug.com/v8/2226)**  
**[Commit: After transitioning to constant function, return the constant function as result of the assignment.](https://chromium.googlesource.com/v8/v8/+/1007696)**  
  
Date(Commit): Tue Jul 10 09:31:30 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10700137](https://chromiumcodereview.appspot.com/10700137)  
Regress: [mjsunit/regress/regress-2226.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2226.js)  
```javascript
var foo = function() { 0; /* foo function */ };
var bar = function() { 1; /* bar function */ };
var baz = function() { 2; /* baz function */ };

var test = foo.test = bar.test = baz;

assertEquals(baz, test);
assertEquals(baz, foo.test);
assertEquals(baz, bar.test);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1007696^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=1007696)  
[test/mjsunit/regress/regress-2226.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2226.js?cl=1007696)  
  

---   

## **regress-136048.js (chromium issue)**  
   
**[Issue 136048:
 regex with unicode escaped flag and weird error glitch](https://crbug.com/136048)**  
**[Commit: Correctly advance the scanner when scanning unicode regexp flag.](https://chromium.googlesource.com/v8/v8/+/3e3160b)**  
  
Date(Commit): Fri Jul 06 14:04:15 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10703106](https://chromiumcodereview.appspot.com/10703106)  
Regress: [mjsunit/regress/regress-136048.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-136048.js)  
```javascript
try {
  eval("/foo/\\u0069")
} catch (e) {
  assertEquals(
      "SyntaxError: Invalid regular expression flags",
      e.toString());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e3160b^!)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=3e3160b)  
[test/mjsunit/regress/regress-136048.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-136048.js?cl=3e3160b)  
  

---   

## **regress-2219.js (v8 issue)**  
   
**[Issue 2219:
 Harmony Proxy traps are not GC-safe](https://crbug.com/v8/2219)**  
**[Commit: Fix unhandlified code calling Harmony Proxy traps.](https://chromium.googlesource.com/v8/v8/+/026f179)**  
  
Date(Commit): Fri Jul 06 11:34:22 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10703103](https://chromiumcodereview.appspot.com/10703103)  
Regress: [mjsunit/es6/regress/regress-2219.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2219.js)  
```javascript
var p = new Proxy({}, {getOwnPropertyDescriptor: function() { gc() }});
var o = Object.create(p);
assertSame(23, o.x = 23);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/026f179^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=026f179)  
[test/mjsunit/regress/regress-2219.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2219.js?cl=026f179)  
  

---   

## **regress-crbug-135008.js (chromium issue)**  
   
**[Issue 135008:
 Chrome: Crash Report -         Stack Signature: v8::internal::AstVisitor::VisitDeclarations...](https://crbug.com/135008)**  
**[Commit: Fix lazy parsing heuristics to respect outer scope.](https://chromium.googlesource.com/v8/v8/+/a691c69)**  
  
Date(Commit): Thu Jun 28 14:56:28 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10698032](https://chromiumcodereview.appspot.com/10698032)  
Regress: [mjsunit/regress/regress-crbug-135008.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-135008.js)  
```javascript
var filler = "//" + new Array(1024).join('x');

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
  

---   

## **regress-2119.js (v8 issue)**  
   
**[Issue 2119:
 error in strict mode with --nouse_ic](https://crbug.com/v8/2119)**  
**[Commit: Correctly throw reference error in strict mode with ICs disabled.](https://chromium.googlesource.com/v8/v8/+/99a58e3)**  
  
Date(Commit): Mon Jun 25 13:28:11 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10659011](https://chromiumcodereview.appspot.com/10659011)  
Regress: [mjsunit/regress/regress-2119.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2119.js)  
```javascript
function strict_function() {
  "use strict"
  undeclared = 1;
}

assertThrows(strict_function);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/99a58e3^!)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=99a58e3)  
[test/mjsunit/regress/regress-2119.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2119.js?cl=99a58e3)  
  

---   

## **regress-2193.js (v8 issue)**  
   
**[Issue 2193:
 V8 roll to 3.12.1 introduced intermittent crashes in layout tests](https://crbug.com/v8/2193)**  
**[Commit: Fix sharing of literal boilerplates for optimized code.](https://chromium.googlesource.com/v8/v8/+/84b866b)**  
  
Date(Commit): Fri Jun 22 13:55:15 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10649008](https://chromiumcodereview.appspot.com/10649008)  
Regress: [mjsunit/regress/regress-2193.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2193.js)  
```javascript
function bozo() {};
function MakeClosure() {
  return function f(use_literals) {
    if (use_literals) {
      return [1,2,3,3,4,5,6,7,8,9,bozo];
    } else {
      return 0;
    }
  }
}

var closure1 = MakeClosure();
var closure2 = MakeClosure();
var expected = [1,2,3,3,4,5,6,7,8,9,bozo];

assertEquals(0, closure1(false));
assertEquals(expected, closure1(true));
%OptimizeFunctionOnNextCall(closure1);
assertEquals(expected, closure1(true));

assertEquals(0, closure2(false));
%OptimizeFunctionOnNextCall(closure2);
assertEquals(expected, closure2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/84b866b^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=84b866b)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=84b866b)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=84b866b)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=84b866b)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=84b866b)  
...  
  

---   

## **regress-115100.js (chromium issue)**  
   
**[Issue 115100:
 Chrome: Crash Report -         Stack Signature: v8::internal::JSReceiver::Lookup(v8::intern...](https://crbug.com/115100)**  
**[Commit: Fix crash bug in Hydrogen occurring with empty prototype chain.](https://chromium.googlesource.com/v8/v8/+/3e6b01d)**  
  
Date(Commit): Tue Jun 19 13:44:07 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "M-19"]  
Code Review: [https://chromiumcodereview.appspot.com/10576013](https://chromiumcodereview.appspot.com/10576013)  
Regress: [mjsunit/regress/regress-115100.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-115100.js)  
```javascript
function foo(obj) {
  obj.prop = 0;
};
%PrepareFunctionForOptimization(foo);
function mk() {
  return Object.create(null);
}

foo(mk());
foo(mk());
%OptimizeFunctionOnNextCall(foo);
foo(mk());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e6b01d^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=3e6b01d)  
[test/mjsunit/regress/regress-115100.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-115100.js?cl=3e6b01d)  
  

---   

## **regress-133211.js (chromium issue)**  
   
**[Issue 133211:
 [LangFuzz] CHECK(holder != __null) failed or Crash in v8::internal::Object::Lookup](https://crbug.com/133211)**  
**[Commit: Make sure we don't leak map transitions from AccessorPairs to the Javascript world.](https://chromium.googlesource.com/v8/v8/+/8b7b7f4)**  
  
Date(Commit): Tue Jun 19 10:58:15 2012  
Components/Type: None/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "Security_severity-None", "Merge-Merged", "M-21", "Security_Impact-None"]  
Code Review: [https://chromiumcodereview.appspot.com/10559062](https://chromiumcodereview.appspot.com/10559062)  
Regress: [mjsunit/regress/regress-133211.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-133211.js), [mjsunit/regress/regress-133211b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-133211b.js)  
```javascript
var o = {};
var x = {};
Object.defineProperty(o, "foo", { get: undefined });
Object.defineProperty(x, "foo", { get: undefined, set: undefined });
var pd = Object.getOwnPropertyDescriptor(o, "foo");
assertEquals(undefined, pd.set);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8b7b7f4^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=8b7b7f4)  
[test/mjsunit/regress/regress-133211.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-133211.js?cl=8b7b7f4)  
  

---   

## **regress-2186.js (v8 issue)**  
   
**[Issue 2186:
 Harmony Map distinguishes heapnumber and smi representations of the same number](https://crbug.com/v8/2186)**  
**[Commit: Fix handling of numbers in SameValue method.](https://chromium.googlesource.com/v8/v8/+/928a1bf)**  
  
Date(Commit): Mon Jun 18 14:21:29 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10532198](https://chromiumcodereview.appspot.com/10532198)  
Regress: [mjsunit/es6/regress/regress-2186.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2186.js)  
```javascript
function heapify(i) {
  return 2.0 * (i / 2);
}
heapify(1);

var ONE = 1;
var ANOTHER_ONE = heapify(ONE);
assertSame(ONE, ANOTHER_ONE);
assertEquals("number", typeof ONE);
assertEquals("number", typeof ANOTHER_ONE);

var set = new Set;
set.add(ONE);
assertTrue(set.has(ONE));
assertTrue(set.has(ANOTHER_ONE));

var map = new Map;
map.set(ONE, 23);
assertSame(23, map.get(ONE));
assertSame(23, map.get(ANOTHER_ONE));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/928a1bf^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=928a1bf)  
[test/mjsunit/regress/regress-2186.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2186.js?cl=928a1bf)  
  

---   

## **regress-2172.js (v8 issue)**  
   
**[Issue 2172:
 toString and split methods causes "illegal access" error](https://crbug.com/v8/2172)**  
**[Commit: Add missing string length check in regexp engine.](https://chromium.googlesource.com/v8/v8/+/675d9b8)**  
  
Date(Commit): Thu Jun 14 13:59:48 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10536170](https://chromiumcodereview.appspot.com/10536170)  
Regress: [mjsunit/regress/regress-2172.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2172.js)  
```javascript
for (var i = 0; i < 10000; i++){
  (i + "\0").split(/(.)\1/i);
}

for (var i = 0; i < 10000; i++){
  (i + "\u1234\0").split(/(.)\1/i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/675d9b8^!)  
[src/ia32/regexp-macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/regexp-macro-assembler-ia32.cc?cl=675d9b8)  
[src/x64/regexp-macro-assembler-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/regexp-macro-assembler-x64.cc?cl=675d9b8)  
[test/mjsunit/regress/regress-2172.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2172.js?cl=675d9b8)  
  

---   

## **regress-2156.js (v8 issue)**  
   
**[Issue 2156:
 Major speed regression:  3.6.6.25 -> 3.11.1](https://crbug.com/v8/2156)**  
**[Commit: Fix performance regression caused by r11202.](https://chromium.googlesource.com/v8/v8/+/74ab92e)**  
  
Date(Commit): Wed Jun 13 11:58:18 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10539131](https://chromiumcodereview.appspot.com/10539131)  
Regress: [mjsunit/es6/regress/regress-2156.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2156.js)  
```javascript
var key1 = {};
var key2 = {};
var map = new WeakMap;

assertTrue(%HaveSameMap(key1, key2));
map.set(key1, 1);
map.set(key2, 2);
assertTrue(%HaveSameMap(key1, key2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/74ab92e^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=74ab92e)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=74ab92e)  
[test/mjsunit/regress/regress-2156.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2156.js?cl=74ab92e)  
  

---   

## **regress-deep-proto.js (other issue)**  
   
**[Commit: Fix r11780 to avoid bugs where near branches are used to labels that are out of range.](https://chromium.googlesource.com/v8/v8/+/5eb4bae)**  
  
Date(Commit): Wed Jun 13 09:54:34 2012  
Code Review: [http://codereview.chromium.org/10542137](http://codereview.chromium.org/10542137)  
Regress: [mjsunit/regress/regress-deep-proto.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deep-proto.js)  
```javascript
function poly(x) {
  return x.foo;
};
%PrepareFunctionForOptimization(poly);
var one = {foo: 0};
var two = {foo: 0, bar: 1};
var three = {bar: 0};
three.__proto__ = {};
three.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__.__proto__ = {};
three.__proto__.__proto__.__proto__.__proto__.__proto__ = {};

poly(one);
poly(two);
poly(three);

%OptimizeFunctionOnNextCall(poly);

poly(one);
poly(two);
poly(three);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5eb4bae^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=5eb4bae)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=5eb4bae)  
[test/mjsunit/regress/regress-deep-proto.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-deep-proto.js?cl=5eb4bae)  
  
  
---   

## **regress-131923.js (chromium issue)**  
   
**[Issue 131923:
 Regular Expressions issue](https://crbug.com/131923)**  
**[Commit: Fix optimization of Unicode regexp with ASCII subject to respect repeat counts.](https://chromium.googlesource.com/v8/v8/+/afc9b8e)**  
  
Date(Commit): Mon Jun 11 11:18:04 2012  
Components/Type: Blink/Bug-Regression  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-20", "ReleaseBlock-Stable"]  
Code Review: [http://codereview.chromium.org/10544093](http://codereview.chromium.org/10544093)  
Regress: [mjsunit/regress/regress-131923.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-131923.js)  
```javascript
assertFalse(/\u9999{4}/.test(""));
assertTrue(/\u9999{0,4}/.test(""));
assertFalse(/\u9999{4,}/.test(""));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afc9b8e^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=afc9b8e)  
[test/mjsunit/regress/regress-131923.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-131923.js?cl=afc9b8e)  
  

---   

## **regress-2170.js (v8 issue)**  
   
**[Issue 2170:
 Strange effects with floating point arrays that have other properties.](https://crbug.com/v8/2170)**  
**[Commit: Fix EnsureCanContainElements to properly handle double values.](https://chromium.googlesource.com/v8/v8/+/a1d9aca)**  
  
Date(Commit): Mon Jun 11 08:41:48 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10542084](https://chromiumcodereview.appspot.com/10542084)  
Regress: [mjsunit/regress/regress-2170.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2170.js)  
```javascript
function array_fun() {
  for (var i = 0; i < 2; i++) {
    var a = [1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 1.7, 1.8];
    var x = new Array();
    x.fixed$length = true;
    for (var j = 0; j < a.length; j++) {
      x.push(a[j]);
    }
    for (var j = 0; j < x.length; j++) {
      if (typeof x[j] != 'number') {
        throw "foo";
      }
      x[j] = x[j];
    }
  }
};
%PrepareFunctionForOptimization(array_fun);
try {
  for (var i = 0; i < 10; ++i) {
    array_fun();
  }
  %OptimizeFunctionOnNextCall(array_fun);
  for (var i = 0; i < 10; ++i) {
    array_fun();
  }
} catch (e) {
  assertUnreachable();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a1d9aca^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=a1d9aca)  
[test/mjsunit/regress/regress-2170.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2170.js?cl=a1d9aca)  
  

---   

## **regress-2163.js (v8 issue)**  
   
**[Issue 2163:
 ClearNonLiveTransitions throws away non-stale accessor-pairs](https://crbug.com/v8/2163)**  
**[Commit: ClearNonLiveTransitions has to hold on to non-map values.](https://chromium.googlesource.com/v8/v8/+/a85f4e4)**  
  
Date(Commit): Tue Jun 05 11:36:57 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10535004](https://chromiumcodereview.appspot.com/10535004)  
Regress: [mjsunit/regress/regress-2163.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2163.js)  
```javascript
var dp = Object.defineProperty;

function getter() { return 111; }
function setter(x) { print(222); }
function anotherGetter() { return 333; }
function anotherSetter(x) { print(444); }
var obj1, obj2;

obj1 = {};
dp(obj1, "alpha", { get: getter, set: setter });
obj2 = {}
dp(obj2, "alpha", { get: getter });
obj1 = {};
assertEquals(111, obj2.alpha);
gc();
assertEquals(111, obj2.alpha);

obj1 = {};
dp(obj1, "alpha", { get: getter, set: setter });
obj2 = {}
dp(obj2, "alpha", { get: getter });
obj1 = {};
gc();
obj3 = {}
dp(obj3, "alpha", { get: getter });


obj1 = {};
dp(obj1, "alpha", { get: getter, set: setter });
obj1.beta = 10;
obj2 = {}
dp(obj2, "alpha", { get: getter, set: setter });
obj1 = {};
assertEquals(111, obj2.alpha);
gc();
obj2.alpha = 100
assertEquals(111, obj2.alpha);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a85f4e4^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=a85f4e4)  
[test/mjsunit/accessor-map-sharing.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/accessor-map-sharing.js?cl=a85f4e4)  
[test/mjsunit/regress/regress-2163.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2163.js?cl=a85f4e4)  
  

---   

## **regress-iteration-order.js (other issue)**  
   
**[Commit: Fix bug in __proto__ assignment transition cache where we forget the next enumeration index resulting in wrong iteration order.](https://chromium.googlesource.com/v8/v8/+/0a856e0)**  
  
Date(Commit): Mon Jun 04 12:07:46 2012  
Code Review: [https://chromiumcodereview.appspot.com/10515006](https://chromiumcodereview.appspot.com/10515006)  
Regress: [mjsunit/regress/regress-iteration-order.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-iteration-order.js)  
```javascript
var x = {a: 1, b: 2, c: 3};

x.__proto__ = {};

delete x.b;

x.d = 4;

s = "";

for (key in x) {
    s += x[key];
}

assertEquals("134", s);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a856e0^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0a856e0)  
[test/mjsunit/regress/regress-iteration-order.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-iteration-order.js?cl=0a856e0)  
  
  
---   

## **regress-2153.js (v8 issue)**  
   
**[Issue 2153:
 CALLBACKS transition conflicts with normal property](https://crbug.com/v8/2153)**  
**[Commit: Fixed JSObject::SetPropertyForResult (issue 2153)](https://chromium.googlesource.com/v8/v8/+/39f88f1)**  
  
Date(Commit): Tue May 29 12:42:22 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/10453054](https://chromiumcodereview.appspot.com/10453054)  
Regress: [mjsunit/regress/regress-2153.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2153.js)  
```javascript
var o = {};
o.__defineGetter__('foo', function () { return null; });
var o = {};
o.foo = 42;
assertEquals(42, o.foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/39f88f1^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=39f88f1)  
[test/mjsunit/regress/regress-2153.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2153.js?cl=39f88f1)  
  

---   

## **regress-2071.js (v8 issue)**  
   
**[Issue 2071:
 with statement is broken since Chrome 19](https://crbug.com/v8/2071)**  
**[Commit: Disable optimization for functions that have scopes that cannot be reconstructed from the context chain.](https://chromium.googlesource.com/v8/v8/+/15b796b)**  
  
Date(Commit): Fri May 18 13:06:16 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10388164](https://chromiumcodereview.appspot.com/10388164)  
Regress: [mjsunit/regress/regress-2071.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2071.js)  
```javascript
a = {};

a.b = 42;

with(a) {
  a.f = (function f1() {
    function f2() {
      return b;
    };
    return f2;
  })();
}

for(var i = 0; i < 10000; i++) {
  assertEquals(42, a.f());
}

with(a) {
  a.g = (function f1() {
    function f2() {
      function f3() {
        return b;
      }
      return f3;
    };
    return f2();
  })();
}

for(var i = 0; i < 10000; i++) {
  assertEquals(42, a.g());
}

function outer() {
  with(a) {
    a.h = (function f1() {
      function f2() {
        function f3() {
          return b;
        }
        return f3;
      };
      return f2();
    })();
  }
};

outer();

for(var i = 0; i < 10000; i++) {
  assertEquals(42, a.h());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/15b796b^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=15b796b)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=15b796b)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=15b796b)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=15b796b)  
[test/mjsunit/regress/regress-2071.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2071.js?cl=15b796b)  
  

---   

## **regress-128146.js (chromium issue)**  
   
**[Issue 128146:
 UNKNOWN in v8::internal::DescriptorArray::Set](https://crbug.com/128146)**  
**[Commit: Revert r11496.](https://chromium.googlesource.com/v8/v8/+/ec1fc61)**  
  
Date(Commit): Wed May 16 11:07:54 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-21", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10238005](https://chromiumcodereview.appspot.com/10238005)  
Regress: [mjsunit/regress/regress-128146.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-128146.js)  
```javascript
Object.defineProperty({},"foo",{set:function(){},configurable:false});
Object.defineProperty({},"foo",{get:function(){},configurable:false});

Object.defineProperty({},"foo",{});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ec1fc61^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ec1fc61)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=ec1fc61)  
[test/mjsunit/accessor-map-sharing.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/accessor-map-sharing.js?cl=ec1fc61)  
[test/mjsunit/regress/regress-128146.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-128146.js?cl=ec1fc61)  
  

---   

## **regress-128018.js (chromium issue)**  
   
**[Issue 128018:
 [LangFuzz] Crash in v8::internal::ShortCircuitConsString with invalid read](https://crbug.com/128018)**  
**[Commit: Always transition empty FAST_DOUBLE_ARRAYs on push](https://chromium.googlesource.com/v8/v8/+/7966fb3)**  
  
Date(Commit): Tue May 15 16:17:53 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Reward-1000", "M-19", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-High", "allpublic", "CVE-2011-3115", "CVE_description-submitted"]  
Code Review: [https://chromiumcodereview.appspot.com/10387130](https://chromiumcodereview.appspot.com/10387130)  
Regress: [mjsunit/regress/regress-128018.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-128018.js)  
```javascript
function KeyedStoreIC(a) { a[(1)] = Math.E; }
var literal = [1.2];
literal.length = 0;
literal.push('0' && 0 );
KeyedStoreIC(literal);
gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7966fb3^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=7966fb3)  
[test/mjsunit/regress/regress-128018.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-128018.js?cl=7966fb3)  
  

---   

## **regress-117409.js (chromium issue)**  
   
**[Issue 117409:
 Chrome: Crash Report -         Stack Signature: v8::internal::MarkCompactCollector::RecordS...](https://crbug.com/117409)**  
**[Commit: Properly set ElementsKind of empty FAST_DOUBLE_ELEMENTS arrays when transitioning.](https://chromium.googlesource.com/v8/v8/+/159ee25)**  
  
Date(Commit): Wed May 09 15:18:50 2012  
Components/Type: Blink/Bug-Security  
Labels: ["M-19", "Merge-Merged", "Security_Impact-Stable", "CVE-2011-3103", "Security_Severity-High", "allpublic", "CVE_description-submitted"]  
Code Review: [https://chromiumcodereview.appspot.com/10386045](https://chromiumcodereview.appspot.com/10386045)  
Regress: [mjsunit/regress/regress-117409.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-117409.js)  
```javascript
function KeyedStoreIC(a) { a[0] = Math.E; }

var literal = [1.2];

KeyedStoreIC(literal);
KeyedStoreIC(literal);

literal.length = 0;

literal.push(Math.E, Math.E);

KeyedStoreIC(literal);

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/159ee25^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=159ee25)  
[src/objects-printer.cc](https://cs.chromium.org/chromium/src/v8/src/objects-printer.cc?cl=159ee25)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=159ee25)  
[test/mjsunit/regress/regress-117409.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-117409.js?cl=159ee25)  
  

---   

## **regress-126412.js (chromium issue)**  
   
**[Issue 126412:
 V8 regex stack exhaustion crash](https://crbug.com/126412)**  
**[Commit: Regexp: Fix overflow in min-match-length calculation.  Crbug=126412.](https://chromium.googlesource.com/v8/v8/+/681f295)**  
  
Date(Commit): Tue May 08 12:18:08 2012  
Components/Type: Blink/Bug  
Labels: ["Security_severity-None", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10384053](https://chromiumcodereview.appspot.com/10384053)  
Regress: [mjsunit/regress/regress-126412.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-126412.js)  
```javascript
"".match(/(A{9999999999}B|C*)*D/);
"C".match(/(A{9999999999}B|C*)*D/);
"".match(/(A{9999999999}B|C*)*/ );
"C".match(/(A{9999999999}B|C*)*/ );
"".match(/(9u|(2\`shj{2147483649,}\r|3|f|y|3*)+8\B)\W93+/);
"9u8 ".match(/(9u|(2\`shj{2147483649,}\r|3|f|y|3*)+8\B)\W93+/);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/681f295^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=681f295)  
[test/mjsunit/regress/regress-126412.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-126412.js?cl=681f295)  
  

---   

## **regress-crbug-126414.js (chromium issue)**  
   
**[Issue 126414:
 [LangFuzz] Crash on heap with invalid read from random address (32 bit)](https://crbug.com/126414)**  
**[Commit: Fix unsigned-Smi check in MappedArgumentsLookup](https://chromium.googlesource.com/v8/v8/+/63263a9)**  
  
Date(Commit): Mon May 07 10:05:39 2012  
Components/Type: Internals/Bug-Security  
Labels: ["CVE-2011-3111", "reward-500", "M-19", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "CVE_description-submitted"]  
Code Review: [https://chromiumcodereview.appspot.com/10375033](https://chromiumcodereview.appspot.com/10375033)  
Regress: [mjsunit/regress/regress-crbug-126414.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-126414.js)  
```javascript
function foo(bar)  {
  return arguments[bar];
}
foo(0);           // Handled in runtime.
foo(-536870912);  // Triggers bug.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/63263a9^!)  
[src/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/ic-arm.cc?cl=63263a9)  
[src/ia32/ic-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/ic-ia32.cc?cl=63263a9)  
[src/mips/ic-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/ic-mips.cc?cl=63263a9)  
[test/mjsunit/regress/regress-crbug-126414.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-126414.js?cl=63263a9)  
  

---   

## **regress-125515.js (chromium issue)**  
   
**[Issue 125515:
 [LangFuzz] Crash on heap with invalid write to random address](https://crbug.com/125515)**  
**[Commit: Ensure reload of elements pointer in StoreFastDoubleElement stub.](https://chromium.googlesource.com/v8/v8/+/908e77a)**  
  
Date(Commit): Wed May 02 09:58:42 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-19", "Security_Severity-High", "Security_Impact-Beta", "ReleaseBlock-Stable", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10260014](https://chromiumcodereview.appspot.com/10260014)  
Regress: [mjsunit/regress/regress-125515.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-125515.js)  
```javascript
function test(a) {
  a[0] = 1.5;
  assertEquals(0, a.length = 0);
}
a = new Array();
test(a);
test(a);
gc();
gc();
test(a);
test(a);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/908e77a^!)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=908e77a)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=908e77a)  
[test/mjsunit/regress/regress-125515.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-125515.js?cl=908e77a)  
  

---   

## **regress-2110.js (v8 issue)**  
   
**[Issue 2110:
 Inconsistent results assigning large numbers to Uint8Array](https://crbug.com/v8/2110)**  
**[Commit: Fixed corner cases in truncation behavior when storing to TypedArrays.](https://chromium.googlesource.com/v8/v8/+/f6dacfe)**  
  
Date(Commit): Mon Apr 30 15:17:59 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10260011](https://chromiumcodereview.appspot.com/10260011)  
Regress: [mjsunit/regress/regress-2110.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2110.js)  
```javascript
var uint8 = new Uint8Array(1);

function test() {
  uint8[0] = 0x800000aa;
  assertEquals(0xaa, uint8[0]);
};
%PrepareFunctionForOptimization(test);
test();
test();
test();
%OptimizeFunctionOnNextCall(test);
test();

var uint32 = new Uint32Array(1);

function test2() {
  uint32[0] = 0x80123456789abcde;
  assertEquals(0x789ac000, uint32[0]);
};
%PrepareFunctionForOptimization(test2);
test2();
test2();
%OptimizeFunctionOnNextCall(test2);
test2();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6dacfe^!)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=f6dacfe)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=f6dacfe)  
[test/mjsunit/regress/regress-2110.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2110.js?cl=f6dacfe)  
  

---   

## **regress-fast-literal-transition.js (other issue)**  
   
**[Commit: Fix LFastLiteral to check boilerplate elements kind.](https://chromium.googlesource.com/v8/v8/+/b54ca31)**  
  
Date(Commit): Mon Apr 30 14:59:13 2012  
Code Review: [https://chromiumcodereview.appspot.com/10254006](https://chromiumcodereview.appspot.com/10254006)  
Regress: [mjsunit/regress/regress-fast-literal-transition.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-fast-literal-transition.js)  
```javascript
function f(x) {
  switch (x) {
    case 1:
      return 1.4;
    case 2:
      return 1.5;
    case 3:
      return {};
    default:
      gc();
  }
}

function g(x) {
  return [1.1, 1.2, 1.3, f(x)];
}

;
%PrepareFunctionForOptimization(g);
assertEquals([1.1, 1.2, 1.3, 1.4], g(1));
assertEquals([1.1, 1.2, 1.3, 1.5], g(2));
%OptimizeFunctionOnNextCall(g);

assertEquals([1.1, 1.2, 1.3, {}], g(3));

assertEquals([1.1, 1.2, 1.3, undefined], g(4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b54ca31^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=b54ca31)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=b54ca31)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=b54ca31)  
[test/mjsunit/regress/regress-fast-literal-transition.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-fast-literal-transition.js?cl=b54ca31)  
  
  
---   

## **regress-124594.js (chromium issue)**  
   
**[Issue 124594:
 UNKNOWN in v8::internal::MarkCompactCollector::PrepareThreadForCodeFlushing](https://crbug.com/124594)**  
**[Commit: Fix deopted construct stub frame to contain code object.](https://chromium.googlesource.com/v8/v8/+/21fc0fe)**  
  
Date(Commit): Wed Apr 25 13:22:04 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "reward-500", "M-20", "Security_Severity-Medium", "Security_Impact-None", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/10155024](https://chromiumcodereview.appspot.com/10155024)  
Regress: [mjsunit/regress/regress-124594.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-124594.js)  
```javascript
function f(deopt) {
  var x = 1;
  if (deopt) {
    x = x + "foo";
    gc();
  }
  this.x = x;
}

function g(deopt) {
  return new f(deopt);
};
%PrepareFunctionForOptimization(g);
assertEquals({x: 1}, g(false));
assertEquals({x: 1}, g(false));
%OptimizeFunctionOnNextCall(g);
assertEquals({x: '1foo'}, g(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21fc0fe^!)  
[src/arm/deoptimizer-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/deoptimizer-arm.cc?cl=21fc0fe)  
[src/ia32/deoptimizer-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/deoptimizer-ia32.cc?cl=21fc0fe)  
[src/mips/deoptimizer-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/deoptimizer-mips.cc?cl=21fc0fe)  
[src/x64/deoptimizer-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/deoptimizer-x64.cc?cl=21fc0fe)  
[test/mjsunit/regress/regress-124594.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-124594.js?cl=21fc0fe)  
  

---   

## **regress-123919.js (chromium issue)**  
   
**[Issue 123919:
 [Regression] V8 Memory Corruption in renderers during GC](https://crbug.com/123919)**  
**[Commit: Fix missing GVN flag for new-space promotion.](https://chromium.googlesource.com/v8/v8/+/5773910)**  
  
Date(Commit): Thu Apr 19 07:49:11 2012  
Components/Type: Blink/Bug-Regression  
Labels: ["Restrict-AddIssueComment-EditIssue", "Stability-Crash", "M-20"]  
Code Review: [https://chromiumcodereview.appspot.com/10119016](https://chromiumcodereview.appspot.com/10119016)  
Regress: [mjsunit/regress/regress-123919.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-123919.js)  
```javascript
function g(max, val) {
  this.x = 0;
  for (var i = 0; i < max; i++) {
    this.x = i / 100;
  }
  this.val = val;
}

function f(max) {
  var val = 0.5;
  var obj = new g(max, val);
  assertSame(val, obj.val);
};
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(200000);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5773910^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=5773910)  
[test/mjsunit/regress/regress-123919.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-123919.js?cl=5773910)  
  

---   

## **regress-123512.js (chromium issue)**  
   
**[Issue 123512:
 Reproducible crash at v8::internal::IsFastLiteral (null pointer deref)](https://crbug.com/123512)**  
**[Commit: Fix fast array literals to ignore prototype chain.](https://chromium.googlesource.com/v8/v8/+/47d07b8)**  
  
Date(Commit): Tue Apr 17 11:12:37 2012  
Components/Type: Blink/Bug  
Labels: ["Stability-Crash", "Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/10105025](https://chromiumcodereview.appspot.com/10105025)  
Regress: [mjsunit/regress/regress-123512.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-123512.js)  
```javascript
function f(x) {
  return [x][0];
}

Object.prototype[0] = 23;
assertSame(1, f(1));
assertSame(2, f(2));
%OptimizeFunctionOnNextCall(f);
assertSame(3, f(3));
%DeoptimizeFunction(f);

Object.prototype.__defineGetter__(0, function() { throw Error(); });
assertSame(4, f(4));
assertSame(5, f(5));
%OptimizeFunctionOnNextCall(f);
assertSame(6, f(6));
%DeoptimizeFunction(f);


function g(x, y) {
  var o = { foo:x, 0:y };
  return o.foo + o[0];
}

Object.prototype[0] = 23;
Object.prototype.foo = 42;
assertSame(3, g(1, 2));
assertSame(5, g(2, 3));
%OptimizeFunctionOnNextCall(g);
assertSame(7, g(3, 4));
%DeoptimizeFunction(g);

Object.prototype.__defineGetter__(0, function() { throw Error(); });
Object.prototype.__defineGetter__('foo', function() { throw Error(); });
assertSame(3, g(1, 2));
assertSame(5, g(2, 3));
%OptimizeFunctionOnNextCall(g);
assertSame(7, g(3, 4));
%DeoptimizeFunction(g);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/47d07b8^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=47d07b8)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=47d07b8)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=47d07b8)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=47d07b8)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=47d07b8)  
...  
  

---   

## **regress-crbug-122271.js (chromium issue)**  
   
**[Issue 122271:
 Page starts rendering incorrectly after V8 roll](https://crbug.com/122271)**  
**[Commit: Fix regular and ElementsKind transitions interfering with each other](https://chromium.googlesource.com/v8/v8/+/14e1817)**  
  
Date(Commit): Thu Apr 12 12:30:32 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-19", "ReleaseBlock-Stable"]  
Code Review: [https://chromiumcodereview.appspot.com/10038010](https://chromiumcodereview.appspot.com/10038010)  
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
  

---   

## **regress-2058.js (v8 issue)**  
   
**[Issue 2058:
 RegExp.$1 not updated after a second global regex match within a replace callback](https://crbug.com/v8/2058)**  
**[Commit: Ensure that a call to String.prototype.match with a](https://chromium.googlesource.com/v8/v8/+/f90e665)**  
  
Date(Commit): Tue Apr 10 10:42:25 2012  
Type: ----  
Code Review: [http://codereview.chromium.org/10029009](http://codereview.chromium.org/10029009)  
Regress: [mjsunit/regress/regress-2058.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2058.js)  
```javascript
"Now is the".replace(/Now (\w+) the/g, function() {
  "foo bar".match(/( )/);
  assertEquals(RegExp.$1, " ");
})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f90e665^!)  
[src/string.js](https://cs.chromium.org/chromium/src/v8/src/string.js?cl=f90e665)  
[test/mjsunit/regress/regress-2058.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2058.js?cl=f90e665)  
  

---   

## **regress-2056.js (v8 issue)**  
   
**[Issue 2056:
 Math.max/Math.min have been bebugged in Chrome 18](https://crbug.com/v8/2056)**  
**[Commit: Add regression test case for issue 2025.](https://chromium.googlesource.com/v8/v8/+/db34072)**  
  
Date(Commit): Thu Apr 05 08:08:05 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/10006004](https://chromiumcodereview.appspot.com/10006004)  
Regress: [mjsunit/regress/regress-2056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2056.js)  
```javascript
var cases = [
  [0.0, 0.0, 0.0, 0, 0], [undefined, 0.0, NaN, NaN], [0.0, undefined, NaN, NaN],
  [NaN, 0.0, NaN, NaN], [0.0, NaN, NaN, NaN], [-NaN, 0.0, NaN, NaN],
  [0.0, -NaN, NaN, NaN], [Infinity, 0.0, Infinity, 0.0],
  [0.0, Infinity, Infinity, 0.0], [-Infinity, 0.0, 0.0, -Infinity],
  [0.0, -Infinity, 0.0, -Infinity]
];

function do_min(a, b) {
  return Math.min(a, b);
};
%PrepareFunctionForOptimization(do_min);
function do_max(a, b) {
  return Math.max(a, b);
}

;
%PrepareFunctionForOptimization(do_max);
for (i = 0; i < cases.length; ++i) {
  var c = cases[i];
  assertEquals(c[3], do_min(c[0], c[1]));
  assertEquals(c[2], do_max(c[0], c[1]));
}

for (i = 0; i < cases.length; ++i) {
  var c = cases[i];
  %OptimizeFunctionOnNextCall(do_min);
  %OptimizeFunctionOnNextCall(do_max);
  assertEquals(c[3], do_min(c[0], c[1]));
  assertEquals(c[2], do_max(c[0], c[1]));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/db34072^!)  
[test/mjsunit/regress/regress-2056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2056.js?cl=db34072)  
  

---   

## **regress-2054.js (v8 issue)**  
   
**[Issue 2054:
 Fatal error in src\hydrogen.cc, line 4543](https://crbug.com/v8/2054)**  
**[Commit: Fix rewriter to not treat throw as an expression.](https://chromium.googlesource.com/v8/v8/+/47aa325)**  
  
Date(Commit): Wed Apr 04 13:41:05 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9969146](https://chromiumcodereview.appspot.com/9969146)  
Regress: [mjsunit/regress/regress-2054.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2054.js)  
```javascript
var N = 1e5;  // Number of iterations that trigger optimization.
for (var i = 0; i < N; i++) {
  if (i > N) throw new Error;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/47aa325^!)  
[src/rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/rewriter.cc?cl=47aa325)  
[test/mjsunit/regress/regress-2054.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2054.js?cl=47aa325)  
  

---   

## **regress-2055.js (v8 issue)**  
   
**[Issue 2055:
 Assertion in JSObject::TransitionElementsKind hit](https://crbug.com/v8/2055)**  
**[Commit: Fix array boilerplate object transitioning.](https://chromium.googlesource.com/v8/v8/+/7b59b1d)**  
  
Date(Commit): Tue Apr 03 16:54:28 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9950095](https://chromiumcodereview.appspot.com/9950095)  
Regress: [mjsunit/regress/regress-2055.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2055.js)  
```javascript
function test1(depth) {
  if (--depth < 0) {
    return [];
  } else {
    return [ 0, test1(depth) ];
  }
}
assertEquals([0,[0,[]]], test1(2));

function test2(depth) {
  if (--depth < 0) {
    return [];
  } else {
    var o = [ 0, test2(depth) ];
    return (depth == 0) ? 0.5 : o;
  }
}
assertEquals([0,0.5], test2(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b59b1d^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=7b59b1d)  
[test/mjsunit/regress/regress-2055.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2055.js?cl=7b59b1d)  
  

---   

## **regress-119429.js (chromium issue)**  
   
**[Issue 119429:
 UNKNOWN in v8::Message::GetScriptResourceName](https://crbug.com/119429)**  
**[Commit: Don't crash on stack overflow entering the debugger.](https://chromium.googlesource.com/v8/v8/+/8dc9bc9)**  
  
Date(Commit): Tue Apr 03 13:45:56 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "reward-500", "Stability-Memory-AddressSanitizer", "M-18", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "reward-decline", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/9965101](https://chromiumcodereview.appspot.com/9965101)  
Regress: [mjsunit/regress/regress-119429.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-119429.js)  
```javascript
var d = 0;
function recurse() {
  if (++d == 25135) { // A magic number just below stack overflow  on ia32
    %HandleDebuggerStatement();
  }
  recurse();
}
assertThrows(function() { recurse();} );  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8dc9bc9^!)  
[src/debug.cc](https://cs.chromium.org/chromium/src/v8/src/debug.cc?cl=8dc9bc9)  
[src/execution.cc](https://cs.chromium.org/chromium/src/v8/src/execution.cc?cl=8dc9bc9)  
[test/mjsunit/regress/regress-119429.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-119429.js?cl=8dc9bc9)  
  

---   

## **regress-121407.js (chromium issue)**  
   
**[Issue 121407:
 [LangFuzz] Invalid write in v8::internal::ElementsAccessorBase<...>::CopyElements](https://crbug.com/121407)**  
**[Commit: Properly support shrinking arrays in CopyDictionaryToObjectElements.](https://chromium.googlesource.com/v8/v8/+/d943772)**  
  
Date(Commit): Tue Apr 03 08:13:59 2012  
Components/Type: Blink/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-19", "Merge-Merged", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/9968056](https://chromiumcodereview.appspot.com/9968056)  
Regress: [mjsunit/regress/regress-121407.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-121407.js)  
```javascript
var a = [0,1,2,3];
a[2000000] = 2000000;
a.length=2000;
for (var i = 0; i <= 256; i++) {
    a[i] = new Object();
}

a = [0.5,1.5,2.5,3.5,4.5,5.5];
a[2000000] = 2000000;
a.length=2000;
for (var i = 0; i <= 256; i++) {
    a[i] = new Object();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d943772^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=d943772)  
[test/mjsunit/regress/regress-121407.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-121407.js?cl=d943772)  
  

---   

## **regress-2034.js (v8 issue)**  
   
**[Issue 2034:
 Harmony WeakMap does not store frozen proxy](https://crbug.com/v8/2034)**  
**[Commit: Fix hidden properties to ignore [[Extensible]].](https://chromium.googlesource.com/v8/v8/+/5798bc2)**  
  
Date(Commit): Mon Apr 02 08:26:30 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9844025](https://chromiumcodereview.appspot.com/9844025)  
Regress: [mjsunit/es6/regress/regress-2034.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-2034.js)  
```javascript
var key = {};
var map = new WeakMap;
Object.preventExtensions(key);

assertFalse(map.has(key));
assertSame(undefined, map.get(key));

map.set(key, 1);
assertTrue(map.has(key));
assertSame(1, map.get(key));

map.delete(key, 1);
assertFalse(map.has(key));
assertSame(undefined, map.get(key));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5798bc2^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=5798bc2)  
[test/mjsunit/regress/regress-2034.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2034.js?cl=5798bc2)  
  

---   

## **regress-2045.js (v8 issue)**  
   
**[Issue 2045:
 Google Maps are broken with V8 3.9.24.1](https://crbug.com/v8/2045)**  
**[Commit: Ensure that arguments object is materialized when deoptimizing from inlined function.](https://chromium.googlesource.com/v8/v8/+/8360ec8)**  
  
Date(Commit): Fri Mar 30 13:22:39 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9963008](https://chromiumcodereview.appspot.com/9963008)  
Regress: [mjsunit/regress/regress-2045.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2045.js)  
```javascript
function foo() {
  assertEquals(2, arguments.length);
}

function bar() {
  G.x;
  return foo.apply(this, arguments);
}

function baz() {
  return bar(1, 2);
};
%PrepareFunctionForOptimization(baz);
G = {
  x: 0
};
baz();
baz();
%OptimizeFunctionOnNextCall(baz);
baz();
delete G.x;
baz();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8360ec8^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=8360ec8)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=8360ec8)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=8360ec8)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=8360ec8)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=8360ec8)  
...  
  

---   

## **regress-120099.js (chromium issue)**  
   
**[Issue 120099:
 Object.defineProperty for an object affects a property with the same name of another object](https://crbug.com/120099)**  
**[Commit: Add missing regression test for r11173.](https://chromium.googlesource.com/v8/v8/+/552393c)**  
  
Date(Commit): Wed Mar 28 15:17:14 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/9873027](https://chromiumcodereview.appspot.com/9873027)  
Regress: [mjsunit/regress/regress-120099.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-120099.js)  
```javascript
'use strict';

var a = Object.create(Object.prototype);
var b = Object.create(Object.prototype);
assertFalse(a === b);

Object.defineProperty(a, 'x', { value: 1 });
assertTrue(a.x === 1);
assertTrue(b.x === undefined);

b.x = 2;
assertTrue(a.x === 1);
assertTrue(b.x === 2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/552393c^!)  
[test/mjsunit/regress/regress-120099.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-120099.js?cl=552393c)  
  

---   

## **regress-2030.js (v8 issue)**  
   
**[Issue 2030:
 Failures in mjsunit/mirror-array on x64.](https://crbug.com/v8/2030)**  
**[Commit: Fix polymorphic load on named fields.](https://chromium.googlesource.com/v8/v8/+/057371d)**  
  
Date(Commit): Tue Mar 27 10:42:38 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9864028](https://chromiumcodereview.appspot.com/9864028)  
Regress: [mjsunit/regress/regress-2030.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2030.js)  
```javascript
function a() {
  this.x = 1;
}
var aa = new a();
%DebugPrint(aa);

function b() {
  this.z = 23;
  this.x = 2;
}
var bb = new b();
%DebugPrint(bb);

function f(o) {
  return o.x;
};
%PrepareFunctionForOptimization(f);
assertSame(1, f(aa));
assertSame(1, f(aa));
assertSame(2, f(bb));
assertSame(2, f(bb));
%OptimizeFunctionOnNextCall(f);
assertSame(1, f(aa));
assertSame(2, f(bb));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/057371d^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=057371d)  
[test/mjsunit/regress/regress-2030.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2030.js?cl=057371d)  
  

---   

## **regress-2032.js (v8 issue)**  
   
**[Issue 2032:
 Case independent regexp test (new webkit test) is failing](https://crbug.com/v8/2032)**  
**[Commit: Fix edge case for case independent regexp character classes.](https://chromium.googlesource.com/v8/v8/+/bfb1e9e)**  
  
Date(Commit): Tue Mar 27 08:42:37 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9860029](https://chromiumcodereview.appspot.com/9860029)  
Regress: [mjsunit/regress/regress-2032.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2032.js)  
```javascript
assertTrue(/[@-A]/i.test("a"));
assertTrue(/[@-A]/i.test("A"));
assertTrue(/[@-A]/i.test("@"));

assertFalse(/[@-A]/.test("a"));
assertTrue(/[@-A]/.test("A"));
assertTrue(/[@-A]/.test("@"));

assertFalse(/[¿-À]/i.test('¾'));
assertTrue(/[¿-À]/i.test('¿'));
assertTrue(/[¿-À]/i.test('À'));
assertTrue(/[¿-À]/i.test('à'));
assertFalse(/[¿-À]/i.test('á'));
assertFalse(/[¿-À]/i.test('Á'));

assertFalse(/[¿-À]/.test('¾'));
assertTrue(/[¿-À]/.test('¿'));
assertTrue(/[¿-À]/.test('À'));
assertFalse(/[¿-À]/.test('à'));
assertFalse(/[¿-À]/.test('á'));
assertFalse(/[¿-À]/.test('á'));
assertFalse(/[¿-À]/i.test('Á'));

assertFalse(/[Ö-×]/i.test('Õ'));
assertTrue(/[Ö-×]/i.test('Ö'));
assertTrue(/[Ö-×]/i.test('ö'));
assertTrue(/[Ö-×]/i.test('×'));
assertFalse(/[Ö-×]/i.test('Ø'));

assertFalse(/[Ö-×]/.test('Õ'));
assertTrue(/[Ö-×]/.test('Ö'));
assertFalse(/[Ö-×]/.test('ö'));
assertTrue(/[Ö-×]/.test('×'));
assertFalse(/[Ö-×]/.test('Ø'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bfb1e9e^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=bfb1e9e)  
[test/mjsunit/regress/regress-2032.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2032.js?cl=bfb1e9e)  
  

---   

## **regress-2027.js (v8 issue)**  
   
**[Issue 2027:
 setMinutes Has Incorrect Return Type](https://crbug.com/v8/2027)**  
**[Commit: Fix the return type of the date set methods.](https://chromium.googlesource.com/v8/v8/+/a47d1c0)**  
  
Date(Commit): Mon Mar 26 10:13:03 2012  
Type: ----  
Code Review: [https://chromiumcodereview.appspot.com/9809010](https://chromiumcodereview.appspot.com/9809010)  
Regress: [mjsunit/regress/regress-2027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2027.js)  
```javascript
var d = new Date(2010, 1, 1);

function Check(time) {
  assertEquals(d.getTime(), time);
}

Check(d.setMilliseconds(10));
Check(d.setSeconds(10));
Check(d.setMinutes(10));
Check(d.setHours(10));
Check(d.setDate(10));
Check(d.setMonth(10));
Check(d.setFullYear(2010));
Check(d.setUTCMilliseconds(10));
Check(d.setUTCSeconds(10));
Check(d.setUTCMinutes(10));
Check(d.setUTCHours(10));
Check(d.setUTCDate(10));
Check(d.setUTCMonth(10));
Check(d.setUTCFullYear(2010));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a47d1c0^!)  
[src/date.js](https://cs.chromium.org/chromium/src/v8/src/date.js?cl=a47d1c0)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=a47d1c0)  
[test/mjsunit/regress/regress-2027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2027.js?cl=a47d1c0)  
  

---   

## **regress-crbug-119926.js (chromium issue)**  
   
**[Issue 119926:
 Use after free in v8::internal::IncrementalMarking::Step](https://crbug.com/119926)**  
**[Commit: Fix missing write barrier in CopyObjectToObjectElements.](https://chromium.googlesource.com/v8/v8/+/4e405b6)**  
  
Date(Commit): Sun Mar 25 15:16:06 2012  
Components/Type: Internals/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-19", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/9808111](https://chromiumcodereview.appspot.com/9808111)  
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
  

---   

## **regress-119925.js (chromium issue)**  
   
**[Issue 119925:
 OOB read in v8::internal::ElementsAccessorBase<v8::internal::FastDoubleElementsAccessor, v8::internal::ElementsK](https://crbug.com/119925)**  
**[Commit: Check double array bounds in HasElementImpl.](https://chromium.googlesource.com/v8/v8/+/8833c99)**  
  
Date(Commit): Sun Mar 25 14:21:51 2012  
Components/Type: Internals/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-19", "Security_severity-None", "Security_Impact-None", "Hotlist-Torque"]  
Code Review: [https://chromiumcodereview.appspot.com/9808110](https://chromiumcodereview.appspot.com/9808110)  
Regress: [mjsunit/regress/regress-119925.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-119925.js)  
```javascript
Array.prototype.__proto__ = { 77e4  : null };
function continueWithinLoop() {
    for (var key in [(1.2)]) {  }
};
continueWithinLoop();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8833c99^!)  
[src/elements.cc](https://cs.chromium.org/chromium/src/v8/src/elements.cc?cl=8833c99)  
[test/mjsunit/regress/regress-119925.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-119925.js?cl=8833c99)  
  

---   

## **regress-1624-strict.js (v8 issue)**  
   
**[Issue 1624:
 Strict eval function leaks variable definitions](https://crbug.com/v8/1624)**  
**[Commit: Fix declarations escaping global strict eval.](https://chromium.googlesource.com/v8/v8/+/79a98de)**  
  
Date(Commit): Thu Mar 15 13:02:21 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9703021](https://chromiumcodereview.appspot.com/9703021)  
Regress: [mjsunit/regress/regress-1624-strict.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1624-strict.js), [mjsunit/regress/regress-1624.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1624.js)  
```javascript
"use strict";
var evil = eval;

var no_touch = 0;
eval('"use strict"; var no_touch = 1;');
assertSame(0, no_touch);

var no_touch = 0;
evil('"use strict"; var no_touch = 2;');
assertSame(0, no_touch);

var no_touch = 0;
eval('var no_touch = 3;');
assertSame(0, no_touch);

var no_touch = 0;
evil('var no_touch = 4;');
assertSame(4, no_touch);

var no_touch = 0;
(function() {
  var no_touch = 0;
  eval('"use strict"; var no_touch = 5;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  var no_touch = 0;
  evil('"use strict"; var no_touch = 6;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  var no_touch = 0;
  eval('var no_touch = 7;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  var no_touch = 0;
  evil('var no_touch = 8;');
  assertSame(0, no_touch);
})()
assertSame(8, no_touch);

var no_touch = 0;
(function() {
  "use strict";
  var no_touch = 0;
  eval('"use strict"; var no_touch = 9;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  "use strict";
  var no_touch = 0;
  evil('"use strict"; var no_touch = 10;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  "use strict";
  var no_touch = 0;
  eval('var no_touch = 11;');
  assertSame(0, no_touch);
})()
assertSame(0, no_touch);

var no_touch = 0;
(function() {
  "use strict";
  var no_touch = 0;
  evil('var no_touch = 12;');
  assertSame(0, no_touch);
})()
assertSame(12, no_touch);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/79a98de^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=79a98de)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=79a98de)  
[test/mjsunit/regress/regress-1624-strict.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1624-strict.js?cl=79a98de)  
[test/mjsunit/regress/regress-1624.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1624.js?cl=79a98de)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=79a98de)  
  

---   

## **regress-1973.js (v8 issue)**  
   
**[Issue 1973:
 [[Get]]/[[Put]] for primitives should wrap on non-strict accessor call](https://crbug.com/v8/1973)**  
**[Commit: Fix wrapping of receiver for non-strict callbacks.](https://chromium.googlesource.com/v8/v8/+/2c7f0ed)**  
  
Date(Commit): Wed Mar 14 17:42:19 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9705020](https://chromiumcodereview.appspot.com/9705020)  
Regress: [mjsunit/regress/regress-1973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1973.js)  
```javascript
function TestAccessorWrapping(primitive) {
  var prototype = Object.getPrototypeOf(Object(primitive))
  // Check that strict mode passes unwrapped this value.
  var strict_type = typeof primitive;
  Object.defineProperty(prototype, "strict", {
    get: function() { "use strict"; assertSame(strict_type, typeof this); },
    set: function() { "use strict"; assertSame(strict_type, typeof this); }
  });
  primitive.strict = primitive.strict;
  // Check that non-strict mode passes wrapped this value.
  var sloppy_type = typeof Object(primitive);
  Object.defineProperty(prototype, "sloppy", {
    get: function() { assertSame(sloppy_type, typeof this); },
    set: function() { assertSame(sloppy_type, typeof this); }
  });
  primitive.sloppy = primitive.sloppy;
}

TestAccessorWrapping(true);
TestAccessorWrapping(0);
TestAccessorWrapping({});
TestAccessorWrapping("");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2c7f0ed^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=2c7f0ed)  
[test/mjsunit/getter-in-value-prototype.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/getter-in-value-prototype.js?cl=2c7f0ed)  
[test/mjsunit/regress/regress-1973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1973.js?cl=2c7f0ed)  
  

---   

## **regress-115452.js (chromium issue)**  
   
**[Issue 115452:
 JavaScript: Constants (defined using Object.defineProperty) are overwritten by a function declaration.](https://crbug.com/115452)**  
**[Commit: Function declarations shall not overwrite read-only global properties.](https://chromium.googlesource.com/v8/v8/+/46001aa)**  
  
Date(Commit): Wed Mar 14 13:51:00 2012  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [https://chromiumcodereview.appspot.com/9696035](https://chromiumcodereview.appspot.com/9696035)  
Regress: [mjsunit/regress/regress-115452.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-115452.js)  
```javascript
function foobl() {}
assertTrue(typeof this.foobl == "function");
assertTrue(Object.getOwnPropertyDescriptor(this, "foobl").writable);

Object.defineProperty(this, "foobl", {value: 1, writable: false});
assertSame(1, this.foobl);
assertFalse(Object.getOwnPropertyDescriptor(this, "foobl").writable);

try {
  eval("function foobl() {}");  // Should throw.
  assertUnreachable();
} catch (e) {
  assertInstanceof(e, TypeError);
}
assertSame(1, this.foobl);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/46001aa^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=46001aa)  
[test/mjsunit/regress/regress-115452.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-115452.js?cl=46001aa)  
  

---   

## **regress-117794.js (chromium issue)**  
   
**[Issue 117794:
 [LangFuzz] Crash on heap with invalid read through GetPropertyWithCallback](https://crbug.com/117794)**  
**[Commit: Ensure there is a smi check of the receiver for global load and call ICs.](https://chromium.googlesource.com/v8/v8/+/7d6fd56)**  
  
Date(Commit): Tue Mar 13 11:39:30 2012  
Components/Type: Internals/Bug-Security  
Labels: ["reward-500", "CVE-2011-3057", "M-18", "Merge-Merged", "Security_Impact-Stable", "Security_Severity-Medium", "allpublic", "CVE_description-submitted"]  
Code Review: [https://chromiumcodereview.appspot.com/9691038](https://chromiumcodereview.appspot.com/9691038)  
Regress: [mjsunit/regress/regress-117794.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-117794.js)  
```javascript
print = function() {}

function constructor() {};

function assertHasOwnProperties(object, limit) {
  for (var i = 0; i < limit; i++) {  }
}

try {
  Object.keys();
} catch(exc2) {
  print(exc2.stack);
}

var x1 = new Object();

try {
  new Function("A Man Called Horse", x1.d);
} catch(exc3) {
  print(exc3.stack);
}

try {
  (-(true)).toPrecision(0x30, 'lib1-f1');
} catch(exc1) {
  print(exc1.stack);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7d6fd56^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=7d6fd56)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=7d6fd56)  
[src/mips/stub-cache-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/stub-cache-mips.cc?cl=7d6fd56)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=7d6fd56)  
[test/mjsunit/regress/regress-117794.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-117794.js?cl=7d6fd56)  
  

---   

## **regress-sqrt.js (other issue)**  
   
**[Commit: Ensure consistency of Math.sqrt on Intel platforms.](https://chromium.googlesource.com/v8/v8/+/7659bea)**  
  
Date(Commit): Mon Mar 12 14:56:04 2012  
Code Review: [https://chromiumcodereview.appspot.com/9690010](https://chromiumcodereview.appspot.com/9690010)  
Regress: [mjsunit/regress/regress-sqrt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-sqrt.js)  
```javascript
function f(x) {
  return Math.sqrt(x);
};
%PrepareFunctionForOptimization(f);
var x = 7.0506280066499245e-233;

var a = f(x);

f(0.1);
f(0.2);
%OptimizeFunctionOnNextCall(f);

var b = f(x);

assertEquals(a, b);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7659bea^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=7659bea)  
[src/codegen.h](https://cs.chromium.org/chromium/src/v8/src/codegen.h?cl=7659bea)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=7659bea)  
[src/mips/codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/codegen-mips.cc?cl=7659bea)  
[src/platform-posix.cc](https://cs.chromium.org/chromium/src/v8/src/platform-posix.cc?cl=7659bea)  
...  
  
  
---   

## **regress-1980.js (v8 issue)**  
   
**[Issue 1980:
 Error.prototype.toString throws 'illegal access' instead of TypeError](https://crbug.com/v8/1980)**  
**[Commit: Fix Error.prototype.toString to throw TypeError.](https://chromium.googlesource.com/v8/v8/+/8c2708d)**  
  
Date(Commit): Mon Mar 05 13:57:48 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9568005](https://chromiumcodereview.appspot.com/9568005)  
Regress: [mjsunit/regress/regress-1980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1980.js)  
```javascript
var msg = "Method Error.prototype.toString called on incompatible receiver ";

var invalid_this = [ "invalid", 23, undefined, null ];
for (var i = 0; i < invalid_this.length; i++) {
  var exception = false;
  try {
    Error.prototype.toString.call(invalid_this[i]);
  } catch (e) {
    exception = true;
    assertEquals(msg + invalid_this[i], e.message);
  }
  assertTrue(exception);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c2708d^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=8c2708d)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=8c2708d)  
[test/mjsunit/function-call.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/function-call.js?cl=8c2708d)  
[test/mjsunit/regress/regress-1980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1980.js?cl=8c2708d)  
  

---   

## **regress-transcendental.js (other issue)**  
   
**[Commit: Ensure consistent result of transcendental functions.](https://chromium.googlesource.com/v8/v8/+/12f2099)**  
  
Date(Commit): Fri Mar 02 14:33:15 2012  
Code Review: [https://chromiumcodereview.appspot.com/9572009](https://chromiumcodereview.appspot.com/9572009)  
Regress: [mjsunit/regress/regress-transcendental.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-transcendental.js)  
```javascript
function test(f, x, name) {
  // Reset transcendental cache.
  gc();
  // Initializing cache leads to a runtime call.
  var runtime_result = f(x);
  // Flush transcendental cache entries and optimize f.
  for (var i = 0; i < 100000; i++) f(i);
  // Calculate using generated code.
  var gencode_result = f(x);
  print(name + " runtime function: " + runtime_result);
  print(name + " generated code  : " + gencode_result);
  assertEquals(gencode_result, runtime_result);
}

test(Math.tan, -1.57079632679489660000, "Math.tan");
test(Math.sin, 6.283185307179586, "Math.sin");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/12f2099^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=12f2099)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=12f2099)  
[src/codegen.h](https://cs.chromium.org/chromium/src/v8/src/codegen.h?cl=12f2099)  
[src/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap-inl.h?cl=12f2099)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=12f2099)  
...  
  
  
---   

## **regress-toint32.js (other issue)**  
   
**[Commit: Fix a register assignment bug in typed array stores without SSE3 available.](https://chromium.googlesource.com/v8/v8/+/1e40f7a)**  
  
Date(Commit): Thu Mar 01 12:45:46 2012  
Code Review: [https://chromiumcodereview.appspot.com/9565007](https://chromiumcodereview.appspot.com/9565007)  
Regress: [mjsunit/compiler/regress-toint32.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-toint32.js)  
```javascript
var a = new Int32Array(1);
var G = 0x80000000;

function f(x) {
  var v = x;
  v = v + 1;
  a[0] = v;
  v = v - 1;
  return v;
}

%PrepareFunctionForOptimization(f);
assertEquals(G, f(G));
assertEquals(G, f(G));
%OptimizeFunctionOnNextCall(f);
assertEquals(G, f(G));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e40f7a^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=1e40f7a)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=1e40f7a)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=1e40f7a)  
[src/ia32/lithium-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-ia32.cc?cl=1e40f7a)  
[src/mips/lithium-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-mips.cc?cl=1e40f7a)  
...  
  
  
---   

## **regress-inlining-function-literal-context.js (other issue)**  
   
**[Commit: On ia32 LFunctionLiteral instruction should get context from esi register instead of stack slot.](https://chromium.googlesource.com/v8/v8/+/f5c8ac9)**  
  
Date(Commit): Tue Feb 21 12:10:04 2012  
Code Review: [https://chromiumcodereview.appspot.com/9425061](https://chromiumcodereview.appspot.com/9425061)  
Regress: [mjsunit/regress/regress-inlining-function-literal-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inlining-function-literal-context.js)  
```javascript
function mkbaz(x) {
  function baz() {
    return function() {
      return [x];
    };
  }
  return baz;
}

var baz = mkbaz(1);

function foo() {
  var f = baz();
  return f();
}

;
%PrepareFunctionForOptimization(foo);
gc();
gc();

assertArrayEquals([1], foo());
assertArrayEquals([1], foo());
%OptimizeFunctionOnNextCall(foo);
assertArrayEquals([1], foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f5c8ac9^!)  
[src/ia32/lithium-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/lithium-codegen-ia32.cc?cl=f5c8ac9)  
[test/mjsunit/regress/regress-inlining-function-literal-context.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-inlining-function-literal-context.js?cl=f5c8ac9)  
  
  
---   

## **regress-1790.js (v8 issue)**  
   
**[Issue 1790:
 Sequence of element access in built-in Array functions not spec conform.](https://crbug.com/v8/1790)**  
**[Commit: Fix sequence of element access in array builtins.](https://chromium.googlesource.com/v8/v8/+/e423637)**  
  
Date(Commit): Fri Feb 17 10:06:26 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9419044](https://chromiumcodereview.appspot.com/9419044)  
Regress: [mjsunit/regress/regress-1790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1790.js)  
```javascript
function CheckSequence(builtin, callback) {
  var array = [1,2,3];
  var callback_count = 0;
  var callback_wrapper = function() {
    callback_count++;
    return callback()
  }

  // Define getter that will delete itself upon first invocation.
  Object.defineProperty(array, '1', {
    get: function () { delete array[1]; },
    configurable: true
  });

  assertTrue(array.hasOwnProperty('1'));
  builtin.apply(array, [callback_wrapper, 'argument']);
  assertFalse(array.hasOwnProperty('1'));
  assertEquals(3, callback_count);
}

CheckSequence(Array.prototype.every,       function() { return true; });
CheckSequence(Array.prototype.filter,      function() { return true; });
CheckSequence(Array.prototype.forEach,     function() { return 0; });
CheckSequence(Array.prototype.map,         function() { return 0; });
CheckSequence(Array.prototype.reduce,      function() { return 0; });
CheckSequence(Array.prototype.reduceRight, function() { return 0; });
CheckSequence(Array.prototype.some,        function() { return false; });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e423637^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=e423637)  
[test/mjsunit/regress/regress-1790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1790.js?cl=e423637)  
[test/test262/test262.status](https://cs.chromium.org/chromium/src/v8/test/test262/test262.status?cl=e423637)  
  

---   

## **regress-113924.js (chromium issue)**  
   
**[Issue 113924:
 [LangFuzz] Crash at v8::internal::HashTable<...>::FindEntry with invalid read](https://crbug.com/113924)**  
**[Commit: Fix crashing bugs in store-and-grow IC for double values.](https://chromium.googlesource.com/v8/v8/+/71cd77e)**  
  
Date(Commit): Tue Feb 14 15:09:49 2012  
Components/Type: Internals/Bug-Security  
Labels: ["Restrict-AddIssueComment-EditIssue", "Reward-1000", "M-19", "Security_Impact-None", "Security_Severity-High", "allpublic"]  
Code Review: [https://chromiumcodereview.appspot.com/9365055](https://chromiumcodereview.appspot.com/9365055)  
Regress: [mjsunit/regress/regress-113924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-113924.js)  
```javascript
var count=12000;
while(count--) {
  eval("var a = new Object(10); a[2] += 7;");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/71cd77e^!)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=71cd77e)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=71cd77e)  
[test/mjsunit/regress/regress-113924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-113924.js?cl=71cd77e)  
  

---   

## **regress-smi-only-concat.js (other issue)**  
   
**[Commit: Fix elements transition bug related to array.concat.](https://chromium.googlesource.com/v8/v8/+/3e58827)**  
  
Date(Commit): Wed Feb 08 09:50:13 2012  
Code Review: [https://chromiumcodereview.appspot.com/9358018](https://chromiumcodereview.appspot.com/9358018)  
Regress: [mjsunit/regress/regress-smi-only-concat.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-only-concat.js)  
```javascript
var fast_array = ['a', 'b'];
var array = fast_array.concat(fast_array);

assertTrue(%HasObjectElements(fast_array));
assertTrue(%HasObjectElements(array));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e58827^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=3e58827)  
[test/mjsunit/regress/regress-smi-only-concat.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-smi-only-concat.js?cl=3e58827)  
  
  
---   

## **regress-1924.js (v8 issue)**  
   
**[Issue 1924:
 Parse error with labelled statements](https://crbug.com/v8/1924)**  
**[Commit: Fix handling of 'c: if (0) break c; else ()' where a parser optimization](https://chromium.googlesource.com/v8/v8/+/f0a87d7)**  
  
Date(Commit): Wed Feb 08 08:40:11 2012  
Type: Bug  
Code Review: [https://chromiumcodereview.appspot.com/9159043](https://chromiumcodereview.appspot.com/9159043)  
Regress: [mjsunit/regress/regress-1924.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1924.js)  
```javascript
a: break a;
a: b: break a;
a: b: break b;
assertThrows("a: break a a", SyntaxError)
assertThrows("a: break a 1", SyntaxError)
assertThrows("a: break a ''", SyntaxError)
assertThrows("a: break a var b", SyntaxError)
assertThrows("a: break a {}", SyntaxError)

a: if (0) break a;
b: if (0) {break b;} else {}
c: if (0) break c; else {}
d: if (0) break d; else break d;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0a87d7^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=f0a87d7)  
[test/mjsunit/regress/regress-1924.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1924.js?cl=f0a87d7)  
  

---   

## **regress-110509.js (chromium issue)**  
   
**[Issue 110509:
 Crash in renderer ("Aw snap!") when visiting CWS after signing in](https://crbug.com/110509)**  
**[Commit: Ensure that LRandom restores rsi after call to the C function on x64.](https://chromium.googlesource.com/v8/v8/+/704c92c)**  
  
Date(Commit): Thu Jan 19 08:43:34 2012  
Components/Type: Internals/Bug  
Labels: ["Restrict-AddIssueComment-EditIssue", "ReleaseBlock-Beta", "M-18", "hasTestcase"]  
Code Review: [https://chromiumcodereview.appspot.com/9265003](https://chromiumcodereview.appspot.com/9265003)  
Regress: [mjsunit/regress/regress-110509.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-110509.js)  
```javascript
function foo() {
  Math.random();
  new Function("");
};
%PrepareFunctionForOptimization(foo);
foo();
foo();
foo();
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/704c92c^!)  
[src/x64/lithium-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-codegen-x64.cc?cl=704c92c)  
[test/mjsunit/regress/regress-110509.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-110509.js?cl=704c92c)  
  

---   

## **regress-1898.js (v8 issue)**  
   
**[Issue 1898:
 Sputnik math tests crash due to Math.min/max](https://crbug.com/v8/1898)**  
**[Commit: Fixing issue 1898 (using HChange outside the insert-representation-changes phase).](https://chromium.googlesource.com/v8/v8/+/ddc0144)**  
  
Date(Commit): Fri Jan 13 07:48:44 2012  
Type: Bug  
Code Review: [http://codereview.chromium.org/9190047](http://codereview.chromium.org/9190047)  
Regress: [mjsunit/regress/regress-1898.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1898.js)  
```javascript
function f(x) {
  Math.log(Math.min(0.1, Math.abs(x)));
};
%PrepareFunctionForOptimization(f);
f(0.1);
f(0.1);
%OptimizeFunctionOnNextCall(f);
f(0.1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ddc0144^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=ddc0144)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=ddc0144)  
[test/mjsunit/regress/regress-1898.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-1898.js?cl=ddc0144)  
  

---   
