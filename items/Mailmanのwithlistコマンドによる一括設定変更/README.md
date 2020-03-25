組織の運用ポリシーの変更などによりメーリングリストのアーカイブを削除することになった場合など、複数のメーリングリストの設定変更が必要になることがありますが、Webの管理画面からいちいち各メーリングリストに対して行うのは困難です。

このような場合に、Mailmanではwithlistコマンドが利用できます。

まずは1つのメーリングリストを対象にして、設定変更ができることを確認します。バイナリ形式で保存されている現在の設定を、dumpdbコマンドで確認します。

```
# cd /var/lib/mailman
# bin/dumpdb -p lists/mlname/config.pck | grep archive
    'archive': True,
    'archive_private': 0,
    'archive_volume_frequency': 1,
```

現在mlnameというメーリングリストではアーカイブが有効になっています。これを無効に変更します。

準備として、設定変更の処理をPythonスクリプトで用意します。引数としてメーリングリストのオブジェクトが渡されるので、Lock()とSave()の間で設定変更を行います。

```python:disable_archive.py
def disable_archive(mlist):
    mlist.Lock()
    mlist.archive = False
    mlist.Save()
```

withlistコマンドでPythonスクリプトに書かれた処理を適用します。

```
# withlist -r disable_archive mlname
Importing disable_archive...
Running disable_archive.disable_archive()...
Loading list mlname (unlocked)
Unlocking (but not saving) list: mlname
Finalizing
```

これで設定変更ができました。念のため確認してみます。

```
# bin/dumpdb -p lists/mlname/config.pck | grep archive
    'archive': False,
    'archive_private': 0,
    'archive_volume_frequency': 1,
```

設定変更ができることが確認できたら、今度は全てのメーリングリストに対して適用するため、メーリングリスト名の代わりに「-a」オプションを利用します。

```
# withlist -r disable_archive -a
Importing disable_archive...
Running disable_archive.disable_archive()...
Loading list ml1 (unlocked)
Loading list ml2 (unlocked)
Loading list ml3 (unlocked)
(省略)
Loading list ml99 (unlocked)
```

これで全てのメーリングリストで設定変更が行われました。念のため、いくつか適当にサンプリングして設定が変更されていることを確認します。

```
# bin/dumpdb -p lists/ml1/config.pck | grep archive
    'archive': False,
    'archive_private': 0,
    'archive_volume_frequency': 1,
```

大丈夫のようです。

Mailmanは通常はWebの管理画面から操作することが多いですが、コマンドラインでの操作ができると一気に運用が楽になります。