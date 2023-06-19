# vagrant_ansible_tomcat9

## 概要
- Vagrant と Ansible を使い Tomcat9 を起動するサンプル

## 前提
以下がインストールされていること
- VirtualBox
- Vagrant

※インストールする場合は winget が手軽でオススメ
```
winget install Oracle.VirtualBox
winget install Hashicorp.Vagrant
```

## 実行
- 本フォルダ上で ```vagrant up``` を実行 
- しばらく待って以下のログが表示されたら完了
- http://192.168.33.10:8080 にアクセスして Tomcat のトップページが表示されることを確認
```
$ vagrant up     
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'almalinux/9'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'almalinux/9' version '9.2.20230513' is up to date...
==> default: Setting the name of the VM: vagrant_ansible_tomcat9_default_1687183087604_16170
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => C:/Users/Tobotobo/Desktop/vagrant/vagrant_ansible_tomcat9
==> default: Running provisioner: ansible_local...
    default: Installing Ansible...
Vagrant gathered an unknown Ansible version:


and falls back on the compatibility mode '1.8'.

Alternatively, the compatibility mode can be specified in your Vagrantfile:
https://www.vagrantup.com/docs/provisioning/ansible_common.html#compatibility_mode
    default: Running ansible-playbook...

PLAY [default] *****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [default]

TASK [java-17-openjdk のインストール] ******************************************
changed: [default]

TASK [java-17-openjdk-devel のインストール] ************************************
changed: [default]

TASK [validate if Java is availble] ********************************************
changed: [default]

TASK [tomcat グループを作成] ***************************************************
changed: [default]

TASK [tomcat ユーザーを作成] ***************************************************
changed: [default]

TASK [/opt/tomcat9 フォルダを作成] *********************************************
changed: [default]

TASK [Tomcat をダウンロード] ***************************************************
changed: [default]

TASK [Move files to the /opt/tomcat9 directory] ********************************
changed: [default]

TASK [Creating a service file] *************************************************
changed: [default]

TASK [Reload the SystemD to re-read configurations] ****************************
ok: [default]

TASK [Enable the tomcat service and start] *************************************
changed: [default]

TASK [Connect to Tomcat server on port 8080 and check status 200 - Try 5 times] ***
FAILED - RETRYING: [default]: Connect to Tomcat server on port 8080 and check status 200 - Try 5 times (5 retries left).
ok: [default]

PLAY RECAP *********************************************************************
default                    : ok=13   changed=10   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```


## メモ
```
vagrant init
```

```
vagrant up
```

SSH  
VSCodeからVagrantにリモートアクセスする  
https://qiita.com/yamashin0616/items/87e042b3f5ec52e57017  
※注意：ssh-configではPort 2222でも22にフォワーディングされているためvscodeからは22でアクセスすること
```
vagrant ssh
vagrant ssh-config
```

Vagrantfile の変更は内容にもよるが config.vm.network の変更など、ちょっとしたものなら reload で反映される。
```
vagrant reload
```

GitHub - Ansible  
https://github.com/ansible/ansible

Ansible - Windows ガイド
https://docs.ansible.com/ansible/2.9_ja/user_guide/windows.html#windows


pip が利用可能か確認
```
python3 -m pip -V
```

pip インストール
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
```

Ansible インストール
```
python3 -m pip install --user ansible
```

Ansible のバージョン確認
```
ansible --version
```

VagrantにはAnsibleと連携する機能が初めからある  
※Ansibleのインストールも勝手にやってくれる
```
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
```
