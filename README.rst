Dell RacAdm and other bios tools (OSMA?)
========================================


This is a proof of concept container packaging.

- centos 7
- dell tools installed to /opt/dell

Getting and running this knime container:

::

	singularity pull shub://
	--or--
	singularity pull shub://tin6150/dell_idracadm.img
        ln -s dell_idracadm.img idracadm
        sudo ./idracadm help


Creating containers (if not using Singularity Hub):

::

        Singularity=/prog/singularity/2.3/bin/singularity       
        sudo $Singularity create --size 1900 dell_idracadm.img
        sudo $Singularity bootstrap racadm.img  centos7_racadm.def | tee racadm.log 2>&1 

  
Ref:

- https://github.com/tin6150/singhub/centos7_racadm.def
- https://github.com/tin6150/dell_idracadm
- https://singularity-hub.org/collections/418/


