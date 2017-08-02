
 Package html

    import "html"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package html provides functions for escaping and unescaping HTML text.
Index ▾

    func EscapeString(s string) string
    func UnescapeString(s string) string

Examples

    EscapeString
    UnescapeString

Package files

entity.go escape.go
func EscapeString

func EscapeString(s string) string

EscapeString escapes special characters like "<" to become "&lt;". It escapes only five such characters: <, >, &, ' and ". UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.

▹ Example
func UnescapeString

func UnescapeString(s string) string

UnescapeString unescapes entities like "&lt;" to become "<". It unescapes a larger range of entities than EscapeString escapes. For example, "&aacute;" unescapes to "á", as does "&#225;" and "&#xE1;". UnescapeString(EscapeString(s)) == s always holds, but the converse isn't always true.

▹ Example
