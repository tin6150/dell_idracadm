# Singularity definition file to contain Dell RACADM 
# runs on CentOS 7

# dell_idracadm

# Early test
# main motivation is to run 
# /opt/dell/srvadmin/sbin/racadm set BIOS.ProcSettings.LogicalProc Disabled
# /opt/dell/srvadmin/sbin/racadm jobqueue create BIOS.Setup.1-1
# /opt/dell/srvadmin/sbin/racadm serveraction powercycle
# but the install into warewulf vNFS didnt work for some odd reasons
# so trying a container-based install and see if it works better.
#
# Adopted from https://github.com/singularityware/singularity/blob/master/examples/centos.def
#
# This Singularity .def file result in 1.6 GB image
#

BootStrap:docker
From:centos:7

#BootStrap: yum
#OSVersion: 7

MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
Include: yum

%runscript
    /opt/dell/srvadmin/bin/idracadm "$@"

%post
	yum -y install bash tar gzip bzip2 wget curl coreutils util-linux-ng which less vi kmod dmidecode libcmpiCppImpl0 openwsman-server sblim-sfcb sblim-sfcc net-snmp-utils  pciutils libxslt openssl setserial libwsman1 openwsman-client
	#yum -y install bash tar gzip bzip2 wget curl coreutils util-linux-ng which less vi kmod dmidecode 
	#  libcmpiCppImpl0 
	# openwsman-server 
	# sblim-sfcb sblim-sfcc 
	# net-snmp-utils  pciutils libxslt openssl setserial libwsman1 openwsman-client
	# util-linux-ng  # provides u/mount, etc
	# dmidecode is needed by dell racadm, and many more dependencies as listed 
	# kmod provides lsmod
	cd /opt
	mkdir src
	cd src
    	wget -nc https://downloads.dell.com/FOLDER04162301M/1/OM-SrvAdmin-Dell-Web-LX-8.5.0-2372.RHEL7.x86_64_A00.tar.gz -O racadm.tgz
	tar xzf racadm.tgz 
	rm      racadm.tgz
	#cd linux/RPMS/supportRPMS/srvadmin/RHEL7/x86_64
	#for F in $( ls -1 *rpm ); do rpm -ivh --nodeps $F; done
	cd linux/custom 
	find . -name *rpm -exec rpm -i --nodeps {} \;
	echo 'PATH=$PATH:/opt/dell/srvadmin/bin:/opt/dell/srvadmin/sbin:/opt/dell/toolkit/bin/; export PATH' > /etc/profile.d/dell_env.sh 
	chmod 664 /etc/profile.d/dell_env.sh

	# find linux/RPMS/ | grep rpm | wc  = 56
	# find linux/custom/ | grep rpm | wc = 70


	# /opt/dell/srvadmin/bin/idracadm  getsysinfo 
	# /opt/dell/srvadmin/bin/idracadm  set BIOS.ProcSettings.LogicalProc Disabled   seems kinda ok
	# even when
	# Singularity> /opt/dell/srvadmin/sbin/racadm  getsysinfo
	# ERROR: Unable to perform requested operation
	# racadm.img (no deps, 52 srvadm rpm

	# [master 876aadf] racadm broken, but idracadm seems to respond predictably.  moving build for singularity hub


	# so, trying with all deps rpm, and 70 rpm listed in linux/custom/
	# also moving to singhub so that it build overnite.  going to bed soon.
