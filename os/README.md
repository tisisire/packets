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
* **func Chdir** (το όνομα προέρχεται από το **Ch**ange working **dir**ectory)

```golang
func Chdir(dir string) error
```
H Chdir αλλάζει τον τωρινό κατάλογο εργασίας με αυτόν που δίνεται. Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *PathError.
Παράδειγμα:
```golang
package main

import (
  "fmt"
  "os"
)

func main() {
  s, _ := os.Getwd() // ζητάμε τον τωρινό κατάλογο εργασίας
  fmt.Println(s) // εκτύπωσε τον

  if err := os.Chdir("/tmp"); err != nil { // Εκτός και αν υπάρχει σφάλμα, άλλαξε κατάλογο και πήγαινε στον /tmp
    panic(err) // αν υπάρχει σφάλμα τερμάτισε και δείξε μας το σφάλμα
  }
  s, _ = os.Getwd() // ζητάμε πάλι τον τωρινό κατάλογο εργασίας
  fmt.Println(s) // εκτύπωσε τον (Τώρα είμαστε στον /tmp ή οποιοδήποτε symlink υπάρχει από το λειτουργικό σύστημα)

}
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/0rJjmTgkK6)_

* **func Chmod** (το όνομα προέρχεται από το **Ch**ange **mod**e)
```golang
func Chmod(name string, mode FileMode) error
```
Η Chmod αλλάζει τον τρόπο λειτουργιας του δοθέντος αρχείου σύμφωνα με τον τρόπο που δίνεται. Εάν το αρχείο είναι ένας συμβολικός σύνδεσμος, αλλάζει τον τρόπο λειτουργίας του συνδεδεμένου στόχου. Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *PathError.

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	err := os.Chmod("file.txt", 0777) // Άλλαξε τον τρόπο λειτουργίας του file.txt σε 0777
	if err != nil {
		fmt.Println(err) // Επιστρέφει err αν δεν υπάρχει το αρχείο ή δεν μπορεί να αλλάξει τρόπο λειτουργας
	}
}

```
Output:
```bash
chmod file.txt: No such file or directory
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/MZYLHyuaJH)_

Χρήσιμο: _[Unix Permissions Calculator](http://permissions-calculator.org/)_

* **func Chown** (το όνομα προέρχεται από το **Ch**ange **own**er)
```golang
func Chown(name string, uid, gid int) error
```
Η Chmod αλλάζει το αριθμητικό user id (uid) και το group id (gid) του δοθέντος αρχείου. Εάν το αρχείο είναι ένας συμβολικός σύνδεσμος, αλλάζει το αριθμητικό user id (uid) και το group id (gid) του συνδεδεμένου στόχου. Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *PathError.

* **func Chtimes** (το όνομα προέρχεται από το **Ch**ange **times**)
```golang
func Chtimes(name string, atime time.Time, mtime time.Time) error func Clearenv()
```
Η Chtimes αλλάζει την ημερομηνία προσπέλασης και τροποποίησης του δοθέντος αρχείου, με τρόπο παρόμοιο σαν τις συναρτήσεις utime() και utimes() του Unix.
Το υποκείμενο σύστημα αρχείων μπορεί να περικόψει ή να στρογγυλοποιήσει τις τιμές σε μιά λιγότερο ακριβής μονάδα.Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *PathError.


Παράδειγμα:
```golang
package main

//Κάνουμε import τα πακέτα log και time, μαζί με το os
import (
	"log"
	"os"
	"time"
)

func main() {
	mtime := time.Date(2016, time.February, 1, 3, 4, 5, 0, time.UTC) // Θέτουμε ημερομηνία τροποποίησης
	atime := time.Date(2017, time.March, 2, 4, 5, 6, 0, time.UTC) // θέτουμε ημερομηνία προσπέλασης
	if err := os.Chtimes("file.txt", atime, mtime); err != nil {
		log.Fatal(err) // εάν υπάρχει σφάλμα (όπως στην περίπτωσή μας) εκτύπωσέ το
	}
}


```

_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/cWt5dmfHyW)_
* **func Clearenv** (το όνομα προέρχεται από το **Clear** **env**ironment)
```golang
func Clearenv()
```
Η Clearenv διαγράφει όλες τις μεταβλητές περιβάλλοντος.
* **func Environ** (το όνομα προέρχεται από το **Environ**ment)

```golang
func Environ() []string
```
Η Environ επιστρέφει ένα αντίγραφο από συμβολοσειρές που αντιπροσωπεύουν τις μεταβλητές περιβάλλοντος, στην μορφή "κλειδί=τιμή".

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
	"strings"
)

func main() {
	var env []string
	env = os.Environ()

	fmt.Println("Η λίστα με τις μεταβλητές περιβάλλοντος : \n")

	for k, v := range env {
		name := strings.Split(v, "=") // split by = sign

		fmt.Printf("[%d] %s : %v\n", k, name[0], name[1])
	}
}
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/WOqgr4F1Kk)_
* **func Executable**
```golang
func Executable() (string, error)
```
Η Executable επιστρέφει τη διαδρομή του αρχείου που ξεκίνησε την τρέχουσα διαδικασία. Δεν είναι εγγυημένο ότι η διαδρομή δείχνει το σωστό εκτελέσιμο. Εάν μια συμβολική σύνδεση (symlink) έχει χρησιμοποιηθεί για να ξεκινήσει η διαδικασία, το αποτέλεσμα, βασιζόμενο στο λειτουργικό σύστημα, μπορεί να είναι η συμβολική σύνδεση ή η διαδρομή που δείχνει σε αυτή. Εάν χρειαζόμαστε ένα σταθερο αποτέλεσαμ, η path/filepath.EvalSymlinks μπορεί να βοηθήσει.   
Η Executable επιστρέφει μια απόλυτη διαδρομή εκτός και αν προκύψει σφάλμα. Η βασική χρήση του είναι να βρίσκει τους πόρους τους σχετικούς με το εκτελέσιμο. Η Executable δεν υποστηρίζεται σε nacl ή σε OpenBSD (εκτός και αν κάνουμε mount την procfs)

* **func Exit**
```golang
func Exit(code int)
```
Η Exit προκαλεί τον τερματισμό της τρέχουσας διαδικασας με τον δοθέντα κωδικό κατάστασης. Κατά συνθήκη, ο κωδικός μηδέν υποδεικνύει επιτυχα, και οποιοσδήποτε άλλος, σφάλμα. Το πρόγραμμα τερματίζει αμέσως: Οι αναβαλλόμενες συναρτήσεις δεν τρέχουν.

* **func Expand**
```golang
func Expand(s string, mapping func(string) string) string
```
Η Expand αντικαθιστά την ${var} ή την $var σε μία συμβολοσείρα,  βασιζόμενη στην δοθείσα λειτουργα αντιστοίχισης. Για παράδειγμα η os.ExpandEnv(s) είναι παρόμοια με την os.Expand(s, os.Getenv).

* **func ExpandEnv** (το όνομα προέρχεται από το **Expand** **Env**ironment)
```golang
func ExpandEnv(s string) string
```
Η ExpandEnv αντικαθιστά την ${var} ή την $var σε μία συμβολοσειρά, βασιζόμενη στις τιμές των τρεχουσών μεταβλητών περιβάλλοντος. Αναφορές σε απροσδιόριστες μεταβλητές θα αντικατασταθούν από την κενή συμβολοσειρά. 

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	fmt.Println(os.ExpandEnv("$USER lives in ${HOME}."))
}
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/CS_9cNTUDv)_

* **func Gategid** (το όνομα προέρχεται από το **Get** **e**ffective **g**roup **id**)
```golang
func Getegid() int
```
Η Getegid  επιστέφει το αριθμητικό effective group id του καλούντος.

* **func Getenv** (το όνομα προέρχεται από το **Get** **env**ironment)
```golang
func Getenv(key string) string
```
Η Getenv ανακτά την μεταβλητήτη περιβάλλοντος, σύμφωνα με το δοθέντα κλειδί. Επιστρέφει την τιμή της, η οποία θα είναι κενή εάν η μεταβλητή δεν είναι παρούσα.  the value of the environment variable named by the key. It returns the value, which will be empty if the variable is not present. Για την διάκριση ανάμεσα σε κενή τιμή και σε μία ανενεργή μεταβλητή, χρησιμοποίησε την LookupEnv.

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	fmt.Printf("%s lives in %s.\n", os.Getenv("USER"), os.Getenv("HOME"))
}
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/b48PSRsQcW)_

* **func Geteuid** (το όνομα προέρχεται από το **Get** **e**ffective **u**ser **id**)
```golang
func Geteuid() int
```
Η Geteuid επιστρέφει το αριθμητικό effective user id του καλούντος.

* **func Getgid** (το όνομα προέρχεται από το **Get** **g**roup **id**)
```golang
func Getgid() int
```
Η Getgid επιστρέφιε το group id του καλούντος.

* **func Getgroups**
```golang
func Getgroups() ([]int, error)
```
Η Getgroups επιστρέφει μια λίστα από αριμητικά ids των groups που ανήκει ο καλούντας.

* **func Getpagesize**
```golang
func Getpagesize() int
```
Η Getpagesize επιστρέφει το μέγεθος της σελίδας μνήμης του υποκείμενου συστήματος.

* **func Getpid** (το όνομα προέρχεται από το **Get** **p**rocess **id**)
```golang
func Getpid() int
```
Η Getpid επιστρέφει το process id του καλούντος.

* **func Getppid** (το όνομα προέρχεται από το **Get** **p**arent **p**rocess **id**)
```golang
func Getppid() int
```
Η Getppid επιστρέφει το process id της γονικής διαδικασίας του καλούντος.

* **func Getuid** (το όνομα προέρχεται από το **Get** **u**ser **id**)
```golang
func Getuid() int
```
Η Getuid επιστρέφει το user id του καλούντος.

* **func Getwd** (το όνομα προέρχεται από το **Get** **w**orking **d**irectory)
```golang
func Getwd() (dir string, err error)
```
Η Getwd επιστρέφει το όνομα της ριζικής διαδρομής που αντιστοιχεί στον τρέχον κατάλογο. Εάν ο τωρινός κατάλογος μπορεί να προσπελαστεί διαμέσου πολλαπλών διαδρομών (εξαιτίας συμβολικών συνδέσμων), η Getwd μπορεί να επιστρέψει οποιαδήποτε από αυτές.

* **func Hostname** 
```golang
func Hostname() (name string, err error)
```
Η Hostname επιστρέφει το όνομα του συστήματος όπως αναφέρεται από τον πυρήνα.
* **func IsExist** 
```golang
func IsExist(err error) bool
```
IsExist returns a boolean indicating whether the error is known to report that a file or directory already exists. It is satisfied by ErrExist as well as some syscall errors.


* **func IsNotExist** 
 ```golang
func IsNotExist(err error) bool
```

IsNotExist returns a boolean indicating whether the error is known to report that a file or directory does not exist. It is satisfied by ErrNotExist as well as some syscall errors.

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	filename := "a-nonexistent-file"
	if _, err := os.Stat(filename); os.IsNotExist(err) {
		fmt.Printf("file does not exist")
	}
}

```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/DwKS5-aAid)_

* **func IsPathSeparator** 
```golang
func IsPathSeparator(c uint8) bool
```
IsPathSeparator reports whether c is a directory separator character.
* **func IsPermission** 
```golang
func IsPermission(err error) bool
```
IsPermission returns a boolean indicating whether the error is known to report that permission is denied. It is satisfied by ErrPermission as well as some syscall errors.
* **func Lchown** (το όνομα προέρχεται από το symbolic **L**ink **c**hange **own**er)
```golang
func Lchown(name string, uid, gid int) error
```
Lchown changes the numeric uid and gid of the named file. If the file is a symbolic link, it changes the uid and gid of the link itself. If there is an error, it will be of type *PathError.
* **func Link**
```golang
func Link(oldname, newname string) error
```
Link creates newname as a hard link to the oldname file. If there is an error, it will be of type *LinkError.
* **func LookupEnv** (το όνομα προέρχεται από το **Lookup** **Env**ironment)
```golang
func LookupEnv(key string) (string, bool)
```
LookupEnv retrieves the value of the environment variable named by the key. If the variable is present in the environment the value (which may be empty) is returned and the boolean is true. Otherwise the returned value will be empty and the boolean will be false.

Παράδειγμα:
```golang
package main

import (
	"fmt"
	"os"
)

func main() {
	show := func(key string) {
		val, ok := os.LookupEnv(key)
		if !ok {
			fmt.Printf("%s not set\n", key)
		} else {
			fmt.Printf("%s=%s\n", key, val)
		}
	}
	show("USER")
	show("GOPATH")
}
```
_Δοκίμασε το στο [Go Playground](https://play.golang.org/p/50uEk6VPb7)_

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
func OpenFile(name string, flag int, perm FileMode) (*File, error) 
```
```golang
func Pipe() (r *File, w *File, err error)
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
func Lstat(name string) (FileInfo, error)
```
```golang
func Stat(name string) (FileInfo, error)
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
### type SyscallError
```golang
func (e *SyscallError) Error() string
```





