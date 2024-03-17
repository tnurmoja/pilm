---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: home
---
## Introduction

Selle leheküleje eesmärk on jagada teadmisi ja kogemusi logihalduse korraldamise kohta. 

The process of [log management](https://en.wikipedia.org/wiki/Log_management) generally breaks down into:

* Log collection - a process of capturing actual data from log files, application standard output stream (stdout), network socket and other sources.
* Logs aggregation (centralization) - a process of putting all the log data together in a single place for the sake of further analysis or/and retention.
* Log storage and retention - a process of handling large volumes of log data according to corporate or regulatory policies (compliance).
* Log analysis - a process that helps operations and security team to handle system performance issues and security incidents

## Log format.

Kui logi on kogutud, siis:

* kuidas juurdepääsuõigused võimalikul lihtsalt paika panna;
* kuidas töödelda ja salvestada võimalikult lihtsalt;
* kuidas edastada log analysis vahendatiele.
* kuidas majandada logi elueaga (retention time) 

### Metadata

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


Here we will have big picture.
![Big picture](https://raw.githubusercontent.com/tnurmoja/pilm/main/diagrams/lab.drawio-high.png)

 * separate log collection/archiving from analysis

## Log collection

Pick up messages:
 * socket
 * syslog
 * file
 * kafka

## Log Aggregation

Lookup tables

Rulesets

Templates
 * filenames 

## Log storage and retention

## Log analysis

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
