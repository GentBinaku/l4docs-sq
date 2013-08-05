# Instalimi

- [Instalimi i Composer](#instalimi-i-composer)
- [Instalimi i Laravel](#instalimi-i-laravel)
- [Kërkesat e Serverit](#kerkesat-e-serverit)
- [Konfigurimi](#konfigurimi)
- [URL të Bukura](#url-te-bukura)

<a name="instalimi-i-composer"></a>
## Instalimi i Composer

Laravel përdor [Composer](http://getcomposer.org) për ti menaxhuar vartësitë e saj. Së pari, shkarkoni një kopje të `composer.phar`. Pasi ta keni shkarkuar arkivën PHAR, mund ta mbani atë në ndonjë direktori lokale, apo ta zhvendosni në `usr/local/bin` për të pasur mundësi ta përdorni në të gjithë sistemin. Në sistemin operativ Windows, mund të përdorni këtë [instalues automatik](https://getcomposer.org/Composer-Setup.exe).

<a name="instalimi-i-laravel"></a>
## Instalimi i Laravel

### Përmes Composer Create-Project

Mund ta instaloni Laravel duke shtypur komandën e Composer `create-project` në terminal:

	composer create-project laravel/laravel

### Përmes shkarkimit

Pasi ta keni instaluar Composer, shkarkoni [versionin e fundit](https://github.com/laravel/laravel/archive/master.zip) të Laravel dhe vendosini skedarët në një direktori në serverin tuaj. Më pas, në direktorinë kryesore të Laravel, ekzekutoni komandën `php composer.phar install` (ose `composer install`) për ti instaluar të gjithë skedarët e nevojshëm. Për ta përfunduar me sukses këtë proçes, Git duhet të jetë i instaluar në serverin tuaj.

Nëse doni ta përditësoni Laravel, thjeshtë ekzekutoni komandën `php composer.phar update`.

<a name="kerkesat-e-serverit"></a>
## Kërkesat e Serverit

Laravel ka disa kërkesa të serverit në mënyrë që të funksionojë si duhet:

- PHP >= 5.3.7
- MCrypt PHP Extension

<a name="konfigurimi"></a>
## Konfigurimi

Nëse mund të themi, Laravel-it nuk i duhet fare konfigurim. Jeni të lirshëm të filloni zhvillimin e aplikacioneve menjëherë. Gjithsesi, ndoshta do jeni të interesuar ti hidhni një sy skedarit `app/config/app.php` dhe dokumentimit të tij. Ky skedar përmban disa opsione si `timezone`dhe `locale` që ndoshta do ju duhet ti ndërroni për aplikacionin tuaj.

> **Shënim:** Një opsion i konfigurimit i cili nuk duhet të harrohet është `key` në skedarin `app/config/app.php`. Kjo vlerë duhet të jetë një 'string' prej 32 karakteresh. Ky opsion përdoret për kriptim të vlerave dhe ato vlera nuk do jenë të sigurta për aq kohë sa ky është vendosur. Mund ta rregulloni thjeshtë dhe shpejtë duke përdorur komandën e artisan, `php artisan key:generate`.

<a name="permissions"></a>
### Autorizimet
Laravel kërkon disa autorizime nga ana e serverit - direktoritë e vendosur brenda `app/storage` duhet të kenë akses për shkruarje (write access).

<a name="paths"></a>
### Lokacionet

Shumica e lokacioneve dhe direktorive të Laravel janë të ndryshueshme. Për ti ndryshuar shikoni skedarin `bootstrap/paths.php`.

> **Shënim:** Laravel është krijuar për ta mbrojtur kodin e aplikacionit tuaj, dhe skedarët në pergjithësi, duke mos i lejuar të shfaqen publikisht për aq kohë sa nuk është e nevojshme. Preferohet që ta konfiguroni direktorinë publike si `documentRoot` ose ti vendosni të gjithë skedarët publikë në direktorinë publike të serverit dhe ti lini jashtë saj skedarët e tjerë të Laravel.

<a name="url-te-bukura"></a>
## URL të Bukura

Laravel përmban edhe skedarin `public/.htaccess` që shfrytëzohet për ta fshehur `index.php` nga URL-të. Nëse përdorni Apache në serverin tuaj, sigurohuni që ta aktivizoni modulin `mod_rewrite`.

Nëse skedari `.htaccess` që vjen bashkë me Laravel nuk funksionon në serverin tuaj me Apache, përdoreni këtë metodë:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
