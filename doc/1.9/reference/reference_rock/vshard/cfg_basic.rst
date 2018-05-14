* :ref:`sharding <cfg_basic-sharding>`
* :ref:`weights <cfg_basic-weights>`
* :ref:`shard_index <cfg_basic-shard_index>`
* :ref:`bucket_count <cfg_basic-bucket_count>`
* :ref:`collect_bucket_garbage_interval <cfg_basic-collect_bucket_garbage_interval>`
* :ref:`collect_lua_garbage <cfg_basic-collect_lua_garbage>`
* :ref:`sync_timeout <cfg_basic-sync_timeout>`
* :ref:`rebalancer_disbalance_threshold <cfg_basic-rebalancer_disbalance_threshold>`
* :ref:`rebalancer_max_receiving <cfg_basic-rebalancer_max_receiving>`

.. _cfg_basic-sharding:

.. confval:: sharding

    A field defining the logical topology of the sharded Tarantool cluster.
    See Configured Sharded Cluster Example section.

    | Type: table
    | Default: false
    | Dynamic: yes

.. _cfg_basic-weights:

.. confval:: weights

    A field defining the configuration of relative weights for each zone pair in a
    replica set. See Replica Weight section.

    | Type: table
    | Default: false
    | Dynamic: yes

.. _cfg_basic-shard_index:

.. confval:: shard_index

    An index over the bucket id.

    | Type: non-empty string or non-negative integer
    | Default: coincides with the bucket id number
    | Dynamic: no

.. _cfg_basic-bucket_count:

.. confval:: bucket_count

    A total number of buckets in a cluster.

    This number should be several orders of magnitude larger than the potential number
    of cluster nodes, considering the potential scaling out in the foreseeable future.

    Example:
    If the estimated number of nodes is M, then the data set should be divided into
    100M or even 1000M buckets, depending on the planned scale out. This number is
    certainly greater than the potential number of cluster nodes in the system being
    designed.

    Mind that too many buckets can cause the need to allocate more memory to store
    routing information. In its turn, an insufficient number of buckets can lead to
    decreased granularity when rebalancing.

    | Type: number
    | Default: 3000
    | Dynamic: no

.. _cfg_basic-collect_bucket_garbage_interval:

.. confval:: collect_bucket_garbage_interval

    The interval between the garbage collector actions, in seconds.

    | Type: number
    | Default: 0.5
    | Dynamic: yes

.. _cfg_basic-collect_lua_garbage:

.. confval:: collect_lua_garbage

    If set to true, the Lua’s collectgarbage() function is called periodically.

    | Type: boolen
    | Default: no
    | Dynamic: yes

.. _cfg_basic-sync_timeout:

.. confval:: sync_timeout

    Timeout to wait for synchronization with replicas. Used when switching a
    master or when manually calling ``sync()`` function.

    | Type: number
    | Default: 1
    | Dynamic: yes

.. _cfg_basic-rebalancer_disbalance_threshold:

.. confval:: rebalancer_disbalance_threshold

    A maximal bucket disbalance threshold, in percent.
    The threshold is calculated for each replica set using the following formula:

    .. code-block:: console

        |etalon_bucket_count - real_bucket_count| / etalon_bucket_count * 100

    | Type: number
    | Default: 1
    | Dynamic: yes

.. _cfg_basic-rebalancer_max_receiving:

.. confval:: rebalancer_max_receiving

    The maximal number of buckets that can be received in parallel by a single
    replica set. This number must be limited, as when a new replica set is added to
    a cluster, the rebalancer sends a very large amount of buckets from the existing
    replica sets to the new replica set. This produces a heavy load on a new replica set.

    Example:

    Suppose ``rebalancer_max_receiving`` is equal to 100, ``bucket_count`` is equal to 1000.
    There are 3 replica sets with 333, 333 and 334 buckets on each correspondingly.
    When a new replica set is added, each replica set’s ``etalon_bucket_count`` becomes
    equal to 250. Rather than receiving all 250 buckets at once, the new replica set
    receives 100, 100 and 50 buckets sequentially.

    | Type: number
    | Default: 100
    | Dynamic: yes
