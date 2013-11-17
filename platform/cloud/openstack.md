# 建置 OpenStack 環境

[OpenStack](http://www.openstack.org/) 是一個雲端平台專案，目標是提供 [IaaS](http://en.wikipedia.org/wiki/Infrastructure_as_a_service#Infrastructure_as_a_service_.28IaaS.29) 服務。它採取 Apache 授權。目前這個專案有許多大公司的參與，包括 IBM、Intel 等。專案中包含了數個相關的子專案，分別負責提供管理資料中心內的計算、儲存、網路等資源的功能。OpenStack 大約六個月就會釋出一次新版本。2013 年的版本分別為 Grizzly (四月) 和 Havana (十月)。

## 主要元件

OpenStack 的元件會隨著版本而增加變化，目前 (Havana) 有下列主要的元件。

### Compute (Nova)

Nova 提供運算資源的管理。它的架構設計為易於水平擴展，而且不需要特殊的硬體支援。它可以使用多種虛擬化技術，像是 [KVM](http://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine)、[Xen](http://en.wikipedia.org/wiki/Xen)、[Hyper-V](http://en.wikipedia.org/wiki/Hyper-V) 和 [LXC](http://en.wikipedia.org/wiki/LXC) 等。除此之外，OpenStack 可以運行在 ARM 系統上。

### Object Storage (Swift)

Swift 是個可擴展的冗餘儲存系統。物件和檔案會被分散儲存在資料中心的機器上的多台磁碟內，Swift 會負責這些資料的複製和完整性。要擴展儲存容量只需要增加伺服器即可。Rackspace Cloud File 就是利用 Swift 提供類似 Amazon S3 的服務。

### Block Storage (Cinder)

Cinder 提供持續性的區塊層級 (block-level) 儲存設備 (raw drive) 給 Nova 的虛擬機使用。區塊儲存設備適用於需要高效能儲存的應用，如資料庫或分散式檔案系統等。

### Networking (Neutron)

Neutron 負責管理網路和 IP 位址，提供網路虛擬化功能。Neutron 在 Havana 之前稱為 Quantum。

### Dashboard (Horizon)

Horizon 提供 web 界面讓管理者和使用者存取 OpenStack 內的資源。

### Identity Service (Keystone)

Keystone 提供共用的認證機制給其他元件進行使用者認證和存取控制。

### Image Service (Glance)

Glance 提供管理系統映像檔的功能。

## 安裝 OpenStack

OpenStack 的安裝並沒有一個單一的安裝程序，按不同的需求和資源會有不同的安裝方式。因此第一次接觸時很容易無所適從。如果只是安裝來玩玩的話，可以將所有元件都裝到一台機器上。網路上有很多命令稿可以幫忙進行這種簡單安裝。

若要安裝一個實際可用的環境的話，最低限度的硬體需求是二台主機，CPU 最好是四核心以上，RAM 要 16 GB 以上。OpenStack 有[文件說明](http://docs.openstack.org/)如何在各種常見的 GNU/Linux 發行版本上的詳細安裝方法。

除此之外，還可以使用 [Chef](http://en.wikipedia.org/wiki/Chef_(software)) 或 [Puppet](http://en.wikipedia.org/wiki/Puppet_(software)) 等配置管理工具來幫忙安裝部署 OpenStack 到叢集裡。採用這種方式的好處是方便擴展和昇級整個環境，而且不容易犯錯。但壞處就是要設計和撰寫部署的命令稿。幸好網路上有許多專案是在開發部署 OpenStack 的命令稿，我們可以直接採用。

### 使用 Rackspace Private Cloud Software 來安裝

[Rackspace Private Cloud Software](http://www.rackspace.com/cloud/private/openstack_software/)  是 Rackspace 提供的開源 Chef Cookbooks。使用它來安裝 OpenStack 除了省事外，它還支援 high availablity 支援 (但需要較多硬體支援)。這篇文章只簡單說明安裝的過程，詳細的解說和注意事項請參考 [Rackspace Private Cloud Software 的文件](http://www.rackspace.com/knowledge_center/getting-started/rackspace-private-cloud)。

#### 硬體需求

所需的硬體和建議的最低需求如下

-  一台 Chef 伺服器

    4 核 CPU，4 GB RAM，50 GB 磁碟空間

-  一台 OpenStack Nova controller 節點

    4 核 CPU，16 GB RAM，144 GB 磁碟空間

-  一台以上的 OpenStack Nova Compute 節點

    4 核 CPU，32 GB RAM，144 GB 磁碟空間

#### 軟體需求

Rackspace 已測試過的作業系統有

- Ubuntu 12.04
- CentOS 6.3
- CentOS 6.4

#### 網路需求

安裝過程中需要從 Internet 上存取資料，所以各機器必須能存取到 Internet。另外，這些實體機必須是採用固定的 IP 位址，彼此間最好都能用完整的域名 (FQDN) 存取到。

Rackspace Private Cloud cookbook 預設會安裝 nova-network 來支援網路功能，要使用 Neutron (或 Quantum) 的話，須要額外的安裝步驟。

Rackspace Private Cloud cookbook 規劃了三種邏輯上的網路

- nova

    nova 服務所綁定的 IP 位址的所在網路

- public

    OpenStack API 端點和管理界面所綁定的 IP 位址的所在網路

- management

    管理用的 API 端點和服務以及內部所需服務 (資料庫和佇列等) 所綁定的 IP 位址的所在網路

這些網路都用 [CIDR 方式](http://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)來描述，並且可以相同的設定。

Rackspace Private Cloud cookbook 支援多網卡和單網卡，但單網卡時又用多網路的設定時需要對網卡作額外的設定。

這篇文章將只以單網卡且將三種網路都規劃在 192.168.1.0/24 的設定來進行安裝。

#### 準備機器

如果沒有設定 DNS 的話，可以使用 [mDNS](http://en.wikipedia.org/wiki/MDNS) (在 Ubuntu 上可以安裝 `avahi-daemon` 套件) 讓各節點都有自己的 FQDN，並可以被其他的節點以 FQDN 來存取，但要注意每台機器是否可以取得自己的 FQDN (可用 `hostname -f` 確認)，若不行的話，可在 `/etc/hosts` 加上 FQDN。如

    ...
    127.0.1.1    chef.local chef
    ...

確定每個節點的系統是否已更新到最新狀態。而且每個節點都有著有相同名稱的管理者使用者。

確定系統上有 `curl`。

#### 安裝 Chef 服務器

登入作為 Chef 服務器的節點，切換為 `root` 執行下列命令安裝 Chef sever

    # curl -s -L https://raw.github.com/rcbops/support-tools/master/chef-install/install-chef-server.sh > install-chef-server.sh
    # bash install-chef-server.sh 

安裝完後，執行 `knife client list` 來驗證是否安裝成功。

#### 安裝 Rackspace Private Cloud Cookbooks

下載 `install-cookbooks.sh`

    # curl -s -L https://raw.github.com/rcbops/support-tools/master/chef-install/install-cookbooks.sh

開啟 `install-cookbooks.sh` 確認 cookbooks 的版本是否正確

    COOKBOOK_BRANCH=${COOKBOOK_BRANCH:-<versionNumber>}

目前可能的版號為 v4.2.0，v4.1.2 和 v4.0.0 (請參考各版本的 Release Notes)，安裝 OpenStack Grizzly 的話，請用 v4.1.2，Havana 則用 v4.2.0。

執行 `install-cookbooks.sh`

    # bash install-cookbooks.sh

#### 安裝 Chef 客戶端

在 Chef 服務器的節點上，切換回管理者帳號，使用 `ssh-keygen` 來產生 SSH 金錀對，並將公鑰複製到其他要安裝 Chef 客戶端的節點上

    $ ssh-copy-id <user>@<deviceHostname> 

下載 `install-chef-client.sh` 並讓它可執行

    $ curl -skS https://raw.github.com/rcbops/support-tools/master/chef-install/install-chef-client.sh > install-chef-client.sh
    $ chmod +x install-chef-client.sh

執行它來安裝 Chef 客戶端到其他節點上

    $ ./install-chef-client.sh <deviceHostname>

#### 安裝 OpenStack

##### 建立 Chef 環境

建立新的 Chef 環境來部署 OpenStack

    # knife environment create private-cloud -d "Rackspace Private Cloud OpenStack Environment"

編輯環境來設定 override attributes

    # knife environment edit private-cloud

##### 定義網路層性

現在必須設定 override attributes 來設定 nova，public 和 management 三個網路，語法如下 (v4.0.0 的語法請參考文件)

    "override_attributes": {
        "nova": {
            "network": {
                "public_interface": "<publicInterface>"
            },
            "networks": {
                "public": {
                    "label": "public",
                    "bridge_dev": "<VMNetworkInterface>",
                    "dns2": "8.8.4.4",
                    "ipv4_cidr": "<VMNetworkCIDR>",
                    "bridge": "<networkBridge>",
                    "dns1": "8.8.8.8"
                }
            }
        },
        "mysql": {
            "allow_remote_root": true,
            "root_network_acl": "%"
        },
        "osops_networks": {
            "nova": "<novaNetworkCIDR>",
            "public": "<publicNetworkCIDR>",
            "management": "<managementNetworkCIDR>"
        }
    }

依照此篇文章的設定 (單 NIC，三個網路都設定為 192.168.1.0/24)，則設定如下

    "override_attributes": {
        "nova": {
            "network": {
                "public_interface": "br100"
            },
            "networks": {
                "public": {
                    "label": "public",
                    "bridge_dev": "eth0",
                    "dns2": "8.8.4.4",
                    "ipv4_cidr": "10.0.0.0/24",
                    "bridge": "br100",
                    "dns1": "8.8.8.8"
                }
            }
        },
        "mysql": {
            "allow_remote_root": true,
            "root_network_acl": "%"
        },
        "osops_networks": {
            "nova": "192.168.1.0/24",
            "public": "192.168.1.0/24",
            "management": "192.168.1.0/24"
        }
    }

##### 設定節點環境

為了確定所有的變動有被套用，必須將所有節點的 Chef 環境和 Chef 服務器的環境維持一致

    # knife exec -E 'nodes.transform("chef_environment:_default") \
     { |n| n.chef_environment("<environmentName>") }'

這命令會把所有節點的環境改動，所以如果有非 OpenStack 的節點在裡面的話，它的 Chef 環境也會被改動。

##### 新增 Controller 節點

新增 `single-controller` role 到目的節點的 run_list 裡

    # knife node run_list add <deviceHostname> 'role[single-controller]'

使用 ssh 登入目的節點，並執行

    sudo chef-client

##### 新增 Compute 節點

在 Controller 節點建立後就可以來安裝 Compute 節點

新增 `single-compute` role 到目的節點的 run_list 裡

    # knife node run_list add <deviceHostname> 'role[single-compute]'

使用 ssh 登入目的節點，並執行

    sudo chef-client

## 開始使用 OpenStack

### 存取 Dashboard

使用瀏覽器連線到 Controller 節點，如果安裝正確的話，應該可以看到 OpenStack Horizon 的登入頁面。使用 Rackspace Private Cloud Cookbooks 安裝的話，預設的帳號密碼分別是 "admin" 和 "secrete"。

### 上傳系統映像檔

虛擬機啟動後必須有系統可以運行，所以在建立虛擬機前必須要先準備好系統映像檔讓虛擬機可以使用。上傳映像檔到 OpenStack 中有許多方法，最簡單的方法就是使用 Dashboard 建立映像檔的功能來上傳。這個功能提供二種方法來取得映像檔

  1. 讓使用者指定映像檔的 URI，系統會從此 URI 取得映像檔
  2. 讓使用者從存取管理頁面的系統上傳映像檔

要給 OpenStack 使用的映像檔必須要符合一些條件，詳細說明請參考 [OpenStack Virtual Machine Image Guide](http://docs.openstack.org/image-guide/content/)。

Canonical 有維護一個官方的 [Ubuntu cloud image](http://docs.openstack.org/image-guide/content/ch_obtaining_images.html#ubuntu-images)，它以 Ubuntu 的版本和釋出日期來組織，'current' 目錄中的是最新的釋出。 

### 設定網路

如果安裝了 Neutron (Quantum)，就需要先建立虛擬網路，請參考 [OpenStack Networking Administration Guide](http://docs.openstack.org/grizzly/openstack-network/admin/content/)。

### 建立專案

每個虛擬機實例必須屬於某個專案，所以在啟動虛擬機前要先建立專案，請使用管理界面來建立專案。

### 建立存取金鑰對

要使用 SSH 安全的登入虛擬機可以使用 SSH 金鑰登入，OpenStack 可以在啟動虛擬機時將可存取的公鑰放入虛擬機內。所以在啟動前，必須指定可用來存取該虛擬機的金鑰對。金鑰對也是屬於專案的資源，一個專案可以建立多個金鑰對。

### 更新安全群組

安全群組是一組套用到流入封包的規則，符合規則的封包才能存取該虛擬機，否則將會被拒絕。一般來說，至少要允許 SSH 和 ping 連線。因此套用到虛擬機實例上的安全群組中，至少要有下列規則。

    +-------------+-----------+---------+-----------+--------------+
    | IP Protocol | From Port | To Port |  IP Range | Source Group |
    +-------------+-----------+---------+-----------+--------------+
    |     icmp    |     -1    |    -1   | 0.0.0.0/0 |              |
    |     tcp     |     22    |    22   | 0.0.0.0/0 |              |
    +-------------+-----------+---------+-----------+--------------+

### 建立虛擬機實例



### 存取虛擬機



#### 為 Nova-Network 建立可配置的浮動 IP 位址 


