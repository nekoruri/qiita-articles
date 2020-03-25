Ruby環境にはbundlerが入った状態で：

* git init
* bundle init
* vim Gemfile

```Gemfile
    source 'https://rubygems.org'
    gem 'rails', '4.0.0.beta1'
```

* bundle install --path vendor/bundle
* bundle exec rails new . -T 
 * -Tはrspecつかうため
 * APP_PATHに . を指定すると、今居るディレクトリの名前がアプリケーションのクラス名に使われるので注意
   * 例: rails4 => Rails4::Application
 * Gemfileの上書きを聞かれるのでy
* .gitignoreにvendor/bundleを追加

```.gitignore
    /.bundle
    /vendor/bundle
    
    /db/*.sqlite3
    /db/*.sqlite3-journal
    
    /log/*.log
    /tmp
```

* git add .
* git commit -m 'new project'
