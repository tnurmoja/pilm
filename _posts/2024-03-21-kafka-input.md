---
layout: post
title: "Message format"
categories: syslog
---

```
module (load="imkafka" )
```

```
input(  type="imkafka" 
        topic="{{ event_hub_sa_topic }}" 
        broker="{{ event_hub_sa_broker }}"
        consumergroup="{{ event_hub_sa_consumergroup }}"
        confparam=[     "security.protocol=SASL_SSL",
                        "sasl.mechanism=PLAIN",
                        "sasl.username=$ConnectionString",
                        "sasl.password={{ event_hub_sa_authorization_connection_string }}",
                        "session.timeout.ms=10000",
                        "socket.timeout.ms=5000",
                        "socket.keepalive.enable=true",
                        "enable.partition.eof=false" ]
        ruleset="kafka_process")
```

```
ruleset(name="kafka_process"
        queue.filename="input_queue_kafka"
        queue.type="FixedArray"
        queue.saveonshutdown="on"
        queue.workerthreads="4"
        queue.size="380000"
        queue.dequeueBatchSize="4096"
        queue.workerThreadMinimumMessages="65000"
        queue.maxfilesize="1g"
        queue.maxdiskspace="20g"
        queue.timeoutenqueue="0"
        ) {
            set $!v_fl="audit";
            set $!v_svc="W742";
            set $!v_id="ARCHIVE";
            set $!v_appname=$app-name;
            set $!v_msg=$msg;

            # Setting v_svc and v_id values to uppercase for concistancy
            set $!v_svc=exec_template("t_svc_uppercase");
            set $!v_id=exec_template("t_id_uppercase");
            set $!v_ret = "Y01";
            
            call default_log_storage
            call forward_event_hub
}
```

