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
#include <stdio.h>
#include <Windows.h>
#include <stdlib.h>

int main() {
    SYSTEM_INFO *systeminfo = (SYSTEM_INFO *)malloc(sizeof(SYSTEM_INFO));
    GetNativeSystemInfo(systeminfo);
    printf("Processor Type: %d\n",systeminfo->dwProcessorType);
    printf("Number of Processors: %d\n",systeminfo->dwNumberOfProcessors);
    printf("Page size: %d\n",systeminfo->dwPageSize);
}
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
- Output
```
Processor Type: 8664
Number of Processors: 4
Page size: 4096
```
### Handling Errors
`GetLastError` can be used to get error code encountered in the last function. `FormatMessage` can be used for format the error message to get the textual description.
```c
void errorhandling(){
    DWORD errchar; // for storing error code
    WCHAR errormsg[512]; // buffer for storing error message
    HANDLE process = OpenProcess(
        PROCESS_TERMINATE, // asking terminal permission for the process
        FALSE,
        468 // process ID
    );
    if(process) { // handle will be NULL or 0 if access is denied
        TerminateProcess(
            process, // handle to process
            0 // exit code
        );
    } else {
        DWORD errorcode = GetLastError(); // get error code
        printf("Error Code: %u\n",errorcode);
        FormatMessageW(
            FORMAT_MESSAGE_FROM_SYSTEM, // flags for the error
            NULL,
            errorcode, // error code
            0,
            errormsg, // buffer for error message
            512,
            NULL);
        printf("Error Message: %ws\n",errormsg);
    }
}
```
### Strings
- Windows uses UTF-16 to represent strings, which is 2 bytes per character.
- Sometimes also known as Unicode.
- Windows API also makes use of Unicode strings. For compatibility reasons, ANSI (ASCII) versions of the API call is also available.
        - Functions ending with "W" represents Unicode version.
        - Functions ending with "A" represents ANSI version.
        - Function not having "A" or "W" is really a macro to either A or W version, depending on the machine and compiler. This is usually W for modern systems.
- Types available:
        - *WCHAR*, which a typedef to *wchar_t*, also knows as wide char, represents UNICODE charachters - 2 Bytes.
        - *PWSTR* - Pointer to wide string (Unicode String Pointer), *PSTR* - Pointer to ASCII characters (ASCII String Pointer)
        - *PCWSTR* - Pointer to constant wide string, *PCSTR* - Pointer to constant ascii string.
        - Unicode Literal : L"Doctor" - Uses 2 bytes per character.
- String manipulation:
        - Classic S functions for string manipulation is unsafe.
                - Eg: strcpy(ascii) , wcscpy(unicode), strcat etc.
        - Safe versions:
                - strcpy_s()
                - wcscpy_s()
        - Other safe versions, included in <strsafe.h>:
                - StringCchCopy
                - StringCchCat
                - StringCchPrintf
                - Have A and W version.
- Some miscellaneous functions:
```c
void stringsfunctions() {
    // get system directory path API.
    WCHAR systemdirectory[MAX_PATH];
    GetSystemDirectoryW(
        systemdirectory, // buffer to store systemdirecotry information
        _countof(systemdirectory) // length so that the function cannot overflow the buffer
    );
    printf("System Directory %ws\n",systemdirectory);

    // get computer name API
    WCHAR computername[MAX_COMPUTERNAME_LENGTH];
    DWORD len = _countof(computername); // _countof counts the element in a buffer
    BOOL res = GetComputerNameW( // returns BOOL
        computername, // computer name buffer
        &len // LPDWORD nSize, which is a long pointer to a DWORD.
        // this type of parameter is called in-out parameter.
        // IN: len specifies the length to fill in the buffer.
        // OUT: the function then writes the length actually filled by the function.
    );
    if (res) {
        printf("Computer Name(%d): %ws\n",len,computername);
    }
}
```
### Structures
- Most structures in the windows API are defined like:
```c
typedef struct _SOME_STRUCT {
        // fields
} SOME_STRUCT, *PSOME_STRUCT, *LPSOMESTRUCT;
```
- The long prefix is there for historic reasons. There is no long pointer as such. Pointers have defined size:
        - 4 bytes for 32 bit processor
        - 8 bytes for 64 bit processor
- Some structures have version, and the version depends on the size of the structure. The size of the structure is defined by the first member.
```c

```
### Windows Version
- Windows Numeric Version:
        - Windows NT (4.0)
        - Windows 2000 (5.0)
        - Windows XP (5.1)
        - Windows Server 2003, 2003 R2 (5.2)
        - Windows Vista, Server 2008 (6.0)
        - Windows 7, Server 2008 (6.1)
        - Windows 8, Server 2012 (6.2)
        - Windows 8.1, Server 2012 R2 (6.3)
        - Windows 10, Server 2016 (10)
- `GetVersionEx()` function was used traditionally to get the version information, but is now deprecated.
- New helper functions defined in `versionhelpers.h` is used now:
        - `IsWindowsXPSP3OrGreater()`
        - `IsWindows10OrGreater()`
        - `IsWindowsServer()`
- These functions are implemented with `VerifyVersionInfo()`
- It requires manifest file to get correct information%
