---
layout: post
title: "Metrics Collection"
categories: metrics
---

We copy once per day logs from rsyslog nodes to long term archive. As we iterate over all files it makes sense to collect metrics about our logs and logfiles. 

```python
def gzipfile_info(filename):

    lineno = 0
    filesize=0
    try:
        with gzip.open(filename, 'rb') as file_obj:
            for line_bin in file_obj:
                lineno=lineno+1
            filesize=file_obj.seek(0, io.SEEK_END)
    except Exception as ex:
        logging.error(f'Exception: { ex }. File: { filename }')
        pass
    return { "filesize": filesize, "lineno": lineno }
```
