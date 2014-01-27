Motivácia typovať
=================

V prvom rade si ujasnime jednu vec: ak sa máme baviť o motivácii typovať a
kontrolovať typy, musíme v konečnom dôsledku otázku skonkretizovať na skúmanie motivácie _staticky_ typovať. Jazyky ako napr. JavaScript, ktoré sa na prvý
pohľad zdajú vonkoncom netypované, totiž môžeme chápať inak: v skutočnosti sú
typované, ale jestvuje v nich len jeden jediný typ. Otázka teda neznie, prečo
vôbec priraďovať entitám jazyka nejaké typy, ale či
tieto typy máme priraďovať (a následne aj tieto typové priradenia kontrolovať)
univerzálne nad všetkými entitami programu pred samotným behom programu
(statické typovanie)
alebo až individuálnym entitám, s ktorými sa manipuluje počas behu (dynamické typovanie).

Zoberme si jednoduchý program v Pythone (dynamicky typovanom jazyku):

    adresa = "Lunik"
    adresa = adresa + 9
    print adresa

V tomto prípade sa pred samotným behom programu (v čase kompilácie) typy vôbec nepriradzujú. V čase behu programu vieme povedať, že premenná `adresa` je v programe vždy typu *reťazec* a zhodou okolností to platí počas celého behu programu.

A vezmime si analogický program v Jave:

    public class StatickeTypovanie {
         public static void main(String []args){
                String adresa = "Lunik ";
                adresa = adresa + 9;
                System.out.println(adresa);
         }
    }

V ňom je exaktne povedané, že `adresa` je typu reťazec (`String`), a dokonca po pripočítaní čísla 9 (čo je celočíselná konštanta typu `int`) dostaneme `String` a to všetko ešte pred spustením programu.

Aj keď táto dilema už dlhé roky rozdeľuje programátorov do dvoch táborov, nedá sa tvrdiť, že by predstavovala nejakú krízu a bolo ju treba čo najskôr
definitívne vyriešiť. Dynamické vs. statické typovanie má každé svoje výhody.
Za výhodu dynamického programovania sa často uvádza priamočiarosť tvorby nových
programov (prototypov) a okamžitá testovateľnosť, na druhú stranu statické
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

Môžeme mať množinu ${2, 4, 6}$ alebo množinu párnych čísiel menších ako 8
alebo množinu prirodzených čísiel menších ako 7, od ktorej odčítame množinu ${1, 3, 5}$:
a to všetko popisuje rovnakú sadu prvkov. Na tomto základe by sme tak mohli 
pokojne ustanoviť typ *PárneČíslaMenšieAkoDesať*. Ak by sme chceli byť viac
praktickí, môžeme ustanoviť typ *Znak*, čo by sme mohli ustanoviť ako sadu
symbolov, ktoré vymenujeme podľa ASCII tabuľky.

Tvorba množín pomocou charakteristickej vlastnosti sa zdá byť prirodzená, ale
skrýva nejedno zákerné prekvapenie. Pán Bertrand Russell začiatkom 20. storočia objavil paradox, ktorý elegantne
poukazuje na hrôzy spôsobené nedostatočne obmedzenou voľnosťou tvorby množín.
Veď vezmime si podľa jeho príkladu množinu $R = {x | x \not\in x}$, teda
množinu takých množín, ktoré neobsahujú samé seba. Nič na prvý pohľad
nepoukazuje na to, že by taká množina mala byť nejak divná: jednoducho sme
charakteristickou podmienkou vymedzili, ktoré prvky nej patria.

No ale teraz uvažujme nad tým, či platí $R \in R$. Lebo ak áno, potom $R$
obsahuje samú seba, ale v podmienke príslušnosti do $R$ sme predsa povedali,
že do $R$ patria len tie množiny, ktoré neobsahujú samé seba. Takže náš
tip, že $R \in R$ platí, nemohol byť správny. Potom ale musí platiť $R \not\in
R$. Hm, ale to by znamenalo, že $R$ neobsahuje samú seba, čo je ale zase veľmi
škaredá situácia, lebo $R$ by mala obsahovať práve tie množiny, ktoré
neobsahujú samú seba! A teda $R \not\in R$ by malo proste viesť k tomu, že $R
\in R$, v čom je zrejme nejaký pes zakopaný.

Takto sme teda prišli na to, že množina ako $R$ nesmie existovať (iba ak by to
bola množina všetkých množín). Tým pádom teda chceme nejak obmedziť pravidlá
tvorby množín tak, aby sa množina v štýle $R$ nedala vytvoriť. Dalo by sa
povedať, že jednou z reakcií na tento problém bolo práve aj vytvorenie teórie
typov.

Typy sú ako riešenie Russellovho paradoxu užitočné preto, lebo
vytvárajú akúsi hierarchiu medzi typmi a ich obsahom. Najprv ustanovme
"najzákladnejšie" hodnoty -- napríklad čísla, znaky, booleany -- ktoré budeme volať typmi nultej
úrovne. Tieto potom môžeme združovať do typov prvej úrovne:
takže popri type *Číslo*, *Znak* a *Boolean* môžeme vytvoriť aj typ prvej úrovne 
???? s hodnotou ?????? {1, True}. (pozerajme sa v tejto chvíli na typy zjednodušene ako na množiny).
Dôležité je, že máme zakázané vytvoriť typ prvej úrovne {{1, True}, True}, lebo
typy prvej úrovne môžu obsahovať len typy nultej úrovne. Ak to zovšeobecníme, typy $n$-tej
úrovne môžu obsahovať len typy úrovní menšej ako $n$, čím predídeme Russelovmu paradoxu.
(Spomeňme si, že množina $R$, čiže akýsi typ $R$, mohol obsahovať množinu, 
teda opäť akýsi typ, tej istej úrovne.)

Výrazy, funkcie a typy
======================

Vyššie sme spomínali, že výrazy si v Haskelli navždy zachovávajú ten istý typ.
Čo teda
myslíme pod tým pojmom "výraz"? Výraz je vlastne všetko, čo sa dá vyhodnotiť,
pri čom výsledkom vyhodnocovania je vždy funkcia.

Zoberme si jednoduchý výraz:

    42

Zdalo by sa, že výsledkom vyhodnotenia je číslo (konštanta) 42, ale môžeme ho veľmi jednoducho
chápať ako *nulárnu funkciu*, ktorá neprijíma žiadne argumenty.
Ak zadefinujeme vyhodnotenie výrazu ako proces, ktorého výsledkom je vždy funkcia 
(a čísla budeme chápať ako funkcie), zjednodušíme a zelegantníme si potom niektoré úvahy.

Treba však povedať, že výsledkom nemusí byť vždy nulárna funkcia: výsledkom
vyhodnocovania môže byť aj funkcia inej arity -- ale príklady si zatiaľ necháme pre seba.

Vezmime si zložitejšie výrazy. Konkrétne, predstavme si Haskellovský program, v ktorom je tento riadok:

    x = id 42

Toto je príklad definície/pomenovania funkcie v Haskelli, $id$ je *funkcia identity*,
ktorá vždy vráti to, čo dostane na vstup.

Výrazy na tomto riadku sú tri:

(i).  `id 42`: po vyhodnotení (vo viacerých skrytých krokoch) dostaneme $42$, čo je nulárna funkcia.

(ii).  podvýraz výrazu (i): `id` sa vyhodnotí okamžite (bez akýchkoľvek krokov) ako unárna funkcia identity;

(iii).  taktiež podvýraz výrazu (i): `42` sa okamžite vyhodnotí na nulárnu funkciu.

Výraz `id 42` je telo funkcie
-- naozaj, ak by sme veľmi chceli mohli by sme tento riadok z programu zmazať
a všetky ostatné výskyty "x" v programe, ktoré sa odkazovali na túto
definíciu/pomenovanie by sme nahradili telom funkcie, teda výrazom "id 42",
správanie programu by zostalo také isté.

Keďže definície/pomenovania funkcií sú v tomto duchu vlastne len pomôckou pre
programátora (až na rekurzívne funkcie, tie sú špeciálne, lebo vyžadujú
pomenovanie na to, aby ich telo dávalo zmysel, uvidíme neskôr), čo nás bude
vždy pri otypovávaní zaujímať, bude vždy len telo funkcie. Ak sa opýtame na
typ názvu funkcie, tak vždy sa následne pozrieme na výraz predstavujúci telo
funkcie, a z neho následne odvodíme typ, ktorý bude aj typom názvu funkcie.

%TODO% priklad

Sú to teda práve výrazy, ktorým priraďujeme typy, a ako sme si vysvetlili
vyššie, dáva to zmysel, pretože žiaden výraz počas behu svojho programu nemôže
zmeniť svoj typ. Integer bude vždy integer a binárna funkcia berúca dva
booleany a dávajúca boolean zostane navždy funkciou s takýmto typom (napr. funkcia (&&)).

1. zoznam typov
2. urcenie prislusnosti do 'mnoziny'
