
# Zabbix Server初期セットアップ用Playbook

## ロール一覧

|  ロール名  |  処理概要  |  備考  |
| ---- | ---- | ---- |
|  create_swap_space  |  swapファイルの設定を行う  | 物理メモリの2倍でswapを設定 |
|  install_zabbix_server  |  Zabbix Serverのインストールを行う  |  |
|  set_locale  |  Localeの設定を行う  |  Zabbixの日本語化に必要  |

## コンテナを使用したPlaybookの実行方法

0. 事前準備

    - DockerまたはPodmanを導入し、以下のPythonパッケージをインストールする

        - ansible-navigator
        - ansible-builder

    - Managed Nodeへの接続ユーザー用秘密鍵の配置

      マネージドノード接続用のユーザーの秘密鍵を`.ssh`ディレクトリに`ansible.pem`という名前で保存\
      秘密鍵のパーミッションは`600`とする

    - インベントリファイルの修正

      `inventories/inventory.yml`を開き、`ansible_host`にマネージドノードのIPを記載する\

    - 暗号化を行う変数の対応
      `inventories/group_vars/all.yml`のzabbix_user_passwordとroot_user_passwordにvaultで暗号化した値を入力する\
      ※以下のコマンドを実行して、暗号化を行う\
      `ansible-vault encrypt_string password --name 'zabbix_user_password'`

      また、`.vault_pass.sample`を`.vault_pass`としてコピーし、vaultパスワードを記載する。

1. コンテナイメージの作成

    以下のコマンドを実行し、コンテナイメージを作成する\
    `cd ./execution_environment`\
    `ansible-builder build --tag=costom-ee --file ./execution-environment.yml`

2. Playbook実行

   `ansible-navigator run <Playbookファイル> -i <inventory fileのパス> --ee true --eei custom-ee --pull-policy never -m stdout`

   ※`ansible-navigator.yml`でAnsible Navigatorの設定をしているため、以下のように省略可能\
   `ansible-navigator run <playbookファイル> -i inventories/inventory.yml`

## Moleculeを使用したテスト

各ロールに`molecule`ディレクトリを配置し、以下のコマンドでテストを実施する\
`molecule test`
