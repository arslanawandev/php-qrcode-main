# arslanawan/php-qrcode


## Overview

**Attention:** there is now also a javascript port: [chillerlan/js-qrcode](https://github.com/chillerlan/js-qrcode).

### Features

- Creation of [Model 2 QR Codes](https://www.qrcode.com/en/codes/model12.html), [Version 1 to 40](https://www.qrcode.com/en/about/version.html)
- [ECC Levels](https://www.qrcode.com/en/about/error_correction.html) L/M/Q/H supported
- Mixed mode support (encoding modes can be combined within a QR symbol). Supported modes:
  - numeric
  - alphanumeric
  - 8-bit binary
  - 13-bit double-byte:
    - kanji (Japanese, Shift-JIS)
    - hanzi (simplified Chinese, GB2312/GB18030) as [defined in GBT18284-2000](https://www.chinesestandard.net/PDF/English.aspx/GBT18284-2000)
- Flexible, easily extensible output modules, built-in support for the following output formats:
  - [GdImage](https://www.php.net/manual/book.image)
  - [ImageMagick](https://www.php.net/manual/book.imagick)
  - Markup types: SVG, HTML, etc.
  - String types: JSON, plain text, etc.
  - Encapsulated Postscript (EPS)
  - PDF via [FPDF](https://github.com/setasign/fpdf)
- QR Code reader (via GD and ImageMagick)


### Requirements

- PHP 7.4+
  - [`ext-mbstring`](https://www.php.net/manual/book.mbstring.php)
  - optional:
    - [`ext-fileinfo`](https://www.php.net/manual/book.fileinfo.php) (required by `QRImagick` output)
    - [`ext-gd`](https://www.php.net/manual/book.image)
    - [`ext-imagick`](https://github.com/Imagick/imagick) with [ImageMagick](https://imagemagick.org) installed
    - [`setasign/fpdf`](https://github.com/setasign/fpdf) for the PDF output module

For the QRCode reader, either `ext-gd` or `ext-imagick` is required!


me)


## Installation with [composer](https://getcomposer.org)


### Terminal

```
composer require chillerlan/php-qrcode
```


### composer.json

```json
{
	"require": {
		"php": "^7.4 || ^8.0",
		"chillerlan/php-qrcode": "dev-main#<commit_hash>"
	}
}
```

Note: replace `dev-main` with a [version constraint](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints).


## Quickstart

We want to encode this URI for a mobile authenticator into a QRcode image:

```php
$data = 'otpauth://totp/test?secret=B3JX4VCVJDVNXNZ5&issuer=chillerlan.net';

// quick and simple:
echo '<img src="'.(new QRCode)->render($data).'" alt="QR Code" />';
```



<p align="center">
	<img alt="QR codes are awesome!" style="width: auto; height: 530px;" src="https://raw.githubusercontent.com/chillerlan/php-qrcode/main/.github/images/example.svg">
</p>


### Reading QR Codes

Using the built-in QR Code reader is pretty straight-forward:

```php
// it's generally a good idea to wrap the reader in a try/catch block because it WILL throw eventually
try{
	$result = (new QRCode)->readFromFile('path/to/file.png'); // -> DecoderResult

	// you can now use the result instance...
	$content = $result->data;
	$matrix  = $result->getMatrix(); // -> QRMatrix

	// ...or simply cast it to string to get the content:
	$content = (string)$result;
}
catch(Throwable $e){
	// oopsies!
}
```





## Disclaimer!

I don't take responsibility for molten CPUs, misled applications, failed log-ins etc.. Use at your own risk!
