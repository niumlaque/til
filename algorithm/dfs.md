## DFS (深さ優先探索, Depth-first search)

木やグラフを探索するアルゴリズムである。  
経路を行き止まりになるまで進み、期待する結果で無い場合は直前の分岐まで戻り別の経路を探索する。

<a href="https://en.wikipedia.org/wiki/File:Depth-First-Search.gif"><img width="200px" alt="木に対する深さ優先探索のイメージ" src="https://upload.wikimedia.org/wikipedia/commons/7/7f/Depth-First-Search.gif"></a>
* メモリ使用量が少ない
* 最短経路であると保証されない


## 勉強用記事

https://qiita.com/drken/items/4a7869c5e304883f539b

とてもわかり易い。

> グラフ探索は、ネットサーフィンするときのことを思い浮かべるとわかりやすいかもしれません。下図のようなグラフを Web ページのリンク関係を表すものだと考えてみましょう。

しっくりきた。

## 例題
頂点数 $V$、辺の数 $E$ の有向グラフ $G$ について、二頂点 $s, t \in V$ が与えられたとき、
$s$ から $t$ へ到達できるかを確認する。

#### 入力
```
V E
s t
a1 b1
a2 b2
︙
aE bE
```
$V, E, s, t$ に加えて $ai$ から $bi$ への有効辺が $E$ 個与えられる。

#### 出力
$s$ から $t$ へ到達できる場合は `yes` と出力し、到達できない場合は `no` と出力する。


#### 入力例1
```
4 5
0 3
0 1
0 2
1 3
2 1
2 3
```

#### 出力例1
```
yes
```
入力は以下のグラフを表す。
```
0 → 1
↓ ↗ ↓
2 → 3
```
0 から 3 は到達できる。

#### 入力例2
```
10 9
0 6
0 1
0 4
0 8
1 2
2 3
4 5
4 7
5 6
8 9
```

#### 出力例2
```
yes
```
入力は本記事先頭に引用した gif 画像のグラフを表す。  
($s, t, ai, bi$ の値は画像上の頂点の値から -1 されている。)  
(画像は無向グラフだけど上から下への有向グラフとする。)

0 から 6 (画像上は 1 から 7)は到達できる。

#### 入力例3
```
10 9
4 9
0 1
0 4
0 8
1 2
2 3
4 5
4 7
5 6
8 9
```

#### 出力例3
```
no
```
入力は本記事先頭に引用した gif 画像のグラフを表す。  
($s, t, ai, bi$ の値は画像上の頂点の値から -1 されている。)  
(画像は無向グラフだけど上から下への有向グラフとする。)

4 から 9 (画像上は 5 から 10)は到達できない。

#### 実装
```rust
use proconio::input;
fn main() {
    input! {
        v: usize,
        e: usize,
        s: usize,
        t: usize,
    }

    // 正直に (a, b) にするのではなく、
    // 読む段階で頂点 a を始点とする辺に纏めてしまう。
    let mut a: Vec<Vec<usize>> = vec![Vec::new(); v];
    for _ in 0..e {
        input! {
            ai: usize,
            bi: usize,
        }

        a[ai].push(bi);
    }

    // 既に通過した頂点かどうか
    let mut seen: Vec<bool> = vec![false; v];

    let mut stack = Vec::new();

    // s から開始し、
    // 全ての探索を終えて seen[t] が true なら到達可能。
    seen[s] = true;
    stack.push(s);
    'outer: while !stack.is_empty() {
        // 現在値(頂点)
        let current = stack.pop().unwrap();

        // 現在地から 1 歩で到達可能な頂点。
        for item in a[current].iter() {
            let p = *item;
            // ただし、未到達の頂点のみ。
            if !seen[p] {
                seen[p] = true;
                stack.push(p);

                // 全て探索したい場合はこの break を削除。
                if p == t {
                    break 'outer;
                }
            }
        }
    }

    if seen[t] {
        println!("yes");
    } else {
        println!("no");
    }
}
```

#### 入力例1 の 1 ループ毎の変数の値の遷移
0->3
|ループ回数|current|seen|stack|
|--|--|--|--|
|0|-|[true, false, false, false]|[0]|
|1|0|[true, true, true, false]|[1, 2]|
|2|2|[true, true, true, true]|[1, 3]|

#### 入力例2 の 1 ループ毎の変数の値の遷移
0->6
|ループ回数|current|seen|stack|
|--|--|--|--|
|0|-|[true, false, false, false, false, false, false, false, false, false]|[0]|
|1|0|[true, true, false, false, true, false, false, false, true, false]|[1, 4, 8]|
|2|8|[true, true, false, false, true, false, false, false, true, true]|[1, 4, 9]|
|3|9|[true, true, false, false, true, false, false, false, true, true]|[1, 4]|
|4|4|[true, true, false, false, true, true, false, true, true, true]|[1, 5, 7]|
|5|7|[true, true, false, false, true, true, false, true, true, true]|[1, 5]|
|6|5|[true, true, false, false, true, true, true, true, true, true]|[1, 6]|

#### 入力例3 の 1 ループ毎の変数の値の遷移
4->9
|ループ回数|current|seen|stack|
|--|--|--|--|
|0|-|[false, false, false, false, true, false, false, false, false, false]|[4]|
|1|4|[false, false, false, false, true, true, false, true, false, false]|[5, 7]|
|2|7|[false, false, false, false, true, true, false, true, false, false]|[5]|
|3|5|[false, false, false, false, true, true, true, true, false, false]|[6]|
|4|6|[false, false, false, false, true, true, true, true, false, false]|[]|

#### 実装(再帰)
```rs
use proconio::input;
fn main() {
    input! {
        v: usize,
        e: usize,
        s: usize,
        t: usize,
    }

    // 正直に (a, b) にするのではなく、
    // 読む段階で頂点 a を始点とする辺に纏めてしまう。
    let mut a: Vec<Vec<usize>> = vec![Vec::new(); v];
    for _ in 0..e {
        input! {
            ai: usize,
            bi: usize,
        }
        a[ai].push(bi);
    }

    // 既に通過した頂点かどうか
    let mut seen: Vec<bool> = vec![false; v];
    dfs(&a, &mut seen, s);

    if seen[t] {
        println!("yes");
    } else {
        println!("no");
    }
}

fn dfs(a: &[Vec<usize>], seen: &mut [bool], current: usize) {
    seen[current] = true;
    for item in a[current].iter() {
        if seen[*item] {
            continue;
        }

        dfs(a, seen, *item);
    }
}
```
