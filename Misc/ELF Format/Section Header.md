# Section header
The section header entries contain all information needed for linking. Entries can contain executable code, initialized and uninitialized data, pointers, the global offset table and more.

## *sh_name* field
### Datatype: uint32_t (4 bytes)
This field specifies the name of the section. Its value is an index of the section header string table section array, which has a null-terminated string.

## *sh_type* field
### Datatype: uint32_t (4 bytes)
This field describes the sections content type:
- SHT_NULL (0x00) = Section is inactive.
- SHT_PROGBITS (0x01) = Section holds information defined by the program.
- SHT_SYMTAB (0x02) = Section holds a symbol table.
- SHT_STRTAB (0x03) = Section holds a string table.
- SHT_RELA (0x04) = Section holds relocation entries.
- SHT_HASH (0x05) = Section holds a symbol hash table used in dynamic linking.
- SHT_DYNAMIC (0x06) = Section holds information for dynamic linking.
- SHT_NOTE (0x07) = Section holds notes.
- SHT_NOBITS (0x08) = *Not really sure*
- SHT_REL (0x09) = Section holds relocation offsets, *Not really sure what this means*.
- SHT_SHLIB (0x0A) = Section is reserved.

## *sh_flags* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field holds one-bit flags that has miscellaneous attributes. The flags are set via a bit mask:
- SHF_WRITE (1000) = Sections contains data that should be writable during process execution.
- SHF_ALLOC (0100) = Section allocates memory during execution.
- SHF_EXECINSTR (0010) = Section contains execution instructions.
- SHF_MASKPROC (0001) = Reserved for processor-specific semantics.

## *sh_addr* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This fields holds the virtual address of the first byte of the section.

## *sh_offset* field
### Datatype: 32/64 bit address (4 or 8 bytes)
The field holds the offset from the beginning of the file to the first byte in the section.

## *sh_size* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field holds the section's size in bytes.

## *sh_link* field
### Datatype: uint32_t (4 bytes)
*I do not know what this field means.*

## *sh_info* field
### Datatype: uint32_t (4 bytes)
This is similar to *sh_link*.

## *sh_addralign* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field describes the alignment of bytes in virtual memory.

## *sh_entsize* field
### Datatype: uint32/64_t (4 or 8 bytes)
This field specifies the optional entries size contained in the section. This may be zero.
