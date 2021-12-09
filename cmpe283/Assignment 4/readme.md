CMPE 283 Assignment 4 write-up

—----------------------------------------------------------------------------------------------------------------------------

Pavan Karthik Gollakaram (015945670)
Worked with Shahbaz to get the VM function as required to get the exit handler and timer counts right. Partook in the removal and reloading of the modules. Tried to debug the entire setup but was unable to. Extensively discussed, went over and predicted what would be the changes observable between 'ept and no-ept' exit counts.


—----------------------------------------------------------------------------------------------------------------------------

Shahbaz Singh Mansahia (010027459)
I tried to get the nested VM to work correctly and show the right results for the exit handler and timer counts but was unsuccessful. I researched on ways to resolve or debug my configuration so that I would get the right results but due to the complexity of the projects, and the number of levels things could go wrong on, I was unable to do much. I did document the procedure for carrying out everything required for assignment 4.

—----------------------------------------------------------------------------------------------------------------------------

Check if kvm modules were loaded into the outer VM using ‘lsmod | grep kvm’
Removed both, the kvm and kvm_intel modules using ‘sudo rmmod kvm’ and ‘sudo rmmod kvm_intel’
Loaded in the correct  kvm modules using the commands “sudo insmod /classProjects/linux/arch/x86/kvm/kvm.ko” and “sudo insmod /classProjects/linux/arch/x86/kvm/kvm-intel.ko”
Took a screenshot of this screen to show successful loading of the correct modules.
Booted up the internal/nested VM which was set up in the previous assignments in hopes that it would now correctly show the right data in the registers.
Called all the leaf nodes using the cpuid command and took screenshots for each and stored it in “linux>cmpe283>Assignment 4> without ept0”
Turned off the nested VM
Removed kvm_intel module using ‘sudo rmmod kvm_intel’
Re-loaded the correct  kvm modules using the command “sudo insmod /classProjects/linux/arch/x86/kvm/kvm-intel.ko ept=0”
Took a screenshot of this screen again to show successful loading of the correct modules.
Booted up the internal/nested VM again.
Called all the leaf nodes using the cpuid command and took screenshots for each and stored it in “linux>cmpe283>Assignment 4> with ept0”

—----------------------------------------------------------------------------------------------------------------------------

Q) What did you learn from the count of exits? Was the count what you expected? If not, why not?
A)  Nothing went as expected. We routinely tried to fix the nested-vm to VM relationship but despite the modules loading and all the variables existing in the loaded modules, the nested VM simply did not print any of the requests that we were passing to it. 



Q) What changed between the two runs (ept vs no-ept)?

A) Since the configuration was not set up properly, we are simply discussing what we would think would happen based on what we have discussed in our lectures. We feel like there will be more exits with shadow paging as well as substantially more page fault exceptions. Since shadow paging relies upon page faults to actually update the local VM page table, this too would result in more interrupts while regular functioning. We know from our lectures that shadow paging would trigger more TLB flushes, page faults and Control Register access exits. The most common exits would be HLT and external interrupts (due to the nature of shadow paging). Cpuid exits will also be there as we call them to run the kvm_emulate_cpuid function. There will be fewer external interrupts in nested paging as it is structured very differently.
