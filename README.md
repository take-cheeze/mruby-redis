# mruby-redis   [![Build Status](https://travis-ci.org/matsumoto-r/mruby-redis.svg?branch=master)](https://travis-ci.org/matsumoto-r/mruby-redis)
[Hiredis](https://github.com/redis/hiredis) binding by mruby. Hiredis is a minimalistic C client library for the Redis database. Redis is an open source, BSD licensed, advanced key-value store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets and sorted sets. Plese see [redis official website](http://redis.io/).

__If Redis is over performance or over capacity for you, I recommend [mruby-vedis](https://github.com/matsumoto-r/mruby-vedis).__ vedis is an embeddable datastore C library built with over 70 commands similar in concept to Redis but without the networking layer since Vedis run in the same process of the host application.
Please see [vedis pages](http://vedis.symisc.net/index.html).
## install by mrbgems
 - add conf.gem line to `build_config.rb`
```ruby
MRuby::Build.new do |conf|

    # ... (snip) ...

    conf.gem :git => 'https://github.com/matsumoto-r/mruby-redis.git'
end
```


## redis.rb

* code


```ruby
host     = "127.0.0.1"
port     = 6379
key      = "hoge"
database = 0

puts "> redis connect #{host}: #{port.to_s}"
r = Redis.new host, port

puts "> redis select: #{database}"
r.select database

puts "> redis set #{key} 200"
r.set key, "200"

puts "> redis get #{key}"
puts "#{key}: #{r[key]}"

puts "> redis exists #{key}"
puts "#{r.exists?(key)}"

puts "> redis exists fuga"
puts "#{r.exists?("fuga")}"

puts "> redis set #{key} fuga"
r[key] =  "fuga"

puts "> redis get #{key}"
puts "#{key}: #{r.get key}"

puts "> redis randomkey"
r.randomkey

puts "> redis del #{key}"
r.del key

if r[key].nil?
    puts "del success!"
end

puts "> redis incr #{key}"
puts "#{key} incr: #{r.incr(key)}"
puts "#{key} incr: #{r.incr(key)}"
puts "#{key} incr: #{r.incr(key)}"
puts "#{key} incr: #{r.incr(key)}"

puts "> redis decr #{key}"
puts "#{key} decr: #{r.decr(key)}"
puts "#{key} decr: #{r.decr(key)}"
puts "#{key} decr: #{r.decr(key)}"
puts "#{key} decr: #{r.decr(key)}"

puts "> redis decrby #{key} 100"
puts r.incrby key, 100

puts "> redis lpush logs error"
r.lpush "logs", "error1"
r.lpush "logs", "error2"
r.lpush "logs", "error3"

puts "> redis lrange 0 -1"
puts r.lrange "logs", 0, -1

puts "> redis ltrim 1 -1"
r.ltrim "logs", 1, -1

puts "> redis lrange 0 -1"
puts r.lrange "logs", 0, -1

puts "> redis del logs"
r.del "logs"

if r["logs"].nil?
    puts "del success!"
end

puts "> redis hset myhash field1 a"
r.hset "myhash", "field1", "a"

puts "> redis hset myhash field2 b"
r.hset "myhash", "field2", "b"

puts "> redis hget myhash field1"
puts r.hget "myhash", "field1"

puts "> redis hget myhash field2"
puts r.hget "myhash", "field2"

puts "> redis hdel myhash field1"
puts r.hdel "myhash", "field1"

puts "> redis del myhash"

if r["myhash"].nil?
    puts "del success!"
end

puts "> redis zadd hs 80 a"
r.zadd "hs", 80, "a"

puts "> redis zadd hs 50.1 b"
r.zadd "hs", 50.1, "b"

puts "> redis zadd hs 60 c"
r.zadd "hs", 60, "c"

puts "> redis zscore hs a"
puts r.zscore "hs", "a"

puts "> redis zrange hs 0 -1"
puts r.zrange "hs", 0, -1

puts "> redis zrank hs b"
puts r.zrank "hs", "b"

puts "> redis zrank hs c"
puts r.zrank "hs", "c"

puts "> redis zrank hs a"
puts r.zrank "hs", "a"

puts ">redis zrevrange hs 0 -1"
puts r.zrevrange "hs", 0, -1

puts ">redis zrevrank hs a"
puts r.zrevrank "hs", "a"

puts ">redis zrevrank hs c"
puts r.zrevrank "hs", "c"

puts ">redis zrevrank hs b"
puts r.zrevrank "hs", "b"

puts "> redis del hs"
r.del "hs"
if r["hs"].nil?
    puts "del success!"
end

puts "> redis publish :one hello"
r.publish "one", "hello"

r.close
```

* execute

```text
> redis connect 127.0.0.1: 6379
> redis select: 0
> redis set hoge 200
> redis get hoge
hoge: 200
> redis exists hoge
true
> redis exists fuga
false
> redis set hoge fuga
> redis get hoge
hoge: fuga
> redis randomkey
hoge
> redis del hoge
del success!
> redis incr hoge
hoge incr: 1
hoge incr: 2
hoge incr: 3
hoge incr: 4
> redis decr hoge
hoge decr: 3
hoge decr: 2
hoge decr: 1
hoge decr: 0
> redis decrby hoge 100
100
> redis lpush logs error
> redis lrange 0 -1
["error3", "error2", "error1"]
> redis ltrim 1 -1
> redis lrange 0 -1
["error2", "error1"]
> redis del logs
del success!
> redis hset myhash field1 a
> redis hset myhash field2 b
> redis hget myhash field1
a
> redis hget myhash field2
b
> redis hdel myhash field1
1
> redis del myhash
del success!
> redis zadd hs 80 a
> redis zadd hs 50.1 b
> redis zadd hs 60 c
> redis zscore hs a
80
> redis zrange hs 0 -1
["b", "c", "a"]
> redis zrank hs b
0
> redis zrank hs c
1
> redis zrank hs a
2
>redis zrevrange hs 0 -1
["a", "c", "b"]
>redis zrevrank hs a
0
>redis zrevrank hs c
1
>redis zrevrank hs b
2
> redis del hs
del success!
> redis publish :one hello
```
