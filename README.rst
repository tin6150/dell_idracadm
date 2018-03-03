Dell RacAdm and other bios tools (OSMA)
=======================================


This singularity container contain sys admin tools for Dell computers.

- centos 7
- dell tools installed to /opt/dell


Running this container:

::

	singularity pull --name dell_idracadm.simg shub://tin6150/dell_idracadm.img
             singularity exec    ./dell_idracadm.simg /opt/dell/srvadmin/bin/idracadm getsysinfo

        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  get BIOS.ProcSettings.LogicalProc 
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  set BIOS.ProcSettings.LogicalProc Disabled
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  jobqueue create BIOS.Setup.1-1
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  serveraction powercycle
    	The -w option may not always be needed, but racadm sometime need to write something somewhere :(  YMMV.


        sudo singularity exec ./dell_idracadm.simg /usr/bin/ipmitool raw 0x30 0xc8 0x01 0x00 0x0b 0x00 0x00 0x00 | singularity exec ./dell_idracadm.simg /usr/bin/xxd -r 
	# get chassis serial from a C6320 node, write should not be needed.  eg output:
	# 00 0b 00 00 00 0b 00 11 07 33 4a 53 47 47 4b 32
	# 3JRKGK2




Note that dell_rbu with DKS shows error during build process, as kernel source is not included.  
But the racadm and idracadm would work if matching hardware level is found.


Building the container:
ie, not using Singularity Hub, cuz need to create writable image (a la singularity < 2.4):
Actually, may not need writable image, just need to run as -B /var/run where writing (lock?) takes place.
But singularity-hub version don't produce working racadm, cuz not correctly symlinked when build on machine w/o iDRAC ?

So, for now, this recipe need to be executed locally.
This has produced working container in regular use for racadm to make bios settings
- /global/home/users/tin/sn-gh/dell_idracadm/dell_idracadm.img
- /global/scratch/tin/singularity-repo/dell_idracadm.img


::

	# Singularity 2.3.x
        Singularity=/opt/singularity/2.3.2/bin/singularity       
        sudo $Singularity create --size 1600 dell_idracadm.img
        sudo $Singularity bootstrap dell_idracadm.img  Singularity | tee sing.log 2>&1 

	# Singularity 2.4
        Singularity=/bin/singularity       
        sudo $Singularity image.create --size 2300 dell_idracadm.img
        singularity build dell_idracadm.img Singularity | tee sing.log 2>&1 

	# think build this on a lenovo laptop...
	# racadm was still okay.  so why did singularity-hub build result in non working img? 

  
Ref:

- https://github.com/tin6150/dell_idracadm
- https://singularity-hub.org/collections/390/
- https://github.com/tin6150/singhub/centos7_racadm.def



PS:

- This is not official release from Dell, I am not affiliated with them.
- I am just a sys admin trying to get things to work.  Hopefully this is useful to you too.  
- The proof is left as an exercise to the reader.  Use at your own risk.  :)

Tin
