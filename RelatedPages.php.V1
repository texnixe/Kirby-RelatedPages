<?php 

if(!defined('KIRBY')) die('Direct access is not allowed');

/**
 * RelatedPages plugin for Kirby CMS Version 1
 *
 * The RelatedPages plugin provides an easy, but flexible way of 
 * incorporating links (or other data) of related pages to the
 * current page. The relationsship is considered by keywords in an
 * arbitrary field of the content file.
 *
 * @author Uwe Gehring <uwe@imap.cc>
 * @copyright 2013 Uwe Gehring
 * @version 1.0
 *
 * Installation:
 * =============
 *
 * Save this file into the plugins folder of Kirby.
 *
 * Basic usage:
 * ============
 *
 * <?php
 * $MyRelatedPages = new RelatedPages();
 *
 * foreach ($MyRelatedPages->Pages() as $UID => $Page) {
 *   echo $Page->title();
 * }
 * ?>
 *
 * Methods:
 * ========
 *
 * $MyRelatedPages->Pages() outputs an array with all related pages. The key
 * of the array is the uid of the related page and the value is the kirby page
 * object.
 *
 * $MyRelatedPages->Count() outputs an integer with the number of related
 * pages found.
 *
 * $MyRelatedPages->Options() outputs the options array (see next) in a
 * readable way.
 *
 * Note: If you use the object as string, you will get an array of uids of
 * the pages found in a readable way. Example: <?php echo $MyRelatedPages ?>
 *
 * Options:
 * ========
 *
 * You can control which pages are searched for and which pages are found by
 * a number of options, which should be supplied as an associative array upon
 * instantiation of a new object:
 *
 * <?php $MyRelatedPages = new RelatedPages($Options); ?>
 *
 * Example: $Options = array('VisibleOnly' => false, 'Depth' => 1);
 *
 * This will find all pages, not only visible pages, and the depth of recursion
 * is 1, which means 1 level down the page hyrachie starting at the root level.
 *
 * Possible options (with defaults values in parenthesis):
 * -------------------------------------------------------
 *
 * 'VisibleOnly' (true)     If true, searches only visible pages, otherwise all.
 *
 * 'StartURI'    ('')       Start folder of search. If blank, starts at the root
 *                          level. Use only folder names without numbers. No
 *                          trailing slash. Example: '/folder/subfolder'
 *
 * 'Depth'       (0)        Depth of recursion into the folder structure. 0
 *                          means infinitely. Count starts at StartURI level,
 *                          this means it is relative to the root level.
 *
 * 'Field'       ('Tags')   The name of the field in your content file which
 *                          holds the keywords.
 *
 * 'Items'       (array())  A list of keywords which should be searched for. An
 *                          empty array means that all keywords will be searched
 *                          for.
 *                        
 */
 
class RelatedPages {

/**
 * @var array An associative array of pages found (uid => (obj)page)
 */
  private $Pages = array();

/**
 * @var array An associative array either of all pages or all visible pages
 */
  protected $AllPages = array();
  
/**
 * @var array An associative array of default options
 */
  protected $Defaults = array(
        'VisibleOnly'   => TRUE,
        'StartURI'      => '',
        'Depth'         => '0',
        'Field'         => 'Tags',
        'Items'         => array(),
  );

/**
 * Instantiation
 *
 * @param array $param An associative array of options
 * @return void
 */
  function __construct($param = array()) {
  
/**
 * Get the global $site object
 */
    global $site;

/**
 * Create the option object by merging default and supplied options
 */
    $this->Option = new RelatedPagesOptions(array_merge($this->Defaults,$param));
   
/**
 * Get pages from $site->pages()->index(), either all or only visible pages
 */ 
    if ($this->Option->VisibleOnly) { $this->AllPages = $site->pages()->visible()->index(); }
    else { $this->AllPages = $site->pages()->index(); }

/**
 * Get the items to search for from the current active page, but only if no items supplied
 * Items are get from the field supplied or from the field 'Tags'
 */
    if (count($this->Option->Items) == 0) { $this->Option->Items = str::split($site->pages()->active()->{str::lower($this->Option->Field)}); }

/**
 * Main loop #1: Go through all pages from index
 */    
    foreach ($this->AllPages as $UID => $Page) {

/**
 * Skip current active page
 */
      if ($Page->isActive()) continue;
   
/**
 * Skip page if StartURI is given and does not match
 */
      if (($this->Option->StartURI) && !stristr('/'.$UID, $this->Option->StartURI) ) continue;

/**
 * Skip page if Depth is given and depth of page is greater than this limit
 */
      if (($this->Option->Depth) && $Page->depth() > substr_count($this->Option->StartURI, '/') + $this->Option->Depth) continue;
    
/**
 * Main loop #2: Go through all items to search for
 */
      foreach ($this->Option->Items as $item) {

/**
 * Save page if the item appears in the corresponding field, from where we have got the items
 */
        if (a::contains(str::split($Page->{str::lower($this->Option->Field)}),$item)) { $this->Pages[$UID] = $Page; }
      }
    }
  }

/**
 * @return string A string with keys (uids) from pages found
 */
  function __toString() {
    return a::show(array_keys($this->Pages),false);
  }
  
/**
 * @return array An associative array of pages found
 */
  public function Pages() {
    return $this->Pages;
  }
  
/**
 * @return int The number of pages found
 */
  public function Count() {
    return count($this->Pages);
  }
  
/**
 * @return string A string with the options
 */
  public function Options() {
    return $this->Option;
  }
}

/**
 * Helper class to deal with the options
 */
class RelatedPagesOptions extends obj {

  function __construct($options = array()) {
    parent::__construct($options);
  }

  function __toString() {
    return a::show($this,false);
  }
}

/**
 * End of RelatedPages.php
 */
?>

