1071. Greatest Common Divisor of Strings
For two strings s and t, we say "t divides s" if and only if s = t + t + t + ... + t + t (i.e., t is concatenated with itself one or more times).

Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2.

Constraints:

1 <= str1.length, str2.length <= 1000
str1 and str2 consist of English uppercase letters.

想法:
1. 先讓 str2.size() <= str1.size()
2. 把兩個字串掃描並比較一遍
3. 如題目給的 case, str1 = "ABABAB", str2 = "ABAB", 結果為 "AB"
4. 這時候掃描到的會是 ABAB
5. 所以問題是, 掃描到之後的結果, 需要再掃一次
6. 偶數長度才需要再掃, 因為如果是奇數長度, 無法再度構成 common divisor, 例如結果是 "ABCAB", 這時就不能說 "AB" 是答案
7. 錯了, 上一條的假設錯了, "ABC" 是答案
8. 再重看一次問題, t is concatenated with itself one or more times, 所以對於奇數長度的結果, "ABCAB", 答案就是 "ABCAB", 不能再精簡了
9. 再考慮一個偶數的結果, "ABABAB", 答案是 "AB", 因此, 不是 0 跟 i/2 開始比較 (切一半), 也不是 0 跟偶數的 i 比較
10. 因為這樣比較下去會變成 0,2 0,4 0,6 比較, 如果還比不到, 還會變成 0,3 0,6 0,9 時間複雜度會增加很多
11. 再回到原本的問題, "ABABAB" 的結果要怎麼樣才能取得更精簡的 "AB"? 用 hash? 對於題目而言我就是要找到那個重複的 t

目前經過時間: 15 分

看提示：Math, String

繼續想法:
12. 再想一次剛剛的步驟 10, 現在其實是要把掃描的結果因式分解? 那還是用步驟 10 的想法, 把掃到的結果從 1~(n/2) 都比較看看
13. 這時預測的時間複雜度: 第一次的掃描 O(n), 第二次要把結果再分解, 這一步是 1+2+3+...+(n/2) = 等差數列的和 = (t(s1+s2))/2 = O(n^2)
14. 因此相加是 O(n^2)

目前經過時間: 22 分

看提示: The greatest common divisor must be a prefix of each string, so we can try all prefixes.

繼續想法:
15. 根據提示, 似乎還是需要用 O(n^2) 的解法, 先實作看看結果對不對
16. 實作到一半, 想到另一個 case, str1 = "ABC", str2 = "AB", 答案應該是 "" (null), 所以掃完 "AB" 後, 要接著掃 "C"
17. 暫時休息, 明天再做 (目前經過時間: 27 分)
18. 隔日開始實作 接續昨天進度
19. 實作方向, 第一, 先確定 str2 是否為 str1 的 common divisor, 如果不是, 直接回傳 "" (代表兩個互質) 至此實作驗證確認 ok
20. 第二, str2 要開始因式分解, 每次掃描的 chunk 由 1~(n/2)
21. submit: WA, 目前實作成果如下

```c++
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if (str1.size() < str2.size()) {
            swap(str1, str2);
        }
        string common;
        int p1 = 0;
        int p2 = 0;
        
        while (p1 < str1.size()) {
            if (str1[p1] == str2[p2]) {
                ++p1;
                ++p2;
            } else {
                return "";
            }
            if (p2 >= str2.size()) {
                p2 = 0;
            }
        }

        for (int chunk = 1; chunk <= str2.size() / 2; ++chunk) {
            string sub = str2.substr(0, chunk);
            bool found = true;
            for (int i = 0; i < str2.size(); i += chunk) {
                string compare = str2.substr(i, chunk);
                if (sub != compare) {
                    found = false;
                    break;
                }
            }
            if (found) {
                return sub;
            }
        }
        return str2;
    }
};
```

出錯的 test case:
str1 = "ABABABAB"
str2 = "ABAB"

output: "AB"
expected: "ABAB"

思考錯誤:
1. 第一眼認為 AB 應該沒錯
2. 應該要從 n 計算到 n/2, 這樣才能找到最大的
3. 跑測試程式錯誤, 因為另一個 test case 會出錯：

str1 = "ABABAB"
str2 = "ABAB"

output: "ABAB"
expected: "AB"

4. 因此這兩個 test case 要重新思考

暫時休息, 明天再做 (目前經過時間: 47 分)