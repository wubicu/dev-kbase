# 開發環境平台

為了方便使用開源軟體來開發非 Windows 平台的應用程式，我們強列建議使用 Ubuntu 或 OS X 作為開發環境平台。

由於市面上少有安裝 Ubuntu 出貨的個人電腦，而且近幾年處理機和虛擬化技術的進步，所以若要安裝 Ubuntu 系統，我們也建議採用虛擬機器來安裝。Windows 系統上可以使用免費的 [VMware Player](http://www.vmware.com/products/player/) 和 [VirtualBox](https://www.virtualbox.org) 來建立虛擬機，但 VMware Player 的效能會比較好。在 OS X 上免費的解決方案就只有 VirtualBox。

## 準備 Ubuntu 環境

Ubuntu 有桌面和服務器二個版本，並且於每年的四月和十月釋出新版本，並以年份和月份作為版本號碼 (例如 2013 年四月釋出的版本就編號為 13.04)。另外還有一個以相同字首的形容詞和動物名組成的代號 (code name)，13.04 的代號為 "Raring Ringtail"。除了最初的三個版本外，代號的字首是按字母順序延續下來的。通常我們也會只以形容詞的部份來指涉某個版本，例如 "Precise" 就代表 12.04。另外每二年會有一版長期支援版 (LTS - long term support)，桌面版的支援期會長達三年，服務器版會長達五年。

作為開發環境，建議使用最新版的桌面版本，因為所需的開發工具會維持在較新的版本，例如 Git。

### 安裝在虛擬機上

為了有較好的效能，最好使用有支援[虛擬化指令集](http://en.wikipedia.org/wiki/X86_virtualization)的機器 (近年的新機器應該都已使用有支援的 CPU 了)，並且請勿必安裝 VMware Tools 和 VirtualBox Guest Addition。它們除了可以提昇效能外，還可以提供一些方便的功能，像是與主系統共用的共用資料夾，和較好的顯示支援。

## 準備 OS X 環境

首先要熟悉使用 Terminal.app 終端機，因為大部份開發都會需要使用命令列工具。另外為了使用眾多的開源軟體，最好能使用套件管理程式來安裝。OS X 上有三套套件管理程式

  - [MacPorts](http://www.macports.org)
  - [Homebrew](http://brew.sh)
  - [Fink](http://fink.sf.net)

而我們比較建議使用 MacPorts 或 Homebrew，以下的安裝說明都將以 MacPorts 環境來說明。

## 開發工具

### Git

必要的版本控制工具，需熟悉以命令列操作日常工作，並以 GUI 前端來幫助使用。

#### Ubuntu

    sudo apt-get install git git-flow

GUI 前端可採用 [RabbitVCS](http://rabbitvcs.org)，請參考官網[文件](http://wiki.rabbitvcs.org/wiki/install/ubuntu) 並按下列命令安裝套件:

    sudo add-apt-repository ppa:rabbitvcs/ppa
    sudo apt-get update
    sudo apt-get install rabbitvcs-nautilus3 rabbitvcs-gedit rabbitvcs-cli

    # bug workaround 
    sudo ln -sf /usr/lib/{i386 or x86_64}-linux-gnu/libpython2.7.so.1 /usr/lib/libpython2.7.so.1
    sudo ln -sf /usr/lib/{i386 or x86_64}-linux-gnu/libpython2.7.so.1.0 /usr/lib/libpython2.7.so.1.0

安裝完後，必須登出再登入才會運作。

#### OS X

    sudo port install git-core git-flow

GUI 前端可採用 [SourceTree](http://www.sourcetreeapp.com)，App Store 上的版本更新速度比較慢，最好是直接從官網下載安裝。

#### 設定 Git config

設定一些基本的設定

    git config --global user.name {你的名字}
    git config --global user.email {你的 email}

    # 使用命令列彩色輸出
    git config --global color.ui auto

    # 設定未指明 refspec 時的 git push 行為，simple 為 Git 2.0 的預設行為
    # 請參考 man git-config 中的 push.default 說明
    git config --global push.default simple

### JDK

要開發 Java 程式或要使用像是 JetBrains 或 Eclipse 等 IDE 的話，就需要安裝 JDK。

#### Ubuntu

透過第三方 ([webupd8team](https://launchpad.net/~webupd8team/+archive/java)) 的 PPA 安裝 Oracle JDK 7

    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java7-installer

#### OS X

雖然 OS X 內建的 Java VM 只支援到 1.6.x，但若只是用來執行 IDE 的話是足夠的，不需要安裝 Oracle 的 JDK。

### Python

Python 開發環境會以 [virtualenv](https://pypi.python.org/pypi/virtualenv) 來建立，以避免將 PyPI 套件安裝到系統目錄去。但系統的 Python 套件庫 (site-packages/dist-packages) 仍需有 1.9 版以上的 virtualenv，以用來建立 Python 虛擬環境 (virtualenv 1.9 以上至少會安裝 [pip](https://pypi.python.org/pypi/pip) 1.3.1 和 [setuptools](https://pypi.python.org/pypi/setuptools) 0.6.34)。

有些 Python 套件會需要 [Cython](https://pypi.python.org/pypi/Cython) 來建置模組，這可以安裝到虛擬環境也可以安裝到系統套件庫。

#### Ubuntu

安裝 python-virtualenv 會連帶安裝 python-pip 和 python-setuptools。

    sudo apt-get install python-virtualenv

    # 若想把 Cython 安裝到系統套件庫中，請用下列命令
    sudo apt-get install cython

#### OS X

在 OS X 下最好不要使用系統內建的 Python 執行環境，建議用 MacPorts 安裝另一份 Python 執行環境會比較好管理

    sudo port install python27 py27-virtualenv
    
    # 若想把 Cython 安裝到 MacPorts 的 Python 系統套件庫中，請用下列命令
    sudo port install py27-cython

### Node

為了方便管理 Node 套件，而不污染系統，我們建議採用 [NVM](https://github.com/creationix/nvm) 來安裝 Node 環境。NVM 會在使用者目錄中安裝 Node 環境並且支援同時安裝多個版本的 Node 環境。安裝 NVM 的方法不分 Ubuntu 和 OS X。

若 Node 環境是用來開發 web 前端的話，可以在 Node 的全域環境中安裝下列工具套件

  - [Yeoman](http://yeoman.io)
  - [Bower](http://bower.io)
  - [Grunt](http://gruntjs.com)

依照 [NVM](https://github.com/creationix/nvm) 的安裝說明安裝 NVM 和 Node。

安裝 NVM

    curl https://raw.github.com/creationix/nvm/master/install.sh | sh

或

    wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh

安裝 Node

    nvm install 0.10.19
    nvm use 0.10.19
    nvm alias default 0.10.19

安裝前端開發工具套件到全域環境

    npm install -g yo bower grunt-cli

### Ruby

Ruby 環境和 Node 環境類似，為了不因為安裝 Ruby 套件而污染系統內建的 Ruby 環境，我們也建議使用 [RVM](http://rvm.io) 來安裝 Ruby 執行環境。NVM 基本上是學 RVM 的，所以用法上是類似的。它也會在使用者目錄中安裝多個版本的 Ruby 執行環境。同樣的，安裝 RVM 的方法也不分 Ubuntu 和 OS X。

我們安裝 Ruby 執行環境主要是為了進行下列工作

  - 使用 [Compass](http://compass-style.org) 編譯 [SASS](http://sass-lang.com) 樣式檔
  - 使用 [Jekyll](http://jekyllrb.com) 將 Markdown 文件檔轉成靜態網站或 Blog

依照 [RVM](http://rvm.io) 的安裝說明安裝 RVM 和 Ruby。

安裝 RVM

    \curl -L https://get.rvm.io | bash

安裝 Ruby

    rvm insstall 1.9.3
    rvm use 1.9.3 --default

安裝開發工具套件到全域環境

    # 安裝 compass 工具
    gem install compass

    # 安裝 Jekyll 工具
    gem install jekyll

### 文件和圖表

我們需要下列工具來轉換 Markdown 文件檔和繪製圖表

  - [Pandoc 文件轉換器](http://johnmacfarlane.net/pandoc/)
  - [LaTeX 排版系統](http://www.latex-project.org) ([TeX Live](http://www.tug.org/texlive/) 或 [MacTeX](http://www.tug.org/mactex/))
  - [Graphviz 圖形視覺化工具](http://www.graphviz.org)
  - [PlantUML UML 圖表繪製工具](http://plantuml.sourceforge.net)

#### Ubuntu

安裝 Pandoc

    sudo apt-get install pandoc

安裝 LaTeX

    sudo apt-get install texlive texlive-xetex

安裝 Graphviz

    sudo add-apt-repository ppa:gviz-adm/graphviz-dev
    sudo apt-get update
    sudo apt-get install graphviz

安裝 PlantUML

從 Wubicu 在 GitHub 上的 plantuml\_wrapper (<http://github.com/wubicu/plantuml_wrapper>) 專案下載安裝到 `/usr/local` 目錄下。

#### OS X

安裝 Pandoc

從 Pandoc 網站[下載](http://johnmacfarlane.net/pandoc/installing.html) OS X 的安裝程式 (DMG 檔) 安裝。`pandoc` 執行檔將被裝到 `/usr/local/bin` 目錄下。

安裝 LaTeX

從 MacTeX 網站[下載](http://tug.org/mactex/morepackages.html) `BasicTeX.pkg` OS X 套件來安裝。此套件會安裝我們所需要的 LaTeX 元件 (XeLaTeX 等)。

安裝 Graphviz

從 Graphviz 網站[下載](http://www.graphviz.org/Download_macos.php)與 OS X 版本相符的套件檔安裝。

安裝 PlantUML

從 Wubicu 在 GitHub 上的 plantuml\_wrapper (<http://github.com/wubicu/plantuml_wrapper>) 專案下載安裝到 `/usr/local` 目錄下。

### Sublime Text 3 編輯器

安裝 Sublime Text 3

從 Sublime Text 3 網站[下載](http://www.sublimetext.com/3)安裝程式或套件來安裝。在 OS X 下可以用

    sudo ln -s /Applications/Sublime Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl

建立符號連結以方便在命令列下叫用 Sublime Text 3 編輯器。

安裝 Pacakge Control


安裝所需套件


設定 Pandown


### 雲端和虛擬機

  - libvirt
  - vagrant
  - euca2tools

#### Ubuntu


#### OS X

