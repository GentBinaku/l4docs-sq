# Kontributi për Laravel

- [Hyrje](#hyrje)
- [Kërkesat për Tërheqje](#pull-requests)
- [Coding Guidelines](#coding-guidelines)

<a name="hyrje"></a>
## Hyrje

Laravel është një software falas dhe me kod të hapur, që do të thotë se kushdo mund të kontribojë në zhvillimin dhe progresin e tij. Kodi i Laravel është aktualisht i ruajtur në [Github](http://github.com), i cili ofron metoda të lehta për `fork` të projektit dhe implementimin e kontributeve.

<a name="pull-requests"></a>
## Kërkesat për Tërheqje

Proçesi i kërkesave për tërheqje (pull requests) ndryshon për opsione të reja dhe probleme. Përpara se të dërgoni një kërkesë për një opsion të ri, fillimisht duhet të krijoni një "issue" në GitHub me `[Proposal]` në titull. Propozimi duhet të përshkruajë opsionin e ri, së bashku me implementimin e idesë. Më pas, propozimi do të shqyrtohet dhe mund të aprovohet apo refuzohet. Kur propozimi të jetë aprovuar, mund të krijohet një kërkesë tërheqjeje që implementon opsionin e ri. Kërkesat për tërheqje që nuk ndjekin udhëzimet, do të mbyllen menjëherë.

Kërkesat për tërheqje për probleme mund të dërgohen pa krijuar një "issue" propozuese. Nëse besoni se keni gjetur një zgjidhje për një problem egzistues të hapur në GitHub, mund të lini një koment me propozimin tuaj se si mund të rregullohet.

### Kërkesat për Opsione

Nëse keni një ide për opsion të ri që doni të shtohet në Laravel, mund të krijoni një "issue" në GitHub me `[Request]` në titull. Opsioni do të shqyrtohet nga një nga kontribuesit kryesorë.

<a name="coding-guidelines"></a>
## Udhëzimet e Kodimit

Laravel ndjek standartet e kodimit [PSR-0](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) dhe [PSR-1](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md). Krahas këtyre standarteve, më poshtë është një listë të disa udhëzimeve të kodimit që duhet të ndiqni:

- Deklarimi i namespaces duhet të jetë në një rresht me `<?php`.
- Hapja e klasës `{` duhet të jetë në një rresht me emrin e klasës.
- Hapjet e funksioneve dhe strukturave të kontrollit `{` duhet të jenë në një rresht.
- Emrat e ndërfaqeve duhet të mbarojnë me `Interface` (`FooInterface`).