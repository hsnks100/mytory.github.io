~~~ php
<?
$file = $argv[1];

echo "{$file}\n";

$content = file_get_contents($file);
$tokens = token_get_all($content);
$output = '';

foreach($tokens as $token) {
 if(is_array($token)) {
  list($index, $code, $line) = $token;
  switch($index) {
   case T_OPEN_TAG_WITH_ECHO:
    // $output .= '<?php echo ';
    break;
   case T_OPEN_TAG:
    $output .= '<?php ';
    break;
   default:
    $output .= $code;
    break;
  }

 }
 else {
  $output .= $token;
 }
}
file_put_contents($file, $output);
~~~



    find application -type f -name \*php -exec php -d short_open_tag=On replace_short_open_tag.php {} \;
    

