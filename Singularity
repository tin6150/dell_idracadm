# Dell EMC OpenManage Linux Remote Access Utilities v 9.1.0-2771 in a Singularity container 
# ie this definition file to contain Dell tools racadm / idracadm / omsa / ipmi
# runs on CentOS 7

# http://www.dell.com/support/home/us/en/19/drivers/DriversDetails?productCode=poweredge-r630&driverId=49T1M


# Main motivation is to run 
# /opt/dell/srvadmin/sbin/racadm set BIOS.ProcSettings.LogicalProc Disabled
# /opt/dell/srvadmin/sbin/racadm jobqueue create BIOS.Setup.1-1
# /opt/dell/srvadmin/sbin/racadm serveraction powercycle
# but the install into warewulf vNFS didnt work, most likely cuz read-only FS.
# Thus, placing the command in a container and let it run and write (transient flags) from there.  
# This also prevent the need of production node needing to be bloated up with dell osma packages.
#
# Singularity .def file adopted from https://github.com/singularityware/singularity/blob/master/examples/centos.def
#

BootStrap:docker
From:centos:7

#BootStrap: yum
#OSVersion: 7

MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum

%runscript
    echo "example runs (need container to be writable cuz dell commands need to write somewhere transiently):"
    echo "sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/bin/idracadm getsysinfo " 
    echo "sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  get BIOS.ProcSettings.LogicalProc"
    echo "sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  set BIOS.ProcSettings.LogicalProc Disabled"
    echo "sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  jobqueue create BIOS.Setup.1-1"
    echo "sudo singularity exec -w ./dell_idracadm.img /opt/dell/srvadmin/sbin/racadm  serveraction powercycle"
    echo "The -w option may not always be needed, but racadm sometime need to write something somewhere :(  YMMV."
    ##/opt/dell/srvadmin/bin/idracadm "$@"  # dont really work, cuz need to write somewhere

%post
	yum -y install bash tar gzip bzip2 wget curl coreutils util-linux-ng which less vi kmod dmidecode libcmpiCppImpl0 openwsman-server sblim-sfcb sblim-sfcc net-snmp-utils  pciutils libxslt openssl setserial libwsman1 openwsman-client gcc
	# dmidecode is needed by dell racadm, and many more dependencies as listed 
	# kmod provides lsmod
	cd /opt
	mkdir src
	cd src
    	wget --no-verbose https://downloads.dell.com/FOLDER04651962M/1/OM-MgmtStat-Dell-Web-LX-9.1.0-2771_A00.tar.gz -O racadm.tgz
	tar xzf racadm.tgz 
	rm      racadm.tgz
	#cd linux/rac/RHEL7
	find ./linux/rac/RHEL7                 -name *rpm -exec rpm -i --nodeps {} \;
	find ./linux/bmc/ipmitool/RHEL7_x86_64 -name *rpm -exec rpm -i --nodeps {} \;
	rpm -i --nodeps ./linux/bmc/osabmcutil9g-RHEL-4.4-0.i386.rpm
	echo 'PATH=$PATH:/opt/dell/srvadmin/bin:/opt/dell/srvadmin/sbin:/opt/dell/toolkit/bin/; export PATH' > /etc/profile.d/dell_env.sh 
	chmod 664 /etc/profile.d/dell_env.sh

	# see git log b507d45 for install steps needed for prior version of OM tar file version (9.1.0)

