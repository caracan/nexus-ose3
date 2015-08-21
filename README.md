#Overview

Running a [Nexus](http://www.sonatype.org/nexus/go/) service as a container within OSE v3, with a persistent volume to survive restarts. Uses the sonatype/nexus container.

##Environment Config

This assumes you have followed the [training](https://github.com/openshift/training/blob/master/beta-4-setup.md#using-persistent-storage-optional) in Using persistent storage. So from that the following would be configured :

	nfs-utils installed
	Firewall Configured
	SELinux Configured

1. Create another export

        mkdir -p /var/export/nexus
        chown nfsnobody:nfsnobody /var/export/nexus
        chmod 700 /var/export/nexus

1. Edit `/etc/exports` and add the following line:

        /var/export/nexus *(rw,sync,all_squash)

1. Export

        exportfs -a

Running `exportfs` you should see something similar to:

	/var/export/nexus
		<world>
	/var/export/vol1
		<world>

##Installation

This assumes you are running as the admin user. You can run as a non-admin user, but for this example creating the Project and the PersistentVolume still needs to be done as the administrator. We will create a separate project to run Nexus in and target it to nodes labelled "region=infra" in.

1. Create the project

				 oadm new-project infra --node-selector="region=infra" --description="Project for running infrastructure services"

1. Create the PersistentVolume:

        osc create -f persistent-volume.yaml

1. Create the PersistentVolumeClaim

        osc create -f persistent-volume-claim.yaml

1. Create the pods, service & route

        osc create -f nexus.yaml

##Running

OSE will do the running for you! First run you will have to configure Nexus.

1. Head to http://nexus.cloudapps.example.com/ (That is the entry the route creates) which on first run will send you to the install page
