Pavan Karthik Gollakaram (015945670)

Overall, I handled the testing phase of the VM by using my team member’s VM setup, as my own VM kept running into unending issues. I ran the CPUID command and tested the KV responses from the VM for each register whenever the handle_exit/emulate_cpu_Id methods were passed with the values of 0x4fffffff and 0x4ffffffe using the ‘sudo cpuid -l 0x4ffffffe’ commands. All we could get were mixed results for the build as we struggled with the initial setup of the VM environment and also because it simply wouldn’t build the ‘INSTALL MOD_STRIP=1 install_modules’ command. ‘Make’ and ‘make modules’ are executed successfully but ‘kvm=>kvm_inte->kvm’ command would throw cyclic dependency errors every time. We spend hours collectively researching a solution for this problem but were unable to resolve the issue.

—-----------------------------------------------------------------------------------------------------------------

Shahbaz Singh Mansahia (010027459) I basically tried to code the exit_handler code and the cpuid’s emulate_cpuid code. Created some if-else statements and skeletal code for both the assignments. Managed to debug most issues as we were simply told about the extern command but did not know that we had to also explicitly export the variables using the EXPORT_SYMBOL_GPL() commands. I initially assumed these would apss regardless since c++ tends to share globals across c files as long as they are imported. I tried to install the nested VM and asked Pavan for help with testing and researching how to pass the cpuid command with the relevant parameters and test the code I wrote. We struggled mostly because, despite researching the issue of the cyclic dependency, we got nowhere. I hope I can resolve this issue and succeed in the next assignment. Most of the issues stemmed from the configuration of the system and the compilation time for the linux source code files rather than the code complexity of the code since documentation can be studied but if you don’t have experience with C, and on top of that try to setup a nested VM environment with a custom VMM, one who’s source code you don’t completely understand, it gets very hard to find any answers or support for any problems one runs into.

—-----------------------------------------------------------------------------------------------------------------

Added if-else statements to the ‘kvm_emulate-cpuid()’ function in cpuid.c as required by the assignment(s) for each of the assignment clauses.

Initialized and added the total_exits variable to the vmx.c file in the ‘__vmx_handle_exit()’ method to tally the total number of exits taken by the VM.

Defined the total_exits variable within the vmx_handle_exit method mentioned earlier but initialized as an unsigned 32 bit variable right above the method definition in the section labelled “Tweaked code”. This effectively turned total_exits into a global variable for the scope of that file.

Declared the total_exits variable as an extern uint32_t variable inside the cpuid.c file under the cpuid function.

Researched the nature of the errors causing the builds to fail. Eventually figured out that it was being caused because total_exits wasn’t being exported properly. Utilized the ‘EXPORT_SYMBOL_GPL()’ method to export the variable to cpuid file.

Built the code successfully. It still fails at the second build/repacking command (‘make INSTALL_MOD_STRIP=1 modules_install && make install’) though.

Added further code to calculate the time spent exiting. Used the same EXPORT_SYMBOL_GPL() method to export the 2 uint64_t values that stored the clock timer values from the VCPUs using the rdtsc() method.

Researched on how to split up the uint64_t variables into uint32_t variables as the registers are 32 bit.

Found the answer in the assignment 1 coding file. Utilized similar logic to how pr_info() method for splitting/using the lo and hi values.

Developed the code to calculate the total time spent in the exit handler and assigned it to the necessary registers.

Built the code successfully using the ‘make -j 8 modules’ command but again, failed at repackaging. I am assuming that the code is error free but the linux package build fails. It is still the same issue as the old ‘kvm’ and ‘kvm->intel’ cyclical dependency issue. As far as I could tell, while I could not find any reference to this exact dependency, I did find some data on the fact that a similar dependency was being caused by updates to linux builds, etc.

Installed virt-Manager, qemu-kvm, libvirt-daemon-system, libvirt-clients, bridge-utils and virtinst to run the nested VM.

Tried to chcek nested virtualization and kvm status but got “unknown symbol” issues for all the custom variables used in the cpuid.c and vmx.c files. The “kvm->kvm_intel->kvm” cyclic dependency still causes an error in the ‘make INSTALL_MOD_STRIP=1 install_modules” command

Installed the nested VM and researched installing custom KVM in the initial VM to del/test with the nested VM using out custom code

Installed the cpuid package in the nested VM and conducted tests.
