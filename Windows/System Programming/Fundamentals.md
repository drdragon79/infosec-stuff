# Processes
- A set of resources used to execute a program.
- Are isolated from one another.
- Consists of:
	- A private virtual address space
	- Image of the executable program, contains the initial code to be executed
	- A private table containing the handle of the kernel objects
	- Access token - Security context - the context in which the process is running. Used to check of the process has permission over a resource
	- Threads - one or more threads to execute the program. First thread is created when the process is started.
* Some properties of process that we can find in task manager:
	* `Name`: the executable that is run by the process.
	* `PID`: Process ID, unique identified for the process.
	* `Status`: Current status of the process:
		* Running: Is consuming data coming from the message queue and processing it.
		* Suspended: The program doesn't look at it's message queue for at least 5 seconds.
	* `Users`: The owner of the process. 
	* `Session`: 0 is for process run by the system (services and system processes).
	* `Memory (active private working set)`: Amount of private memory that the process is using. 
	* `Commit Size`: The actual amount of private memory allocated for the process.
	* `Handle`: The number of handle the process currently has.
	* `Threads`: Number of threads executing the code.
	* `Platform`: Tells if the process is 64 bit or 32 bit.
# Virtual Memory
-  Every process has virtual address space.
- Process sees a flat linear memory assigned to it, but the virtual memory is mapped to different places in physical memory or on disk (This is taken care by memory manager).
-  Memory is managed in chunks called pages:
	- Default size of 4kb
- Processes access the memory irrespective of where it resides. 
	- Memory managed handles mapping of virtual memory to physical pages
	- Process doesn't need to know the actual physical address of a given address in virtual memory.
## Virtual Memory Layout
### 32 Bit
- Each process gets a potential User Process Space of 2 gigs that it can use. This is relative as each process gets its own private space.
- `User Process Space`: 00000000 to 80000000 - 2GB
- The other 2 gigs in a 32 bit computer is allocated to System Space where all the kernel, device drivers are loaded. As there is only one kernel, there is only one instance of this space
- `System Space`: 8000000 - FFFFFFFF - 2 GB
### 64 Bit
- Each process get a potential User Process space of 128 TB. This is relative as each process gets its own private space.
- `User Process Space` : 000000000000 - 7FFFFFFFFFFF - 128 TB
- Another absolute space is used by System Space with is also 128 TB, used by kernel, device drivers etc. As there is only one kernel, there is only instance of this space.
- `System Space`: FFF800000000000 - FFFFFFFFFFFFFFF: 128 TB
- The actual memory size that a register in 64 bit CPU can refer is in exabytes, but is not possible today. Hence most of the address space is left as it is or unmapped.

In 64 bit version of windows 8 and earlier, only 8 TB of user and kernel space was used.

# Dynamic Link Libraries (DLLs)
- Loadable modules, mapped into a process address space
- Contains:
	- Code
	- Data
	- Resource
- Can be shared between processes. Process A can point to a certain location in memory in RAM where the DLL is present, and another process B can also point to the same location in memory to access the same DLL. It will very expensive, to copy the DLL for every process.
- Many DLLs are provided with windows by default.

# Threads
- Entity that is schedule by the kernel to execute code on the processor 
- A thread contains:
	- State of CPU registers
	- Current access mode (user mode or kernel mode)
	- Each user mode has two stacks - One in user space and one in kernel space
	- Thread Local Storage (TLS) - User mode mechanism to store data on per thread basis.
	- Access Token (Optional) - by default, thread use default access token of the user, but sometimes the process needs to impersonate someone. 
	- Message queue and Windows (Optional) - Usually a thread is a CPU thread or an IO thread. But if the process creates a windows, the thread handling the window automatically gets a message queue. The thread sends data of everything happening on the window
	- Priority - A number between 0 and 31. Specifies the priority of the thread.
	- State:
		- Running - Thread is currently executing.
		- Ready - Wants to execute code but CPU is busy running other threads.
		- Waiting - Thread doesn't want to execute, as it is waiting for some data or IO processing to complete.
# System Architecture
![[Pasted image 20230828194948.png]]
![[Pasted image 20230828195012.png]]
# Windows Subsystem APIs
### Windows API
- Also called Win32 API
	- Classic C APIs from the initial day of windows NT.
- Some APIs are COM (Component Object Model)
### .NET
- Managed libraries running on top of the CLR
- Common language: C#, VB.NET, F#, C++, CLI, Powershell
### Windows Runtime (WinRT)
- New unmanaged API available from Windows 8.
- Built on top of enhanced version of COM.
# Windows API/Win32 API
### Getting System Information
```c
#include<stdio.h>
```
```rust
use windows::Win32::System::SystemInformation::{self, GetNativeSystemInfo, SYSTEM_INFO};
fn main() {
    let mut sysinfo: SYSTEM_INFO = SYSTEM_INFO::default(); // initializing the default value of SYSTEM_INFO
    unsafe { // using unsafe as GetNativeSystemInfo is an unsafe function
        GetNativeSystemInfo(&mut sysinfo as *mut SYSTEM_INFO); // using raw pointers as this API uses the raw pointer
    }
    println!("Number of Processor: {}", sysinfo.dwNumberOfProcessors);
    println!("Processor Type: {}", sysinfo.dwProcessorType);
    println!("Page size: {} bytes", sysinfo.dwPageSize);

}
```
### Handling Errors
```rust
```