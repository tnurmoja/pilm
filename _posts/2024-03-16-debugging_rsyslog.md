---
layout: post
title: "Debugging rsyslog"
categories: syslog
---
```

global( 
        debug.logfile="/org/logs/local/debug.log"
        debug.whitelist="on"
        debug.files=["imkafka.c" ]
        abortOnUncleanConfig="off"
)
```
