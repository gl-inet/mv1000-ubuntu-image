
1,Docker kernel configuration:

General setup  --->
	[*] POSIX Message Queues
	-*- Control Group support  --->
		[*]   Example debug cgroup subsystem
		[*]   Freezer cgroup subsystem
		[*]   PIDs cgroup subsystem
		[*]   Device controller for cgroups
		[*]   Cpuset support
		[*]     Include legacy /proc/<pid>/cpuset file
		[*]   Simple CPU accounting cgroup subsystem
		[*]   Memory Resource Controller for Control Groups
		[*]     Memory Resource Controller Swap Extension
		[*]       Memory Resource Controller Swap Extension enabled by default
		[*]     Memory Resource Controller Kernel Memory accounting
		[*]   HugeTLB Resource Controller for Control Groups
		[*]   Enable perf_event per-cpu per-container group (cgroup) monitoring
		-*-   Group CPU scheduler  --->
			-*-   Group scheduling for SCHED_OTHER
			[*]     CPU bandwidth provisioning for FAIR_GROUP_SCHED
			[*]   Group scheduling for SCHED_RR/FIFO
		[*]   Block IO controller
		[*]     Enable Block IO controller debugging
	-*- Namespaces support  --->
		[*]   UTS namespace
		[*]   IPC namespace
		[*]   User namespace
		[*]   PID Namespaces
		[*]   Network namespace
		
-*- Enable the block layer  --->
	[*]   Block layer bio throttling support
	IO Schedulers  --->
		<*> CFQ I/O scheduler
		[*]   CFQ Group Scheduling support
		
		
Kernel Features  --->
	[*] Enable seccomp to safely compute untrusted bytecode


		
[*] Networking support  --->
	Networking options  --->
		<*> Transformation user configuration interface
		<*>   IP: ESP transformation
		[*] Network packet filtering framework (Netfilter)  --->
			[*]   Advanced netfilter configuration
			<M>     Bridged IP/ARP packets filtering
			Core Netfilter Configuration  --->
				<*> Netfilter connection tracking support
				<*> FTP protocol support
				<*> TFTP protocol support
				-*- Netfilter Xtables support (required for ip_tables)
				<*>   "addrtype" address type match support
				<M>   "conntrack" connection tracking match support
				<M>   "ipvs" match support
			<M>   IP virtual server support  --->
				[*]   TCP load balancing support
				[*]   UDP load balancing support
				<M>   round-robin scheduling
				[*]   Netfilter connection tracking
			IP: Netfilter Configuration  --->
				<*> IPv4 connection tracking support (required for NAT)
				<M> IP tables support (required for filtering/masq/NAT)
				<M>   Packet filtering
				{*} IPv4 NAT
				<M>     NETMAP target support
				<M>   iptables NAT support
				<M>     MASQUERADE target support
				<M>     REDIRECT target support

		<*> 802.1d Ethernet Bridging
		[*]   IGMP/MLD snooping
		[*]   VLAN filtering
		[*] QoS and/or fair queueing  --->
			<*>   Control Group Classifier
		[*] L3 Master device support
		[*] Network priority cgroup
		-*- Network classid cgroup
	
		
Device Drivers  --->
	[*] Multiple devices driver support (RAID and LVM)  --->
		<*>   Device mapper support
		<*>     Thin provisioning target
	[*] Network device support  --->
		[*]   Network core driver support
		<M>     Dummy net driver support
		<M>     MAC-VLAN support
		<M>     IP-VLAN support
		<M>     Virtual eXtensible Local Area Network (VXLAN)
		<*>     Virtual ethernet pair device
	Character devices  --->
		-*-   Unix98 PTY support
		[*]     Support multiple instances of devpts
		[*]   Legacy (BSD) PTY support
		


File systems  --->
	<*> The Extended 3 (ext3) filesystem
	[*]   Ext3 POSIX Access Control Lists
	[*]   Ext3 Security Labels
	-*- The Extended 4 (ext4) filesystem
	-*-   Ext4 POSIX Access Control Lists
	-*-   Ext4 Security Labels
	<*> Btrfs filesystem suppor
	[*]   Btrfs POSIX Access Control Lists
	<*> Overlay filesystem support
	Pseudo filesystems  --->
		[*] HugeTLB file system support

		
Security options  --->
	-*- Enable access key retention support
	[*]   Enable register of persistent per-UID keyrings
	<*>   ENCRYPTED KEYS
	[*] Enable different security models
	

-*- Cryptographic API  --->
	<*>   GCM/GMAC support
	
	
2,Docker install:
curl -fsSL https://get.docker.com -o get-docker.sh
bash get-docker.sh
