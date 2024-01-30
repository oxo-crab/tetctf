### Stress Release Service

in this challenge, a link is provided http://192.53.173.71:8080/  as well as the source code was  give. In the folder there are `secret.php` and `index.js` .

`secret.js` has the flag in it 

```
<?php 

// Veryyyyyy Secretttttttttttt !!!!!!!!!!!!!!!!!
$FL4ggggggggggg = "TetCTF{Fake_FLAG}";

?>
```

and the `index.js` has the source code:

```
<br><center>
<font size=5 color=red >STRESS RELEASE SERVICE</font>
<br><br><br>
To relieve all your stress from the old year, all you need is SHOUTTTTTT!!!!
<br><br><br>
<form action="/" method="GET">
	<input type="submit" value="shout"/><input type="text" name="shout" value="@!@!@!@!@!@!@!@!" />
</form>
</center>

<?php

function validateInput($input) {
    // To make your shout effective, it shouldn't contain alphabets or numbers.
    $pattern = '/[a-z0-9]/i';
    if (preg_match($pattern, $input)) {
        return false;
    } 

    // and only a few characters. Let's make your shout clean.
	$count = count(array_count_values(str_split($input)));
	if ($count > 7) {
		return false;
	}

	return true;
}

if (isset($_GET["shout"]) && !empty($_GET["shout"]) && is_string($_GET["shout"])) {
	$voice = $_GET["shout"];
	$res = "<center><br><br><img src=\"https://i.imgur.com/SvbbT0W.png\" width=5% /> WRONGGGGG WAYYYYYY TOOOO RELEASEEEEE STRESSSSSSSS!!!!!!</center>";
	if(validateInput($voice) === true) {
		eval("\$res='<center><br><br><img src=\"https://i.imgur.com/TL6siVW.png\" width=5% /> ".$voice.".</center>';");
	}

	if (strlen($res) < 300) {
		echo $res;
	} else {
		echo "<center>Too loud!!! Please respect your neighbor.</center>";
	}
} 

?>
```

It was obvious that i had to fish around plenty of sites to look up on some php exploits

https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/php-tricks-esp

this was a pretty cool site where i could see the overview of some exploits.

I found that there's exploit related to `preg_match()` which filtered all our inputs forcing us to only enter special characters

![image](https://github.com/oxo-crab/tetctf/assets/111520157/04615f7c-737f-425b-b2cd-9f8f8badb988)

unfortunately it didn't work, scrolling down even more i found a possiblity of abusing xor : https://ctf-wiki.org/web/php/php/#preg_match

so i trued using it to produce `ls -la` and other commands. Another important thing to note is, we can only use 7 unique special characters, leading to an interesting rabbithole of phpfuk

Unfortunately i couldn't solve it in time.

After looking at some writeup i realised i was supposed use `readfile()` function from php to see the content of 'secret.php` 
and also had to wrap the readfile function like this : `.(readfile).(secret.php).' and replace the equivalent phpfuk of readfile and secret.php to actually evaluate it.

---








