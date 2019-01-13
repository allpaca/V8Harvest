# V8Harvest  
The Harvest of V8 regress in 2010.  
  

## **regress-intoverflow.js (other issue)**  
   
**[Commit: Fix bugs in the range analysis for integers.](https://chromium.googlesource.com/v8/v8/+/73737fc)**  
  
Date(Commit): Thu Dec 16 18:01:36 2010  
Code Review: [http://codereview.chromium.org/5860009](http://codereview.chromium.org/5860009)  
Regress: [mjsunit/compiler/regress-intoverflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-intoverflow.js)  
```javascript
function testMul(a, b) {
  a *= 2;
  b *= 2;
  if (a < 1 && b < 1) {
    return a * b;
  }
}

for (var i=0; i<5; i++) testMul(0,0);
%OptimizeFunctionOnNextCall(testMul);
assertEquals(4611686018427388000, testMul(-0x40000000, -0x40000000));

function testAdd(a, b) {
  a *= 2;
  b *= 2;
  if (a < 1 && b < 1) {
    return a + b;
  }
}

for (var i=0; i<5; i++) testAdd(0,0);
%OptimizeFunctionOnNextCall(testAdd);
assertEquals(-4294967296, testAdd(-0x40000000, -0x40000000));


function testSub(a, b) {
  a *= 2;
  b *= 2;
  if (b == 2) {print(a); print(b);}
  if (a < 1 && b < 3) {
    return a - b;
  }
}

for (var i=0; i<5; i++) testSub(0,0);
%OptimizeFunctionOnNextCall(testSub);
assertEquals(-2147483650, testSub(-0x40000000, 1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73737fc^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=73737fc)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=73737fc)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=73737fc)  
[test/mjsunit/compiler/regress-intoverflow.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-intoverflow.js?cl=73737fc)  
  
  
---   

## **regress-995.js (v8 issue)**  
   
**[Issue: %_IsArray returns true for string wrapper after %_IsSpecObject call.](https://crbug.com/v8/995)**  
**[Commit: A number of instructions use GVN but do not provide a comparison](https://chromium.googlesource.com/v8/v8/+/6e30a77)**  
  
Date(Commit): Thu Dec 16 15:40:02 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5898003](http://codereview.chromium.org/5898003)  
Regress: [mjsunit/regress/regress-995.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-995.js)  
```javascript
function f(value) {
  if (%_IsJSReceiver(value)) {
    if ((%_IsArray(value))) assertTrue(false);
  }
}
f(new String("bar"));

function h(value) {
  if (value == null) {
    if (value === null) assertTrue(false);
  }
}
h(undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6e30a77^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=6e30a77)  
[test/mjsunit/regress/regress-995.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-995.js?cl=6e30a77)  
  

---   

## **regress-687.js (v8 issue)**  
   
**[Issue: Object.defineProperty problems with redefining accessor properties in some situations.](https://crbug.com/v8/687)**  
**[Commit: Change Object.defineProperty to accept undefined as getters and setters and to correctly accept overriding an accessor with a data property.](https://chromium.googlesource.com/v8/v8/+/357afa3)**  
  
Date(Commit): Thu Dec 16 12:21:08 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/5861006](http://codereview.chromium.org/5861006)  
Regress: [mjsunit/regress/regress-687.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-687.js)  
```javascript
var obj = { get value() {}, set value (v) { throw "Error";} };
assertDoesNotThrow(
    Object.defineProperty(obj, "value",
                          { value: 5, writable:true, configurable: true }));
var desc = Object.getOwnPropertyDescriptor(obj, "value");
assertEquals(obj.value, 5);
assertTrue(desc.configurable);
assertTrue(desc.enumerable);
assertTrue(desc.writable);
assertEquals(desc.get, undefined);
assertEquals(desc.set, undefined);


var proto = {
  get value() {},
  set value(v) { Object.defineProperty(this, "value", {value: v}); }
};

var create = Object.create(proto);

assertEquals(create.value, undefined);
assertDoesNotThrow(create.value = 4);
assertEquals(create.value, 4);

var obj1 = {};
Object.defineProperty(obj1, 'p', {get: undefined, set: undefined});
assertTrue("p" in obj1);
desc = Object.getOwnPropertyDescriptor(obj1, "p");
assertFalse(desc.configurable);
assertFalse(desc.enumerable);
assertEquals(desc.value, undefined);
assertEquals(desc.get, undefined);
assertEquals(desc.set, undefined);


var obj2 = { get p() {}};
Object.defineProperty(obj2, 'p', {get: undefined})
assertTrue("p" in obj2);
desc = Object.getOwnPropertyDescriptor(obj2, "p");
assertTrue(desc.configurable);
assertTrue(desc.enumerable);
assertEquals(desc.value, undefined);
assertEquals(desc.get, undefined);
assertEquals(desc.set, undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/357afa3^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=357afa3)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=357afa3)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=357afa3)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=357afa3)  
[test/mjsunit/object-define-property.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-define-property.js?cl=357afa3)  
...  
  

---   

## **regress-974.js (v8 issue)**  
   
**[Issue: Verify heap assertion found by fuzzing](https://crbug.com/v8/974)**  
**[Commit: Fix issue 974.](https://chromium.googlesource.com/v8/v8/+/ace6290)**  
  
Date(Commit): Wed Dec 15 16:14:29 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5883003](http://codereview.chromium.org/5883003)  
Regress: [mjsunit/regress/regress-974.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-974.js)  
```javascript
eval("(function(){try {  } catch(x) {  } finally { gc() }})")();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ace6290^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=ace6290)  
[src/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen.cc?cl=ace6290)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=ace6290)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=ace6290)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=ace6290)  
...  
  

---   

## **regress-982.js (v8 issue)**  
   
**[Issue: Reliability stress test: Crash in generated code on http://tv.sbs.co.kr/drama_main.jsp](https://crbug.com/v8/982)**  
**[Commit: Fix issue 982.](https://chromium.googlesource.com/v8/v8/+/655b308)**  
  
Date(Commit): Wed Dec 15 14:35:46 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5898001](http://codereview.chromium.org/5898001)  
Regress: [mjsunit/regress/regress-982.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-982.js)  
```javascript
function f(a) {
 return {className: 'xxx'};
};

var x = 1;

function g(active) {
 for (i = 1; i <= 20000; i++) {
   if (i == active) {
     x = i;
     if (f("" + i) != null) { }
   } else {
     if (f("" + i) != null) { }
   }
 }
}

g(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/655b308^!)  
[src/lithium-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.cc?cl=655b308)  
[test/mjsunit/regress/regress-982.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-982.js?cl=655b308)  
  

---   

## **regress-swapelements.js (other issue)**  
   
**[Commit: Add array bound checks to code generated for SwapElements. This fixes a bug that lead to a segfault when an array was modified while it was sorted.](https://chromium.googlesource.com/v8/v8/+/5f962f2)**  
  
Date(Commit): Wed Dec 15 09:52:58 2010  
Code Review: [http://codereview.chromium.org/5686006](http://codereview.chromium.org/5686006)  
Regress: [mjsunit/regress/regress-swapelements.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-swapelements.js)  
```javascript
function Item(val) {
  this.value = val;
}


var size = 23;
var array1 = new Array(size);


function myToString() {
  array1.splice(0, 1);
  return this.value.toString();
}


function test() {
  for (var i = 0; i < size; i++) {
    array1[i] = new Item(i);
    array1[i].toString = myToString;
  }
  array1.sort();
}


test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f962f2^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=5f962f2)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=5f962f2)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=5f962f2)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=5f962f2)  
[test/mjsunit/regress/regress-swapelements.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-swapelements.js?cl=5f962f2)  
  
  
---   

## **regress-969.js (v8 issue)**  
   
**[Issue: Assertion failure in deoptimizer-ia32.cc](https://crbug.com/v8/969)**  
**[Commit: Deoptimize to the proper target after assignment side effects.](https://chromium.googlesource.com/v8/v8/+/49f4c39)**  
  
Date(Commit): Mon Dec 13 16:29:47 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5682010](http://codereview.chromium.org/5682010)  
Regress: [mjsunit/regress/regress-969.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-969.js)  
```javascript
function first(x, y) { return x; }
var y = 0;
var o = {};
o.x = 0;
o[0] = 0;

x0 = 0;
function test0() { return first((y = 1, typeof x0), 2); }
assertEquals('number', test0());
delete x0;
assertEquals('undefined', test0());

x1 = 0;
function test1() { return first((y += 1, typeof x1), 2); }
assertEquals('number', test1(), 'test1 before');
delete x1;
assertEquals('undefined', test1(), 'test1 after');

x2 = 0;
function test2() { return first((++y, typeof x2), 2); }
assertEquals('number', test2(), 'test2 before');
delete x2;
assertEquals('undefined', test2(), 'test2 after');

x3 = 0;
function test3() { return first((y++, typeof x3), 2); }
assertEquals('number', test3(), 'test3 before');
delete x3;
assertEquals('undefined', test3(), 'test3 after');


x4 = 0;
function test4() { return first((o.x = 1, typeof x4), 2); }
assertEquals('number', test4());
delete x4;
assertEquals('undefined', test4());

x5 = 0;
function test5() { return first((o.x += 1, typeof x5), 2); }
assertEquals('number', test5());
delete x5;
assertEquals('undefined', test5());

x6 = 0;
function test6() { return first((++o.x, typeof x6), 2); }
assertEquals('number', test6());
delete x6;
assertEquals('undefined', test6());

x7 = 0;
function test7() { return first((o.x++, typeof x7), 2); }
assertEquals('number', test7());
delete x7;
assertEquals('undefined', test7());


x8 = 0;
function test8(index) { return first((o[index] = 1, typeof x8), 2); }
assertEquals('number', test8());
delete x8;
assertEquals('undefined', test8());

x9 = 0;
function test9(index) { return first((o[index] += 1, typeof x9), 2); }
assertEquals('number', test9());
delete x9;
assertEquals('undefined', test9());

x10 = 0;
function test10(index) { return first((++o[index], typeof x10), 2); }
assertEquals('number', test10());
delete x10;
assertEquals('undefined', test10());

x11 = 0;
function test11(index) { return first((o[index]++, typeof x11), 2); }
assertEquals('number', test11());
delete x11;
assertEquals('undefined', test11());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49f4c39^!)  
[src/ast-inl.h](https://cs.chromium.org/chromium/src/v8/src/ast-inl.h?cl=49f4c39)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=49f4c39)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=49f4c39)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=49f4c39)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=49f4c39)  
...  
  

---   

## **regress-962.js (v8 issue)**  
   
**[Issue: Assert failure in debug linux 32-bit Chrome with V8 3.0.0.  In register allocator.](https://crbug.com/v8/962)**  
**[Commit: Fix issue 962.](https://chromium.googlesource.com/v8/v8/+/65f98b1)**  
  
Date(Commit): Fri Dec 10 14:25:10 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5720001](http://codereview.chromium.org/5720001)  
Regress: [mjsunit/regress/regress-962.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-962.js)  
```javascript
function L(scope) { this.s = new Object(); }

L.prototype.c = function() { return true; }

function F() {
  this.l = [new L, new L];
}

F.prototype.foo = function () {
    var f, d = arguments,
        e, b = this.l,
        g;
    for (e = 0; e < b.length; e++) {
        g = b[e];
        f = g.c.apply(g.s, d);
        if (f === false) {
            break
        }
    }
    return f
}


var ctx = new F;

for (var i = 0; i < 5; i++) ctx.foo();
%OptimizeFunctionOnNextCall(F.prototype.foo);
ctx.foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65f98b1^!)  
[src/lithium-allocator.cc](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.cc?cl=65f98b1)  
[src/lithium-allocator.h](https://cs.chromium.org/chromium/src/v8/src/lithium-allocator.h?cl=65f98b1)  
[test/mjsunit/regress/regress-962.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-962.js?cl=65f98b1)  
  

---   

## **regress-3260426.js (other issue)**  
   
**[Commit: Be more careful about exiting inlined functions in a test context.](https://chromium.googlesource.com/v8/v8/+/e0d3f6a)**  
  
Date(Commit): Tue Dec 07 12:07:40 2010  
Code Review: [http://codereview.chromium.org/5566005](http://codereview.chromium.org/5566005)  
Regress: [mjsunit/compiler/regress-3260426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3260426.js)  
```javascript
function always_false() {}
function test() { return always_false() ? 0 : 1; }

assertEquals(1, test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0d3f6a^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=e0d3f6a)  
[test/mjsunit/compiler/regress-3260426.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-3260426.js?cl=e0d3f6a)  
  
  
---   

## **regress-stacktrace-methods.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-stacktrace-methods.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-stacktrace-methods.js)  
```javascript
function Hest() {}
function Svin() {}

Svin.prototype.two = function() { /* xxxxxxx */ o.three(); }

Hest.prototype.one = function(x) { x.two(); }

Hest.prototype.three = function() { if (v == 42) throw new Error("urg"); }

var o = new Hest();
var s = new Svin();
var v = 0;

for (var i = 0; i < 5; i++) {
  o.one(s);
}
%OptimizeFunctionOnNextCall(Hest.prototype.one);
%OptimizeFunctionOnNextCall(Hest.prototype.three);
o.one(s);

v = 42;

try {
  o.one(s);
} catch (e) {
  var stack = e.stack.toString();
  var p3 = stack.indexOf("at Hest.three");
  var p2 = stack.indexOf("at Svin.two");
  var p1 = stack.indexOf("at Hest.one");
  assertTrue(p3 != -1);
  assertTrue(p2 != -1);
  assertTrue(p1 != -1);
  assertTrue(p3 < p2);
  assertTrue(p2 < p1);
  assertTrue(stack.indexOf("38:56") != -1);
  assertTrue(stack.indexOf("34:51") != -1);
  assertTrue(stack.indexOf("36:38") != -1);
  assertTrue(stack.indexOf("54:5") != -1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-stacktrace.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-stacktrace.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-stacktrace.js)  
```javascript
eval("function two() { /* xxxxxxx */ three(); }");

function one() {
  two();
}

function three() {
  throw new Error("urg");
}

try {
 one();
} catch (e) {
  var stack = e.stack.toString();
  var p3 = stack.indexOf("at three");
  var p2 = stack.indexOf("at two");
  var p1 = stack.indexOf("at one");
  assertTrue(p3 != -1);
  assertTrue(p2 != -1);
  assertTrue(p1 != -1);
  assertTrue(p3 < p2);
  assertTrue(p2 < p1);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-rep-change.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-rep-change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-rep-change.js)  
```javascript
function test(start) {
  if (true) {
    for (var i = start; i < 10; i++) { }
  }
  for (var i = start; i < 10; i++) { }
}

var n = 3;

for (var i = 0; i < n; ++i) {
  test(0);
}

%OptimizeFunctionOnNextCall(test);
test(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-or.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-or.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-or.js)  
```javascript
function f1(x) {
  var c = "fail";
  if (!x || g1()) {
    c = ~x;
  }
  return c;
}

function g1() { try { return 1; } finally {} }

for (var i = 0; i < 5; i++) f1(42);
%OptimizeFunctionOnNextCall(f1);

assertEquals(-1, f1(0));
assertEquals(-43, f1(42));
assertEquals(-1, f1(""));

function f2(x) {
  var c = "fail";
  if (!x || !g2()) {
    c = ~x;
  }
  return c;
}

function g2() { try { return 0; } finally {} }

for (var i = 0; i < 5; i++) f2(42);
%OptimizeFunctionOnNextCall(f2);

assertEquals(-1, f2(""));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-max.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-max.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-max.js)  
```javascript
for (var i = 0; i < 5; i++) Math.max(0, 0);
Math.max(0, 0);

var r = Math.max(-0, -0);
assertEquals(-Infinity, 1 / r);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-loop-deopt.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-loop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-loop-deopt.js)  
```javascript
function h() {
  var i = 3, j = 0;
  while(--i >= 0) {
    var x = i & 1;
    if(x > 0) {
      continue;
    }
    j++;
  }
  return j;
}

assertEquals(2, h());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-gvn.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-gvn.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-gvn.js)  
```javascript
function test(a) {
  var res = a[0] + a[0];
  if (res == 0) {
    a[0] = 1;
  }
  return a[0];
}

var a = new Array();

var n = 100;

var result = 0;
for (var i = 0; i < n; ++i) {
  if (i == 10) %OptimizeFunctionOnNextCall(test);
  a[0] = 0;
  result += test(a);
}


assertEquals(n, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-gap.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-gap.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-gap.js)  
```javascript
function small_select(n, v1, v2) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = tmp;
  }
  return v1;
}

function select(n, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = v6;
    v6 = v7;
    v7 = v8;
    v8 = v9;
    v9 = v10;
    v10 = tmp;
  }
  return v1;
}

function select_while(n, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10) {
  var i = 0;
  while (i < n) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = v6;
    v6 = v7;
    v7 = v8;
    v8 = v9;
    v9 = v10;
    v10 = tmp;
    i++;
  }
  return v1;
}

function two_cycles(n, v1, v2, v3, v4, v5, x1, x2, x3, x4, x5) {
  for (var i = 0; i < n; ++i) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = tmp;
    tmp = x1;
    x1 = x2;
    x2 = x3;
    x3 = x4;
    x4 = x5;
    x5 = tmp;
  }
  return v1 + x1;
}

function two_cycles_while(n, v1, v2, v3, v4, v5, x1, x2, x3, x4, x5) {
  var i = 0;
  while (i < n) {
    var tmp = v1;
    v1 = v2;
    v2 = v3;
    v3 = v4;
    v4 = v5;
    v5 = tmp;
    tmp = x1;
    x1 = x2;
    x2 = x3;
    x3 = x4;
    x4 = x5;
    x5 = tmp;
    i++;
  }
  return v1 + x1;
}
assertEquals(1, small_select(0, 1, 2));
assertEquals(2, small_select(1, 1, 2));
assertEquals(1, small_select(10, 1, 2));

assertEquals(1, select(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4, select(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(10, select(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1 + 6, two_cycles(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4 + 9, two_cycles(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(5 + 10, two_cycles(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1, select_while(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4, select_while(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(10, select_while(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

assertEquals(1 + 6, two_cycles_while(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(4 + 9, two_cycles_while(3, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
assertEquals(5 + 10, two_cycles_while(9, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-funcaller.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-funcaller.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-funcaller.js)  
```javascript
function A() {}

function fun(x) {
  if (x == 0) return fun.caller;
  if (x == 1) return gee.caller;
  return 42;
}
function gee(x) { return this.f(x); }

A.prototype.f = fun;
A.prototype.g = gee;

var o = new A();

for (var i=0; i<5; i++) {
  o.g(i);
}
%OptimizeFunctionOnNextCall(o.g);
assertEquals(gee, o.g(0));
assertEquals(null, o.g(1));

function hej(x) {
  if (x == 0) return o.g(x);
  if (x == 1) return o.g(x);
  return o.g(x);
}

for (var j=0; j<5; j++) {
  hej(j);
}
%OptimizeFunctionOnNextCall(hej);
assertEquals(gee, hej(0));
assertEquals(hej, hej(1));

function from_eval(x) {
  if (x == 0) return eval("o.g(x);");
  if (x == 1) return eval("o.g(x);");
  return o.g(x);
}

for (var j=0; j<5; j++) {
  from_eval(j);
}
%OptimizeFunctionOnNextCall(from_eval);
assertEquals(gee, from_eval(0));
assertEquals(from_eval, from_eval(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-funarguments.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-funarguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-funarguments.js)  
```javascript
function A() {}
function B() {}

function fee(x, y) {
  if (x == 1) return fee["arg" + "uments"];
  if (x == 2) return gee["arg" + "uments"];
  return 42;
}

function gee(x) { return this.f(2 - x, "f"); }

function foo(x, y) {
  if (x == 0) return foo["arg" + "uments"];
  if (x == 1) return goo["arg" + "uments"];
  return 42;
}

function goo(x) { return this.f(x, "f"); }

A.prototype.f = fee;
A.prototype.g = gee;

B.prototype.f = foo;
B.prototype.g = goo;

var o = new A();

function hej(x) {
  if (x == 0) return o.g(x, "h");
  if (x == 1) return o.g(x, "h");
  return o.g(x, "z");
}

function opt() {
  for (var k=0; k<2; k++) {
    for (var i=0; i<5; i++) o.g(i, "g");
    for (var j=0; j<5; j++) hej(j);
  }
  %OptimizeFunctionOnNextCall(o.g);
  %OptimizeFunctionOnNextCall(hej);
}

opt();
assertArrayEquals([0, "g"], o.g(0, "g"));
assertArrayEquals([1, "f"], o.g(1, "g"));
assertArrayEquals([0, "h"], hej(0));
assertArrayEquals([1, "f"], hej(1));

o = new B();

opt();
assertArrayEquals([0, "f"], o.g(0, "g"));
assertArrayEquals([1, "g"], o.g(1, "g"));
assertArrayEquals([0, "f"], hej(0));
assertArrayEquals([1, "h"], hej(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-arrayliteral.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-arrayliteral.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-arrayliteral.js)  
```javascript
var G = 41;
var H = 42;
function f() { var v = [G,H]; return v[1]; }
assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-arguments.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-arguments.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-arguments.js)  
```javascript
function f() { return this.foo; }

function g() { return f.apply(null, arguments); }
function h() { return f.apply(void 0, arguments); }

var foo = 42;

for (var i = 0; i < 3; i++) assertEquals(42, g());
%OptimizeFunctionOnNextCall(g);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, g());

for (var i = 0; i < 3; i++) assertEquals(42, h());
%OptimizeFunctionOnNextCall(h);
%OptimizeFunctionOnNextCall(f);
assertEquals(42, h());

var G1 = 21;
var G2 = 22;

function u() {
 var v = G1 + G2;
 return f.apply(v, arguments);
}

Number.prototype.foo = 42;
delete Number.prototype.foo;

for (var i = 0; i < 3; i++) assertEquals(void 0, u());
%OptimizeFunctionOnNextCall(u);
%OptimizeFunctionOnNextCall(f);
assertEquals(void 0, u());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3252443.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3252443.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3252443.js)  
```javascript
var document = new Object();
document.getElementById = function(s) { return { style: {}}};
function x(p0, p1, p2, p3) {
  document.getElementById(p1+p0).style.display='';
  document.getElementById(p1+''+p0).style.backgroundColor = "";
  document.getElementById(p1+''+p0).style.color="";
  document.getElementById(p1+''+p0).style.borderBottomColor = "";
  for (var i = p3; i <= p2; ++i) {
    if (i != p0) {
      document.getElementById(p1+i).style.display='';
      document.getElementById(p1+''+i).style.backgroundColor = "";
      document.getElementById(p1+''+i).style.color="";
      document.getElementById(p1+''+i).style.borderBottomColor = "";
    }
  }
}

x(1, "xxx", 10000, 1)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3249650.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3249650.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3249650.js)  
```javascript
function f0(x) { try { } catch (e) {}}
function f1(x) { try { } catch (e) {}}
function f2(x) { try { } catch (e) {}}
function f3(x) { try { } catch (e) {}}

var object = { a: "", b: false, c: {}};
object.f = function(x) { return this; }


function test(x) {
  f0(x);
  f1(x);
  f2(x);
  f3(x);
  x.a.b == "";
  object.f("A").b = true;
  object.f("B").a = "";
  object.f("C").c.display = "A";
  object.f("D").c.display = "A";
}

var x = {a: {b: "" }};
for (var i = 0; i < 20000; i++) test(x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3247124.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3247124.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3247124.js)  
```javascript
var foo = unescape("%E0%E2%EA%F4%FB%E3%F5%E1%E9%ED%F3%FA%E7%FC%C0%C2%CA%D4%DB%C3%D5%C1%C9%CD%D3%DA%C7%DC");

function bar(x) {
  var s = new String(x);
  var a = new String(foo);
  var b = new String('aaeouaoaeioucuAAEOUAOAEIOUCU');

  var i = new Number();
  var j = new Number();
  var c = new String();
  var r = '';

  for (i = 0; i < s.length; i++) {
    c = s.substring(i, i + 1);
    for (j = 0; j < a.length; j++) {
      if (a.substring(j, j + 1) == c) {
        c = b.substring(j, j + 1);
      }
    }
    r += c;
  }

  return r.toLowerCase();
}

for (var i = 0; i < 100; i++) bar(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3230771.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3230771.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3230771.js)  
```javascript
function f() {
  for (var h = typeof arguments[0] == "object" ? 0 : arguments; false; ) { }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3218915.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3218915.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3218915.js)  
```javascript
function withCommaExpressionInConditional(x) {
  if (x > 1000) { for (var i = 0; i < 10000; i++) { } }
  var y;
  if (y = x, y > 1) {
    return 'big';
  }
  return (y = x + 1, y > 1) ? 'medium' : 'small';
}

for (var i = 0; i < 5; i++) {
  withCommaExpressionInConditional(i);
}
%OptimizeFunctionOnNextCall(withCommaExpressionInConditional);
withCommaExpressionInConditional(i);
withCommaExpressionInConditional("1")  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3218915.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3218915.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3218915.js)  
```javascript
function side_effect() { try {} finally {} return "wrong"; }

function observe(x, y) { try {} finally {} return x; }

function test(x) { return observe(this, ((0, side_effect()), x + 1)); }

for (var i = 0; i < 5; ++i) test(0);
%OptimizeFunctionOnNextCall(test);
test(0);

assertFalse(test("a") === "wrong");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3218530.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3218530.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3218530.js)  
```javascript
var m = Math;
var p = "floor";

function test() {
  var bignumber = 31363200000;
  assertDoesNotThrow(assertEquals(m[p](Math.round(bignumber/864E5)/7)+1, 52));
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3199913.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3199913.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3199913.js)  
```javascript
var y = {
  'a' : function (x, y) { return 'called a(' + x + ', ' + y + ')' },
  'b' : function (x, y) { return 'called b(' + x + ', ' + y + ')' }
}

function C() {
}

C.prototype.f = function () {
  return y[(this.a == 1 ? "a" : "b")](0, 1);
}

obj = new C()
assertEquals('called b(0, 1)', obj.f())  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3185905.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3185905.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3185905.js)  
```javascript
function test1(x) {
  var a = arguments.callee;
  x = 1;
  x = 2;
  assertEquals(2, x);
}
test1(0)

function test2(x) {
  var a = arguments.callee;
  x++;
  x++;
  assertEquals(2, x);
}
test2(0)

function test3(x) {
  var a = arguments.callee;
  x += 1;
  x += 1;
  assertEquals(2, x);
}
test3(0)

function test4(x) {
  var arguments = { 0 : 3, 'x' : 4 };
  x += 1;
  x += 1;
  assertEquals(2, x);
  assertEquals(3, arguments[0])
  assertEquals(4, arguments['x'])
}
test4(0)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3185901.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3185901.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3185901.js)  
```javascript
var x;

function f() { if (g()) { } }
function g() { if (x) { return true; } }

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3136962.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3136962.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3136962.js)  
```javascript
var height = 267;

var count = 0;
function inner() { height = 0; ++count; }
function outer() {}

function test() {
  for (var i = 0; i < height; ++i) {
    for (var j = -6; j < 7; ++j) {
      if (i + j < 0 || i + j >= height) continue;
      for (var k = -6; k < 7; ++k) {
        inner();
      }
    }
    outer();
  }
}

test();

assertEquals(13, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3006390.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/regress/regress-3006390.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3006390.js)  
```javascript
function X() { }
X.prototype.valueOf = function () { return 7; }

function f(x, y) { return x % y; }

assertEquals(1, f(8, new X()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-8.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-8.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-8.js)  
```javascript
var gp = "";
var yE = "";
var W = "";
var LA = "";
var zE = "";
var Fp = "";
var AE = "";
var Gob = "";
var Hob = "";
var Iob = "";
var Job = "";
var Kob = "";
var Lob = "";
var Mob = "";
var p = "";
function O() { this.append = function(a,b,c,d,e) { return a + b + c + d + e; } }

function Nob(b,a) {
 var c;
 if (b==2) {
   c=new O;
   c.append(gp,
            yE,
            W,
            LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(AE+(LA+(a.total+Gob))))))))),
            p);
   c=c.toString();
 } else {
   if (b==1) {
     if(a.total>=2E6) {
       c=new O;
       c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Hob))))),p);
       c=c.toString();
     } else {
       if(a.total>=2E5) {
         c=new O;
         c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Iob))))),p);
         c=c.toString();
       } else {
         if(a.total>=2E4) {
           c=new O;
           c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Job))))),p);
           c=c.toString();
         } else {
           if(a.total>=2E3) {
             c=new O;
             c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Kob))))),p);
             c=c.toString();
           } else {
             if(a.total>=200) {
               c=new O;
               c.append(gp,yE,W,LA+(a.Un+(zE+(Fp+(LA+(a.Im+Lob))))),p);
               c=c.toString();
             } else {
               c=new O;
               c.append(gp,yE,W,
                        LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(Mob+(LA+(a.total+zE))))))))),
                        p);
               c=c.toString();
             }
             c=c;
           }
           c=c;
         }
         c=c;
       }
       c=c;
     }
     c=c;
   } else {
     c=new O;
     c.append(gp,yE,W,
              LA+(a.Un+(zE+(Fp+(LA+(a.Im+(zE+(AE+(LA+(a.total+zE))))))))),
              p);
     c=c.toString();
   }
   c=c;
 }
 return c;
}
Nob(2, { Un: "" , Im: "" , total: 42});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-7.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-7.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-7.js)  
```javascript
var G = 42;

function f() {
  var v = G;
  var w = v >> 0;
  return w;
}

for(var i=0; i<10000; i++) f();

assertEquals(G, f());
G = 2000000000;
assertEquals(G, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-6.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-6.js)  
```javascript
function f(a, b, c) {
  if (a == 0 || b == 0) return a;
  return a + c;
}

assertEquals(0, f(0, 0, 0));
assertEquals(0, f(0, 1, 0));
assertEquals(1, f(1, 0, 0));
assertEquals(2, f(2, 1, 0));

assertEquals(1.5, f(1, 1, 0.5));
assertEquals(2.5, f(2, 1, 0.5));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-5.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-5.js)  
```javascript
function f(y) {
  var x = 0;

  foo: {
    x++;
    bar: {
       if (y == 0) break bar; else break foo;
    }
    x++;
  }
  return x;
}

assertEquals(2, f(0));
assertEquals(1, f(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-4.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4.js)  
```javascript
function f(p) {
  var y=0;
  for (var x=0; x<10; x++) {
    if (x > 5) { y=y+p; break;}
  }
  return y+x;
}

for (var i=0; i<100000; i++) f(42);

var result = f("foo");
assertEquals("0foo6", result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-3.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3.js)  
```javascript
function fib(n) {
  var f0 = 0, f1 = 1;
  for (; n > 0; n = n -1) {
    var f2 = f0 + f1;
    f0 = f1; f1 = f2;
  }
  return f0;
}

assertEquals(2111485077978050, fib(75));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-2.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-2.js)  
```javascript
function TestCreateString(n)
{
  var l = n * 1;
  var r = 'r';
  while (r.length < n)
  {
    r = r + r;
  }
  return r;
}

assertEquals("r", TestCreateString(1));
assertEquals("rr", TestCreateString(2));
assertEquals("rrrr", TestCreateString(3));
assertEquals("rrrrrrrr", TestCreateString(6));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-1.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-1.js)  
```javascript
function DaysInYear(y) {
  if (y % 4 != 0) return 365;
  if (y % 4 == 0 && y % 100 != 0) return 366;
  if (y % 100 == 0 && y % 400 != 0) return 365;
  if (y % 400 == 0) return 366;
}
assertEquals(365, DaysInYear(1999));
assertEquals(366, DaysInYear(2000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-0.js (other issue)**  
   
**[Commit: Update V8 to version 3.0.](https://chromium.googlesource.com/v8/v8/+/e5860bd)**  
  
Date(Commit): Tue Dec 07 09:11:56 2010  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@5920](http://v8.googlecode.com/svn/branches/bleeding_edge@5920)  
Regress: [mjsunit/compiler/regress-0.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-0.js)  
```javascript
function TestNestedLoops() {
  var sum = 0;
  for (var i = 0; i < 200; i = i + 1) {
    for (var j = 0; j < 200; j = j + 1) {
      sum = sum + 1;
    }
  }
  return sum;
}
assertEquals(200 * 200, TestNestedLoops());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e5860bd^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=e5860bd)  
[include/v8-debug.h](https://cs.chromium.org/chromium/src/v8/include/v8-debug.h?cl=e5860bd)  
[include/v8-testing.h](https://cs.chromium.org/chromium/src/v8/include/v8-testing.h?cl=e5860bd)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=e5860bd)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=e5860bd)  
...  
  
  
---   

## **regress-944.js (v8 issue)**  
   
**[Issue: TimeComposer broken for times with milliseconds values that have only 1 or 2 digits](https://crbug.com/v8/944)**  
**[Commit: make DateParser::TimeComposer handle 1-2 digits millisecond values](https://chromium.googlesource.com/v8/v8/+/7be18f7)**  
  
Date(Commit): Fri Nov 26 11:48:35 2010  
Type: ----  
Code Review: [http://code.google.com/p/v8/issues/detail?id=944](http://code.google.com/p/v8/issues/detail?id=944)  
Regress: [mjsunit/regress/regress-944.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-944.js)  
```javascript
assertEquals(1290722550521, Date.parse("2010-11-25T22:02:30.521Z"));

assertEquals(1290722550500, Date.parse("2010-11-25T22:02:30.5Z"));
assertEquals(1290722550520, Date.parse("2010-11-25T22:02:30.52Z"));
assertFalse(Date.parse("2010-11-25T22:02:30.5Z") === Date.parse("2010-11-25T22:02:30.005Z"));

assertEquals(Date.parse("2010-11-25T22:02:30.1005Z"), Date.parse("2010-11-25T22:02:30.100Z"));

assertEquals(Date.parse("2010-11-25T22:02:30.999Z"), Date.parse("2010-11-25T22:02:30.99999999999999999999999999999999999999999999999999999999999999999999999999999999999999Z"));

assertTrue(isNaN(Date.parse("2010-11-25T22:02:30.Z")));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7be18f7^!)  
[src/dateparser-inl.h](https://cs.chromium.org/chromium/src/v8/src/dateparser-inl.h?cl=7be18f7)  
[src/dateparser.h](https://cs.chromium.org/chromium/src/v8/src/dateparser.h?cl=7be18f7)  
[test/mjsunit/regress/regress-944.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-944.js?cl=7be18f7)  
  

---   

## **regress-931.js (v8 issue)**  
   
**[Issue: Subexpression evaluation for calls is incorrect](https://crbug.com/v8/931)**  
**[Commit: Change the order of evaluation of sub-expressions for keyed call](https://chromium.googlesource.com/v8/v8/+/010f35f)**  
  
Date(Commit): Wed Nov 17 13:59:07 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5161002](http://codereview.chromium.org/5161002)  
Regress: [mjsunit/regress/regress-931.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-931.js)  
```javascript
var sequence = '';

var o = { f: function (x, y) { return x + y; },
          2: function (x, y) { return x - y} };

function first() { sequence += "1"; return o; }
function second() { sequence += "2"; return "f"; }
function third() { sequence += "3"; return 3; }
function fourth() { sequence += "4"; return 4; }

var result = (first()[second()](third(), fourth()))
assertEquals(7, result);
assertEquals("1234", sequence);

function second_prime() { sequence += "2'"; return 2; }

var result = (first()[second_prime()](third(), fourth()))
assertEquals(-1, result);
assertEquals("123412'34", sequence);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/010f35f^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=010f35f)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=010f35f)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=010f35f)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=010f35f)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=010f35f)  
...  
  

---   

## **regress-918.js (v8 issue)**  
   
**[Issue: Parser accepts parenthesized labels.](https://crbug.com/v8/918)**  
**[Commit: Fix bug in parser that allows "(foo):42" as a labeled statement.](https://chromium.googlesource.com/v8/v8/+/0464b33)**  
  
Date(Commit): Tue Nov 16 12:10:48 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/5044003](http://codereview.chromium.org/5044003)  
Regress: [mjsunit/regress/regress-918.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-918.js)  
```javascript
assertThrows("(label):42;");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0464b33^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=0464b33)  
[test/mjsunit/regress/regress-918.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-918.js?cl=0464b33)  
  

---   

## **regress-927.js (v8 issue)**  
   
**[Issue: V8 prints wrong result for this test case](https://crbug.com/v8/927)**  
**[Commit: Add check for overflow after MUL operations in side-effect free int32 expressions.](https://chromium.googlesource.com/v8/v8/+/20d3aad)**  
  
Date(Commit): Tue Nov 09 19:32:49 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/4746001](http://codereview.chromium.org/4746001)  
Regress: [mjsunit/regress/regress-927.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-927.js)  
```javascript
function a1() {
    var a2 = -1756315459;
    return ((((a2 & a2) ^ 1) * a2) << -10);
}

assertEquals(a1(), -2147483648);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/20d3aad^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=20d3aad)  
[test/mjsunit/regress/regress-927.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-927.js?cl=20d3aad)  
  

---   

## **regress-conditional-position.js (other issue)**  
   
**[Commit: Improve positions recording for calls.](https://chromium.googlesource.com/v8/v8/+/746d724)**  
  
Date(Commit): Thu Nov 04 15:12:03 2010  
Code Review: [http://codereview.chromium.org/4469002](http://codereview.chromium.org/4469002)  
Regress: [mjsunit/regress/regress-conditional-position.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-conditional-position.js)  
```javascript
var functionToCatch;
var lineNumber;

function catchLineNumber () {
  var x = {};

  Error.prepareStackTrace = function (error, stackTrace) {
    stackTrace.some(function (frame) {
      if (frame.getFunction() == functionToCatch) {
        lineNumber = frame.getLineNumber();
        return true;
      }
      return false;
    });
    return lineNumber;
  };

  Error.captureStackTrace(x);
  return x.stack;
}

function log() {
  catchLineNumber();
}

function foo() {}

function test1() {
  log(foo() == foo()
      ? 'a'
      : 'b');
}

function test2() {
  var o = { foo: function () {}}
  log(o.foo() == o.foo()
      ? 'a'
      : 'b');
}

function test3() {
  var o = { log: log, foo: function() { } };
  o.log(o.foo() == o.foo()
      ? 'a'
      : 'b');

}

function test(f, expectedLineNumber) {
  functionToCatch = f;
  f();

  assertEquals(expectedLineNumber, lineNumber);
}

test(test1, 58);
test(test2, 65);
test(test3, 72);

eval(test1.toString() + "//@ sourceUrl=foo");
eval(test2.toString() + "//@ sourceUrl=foo");
eval(test3.toString() + "//@ sourceUrl=foo");

test(test1, 2);
test(test2, 3);
test(test3, 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/746d724^!)  
[src/arm/assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.cc?cl=746d724)  
[src/arm/assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/assembler-arm.h?cl=746d724)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=746d724)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=746d724)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=746d724)  
...  
  
  
---   

## **regress-create-exception.js (other issue)**  
   
**[Commit: Fix creation of an exception to avoid rare GC corner case.](https://chromium.googlesource.com/v8/v8/+/d22965c)**  
  
Date(Commit): Fri Oct 15 07:54:20 2010  
Code Review: [http://codereview.chromium.org/3782009](http://codereview.chromium.org/3782009)  
Regress: [mjsunit/regress/regress-create-exception.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-create-exception.js)  
```javascript
"use strict";

var v = [1, 2, 3, 4]

Object.preventExtensions(v);

function foo() {
  var re = /2147483647/;  // Equal to 0x7fffffff.
  for  (var i = 0; i < 10000; i++) {
    var ok = false;
    try {
      var j = 1;
      // Allocate some heap numbers in order to randomize the behaviour of the
      // garbage collector.  93 is chosen to be a prime number to avoid the
      // allocation settling into a too neat pattern.
      for (var j = 0; j < i % 93; j++) {
        j *= 1.123567;  // An arbitrary floating point number.
      }
      v[0x7fffffff] = 0;  // Trigger exception.
      assertTrue(false);
      return j;  // Make sure that future optimizations don't eliminate j.
    } catch(e) {
      ok = true;
      assertTrue(re.test(e), 'e: ' + e);
    }
    assertTrue(ok);
  }
}

foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d22965c^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=d22965c)  
[test/mjsunit/regress/regress-create-exception.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-create-exception.js?cl=d22965c)  
  
  
---   

## **regress-58740.js (chromium issue)**  
   
**[Issue: Regular expression, exec and related method(test) -- cause strange behavior](https://crbug.com/58740)**  
**[Commit: Fix bug in cache handling of lastIndex on global regexps.](https://chromium.googlesource.com/v8/v8/+/6c0cde6)**  
  
Date(Commit): Thu Oct 14 08:51:20 2010  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/3745005](http://codereview.chromium.org/3745005)  
Regress: [mjsunit/regress/regress-58740.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-58740.js)  
```javascript
var re = /.+/g;
re.exec("");
re.exec("anystring");
re=/.+/g;
re.exec("");
assertEquals(0, re.lastIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6c0cde6^!)  
[src/regexp.js](https://cs.chromium.org/chromium/src/v8/src/regexp.js?cl=6c0cde6)  
[test/mjsunit/regress/regress-58740.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-58740.js?cl=6c0cde6)  
  

---   

## **regress-874.js (v8 issue)**  
   
**[Issue: Crash when applying getOwnPropertyDescriptor to array index with getter](https://crbug.com/v8/874)**  
**[Commit: Fix getOwnPropertyDescriptor() support for index properties.](https://chromium.googlesource.com/v8/v8/+/622351f)**  
  
Date(Commit): Thu Sep 23 11:25:01 2010  
Type: Bug  
Code Review: [http://code.google.com/p/v8/issues/detail?id=877](http://code.google.com/p/v8/issues/detail?id=877)  
Regress: [mjsunit/regress/regress-874.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-874.js)  
```javascript
var x = { };

var getter = function(){ return 42; };
var setter = function(value){ };
x.__defineGetter__(0, getter);
x.__defineSetter__(0, setter);

assertEquals (undefined, Object.getOwnPropertyDescriptor(x, 0).value);
assertEquals (getter, Object.getOwnPropertyDescriptor(x, 0).get);
assertEquals (setter, Object.getOwnPropertyDescriptor(x, 0).set);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/622351f^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=622351f)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=622351f)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=622351f)  
[test/cctest/test-api.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-api.cc?cl=622351f)  
[test/mjsunit/regress/regress-874.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-874.js?cl=622351f)  
  

---   

## **regress-52801.js (chromium issue)**  
   
**[Issue: Regular expression problem  -- causes infinite loop](https://crbug.com/52801)**  
**[Commit: RegExp: Fix caching to correctly set lastIndex.](https://chromium.googlesource.com/v8/v8/+/0dece53)**  
  
Date(Commit): Wed Sep 22 11:22:57 2010  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/3389022](http://codereview.chromium.org/3389022)  
Regress: [mjsunit/regress/regress-52801.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-52801.js)  
```javascript
var re = /a/g;

var str = "bbbbabbbbabbbb";


re.test(str);
assertEquals(5, re.lastIndex);

re.lastIndex = 0;
re.test(str);
assertEquals(5, re.lastIndex);  // Fails if caching.

re.lastIndex = 0;
re.test(str);
assertEquals(5, re.lastIndex);  // Fails if caching.


re = /a/g;

re.exec(str);
assertEquals(5, re.lastIndex);

re.lastIndex = 0;
re.exec(str);
assertEquals(5, re.lastIndex);  // Fails if caching.

re.lastIndex = 0;
re.exec(str);
assertEquals(5, re.lastIndex);  // Fails if caching.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0dece53^!)  
[src/regexp.js](https://cs.chromium.org/chromium/src/v8/src/regexp.js?cl=0dece53)  
[test/mjsunit/regress/regress-52801.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-52801.js?cl=0dece53)  
  

---   

## **regress-857.js (v8 issue)**  
   
**[Issue: Date does not support W3C format](https://crbug.com/v8/857)**  
**[Commit: make Date.parse properly handle TZ offsets](https://chromium.googlesource.com/v8/v8/+/ac2ae05)**  
  
Date(Commit): Fri Sep 10 07:00:28 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/3318017](http://codereview.chromium.org/3318017)  
Regress: [mjsunit/regress/regress-857.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-857.js)  
```javascript
assertEquals(1283326536000, Date.parse("2010-08-31T22:35:36-09:00"));
assertEquals(1283261736000, Date.parse("2010-08-31T22:35:36+09:00"));
assertEquals(1283326536000, Date.parse("2010-08-31T22:35:36.0-09:00"));
assertEquals(1283261736000, Date.parse("2010-08-31T22:35:36.0+09:00"));
assertEquals(1283326536000, Date.parse("2010-08-31T22:35:36-0900"));
assertEquals(1283261736000, Date.parse("2010-08-31T22:35:36+0900"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac2ae05^!)  
[src/dateparser-inl.h](https://cs.chromium.org/chromium/src/v8/src/dateparser-inl.h?cl=ac2ae05)  
[test/mjsunit/regress/regress-857.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-857.js?cl=ac2ae05)  
  

---   

## **regress-push-args-twice.js (other issue)**  
   
**[Commit: Avoid pushing arguments twice in GenericBinaryOpStub.](https://chromium.googlesource.com/v8/v8/+/73c0239)**  
  
Date(Commit): Tue Sep 07 13:33:40 2010  
Code Review: [http://codereview.chromium.org/3290012](http://codereview.chromium.org/3290012)  
Regress: [mjsunit/regress/regress-push-args-twice.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-push-args-twice.js)  
```javascript
try {
  for (var key = 0; key != 10; key++) {
    var x = 1 + undefined;
  }
} catch(e) {
  fail("no exception", e);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/73c0239^!)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=73c0239)  
[test/mjsunit/regress/regress-push-args-twice.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-push-args-twice.js?cl=73c0239)  
  
  
---   

## **regress-851.js (v8 issue)**  
   
**[Issue: Object.freeze() might cause memory corruption.](https://crbug.com/v8/851)**  
**[Commit: Check result of JSObject::NormalizeElements() in JSObject::PreventExtensions().](https://chromium.googlesource.com/v8/v8/+/f059093)**  
  
Date(Commit): Fri Aug 27 13:06:50 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/3262001](http://codereview.chromium.org/3262001)  
Regress: [mjsunit/regress/regress-851.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-851.js)  
```javascript
var i = 0;
for (var i = 0; i < 10000; i++) {
  Object.freeze({});
  assertNull(JSON.stringify({x: null}).match(/\0/));
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f059093^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f059093)  
[test/mjsunit/regress/regress-851.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-851.js?cl=f059093)  
  

---   

## **regress-842.js (v8 issue)**  
   
**[Issue: Object.freeze() throws exception is Object or Array has been extended](https://crbug.com/v8/842)**  
**[Commit: Fixes bug in Object.freeze and Object.seal causing them to misbehave when Array.prototype has changed.](https://chromium.googlesource.com/v8/v8/+/7672338)**  
  
Date(Commit): Thu Aug 26 08:35:49 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/3137041](http://codereview.chromium.org/3137041)  
Regress: [mjsunit/regress/regress-842.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-842.js)  
```javascript
Array.prototype.myfunc = function() {};
Array.prototype[10] = 42;
Array.prototype.length = 3000;

var obj = { name: "n1" };

try {
  obj = Object.freeze(obj);
} catch (e) {
  assertUnreachable();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7672338^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=7672338)  
[test/mjsunit/regress/regress-842.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-842.js?cl=7672338)  
  

---   

## **regress-815.js (v8 issue)**  
   
**[Issue: ARM port fails assertion: !SpilledScope::is_spilled() in src/arm/virtual-frame-arm.h, line 168](https://crbug.com/v8/815)**  
**[Commit: ARM: Ensure that we are not in a spilled scope when calling](https://chromium.googlesource.com/v8/v8/+/e18d07b)**  
  
Date(Commit): Mon Aug 16 11:43:30 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/3125011](http://codereview.chromium.org/3125011)  
Regress: [mjsunit/regress/regress-815.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-815.js)  
```javascript
var o = new Object();

for (x in +o) { }

for (a[+o] in o) {}

try {
  o[+o](1,2,3)
} catch(e) {
  // It's OK as long as it does not hit an assert.
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e18d07b^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=e18d07b)  
[test/mjsunit/regress/regress-815.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-815.js?cl=e18d07b)  
  

---   

## **regress-798.js (v8 issue)**  
   
**[Issue: Reading Error.stack in a __defineGetter__ callback causes inf loop or tab crash](https://crbug.com/v8/798)**  
**[Commit: Handle accessors when generating Error.stack](https://chromium.googlesource.com/v8/v8/+/56e0221)**  
  
Date(Commit): Fri Aug 13 08:31:52 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/3082012](http://codereview.chromium.org/3082012)  
Regress: [mjsunit/regress/regress-798.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-798.js)  
```javascript
var x = {};

x.__defineGetter__("a", function() {
  try {
    y.x = 40;
  } catch (e) {
    assertEquals(3, e.stack.split('\n').length);
  }
  return 40;
});

x.__defineSetter__("a", function(val) {
  try {
    y.x = 40;
  } catch(e) {
    assertEquals(3, e.stack.split('\n').length);
  }
});

function getB() {
  try {
    y.x = 30;
  } catch (e) {
    assertEquals(3, e.stack.split('\n').length);
  }
  return 30;
}

function setB(val) {
  try {
    y.x = 30;
  } catch(e) {
    assertEquals(3, e.stack.split('\n').length);
  }
}

x.__defineGetter__("b", getB);
x.__defineSetter__("b", setB);

var descriptor  = {
  get: function() {
    try {
      y.x = 40;
    } catch (e) {
      assertEquals(3, e.stack.split('\n').length);
    }
    return 40;
  },
  set: function(val) {
    try {
      y.x = 40;
    } catch(e) {
      assertEquals(3, e.stack.split('\n').length);
    }
  }
}

Object.defineProperty(x, 'c', descriptor)

x.a;
x.b;
x.c;
x.a = 1;
x.b = 1;
x.c = 1;

xx = {}
xx.__proto__ = x

xx.a;
xx.b;
xx.c;
xx.a = 1;
xx.b = 1;
xx.c = 1;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/56e0221^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=56e0221)  
[test/mjsunit/regress/regress-798.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-798.js?cl=56e0221)  
  

---   

## **regress-760-1.js (v8 issue)**  
   
**[Issue: [[DefaultValue]] discrepancy](https://crbug.com/v8/760)**  
**[Commit: Handle overwriting valueOf on String objects correctly when adding](https://chromium.googlesource.com/v8/v8/+/8e0cd6d)**  
  
Date(Commit): Thu Aug 12 13:43:08 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/3117006](http://codereview.chromium.org/3117006)  
Regress: [mjsunit/regress/regress-760-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-760-1.js), [mjsunit/regress/regress-760-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-760-2.js)  
```javascript
String.prototype.valueOf = function() { return 'y' };

function test() {
  var o = Object('x');
  assertEquals('y', o + '');
  assertEquals('y', '' + o);
}

for (var i = 0; i < 10; i++) {
  var o = Object('x');
  assertEquals('y', o + '');
  assertEquals('y', '' + o);
}

for (var i = 0; i < 10; i++) {
  test()
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8e0cd6d^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=8e0cd6d)  
[src/arm/codegen-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.h?cl=8e0cd6d)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=8e0cd6d)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=8e0cd6d)  
[src/arm/macro-assembler-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.h?cl=8e0cd6d)  
...  
  

---   

## **regress-806.js (v8 issue)**  
   
**[Issue: 64bit os,  v8 crash (unknow reason)](https://crbug.com/v8/806)**  
**[Commit: Fix issue 806.](https://chromium.googlesource.com/v8/v8/+/4a2f05c)**  
  
Date(Commit): Mon Aug 02 09:14:44 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/3081007](http://codereview.chromium.org/3081007)  
Regress: [mjsunit/regress/regress-806.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-806.js)  
```javascript
function foo(a) {
  for (var o = 1; o < 2; o++) {
    for (var n = 1; n < 2; n++) {
      for (var m = 1; m < 2; m++) {
        for (var l = 1; l < 2; l++) {
          for (var i = 1; i < 2; i++) {
            for (var j = 1; j < 2; j++) {
              for (var k = 1; k < 2; k++) {
                var z = a.foo;
                z.foo = i * j * k * m * n * o;
              }
            }
          }
        }
      }
    }
  }
}

foo({foo: {foo: 1}});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4a2f05c^!)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=4a2f05c)  
[test/mjsunit/regress/regress-806.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-806.js?cl=4a2f05c)  
  

---   

## **regress-784.js (v8 issue)**  
   
**[Issue: arm v8 throws an exception for now reason when starting node.js](https://crbug.com/v8/784)**  
**[Commit: Fix error in optimized x.apply(y, arguments) code generation on ARM.  Fixes issue 784.  Adds regression test.](https://chromium.googlesource.com/v8/v8/+/3607a9e)**  
  
Date(Commit): Wed Jul 28 12:50:27 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/3048035](http://codereview.chromium.org/3048035)  
Regress: [mjsunit/regress/regress-784.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-784.js)  
```javascript
A = {x:{y:function(i){return i;}}};
B = function(x){return 17;};

foo = function () {
  A.x.y(B.apply(this, arguments));
};

foo();
foo("Hello", "There");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3607a9e^!)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=3607a9e)  
[test/mjsunit/regress/regress-784.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-784.js?cl=3607a9e)  
  

---   

## **regress-r4998.js (other issue)**  
   
**[Commit: Fix error in x64 fast smi loops, change 4998.](https://chromium.googlesource.com/v8/v8/+/cb1eedd)**  
  
Date(Commit): Wed Jul 14 13:22:47 2010  
Code Review: [http://codereview.chromium.org/2925012](http://codereview.chromium.org/2925012)  
Regress: [mjsunit/regress/regress-r4998.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-r4998.js)  
```javascript
function foo() {
  return;
}

function bar() {
  var x1 = 3;
  var x2 = 3;
  var x3 = 3;
  var x4 = 3;
  var x5 = 3;
  var x6 = 3;
  var x7 = 3;
  var x8 = 3;
  var x9 = 3;
  var x10 = 3;
  var x11 = 3;
  var x12 = 3;
  var x13 = 3;

  foo();

  x1 = 257;
  x2 = 258;
  x3 = 259;
  x4 = 260;
  x5 = 261;
  x6 = 262;
  x7 = 263;
  x8 = 264;
  x9 = 265;
  x10 = 266;
  x11 = 267;
  x12 = 268;
  x13 = 269;

  // The loop variable x7 is initialized to 3,
  // and then MakeMergeable is called on the virtual frame.
  // MakeMergeable has forced the loop variable x7 to be spilled,
  // so it is marked as synced
  // The back edge then merges its virtual frame, which incorrectly
  // claims that x7 is synced, and does not save the modified
  // value.
  for (x7 = 3; x7 < 10; ++x7) {
    foo();
  }
}

bar();

function aliasing() {
  var x = 3;
  var j;
  for (j = 7; j < 11; ++j) {
    x = j;
  }

  assertEquals(10, x);
  assertEquals(11, j);
}

aliasing();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cb1eedd^!)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=cb1eedd)  
[test/mjsunit/regress/regress-r4998.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-r4998.js?cl=cb1eedd)  
  
  
---   

## **regress-753.js (v8 issue)**  
   
**[Issue: JSON.stringify does not truncate the space parameter](https://crbug.com/v8/753)**  
**[Commit: Update JSON.stringify to floor the space parameter (fixes issue 753).](https://chromium.googlesource.com/v8/v8/+/eff34b9)**  
  
Date(Commit): Tue Jun 29 07:22:40 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2877004](http://codereview.chromium.org/2877004)  
Regress: [mjsunit/regress/regress-753.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-753.js)  
```javascript
var obj = {a1: {b1: [1,2,3,4], b2: {c1: 1, c2: 2}},a2: 'a2'};
assertEquals(JSON.stringify(obj, null, 5.99999), JSON.stringify(obj, null, 5));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eff34b9^!)  
[src/json.js](https://cs.chromium.org/chromium/src/v8/src/json.js?cl=eff34b9)  
[test/es5conform/es5conform.status](https://cs.chromium.org/chromium/src/v8/test/es5conform/es5conform.status?cl=eff34b9)  
[test/mjsunit/regress/regress-753.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-753.js?cl=eff34b9)  
  

---   

## **regress-754.js (v8 issue)**  
   
**[Issue: Array.lastIndexOf does not correctly handle null and undefined values as the fromIndex argument](https://crbug.com/v8/754)**  
**[Commit: Fixes bug in Array.prototype.lastIndexOf when called with null or undefined as fromIndex argument. (fixes issue 754).](https://chromium.googlesource.com/v8/v8/+/faaf524)**  
  
Date(Commit): Fri Jun 25 09:28:38 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2840021](http://codereview.chromium.org/2840021)  
Regress: [mjsunit/regress/regress-754.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-754.js)  
```javascript
var a = new Array(1,2,1);
assertEquals(1, a.lastIndexOf(2));
assertEquals(2, a.lastIndexOf(1));
assertEquals(0, a.lastIndexOf(1, undefined));
assertEquals(0, a.lastIndexOf(1, null));
assertEquals(-1, a.lastIndexOf(2, undefined));
assertEquals(-1, a.lastIndexOf(2, null));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/faaf524^!)  
[src/array.js](https://cs.chromium.org/chromium/src/v8/src/array.js?cl=faaf524)  
[test/mjsunit/regress/regress-754.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-754.js?cl=faaf524)  
  

---   

## **regress-752.js (v8 issue)**  
   
**[Issue: JSON.stringify returns wrong result with a function given as the replacer argument](https://crbug.com/v8/752)**  
**[Commit: Fix bug in JSON.stringify where Boolean objects are incorrectly unwrapped.](https://chromium.googlesource.com/v8/v8/+/b71fe5b)**  
  
Date(Commit): Fri Jun 25 07:45:52 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2845023](http://codereview.chromium.org/2845023)  
Regress: [mjsunit/regress/regress-752.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-752.js)  
```javascript
function replacer(key, value) {
  return value === 42 ? new Boolean(false) : value;
}

assertEquals("[false]", JSON.stringify([42], replacer));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b71fe5b^!)  
[src/json.js](https://cs.chromium.org/chromium/src/v8/src/json.js?cl=b71fe5b)  
[test/mjsunit/regress/regress-752.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-752.js?cl=b71fe5b)  
  

---   

## **regress-45469.js (chromium issue)**  
   
**[Issue: Date.localeFormat doesn't return a value](https://crbug.com/45469)**  
**[Commit: Fix bug in regexp exec with global regexps.](https://chromium.googlesource.com/v8/v8/+/7b46a1f)**  
  
Date(Commit): Fri Jun 25 07:00:29 2010  
Components/Type: Blink/Bug-Regression  
Labels: ["Restrict-AddIssueComment-EditIssue", "M-6", "bulkmove"]  
Code Review: [http://codereview.chromium.org/2826020](http://codereview.chromium.org/2826020)  
Regress: [mjsunit/regress/regress-45469.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-45469.js)  
```javascript
var re = /x/g;

for (var i = 0; i < 15; i++) {
  assertEquals(i % 3, re.lastIndex, "preindex" + i);
  var res = re.exec("xx");
  assertEquals(i % 3 == 2 ? null : ["x"], res, "res" + i);
}

re = /x/g;

for (var i = 0; i < 15; i++) {
  assertEquals(i % 3, re.lastIndex, "testpreindex" + i);
  var res = re.test("xx");
  assertEquals(i % 3 != 2, res, "testres" + i);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7b46a1f^!)  
[src/regexp.js](https://cs.chromium.org/chromium/src/v8/src/regexp.js?cl=7b46a1f)  
[test/mjsunit/regress/regress-45469.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-45469.js?cl=7b46a1f)  
  

---   

## **regress-747.js (v8 issue)**  
   
**[Issue: Code flushing during gc will flush code that has heap allocated locals](https://crbug.com/v8/747)**  
**[Commit: Add regression test for the code flushing in issue 474 (which was](https://chromium.googlesource.com/v8/v8/+/be531ac)**  
  
Date(Commit): Wed Jun 23 08:02:06 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2829020](http://codereview.chromium.org/2829020)  
Regress: [mjsunit/regress/regress-747.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-747.js)  
```javascript
(function() {
  var x = 42;
  this.callEval = function() {eval('x');};
})();

try {
  callEval();
} catch (e) {
  assertUnreachable();
}

gc();
gc();
gc();
gc();
gc();
gc();

try {
  callEval();
} catch (e) {
  assertUnreachable();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/be531ac^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=be531ac)  
[test/mjsunit/regress/regress-747.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-747.js?cl=be531ac)  
  

---   

## **regress-732.js (v8 issue)**  
   
**[Issue: Using large numeric strings as hash keys is broken](https://crbug.com/v8/732)**  
**[Commit: Add regression tests for issues 728, 732](https://chromium.googlesource.com/v8/v8/+/1d932dc)**  
  
Date(Commit): Mon Jun 07 10:54:42 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/2698004](http://codereview.chromium.org/2698004)  
Regress: [mjsunit/regress/regress-732.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-732.js)  
```javascript
var idx = 10000000;

var obj = { };
for (var i = 0; i < 100000; i += 100) { obj[i] = "obj" + i; }

obj[idx] = "obj" + idx;

var str = "" + idx;

for (var i = 0; i < 10; i++) { ({})[str]; }

assertEquals(obj[str], obj[idx])  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d932dc^!)  
[test/mjsunit/regress/regress-728.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-728.js?cl=1d932dc)  
[test/mjsunit/regress/regress-732.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-732.js?cl=1d932dc)  
  

---   

## **regress-728.js (v8 issue)**  
   
**[Issue: Incorrect marking of some strings as array indices.](https://crbug.com/v8/728)**  
**[Commit: Add regression tests for issues 728, 732](https://chromium.googlesource.com/v8/v8/+/1d932dc)**  
  
Date(Commit): Mon Jun 07 10:54:42 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2698004](http://codereview.chromium.org/2698004)  
Regress: [mjsunit/regress/regress-728.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-728.js)  
```javascript
var obj = { 0: "obj0" };

var k = 16777217;
var h = "" + k;

obj[k] = "obj" + k;

for (var i = 0; i < 10; i++) { ({})[h]; }

function get(idx) { return obj[idx]; }

assertEquals(get(0), "obj0");
assertEquals(get(h), "obj" + h);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1d932dc^!)  
[test/mjsunit/regress/regress-728.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-728.js?cl=1d932dc)  
[test/mjsunit/regress/regress-732.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-732.js?cl=1d932dc)  
  

---   

## **regress-720.js (v8 issue)**  
   
**[Issue: Object.defineProperty overwrites existing writable flag if not provided by descriptor](https://crbug.com/v8/720)**  
**[Commit: Fix issue 720 making Object.defineProperty handle existing writable flags correctly.](https://chromium.googlesource.com/v8/v8/+/95939ad)**  
  
Date(Commit): Wed May 26 08:31:57 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2271001](http://codereview.chromium.org/2271001)  
Regress: [mjsunit/regress/regress-720.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-720.js)  
```javascript
var o = {x: 10};
Object.defineProperty(o, "x", {value: 5});
var desc = Object.getOwnPropertyDescriptor(o, "x");
assertTrue(desc["writable"]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/95939ad^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=95939ad)  
[test/mjsunit/object-define-property.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-define-property.js?cl=95939ad)  
[test/mjsunit/regress/regress-720.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-720.js?cl=95939ad)  
  

---   

## **regress-712.js (v8 issue)**  
   
**[Issue: Object.defineProperty allows overriding an accessor with an empty descriptor](https://crbug.com/v8/712)**  
**[Commit: Fixes issue 712 causing non-configurable accessors to be overwritable by using](https://chromium.googlesource.com/v8/v8/+/fb58bc0)**  
  
Date(Commit): Tue May 25 06:25:27 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2131019](http://codereview.chromium.org/2131019)  
Regress: [mjsunit/regress/regress-712.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-712.js)  
```javascript
var obj = {};
Object.defineProperty(obj, "x", { get: function() { return "42"; },
                                  configurable: false });
assertEquals(obj.x, "42");
Object.defineProperty(obj, 'x', {});
assertEquals(obj.x, "42");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb58bc0^!)  
[src/runtime.js](https://cs.chromium.org/chromium/src/v8/src/runtime.js?cl=fb58bc0)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=fb58bc0)  
[test/mjsunit/object-define-property.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/object-define-property.js?cl=fb58bc0)  
[test/mjsunit/regress/regress-712.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-712.js?cl=fb58bc0)  
  

---   

## **regress-697.js (v8 issue)**  
   
**[Issue: Object.create does not work when given a function as proto](https://crbug.com/v8/697)**  
**[Commit: Fixed issue 619 allowing Object.create to be called with a function.](https://chromium.googlesource.com/v8/v8/+/8d51195)**  
  
Date(Commit): Sun May 09 08:43:59 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/2051001](http://codereview.chromium.org/2051001)  
Regress: [mjsunit/regress/regress-697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-697.js)  
```javascript
try {
  Object.create(function(){});
} catch (e) {
  assertTrue(false);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8d51195^!)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=8d51195)  
[test/mjsunit/regress/regress-697.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-697.js?cl=8d51195)  
  

---   

## **regress-696.js (v8 issue)**  
   
**[Issue: Date.parse() does not return NaN for invalid date strings](https://crbug.com/v8/696)**  
**[Commit: Correct issue 696 with Date.parse returning a value when called on a non date string.](https://chromium.googlesource.com/v8/v8/+/fb3e01a)**  
  
Date(Commit): Fri May 07 11:53:20 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/2017005](http://codereview.chromium.org/2017005)  
Regress: [mjsunit/regress/regress-696.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-696.js)  
```javascript
assertTrue(isNaN(Date.parse('x')));
assertTrue(isNaN(Date.parse('1x')));
assertTrue(isNaN(Date.parse('xT10:00:00')));
assertTrue(isNaN(Date.parse('This is a relatively long string')));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb3e01a^!)  
[src/dateparser.cc](https://cs.chromium.org/chromium/src/v8/src/dateparser.cc?cl=fb3e01a)  
[test/mjsunit/regress/regress-696.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-696.js?cl=fb3e01a)  
  

---   

## **regress-crbug-40931.js (chromium issue)**  
   
**[Issue: Javascript split method of string object returns array with some additional strange keys](https://crbug.com/40931)**  
**[Commit: Added regression test for crbug 40931 http://crbug.com/40931](https://chromium.googlesource.com/v8/v8/+/f066a9a)**  
  
Date(Commit): Mon Apr 26 13:26:11 2010  
Components/Type: None/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://crbug.com/40931](http://crbug.com/40931)  
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
  

---   

## **regress-681.js (v8 issue)**  
   
**[Issue: Crash introduced in r4426](https://crbug.com/v8/681)**  
**[Commit: Add missing smi check in IC for nonexistent properties.](https://chromium.googlesource.com/v8/v8/+/c678e44)**  
  
Date(Commit): Tue Apr 20 10:20:39 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/1673005](http://codereview.chromium.org/1673005)  
Regress: [mjsunit/regress/regress-681.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-681.js)  
```javascript
var x = {};
function f() { return x.y; }

f();
f();

x = 23;

assertEquals(undefined, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c678e44^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=c678e44)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=c678e44)  
[src/x64/stub-cache-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/stub-cache-x64.cc?cl=c678e44)  
[test/mjsunit/regress/regress-681.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-681.js?cl=c678e44)  
  

---   

## **regress-675.js (v8 issue)**  
   
**[Issue: ICs for nonexistent properties fail for global properties](https://crbug.com/v8/675)**  
**[Commit: Reapply load ICs for nonexistent properties.](https://chromium.googlesource.com/v8/v8/+/afc15bb)**  
  
Date(Commit): Thu Apr 15 11:25:41 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/1559033](http://codereview.chromium.org/1559033)  
Regress: [mjsunit/regress/regress-675.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-675.js)  
```javascript
function f() { return this.x; }

f();
f();

this.x = 23;

assertEquals(23, f());


this.__proto__ = null;
function g() { return this.y; }

g();
g();

this.y = 42;

assertEquals(42, g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afc15bb^!)  
[src/arm/stub-cache-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/stub-cache-arm.cc?cl=afc15bb)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=afc15bb)  
[src/ia32/stub-cache-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/stub-cache-ia32.cc?cl=afc15bb)  
[src/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic.cc?cl=afc15bb)  
[src/stub-cache.cc](https://cs.chromium.org/chromium/src/v8/src/stub-cache.cc?cl=afc15bb)  
...  
  

---   

## **regress-crbug-39160.js (chromium issue)**  
   
**[Issue: Reliability failure in V8 generated code](https://crbug.com/39160)**  
**[Commit: Re-apply "Inline floating point compare"](https://chromium.googlesource.com/v8/v8/+/6a63910)**  
  
Date(Commit): Thu Mar 25 12:04:34 2010  
Components/Type: None/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/1251009](http://codereview.chromium.org/1251009)  
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
...  
  

---   

## **regress-646.js (v8 issue)**  
   
**[Issue: __proto__ "problem"](https://crbug.com/v8/646)**  
**[Commit: Don't generate inline constructors if this.__proto__ is assigned.](https://chromium.googlesource.com/v8/v8/+/1963ffb)**  
  
Date(Commit): Wed Mar 17 13:23:53 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/1023008](http://codereview.chromium.org/1023008)  
Regress: [mjsunit/regress/regress-646.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-646.js)  
```javascript
function f() { this.__proto__ = 42 }
var count = 0;
for (var x in new f()) count++;
assertEquals(0, count);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1963ffb^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=1963ffb)  
[test/mjsunit/regress/regress-646.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-646.js?cl=1963ffb)  
  

---   

## **regress-643.js (v8 issue)**  
   
**[Issue: Bug in assigned variables analysis.](https://crbug.com/v8/643)**  
**[Commit: Fix bug in assigned variables analysis.](https://chromium.googlesource.com/v8/v8/+/d090867)**  
  
Date(Commit): Fri Mar 12 13:12:08 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/903003](http://codereview.chromium.org/903003)  
Regress: [mjsunit/regress/regress-643.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-643.js)  
```javascript
function f() {
  var test = {x:1};
  var a = test;
  a.x = a = 42;
  return test.x;
}

assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d090867^!)  
[src/data-flow.cc](https://cs.chromium.org/chromium/src/v8/src/data-flow.cc?cl=d090867)  
[test/mjsunit/regress/regress-643.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-643.js?cl=d090867)  
  

---   

## **regress-crbug-37853.js (chromium issue)**  
   
**[Issue: CHECK fail when editing a sites page](https://crbug.com/37853)**  
**[Commit: Fix code cache lookup for keyed IC's](https://chromium.googlesource.com/v8/v8/+/b0c9738)**  
  
Date(Commit): Thu Mar 11 08:52:31 2010  
Components/Type: Blink/Bug  
Labels: ["Restrict-AddIssueComment-Commit"]  
Code Review: [http://codereview.chromium.org/872001](http://codereview.chromium.org/872001)  
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
  

---   

## **regress-636.js (v8 issue)**  
   
**[Issue: String handling broken (test case)](https://crbug.com/v8/636)**  
**[Commit: Correct handling of adding a string and a smal integer](https://chromium.googlesource.com/v8/v8/+/800b6df)**  
  
Date(Commit): Tue Mar 09 09:40:35 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/720001](http://codereview.chromium.org/720001)  
Regress: [mjsunit/regress/regress-636.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-636.js)  
```javascript
function test() {
  var i, result = "";
  var value = parseFloat(5.5);
  value = Math.abs(1025);
  for(i = 12; --i; result = ( value % 2 ) + result, value >>= 1);
  return result;
};

assertEquals("10000000001", test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/800b6df^!)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=800b6df)  
[test/mjsunit/regress/regress-636.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-636.js?cl=800b6df)  
  

---   

## **regress-619.js (v8 issue)**  
   
**[Issue: Object.defineProperty does not behave correctly when trying to define properties on elements.](https://crbug.com/v8/619)**  
**[Commit: Added test for bug 619 - we should move this to object-define-property when the bug has been corrected.](https://chromium.googlesource.com/v8/v8/+/27eaf97)**  
  
Date(Commit): Fri Feb 19 13:27:43 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/651028](http://codereview.chromium.org/651028)  
Regress: [mjsunit/regress/regress-619.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-619.js)  
```javascript
var obj = {};
obj[1] = 42;
assertEquals(42, obj[1]);
Object.defineProperty(obj, '1', {value:10, writable:false});
assertEquals(10, obj[1]);

obj[1] = 5;
assertEquals(10, obj[1]);

for(var i = 0; i < 1024; i++) {
  obj[i] = 42;
}

for(var i = 0; i < 1024; i++) {
  Object.defineProperty(obj, i, {value: i, writable:false});
}

for(var i = 0; i < 1024; i++) {
  assertEquals(i, obj[i]);
}

for(var i = 0; i < 1024; i++) {
  obj[1] = 5;
}

for(var i = 0; i < 1024; i++) {
  assertEquals(i, obj[i]);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/27eaf97^!)  
[test/mjsunit/bugs/bug-619.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-619.js?cl=27eaf97)  
  

---   

## **regress-618.js (v8 issue)**  
   
**[Issue: Simple constructors does not handle adding setters](https://crbug.com/v8/618)**  
**[Commit: Add a test case for issue 618 Review URL: http://codereview.chromium.org/647014](https://chromium.googlesource.com/v8/v8/+/17e80e7)**  
  
Date(Commit): Thu Feb 18 13:01:58 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/647014](http://codereview.chromium.org/647014)  
Regress: [mjsunit/regress/regress-618.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-618.js)  
```javascript
function C1() {
  this.x = 23;
};
var c1 = new C1();
assertEquals(23, c1.x);
assertEquals("undefined", typeof c1.y);

C1.prototype = { set x(value) { this.y = 23; } };
var c1 = new C1();
assertEquals("undefined", typeof c1.x);
assertEquals(23, c1.y);

function C2() {
  this.x = 23;
};
var c2 = new C2();
assertEquals(23, c2.x);
assertEquals("undefined", typeof c2.y);

C2.prototype.__proto__ = { set x(value) { this.y = 23; } };
var c2 = new C2();
assertEquals("undefined", typeof c2.x);
assertEquals(23, c2.y);

function C3() {
  this.x = 23;
};
var c3 = new C3();
assertEquals(23, c3.x);
assertEquals("undefined", typeof c3.y);

C3.prototype.__defineSetter__('x', function(value) { this.y = 23; });
var c3 = new C3();
assertEquals("undefined", typeof c3.x);
assertEquals(23, c3.y);

function C4() {
  this.x = 23;
};
var c4 = new C4();
assertEquals(23, c4.x);
assertEquals("undefined", typeof c4.y);

C4.prototype.__proto__.__defineSetter__('x', function(value) { this.y = 23; });
var c4 = new C4();
assertEquals("undefined", typeof c4.x);
assertEquals(23, c4.y);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/17e80e7^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=17e80e7)  
[test/mjsunit/bugs/618.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/618.js?cl=17e80e7)  
  

---   

## **regress-603.js (v8 issue)**  
   
**[Issue: Function.prototype.call.call(/x/) messes up the stack?](https://crbug.com/v8/603)**  
**[Commit: Fix stack corruption when calling non-function.](https://chromium.googlesource.com/v8/v8/+/3c0d77f)**  
  
Date(Commit): Wed Feb 17 08:26:50 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/604064](http://codereview.chromium.org/604064)  
Regress: [mjsunit/regress/regress-603.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-603.js)  
```javascript
var re = /b../;
assertThrows(function() {
  return re('abcdefghijklm') + 'z';
});

var re1 = /c../;
re1.call = Function.prototype.call;
assertThrows(function() {
  re1.call(null, 'abcdefghijklm') + 'z';
});

var re2 = /d../;
assertThrows(function() {
  Function.prototype.call.call(re2, null, 'abcdefghijklm') + 'z';
});

var re3 = /e../;
assertThrows(function() {
  Function.prototype.call.apply(
      re3, [null, 'abcdefghijklm']) + 'z';
});

var re4 = /f../;
assertThrows(function() {
  Function.prototype.apply.call(
      re4, null, ['abcdefghijklm']) + 'z';
});

var re5 = /g../;
assertThrows(function() {
  Function.prototype.apply.apply(
      re4, [null, ['abcdefghijklm']]) + 'z';
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c0d77f^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=3c0d77f)  
[src/arm/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/codegen-arm.cc?cl=3c0d77f)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=3c0d77f)  
[src/ia32/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/codegen-ia32.cc?cl=3c0d77f)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=3c0d77f)  
...  
  

---   

## **regress-612.js (v8 issue)**  
   
**[Issue: Crash when calling runtime DefineAccessor](https://crbug.com/v8/612)**  
**[Commit: Normalize the object before updating getter/setter info.](https://chromium.googlesource.com/v8/v8/+/087fede)**  
  
Date(Commit): Wed Feb 17 06:53:19 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/608014](http://codereview.chromium.org/608014)  
Regress: [mjsunit/regress/regress-612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-612.js)  
```javascript
obj = {}

obj.__defineGetter__('foobar', function() { return 42; })

obj.a = 1
obj.b = 2;
obj.c = 3;

obj.__defineGetter__('foobar', function() { return 42; })  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/087fede^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=087fede)  
[test/mjsunit/regress/regress-612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-612.js?cl=087fede)  
  

---   

## **regress-crbug-3867.js (v8 issue)**  
   
**[Issue: super in ScriptBody : StatementList should be SyntaxError](https://crbug.com/v8/3867)**  
**[Commit: Handle insertion order for simple constructors](https://chromium.googlesource.com/v8/v8/+/1091039)**  
  
Date(Commit): Tue Feb 02 13:33:29 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/566016](http://codereview.chromium.org/566016)  
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
  

---   

## **regress-580.js (v8 issue)**  
   
**[Issue: x64 incorrectly folds numeric constants](https://crbug.com/v8/580)**  
**[Commit: Fix V8 issue 580: Arithmetic on some integer constants gives wrong anwers.](https://chromium.googlesource.com/v8/v8/+/04e9399)**  
  
Date(Commit): Wed Jan 20 17:01:34 2010  
Type: Bug  
Code Review: [http://codereview.chromium.org/545134](http://codereview.chromium.org/545134)  
Regress: [mjsunit/regress/regress-580.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-580.js)  
```javascript
function num_ops() {
  var x;
  var tmp = 0;
  x = (tmp = 1578221999, tmp)+(tmp = 572285336, tmp);
  assertEquals(2150507335, x, "++");
  x = 1578221999 + 572285336;
  assertEquals(2150507335, x);

  x = (tmp = -1500000000, tmp)+(tmp = -2000000000, tmp);
  assertEquals(-3500000000, x, "+-");
  x = -1500000000 + -2000000000;
  assertEquals(-3500000000, x);

  x = (tmp = 1578221999, tmp)-(tmp = -572285336, tmp);
  assertEquals(2150507335, x, "--");
  x = 1578221999 - -572285336;
  assertEquals(2150507335, x);

  x = (tmp = -1500000000, tmp)-(tmp = 2000000000, tmp);
  assertEquals(-3500000000, x, "-+");
  x = -1500000000 - 2000000000;
  assertEquals(-3500000000, x);
}

num_ops();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/04e9399^!)  
[src/x64/codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/codegen-x64.cc?cl=04e9399)  
[test/mjsunit/regress/regress-580.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-580.js?cl=04e9399)  
  

---   

## **regress-crbug-3184.js (v8 issue)**  
   
**[Issue: check fail when using --hydrogen_track_positions](https://crbug.com/v8/3184)**  
**[Commit: Ensure correct boxing of values when calling functions on them](https://chromium.googlesource.com/v8/v8/+/562f90d)**  
  
Date(Commit): Fri Jan 15 13:42:32 2010  
Type: ----  
Code Review: [http://codereview.chromium.org/542087](http://codereview.chromium.org/542087)  
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
...  
  

---   
