## #28 实现 strStr()

实现 `strStr()` 函数。

给定一个 `haystack` 字符串和一个 `needle` 字符串，在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  `-1`。

---
### 1. sunday
```
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    const hayLen = haystack.length
    const needleLen = needle.length

    if(needleLen == 0) return 0
    
    function initailOffsetMap () {
        const offsetMap = {}
        for (let i = 0; i < needleLen; i++) {
            offsetMap[needle[i]] = needleLen - i
        }
        return s => offsetMap[s] || needleLen
    }

    let i = 0 // m = needleLen
    while(i + needleLen <= hayLen){
        let temp = 0
        for(let j = 0; j < needleLen; j++){
            if(haystack[i+j] == needle[j]) temp++
            else break
        }

        if(temp == needleLen) return i
        else{
            i += initailOffsetMap()(haystack[i + needleLen])
        }
        
        /**
        *　else{
        *    let k = needleLen - 1
        *    for(; k >= 0; k--){
        *        if(needle[k] == haystack[m])break           
        *    }
        *    i = m - k
        *    m = i + needleLen
        *}
        */
    }
    return -1
    
};
```
---
### 2.双指针

```
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    let a = 0, b = 0
    let neeLen = needle.length,
        hayLen = haystack.length
    if(neeLen == 0) return 0
    while(a < hayLen && b < neeLen){
        if(haystack[a] == needle[b]){
            a ++
            b ++
            if(b == neeLen) return a - neeLen
        }
        else{
            a = a-b+1
            b = 0
        }
    }
    return -1
};
```

---
### 3.KMP（来自leetcode他人题解）
```
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if(!needle) return 0;
    let kmp = new KMP(haystack,needle);
    return kmp.kmpSearch();
}
            class  KMP{
                /*
                    p_str:主字符串
                    c_str:子字符串
                */
                constructor(p_str,c_str){
                    this.p_str = p_str;
                    this.c_str = c_str;
                    this.next = [];
                }
                getNext(){
                    this.next[0] = -1;
                    this.next[1] = 0;
                    let Cn = 0;
                    let i =2;
                    while(i<this.c_str.length){
                        if(this.c_str[i-1]==this.c_str[Cn]){
                            this.next[i++] = ++Cn;
                        }
                        else if(Cn>0){
                            Cn = this.next[Cn];
                        }
                        else{
                            this.next[i++] = 0;
                        }
                    }
                }
                kmpSearch(){
                    this.getNext();
                    let i = 0;
                    let j = 0;
                    while(i<this.p_str.length&&j<this.c_str.length){
                        if(this.p_str[i]==this.c_str[j]){
                            i++;
                            j++;
                        }
                        else if(this.next[j]==-1) i++;
                        else j = this.next[j];
                    }
                    return j==this.c_str.length?i-j:-1;
                }
            }
```
