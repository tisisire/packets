
 Package macho

    import "debug/macho"

    Overview
    Index

Overview ▾

Package macho implements access to Mach-O object files.
Index ▾

    Constants
    Variables
    type Cpu
        func (i Cpu) GoString() string
        func (i Cpu) String() string
    type Dylib
    type DylibCmd
    type Dysymtab
    type DysymtabCmd
    type FatArch
    type FatArchHeader
    type FatFile
        func NewFatFile(r io.ReaderAt) (*FatFile, error)
        func OpenFat(name string) (*FatFile, error)
        func (ff *FatFile) Close() error
    type File
        func NewFile(r io.ReaderAt) (*File, error)
        func Open(name string) (*File, error)
        func (f *File) Close() error
        func (f *File) DWARF() (*dwarf.Data, error)
        func (f *File) ImportedLibraries() ([]string, error)
        func (f *File) ImportedSymbols() ([]string, error)
        func (f *File) Section(name string) *Section
        func (f *File) Segment(name string) *Segment
    type FileHeader
    type FormatError
        func (e *FormatError) Error() string
    type Load
    type LoadBytes
        func (b LoadBytes) Raw() []byte
    type LoadCmd
        func (i LoadCmd) GoString() string
        func (i LoadCmd) String() string
    type Nlist32
    type Nlist64
    type Regs386
    type RegsAMD64
    type Section
        func (s *Section) Data() ([]byte, error)
        func (s *Section) Open() io.ReadSeeker
    type Section32
    type Section64
    type SectionHeader
    type Segment
        func (s *Segment) Data() ([]byte, error)
        func (s *Segment) Open() io.ReadSeeker
    type Segment32
    type Segment64
    type SegmentHeader
    type Symbol
    type Symtab
    type SymtabCmd
    type Thread
    type Type

Package files

fat.go file.go macho.go
Constants

const (
        Magic32  uint32 = 0xfeedface
        Magic64  uint32 = 0xfeedfacf
        MagicFat uint32 = 0xcafebabe
)

Variables

ErrNotFat is returned from NewFatFile or OpenFat when the file is not a universal binary but may be a thin binary, based on its magic number.

var ErrNotFat = &FormatError{0, "not a fat Mach-O file", nil}

type Cpu

A Cpu is a Mach-O cpu type.

type Cpu uint32

const (
        Cpu386   Cpu = 7
        CpuAmd64 Cpu = Cpu386 | cpuArch64
        CpuArm   Cpu = 12
        CpuPpc   Cpu = 18
        CpuPpc64 Cpu = CpuPpc | cpuArch64
)

func (Cpu) GoString

func (i Cpu) GoString() string

func (Cpu) String

func (i Cpu) String() string

type Dylib

A Dylib represents a Mach-O load dynamic library command.

type Dylib struct {
        LoadBytes
        Name           string
        Time           uint32
        CurrentVersion uint32
        CompatVersion  uint32
}

type DylibCmd

A DylibCmd is a Mach-O load dynamic library command.

type DylibCmd struct {
        Cmd            LoadCmd
        Len            uint32
        Name           uint32
        Time           uint32
        CurrentVersion uint32
        CompatVersion  uint32
}

type Dysymtab

A Dysymtab represents a Mach-O dynamic symbol table command.

type Dysymtab struct {
        LoadBytes
        DysymtabCmd
        IndirectSyms []uint32 // indices into Symtab.Syms
}

type DysymtabCmd

A DysymtabCmd is a Mach-O dynamic symbol table command.

type DysymtabCmd struct {
        Cmd            LoadCmd
        Len            uint32
        Ilocalsym      uint32
        Nlocalsym      uint32
        Iextdefsym     uint32
        Nextdefsym     uint32
        Iundefsym      uint32
        Nundefsym      uint32
        Tocoffset      uint32
        Ntoc           uint32
        Modtaboff      uint32
        Nmodtab        uint32
        Extrefsymoff   uint32
        Nextrefsyms    uint32
        Indirectsymoff uint32
        Nindirectsyms  uint32
        Extreloff      uint32
        Nextrel        uint32
        Locreloff      uint32
        Nlocrel        uint32
}

type FatArch

A FatArch is a Mach-O File inside a FatFile.

type FatArch struct {
        FatArchHeader
        *File
}

type FatArchHeader

A FatArchHeader represents a fat header for a specific image architecture.

type FatArchHeader struct {
        Cpu    Cpu
        SubCpu uint32
        Offset uint32
        Size   uint32
        Align  uint32
}

type FatFile

A FatFile is a Mach-O universal binary that contains at least one architecture.

type FatFile struct {
        Magic  uint32
        Arches []FatArch
        // contains filtered or unexported fields
}

func NewFatFile

func NewFatFile(r io.ReaderAt) (*FatFile, error)

NewFatFile creates a new FatFile for accessing all the Mach-O images in a universal binary. The Mach-O binary is expected to start at position 0 in the ReaderAt.
func OpenFat

func OpenFat(name string) (*FatFile, error)

OpenFat opens the named file using os.Open and prepares it for use as a Mach-O universal binary.
func (*FatFile) Close

func (ff *FatFile) Close() error

type File

A File represents an open Mach-O file.

type File struct {
        FileHeader
        ByteOrder binary.ByteOrder
        Loads     []Load
        Sections  []*Section

        Symtab   *Symtab
        Dysymtab *Dysymtab
        // contains filtered or unexported fields
}

func NewFile

func NewFile(r io.ReaderAt) (*File, error)

NewFile creates a new File for accessing a Mach-O binary in an underlying reader. The Mach-O binary is expected to start at position 0 in the ReaderAt.
func Open

func Open(name string) (*File, error)

Open opens the named file using os.Open and prepares it for use as a Mach-O binary.
func (*File) Close

func (f *File) Close() error

Close closes the File. If the File was created using NewFile directly instead of Open, Close has no effect.
func (*File) DWARF

func (f *File) DWARF() (*dwarf.Data, error)

DWARF returns the DWARF debug information for the Mach-O file.
func (*File) ImportedLibraries

func (f *File) ImportedLibraries() ([]string, error)

ImportedLibraries returns the paths of all libraries referred to by the binary f that are expected to be linked with the binary at dynamic link time.
func (*File) ImportedSymbols

func (f *File) ImportedSymbols() ([]string, error)

ImportedSymbols returns the names of all symbols referred to by the binary f that are expected to be satisfied by other libraries at dynamic load time.
func (*File) Section

func (f *File) Section(name string) *Section

Section returns the first section with the given name, or nil if no such section exists.
func (*File) Segment

func (f *File) Segment(name string) *Segment

Segment returns the first Segment with the given name, or nil if no such segment exists.
type FileHeader

A FileHeader represents a Mach-O file header.

type FileHeader struct {
        Magic  uint32
        Cpu    Cpu
        SubCpu uint32
        Type   Type
        Ncmd   uint32
        Cmdsz  uint32
        Flags  uint32
}

type FormatError

FormatError is returned by some operations if the data does not have the correct format for an object file.

type FormatError struct {
        // contains filtered or unexported fields
}

func (*FormatError) Error

func (e *FormatError) Error() string

type Load

A Load represents any Mach-O load command.

type Load interface {
        Raw() []byte
}

type LoadBytes

A LoadBytes is the uninterpreted bytes of a Mach-O load command.

type LoadBytes []byte

func (LoadBytes) Raw

func (b LoadBytes) Raw() []byte

type LoadCmd

A LoadCmd is a Mach-O load command.

type LoadCmd uint32

const (
        LoadCmdSegment    LoadCmd = 1
        LoadCmdSymtab     LoadCmd = 2
        LoadCmdThread     LoadCmd = 4
        LoadCmdUnixThread LoadCmd = 5 // thread+stack
        LoadCmdDysymtab   LoadCmd = 11
        LoadCmdDylib      LoadCmd = 12
        LoadCmdDylinker   LoadCmd = 15
        LoadCmdSegment64  LoadCmd = 25
)

func (LoadCmd) GoString

func (i LoadCmd) GoString() string

func (LoadCmd) String

func (i LoadCmd) String() string

type Nlist32

An Nlist32 is a Mach-O 32-bit symbol table entry.

type Nlist32 struct {
        Name  uint32
        Type  uint8
        Sect  uint8
        Desc  uint16
        Value uint32
}

type Nlist64

An Nlist64 is a Mach-O 64-bit symbol table entry.

type Nlist64 struct {
        Name  uint32
        Type  uint8
        Sect  uint8
        Desc  uint16
        Value uint64
}

type Regs386

Regs386 is the Mach-O 386 register structure.

type Regs386 struct {
        AX    uint32
        BX    uint32
        CX    uint32
        DX    uint32
        DI    uint32
        SI    uint32
        BP    uint32
        SP    uint32
        SS    uint32
        FLAGS uint32
        IP    uint32
        CS    uint32
        DS    uint32
        ES    uint32
        FS    uint32
        GS    uint32
}

type RegsAMD64

RegsAMD64 is the Mach-O AMD64 register structure.

type RegsAMD64 struct {
        AX    uint64
        BX    uint64
        CX    uint64
        DX    uint64
        DI    uint64
        SI    uint64
        BP    uint64
        SP    uint64
        R8    uint64
        R9    uint64
        R10   uint64
        R11   uint64
        R12   uint64
        R13   uint64
        R14   uint64
        R15   uint64
        IP    uint64
        FLAGS uint64
        CS    uint64
        FS    uint64
        GS    uint64
}

type Section

type Section struct {
        SectionHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        io.ReaderAt
        // contains filtered or unexported fields
}

func (*Section) Data

func (s *Section) Data() ([]byte, error)

Data reads and returns the contents of the Mach-O section.
func (*Section) Open

func (s *Section) Open() io.ReadSeeker

Open returns a new ReadSeeker reading the Mach-O section.
type Section32

A Section32 is a 32-bit Mach-O section header.

type Section32 struct {
        Name     [16]byte
        Seg      [16]byte
        Addr     uint32
        Size     uint32
        Offset   uint32
        Align    uint32
        Reloff   uint32
        Nreloc   uint32
        Flags    uint32
        Reserve1 uint32
        Reserve2 uint32
}

type Section64

A Section64 is a 64-bit Mach-O section header.

type Section64 struct {
        Name     [16]byte
        Seg      [16]byte
        Addr     uint64
        Size     uint64
        Offset   uint32
        Align    uint32
        Reloff   uint32
        Nreloc   uint32
        Flags    uint32
        Reserve1 uint32
        Reserve2 uint32
        Reserve3 uint32
}

type SectionHeader

type SectionHeader struct {
        Name   string
        Seg    string
        Addr   uint64
        Size   uint64
        Offset uint32
        Align  uint32
        Reloff uint32
        Nreloc uint32
        Flags  uint32
}

type Segment

A Segment represents a Mach-O 32-bit or 64-bit load segment command.

type Segment struct {
        LoadBytes
        SegmentHeader

        // Embed ReaderAt for ReadAt method.
        // Do not embed SectionReader directly
        // to avoid having Read and Seek.
        // If a client wants Read and Seek it must use
        // Open() to avoid fighting over the seek offset
        // with other clients.
        io.ReaderAt
        // contains filtered or unexported fields
}

func (*Segment) Data

func (s *Segment) Data() ([]byte, error)

Data reads and returns the contents of the segment.
func (*Segment) Open

func (s *Segment) Open() io.ReadSeeker

Open returns a new ReadSeeker reading the segment.
type Segment32

A Segment32 is a 32-bit Mach-O segment load command.

type Segment32 struct {
        Cmd     LoadCmd
        Len     uint32
        Name    [16]byte
        Addr    uint32
        Memsz   uint32
        Offset  uint32
        Filesz  uint32
        Maxprot uint32
        Prot    uint32
        Nsect   uint32
        Flag    uint32
}

type Segment64

A Segment64 is a 64-bit Mach-O segment load command.

type Segment64 struct {
        Cmd     LoadCmd
        Len     uint32
        Name    [16]byte
        Addr    uint64
        Memsz   uint64
        Offset  uint64
        Filesz  uint64
        Maxprot uint32
        Prot    uint32
        Nsect   uint32
        Flag    uint32
}

type SegmentHeader

A SegmentHeader is the header for a Mach-O 32-bit or 64-bit load segment command.

type SegmentHeader struct {
        Cmd     LoadCmd
        Len     uint32
        Name    string
        Addr    uint64
        Memsz   uint64
        Offset  uint64
        Filesz  uint64
        Maxprot uint32
        Prot    uint32
        Nsect   uint32
        Flag    uint32
}

type Symbol

A Symbol is a Mach-O 32-bit or 64-bit symbol table entry.

type Symbol struct {
        Name  string
        Type  uint8
        Sect  uint8
        Desc  uint16
        Value uint64
}

type Symtab

A Symtab represents a Mach-O symbol table command.

type Symtab struct {
        LoadBytes
        SymtabCmd
        Syms []Symbol
}

type SymtabCmd

A SymtabCmd is a Mach-O symbol table command.

type SymtabCmd struct {
        Cmd     LoadCmd
        Len     uint32
        Symoff  uint32
        Nsyms   uint32
        Stroff  uint32
        Strsize uint32
}

type Thread

A Thread is a Mach-O thread state command.

type Thread struct {
        Cmd  LoadCmd
        Len  uint32
        Type uint32
        Data []uint32
}

type Type

A Type is the Mach-O file type, e.g. an object file, executable, or dynamic library.

type Type uint32

const (
        TypeObj    Type = 1
        TypeExec   Type = 2
        TypeDylib  Type = 6
        TypeBundle Type = 8
)
