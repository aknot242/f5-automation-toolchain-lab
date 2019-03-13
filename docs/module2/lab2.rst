Module |labmodule|\, Lab \ |labnum|\: F5 Declarative Onboarding
===============================================================

|image1| **Declarative Onboarding**

Task |labmodule|\.\ |labnum|\.1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using `Chrome` open a tab to each BIG-IP system, they are currently in a default configuration. These units have a management IP address and are sitting at the Setup Utility ready to accept configuration. 

.. Note:: This would be the configuration of a new BIG-IP (Virtual or Hardware).

BIG-IP 1 and 2:
Username: ``admin``
Password: ``admin``

  |image8|

Leave the tabs open in Chrome for later.

Task |labmodule|\.\ |labnum|\.2
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

F5 Declarative Onboarding is an iControlLX_ package, which needs to be installed on the BIG-IP or API Services Gateway. After DO is installed in a location we will interact with it through a REST verb to configure our BIG-IP(s).

.. seealso:: The iControl® LX extension allows you to use Node.js to extend the REST API on any BIG-IP. You can write an iControl LX extension to implement your REST API using JavaScript to represent the URI resources (nouns) that you can then invoke in a RESTful manner. The REST verb handlers can then perform appropriate actions local to the F5 devices or across the distributed data center. An iControl LX extension is an extensibility point attached to a specific URI, enabling customer-provided JavaScript/Node.js code to run in the context of the BIG-IP/iWorkflow control plane extending the REST API with additional services. You can extend existing F5 REST APIs as well as convert your own services into multiple extensions that are run on F5’s control plane.

Expand the `Module 2 - DO and BIG-IP` tab within the collection and execute `Step 1: Get Installed iControl LX Extensions BIGIP1`. This step will request the icontrollx packages already installed on the BIG-IP

  |image10|

The response of the currently installed packages:

  |image11|

Task |labmodule|\.\ |labnum|\.3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An icontrollx package can be installed via the BIG-IP API, or through the GUI, we are going to install this through the API with `Step 2: Upload DO Extension RPM BIGIP1`.

Installing the rpm package through the API with Postman requires a few variables in the collection which have already been set on your behalf (size of package and name). However you will need to select the package to be installed, these have been downloaded for you and are in the `Downloads` folder of your jump host.

Select the Declarative Onboarding rpm file for this Step

  |image13|

Execute the step to upload the package to the BIG-IP

  |image14|

Task |labmodule|\.\ |labnum|\.4
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the rpm package is installed on the BIGIP1 we will need to tell the BIG-IP to install the package so we can make use of it.

Execute the following steps viewing the response pages and the status of installing the DO package.

Step 3: Create DO Extension Install Task BIGIP1:

  |image15|

Step 4: Get DO Install Task Status BIGIP1:

  |image16|

Step 5: Check DO Installed BIGIP1:

  |image17|

.. Note:: Declarative Onboarding status installed and ready with no configuration will show an empty response, like the picture above

Task |labmodule|\.\ |labnum|\.5
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With the DO package installed on the BIGIP1 we are ready to build out our first BIG-IP. 

The desired end state of these DO configurations is to configure the below objects, built on the BIG-IPs with a single call in a single file. This declarative solution allows me to compose configurations that are reusable with templating technologies and storeable in Source Control.

Configuration Items in our declaration:
  - Licensing
  - Credentials
  - Provisioning
  - DNS
  - NTP
  - Self-IPs
  - Vlans
  - Clustering

Our declaration for BIGIP1:

.. literalinclude :: files/do_cluster_bigip1.json
   :language: json

Copy **all of** the BIGIP1 declaration

Task |labmodule|\.\ |labnum|\.6
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

F5 publishs a schema for each of the Automation Toolchain items, this published schema can be read with Visual Studio Code allowing you to see context and find errors within your different declarations. The schema referece is added at the top of your declartion, and requires vscode to know the language is JSON.

.. seealso:: Schema Validation for Declarative Onboarding (DO_Schema_)

Open `Visual Studio Code` on your jump host desktop and open a `New File` (shortcut Ctrl+n) and paste in all of the BIGIP1 declaration contents, then set the language to `json`.

  |image18|

Once the declaration and language are set you can highlight over sections of the code to see context and errors.

.. note:: You can try misspelling some of the declaration to see the errors, just remember to revert your changes.

  |image19|

Task |labmodule|\.\ |labnum|\.7
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We now need to send our declaration to BIGIP1, this will be the first member of our cluster and the one our second BIG-IP will pull its configuration. 

Click on step `Step 6: DO Declaration BIGIP1`, navigate to the `Body` tab and paste in all of your declaration, and send the call. 

.. note:: You can leave the schema validation line, the BIG-IP will ignore it.

  |image20|

The declaration is now on BIGIP1 and being processed, this will take a few seconds to process and build out our objects. 

Task |labmodule|\.\ |labnum|\.8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Return to your BIGIP1 gui in Chrome, you can now navigate around the UI and see the objects in our declaration have been created.

  |image21|

Task |labmodule|\.\ |labnum|\.9
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Building out BIGIP2 is going to be similar, however the declaration is slightly different, BIGIP2 will have some device specific items, like Self-IPs.

.. Note:: iControlLX packages are device configuration objects that will sync across clustered BIG-IP, however since we are creating our cluster with DO we will need to install it on BIGIP2 as well.

These declarations are similar but slightly different, they are a perfect use-case for a templating technology. We could have used Parameters in Postman or other templating tools that an Orchestrator may provide (like Jinja2 in Ansible).

Progress through **Steps 7-11**, remember to select your file for the upload step.

Task |labmodule|\.\ |labnum|\.10
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The last step of Module 2 is to place our declaration on BIGIP2, this declaration will provide the needed pieces to create our cluster.

  |image22|

.. Note:: Clustering can take a couple minutes to sync and establish, this is normal

Return to either BIG-IP in Chrome and check the cluster configuration and status.

.. warning:: You may need to refresh the BIG-IP gui to see the changes

  |image23|

This concludes Module 2 and onboarding your BIG-IP with F5 Declarative Onboarding

.. |labmodule| replace:: 2
.. |labnum| replace:: 2
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|
.. |image1| image:: images/image1.png
   :width: 50px
.. |image8| image:: images/image8.png
   :width: 80%
.. |image9| image:: images/image9.png
   :width: 40%
.. |image10| image:: images/image10.png
   :width: 50%
.. |image11| image:: images/image11.png
   :width: 80%
.. |image12| image:: images/image12.png
   :width: 50%
.. |image13| image:: images/image13.png
.. |image14| image:: images/image14.png
.. |image15| image:: images/image15.png
.. |image16| image:: images/image16.png
.. |image17| image:: images/image17.png
.. |image18| image:: images/image18.png
.. |image19| image:: images/image19.png
.. |image20| image:: images/image20.png
.. |image21| image:: images/image21.png
.. |image22| image:: images/image22.png
.. |image23| image:: images/image23.png
.. _iControlLX: https://clouddocs.f5.com/products/iapp/iapp-lx/tmos-13_1/icontrollx_concepts/icontrollx-overview.html
.. _DO_Schema: https://clouddocs.f5.com/products/extensions/f5-declarative-onboarding/latest/validate.html