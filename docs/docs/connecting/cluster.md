# Connecting to a cluster

::: tip
Packages required
- EventStore.Client.Grpc.Streams
:::

## Simple connection

For redundancy, you can run EventStoreDB in a cluster. In this scenario you should specify all the nodes in your cluster when connecting. This allows your client to discover which node it should be speaking to at any given time.

::: warning
Important: You should never hide your cluster behind a load balancer as this will stop your clients from connecting to the correct nodes.
:::
 
<<< @/samples/connecting-to-a-cluster/Program.cs#connecting-to-a-cluster
 
## Fine-tuning cluster connection

There are a number of additional settings that can be altered when connecting to a cluster.

<<< @/samples/connecting-to-a-cluster/Program.cs#connecting-to-a-cluster-complex

### DiscoveryInterval

The interval between node discovery attempts. If the client attempts to discover a node, and it is unresponsive this is the length of time the client will wait before trying to discover it again.

**Default**: `100` (ms)

### GossipTimeout

The length of time the client will wait to get a gossip request from a node. The gossip requests lets the client know the current state of any given node. To find out more see the gossip protocol [documentation](/server/20.6/server/clustering/gossip).

**Default**: `10` (sec)

### NodePreference

You can tell your client which node type you would prefer to connect to. The options are:

| Option | Purpose |
|:------ |:------- |
| `NodePreference.Leader` | Always try and connect to the leader node |
| `NodePreference.Follower` | Always try and connect to a follower node | 
| `NodePreference.Random` | Connect to a random node type (useful if you want to spread the connections around) |
| `NodePreference.ReadOnlyReplica` | Always try and connect to a read only replica |

Find out more about different node roles in the [serevr documentation](/server/20.6/server/clustering/node-roles). 

**Default**: `NodePreference.Leader`

### MaxDiscoveryAttempts

This is the number of times we will attempt to discover the node before EventStoreDb aborts and throws an exception.

**Default**: `10`

## Providing default credentials

When creating a connection EventStoreDB allows you to set default credentials. These will be used for executing all commands unless they are explicitly overridden.

<<< @/samples/connecting-to-a-cluster/Program.cs#providing-default-credentials

<!-- TODO: Add link to supply with write -->
