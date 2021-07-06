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
