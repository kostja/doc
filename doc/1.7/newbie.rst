.. _newbie:

********************************************************************************
Quick start
********************************************************************************

This quick start will help you try what Tarantool is like.

================================================================================
Starting Tarantool
================================================================================

You can:

* run Tarantool in a Docker container,
* install Tarantool from a binary package, or
* build Tarantool from source files.

Choose what you like:

.. container:: b-block-wrapper_doc

    .. container:: b-doc_catalog
        :name: catalog-1

        .. raw:: html

            <ul class="b-tab_switcher">
                <li class="b-tab_switcher-item">
                    <a href="#terminal-1-1" class="b-tab_switcher-item-url p-active">Docker</a>
                </li>
                <li class="b-tab_switcher-item">
                    <a href="#terminal-1-2" class="b-tab_switcher-item-url">Binary</a>
                </li>
                <li class="b-tab_switcher-item">
                    <a href="#terminal-1-3" class="b-tab_switcher-item-url">Source</a>
                </li>
            </ul>

    .. container:: b-documentation_tab_content
        :name: catalog-1-content

        .. container:: b-documentation_tab
            :name: terminal-1-1

            Create a sandbox directory for test purposes:

            .. code-block:: console

                $ mkdir ~/tarantool-sandbox

            To run Tarantool in Docker, say this:

            .. code-block:: console

                $ docker run \
                  --name example \
                  -d -p 3301:3301 \
                  -v /full/path/to/tarantool-sandbox:/var/lib/tarantool \
                  tarantool/tarantool:1.7

            .. NOTE::

                For ``-v`` option, specify an absolute path to
                ``~/tarantool-sandbox`` directory.

        .. container:: b-documentation_tab
            :name: terminal-1-2

            Create a sandbox directory for test purposes:

            .. code-block:: console

                $ mkdir ~/tarantool-sandbox

            To install Tarantool from a binary package, visit our
            `download page <http://tarantool.org/downloads.html>`_
            and copy-paste installation commands for your OS.

            To run Tarantool, say this:

            .. code-block:: console

                $ tarantoolctl start PATH-TO-EXAMPLE.LUA

        .. container:: b-documentation_tab
            :name: terminal-1-3

            Create a sandbox directory for test purposes:

            .. code-block:: console

                $ mkdir ~/tarantool-sandbox

            To build Tarantool from source files, follow the
            :ref:`build instructions <building_from_source>` that
            include OS-specific notes.
            Here's an example for a generic GNU/Linux system.

            .. code-block:: console

                # PREREQUISITES:
                # - gcc and g++, or clang
                # - git
                # - cmake
                $ git clone https://github.com/tarantool/tarantool.git ~/tarantool-sandbox
                $ cd ~/tarantool-sandbox
                $ git submodule init
                $ git submodule update --init --recursive
                $ cmake . -DENABLE_DIST=ON
                $ make
                $ make install

            To run Tarantool, say this:

            .. code-block:: console

                $ cd ~/tarantool-sandbox
                $ tarantoolctl start extra/dist/example.lua

================================================================================
Attaching to Tarantool
================================================================================

You can attach to a running Tarantool instance and use Tarantool as a client.

The exact command depends on how you started Tarantool: in a Docker container,
from a binary package, or built from source files. Choose what you need:

.. container:: b-block-wrapper_doc

    .. container:: b-doc_catalog
        :name: catalog-2

        .. raw:: html

            <ul class="b-tab_switcher">
                <li class="b-tab_switcher-item">
                    <a href="#terminal-2-1" class="b-tab_switcher-item-url p-active">Docker</a>
                </li>
                <li class="b-tab_switcher-item">
                    <a href="#terminal-2-2" class="b-tab_switcher-item-url">Binary</a>
                </li>
                <li class="b-tab_switcher-item">
                    <a href="#terminal-2-3" class="b-tab_switcher-item-url">Source</a>
                </li>
            </ul>

    .. container:: b-documentation_tab_content
        :name: catalog-2-content

        .. container:: b-documentation_tab
            :name: terminal-2-1

            .. code-block:: console

                $ docker exec -i -t example console

        .. container:: b-documentation_tab
            :name: terminal-2-2

            .. code-block:: console

                $ tarantoolctl enter example

        .. container:: b-documentation_tab
            :name: terminal-2-3

            .. code-block:: console

                $ tarantoolctl enter example

================================================================================
Creating a database
================================================================================

Scenario: create a space + primary index + a key-value record + select

get instructions from doc/1.7/book/getting_started/*

================================================================================
Loading data from your favorite database
================================================================================

Scenario: export data in CSV + adjust schema description to make it similar to
an example we give here + upload to Tarantool

================================================================================
Getting top performance
================================================================================

Scenario: use Dennis' experiment with 1 million transactions on a CPU core
(make it easy to copy-paste and run locally)

================================================================================
Setting up replication
================================================================================

Scenario: MM cluster of 2 instances

================================================================================
Connecting from your favorite language
================================================================================

We need copy-paste instructions 4-5 most popular langs:
Python, C#, Perl, Java, Go, PHP?
