# V8Harvest  
The Harvest of V8 regress in 2015.  
  

## **regress-4640.js (v8 issue)**  
   
**[Issue: illegal access exception from Date strings](https://crbug.com/v8/4640)**  
**[Commit: Fix 'illegal access' in Date constructor edge case](https://chromium.googlesource.com/v8/v8/+/a9c7910)**  
  
Date(Commit): Wed Dec 30 23:54:59 2015  
Code Review: [https://codereview.chromium.org/1545883003](https://codereview.chromium.org/1545883003)  
Regress: [mjsunit/regress/regress-4640.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4640.js)  
```javascript
assertTrue(new Date('275760-10-14') == 'Invalid Date');
assertTrue(new Date('275760-09-23') == 'Invalid Date');
assertTrue(new Date('+275760-09-24') == 'Invalid Date');
assertTrue(new Date('+275760-10-13') == 'Invalid Date');

assertTrue(new Date('275760-09-24') == 'Invalid Date');
assertTrue(new Date('275760-10-13') == 'Invalid Date');
assertTrue(new Date('+275760-10-13 ') == 'Invalid Date');

assertTrue(new Date('100000-10-13') != 'Invalid Date');
assertTrue(new Date('+100000-10-13') != 'Invalid Date');
assertTrue(new Date('+100000-10-13 ') != 'Invalid Date');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9c7910^!)  
[src/date.h](https://cs.chromium.org/chromium/src/v8/src/date.h?cl=a9c7910)  
[test/mjsunit/regress/regress-4640.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4640.js?cl=a9c7910)  
  

---   

## **regress-crbug-571064.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSObject()) in src/objects](https://crbug.com/571064)**  
**[Commit: [crankshaft] Don't inline array resize operations if receiver's proto is not a JSObject.](https://chromium.googlesource.com/v8/v8/+/bae0d6c)**  
  
Date(Commit): Tue Dec 29 14:35:18 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1548363003](https://codereview.chromium.org/1548363003)  
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
};
%PrepareFunctionForOptimization(CallFuncWithPrototype);
CallFunc([]);
CallFunc([]);
%OptimizeFunctionOnNextCall(CallFuncWithPrototype);
CallFuncWithPrototype();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bae0d6c^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=bae0d6c)  
[test/mjsunit/regress/regress-crbug-571064.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-571064.js?cl=bae0d6c)  
  

---   

## **regress-crbug-571370.js (chromium issue)**  
   
**[Issue: properties()->IsDictionary() == map()->is_dictionary_map() in src/objects-inl.h](https://crbug.com/571370)**  
**[Commit: [ic] Fixed receiver_map register trashing in KeyedStoreIC megamorphic.](https://chromium.googlesource.com/v8/v8/+/c1aded3)**  
  
Date(Commit): Tue Dec 29 12:52:13 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1546323002](https://codereview.chromium.org/1546323002)  
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
...  
  

---   

## **regress-crbug-572590.js (chromium issue)**  
   
**[Issue: p->IsSmi() in src/objects-debug.cc](https://crbug.com/572590)**  
**[Commit: Only verify in-object fields in fast properties case.](https://chromium.googlesource.com/v8/v8/+/2fcf3aa)**  
  
Date(Commit): Tue Dec 29 11:20:52 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1553463002](https://codereview.chromium.org/1553463002)  
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
  

---   

## **regress-crbug-561973.js (chromium issue)**  
   
**[Issue: result == value >= kMinValue && value <= kMaxValue in src/objects.h](https://crbug.com/561973)**  
**[Commit: Fix UTC offset computation in date parser.](https://chromium.googlesource.com/v8/v8/+/37b5ebc)**  
  
Date(Commit): Thu Dec 17 16:29:33 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1532573003](https://codereview.chromium.org/1532573003)  
Regress: [mjsunit/regress/regress-crbug-561973.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-561973.js)  
```javascript
Date.parse('Sat, 01 Jan 100 08:00:00 UT-59011430400000');  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/37b5ebc^!)  
[src/dateparser.cc](https://cs.chromium.org/chromium/src/v8/src/dateparser.cc?cl=37b5ebc)  
[test/mjsunit/regress/regress-crbug-561973.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-561973.js?cl=37b5ebc)  
  

---   

## **regress-crbug-570241.js (chromium issue)**  
   
**[Issue: Stack-buffer-underflow in v8::internal::QuickCheckDetails::Advance](https://crbug.com/570241)**  
**[Commit: [regexp] clear QuickCheckDetails for backward reads.](https://chromium.googlesource.com/v8/v8/+/65d3009)**  
  
Date(Commit): Wed Dec 16 13:43:23 2015  
Components: Blink>JavaScript  
Labels: Merge-na, M-48, Reproducible, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1528333002](https://codereview.chromium.org/1528333002)  
Regress: [mjsunit/regress/regress-crbug-570241.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-570241.js)  
```javascript
assertTrue(/(?<=12345123451234512345)/.test("12345123451234512345"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/65d3009^!)  
[src/regexp/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.cc?cl=65d3009)  
[src/regexp/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/regexp/jsregexp.h?cl=65d3009)  
[test/mjsunit/regress/regress-crbug-570241.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-570241.js?cl=65d3009)  
  

---   

## **regress-4595.js (v8 issue)**  
   
**[Issue: Performance regression in Chrome 47 / V8 4.7.80.23 on generated ES6 code ](https://crbug.com/v8/4595)**  
**[Commit: Fix FuncNameInferrer usage in ParseAssignmentExpression](https://chromium.googlesource.com/v8/v8/+/eb67f85)**  
  
Date(Commit): Thu Dec 10 19:19:35 2015  
Code Review: [https://codereview.chromium.org/1507283003](https://codereview.chromium.org/1507283003)  
Regress: [mjsunit/regress/regress-4595.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4595.js)  
```javascript
var obj = {
foo0: () => {},
foo1: () => {},
foo2: () => {},
foo3: () => {},
foo4: () => {},
foo5: () => {},
foo6: () => {},
foo7: () => {},
foo8: () => {},
foo9: () => {},
foo10: () => {},
foo11: () => {},
foo12: () => {},
foo13: () => {},
foo14: () => {},
foo15: () => {},
foo16: () => {},
foo17: () => {},
foo18: () => {},
foo19: () => {},
foo20: () => {},
foo21: () => {},
foo22: () => {},
foo23: () => {},
foo24: () => {},
foo25: () => {},
foo26: () => {},
foo27: () => {},
foo28: () => {},
foo29: () => {},
foo30: () => {},
foo31: () => {},
foo32: () => {},
foo33: () => {},
foo34: () => {},
foo35: () => {},
foo36: () => {},
foo37: () => {},
foo38: () => {},
foo39: () => {},
foo40: () => {},
foo41: () => {},
foo42: () => {},
foo43: () => {},
foo44: () => {},
foo45: () => {},
foo46: () => {},
foo47: () => {},
foo48: () => {},
foo49: () => {},
foo50: () => {},
foo51: () => {},
foo52: () => {},
foo53: () => {},
foo54: () => {},
foo55: () => {},
foo56: () => {},
foo57: () => {},
foo58: () => {},
foo59: () => {},
foo60: () => {},
foo61: () => {},
foo62: () => {},
foo63: () => {},
foo64: () => {},
foo65: () => {},
foo66: () => {},
foo67: () => {},
foo68: () => {},
foo69: () => {},
foo70: () => {},
foo71: () => {},
foo72: () => {},
foo73: () => {},
foo74: () => {},
foo75: () => {},
foo76: () => {},
foo77: () => {},
foo78: () => {},
foo79: () => {},
foo80: () => {},
foo81: () => {},
foo82: () => {},
foo83: () => {},
foo84: () => {},
foo85: () => {},
foo86: () => {},
foo87: () => {},
foo88: () => {},
foo89: () => {},
foo90: () => {},
foo91: () => {},
foo92: () => {},
foo93: () => {},
foo94: () => {},
foo95: () => {},
foo96: () => {},
foo97: () => {},
foo98: () => {},
foo99: () => {},
foo100: () => {},
foo101: () => {},
foo102: () => {},
foo103: () => {},
foo104: () => {},
foo105: () => {},
foo106: () => {},
foo107: () => {},
foo108: () => {},
foo109: () => {},
foo110: () => {},
foo111: () => {},
foo112: () => {},
foo113: () => {},
foo114: () => {},
foo115: () => {},
foo116: () => {},
foo117: () => {},
foo118: () => {},
foo119: () => {},
foo120: () => {},
foo121: () => {},
foo122: () => {},
foo123: () => {},
foo124: () => {},
foo125: () => {},
foo126: () => {},
foo127: () => {},
foo128: () => {},
foo129: () => {},
foo130: () => {},
foo131: () => {},
foo132: () => {},
foo133: () => {},
foo134: () => {},
foo135: () => {},
foo136: () => {},
foo137: () => {},
foo138: () => {},
foo139: () => {},
foo140: () => {},
foo141: () => {},
foo142: () => {},
foo143: () => {},
foo144: () => {},
foo145: () => {},
foo146: () => {},
foo147: () => {},
foo148: () => {},
foo149: () => {},
foo150: () => {},
foo151: () => {},
foo152: () => {},
foo153: () => {},
foo154: () => {},
foo155: () => {},
foo156: () => {},
foo157: () => {},
foo158: () => {},
foo159: () => {},
foo160: () => {},
foo161: () => {},
foo162: () => {},
foo163: () => {},
foo164: () => {},
foo165: () => {},
foo166: () => {},
foo167: () => {},
foo168: () => {},
foo169: () => {},
foo170: () => {},
foo171: () => {},
foo172: () => {},
foo173: () => {},
foo174: () => {},
foo175: () => {},
foo176: () => {},
foo177: () => {},
foo178: () => {},
foo179: () => {},
foo180: () => {},
foo181: () => {},
foo182: () => {},
foo183: () => {},
foo184: () => {},
foo185: () => {},
foo186: () => {},
foo187: () => {},
foo188: () => {},
foo189: () => {},
foo190: () => {},
foo191: () => {},
foo192: () => {},
foo193: () => {},
foo194: () => {},
foo195: () => {},
foo196: () => {},
foo197: () => {},
foo198: () => {},
foo199: () => {},
foo200: () => {},
foo201: () => {},
foo202: () => {},
foo203: () => {},
foo204: () => {},
foo205: () => {},
foo206: () => {},
foo207: () => {},
foo208: () => {},
foo209: () => {},
foo210: () => {},
foo211: () => {},
foo212: () => {},
foo213: () => {},
foo214: () => {},
foo215: () => {},
foo216: () => {},
foo217: () => {},
foo218: () => {},
foo219: () => {},
foo220: () => {},
foo221: () => {},
foo222: () => {},
foo223: () => {},
foo224: () => {},
foo225: () => {},
foo226: () => {},
foo227: () => {},
foo228: () => {},
foo229: () => {},
foo230: () => {},
foo231: () => {},
foo232: () => {},
foo233: () => {},
foo234: () => {},
foo235: () => {},
foo236: () => {},
foo237: () => {},
foo238: () => {},
foo239: () => {},
foo240: () => {},
foo241: () => {},
foo242: () => {},
foo243: () => {},
foo244: () => {},
foo245: () => {},
foo246: () => {},
foo247: () => {},
foo248: () => {},
foo249: () => {},
foo250: () => {},
foo251: () => {},
foo252: () => {},
foo253: () => {},
foo254: () => {},
foo255: () => {},
foo256: () => {},
foo257: () => {},
foo258: () => {},
foo259: () => {},
foo260: () => {},
foo261: () => {},
foo262: () => {},
foo263: () => {},
foo264: () => {},
foo265: () => {},
foo266: () => {},
foo267: () => {},
foo268: () => {},
foo269: () => {},
foo270: () => {},
foo271: () => {},
foo272: () => {},
foo273: () => {},
foo274: () => {},
foo275: () => {},
foo276: () => {},
foo277: () => {},
foo278: () => {},
foo279: () => {},
foo280: () => {},
foo281: () => {},
foo282: () => {},
foo283: () => {},
foo284: () => {},
foo285: () => {},
foo286: () => {},
foo287: () => {},
foo288: () => {},
foo289: () => {},
foo290: () => {},
foo291: () => {},
foo292: () => {},
foo293: () => {},
foo294: () => {},
foo295: () => {},
foo296: () => {},
foo297: () => {},
foo298: () => {},
foo299: () => {},
foo300: () => {},
foo301: () => {},
foo302: () => {},
foo303: () => {},
foo304: () => {},
foo305: () => {},
foo306: () => {},
foo307: () => {},
foo308: () => {},
foo309: () => {},
foo310: () => {},
foo311: () => {},
foo312: () => {},
foo313: () => {},
foo314: () => {},
foo315: () => {},
foo316: () => {},
foo317: () => {},
foo318: () => {},
foo319: () => {},
foo320: () => {},
foo321: () => {},
foo322: () => {},
foo323: () => {},
foo324: () => {},
foo325: () => {},
foo326: () => {},
foo327: () => {},
foo328: () => {},
foo329: () => {},
foo330: () => {},
foo331: () => {},
foo332: () => {},
foo333: () => {},
foo334: () => {},
foo335: () => {},
foo336: () => {},
foo337: () => {},
foo338: () => {},
foo339: () => {},
foo340: () => {},
foo341: () => {},
foo342: () => {},
foo343: () => {},
foo344: () => {},
foo345: () => {},
foo346: () => {},
foo347: () => {},
foo348: () => {},
foo349: () => {},
foo350: () => {},
foo351: () => {},
foo352: () => {},
foo353: () => {},
foo354: () => {},
foo355: () => {},
foo356: () => {},
foo357: () => {},
foo358: () => {},
foo359: () => {},
foo360: () => {},
foo361: () => {},
foo362: () => {},
foo363: () => {},
foo364: () => {},
foo365: () => {},
foo366: () => {},
foo367: () => {},
foo368: () => {},
foo369: () => {},
foo370: () => {},
foo371: () => {},
foo372: () => {},
foo373: () => {},
foo374: () => {},
foo375: () => {},
foo376: () => {},
foo377: () => {},
foo378: () => {},
foo379: () => {},
foo380: () => {},
foo381: () => {},
foo382: () => {},
foo383: () => {},
foo384: () => {},
foo385: () => {},
foo386: () => {},
foo387: () => {},
foo388: () => {},
foo389: () => {},
foo390: () => {},
foo391: () => {},
foo392: () => {},
foo393: () => {},
foo394: () => {},
foo395: () => {},
foo396: () => {},
foo397: () => {},
foo398: () => {},
foo399: () => {},
foo400: () => {},
foo401: () => {},
foo402: () => {},
foo403: () => {},
foo404: () => {},
foo405: () => {},
foo406: () => {},
foo407: () => {},
foo408: () => {},
foo409: () => {},
foo410: () => {},
foo411: () => {},
foo412: () => {},
foo413: () => {},
foo414: () => {},
foo415: () => {},
foo416: () => {},
foo417: () => {},
foo418: () => {},
foo419: () => {},
foo420: () => {},
foo421: () => {},
foo422: () => {},
foo423: () => {},
foo424: () => {},
foo425: () => {},
foo426: () => {},
foo427: () => {},
foo428: () => {},
foo429: () => {},
foo430: () => {},
foo431: () => {},
foo432: () => {},
foo433: () => {},
foo434: () => {},
foo435: () => {},
foo436: () => {},
foo437: () => {},
foo438: () => {},
foo439: () => {},
foo440: () => {},
foo441: () => {},
foo442: () => {},
foo443: () => {},
foo444: () => {},
foo445: () => {},
foo446: () => {},
foo447: () => {},
foo448: () => {},
foo449: () => {},
foo450: () => {},
foo451: () => {},
foo452: () => {},
foo453: () => {},
foo454: () => {},
foo455: () => {},
foo456: () => {},
foo457: () => {},
foo458: () => {},
foo459: () => {},
foo460: () => {},
foo461: () => {},
foo462: () => {},
foo463: () => {},
foo464: () => {},
foo465: () => {},
foo466: () => {},
foo467: () => {},
foo468: () => {},
foo469: () => {},
foo470: () => {},
foo471: () => {},
foo472: () => {},
foo473: () => {},
foo474: () => {},
foo475: () => {},
foo476: () => {},
foo477: () => {},
foo478: () => {},
foo479: () => {},
foo480: () => {},
foo481: () => {},
foo482: () => {},
foo483: () => {},
foo484: () => {},
foo485: () => {},
foo486: () => {},
foo487: () => {},
foo488: () => {},
foo489: () => {},
foo490: () => {},
foo491: () => {},
foo492: () => {},
foo493: () => {},
foo494: () => {},
foo495: () => {},
foo496: () => {},
foo497: () => {},
foo498: () => {},
foo499: () => {},
foo500: () => {},
foo501: () => {},
foo502: () => {},
foo503: () => {},
foo504: () => {},
foo505: () => {},
foo506: () => {},
foo507: () => {},
foo508: () => {},
foo509: () => {},
foo510: () => {},
foo511: () => {},
foo512: () => {},
foo513: () => {},
foo514: () => {},
foo515: () => {},
foo516: () => {},
foo517: () => {},
foo518: () => {},
foo519: () => {},
foo520: () => {},
foo521: () => {},
foo522: () => {},
foo523: () => {},
foo524: () => {},
foo525: () => {},
foo526: () => {},
foo527: () => {},
foo528: () => {},
foo529: () => {},
foo530: () => {},
foo531: () => {},
foo532: () => {},
foo533: () => {},
foo534: () => {},
foo535: () => {},
foo536: () => {},
foo537: () => {},
foo538: () => {},
foo539: () => {},
foo540: () => {},
foo541: () => {},
foo542: () => {},
foo543: () => {},
foo544: () => {},
foo545: () => {},
foo546: () => {},
foo547: () => {},
foo548: () => {},
foo549: () => {},
foo550: () => {},
foo551: () => {},
foo552: () => {},
foo553: () => {},
foo554: () => {},
foo555: () => {},
foo556: () => {},
foo557: () => {},
foo558: () => {},
foo559: () => {},
foo560: () => {},
foo561: () => {},
foo562: () => {},
foo563: () => {},
foo564: () => {},
foo565: () => {},
foo566: () => {},
foo567: () => {},
foo568: () => {},
foo569: () => {},
foo570: () => {},
foo571: () => {},
foo572: () => {},
foo573: () => {},
foo574: () => {},
foo575: () => {},
foo576: () => {},
foo577: () => {},
foo578: () => {},
foo579: () => {},
foo580: () => {},
foo581: () => {},
foo582: () => {},
foo583: () => {},
foo584: () => {},
foo585: () => {},
foo586: () => {},
foo587: () => {},
foo588: () => {},
foo589: () => {},
foo590: () => {},
foo591: () => {},
foo592: () => {},
foo593: () => {},
foo594: () => {},
foo595: () => {},
foo596: () => {},
foo597: () => {},
foo598: () => {},
foo599: () => {},
foo600: () => {},
foo601: () => {},
foo602: () => {},
foo603: () => {},
foo604: () => {},
foo605: () => {},
foo606: () => {},
foo607: () => {},
foo608: () => {},
foo609: () => {},
foo610: () => {},
foo611: () => {},
foo612: () => {},
foo613: () => {},
foo614: () => {},
foo615: () => {},
foo616: () => {},
foo617: () => {},
foo618: () => {},
foo619: () => {},
foo620: () => {},
foo621: () => {},
foo622: () => {},
foo623: () => {},
foo624: () => {},
foo625: () => {},
foo626: () => {},
foo627: () => {},
foo628: () => {},
foo629: () => {},
foo630: () => {},
foo631: () => {},
foo632: () => {},
foo633: () => {},
foo634: () => {},
foo635: () => {},
foo636: () => {},
foo637: () => {},
foo638: () => {},
foo639: () => {},
foo640: () => {},
foo641: () => {},
foo642: () => {},
foo643: () => {},
foo644: () => {},
foo645: () => {},
foo646: () => {},
foo647: () => {},
foo648: () => {},
foo649: () => {},
foo650: () => {},
foo651: () => {},
foo652: () => {},
foo653: () => {},
foo654: () => {},
foo655: () => {},
foo656: () => {},
foo657: () => {},
foo658: () => {},
foo659: () => {},
foo660: () => {},
foo661: () => {},
foo662: () => {},
foo663: () => {},
foo664: () => {},
foo665: () => {},
foo666: () => {},
foo667: () => {},
foo668: () => {},
foo669: () => {},
foo670: () => {},
foo671: () => {},
foo672: () => {},
foo673: () => {},
foo674: () => {},
foo675: () => {},
foo676: () => {},
foo677: () => {},
foo678: () => {},
foo679: () => {},
foo680: () => {},
foo681: () => {},
foo682: () => {},
foo683: () => {},
foo684: () => {},
foo685: () => {},
foo686: () => {},
foo687: () => {},
foo688: () => {},
foo689: () => {},
foo690: () => {},
foo691: () => {},
foo692: () => {},
foo693: () => {},
foo694: () => {},
foo695: () => {},
foo696: () => {},
foo697: () => {},
foo698: () => {},
foo699: () => {},
foo700: () => {},
foo701: () => {},
foo702: () => {},
foo703: () => {},
foo704: () => {},
foo705: () => {},
foo706: () => {},
foo707: () => {},
foo708: () => {},
foo709: () => {},
foo710: () => {},
foo711: () => {},
foo712: () => {},
foo713: () => {},
foo714: () => {},
foo715: () => {},
foo716: () => {},
foo717: () => {},
foo718: () => {},
foo719: () => {},
foo720: () => {},
foo721: () => {},
foo722: () => {},
foo723: () => {},
foo724: () => {},
foo725: () => {},
foo726: () => {},
foo727: () => {},
foo728: () => {},
foo729: () => {},
foo730: () => {},
foo731: () => {},
foo732: () => {},
foo733: () => {},
foo734: () => {},
foo735: () => {},
foo736: () => {},
foo737: () => {},
foo738: () => {},
foo739: () => {},
foo740: () => {},
foo741: () => {},
foo742: () => {},
foo743: () => {},
foo744: () => {},
foo745: () => {},
foo746: () => {},
foo747: () => {},
foo748: () => {},
foo749: () => {},
foo750: () => {},
foo751: () => {},
foo752: () => {},
foo753: () => {},
foo754: () => {},
foo755: () => {},
foo756: () => {},
foo757: () => {},
foo758: () => {},
foo759: () => {},
foo760: () => {},
foo761: () => {},
foo762: () => {},
foo763: () => {},
foo764: () => {},
foo765: () => {},
foo766: () => {},
foo767: () => {},
foo768: () => {},
foo769: () => {},
foo770: () => {},
foo771: () => {},
foo772: () => {},
foo773: () => {},
foo774: () => {},
foo775: () => {},
foo776: () => {},
foo777: () => {},
foo778: () => {},
foo779: () => {},
foo780: () => {},
foo781: () => {},
foo782: () => {},
foo783: () => {},
foo784: () => {},
foo785: () => {},
foo786: () => {},
foo787: () => {},
foo788: () => {},
foo789: () => {},
foo790: () => {},
foo791: () => {},
foo792: () => {},
foo793: () => {},
foo794: () => {},
foo795: () => {},
foo796: () => {},
foo797: () => {},
foo798: () => {},
foo799: () => {},
foo800: () => {},
foo801: () => {},
foo802: () => {},
foo803: () => {},
foo804: () => {},
foo805: () => {},
foo806: () => {},
foo807: () => {},
foo808: () => {},
foo809: () => {},
foo810: () => {},
foo811: () => {},
foo812: () => {},
foo813: () => {},
foo814: () => {},
foo815: () => {},
foo816: () => {},
foo817: () => {},
foo818: () => {},
foo819: () => {},
foo820: () => {},
foo821: () => {},
foo822: () => {},
foo823: () => {},
foo824: () => {},
foo825: () => {},
foo826: () => {},
foo827: () => {},
foo828: () => {},
foo829: () => {},
foo830: () => {},
foo831: () => {},
foo832: () => {},
foo833: () => {},
foo834: () => {},
foo835: () => {},
foo836: () => {},
foo837: () => {},
foo838: () => {},
foo839: () => {},
foo840: () => {},
foo841: () => {},
foo842: () => {},
foo843: () => {},
foo844: () => {},
foo845: () => {},
foo846: () => {},
foo847: () => {},
foo848: () => {},
foo849: () => {},
foo850: () => {},
foo851: () => {},
foo852: () => {},
foo853: () => {},
foo854: () => {},
foo855: () => {},
foo856: () => {},
foo857: () => {},
foo858: () => {},
foo859: () => {},
foo860: () => {},
foo861: () => {},
foo862: () => {},
foo863: () => {},
foo864: () => {},
foo865: () => {},
foo866: () => {},
foo867: () => {},
foo868: () => {},
foo869: () => {},
foo870: () => {},
foo871: () => {},
foo872: () => {},
foo873: () => {},
foo874: () => {},
foo875: () => {},
foo876: () => {},
foo877: () => {},
foo878: () => {},
foo879: () => {},
foo880: () => {},
foo881: () => {},
foo882: () => {},
foo883: () => {},
foo884: () => {},
foo885: () => {},
foo886: () => {},
foo887: () => {},
foo888: () => {},
foo889: () => {},
foo890: () => {},
foo891: () => {},
foo892: () => {},
foo893: () => {},
foo894: () => {},
foo895: () => {},
foo896: () => {},
foo897: () => {},
foo898: () => {},
foo899: () => {},
foo900: () => {},
foo901: () => {},
foo902: () => {},
foo903: () => {},
foo904: () => {},
foo905: () => {},
foo906: () => {},
foo907: () => {},
foo908: () => {},
foo909: () => {},
foo910: () => {},
foo911: () => {},
foo912: () => {},
foo913: () => {},
foo914: () => {},
foo915: () => {},
foo916: () => {},
foo917: () => {},
foo918: () => {},
foo919: () => {},
foo920: () => {},
foo921: () => {},
foo922: () => {},
foo923: () => {},
foo924: () => {},
foo925: () => {},
foo926: () => {},
foo927: () => {},
foo928: () => {},
foo929: () => {},
foo930: () => {},
foo931: () => {},
foo932: () => {},
foo933: () => {},
foo934: () => {},
foo935: () => {},
foo936: () => {},
foo937: () => {},
foo938: () => {},
foo939: () => {},
foo940: () => {},
foo941: () => {},
foo942: () => {},
foo943: () => {},
foo944: () => {},
foo945: () => {},
foo946: () => {},
foo947: () => {},
foo948: () => {},
foo949: () => {},
foo950: () => {},
foo951: () => {},
foo952: () => {},
foo953: () => {},
foo954: () => {},
foo955: () => {},
foo956: () => {},
foo957: () => {},
foo958: () => {},
foo959: () => {},
foo960: () => {},
foo961: () => {},
foo962: () => {},
foo963: () => {},
foo964: () => {},
foo965: () => {},
foo966: () => {},
foo967: () => {},
foo968: () => {},
foo969: () => {},
foo970: () => {},
foo971: () => {},
foo972: () => {},
foo973: () => {},
foo974: () => {},
foo975: () => {},
foo976: () => {},
foo977: () => {},
foo978: () => {},
foo979: () => {},
foo980: () => {},
foo981: () => {},
foo982: () => {},
foo983: () => {},
foo984: () => {},
foo985: () => {},
foo986: () => {},
foo987: () => {},
foo988: () => {},
foo989: () => {},
foo990: () => {},
foo991: () => {},
foo992: () => {},
foo993: () => {},
foo994: () => {},
foo995: () => {},
foo996: () => {},
foo997: () => {},
foo998: () => {},
foo999: () => {},
foo1000: () => {},
foo1001: () => {},
foo1002: () => {},
foo1003: () => {},
foo1004: () => {},
foo1005: () => {},
foo1006: () => {},
foo1007: () => {},
foo1008: () => {},
foo1009: () => {},
foo1010: () => {},
foo1011: () => {},
foo1012: () => {},
foo1013: () => {},
foo1014: () => {},
foo1015: () => {},
foo1016: () => {},
foo1017: () => {},
foo1018: () => {},
foo1019: () => {},
foo1020: () => {},
foo1021: () => {},
foo1022: () => {},
foo1023: () => {},
foo1024: () => {},
foo1025: () => {},
foo1026: () => {},
foo1027: () => {},
foo1028: () => {},
foo1029: () => {},
foo1030: () => {},
foo1031: () => {},
foo1032: () => {},
foo1033: () => {},
foo1034: () => {},
foo1035: () => {},
foo1036: () => {},
foo1037: () => {},
foo1038: () => {},
foo1039: () => {},
foo1040: () => {},
foo1041: () => {},
foo1042: () => {},
foo1043: () => {},
foo1044: () => {},
foo1045: () => {},
foo1046: () => {},
foo1047: () => {},
foo1048: () => {},
foo1049: () => {},
foo1050: () => {},
foo1051: () => {},
foo1052: () => {},
foo1053: () => {},
foo1054: () => {},
foo1055: () => {},
foo1056: () => {},
foo1057: () => {},
foo1058: () => {},
foo1059: () => {},
foo1060: () => {},
foo1061: () => {},
foo1062: () => {},
foo1063: () => {},
foo1064: () => {},
foo1065: () => {},
foo1066: () => {},
foo1067: () => {},
foo1068: () => {},
foo1069: () => {},
foo1070: () => {},
foo1071: () => {},
foo1072: () => {},
foo1073: () => {},
foo1074: () => {},
foo1075: () => {},
foo1076: () => {},
foo1077: () => {},
foo1078: () => {},
foo1079: () => {},
foo1080: () => {},
foo1081: () => {},
foo1082: () => {},
foo1083: () => {},
foo1084: () => {},
foo1085: () => {},
foo1086: () => {},
foo1087: () => {},
foo1088: () => {},
foo1089: () => {},
foo1090: () => {},
foo1091: () => {},
foo1092: () => {},
foo1093: () => {},
foo1094: () => {},
foo1095: () => {},
foo1096: () => {},
foo1097: () => {},
foo1098: () => {},
foo1099: () => {},
foo1100: () => {},
foo1101: () => {},
foo1102: () => {},
foo1103: () => {},
foo1104: () => {},
foo1105: () => {},
foo1106: () => {},
foo1107: () => {},
foo1108: () => {},
foo1109: () => {},
foo1110: () => {},
foo1111: () => {},
foo1112: () => {},
foo1113: () => {},
foo1114: () => {},
foo1115: () => {},
foo1116: () => {},
foo1117: () => {},
foo1118: () => {},
foo1119: () => {},
foo1120: () => {},
foo1121: () => {},
foo1122: () => {},
foo1123: () => {},
foo1124: () => {},
foo1125: () => {},
foo1126: () => {},
foo1127: () => {},
foo1128: () => {},
foo1129: () => {},
foo1130: () => {},
foo1131: () => {},
foo1132: () => {},
foo1133: () => {},
foo1134: () => {},
foo1135: () => {},
foo1136: () => {},
foo1137: () => {},
foo1138: () => {},
foo1139: () => {},
foo1140: () => {},
foo1141: () => {},
foo1142: () => {},
foo1143: () => {},
foo1144: () => {},
foo1145: () => {},
foo1146: () => {},
foo1147: () => {},
foo1148: () => {},
foo1149: () => {},
foo1150: () => {},
foo1151: () => {},
foo1152: () => {},
foo1153: () => {},
foo1154: () => {},
foo1155: () => {},
foo1156: () => {},
foo1157: () => {},
foo1158: () => {},
foo1159: () => {},
foo1160: () => {},
foo1161: () => {},
foo1162: () => {},
foo1163: () => {},
foo1164: () => {},
foo1165: () => {},
foo1166: () => {},
foo1167: () => {},
foo1168: () => {},
foo1169: () => {},
foo1170: () => {},
foo1171: () => {},
foo1172: () => {},
foo1173: () => {},
foo1174: () => {},
foo1175: () => {},
foo1176: () => {},
foo1177: () => {},
foo1178: () => {},
foo1179: () => {},
foo1180: () => {},
foo1181: () => {},
foo1182: () => {},
foo1183: () => {},
foo1184: () => {},
foo1185: () => {},
foo1186: () => {},
foo1187: () => {},
foo1188: () => {},
foo1189: () => {},
foo1190: () => {},
foo1191: () => {},
foo1192: () => {},
foo1193: () => {},
foo1194: () => {},
foo1195: () => {},
foo1196: () => {},
foo1197: () => {},
foo1198: () => {},
foo1199: () => {},
foo1200: () => {},
foo1201: () => {},
foo1202: () => {},
foo1203: () => {},
foo1204: () => {},
foo1205: () => {},
foo1206: () => {},
foo1207: () => {},
foo1208: () => {},
foo1209: () => {},
foo1210: () => {},
foo1211: () => {},
foo1212: () => {},
foo1213: () => {},
foo1214: () => {},
foo1215: () => {},
foo1216: () => {},
foo1217: () => {},
foo1218: () => {},
foo1219: () => {},
foo1220: () => {},
foo1221: () => {},
foo1222: () => {},
foo1223: () => {},
foo1224: () => {},
foo1225: () => {},
foo1226: () => {},
foo1227: () => {},
foo1228: () => {},
foo1229: () => {},
foo1230: () => {},
foo1231: () => {},
foo1232: () => {},
foo1233: () => {},
foo1234: () => {},
foo1235: () => {},
foo1236: () => {},
foo1237: () => {},
foo1238: () => {},
foo1239: () => {},
foo1240: () => {},
foo1241: () => {},
foo1242: () => {},
foo1243: () => {},
foo1244: () => {},
foo1245: () => {},
foo1246: () => {},
foo1247: () => {},
foo1248: () => {},
foo1249: () => {},
foo1250: () => {},
foo1251: () => {},
foo1252: () => {},
foo1253: () => {},
foo1254: () => {},
foo1255: () => {},
foo1256: () => {},
foo1257: () => {},
foo1258: () => {},
foo1259: () => {},
foo1260: () => {},
foo1261: () => {},
foo1262: () => {},
foo1263: () => {},
foo1264: () => {},
foo1265: () => {},
foo1266: () => {},
foo1267: () => {},
foo1268: () => {},
foo1269: () => {},
foo1270: () => {},
foo1271: () => {},
foo1272: () => {},
foo1273: () => {},
foo1274: () => {},
foo1275: () => {},
foo1276: () => {},
foo1277: () => {},
foo1278: () => {},
foo1279: () => {},
foo1280: () => {},
foo1281: () => {},
foo1282: () => {},
foo1283: () => {},
foo1284: () => {},
foo1285: () => {},
foo1286: () => {},
foo1287: () => {},
foo1288: () => {},
foo1289: () => {},
foo1290: () => {},
foo1291: () => {},
foo1292: () => {},
foo1293: () => {},
foo1294: () => {},
foo1295: () => {},
foo1296: () => {},
foo1297: () => {},
foo1298: () => {},
foo1299: () => {},
foo1300: () => {},
foo1301: () => {},
foo1302: () => {},
foo1303: () => {},
foo1304: () => {},
foo1305: () => {},
foo1306: () => {},
foo1307: () => {},
foo1308: () => {},
foo1309: () => {},
foo1310: () => {},
foo1311: () => {},
foo1312: () => {},
foo1313: () => {},
foo1314: () => {},
foo1315: () => {},
foo1316: () => {},
foo1317: () => {},
foo1318: () => {},
foo1319: () => {},
foo1320: () => {},
foo1321: () => {},
foo1322: () => {},
foo1323: () => {},
foo1324: () => {},
foo1325: () => {},
foo1326: () => {},
foo1327: () => {},
foo1328: () => {},
foo1329: () => {},
foo1330: () => {},
foo1331: () => {},
foo1332: () => {},
foo1333: () => {},
foo1334: () => {},
foo1335: () => {},
foo1336: () => {},
foo1337: () => {},
foo1338: () => {},
foo1339: () => {},
foo1340: () => {},
foo1341: () => {},
foo1342: () => {},
foo1343: () => {},
foo1344: () => {},
foo1345: () => {},
foo1346: () => {},
foo1347: () => {},
foo1348: () => {},
foo1349: () => {},
foo1350: () => {},
foo1351: () => {},
foo1352: () => {},
foo1353: () => {},
foo1354: () => {},
foo1355: () => {},
foo1356: () => {},
foo1357: () => {},
foo1358: () => {},
foo1359: () => {},
foo1360: () => {},
foo1361: () => {},
foo1362: () => {},
foo1363: () => {},
foo1364: () => {},
foo1365: () => {},
foo1366: () => {},
foo1367: () => {},
foo1368: () => {},
foo1369: () => {},
foo1370: () => {},
foo1371: () => {},
foo1372: () => {},
foo1373: () => {},
foo1374: () => {},
foo1375: () => {},
foo1376: () => {},
foo1377: () => {},
foo1378: () => {},
foo1379: () => {},
foo1380: () => {},
foo1381: () => {},
foo1382: () => {},
foo1383: () => {},
foo1384: () => {},
foo1385: () => {},
foo1386: () => {},
foo1387: () => {},
foo1388: () => {},
foo1389: () => {},
foo1390: () => {},
foo1391: () => {},
foo1392: () => {},
foo1393: () => {},
foo1394: () => {},
foo1395: () => {},
foo1396: () => {},
foo1397: () => {},
foo1398: () => {},
foo1399: () => {},
foo1400: () => {},
foo1401: () => {},
foo1402: () => {},
foo1403: () => {},
foo1404: () => {},
foo1405: () => {},
foo1406: () => {},
foo1407: () => {},
foo1408: () => {},
foo1409: () => {},
foo1410: () => {},
foo1411: () => {},
foo1412: () => {},
foo1413: () => {},
foo1414: () => {},
foo1415: () => {},
foo1416: () => {},
foo1417: () => {},
foo1418: () => {},
foo1419: () => {},
foo1420: () => {},
foo1421: () => {},
foo1422: () => {},
foo1423: () => {},
foo1424: () => {},
foo1425: () => {},
foo1426: () => {},
foo1427: () => {},
foo1428: () => {},
foo1429: () => {},
foo1430: () => {},
foo1431: () => {},
foo1432: () => {},
foo1433: () => {},
foo1434: () => {},
foo1435: () => {},
foo1436: () => {},
foo1437: () => {},
foo1438: () => {},
foo1439: () => {},
foo1440: () => {},
foo1441: () => {},
foo1442: () => {},
foo1443: () => {},
foo1444: () => {},
foo1445: () => {},
foo1446: () => {},
foo1447: () => {},
foo1448: () => {},
foo1449: () => {},
foo1450: () => {},
foo1451: () => {},
foo1452: () => {},
foo1453: () => {},
foo1454: () => {},
foo1455: () => {},
foo1456: () => {},
foo1457: () => {},
foo1458: () => {},
foo1459: () => {},
foo1460: () => {},
foo1461: () => {},
foo1462: () => {},
foo1463: () => {},
foo1464: () => {},
foo1465: () => {},
foo1466: () => {},
foo1467: () => {},
foo1468: () => {},
foo1469: () => {},
foo1470: () => {},
foo1471: () => {},
foo1472: () => {},
foo1473: () => {},
foo1474: () => {},
foo1475: () => {},
foo1476: () => {},
foo1477: () => {},
foo1478: () => {},
foo1479: () => {},
foo1480: () => {},
foo1481: () => {},
foo1482: () => {},
foo1483: () => {},
foo1484: () => {},
foo1485: () => {},
foo1486: () => {},
foo1487: () => {},
foo1488: () => {},
foo1489: () => {},
foo1490: () => {},
foo1491: () => {},
foo1492: () => {},
foo1493: () => {},
foo1494: () => {},
foo1495: () => {},
foo1496: () => {},
foo1497: () => {},
foo1498: () => {},
foo1499: () => {},
foo1500: () => {},
foo1501: () => {},
foo1502: () => {},
foo1503: () => {},
foo1504: () => {},
foo1505: () => {},
foo1506: () => {},
foo1507: () => {},
foo1508: () => {},
foo1509: () => {},
foo1510: () => {},
foo1511: () => {},
foo1512: () => {},
foo1513: () => {},
foo1514: () => {},
foo1515: () => {},
foo1516: () => {},
foo1517: () => {},
foo1518: () => {},
foo1519: () => {},
foo1520: () => {},
foo1521: () => {},
foo1522: () => {},
foo1523: () => {},
foo1524: () => {},
foo1525: () => {},
foo1526: () => {},
foo1527: () => {},
foo1528: () => {},
foo1529: () => {},
foo1530: () => {},
foo1531: () => {},
foo1532: () => {},
foo1533: () => {},
foo1534: () => {},
foo1535: () => {},
foo1536: () => {},
foo1537: () => {},
foo1538: () => {},
foo1539: () => {},
foo1540: () => {},
foo1541: () => {},
foo1542: () => {},
foo1543: () => {},
foo1544: () => {},
foo1545: () => {},
foo1546: () => {},
foo1547: () => {},
foo1548: () => {},
foo1549: () => {},
foo1550: () => {},
foo1551: () => {},
foo1552: () => {},
foo1553: () => {},
foo1554: () => {},
foo1555: () => {},
foo1556: () => {},
foo1557: () => {},
foo1558: () => {},
foo1559: () => {},
foo1560: () => {},
foo1561: () => {},
foo1562: () => {},
foo1563: () => {},
foo1564: () => {},
foo1565: () => {},
foo1566: () => {},
foo1567: () => {},
foo1568: () => {},
foo1569: () => {},
foo1570: () => {},
foo1571: () => {},
foo1572: () => {},
foo1573: () => {},
foo1574: () => {},
foo1575: () => {},
foo1576: () => {},
foo1577: () => {},
foo1578: () => {},
foo1579: () => {},
foo1580: () => {},
foo1581: () => {},
foo1582: () => {},
foo1583: () => {},
foo1584: () => {},
foo1585: () => {},
foo1586: () => {},
foo1587: () => {},
foo1588: () => {},
foo1589: () => {},
foo1590: () => {},
foo1591: () => {},
foo1592: () => {},
foo1593: () => {},
foo1594: () => {},
foo1595: () => {},
foo1596: () => {},
foo1597: () => {},
foo1598: () => {},
foo1599: () => {},
foo1600: () => {},
foo1601: () => {},
foo1602: () => {},
foo1603: () => {},
foo1604: () => {},
foo1605: () => {},
foo1606: () => {},
foo1607: () => {},
foo1608: () => {},
foo1609: () => {},
foo1610: () => {},
foo1611: () => {},
foo1612: () => {},
foo1613: () => {},
foo1614: () => {},
foo1615: () => {},
foo1616: () => {},
foo1617: () => {},
foo1618: () => {},
foo1619: () => {},
foo1620: () => {},
foo1621: () => {},
foo1622: () => {},
foo1623: () => {},
foo1624: () => {},
foo1625: () => {},
foo1626: () => {},
foo1627: () => {},
foo1628: () => {},
foo1629: () => {},
foo1630: () => {},
foo1631: () => {},
foo1632: () => {},
foo1633: () => {},
foo1634: () => {},
foo1635: () => {},
foo1636: () => {},
foo1637: () => {},
foo1638: () => {},
foo1639: () => {},
foo1640: () => {},
foo1641: () => {},
foo1642: () => {},
foo1643: () => {},
foo1644: () => {},
foo1645: () => {},
foo1646: () => {},
foo1647: () => {},
foo1648: () => {},
foo1649: () => {},
foo1650: () => {},
foo1651: () => {},
foo1652: () => {},
foo1653: () => {},
foo1654: () => {},
foo1655: () => {},
foo1656: () => {},
foo1657: () => {},
foo1658: () => {},
foo1659: () => {},
foo1660: () => {},
foo1661: () => {},
foo1662: () => {},
foo1663: () => {},
foo1664: () => {},
foo1665: () => {},
foo1666: () => {},
foo1667: () => {},
foo1668: () => {},
foo1669: () => {},
foo1670: () => {},
foo1671: () => {},
foo1672: () => {},
foo1673: () => {},
foo1674: () => {},
foo1675: () => {},
foo1676: () => {},
foo1677: () => {},
foo1678: () => {},
foo1679: () => {},
foo1680: () => {},
foo1681: () => {},
foo1682: () => {},
foo1683: () => {},
foo1684: () => {},
foo1685: () => {},
foo1686: () => {},
foo1687: () => {},
foo1688: () => {},
foo1689: () => {},
foo1690: () => {},
foo1691: () => {},
foo1692: () => {},
foo1693: () => {},
foo1694: () => {},
foo1695: () => {},
foo1696: () => {},
foo1697: () => {},
foo1698: () => {},
foo1699: () => {},
foo1700: () => {},
foo1701: () => {},
foo1702: () => {},
foo1703: () => {},
foo1704: () => {},
foo1705: () => {},
foo1706: () => {},
foo1707: () => {},
foo1708: () => {},
foo1709: () => {},
foo1710: () => {},
foo1711: () => {},
foo1712: () => {},
foo1713: () => {},
foo1714: () => {},
foo1715: () => {},
foo1716: () => {},
foo1717: () => {},
foo1718: () => {},
foo1719: () => {},
foo1720: () => {},
foo1721: () => {},
foo1722: () => {},
foo1723: () => {},
foo1724: () => {},
foo1725: () => {},
foo1726: () => {},
foo1727: () => {},
foo1728: () => {},
foo1729: () => {},
foo1730: () => {},
foo1731: () => {},
foo1732: () => {},
foo1733: () => {},
foo1734: () => {},
foo1735: () => {},
foo1736: () => {},
foo1737: () => {},
foo1738: () => {},
foo1739: () => {},
foo1740: () => {},
foo1741: () => {},
foo1742: () => {},
foo1743: () => {},
foo1744: () => {},
foo1745: () => {},
foo1746: () => {},
foo1747: () => {},
foo1748: () => {},
foo1749: () => {},
foo1750: () => {},
foo1751: () => {},
foo1752: () => {},
foo1753: () => {},
foo1754: () => {},
foo1755: () => {},
foo1756: () => {},
foo1757: () => {},
foo1758: () => {},
foo1759: () => {},
foo1760: () => {},
foo1761: () => {},
foo1762: () => {},
foo1763: () => {},
foo1764: () => {},
foo1765: () => {},
foo1766: () => {},
foo1767: () => {},
foo1768: () => {},
foo1769: () => {},
foo1770: () => {},
foo1771: () => {},
foo1772: () => {},
foo1773: () => {},
foo1774: () => {},
foo1775: () => {},
foo1776: () => {},
foo1777: () => {},
foo1778: () => {},
foo1779: () => {},
foo1780: () => {},
foo1781: () => {},
foo1782: () => {},
foo1783: () => {},
foo1784: () => {},
foo1785: () => {},
foo1786: () => {},
foo1787: () => {},
foo1788: () => {},
foo1789: () => {},
foo1790: () => {},
foo1791: () => {},
foo1792: () => {},
foo1793: () => {},
foo1794: () => {},
foo1795: () => {},
foo1796: () => {},
foo1797: () => {},
foo1798: () => {},
foo1799: () => {},
foo1800: () => {},
foo1801: () => {},
foo1802: () => {},
foo1803: () => {},
foo1804: () => {},
foo1805: () => {},
foo1806: () => {},
foo1807: () => {},
foo1808: () => {},
foo1809: () => {},
foo1810: () => {},
foo1811: () => {},
foo1812: () => {},
foo1813: () => {},
foo1814: () => {},
foo1815: () => {},
foo1816: () => {},
foo1817: () => {},
foo1818: () => {},
foo1819: () => {},
foo1820: () => {},
foo1821: () => {},
foo1822: () => {},
foo1823: () => {},
foo1824: () => {},
foo1825: () => {},
foo1826: () => {},
foo1827: () => {},
foo1828: () => {},
foo1829: () => {},
foo1830: () => {},
foo1831: () => {},
foo1832: () => {},
foo1833: () => {},
foo1834: () => {},
foo1835: () => {},
foo1836: () => {},
foo1837: () => {},
foo1838: () => {},
foo1839: () => {},
foo1840: () => {},
foo1841: () => {},
foo1842: () => {},
foo1843: () => {},
foo1844: () => {},
foo1845: () => {},
foo1846: () => {},
foo1847: () => {},
foo1848: () => {},
foo1849: () => {},
foo1850: () => {},
foo1851: () => {},
foo1852: () => {},
foo1853: () => {},
foo1854: () => {},
foo1855: () => {},
foo1856: () => {},
foo1857: () => {},
foo1858: () => {},
foo1859: () => {},
foo1860: () => {},
foo1861: () => {},
foo1862: () => {},
foo1863: () => {},
foo1864: () => {},
foo1865: () => {},
foo1866: () => {},
foo1867: () => {},
foo1868: () => {},
foo1869: () => {},
foo1870: () => {},
foo1871: () => {},
foo1872: () => {},
foo1873: () => {},
foo1874: () => {},
foo1875: () => {},
foo1876: () => {},
foo1877: () => {},
foo1878: () => {},
foo1879: () => {},
foo1880: () => {},
foo1881: () => {},
foo1882: () => {},
foo1883: () => {},
foo1884: () => {},
foo1885: () => {},
foo1886: () => {},
foo1887: () => {},
foo1888: () => {},
foo1889: () => {},
foo1890: () => {},
foo1891: () => {},
foo1892: () => {},
foo1893: () => {},
foo1894: () => {},
foo1895: () => {},
foo1896: () => {},
foo1897: () => {},
foo1898: () => {},
foo1899: () => {},
foo1900: () => {},
foo1901: () => {},
foo1902: () => {},
foo1903: () => {},
foo1904: () => {},
foo1905: () => {},
foo1906: () => {},
foo1907: () => {},
foo1908: () => {},
foo1909: () => {},
foo1910: () => {},
foo1911: () => {},
foo1912: () => {},
foo1913: () => {},
foo1914: () => {},
foo1915: () => {},
foo1916: () => {},
foo1917: () => {},
foo1918: () => {},
foo1919: () => {},
foo1920: () => {},
foo1921: () => {},
foo1922: () => {},
foo1923: () => {},
foo1924: () => {},
foo1925: () => {},
foo1926: () => {},
foo1927: () => {},
foo1928: () => {},
foo1929: () => {},
foo1930: () => {},
foo1931: () => {},
foo1932: () => {},
foo1933: () => {},
foo1934: () => {},
foo1935: () => {},
foo1936: () => {},
foo1937: () => {},
foo1938: () => {},
foo1939: () => {},
foo1940: () => {},
foo1941: () => {},
foo1942: () => {},
foo1943: () => {},
foo1944: () => {},
foo1945: () => {},
foo1946: () => {},
foo1947: () => {},
foo1948: () => {},
foo1949: () => {},
foo1950: () => {},
foo1951: () => {},
foo1952: () => {},
foo1953: () => {},
foo1954: () => {},
foo1955: () => {},
foo1956: () => {},
foo1957: () => {},
foo1958: () => {},
foo1959: () => {},
foo1960: () => {},
foo1961: () => {},
foo1962: () => {},
foo1963: () => {},
foo1964: () => {},
foo1965: () => {},
foo1966: () => {},
foo1967: () => {},
foo1968: () => {},
foo1969: () => {},
foo1970: () => {},
foo1971: () => {},
foo1972: () => {},
foo1973: () => {},
foo1974: () => {},
foo1975: () => {},
foo1976: () => {},
foo1977: () => {},
foo1978: () => {},
foo1979: () => {},
foo1980: () => {},
foo1981: () => {},
foo1982: () => {},
foo1983: () => {},
foo1984: () => {},
foo1985: () => {},
foo1986: () => {},
foo1987: () => {},
foo1988: () => {},
foo1989: () => {},
foo1990: () => {},
foo1991: () => {},
foo1992: () => {},
foo1993: () => {},
foo1994: () => {},
foo1995: () => {},
foo1996: () => {},
foo1997: () => {},
foo1998: () => {},
foo1999: () => {},
foo2000: () => {},
foo2001: () => {},
foo2002: () => {},
foo2003: () => {},
foo2004: () => {},
foo2005: () => {},
foo2006: () => {},
foo2007: () => {},
foo2008: () => {},
foo2009: () => {},
foo2010: () => {},
foo2011: () => {},
foo2012: () => {},
foo2013: () => {},
foo2014: () => {},
foo2015: () => {},
foo2016: () => {},
foo2017: () => {},
foo2018: () => {},
foo2019: () => {},
foo2020: () => {},
foo2021: () => {},
foo2022: () => {},
foo2023: () => {},
foo2024: () => {},
foo2025: () => {},
foo2026: () => {},
foo2027: () => {},
foo2028: () => {},
foo2029: () => {},
foo2030: () => {},
foo2031: () => {},
foo2032: () => {},
foo2033: () => {},
foo2034: () => {},
foo2035: () => {},
foo2036: () => {},
foo2037: () => {},
foo2038: () => {},
foo2039: () => {},
foo2040: () => {},
foo2041: () => {},
foo2042: () => {},
foo2043: () => {},
foo2044: () => {},
foo2045: () => {},
foo2046: () => {},
foo2047: () => {},
foo2048: () => {},
foo2049: () => {},
foo2050: () => {},
foo2051: () => {},
foo2052: () => {},
foo2053: () => {},
foo2054: () => {},
foo2055: () => {},
foo2056: () => {},
foo2057: () => {},
foo2058: () => {},
foo2059: () => {},
foo2060: () => {},
foo2061: () => {},
foo2062: () => {},
foo2063: () => {},
foo2064: () => {},
foo2065: () => {},
foo2066: () => {},
foo2067: () => {},
foo2068: () => {},
foo2069: () => {},
foo2070: () => {},
foo2071: () => {},
foo2072: () => {},
foo2073: () => {},
foo2074: () => {},
foo2075: () => {},
foo2076: () => {},
foo2077: () => {},
foo2078: () => {},
foo2079: () => {},
foo2080: () => {},
foo2081: () => {},
foo2082: () => {},
foo2083: () => {},
foo2084: () => {},
foo2085: () => {},
foo2086: () => {},
foo2087: () => {},
foo2088: () => {},
foo2089: () => {},
foo2090: () => {},
foo2091: () => {},
foo2092: () => {},
foo2093: () => {},
foo2094: () => {},
foo2095: () => {},
foo2096: () => {},
foo2097: () => {},
foo2098: () => {},
foo2099: () => {},
foo2100: () => {},
foo2101: () => {},
foo2102: () => {},
foo2103: () => {},
foo2104: () => {},
foo2105: () => {},
foo2106: () => {},
foo2107: () => {},
foo2108: () => {},
foo2109: () => {},
foo2110: () => {},
foo2111: () => {},
foo2112: () => {},
foo2113: () => {},
foo2114: () => {},
foo2115: () => {},
foo2116: () => {},
foo2117: () => {},
foo2118: () => {},
foo2119: () => {},
foo2120: () => {},
foo2121: () => {},
foo2122: () => {},
foo2123: () => {},
foo2124: () => {},
foo2125: () => {},
foo2126: () => {},
foo2127: () => {},
foo2128: () => {},
foo2129: () => {},
foo2130: () => {},
foo2131: () => {},
foo2132: () => {},
foo2133: () => {},
foo2134: () => {},
foo2135: () => {},
foo2136: () => {},
foo2137: () => {},
foo2138: () => {},
foo2139: () => {},
foo2140: () => {},
foo2141: () => {},
foo2142: () => {},
foo2143: () => {},
foo2144: () => {},
foo2145: () => {},
foo2146: () => {},
foo2147: () => {},
foo2148: () => {},
foo2149: () => {},
foo2150: () => {},
foo2151: () => {},
foo2152: () => {},
foo2153: () => {},
foo2154: () => {},
foo2155: () => {},
foo2156: () => {},
foo2157: () => {},
foo2158: () => {},
foo2159: () => {},
foo2160: () => {},
foo2161: () => {},
foo2162: () => {},
foo2163: () => {},
foo2164: () => {},
foo2165: () => {},
foo2166: () => {},
foo2167: () => {},
foo2168: () => {},
foo2169: () => {},
foo2170: () => {},
foo2171: () => {},
foo2172: () => {},
foo2173: () => {},
foo2174: () => {},
foo2175: () => {},
foo2176: () => {},
foo2177: () => {},
foo2178: () => {},
foo2179: () => {},
foo2180: () => {},
foo2181: () => {},
foo2182: () => {},
foo2183: () => {},
foo2184: () => {},
foo2185: () => {},
foo2186: () => {},
foo2187: () => {},
foo2188: () => {},
foo2189: () => {},
foo2190: () => {},
foo2191: () => {},
foo2192: () => {},
foo2193: () => {},
foo2194: () => {},
foo2195: () => {},
foo2196: () => {},
foo2197: () => {},
foo2198: () => {},
foo2199: () => {},
foo2200: () => {},
foo2201: () => {},
foo2202: () => {},
foo2203: () => {},
foo2204: () => {},
foo2205: () => {},
foo2206: () => {},
foo2207: () => {},
foo2208: () => {},
foo2209: () => {},
foo2210: () => {},
foo2211: () => {},
foo2212: () => {},
foo2213: () => {},
foo2214: () => {},
foo2215: () => {},
foo2216: () => {},
foo2217: () => {},
foo2218: () => {},
foo2219: () => {},
foo2220: () => {},
foo2221: () => {},
foo2222: () => {},
foo2223: () => {},
foo2224: () => {},
foo2225: () => {},
foo2226: () => {},
foo2227: () => {},
foo2228: () => {},
foo2229: () => {},
foo2230: () => {},
foo2231: () => {},
foo2232: () => {},
foo2233: () => {},
foo2234: () => {},
foo2235: () => {},
foo2236: () => {},
foo2237: () => {},
foo2238: () => {},
foo2239: () => {},
foo2240: () => {},
foo2241: () => {},
foo2242: () => {},
foo2243: () => {},
foo2244: () => {},
foo2245: () => {},
foo2246: () => {},
foo2247: () => {},
foo2248: () => {},
foo2249: () => {},
foo2250: () => {},
foo2251: () => {},
foo2252: () => {},
foo2253: () => {},
foo2254: () => {},
foo2255: () => {},
foo2256: () => {},
foo2257: () => {},
foo2258: () => {},
foo2259: () => {},
foo2260: () => {},
foo2261: () => {},
foo2262: () => {},
foo2263: () => {},
foo2264: () => {},
foo2265: () => {},
foo2266: () => {},
foo2267: () => {},
foo2268: () => {},
foo2269: () => {},
foo2270: () => {},
foo2271: () => {},
foo2272: () => {},
foo2273: () => {},
foo2274: () => {},
foo2275: () => {},
foo2276: () => {},
foo2277: () => {},
foo2278: () => {},
foo2279: () => {},
foo2280: () => {},
foo2281: () => {},
foo2282: () => {},
foo2283: () => {},
foo2284: () => {},
foo2285: () => {},
foo2286: () => {},
foo2287: () => {},
foo2288: () => {},
foo2289: () => {},
foo2290: () => {},
foo2291: () => {},
foo2292: () => {},
foo2293: () => {},
foo2294: () => {},
foo2295: () => {},
foo2296: () => {},
foo2297: () => {},
foo2298: () => {},
foo2299: () => {},
foo2300: () => {},
foo2301: () => {},
foo2302: () => {},
foo2303: () => {},
foo2304: () => {},
foo2305: () => {},
foo2306: () => {},
foo2307: () => {},
foo2308: () => {},
foo2309: () => {},
foo2310: () => {},
foo2311: () => {},
foo2312: () => {},
foo2313: () => {},
foo2314: () => {},
foo2315: () => {},
foo2316: () => {},
foo2317: () => {},
foo2318: () => {},
foo2319: () => {},
foo2320: () => {},
foo2321: () => {},
foo2322: () => {},
foo2323: () => {},
foo2324: () => {},
foo2325: () => {},
foo2326: () => {},
foo2327: () => {},
foo2328: () => {},
foo2329: () => {},
foo2330: () => {},
foo2331: () => {},
foo2332: () => {},
foo2333: () => {},
foo2334: () => {},
foo2335: () => {},
foo2336: () => {},
foo2337: () => {},
foo2338: () => {},
foo2339: () => {},
foo2340: () => {},
foo2341: () => {},
foo2342: () => {},
foo2343: () => {},
foo2344: () => {},
foo2345: () => {},
foo2346: () => {},
foo2347: () => {},
foo2348: () => {},
foo2349: () => {},
foo2350: () => {},
foo2351: () => {},
foo2352: () => {},
foo2353: () => {},
foo2354: () => {},
foo2355: () => {},
foo2356: () => {},
foo2357: () => {},
foo2358: () => {},
foo2359: () => {},
foo2360: () => {},
foo2361: () => {},
foo2362: () => {},
foo2363: () => {},
foo2364: () => {},
foo2365: () => {},
foo2366: () => {},
foo2367: () => {},
foo2368: () => {},
foo2369: () => {},
foo2370: () => {},
foo2371: () => {},
foo2372: () => {},
foo2373: () => {},
foo2374: () => {},
foo2375: () => {},
foo2376: () => {},
foo2377: () => {},
foo2378: () => {},
foo2379: () => {},
foo2380: () => {},
foo2381: () => {},
foo2382: () => {},
foo2383: () => {},
foo2384: () => {},
foo2385: () => {},
foo2386: () => {},
foo2387: () => {},
foo2388: () => {},
foo2389: () => {},
foo2390: () => {},
foo2391: () => {},
foo2392: () => {},
foo2393: () => {},
foo2394: () => {},
foo2395: () => {},
foo2396: () => {},
foo2397: () => {},
foo2398: () => {},
foo2399: () => {},
foo2400: () => {},
foo2401: () => {},
foo2402: () => {},
foo2403: () => {},
foo2404: () => {},
foo2405: () => {},
foo2406: () => {},
foo2407: () => {},
foo2408: () => {},
foo2409: () => {},
foo2410: () => {},
foo2411: () => {},
foo2412: () => {},
foo2413: () => {},
foo2414: () => {},
foo2415: () => {},
foo2416: () => {},
foo2417: () => {},
foo2418: () => {},
foo2419: () => {},
foo2420: () => {},
foo2421: () => {},
foo2422: () => {},
foo2423: () => {},
foo2424: () => {},
foo2425: () => {},
foo2426: () => {},
foo2427: () => {},
foo2428: () => {},
foo2429: () => {},
foo2430: () => {},
foo2431: () => {},
foo2432: () => {},
foo2433: () => {},
foo2434: () => {},
foo2435: () => {},
foo2436: () => {},
foo2437: () => {},
foo2438: () => {},
foo2439: () => {},
foo2440: () => {},
foo2441: () => {},
foo2442: () => {},
foo2443: () => {},
foo2444: () => {},
foo2445: () => {},
foo2446: () => {},
foo2447: () => {},
foo2448: () => {},
foo2449: () => {},
foo2450: () => {},
foo2451: () => {},
foo2452: () => {},
foo2453: () => {},
foo2454: () => {},
foo2455: () => {},
foo2456: () => {},
foo2457: () => {},
foo2458: () => {},
foo2459: () => {},
foo2460: () => {},
foo2461: () => {},
foo2462: () => {},
foo2463: () => {},
foo2464: () => {},
foo2465: () => {},
foo2466: () => {},
foo2467: () => {},
foo2468: () => {},
foo2469: () => {},
foo2470: () => {},
foo2471: () => {},
foo2472: () => {},
foo2473: () => {},
foo2474: () => {},
foo2475: () => {},
foo2476: () => {},
foo2477: () => {},
foo2478: () => {},
foo2479: () => {},
foo2480: () => {},
foo2481: () => {},
foo2482: () => {},
foo2483: () => {},
foo2484: () => {},
foo2485: () => {},
foo2486: () => {},
foo2487: () => {},
foo2488: () => {},
foo2489: () => {},
foo2490: () => {},
foo2491: () => {},
foo2492: () => {},
foo2493: () => {},
foo2494: () => {},
foo2495: () => {},
foo2496: () => {},
foo2497: () => {},
foo2498: () => {},
foo2499: () => {},
foo2500: () => {},
foo2501: () => {},
foo2502: () => {},
foo2503: () => {},
foo2504: () => {},
foo2505: () => {},
foo2506: () => {},
foo2507: () => {},
foo2508: () => {},
foo2509: () => {},
foo2510: () => {},
foo2511: () => {},
foo2512: () => {},
foo2513: () => {},
foo2514: () => {},
foo2515: () => {},
foo2516: () => {},
foo2517: () => {},
foo2518: () => {},
foo2519: () => {},
foo2520: () => {},
foo2521: () => {},
foo2522: () => {},
foo2523: () => {},
foo2524: () => {},
foo2525: () => {},
foo2526: () => {},
foo2527: () => {},
foo2528: () => {},
foo2529: () => {},
foo2530: () => {},
foo2531: () => {},
foo2532: () => {},
foo2533: () => {},
foo2534: () => {},
foo2535: () => {},
foo2536: () => {},
foo2537: () => {},
foo2538: () => {},
foo2539: () => {},
foo2540: () => {},
foo2541: () => {},
foo2542: () => {},
foo2543: () => {},
foo2544: () => {},
foo2545: () => {},
foo2546: () => {},
foo2547: () => {},
foo2548: () => {},
foo2549: () => {},
foo2550: () => {},
foo2551: () => {},
foo2552: () => {},
foo2553: () => {},
foo2554: () => {},
foo2555: () => {},
foo2556: () => {},
foo2557: () => {},
foo2558: () => {},
foo2559: () => {},
foo2560: () => {},
foo2561: () => {},
foo2562: () => {},
foo2563: () => {},
foo2564: () => {},
foo2565: () => {},
foo2566: () => {},
foo2567: () => {},
foo2568: () => {},
foo2569: () => {},
foo2570: () => {},
foo2571: () => {},
foo2572: () => {},
foo2573: () => {},
foo2574: () => {},
foo2575: () => {},
foo2576: () => {},
foo2577: () => {},
foo2578: () => {},
foo2579: () => {},
foo2580: () => {},
foo2581: () => {},
foo2582: () => {},
foo2583: () => {},
foo2584: () => {},
foo2585: () => {},
foo2586: () => {},
foo2587: () => {},
foo2588: () => {},
foo2589: () => {},
foo2590: () => {},
foo2591: () => {},
foo2592: () => {},
foo2593: () => {},
foo2594: () => {},
foo2595: () => {},
foo2596: () => {},
foo2597: () => {},
foo2598: () => {},
foo2599: () => {},
foo2600: () => {},
foo2601: () => {},
foo2602: () => {},
foo2603: () => {},
foo2604: () => {},
foo2605: () => {},
foo2606: () => {},
foo2607: () => {},
foo2608: () => {},
foo2609: () => {},
foo2610: () => {},
foo2611: () => {},
foo2612: () => {},
foo2613: () => {},
foo2614: () => {},
foo2615: () => {},
foo2616: () => {},
foo2617: () => {},
foo2618: () => {},
foo2619: () => {},
foo2620: () => {},
foo2621: () => {},
foo2622: () => {},
foo2623: () => {},
foo2624: () => {},
foo2625: () => {},
foo2626: () => {},
foo2627: () => {},
foo2628: () => {},
foo2629: () => {},
foo2630: () => {},
foo2631: () => {},
foo2632: () => {},
foo2633: () => {},
foo2634: () => {},
foo2635: () => {},
foo2636: () => {},
foo2637: () => {},
foo2638: () => {},
foo2639: () => {},
foo2640: () => {},
foo2641: () => {},
foo2642: () => {},
foo2643: () => {},
foo2644: () => {},
foo2645: () => {},
foo2646: () => {},
foo2647: () => {},
foo2648: () => {},
foo2649: () => {},
foo2650: () => {},
foo2651: () => {},
foo2652: () => {},
foo2653: () => {},
foo2654: () => {},
foo2655: () => {},
foo2656: () => {},
foo2657: () => {},
foo2658: () => {},
foo2659: () => {},
foo2660: () => {},
foo2661: () => {},
foo2662: () => {},
foo2663: () => {},
foo2664: () => {},
foo2665: () => {},
foo2666: () => {},
foo2667: () => {},
foo2668: () => {},
foo2669: () => {},
foo2670: () => {},
foo2671: () => {},
foo2672: () => {},
foo2673: () => {},
foo2674: () => {},
foo2675: () => {},
foo2676: () => {},
foo2677: () => {},
foo2678: () => {},
foo2679: () => {},
foo2680: () => {},
foo2681: () => {},
foo2682: () => {},
foo2683: () => {},
foo2684: () => {},
foo2685: () => {},
foo2686: () => {},
foo2687: () => {},
foo2688: () => {},
foo2689: () => {},
foo2690: () => {},
foo2691: () => {},
foo2692: () => {},
foo2693: () => {},
foo2694: () => {},
foo2695: () => {},
foo2696: () => {},
foo2697: () => {},
foo2698: () => {},
foo2699: () => {},
foo2700: () => {},
foo2701: () => {},
foo2702: () => {},
foo2703: () => {},
foo2704: () => {},
foo2705: () => {},
foo2706: () => {},
foo2707: () => {},
foo2708: () => {},
foo2709: () => {},
foo2710: () => {},
foo2711: () => {},
foo2712: () => {},
foo2713: () => {},
foo2714: () => {},
foo2715: () => {},
foo2716: () => {},
foo2717: () => {},
foo2718: () => {},
foo2719: () => {},
foo2720: () => {},
foo2721: () => {},
foo2722: () => {},
foo2723: () => {},
foo2724: () => {},
foo2725: () => {},
foo2726: () => {},
foo2727: () => {},
foo2728: () => {},
foo2729: () => {},
foo2730: () => {},
foo2731: () => {},
foo2732: () => {},
foo2733: () => {},
foo2734: () => {},
foo2735: () => {},
foo2736: () => {},
foo2737: () => {},
foo2738: () => {},
foo2739: () => {},
foo2740: () => {},
foo2741: () => {},
foo2742: () => {},
foo2743: () => {},
foo2744: () => {},
foo2745: () => {},
foo2746: () => {},
foo2747: () => {},
foo2748: () => {},
foo2749: () => {},
foo2750: () => {},
foo2751: () => {},
foo2752: () => {},
foo2753: () => {},
foo2754: () => {},
foo2755: () => {},
foo2756: () => {},
foo2757: () => {},
foo2758: () => {},
foo2759: () => {},
foo2760: () => {},
foo2761: () => {},
foo2762: () => {},
foo2763: () => {},
foo2764: () => {},
foo2765: () => {},
foo2766: () => {},
foo2767: () => {},
foo2768: () => {},
foo2769: () => {},
foo2770: () => {},
foo2771: () => {},
foo2772: () => {},
foo2773: () => {},
foo2774: () => {},
foo2775: () => {},
foo2776: () => {},
foo2777: () => {},
foo2778: () => {},
foo2779: () => {},
foo2780: () => {},
foo2781: () => {},
foo2782: () => {},
foo2783: () => {},
foo2784: () => {},
foo2785: () => {},
foo2786: () => {},
foo2787: () => {},
foo2788: () => {},
foo2789: () => {},
foo2790: () => {},
foo2791: () => {},
foo2792: () => {},
foo2793: () => {},
foo2794: () => {},
foo2795: () => {},
foo2796: () => {},
foo2797: () => {},
foo2798: () => {},
foo2799: () => {},
foo2800: () => {},
foo2801: () => {},
foo2802: () => {},
foo2803: () => {},
foo2804: () => {},
foo2805: () => {},
foo2806: () => {},
foo2807: () => {},
foo2808: () => {},
foo2809: () => {},
foo2810: () => {},
foo2811: () => {},
foo2812: () => {},
foo2813: () => {},
foo2814: () => {},
foo2815: () => {},
foo2816: () => {},
foo2817: () => {},
foo2818: () => {},
foo2819: () => {},
foo2820: () => {},
foo2821: () => {},
foo2822: () => {},
foo2823: () => {},
foo2824: () => {},
foo2825: () => {},
foo2826: () => {},
foo2827: () => {},
foo2828: () => {},
foo2829: () => {},
foo2830: () => {},
foo2831: () => {},
foo2832: () => {},
foo2833: () => {},
foo2834: () => {},
foo2835: () => {},
foo2836: () => {},
foo2837: () => {},
foo2838: () => {},
foo2839: () => {},
foo2840: () => {},
foo2841: () => {},
foo2842: () => {},
foo2843: () => {},
foo2844: () => {},
foo2845: () => {},
foo2846: () => {},
foo2847: () => {},
foo2848: () => {},
foo2849: () => {},
foo2850: () => {},
foo2851: () => {},
foo2852: () => {},
foo2853: () => {},
foo2854: () => {},
foo2855: () => {},
foo2856: () => {},
foo2857: () => {},
foo2858: () => {},
foo2859: () => {},
foo2860: () => {},
foo2861: () => {},
foo2862: () => {},
foo2863: () => {},
foo2864: () => {},
foo2865: () => {},
foo2866: () => {},
foo2867: () => {},
foo2868: () => {},
foo2869: () => {},
foo2870: () => {},
foo2871: () => {},
foo2872: () => {},
foo2873: () => {},
foo2874: () => {},
foo2875: () => {},
foo2876: () => {},
foo2877: () => {},
foo2878: () => {},
foo2879: () => {},
foo2880: () => {},
foo2881: () => {},
foo2882: () => {},
foo2883: () => {},
foo2884: () => {},
foo2885: () => {},
foo2886: () => {},
foo2887: () => {},
foo2888: () => {},
foo2889: () => {},
foo2890: () => {},
foo2891: () => {},
foo2892: () => {},
foo2893: () => {},
foo2894: () => {},
foo2895: () => {},
foo2896: () => {},
foo2897: () => {},
foo2898: () => {},
foo2899: () => {},
foo2900: () => {},
foo2901: () => {},
foo2902: () => {},
foo2903: () => {},
foo2904: () => {},
foo2905: () => {},
foo2906: () => {},
foo2907: () => {},
foo2908: () => {},
foo2909: () => {},
foo2910: () => {},
foo2911: () => {},
foo2912: () => {},
foo2913: () => {},
foo2914: () => {},
foo2915: () => {},
foo2916: () => {},
foo2917: () => {},
foo2918: () => {},
foo2919: () => {},
foo2920: () => {},
foo2921: () => {},
foo2922: () => {},
foo2923: () => {},
foo2924: () => {},
foo2925: () => {},
foo2926: () => {},
foo2927: () => {},
foo2928: () => {},
foo2929: () => {},
foo2930: () => {},
foo2931: () => {},
foo2932: () => {},
foo2933: () => {},
foo2934: () => {},
foo2935: () => {},
foo2936: () => {},
foo2937: () => {},
foo2938: () => {},
foo2939: () => {},
foo2940: () => {},
foo2941: () => {},
foo2942: () => {},
foo2943: () => {},
foo2944: () => {},
foo2945: () => {},
foo2946: () => {},
foo2947: () => {},
foo2948: () => {},
foo2949: () => {},
foo2950: () => {},
foo2951: () => {},
foo2952: () => {},
foo2953: () => {},
foo2954: () => {},
foo2955: () => {},
foo2956: () => {},
foo2957: () => {},
foo2958: () => {},
foo2959: () => {},
foo2960: () => {},
foo2961: () => {},
foo2962: () => {},
foo2963: () => {},
foo2964: () => {},
foo2965: () => {},
foo2966: () => {},
foo2967: () => {},
foo2968: () => {},
foo2969: () => {},
foo2970: () => {},
foo2971: () => {},
foo2972: () => {},
foo2973: () => {},
foo2974: () => {},
foo2975: () => {},
foo2976: () => {},
foo2977: () => {},
foo2978: () => {},
foo2979: () => {},
foo2980: () => {},
foo2981: () => {},
foo2982: () => {},
foo2983: () => {},
foo2984: () => {},
foo2985: () => {},
foo2986: () => {},
foo2987: () => {},
foo2988: () => {},
foo2989: () => {},
foo2990: () => {},
foo2991: () => {},
foo2992: () => {},
foo2993: () => {},
foo2994: () => {},
foo2995: () => {},
foo2996: () => {},
foo2997: () => {},
foo2998: () => {},
foo2999: () => {},
foo3000: () => {},
foo3001: () => {},
foo3002: () => {},
foo3003: () => {},
foo3004: () => {},
foo3005: () => {},
foo3006: () => {},
foo3007: () => {},
foo3008: () => {},
foo3009: () => {},
foo3010: () => {},
foo3011: () => {},
foo3012: () => {},
foo3013: () => {},
foo3014: () => {},
foo3015: () => {},
foo3016: () => {},
foo3017: () => {},
foo3018: () => {},
foo3019: () => {},
foo3020: () => {},
foo3021: () => {},
foo3022: () => {},
foo3023: () => {},
foo3024: () => {},
foo3025: () => {},
foo3026: () => {},
foo3027: () => {},
foo3028: () => {},
foo3029: () => {},
foo3030: () => {},
foo3031: () => {},
foo3032: () => {},
foo3033: () => {},
foo3034: () => {},
foo3035: () => {},
foo3036: () => {},
foo3037: () => {},
foo3038: () => {},
foo3039: () => {},
foo3040: () => {},
foo3041: () => {},
foo3042: () => {},
foo3043: () => {},
foo3044: () => {},
foo3045: () => {},
foo3046: () => {},
foo3047: () => {},
foo3048: () => {},
foo3049: () => {},
foo3050: () => {},
foo3051: () => {},
foo3052: () => {},
foo3053: () => {},
foo3054: () => {},
foo3055: () => {},
foo3056: () => {},
foo3057: () => {},
foo3058: () => {},
foo3059: () => {},
foo3060: () => {},
foo3061: () => {},
foo3062: () => {},
foo3063: () => {},
foo3064: () => {},
foo3065: () => {},
foo3066: () => {},
foo3067: () => {},
foo3068: () => {},
foo3069: () => {},
foo3070: () => {},
foo3071: () => {},
foo3072: () => {},
foo3073: () => {},
foo3074: () => {},
foo3075: () => {},
foo3076: () => {},
foo3077: () => {},
foo3078: () => {},
foo3079: () => {},
foo3080: () => {},
foo3081: () => {},
foo3082: () => {},
foo3083: () => {},
foo3084: () => {},
foo3085: () => {},
foo3086: () => {},
foo3087: () => {},
foo3088: () => {},
foo3089: () => {},
foo3090: () => {},
foo3091: () => {},
foo3092: () => {},
foo3093: () => {},
foo3094: () => {},
foo3095: () => {},
foo3096: () => {},
foo3097: () => {},
foo3098: () => {},
foo3099: () => {},
foo3100: () => {},
foo3101: () => {},
foo3102: () => {},
foo3103: () => {},
foo3104: () => {},
foo3105: () => {},
foo3106: () => {},
foo3107: () => {},
foo3108: () => {},
foo3109: () => {},
foo3110: () => {},
foo3111: () => {},
foo3112: () => {},
foo3113: () => {},
foo3114: () => {},
foo3115: () => {},
foo3116: () => {},
foo3117: () => {},
foo3118: () => {},
foo3119: () => {},
foo3120: () => {},
foo3121: () => {},
foo3122: () => {},
foo3123: () => {},
foo3124: () => {},
foo3125: () => {},
foo3126: () => {},
foo3127: () => {},
foo3128: () => {},
foo3129: () => {},
foo3130: () => {},
foo3131: () => {},
foo3132: () => {},
foo3133: () => {},
foo3134: () => {},
foo3135: () => {},
foo3136: () => {},
foo3137: () => {},
foo3138: () => {},
foo3139: () => {},
foo3140: () => {},
foo3141: () => {},
foo3142: () => {},
foo3143: () => {},
foo3144: () => {},
foo3145: () => {},
foo3146: () => {},
foo3147: () => {},
foo3148: () => {},
foo3149: () => {},
foo3150: () => {},
foo3151: () => {},
foo3152: () => {},
foo3153: () => {},
foo3154: () => {},
foo3155: () => {},
foo3156: () => {},
foo3157: () => {},
foo3158: () => {},
foo3159: () => {},
foo3160: () => {},
foo3161: () => {},
foo3162: () => {},
foo3163: () => {},
foo3164: () => {},
foo3165: () => {},
foo3166: () => {},
foo3167: () => {},
foo3168: () => {},
foo3169: () => {},
foo3170: () => {},
foo3171: () => {},
foo3172: () => {},
foo3173: () => {},
foo3174: () => {},
foo3175: () => {},
foo3176: () => {},
foo3177: () => {},
foo3178: () => {},
foo3179: () => {},
foo3180: () => {},
foo3181: () => {},
foo3182: () => {},
foo3183: () => {},
foo3184: () => {},
foo3185: () => {},
foo3186: () => {},
foo3187: () => {},
foo3188: () => {},
foo3189: () => {},
foo3190: () => {},
foo3191: () => {},
foo3192: () => {},
foo3193: () => {},
foo3194: () => {},
foo3195: () => {},
foo3196: () => {},
foo3197: () => {},
foo3198: () => {},
foo3199: () => {},
foo3200: () => {},
foo3201: () => {},
foo3202: () => {},
foo3203: () => {},
foo3204: () => {},
foo3205: () => {},
foo3206: () => {},
foo3207: () => {},
foo3208: () => {},
foo3209: () => {},
foo3210: () => {},
foo3211: () => {},
foo3212: () => {},
foo3213: () => {},
foo3214: () => {},
foo3215: () => {},
foo3216: () => {},
foo3217: () => {},
foo3218: () => {},
foo3219: () => {},
foo3220: () => {},
foo3221: () => {},
foo3222: () => {},
foo3223: () => {},
foo3224: () => {},
foo3225: () => {},
foo3226: () => {},
foo3227: () => {},
foo3228: () => {},
foo3229: () => {},
foo3230: () => {},
foo3231: () => {},
foo3232: () => {},
foo3233: () => {},
foo3234: () => {},
foo3235: () => {},
foo3236: () => {},
foo3237: () => {},
foo3238: () => {},
foo3239: () => {},
foo3240: () => {},
foo3241: () => {},
foo3242: () => {},
foo3243: () => {},
foo3244: () => {},
foo3245: () => {},
foo3246: () => {},
foo3247: () => {},
foo3248: () => {},
foo3249: () => {},
foo3250: () => {},
foo3251: () => {},
foo3252: () => {},
foo3253: () => {},
foo3254: () => {},
foo3255: () => {},
foo3256: () => {},
foo3257: () => {},
foo3258: () => {},
foo3259: () => {},
foo3260: () => {},
foo3261: () => {},
foo3262: () => {},
foo3263: () => {},
foo3264: () => {},
foo3265: () => {},
foo3266: () => {},
foo3267: () => {},
foo3268: () => {},
foo3269: () => {},
foo3270: () => {},
foo3271: () => {},
foo3272: () => {},
foo3273: () => {},
foo3274: () => {},
foo3275: () => {},
foo3276: () => {},
foo3277: () => {},
foo3278: () => {},
foo3279: () => {},
foo3280: () => {},
foo3281: () => {},
foo3282: () => {},
foo3283: () => {},
foo3284: () => {},
foo3285: () => {},
foo3286: () => {},
foo3287: () => {},
foo3288: () => {},
foo3289: () => {},
foo3290: () => {},
foo3291: () => {},
foo3292: () => {},
foo3293: () => {},
foo3294: () => {},
foo3295: () => {},
foo3296: () => {},
foo3297: () => {},
foo3298: () => {},
foo3299: () => {},
foo3300: () => {},
foo3301: () => {},
foo3302: () => {},
foo3303: () => {},
foo3304: () => {},
foo3305: () => {},
foo3306: () => {},
foo3307: () => {},
foo3308: () => {},
foo3309: () => {},
foo3310: () => {},
foo3311: () => {},
foo3312: () => {},
foo3313: () => {},
foo3314: () => {},
foo3315: () => {},
foo3316: () => {},
foo3317: () => {},
foo3318: () => {},
foo3319: () => {},
foo3320: () => {},
foo3321: () => {},
foo3322: () => {},
foo3323: () => {},
foo3324: () => {},
foo3325: () => {},
foo3326: () => {},
foo3327: () => {},
foo3328: () => {},
foo3329: () => {},
foo3330: () => {},
foo3331: () => {},
foo3332: () => {},
foo3333: () => {},
foo3334: () => {},
foo3335: () => {},
foo3336: () => {},
foo3337: () => {},
foo3338: () => {},
foo3339: () => {},
foo3340: () => {},
foo3341: () => {},
foo3342: () => {},
foo3343: () => {},
foo3344: () => {},
foo3345: () => {},
foo3346: () => {},
foo3347: () => {},
foo3348: () => {},
foo3349: () => {},
foo3350: () => {},
foo3351: () => {},
foo3352: () => {},
foo3353: () => {},
foo3354: () => {},
foo3355: () => {},
foo3356: () => {},
foo3357: () => {},
foo3358: () => {},
foo3359: () => {},
foo3360: () => {},
foo3361: () => {},
foo3362: () => {},
foo3363: () => {},
foo3364: () => {},
foo3365: () => {},
foo3366: () => {},
foo3367: () => {},
foo3368: () => {},
foo3369: () => {},
foo3370: () => {},
foo3371: () => {},
foo3372: () => {},
foo3373: () => {},
foo3374: () => {},
foo3375: () => {},
foo3376: () => {},
foo3377: () => {},
foo3378: () => {},
foo3379: () => {},
foo3380: () => {},
foo3381: () => {},
foo3382: () => {},
foo3383: () => {},
foo3384: () => {},
foo3385: () => {},
foo3386: () => {},
foo3387: () => {},
foo3388: () => {},
foo3389: () => {},
foo3390: () => {},
foo3391: () => {},
foo3392: () => {},
foo3393: () => {},
foo3394: () => {},
foo3395: () => {},
foo3396: () => {},
foo3397: () => {},
foo3398: () => {},
foo3399: () => {},
foo3400: () => {},
foo3401: () => {},
foo3402: () => {},
foo3403: () => {},
foo3404: () => {},
foo3405: () => {},
foo3406: () => {},
foo3407: () => {},
foo3408: () => {},
foo3409: () => {},
foo3410: () => {},
foo3411: () => {},
foo3412: () => {},
foo3413: () => {},
foo3414: () => {},
foo3415: () => {},
foo3416: () => {},
foo3417: () => {},
foo3418: () => {},
foo3419: () => {},
foo3420: () => {},
foo3421: () => {},
foo3422: () => {},
foo3423: () => {},
foo3424: () => {},
foo3425: () => {},
foo3426: () => {},
foo3427: () => {},
foo3428: () => {},
foo3429: () => {},
foo3430: () => {},
foo3431: () => {},
foo3432: () => {},
foo3433: () => {},
foo3434: () => {},
foo3435: () => {},
foo3436: () => {},
foo3437: () => {},
foo3438: () => {},
foo3439: () => {},
foo3440: () => {},
foo3441: () => {},
foo3442: () => {},
foo3443: () => {},
foo3444: () => {},
foo3445: () => {},
foo3446: () => {},
foo3447: () => {},
foo3448: () => {},
foo3449: () => {},
foo3450: () => {},
foo3451: () => {},
foo3452: () => {},
foo3453: () => {},
foo3454: () => {},
foo3455: () => {},
foo3456: () => {},
foo3457: () => {},
foo3458: () => {},
foo3459: () => {},
foo3460: () => {},
foo3461: () => {},
foo3462: () => {},
foo3463: () => {},
foo3464: () => {},
foo3465: () => {},
foo3466: () => {},
foo3467: () => {},
foo3468: () => {},
foo3469: () => {},
foo3470: () => {},
foo3471: () => {},
foo3472: () => {},
foo3473: () => {},
foo3474: () => {},
foo3475: () => {},
foo3476: () => {},
foo3477: () => {},
foo3478: () => {},
foo3479: () => {},
foo3480: () => {},
foo3481: () => {},
foo3482: () => {},
foo3483: () => {},
foo3484: () => {},
foo3485: () => {},
foo3486: () => {},
foo3487: () => {},
foo3488: () => {},
foo3489: () => {},
foo3490: () => {},
foo3491: () => {},
foo3492: () => {},
foo3493: () => {},
foo3494: () => {},
foo3495: () => {},
foo3496: () => {},
foo3497: () => {},
foo3498: () => {},
foo3499: () => {},
foo3500: () => {},
foo3501: () => {},
foo3502: () => {},
foo3503: () => {},
foo3504: () => {},
foo3505: () => {},
foo3506: () => {},
foo3507: () => {},
foo3508: () => {},
foo3509: () => {},
foo3510: () => {},
foo3511: () => {},
foo3512: () => {},
foo3513: () => {},
foo3514: () => {},
foo3515: () => {},
foo3516: () => {},
foo3517: () => {},
foo3518: () => {},
foo3519: () => {},
foo3520: () => {},
foo3521: () => {},
foo3522: () => {},
foo3523: () => {},
foo3524: () => {},
foo3525: () => {},
foo3526: () => {},
foo3527: () => {},
foo3528: () => {},
foo3529: () => {},
foo3530: () => {},
foo3531: () => {},
foo3532: () => {},
foo3533: () => {},
foo3534: () => {},
foo3535: () => {},
foo3536: () => {},
foo3537: () => {},
foo3538: () => {},
foo3539: () => {},
foo3540: () => {},
foo3541: () => {},
foo3542: () => {},
foo3543: () => {},
foo3544: () => {},
foo3545: () => {},
foo3546: () => {},
foo3547: () => {},
foo3548: () => {},
foo3549: () => {},
foo3550: () => {},
foo3551: () => {},
foo3552: () => {},
foo3553: () => {},
foo3554: () => {},
foo3555: () => {},
foo3556: () => {},
foo3557: () => {},
foo3558: () => {},
foo3559: () => {},
foo3560: () => {},
foo3561: () => {},
foo3562: () => {},
foo3563: () => {},
foo3564: () => {},
foo3565: () => {},
foo3566: () => {},
foo3567: () => {},
foo3568: () => {},
foo3569: () => {},
foo3570: () => {},
foo3571: () => {},
foo3572: () => {},
foo3573: () => {},
foo3574: () => {},
foo3575: () => {},
foo3576: () => {},
foo3577: () => {},
foo3578: () => {},
foo3579: () => {},
foo3580: () => {},
foo3581: () => {},
foo3582: () => {},
foo3583: () => {},
foo3584: () => {},
foo3585: () => {},
foo3586: () => {},
foo3587: () => {},
foo3588: () => {},
foo3589: () => {},
foo3590: () => {},
foo3591: () => {},
foo3592: () => {},
foo3593: () => {},
foo3594: () => {},
foo3595: () => {},
foo3596: () => {},
foo3597: () => {},
foo3598: () => {},
foo3599: () => {},
foo3600: () => {},
foo3601: () => {},
foo3602: () => {},
foo3603: () => {},
foo3604: () => {},
foo3605: () => {},
foo3606: () => {},
foo3607: () => {},
foo3608: () => {},
foo3609: () => {},
foo3610: () => {},
foo3611: () => {},
foo3612: () => {},
foo3613: () => {},
foo3614: () => {},
foo3615: () => {},
foo3616: () => {},
foo3617: () => {},
foo3618: () => {},
foo3619: () => {},
foo3620: () => {},
foo3621: () => {},
foo3622: () => {},
foo3623: () => {},
foo3624: () => {},
foo3625: () => {},
foo3626: () => {},
foo3627: () => {},
foo3628: () => {},
foo3629: () => {},
foo3630: () => {},
foo3631: () => {},
foo3632: () => {},
foo3633: () => {},
foo3634: () => {},
foo3635: () => {},
foo3636: () => {},
foo3637: () => {},
foo3638: () => {},
foo3639: () => {},
foo3640: () => {},
foo3641: () => {},
foo3642: () => {},
foo3643: () => {},
foo3644: () => {},
foo3645: () => {},
foo3646: () => {},
foo3647: () => {},
foo3648: () => {},
foo3649: () => {},
foo3650: () => {},
foo3651: () => {},
foo3652: () => {},
foo3653: () => {},
foo3654: () => {},
foo3655: () => {},
foo3656: () => {},
foo3657: () => {},
foo3658: () => {},
foo3659: () => {},
foo3660: () => {},
foo3661: () => {},
foo3662: () => {},
foo3663: () => {},
foo3664: () => {},
foo3665: () => {},
foo3666: () => {},
foo3667: () => {},
foo3668: () => {},
foo3669: () => {},
foo3670: () => {},
foo3671: () => {},
foo3672: () => {},
foo3673: () => {},
foo3674: () => {},
foo3675: () => {},
foo3676: () => {},
foo3677: () => {},
foo3678: () => {},
foo3679: () => {},
foo3680: () => {},
foo3681: () => {},
foo3682: () => {},
foo3683: () => {},
foo3684: () => {},
foo3685: () => {},
foo3686: () => {},
foo3687: () => {},
foo3688: () => {},
foo3689: () => {},
foo3690: () => {},
foo3691: () => {},
foo3692: () => {},
foo3693: () => {},
foo3694: () => {},
foo3695: () => {},
foo3696: () => {},
foo3697: () => {},
foo3698: () => {},
foo3699: () => {},
foo3700: () => {},
foo3701: () => {},
foo3702: () => {},
foo3703: () => {},
foo3704: () => {},
foo3705: () => {},
foo3706: () => {},
foo3707: () => {},
foo3708: () => {},
foo3709: () => {},
foo3710: () => {},
foo3711: () => {},
foo3712: () => {},
foo3713: () => {},
foo3714: () => {},
foo3715: () => {},
foo3716: () => {},
foo3717: () => {},
foo3718: () => {},
foo3719: () => {},
foo3720: () => {},
foo3721: () => {},
foo3722: () => {},
foo3723: () => {},
foo3724: () => {},
foo3725: () => {},
foo3726: () => {},
foo3727: () => {},
foo3728: () => {},
foo3729: () => {},
foo3730: () => {},
foo3731: () => {},
foo3732: () => {},
foo3733: () => {},
foo3734: () => {},
foo3735: () => {},
foo3736: () => {},
foo3737: () => {},
foo3738: () => {},
foo3739: () => {},
foo3740: () => {},
foo3741: () => {},
foo3742: () => {},
foo3743: () => {},
foo3744: () => {},
foo3745: () => {},
foo3746: () => {},
foo3747: () => {},
foo3748: () => {},
foo3749: () => {},
foo3750: () => {},
foo3751: () => {},
foo3752: () => {},
foo3753: () => {},
foo3754: () => {},
foo3755: () => {},
foo3756: () => {},
foo3757: () => {},
foo3758: () => {},
foo3759: () => {},
foo3760: () => {},
foo3761: () => {},
foo3762: () => {},
foo3763: () => {},
foo3764: () => {},
foo3765: () => {},
foo3766: () => {},
foo3767: () => {},
foo3768: () => {},
foo3769: () => {},
foo3770: () => {},
foo3771: () => {},
foo3772: () => {},
foo3773: () => {},
foo3774: () => {},
foo3775: () => {},
foo3776: () => {},
foo3777: () => {},
foo3778: () => {},
foo3779: () => {},
foo3780: () => {},
foo3781: () => {},
foo3782: () => {},
foo3783: () => {},
foo3784: () => {},
foo3785: () => {},
foo3786: () => {},
foo3787: () => {},
foo3788: () => {},
foo3789: () => {},
foo3790: () => {},
foo3791: () => {},
foo3792: () => {},
foo3793: () => {},
foo3794: () => {},
foo3795: () => {},
foo3796: () => {},
foo3797: () => {},
foo3798: () => {},
foo3799: () => {},
foo3800: () => {},
foo3801: () => {},
foo3802: () => {},
foo3803: () => {},
foo3804: () => {},
foo3805: () => {},
foo3806: () => {},
foo3807: () => {},
foo3808: () => {},
foo3809: () => {},
foo3810: () => {},
foo3811: () => {},
foo3812: () => {},
foo3813: () => {},
foo3814: () => {},
foo3815: () => {},
foo3816: () => {},
foo3817: () => {},
foo3818: () => {},
foo3819: () => {},
foo3820: () => {},
foo3821: () => {},
foo3822: () => {},
foo3823: () => {},
foo3824: () => {},
foo3825: () => {},
foo3826: () => {},
foo3827: () => {},
foo3828: () => {},
foo3829: () => {},
foo3830: () => {},
foo3831: () => {},
foo3832: () => {},
foo3833: () => {},
foo3834: () => {},
foo3835: () => {},
foo3836: () => {},
foo3837: () => {},
foo3838: () => {},
foo3839: () => {},
foo3840: () => {},
foo3841: () => {},
foo3842: () => {},
foo3843: () => {},
foo3844: () => {},
foo3845: () => {},
foo3846: () => {},
foo3847: () => {},
foo3848: () => {},
foo3849: () => {},
foo3850: () => {},
foo3851: () => {},
foo3852: () => {},
foo3853: () => {},
foo3854: () => {},
foo3855: () => {},
foo3856: () => {},
foo3857: () => {},
foo3858: () => {},
foo3859: () => {},
foo3860: () => {},
foo3861: () => {},
foo3862: () => {},
foo3863: () => {},
foo3864: () => {},
foo3865: () => {},
foo3866: () => {},
foo3867: () => {},
foo3868: () => {},
foo3869: () => {},
foo3870: () => {},
foo3871: () => {},
foo3872: () => {},
foo3873: () => {},
foo3874: () => {},
foo3875: () => {},
foo3876: () => {},
foo3877: () => {},
foo3878: () => {},
foo3879: () => {},
foo3880: () => {},
foo3881: () => {},
foo3882: () => {},
foo3883: () => {},
foo3884: () => {},
foo3885: () => {},
foo3886: () => {},
foo3887: () => {},
foo3888: () => {},
foo3889: () => {},
foo3890: () => {},
foo3891: () => {},
foo3892: () => {},
foo3893: () => {},
foo3894: () => {},
foo3895: () => {},
foo3896: () => {},
foo3897: () => {},
foo3898: () => {},
foo3899: () => {},
foo3900: () => {},
foo3901: () => {},
foo3902: () => {},
foo3903: () => {},
foo3904: () => {},
foo3905: () => {},
foo3906: () => {},
foo3907: () => {},
foo3908: () => {},
foo3909: () => {},
foo3910: () => {},
foo3911: () => {},
foo3912: () => {},
foo3913: () => {},
foo3914: () => {},
foo3915: () => {},
foo3916: () => {},
foo3917: () => {},
foo3918: () => {},
foo3919: () => {},
foo3920: () => {},
foo3921: () => {},
foo3922: () => {},
foo3923: () => {},
foo3924: () => {},
foo3925: () => {},
foo3926: () => {},
foo3927: () => {},
foo3928: () => {},
foo3929: () => {},
foo3930: () => {},
foo3931: () => {},
foo3932: () => {},
foo3933: () => {},
foo3934: () => {},
foo3935: () => {},
foo3936: () => {},
foo3937: () => {},
foo3938: () => {},
foo3939: () => {},
foo3940: () => {},
foo3941: () => {},
foo3942: () => {},
foo3943: () => {},
foo3944: () => {},
foo3945: () => {},
foo3946: () => {},
foo3947: () => {},
foo3948: () => {},
foo3949: () => {},
foo3950: () => {},
foo3951: () => {},
foo3952: () => {},
foo3953: () => {},
foo3954: () => {},
foo3955: () => {},
foo3956: () => {},
foo3957: () => {},
foo3958: () => {},
foo3959: () => {},
foo3960: () => {},
foo3961: () => {},
foo3962: () => {},
foo3963: () => {},
foo3964: () => {},
foo3965: () => {},
foo3966: () => {},
foo3967: () => {},
foo3968: () => {},
foo3969: () => {},
foo3970: () => {},
foo3971: () => {},
foo3972: () => {},
foo3973: () => {},
foo3974: () => {},
foo3975: () => {},
foo3976: () => {},
foo3977: () => {},
foo3978: () => {},
foo3979: () => {},
foo3980: () => {},
foo3981: () => {},
foo3982: () => {},
foo3983: () => {},
foo3984: () => {},
foo3985: () => {},
foo3986: () => {},
foo3987: () => {},
foo3988: () => {},
foo3989: () => {},
foo3990: () => {},
foo3991: () => {},
foo3992: () => {},
foo3993: () => {},
foo3994: () => {},
foo3995: () => {},
foo3996: () => {},
foo3997: () => {},
foo3998: () => {},
foo3999: () => {},
foo4000: () => {},
foo4001: () => {},
foo4002: () => {},
foo4003: () => {},
foo4004: () => {},
foo4005: () => {},
foo4006: () => {},
foo4007: () => {},
foo4008: () => {},
foo4009: () => {},
foo4010: () => {},
foo4011: () => {},
foo4012: () => {},
foo4013: () => {},
foo4014: () => {},
foo4015: () => {},
foo4016: () => {},
foo4017: () => {},
foo4018: () => {},
foo4019: () => {},
foo4020: () => {},
foo4021: () => {},
foo4022: () => {},
foo4023: () => {},
foo4024: () => {},
foo4025: () => {},
foo4026: () => {},
foo4027: () => {},
foo4028: () => {},
foo4029: () => {},
foo4030: () => {},
foo4031: () => {},
foo4032: () => {},
foo4033: () => {},
foo4034: () => {},
foo4035: () => {},
foo4036: () => {},
foo4037: () => {},
foo4038: () => {},
foo4039: () => {},
foo4040: () => {},
foo4041: () => {},
foo4042: () => {},
foo4043: () => {},
foo4044: () => {},
foo4045: () => {},
foo4046: () => {},
foo4047: () => {},
foo4048: () => {},
foo4049: () => {},
foo4050: () => {},
foo4051: () => {},
foo4052: () => {},
foo4053: () => {},
foo4054: () => {},
foo4055: () => {},
foo4056: () => {},
foo4057: () => {},
foo4058: () => {},
foo4059: () => {},
foo4060: () => {},
foo4061: () => {},
foo4062: () => {},
foo4063: () => {},
foo4064: () => {},
foo4065: () => {},
foo4066: () => {},
foo4067: () => {},
foo4068: () => {},
foo4069: () => {},
foo4070: () => {},
foo4071: () => {},
foo4072: () => {},
foo4073: () => {},
foo4074: () => {},
foo4075: () => {},
foo4076: () => {},
foo4077: () => {},
foo4078: () => {},
foo4079: () => {},
foo4080: () => {},
foo4081: () => {},
foo4082: () => {},
foo4083: () => {},
foo4084: () => {},
foo4085: () => {},
foo4086: () => {},
foo4087: () => {},
foo4088: () => {},
foo4089: () => {},
foo4090: () => {},
foo4091: () => {},
foo4092: () => {},
foo4093: () => {},
foo4094: () => {},
foo4095: () => {},
foo4096: () => {},
foo4097: () => {},
foo4098: () => {},
foo4099: () => {},
foo4100: () => {},
foo4101: () => {},
foo4102: () => {},
foo4103: () => {},
foo4104: () => {},
foo4105: () => {},
foo4106: () => {},
foo4107: () => {},
foo4108: () => {},
foo4109: () => {},
foo4110: () => {},
foo4111: () => {},
foo4112: () => {},
foo4113: () => {},
foo4114: () => {},
foo4115: () => {},
foo4116: () => {},
foo4117: () => {},
foo4118: () => {},
foo4119: () => {},
foo4120: () => {},
foo4121: () => {},
foo4122: () => {},
foo4123: () => {},
foo4124: () => {},
foo4125: () => {},
foo4126: () => {},
foo4127: () => {},
foo4128: () => {},
foo4129: () => {},
foo4130: () => {},
foo4131: () => {},
foo4132: () => {},
foo4133: () => {},
foo4134: () => {},
foo4135: () => {},
foo4136: () => {},
foo4137: () => {},
foo4138: () => {},
foo4139: () => {},
foo4140: () => {},
foo4141: () => {},
foo4142: () => {},
foo4143: () => {},
foo4144: () => {},
foo4145: () => {},
foo4146: () => {},
foo4147: () => {},
foo4148: () => {},
foo4149: () => {},
foo4150: () => {},
foo4151: () => {},
foo4152: () => {},
foo4153: () => {},
foo4154: () => {},
foo4155: () => {},
foo4156: () => {},
foo4157: () => {},
foo4158: () => {},
foo4159: () => {},
foo4160: () => {},
foo4161: () => {},
foo4162: () => {},
foo4163: () => {},
foo4164: () => {},
foo4165: () => {},
foo4166: () => {},
foo4167: () => {},
foo4168: () => {},
foo4169: () => {},
foo4170: () => {},
foo4171: () => {},
foo4172: () => {},
foo4173: () => {},
foo4174: () => {},
foo4175: () => {},
foo4176: () => {},
foo4177: () => {},
foo4178: () => {},
foo4179: () => {},
foo4180: () => {},
foo4181: () => {},
foo4182: () => {},
foo4183: () => {},
foo4184: () => {},
foo4185: () => {},
foo4186: () => {},
foo4187: () => {},
foo4188: () => {},
foo4189: () => {},
foo4190: () => {},
foo4191: () => {},
foo4192: () => {},
foo4193: () => {},
foo4194: () => {},
foo4195: () => {},
foo4196: () => {},
foo4197: () => {},
foo4198: () => {},
foo4199: () => {},
foo4200: () => {},
foo4201: () => {},
foo4202: () => {},
foo4203: () => {},
foo4204: () => {},
foo4205: () => {},
foo4206: () => {},
foo4207: () => {},
foo4208: () => {},
foo4209: () => {},
foo4210: () => {},
foo4211: () => {},
foo4212: () => {},
foo4213: () => {},
foo4214: () => {},
foo4215: () => {},
foo4216: () => {},
foo4217: () => {},
foo4218: () => {},
foo4219: () => {},
foo4220: () => {},
foo4221: () => {},
foo4222: () => {},
foo4223: () => {},
foo4224: () => {},
foo4225: () => {},
foo4226: () => {},
foo4227: () => {},
foo4228: () => {},
foo4229: () => {},
foo4230: () => {},
foo4231: () => {},
foo4232: () => {},
foo4233: () => {},
foo4234: () => {},
foo4235: () => {},
foo4236: () => {},
foo4237: () => {},
foo4238: () => {},
foo4239: () => {},
foo4240: () => {},
foo4241: () => {},
foo4242: () => {},
foo4243: () => {},
foo4244: () => {},
foo4245: () => {},
foo4246: () => {},
foo4247: () => {},
foo4248: () => {},
foo4249: () => {},
foo4250: () => {},
foo4251: () => {},
foo4252: () => {},
foo4253: () => {},
foo4254: () => {},
foo4255: () => {},
foo4256: () => {},
foo4257: () => {},
foo4258: () => {},
foo4259: () => {},
foo4260: () => {},
foo4261: () => {},
foo4262: () => {},
foo4263: () => {},
foo4264: () => {},
foo4265: () => {},
foo4266: () => {},
foo4267: () => {},
foo4268: () => {},
foo4269: () => {},
foo4270: () => {},
foo4271: () => {},
foo4272: () => {},
foo4273: () => {},
foo4274: () => {},
foo4275: () => {},
foo4276: () => {},
foo4277: () => {},
foo4278: () => {},
foo4279: () => {},
foo4280: () => {},
foo4281: () => {},
foo4282: () => {},
foo4283: () => {},
foo4284: () => {},
foo4285: () => {},
foo4286: () => {},
foo4287: () => {},
foo4288: () => {},
foo4289: () => {},
foo4290: () => {},
foo4291: () => {},
foo4292: () => {},
foo4293: () => {},
foo4294: () => {},
foo4295: () => {},
foo4296: () => {},
foo4297: () => {},
foo4298: () => {},
foo4299: () => {},
foo4300: () => {},
foo4301: () => {},
foo4302: () => {},
foo4303: () => {},
foo4304: () => {},
foo4305: () => {},
foo4306: () => {},
foo4307: () => {},
foo4308: () => {},
foo4309: () => {},
foo4310: () => {},
foo4311: () => {},
foo4312: () => {},
foo4313: () => {},
foo4314: () => {},
foo4315: () => {},
foo4316: () => {},
foo4317: () => {},
foo4318: () => {},
foo4319: () => {},
foo4320: () => {},
foo4321: () => {},
foo4322: () => {},
foo4323: () => {},
foo4324: () => {},
foo4325: () => {},
foo4326: () => {},
foo4327: () => {},
foo4328: () => {},
foo4329: () => {},
foo4330: () => {},
foo4331: () => {},
foo4332: () => {},
foo4333: () => {},
foo4334: () => {},
foo4335: () => {},
foo4336: () => {},
foo4337: () => {},
foo4338: () => {},
foo4339: () => {},
foo4340: () => {},
foo4341: () => {},
foo4342: () => {},
foo4343: () => {},
foo4344: () => {},
foo4345: () => {},
foo4346: () => {},
foo4347: () => {},
foo4348: () => {},
foo4349: () => {},
foo4350: () => {},
foo4351: () => {},
foo4352: () => {},
foo4353: () => {},
foo4354: () => {},
foo4355: () => {},
foo4356: () => {},
foo4357: () => {},
foo4358: () => {},
foo4359: () => {},
foo4360: () => {},
foo4361: () => {},
foo4362: () => {},
foo4363: () => {},
foo4364: () => {},
foo4365: () => {},
foo4366: () => {},
foo4367: () => {},
foo4368: () => {},
foo4369: () => {},
foo4370: () => {},
foo4371: () => {},
foo4372: () => {},
foo4373: () => {},
foo4374: () => {},
foo4375: () => {},
foo4376: () => {},
foo4377: () => {},
foo4378: () => {},
foo4379: () => {},
foo4380: () => {},
foo4381: () => {},
foo4382: () => {},
foo4383: () => {},
foo4384: () => {},
foo4385: () => {},
foo4386: () => {},
foo4387: () => {},
foo4388: () => {},
foo4389: () => {},
foo4390: () => {},
foo4391: () => {},
foo4392: () => {},
foo4393: () => {},
foo4394: () => {},
foo4395: () => {},
foo4396: () => {},
foo4397: () => {},
foo4398: () => {},
foo4399: () => {},
foo4400: () => {},
foo4401: () => {},
foo4402: () => {},
foo4403: () => {},
foo4404: () => {},
foo4405: () => {},
foo4406: () => {},
foo4407: () => {},
foo4408: () => {},
foo4409: () => {},
foo4410: () => {},
foo4411: () => {},
foo4412: () => {},
foo4413: () => {},
foo4414: () => {},
foo4415: () => {},
foo4416: () => {},
foo4417: () => {},
foo4418: () => {},
foo4419: () => {},
foo4420: () => {},
foo4421: () => {},
foo4422: () => {},
foo4423: () => {},
foo4424: () => {},
foo4425: () => {},
foo4426: () => {},
foo4427: () => {},
foo4428: () => {},
foo4429: () => {},
foo4430: () => {},
foo4431: () => {},
foo4432: () => {},
foo4433: () => {},
foo4434: () => {},
foo4435: () => {},
foo4436: () => {},
foo4437: () => {},
foo4438: () => {},
foo4439: () => {},
foo4440: () => {},
foo4441: () => {},
foo4442: () => {},
foo4443: () => {},
foo4444: () => {},
foo4445: () => {},
foo4446: () => {},
foo4447: () => {},
foo4448: () => {},
foo4449: () => {},
foo4450: () => {},
foo4451: () => {},
foo4452: () => {},
foo4453: () => {},
foo4454: () => {},
foo4455: () => {},
foo4456: () => {},
foo4457: () => {},
foo4458: () => {},
foo4459: () => {},
foo4460: () => {},
foo4461: () => {},
foo4462: () => {},
foo4463: () => {},
foo4464: () => {},
foo4465: () => {},
foo4466: () => {},
foo4467: () => {},
foo4468: () => {},
foo4469: () => {},
foo4470: () => {},
foo4471: () => {},
foo4472: () => {},
foo4473: () => {},
foo4474: () => {},
foo4475: () => {},
foo4476: () => {},
foo4477: () => {},
foo4478: () => {},
foo4479: () => {},
foo4480: () => {},
foo4481: () => {},
foo4482: () => {},
foo4483: () => {},
foo4484: () => {},
foo4485: () => {},
foo4486: () => {},
foo4487: () => {},
foo4488: () => {},
foo4489: () => {},
foo4490: () => {},
foo4491: () => {},
foo4492: () => {},
foo4493: () => {},
foo4494: () => {},
foo4495: () => {},
foo4496: () => {},
foo4497: () => {},
foo4498: () => {},
foo4499: () => {},
foo4500: () => {},
foo4501: () => {},
foo4502: () => {},
foo4503: () => {},
foo4504: () => {},
foo4505: () => {},
foo4506: () => {},
foo4507: () => {},
foo4508: () => {},
foo4509: () => {},
foo4510: () => {},
foo4511: () => {},
foo4512: () => {},
foo4513: () => {},
foo4514: () => {},
foo4515: () => {},
foo4516: () => {},
foo4517: () => {},
foo4518: () => {},
foo4519: () => {},
foo4520: () => {},
foo4521: () => {},
foo4522: () => {},
foo4523: () => {},
foo4524: () => {},
foo4525: () => {},
foo4526: () => {},
foo4527: () => {},
foo4528: () => {},
foo4529: () => {},
foo4530: () => {},
foo4531: () => {},
foo4532: () => {},
foo4533: () => {},
foo4534: () => {},
foo4535: () => {},
foo4536: () => {},
foo4537: () => {},
foo4538: () => {},
foo4539: () => {},
foo4540: () => {},
foo4541: () => {},
foo4542: () => {},
foo4543: () => {},
foo4544: () => {},
foo4545: () => {},
foo4546: () => {},
foo4547: () => {},
foo4548: () => {},
foo4549: () => {},
foo4550: () => {},
foo4551: () => {},
foo4552: () => {},
foo4553: () => {},
foo4554: () => {},
foo4555: () => {},
foo4556: () => {},
foo4557: () => {},
foo4558: () => {},
foo4559: () => {},
foo4560: () => {},
foo4561: () => {},
foo4562: () => {},
foo4563: () => {},
foo4564: () => {},
foo4565: () => {},
foo4566: () => {},
foo4567: () => {},
foo4568: () => {},
foo4569: () => {},
foo4570: () => {},
foo4571: () => {},
foo4572: () => {},
foo4573: () => {},
foo4574: () => {},
foo4575: () => {},
foo4576: () => {},
foo4577: () => {},
foo4578: () => {},
foo4579: () => {},
foo4580: () => {},
foo4581: () => {},
foo4582: () => {},
foo4583: () => {},
foo4584: () => {},
foo4585: () => {},
foo4586: () => {},
foo4587: () => {},
foo4588: () => {},
foo4589: () => {},
foo4590: () => {},
foo4591: () => {},
foo4592: () => {},
foo4593: () => {},
foo4594: () => {},
foo4595: () => {},
foo4596: () => {},
foo4597: () => {},
foo4598: () => {},
foo4599: () => {},
foo4600: () => {},
foo4601: () => {},
foo4602: () => {},
foo4603: () => {},
foo4604: () => {},
foo4605: () => {},
foo4606: () => {},
foo4607: () => {},
foo4608: () => {},
foo4609: () => {},
foo4610: () => {},
foo4611: () => {},
foo4612: () => {},
foo4613: () => {},
foo4614: () => {},
foo4615: () => {},
foo4616: () => {},
foo4617: () => {},
foo4618: () => {},
foo4619: () => {},
foo4620: () => {},
foo4621: () => {},
foo4622: () => {},
foo4623: () => {},
foo4624: () => {},
foo4625: () => {},
foo4626: () => {},
foo4627: () => {},
foo4628: () => {},
foo4629: () => {},
foo4630: () => {},
foo4631: () => {},
foo4632: () => {},
foo4633: () => {},
foo4634: () => {},
foo4635: () => {},
foo4636: () => {},
foo4637: () => {},
foo4638: () => {},
foo4639: () => {},
foo4640: () => {},
foo4641: () => {},
foo4642: () => {},
foo4643: () => {},
foo4644: () => {},
foo4645: () => {},
foo4646: () => {},
foo4647: () => {},
foo4648: () => {},
foo4649: () => {},
foo4650: () => {},
foo4651: () => {},
foo4652: () => {},
foo4653: () => {},
foo4654: () => {},
foo4655: () => {},
foo4656: () => {},
foo4657: () => {},
foo4658: () => {},
foo4659: () => {},
foo4660: () => {},
foo4661: () => {},
foo4662: () => {},
foo4663: () => {},
foo4664: () => {},
foo4665: () => {},
foo4666: () => {},
foo4667: () => {},
foo4668: () => {},
foo4669: () => {},
foo4670: () => {},
foo4671: () => {},
foo4672: () => {},
foo4673: () => {},
foo4674: () => {},
foo4675: () => {},
foo4676: () => {},
foo4677: () => {},
foo4678: () => {},
foo4679: () => {},
foo4680: () => {},
foo4681: () => {},
foo4682: () => {},
foo4683: () => {},
foo4684: () => {},
foo4685: () => {},
foo4686: () => {},
foo4687: () => {},
foo4688: () => {},
foo4689: () => {},
foo4690: () => {},
foo4691: () => {},
foo4692: () => {},
foo4693: () => {},
foo4694: () => {},
foo4695: () => {},
foo4696: () => {},
foo4697: () => {},
foo4698: () => {},
foo4699: () => {},
foo4700: () => {},
foo4701: () => {},
foo4702: () => {},
foo4703: () => {},
foo4704: () => {},
foo4705: () => {},
foo4706: () => {},
foo4707: () => {},
foo4708: () => {},
foo4709: () => {},
foo4710: () => {},
foo4711: () => {},
foo4712: () => {},
foo4713: () => {},
foo4714: () => {},
foo4715: () => {},
foo4716: () => {},
foo4717: () => {},
foo4718: () => {},
foo4719: () => {},
foo4720: () => {},
foo4721: () => {},
foo4722: () => {},
foo4723: () => {},
foo4724: () => {},
foo4725: () => {},
foo4726: () => {},
foo4727: () => {},
foo4728: () => {},
foo4729: () => {},
foo4730: () => {},
foo4731: () => {},
foo4732: () => {},
foo4733: () => {},
foo4734: () => {},
foo4735: () => {},
foo4736: () => {},
foo4737: () => {},
foo4738: () => {},
foo4739: () => {},
foo4740: () => {},
foo4741: () => {},
foo4742: () => {},
foo4743: () => {},
foo4744: () => {},
foo4745: () => {},
foo4746: () => {},
foo4747: () => {},
foo4748: () => {},
foo4749: () => {},
foo4750: () => {},
foo4751: () => {},
foo4752: () => {},
foo4753: () => {},
foo4754: () => {},
foo4755: () => {},
foo4756: () => {},
foo4757: () => {},
foo4758: () => {},
foo4759: () => {},
foo4760: () => {},
foo4761: () => {},
foo4762: () => {},
foo4763: () => {},
foo4764: () => {},
foo4765: () => {},
foo4766: () => {},
foo4767: () => {},
foo4768: () => {},
foo4769: () => {},
foo4770: () => {},
foo4771: () => {},
foo4772: () => {},
foo4773: () => {},
foo4774: () => {},
foo4775: () => {},
foo4776: () => {},
foo4777: () => {},
foo4778: () => {},
foo4779: () => {},
foo4780: () => {},
foo4781: () => {},
foo4782: () => {},
foo4783: () => {},
foo4784: () => {},
foo4785: () => {},
foo4786: () => {},
foo4787: () => {},
foo4788: () => {},
foo4789: () => {},
foo4790: () => {},
foo4791: () => {},
foo4792: () => {},
foo4793: () => {},
foo4794: () => {},
foo4795: () => {},
foo4796: () => {},
foo4797: () => {},
foo4798: () => {},
foo4799: () => {},
foo4800: () => {},
foo4801: () => {},
foo4802: () => {},
foo4803: () => {},
foo4804: () => {},
foo4805: () => {},
foo4806: () => {},
foo4807: () => {},
foo4808: () => {},
foo4809: () => {},
foo4810: () => {},
foo4811: () => {},
foo4812: () => {},
foo4813: () => {},
foo4814: () => {},
foo4815: () => {},
foo4816: () => {},
foo4817: () => {},
foo4818: () => {},
foo4819: () => {},
foo4820: () => {},
foo4821: () => {},
foo4822: () => {},
foo4823: () => {},
foo4824: () => {},
foo4825: () => {},
foo4826: () => {},
foo4827: () => {},
foo4828: () => {},
foo4829: () => {},
foo4830: () => {},
foo4831: () => {},
foo4832: () => {},
foo4833: () => {},
foo4834: () => {},
foo4835: () => {},
foo4836: () => {},
foo4837: () => {},
foo4838: () => {},
foo4839: () => {},
foo4840: () => {},
foo4841: () => {},
foo4842: () => {},
foo4843: () => {},
foo4844: () => {},
foo4845: () => {},
foo4846: () => {},
foo4847: () => {},
foo4848: () => {},
foo4849: () => {},
foo4850: () => {},
foo4851: () => {},
foo4852: () => {},
foo4853: () => {},
foo4854: () => {},
foo4855: () => {},
foo4856: () => {},
foo4857: () => {},
foo4858: () => {},
foo4859: () => {},
foo4860: () => {},
foo4861: () => {},
foo4862: () => {},
foo4863: () => {},
foo4864: () => {},
foo4865: () => {},
foo4866: () => {},
foo4867: () => {},
foo4868: () => {},
foo4869: () => {},
foo4870: () => {},
foo4871: () => {},
foo4872: () => {},
foo4873: () => {},
foo4874: () => {},
foo4875: () => {},
foo4876: () => {},
foo4877: () => {},
foo4878: () => {},
foo4879: () => {},
foo4880: () => {},
foo4881: () => {},
foo4882: () => {},
foo4883: () => {},
foo4884: () => {},
foo4885: () => {},
foo4886: () => {},
foo4887: () => {},
foo4888: () => {},
foo4889: () => {},
foo4890: () => {},
foo4891: () => {},
foo4892: () => {},
foo4893: () => {},
foo4894: () => {},
foo4895: () => {},
foo4896: () => {},
foo4897: () => {},
foo4898: () => {},
foo4899: () => {},
foo4900: () => {},
foo4901: () => {},
foo4902: () => {},
foo4903: () => {},
foo4904: () => {},
foo4905: () => {},
foo4906: () => {},
foo4907: () => {},
foo4908: () => {},
foo4909: () => {},
foo4910: () => {},
foo4911: () => {},
foo4912: () => {},
foo4913: () => {},
foo4914: () => {},
foo4915: () => {},
foo4916: () => {},
foo4917: () => {},
foo4918: () => {},
foo4919: () => {},
foo4920: () => {},
foo4921: () => {},
foo4922: () => {},
foo4923: () => {},
foo4924: () => {},
foo4925: () => {},
foo4926: () => {},
foo4927: () => {},
foo4928: () => {},
foo4929: () => {},
foo4930: () => {},
foo4931: () => {},
foo4932: () => {},
foo4933: () => {},
foo4934: () => {},
foo4935: () => {},
foo4936: () => {},
foo4937: () => {},
foo4938: () => {},
foo4939: () => {},
foo4940: () => {},
foo4941: () => {},
foo4942: () => {},
foo4943: () => {},
foo4944: () => {},
foo4945: () => {},
foo4946: () => {},
foo4947: () => {},
foo4948: () => {},
foo4949: () => {},
foo4950: () => {},
foo4951: () => {},
foo4952: () => {},
foo4953: () => {},
foo4954: () => {},
foo4955: () => {},
foo4956: () => {},
foo4957: () => {},
foo4958: () => {},
foo4959: () => {},
foo4960: () => {},
foo4961: () => {},
foo4962: () => {},
foo4963: () => {},
foo4964: () => {},
foo4965: () => {},
foo4966: () => {},
foo4967: () => {},
foo4968: () => {},
foo4969: () => {},
foo4970: () => {},
foo4971: () => {},
foo4972: () => {},
foo4973: () => {},
foo4974: () => {},
foo4975: () => {},
foo4976: () => {},
foo4977: () => {},
foo4978: () => {},
foo4979: () => {},
foo4980: () => {},
foo4981: () => {},
foo4982: () => {},
foo4983: () => {},
foo4984: () => {},
foo4985: () => {},
foo4986: () => {},
foo4987: () => {},
foo4988: () => {},
foo4989: () => {},
foo4990: () => {},
foo4991: () => {},
foo4992: () => {},
foo4993: () => {},
foo4994: () => {},
foo4995: () => {},
foo4996: () => {},
foo4997: () => {},
foo4998: () => {},
foo4999: () => {},
foo5000: () => {},
foo5001: () => {},
foo5002: () => {},
foo5003: () => {},
foo5004: () => {},
foo5005: () => {},
foo5006: () => {},
foo5007: () => {},
foo5008: () => {},
foo5009: () => {},
foo5010: () => {},
foo5011: () => {},
foo5012: () => {},
foo5013: () => {},
foo5014: () => {},
foo5015: () => {},
foo5016: () => {},
foo5017: () => {},
foo5018: () => {},
foo5019: () => {},
foo5020: () => {},
foo5021: () => {},
foo5022: () => {},
foo5023: () => {},
foo5024: () => {},
foo5025: () => {},
foo5026: () => {},
foo5027: () => {},
foo5028: () => {},
foo5029: () => {},
foo5030: () => {},
foo5031: () => {},
foo5032: () => {},
foo5033: () => {},
foo5034: () => {},
foo5035: () => {},
foo5036: () => {},
foo5037: () => {},
foo5038: () => {},
foo5039: () => {},
foo5040: () => {},
foo5041: () => {},
foo5042: () => {},
foo5043: () => {},
foo5044: () => {},
foo5045: () => {},
foo5046: () => {},
foo5047: () => {},
foo5048: () => {},
foo5049: () => {},
foo5050: () => {},
foo5051: () => {},
foo5052: () => {},
foo5053: () => {},
foo5054: () => {},
foo5055: () => {},
foo5056: () => {},
foo5057: () => {},
foo5058: () => {},
foo5059: () => {},
foo5060: () => {},
foo5061: () => {},
foo5062: () => {},
foo5063: () => {},
foo5064: () => {},
foo5065: () => {},
foo5066: () => {},
foo5067: () => {},
foo5068: () => {},
foo5069: () => {},
foo5070: () => {},
foo5071: () => {},
foo5072: () => {},
foo5073: () => {},
foo5074: () => {},
foo5075: () => {},
foo5076: () => {},
foo5077: () => {},
foo5078: () => {},
foo5079: () => {},
foo5080: () => {},
foo5081: () => {},
foo5082: () => {},
foo5083: () => {},
foo5084: () => {},
foo5085: () => {},
foo5086: () => {},
foo5087: () => {},
foo5088: () => {},
foo5089: () => {},
foo5090: () => {},
foo5091: () => {},
foo5092: () => {},
foo5093: () => {},
foo5094: () => {},
foo5095: () => {},
foo5096: () => {},
foo5097: () => {},
foo5098: () => {},
foo5099: () => {},
foo5100: () => {},
foo5101: () => {},
foo5102: () => {},
foo5103: () => {},
foo5104: () => {},
foo5105: () => {},
foo5106: () => {},
foo5107: () => {},
foo5108: () => {},
foo5109: () => {},
foo5110: () => {},
foo5111: () => {},
foo5112: () => {},
foo5113: () => {},
foo5114: () => {},
foo5115: () => {},
foo5116: () => {},
foo5117: () => {},
foo5118: () => {},
foo5119: () => {},
foo5120: () => {},
foo5121: () => {},
foo5122: () => {},
foo5123: () => {},
foo5124: () => {},
foo5125: () => {},
foo5126: () => {},
foo5127: () => {},
foo5128: () => {},
foo5129: () => {},
foo5130: () => {},
foo5131: () => {},
foo5132: () => {},
foo5133: () => {},
foo5134: () => {},
foo5135: () => {},
foo5136: () => {},
foo5137: () => {},
foo5138: () => {},
foo5139: () => {},
foo5140: () => {},
foo5141: () => {},
foo5142: () => {},
foo5143: () => {},
foo5144: () => {},
foo5145: () => {},
foo5146: () => {},
foo5147: () => {},
foo5148: () => {},
foo5149: () => {},
foo5150: () => {},
foo5151: () => {},
foo5152: () => {},
foo5153: () => {},
foo5154: () => {},
foo5155: () => {},
foo5156: () => {},
foo5157: () => {},
foo5158: () => {},
foo5159: () => {},
foo5160: () => {},
foo5161: () => {},
foo5162: () => {},
foo5163: () => {},
foo5164: () => {},
foo5165: () => {},
foo5166: () => {},
foo5167: () => {},
foo5168: () => {},
foo5169: () => {},
foo5170: () => {},
foo5171: () => {},
foo5172: () => {},
foo5173: () => {},
foo5174: () => {},
foo5175: () => {},
foo5176: () => {},
foo5177: () => {},
foo5178: () => {},
foo5179: () => {},
foo5180: () => {},
foo5181: () => {},
foo5182: () => {},
foo5183: () => {},
foo5184: () => {},
foo5185: () => {},
foo5186: () => {},
foo5187: () => {},
foo5188: () => {},
foo5189: () => {},
foo5190: () => {},
foo5191: () => {},
foo5192: () => {},
foo5193: () => {},
foo5194: () => {},
foo5195: () => {},
foo5196: () => {},
foo5197: () => {},
foo5198: () => {},
foo5199: () => {},
foo5200: () => {},
foo5201: () => {},
foo5202: () => {},
foo5203: () => {},
foo5204: () => {},
foo5205: () => {},
foo5206: () => {},
foo5207: () => {},
foo5208: () => {},
foo5209: () => {},
foo5210: () => {},
foo5211: () => {},
foo5212: () => {},
foo5213: () => {},
foo5214: () => {},
foo5215: () => {},
foo5216: () => {},
foo5217: () => {},
foo5218: () => {},
foo5219: () => {},
foo5220: () => {},
foo5221: () => {},
foo5222: () => {},
foo5223: () => {},
foo5224: () => {},
foo5225: () => {},
foo5226: () => {},
foo5227: () => {},
foo5228: () => {},
foo5229: () => {},
foo5230: () => {},
foo5231: () => {},
foo5232: () => {},
foo5233: () => {},
foo5234: () => {},
foo5235: () => {},
foo5236: () => {},
foo5237: () => {},
foo5238: () => {},
foo5239: () => {},
foo5240: () => {},
foo5241: () => {},
foo5242: () => {},
foo5243: () => {},
foo5244: () => {},
foo5245: () => {},
foo5246: () => {},
foo5247: () => {},
foo5248: () => {},
foo5249: () => {},
foo5250: () => {},
foo5251: () => {},
foo5252: () => {},
foo5253: () => {},
foo5254: () => {},
foo5255: () => {},
foo5256: () => {},
foo5257: () => {},
foo5258: () => {},
foo5259: () => {},
foo5260: () => {},
foo5261: () => {},
foo5262: () => {},
foo5263: () => {},
foo5264: () => {},
foo5265: () => {},
foo5266: () => {},
foo5267: () => {},
foo5268: () => {},
foo5269: () => {},
foo5270: () => {},
foo5271: () => {},
foo5272: () => {},
foo5273: () => {},
foo5274: () => {},
foo5275: () => {},
foo5276: () => {},
foo5277: () => {},
foo5278: () => {},
foo5279: () => {},
foo5280: () => {},
foo5281: () => {},
foo5282: () => {},
foo5283: () => {},
foo5284: () => {},
foo5285: () => {},
foo5286: () => {},
foo5287: () => {},
foo5288: () => {},
foo5289: () => {},
foo5290: () => {},
foo5291: () => {},
foo5292: () => {},
foo5293: () => {},
foo5294: () => {},
foo5295: () => {},
foo5296: () => {},
foo5297: () => {},
foo5298: () => {},
foo5299: () => {},
foo5300: () => {},
foo5301: () => {},
foo5302: () => {},
foo5303: () => {},
foo5304: () => {},
foo5305: () => {},
foo5306: () => {},
foo5307: () => {},
foo5308: () => {},
foo5309: () => {},
foo5310: () => {},
foo5311: () => {},
foo5312: () => {},
foo5313: () => {},
foo5314: () => {},
foo5315: () => {},
foo5316: () => {},
foo5317: () => {},
foo5318: () => {},
foo5319: () => {},
foo5320: () => {},
foo5321: () => {},
foo5322: () => {},
foo5323: () => {},
foo5324: () => {},
foo5325: () => {},
foo5326: () => {},
foo5327: () => {},
foo5328: () => {},
foo5329: () => {},
foo5330: () => {},
foo5331: () => {},
foo5332: () => {},
foo5333: () => {},
foo5334: () => {},
foo5335: () => {},
foo5336: () => {},
foo5337: () => {},
foo5338: () => {},
foo5339: () => {},
foo5340: () => {},
foo5341: () => {},
foo5342: () => {},
foo5343: () => {},
foo5344: () => {},
foo5345: () => {},
foo5346: () => {},
foo5347: () => {},
foo5348: () => {},
foo5349: () => {},
foo5350: () => {},
foo5351: () => {},
foo5352: () => {},
foo5353: () => {},
foo5354: () => {},
foo5355: () => {},
foo5356: () => {},
foo5357: () => {},
foo5358: () => {},
foo5359: () => {},
foo5360: () => {},
foo5361: () => {},
foo5362: () => {},
foo5363: () => {},
foo5364: () => {},
foo5365: () => {},
foo5366: () => {},
foo5367: () => {},
foo5368: () => {},
foo5369: () => {},
foo5370: () => {},
foo5371: () => {},
foo5372: () => {},
foo5373: () => {},
foo5374: () => {},
foo5375: () => {},
foo5376: () => {},
foo5377: () => {},
foo5378: () => {},
foo5379: () => {},
foo5380: () => {},
foo5381: () => {},
foo5382: () => {},
foo5383: () => {},
foo5384: () => {},
foo5385: () => {},
foo5386: () => {},
foo5387: () => {},
foo5388: () => {},
foo5389: () => {},
foo5390: () => {},
foo5391: () => {},
foo5392: () => {},
foo5393: () => {},
foo5394: () => {},
foo5395: () => {},
foo5396: () => {},
foo5397: () => {},
foo5398: () => {},
foo5399: () => {},
foo5400: () => {},
foo5401: () => {},
foo5402: () => {},
foo5403: () => {},
foo5404: () => {},
foo5405: () => {},
foo5406: () => {},
foo5407: () => {},
foo5408: () => {},
foo5409: () => {},
foo5410: () => {},
foo5411: () => {},
foo5412: () => {},
foo5413: () => {},
foo5414: () => {},
foo5415: () => {},
foo5416: () => {},
foo5417: () => {},
foo5418: () => {},
foo5419: () => {},
foo5420: () => {},
foo5421: () => {},
foo5422: () => {},
foo5423: () => {},
foo5424: () => {},
foo5425: () => {},
foo5426: () => {},
foo5427: () => {},
foo5428: () => {},
foo5429: () => {},
foo5430: () => {},
foo5431: () => {},
foo5432: () => {},
foo5433: () => {},
foo5434: () => {},
foo5435: () => {},
foo5436: () => {},
foo5437: () => {},
foo5438: () => {},
foo5439: () => {},
foo5440: () => {},
foo5441: () => {},
foo5442: () => {},
foo5443: () => {},
foo5444: () => {},
foo5445: () => {},
foo5446: () => {},
foo5447: () => {},
foo5448: () => {},
foo5449: () => {},
foo5450: () => {},
foo5451: () => {},
foo5452: () => {},
foo5453: () => {},
foo5454: () => {},
foo5455: () => {},
foo5456: () => {},
foo5457: () => {},
foo5458: () => {},
foo5459: () => {},
foo5460: () => {},
foo5461: () => {},
foo5462: () => {},
foo5463: () => {},
foo5464: () => {},
foo5465: () => {},
foo5466: () => {},
foo5467: () => {},
foo5468: () => {},
foo5469: () => {},
foo5470: () => {},
foo5471: () => {},
foo5472: () => {},
foo5473: () => {},
foo5474: () => {},
foo5475: () => {},
foo5476: () => {},
foo5477: () => {},
foo5478: () => {},
foo5479: () => {},
foo5480: () => {},
foo5481: () => {},
foo5482: () => {},
foo5483: () => {},
foo5484: () => {},
foo5485: () => {},
foo5486: () => {},
foo5487: () => {},
foo5488: () => {},
foo5489: () => {},
foo5490: () => {},
foo5491: () => {},
foo5492: () => {},
foo5493: () => {},
foo5494: () => {},
foo5495: () => {},
foo5496: () => {},
foo5497: () => {},
foo5498: () => {},
foo5499: () => {},
foo5500: () => {},
foo5501: () => {},
foo5502: () => {},
foo5503: () => {},
foo5504: () => {},
foo5505: () => {},
foo5506: () => {},
foo5507: () => {},
foo5508: () => {},
foo5509: () => {},
foo5510: () => {},
foo5511: () => {},
foo5512: () => {},
foo5513: () => {},
foo5514: () => {},
foo5515: () => {},
foo5516: () => {},
foo5517: () => {},
foo5518: () => {},
foo5519: () => {},
foo5520: () => {},
foo5521: () => {},
foo5522: () => {},
foo5523: () => {},
foo5524: () => {},
foo5525: () => {},
foo5526: () => {},
foo5527: () => {},
foo5528: () => {},
foo5529: () => {},
foo5530: () => {},
foo5531: () => {},
foo5532: () => {},
foo5533: () => {},
foo5534: () => {},
foo5535: () => {},
foo5536: () => {},
foo5537: () => {},
foo5538: () => {},
foo5539: () => {},
foo5540: () => {},
foo5541: () => {},
foo5542: () => {},
foo5543: () => {},
foo5544: () => {},
foo5545: () => {},
foo5546: () => {},
foo5547: () => {},
foo5548: () => {},
foo5549: () => {},
foo5550: () => {},
foo5551: () => {},
foo5552: () => {},
foo5553: () => {},
foo5554: () => {},
foo5555: () => {},
foo5556: () => {},
foo5557: () => {},
foo5558: () => {},
foo5559: () => {},
foo5560: () => {},
foo5561: () => {},
foo5562: () => {},
foo5563: () => {},
foo5564: () => {},
foo5565: () => {},
foo5566: () => {},
foo5567: () => {},
foo5568: () => {},
foo5569: () => {},
foo5570: () => {},
foo5571: () => {},
foo5572: () => {},
foo5573: () => {},
foo5574: () => {},
foo5575: () => {},
foo5576: () => {},
foo5577: () => {},
foo5578: () => {},
foo5579: () => {},
foo5580: () => {},
foo5581: () => {},
foo5582: () => {},
foo5583: () => {},
foo5584: () => {},
foo5585: () => {},
foo5586: () => {},
foo5587: () => {},
foo5588: () => {},
foo5589: () => {},
foo5590: () => {},
foo5591: () => {},
foo5592: () => {},
foo5593: () => {},
foo5594: () => {},
foo5595: () => {},
foo5596: () => {},
foo5597: () => {},
foo5598: () => {},
foo5599: () => {},
foo5600: () => {},
foo5601: () => {},
foo5602: () => {},
foo5603: () => {},
foo5604: () => {},
foo5605: () => {},
foo5606: () => {},
foo5607: () => {},
foo5608: () => {},
foo5609: () => {},
foo5610: () => {},
foo5611: () => {},
foo5612: () => {},
foo5613: () => {},
foo5614: () => {},
foo5615: () => {},
foo5616: () => {},
foo5617: () => {},
foo5618: () => {},
foo5619: () => {},
foo5620: () => {},
foo5621: () => {},
foo5622: () => {},
foo5623: () => {},
foo5624: () => {},
foo5625: () => {},
foo5626: () => {},
foo5627: () => {},
foo5628: () => {},
foo5629: () => {},
foo5630: () => {},
foo5631: () => {},
foo5632: () => {},
foo5633: () => {},
foo5634: () => {},
foo5635: () => {},
foo5636: () => {},
foo5637: () => {},
foo5638: () => {},
foo5639: () => {},
foo5640: () => {},
foo5641: () => {},
foo5642: () => {},
foo5643: () => {},
foo5644: () => {},
foo5645: () => {},
foo5646: () => {},
foo5647: () => {},
foo5648: () => {},
foo5649: () => {},
foo5650: () => {},
foo5651: () => {},
foo5652: () => {},
foo5653: () => {},
foo5654: () => {},
foo5655: () => {},
foo5656: () => {},
foo5657: () => {},
foo5658: () => {},
foo5659: () => {},
foo5660: () => {},
foo5661: () => {},
foo5662: () => {},
foo5663: () => {},
foo5664: () => {},
foo5665: () => {},
foo5666: () => {},
foo5667: () => {},
foo5668: () => {},
foo5669: () => {},
foo5670: () => {},
foo5671: () => {},
foo5672: () => {},
foo5673: () => {},
foo5674: () => {},
foo5675: () => {},
foo5676: () => {},
foo5677: () => {},
foo5678: () => {},
foo5679: () => {},
foo5680: () => {},
foo5681: () => {},
foo5682: () => {},
foo5683: () => {},
foo5684: () => {},
foo5685: () => {},
foo5686: () => {},
foo5687: () => {},
foo5688: () => {},
foo5689: () => {},
foo5690: () => {},
foo5691: () => {},
foo5692: () => {},
foo5693: () => {},
foo5694: () => {},
foo5695: () => {},
foo5696: () => {},
foo5697: () => {},
foo5698: () => {},
foo5699: () => {},
foo5700: () => {},
foo5701: () => {},
foo5702: () => {},
foo5703: () => {},
foo5704: () => {},
foo5705: () => {},
foo5706: () => {},
foo5707: () => {},
foo5708: () => {},
foo5709: () => {},
foo5710: () => {},
foo5711: () => {},
foo5712: () => {},
foo5713: () => {},
foo5714: () => {},
foo5715: () => {},
foo5716: () => {},
foo5717: () => {},
foo5718: () => {},
foo5719: () => {},
foo5720: () => {},
foo5721: () => {},
foo5722: () => {},
foo5723: () => {},
foo5724: () => {},
foo5725: () => {},
foo5726: () => {},
foo5727: () => {},
foo5728: () => {},
foo5729: () => {},
foo5730: () => {},
foo5731: () => {},
foo5732: () => {},
foo5733: () => {},
foo5734: () => {},
foo5735: () => {},
foo5736: () => {},
foo5737: () => {},
foo5738: () => {},
foo5739: () => {},
foo5740: () => {},
foo5741: () => {},
foo5742: () => {},
foo5743: () => {},
foo5744: () => {},
foo5745: () => {},
foo5746: () => {},
foo5747: () => {},
foo5748: () => {},
foo5749: () => {},
foo5750: () => {},
foo5751: () => {},
foo5752: () => {},
foo5753: () => {},
foo5754: () => {},
foo5755: () => {},
foo5756: () => {},
foo5757: () => {},
foo5758: () => {},
foo5759: () => {},
foo5760: () => {},
foo5761: () => {},
foo5762: () => {},
foo5763: () => {},
foo5764: () => {},
foo5765: () => {},
foo5766: () => {},
foo5767: () => {},
foo5768: () => {},
foo5769: () => {},
foo5770: () => {},
foo5771: () => {},
foo5772: () => {},
foo5773: () => {},
foo5774: () => {},
foo5775: () => {},
foo5776: () => {},
foo5777: () => {},
foo5778: () => {},
foo5779: () => {},
foo5780: () => {},
foo5781: () => {},
foo5782: () => {},
foo5783: () => {},
foo5784: () => {},
foo5785: () => {},
foo5786: () => {},
foo5787: () => {},
foo5788: () => {},
foo5789: () => {},
foo5790: () => {},
foo5791: () => {},
foo5792: () => {},
foo5793: () => {},
foo5794: () => {},
foo5795: () => {},
foo5796: () => {},
foo5797: () => {},
foo5798: () => {},
foo5799: () => {},
foo5800: () => {},
foo5801: () => {},
foo5802: () => {},
foo5803: () => {},
foo5804: () => {},
foo5805: () => {},
foo5806: () => {},
foo5807: () => {},
foo5808: () => {},
foo5809: () => {},
foo5810: () => {},
foo5811: () => {},
foo5812: () => {},
foo5813: () => {},
foo5814: () => {},
foo5815: () => {},
foo5816: () => {},
foo5817: () => {},
foo5818: () => {},
foo5819: () => {},
foo5820: () => {},
foo5821: () => {},
foo5822: () => {},
foo5823: () => {},
foo5824: () => {},
foo5825: () => {},
foo5826: () => {},
foo5827: () => {},
foo5828: () => {},
foo5829: () => {},
foo5830: () => {},
foo5831: () => {},
foo5832: () => {},
foo5833: () => {},
foo5834: () => {},
foo5835: () => {},
foo5836: () => {},
foo5837: () => {},
foo5838: () => {},
foo5839: () => {},
foo5840: () => {},
foo5841: () => {},
foo5842: () => {},
foo5843: () => {},
foo5844: () => {},
foo5845: () => {},
foo5846: () => {},
foo5847: () => {},
foo5848: () => {},
foo5849: () => {},
foo5850: () => {},
foo5851: () => {},
foo5852: () => {},
foo5853: () => {},
foo5854: () => {},
foo5855: () => {},
foo5856: () => {},
foo5857: () => {},
foo5858: () => {},
foo5859: () => {},
foo5860: () => {},
foo5861: () => {},
foo5862: () => {},
foo5863: () => {},
foo5864: () => {},
foo5865: () => {},
foo5866: () => {},
foo5867: () => {},
foo5868: () => {},
foo5869: () => {},
foo5870: () => {},
foo5871: () => {},
foo5872: () => {},
foo5873: () => {},
foo5874: () => {},
foo5875: () => {},
foo5876: () => {},
foo5877: () => {},
foo5878: () => {},
foo5879: () => {},
foo5880: () => {},
foo5881: () => {},
foo5882: () => {},
foo5883: () => {},
foo5884: () => {},
foo5885: () => {},
foo5886: () => {},
foo5887: () => {},
foo5888: () => {},
foo5889: () => {},
foo5890: () => {},
foo5891: () => {},
foo5892: () => {},
foo5893: () => {},
foo5894: () => {},
foo5895: () => {},
foo5896: () => {},
foo5897: () => {},
foo5898: () => {},
foo5899: () => {},
foo5900: () => {},
foo5901: () => {},
foo5902: () => {},
foo5903: () => {},
foo5904: () => {},
foo5905: () => {},
foo5906: () => {},
foo5907: () => {},
foo5908: () => {},
foo5909: () => {},
foo5910: () => {},
foo5911: () => {},
foo5912: () => {},
foo5913: () => {},
foo5914: () => {},
foo5915: () => {},
foo5916: () => {},
foo5917: () => {},
foo5918: () => {},
foo5919: () => {},
foo5920: () => {},
foo5921: () => {},
foo5922: () => {},
foo5923: () => {},
foo5924: () => {},
foo5925: () => {},
foo5926: () => {},
foo5927: () => {},
foo5928: () => {},
foo5929: () => {},
foo5930: () => {},
foo5931: () => {},
foo5932: () => {},
foo5933: () => {},
foo5934: () => {},
foo5935: () => {},
foo5936: () => {},
foo5937: () => {},
foo5938: () => {},
foo5939: () => {},
foo5940: () => {},
foo5941: () => {},
foo5942: () => {},
foo5943: () => {},
foo5944: () => {},
foo5945: () => {},
foo5946: () => {},
foo5947: () => {},
foo5948: () => {},
foo5949: () => {},
foo5950: () => {},
foo5951: () => {},
foo5952: () => {},
foo5953: () => {},
foo5954: () => {},
foo5955: () => {},
foo5956: () => {},
foo5957: () => {},
foo5958: () => {},
foo5959: () => {},
foo5960: () => {},
foo5961: () => {},
foo5962: () => {},
foo5963: () => {},
foo5964: () => {},
foo5965: () => {},
foo5966: () => {},
foo5967: () => {},
foo5968: () => {},
foo5969: () => {},
foo5970: () => {},
foo5971: () => {},
foo5972: () => {},
foo5973: () => {},
foo5974: () => {},
foo5975: () => {},
foo5976: () => {},
foo5977: () => {},
foo5978: () => {},
foo5979: () => {},
foo5980: () => {},
foo5981: () => {},
foo5982: () => {},
foo5983: () => {},
foo5984: () => {},
foo5985: () => {},
foo5986: () => {},
foo5987: () => {},
foo5988: () => {},
foo5989: () => {},
foo5990: () => {},
foo5991: () => {},
foo5992: () => {},
foo5993: () => {},
foo5994: () => {},
foo5995: () => {},
foo5996: () => {},
foo5997: () => {},
foo5998: () => {},
foo5999: () => {},
foo6000: () => {},
foo6001: () => {},
foo6002: () => {},
foo6003: () => {},
foo6004: () => {},
foo6005: () => {},
foo6006: () => {},
foo6007: () => {},
foo6008: () => {},
foo6009: () => {},
foo6010: () => {},
foo6011: () => {},
foo6012: () => {},
foo6013: () => {},
foo6014: () => {},
foo6015: () => {},
foo6016: () => {},
foo6017: () => {},
foo6018: () => {},
foo6019: () => {},
foo6020: () => {},
foo6021: () => {},
foo6022: () => {},
foo6023: () => {},
foo6024: () => {},
foo6025: () => {},
foo6026: () => {},
foo6027: () => {},
foo6028: () => {},
foo6029: () => {},
foo6030: () => {},
foo6031: () => {},
foo6032: () => {},
foo6033: () => {},
foo6034: () => {},
foo6035: () => {},
foo6036: () => {},
foo6037: () => {},
foo6038: () => {},
foo6039: () => {},
foo6040: () => {},
foo6041: () => {},
foo6042: () => {},
foo6043: () => {},
foo6044: () => {},
foo6045: () => {},
foo6046: () => {},
foo6047: () => {},
foo6048: () => {},
foo6049: () => {},
foo6050: () => {},
foo6051: () => {},
foo6052: () => {},
foo6053: () => {},
foo6054: () => {},
foo6055: () => {},
foo6056: () => {},
foo6057: () => {},
foo6058: () => {},
foo6059: () => {},
foo6060: () => {},
foo6061: () => {},
foo6062: () => {},
foo6063: () => {},
foo6064: () => {},
foo6065: () => {},
foo6066: () => {},
foo6067: () => {},
foo6068: () => {},
foo6069: () => {},
foo6070: () => {},
foo6071: () => {},
foo6072: () => {},
foo6073: () => {},
foo6074: () => {},
foo6075: () => {},
foo6076: () => {},
foo6077: () => {},
foo6078: () => {},
foo6079: () => {},
foo6080: () => {},
foo6081: () => {},
foo6082: () => {},
foo6083: () => {},
foo6084: () => {},
foo6085: () => {},
foo6086: () => {},
foo6087: () => {},
foo6088: () => {},
foo6089: () => {},
foo6090: () => {},
foo6091: () => {},
foo6092: () => {},
foo6093: () => {},
foo6094: () => {},
foo6095: () => {},
foo6096: () => {},
foo6097: () => {},
foo6098: () => {},
foo6099: () => {},
foo6100: () => {},
foo6101: () => {},
foo6102: () => {},
foo6103: () => {},
foo6104: () => {},
foo6105: () => {},
foo6106: () => {},
foo6107: () => {},
foo6108: () => {},
foo6109: () => {},
foo6110: () => {},
foo6111: () => {},
foo6112: () => {},
foo6113: () => {},
foo6114: () => {},
foo6115: () => {},
foo6116: () => {},
foo6117: () => {},
foo6118: () => {},
foo6119: () => {},
foo6120: () => {},
foo6121: () => {},
foo6122: () => {},
foo6123: () => {},
foo6124: () => {},
foo6125: () => {},
foo6126: () => {},
foo6127: () => {},
foo6128: () => {},
foo6129: () => {},
foo6130: () => {},
foo6131: () => {},
foo6132: () => {},
foo6133: () => {},
foo6134: () => {},
foo6135: () => {},
foo6136: () => {},
foo6137: () => {},
foo6138: () => {},
foo6139: () => {},
foo6140: () => {},
foo6141: () => {},
foo6142: () => {},
foo6143: () => {},
foo6144: () => {},
foo6145: () => {},
foo6146: () => {},
foo6147: () => {},
foo6148: () => {},
foo6149: () => {},
foo6150: () => {},
foo6151: () => {},
foo6152: () => {},
foo6153: () => {},
foo6154: () => {},
foo6155: () => {},
foo6156: () => {},
foo6157: () => {},
foo6158: () => {},
foo6159: () => {},
foo6160: () => {},
foo6161: () => {},
foo6162: () => {},
foo6163: () => {},
foo6164: () => {},
foo6165: () => {},
foo6166: () => {},
foo6167: () => {},
foo6168: () => {},
foo6169: () => {},
foo6170: () => {},
foo6171: () => {},
foo6172: () => {},
foo6173: () => {},
foo6174: () => {},
foo6175: () => {},
foo6176: () => {},
foo6177: () => {},
foo6178: () => {},
foo6179: () => {},
foo6180: () => {},
foo6181: () => {},
foo6182: () => {},
foo6183: () => {},
foo6184: () => {},
foo6185: () => {},
foo6186: () => {},
foo6187: () => {},
foo6188: () => {},
foo6189: () => {},
foo6190: () => {},
foo6191: () => {},
foo6192: () => {},
foo6193: () => {},
foo6194: () => {},
foo6195: () => {},
foo6196: () => {},
foo6197: () => {},
foo6198: () => {},
foo6199: () => {},
foo6200: () => {},
foo6201: () => {},
foo6202: () => {},
foo6203: () => {},
foo6204: () => {},
foo6205: () => {},
foo6206: () => {},
foo6207: () => {},
foo6208: () => {},
foo6209: () => {},
foo6210: () => {},
foo6211: () => {},
foo6212: () => {},
foo6213: () => {},
foo6214: () => {},
foo6215: () => {},
foo6216: () => {},
foo6217: () => {},
foo6218: () => {},
foo6219: () => {},
foo6220: () => {},
foo6221: () => {},
foo6222: () => {},
foo6223: () => {},
foo6224: () => {},
foo6225: () => {},
foo6226: () => {},
foo6227: () => {},
foo6228: () => {},
foo6229: () => {},
foo6230: () => {},
foo6231: () => {},
foo6232: () => {},
foo6233: () => {},
foo6234: () => {},
foo6235: () => {},
foo6236: () => {},
foo6237: () => {},
foo6238: () => {},
foo6239: () => {},
foo6240: () => {},
foo6241: () => {},
foo6242: () => {},
foo6243: () => {},
foo6244: () => {},
foo6245: () => {},
foo6246: () => {},
foo6247: () => {},
foo6248: () => {},
foo6249: () => {},
foo6250: () => {},
foo6251: () => {},
foo6252: () => {},
foo6253: () => {},
foo6254: () => {},
foo6255: () => {},
foo6256: () => {},
foo6257: () => {},
foo6258: () => {},
foo6259: () => {},
foo6260: () => {},
foo6261: () => {},
foo6262: () => {},
foo6263: () => {},
foo6264: () => {},
foo6265: () => {},
foo6266: () => {},
foo6267: () => {},
foo6268: () => {},
foo6269: () => {},
foo6270: () => {},
foo6271: () => {},
foo6272: () => {},
foo6273: () => {},
foo6274: () => {},
foo6275: () => {},
foo6276: () => {},
foo6277: () => {},
foo6278: () => {},
foo6279: () => {},
foo6280: () => {},
foo6281: () => {},
foo6282: () => {},
foo6283: () => {},
foo6284: () => {},
foo6285: () => {},
foo6286: () => {},
foo6287: () => {},
foo6288: () => {},
foo6289: () => {},
foo6290: () => {},
foo6291: () => {},
foo6292: () => {},
foo6293: () => {},
foo6294: () => {},
foo6295: () => {},
foo6296: () => {},
foo6297: () => {},
foo6298: () => {},
foo6299: () => {},
foo6300: () => {},
foo6301: () => {},
foo6302: () => {},
foo6303: () => {},
foo6304: () => {},
foo6305: () => {},
foo6306: () => {},
foo6307: () => {},
foo6308: () => {},
foo6309: () => {},
foo6310: () => {},
foo6311: () => {},
foo6312: () => {},
foo6313: () => {},
foo6314: () => {},
foo6315: () => {},
foo6316: () => {},
foo6317: () => {},
foo6318: () => {},
foo6319: () => {},
foo6320: () => {},
foo6321: () => {},
foo6322: () => {},
foo6323: () => {},
foo6324: () => {},
foo6325: () => {},
foo6326: () => {},
foo6327: () => {},
foo6328: () => {},
foo6329: () => {},
foo6330: () => {},
foo6331: () => {},
foo6332: () => {},
foo6333: () => {},
foo6334: () => {},
foo6335: () => {},
foo6336: () => {},
foo6337: () => {},
foo6338: () => {},
foo6339: () => {},
foo6340: () => {},
foo6341: () => {},
foo6342: () => {},
foo6343: () => {},
foo6344: () => {},
foo6345: () => {},
foo6346: () => {},
foo6347: () => {},
foo6348: () => {},
foo6349: () => {},
foo6350: () => {},
foo6351: () => {},
foo6352: () => {},
foo6353: () => {},
foo6354: () => {},
foo6355: () => {},
foo6356: () => {},
foo6357: () => {},
foo6358: () => {},
foo6359: () => {},
foo6360: () => {},
foo6361: () => {},
foo6362: () => {},
foo6363: () => {},
foo6364: () => {},
foo6365: () => {},
foo6366: () => {},
foo6367: () => {},
foo6368: () => {},
foo6369: () => {},
foo6370: () => {},
foo6371: () => {},
foo6372: () => {},
foo6373: () => {},
foo6374: () => {},
foo6375: () => {},
foo6376: () => {},
foo6377: () => {},
foo6378: () => {},
foo6379: () => {},
foo6380: () => {},
foo6381: () => {},
foo6382: () => {},
foo6383: () => {},
foo6384: () => {},
foo6385: () => {},
foo6386: () => {},
foo6387: () => {},
foo6388: () => {},
foo6389: () => {},
foo6390: () => {},
foo6391: () => {},
foo6392: () => {},
foo6393: () => {},
foo6394: () => {},
foo6395: () => {},
foo6396: () => {},
foo6397: () => {},
foo6398: () => {},
foo6399: () => {},
foo6400: () => {},
foo6401: () => {},
foo6402: () => {},
foo6403: () => {},
foo6404: () => {},
foo6405: () => {},
foo6406: () => {},
foo6407: () => {},
foo6408: () => {},
foo6409: () => {},
foo6410: () => {},
foo6411: () => {},
foo6412: () => {},
foo6413: () => {},
foo6414: () => {},
foo6415: () => {},
foo6416: () => {},
foo6417: () => {},
foo6418: () => {},
foo6419: () => {},
foo6420: () => {},
foo6421: () => {},
foo6422: () => {},
foo6423: () => {},
foo6424: () => {},
foo6425: () => {},
foo6426: () => {},
foo6427: () => {},
foo6428: () => {},
foo6429: () => {},
foo6430: () => {},
foo6431: () => {},
foo6432: () => {},
foo6433: () => {},
foo6434: () => {},
foo6435: () => {},
foo6436: () => {},
foo6437: () => {},
foo6438: () => {},
foo6439: () => {},
foo6440: () => {},
foo6441: () => {},
foo6442: () => {},
foo6443: () => {},
foo6444: () => {},
foo6445: () => {},
foo6446: () => {},
foo6447: () => {},
foo6448: () => {},
foo6449: () => {},
foo6450: () => {},
foo6451: () => {},
foo6452: () => {},
foo6453: () => {},
foo6454: () => {},
foo6455: () => {},
foo6456: () => {},
foo6457: () => {},
foo6458: () => {},
foo6459: () => {},
foo6460: () => {},
foo6461: () => {},
foo6462: () => {},
foo6463: () => {},
foo6464: () => {},
foo6465: () => {},
foo6466: () => {},
foo6467: () => {},
foo6468: () => {},
foo6469: () => {},
foo6470: () => {},
foo6471: () => {},
foo6472: () => {},
foo6473: () => {},
foo6474: () => {},
foo6475: () => {},
foo6476: () => {},
foo6477: () => {},
foo6478: () => {},
foo6479: () => {},
foo6480: () => {},
foo6481: () => {},
foo6482: () => {},
foo6483: () => {},
foo6484: () => {},
foo6485: () => {},
foo6486: () => {},
foo6487: () => {},
foo6488: () => {},
foo6489: () => {},
foo6490: () => {},
foo6491: () => {},
foo6492: () => {},
foo6493: () => {},
foo6494: () => {},
foo6495: () => {},
foo6496: () => {},
foo6497: () => {},
foo6498: () => {},
foo6499: () => {},
foo6500: () => {},
foo6501: () => {},
foo6502: () => {},
foo6503: () => {},
foo6504: () => {},
foo6505: () => {},
foo6506: () => {},
foo6507: () => {},
foo6508: () => {},
foo6509: () => {},
foo6510: () => {},
foo6511: () => {},
foo6512: () => {},
foo6513: () => {},
foo6514: () => {},
foo6515: () => {},
foo6516: () => {},
foo6517: () => {},
foo6518: () => {},
foo6519: () => {},
foo6520: () => {},
foo6521: () => {},
foo6522: () => {},
foo6523: () => {},
foo6524: () => {},
foo6525: () => {},
foo6526: () => {},
foo6527: () => {},
foo6528: () => {},
foo6529: () => {},
foo6530: () => {},
foo6531: () => {},
foo6532: () => {},
foo6533: () => {},
foo6534: () => {},
foo6535: () => {},
foo6536: () => {},
foo6537: () => {},
foo6538: () => {},
foo6539: () => {},
foo6540: () => {},
foo6541: () => {},
foo6542: () => {},
foo6543: () => {},
foo6544: () => {},
foo6545: () => {},
foo6546: () => {},
foo6547: () => {},
foo6548: () => {},
foo6549: () => {},
foo6550: () => {},
foo6551: () => {},
foo6552: () => {},
foo6553: () => {},
foo6554: () => {},
foo6555: () => {},
foo6556: () => {},
foo6557: () => {},
foo6558: () => {},
foo6559: () => {},
foo6560: () => {},
foo6561: () => {},
foo6562: () => {},
foo6563: () => {},
foo6564: () => {},
foo6565: () => {},
foo6566: () => {},
foo6567: () => {},
foo6568: () => {},
foo6569: () => {},
foo6570: () => {},
foo6571: () => {},
foo6572: () => {},
foo6573: () => {},
foo6574: () => {},
foo6575: () => {},
foo6576: () => {},
foo6577: () => {},
foo6578: () => {},
foo6579: () => {},
foo6580: () => {},
foo6581: () => {},
foo6582: () => {},
foo6583: () => {},
foo6584: () => {},
foo6585: () => {},
foo6586: () => {},
foo6587: () => {},
foo6588: () => {},
foo6589: () => {},
foo6590: () => {},
foo6591: () => {},
foo6592: () => {},
foo6593: () => {},
foo6594: () => {},
foo6595: () => {},
foo6596: () => {},
foo6597: () => {},
foo6598: () => {},
foo6599: () => {},
foo6600: () => {},
foo6601: () => {},
foo6602: () => {},
foo6603: () => {},
foo6604: () => {},
foo6605: () => {},
foo6606: () => {},
foo6607: () => {},
foo6608: () => {},
foo6609: () => {},
foo6610: () => {},
foo6611: () => {},
foo6612: () => {},
foo6613: () => {},
foo6614: () => {},
foo6615: () => {},
foo6616: () => {},
foo6617: () => {},
foo6618: () => {},
foo6619: () => {},
foo6620: () => {},
foo6621: () => {},
foo6622: () => {},
foo6623: () => {},
foo6624: () => {},
foo6625: () => {},
foo6626: () => {},
foo6627: () => {},
foo6628: () => {},
foo6629: () => {},
foo6630: () => {},
foo6631: () => {},
foo6632: () => {},
foo6633: () => {},
foo6634: () => {},
foo6635: () => {},
foo6636: () => {},
foo6637: () => {},
foo6638: () => {},
foo6639: () => {},
foo6640: () => {},
foo6641: () => {},
foo6642: () => {},
foo6643: () => {},
foo6644: () => {},
foo6645: () => {},
foo6646: () => {},
foo6647: () => {},
foo6648: () => {},
foo6649: () => {},
foo6650: () => {},
foo6651: () => {},
foo6652: () => {},
foo6653: () => {},
foo6654: () => {},
foo6655: () => {},
foo6656: () => {},
foo6657: () => {},
foo6658: () => {},
foo6659: () => {},
foo6660: () => {},
foo6661: () => {},
foo6662: () => {},
foo6663: () => {},
foo6664: () => {},
foo6665: () => {},
foo6666: () => {},
foo6667: () => {},
foo6668: () => {},
foo6669: () => {},
foo6670: () => {},
foo6671: () => {},
foo6672: () => {},
foo6673: () => {},
foo6674: () => {},
foo6675: () => {},
foo6676: () => {},
foo6677: () => {},
foo6678: () => {},
foo6679: () => {},
foo6680: () => {},
foo6681: () => {},
foo6682: () => {},
foo6683: () => {},
foo6684: () => {},
foo6685: () => {},
foo6686: () => {},
foo6687: () => {},
foo6688: () => {},
foo6689: () => {},
foo6690: () => {},
foo6691: () => {},
foo6692: () => {},
foo6693: () => {},
foo6694: () => {},
foo6695: () => {},
foo6696: () => {},
foo6697: () => {},
foo6698: () => {},
foo6699: () => {},
foo6700: () => {},
foo6701: () => {},
foo6702: () => {},
foo6703: () => {},
foo6704: () => {},
foo6705: () => {},
foo6706: () => {},
foo6707: () => {},
foo6708: () => {},
foo6709: () => {},
foo6710: () => {},
foo6711: () => {},
foo6712: () => {},
foo6713: () => {},
foo6714: () => {},
foo6715: () => {},
foo6716: () => {},
foo6717: () => {},
foo6718: () => {},
foo6719: () => {},
foo6720: () => {},
foo6721: () => {},
foo6722: () => {},
foo6723: () => {},
foo6724: () => {},
foo6725: () => {},
foo6726: () => {},
foo6727: () => {},
foo6728: () => {},
foo6729: () => {},
foo6730: () => {},
foo6731: () => {},
foo6732: () => {},
foo6733: () => {},
foo6734: () => {},
foo6735: () => {},
foo6736: () => {},
foo6737: () => {},
foo6738: () => {},
foo6739: () => {},
foo6740: () => {},
foo6741: () => {},
foo6742: () => {},
foo6743: () => {},
foo6744: () => {},
foo6745: () => {},
foo6746: () => {},
foo6747: () => {},
foo6748: () => {},
foo6749: () => {},
foo6750: () => {},
foo6751: () => {},
foo6752: () => {},
foo6753: () => {},
foo6754: () => {},
foo6755: () => {},
foo6756: () => {},
foo6757: () => {},
foo6758: () => {},
foo6759: () => {},
foo6760: () => {},
foo6761: () => {},
foo6762: () => {},
foo6763: () => {},
foo6764: () => {},
foo6765: () => {},
foo6766: () => {},
foo6767: () => {},
foo6768: () => {},
foo6769: () => {},
foo6770: () => {},
foo6771: () => {},
foo6772: () => {},
foo6773: () => {},
foo6774: () => {},
foo6775: () => {},
foo6776: () => {},
foo6777: () => {},
foo6778: () => {},
foo6779: () => {},
foo6780: () => {},
foo6781: () => {},
foo6782: () => {},
foo6783: () => {},
foo6784: () => {},
foo6785: () => {},
foo6786: () => {},
foo6787: () => {},
foo6788: () => {},
foo6789: () => {},
foo6790: () => {},
foo6791: () => {},
foo6792: () => {},
foo6793: () => {},
foo6794: () => {},
foo6795: () => {},
foo6796: () => {},
foo6797: () => {},
foo6798: () => {},
foo6799: () => {},
foo6800: () => {},
foo6801: () => {},
foo6802: () => {},
foo6803: () => {},
foo6804: () => {},
foo6805: () => {},
foo6806: () => {},
foo6807: () => {},
foo6808: () => {},
foo6809: () => {},
foo6810: () => {},
foo6811: () => {},
foo6812: () => {},
foo6813: () => {},
foo6814: () => {},
foo6815: () => {},
foo6816: () => {},
foo6817: () => {},
foo6818: () => {},
foo6819: () => {},
foo6820: () => {},
foo6821: () => {},
foo6822: () => {},
foo6823: () => {},
foo6824: () => {},
foo6825: () => {},
foo6826: () => {},
foo6827: () => {},
foo6828: () => {},
foo6829: () => {},
foo6830: () => {},
foo6831: () => {},
foo6832: () => {},
foo6833: () => {},
foo6834: () => {},
foo6835: () => {},
foo6836: () => {},
foo6837: () => {},
foo6838: () => {},
foo6839: () => {},
foo6840: () => {},
foo6841: () => {},
foo6842: () => {},
foo6843: () => {},
foo6844: () => {},
foo6845: () => {},
foo6846: () => {},
foo6847: () => {},
foo6848: () => {},
foo6849: () => {},
foo6850: () => {},
foo6851: () => {},
foo6852: () => {},
foo6853: () => {},
foo6854: () => {},
foo6855: () => {},
foo6856: () => {},
foo6857: () => {},
foo6858: () => {},
foo6859: () => {},
foo6860: () => {},
foo6861: () => {},
foo6862: () => {},
foo6863: () => {},
foo6864: () => {},
foo6865: () => {},
foo6866: () => {},
foo6867: () => {},
foo6868: () => {},
foo6869: () => {},
foo6870: () => {},
foo6871: () => {},
foo6872: () => {},
foo6873: () => {},
foo6874: () => {},
foo6875: () => {},
foo6876: () => {},
foo6877: () => {},
foo6878: () => {},
foo6879: () => {},
foo6880: () => {},
foo6881: () => {},
foo6882: () => {},
foo6883: () => {},
foo6884: () => {},
foo6885: () => {},
foo6886: () => {},
foo6887: () => {},
foo6888: () => {},
foo6889: () => {},
foo6890: () => {},
foo6891: () => {},
foo6892: () => {},
foo6893: () => {},
foo6894: () => {},
foo6895: () => {},
foo6896: () => {},
foo6897: () => {},
foo6898: () => {},
foo6899: () => {},
foo6900: () => {},
foo6901: () => {},
foo6902: () => {},
foo6903: () => {},
foo6904: () => {},
foo6905: () => {},
foo6906: () => {},
foo6907: () => {},
foo6908: () => {},
foo6909: () => {},
foo6910: () => {},
foo6911: () => {},
foo6912: () => {},
foo6913: () => {},
foo6914: () => {},
foo6915: () => {},
foo6916: () => {},
foo6917: () => {},
foo6918: () => {},
foo6919: () => {},
foo6920: () => {},
foo6921: () => {},
foo6922: () => {},
foo6923: () => {},
foo6924: () => {},
foo6925: () => {},
foo6926: () => {},
foo6927: () => {},
foo6928: () => {},
foo6929: () => {},
foo6930: () => {},
foo6931: () => {},
foo6932: () => {},
foo6933: () => {},
foo6934: () => {},
foo6935: () => {},
foo6936: () => {},
foo6937: () => {},
foo6938: () => {},
foo6939: () => {},
foo6940: () => {},
foo6941: () => {},
foo6942: () => {},
foo6943: () => {},
foo6944: () => {},
foo6945: () => {},
foo6946: () => {},
foo6947: () => {},
foo6948: () => {},
foo6949: () => {},
foo6950: () => {},
foo6951: () => {},
foo6952: () => {},
foo6953: () => {},
foo6954: () => {},
foo6955: () => {},
foo6956: () => {},
foo6957: () => {},
foo6958: () => {},
foo6959: () => {},
foo6960: () => {},
foo6961: () => {},
foo6962: () => {},
foo6963: () => {},
foo6964: () => {},
foo6965: () => {},
foo6966: () => {},
foo6967: () => {},
foo6968: () => {},
foo6969: () => {},
foo6970: () => {},
foo6971: () => {},
foo6972: () => {},
foo6973: () => {},
foo6974: () => {},
foo6975: () => {},
foo6976: () => {},
foo6977: () => {},
foo6978: () => {},
foo6979: () => {},
foo6980: () => {},
foo6981: () => {},
foo6982: () => {},
foo6983: () => {},
foo6984: () => {},
foo6985: () => {},
foo6986: () => {},
foo6987: () => {},
foo6988: () => {},
foo6989: () => {},
foo6990: () => {},
foo6991: () => {},
foo6992: () => {},
foo6993: () => {},
foo6994: () => {},
foo6995: () => {},
foo6996: () => {},
foo6997: () => {},
foo6998: () => {},
foo6999: () => {},
foo7000: () => {},
foo7001: () => {},
foo7002: () => {},
foo7003: () => {},
foo7004: () => {},
foo7005: () => {},
foo7006: () => {},
foo7007: () => {},
foo7008: () => {},
foo7009: () => {},
foo7010: () => {},
foo7011: () => {},
foo7012: () => {},
foo7013: () => {},
foo7014: () => {},
foo7015: () => {},
foo7016: () => {},
foo7017: () => {},
foo7018: () => {},
foo7019: () => {},
foo7020: () => {},
foo7021: () => {},
foo7022: () => {},
foo7023: () => {},
foo7024: () => {},
foo7025: () => {},
foo7026: () => {},
foo7027: () => {},
foo7028: () => {},
foo7029: () => {},
foo7030: () => {},
foo7031: () => {},
foo7032: () => {},
foo7033: () => {},
foo7034: () => {},
foo7035: () => {},
foo7036: () => {},
foo7037: () => {},
foo7038: () => {},
foo7039: () => {},
foo7040: () => {},
foo7041: () => {},
foo7042: () => {},
foo7043: () => {},
foo7044: () => {},
foo7045: () => {},
foo7046: () => {},
foo7047: () => {},
foo7048: () => {},
foo7049: () => {},
foo7050: () => {},
foo7051: () => {},
foo7052: () => {},
foo7053: () => {},
foo7054: () => {},
foo7055: () => {},
foo7056: () => {},
foo7057: () => {},
foo7058: () => {},
foo7059: () => {},
foo7060: () => {},
foo7061: () => {},
foo7062: () => {},
foo7063: () => {},
foo7064: () => {},
foo7065: () => {},
foo7066: () => {},
foo7067: () => {},
foo7068: () => {},
foo7069: () => {},
foo7070: () => {},
foo7071: () => {},
foo7072: () => {},
foo7073: () => {},
foo7074: () => {},
foo7075: () => {},
foo7076: () => {},
foo7077: () => {},
foo7078: () => {},
foo7079: () => {},
foo7080: () => {},
foo7081: () => {},
foo7082: () => {},
foo7083: () => {},
foo7084: () => {},
foo7085: () => {},
foo7086: () => {},
foo7087: () => {},
foo7088: () => {},
foo7089: () => {},
foo7090: () => {},
foo7091: () => {},
foo7092: () => {},
foo7093: () => {},
foo7094: () => {},
foo7095: () => {},
foo7096: () => {},
foo7097: () => {},
foo7098: () => {},
foo7099: () => {},
foo7100: () => {},
foo7101: () => {},
foo7102: () => {},
foo7103: () => {},
foo7104: () => {},
foo7105: () => {},
foo7106: () => {},
foo7107: () => {},
foo7108: () => {},
foo7109: () => {},
foo7110: () => {},
foo7111: () => {},
foo7112: () => {},
foo7113: () => {},
foo7114: () => {},
foo7115: () => {},
foo7116: () => {},
foo7117: () => {},
foo7118: () => {},
foo7119: () => {},
foo7120: () => {},
foo7121: () => {},
foo7122: () => {},
foo7123: () => {},
foo7124: () => {},
foo7125: () => {},
foo7126: () => {},
foo7127: () => {},
foo7128: () => {},
foo7129: () => {},
foo7130: () => {},
foo7131: () => {},
foo7132: () => {},
foo7133: () => {},
foo7134: () => {},
foo7135: () => {},
foo7136: () => {},
foo7137: () => {},
foo7138: () => {},
foo7139: () => {},
foo7140: () => {},
foo7141: () => {},
foo7142: () => {},
foo7143: () => {},
foo7144: () => {},
foo7145: () => {},
foo7146: () => {},
foo7147: () => {},
foo7148: () => {},
foo7149: () => {},
foo7150: () => {},
foo7151: () => {},
foo7152: () => {},
foo7153: () => {},
foo7154: () => {},
foo7155: () => {},
foo7156: () => {},
foo7157: () => {},
foo7158: () => {},
foo7159: () => {},
foo7160: () => {},
foo7161: () => {},
foo7162: () => {},
foo7163: () => {},
foo7164: () => {},
foo7165: () => {},
foo7166: () => {},
foo7167: () => {},
foo7168: () => {},
foo7169: () => {},
foo7170: () => {},
foo7171: () => {},
foo7172: () => {},
foo7173: () => {},
foo7174: () => {},
foo7175: () => {},
foo7176: () => {},
foo7177: () => {},
foo7178: () => {},
foo7179: () => {},
foo7180: () => {},
foo7181: () => {},
foo7182: () => {},
foo7183: () => {},
foo7184: () => {},
foo7185: () => {},
foo7186: () => {},
foo7187: () => {},
foo7188: () => {},
foo7189: () => {},
foo7190: () => {},
foo7191: () => {},
foo7192: () => {},
foo7193: () => {},
foo7194: () => {},
foo7195: () => {},
foo7196: () => {},
foo7197: () => {},
foo7198: () => {},
foo7199: () => {},
foo7200: () => {},
foo7201: () => {},
foo7202: () => {},
foo7203: () => {},
foo7204: () => {},
foo7205: () => {},
foo7206: () => {},
foo7207: () => {},
foo7208: () => {},
foo7209: () => {},
foo7210: () => {},
foo7211: () => {},
foo7212: () => {},
foo7213: () => {},
foo7214: () => {},
foo7215: () => {},
foo7216: () => {},
foo7217: () => {},
foo7218: () => {},
foo7219: () => {},
foo7220: () => {},
foo7221: () => {},
foo7222: () => {},
foo7223: () => {},
foo7224: () => {},
foo7225: () => {},
foo7226: () => {},
foo7227: () => {},
foo7228: () => {},
foo7229: () => {},
foo7230: () => {},
foo7231: () => {},
foo7232: () => {},
foo7233: () => {},
foo7234: () => {},
foo7235: () => {},
foo7236: () => {},
foo7237: () => {},
foo7238: () => {},
foo7239: () => {},
foo7240: () => {},
foo7241: () => {},
foo7242: () => {},
foo7243: () => {},
foo7244: () => {},
foo7245: () => {},
foo7246: () => {},
foo7247: () => {},
foo7248: () => {},
foo7249: () => {},
foo7250: () => {},
foo7251: () => {},
foo7252: () => {},
foo7253: () => {},
foo7254: () => {},
foo7255: () => {},
foo7256: () => {},
foo7257: () => {},
foo7258: () => {},
foo7259: () => {},
foo7260: () => {},
foo7261: () => {},
foo7262: () => {},
foo7263: () => {},
foo7264: () => {},
foo7265: () => {},
foo7266: () => {},
foo7267: () => {},
foo7268: () => {},
foo7269: () => {},
foo7270: () => {},
foo7271: () => {},
foo7272: () => {},
foo7273: () => {},
foo7274: () => {},
foo7275: () => {},
foo7276: () => {},
foo7277: () => {},
foo7278: () => {},
foo7279: () => {},
foo7280: () => {},
foo7281: () => {},
foo7282: () => {},
foo7283: () => {},
foo7284: () => {},
foo7285: () => {},
foo7286: () => {},
foo7287: () => {},
foo7288: () => {},
foo7289: () => {},
foo7290: () => {},
foo7291: () => {},
foo7292: () => {},
foo7293: () => {},
foo7294: () => {},
foo7295: () => {},
foo7296: () => {},
foo7297: () => {},
foo7298: () => {},
foo7299: () => {},
foo7300: () => {},
foo7301: () => {},
foo7302: () => {},
foo7303: () => {},
foo7304: () => {},
foo7305: () => {},
foo7306: () => {},
foo7307: () => {},
foo7308: () => {},
foo7309: () => {},
foo7310: () => {},
foo7311: () => {},
foo7312: () => {},
foo7313: () => {},
foo7314: () => {},
foo7315: () => {},
foo7316: () => {},
foo7317: () => {},
foo7318: () => {},
foo7319: () => {},
foo7320: () => {},
foo7321: () => {},
foo7322: () => {},
foo7323: () => {},
foo7324: () => {},
foo7325: () => {},
foo7326: () => {},
foo7327: () => {},
foo7328: () => {},
foo7329: () => {},
foo7330: () => {},
foo7331: () => {},
foo7332: () => {},
foo7333: () => {},
foo7334: () => {},
foo7335: () => {},
foo7336: () => {},
foo7337: () => {},
foo7338: () => {},
foo7339: () => {},
foo7340: () => {},
foo7341: () => {},
foo7342: () => {},
foo7343: () => {},
foo7344: () => {},
foo7345: () => {},
foo7346: () => {},
foo7347: () => {},
foo7348: () => {},
foo7349: () => {},
foo7350: () => {},
foo7351: () => {},
foo7352: () => {},
foo7353: () => {},
foo7354: () => {},
foo7355: () => {},
foo7356: () => {},
foo7357: () => {},
foo7358: () => {},
foo7359: () => {},
foo7360: () => {},
foo7361: () => {},
foo7362: () => {},
foo7363: () => {},
foo7364: () => {},
foo7365: () => {},
foo7366: () => {},
foo7367: () => {},
foo7368: () => {},
foo7369: () => {},
foo7370: () => {},
foo7371: () => {},
foo7372: () => {},
foo7373: () => {},
foo7374: () => {},
foo7375: () => {},
foo7376: () => {},
foo7377: () => {},
foo7378: () => {},
foo7379: () => {},
foo7380: () => {},
foo7381: () => {},
foo7382: () => {},
foo7383: () => {},
foo7384: () => {},
foo7385: () => {},
foo7386: () => {},
foo7387: () => {},
foo7388: () => {},
foo7389: () => {},
foo7390: () => {},
foo7391: () => {},
foo7392: () => {},
foo7393: () => {},
foo7394: () => {},
foo7395: () => {},
foo7396: () => {},
foo7397: () => {},
foo7398: () => {},
foo7399: () => {},
foo7400: () => {},
foo7401: () => {},
foo7402: () => {},
foo7403: () => {},
foo7404: () => {},
foo7405: () => {},
foo7406: () => {},
foo7407: () => {},
foo7408: () => {},
foo7409: () => {},
foo7410: () => {},
foo7411: () => {},
foo7412: () => {},
foo7413: () => {},
foo7414: () => {},
foo7415: () => {},
foo7416: () => {},
foo7417: () => {},
foo7418: () => {},
foo7419: () => {},
foo7420: () => {},
foo7421: () => {},
foo7422: () => {},
foo7423: () => {},
foo7424: () => {},
foo7425: () => {},
foo7426: () => {},
foo7427: () => {},
foo7428: () => {},
foo7429: () => {},
foo7430: () => {},
foo7431: () => {},
foo7432: () => {},
foo7433: () => {},
foo7434: () => {},
foo7435: () => {},
foo7436: () => {},
foo7437: () => {},
foo7438: () => {},
foo7439: () => {},
foo7440: () => {},
foo7441: () => {},
foo7442: () => {},
foo7443: () => {},
foo7444: () => {},
foo7445: () => {},
foo7446: () => {},
foo7447: () => {},
foo7448: () => {},
foo7449: () => {},
foo7450: () => {},
foo7451: () => {},
foo7452: () => {},
foo7453: () => {},
foo7454: () => {},
foo7455: () => {},
foo7456: () => {},
foo7457: () => {},
foo7458: () => {},
foo7459: () => {},
foo7460: () => {},
foo7461: () => {},
foo7462: () => {},
foo7463: () => {},
foo7464: () => {},
foo7465: () => {},
foo7466: () => {},
foo7467: () => {},
foo7468: () => {},
foo7469: () => {},
foo7470: () => {},
foo7471: () => {},
foo7472: () => {},
foo7473: () => {},
foo7474: () => {},
foo7475: () => {},
foo7476: () => {},
foo7477: () => {},
foo7478: () => {},
foo7479: () => {},
foo7480: () => {},
foo7481: () => {},
foo7482: () => {},
foo7483: () => {},
foo7484: () => {},
foo7485: () => {},
foo7486: () => {},
foo7487: () => {},
foo7488: () => {},
foo7489: () => {},
foo7490: () => {},
foo7491: () => {},
foo7492: () => {},
foo7493: () => {},
foo7494: () => {},
foo7495: () => {},
foo7496: () => {},
foo7497: () => {},
foo7498: () => {},
foo7499: () => {},
foo7500: () => {},
foo7501: () => {},
foo7502: () => {},
foo7503: () => {},
foo7504: () => {},
foo7505: () => {},
foo7506: () => {},
foo7507: () => {},
foo7508: () => {},
foo7509: () => {},
foo7510: () => {},
foo7511: () => {},
foo7512: () => {},
foo7513: () => {},
foo7514: () => {},
foo7515: () => {},
foo7516: () => {},
foo7517: () => {},
foo7518: () => {},
foo7519: () => {},
foo7520: () => {},
foo7521: () => {},
foo7522: () => {},
foo7523: () => {},
foo7524: () => {},
foo7525: () => {},
foo7526: () => {},
foo7527: () => {},
foo7528: () => {},
foo7529: () => {},
foo7530: () => {},
foo7531: () => {},
foo7532: () => {},
foo7533: () => {},
foo7534: () => {},
foo7535: () => {},
foo7536: () => {},
foo7537: () => {},
foo7538: () => {},
foo7539: () => {},
foo7540: () => {},
foo7541: () => {},
foo7542: () => {},
foo7543: () => {},
foo7544: () => {},
foo7545: () => {},
foo7546: () => {},
foo7547: () => {},
foo7548: () => {},
foo7549: () => {},
foo7550: () => {},
foo7551: () => {},
foo7552: () => {},
foo7553: () => {},
foo7554: () => {},
foo7555: () => {},
foo7556: () => {},
foo7557: () => {},
foo7558: () => {},
foo7559: () => {},
foo7560: () => {},
foo7561: () => {},
foo7562: () => {},
foo7563: () => {},
foo7564: () => {},
foo7565: () => {},
foo7566: () => {},
foo7567: () => {},
foo7568: () => {},
foo7569: () => {},
foo7570: () => {},
foo7571: () => {},
foo7572: () => {},
foo7573: () => {},
foo7574: () => {},
foo7575: () => {},
foo7576: () => {},
foo7577: () => {},
foo7578: () => {},
foo7579: () => {},
foo7580: () => {},
foo7581: () => {},
foo7582: () => {},
foo7583: () => {},
foo7584: () => {},
foo7585: () => {},
foo7586: () => {},
foo7587: () => {},
foo7588: () => {},
foo7589: () => {},
foo7590: () => {},
foo7591: () => {},
foo7592: () => {},
foo7593: () => {},
foo7594: () => {},
foo7595: () => {},
foo7596: () => {},
foo7597: () => {},
foo7598: () => {},
foo7599: () => {},
foo7600: () => {},
foo7601: () => {},
foo7602: () => {},
foo7603: () => {},
foo7604: () => {},
foo7605: () => {},
foo7606: () => {},
foo7607: () => {},
foo7608: () => {},
foo7609: () => {},
foo7610: () => {},
foo7611: () => {},
foo7612: () => {},
foo7613: () => {},
foo7614: () => {},
foo7615: () => {},
foo7616: () => {},
foo7617: () => {},
foo7618: () => {},
foo7619: () => {},
foo7620: () => {},
foo7621: () => {},
foo7622: () => {},
foo7623: () => {},
foo7624: () => {},
foo7625: () => {},
foo7626: () => {},
foo7627: () => {},
foo7628: () => {},
foo7629: () => {},
foo7630: () => {},
foo7631: () => {},
foo7632: () => {},
foo7633: () => {},
foo7634: () => {},
foo7635: () => {},
foo7636: () => {},
foo7637: () => {},
foo7638: () => {},
foo7639: () => {},
foo7640: () => {},
foo7641: () => {},
foo7642: () => {},
foo7643: () => {},
foo7644: () => {},
foo7645: () => {},
foo7646: () => {},
foo7647: () => {},
foo7648: () => {},
foo7649: () => {},
foo7650: () => {},
foo7651: () => {},
foo7652: () => {},
foo7653: () => {},
foo7654: () => {},
foo7655: () => {},
foo7656: () => {},
foo7657: () => {},
foo7658: () => {},
foo7659: () => {},
foo7660: () => {},
foo7661: () => {},
foo7662: () => {},
foo7663: () => {},
foo7664: () => {},
foo7665: () => {},
foo7666: () => {},
foo7667: () => {},
foo7668: () => {},
foo7669: () => {},
foo7670: () => {},
foo7671: () => {},
foo7672: () => {},
foo7673: () => {},
foo7674: () => {},
foo7675: () => {},
foo7676: () => {},
foo7677: () => {},
foo7678: () => {},
foo7679: () => {},
foo7680: () => {},
foo7681: () => {},
foo7682: () => {},
foo7683: () => {},
foo7684: () => {},
foo7685: () => {},
foo7686: () => {},
foo7687: () => {},
foo7688: () => {},
foo7689: () => {},
foo7690: () => {},
foo7691: () => {},
foo7692: () => {},
foo7693: () => {},
foo7694: () => {},
foo7695: () => {},
foo7696: () => {},
foo7697: () => {},
foo7698: () => {},
foo7699: () => {},
foo7700: () => {},
foo7701: () => {},
foo7702: () => {},
foo7703: () => {},
foo7704: () => {},
foo7705: () => {},
foo7706: () => {},
foo7707: () => {},
foo7708: () => {},
foo7709: () => {},
foo7710: () => {},
foo7711: () => {},
foo7712: () => {},
foo7713: () => {},
foo7714: () => {},
foo7715: () => {},
foo7716: () => {},
foo7717: () => {},
foo7718: () => {},
foo7719: () => {},
foo7720: () => {},
foo7721: () => {},
foo7722: () => {},
foo7723: () => {},
foo7724: () => {},
foo7725: () => {},
foo7726: () => {},
foo7727: () => {},
foo7728: () => {},
foo7729: () => {},
foo7730: () => {},
foo7731: () => {},
foo7732: () => {},
foo7733: () => {},
foo7734: () => {},
foo7735: () => {},
foo7736: () => {},
foo7737: () => {},
foo7738: () => {},
foo7739: () => {},
foo7740: () => {},
foo7741: () => {},
foo7742: () => {},
foo7743: () => {},
foo7744: () => {},
foo7745: () => {},
foo7746: () => {},
foo7747: () => {},
foo7748: () => {},
foo7749: () => {},
foo7750: () => {},
foo7751: () => {},
foo7752: () => {},
foo7753: () => {},
foo7754: () => {},
foo7755: () => {},
foo7756: () => {},
foo7757: () => {},
foo7758: () => {},
foo7759: () => {},
foo7760: () => {},
foo7761: () => {},
foo7762: () => {},
foo7763: () => {},
foo7764: () => {},
foo7765: () => {},
foo7766: () => {},
foo7767: () => {},
foo7768: () => {},
foo7769: () => {},
foo7770: () => {},
foo7771: () => {},
foo7772: () => {},
foo7773: () => {},
foo7774: () => {},
foo7775: () => {},
foo7776: () => {},
foo7777: () => {},
foo7778: () => {},
foo7779: () => {},
foo7780: () => {},
foo7781: () => {},
foo7782: () => {},
foo7783: () => {},
foo7784: () => {},
foo7785: () => {},
foo7786: () => {},
foo7787: () => {},
foo7788: () => {},
foo7789: () => {},
foo7790: () => {},
foo7791: () => {},
foo7792: () => {},
foo7793: () => {},
foo7794: () => {},
foo7795: () => {},
foo7796: () => {},
foo7797: () => {},
foo7798: () => {},
foo7799: () => {},
foo7800: () => {},
foo7801: () => {},
foo7802: () => {},
foo7803: () => {},
foo7804: () => {},
foo7805: () => {},
foo7806: () => {},
foo7807: () => {},
foo7808: () => {},
foo7809: () => {},
foo7810: () => {},
foo7811: () => {},
foo7812: () => {},
foo7813: () => {},
foo7814: () => {},
foo7815: () => {},
foo7816: () => {},
foo7817: () => {},
foo7818: () => {},
foo7819: () => {},
foo7820: () => {},
foo7821: () => {},
foo7822: () => {},
foo7823: () => {},
foo7824: () => {},
foo7825: () => {},
foo7826: () => {},
foo7827: () => {},
foo7828: () => {},
foo7829: () => {},
foo7830: () => {},
foo7831: () => {},
foo7832: () => {},
foo7833: () => {},
foo7834: () => {},
foo7835: () => {},
foo7836: () => {},
foo7837: () => {},
foo7838: () => {},
foo7839: () => {},
foo7840: () => {},
foo7841: () => {},
foo7842: () => {},
foo7843: () => {},
foo7844: () => {},
foo7845: () => {},
foo7846: () => {},
foo7847: () => {},
foo7848: () => {},
foo7849: () => {},
foo7850: () => {},
foo7851: () => {},
foo7852: () => {},
foo7853: () => {},
foo7854: () => {},
foo7855: () => {},
foo7856: () => {},
foo7857: () => {},
foo7858: () => {},
foo7859: () => {},
foo7860: () => {},
foo7861: () => {},
foo7862: () => {},
foo7863: () => {},
foo7864: () => {},
foo7865: () => {},
foo7866: () => {},
foo7867: () => {},
foo7868: () => {},
foo7869: () => {},
foo7870: () => {},
foo7871: () => {},
foo7872: () => {},
foo7873: () => {},
foo7874: () => {},
foo7875: () => {},
foo7876: () => {},
foo7877: () => {},
foo7878: () => {},
foo7879: () => {},
foo7880: () => {},
foo7881: () => {},
foo7882: () => {},
foo7883: () => {},
foo7884: () => {},
foo7885: () => {},
foo7886: () => {},
foo7887: () => {},
foo7888: () => {},
foo7889: () => {},
foo7890: () => {},
foo7891: () => {},
foo7892: () => {},
foo7893: () => {},
foo7894: () => {},
foo7895: () => {},
foo7896: () => {},
foo7897: () => {},
foo7898: () => {},
foo7899: () => {},
foo7900: () => {},
foo7901: () => {},
foo7902: () => {},
foo7903: () => {},
foo7904: () => {},
foo7905: () => {},
foo7906: () => {},
foo7907: () => {},
foo7908: () => {},
foo7909: () => {},
foo7910: () => {},
foo7911: () => {},
foo7912: () => {},
foo7913: () => {},
foo7914: () => {},
foo7915: () => {},
foo7916: () => {},
foo7917: () => {},
foo7918: () => {},
foo7919: () => {},
foo7920: () => {},
foo7921: () => {},
foo7922: () => {},
foo7923: () => {},
foo7924: () => {},
foo7925: () => {},
foo7926: () => {},
foo7927: () => {},
foo7928: () => {},
foo7929: () => {},
foo7930: () => {},
foo7931: () => {},
foo7932: () => {},
foo7933: () => {},
foo7934: () => {},
foo7935: () => {},
foo7936: () => {},
foo7937: () => {},
foo7938: () => {},
foo7939: () => {},
foo7940: () => {},
foo7941: () => {},
foo7942: () => {},
foo7943: () => {},
foo7944: () => {},
foo7945: () => {},
foo7946: () => {},
foo7947: () => {},
foo7948: () => {},
foo7949: () => {},
foo7950: () => {},
foo7951: () => {},
foo7952: () => {},
foo7953: () => {},
foo7954: () => {},
foo7955: () => {},
foo7956: () => {},
foo7957: () => {},
foo7958: () => {},
foo7959: () => {},
foo7960: () => {},
foo7961: () => {},
foo7962: () => {},
foo7963: () => {},
foo7964: () => {},
foo7965: () => {},
foo7966: () => {},
foo7967: () => {},
foo7968: () => {},
foo7969: () => {},
foo7970: () => {},
foo7971: () => {},
foo7972: () => {},
foo7973: () => {},
foo7974: () => {},
foo7975: () => {},
foo7976: () => {},
foo7977: () => {},
foo7978: () => {},
foo7979: () => {},
foo7980: () => {},
foo7981: () => {},
foo7982: () => {},
foo7983: () => {},
foo7984: () => {},
foo7985: () => {},
foo7986: () => {},
foo7987: () => {},
foo7988: () => {},
foo7989: () => {},
foo7990: () => {},
foo7991: () => {},
foo7992: () => {},
foo7993: () => {},
foo7994: () => {},
foo7995: () => {},
foo7996: () => {},
foo7997: () => {},
foo7998: () => {},
foo7999: () => {},
foo8000: () => {},
foo8001: () => {},
foo8002: () => {},
foo8003: () => {},
foo8004: () => {},
foo8005: () => {},
foo8006: () => {},
foo8007: () => {},
foo8008: () => {},
foo8009: () => {},
foo8010: () => {},
foo8011: () => {},
foo8012: () => {},
foo8013: () => {},
foo8014: () => {},
foo8015: () => {},
foo8016: () => {},
foo8017: () => {},
foo8018: () => {},
foo8019: () => {},
foo8020: () => {},
foo8021: () => {},
foo8022: () => {},
foo8023: () => {},
foo8024: () => {},
foo8025: () => {},
foo8026: () => {},
foo8027: () => {},
foo8028: () => {},
foo8029: () => {},
foo8030: () => {},
foo8031: () => {},
foo8032: () => {},
foo8033: () => {},
foo8034: () => {},
foo8035: () => {},
foo8036: () => {},
foo8037: () => {},
foo8038: () => {},
foo8039: () => {},
foo8040: () => {},
foo8041: () => {},
foo8042: () => {},
foo8043: () => {},
foo8044: () => {},
foo8045: () => {},
foo8046: () => {},
foo8047: () => {},
foo8048: () => {},
foo8049: () => {},
foo8050: () => {},
foo8051: () => {},
foo8052: () => {},
foo8053: () => {},
foo8054: () => {},
foo8055: () => {},
foo8056: () => {},
foo8057: () => {},
foo8058: () => {},
foo8059: () => {},
foo8060: () => {},
foo8061: () => {},
foo8062: () => {},
foo8063: () => {},
foo8064: () => {},
foo8065: () => {},
foo8066: () => {},
foo8067: () => {},
foo8068: () => {},
foo8069: () => {},
foo8070: () => {},
foo8071: () => {},
foo8072: () => {},
foo8073: () => {},
foo8074: () => {},
foo8075: () => {},
foo8076: () => {},
foo8077: () => {},
foo8078: () => {},
foo8079: () => {},
foo8080: () => {},
foo8081: () => {},
foo8082: () => {},
foo8083: () => {},
foo8084: () => {},
foo8085: () => {},
foo8086: () => {},
foo8087: () => {},
foo8088: () => {},
foo8089: () => {},
foo8090: () => {},
foo8091: () => {},
foo8092: () => {},
foo8093: () => {},
foo8094: () => {},
foo8095: () => {},
foo8096: () => {},
foo8097: () => {},
foo8098: () => {},
foo8099: () => {},
foo8100: () => {},
foo8101: () => {},
foo8102: () => {},
foo8103: () => {},
foo8104: () => {},
foo8105: () => {},
foo8106: () => {},
foo8107: () => {},
foo8108: () => {},
foo8109: () => {},
foo8110: () => {},
foo8111: () => {},
foo8112: () => {},
foo8113: () => {},
foo8114: () => {},
foo8115: () => {},
foo8116: () => {},
foo8117: () => {},
foo8118: () => {},
foo8119: () => {},
foo8120: () => {},
foo8121: () => {},
foo8122: () => {},
foo8123: () => {},
foo8124: () => {},
foo8125: () => {},
foo8126: () => {},
foo8127: () => {},
foo8128: () => {},
foo8129: () => {},
foo8130: () => {},
foo8131: () => {},
foo8132: () => {},
foo8133: () => {},
foo8134: () => {},
foo8135: () => {},
foo8136: () => {},
foo8137: () => {},
foo8138: () => {},
foo8139: () => {},
foo8140: () => {},
foo8141: () => {},
foo8142: () => {},
foo8143: () => {},
foo8144: () => {},
foo8145: () => {},
foo8146: () => {},
foo8147: () => {},
foo8148: () => {},
foo8149: () => {},
foo8150: () => {},
foo8151: () => {},
foo8152: () => {},
foo8153: () => {},
foo8154: () => {},
foo8155: () => {},
foo8156: () => {},
foo8157: () => {},
foo8158: () => {},
foo8159: () => {},
foo8160: () => {},
foo8161: () => {},
foo8162: () => {},
foo8163: () => {},
foo8164: () => {},
foo8165: () => {},
foo8166: () => {},
foo8167: () => {},
foo8168: () => {},
foo8169: () => {},
foo8170: () => {},
foo8171: () => {},
foo8172: () => {},
foo8173: () => {},
foo8174: () => {},
foo8175: () => {},
foo8176: () => {},
foo8177: () => {},
foo8178: () => {},
foo8179: () => {},
foo8180: () => {},
foo8181: () => {},
foo8182: () => {},
foo8183: () => {},
foo8184: () => {},
foo8185: () => {},
foo8186: () => {},
foo8187: () => {},
foo8188: () => {},
foo8189: () => {},
foo8190: () => {},
foo8191: () => {},
foo8192: () => {},
foo8193: () => {},
foo8194: () => {},
foo8195: () => {},
foo8196: () => {},
foo8197: () => {},
foo8198: () => {},
foo8199: () => {},
foo8200: () => {},
foo8201: () => {},
foo8202: () => {},
foo8203: () => {},
foo8204: () => {},
foo8205: () => {},
foo8206: () => {},
foo8207: () => {},
foo8208: () => {},
foo8209: () => {},
foo8210: () => {},
foo8211: () => {},
foo8212: () => {},
foo8213: () => {},
foo8214: () => {},
foo8215: () => {},
foo8216: () => {},
foo8217: () => {},
foo8218: () => {},
foo8219: () => {},
foo8220: () => {},
foo8221: () => {},
foo8222: () => {},
foo8223: () => {},
foo8224: () => {},
foo8225: () => {},
foo8226: () => {},
foo8227: () => {},
foo8228: () => {},
foo8229: () => {},
foo8230: () => {},
foo8231: () => {},
foo8232: () => {},
foo8233: () => {},
foo8234: () => {},
foo8235: () => {},
foo8236: () => {},
foo8237: () => {},
foo8238: () => {},
foo8239: () => {},
foo8240: () => {},
foo8241: () => {},
foo8242: () => {},
foo8243: () => {},
foo8244: () => {},
foo8245: () => {},
foo8246: () => {},
foo8247: () => {},
foo8248: () => {},
foo8249: () => {},
foo8250: () => {},
foo8251: () => {},
foo8252: () => {},
foo8253: () => {},
foo8254: () => {},
foo8255: () => {},
foo8256: () => {},
foo8257: () => {},
foo8258: () => {},
foo8259: () => {},
foo8260: () => {},
foo8261: () => {},
foo8262: () => {},
foo8263: () => {},
foo8264: () => {},
foo8265: () => {},
foo8266: () => {},
foo8267: () => {},
foo8268: () => {},
foo8269: () => {},
foo8270: () => {},
foo8271: () => {},
foo8272: () => {},
foo8273: () => {},
foo8274: () => {},
foo8275: () => {},
foo8276: () => {},
foo8277: () => {},
foo8278: () => {},
foo8279: () => {},
foo8280: () => {},
foo8281: () => {},
foo8282: () => {},
foo8283: () => {},
foo8284: () => {},
foo8285: () => {},
foo8286: () => {},
foo8287: () => {},
foo8288: () => {},
foo8289: () => {},
foo8290: () => {},
foo8291: () => {},
foo8292: () => {},
foo8293: () => {},
foo8294: () => {},
foo8295: () => {},
foo8296: () => {},
foo8297: () => {},
foo8298: () => {},
foo8299: () => {},
foo8300: () => {},
foo8301: () => {},
foo8302: () => {},
foo8303: () => {},
foo8304: () => {},
foo8305: () => {},
foo8306: () => {},
foo8307: () => {},
foo8308: () => {},
foo8309: () => {},
foo8310: () => {},
foo8311: () => {},
foo8312: () => {},
foo8313: () => {},
foo8314: () => {},
foo8315: () => {},
foo8316: () => {},
foo8317: () => {},
foo8318: () => {},
foo8319: () => {},
foo8320: () => {},
foo8321: () => {},
foo8322: () => {},
foo8323: () => {},
foo8324: () => {},
foo8325: () => {},
foo8326: () => {},
foo8327: () => {},
foo8328: () => {},
foo8329: () => {},
foo8330: () => {},
foo8331: () => {},
foo8332: () => {},
foo8333: () => {},
foo8334: () => {},
foo8335: () => {},
foo8336: () => {},
foo8337: () => {},
foo8338: () => {},
foo8339: () => {},
foo8340: () => {},
foo8341: () => {},
foo8342: () => {},
foo8343: () => {},
foo8344: () => {},
foo8345: () => {},
foo8346: () => {},
foo8347: () => {},
foo8348: () => {},
foo8349: () => {},
foo8350: () => {},
foo8351: () => {},
foo8352: () => {},
foo8353: () => {},
foo8354: () => {},
foo8355: () => {},
foo8356: () => {},
foo8357: () => {},
foo8358: () => {},
foo8359: () => {},
foo8360: () => {},
foo8361: () => {},
foo8362: () => {},
foo8363: () => {},
foo8364: () => {},
foo8365: () => {},
foo8366: () => {},
foo8367: () => {},
foo8368: () => {},
foo8369: () => {},
foo8370: () => {},
foo8371: () => {},
foo8372: () => {},
foo8373: () => {},
foo8374: () => {},
foo8375: () => {},
foo8376: () => {},
foo8377: () => {},
foo8378: () => {},
foo8379: () => {},
foo8380: () => {},
foo8381: () => {},
foo8382: () => {},
foo8383: () => {},
foo8384: () => {},
foo8385: () => {},
foo8386: () => {},
foo8387: () => {},
foo8388: () => {},
foo8389: () => {},
foo8390: () => {},
foo8391: () => {},
foo8392: () => {},
foo8393: () => {},
foo8394: () => {},
foo8395: () => {},
foo8396: () => {},
foo8397: () => {},
foo8398: () => {},
foo8399: () => {},
foo8400: () => {},
foo8401: () => {},
foo8402: () => {},
foo8403: () => {},
foo8404: () => {},
foo8405: () => {},
foo8406: () => {},
foo8407: () => {},
foo8408: () => {},
foo8409: () => {},
foo8410: () => {},
foo8411: () => {},
foo8412: () => {},
foo8413: () => {},
foo8414: () => {},
foo8415: () => {},
foo8416: () => {},
foo8417: () => {},
foo8418: () => {},
foo8419: () => {},
foo8420: () => {},
foo8421: () => {},
foo8422: () => {},
foo8423: () => {},
foo8424: () => {},
foo8425: () => {},
foo8426: () => {},
foo8427: () => {},
foo8428: () => {},
foo8429: () => {},
foo8430: () => {},
foo8431: () => {},
foo8432: () => {},
foo8433: () => {},
foo8434: () => {},
foo8435: () => {},
foo8436: () => {},
foo8437: () => {},
foo8438: () => {},
foo8439: () => {},
foo8440: () => {},
foo8441: () => {},
foo8442: () => {},
foo8443: () => {},
foo8444: () => {},
foo8445: () => {},
foo8446: () => {},
foo8447: () => {},
foo8448: () => {},
foo8449: () => {},
foo8450: () => {},
foo8451: () => {},
foo8452: () => {},
foo8453: () => {},
foo8454: () => {},
foo8455: () => {},
foo8456: () => {},
foo8457: () => {},
foo8458: () => {},
foo8459: () => {},
foo8460: () => {},
foo8461: () => {},
foo8462: () => {},
foo8463: () => {},
foo8464: () => {},
foo8465: () => {},
foo8466: () => {},
foo8467: () => {},
foo8468: () => {},
foo8469: () => {},
foo8470: () => {},
foo8471: () => {},
foo8472: () => {},
foo8473: () => {},
foo8474: () => {},
foo8475: () => {},
foo8476: () => {},
foo8477: () => {},
foo8478: () => {},
foo8479: () => {},
foo8480: () => {},
foo8481: () => {},
foo8482: () => {},
foo8483: () => {},
foo8484: () => {},
foo8485: () => {},
foo8486: () => {},
foo8487: () => {},
foo8488: () => {},
foo8489: () => {},
foo8490: () => {},
foo8491: () => {},
foo8492: () => {},
foo8493: () => {},
foo8494: () => {},
foo8495: () => {},
foo8496: () => {},
foo8497: () => {},
foo8498: () => {},
foo8499: () => {},
foo8500: () => {},
foo8501: () => {},
foo8502: () => {},
foo8503: () => {},
foo8504: () => {},
foo8505: () => {},
foo8506: () => {},
foo8507: () => {},
foo8508: () => {},
foo8509: () => {},
foo8510: () => {},
foo8511: () => {},
foo8512: () => {},
foo8513: () => {},
foo8514: () => {},
foo8515: () => {},
foo8516: () => {},
foo8517: () => {},
foo8518: () => {},
foo8519: () => {},
foo8520: () => {},
foo8521: () => {},
foo8522: () => {},
foo8523: () => {},
foo8524: () => {},
foo8525: () => {},
foo8526: () => {},
foo8527: () => {},
foo8528: () => {},
foo8529: () => {},
foo8530: () => {},
foo8531: () => {},
foo8532: () => {},
foo8533: () => {},
foo8534: () => {},
foo8535: () => {},
foo8536: () => {},
foo8537: () => {},
foo8538: () => {},
foo8539: () => {},
foo8540: () => {},
foo8541: () => {},
foo8542: () => {},
foo8543: () => {},
foo8544: () => {},
foo8545: () => {},
foo8546: () => {},
foo8547: () => {},
foo8548: () => {},
foo8549: () => {},
foo8550: () => {},
foo8551: () => {},
foo8552: () => {},
foo8553: () => {},
foo8554: () => {},
foo8555: () => {},
foo8556: () => {},
foo8557: () => {},
foo8558: () => {},
foo8559: () => {},
foo8560: () => {},
foo8561: () => {},
foo8562: () => {},
foo8563: () => {},
foo8564: () => {},
foo8565: () => {},
foo8566: () => {},
foo8567: () => {},
foo8568: () => {},
foo8569: () => {},
foo8570: () => {},
foo8571: () => {},
foo8572: () => {},
foo8573: () => {},
foo8574: () => {},
foo8575: () => {},
foo8576: () => {},
foo8577: () => {},
foo8578: () => {},
foo8579: () => {},
foo8580: () => {},
foo8581: () => {},
foo8582: () => {},
foo8583: () => {},
foo8584: () => {},
foo8585: () => {},
foo8586: () => {},
foo8587: () => {},
foo8588: () => {},
foo8589: () => {},
foo8590: () => {},
foo8591: () => {},
foo8592: () => {},
foo8593: () => {},
foo8594: () => {},
foo8595: () => {},
foo8596: () => {},
foo8597: () => {},
foo8598: () => {},
foo8599: () => {},
foo8600: () => {},
foo8601: () => {},
foo8602: () => {},
foo8603: () => {},
foo8604: () => {},
foo8605: () => {},
foo8606: () => {},
foo8607: () => {},
foo8608: () => {},
foo8609: () => {},
foo8610: () => {},
foo8611: () => {},
foo8612: () => {},
foo8613: () => {},
foo8614: () => {},
foo8615: () => {},
foo8616: () => {},
foo8617: () => {},
foo8618: () => {},
foo8619: () => {},
foo8620: () => {},
foo8621: () => {},
foo8622: () => {},
foo8623: () => {},
foo8624: () => {},
foo8625: () => {},
foo8626: () => {},
foo8627: () => {},
foo8628: () => {},
foo8629: () => {},
foo8630: () => {},
foo8631: () => {},
foo8632: () => {},
foo8633: () => {},
foo8634: () => {},
foo8635: () => {},
foo8636: () => {},
foo8637: () => {},
foo8638: () => {},
foo8639: () => {},
foo8640: () => {},
foo8641: () => {},
foo8642: () => {},
foo8643: () => {},
foo8644: () => {},
foo8645: () => {},
foo8646: () => {},
foo8647: () => {},
foo8648: () => {},
foo8649: () => {},
foo8650: () => {},
foo8651: () => {},
foo8652: () => {},
foo8653: () => {},
foo8654: () => {},
foo8655: () => {},
foo8656: () => {},
foo8657: () => {},
foo8658: () => {},
foo8659: () => {},
foo8660: () => {},
foo8661: () => {},
foo8662: () => {},
foo8663: () => {},
foo8664: () => {},
foo8665: () => {},
foo8666: () => {},
foo8667: () => {},
foo8668: () => {},
foo8669: () => {},
foo8670: () => {},
foo8671: () => {},
foo8672: () => {},
foo8673: () => {},
foo8674: () => {},
foo8675: () => {},
foo8676: () => {},
foo8677: () => {},
foo8678: () => {},
foo8679: () => {},
foo8680: () => {},
foo8681: () => {},
foo8682: () => {},
foo8683: () => {},
foo8684: () => {},
foo8685: () => {},
foo8686: () => {},
foo8687: () => {},
foo8688: () => {},
foo8689: () => {},
foo8690: () => {},
foo8691: () => {},
foo8692: () => {},
foo8693: () => {},
foo8694: () => {},
foo8695: () => {},
foo8696: () => {},
foo8697: () => {},
foo8698: () => {},
foo8699: () => {},
foo8700: () => {},
foo8701: () => {},
foo8702: () => {},
foo8703: () => {},
foo8704: () => {},
foo8705: () => {},
foo8706: () => {},
foo8707: () => {},
foo8708: () => {},
foo8709: () => {},
foo8710: () => {},
foo8711: () => {},
foo8712: () => {},
foo8713: () => {},
foo8714: () => {},
foo8715: () => {},
foo8716: () => {},
foo8717: () => {},
foo8718: () => {},
foo8719: () => {},
foo8720: () => {},
foo8721: () => {},
foo8722: () => {},
foo8723: () => {},
foo8724: () => {},
foo8725: () => {},
foo8726: () => {},
foo8727: () => {},
foo8728: () => {},
foo8729: () => {},
foo8730: () => {},
foo8731: () => {},
foo8732: () => {},
foo8733: () => {},
foo8734: () => {},
foo8735: () => {},
foo8736: () => {},
foo8737: () => {},
foo8738: () => {},
foo8739: () => {},
foo8740: () => {},
foo8741: () => {},
foo8742: () => {},
foo8743: () => {},
foo8744: () => {},
foo8745: () => {},
foo8746: () => {},
foo8747: () => {},
foo8748: () => {},
foo8749: () => {},
foo8750: () => {},
foo8751: () => {},
foo8752: () => {},
foo8753: () => {},
foo8754: () => {},
foo8755: () => {},
foo8756: () => {},
foo8757: () => {},
foo8758: () => {},
foo8759: () => {},
foo8760: () => {},
foo8761: () => {},
foo8762: () => {},
foo8763: () => {},
foo8764: () => {},
foo8765: () => {},
foo8766: () => {},
foo8767: () => {},
foo8768: () => {},
foo8769: () => {},
foo8770: () => {},
foo8771: () => {},
foo8772: () => {},
foo8773: () => {},
foo8774: () => {},
foo8775: () => {},
foo8776: () => {},
foo8777: () => {},
foo8778: () => {},
foo8779: () => {},
foo8780: () => {},
foo8781: () => {},
foo8782: () => {},
foo8783: () => {},
foo8784: () => {},
foo8785: () => {},
foo8786: () => {},
foo8787: () => {},
foo8788: () => {},
foo8789: () => {},
foo8790: () => {},
foo8791: () => {},
foo8792: () => {},
foo8793: () => {},
foo8794: () => {},
foo8795: () => {},
foo8796: () => {},
foo8797: () => {},
foo8798: () => {},
foo8799: () => {},
foo8800: () => {},
foo8801: () => {},
foo8802: () => {},
foo8803: () => {},
foo8804: () => {},
foo8805: () => {},
foo8806: () => {},
foo8807: () => {},
foo8808: () => {},
foo8809: () => {},
foo8810: () => {},
foo8811: () => {},
foo8812: () => {},
foo8813: () => {},
foo8814: () => {},
foo8815: () => {},
foo8816: () => {},
foo8817: () => {},
foo8818: () => {},
foo8819: () => {},
foo8820: () => {},
foo8821: () => {},
foo8822: () => {},
foo8823: () => {},
foo8824: () => {},
foo8825: () => {},
foo8826: () => {},
foo8827: () => {},
foo8828: () => {},
foo8829: () => {},
foo8830: () => {},
foo8831: () => {},
foo8832: () => {},
foo8833: () => {},
foo8834: () => {},
foo8835: () => {},
foo8836: () => {},
foo8837: () => {},
foo8838: () => {},
foo8839: () => {},
foo8840: () => {},
foo8841: () => {},
foo8842: () => {},
foo8843: () => {},
foo8844: () => {},
foo8845: () => {},
foo8846: () => {},
foo8847: () => {},
foo8848: () => {},
foo8849: () => {},
foo8850: () => {},
foo8851: () => {},
foo8852: () => {},
foo8853: () => {},
foo8854: () => {},
foo8855: () => {},
foo8856: () => {},
foo8857: () => {},
foo8858: () => {},
foo8859: () => {},
foo8860: () => {},
foo8861: () => {},
foo8862: () => {},
foo8863: () => {},
foo8864: () => {},
foo8865: () => {},
foo8866: () => {},
foo8867: () => {},
foo8868: () => {},
foo8869: () => {},
foo8870: () => {},
foo8871: () => {},
foo8872: () => {},
foo8873: () => {},
foo8874: () => {},
foo8875: () => {},
foo8876: () => {},
foo8877: () => {},
foo8878: () => {},
foo8879: () => {},
foo8880: () => {},
foo8881: () => {},
foo8882: () => {},
foo8883: () => {},
foo8884: () => {},
foo8885: () => {},
foo8886: () => {},
foo8887: () => {},
foo8888: () => {},
foo8889: () => {},
foo8890: () => {},
foo8891: () => {},
foo8892: () => {},
foo8893: () => {},
foo8894: () => {},
foo8895: () => {},
foo8896: () => {},
foo8897: () => {},
foo8898: () => {},
foo8899: () => {},
foo8900: () => {},
foo8901: () => {},
foo8902: () => {},
foo8903: () => {},
foo8904: () => {},
foo8905: () => {},
foo8906: () => {},
foo8907: () => {},
foo8908: () => {},
foo8909: () => {},
foo8910: () => {},
foo8911: () => {},
foo8912: () => {},
foo8913: () => {},
foo8914: () => {},
foo8915: () => {},
foo8916: () => {},
foo8917: () => {},
foo8918: () => {},
foo8919: () => {},
foo8920: () => {},
foo8921: () => {},
foo8922: () => {},
foo8923: () => {},
foo8924: () => {},
foo8925: () => {},
foo8926: () => {},
foo8927: () => {},
foo8928: () => {},
foo8929: () => {},
foo8930: () => {},
foo8931: () => {},
foo8932: () => {},
foo8933: () => {},
foo8934: () => {},
foo8935: () => {},
foo8936: () => {},
foo8937: () => {},
foo8938: () => {},
foo8939: () => {},
foo8940: () => {},
foo8941: () => {},
foo8942: () => {},
foo8943: () => {},
foo8944: () => {},
foo8945: () => {},
foo8946: () => {},
foo8947: () => {},
foo8948: () => {},
foo8949: () => {},
foo8950: () => {},
foo8951: () => {},
foo8952: () => {},
foo8953: () => {},
foo8954: () => {},
foo8955: () => {},
foo8956: () => {},
foo8957: () => {},
foo8958: () => {},
foo8959: () => {},
foo8960: () => {},
foo8961: () => {},
foo8962: () => {},
foo8963: () => {},
foo8964: () => {},
foo8965: () => {},
foo8966: () => {},
foo8967: () => {},
foo8968: () => {},
foo8969: () => {},
foo8970: () => {},
foo8971: () => {},
foo8972: () => {},
foo8973: () => {},
foo8974: () => {},
foo8975: () => {},
foo8976: () => {},
foo8977: () => {},
foo8978: () => {},
foo8979: () => {},
foo8980: () => {},
foo8981: () => {},
foo8982: () => {},
foo8983: () => {},
foo8984: () => {},
foo8985: () => {},
foo8986: () => {},
foo8987: () => {},
foo8988: () => {},
foo8989: () => {},
foo8990: () => {},
foo8991: () => {},
foo8992: () => {},
foo8993: () => {},
foo8994: () => {},
foo8995: () => {},
foo8996: () => {},
foo8997: () => {},
foo8998: () => {},
foo8999: () => {},
foo9000: () => {},
foo9001: () => {},
foo9002: () => {},
foo9003: () => {},
foo9004: () => {},
foo9005: () => {},
foo9006: () => {},
foo9007: () => {},
foo9008: () => {},
foo9009: () => {},
foo9010: () => {},
foo9011: () => {},
foo9012: () => {},
foo9013: () => {},
foo9014: () => {},
foo9015: () => {},
foo9016: () => {},
foo9017: () => {},
foo9018: () => {},
foo9019: () => {},
foo9020: () => {},
foo9021: () => {},
foo9022: () => {},
foo9023: () => {},
foo9024: () => {},
foo9025: () => {},
foo9026: () => {},
foo9027: () => {},
foo9028: () => {},
foo9029: () => {},
foo9030: () => {},
foo9031: () => {},
foo9032: () => {},
foo9033: () => {},
foo9034: () => {},
foo9035: () => {},
foo9036: () => {},
foo9037: () => {},
foo9038: () => {},
foo9039: () => {},
foo9040: () => {},
foo9041: () => {},
foo9042: () => {},
foo9043: () => {},
foo9044: () => {},
foo9045: () => {},
foo9046: () => {},
foo9047: () => {},
foo9048: () => {},
foo9049: () => {},
foo9050: () => {},
foo9051: () => {},
foo9052: () => {},
foo9053: () => {},
foo9054: () => {},
foo9055: () => {},
foo9056: () => {},
foo9057: () => {},
foo9058: () => {},
foo9059: () => {},
foo9060: () => {},
foo9061: () => {},
foo9062: () => {},
foo9063: () => {},
foo9064: () => {},
foo9065: () => {},
foo9066: () => {},
foo9067: () => {},
foo9068: () => {},
foo9069: () => {},
foo9070: () => {},
foo9071: () => {},
foo9072: () => {},
foo9073: () => {},
foo9074: () => {},
foo9075: () => {},
foo9076: () => {},
foo9077: () => {},
foo9078: () => {},
foo9079: () => {},
foo9080: () => {},
foo9081: () => {},
foo9082: () => {},
foo9083: () => {},
foo9084: () => {},
foo9085: () => {},
foo9086: () => {},
foo9087: () => {},
foo9088: () => {},
foo9089: () => {},
foo9090: () => {},
foo9091: () => {},
foo9092: () => {},
foo9093: () => {},
foo9094: () => {},
foo9095: () => {},
foo9096: () => {},
foo9097: () => {},
foo9098: () => {},
foo9099: () => {},
foo9100: () => {},
foo9101: () => {},
foo9102: () => {},
foo9103: () => {},
foo9104: () => {},
foo9105: () => {},
foo9106: () => {},
foo9107: () => {},
foo9108: () => {},
foo9109: () => {},
foo9110: () => {},
foo9111: () => {},
foo9112: () => {},
foo9113: () => {},
foo9114: () => {},
foo9115: () => {},
foo9116: () => {},
foo9117: () => {},
foo9118: () => {},
foo9119: () => {},
foo9120: () => {},
foo9121: () => {},
foo9122: () => {},
foo9123: () => {},
foo9124: () => {},
foo9125: () => {},
foo9126: () => {},
foo9127: () => {},
foo9128: () => {},
foo9129: () => {},
foo9130: () => {},
foo9131: () => {},
foo9132: () => {},
foo9133: () => {},
foo9134: () => {},
foo9135: () => {},
foo9136: () => {},
foo9137: () => {},
foo9138: () => {},
foo9139: () => {},
foo9140: () => {},
foo9141: () => {},
foo9142: () => {},
foo9143: () => {},
foo9144: () => {},
foo9145: () => {},
foo9146: () => {},
foo9147: () => {},
foo9148: () => {},
foo9149: () => {},
foo9150: () => {},
foo9151: () => {},
foo9152: () => {},
foo9153: () => {},
foo9154: () => {},
foo9155: () => {},
foo9156: () => {},
foo9157: () => {},
foo9158: () => {},
foo9159: () => {},
foo9160: () => {},
foo9161: () => {},
foo9162: () => {},
foo9163: () => {},
foo9164: () => {},
foo9165: () => {},
foo9166: () => {},
foo9167: () => {},
foo9168: () => {},
foo9169: () => {},
foo9170: () => {},
foo9171: () => {},
foo9172: () => {},
foo9173: () => {},
foo9174: () => {},
foo9175: () => {},
foo9176: () => {},
foo9177: () => {},
foo9178: () => {},
foo9179: () => {},
foo9180: () => {},
foo9181: () => {},
foo9182: () => {},
foo9183: () => {},
foo9184: () => {},
foo9185: () => {},
foo9186: () => {},
foo9187: () => {},
foo9188: () => {},
foo9189: () => {},
foo9190: () => {},
foo9191: () => {},
foo9192: () => {},
foo9193: () => {},
foo9194: () => {},
foo9195: () => {},
foo9196: () => {},
foo9197: () => {},
foo9198: () => {},
foo9199: () => {},
foo9200: () => {},
foo9201: () => {},
foo9202: () => {},
foo9203: () => {},
foo9204: () => {},
foo9205: () => {},
foo9206: () => {},
foo9207: () => {},
foo9208: () => {},
foo9209: () => {},
foo9210: () => {},
foo9211: () => {},
foo9212: () => {},
foo9213: () => {},
foo9214: () => {},
foo9215: () => {},
foo9216: () => {},
foo9217: () => {},
foo9218: () => {},
foo9219: () => {},
foo9220: () => {},
foo9221: () => {},
foo9222: () => {},
foo9223: () => {},
foo9224: () => {},
foo9225: () => {},
foo9226: () => {},
foo9227: () => {},
foo9228: () => {},
foo9229: () => {},
foo9230: () => {},
foo9231: () => {},
foo9232: () => {},
foo9233: () => {},
foo9234: () => {},
foo9235: () => {},
foo9236: () => {},
foo9237: () => {},
foo9238: () => {},
foo9239: () => {},
foo9240: () => {},
foo9241: () => {},
foo9242: () => {},
foo9243: () => {},
foo9244: () => {},
foo9245: () => {},
foo9246: () => {},
foo9247: () => {},
foo9248: () => {},
foo9249: () => {},
foo9250: () => {},
foo9251: () => {},
foo9252: () => {},
foo9253: () => {},
foo9254: () => {},
foo9255: () => {},
foo9256: () => {},
foo9257: () => {},
foo9258: () => {},
foo9259: () => {},
foo9260: () => {},
foo9261: () => {},
foo9262: () => {},
foo9263: () => {},
foo9264: () => {},
foo9265: () => {},
foo9266: () => {},
foo9267: () => {},
foo9268: () => {},
foo9269: () => {},
foo9270: () => {},
foo9271: () => {},
foo9272: () => {},
foo9273: () => {},
foo9274: () => {},
foo9275: () => {},
foo9276: () => {},
foo9277: () => {},
foo9278: () => {},
foo9279: () => {},
foo9280: () => {},
foo9281: () => {},
foo9282: () => {},
foo9283: () => {},
foo9284: () => {},
foo9285: () => {},
foo9286: () => {},
foo9287: () => {},
foo9288: () => {},
foo9289: () => {},
foo9290: () => {},
foo9291: () => {},
foo9292: () => {},
foo9293: () => {},
foo9294: () => {},
foo9295: () => {},
foo9296: () => {},
foo9297: () => {},
foo9298: () => {},
foo9299: () => {},
foo9300: () => {},
foo9301: () => {},
foo9302: () => {},
foo9303: () => {},
foo9304: () => {},
foo9305: () => {},
foo9306: () => {},
foo9307: () => {},
foo9308: () => {},
foo9309: () => {},
foo9310: () => {},
foo9311: () => {},
foo9312: () => {},
foo9313: () => {},
foo9314: () => {},
foo9315: () => {},
foo9316: () => {},
foo9317: () => {},
foo9318: () => {},
foo9319: () => {},
foo9320: () => {},
foo9321: () => {},
foo9322: () => {},
foo9323: () => {},
foo9324: () => {},
foo9325: () => {},
foo9326: () => {},
foo9327: () => {},
foo9328: () => {},
foo9329: () => {},
foo9330: () => {},
foo9331: () => {},
foo9332: () => {},
foo9333: () => {},
foo9334: () => {},
foo9335: () => {},
foo9336: () => {},
foo9337: () => {},
foo9338: () => {},
foo9339: () => {},
foo9340: () => {},
foo9341: () => {},
foo9342: () => {},
foo9343: () => {},
foo9344: () => {},
foo9345: () => {},
foo9346: () => {},
foo9347: () => {},
foo9348: () => {},
foo9349: () => {},
foo9350: () => {},
foo9351: () => {},
foo9352: () => {},
foo9353: () => {},
foo9354: () => {},
foo9355: () => {},
foo9356: () => {},
foo9357: () => {},
foo9358: () => {},
foo9359: () => {},
foo9360: () => {},
foo9361: () => {},
foo9362: () => {},
foo9363: () => {},
foo9364: () => {},
foo9365: () => {},
foo9366: () => {},
foo9367: () => {},
foo9368: () => {},
foo9369: () => {},
foo9370: () => {},
foo9371: () => {},
foo9372: () => {},
foo9373: () => {},
foo9374: () => {},
foo9375: () => {},
foo9376: () => {},
foo9377: () => {},
foo9378: () => {},
foo9379: () => {},
foo9380: () => {},
foo9381: () => {},
foo9382: () => {},
foo9383: () => {},
foo9384: () => {},
foo9385: () => {},
foo9386: () => {},
foo9387: () => {},
foo9388: () => {},
foo9389: () => {},
foo9390: () => {},
foo9391: () => {},
foo9392: () => {},
foo9393: () => {},
foo9394: () => {},
foo9395: () => {},
foo9396: () => {},
foo9397: () => {},
foo9398: () => {},
foo9399: () => {},
foo9400: () => {},
foo9401: () => {},
foo9402: () => {},
foo9403: () => {},
foo9404: () => {},
foo9405: () => {},
foo9406: () => {},
foo9407: () => {},
foo9408: () => {},
foo9409: () => {},
foo9410: () => {},
foo9411: () => {},
foo9412: () => {},
foo9413: () => {},
foo9414: () => {},
foo9415: () => {},
foo9416: () => {},
foo9417: () => {},
foo9418: () => {},
foo9419: () => {},
foo9420: () => {},
foo9421: () => {},
foo9422: () => {},
foo9423: () => {},
foo9424: () => {},
foo9425: () => {},
foo9426: () => {},
foo9427: () => {},
foo9428: () => {},
foo9429: () => {},
foo9430: () => {},
foo9431: () => {},
foo9432: () => {},
foo9433: () => {},
foo9434: () => {},
foo9435: () => {},
foo9436: () => {},
foo9437: () => {},
foo9438: () => {},
foo9439: () => {},
foo9440: () => {},
foo9441: () => {},
foo9442: () => {},
foo9443: () => {},
foo9444: () => {},
foo9445: () => {},
foo9446: () => {},
foo9447: () => {},
foo9448: () => {},
foo9449: () => {},
foo9450: () => {},
foo9451: () => {},
foo9452: () => {},
foo9453: () => {},
foo9454: () => {},
foo9455: () => {},
foo9456: () => {},
foo9457: () => {},
foo9458: () => {},
foo9459: () => {},
foo9460: () => {},
foo9461: () => {},
foo9462: () => {},
foo9463: () => {},
foo9464: () => {},
foo9465: () => {},
foo9466: () => {},
foo9467: () => {},
foo9468: () => {},
foo9469: () => {},
foo9470: () => {},
foo9471: () => {},
foo9472: () => {},
foo9473: () => {},
foo9474: () => {},
foo9475: () => {},
foo9476: () => {},
foo9477: () => {},
foo9478: () => {},
foo9479: () => {},
foo9480: () => {},
foo9481: () => {},
foo9482: () => {},
foo9483: () => {},
foo9484: () => {},
foo9485: () => {},
foo9486: () => {},
foo9487: () => {},
foo9488: () => {},
foo9489: () => {},
foo9490: () => {},
foo9491: () => {},
foo9492: () => {},
foo9493: () => {},
foo9494: () => {},
foo9495: () => {},
foo9496: () => {},
foo9497: () => {},
foo9498: () => {},
foo9499: () => {},
foo9500: () => {},
foo9501: () => {},
foo9502: () => {},
foo9503: () => {},
foo9504: () => {},
foo9505: () => {},
foo9506: () => {},
foo9507: () => {},
foo9508: () => {},
foo9509: () => {},
foo9510: () => {},
foo9511: () => {},
foo9512: () => {},
foo9513: () => {},
foo9514: () => {},
foo9515: () => {},
foo9516: () => {},
foo9517: () => {},
foo9518: () => {},
foo9519: () => {},
foo9520: () => {},
foo9521: () => {},
foo9522: () => {},
foo9523: () => {},
foo9524: () => {},
foo9525: () => {},
foo9526: () => {},
foo9527: () => {},
foo9528: () => {},
foo9529: () => {},
foo9530: () => {},
foo9531: () => {},
foo9532: () => {},
foo9533: () => {},
foo9534: () => {},
foo9535: () => {},
foo9536: () => {},
foo9537: () => {},
foo9538: () => {},
foo9539: () => {},
foo9540: () => {},
foo9541: () => {},
foo9542: () => {},
foo9543: () => {},
foo9544: () => {},
foo9545: () => {},
foo9546: () => {},
foo9547: () => {},
foo9548: () => {},
foo9549: () => {},
foo9550: () => {},
foo9551: () => {},
foo9552: () => {},
foo9553: () => {},
foo9554: () => {},
foo9555: () => {},
foo9556: () => {},
foo9557: () => {},
foo9558: () => {},
foo9559: () => {},
foo9560: () => {},
foo9561: () => {},
foo9562: () => {},
foo9563: () => {},
foo9564: () => {},
foo9565: () => {},
foo9566: () => {},
foo9567: () => {},
foo9568: () => {},
foo9569: () => {},
foo9570: () => {},
foo9571: () => {},
foo9572: () => {},
foo9573: () => {},
foo9574: () => {},
foo9575: () => {},
foo9576: () => {},
foo9577: () => {},
foo9578: () => {},
foo9579: () => {},
foo9580: () => {},
foo9581: () => {},
foo9582: () => {},
foo9583: () => {},
foo9584: () => {},
foo9585: () => {},
foo9586: () => {},
foo9587: () => {},
foo9588: () => {},
foo9589: () => {},
foo9590: () => {},
foo9591: () => {},
foo9592: () => {},
foo9593: () => {},
foo9594: () => {},
foo9595: () => {},
foo9596: () => {},
foo9597: () => {},
foo9598: () => {},
foo9599: () => {},
foo9600: () => {},
foo9601: () => {},
foo9602: () => {},
foo9603: () => {},
foo9604: () => {},
foo9605: () => {},
foo9606: () => {},
foo9607: () => {},
foo9608: () => {},
foo9609: () => {},
foo9610: () => {},
foo9611: () => {},
foo9612: () => {},
foo9613: () => {},
foo9614: () => {},
foo9615: () => {},
foo9616: () => {},
foo9617: () => {},
foo9618: () => {},
foo9619: () => {},
foo9620: () => {},
foo9621: () => {},
foo9622: () => {},
foo9623: () => {},
foo9624: () => {},
foo9625: () => {},
foo9626: () => {},
foo9627: () => {},
foo9628: () => {},
foo9629: () => {},
foo9630: () => {},
foo9631: () => {},
foo9632: () => {},
foo9633: () => {},
foo9634: () => {},
foo9635: () => {},
foo9636: () => {},
foo9637: () => {},
foo9638: () => {},
foo9639: () => {},
foo9640: () => {},
foo9641: () => {},
foo9642: () => {},
foo9643: () => {},
foo9644: () => {},
foo9645: () => {},
foo9646: () => {},
foo9647: () => {},
foo9648: () => {},
foo9649: () => {},
foo9650: () => {},
foo9651: () => {},
foo9652: () => {},
foo9653: () => {},
foo9654: () => {},
foo9655: () => {},
foo9656: () => {},
foo9657: () => {},
foo9658: () => {},
foo9659: () => {},
foo9660: () => {},
foo9661: () => {},
foo9662: () => {},
foo9663: () => {},
foo9664: () => {},
foo9665: () => {},
foo9666: () => {},
foo9667: () => {},
foo9668: () => {},
foo9669: () => {},
foo9670: () => {},
foo9671: () => {},
foo9672: () => {},
foo9673: () => {},
foo9674: () => {},
foo9675: () => {},
foo9676: () => {},
foo9677: () => {},
foo9678: () => {},
foo9679: () => {},
foo9680: () => {},
foo9681: () => {},
foo9682: () => {},
foo9683: () => {},
foo9684: () => {},
foo9685: () => {},
foo9686: () => {},
foo9687: () => {},
foo9688: () => {},
foo9689: () => {},
foo9690: () => {},
foo9691: () => {},
foo9692: () => {},
foo9693: () => {},
foo9694: () => {},
foo9695: () => {},
foo9696: () => {},
foo9697: () => {},
foo9698: () => {},
foo9699: () => {},
foo9700: () => {},
foo9701: () => {},
foo9702: () => {},
foo9703: () => {},
foo9704: () => {},
foo9705: () => {},
foo9706: () => {},
foo9707: () => {},
foo9708: () => {},
foo9709: () => {},
foo9710: () => {},
foo9711: () => {},
foo9712: () => {},
foo9713: () => {},
foo9714: () => {},
foo9715: () => {},
foo9716: () => {},
foo9717: () => {},
foo9718: () => {},
foo9719: () => {},
foo9720: () => {},
foo9721: () => {},
foo9722: () => {},
foo9723: () => {},
foo9724: () => {},
foo9725: () => {},
foo9726: () => {},
foo9727: () => {},
foo9728: () => {},
foo9729: () => {},
foo9730: () => {},
foo9731: () => {},
foo9732: () => {},
foo9733: () => {},
foo9734: () => {},
foo9735: () => {},
foo9736: () => {},
foo9737: () => {},
foo9738: () => {},
foo9739: () => {},
foo9740: () => {},
foo9741: () => {},
foo9742: () => {},
foo9743: () => {},
foo9744: () => {},
foo9745: () => {},
foo9746: () => {},
foo9747: () => {},
foo9748: () => {},
foo9749: () => {},
foo9750: () => {},
foo9751: () => {},
foo9752: () => {},
foo9753: () => {},
foo9754: () => {},
foo9755: () => {},
foo9756: () => {},
foo9757: () => {},
foo9758: () => {},
foo9759: () => {},
foo9760: () => {},
foo9761: () => {},
foo9762: () => {},
foo9763: () => {},
foo9764: () => {},
foo9765: () => {},
foo9766: () => {},
foo9767: () => {},
foo9768: () => {},
foo9769: () => {},
foo9770: () => {},
foo9771: () => {},
foo9772: () => {},
foo9773: () => {},
foo9774: () => {},
foo9775: () => {},
foo9776: () => {},
foo9777: () => {},
foo9778: () => {},
foo9779: () => {},
foo9780: () => {},
foo9781: () => {},
foo9782: () => {},
foo9783: () => {},
foo9784: () => {},
foo9785: () => {},
foo9786: () => {},
foo9787: () => {},
foo9788: () => {},
foo9789: () => {},
foo9790: () => {},
foo9791: () => {},
foo9792: () => {},
foo9793: () => {},
foo9794: () => {},
foo9795: () => {},
foo9796: () => {},
foo9797: () => {},
foo9798: () => {},
foo9799: () => {},
foo9800: () => {},
foo9801: () => {},
foo9802: () => {},
foo9803: () => {},
foo9804: () => {},
foo9805: () => {},
foo9806: () => {},
foo9807: () => {},
foo9808: () => {},
foo9809: () => {},
foo9810: () => {},
foo9811: () => {},
foo9812: () => {},
foo9813: () => {},
foo9814: () => {},
foo9815: () => {},
foo9816: () => {},
foo9817: () => {},
foo9818: () => {},
foo9819: () => {},
foo9820: () => {},
foo9821: () => {},
foo9822: () => {},
foo9823: () => {},
foo9824: () => {},
foo9825: () => {},
foo9826: () => {},
foo9827: () => {},
foo9828: () => {},
foo9829: () => {},
foo9830: () => {},
foo9831: () => {},
foo9832: () => {},
foo9833: () => {},
foo9834: () => {},
foo9835: () => {},
foo9836: () => {},
foo9837: () => {},
foo9838: () => {},
foo9839: () => {},
foo9840: () => {},
foo9841: () => {},
foo9842: () => {},
foo9843: () => {},
foo9844: () => {},
foo9845: () => {},
foo9846: () => {},
foo9847: () => {},
foo9848: () => {},
foo9849: () => {},
foo9850: () => {},
foo9851: () => {},
foo9852: () => {},
foo9853: () => {},
foo9854: () => {},
foo9855: () => {},
foo9856: () => {},
foo9857: () => {},
foo9858: () => {},
foo9859: () => {},
foo9860: () => {},
foo9861: () => {},
foo9862: () => {},
foo9863: () => {},
foo9864: () => {},
foo9865: () => {},
foo9866: () => {},
foo9867: () => {},
foo9868: () => {},
foo9869: () => {},
foo9870: () => {},
foo9871: () => {},
foo9872: () => {},
foo9873: () => {},
foo9874: () => {},
foo9875: () => {},
foo9876: () => {},
foo9877: () => {},
foo9878: () => {},
foo9879: () => {},
foo9880: () => {},
foo9881: () => {},
foo9882: () => {},
foo9883: () => {},
foo9884: () => {},
foo9885: () => {},
foo9886: () => {},
foo9887: () => {},
foo9888: () => {},
foo9889: () => {},
foo9890: () => {},
foo9891: () => {},
foo9892: () => {},
foo9893: () => {},
foo9894: () => {},
foo9895: () => {},
foo9896: () => {},
foo9897: () => {},
foo9898: () => {},
foo9899: () => {},
foo9900: () => {},
foo9901: () => {},
foo9902: () => {},
foo9903: () => {},
foo9904: () => {},
foo9905: () => {},
foo9906: () => {},
foo9907: () => {},
foo9908: () => {},
foo9909: () => {},
foo9910: () => {},
foo9911: () => {},
foo9912: () => {},
foo9913: () => {},
foo9914: () => {},
foo9915: () => {},
foo9916: () => {},
foo9917: () => {},
foo9918: () => {},
foo9919: () => {},
foo9920: () => {},
foo9921: () => {},
foo9922: () => {},
foo9923: () => {},
foo9924: () => {},
foo9925: () => {},
foo9926: () => {},
foo9927: () => {},
foo9928: () => {},
foo9929: () => {},
foo9930: () => {},
foo9931: () => {},
foo9932: () => {},
foo9933: () => {},
foo9934: () => {},
foo9935: () => {},
foo9936: () => {},
foo9937: () => {},
foo9938: () => {},
foo9939: () => {},
foo9940: () => {},
foo9941: () => {},
foo9942: () => {},
foo9943: () => {},
foo9944: () => {},
foo9945: () => {},
foo9946: () => {},
foo9947: () => {},
foo9948: () => {},
foo9949: () => {},
foo9950: () => {},
foo9951: () => {},
foo9952: () => {},
foo9953: () => {},
foo9954: () => {},
foo9955: () => {},
foo9956: () => {},
foo9957: () => {},
foo9958: () => {},
foo9959: () => {},
foo9960: () => {},
foo9961: () => {},
foo9962: () => {},
foo9963: () => {},
foo9964: () => {},
foo9965: () => {},
foo9966: () => {},
foo9967: () => {},
foo9968: () => {},
foo9969: () => {},
foo9970: () => {},
foo9971: () => {},
foo9972: () => {},
foo9973: () => {},
foo9974: () => {},
foo9975: () => {},
foo9976: () => {},
foo9977: () => {},
foo9978: () => {},
foo9979: () => {},
foo9980: () => {},
foo9981: () => {},
foo9982: () => {},
foo9983: () => {},
foo9984: () => {},
foo9985: () => {},
foo9986: () => {},
foo9987: () => {},
foo9988: () => {},
foo9989: () => {},
foo9990: () => {},
foo9991: () => {},
foo9992: () => {},
foo9993: () => {},
foo9994: () => {},
foo9995: () => {},
foo9996: () => {},
foo9997: () => {},
foo9998: () => {},
foo9999: () => {},
foo10000: () => {},
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eb67f85^!)  
[src/parsing/func-name-inferrer.h](https://cs.chromium.org/chromium/src/v8/src/parsing/func-name-inferrer.h?cl=eb67f85)  
[src/parsing/parser-base.h](https://cs.chromium.org/chromium/src/v8/src/parsing/parser-base.h?cl=eb67f85)  
[src/parsing/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parsing/parser.cc?cl=eb67f85)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=eb67f85)  
[test/mjsunit/regress/regress-4595.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4595.js?cl=eb67f85)  
  

---   

## **regress-crbug-568525.js (chromium issue)**  
   
**[Issue: object->IsJSArray() in src/objects.cc](https://crbug.com/568525)**  
**[Commit: Fix mix-up in HasEnumerableElements()](https://chromium.googlesource.com/v8/v8/+/989f44f)**  
  
Date(Commit): Thu Dec 10 15:01:49 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1515963002](https://codereview.chromium.org/1515963002)  
Regress: [mjsunit/regress/regress-crbug-568525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-568525.js)  
```javascript
var a = /a/;
a[4] = 1.5;
for (var x in a) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/989f44f^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=989f44f)  
[test/mjsunit/regress/regress-crbug-568525.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-568525.js?cl=989f44f)  
  

---   

## **regress-3641.js (v8 issue)**  
   
**[Issue: Promise with custom |then| method is interpreted incorrectly](https://crbug.com/v8/3641)**  
**[Commit: Clean up promises and fix an edge case bug](https://chromium.googlesource.com/v8/v8/+/1deb89c)**  
  
Date(Commit): Fri Dec 04 18:56:17 2015  
Code Review: [https://codereview.chromium.org/1488783002](https://codereview.chromium.org/1488783002)  
Regress: [mjsunit/regress/regress-3641.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3641.js)  
```javascript
var asyncAssertsExpected = 0;

function assertAsyncRan() { ++asyncAssertsExpected }

function assertLater(f, name) {
  assertFalse(f()); // should not be true synchronously
  ++asyncAssertsExpected;
  var iterations = 0;
  function runAssertion() {
    if (f()) {
      print(name, "succeeded");
      --asyncAssertsExpected;
    } else if (iterations++ < 10) {
      %EnqueueMicrotask(runAssertion);
    } else {
      %AbortJS(name + " FAILED!");
    }
  }
  %EnqueueMicrotask(runAssertion);
}

function assertAsyncDone(iteration) {
  var iteration = iteration || 0;
  %EnqueueMicrotask(function() {
    if (asyncAssertsExpected === 0)
      assertAsync(true, "all")
    else if (iteration > 10)  // Shouldn't take more.
      assertAsync(false, "all... " + asyncAssertsExpected)
    else
      assertAsyncDone(iteration + 1)
  });
}


var y;
var x = Promise.resolve();
x.then = () => { y = true; }
Promise.resolve().then(() => x);
assertLater(() => y === true, "y === true");

assertAsyncDone();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1deb89c^!)  
[src/js/promise.js](https://cs.chromium.org/chromium/src/v8/src/js/promise.js?cl=1deb89c)  
[test/mjsunit/es6/promises.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/promises.js?cl=1deb89c)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=1deb89c)  
[test/mjsunit/regress/regress-3641.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3641.js?cl=1deb89c)  
  

---   

## **regress-new-target-context.js (other issue)**  
   
**[Commit: [fullcode] Switch passing of new.target to register.](https://chromium.googlesource.com/v8/v8/+/440a42b)**  
  
Date(Commit): Thu Dec 03 10:04:35 2015  
Code Review: [https://codereview.chromium.org/1492793002](https://codereview.chromium.org/1492793002)  
Regress: [mjsunit/es6/regress/regress-new-target-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-new-target-context.js)  
```javascript
function makeFun(n) {
  var source = "(function f" + n + "() { ";
  for (var i = 0; i < n; ++i) source += "var v" + i + "; ";
  source += "(function() { 0 ";
  for (var i = 0; i < n; ++i) source += "+ v" + i + " ";
  source += "})(); return { value: new.target }; })";
  return eval(source);
}

var a = makeFun(4);
assertEquals(a, new a().value);
assertEquals(undefined, a().value);

var b = makeFun(128);
assertEquals(b, new b().value);
assertEquals(undefined, b().value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/440a42b^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=440a42b)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=440a42b)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=440a42b)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=440a42b)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=440a42b)  
...  
  
  
---   

## **regress-ensure-initial-map.js (other issue)**  
   
**[Commit: Don't EnsureHasInitialMap on non-constructors.](https://chromium.googlesource.com/v8/v8/+/9bee675)**  
  
Date(Commit): Wed Dec 02 10:39:46 2015  
Code Review: [https://codereview.chromium.org/1490003003](https://codereview.chromium.org/1490003003)  
Regress: [mjsunit/regress/regress-ensure-initial-map.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-ensure-initial-map.js)  
```javascript
var x = Object.getOwnPropertyDescriptor({get x() {}}, 'x').get;
function f(o, b) {
  if (b) {
    return o instanceof x;
  }
};
%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();

function g() {
  return new x();
};
%PrepareFunctionForOptimization(g);
%OptimizeFunctionOnNextCall(g);
assertThrows(() => g());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9bee675^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=9bee675)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=9bee675)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=9bee675)  
[test/mjsunit/regress/regress-ensure-initial-map.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-ensure-initial-map.js?cl=9bee675)  
  
  
---   

## **regress-4585.js (v8 issue)**  
   
**[Issue: Pattern rewriter is unhappy about function literal](https://crbug.com/v8/4585)**  
**[Commit: [parser] treat MethodDefinitions in ObjectPatterns as SyntaxErrors](https://chromium.googlesource.com/v8/v8/+/5058f68)**  
  
Date(Commit): Tue Dec 01 20:33:11 2015  
Code Review: [https://codereview.chromium.org/1488043002](https://codereview.chromium.org/1488043002)  
Regress: [mjsunit/es6/regress/regress-4585.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4585.js)  
```javascript
assertThrows(`for(const { method() {} } = this) {}`, SyntaxError);
assertThrows(`var { method() {} } = this;`, SyntaxError);
assertThrows(`for(const { *method() {} } = this) {}`, SyntaxError);
assertThrows(`var { *method() {} } = this;`, SyntaxError);
assertThrows(`for(var { get foo() {} } = this) {}`, SyntaxError);
assertThrows(`for(var { set foo() {} } = this) {}`, SyntaxError);

for (var { name = "" + { toString() { return "test" } } } in { a: 1}) break;
assertEquals(name, "test");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5058f68^!)  
[src/parsing/preparser.h](https://cs.chromium.org/chromium/src/v8/src/parsing/preparser.h?cl=5058f68)  
[test/cctest/test-parsing.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-parsing.cc?cl=5058f68)  
[test/mjsunit/harmony/regress/regress-4585.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4585.js?cl=5058f68)  
  

---   

## **regress-inlined-new-target.js (other issue)**  
   
**[Commit: [crankshaft] Prevent inlining of new.target functions.](https://chromium.googlesource.com/v8/v8/+/8c793fe)**  
  
Date(Commit): Tue Dec 01 14:19:43 2015  
Code Review: [https://codereview.chromium.org/1484163003](https://codereview.chromium.org/1484163003)  
Regress: [mjsunit/es6/regress/regress-inlined-new-target.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-inlined-new-target.js)  
```javascript
function g() { return { val: new.target }; }
function f() { return (new g()).val; }

%PrepareFunctionForOptimization(f);
assertEquals(g, f());
assertEquals(g, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(g, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8c793fe^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=8c793fe)  
[test/mjsunit/es6/regress/regress-inlined-new-target.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-inlined-new-target.js?cl=8c793fe)  
  
  
---   

## **regress-crbug-563929.js (chromium issue)**  
   
**[Issue: is_int8(disp) in src/x64/assembler-x64.cc](https://crbug.com/563929)**  
**[Commit: [x86] Sane default for Label::Distance on JumpIfRoot/JumpIfNotRoot.](https://chromium.googlesource.com/v8/v8/+/c83db2d)**  
  
Date(Commit): Tue Dec 01 12:23:25 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1483343002](https://codereview.chromium.org/1483343002)  
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
  

---   

## **regress-crbug-551287.js (chromium issue)**  
   
**[Issue: block->IsFinished() in src/crankshaft/hydrogen.cc](https://crbug.com/551287)**  
**[Commit: [crankshaft] Fix crash when case labels inline endless loops](https://chromium.googlesource.com/v8/v8/+/3cb3a6f)**  
  
Date(Commit): Tue Dec 01 12:17:31 2015  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1483373002](https://codereview.chromium.org/1483373002)  
Regress: [mjsunit/regress/regress-crbug-551287.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-551287.js)  
```javascript
function f() {
  do {
  } while (true);
}

function boom(x) {
  switch (x) {
    case 1:
    case f():
      return;
  }
};
%PrepareFunctionForOptimization(boom);
%OptimizeFunctionOnNextCall(boom);
boom(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cb3a6f^!)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=3cb3a6f)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=3cb3a6f)  
[test/mjsunit/regress/regress-crbug-551287.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-551287.js?cl=3cb3a6f)  
  

---   

## **regress-inline-arrow-as-construct.js (other issue)**  
   
**[Commit: Install ConstructNonConstructable as construct stub for non-constructables.](https://chromium.googlesource.com/v8/v8/+/8e28e85)**  
  
Date(Commit): Tue Nov 24 17:17:00 2015  
Code Review: [https://codereview.chromium.org/1467473002](https://codereview.chromium.org/1467473002)  
Regress: [mjsunit/regress/regress-inline-arrow-as-construct.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-arrow-as-construct.js)  
```javascript
var g = () => {};

function f() {
  return new g();
};
%PrepareFunctionForOptimization(f);
assertThrows(f);
assertThrows(f);
%OptimizeFunctionOnNextCall(f);
assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8e28e85^!)  
[src/api-natives.cc](https://cs.chromium.org/chromium/src/v8/src/api-natives.cc?cl=8e28e85)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=8e28e85)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=8e28e85)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=8e28e85)  
[src/builtins.h](https://cs.chromium.org/chromium/src/v8/src/builtins.h?cl=8e28e85)  
...  
  
  
---   

## **regress-crbug-557807.js (chromium issue)**  
   
**[Issue: map->is_stable() in src/compilation-dependencies.cc](https://crbug.com/557807)**  
**[Commit: [turbofan] Unstable prototype maps are not supported currently.](https://chromium.googlesource.com/v8/v8/+/3c9ac97)**  
  
Date(Commit): Thu Nov 19 06:21:06 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1456973004](https://codereview.chromium.org/1456973004)  
Regress: [mjsunit/regress/regress-crbug-557807.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-557807.js)  
```javascript
function bar() {
  return {__proto__: this};
}
function foo(a) {
  a[0] = 0.3;
};
%PrepareFunctionForOptimization(foo);
foo(bar());
%OptimizeFunctionOnNextCall(foo);
foo(bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c9ac97^!)  
[src/compiler/access-info.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/access-info.cc?cl=3c9ac97)  
[test/mjsunit/regress/regress-crbug-557807.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-557807.js?cl=3c9ac97)  
  

---   

## **regress-crbug-554831.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/554831)**  
**[Commit: [crankshaft] only compile string index access with element key.](https://chromium.googlesource.com/v8/v8/+/5bcddae)**  
  
Date(Commit): Wed Nov 18 13:53:34 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1455883004](https://codereview.chromium.org/1455883004)  
Regress: [mjsunit/regress/regress-crbug-554831.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-554831.js)  
```javascript
(function() {
  var key = "s";
  function f(object) { return object[key]; };
  %PrepareFunctionForOptimization(f);
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
  

---   

## **regress-deopt-in-array-literal-spread.js (other issue)**  
   
**[Commit: [turbofan] Fix deoptimization from array literal spread.](https://chromium.googlesource.com/v8/v8/+/279f2aa)**  
  
Date(Commit): Wed Nov 18 11:45:41 2015  
Code Review: [https://codereview.chromium.org/1455953002](https://codereview.chromium.org/1455953002)  
Regress: [mjsunit/regress/regress-deopt-in-array-literal-spread.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deopt-in-array-literal-spread.js)  
```javascript
function f(a, b, c, d) {
  return [a, ...(%DeoptimizeNow(), [b, c]), d];
};
%PrepareFunctionForOptimization(f);
assertEquals([1, 2, 3, 4], f(1, 2, 3, 4));
assertEquals([1, 2, 3, 4], f(1, 2, 3, 4));
%OptimizeFunctionOnNextCall(f);
assertEquals([1, 2, 3, 4], f(1, 2, 3, 4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/279f2aa^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=279f2aa)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=279f2aa)  
[test/mjsunit/regress/regress-deopt-in-array-literal-spread.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-deopt-in-array-literal-spread.js?cl=279f2aa)  
[test/mjsunit/regress/regress-osr-in-literal.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-osr-in-literal.js?cl=279f2aa)  
  
  
---   

## **regress-f64-w32-change.js (other issue)**  
   
**[Commit: [turbofan] Only infer signedness for Float64->Word32 representation change from the input type.](https://chromium.googlesource.com/v8/v8/+/a9fa049)**  
  
Date(Commit): Wed Nov 18 10:02:33 2015  
Code Review: [https://codereview.chromium.org/1455103002](https://codereview.chromium.org/1455103002)  
Regress: [mjsunit/compiler/regress-f64-w32-change.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-f64-w32-change.js)  
```javascript
var f = (function () {
  "use asm";
  var f64use = 0;
  function f(x, b) {
    x = x|0;
    b = b >>> 0;
    var f64 = x ? -1 : b;
    f64use = f64 + 0.5;
    var w32 = x ? 1 : f64;
    return (w32 + 1)|0;
  }

  return f;
})();

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
assertEquals(0, f(0, -1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9fa049^!)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=a9fa049)  
[test/cctest/compiler/test-representation-change.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-representation-change.cc?cl=a9fa049)  
[test/cctest/compiler/test-simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-simplified-lowering.cc?cl=a9fa049)  
[test/mjsunit/compiler/regress-f64-w32-change.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-f64-w32-change.js?cl=a9fa049)  
  
  
---   

## **regress-556543.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::compiler::Node::opcode](https://crbug.com/556543)**  
**[Commit: [turbofan] Check for dead node in the common operator reducer.](https://chromium.googlesource.com/v8/v8/+/a77f917)**  
  
Date(Commit): Tue Nov 17 09:03:10 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1450883003](https://codereview.chromium.org/1450883003)  
Regress: [mjsunit/regress/regress-556543.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-556543.js)  
```javascript
function f() {
  for (var __v_2 = 0; __v_2 < __v_5; ++__v_2) {
    for (var __v_5 = 0; __v_3 < 1; ++__v_8) {
      if (true || 0 > -6) continue;
      for (var __v_3 = 0; __v_3 < 1; ++__v_3) {
      }
    }
  }
};
%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a77f917^!)  
[src/compiler/common-operator-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator-reducer.cc?cl=a77f917)  
[test/mjsunit/regress/regress-556543.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-556543.js?cl=a77f917)  
  

---   

## **regress-554865.js (chromium issue)**  
   
**[Issue: Crash in v8::internal::Execution::ToObject](https://crbug.com/554865)**  
**[Commit: Run the materialized literal reindexer on default parameter initializers](https://chromium.googlesource.com/v8/v8/+/e971005)**  
  
Date(Commit): Fri Nov 13 17:11:05 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, findit-wrong, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1442653004](https://codereview.chromium.org/1442653004)  
Regress: [mjsunit/regress/regress-554865.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-554865.js)  
```javascript
(function() {
  var x = {};
  ((y = [42]) => assertEquals(42, y[0]))();
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e971005^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=e971005)  
[test/mjsunit/regress/regress-554865.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-554865.js?cl=e971005)  
  

---   

## **regress-crbug-554946.js (chromium issue)**  
   
**[Issue: Security: Pwn2Own mobile case, out-of-bound access in json stringifier](https://crbug.com/554946)**  
**[Commit: [JSON stringifier] Correctly load array elements.](https://chromium.googlesource.com/v8/v8/+/6df9a1d)**  
  
Date(Commit): Thu Nov 12 19:30:58 2015  
Components: Blink>JavaScript  
Labels: Release-0-M47, M-47, CVE-2015-6764, merge-merged-46, Security_Impact-Stable, Security_Severity-High, reward-7500, Merge-merged-47, allpublic, merge-merged-48, CVE_description-submitted  
Code Review: [https://codereview.chromium.org/1435083003](https://codereview.chromium.org/1435083003)  
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
  

---   

## **regress-crbug-523919.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/523919)**  
**[Commit: Serializer: attach alignment to deferred objects.](https://chromium.googlesource.com/v8/v8/+/ee9020d)**  
  
Date(Commit): Thu Nov 12 11:28:31 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1440983002](https://codereview.chromium.org/1440983002)  
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
  

---   

## **regress-arguments-slice.js (other issue)**  
   
**[Commit: Fix Array.prototype.slice with arguments object with negative length.](https://chromium.googlesource.com/v8/v8/+/2ebd5fc)**  
  
Date(Commit): Wed Nov 11 11:50:38 2015  
Code Review: [https://codereview.chromium.org/1436813002](https://codereview.chromium.org/1436813002)  
Regress: [mjsunit/regress/regress-arguments-slice.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arguments-slice.js)  
```javascript
function f() { return arguments; }
var o = f();
o.length = -100;
Array.prototype.slice.call(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ebd5fc^!)  
[src/builtins.cc](https://cs.chromium.org/chromium/src/v8/src/builtins.cc?cl=2ebd5fc)  
[test/mjsunit/regress/regress-arguments-slice.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-arguments-slice.js?cl=2ebd5fc)  
  
  
---   

## **regress-552302.js (chromium issue)**  
   
**[Issue: Unreachable code in src/pattern-rewriter.cc](https://crbug.com/552302)**  
**[Commit: Properly handle parsing a '%'-prefixed runtime call as a binding pattern](https://chromium.googlesource.com/v8/v8/+/9a8c011)**  
  
Date(Commit): Mon Nov 09 15:32:25 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1425723004](https://codereview.chromium.org/1425723004)  
Regress: [mjsunit/regress/regress-552302.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-552302.js)  
```javascript
assertThrows('var %OptimizeFunctionOnNextCall()', SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9a8c011^!)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=9a8c011)  
[test/mjsunit/regress/regress-552302.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-552302.js?cl=9a8c011)  
  

---   

## **regress-crbug-548580.js (chromium issue)**  
   
**[Issue: value->IsHeapObject() in src/objects-debug.cc](https://crbug.com/548580)**  
**[Commit: Regression test for JSRegExp literals sharing.](https://chromium.googlesource.com/v8/v8/+/37a9be5)**  
  
Date(Commit): Sat Nov 07 08:19:27 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1429743006](https://codereview.chromium.org/1429743006)  
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
  

---   

## **regress-crbug-552304.js (chromium issue)**  
   
**[Issue: -1 <= index && index < param_count in src/frames-inl.h](https://crbug.com/552304)**  
**[Commit: [turbofan] Fix wrong parameter indices in JSFrameSpecialization.](https://chromium.googlesource.com/v8/v8/+/925a200)**  
  
Date(Commit): Fri Nov 06 13:12:51 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1429233004](https://codereview.chromium.org/1429233004)  
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
  

---   

## **regress-4534.js (v8 issue)**  
   
**[Issue: mjsunit/accessor-map-sharing fails gc-stress with certain gc timings](https://crbug.com/v8/4534)**  
**[Commit: Fix accessor map transitions vs. Object.defineProperty](https://chromium.googlesource.com/v8/v8/+/b4d46bc)**  
  
Date(Commit): Tue Nov 03 14:41:53 2015  
Code Review: [https://codereview.chromium.org/1413723011](https://codereview.chromium.org/1413723011)  
Regress: [mjsunit/regress/regress-4534.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4534.js)  
```javascript
var dp = Object.defineProperty;
function getter() { return 111; }
function setter(x) { print(222); }
obj1 = {};
dp(obj1, "golf", { get: getter, configurable: true });
dp(obj1, "golf", { set: setter, configurable: true });
gc();
obj2 = {};
dp(obj2, "golf", { get: getter, configurable: true });
dp(obj2, "golf", { set: setter, configurable: true });
assertTrue(%HaveSameMap(obj1, obj2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4d46bc^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=b4d46bc)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=b4d46bc)  
[test/mjsunit/regress/regress-4534.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4534.js?cl=b4d46bc)  
  

---   

## **regress-crbug-549162.js (chromium issue)**  
   
**[Issue: index >= 0 && index < this->length() in src/objects-inl.h](https://crbug.com/549162)**  
**[Commit: Fix cached EnumLength retrieval in JSObject::NumberOfOwnProperties](https://chromium.googlesource.com/v8/v8/+/70a2f53)**  
  
Date(Commit): Fri Oct 30 10:35:43 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1424293002](https://codereview.chromium.org/1424293002)  
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
  

---   

## **regress-4525.js (v8 issue)**  
   
**[Issue: TurboFan super property calls not performed as method calls](https://crbug.com/v8/4525)**  
**[Commit: [turbofan] Fix super property calls to act as method calls.](https://chromium.googlesource.com/v8/v8/+/26f90c9)**  
  
Date(Commit): Thu Oct 29 17:19:39 2015  
Code Review: [https://codereview.chromium.org/1419173004](https://codereview.chromium.org/1419173004)  
Regress: [mjsunit/regress/regress-4525.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4525.js)  
```javascript
function receiver() {
  return this;
}

function construct(f) {
  "use strict";
  class B {}
  class C extends B {
    bar() { return super.foo() }
  }
  B.prototype.foo = f;
  return new C();
}

function check(x, value, type) {
  assertEquals("object", typeof x);
  assertInstanceof(x, type);
  assertEquals(value, x);
}

var o = construct(receiver);
%PrepareFunctionForOptimization(o.bar);
check(o.bar.call(123), Object(123), Number);
check(o.bar.call("a"), Object("a"), String);
check(o.bar.call(undefined), this, Object);
check(o.bar.call(null), this, Object);

%OptimizeFunctionOnNextCall(o.bar);
check(o.bar.call(456), Object(456), Number);
check(o.bar.call("b"), Object("b"), String);
check(o.bar.call(undefined), this, Object);
check(o.bar.call(null), this, Object);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/26f90c9^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=26f90c9)  
[test/mjsunit/regress/regress-4525.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4525.js?cl=26f90c9)  
  

---   

## **regress-4395-global-eval.js (v8 issue)**  
   
**[Issue: assignments in default parameter initializers](https://crbug.com/v8/4395)**  
**[Commit: Fix eval calls in initializers of arrow function parameters](https://chromium.googlesource.com/v8/v8/+/0bdaa4d)**  
  
Date(Commit): Thu Oct 29 15:16:40 2015  
Code Review: [https://codereview.chromium.org/1423613002](https://codereview.chromium.org/1423613002)  
Regress: [mjsunit/es6/regress/regress-4395-global-eval.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4395-global-eval.js), [mjsunit/es6/regress/regress-4395.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4395.js)  
```javascript
((x, y = eval('x')) => assertEquals(42, y))(42);
((x, {y = eval('x')}) => assertEquals(42, y))(42, {});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0bdaa4d^!)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=0bdaa4d)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=0bdaa4d)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=0bdaa4d)  
[test/mjsunit/harmony/regress/regress-4395-global-eval.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4395-global-eval.js?cl=0bdaa4d)  
[test/mjsunit/harmony/regress/regress-4395.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4395.js?cl=0bdaa4d)  
  

---   

## **regress-4522.js (v8 issue)**  
   
**[Issue: super access in eval in arrow function fails](https://crbug.com/v8/4522)**  
**[Commit: Properly handle direct evals referencing super in arrow functions](https://chromium.googlesource.com/v8/v8/+/4c3c89c)**  
  
Date(Commit): Thu Oct 29 15:09:51 2015  
Code Review: [https://codereview.chromium.org/1411093008](https://codereview.chromium.org/1411093008)  
Regress: [mjsunit/es6/regress/regress-4522.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4522.js)  
```javascript
"use strict";

class C {
  foo() {
    return 42;
  }
}

class D extends C {
  foo() {
    return (() => eval("super.foo()"))();
  }
}

assertEquals(42, new D().foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c3c89c^!)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=4c3c89c)  
[test/mjsunit/es6/regress/regress-4522.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-4522.js?cl=4c3c89c)  
  

---   

## **regress-4521.js (v8 issue)**  
   
**[Issue: TurboFan environment borked for Call::PROPERTY_CALL to super](https://crbug.com/v8/4521)**  
**[Commit: [turbofan] Fix and rework deopt in call to super property.](https://chromium.googlesource.com/v8/v8/+/d3c4adf)**  
  
Date(Commit): Thu Oct 29 12:32:49 2015  
Code Review: [https://codereview.chromium.org/1426893003](https://codereview.chromium.org/1426893003)  
Regress: [mjsunit/regress/regress-4521.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4521.js)  
```javascript
"use strict";

class B {
  foo() { return 23 }
}

class C extends B {
  bar() { return super[%DeoptimizeFunction(C.prototype.bar), "foo"]() }
}
%PrepareFunctionForOptimization(C.prototype.bar);

assertEquals(23, new C().bar());
assertEquals(23, new C().bar());
%OptimizeFunctionOnNextCall(C.prototype.bar);
assertEquals(23, new C().bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d3c4adf^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=d3c4adf)  
[test/mjsunit/regress/regress-4521.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4521.js?cl=d3c4adf)  
  

---   

## **regress-4450.js (v8 issue)**  
   
**[Issue: Sliced string keys with Unicode characters are broken](https://crbug.com/v8/4450)**  
**[Commit: Make AstRawString deduplication encoding-agnostic.](https://chromium.googlesource.com/v8/v8/+/200315c)**  
  
Date(Commit): Wed Oct 28 11:28:55 2015  
Code Review: [https://codereview.chromium.org/1411103006](https://codereview.chromium.org/1411103006)  
Regress: [mjsunit/regress/regress-4450.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4450.js)  
```javascript
({})['foobar\u2653'.slice(0, 6)] = null;
var x;
eval('x = function foobar() { return foobar };');
x();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/200315c^!)  
[src/ast-value-factory.cc](https://cs.chromium.org/chromium/src/v8/src/ast-value-factory.cc?cl=200315c)  
[test/mjsunit/regress/regress-4450.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4450.js?cl=200315c)  
  

---   

## **regress-4515.js (v8 issue)**  
   
**[Issue: TurboFan specialization of JSArray::length borked](https://crbug.com/v8/4515)**  
**[Commit: [turbofan] Fix representation type for JSArray::length.](https://chromium.googlesource.com/v8/v8/+/e121aab)**  
  
Date(Commit): Mon Oct 26 12:04:16 2015  
Code Review: [https://codereview.chromium.org/1410393006](https://codereview.chromium.org/1410393006)  
Regress: [mjsunit/regress/regress-4515.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4515.js)  
```javascript
function f(array) {
  return array.length >>> 0;
};
%PrepareFunctionForOptimization(f);
var a = new Array();
a[4000000000] = "A";

assertEquals(4000000001, f(a));
assertEquals(4000000001, f(a));
%OptimizeFunctionOnNextCall(f);
assertEquals(4000000001, f(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e121aab^!)  
[src/compiler/js-native-context-specialization.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-native-context-specialization.cc?cl=e121aab)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=e121aab)  
[test/mjsunit/regress/regress-4515.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4515.js?cl=e121aab)  
  

---   

## **regress-4470-1.js (v8 issue)**  
   
**[Issue: Support native context specialization in TurboFan](https://crbug.com/v8/4470)**  
**[Commit: [turbofan] Add test case for stores to properties that are also present on prototype.](https://chromium.googlesource.com/v8/v8/+/2ab54f1)**  
  
Date(Commit): Fri Oct 23 12:09:54 2015  
Code Review: [https://codereview.chromium.org/1407233006](https://codereview.chromium.org/1407233006)  
Regress: [mjsunit/compiler/regress-4470-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4470-1.js)  
```javascript
function Foo() {}
Foo.prototype.x = 0;
function foo(f) {
  f.x = 1;
}
%PrepareFunctionForOptimization(foo);
foo(new Foo);
foo(new Foo);
%OptimizeFunctionOnNextCall(foo);
foo(new Foo);
assertEquals(Foo.prototype.x, 0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ab54f1^!)  
[test/mjsunit/compiler/regress-4470-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4470-1.js?cl=2ab54f1)  
  

---   

## **regress-inline-class-constructor.js (other issue)**  
   
**[Commit: Ensure we never inline class constructors in Crankshaft, as it currently is entirely unsupported.](https://chromium.googlesource.com/v8/v8/+/f464f12)**  
  
Date(Commit): Thu Oct 22 14:39:07 2015  
Code Review: [https://codereview.chromium.org/1415723005](https://codereview.chromium.org/1415723005)  
Regress: [mjsunit/regress/regress-inline-class-constructor.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-inline-class-constructor.js)  
```javascript
"use strict";

var B = class extends Int32Array {};

function f(b) {
  if (b) {
    null instanceof B;
  }
};
%PrepareFunctionForOptimization(f);
f();
f();
f();
%OptimizeFunctionOnNextCall(f);
f();

function f2() {
  return new B();
};
%PrepareFunctionForOptimization(f2);
%OptimizeFunctionOnNextCall(f2);
f2();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f464f12^!)  
[src/crankshaft/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen.cc?cl=f464f12)  
[test/mjsunit/regress/regress-inline-class-constructor.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-inline-class-constructor.js?cl=f464f12)  
  
  
---   

## **regress-4507.js (v8 issue)**  
   
**[Issue: Large integer is treated as signed int and division gives wrong value](https://crbug.com/v8/4507)**  
**[Commit: [Crankshaft] Don't do HMathFloorOfDiv optimization for kUint32 values](https://chromium.googlesource.com/v8/v8/+/fdfab67)**  
  
Date(Commit): Thu Oct 22 13:22:09 2015  
Code Review: [https://codereview.chromium.org/1409353005](https://codereview.chromium.org/1409353005)  
Regress: [mjsunit/regress/regress-4507.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4507.js)  
```javascript
function broken(value) {
  return Math.floor(value / 65536);
}
function toUnsigned(i) {
  return i >>> 0;
}
function outer(i) {
  return broken(toUnsigned(i));
};
%PrepareFunctionForOptimization(outer);
for (var i = 0; i < 5; i++) outer(0);
broken(0x80000000);  // Spice things up with a sprinkling of type feedback.
%OptimizeFunctionOnNextCall(outer);
assertEquals(32768, outer(0x80000000));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fdfab67^!)  
[src/crankshaft/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/crankshaft/hydrogen-instructions.cc?cl=fdfab67)  
[test/mjsunit/regress/regress-4507.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4507.js?cl=fdfab67)  
  

---   

## **regress-variable-liveness-let.js (other issue)**  
   
**[Commit: [turbofan] Fix liveness analysis for let variable in TDZ.](https://chromium.googlesource.com/v8/v8/+/d9a5add)**  
  
Date(Commit): Wed Oct 21 12:23:06 2015  
Code Review: [https://codereview.chromium.org/1420573002](https://codereview.chromium.org/1420573002)  
Regress: [mjsunit/compiler/regress-variable-liveness-let.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-variable-liveness-let.js)  
```javascript
"use strict";

function f() {
  %DeoptimizeNow();
  let x = 23;
}

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d9a5add^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=d9a5add)  
[test/mjsunit/compiler/regress-variable-liveness-let.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-variable-liveness-let.js?cl=d9a5add)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=d9a5add)  
  
  
---   

## **regress-crbug-545364.js (chromium issue)**  
   
**[Issue: AllowHeapAllocation::IsAllowed() in src/objects.cc](https://crbug.com/545364)**  
**[Commit: [turbofan] We cannot unconditionally flatten cons strings in the JSGraph.](https://chromium.googlesource.com/v8/v8/+/d168a1e)**  
  
Date(Commit): Tue Oct 20 15:48:07 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1413373002](https://codereview.chromium.org/1413373002)  
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
  

---   

## **regress-4493-1.js (v8 issue)**  
   
**[Issue: Enable general purpose inlining in TurboFan](https://crbug.com/v8/4493)**  
**[Commit: [turbofan] Respect effect input when lowering JSToBoolean for string inputs.](https://chromium.googlesource.com/v8/v8/+/2abd768)**  
  
Date(Commit): Tue Oct 20 15:24:26 2015  
Code Review: [https://codereview.chromium.org/1418643002](https://codereview.chromium.org/1418643002)  
Regress: [mjsunit/regress/regress-4493-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4493-1.js)  
```javascript
function baz(x, f) {
  return x.length;
};

function bar(x, y) {
  if (y) {}
  baz(x, function() {
    return x;
  });
};

function foo(x) {
  bar(x, '');
};
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo(['a']);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2abd768^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=2abd768)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=2abd768)  
[test/mjsunit/regress/regress-4493-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4493-1.js?cl=2abd768)  
  

---   

## **regress-544991.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/544991)**  
**[Commit: Refactor array construction for map, filter](https://chromium.googlesource.com/v8/v8/+/c227dd5)**  
  
Date(Commit): Tue Oct 20 09:57:08 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1408213004](https://codereview.chromium.org/1408213004)  
Regress: [mjsunit/regress/regress-544991.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-544991.js)  
```javascript
'use strict';

var typedArray = new Int8Array(1);
var saved;
var called;
class TypedArraySubclass extends Int8Array {
  constructor(x) {
    super(x);
    called = true;
    saved = x;
  }
}
typedArray.constructor = TypedArraySubclass
typedArray.map(function(){});

assertTrue(called);
assertEquals(saved, 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c227dd5^!)  
[src/js/array.js](https://cs.chromium.org/chromium/src/v8/src/js/array.js?cl=c227dd5)  
[test/mjsunit/regress/regress-544991.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-544991.js?cl=c227dd5)  
  

---   

## **regress-4495.js (v8 issue)**  
   
**[Issue: mjsunit/es6/collections crashes flakily on win32](https://crbug.com/v8/4495)**  
**[Commit: VectorICs: Bugfix in KeyedStore dispatcher.](https://chromium.googlesource.com/v8/v8/+/2f2302f)**  
  
Date(Commit): Mon Oct 19 09:51:46 2015  
Code Review: [https://codereview.chromium.org/1415533003](https://codereview.chromium.org/1415533003)  
Regress: [mjsunit/regress/regress-4495.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4495.js)  
```javascript
function foo(a, s) { a[s] = 35; }
var x = { bilbo: 3 };
var y = { frodo: 3, bilbo: 'hi' };
foo(x, "bilbo");
foo(x, "bilbo");
foo(y, "bilbo");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f2302f^!)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=2f2302f)  
[src/x87/code-stubs-x87.cc](https://cs.chromium.org/chromium/src/v8/src/x87/code-stubs-x87.cc?cl=2f2302f)  
[test/mjsunit/regress/regress-4495.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4495.js?cl=2f2302f)  
  

---   

## **regress-543994.js (chromium issue)**  
   
**[Issue: Crash in NULL@0x...60](https://crbug.com/543994)**  
**[Commit: [turbofan] Introduce lazy bailout, masked as a call.](https://chromium.googlesource.com/v8/v8/+/f9a9c6b)**  
  
Date(Commit): Mon Oct 19 06:21:26 2015  
Components: Blink>JavaScript  
Labels: M-47, Reproducible, Stability-Memory-AddressSanitizer, Nag, merge-rejected-4.7, Security_Severity-Low, M-46, Security_Impact-None, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1412443003](https://codereview.chromium.org/1412443003)  
Regress: [mjsunit/regress/regress-543994.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-543994.js)  
```javascript
try { a = f();
} catch(e) {
}
var i = 0;
function f() {
   try {
     f();
   } catch(e) {
     i++;
     [];
   }
}
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f9a9c6b^!)  
[src/compiler/arm/code-generator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/code-generator-arm.cc?cl=f9a9c6b)  
[src/compiler/arm/instruction-selector-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/instruction-selector-arm.cc?cl=f9a9c6b)  
[src/compiler/arm64/code-generator-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/code-generator-arm64.cc?cl=f9a9c6b)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=f9a9c6b)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=f9a9c6b)  
...  
  

---   

## **regress-arm64-spillslots.js (other issue)**  
   
**[Commit: [arm64] Fix jssp based spill slot accesses in Crankshaft](https://chromium.googlesource.com/v8/v8/+/102e3e8)**  
  
Date(Commit): Thu Oct 15 13:34:15 2015  
Code Review: [https://codereview.chromium.org/1401703003](https://codereview.chromium.org/1401703003)  
Regress: [mjsunit/regress/regress-arm64-spillslots.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arm64-spillslots.js)  
```javascript
"use strict";

function Message(message) {
  this.message = message;
}

function Inlined(input) {
  var dummy = arguments[1] === undefined;
  if (input instanceof Message) {
    return input;
  }
  print("unreachable, but we must create register allocation complexity");
  return [];
}

function Process(input) {
  var ret = [];
  ret.push(Inlined(input[0], 1, 2));
  return ret;
};
%PrepareFunctionForOptimization(Process);
var input = [new Message("TEST PASS")];

Process(input);
Process(input);
%OptimizeFunctionOnNextCall(Process);
var result = Process(input);
assertEquals("TEST PASS", result[0].message);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/102e3e8^!)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=102e3e8)  
[test/mjsunit/regress/regress-arm64-spillslots.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-arm64-spillslots.js?cl=102e3e8)  
  
  
---   

## **regress-539875.js (chromium issue)**  
   
**[Issue: Security: Symbols ignored in Object.{freeze, seal, isFrozen, isSealed}()](https://crbug.com/539875)**  
**[Commit: Take Symbol-keyed properties into account in Object.freeze and friends](https://chromium.googlesource.com/v8/v8/+/b646cb3)**  
  
Date(Commit): Thu Oct 15 13:32:57 2015  
Components: Blink>JavaScript>Language  
Labels: Release-0-M47, M-47, reward-0, Security_Impact-Stable, Security_Severity-Medium, M-46, merge-merged-4.7, allpublic  
Code Review: [https://codereview.chromium.org/1393373005](https://codereview.chromium.org/1393373005)  
Regress: [mjsunit/regress/regress-539875.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-539875.js)  
```javascript
(function testSeal() {
  var sloppy = arguments;
  var sym = Symbol();
  sloppy[sym] = 123;
  Object.seal(sloppy);
  assertTrue(Object.isSealed(sloppy));
  var desc = Object.getOwnPropertyDescriptor(sloppy, sym);
  assertEquals(123, desc.value);
  assertFalse(desc.configurable);
  assertTrue(desc.writable);
})();


(function testFreeze() {
  var sloppy = arguments;
  var sym = Symbol();
  sloppy[sym] = 123;
  Object.freeze(sloppy);
  assertTrue(Object.isFrozen(sloppy));
  var desc = Object.getOwnPropertyDescriptor(sloppy, sym);
  assertEquals(123, desc.value);
  assertFalse(desc.configurable);
  assertFalse(desc.writable);
})();


(function testIsFrozenAndIsSealed() {
  var sym = Symbol();
  var obj = { [sym]: 123 };
  Object.preventExtensions(obj);
  assertFalse(Object.isFrozen(obj));
  assertFalse(Object.isSealed(obj));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b646cb3^!)  
[src/js/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/js/v8natives.js?cl=b646cb3)  
[test/mjsunit/regress/regress-539875.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-539875.js?cl=b646cb3)  
  

---   

## **regress-542823.js (chromium issue)**  
   
**[Issue: (0 < size) && (size <= Page::kMaxRegularHeapObjectSize) in src/heap/mark-compact](https://crbug.com/542823)**  
**[Commit: Bailout for large object allocations in full code EmitFastOneByteArrayJoin.](https://chromium.googlesource.com/v8/v8/+/24622f5)**  
  
Date(Commit): Wed Oct 14 12:44:45 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1391373004](https://codereview.chromium.org/1391373004)  
Regress: [mjsunit/regress/regress-542823.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-542823.js)  
```javascript
__v_0 = 100000;
__v_1 = new Array();
for (var __v_2 = 0; __v_2 < __v_0; __v_2++) {
  __v_1[__v_2] = 0.5;
}
for (var __v_2 = 0; __v_2 < 10; __v_2++) {
  var __v_0 = __v_1 + 0.5;
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/24622f5^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=24622f5)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=24622f5)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=24622f5)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=24622f5)  
[src/full-codegen/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips64/full-codegen-mips64.cc?cl=24622f5)  
...  
  

---   

## **regress-crbug-528379.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSFunction()) in src/objec](https://crbug.com/528379)**  
**[Commit: Check for validity when accessing call site objects in runtime.](https://chromium.googlesource.com/v8/v8/+/82b3082)**  
  
Date(Commit): Tue Oct 13 10:53:22 2015  
Components: Blink>JavaScript  
Labels: Hotlist-Recharge, Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1404613002](https://codereview.chromium.org/1404613002)  
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
  

---   

## **regress-crbug-542101.js (chromium issue)**  
   
**[Issue: !HasFastProperties() in src/objects-inl.h](https://crbug.com/542101)**  
**[Commit: Fix Error object value lookups.](https://chromium.googlesource.com/v8/v8/+/1a94bc2)**  
  
Date(Commit): Tue Oct 13 09:26:47 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1403923002](https://codereview.chromium.org/1403923002)  
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
  

---   

## **regress-4482.js (v8 issue)**  
   
**[Issue: Function name binding for function expressions should retain legacy const behavior in sloppy mode](https://crbug.com/v8/4482)**  
**[Commit: Don't throw on assignment to function name binding in harmony sloppy mode](https://chromium.googlesource.com/v8/v8/+/18534df)**  
  
Date(Commit): Mon Oct 12 16:55:35 2015  
Code Review: [https://codereview.chromium.org/1397513004](https://codereview.chromium.org/1397513004)  
Regress: [mjsunit/es6/regress/regress-4482.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4482.js)  
```javascript
assertEquals("function", (function f() { f = 42; return typeof f })());
assertEquals("function",
             (function* g() { g = 42; yield typeof g })().next().value);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/18534df^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=18534df)  
[test/mjsunit/harmony/regress/regress-4482.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4482.js?cl=18534df)  
  

---   

## **regress-4466.js (v8 issue)**  
   
**[Issue: "'super' keyword unexpected" in nested arrows within method](https://crbug.com/v8/4466)**  
**[Commit: Use Scope::function_kind_ to distinguish arrow function scopes](https://chromium.googlesource.com/v8/v8/+/24565b8)**  
  
Date(Commit): Wed Oct 07 14:55:45 2015  
Code Review: [https://codereview.chromium.org/1386253002](https://codereview.chromium.org/1386253002)  
Regress: [mjsunit/es6/regress/regress-4466.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4466.js)  
```javascript
"use strict";

class Parent {
  parentMethod(x, y) {
    assertEquals(42, x);
    assertEquals('hello world', y);
  }
}

class Child extends Parent {
  method(x) {
    let outer = (y) => {
      let inner = () => {
        super.parentMethod(x, y);
      };
      inner();
    };
    outer('hello world');
  }
}

new Child().method(42);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/24565b8^!)  
[src/debug/debug-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/debug/debug-scopes.cc?cl=24565b8)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=24565b8)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=24565b8)  
[src/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/preparser.cc?cl=24565b8)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=24565b8)  
...  
  

---   

## **regress-crbug-540593.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsScript()) in src/objects-i](https://crbug.com/540593)**  
**[Commit: [turbofan] Don't try to inline non-inlineable functions.](https://chromium.googlesource.com/v8/v8/+/a916059)**  
  
Date(Commit): Wed Oct 07 11:43:39 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1395453002](https://codereview.chromium.org/1395453002)  
Regress: [mjsunit/compiler/regress-crbug-540593.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-crbug-540593.js)  
```javascript
var __f_2 = (function(stdlib) {
  "use asm";
  var __v_3 = stdlib.Symbol;
  function __f_2() { return __v_3(); }
  return __f_2;
})(this);
%PrepareFunctionForOptimization(__f_2);
%OptimizeFunctionOnNextCall(__f_2);
__f_2();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a916059^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=a916059)  
[test/mjsunit/compiler/regress-crbug-540593.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-crbug-540593.js?cl=a916059)  
  

---   

## **regress-4413-1.js (v8 issue)**  
   
**[Issue: Call sequence is inconsistent and almost always wrong](https://crbug.com/v8/4413)**  
**[Commit: [builtins] Make sure argument count is always valid for C++ builtins.](https://chromium.googlesource.com/v8/v8/+/9c8262f)**  
  
Date(Commit): Tue Oct 06 08:23:51 2015  
Code Review: [https://codereview.chromium.org/1391543002](https://codereview.chromium.org/1391543002)  
Regress: [mjsunit/compiler/regress-4413-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4413-1.js)  
```javascript
var foo = (function(stdlib) {
  "use asm";
  var bar = stdlib.Symbol;
  function foo() { return bar("lala"); }
  return foo;
})(this);

%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c8262f^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=9c8262f)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=9c8262f)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=9c8262f)  
[src/mips/builtins-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/builtins-mips.cc?cl=9c8262f)  
[src/mips64/builtins-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/builtins-mips64.cc?cl=9c8262f)  
...  
  

---   

## **regress-crbug-538086.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsFixedArray()) in src/objec](https://crbug.com/538086)**  
**[Commit: Fix FixedArrayBase cast in NumberOfOwnElements](https://chromium.googlesource.com/v8/v8/+/ecf2327)**  
  
Date(Commit): Fri Oct 02 11:49:00 2015  
Components: Blink>JavaScript  
Labels: Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1384673003](https://codereview.chromium.org/1384673003)  
Regress: [mjsunit/regress/regress-crbug-538086.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-538086.js)  
```javascript
this[1]++;
for (var x in this) {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ecf2327^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=ecf2327)  
[test/mjsunit/regress/regress-crbug-538086.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-538086.js?cl=ecf2327)  
  

---   

## **regress-2529.js (v8 issue)**  
   
**[Issue: Wrong completion value for try-finally](https://crbug.com/v8/2529)**  
**[Commit: Fix completion of try..finally.](https://chromium.googlesource.com/v8/v8/+/cf82eea)**  
  
Date(Commit): Thu Oct 01 13:59:56 2015  
Code Review: [https://codereview.chromium.org/1375203004](https://codereview.chromium.org/1375203004)  
Regress: [mjsunit/regress/regress-2529.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-2529.js)  
```javascript
function makeScript(s) {
  return 'while(true) { try { "try"; break } finally { "finally" }; ' + s + ' }';
}

var s1 = makeScript('');
var s2 = makeScript('y = "done"');
var s3 = makeScript('if (true) 2; else var x = 3;');
var s4 = makeScript('if (true) 2; else 3;');

assertEquals("try", eval(s1));
assertEquals("try", eval(s2));
assertEquals("try", eval(s3));
assertEquals("try", eval(s4));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf82eea^!)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=cf82eea)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=cf82eea)  
[src/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/pattern-rewriter.cc?cl=cf82eea)  
[src/rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/rewriter.cc?cl=cf82eea)  
[test/mjsunit/regress/regress-2529.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-2529.js?cl=cf82eea)  
...  
  

---   

## **regress-4325.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/4325)**  
**[Commit: [field type tracking] Fix handling of cleared WeakCells](https://chromium.googlesource.com/v8/v8/+/afa60ff)**  
  
Date(Commit): Wed Sep 23 12:35:36 2015  
Code Review: [https://codereview.chromium.org/1361103002](https://codereview.chromium.org/1361103002)  
Regress: [mjsunit/regress/regress-4325.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4325.js)  
```javascript
function Inner() {
  this.p1 = 0;
  this.p2 = 3;
}

function Outer() {
  this.p3 = 0;
}

var i1 = new Inner();
var i2 = new Inner();
var o1 = new Outer();
o1.inner = i1;
i1.p1 = 0.5;
print(i2.p1);
gc();
i2.p2 = 0.5;
var o2 = new Outer();
o2.p3 = 0.5;
o2.inner = i2;
print(o1.p3);

function loader(o) {
  return o.inner.p2;
};
%PrepareFunctionForOptimization(loader);
loader(o2);
loader(o2);
%OptimizeFunctionOnNextCall(loader);
assertEquals(0.5, loader(o2));
assertEquals(3, loader(o1));
gc();  // Crashes with --verify-heap.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/afa60ff^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=afa60ff)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=afa60ff)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=afa60ff)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=afa60ff)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=afa60ff)  
...  
  

---   

## **regress-4173.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/4173)**  
**[Commit: Share literals arrays per <NativeContext, SharedFunctionInfo> pair.](https://chromium.googlesource.com/v8/v8/+/4dd45e1)**  
  
Date(Commit): Wed Sep 23 08:46:28 2015  
Code Review: [https://codereview.chromium.org/1353363002](https://codereview.chromium.org/1353363002)  
Regress: [mjsunit/regress/regress-4173.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4173.js)  
```javascript
function Migrator(o) {
  return o.foo;
};
%PrepareFunctionForOptimization(Migrator);
function Loader(o) {
  return o[0];
};
%PrepareFunctionForOptimization(Loader);
var first_smi_array = [1];
var second_smi_array = [2];
var first_object_array = ["first"];
var second_object_array = ["string"];

assertTrue(%HasSmiElements(first_smi_array));
assertTrue(%HasSmiElements(second_smi_array));
assertTrue(%HasObjectElements(first_object_array));
assertTrue(%HasObjectElements(second_object_array));

first_smi_array.foo = 0;
second_smi_array.foo = 0;
first_object_array.foo = 0;
second_object_array.foo = 0;

for (var i = 0; i < 3; i++) Migrator(second_object_array);

first_smi_array.foo = 0.5;
print(second_smi_array.foo);

first_object_array.foo = 0.5;
%OptimizeFunctionOnNextCall(Migrator);
Migrator(second_object_array);


for (var i = 0; i < 3; i++) Loader(second_smi_array);
%OptimizeFunctionOnNextCall(Loader);
assertEquals("string", Loader(second_object_array));

assertTrue(%HasObjectElements(second_object_array));
assertFalse(%HasSmiElements(second_object_array));
assertTrue(%HaveSameMap(first_object_array, second_object_array));
assertFalse(%HaveSameMap(first_smi_array, second_object_array));

%ClearFunctionFeedback(Loader);
%ClearFunctionFeedback(Migrator);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4dd45e1^!)  
[src/code-stubs-hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/code-stubs-hydrogen.cc?cl=4dd45e1)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=4dd45e1)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=4dd45e1)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4dd45e1)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=4dd45e1)  
...  
  

---   

## **regress-4400.js (v8 issue)**  
   
**[Issue: Default parameters segfault when lazily parsed](https://crbug.com/v8/4400)**  
**[Commit: Don't crash when preparsing destructured arguments](https://chromium.googlesource.com/v8/v8/+/7485da7)**  
  
Date(Commit): Tue Sep 22 17:43:43 2015  
Code Review: [https://codereview.chromium.org/1350913005](https://codereview.chromium.org/1350913005)  
Regress: [mjsunit/es6/regress/regress-4400.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4400.js)  
```javascript
function borked(a = [], b = {}, c) {}
borked();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7485da7^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=7485da7)  
[test/mjsunit/harmony/regress/regress-4400.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4400.js?cl=7485da7)  
  

---   

## **regress-4417.js (v8 issue)**  
   
**[Issue: Spread operator in Object literal results in empty array](https://crbug.com/v8/4417)**  
**[Commit: Fix spread operator in ArrayLiterals when nested in other literals](https://chromium.googlesource.com/v8/v8/+/f44efd6)**  
  
Date(Commit): Tue Sep 15 16:43:39 2015  
Code Review: [https://codereview.chromium.org/1336123002](https://codereview.chromium.org/1336123002)  
Regress: [mjsunit/es6/regress/regress-4417.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4417.js)  
```javascript
var arr = [1, 2, 3];
assertEquals({arr: [1, 2, 3]}, {arr: [...arr]});
assertEquals([[1, 2, 3]], [[...arr]]);

assertEquals({arr: [6, 5, [1, 2, 3]]}, {arr: [6, 5, [...arr]]});
assertEquals([8, 7, [6, 5, [1, 2, 3]]], [8, 7, [6, 5, [...arr]]]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f44efd6^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=f44efd6)  
[test/mjsunit/harmony/regress/regress-4417.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4417.js?cl=f44efd6)  
  

---   

## **regress-crbug-530598.js (chromium issue)**  
   
**[Issue: 0u != values.size() in src/compiler/js-inlining.cc](https://crbug.com/530598)**  
**[Commit: [turbofan] Fix JSInliner to handle non-returning bodies.](https://chromium.googlesource.com/v8/v8/+/9e47ec6)**  
  
Date(Commit): Tue Sep 15 11:19:23 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1333193005](https://codereview.chromium.org/1333193005)  
Regress: [mjsunit/regress/regress-crbug-530598.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-530598.js)  
```javascript
var f1 = (function() {
  "use asm";
  function g() { throw 0; }
  function f() { return g(); }
  return f;
})();
%PrepareFunctionForOptimization(f1);
assertThrows("f1()");
%OptimizeFunctionOnNextCall(f1);
assertThrows("f1()");

var f2 = (function() {
  "use asm";
  function g() { for (;;); }
  function f(a) { return a || g(); }
  return f;
})();
%PrepareFunctionForOptimization(f2);
assertTrue(f2(true));
%OptimizeFunctionOnNextCall(f2);
assertTrue(f2(true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9e47ec6^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=9e47ec6)  
[test/mjsunit/regress/regress-crbug-530598.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-530598.js?cl=9e47ec6)  
  

---   

## **regress-4380.js (v8 issue)**  
   
**[Issue: 'Array' is slower than 'new Array'](https://crbug.com/v8/4380)**  
**[Commit: Crankshaft: consolidated element loads always deopted on seeing the hole](https://chromium.googlesource.com/v8/v8/+/164f92d)**  
  
Date(Commit): Wed Sep 09 15:15:30 2015  
Code Review: [https://codereview.chromium.org/1329793003](https://codereview.chromium.org/1329793003)  
Regress: [mjsunit/regress/regress-4380.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4380.js)  
```javascript
function bar(a) {
  var x = a[0];
  return x == undefined;
}

%PrepareFunctionForOptimization(bar);
bar([, 2, 3]);
bar([, 'two', 'three']);
bar([, 2, 3]);

%OptimizeFunctionOnNextCall(bar);
bar([, 2, 3]);
assertOptimized(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/164f92d^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=164f92d)  
[test/mjsunit/regress/regress-4380.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4380.js?cl=164f92d)  
  

---   

## **regress-4374.js (v8 issue)**  
   
**[Issue: Math.max inside asm.js produces the wrong result with  --turbo_inlining](https://crbug.com/v8/4374)**  
**[Commit: [turbofan] Make %Arguments composable with inlining.](https://chromium.googlesource.com/v8/v8/+/a504a18)**  
  
Date(Commit): Wed Sep 09 14:14:18 2015  
Code Review: [https://codereview.chromium.org/1328363002](https://codereview.chromium.org/1328363002)  
Regress: [mjsunit/regress/regress-4374.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4374.js)  
```javascript
var f = (function() {
  var max = Math.max;
  return function f() { return max(0, -1); };
})();
%PrepareFunctionForOptimization(f);

assertEquals(0, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(0, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a504a18^!)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=a504a18)  
[src/runtime/runtime-classes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-classes.cc?cl=a504a18)  
[src/runtime/runtime-function.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-function.cc?cl=a504a18)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=a504a18)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=a504a18)  
...  
  

---   

## **regress-crbug-527364.js (chromium issue)**  
   
**[Issue: !isolate->has_pending_exception() in src/compiler.cc](https://crbug.com/527364)**  
**[Commit: [turbofan] Handle stack overflow exceptions in JSInliner.](https://chromium.googlesource.com/v8/v8/+/c505907)**  
  
Date(Commit): Wed Sep 09 10:24:31 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1322203005](https://codereview.chromium.org/1322203005)  
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
%PrepareFunctionForOptimization(boom);
%OptimizeFunctionOnNextCall(boom)
run_close_to_stack_limit(boom);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c505907^!)  
[src/compiler/js-inlining.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-inlining.cc?cl=c505907)  
[test/mjsunit/regress/regress-crbug-527364.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-527364.js?cl=c505907)  
  

---   

## **regress-4266.js (v8 issue)**  
   
**[Issue: X is not a function error messages with TurboFan. ](https://crbug.com/v8/4266)**  
**[Commit: Use baseline code to compute message locations.](https://chromium.googlesource.com/v8/v8/+/819b40a)**  
  
Date(Commit): Tue Sep 08 14:14:59 2015  
Code Review: [https://codereview.chromium.org/1331603002](https://codereview.chromium.org/1331603002)  
Regress: [mjsunit/regress/regress-4266.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4266.js)  
```javascript
function test() {
  try {
    [].foo();
  } catch (e) {
    return e.message;
  }
};
%PrepareFunctionForOptimization(test);
assertEquals("[].foo is not a function", test());
%OptimizeFunctionOnNextCall(test);
assertEquals("[].foo is not a function", test());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/819b40a^!)  
[src/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast-numbering.cc?cl=819b40a)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=819b40a)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=819b40a)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=819b40a)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=819b40a)  
...  
  

---   

## **regress-508074.js (chromium issue)**  
   
**[Issue: Rest parameters for arrow functions do not fully work](https://crbug.com/508074)**  
**[Commit: [es6] Re-implement rest parameters via desugaring.](https://chromium.googlesource.com/v8/v8/+/510baea)**  
  
Date(Commit): Wed Sep 02 21:11:05 2015  
Components: Blink>JavaScript>Compiler  
Labels: Hotlist-Recharge, merge-merged-4.5  
Code Review: [https://crrev.com/1235153006/](https://crrev.com/1235153006/)  
Regress: [mjsunit/es6/regress/regress-508074.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-508074.js)  
```javascript
var f = (a, b, ...c) => {
  print(a);
  print(b);
  print(c);
  assertEquals(6, a);
  assertEquals(5, b);
  assertEquals([4, 3, 2, 1], c);
};

function g() {
  f(6, 5, 4, 3, 2, 1);
};

%PrepareFunctionForOptimization(g);
g();
g();
g();

%OptimizeFunctionOnNextCall(g);
g();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/510baea^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=510baea)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=510baea)  
[src/ast-literal-reindexer.h](https://cs.chromium.org/chromium/src/v8/src/ast-literal-reindexer.h?cl=510baea)  
[src/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast-value-factory.h?cl=510baea)  
[src/bailout-reason.h](https://cs.chromium.org/chromium/src/v8/src/bailout-reason.h?cl=510baea)  
...  
  

---   

## **regress-crbug-523307.js (chromium issue)**  
   
**[Issue: !found in src/lithium-allocator.cc](https://crbug.com/523307)**  
**[Commit: [arm64] Don't try convert binary operation to shifted form when both operands are the same.](https://chromium.googlesource.com/v8/v8/+/85f6e16)**  
  
Date(Commit): Wed Sep 02 09:32:44 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1304923003](https://codereview.chromium.org/1304923003)  
Regress: [mjsunit/regress/regress-crbug-523307.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523307.js)  
```javascript
function f(x) {
  var c = x * x << 366;
  var a = c + c;
  return a;
};
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
f(1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/85f6e16^!)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=85f6e16)  
[test/mjsunit/regress/regress-crbug-523307.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523307.js?cl=85f6e16)  
  

---   

## **regress-4399-01.js (v8 issue)**  
   
**[Issue: Switch desugaring broke return value for eval](https://crbug.com/v8/4399)**  
**[Commit: Propagate switch statement value for 'eval'](https://chromium.googlesource.com/v8/v8/+/6773e29)**  
  
Date(Commit): Fri Aug 28 22:43:07 2015  
Code Review: [https://codereview.chromium.org/1309303006](https://codereview.chromium.org/1309303006)  
Regress: [mjsunit/regress/regress-4399-01.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4399-01.js), [mjsunit/regress/regress-4399-02.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4399-02.js)  
```javascript
assertEquals("foo", eval('switch(1) { case 1: "foo" }'));
assertEquals("foo", eval('{ switch(1) { case 1: "foo" } }'));
assertEquals("foo", eval('switch(1) { case 1: { "foo" } }'));
assertEquals("foo", eval('switch(1) { case 1: "foo"; break; case 2: "bar"; break }'));
assertEquals("bar", eval('switch(2) { case 1: "foo"; break; case 2: "bar"; break }'));
assertEquals("bar", eval('switch(1) { case 1: "foo"; case 2: "bar"; break }'));


assertEquals(undefined, eval('switch (1) {}'));
assertEquals(undefined, eval('switch (1) { case 1: {} }'));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6773e29^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=6773e29)  
[test/cctest/test-ast-expression-visitor.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-ast-expression-visitor.cc?cl=6773e29)  
[test/mjsunit/regress/regress-4399.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4399.js?cl=6773e29)  
  

---   

## **regress-3926.js (v8 issue)**  
   
**[Issue: Let/const in CaseBlock](https://crbug.com/v8/3926)**  
**[Commit: Ensure hole checks take place in switch statement scopes](https://chromium.googlesource.com/v8/v8/+/d6fb6de)**  
  
Date(Commit): Fri Aug 28 18:49:57 2015  
Code Review: [https://codereview.chromium.org/1312613003](https://codereview.chromium.org/1312613003)  
Regress: [mjsunit/regress/regress-3926.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3926.js)  
```javascript
function f(x) {
  var z;
  switch (x) {
    case 1:
      let y = 1;
    case 2:
      y = 2;
    case 3:
      z = y;
  }
  return z;
}
assertEquals(2, f(1));
assertThrows(function() {f(2)}, ReferenceError);
assertThrows(function() {f(3)}, ReferenceError);

assertThrows(function() {
  switch (1) {
    case 0:
      let x = 2;
    case 1:
    { // this block, plus the let below, adds another linear lexical scope
      let y = 3;
      x;
    }
  }
}, ReferenceError);


function g(x) {
  switch (x) {
    case 1:
      let z;
    case 2:
      return function() { z = 1; }
    case 3:
      return function() { return z; }
    case 4:
      return eval("z = 1");
    case 5:
      return eval("z");
  }
}

assertEquals(undefined, g(1)());
assertThrows(g(2), ReferenceError);
assertThrows(g(3), ReferenceError);
assertThrows(function () {g(4)}, ReferenceError);
assertThrows(function () {g(5)}, ReferenceError);


function h(x) {
  'use strict'
  switch (x) {
    case 1:
      let z;
    case 2:
      return function() { z = 1; }
    case 3:
      return function() { return z; }
    case 4:
      return eval("z = 1");
    case 5:
      return eval("z");
  }
}

assertEquals(undefined, h(1)());
assertThrows(h(2), ReferenceError);
assertThrows(h(3), ReferenceError);
assertThrows(function () {h(4)}, ReferenceError);
assertThrows(function () {h(5)}, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d6fb6de^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=d6fb6de)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=d6fb6de)  
[src/full-codegen/full-codegen.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.cc?cl=d6fb6de)  
[src/full-codegen/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen/full-codegen.h?cl=d6fb6de)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=d6fb6de)  
...  
  

---   

## **regress-4388.js (v8 issue)**  
   
**[Issue: TurboFan AST graph builder TDZ check for let assignment is borked](https://crbug.com/v8/4388)**  
**[Commit: [turbofan] Fix broken dynamic TDZ check for let and const.](https://chromium.googlesource.com/v8/v8/+/cbd4f5a)**  
  
Date(Commit): Wed Aug 26 09:53:11 2015  
Code Review: [https://codereview.chromium.org/1318693002](https://codereview.chromium.org/1318693002)  
Regress: [mjsunit/regress/regress-4388.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4388.js)  
```javascript
function test_hole_check_for_let(a) {
  'use strict';
  { switch (a) {
      case 0: let x;
      case 1: x = 9;
    }
  }
}
%PrepareFunctionForOptimization(test_hole_check_for_let);
assertDoesNotThrow("test_hole_check_for_let(0)");
assertThrows("test_hole_check_for_let(1)", ReferenceError);
%OptimizeFunctionOnNextCall(test_hole_check_for_let)
assertThrows("test_hole_check_for_let(1)", ReferenceError);

function test_hole_check_for_const(a) {
  'use strict';
  { switch (a) {
      case 0: const x = 3;
      case 1: x = 2;
    }
  }
}
%PrepareFunctionForOptimization(test_hole_check_for_const);
assertThrows("test_hole_check_for_const(0)", TypeError);
assertThrows("test_hole_check_for_const(1)", ReferenceError);
%OptimizeFunctionOnNextCall(test_hole_check_for_const)
assertThrows("test_hole_check_for_const(1)", ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cbd4f5a^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=cbd4f5a)  
[test/mjsunit/regress/regress-4388.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4388.js?cl=cbd4f5a)  
  

---   

## **regress-crbug-523213.js (chromium issue)**  
   
**[Issue: DescriptorArray::kNotFound != number in src/hydrogen.cc](https://crbug.com/523213)**  
**[Commit: Do not inline array resize operations for outdated prototype maps.](https://chromium.googlesource.com/v8/v8/+/590b3be)**  
  
Date(Commit): Wed Aug 26 09:37:53 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1313303002](https://codereview.chromium.org/1313303002)  
Regress: [mjsunit/regress/regress-crbug-523213.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-523213.js)  
```javascript
var v1 = [];
var v2 = [];
v1.__proto__ = v2;

function f() {
  var a = [];
  for (var i = 0; i < 2; i++) {
    a.push([]);
    a = v2;
  }
};
%PrepareFunctionForOptimization(f);
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/590b3be^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=590b3be)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=590b3be)  
[test/mjsunit/regress/regress-crbug-523213.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-523213.js?cl=590b3be)  
  

---   

## **regress-4389-1.js (v8 issue)**  
   
**[Issue: Crankshaft optimization of inlined Math operators generates wrong code](https://crbug.com/v8/4389)**  
**[Commit: [crankshaft] DCE must not eliminate (observable) math operations.](https://chromium.googlesource.com/v8/v8/+/fef38c2)**  
  
Date(Commit): Tue Aug 25 06:24:55 2015  
Code Review: [https://codereview.chromium.org/1307413003](https://codereview.chromium.org/1307413003)  
Regress: [mjsunit/compiler/regress-4389-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-1.js), [mjsunit/compiler/regress-4389-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-2.js), [mjsunit/compiler/regress-4389-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-3.js), [mjsunit/compiler/regress-4389-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-4.js), [mjsunit/compiler/regress-4389-5.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-5.js), [mjsunit/compiler/regress-4389-6.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4389-6.js)  
```javascript
function foo(x) { Math.fround(x); }
%PrepareFunctionForOptimization(foo);
foo(1);
foo(2);
%OptimizeFunctionOnNextCall(foo);
assertThrows(function() { foo(Symbol()) }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fef38c2^!)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=fef38c2)  
[test/mjsunit/compiler/regress-4389-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4389-1.js?cl=fef38c2)  
[test/mjsunit/compiler/regress-4389-2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4389-2.js?cl=fef38c2)  
[test/mjsunit/compiler/regress-4389-3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4389-3.js?cl=fef38c2)  
[test/mjsunit/compiler/regress-4389-4.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4389-4.js?cl=fef38c2)  
...  
  

---   

## **regress-4376-1.js (v8 issue)**  
   
**[Issue: Optimization for instanceof with known global is unsound](https://crbug.com/v8/4376)**  
**[Commit: Correctify instanceof and make it optimizable.](https://chromium.googlesource.com/v8/v8/+/5d875a5)**  
  
Date(Commit): Tue Aug 25 04:48:54 2015  
Code Review: [https://codereview.chromium.org/1304633002](https://codereview.chromium.org/1304633002)  
Regress: [mjsunit/regress/regress-4376-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4376-1.js), [mjsunit/regress/regress-4376-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4376-2.js), [mjsunit/regress/regress-4376-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4376-3.js)  
```javascript
function Bar() { }
function Baz() { }
Baz.prototype = { __proto__: new Bar() }
var x = new Baz();
function foo(y) { return y instanceof Bar; }
assertTrue(foo(x));
Baz.prototype.__proto__ = null;
assertFalse(foo(x));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5d875a5^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=5d875a5)  
[src/arm/interface-descriptors-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/interface-descriptors-arm.cc?cl=5d875a5)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=5d875a5)  
[src/arm/lithium-arm.h](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.h?cl=5d875a5)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=5d875a5)  
...  
  

---   

## **regress-crbug-522380.js (chromium issue)**  
   
**[Issue: Stack-overflow in v8::base::Mutex::Lock](https://crbug.com/522380)**  
**[Commit: Make Simulator respect C stack limits as well.](https://chromium.googlesource.com/v8/v8/+/7fb31bd)**  
  
Date(Commit): Mon Aug 24 15:55:40 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Stability-Memory-AddressSanitizer, merge-merged-4.6, M-46, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1314623002](https://codereview.chromium.org/1314623002)  
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
...  
  

---   

## **regress-crbug-523308.js (chromium issue)**  
   
**[Issue: IsFound() in src/lookup.h](https://crbug.com/523308)**  
**[Commit: Message formatting: handle unexpected case of failing property lookup.](https://chromium.googlesource.com/v8/v8/+/2454469)**  
  
Date(Commit): Mon Aug 24 13:40:27 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, merge-merged-4.6, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1314543004](https://codereview.chromium.org/1314543004)  
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
  

---   

## **regress-520029.js (chromium issue)**  
   
**[Issue: Functions in blocks that refer to local lexically-scoped variables don't work](https://crbug.com/520029)**  
**[Commit: Fix function scoping issue](https://chromium.googlesource.com/v8/v8/+/9c79e69)**  
  
Date(Commit): Sat Aug 22 00:18:23 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1303033003](https://codereview.chromium.org/1303033003)  
Regress: [mjsunit/regress/regress-520029.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-520029.js)  
```javascript
function f(one) { class x { } { class x { } function g() { one; x; } g() } } f()

function g() { var x = 1; { let x = 2; function g() { x; } g(); } }
assertEquals(undefined, g());

function __f_4(one) {
  var __v_10 = one + 1;
  {
    let __v_10 = one + 3;
    function __f_6() {
 one;
 __v_10;
    }
    __f_6();
  }
}
__f_4();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9c79e69^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=9c79e69)  
[test/mjsunit/regress/regress-520029.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-520029.js?cl=9c79e69)  
  

---   

## **regress-4377.js (v8 issue)**  
   
**[Issue: Switch should introduce a scope](https://crbug.com/v8/4377)**  
**[Commit: Add a separate scope for switch](https://chromium.googlesource.com/v8/v8/+/9edbc1f)**  
  
Date(Commit): Fri Aug 21 23:54:36 2015  
Code Review: [https://codereview.chromium.org/1293283002](https://codereview.chromium.org/1293283002)  
Regress: [mjsunit/regress/regress-4377.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4377.js)  
```javascript
'use strict';

switch (1) { case 1: let x = 2; }

assertThrows(function() { return x; }, ReferenceError);

{
  let result;
  let x = 1;
  switch (x) {
    case 1:
      let x = 2;
      result = x;
      break;
    default:
      result = 0;
      break;
  }
  assertEquals(1, x);
  assertEquals(2, result);
}

{
  let result;
  let x = 1;
  switch (eval('x')) {
    case 1:
      let x = 2;
      result = x;
      break;
    default:
      result = 0;
      break;
  }
  assertEquals(1, x);
  assertEquals(2, result);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9edbc1f^!)  
[src/ast-value-factory.h](https://cs.chromium.org/chromium/src/v8/src/ast-value-factory.h?cl=9edbc1f)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=9edbc1f)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=9edbc1f)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=9edbc1f)  
[test/mjsunit/regress/regress-4377.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4377.js?cl=9edbc1f)  
...  
  

---   

## **regress-4211.js (v8 issue)**  
   
**[Issue: Wrong precedence for arrow function without parameters](https://crbug.com/v8/4211)**  
**[Commit: Parse arrow functions at proper precedence level](https://chromium.googlesource.com/v8/v8/+/9271b0c)**  
  
Date(Commit): Fri Aug 21 11:33:42 2015  
Code Review: [https://codereview.chromium.org/1286383005](https://codereview.chromium.org/1286383005)  
Regress: [mjsunit/es6/regress/regress-4211.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4211.js)  
```javascript
assertThrows("()=>{}()", SyntaxError);
assertThrows("x=>{}()", SyntaxError);
assertThrows("(...x)=>{}()", SyntaxError);
assertThrows("(x)=>{}()", SyntaxError);
assertThrows("(x,y)=>{}()", SyntaxError);
assertThrows("(x,y,...z)=>{}()", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9271b0c^!)  
[src/ast-literal-reindexer.cc](https://cs.chromium.org/chromium/src/v8/src/ast-literal-reindexer.cc?cl=9271b0c)  
[src/ast-numbering.cc](https://cs.chromium.org/chromium/src/v8/src/ast-numbering.cc?cl=9271b0c)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=9271b0c)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=9271b0c)  
[src/compiler/ast-loop-assignment-analyzer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-loop-assignment-analyzer.cc?cl=9271b0c)  
...  
  

---   

## **regress-crbug-522895.js (chromium issue)**  
   
**[Issue: old_target == new_target in src/objects-debug.cc](https://crbug.com/522895)**  
**[Commit: Fix bug in Code::VerifyRecompiledCode.](https://chromium.googlesource.com/v8/v8/+/a683f83)**  
  
Date(Commit): Thu Aug 20 17:20:02 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1300363002](https://codereview.chromium.org/1300363002)  
Regress: [mjsunit/regress/regress-crbug-522895.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-522895.js)  
```javascript
var body =
  "function bar1(  )  {" +
  "  var i = 35;       " +
  "  while (i-- > 31) {" +
  "    %OptimizeOsr(); " +
  "    j = 9;          " +
  "    %PrepareFunctionForOptimization(bar1); " +
  "    while (j-- > 7);" +
  "  }                 " +
  "  return i;         " +
  "}";

function gen() {
  return eval("(" + body + ")");
}

var f = gen();
%PrepareFunctionForOptimization(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a683f83^!)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=a683f83)  
[test/mjsunit/regress/regress-crbug-522895.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-522895.js?cl=a683f83)  
  

---   

## **regress-crbug-522496.js (chromium issue)**  
   
**[Issue: data != NULL in src/api.cc](https://crbug.com/522496)**  
**[Commit: [api] Relax CHECK for ArrayBuffer API abuse](https://chromium.googlesource.com/v8/v8/+/de26ce0)**  
  
Date(Commit): Wed Aug 19 21:53:17 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, merge-merged-46, M-46, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1302803003](https://codereview.chromium.org/1302803003)  
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
  

---   

## **regress-455207.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/arm/assembler-arm.cc,](https://crbug.com/455207)**  
**[Commit: Fix variable decl register collision on ARM.](https://chromium.googlesource.com/v8/v8/+/bb86937)**  
  
Date(Commit): Wed Aug 19 12:50:14 2015  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1040703003](https://codereview.chromium.org/1040703003)  
Regress: [mjsunit/regress/regress-455207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-455207.js)  
```javascript
"use strict";
var s = "";
for (var i = 16; i < 1085; i++) {
  s += ("var a" + i + " = " + i + ";");
}
s += "const x = 10;" +
    "assertEquals(10, x); x = 11; assertEquals(11, x)";
assertThrows(function() { eval(s); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb86937^!)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=bb86937)  
[test/mjsunit/regress/regress-455207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-455207.js?cl=bb86937)  
  

---   

## **regress-crbug-513472.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (object->IsJSGlobalObject()) in src/o](https://crbug.com/513472)**  
**[Commit: Rewrite Error.prototype.toString in C++.](https://chromium.googlesource.com/v8/v8/+/2e2765a)**  
  
Date(Commit): Tue Aug 11 09:15:41 2015  
Components: Blink>JavaScript  
Labels: Hotlist-Recharge, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1281833002](https://codereview.chromium.org/1281833002)  
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
...  
  

---   

## **regress-crbug-518747.js (chromium issue)**  
   
**[Issue: Fatal error in v8::Object::SetAlignedPointerInInternalField](https://crbug.com/518747)**  
**[Commit: [d8 Workers] Make Worker prototype read-only](https://chromium.googlesource.com/v8/v8/+/cd92934)**  
  
Date(Commit): Tue Aug 11 00:17:13 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1277543006](https://codereview.chromium.org/1277543006)  
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
  

---   

## **regress-517455.js (chromium issue)**  
   
**[Issue: context_index >= 0 in src/runtime/runtime-scopes.cc](https://crbug.com/517455)**  
**[Commit: Regression test for crbug 517455](https://chromium.googlesource.com/v8/v8/+/651f55c)**  
  
Date(Commit): Fri Aug 07 13:32:46 2015  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://chromium.googlesource.com/v8/v8/+/826f8da55fb868a365d047a4a653eb8ff2bfc14e](https://chromium.googlesource.com/v8/v8/+/826f8da55fb868a365d047a4a653eb8ff2bfc14e)  
Regress: [mjsunit/es6/regress/regress-517455.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-517455.js)  
```javascript
function f({x = ""}) { eval(x) }
f({})  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/651f55c^!)  
[test/mjsunit/harmony/regress/regress-517455.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-517455.js?cl=651f55c)  
  

---   

## **regress-crbug-516775.js (chromium issue)**  
   
**[Issue: IsNumber() in src/objects-inl.h:1105](https://crbug.com/516775)**  
**[Commit: Fix Array.prototype.concat for arguments object with getter.](https://chromium.googlesource.com/v8/v8/+/2e0d55a)**  
  
Date(Commit): Thu Aug 06 10:28:36 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1270403002](https://codereview.chromium.org/1270403002)  
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
  

---   

## **regress-crbug-516592.js (chromium issue)**  
   
**[Issue: kMaxUInt32 != index_ in src/lookup.h:110](https://crbug.com/516592)**  
**[Commit: Fix off-by-one in Array.concat's max index check](https://chromium.googlesource.com/v8/v8/+/087ae1b)**  
  
Date(Commit): Thu Aug 06 09:57:19 2015  
Components: Blink>JavaScript  
Labels: Stability-Crash, Reproducible, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1278703003](https://codereview.chromium.org/1278703003)  
Regress: [mjsunit/regress/regress-crbug-516592.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-516592.js)  
```javascript
var i = Math.pow(2, 31);
var a = [];
a[i] = 31;
var b = [];
b[i - 2] = 33;
try {
  // This is supposed to throw a RangeError.
  var c = a.concat(b);
  // If it didn't, ObservableSetLength will detect the problem.
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
  

---   

## **regress-crbug-513507.js (chromium issue)**  
   
**[Issue: V8: Check failed: new_code_map->get(i + kContextOffset)->IsNativeContext().](https://crbug.com/513507)**  
**[Commit: Introduce safe interface to "copy and grow" FixedArray.](https://chromium.googlesource.com/v8/v8/+/bcad9b5)**  
  
Date(Commit): Tue Aug 04 17:49:42 2015  
Components: Internals>GPU>Testing, Blink>JavaScript  
Labels: M-46, ReleaseBlock-Stable  
Code Review: [https://codereview.chromium.org/1255173006](https://codereview.chromium.org/1255173006)  
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
  %PrepareFunctionForOptimization(fun);
  return fun;
}
%PrepareFunctionForOptimization(makeFun);

makeFun()(7);  // Warm up.
makeFun()(4);  // Optimize once.
makeFun()(1);  // Optimize again.  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bcad9b5^!)  
[src/factory.cc](https://cs.chromium.org/chromium/src/v8/src/factory.cc?cl=bcad9b5)  
[src/factory.h](https://cs.chromium.org/chromium/src/v8/src/factory.h?cl=bcad9b5)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=bcad9b5)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=bcad9b5)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=bcad9b5)  
...  
  

---   

## **regress-crbug-514081.js (chromium issue)**  
   
**[Issue: IsNumber() in src/objects-inl.h:1117](https://crbug.com/514081)**  
**[Commit: [d8 worker] Fix regression when serializing very large arraybuffer](https://chromium.googlesource.com/v8/v8/+/df1f72b)**  
  
Date(Commit): Mon Aug 03 17:08:00 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1264723002](https://codereview.chromium.org/1264723002)  
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
    // postMessage failed, should be a DataCloneError message.
    assertContains('cloned', e.message);
    threw = true;
  }
  assertTrue(threw, 'Should throw when trying to serialize large message.');
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df1f72b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=df1f72b)  
[test/mjsunit/regress/regress-crbug-514081.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-514081.js?cl=df1f72b)  
  

---   

## **regress-crbug-511880.js (chromium issue)**  
   
**[Issue: Fatal error in v8::External::Cast](https://crbug.com/511880)**  
**[Commit: [d8 Workers] Fix bug creating Worker during main thread termination](https://chromium.googlesource.com/v8/v8/+/a87db3d)**  
  
Date(Commit): Thu Jul 30 08:19:39 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1255563002](https://codereview.chromium.org/1255563002)  
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
  

---   

## **regress-crbug-513602.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/513602)**  
**[Commit: Fix prototype registration upon SlowToFast migration](https://chromium.googlesource.com/v8/v8/+/c906efd)**  
  
Date(Commit): Tue Jul 28 15:41:29 2015  
Components: Blink>JavaScript>Runtime  
Labels: Release-0-M45, Stability-Crash, M-45, M-44, reward-0, merge-merged-4.4, merge-merged-4.5, Security_Impact-Stable, allpublic  
Code Review: [https://codereview.chromium.org/1263543004](https://codereview.chromium.org/1263543004)  
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
  

---   

## **regress-513474.js (chromium issue)**  
   
**[Issue: declaration_scope->is_function_scope() in src/arm/full-codegen-arm.cc:5265](https://crbug.com/513474)**  
**[Commit: Find right scope associated with prologue](https://chromium.googlesource.com/v8/v8/+/3e40b64)**  
  
Date(Commit): Fri Jul 24 13:08:32 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1250423002](https://codereview.chromium.org/1250423002)  
Regress: [mjsunit/es6/regress/regress-513474.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-513474.js)  
```javascript
(function(...a) { function f() { eval() } })();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3e40b64^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=3e40b64)  
[src/full-codegen/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm/full-codegen-arm.cc?cl=3e40b64)  
[src/full-codegen/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/arm64/full-codegen-arm64.cc?cl=3e40b64)  
[src/full-codegen/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/ia32/full-codegen-ia32.cc?cl=3e40b64)  
[src/full-codegen/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/full-codegen/mips/full-codegen-mips.cc?cl=3e40b64)  
...  
  

---   

## **regress-cr512574.js (chromium issue)**  
   
**[Issue: temps_.is_empty() in src/scopes.cc:348](https://crbug.com/512574)**  
**[Commit: [es6] Make sure temporaries are not allocated in block scope](https://chromium.googlesource.com/v8/v8/+/9ab8bfb)**  
  
Date(Commit): Thu Jul 23 13:51:35 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1250513004](https://codereview.chromium.org/1250513004)  
Regress: [mjsunit/es6/regress/regress-cr512574.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-cr512574.js)  
```javascript
function f({}) {
  for (var v in []);
};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9ab8bfb^!)  
[src/contexts.cc](https://cs.chromium.org/chromium/src/v8/src/contexts.cc?cl=9ab8bfb)  
[src/globals.h](https://cs.chromium.org/chromium/src/v8/src/globals.h?cl=9ab8bfb)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=9ab8bfb)  
[src/pattern-rewriter.cc](https://cs.chromium.org/chromium/src/v8/src/pattern-rewriter.cc?cl=9ab8bfb)  
[src/runtime/runtime-scopes.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-scopes.cc?cl=9ab8bfb)  
...  
  

---   

## **regress-crbug-510426.js (chromium issue)**  
   
**[Issue: ContainsOnlyValidKeys(content) in src/objects.cc:6159](https://crbug.com/510426)**  
**[Commit: Fix element enumeration on String wrappers with dictionary elements](https://chromium.googlesource.com/v8/v8/+/e6cb6bb)**  
  
Date(Commit): Mon Jul 20 09:01:06 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1246513002](https://codereview.chromium.org/1246513002)  
Regress: [mjsunit/regress/regress-crbug-510426.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-510426.js)  
```javascript
var s = new String('a');
s[10000000] = 'bente';
assertEquals(['0', '10000000'], Object.keys(s));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e6cb6bb^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=e6cb6bb)  
[test/mjsunit/regress/regress-crbug-510426.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-510426.js?cl=e6cb6bb)  
  

---   

## **regress-crbug-510738.js (chromium issue)**  
   
**[Issue: length() >= kReservedIndexCount in src/type-feedback-vector.h:102](https://crbug.com/510738)**  
**[Commit: Crankshaft part of the 'loads and stores to global vars through property cell shortcuts' feature.](https://chromium.googlesource.com/v8/v8/+/cc66a1c)**  
  
Date(Commit): Mon Jul 20 08:49:28 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1228113008](https://codereview.chromium.org/1228113008)  
Regress: [mjsunit/regress/regress-crbug-510738.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-510738.js)  
```javascript
function check(f, result) {
  %PrepareFunctionForOptimization(f);
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
...  
  

---   

## **regress-4298.js (v8 issue)**  
   
**[Issue: Invalid result of the spread operator](https://crbug.com/v8/4298)**  
**[Commit: Fix spread array inside array literal](https://chromium.googlesource.com/v8/v8/+/24e9828)**  
  
Date(Commit): Wed Jul 15 15:16:13 2015  
Code Review: [https://codereview.chromium.org/1225223004](https://codereview.chromium.org/1225223004)  
Regress: [mjsunit/es6/regress/regress-4298.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4298.js)  
```javascript
var arr = [1, 2, ...[3]];
assertEquals([1, 2, 3], arr);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/24e9828^!)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=24e9828)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=24e9828)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=24e9828)  
[test/mjsunit/harmony/regress/regress-4298.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4298.js?cl=24e9828)  
  

---   

## **regress-503565.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/503565)**  
**[Commit: Scoping error caused crash in CallICNexus::StateFromFeedback](https://chromium.googlesource.com/v8/v8/+/ae11f20)**  
  
Date(Commit): Wed Jul 15 09:15:05 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1231343003](https://codereview.chromium.org/1231343003)  
Regress: [mjsunit/regress/regress-503565.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-503565.js)  
```javascript
function f() {}
function g() {}
function h() {
    g()
}
(function() {
    eval("\
        \"use strict\";\
        g = (function(x) {\
            +Math.log(+Math.log((+(+x>0)), f(Math.log())))\
        })\
    ")
})()
for (var j = 0; j < 999; j++) {
    h()
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ae11f20^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=ae11f20)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=ae11f20)  
[src/preparser.cc](https://cs.chromium.org/chromium/src/v8/src/preparser.cc?cl=ae11f20)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=ae11f20)  
[test/mjsunit/regress/regress-491536.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-491536.js?cl=ae11f20)  
...  
  

---   

## **regress-509961.js (chromium issue)**  
   
**[Issue: !name->AsArrayIndex(&index) in src/lookup.h:65](https://crbug.com/509961)**  
**[Commit: Properly handle missing from normalized stores with keys convertible to array indices](https://chromium.googlesource.com/v8/v8/+/5f24690)**  
  
Date(Commit): Tue Jul 14 11:44:56 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1241613003](https://codereview.chromium.org/1241613003)  
Regress: [mjsunit/regress/regress-509961.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-509961.js)  
```javascript
var o = { x: 0 };
delete o.x;
function store(o, p, v) { o[p] = v; }
store(o, "x", 1);
store(o, "x", 1);
store(o, "0", 1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f24690^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=5f24690)  
[test/mjsunit/regress/regress-509961.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-509961.js?cl=5f24690)  
  

---   

## **regress-4296.js (v8 issue)**  
   
**[Issue: Element access broken in ICs (probably also optimizing compilers) when string wrappers are involved.](https://crbug.com/v8/4296)**  
**[Commit: Fix keyed element access wrt string wrappers](https://chromium.googlesource.com/v8/v8/+/01f40e6)**  
  
Date(Commit): Mon Jul 13 15:39:07 2015  
Code Review: [https://codereview.chromium.org/1228063004](https://codereview.chromium.org/1228063004)  
Regress: [mjsunit/regress/regress-4296.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4296.js)  
```javascript
(function () {
  var o = new String("ab");
  function store(o, i, v) { o[i] = v; }
  function load(o, i) { return o[i]; }

  // Initialize the IC.
  store(o, 2, 10);
  load(o, 2);

  store(o, 0, 100);
  assertEquals("a", load(o, 0));
})();

(function () {
  var o = {__proto__: new String("ab")};
  function store(o, i, v) { o[i] = v; }
  function load(o, i) { return o[i]; }

  // Initialize the IC.
  store(o, 2, 10);
  load(o, 2);

  store(o, 0, 100);
  assertEquals("a", load(o, 0));
})();

(function () {
  "use strict";
  var o = {__proto__: {}};
  function store(o, i, v) { o[i] = v; }

  // Initialize the IC.
  store(o, 0, 100);
  o.__proto__.__proto__ = new String("bla");
  assertThrows(function () { store(o, 1, 100) });
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/01f40e6^!)  
[src/arm/macro-assembler-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/macro-assembler-arm.cc?cl=01f40e6)  
[src/arm64/macro-assembler-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/macro-assembler-arm64.cc?cl=01f40e6)  
[src/ia32/macro-assembler-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/macro-assembler-ia32.cc?cl=01f40e6)  
[src/ic/arm/ic-arm.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm/ic-arm.cc?cl=01f40e6)  
[src/ic/arm64/ic-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/ic/arm64/ic-arm64.cc?cl=01f40e6)  
...  
  

---   

## **regress-507980.js (chromium issue)**  
   
**[Issue: target_number_of_fields >= *old_number_of_fields in src/objects.cc:1733](https://crbug.com/507980)**  
**[Commit: Reload the map of typed arrays after performing ToNumber.](https://chromium.googlesource.com/v8/v8/+/0b3d6f7)**  
  
Date(Commit): Fri Jul 10 12:49:40 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.5, Hotlist-Merge-Approved, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1234553002](https://codereview.chromium.org/1234553002)  
Regress: [mjsunit/regress/regress-507980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-507980.js)  
```javascript
__v_1 = new Float64Array(1);
__v_8 = { valueOf: function() { __v_13.y = "bar"; return 42; }};
__v_13 = __v_1;
__v_13[0] = __v_8;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b3d6f7^!)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=0b3d6f7)  
[src/lookup.h](https://cs.chromium.org/chromium/src/v8/src/lookup.h?cl=0b3d6f7)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=0b3d6f7)  
[test/mjsunit/regress/regress-507980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-507980.js?cl=0b3d6f7)  
  

---   

## **regress-crbug-490021.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/lithium-codegen.cc,](https://crbug.com/490021)**  
**[Commit: [arm64] Fixed unnecessary environment assignment to LSmiTag instruction.](https://chromium.googlesource.com/v8/v8/+/b625d4d)**  
  
Date(Commit): Fri Jul 10 11:36:17 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1235563002](https://codereview.chromium.org/1235563002)  
Regress: [mjsunit/regress/regress-crbug-490021.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-490021.js)  
```javascript
var global = new Object(3);
function f() {
  global[0] = global[0] >>> 15.5;
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b625d4d^!)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=b625d4d)  
[test/mjsunit/regress/regress-crbug-490021.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-490021.js?cl=b625d4d)  
  

---   

## **regress-4279.js (v8 issue)**  
   
**[Issue: Check failed: 0 == result in v8::base::DestroyNativeHandle](https://crbug.com/v8/4279)**  
**[Commit: d8 workers: fix race on quit() with context_mutex_](https://chromium.googlesource.com/v8/v8/+/d42e81d)**  
  
Date(Commit): Thu Jul 09 19:30:29 2015  
Code Review: [https://codereview.chromium.org/1231473002](https://codereview.chromium.org/1231473002)  
Regress: [mjsunit/regress/regress-4279.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4279.js)  
```javascript
if (this.Worker && this.quit) {
  try {
      new Function(new Worker("55"), {type: 'string'});
  } catch(err) {}

  quit();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d42e81d^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=d42e81d)  
[test/mjsunit/regress/regress-4279.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4279.js?cl=d42e81d)  
  

---   

## **regress-crbug-506549.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (static_cast<unsigned>(i) < static_ca](https://crbug.com/506549)**  
**[Commit: Fix cluster-fuzz found regression with d8 Workers](https://chromium.googlesource.com/v8/v8/+/54920cd)**  
  
Date(Commit): Wed Jul 08 17:58:00 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1220193004](https://codereview.chromium.org/1220193004)  
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
  

---   

## **regress-crbug-506956.js (chromium issue)**  
   
**[Issue: !isolate->has_pending_exception() in src/compiler.cc:857](https://crbug.com/506956)**  
**[Commit: Fixed a couple of proxies-related unhandled exceptions.](https://chromium.googlesource.com/v8/v8/+/52b3e41)**  
  
Date(Commit): Wed Jul 08 11:46:14 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1215463012](https://codereview.chromium.org/1215463012)  
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
  

---   

## **regress-crbug-505907.js (chromium issue)**  
   
**[Issue: !isolate->has_pending_exception() in src/contexts.cc:273](https://crbug.com/505907)**  
**[Commit: Fixed a couple of proxies-related unhandled exceptions.](https://chromium.googlesource.com/v8/v8/+/52b3e41)**  
  
Date(Commit): Wed Jul 08 11:46:14 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1215463012](https://codereview.chromium.org/1215463012)  
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
  

---   

## **regress-crbug-478612.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/x64/lithium-codegen-x64.cc,](https://crbug.com/478612)**  
**[Commit: [x64] Fix handling of Smi constants in LSubI and LBitI](https://chromium.googlesource.com/v8/v8/+/5379d8b)**  
  
Date(Commit): Wed Jul 08 10:20:31 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.4, merge-merged-4.5, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1224623017](https://codereview.chromium.org/1224623017)  
Regress: [mjsunit/regress/regress-crbug-478612.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-478612.js)  
```javascript
var z = {
  valueOf: function() {
    return 3;
  }
};

function f() {
  var y = -2;
  return (1 & z) - y++;
};
%PrepareFunctionForOptimization(f);
assertEquals(3, f());
assertEquals(3, f());
%OptimizeFunctionOnNextCall(f);
assertEquals(3, f());


function g() {
  var y = 2;
  return 1 & z | y++;
};
%PrepareFunctionForOptimization(g);
assertEquals(3, g());
assertEquals(3, g());
%OptimizeFunctionOnNextCall(g);
assertEquals(3, g());


function h() {
  var y = 3;
  return 3 & z & y++;
};
%PrepareFunctionForOptimization(h);
assertEquals(3, h());
assertEquals(3, h());
%OptimizeFunctionOnNextCall(h);
assertEquals(3, h());


function i() {
  var y = 2;
  return 1 & z ^ y++;
};
%PrepareFunctionForOptimization(i);
assertEquals(3, i());
assertEquals(3, i());
%OptimizeFunctionOnNextCall(i);
assertEquals(3, i());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5379d8b^!)  
[src/x64/lithium-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/lithium-x64.cc?cl=5379d8b)  
[test/mjsunit/regress/regress-crbug-478612.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-478612.js?cl=5379d8b)  
  

---   

## **regress-4271.js (v8 issue)**  
   
**[Issue: Fatal error in v8::Object::GetInternalField](https://crbug.com/v8/4271)**  
**[Commit: [d8] bounds-check before getting Shell::Worker internal field](https://chromium.googlesource.com/v8/v8/+/43ce9c6)**  
  
Date(Commit): Tue Jul 07 21:06:19 2015  
Code Review: [https://codereview.chromium.org/1214053004](https://codereview.chromium.org/1214053004)  
Regress: [mjsunit/regress/regress-4271.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4271.js)  
```javascript
if (this.Worker) {
  // Throw rather than overflow internal field index
  assertThrows(function() {
    Worker.prototype.terminate();
  });

  assertThrows(function() {
    Worker.prototype.getMessage();
  });

  assertThrows(function() {
    Worker.prototype.postMessage({});
  });

  // Don't throw for real worker
  var worker = new Worker('', {type: 'string'});
  worker.getMessage();
  worker.postMessage({});
  worker.terminate();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/43ce9c6^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=43ce9c6)  
[test/mjsunit/regress/regress-4271.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4271.js?cl=43ce9c6)  
  

---   

## **regress-osr-context.js (other issue)**  
   
**[Commit: [turbofan] Fix bogus materialization from frame with OSR.](https://chromium.googlesource.com/v8/v8/+/b8ecd94)**  
  
Date(Commit): Mon Jul 06 03:40:29 2015  
Code Review: [https://codereview.chromium.org/1220013003](https://codereview.chromium.org/1220013003)  
Regress: [mjsunit/regress/regress-osr-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-osr-context.js)  
```javascript
(function() {
  "use strict";
  var a = 23;
  function f() {
    for (let i = 0; i < 5; ++i) {
      a--;  // Make sure {a} is non-immutable, hence context allocated.
      function g() { return i }  // Make sure block has a context.
      if (i == 2) %OptimizeOsr();
    }
    return a;
  }
  %PrepareFunctionForOptimization(f);
  assertEquals(18, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b8ecd94^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=b8ecd94)  
[test/mjsunit/regress/regress-osr-context.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-osr-context.js?cl=b8ecd94)  
  
  
---   

## **regress-shift-right-logical.js (other issue)**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-right-logical.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-right-logical.js)  
```javascript
(function ShiftRightLogicalWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 >>> tmp1;
  }

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();


(function ShiftRightLogicalWithCallUsage() {
  var f = (function() {
    "use asm"
    // This is not a valid asm.js, we use the "use asm" here to
    // trigger Turbofan without deoptimization support.

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 >>> tmp1, tmp1);
    }

    return f;
  })();

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=5f288c2)  
[src/compiler/js-typed-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.h?cl=5f288c2)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=5f288c2)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=5f288c2)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=5f288c2)  
...  
  
  
---   

## **regress-shift-right.js (other issue)**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-right.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-right.js)  
```javascript
(function ShiftRightWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 >> tmp1;
  }

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();


(function ShiftRightWithCallUsage() {
  var f = (function() {
    "use asm"
    // This is not a valid asm.js, we use the "use asm" here to
    // trigger Turbofan without deoptimization support.

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 >> tmp1, tmp1);
    }

    return f;
  })();

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(0, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=5f288c2)  
[src/compiler/js-typed-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.h?cl=5f288c2)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=5f288c2)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=5f288c2)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=5f288c2)  
...  
  
  
---   

## **regress-shift-left.js (other issue)**  
   
**[Commit: [turbofan] Right hand side of shifts needs ToUint32.](https://chromium.googlesource.com/v8/v8/+/5f288c2)**  
  
Date(Commit): Fri Jul 03 11:42:00 2015  
Code Review: [https://codereview.chromium.org/1213803008](https://codereview.chromium.org/1213803008)  
Regress: [mjsunit/compiler/regress-shift-left.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-shift-left.js)  
```javascript
(function ShiftLeftWithDeoptUsage() {
  function g() {}

  function f() {
    var tmp = 1264475713;
    var tmp1 = tmp - (-913041544);
    g();
    return 1 << tmp1;
  }

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(512, f());
})();


(function ShiftLeftWithCallUsage() {
  var f = (function() {
    "use asm"
    // This is not a valid asm.js, we use the "use asm" here to
    // trigger Turbofan without deoptimization support.

    function g(x) { return x; }

    function f() {
      var tmp = 1264475713;
      var tmp1 = tmp - (-913041544);
      return g(1 << tmp1, tmp1);
    }

    return f;
  })();

  %PrepareFunctionForOptimization(f);
  %OptimizeFunctionOnNextCall(f);
  assertEquals(512, f());
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f288c2^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=5f288c2)  
[src/compiler/js-typed-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.h?cl=5f288c2)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=5f288c2)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=5f288c2)  
[src/compiler/simplified-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.h?cl=5f288c2)  
...  
  
  
---   

## **regress-crbug-505778.js (chromium issue)**  
   
**[Issue: !v8::internal::FLAG_enable_slow_asserts || (static_cast<unsigned>(i) < static_ca](https://crbug.com/505778)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers](https://chromium.googlesource.com/v8/v8/+/abaa094)**  
  
Date(Commit): Tue Jun 30 16:49:09 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1216853003](https://codereview.chromium.org/1216853003)  
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
  

---   

## **regress-crbug-505370.js (chromium issue)**  
   
**[Issue: !name->AsArrayIndex(&index) in src/lookup.h:65](https://crbug.com/505370)**  
**[Commit: Use correct LookupIterator in CallSite::GetMethodName.](https://chromium.googlesource.com/v8/v8/+/4f9cf2b)**  
  
Date(Commit): Tue Jun 30 16:28:07 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1218023002](https://codereview.chromium.org/1218023002)  
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
  

---   

## **regress-crbug-505354.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Execution::ToObject](https://crbug.com/505354)**  
**[Commit: [turbofan] Fix exit control flow in TryCatchBuilder.](https://chromium.googlesource.com/v8/v8/+/df06f1c)**  
  
Date(Commit): Tue Jun 30 03:23:41 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, M-45, Stability-Memory-AddressSanitizer, findit-wrong, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1213183003](https://codereview.chromium.org/1213183003)  
Regress: [mjsunit/regress/regress-crbug-505354.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-505354.js)  
```javascript
function f() {
  "use strict";
  try {
    for (let i = 0; i < 10; i++) {}
  } catch (e) {
  }
};
%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/df06f1c^!)  
[src/compiler/control-builders.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-builders.cc?cl=df06f1c)  
[test/mjsunit/regress/regress-crbug-505354.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-505354.js?cl=df06f1c)  
  

---   

## **regress-4255-1.js (v8 issue)**  
   
**[Issue: Let variables are inconsistently allocated, causing crash on OSR.](https://crbug.com/v8/4255)**  
**[Commit: Parse eagerly inside block scopes.](https://chromium.googlesource.com/v8/v8/+/972beef)**  
  
Date(Commit): Mon Jun 29 16:16:21 2015  
Code Review: [https://codereview.chromium.org/1216013002](https://codereview.chromium.org/1216013002)  
Regress: [mjsunit/regress/regress-4255-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4255-1.js), [mjsunit/regress/regress-4255-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4255-2.js), [mjsunit/regress/regress-4255-3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4255-3.js), [mjsunit/regress/regress-4255-4.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4255-4.js)  
```javascript
'use strict';
{
  let x = function() {};
  // Trigger OSR.
  for (var i = 0; i < 1000000; i++);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/972beef^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=972beef)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=972beef)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=972beef)  
[src/scopes.h](https://cs.chromium.org/chromium/src/v8/src/scopes.h?cl=972beef)  
[test/mjsunit/regress/regress-4255-1.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4255-1.js?cl=972beef)  
...  
  

---   

## **regress-crbug-504729.js (chromium issue)**  
   
**[Issue: UNKNOWN in strlen](https://crbug.com/504729)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers.](https://chromium.googlesource.com/v8/v8/+/e291b78)**  
  
Date(Commit): Mon Jun 29 15:53:22 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1214803004](https://codereview.chromium.org/1214803004)  
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
  

---   

## **regress-crbug-504727.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Object::GetProperty](https://crbug.com/504727)**  
**[Commit: Fix cluster-fuzz found regression in d8 Workers.](https://chromium.googlesource.com/v8/v8/+/93c4352)**  
  
Date(Commit): Mon Jun 29 15:48:39 2015  
Components: Blink>JavaScript  
Labels: ReleaseBlock-Beta, Merge-na, M-45, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1219563002](https://codereview.chromium.org/1219563002)  
Regress: [mjsunit/regress/regress-crbug-504727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-504727.js)  
```javascript
if (this.Worker) {
  var __v_2 = new Worker('', {type: 'string'});
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/93c4352^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=93c4352)  
[test/mjsunit/regress/regress-crbug-504727.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-504727.js?cl=93c4352)  
  

---   

## **regress-crbug-504787.js (chromium issue)**  
   
**[Issue: script->FindSharedFunctionInfo(literal).is_null() in src/compiler.cc:1339](https://crbug.com/504787)**  
**[Commit: Mark function info as compiled after EnsureDeoptimizationSupport.](https://chromium.googlesource.com/v8/v8/+/8c72792)**  
  
Date(Commit): Fri Jun 26 13:17:05 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1209383002](https://codereview.chromium.org/1209383002)  
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
  

---   

## **regress-crbug-504136.js (chromium issue)**  
   
**[Issue: UNKNOWN in RtlEncodePointer+0x1eb](https://crbug.com/504136)**  
**[Commit: Fix cluster-fuzz regression when getting message from Worker](https://chromium.googlesource.com/v8/v8/+/28b0129)**  
  
Date(Commit): Thu Jun 25 18:01:22 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, M-45, Stability-Memory-AddressSanitizer, Findit-for-crash, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1208733002](https://codereview.chromium.org/1208733002)  
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
  

---   

## **regress-existing-shared-function-info.js (other issue)**  
   
**[Commit: Reland 2 "Keep a canonical list of shared function infos."](https://chromium.googlesource.com/v8/v8/+/6434ec3)**  
  
Date(Commit): Thu Jun 25 12:20:06 2015  
Code Review: [https://codereview.chromium.org/1211803002](https://codereview.chromium.org/1211803002)  
Regress: [mjsunit/regress/regress-existing-shared-function-info.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-existing-shared-function-info.js)  
```javascript
function f() {
  return function g() {
    return function h() {}
  }
}

var h = f()();

for (var i of Array(10)) gc();

f()();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/6434ec3^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=6434ec3)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=6434ec3)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=6434ec3)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=6434ec3)  
[src/bootstrapper.cc](https://cs.chromium.org/chromium/src/v8/src/bootstrapper.cc?cl=6434ec3)  
...  
  
  
---   

## **regress-crbug-503968.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::MemoryChunk::heap](https://crbug.com/503968)**  
**[Commit: Fix cluster-fuzz regression with Workers and recursive serialization](https://chromium.googlesource.com/v8/v8/+/5023335)**  
  
Date(Commit): Wed Jun 24 18:31:50 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1211433003](https://codereview.chromium.org/1211433003)  
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
  

---   

## **regress-crbug-503991.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/vector.h,](https://crbug.com/503991)**  
**[Commit: Fix cluster-fuzz regression with Workers when serializing empty string](https://chromium.googlesource.com/v8/v8/+/b3bd728)**  
  
Date(Commit): Wed Jun 24 17:47:23 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1210623002](https://codereview.chromium.org/1210623002)  
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
  

---   

## **regress-crbug-503698.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/isolate.h,](https://crbug.com/503698)**  
**[Commit: Fix cluster-fuzz regression with Workers on mips.debug](https://chromium.googlesource.com/v8/v8/+/627627b)**  
  
Date(Commit): Wed Jun 24 17:09:59 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1208573003](https://codereview.chromium.org/1208573003)  
Regress: [mjsunit/regress/regress-crbug-503698.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-503698.js)  
```javascript
if (this.Worker) {
  var __v_6 = new Worker('', {type: 'string'});
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/627627b^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=627627b)  
[test/mjsunit/regress/regress-crbug-503698.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-503698.js?cl=627627b)  
  

---   

## **regress-4214.js (v8 issue)**  
   
**[Issue: Wrong this-value when deleting property named "eval" in with-statement context](https://crbug.com/v8/4214)**  
**[Commit: Fix receiver when calling eval() bound by with scope](https://chromium.googlesource.com/v8/v8/+/3c5f0db)**  
  
Date(Commit): Wed Jun 24 16:47:58 2015  
Code Review: [https://codereview.chromium.org/1202963005](https://codereview.chromium.org/1202963005)  
Regress: [mjsunit/regress/regress-4214.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4214.js)  
```javascript
var o = { eval: function() { return this; } }
with (o) assertSame(o, eval());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3c5f0db^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=3c5f0db)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=3c5f0db)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=3c5f0db)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=3c5f0db)  
[src/full-codegen.h](https://cs.chromium.org/chromium/src/v8/src/full-codegen.h?cl=3c5f0db)  
...  
  

---   

## **regress-crbug-503578.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/d8.cc,](https://crbug.com/503578)**  
**[Commit: Fix cluster-fuzz found regression in d8 when deserializing ArrayBuffer](https://chromium.googlesource.com/v8/v8/+/10b6af7)**  
  
Date(Commit): Wed Jun 24 04:23:58 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1204753002](https://codereview.chromium.org/1204753002)  
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
  

---   

## **regress-crbug-501711.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/compiler.cc,](https://crbug.com/501711)**  
**[Commit: Fixed exception handling in Realm.create().](https://chromium.googlesource.com/v8/v8/+/bcb276c)**  
  
Date(Commit): Tue Jun 23 15:08:50 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1207453002](https://codereview.chromium.org/1207453002)  
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
  

---   

## **regress-499790.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/objects.cc,](https://crbug.com/499790)**  
**[Commit: Don't insert elements transitions into normalized maps](https://chromium.googlesource.com/v8/v8/+/c49659b)**  
  
Date(Commit): Tue Jun 23 14:33:11 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1203653003](https://codereview.chromium.org/1203653003)  
Regress: [mjsunit/regress/regress-499790.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-499790.js)  
```javascript
var a = [1];
a.foo = "bla";
delete a.foo;
a[0] = 1.5;

var a2 = [];
a2.foo = "bla";
delete a2.foo;  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c49659b^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=c49659b)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=c49659b)  
[src/transitions.cc](https://cs.chromium.org/chromium/src/v8/src/transitions.cc?cl=c49659b)  
[test/mjsunit/regress/regress-499790.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-499790.js?cl=c49659b)  
  

---   

## **regress-4207.js (v8 issue)**  
   
**[Issue: 0/0 is truthy in some cases](https://crbug.com/v8/4207)**  
**[Commit: [turbofan] NaN is never truish.](https://chromium.googlesource.com/v8/v8/+/78e9a2d)**  
  
Date(Commit): Tue Jun 23 12:24:54 2015  
Code Review: [https://codereview.chromium.org/1198993009](https://codereview.chromium.org/1198993009)  
Regress: [mjsunit/compiler/regress-4207.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4207.js)  
```javascript
function bar() { return 0/0 && 1; }
%PrepareFunctionForOptimization(bar);
assertEquals(NaN, bar());
%OptimizeFunctionOnNextCall(bar);
assertEquals(NaN, bar());

function foo() { return 0/0 || 1; }
%PrepareFunctionForOptimization(foo);
assertEquals(1, foo());
%OptimizeFunctionOnNextCall(foo);
assertEquals(1, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/78e9a2d^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=78e9a2d)  
[test/mjsunit/compiler/regress-4207.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4207.js?cl=78e9a2d)  
  

---   

## **regress-4206.js (v8 issue)**  
   
**[Issue: TurboFan's Float(32|64)(Min|Max) operations are not consistent between architectures.](https://crbug.com/v8/4206)**  
**[Commit: [arm64][turbofan] Fix implementation of Float64Min.](https://chromium.googlesource.com/v8/v8/+/d783b76)**  
  
Date(Commit): Tue Jun 23 11:58:58 2015  
Code Review: [https://codereview.chromium.org/1200123004](https://codereview.chromium.org/1200123004)  
Regress: [mjsunit/compiler/regress-4206.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-4206.js)  
```javascript
function Module(stdlib) {
  "use asm";
  function TernaryMin(a, b) {
    a=+(a);
    b=+(b);
    return (+((a < b) ? a : b));
  }
  function TernaryMax(a, b) {
    a=+(a);
    b=+(b);
    return (+((b < a) ? a : b));
  }
  return { TernaryMin: TernaryMin,
           TernaryMax: TernaryMax };
}
var min = Module(this).TernaryMin;
var max = Module(this).TernaryMax;

assertEquals(0.0, min(-0.0, 0.0));
assertEquals(0.0, min(NaN, 0.0));
assertEquals(-0.0, min(NaN, -0.0));
assertEquals(-0.0, max(0.0, -0.0));
assertEquals(0.0, max(NaN, 0.0));
assertEquals(-0.0, max(NaN, -0.0));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d783b76^!)  
[src/compiler/arm64/instruction-selector-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/instruction-selector-arm64.cc?cl=d783b76)  
[src/compiler/machine-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/machine-operator.h?cl=d783b76)  
[test/mjsunit/compiler/regress-4206.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-4206.js?cl=d783b76)  
  

---   

## **regress-crbug-502930.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/objects-debug.cc,](https://crbug.com/502930)**  
**[Commit: Map::ReconfigureProperty() should mark map as unstable when it returns a different map.](https://chromium.googlesource.com/v8/v8/+/4742176)**  
  
Date(Commit): Tue Jun 23 11:30:58 2015  
Components: Blink>JavaScript  
Labels: M-43, M-44, merge-merged-4.4, Merge-Merged-4.3, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1200003002](https://codereview.chromium.org/1200003002)  
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
  

---   

## **regress-crbug-501808.js (chromium issue)**  
   
**[Issue: Direct-leak in uprv_malloc_54](https://crbug.com/501808)**  
**[Commit: Global handle leak in Realm.create() fixed.](https://chromium.googlesource.com/v8/v8/+/5c4aae3)**  
  
Date(Commit): Tue Jun 23 11:04:21 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-LeakSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1197403002](https://codereview.chromium.org/1197403002)  
Regress: [mjsunit/regress/regress-crbug-501808.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-501808.js)  
```javascript
var r = Realm.create();
assertEquals(0, "a".localeCompare("a"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5c4aae3^!)  
[src/d8.cc](https://cs.chromium.org/chromium/src/v8/src/d8.cc?cl=5c4aae3)  
[test/mjsunit/regress/regress-crbug-501808.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-501808.js?cl=5c4aae3)  
  

---   

## **regress-crbug-501809.js (chromium issue)**  
   
**[Issue: UNKNOWN in int v8::internal::CompareExchangeSeqCst<int>](https://crbug.com/501809)**  
**[Commit: Fix cluster-fuzz bug introduced in refs/heads/master@{#28796}](https://chromium.googlesource.com/v8/v8/+/e6fed5e)**  
  
Date(Commit): Fri Jun 19 16:14:15 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1189223003](https://codereview.chromium.org/1189223003)  
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
  

---   

## **regress-500980.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/handles.h,](https://crbug.com/500980)**  
**[Commit: Protect error message formatter against invalid string length.](https://chromium.googlesource.com/v8/v8/+/4b7d5dc)**  
  
Date(Commit): Fri Jun 19 08:31:31 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1191263002](https://codereview.chromium.org/1191263002)  
Regress: [mjsunit/regress/regress-500980.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-500980.js)  
```javascript
var a = "a";
assertThrows(function() { while (true) a += a; }, RangeError);
assertThrows(function() { a in a; }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4b7d5dc^!)  
[src/messages.cc](https://cs.chromium.org/chromium/src/v8/src/messages.cc?cl=4b7d5dc)  
[src/string-builder.cc](https://cs.chromium.org/chromium/src/v8/src/string-builder.cc?cl=4b7d5dc)  
[src/string-builder.h](https://cs.chromium.org/chromium/src/v8/src/string-builder.h?cl=4b7d5dc)  
[test/mjsunit/regress/regress-500980.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-500980.js?cl=4b7d5dc)  
  

---   

## **regress-500831.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/assembler.cc,](https://crbug.com/500831)**  
**[Commit: ARM: make predictable code size scope more precise in DoDeferredInstanceOfKnownGlobal.](https://chromium.googlesource.com/v8/v8/+/fda60dc)**  
  
Date(Commit): Fri Jun 19 04:54:51 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1189123003](https://codereview.chromium.org/1189123003)  
Regress: [mjsunit/regress/regress-500831.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-500831.js)  
```javascript
function deepEquals(a, b) {
  if (a === b) {;
    return true;
  }
  if (typeof a != typeof b) return false;
  if (typeof a == 'number')
    ;
  if (typeof a !== "object" && typeof a !== "function") return false;
  var objectClass = classOf();
  if (b) return false;
  if (objectClass === "RegExp") {;
  }
  if (objectClass === "Function") return false;
  if (objectClass === "Array") {
    var elementCount = 0;
    if (a.length != b.length) {
      return false;
    }
    for (var i = 0; i < a.length; i++) {
      if (a[i][i]) return false;
    }
    return true;
  }
  if (objectClass == 'String' || objectClass == 'Number' ||
      objectClass == 'Boolean' || objectClass == 'Date') {
    if (a.valueOf()) return false;
  };
}
function equals(expected, found, name_opt) {
  if (!deepEquals(found, expected)) {}
};
function instof(obj, type) {
  if (!(obj instanceof type)) {
    var actualTypeName = null;
    var actualConstructor = Object.getPrototypeOf().constructor;
    if (typeof actualConstructor == "function") {;
    };
  }
};
var __v_0 = 1;
var __v_6 = {};
var __v_9 = {};

function __f_4() {
  return function() {};
}
__v_6 = new Uint8ClampedArray(10);

function __f_6() {
  __v_6[0] = 0.499;
  instof(__f_4(), Function);
  equals();
  __v_6[0] = 0.5;
  equals();
  __v_0[0] = 0.501;
  equals(__v_6[4294967295]);
  __v_6[0] = 1.499;
  equals();
  __v_6[0] = 1.5;
  equals();
  __v_6[0] = 1.501;
  equals();
  __v_6[0] = 2.5;
  equals(__v_6[-1073741824]);
  __v_6[0] = 3.5;
  equals();
  __v_6[0] = 252.5;
  equals();
  __v_6[0] = 253.5;
  equals();
  __v_6[0] = 254.5;
  equals();
  __v_6[0] = 256.5;
  equals();
  __v_6[0] = -0.5;
  equals(__v_6[8]);
  __v_6[0] = -1.5;
  equals();
  __v_6[0] = 1000000000000;
  equals();
  __v_9[0] = -1000000000000;
  equals(__v_6[0]);
};
%PrepareFunctionForOptimization(__f_6);
__f_6();
__f_6();
%OptimizeFunctionOnNextCall(__f_6);
__f_6();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fda60dc^!)  
[src/arm/lithium-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-codegen-arm.cc?cl=fda60dc)  
[test/mjsunit/regress/regress-500831.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-500831.js?cl=fda60dc)  
  

---   

## **regress-487981.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/487981)**  
**[Commit: ARM64: remove stack pushes without frame in RegExpExecStub.](https://chromium.googlesource.com/v8/v8/+/19cdd00)**  
  
Date(Commit): Thu Jun 18 15:45:32 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1183593005](https://codereview.chromium.org/1183593005)  
Regress: [mjsunit/regress/regress-487981.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-487981.js)  
```javascript
function __f_2(o) {
  return o.field.b.x;
}

%PrepareFunctionForOptimization(__f_2);

try {
  %OptimizeFunctionOnNextCall(__f_2);
  __v_1 = __f_2();
} catch (e) { }

function __f_3() { __f_3(/./.test()); };

try {
  __f_3();
} catch (e) { }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/19cdd00^!)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=19cdd00)  
[test/mjsunit/regress/regress-487981.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-487981.js?cl=19cdd00)  
  

---   

## **regress-crbug-500497.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/500497)**  
**[Commit: Hydrogen object literals: always initialize in-object properties](https://chromium.googlesource.com/v8/v8/+/5fca394)**  
  
Date(Commit): Wed Jun 17 11:24:24 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/1182113007](https://codereview.chromium.org/1182113007)  
Regress: [mjsunit/regress/regress-crbug-500497.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-500497.js)  
```javascript
var global = [];  // Used to keep some objects alive.

function Ctor() {
  var result = {a: {}, b: {}, c: {}, d: {}, e: {}, f: {}, g: {}};
  return result;
};
%PrepareFunctionForOptimization(Ctor);
gc();

for (var i = 0; i < 120; i++) {
  // Make the "a" property long-lived, while everything else is short-lived.
  global.push(Ctor().a);
  (function FillNewSpace() {
    new Array(10000);
  })();
}

assertUnoptimized(Ctor);
%OptimizeFunctionOnNextCall(Ctor);
for (var i = 0; i < 10000; i++) {
  // At least one of these calls will run out of new space. The bug is
  // triggered when it is allocation #3 that triggers GC.
  Ctor();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5fca394^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=5fca394)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=5fca394)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=5fca394)  
[test/mjsunit/regress/regress-crbug-500497.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500497.js?cl=5fca394)  
  

---   

## **regress-479528.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/objects.cc,](https://crbug.com/479528)**  
**[Commit: Only walk the hidden prototype chain for private nonexistent symbols](https://chromium.googlesource.com/v8/v8/+/bb1b54a)**  
  
Date(Commit): Wed Jun 17 10:20:52 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1185373004](https://codereview.chromium.org/1185373004)  
Regress: [mjsunit/regress/regress-479528.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-479528.js)  
```javascript
var __v_7 = {"__proto__": this};
__v_9 = %CreatePrivateSymbol("__v_9");
this[__v_9] = "moo";
function __f_5() {
    __v_7[__v_9] = "bow-wow";
}
__f_5();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bb1b54a^!)  
[src/ic/handler-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/ic/handler-compiler.cc?cl=bb1b54a)  
[test/mjsunit/regress/regress-479528.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-479528.js?cl=bb1b54a)  
  

---   

## **regress-500173.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/objects.h,](https://crbug.com/500173)**  
**[Commit: Rely on the map being a dictionary map rather than not having a backpointer](https://chromium.googlesource.com/v8/v8/+/72cdb99)**  
  
Date(Commit): Wed Jun 17 10:14:01 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1194513003](https://codereview.chromium.org/1194513003)  
Regress: [mjsunit/regress/regress-500173.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-500173.js)  
```javascript
function f(a) {
  a.foo = {};
  a[0] = 1;
  a.__defineGetter__('foo', function() {});
  a[0] = {};
  a.bar = 0;
}
f(new Array());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/72cdb99^!)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=72cdb99)  
[src/lookup.h](https://cs.chromium.org/chromium/src/v8/src/lookup.h?cl=72cdb99)  
[test/mjsunit/regress/regress-500173.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-500173.js?cl=72cdb99)  
  

---   

## **regress-crbug-500824.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/zone.h,](https://crbug.com/500824)**  
**[Commit: [turbofan] Work around negative parameter count.](https://chromium.googlesource.com/v8/v8/+/21a1975)**  
  
Date(Commit): Tue Jun 16 09:44:28 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1186333002](https://codereview.chromium.org/1186333002)  
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

%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/21a1975^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=21a1975)  
[test/mjsunit/regress/regress-crbug-500824.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500824.js?cl=21a1975)  
  

---   

## **regress-crbug-500435.js (chromium issue)**  
   
**[Issue: Regression: 'Letterless' app is not working properly.](https://crbug.com/500435)**  
**[Commit: [crankshaft] Fix wrong bailout points in for-in loop body.](https://chromium.googlesource.com/v8/v8/+/45439b9)**  
  
Date(Commit): Tue Jun 16 08:08:42 2015  
Components: Blink>JavaScript>GC, Blink>JavaScript, Platform>Apps  
Labels: M-45, Needs-Feedback, ReleaseBlock-Stable  
Code Review: [https://codereview.chromium.org/1183683004](https://codereview.chromium.org/1183683004)  
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

%PrepareFunctionForOptimization(foo);
foo([1,2]);
foo([2,3]);
%OptimizeFunctionOnNextCall(foo);
foo([1,2]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/45439b9^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=45439b9)  
[test/mjsunit/regress/regress-crbug-500435.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-500435.js?cl=45439b9)  
  

---   

## **regress-500176.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/handles-inl.h,](https://crbug.com/500176)**  
**[Commit: MIPS: Remove unsafe EmitLoadRegister usage in AddI/SubI for constant right operand.](https://chromium.googlesource.com/v8/v8/+/b7d8cb4)**  
  
Date(Commit): Mon Jun 15 17:58:43 2015  
Components: Blink>JavaScript  
Labels: Arch-MIPS, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1185143002](https://codereview.chromium.org/1185143002)  
Regress: [mjsunit/regress/regress-500176.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-500176.js)  
```javascript
function __f_0(p) {
  var __v_0 = -2147483642;
  for (var __v_1 = 0; __v_1 < 10; __v_1++) {
    if (__v_1 > 5) { __v_0 = __v_0 + p; break;}
  }
}
for (var __v_2 = 0; __v_2 < 100000; __v_2++) __f_0(42);
__v_2 = { f: function () { return x + y; },
          2: function () { return x - y} };  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b7d8cb4^!)  
[src/mips/lithium-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/lithium-codegen-mips.cc?cl=b7d8cb4)  
[src/mips/macro-assembler-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/macro-assembler-mips.cc?cl=b7d8cb4)  
[test/mjsunit/regress/regress-500176.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-500176.js?cl=b7d8cb4)  
  

---   

## **regress-crbug-498022.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/498022)**  
**[Commit: Fix clobbered register when setting this_function variable.](https://chromium.googlesource.com/v8/v8/+/bf2bbc8)**  
  
Date(Commit): Mon Jun 15 10:18:57 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, M-45, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1185703002](https://codereview.chromium.org/1185703002)  
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
...  
  

---   

## **regress-4121.js (v8 issue)**  
   
**[Issue: Regression: Memory leak since at least 3.26.33](https://crbug.com/v8/4121)**  
**[Commit: Map::TryUpdate() must be in sync with Map::Update().](https://chromium.googlesource.com/v8/v8/+/4cc4bc5)**  
  
Date(Commit): Fri Jun 12 12:36:40 2015  
Code Review: [https://codereview.chromium.org/1181163002](https://codereview.chromium.org/1181163002)  
Regress: [mjsunit/regress/regress-4121.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4121.js)  
```javascript
function literals_sharing_test(warmup, optimize) {
  function closure() {
    // Ensure small array literals start in specific element kind mode.
    assertTrue(%HasSmiElements([]));
    assertTrue(%HasSmiElements([1]));
    assertTrue(%HasSmiElements([1, 2]));
    assertTrue(%HasDoubleElements([1.1]));
    assertTrue(%HasDoubleElements([1.1, 2]));

    var a = [1, 2, 3];
    if (warmup) {
      // Transition elements kind during warmup...
      assertTrue(%HasSmiElements(a));
      assertEquals(4, a.push(1.3));
    }
    // ... and ensure that the information about transitioning is
    // propagated to the next closure.
    assertTrue(%HasDoubleElements(a));
  };
  %PrepareFunctionForOptimization(closure);
  ;
  %EnsureFeedbackVectorForFunction(closure);
  if (optimize) %OptimizeFunctionOnNextCall(closure);
  closure();
}


function test() {
  var warmup = true;
  for (var i = 0; i < 3; i++) {
    print('iter: ' + i + ', warmup: ' + warmup);
    literals_sharing_test(warmup, false);
    warmup = false;
  }
  print("iter: " + i + ", opt: true");
  literals_sharing_test(warmup, true);
}

test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4cc4bc5^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4cc4bc5)  
[test/cctest/test-migrations.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-migrations.cc?cl=4cc4bc5)  
[test/mjsunit/regress/regress-4121.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4121.js?cl=4cc4bc5)  
  

---   

## **regress-crbug-498811.js (chromium issue)**  
   
**[Issue: Unresolved "this" reference.](https://crbug.com/498811)**  
**[Commit: Add script context with context-allocated "const this"](https://chromium.googlesource.com/v8/v8/+/103fcfa)**  
  
Date(Commit): Fri Jun 12 12:34:24 2015  
Components: Blink>JavaScript>Compiler  
Labels: None  
Code Review: [https://codereview.chromium.org/1173333004.](https://codereview.chromium.org/1173333004.)  
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
...  
  

---   

## **regress-cr493566.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/lookup.cc,](https://crbug.com/493566)**  
**[Commit: [es6] Make sure we call add property when adding a new property](https://chromium.googlesource.com/v8/v8/+/5f72593)**  
  
Date(Commit): Thu Jun 11 21:24:41 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1161073002](https://codereview.chromium.org/1161073002)  
Regress: [mjsunit/es6/regress/regress-cr493566.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-cr493566.js)  
```javascript
"use strict";
var global = this;

(function TestGlobalReceiver() {
  class A {
    s(value) {
      super.bla = value;
    }
  }
  var a = new A();
  a.s(9);
  assertEquals(undefined, global.bla);
  assertEquals(9, a.bla);

  a = new A();
  a.s.call(global, 10);
  assertEquals(10, global.bla);
  assertEquals(undefined, a.bla);
})();


(function TestProxyProto() {
  var calls = 0;
  var handler = {
    set(t, p, v, r) {
      calls++;
      return Reflect.set(t, p, v, r);
    },
    getPropertyDescriptor(target, name) {
      calls += 10;
      return undefined;
    }
  };
  var target = {};
  var proxy = new Proxy(target, handler);
  var object = {
    __proto__: proxy,
    setX(v) {
      super.x = v;
    },
    setSymbol(sym, v) {
      super[sym] = v;
    }
  };

  object.setX(1);
  assertEquals(1, object.x);
  assertEquals(1, Object.getOwnPropertyDescriptor(object, 'x').value);
  assertEquals(1, calls);

  calls = 0;
  object.setX.call(proxy, 2);
  assertEquals(2, target.x);
  assertEquals(1, Object.getOwnPropertyDescriptor(object, 'x').value);
  assertEquals(1, calls);

  var sym = Symbol();
  calls = 0;
  object.setSymbol.call(global, sym, 2);
  assertEquals(2, Object.getOwnPropertyDescriptor(global, sym).value);
  // We currently do not invoke proxy traps for symbols
  assertEquals(1, calls);
});


(function TestProxyReceiver() {
  var object = {
    setY(v) {
      super.y = v;
    }
  };

  var calls = 0;
  var target = {target:1};
  var handler = {
    getOwnPropertyDescriptor(t, name) {
      calls++;
    },
    defineProperty(t, name, desc) {
      calls += 10;
      t[name] = desc.value;
      return true;
    },
    set(target, name, value) {
      assertUnreachable();
    }
  };
  var proxy = new Proxy(target, handler);

  assertEquals(undefined, object.y);
  object.setY(10);
  assertEquals(10, object.y);

  // Change the receiver to the proxy, but the set is called on the global.
  object.setY.call(proxy, 3);
  assertEquals(3, target.y);
  assertEquals(11, calls);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f72593^!)  
[test/mjsunit/es6/regress/regress-cr493566.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-cr493566.js?cl=5f72593)  
  

---   

## **regress-arrow-duplicate-params.js (other issue)**  
   
**[Commit: Support rest parameters in arrow functions](https://chromium.googlesource.com/v8/v8/+/e7a62c2)**  
  
Date(Commit): Wed Jun 10 16:11:31 2015  
Code Review: [https://codereview.chromium.org/1178523002](https://codereview.chromium.org/1178523002)  
Regress: [mjsunit/es6/regress/regress-arrow-duplicate-params.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-arrow-duplicate-params.js)  
```javascript
assertThrows("(x, x, y) => 10;", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e7a62c2^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=e7a62c2)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=e7a62c2)  
[src/preparser.h](https://cs.chromium.org/chromium/src/v8/src/preparser.h?cl=e7a62c2)  
[test/message/arrow-bare-rest-param.js](https://cs.chromium.org/chromium/src/v8/test/message/arrow-bare-rest-param.js?cl=e7a62c2)  
[test/message/arrow-bare-rest-param.out](https://cs.chromium.org/chromium/src/v8/test/message/arrow-bare-rest-param.out?cl=e7a62c2)  
...  
  
  
---   

## **regress-crbug-482998.js (chromium issue)**  
   
**[Issue: Regexp performance tanks on unicode inputs](https://crbug.com/482998)**  
**[Commit: Reland II of 'Optimize trivial regexp disjunctions' CL 1176453002](https://chromium.googlesource.com/v8/v8/+/05507cc)**  
  
Date(Commit): Wed Jun 10 09:55:31 2015  
Components: Blink>JavaScript>Runtime  
Labels: Performance, Via-Wizard, Arch-x86_64  
Code Review: [https://codereview.chromium.org/1180433003](https://codereview.chromium.org/1180433003)  
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
...  
  

---   

## **regress-4169.js (v8 issue)**  
   
**[Issue: Pushing new contexts in top-level code is borked](https://crbug.com/v8/4169)**  
**[Commit: [turbofan] Fix context chain extension for top-level code.](https://chromium.googlesource.com/v8/v8/+/eb0593e)**  
  
Date(Commit): Wed Jun 10 06:03:14 2015  
Code Review: [https://codereview.chromium.org/1172013002](https://codereview.chromium.org/1172013002)  
Regress: [mjsunit/regress/regress-4169.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4169.js)  
```javascript
with ({}) {
  eval("var x = 23");
  assertEquals(23, x);
}
assertEquals(23, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/eb0593e^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=eb0593e)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=eb0593e)  
[test/mjsunit/regress/regress-4169.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4169.js?cl=eb0593e)  
  

---   

## **regress-eval-context.js (other issue)**  
   
**[Commit: [turbofan] Fix one mean typo in kResolvePossiblyDirectEval.](https://chromium.googlesource.com/v8/v8/+/f45f24d)**  
  
Date(Commit): Tue Jun 09 17:14:52 2015  
Code Review: [https://codereview.chromium.org/1169853006](https://codereview.chromium.org/1169853006)  
Regress: [mjsunit/regress/regress-eval-context.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-eval-context.js)  
```javascript
(function() {
  'use strict';
  var x = 0;

  {
    let x = 1;
    assertEquals(1, eval("x"));
  }

  {
    let y = 2;
    assertEquals(0, eval("x"));
  }

  assertEquals(0, eval("x"));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f45f24d^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=f45f24d)  
[test/mjsunit/regress/regress-eval-context.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-eval-context.js?cl=f45f24d)  
  
  
---   

## **regress-crbug-493290.js (chromium issue)**  
   
**[Issue: Fatal error in ../../src/objects-inl.h,](https://crbug.com/493290)**  
**[Commit: Drop computed handler count and index from AST.](https://chromium.googlesource.com/v8/v8/+/c14ba5e)**  
  
Date(Commit): Mon Jun 08 18:19:40 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1157213004](https://codereview.chromium.org/1157213004)  
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
...  
  

---   

## **regress-3718.js (v8 issue)**  
   
**[Issue: CallSite.getTypeName() throws TypeError in strict mode](https://crbug.com/v8/3718)**  
**[Commit: Check for null and undefined when getting type name for stack trace.](https://chromium.googlesource.com/v8/v8/+/f2cce3c)**  
  
Date(Commit): Mon Jun 08 13:02:27 2015  
Code Review: [https://codereview.chromium.org/1164933005](https://codereview.chromium.org/1164933005)  
Regress: [mjsunit/regress/regress-3718.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3718.js)  
```javascript
"use strict";

function getTypeName(receiver) {
  Error.prepareStackTrace = function(e, stack) { return stack; }
  var stack = (function() { return new Error().stack; }).call(receiver);
  Error.prepareStackTrace = undefined;
  return stack[0].getTypeName();
}

assertNull(getTypeName(undefined));
assertNull(getTypeName(null));
assertEquals("Number", getTypeName(1));
assertEquals("String", getTypeName(""));
assertEquals("Boolean", getTypeName(false));
assertEquals("Object", getTypeName({}));
assertEquals("Array", getTypeName([]));
assertEquals("Proxy", getTypeName(new Proxy({},{})));
assertEquals("Custom", getTypeName(new (function Custom(){})()));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f2cce3c^!)  
[src/messages.js](https://cs.chromium.org/chromium/src/v8/src/messages.js?cl=f2cce3c)  
[test/mjsunit/regress/regress-3718.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3718.js?cl=f2cce3c)  
  

---   

## **regress-crbug-471659.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/runtime/runtime-strings.cc,](https://crbug.com/471659)**  
**[Commit: A couple of other "stack overflow" vs. "has_pending_exception()" issues fixed.](https://chromium.googlesource.com/v8/v8/+/050e888)**  
  
Date(Commit): Fri Jun 05 15:52:20 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1151333005](https://codereview.chromium.org/1151333005)  
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
...  
  

---   

## **regress-crbug-493284.js (chromium issue)**  
   
**[Issue: Direct-leak in uprv_malloc_54](https://crbug.com/493284)**  
**[Commit: Fixed memory-leak in d8. It did not clean evaluation context used for executing shell commands.](https://chromium.googlesource.com/v8/v8/+/405844b)**  
  
Date(Commit): Wed Jun 03 14:34:58 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-LeakSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1147343004](https://codereview.chromium.org/1147343004)  
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
  

---   

## **regress-4160.js (v8 issue)**  
   
**[Issue: Out of sync context/scope chains for arrow function calling eval](https://crbug.com/v8/4160)**  
**[Commit: Fix arrow functions requiring context without slots.](https://chromium.googlesource.com/v8/v8/+/68beef5)**  
  
Date(Commit): Wed Jun 03 11:32:31 2015  
Code Review: [https://codereview.chromium.org/1146063006](https://codereview.chromium.org/1146063006)  
Regress: [mjsunit/es6/regress/regress-4160.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4160.js)  
```javascript
(function(x) {
  (function(x) {
    var boom = (() => eval(x));
    %PrepareFunctionForOptimization(boom);
    assertEquals(23, boom());
    assertEquals(23, boom());
    %OptimizeFunctionOnNextCall(boom);
    assertEquals(23, boom());
    assertEquals("23", x);
  })("23");
  assertEquals("42", x);
})("42");

(function(x) {
  (function(x) {
    var boom = (() => (eval("var x = 66"), x));
    %PrepareFunctionForOptimization(boom);
    assertEquals(66, boom());
    assertEquals(66, boom());
    %OptimizeFunctionOnNextCall(boom);
    assertEquals(66, boom());
    assertEquals("23", x);
  })("23");
  assertEquals("42", x);
})("42");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68beef5^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=68beef5)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=68beef5)  
[src/code-stubs.h](https://cs.chromium.org/chromium/src/v8/src/code-stubs.h?cl=68beef5)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=68beef5)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=68beef5)  
...  
  

---   

## **regress-crbug-493779.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Heap::CreateFillerObjectAt](https://crbug.com/493779)**  
**[Commit: Fix bogus insertion of filler in LO-space by String#replace.](https://chromium.googlesource.com/v8/v8/+/d207fce)**  
  
Date(Commit): Mon Jun 01 13:36:11 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1163793002](https://codereview.chromium.org/1163793002)  
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
  

---   

## **regress-crbug-490680.js (chromium issue)**  
   
**[Issue: toString() is called on values when they are thrown](https://crbug.com/490680)**  
**[Commit: Do not eagerly convert exception to string when creating a message object](https://chromium.googlesource.com/v8/v8/+/36d8363)**  
  
Date(Commit): Thu May 28 06:30:14 2015  
Components: Blink>JavaScript  
Labels: Hotlist-Slow, M-43, Hotlist-V8-Recharge, Via-Wizard  
Code Review: [https://codereview.chromium.org/1157563005](https://codereview.chromium.org/1157563005)  
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
...  
  

---   

## **regress-491578.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/verifier.cc,](https://crbug.com/491578)**  
**[Commit: [turbofan] Properly kill Terminate nodes when removing loops.](https://chromium.googlesource.com/v8/v8/+/b53c35a)**  
  
Date(Commit): Tue May 26 10:48:07 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1161583002](https://codereview.chromium.org/1161583002)  
Regress: [mjsunit/compiler/regress-491578.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-491578.js)  
```javascript
function foo(x) {
  if (x === undefined) return;
  while (true) {
    while (1 || 2) { }
    f();
  }
}
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b53c35a^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=b53c35a)  
[test/mjsunit/compiler/regress-491578.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-491578.js?cl=b53c35a)  
  

---   

## **regress-crbug-491062.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/isolate.cc,](https://crbug.com/491062)**  
**[Commit: Fixed a couple of failing DCHECK(has_pending_exception()).](https://chromium.googlesource.com/v8/v8/+/62b5650)**  
  
Date(Commit): Tue May 26 10:06:54 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1151373002](https://codereview.chromium.org/1151373002)  
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
  

---   

## **regress-491481.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/contexts.h,](https://crbug.com/491481)**  
**[Commit: Exclude non-optimizable functions from OptimizeFunctionOnNextCall.](https://chromium.googlesource.com/v8/v8/+/a893a5e)**  
  
Date(Commit): Tue May 26 08:47:04 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1143223004](https://codereview.chromium.org/1143223004)  
Regress: [mjsunit/regress/regress-491481.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-491481.js)  
```javascript
try {
%OptimizeFunctionOnNextCall(print);
try {
  __f_16();
} catch(e) { print(e); }
try {
  __f_10();
} catch(e) {; }
} catch(e) {}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a893a5e^!)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=a893a5e)  
[test/mjsunit/regress/regress-491481.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-491481.js?cl=a893a5e)  
  

---   

## **regress-variable-liveness.js (other issue)**  
   
**[Commit: [turbofan] Fix variable liveness control structure creation.](https://chromium.googlesource.com/v8/v8/+/d2ca18d)**  
  
Date(Commit): Thu May 21 09:57:11 2015  
Code Review: [https://codereview.chromium.org/1148133002](https://codereview.chromium.org/1148133002)  
Regress: [mjsunit/compiler/regress-variable-liveness.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-variable-liveness.js)  
```javascript
function foo(x) {
  %DeoptimizeFunction(run);
  return x;
}

function run() {
  var line = new Array(2);
  for (var n = 3; n > 0; n = n - 1) {
    if (n < foo(line.length)) line = new Array(n);
    line[0] = n;
  }
}

%PrepareFunctionForOptimization(run);
assertEquals(void 0, run());
%OptimizeFunctionOnNextCall(run);
assertEquals(void 0, run());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d2ca18d^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=d2ca18d)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=d2ca18d)  
[test/mjsunit/compiler/regress-variable-liveness.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-variable-liveness.js?cl=d2ca18d)  
  
  
---   

## **regress-crbug-489597.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/ic/ic.cc,](https://crbug.com/489597)**  
**[Commit: Fixed DCHECK in StoreIC::CompileHandler().](https://chromium.googlesource.com/v8/v8/+/1c673a5)**  
  
Date(Commit): Wed May 20 13:36:27 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1123153005](https://codereview.chromium.org/1123153005)  
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
  

---   

## **regress-crbug-489293.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/code-generator.cc,](https://crbug.com/489293)**  
**[Commit: [turbofan] Fix over-restictive assertion in code generator.](https://chromium.googlesource.com/v8/v8/+/7bd2d3e)**  
  
Date(Commit): Tue May 19 16:14:28 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1142873005](https://codereview.chromium.org/1142873005)  
Regress: [mjsunit/regress/regress-crbug-489293.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-489293.js)  
```javascript
function f() {
  var x = 0;
  for (var y = 0; y < 0; ++y) {
    x = x + y | 0;
  }
  return unbound;
};
%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);
assertThrows(f, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7bd2d3e^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=7bd2d3e)  
[test/mjsunit/regress/regress-crbug-489293.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-489293.js?cl=7bd2d3e)  
  

---   

## **regress-crbug-487105.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/x64/full-codegen-x64.cc,](https://crbug.com/487105)**  
**[Commit: Another regression test for resolving references to "this" in strict mode.](https://chromium.googlesource.com/v8/v8/+/18b6059)**  
  
Date(Commit): Tue May 19 12:51:42 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1136123010](https://codereview.chromium.org/1136123010)  
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
  

---   

## **regress-crbug-487608.js (chromium issue)**  
   
**[Issue: WebGL three.js example runs into V8 exception](https://crbug.com/487608)**  
**[Commit: Fix harmless HGraph verification failure after hoisting inlined bounds checks](https://chromium.googlesource.com/v8/v8/+/f817520)**  
  
Date(Commit): Tue May 19 07:32:48 2015  
Components: Blink>JavaScript, Blink, Internals>GPU>ANGLE  
Labels: None  
Code Review: [https://codereview.chromium.org/1133343003](https://codereview.chromium.org/1133343003)  
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
};
%PrepareFunctionForOptimization(foo);
foo(0);
foo(0);
%OptimizeFunctionOnNextCall(foo);
foo(0);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f817520^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=f817520)  
[test/mjsunit/regress/regress-crbug-487608.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-487608.js?cl=f817520)  
  

---   

## **regress-489151.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Simulator::LoadStoreHelper](https://crbug.com/489151)**  
**[Commit: Reland "Mark internal AccessorInfo properties as 'special data properties'"](https://chromium.googlesource.com/v8/v8/+/4268141)**  
  
Date(Commit): Mon May 18 12:36:40 2015  
Components: Blink>JavaScript  
Labels: Merge-na, Stability-Memory-AddressSanitizer, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1138483005](https://codereview.chromium.org/1138483005)  
Regress: [mjsunit/regress/regress-489151.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-489151.js)  
```javascript
this.__proto__ = Array.prototype;
Object.freeze(this);
function __f_0() {
  for (var __v_0 = 0; __v_0 < 10; __v_0++) {
    this.length = 1;
  }
}
 __f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4268141^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=4268141)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=4268141)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=4268141)  
[src/objects-inl.h](https://cs.chromium.org/chromium/src/v8/src/objects-inl.h?cl=4268141)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=4268141)  
...  
  

---   

## **regress-488398.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/deoptimizer.cc,](https://crbug.com/488398)**  
**[Commit: Bug: Runtime_GrowArrayElements provoked unnecessary lazy deopt.](https://chromium.googlesource.com/v8/v8/+/de3a1ca)**  
  
Date(Commit): Fri May 15 13:05:00 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.4, Security_Severity-High, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1141163004](https://codereview.chromium.org/1141163004)  
Regress: [mjsunit/regress/regress-488398.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-488398.js)  
```javascript
var __v_10 = 4294967295;
__v_0 = [];
__v_0.__proto__ = [];
__v_16 = __v_0;
function __f_17(__v_16, base) {
  __v_16[base + 1] = 1;
  __v_16[base + 4] = base + 4;
}
%PrepareFunctionForOptimization(__f_17);
__f_17(__v_16, true);
__f_17(__v_16, 14);
%OptimizeFunctionOnNextCall(__f_17);
__f_17(__v_16, 2048);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/de3a1ca^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=de3a1ca)  
[test/mjsunit/regress/regress-488398.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-488398.js?cl=de3a1ca)  
  

---   

## **regress-crbug-485548-1.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/objects-debug.cc,](https://crbug.com/485548)**  
**[Commit: Map::ReconfigureProperty() should mark map as unstable when there is an element kind transition somewhere in the middle of the transition tree.](https://chromium.googlesource.com/v8/v8/+/3c1487d)**  
  
Date(Commit): Fri May 15 10:39:51 2015  
Components: Blink>JavaScript  
Labels: M-43, M-44, merge-merged-4.4, Merge-Merged-4.3, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/1128043005](https://codereview.chromium.org/1128043005)  
Regress: [mjsunit/regress/regress-crbug-485548-1.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485548-1.js), [mjsunit/regress/regress-crbug-485548-2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485548-2.js)  
```javascript
var inner = new Array();
inner.a = {
  x: 1
};
inner[0] = 1.5;
inner.b = {
  x: 2
};
assertTrue(%HasDoubleElements(inner));

function foo(o) {
  return o.field.a.x;
};
%PrepareFunctionForOptimization(foo);
var outer = {};
outer.field = inner;
foo(outer);
foo(outer);
foo(outer);
%OptimizeFunctionOnNextCall(foo);
foo(outer);

var v = {
  get x() {
    return 0x7fffffff;
  }
};
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
  

---   

## **regress-4097.js (v8 issue)**  
   
**[Issue: Crash when assigning to 'super.foo' with receiver that is not an object](https://crbug.com/v8/4097)**  
**[Commit: Fix the behavior of 'super.foo' assignment when receiver is not an object.](https://chromium.googlesource.com/v8/v8/+/30b771a)**  
  
Date(Commit): Tue May 12 17:13:07 2015  
Code Review: [https://codereview.chromium.org/1132203005](https://codereview.chromium.org/1132203005)  
Regress: [mjsunit/es6/regress/regress-4097.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4097.js)  
```javascript
(function StoreToSuper () {
  "use strict";
  class A {
    s() {
      super.bla = 10;
    }
  };

  let a = new A();
  (new A).s.call(a);
  assertEquals(10, a.bla);
  assertThrows(function() { (new A).s.call(undefined); }, TypeError);
  assertThrows(function() { (new A).s.call(42); }, TypeError);
  assertThrows(function() { (new A).s.call(null); }, TypeError);
  assertThrows(function() { (new A).s.call("abc"); }, TypeError);
})();


(function LoadFromSuper () {
  "use strict";
  class A {
    s() {
      return super.bla;
    }
  };

  let a = new A();
  assertSame(undefined, (new A).s.call(a));
  assertSame(undefined, (new A).s.call(undefined));
  assertSame(undefined, (new A).s.call(42));
  assertSame(undefined, (new A).s.call(null));
  assertSame(undefined, (new A).s.call("abc"));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/30b771a^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=30b771a)  
[test/mjsunit/es6/regress/regress-4097.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-4097.js?cl=30b771a)  
  

---   

## **regress-crbug-485410.js (chromium issue)**  
   
**[Issue: Google Sheets crashes when scrolling with "illegal access"](https://crbug.com/485410)**  
**[Commit: Let Runtime_GrowArrayElements accept non-Smi numbers as |key|.](https://chromium.googlesource.com/v8/v8/+/f10b992)**  
  
Date(Commit): Sat May 09 10:30:49 2015  
Components: Blink>JavaScript>Compiler  
Labels: M-44, Via-Wizard, Hotlist-Google, ReleaseBlock-Stable, Hotlist-Partner-GSuite  
Code Review: [https://codereview.chromium.org/1132113004](https://codereview.chromium.org/1132113004)  
Regress: [mjsunit/regress/regress-crbug-485410.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-485410.js)  
```javascript
var doubles = new Float64Array(1);
function ToHeapNumber(i) {
  doubles[0] = i;
  return doubles[0];
};
%PrepareFunctionForOptimization(ToHeapNumber);
for (var i = 0; i < 3; i++) ToHeapNumber(i);
%OptimizeFunctionOnNextCall(ToHeapNumber);
ToHeapNumber(1);

function Fail(a, i, v) {
  a[i] = v;
};
%PrepareFunctionForOptimization(Fail);
for (var i = 0; i < 3; i++) Fail(new Array(1), 1, i);
%OptimizeFunctionOnNextCall(Fail);
Fail(new Array(1), ToHeapNumber(1050), 3);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f10b992^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=f10b992)  
[test/mjsunit/regress/regress-crbug-485410.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-485410.js?cl=f10b992)  
  

---   

## **regress-484544.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::IncrementalMarking::WhiteToGreyAndPush](https://crbug.com/484544)**  
**[Commit: Initialize sub-array literals first before pointing to it.](https://chromium.googlesource.com/v8/v8/+/c80d730)**  
  
Date(Commit): Fri May 08 09:24:31 2015  
Components: Blink>JavaScript>GC  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1132763002](https://codereview.chromium.org/1132763002)  
Regress: [mjsunit/regress/regress-484544.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-484544.js)  
```javascript
function f() {
  return [[], [], [[], [], []]];
}

for (var i=0; i<10000; i++) {
  f();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c80d730^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=c80d730)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=c80d730)  
[test/mjsunit/regress/regress-484544.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-484544.js?cl=c80d730)  
  

---   

## **regress-474783.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/474783)**  
**[Commit: Handle the case when derived constructor is [[Call]]ed with 0 args.](https://chromium.googlesource.com/v8/v8/+/cf53fed)**  
  
Date(Commit): Tue May 05 19:57:04 2015  
Components: Blink>JavaScript>Runtime  
Labels: Hotlist-Merge-Review, ReleaseBlock-Beta, M-43, Stability-Memory-AddressSanitizer, M-44, Nag, Merge-Merged-4.3, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1126783003](https://codereview.chromium.org/1126783003)  
Regress: [mjsunit/es6/regress/regress-474783.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-474783.js)  
```javascript
"use strict";
class Base {
}
class Subclass extends Base {
  constructor(a,b,c) {
    arguments[1];
  }
}
assertThrows(function() { Subclass(); }, TypeError);
assertThrows(function() { Subclass(1); }, TypeError);
assertThrows(function() { Subclass(1, 2); }, TypeError);
assertThrows(function() { Subclass(1, 2, 3); }, TypeError);
assertThrows(function() { Subclass(1, 2, 3, 4); }, TypeError);

assertThrows(function() { Subclass.call(); }, TypeError);
assertThrows(function() { Subclass.call({}); }, TypeError);
assertThrows(function() { Subclass.call({}, 1); }, TypeError);
assertThrows(function() { Subclass.call({}, 1, 2); }, TypeError);
assertThrows(function() { Subclass.call({}, 1, 2, 3, 4); }, TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf53fed^!)  
[src/arm/code-stubs-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/code-stubs-arm.cc?cl=cf53fed)  
[src/arm64/code-stubs-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/code-stubs-arm64.cc?cl=cf53fed)  
[src/ia32/code-stubs-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/code-stubs-ia32.cc?cl=cf53fed)  
[src/mips/code-stubs-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/code-stubs-mips.cc?cl=cf53fed)  
[src/mips64/code-stubs-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/code-stubs-mips64.cc?cl=cf53fed)  
...  
  

---   

## **regress-smi-scanning.js (other issue)**  
   
**[Commit: Fix smi scanning](https://chromium.googlesource.com/v8/v8/+/f21ea06)**  
  
Date(Commit): Mon May 04 15:02:30 2015  
Code Review: [https://codereview.chromium.org/1114073003](https://codereview.chromium.org/1114073003)  
Regress: [mjsunit/regress/regress-smi-scanning.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-smi-scanning.js)  
```javascript
x = 2
3;
assertEquals(2, x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f21ea06^!)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=f21ea06)  
[src/scanner.h](https://cs.chromium.org/chromium/src/v8/src/scanner.h?cl=f21ea06)  
[test/mjsunit/regress/regress-smi-scanning.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-smi-scanning.js?cl=f21ea06)  
  
  
---   

## **regress-crbug-484077.js (chromium issue)**  
   
**[Issue: Bogus expectation for inspector/sources/debugger/properties-special.html](https://crbug.com/484077)**  
**[Commit: Set inferred name of bound function to empty string.](https://chromium.googlesource.com/v8/v8/+/f42544b)**  
  
Date(Commit): Mon May 04 09:55:43 2015  
Components: Blink>JavaScript  
Labels: None  
Code Review: [https://codereview.chromium.org/1122733002](https://codereview.chromium.org/1122733002)  
Regress: [mjsunit/regress/regress-crbug-484077.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-484077.js)  
```javascript
assertEquals("", %FunctionGetInferredName((function(){}).bind({})));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f42544b^!)  
[src/runtime/runtime-function.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-function.cc?cl=f42544b)  
[test/mjsunit/regress/regress-crbug-484077.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-484077.js?cl=f42544b)  
  

---   

## **regress-crbug-480819.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/verifier.cc,](https://crbug.com/480819)**  
**[Commit: [turbofan] Fix frame state for class literal definition.](https://chromium.googlesource.com/v8/v8/+/6b60f19)**  
  
Date(Commit): Fri Apr 24 11:12:57 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1104673004](https://codereview.chromium.org/1104673004)  
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
  

---   

## **regress-crbug-480807.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::mark](https://crbug.com/480807)**  
**[Commit: [turbofan] Ignore dead cached nodes in the JSGraph.](https://chromium.googlesource.com/v8/v8/+/4f9bc2d)**  
  
Date(Commit): Fri Apr 24 10:51:32 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1101273002](https://codereview.chromium.org/1101273002)  
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
%PrepareFunctionForOptimization(foo);

try {
  foo();
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f9bc2d^!)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=4f9bc2d)  
[test/mjsunit/regress/regress-crbug-480807.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-480807.js?cl=4f9bc2d)  
  

---   

## **regress-4056.js (v8 issue)**  
   
**[Issue: Bug in strict-mode scoping in arrow functions that call eval](https://crbug.com/v8/4056)**  
**[Commit: Function scopes only must have a context if they call sloppy eval](https://chromium.googlesource.com/v8/v8/+/d5fd581)**  
  
Date(Commit): Thu Apr 23 13:19:54 2015  
Code Review: [https://codereview.chromium.org/1093183003](https://codereview.chromium.org/1093183003)  
Regress: [mjsunit/es6/regress/regress-4056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-4056.js)  
```javascript
function strictFunctionArrowEval(s) {
  "use strict";
  return (()=>eval(s))();
};

assertEquals(strictFunctionArrowEval("42"), 42)  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5fd581^!)  
[src/scopeinfo.cc](https://cs.chromium.org/chromium/src/v8/src/scopeinfo.cc?cl=d5fd581)  
[src/scopes.cc](https://cs.chromium.org/chromium/src/v8/src/scopes.cc?cl=d5fd581)  
[test/mjsunit/harmony/regress/regress-4056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-4056.js?cl=d5fd581)  
  

---   

## **regress-crbug-478011.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/handles.h,](https://crbug.com/478011)**  
**[Commit: Throw when attaching a stack trace to an object fails.](https://chromium.googlesource.com/v8/v8/+/8cf289c)**  
  
Date(Commit): Mon Apr 20 14:40:45 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1077153003](https://codereview.chromium.org/1077153003)  
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
  

---   

## **regress-crbug-477924.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::MemoryChunk::heap](https://crbug.com/477924)**  
**[Commit: Don't use normalized map cache for prototype maps](https://chromium.googlesource.com/v8/v8/+/4204c72)**  
  
Date(Commit): Fri Apr 17 12:16:07 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, Stability-Memory-AddressSanitizer, M-44, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1090193002](https://codereview.chromium.org/1090193002)  
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
  

---   

## **regress-475705.js (chromium issue)**  
   
**[Issue: Chrome: Crash Report - v8::internal::Trace::Flush](https://crbug.com/475705)**  
**[Commit: Reduce regexp compiler stack size when not optimizing regexps](https://chromium.googlesource.com/v8/v8/+/e0be050)**  
  
Date(Commit): Wed Apr 15 15:15:52 2015  
Components: Blink>JavaScript>Runtime  
Labels: Stability-Crash, M-44, MovedFrom-43  
Code Review: [https://codereview.chromium.org/1082763002](https://codereview.chromium.org/1082763002)  
Regress: [mjsunit/regress/regress-475705.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-475705.js)  
```javascript
function use_space_then_do_test(depth) {
  try {
    // The "+ depth" is to avoid the regexp compilation cache.
    var regexp_src = repeat(".(.)", 12) + depth;
    use_space(depth, regexp_src);
    return true;
  } catch (e) {
    assertFalse(("" + e).indexOf("tack") == -1);  // Check for [Ss]tack.
    return false;
  }
}

function use_space(n, regexp_src) {
  if (--n == 0) {
    do_test(regexp_src);
    return;
  }
  use_space(n, regexp_src);
}

function repeat(str, n) {
  var answer = "";
  while (n-- != 0) {
    answer += str;
  }
  return answer;
}

var subject = repeat("y", 200);

function do_test(regexp_src) {
  var re = new RegExp(regexp_src);
  re.test(subject);
}

function try_different_stack_limits() {
  var lower = 100;
  var higher = 100000;
  while (lower < higher - 1) {
    var average = Math.floor((lower + higher) / 2);
    if (use_space_then_do_test(average)) {
      lower = average;
    } else {
      higher = average;
    }
  }
  for (var i = lower - 5; i < higher + 5; i++) {
    use_space_then_do_test(i);
  }
}

try_different_stack_limits();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e0be050^!)  
[src/jsregexp.cc](https://cs.chromium.org/chromium/src/v8/src/jsregexp.cc?cl=e0be050)  
[src/jsregexp.h](https://cs.chromium.org/chromium/src/v8/src/jsregexp.h?cl=e0be050)  
[test/mjsunit/regress/regress-475705.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-475705.js?cl=e0be050)  
  

---   

## **regress-4027.js (v8 issue)**  
   
**[Issue: mjsunit/es7/object-observe fails on arm64 and linux with custom snapshot](https://crbug.com/v8/4027)**  
**[Commit: Correctly handle clearing of deprecated field types.](https://chromium.googlesource.com/v8/v8/+/68a7773)**  
  
Date(Commit): Wed Apr 15 09:55:33 2015  
Code Review: [https://codereview.chromium.org/1086063003](https://codereview.chromium.org/1086063003)  
Regress: [mjsunit/regress/regress-4027.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4027.js)  
```javascript
function Inner() {
  this.inner_name = "inner";
}

function Boom() {
  this.boom = "boom";
}

function Outer() {
  this.outer_name = "outer";
}

function SetInner(inner, value) {
  inner.prop = value;
}

function SetOuter(outer, value) {
  outer.inner = value;
}

var inner1 = new Inner();
var inner2 = new Inner();

SetInner(inner1, 10);
SetInner(inner2, 10);

var outer1 = new Outer();
var outer2 = new Outer();
var outer3 = new Outer();

SetOuter(outer1, inner1);
SetOuter(outer1, inner1);
SetOuter(outer1, inner1);

SetOuter(outer2, inner2);
SetOuter(outer2, inner2);
SetOuter(outer2, inner2);

SetOuter(outer3, inner2);
SetOuter(outer3, inner2);
SetOuter(outer3, inner2);


SetInner(inner2, 6.5);

outer1 = null;
inner1 = null;

gc();

var boom = new Boom();
SetOuter(outer2, boom);

gc();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/68a7773^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=68a7773)  
[test/mjsunit/regress/regress-4027.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4027.js?cl=68a7773)  
  

---   

## **regress-476488.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::HDeadCodeEliminationPhase::MarkLive](https://crbug.com/476488)**  
**[Commit: VectorICs: recreate feedback vector if scoping changes on recompile.](https://chromium.googlesource.com/v8/v8/+/2ebb794)**  
  
Date(Commit): Tue Apr 14 12:31:31 2015  
Components: Blink>JavaScript>Runtime, Blink>JavaScript  
Labels: Te-Logged, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1080253003](https://codereview.chromium.org/1080253003)  
Regress: [mjsunit/regress/regress-476488.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-476488.js)  
```javascript
function __f_0(message, a) {
  eval(), message;
  (function blue() {
    'use strict';
    eval(), eval(), message;
    gc();
  })();
}
__f_0();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2ebb794^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=2ebb794)  
[src/type-feedback-vector.cc](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.cc?cl=2ebb794)  
[src/type-feedback-vector.h](https://cs.chromium.org/chromium/src/v8/src/type-feedback-vector.h?cl=2ebb794)  
[test/mjsunit/regress/regress-476488.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-476488.js?cl=2ebb794)  
  

---   

## **regress-indirect-push-unchecked.js (other issue)**  
   
**[Commit: Fix indirect push](https://chromium.googlesource.com/v8/v8/+/434b456)**  
  
Date(Commit): Mon Apr 13 16:25:33 2015  
Code Review: [https://codereview.chromium.org/1087463003](https://codereview.chromium.org/1087463003)  
Regress: [mjsunit/regress/regress-indirect-push-unchecked.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-indirect-push-unchecked.js)  
```javascript
var a = [1.5];

function p() {
  Array.prototype.push.call(a, 1.7);
};
%PrepareFunctionForOptimization(p);
p();
p();
p();
%OptimizeFunctionOnNextCall(p);
p();
a.push({});
p();
assertEquals(1.7, a[a.length - 1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/434b456^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=434b456)  
[test/mjsunit/regress/regress-indirect-push-unchecked.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-indirect-push-unchecked.js?cl=434b456)  
  
  
---   

## **regress-4023.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/4023)**  
**[Commit: Do not inline store if field map was cleared.](https://chromium.googlesource.com/v8/v8/+/2f327a5)**  
  
Date(Commit): Mon Apr 13 09:43:52 2015  
Code Review: [https://codereview.chromium.org/1081033004](https://codereview.chromium.org/1081033004)  
Regress: [mjsunit/regress/regress-4023.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-4023.js)  
```javascript
function Inner() {
  this.property = "OK";
  this.prop2 = 1;
}

function Outer() {
  this.o = "u";
}
function KeepMapAlive(o) {
  return o.o;
};
%PrepareFunctionForOptimization(KeepMapAlive);
function SetInner(o, i) {
  o.inner_field = i;
};
%PrepareFunctionForOptimization(SetInner);
function Crash(o) {
  return o.inner_field.property;
};
%PrepareFunctionForOptimization(Crash);
var inner = new Inner();
var outer = new Outer();

SetInner(new Outer(), inner);
SetInner(outer, inner);

KeepMapAlive(outer);
KeepMapAlive(outer);
%OptimizeFunctionOnNextCall(KeepMapAlive, "concurrent");
KeepMapAlive(outer);

print(Crash(outer));
print(Crash(outer));
%OptimizeFunctionOnNextCall(Crash);
print(Crash(outer));

inner = undefined;
outer = undefined;
gc();


%OptimizeFunctionOnNextCall(SetInner);

var o2 = new Outer();
SetInner(o2, { invalid: 1.51, property: "OK" });
print(Crash(o2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/2f327a5^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=2f327a5)  
[src/hydrogen.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen.h?cl=2f327a5)  
[src/objects-debug.cc](https://cs.chromium.org/chromium/src/v8/src/objects-debug.cc?cl=2f327a5)  
[test/mjsunit/regress/regress-4023.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-4023.js?cl=2f327a5)  
  

---   

## **regress-crbug-469768.js (chromium issue)**  
   
**[Issue: Crash with signature v8::internal::Invoke ](https://crbug.com/469768)**  
**[Commit: JSEntryTrampoline: check for stack space before pushing arguments](https://chromium.googlesource.com/v8/v8/+/146598f)**  
  
Date(Commit): Tue Apr 07 09:13:44 2015  
Components: Blink  
Labels: M-43  
Code Review: [https://codereview.chromium.org/1056913003](https://codereview.chromium.org/1056913003)  
Regress: [mjsunit/regress/regress-crbug-469768.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-469768.js)  
```javascript
try {
  Array.prototype.concat.apply([], new Array(100000));
} catch (e) {
  // Throwing is fine, just don't crash.
}


try {
  Array.prototype.concat.apply([], new Array(150000));
} catch (e) {
  // Throwing is fine, just don't crash.
}


try {
  Array.prototype.concat.apply([], new Array(200000));
} catch (e) {
  // Throwing is fine, just don't crash.
}


try {
  Array.prototype.concat.apply([], new Array(248000));
} catch (e) {
  // Throwing is fine, just don't crash.
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/146598f^!)  
[src/arm/builtins-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/builtins-arm.cc?cl=146598f)  
[src/arm64/builtins-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/builtins-arm64.cc?cl=146598f)  
[src/ia32/builtins-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/builtins-ia32.cc?cl=146598f)  
[src/x64/builtins-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/builtins-x64.cc?cl=146598f)  
[test/mjsunit/regress/regress-crbug-469768.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-469768.js?cl=146598f)  
  

---   

## **regress-472504.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/isolate.cc,](https://crbug.com/472504)**  
**[Commit: Reland: Fix JSON parser Handle leak (previous CL 1041483004)](https://chromium.googlesource.com/v8/v8/+/5a93a33)**  
  
Date(Commit): Wed Apr 01 16:58:47 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1051833002](https://codereview.chromium.org/1051833002)  
Regress: [mjsunit/regress/regress-472504.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-472504.js)  
```javascript
function shouldThrow() {
    shouldThrow(JSON.parse('{"0":1}'));
}
assertThrows("shouldThrow()", RangeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5a93a33^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=5a93a33)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=5a93a33)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=5a93a33)  
[src/json-parser.h](https://cs.chromium.org/chromium/src/v8/src/json-parser.h?cl=5a93a33)  
[test/mjsunit/regress/regress-3976.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3976.js?cl=5a93a33)  
...  
  

---   

## **regress-470804.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/layout-descriptor.cc,](https://crbug.com/470804)**  
**[Commit: Layout descriptor must be trimmed when corresponding descriptors array is trimmed to stay in sync.](https://chromium.googlesource.com/v8/v8/+/3cb9f13)**  
  
Date(Commit): Mon Mar 30 17:03:50 2015  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, M-43, Merge-Merged-4.3, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1033273005](https://codereview.chromium.org/1033273005)  
Regress: [mjsunit/regress/regress-470804.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-470804.js)  
```javascript
function f() {
  this.foo00 = 0;
  this.foo01 = 0;
  this.foo02 = 0;
  this.foo03 = 0;
  this.foo04 = 0;
  this.foo05 = 0;
  this.foo06 = 0;
  this.foo07 = 0;
  this.foo08 = 0;
  this.foo09 = 0;
  this.foo0a = 0;
  this.foo0b = 0;
  this.foo0c = 0;
  this.foo0d = 0;
  this.foo0e = 0;
  this.foo0f = 0;
  this.foo10 = 0;
  this.foo11 = 0;
  this.foo12 = 0;
  this.foo13 = 0;
  this.foo14 = 0;
  this.foo15 = 0;
  this.foo16 = 0;
  this.foo17 = 0;
  this.foo18 = 0;
  this.foo19 = 0;
  this.foo1a = 0;
  this.foo1b = 0;
  this.foo1c = 0;
  this.foo1d = 0;
  this.foo1e = 0;
  this.foo1f = 0;
  this.d = 1.3;
  gc();
  this.boom = 230;
  this.boom = 1.4;
}

function g() {
  return new f();
}
g();
g();
var o = g();
assertEquals(0, o.foo00);
assertEquals(1.4, o.boom);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3cb9f13^!)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=3cb9f13)  
[src/layout-descriptor-inl.h](https://cs.chromium.org/chromium/src/v8/src/layout-descriptor-inl.h?cl=3cb9f13)  
[src/layout-descriptor.cc](https://cs.chromium.org/chromium/src/v8/src/layout-descriptor.cc?cl=3cb9f13)  
[src/layout-descriptor.h](https://cs.chromium.org/chromium/src/v8/src/layout-descriptor.h?cl=3cb9f13)  
[test/cctest/test-unboxed-doubles.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-unboxed-doubles.cc?cl=3cb9f13)  
...  
  

---   

## **regress-typedarray-length.js (other issue)**  
   
**[Commit: Fix speedup of typedarray-length loading in the ICs as well as Crankshaft](https://chromium.googlesource.com/v8/v8/+/87eef73)**  
  
Date(Commit): Mon Mar 30 11:50:23 2015  
Code Review: [https://codereview.chromium.org/1034393002](https://codereview.chromium.org/1034393002)  
Regress: [mjsunit/regress/regress-typedarray-length.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-typedarray-length.js)  
```javascript
var a = new Int32Array(100);
a.__proto__ = null;

function get(a) {
  return a.length;
};
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteLength;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteOffset;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

(function() {
"use strict";

class MyTypedArray extends Int32Array {
  get length() {
    return "length";
  }
}

a = new MyTypedArray();

get = function(a) {
  return a.length;
};
;
%PrepareFunctionForOptimization(get);
assertEquals("length", get(a));
assertEquals("length", get(a));
assertEquals("length", get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals("length", get(a));

a.__proto__ = null;

get = function(a) {
  return a.length;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));
})();

(function() {
"use strict";

class MyTypedArray extends Int32Array {
  constructor(length) {
    super(length);
  }
}

a = new MyTypedArray(1024);

get = function(a) {
  return a.length;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(1024, get(a));
assertEquals(1024, get(a));
assertEquals(1024, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(1024, get(a));
})();

(function() {
"use strict";
var a = new Uint8Array(4);
Object.defineProperty(a, 'length', {
  get: function() {
    return 'blah';
  }
});
get = function(a) {
  return a.length;
};
;
%PrepareFunctionForOptimization(get);
assertEquals("blah", get(a));
assertEquals("blah", get(a));
assertEquals("blah", get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals("blah", get(a));
})();

assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("length"));
assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("byteOffset"));
assertTrue(Int32Array.prototype.__proto__.hasOwnProperty("byteLength"));
assertTrue(delete Int32Array.prototype.__proto__.length);
assertTrue(delete Int32Array.prototype.__proto__.byteOffset);
assertTrue(delete Int32Array.prototype.__proto__.byteLength);

a = new Int32Array(100);

get = function(a) {
  return a.length;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteLength;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));

get = function(a) {
  return a.byteOffset;
};
;
%PrepareFunctionForOptimization(get);
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
assertEquals(undefined, get(a));
%OptimizeFunctionOnNextCall(get);
assertEquals(undefined, get(a));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/87eef73^!)  
[src/accessors.cc](https://cs.chromium.org/chromium/src/v8/src/accessors.cc?cl=87eef73)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=87eef73)  
[src/typedarray.js](https://cs.chromium.org/chromium/src/v8/src/typedarray.js?cl=87eef73)  
[src/v8natives.js](https://cs.chromium.org/chromium/src/v8/src/v8natives.js?cl=87eef73)  
[test/mjsunit/regress/regress-typedarray-length.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-typedarray-length.js?cl=87eef73)  
  
  
---   

## **regress-466993.js (chromium issue)**  
   
**[Issue: Google Chrome 39+ bug: var a = {"1": false, "2": false, "3": false, "4": false}; alert(a[1]); //true](https://crbug.com/466993)**  
**[Commit: Ensure object literal element boilerplates aren't modified.](https://chromium.googlesource.com/v8/v8/+/7c347c5)**  
  
Date(Commit): Mon Mar 30 09:20:09 2015  
Components: Blink>JavaScript  
Labels: M-43, Via-Wizard, Merge-Merged-4.3, M-41  
Code Review: [https://codereview.chromium.org/1037273002](https://codereview.chromium.org/1037273002)  
Regress: [mjsunit/regress/regress-466993.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-466993.js)  
```javascript
var test = function() {
  var a = {'1': false, '2': false, '3': false, '4': false};
  assertEquals(false, a[1]);
  a[1] = true;
};
;
%PrepareFunctionForOptimization(test);
test();
test();
test();
%OptimizeFunctionOnNextCall(test);
test();
test();
test();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c347c5^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=7c347c5)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=7c347c5)  
[src/ast.cc](https://cs.chromium.org/chromium/src/v8/src/ast.cc?cl=7c347c5)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=7c347c5)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=7c347c5)  
...  
  

---   

## **regress-3976.js (v8 issue)**  
   
**[Issue: OOM crash caused by GC not keeping up with fragmentation](https://crbug.com/v8/3976)**  
**[Commit: Fix OOM bug 3976.](https://chromium.googlesource.com/v8/v8/+/4c80680)**  
  
Date(Commit): Tue Mar 24 15:02:28 2015  
Code Review: [https://codereview.chromium.org/1024823002](https://codereview.chromium.org/1024823002)  
Regress: [mjsunit/regress/regress-3976.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3976.js)  
```javascript
table = [];

for (var i = 0; i < 32; i++) {
 table[i] = String.fromCharCode(i + 0x410);
}


var random = (function() {
  var seed = 10;
  return function() {
    seed = (seed * 1009) % 8831;
    return seed;
  };
})();


function key(length) {
  var s = "";
  for (var i = 0; i < length; i++) {
    s += table[random() % 32];
  }
  return '"' + s + '"';
}


function value() {
  return '[{' + '"field1" : ' + random() + ', "field2" : ' + random() + '}]';
}


function generate(n) {
  var s = '{';
  for (var i = 0; i < n; i++) {
     if (i > 0) s += ', ';
     s += key(random() % 10 + 7);
     s += ':';
     s += value();
  }
  s += '}';
  return s;
}


print("generating");

var str = generate(10000);

print("parsing "  + str.length);
JSON.parse(str);

print("done");  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c80680^!)  
[src/flag-definitions.h](https://cs.chromium.org/chromium/src/v8/src/flag-definitions.h?cl=4c80680)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=4c80680)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=4c80680)  
[src/heap/spaces.h](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.h?cl=4c80680)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=4c80680)  
...  
  

---   

## **regress-3985.js (v8 issue)**  
   
**[Issue: Arguments object materialization for escape analyzed objects can return wrong arguments](https://crbug.com/v8/3985)**  
**[Commit: Test for wrong arguments object materialization.](https://chromium.googlesource.com/v8/v8/+/0f94c96)**  
  
Date(Commit): Tue Mar 24 13:20:21 2015  
Code Review: [https://codereview.chromium.org/1032623003](https://codereview.chromium.org/1032623003)  
Regress: [mjsunit/regress/regress-3985.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3985.js)  
```javascript
var shouldThrow = false;

function h() {
  try {  // Prevent inlining in Crankshaft.
  } catch (e) {
  }
  var res = g.arguments[0].x;
  if (shouldThrow) {
    throw res;
  }
  return res;
}

function g(o) {
  h();
}

function f1() {
  var o = {x: 1};
  g(o);
  return o.x;
};
%PrepareFunctionForOptimization(f1);
function f2() {
  var o = {x: 2};
  g(o);
  return o.x;
};
%PrepareFunctionForOptimization(f2);
f1();
f2();
f1();
f2();
%OptimizeFunctionOnNextCall(f1);
%OptimizeFunctionOnNextCall(f2);
shouldThrow = true;
try {
  f1();
} catch (e) {
  assertEquals(e, 1);
}
try {
  f2();
} catch (e) {
  assertEquals(e, 2);
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f94c96^!)  
[test/mjsunit/mjsunit.status](https://cs.chromium.org/chromium/src/v8/test/mjsunit/mjsunit.status?cl=0f94c96)  
[test/mjsunit/regress/regress-3985.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3985.js?cl=0f94c96)  
  

---   

## **regress-filter-contexts.js (other issue)**  
   
**[Commit: Properly handle non-JSFunction constructors in CanRetainOtherContext](https://chromium.googlesource.com/v8/v8/+/1b16678)**  
  
Date(Commit): Mon Mar 23 19:24:58 2015  
Code Review: [https://codereview.chromium.org/1017263003](https://codereview.chromium.org/1017263003)  
Regress: [mjsunit/regress/regress-filter-contexts.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-filter-contexts.js)  
```javascript
function f() {
  return f.x;
};
%PrepareFunctionForOptimization(f);
f.__proto__ = null;
f.prototype = "";

f();
f();
%OptimizeFunctionOnNextCall(f);
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1b16678^!)  
[src/type-info.cc](https://cs.chromium.org/chromium/src/v8/src/type-info.cc?cl=1b16678)  
[test/mjsunit/regress/regress-filter-contexts.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-filter-contexts.js?cl=1b16678)  
  
  
---   

## **regress-469605.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::mark](https://crbug.com/469605)**  
**[Commit: [turbofan] Fix control reducer bug with walking non-control edges during ConnectNTL phase.](https://chromium.googlesource.com/v8/v8/+/d931700)**  
  
Date(Commit): Mon Mar 23 14:08:25 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1030623003](https://codereview.chromium.org/1030623003)  
Regress: [mjsunit/regress/regress-469605.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-469605.js), [mjsunit/regress/regress-469605b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-469605b.js)  
```javascript
function counter() {
  var i = 100;
  return function() {
    if (i-- > 0) return i;
    throw "done";
  }
}

var c1 = counter();
var c2 = counter();

var f = (function() {
  "use asm";
  return function f(i) {
    i = i|0;
    do {
      if (i > 0) c1();
      else c2();
    } while (true);
  }
})();

assertThrows(function() { f(0); });
assertThrows(function() { f(1); });

var c3 = counter();

var g = (function() {
  "use asm";
  return function g(i) {
    i = i + 1;
    do {
      i = c3(i);
    } while (true);
  }
})();

assertThrows(function() { g(0); });
assertThrows(function() { g(1); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d931700^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=d931700)  
[test/mjsunit/regress/regress-469605.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-469605.js?cl=d931700)  
  

---   

## **regress-bce-underflow.js (other issue)**  
   
**[Commit: Ensure we don't overflow in BCE](https://chromium.googlesource.com/v8/v8/+/0f57346)**  
  
Date(Commit): Fri Mar 20 16:43:05 2015  
Code Review: [https://codereview.chromium.org/1023123003](https://codereview.chromium.org/1023123003)  
Regress: [mjsunit/regress/regress-bce-underflow.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-bce-underflow.js)  
```javascript
function f(a, i, bool) {
  var result;
  if (bool) {
    // Make sure i - -0x80000000 doesn't overflow in BCE, missing a check for
    // x-0 later on.
    result = f2(a, 0x7fffffff, i, i, -0x80000000);
  } else {
    result = f2(a, -3, 4, i, 0);
  }
  return result;
}
%PrepareFunctionForOptimization(f);

function f2(a, c, x, i, d) {
  return a[x + c] + a[x - 0] + a[i - d];
}


var a = [];
var i = 0;
a.push(i++);
a.push(i++);
a.push(i++);
a.push(i++);
a.push(i++);
f(a, 0, false);
f(a, 0, false);
f(a, 0, false);
%OptimizeFunctionOnNextCall(f);
%DebugPrint(f(a, -0x7fffffff, true));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0f57346^!)  
[src/hydrogen-bce.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-bce.cc?cl=0f57346)  
[test/mjsunit/regress/regress-bce-underflow.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-bce-underflow.js?cl=0f57346)  
  
  
---   

## **regress-468162.js (chromium issue)**  
   
**[Issue: usage of asm.js causes calculation errors](https://crbug.com/468162)**  
**[Commit: [turbofan] Fix lowering of Math.max for integral inputs.](https://chromium.googlesource.com/v8/v8/+/ff89876)**  
  
Date(Commit): Fri Mar 20 12:05:19 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.1, Via-Wizard, merge-merged-4.2, ReleaseBlock-Stable, M-42  
Code Review: [https://codereview.chromium.org/1027753002](https://codereview.chromium.org/1027753002)  
Regress: [mjsunit/compiler/regress-468162.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-468162.js)  
```javascript
var asm = (function() {
  "use asm";
  var max = Math.max;
  return function f() { return max(0, -17); };
})();

assertEquals(0, asm());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ff89876^!)  
[src/compiler/js-builtin-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-builtin-reducer.cc?cl=ff89876)  
[test/mjsunit/compiler/regress-468162.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-468162.js?cl=ff89876)  
[test/unittests/compiler/js-builtin-reducer-unittest.cc](https://cs.chromium.org/chromium/src/v8/test/unittests/compiler/js-builtin-reducer-unittest.cc?cl=ff89876)  
  

---   

## **regress-469089.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::VerifyPointersVisitor::VisitPointers](https://crbug.com/469089)**  
**[Commit: [turbofan] Work-around untagged result of CompareIC in pointer maps.](https://chromium.googlesource.com/v8/v8/+/d5893ca)**  
  
Date(Commit): Fri Mar 20 09:45:12 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1026683002](https://codereview.chromium.org/1026683002)  
Regress: [mjsunit/compiler/regress-469089.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-469089.js)  
```javascript
(function() {
  var __v_6 = false;
  function f(val, idx) {
    if (idx === 1) {
      gc();
      __v_6 = (val === 0);
    }
  }
  f(.1, 1);
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d5893ca^!)  
[src/compiler/arm/linkage-arm.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm/linkage-arm.cc?cl=d5893ca)  
[src/compiler/arm64/linkage-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/arm64/linkage-arm64.cc?cl=d5893ca)  
[src/compiler/ia32/linkage-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ia32/linkage-ia32.cc?cl=d5893ca)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=d5893ca)  
[src/compiler/linkage-impl.h](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage-impl.h?cl=d5893ca)  
...  
  

---   

## **regress-crbug-465671-null.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/parser.cc,](https://crbug.com/465671)**  
**[Commit: Parser: Fix crash on stack overflow when lazy-parsing arrow functions](https://chromium.googlesource.com/v8/v8/+/3c3ce1b)**  
  
Date(Commit): Fri Mar 20 00:17:50 2015  
Components: Blink>JavaScript>Language, Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1023483003](https://codereview.chromium.org/1023483003)  
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
  

---   

## **regress-468727.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/code-generator.cc,](https://crbug.com/468727)**  
**[Commit: [turbofan] Remember types for deoptimization during simplified lowering.](https://chromium.googlesource.com/v8/v8/+/b7dc9c5)**  
  
Date(Commit): Thu Mar 19 14:00:33 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1015423002](https://codereview.chromium.org/1015423002)  
Regress: [mjsunit/compiler/regress-468727.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-468727.js)  
```javascript
function f() {
  var __v_7 = -126 - __v_3;
  var __v_17 = ((__v_15 & __v_14) != 4) | 16;
  if (__v_17) {
    var __v_11 = 1 << __v_7;
  }
  __v_12 >>= __v_3;
}

assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b7dc9c5^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=b7dc9c5)  
[src/compiler/common-operator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.cc?cl=b7dc9c5)  
[src/compiler/common-operator.h](https://cs.chromium.org/chromium/src/v8/src/compiler/common-operator.h?cl=b7dc9c5)  
[src/compiler/instruction-selector.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.cc?cl=b7dc9c5)  
[src/compiler/instruction-selector.h](https://cs.chromium.org/chromium/src/v8/src/compiler/instruction-selector.h?cl=b7dc9c5)  
...  
  

---   

## **regress-3969.js (v8 issue)**  
   
**[Issue: Permission denied](https://crbug.com/v8/3969)**  
**[Commit: Add regression test for dependency to field type tracked weak map.](https://chromium.googlesource.com/v8/v8/+/f289311)**  
  
Date(Commit): Thu Mar 19 08:51:29 2015  
Code Review: [https://codereview.chromium.org/1019223002](https://codereview.chromium.org/1019223002)  
Regress: [mjsunit/regress/regress-3969.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3969.js)  
```javascript
function Inner() {
  this.property = "OK";
  this.o2 = 1;
}

function Outer(inner) {
  this.inner = inner;
}

var inner = new Inner();
var outer = new Outer(inner);

Outer.prototype.boom = function() {
  return this.inner.property;
};

%PrepareFunctionForOptimization(Outer.prototype.boom);
assertEquals("OK", outer.boom());
assertEquals("OK", outer.boom());
%OptimizeFunctionOnNextCall(Outer.prototype.boom);
assertEquals("OK", outer.boom());

inner = undefined;
%SetAllocationTimeout(0 /*interval*/, 2 /*timeout*/);
delete outer.inner;

outer = new Outer({field: 1.51, property: "OK"});

assertEquals("OK", outer.boom());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f289311^!)  
[test/mjsunit/regress/regress-3969.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3969.js?cl=f289311)  
  

---   

## **regress-crbug-467047.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/frames.cc,](https://crbug.com/467047)**  
**[Commit: Delegate throwing in RegExpExecStub to CEntryStub.](https://chromium.googlesource.com/v8/v8/+/86b391e)**  
  
Date(Commit): Tue Mar 17 15:49:40 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/1012103002](https://codereview.chromium.org/1012103002)  
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
...  
  

---   

## **regress-crbug-467531.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::AppendUse](https://crbug.com/467531)**  
**[Commit: [turbofan] Fix C++ evaluation order in AstGraphBuilder.](https://chromium.googlesource.com/v8/v8/+/7e8a62e)**  
  
Date(Commit): Tue Mar 17 12:37:07 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1015683002](https://codereview.chromium.org/1015683002)  
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
    // re-throw
  }
}, ReferenceError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e8a62e^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=7e8a62e)  
[test/mjsunit/regress/regress-crbug-467531.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-467531.js?cl=7e8a62e)  
  

---   

## **regress-467481.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::base::NoBarrier_Load](https://crbug.com/467481)**  
**[Commit: Bugfix in hydrogen GVN.](https://chromium.googlesource.com/v8/v8/+/ddfca2b)**  
  
Date(Commit): Mon Mar 16 13:46:20 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.1, Stability-Memory-AddressSanitizer, M-41, Security_Impact-Stable, merge-merged-4.2, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/1009933002](https://codereview.chromium.org/1009933002)  
Regress: [mjsunit/regress/regress-467481.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-467481.js)  
```javascript
function f(a1, a2) {
  var v7 = a2[0];
  var v8 = a1[0];
  a2[0] = 0.3;
};
%PrepareFunctionForOptimization(f);
v6 = new Array(1);
v6[0] = "tagged";
f(v6, [1]);
v5 = new Array(1);
v5[0] = 0.1;
f(v5, v5);
v5 = new Array(10);
f(v5, v5);
%OptimizeFunctionOnNextCall(f);
f(v5, v5);
v5[0];  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ddfca2b^!)  
[src/hydrogen.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen.cc?cl=ddfca2b)  
[test/mjsunit/regress/regress-467481.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-467481.js?cl=ddfca2b)  
  

---   

## **regress-460917.js (chromium issue)**  
   
**[Issue: OOB write in v8 due to elements kind confusion](https://crbug.com/460917)**  
**[Commit: Incorrect handling of HTransitionElementsKind in hydrogen check elimination phase fixed.](https://chromium.googlesource.com/v8/v8/+/0902b5f)**  
  
Date(Commit): Thu Mar 12 11:44:29 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.1, Stability-Crash, Release-2-M41, CVE-2015-1242, reward-500, M-41, Security_Impact-Stable, merge-merged-4.2, Security_Severity-High, allpublic, M-42, CVE_description-submitted  
Code Review: [https://codereview.chromium.org/1000893003](https://codereview.chromium.org/1000893003)  
Regress: [mjsunit/regress/regress-460917.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-460917.js)  
```javascript
function boom(a1, a2) {
  // Do something with a2 that needs a map check (for DOUBLE_ELEMENTS).
  var s = a2[0];
  // Emit a load that transitions a1 to PACKED_ELEMENTS.
  var t = a1[0];
  // Emit a store to a2 that assumes DOUBLE_ELEMENTS.
  // The map check is considered redundant and will be eliminated.
  a2[0] = 0.3;
}

;
%PrepareFunctionForOptimization(boom);
var fast_elem = new Array(1);
fast_elem[0] = 'tagged';
boom(fast_elem, [1]);

var double_elem = new Array(1);
double_elem[0] = 0.1;
boom(double_elem, double_elem);

double_elem = new Array(10);
double_elem[0] = 0.1;

%OptimizeFunctionOnNextCall(boom);
boom(double_elem, double_elem);

assertEquals(0.3, double_elem[0]);
assertEquals(undefined, double_elem[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0902b5f^!)  
[src/hydrogen-check-elimination.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-check-elimination.cc?cl=0902b5f)  
[src/hydrogen-instructions.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.h?cl=0902b5f)  
[test/mjsunit/regress/regress-460917.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-460917.js?cl=0902b5f)  
  

---   

## **regress-crbug-465564.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::ExternalReferenceEncoder::Encode](https://crbug.com/465564)**  
**[Commit: Add test case for serializing external references to runtime functions.](https://chromium.googlesource.com/v8/v8/+/3ed5dea)**  
  
Date(Commit): Tue Mar 10 10:36:16 2015  
Components: None  
Labels: Te-Logged, M-43, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/996603002](https://codereview.chromium.org/996603002)  
Regress: [mjsunit/regress/regress-crbug-465564.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-crbug-465564.js)  
```javascript
assertTrue(%StringLessThan("a", "b"));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3ed5dea^!)  
[test/mjsunit/regress/regress-crbug-465564.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-465564.js?cl=3ed5dea)  
  

---   

## **regress-463028.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::MarkCompactCollector::PrepareThreadForCodeFlushing](https://crbug.com/463028)**  
**[Commit: [turbofan] Fix the deopt ids in assignment.](https://chromium.googlesource.com/v8/v8/+/9b40c5d)**  
  
Date(Commit): Fri Mar 06 12:50:47 2015  
Components: Blink  
Labels: Te-Logged, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/987733003](https://codereview.chromium.org/987733003)  
Regress: [mjsunit/regress/regress-463028.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-463028.js)  
```javascript
var o = {}
Object.defineProperty(o, "z", {
    set: function() {
      %DeoptimizeFunction(f);
    },
});

function f(o) {
  return 19 + (void(o.z = 12));
}

f(o);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9b40c5d^!)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=9b40c5d)  
[test/mjsunit/regress/regress-463028.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-463028.js?cl=9b40c5d)  
  

---   

## **regress-3938.js (v8 issue)**  
   
**[Issue: const bindings in for loop: invalid assignment in next expression](https://crbug.com/v8/3938)**  
**[Commit: [es6] Fix for-const loops](https://chromium.googlesource.com/v8/v8/+/054989b)**  
  
Date(Commit): Tue Mar 03 18:34:40 2015  
Code Review: [https://codereview.chromium.org/977543002](https://codereview.chromium.org/977543002)  
Regress: [mjsunit/es6/regress/regress-3938.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3938.js)  
```javascript
'use strict';

assertThrows(function() { for (const i = 0; ; i++) {} }, TypeError);
assertThrows("'use strict'; for (const i = 0; ; i++) {}", TypeError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/054989b^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=054989b)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=054989b)  
[test/mjsunit/es6/regress/regress-3938.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-3938.js?cl=054989b)  
[test/mjsunit/harmony/block-const-assign.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/block-const-assign.js?cl=054989b)  
  

---   

## **regress-463056.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::mark](https://crbug.com/463056)**  
**[Commit: [turbofan] Fix deferred replacement in simplified lowering.](https://chromium.googlesource.com/v8/v8/+/f0b1187)**  
  
Date(Commit): Mon Mar 02 12:49:49 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/966423002](https://codereview.chromium.org/966423002)  
Regress: [mjsunit/compiler/regress-463056.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-463056.js)  
```javascript
function f() {
 return ((0%0)&1) + (1>>>(0%0));
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f0b1187^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=f0b1187)  
[test/mjsunit/compiler/regress-463056.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-463056.js?cl=f0b1187)  
  

---   

## **regress-crbug-461520.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/runtime/runtime-scopes.cc,](https://crbug.com/461520)**  
**[Commit: Remove effectful assertion](https://chromium.googlesource.com/v8/v8/+/68c8073)**  
  
Date(Commit): Wed Feb 25 15:34:21 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/955973003](https://codereview.chromium.org/955973003)  
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
  

---   

## **regress-3902.js (v8 issue)**  
   
**[Issue: Unsafe %GeneratorFuntion% intrinsic cannot be denied](https://crbug.com/v8/3902)**  
**[Commit: Make generator constructors configurable](https://chromium.googlesource.com/v8/v8/+/4c082b5)**  
  
Date(Commit): Thu Feb 19 11:30:33 2015  
Code Review: [https://codereview.chromium.org/939953002](https://codereview.chromium.org/939953002)  
Regress: [mjsunit/es6/regress/regress-3902.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3902.js)  
```javascript
function* g() {}
assertTrue(Object.getOwnPropertyDescriptor(g.__proto__, "constructor").configurable);
assertTrue(Object.getOwnPropertyDescriptor(g.prototype.__proto__, "constructor").configurable);

function FakeGeneratorFunctionConstructor() {}
Object.defineProperty(g.__proto__, "constructor", {value: FakeGeneratorFunctionConstructor});
assertSame(g.__proto__.constructor, FakeGeneratorFunctionConstructor);

function FakeGeneratorObjectConstructor() {}
Object.defineProperty(g.prototype.__proto__, "constructor", {value: FakeGeneratorObjectConstructor});
assertSame(g.prototype.__proto__.constructor, FakeGeneratorObjectConstructor);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c082b5^!)  
[src/generator.js](https://cs.chromium.org/chromium/src/v8/src/generator.js?cl=4c082b5)  
[test/mjsunit/es6/regress/regress-3902.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/es6/regress/regress-3902.js?cl=4c082b5)  
  

---   

## **regress-459955.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/typer.cc,](https://crbug.com/459955)**  
**[Commit: [turbofan] Fix typing of comparisons.](https://chromium.googlesource.com/v8/v8/+/9951e1e)**  
  
Date(Commit): Thu Feb 19 10:56:23 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/943483002](https://codereview.chromium.org/943483002)  
Regress: [mjsunit/regress/regress-459955.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-459955.js)  
```javascript
function f(x) {
 var v;
 if (x) v = 0;
 return v <= 1;
}
assertFalse(f(false));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9951e1e^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=9951e1e)  
[test/mjsunit/regress/regress-459955.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-459955.js?cl=9951e1e)  
  

---   

## **regress-430201.js (chromium issue)**  
   
**[Issue: UNKNOWN in SizeFromMap](https://crbug.com/430201)**  
**[Commit: Unlink pages from the space page list after evacuation.](https://chromium.googlesource.com/v8/v8/+/206e913)**  
  
Date(Commit): Thu Feb 19 09:28:59 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, Stability-Memory-AddressSanitizer, M-41, Clusterfuzz, Merge-merged-2272  
Code Review: [https://codereview.chromium.org/937833002](https://codereview.chromium.org/937833002)  
Regress: [mjsunit/regress/regress-430201.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-430201.js), [mjsunit/regress/regress-430201b.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-430201b.js)  
```javascript
var array_1 = [];

for (var a = 0; a < 10000; a++) { array_1[a * 100] = 0; }

gc();
gc();

var array_2 = [];
for (var i = 0; i < 321361; i++) {
  array_2[i] = String.fromCharCode(i)[0];
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/206e913^!)  
[src/heap/mark-compact.cc](https://cs.chromium.org/chromium/src/v8/src/heap/mark-compact.cc?cl=206e913)  
[src/heap/spaces.cc](https://cs.chromium.org/chromium/src/v8/src/heap/spaces.cc?cl=206e913)  
[test/mjsunit/regress/regress-430201.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-430201.js?cl=206e913)  
  

---   

## **regress-457935.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/lookup.cc,](https://crbug.com/457935)**  
**[Commit: Convert to immutable heap number when materializing arguments object.](https://chromium.googlesource.com/v8/v8/+/3f3558f)**  
  
Date(Commit): Tue Feb 17 18:08:59 2015  
Components: Blink>JavaScript  
Labels: Te-Logged, Needs-triage, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/935623002](https://codereview.chromium.org/935623002)  
Regress: [mjsunit/regress/regress-457935.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-457935.js)  
```javascript
function dummy(x) {};

function g() {
  return g.arguments;
}

function f(limit) {
  var i = 0;
  var o = {};
  for (; i < limit; i++) {
    o.y = +o.y;
    g();
  }
};
%PrepareFunctionForOptimization(f);
f(1);
f(1);
%OptimizeFunctionOnNextCall(f);
dummy(f(1));
dummy(f(2));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3f3558f^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=3f3558f)  
[test/mjsunit/regress/regress-457935.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-457935.js?cl=3f3558f)  
  

---   

## **regress-3884.js (v8 issue)**  
   
**[Issue: turbofan seems to have incorrect pointer map populated for regress-builtinbust-7](https://crbug.com/v8/3884)**  
**[Commit: Fix representation for CompareIC in JSGenericLowering.](https://chromium.googlesource.com/v8/v8/+/22dd6dc)**  
  
Date(Commit): Tue Feb 17 16:37:36 2015  
Code Review: [https://codereview.chromium.org/933913002](https://codereview.chromium.org/933913002)  
Regress: [mjsunit/regress/regress-3884.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3884.js)  
```javascript
function f(x) {
  // TurboFan will hoist the CompareIC for x === 'some_string' and spill it.
  if (x === 'some_other_string_1' || x === 'some_string') {
    gc();
  }
  if (x === 'some_other_string_2' || x === 'some_string') {
    gc();
  }
  // TurboFan will hoist the CompareIC for x === 1.4 and spill it.
  if (x === 1.7 || x === 1.4) {
    gc();
  }
  if (x === 1.9 || x === 1.4) {
    gc();
  }
};
%PrepareFunctionForOptimization(f);
%OptimizeFunctionOnNextCall(f);

f('some_other_string_1');
f(1.7);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/22dd6dc^!)  
[src/compiler/js-generic-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-generic-lowering.cc?cl=22dd6dc)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=22dd6dc)  
[src/runtime/runtime-object.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-object.cc?cl=22dd6dc)  
[src/runtime/runtime.h](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime.h?cl=22dd6dc)  
[test/cctest/compiler/test-run-jscalls.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-run-jscalls.cc?cl=22dd6dc)  
...  
  

---   

## **regress-arg-materialize-store.js (other issue)**  
   
**[Commit: During arguments materialization, do not store materialized objects without lazy deopt.](https://chromium.googlesource.com/v8/v8/+/0a4047a)**  
  
Date(Commit): Tue Feb 17 15:24:34 2015  
Code Review: [https://codereview.chromium.org/919173003](https://codereview.chromium.org/919173003)  
Regress: [mjsunit/regress/regress-arg-materialize-store.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-arg-materialize-store.js)  
```javascript
function f() {
  return f.arguments;
}

function g(deopt) {
  var o = {x: 2};
  f();
  o.x = 1;
  deopt + 0;
  return o.x;
};
%PrepareFunctionForOptimization(g);
g(0);
g(0);
%OptimizeFunctionOnNextCall(g);
assertEquals(1, g({}));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0a4047a^!)  
[src/deoptimizer.cc](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.cc?cl=0a4047a)  
[src/deoptimizer.h](https://cs.chromium.org/chromium/src/v8/src/deoptimizer.h?cl=0a4047a)  
[test/mjsunit/regress/regress-arg-materialize-store.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-arg-materialize-store.js?cl=0a4047a)  
  
  
---   

## **regress-458876.js (chromium issue)**  
   
**[Issue: Use-of-uninitialized-value in v8::internal::compiler::Schedule::block](https://crbug.com/458876)**  
**[Commit: [turbofan] Fix control reducer with re-reducing branches.](https://chromium.googlesource.com/v8/v8/+/c5f7d2b)**  
  
Date(Commit): Mon Feb 16 14:56:49 2015  
Components: None  
Labels: Merge-na, Reward-1000, External-Fuzzer-Contribution, Security_Severity-Medium, Security_Impact-Head, Stability-Memory-MemorySanitizer, allpublic, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/917383004](https://codereview.chromium.org/917383004)  
Regress: [mjsunit/regress/regress-458876.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-458876.js)  
```javascript
function module() {
    "use asm";
    function foo() {
      do ; while (foo ? 0 : 1) ;
      return -1 > 0 ? -1 : 0;
    }
    return foo;
}

var foo = module();
assertEquals(0, foo());
assertEquals(0, foo());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c5f7d2b^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=c5f7d2b)  
[src/compiler/graph-visualizer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/graph-visualizer.cc?cl=c5f7d2b)  
[test/mjsunit/regress/regress-458876.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-458876.js?cl=c5f7d2b)  
  

---   

## **regress-458987.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/isolate.cc,](https://crbug.com/458987)**  
**[Commit: [turbofan] Clear pending exception from unsuccessful compilation.](https://chromium.googlesource.com/v8/v8/+/d075894)**  
  
Date(Commit): Mon Feb 16 14:25:23 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/932603002](https://codereview.chromium.org/932603002)  
Regress: [mjsunit/regress/regress-458987.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-458987.js)  
```javascript
(function () {
  "use asm";

  function g() {}

  runNearStackLimit(g);
})();

function runNearStackLimit(f) {
  function g() { try { g(); } catch(e) { f(); } };
  g();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d075894^!)  
[src/compiler.cc](https://cs.chromium.org/chromium/src/v8/src/compiler.cc?cl=d075894)  
[test/mjsunit/regress/regress-458987.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-458987.js?cl=d075894)  
  

---   

## **regress-to-number-binop-deopt.js (other issue)**  
   
**[Commit: [turbofan] Avoid ToNumber conversions if they could deoptimize.](https://chromium.googlesource.com/v8/v8/+/0b8063c)**  
  
Date(Commit): Mon Feb 16 12:59:20 2015  
Code Review: [https://codereview.chromium.org/922623002](https://codereview.chromium.org/922623002)  
Regress: [mjsunit/compiler/regress-to-number-binop-deopt.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-to-number-binop-deopt.js)  
```javascript
function deopt(f) {
  return { valueOf : function() { %DeoptimizeFunction(f); return 1.1; } };
}

function or_zero(o) {
  return o|0;
}

function multiply_one(o) {
  return +o;
}

function multiply_one_symbol() {
  return +Symbol();
}

assertThrows(multiply_one_symbol, TypeError);
assertEquals(1, or_zero(deopt(or_zero)));
assertEquals(1.1, multiply_one(deopt(multiply_one)));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0b8063c^!)  
[src/compiler/js-typed-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-typed-lowering.cc?cl=0b8063c)  
[test/mjsunit/compiler/regress-to-number-binop-deopt.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-to-number-binop-deopt.js?cl=0b8063c)  
  
  
---   

## **regress-454725.js (chromium issue)**  
   
**[Issue: Fatal error in ..\..\v8\src/heap/mark-compact.h,](https://crbug.com/454725)**  
**[Commit: Use just one to-space page for the promotion queue.](https://chromium.googlesource.com/v8/v8/+/c889fb4)**  
  
Date(Commit): Wed Feb 11 13:39:40 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/919473008](https://codereview.chromium.org/919473008)  
Regress: [mjsunit/regress/regress-454725.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-454725.js)  
```javascript
var __v_9 = {};
var depth = 15;
var current = 0;

function __f_15(__v_3) {
  if ((__v_3 % 50) != 0) {
    return __v_3;
  } else {
    return __v_9 + 0.5;
  }
}
function __f_13(a) {
  a[100000 - 2] = 1;
  for (var __v_3= 0; __v_3 < 70000; ++__v_3 ) {
    a[__v_3] = __f_15(__v_3);
  }
}
function __f_2(size) {

}
var tmp;
function __f_18(allocator) {
  current++;
  if (current == depth) return;
  var __v_7 = new allocator(100000);
  __f_13(__v_7);
  var __v_4 = 6;
  for (var __v_3= 0; __v_3 < 70000; __v_3 += 501 ) {
    tmp += __v_3;
  }
  __f_18(Array);
  current--;
}

gc();
__f_18(__f_2);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/c889fb4^!)  
[src/heap/heap-inl.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap-inl.h?cl=c889fb4)  
[src/heap/heap.cc](https://cs.chromium.org/chromium/src/v8/src/heap/heap.cc?cl=c889fb4)  
[src/heap/heap.h](https://cs.chromium.org/chromium/src/v8/src/heap/heap.h?cl=c889fb4)  
[test/mjsunit/regress/regress-454725.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-454725.js?cl=c889fb4)  
  

---   

## **regress-typedarray-out-of-bounds.js (other issue)**  
   
**[Commit: fix transition of typedarrays in ics](https://chromium.googlesource.com/v8/v8/+/9896fab)**  
  
Date(Commit): Mon Feb 09 09:50:15 2015  
Code Review: [https://codereview.chromium.org/904423002](https://codereview.chromium.org/904423002)  
Regress: [mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js)  
```javascript
var a = new Int32Array(10);
function f(a) { a["-1"] = 15; }
for (var i = 0; i < 3; i++) {
  f(a);
}
assertEquals(undefined, a[-1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9896fab^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=9896fab)  
[src/lookup.cc](https://cs.chromium.org/chromium/src/v8/src/lookup.cc?cl=9896fab)  
[test/mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-typedarray-out-of-bounds.js?cl=9896fab)  
  
  
---   

## **regress-crbug-455644.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::AppendUse](https://crbug.com/455644)**  
**[Commit: Fix try-finally for dead AST-branches in TurboFan.](https://chromium.googlesource.com/v8/v8/+/df986d0)**  
  
Date(Commit): Thu Feb 05 12:29:33 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/880443004](https://codereview.chromium.org/880443004)  
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
  

---   

## **regress-3865.js (v8 issue)**  
   
**[Issue: Hydrogen produces invalid code](https://crbug.com/v8/3865)**  
**[Commit: Fix HConstant(double, ...) constructor](https://chromium.googlesource.com/v8/v8/+/bfe7f4a)**  
  
Date(Commit): Thu Feb 05 10:28:13 2015  
Code Review: [https://codereview.chromium.org/897263002](https://codereview.chromium.org/897263002)  
Regress: [mjsunit/regress/regress-3865.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3865.js)  
```javascript
function bar() {
  var radix = 10;
  return 21 / radix | 0;
};
%PrepareFunctionForOptimization(bar);
assertEquals(2, bar());
assertEquals(2, bar());
%OptimizeFunctionOnNextCall(bar);
assertEquals(2, bar());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bfe7f4a^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=bfe7f4a)  
[test/mjsunit/regress/regress-3865.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3865.js?cl=bfe7f4a)  
  

---   

## **regress-455212.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/utils.h,](https://crbug.com/455212)**  
**[Commit: templates: Don't check IsLineTerminator() if character is negative](https://chromium.googlesource.com/v8/v8/+/49ef549)**  
  
Date(Commit): Wed Feb 04 21:05:48 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/902703002](https://codereview.chromium.org/902703002)  
Regress: [mjsunit/regress/regress-455212.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-455212.js)  
```javascript
assertThrows("\u0060\u005c", SyntaxError);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/49ef549^!)  
[src/scanner.cc](https://cs.chromium.org/chromium/src/v8/src/scanner.cc?cl=49ef549)  
[test/mjsunit/regress/regress-455212.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-455212.js?cl=49ef549)  
  

---   

## **regress-455141.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::FullCodeGenerator::EmitVariableLoad](https://crbug.com/455141)**  
**[Commit: Fix assertion in full codegen for holed 'this'.](https://chromium.googlesource.com/v8/v8/+/275e088)**  
  
Date(Commit): Wed Feb 04 12:14:33 2015  
Components: Blink>JavaScript>Language, Internals  
Labels: Te-Logged, Hotlist-Recharge, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/902563002](https://codereview.chromium.org/902563002)  
Regress: [mjsunit/es6/regress/regress-455141.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-455141.js)  
```javascript
"use strict";
class Base {
}
class Subclass extends Base {
  constructor() {
      this.prp1 = 3;
  }
}
function __f_1(){
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/275e088^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=275e088)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=275e088)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=275e088)  
[src/x64/full-codegen-x64.cc](https://cs.chromium.org/chromium/src/v8/src/x64/full-codegen-x64.cc?cl=275e088)  
[test/mjsunit/harmony/regress/regress-455141.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/harmony/regress/regress-455141.js?cl=275e088)  
  

---   

## **regress-449291.js (chromium issue)**  
   
**[Issue: Global-buffer-overflow in v8::internal::MarkCompactCollector::EmptyMarkingDeque](https://crbug.com/449291)**  
**[Commit: Infer HConstant::NotInNewSpace only if the supplied handle is null.](https://chromium.googlesource.com/v8/v8/+/4f786be)**  
  
Date(Commit): Tue Feb 03 17:48:35 2015  
Components: Blink>JavaScript  
Labels: ReleaseBlock-Beta, Merge-na, Stability-Memory-AddressSanitizer, Nag, Security_Impact-Head, Security_Severity-High, allpublic, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/898753003](https://codereview.chromium.org/898753003)  
Regress: [mjsunit/regress/regress-449291.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-449291.js)  
```javascript
a = {
  y: 1.5
};
a.y = 1093445778;
b = a.y;
c = {
  y: {}
};

function f() {
  return {y: b};
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(f().y, 1093445778);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4f786be^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=4f786be)  
[test/mjsunit/regress/regress-449291.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-449291.js?cl=4f786be)  
  

---   

## **regress-3859.js (v8 issue)**  
   
**[Issue: Init collections from iterable & SameValueZero](https://crbug.com/v8/3859)**  
**[Commit: Compute the same hash for all NaN values.](https://chromium.googlesource.com/v8/v8/+/f6e02e1)**  
  
Date(Commit): Tue Feb 03 06:29:18 2015  
Code Review: [https://codereview.chromium.org/897593002](https://codereview.chromium.org/897593002)  
Regress: [mjsunit/regress/regress-3859.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-3859.js)  
```javascript
assertEquals(1, new Set([NaN, NaN, NaN]).size);
assertEquals(42, new Map([[NaN, 42]]).get(NaN));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/f6e02e1^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=f6e02e1)  
[test/mjsunit/regress/regress-3859.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3859.js?cl=f6e02e1)  
  

---   

## **regress-crbug-450960.js (chromium issue)**  
   
**[Issue: CHECK(!isolate->has_pending_exception()) failed: ../../v8/src/compiler.cc(9](https://crbug.com/450960)**  
**[Commit: Clear pending exception on stack overflow in the parser](https://chromium.googlesource.com/v8/v8/+/9cce4ff)**  
  
Date(Commit): Tue Feb 03 06:22:36 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/858213003](https://codereview.chromium.org/858213003)  
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
  

---   

## **regress-crbug-454091.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/handles.h,](https://crbug.com/454091)**  
**[Commit: Check global object behind global proxy for extensibility](https://chromium.googlesource.com/v8/v8/+/1de7dff)**  
  
Date(Commit): Mon Feb 02 12:49:12 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/895573002](https://codereview.chromium.org/895573002)  
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
  

---   

## **regress-453481.js (chromium issue)**  
   
**[Issue: CHECK(obj->IsTypeFeedbackVector()) failed: ../../v8/src/type-feedback-vecto](https://crbug.com/453481)**  
**[Commit: CallIC used an invalid mechanism to detect if it was in optimized code.](https://chromium.googlesource.com/v8/v8/+/3df0a9a)**  
  
Date(Commit): Fri Jan 30 15:07:14 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/885333002](https://codereview.chromium.org/885333002)  
Regress: [mjsunit/regress/regress-453481.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-453481.js)  
```javascript
var __v_0 = "";
var __v_1 = {};
var __v_2 = {};
var __v_3 = {};
var __v_4 = {};
var __v_5 = {};
var __v_6 = {};
var __v_7 = {};
var __v_8 = {};
var __v_10 = {};
var __v_13 = {};
var __v_15 = {};
var __v_16 = /abc/;
var __v_17 = {};
var __v_18 = function() {};
var __v_19 = this;
var __v_20 = {};
var __v_21 = this;

function __f_5(s) {
  return __f_11(__f_3(__f_7(s), s.length * 8));
}
function __f_3(x, len) {
  var __v_3 =  1732584193;
  var __v_6 = -271733879;
  var __v_5 = -1732584194;
  var __v_7 =  271733892;

  for (var i = 0; i < 1; i++) {
    var __v_11 = __v_3;
    var __v_14 = __v_6;
    var __v_13 = __v_5;
    var __v_15 = __v_7;

    __v_3 = __f_10(__v_3, __v_6, __v_5, __v_7, x[__v_8+ 0], 6 , -198630844);
    __v_7 = __f_10(__v_7, __v_3, __v_6, __v_5, x[__v_8+ 7], 10,  1126891415);
    __v_5 = __f_10(__v_5, __v_7, __v_3, __v_6, x[__v_8+14], 15, -1416354905);
    __v_6 = __f_10(__v_6, __v_5, __v_7, __v_3, x[__v_8+ 5], 21, -57434055);
    __v_3 = __f_10(__v_3, __v_6, __v_5, __v_7, x[__v_8+12], 6 ,  1700485571);
    __v_7 = __f_10(__v_7, __v_3, __v_6, __v_5, x[__v_8+ 3], 10, -1894986606);
    __v_5 = __f_10(__v_5, __v_7, __v_3, __v_6, x[__v_8+10], 15, -1051523);
    __v_6 = __f_10(__v_6, __v_5, __v_7, __v_3, x[__v_8+ 1], 21, -2054922799);
    __v_3 = __f_10(__v_3, __v_6, __v_5, __v_7, x[__v_8+ 8], 6 ,  1873313359);
    __v_7 = __f_10(__v_7, __v_3, __v_6, __v_5, x[__v_8+15], 10, -30611744);
    __v_5 = __f_10(__v_5, __v_7, __v_3, __v_6, x[__v_8+ 22], 14, -1560198371);
    __v_3 = __f_10(__v_3, __v_6, __v_5, __v_7, x[__v_8+ 4], 6 , -145523070);
    __v_7 = __f_10(__v_7, __v_3, __v_6, __v_5, x[__v_8+11], 10, -1120210379);
    __v_5 = __f_10(__v_5, __v_7, __v_3, __v_6, x[__v_8+ 2], 15,  718787259);
    __v_6 = __f_10(__v_13, __v_5, __v_7, __v_3, x[__v_8+ 9], 21, -343485551);
    __v_3 = __f_6(__v_3, __v_11);
    __v_6 = __f_6(__v_6, __v_14);
    __v_5 = __f_6(__v_5, __v_13);
    __v_7 = __f_6(__v_7, __v_15);

  }

  return Array(__v_3, __v_13, __v_4, __v_19);
}
function __f_4(q, __v_3, __v_6, x, s, t) {
  return __f_6(__f_12(__f_6(__f_6(__v_3, q), __f_6(x, t)), s),__v_6);
}
function __f_13(__v_3, __v_6, __v_5, __v_7, x, s, t) {
  return __f_4((__v_6 & __v_5) | ((~__v_6) & __v_7), __v_3, __v_6, x, s, t);
}
function __f_8(__v_3, __v_6, __v_5, __v_7, x, s, t) {
  return __f_4((__v_6 & __v_7) | (__v_5 & (~__v_7)), __v_3, __v_6, x, s, t);
}
function __f_9(__v_3, __v_6, __v_5, __v_7, x, s, t) {
  return __f_4(__v_6 ^ __v_5 ^ __v_7, __v_3, __v_6, x, s, t);
}
function __f_10(__v_3, __v_6, __v_5, __v_7, x, s, t) {
  return __f_4(__v_5 ^ (__v_6 | (~__v_7)), __v_3, __v_6, x, s, t);
}
function __f_6(x, y) {
  var __v_12 = (x & 0xFFFF) + (y & 0xFFFF);
  var __v_18 = (x >> 16) + (y >> 16) + (__v_12 >> 16);
  return (__v_18 << 16) | (__v_12 & 0xFFFF);
}
function __f_12(num, cnt) {
  return (num << cnt) | (num >>> (32 - cnt));
}
function __f_7(__v_16) {
  var __v_4 = Array();
  var __v_9 = (1 << 8) - 1;
  for(var __v_8 = 0; __v_8 < __v_16.length * 8; __v_8 += 8)
    __v_4[__v_8>>5] |= (__v_16.charCodeAt(__v_8 / 8) & __v_9) << (__v_8%32);
  return __v_4;
}

function __f_11(binarray) { return __v_16; }

try {
__v_10 = "Rebellious subjects, enemies to peace,\n\
Profaners of this neighbour-stained steel,--\n\
Will they not hear? What, ho! you men, you beasts,\n\
That quench the fire of your pernicious rage\n\
With purple fountains issuing from your veins,\n\
On pain of torture, from those bloody hands\n\
Throw your mistemper'__v_7 weapons to the ground,\n\
And hear the sentence of your moved prince.\n\
Three civil brawls, bred of an airy word,\n\
By thee, old Capulet, and Montague,\n\
Have thrice disturb'__v_7 the quiet of our streets,\n\
And made Verona's ancient citizens\n\
Cast by their grave beseeming ornaments,\n\
To wield old partisans, in hands as old,\n\
Canker'__v_7 with peace, to part your canker'__v_7 hate:\n\
If ever you disturb our streets again,\n\
Your lives shall pay the forfeit of the peace.\n\
For this time, all the rest depart away:\n\
You Capulet; shall go along with me:\n\
And, Montague, come you this afternoon,\n\
To know our further pleasure in this case,\n\
To old Free-town, our common judgment-place.\n\
Once more, on pain of death, all men depart.\n"
  function assertEquals(a, b) { }
for (var __v_8 = 0; __v_8 < 11; ++__v_8) {
  assertEquals(__f_5(__v_10), "1b8719c72d5d8bfd06e096ef6c6288c5");
}

} catch(e) { print("Caught: " + e); }  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/3df0a9a^!)  
[src/ic/ic.cc](https://cs.chromium.org/chromium/src/v8/src/ic/ic.cc?cl=3df0a9a)  
[test/mjsunit/regress/regress-453481.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-453481.js?cl=3df0a9a)  
  

---   

## **regress-deoptimize-constant-keyed-load.js (other issue)**  
   
**[Commit: Always emit bailout id for inlining property access (even for keyed access).](https://chromium.googlesource.com/v8/v8/+/da90aab)**  
  
Date(Commit): Fri Jan 30 14:35:43 2015  
Code Review: [https://codereview.chromium.org/887023003](https://codereview.chromium.org/887023003)  
Regress: [mjsunit/regress/regress-deoptimize-constant-keyed-load.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-deoptimize-constant-keyed-load.js)  
```javascript
var o = {};
o.__defineGetter__('progressChanged', function() {
  %DeoptimizeFunction(f);
  return 10;
});

function g(a, b, c) {
  return a + b + c;
}

function f() {
  var t = 'progressChanged';
  return g(1, o[t], 100);
};
%PrepareFunctionForOptimization(f);
f();
f();
%OptimizeFunctionOnNextCall(f);
assertEquals(111, f());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/da90aab^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=da90aab)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=da90aab)  
[src/ia32/full-codegen-ia32.cc](https://cs.chromium.org/chromium/src/v8/src/ia32/full-codegen-ia32.cc?cl=da90aab)  
[src/mips/full-codegen-mips.cc](https://cs.chromium.org/chromium/src/v8/src/mips/full-codegen-mips.cc?cl=da90aab)  
[src/mips64/full-codegen-mips64.cc](https://cs.chromium.org/chromium/src/v8/src/mips64/full-codegen-mips64.cc?cl=da90aab)  
...  
  
  
---   

## **regress-437713.js (chromium issue)**  
   
**[Issue: Permission denied](https://crbug.com/437713)**  
**[Commit: Layout descriptor sharing issue fixed.](https://chromium.googlesource.com/v8/v8/+/32fe247)**  
  
Date(Commit): Fri Jan 30 12:55:25 2015  
Components: None  
Labels: None  
Code Review: [https://codereview.chromium.org/885003002](https://codereview.chromium.org/885003002)  
Regress: [mjsunit/regress/regress-437713.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-437713.js)  
```javascript
var o1 = {
  a00:0, a01:0, a02:0, a03:0, a04:0, a05:0, a06:0, a07:0, a08:0, a09:0, a0a:0, a0b:0, a0c:0, a0d:0, a0e:0, a0f:0,
  a10:0, a11:0, a12:0, a13:0, a14:0, a15:0, a16:0, a17:0, a18:0, a19:0, a1a:0, a1b:0, a1c:0, a1d:0, a1e:0, a1f:0,

  dbl: 0.1,

  some_double: 2.13,
};

var o2 = {
  a00:0, a01:0, a02:0, a03:0, a04:0, a05:0, a06:0, a07:0, a08:0, a09:0, a0a:0, a0b:0, a0c:0, a0d:0, a0e:0, a0f:0,
  a10:0, a11:0, a12:0, a13:0, a14:0, a15:0, a16:0, a17:0, a18:0, a19:0, a1a:0, a1b:0, a1c:0, a1d:0, a1e:0, a1f:0,

  dbl: 0.1,

  boom: [],
};

o2.boom.push(42);
assertEquals(42, o2.boom[0]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/32fe247^!)  
[src/layout-descriptor.cc](https://cs.chromium.org/chromium/src/v8/src/layout-descriptor.cc?cl=32fe247)  
[src/layout-descriptor.h](https://cs.chromium.org/chromium/src/v8/src/layout-descriptor.h?cl=32fe247)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=32fe247)  
[test/cctest/test-unboxed-doubles.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/test-unboxed-doubles.cc?cl=32fe247)  
[test/mjsunit/regress/regress-437713.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-437713.js?cl=32fe247)  
  

---   

## **regress-3501.js (v8 issue)**  
   
**[Issue: CHECK(!already_resolved()) failed when parsing let/const arrow function expression](https://crbug.com/v8/3501)**  
**[Commit: Do not create unresolved variables when parsing arrow functions lazily](https://chromium.googlesource.com/v8/v8/+/91b87e7)**  
  
Date(Commit): Thu Jan 29 15:53:15 2015  
Code Review: [https://codereview.chromium.org/880253004](https://codereview.chromium.org/880253004)  
Regress: [mjsunit/es6/regress/regress-3501.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-3501.js)  
```javascript
"use strict";
let lift = f => (x, k) => k (f (x));
lift(isNaN);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/91b87e7^!)  
[src/parser.cc](https://cs.chromium.org/chromium/src/v8/src/parser.cc?cl=91b87e7)  
[src/parser.h](https://cs.chromium.org/chromium/src/v8/src/parser.h?cl=91b87e7)  
[test/mjsunit/regress/regress-3501.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-3501.js?cl=91b87e7)  
  

---   

## **regress-446647.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::IC::raw_target](https://crbug.com/446647)**  
**[Commit: [turbofan] Make sure there is space for lazy deopt patching before the constant pool.](https://chromium.googlesource.com/v8/v8/+/a4b163a)**  
  
Date(Commit): Thu Jan 29 15:30:32 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, M-40, Clusterfuzz  
Code Review: [https://codereview.chromium.org/874863003](https://codereview.chromium.org/874863003)  
Regress: [mjsunit/compiler/regress-446647.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-446647.js)  
```javascript
function f(a,b) {
  a%b
};

f({ toString : function() { %DeoptimizeFunction(f); }});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4b163a^!)  
[src/compiler/code-generator.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/code-generator.cc?cl=a4b163a)  
[test/mjsunit/compiler/regress-446647.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-446647.js?cl=a4b163a)  
  

---   

## **regress-416359.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/deoptimizer.cc,](https://crbug.com/416359)**  
**[Commit: [turbofan] Add missing deopt for the assignment in the for-in statement.](https://chromium.googlesource.com/v8/v8/+/489b6f7)**  
  
Date(Commit): Wed Jan 28 16:16:24 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/881303002](https://codereview.chromium.org/881303002)  
Regress: [mjsunit/compiler/regress-416359.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-416359.js)  
```javascript
"use strict"
function f() {
  for (x in {a:0});
}

assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/489b6f7^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=489b6f7)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=489b6f7)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=489b6f7)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=489b6f7)  
[src/compiler/ast-graph-builder.h](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.h?cl=489b6f7)  
...  
  

---   

## **regress-crbug-451770.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::SharedFunctionInfo::code](https://crbug.com/451770)**  
**[Commit: Add missing FrameState to JSToName nodes.](https://chromium.googlesource.com/v8/v8/+/c5833e8)**  
  
Date(Commit): Wed Jan 28 11:40:02 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/880963002](https://codereview.chromium.org/880963002)  
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
...  
  

---   

## **regress-447561.js (chromium issue)**  
   
**[Issue: CHECK(!v8::internal::FLAG_enable_slow_asserts || (object->IsJSRegExp())) fa](https://crbug.com/447561)**  
**[Commit: Land test case for RegExp.source.](https://chromium.googlesource.com/v8/v8/+/1e90546)**  
  
Date(Commit): Tue Jan 27 15:17:37 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.1, Merge-Merged, M-41, Clusterfuzz  
Code Review: [https://codereview.chromium.org/878033003](https://codereview.chromium.org/878033003)  
Regress: [mjsunit/regress/regress-447561.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-447561.js)  
```javascript
__proto__ = /foo/gi;
assertThrows(function() { source });
assertThrows(function() { global });
assertThrows(function() { ignoreCase });
assertThrows(function() { multiline });
assertEquals(0, lastIndex);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/1e90546^!)  
[test/mjsunit/regress/regress-447561.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-447561.js?cl=1e90546)  
  

---   

## **regress-448711.js (chromium issue)**  
   
**[Issue: CHECK(details.representation().Equals(new_representation) || details.repres](https://crbug.com/448711)**  
**[Commit: Do not generalize field representations when making elements kind or observed transition.](https://chromium.googlesource.com/v8/v8/+/7f9b2fa)**  
  
Date(Commit): Tue Jan 27 11:19:06 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/861173004](https://codereview.chromium.org/861173004)  
Regress: [mjsunit/regress/regress-448711.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-448711.js)  
```javascript
function f() {
  this.a = { text: "Hello!" };
}
var v4 = new f();
var v7 = new f();
v7.b = {};
Object.defineProperty(v4, '2', {});
var v6 = new f();
v6.a = {};  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7f9b2fa^!)  
[src/objects.cc](https://cs.chromium.org/chromium/src/v8/src/objects.cc?cl=7f9b2fa)  
[src/objects.h](https://cs.chromium.org/chromium/src/v8/src/objects.h?cl=7f9b2fa)  
[test/mjsunit/regress/regress-448711.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-448711.js?cl=7f9b2fa)  
  

---   

## **regress-452427.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/representation-change.h,](https://crbug.com/452427)**  
**[Commit: [turbofan] Only replace nodes eagerly during simplified lowering if the types stay the same.](https://chromium.googlesource.com/v8/v8/+/b4a4c4c)**  
  
Date(Commit): Tue Jan 27 09:27:37 2015  
Components: Blink>JavaScript  
Labels: Hotlist-Merge-Review, M-41, Clusterfuzz  
Code Review: [https://codereview.chromium.org/871373010](https://codereview.chromium.org/871373010)  
Regress: [mjsunit/compiler/regress-452427.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-452427.js)  
```javascript
var stdlib = {};
var foreign = {};
var heap = new ArrayBuffer(64 * 1024);

var rol = (function Module(stdlib, foreign, heap) {
  "use asm";
  function rol() {
    y = "a" > false;
    return y + (1 - y);
  }
  return { rol: rol };
})(stdlib, foreign, heap).rol;

assertEquals(1, rol());  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/b4a4c4c^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=b4a4c4c)  
[test/mjsunit/compiler/regress-452427.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-452427.js?cl=b4a4c4c)  
  

---   

## **regress-451012.js (chromium issue)**  
   
**[Issue: CHECK(result.upper->Maybe(Type::Internal())) failed: ../../v8/src/compiler/](https://crbug.com/451012)**  
**[Commit: [turbofan] Handle cyclic dependencies in context typing.](https://chromium.googlesource.com/v8/v8/+/4c79f55)**  
  
Date(Commit): Tue Jan 27 06:57:41 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/874983002](https://codereview.chromium.org/874983002)  
Regress: [mjsunit/compiler/regress-451012.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-451012.js)  
```javascript
"use strict";
function f() {
  for (let v; v; ) {
    let x;
  }
}

f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/4c79f55^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=4c79f55)  
[test/mjsunit/compiler/regress-451012.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-451012.js?cl=4c79f55)  
  

---   

## **regress-451958.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::NodeMarkerBase::Get](https://crbug.com/451958)**  
**[Commit: [turbofan] Simplify reduction if IfTrue and IfFalse and fix bugs.](https://chromium.googlesource.com/v8/v8/+/7c81161b)**  
  
Date(Commit): Mon Jan 26 16:11:24 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/880533002](https://codereview.chromium.org/880533002)  
Regress: [mjsunit/regress/regress-451958.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-451958.js)  
```javascript
function k() { throw "e"; }
var a = true;
var a = false;
function foo(a) {
  var i, j;
  if (a) {
    for (i = 0; i < 1; j++) ;
    for (i = 0; i < 1; k()) ;
    for (i = 0; i < 1; i++) ;
  }
}
%PrepareFunctionForOptimization(foo);
%OptimizeFunctionOnNextCall(foo);
foo();

function bar() {
var __v_45;
  for (__v_45 = 0; __v_45 < 64; __v_63++) {
  }
  for (__v_45 = 0; __v_45 < 128; __v_36++) {
  }
  for (__v_45 = 128; __v_45 < 256; __v_45++) {
  }
}
%PrepareFunctionForOptimization(bar);
%OptimizeFunctionOnNextCall(bar);
assertThrows(bar);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7c81161b^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=7c81161b)  
[src/compiler/control-reducer.h](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.h?cl=7c81161b)  
[test/cctest/compiler/test-control-reducer.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-control-reducer.cc?cl=7c81161b)  
[test/mjsunit/regress/regress-451958.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-451958.js?cl=7c81161b)  
  

---   

## **regress-crbug-451013.js (chromium issue)**  
   
**[Issue: CHECK(*deopt_index != Safepoint::kNoDeoptimizationIndex) failed: ../../v8/s](https://crbug.com/451013)**  
**[Commit: Add missing FrameState for Runtime_CreateArrayLiteral.](https://chromium.googlesource.com/v8/v8/+/00f3f99)**  
  
Date(Commit): Mon Jan 26 12:45:34 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/873973003](https://codereview.chromium.org/873973003)  
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
...  
  

---   

## **regress-451322.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/lithium-codegen.cc,](https://crbug.com/451322)**  
**[Commit: Fixed Hydrogen environment handling for mul-i on ARM and ARM64.](https://chromium.googlesource.com/v8/v8/+/a7d67a6)**  
  
Date(Commit): Mon Jan 26 08:35:58 2015  
Components: Blink  
Labels: Te-Logged, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/873703002](https://codereview.chromium.org/873703002)  
Regress: [mjsunit/regress/regress-451322.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-451322.js)  
```javascript
var foo = 0;

function bar() {
  var baz = 0 - {};
  if (foo > 24) return baz * 0;
};
%PrepareFunctionForOptimization(bar);
bar();
bar();
%OptimizeFunctionOnNextCall(bar);
bar();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a7d67a6^!)  
[src/arm/lithium-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/lithium-arm.cc?cl=a7d67a6)  
[src/arm64/lithium-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-arm64.cc?cl=a7d67a6)  
[test/mjsunit/regress/regress-451322.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-451322.js?cl=a7d67a6)  
  

---   

## **regress-crbug-451016.js (chromium issue)**  
   
**[Issue: CHECK(IsBootstrappingOrGlobalObject(this->GetIsolate(), result)) failed: ..](https://crbug.com/451016)**  
**[Commit: Avoid unintentional optimization of hot builtins by TurboFan.](https://chromium.googlesource.com/v8/v8/+/d2e424a)**  
  
Date(Commit): Thu Jan 22 18:52:15 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/817293005](https://codereview.chromium.org/817293005)  
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
  

---   

## **regress-450895.js (chromium issue)**  
   
**[Issue: Unreachable code in ../../v8/src/runtime/runtime-array.cc(856)](https://crbug.com/450895)**  
**[Commit: Support concatenating with zero-size arrays with DICTIONARY_ELEMENTS in Runtime_ArrayConcat.](https://chromium.googlesource.com/v8/v8/+/8ccc696)**  
  
Date(Commit): Thu Jan 22 11:15:30 2015  
Components: None  
Labels: Te-Logged, Clusterfuzz, M-42  
Code Review: [https://codereview.chromium.org/849693003](https://codereview.chromium.org/849693003)  
Regress: [mjsunit/regress/regress-450895.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-450895.js)  
```javascript
var v = new Array();
Object.freeze(v);
v = v.concat(0.5);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/8ccc696^!)  
[src/runtime/runtime-array.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-array.cc?cl=8ccc696)  
[test/mjsunit/regress/regress-450895.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-450895.js?cl=8ccc696)  
  

---   

## **regress-crbug-450642.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Code::deoptimization_data](https://crbug.com/450642)**  
**[Commit: Add missing BailoutId and FrameState to with statements.](https://chromium.googlesource.com/v8/v8/+/558efe2)**  
  
Date(Commit): Thu Jan 22 10:57:42 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Security_Impact-None, Security_Severity-High, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/865833002](https://codereview.chromium.org/865833002)  
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
...  
  

---   

## **regress-undefined-nan3.js (other issue)**  
   
**[Commit: Double field values need sNaN -> qNaN canonicalization.](https://chromium.googlesource.com/v8/v8/+/0381acf)**  
  
Date(Commit): Thu Jan 22 08:36:12 2015  
Code Review: [https://codereview.chromium.org/863153002](https://codereview.chromium.org/863153002)  
Regress: [mjsunit/regress/regress-undefined-nan3.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan3.js)  
```javascript
var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper();
i_view[1] = %GetHoleNaNLower();
var f_view = new Float64Array(ab);

var fixed_double_elements = new Float64Array(1);
fixed_double_elements[0] = f_view[0];

function A(src) {
  this.x = src[0];
};
%PrepareFunctionForOptimization(A);
new A(fixed_double_elements);
new A(fixed_double_elements);

%OptimizeFunctionOnNextCall(A);

var obj = new A(fixed_double_elements);

function move_x(dst, obj) {
  dst[0] = obj.x;
};
%PrepareFunctionForOptimization(move_x);
var doubles = [0.5];
move_x(doubles, obj);
move_x(doubles, obj);
%OptimizeFunctionOnNextCall(move_x);
move_x(doubles, obj);
assertTrue(doubles[0] !== undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0381acf^!)  
[src/hydrogen-instructions.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-instructions.cc?cl=0381acf)  
[test/mjsunit/regress/regress-undefined-nan.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-undefined-nan.js?cl=0381acf)  
[test/mjsunit/regress/regress-undefined-nan3.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-undefined-nan3.js?cl=0381acf)  
  
  
---   

## **regress-undefined-nan2.js (other issue)**  
   
**[Commit: [arm] Fix sNaN quietening in the ARM simulator on IA-32.](https://chromium.googlesource.com/v8/v8/+/ee86227)**  
  
Date(Commit): Wed Jan 21 13:01:23 2015  
Code Review: [https://codereview.chromium.org/802243004](https://codereview.chromium.org/802243004)  
Regress: [mjsunit/regress/regress-undefined-nan2.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan2.js)  
```javascript
function foo(a, i) {
  var o = [0.5,,1];
  a[i] = o[i];
}
var a1 = [0.1,0.1];
foo(a1, 0);
foo(a1, 1);
assertEquals(undefined, a1[1]);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ee86227^!)  
[src/arm/simulator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/simulator-arm.cc?cl=ee86227)  
[test/mjsunit/regress/regress-undefined-nan2.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-undefined-nan2.js?cl=ee86227)  
  
  
---   

## **regress-undefined-nan.js (other issue)**  
   
**[Commit: Use signaling NaN for holes in fixed double arrays.](https://chromium.googlesource.com/v8/v8/+/9eace97)**  
  
Date(Commit): Wed Jan 21 08:52:25 2015  
Code Review: [https://codereview.chromium.org/863633002](https://codereview.chromium.org/863633002)  
Regress: [mjsunit/regress/regress-undefined-nan.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-undefined-nan.js)  
```javascript
function loader(dst, src, i) {
  dst[i] = src[i];
};
%PrepareFunctionForOptimization(loader);
var ab = new ArrayBuffer(8);
var i_view = new Int32Array(ab);
i_view[0] = %GetHoleNaNUpper();
i_view[1] = %GetHoleNaNLower();
var f_view = new Float64Array(ab);

var fixed_double_elements = new Float64Array(1);

function opt_store() {
  fixed_double_elements[0] = f_view[0];
};
%PrepareFunctionForOptimization(opt_store);
opt_store();
opt_store();
%OptimizeFunctionOnNextCall(opt_store);
opt_store();

var i32 = new Int32Array(fixed_double_elements.buffer);
assertEquals(i_view[0], i32[0]);
assertEquals(i_view[1], i32[1]);

var doubles = [0.5];
loader(doubles, fixed_double_elements, 0);
loader(doubles, fixed_double_elements, 0);
%OptimizeFunctionOnNextCall(loader);
loader(doubles, fixed_double_elements, 0);
assertTrue(doubles[0] !== undefined);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/9eace97^!)  
[build/toolchain.gypi](https://cs.chromium.org/chromium/src/v8/build/toolchain.gypi?cl=9eace97)  
[src/arm/simulator-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/simulator-arm.cc?cl=9eace97)  
[src/arm64/lithium-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/lithium-codegen-arm64.cc?cl=9eace97)  
[src/assembler.cc](https://cs.chromium.org/chromium/src/v8/src/assembler.cc?cl=9eace97)  
[src/assembler.h](https://cs.chromium.org/chromium/src/v8/src/assembler.h?cl=9eace97)  
...  
  
  
---   

## **regress-crbug-448730.js (chromium issue)**  
   
**[Issue: CHECK(field_type_.IsHeapObject()) failed: ../../v8/src/hydrogen.cc(6092)](https://crbug.com/448730)**  
**[Commit: Add proper support for proxies to HType.](https://chromium.googlesource.com/v8/v8/+/e1d878d)**  
  
Date(Commit): Wed Jan 14 13:57:09 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/847373002](https://codereview.chromium.org/847373002)  
Regress: [mjsunit/es6/regress/regress-crbug-448730.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/es6/regress/regress-crbug-448730.js)  
```javascript
function bar() {}
bar({ a: new Proxy({}, {}) });
function foo(x) { x.a.b == ""; }
var x = {a: {b: "" }};
%PrepareFunctionForOptimization(foo);
foo(x);
foo(x);
%OptimizeFunctionOnNextCall(foo);
foo(x);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/e1d878d^!)  
[src/hydrogen-types.cc](https://cs.chromium.org/chromium/src/v8/src/hydrogen-types.cc?cl=e1d878d)  
[src/hydrogen-types.h](https://cs.chromium.org/chromium/src/v8/src/hydrogen-types.h?cl=e1d878d)  
[test/mjsunit/regress/regress-crbug-448730.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-crbug-448730.js?cl=e1d878d)  
  

---   

## **regress-445907.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::FixedArray::get](https://crbug.com/445907)**  
**[Commit: [turbofan] Allow deoptimization for JSToNumber operator.](https://chromium.googlesource.com/v8/v8/+/ac04d77)**  
  
Date(Commit): Wed Jan 14 13:09:32 2015  
Components: Blink>JavaScript  
Labels: M-39, Stability-Memory-AddressSanitizer, Security_Impact-Stable, Security_Severity-Medium, allpublic, Clusterfuzz  
Code Review: [https://codereview.chromium.org/841443004](https://codereview.chromium.org/841443004)  
Regress: [mjsunit/compiler/regress-445907.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445907.js)  
```javascript
v = [];
v.length = (1 << 30);

function f() {
  v++;
}

assertThrows(f);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/ac04d77^!)  
[src/arm/full-codegen-arm.cc](https://cs.chromium.org/chromium/src/v8/src/arm/full-codegen-arm.cc?cl=ac04d77)  
[src/arm64/full-codegen-arm64.cc](https://cs.chromium.org/chromium/src/v8/src/arm64/full-codegen-arm64.cc?cl=ac04d77)  
[src/ast.h](https://cs.chromium.org/chromium/src/v8/src/ast.h?cl=ac04d77)  
[src/compiler/ast-graph-builder.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/ast-graph-builder.cc?cl=ac04d77)  
[src/compiler/change-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/change-lowering.cc?cl=ac04d77)  
...  
  

---   

## **regress-3812.js (v8 issue)**  
   
**[Issue: Truncation analysis for ToBoolean is wrong](https://crbug.com/v8/3812)**  
**[Commit: [turbofan] Fix truncation/representation sloppiness wrt. bool/bit.](https://chromium.googlesource.com/v8/v8/+/70b32e4)**  
  
Date(Commit): Wed Jan 14 12:06:56 2015  
Code Review: [https://codereview.chromium.org/850013003](https://codereview.chromium.org/850013003)  
Regress: [mjsunit/compiler/regress-3812.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-3812.js)  
```javascript
var stdlib = this;
var buffer = new ArrayBuffer(64 * 1024);
var foreign = {}

var foo = (function Module(stdlib, foreign, heap) {
  "use asm";
  function foo(i) {
    var x = i ? (i&1) : true;
    if (x) return x;
    return false;
  }
  return {foo:foo};
})(stdlib, foreign, buffer).foo;

assertEquals(1, foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/70b32e4^!)  
[src/compiler/change-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/change-lowering.cc?cl=70b32e4)  
[src/compiler/change-lowering.h](https://cs.chromium.org/chromium/src/v8/src/compiler/change-lowering.h?cl=70b32e4)  
[src/compiler/js-graph.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.cc?cl=70b32e4)  
[src/compiler/js-graph.h](https://cs.chromium.org/chromium/src/v8/src/compiler/js-graph.h?cl=70b32e4)  
[src/compiler/opcodes.h](https://cs.chromium.org/chromium/src/v8/src/compiler/opcodes.h?cl=70b32e4)  
...  
  

---   

## **regress-447567.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::JSFunction::shared](https://crbug.com/447567)**  
**[Commit: [turbofan] Add missing deopt.](https://chromium.googlesource.com/v8/v8/+/527e19a)**  
  
Date(Commit): Tue Jan 13 08:40:54 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Security_Severity-Low, Security_Impact-Stable, allpublic, Clusterfuzz, Owner-Triage  
Code Review: [https://codereview.chromium.org/809463005](https://codereview.chromium.org/809463005)  
Regress: [mjsunit/compiler/regress-447567.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-447567.js)  
```javascript
assertThrows(function () {
  Object.freeze(new Int8Array(1))
});

assertThrows(function() {
  "use strict";
  const v = 42;
  v += 1;
});  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/527e19a^!)  
[src/compiler/linkage.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/linkage.cc?cl=527e19a)  
[test/mjsunit/compiler/regress-447567.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-447567.js?cl=527e19a)  
  

---   

## **regress-447756.js (chromium issue)**  
   
**[Issue: CHECK(IsUint32Double(value)) failed: ../../v8/src/compiler/representation-c](https://crbug.com/447756)**  
**[Commit: Map -0 to integer 0 for typed array constructors.](https://chromium.googlesource.com/v8/v8/+/a4124b3)**  
  
Date(Commit): Mon Jan 12 11:42:57 2015  
Components: Blink>JavaScript  
Labels: merge-merged-4.1, M-41, Arch-All, Clusterfuzz  
Code Review: [https://codereview.chromium.org/790813005](https://codereview.chromium.org/790813005)  
Regress: [mjsunit/regress/regress-447756.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-447756.js)  
```javascript
function TestConstructor(c) {
  var a = new c(-0);
  assertSame(Infinity, 1 / a.length);
  assertSame(Infinity, 1 / a.byteLength);

  var ab = new ArrayBuffer(-0);
  assertSame(Infinity, 1 / ab.byteLength);

  var a1 = new c(ab, -0, -0);
  assertSame(Infinity, 1 / a1.length);
  assertSame(Infinity, 1 / a1.byteLength);
  assertSame(Infinity, 1 / a1.byteOffset);
}

var constructors =
  [ Uint8Array, Int8Array, Uint8ClampedArray,
    Uint16Array, Int16Array,
    Uint32Array, Int32Array,
    Float32Array, Float64Array ];
for (var i = 0; i < constructors.length; i++) {
  TestConstructor(constructors[i]);
}


function TestOptimizedCode() {
  var a = new Uint8Array(-0);
  assertSame(Infinity, 1 / a.length);
  assertSame(Infinity, 1 / a.byteLength);

  var ab = new ArrayBuffer(-0);
  assertSame(Infinity, 1 / ab.byteLength);

  var a1 = new Uint8Array(ab, -0, -0);
  assertSame(Infinity, 1 / a1.length);
  assertSame(Infinity, 1 / a1.byteLength);
  assertSame(Infinity, 1 / a1.byteOffset);
}

%OptimizeFunctionOnNextCall(Uint8Array);
for (var i = 0; i < 1000; i++) {
  TestOptimizedCode();
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a4124b3^!)  
[src/runtime.js](https://cs.chromium.org/chromium/src/v8/src/runtime.js?cl=a4124b3)  
[test/mjsunit/regress/regress-447756.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-447756.js?cl=a4124b3)  
  

---   

## **regress-447526.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::Node::mark](https://crbug.com/447526)**  
**[Commit: [turbofan] Fix control reducer for degenerate cases of self-loop branches.](https://chromium.googlesource.com/v8/v8/+/7e98658)**  
  
Date(Commit): Fri Jan 09 12:28:14 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/828823006](https://codereview.chromium.org/828823006)  
Regress: [mjsunit/regress/regress-447526.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-447526.js)  
```javascript
function bar() {
  throw "done";
}

function foo() {
  var i;
  while (i) {
    while (i) {
}
    i++;
  }
  while (true) {
    bar();
  }
}
%PrepareFunctionForOptimization(foo);


%OptimizeFunctionOnNextCall(foo);
assertThrows(foo);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/7e98658^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=7e98658)  
[test/mjsunit/regress/regress-447526.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-447526.js?cl=7e98658)  
  

---   

## **regress-bit-number-constant.js (other issue)**  
   
**[Commit: [turbofan] Fix bit representation of NumberConstant.](https://chromium.googlesource.com/v8/v8/+/d1c1a3c)**  
  
Date(Commit): Wed Jan 07 15:44:22 2015  
Code Review: [https://codereview.chromium.org/839813002](https://codereview.chromium.org/839813002)  
Regress: [mjsunit/compiler/regress-bit-number-constant.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-bit-number-constant.js)  
```javascript
var stdlib = this;
var buffer = new ArrayBuffer(64 * 1024);
var foreign = {}

var foo = (function Module(stdlib, foreign, heap) {
  "use asm";
  function foo(i) {
    return !(i ? 1 : false);
  }
  return {foo:foo};
})(stdlib, foreign, buffer).foo;

assertFalse(foo(1));  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d1c1a3c^!)  
[src/compiler/representation-change.h](https://cs.chromium.org/chromium/src/v8/src/compiler/representation-change.h?cl=d1c1a3c)  
[test/mjsunit/compiler/regress-bit-number-constant.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-bit-number-constant.js?cl=d1c1a3c)  
  
  
---   

## **regress-444805.js (chromium issue)**  
   
**[Issue: CHECK(!has_pending_exception()) failed: ../../v8/src/isolate.cc(1255)](https://crbug.com/444805)**  
**[Commit: Correct handling of exceptions occured during getting of exception stack trace.](https://chromium.googlesource.com/v8/v8/+/0d67858)**  
  
Date(Commit): Wed Jan 07 14:50:16 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/793333003](https://codereview.chromium.org/793333003)  
Regress: [mjsunit/regress/regress-444805.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-444805.js), [mjsunit/regress/regress-444805.js-script](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-444805.js-script)  
```javascript
try {
  load("test/mjsunit/regress/regress-444805.js-script");
} catch (e) {
}  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/0d67858^!)  
[src/api.cc](https://cs.chromium.org/chromium/src/v8/src/api.cc?cl=0d67858)  
[test/mjsunit/regress/regress-444805.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-444805.js?cl=0d67858)  
[test/mjsunit/regress/regress-444805.js-script](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-444805.js-script?cl=0d67858)  
  

---   

## **regress-446389.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::TypeImpl<v8::internal::ZoneTypeConfig>::BitsetType::Lub](https://crbug.com/446389)**  
**[Commit: Fix bug in Runtime_CompileOptimized resulting from stack overflow.](https://chromium.googlesource.com/v8/v8/+/d77d3ba)**  
  
Date(Commit): Wed Jan 07 13:43:44 2015  
Components: Blink>JavaScript>Compiler  
Labels: Stability-Memory-AddressSanitizer, M-41, Clusterfuzz  
Code Review: [https://codereview.chromium.org/844503002](https://codereview.chromium.org/844503002)  
Regress: [mjsunit/regress/regress-446389.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/regress/regress-446389.js)  
```javascript
function runNearStackLimit(f) { function t() { try { t(); } catch(e) { f(); } }; try { t(); } catch(e) {} }
%PrepareFunctionForOptimization(__f_3);
%OptimizeFunctionOnNextCall(__f_3);
function __f_3() {
    var __v_5 = a[0];
}
runNearStackLimit(function() { __f_3(); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/d77d3ba^!)  
[src/runtime/runtime-compiler.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-compiler.cc?cl=d77d3ba)  
[test/mjsunit/regress/regress-446389.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/regress/regress-446389.js?cl=d77d3ba)  
  

---   

## **regress-446778.js (chromium issue)**  
   
**[Issue: CHECK(IsUint32Double(value)) failed: ../../v8/src/compiler/representation-c](https://crbug.com/446778)**  
**[Commit: Restrict representation inference to avoid truncation of phi inputs.](https://chromium.googlesource.com/v8/v8/+/80a7be5)**  
  
Date(Commit): Wed Jan 07 11:38:54 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/837153002](https://codereview.chromium.org/837153002)  
Regress: [mjsunit/compiler/regress-446778.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-446778.js)  
```javascript
function Module() {
  "use asm";
  function f() {
   var i = (140737463189505);
   do {
    i = i + i | 0;
    x = undefined + i | 0;
   } while (!i);
  }
  return { f: f };
}

Module().f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/80a7be5^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=80a7be5)  
[test/cctest/compiler/test-simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/test/cctest/compiler/test-simplified-lowering.cc?cl=80a7be5)  
[test/mjsunit/compiler/regress-446778.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-446778.js?cl=80a7be5)  
  

---   

## **regress-445876.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::compiler::BasicBlock::id](https://crbug.com/445876)**  
**[Commit: Make control reducer revisit newly introduced merges.](https://chromium.googlesource.com/v8/v8/+/a9716d9)**  
  
Date(Commit): Mon Jan 05 16:35:34 2015  
Components: Blink>JavaScript  
Labels: Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/830293003](https://codereview.chromium.org/830293003)  
Regress: [mjsunit/compiler/regress-445876.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445876.js)  
```javascript
function f(x) {
  while (1) { s++; }
  while (x) { s++; }
}

assertThrows(function () { f(1); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a9716d9^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=a9716d9)  
[test/mjsunit/compiler/regress-445876.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-445876.js?cl=a9716d9)  
  

---   

## **regress-446156.js (chromium issue)**  
   
**[Issue: Unreachable code in ../../v8/src/compiler/typer.cc(1626)](https://crbug.com/446156)**  
**[Commit: [turbofan] Don't crash when typing load from a Uint8ClampedArray.](https://chromium.googlesource.com/v8/v8/+/17a1808)**  
  
Date(Commit): Mon Jan 05 13:43:47 2015  
Components: Blink>JavaScript  
Labels: Clusterfuzz  
Code Review: [https://codereview.chromium.org/835883003](https://codereview.chromium.org/835883003)  
Regress: [mjsunit/compiler/regress-446156.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-446156.js)  
```javascript
(function Module(stdlib, foreign, heap) {
  "use asm";
  // This is not valid asm.js, but should nevertheless work.
  var MEM = new Uint8ClampedArray(heap);
  function foo(  )  { MEM[0] ^=  1; }
  return {foo: foo};
})(this, {}, new ArrayBuffer(  ) ).foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/17a1808^!)  
[src/compiler/typer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/typer.cc?cl=17a1808)  
[test/mjsunit/compiler/regress-446156.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-446156.js?cl=17a1808)  
  

---   

## **regress-ntl-effect.js (other issue)**  
   
**[Commit: Do not reduce effect phis for loops.](https://chromium.googlesource.com/v8/v8/+/bdf446f)**  
  
Date(Commit): Sat Jan 03 12:46:00 2015  
Code Review: [https://codereview.chromium.org/830923002](https://codereview.chromium.org/830923002)  
Regress: [mjsunit/compiler/regress-ntl-effect.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-ntl-effect.js)  
```javascript
function g() {
  throw 0;
}

function f() {
  g();
  while (1) {}
}

assertThrows(function () { f(); });  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/bdf446f^!)  
[src/compiler/control-reducer.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/control-reducer.cc?cl=bdf446f)  
[test/mjsunit/compiler/regress-ntl-effect.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-ntl-effect.js?cl=bdf446f)  
  
  
---   

## **regress-445859.js (chromium issue)**  
   
**[Issue: Fatal error in ../../v8/src/compiler/representation-change.h,](https://crbug.com/445859)**  
**[Commit: [turbofan] Truncation of Bit/Word8/16 to Word32 is a no-op.](https://chromium.googlesource.com/v8/v8/+/fb2643c)**  
  
Date(Commit): Fri Jan 02 10:39:10 2015  
Components: Blink>JavaScript  
Labels: merge-merged-3.31, Stability-Memory-AddressSanitizer, Clusterfuzz  
Code Review: [https://codereview.chromium.org/828313002](https://codereview.chromium.org/828313002)  
Regress: [mjsunit/compiler/regress-445859.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445859.js)  
```javascript
var foo = (function Module(global, env, buffer) {
  "use asm";
  var i8 = new global.Int8Array(buffer);
  function foo() { i8[0] += 4294967295; }
  return { foo: foo };
})(this, {}, new ArrayBuffer(64 * 1024)).foo;
foo();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/fb2643c^!)  
[src/compiler/simplified-lowering.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/simplified-lowering.cc?cl=fb2643c)  
[test/mjsunit/compiler/regress-445859.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-445859.js?cl=fb2643c)  
  

---   

## **regress-445858.js (chromium issue)**  
   
**[Issue: UNKNOWN in v8::internal::Invoke](https://crbug.com/445858)**  
**[Commit: [x64] Rearrange code for OOB integer loads.](https://chromium.googlesource.com/v8/v8/+/cf866b7)**  
  
Date(Commit): Fri Jan 02 10:15:40 2015  
Components: Blink>JavaScript  
Labels: merge-merged-3.31, Stability-Memory-AddressSanitizer, Arch-x86_64, Clusterfuzz  
Code Review: [https://codereview.chromium.org/828303002](https://codereview.chromium.org/828303002)  
Regress: [mjsunit/compiler/regress-445858.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445858.js)  
```javascript
var foo = (function module(stdlib, foreign, heap) {
  "use asm";
  var MEM = new stdlib.Int8Array(heap);
  function foo(i) {
    i = i|0;
    i[0] = i;
    return MEM[i + 1 >> 0]|0;
  }
  return { foo: foo };
})(this, {}, new ArrayBuffer(64 * 1024)).foo;
foo(-1);  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/cf866b7^!)  
[src/compiler/x64/code-generator-x64.cc](https://cs.chromium.org/chromium/src/v8/src/compiler/x64/code-generator-x64.cc?cl=cf866b7)  
[test/mjsunit/compiler/regress-445858.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-445858.js?cl=cf866b7)  
  

---   

## **regress-445732.js (chromium issue)**  
   
**[Issue: CHECK(reason != kNoReason) failed: ../../v8/src/objects.cc(10436)](https://crbug.com/445732)**  
**[Commit: Fix %NeverOptimizeFunction() intrinsic.](https://chromium.googlesource.com/v8/v8/+/a64ac45)**  
  
Date(Commit): Fri Jan 02 08:18:01 2015  
Components: Blink>JavaScript  
Labels: merge-merged-3.31, Arch-All, Clusterfuzz  
Code Review: [https://codereview.chromium.org/832003002](https://codereview.chromium.org/832003002)  
Regress: [mjsunit/compiler/regress-445732.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/compiler/regress-445732.js)  
```javascript
"use asm";

%NeverOptimizeFunction(f);
function f() { }
f();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/a64ac45^!)  
[src/runtime/runtime-test.cc](https://cs.chromium.org/chromium/src/v8/src/runtime/runtime-test.cc?cl=a64ac45)  
[test/mjsunit/compiler/regress-445732.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/compiler/regress-445732.js?cl=a64ac45)  
  

---   
