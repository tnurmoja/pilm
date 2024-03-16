---
layout: post
title: "Message format"
categories: syslog
---

```
template (name="MessageFormat_ICP" type="string"
  string="<%PRI%>1 %TIMESTAMP:::date-rfc3339% %HOSTNAME% %APP-NAME% %PROCID% %MSGID% [34267 v=\"2\" svc=\"1234\" id=\"ICP\" fl=\"%SYSLOGFACILITY-TEXT%\"]%MSG:::sp-if-no-1st-sp%%MSG%\n"
)
```

Save location:
```
template(name="t_file_path_gz"
         type="string"
         string="/sb/logs/svc-%$!v_svc%/os_%$!v_id%/ret_%$!v_ret%/%$year%/%$month%/%$day%/%fromhost-ip%/r_%$myhostname%/%$!v_fl:::lowercase%.gz")
```

