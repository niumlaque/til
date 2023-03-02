## 勉強用記事

https://qiita.com/drken/items/a5e6fe22863b7992efdb

## とりあえず問題を解いて感触をつかんで見る。

#### 勉強用記事の問題 1
```rs
use proconio::input;

fn exec1(n: usize, a: &[i64]) -> i64 {
    use std::cmp;

    let mut dp = vec![0; n + 1];
    
    // 何も選ばない状態、初期値。
    dp[0] = 0;

    for i in 0..n {
        // 現在着目している値 a[i] を選ぶべきかどうか。
        // 今回の場合であれば、a[i] を加算することで dp[i] より大きい値になるならば選ぶべき。
        dp[i + 1] = cmp::max(dp[i], dp[i] + a[i]);
    }

    dp[n]
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn sample1() {
        let actual = exec1(3, &[7, -6, 9]);
        assert_eq!(actual, 16);
    }

    #[test]
    fn sample2() {
        let actual = exec1(2, &[-9, -16]);
        assert_eq!(actual, 0);
    }
}
```
現在の値を使うことで前回に比べてより良い結果になるのであれば現在の値を使って状態を更新し、  
そうでないのであれば前回の値を現在の値にする感じ。


#### 勉強用記事の問題 2
