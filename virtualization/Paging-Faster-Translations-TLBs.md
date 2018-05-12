To speed address translation, we are going to add what is called a **_Translation-Lookaside Buffer_**, or TLB. 

A TLB is part of the chip’s memory-management unit (MMU), and is simply a hardware cache of popular virtual-to-physical address translations; thus, a better name would be an address-translation cache. 

Upon each virtual memory reference, the hardware first checks the TLB to see if the desired translation is held therein; if so, the translation is performed (quickly) without having to consult the page table (which has all translations).

Below shows a rough sketch of how hardware might handle a virtual address translation, assuming a simple linear page table (i.e., the page table is an array) and a hardware-managed TLB

	1 VPN = (VirtualAddress & VPN_MASK) >> SHIFT 
	2 (Success, TlbEntry) = TLB_Lookup(VPN) 
	3 if (Success == True) // TLB Hit 
	4 	if (CanAccess(TlbEntry.ProtectBits) == True) 
	5 		Offset = VirtualAddress & OFFSET_MASK 
	6 		PhysAddr = (TlbEntry.PFN << SHIFT) | Offset 
	7 		Register = AccessMemory(PhysAddr) 
	8 	else 
	9 		RaiseException(PROTECTION_FAULT) 
	10 else // TLB Miss 
	11 	PTEAddr = PTBR + (VPN * sizeof(PTE)) 
	12 	PTE = AccessMemory(PTEAddr) 
	13 	if (PTE.Valid == False) 
	14 		RaiseException(SEGMENTATION_FAULT) 
	15 	else if (CanAccess(PTE.ProtectBits) == False) 
	16 		RaiseException(PROTECTION_FAULT) 
	17 	else 
	18 	TLB_Insert(VPN, PTE.PFN, PTE.ProtectBits) 
	19 	RetryInstruction()



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4NDQ3MDIyOCwtMTg5NTIzMDI5M119
-->