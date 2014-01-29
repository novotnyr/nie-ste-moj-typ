Výrazy, funkcie a typy
======================

Vyššie sme spomínali, že výrazy si v Haskelli navždy zachovávajú ten istý typ.
Čo teda
myslíme pod tým pojmom "výraz"? Výraz je vlastne všetko, čo sa dá vyhodnotiť,
pri čom výsledkom vyhodnocovania je vždy funkcia.

Zoberme si jednoduchý výraz:

    42

Zdalo by sa, že výsledkom vyhodnotenia je číslo 42, ale môžeme ho veľmi jednoducho
chápať ako *nulárnu funkciu*, ktorá neprijíma žiadne argumenty.
Vyhodnotenie výrazu je teda proste proces, ktorého výsledkom je vždy funkcia 
(lebo čísla, znaky, booleany, zoznamy a podobné základné entity chápeme ako
nuklárne funkcie).

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

* https://xkcd.com/1312/
