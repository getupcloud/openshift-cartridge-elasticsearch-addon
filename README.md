[Versão Português](https://github.com/getupcloud/openshift-elasticsearch-addon-cartridge/blob/master/README_pt.md)

OpenShift Elasticsearch Cartridge
=================================
This cartridge provides an embedded Elasticsearch cluster to your application.

In order to create your application, first you need to register at Getup Cloud.
Go to http://getupcloud.com/#/sign-up and signup for free.
You receive a free 750h trial to test our services.

To add Elasticsearch into your app, run from the terminal:

    rhc cartridge-add --app <app> http://reflector-getupcloud.getup.io/github/getupcloud/openshift-cartridge-elasticsearch-addon

To create a scalable app, append option `--scaling` to command above.

Adding nodes to your cluster
============================
Before we add some nodes to the cluster, you need to provide a env var OPENSHIFT_ELASTICSEARCH_MASTER with connection info of your first ES gear.

SSH into your ES gear:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL>

Take note of following variables:

    > env | grep OPENSHIFT_GEAR_DNS
    > env | grep OPENSHIFT_ELASTICSEARCH_NODE_PROXY_PORT
    > exit

* If nothing appears on second command, maybe your app is no scalable. Please see first section above.

Export the connection info to your app:

    $ rhc env-set --app <app> OPENSHIFT_ELASTICSEARCH_MASTER=$GEAR_DNS:$NODE_PROXY_PORT
                                                             ^^^^^^^^^ ^^^^^^^^^^^^^^^^
Now we are ready to add new nodes to the cluster 

    $ rhc cartridge-scale --app <app> getup-elasticsearch-addon 2 

Plugins
=======
In order to install plugins you need first to SSH into your ES gear(s) and run the command `elasticsearch-addon/bin/plugin` as usual:

    $ rhc app-show <app> --gears
    $ ssh <ELASTICSEARCH-GEAR-SSH-URL> elasticsearch-addon/bin/plugin --install mobz/elasticsearch-head

Configuration
=============
Configuration file is built during startup, from source file `elasticsearch/config/elasticsearch.yml.erb`.

Credits
=======
Based on initial work from https://github.com/ncdc/openshift-elasticsearch-cartridge.

License
=======
This project is licensed under the [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
