# Το πακέτο os
```golang
import "os"
```
Περιεχόμενα:
* [Γενικά](#info)
* [Σταθερες](#const)
* [Μεταβλητές](#variables)


### <a name="info"></a>Γενικά
Το βασικό πακέτο os μας παρέχει την διασύνδεση με το λειτουργικό σύστημα, ανεξαρτήτως πλατφόρμας.Ο σχεδιασμός του είναι πανόμοιος με του Unix, παρόλο που ο χειρισμός σφαλμάτων είναι αντίστοιχος με αυτόν της Go: Οι απότυχημένες κλήσεις, επιστρέφουν τιμές τύπου σφάλματος, αντί για αριθμητικά σφάλματα. 
Συχνά, είναι διαθέσιμες περισσότερες πληροφορίες για το σφάλμα. Για παράδειγμα, εάν μια κλήση που δέχεται όρισμα ένα όνομα αρχείου,όπως Open ή Stat,αποτύχει, το σφαλμα, όταν εκτυπωθεί, θα συμπεριλάβει το αποτυχημένο όνομα αρχείου και θα είναι τύπου *PathError, το οποίο μπορεί να αποσυσκευαστεί για περισσότερες πληροφορες.

Ένα απλό παράδειγμα κώδικα για τον χειρισμό αρχείων (Άνοιγμα αρχείου για ανάγνωση δεδομένων):
```golang
    file, err := os.Open("file.go") // Για πρόσβαση ανάγνωσης.
    if err != nil {
            log.Fatal(err)
}
```
Εάν αποτύχει η κλήση Open (Άνοιξε), θα εμφανιστεί μια αυτοεπεξηγηματική συμβολοσειρά σφάλματος.
```bash
    open file.go: no such file or directory
```

Εάν είναι πετυχημένη η κλήση, τα δεδομένα από το αρχείο μπορούν να διαβαστούν ως μια φέτα (slice) από ψηφιολέξεις (bytes). Η Read (Ανάγνωσε) και η Write (Γράψε), απαριθμούν bytes, σύμφωνα με την χωρητικότητα της φέτας, όπως έχει ορισθεί.

```golang
    data := make([]byte, 100)     // δημιουργούμε μία slice με χωρητικότητα 100 bytes
    count, err := file.Read(data) // Καλούμε την Read() για να διαβάσουμε τα bytes
                                  // Επιστρέφει type File ή σφάλμα
    if err != nil {
            log.Fatal(err)
    fmt.Printf("read %d bytes: %q\n", count, data[:count])
```


### <a name="const"></a>Σταθερές (Constants)

Σημαίες για OpenFile, τυλιγμένες γυρώ από αυτές του λειτουργικού συστήματος. Δεν υλοποιούνται όλες, σε κάθε σύστημα.
```golang
   const (
           O_RDONLY int = syscall.O_RDONLY // Άνοιξε το αρχείο μόνο για ανάγνωση (read-only).
           O_WRONLY int = syscall.O_WRONLY // Άνοιξε το αρχείο μόνο για γράψιμο  (write-only).
           O_RDWR   int = syscall.O_RDWR   // Άνοιξε το αρχείο για ανάγνωση και γράψιμο (read-write).
           O_APPEND int = syscall.O_APPEND // Πρόσθεσε δεδομένα στο αρχείο καθώς γράφεις.
           O_CREATE int = syscall.O_CREAT  // Δημιούργησε ένα νέο αρχείο αν δεν υπάρχει κάποιο.
           O_EXCL   int = syscall.O_EXCL   // Χρησιμοποιείται με το O_CREATE. Το αρχείο δεν πρέπει να υπάρχει ήδη.
           O_SYNC   int = syscall.O_SYNC   // Άνοιξε για συγχρονισμένη είσοδο-έξοδο (I/O).
           O_TRUNC  int = syscall.O_TRUNC  // Εάν είναι δυνατό, κόψε το αρχείο όταν το ανοίγεις.
)
```
Αναζήτηση από ποιές τιμές.

```Ξεπερασμένο: Χρησιμοποίησε io.SeekStart, io.SeekCurrent, και io.SeekEnd.```

```golang
   const (
           SEEK_SET int = 0 // αναζήτηση σχετικά με την προέλευση του αρχείου
           SEEK_CUR int = 1 // αναζήτηση σχετικά με το τρέχον offset
           SEEK_END int = 2 // αναζήτηση σχετικά με το τέλος
)
```
 ```golang

    const (
            PathSeparator     = '/' // συγκεκριμένος διαχωριστής διαδρομης ανάλογα με το OS
            PathListSeparator = ':' // συγκεκριμένος διαχωριστής λίστας διαδρομης ανάλογα με το OS
)
 ```
 ```       
 DevNull είναι το όνομα της “μηδενικής συσκευής” σε ένα λειτουργικό σύστημα. 
 Στα συστήματα τύπου Unix, είναι "/dev/null"; στα Windows, "NUL".
 ```
 ```golang
    const DevNull = "/dev/null"
```
### <a name="variables"></a>Μεταβλητές
Ευέλικτα ανάλογα, συνηθισμένων σφαλμάτων κλήσης συστήματος.
 ```golang
   var (
            ErrInvalid    = errors.New("invalid argument") // οι μέθοδοι του File θα επιστρέψουν αυτό το σφάλμα όταν ο δέκτης είναι nil
            ErrPermission = errors.New("permission denied")
            ErrExist = errors.New("file already exists")
            ErrNotExist = errors.New("file does not exist")
            ErrClosed = errors.New("file already closed")
)
```
Τα Stdin, Stdout, and Stderr  είναι ανοικτά αρχεία που δείχνουν στους περιγραφείς αρχείων της τυπικής εξόδου, τυπικής εισόδου και τυπικού σφάλματος.

Σημειώστε ότι κατά το χρόνο εκτέλεσης η Go γράφει στο standard error τυχόν panics και κρασσαρίσματα: Το κλείσιμο του Stderr μπορεί να οδηγήσει αυτά τα μηνύματα να πάνε κάπου αλλού, όπως, ίσως σε ένα αρχείο που θα ανοιχτεί αργότερα.
 ```golang
   var (
           Stdin  = NewFile(uintptr(syscall.Stdin), "/dev/stdin")
           Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
           Stderr = NewFile(uintptr(syscall.Stderr), "/dev/stderr")
)
```
Η Args κρατάει τα ορίσματα της γραμμής εντολών, ξεκινώντας από το όνομα του προγράμματος.
 ```golang
var Args []string
```
### Οι διαθέσιμες συναρτήσεις
```golang
func Chdir(dir string) error
```
```golang
func Chmod(name string, mode FileMode) error
```
```golang
func Chown(name string, uid, gid int) error
```
```golang
func Chtimes(name string, atime time.Time, mtime time.Time) error func Clearenv()
```
```golang
func Environ() []string
```
```golang
func Executable() (string, error)
```
```golang
func Exit(code int)
```
```golang
func Expand(s string, mapping func(string) string) string
```
```golang
func ExpandEnv(s string) string
```
```golang
func Getegid() int
```
```golang
func Getenv(key string) string
```
```golang
func Geteuid() int
```
```golang
func Getgid() int
```
```golang
func Getgroups() ([]int, error)
```
```golang
func Getpagesize() int
```
```golang
func Getpid() int
```
```golang
func Getppid() int
```
```golang
func Getuid() int
```
```golang
func Getwd() (dir string, err error)
```
```golang
func Hostname() (name string, err error)
```
```golang
func IsExist(err error) bool
```
 ```golang
func IsNotExist(err error) bool
```
```golang
func IsPathSeparator(c uint8) bool
```
```golang
func IsPermission(err error) bool
```
```golang
func Lchown(name string, uid, gid int) error
```
```golang
func Link(oldname, newname string) error
```
```golang
func LookupEnv(key string) (string, bool)
```
```golang
func Mkdir(name string, perm FileMode) error
```
```golang
func MkdirAll(path string, perm FileMode) error 
```
```golang
func NewSyscallError(syscall string, err error) error 
```
```golang
func Readlink(name string) (string, error)
```
```golang
func Remove(name string) error
```
```golang
func RemoveAll(path string) error
```
```golang
func Rename(oldpath, newpath string) error
```
```golang
func SameFile(fi1, fi2 FileInfo) bool
```
```golang
func Setenv(key, value string) error
```
```golang
func Symlink(oldname, newname string) error
```
```golang
func TempDir() string
```
```golang
func Truncate(name string, size int64) error
```
```golang
func Unsetenv(key string) error
```
### type File
```golang
func Create(name string) (*File, error)
```
```golang
func NewFile(fd uintptr, name string) *File
```
```golang
func Open(name string) (*File, error)
```
```golang
func OpenFile(name string, flag int, perm FileMode) (*File, error) func Pipe() (r *File, w *File, err error)
```
```golang
func (f *File) Chdir() error
```
```golang
func (f *File) Chmod(mode FileMode) error
```
```golang
func (f *File) Chown(uid, gid int) error
```
```golang
func (f *File) Close() error
```
```golang
func (f *File) Fd() uintptr
```
```golang
func (f *File) Name() string
```
```golang
func (f *File) Read(b []byte) (n int, err error)
```
```golang
func (f *File) ReadAt(b []byte, off int64) (n int, err error)
```
```golang
func (f *File) Readdir(n int) ([]FileInfo, error)
```
```golang
func (f *File) Readdirnames(n int) (names []string, err error)
```
```golang
func (f *File) Seek(offset int64, whence int) (ret int64, err error)
```
```golang
func (f *File) Stat() (FileInfo, error)
```
```golang
func (f *File) Sync() error
```
```golang
func (f *File) Truncate(size int64) error
```
```golang
func (f *File) Write(b []byte) (n int, err error)
```
```golang
func (f *File) WriteAt(b []byte, off int64) (n int, err error)
```
```golang
func (f *File) WriteString(s string) (n int, err error)
```

### type FileInfo
```golang
func Lstat(name string) (FileInfo, error) func Stat(name string) (FileInfo, error)
```
### type FileMode
```golang
func (m FileMode) IsDir() bool
```
```golang
func (m FileMode) IsRegular() bool 
```
```golang
func (m FileMode) Perm() FileMode 
```
```golang
func (m FileMode) String() string
```
### type LinkError
```golang
func (e *LinkError) Error() string
```
### type PathError
```golang
func (e *PathError) Error() string
```
### type ProcAttr 

### type Process

```golang
func FindProcess(pid int) (*Process, error)
```
```golang
func StartProcess(name string, argv []string, attr *ProcAttr) (*Process, error) 
```
```golang
func (p *Process) Kill() error
```
```golang
func (p *Process) Release() error
```
```golang
func (p *Process) Signal(sig Signal) error
```
```golang
func (p *Process) Wait() (*ProcessState, error)
```
### type ProcessState
```golang
func (p *ProcessState) Exited() bool
```
```golang
func (p *ProcessState) Pid() int
```
```golang
func (p *ProcessState) String() string
```
```golang
func (p *ProcessState) Success() bool
```
```golang
func (p *ProcessState) Sys() interface{}
```
```golang
func (p *ProcessState) SysUsage() interface{}
```
```golang
func (p *ProcessState) SystemTime() time.Duration 
```
```golang
func (p *ProcessState) UserTime() time.Duration
```
### type Signal







