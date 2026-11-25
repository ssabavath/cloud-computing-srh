# Session 5: Kafka on GKE

## Topic while broker 1 was down

```
Topic: ocean-events  PartitionCount: 3  ReplicationFactor: 3  Configs: min.insync.replicas=2,retention.ms=2592000000
  Partition: 0  Leader: 0  Replicas: 0,1,2  Isr: 0,2
  Partition: 1  Leader: 2  Replicas: 1,2,0  Isr: 2,0
  Partition: 2  Leader: 2  Replicas: 2,0,1  Isr: 2,0
```

## ops-dashboard before the reset

```
GROUP          TOPIC         PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG
ops-dashboard  ocean-events  0          0               163             163
ops-dashboard  ocean-events  1          0               194             194
ops-dashboard  ocean-events  2          50              71              21
```

## ops-dashboard after the reset (--to-earliest)

```
GROUP          TOPIC         PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG
ops-dashboard  ocean-events  0          0               436             436
ops-dashboard  ocean-events  1          0               394             394
ops-dashboard  ocean-events  2          0               355             355
```

## Which partition did my shipment land on, and why?

I picked shipment LIVE-SHP05-000038. All 7 of its events ended up on partition 2, and none showed up anywhere else. Kafka uses the shipment ID as the key and hashes it to choose a partition, so the same shipment always goes to the same partition. That keeps its events in order (BOOKED, then GATE_IN, and so on), while events from different shipments can be spread across partitions with no order between them.

## What happened when the broker died, and why didn't the producer fail?

When I deleted freight-dual-1, broker 1 dropped out of the in-sync replicas (the Isr changed from 0,1,2 to 0,2 or 2,0), and partition 1 got a new leader, broker 2, because broker 1 had been leading it. The producer didn't fail because every partition still had two in-sync copies, which is enough for min.insync.replicas=2, so writes were accepted the whole time (it kept counting up from 680 to 760 events). A few seconds later the broker came back on its old disk and rejoined the Isr, and I didn't have to restart anything myself.

