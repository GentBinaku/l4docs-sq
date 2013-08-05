# Konfigurimi

- [Hyrje](#hyrje)
- [Konfigurimi i Mjedisit](#konfigurimi-mjedisit)
- [Statusi i Mirëmbajtjes](#statusi-mirembajtjes)

<a name="hyrje"></a>
## Hyrje

Të gjithë skedarët e konfigurimit të Laravel ruhen në direktorinë `app/config`. Çdo opsion në të gjithë skedarët është i dokumentuar, kështu që sigurohuni ti lexoni dhe të bëheni familjar me opsionet në dispozicion.

Ndonjëherë mund t'ju duhet ti aksesioni opsionet e konfigurimit gjatë ekzekutimit të aplikacionit. Këtë mund ta bëni përmes klasës `Config`: 

**Aksesimi i një Vlere Konfigurimi**

	Config::get('app.timezone');

Mund të vendosni gjithashtu vlerën bazë që do të kthehen nëse opsioni i konfigurimit nuk egziston:

	$timezone = Config::get('app.timezone', 'UTC');

Vini re sintaksën më "pikë", e cila përdoret për të aksesuar vlera në skedarë të ndryshëm. Pjesa e parë përcakton skedarin ("app"), ndërsa pjesa e dytë opsionin ("timezone").

Vlerat e konfigurimit mund edhe të vendosen gjatë ekzekutimit:

**Vendosja e një Vlere Konfigurimi**

	Config::set('database.default', 'sqlite');

Vlerat e konfigurimit të vendosura gjatë ekzekutimit vlejnë vetëm për kërkesën aktuale dhe nuk do të mbarten për kërkesa të tjera. Për vendosje definitive vlerash, duhet të ndryshoni skedarët e konfigurimit.

<a name="konfigurimi-mjedisit"></a>
## Konfigurimi i Mjedisit

Shpesh ndihmon të keni disa konfigurime në bazë të mjedisit që aplikacioni po ekzekutohet. Për shembull, mund t'ju duhet të përdorni një cache driver të ndryshëm në mjedis lokal dhë një tjetër në serverin e produksionit. Kjo arrihet lehtë duke përdorur konfigurim të bazuar në mjedise.

Fillimisht krijoni një direktori brenda `config` që përputhet me emrin e mjedisit; për shembull: `local`. Më pas krijoni skedarin e konfigurimit që doni të mbivendosni për atë mjedis. Për shembull, për të mbivendosur cache driver-in për mjedisin lokal, duhet të krijoni një skedar `cache.php` në `app/config/local` me përmbajtjen më poshtë:

	<?php

	return array(

		'driver' => 'file',

	);

> **Shënim:** Mos përdorni 'testing' si emër mjedisi sepse është i rezervuar për unit testing.

Kini parasysh që nuk ju duhet të shkruani çdo opsion që ndodhet në skedarin bazë të konfigurimit, por vetëm ato opsione që doni ti mbivendosni. Sistemi i konfigurimit punon me një sistem "ujëvarë", ku lexohen të gjitha skedarët e konfigurimit dhe mbivendosen vetëm ato të përcaktuara në konfigurimin e mjedisit.

Hapi tjetër është ti tregojmë Laravel se cilat janë kushtet për të përcaktuar mjedisin. Mjedisi bazë është gjithmonë `production`. Megjithatë, mund të krijoni mjedise të tjera brenda skedarit `bootstrap/start.php`. Në këtë skedar do të gjeni një thirrje `$app->detectEnvironment`. Vektori që i kalohet asaj metode përcakton mjedisin aktual. Mund të shtoni mjedise dhe emra kompjuteri të tjerë në atë vektor.

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('emri-i-kompjuterit'),

    ));

Mundet gjithashtu ti kaloni një funksion anonim metodës `detectEnvironment`, ku mund të implementoni detektimin tuaj të mjedisit:

	$env = $app->detectEnvironment(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

Mjedisin aktual të aplikacionit mund ta detektoni përmes metodës `environment`:

**Aksesimi i Mjedisit Aktual të Aplikacionit**

	$mjedisi = App::environment();

<a name="statusi-mirembajtjes"></a>
## Statusi i Mirëmbajtjes

Kur aplikacioni është në statusin e mirëmbajtjes, të route-at e aplikacionit do të çojnë drejt një pamjeje të personalizuar. Kjo e bën të lehtë çaktivizimin e aplikacionit ndërkohë që është duke u përditësuar apo punuar në të. Një thirrje e metodës `App::down` ndodhet në skedarin `app/start/global.php`. Përgjigja e kësaj metode do të dërgohet tek vizitorët gjatë statusit të mirëmbajtjes.

Për të aktivizuar statusin e mirëmbajtjes, ekzekutoni komandën `down` në terminal:

	php artisan down

Për ta çaktivizuar statusin e mirëmbajtjes, ekzekutoni komandën `up` në terminal:

	php artisan up

Për të shfaqur një pamje të personalizuar gjatë kohës së mirëmbajtjes, mund të shtoni një kod si më poshtë në skedarin `app/start/global.php`:

	App::down(function()
	{
		return Response::view('mirembajtje', array(), 503);
	});
