# Artisan CLI

- [Hyrje](#hyrje)
- [Përdorimi](#perdorimi)

<a name="hyrje"></a>
## Hyrje

Artisan është emri i ndërfaqes CLI të përfshirë me Laravel. Ofron disa komanda ndihmëse për përdorim gjatë zhvillimit të aplikacionit. Fuqizohet nga komponenti Symfony Console.

<a name="perdorimi"></a>
## Përdorimi

Për të parë një listë me të gjitha komandat e disponueshme, mund të përdorni komandën `list`:

**Listimi i të Gjitha Komandave**

	php artisan list

Çdo komandë ka një seksion ndihme e cila shfaq dhe përshkruan argumentet dhe opsionet e asaj komande. Për të parë ndihmën, thjeshtë shtoni pas komandës fjalën kyçe `help`:

**Shikimi i Ndihmës për një Komande**

	php artisan help migrate

Mundet gjithashtu të përcaktoni mjedisin ku komanda do egzekutohet duke përdorur çelësin `--env`:

**Përcaktimi i Mjedisit**

	php artisan migrate --env=local

Mund të shihni lehtësisht versionin aktual të Laravel duke përdorur opsionin `--version`:

**Shfaqja e Versionit Aktual të Laravel**

	php artisan --version
