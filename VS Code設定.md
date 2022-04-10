# VS Code設定

- [VS Code設定](#vs-code設定)
  - [拡張機能のインストール](#拡張機能のインストール)
  - [コードスニペットの生成](#コードスニペットの生成)
  - [ツールのダウンロード](#ツールのダウンロード)
  - [settings.jsonの設定](#settingsjsonの設定)

## 拡張機能のインストール

- Vim

- Iceberg Theme
 画面テーマの設定
- VSCode File Explorer menu
\<Ctrl>+\<h>でエクスプローラへ移動
\<Ctrl>+\<l>でエクスプローラからコード編集ペインへ移動
- Material Icon Theme       アイコンの表示
- Bracket Pair Colorizer 2  括弧の対応可視化
- Rainbow CSV               CSVファイルの表示
- Activitus Bar             　アクティビティバーに表示する項目をステータスバーに表示
- code eol 改行コードの可視化
- Alignment 複数のコードを揃える =の位置とか
- comment aligner コメントの位置を揃える

## コードスニペットの生成

- [コードスニペットジェネレータ](https://snippet-generator.app/)

## ツールのダウンロード

- 全角半角切り替えツール(zenhan.exe)のダウンロード
[実行ファイル](https://github.com/iuchim/zenhan/releases/download/v0.0.1/zenhan.zip)
ダウンロードしたファイルを任意のフォルダへ格納する

## settings.jsonの設定

1. Ctr+Shift+pを入力する
2. settings.jsonと入力する
3. 以下の内容を貼り付ける
4. zenhan.exeのパスを修正する
5. ステータスバーの色変更を有効にするため、以下のファイルを編集する
    "~\.vscode\extensions\cocopon.iceberg-theme-2.0.3\themes\iceberg.color-theme.json"
    以下の3行をコメントアウト

    ```json
    //“statusBar.debuggingForeground”: “#6b7089”,
    //“statusBar.foreground”: “#6b7089”,
    //“statusBar.noFolderForeground”: “#6b7089”,
    ```

6. .vimrcの編集
    VS Codeはremapのみ対応

    ```vimrc
    " ESC2度押しでハイライトを解除する
    nnoremap <Esc><Esc> :nohlsearch<CR><Esc>
    ```

7. settings.jsonの編集

    ```json
    {
        "vim.vimrc.enable" : true,
        "vim.vimrc.path" : "~/.vimrc",
        "security.workspace.trust.untrustedFiles": "open",
        // システムのクリップボードを利用する
        "vim.useSystemClipboard": true,
        // ハイライトサーチ
        "vim.hlsearch": true,
        // easymotionを有効にする
        "vim.easymotion": true,
        "workbench.colorTheme": "Iceberg",
        // ビジュアルモードで*や#で検索
        "vim.visualstar": true,
        // 検索時に大文字小文字を無視する
        "vim.ignorecase": true,
        // インクリメンタルサーチ
        "vim.incsearch": true,
        // CamelCaseMotionを有効にする
        "vim.camelCaseMotion.enable": true,
        // leaderキーの設定
        "vim.leader": "<space>",
        // yankした箇所をハイライト表示する
        "vim.highlightedyank.enable": true,
        // yankした時の色
        "vim.highlightedyank.color": "rgba(250, 240, 170, 0.5)",
        // yankした時の色の表示時間(msec)
        "vim.highlightedyank.duration": 500,
        // nnorremap
        "vim.normalModeKeyBindingsNonRecursive": [
            {"before": ["J"],"after"               : ["1","0","j"]},        //移動を早める
            {"before": ["K"],"after"               : ["1","0","k"]},        //移動を早める
            {"before": ["H"],"after"               : ["0"]},                //端に移動
            {"before": ["L"],"after"               : ["$"]},                //端に移動
            {"before": ["<Leader>", "h"],"after"   : ["<C-w>","h"]},        //window移動
            {"before": ["<Leader>", "j"],"after"   : ["<C-w>","j"]},        //window移動
            {"before": ["<Leader>", "k"],"after"   : ["<C-w>","k"]},        //window移動
            {"before": ["<Leader>", "l"],"after"   : ["<C-w>","l"]},        //window移動
            {"before": ["<Leader>", "s"],"after"   : ["<C-w>","s"]},        //WIndow水平分割
            {"before": ["<Leader>", "v"],"after"   : ["<C-w>","v"]},        //WIndow垂直分割
            {"before": ["v", "y"],"after"          : ["v","a","w","y"]},    //カーソル位置の単語をyankする
            // VSCodeのRedo/Undoに設定する(外部で変更されると戻せないため)
            {
                "before": ["u"],
                "after": [],
                "commands": [
                    {
                        "command": "undo"
                    }
                ]
            },
            {
                "before": ["<C-r>"],
                "after": [],
                "commands": [
                    {
                        "command": "redo"
                    }
                ]
            },
            // xやsでコピー(ヤンク)しないようにする
            {
                "before": ["x"],
                "after": ["\"", "_", "x"]
            },
            {
                "before": ["s"],
                "after": ["\"", "_", "s"]
            },
            //{"before": ["<Leader>","s","\""], "after": ["c","i","w","\"","\"","<Esc>","P"]},
            // Leader+cでその行をコメントアウトする
            {"before": ["<Leader>", "c"], "commands": [{"command": "editor.action.commentLine"}]},
            // Leader+mで対応する括弧へ移動するデフォルトのキーが押しにくい
            {
                "before": ["<Leader>", "m"],
                "after": ["%"]
            },
            // Leader+s+"でクリップボードにコピーされた文字列を""でくくられた文字列に置換する
            {
                "before": ["<Leader>", "s", "\""],
                "after": ["\"", "_", "c", "i", "\"", "<C-r>", "*", "<Esc>"]
            },
        ],
        "vim.insertModeKeyBindings": [
            // 入力モードからjj入力でESCと同じ動作をさせる
            {
                "before": ["j", "j"],
                "after": ["<Esc>"]
            },
            // ;+;で補完ビューを表示する設定
            {
                "before": [";", ";"],
                "commands": ["editor.action.triggerSuggest"]
            },
            {"before": ["<C-l>"],"after"   : ["<Right>"]},        //右へ移動
        ],
        // Visual Mode
        "vim.visualModeKeyBindingsNonRecursive": [
            //vを押した直後はvのコマンドが残っているので注意
            //visualmode後にすぐ実行したいものは、二重で定義する。
            //外部プラグイン呼び出し
            {"before": ["<Leader>", "b"],"commands": [{"command":"alignment.align"}]},             //揃える
            {"before": ["<Leader>", "v"],"commands": [{"command":"extension.commentaligner"}]},    //コメントを揃える
            {"before": ["J"],"after"               : ["1","0","j"]},        //移動を早める
            {"before": ["K"],"after"               : ["1","0","k"]},        //移動を早める
            {"before": ["H"],"after"               : ["0"]},                //端に移動
            {"before": ["L"],"after"               : ["$"]},                //端に移動
        ],
        // 入力モードからノーマルモードへ戻った時に日本語入力をOFFにする
        // zenhan.exe(全角半角変更ツール)を指定フォルダへ格納しておくこと
        "vim.autoSwitchInputMethod.enable": true,
        "vim.autoSwitchInputMethod.defaultIM": "0",
        "vim.autoSwitchInputMethod.obtainIMCmd": "D:\\Tool\\zenhan\\bin64\\zenhan.exe 0",
        "vim.autoSwitchInputMethod.switchIMCmd": "D:\\Tool\\zenhan\\bin64\\zenhan.exe {im}",
        // カーソルの点滅を滑らかにする
        "editor.cursorBlinking": "smooth",
        // 制御文字の表示
        "editor.renderControlCharacters": true,
        "editor.renderWhitespace": "all",
        //markdown setting
        "markdown.preview.breaks": true,
        "editor.insertSpaces": false,
        //========================================================================
        //言語毎の設定
        //========================================================================
        //C関係の設定は社内の設定に合わせる
        "[c]": {
            "editor.tabSize": 4,
            "editor.insertSpaces": false,
        },
        "[cpp]": {
            "editor.tabSize": 4,
            "editor.insertSpaces": false,
        },
        "[markdown]": {
            "editor.tabSize": 2,                //タブサイズの設定
            "editor.quickSuggestions":true,     //サジェスチョンをすべて有効にする
            "editor.quickSuggestionsDelay": 0,  //サジェスチョンのディレイ
        },
        "workbench.iconTheme": "material-icon-theme",
        "bracket-pair-colorizer-2.depreciation-notice": false,
        "editor.fontLigatures": false,
        // フォント設定を'MyricaM'に設定
        //"editor.fontFamily": "Consolas, 'Courier New', monospace"
        "editor.fontFamily": "Consolas, 'MyricaM M', monospace",
        // 入力補完でカーソル付近の単語を優先する
        "editor.suggest.localityBonus": true,
        // keywordを表示しない
        "editor.suggest.showKeywords": false,
        // 最近の候補を選択する
        "editor.suggestSelection": "recentlyUsed",
        // Enterキーで候補を受け入れない
        "editor.acceptSuggestionOnEnter": "off",
        // スニペットを先頭に表示する
        "editor.snippetSuggestions": "top",
        // アクティビティバーを非表示にする
        "workbench.activityBar.visible": false,
        "editor.unicodeHighlight.includeComments": true,
        // 全角空白のみハイライト表示する
        "editor.unicodeHighlight.ambiguousCharacters": false,
        // ステータスバーの色の設定を有効化する
        // 動作がもっさりするのでfalseにする
        "vim.statusBarColorControl": false,
        "vim.statusBarColors.normal": [
            "#161821",
            "#818596"
        ],
        "vim.statusBarColors.insert": [
            "#84A0C6",
            "#161821"
        ],
        "vim.statusBarColors.visual": [
            "#B4BE82",
            "#161821"
        ],
        "vim.statusBarColors.visualline": [
            "#B4BE82",
            "#161821"
        ],
        "vim.statusBarColors.visualblock": [
            "#B4BE82",
            "#161821"
        ],
        "vim.statusBarColors.replace": [
            "#E2A478",
            "#161821"
        ],
        "vim.statusBarColors.commandlineinprogress": [
            "#818596",
            "#161821"
        ],
        "vim.statusBarColors.searchinprogressmode": [
            "#818596",
            "#161821"
        ],
        "vim.statusBarColors.easymotionmode": [
            "#818596",
            "#161821"
        ],
        "vim.statusBarColors.easymotioninputmode": [
            "#818596",
            "#161821"
        ],
        "vim.statusBarColors.surroundinputmode": [
            "#818596",
            "#161821"
        ],
        "workbench.colorCustomizations": {
            "statusBar.background": "#161821",
            "statusBar.noFolderBackground": "#161821",
            "statusBar.debuggingBackground": "#161821",
            "statusBar.foreground": "#818596",
            "statusBar.debuggingForeground": "#818596"
        },
        "git.autofetch": true,
    }
    ```
