.. _era_mssql:

--------------
Era Deployment
--------------

The below will guide you through the necessary steps to initially deploy and configure Era..

**Expected Module Duration:** 90 minutes


Installation
++++++++++++


#. Select :fa:`bars` **Virtual Infrastructure > VMs**. and then click **Create VM**.

   Enter the following in the indicated fields.

   - **Name** - Era
   - **Description** - (Optional) Description for your VM.
   - **vCPU(s)** - 4
   - **Number of Cores per vCPU** - 1
   - **Memory** - 16 GiB

   - Select **+ Add New Disk**
       - **Type** - DISK
       - **Operation** - Clone from Image Service
       - **Image** - ERA-Server-build-#.#.#.qcow2 (# = Version Number)
       - Select **Add**

   - Select **Add New NIC**
       - **VLAN Name** - Primary
       - Select **Add**

#. Click **Save** to create the VM.

#. (Optional) If you want to assign a static IP address to the Era VM, perform the following:

   - Select the **Custom Script** check box.

   - In the **Type or paste script** text box, paste the following script, and replace the indicated references (ex. <NETMASK-IP> would be 255.255.255.128).

      .. code-block:: bash

         #cloud-config
         runcmd:
          - configure_static_ip ip=<STATIC-IP-ADDRESS> gateway=<GATEWAY-ADDRESS> netmask=<NETMASK-IP> nameserver=<NAMESERVER>

      All parameters except the *nameserver* parameter are mandatory.

#. Click the **Save** button to create the VM.

#.  **Power On** the VM.

#. If you did not set a static IP, determine the IP address assigned to the Era VM from the *IP Addresses* field.

   .. note::

      If you assigned a static IP address to the Era VM on a VLAN that has a DHCP server (ex. the *Primary* VLAN on the HPOC), Prism Element first assigns an IP address to the Era VM by using DHCP. Wait for one or two minutes and refresh the Prism Element page to verify if the static IP address you specified has been assigned to the VM.

Configuration
+++++++++++++

#. In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs > List**.

#. Identify the IP address assigned to the **Era** VM using the **IP Addresses** column.

#. Open \https://*ERA-VM-IP:8443*/ in a new browser tab.

#. Read the **Nutanix End User License Agreement (EULA) agreement**, click the **I have read and agree to terms and conditions option**, and then click **Continue**.

#. In the **Nutanix Customer Experience Program** screen, click **OK**.

#. When Prompted on the logon screen, set the cluster password as the password for the administrator user (admin), and click **Set Password**.

   - **Password** - *<Cluster Password>*

#. In the **Era’s Cluster** screen, enter the following in the indicated fields:

   - **Name** - EraCluster
   - **Description** - (Optional) Type a description of the Nutanix cluster.
   - **Address** - Prism Element VIP
   - **Prism Element Administrator** - admin
   - **Password** - *<Cluster Password>*

   .. note::

     It is not best practice to use the default administrative account for Era operations. In a production environment, it is therefore recommended to use a separate Prism Element user account with Nutanix cluster administrative privileges as Era service account.

#. Click **Next**

      .. figure:: images/era1.png

#. (Optional) Configure the SMTP server.

    If you do not configure this, remove the e-mail address listed within the *Sender's Email* box.

#. In the **Era Server's OS Time Zone** list, select a timezone, or leave the default UTC.

   .. figure:: images/era2.png

#. Click **Next**. This will validate your settings.

   .. figure:: images/era3.png

#. In the **Storage Container** screen, select the storage container that you want Era to use to provision new databases and database servers, and click **Next**.

   - **Storage Container** - Era

   .. figure:: images/era4.png

#. In the **Network Profile** screen, within the *VLAN* section, select the **Primary** VLAN from the drop-down list, and click **Next**.

   Do NOT check the Manage IP Address Pool

   .. figure:: images/era5.png

#. Click **Get Started**.

    The **Getting Started** page describes how to register and provision databases in Era. You can also open the main menu and start using the product.

   .. figure:: images/era6.png

#. In the **Getting Started** screen, select the **Yes** button.

   .. figure:: images/era7.png

Windows Domain Configuration
............................

#. From the dropdown, choose **Profiles**.

#. Select **Windows Domain** from the left-hand menu.

#. Click :fa:`plus` **Create**.

#. In the **Create Windows Domain Profile** screen, enter the following in the indicated fields:

   - **Name** NTNXLAB
   - **Domain to Join (FQDN)** ntnxlab.local

#. In the **Domain Account with Permission to Join Computer to the Domain** section, enter the following in the indicated fields:

   - **Username** ntnxlab.local\\administrators
   - **Password** nutanix/4u

#. In the **SQL Service Startup Account** section, deselect **Specify Startup Account in Profile**.

#. In the **Era Worker Service Account** section, enter the following in the indicated fields:

   - **Username** ntnxlab.local\\administrators
   - **Password** nutanix/4u

   .. figure:: images/era15.png

#. Click **Create**.

(Optional) Configure UI Timeout
....................

#. Click on the **admin** dropdown at the top right, and choose **Profile**.

#. Set the **Timeout** setting to **Never**. This will help avoid being logged out unexpectedly during your POC.

#. Click **Save**.

Modifying Era VM Network Settings Post-Launch
.............................................

.. note::

   These instructions are taken from the *Assigning A Static IP Address To The Era VM By Using The Console* section of the Era Guide. However, you may utilize any or all of the parameters for the `era-server set` command to accomplish your goal. For example, if you only need to modify the name server that the Era VM is using, you would type `era_server set nameserver=<NAMESERVER-IP>`.

#. Within Prism, right click the Era VM, and click **Launch Console**

#. Use the following credentials to log on to Era:

   - **User name**: era
   - **Password**: Nutanix.1

#. Launch the Era server prompt by typing `era-server`.

#. The full command is `era_server set ip=<IP-address> gateway=<GATEWAY-ADDRESS> netmask=<NETMASK-IP> nameserver=<NAMESERVER>`
