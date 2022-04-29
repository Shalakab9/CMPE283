CMPE 283: Virtualization Technologies
Assignment 3: Instrumentation via hypercall (part 2)

Team Members:
Shalaka Bodke (015357069)
Saketh Reddy Banda (014843881)

Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.
A1.
Shalaka Bodke:
Worked on writing code for one of the remaining two leaf nodes (0x4FFFFFFD). Wrote the logic for returning the number of exits, the time spent on processing the exit number provided on input and total time spent for that  particular exit. Made these changes to the cpuid.c and vmx.c files. Wrote answers for the questions in the readme.md file.

Saketh Reddy Banda:
Worked on writing code for one of the remaining two leaf nodes (0x4FFFFFFC). Wrote the logic for returning the number of exits, the time spent on processing the exit number provided on input and total time spent for that  particular exit. Researched on testing of the kernel code. Wrote down the steps to complete this assignment in the readme.md file.

Q2. Describe in detail the steps you used to complete the assignment. 
A2.
* Make code changes in cpuid.c and vmx.c as per assignment requirements.
* Compile the code using: make -j 4 modules.
* Run: sudo bash
* Install the module packages using: sudo make INSTALL_MOD_STRIP=1 modules_install && make install
* Stop the inner VM.
* Run the following command on the outer VM to remove the old binaries:
    rmmod kvm_intel
    rmmod kvm 
* Install latest binaries using:
    modprobe kvm
    modprobe kvm_intel
* Create inner VM using:
    sudo apt update
    sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
    sudo systemctl status libvirtd
    sudo systemctl enable --now libvirtd
    sudo apt install virt-manager
    sudo virt-manager
* Install ubuntu 20.4 iso image. Also, download the CPUID debian Package and install it.
* Run the following commands in the inner VM:
    cpuid -l 0X4ffffffc -s exit_number
    cpuid -l 0X4ffffffd -s exit_number
* Run the 'dmesg' on the outer VM to get the output.

Q3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
A3.
Exit counts greater than 0 were observed for exit codes: 0, 10, 28, 30, 31, 40, 48. For exit codes 29, 46, 47, 54 and 55 the count increased at a stable rate only during reboot operation. For the remaining exit codes the counts increased to higher value during reboot operation, whereas it increased with a lower value for normal operations such as file creation and deletion etc. From the output pattern mentioned below, it was observed that, there were nearly 1.5 million exits during a reboot operation.

Q4. Of the exit types defined in the SDM, which are the most frequent? Least?
A4. 
Most frequent - 48, 30.
Least frequent - 29, 54.