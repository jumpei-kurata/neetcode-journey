# NeetCode 150 Journey

[NeetCode 150](https://neetcode.io/roadmap) に挑戦する個人リポジトリ。パターン別に解いて、関係性を理解しながら実力をつけるのが目標。

## 進捗

- 解いた問題数: **0 / 150** (0%)
- 開始日: 2026-04-23
- 最終更新: 2026-04-23

### カテゴリ別

| カテゴリ | 進捗 |
|---|---|
| Arrays & Hashing | 0 / 9 |
| Two Pointers | 0 / 5 |
| Sliding Window | 0 / 6 |
| Stack | 0 / 7 |
| Binary Search | 0 / 7 |
| Linked List | 0 / 11 |
| Trees | 0 / 15 |
| Tries | 0 / 3 |
| Heap / Priority Queue | 0 / 7 |
| Backtracking | 0 / 9 |
| Graphs | 0 / 13 |
| Advanced Graphs | 0 / 6 |
| 1-D DP | 0 / 12 |
| 2-D DP | 0 / 11 |
| Greedy | 0 / 8 |
| Intervals | 0 / 6 |
| Math & Geometry | 0 / 8 |
| Bit Manipulation | 0 / 7 |

## リポジトリ構成

```
.
├── README.md          # このファイル。進捗サマリー
├── INDEX.md           # カテゴリ別インデックス（パターン・前提・計算量・代表問題）
├── TRACKER.md         # 全150問のチェックリスト
├── solutions/         # 各問題の解答コード（カテゴリ別に分類）
│   ├── arrays_hashing/
│   ├── two_pointers/
│   ├── sliding_window/
│   └── ...
└── notes/             # 学習メモ、詰まったときの振り返り
    └── mistakes.md
```

## ワークフロー

1. [TRACKER.md](./TRACKER.md) を開いて、次に解く問題を決める
2. LeetCodeのリンクから問題ページへ飛ぶ
3. **15〜30分は自力で考える**。詰まったら [INDEX.md](./INDEX.md) の該当カテゴリを見て、パターンの引き出しを探る
4. それでも無理なら NeetCode の解説動画（各問題ページ）を見る
5. 解説を見た場合は、**動画を閉じて何も見ずに再実装**する
6. Submit 通ったらコードを Cursor にコピーして `solutions/<category>/<problem>.py` として保存
7. TRACKER.md にチェックを入れて `git commit -m "solved: <Problem Name>"`
8. 解いた3日後、1週間後、1ヶ月後に復習

## 命名規則（解答ファイル）

```
solutions/<category>/<problem_name_snake_case>.py
```

例:
```
solutions/arrays_hashing/two_sum.py
solutions/sliding_window/longest_substring_without_repeating_characters.py
```

各ファイルの先頭に以下をコメントで書く:

```python
"""
Two Sum (LeetCode #1, Easy)
https://leetcode.com/problems/two-sum/

Pattern: Arrays & Hashing
Time: O(n)
Space: O(n)

Key idea: 一度のループで (target - num) を dict で探しながら埋めていく
"""
```

## 参考リンク

- [NeetCode.io](https://neetcode.io/)
- [NeetCode 150 Roadmap](https://neetcode.io/roadmap)
- [LeetCode](https://leetcode.com/)
- [NeetCode YouTube](https://www.youtube.com/@NeetCode)
