--
    title: Levenshtein Distane (莱文斯坦距离) 
    layout: post
--
## Levenshtein Distane (莱文斯坦距离)

Levenshtein距离(LD)是衡量两个字符串之间的相似度,我们将称之为源字符串(s)和目标字符串(t)的距离是删除,插入,或需要替换变换成t。例如,

* 如果s是test，t是test，那么```LD(s, t) = 0```，因为他们之前不需要转换，字符串已经完全相同。
* 如果s是test，t是tent，那么```LD(s, t) = 1```，因为一次替换(将's'替换为'n')就可以将s转换成t。

莱温斯坦距离越大，则表示两个字符串的相似度越小。

Levenshtein distance是以俄罗斯科学家Vladimir Levenshtein，他在1965年设计了这个算法，这个度量值有时也被称为编辑距离（edit distance）

用途：
* 拼写检查(spell checking)
* 语音识别(speech recognition)
* DNA分析（DNA analysis）
* 抄袭检测


### 算法步骤(algorithm)

```
String s;
String t;

int n = s.length()
int m = t.length()
int cost = 0;

```

1. ```n```是字符串```s```的长度，```m```是字符串```t```的长度
    * 如果```n == 0```返回```m```
    * 如果```m == 0```返回```n```
    * 构造一个矩阵，行数(rows)是```0...m```，列数(columns)是```0...n```
2. 初始化第一行: ```0-m```，
    * 初始化第一列: ```0-n```
3. 检查字符串```s```的每个字符(```i = 1; i < n; i++```)
4. 检查字符串```t```的每个字符串(```j = 1; j < n; j++```)
5. 如果```s[i] == t[j]```， ```cost = 0```，否则```cost = 1 ```
6. 设置矩阵```matrix``` 单元格```d[i, j]```的值为下面三种情况的最小值：
    1. 紧接着上面的单元格加一```d[i-1, j]+1```
    2. 左边的单元格加一```d[i, j-1] + 1```
    3. 左上方的单元格加上```cost```：```d[i - 1, j - 1] + cost```
7. 在迭代步骤```(3,4,5,6)```完成之后，在单元格```d[n, m]```中找到距离


### 列子

```
# 两个字符串s 和t

String s = "GUMBO";
String t = “GAMBOL”；
```

#### 步骤1和步骤2


```
 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 |
 A | 2 |
 M | 3 |
 B | 4 |
 O | 5 |
 L | 6 |
 ```

#### 步骤3到步骤6，当i=1
```

 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 | 0 |
 A | 2 | 1
 M | 3 | 2
 B | 4 | 3
 O | 5 | 4
 L | 6 | 5
 
 ```

#### 步骤3到步骤6，当i=2
```

 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 | 0 | 1
 A | 2 | 1 | 1
 M | 3 | 2 | 2
 B | 4 | 3 | 3
 O | 5 | 4 | 4
 L | 6 | 5 | 5

```

#### 步骤3到步骤6，当i=3

```
 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 | 0 | 1 | 2
 A | 2 | 1 | 1 | 2
 M | 3 | 2 | 2 | 1
 B | 4 | 3 | 3 | 2
 O | 5 | 4 | 4 | 3
 L | 6 | 5 | 5 | 4
```

#### 步骤3到步骤6，当i=4

```
 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 | 0 | 1 | 2 | 3
 A | 2 | 1 | 1 | 2 | 3
 M | 3 | 2 | 2 | 1 | 2
 B | 4 | 3 | 3 | 2 | 1
 O | 5 | 4 | 4 | 3 | 2
 L | 6 | 5 | 5 | 4 | 3

```

#### 步骤3到步骤6，当i=5

```
 - | - | G | U | M | B | O |
-- | - | - | - | - | - | - |
 - | 0 | 1 | 2 | 3 | 4 | 5
 G | 1 | 0 | 1 | 2 | 3 | 4
 A | 2 | 1 | 1 | 2 | 3 | 4
 M | 3 | 2 | 2 | 1 | 2 | 3
 B | 4 | 3 | 3 | 2 | 1 | 2
 O | 5 | 4 | 4 | 3 | 2 | 1
 L | 6 | 5 | 5 | 4 | 3 | 2
```

#### 步骤7

得到距离在矩阵的右下角，"GUMBO"可以通过两个步骤得到"GAMBOL"
1. 把```U```替换成```A```
2. 在```O```后面插入```L```

所以他们的距离是```2```

### 实现

* [levenshteinDistance.go](https://github.com/raojinlin/DataStructExercise/blob/master/src/util/levenshteinDistance.go)

#### 参考

* [https://people.cs.pitt.edu/~kirk/cs1501/Pruhs/Spring2006/assignments/editdistance/Levenshtein%20Distance.htm](https://people.cs.pitt.edu/~kirk/cs1501/Pruhs/Spring2006/assignments/editdistance/Levenshtein%20Distance.htm)
