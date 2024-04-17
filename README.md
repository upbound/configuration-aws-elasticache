# configuration-aws-elasticache
A configuration for AWS Elasticache cluster and related resources.
This configuration builds on top of the `configuration-aws-network`
reference package. It creates a parameter group, and an elasticache
cluster.

Once resouces have been created, `crossplane beta trace` shows the
following output for the resource claim. The nested composition consists
of the `XNetwork` foundation, the `XParameterGroup` and the `XCluster` to
build the composite Cache resource.
```
NAME                                                      SYNCED   READY   STATUS
Cache/cache (default)                                     True     True    Available
└─ XCache/cache-qsfmj                                     True     True    Available
   ├─ XCluster/cache-qsfmj-l9qmj                          True     True    Available
   │  └─ Cluster/cache-qsfmj-d4qdn                        True     True    Available
   ├─ XNetwork/cache-qsfmj-h5msh                          True     True    Available
   │  ├─ XNetwork/cache-qsfmj-5vpj2                       True     True    Available
   │  │  ├─ InternetGateway/cache-qsfmj-l5jmz             True     True    Available
   │  │  ├─ MainRouteTableAssociation/cache-qsfmj-4b7kw   True     True    Available
   │  │  ├─ RouteTableAssociation/cache-qsfmj-8kdsd       True     True    Available
   │  │  ├─ RouteTableAssociation/cache-qsfmj-c4k52       True     True    Available
   │  │  ├─ RouteTableAssociation/cache-qsfmj-lhz46       True     True    Available
   │  │  ├─ RouteTableAssociation/cache-qsfmj-nf2jk       True     True    Available
   │  │  ├─ RouteTable/cache-qsfmj-bp42x                  True     True    Available
   │  │  ├─ Route/cache-qsfmj-vr6dm                       True     True    Available
   │  │  ├─ SecurityGroupRule/cache-qsfmj-h6q8h           True     True    Available
   │  │  ├─ SecurityGroupRule/cache-qsfmj-hchsv           True     True    Available
   │  │  ├─ SecurityGroupRule/cache-qsfmj-jggc6           True     True    Available
   │  │  ├─ SecurityGroup/cache-qsfmj-kpnd5               True     True    Available
   │  │  ├─ Subnet/cache-qsfmj-cctjf                      True     True    Available
   │  │  ├─ Subnet/cache-qsfmj-lkzt4                      True     True    Available
   │  │  ├─ Subnet/cache-qsfmj-nq5z5                      True     True    Available
   │  │  ├─ Subnet/cache-qsfmj-x5btl                      True     True    Available
   │  │  └─ VPC/cache-qsfmj-bkssx                         True     True    Available
   │  └─ SubnetGroup/cache-qsfmj-5hx56                    True     True    Available
   └─ XParameterGroup/cache-qsfmj-pbxtx                   True     True    Available
      └─ ParameterGroup/cache-qsfmj-84zts                 True     True    Available
```

The cluster endpoint is available at the Redis cluster status information.
Note that for this one node example cluster the endpoint is at the
cache mode. In this case it
is `cache-qsfmj-d4qdn.49syq0.0001.usw2.cache.amazonaws.com`.
Your cluster node(s) will be accessible at their respective address(es).
```
status:
  conditions:
  - lastTransitionTime: "2024-04-13T03:27:32Z"
    reason: ReconcileSuccess
    status: "True"
    type: Synced
  - lastTransitionTime: "2024-04-13T03:42:14Z"
    reason: Available
    status: "True"
    type: Ready
  connectionDetails:
    lastPublishedTime: "2024-04-13T03:27:32Z"
  elasticacheCluster:
    applyImmediately: true
    arn: ... MASKED ARN ...
    autoMinorVersionUpgrade: "true"
    availabilityZone: us-west-2b
    azMode: single-az
    cacheNodes:
    - address: cache-qsfmj-d4qdn.49syq0.0001.usw2.cache.amazonaws.com
      availabilityZone: us-west-2b
      id: "0001"
      outpostArn: ""
      port: 6379
    engine: redis
    engineVersion: "7.1"
    engineVersionActual: 7.1.0
    id: cache-qsfmj-d4qdn
    ipDiscovery: ipv4
    maintenanceWindow: fri:13:00-fri:14:00
    networkType: ipv4
    nodeType: cache.t3.micro
    numCacheNodes: 1
    parameterGroupName: cache-redis7
    port: 6379
    preferredOutpostArn: ""
    replicationGroupId: ""
    securityGroupIds:
    - sg-078a8c14afac5bd03
    snapshotRetentionLimit: 0
    snapshotWindow: 08:00-09:00
    subnetGroupName: cache-qsfmj-5hx56
    tags:
      crossplane-kind: cluster.elasticache.aws.upbound.io
      crossplane-name: cache-qsfmj-d4qdn
      crossplane-providerconfig: default
    tagsAll:
      crossplane-kind: cluster.elasticache.aws.upbound.io
      crossplane-name: cache-qsfmj-d4qdn
      crossplane-providerconfig: default
    transitEncryptionEnabled: false
```

Prior to running end to end tests with `make e2e`, configure an
`UPTEST_CLOUD_CREDENTIALS` environment variable with the contents of
your AWS config credentials file. It will need an AWS access key id,
an AWS secret access key, and depending on your setup am AWS session token.
