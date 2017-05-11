---
layout: post
title: Getting Started with Redis
date: 2017-5-10 00:18:23
categories: ruby
short_description: This post will cover how I got started with Redis
image_preview: https://upload.wikimedia.org/wikipedia/en/thumb/6/6b/Redis_Logo.svg/467px-Redis_Logo.svg.png
---
For a Node.js application that needed a mail queueing service, I planned to use
Redis and specifically the [kue](https://github.com/Automattic/kue) library.
This covers how I got about to setting up Redis on my ArchLinux machine.

# Initial Setup
First install `redis` package using the following command for an ArchLinux
machine:
```
sudo pacman -S redis
```
Ubuntu users may have `apt-get install` and similar for other GNU/Linux users.
Windows has an installer executable that you can download from their website.

Next, copy the default conf file located at `/etc/redis.conf` to
`/etc/redis.conf.default`
```
cp /etc/redis.conf /etc/redis.conf.default
```

Start the Redis server using:
```
sudo systemctl start redis.service
```
You can also enable the server to start on startup by using the command 
`sudo systemctl enable redis.service`. Now that Redis was setup, we just needed
to start playing with it.

# Playing with Redis
Enter the following to connect to the Redis server.
```
redis-client
```

You can quit the
application using `QUIT` command. Redis is like a key-value pair, where the
value instead of being just string can also be a data structure. You can read
more about them [here](https://redis.io/topics/data-types-intro).

* On getting the client prompt, try using the `GET` and `SET` commands. For,
instance `SET foo 99` will set key `foo` to `99`. Similarly, `GET foo` will
return `99`. 
* You can also check whether a key-value pair exists by the `EXISTS`
command. For instance `EXISTS foo` after running `SET foo 99` will return 1
(true).
* You can also delete `foo` using `DEL foo`.
* You can also set expirations by `EXPIRE foo 20` for a 20 second expiration.
Using `TTL foo`, we can find the time left for foo to expire.

You can explore the Redis client more from 
[here](https://redis.io/topics/rediscli).

# Persistence
For my Node.js application, I needed persistence, so I explored that. There are
essentially two kinds of persistences that you can use(you may also use them in
combo) in Redis:
* RDB: This performs a point-in-time snapshot of your dataset at specified
  intervals
* AOF: This logs after each write operation received by the server

As you may have figured out, both have their advantages and disadvantages that
you can read more about [here](https://redis.io/topics/persistence). A common
strategy is to use a mix of both. 
