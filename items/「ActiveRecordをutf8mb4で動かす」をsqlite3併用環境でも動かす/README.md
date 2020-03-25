[ActiveRecordをutf8mb4で動かす ](http://qiita.com/items/101aaf8159cf1470d823)
のinitializerが、開発環境とかでsqlite3みたいにMySQL以外のDBエンジンを併用している場合に上書き先のメソッドが未定義で死ぬので、NameError だけとっ捕まえるようにした。

ちなみに、SQLite3以外では未確認。問題無いとは思うけど。

```ruby:config/initializers/ar_innodb_row_format.rb
ActiveSupport.on_load :active_record do
  begin
    module ActiveRecord::ConnectionAdapters

      class AbstractMysqlAdapter < AbstractAdapter
        def create_table_with_innodb_row_format(table_name, options = {})
          table_options = options.reverse_merge(:options => 'ENGINE=InnoDB ROW_FORMAT=DYNAMIC')
          create_table_without_innodb_row_format(table_name, table_options) do |td|
            yield td if block_given?
          end
        end
        alias_method_chain :create_table, :innodb_row_format
      end

    end
  rescue NameError => ex
    # MySQLが使われていないので無視
  end
end

```
