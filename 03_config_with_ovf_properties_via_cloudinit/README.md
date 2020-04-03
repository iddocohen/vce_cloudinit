# Description

This example shows how to use the ``VCOVF`` based data source approach. The advantage of that data source is, it is a data source using the OVF properties, which is an easier learning curve for ESXi based users. Furthermore this type has synergies with other VMware automation products like vRealize Automation (vRA).

In the ova-env.xml already configured:
- VCO server (vco22-fra1.veloclout.net)
- Default password with Velocloud123 
- All interfaces will be using DHCP

**Please remember** to generate a valid .iso for cloud-init to understand the ``VCOVF``data source one needs to use the following:

``genisoimage -o cloud-init.iso -r ovf-env.xml``
