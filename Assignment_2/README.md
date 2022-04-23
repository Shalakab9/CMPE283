CMPE 283: Virtualization Technologies
Assignment 2: Instrumentation via hypercall

Team Members:
Shalaka Bodke (015357069)
Saketh Reddy Banda (014843881)

Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.
A1.
Shalaka Bodke:
Started with the setup which was already present as it was done for assignment 1. Studied and explored about various exits and interrupts from the Intel SDM. Modified the cpuid.c code for returning the total number of exits and total time spent processing all the exits. Modified the kvm_emulate_cpuid function in the cpuid.c file and the vmx_handle_exit function. Wrote answers for the questions in the readme.md file.

Saketh Reddy Banda:
Started with the setup which was already present as it was done for assignment 1. Built another VM in the existing VM using Ubuntu ISO image. Tested if the machine had capability of VMX virtualization. Researched about the MSRs to be read in the code. Wrote the testing logic for the code that exercises the functionality in the hypervisor's modification. Run the cpuid.c successfully on the inner VM and committed it to the git repo. Wrote down the steps to complete this assignment in the readme.md file.

Q2. Describe in detail the steps you used to complete the assignment. 
A2.
* First, start by build a VM with an Ubuntu ISO image.
* Install git and clone the following linux git repository: git clone https://github.com/torvalds/linux.git
* Follow instructions given in the assignment pdf to build the kernel. Use command:
    sudo apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
* Copy the config file of the kernel version that you are building into the cloned linux folder. Use command: 
    cd linux
    cp /boot/config-(linux kernel version) .config
* Make oldconfig for building the kernel. Use: make oldconfig.
* Run the following instruction in the linux folder:
    make -j 2 modules && make -j 2 && sudo make modules_install  && sudo make install
* Reboot the VM.
* Modify the cpuid.c and vmx.c in the linus folder to get the output for the total number of exits and total time spent processing all the exits.
* After the modification, rebuild your code using the following command:
    sudo make -j 2 modules M=arch/x86/kvm
* Load and unload the kvm kernel module (kvm.ko) and kvm-intel module (kvm-intel.ko) using the following commands:
    sudo rmmod arch/x86/kvm/kvm-intel.ko
    sudo rmmod arch/x86/kvm/kvm.ko
    sudo insmod arch/x86/kvm/kvm.ko
    sudo insmod arch/x86/kvm/kvm-intel.ko
* For the testing we virt manager. Use command:
    sudo apt-get update
    sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
* Verify the KVM installation using 'virsh -c qemu:///system list'. You should see an empty list of virtual machines.
* Open the virtual machine manager using the Ubuntu ISO to create an inner VM.
* Install cpuid using following commands:
    sudo apt-get update
    sudo apt-get install cpuid
* Create the test code in inner VM.
* Run the test code in the inner VM.
* Run the below commands in the inner VM (which is inside a VM): 
    cpuid -l 0X4fffffff -s exit_number
    cpuid -l 0X4ffffffe -s exit_number
* Finally, use 'dmesg' to log the VMX features to the kernel log in the outer VM.
