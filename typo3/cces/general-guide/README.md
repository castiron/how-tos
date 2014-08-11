Making a CCE
===

There are a few ways to set up your CCE, and the best one to use depends context, but the general process is the same.

Configuring the new CCE in localconf.php
---
First, you need to answer the following questions in `localconf.php`:

- What MVC action renders this CCE?
- Should this type of element be cached?
- What fields does the CCE's backend user interface have?
- What is the CCE called in the backend?

Here's an example (we are using a flexform to show the bulk of the fields in this case):

    'cce_name_goes_here' => array(
        # Name of the corresponding action in contentController.php - in this case, it will be 'cceNameGoesHereAction'.
        'action' => 'cceNameGoesHere',
        # Should this type of element be cached?
        'noCache' => false,
        # The 'ui' string uses the moronic 'showitem' syntax to define what will appear in the backend user interface for this type of element.
        # See below for more on showitem syntax.
        'ui' => '
            CType;;4;button;1-1-1,
            --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.frames;frames,
            header,
            pi_flexform,
            --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.access, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.visibility;visibility, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.access;access,
            --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.extended,
            tx_gridelements_container,
            tx_gridelements_columns
            ',
        # label is the text that will show up in the type field on tt_content records, and what users will see when looking at a page with this element in the backend.
        'label' => 'CCE Name Goes Here'
    )

Showitem syntax is documented further [here](../../tca/showitem-syntax/README.md).

There are a few approaches to deciding which fields to use in the 'ui' variable, and the best one to use depends on your circumstances:

|Level of complexity|Scenario|Approach|
|-------------------|--------|--------|
|Low|You only need a few fields in your CCE, and they are standard items like a link, a media element, a header, etc.|Use already-existing columns from generic `tt_content` rows to fit your needs. You can provide custom titles for them using showitem syntax. This saves you from needing to modify the database at all. <br><br> *Caveat:* If you're going to need to refer to one of the fields from another element (Another CCE, whatever), or if one of  your fields needs to be totally unique for some reason, you should probably use the following option instead.|
|Middling|Existing `tt_content` columns can cover most of your needs, but there is one field (or *maybe* two) that can't be represented by something already available in `tt_content`|Use the `tt_content` fields for most of your fields, then extend the TCA with a new field in ext_tables.php and include it here. In this case you need to modify the database, but only with one field.|
|High|You have multiple fields that can't be represented by an existing tt_content column.|Here you should use a flexform.|

### A few 'ui' examples

**1. Simple element with no particularly special fields:**

    'ui' => '
        CType;;4;button;1-1-1,
        header,
        image;Background Image,
        bodytext;Description,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.access, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.visibility;visibility, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.access;access,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.extended,
        tx_gridelements_container,
        tx_gridelements_columns
    ',

Standard `tt_content` header, image, and bodytext, fields are used, but the image and bodytext fields are given custom labels to make their purpose in this particular case clearer to the user.

**2. Element with one nonstandard field:**

    'ui' => '
        CType;;4;button;1-1-1,
        header,
        image;Thumbnail,
        tx_t3site_background_image,
        bodytext,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.access, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.visibility;visibility, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.access;access,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.extended,
        tx_gridelements_container,
        tx_gridelements_columns
    ',

Here we needed two image fields, but `tt_content` only had one to offer. We used `tt_content`'s 'image' field as the Thumbnail field, then extended the `tt_content` $TCA to include a new image field, called `tx_t3site_background_image`. Note that the extension name (`tx_t3site`) is prepended to the field name for clarity about where this field came from.

In this case, we would extend the $TCA like this in `ext_tables.php`:

    # Configuration for background image field
    $tempColumns = array (
      # Include the extension name at the beginning for clarity
      'tx_t3site_background_image' => array (
        'exclude' => 0,
        'label' => 'Background Image',
        'config' => \TYPO3\CMS\Core\Utility\ExtensionManagementUtility::getFileFieldTCAConfig('tx_t3site_background_image')
        )
      ),
    );

    t3lib_extMgm::addTCAcolumns('tt_content',$tempColumns,1);

And in `ext_tables.sql`:

    CREATE TABLE tt_content (
      tx_t3site_background_image varchar(255) default NULL
    );

Don't forget to update the databse using the Install Tool after you extend the $TCA like this.

**3. Complex element:**

    'ui' => '
        CType;;4;button;1-1-1,
        header,
        pi_flexform,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.access, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.visibility;visibility, --palette--;LLL:EXT:cms/locallang_ttc.xml:palette.access;access,
        --div--;LLL:EXT:cms/locallang_ttc.xml:tabs.extended,
        tx_gridelements_container,
        tx_gridelements_columns
    ',

Here the backend interface was too complex for us to attempt to use `tt_content` fields to cover what we needed. Instead we included the standard header field as usual, but wrote a separate flexform to define the rest of the fields. The path to this flexform is specified in `ext_tables.php`, like this:

```
$TCA['tt_content']['columns']['pi_flexform']['config']['ds']['*,' . $_EXTKEY . '_cce_name_goes_here'] = 'FILE:EXT:' . $_EXTKEY . '/Sites/Main/Resources/Configuration/FlexForms/cce_name_goes_here_flexform.xml';
```

(In this case your CCE would be called `tx_t3site_cce_name_goes_here`)

Making the CCE available in the New Content Element Wizard with TScofnig
---
In site TSconfig, add the following to `mod.wizards.newContentElement.wizardItems.client.elements`:

    t3site_cce_name_goes_here {
      title = (The title of the CCE as shown in the New Content Element Wizard)
      description = (Description for use in the New Content Element Wizard)
      icon = gfx/c_wiz/user_defined.gif (or something custom)
      tt_content_defValues {
        CType = t3site_cce_name_goes_here
      }
    }

Writing the controller action
---
In `ContentController.php` write a new action like this:

    function cceNameGoesHereAction() {
      # Here you can access the regular tt_content fields like this:
      $data = $this->cObj->data;

      # Or your flexform data like this:
      $settings = $this->settings;

      # Do some stuff with them...
      # ...
      # ...and assign what you need to the template:
      $this->view->assign('header', $data['header']); # You can refer to $data['header'] as {header} in the fluid template now
    }

Writing the view
---
In `Resources/Private/Templates/Content`, add a view along the lines of 'CceNameGoesHere.html'. Here you'll be able to use the variables you assigned to your view like this:

    <f:if condition="{header}">
      <h1>{header}</h1>
    </f:if>
