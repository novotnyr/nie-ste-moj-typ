
Výrazy a funkcie
----------------

Vyššie sme spomínali, že výrazy si v Haskelli navždy zachovávajú ten istý typ.
Čo teda myslíme pod tým pojmom "výraz"? Výraz je vlastne všetko, čo sa dá
vyhodnotiť, pri čom výsledkom vyhodnocovania je vždy funkcia.

Zoberme si jednoduchý výraz:

    42

Zdalo by sa, že výsledkom vyhodnotenia je číslo 42, ale môžeme ho veľmi jednoducho
chápať ako *nulárnu funkciu*, ktorá neprijíma žiadne argumenty.
Vyhodnotenie výrazu je teda proste proces, ktorého výsledkom je vždy funkcia 
(lebo čísla, znaky, booleany, zoznamy a podobné základné entity chápeme ako
nulárne funkcie).

Treba však povedať, že výsledkom nemusí byť vždy nulárna funkcia: výsledkom
vyhodnocovania môže byť akákoľvek funkcia inej arity, a teda aj funkcia inej
arity.

Skúsme sa pozrieť na zložitejšie výrazy. Konkrétne, predstavme si Haskellovský program,
v ktorom je tento riadok:

    x = not True

kde `not` je *funkcia negácie*, ktorá vracia negáciu booleanu, ktorý dostane
ako argument.

Celý tento riadok kódu nie je výraz, pretože výrazy sa zo svojej podstaty musia
dať vyhodnotiť. Tento riadok je naopak definícia/pomenovanie funkcie a
v Haskelli sú toto akési špeciálne konštrukty, nie úplne potrebné pre beh
programu v Haskelli. Haskell počíta iba s výrazmi, vyhodnocuje jedine výrazy.
Pomenovanie funkcie je vlastne len pomôcka pre programátora, keďže sme schopní v princípe
sa všetkých pomenovaní v programe zbaviť a z celého programu spraviť len
jeden ozrutný výraz, v ktorom nami definované názvy/pomenovania funkcií budú
nahradené príslušnými telami funkcií. A tento ozrutný výraz už bude skutočne
výraz v tom zmysle, že sa bude dať priamo vyhodnotiť, a pri tom sa bude
správať takisto ako predošlý program, z ktorého sme vychádzali.

(Ozrutný výraz sa nedá až tak ľahko zostrojiť, ak máme nejaké funkcie
definované rekurzívne, tie sú špeciálne, lebo vyžadujú
pomenovanie na to, aby ich telo dávalo zmysel, ale tým sa teraz nebudeme
zaoberať.)

Späť k veci. Výrazy na tomto riadku sú teda tri:

(i).  samotné telo funkcie, `not True`: po vyhodnotení (vo viacerých skrytých
krokoch) dostaneme `False`, čo je nulárna funkcia;

(ii).  podvýraz výrazu (i): `not` sa vyhodnotí okamžite (bez akýchkoľvek krokov)
ako unárna funkcia identity;

(iii).  taktiež podvýraz výrazu (i): `True` sa okamžite vyhodnotí na nulárnu funkciu.

Keď sa nás teraz niekto opýta na akejkoľvek funkcie, tak sa najprv pozrieme
pozrieme na výraz predstavujúci telo funkcie, ten otypujeme a výsledok 
odovzdáme ako typ celej funkcie.

V príklade vyššie ak sa nás niekto opýta na typ `x`, tak sa pozrieme na telo
funkcie, čo je `not True`. Tu v tejto chvíli budeme postupovať intuitívne,
lebo vieme, že `not True` sa vyhodnotí na `False`, čo je boolean a teda typ
funkcie `x` celkovo je boolean. Na tejto procedúre je okrem iného zaujímavé aj
to, že my ako programátori typ ničoho špecifikovať nemusíme (ale môžeme) a samotný
kompilátor, resp. interpret Haskellu si dokáže typ našich funkcií sám odvodiť
v tom najvšeobecnejšom možnom tvare (nie vždy to však musí byť možné).

Je jasné, že takéto intuitívne poňatie nás nemôže uspokojiť, a tak sa nižšie
pozrieme na konkrétne typy funkcií (s ničím iným sa v Haskelli nestretneme)
a načrtneme si, ako presne odvodzujeme typy zložených výrazov. Z tejto časti
si teda predovšetkým odnesieme, že sú to práve výrazy, ktorým priraďujeme typy,
a ako sme si vysvetlili v minulej epizóde, dáva to zmysel, pretože žiaden výraz
počas behu programu nemôže zmeniť svoj typ.

Typy ako množiny
----------------

Ako sme si taktiež spomenuli v predošlom dieli seriálu, typy môžeme vnímať
ako akési špeciálne množiny. Čo sa základných entít ako čísel, znakov
a booleanov týka, tak Haskell je v tomto smere tak priamočiary, ako to je len
možné. Analogicky s jazykmi ako napr. C alebo Java, v Haskelli nachádzame typy
`Int`, `Char`, `Bool`, `Float`, `Double` s ich zvyčajným "obsahom".

Ako sme si povedali každý výraz v Haskelli si počas celého behu programu
zachováva stále ten istý typ. Pripomeňme si, ako sme vyššie odvodili, že `x` je
boolen, čo sa v Haskellovskej syntaxi zaznačí ako `x :: Bool` (matematicky by
sme písali *x* ∈ *Bool*).  S istotou teda vieme povedať, že `x :: Bool` bude
platiť v *akomkoľvek* bode vyhodnocovania programu. Vždy a všade.

Jediná nezrovnalosť by ešte mohla vzniknúť v tom, či nejaký výraz nemôže
patriť do viacerých typov. Alebo inak: fajn, môžeme vedieť, že `x :: Bool`,
ale znamená to nutne, že `x` teda nie je ani `Int` ani `Char`? V Haskelli tak
ako v mnohých iných programovacích jazykoch je odpoveď áno. Každá funkcia má
jednoznačne priradený jediný typ. Z toho teda vyplýva, že typy v Haskelli sú
*disjunktné*, čo znamená, že dva rôzne typy majú vždy prázdny prienik, nemajú
nikdy spoločný prvok -- alebo ešte inak povedané, že žiadna funkcia nikdy nepatrí
do dvoch rôznych typov zároveň.

Ako ale potom máme vnímať napríklad číslo `42`? Platí `42 :: Int` alebo `42 ::
Float` alebo snáď `42 :: Double`? Zdá sa rozumné, aby sme `42` v rôznych
kontextoch vnímali inak. A autori Haskellu nám dávajú za pravdu -- ak si totiž
otvoríme trebárs GHCi a tam použijeme príkaz interpretu `:t 42`, ktorý nám
vráti najvšeobecnejší typ výrazu `42`, tak dostaneme zvláštnu odpoveď:

  42 :: Num a => a

Malé písmená (`a`, `b`, `c`, ...) predstavujú v typoch tzv. *typové premenné*,
za ktoré možno dosadiť _vhodný_ typ (a vhodný je v tomto prípade iba
numerický). Výraz `42` má teda stále len jeden jediný typ, ale tento typ je
šikovne variabilný a môžeme ho skonkretizovať (dosadiť za premennú) podľa
potreby a kontextu. Typovým premenným sa budeme venovať v niektorom z ďalších dielov
nášho seriálu, tu si ich spomíname len pre zaujímavosť a preto, aby sme sa
náhodou nezľakli, ak si niekedy pomocou `:t` necháme otypovať naoko jednoduchý
výraz a dostaneme podobne záludnú odpoveď.

Najviac sa teda budeme venovať takým funkciám, s ktorými keď budeme pracovať,
tak sa vyhneme typovým premenným. To sú predovšetkým booleany, ktoré poznáme
dva (`True :: Bool` a `False :: Bool`), a znaky, ktoré uzatvárame nutne do jednoduchých
úvodzoviek (`'a' :: Char`, `'b' :: Char` etc.).

Nakoniec si ešte povedzme, že na základe týchto typov vieme sformovať
zložitejšie typy ako zoznamy a *n*-tice. Napríklad `[Bool]` je typ/množina
všetkých zoznamov booleanov. Zoznamom v Haskelli rozumieme takú postupnosť prvkov (potenciálne
aj nekonečnú!), ktorá obsahuje prvky len jedného, toho určeného typu. Takže
napr. platí, že `[True, False, False] :: [Bool]`, ale určite NIE `[True, 'a']
:: [Bool]`. Zoznamy obsahujú vždy funkcie výlučne daného, toho istého typu,
ale môžu mať premenlivú veľkosť. (Prázdny zoznam patrí do všetkých typov
zoznamov, t. j. `[] :: [a]`.)

Na druhú stranu, *n*-tice môžu obsahovať prvky rôznych typov, ale majú predom
danú dĺžku. Tak napríklad platí, že `(True, 'a') :: (Bool, Char)`, ale NIE
`(True, 'a', 'b') :: (Bool, Char)`. V oboch prípadoch totiž `(Bool, Char)`
označuje typ/množinu všetkých dvojíc, ktorých prvá komponenta dvojica je funkcia
typu `Bool` a druhá komponenta je funkcia typu `Char`. (Existuje aj
typ nultice `()`, ktorý obsahuje jedinú možnú nulticu `()`, t. j. `() :: ()`.)

V prípade zoznamov a *n*-tíc sa nenechajme zmiasť podobnou syntaxou na
tvorbu *prvkov* príslušného typu a na názov typu samotného. Tak napríklad
`['x']` je prvok (funkcia) typu `[Char]`, v oboch prípadoch používame hranaté
zátvorky, aby sme indikovali "zoznamovosť" (raz nulárnej funkcie, potom typu).

Typy funkcií vyšších arít
-------------------------

Všetky typy, o ktorých sme sa doteraz rozprávali, boli typy funkcií, lebo --
ako znie známa mantra -- všetko v Haskelli je funkcia. Všetko to boli ale
výlučne typy nulárnych funkcií, t. j. typy akýchsi základných hodnôt, ktoré už
netreba ďalej nijak vyhodnocovať a väčšinou práve ony sú to, čo očakávame ako
výstup programu, resp. výsledok výpočtu.

Ako ale vyzerajú typy ostatných funkcií -- tých funkcií, ktoré bežný človek
skutočne myslí pod pojmom "funkcia"? Používame k tomu šípku `->`. Tak
napríklad funkcia `not`, s ktorou sme sa stretli vyššie, je typu `Bool ->
Bool`, a teda `not :: Bool -> Bool`. Je to tak preto, že `not` je funkcia,
ktorá prijíma nejakú funkciu/hodnotu typu `Bool` ako argument (to je to prvé `Bool`
PRED šíkpou) a ak takúto funkciu/hodnotu skutočne prijme (hovoríme, že `not`
je na túto funkciu/hodnotu aplikovaná), tak potom vráti funkciu/hodnotu typu
`Bool` (to nám hovorí to druhé `Bool` ZA šípkou). Takýmto spôsobom nám typ
funkcie podáva akúsi čiastočnú informáciu o tom, ako `not` funguje, a to bez
toho, aby nás zavalil celým kódom (telom) funkcie `not`.

Formálne by sme vyššie povedané mohli demonštrovať aj takto:

1. Vieme, že `not :: Bool -> Bool`.
2. Nech `a :: Bool`.
3. Potom aplikácia `not a` nie je typovo chybný výraz. (V tom zmysle, že napr.
výraz `not 'z'` obsahuje typovú chybu, keďže `not` je aplikovaná na funkciu, ktorá nie je typu
`Bool`.)
4. Typ výsledku je nutne `not a :: Bool`. Toto vyplýva z toho, že keď je
nejaká funkcia aplikovaná na argument bez toho, aby vznikla typová chyba, tak
typ výsledku je taký ako typ tej funkcie s tým, že sa zmaže prvá šípka a
čokoľvek pred ňou. (To, čo je pred prvou šípkou v type funkcie je práve typ
prvého argumentu.)

Môžeme sa teraz skúsiť zamyslieť, či existuje ešte nejaká iná funkcia typu `Bool ->
Bool`. Túto otázku by sme mohli aj preformulovať: obsahuje množina/typ `Bool
-> Bool` aj nejaký iný prvok/funkciu okrem `not`? Odpoveď je, že áno:

  doubleNot b = not (not b)

Skúsme si odvodiť typ funkcie `doubleNot`: v podvýraze v zátvorke `(not b)` je
na `b` (čo je argument funkcie `doubleNot`) aplikovaná funkcia `not`. Funkcia
`not` je však typu `Bool -> Bool` a teda na to, aby sme sa vyhli typovej chybe (čo
určite chceme), musíme predpokladať, že `b` je typu `Bool`. V tejto chvíli už
vieme s istotou povedať, že `doubleNot` bude vyžadovať funkciu typu `Bool` ako
svoj prvý argument, teda že bude platiť niečo ako `doubleNot :: Bool -> ???`,
pri čom ešte nevieme, čo bude pod otáznikmi, ale ideme to rovno zistiť.

Pokračujme ďalej: Vieme, že typ podvýrazu `(not b)` je opäť `Bool` (viď
formálnu demonštráciu v štyroch krokoch o trochu vyššie). Celé telo funkcie
`not (not b)` je teda výraz bez typovej chyby (za predpokladu, že `b ::
Bool`). Zároveň tiež vieme (opäť na základe tej formálnej demonštrácie), že
`not (not b) :: Bool`, keďže `not b :: Bool`. Celkovo teda dostávame, že 
predpoklad `b :: Bool` nám zabráni typovej chybe a zároveň, že bude viesť
k celkovému výsledku typu `Bool`. Posledná veta sa v Haskelli vlastne zapíše
ako `doubleNot :: Bool -> Bool`.

Ukázali sme teda, že okrem samotnej `not` existuje aj iná funkcia typu `Bool
-> Bool` a teda, že typ/množina `Bool -> Bool` obsahuje aspoň dva rôzne prvky.
Prečo rôzne? Lebo napríklad `doubleNot True` sa vyhodnotí na `True`, zatiaľčo
`not True ~> False`. A teda `doubleNot` a `not` nemôžu byť ne-rôzne
(identické), lebo sa líšia vo výsledku, ktorý dávajú pre jeden ten istý
argument.

Takisto sme predviedli, akým približne spôsobom si kompilátor/interpret
Haskellu automaticky odvodzuje typ z funkcií, o ktorých má len čiastočnú typovú informáciu
(ako my sme začínali vlastne len s tým, že `not :: Bool -> Bool`).

--
TODO: funkcie vyššej arity (asociativita funkcie vs typy), curryfikované
vs uncurryfikované funkcie [prečo nepíšeme zátvorky okolo viacerých argumentov],
treba už splitnúť do ďalšieho dielu?
