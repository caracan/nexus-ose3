#Overview

Running a [Nexus](http://www.sonatype.org/nexus/go/) service as a container within OSE v3 (Beta4), with a persistant volume to survive restarts. Uses the 

Currently it is configured to run in the infra labeled nodes (i.e. the master).

##OSE Environment Config

This assumes you have followed the [training](https://github.com/openshift/training/blob/master/beta-4-setup.md#using-persistent-storage-optional) in Using persistent storage. So from that the following would be configured :

	nfs-utils installed
	Firewall Configured
	SELinux Configured

1. Create another export

        mkdir -p /var/export/nexus
        chown nfsnobody:nfsnobody /var/export/nexus
        chmod 700 /var/export/nexus

2. Edit `/etc/exports` and add the following line:

        /var/export/nexus *(rw,sync,all_squash)

3. Export

        exportfs -a

Running `exportfs` you should see something similar to:

	/var/export/nexus
		<world>
	/var/export/vol1
		<world>

##Installation

1. Create the PersistantVolume:

        osc create -f persistent-volume.yaml

2. Create the PersistantVolumeClaim

        osc create -f persistent-volume-claim.yaml

3. Create the pods, service & route

        osc create -f nexus.yaml

##Running

OSE will do the running for you! First run you will have to config Gogs.

1. Head to http://nexus.cloudapps.example.com/ (That is the entry the route creates) which on first run will send you to the install page


