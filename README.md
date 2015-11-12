# k8s-cfg

Simple config and command line interface for working with a k8s
services or replicas.

Create the files make-yaml usage and mycfg in .cfg/ examples are in
the .cfg directory. 

There is a namespacing make-yaml example to configure renames,
enabling multiple versions to run in the same cluster.

The config is in bash, so non bashisms may break. . .

Sourcing the kubecfg adds an alias to the current shell for direct
access.

    . .k8s-cfg/kubecfg

With prompt for user and password for the cluster, assuming
credentials are enabled for https and basic user/pass authentication.

    k8s-cfg --status

prints nodes, pods, replicationcontrollers services endpoints and secrets for all namespaces so

    k8s-cfg --status|grep service

may be used to reduce the output.

----

### make-yaml

Modifies the name space and renames or options

### usage

Prints details for over ride usages.




