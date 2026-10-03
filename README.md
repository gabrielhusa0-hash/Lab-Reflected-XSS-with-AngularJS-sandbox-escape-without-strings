# Lab: Reflected XSS with AngularJS sandbox escape without strings

## Postup
Tohle byla celkem rychlovka. Celé to šlo vyřešit jedním tahem – stačilo vzít upravený payload a zkopírovat ho přímo do URL vyhledávacího pole nahoře na stránce:

/?search=1&toString().constructor.prototype.charAt%3D[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1

Jakmile jsem to tam poslal, stránka to zpracovala a lab se hned vyřešil.

## Proč to fungovalo?
Stránka v sobě měla AngularJS, který na pozadí zkoumal a vyhodnocoval to, co jsem napsal do vyhledávání (konkrétně přes filtr `orderBy`). 

Háček byl v tom, že laboratoř blokovala uvozovky, takže jsem nemohl normálně napsat text v uvozovkách (proto „without strings“). Musel jsem to obejít chytře:
- Využil jsem `toString().constructor`, abych se dostal k funkcím JavaScriptu bez použití zakázaných uvozovek.
- Pomocí `fromCharCode` jsem si ASCII kódy poskládal kód pro `alert(1)` pomocí čísel.
- AngularJS tenhle výraz vyhodnotil, úspěšně utekl ze svého vězení (sandbox escape) a rovnou spustil můj kód v prohlížeči.
