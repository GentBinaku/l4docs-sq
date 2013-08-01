# Zhvillimi i Komandave për Artisan

- [Hyrje](#hyrje)
- [Ndërtimi i një Komande](#ndertimi-i-nje-komande)
- [Regjistrimi i Komandave](#regjistrimi-i-komandave)
- [Thërritja e Komandave të Tjera](#therritja-e-komandave-te-tjera)

<a name="hyrje"></a>
## Hyrje

Krahas komandave të ofruara me Artisan, mund të ndërtoni komandat tuaja të personalizuara për të thjeshtësuar veprime në aplikacione. Direktoria standarte për të ruajtur komandat është `app/commands`, por mund të përdorni çdo direktori që dëshironi, për aq kohës sa është në skemën e vetëngarkimit në `composer.json`.

<a name="ndertimi-i-nje-komande"></a>
## Ndërtimi i një Komande

### Gjenerimi i Klasës

Për të krijuar një komandë tuajën, përdorni komandën Artisan `command:make`, e cila gjeneron skeletin e klasës:

**Gjenero një Klasë për Komandën**

	php artisan command:make KomandaIme

Klasa e komandës do të ruhet në direktorinë `app/commands`, por mund edhe të përcaktoni direktorinë ose namespace:

	php artisan command:make KomandaIme --path="app/classes" --namespace="Classes"

### Shkrimi i Komandës

Pasi ta keni gjeneruar klasën e komandës, duhet të plotësoni tiparet `name` dhe `description` të klasës, që respektivisht vendosin emrin dhe përshkrimin e komandës, dhe do shfaqen në terminal kur të përdorni komandën `list`.

Metoda `fire` do të thërritet kur komanda të egzekutohet. Logjika e komandës mund të futet atje.

### Argumentat & Opsionet

Metodat `getArguments` dhe `getOptions` janë vendi ku përcaktoni argumentat dhe opsionet që komanda merr. Të dyja metodat duhet të kthejnë një vektor me komanda të përshkruara nga një listë me opsione.

Kur përcaktoni argumentat, vektori përkufizohet si më poshtë:

	array($emri, $menyra, $pershkrimi, $vleraBaze)

Argumenti `menyra` mund të jetë: `InputArgument::REQUIRED` ose `InputArgument::OPTIONAL`, për të vendosur nëse argumenti është i detyrueshëm (REQUIRED) ose opsional (OPTIONAL).

Kur përcaktoni opsionet, vektori përkufizohet si më poshtë:

	array($emri, $shkurtimi, $menyra, $pershkrimi, $vleraBaze)

Argumenti `menyra` mund të jetë: `InputOption::VALUE_REQUIRED`, `InputOption::VALUE_OPTIONAL`, `InputOption::VALUE_IS_ARRAY`, `InputOption::VALUE_NONE`, për të vendosur nëse opsioni është i detyrueshëm (VALUE_REQUIRED), opsional (VALUE_OPTIONAL), vektor (VALUE_IS_ARRAY) apo si çelës (VALUE_NONE).

`VALUE_IS_ARRAY` përcakton që opsioni mund të përdoret disa herë kur thërritet komanda:

	php artisan foo --option=nje --option=dy

`VALUE_NONE` përcakton që opsioni mund të përdoret si çelës.

	php artisan foo --option

### Marrja e Vlerave të Argumentave dhe Opsioneve

Kur komanda egzekutohet, do t'ju duhet të aksesoni vlerat e argumentave apo opsioneve që komanda pret. Për ta bërë këtë, mund të përdorni metodat `argument` dhe `option`:

**Marrja e Vlerës së një Argumenti**

	$vlera = $this->argument('emri');

**Marrja e Vlerave të të Gjithë Argumentave**

	$argumentat = $this->argument();

**Marrja e Vlerës së një Opsioni**

	$vlera = $this->option('emri');

**Marrja e Vlerave të të Gjitha Opsioneve**

	$options = $this->option();

### Shfaqja e Informacioneve në Terminal

Për të shfaqur informacione në terminal, mund të përdorni metodat `info`, `comment`, `question` dhe `error`. Të gjitha metodat printojnë me ngjyra ANSI.

**Shfaqja e Informacioneve**

	$this->info('Shfaqe ne ekran');

**Shfaqja e një Mesazhi Gabimi**

	$this->error('Ndodhi nje gabim!');

### Pyetjet

Gjithashtu mund të bëni pyetje me metodat `ask` dhe `confirm` të cilat plotësohen në terminal:

**Bërja e Pyetjes**

	$emri = $this->ask('Si e keni emrin?');

**Pyetje për Mesazhe Sekrete**

	$fjalekalimi = $this->secret('Cili eshte fjalekalimi?');

**Pyetja për Konfirmim**

	if ($this->confirm('Doni të vazhdoni? [po|jo]'))
	{
		// logjika
	}

Mund edhe të përcaktoni një vlerë bazë për metodën `confirm`, që duhet të jetë `true` ose `false`:

	$this->confirm($pyetja, true);

<a name="regjistrimi-i-komandave"></a>
## Regjistrimi i Komandave

Pasi ta keni mbaruar komandën, duhet ta regjistroni me Artisan që të bëhet e disponueshme. Kjo zakonisht kryhet në skedarin `app/start/artisan.php`. Brenda atij skedari duhet të përdorni metodën `Artisan::add` për ta regjistruar komandën:

**Regjistrimi i Komandës me Artisan**

	Artisan::add(new KomandaIme);

Nëse komanda është regjistruar në [Mbajtësin IoC](/docs/ioc), mund të përdorni metodën `Artisan::resolve` për ta bërë të disponueshëm tek Artisan:

**Regjistrimi i Komandës që Ndodhet në Mbajtësin IoC**

	Artisan::resolve('regjistrimi.emri');

<a name="therritja-e-komandave-te-tjera"></a>
## Thërritja e Komandave të Tjera

Ndonjëherë mund t'ju duhet të thërrisni komanda të tjera prej komandës tuaj. Mund ta bëni duke përdorur metodën `call`:

**Thërritja e një Komande Tjetër**

	$this->call('komanda.emri', array('argument' => 'nje', '--option' => 'dy'));
