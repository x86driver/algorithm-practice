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