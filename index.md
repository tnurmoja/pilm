---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---

Here we will have big picture.
![Big picture](https://raw.githubusercontent.com/tnurmoja/pilm/main/diagrams/lab.drawio-high.png)

 * separate log collection/archiving from analysis
 * add metadata to log record:
   * to provide access control
   * to change analysis destination  

Message format.
Metadata header:
 * service
 * subidentifier
 * filename
 * retention
Metadata header shall be as small as possible. It will be added to each message.

Pick up messages:
 * socket
 * syslog
 * file
 * kafka

Lookup tables

Rulesets

Templates
 * filenames 

Forward messages:
 * UDP
 * TCP
 * RELP
 * AMPQ

Metrics
 * rsyslog pstats 
Archive
 * location  templates
 * packing on-the-fly
Analytics
 * Sentinel
