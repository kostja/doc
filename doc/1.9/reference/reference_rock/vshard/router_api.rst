* :ref:`vshard.router.bootstrap() <router_api-bootstrap>`
* :ref:`vshard.router.cfg(cfg, instance_uuid) <router_api-cfg>`
* :ref:`result = vshard.router.call(bucket_id, mode(read:write), function_name, {argument_list}, {options}) <router_api-call>`
* :ref:`netbox, err = vshard.router.route(bucket_id) <router_api-route>`
* :ref:`vshard.router.routeall() <router_api-routeall>`
* :ref:`vshard.router.bucket_id(key) <router_api-bucket_id>`
* :ref:`vshard.router.bucket_count() <router_api-bucket_count>`
* :ref:`vshard.router.sync(timeout) <router_api-sync>`
* :ref:`vshard.router.bucket_discovery(bucket_id) <router_api-bucket_discovery>`
* :ref:`vshard.router.discovery_wakeup() <router_api-discovery_wakeup>`
* :ref:`vshard.router.info() <router_api-info>`

.. _router_api-bootstrap:

.. function:: vshard.router.bootstrap()

    Perform the initial cluster bootstrap and distribute all buckets across the
    replica sets.

.. _router_api-cfg:

.. function:: vshard.router.cfg(cfg, instance_uuid)

    Configure the database and start sharding for the specified ``router`` instance.

    :param cfg: a ``router`` configuration
    :param instance_uuid: an uuid of the instance

.. _router_api-call:

.. function:: result = vshard.router.call(bucket_id, mode(read:write), function_name, {argument_list}, {options})

    Call the user function on the shard storing the bucket with the specified
    bucket id. See Processing Requests section for details on function operation.

    :param bucket_id: a bucket identifier
    :param mode: a type of the function: read or write
    :param function_name: a function to execute
    :param argument_list: an array of the function's arguments
    :param options:

        * ``timeout`` – a request timeout, in seconds. In case the router cannot identify a
          shard with the bucket id, the operation will be repeated until the
          timeout is reached.

    :Return:

    The original return value of the executed function, or ``nil`` and
    error object. The error object has a type attribute equal to ``ShardingError``
    or one of the regular Tarantool errors (``ClientError``, ``OutOfMemory``,
    ``SocketError``, etc.).

    ``ShardingError`` is returned on errors specific for sharding: the replica
    set is not available, the master is missing, wrong bucket id, etc. It has an
    attribute code containing one of the values from ``vshard.error.code()``, an
    optional attribute containing a message with the human-readable error description,
    and other attributes specific for this error code.

    **Example**

    To call ``customer_add`` function from ``vshard/example``, say:

.. code-block:: tarantoolsession

    result = vshard.router.call(100, 'write', 'customer_add', {{customer_id = 2, bucket_id = 100, name = 'name2', accounts = {}}}, {timeout = 100})

.. _router_api-route:

.. function:: netbox, err = vshard.router.route(bucket_id)

    Return the replica set object for the bucket with the specified bucket id.

    :param bucket_id: a bucket identifier

    :Return: a netbox connection
    :Rtype: netbox

.. _router_api-routeall:

.. function:: vshard.router.routeall()

    Return all available replica set objects.

    :Return: a map of the following type: ``{UUID = replicaset}``
    :Rtype: a replica set object

.. _router_api-bucket_id:

.. function:: vshard.router.bucket_id(key)

    Calculate the bucket id using a simple built-in hash function.

    :Return: a bucket identified
    :Rtype: number

.. _router_api-bucket_count:

.. function:: vshard.router.bucket_count()

    Return the total number of buckets specified in vshard.router.cfg().

    :Return: the total number of buckets
    :Rtype: number

.. _router_api-sync:

.. function:: vshard.router.sync(timeout)

    Wait until the dataset is synchronized on replicas.

    :param timeout: a timeout, in seconds

.. _router_api-bucket_discovery:

.. function:: vshard.router.bucket_discovery(bucket_id)

    Search for the bucket in the whole cluster. If the bucket wasn’t
    found, it’s likely that it doesn’t exist. The bucket might also be
    moved during rebalancing and currently is in the RECEIVING state.

    :param bucket_id: a bucket identifier

.. _router_api-discovery_wakeup:

.. function:: vshard.router.discovery_wakeup()

    Force wakeup of the bucket discovery fiber.

.. _router_api-info:

.. function:: vshard.router.info()

    Return the information on each instance.

    :Return:

    Replica set parameters:

    * replica set uuid
    * master instance parameters
    * replica instance parameters

    Instance parameters:

    * ``uri``
    * ``uuid``
    * ``status`` – available, unreachable, missing
    * ``network_timeout`` – a timeout for the request. The value is updated automatically
      on each 10th successful request and each 2nd failed request.

    Bucket parameters:

    * ``available_ro`` – the number of buckets known to the router and available for read requests
    * ``available_rw`` – the number of buckets known to the router and available for read and write requests
    * ``unavailable`` – the number of buckets known to router but unavailable for any requests
    * ``unreachable`` – the number of buckets which replica sets are not known to the router

    **Example**

.. code-block:: tarantoolsession

    unix/:./data/router_1.control> vshard.router.info()
    ---
    - replicasets:
        ac522f65-aa94-4134-9f64-51ee384f1a54:
          replica: &0
            network_timeout: 0.5
            status: available
            uri: storage@127.0.0.1:3303
            uuid: 1e02ae8a-afc0-4e91-ba34-843a356b8ed7
          uuid: ac522f65-aa94-4134-9f64-51ee384f1a54
          master: *0
        cbf06940-0790-498b-948d-042b62cf3d29:
          replica: &1
            network_timeout: 0.5
            status: available
            uri: storage@127.0.0.1:3301
            uuid: 8a274925-a26d-47fc-9e1b-af88ce939412
          uuid: cbf06940-0790-498b-948d-042b62cf3d29
          master: *1
      bucket:
        unreachable: 0
        available_ro: 0
        unknown: 0
        available_rw: 3000
      status: 0
      alerts: []
    ...
