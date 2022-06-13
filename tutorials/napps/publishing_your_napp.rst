:tocdepth: 2
:orphan:

.. _tutorial-publishing_your_napp:

####################
Publishing your NApp
####################

********
Overview
********

On this tutorial you will learn how to publish your NApp to a GitHub
repository in addition to that, share you NApp with the Kytos Community and also make
easier for you to install it on your **Kytos** instance.

The average time to go throught it is: ``5 min``

What you will need
===================

* Your |dev_env|_ already setup
* How to create a basic NApp - Refer to |Tutorial_01|_

What you will learn
===================

* How to publish your NApp on the |github|_
* How to install a NApp from |github_kytos|_

************
Introduction
************

Now that you have learned how to install the environment and how to create your
own NApp, you can publish it on |github|_.

Before proceeding to the next section of this tutorial, go to |github_signup|_ page
in order to create a github account. After you submit with the required fields, you will be asking
to verify your email with text message sent to your email, After entering the code you will be
redirected to github home page.

******************
Your NApp metadata
******************

First of all, you need to create a NApp. So, let's start creating a new NApp
using your **username** (username should be same as your github account username).

.. code-block:: console

  $ mkdir -p ~/tutorials
  $ cd ~/tutorials
  $ kytos napps create
  --------------------------------------------------------------
  Welcome to the bootstrap process of your NApp.
  --------------------------------------------------------------
  In order to answer both the username and the napp name,
  You must follow this naming rules:
   - name starts with a letter
   - name contains only letters, numbers or underscores
   - at least three characters
  --------------------------------------------------------------

The first question is related to your **username** (username should be same as your github account username).
The second question is the name of your NApp, and, for now,
we will just use ``my_first_napp`` as NApp name. The third question is related
to your NApp description. Let's put some meaningful information over there.

.. code:: console

  Please, insert your NApps Server username: <username>
  Please, insert your NApp name: my_first_napp
  Please, insert a brief description for your NApp [optional]: This is my first NApp, I have built it while doing a Kytos Tutorial.

The NApps contains two important metadata files. The first one is the
**kytos.json**, that will be used by the uploader. The second one is the
**README.rst**. It is pretty important that you take some time on them since
this information will be used by other users to find your NApp.

We are going to start with the **kytos.json** file.

kytos.json
==========

The **kytos.json** file contains the fields *username*, *name* (NApp name),
*description*, *version*, *napp_dependencies*, *license*,
*url* and *tags*. Some of these fields are mandatory, such as **username**,
**name** and **license**, while other aren't, despite them being as important
as the mandatory ones. They also accept different values.

- **username**: String with the username of the NApp creator (required GitHub account username). While publishing your
  NApp to github it should match with your GitHub account username.

- **name**: String with the name of the NApp.

- **description**: String with a small description of the NApp. One or two
  sentences.

- **version**: String with the version of your NApp. We suggest you to use the
  `Semantic Versioning <http://semver.org/>`_.

- **napp_dependencies**: A list of other NApps that are required by your NApp,
  on the form ``["<username>/my_first_napp", "<other_username>/<other_name>", ...]``

- **license**: The license of your NApp (*GPL*, *MIT*, *APACHE*, etc).

- **url**: If your NApp source code is maintained on a public repository, here
  comes its url.

- **tags**: A list of tags to help catalog your NApp on the github on
  the form ``["tag1", "tag2", ...]``

So, here is the **kytos.json** that we have initially generated.

.. code-block:: json

  {
    "username": "<username>",
    "name": "my_first_napp",
    "description": "This is my first NApp, I have built it while doing a Kytos Tutorial.",
    "version": "",
    "napp_dependencies": [],
    "license": "",
    "tags": [],
    "url": ""
  }

And here are an example of how we can complete this file:

.. code-block:: json

  {
    "username": "<username>",
    "name": "my_first_napp",
    "description": "This is my first NApp, I have built it while doing a Kytos Tutorial.",
    "version": "0.0.1",
    "napp_dependencies": [],
    "license": "MIT",
    "tags": ["experimental", "tutorial", "trial"],
    "url": "https://github.com/"
  }

README.rst
==========

Among other things, the **README.rst** will be presented as the main content of
the NApp page on |github|_  (https://github.com/<username>/my_first_napp).

We recommend two initial sections, **Overview** and **Requirements**. The first
will contain a more complete description of your NApp, while the latter will
hold any non-NApp requirement of your NApp.

.. code-block:: rst

    Overview
    ========
    This is my first NApp, I have built it while doing the Kytos Tutorial on
    how to upload a NApp.

    This NApp, for now, is just a 'mock' NApp that does nothing. May be, on the
    future, I'll play with it while doing some more tests on how NApps work.

    Requirements
    ============
    For now this NApp does not have any external requirement beyond *Kytos*
    itself.

******************
Uploading the NApp
******************

Your NApp is now ready to be uploaded. 
Before that, you need to create a repository with NApp name on github.
After creating repository follow the below commands to initialize and push your NApp to github.

.. code-block:: console

  $ cd ~/tutorials/<username>/my_first_napp
  $ git init
  $ git remote add origin https://github.com/<username>/my_first_napp.git
  $ git add .
  $ git commit -m "<you commit message>"
  $ git push -u origin master

You will be asked to input your username and password of github, here password indicates generated token from github, View this |github_token|_ to generate your token.


  Enter username: <username>

  Enter password: <token>

After finishing the above steps your napp will be pushed to github and your will be able to view it in your repository.
Follow the instructions on this |github_topic|_ to include topic name 'kytos-napp' to your repository.


Your NApp is now uploaded. You can see your NApp on the web: https://github.com/topics/kytos-napp


************************************
Search for and Install a remote NApp
************************************

Now that you have published your NApp, you can find your NApp at |kytos_page|_, you can also look for other NApps published.

* Select the napp you want to install
* Clone the NApp you want to install by folowing below commands

.. code-block:: console

  $ git clone https://github.com/<username>/my_first_napp.git
  $ cd my_first_napp
  $ python setup.py develop


Now that your NApp is Intalled you can view in your console by using below command

.. code-block:: console

  $ kytos napps list

  Status |          NApp ID          |                     Description
  =======+===========================+======================================================
   [i-]  | kytos/of_core             | OpenFlow Core of Kytos Controller, responsible for...
   [i-]  | kytos/flow_manager        | Manage switches' flows through a REST API.
   [i-]  | kytos/of_l2ls             | An L2 learning switch application for OpenFlow swi...
   [i-]  | kytos/of_lldp             | Discovers switches and hosts in the network using ...
   [i-]  | kytos/of_stats            | Provide statistics of openflow switches.
   [ie]  | <username>/my_first_napp  | Description of your Napp
   [i-]  | kytos/topology            | Keeps track of links between hosts and switches. R...

  Status: (i)nstalled, (e)nabled


.. |Tutorial_01| replace:: *Tutorial 01*
.. _Tutorial_01: http://tutorials.kytos.io/napps/create_your_napp/

.. include:: ../back_to_list.rst

.. |dev_env| replace:: *Development Environment*
.. _dev_env: http://tutorials.kytos.io/napps/development_environment_setup/

.. |napps_server| replace:: *NApps Server*
.. _napps_server: http://napps.kytos.io

.. |napps_server_sign_up| replace:: **sign_up**
.. _napps_server_sign_up: https://napps.kytos.io/signup/

.. |mininet| replace:: *Mininet*
.. _mininet:  http://mininet.org/overview/

.. |github| replace:: *Github*
.. _github: http://github.com

.. |github_kytos| replace:: *Github Kytos*
.. _github_kytos: https://github.com/topics/kytos-napp

.. |github_signup| replace:: *Github Signup*
.. _github_signup: https://github.com/signup

.. |github_token| replace:: *Link*
.. _github_token: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token

.. |github_topic| replace:: *Link*
.. _github_topic: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics

.. |kytos_page| replace:: *Here*
.. _kytos_page: https://github.com/topics/kytos-napp

