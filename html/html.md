 # Το πακέτο html

```golang
 import "html"
```
Περιεχόμενα:
* [Γενικά](#info
* [Συναρτήσεις - Μέθοδοι](#funcs)

### <a name="info"></a>Γενικά 
Package html provides functions for escaping and unescaping HTML text.

### <a name="funcs"></a>Συναρτήσεις  

* **func EscapeString**

```golang
func EscapeString(s string) string
```
EscapeString escapes special characters like "<" to become "&lt;". It escapes only five such characters: <, >, &, ' and ". UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.

▹ Example
* **func UnescapeString**

```golang
func UnescapeString(s string) string
```
UnescapeString unescapes entities like "&lt;" to become "<". It unescapes a larger range of entities than EscapeString escapes. For example, "&aacute;" unescapes to "á", as does "&#225;" and "&#xE1;". UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.

▹ Example
