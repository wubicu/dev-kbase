`default.latex` 是使用 XeCJK 來支援中文排版的 Pandoc LaTeX [模板檔](http://johnmacfarlane.net/pandoc/README.html#templates)。

請將此模板檔放到 Pandoc 的 `data-dir` 目錄中的 `templates` 子目錄，預設的 `data-dir` 目錄為

Windows XP

    C:\Documents And Settings\USERNAME\Application Data\pandoc

Windows 7+

    C:\Users\USERNAME\AppData\Roaming\pandoc
    
Unix/OS X
    
    $HOME/.pandoc

若要指定中文字形，請使用 `cjkmainfont` 變數來指定，而不是用 `mainfont` 變數。