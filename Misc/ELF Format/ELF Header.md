# ELF Header
The ELF Header consists of 14 fields and starts at the beginning of the file. The fields are the following:

## *e_ident* field
### Datatype: char\[16\]
The e_indent field is an array of chars (1 bytes) that specify how to interpret the file. The array size is 16 and the elements mean the following:
- `e_ident[0]` = First magic byte (**0x7F**)
- `e_ident[1]` = Second magic byte (**0x45** - **"E"**)
- `e_ident[2]` = Third magic byte (**0x4c** - **"L"**)
- `e_ident[3]` = Fourth magic byte (**0x46** - **"F"**)
- `e_ident[4]` = The architecture used for the binary:
	- ELFCLASSNONE (0x00) - The architecture is **invalid**.
	- ELFCLASS32 (0x01) - The architecture is **32 bit**.
	- ELFCLASS64 (0x02) - The arch is **64 bit**.
- `e_ident[5]` = The data encoding of the binary:
	- ELFDATANONE (0x00) - Unknown data format.
	- ELFDATA2LSB (0x01) - 2's complement, least significant byte (little-endian).
	- ELFDATA2MSB (0x02) - 2's complement, most significant byte (big-endian).
- `e_ident[6]` = The version of the ELF binary:
	- EV_NONE (0x00) = Invalid version.
	- EV_CURRENT (0x01) = Current version.
- `e_ident[7]` = The OS ABI of the binary (Some operating systems have special flags and values that have platform-specific meanings):
	- ELFOSABI_NONE (0x00) = Unix System V ABI
	- ELFOSABI_SYSV (0x01) = Unix System V ABI
	- ELFOSABI_HPUX (0x02) = HP-UX ABI
	- ELFOSABI_NETBSD (0x03) = NetBSD ABI
	- ELFOSABI_LINUX (0x04) = Linux ABI
	- ELFOSABI_SOLARIS (0x05) = Solaris ABI
	- ELFOSABI_IRIX (0x06) = IRIX ABI
	- ELFOSABI_FREEBSD (0x07) = FreeBSD ABI
	- ELFOSABI_TRU64 (0x08) = TRU64 UNIX ABI
	- ELFOSABI_ARM (0x09) = ARM Architecture ABI
	- ELFOSABI_STANDALONE (0x0A) = Stand-alone (embedded) ABI
- `e_ident[8]` = The ABI version (usually 0).
- `e_ident[9]` = Start of padding (this is not used, usually 0).
- `e_ident[10]` = The size of the `e_indent` array (usually 0).
- `e_ident[11-15]` = Not used, set to 0.

## *e_type* field
### Datatype: uint16_t (2 bytes)
This field describes the object file type:
- ET_NONE (0x00) = An unknown type.
- ET_REL (0x01) = A relocatable file.
	- A relocatable file is an object file 
- ET_EXEC (0x02) = An executable file.
	- An executable file type allows the user to execute the file as a program.
- ET_DYN (0x03) = A shared object file.
- ET_CORE (0x04) = A core file.

## *e_machine* field
### Datatype: uint16_t (2 bytes)
This field specifies the CPU architecture for the file. There are [many](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format#File_header) CPU Architectures to choose from, so I will include the most popular ones:
- EM_386 (0x03) = Intel i386 (x86) ISA.
- EM_ARM (0x28) = ARM (Aarch32) ISA.
- EM_AMD (0x3E) = AMD x86-64 ISA.
- EM_ARM (0xB7) = Newer ARM (Aarch64) ISA.

## *e_version* field
### Datatype: uint32_t (4 bytes)
This field identifies the file version:
- EV_NONE (0x00) = Invalid version.
- EV_CURRENT (0x01) = Current version.

## *e_entry* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field describes the start virtual address of the entry address, the **\_start** section is set by default (you can change the point address by adding an **-e <symbol/v_address>** option).

## *e_phoff* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field describes the offset address for the program header table.

## *e_shoff* field
### Datatype: 32/64 bit address (4 or 8 bytes)
This field describes the offset address for the section header table.

## *e_flags* field
### Datatype: uint32_t (4 bytes)
This field holds processor-specific flags that are associated with the file.

## *e_ehsize* field
### Datatype: uint16_t (2 bytes)
The ELF header size in bytes.

## *e_phentsize* field
### Datatype: uint16_t (2 bytes)
The size of one program header in bytes (all program header sizes will be the same size).

## *e_phnum* field
### Datatype: uint16_t (2 bytes)
This field describes the number of program headers that exist within the file.

## *e_shentsize* field
### Datatype: uint16_t (2 bytes)
The section header entry size in bytes (all section headers will be the same size).

## *e_shnum* field
### Datatype: uint16_t (2 bytes)
This field holds the number of section header entries that exist within the file.

## *e_shstrndx* field
### Datatype: uint16_t (2 bytes)
This field is responsible for holding the section header table index of the section name string table.

## Resources
- https://docs.oracle.com/cd/E19683-01/817-3677/chapter6-46512/index.html
- https://man7.org/linux/man-pages/man5/elf.5.html
- https://en.wikipedia.org/wiki/Executable_and_Linkable_Format#File_header
