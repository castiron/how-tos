Dynamic fields
===
### Or, fields that update based on other fields

Native flexform capabilities
---

See [This blog post](http://www.t3node.com/blog/dynamic-forms-and-types-with-typo3-flexforms/) for more on native flexform stuff, but here's a quick summary:

### Reloading the form on change (`onChange`)
    <settings.something>
        <TCEforms>
            <label>An important field</label>
            <onChange>reload</onChange>
            <config>
                ...
            </config>
        </TCEforms>
    </settings.something>

### Showing a field only in certain conditions (`displayCond`)
    <settings.somethingElse>
        <TCEforms>
            <label>A less important field</label>
            <displayCond>FIELD:settings.something:=:valueIWant</displayCond>
            <config>
                <type>input</type>
            </config>
        </TCEforms>
    </settings.somethingElse>

In TYPO3 >= 6.1 you can use multiple `displayCond`s like this:

    <displayCond>
        <OR>
            <numIndex index="1">FIELD:settings.fieldOne:=:firstValue</numIndex>
            <numIndex index="1">FIELD:settings.fieldTwo:=:secondValue</numIndex>
        </OR>
    </displayCond>


itemsProcFuncs for extra power
---
Occasionally the above is not enough. In these cases you can use an `itemsProcFunc` function to get what you need. You'll call the function from the flexform, use it to massage the field's values, then return them to the flexform again for normal use.

### Calling your itemsProcFunc in the flexform

    <settings.defaultTier>
        <TCEforms>
            <label>Default Tier</label>
            <config>
                <type>select</type>
                <itemsProcFunc>\You\YourExtension\Utility\General->flexTierSelect</itemsProcFunc>
            </config>
        </TCEforms>
    </settings.defaultTier>

In TYPO3 < 6.0, you call the `itemsProcFunc` like any other old function:

```
<itemsProcFunc>Tx_YourExtension_Something->someFunction</itemsProcFunc>
```

### Writing your itemsProcFunc

The itemsProcFunc will receive the current field's configuration as an argument. 'configuration' here means a `$config` array along these lines:

    $config => array(
        'config' => array(  # Yes, a config within a $config
            'form_type' = 'select'
            'itemsProcFunc' = '\You\YourExtension\Utility\General->flexTierSelect'
            'type' = 'select'
        )
        'field' => 'pi_flexform'
        'items' => array()
        'row' => array(
            # (contains the tt_content table row data for this content element)
        )
        'table' => 'tt_content'
        'TSconfig' => null
    )

Your `itemsProcFunc` will massage this `$config` array and return it.  Here's an example:

    function flexTierSelect($config) {

        # Retrieve the current flexform values from the tt_content row and get them into a usable array
        $piValues = \TYPO3\CMS\Core\Utility\GeneralUtility::xml2array($config['row']['pi_flexform']);

        # Get the values you want for this select element out of the flexform, and put them in the $config array
        # $config is slightly like a TCA field: for a select element, you put your array of options into $config['items']
        $tierData = $piValues['data']['sheetOne']['lDEF']['settings.tiers']['el'];
        if($tierData) {
            $i = 0;
            foreach($tierData as $tier) {
                $config['items'][] = array(
                        0 => $tier['data']['el']['title']['vDEF'],
                        1 => $i
                        );
                $i++;
            }
        }

        # Yay
        return $config;
    }
