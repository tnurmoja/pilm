---
layout: post
title: "Forward events to Azure event hub"
categories: syslog
---

```
module(load="omazureeventhubs")
```
```
template(name="t_event_hub" type="list" option.jsonf="on" option.casesensitive="on") {
        property(outname="TimeReceived" name="timereported" dateFormat="rfc3339" format="jsonf")
        property(outname="SyslogServer" name="$myhostname" format="jsonf")
        property(outname="TimeGenerated" name="timegenerated" dateFormat="rfc3339" format="jsonf")
        property(outname="Severity" name="syslogseverity" caseConversion="upper" format="jsonf" datatype="number")
        property(outname="Facility" name="syslogfacility" format="jsonf" datatype="number")
        property(outname="Hostname" name="hostname" format="jsonf")
        property(outname="IPAddress" name="fromhost-ip" format="jsonf")
        property(outname="ProcessName" name="$!v_appname" format="jsonf")
        property(outname="ProcessId" name="$!v_procid" format="jsonf")
        constant(value="\"LMIdentifier\": \"LM@12345\"")
        constant(value="\"LMVer\": 2")
        property(outname="Service" name="$!v_svc" format="jsonf")
        property(outname="Identifier" name="$!v_id" format="jsonf")
        property(outname="Filename" name="$!v_fl" format="jsonf")
        property(outname="Retention" name="$!v_ret" format="jsonf")
        property(outname="Message" name="$!v_msg" format="jsonf" )
}
```

```
ruleset(name="forward_event_hub"
        queue.filename="q_forward_event_hub"
        queue.type="FixedArray"
        queue.saveonshutdown="on"
        queue.size="380000"
        queue.dequeueBatchSize="2048"
        queue.workerThreads="4"
        queue.workerThreadMinimumMessages="65000"
        queue.maxfilesize="1g"
        queue.maxdiskspace="100g"
        queue.timeoutenqueue="0")
        {
            {% if adx_event_hub_authorization_name is defined %}

            action(type="omazureeventhubs"
                name="a_forward_event_hub_omazureeventhubs"
                azurehost="{{ adx_event_hub_host }}"
                azureport="5671"
                azure_key_name="{{ adx_event_hub_authorization_name }}"
                azure_key="{{ adx_event_hub_authorization_key }}"
                container="ryslog-eventhub"
                template="t_event_hub"
                queue.type="linkedList"
                queue.size="200000"
                queue.saveonshutdown="on"
                queue.dequeueBatchSize="2000"
                queue.minDequeueBatchSize.timeout="1000"
                queue.workerThreads="8"
                queue.workerThreadMinimumMessages="20000"
                queue.timeoutWorkerthreadShutdown="10000"
                queue.timeoutshutdown="1000"
            )
            {% endif %}

            action(type="omfile"
                name="a_forward_event_hub_omfile_json"
                dirCreateMode="0750"
                fileCreateMode="0640"
                template="t_event_hub"
                file="/sbp/logs/local/jsonout.log")
}
```
```
resource "azurerm_storage_blob" "data_explorer_script" {
   name                   = "script.txt"
   storage_account_name   = azurerm_storage_account.data_explorer_storage_account.name
   storage_container_name = azurerm_storage_container.data_explorer_container.name
   type                   = "Block"
   source_content         = ".create table Logs ( TimeReceived: datetime, SyslogServer: string, TimeGenerated:datetime, Severity: int32, Facility: int32, Hostname: string, IPAddress: string, ProcessName: string, ProcessId:int32, LMIdentifier: string, LMVer: int32, Service: string, Identifier: string, Filename: string, Retention: string, Message:string)"
}
```
