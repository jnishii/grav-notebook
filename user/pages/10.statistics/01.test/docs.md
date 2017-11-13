---
title:  Rで検定
---


## LOCALEの設定
```
$ defaults write org.R-project.R force.LANG en_US.UTF-8  # 英語表示
$ defaults write org.R-project.R force.LANG ja_JP.UTF-8  # 日本語表示
```

## データの読み込み

```
m <- scan(“math.cv”)
p <- scan(“physics.cv”)
```

1. 平均値の差の検定
2群間の平均値の差の検定なら，ガウス分布であるか否かによらずWelch's t-testを使う。

- [Graeme D. Ruxton, "The unequal variance t-test is an underused alternative to Student's t-test and the Mann–Whitney U test", Behavioral Ecology (July/August 2006) 17 (4): 688-690.
doi: 10.1093/beheco/ark016](https://beheco.oxfordjournals.org/content/17/4/688.full)
> If you want to compare the central tendency of 2 populations based on samples of unrelated data, then the unequal variance t-test should always be used in preference to the Student's t-test or Mann–Whitney U test. To use this test, first examine the distributions of the 2 samples graphically. If there is evidence of nonnormality in either or both distributions, then rank the data. Take the ranked or unranked data and perform an unequal variance t-test. Draw your conclusions on the basis of this test. Note that some packages (e.g., SPSS) perform a Student's t-test and unequal variances t-test simultaneously and provide output for both. The experimenter ought to have decided which test they consider most appropriate beforehand and thus look at the output for that test alone, ignoring the other.

- [t 検定](http://oku.edu.mie-u.ac.jp/~okumura/stat/ttest.html)
> (1標本の平均の検定の場合)データの分布が正規分布かどうかの検定をしてから
t 検定を行う必要はまったくありません
> ...
> 「分散が等しいかどうか不明のときは，分散が等しいことを仮定しない
t 検定（Welch の方法）を使う」というのが現在のベストプラクティスです。
- [二群の平均値（代表値）の差を検定するとき](http://aoki2.si.gunma-u.ac.jp/lecture/BF/index.html)
> 二群の代表値を検定するときで，二群の分散が等しくないときには，等分散を仮定しない t 検定を採用するのが望ましい。

逆に，ちょっと気になる記事。(n=2とか3の場合だけど)
- J.C.F. de Winter, "Using the Student’s t-test with extremely small sample sizes", Practical Assessment, Research & Evaluation, 18(10), 2013
>Summarizing, given infinite time and resources, large samples are always preferred over small samples. Should the applied researcher conduct research with an extremely small sample size (N ≤ 5), the t-test can be applied, as long as the effect size is expected to be large. Also in case of unequal variances, unequal sample sizes, or skewed population distributions, can the t-test be validly applied in an extremely-small-sample scenario (but beware the high false positive rate of the one-sample t-test on non-normal data, and the high false positive rate that may occur for unequal sample sizes combined with unequal variances). A rank- transformation and the Welch test are generally not recommended when working with extremely small samples.

```
t.test(m,p)   # Welch's t-test
```
ただし，二群の等分散性を仮定できるときには次のようにする。

```
t.test(m,p,var.equal=TRUE)   # Welch's t-test
```
1. データはガウス分布であるか(Shapiro-Wilk normality test)
```
	shapiro.test(m)
```
2. ガウス分布なら
```
	var.test(m,p) # F-test
	t.test(m,p)   # t-test
```
3. ガウス分布でないなら
    1. 2つのデータ間の確率分布の相違の検定: Kolmogorov-Smirnov test
```
		ks.test(p)
```

```
install.packages("lawstat")
library(lawstat)
brunner.munzel.test(x,y)
```
