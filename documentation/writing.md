# 文件的撰寫

文件的撰寫對管理和溝通會有相當程度的影響。好的文件除了內容以外，它也必須讓人容易閱讀和查尋。不容易被閱讀和查尋的文件，就算有好的內容也不容易被他人取得，也等於是無用的資料。

這篇文章主要是說明如何為公司組織撰寫好的文件，讓其內容能真正被他人取用。

## 需求

為了達成前述的目的，撰寫文件的方法和工具應該要滿足下列需求：

- 文件必須可以純文字形式編輯和讀取，以確保未來不用任何工具即可讀取和編輯。
- 文件必須可以轉換成多種格式，且至少必須支援 HTML 和 PDF。
- 撰寫文件所需要的工具必須支援多國語言。
- 必須容易學習和使用。

## 解決方案

我們所提出來滿足上列需求的解決方案為採用 [Markdown][markdown] + [Pandoc][pandoc]。

標準的 Markdown 語言是為了方便撰寫 HTML 所發明的語言，其所支援的語法並不多樣，所以就有一些 Markdown 的實作提供了延伸的語法和功能，最有名的就是 [MultiMarkdown][multimarkdown]，它所提供的功能足夠撰寫出一份使用 TeX 排版系統所撰寫的文件，它也支援較多的輸出格式。

Pandoc 則是另一套類似 MultiMarkdown 的格式轉換工具，但它的輸入格式不只支援 Markdown。還可以使用 HTML、[Textile][textile]、[reStructureText][rst]、[LaTeX][latex]、[DocBook][docbook] 和 [MediaWiki][mediawiki] 等。輸出的格式也比 MultiMarkdown 要多出很多。而且也提供了差不多的 Markdown 的[延伸語法][pandoc_markdown] ([中文版][pandoc_markdown_zh])。它們之間的比較可以參閱 [jgm 寫的 Pandoc-vs-Multimarkdown](https://github.com/jgm/pandoc/wiki/Pandoc-vs-Multimarkdown)。

**!!!注意!!!** 除了 MultiMarkdown 和 Pandoc's Markdown 外，還有許多 Markdown 的實作有延伸語法，像是 [GitHub Flavored Markdown][gfm]、[Markdown Extra][markdown_extra] 和 [Python Markdown][python_markdown] 等。這些語法雖然是大同小異，但彼此間不一定相容，所以撰寫和轉換時必須注意所使用的語法和轉換工具是否一致。

我們的解決方案是採用 Pandoc 文件轉換工具，所以撰寫文件時所採用的 Markdown 延伸語法自然就是使用 [Pandoc's Markdown][pandoc_markdown]。另外由於 Pandoc 也支援 reStructureText，因此如果有 Python 程式的技術文件的話，也可以用 Pandoc 來轉換成 HTML 以外的格式。

[markdown]: http://en.wikipedia.org/wiki/Markdown "Markdown 輕量化標記語言"
[pandoc]: http://johnmacfarlane.net/pandoc/ "通用文件轉換器"
[multimarkdown]: http://en.wikipedia.org/wiki/MultiMarkdown "MultiMarkdown"
[pandoc_markdown]: http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown "Pandoc's Markdown"
[pandoc_markdown_zh]: http://pages.tzengyuxio.me/pandoc "Pandoc's Markdown 語法中文翻譯"
[textile]: http://redcloth.org/textile "Textile"
[rst]: http://docutils.sourceforge.net/docs/ref/rst/introduction.html "reStructureText"
[latex]: http://www.latex-project.org "LaTeX"
[docbook]: http://www.docbook.org "DocBook"
[mediawiki]: http://www.mediawiki.org/wiki/Help:Formatting "MediaWiki"
[gfm]: https://help.github.com/articles/github-flavored-markdown "GitHub Flavored Markdown"
[markdown_extra]: http://michelf.ca/projects/php-markdown/extra/ "PHP Markdown Extra"
[python_markdown]: http://pythonhosted.org/Markdown/ "Python Markdown"

### 安裝 Pandoc

[Pandoc 網站](http://johnmacfarlane.net/pandoc/installing.html)有提供 Windows 和 Mac OS X 的安裝程式，可以直接下載來安裝，Mac OS X 下還可以透過 MacPorts 等套件管理系統來安裝。

Linux 平台下可先嘗試在各自的套件管理系統內找看看有沒有 Pandoc (Ubuntu 和 Fedora 的套件庫裡已經包含了 Pandoc)。如果沒有的話可以依照 Pandoc 網站上的說明安裝。

#### PDF 輸出支援

Pandoc 的 PDF 輸出必須要透過 LaTeX 來產生，所以系統上必須有安裝 LaTeX 系統。

- Windows 系統上建議安裝 [MiKTeX](http://miktex.org)。
- Mac OS X 系統上建議安裝 [MacTeX](http://www.tug.org/mactex/index.html) 的 [BasicTeX](http://www.tug.org/mactex/morepackages.html) 套件。
- Linux 系統上通常都有 [TeX Live](http://www.tug.org/texlive/) 套件，可以直接透過套件管理系統安裝。(Ubuntu 上，執行 `apt-get install texlive`)。

##### PDF 輸出的中文問題

Pandoc 的 PDF 輸出預設是使用 pdfLaTeX TeX 排版引擎來產生 PDF 檔，但由於 pdfLaTeX 不支援 Unicode，所以它還必須使用 CJK LaTeX 套件才能產生中文 PDF。
比較好的[做法](http://johnmacfarlane.net/pandoc/faqs.html)是改用 [XeLaTeX](http://tug.org/xetex/) 作為排版引擎。

MiKTeX 和 MacTeX 的安裝都有包含 XeLaTeX，而一般的 TeX Live 的安裝並不包含，所以必須額外安裝。(Ubuntu 上，執行 `apt-get install texlive-xetex`)。

### 使用 Pandoc

Pandoc 是命令列工具，所以必須到各平台的命令列模式下使用，步驟可以參考 Pandoc 網站的 [Getting Started 說明](http://johnmacfarlane.net/pandoc/getting-started.html)。

詳細的命令說明請參考 [Pandoc 使用者手冊](http://johnmacfarlane.net/pandoc/README.html)。

要輸出中文 PDF，可以簡單地使用下列命令︰

    pandoc -o c.pdf --latex-engine=xelatex -V mainfont='AR PL UMing CN'

`mainfont` 指定使用的字型，`AR PL UMing CN` 是 Ubuntu 系統上的文鼎 TTF 字型名稱。  
**!!!注意!!! ***不同的系統不一定有相同的字型。*

### 推薦的編輯器

支援 Markdown 和 MultiMarkdown 語法的編輯器有很多，但不一定有支援 Pandoc's Markdown 語法或整合 Pandoc。
目前我們推薦使用 [Sublime Text](http://www.sublimetext.com) 加上 [Pandown 套件](https://sublime.wbond.net/packages/Pandown)。

Sublime Text 雖然不是免費或開源軟體，但它可以無限期試用，再加上它也是非常好的編程用編輯器，而且速度快，又容易客製，所以非常推薦使用。試用久了說不定都非常願意花錢購買。

Pandown 套件把 Pandoc 整合進 Sublime Text 的建置系統裡，利用它可以很方便的將 Markdown 文件轉成各種格式。透過 Pandown 的設定，也可以輕易的設定要傳遞給 Pandoc 的參數。

Sublime Text 還有很多支援 Markdown 的套件，各位可以到 [Sublime Text 的 Package Control 套件庫](https://sublime.wbond.net) 去找來用，但要注意各套件所支援的 Markdown 延伸語法，它們不一定會相容於 Pandoc's Markdown 語法。

## 撰寫時的注意事項

#### 使用空格字元取代 tab 字元

因為 tab 字元在不同的編輯器或檢視器下，會依不同的設定而被顯示成不同數量的空格字元。這種不確定性容易造成共同編輯者和檢視者的困擾。因此應該將編輯器設定為自動將 tab 字元轉成空格字元。至於要轉成幾個空格字元則依所編輯的檔案而定。一般文件應設為四個空格字元，原始碼則依語言決定。Python、C/C++ 和 Java 原始檔為四個，HTML、CSS/SASS、JavaScript/CoffeeScript 則為二個。

#### 中文文件應使用中文的全形標點符號

在中文文句中，除了小括號外〝(〞和〝)〞，應該使用全形的標點符號。因為字間的距離看起來會比較美觀和舒服，尤其是逗號、頓號、問號和句號。

#### 段落內不要在中文文字間斷行

Markdown 會把換行字元 (new line) 轉成空格字元，二個換行字元才會產生新段落。這在英文文句裡沒有問題，但是在中文文句中，由於中文字間並不會有空格字元，所以如果輸出的格式沒有在該處換行的話，該處會有明顯的空白。

為了解決此問題，最好是不要在段落內斷行 (hard wrap)，並且應該使用編輯器的自動斷行 (soft wrap) 和顯示行數功能以方便編輯。

另外也可以只在中文標點符號或英文字 (包含英數符號) 後斷行，因為空格字元在標點符號後會較不容易被查覺而英文字間原本就需要空格分隔。這樣的做法不會讓一整個段落都集中在同一列中，而產生太長的列。長列在使用編輯器的自動換行功能時，不會對編輯造成困擾，但如果沒有編輯器幫忙時，偶爾還是會而造成一些困擾 (像是一些依賴行數的操作，例如 diff 和版本控制等)。但要決定在哪裡斷行也是件麻煩事，斷的不好有時也容易造成純文字的原始檔不美觀且不易閱讀。因此要不要採用自行斷行的做法就留待個人選擇。

#### 中文和英文間用空格字元隔開

中文字和英文字間用空格隔開看起來也會比較舒服，比較不會有擠在一起的感覺。

