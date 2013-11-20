# 用 Vagrant 快速建立開發環境

[Vagrant](http://www.vagrantup.com) 是一套可以幫助建立和配置虛擬開發環境的工具。一般 web 應用的開發和部署過程中，常需要使用建立虛擬機並部署相關軟體來進行測試。這個過程通常包含了許多的操作，既繁瑣又容易犯錯。而且，面對不同的虛擬機環境和作業系統，通常也有不同的作法。Vagrant 將這些動作包裝起來，並以簡單且一致的設定檔來設定配置。大大的簡化建立虛擬機和安裝部署軟體到虛擬機的動作，而且是可移植的，他人可以快速的使用已設定好的配置檔來建立相同的環境。

Vagrant 內建支援的虛擬機平台 (*provider*) 是 VirtualBox，另外以 plugin 的形式支援其他的虛擬和雲端平台，像是 VMware (須付費)、AWS、OpenStack 和 libvirt 等。

Vagrant 將自動安裝軟體到虛擬機內並進行配置的支援稱作 *provisioning* ，內建支援以 shell script、Chef 和 Puppet 等配置管理工具來進行安裝配置。

Vagrant 的使用並不難，這篇文章只做簡單的介紹教學，詳細的細節請參考官網的[文件說明](http://docs.vagrantup.com/v2/)。

## 安裝 Vagrant

自[官網下載](http://downloads.vagrantup.com) 各平台的安裝套件安裝即可。

## Vagrant 詞彙

在開始之前，先稍微解釋一下 Vagrant 所用的詞彙

Vagrantfile
  : Vagrantfile 主要的目的就是用來描述專案所需的虛擬機形式，還有如何配置和部署這個機器。Vagrantfile 的檔名必須就是 `Vagrantfile` (不分大小寫)。  
  Vagrantfile 通常都應該加入到版本控制系統內，專案內的其他人才能夠取得它，執行 `vagrant up` 來帶起他自己的一份環境。另外，Vagranfile 在 Vagrant 支援的平台間是可攜的。  
  Vagrantfile 的語法是 Ruby 語法，但因為內容大部份都是單純的變數指定，所以不需要會 Ruby 就可以進行修改。

Boxes
  : Boxes 是建立虛擬機的骨架，它含有虛擬機的映像檔和基本設定，而月它們是可移植的檔案。其他人可以在任何可以執行 Vagrant 的平台上使用它快速建立一個相同的環境。  
  Boxes 是與 provider 相關的，所以你必須使用符合所用 provider 的 box。

Providers
  : Providers 提供虛擬機平台支援。Vagrant 內建支援的虛擬機平台是 VirtualBox，如果要使用其他的虛擬機平台，就需要安裝 provider。

Provisioning
  : Provisioning 讓你可以自動安裝並配置軟體到虛擬機裡。雖然你可以在虛擬機啟動後，透過 SSH 連線進入並手動安裝和配置軟體。但使用 provisioning 可以自動化這個過程並且可以重覆。更重要的是，它完全不需要使用者介入，所以你可以簡單的使用一個指令就可獲得一個完整可工作的環境。
  Vagrant 提供你多種選擇來 provisioning 你的虛擬機，可以用 shell script 或者是使用 Cheif 和 Puppet 等配置管理工具。

Plugins
  : 透過 Plugins 你可以改變 Vagrant 的行為或增加新功能。Vagrant 的核心本身也是以 plugin 的形式來實作，並使用相同的 API 來實作。因此這個機制是穩定且有良好支援的。

## 開始使用

安裝完 Vagrant 後，可以馬上用下列命令快速在 VirtualBox 平台上建立一個虛擬機

    vagrant init precise32 http://files.vagrantup.com/precise32.box
    vagrant up

這二個命令會在 VirtualBox 中建立一個運作 Ubuntu 12.04 32-bits 系統的虛擬機。接著你可以用 `vagrant ssh` 命令使用 SSH 登入此虛擬機。在使用結束登出後，你可以用 `vagrant destroy` 移除它。

接下來就稍微進一步說明 Vagrant 的使用。

### 建立專案

使用 Vagrant 的第一件事就是要建立 `Vagrantfile`。你可以使用 `vagrant init` 命令來將一個目錄初始化為 Vagrant 的專案目錄。這個命令會幫你建立一個 `Vagrantfile`，你可以依照裡面的註解內容來修改設定。`Vagrantfile` 通常都會與你的專案一起加入版本控制，參與此專案的其他人就可以使用它來建立他們的開發測試環境。

### 準備 Boxes

從無到有建立虛擬機是花時間且繁瑣的工作，Vagrant 採用基本的映像檔來快速複製建立虛擬機。這些基本映像檔就是 Vagrant 所謂的 boxes。指明 Vagrant 環境所要使用的 box 是建立 `Vagrantfile` 後的第一件事。 

安裝完後 Vagrant 環境中是沒有任何 boxes 的，你可以 `vagrant box list` 指令查看。但如果你已經照著一開始的說明啟動了 Ubuntu 的虛擬機，那就會有 `precise32` 這個 box 存在。如果沒有的話，你可以下列命令下載 `precise32` box 到 Vagrant 環境中

    vagrant box add precise32 http://files.vagrantup.com/precise32.box

如果 `Vagrantfile` 已存在，請打開它並確認 `config.vm.box` 的值是 `precise32`。如果不存在，可以下列命令初始 `Vagranfile` 和專案

    vagrant init precise32

這二個命令就相當於一開始範例中的指令

    vagrant init precise32 http://files.vagrantup.com/precise32.box

### 啟動並用 SSH 存取

接著用下列命令啟動虛擬機

    vagrant up

過幾分鐘後，當這指令執行結束後，你就有一個運作中的虛擬機。此虛擬機是沒有 UI 的，但你可以用 SSH 連線登入它

    vagrant ssh

當你使用結束登出後，可以使用 `vagrant destroy` 來關畢並移除虛擬機。

### 同步檔案夾

Vagrant 提供*同步檔案夾*的功能，它會將你的 Vagrant 目錄分享至客端虛擬機中的 `/vagrant`。所以你可以在主端系統內編輯你的專案內容，它們會同步反應到客端上。

### 部署

假如你啟動虛擬機的目的是為了運作一個靜態網站，你的專案目錄裡就是網站的內容。此時你可以用 SSH 連入並手動安裝 web server，但是其他人也要跟你做一樣的事才能有相同的環境。所以 Vagrant 提供自動部署的功能，它可以在執行 `vagrant up` 時自動進行安裝部署，所以每個使用者都會得到相同的環境。

這裡以安裝部署 Apache 為例，並且以 shell script 的方式來進行 provisioning。請建立下列的 shell 命令稿，並儲存為 `bootstrap.sh`

~~~
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
rm -rf /var/www
ln -fs /vagrant /var/www
~~~

接著編輯 `Vagrantfile` 來讓 Vagrant 使用 `bootstrap.sh` shell 命令稿來進行 provisioning

~~~
Vagrant.configure("2") do |config|
  config.vm.box = "precise32"
  config.vm.provision :shell, :path => "bootstrap.sh"
end
~~~

`bootstrap.sh` 檔案的路徑是相對於專案根目錄 (`Vagrantfile` 所在的目錄)。

設定好了就可以使用 `vagrant up` 來啟動虛擬機，你應該可以在終端機上看到安裝部署的過程。如果你的虛擬機已經啟動了，你可以執行 `vagrant reload --provision` 命令來快速重啟虛擬機並執行部署命令。

### 網路

到目前為止，我們已經啟動虛擬機並運行了 web server，而且可以從主端編輯專案內容，但是卻無法從主端存取網頁。這時我們就要使用 Vagrant 的網路功能來讓我們可以從主端存取虛擬機。

我們可以使用 Vagrant 的 *port forwarding* 功能來透過主端的埠存取客端的埠。所以我們就可以在主端上存取客端上的 web server。下面的設定可以讓我們從主端的 4567 埠存取客端的 80 埠

~~~
Vagrant.configure("2") do |config|
  config.vm.box = "precise32"
  config.vm.provision :shell, :path => "bootstrap.sh"
  config.vm.network :forwarded_port, host: 4567, guest: 80
end
~~~

執行 `vagrant reload` 或 `vagrant up` 來讓設定作用。這時用主端的瀏覽器存取 `http://127.0.0.1:4567` 就可以看到客端網站的內容。

### 關掉

要關掉 Vagrant 虛擬機有三種方式 `suspend`、`halt` 和 `destroy`。這三種方式各有優缺點，請依用途選擇合適的方式。

**Suspend** 會將目前狀態儲存下來並停掉虛擬機，當你用 `vagrant up` 重新啟動時，它會回到關掉時狀態。這種方式的好處是它非常快，大約 5 ~ 10 秒就可以啟動和關掉。缺點是它需要磁碟空間來儲存 RAM 和磁碟的狀態。

**Halt** 會正常的關掉系統，但會保留磁碟上的內容。好處是它會正常且乾淨的開關機，缺點是它會佔用磁碟空間來儲存磁碟狀態。

**Destroy** 會關掉並移除虛擬機。所以它的好處是它會清除所有的操作痕跡，也不佔用磁碟空間來儲存任何狀態。缺點是每次啟動都必須花費多一點的時間，因為要重新建立虛擬機和部署軟體。

### 重建虛擬機或再啟動

任何時候，只要需要啟動虛擬機，執行

    vagrant up

即可。

### Providers

Vagrant 內建使用的虛擬平台是 VirtualBox，要使用 VMware 或雲端平台的話，就要安裝 provider 的 plugin。例如

  - [vagrant-libvirt](https://github.com/pradels/vagrant-libvirt)
  - [vagrant-aws](https://github.com/mitchellh/vagrant-aws)
  - [vagrant-openstack-plugin](https://github.com/cloudbau/vagrant-openstack-plugin)

#### libvirt

請用下列命令安裝

    vagrant plugin install vagrant-libvirt

啟動時要指明 provider

    vagrant up --provider=libvirt

另外，要注意的是 boxes 是與 provider 相關的，不同的 provider 要使用不同 boxes，所以必須要先安裝可使用的 boxes 到 Vagrant 環境裡。我們的 archive 伺服器有準備幾個 libvirt 的 box 可用。可以下列命令來安裝。

    vagrant box add precise32 http://archive.wubicu.com/vagrant-boxes/libvirt/precise32.box

使用 [vagrant-libvirt](https://github.com/pradels/vagrant-libvirt) 要注意幾點

  1. vagrant-libvirt 的 SSH 連線支援有點問題，它只能使用檔名為 `id_rsa` 的 SSH 金鑰對來驗證。

  2. vagrant-libvirt 目前只支援用 [MacVTap](http://virt.kernelnewbies.org/MacVTap) 的 [libvirt 網路界面](http://www.libvirt.org/formatdomain.html#elementsNICSDirect)來提供公開網路 (public network) 功能 (可從主端外的機器連入)。
  
    因為 MacVTap 必須使用一個實體的網路界面，所以如果只有一個實體網路界面而且又使用 `brctl` 來建立橋接界面給其他 libvirt domain 用的話，那就沒辦法用 vagrant-libvirt 建立有公開網路的虛擬機。

#### aws

使用下列命令安裝 vagrant-aws provider 和 dummy box (image 在 AWS 上，box 本身不需要提供 image)

    vagrant-plugin install vagrant-aws
    vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

初始 Vagrant 專案後，並參考下列內容來設定 Vagrantfile

~~~
    ...
    config.vm.box = "dummy"
    ...
    config.vm.provider :aws do |aws, override|
        aws.access_key_id = "AWS ACCESS KEY ID"
        aws.secret_access_key = "AWS SECRET ACCESS KEY"
        aws.keypair_name = "金鑰對名稱"

        aws.region = "AWS 的區域"                 # 例如 ap-southeast-1
        aws.instance_type = "EC2 實例類型"         # 例如 ti.micro
        aws.ami = "AMI 的 ID"                   # 例如 ami-5afeab08
        aws.security_groups = [要使用的安全群組名清單]

        override.ssh.username = "ubuntu"
        override.ssh.private_key_path = "金鑰對的私鑰檔路徑"
    end
~~~

然後執行 `vagrant up --provider=aws` 就可以啟動 AWS EC2 實例。

#### openstack

使用下列命令安裝 vagrant-openstack-plugin provider 和 dummy box (image 在 OpenStack 上，box 本身不需要提供 image)

    vagrant plugin install vagrant-openstack-plugin
    vagrant box add dummy github.com/cloudbau/vagrant-openstack-plugin/raw/master/dummy.box

初始 Vagrant 專案後，並參考下列內容來設定 Vagrantfile

~~~
    ...
    require 'vagrant-openstack-plugin'
    ...
    config.vm.box = "dummy"
    ...
    ssh.private_key_path = "金鑰對的私鑰檔路徑"
    ...
    config.vm.provider :openstack do |os|
        aws.access_key_id = "AWS ACCESS KEY ID"
        aws.secret_access_key = "AWS SECRET ACCESS KEY"
        aws.keypair_name = "金鑰對名稱"

        aws.region = "AWS 的區域"                 # 例如 ap-southeast-1
        aws.instance_type = "EC2 實例類型"         # 例如 ti.micro
        aws.ami = "AMI 的 ID"                   # 例如 ami-5afeab08
        aws.security_groups = [要使用的安全群組名清單]

        os.username = "使用者名稱"                          # 或用 "#{ENV['OS_USERNAME']}"
        os.api_key = "使用者密碼"                           # 或用 "#{ENV['OS_PASSWORD']}"
        os.flavor = “flavor 名稱”                        # 如 m1.small
        os.image = "系統映像名稱"
        os.endpoint = "OpenStack Keystone 端點 URI"      # 或用 "#{ENV['OS_AUTH_URL']}/tokens"
        os.keypair_name = "金鑰對名稱"
        os.ssh_username = "SSH 的使用者名稱"

        os.security_groups = [要使用的安全群組名清單]
        os.tenant = "OpenStack 專案名稱"
        os.floating_ip = "所要關連的浮動 IP"
    end
~~~

然後執行 `vagrant up --provider=openstack` 就可以啟動 OpenStack nova 的實例。
