# Cache

- [Konfigurimi](#konfigurimi)
- [Përdorimi i Cache](#perdorimi-cache)
- [Rritja & Zvogëlimi](#rritja-dhe-zvogelimi)
- [Seksionet e Cache](#seksionet-e-cache)
- [Cache në Databazë](#cache-ne-databaze)

<a name="konfigurimi"></a>
## Konfigurimi

Laravel ofron një API të unifikuar për sisteme të ndryshme cache. Konfigurimi i cache ndodhet në `app/config/cache.php`. Në këtë skedar mund të përcaktoni cilin driver doni të përdorni. Laravel mbështet teknologji popullore për caching si [Memcached](http://memcached.org) dhe [Redis](http://redis.io).

Skedari i konfigurimit të cache mban gjithashtu opsione të tjera, të dokumentuara brenda skedarit, kështu që sigurohuni ta lexoni. Laravel vjen me `file` cache driver të paravendosur, i cili i ruan objektet e serializuar në skedarë. Për aplikacione të mëdha, rekomandohet të përdorni cache memorje si Memcached apo APC.

<a name="perdorimi-cache"></a>
## Përdorimi i Cache

**Ruajtja në Cache**

	Cache::put('key', 'value', $minutes);

**Ruajtja në Cache nëse Çelësi nuk Egziston**

	Cache::add('key', 'value', $minutes);

**Kontrolli për Egzistencën e një Çelësi**

	if (Cache::has('key'))
	{
		//
	}

**Marrja nga Cache**

	$value = Cache::get('key');

**Marrja e një Vlere ose Kthimi i një Vlere Bazë**

	$value = Cache::get('key', 'default');

	$value = Cache::get('key', function() { return 'default'; });

**Ruajtja në Cache Përgjithmonë**

	Cache::forever('key', 'value');

Ndonjëherë mund t'ju duhet të merrni një vlerë nga cache, por gjithashtu të ruani një vlerë bazë nëse çelësi nuk egziston. Mund ta bëni këtë duke përdorur metodën `Cache::remember`:

	$value = Cache::remember('users', $minutes, function()
	{
		return DB::table('users')->get();
	});

Mund edhe të kombinoni metodat `remember` dhe `forever`:

	$value = Cache::rememberForever('users', function()
	{
		return DB::table('users')->get();
	});

Të gjitha vlerat e ruajtura në cache serializohen më parë, kështu që mund të përdorni çdo tip të dhëne që dëshironi.

**Fshirja e një Çelësi nga Cache**

	Cache::forget('key');

<a name="rritja-dhe-zvogelimi"></a>
## Rritja & Zvogëlimi

Të gjitha driver-at përveç `file` dhe `database` lejojnë përdorimin e `increment` dhe `decrement`, respektivisht për të zmadhuar dhe zvogëluar lehtë numrat.

**Rritja e një Vlere**

	Cache::increment('key');

	Cache::increment('key', $amount);

**Zvogëlimi i një Vlere**

	Cache::decrement('key');

	Cache::decrement('key', $amount);

<a name="seksionet-e-cache"></a>
## Seksionet e Cache

> **Shënim:** Seksionet e cache nuk mbështeten nga driver-at `file` ose `database`.

Seksionet e cache ju lejojnë të gruponi vlera të lidhura me njëra tjetrën dhe pastrimin e të gjithave më një veprim. Për të aksesuar një seksion, përdorni metodën `section`:

**Aksesimi i një Seksioni**

	Cache::section('people')->put('John', $john);

	Cache::section('people')->put('Anne', $anne);

Gjithashtu mund të aksesoni çelësa individualë ose të përdorni metoda të tjera si `increment` dhe `decrement`:

**Aksesimi i Vlerave Individuale në një Seksion**

	$anne = Cache::section('people')->get('Anne');

Në fund, mund ti pastroni të gjitha vlerat në një seksion:

	Cache::section('people')->flush();

<a name="cache-ne-databaze"></a>
## Cache në Databazë

Nëse vendosni të përdorni driver-in `database`, do t'ju duhet të ndërtoni një tabelë në databazë për të mbajtur vlerat e cache. Më poshtë është një skemë shembull për tabelën:

	Schema::create('cache', function($table)
	{
		$table->string('key')->unique();
		$table->text('value');
		$table->integer('expiration');
	});
