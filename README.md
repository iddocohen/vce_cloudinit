# Purpose

Inspired from our [cloud-inits for VCG](https://bitbucket.org/velocloud/deployment/src/master/vcg/), I thought it would be beneficial to do the same for VMware Edges (VCEs)..

This repository should be used as "inspiration" on how cloud-init configurations could look like for the VCEs and with ISOs which can be used immediately. 

Feel free to download and/or fork the repository and add new and exciting use-cases. 

# What is cloud-init?

Referencing the [cloud-init documentation](https://cloudinit.readthedocs.io/en/latest/)
 
> Cloud-init is the industry standard multi-distribution method for cross-platform cloud instance initialization. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations. Cloud-init will identify the cloud it is running on during boot, read any provided metadata from the cloud and initialize the system accordingly. This may involve setting up the network and storage devices to configuring SSH access key and many other aspects of a system. Later on the cloud-init will also parse and process any optional user or vendor data that was passed to the instance.

That is what we are using with  KVM and OVA based VCE images, which gives the user the power to do everything needed to configure the VCE initially so it can then be provisioned via our orchestrator. 

# The way of how the cloud-init operates on our virtual VCE

Since version 3.3.2, the virtual VCE will mandate to "feed it" with a valid cloud-init [data source](https://cloudinit.readthedocs.io/en/latest/topics/datasources.html) so it can initially spun-up on a compute host. 

Cloud-init is configured to use the following data sources: ``NoCloud``, ``ConfigDrive``, ``OpenStack``, ``Azure``, ``Ec2`` and ``VCOVF``

For those who have noticed, ``VCOVF`` is not publicly documented as it is an in house build data source using the ``OVF`` with more functionalities integerated. 

At startup and availability of any of those data sources, cloud-init extracting the content out of the data source and uses that within the modules (aka scripts) to alter the configuration of the VCE based. Those modules do not change based on the data source type, as cloud-init ensures that the data works with all modules.

This means because we are flexible with different data sources we support, it gives one variety choice and hence there are several permutation to send data to the VCE to archive the same goal of configuration - so no absolute right way. 

Please note today this repository covers only ``NoCloud`` (user-data/meta-data) and ``VCOVF``(ovf-env.xml) based configurations; nevertheless, I might include others in the future.

# Usage

Go to the either folder and either use the existing .iso or generate a new one based on your use-case.

When altering existing user-data/meta-data files please note, that they are YAML based and as such are sensitive to syntax. For example, do not use tabs for spaces. 

**Please note** Best practice is to upload the .iso to the data storage where the VCE gets spun-up at. Trying to mount the .iso locally and use something like e.g. VMware Fusion to install the VCE remotely with .iso attached locally will fail.  

# Generating new ISO for VCE

Simplest way is to use ``mkisofs``/``geniosimage`` commands to create a comptabile ISO for the VCE. This can be done under MacOS or Linux based machines (sorry Windows users).

For MacOS users wondering "how to can I use the below genisoimage commands, when I only have mkisofs?!":

```sh
brew install cdrtools
ln -s /usr/local/bin/mkisofs /usr/local/bin/genisoimage
```

``mkisofs`` is newer version of ``genisoimage``, hence the above soft link and the below commands work on MacOS.

Then download this repository and ``cd`` to execute the below commands in each of the folders. 

To generate transport cloud-init ISOs with user-data and meta-data based files use: 

``genisoimage -output cloud-init.iso -volid cidata -joliet -rock user-data meta-data``

To generate transport cloud-init ISOs with OVF properties environment (aka ovf-env.xml) based configuration use:

``genisoimage -o cloud-init.iso -r ovf-env.xml``

The reason we have two type of commands is, we need a ``joliet`` for a cicdata based CD-ROM for ``NoCloud`` based data source but for ``VCOVF`` we do not. 

In each folder there is already generated .iso for the given example file/files, and one can use ``isoinfo -d -i cloud-init.iso`` to get more info how the CD image is build. 

# Licence

MIT, see ``LICENSE``

