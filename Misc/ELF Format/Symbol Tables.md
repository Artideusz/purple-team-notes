# Symbol table
The symbol table holds information about a reference to some type of data or code such as a global variable or function. ELF32 format has different positions in fields in comparison to ELF64. A symbol table entry has the following fieds (ELF64):

## *st_name* field
### Datatype: uint32_t (4 bytes)
This field describes the index of the symbol table to a symbol name.

## *st_info* field
### Datatype: unsigned char (1 byte)
In ELF32, this field is **st_value**. This field specifies the symbol's type:
- STT_NOTYPE (0x00) = It is undefined.
- STT_OBJECT (0x01) = Data object.
- STT_FUNC (0x02) = Function.
- STT_SECTION (0x03) = Section, used for relocation.
- STT_FILE (0x04) = Section's name is a file.

## *st_other* field
### Datatype: unsigned char (1 byte)
This field describes the symbol visibility:
- STV_DEFAULT (0x00) = Global and weak symbols are available to other modules. References to the symbol can be used in other modules.
- STV_INTERNAL (0x01) = Processor-specific hidden class.
- STV_HIDDEN (0x02) = Symbol is unavailable to other modules. References to the symbol can be used only in the module where the symbol is defined.
- STV_PROTECTED (0x03) = Symbol is available to other modules, but references resolve to the module where the symbol is defined.

## *st_shndx* field
### Datatype: uint16_t (2 bytes)
This field holds the relevant section header table index.

## *st_value* field
### Datatype: 64 bit address (8 bytes)
This field gives the value of the associated symbol.

## *st_size* field
### Datatype: uint64_t (8 bytes)
The symbol entry size. Size is 0 if the symbol has no size or the size is unknown.