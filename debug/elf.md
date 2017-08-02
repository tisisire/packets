
 Package elf

    import "debug/elf"

    Overview
    Index

Overview ▾

Package elf implements access to ELF object files.
Index ▾

    Constants
    Variables
    func R_INFO(sym, typ uint32) uint64
    func R_INFO32(sym, typ uint32) uint32
    func R_SYM32(info uint32) uint32
    func R_SYM64(info uint64) uint32
    func R_TYPE32(info uint32) uint32
    func R_TYPE64(info uint64) uint32
    func ST_INFO(bind SymBind, typ SymType) uint8
    type Chdr32
    type Chdr64
    type Class
        func (i Class) GoString() string
        func (i Class) String() string
    type CompressionType
        func (i CompressionType) GoString() string
        func (i CompressionType) String() string
    type Data
        func (i Data) GoString() string
        func (i Data) String() string
    type Dyn32
    type Dyn64
    type DynFlag
        func (i DynFlag) GoString() string
        func (i DynFlag) String() string
    type DynTag
        func (i DynTag) GoString() string
        func (i DynTag) String() string
    type File
        func NewFile(r io.ReaderAt) (*File, error)
        func Open(name string) (*File, error)
        func (f *File) Close() error
        func (f *File) DWARF() (*dwarf.Data, error)
        func (f *File) DynString(tag DynTag) ([]string, error)
        func (f *File) DynamicSymbols() ([]Symbol, error)
        func (f *File) ImportedLibraries() ([]string, error)
        func (f *File) ImportedSymbols() ([]ImportedSymbol, error)
        func (f *File) Section(name string) *Section
        func (f *File) SectionByType(typ SectionType) *Section
        func (f *File) Symbols() ([]Symbol, error)
    type FileHeader
    type FormatError
        func (e *FormatError) Error() string
    type Header32
    type Header64
    type ImportedSymbol
    type Machine
        func (i Machine) GoString() string
        func (i Machine) String() string
    type NType
        func (i NType) GoString() string
        func (i NType) String() string
    type OSABI
        func (i OSABI) GoString() string
        func (i OSABI) String() string
    type Prog
        func (p *Prog) Open() io.ReadSeeker
    type Prog32
    type Prog64
    type ProgFlag
        func (i ProgFlag) GoString() string
        func (i ProgFlag) String() string
    type ProgHeader
    type ProgType
        func (i ProgType) GoString() string
        func (i ProgType) String() string
    type R_386
        func (i R_386) GoString() string
        func (i R_386) String() string
    type R_390
        func (i R_390) GoString() string
        func (i R_390) String() string
    type R_AARCH64
        func (i R_AARCH64) GoString() string
        func (i R_AARCH64) String() string
    type R_ALPHA
        func (i R_ALPHA) GoString() string
        func (i R_ALPHA) String() string
    type R_ARM
        func (i R_ARM) GoString() string
        func (i R_ARM) String() string
    type R_MIPS
        func (i R_MIPS) GoString() string
        func (i R_MIPS) String() string
    type R_PPC
        func (i R_PPC) GoString() string
        func (i R_PPC) String() string
    type R_PPC64
        func (i R_PPC64) GoString() string
        func (i R_PPC64) String() string
    type R_SPARC
        func (i R_SPARC) GoString() string
        func (i R_SPARC) String() string
    type R_X86_64
        func (i R_X86_64) GoString() string
        func (i R_X86_64) String() string
    type Rel32
    type Rel64
    type Rela32
    type Rela64
    type Section
        func (s *Section) Data() ([]byte, error)
        func (s *Section) Open() io.ReadSeeker
    type Section32
    type Section64
    type SectionFlag
        func (i SectionFlag) GoString() string
        func (i SectionFlag) String() string
    type SectionHeader
    type SectionIndex
        func (i SectionIndex) GoString() string
        func (i SectionIndex) String() string
    type SectionType
        func (i SectionType) GoString() string
        func (i SectionType) String() string
    type Sym32
    type Sym64
    type SymBind
        func ST_BIND(info uint8) SymBind
        func (i SymBind) GoString() string
        func (i SymBind) String() string
    type SymType
        func ST_TYPE(info uint8) SymType
        func (i SymType) GoString() string
        func (i SymType) String() string
    type SymVis
        func ST_VISIBILITY(other uint8) SymVis
        func (i SymVis) GoString() string
        func (i SymVis) String() string
    type Symbol
    type Type
        func (i Type) GoString() string
        func (i Type) String() string
    type Version
        func (i Version) GoString() string
        func (i Version) String() string

Package files

elf.go file.go reader.go
Constants

Indexes into the Header.Ident array.

const (
        EI_CLASS      = 4  /* Class of machine. */
        EI_DATA       = 5  /* Data format. */
        EI_VERSION    = 6  /* ELF format version. */
        EI_OSABI      = 7  /* Operating system / ABI identification */
        EI_ABIVERSION = 8  /* ABI version */
        EI_PAD        = 9  /* Start of padding (per SVR4 ABI). */
        EI_NIDENT     = 16 /* Size of e_ident array. */
)

Magic number for the elf trampoline, chosen wisely to be an immediate value.

const ARM_MAGIC_TRAMP_NUMBER = 0x5c000003

Initial magic number for ELF files.

const ELFMAG = "\177ELF"

const Sym32Size = 16

const Sym64Size = 24

Variables

ErrNoSymbols is returned by File.Symbols and File.DynamicSymbols if there is no such section in the File.

var ErrNoSymbols = errors.New("no symbol section")

func R_INFO

func R_INFO(sym, typ uint32) uint64

func R_INFO32

func R_INFO32(sym, typ uint32) uint32

func R_SYM32

func R_SYM32(info uint32) uint32

func R_SYM64

func R_SYM64(info uint64) uint32

func R_TYPE32

func R_TYPE32(info uint32) uint32

func R_TYPE64

func R_TYPE64(info uint64) uint32

func ST_INFO

func ST_INFO(bind SymBind, typ SymType) uint8

type Chdr32

ELF32 Compression header.

type Chdr32 struct {
        Type      uint32
        Size      uint32
        Addralign uint32
}

type Chdr64

ELF64 Compression header.

type Chdr64 struct {
        Type uint32

        Size      uint64
        Addralign uint64
        // contains filtered or unexported fields
}

type Class

Class is found in Header.Ident[EI_CLASS] and Header.Class.

type Class byte

const (
        ELFCLASSNONE Class = 0 /* Unknown class. */
        ELFCLASS32   Class = 1 /* 32-bit architecture. */
        ELFCLASS64   Class = 2 /* 64-bit architecture. */
)

func (Class) GoString

func (i Class) GoString() string

func (Class) String

func (i Class) String() string

type CompressionType

Section compression type.

type CompressionType int

const (
        COMPRESS_ZLIB   CompressionType = 1          /* ZLIB compression. */
        COMPRESS_LOOS   CompressionType = 0x60000000 /* First OS-specific. */
        COMPRESS_HIOS   CompressionType = 0x6fffffff /* Last OS-specific. */
        COMPRESS_LOPROC CompressionType = 0x70000000 /* First processor-specific type. */
        COMPRESS_HIPROC CompressionType = 0x7fffffff /* Last processor-specific type. */
)

func (CompressionType) GoString

func (i CompressionType) GoString() string

func (CompressionType) String

func (i CompressionType) String() string

type Data

Data is found in Header.Ident[EI_DATA] and Header.Data.

type Data byte

const (
        ELFDATANONE Data = 0 /* Unknown data format. */
        ELFDATA2LSB Data = 1 /* 2's complement little-endian. */
        ELFDATA2MSB Data = 2 /* 2's complement big-endian. */
)

func (Data) GoString

func (i Data) GoString() string

func (Data) String

func (i Data) String() string

type Dyn32

ELF32 Dynamic structure. The ".dynamic" section contains an array of them.

type Dyn32 struct {
        Tag int32  /* Entry type. */
        Val uint32 /* Integer/Address value. */
}

type Dyn64

ELF64 Dynamic structure. The ".dynamic" section contains an array of them.

type Dyn64 struct {
        Tag int64  /* Entry type. */
        Val uint64 /* Integer/address value */
}

type DynFlag

DT_FLAGS values.

type DynFlag int

const (
        DF_ORIGIN DynFlag = 0x0001 /* Indicates that the object being loaded may
           make reference to the
           $ORIGIN substitution string */
        DF_SYMBOLIC DynFlag = 0x0002 /* Indicates "symbolic" linking. */
        DF_TEXTREL  DynFlag = 0x0004 /* Indicates there may be relocations in non-writable segments. */
        DF_BIND_NOW DynFlag = 0x0008 /* Indicates that the dynamic linker should
           process all relocations for the object
           containing this entry before transferring
           control to the program. */
        DF_STATIC_TLS DynFlag = 0x0010 /* Indicates that the shared object or
           executable contains code using a static
           thread-local storage scheme. */
)

func (DynFlag) GoString

func (i DynFlag) GoString() string

func (DynFlag) String

func (i DynFlag) String() string

type DynTag

Dyn.Tag

type DynTag int

const (
        DT_NULL         DynTag = 0  /* Terminating entry. */
        DT_NEEDED       DynTag = 1  /* String table offset of a needed shared library. */
        DT_PLTRELSZ     DynTag = 2  /* Total size in bytes of PLT relocations. */
        DT_PLTGOT       DynTag = 3  /* Processor-dependent address. */
        DT_HASH         DynTag = 4  /* Address of symbol hash table. */
        DT_STRTAB       DynTag = 5  /* Address of string table. */
        DT_SYMTAB       DynTag = 6  /* Address of symbol table. */
        DT_RELA         DynTag = 7  /* Address of ElfNN_Rela relocations. */
        DT_RELASZ       DynTag = 8  /* Total size of ElfNN_Rela relocations. */
        DT_RELAENT      DynTag = 9  /* Size of each ElfNN_Rela relocation entry. */
        DT_STRSZ        DynTag = 10 /* Size of string table. */
        DT_SYMENT       DynTag = 11 /* Size of each symbol table entry. */
        DT_INIT         DynTag = 12 /* Address of initialization function. */
        DT_FINI         DynTag = 13 /* Address of finalization function. */
        DT_SONAME       DynTag = 14 /* String table offset of shared object name. */
        DT_RPATH        DynTag = 15 /* String table offset of library path. [sup] */
        DT_SYMBOLIC     DynTag = 16 /* Indicates "symbolic" linking. [sup] */
        DT_REL          DynTag = 17 /* Address of ElfNN_Rel relocations. */
        DT_RELSZ        DynTag = 18 /* Total size of ElfNN_Rel relocations. */
        DT_RELENT       DynTag = 19 /* Size of each ElfNN_Rel relocation. */
        DT_PLTREL       DynTag = 20 /* Type of relocation used for PLT. */
        DT_DEBUG        DynTag = 21 /* Reserved (not used). */
        DT_TEXTREL      DynTag = 22 /* Indicates there may be relocations in non-writable segments. [sup] */
        DT_JMPREL       DynTag = 23 /* Address of PLT relocations. */
        DT_BIND_NOW     DynTag = 24 /* [sup] */
        DT_INIT_ARRAY   DynTag = 25 /* Address of the array of pointers to initialization functions */
        DT_FINI_ARRAY   DynTag = 26 /* Address of the array of pointers to termination functions */
        DT_INIT_ARRAYSZ DynTag = 27 /* Size in bytes of the array of initialization functions. */
        DT_FINI_ARRAYSZ DynTag = 28 /* Size in bytes of the array of termination functions. */
        DT_RUNPATH      DynTag = 29 /* String table offset of a null-terminated library search path string. */
        DT_FLAGS        DynTag = 30 /* Object specific flag values. */
        DT_ENCODING     DynTag = 32 /* Values greater than or equal to DT_ENCODING
           and less than DT_LOOS follow the rules for
           the interpretation of the d_un union
           as follows: even == 'd_ptr', even == 'd_val'
           or none */
        DT_PREINIT_ARRAY   DynTag = 32         /* Address of the array of pointers to pre-initialization functions. */
        DT_PREINIT_ARRAYSZ DynTag = 33         /* Size in bytes of the array of pre-initialization functions. */
        DT_LOOS            DynTag = 0x6000000d /* First OS-specific */
        DT_HIOS            DynTag = 0x6ffff000 /* Last OS-specific */
        DT_VERSYM          DynTag = 0x6ffffff0
        DT_VERNEED         DynTag = 0x6ffffffe
        DT_VERNEEDNUM      DynTag = 0x6fffffff
        DT_LOPROC          DynTag = 0x70000000 /* First processor-specific type. */
        DT_HIPROC          DynTag = 0x7fffffff /* Last processor-specific type. */
)

func (DynTag) GoString

func (i DynTag) GoString() string

func (DynTag) String

func (i DynTag) String() string

type File

A File represents an open ELF file.

type File struct {
        FileHeader
        Sections []*Section
        Progs    []*Prog
        // contains filtered or unexported fields
}

func NewFile

func NewFile(r io.ReaderAt) (*File, error)

NewFile creates a new File for accessing an ELF binary in an underlying reader. The ELF binary is expected to start at position 0 in the ReaderAt.
func Open

func Open(name string) (*File, error)

Open opens the named file using os.Open and prepares it for use as an ELF binary.
func (*File) Close

func (f *File) Close() error

Close closes the File. If the File was created using NewFile directly instead of Open, Close has no effect.
func (*File) DWARF

func (f *File) DWARF() (*dwarf.Data, error)

func (*File) DynString

func (f *File) DynString(tag DynTag) ([]string, error)

DynString returns the strings listed for the given tag in the file's dynamic section.

The tag must be one that takes string values: DT_NEEDED, DT_SONAME, DT_RPATH, or DT_RUNPATH.
func (*File) DynamicSymbols

func (f *File) DynamicSymbols() ([]Symbol, error)

DynamicSymbols returns the dynamic symbol table for f. The symbols will be listed in the order they appear in f.

For compatibility with Symbols, DynamicSymbols omits the null symbol at index 0. After retrieving the symbols as symtab, an externally supplied index x corresponds to symtab[x-1], not symtab[x].
func (*File) ImportedLibraries

func (f *File) ImportedLibraries() ([]string, error)

ImportedLibraries returns the names of all libraries referred to by the binary f that are expected to be linked with the binary at dynamic link time.
func (*File) ImportedSymbols

func (f *File) ImportedSymbols() ([]ImportedSymbol, error)

ImportedSymbols returns the names of all symbols referred to by the binary f that are expected to be satisfied by other libraries at dynamic load time. It does not return weak symbols.
func (*File) Section

func (f *File) Section(name string) *Section

Section returns a section with the given name, or nil if no such section exists.
func (*File) SectionByType

func (f *File) SectionByType(typ SectionType) *Section

SectionByType returns the first section in f with the given type, or nil if there is no such section.
func (*File) Symbols

func (f *File) Symbols() ([]Symbol, error)

Symbols returns the symbol table for f. The symbols will be listed in the order they appear in f.

For compatibility with Go 1.0, Symbols omits the null symbol at index 0. After retrieving the symbols as symtab, an externally supplied index x corresponds to symtab[x-1], not symtab[x].
type FileHeader

A FileHeader represents an ELF file header.

type FileHeader struct {
        Class      Class
        Data       Data
        Version    Version
        OSABI      OSABI
        ABIVersion uint8
        ByteOrder  binary.ByteOrder
        Type       Type
        Machine    Machine
        Entry      uint64
}

type FormatError

type FormatError struct {
        // contains filtered or unexported fields
}

func (*FormatError) Error

func (e *FormatError) Error() string

type Header32

ELF32 File header.

type Header32 struct {
        Ident     [EI_NIDENT]byte /* File identification. */
        Type      uint16          /* File type. */
        Machine   uint16          /* Machine architecture. */
        Version   uint32          /* ELF format version. */
        Entry     uint32          /* Entry point. */
        Phoff     uint32          /* Program header file offset. */
        Shoff     uint32          /* Section header file offset. */
        Flags     uint32          /* Architecture-specific flags. */
        Ehsize    uint16          /* Size of ELF header in bytes. */
        Phentsize uint16          /* Size of program header entry. */
        Phnum     uint16          /* Number of program header entries. */
        Shentsize uint16          /* Size of section header entry. */
        Shnum     uint16          /* Number of section header entries. */
        Shstrndx  uint16          /* Section name strings section. */
}

type Header64

ELF64 file header.

type Header64 struct {
        Ident     [EI_NIDENT]byte /* File identification. */
        Type      uint16          /* File type. */
        Machine   uint16          /* Machine architecture. */
        Version   uint32          /* ELF format version. */
        Entry     uint64          /* Entry point. */
        Phoff     uint64          /* Program header file offset. */
        Shoff     uint64          /* Section header file offset. */
        Flags     uint32          /* Architecture-specific flags. */
        Ehsize    uint16          /* Size of ELF header in bytes. */
        Phentsize uint16          /* Size of program header entry. */
        Phnum     uint16          /* Number of program header entries. */
        Shentsize uint16          /* Size of section header entry. */
        Shnum     uint16          /* Number of section header entries. */
        Shstrndx  uint16          /* Section name strings section. */
}

type ImportedSymbol

type ImportedSymbol struct {
        Name    string
        Version string
        Library string
}

type Machine

Machine is found in Header.Machine.

type Machine uint16

const (
        EM_NONE        Machine = 0   /* Unknown machine. */
        EM_M32         Machine = 1   /* AT&T WE32100. */
        EM_SPARC       Machine = 2   /* Sun SPARC. */
        EM_386         Machine = 3   /* Intel i386. */
        EM_68K         Machine = 4   /* Motorola 68000. */
        EM_88K         Machine = 5   /* Motorola 88000. */
        EM_860         Machine = 7   /* Intel i860. */
        EM_MIPS        Machine = 8   /* MIPS R3000 Big-Endian only. */
        EM_S370        Machine = 9   /* IBM System/370. */
        EM_MIPS_RS3_LE Machine = 10  /* MIPS R3000 Little-Endian. */
        EM_PARISC      Machine = 15  /* HP PA-RISC. */
        EM_VPP500      Machine = 17  /* Fujitsu VPP500. */
        EM_SPARC32PLUS Machine = 18  /* SPARC v8plus. */
        EM_960         Machine = 19  /* Intel 80960. */
        EM_PPC         Machine = 20  /* PowerPC 32-bit. */
        EM_PPC64       Machine = 21  /* PowerPC 64-bit. */
        EM_S390        Machine = 22  /* IBM System/390. */
        EM_V800        Machine = 36  /* NEC V800. */
        EM_FR20        Machine = 37  /* Fujitsu FR20. */
        EM_RH32        Machine = 38  /* TRW RH-32. */
        EM_RCE         Machine = 39  /* Motorola RCE. */
        EM_ARM         Machine = 40  /* ARM. */
        EM_SH          Machine = 42  /* Hitachi SH. */
        EM_SPARCV9     Machine = 43  /* SPARC v9 64-bit. */
        EM_TRICORE     Machine = 44  /* Siemens TriCore embedded processor. */
        EM_ARC         Machine = 45  /* Argonaut RISC Core. */
        EM_H8_300      Machine = 46  /* Hitachi H8/300. */
        EM_H8_300H     Machine = 47  /* Hitachi H8/300H. */
        EM_H8S         Machine = 48  /* Hitachi H8S. */
        EM_H8_500      Machine = 49  /* Hitachi H8/500. */
        EM_IA_64       Machine = 50  /* Intel IA-64 Processor. */
        EM_MIPS_X      Machine = 51  /* Stanford MIPS-X. */
        EM_COLDFIRE    Machine = 52  /* Motorola ColdFire. */
        EM_68HC12      Machine = 53  /* Motorola M68HC12. */
        EM_MMA         Machine = 54  /* Fujitsu MMA. */
        EM_PCP         Machine = 55  /* Siemens PCP. */
        EM_NCPU        Machine = 56  /* Sony nCPU. */
        EM_NDR1        Machine = 57  /* Denso NDR1 microprocessor. */
        EM_STARCORE    Machine = 58  /* Motorola Star*Core processor. */
        EM_ME16        Machine = 59  /* Toyota ME16 processor. */
        EM_ST100       Machine = 60  /* STMicroelectronics ST100 processor. */
        EM_TINYJ       Machine = 61  /* Advanced Logic Corp. TinyJ processor. */
        EM_X86_64      Machine = 62  /* Advanced Micro Devices x86-64 */
        EM_AARCH64     Machine = 183 /* ARM 64-bit Architecture (AArch64) */

        /* Non-standard or deprecated. */
        EM_486         Machine = 6      /* Intel i486. */
        EM_MIPS_RS4_BE Machine = 10     /* MIPS R4000 Big-Endian */
        EM_ALPHA_STD   Machine = 41     /* Digital Alpha (standard value). */
        EM_ALPHA       Machine = 0x9026 /* Alpha (written in the absence of an ABI) */
)

func (Machine) GoString

func (i Machine) GoString() string

func (Machine) String

func (i Machine) String() string

type NType

NType values; used in core files.

type NType int

const (
        NT_PRSTATUS NType = 1 /* Process status. */
        NT_FPREGSET NType = 2 /* Floating point registers. */
        NT_PRPSINFO NType = 3 /* Process state info. */
)

func (NType) GoString

func (i NType) GoString() string

func (NType) String

func (i NType) String() string

type OSABI

OSABI is found in Header.Ident[EI_OSABI] and Header.OSABI.

type OSABI byte

const (
        ELFOSABI_NONE       OSABI = 0   /* UNIX System V ABI */
        ELFOSABI_HPUX       OSABI = 1   /* HP-UX operating system */
        ELFOSABI_NETBSD     OSABI = 2   /* NetBSD */
        ELFOSABI_LINUX      OSABI = 3   /* GNU/Linux */
        ELFOSABI_HURD       OSABI = 4   /* GNU/Hurd */
        ELFOSABI_86OPEN     OSABI = 5   /* 86Open common IA32 ABI */
        ELFOSABI_SOLARIS    OSABI = 6   /* Solaris */
        ELFOSABI_AIX        OSABI = 7   /* AIX */
        ELFOSABI_IRIX       OSABI = 8   /* IRIX */
        ELFOSABI_FREEBSD    OSABI = 9   /* FreeBSD */
        ELFOSABI_TRU64      OSABI = 10  /* TRU64 UNIX */
        ELFOSABI_MODESTO    OSABI = 11  /* Novell Modesto */
        ELFOSABI_OPENBSD    OSABI = 12  /* OpenBSD */
        ELFOSABI_OPENVMS    OSABI = 13  /* Open VMS */
        ELFOSABI_NSK        OSABI = 14  /* HP Non-Stop Kernel */
        ELFOSABI_ARM        OSABI = 97  /* ARM */
        ELFOSABI_STANDALONE OSABI = 255 /* Standalone (embedded) application */
)

func (OSABI) GoString

func (i OSABI) GoString() string

func (OSABI) String

func (i OSABI) String() string

type Prog

A Prog represents a single ELF program header in an ELF binary.

type Prog struct {
        ProgHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        io.ReaderAt
        // contains filtered or unexported fields
}

func (*Prog) Open

func (p *Prog) Open() io.ReadSeeker

Open returns a new ReadSeeker reading the ELF program body.
type Prog32

ELF32 Program header.

type Prog32 struct {
        Type   uint32 /* Entry type. */
        Off    uint32 /* File offset of contents. */
        Vaddr  uint32 /* Virtual address in memory image. */
        Paddr  uint32 /* Physical address (not used). */
        Filesz uint32 /* Size of contents in file. */
        Memsz  uint32 /* Size of contents in memory. */
        Flags  uint32 /* Access permission flags. */
        Align  uint32 /* Alignment in memory and file. */
}

type Prog64

ELF64 Program header.

type Prog64 struct {
        Type   uint32 /* Entry type. */
        Flags  uint32 /* Access permission flags. */
        Off    uint64 /* File offset of contents. */
        Vaddr  uint64 /* Virtual address in memory image. */
        Paddr  uint64 /* Physical address (not used). */
        Filesz uint64 /* Size of contents in file. */
        Memsz  uint64 /* Size of contents in memory. */
        Align  uint64 /* Alignment in memory and file. */
}

type ProgFlag

Prog.Flag

type ProgFlag uint32

const (
        PF_X        ProgFlag = 0x1        /* Executable. */
        PF_W        ProgFlag = 0x2        /* Writable. */
        PF_R        ProgFlag = 0x4        /* Readable. */
        PF_MASKOS   ProgFlag = 0x0ff00000 /* Operating system-specific. */
        PF_MASKPROC ProgFlag = 0xf0000000 /* Processor-specific. */
)

func (ProgFlag) GoString

func (i ProgFlag) GoString() string

func (ProgFlag) String

func (i ProgFlag) String() string

type ProgHeader

A ProgHeader represents a single ELF program header.

type ProgHeader struct {
        Type   ProgType
        Flags  ProgFlag
        Off    uint64
        Vaddr  uint64
        Paddr  uint64
        Filesz uint64
        Memsz  uint64
        Align  uint64
}

type ProgType

Prog.Type

type ProgType int

const (
        PT_NULL    ProgType = 0          /* Unused entry. */
        PT_LOAD    ProgType = 1          /* Loadable segment. */
        PT_DYNAMIC ProgType = 2          /* Dynamic linking information segment. */
        PT_INTERP  ProgType = 3          /* Pathname of interpreter. */
        PT_NOTE    ProgType = 4          /* Auxiliary information. */
        PT_SHLIB   ProgType = 5          /* Reserved (not used). */
        PT_PHDR    ProgType = 6          /* Location of program header itself. */
        PT_TLS     ProgType = 7          /* Thread local storage segment */
        PT_LOOS    ProgType = 0x60000000 /* First OS-specific. */
        PT_HIOS    ProgType = 0x6fffffff /* Last OS-specific. */
        PT_LOPROC  ProgType = 0x70000000 /* First processor-specific type. */
        PT_HIPROC  ProgType = 0x7fffffff /* Last processor-specific type. */
)

func (ProgType) GoString

func (i ProgType) GoString() string

func (ProgType) String

func (i ProgType) String() string

type R_386

Relocation types for 386.

type R_386 int

const (
        R_386_NONE         R_386 = 0  /* No relocation. */
        R_386_32           R_386 = 1  /* Add symbol value. */
        R_386_PC32         R_386 = 2  /* Add PC-relative symbol value. */
        R_386_GOT32        R_386 = 3  /* Add PC-relative GOT offset. */
        R_386_PLT32        R_386 = 4  /* Add PC-relative PLT offset. */
        R_386_COPY         R_386 = 5  /* Copy data from shared object. */
        R_386_GLOB_DAT     R_386 = 6  /* Set GOT entry to data address. */
        R_386_JMP_SLOT     R_386 = 7  /* Set GOT entry to code address. */
        R_386_RELATIVE     R_386 = 8  /* Add load address of shared object. */
        R_386_GOTOFF       R_386 = 9  /* Add GOT-relative symbol address. */
        R_386_GOTPC        R_386 = 10 /* Add PC-relative GOT table address. */
        R_386_TLS_TPOFF    R_386 = 14 /* Negative offset in static TLS block */
        R_386_TLS_IE       R_386 = 15 /* Absolute address of GOT for -ve static TLS */
        R_386_TLS_GOTIE    R_386 = 16 /* GOT entry for negative static TLS block */
        R_386_TLS_LE       R_386 = 17 /* Negative offset relative to static TLS */
        R_386_TLS_GD       R_386 = 18 /* 32 bit offset to GOT (index,off) pair */
        R_386_TLS_LDM      R_386 = 19 /* 32 bit offset to GOT (index,zero) pair */
        R_386_TLS_GD_32    R_386 = 24 /* 32 bit offset to GOT (index,off) pair */
        R_386_TLS_GD_PUSH  R_386 = 25 /* pushl instruction for Sun ABI GD sequence */
        R_386_TLS_GD_CALL  R_386 = 26 /* call instruction for Sun ABI GD sequence */
        R_386_TLS_GD_POP   R_386 = 27 /* popl instruction for Sun ABI GD sequence */
        R_386_TLS_LDM_32   R_386 = 28 /* 32 bit offset to GOT (index,zero) pair */
        R_386_TLS_LDM_PUSH R_386 = 29 /* pushl instruction for Sun ABI LD sequence */
        R_386_TLS_LDM_CALL R_386 = 30 /* call instruction for Sun ABI LD sequence */
        R_386_TLS_LDM_POP  R_386 = 31 /* popl instruction for Sun ABI LD sequence */
        R_386_TLS_LDO_32   R_386 = 32 /* 32 bit offset from start of TLS block */
        R_386_TLS_IE_32    R_386 = 33 /* 32 bit offset to GOT static TLS offset entry */
        R_386_TLS_LE_32    R_386 = 34 /* 32 bit offset within static TLS block */
        R_386_TLS_DTPMOD32 R_386 = 35 /* GOT entry containing TLS index */
        R_386_TLS_DTPOFF32 R_386 = 36 /* GOT entry containing TLS offset */
        R_386_TLS_TPOFF32  R_386 = 37 /* GOT entry of -ve static TLS offset */
)

func (R_386) GoString

func (i R_386) GoString() string

func (R_386) String

func (i R_386) String() string

type R_390

Relocation types for s390x processors.

type R_390 int

const (
        R_390_NONE        R_390 = 0
        R_390_8           R_390 = 1
        R_390_12          R_390 = 2
        R_390_16          R_390 = 3
        R_390_32          R_390 = 4
        R_390_PC32        R_390 = 5
        R_390_GOT12       R_390 = 6
        R_390_GOT32       R_390 = 7
        R_390_PLT32       R_390 = 8
        R_390_COPY        R_390 = 9
        R_390_GLOB_DAT    R_390 = 10
        R_390_JMP_SLOT    R_390 = 11
        R_390_RELATIVE    R_390 = 12
        R_390_GOTOFF      R_390 = 13
        R_390_GOTPC       R_390 = 14
        R_390_GOT16       R_390 = 15
        R_390_PC16        R_390 = 16
        R_390_PC16DBL     R_390 = 17
        R_390_PLT16DBL    R_390 = 18
        R_390_PC32DBL     R_390 = 19
        R_390_PLT32DBL    R_390 = 20
        R_390_GOTPCDBL    R_390 = 21
        R_390_64          R_390 = 22
        R_390_PC64        R_390 = 23
        R_390_GOT64       R_390 = 24
        R_390_PLT64       R_390 = 25
        R_390_GOTENT      R_390 = 26
        R_390_GOTOFF16    R_390 = 27
        R_390_GOTOFF64    R_390 = 28
        R_390_GOTPLT12    R_390 = 29
        R_390_GOTPLT16    R_390 = 30
        R_390_GOTPLT32    R_390 = 31
        R_390_GOTPLT64    R_390 = 32
        R_390_GOTPLTENT   R_390 = 33
        R_390_GOTPLTOFF16 R_390 = 34
        R_390_GOTPLTOFF32 R_390 = 35
        R_390_GOTPLTOFF64 R_390 = 36
        R_390_TLS_LOAD    R_390 = 37
        R_390_TLS_GDCALL  R_390 = 38
        R_390_TLS_LDCALL  R_390 = 39
        R_390_TLS_GD32    R_390 = 40
        R_390_TLS_GD64    R_390 = 41
        R_390_TLS_GOTIE12 R_390 = 42
        R_390_TLS_GOTIE32 R_390 = 43
        R_390_TLS_GOTIE64 R_390 = 44
        R_390_TLS_LDM32   R_390 = 45
        R_390_TLS_LDM64   R_390 = 46
        R_390_TLS_IE32    R_390 = 47
        R_390_TLS_IE64    R_390 = 48
        R_390_TLS_IEENT   R_390 = 49
        R_390_TLS_LE32    R_390 = 50
        R_390_TLS_LE64    R_390 = 51
        R_390_TLS_LDO32   R_390 = 52
        R_390_TLS_LDO64   R_390 = 53
        R_390_TLS_DTPMOD  R_390 = 54
        R_390_TLS_DTPOFF  R_390 = 55
        R_390_TLS_TPOFF   R_390 = 56
        R_390_20          R_390 = 57
        R_390_GOT20       R_390 = 58
        R_390_GOTPLT20    R_390 = 59
        R_390_TLS_GOTIE20 R_390 = 60
)

func (R_390) GoString

func (i R_390) GoString() string

func (R_390) String

func (i R_390) String() string

type R_AARCH64

Relocation types for AArch64 (aka arm64)

type R_AARCH64 int

const (
        R_AARCH64_NONE                            R_AARCH64 = 0
        R_AARCH64_P32_ABS32                       R_AARCH64 = 1
        R_AARCH64_P32_ABS16                       R_AARCH64 = 2
        R_AARCH64_P32_PREL32                      R_AARCH64 = 3
        R_AARCH64_P32_PREL16                      R_AARCH64 = 4
        R_AARCH64_P32_MOVW_UABS_G0                R_AARCH64 = 5
        R_AARCH64_P32_MOVW_UABS_G0_NC             R_AARCH64 = 6
        R_AARCH64_P32_MOVW_UABS_G1                R_AARCH64 = 7
        R_AARCH64_P32_MOVW_SABS_G0                R_AARCH64 = 8
        R_AARCH64_P32_LD_PREL_LO19                R_AARCH64 = 9
        R_AARCH64_P32_ADR_PREL_LO21               R_AARCH64 = 10
        R_AARCH64_P32_ADR_PREL_PG_HI21            R_AARCH64 = 11
        R_AARCH64_P32_ADD_ABS_LO12_NC             R_AARCH64 = 12
        R_AARCH64_P32_LDST8_ABS_LO12_NC           R_AARCH64 = 13
        R_AARCH64_P32_LDST16_ABS_LO12_NC          R_AARCH64 = 14
        R_AARCH64_P32_LDST32_ABS_LO12_NC          R_AARCH64 = 15
        R_AARCH64_P32_LDST64_ABS_LO12_NC          R_AARCH64 = 16
        R_AARCH64_P32_LDST128_ABS_LO12_NC         R_AARCH64 = 17
        R_AARCH64_P32_TSTBR14                     R_AARCH64 = 18
        R_AARCH64_P32_CONDBR19                    R_AARCH64 = 19
        R_AARCH64_P32_JUMP26                      R_AARCH64 = 20
        R_AARCH64_P32_CALL26                      R_AARCH64 = 21
        R_AARCH64_P32_GOT_LD_PREL19               R_AARCH64 = 25
        R_AARCH64_P32_ADR_GOT_PAGE                R_AARCH64 = 26
        R_AARCH64_P32_LD32_GOT_LO12_NC            R_AARCH64 = 27
        R_AARCH64_P32_TLSGD_ADR_PAGE21            R_AARCH64 = 81
        R_AARCH64_P32_TLSGD_ADD_LO12_NC           R_AARCH64 = 82
        R_AARCH64_P32_TLSIE_ADR_GOTTPREL_PAGE21   R_AARCH64 = 103
        R_AARCH64_P32_TLSIE_LD32_GOTTPREL_LO12_NC R_AARCH64 = 104
        R_AARCH64_P32_TLSIE_LD_GOTTPREL_PREL19    R_AARCH64 = 105
        R_AARCH64_P32_TLSLE_MOVW_TPREL_G1         R_AARCH64 = 106
        R_AARCH64_P32_TLSLE_MOVW_TPREL_G0         R_AARCH64 = 107
        R_AARCH64_P32_TLSLE_MOVW_TPREL_G0_NC      R_AARCH64 = 108
        R_AARCH64_P32_TLSLE_ADD_TPREL_HI12        R_AARCH64 = 109
        R_AARCH64_P32_TLSLE_ADD_TPREL_LO12        R_AARCH64 = 110
        R_AARCH64_P32_TLSLE_ADD_TPREL_LO12_NC     R_AARCH64 = 111
        R_AARCH64_P32_TLSDESC_LD_PREL19           R_AARCH64 = 122
        R_AARCH64_P32_TLSDESC_ADR_PREL21          R_AARCH64 = 123
        R_AARCH64_P32_TLSDESC_ADR_PAGE21          R_AARCH64 = 124
        R_AARCH64_P32_TLSDESC_LD32_LO12_NC        R_AARCH64 = 125
        R_AARCH64_P32_TLSDESC_ADD_LO12_NC         R_AARCH64 = 126
        R_AARCH64_P32_TLSDESC_CALL                R_AARCH64 = 127
        R_AARCH64_P32_COPY                        R_AARCH64 = 180
        R_AARCH64_P32_GLOB_DAT                    R_AARCH64 = 181
        R_AARCH64_P32_JUMP_SLOT                   R_AARCH64 = 182
        R_AARCH64_P32_RELATIVE                    R_AARCH64 = 183
        R_AARCH64_P32_TLS_DTPMOD                  R_AARCH64 = 184
        R_AARCH64_P32_TLS_DTPREL                  R_AARCH64 = 185
        R_AARCH64_P32_TLS_TPREL                   R_AARCH64 = 186
        R_AARCH64_P32_TLSDESC                     R_AARCH64 = 187
        R_AARCH64_P32_IRELATIVE                   R_AARCH64 = 188
        R_AARCH64_NULL                            R_AARCH64 = 256
        R_AARCH64_ABS64                           R_AARCH64 = 257
        R_AARCH64_ABS32                           R_AARCH64 = 258
        R_AARCH64_ABS16                           R_AARCH64 = 259
        R_AARCH64_PREL64                          R_AARCH64 = 260
        R_AARCH64_PREL32                          R_AARCH64 = 261
        R_AARCH64_PREL16                          R_AARCH64 = 262
        R_AARCH64_MOVW_UABS_G0                    R_AARCH64 = 263
        R_AARCH64_MOVW_UABS_G0_NC                 R_AARCH64 = 264
        R_AARCH64_MOVW_UABS_G1                    R_AARCH64 = 265
        R_AARCH64_MOVW_UABS_G1_NC                 R_AARCH64 = 266
        R_AARCH64_MOVW_UABS_G2                    R_AARCH64 = 267
        R_AARCH64_MOVW_UABS_G2_NC                 R_AARCH64 = 268
        R_AARCH64_MOVW_UABS_G3                    R_AARCH64 = 269
        R_AARCH64_MOVW_SABS_G0                    R_AARCH64 = 270
        R_AARCH64_MOVW_SABS_G1                    R_AARCH64 = 271
        R_AARCH64_MOVW_SABS_G2                    R_AARCH64 = 272
        R_AARCH64_LD_PREL_LO19                    R_AARCH64 = 273
        R_AARCH64_ADR_PREL_LO21                   R_AARCH64 = 274
        R_AARCH64_ADR_PREL_PG_HI21                R_AARCH64 = 275
        R_AARCH64_ADR_PREL_PG_HI21_NC             R_AARCH64 = 276
        R_AARCH64_ADD_ABS_LO12_NC                 R_AARCH64 = 277
        R_AARCH64_LDST8_ABS_LO12_NC               R_AARCH64 = 278
        R_AARCH64_TSTBR14                         R_AARCH64 = 279
        R_AARCH64_CONDBR19                        R_AARCH64 = 280
        R_AARCH64_JUMP26                          R_AARCH64 = 282
        R_AARCH64_CALL26                          R_AARCH64 = 283
        R_AARCH64_LDST16_ABS_LO12_NC              R_AARCH64 = 284
        R_AARCH64_LDST32_ABS_LO12_NC              R_AARCH64 = 285
        R_AARCH64_LDST64_ABS_LO12_NC              R_AARCH64 = 286
        R_AARCH64_LDST128_ABS_LO12_NC             R_AARCH64 = 299
        R_AARCH64_GOT_LD_PREL19                   R_AARCH64 = 309
        R_AARCH64_ADR_GOT_PAGE                    R_AARCH64 = 311
        R_AARCH64_LD64_GOT_LO12_NC                R_AARCH64 = 312
        R_AARCH64_TLSGD_ADR_PAGE21                R_AARCH64 = 513
        R_AARCH64_TLSGD_ADD_LO12_NC               R_AARCH64 = 514
        R_AARCH64_TLSIE_MOVW_GOTTPREL_G1          R_AARCH64 = 539
        R_AARCH64_TLSIE_MOVW_GOTTPREL_G0_NC       R_AARCH64 = 540
        R_AARCH64_TLSIE_ADR_GOTTPREL_PAGE21       R_AARCH64 = 541
        R_AARCH64_TLSIE_LD64_GOTTPREL_LO12_NC     R_AARCH64 = 542
        R_AARCH64_TLSIE_LD_GOTTPREL_PREL19        R_AARCH64 = 543
        R_AARCH64_TLSLE_MOVW_TPREL_G2             R_AARCH64 = 544
        R_AARCH64_TLSLE_MOVW_TPREL_G1             R_AARCH64 = 545
        R_AARCH64_TLSLE_MOVW_TPREL_G1_NC          R_AARCH64 = 546
        R_AARCH64_TLSLE_MOVW_TPREL_G0             R_AARCH64 = 547
        R_AARCH64_TLSLE_MOVW_TPREL_G0_NC          R_AARCH64 = 548
        R_AARCH64_TLSLE_ADD_TPREL_HI12            R_AARCH64 = 549
        R_AARCH64_TLSLE_ADD_TPREL_LO12            R_AARCH64 = 550
        R_AARCH64_TLSLE_ADD_TPREL_LO12_NC         R_AARCH64 = 551
        R_AARCH64_TLSDESC_LD_PREL19               R_AARCH64 = 560
        R_AARCH64_TLSDESC_ADR_PREL21              R_AARCH64 = 561
        R_AARCH64_TLSDESC_ADR_PAGE21              R_AARCH64 = 562
        R_AARCH64_TLSDESC_LD64_LO12_NC            R_AARCH64 = 563
        R_AARCH64_TLSDESC_ADD_LO12_NC             R_AARCH64 = 564
        R_AARCH64_TLSDESC_OFF_G1                  R_AARCH64 = 565
        R_AARCH64_TLSDESC_OFF_G0_NC               R_AARCH64 = 566
        R_AARCH64_TLSDESC_LDR                     R_AARCH64 = 567
        R_AARCH64_TLSDESC_ADD                     R_AARCH64 = 568
        R_AARCH64_TLSDESC_CALL                    R_AARCH64 = 569
        R_AARCH64_COPY                            R_AARCH64 = 1024
        R_AARCH64_GLOB_DAT                        R_AARCH64 = 1025
        R_AARCH64_JUMP_SLOT                       R_AARCH64 = 1026
        R_AARCH64_RELATIVE                        R_AARCH64 = 1027
        R_AARCH64_TLS_DTPMOD64                    R_AARCH64 = 1028
        R_AARCH64_TLS_DTPREL64                    R_AARCH64 = 1029
        R_AARCH64_TLS_TPREL64                     R_AARCH64 = 1030
        R_AARCH64_TLSDESC                         R_AARCH64 = 1031
        R_AARCH64_IRELATIVE                       R_AARCH64 = 1032
)

func (R_AARCH64) GoString

func (i R_AARCH64) GoString() string

func (R_AARCH64) String

func (i R_AARCH64) String() string

type R_ALPHA

Relocation types for Alpha.

type R_ALPHA int

const (
        R_ALPHA_NONE           R_ALPHA = 0  /* No reloc */
        R_ALPHA_REFLONG        R_ALPHA = 1  /* Direct 32 bit */
        R_ALPHA_REFQUAD        R_ALPHA = 2  /* Direct 64 bit */
        R_ALPHA_GPREL32        R_ALPHA = 3  /* GP relative 32 bit */
        R_ALPHA_LITERAL        R_ALPHA = 4  /* GP relative 16 bit w/optimization */
        R_ALPHA_LITUSE         R_ALPHA = 5  /* Optimization hint for LITERAL */
        R_ALPHA_GPDISP         R_ALPHA = 6  /* Add displacement to GP */
        R_ALPHA_BRADDR         R_ALPHA = 7  /* PC+4 relative 23 bit shifted */
        R_ALPHA_HINT           R_ALPHA = 8  /* PC+4 relative 16 bit shifted */
        R_ALPHA_SREL16         R_ALPHA = 9  /* PC relative 16 bit */
        R_ALPHA_SREL32         R_ALPHA = 10 /* PC relative 32 bit */
        R_ALPHA_SREL64         R_ALPHA = 11 /* PC relative 64 bit */
        R_ALPHA_OP_PUSH        R_ALPHA = 12 /* OP stack push */
        R_ALPHA_OP_STORE       R_ALPHA = 13 /* OP stack pop and store */
        R_ALPHA_OP_PSUB        R_ALPHA = 14 /* OP stack subtract */
        R_ALPHA_OP_PRSHIFT     R_ALPHA = 15 /* OP stack right shift */
        R_ALPHA_GPVALUE        R_ALPHA = 16
        R_ALPHA_GPRELHIGH      R_ALPHA = 17
        R_ALPHA_GPRELLOW       R_ALPHA = 18
        R_ALPHA_IMMED_GP_16    R_ALPHA = 19
        R_ALPHA_IMMED_GP_HI32  R_ALPHA = 20
        R_ALPHA_IMMED_SCN_HI32 R_ALPHA = 21
        R_ALPHA_IMMED_BR_HI32  R_ALPHA = 22
        R_ALPHA_IMMED_LO32     R_ALPHA = 23
        R_ALPHA_COPY           R_ALPHA = 24 /* Copy symbol at runtime */
        R_ALPHA_GLOB_DAT       R_ALPHA = 25 /* Create GOT entry */
        R_ALPHA_JMP_SLOT       R_ALPHA = 26 /* Create PLT entry */
        R_ALPHA_RELATIVE       R_ALPHA = 27 /* Adjust by program base */
)

func (R_ALPHA) GoString

func (i R_ALPHA) GoString() string

func (R_ALPHA) String

func (i R_ALPHA) String() string

type R_ARM

Relocation types for ARM.

type R_ARM int

const (
        R_ARM_NONE          R_ARM = 0 /* No relocation. */
        R_ARM_PC24          R_ARM = 1
        R_ARM_ABS32         R_ARM = 2
        R_ARM_REL32         R_ARM = 3
        R_ARM_PC13          R_ARM = 4
        R_ARM_ABS16         R_ARM = 5
        R_ARM_ABS12         R_ARM = 6
        R_ARM_THM_ABS5      R_ARM = 7
        R_ARM_ABS8          R_ARM = 8
        R_ARM_SBREL32       R_ARM = 9
        R_ARM_THM_PC22      R_ARM = 10
        R_ARM_THM_PC8       R_ARM = 11
        R_ARM_AMP_VCALL9    R_ARM = 12
        R_ARM_SWI24         R_ARM = 13
        R_ARM_THM_SWI8      R_ARM = 14
        R_ARM_XPC25         R_ARM = 15
        R_ARM_THM_XPC22     R_ARM = 16
        R_ARM_COPY          R_ARM = 20 /* Copy data from shared object. */
        R_ARM_GLOB_DAT      R_ARM = 21 /* Set GOT entry to data address. */
        R_ARM_JUMP_SLOT     R_ARM = 22 /* Set GOT entry to code address. */
        R_ARM_RELATIVE      R_ARM = 23 /* Add load address of shared object. */
        R_ARM_GOTOFF        R_ARM = 24 /* Add GOT-relative symbol address. */
        R_ARM_GOTPC         R_ARM = 25 /* Add PC-relative GOT table address. */
        R_ARM_GOT32         R_ARM = 26 /* Add PC-relative GOT offset. */
        R_ARM_PLT32         R_ARM = 27 /* Add PC-relative PLT offset. */
        R_ARM_GNU_VTENTRY   R_ARM = 100
        R_ARM_GNU_VTINHERIT R_ARM = 101
        R_ARM_RSBREL32      R_ARM = 250
        R_ARM_THM_RPC22     R_ARM = 251
        R_ARM_RREL32        R_ARM = 252
        R_ARM_RABS32        R_ARM = 253
        R_ARM_RPC24         R_ARM = 254
        R_ARM_RBASE         R_ARM = 255
)

func (R_ARM) GoString

func (i R_ARM) GoString() string

func (R_ARM) String

func (i R_ARM) String() string

type R_MIPS

Relocation types for MIPS.

type R_MIPS int

const (
        R_MIPS_NONE          R_MIPS = 0
        R_MIPS_16            R_MIPS = 1
        R_MIPS_32            R_MIPS = 2
        R_MIPS_REL32         R_MIPS = 3
        R_MIPS_26            R_MIPS = 4
        R_MIPS_HI16          R_MIPS = 5  /* high 16 bits of symbol value */
        R_MIPS_LO16          R_MIPS = 6  /* low 16 bits of symbol value */
        R_MIPS_GPREL16       R_MIPS = 7  /* GP-relative reference  */
        R_MIPS_LITERAL       R_MIPS = 8  /* Reference to literal section  */
        R_MIPS_GOT16         R_MIPS = 9  /* Reference to global offset table */
        R_MIPS_PC16          R_MIPS = 10 /* 16 bit PC relative reference */
        R_MIPS_CALL16        R_MIPS = 11 /* 16 bit call through glbl offset tbl */
        R_MIPS_GPREL32       R_MIPS = 12
        R_MIPS_SHIFT5        R_MIPS = 16
        R_MIPS_SHIFT6        R_MIPS = 17
        R_MIPS_64            R_MIPS = 18
        R_MIPS_GOT_DISP      R_MIPS = 19
        R_MIPS_GOT_PAGE      R_MIPS = 20
        R_MIPS_GOT_OFST      R_MIPS = 21
        R_MIPS_GOT_HI16      R_MIPS = 22
        R_MIPS_GOT_LO16      R_MIPS = 23
        R_MIPS_SUB           R_MIPS = 24
        R_MIPS_INSERT_A      R_MIPS = 25
        R_MIPS_INSERT_B      R_MIPS = 26
        R_MIPS_DELETE        R_MIPS = 27
        R_MIPS_HIGHER        R_MIPS = 28
        R_MIPS_HIGHEST       R_MIPS = 29
        R_MIPS_CALL_HI16     R_MIPS = 30
        R_MIPS_CALL_LO16     R_MIPS = 31
        R_MIPS_SCN_DISP      R_MIPS = 32
        R_MIPS_REL16         R_MIPS = 33
        R_MIPS_ADD_IMMEDIATE R_MIPS = 34
        R_MIPS_PJUMP         R_MIPS = 35
        R_MIPS_RELGOT        R_MIPS = 36
        R_MIPS_JALR          R_MIPS = 37

        R_MIPS_TLS_DTPMOD32    R_MIPS = 38 /* Module number 32 bit */
        R_MIPS_TLS_DTPREL32    R_MIPS = 39 /* Module-relative offset 32 bit */
        R_MIPS_TLS_DTPMOD64    R_MIPS = 40 /* Module number 64 bit */
        R_MIPS_TLS_DTPREL64    R_MIPS = 41 /* Module-relative offset 64 bit */
        R_MIPS_TLS_GD          R_MIPS = 42 /* 16 bit GOT offset for GD */
        R_MIPS_TLS_LDM         R_MIPS = 43 /* 16 bit GOT offset for LDM */
        R_MIPS_TLS_DTPREL_HI16 R_MIPS = 44 /* Module-relative offset, high 16 bits */
        R_MIPS_TLS_DTPREL_LO16 R_MIPS = 45 /* Module-relative offset, low 16 bits */
        R_MIPS_TLS_GOTTPREL    R_MIPS = 46 /* 16 bit GOT offset for IE */
        R_MIPS_TLS_TPREL32     R_MIPS = 47 /* TP-relative offset, 32 bit */
        R_MIPS_TLS_TPREL64     R_MIPS = 48 /* TP-relative offset, 64 bit */
        R_MIPS_TLS_TPREL_HI16  R_MIPS = 49 /* TP-relative offset, high 16 bits */
        R_MIPS_TLS_TPREL_LO16  R_MIPS = 50 /* TP-relative offset, low 16 bits */
)

func (R_MIPS) GoString

func (i R_MIPS) GoString() string

func (R_MIPS) String

func (i R_MIPS) String() string

type R_PPC

Relocation types for PowerPC.

type R_PPC int

const (
        R_PPC_NONE            R_PPC = 0 /* No relocation. */
        R_PPC_ADDR32          R_PPC = 1
        R_PPC_ADDR24          R_PPC = 2
        R_PPC_ADDR16          R_PPC = 3
        R_PPC_ADDR16_LO       R_PPC = 4
        R_PPC_ADDR16_HI       R_PPC = 5
        R_PPC_ADDR16_HA       R_PPC = 6
        R_PPC_ADDR14          R_PPC = 7
        R_PPC_ADDR14_BRTAKEN  R_PPC = 8
        R_PPC_ADDR14_BRNTAKEN R_PPC = 9
        R_PPC_REL24           R_PPC = 10
        R_PPC_REL14           R_PPC = 11
        R_PPC_REL14_BRTAKEN   R_PPC = 12
        R_PPC_REL14_BRNTAKEN  R_PPC = 13
        R_PPC_GOT16           R_PPC = 14
        R_PPC_GOT16_LO        R_PPC = 15
        R_PPC_GOT16_HI        R_PPC = 16
        R_PPC_GOT16_HA        R_PPC = 17
        R_PPC_PLTREL24        R_PPC = 18
        R_PPC_COPY            R_PPC = 19
        R_PPC_GLOB_DAT        R_PPC = 20
        R_PPC_JMP_SLOT        R_PPC = 21
        R_PPC_RELATIVE        R_PPC = 22
        R_PPC_LOCAL24PC       R_PPC = 23
        R_PPC_UADDR32         R_PPC = 24
        R_PPC_UADDR16         R_PPC = 25
        R_PPC_REL32           R_PPC = 26
        R_PPC_PLT32           R_PPC = 27
        R_PPC_PLTREL32        R_PPC = 28
        R_PPC_PLT16_LO        R_PPC = 29
        R_PPC_PLT16_HI        R_PPC = 30
        R_PPC_PLT16_HA        R_PPC = 31
        R_PPC_SDAREL16        R_PPC = 32
        R_PPC_SECTOFF         R_PPC = 33
        R_PPC_SECTOFF_LO      R_PPC = 34
        R_PPC_SECTOFF_HI      R_PPC = 35
        R_PPC_SECTOFF_HA      R_PPC = 36
        R_PPC_TLS             R_PPC = 67
        R_PPC_DTPMOD32        R_PPC = 68
        R_PPC_TPREL16         R_PPC = 69
        R_PPC_TPREL16_LO      R_PPC = 70
        R_PPC_TPREL16_HI      R_PPC = 71
        R_PPC_TPREL16_HA      R_PPC = 72
        R_PPC_TPREL32         R_PPC = 73
        R_PPC_DTPREL16        R_PPC = 74
        R_PPC_DTPREL16_LO     R_PPC = 75
        R_PPC_DTPREL16_HI     R_PPC = 76
        R_PPC_DTPREL16_HA     R_PPC = 77
        R_PPC_DTPREL32        R_PPC = 78
        R_PPC_GOT_TLSGD16     R_PPC = 79
        R_PPC_GOT_TLSGD16_LO  R_PPC = 80
        R_PPC_GOT_TLSGD16_HI  R_PPC = 81
        R_PPC_GOT_TLSGD16_HA  R_PPC = 82
        R_PPC_GOT_TLSLD16     R_PPC = 83
        R_PPC_GOT_TLSLD16_LO  R_PPC = 84
        R_PPC_GOT_TLSLD16_HI  R_PPC = 85
        R_PPC_GOT_TLSLD16_HA  R_PPC = 86
        R_PPC_GOT_TPREL16     R_PPC = 87
        R_PPC_GOT_TPREL16_LO  R_PPC = 88
        R_PPC_GOT_TPREL16_HI  R_PPC = 89
        R_PPC_GOT_TPREL16_HA  R_PPC = 90
        R_PPC_EMB_NADDR32     R_PPC = 101
        R_PPC_EMB_NADDR16     R_PPC = 102
        R_PPC_EMB_NADDR16_LO  R_PPC = 103
        R_PPC_EMB_NADDR16_HI  R_PPC = 104
        R_PPC_EMB_NADDR16_HA  R_PPC = 105
        R_PPC_EMB_SDAI16      R_PPC = 106
        R_PPC_EMB_SDA2I16     R_PPC = 107
        R_PPC_EMB_SDA2REL     R_PPC = 108
        R_PPC_EMB_SDA21       R_PPC = 109
        R_PPC_EMB_MRKREF      R_PPC = 110
        R_PPC_EMB_RELSEC16    R_PPC = 111
        R_PPC_EMB_RELST_LO    R_PPC = 112
        R_PPC_EMB_RELST_HI    R_PPC = 113
        R_PPC_EMB_RELST_HA    R_PPC = 114
        R_PPC_EMB_BIT_FLD     R_PPC = 115
        R_PPC_EMB_RELSDA      R_PPC = 116
)

func (R_PPC) GoString

func (i R_PPC) GoString() string

func (R_PPC) String

func (i R_PPC) String() string

type R_PPC64

Relocation types for 64-bit PowerPC or Power Architecture processors.

type R_PPC64 int

const (
        R_PPC64_NONE               R_PPC64 = 0
        R_PPC64_ADDR32             R_PPC64 = 1
        R_PPC64_ADDR24             R_PPC64 = 2
        R_PPC64_ADDR16             R_PPC64 = 3
        R_PPC64_ADDR16_LO          R_PPC64 = 4
        R_PPC64_ADDR16_HI          R_PPC64 = 5
        R_PPC64_ADDR16_HA          R_PPC64 = 6
        R_PPC64_ADDR14             R_PPC64 = 7
        R_PPC64_ADDR14_BRTAKEN     R_PPC64 = 8
        R_PPC64_ADDR14_BRNTAKEN    R_PPC64 = 9
        R_PPC64_REL24              R_PPC64 = 10
        R_PPC64_REL14              R_PPC64 = 11
        R_PPC64_REL14_BRTAKEN      R_PPC64 = 12
        R_PPC64_REL14_BRNTAKEN     R_PPC64 = 13
        R_PPC64_GOT16              R_PPC64 = 14
        R_PPC64_GOT16_LO           R_PPC64 = 15
        R_PPC64_GOT16_HI           R_PPC64 = 16
        R_PPC64_GOT16_HA           R_PPC64 = 17
        R_PPC64_JMP_SLOT           R_PPC64 = 21
        R_PPC64_REL32              R_PPC64 = 26
        R_PPC64_ADDR64             R_PPC64 = 38
        R_PPC64_ADDR16_HIGHER      R_PPC64 = 39
        R_PPC64_ADDR16_HIGHERA     R_PPC64 = 40
        R_PPC64_ADDR16_HIGHEST     R_PPC64 = 41
        R_PPC64_ADDR16_HIGHESTA    R_PPC64 = 42
        R_PPC64_REL64              R_PPC64 = 44
        R_PPC64_TOC16              R_PPC64 = 47
        R_PPC64_TOC16_LO           R_PPC64 = 48
        R_PPC64_TOC16_HI           R_PPC64 = 49
        R_PPC64_TOC16_HA           R_PPC64 = 50
        R_PPC64_TOC                R_PPC64 = 51
        R_PPC64_ADDR16_DS          R_PPC64 = 56
        R_PPC64_ADDR16_LO_DS       R_PPC64 = 57
        R_PPC64_GOT16_DS           R_PPC64 = 58
        R_PPC64_GOT16_LO_DS        R_PPC64 = 59
        R_PPC64_TOC16_DS           R_PPC64 = 63
        R_PPC64_TOC16_LO_DS        R_PPC64 = 64
        R_PPC64_TLS                R_PPC64 = 67
        R_PPC64_DTPMOD64           R_PPC64 = 68
        R_PPC64_TPREL16            R_PPC64 = 69
        R_PPC64_TPREL16_LO         R_PPC64 = 70
        R_PPC64_TPREL16_HI         R_PPC64 = 71
        R_PPC64_TPREL16_HA         R_PPC64 = 72
        R_PPC64_TPREL64            R_PPC64 = 73
        R_PPC64_DTPREL16           R_PPC64 = 74
        R_PPC64_DTPREL16_LO        R_PPC64 = 75
        R_PPC64_DTPREL16_HI        R_PPC64 = 76
        R_PPC64_DTPREL16_HA        R_PPC64 = 77
        R_PPC64_DTPREL64           R_PPC64 = 78
        R_PPC64_GOT_TLSGD16        R_PPC64 = 79
        R_PPC64_GOT_TLSGD16_LO     R_PPC64 = 80
        R_PPC64_GOT_TLSGD16_HI     R_PPC64 = 81
        R_PPC64_GOT_TLSGD16_HA     R_PPC64 = 82
        R_PPC64_GOT_TLSLD16        R_PPC64 = 83
        R_PPC64_GOT_TLSLD16_LO     R_PPC64 = 84
        R_PPC64_GOT_TLSLD16_HI     R_PPC64 = 85
        R_PPC64_GOT_TLSLD16_HA     R_PPC64 = 86
        R_PPC64_GOT_TPREL16_DS     R_PPC64 = 87
        R_PPC64_GOT_TPREL16_LO_DS  R_PPC64 = 88
        R_PPC64_GOT_TPREL16_HI     R_PPC64 = 89
        R_PPC64_GOT_TPREL16_HA     R_PPC64 = 90
        R_PPC64_GOT_DTPREL16_DS    R_PPC64 = 91
        R_PPC64_GOT_DTPREL16_LO_DS R_PPC64 = 92
        R_PPC64_GOT_DTPREL16_HI    R_PPC64 = 93
        R_PPC64_GOT_DTPREL16_HA    R_PPC64 = 94
        R_PPC64_TPREL16_DS         R_PPC64 = 95
        R_PPC64_TPREL16_LO_DS      R_PPC64 = 96
        R_PPC64_TPREL16_HIGHER     R_PPC64 = 97
        R_PPC64_TPREL16_HIGHERA    R_PPC64 = 98
        R_PPC64_TPREL16_HIGHEST    R_PPC64 = 99
        R_PPC64_TPREL16_HIGHESTA   R_PPC64 = 100
        R_PPC64_DTPREL16_DS        R_PPC64 = 101
        R_PPC64_DTPREL16_LO_DS     R_PPC64 = 102
        R_PPC64_DTPREL16_HIGHER    R_PPC64 = 103
        R_PPC64_DTPREL16_HIGHERA   R_PPC64 = 104
        R_PPC64_DTPREL16_HIGHEST   R_PPC64 = 105
        R_PPC64_DTPREL16_HIGHESTA  R_PPC64 = 106
        R_PPC64_TLSGD              R_PPC64 = 107
        R_PPC64_TLSLD              R_PPC64 = 108
        R_PPC64_REL16              R_PPC64 = 249
        R_PPC64_REL16_LO           R_PPC64 = 250
        R_PPC64_REL16_HI           R_PPC64 = 251
        R_PPC64_REL16_HA           R_PPC64 = 252
)

func (R_PPC64) GoString

func (i R_PPC64) GoString() string

func (R_PPC64) String

func (i R_PPC64) String() string

type R_SPARC

Relocation types for SPARC.

type R_SPARC int

const (
        R_SPARC_NONE     R_SPARC = 0
        R_SPARC_8        R_SPARC = 1
        R_SPARC_16       R_SPARC = 2
        R_SPARC_32       R_SPARC = 3
        R_SPARC_DISP8    R_SPARC = 4
        R_SPARC_DISP16   R_SPARC = 5
        R_SPARC_DISP32   R_SPARC = 6
        R_SPARC_WDISP30  R_SPARC = 7
        R_SPARC_WDISP22  R_SPARC = 8
        R_SPARC_HI22     R_SPARC = 9
        R_SPARC_22       R_SPARC = 10
        R_SPARC_13       R_SPARC = 11
        R_SPARC_LO10     R_SPARC = 12
        R_SPARC_GOT10    R_SPARC = 13
        R_SPARC_GOT13    R_SPARC = 14
        R_SPARC_GOT22    R_SPARC = 15
        R_SPARC_PC10     R_SPARC = 16
        R_SPARC_PC22     R_SPARC = 17
        R_SPARC_WPLT30   R_SPARC = 18
        R_SPARC_COPY     R_SPARC = 19
        R_SPARC_GLOB_DAT R_SPARC = 20
        R_SPARC_JMP_SLOT R_SPARC = 21
        R_SPARC_RELATIVE R_SPARC = 22
        R_SPARC_UA32     R_SPARC = 23
        R_SPARC_PLT32    R_SPARC = 24
        R_SPARC_HIPLT22  R_SPARC = 25
        R_SPARC_LOPLT10  R_SPARC = 26
        R_SPARC_PCPLT32  R_SPARC = 27
        R_SPARC_PCPLT22  R_SPARC = 28
        R_SPARC_PCPLT10  R_SPARC = 29
        R_SPARC_10       R_SPARC = 30
        R_SPARC_11       R_SPARC = 31
        R_SPARC_64       R_SPARC = 32
        R_SPARC_OLO10    R_SPARC = 33
        R_SPARC_HH22     R_SPARC = 34
        R_SPARC_HM10     R_SPARC = 35
        R_SPARC_LM22     R_SPARC = 36
        R_SPARC_PC_HH22  R_SPARC = 37
        R_SPARC_PC_HM10  R_SPARC = 38
        R_SPARC_PC_LM22  R_SPARC = 39
        R_SPARC_WDISP16  R_SPARC = 40
        R_SPARC_WDISP19  R_SPARC = 41
        R_SPARC_GLOB_JMP R_SPARC = 42
        R_SPARC_7        R_SPARC = 43
        R_SPARC_5        R_SPARC = 44
        R_SPARC_6        R_SPARC = 45
        R_SPARC_DISP64   R_SPARC = 46
        R_SPARC_PLT64    R_SPARC = 47
        R_SPARC_HIX22    R_SPARC = 48
        R_SPARC_LOX10    R_SPARC = 49
        R_SPARC_H44      R_SPARC = 50
        R_SPARC_M44      R_SPARC = 51
        R_SPARC_L44      R_SPARC = 52
        R_SPARC_REGISTER R_SPARC = 53
        R_SPARC_UA64     R_SPARC = 54
        R_SPARC_UA16     R_SPARC = 55
)

func (R_SPARC) GoString

func (i R_SPARC) GoString() string

func (R_SPARC) String

func (i R_SPARC) String() string

type R_X86_64

Relocation types for x86-64.

type R_X86_64 int

const (
        R_X86_64_NONE     R_X86_64 = 0  /* No relocation. */
        R_X86_64_64       R_X86_64 = 1  /* Add 64 bit symbol value. */
        R_X86_64_PC32     R_X86_64 = 2  /* PC-relative 32 bit signed sym value. */
        R_X86_64_GOT32    R_X86_64 = 3  /* PC-relative 32 bit GOT offset. */
        R_X86_64_PLT32    R_X86_64 = 4  /* PC-relative 32 bit PLT offset. */
        R_X86_64_COPY     R_X86_64 = 5  /* Copy data from shared object. */
        R_X86_64_GLOB_DAT R_X86_64 = 6  /* Set GOT entry to data address. */
        R_X86_64_JMP_SLOT R_X86_64 = 7  /* Set GOT entry to code address. */
        R_X86_64_RELATIVE R_X86_64 = 8  /* Add load address of shared object. */
        R_X86_64_GOTPCREL R_X86_64 = 9  /* Add 32 bit signed pcrel offset to GOT. */
        R_X86_64_32       R_X86_64 = 10 /* Add 32 bit zero extended symbol value */
        R_X86_64_32S      R_X86_64 = 11 /* Add 32 bit sign extended symbol value */
        R_X86_64_16       R_X86_64 = 12 /* Add 16 bit zero extended symbol value */
        R_X86_64_PC16     R_X86_64 = 13 /* Add 16 bit signed extended pc relative symbol value */
        R_X86_64_8        R_X86_64 = 14 /* Add 8 bit zero extended symbol value */
        R_X86_64_PC8      R_X86_64 = 15 /* Add 8 bit signed extended pc relative symbol value */
        R_X86_64_DTPMOD64 R_X86_64 = 16 /* ID of module containing symbol */
        R_X86_64_DTPOFF64 R_X86_64 = 17 /* Offset in TLS block */
        R_X86_64_TPOFF64  R_X86_64 = 18 /* Offset in static TLS block */
        R_X86_64_TLSGD    R_X86_64 = 19 /* PC relative offset to GD GOT entry */
        R_X86_64_TLSLD    R_X86_64 = 20 /* PC relative offset to LD GOT entry */
        R_X86_64_DTPOFF32 R_X86_64 = 21 /* Offset in TLS block */
        R_X86_64_GOTTPOFF R_X86_64 = 22 /* PC relative offset to IE GOT entry */
        R_X86_64_TPOFF32  R_X86_64 = 23 /* Offset in static TLS block */
)

func (R_X86_64) GoString

func (i R_X86_64) GoString() string

func (R_X86_64) String

func (i R_X86_64) String() string

type Rel32

ELF32 Relocations that don't need an addend field.

type Rel32 struct {
        Off  uint32 /* Location to be relocated. */
        Info uint32 /* Relocation type and symbol index. */
}

type Rel64

ELF64 relocations that don't need an addend field.

type Rel64 struct {
        Off  uint64 /* Location to be relocated. */
        Info uint64 /* Relocation type and symbol index. */
}

type Rela32

ELF32 Relocations that need an addend field.

type Rela32 struct {
        Off    uint32 /* Location to be relocated. */
        Info   uint32 /* Relocation type and symbol index. */
        Addend int32  /* Addend. */
}

type Rela64

ELF64 relocations that need an addend field.

type Rela64 struct {
        Off    uint64 /* Location to be relocated. */
        Info   uint64 /* Relocation type and symbol index. */
        Addend int64  /* Addend. */
}

type Section

A Section represents a single section in an ELF file.

type Section struct {
        SectionHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        //
        // ReaderAt may be nil if the section is not easily available
        // in a random-access form. For example, a compressed section
        // may have a nil ReaderAt.
        io.ReaderAt
        // contains filtered or unexported fields
}

func (*Section) Data

func (s *Section) Data() ([]byte, error)

Data reads and returns the contents of the ELF section. Even if the section is stored compressed in the ELF file, Data returns uncompressed data.
func (*Section) Open

func (s *Section) Open() io.ReadSeeker

Open returns a new ReadSeeker reading the ELF section. Even if the section is stored compressed in the ELF file, the ReadSeeker reads uncompressed data.
type Section32

ELF32 Section header.

type Section32 struct {
        Name      uint32 /* Section name (index into the section header string table). */
        Type      uint32 /* Section type. */
        Flags     uint32 /* Section flags. */
        Addr      uint32 /* Address in memory image. */
        Off       uint32 /* Offset in file. */
        Size      uint32 /* Size in bytes. */
        Link      uint32 /* Index of a related section. */
        Info      uint32 /* Depends on section type. */
        Addralign uint32 /* Alignment in bytes. */
        Entsize   uint32 /* Size of each entry in section. */
}

type Section64

ELF64 Section header.

type Section64 struct {
        Name      uint32 /* Section name (index into the section header string table). */
        Type      uint32 /* Section type. */
        Flags     uint64 /* Section flags. */
        Addr      uint64 /* Address in memory image. */
        Off       uint64 /* Offset in file. */
        Size      uint64 /* Size in bytes. */
        Link      uint32 /* Index of a related section. */
        Info      uint32 /* Depends on section type. */
        Addralign uint64 /* Alignment in bytes. */
        Entsize   uint64 /* Size of each entry in section. */
}

type SectionFlag

Section flags.

type SectionFlag uint32

const (
        SHF_WRITE            SectionFlag = 0x1        /* Section contains writable data. */
        SHF_ALLOC            SectionFlag = 0x2        /* Section occupies memory. */
        SHF_EXECINSTR        SectionFlag = 0x4        /* Section contains instructions. */
        SHF_MERGE            SectionFlag = 0x10       /* Section may be merged. */
        SHF_STRINGS          SectionFlag = 0x20       /* Section contains strings. */
        SHF_INFO_LINK        SectionFlag = 0x40       /* sh_info holds section index. */
        SHF_LINK_ORDER       SectionFlag = 0x80       /* Special ordering requirements. */
        SHF_OS_NONCONFORMING SectionFlag = 0x100      /* OS-specific processing required. */
        SHF_GROUP            SectionFlag = 0x200      /* Member of section group. */
        SHF_TLS              SectionFlag = 0x400      /* Section contains TLS data. */
        SHF_COMPRESSED       SectionFlag = 0x800      /* Section is compressed. */
        SHF_MASKOS           SectionFlag = 0x0ff00000 /* OS-specific semantics. */
        SHF_MASKPROC         SectionFlag = 0xf0000000 /* Processor-specific semantics. */
)

func (SectionFlag) GoString

func (i SectionFlag) GoString() string

func (SectionFlag) String

func (i SectionFlag) String() string

type SectionHeader

A SectionHeader represents a single ELF section header.

type SectionHeader struct {
        Name      string
        Type      SectionType
        Flags     SectionFlag
        Addr      uint64
        Offset    uint64
        Size      uint64
        Link      uint32
        Info      uint32
        Addralign uint64
        Entsize   uint64

        // FileSize is the size of this section in the file in bytes.
        // If a section is compressed, FileSize is the size of the
        // compressed data, while Size (above) is the size of the
        // uncompressed data.
        FileSize uint64
}

type SectionIndex

Special section indices.

type SectionIndex int

const (
        SHN_UNDEF     SectionIndex = 0      /* Undefined, missing, irrelevant. */
        SHN_LORESERVE SectionIndex = 0xff00 /* First of reserved range. */
        SHN_LOPROC    SectionIndex = 0xff00 /* First processor-specific. */
        SHN_HIPROC    SectionIndex = 0xff1f /* Last processor-specific. */
        SHN_LOOS      SectionIndex = 0xff20 /* First operating system-specific. */
        SHN_HIOS      SectionIndex = 0xff3f /* Last operating system-specific. */
        SHN_ABS       SectionIndex = 0xfff1 /* Absolute values. */
        SHN_COMMON    SectionIndex = 0xfff2 /* Common data. */
        SHN_XINDEX    SectionIndex = 0xffff /* Escape; index stored elsewhere. */
        SHN_HIRESERVE SectionIndex = 0xffff /* Last of reserved range. */
)

func (SectionIndex) GoString

func (i SectionIndex) GoString() string

func (SectionIndex) String

func (i SectionIndex) String() string

type SectionType

Section type.

type SectionType uint32

const (
        SHT_NULL           SectionType = 0          /* inactive */
        SHT_PROGBITS       SectionType = 1          /* program defined information */
        SHT_SYMTAB         SectionType = 2          /* symbol table section */
        SHT_STRTAB         SectionType = 3          /* string table section */
        SHT_RELA           SectionType = 4          /* relocation section with addends */
        SHT_HASH           SectionType = 5          /* symbol hash table section */
        SHT_DYNAMIC        SectionType = 6          /* dynamic section */
        SHT_NOTE           SectionType = 7          /* note section */
        SHT_NOBITS         SectionType = 8          /* no space section */
        SHT_REL            SectionType = 9          /* relocation section - no addends */
        SHT_SHLIB          SectionType = 10         /* reserved - purpose unknown */
        SHT_DYNSYM         SectionType = 11         /* dynamic symbol table section */
        SHT_INIT_ARRAY     SectionType = 14         /* Initialization function pointers. */
        SHT_FINI_ARRAY     SectionType = 15         /* Termination function pointers. */
        SHT_PREINIT_ARRAY  SectionType = 16         /* Pre-initialization function ptrs. */
        SHT_GROUP          SectionType = 17         /* Section group. */
        SHT_SYMTAB_SHNDX   SectionType = 18         /* Section indexes (see SHN_XINDEX). */
        SHT_LOOS           SectionType = 0x60000000 /* First of OS specific semantics */
        SHT_GNU_ATTRIBUTES SectionType = 0x6ffffff5 /* GNU object attributes */
        SHT_GNU_HASH       SectionType = 0x6ffffff6 /* GNU hash table */
        SHT_GNU_LIBLIST    SectionType = 0x6ffffff7 /* GNU prelink library list */
        SHT_GNU_VERDEF     SectionType = 0x6ffffffd /* GNU version definition section */
        SHT_GNU_VERNEED    SectionType = 0x6ffffffe /* GNU version needs section */
        SHT_GNU_VERSYM     SectionType = 0x6fffffff /* GNU version symbol table */
        SHT_HIOS           SectionType = 0x6fffffff /* Last of OS specific semantics */
        SHT_LOPROC         SectionType = 0x70000000 /* reserved range for processor */
        SHT_HIPROC         SectionType = 0x7fffffff /* specific section header types */
        SHT_LOUSER         SectionType = 0x80000000 /* reserved range for application */
        SHT_HIUSER         SectionType = 0xffffffff /* specific indexes */
)

func (SectionType) GoString

func (i SectionType) GoString() string

func (SectionType) String

func (i SectionType) String() string

type Sym32

ELF32 Symbol.

type Sym32 struct {
        Name  uint32
        Value uint32
        Size  uint32
        Info  uint8
        Other uint8
        Shndx uint16
}

type Sym64

ELF64 symbol table entries.

type Sym64 struct {
        Name  uint32 /* String table index of name. */
        Info  uint8  /* Type and binding information. */
        Other uint8  /* Reserved (not used). */
        Shndx uint16 /* Section index of symbol. */
        Value uint64 /* Symbol value. */
        Size  uint64 /* Size of associated object. */
}

type SymBind

Symbol Binding - ELFNN_ST_BIND - st_info

type SymBind int

const (
        STB_LOCAL  SymBind = 0  /* Local symbol */
        STB_GLOBAL SymBind = 1  /* Global symbol */
        STB_WEAK   SymBind = 2  /* like global - lower precedence */
        STB_LOOS   SymBind = 10 /* Reserved range for operating system */
        STB_HIOS   SymBind = 12 /*   specific semantics. */
        STB_LOPROC SymBind = 13 /* reserved range for processor */
        STB_HIPROC SymBind = 15 /*   specific semantics. */
)

func ST_BIND

func ST_BIND(info uint8) SymBind

func (SymBind) GoString

func (i SymBind) GoString() string

func (SymBind) String

func (i SymBind) String() string

type SymType

Symbol type - ELFNN_ST_TYPE - st_info

type SymType int

const (
        STT_NOTYPE  SymType = 0  /* Unspecified type. */
        STT_OBJECT  SymType = 1  /* Data object. */
        STT_FUNC    SymType = 2  /* Function. */
        STT_SECTION SymType = 3  /* Section. */
        STT_FILE    SymType = 4  /* Source file. */
        STT_COMMON  SymType = 5  /* Uninitialized common block. */
        STT_TLS     SymType = 6  /* TLS object. */
        STT_LOOS    SymType = 10 /* Reserved range for operating system */
        STT_HIOS    SymType = 12 /*   specific semantics. */
        STT_LOPROC  SymType = 13 /* reserved range for processor */
        STT_HIPROC  SymType = 15 /*   specific semantics. */
)

func ST_TYPE

func ST_TYPE(info uint8) SymType

func (SymType) GoString

func (i SymType) GoString() string

func (SymType) String

func (i SymType) String() string

type SymVis

Symbol visibility - ELFNN_ST_VISIBILITY - st_other

type SymVis int

const (
        STV_DEFAULT   SymVis = 0x0 /* Default visibility (see binding). */
        STV_INTERNAL  SymVis = 0x1 /* Special meaning in relocatable objects. */
        STV_HIDDEN    SymVis = 0x2 /* Not visible. */
        STV_PROTECTED SymVis = 0x3 /* Visible but not preemptible. */
)

func ST_VISIBILITY

func ST_VISIBILITY(other uint8) SymVis

func (SymVis) GoString

func (i SymVis) GoString() string

func (SymVis) String

func (i SymVis) String() string

type Symbol

A Symbol represents an entry in an ELF symbol table section.

type Symbol struct {
        Name        string
        Info, Other byte
        Section     SectionIndex
        Value, Size uint64
}

type Type

Type is found in Header.Type.

type Type uint16

const (
        ET_NONE   Type = 0      /* Unknown type. */
        ET_REL    Type = 1      /* Relocatable. */
        ET_EXEC   Type = 2      /* Executable. */
        ET_DYN    Type = 3      /* Shared object. */
        ET_CORE   Type = 4      /* Core file. */
        ET_LOOS   Type = 0xfe00 /* First operating system specific. */
        ET_HIOS   Type = 0xfeff /* Last operating system-specific. */
        ET_LOPROC Type = 0xff00 /* First processor-specific. */
        ET_HIPROC Type = 0xffff /* Last processor-specific. */
)

func (Type) GoString

func (i Type) GoString() string

func (Type) String

func (i Type) String() string

type Version

Version is found in Header.Ident[EI_VERSION] and Header.Version.

type Version byte

const (
        EV_NONE    Version = 0
        EV_CURRENT Version = 1
)

func (Version) GoString

func (i Version) GoString() string

func (Version) String

func (i Version) String() string
