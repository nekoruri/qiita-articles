Jenkinsや本番環境などで、ログに含まれるエスケープシーケンスが不要な場合には、以下のように設定することで色分けを止めることができる。

特定の環境のみ止めるのであれば、 config/environments/(環境名).rb に以下のように設定する。
> ExampleApp::Application.configure do
>   # その他の設定
> 
>   config.colorize_logging = false
> end

もしくは全ての環境のデフォルトを変えるには、config/application.rb で設定する。
> module ExampleApp
>   class Application < Rails::Application
>     # その他の設定
> 
>     config.colorize_logging = false
>   end
> end

これを組み合わせることで、例えば、config/environments/development.rb で config.colorize_logging = true と設定することで、開発環境でのみ色分けを有効にすることができる。

なお、Rails3から設定方法が変更になった。
[Ruby on Rails 3.0 Release Notes](http://guides.rubyonrails.org/3_0_release_notes.html)
> ActiveRecord::Base.colorize_logging and config.active_record.colorize_logging are deprecated in favor of Rails::LogSubscriber.colorize_logging or config.colorize_logging