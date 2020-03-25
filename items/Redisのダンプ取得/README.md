多少古いデータでも構わなければ、redis.confで指定した頻度で dump.rdb が更新されているが、RedisのBGSAVEコマンドを実行し、LASTSAVEが更新されるのを待つ。 これでdump.rdbが最新の状態となる。

* https://gist.github.com/nekoruri/6238957

```bash:redis_bgsave.sh
#!/bin/bash
 
# RedisのBGSAVEコマンドを実行し、redis.rdbが更新されるのを待つ。
# タイムアウト時間を超えたら exit status 1 で終了する。
 
TIMEOUT=30
 
# Timeout handler
# http://stackoverflow.com/questions/1226094/how-to-include-a-timer-in-bash-scripting
set_timer() {
        trap handle_timer ALRM
        (sleep $TIMEOUT; kill -ALRM $$)&
        timer_pid=$!
}
 
unset_timer() {
        kill $timer_pid
        trap - ALRM
}
 
handle_timer() {
        exit 1
}
 
 
export PATH=/bin:/usr/bin
 
set_timer
 
# 丁度この瞬間に組込のBGSAVEが走る可能性があるので、
# BGSAVEの前にLASTSAVEを取得
LASTSAVE=`redis-cli LASTSAVE`
 
redis-cli BGSAVE
 
# LASTSAVEが変わるまで待つ。
while [ $LASTSAVE == `redis-cli LASTSAVE` ]; do
        sleep 1
done
 
unset_timer
```

あとは /var/lib/redis/dump.rdb (もしくはその他の場所) を好きにすれば良い。

