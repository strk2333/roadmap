# PHP

W3C PHP

------

1.变量(弱语言)

   $xxx

2.Concatenation Operator

 .

3.strlen()

4.strpos()

5.if switch(略)

if (condition)

 code to be executed if condition is true;

elseif (condition)

 code to be executed if condition is true;

else

 code to be executed if condition is false;

6.数组

数值数组

   $names = array("Peter","Quagmire","Joe");

   $names[0] = "Peter";

   $names[1] = "Quagmire";

   $names[2] = "Joe";

关联数组

$ages = array("Peter"=>32, "Quagmire"=>30, "Joe"=>34);

$ages['Peter'] = "32";

$ages['Quagmire'] = "30";

$ages['Joe'] = "34";

多维数组

$families = array

(

 "Griffin"=>array

 (

 "Peter",

 "Lois",

 "Megan"

 ),

 "Quagmire"=>array

 (

 "Glenn"

 ),

 "Brown"=>array

 (

 "Cleveland",

 "Loretta",

 "Junior"

 )

);

7.'require_once('xxx.php');'

8.循环 while do...while for foreach

foreach (array as value)

{

 code to be executed;

}

9.$_GET 和 $_POST 数组

从带有 GET 方法的表单发送的信息，对任何人都是可见的（会显示在浏览器的地址栏），并且对发送的信息量也有限制（最多 100 个字符）

从带有 POST 方法的表单发送的信息，对任何人都是不可见的（会显示在浏览器的地址栏），并且对发送信息的量也没有限制。无法把页面加入书签.

PHP 的 $_REQUEST 变量包含了 $_GET, $_POST 以及 $_COOKIE 的内容。

10.include require(推荐)

include() 函数会生成一个警告（但是脚本会继续执行），而 require() 函数会生成一个致命错误（fatal error）（在错误发生后脚本会停止执行）。

11.文件读写

   fopen() r r+ w w+ a a+ x x+ (x创建)

   $file=fopen("welcome.txt","r") or exit("Unable to open file!");

   if (feof($file)) echo "End of file";

   fgets() 函数用于从文件中逐行读取文件。

   fgetc() 函数用于从文件逐字符地读取文件。

   \- $_FILES["file"]["name"] - 被上传文件的名称

   \- $_FILES["file"]["type"] - 被上传文件的类型

   \- $_FILES["file"]["size"] - 被上传文件的大小，以字节计

   \- $_FILES["file"]["tmp_name"] - 存储在服务器的文件的临时副本的名称

   \- $_FILES["file"]["error"] - 由文件上传导致的错误代码

   file_exists()   move_uploaded_file($_FILES["file"]["tmp_name"]), "newdir";

12.Cookie

   setcookie() 函数必须位于 <html> 标签之前。

   setcookie(name, value, expire, path, domain);

   setcookie("user", "Alex Porter", time()+3600);

   在发送 cookie 时，cookie 的值会自动进行 URL 编码，在取回时进行自动解码（为防止 URL 编码，请使用 setrawcookie() 取而代之）。

13.Session

   Session 的工作机制是：为每个访问者创建一个唯一的 id (UID)，并基于这个 UID 来存储变量。UID 存储在 cookie 中，亦或通过 URL 进行传导。

   在您把用户信息存储到 PHP session 中之前，首先必须启动会话。

   session_start() 函数必须位于 <html> 标签之前

   unset()  session_destroy()

14.Mail

   mail(**to, subject, message**, headers, parameters)

   $_REQUEST['email']

   

15.错误 异常

   die() 

   error_function(**error_level**, **error_message**, error_file,error_line, error_context);

   set_error_handler("customError");

   trigger_error(**"errmsg",** error_level );

2 E_WARNING 非致命的 run-time 错误。不暂停脚本执行。

8 E_NOTICE Run-time 通知。脚本发现可能有错误发生，但也可能在脚本正常运行时发生。

256 E_USER_ERROR 致命的用户生成的错误。这类似于程序员使用 PHP 函数 trigger_error() 设置的 E_ERROR。

512 E_USER_WARNING 非致命的用户生成的警告。这类似于程序员使用 PHP 函数 trigger_error() 设置的 E_WARNING。

1024 E_USER_NOTICE 用户生成的通知。这类似于程序员使用 PHP 函数 trigger_error() 设置的 E_NOTICE。

4096 E_RECOVERABLE_ERROR 可捕获的致命错误。类似 E_ERROR，但可被用户定义的处理程序捕获。(参见 set_error_handler())

8191 E_ALL 所有错误和警告，除级别 E_STRICT 以外。（在 PHP 6.0，E_STRICT 是 E_ALL 的一部分）

   error_log("Error: [$errno] $errstr", 1, "someone@example.com", "From: webmaster@example.com");

   class customException extends Exception

   set_exception_handler() 函数可设置处理所有未捕获异常的用户定义函数。

16.过滤器

   \- filter_var() - 通过一个指定的过滤器来过滤单一的变量

   \- filter_var_array() - 通过相同的或不同的过滤器来过滤多个变量

   \- filter_input - 获取一个输入变量, 并对它进行过滤

   \- filter_input_array - 获取多个输入变量, 并通过相同的或不同的过滤器对它们进行过滤

   Validating 过滤器：

用于验证用户输入

严格的格式规则（比如 URL 或 E-Mail 验证）

返回若成功预期的类型，否则返回 FALSE

   Sanitizing 过滤器：

用于允许或禁止字符串中指定的字符

无数据格式规则

始终返回字符串

选项 标志

选项必须放入一个名为 "options" 的相关数组中。如果使用标志，则不需在数组内 

   FILTER_CALLBACK

   filter_var($string, FILTER_CALLBACK,

array("options"=>"convertSpace"));

17.XML解析

Expat 基于事件

\- 通过 xml_parser_create() 函数初始化 XML 解析器

\- 创建配合不同事件处理程序的的函数

\- 添加 xml_set_element_handler() 函数来定义，当解析器遇到开始和结束标签时执行哪个函数

\- 添加 xml_set_character_data_handler() 函数来定义，当解析器遇到字符数据时执行哪个函数

\- 通过 xml_parse() 函数来解析文件 "test.xml"

\- 万一有错误的话，添加 xml_error_string() 函数把 XML 错误转换为文本说明

\- 调用 xml_parser_free() 函数来释放分配给 xml_parser_create() 函数的内存

  

   DOM 基于树

   $xmlDoc = new DOMDocument();

   $xmlDoc->load("note.xml");

   $x = $xmlDoc->documentElement;

   foreach ($x->childNodes AS $item)

   {

​    print $item->nodeName . " = " . $item->nodeValue . "<br />";

   }

   SimpleXML

   $xml = simplexml_load_file("test.xml");

   echo $xml->getName() . "<br />";

   foreach($xml->children() as $child)

   {

​    echo $child->getName() . ": " . $child . "<br />";

   }