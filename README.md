# WUSS

Word-focused Unsatisfactory Style Sheets, a possibly simpler way of specifying the somewhat wordy `w:style` and similar structures used by WordprocessingML, the MS Word flavor of Open Office XML.

*function* **COMPILE-STYLE** `style-form`

Returns a string representing the *xml*-form of the *Style* or *Numbering* instance `style-form`, interpreted as follows:

* A **keyword**, representing an element tag

* Zero or more **attribute** *name*-*value* pairs, representing the attributes of the element (but see below)

* Any children, in a parenthesised list.

Symbols are converted from kebab-case to camel-case, strings are left as-is, and other items are `PRINC-TO-STRING`ed. Items in an element or attribute-name position have the "w" namespace prepended. If the length of the attribute list is odd, "w:val" is prepended as a further shorthand. 

Examples might make this less obscure:

```lisp
(:style type "character" custom-style 1 style-id "mdcode"
 (:name "MD Code"
  :ui-priority 1
  :q-format
  :r-pr
  (:no-proof
   :r-fonts ascii "Consolas"
   :sz 20
   :shd 1 color "auto" fill "DCDCDC")))
```

yields:

```xml
<w:style w:type="character" w:customStyle="1" w:styleId="mdcode" >
	<w:name w:val="MD Code" />
	<w:uiPriority w:val="1" />
	<w:qFormat />
	<w:rPr >
		<w:noProof />
		<w:rFonts w:ascii="Consolas" />
		<w:sz w:val="20" />
		<w:shd w:val="1" w:color="auto" w:fill="DCDCDC" />
	</w:rPr>
</w:style>
```

*function* **DECOMPILE-STYLE** `element`

Returns a list of items in the style accepted by **COMPILE-STYLE** generated from `element`, a **PLUMP-DOM:ELEMENT**. The inverse of **COMPILE-STYLE**. The `<w:style>` element generated from the above xml, when fed to **DECOMPILE-STYLE** yields:

```lisp
(:STYLE TYPE "character" CUSTOM-STYLE 1 STYLE-ID "mdcode"
 (:NAME "MD Code" :UI-PRIORITY 1 :Q-FORMAT :R-PR
  (:NO-PROOF :R-FONTS ASCII "Consolas" :SZ 20 :SHD 1 COLOR "auto" FILL
   "DCDCDC")))
```

