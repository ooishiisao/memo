# virsh
```
list      ドメインの一覧表示
console   ゲストのコンソールへの接続
start     (以前に定義された) 非アクティブなドメインを開始します
shutdown  ドメインを正常にシャットダウンします
destroy   ドメインの強制停止
reboot    ドメインを再起動します
reset     ドメインのリセット
undefine  ドメインの削除
dumpxml   XML のドメイン情報
```

# virt-install
## OK
```
virt-install \
 --name test1\
 --memory 1024 \
 --vcpus 1 \
 --os-variant centos-stream9 \
 --network bridge=br0 \
 --graphics none \
 --location /home/vm/iso/CentOS-Stream-9-latest-x86_64-dvd1.iso \
 --extra-args 'console=ttyS0'
```

- `name`
    仮想マシンの名前
- `memory`
    メモリ（単位：MB）
- `vcpus`
    CPU数
- `os-variant`
    OS。以下で指定できるOSのリストが出る。
    `virt-install --osinfo list`
- `network`
    接続する仮想スイッチ
- `graphics`
    画面表示を指定（none, spice, vncなど）
- `location`
    インストール元
    isoファイルへのパスを指定しておくのが無難。
    isoファイル展開済みのパスを指定するとなぜか失敗する。CentOS9
- `cdrom`
    インストールメディアのドライブを指定する場合。（デバイスファイル）
- `extra-args`
    カーネルコマンドラインパラメータ
    [The kernel’s command-line parameters — The Linux Kernel  documentation](https://docs.kernel.org/admin-guide/kernel-parameters.html)
    画面なしの場合は、コンソールの入出力先をカーネルに教えてあげる必要がある。
    > `console=ttyS0` 
    `console=` 出力コンソールデバイスとオプション			
    `tty<n>` 仮想コンソールデバイス <n> を使います。	
    `ttyS<n>[,options]` 指定したシリアルポートを使います。オプションは、書式 "bbbbpn" で、ここでの "bbbb" はボーレートで、p はパリティ ("n", "o", "e")、"n" はそのビットです。初期値は、"9600n8" です。

