# インストールされるバージョンを見る

```console
% apt-cache policy nginx
nginx:
  インストールされているバージョン: (なし)
  候補:               1.1.19-1ubuntu0.6
  バージョンテーブル:
     1.1.19-1ubuntu0.6 0
        500 http://jp.archive.ubuntu.com/ubuntu/ precise-updates/universe amd64 Packages
     1.1.19-1ubuntu0.5 0
        500 http://security.ubuntu.com/ubuntu/ precise-security/universe amd64 Packages
     1.1.19-1 0
        500 http://jp.archive.ubuntu.com/ubuntu/ precise/universe amd64 Packages
```

候補(Candidate)の行に書かれたパッケージが、今apt-get installしたときにインストールされるバージョン。

# パッケージの更新履歴を見る

上記のCandidateのパッケージのchangelogを表示する。

```console
% apt-get changelog nginx
nginx (1.1.19-1ubuntu0.6) precise-proposed; urgency=low

  * Enable building of the http_stub_status_module in nginx-naxsi, which was
    apparently not marked for compiling even though it's listed in the package
    description.  (LP: #1170586)

 -- Thomas Ward <teward@ubuntu.com>  Fri, 31 Jan 2014 11:02:23 -0500

nginx (1.1.19-1ubuntu0.5) precise-security; urgency=low

  * SECURITY UPDATE: ACL bypass via space character (LP: #1253691)
    - debian/patches/cve-2013-4547.patch: modify src/http/ngx_http_parse.c
      to account for a space character, fixing an issue which could result in
      security restrictions being bypassed
    - CVE-2013-4547

 -- Thomas Ward <teward@ubuntu.com>  Thu, 21 Nov 2013 13:02:22 -0500
(省略)
```

# パッケージの詳細を見る

なお、細かく Depends とか見たければ apt-cache show が使える。

```console
% apt-cache show nginx                                                          <18:10>
Package: nginx
Priority: optional
Section: universe/web
Installed-Size: 85
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Kartik Mistry <kartik@debian.org>
Architecture: all
Version: 1.1.19-1ubuntu0.6
Depends: nginx-full | nginx-light
Filename: pool/universe/n/nginx/nginx_1.1.19-1ubuntu0.6_all.deb
Size: 5886
MD5sum: de2fd98b4d0ed9d7cf2367fb94cdb958
SHA1: d66ba022ca8e842754e5cfc718e96036bbaef8e6
SHA256: b690e0b569023141a577200612e6b2880ddc795ad7051e648bcdc1da0a790701
Description-en: small, but very powerful and efficient web server and mail proxy
 Nginx (engine x) is a web server created by Igor Sysoev and kindly provided to
 the open-source community. This server can be used as standalone HTTP server
 and as a reverse proxy server before some Apache or another big server to
 reduce load to backend servers by many concurrent HTTP-sessions.
 .
 This is a dummy package that selects nginx-full by default, but also can be
 installed with nginx-light for upgrading to nginx-light directly.
Homepage: http://nginx.net
Description-md5: c71c42a7e7b75f4b7b417a4547f32b56
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Origin: Ubuntu

Package: nginx
Priority: optional
Section: universe/web
Installed-Size: 84
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Kartik Mistry <kartik@debian.org>
Architecture: all
Version: 1.1.19-1ubuntu0.5
Depends: nginx-full | nginx-light
(省略)
```
