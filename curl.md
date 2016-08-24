# PHP CURL POST & GET Examples

## Why we need PHP CURL ?

To send HTTP GET requests, simply we can use `file_get_contents()` method.

```php
file_get_contents('http://example.com')
```

But sending POST request and handling errors are not easy with `file_get_contents()`.

__step 1).__ Initialize CURL session

```php
$ch = curl_init();
```

__step 2).__ Provide options for the CURL session

```php
curl_setopt($ch, CURLOPT_URL, "http://example.com");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
// curl_setopt($ch, CURLOPT_HEADER, true); // if you want headers
```

* __CURLOPT_URL__ -> URL to fetch
* __CURLOPT_HEADER__ -> to include the header/not
* __CURLOPT_RETURNTRANSFER__ -> if it is set to true, data is returned as string instead of outputting it.

For full list of options, check this [PHP Documentation](http://php.net/curl_setopt).

__step 3).__ Execute the CURL session

```php
$output=curl_exec($ch);
```

__step 4).__ Close the session

```php
curl_close($ch);
```

__Note:__ You can check whether CURL enabled/not with the following code.

```php
if (is_callable('curl_init')) {
    echo "Enabled";
}
else
{
    echo "Not enabled";
}
```

## 1. PHP CURL GET EXAMPLE

You can use the below code to send GET request.

```php
function httpGet($url)
{
    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
//  curl_setopt($ch, CURLOPT_HEADER, false);

    $output = curl_exec($ch);

    curl_close($ch);
    return $output;
}

echo httpGet("http://example.com");
```

## 2. PHP CURL POST EXAMPLE

You can use the below code to submit form using PHP CURL.

```php
function httpPost($url,$params)
{
    $postData = '';
    // create name value pairs seperated by &
    foreach ($params as $k => $v)
    {
        $postData .= $k.'='.$v.'&';
    }
    $postData = rtrim($postData, '&');

    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HEADER, false);
    curl_setopt($ch, CURLOPT_POST, count($postData));
    curl_setopt($ch, CURLOPT_POSTFIELDS, $postData);
    // in real life you should use something like:
    // curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(array('postvar1' => 'value1')));

    $output = curl_exec($ch);

    curl_close($ch);
    return $output;
}
```

How to use the function:

```php
$params = array(
    "name" => "Ravishanker Kusuma",
    "age" => "32",
    "location" => "India"
);

echo httpPost("http://example.com/post.php", $params);
```

## 3. SEND RANDOM USER-AGENT IN THE REQUESTS

You can use the below function to get Random User-Agent.

```php
function getRandomUserAgent()
{
    $userAgents = array(
        "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-GB; rv:1.8.1.6) Gecko/20070725 Firefox/2.0.0.6",
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
        "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; .NET CLR 1.1.4322; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30)",
        "Opera/9.20 (Windows NT 6.0; U; en)",
        "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; en) Opera 8.50",
        "Mozilla/4.0 (compatible; MSIE 6.0; MSIE 5.5; Windows NT 5.1) Opera 7.02 [en]",
        "Mozilla/5.0 (Macintosh; U; PPC Mac OS X Mach-O; fr; rv:1.7) Gecko/20040624 Firefox/0.9",
        "Mozilla/5.0 (Macintosh; U; PPC Mac OS X; en) AppleWebKit/48 (like Gecko) Safari/48"
    );
    $random = rand(0, count($userAgents) - 1);

    return $userAgents[$random];
}
```

Using __CURLOPT_USERAGENT__, you can set User-Agent string.

```php
curl_setopt($ch, CURLOPT_USERAGENT, getRandomUserAgent());
```

## 4. HANDLE REDIRECTS (HTTP 301,302)

To handle URL redirects, set CURLOPT_FOLLOWLOCATION to TRUE.Maximum number of redirects can be controlled using CURLOPT_MAXREDIRS.

```php
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, TRUE);
curl_setopt($ch, CURLOPT_MAXREDIRS, 2); // only 2 redirects
```

## 5. HOW TO HANDLE CURL ERRORS

we can use `curl_errno()`, `curl_error()` methods, to get the last errors for the current session.

__curl_error($ch)__ -> returns error as string

__curl_errno($ch)__ -> returns error number

You can use the below code to handle errors.

```php
function httpGetWithErros($url)
{
    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $output = curl_exec($ch);

    if ($output === false)
    {
        echo "Error Number:".curl_errno($ch)."<br>";
        echo "Error String:".curl_error($ch);
    }

    curl_close($ch);

    return $output;
}
```

For the full list of errors, refer [CURL errors](https://curl.haxx.se/libcurl/c/libcurl-errors.html)
