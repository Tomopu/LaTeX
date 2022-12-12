# LaTeX講習会
更新日: 2022年12月13日(火)  
追記: 自動コンパイル時のエラーについて (2022/12/13)

## 1. LaTeXの環境構築
まずは, LaTeXが動作する環境を構築しましょう.
OSによってダウンロード方法が異なるので, インターネットを活用して自分の環境に合った構築をしてください.
(e.g. "LaTeX Mac インストール" で検索する.)

以下に, LaTeXの環境構築の参考になるであろうWebサイトを載せておきます. (2022年12月時点)

### **macOS**
[1] MacTeX のダウンロードとインストール - LaTeX入門, [https://medemanabu.net/latex/mactex-download-install/](https://medemanabu.net/latex/mactex-download-install/).  
[2] Mac - TeX Wiki, [https://texwiki.texjp.org/?Mac](https://texwiki.texjp.org/?Mac).

### **Windows**
[1] 【大学生向け】LaTeX完全導入ガイド Windows編 (2022年), [https://qiita.com/passive-radio/items/623c9a35e86b6666b89e](https://qiita.com/passive-radio/items/623c9a35e86b6666b89e).  
[2] [VSCode対応]TeX Liveを使ってWindowsでLaTeX使おうぜ！, [https://kurosute.com/tex-windows/](https://kurosute.com/tex-windows/).  
[3] Microsoft Windows - TeX Wiki, [https://kurosute.com/tex-windows/](https://kurosute.com/tex-windows/).



## 2. VSCode に LaTeX の拡張機能を導入する.
本講習会では, TeX ファイルを VSCode (Visual Stadio Code) で編集します.
VSCode をダウンロードしていない方は, [https://code.visualstudio.com/download](https://code.visualstudio.com/download) からダウンロードしてください.

### **2-1. LaTeX Workshop をインストール**
**LaTeX Workshop** とは, LaTeX のコマンド補完をはじめ, シンタックスハイライトやファイル保存時に自動コンパイルをしてくれる便利な拡張機能です.  
VSCode 内の Marketplace で "LaTeX Workshop" と検索をしてインストールしてください.  
![LaTeX Workshop](image/latex_workshop.png)

### **2-2. LaTeX Workshop の設定**
VSCode の設定から "latex tools" と検索し, 「setting.jsonで編集」を選択してください.
settings.jsonファイルが開いたら, 下記の内容を追加します.
```
{
    // LaTeXの設定
    "[tex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[latex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[bibtex]": {
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    // --- LaTeX Workshop ---
    // 使用パッケージのコマンドや環境の補完を有効にする
    "latex-workshop.intellisense.package.enabled": true,

    // 生成ファイルを削除するときに対象とするファイル
    // デフォルト値に "*.synctex.gz" を追加
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk",
        "*.snm",
        "*.nav",
        "*.dvi",
        "*.synctex.gz"
    ],

    // 生成ファイルを "out" ディレクトリに吐き出す
    "latex-workshop.latex.outDir": "",

    "latex-workshop.latex.recipes": [
        {
            "name": "platex + dvipdfmx",
            "tools": [
                "platex",
                "dvipdfmx"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "platex",
            "command": "platex",
            "args": [
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "dvipdfmx",
            "command": "dvipdfmx",
            "args": [
                "-V 4",
                "%DOC%"
            ],
            "env": {}
        }
    ]
}
```
settings.jsonの編集が終わったら変更内容を保存してVSCodeを再起動してください.

以上でLaTeXの環境構築は終了です.  
それでは, よきLaTeXライフを~👋

## **追記(2022/12/13): 自動コンパイル時のエラーについて**
VSCode の LaTeX Workshop を用いて自動コンパイルをする際に, 上記の "setting.json" では **Recipe terminated with fatal error: spawn platex ENOENT.** というエラーが発生する可能性があります.

これは platex がどこにあるか分からないというエラーですので, この場合,
```
$ which platex
```
などで platex が保存されている場所のパスを検索し, 例えば ```/Library/TeX/texbin/platex``` に platex があれば, setting.json 内の "env" の内容を,
```
"latex-workshop.latex.tools": [
    {
        "name": "platex",
        (中略)
        "env": {
            "PATH: "/Library/TeX/texbin"
        }
    },
    {
        "name": "dvipdfmx",
        (中略)
        "env": {
            "PATH: "/Library/TeX/texbin"
        }
    }
]
```
のように変更してください. (dvipdfmxについても同様.)

### **参考文献**
[1] VSCode で "Recipe terminated with fatal error: spawn ptex2pdf ENOENT." が出て，TeX のコンパイルができない, [https://blog.n-hassy.info/2021/05/vscode-latex-enoent/](https://blog.n-hassy.info/2021/05/vscode-latex-enoent/).