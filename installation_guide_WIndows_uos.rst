Install UserOS with Windows 10 
###############################################################

 This doument only contains the installation guide of UOS with Windows 10.

 The installation guide of the ServiceOS and RTOS_ubuntu please refer to the file **installation_guide.rst**.


.. contents::
   :local:
   :depth: 2

Verified version
****************

- Ubuntu version: **18.04**
- GCC version: **7.5.0**
- ACRN-hypervisor branch: **v2.1**
- ACRN-Kernel (Service VM kernel): **v2.1**
- RT kernel for Ubuntu User VM OS: **Linux kernel 4.19.59 with Xenomai 3.1**
- HW: `ROScube-I`_

  ADLINK `ROScube-I`_ is a real-time `ROS 2`_-enabled robotic controller based
  on Intel® Xeon® 9th Gen Intel® Core™ i7/i3 and 8th Gen Intel® Core™ i5
  processors. It features comprehensive I/O connectivity supporting a wide
  variety of sensors and actuators for unlimited robotic applications.

.. _ROScube-I:
   https://www.adlinktech.com/Products/ROS2_Solution/ROS2_Controller/ROScube-I?lang=en

.. _ROS 2:
   https://index.ros.org/doc/ros2/


Install User VM Windows
***********************

Before create User VM with Windows
==================================

#. Download `Windows 10 Disc Image (ISO File)`_

   - Select ISO - LTSC and click Continue.
   - Complete the form and click Continue.
   - Select 64 bit platform and language and click Download.
   - Save image name or rename as windows10-LTSC-<build version>.iso. E.g. windows10-LTSC-19042.iso

   In the following steps will take build version 19042 "windows10-LTSC-19042.iso" as example. Please rename it, if you are using different version, or different image name.

#. Download `Oracle Windows driver`_

   - Sign in. If you do not have an Oracle account, register for one.
   - Select Download Package. Key in Oracle Linux 7.6 and click Search.
   - Click DLP: Oracle Linux 7.6 to add to your Cart.
   - Click Checkout which is located at the top-right corner.
   - Under Platforms/Language, select x86 64 bit. Click Continue.
   - Check I accept the terms in the license agreement. Click Continue.
   - From the list, right check the item labeled Oracle VirtIO Drivers Version for Microsoft Windows 1.x.x, yy MB, and then Save link as …. Currently, it is named V982789-01.zip.
   - Click Download. When the download is complete, unzip the file. You will see an ISO named winvirtio.iso.

.. _Windows 10 Disc Image (ISO File):
   https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise

.. _Oracle Windows driver:
   https://edelivery.oracle.com/osdc/faces/Home.jspx

Create User VM image Windows10
==============================

.. note:: Reboot into the **native Linux kernel** (not the ACRN kernel)
   and create User VM image.

#. Create a win10 uos workspace.

   .. code-block:: bash

      mkdir -p ~/acrn/uosWinVM
      cd ~/acrn/uosWinVM

   Put image file "**windows10-LTSC-19042.iso**" and "**winvirtio.iso**" here.


#. Start virtual machine manager application.

   .. code-block:: bash

     sudo virt-manager

#. Create a new virtual machine.

   .. figure:: images/rqi-acrn-kvm-new-vm.png

#. Select your ISO image path.

   .. figure:: images/rqi-acrn-kvm-choose-iso_win.png

#. Select CPU and RAM for the VM.  

   .. figure:: images/rqi-acrn-kvm-cpu-ram-win.png

   Modify CPUs and RAM resources as high as you can, this will help you reduce the installation time.
   The configuration of the number of CPU or the amount of RAM resources will not hook up with ACRN resources distribution.

#. Select disk size you want. **Note that this can't be modified after creating image!**

   .. figure:: images/rqi-acrn-kvm-storage-win.png

   Modify disk image size you want, then forward. Recommend at least 50 GiB in windows environment will be good.
   **The configuration of disk image size unlike CPU, RAM or others passthrough devices can modify dynamically during ACRN launch stage. You have to make decision in this stage.**


#. Edit image name and select "**Customize configuration before install**".

   .. figure:: images/rqi-acrn-kvm-name-win.png

#. Firmware setting.

   #. Select **UEFI x86_64...OVMF/OVMF_CODE.fd** in firmware property, apply it.
   #. **Apply** it before you do next step.
   #. Select **Add Hardware**.
 
      .. figure:: images/rqi-acrn-kvm-firmware-win-1.png

   #. Click **Select or create custom storage**.
   #. Click **Manage**.
   
      .. figure:: images/rqi-acrn-kvm-firmware-win-2.png

   #. Find **winvirtio.iso** image.

      .. figure:: images/rqi-acrn-kvm-firmware-win-3.png
   
   #. Select **CDROM device** in device type, then click **Finish**.
   
      .. figure:: images/rqi-acrn-kvm-firmware-win-4.png

   #. Click **Begin installation**, now we are ready to install Win10 OS.
   
      .. figure:: images/rqi-acrn-kvm-firmware-win-5.png

#. Install Windows10

   #. Press **Enter** and continue.

      .. figure:: images/rqi-acrn-kvm-install-win-1.png

   #. Enter **exit** in UEFI shell screen.

      .. figure:: images/rqi-acrn-kvm-install-win-2.png

   #. Select **Boot Manager**.
   
      .. figure:: images/rqi-acrn-kvm-install-win-3.png

   #. Select **UEFI QEMU DVD-ROM**.

      .. figure:: images/rqi-acrn-kvm-install-win-4.png

   #. Press **Enter** and continue.
   
      .. figure:: images/rqi-acrn-kvm-install-win-1.png

   #. Customized to your preference.
   
      .. figure:: images/rqi-acrn-kvm-install-win-5.png

      .. figure:: images/rqi-acrn-kvm-install-win-6.png

      .. figure:: images/rqi-acrn-kvm-install-win-7.png

   #. Select **Custom: Install Windows only (advanced)**.
   
      .. figure:: images/rqi-acrn-kvm-install-win-8.png

#. Load Oracle Windows driver.

   #. Click **Load driver** and install some of virtual I/O driver.
   
      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-1.png
   
   #. Click **Browse**.
   
      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-2.png
   
   #. Find the folder **amd64** in **Win10** folder in **CDROM**.
   
      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-3.png
   
   #. Disable the ckeckbox **Hide drivers that aren't compatible with this computer's hardware**.

      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-4.png

   #. Select the following VirtIO driver with Ctrl+<mouse left-click button> to select mutiple items.
      
      - Oracle VirtIO Ballon Driver
      - Oracle VirtIO Ethernet Adapter
      - Oracle VirtIO Input Driver
      - Oracle VirtIO RNG Device
      - Oracle VirtIO SCSI controller
      - Oracle VirtIO SCSI pass-through controller
      - Oracle VirtIO Serial Driver
      
      Then click **Next** and continue.

      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-5.png
   
   #. Click **Next**.

      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-6.png

   #. Waiting Windows Setup UI finish the installation, however the system will reboot automatically during the procedures. Please follow the instructions to complete the installation.
      
      .. figure:: images/rqi-acrn-kvm-install-win-oracle-driver-7.png
   
#. Finish installing Windows10. Log-in to Windows and shutdown.

   .. figure:: images/rqi-acrn-kvm-install-win-finish.png

**Now we are ready to convert image as ACRN readable type, and launch through ACRN VMM.**

Launch User VM Windows
************************

Convert Image File Format
=========================

#. If you finished the steps of **ACRN UOS Win10 Image**, you should have the following workspace, or find the one you customized it before.

   .. code-block:: bash

      cd ~/acrn/uosWinVM

#. In ACRN UOS Win10 Image image creation, the name of win10-ltsc is our target, convert image which is specified to the path of your workspace.
   
   .. code-block:: bash

      sudo qemu-img convert -f qcow2 -O raw /var/lib/libvirt/images/<your image name>.qcow2 ./<your image name>.img

  for example:

   .. code-block:: bash

      sudo qemu-img convert -f qcow2 -O raw /var/lib/libvirt/images/win10-ltsc.qcow2 ./win10-ltsc.img

Prepare a Launch Script File for Windows-UOS
============================================

#. Create a winUOS launch file.

   .. code-block:: bash

      touch launch_win_uos.sh
      chmod +x ./launch_win_uos.sh

#. Edit launch file.

   .. code-block:: bash

      gedit launch_win_uos.sh
   
#. Copy and paste the next code block contents to the launch file and save it.

   .. code-block:: bash

      #!/bin/bash
      # board: ROS-CUBE-CFL, scenario: INDUSTRY_ROS2SYSTEMOS, uos: WIN10
      # pci devices for passthru
      declare -A passthru_vpid
      declare -A passthru_bdf

      passthru_vpid=(
      ["gpu"]="8086 3e9b"
      )
      passthru_bdf=(
      ["gpu"]="0000:00:02.0"
      )

      function tap_net() {
      # create a unique tap device for each VM
      tap=$1
      tap_exist=$(ip a | grep "$tap" | awk '{print $1}')
      if [ "$tap_exist"x != "x" ]; then
        echo "tap device existed, reuse $tap"
      else
        ip tuntap add dev $tap mode tap
      fi

      # if acrn-br0 exists, add VM's unique tap device under it
      br_exist=$(ip a | grep acrn-br0 | awk '{print $1}')
      if [ "$br_exist"x != "x" -a "$tap_exist"x = "x" ]; then
        echo "acrn-br0 bridge aleady exists, adding new tap device to it..."
        ip link set "$tap" master acrn-br0
        ip link set dev "$tap" down
        ip link set dev "$tap" up
      fi
      }

      function launch_win() {
      #vm-name used to generate uos-mac address
      mac=$(cat /sys/class/net/e*/address)
      vm_name=win_vm$1
      mac_seed=${mac:0:17}-${vm_name}

      #check if the vm is running or not
      vm_ps=$(pgrep -a -f acrn-dm)
      result=$(echo $vm_ps | grep "${vm_name}")
      if [[ "$result" != "" ]]; then
        echo "$vm_name is running, can't create twice!"
        exit
      fi

      modprobe pci_stub
      echo ${passthru_vpid["gpu"]} > /sys/bus/pci/drivers/pci-stub/new_id
      echo ${passthru_bdf["gpu"]} > /sys/bus/pci/devices/${passthru_bdf["gpu"]}/driver/unbind
      echo ${passthru_bdf["gpu"]} > /sys/bus/pci/drivers/pci-stub/bind

      #interrupt storm monitor for pass-through devices, params order:
      #threshold/s,probe-period(s),intr-inject-delay-time(ms),delay-duration(ms)
      intr_storm_monitor="--intr_monitor 10000,10,1,100"

      #for pm by vuart setting
      pm_channel="--pm_notify_channel uart "
      pm_by_vuart="--pm_by_vuart pty,/run/acrn/life_mngr_"$vm_name
      pm_vuart_node=" -s 1:0,lpc -l com2,/run/acrn/life_mngr_"$vm_name

      # for virt net setting
      tap_id=tap_win_vm$1
      tap_net ${tap_id}

      #for memsize setting
      mem_size=8192M

      acrn-dm -A -m $mem_size -s 0:0,hostbridge -U d2795438-25d6-11e8-864e-cb7a18b34643 \
        --mac_seed $mac_seed \
        -s 2,passthru,00/02/0,gpu \
        -s 3,virtio-blk,./win10-ltsc.img \
        -s 4,virtio-hyper_dmabuf \
        -s 5,virtio-rnd \
        -s 6,virtio-net,${tap_id} \
        -s 7,xhci,1-1:1-2 \
        --cpu_affinity 1,2,3 \
        --ovmf ./OVMF_GOP.fd \
        --windows \
        $pm_channel $pm_by_vuart $pm_vuart_node \
        $intr_storm_monitor \
        $vm_name

      }

      # offline SOS CPUs except BSP before launch UOS
      for i in `ls -d /sys/devices/system/cpu/cpu[1-99]`; do
              online=`cat $i/online`
              idx=`echo $i | tr -cd "[1-99]"`
              echo cpu$idx online=$online
              if [ "$online" = "1" ]; then
                      echo 0 > $i/online
                      echo $idx > /sys/class/vhm/acrn_vhm/offline_cpu
              fi
      done

      launch_win 1 "64 448 8"

Hardware Resources Distribution
===============================

Most of resources distribution rules are the same as **ACRN UOS Ubuntu Launch and Guide**, please refer to it for details. We are not to do the same description here.

*Important Notice*
=====================

**Close or exit terminal will not terminate the VM when launching VM successful. You need to run "poweroff" or "shutdown now -h" command in the VM.**


Prepare an OVMF file
====================

#. Put **ROScube-I_OVMF.zip** in uosWinVM workspace, then extract files.

   .. code-block:: bash

      cp ./ROScube-I_OVMF.zip ~/acrn/uosWinVM
      unzip ROScube-I_OVMF.zip

   - *Remarks*: **ROScube-I_OVMF.zip** doesn't exist in this Repo. If you need this file, please contact ADLINK.

#. Return **OK** means the file was not broken.

   .. code-block:: bash

      md5sum -c OVMF_GOP.md5sum

SOS Configuration for WinVM
===========================

Enable WinVM
------------

**Enable the following configurations will make SOS VM display no longer available. Using SSH remote log-in only.**

#. Adding the following settings.

   .. code-block:: bash

      sudo sed -i '/multiboot2/s/$/ i915.modeset=0 video=efifb:off/' /etc/grub.d/40_custom
      sudo sed -i '/echo/c\  echo "WinVM service is enabled! Please access ACRN SOS through ssh only."' /etc/grub.d/40_custom

#. Update grub menu and reboot SOS.

   .. code-block:: bash

      sudo update-grub
      sudo reboot

Disable WinVM
-------------

Do the following steps if you decide to disable winVM.

#. Log-in SOS through ssh and remove the following settings.

   .. code-block:: bash

      sudo sed -r -i 's/\b( i915.modeset=0| video=efifb:off)\b//g' /etc/grub.d/40_custom
      sudo sed -i '/echo/c\  echo "Loading ACRN SOS..."' /etc/grub.d/40_custom

#. Update grub menu and reboot SOS.

   .. code-block:: bash

      sudo update-grub
      sudo reboot

*Remarks:* **In the meantime, you must not launch winVM script after disable it; however, that system will crash, once you try to run it.**

Launch the WinVM
================

**Enable winVM above first**. Now you notice the display desktop UI is no longer available. There are two methods to launch WinVM.

#.	Remote access target IPC through ssh.

#.
   
   .. code-block:: bash

      cd ~/acrn/uosWinVM	
      sudo ./launch_win_uos.sh

#.	Setting as **Runascriptonstartup**.

 This step only recommend when the system environments setup are ready, which means the launch file is no longer need to be modified.

Install Graphics Driver
=======================

Open any one of browser, then goolge Intel DCH Driver, download and install it.

`Intel® Graphics - Windows® 10 DCH Drivers`_

.. _Intel® Graphics - Windows® 10 DCH Drivers: 
   https://downloadcenter.intel.com/download/29988/Intel-Graphics-Windows-10-DCH-Drivers?v=t


Done installing
===============
Please continue the installation part in the file **installation_guide.rst**.