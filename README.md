Kirby-RelatedPages
==================

## RelatedPages Plugin for Kirby CMS

The RelatedPages plugin provides an easy, but flexible way of incorporating links (or other data) of related pages to the current page. The relationsship is considered by keywords in an arbitrary field of the content files. Example: If you provide the field 'Tags' in your content file which is rendered together with the RelatedPages plugin, all pages in your site will be searched for any of the *tags* (seperated by comma) in the field 'Tags'.

### Installation

Kirby 1: Save the file `RelatedPages.php.V1` as `RelatedPages.php` into the plugins folder of Kirby 1.

Kirby 2: Save the file `RelatedPages.php.V2` as `RelatedPages.php` into the plugins folder of Kirby 2.

### Basic usage

```php
<?php
$MyRelatedPages = new RelatedPages();

foreach ($MyRelatedPages->Pages() as $UID => $Page) {
  echo $Page->title();
}
?>
```

### Methods

- `$MyRelatedPages->Pages()` outputs an array with all related pages. The key of the array is the uid of the related page and the value is the kirby page object.

- `$MyRelatedPages->Count()` outputs an integer with the number of related pages found.

- `$MyRelatedPages->Options()` outputs the options array (see next) in a readable way.

**Note**: If you use the object as string, you will get an array of uids of the pages found in a readable way. Example: `<?php echo $MyRelatedPages ?>`

### Options

You can control which pages are searched for and which pages are found by a number of options, which should be supplied as an associative array upon instantiation of a new object:

```php
<?php
$Options = array('VisibleOnly' => false,'Depth' => 1);
$MyRelatedPages = new RelatedPages($Options);
?>
```

This will find all pages, not only visible pages, and the depth of recursion is 1, which means 1 level down the page hierachy starting at the root level.

#### Possible options

| Key         | Value   | Default | Description |
|-------------|---------|---------|-------------|
| VisibleOnly | Bool    | true    | If true, searches only visible pages, otherwise all. |
| StartURI    | String  | ''      | Start folder of search. If blank, starts at the root level. Use only folder names without numbers. No trailing slash. Example: '/folder/subfolder' |
| Depth       | Integer | 0       | Depth of recursion into the folder structure. 0 means infinitely. Count starts at StartURI level, this means it is relative to the root level. |
| Field       | String  | 'Tags'  | The name of the field in your content file which holds the keywords. |
| Items       | Array   | array() | A list of keywords which should be searched for. An empty array means that all keywords which are found in the 'Field' will be searched for. |

