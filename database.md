# Fillimi me Bazat e të Dhënave

- [Konfigurimi](#konfigurimi)
- [Ekzekutimi i Query-ve](#ekzekutimi-query)
- [Transaksionet](#transaksionet)
- [Aksesimi i Lidhjeve](#aksesimi-lidhjeve)
- [Logimi i Query-ve](#logimi-query)

<a name="konfigurimi"></a>
## Konfigurimi

Laravel e bën lidhjen me bazat e të dhënave dhe ekzekutimin e query-ve shumë të thjeshtë. Skedari i konfigurimit të bazës së të dhënave është `app/config/database.php`. Në këtë skedar mund të përcaktoni të gjitha lidhjet dhe një të paravendosur. Shembuj për të gjitha sistemet e përkrahura ndohen në po atë skedar.

Aktualisht, Laravel përkrah katër sisteme: MySQL, Postgres, SQLite dhe SQL Server.

<a name="ekzekutimi-query"></a>
## Ekzekutimi i Query-ve

Pasi të keni konfiguruar lidhjen me bazën e të dhënave, mund të ekzekutoni query duke përdorur klasën `DB`.

**Ekzekutimi i një Query SELECT**

	$results = DB::select('select * from users where id = ?', array(1));

Metoda `select` do të kthejë gjithmonë një `vektor` me rezultate.

**Ekzekutimi i një Query INSERT**

	DB::insert('insert into users (id, name) values (?, ?)', array(1, 'Dayle'));

**Ekzekutimi i një Query UPDATE**

	DB::update('update users set votes = 100 where name = ?', array('John'));

**Ekzekutimi i një Query DELETE**

	DB::delete('delete from users');

> **Note:** Query-t `update` dhe `delete` kthejne numrin e rreshtave të prekur nga veprimi.

**Ekzekutimi i një Query të Përgjithshme**

	DB::statement('drop table users');

Mund të "dëgjoni" për evente të bazës së të dhënave duke përdorur metodën `DB::listen`:

**Dëgjimi për Evente**

	DB::listen(function($sql, $bindings, $time)
	{
		//
	});

<a name="transaksionet"></a>
## Transaksionet

Për të ekzekutuar disa veprime brenda një transaksioni të vetëm, mund të përdorni metodën `transaction`:

	DB::transaction(function()
	{
		DB::table('users')->update(array('votes' => 1));

		DB::table('posts')->delete();
	});

<a name="aksesimi-lidhjeve"></a>
## Aksesimi i Lidhjeve

Kur përdorni më shumë se një lidje, mund ti aksesoni përmes metodës `DB::connection`:

	$users = DB::connection('foo')->select(...);

Gjithashtu, mund të merrni instancën PDO:

	$pdo = DB::connection()->getPdo();

Ndonjëhere mund t'ju duhet të rilidheni me një bazë të dhënash:

	DB::reconnect('foo');

<a name="logimi-query"></a>
## Logimi i Query-ve

Laravel i ruan në memorje të gjitha query-t e ekzekutuara për kërkesën aktuale. Megjithatë, në disa raste, si në futjen e një numri të madh rreshtash, kjo mund të bëjë që aplikacioni të përdorë më shumë se memorja në dispozicion. Për të çaktivizuar logimin, mund të përdorni metodën `disableQueryLog`:

	DB::connection()->disableQueryLog();