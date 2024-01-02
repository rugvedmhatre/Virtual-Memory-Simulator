# Virtual-Memory-Simulator
An implementation of a critical component in Virtual Memory systems: the two-level page table. With a physical address space of 12-bits and a 14-bit virtual address space, each page table is contained within a single page and features 4-byte entries. The page size is 64B and the memory is byte addressable.

## Virtual Address

The virtual address size of this design will be 14 bits. The following depicts the virtual address layout:

| 4-bit outer page table | 4-bit inner page table | 6-bit offset |
|---|---|---|

## Page Table Structure

The page/frame size is 64B. The size of a page table entry is 4B. For both levels, bit 0 is used as the valid bit. In the outer page table, the top 12 bits contain the physical address of the beginning of the next level page table. Bits 1-19 are unused and must be set to zero. The following depicts the PTE format for the outer page table.

| 12-bit Physical Address | Not used | Valid Bit |
| --- | --- | --- |

To access the outer page table, the 4-bit outer page table value in the virtual address should be added to the value stored in the page table register. This value is shifted by 2 to point to the beginning of an inner page table.

In the inner page table, the top 6 bits represent the frame number, and bits 1-25 are unused. The following table depicts the PTE format for the inner page table. To access this level of the page table, the 4-bit inner page table value in virtual address should be added to the frame number coming from the outer page table. Same as the outer level page table, the 4-bit should be shifted by 2.

| 6-bit Frame Number | Not used | Valid Bit |
| --- | --- | --- |

## Physical Addresses

The physical address size is 12 bits.

## Page Table Register File

This special register points to the start of the outer page table. It is a 12-bit register.

## Implementation

The function correctly translates virtual addresses to physical addresses using the 2-level page table structure. If the page does not contain a valid frame number (the valid bit is “0”), the function prints out “0” in the output, otherwise it prints out “1”. The same applies to both outer and inner level page tables. In case the physical address cannot be retrieved, we print `0x000` and `0x00000000` for the physical address and the memory value, respectively.

The output file has the following format for each memory request:

```
0/1, 0/1, 0xPhysical_address(12-bits), 0xMemory_value(32-bits)
```

An example of queries to the page table (“pt\_requests.txt”), initialization of physical memory (“pt\_initialize.txt”) and paget table register (“PTBR.txt”), and the output file (“pt\_results.txt”) are provided.

The code can be executed as follows:

```
./PageTable pt_requests.txt PTBR.txt
```