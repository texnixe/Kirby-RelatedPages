Kirby-RelatedPages
==================

RelatedPages Plugin for Kirby CMS Version 1

The RelatedPages plugin provides an easy, but flexible way of 
incorporating links (or other data) of related pages to the
current page. The relationsship is considered by keywords in an
arbitrary field of the content file.

Installation:
=============

Save this file into the plugins folder of Kirby.

Basic usage:
============

```php
<?php
$MyRelatedPages = new RelatedPages();

foreach ($MyRelatedPages->Pages() as $UID => $Page) {
  echo $Page->title();
}
?>
```


Methods:
========

`$MyRelatedPages->Pages()` outputs an array with all related pages. The key
of the array is the uid of the related page and the value is the kirby page
object.

`$MyRelatedPages->Count()` outputs an integer with the number of related
pages found.

`$MyRelatedPages->Options()` outputs the options array (see next) in a
readable way.

Note: If you use the object as string, you will get an array of uids of
the pages found in a readable way. Example: `<?php echo $MyRelatedPages ?>`

Options:
========

You can control which pages are searched for and which pages are found by
a number of options, which should be supplied as an associative array upon
instantiation of a new object:

`<?php $MyRelatedPages = new RelatedPages($Options); ?>`

Example: `$Options = array('VisibleOnly' => false, 'Depth' => 1);`

This will find all pages, not only visible pages, and the depth of recursion
is 1, which means 1 level down the page hyrachie starting at the root level.

Possible options (with defaults values in parenthesis):
-------------------------------------------------------

'VisibleOnly' (true)     If true, searches only visible pages, otherwise all.

'StartURI'    ('')       Start folder of search. If blank, starts at the root
                         level. Use only folder names without numbers. No
                         trailing slash. Example: '/folder/subfolder'

'Depth'       (0)        Depth of recursion into the folder structure. 0
                         means infinitely. Count starts at StartURI level,
                         this means it is relative to the root level.

'Field'       ('Tags')   The name of the field in your content file which
                         holds the keywords.

'Items'       (array())  A list of keywords which should be searched for. An
                         empty array means that all keywords will be searched
                         for.
                        

