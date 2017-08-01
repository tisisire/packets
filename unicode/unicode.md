
 Package unicode

    import "unicode"

    Overview
    Index
    Examples
    Subdirectories

Overview ▾

Package unicode provides data and functions to test some properties of Unicode code points.

▹ Example (Is)
Index ▾

    Constants
    Variables
    func In(r rune, ranges ...*RangeTable) bool
    func Is(rangeTab *RangeTable, r rune) bool
    func IsControl(r rune) bool
    func IsDigit(r rune) bool
    func IsGraphic(r rune) bool
    func IsLetter(r rune) bool
    func IsLower(r rune) bool
    func IsMark(r rune) bool
    func IsNumber(r rune) bool
    func IsOneOf(ranges []*RangeTable, r rune) bool
    func IsPrint(r rune) bool
    func IsPunct(r rune) bool
    func IsSpace(r rune) bool
    func IsSymbol(r rune) bool
    func IsTitle(r rune) bool
    func IsUpper(r rune) bool
    func SimpleFold(r rune) rune
    func To(_case int, r rune) rune
    func ToLower(r rune) rune
    func ToTitle(r rune) rune
    func ToUpper(r rune) rune
    type CaseRange
    type Range16
    type Range32
    type RangeTable
    type SpecialCase
        func (special SpecialCase) ToLower(r rune) rune
        func (special SpecialCase) ToTitle(r rune) rune
        func (special SpecialCase) ToUpper(r rune) rune
    Bugs

Examples

    SimpleFold
    SpecialCase
    To
    ToLower
    ToTitle
    ToUpper
    Package (Is)

Package files

casetables.go digit.go graphic.go letter.go tables.go
Constants

const (
        MaxRune         = '\U0010FFFF' // Maximum valid Unicode code point.
        ReplacementChar = '\uFFFD'     // Represents invalid code points.
        MaxASCII        = '\u007F'     // maximum ASCII value.
        MaxLatin1       = '\u00FF'     // maximum Latin-1 value.
)

Indices into the Delta arrays inside CaseRanges for case mapping.

const (
        UpperCase = iota
        LowerCase
        TitleCase
        MaxCase
)

If the Delta field of a CaseRange is UpperLower, it means this CaseRange represents a sequence of the form (say) Upper Lower Upper Lower.

const (
        UpperLower = MaxRune + 1 // (Cannot be a valid delta.)
)

Version is the Unicode edition from which the tables are derived.

const Version = "9.0.0"

Variables

These variables have type *RangeTable.

var (
        Cc     = _Cc // Cc is the set of Unicode characters in category Cc.
        Cf     = _Cf // Cf is the set of Unicode characters in category Cf.
        Co     = _Co // Co is the set of Unicode characters in category Co.
        Cs     = _Cs // Cs is the set of Unicode characters in category Cs.
        Digit  = _Nd // Digit is the set of Unicode characters with the "decimal digit" property.
        Nd     = _Nd // Nd is the set of Unicode characters in category Nd.
        Letter = _L  // Letter/L is the set of Unicode letters, category L.
        L      = _L
        Lm     = _Lm // Lm is the set of Unicode characters in category Lm.
        Lo     = _Lo // Lo is the set of Unicode characters in category Lo.
        Lower  = _Ll // Lower is the set of Unicode lower case letters.
        Ll     = _Ll // Ll is the set of Unicode characters in category Ll.
        Mark   = _M  // Mark/M is the set of Unicode mark characters, category M.
        M      = _M
        Mc     = _Mc // Mc is the set of Unicode characters in category Mc.
        Me     = _Me // Me is the set of Unicode characters in category Me.
        Mn     = _Mn // Mn is the set of Unicode characters in category Mn.
        Nl     = _Nl // Nl is the set of Unicode characters in category Nl.
        No     = _No // No is the set of Unicode characters in category No.
        Number = _N  // Number/N is the set of Unicode number characters, category N.
        N      = _N
        Other  = _C // Other/C is the set of Unicode control and special characters, category C.
        C      = _C
        Pc     = _Pc // Pc is the set of Unicode characters in category Pc.
        Pd     = _Pd // Pd is the set of Unicode characters in category Pd.
        Pe     = _Pe // Pe is the set of Unicode characters in category Pe.
        Pf     = _Pf // Pf is the set of Unicode characters in category Pf.
        Pi     = _Pi // Pi is the set of Unicode characters in category Pi.
        Po     = _Po // Po is the set of Unicode characters in category Po.
        Ps     = _Ps // Ps is the set of Unicode characters in category Ps.
        Punct  = _P  // Punct/P is the set of Unicode punctuation characters, category P.
        P      = _P
        Sc     = _Sc // Sc is the set of Unicode characters in category Sc.
        Sk     = _Sk // Sk is the set of Unicode characters in category Sk.
        Sm     = _Sm // Sm is the set of Unicode characters in category Sm.
        So     = _So // So is the set of Unicode characters in category So.
        Space  = _Z  // Space/Z is the set of Unicode space characters, category Z.
        Z      = _Z
        Symbol = _S // Symbol/S is the set of Unicode symbol characters, category S.
        S      = _S
        Title  = _Lt // Title is the set of Unicode title case letters.
        Lt     = _Lt // Lt is the set of Unicode characters in category Lt.
        Upper  = _Lu // Upper is the set of Unicode upper case letters.
        Lu     = _Lu // Lu is the set of Unicode characters in category Lu.
        Zl     = _Zl // Zl is the set of Unicode characters in category Zl.
        Zp     = _Zp // Zp is the set of Unicode characters in category Zp.
        Zs     = _Zs // Zs is the set of Unicode characters in category Zs.
)

These variables have type *RangeTable.

var (
        Adlam                  = _Adlam                  // Adlam is the set of Unicode characters in script Adlam.
        Ahom                   = _Ahom                   // Ahom is the set of Unicode characters in script Ahom.
        Anatolian_Hieroglyphs  = _Anatolian_Hieroglyphs  // Anatolian_Hieroglyphs is the set of Unicode characters in script Anatolian_Hieroglyphs.
        Arabic                 = _Arabic                 // Arabic is the set of Unicode characters in script Arabic.
        Armenian               = _Armenian               // Armenian is the set of Unicode characters in script Armenian.
        Avestan                = _Avestan                // Avestan is the set of Unicode characters in script Avestan.
        Balinese               = _Balinese               // Balinese is the set of Unicode characters in script Balinese.
        Bamum                  = _Bamum                  // Bamum is the set of Unicode characters in script Bamum.
        Bassa_Vah              = _Bassa_Vah              // Bassa_Vah is the set of Unicode characters in script Bassa_Vah.
        Batak                  = _Batak                  // Batak is the set of Unicode characters in script Batak.
        Bengali                = _Bengali                // Bengali is the set of Unicode characters in script Bengali.
        Bhaiksuki              = _Bhaiksuki              // Bhaiksuki is the set of Unicode characters in script Bhaiksuki.
        Bopomofo               = _Bopomofo               // Bopomofo is the set of Unicode characters in script Bopomofo.
        Brahmi                 = _Brahmi                 // Brahmi is the set of Unicode characters in script Brahmi.
        Braille                = _Braille                // Braille is the set of Unicode characters in script Braille.
        Buginese               = _Buginese               // Buginese is the set of Unicode characters in script Buginese.
        Buhid                  = _Buhid                  // Buhid is the set of Unicode characters in script Buhid.
        Canadian_Aboriginal    = _Canadian_Aboriginal    // Canadian_Aboriginal is the set of Unicode characters in script Canadian_Aboriginal.
        Carian                 = _Carian                 // Carian is the set of Unicode characters in script Carian.
        Caucasian_Albanian     = _Caucasian_Albanian     // Caucasian_Albanian is the set of Unicode characters in script Caucasian_Albanian.
        Chakma                 = _Chakma                 // Chakma is the set of Unicode characters in script Chakma.
        Cham                   = _Cham                   // Cham is the set of Unicode characters in script Cham.
        Cherokee               = _Cherokee               // Cherokee is the set of Unicode characters in script Cherokee.
        Common                 = _Common                 // Common is the set of Unicode characters in script Common.
        Coptic                 = _Coptic                 // Coptic is the set of Unicode characters in script Coptic.
        Cuneiform              = _Cuneiform              // Cuneiform is the set of Unicode characters in script Cuneiform.
        Cypriot                = _Cypriot                // Cypriot is the set of Unicode characters in script Cypriot.
        Cyrillic               = _Cyrillic               // Cyrillic is the set of Unicode characters in script Cyrillic.
        Deseret                = _Deseret                // Deseret is the set of Unicode characters in script Deseret.
        Devanagari             = _Devanagari             // Devanagari is the set of Unicode characters in script Devanagari.
        Duployan               = _Duployan               // Duployan is the set of Unicode characters in script Duployan.
        Egyptian_Hieroglyphs   = _Egyptian_Hieroglyphs   // Egyptian_Hieroglyphs is the set of Unicode characters in script Egyptian_Hieroglyphs.
        Elbasan                = _Elbasan                // Elbasan is the set of Unicode characters in script Elbasan.
        Ethiopic               = _Ethiopic               // Ethiopic is the set of Unicode characters in script Ethiopic.
        Georgian               = _Georgian               // Georgian is the set of Unicode characters in script Georgian.
        Glagolitic             = _Glagolitic             // Glagolitic is the set of Unicode characters in script Glagolitic.
        Gothic                 = _Gothic                 // Gothic is the set of Unicode characters in script Gothic.
        Grantha                = _Grantha                // Grantha is the set of Unicode characters in script Grantha.
        Greek                  = _Greek                  // Greek is the set of Unicode characters in script Greek.
        Gujarati               = _Gujarati               // Gujarati is the set of Unicode characters in script Gujarati.
        Gurmukhi               = _Gurmukhi               // Gurmukhi is the set of Unicode characters in script Gurmukhi.
        Han                    = _Han                    // Han is the set of Unicode characters in script Han.
        Hangul                 = _Hangul                 // Hangul is the set of Unicode characters in script Hangul.
        Hanunoo                = _Hanunoo                // Hanunoo is the set of Unicode characters in script Hanunoo.
        Hatran                 = _Hatran                 // Hatran is the set of Unicode characters in script Hatran.
        Hebrew                 = _Hebrew                 // Hebrew is the set of Unicode characters in script Hebrew.
        Hiragana               = _Hiragana               // Hiragana is the set of Unicode characters in script Hiragana.
        Imperial_Aramaic       = _Imperial_Aramaic       // Imperial_Aramaic is the set of Unicode characters in script Imperial_Aramaic.
        Inherited              = _Inherited              // Inherited is the set of Unicode characters in script Inherited.
        Inscriptional_Pahlavi  = _Inscriptional_Pahlavi  // Inscriptional_Pahlavi is the set of Unicode characters in script Inscriptional_Pahlavi.
        Inscriptional_Parthian = _Inscriptional_Parthian // Inscriptional_Parthian is the set of Unicode characters in script Inscriptional_Parthian.
        Javanese               = _Javanese               // Javanese is the set of Unicode characters in script Javanese.
        Kaithi                 = _Kaithi                 // Kaithi is the set of Unicode characters in script Kaithi.
        Kannada                = _Kannada                // Kannada is the set of Unicode characters in script Kannada.
        Katakana               = _Katakana               // Katakana is the set of Unicode characters in script Katakana.
        Kayah_Li               = _Kayah_Li               // Kayah_Li is the set of Unicode characters in script Kayah_Li.
        Kharoshthi             = _Kharoshthi             // Kharoshthi is the set of Unicode characters in script Kharoshthi.
        Khmer                  = _Khmer                  // Khmer is the set of Unicode characters in script Khmer.
        Khojki                 = _Khojki                 // Khojki is the set of Unicode characters in script Khojki.
        Khudawadi              = _Khudawadi              // Khudawadi is the set of Unicode characters in script Khudawadi.
        Lao                    = _Lao                    // Lao is the set of Unicode characters in script Lao.
        Latin                  = _Latin                  // Latin is the set of Unicode characters in script Latin.
        Lepcha                 = _Lepcha                 // Lepcha is the set of Unicode characters in script Lepcha.
        Limbu                  = _Limbu                  // Limbu is the set of Unicode characters in script Limbu.
        Linear_A               = _Linear_A               // Linear_A is the set of Unicode characters in script Linear_A.
        Linear_B               = _Linear_B               // Linear_B is the set of Unicode characters in script Linear_B.
        Lisu                   = _Lisu                   // Lisu is the set of Unicode characters in script Lisu.
        Lycian                 = _Lycian                 // Lycian is the set of Unicode characters in script Lycian.
        Lydian                 = _Lydian                 // Lydian is the set of Unicode characters in script Lydian.
        Mahajani               = _Mahajani               // Mahajani is the set of Unicode characters in script Mahajani.
        Malayalam              = _Malayalam              // Malayalam is the set of Unicode characters in script Malayalam.
        Mandaic                = _Mandaic                // Mandaic is the set of Unicode characters in script Mandaic.
        Manichaean             = _Manichaean             // Manichaean is the set of Unicode characters in script Manichaean.
        Marchen                = _Marchen                // Marchen is the set of Unicode characters in script Marchen.
        Meetei_Mayek           = _Meetei_Mayek           // Meetei_Mayek is the set of Unicode characters in script Meetei_Mayek.
        Mende_Kikakui          = _Mende_Kikakui          // Mende_Kikakui is the set of Unicode characters in script Mende_Kikakui.
        Meroitic_Cursive       = _Meroitic_Cursive       // Meroitic_Cursive is the set of Unicode characters in script Meroitic_Cursive.
        Meroitic_Hieroglyphs   = _Meroitic_Hieroglyphs   // Meroitic_Hieroglyphs is the set of Unicode characters in script Meroitic_Hieroglyphs.
        Miao                   = _Miao                   // Miao is the set of Unicode characters in script Miao.
        Modi                   = _Modi                   // Modi is the set of Unicode characters in script Modi.
        Mongolian              = _Mongolian              // Mongolian is the set of Unicode characters in script Mongolian.
        Mro                    = _Mro                    // Mro is the set of Unicode characters in script Mro.
        Multani                = _Multani                // Multani is the set of Unicode characters in script Multani.
        Myanmar                = _Myanmar                // Myanmar is the set of Unicode characters in script Myanmar.
        Nabataean              = _Nabataean              // Nabataean is the set of Unicode characters in script Nabataean.
        New_Tai_Lue            = _New_Tai_Lue            // New_Tai_Lue is the set of Unicode characters in script New_Tai_Lue.
        Newa                   = _Newa                   // Newa is the set of Unicode characters in script Newa.
        Nko                    = _Nko                    // Nko is the set of Unicode characters in script Nko.
        Ogham                  = _Ogham                  // Ogham is the set of Unicode characters in script Ogham.
        Ol_Chiki               = _Ol_Chiki               // Ol_Chiki is the set of Unicode characters in script Ol_Chiki.
        Old_Hungarian          = _Old_Hungarian          // Old_Hungarian is the set of Unicode characters in script Old_Hungarian.
        Old_Italic             = _Old_Italic             // Old_Italic is the set of Unicode characters in script Old_Italic.
        Old_North_Arabian      = _Old_North_Arabian      // Old_North_Arabian is the set of Unicode characters in script Old_North_Arabian.
        Old_Permic             = _Old_Permic             // Old_Permic is the set of Unicode characters in script Old_Permic.
        Old_Persian            = _Old_Persian            // Old_Persian is the set of Unicode characters in script Old_Persian.
        Old_South_Arabian      = _Old_South_Arabian      // Old_South_Arabian is the set of Unicode characters in script Old_South_Arabian.
        Old_Turkic             = _Old_Turkic             // Old_Turkic is the set of Unicode characters in script Old_Turkic.
        Oriya                  = _Oriya                  // Oriya is the set of Unicode characters in script Oriya.
        Osage                  = _Osage                  // Osage is the set of Unicode characters in script Osage.
        Osmanya                = _Osmanya                // Osmanya is the set of Unicode characters in script Osmanya.
        Pahawh_Hmong           = _Pahawh_Hmong           // Pahawh_Hmong is the set of Unicode characters in script Pahawh_Hmong.
        Palmyrene              = _Palmyrene              // Palmyrene is the set of Unicode characters in script Palmyrene.
        Pau_Cin_Hau            = _Pau_Cin_Hau            // Pau_Cin_Hau is the set of Unicode characters in script Pau_Cin_Hau.
        Phags_Pa               = _Phags_Pa               // Phags_Pa is the set of Unicode characters in script Phags_Pa.
        Phoenician             = _Phoenician             // Phoenician is the set of Unicode characters in script Phoenician.
        Psalter_Pahlavi        = _Psalter_Pahlavi        // Psalter_Pahlavi is the set of Unicode characters in script Psalter_Pahlavi.
        Rejang                 = _Rejang                 // Rejang is the set of Unicode characters in script Rejang.
        Runic                  = _Runic                  // Runic is the set of Unicode characters in script Runic.
        Samaritan              = _Samaritan              // Samaritan is the set of Unicode characters in script Samaritan.
        Saurashtra             = _Saurashtra             // Saurashtra is the set of Unicode characters in script Saurashtra.
        Sharada                = _Sharada                // Sharada is the set of Unicode characters in script Sharada.
        Shavian                = _Shavian                // Shavian is the set of Unicode characters in script Shavian.
        Siddham                = _Siddham                // Siddham is the set of Unicode characters in script Siddham.
        SignWriting            = _SignWriting            // SignWriting is the set of Unicode characters in script SignWriting.
        Sinhala                = _Sinhala                // Sinhala is the set of Unicode characters in script Sinhala.
        Sora_Sompeng           = _Sora_Sompeng           // Sora_Sompeng is the set of Unicode characters in script Sora_Sompeng.
        Sundanese              = _Sundanese              // Sundanese is the set of Unicode characters in script Sundanese.
        Syloti_Nagri           = _Syloti_Nagri           // Syloti_Nagri is the set of Unicode characters in script Syloti_Nagri.
        Syriac                 = _Syriac                 // Syriac is the set of Unicode characters in script Syriac.
        Tagalog                = _Tagalog                // Tagalog is the set of Unicode characters in script Tagalog.
        Tagbanwa               = _Tagbanwa               // Tagbanwa is the set of Unicode characters in script Tagbanwa.
        Tai_Le                 = _Tai_Le                 // Tai_Le is the set of Unicode characters in script Tai_Le.
        Tai_Tham               = _Tai_Tham               // Tai_Tham is the set of Unicode characters in script Tai_Tham.
        Tai_Viet               = _Tai_Viet               // Tai_Viet is the set of Unicode characters in script Tai_Viet.
        Takri                  = _Takri                  // Takri is the set of Unicode characters in script Takri.
        Tamil                  = _Tamil                  // Tamil is the set of Unicode characters in script Tamil.
        Tangut                 = _Tangut                 // Tangut is the set of Unicode characters in script Tangut.
        Telugu                 = _Telugu                 // Telugu is the set of Unicode characters in script Telugu.
        Thaana                 = _Thaana                 // Thaana is the set of Unicode characters in script Thaana.
        Thai                   = _Thai                   // Thai is the set of Unicode characters in script Thai.
        Tibetan                = _Tibetan                // Tibetan is the set of Unicode characters in script Tibetan.
        Tifinagh               = _Tifinagh               // Tifinagh is the set of Unicode characters in script Tifinagh.
        Tirhuta                = _Tirhuta                // Tirhuta is the set of Unicode characters in script Tirhuta.
        Ugaritic               = _Ugaritic               // Ugaritic is the set of Unicode characters in script Ugaritic.
        Vai                    = _Vai                    // Vai is the set of Unicode characters in script Vai.
        Warang_Citi            = _Warang_Citi            // Warang_Citi is the set of Unicode characters in script Warang_Citi.
        Yi                     = _Yi                     // Yi is the set of Unicode characters in script Yi.
)

These variables have type *RangeTable.

var (
        ASCII_Hex_Digit                    = _ASCII_Hex_Digit                    // ASCII_Hex_Digit is the set of Unicode characters with property ASCII_Hex_Digit.
        Bidi_Control                       = _Bidi_Control                       // Bidi_Control is the set of Unicode characters with property Bidi_Control.
        Dash                               = _Dash                               // Dash is the set of Unicode characters with property Dash.
        Deprecated                         = _Deprecated                         // Deprecated is the set of Unicode characters with property Deprecated.
        Diacritic                          = _Diacritic                          // Diacritic is the set of Unicode characters with property Diacritic.
        Extender                           = _Extender                           // Extender is the set of Unicode characters with property Extender.
        Hex_Digit                          = _Hex_Digit                          // Hex_Digit is the set of Unicode characters with property Hex_Digit.
        Hyphen                             = _Hyphen                             // Hyphen is the set of Unicode characters with property Hyphen.
        IDS_Binary_Operator                = _IDS_Binary_Operator                // IDS_Binary_Operator is the set of Unicode characters with property IDS_Binary_Operator.
        IDS_Trinary_Operator               = _IDS_Trinary_Operator               // IDS_Trinary_Operator is the set of Unicode characters with property IDS_Trinary_Operator.
        Ideographic                        = _Ideographic                        // Ideographic is the set of Unicode characters with property Ideographic.
        Join_Control                       = _Join_Control                       // Join_Control is the set of Unicode characters with property Join_Control.
        Logical_Order_Exception            = _Logical_Order_Exception            // Logical_Order_Exception is the set of Unicode characters with property Logical_Order_Exception.
        Noncharacter_Code_Point            = _Noncharacter_Code_Point            // Noncharacter_Code_Point is the set of Unicode characters with property Noncharacter_Code_Point.
        Other_Alphabetic                   = _Other_Alphabetic                   // Other_Alphabetic is the set of Unicode characters with property Other_Alphabetic.
        Other_Default_Ignorable_Code_Point = _Other_Default_Ignorable_Code_Point // Other_Default_Ignorable_Code_Point is the set of Unicode characters with property Other_Default_Ignorable_Code_Point.
        Other_Grapheme_Extend              = _Other_Grapheme_Extend              // Other_Grapheme_Extend is the set of Unicode characters with property Other_Grapheme_Extend.
        Other_ID_Continue                  = _Other_ID_Continue                  // Other_ID_Continue is the set of Unicode characters with property Other_ID_Continue.
        Other_ID_Start                     = _Other_ID_Start                     // Other_ID_Start is the set of Unicode characters with property Other_ID_Start.
        Other_Lowercase                    = _Other_Lowercase                    // Other_Lowercase is the set of Unicode characters with property Other_Lowercase.
        Other_Math                         = _Other_Math                         // Other_Math is the set of Unicode characters with property Other_Math.
        Other_Uppercase                    = _Other_Uppercase                    // Other_Uppercase is the set of Unicode characters with property Other_Uppercase.
        Pattern_Syntax                     = _Pattern_Syntax                     // Pattern_Syntax is the set of Unicode characters with property Pattern_Syntax.
        Pattern_White_Space                = _Pattern_White_Space                // Pattern_White_Space is the set of Unicode characters with property Pattern_White_Space.
        Prepended_Concatenation_Mark       = _Prepended_Concatenation_Mark       // Prepended_Concatenation_Mark is the set of Unicode characters with property Prepended_Concatenation_Mark.
        Quotation_Mark                     = _Quotation_Mark                     // Quotation_Mark is the set of Unicode characters with property Quotation_Mark.
        Radical                            = _Radical                            // Radical is the set of Unicode characters with property Radical.
        STerm                              = _Sentence_Terminal                  // STerm is an alias for Sentence_Terminal.
        Sentence_Terminal                  = _Sentence_Terminal                  // Sentence_Terminal is the set of Unicode characters with property Sentence_Terminal.
        Soft_Dotted                        = _Soft_Dotted                        // Soft_Dotted is the set of Unicode characters with property Soft_Dotted.
        Terminal_Punctuation               = _Terminal_Punctuation               // Terminal_Punctuation is the set of Unicode characters with property Terminal_Punctuation.
        Unified_Ideograph                  = _Unified_Ideograph                  // Unified_Ideograph is the set of Unicode characters with property Unified_Ideograph.
        Variation_Selector                 = _Variation_Selector                 // Variation_Selector is the set of Unicode characters with property Variation_Selector.
        White_Space                        = _White_Space                        // White_Space is the set of Unicode characters with property White_Space.
)

CaseRanges is the table describing case mappings for all letters with non-self mappings.

var CaseRanges = _CaseRanges

Categories is the set of Unicode category tables.

var Categories = map[string]*RangeTable{
        "C":  C,
        "Cc": Cc,
        "Cf": Cf,
        "Co": Co,
        "Cs": Cs,
        "L":  L,
        "Ll": Ll,
        "Lm": Lm,
        "Lo": Lo,
        "Lt": Lt,
        "Lu": Lu,
        "M":  M,
        "Mc": Mc,
        "Me": Me,
        "Mn": Mn,
        "N":  N,
        "Nd": Nd,
        "Nl": Nl,
        "No": No,
        "P":  P,
        "Pc": Pc,
        "Pd": Pd,
        "Pe": Pe,
        "Pf": Pf,
        "Pi": Pi,
        "Po": Po,
        "Ps": Ps,
        "S":  S,
        "Sc": Sc,
        "Sk": Sk,
        "Sm": Sm,
        "So": So,
        "Z":  Z,
        "Zl": Zl,
        "Zp": Zp,
        "Zs": Zs,
}

FoldCategory maps a category name to a table of code points outside the category that are equivalent under simple case folding to code points inside the category. If there is no entry for a category name, there are no such points.

var FoldCategory = map[string]*RangeTable{
        "Common":    foldCommon,
        "Greek":     foldGreek,
        "Inherited": foldInherited,
        "L":         foldL,
        "Ll":        foldLl,
        "Lt":        foldLt,
        "Lu":        foldLu,
        "M":         foldM,
        "Mn":        foldMn,
}

FoldScript maps a script name to a table of code points outside the script that are equivalent under simple case folding to code points inside the script. If there is no entry for a script name, there are no such points.

var FoldScript = map[string]*RangeTable{}

GraphicRanges defines the set of graphic characters according to Unicode.

var GraphicRanges = []*RangeTable{
        L, M, N, P, S, Zs,
}

PrintRanges defines the set of printable characters according to Go. ASCII space, U+0020, is handled separately.

var PrintRanges = []*RangeTable{
        L, M, N, P, S,
}

Properties is the set of Unicode property tables.

var Properties = map[string]*RangeTable{
        "ASCII_Hex_Digit":                    ASCII_Hex_Digit,
        "Bidi_Control":                       Bidi_Control,
        "Dash":                               Dash,
        "Deprecated":                         Deprecated,
        "Diacritic":                          Diacritic,
        "Extender":                           Extender,
        "Hex_Digit":                          Hex_Digit,
        "Hyphen":                             Hyphen,
        "IDS_Binary_Operator":                IDS_Binary_Operator,
        "IDS_Trinary_Operator":               IDS_Trinary_Operator,
        "Ideographic":                        Ideographic,
        "Join_Control":                       Join_Control,
        "Logical_Order_Exception":            Logical_Order_Exception,
        "Noncharacter_Code_Point":            Noncharacter_Code_Point,
        "Other_Alphabetic":                   Other_Alphabetic,
        "Other_Default_Ignorable_Code_Point": Other_Default_Ignorable_Code_Point,
        "Other_Grapheme_Extend":              Other_Grapheme_Extend,
        "Other_ID_Continue":                  Other_ID_Continue,
        "Other_ID_Start":                     Other_ID_Start,
        "Other_Lowercase":                    Other_Lowercase,
        "Other_Math":                         Other_Math,
        "Other_Uppercase":                    Other_Uppercase,
        "Pattern_Syntax":                     Pattern_Syntax,
        "Pattern_White_Space":                Pattern_White_Space,
        "Prepended_Concatenation_Mark":       Prepended_Concatenation_Mark,
        "Quotation_Mark":                     Quotation_Mark,
        "Radical":                            Radical,
        "Sentence_Terminal":                  Sentence_Terminal,
        "STerm":                              Sentence_Terminal,
        "Soft_Dotted":                        Soft_Dotted,
        "Terminal_Punctuation":               Terminal_Punctuation,
        "Unified_Ideograph":                  Unified_Ideograph,
        "Variation_Selector":                 Variation_Selector,
        "White_Space":                        White_Space,
}

Scripts is the set of Unicode script tables.

var Scripts = map[string]*RangeTable{
        "Adlam":                  Adlam,
        "Ahom":                   Ahom,
        "Anatolian_Hieroglyphs":  Anatolian_Hieroglyphs,
        "Arabic":                 Arabic,
        "Armenian":               Armenian,
        "Avestan":                Avestan,
        "Balinese":               Balinese,
        "Bamum":                  Bamum,
        "Bassa_Vah":              Bassa_Vah,
        "Batak":                  Batak,
        "Bengali":                Bengali,
        "Bhaiksuki":              Bhaiksuki,
        "Bopomofo":               Bopomofo,
        "Brahmi":                 Brahmi,
        "Braille":                Braille,
        "Buginese":               Buginese,
        "Buhid":                  Buhid,
        "Canadian_Aboriginal":    Canadian_Aboriginal,
        "Carian":                 Carian,
        "Caucasian_Albanian":     Caucasian_Albanian,
        "Chakma":                 Chakma,
        "Cham":                   Cham,
        "Cherokee":               Cherokee,
        "Common":                 Common,
        "Coptic":                 Coptic,
        "Cuneiform":              Cuneiform,
        "Cypriot":                Cypriot,
        "Cyrillic":               Cyrillic,
        "Deseret":                Deseret,
        "Devanagari":             Devanagari,
        "Duployan":               Duployan,
        "Egyptian_Hieroglyphs":   Egyptian_Hieroglyphs,
        "Elbasan":                Elbasan,
        "Ethiopic":               Ethiopic,
        "Georgian":               Georgian,
        "Glagolitic":             Glagolitic,
        "Gothic":                 Gothic,
        "Grantha":                Grantha,
        "Greek":                  Greek,
        "Gujarati":               Gujarati,
        "Gurmukhi":               Gurmukhi,
        "Han":                    Han,
        "Hangul":                 Hangul,
        "Hanunoo":                Hanunoo,
        "Hatran":                 Hatran,
        "Hebrew":                 Hebrew,
        "Hiragana":               Hiragana,
        "Imperial_Aramaic":       Imperial_Aramaic,
        "Inherited":              Inherited,
        "Inscriptional_Pahlavi":  Inscriptional_Pahlavi,
        "Inscriptional_Parthian": Inscriptional_Parthian,
        "Javanese":               Javanese,
        "Kaithi":                 Kaithi,
        "Kannada":                Kannada,
        "Katakana":               Katakana,
        "Kayah_Li":               Kayah_Li,
        "Kharoshthi":             Kharoshthi,
        "Khmer":                  Khmer,
        "Khojki":                 Khojki,
        "Khudawadi":              Khudawadi,
        "Lao":                    Lao,
        "Latin":                  Latin,
        "Lepcha":                 Lepcha,
        "Limbu":                  Limbu,
        "Linear_A":               Linear_A,
        "Linear_B":               Linear_B,
        "Lisu":                   Lisu,
        "Lycian":                 Lycian,
        "Lydian":                 Lydian,
        "Mahajani":               Mahajani,
        "Malayalam":              Malayalam,
        "Mandaic":                Mandaic,
        "Manichaean":             Manichaean,
        "Marchen":                Marchen,
        "Meetei_Mayek":           Meetei_Mayek,
        "Mende_Kikakui":          Mende_Kikakui,
        "Meroitic_Cursive":       Meroitic_Cursive,
        "Meroitic_Hieroglyphs":   Meroitic_Hieroglyphs,
        "Miao":                   Miao,
        "Modi":                   Modi,
        "Mongolian":              Mongolian,
        "Mro":                    Mro,
        "Multani":                Multani,
        "Myanmar":                Myanmar,
        "Nabataean":              Nabataean,
        "New_Tai_Lue":            New_Tai_Lue,
        "Newa":                   Newa,
        "Nko":                    Nko,
        "Ogham":                  Ogham,
        "Ol_Chiki":               Ol_Chiki,
        "Old_Hungarian":          Old_Hungarian,
        "Old_Italic":             Old_Italic,
        "Old_North_Arabian":      Old_North_Arabian,
        "Old_Permic":             Old_Permic,
        "Old_Persian":            Old_Persian,
        "Old_South_Arabian":      Old_South_Arabian,
        "Old_Turkic":             Old_Turkic,
        "Oriya":                  Oriya,
        "Osage":                  Osage,
        "Osmanya":                Osmanya,
        "Pahawh_Hmong":           Pahawh_Hmong,
        "Palmyrene":              Palmyrene,
        "Pau_Cin_Hau":            Pau_Cin_Hau,
        "Phags_Pa":               Phags_Pa,
        "Phoenician":             Phoenician,
        "Psalter_Pahlavi":        Psalter_Pahlavi,
        "Rejang":                 Rejang,
        "Runic":                  Runic,
        "Samaritan":              Samaritan,
        "Saurashtra":             Saurashtra,
        "Sharada":                Sharada,
        "Shavian":                Shavian,
        "Siddham":                Siddham,
        "SignWriting":            SignWriting,
        "Sinhala":                Sinhala,
        "Sora_Sompeng":           Sora_Sompeng,
        "Sundanese":              Sundanese,
        "Syloti_Nagri":           Syloti_Nagri,
        "Syriac":                 Syriac,
        "Tagalog":                Tagalog,
        "Tagbanwa":               Tagbanwa,
        "Tai_Le":                 Tai_Le,
        "Tai_Tham":               Tai_Tham,
        "Tai_Viet":               Tai_Viet,
        "Takri":                  Takri,
        "Tamil":                  Tamil,
        "Tangut":                 Tangut,
        "Telugu":                 Telugu,
        "Thaana":                 Thaana,
        "Thai":                   Thai,
        "Tibetan":                Tibetan,
        "Tifinagh":               Tifinagh,
        "Tirhuta":                Tirhuta,
        "Ugaritic":               Ugaritic,
        "Vai":                    Vai,
        "Warang_Citi":            Warang_Citi,
        "Yi":                     Yi,
}

func In

func In(r rune, ranges ...*RangeTable) bool

In reports whether the rune is a member of one of the ranges.
func Is

func Is(rangeTab *RangeTable, r rune) bool

Is reports whether the rune is in the specified table of ranges.
func IsControl

func IsControl(r rune) bool

IsControl reports whether the rune is a control character. The C (Other) Unicode category includes more code points such as surrogates; use Is(C, r) to test for them.
func IsDigit

func IsDigit(r rune) bool

IsDigit reports whether the rune is a decimal digit.
func IsGraphic

func IsGraphic(r rune) bool

IsGraphic reports whether the rune is defined as a Graphic by Unicode. Such characters include letters, marks, numbers, punctuation, symbols, and spaces, from categories L, M, N, P, S, Zs.
func IsLetter

func IsLetter(r rune) bool

IsLetter reports whether the rune is a letter (category L).
func IsLower

func IsLower(r rune) bool

IsLower reports whether the rune is a lower case letter.
func IsMark

func IsMark(r rune) bool

IsMark reports whether the rune is a mark character (category M).
func IsNumber

func IsNumber(r rune) bool

IsNumber reports whether the rune is a number (category N).
func IsOneOf

func IsOneOf(ranges []*RangeTable, r rune) bool

IsOneOf reports whether the rune is a member of one of the ranges. The function "In" provides a nicer signature and should be used in preference to IsOneOf.
func IsPrint

func IsPrint(r rune) bool

IsPrint reports whether the rune is defined as printable by Go. Such characters include letters, marks, numbers, punctuation, symbols, and the ASCII space character, from categories L, M, N, P, S and the ASCII space character. This categorization is the same as IsGraphic except that the only spacing character is ASCII space, U+0020.
func IsPunct

func IsPunct(r rune) bool

IsPunct reports whether the rune is a Unicode punctuation character (category P).
func IsSpace

func IsSpace(r rune) bool

IsSpace reports whether the rune is a space character as defined by Unicode's White Space property; in the Latin-1 space this is

'\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL), U+00A0 (NBSP).

Other definitions of spacing characters are set by category Z and property Pattern_White_Space.
func IsSymbol

func IsSymbol(r rune) bool

IsSymbol reports whether the rune is a symbolic character.
func IsTitle

func IsTitle(r rune) bool

IsTitle reports whether the rune is a title case letter.
func IsUpper

func IsUpper(r rune) bool

IsUpper reports whether the rune is an upper case letter.
func SimpleFold

func SimpleFold(r rune) rune

SimpleFold iterates over Unicode code points equivalent under the Unicode-defined simple case folding. Among the code points equivalent to rune (including rune itself), SimpleFold returns the smallest rune > r if one exists, or else the smallest rune >= 0. If r is not a valid Unicode code point, SimpleFold(r) returns r.

For example:

SimpleFold('A') = 'a'
SimpleFold('a') = 'A'

SimpleFold('K') = 'k'
SimpleFold('k') = '\u212A' (Kelvin symbol, K)
SimpleFold('\u212A') = 'K'

SimpleFold('1') = '1'

SimpleFold(-2) = -2

▹ Example
func To

func To(_case int, r rune) rune

To maps the rune to the specified case: UpperCase, LowerCase, or TitleCase.

▹ Example
func ToLower

func ToLower(r rune) rune

ToLower maps the rune to lower case.

▹ Example
func ToTitle

func ToTitle(r rune) rune

ToTitle maps the rune to title case.

▹ Example
func ToUpper

func ToUpper(r rune) rune

ToUpper maps the rune to upper case.

▹ Example
type CaseRange

CaseRange represents a range of Unicode code points for simple (one code point to one code point) case conversion. The range runs from Lo to Hi inclusive, with a fixed stride of 1. Deltas are the number to add to the code point to reach the code point for a different case for that character. They may be negative. If zero, it means the character is in the corresponding case. There is a special case representing sequences of alternating corresponding Upper and Lower pairs. It appears with a fixed Delta of

{UpperLower, UpperLower, UpperLower}

The constant UpperLower has an otherwise impossible delta value.

type CaseRange struct {
        Lo    uint32
        Hi    uint32
        Delta d
}

type Range16

Range16 represents of a range of 16-bit Unicode code points. The range runs from Lo to Hi inclusive and has the specified stride.

type Range16 struct {
        Lo     uint16
        Hi     uint16
        Stride uint16
}

type Range32

Range32 represents of a range of Unicode code points and is used when one or more of the values will not fit in 16 bits. The range runs from Lo to Hi inclusive and has the specified stride. Lo and Hi must always be >= 1<<16.

type Range32 struct {
        Lo     uint32
        Hi     uint32
        Stride uint32
}

type RangeTable

RangeTable defines a set of Unicode code points by listing the ranges of code points within the set. The ranges are listed in two slices to save space: a slice of 16-bit ranges and a slice of 32-bit ranges. The two slices must be in sorted order and non-overlapping. Also, R32 should contain only values >= 0x10000 (1<<16).

type RangeTable struct {
        R16         []Range16
        R32         []Range32
        LatinOffset int // number of entries in R16 with Hi <= MaxLatin1
}

type SpecialCase

SpecialCase represents language-specific case mappings such as Turkish. Methods of SpecialCase customize (by overriding) the standard mappings.

type SpecialCase []CaseRange

var AzeriCase SpecialCase = _TurkishCase

var TurkishCase SpecialCase = _TurkishCase

▹ Example
func (SpecialCase) ToLower

func (special SpecialCase) ToLower(r rune) rune

ToLower maps the rune to lower case giving priority to the special mapping.
func (SpecialCase) ToTitle

func (special SpecialCase) ToTitle(r rune) rune

ToTitle maps the rune to title case giving priority to the special mapping.
func (SpecialCase) ToUpper

func (special SpecialCase) ToUpper(r rune) rune

ToUpper maps the rune to upper case giving priority to the special mapping.
Bugs

    ☞

    There is no mechanism for full case folding, that is, for characters that involve multiple runes in the input or output.
