CMPE 283 Assignment 3 write-up

—-----------------------------------------------------------------------------------------------------------------
Pavan Karthik Gollakaram (015945670)

I worked with Shahbaz in setting up the code of cpuid.c and vmx.c. We examined Intel's SDM to view the available exit codes and gathered the required code to print out the supported KVM exit codes and what numbers are associated with the exit codes. 

—-----------------------------------------------------------------------------------------------------------------

Shahbaz Singh Mansahia (010027459)

Coded the D leaf node and installed the kvm modules on my local machine this time. Also rewrote the lost data from the force push from last week to keep assignment documentation intact with the help of Pavan. Debugged the virtual network adapter issues that arose during this particular assignment. Also worked with Pavan for implementing the C leaf of the assignment. Tested using the cpuid function in the nested VM with mixed results as the code, despite building and installing, and the kvm and kvm_intel modules being mod-probe’d successfully, would not translate to cpuid outputs in the nested VM terminal (when using the cpuid package installed and tested using “cpuid -l 0x4ffffff<leaf>; with <leaf> being the specific code value passed to the function as instructed in the assignment).
—-----------------------------------------------------------------------------------------------------------------

Setup the virt-manager and other dependencies for the nested VM in the previous assignment but forgot to mention it/ran out of time/issues during the last 15 minutes so I couldn’t mention it.
Swapped the extern declaration to vmx.c and the variable declaration to cpuid.c and that has fixed the install issue.
Re-tweaked the code in the vmx.c file to match the new code format. Initially, the code was based on the kvm_vmx_max_exit_handlers value as defined in the vmx.c file but now have had to hard code the array size to 69 to match SDM documentation.
The if clause already defined (wrote this code as a safety net initially) to return 0 and 0x4fffffff values in registers is now more relevant. 
Built the modules and linux files again.
Switched to super-user mode using ‘sudo bash’ command.
Installed the modules successfully using ‘sudo make INSTALL_MOD_STRIP=1 modules_install && make install’ command in terminal.
Checked the kvm status using ‘kvm-ok’ and then ran the installed kvm module using ‘modprobe intel_kvm’ and ‘kvm’.
Rebooted machine to make sure installed changes are preloaded.
Changed the virtual machine manager cpu and memory config for nested vm to ensure smoother and faster functioning.
Ran into an issue where the networking interface for the VM was down due to a suspend-resume crash
Spent a few hours researching how to fix the issue and fixed it using ‘nmcli networking on’ . The following link helped me resolve the issue: “https://askubuntu.com/questions/1218616/after-a-restart-due-to-a-system-crash-the-ethernet-interface-is-disappeared-how” ("AndreaNobili", 2020)
Used ‘rmmod’ on ‘kvm’ and ‘kvm_intel’ modules to unload them and then loaded them back again using ‘modprobe’.
After getting rid of the cyclical dependency issues, and extensive debugging of the code and system configuration, we were unable to get the code to display in cpuid calls from the nested vm.
Decided to attach all the setup and configuration build snapshots to document our progress on the assignments and project overall. Getting it to build was some sort of feat I guess. We are aware that the code does not successfully check for disabled controls from the VM but without an active code display (despite trying printk and pr_info messages we were unable to find them in the dmesg log or anywhere else), we could only work on it as much as we did.

—---------------------------------------------------------------------------------------------------------------------------

Q1) Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail? 

A1) Since we did not have any displays indicating or giving us data on this, I would like to point out that one of the highest tolls was the device interrupts (especially the network adapter). I say this because when the network adapter was disabled by the VM and on the path to re-enable those controls, I noticed a substantial performance boost in the VM which could only be explained by no constant network device interrupts. Another noticeable performance discrepancy was when the VM(s) were left on idle. When the VM was not doing anything and I’d switch windows, it’d lag a bit before coming back to normal speeds. This was probably due to the halt (HLT) interrupts. At multiple points while working on the assignments, when I’d switch away from VM window (with machine on idle) and come back after long period of time (about 30 minutes or so), there would be a very noticeable lag initially. Sometimes it could be explained by the lock screen being triggered; I feel like it was due to a combination of multiple factors like process scheduling on the host machine, the HLT interrupts/loop on the VM(s) and how those interrupts/loops function on a VM lock screen.

Q2) Of the exit types defined in the SDM, which are the most frequent? Least?

A2) I’d say the device interrupts and HLT interrupts were the most common. Again, this is coming not from displayed feedback from the machine but from intuition based on what we learned in the course so far and observed machine behavior over the course of the assignments.

—---------------------------------------------------------------------------------------------------------------------------


References
"AndreaNobili". (2020, March 19). After a restart due to a system crash the ethernet interface is disappeared. How to restore it? Ask Ubuntu. Retrieved November 29, 2021, from https://askubuntu.com/questions/1218616/after-a-restart-due-to-a-system-crash-the-ethernet-interface-is-disappeared-how


