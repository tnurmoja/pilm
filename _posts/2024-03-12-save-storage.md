---
layout: post
title: "Compress logs on-the-fly"
categories: log management
---

## How to save storage packing logs on-the-fly.

Rsyslog has functionality to save the logs to file in compressed format either gzip format and newer rsyslog versions support also zlib format.

https://www.rsyslog.com/doc/configuration/modules/omfile.html
```
module(load="builtin:omfile"
       dirCreateMode="0750"
       fileCreateMode="0640"
       compression.driver="zlib")
```
You can use only gzip or zlib not both on the same time
```
action(type="omfile"
       name="a_default_log_storage"
       DynaFile="t_file_path"
       template="t_file_record_RAW"
       ioBufferSize="128k"
       flushOnTXEnd="off"
       asyncWriting="off"
       veryRobustZip="on"
       DynaFileCacheSize="70000"
       zipLevel="6")
```
Under heavy load there can be needed also option veryRobustZip="on". The explaination from rsyslog doc:

> In order to avoid this degradation in compression both flushOnTXEnd and asyncWriting parameters must be set to “off” and also ioBufferSize must be raised from default “4k” value to at least “32k”. This way a reasonable compression factor is maintained, similar to a non-blocked gzip file:
> 
> ```veryRobustZip="on" ioBufferSize="64k" flushOnTXEnd="off" asyncWriting="off"```

Here is a real world example: 775GB compressed logs vs 12.78TB uncompressed. All logfiles are compressed upon arrival. The uncompressed size is calculated during the log archiving process:

![Archive_stats](https://raw.githubusercontent.com/tnurmoja/pilm/main/_posts/pictures/compress-stats.PNG)
