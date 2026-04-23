# NeetCode 150 カテゴリ別インデックス

学習前に「脳内インデックス」を作るための一覧。  
各カテゴリで、**何のパターンか / いつ使うか / コアのテクニック / 典型計算量 / 代表問題** を押さえておくと、初見の問題でも「これはあのパターンだ」と紐付けられるようになる。

表記: `n` = 入力サイズ / `k` = 定数や小さい値 / `V` = 頂点数 / `E` = 辺数

---

# トピック間の関係性マップ（NeetCode公式ロードマップより）

NeetCode本人が強調している重要な視点: **トピックは独立していない。多くは「より一般的なパターンの特殊化（specialization）」として連鎖している。** この関係性を知っておくと、問題が解けないときに「どの前提トピックが弱いのか」を診断できる。

## 特殊化の連鎖（"X は Y の特殊版"）

| 特殊化したもの | 元のパターン | 関係性 |
|---|---|---|
| Binary Search | Two Pointers | 探索空間を半分に絞る2ポインタ |
| Sliding Window | Two Pointers | 連続区間を維持する2ポインタ |
| Trees | Linked List | 各ノードが複数の子を持つリンクドリスト |
| Heap | Binary Tree | 半順序を保つ完全二分木 |
| Backtracking | Tree の走査 | 本質的に「でかい決定木」 |
| 1-D DP | Backtracking + メモ化 | **全探索にキャッシュを足したもの** |
| 2-D DP | 1-D DP（× グリッド） | グリッド/グラフ上の DP |
| Advanced Graphs | Graphs + Heap | Dijkstraなどヒープを使う最短路系 |

## 前提トピックの依存関係（詰まったら戻る先）

```
Arrays & Hashing           ←── すべての土台
    ↓
Two Pointers ──→ Binary Search / Sliding Window
    ↓
Linked List（2ポインタを多用）
    ↓
Trees（リンクドリストの複雑版）
    ↓
    ├── Tries
    ├── Heap（二分木の特殊版）
    └── Backtracking（決定木）
            ↓
            ├── Graphs（DFS = 再帰バックトラッキング）
            │       ↓
            │       └── Advanced Graphs（+ Heap）
            │
            └── 1-D DP（= Backtracking + メモ化）
                    ↓
                    └── 2-D DP（グリッド/2文字列）
```

**Intervals / Greedy / Bit Manipulation / Math** は比較的独立しており、上の連鎖のどこにでも挟める。重要度としては DP/Graph 系より下と Navdeep は位置付けている。

## 診断ツールとしての使い方

これが一番実用的なポイント:

- **Graph で詰まる → Backtracking（DFS/再帰）が弱い可能性**
- **DP で詰まる → そもそも Backtracking で全探索が書けていない可能性**
- **Trees で詰まる → 再帰そのもの / リンクドリストへの慣れが不足**
- **Linked List で詰まる → Two Pointers の基礎が不足**
- **Two Pointers / Sliding Window で詰まる → 配列操作・インデックス感覚が不足**

問題を解けなかったときに「難しすぎた」で終わらせず、**「どの前提スキルが穴だったか」を特定してそこに戻る**。これが最速で実力を上げるループ。

---

# 各カテゴリ詳細

## 1. Arrays & Hashing（配列とハッシュ）

**何のパターンか**: 配列を走査して、要素の存在・頻度・重複をハッシュ（`dict` / `set`）で高速に管理する最基礎。

**いつ使う**: 「〇〇が存在するか」「頻度は？」「重複は？」「対応するペアがあるか」といった問い。ほぼ全問題の土台。

**コアのテクニック**:
- ハッシュマップで O(n) のルックアップ
- 「一度走査しながら集める → もう一度走査して答える」2パス
- 文字列はソート/カウントで正規化して比較

**典型計算量**: 時間 O(n)、空間 O(n)

**代表問題**: Two Sum / Contains Duplicate / Valid Anagram / Group Anagrams / Top K Frequent / Product of Array Except Self / Longest Consecutive Sequence

---

## 2. Two Pointers（2ポインタ）

**何のパターンか**: 配列の左右（または前後）からポインタを動かして、O(n²) を O(n) に落とす。

**いつ使う**: ソート済み配列、回文判定、ペア/トリプレット探索、容量最大化など。

**コアのテクニック**:
- 左右から挟み撃ち（`l`, `r`）
- 同方向の "fast / slow" ポインタ
- まずソートしてから 2pt を使うケースが多い（3Sum 等）

**典型計算量**: 時間 O(n) or O(n log n)（ソート込み）、空間 O(1)

**代表問題**: Valid Palindrome / Two Sum II（ソート済み）/ 3Sum / Container With Most Water / Trapping Rain Water

---

## 3. Sliding Window（スライディングウィンドウ）

**前提**: Two Pointers（窓も2ポインタの変種）

**何のパターンか**: 連続する部分列（subarray/substring）を窓として動かし、条件を満たす最長/最短/個数を求める。

**いつ使う**: 「連続する〇〇」「最長/最短の部分文字列」「合計が〇〇以上/以下」系。

**コアのテクニック**:
- 固定長ウィンドウ（窓サイズが与えられる）
- 可変長ウィンドウ（条件を満たすまで `r` を伸ばし、破れたら `l` を縮める）
- 窓の中身を dict や Counter で管理

**典型計算量**: 時間 O(n)、空間 O(k)

**代表問題**: Best Time to Buy/Sell Stock / Longest Substring Without Repeating / Longest Repeating Character Replacement / Minimum Window Substring / Sliding Window Maximum

---

## 4. Stack（スタック）

**何のパターンか**: LIFO（後入れ先出し）。「直前の要素」や「対応する括弧」の処理。

**いつ使う**: 括弧のマッチング、式の評価、単調増加/減少の関係（monotonic stack）、「次に大きい要素」系。

**コアのテクニック**:
- 括弧 → `push` / `pop`
- **Monotonic Stack**: 単調な順序を保ちながら push/pop することで「次に大きい要素」を O(n) で求める
- 式のトークン処理（RPN）

**典型計算量**: 時間 O(n)、空間 O(n)

**代表問題**: Valid Parentheses / Min Stack / Evaluate RPN / Daily Temperatures / Car Fleet / Largest Rectangle in Histogram

---

## 5. Binary Search（二分探索）

**前提**: Two Pointers（Binary Search は 2pt の特殊版）

**何のパターンか**: ソート済み（または単調性のある）空間を半分ずつに絞る。

**いつ使う**: 「ソート済み配列の検索」だけでなく、「答えを二分探索する」パターンが超重要。「最小の〇〇で条件を満たす」「最大の〇〇で制約内に収まる」はほぼこれ。

**コアのテクニック**:
- 古典的な `l`, `r`, `mid` の探索
- **答えの二分探索**: 解の値域に対して「この値で条件満たせるか？」を判定関数で問う
- 回転配列の探索（ローテーション位置で場合分け）

**典型計算量**: 時間 O(log n) or O(n log n)（判定関数が O(n) のとき）

**代表問題**: Binary Search / Search in Rotated Sorted Array / Find Min in Rotated Sorted Array / Koko Eating Bananas / Time Based Key-Value Store / Median of Two Sorted Arrays

---

## 6. Linked List（連結リスト）

**前提**: Two Pointers（fast/slow を多用）

**何のパターンか**: ポインタ（`next`）の張り替えで O(1) の挿入/削除。

**いつ使う**: 順序付きのデータでランダムアクセス不要、サイクル検出、リストの反転。

**コアのテクニック**:
- **Fast & Slow ポインタ**: サイクル検出（Floyd）、中央ノード発見
- **dummy ヘッドノード**: 先頭の特殊処理を消す定番テク
- 反転（`prev`, `cur`, `next` の3変数）

**典型計算量**: 時間 O(n)、空間 O(1)

**代表問題**: Reverse Linked List / Merge Two Sorted Lists / Linked List Cycle / Reorder List / Remove Nth Node From End / LRU Cache / Merge k Sorted Lists / Reverse Nodes in k-Group

---

## 7. Trees（木）

**前提**: Linked List / 再帰の基礎（Tree は「子が複数あるリンクドリスト」）

**何のパターンか**: 再帰で分割統治。親 → 子（preorder）/ 子 → 親（postorder）の判断が肝。

**いつ使う**: 階層構造、BST、LCA、パス合計、シリアライズなど。

**コアのテクニック**:
- **DFS**: 再帰 or スタック。preorder / inorder / postorder
- **BFS**: キューでレベル順（Level Order Traversal）
- **postorder で子の情報を集約** → 深さ、直径、バランス判定
- **BST の性質**: inorder で昇順 / 左 < 根 < 右

**典型計算量**: 時間 O(n)、空間 O(h)（h = 木の高さ、平均 O(log n)・最悪 O(n)）

**代表問題**: Invert Binary Tree / Max Depth / Diameter / Balanced / Same Tree / Subtree / LCA of BST / Level Order / Right Side View / Validate BST / Kth Smallest in BST / Construct Tree from Preorder+Inorder / Max Path Sum / Serialize & Deserialize

---

## 8. Tries（トライ木）

**前提**: Trees（Trie は子が文字キーの木）

**何のパターンか**: 文字列のプレフィックスを共有する木構造。

**いつ使う**: 「プレフィックスで検索」「辞書で単語を高速検索」「ワイルドカード検索」。

**コアのテクニック**:
- 各ノードが子 dict と `isEnd` フラグを持つ
- DFS と組み合わせて複数単語を同時検索（Word Search II）

**典型計算量**: 挿入/検索: 時間 O(L)（L = 単語長）、空間 O(総文字数)

**代表問題**: Implement Trie / Design Add and Search Words / Word Search II

---

## 9. Heap / Priority Queue（ヒープ・優先度付きキュー）

**前提**: Binary Tree（Heap は完全二分木の特殊版）

**何のパターンか**: 常に最小/最大を O(log n) で取り出せる半順序木。

**いつ使う**: 「上位 k 個」「k 番目に大きい/小さい」「中央値をストリームで」「スケジューリング」。

**コアのテクニック**:
- Python は `heapq`（min-heap）。max-heap は値を負にして入れる
- **k 個の最小/最大**: サイズ k のヒープを保つ → 時間 O(n log k)
- **Two Heaps**: 中央値 = max-heap（下半分）+ min-heap（上半分）

**典型計算量**: 時間 O(n log k) or O(n log n)、空間 O(k)

**代表問題**: Kth Largest in Stream / Last Stone Weight / K Closest Points / Kth Largest in Array / Task Scheduler / Find Median from Data Stream

---

## 10. Backtracking（バックトラッキング）

**前提**: Trees / 再帰（Backtracking は本質的に「でかい決定木」の走査）

**何のパターンか**: 全探索の再帰で、ダメな分岐を早めに切り落とす（枝刈り）。

**いつ使う**: 部分集合、順列、組み合わせ、N-Queens、Sudoku、制約付きパスなど「全部列挙」系。

**コアのテクニック**:
- テンプレート: `choose → recurse → unchoose`
- 同じ枝を 2 回探索しないための `start` インデックスや訪問済みフラグ
- 重複排除はソート + `if i > start and nums[i] == nums[i-1]: continue`

**典型計算量**: 時間 O(2ⁿ) or O(n!)、空間 O(n)（再帰の深さ）

**代表問題**: Subsets / Combination Sum / Permutations / Word Search / Palindrome Partitioning / Letter Combinations / N-Queens

---

## 11. Graphs（グラフ）

**前提**: Backtracking（DFS = 再帰バックトラッキング）/ Trees

**何のパターンか**: ノードと辺で表される構造を BFS/DFS で走査。グリッド問題もグラフとして扱える。

**いつ使う**: 連結成分、最短経路（重みなし）、トポロジカル順序、閉路検出、島カウントなど。

**コアのテクニック**:
- **BFS** = 最短ステップ数（重みなしの最短経路）
- **DFS** = 連結成分、閉路検出、トポロジカルソート
- **Union-Find（DSU）** = 動的な連結判定、MST
- **多始点 BFS** = 腐ったミカン、壁とゲートなど同時拡散

**典型計算量**: 時間 O(V + E)、空間 O(V)

**代表問題**: Number of Islands / Clone Graph / Pacific Atlantic / Rotting Oranges / Course Schedule (I & II) / Redundant Connection / Graph Valid Tree / Word Ladder

---

## 12. Advanced Graphs（発展グラフ）

**前提**: Graphs + Heap（Dijkstraはヒープを使う）

**何のパターンか**: 重み付きグラフの最短路、MST、オイラー路など。

**いつ使う**: 重み付き最短経路、全頂点接続の最小コスト、到達可能性で K 経由など。

**コアのテクニック**:
- **Dijkstra**: 非負重み付きの最短路。ヒープで O((V+E) log V)
- **Bellman-Ford**: 負の辺にも対応。K ステップ制約にも使える
- **Prim / Kruskal**: MST（最小全域木）
- **トポロジカルソート**: DAG の依存解決

**典型計算量**: Dijkstra: 時間 O((V+E) log V) / MST: 時間 O(E log V)

**代表問題**: Reconstruct Itinerary / Min Cost to Connect All Points / Network Delay Time / Swim in Rising Water / Alien Dictionary / Cheapest Flights Within K Stops

---

## 13. 1-D Dynamic Programming（1次元DP）

**前提**: Backtracking（⚠️ 超重要）

**核心の言い換え**: **DP = Backtracking + メモ化**。まず素朴な再帰（= バックトラッキング）で全探索を書けるようにしてから、「同じ部分問題を何度も計算している」箇所にキャッシュ（dict や配列）を付けるだけ。これが一番挫折しない学び方。

**何のパターンか**: 部分問題の答えをメモ化・テーブル化して、重複計算を消す。

**いつ使う**: 「最小/最大/個数」で、小さい部分問題の答えから組み上げられるとき。

**コアのテクニック**:
- **遷移式（recurrence）を言語化する** のが一番大事
- トップダウン（再帰 + memo）かボトムアップ（for ループ + table）
- 1 次元なら `dp[i]` が位置 i までの答え

**典型計算量**: 時間 O(n) or O(n²)、空間 O(n) or O(1)（ローリング配列）

**代表問題**: Climbing Stairs / House Robber (I & II) / Longest Palindromic Substring / Decode Ways / Coin Change / Word Break / Longest Increasing Subsequence / Partition Equal Subset Sum

---

## 14. 2-D Dynamic Programming（2次元DP）

**前提**: 1-D DP / Graphs（グリッド問題はグラフ上のDPとも見れる）

**何のパターンか**: 2 つの変数（2 つの文字列、2 次元グリッド、残量 × 位置など）を持つ DP。

**いつ使う**: 2 つの配列の関係、グリッド経路、ナップサック、編集距離など。

**コアのテクニック**:
- `dp[i][j]` の意味を最初に明確に決める（これが 9 割）
- テーブルの初期化（最初の行・列）を丁寧に
- 空間圧縮（行 → 1 次元）できる場合が多い

**典型計算量**: 時間 O(n·m)、空間 O(n·m) or O(min(n,m))

**代表問題**: Unique Paths / Longest Common Subsequence / Coin Change II / Target Sum / Edit Distance / Distinct Subsequences / Burst Balloons / Regular Expression Matching

---

## 15. Greedy（貪欲法）

**何のパターンか**: 各ステップで「局所最適」を選ぶと「全体最適」になる問題。

**いつ使う**: ソートして順に処理するタイプ、局所的な比較で決着がつく問題。

**コアのテクニック**:
- 「なぜ貪欲で OK なのか」を言語化できるかが肝（証明感覚）
- ソート + 1 パス
- DP との違い: 貪欲は過去を見直さない、DP は見直す

**典型計算量**: 時間 O(n log n)（ソート込み）、空間 O(1)

**代表問題**: Maximum Subarray（Kadane）/ Jump Game (I & II) / Gas Station / Hand of Straights / Partition Labels / Valid Parenthesis String

---

## 16. Intervals（区間）

**何のパターンか**: 区間 `[start, end]` の重なり・統合・挿入。

**いつ使う**: カレンダー予約、会議室、連続スケジュール、区間マージ。

**コアのテクニック**:
- **まず start でソート** → 順に見て重なり判定
- 会議室問題は **heap で終了時刻を管理** するのが定番
- 「終了時刻」でソートする派生もある

**典型計算量**: 時間 O(n log n)、空間 O(n)

**代表問題**: Insert Interval / Merge Intervals / Non-overlapping Intervals / Meeting Rooms (I & II) / Min Interval to Include Each Query

---

## 17. Math & Geometry（数学・幾何）

**何のパターンか**: 算術、行列操作、幾何の基本。

**いつ使う**: 行列の回転・螺旋、剰余、累乗、文字列としての数値計算。

**コアのテクニック**:
- 行列の **in-place 操作**（余分な空間を使わない回転など）
- 累乗は二分法で O(log n)
- 繰り返し数字 → 周期発見（cycle detection）

**典型計算量**: ケース依存。行列系は O(n·m)、累乗は O(log n)

**代表問題**: Rotate Image / Spiral Matrix / Set Matrix Zeroes / Happy Number / Pow(x, n) / Multiply Strings / Detect Squares

---

## 18. Bit Manipulation（ビット演算）

**何のパターンか**: AND / OR / XOR / シフトで整数を扱う。

**いつ使う**: 「片方だけ出現する数」（XOR）、ビット数のカウント、集合の表現、整数の加算（算術演算子なし）。

**コアのテクニック**:
- `x & (x - 1)` で最下位の 1 ビットを消す
- `a ^ b` で差分検出（同じ値は 0 になる）
- マスクとシフトで「特定ビットを立てる/消す」

**典型計算量**: 時間 O(1) or O(log n)（ビット数に比例）

**代表問題**: Single Number / Number of 1 Bits / Counting Bits / Reverse Bits / Missing Number / Sum of Two Integers / Reverse Integer

---

# 学習の優先順位（推奨ルート）

まずは **Blind 75 相当の順番** で進めるのがベスト。

```
①Arrays & Hashing → ②Two Pointers → ③Sliding Window → ④Stack →
⑤Binary Search → ⑥Linked List → ⑦Trees → ⑧Tries →
⑨Heap → ⑩Backtracking → ⑪Graphs → ⑫Advanced Graphs →
⑬1-D DP → ⑭2-D DP → ⑮Greedy → ⑯Intervals →
⑰Math & Geometry → ⑱Bit Manipulation
```

- 最初の ①〜⑤ で「ハッシュ/ポインタ/窓/スタック/二分探索」のクセをつける
- ⑥〜⑨ でデータ構造への慣れ
- ⑩〜⑭ が山場（⑬⑭のDPが最難関）
- ⑮〜⑱ は比較的独立、後でも OK

---

# 各問題で最低限聞かれること

1. **この問題はどのパターンか？**（= このインデックスのどこか？）
2. **時間計算量はいくつか？**
3. **空間計算量はいくつか？**
4. **エッジケースは？**（空入力、1 要素、重複、負の数、オーバーフロー）
5. **改善の余地はあるか？**（ヒントを貰ったらどの軸で速くできるか）

この 5 つをルーチンにすると、面接の「解説フェーズ」で事故らない。

---

# 解けなかったときの自己診断（NeetCode流）

「難しすぎた」で終わらせず、毎回これを自問する:

- **どのパターンだと最初に気づけなかったか？** → インデックスの見直しが足りない
- **どの前提トピックが穴だったか？** → 上の「前提トピックの依存関係」に戻る
- **コードには書けたが計算量が悪かったか？** → データ構造の選択が悪い
- **アイデアは出たが実装が詰まったか？** → 単純に手数が足りない、同系統の問題を数こなす

トラッカーの「メモ」欄にこれを書くと、1ヶ月後に自分の弱点トレンドが見える。
