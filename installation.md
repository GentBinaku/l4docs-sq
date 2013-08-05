# Instalimi

- [Instalimi i Composer](#instalimi-i-composer)
- [Instalimi i Laravel](#instalimi-i-laravel)
- [Kërkesat e Serverit](#kerkesat-e-serverit)
- [Konfigurimi](#konfigurimi)
- [Pretty URLs](#pretty-urls)

<a name="instalimi-i-composer"></a>
## Instalimi i Composer

Laravel përdor [Composer](http://getcomposer.org) për ti menagjuar vartësitë e saj. Së pari, shkarkoni një kopje të `composer.phar`. Pasi që keni shkarkuar arkivën PHAR, ju ose mund ta mbani atë në ndonjë follder lokal, apo ta zhvendosni në `usr/local/bin` për të pasur mundësi ta përdorni në gjithë sistemin tuaj. Në sistemin operativ Windows, ju mund ta përdorni këtë [installer](https://getcomposer.org/Composer-Setup.exe).

<a name="instalimi-i-laravel"></a>
## Instalimi i Laravel

### Përmes Composer Create-Project

Ju mund ta instaloni Laravel-in duke shtypur komandën e Composer `create-project` në terminal:

	composer create-project laravel/laravel

### Përmes shkarkimit

Pasi që keni instaluar Composer, shkarkoni [verzionin e fundit](https://github.com/laravel/laravel/archive/master.zip) të Laravel dhe vendosini fajllat në një follder në serverin tuaj. Pastaj, në follderin kryesor të aplikacionit tuaj në Laravel, ekzekutoni komandën `php composer.phar install` (ose `composer install`) për ti instaluar të gjithë fajllat e nevojshëm. Për ta përfunduar me sukses këtë proces, Git duhet të jetë i instaluar në serverin tuaj.

Nëse doni ta përditësoni Laravel, thjeshtë ekzekutoni komandën `php composer.phar update`.

<a name="kerkesat-e-serverit"></a>
## Kërkesat e Serverit

Laravel ka disa kërkesa të serverit në mënyrë që të funksionojë si duhet:

- PHP >= 5.3.7
- MCrypt PHP Extension

<a name="konfigurimi"></a>
## Konfigurimi

Nëse mund të themi, Laravel-it nuk i duhet fare konfigurim. You jeni të lirshëm të filloni zhvillimit të aplikacioneve menjëherë. Gjithsesi, ndoshta do jeni të interesuar ti hidhni një sy fajllit `app/config/app.php` dhe dokumentimit të tij. Ky fajll përmban disa opcione si `timezone`dhe `locale` që ndoshta do ju duhet ti ndërroni për aplikacionin tuaj.

> **Shënim:** Një opcion i konfigurimit i cili nuk duhet të harrohet të rregullohet, është opcioni `key` në fajllin `app/config/app.php`. Kjo vlerë duhet të jetë një 'string' prej 32 shifrash. Ky opcion përdoret për kodim të vlerave, dhe ato vlera nuk do jenë të sigurta përderisa ky opcion nuk është rregulluar si duhet. Ju mund ta rregulloni këtë opcion thjeshtë dhe shpejtë duke përdorur komandën e artisan, `php artisan key:generate`.

<a name="permissions"></a>
### Autorizimet
Laravel kërkon disa autorizime nga ana e serverit - follderët e vendosur brenda app/storage duhet të kena qasje për shkruarje (write access).

<a name="paths"></a>
### Lokacionet

Shumica nga lokacionet e dosjeve të Laravel janë të ndryshueshme. Për ti ndryshuar ato lokacioni, shikojeni fajllin `bootstrap/paths.php`.

> **Shënim:** Laravel është krijuar për ta mbrojtur kodin e aplikacionit tuaj, dhe fajllat në pergjithësi, duke mos i lejuar të shfaqen në follderin kryesor përderisa nuk është e nevojshme. Preferohet që ose ta konfiguroni follderin kryesor si documentRoot, ose ti vendosni të gjitha fajllat e follderit kryesor në follderin kryesor të serverit tuaj, dhe pastaj të gjithë fajllat tjerë të Laravel jashtë atij follderi. 

<a name="pretty-urls"></a>
## Pretty URLs

Laravel përmban edhe fajllin `public/.htaccess` që shfrytëzohet për ta fshehur `index.php` nga URL-të. Nëse përdorni Apache në serverin tuaj, sigurohuni që ta aktivizoni modulin `mod_rewrite`.

Nëse fajlli `.htaccess` që vie bashkë me Laravel nuk funksionon me serverin tuaj me Apache, përdoreni këtë metodë:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
