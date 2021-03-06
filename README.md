# Bing Daily Photo

BingPhoto is a simple PHP class to fetch Bing's image of the day with meta data.

## Basic usage

```php
$bing = new BingPhoto();
$image = $bing->getImage();

// Example result ($image)
[
    [startdate] => '20160913'
    [fullstartdate] => '201609130700'
    [enddate] => '20160914'
    [url] => 'http://www.bing.com/az/hprichbg/rb/Meteora_EN-US6763889417_1920x1080.jpg'
    [urlbase] => '/az/hprichbg/rb/Meteora_EN-US6763889417'
    [copyright] => 'Roussanou and other monasteries in Metéora, Greece (© Stian Rekdal/Nimia)'   
    // ...
]
```

## Parameters

The class has some optional parameters to control various options:

| Parameter   |Description        |Default              |Valid values|
|-------------|-------------------|---------------------|------------|
| $date|Date of photo|`BingPhoto::DATE_TODAY` |`BingPhoto::DATE_YESTERDAY`, `BingPhoto::DATE_TODAY`, `BingPhoto::DATE_TOMORROW`, `any integer >= -1`|
| $n|Number of photos to fetch, going from date backwards|1|1 - 8|
| $locale     |Locale code|en-US|Whatever language Bing supports|
| $resolution |Image resolution|`BingPhoto::RESOLUTION_HIGH`|`BingPhoto::RESOLUTION_LOW`, `BingPhoto::RESOLUTION_HIGH`|

## Examples

```php
// Fetches two images of the day in high resolution from the American Bing portal
// Composer autoloader first
require __DIR__ . "/vendor/autoload.php";
// Here we go
use BingPhoto\BingPhoto;
$bing = new BingPhoto(BingPhoto::YESTERDAY, 2);
$images = $bing->getImages();
```

```php
// Fetches three images of the day in low resolution, starting yesterday from the French Bing portal
// Composer autoloader first
require __DIR__ . "/vendor/autoload.php";
// Here we go
use BingPhoto\BingPhoto;
$bing = new BingPhoto(BingPhoto::YESTERDAY, 3, 'fr-FR', BingPhoto::RESOLUTION_LOW);
foreach ($bing->getImages() as $image) {
    printf('<img src="%s">', $image['url']);
}
```

```php
// Output the image of the day as an image in high resolution
// Composer autoloader first
require __DIR__ . "/vendor/autoload.php";
// Here we go
use BingPhoto\BingPhoto;
$bing       = new BingPhoto(BingPhoto::TODAY, 1, 'de-DE', BingPhoto::RESOLUTION_HIGH);
$current    = $bing->getImage();
$info       = getimagesize($current['url']);

header("Content-type: " . $info["mime"]);
readfile($current['url']);
die();
```
