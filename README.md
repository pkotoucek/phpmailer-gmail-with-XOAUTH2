# phpmailer-gmail-with-XOAUTH2
Optional settings for [primakurzy.cz](https://www.primakurzyonline.cz/): training with project PrimaKavarna

## 1. PURPOSE

Library Is For Educational Purposes Only Directly Connected With Project PrimaKavarna

## 2. INFO

As Google still changing rules, like example: [Google API Client Upgrade Guide](https://github.com/googleapis/google-api-php-client/blob/main/UPGRADING.md),
now (11.2024) this tutorial works, but if will stop working, you need to update procedure accordingly to changes!

## 3. PREREQ:

Create your project with [Google](https://github.com/PHPMailer/PHPMailer/wiki/Using-Gmail-with-XOAUTH2)
You will need complete your API, do not continue with token part from this link, as will be done in step6 bellow:
Make note about:
'clientId'
'client secret'
'redirecturi'

```php
$clientId = 'your-client-id.apps.googleusercontent.com';
$clientSecret = 'your-client-secret';
$redirectUri = 'set-more-urls-than-one';
```

Inputs for "$redirectUri ='';" created:
'http://localhost/primakavarna/get_oauth_token.php';
'http://localhost/primakavarna/send_gmail_token.php';
'http://localhost/primakavarna/send_gmail.php';

![image](https://github.com/user-attachments/assets/26bc727d-9618-4156-ab46-65ace484cf6e)

Warning:
In my case google project is set as "TEST", client ID / secret need to be refreshed every 7days to keep it alive!!

## 4. Installation:

The easiest way to install the library is using [Composer](https://getcomposer.org/):

```sh
composer require phpmailer/phpmailer
composer require league/oauth2-google
```

In my case I have "vendor" folder in root of primakavarna project!

## 5. Verify settings

Downloaded from [oauth2-google](https://github.com/thephpleague/oauth2-google)
And created send_gmail_token.php in root of primakavarna project!
If you are using Intelephense, fix error [intelephense(P1013)](https://github.com/bmewburn/vscode-intelephense/issues/1413) via disable Intelephense extension..
Via this code you will verify, if all is set correctly or not.
Must be changed:
'vendor'
'clientId'
'clientSecret'
'redirectUri'

```php

require 'vendor/autoload.php';

    'clientId'     => 'my-client-id.apps.googleusercontent.com',
    'clientSecret' => 'my-client-secret',
    'redirectUri'  => 'http://localhost/primakavarna/send_gmail_token.php'

```

=> run it.

You must be logged in browser with Google account, with which you created project in step3:

![image](https://github.com/user-attachments/assets/dcb3d18d-7524-4fdb-839f-e757467901e6)

Finally success:

![image](https://github.com/user-attachments/assets/ae463bb1-2339-4f38-ab8e-3c1bd34bb3e8)

## 6. Gain refresh token
Downloaded from [get_oauth_token)](https://github.com/PHPMailer/PHPMailer/blob/master/get_oauth_token.php)
And created get_oauth_token.php in root of primakavarna project!
Must be changed only '$redirectUri' part of this script:

```php
//$redirectUri = (isset($_SERVER['HTTPS']) ? 'https://' : 'http://') . $_SERVER['HTTP_HOST'] . $_SERVER['PHP_SELF'];
$redirectUri = 'http://localhost/primakavarna/get_oauth_token.php';
```

 => run it.

 Select provider, enter 'ID' and 'secret' previously used in step5:
 ![image](https://github.com/user-attachments/assets/4770e7bf-c2a2-48a6-8351-076f3fe930d0)

 Finally you have your refresh token!
 ![image](https://github.com/user-attachments/assets/16509090-d4fa-4b92-ac62-45529568a551)

## 7. Send email
Downloaded from [gmail_xoauth](https://github.com/PHPMailer/PHPMailer/blob/561609ac2ebae1b3ec1b636a38bf174d8de12955/examples/gmail_xoauth.phps)
And created send_gmail.php in root of primakavarna project!
Additional download [contentsutf8.html](https://github.com/PHPMailer/PHPMailer/blob/master/examples/contentsutf8.html)
And created contentsutf8.html in root of primakavarna project! (In my case I left it without change)
For send_gmail.php must be changed:
'vendor';
'email'
'clientId'
'clientSecret'
'redirectUri'
'refreshToken'

```php
require 'vendor/autoload.php';

$email = 'your-email@gmail.com';
$clientId = 'your-client-id.apps.googleusercontent.com';
$clientSecret = 'your-client-id';
$redirectUri = 'http://localhost/primakavarna/send_gmail.php';

$refreshToken = 'your-refresh-token';
```

 => run it.

![image](https://github.com/user-attachments/assets/97e732a8-38bc-43a9-9e96-cb2c0b52b999)

Success!!

![image](https://github.com/user-attachments/assets/652b79dd-f8d1-4bfd-b357-33e29c675fec)

## 8. Troubleshooting
If you are facing issue with error message with 'invalid_grant', in my case was connected with wrongly set '$redirectUri'.
But it can be connected with multiple another issues, as is mentioned in[invalid-grant-nightmare](https://blog.timekit.io/google-oauth-invalid-grant-nightmare-and-how-to-fix-it-9f4efaf1da35)

Example:

```php
2024-11-19 23:04:14 SERVER -> CLIENT: 220 smtp.gmail.com ESMTP a640c23a62f3a-aa20df263bcsm696223666b.11 - gsmtp
2024-11-19 23:04:14 CLIENT -> SERVER: EHLO localhost
2024-11-19 23:04:14 SERVER -> CLIENT: 250-smtp.gmail.com at your service, [178.209.137.107]250-SIZE 35882577250-8BITMIME250-AUTH LOGIN PLAIN XOAUTH2 PLAIN-CLIENTTOKEN OAUTHBEARER XOAUTH250-ENHANCEDSTATUSCODES250-PIPELINING250-CHUNKING250 SMTPUTF8
( ! ) Fatal error: Uncaught League\OAuth2\Client\Provider\Exception\IdentityProviderException: invalid_grant in
… => it was really long error, shortened here…
2024-11-19 23:04:15 CLIENT -> SERVER: QUIT
2024-11-19 23:04:15 SERVER -> CLIENT: 221 2.0.0 closing connection a640c23a62f3a-aa20df263bcsm696223666b.11
