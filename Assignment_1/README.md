CMPE 283: Virtualization Technologies 
Assignment 1: Discovering VMX Features

Team Members:
Shalaka Bodke (015357069)
Saketh Reddy Banda (014843881)

Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.
A1.
Shalaka Bodke:
Worked on setting up the environment in Windows by installing VMware Workstation and then installed Linux Ubuntu in it. Built the Linux kernel modules and libraries for creating it's local copy. Researched about MSRs to be read in the SDM. Added custom logic to cmpe283-1.c for reading and getting output for different MSRs. Wrote MSR code for primary and secondary procbased controls. Explored the VMX features present in the Intel processor by writing a linux kernel module that queries these features. Tested the code after running by verifying the output with the sample output provided. Wrote answers for the questions that are in readme.md file.

Saketh Reddy Banda:
Worked on setting up the environment in Windows by installing VMware Workstation and then installed Linux Ubuntu in it. Built a VM successfully by allocating 100GB storage and 8GB RAM to it. Tested the VM created to check it's capability of feature recognition and VMX virtualiztion. Researched about MSRs to be read in the SDM. Added custom logic to cmpe283-1.c for determining if the secondary procbased controls are present. Wrote MSR code for entry and exit controls. Run the cmpe283-1.c successfully on the VM and committed it to the git repo. Wrote down the steps to complete this assignment in the readme.md file.

Q2. Describe in detail the steps you used to complete the assignment. 
A2.
* First, download and install VMware workstation player and install Ubuntu ISO in it to create a virtual machine. The links for both are as follows:
    https://ubuntu.com/download/desktop
    https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
* Fork the below given linux repository:
    https://github.com/torvalds/linux
* Install git in your VM using: sudo apt-get install git
* Clone the forked master linux git repository for the kernel sources using: https://github.com/Shalakab9/linux.git
* Install packages for building the the linux kernel. Use command:
    sudo apt-get install libncurses-dev gawk flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
* Copy the config file of the kernel version that you are building into the cloned linux folder. Use command: 
    cd linux
    cp /boot/config-(linux kernel version) .config
* Make oldconfig for building the kernel. Use: make oldconfig.
* Prepare to build the kernel. Use: make prepare.
* Install all required modules for the kernel. Use: make modules.
* Build the kernel using 'make' command.
* Copy the modules into a folder using: sudo make INSTALL_MOD_STRIP=1 modules_install
* Use 'make install' to build the kernel.
* Reboot the ubuntu linux to load the latest kernel build.
* Create a folder for the assignment in the cloned linux folder and copy the provided 'makefile' and the modified 'cmpe283-1.c' files in that folder.
* Add the following license into the .c file:
    MODULE_LICENSE("GPL_v2");
* Run the makefile using: make.
* The modifications done to cmpe283-1.c file are:
    * Write functionality for querying the given MSRs.
    * We referred to the SDM and created different names, structures and bit positions for different MSRs: entry, exit, pinbased, primary and secondary procbased controls.
    * We called the function report_capability() with parameters to print result for entry, exit, pinbased and procbased controls. This function is used to detect and print VMX capabilities of CPU.
* Load and unload specific kernel modules into the kernel. Use commands:
    sudo insmod ./cmpe283-1.ko (When we insert a module into the kernel, the module_init macro will be invoked which will call the init_module function).
    sudo rmmod ./cmpe283-1.ko (When module is removed using rmod, the module_exit will be invoked which will call the cleanup_module).
* Finally, use 'dmesg' to log the VMX features to the kernel log.


