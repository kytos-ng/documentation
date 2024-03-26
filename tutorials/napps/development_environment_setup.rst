:tocdepth: 2
:orphan:

.. _tutorial-setup-the-development-environment:

###############################
Setting up your dev environment
###############################

.. NOTE:: We tested this tutorial on Debian 12 bookworm, but feel free to adapt to your
  preferred Linux distribution.

********
Overview
********

This tutorial shows how to setup your development environment in order to run
the latest Kytos SDN Platform code (python-openflow, kytos, kytos NApps, ...) and
contribute to it.

.. DANGER:: Do not use this procedure in production environments.
   This setup is for development only.

.. CAUTION:: Code to be run in terminal begins with the dollar sign ($). If
  you copy and paste, don't forget to skip this symbol.

The average time to go through it is: ``10 min``.

What you will learn
===================

* How to create an isolated environment for development;
* How to install the Kytos Projects in a development environment;
* How to install Mininet.

********************************
Installing required dependencies
********************************

In order to start using and coding with Kytos, you need a few required
dependencies. One of them is Python 3.11.x


Installing Python 3.11.x
========================

Install the dependencies necessary to build Python:

.. code-block:: console

  sudo apt update
  sudo apt install python3.11-dev python3.11-venv

Required packages
=================

The required Debian package can be installed by:

.. code-block:: console

  sudo apt install git


********************************
Setting up a virtual environment
********************************

To make changes to Kytos projects, we recommend you to use
|venv|_. The main reason for this recommendation is to keep the dependencies
required by different projects in separate places by creating virtual Python
environments for each one. It solves the “Project X depends on version 1.x, but
Project Y needs 4.x” dilemma, and keeps your global site-packages directory
clean and manageable.

In this tutorial, we will use the new built-in :mod:`venv` Python module,
but if you wish to use another tool to create isolated environments or install
libraries on your global system, feel free to do it your way.

Creating a new virtualenv
=========================

To create a new virtualenv, use the command below (you can replace ``test42``
by another name, if you wish):

.. code-block:: console

  python3.11 -m venv test42

This command will create a virtualenv named *test42* and a folder with the same
name for it.

.. NOTE:: Kytos requires Python 3.11.x

Removing a virtualenv
---------------------

If you want to remove an existing virtualenv, just delete its folder
(e.g. ``rm -rf test42``).

Using the virtual environment
=============================

If you want to use an existing environment you can use the following command:

.. code-block:: console

  source test42/bin/activate

After that, your console prompt will show the activated virtualenv name between
parenthesis. Now, update the *pip* package that is already installed in the
virtualenv, with setuptools and wheel as well:

.. code-block:: console

  (test42) pip install --upgrade pip setuptools wheel

The parenthesis marker identifies that the test42 virtualenv is activated. If
you want leave this virtualenv you can use the command ``deactivate``.
After this, the virtualenv name will disappear from your prompt and you will be
using your regular Linux environment.

.. note:: Inside the virtualenv, all pip packages will be installed within the
   *test42* folder. Outside the virtualenv, all pip packages will be installed
   into the default system environment (standard Linux folder).

If you want to read more about it, please visit: |virtualenv|_ and
|virtualenv_docs|_ pages.

***********************************
Installing the latest Kytos release
***********************************

Installing from Source
======================

To install the latest Kytos from source, first you need to start your environment:

.. code-block:: console

  $ source YOURENV/bin/activate

Then you need to run the commands below to clone the python-openflow,
kytos-utils and kytos projects locally.

.. code-block:: shell

  for repo in python-openflow kytos-utils kytos; do
    git clone https://github.com/kytos-ng/"${repo}"
  done

After cloning, the Kytos installation process is done running setuptools installation
procedure for each cloned repository, in order. Below we execute its commands.

.. code-block:: shell

    for repo in python-openflow kytos-utils kytos; do
      cd "${repo}"
      python -m pip install --editable .
      cd ..
    done

Installing the NApps from Kytos team
====================================

We will now install some NApps developed by the Kytos team, which will be used
later in the following tutorials.

.. NOTE:: Currently, NApps should be installed by cloning the source code and installing through the setup.py file

Currently, flow_manager and topology require MongoDB to be setup. Before installing the Napps, follow the instructions below

How to use with MongoDB
=======================

Kytos with MongoDB requires docker and docker-compose to be installed on your linux environment. The following tutorials have been tested on Debian 12 for docker and docker-compose


Installing docker
-----------------

`Install docker <https://docs.docker.com/engine/install/debian/#install-using-the-repository/>`_

Set docker as non-root
----------------------

`Set up non-root user <https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user/>`_ (Follow only the ``"Manage Docker as a non-root user"`` section)

Installing docker-compose
-------------------------

`Install docker-compose <https://docs.docker.com/compose/install/linux/#install-using-the-repository>`_

After installing docker and docker-compose you can follow this link to setup Kytos with MongoDB: `Kytos-MongoDB <https://github.com/kytos-ng/kytos#how-to-use-with-mongodb>`_

.. NOTE:: MongoDB 5.0+ requires a CPU that supports AVX instructions. 

    Also, if you're running VirtualBox on Windows as host and a Debian guest you'll need to disable Hyper-V:
    Find the Command Prompt icon, right click it and choose Run As Administrator.
    Enter this command: bcdedit /set hypervisorlaunchtype off.
    Some report this command was needed also: DISM /Online /Disable-Feature:Microsoft-Hyper-V.
    Reboot.


of_core, flow_manager, topology, of_lldp, of_l2ls:

.. code-block:: shell

  for repo in of_core flow_manager topology of_lldp of_l2ls; do
    git clone https://github.com/kytos-ng/"${repo}"
  done


.. code-block:: shell

  for repo in of_core flow_manager topology of_lldp of_l2ls; do
    cd "${repo}"
    python3 setup.py develop
    cd ..
  done

If you wish to disable all NApps you can use the following command, or for an individual NApp you can use replace "all" with the NApp name

.. code-block:: console

  $ kytos napps disable all

Now open another terminal window and run the following command to start the kytos server if it is not already running

.. code-block:: console

  $ kytosd -f --database mongodb

That's it! you can type ``quit`` to exit
Kytos.


One more step: Mininet.

How to install Mininet
======================

Mininet is a network simulator that creates a network of virtual hosts,
switches, controller and the links among them. Mininet hosts run standard Linux
network software, and its switches support Openflow for highly flexible custom
routing and Software Defined Networking.

First, we need to install the mininet package. The `Mininet project
<http://mininet.org/>`_ lists a few methods for installing the simulator. For
instance, you can use a virtual machine or you can install it to you operating
system.

.. code-block:: console

  $ sudo apt install mininet

To test if the mininet is working for you, run the command:

.. code-block:: console

  $ sudo mn --test pingall --controller=remote,ip=127.0.0.1,port=6653
  *** No default OpenFlow controller found for default switch!
  *** Falling back to OVS Bridge
  *** Creating network
  *** Adding controller
  *** Adding hosts:
  h1 h2
  *** Adding switches:
  s1
  *** Adding links:
  (h1, s1) (h2, s1)
  *** Configuring hosts
  h1 h2
  *** Starting controller

  *** Starting 1 switches
  s1 ...
  *** Waiting for switches to connect
  s1
  *** Ping: testing ping reachability
  h1 -> h2
  h2 -> h1
  *** Results: 0% dropped (2/2 received)
  *** Stopping 0 controllers

  *** Stopping 2 links
  ..
  *** Stopping 1 switches
  s1
  *** Stopping 2 hosts
  h1 h2
  *** Done
  completed in 0.154 seconds

To see more about Mininet, you can access the webpage `mininet.org
<http://mininet.org/walkthrough/>`_.


.. include:: ../back_to_list.rst

.. |kytos| replace:: *Kytos*
.. _kytos: http://docs.kytos.io/kytos

.. |venv| replace:: *Virtual Environments*
.. _venv: https://en.wikipedia.org/wiki/Virtual_environment_software

.. |virtualenv| replace:: **virtualenv**
.. _virtualenv: http://docs.python-guide.org/en/latest/dev/virtualenvs/

.. |virtualenv_docs| replace:: **virtualenv docs**
.. _virtualenv_docs: https://virtualenv.pypa.io/en/stable/
