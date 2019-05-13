# V8Harvest  
The Harvest of V8 regress in 2018.  
  

## **regress-913822.js (chromium issue)**  
   
**[Issue 913822:
 DCHECK failure in !failed_ in asm-parser.cc](https://crbug.com/913822)**  
**[Commit: [asm.js] Fix semicolon insertion in presence of comments.](https://chromium.googlesource.com/v8/v8/+/5f8cd45)**  
  
Date(Commit): Wed Dec 12 14:43:05 2018  
Components/Type: Blink>JavaScript/Bug-Security  
Labels: ["Hotlist-Merge-Review", "Reproducible", "Stability-Memory-AddressSanitizer", "Security_Severity-High", "Security_Impact-Beta", "ReleaseBlock-Stable", "allpublic", "Clusterfuzz", "ClusterFuzz-Verified", "M-72", "Test-Predator-Auto-Components", "merge-merged-7.2"]  
Code Review: [https://chromium-review.googlesource.com/c/1373551](https://chromium-review.googlesource.com/c/1373551)  
Regress: [mjsunit/asm/regress-913822.js](https://chromium.googlesource.com/v8/v8/+/master/test/mjsunit/asm/regress-913822.js)  
```javascript
(function TestNewlineInCPPComment() {
  function Module() {
    "use asm" // Crash by comment!
    function f() {}
    return f
  }
  Module();
  assertTrue(%IsAsmWasmCode(Module));
})();

(function TestNewlineInCComment() {
  function Module() {
    "use asm" /* Crash by
    comment! */ function f() {}
    return f
  }
  Module();
  assertTrue(%IsAsmWasmCode(Module));
})();  
```  
  
[[Diff]](https://chromium.googlesource.com/v8/v8/+/5f8cd45^!)  
[src/asmjs/asm-scanner.cc](https://cs.chromium.org/chromium/src/v8/src/asmjs/asm-scanner.cc?cl=5f8cd45)  
[test/mjsunit/asm/regress-913822.js](https://cs.chromium.org/chromium/src/v8/test/mjsunit/asm/regress-913822.js?cl=5f8cd45)  
  

---   
