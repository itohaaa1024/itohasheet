# itohasheet

高校の演習プリントのような、2〜4段の多段組みのワークシートを作るための LaTeX パッケージです。
ヘッダーのタイトル、組・番・名前の記入欄、段の区切り線、ページ番号を TikZ で描画するので、問題と解答を書くだけで数学や理科の演習プリントが作れます。

- 現在のバージョン：ver.1.1（2026/11/23）
- 詳しい使い方は [itohasheet-manual.pdf](itohasheet-manual.pdf) を、実際の出力例は [sample.pdf](sample.pdf) を見てください。

## 特徴

- 簡単に 2〜4 段の多段組みの文書が作れる
- 分割数に合わせて枠（段の区切り線）のデザインも自動で変わる
- 問題番号つきの問題文、マゼンタの解答文、穴埋め枠などの理数系プリント向けの部品がそろっている
- `answer` オプションで、氏名欄に「解答例」などの文字を表示するかどうかを切り替えられる

横書きのみに対応しています。縦書き、1段や5段以上の段組みには対応していません。

## 動作環境

- エンジン：LuaLaTeX、または (u)pLaTeX + dvipdfmx
- 文書クラス：`jlreq`
- 動作確認：TeX Live 2026

### 依存パッケージ

| 対象 | パッケージ |
| --- | --- |
| 共通 | `iftex`, `geometry`, `multicol`, `tikz`（`calc` ライブラリ）, `tcolorbox`（`most` ライブラリ）, `enumitem`, `fontenc`, `ceo` |
| LuaLaTeX | `luatexja`, `luatexja-fontspec`, `luatexja-otf` |
| (u)pLaTeX | `jlreq-deluxe`, `pxchfon` |

### フォント（LuaLaTeX のとき）

- 本文：UDデジタル教科書体（`UDDigiKyokashoProN`）
- タイトル・氏名欄：UD黎ミン（`PUDReiminPr6N`）

インストールされていない場合は標準のフォントのまま使えます。(u)pLaTeX では `pxchfon` の設定に従います。

## インストール

`itohasheet.sty` を、使いたい `.tex` ファイルと同じフォルダに置いてください。
どの文書からも使いたい場合は、`TEXMFHOME` 以下（例：`~/texmf/tex/latex/itohasheet/`）に置きます。

## 使い方

### 最小の例

```latex
\documentclass[paper=b4j,landscape,fontsize=9pt]{jlreq}
\usepackage{itohasheet}
\タイトル{期末考査　対策プリント}
\begin{document}
\begin{分割}
\begin{問題挿入}{1}
  次の関数の最大値を求めよ。
  \begin{enumerate}
    \item $y=-x^2+4x$
    \item $y=x^2-2x+3$ ($0\leq x \leq 3$)
  \end{enumerate}
\end{問題挿入}
\end{分割}
\end{document}
```

### コンパイル

```sh
# LuaLaTeX
lualatex sample.tex

# upLaTeX
uplatex sample.tex
dvipdfmx sample.dvi
```

### パッケージオプション

| オプション | 値 | 既定値 | 説明 |
| --- | --- | --- | --- |
| `divide` | 2, 3, 4 | 3 | 段数。これ以外の値はエラーになります |
| `answer` | true, false | false | 氏名欄に `\タイトル` の第2引数（既定は「解答例」）をマゼンタで表示します |

```latex
\usepackage[divide=3,answer]{itohasheet}
```

### 環境

| 環境 | 書き方 | はたらき |
| --- | --- | --- |
| 分割 | `\begin{分割}[段数] … \end{分割}` | 本文を段組みにします（中身は `multicols*`） |
| 問題挿入 | `\begin{問題挿入}[空き]{番号}<補足> … \end{問題挿入}` | 四角囲みの番号つきで問題文を入れます。補足は「各5点」など。空きは環境の後ろに入れる空白（`\zw` 単位、既定 1） |
| 解答挿入 | `\begin{解答挿入} … \end{解答挿入}` | 「解答」ラベルつきで解答をマゼンタで表示します |

### コマンド

| コマンド | はたらき |
| --- | --- |
| `\タイトル{メインタイトル}[解答表示]` | ヘッダーのタイトルと、`answer` 時に氏名欄に出す文字を設定します |
| `\穴埋め枠[幅]{中身}` | 穴埋め用の枠（幅は `\zw` 単位、既定 3.25） |
| `\解答欄[色]` / `\解答[色]` | 角の丸いラベル（既定は黒 / マゼンタ） |
| `\mondaibangou{番号}` | 四角で囲んだ問題番号だけを表示します |
| `\ShowWorksheetFrame` / `\HideWorksheetFrame` | 以降のページの枠を表示 / 非表示にします |
| `\NoWorksheetFrameThisPage` | 次に出力されるページだけ枠を消します |
| `\zenkakunum{n}` / `\toanum{n}` | `enumerate` の1段目・2段目で使われる番号（1〜20） |

## 注意点

- 枠の段数は `divide` オプションで決まります。`\begin{分割}[2]` のように環境側だけで段数を変えると、罫線と本文がずれます。
- 余白は `geometry` で固定されています。`\geometry` で余白を変えると枠と本文がずれます（用紙サイズの変更は問題ありません）。
- `enumerate` の 21 番目以降の番号は表示されません。

そのほかの注意点は [マニュアル](itohasheet-manual.pdf) を見てください。

## ファイル構成

| ファイル | 内容 |
| --- | --- |
| `itohasheet.sty` | パッケージ本体 |
| `itohasheet-manual.tex` / `.pdf` | マニュアル（このパッケージ自身で組版しています） |
| `sample.tex` / `.pdf` | 三角比の演習プリントのサンプル |

## ライセンス

[LaTeX Project Public License (LPPL)](https://www.latex-project.org/lppl.txt) バージョン 1.3c 以降で公開しています。
改変したものを配布する場合は、ファイル名を変えるなどして元のパッケージと区別できるようにしてください。

## 連絡先

不具合の報告や要望は [X (@itohaaa_1024)](https://x.com/itohaaa_1024) までお願いします。
