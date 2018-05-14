.. NOTE::

    The cluster default name is *'default'*.

* :ref:`/tarantool/cluster/{cluster_name}/new_nodes/{host_name:instance_name} <zookeper_structure-instance_info>`
* :ref:`/tarantool/cluster/{cluster_name}/node/{uuid} <zookeper_structure-instance_status>`
* :ref:`/tarantool/cluster/{cluster_name}/cfg <zookeper_structure-cluster_cfg>`

.. _zookeper_structure-instance_info:

.. confval:: /tarantool/cluster/{cluster_name}/new_nodes/{host_name:instance_name}

    The directory contains information on the Tarantool instance. This
    information is obtained once the instance is detected.

    **Example**

    .. code-block:: kconfig

        {
            "hostname": host_name,
            "name": instance_name,
            "uuid": "uuid-1"
        }

.. _zookeper_structure-instance_status:

.. confval:: /tarantool/cluster/{cluster_name}/node/{uuid}

    The directory contains the Tarantool instance status.

    **Example**

    .. code-block:: kconfig

        "status": ['online'/'resharding'/'offline']

.. _zookeper_structure-cluster_cfg:

.. confval:: /tarantool/cluster/{cluster_name}/cfg

    The directory contains the configuration of the entire cluster.

    **Example**

    .. code-block:: kconfig

        {
            "zones": {
                1:  {
                  "id": 1,
                  "name": 'DC 1',
                }
            },
            "servers": {
                INSTANCE1_UUID: {
                    "zone": 1,
                    "hostname": "tnt1.a.public.i",  # from new_nodes
                    "name": "tnt1",  # from new_nodes
                    "uri": "tnt1.a.public.i:3301",
                    "user": "user1:pass1",
                    "repl_user": "repl_user1:repl_pass1",
                    "cfg": {
                      "listen": "0.0.0.0:3301",
                      # ...other box.cfg{} parameters
                    }
                },
                INSTANCE2_UUID: { ... },
                ...
            },

            "cluster": {
                "cfg": {
                    "vbuckets": 1000,
                    # sharding config parameters
                },

                "routers": [
                    INSTANCE1_UUID,
                    ...
                ],

                "storage": {  # replica sets
                    REPLICASET1_UUID: {  # replica set 1
                        INSTANCE1_UUID: {
                            "master": true
                        },
                        INSTANCE2_UUID: {
                            "master": false
                        },
                    },

                    {  # replica set 2
                      ...
                    }
                }
            }
        }

