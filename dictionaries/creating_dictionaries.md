# Creating Dictionaries

There are several ways of creating ContentMine dictionaries. We recommend that wherever possible they are linked to 
Wikipedia and Wikidata. This gives them semantic structure and also adds an authority.

## prerequisites and help

You must have installed AMI and be able to run:
```
ami-dictionary
```
This should output the help. It's long, and we have snipped most of the 25+ options
```
Usage: ami-dictionary [OPTIONS] [<operation>[,<operation>...]...]
Description
===========
Manages AMI dictionaries
Parameters
=========
      [<operation>[,<operation>...]...]
                            primary operation: (create, display, help, translate);
                              if no operation, runs help
                              Default: help
Options
=======
      --basename=<userBasename>
                            User's basename for outputfiles (e.g. foo/bar/<basename>.
                              png. By default this is computed by AMI. This allows
                              users to create their own variants, but they won't be
                              known by default to subsequentapplications
  [...]
      --directory=<dictionaryTopname>
                            top directory containing dictionary/s. Subdirectories
                              will use structured names (NYI). Thus dictionary
                              'animals' is found in '<directory>/animals.xml', while
                              'plants.parts' is found in <directory>/plants/parts.
                              xml. Required for relative dictionary names.
  [...]
                              
      --hreftext            hyperlinks from text (maybe excludes tables); requires
                              wikipedia or wikitable input at present; still under
                              test
  [...]
                              
      --informat=input format
                            input format (csv, wikicategory, wikipage, wikitable)
  [...]
      --outformats=output format[,output format...]...
                            output format (xml, html, json)
                              Default: [xml]
  [...]
                              
      --terms=<terms>[,<terms>...]...
                            list of terms (entries), comma-separated
  [...]
                            
      --wikilinks[=<wikiLinks>[,<wikiLinks>...]...]
                            try to add link to Wikidata and/or Wikipedia page of
                              same name.
                              Default: wikipedia,wikidata
  [...]

-d, --dictionary=<dictionary>...
                            input or output dictionary name/s. for 'create' must be
                              singular; when 'display' or 'translate', any number.
                              Names should be lowercase, unique. [a-z][a-z0-9._].
                              Dots can be used to structure dictionaries
                              intodirectories. Dictionary names are relative to
                              'directory'. If <directory> is absent then dictionary
                              names are absolute.
  -h, --help                Show this help message and exit.
  -i, --input=<input>       input stream; URL if starts with 'http' else file
   [...]
  -v, --verbose             Specify multiple -v options to increase verbosity.
                            For example, `-v -v -v` or `-vvv`We map ERROR or WARN ->
                              0 (i.e. always print), INFO -> 1(-v), DEBUG->2 (-vv)
                              Default: []
  -V, --version             Print version information and exit.
  ```

## options
  Options control what the command does and operates upon. Here are ones you will encounter
  
  * `--directory`
  * `--hreftext`
  * `--informat`
  * `--outformats
  * `--terms`
  * `--input`
 
 You are unlikely to use anything else in the TIGR2ESS workshop.

## input values
 The system also outputs the values you entered (some may be created by default). If you have bugs/problems you should always 
 include these when reporting errors.
  
  ```

Generic values (AMIDictionaryTool)
================================
basename            null
cproject            
ctree               
cTreeList           null
dryrun              false
excludeBase         null
excludeTrees        null
file types          []
forceMake           false
includeBase         null
includeTrees        null
log4j               
logfile             null
verbose             0

Specific values (AMIDictionaryTool)
================================
dataCols      null
dictionary    null
dictionaryTop     null
href          null
hrefCols      null
input         null
informat      null
dictInformat  null
linkCol       null
log4j         null
nameCol       null
operation     help
outformats    [xml]
splitCol      ,
termCol       null
terms         null
wikiLinks     [wikipedia, wikidata]

```

# creating from a list of terms

This is the simplest way. Create a list of terms that are likely to be in Wikipedia and/or Wikidata. Here is an example for monoterpenes.
`cd` to the place where you want to put the dictionaries.
## input
```
ami-dictionary create --terms thymol pineol menthol --dictionary myterpenes --directory mydictionaries --outformats xml,html
```
Let's look at the components:
* `ami-dictionary` runs the command
* `create` is one of the several parameters that can be used (we'll see others later)
* `--terms` requires a list of terms (as many as you like until the next `--`) here `thymol pineol menthol`.
* `--dictionary myterpenes` creates the `basename` for the output dictionaries.
* `--directory mydictionaries` the directory where to put the dictionaries. This is relative to where you run the command (unless a full pathname is given, e.g. `/Users/pm286/ContentMine/dictionaries/`
* `outformats "xml,html"` output dictionaries in various formats. Hers it creates `myterpenes.xml` and `myterpenes.html`. 

## output
```
 ami-dictionary create --terms thymol pineol menthol --dictionary myterpenes --directory mydictionaries --outformats xml,html
```
### input values
```
Generic values (AMIDictionaryTool)
================================
basename            null
cproject            
ctree               
cTreeList           null
dryrun              false
excludeBase         null
excludeTrees        null
file types          []
forceMake           false
includeBase         null
includeTrees        null
log4j               
logfile             null
verbose             0

Specific values (AMIDictionaryTool)
================================
dataCols      null
dictionary    [myterpenes]
dictionaryTop     mydictionaries
href          null
hrefCols      null
input         null
informat      null
dictInformat  null
linkCol       null
log4j         null
nameCol       null
operation     create
outformats    [xml, html]
splitCol      ,
termCol       null
terms         null
wikiLinks     [wikipedia, wikidata]
```
### output
```
.!.>CM.myterpenes.0>CM.myterpenes.1>CM.myterpenes.2+++
Missing wikipedia: :

pineol; 
```
The output can be verbose so we normally track each dictionary entry with a single character. `.` or `+` normally means OK, `!` pr `?` means something went wrong. We got:
```
.!.
```
so something went wrong with `pineol`.  The diagnostic:
```
Missing wikipedia: :

pineol; 
```
means that we couldn't find `pineol` in Wikpedia (but we could find it in Wikidata.
### dictionaries
The XML dictionary contains:
```
<?xml version="1.0" encoding="UTF-8"?>
<dictionary title="myterpenes">
 <entry term="menthol" wikipedia="menthol" wikidata="Q407418" name="‎(-)-menthol‎" description="chemical compound" id="CM.myterpenes.0"/>
 <entry term="pineol" wikidata="Q198654" name="‎pineal gland‎" description="small endocrine gland found in most vertebrates, which produces melatonin; in humans, located in the epithalamus, in a groove where the two halves of the thalamus join; its shape and size resembles a pine nut, after which it is named" id="CM.myterpenes.1"/>
 <entry term="thymol" wikipedia="thymol" wikidata="Q408883" name="‎thymol‎" description="chemical compound" id="CM.myterpenes.2"/>
 <query>('menthol') OR ('pineol') OR ('thymol')</query>
</dictionary>
```
This is a <b>`dictionary`</b> "element" with 3 <b>`entry`</b> "child elements" (notice the indents). Each entry contains:
* `term="menthol"` the term that will be used for identification and searching.
* `name="‎(-)-menthol‎" the name by which it is known in Wikipedia or Wikidata.
* `wikipedia="thymol" the relative address of the Wikipedia entry 
* `wikidata="Q407418"` the Wikidata identifier (aka `item`)
* `description="chemical compound"` Wikidata's formalised description (which can sometimes be used for classification).
* `id="CM.myterpenes.0"` a universal identifier over ContentMine dictionaries.

### goof-up!!
The entry for `pineol` is not a chemical compound! That's because I mistyped `cineol`! And that's why we never found it's entry. Wikidata did a fuzzy search and found `pineal` .
** this is a warning to check everything! Don't trust Wikidata blindly. Or anything else!

