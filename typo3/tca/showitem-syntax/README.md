Showitem Syntax
===

The only real documentation for showitem is [here](http://docs.typo3.org/typo3cms/TCAReference/Reference/Types/Index.html#types-properties-showitem), but it warrants more information:

Fields in showitem strings are delimited by commas. Options for these fields are separated by semicolons. Each field has five possible total options, but only the first is required:

|Field component|Description|
|----|----|
|Field name|The name of the field as stored in the database. **Required.**|
|Label|If set, this overrides the default title associated with the field|
|Palette number||
|Special configuration options|e.g. rich text or nowrap. Multiple options here should be separated by colons. See [Additional TCA Features](http://docs.typo3.org/typo3cms/TCAReference/AdditionalFeatures/SpecialConfigurationOptions/Index.html) for the available options.|
|Form style codes|There are three visual customizations you can make to form fields: colorscheme, stylescheme, and borderscheme. There are also several numbered default styles, which provide the values for these three customizations, for example 1-2-1 would give color from default 1, style from default 2, and border from default 1. See [Visual Style of TCEforms](http://docs.typo3.org/typo3cms/TCAReference/AppendixA/StyleTceforms/Index.html) for more on these values.

The following keywords are also available to insert in place of a field:

|Keyword|Effect|
|-------|------|
|`--div--`|start a new tab here|
|`--newline--`|add a linebreak here|
|`--palette--`|add a predefined palette (defined in TCA 'palettes' section) by name here|
|`pi_flexform`|not really a keyword like the others, but use this when you want to insert the entire contents of a flexfom here - set the specific flexform in `ext_tables.php`|
