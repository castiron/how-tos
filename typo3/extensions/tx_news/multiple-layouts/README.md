Using multiple layouts in tx_news
===

This is simple, but I often have to look it up, so here it is.

In `ext_localconf.php`, you set up an array of available News templates like so:

    $GLOBALS['TYPO3_CONF_VARS']['EXT']['news']['templateLayouts'] = array(
        0 =>  array(
            0 =>  'Homepage List Template', //Label
            1 =>  'HomepageList'  //Key
        ),
        1 =>  array(
            0 =>  'List With Teaser Text',
            1 =>  'ListWithTeasers'
        ),
    );

If you want, you can also put this in full-on `LocalConfiguration.php` instead. Whatever floats your boat.

Now the user will be able to select these template in news plugins, and you'll be able to do if-conditions with the template key in the actual template files, like this:

    <f:if condition="{0:settings.templateLayout} == {0:'HomepageList'}">
        <h3>Latest News</h3>
        <ul class="news-list">
            <f:for each="{news}" as="newsItem">
                <f:render partial="List/HomepageItem" arguments="{newsItem: newsItem, settings:settings, className:className, view:'list'}"/>
            </f:for>
        </ul>
    </f:if>

