# Program header
The program header table is an array of program header structures, each containing 0 or more sections. Program headers tell the OS what permissions should the sections of a segment have. It also describes where in the virtual and/or physical memory should each individual segment exist. For ELF64 binary files, The **p_flags** field is located as the second field, whereas the ELF32 format holds this field in the seventh position.

## *p_type* field
### Datatype: uint32_t (4 bytes)
The field describes the kind of segment the element is and how to interpret the element's information:
- PT_NULL (0x00) = The program header element is unused and it enables this program header entry to be ignored.
- PT_LOAD (0x01) = The entry type is a loadable segment, which tells the linker that this particular segment should be allocated with specific permissions. It can contain many sections, including:
	- `.init`
	- `.text`
	- `.interp`
	- `.got`
	- `.got.plt`
	- `.data`
	- `.bss`
- PT_DYNAMIC (0x02) = The entry type is a dynamic linking segment.
- PT_INTERP (0x03) = The entry type contains program interpreter information with which the program will be executed. This segment usually only has one section, which is `.interp`. This entry should precede any segment entry of type `LOAD`.
- PT_NOTE (0x04) = The entry type contains auxilliary information used by the system or extensions, like the GNU tool chain.
- PT_SHLIB (0x05) = This entry type is reserved.
- PT_PHDR (0x06) = This entry type contains the program header table size and location.
- PT_TLS (0x07) = This type of entry describes the Thread-Local Storage Template.

## *p_flags* field
### Datatype: uint32_t (4 bytes)
For ELF64, this entry is the second entry in the array, right after *p_type*, but for ELF32 binaries, it is the seventh element in the array. The entry holds the permissions for the particular segment in a bit mask format, which is 0bRWX (R for READ, W for WRITE, X for execute). Simply put, enabling Read-Only for a segment is done by setting the *p_flags* field to 0x4, which in binary is 0b100, giving us only the READ flag turned on.

## *p_offset* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field is used as an offset from the beginning of the file at which the first byte of the segment exists.

## *p_vaddr* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field holds the virtual address of the first byte in memory.

## *p_paddr* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field holds the segments physical address of the first byte in memory.

## *p_filesz* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field holds the number of bytes in the file memory. This

## *p_memsz* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field holds the byte size of the segment in memory.

## *p_align* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field holds the alignment for the next program header.
