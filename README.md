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

Exercise overwrites of names or inject secrets as defined in the
function call.

override the default generation of a secret from the *ssh_key_type*
variable defined in .cfg/mycfg with other renames or overrides.

The default implementation uses make-yaml-stdio as it's driver.

Examples are in .cfg.

#### make-yaml-stdio

make-yaml-stdio reading stdio and writing to stdout manipulating the
content (stream) with the same rewrite options used for the yaml
generation from the {name}-template.yaml file.

#### make-yaml-call

make-yaml-call with a file argument, manipulating the contents of the
given file with the same rewrite options used for the yaml generation
from the {name}-template.yaml file.

### usage

Prints details for over ride usages.


### overrides

The current directory's .cfg/overrides can be used to override
definitions and variables like the usage and make-yaml functions, for
example.

---
### sharing with another project

cd /path/to/project/ link or copy a version of k8s-cfg scripts to
.k8s-cfg and update .cfg/{usage,make-yaml,mycfg,overrides}.

At a minimum .cfg/mycfg is required to point to the clusters address
and naming convention. 

    ln -s /path/to/k8s-cfg/.k8s-cfg .k8s-cfg

If you need to send a build script to the cluster via the service you
can proxy the location or curl to the address


If you need to use json, you might use yaml2json2yaml's

    yaml2json --compress < yaml > json . . .

As a preference the current version of the spec is derived from
k8s-bldr-api v0.1, while the actual path for the api is api/v1 for now
and will remain so, v0.1 up to v0.9 as this is considered alpha
software.
