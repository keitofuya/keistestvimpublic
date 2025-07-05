# テスト

## 目次

- [テスト](#テスト)
  - [目次](#目次)
  - [デモ1](#デモ1)
    - [デモデモ1](#デモデモ1)
    - [デモデモ2](#デモデモ2)
  - [デモ2](#デモ2)
    - [デモデモ3](#デモデモ3)
  - [テスト3](#テスト3)
    - [テスト3-1](#テスト3-1)
- [拡張機能](#拡張機能)

## デモ1

### デモデモ1

- *abcde*
- **bef**
- **abdc**
- dbc

___

### デモデモ2

- e
- f
- g
- h

___

## デモ2

### デモデモ3

## テスト3

### テスト3-1

1. リスト
2. リスト2
3. リスト3
4. リスト4

- [x] test
- [x] バージョン設定

|左寄せ|中央ぞろえ|右寄せ|
| --------------- | :-------------------: | ----------------: |
| その１ | コロン中央 | 右寄せ2 |
| その2 | テスト | BBBBB |
| その3 | テストテストABCD | CCCCC |

```c
int listCount;
for( int i = 0; i < 10; i++ )
{
  listCount++;
}
```

# 拡張機能
1. Markdown All in One
リアルタイムプレビュー、自動目次生成やキーボードショートカット、自動補完が可能
必須級
2. Markdown Preview Enhanced
これも必須級
Markdownのプレビュー表示を大幅に強化する
3. Auto-Open Markdown Preview
Markdownファイルを開いたときに自動的にプレビューを表示する
Markdown Preview Enhancedの表示を自動プレビューしたい場合はsetting.jsonに以下を追加する
```json
    // 標準Markdownプレビューを無効化
    "markdown.preview.autoShowPreviewToSide": false,
    // Markdown Preview Enhanced の自動プレビューを有効化
    "markdown-preview-enhanced.automaticallyShowPreviewOfMarkdownBeingEdited": true,
```
