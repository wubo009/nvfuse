NVFUSE
======

NVFUSE is an embeddable file system as a library running in the user-space incorporated with [SPDK library](https://github.com/spdk/spdk) which is a user-space NVMe driver newly introduced by Intel. Using this file system applications can directly submit I/O requests to NVMe SSDs. Then, it provides high performance, high reliability as well as POSIX-like interfaces (e.g., nvfuse_open, nvfuse_read, nvfuse_write, and nvfuse_close functions). Note that this file system doesn't make use of well-known FUSE (File System in Userspace) library to provide POSIX compatibility. Key features are listed as follows.

 - Running in userspace without any interactions with kernel driver so that interrupts, context switches, and memory copies are eliminated. 
 - Embeddable file system as a library that introduces new interfaces are similar to POSIX API.
 - Simple file system layout that is identical to EXT family file system (e.g., EXT3).
 - Strong file system consistency and durability without pessimistic journaling mechanism (e.g., EXT4 journaling) through NVMe metadata feature as described [NVMe spec](http://nvmexpress.org/wp-content/uploads/NVM_Express_1_2_Gold_20141209.pdf). 

Prerequisites
=============
In order to build NVFUSE, some relevant libraries must be configured. 

[DPDK](http://dpdk.org/doc/quick-start) and [SPDK](https://github.com/spdk/spdk) are required.

    1) Move onto https://github.com/spdk/spdk
    2) DPDK and SPDK must be installed based on their install guide explained in README.

Ubuntu/Debian:
	
    # sudo apt-get install -y gcc make libaio-dev libfuse-dev
	# sudo apt-get install -y gcc g++ libcunit1-dev libaio-dev libssl-dev

SPDK compile:

	# git clone https://github.com/spdk/spdk.git
	# cd ~/spdk
	# git submodule update --init
	# ./configure
	# make

Building
========

Once the prerequisites are installed, run 'make' within the NVFUSE directory to build the NVFUSE library and examples.

    # make SPDK_ROOT_DIR=/home/ysoh/spdk DPDK_DIR=/home/ysoh/spdk/dpdk/build

Before conducting an NVFUSE application, the kernel NVMe driver must be unloaded and some hugepages must be allocated. It can be done with the automation script like the below command line.

    # sudo scripts/setup.sh config

Disabling ASLR (Address Space Layout Randomization) is also required like this.

	# sudo scripts/disable_aslr.sh

Move to examples directory and execute helloworld. 

    # cd examples/helloworld
    # sudo ./start_helloworld.sh

Make use of performance benchmark.
	# cd examples/perf
	# sudo ./start_perf.sh

After finalizing an NVFUSE application, the kernel NVMe driver and hugepages can be reverted with the automation script.

    # sudo scripts/setup.sh reset

| Examples Not Working (To Be Enabled) |
|--|
|control_plane_proc|
|dpdk_simple_mp|
|fio_plugin|
|libfuse|
|multi_process|
|raw_block_device_perf|
|regression_test|
|create_1m_files|
|fio-nvfuse|
|fsync_test|
|kernel_filesystem|
|mkfs|
|reactor|

To do
=====

 - File system consistency management via light-weight journaling
 - Integration with scalable file system such as Ceph and GlusterFS

Contact
=======
Yongseok Oh (yongseok.oh@sk.com)
