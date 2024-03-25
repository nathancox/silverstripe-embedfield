SilverStripe EmbedField
===================================

This field is designed to let users attached an oembed object (eg a YouTube video) to a page or dataobject.  It stores the oembed result information in an EmbedObject for easy access from the template (or wherever you want it).

Work in progress.

Requirements
------------
* SilverStripe 4.11+ or 5.x

Documentation
-------------
[GitHub](https://github.com/nathancox/silverstripe-embedfield/wiki)

Installation Instructions
-------------------------

1. Install with composer `composer require nathancox/embedfield`
2. Visit yoursite.com/dev/build to rebuild the database

Usage Overview
--------------

Make a has_one relationship to an EmbedObject then create an EmbedField in getCMSFields:

```php
namespace {

    use SilverStripe\CMS\Model\SiteTree;
    use nathancox\EmbedField\Model\EmbedObject;
    use nathancox\EmbedField\Forms\EmbedField;

    class Page extends SiteTree
    {
        private static $db = [];

        private static $has_one = [
            'MyVideo' => EmbedObject::class
        ];
        
        public function getCMSFields() {
            $fields = parent::getCMSFields();
            
            $fields->addFieldToTab('Root.Main', EmbedField::create('MyVideoID', 'Sidebar video'));
            
            return $fields;
        }
    }
}
```

Gives us:

![example embedfield](https://i.ibb.co/Hxz7VGB/embedfield.jpg)


In the page template the video can now be embedded with `$MyVideo`.


Each embed type is rendered with it's own template (eg EmbedObject_video.ss and EmbedObject_photo.ss).  The default templates just return the markup generated by SilverStripe's OembedResult::forTemplate().  You can override them in your theme:

themes/mytheme/templates/nathancox/EmbedField/Model/EmbedObject_video.ss:
```html

	<div class='flex-video self-sizing' style='padding-bottom:$AspectRatioHeight;'>
		$EmbedHTML
	</div>

```

This can be combined with your own CSS to make aspect ratio aware flexible video (see http://alistapart.com/article/creating-intrinsic-ratios-for-video).


Known Issues
------------
[Issue Tracker](https://github.com/nathancox/silverstripe-embedfield/issues)
