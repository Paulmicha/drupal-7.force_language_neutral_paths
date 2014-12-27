drupal-7.force_language_neutral_paths
=====================================

When visiting admin/content page, aliases don't work for nodes in different language (we get 404 errors unless current language is the same).
-> this module forces newly created aliases to stay language neutral, preventing this unwanted default behaviour.

Source & credit :  
https://www.drupal.org/node/1234924#comment-9235023  
https://www.drupal.org/node/1234924#comment-9296037


If you already have lot of existing content with wrong language, this module will help :  
https://www.drupal.org/sandbox/akamaus/1420990

Or you can run once the following code:
```
<?php
function _neutral_paths_set_all_to_neutral($type) {
  $num_updated = db_update('url_alias')
    ->fields(array('language' => LANGUAGE_NONE))
    ->condition('language', LANGUAGE_NONE, '!=')
    ->condition('source', $type . '/%', 'LIKE')
    ->execute();
  if ($num_updated > 0) {
    drupal_set_message(t('@num aliases were reset to language neutral', array('@num' => $num_updated)));
  }
  else {
    drupal_set_message(t('No aliases were updated.') . $type);
  }
}
_neutral_paths_set_all_to_neutral('node');
?>
```