CMPE 283: Virtualization Technologies
Assignment 4: Nested Paging vs. Shadow Paging

Team Members:
Shalaka Bodke (015357069)
Saketh Reddy Banda (014843881)

Q1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented/researched.
A1.
Shalaka Bodke:
Wrote the test script as per the assignment requirement. Executed the code with ept=0 and extracted the result. Wrote answers for the questions in the readme.md file.

Saketh Banda Reddy:
Wrote and executed code with ept!=0 and extracted the result. Tested the code and verified the results against the test script for nested and shadow paging. Wrote down the steps to complete this assignment in the readme.md file.


Q2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.
A2. Included in the folder under the names of Output_images.


Q3. What did you learn from the count of exits? Was the count what you expected? If not, why not?
A3. When compared to nested paging, the number of exits increases in shadow paging. This is to be expected because VM exit occurs during nested paging when an EPT violation occurs. However, in the case of shadow paging, it may exit whenever the VM attempts to execute CR0, CR3, CR4, or any paging-related exits such as a page fault.

Q4. What changed between the two runs (ept vs no-ept)?
A4. EPT Mode - In this mode, we have two layers of page tables that are used to translate from Guest VA to Guest PA and then to Host PA. If more page accesses are required, the guest VM should own the page table, and thus all operations on CR3 are performed in the same environment, i.e. there is no need to exit.
    No-EPT Mode - In shadow paging mode, the guest VM does not own the page table; the VMM must simulate CR3.