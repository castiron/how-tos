## ExtBase Snippets

This is a list of useful snippets for making ExtBase extensions.

* [How to make an update script](https://github.com/castiron/sjcert/blob/master/class.ext_update.php)
* [How to register scheduled task](https://github.com/castiron/parallels.com/blob/master/Source/typo3conf/ext/l10nmgrclient/ext_localconf.php#L3-L7)
* [How to register scheduled task with additional fields](https://github.com/castiron/sjcert/blob/master/ext_localconf.php#L162-L167) and [how to make the fields](https://github.com/castiron/sjcert/blob/master/Classes/Task/EnvironmentCheck.php#L39-L70)
* [Bi-directional relationship 1:m](https://github.com/castiron/sjcert/blob/master/Configuration/TCA/Applicant.php#L127-L172) (child applicants : parent applicant)
* [Locallang override](https://github.com/castiron/sustainable-schools/blob/3a5046307acbc86eb19b771a217b71602da8b1cd/Source/typo3conf/ext/t3site/ext_localconf.php#L11-L13)
* [Dynamically add property validators](https://github.com/castiron/cicevents/blob/master/Classes/Controller/EventController.php#L93-L101)
* [Dynamically add property type converter](https://github.com/castiron/cicevents/blob/master/Classes/Controller/EventController.php#L118-L123)
* [Allowing sub-property mapping](https://github.com/castiron/cicevents/blob/master/Classes/Controller/EventController.php#L104-L116)
* [Setting property errors _after_ property mapping/validation has already happened](https://github.com/castiron/naset.org/blob/master/Source/typo3conf/ext/nasetregistration/Classes/Controller/FrontendUserController.php#L743-L784)
* [Flexform displaycond with AND and OR](https://github.com/castiron/oregonbest/blob/master/Source/typo3conf/ext/orbest/Configuration/FlexForms/flexform_partners.xml#L87-L95)
* [Alter how page caches are identified](https://github.com/castiron/oregonbest/blob/master/Source/typo3conf/ext/orbest/Classes/Controller/PartnerController.php#L333-L362) - useful for caching pages depending on variable information (like user permissions)
* [Injecting settings](https://github.com/TYPO3/TYPO3.CMS/blob/master/typo3/sysext/extbase/Classes/Mvc/Controller/AbstractController.php#L147-L155)
* [Dynamically add new action arguments](https://github.com/TYPO3/TYPO3.CMS/blob/TYPO3_6-2/typo3/sysext/extbase/Classes/Mvc/Controller/ActionController.php#L183)
* [Inline relationships](https://wiki.typo3.org/Inline_Relational_Record_Editing_Attributes#Bidirectional_symmetric_relations)
* [Weird RTE stuff](http://www.typo3.net/forum/thematik/zeige/thema/39948/?show=1)


Also, check out the [cookbook](http://cicdocs.uat.cicdev.net/cookbook/)