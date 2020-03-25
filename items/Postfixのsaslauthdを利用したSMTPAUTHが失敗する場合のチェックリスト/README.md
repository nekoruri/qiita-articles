※ Ubuntu 12.04 LTSの場合

* master.cfを確認してsubmissionなどがchrootになっている場合は、/etc/default/saslauthd のコメントに書かれているように、「OPTIONS="-c -m /var/spool/postfix/var/run/saslauthd"」とする。
* /var/run/saslauthdにアクセスできるよう、sasl groupにpostfixユーザを加える。

