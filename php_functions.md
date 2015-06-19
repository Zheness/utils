# PHP functions

Some useful functions found on the web. I try to put the source every time.

## Seconds elapsed to literal form

```php
function time_elapsed($secs){
	$bit = array(
		' year'        => $secs / 31556926 % 12,
		' week'        => $secs / 604800 % 52,
		' day'        => $secs / 86400 % 7,
		' hour'        => $secs / 3600 % 24,
		' minute'    => $secs / 60 % 60,
		' second'    => $secs % 60
	);
	
	foreach($bit as $k => $v) {
		if($v > 1)$ret[] = $v . $k . 's';
		if($v == 1)$ret[] = $v . $k;
	}
	array_splice($ret, count($ret)-1, 0, 'and');
	$ret[] = 'ago.';
	
	return join(' ', $ret);
}
```

Usage :

```php
echo time_elapsed(8000);
// display "2 hours 13 minutes and 20 seconds ago.";
```

Source : http://php.net/manual/fr/function.time.php#108581

## Send a mail in PHP with Gmail

```php
ini_set("SMTP", "ssl://smtp.gmail.com");
ini_set("smtp_port", 465);
ini_set("auth_username", "email@gmail.com");
ini_set("auth_password", "password");

$to = 'email@gmail.com';	
$subject = 'Subject';
$message = 'Message';
$headers = 'From: webmaster@example.com' . "\r\n" .
'Reply-To: webmaster@example.com' . "\r\n" .
'X-Mailer: PHP/' . phpversion();

$response = mail($to, $subject, $message, $headers);
```

## Send a mail with PHPMailer

### Send mail using Gmail

```php
$mail = new PHPMailer();

// Configuration
$mail->Username = 'email@gmail.com';
$mail->Password = 'password';

$mail->Host = "ssl://smtp.gmail.com";
$mail->Port = 465;
$mail->SMTPSecure = 'ssl';
$mail->isSMTP();
$mail->SMTPAuth = true;
$mail->CharSet = "utf-8";
// /Configuration

$mail->addAddress('hello@gmail.com', 'John DOE');
$mail->isHTML(true);
$mail->Subject = 'Here is the subject';
$mail->Body = 'This is the HTML message body <b>in bold! hello</b>';
$mail->AltBody = 'This is the body in plain text for non-HTML mail clients';

$response = $mail->send();
```

### Send mail using another smtp

```php
$mail = new PHPMailer();

// Configuration
$mail->Username = 'email@domain.com';
$mail->Password = 'password';

$mail->Host = "smtp.domain.com";
$mail->Port = 465;
$mail->SMTPSecure = 'ssl';
$mail->isSMTP();
$mail->SMTPAuth = true;
$mail->CharSet = "utf-8";
$mail->FromName = 'Webmaster';						// You can customise the name
$mail->setFrom("weblocal@another-domain.com");		// and the mail of the sender
// /Configuration

$mail->addAddress('hello@gmail.com', 'John DOE');
$mail->isHTML(true);
$mail->Subject = 'Here is the subject';
$mail->Body = 'This is the HTML message body <b>in bold! hello</b>';
$mail->AltBody = 'This is the body in plain text for non-HTML mail clients';

$response = $mail->send();
```

## Generate a slug in multi-language

```php
public static function slugify($string, $replace = array(), $delimiter = '-')
{
	if (!extension_loaded('iconv')) {
		throw new Exception('iconv module not loaded');
	}
	// Save the old locale and set the new locale to UTF-8
	$oldLocale = setlocale(LC_ALL, '0');
	setlocale(LC_ALL, 'en_US.UTF-8');
	$clean = iconv('UTF-8', 'ASCII//TRANSLIT', $string);
	if (!empty($replace)) {
		$clean = str_replace((array) $replace, ' ', $clean);
	}
	$clean = preg_replace("/[^a-zA-Z0-9\/_|+ -]/", '', $clean);
	$clean = strtolower($clean);
	$clean = preg_replace("/[\/_|+ -]+/", $delimiter, $clean);
	$clean = trim($clean, $delimiter);
	// Revert back to the old locale
	setlocale(LC_ALL, $oldLocale);
	return $clean;
}
```

Source : https://github.com/phalcon/incubator/blob/master/Library/Phalcon/Utils/Slug.php

## Create a diff array (insert/update/delete)

```php
function diff_array($new, $old)
{
	$diff = [];
	foreach ($new as $e) {
		if (in_array($e, $old)) {
			$diff["update"][] = $e;
		} else {
			$diff["insert"][] = $e;
		}
	}
	foreach ($old as $e) {
		if (!in_array($e, $new)) {
			$diff["delete"][] = $e;
		}
	}
	return $diff;
}

$new = ["red", "blue", "green"];	// My new article's tags
$old = ["green", "violet", "purple"];	// My old article's tags

print_r(diff_array($new, $old));
/*
Output :
Array
(
	[insert] => Array
	(
		[0] => red
		[1] => blue
	)
	
	[update] => Array
	(
		[0] => green
	)
	
	[delete] => Array
	(
		[0] => violet
		[1] => purple
	)
)
*/
```
