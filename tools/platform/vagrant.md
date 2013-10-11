# 用 Vagrant 快速建立開發環境

[Vagrant](http://www.vagrantup.com) 是一套可以幫助建立和配置虛擬開發環境的工具。一般 web 應用的開發和部署過程中，常需要使用建立虛擬機並部署相關軟體來進行測試。這個過程通常包含了許多的操作，既繁瑣又容易犯錯。而且，面對不同的虛擬機環境和作業系統，通常也有不同的作法。Vagrant 將這些動作包裝起來，並以簡單且一致的設定檔來設定配置。大大的簡化建立虛擬機和安裝部署軟體到虛擬機的動作，而且是可移植的，他人可以快速的使用已設定好的配置檔來建立相同的環境。

Vagrant 內建支援的虛擬機平台 (*provider*) 是 VirtualBox，另外以 plugin 的形式支援其他的虛擬和雲端平台，像是 VMware (須付費)、AWS、OpenStack 和 libvirt 等。

Vagrant 將自動安裝軟體到虛擬機內並進行配置的支援稱作 *provisioning* ，內建支援以 shell script、Chef 和 Puppet 等配置管理工具來進行安裝配置。

Vagrant 的使用並不難，這篇文章只做簡單的介紹教學，詳細的細節請參考官網的[文件說明](http://docs.vagrantup.com/v2/)。

## 安裝 Vagrant

自[官網下載](http://downloads.vagrantup.com) 各平台的安裝套件安裝即可。

## 開始使用

在開始之前，先稍微解釋一下 Vagrant 所用的詞彙

Vagrantfile
  : Vagrantfile 主要的目的就是用來描述專案所需的虛擬機形式，還有如何配置和部署這個機器。Vagrantfile 的檔名必須就是 `Vagrantfile` (不分大小寫)。  
  Vagrantfile 通常都應該加入到版本控制系統內，專案內的其他人才能夠取得它，執行 `vagrant up` 來帶起他自己的一份環境。另外，Vagranfile 在 Vagrant 支援的平台間是可攜的。  
  Vagrantfile 的語法是 Ruby 語法，但因為內容大部份都是單純的變數指定，所以不需要會 Ruby 就可以進行修改。

Boxes
  : Boxes 是建立虛擬機的骨架，它含有虛擬機的映像檔和基本設定，而月它們是可移植的檔案。其他人可以在任何可以執行 Vagrant 的平台上使用它快速建立一個相同的環境。  
  Boxes 是與 provider 相關的，所以你必須使用符合所用 provider 的 box。

Providers
  : 定義

Provisioners
  : 定義

Plugins
  : 定義

### 建立專案

### 準備 Boxes

### 啟動並用 SSH 存取

### 同步檔案夾

### 部署

### 網路

### 關掉

### 再啟動

### Providers

#### libvirt

