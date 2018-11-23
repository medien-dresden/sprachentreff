---
url: "send.php"
outputs:
  - PHP
showInSitemap: false
---
<?php

if ($_POST) {
  $to_email = "{{< email >}}";
  $from_email = 'noreply@medien-dresden.de';

  if (!isset($_SERVER['HTTP_X_REQUESTED_WITH'])
    || strtolower($_SERVER['HTTP_X_REQUESTED_WITH']) != 'xmlhttprequest'
  ) {
    http_response_code(400);
    die();
  } 

  $user_name  = filter_var($_POST["name"], FILTER_SANITIZE_STRING);
  $user_email = filter_var($_POST["email"], FILTER_SANITIZE_EMAIL);
  $subject    = filter_var($_POST["subject"], FILTER_SANITIZE_STRING);
  $message    = filter_var($_POST["message"], FILTER_SANITIZE_STRING);

  $message_body = $message;
  
  $headers = 
    'From: ' . $user_name . ' <' . $from_email . ">\r\n" .
    'Reply-To: ' . $user_email . "\r\n" .
    'X-Mailer: PHP/' . phpversion();
  
  if (mail($to_email, $subject, $message_body, $headers)) {
    http_response_code(204);
  } else {
    http_response_code(500);
  }

  exit();
}

?>