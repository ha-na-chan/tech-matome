Ubuntu における netplan 設定メモ
######################################

はじめに
***********

Ubuntu の基本的なネットワーク設定の個人的なメモです。

ここでは、netplan の設定ファイルを書くことで、ubuntu の IP アドレス設定をします。

設定ファイルと内容
********************

以下のディレクトリに、新しいファイル（ここでは ``99-static.yaml`` としました。）を作成します。

::

	/etc/netplan/99-static.yaml

前後しますが、下記の例のように記入したら、次のコマンドを実行して反映してください。

::

	sudo netplan apply

DHCP を利用する設定例
=========================

作成した新しいファイルに、下記のような設定を記述します。値は適宜書き換えてください。

::

	network:
  	version: 2
 	 renderer: networkd
	  ethernets:
	    enp3s0:			# interface 名
	      dhcp4: true

IPアドレスを指定する設定例
==============================

アドレスを指定するときは、下記のような設定を記述します。値は適宜書き換えてください。

::

	network:
	  version: 2
	  renderer: networkd
	  ethernets:
	    enp3s0:			# interface 名
	      addresses:
		- 10.10.10.2/24
	      gateway4: 10.10.10.1	# DGW
	      nameservers:
		  search: [mydomain, otherdomain]
		  addresses: [10.10.10.1, 1.1.1.1]

netplan の設定について
==================================

netplan はIPアドレスを設定する際に、 ``/{lib,etc,run}/netplan`` のディレクトリの下にあって、 ``.yaml`` で終わるファイルを取得します。ファイルが複数あるときは、辞書配列順に読み込まれ、同じ（mapping keys の）ものに対しての設定は、あとに読み込まれたものに書き換えられます。

詳しくは、公式のドキュメントが詳しいので参照されたいです。

参照
======

https://netplan.io/reference
