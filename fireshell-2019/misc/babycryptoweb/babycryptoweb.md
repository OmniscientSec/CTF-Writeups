# babycryptoweb

## Prologue

Not a hard challenge, just required some playing around with the code

## Source code

```php
$code = '$kkk=5;$s="e1iwZaNolJeuqWiUp6pmo2iZlKKulJqjmKeupalmnmWjVrI=";$s=base64_decode($s);$res="";for($i=0,$j=strlen($s);$i<$j;$i++){$ch=substr($s,$i,1);$kch=substr($kkk,($i%strlen($kkk))-1,1);$ch=chr(ord($ch)+ord($kch));$res.=$ch;};echo $res;';
$a = $_GET['a'];
$b = $_GET['b'];
$code[$a] = $b;
eval($code);
```

## Analyzing the source code

```php
The $a and $b, $code[$a] = $b are red herrings.
$kkk=5;
$s="e1iwZaNolJeuqWiUp6pmo2iZlKKulJqjmKeupalmnmWjVrI=";
$s=base64_decode($s);
$res="";
for($i=0,$j=strlen($s);$i<$j;$i++){
	$ch=substr($s,$i,1);
	$kch=substr($kkk,($i%strlen($kkk))-1,1);
	$ch=chr(ord($ch)+ord($kch));
	$res.=$ch;
};
echo $res;
```

## Solution

Switch ```ord($ch) + ord($kch)``` to ```ord($ch) - ord($kch)``` and get the flag!

## Flag

```
F#{0n3_byt3_ru1n3d_my_encrypt1i0n!}
```

