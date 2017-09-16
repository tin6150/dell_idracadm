Dell RacAdm and other bios tools (OSMA)
=======================================


This is a proof of concept container packaging.

- centos 7
- dell tools installed to /opt/dell


Getting and running this knime container:

::

	singularity pull shub://tin6150/dell_idracadm.img
        ln -s dell_idracadm.img idracadm
        sudo ./idracadm help
        sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/bin/idracadm getsysinfo
        # stupid command need write access somewhere, can't run in read-only container :(

Creating containers (if not using Singularity Hub):

::

        Singularity=/opt/singularity/2.3.2/bin/singularity       
        sudo $Singularity create --size 1900 dell_idracadm.img
        sudo $Singularity bootstrap dell_idracadm.img  Singularity | tee sing.log 2>&1 

  
Ref:

- https://github.com/tin6150/singhub/centos7_racadm.def
- https://github.com/tin6150/dell_idracadm
- https://singularity-hub.org/collections/450/



PS:

- This is not official release from Dell, I am not affiliated with them.
- I am just a sys admin trying to get things to work.  Hopefully this is useful to you too.  
- The proof is left as an exercise to the reader.  Use at your own risk.  :)

Tin
