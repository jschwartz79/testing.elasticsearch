testing.elasticsearch
=====================

`testing.elasticsearch` automatically sets up an elasticsearch instance in a
temporary directory, and destroys it after testing. It's useful as a pytest
fixture for testing interactions with elasticsearch in an isolated manner.


Implementation is based off the awesome  `testing.redis <https://bitbucket.org/tk0miya/testing.redis>`_ module.

Example usage:

.. code-block:: python

    import testing.elasticsearch
    import pyes.es import ES

    # launch new elasticsearch server:
    with testing.elasticsearch.ElasticSearchServer() es:
        elasticsearch = ES(es.uri())
        # perform any testing with elasticsearch here

    # elasticsearch server is terminated and cleaned up here


You can change the server configuration by specifying a `config` dict:

.. code-block:: python

    with ElasticSearchServer(config={
        'logger.level': 'DEBUG',
        # Keep index in memory
        'index.store.type': 'mmapfs',
    }) as es:
        ...


...or by setting them on the `config` attribute before starting the server:

.. code-block:: python

    es = ElasticSearchServer()
    es.config['logger.level'] = 'DEBUG'
    es.start()


You can also setup a pytest fixture:

.. code-block:: python

    @pytest.fixture(scope='session')
    def elasticsearch(request):
        """
        A testing fixture that provides a running elasticsearch server.
        """
        es = ElasticSearchServer()
        es.start()
        request.addfinalizer(es.stop)
        return es


Testing
-------

To run tests you'll need to install the test requirements::

    pip install -r src/tests/requirements.txt

Run tests::

    python src/tests/runtests.py
