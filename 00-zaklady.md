== Motivácia typovať ==

V prvom rade si ujasnime jednu vec: ak sa máme baviť o motivácii typovať a
kontrolovať typy,
musíme v konečnom dôsledku otázku skonkretizovať na skúmanie motivácie
_staticky_ typovať. Jazyky ako napr. Javascript, ktoré sa na prvý
pohľad zdajú vonkoncom netypované totiž môžeme chápať inak: totiž, že sú
typované, ale jestvuje v nich len jeden jediný typ. Otázka teda neznie, prečo
vôbec priraďovať entitám jazyka nejaké typy, ale či
tieto typy máme priraďovať (a následne aj tieto typové priradenia kontrolovať)
univerzálne nad všetkými entitami programu pred samotným behom programu
(statické typovanie)
alebo až individuálnym entitám, s ktorými sa manipuluje počas behu (dynamické typovanie).

Táto dilema už dlhé roky rozdeľuje programátorov do dvoch táborov, nedá sa
však tvrdiť, že by predstavovala nejakú krízu a bolo ju treba čo najskôr
definitívne vyriešiť. Dynamické vs. statické typovanie má každé svoje výhody.
Za výhodu dynamického programovania sa často uvádza priamočiarosť tvorby nových
programov (prototypov) a okamžitá testovateľnosť, na druhú stranu statické
typovanie má snáď navrch, čo sa garancie korektného fungovania programov týka
-- je zrejmé, že statická kontrola typov pokrýva nemalú časť prípadov
zahrnutých v unit testoch programov s dynamickou kontrolou typov. Ako ďaľšie
výhody statického typovania by sme mohli spomenúť vyššiu výpočetnú
efektivitu a dostupnosť nástrojov automatickej analýzy kódu, hoci v týchto
bodoch sa už nedá posúdiť vo všeobecnosti, lebo rozdiely sa celkom stierajú.

Konkrétne v prípade Haskellu je však voľba statického typovania zvlášť
rozumná. Keďže ide o deklaratívny/funkcionálny programovací jazyk, nie sú
v ňom žiadne vedľajšie efekty (až na také prízemné úkony ako vstup-výstupné
operácie a spol.) a teda každý výraz má zaručene vždy ten istý typ od začiatku
až do konca behu programu. Takýto prístup si vyslovene pýta, aby priradenie
typov a ich kontrola prebehla jednorázovo, staticky.

Nesmieme ale takisto zabúdať, že Haskell je jazyk, ktorý v očiach mnohých má
k matematickému formalizmu bližšie,
ako sa na riadny programovací jazyk patrí. Preto isto nebude na škodu si
uviesť popri vyššie uvedených pragmatických dôvodoch aj nejakú peknú
matematickú motiváciu zavádzať typy. V matematike sa totiž veľmi často pracuje
s množinami, ktoré typy v mnohom pripomínajú, ale množiny sa zdajú akoby až
príliš "voľné" oproti jasne danému obsahu a štruktúre typov a
pravidlám na tvorbu nových typov (a Haskell je na pochopenie týchto pravidiel
veľmi vhodný, uvidíme nižšie).

Pán Bertrand Russell začiatkom 20. storočia objavil paradox, ktorý elegantne
poukazuje na hrôzy spôsbené nedostatočne obmedzenou voľnosťou tvorby množín.
Veď vezmime si podľa jeho príkladu množinu $R = {x | x \not\in x}$, teda
množinu takých množín, ktoré neobsahujú samé seba. Nič na prvý pohľad
nepoukazuje na to, že by taká množina mala byť nejak divná, proste sme
podmienkou vymedzili, ktoré prvky do množiny patria, pri čom žiadne iné
tam nepatria.

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
tvorby množín tak, aby sa množina na štýl $R$ nedala vytvoriť. Dalo by sa
povedať, že jednou z reakcií na tento problém bolo práve aj vytvorenie teórie
typov.

Typy sú ako riešenie Russellovho paradoxu užitočné preto, lebo
vytvárajú akúsi hierarchiu medzi typmi a ich obsahom. Sú nejaké najviac
základné hodnoty (čísla, znaky, booleany a pod.) -- toto budú typy nultej
úrovne. Tieto typy nultej úrovne môžeme združovať do
typov prvej úrovne: prirodzene budeme mať tendenciu vytvoriť typ čísel, znakov
a booleanov, ale zároveň môžeme urobiť aj typ prvej úrovne {1, True}
(pozerajme sa v tejto chvíli na typy zjednodušene ako na množiny). Pointa
je však v tom, že nemôžeme urobiť typ prvej úrovne {{1, True}, True}, lebo
typy prvej úrovne môžu obsahovať len typy nultej úrovne. A takto vo
všeobecnosti môžu typy n-tej úrovne obsahovať len typy úrovní menšej ako n.
Tým sa vyhneme Russellovmu paradoxu, ktorý stavia na možnosti vytvoriť typ,
ktorý obsahuje typ tej istej úrovne.

== Výrazy, funkcie a typy ==

Vyššie sme spomínali, že výrazy si v Haskelli navždy zachovávajú ten istý typ.
Čo teda
myslíme pod tým pojmom "výraz"? Výraz je vlastne všetko, čo sa dá vyhodnotiť,
pri čom výsledkom vyhodnocovania je vždy funkcia (zvyčajne to je nulárna
funkcia, teda funkcia, ktorá neprijíma žiadne argumenty, napr. nejaká čiselná
konštanta; nemusí to však byť vždy nulárna funkcia, na to, aby išlo o výraz
nám stačí akákoľvek funkcia).

Konkrétne, predstavme si Haskellovský program, v ktorom je tento riadok:

  x = id 42

Toto je príklad definície/pomenovania funkcie v Haskelli, $id$ je funkcia,
ktorá vždy vráti to, čo dostane.
Výrazy na tomto riadku sú tri:

(i) "id 42", po vyhodnotení (v jednom kroku) tohto výrazu dostaneme $42$, čo je funkcia (nulárna);
(ii) podvýraz výrazu (i): "id", vyhodnotí sa okamžite (bez akýchkoľvek krokov) ako unárna funkcia identity;
(iii) taktiež podvýraz výrazu (i): "42", okamžite, nulárna funkcia.

Výraz "id 42" je telo funkcie
-- naozaj, ak by sme veľmi chceli mohli by sme tento riadok z programu zmazať
a všetky ostatné výskyty "x" v programe, ktoré sa odkazovali na túto
definíciu/pomenovanie by sme nahradili proste telom funkcie, výrazom "id 42",
a správanie programu by zostalo také isté.

Keďže definície/pomenovania funkcií sú v tomto duchu vlastne len pomôckou pre
programátora (až na rekurzívne funkcie, tie sú špeciálne, lebo vyžadujú
pomenovanie na to, aby ich telo dávalo zmysel, uvidíme neskôr), čo nás bude
vždy pri otypovávaní zaujímať, bude vždy len telo funkcie. Ak sa opýtame na
typ názvu funkcie, tak vždy sa následne pozrieme na výraz predstavujúci telo
funkcie, a z neho následne odvodíme typ, ktorý bude aj typom názvu funkcie.

Sú to teda práve výrazy, ktorým priraďujeme typy, a ako sme si vysvetlili
vyššie, dáva to zmysel, pretože žiaden výraz počas behu svojho programu nemôže
zmeniť svoj typ. Integer bude vždy integer a binárna funkcia berúca dva
booleany a dávajúca boolean zostane navždy funkciou s takýmto typom (napr. funkcia (&&)).

1. zoznam typov
2. urcenie prislusnosti do 'mnoziny'
