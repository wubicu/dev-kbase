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

Pandoc 則是另一套類似 MultiMarkdown 的格式轉換工具，但它的輸入格式不只支援 Markdown。還可以使用 HTML、Textile、reStructureText、LaTeX、DocBook 和 MediaWiki。輸出的格式也比 MultiMarkdown 要多出很多。而且也提供了差不多的 Markdown 的[延伸語法][pandoc_markdown] ([中文版][pandoc_markdown_zh])。它們之間的比較可以參閱 [jgm 寫的 Pandoc-vs-Multimarkdown](https://github.com/jgm/pandoc/wiki/Pandoc-vs-Multimarkdown)。

**!!!注意!!!** 除了 MultiMarkdown 和 Pandoc's Markdown 外，還有許多 Markdown 的實作有延伸語法，像是 GitHub Flavored Markdown、Markdown Extra 和 Python Markdown 等。這些語法雖然是大同小異，但彼此間不一定相容，所以撰寫和轉換時必須注意所使用的語法和轉換工具是否一致。

我們的解決方案是採用 Pandoc 文件轉換工具，所以撰寫文件時所採用的 Markdown 延伸語法自然就是使用 [Pandoc's Markdown][pandoc_markdown]。另外由於 Pandoc 也支援 reStructureText，因此如果有 Python 程式的技術文件的話，也可以用 Pandoc 來轉換成 HTML 以外的格式。

[markdown]: http://en.wikipedia.org/wiki/Markdown "Markdown 輕量化標記語言"
[pandoc]: http://johnmacfarlane.net/pandoc/ "通用文件轉換器"
[multimarkdown]: http://en.wikipedia.org/wiki/MultiMarkdown "MultiMarkdown"
[pandoc_markdown]: http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown "Pandoc's Markdown"
[pandoc_markdown_zh]: http://pages.tzengyuxio.me/pandoc "Pandoc's Markdown 語法中文翻譯"

### 安裝 Pandoc

#### PDF 輸出支援

##### PDF 輸出的中文問題

### 推薦的編輯器

#### Windows


#### Linux


#### Mac OSX


## 撰寫時的注意事項

#### 使用空格字元取代 tab 字元


#### 中文文件應使用中文的全形標點符號


#### 段落內不要在中文文字間斷行

Markdown 會把換行字元 (new line) 轉成空格字元，二個換行字元才會產生新段落。這在英文文句裡沒有問題，但是在中文文句中，由於中文字間並不會有空格字元，所以如果輸出的格式沒有在該處換行的話，該處會有明顯的空白。

為了解決此問題，最好是不要在段落內斷行 (hard wrap)，並且應該使用編輯器的自動斷行 (soft wrap) 和顯示行數功能以方便編輯。

另外也可以只在中文標點符號或英文字 (包含英數符號) 後斷行，因為空格字元在標點符號後會較不容易被查覺而英文字間原本就需要空格分隔。這樣的做法不會讓一整個段落都集中在同一列中，而產生太長的列。長列在使用編輯器的自動換行功能時，不會對編輯造成困擾，但如果沒有編輯器幫忙時，偶爾還是會而造成一些困擾。但要決定在哪裡斷行也是件麻煩事，斷的不好有時也容易造成純文字的原始檔不美觀且不易閱讀。因此要不要採用自行斷行的做法就留待個人選擇。

#### 中文和英文間用空格字元隔開

