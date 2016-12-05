---
layout: post
title:  Bitcoin mining
date:   2016-12-02 22:02:07 +0100
comments: true
categories: Linux Bitcoin
---
### What is Bitcoin mining? ###

<http://www.bitcoinmining.com>

Basically, Bitcoin mining is giving computing power to confirm transactions.

### Looking for Bitcoin mining on a server? ###

I'm mining with CPU power, nog GPU or ASICS, using cpuminer: <http://sourceforge.net/projects/cpuminer/files/>. Download the latest version for Linux, 32 or 64 bits. Unpack it and you are ready to go on software level.

Best is to do Pooled Mining, to mine together with other people and get much faster a (Bitcoin) reward. I'm using <https://mining.bitcoin.cz>. Register an account at <https://mining.bitcoin.cz/accounts/register/> and you can start.

In the command line, where you have the mining software, run:

{% highlight bash %}
fred@pepper:~/cpuminer$ ./minerd --url http://api.bitcoin.cz:8332
 --userpass CoolUsername.worker1:SomePassword --algo sha256d --threads=2
[2014-07-09 20:35:22] Binding thread 0 to cpu 0
[2014-07-09 20:35:22] 2 miner threads started, using 'sha256d' algorithm.
[2014-07-09 20:35:22] Binding thread 1 to cpu 1
[2014-07-09 20:35:22] Starting Stratum on stratum+tcp://stratum.bitcoin.cz:3333
[2014-07-09 20:35:22] JSON-RPC call failed: {
   "message": "Unsupported method 'getblocktemplate'",
   "code": -1
}
[2014-07-09 20:35:23] Stratum requested work restart
[2014-07-09 20:35:25] thread 0: 2097152 hashes, 1249 khash/s
[2014-07-09 20:35:25] thread 1: 2097152 hashes, 1201 khash/s
[2014-07-09 20:35:50] Stratum requested work restart
[2014-07-09 20:35:50] thread 1: 32976172 hashes, 1302 khash/s
[2014-07-09 20:35:50] thread 0: 32869400 hashes, 1295 khash/s
[2014-07-09 20:36:19] Stratum requested work restart
[2014-07-09 20:36:19] thread 1: 37260644 hashes, 1314 khash/s
[2014-07-09 20:36:19] thread 0: 36532456 hashes, 1288 khash/s
[2014-07-09 20:36:51] Stratum requested work restart
[2014-07-09 20:36:51] thread 1: 42340552 hashes, 1316 khash/s
[2014-07-09 20:36:51] thread 0: 41552044 hashes, 1292 khash/s
[2014-07-09 20:37:53] thread 0: 77505964 hashes, 1238 khash/s
[2014-07-09 20:37:54] thread 1: 78976356 hashes, 1254 khash/s
[2014-07-09 20:38:51] thread 0: 74282232 hashes, 1295 khash/s
[2014-07-09 20:38:52] thread 1: 75240412 hashes, 1301 khash/s
[2014-07-09 20:38:55] Stratum requested work restart
[2014-07-09 20:38:55] thread 0: 5323320 hashes, 1309 khash/s
[2014-07-09 20:38:55] thread 1: 4108764 hashes, 1286 khash/s
[2014-07-09 20:39:55] thread 1: 77130484 hashes, 1283 khash/s
[2014-07-09 20:39:55] thread 0: 78565936 hashes, 1302 khash/s
[2014-07-09 20:40:54] thread 1: 76985272 hashes, 1305 khash/s
[...]
{% endhighlight %}

Of course CoolUsername.worker1:SomePassword must fit your username and password. Threads must be the number of Cores you want to use for cpuminer. That's it!

Best is to run cpuminer inside screen. That way it keeps running when you leave the server :]
