メールサーバのリプレースで旧サーバに来たTCP接続を新サーバに転送したい場合など、他サーバへのTCP接続の転送が必要な場合、xinetdだけで実現することができます。

なお、最近のディストリビューションでは標準ではxinetdが無効化されている場合があるため、xinetd パッケージを導入し、chkconfig や update-rc.d などであらかじめ有効化してください。

例えば、imaps(TCP ポート番号993)を、別のサーバ<yy.yy.yy.yy>のポート番号993に転送するのであれば、以下のファイルを /etc/xinetd.d/imaps として作成します。

> service imaps
> {
>         disable                 = no
>         socket_type             = stream
>         wait                    = no
>         user                    = nobody
>         redirect                = yy.yy.yy.yy imaps
> }

ポート番号は port = 10993 のように変更することができます。

TCP接続を受けるたびに、userで指定したユーザ(上記ではnobody)で xinetd のプロセスが増えるので、たくさんの接続が来ることが想定される場合は、秒間接続数を制限するcpsなどを指定するか、別の手法を用いるべきです。