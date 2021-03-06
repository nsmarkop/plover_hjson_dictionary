Plover Hjson Dictionary
=======================

`Hjson <https://hjson.org/>`__ dictionary support for
`Plover <https://github.com/openstenoproject/plover>`__.

Installation
------------

Download the latest version of Plover for your operating system from the
`releases page <https://github.com/openstenoproject/plover/releases>`__.
Only versions 4.0.0.dev1 and higher are supported.

1. Open Plover
2. Navigate to the Plugin Manager tool
3. Select the “plover-hjson-dictionary” plugin entry in the list
4. Click install
5. Restart Plover

The same method can be used for updating and uninstalling the plugin.

Dictionary Format
-----------------

The Hjson dictionary format used by this plugin maps translations to
strokes rather than strokes to translations which differs from the
default JSON dictionary format used by Plover. For a basic comparison,

JSON:

.. code:: json

    {
    "PHRO*EFR": "Plover",
    "PHRO*FR": "plover",
    "PHROFR": "Plover",
    }

Hjson:

.. code:: yaml

    {
      Plover: # A cool open source project
      [
        PHRO*EFR
        PHROFR
      ]
      plover: # A bird
      [
        PHRO*FR
      ]
    }

Comments added within the Hjson dictionary file are currently not
preserved on dictionary modification due to round-trip limitations of
the Hjson package for Python.

Converting JSON dictionaries to Hjson
-------------------------------------

You can use the following script to take an input JSON dictionary file
path and output Hjson dictionary file path to convert existing Plover
JSON dictionaries to the Hjson format used by this plugin.

.. code:: python


    import json

    import hjson


    JSON_FILENAME = r''
    HJSON_FILENAME = r''

    # Load JSON dictionary
    with open(JSON_FILENAME, 'r', encoding='utf-8') as in_file:
        in_data = json.load(in_file)

    # Group dictionary by value, sorted alphabetically
    out_data = {}

    for key, value in sorted(in_data.items(), key=lambda x: x[1].casefold()):
        out_data.setdefault(value, []).append(key)
        out_data[value] = sorted(out_data[value])

    # Write dictionary to Hjson
    with open(HJSON_FILENAME, 'w', encoding='utf-8') as out_file:
        hjson.dump(out_data, out_file,
                   sort_keys=True, ensure_ascii=False, encoding='utf-8')

