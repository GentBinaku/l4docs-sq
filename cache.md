# Cache

- [Konfigurimi](#konfigurimi)
- [Përdorimi i Cache](#perdorimi-cache)
- [Rritja & Zvogëlimi](#rritja-dhe-zvogelimi)
- [Seksionet e Cache](#seksionet-e-cache)
- [Cache në Databazë](#cache-ne-databaze)

<a name="konfigurimi"></a>
## Konfigurimi

Laravel ofron një API të unifikuar për sisteme të ndryshme cache. Konfigurimi i cache ndodhet në `app/config/cache.php`. Në këtë skedar mund të përcaktoni cilin driver doni të përdorni. Laravel mbështet teknologji popullore për caching si [Memcached](http://memcached.org) dhe [Redis](http://redis.io).

Skedari i konfigurimit të cache mban gjithashtu opsione të tjera, të dokumentuara brenda skedari, kështu që sigurohuni ta lexoni. Laravel vjen me `file` cache driver të paravendosur, i cili i ruan objektet e serializuar në skedarë. Për aplikacione të mëdha, rekomandohet të përdorni cache memorje si Memcached apo APC.

<a name="perdorimi-cache"></a>
## Përdorimi i Cache

**Ruajtja në Cache**

	Cache::put('celesi', 'vlera', $minutat);

**Ruajtja në Cache nëse Çelësi nuk Egziston**

	Cache::add('celesi', 'vlera', $minutat);

**Kontrolli për Egzistencën e një Çelësi**

	if (Cache::has('celesi'))
	{
		//
	}

**Marrja nga Cache**

	$vlera = Cache::get('celesi');

**Marrja e një Vlere ose Kthimi i një Vlere Bazë**

	$vlera = Cache::get('celesi', 'vlera baze');

	$vlera = Cache::get('celesi', function() { return 'vlera baze'; });

**Ruajtja në Cache Përgjithmonë**

	Cache::forever('celesi', 'vlera');

Ndonjëherë mund t'ju duhet të merrni një vlerë nga cache, por gjithashtu të ruani një vlerë bazë nëse çelësi nuk egziston. Mund ta bëni këtë duke përdorur metodën `Cache::remember`:

	$vlera = Cache::remember('perdoruesit', $minutat, function()
	{
		return DB::table('perdoruesit')->get();
	});

Mund edhe të kombinoni metodat `remember` dhe `forever`:

	$vlera = Cache::rememberForever('perdoruesit', function()
	{
		return DB::table('perdoruesit')->get();
	});

Të gjitha vlerat e ruajtura në cache serializohen më parë, kështu që mund të përdorni çdo tip të dhëne që dëshironi.

**Fshirja e një Çelësi nga Cache**

	Cache::forget('celesi');

<a name="rritja-dhe-zvogelimi"></a>
## Rritja & Zvogëlimi

Të gjitha driver-at përveç `file` dhe `database` lejojnë përdorimin e `increment` dhe `decrement`, respektivisht për të zmadhuar dhe zvogëluar lehtë numrat.

**Rritja e një Vlere**

	Cache::increment('celesi');

	Cache::increment('celesi', $sasia);

**Zvogëlimi i një Vlere**

	Cache::decrement('celesi');

	Cache::decrement('celesi', $sasia);

<a name="seksionet-e-cache"></a>
## Seksionet e Cache

> **Shënim:** Seksionet e cache nuk mbështeten nga driver-at `file` ose `database`.

Seksionet e cache ju lejojnë të gruponi vlera të lidhura me njëra tjetrën dhe pastrimin e të gjithave më një veprim. Për të aksesuar një seksion, përdorni metodën `section`:

**Aksesimi i një Seksioni**

	Cache::section('njerezit')->put('Beni', $beni);

	Cache::section('njerezit')->put('Arjana', $arjana);

Gjithashtu mund të aksesoni çelësa individualë ose të përdorni metoda të tjera si `increment` dhe `decrement`:

**Aksesimi i Vlerave Individuale në një Seksion**

	$arjana = Cache::section('njerezit')->get('Arjana');

Në fund, mund ti pastroni të gjitha vlerat në një seksion:

	Cache::section('njerezit')->flush();

<a name="cache-ne-databaze"></a>
## Cache në Databazë

Nëse vendosni të përdorni driver-in `database`, do t'ju duhet të ndërtoni një tabelë në databazë për të mbajtur vlerat e cache. Më poshtë është një skemë shembull për tabelën:

	Schema::create('cache', function($table)
	{
		$table->string('key')->unique();
		$table->text('value');
		$table->integer('expiration');
	});
