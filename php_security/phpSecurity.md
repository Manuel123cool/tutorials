### PHP security collection
I am not an expert, just somemone how collects security vulnerability!

#### Timing attack

String compare operators return immediately if the length is not the same:
So you could see if one length takes longer than other, based on that you can get the length. The string compare operator return immediately if the a character is wrong, so based on that: you can get the character that takes the longest and that this is the right character.

```
if ($_POST["password"] === "Password") {
    return true;
}
```
Even if the time difference is so little and ther is network jitter: why risk it. Use hash_equals($known_string, $user_string), which doesnt leak time difference (only for getting the length).
```
if (hash_equals("Password", $_POST["password"]) {
    return true;
}
```

[Link on topic](https://blog.ircmaxell.com/2014/11/its-all-about-time.html)
