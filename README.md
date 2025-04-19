
# Zabbix Server初期セットアップ用Playbook

## ロール一覧

|  ロール名  |  処理概要  |  備考  |
| ---- | ---- | ---- |
|  create_swap_space  |  swapファイルの設定を行う  | 物理メモリの2倍でswapを設定 |
|  install_zabbix_server  |  Zabbix Serverのインストールを行う  |  |
|  set_locale  |  Localeの設定を行う  |  Zabbixの日本語化に必要  |

実行環境に必要なパッケージ
    - PyMySQL
    - MySQLの実行
