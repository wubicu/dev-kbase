# Ubuntu 系統的設定和配置

## 管理者帳號的設定

Ubuntu 系統預設是關閉 root 帳號，而另外在安裝系統時建立一個有 sudo 權利的管理者帳號來管理系統。我們可以再做進一步的設定讓這個帳號更安全也更方便使用。

  - 只允許使用 SSH 金鑰登入

    修改 `/etc/ssh/sshd_config`，加入下列設定

        PasswordAuthentication no

    以關閉 SSH 允許使用密碼登入的功能。建議所有 Ubuntu 系統都要採用這個做法來加強安全性。

  - 不需密碼使用 sudo

    在 `/etc/sudoers.d` 目錄下建立有如下內容的設定檔

        ubuntu ALL=(ALL) NOPASSWD:ALL

    `ubuntu` 是管理者帳號名，請換成你的管理者帳號名。這個設定檔的存取權必須是 `0440`，檔名最好是以二位數字開頭以決定套用的順序，如 `90-clouding-ubuntu`。

  - 關閉密碼登入功能 (disalbe password)

    密碼可用 `passwd -l` 來關閉。如此一來，就不能用密碼來登入系統，只能用其他的認證方式登入，例如 SSH 金鑰。這個做法只建議用在可輕易重建的系統上。
    
