# HeapExplore

HeapExplore is a Windows memory inspection tool that demonstrates how heap structures of a running process can be enumerated and inspected using the Windows ToolHelp API.

The program allows you to search for a specific string inside heap allocations of another process by reading heap blocks via ReadProcessMemory.

This project was primarily created as a learning exercise for Windows internals, memory management, and process inspection.



# Features

Enumerates heaps of a target process

Iterates over heap entries using the Windows ToolHelp API

Reads heap memory blocks from another process

Searches for user-provided strings in heap memory

Demonstrates pointer arithmetic and memory scanning

Includes a built-in demo mode
# Screenshots
![App Screenshot](https://i.imgur.com/OrkXWuH.png)
# How It Works

HeapExplore uses the following Windows APIs:

CreateToolhelp32Snapshot

Heap32ListFirst

Heap32First

Heap32Next

Heap32ListNext

OpenProcess

ReadProcessMemory

# Workflow

The user enters the PID of a target process

The program creates a heap snapshot

Each heap is enumerated

Every allocated heap entry (PROCESS_HEAP_ENTRY_BUSY) is inspected

The heap block is copied locally using ReadProcessMemory

A custom memfind function searches for the specified string

If found, the program prints the real address inside the target process

# Example
```bash
  PID of Process (or type malloc_t for demo)-> 1234

Enter the needle -> Hello

=== RESULTS FOUND FOR PROCESS ID 1234 ===
[+] String found successfully at -> 0000021F3C4A1010
Start of data-blob -> 0000021F3C4A1000
Heap ID for data blob -> 0000000000000001
Demo Mode

You can test the scanner without another process.

Enter:

malloc_t

The program will allocate a heap buffer internally containing:

Jelly World :o !

and display the buffer information.
```


Example Target Program

To test HeapExplore you can run a simple program like this:

#include <Windows.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    char* datablob = malloc(100000);
    strcpy(datablob, "Hello");

    printf("Buffer address: %p\n", datablob);
    getchar();

    free(datablob);
    return 0;
}

Run this program, obtain its PID, and scan it with HeapExplore.

Build

The project was developed using:

Microsoft Visual Studio

Windows API

C++

Compile as either:

x64 → x64 target process

or

x86 → x86 target process

Mixing architectures may prevent heap enumeration.
----
Important Notes
Windows Heap Behavior

Modern Windows versions use the Low Fragmentation Heap (LFH).

Because of this:

Some allocations may not appear through Heap32First / Heap32Next

Small allocations can be internally grouped by the heap manager

Therefore HeapExplore is intended primarily as a learning tool.

Professional memory scanners typically enumerate memory using:

VirtualQueryEx
ReadProcessMemory

instead of relying solely on heap enumeration APIs.

# Limitations

Only scans heap allocations

Does not scan stack or global memory

Some heap allocations may be hidden by LFH

Requires permission to read target process memory

Best used when both processes share the same architecture

Educational Goals

# This project demonstrates:

Windows process memory access

Heap enumeration

Pointer arithmetic

Pattern searching in memory

Interaction with low-level Windows APIs

Future Improvements

# Possible future features:

full process memory scanner (VirtualQueryEx)

pattern scanning (byte signatures)

wide string support

better error handling

multithreaded scanning

memory region filtering

# Credits

Created by:

Fabio Baensch
GitHub: KernelPhantom-010

Disclaimer

This project is intended for educational and research purposes only.

Do not use this software to inspect or manipulate processes without proper authorization.
