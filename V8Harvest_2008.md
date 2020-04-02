# V8Harvest  
The Harvest of V8 regress in 2008.  
  

## **regress-176.js (v8 issue)**  
   
**[Issue: Regexp: ? should be implemented as {0,1}](https://crbug.com/v8/176)**  
**[Commit: Added test for bug 176 (zero length matches should fail in quantifiers).](https://chromium.googlesource.com/v8/v8/+/4ede982)**  
  
Date(Commit): Thu Dec 11 09:01:55 2008  
Code Review: [http://codereview.chromium.org/13381](http://codereview.chromium.org/13381)  
Regress: [mjsunit/regress/regress-176.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-176.js)  
```javascript
assertArrayEquals(["f", undefined],
                  "foo".match(/(?:(?=(f)o))?f/),
                  "zero length match in (?:) with capture in lookahead");
assertArrayEquals(["f", undefined],
                  "foo".match(/(?=(f)o)?f/),
                  "zero length match in (?=) with capture in lookahead");
assertArrayEquals(["fo", "f"],
                  "foo".match(/(?:(?=(f)o)f)?o/),
                  "non-zero length match with capture in lookahead");
assertArrayEquals(["fo", "f"],
                  "foo".match(/(?:(?=(f)o)f?)?o/),
                  "non-zero length match with greedy ? in (?:)");
assertArrayEquals(["fo", "f"],
                  "foo".match(/(?:(?=(f)o)f??)?o/),
                  "non-zero length match with non-greedy ? in (?:), o forces backtrack");
assertArrayEquals(["fo", "f"],
                  "foo".match(/(?:(?=(f)o)f??)?./),
                  "non-zero length match with non-greedy ? in (?:), zero length match causes backtrack");
assertArrayEquals(["f", undefined],
                  "foo".match(/(?:(?=(f)o)fx)?./),
                  "x causes backtrack inside (?:)");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4ede982^!)  
[test/mjsunit/bugs/bug-176.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-176.js?cl=4ede982)  
  

---   

## **regress-149.js (v8 issue)**  
   
**[Issue: Some upper/lower case mappings are performed incorrectly](https://crbug.com/v8/149)**  
**[Commit: Merge regexp2000 back into bleeding_edge](https://chromium.googlesource.com/v8/v8/+/b57b4a1)**  
  
Date(Commit): Tue Nov 25 11:07:48 2008  
Code Review: [http://codereview.chromium.org/12427](http://codereview.chromium.org/12427)  
Regress: [mjsunit/regress/regress-149.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-149.js)  
```javascript
assertEquals(String.fromCharCode(0x26B), String.fromCharCode(0x2C62).toLowerCase());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b57b4a1^!)  
[src/SConscript](https://cs.chromium.org/chromium/src/v8/src/SConscript?cl=b57b4a1)  
[src/assembler-ia32-inl.h](https://cs.chromium.org/chromium/src/v8/src/assembler-ia32-inl.h?cl=b57b4a1)  
[src/assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/assembler-ia32.cc?cl=b57b4a1)  
[src/assembler-ia32.h](https://cs.chromium.org/chromium/src/v8/src/assembler-ia32.h?cl=b57b4a1)  
[src/assembler-irregexp-inl.h](https://cs.chromium.org/chromium/src/v8/src/assembler-irregexp-inl.h?cl=b57b4a1)  
...  
  

---   

## **regress-137.js (v8 issue)**  
   
**[Issue: division-> switch problem (it works correctly in previous chrome, in FF, Safari and IE)](https://crbug.com/v8/137)**  
**[Commit: If a HeapNumber is the incoming value, it must be converted to Smi before](https://chromium.googlesource.com/v8/v8/+/4e3bbd8)**  
  
Date(Commit): Mon Nov 03 13:33:13 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@679](http://v8.googlecode.com/svn/branches/bleeding_edge@679)  
Regress: [mjsunit/regress/regress-137.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-137.js)  
```javascript
(function () {
  var strNum = 170;
  var base = strNum / 16;
  var rem = strNum % 16;
  var base = base - (rem / 16);  // base is now HeapNumber with valid Smi value.

  switch(base) {
    case 10: return "A";  // Expected result.
    case 11: return "B";
    case 12: return "C";
    case 13: return "D";
    case 14: return "E";
    case 15: return "F";  // Enough cases to trigger fast-case Smi switch.
  };
  fail("case 10", "Default case", "Heap number not recognized as Smi value");
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4e3bbd8^!)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=4e3bbd8)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=4e3bbd8)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=4e3bbd8)  
[src/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime.h?cl=4e3bbd8)  
[test/mjsunit/regress/regress-137.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-137.js?cl=4e3bbd8)  
...  
  

---   

## **regress-124.js (v8 issue)**  
   
**[Issue: implicit 'this' is not as expected in an eval statement inside a function](https://crbug.com/v8/124)**  
**[Commit: Added failing test case for bug 124.](https://chromium.googlesource.com/v8/v8/+/96733af)**  
  
Date(Commit): Thu Oct 23 05:49:05 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@558](http://v8.googlecode.com/svn/branches/bleeding_edge@558)  
Regress: [mjsunit/regress/regress-124.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-124.js)  
```javascript
assertEquals("[object global]", this.toString());
assertEquals("[object Undefined]", toString());

assertEquals("[object global]", eval("this.toString()"));
assertEquals("[object Undefined]", eval("toString()"));

assertEquals("[object global]", eval("var f; this.toString()"));
assertEquals("[object Undefined]", eval("var f; toString()"));


function F(f) {
  assertEquals("[object global]", this.toString());
  assertEquals("[object Undefined]", toString());

  assertEquals("[object global]", eval("this.toString()"));
  assertEquals("[object Undefined]", eval("toString()"));

  assertEquals("[object global]", eval("var f; this.toString()"));
  assertEquals("[object Undefined]", eval("var f; toString()"));

  assertEquals("[object Undefined]", eval("f()"));

  // Receiver should be the arguments object here.
  assertEquals("[object Arguments]", eval("arguments[0]()"));
  with (arguments) {
    assertEquals("[object Arguments]", toString());
  }
}

F(Object.prototype.toString);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/96733af^!)  
[test/mjsunit/bugs/bug-124.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-124.js?cl=96733af)  
  

---   

## **regress-1439135.js (other issue)**  
   
**[Commit: Added failing test case for bug 1439135.](https://chromium.googlesource.com/v8/v8/+/3d4d596)**  
  
Date(Commit): Tue Oct 21 07:39:53 2008  
Code Review: [http://codereview.chromium.org/7808](http://codereview.chromium.org/7808)  
Regress: [mjsunit/regress/regress-1439135.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1439135.js)  
```javascript
function Test() {
  var left  = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
  var right = "YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY";
  for (var i = 0; i < 100000; i++) {
    var cons = left + right;
    var substring = cons.substring(20, 80);
    var index = substring.indexOf('Y');
    assertEquals(34, index);
  }
}

Test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3d4d596^!)  
[test/mjsunit/bugs/bug-1439135.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-1439135.js?cl=3d4d596)  
  
  
---   

## **regress-116.js (v8 issue)**  
   
**[Issue: function return value when accessing/setting array is not consistent with other JS engines](https://crbug.com/v8/116)**  
**[Commit: Fix issue 116 by returning the value from SetFastElement.](https://chromium.googlesource.com/v8/v8/+/c63477d)**  
  
Date(Commit): Fri Oct 17 06:36:35 2008  
Code Review: [http://codereview.chromium.org/7615](http://codereview.chromium.org/7615)  
Regress: [mjsunit/regress/regress-116.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-116.js)  
```javascript
var testCache = {};
var doLookup = function(id) {
  return testCache[id] = 'foo';
};

var r2 = doLookup(0);
var r1 = doLookup([0]);

assertFalse(r1 === testCache);
assertEquals('foo', r1);
assertEquals('f', r1[0]);
assertEquals('foo', r2);
assertEquals('f', r2[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c63477d^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c63477d)  
[test/mjsunit/regress/regress-116.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-116.js?cl=c63477d)  
  

---   

## **regress-114.js (v8 issue)**  
   
**[Issue: Incorrect loop in "runtime.cc"](https://crbug.com/v8/114)**  
**[Commit: Fixed bug 114](https://chromium.googlesource.com/v8/v8/+/a601594)**  
  
Date(Commit): Tue Oct 14 09:13:23 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@493](http://v8.googlecode.com/svn/branches/bleeding_edge@493)  
Regress: [mjsunit/regress/regress-114.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-114.js)  
```javascript
assertEquals("FRIEDRICHSTRASSE 14", "friedrichstra\xDFe 14".toUpperCase());
assertEquals("XXSSSSSSXX", "xx\xDF\xDF\xDFxx".toUpperCase());
assertEquals("(SS)", "(\xDF)".toUpperCase());
assertEquals("SS", "\xDF".toUpperCase());

assertEquals("i\u0307", "\u0130".toLowerCase());
assertEquals("(i\u0307)", "(\u0130)".toLowerCase());
assertEquals("xxi\u0307xx", "XX\u0130XX".toLowerCase());

assertEquals("\u03A5\u0308\u0301", "\u03B0".toUpperCase());
assertEquals("(\u03A5\u0308\u0301)", "(\u03B0)".toUpperCase());
assertEquals("XX\u03A5\u0308\u0301XX", "xx\u03B0xx".toUpperCase());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a601594^!)  
[src/runtime.cc](https://cs.chromium.org/chromium/src/v8/src/runtime.cc?cl=a601594)  
[test/mjsunit/regress/regress-114.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-114.js?cl=a601594)  
  

---   

## **regress-86.js (v8 issue)**  
   
**[Issue: Continuing a for-each loop in a finally clause exits the loop](https://crbug.com/v8/86)**  
**[Commit: - Added support for warnings on unused test rules.](https://chromium.googlesource.com/v8/v8/+/2d0c43a)**  
  
Date(Commit): Thu Sep 25 12:38:34 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@375](http://v8.googlecode.com/svn/branches/bleeding_edge@375)  
Regress: [mjsunit/regress/regress-86.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-86.js)  
```javascript
var aList = [1, 2, 3];
var loopCount = 0;
var leftThroughFinally = false;
var enteredFinally = false;
for (x in aList) {
  leftThroughFinally = true;
  try {
    throw "ex1";
  } catch(er1) {
    loopCount += 1;
  } finally {
    enteredFinally = true;
    continue;
  }
  leftThroughFinally = false;
}
assertEquals(3, loopCount);
assertTrue(enteredFinally);
assertTrue(leftThroughFinally);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2d0c43a^!)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=2d0c43a)  
[test/mjsunit/bugs/bug-86.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-86.js?cl=2d0c43a)  
[test/mjsunit/bugs/bug-87.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-87.js?cl=2d0c43a)  
[tools/test.py](https://cs.chromium.org/chromium/src/v8/tools/test.py?cl=2d0c43a)  
  

---   

## **regress-69.js (v8 issue)**  
   
**[Issue: Crash on http://www.steev.net](https://crbug.com/v8/69)**  
**[Commit: Fix http://code.google.com/p/v8/issues/detail?id=69 :](https://chromium.googlesource.com/v8/v8/+/88192fc)**  
  
Date(Commit): Tue Sep 16 11:23:02 2008  
Code Review: [http://code.google.com/p/v8/issues/detail?id=69](http://code.google.com/p/v8/issues/detail?id=69)  
Regress: [mjsunit/regress/regress-69.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-69.js)  
```javascript
function unbalanced_switch(a) {
  try {
    switch (a) {
      default: break;
    }
  } catch (e) {}
  gc();
}

unbalanced_switch(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/88192fc^!)  
[src/codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-arm.cc?cl=88192fc)  
[src/codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/codegen-ia32.cc?cl=88192fc)  
[test/mjsunit/regress/regress-69.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-69.js?cl=88192fc)  
  

---   

## **regress-57.js (v8 issue)**  
   
**[Issue: Crash when deleting prototype[0]](https://crbug.com/v8/57)**  
**[Commit: Fixed bug #57.  Introduced String::Utf8Value and replaced a bunch of](https://chromium.googlesource.com/v8/v8/+/6974e4b)**  
  
Date(Commit): Wed Sep 10 11:41:48 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@254](http://v8.googlecode.com/svn/branches/bleeding_edge@254)  
Regress: [mjsunit/regress/regress-57.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-57.js)  
```javascript
try {
  delete (void 0).x;
} catch (e) {
  print(e.toString());
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6974e4b^!)  
[include/v8.h](https://cs.chromium.org/chromium/src/v8/include/v8.h?cl=6974e4b)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=6974e4b)  
[samples/shell.cc](https://cs.chromium.org/chromium/src/v8/samples/shell.cc?cl=6974e4b)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=6974e4b)  
[src/checks.cc](https://cs.chromium.org/chromium/src/v8/src/checks.cc?cl=6974e4b)  
...  
  

---   

## **regress-35.js (v8 issue)**  
   
**[Issue: Need to check for end of string when parsing break or continue.](https://crbug.com/v8/35)**  
**[Commit: Fix issue 35 by applying patch by Daniel James.](https://chromium.googlesource.com/v8/v8/+/2f0c910)**  
  
Date(Commit): Mon Sep 08 07:58:54 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@195](http://v8.googlecode.com/svn/branches/bleeding_edge@195)  
Regress: [mjsunit/regress/regress-35.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-35.js)  
```javascript
var result;
eval("result = 42; while(true)break");
assertEquals(42, result);

eval("result = 87; while(false)continue");
assertEquals(87, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f0c910^!)  
[AUTHORS](https://cs.chromium.org/chromium/src/v8/AUTHORS?cl=2f0c910)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=2f0c910)  
[test/mjsunit/regress/regress-35.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-35.js?cl=2f0c910)  
  

---   

## **regress-1134697.js (other issue)**  
   
**[Commit: - Added some "special" tests that were left out before.](https://chromium.googlesource.com/v8/v8/+/472ae34)**  
  
Date(Commit): Wed Sep 03 07:31:19 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@116](http://v8.googlecode.com/svn/branches/bleeding_edge@116)  
Regress: [mjsunit/regress/regress-1134697.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1134697.js)  
```javascript
(-90).toPrecision(6);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/472ae34^!)  
[test/mjsunit/fuzz-natives.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/fuzz-natives.js?cl=472ae34)  
[test/mjsunit/greedy.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/greedy.js?cl=472ae34)  
[test/mjsunit/leakcheck.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/leakcheck.js?cl=472ae34)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=472ae34)  
[test/mjsunit/number-tostring-small.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/number-tostring-small.js?cl=472ae34)  
...  
  
  
---   

## **regress-1346700.js (other issue)**  
   
**[Commit: Add a test that access a property name in its unicode escaped form.](https://chromium.googlesource.com/v8/v8/+/69b74a9)**  
  
Date(Commit): Thu Aug 28 18:32:52 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@30](http://v8.googlecode.com/svn/branches/bleeding_edge@30)  
Regress: [mjsunit/regress/regress-1346700.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1346700.js)  
```javascript
var o = {"\u59cb\u53d1\u7ad9": 1};
assertEquals(1, o.\u59cb\u53d1\u7ad9);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/69b74a9^!)  
[test/mjsunit/bugs/bug-1346700.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/bugs/bug-1346700.js?cl=69b74a9)  
  
  
---   

## **regress-20070207.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-20070207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-20070207.js)  
```javascript
function f(s) {
  arguments.length;
  return (s += 10) < 0;
}

assertTrue(f(-100));
assertTrue(f(-20));
assertFalse(f(-10));
assertFalse(f(-5));
assertFalse(f(0));
assertFalse(f(10));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1327557.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1327557.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1327557.js)  
```javascript
var x = { valueOf: function() { throw "x"; } };
var y = { valueOf: function() { throw "y"; } };

var exception = false;
try {
  x * -y;
} catch (e) {
  exception = true;
  assertEquals("y", e);
}
assertTrue(exception);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1254366.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1254366.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1254366.js)  
```javascript
function gee() {};

Object.prototype.findOrStore = function() {
  var z = this.vvv = gee;
  return z;
};

var a =  new Object();
assertEquals(gee, a.findOrStore());
assertEquals(gee, a.findOrStore());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1215653.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1215653.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1215653.js)  
```javascript
var caught = false;
try {
  OverflowParserStack();
  assertTrue(false);
} catch (e) {
  assertTrue(e instanceof RangeError);
  caught = true;
}
assertTrue(caught);


function OverflowParserStack() {
  var s =
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((" +
      "((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((((";
  eval(s);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1213516.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1213516.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1213516.js)  
```javascript
function run() {
 var a = 0;
 L: try {
   throw "x";
 } catch(x) {
   break L;
 } finally {
   a = 1;
 }
 assertEquals(1, a);
}

run();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1203459.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1203459.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1203459.js)  
```javascript
var obj = { 0.2 : 'a' }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1200351.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1200351.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1200351.js)  
```javascript
var enums = "";
for (var k in this) enums += (k + '|');
assertEquals(-1, enums.split('|').indexOf("constructor"));

new this.constructor;
new this.constructor();
new this.constructor(1,2,3,4,5,6);

var x = 0;
try {
  eval("SetValueOf(typeof(break.prototype.name), Math.max(typeof(break)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join((void), false.className(), null instanceof continue, return 'a', 0.__defineGetter__(x,function(){native}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ void&&null.push(goto NaN) : Math.max(undef).toText }) { {-1/null,1.isNull} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new break>>>=native.charCodeAt(-1.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(this > native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {native,0.2}?continue+undef:IsSmi(0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = break.toString()&&return continue")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (-1==continue.toJSONProtocol, GetFunctionFor(break.call(NaN)), (!new RegExp).prototype.new Object()<<void) { debugger.__defineSetter__(null,function(){continue})>>>=GetFunctionFor(-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat(NaN).splice() in null.add(1).className()) { true[0.2]<<x.splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (debugger.constructor.valueOf()) { this.sort().true.splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("unescape(break.toObject()).prototype.new RegExp.continue.__lookupGetter__(x.slice(1, NaN)) = typeof(null.push(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Iterator(continue.pop()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new RegExp.shift().concat({debugger,continue}) }; X(return goto 0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(0.add(break)&&x > null)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ eval(Array(x)) : 1.call('a').superConstructor }) { debugger.lastIndex.toLocaleString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = return true.__defineGetter__(this,function(){0.2})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new typeof(0)&this.lastIndex")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("String(new RegExp.call(1)).prototype.unescape(parseFloat(-1)) = false<<true.x.lastIndexOf(1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 1+debugger.valueOf() : continue.join().name() }) { parseInt(true)==undef.sort() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new RegExp>>0.2.superConstructor.prototype.eval(void).className() = false.join().prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export (new Object()?undef:native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new null.isNull.slice(x.prototype.value, Iterator(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export function () { 0.2 }.unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.max(continue.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = return debugger.toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (-1.length+new Object().prototype.name) { case (debugger.constructor.sort()): IsPrimitive(undef.__defineSetter__(undef,function(){native})); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete (!new Object().toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(0<<'a'>>>=new RegExp['a'])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {unescape(true),new RegExp.isNull}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1.lastIndexOf(false)?parseFloat(void):Join(null, continue, new Object(), x, break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label null/void-break.__lookupGetter__(native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2.join().constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label function () { false }.__lookupGetter__(this==1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(-1.prototype.0.2.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new return goto -1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {Number(debugger)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (parseInt(break) instanceof 0.length) { this.(!0.2) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(break.superConstructor[throw new false(true)], this.~x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(function () { IsSmi(-1) }, unescape(IsPrimitive(void)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (new RegExp.join().className() in new Object().length()>>true.toObject()) { parseFloat(escape(debugger)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new String(debugger).toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(1.indexOf('a')<<break.__lookupGetter__('a'), new Object().null.prototype.new RegExp.charCodeAt(-1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new {parseInt(0)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(void.join().add(escape(undef)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native parseFloat(false.charAt(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(~Iterator(void))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(NaN.shift().toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(native-debugger<<continue.slice(x, new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = parseFloat(~new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (null.size/true.add(void) in 0+continue&true.null) { continue.toObject()/throw new true(debugger) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Iterator(native+break) in debugger.superConstructor.constructor) { Math.max(0.add(undef)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {-1.add(native),true.sort()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {IsSmi(break),throw new 'a'(null)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(0).length()) { case ('a'.toObject().__defineSetter__(GetFunctionFor(null),function(){(!x)})): IsSmi(void).constructor; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 0.lastIndexOf(NaN).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0>>>=this.lastIndex : new Object().lastIndexOf(true).toObject() }) { x.lastIndex > 1.__defineSetter__(false,function(){this}) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ throw new false(0.2).prototype.name : parseFloat(false)+(!debugger) }) { escape(undef.lastIndex) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Math.pow(0.2).toJSONProtocol.prototype.break.superConstructor.slice(NaN.exec(undef), -1.lastIndexOf(NaN)) = true.splice().length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native continue.className().constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (0.2.isNull&undef.toString()) { continue/void+parseInt(null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Math.pow(break==this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(continue.__lookupGetter__(null).constructor, debugger.filter(0.2)>>>=this.'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2.unshift() > true.size : return Math.max(new RegExp) }) { void.splice().toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new unescape(false).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return this.true?'a'==this:0.2.__lookupGetter__(void) }; X(Iterator(false).length)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = function () { null }.__defineSetter__(0.charCodeAt(new Object()),function(){null>>>=new Object()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import goto 'a'.charAt(native.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import 0.2.isNull.__lookupGetter__(debugger.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~new Object().push(Array(null)) in new RegExp>>>=void.prototype.name) { goto break.lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete String(x).slice(String('a'), parseFloat(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(continue.__defineGetter__(0.2,function(){1}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(true.concat(undef)==0.2.new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return NaN['a']?-1.exec(0):NaN.prototype.this }; X(native.prototype.name.toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (debugger==continue.toObject(), Array(NaN.className()), Math.max(new RegExp).prototype.value) { GetFunctionFor('a').prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new parseInt(break)==Array(x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseInt(0.2.charCodeAt(this)), this.continue.prototype.name, native.superConstructor.superConstructor) { Join(0.__defineGetter__(continue,function(){undef}), {1}, parseFloat(0), undef.__defineSetter__(break,function(){null}), x?-1:-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join(debugger.splice(), parseInt(NaN), new RegExp.pop(), this.false, x.-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Math.max(native).charCodeAt(continue==break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (void==NaN.sort(), new Object()==new RegExp.toObject(), -1/NaN.unshift()) { GetFunctionFor(true).name() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ((!'a'.join()), ~NaN.__defineGetter__(undef,function(){this}), Math.pow(NaN).__lookupGetter__(typeof(false))) { throw new debugger.toObject()(Math.max(-1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (NaN.shift()&&undef&&continue in throw new x(NaN).prototype.-1&x) { return native.toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (0).charAt(this.charCodeAt(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return x.valueOf().size }; X(0.2.unshift().unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (eval(new Object().valueOf())) { break.prototype.name.__defineGetter__(eval(NaN),function(){Math.max(native)}) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Math.pow(1).isNull in Iterator(continue.length())) { Join(true, 0.2, null, x, new Object()).length }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(0>>>=void.unshift(), void.exec('a').undef.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete throw new this(0.2).pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Iterator(unescape(continue))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return unescape(goto debugger) }; X(new RegExp.push(break).name())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = undef/'a'.indexOf(-1.exec(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.isNull.filter(this.toText), function () { throw new 'a'(0.2) }, native?break:undef.prototype.return continue) { Array(void.toText) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.slice(new Object(), 1).isNull")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (0.2.className().call((!debugger)), native.__defineGetter__(0,function(){x}).name(), null.splice().splice()) { NaN.charCodeAt(new Object()) > true.toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native false.length?new RegExp instanceof this:Array(undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new ~0.2.call(typeof(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(0.2.sort())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new x.join().shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (~new Object().toText) { case (new RegExp.unshift().exec(new RegExp<<debugger)): -1.length.exec(this.isNull); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(~true)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new unescape(debugger.call(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new GetFunctionFor(0.2).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete IsPrimitive(null.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (eval(0.2) instanceof debugger.splice() in null.superConstructor==new Object()&void) { Number(0+x) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ('a'-continue?null.length():escape(continue)) { return undef.push(false.shift()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Array(x.length) in 'a'.length().sort()) { goto (new Object()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (NaN==true.length) { IsPrimitive(0.2).prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(return true&&void, new RegExp.toObject().length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.pow(void).length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(void.add(continue).charCodeAt(this.toObject()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Join(break.toObject(), 0.2.isNull, false.call(0), break.filter(break), 1.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (1/NaN.__lookupGetter__(undef.prototype.value)) { escape(eval(this)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(Join(unescape(x), new RegExp.__defineGetter__(debugger,function(){NaN}), 'a'.indexOf(0.2), false.prototype.name, (this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Math.pow(native).indexOf(1>>>=-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new RegExp?native:continue.join().prototype.Math.max(x.__defineSetter__(1,function(){continue})) = parseFloat(parseInt(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native function () { new RegExp }.new RegExp.pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import typeof(new RegExp.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (0.2.size>>NaN-continue) { case ('a'.push(true).indexOf(NaN.lastIndexOf(-1))): {0.2,x}.toObject(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (IsSmi(new Object())/false.filter('a')) { function () { Iterator(debugger) } }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = break.lastIndex.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(new Object() > 0.length())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native IsPrimitive(continue)==break.charCodeAt(new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new break.true<<'a'-NaN")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Number(-1?'a':-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat('a'.exec(continue)) in (!new RegExp)&&0.2.toObject()) { {true,x}.add(void.prototype.NaN) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (-1.prototype.value.join()) { (!1.prototype.name) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new GetFunctionFor(continue).toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Math.pow(continue.slice(null, native)), goto (!0), native?1:this.charAt(String(debugger))) { parseFloat(~this) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(debugger.pop().length, new RegExp.isNull.toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (typeof(new RegExp.slice(new RegExp, 0)) in native.toLocaleString().lastIndexOf(0.2.length())) { native>>>=new RegExp.length() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native x.join().className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0?0:true.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = IsPrimitive(0).concat(new Object().name())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new parseFloat(x)?this.valueOf():IsSmi(x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 'a'.slice(null, -1).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 'a'+void.concat('a'>>>=-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(escape(0.length))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = parseInt(0.lastIndexOf(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(null&debugger.valueOf(), 0[false].push(false.add(debugger)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = parseInt(new RegExp.__lookupGetter__(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(~false&&break>>0, new RegExp.lastIndex.add({this}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Join(break, continue, 0, debugger, NaN).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import new Object().sort().superConstructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsSmi(goto -1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Iterator(null).toObject() }; X(-1==new Object()==0.__lookupGetter__(native))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native void.join().add(parseFloat(continue))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (function () { -1 }.shift()) { escape(1.unshift()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(new RegExp.indexOf(1).filter(continue instanceof break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (NaN?continue:NaN.shift()) { native.push(null).add(new Object().superConstructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new Object().length().toText }; X(debugger.indexOf(this).toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new Object().call('a').charCodeAt(native.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new function () { continue }.add(true.slice(continue, new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x[native] instanceof -1.join().prototype.this.null.size = 0.2.prototype.x+0.2.indexOf(false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (this instanceof new RegExp.splice() in null>>>=new RegExp.valueOf()) { function () { unescape(1) } }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (true.shift()/native.null in undef.call(NaN).isNull) { native+this-x.size }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return false.pop()<<Join(continue, false, break, NaN, -1) }; X(IsSmi(debugger>>x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if ({parseFloat(null),Math.max(native)}) { 0.2-new Object().__lookupGetter__(eval(new Object())) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(Array(1).toLocaleString(), null.name().exec(undef.filter(false)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(true.filter(this).pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (break.lastIndex.superConstructor) { new Object().toString().length() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label (!0.2/debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ NaN.concat(new RegExp)+Join(1, false, new Object(), new Object(), x) : unescape(x).concat(Iterator(-1)) }) { 'a'.isNull.__lookupGetter__(this+native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.name()/IsPrimitive(this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {null}.prototype.value")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new true+false.__lookupGetter__(null&continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (-1.push(new RegExp)[void.valueOf()]) { new RegExp.className().__lookupGetter__(Array(0)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export NaN.__lookupGetter__(undef).__lookupGetter__(void.isNull)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ ~new RegExp.filter(undef&&this) : String(continue)<<NaN.toText }) { this.exec(this).length }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (true&void.exec(void.exec(continue)) in Join('a', undef, new Object(), continue, x) instanceof {undef}) { unescape(-1.prototype.name) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import void.push(true).join()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({break}&x.name(), 1.charAt(false).slice(continue.superConstructor, this&&break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (this.call(this) > Iterator(continue)) { new Object().prototype.value.slice(1.slice(native, -1), (!false)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export parseInt(new RegExp>>>=x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (escape(x==debugger), NaN.shift()&debugger?false:0.2, (!new RegExp)&goto break) { unescape(x.toText) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(throw new NaN.toObject()(this?break:true))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new (typeof(this))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape('a'/0) in ~new Object().lastIndex) { IsSmi(0).push(0.concat(0.2)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("(!new RegExp)[0.2 > new Object()].prototype.Number(debugger.join()) = native&-1.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new false.toJSONProtocol&&0.2.constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~0?0.2:undef in new RegExp.charCodeAt(0).prototype.name) { NaN.toLocaleString().splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (~IsPrimitive(new RegExp), true.toString().size, null.charCodeAt('a') > null.concat(0)) { break.toJSONProtocol/IsPrimitive(break) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new parseInt(new Object()).lastIndexOf(NaN > void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.splice()&&-1.prototype.new Object()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{{true,0}}.prototype.break.length.splice() = 'a'.toText.superConstructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (debugger>>>=continue > break.exec(1)) { Math.pow(new RegExp)==NaN>>>=0.2 }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2==0.2/goto true : IsSmi(native).isNull }) { throw new {x,null}(false.className()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {false.concat(null),Math.pow(NaN)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Array(null).add(NaN.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (parseFloat(new Object()==true) in GetFunctionFor('a'&false)) { native&undef.toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new {eval(null),(debugger)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import {this.0,debugger.filter(NaN)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import break.charAt(-1)<<false.__defineSetter__(0,function(){x})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = goto false > new Object()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.superConstructor[debugger.isNull].prototype.Math.max('a').shift() = parseInt(0).size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native eval(void.add(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(x > void.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {this.toObject()} : Number(NaN).toJSONProtocol }) { 0.2.className().prototype.name }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (false.__defineGetter__(undef,function(){undef}).exec(NaN.splice())) { typeof(Join(void, new RegExp, break, -1, -1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (false.splice().toObject(), continue.name().size, Join(void?debugger:this, new RegExp.__defineSetter__(NaN,function(){NaN}), x.unshift(), this.true, parseInt(break))) { undef<<continue.toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (this.0.indexOf(break)) { break.charAt(this).unshift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Join(new Object().splice(), this instanceof 1, parseFloat(NaN), undef.concat(x), void.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(goto NaN.toString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 'a'<<break.shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Iterator(continue)[new Object()>>NaN]")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Join(new RegExp, 'a', this, void, true)>>>=continue>>native")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import new Object().toJSONProtocol.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return undef.__defineSetter__(native,function(){void}).toJSONProtocol }; X(eval(x).charCodeAt('a'.concat(true)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(throw new 0.2.__defineGetter__(NaN,function(){-1})(void&&new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 0.unshift() > IsSmi(NaN)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label x.call(null).lastIndex")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(IsSmi(0.2.add(0)), x.add(break).this.__defineGetter__(undef,function(){new RegExp}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native Number(this).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new NaN.shift().add(String(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new null.name().splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 1.undef.push(new Object().call(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(parseInt(1).size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = this.x.sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(continue.valueOf().prototype.new RegExp.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(this.charAt(continue)?undef+'a':unescape(1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({throw new 'a'(0.2),void.lastIndexOf(NaN)}, Math.pow(new Object().className()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (1.slice(new Object(), this).valueOf()) { parseInt(true).pop() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 0.2.superConstructor.lastIndex : goto debugger<<Join(undef, 1, true, undef, debugger) }) { function () { NaN }.prototype.name }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("-1.exec(debugger).length.prototype.debugger > null.slice(Iterator(void), continue.concat(0)) = parseInt(throw new 1(1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(new Object().constructor.call(Number(1)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new null.unshift().call(escape(x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (Math.pow(native).toLocaleString()) { case (false instanceof native.join()): Math.pow(NaN).size; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label function () { new Object() }.prototype.true.size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Join('a', 0.2, false, new Object(), void).continue.className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = IsPrimitive(break.__lookupGetter__(-1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Object()>>0.2.prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsPrimitive(new Object()).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (Array(parseInt(break))) { 'a'.toString().unshift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = return 0.2>>>=-1?undef:undef")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Object().splice().unshift().prototype.null&&native.__lookupGetter__(undef>>>=NaN) = (1<<break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete NaN.charAt(1).concat(NaN.0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(new RegExp.sort().toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return GetFunctionFor(false).lastIndexOf(1.shift()) }; X(this.0.2.charCodeAt(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto NaN.toObject(), ~true.'a', parseInt(debugger)+eval(false)) { eval(0.2.constructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(debugger).pop()) { case (this.push(true).valueOf()): Join(continue, debugger, native, native, debugger).filter(Array(continue)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new debugger.sort() instanceof this>>1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ parseFloat(false).prototype.(!new Object()) : {unescape(-1)} }) { Math.max(new RegExp.superConstructor) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate({Math.pow(break)})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import typeof(break.valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Math.pow(-1[new RegExp]))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native IsPrimitive(1).concat({x,null})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("NaN.length.prototype.value.prototype.function () { null==new Object() } = break.name()&IsPrimitive(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete NaN.prototype.-1.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new continue.unshift()+parseFloat(undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new NaN-break.call(false.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native new RegExp.exec(break).pop()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf({'a',null}.prototype.value, 1.shift() instanceof {'a',0})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (debugger.valueOf().size, function () { x.unshift() }, IsSmi(1)&&true==native) { new Object().__defineGetter__(this,function(){'a'})&&eval(native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export 'a'.pop().charCodeAt(x.className())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export String(IsSmi(debugger))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("typeof(debugger).valueOf().prototype.(1).lastIndexOf(this.break) = x.prototype.name.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native Array(typeof(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(1.__defineGetter__(1,function(){1}).null.constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = 1.charAt(0).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Math.max('a'.filter(new Object())))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(void.prototype.name.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (-1.toJSONProtocol.call(-1.size) in ~x.sort()) { eval(0&debugger) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ('a'==undef.join() in Math.pow(IsSmi(false))) { undef > this>>goto x }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate('a'.constructor.isNull)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (GetFunctionFor(this.slice(0.2, this)), this.prototype.void?null.unshift():native.className(), Number(new Object().call(-1))) { 0.splice() > debugger&&this }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {goto new RegExp,Join(new Object(), native, continue, -1, x)} : NaN&x/{0,break} }) { this.lastIndexOf(new RegExp).join() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (typeof(break.length())) { native&&false.sort() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new parseFloat(-1 instanceof break)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label throw new continue.unshift()(null.shift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Math.max(0.2.toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return false.unshift().className() }; X(escape(NaN&NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Join(native.toText, goto x, 0.2.splice(), Join('a', 0, void, NaN, 1), eval(native)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (GetFunctionFor(true.prototype.name)) { parseInt(NaN).toLocaleString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new escape(native).__defineSetter__(return native,function(){undef > native})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new typeof(true > 'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (debugger.prototype.0.2<<new RegExp+false) { case (native.splice().filter({x})): false&true.indexOf(1.__defineGetter__(native,function(){continue})); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label true-NaN.prototype.native.shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new typeof(new RegExp.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (function () { this.NaN }) { case (this.continue.prototype.parseFloat(false)): IsPrimitive(new Object()-'a'); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.__lookupGetter__(debugger).indexOf(native.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (GetFunctionFor(NaN.lastIndex)) { case (new RegExp.lastIndex.toLocaleString()): NaN.join().indexOf(eval(-1)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {void.charAt(true)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new Object()==NaN.join()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(typeof(Array(new Object())))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label throw new (false)(eval(x))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new RegExp.size.charAt(true > -1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger.toObject().charAt(this<<undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 'a'.valueOf()+parseInt(undef) : IsPrimitive(null).lastIndex }) { NaN.toObject().isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new new Object()&&void.lastIndexOf(0.2.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ 1+1.name() : Join(Math.pow(debugger), new RegExp-1, x > 1, x<<-1, new RegExp.size) }) { undef[undef].size }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete native.call(-1).isNull")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (new Object()>>>=break==Math.pow(debugger)) { IsPrimitive(this).lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ((!x&&new RegExp) in undef.toLocaleString().slice(new RegExp.indexOf(NaN), IsPrimitive(-1))) { false.size+debugger[x] }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import 0.length.__defineGetter__(0.2.shift(),function(){'a'.className()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(goto new Object().push(void))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Array(this.0) : parseFloat(void).pop() }) { escape(true).slice(continue.lastIndex, false.toObject()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new native==true.filter({NaN,-1})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ('a'.__defineSetter__(continue,function(){-1}).unshift(), Array(undef).toLocaleString(), undef.__lookupGetter__(void).toLocaleString()) { parseInt(false/native) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.x<<false.prototype.true.toLocaleString()==NaN.pop() = this.superConstructor>>Math.max(true)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return this.prototype.name.splice() }; X(unescape(x).__lookupGetter__(Number(debugger)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (!NaN).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(escape(Iterator(this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Number(new RegExp)<<this?true:-1 }; X(Number(null).lastIndex)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export this.void.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (this.prototype.null.sort() in -1.className()&void.filter(new Object())) { GetFunctionFor(new Object()).pop() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label 0[break].sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (null.length().toString(), eval(-1).toObject(), (!continue.concat(continue))) { true.name()/native<<new RegExp }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape(null).sort(), Number(undef).charCodeAt(IsPrimitive(NaN)), null>>true/null.join()) { 0.2.toObject() > IsPrimitive(new RegExp) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date({NaN,native}&&1+undef)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(IsPrimitive(undef>>>=1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (Join(true, 'a', true, 1, NaN).add({1}), GetFunctionFor(new Object().push(new Object())), goto 1.length) { Math.pow(GetFunctionFor(native)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return break.isNull > parseInt(continue) }; X((new RegExp instanceof 1))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Number(false).indexOf(x instanceof new Object()) : function () { x.toString() } }) { false.name().indexOf(GetFunctionFor(null)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date('a'.constructor.prototype.name)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("GetFunctionFor(void&new Object()).prototype.debugger.add(null)[void.unshift()] = new RegExp.isNull.Iterator(this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete false?break:undef.constructor")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ (native.filter(1)) : eval(this&&0.2) }) { undef.length instanceof new Object().toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export String(break.lastIndexOf(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label (!Iterator(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(String(null==-1), {1&0})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(parseInt('a' > 0))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(debugger.toJSONProtocol.indexOf(escape(0)), this.filter(null).__defineSetter__(continue.break,function(){debugger>>null}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.name().length().prototype.goto false.exec(true.charCodeAt(continue)) = Join(-1-false, undef.superConstructor, 'a'.shift(), (!x), NaN.this)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(typeof(new RegExp).sort())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0.2.concat(x).splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto void.indexOf(throw new x(1)), typeof(return new RegExp), IsPrimitive(-1).add(void.lastIndexOf(debugger))) { null.indexOf(void).toText }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("return new RegExp.pop().prototype.String(x.toObject()) = 1.superConstructor.charCodeAt(new RegExp.charCodeAt(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new null&true.prototype.name")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1>>>=NaN.indexOf((debugger))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new parseFloat(null).splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import -1.lastIndexOf(new RegExp) instanceof throw new void(0.2)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if ((0.shift())) { Join(IsPrimitive(-1), break.__defineSetter__(true,function(){break}), parseInt(null), parseFloat(break), true/null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new escape(1 > continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (parseInt(undef)>>false.filter(continue)) { case (this.undef/new Object()): 'a'.toJSONProtocol.__defineGetter__(new RegExp-undef,function(){parseFloat(new RegExp)}); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{void}.shift().prototype.this.Array(new Object()) = {0.2,new RegExp}.lastIndexOf(break.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new continue&&new Object().lastIndexOf(new Object() instanceof 1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (throw new 'a'.exec(x)(return false), native/void.constructor, {native}==true.toLocaleString()) { goto 1 instanceof 1.isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (break.concat(break) > native>>>=-1, (debugger.x), Join(x, void, void, new RegExp, null).name()) { void.charCodeAt(true).valueOf() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new 'a'>>0 instanceof new Object().push(new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (return ~break) { break.__defineGetter__(break,function(){-1}).shift() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Join(null, -1, undef, null, 0).toString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ({new RegExp,void}.slice(break.isNull, false.shift())) { eval(debugger.slice(this, 1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return {GetFunctionFor(0)} }; X('a'.prototype.debugger.concat(void.constructor))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (~true instanceof continue) { escape(new RegExp.toObject()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("escape(0[native]).prototype.debugger.add(1).unshift() = (true.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (unescape(void).length, undef.toObject() instanceof x.toObject(), 0.2+true.concat(true.__lookupGetter__(this))) { (x).toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(escape(null).__lookupGetter__(undef.size))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label Array(continue[false])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Number(this&&false) }; X(NaN.toJSONProtocol.toJSONProtocol)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.toString().shift().prototype.Array(x).__lookupGetter__('a'.prototype.x) = {1.length,break.join()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new 1.charCodeAt(break)+IsSmi(false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(String(this) > 0.2.toText, new RegExp.length.lastIndexOf(1<<0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (new RegExp.pop().charAt(IsSmi(new RegExp))) { case (native.indexOf(this)/native.lastIndex): this.debugger.indexOf(debugger); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Number(x)[debugger.prototype.break])")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new RegExp>>>=x.unshift() }; X(Math.max(continue.name()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(IsSmi(null.size))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = native?0.2:1+GetFunctionFor(void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (IsPrimitive(-1)>>>=break.valueOf() in String(0 > 0.2)) { Math.max(true.length()) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (escape(unescape(NaN))) { case (Math.pow(eval(undef))): true.charAt(null)&new RegExp.pop(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete Join(new RegExp, 1, false, new Object(), this).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label return x.filter(x.join())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new new RegExp.pop().shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new (!debugger.size)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label Math.max(debugger.__lookupGetter__(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(eval(debugger[debugger]))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 0.2.filter(true)&throw new true(debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(continue.exec(debugger) > Math.pow(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("void.prototype.value.name().prototype.Number(undef&NaN) = false.__lookupGetter__(-1).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(null.__defineGetter__(native,function(){continue}).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ {new Object()[continue],native.length()} : undef.name().superConstructor }) { Math.pow(break).indexOf(0.toJSONProtocol) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (Iterator(native.call(new RegExp))) { case (String(new RegExp).isNull): goto new RegExp.pop(); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new x.constructor instanceof undef.indexOf(-1)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(this.~null, continue.pop()&0&'a')")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (GetFunctionFor(~0)) { case ('a'.'a'<<undef.__defineGetter__(false,function(){true})): (!1).lastIndex; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return debugger.unshift().0.toString() }; X(Number(break).0.2>>>=false)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Iterator(x)/undef.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(undef.join().toLocaleString(), null.add(false).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("IsSmi(x).toString().prototype.0>>continue.indexOf(NaN.__lookupGetter__(new Object())) = ~-1&typeof(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.__lookupGetter__(new RegExp).toObject(), false-0.toString(), return native.sort()) { new RegExp.name().className() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (escape(new RegExp).toString()) { case (goto eval(1)): this.filter(new Object()).call(new RegExp.slice(null, this)); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger-false.toText")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = Number(null>>new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete this&native.indexOf('a'.splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(~Math.max(break), 0.2.valueOf().length)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(Number(native.charCodeAt(x)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new goto continue.add(0)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete typeof(debugger).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("'a'<<false.toText.prototype.throw new true(1).lastIndex = 'a'.name().length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native 'a'.indexOf(debugger).charAt(NaN.add(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(break>>false.toString(), (false.indexOf(this)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete goto NaN==(!debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(0.2.join().superConstructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.void.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("SetValueOf(x.exec(debugger)[GetFunctionFor(0)], native.toObject().exec(new RegExp.sort()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(0.2.valueOf().toLocaleString())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(-1.toJSONProtocol.prototype.name)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(Array(-1.shift()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export break.concat(undef).unshift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native parseFloat(-1)?NaN.toText:debugger.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (void-continue/continue.prototype.undef in String(break.toText)) { parseInt(false).isNull }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(true.isNull.toObject())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ typeof(debugger).toObject() : x.constructor>>>=null.__defineGetter__(native,function(){debugger}) }) { unescape(undef.lastIndexOf(false)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export unescape(continue)<<native[0]")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (String(0).unescape(debugger)) { {break.pop(),0.2.constructor} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("String({true}).prototype.break.length.call(false > 0.2) = GetFunctionFor(0.prototype.new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ false.push(0.2).indexOf(Math.max(debugger)) : x&x.prototype.name }) { goto 1.lastIndex }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2.lastIndex&0.2?break:NaN)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = -1.prototype.value.toText")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import native.toLocaleString()-1.prototype.0")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export debugger[-1].indexOf(Join(new Object(), 0, x, new Object(), 0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return (!true).lastIndexOf(true.splice()) }; X(NaN.toString().prototype.value)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return continue.slice(-1, 1).prototype.true.name() }; X('a'.push(void).prototype.value)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (goto new RegExp.length(), x.sort().className(), Math.max(new RegExp.toJSONProtocol)) { (IsSmi(-1)) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 0.splice()&&-1.sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (Math.max(-1>>1)) { break.toLocaleString().toJSONProtocol }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {void.prototype.break,new RegExp.toString()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new IsSmi(debugger).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 'a'.concat(undef).sort()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new {debugger.toObject(),'a' > false}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (goto 1.concat(Join(x, undef, native, x, new Object()))) { new RegExp.prototype.name==new RegExp.superConstructor }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return new Object().__defineGetter__(0.2,function(){0.2}).length() }; X(void.isNull<<parseFloat(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete continue.toJSONProtocol.toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (continue.constructor.toObject() in true&&undef.toJSONProtocol) { String(0+break) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import true.call(continue)>>break.toString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label escape(this) > Math.pow(new RegExp)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {void}/IsSmi(new Object())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (native==null?debugger.prototype.name:null.toLocaleString()) { case (NaN.push(this).join()): (break instanceof continue); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Math.pow(x.push(0))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new (Array(NaN))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label IsSmi(new RegExp).toLocaleString()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label NaN.push(1).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("{escape(undef),debugger.filter(0.2)}.prototype.-1 > new RegExp[0.2.valueOf()] = new RegExp.prototype.value.splice()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Join(0.2, x, continue, debugger, new Object()).size")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("with ({ Number(null).name() : Math.pow(true).__defineGetter__(debugger.toString(),function(){false+0.2}) }) { this.{x,break} }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Math.pow(goto debugger)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = IsPrimitive(void.pop())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new Object().toString().toJSONProtocol")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(this.String(0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let ({-1.call(new RegExp)}) { break.length().splice() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import null.size.__defineGetter__(void.filter(x),function(){null.pop()})")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new IsPrimitive(null.superConstructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new eval(-1.prototype.continue)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (typeof(Iterator('a'))) { case (0.constructor>>~1): void.__defineGetter__(void,function(){1})/GetFunctionFor(0); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for (false instanceof x.add(true.charAt(new RegExp)) in Join(undef.lastIndexOf(break), 0.2.add(new Object()), Iterator(1), {'a',x}, Array(new Object()))) { function () { null }/1&&-1 }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new escape('a'.concat(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(Math.pow(NaN).toText)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new throw new 0(NaN).className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete String(GetFunctionFor(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = Iterator(new Object()).charAt((0.2))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Number(undef.charAt(1)).prototype.undef.lastIndexOf(true).slice(1.className(), undef.filter(-1)) = null<<null.push(parseInt('a'))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {Math.max(1),IsSmi(new Object())}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch (new Object().exec(0).isNull) { case (escape(IsSmi(false))): false.toObject()-null.size; break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new 'a'.__defineSetter__(debugger,function(){false}).name()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = debugger?-1:0+true.prototype.1")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new {false instanceof continue,native.size}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("GetFunctionFor(continue.__lookupGetter__(0.2)).prototype.Math.max(1.splice()) = true.__defineGetter__(undef,function(){NaN}).filter(String(new RegExp))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("null.size-1.toLocaleString().prototype.(this).shift() = GetFunctionFor(native.charAt(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate((!null.indexOf(-1)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = {break.sort()}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new throw new debugger.splice()(this.__lookupGetter__(undef))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("unescape(x[native]).prototype.0.splice().-1.prototype.true = x.prototype.value.className()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export x+true.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export debugger.indexOf(-1).indexOf(true.constructor)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("for ({break}.exec(new Object().continue) in eval(0.2.charAt(new Object()))) { throw new null.length(null?break:-1) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = NaN.toLocaleString().toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Math.pow(break+false) }; X(Join(true.add(new Object()), null[-1], new RegExp[true], NaN&&debugger, x.charAt(undef)))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("switch ((break).add(true.sort())) { case (undef.charAt(native).__defineGetter__(IsPrimitive(1),function(){NaN<<new RegExp})): -1.__defineSetter__(null,function(){-1}) > this.charCodeAt(this); break; }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import return 0.2.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("continue.join().toText.prototype.Number(debugger).slice(new RegExp.-1, (NaN)) = function () { (!null) }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Number(break.__lookupGetter__(false))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Date(return null/x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export Number(undef).shift()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = 1[native]/this&true")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete typeof(debugger.unshift())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import x.charAt(false)&-1>>x")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("if (null.toText.superConstructor) { typeof(-1).toString() }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (parseFloat(continue.superConstructor)) { 0.2.toText.prototype.value }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label parseInt(IsSmi(null))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete new Object().valueOf().indexOf(true-x)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new unescape(1.__defineGetter__(new Object(),function(){x}))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("let (undef.size.splice()) { 1.constructor.charCodeAt(0+'a') }")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("this.new RegExp.pop().prototype.eval(debugger).toJSONProtocol = unescape(continue).valueOf()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("const x = new this.new RegExp.indexOf(unescape(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = new break instanceof false instanceof native.length()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate(parseFloat(x).valueOf())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label {escape(true),Math.max(null)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("'a'>>>=void.prototype.value.prototype.break.prototype.break.indexOf(0.className()) = (!this&native)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("import Number(NaN).push(IsSmi(break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("export true.exec(void).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function({'a',true}/eval(new Object()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("label null.concat(null).toObject()")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("native {0.2.length,new RegExp.lastIndexOf(-1)}")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("function X(x) { return Math.max({0.2}) }; X(true.charCodeAt(null).add(new RegExp.name()))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("delete -1.lastIndex.length")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("new Function(0.2[1].call(true > break))")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("Instantiate('a'.toLocaleString().splice())")
} catch (e) { if (e.message.length > 0) { print (e.message); } };

try {
  eval("x = typeof(void&&void)")
} catch (e) { if (e.message.length > 0) { print (e.message); } };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1199637.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1199637.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1199637.js)  
```javascript
var NONE = 0;
var READ_ONLY = 1;

function AddNamedProperty(object, name, value, attrs) {
  Object.defineProperty(object, name, {
      value,
      configurable: true,
      enumerable: true,
      writable: (attrs & READ_ONLY) === 0
  });
}

AddNamedProperty(this.__proto__, "a", 1234, NONE);
assertEquals(1234, a);
eval("var a = 5678;");
assertEquals(5678, a);

AddNamedProperty(this.__proto__, "b", 1234, NONE);
assertEquals(1234, b);
eval("var b = 5678;");
assertEquals(5678, b);

AddNamedProperty(this.__proto__, "c", 1234, READ_ONLY);
assertEquals(1234, c);
eval("var c = 5678;");
assertEquals(5678, c);

AddNamedProperty(this.__proto__, "d", 1234, READ_ONLY);
assertEquals(1234, d);
eval("var d = 5678;");
assertEquals(5678, d);

AddNamedProperty(this.__proto__, "x", 1234, NONE);
assertEquals(1234, x);
eval("with({}) { var x = 5678; }");
assertEquals(5678, x);

AddNamedProperty(this.__proto__, "y", 1234, NONE);
assertEquals(1234, y);
eval("with({}) { var y = 5678; }");
assertEquals(5678, y);

AddNamedProperty(this.__proto__, "z", 1234, READ_ONLY);
assertEquals(1234, z);
eval("with({}) { var z = 5678; }");
assertEquals(5678, z);

AddNamedProperty(this.__proto__, "w", 1234, READ_ONLY);
assertEquals(1234, w);
eval("with({}) { var w = 5678; }");
assertEquals(5678, w);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1199401.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1199401.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1199401.js)  
```javascript
var ranges = [{min: -1073741824, max: 1073741823, bits: 31},
              {min: -2147483648, max: 2147483647, bits: 32}];

for (var i = 0; i < ranges.length; i++) {
  var range = ranges[i];
  var min_smi = range.min;
  var max_smi = range.max;
  var bits = range.bits;
  var name = bits + "-bit";

  var result = max_smi + 1;

  // Min smi as literal
  assertEquals(result, eval(min_smi + " * -1"), name + "-litconmult");
  assertEquals(result, eval(min_smi + " / -1"), name + "-litcondiv");
  assertEquals(result, eval("-(" + min_smi + ")"), name + "-litneg");
  assertEquals(result, eval("0 - (" + min_smi + ")")), name + "-conlitsub";

  // As variable:
  assertEquals(result, min_smi * -1, name + "-varconmult");
  assertEquals(result, min_smi / -1, name + "-varcondiv");
  assertEquals(result, -min_smi, name + "-varneg");
  assertEquals(result, 0 - min_smi, name + "-convarsub");

  // Only variables:
  var zero = 0;
  var minus_one = -1;

  assertEquals(result, min_smi * minus_one, name + "-varvarmult");
  assertEquals(result, min_smi / minus_one, name + "-varvardiv");
  assertEquals(result, zero - min_smi, name + "-varvarsub");

  // Constants as variables
  assertEquals(result, eval(min_smi + " * minus_one"), name + "-litvarmult");
  assertEquals(result, eval(min_smi + " / minus_one"), name + "-litvarmdiv");
  assertEquals(result, eval("0 - (" + min_smi + ")"), name + "-varlitsub");

  var half_min_smi = -(1 << (bits >> 1));
  var half_max_smi = 1 << ((bits - 1) >> 1);

  assertEquals(max_smi + 1, -half_min_smi * half_max_smi, name + "-half1");
  assertEquals(max_smi + 1, half_min_smi * -half_max_smi, name + "-half2");
  assertEquals(max_smi + 1, half_max_smi * -half_min_smi, name + "-half3");
  assertEquals(max_smi + 1, -half_max_smi * half_min_smi, name + "-half4");
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1187524.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1187524.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1187524.js)  
```javascript
assertEquals(undefined, ""[0x40000000]);
assertEquals(undefined, ""[0x80000000]);
assertEquals(undefined, ""[-1]);
assertEquals(undefined, ""[-0x40000001]);
assertEquals(undefined, ""[-0x80000000]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1178598.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1178598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1178598.js)  
```javascript
var value = (function() {
  var result;
  try {
    throw 42;
  } catch (e) {
    result = eval("e");
  }
  return result;
})();

assertEquals(42, value);



var value = (function() {
  var result;
  try {
    throw 87;
  } catch(e) {
    // Force the 'e' variable to be heap-allocated
    // by capturing it in a function closure.
    (function() { e; });
    result = eval("e");
  }

  // Expect accessing 'e' to yield an exception because
  // it is not defined in the current scope.
  try {
    eval("e");
    assertTrue(false);  // should throw exception
  } catch(exception) {
    assertTrue(exception instanceof ReferenceError);
    return result;
  }
})();

assertEquals(87, value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1177809.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1177809.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1177809.js)  
```javascript
String.fromCharCode(48,48,48,59,32,102,111,110,116,45,119,101,105,103,104,116,58,98,111,108,100,59,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,32,99,111,108,111,114,61,34,35,70,70,48,48,48,48,34,62,70,79,82,69,88,47,80,65,82,38,35,51,48,52,59,60,119,98,114,32,47,62,84,69,32,38,35,51,48,52,59,38,35,51,53,48,59,76,69,77,76,69,82,38,35,51,48,52,59,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,115,112,97,110,32,105,100,61,34,97,99,95,100,101,115,99,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,49,112,120,59,32,99,111,108,111,114,58,35,48,48,48,48,48,48,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,38,112,111,117,110,100,59,47,36,32,50,32,112,105,112,44,32,89,84,76,32,49,50,32,112,105,112,44,65,108,116,38,35,51,48,53,59,110,32,51,32,99,101,110,116,46,32,83,97,98,105,116,32,83,112,114,101,97,100,45,84,38,117,117,109,108,59,114,60,119,98,114,32,47,62,107,32,66,97,110,107,97,115,38,35,51,48,53,59,32,65,86,65,78,84,65,74,73,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,100,105,118,32,105,100,61,34,97,99,95,117,114,108,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,48,112,120,59,32,99,111,108,111,114,58,35,70,70,54,54,57,57,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,119,119,119,46,104,101,100,101,102,111,60,119,98,114,32,47,62,110,108,105,110,101,46,99,111,109,60,47,102,111,110,116,62,60,47,100,105,118,62,60,47,116,100,62,60,47,116,114,62,60,47,116,97,98,108,101,62,60,47,116,100,62,60,47,116,114,62,60,116,114,62,10,60,116,100,32,99,108,97,115,115,61,34,97,99,95,107,97,114,105,109,34,32,104,101,105,103,104,116,61,34,50,48,37,34,32,98,103,99,111,108,111,114,61,34,35,70,70,70,70,70,70,34,32,105,100,61,34,116,97,119,52,34,32,97,108,105,103,110,61,34,108,101,102,116,34,32,118,97,108,105,103,110,61,34,109,105,100,100,108,101,34,32,111,110,70,111,99,117,115,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,39,44,39,97,119,52,39,41,34,32,111,110,77,111,117,115,101,79,118,101,114,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,39,44,39,97,119,52,39,41,34,32,32,111,110,77,111,117,115,101,79,117,116,61,34,99,115,40,41,34,32,111,110,67,108,105,99,107,61,34,103,97,40,39,104,116,116,112,58,47,47,97,100,115,101,114,118,101,114,46,109,121,110,101,116,46,99,111,109,47,65,100,83,101,114,118,101,114,47,99,108,105,99,107,46,106,115,112,63,117,114,108,61,56,56,49,48,48,50,53,49,50,49,55,54,51,57,52,54,50,51,49,56,52,52,48,51,57,54,48,48,54,51,49,51,54,54,52,52,56,50,56,54,50,48,49,49,49,52,55,51,55,54,52,51,50,57,50,52,50,56,51,53,56,51,54,53,48,48,48,48,53,56,49,55,50,56,57,53,48,48,52,49,57,48,54,56,56,55,50,56,49,55,48,55,53,48,57,50,55,53,55,57,57,51,54,53,50,52,54,49,51,56,49,57,53,55,52,53,50,49,52,50,55,54,48,57,53,57,56,52,55,50,55,48,56,52,51,49,54,52,49,54,57,53,48,56,57,50,54,54,54,48,57,49,54,53,55,57,48,57,49,55,57,52,55,52,55,57,50,48,55,50,55,51,51,53,51,50,55,53,50,54,55,50,56,48,51,57,49,56,54,50,56,55,49,51,55,48,52,51,49,51,52,55,56,51,54,51,52,53,50,54,55,53,57,48,57,48,56,54,57,49,52,53,49,49,52,55,53,50,120,49,57,50,88,49,54,56,88,51,56,88,52,49,88,56,48,56,48,88,65,39,41,34,32,115,116,121,108,101,61,34,99,117,114,115,111,114,58,112,111,105,110,116,101,114,34,62,10,60,116,97,98,108,101,32,119,105,100,116,104,61,34,49,53,54,34,32,98,111,114,100,101,114,61,34,48,34,32,99,101,108,108,115,112,97,99,105,110,103,61,34,49,34,32,99,101,108,108,112,97,100,100,105,110,103,61,34,49,34,62,10,60,116,114,62,10,32,32,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,32,62,60,115,112,97,110,32,105,100,61,34,97,99,95,116,105,116,108,101,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,50,112,120,59,32,99,111,108,111,114,58,35,70,70,48,48,48,48,59,32,102,111,110,116,45,119,101,105,103,104,116,58,98,111,108,100,59,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,32,99,111,108,111,114,61,34,35,70,70,48,48,48,48,34,62,66,108,117,101,32,72,111,117,115,101,32,77,105,107,115,101,114,39,100,101,32,38,35,51,53,48,59,111,107,33,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,115,112,97,110,32,105,100,61,34,97,99,95,100,101,115,99,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,49,112,120,59,32,99,111,108,111,114,58,35,48,48,48,48,48,48,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,66,108,117,101,32,72,111,117,115,101,32,77,105,107,115,101,114,39,100,101,32,65,110,110,101,108,101,114,101,32,38,79,117,109,108,59,122,101,108,32,70,105,121,97,116,32,83,65,68,69,67,69,32,50,57,44,57,54,32,89,84,76,33,60,47,102,111,110,116,62,60,47,115,112,97,110,62,60,47,116,100,62,10,60,47,116,114,62,60,116,114,62,10,60,116,100,32,97,108,105,103,110,61,34,108,101,102,116,34,62,60,100,105,118,32,105,100,61,34,97,99,95,117,114,108,34,62,60,102,111,110,116,32,115,116,121,108,101,61,34,102,111,110,116,45,115,105,122,101,58,49,48,112,120,59,32,99,111,108,111,114,58,35,70,70,54,54,57,57,59,32,102,111,110,116,45,102,97,109,105,108,121,58,65,114,105,97,108,44,32,72,101,108,118,101,116,105,99,97,44,32,115,97,110,115,45,115,101,114,105,102,44,86,101,114,100,97,110,97,34,62,119,119,119,46,107,97,108,101,100,101,60,119,98,114,32,47,62,46,99,111,109,60,47,102,111,110,116,62,60,47,100,105,118,62,60,47,116,100,62,60,47,116,114,62,60,47,116,97,98,108,101,62,60,47,116,100,62,60,47,116,114,62,60,116,114,62,10,60,116,100,32,99,108,97,115,115,61,34,97,99,95,107,97,114,105,109,34,32,104,101,105,103,104,116,61,34,50,48,37,34,32,98,103,99,111,108,111,114,61,34,35,70,70,70,70,70,70,34,32,105,100,61,34,116,97,119,53,34,32,97,108,105,103,110,61,34,108,101,102,116,34,32,118,97,108,105,103,110,61,34,109,105,100,100,108,101,34,32,111,110,70,111,99,117,115,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,98,105,116,109,101,100,60,119,98,114,32,47,62,101,110,46,99,111,109,39,44,39,97,119,53,39,41,34,32,111,110,77,111,117,115,101,79,118,101,114,61,34,115,115,40,39,103,111,32,116,111,32,119,119,119,46,98,105,116,109,101,100,60,119,98,114,32,47,62,101,110,46,99,111,109,39,44,39,97,119,53,39,41,34,32,32,111,110,77,111,117,115,101,79,117,116,61,34,99,115,40,41,34,32,111,110,67,108,105,99,107,61,34,103,97,40,39,104,116,116,112,58,47,47,97,100,115,101,114,118,101,114,46,109,121,110,101,116,46,99,111,109,47,65,100,83,101,114,118,101,114,47,99,108,105,99,107,46,106,115,112,63,117,114,108,61,51,51,54,49,55,53,56,50,56,51,56,50,53,52,57,55,54,49,48)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1177518.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1177518.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1177518.js)  
```javascript
isFinite = 0;
Math.floor = 0;
Math.abs = 0;

assertEquals(4, parseInt(4.5));

assertEquals('string', typeof (new Date(9999)).toString());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1175390.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1175390.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1175390.js)  
```javascript
a = 0;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1173979.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1173979.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1173979.js)  
```javascript
var null_var = null;
var undef_var = [][0];
var boolean_var = false;
var number_var = 0;
var string_var = "";
var object_var = { foo : 0 };

assertTrue(null_var == null_var);
assertTrue(null_var == undef_var);
assertTrue(null_var != boolean_var);
assertTrue(null_var != number_var);
assertTrue(null_var != string_var);
assertTrue(null_var != object_var);

assertTrue(undef_var == null_var);
assertTrue(boolean_var != null_var);
assertTrue(number_var != null_var);
assertTrue(string_var != null_var);
assertTrue(object_var != null_var);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1114040.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1114040.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1114040.js)  
```javascript
function TestBreak() {
  var sequence = "";
  for (var a in [0,1]) {
    L: {
      for (var b in [2,3,4]) {
        break L;
      }
    }
    sequence += a;
  }
  return sequence;
}


function TestContinue() {
  var sequence = "";
  for (var a in [0,1]) {
    L: do {
      for (var b in [2,3,4]) {
        continue L;
      }
    } while (false);
    sequence += a;
  }
  return sequence;
}


assertEquals("01", TestBreak());
assertEquals("01", TestContinue());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1112051.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1112051.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1112051.js)  
```javascript
function f() { }
assertThrows("f.call.apply()");
assertThrows("f.call.apply(null)");
assertThrows("f.call.apply(null, [], 0)");
assertThrows("f.call.apply(null, [1,2,3,4,5,6,7,8,9], 0)");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1110164.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1110164.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1110164.js)  
```javascript
var o = { x: 0, f: function() { return 42; } };
delete o.x;  // go dictionary

function CallF(o) {
  return o.f();
}

for (var i = 0; i < 10; i++) assertEquals(42, CallF(o));

var caught = false;
o.f = 87;
try {
  CallF(o);
} catch (e) {
  caught = true;
  assertTrue(e instanceof TypeError);
}
assertTrue(caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1102760.js (other issue)**  
   
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1102760.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1102760.js)  
```javascript
function F() {
  return arguments.length;
}

assertEquals(0, F.apply(), "no receiver or args");
assertEquals(0, F.apply(this), "no args");
assertEquals(0, F.apply(this, []), "empty args");
assertEquals(0, F.apply(this, [], 0), "empty args, extra argument");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  
  
---   

## **regress-1066899.js (chromium issue)**  
   
**[Issue: e2e testing: Save screenshot on failure](https://crbug.com/1066899)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Platform>DevTools  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1066899.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1066899.js)  
```javascript
function Crash() {
  for (var key in [0]) {
    try { } finally { continue; }
  }
}

Crash();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-1062422.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1062422)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1062422.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1062422.js)  
```javascript
Number.prototype.__proto__ = String.prototype;
assertEquals((123).length, 0)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-1050043.js (chromium issue)**  
   
**[Issue: it making fast contact with server.](https://crbug.com/1050043)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Internals>Network  
Labels: Needs-Feedback, Arch-x86_64, Via-Wizard-NetworkDownloading  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1050043.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1050043.js)  
```javascript
function unsignedShiftRight(val, shift) {
  return val >>> shift;
}

assertEquals(        15, unsignedShiftRight(15, 0), "15 >>> 0");
assertEquals(         7, unsignedShiftRight(15, 1), "15 >>> 1");
assertEquals(         3, unsignedShiftRight(15, 2), "15 >>> 2");

assertEquals(4294967288, unsignedShiftRight(-8, 0), "-8 >>> 0");
assertEquals(2147483644, unsignedShiftRight(-8, 1), "-8 >>> 1");
assertEquals(1073741822, unsignedShiftRight(-8, 2), "-8 >>> 2");

assertEquals(         1, unsignedShiftRight(-8, 31), "-8 >>> 31");
assertEquals(4294967288, unsignedShiftRight(-8, 32), "-8 >>> 32");
assertEquals(2147483644, unsignedShiftRight(-8, 33), "-8 >>> 33");
assertEquals(1073741822, unsignedShiftRight(-8, 34), "-8 >>> 34");

assertEquals(2147483648, unsignedShiftRight(0x80000000, 0), "0x80000000 >>> 0");
assertEquals(1073741824, unsignedShiftRight(0x80000000, 1), "0x80000000 >>> 1");
assertEquals( 536870912, unsignedShiftRight(0x80000000, 2), "0x80000000 >>> 2");

assertEquals(1073741824, unsignedShiftRight(0x40000000, 0), "0x40000000 >>> 0");
assertEquals( 536870912, unsignedShiftRight(0x40000000, 1), "0x40000000 >>> 1");
assertEquals( 268435456, unsignedShiftRight(0x40000000, 2), "0x40000000 >>> 2");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-1039610.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1039610)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1039610.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1039610.js)  
```javascript
assertTrue(typeof(Debug) === 'undefined');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-1036894.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1036894)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1036894.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1036894.js)  
```javascript
assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");

function foo() {
  assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");
}
foo();

assertThrows("$=function anonymous() { /*noex*/do {} while(({ get x(x) { break ; }, set x() { (undefined);} })); }");

xeval = function(s) { eval(s); }
xeval('$=function(){L: {break L;break L;}};');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-1030466.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/1030466)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-1030466.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-1030466.js)  
```javascript
var result = (function outer() {
 with ({}) { }
 var x = 10;
 function inner() {
   return x;
 };
 return inner();
})();

assertEquals(10, result);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-996542.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/996542)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-996542.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-996542.js)  
```javascript
var zero = 0;
var one = 1;
var minus_one = -1;

assertEquals(-Infinity, 1 / (0 / -1));
assertEquals(-Infinity, one / (zero / minus_one));
assertEquals(Infinity, 1 / (0 / 1));
assertEquals(Infinity, one / (zero / one));

assertEquals(-Infinity, 1 / (-1 % 1));
assertEquals(-Infinity, one / (minus_one % one))
assertEquals(Infinity, 1 / (1 % 1));
assertEquals(Infinity, one / (one % one));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-992733.js (chromium issue)**  
   
**[Issue: Stack-overflow in __CFSearchStringROM](https://crbug.com/992733)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Platform  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz, ClusterFuzz-Verified, Test-Predator-Auto-Components  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-992733.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-992733.js)  
```javascript
assertEquals("object", typeof this);
var threw = false;
try {
  this();
} catch (e) {
  threw = true;
}
assertTrue(threw);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-990205.js (chromium issue)**  
   
**[Issue: hourglass icon mouse pointer appearing while loading pages](https://crbug.com/990205)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-990205.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-990205.js)  
```javascript
function f() {
  // Force eager compilation of x through the use of eval. The break
  // in function x should not try to break out of the enclosing while.
  return eval("while(0) function x() { break; }; 42");
};

assertThrows("f()");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-937896.js (chromium issue)**  
   
**[Issue: Collect UserCounts for the Idle Detection API usage](https://crbug.com/937896)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-937896.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-937896.js)  
```javascript
function f() {
  try {
    for (var i = 0; i < 2; i++) {
      continue;
      try {
        continue;
        continue;
      } catch (ex) {
        // Empty.
      }
    }
  } catch (e) {
    // Empty.
  }
  return 42;
}


assertEquals(42, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-925537.js (chromium issue)**  
   
**[Issue: shill: revisit and refine metrics](https://crbug.com/925537)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: OS>Systems>Network  
Labels: M-74  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-925537.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-925537.js)  
```javascript
function assertClose(expected, actual) {
  var delta = 0.00001;
  if (Math.abs(expected - actual) > delta) {
    print('Failure: Expected <' + actual + '> to be close to <' +
          expected + '>');
  }
}

assertEquals(1, Math.pow(NaN, 0));
var pinf = Number.POSITIVE_INFINITY, ninf = Number.NEGATIVE_INFINITY;
assertClose( Math.PI / 4, Math.atan2(pinf, pinf));
assertClose(-Math.PI / 4, Math.atan2(ninf, pinf));
assertClose( 3 * Math.PI / 4, Math.atan2(pinf, ninf));
assertClose(-3 * Math.PI / 4, Math.atan2(ninf, ninf));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-900966.js (chromium issue)**  
   
**[Issue: GCPW allows multiple instances of chrome to spawn leading to possible breakout of sandbox](https://crbug.com/900966)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Enterprise>CredentialProvider  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-900966.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900966.js)  
```javascript
assertTrue('abc'[10] === undefined);
String.prototype[10] = 'x';
assertEquals('abc'[10], 'x');

function f() {
  assertEquals('abc'[10], 'x');
}
f();
f();
f();
f();

assertTrue(2[11] === undefined);
Number.prototype[11] = 'y';
assertEquals(2[11], 'y');

assertTrue(true[12] === undefined);
Boolean.prototype[12] = 'z';
assertEquals(true[12], 'z');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-900055.js (chromium issue)**  
   
**[Issue: ChromeVox does not announce all macros options in tools menu](https://crbug.com/900055)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: UI>Accessibility  
Labels: M71a11ySmoke, Team-Accessibility  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-900055.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-900055.js)  
```javascript
var alias = eval;
function e(s) { return alias(s); }

assertEquals(42, e("42"));
assertEquals(Object, e("Object"));
assertEquals(e, e("e"));

var caught = false;
try {
  e('s');  // should throw exception since aliased eval is global
} catch (e) {
  caught = true;
  assertTrue(e instanceof ReferenceError);
}
assertTrue(caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-892742.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/892742)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-892742.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-892742.js)  
```javascript
function f() {
  return/* Counts as non-line-terminating whitespace */1;
};

function g() {
  return/* Counts as line-terminator whitespace.
          */2;
};

function h() {
  return// Comment doesn't include line-terminator at end.
      3;
};


assertEquals(1, f());
assertEquals(undefined, g());
assertEquals(undefined, h());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-877615.js (chromium issue)**  
   
**[Issue: Form input controls disappeared for High Sierra](https://crbug.com/877615)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Internals>Compositing>OOP-Raster  
Labels: Hotlist-Merge-Review, Needs-Feedback, Needs-Bisect, ReleaseBlock-Stable, Via-Wizard-UI, Triaged-ET, M-70, Needs-Triage-M69, merge-merged-3538, Merge-Merged-70-refsbranch-heads3538  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-877615.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-877615.js)  
```javascript
Number.prototype.toLocaleString = function() { return 'invalid'; };
assertEquals('invalid', [1].toLocaleString());  // invalid

Number.prototype.toLocaleString = 'invalid';
assertThrows(function() { [1].toLocaleString(); });  // Not callable.

delete Number.prototype.toLocaleString;
Number.prototype.toString = function() { return 'invalid' };
assertEquals([1].toLocaleString(), 'invalid');  // Uses ToObject on elements.
assertEquals([1].toString(), '1');        // Uses ToString directly on elements.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-875031.js (chromium issue)**  
   
**[Issue: Shutdown crash in ~ArcNotificationManager](https://crbug.com/875031)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: UI>Shell>Notifications  
Labels: Hotlist-Merge-Review, M-69, merge-merged-3497  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-875031.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-875031.js)  
```javascript
var caught = false;
try {
  eval("return;");
  assertTrue(false);  // should not reach here
} catch (e) {
  caught = true;
}
assertTrue(caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-874178.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/874178)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-874178.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-874178.js)  
```javascript
function foo(){}
assertTrue(Function.prototype.isPrototypeOf(foo));

foo.bar = 'hello';
assertTrue(foo.propertyIsEnumerable('bar'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-842017.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/842017)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-842017.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-842017.js)  
```javascript
function break_from_for_in() {
  L: {
    try {
      for (var x in [1,2,3]) {
        break L;
      }
    } finally {}
  }
}

function break_from_finally() {
  L: {
    try {
    } finally {
      break L;
    }
  }
}

for (var i = 0; i < 10; i++) {
  break_from_for_in();
  gc();
}

for (var j = 0; j < 10; j++) {
  break_from_finally();
  gc();
}

assertEquals(10, i);
assertEquals(10, j);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-806473.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/806473)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-806473.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-806473.js)  
```javascript
function catchThese() {
  L: {
    try {
      break L;
    } catch (e) {}
  }
}

function finallyThese() {
  L: {
    try {
      break L;
    } finally {}
  }
}


for (var i = 0; i < 10; i++) {
  catchThese();
  gc();
}

for (var j = 0; j < 10; j++) {
  finallyThese();
  gc();
}

assertEquals(10, i);
assertEquals(10, j);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-780423.js (chromium issue)**  
   
**[Issue: Add unit test for CfM autotest utility classes](https://crbug.com/780423)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Enterprise, Infra>Client>ChromeOS  
Labels: Enterprise-Triaged  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-780423.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-780423.js)  
```javascript
var Class = {
 create: function() {
   return function kurt() {
   }
 }
};

var o1 = Class.create();
var o2 = Class.create();

assertTrue(o1 !== o2, "different functions");
assertTrue(o1.prototype !== o2.prototype, "different protos");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-737588.js (chromium issue)**  
   
**[Issue: Purple Bot on chromium.perf: Android Nexus6 WebView Perf (1)](https://crbug.com/737588)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: Infra-Troopers, Performance-Sheriff-BotHealth  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-737588.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-737588.js)  
```javascript
var goog = goog || {} ;
goog.global = this;
goog.globalEval = function(script) {
  return goog.global.eval(script);
};

assertEquals(125, goog.globalEval('var foofoofoo = 125; foofoofoo'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-734862.js (chromium issue)**  
   
**[Issue: "WebContentsImplBrowserTest.DismissingBeforeUnloadDialogInvalidatesUrl" is flaky](https://crbug.com/734862)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: Test-Flaky, Via-TryFlakes  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-734862.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-734862.js)  
```javascript
function catcher(o, p) {
  try { o[p]; } catch (e) { return e; }
  throw p;
}

assertTrue(catcher(null, 'foo') instanceof TypeError);
assertTrue(catcher(void 0, 'foo') instanceof TypeError);
assertTrue(catcher(null, 123) instanceof TypeError);
assertTrue(catcher(void 0, 123) instanceof TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-682649.js (chromium issue)**  
   
**[Issue: webrtc.peerconnection.reference fails because of not enough capacity](https://crbug.com/682649)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Infra>Platform>Swarming  
Labels: Performance-Sheriff-BotHealth  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-682649.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-682649.js)  
```javascript
assertEquals(this.toString(), eval("this.toString()"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-678525.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/678525)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-678525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-678525.js)  
```javascript
assertEquals(0,  '\0'.charCodeAt(0));
assertEquals(1,  '\1'.charCodeAt(0));
assertEquals(2,  '\2'.charCodeAt(0));
assertEquals(3,  '\3'.charCodeAt(0));
assertEquals(4,  '\4'.charCodeAt(0));
assertEquals(5,  '\5'.charCodeAt(0));
assertEquals(6,  '\6'.charCodeAt(0));
assertEquals(7,  '\7'.charCodeAt(0));
assertEquals(56, '\8'.charCodeAt(0));

assertEquals('\010', '\10');
assertEquals('\011', '\11');
assertEquals('\012', '\12');
assertEquals('\013', '\13');
assertEquals('\014', '\14');
assertEquals('\015', '\15');
assertEquals('\016', '\16');
assertEquals('\017', '\17');

assertEquals('\020', '\20');
assertEquals('\021', '\21');
assertEquals('\022', '\22');
assertEquals('\023', '\23');
assertEquals('\024', '\24');
assertEquals('\025', '\25');
assertEquals('\026', '\26');
assertEquals('\027', '\27');

assertEquals(73,  '\111'.charCodeAt(0));
assertEquals(105, '\151'.charCodeAt(0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-676025.js (chromium issue)**  
   
**[Issue: Quick swipe fails to un-hide shelf](https://crbug.com/676025)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: UI>Shell>Shelf  
Labels: Proj-MaterialDesign-CrOS, Touch-Friendly-Launcher, Touch-Friendly-Launcher-Triaged  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-676025.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-676025.js)  
```javascript
var result;
try { eval('a=/(/'); } catch (e) { result = e; }
assertEquals('object', typeof result);
assertTrue(result instanceof SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-674753.js (chromium issue)**  
   
**[Issue: Cancelled XHR has no information](https://crbug.com/674753)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Blink>Network>XHR  
Labels: M-55, Needs-Feedback, Hotlist-Interop, TE-NeedsTriageHelp, Via-Wizard-API  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-674753.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-674753.js)  
```javascript
var undetectable = %GetUndetectable();

assertTrue(typeof 0 == 'number');
assertTrue(typeof 0 === 'number');
assertFalse(typeof 0 != 'number');
assertFalse(typeof 0 !== 'number');
assertTrue(typeof 1.2 == 'number');
assertTrue(typeof 1.2 === 'number');
assertFalse(typeof 1.2 != 'number');
assertFalse(typeof 1.2 !== 'number');
assertTrue(typeof 'x' != 'number');
assertTrue(typeof 'x' !== 'number');
assertFalse(typeof 'x' == 'number');
assertFalse(typeof 'x' === 'number');
assertTrue(typeof Object() != 'number');
assertTrue(typeof Object() !== 'number');
assertFalse(typeof Object() == 'number');
assertFalse(typeof Object() === 'number');

assertTrue(typeof 'x' == 'string');
assertTrue(typeof 'x' === 'string');
assertFalse(typeof 'x' != 'string');
assertFalse(typeof 'x' !== 'string');
assertTrue(typeof ('x' + 'x') == 'string');
assertTrue(typeof ('x' + 'x') === 'string');
assertFalse(typeof ('x' + 'x') != 'string');
assertFalse(typeof ('x' + 'x') !== 'string');
assertTrue(typeof 1 != 'string');
assertTrue(typeof 1 !== 'string');
assertFalse(typeof 1 == 'string');
assertFalse(typeof 1 === 'string');
assertTrue(typeof Object() != 'string');
assertTrue(typeof Object() !== 'string');
assertFalse(typeof Object() == 'string');
assertFalse(typeof Object() === 'string');

assertTrue(typeof true == 'boolean');
assertTrue(typeof true === 'boolean');
assertFalse(typeof true != 'boolean');
assertFalse(typeof true !== 'boolean');
assertTrue(typeof false == 'boolean');
assertTrue(typeof false === 'boolean');
assertFalse(typeof false != 'boolean');
assertFalse(typeof false !== 'boolean');
assertTrue(typeof 1 != 'boolean');
assertTrue(typeof 1 !== 'boolean');
assertFalse(typeof 1 == 'boolean');
assertFalse(typeof 1 === 'boolean');
assertTrue(typeof 'x' != 'boolean');
assertTrue(typeof 'x' !== 'boolean');
assertFalse(typeof 'x' == 'boolean');
assertFalse(typeof 'x' === 'boolean');
assertTrue(typeof Object() != 'boolean');
assertTrue(typeof Object() !== 'boolean');
assertFalse(typeof Object() == 'boolean');
assertFalse(typeof Object() === 'boolean');

assertTrue(typeof void 0 == 'undefined');
assertTrue(typeof void 0 === 'undefined');
assertFalse(typeof void 0 != 'undefined');
assertFalse(typeof void 0 !== 'undefined');
assertTrue(typeof 1 != 'undefined');
assertTrue(typeof 1 !== 'undefined');
assertFalse(typeof 1 == 'undefined');
assertFalse(typeof 1 === 'undefined');
assertTrue(typeof null != 'undefined');
assertTrue(typeof null !== 'undefined');
assertFalse(typeof null == 'undefined');
assertFalse(typeof null === 'undefined');
assertTrue(typeof Object() != 'undefined');
assertTrue(typeof Object() !== 'undefined');
assertFalse(typeof Object() == 'undefined');
assertFalse(typeof Object() === 'undefined');
assertTrue(typeof undetectable == 'undefined');
assertTrue(typeof undetectable === 'undefined');
assertFalse(typeof undetectable != 'undefined');
assertFalse(typeof undetectable !== 'undefined');

assertTrue(typeof Object == 'function');
assertTrue(typeof Object === 'function');
assertFalse(typeof Object != 'function');
assertFalse(typeof Object !== 'function');
assertTrue(typeof 1 != 'function');
assertTrue(typeof 1 !== 'function');
assertFalse(typeof 1 == 'function');
assertFalse(typeof 1 === 'function');
assertTrue(typeof Object() != 'function');
assertTrue(typeof Object() !== 'function');
assertFalse(typeof Object() == 'function');
assertFalse(typeof Object() === 'function');
assertTrue(typeof undetectable != 'function');
assertTrue(typeof undetectable !== 'function');
assertFalse(typeof undetectable == 'function');
assertFalse(typeof undetectable === 'function');

assertTrue(typeof Object() == 'object');
assertTrue(typeof Object() === 'object');
assertFalse(typeof Object() != 'object');
assertFalse(typeof Object() !== 'object');
assertTrue(typeof new String('x') == 'object');
assertTrue(typeof new String('x') === 'object');
assertFalse(typeof new String('x') != 'object');
assertFalse(typeof new String('x') !== 'object');
assertTrue(typeof ['x'] == 'object');
assertTrue(typeof ['x'] === 'object');
assertFalse(typeof ['x'] != 'object');
assertFalse(typeof ['x'] !== 'object');
assertTrue(typeof null == 'object');
assertTrue(typeof null === 'object');
assertFalse(typeof null != 'object');
assertFalse(typeof null !== 'object');
assertTrue(typeof 1 != 'object');
assertTrue(typeof 1 !== 'object');
assertFalse(typeof 1 == 'object');
assertFalse(typeof 1 === 'object');
assertTrue(typeof 'x' != 'object');
assertTrue(typeof 'x' !== 'object');
assertFalse(typeof 'x' == 'object');  // bug #674753
assertFalse(typeof 'x' === 'object');
assertTrue(typeof Object != 'object');
assertTrue(typeof Object !== 'object');
assertFalse(typeof Object == 'object');
assertFalse(typeof Object === 'object');
assertTrue(typeof undetectable != 'object');
assertTrue(typeof undetectable !== 'object');
assertFalse(typeof undetectable == 'object');
assertFalse(typeof undetectable === 'object');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-670147.js (chromium issue)**  
   
**[Issue: Add VR Shell unit tests to Android Tryservers](https://crbug.com/670147)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Infra>Client>Android, UI>Browser>VR  
Labels: Proj-VR, VR-Test, Proj-XR, Proj-XR-VR  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-670147.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-670147.js)  
```javascript
function XXX(x) {
  var k = delete x;
  return k;
}

assertFalse(XXX('Hello'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-667061.js (chromium issue)**  
   
**[Issue: "virtual/rootlayerscrolls/scrollingcoordinator/non-fast-scrollable-visibility-change.html" is flaky](https://crbug.com/667061)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: Blink>Paint>Invalidation  
Labels: Test-Flaky, Sheriff-Chromium, Via-TryFlakes  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-667061.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-667061.js)  
```javascript
var caught = false;
try {
  (('foo'))();
} catch (o) {
  assertTrue(o instanceof TypeError);
  caught = true;
}
assertTrue(caught);


function h(o) {
  return o.x();
}

var caught = false;
try {
  h({ x: 1 });
} catch (o) {
  assertTrue(o instanceof TypeError);
  caught = true;
}
assertTrue(caught);


function g(o) {
  return o.x();
}

function O(x) { this.x = x; };
var o = new O(function() { return 1; });
assertEquals(1, g(o));  // go monomorphic
assertEquals(1, g(o));  // stay monomorphic

var caught = false;
try {
  g(new O(3));
} catch (o) {
  assertTrue(o instanceof TypeError);
  caught = true;
}
assertTrue(caught);


function f(o) {
  return o.x();
}

assertEquals(1, f({ x: function () { return 1; }}));  // go monomorphic
assertEquals(2, f({ x: function () { return 2; }}));  // go megamorphic
assertEquals(3, f({ x: function () { return 3; }}));  // stay megamorphic

var caught = false;
try {
  f({ x: 4 });
} catch (o) {
  assertTrue(o instanceof TypeError);
  caught = true;
}
assertTrue(caught);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-666721.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/666721)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-666721.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-666721.js)  
```javascript
function len0(a) { return a.length; }
function len1(a) { return a.length; }
function len2(a) { return a.length; }
function len3(a) { return a.length; }

assertEquals(0, len0([]));
assertEquals(1, len0({length:1}));
assertEquals(2, len0([1,2]));
assertEquals(3, len0('123'));

assertEquals(0, len1(''));
assertEquals(1, len1({length:1}));
assertEquals(2, len1('12'));
assertEquals(3, len1([1,2,3]));

assertEquals(0, len2({length:0}));
assertEquals(1, len2([1]));
assertEquals(2, len2({length:2}));
assertEquals(3, len2([1,2,3]));
assertEquals(4, len2('1234'));

assertEquals(0, len3({length:0}));
assertEquals(1, len3('1'));
assertEquals(2, len3({length:2}));
assertEquals(3, len3('123'));
assertEquals(4, len3([1,2,3,4]));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-662254.js (chromium issue)**  
   
**[Issue: Accessibility issue: Hypertexts contained w/in an element w/ a "click-and-mouse-related" event listener are "clickable"](https://crbug.com/662254)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: UI>Accessibility>Compatibility  
Labels: Via-Wizard-Javascript, win-a11y, Team-Accessibility  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-662254.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-662254.js)  
```javascript
function f() {
  for (var c in []) { }
}

f();


function g() {
  var c;
  for (c in []) { }
}

g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   

## **regress-588599.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/588599)**  
**[Commit: Included mjsunit JavaScript test suite and C++ unit tests.](https://chromium.googlesource.com/v8/v8/+/c42f582)**  
  
Date(Commit): Fri Aug 22 13:33:59 2008  
Components: None  
Labels: None  
Code Review: [http://v8.googlecode.com/svn/branches/bleeding_edge@16](http://v8.googlecode.com/svn/branches/bleeding_edge@16)  
Regress: [mjsunit/regress/regress-588599.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-588599.js)  
```javascript
assertFalse(Infinity == -Infinity);
assertEquals(Infinity, 1 / 1e-9999);
assertEquals(-Infinity, 1 / -1e-9999);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c42f582^!)  
[SConstruct](https://cs.chromium.org/chromium/src/v8/SConstruct?cl=c42f582)  
[public/debug.h](https://cs.chromium.org/chromium/src/v8/public/debug.h?cl=c42f582)  
[public/v8.h](https://cs.chromium.org/chromium/src/v8/public/v8.h?cl=c42f582)  
[samples/SConscript](https://cs.chromium.org/chromium/src/v8/samples/SConscript?cl=c42f582)  
[samples/process.cc](https://cs.chromium.org/chromium/src/v8/samples/process.cc?cl=c42f582)  
...  
  

---   
