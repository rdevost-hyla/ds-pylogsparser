===============
Log Line Parser
===============
Parse log lines (strings).

This migrates pylogsparser-0.8 to Python 3 and extends to handle json logs.
See the original README in README-ORIGINAL.rst.

INSTRUCTIONS
============
Log entries (lines) can be (1) simple strings with keywords and possibly delimiters to designate
its components, (2) CSV records, or (2) json records with key/value items. The record formats
are defined in XML files in the normalizers folder.

Log lines are parsed using definition XML files in the `normalizers` folder.

To use, initialize the `normalizer`, then pass it a dictionary with the log line loaded as the
value for the key "raw".  Values extracted from the log line are added to the dictionary.

Example::

    from ds-pylogsparser.lognormalizer import LogNormalizer

    ln = LogNormalizer(<normalizer folder>)
    for line in lines:
        parsed_line = {'raw': line}
        ln.lognormalize(parsed_line)
        if parsed_line['level'] == 'ERROR'
            print(f'Line {parsed_line["raw"]} had an ERROR')

XML Elements of Normalizer Files
================================
<pattern name="some_pattern">...</pattern>::

    Fields to extract from the log file are defined as in <tag> elements. If a record is a CSV
    or JSON format, then a <text type="csv"...> or <text type="json"...> element is included.
    See the CSVPattern and JSONPattern classes in normalizers.py for instructions on how to
    configure the csv and json records.

<tag name="some_tag" tagType="some_type">...</tag>::

    This defines a piece of the log file. There are a number of predefined tagTypes
    listed in the normalizers/common_tagTypes.xml file; new tagTypes can also be created in
    this file.

<callback>some_callback_function</callback>::

    To modify the data extracted from the log record before outputting it, callback functions
    are used. There are common callbacks (e.g., for date transformations) defined in the
    normalizers/common_callBacks.xml file. Callbacks can also be defined directly in the
    current XML file in the <callbacks> section.

    Only the body of a callback function is defined. The function will receive two parameters:
    `value`, witch is portion extracted from the <tag>, and `log`, which is the whole log record
    transformed into a dictionary.

    The callback does not return any values, rather it adds dictionary items to the log
    record (dictionary). For example, a complete callback could be:

    log['add_50_pct'] = round(float(value) * 1.5, 2)

    These callbacks are hard to debug. To execute the callback in debug mode, add a breakpoint
    where it's executed and call it manually. The full function can also be viewed by
    displaying callback.__doc__.

<substitute>some_name</substitute>::

    The substitute name is used for searching the log record/dictionary instead of the tag
    name. Consider its meaning as, substitute this word for tag name when searching the
    log. The final log dictionary will replace the tag key with the substitute name.

NOTES
=====
For unit testing, environment variable NORMALIZERS_PATH must be set with absolute path to
the normalizer definition files.
