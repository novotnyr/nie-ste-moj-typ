Typovať či netypovať? To je tá otázka!
======================================

TODO: Alef0 úvodný obkec, príp. úvod do celého seriálu?

No, nie až tak úplne *tá* otázka
--------------------------------

Na rovinu si ujasnime jednu vec: ak sa máme baviť o motivácii typovať a
kontrolovať typy, musíme v konečnom dôsledku otázku skonkretizovať na skúmanie motivácie _staticky_ typovať. Jazyky ako napr. JavaScript, ktoré sa na prvý
pohľad zdajú vonkoncom netypované, totiž môžeme chápať inak: v skutočnosti sú
typované, ale jestvuje v nich len jeden jediný typ, čo samozrejme znamená, že
v istom zmysle aj tieto jazyky _sú_ typované.

Otázka teda neznie, prečo
vôbec priraďovať entitám jazyka nejaké typy, ale či
tieto typy máme priraďovať (a následne aj tieto typové priradenia kontrolovať)
univerzálne nad všetkými entitami programu pred samotným behom programu
(statické typovanie)
alebo až individuálnym entitám, s ktorými sa manipuluje počas behu (dynamické typovanie).

Aj keď táto dilema už dlhé roky rozdeľuje programátorov do dvoch táborov, nedá sa tvrdiť, že by predstavovala nejakú krízu a bolo ju treba čo najskôr
definitívne vyriešiť. Dynamické vs. statické typovanie má každé svoje výhody.
Za výhodu dynamického programovania sa často uvádza priamočiarosť tvorby nových
programov (prototypov) a okamžitá testovateľnosť. Na druhú stranu statické
typovanie má snáď navrch, čo sa garancie korektného fungovania programov týka
-- je zrejmé, že statická kontrola typov pokrýva nemalú časť prípadov
zahrnutých v jednotkových testoch programov s dynamickou kontrolou typov. Ako ďalšie
výhody statického typovania by sme mohli spomenúť vyššiu výpočtovú
efektivitu a dostupnosť nástrojov automatickej analýzy kódu, hoci v týchto
bodoch sa už nedá posúdiť vo všeobecnosti, lebo rozdiely sa celkom stierajú.

Konkrétne v prípade Haskellu je však voľba statického typovania zvlášť
rozumná. Keďže ide o deklaratívny/funkcionálny programovací jazyk, nie sú
v ňom žiadne vedľajšie efekty (až na také prízemné úkony ako vstup-výstupné
operácie a spol.) a teda každý výraz má zaručene vždy ten istý typ od začiatku
až do konca behu programu. Takýto prístup si vyslovene pýta, aby priradenie
typov a ich kontrola prebehla jednorazovo, staticky.

[![Haskell](https://sslimgs.xkcd.com/comics/haskell.png)](https://xkcd.com/1312/)

(Ešte upozornime, že statické typovanie nie je to isté ako explicitné
typovanie, ktoré znamená, že programátor sám musí špecifikovať, akého typu je
tá-ktorá premenná, funkcia a pod. Ako uvidíme práve v Haskelli, staticky sa dá typovať aj bez
tohto explicitného typovania -- Haskell sám si totiž vie odvodiť
najvšeobecnejšie typy funkcií, ktoré v programe definujeme.)

Trošku matematiky a histórie (nie nutne v tomto poradí)
-------------------------------------------------------

Nesmieme ale takisto zabúdať, že Haskell je jazyk, ktorý v očiach mnohých má
k matematickému formalizmu bližšie, ako sa na riadny programovací jazyk patrí. 
Preto isto nebude na škodu si uviesť popri vyššie uvedených pragmatických dôvodoch 
aj nejakú peknú matematickú motiváciu zavádzať typy. V matematike sa totiž
veľmi často pracuje s množinami, ktoré typy v mnohom pripomínajú, 
ale oproti jasne danému obsahu a štruktúre typov a pravidlám na
tvorbu nových typov sú až príliš "voľné". (Ako uvidíme neskôr, 
Haskell je na pochopenie týchto pravidiel mimoriadne vhodný.)

Ešte od čias maturity vieme, že množiny môžeme určiť buď vymenovaním prvkov, 
alebo charakteristickou vlastnosťou, alebo operáciou s inou množinou.
Môžeme mať množinu {2, 4, 6} alebo množinu párnych čísiel menších ako 8
alebo množinu prirodzených čísiel menších ako 7, od ktorej odčítame množinu {1, 3, 5}:
a to všetko popisuje rovnakú sadu prvkov.

Tvorba množín pomocou charakteristickej vlastnosti sa zdá byť prirodzená, ale
skrýva nejedno zákerné prekvapenie. Pán Bertrand Russell začiatkom 20. storočia objavil paradox, ktorý elegantne
poukazuje na hrôzy spôsobené nedostatočne obmedzenou voľnosťou tvorby množín.
Veď vezmime si podľa jeho príkladu množinu *R* = {*X* | *X* ∉ *X*}, teda
množinu takých množín, ktoré neobsahujú samé seba. Nič na prvý pohľad
nepoukazuje na to, že by taká množina mala byť nejak divná: jednoducho sme
charakteristickou podmienkou vymedzili, ktoré prvky nej patria.

No ale teraz uvažujme nad tým, či platí *R* ∈ *R*. Lebo ak áno, potom *R*
obsahuje samú seba, ale v podmienke príslušnosti do *R* sme predsa povedali,
že do *R* patria len tie množiny, ktoré neobsahujú samé seba. Takže náš
tip, že *R* ∈ *R$ platí, nemohol byť správny. Potom ale musí platiť *R*
∉ *R*. Hm, ale to by znamenalo, že $R$ neobsahuje samú seba, čo je ale zase veľmi
škaredá situácia, lebo $R$ by mala obsahovať práve tie množiny, ktoré
neobsahujú samú seba! A teda *R* ∉ *R* by malo proste viesť k tomu, že
*R* ∈ *R*. Celkovo platí, že *R* ∈ *R* vtedy a len vtedy, keď *R* ∉ *R*,
v čom musí byť nejaký pes zakopaný.

(Podobné paradoxy môžeme formulovať aj v bežnom jazyku. Trebárs je asi
celkom jasné, že holiči holia iba takých mužov, ktorí sa neholia sami.
Ale ak zároveň máme v dedine len jedného holiča, Fera, tak všetkých mužov
z dediny, ktorí sa sami neholia, holí práve Fero. Otázka vedúca k podobnému
paradoxu ako s množinou *R* potom bude: Holí sa Fero sám, alebo nie?)

Takto sme teda prišli na to, že množina ako *R* nesmie existovať.
Tým pádom teda chceme nejak obmedziť pravidlá
tvorby množín tak, aby sa množina v štýle *R* nedala vytvoriť. Dalo by sa
povedať, že jednou z reakcií na tento problém bolo aj vytvorenie teórie
typov.

Typy sú v zásade také špeciálne množiny, medzi ktorými je zavedená hierarchia.
Najprv si vezmeme "najzákladnejšie" primitívne hodnoty: čísla,
znaky, booleany a spol., pre ktoré už máme intuitívne pochopenie.
Tieto budú náš odrazový mostík, budú to typy nultej úrovne.
(Ako to? Veď tieto primitívne hodnoty nie sú množiny a práve sme
povedali, že typy sú špeciálnymi prípadmi množín, huh? No, ide len o to, že
v matematike vieme každú entitu zakódovať ako množinu, podobne ako na počítači vieme
všetko zakódovať do postupnosti bitov. To berme ako fakt.)

Typy nultej úrovne vieme potom ale združiť do množiny. A tieto množiny,
obsahujúce výlučne typy nultej úrovne, sa nazývajú typy prvej úrovne.
Samozrejme, medzi typy prvej úrovne budú patriť prirodzené typy ako
*Číslo* (obsahujúci iba čísla), *Znak* (iba znaky) a *Boolean* (iba booleany).
Popri týchto prirodzených typoch prvej úrovne sme schopní korektne vytvoriť aj
iné, napr. typ {1, True}, ktorý obsahuje ako aj číslo, tak aj boolean. Ale oba
prvky tejto množiny sú sami o sebe typy nultej úrovne a teda táto množina
obsahuje výlučne typy nultej úrovne a práve preto aj {1, True} je typ prvej
úrovne. Oplatí sa upozorniť aj na to, že množina *VšetkyTypyNultejÚrovne* je
tiež typ prvej úrovne.

Zovšeobecňujúc, príklad typu druhej úrovne môže byť, povedzme, množina
{*Znak*, {1, True}}. O *Znak* ako i o {1, True} sme si vysvetlili, že ide
o typy prvej úrovne. Keďže teda {*Znak*, {1, True}} je množina obsahujúca iba
typy prvej úrovne, môžeme túto množinu nazvať typom druhej úrovne. Na druhú
stranu, množina {'a', {1, True}} nie je typ druhej úrovne, keďže obsahuje aj
typ nultej úrovne, aj typ prvej úrovne. Na to, aby akákoľvek množina mohla byť
typom druhej úrovne, smie obsahovať jedine typy prvej úrovne a nie zmes typov
nižších úrovní.

(Nakoniec si ešte všimnime, že prázdna množina je typom akejkoľvek úrovne
okrem nultej.
Toto približne odpovedá hodnote `undefined` z Haskellu, ktorá je v tomto
zmysle tiež prvkom každého typu. Ale to už trošku predbiehame...)

Snáď už teraz teda máme akési uchopenie onej hierarchie, ktorá je typom
vlastná. Vo všeobecnosti typy úrovne *n* sú také množiny, ktoré obsahujú
výlučne typy úrovne *n-1* za svoje prvky. Ako ale tieto typy, t. j. hierarchiou zviazané
množiny, adresujú problém Russellovho paradoxu? Celkom jednoducho: ak
uvažujeme typy ako jediné možné množiny, tak pýtať sa, či množina obsahuje
samú seba, je jednoducho nezmyselné, lebo samotná množina je vždy o úroveň
vyššie ako prvky, ktoré obsahuje (a teda nemôže samú seba obsahovať).

Food for thought
================
* Názorne vysvetlené pojmy statického a dynamického typovania (a ešte ďalej):
  https://pythonconquerstheuniverse.wordpress.com/2009/10/03/static-vs-dynamic-typing-of-programming-languages/
* Trošičočku viac teoreticky o súčasnom stave diskusie o typových systémoch:
  http://blogs.perl.org/users/ovid/2010/08/what-to-know-before-debating-type-systems.html
