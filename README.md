## Overview

The Apache Hadoop software library is a framework that allows for the
distributed processing of large data sets across clusters of computers
using a simple programming model.

It is designed to scale up from single servers to thousands of machines,
each offering local computation and storage. Rather than rely on hardware
to deliver high-avaiability, the library itself is designed to detect
and handle failures at the application layer, so delivering a
highly-availabile service on top of a cluster of computers, each of
which may be prone to failures.

This bundle provides a complete deployment of the core components of the
[Apache Bigtop](http://bigtop.apache.org/)
platform to perform distributed data analytics at scale.  These components
include:

  * NameNode (HDFS)
  * ResourceManager (Yarn)
  * Slaves (DataNode and NodeManager)
  * Plugin (Subordinate Bigtop Gateway)

Deploying this bundle gives you a fully configured and connected Apache Bigtop
cluster on any supported cloud, which can be easily scaled to meet workload
demands.


## Deploying this bundle

In this deployment, the aforementioned components are deployed on separate
units. To deploy this bundle, simply use:

    juju quickstart bigtop-processing-mapreduce

See `juju quickstart --help` for deployment options, including machine
constraints and how to deploy a locally modified version of `bundle.yaml`.

The default bundle deploys three slave nodes and one node of each of
the other services. To scale the cluster, use:

    juju add-unit slave -n 2

This will add two additional slave nodes, for a total of five.


### Verify the deployment

The services provide extended status reporting to indicate when they are ready:

    juju status --format=tabular

This is particularly useful when combined with `watch` to track the on-going
progress of the deployment:

    watch -n 0.5 juju status --format=tabular

The charms for each master component (namenode, resourcemanager)
also each provide a `smoke-test` action that can be used to verify that each
component is functioning as expected.  You can run them all and then watch the
action status list:

    juju action do namenode/0 smoke-test
    juju action do resourcemanager/0 smoke-test
    watch -n 0.5 juju action status

Eventually, all of the actions should settle to `status: completed`.  If
any go instead to `status: failed` then it means that component is not working
as expected.  You can get more information about that component's smoke test:

    juju action fetch <action-id>


## Deploying in Network-Restricted Environments

The Apache Hadoop charms can be deployed in environments with limited network
access. To deploy in this environment, you will need a local mirror to serve
the packages and resources required by these charms.

### Mirroring Packages

You can setup a local mirror for apt packages using squid-deb-proxy.
For instructions on configuring juju to use this, see the
[Juju Proxy Documentation](https://juju.ubuntu.com/docs/howto-proxies.html).


## Contact Information

- <bigdata@lists.ubuntu.com>


## Resources

- [Apache Hadoop](http://hadoop.apache.org/) home page
- [Apache Hadoop bug tracker](http://hadoop.apache.org/issue_tracking.html)
- [Apache Hadoop mailing lists](http://hadoop.apache.org/mailing_lists.html)
- [Apache Bigtop charms](https://jujucharms.com/q/apache/bigtop)
- [Bundle issue tracker](https://github.com/juju-solutions/bundle-bigtop-processing-mapreduce/issues)
- [Juju mailing list](https://lists.ubuntu.com/mailman/listinfo/juju)
- [Juju community](https://jujucharms.com/community)
