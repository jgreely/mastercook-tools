# mastercook-tools
Scripts for working with MasterCook's MX2 export format

## Overview

MX2, the popular recipe export format introduced in MasterCook 5
(1999), is not quite XML. It's close, but there are several errors,
and the DTD included with the software is just plain wrong. In
addition to fixing those problems, I've knocked together some Perl
scripts to work with and clean up MX2 files collected from assorted
Internet archives, such as the archived [Mad's Recipes].

## Files

* `mx2.dtd` - working DTD to validate against, includes the `img`
attribute introduced in recent versions, and handles the common
error of putting the summary list at the end.

* `mx2toxml` - convert to valid UTF-8 XML (uses xmllint to do the
character-set conversion and double-check validity).

* `mx2fixdir` - split up recipe directions that include embedded CRLF
into separate directions, and migrate obvious notes to the Notes
section (common in recipes originally exported to the text MXP format).

* `mx2cat` - concatenate one or more MX2 files into a composite file,
sorting them by name and removing any duplicates (detected by MD5
checksum). Exact duplicates are usually the result of exporting
embedded recipes (`<IngR ... code="R">`) that are used more than once.

* `mx2grep` - search one or more MX2 files for recipes matching all
of the supplied regular expressions, in `--name`, `--ingredient`,
or `--anywhere`. Also does the same sort/uniq that `mx2cat` does.
Has `-l` option to just list the names of the matching recipes.

* `mx2ls` - list the names of all recipes in one or more MX2 files.

* `mx2split` - write out one MX2 file per recipe. Recombine with
`mx2cat` after cleaning up duplicates and removing unwanted recipes.

* `mz2` - the MZ2 format required for uploads to mastercook.com is
just a ZIP file containing an MX2 file, so this is a trivial Bash
script to do that.

* `xml2md` - convert every recipe in an MX2 file into a separate
Hugo content file, suitable for my [recipe theme]. This one still
needs a bit of work, but it does capture every field I've ever
seen used in an MX2 file.

## Notes

* While `mx2toxml` and `mz2` write to files, the rest write to STDOUT, and
can read from STDIN.

* Some MX2 files need manual edits before converting to XML. `xmllint`
will let you know by barfing on them during the conversion.

## Other Tools

* [cb2cb] - Windows Java app that converts between various formats.
Good for quickly converting MXP to MX2, provided you uncheck all the
options.

[Mad's Recipes]: https://web.archive.org/web/20090630125642/http://www.madsrecipes.com:80/recipes/
[cb2cb]: http://recipetools.sourceforge.net/joomla3/index.php/cb2cb
[recipe theme]: https://github.com/jgreely/hugo-theme-recipe
