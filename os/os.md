# Το πακέτο os
```golang
import "os"
```
Περιεχόμενα:
* [Γενικά](#info)
* [Σταθερες](#const)
* [Μεταβλητές](#variables)
* [Συναρτήσεις - Μέθοδοι](#funcs)


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
### <a name="funcs"></a> Οι ενσωματωμένες Συναρτήσεις - Μέθοδοι
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
Η Executable επιστρέφει τη διαδρομή του αρχείου που ξεκίνησε την τρέχουσα διαδικασία. Δεν είναι εγγυημένο ότι η διαδρομή δείχνει το σωστό εκτελέσιμο. Εάν μια συμβολική σύνδεση (symlink) έχει χρησιμοποιηθεί για να ξεκινήσει η διαδικασία, το αποτέλεσμα, βασιζόμενο στο λειτουργικό σύστημα, μπορεί να είναι η συμβολική σύνδεση ή η διαδρομή που δείχνει σε αυτή. Εάν χρειαζόμαστε ένα σταθερο αποτέλεσμα, η path/filepath.EvalSymlinks μπορεί να βοηθήσει.   
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
Η IsExist επιστρέφει μια boolean τιμή (true or false) που δηλώνει κατα πόσον το σφάλμα ότι το αρχείο ή ο κατάλογος υπάρχει ήδη, είναι γνωστό και μπορεί να αναφερθεί. Αυτό ικανοποιείται από την ErrExist και από άλλα σφάλματα κλήσεων συστήματος. 

* **func IsNotExist** 
 ```golang
func IsNotExist(err error) bool
```

Η IsNotExist επιστρέφει μια boolean τιμή (true or false) που δηλώνει κατα πόσον το σφάλμα ότι το αρχείο ή ο κατάλογος δεν υπάρχει, είναι γνωστό και μπορεί να αναφερθεί. υτό ικανοποιείται από την ErrNotExist και από άλλα σφάλματα κλήσεων συστήματος. 
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
Η IsPathSeparator αναφέρει κατά πόσον ο χαρακτήρας c είναι διαχωριστικό καταλόγων.

* **func IsPermission** 
```golang
func IsPermission(err error) bool
```
Η IsPermission μια boolean τιμή (true or false) που δηλώνει κατα πόσον το σφάλμα ότι η πρόσβαση δεν είναι επιτρεπτή είναι γνωστό και μπορεί να αναφερθεί. Αυτό ικανοποιείται από την ErrPermission και από άλλα σφάλματα κλήσεων συστήματος.  

* **func Lchown** (το όνομα προέρχεται από το **L**ink **c**hange **own**er)
```golang
func Lchown(name string, uid, gid int) error
```
Η Lchown αλλάζει το αριθμητικό uid και gid του δοθέντος αρχείου. Εάν το αρχείο είναι ένας συμβολικός σύνδεσμος, αλλάζει το αριθμητικό uid και gid του συνδέσμου αυτού. Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *PathError.

* **func Link**
```golang
func Link(oldname, newname string) error
```
Η Link δημιουργεί ένα νεό όνομα (newname) ως μόνιμη σύνδεση στο προηγούμενο όνομα (oldname) του αρχείου.Εάν υπάρχει σφάλμα αυτό θα είναι τύπου *LinkError.

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

* **func Mkdir** (το όνομα προέρχεται από το **M**a**k**e **dir**ectory)
```golang
func Mkdir(name string, perm FileMode) error
```
Mkdir creates a new directory with the specified name and permission bits. If there is an error, it will be of type *PathError. 

* **func MkdirAll** (το όνομα προέρχεται από το **M**a**k**e **dir**ectory **All**)
```golang
func MkdirAll(path string, perm FileMode) error 
```
MkdirAll creates a directory named path, along with any necessary parents, and returns nil, or else returns an error. The permission bits perm are used for all directories that MkdirAll creates. If path is already a directory, MkdirAll does nothing and returns nil. 

* **func NewSyscallError** 
```golang
func NewSyscallError(syscall string, err error) error 
```

NewSyscallError returns, as an error, a new SyscallError with the given system call name and error details. As a convenience, if err is nil, NewSyscallError returns nil. 

* **func Readlink** 
```golang
func Readlink(name string) (string, error)
```
Readlink returns the destination of the named symbolic link. If there is an error, it will be of type *PathError. 
 
* **func Remove** 
```golang
func Remove(name string) error
```

Remove removes the named file or directory. If there is an error, it will be of type *PathError. 
 
* **func RemoveAll** 
```golang
func RemoveAll(path string) error
```
RemoveAll removes path and any children it contains. It removes everything it can but returns the first error it encounters. If the path does not exist, RemoveAll returns nil (no error). 
 
* **func Rename** 
```golang
func Rename(oldpath, newpath string) error
```
Rename renames (moves) oldpath to newpath. If newpath already exists and is not a directory, Rename replaces it. OS-specific restrictions may apply when oldpath and newpath are in different directories. If there is an error, it will be of type *LinkError. 

* **func SameFile** 
```golang
func SameFile(fi1, fi2 FileInfo) bool
```
SameFile reports whether fi1 and fi2 describe the same file. For example, on Unix this means that the device and inode fields of the two underlying structures are identical; on other systems the decision may be based on the path names. SameFile only applies to results returned by this package's Stat. It returns false in other cases.

* **func Setenv** (το όνομα προέρχεται από το  **Set** **env**ironment)
```golang
func Setenv(key, value string) error
```
Setenv sets the value of the environment variable named by the key. It returns an error, if any. 
 
* **func Symlink** (το όνομα προέρχεται από το **Sym**bolic **link**)
```golang
func Symlink(oldname, newname string) error
```
Symlink creates newname as a symbolic link to oldname. If there is an error, it will be of type *LinkError. 

* **func TempDir** (το όνομα προέρχεται από το **Temp**orary **Dir**ectory)
```golang
func TempDir() string
```
TempDir returns the default directory to use for temporary files

* **func Truncate**
```golang
func Truncate(name string, size int64) error
```
Truncate changes the size of the named file. If the file is a symbolic link, it changes the size of the link's target. If there is an error, it will be of type *PathError.

* **func Unsetenv** (το όνομα προέρχεται από το **Unset** **env**ironment)
```golang
func Unsetenv(key string) error
```
Unsetenv unsets a single environment variable. 
Παράδειγμα:
```golang
package main

import (
	"os"
)

func main() {
	os.Setenv("TMPDIR", "/my/tmp")
	defer os.Unsetenv("TMPDIR")
}
```


### type File
File represents an open file descriptor.
```golang
type File struct {
        // contains filtered or unexported fields
}
```
* **func Create**
```golang
func Create(name string) (*File, error)
```
Create creates the named file with mode 0666 (before umask), truncating it if it already exists. If successful, methods on the returned File can be used for I/O; the associated file descriptor has mode O_RDWR. If there is an error, it will be of type *PathError. 

* **func NewFile**
```golang
func NewFile(fd uintptr, name string) *File
```
 NewFile returns a new File with the given file descriptor and name. 
 
* **func Open**
```golang
func Open(name string) (*File, error)
```
Open opens the named file for reading. If successful, methods on the returned file can be used for reading; the associated file descriptor has mode O_RDONLY. If there is an error, it will be of type *PathError.

* **func OpenFile**
```golang
func OpenFile(name string, flag int, perm FileMode) (*File, error) 
```
OpenFile is the generalized open call; most users will use Open or Create instead. It opens the named file with specified flag (O_RDONLY etc.) and perm, (0666 etc.) if applicable. If successful, methods on the returned File can be used for I/O. If there is an error, it will be of type *PathError. 

Παράδειγμα:
```golang
package main

import (
	"log"
	"os"
)

func main() {
	f, err := os.OpenFile("notes.txt", os.O_RDWR|os.O_CREATE, 0755)
	if err != nil {
		log.Fatal(err)
	}
	if err := f.Close(); err != nil {
		log.Fatal(err)
	}
}
```

* **func Pipe**
```golang
func Pipe() (r *File, w *File, err error)
```

Pipe returns a connected pair of Files; reads from r return bytes written to w. It returns the files and an error, if any. 

* **method Chdir**
```golang
func (f *File) Chdir() error
```
hdir changes the current working directory to the file, which must be a directory. If there is an error, it will be of type *PathError. 

* **method Chmod**
```golang
func (f *File) Chmod(mode FileMode) error
```

Chmod changes the mode of the file to mode. If there is an error, it will be of type *PathError. 
 
* **method Chown**
```golang
func (f *File) Chown(uid, gid int) error
```
Chown changes the numeric uid and gid of the named file. If there is an error, it will be of type *PathError.

* **method Close**
```golang
func (f *File) Close() error
```
Close closes the File, rendering it unusable for I/O. It returns an error, if any. 

* **method Fd**
```golang
func (f *File) Fd() uintptr
```
Fd returns the integer Unix file descriptor referencing the open file. The file descriptor is valid only until f.Close is called or f is garbage collected.

* **method Name**
```golang
func (f *File) Name() string
```
Name returns the name of the file as presented to Open.
* **method Read**
```golang
func (f *File) Read(b []byte) (n int, err error)
```
Read reads up to len(b) bytes from the File. It returns the number of bytes read and any error encountered. At end of file, Read returns 0, io.EOF.

* **method ReadAt**
```golang
func (f *File) ReadAt(b []byte, off int64) (n int, err error)
```
ReadAt reads len(b) bytes from the File starting at byte offset off. It returns the number of bytes read and the error, if any. ReadAt always returns a non-nil error when n < len(b). At end of file, that error is io.EOF.

* **method Readdir**
```golang
func (f *File) Readdir(n int) ([]FileInfo, error)
```
Readdir reads the contents of the directory associated with file and returns a slice of up to n FileInfo values, as would be returned by Lstat, in directory order. Subsequent calls on the same file will yield further FileInfos.

If n > 0, Readdir returns at most n FileInfo structures. In this case, if Readdir returns an empty slice, it will return a non-nil error explaining why. At the end of a directory, the error is io.EOF.

If n <= 0, Readdir returns all the FileInfo from the directory in a single slice. In this case, if Readdir succeeds (reads all the way to the end of the directory), it returns the slice and a nil error. If it encounters an error before the end of the directory, Readdir returns the FileInfo read until that point and a non-nil error. 

* **method Readdirnames**
```golang
func (f *File) Readdirnames(n int) (names []string, err error)
```
Readdirnames reads and returns a slice of names from the directory f.

If n > 0, Readdirnames returns at most n names. In this case, if Readdirnames returns an empty slice, it will return a non-nil error explaining why. At the end of a directory, the error is io.EOF.

If n <= 0, Readdirnames returns all the names from the directory in a single slice. In this case, if Readdirnames succeeds (reads all the way to the end of the directory), it returns the slice and a nil error. If it encounters an error before the end of the directory, Readdirnames returns the names read until that point and a non-nil error. 

* **method Seek**
```golang
func (f *File) Seek(offset int64, whence int) (ret int64, err error)
```
Seek sets the offset for the next Read or Write on file to offset, interpreted according to whence: 0 means relative to the origin of the file, 1 means relative to the current offset, and 2 means relative to the end. It returns the new offset and an error, if any. The behavior of Seek on a file opened with O_APPEND is not specified. 

* **method Stat**
```golang
func (f *File) Stat() (FileInfo, error)
```
Stat returns the FileInfo structure describing file. If there is an error, it will be of type *PathError. 
* **method Sync**
```golang
func (f *File) Sync() error
```
Sync commits the current contents of the file to stable storage. Typically, this means flushing the file system's in-memory copy of recently written data to disk.

* **method Truncate**
```golang
func (f *File) Truncate(size int64) error
```
Truncate changes the size of the file. It does not change the I/O offset. If there is an error, it will be of type *PathError. 
* **method Write**
```golang
func (f *File) Write(b []byte) (n int, err error)
```
Write writes len(b) bytes to the File. It returns the number of bytes written and an error, if any. Write returns a non-nil error when n != len(b)

* **method WriteAt**
```golang
func (f *File) WriteAt(b []byte, off int64) (n int, err error)
```
WriteAt writes len(b) bytes to the File starting at byte offset off. It returns the number of bytes written and an error, if any. WriteAt returns a non-nil error when n != len(b).

* **method WriteString**
```golang
func (f *File) WriteString(s string) (n int, err error)
```
WriteString is like Write, but writes the contents of string s rather than a slice of bytes. 
 
### type FileInfo

A FileInfo describes a file and is returned by Stat and Lstat.
```golang
type FileInfo interface {
        Name() string       // base name of the file
        Size() int64        // length in bytes for regular files; system-dependent for others
        Mode() FileMode     // file mode bits
        ModTime() time.Time // modification time
        IsDir() bool        // abbreviation for Mode().IsDir()
        Sys() interface{}   // underlying data source (can return nil)
}
```
* **func Lstat**
```golang
func Lstat(name string) (FileInfo, error)
```
Lstat returns a FileInfo describing the named file. If the file is a symbolic link, the returned FileInfo describes the symbolic link. Lstat makes no attempt to follow the link. If there is an error, it will be of type *PathError. 

* **func Stat**
```golang
func Stat(name string) (FileInfo, error)
```
 
Stat returns a FileInfo describing the named file. If there is an error, it will be of type *PathError. 
 
### type FileMode
 A FileMode represents a file's mode and permission bits. The bits have the same definition on all systems, so that information about files can be moved from one system to another portably. Not all bits apply to all systems. The only required bit is ModeDir for directories.

```golang
type FileMode uint32
```

The defined file mode bits are the most significant bits of the FileMode. The nine least-significant bits are the standard Unix rwxrwxrwx permissions. The values of these bits should be considered part of the public API and may be used in wire protocols or disk representations: they must not be changed, although new bits might be added. 

```golang
const (
        // The single letters are the abbreviations
        // used by the String method's formatting.
        ModeDir        FileMode = 1 << (32 - 1 - iota) // d: is a directory
        ModeAppend                                     // a: append-only
        ModeExclusive                                  // l: exclusive use
        ModeTemporary                                  // T: temporary file (not backed up)
        ModeSymlink                                    // L: symbolic link
        ModeDevice                                     // D: device file
        ModeNamedPipe                                  // p: named pipe (FIFO)
        ModeSocket                                     // S: Unix domain socket
        ModeSetuid                                     // u: setuid
        ModeSetgid                                     // g: setgid
        ModeCharDevice                                 // c: Unix character device, when ModeDevice is set
        ModeSticky                                     // t: sticky

        // Mask for the type bits. For regular files, none will be set.
        ModeType = ModeDir | ModeSymlink | ModeNamedPipe | ModeSocket | ModeDevice

        ModePerm FileMode = 0777 // Unix permission bits
)
```


Παράδειγμα:
```golang
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	fi, err := os.Stat("some-filename")
	if err != nil {
		log.Fatal(err)
	}

	switch mode := fi.Mode(); {
	case mode.IsRegular():
		fmt.Println("regular file")
	case mode.IsDir():
		fmt.Println("directory")
	case mode&os.ModeSymlink != 0:
		fmt.Println("symbolic link")
	case mode&os.ModeNamedPipe != 0:
		fmt.Println("named pipe")
	}
}
```

* **method IsDir**
```golang
func (m FileMode) IsDir() bool
```
IsDir reports whether m describes a directory. That is, it tests for the ModeDir bit being set in m.
* **method IsRegular**
```golang
func (m FileMode) IsRegular() bool 
```
IsRegular reports whether m describes a regular file. That is, it tests that no mode type bits are set.
* **method Perm**
```golang
func (m FileMode) Perm() FileMode 
```
Perm returns the Unix permission bits in m.
* **method String**
```golang
func (m FileMode) String() string
```
### type LinkError

LinkError records an error during a link or symlink or rename system call and the paths that caused it.
```golang
type LinkError struct {
	Op  string
	Old string
	New string
	Err error
}
```

* **method Error**
```golang
func (e *LinkError) Error() string
```
### type PathError

PathError records an error and the operation and file path that caused it.
```golang
type PathError struct {
	Op   string
	Path string
	Err  error
}
```
* **method Error**
```golang
func (e *PathError) Error() string
```
### type ProcAttr 

ProcAttr holds the attributes that will be applied to a new process started by StartProcess.
```golang
type ProcAttr struct {
	// If Dir is non-empty, the child changes into the directory before
	// creating the process.
	Dir string
	// If Env is non-nil, it gives the environment variables for the
	// new process in the form returned by Environ.
	// If it is nil, the result of Environ will be used.
	Env []string
	// Files specifies the open files inherited by the new process. The
	// first three entries correspond to standard input, standard output, and
	// standard error. An implementation may support additional entries,
	// depending on the underlying operating system. A nil entry corresponds
	// to that file being closed when the process starts.
	Files []*File
	// Operating system-specific process creation attributes.
	// Note that setting this field means that your program
	// may not execute properly or even compile on some
	// operating systems.
	Sys *syscall.SysProcAttr
}
```
### type Process

Process stores the information about a process created by StartProcess.
```golang
   type Process struct {
           Pid int
           // contains filtered or unexported fields
}
```
* **func FindProcess**
```golang
func FindProcess(pid int) (*Process, error)
```

FindProcess looks for a running process by its pid.
The Process it returns can be used to obtain information about the underlying operating system process.
On Unix systems, FindProcess always succeeds and returns a Process for the given pid, regardless of whether the process exists.

* **func StartProcess**
```golang
func StartProcess(name string, argv []string, attr *ProcAttr) (*Process, error) 
```
StartProcess starts a new process with the program, arguments and attributes specified by name, argv and attr. StartProcess is a low­level interface. The os/exec package provides higher­level interfaces.
If there is an error, it will be of type *PathError.

* **method Kill**
```golang
func (p *Process) Kill() error
```
Kill causes the Process to exit immediately.

* **method Release**
```golang
func (p *Process) Release() error
```
Release releases any resources associated with the Process p, rendering it unusable in the future. Release only
needs to be called if Wait is not.

* **method Signal**
```golang
func (p *Process) Signal(sig Signal) error
```
Signal sends a signal to the Process. Sending Interrupt on Windows is not implemented.

* **method Wait**
```golang
func (p *Process) Wait() (*ProcessState, error)
```
Wait waits for the Process to exit, and then returns a ProcessState describing its status and an error, if any. Wait releases any resources associated with the Process. On most operating systems, the Process must be a child of the current process or an error will be returned.

### type ProcessState

ProcessState stores information about a process, as reported by Wait.
```golang
   type ProcessState struct {
           // contains filtered or unexported fields
}
```
* **method Exited**
```golang
func (p *ProcessState) Exited() bool
```

Exited reports whether the program has exited.
* **method Pid**
```golang
func (p *ProcessState) Pid() int
```
Pid returns the process id of the exited process.
* **method String**
```golang
func (p *ProcessState) String() string
```
* **method Success**
```golang
func (p *ProcessState) Success() bool
```
Success reports whether the program exited successfully, such as with exit status 0 on Unix.
* **method Sys**
```golang
func (p *ProcessState) Sys() interface{}
```

Sys returns system­dependent exit information about the process. Convert it to the appropriate underlying type,
such as syscall.WaitStatus on Unix, to access its contents.

* **method SysUsage**
```golang
func (p *ProcessState) SysUsage() interface{}
```

SysUsage returns system­dependent resource usage information about the exited process. Convert it to the appropriate underlying type, such as *syscall.Rusage on Unix, to access its contents. (On Unix, *syscall.Rusage matches struct rusage as defined in the getrusage(2) manual page.)

* **method SystemTime**
```golang
func (p *ProcessState) SystemTime() time.Duration 
```
SystemTime returns the system CPU time of the exited process and its children.

* **method UserTime**
```golang
func (p *ProcessState) UserTime() time.Duration
```
UserTime returns the user CPU time of the exited process and its children.

### type Signal
A Signal represents an operating system signal. The usual underlying implementation is operating system­
dependent: on Unix it is syscall.Signal.
```golang
   type Signal interface {
           String() string
           Signal() // to distinguish from other Stringers
}
```
The only signal values guaranteed to be present on all systems are Interrupt (send the process an interrupt) and Kill (force the process to exit).
```golang
   var (
           Interrupt Signal = syscall.SIGINT
           Kill      Signal = syscall.SIGKILL
)
```
### type SyscallError
SyscallError records an error from a specific system call.
```golang
type SyscallError struct {
	Syscall string
	Err     error
}
```
* **method Error**
```golang
func (e *SyscallError) Error() string
```





