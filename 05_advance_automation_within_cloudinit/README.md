# Description

This is an advance user-data file for ``NoCloud`` data source, where a python script is executed to configure the VCE.

The following is happening:
- Creating a configuration JSON file under /tmp/environment.json on the VCE
- Creating a python script which can read that file
- Executes the file after writing both filesb

This approach is inspired from how we configure our [VCG](https://bitbucket.org/velocloud/deployment/src/master/vcg/) via cloud-init.

It should show how for example other approaches could be used to configure the VCE, e.g. disabling DPDK for lab environment purpose.

