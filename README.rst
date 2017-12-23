Dell RacAdm and other bios tools (OSMA)
=======================================


This singularity container contain sys admin tools for Dell computers.

- centos 7
- dell tools installed to /opt/dell


Getting and running this container:

::

	singularity pull shub://tin6150/dell_idracadm.img
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/bin/idracadm getsysinfo
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  get BIOS.ProcSettings.LogicalProc 
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  set BIOS.ProcSettings.LogicalProc Disabled
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  jobqueue create BIOS.Setup.1-1
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  serveraction powercycle
    	The -w option may not always be needed, but racadm sometime need to write something somewhere :(  YMMV.



Note that dell_rbu with DKS shows error during build process, as kernel source is not included.  
But the racadm and idracadm would work if matching hardware level is found.


Creating containers (if not using Singularity Hub):

::

        Singularity=/opt/singularity/2.3.2/bin/singularity       
        sudo $Singularity create --size 1600 dell_idracadm.img
        sudo $Singularity bootstrap dell_idracadm.img  Singularity | tee sing.log 2>&1 

  
Ref:

- https://github.com/tin6150/dell_idracadm
- https://singularity-hub.org/collections/304/
- https://github.com/tin6150/singhub/centos7_racadm.def



PS:

- This is not official release from Dell, I am not affiliated with them.
- I am just a sys admin trying to get things to work.  Hopefully this is useful to you too.  
- The proof is left as an exercise to the reader.  Use at your own risk.  :)

Tin
