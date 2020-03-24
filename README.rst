ds-pylogsparser
===============
Parse log files.

This migrates pylogsparser-0.8 to Python 3 and extends to handle json logs.
See the original README in README-ORIGINAL.rst.

INSTRUCTIONS
============
Log entries (lines) can be (1) simple strings with keywords and possibly delimiters to designate
its components, or (2) json records with key/value items.

In the first case, define the log record using an XML file. In the second, designate
Logs that are in delimited string formats can be defined using an XML template.

NOTES
=====
Environment variable NORMALIZERS_PATH must be set with absolute path to normalizers.
