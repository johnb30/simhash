simhash
===========

**This is a fork of
[https://github.com/leonsim/simhash](https://github.com/leonsim/simhash). The
major difference is use of redis as a backend for the `SimhashIndex`
functionality.**

This is a Python implementation of [Simhash](http://www.wwwconference.org/www2007/papers/paper215.pdf).

## Getting Started

<https://leons.im/posts/a-python-implementation-of-simhash-algorithm/>

###Redis and SimhashIndex

The only difference between usage of the previous implementation of `SimhashIndex` and
the current implementation is the dependency on redis. Thus, an active redis
connection must be passed to `SimhashIndex` as shown below (assumes a running
redis server on `localhost`):

```
import re
import redis

from simhash import Simhash, SimhashIndex

def get_features(s):
    width = 3
    s = s.lower()
    s = re.sub(r'[^\w]+', '', s)
    return [s[i:i + width] for i in range(max(len(s) - width + 1, 1))]

data = {
    1: u'How are you? I Am fine. blar blar blar blar blar Thanks.',
    2: u'How are you i am fine. blar blar blar blar blar than',
    3: u'This is simhash test.',
}

conn = redis.StrictRedis(host=host, port=6379, db=0)
objs = [(str(k), Simhash(get_features(v))) for k, v in data.items()]

index = SimhashIndex(conn, objs, k=3)

```

## Build Status

[![Build Status](https://travis-ci.org/leonsim/simhash.png?branch=master)](https://travis-ci.org/leonsim/simhash)
