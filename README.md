# k8s-cfg

Simple config and command line interface for working with a k8s
services or replicas.

Create the files make-yaml usage and config in .cfg/ examples are in
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
function call, stream editing the yaml text. The text is not written
to disk unless --debug is specified, so that secrets aren't stored in
unrestricted locations.

override the default generation of a secret from the *ssh_key_name*
variable defined in .cfg/config with other renames or overrides.

Notice the name conversion of **_** to **-** for secrets. Underscores
weren't allowed as of v1.0x kubernetes which this was tested with.

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
.k8s-cfg and update .cfg/{usage,make-yaml,config,overrides}.

At a minimum .cfg/config is required to point to the clusters address
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

----
### defaults / initial configuration

If using a reconfigured name system then the id might be needed.
For example if the following were used to modify a terraform script

```
# terraform.tvars.default
# Complete the required configuration below and copy this file and
# openstack.sample.tf or openstack-floating.sample.tf to the root directory
# before running terraform commands

# Configuration variables

auth_url = {}
tenant_id = {}
tenant_name = {}
public_key = {}
cluster_name = "cluster.local"
image_name = "CentOS-71"
master_flavor = "GP2-Large"
node_flavor = "GP2-Xlarge"
master_count = "1"
node_count = "2"
datacenter = "dc1"
glusterfs_volume_size = "2"
short_name = {}
keypair_name = {}

# If using openstack-floating.sample.tf, set the two variables below

floating_pool = "public-floating-601"
external_net_id = "8f3508a9-d4f5-4f9c-a5da-fd7f04059303"

# If using openstack.sample.tf, set the following
net_id = ""
```

```
# appended to txsdemopaas to reformat some options
SHORT_NAME=k8s-dw-txs
sed -e "s,auth_url *= *{},auth_url = \"${OS_AUTH_URL}\",g" terraform.tfvars.default > terraform.tfvars
sed -i -e "s,tenant_id *= *{},tenant_id = \"${OS_TENANT_ID}\",g" terraform.tfvars
sed -i -e "s,tenant_name *= *{},tenant_name = \"${OS_TENANT_NAME}\",g" terraform.tfvars
sed -i -e "s,short_name = {},short_name = \"${SHORT_NAME}\",g" terraform.tfvars
sed -i -e "s,keypair_name = {},keypair_name = \"${SHORT_NAME}-keypair\",g" terraform.tfvars
sed -i -e "s,public_key = {},public_key = \"~/.ssh/id_rsa.pub\",g" terraform.tfvars
```

Since the default naming of kubernetes-ansible was k8s-{master,node} like,

    k8s-master-[0-1][0-9]
    k8s-node-[0-1][0-9]

e.g., k8s-master-01, if renaming these for uniqueness, then replace
the id with a value like dw-txs.

```
id=""
ssh_key_name=rsa
master=k8s-dw-txs-master-01
os_user=centos
name=${dir##*/}
namespace=${name}
yaml=${dir}/${name}.yaml
template=${dir}/${name}-template.yaml
domain=cluster.local
hubuid=davidwalter0
k8srepo=git@${id}k8s-git-repo:${name}.git
hubrepo=git@github.com:${hubuid}/${name}.git
```


```
id=dw-txs-
ssh_key_name=rsa
master=k8s-${id}master-01
os_user=centos
name=${dir##*/}
namespace=${name}
yaml=${dir}/${name}.yaml
template=${dir}/${name}-template.yaml
domain=cluster.local
hubuid=davidwalter0
k8srepo=git@${id}k8s-git-repo:${name}.git
hubrepo=git@github.com:${hubuid}/${name}.git
```

