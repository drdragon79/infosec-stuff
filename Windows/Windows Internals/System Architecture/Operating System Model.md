- Windows is a multi-user operating system, where application is separated from the OS.
- The kernel code runs in the `privileged mode` also known as the kernel mode.
	- Kernel mode has access to all the system data and hardware
- The user application runs in the `non-privileged mode` also known as user mode.
	- User mode has limited access to the system data.
- The user mode, when requires system service, the system service executes a special instruction (interrupt), switches the thread to the kernel mode.

- Windows kernel is `monolithic` in nature like most UNIX kernels.
- Which means that, OS, kernel code and the device drives, all share the same `kernel-mode protected memory space`.