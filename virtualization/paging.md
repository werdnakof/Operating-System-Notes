# Paging

	Chopping up virtual segment into fixed-sized pieces

Instead of splitting up a process’s address space into some number of
variable-sized virtual segments (e.g., code, heap, stack), we divide it into
 fixed-sized units, each of which is a **_Page_**.

**Frames**: physical memory as an array of fixed-sized slots called frames

Each of these frames can contain a single virtual-memory page.

Example:
An address space (AS) of 64 bytes total in size, 
with 4 pages of size 16-byte pages
(virtual pages 0, 1, 2, and 3)

| Space   | VPN    | VPN in binary |
|---------|--------|---------------|
| 0 - 16  | page 0 | 00            |
| 16 - 32 | page 1 | 01            |
| 32 - 48 | page 2 | 10            |
| 48 - 64 | page 3 | 11            |

2 bits (2^2 = 4) needed minimum to select each page in a 4 page virtual address space

The Physical memory consists of an array of fixed-sized slots, 
consider 8 frames, making for a 128-byte physical memory

| Space     | usage           | PFN          | PFN in binary |
|-----------|-----------------|--------------|---------------|
| 0 - 15    | reserved for OS | page frame 0 | 000           |
| 16 - 32   | unused          | page frame 1 | 001           |
| 32 - 47   | page 3 of AS    | page frame 2 | 010           |
| 48 - 63   | page 0 of AS    | page frame 3 | 011           |
| 64 - 79   | unused          | page frame 4 | 100           |
| 80 - 95   | page 2 of AS    | page frame 5 | 101           |
| 96 - 111  | unused          | page frame 6 | 110           |
| 112 - 127 | page 1 of AS    | page frame 7 | 111           |

3 bits (2^3 = 8) needed minimum to select each page in a 8 page virtual address space

The pages of the virtual address space have been placed at different locations
throughout physical memory, and when the OS wishes to place the 64-byte
address space into our eight-page physical memory, it simply finds 4 free pages

# Page Table

OS usually keeps a _per-process_ data structure known as a **_page table_**.
 
Page table stores address translations for each of the pages in the address space, and its respective physical memory address space each page resides

Imagine the process with that tiny address space (64 bytes) is performing a memory access:

movl <virtual address>, %eax

i.e. load of the data from address <virtual address> into the register eax

To translate this virtual address that the process generated to physical address , 
we have to first split it into two components: 
- the virtual page number (VPN), and
- the offset within the page

Physical frame number (PFN) is what we want

Using above example, for a virtual page size of 64 bytes- 4 pages of 16 bytes
- The minimum no. bits required to achieve enough granularity to select each byte is 2^6 = 64
- i.e. the virtual address needs to be 6 bits
- 2 bits to represent which page
- 4 bits to represent the offset from each page

OS needs to translate the virutal address to physical address
Example:
movl 21, $eax

21 -> 01 0101
01 -> VPN 1
0101 -> 5th byte in 1st virtual page

1st page resides in 112-127 physical page i.e. PFN 7 
virtual address becomes 01 0101 to 111 0101

The page table is just a data structure that is used to map virtual addresses (virtual page numbers) to physical addresses (physical frame numbers). 

The simplest form is called a linear page table, which is just an array. 
The OS indexes the array by the VPN, 
and looks up the page-table entry (PTE) at that index,
in order to find the desired physical frame number (PFN).

The flow of getting physicla address from virtual address

PageTableBaseRegister is the starting index of the page table

  VPN = (VirtualAddress & VPN_MASK) >> SHIFT
  PTEAddr = PageTableBaseRegister + (VPN * sizeof(PTE))
  PFN = AccessMemory(PTEAddr)
  offset = VirtualAddress & OFFSET_MASK
  PhysAddr = (PFN << SHIFT) | offset
  Register = AccessMemory(PhysAddr)

In our example, VPN_MASK would be set to 0x30 (hex 30, or binary
110000) which picks out the VPN bits from the full virtual address; 
SHIFT is set to 4 (the number of bits in the offset), such that we move the VPN
bits down to form the correct integer virtual page number. 

For example, with virtual address 21 (010101), and masking turns this value into
010000; the shift turns it into 01, or virtual page 1, as desired. We then use
this value as an index into the array of PTEs pointed to by the page table
base register

Once this physical address is known, the hardware can fetch the PTE
from memory, extract the PFN, and concatenate it with the offset from
the virtual address to form the desired physical address. The PFN being left-shifted by SHIFT, and then logically OR’d with the offset to form the final address

In addition a PTE has
- Protection bits indicates whether the page could be read from, written to, or executed from. 
- Present bit indicates whether this page is in physical memory or on disk (i.e., it has been swapped out).
- Dirty bit indicates whether the page has been modified since it was brought into memory
- reference bit (a.k.a. accessed bit) is sometimes used to track whether a page has been accessed, and is useful in determining which pages are popular and thus should be kept in memory


1024 movl $0x0, (%edi,%eax,4)
1028 incl %eax
1032 cmpl $0x03e8,%eax
1036 jne 0x1024

virtual address space of size 64KB

page size of 1KB

there is the virtual page the code lives on. 

Because the page size is 1KB, 
virtual address 1024 resides on the second page of the virtual address space 
(VPN=1, as VPN=0 is the first page). 

Assume this virtual page maps to physical frame 4 (VPN 1 → PFN 4).

there is the array itself. Its size is 4000 bytes / 4KB (1000 integers),
and assume that it resides at virtual addresses 40000 through 44000

Virtual pages for this decimal range are VPN=39 ... VPN=42. 
Thus, we need mappings for these pages. 

Assume these virtual-to-physical mappings for the example: 
(VPN 39 → PFN 7),(VPN 40 → PFN 8), (VPN 41 → PFN 9), (VPN 42 → PFN 10).

each instruction fetch will generate two memory references:
- one to the page table to find the physical frame that the instruction resides within, and 
- one to the instruction itself to fetch it to the CPU for processing

there is one explicit memory reference in the form of
the mov instruction; this adds another page table access first (to translate
the array virtual address to the correct physical one) and then the array
access itself.

there are 10 memory accesses per loop,
which includes four instruction fetches, one explicit update of memory,
and five page table accesses to translate those four fetches and one explicit
update.

















<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQxNDE3OTU2MV19
-->