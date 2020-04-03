# Purpose

Inspired from our [cloud-inits for VCG](https://bitbucket.org/velocloud/deployment/src/master/vcg/), I thought it would be beneficial to do the same for VMware Edges (VCEs)..

This repository should be used as "inspiration" on how cloud-init configurations could look like for the VCEs and with ISOs which can be used immediately. 

Feel free to download and/or fork the repository and add new and exciting use-cases. 

# What is cloud-init?

Referencing the [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/)
 
> Cloud-init is the industry standard multi-distribution method for cross-platform cloud instance initialization. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations. Cloud-init will identify the cloud it is running on during boot, read any provided metadata from the cloud and initialize the system accordingly. This may involve setting up the network and storage devices to configuring SSH access key and many other aspects of a system. Later on the cloud-init will also parse and process any optional user or vendor data that was passed to the instance.

That is what we are using with  KVM and OVA based VCE images, which gives the user the power to do everything needed to configure the VCE initially so it can then be provisioned via our orchestrator. 

# The way of how the cloud-init operates on our virtual VCE

Since version 3.3.2, the virtual VCE will mandate to "feed it" with a valid cloud-init [data source](https://cloudinit.readthedocs.io/en/latest/topics/data sources.html) so it can initially spun-up on a compute host. 

Cloud-init is configured to use the following data sources: ``NoCloud``, ``ConfigDrive``, ``OpenStack``, `Àzure``, `Èc2``and ``VCOVF``

For those who have notice, ``VCOVF`` is not in the online documentation as it is an in house build data source and uses the ``ÒVF`` data source but with more functionalities for our VCE use-case. 

At startup and availability of any of those data sources, cloud-init phases the content and uses modules (aka scripts) to alter the configuration of the VCE based on the phased data. Those modules do not change based on the data sources used but cloud-init ensures that the phased data works with any module correctly.

This means because we are so flexible of how many data sources we support, it gives one the choice what to use and hence there are several permutation to send data to the VCE to archive the same goal of configuration but there is no absolute right way to do it. 

Please note today this repository covers only ``NoCloud`` (user-data/meta-data) and ``VCOVF``(ovf-env.xml) based approaches; nevertheless, I might include others in the future.

# Usage

Go to the either folder and either use the existing .iso or generate a new one based on your use-case.

**Please note** Best practice is to upload the .iso to the data storage where the VCE gets spun-up at. Trying to mount the .iso locally and use something like e.g. VMware Fusion to install the VCE remotely with .iso attached locally will fail.  

# Generating new ISO for VCE

Simplest way is to use ``mkisofs``/``geniosimage`` commands to create a comptabile ISO for the VCE. This can be done under MacOS or Linux based machines (sorry Windows users).

For MacOS users wondering "how to can I use the below genisoimage commands, when I only have mkisofs?!":

```sh
brew install cdrtools
ln -s /usr/local/bin/mkisofs /usr/local/bin/genisoimage
```

Then please ``cd <folder>``to execute the below commands. 

To generate transport cloud-init ISOs with user-data and meta-data based files use: 

`` genisoimage -output cloud-init.iso -volid cidata -joliet -rock user-data meta-data

To generate transport cloud-init ISOs with OVF properties environment (aka ovf-env.xml) based configuration use:

`` genisoimage -o cloud-init.iso -r ovf-env.xml``

In each folder there is already generated .iso for the given example file/files.

# Licence

MIT, see ``LICENSE``

