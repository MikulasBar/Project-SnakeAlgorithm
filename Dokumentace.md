# Algoritmus na dohrání hry Snake
## Cíl projektu
Cíl je vytořit samotnou hru Snake a propojit ji s algoritmem, který se bude snažit hru dohrát.<br>
Algoritmus není dokonalý a je velice malá šance na to, že hru nedohraje.

## Popis hry Snake
Hra Snake spočívá v tom, že na herní ploše, která se skládá ze čterců a tvoří tak mřížku, je umístěný 'had'. <br>
Každé pole v mřížce má nějakou z 3 barev. Černá značí prázdné pole, zelená značí hada a červená značí jablko. <br>
Při spustění hry se na herním poli objeví had, který je jen 3 pole dlouhý. <br>
Had se neustále plazí a zastaví se jen tehdy, když umře nebo když vyhraje hru. <br>
Na pole je také umístěno jablko, které když had sní, neboli se přesune na pozici jablka, zvětší se o jedno pole a na náhodném prázdném poli se objeví jablko. <br>
Hráč může hada ovládat šipkami a měnit jeho směr. V tomto programu je hra upravená tak, že hráč nemůže hada ovládat a had se bude pomocí algoritmu ovládat sám.<br>
Had umírá, když narazí do kraje pole nebo sám do sebe. <br>
Dohraná hra je tehdy, když had zaplní celou herní plochu a nezbyde tak žádné prázdné pole.<br>

## Popis použitých technologií
Program je napsaný v jazyce C# verze 11. Verze použitého DOTNETu je 7.0. <br>
Program používá šablonu WPF, která umožňuje lehčí stylizaci programu a vizualizaci hry pomocí XAML jazyku. <br>

## Popis algoritmu
Algoritmus ovládá hada tak, že mu na každém poli, na které had připlazí, podává instrukce o dalším směru, kterým se má had dále vydat. <br>
Algoritmus se dělí na 4 části : <br>

### Hamiltonovský cyklus
Hamiltonovský cyklus je pojem v topologii, který označuje graf, který lze projít tak, že každý uzel grafu je navštíven jen jednou, kromě startovního uzlu, který je zároven uzlem konečným. <br>
Tento pojem je perfektní pro dohrání Snaka v tom, že pokud had bude Hamiltonovský cyklus následovat, vždy sebere v cyklu jablko a nidky se nesrazí, protože jde jen jedním směrem. <br>
Tento cyklus je generovaný náhodně a je základní částí algoritmu.

#### Generace náhodného Cyklu
Za normálních podmínek je vytvořit takovýto cyklus velice těžké. Ovšem na tento program jen stačí, aby program generoval cyklus ve tvaru mřížky. <br>
Postup :
* nejprve se vytvoří graf s dvakrát menšími stranami než má graf původní. Vznikne tak graf jehož každý uzel pokrývá 4 uzle (2 x 2) z původního grafu
* pak se každý uzel grafu propojí horizontálně a vertikálně se sousedními uzly. Každá hrana grafu je náhodně ohodnocena přirozeným číslem
* poté se graf předělá na minimální kostru grafu
* cyklus se vytvoří tak, že had pojede kolem hran grafu, které zbyly, a bude zatáčet co nejvíce doprava, na každém pole rozdá směr, který odkazuje na další pole v cyklu  

#### Minimální kostra grafu
Minimální kostra grafu je graf, který obsahuje všechny uzly grafu ale jen některé hrany, tak aby celková hodnota grafu, vypočítaná ze součtu hodnot hran v grafu, byla co nejmenší <br>
Minimální kostra grafu je vytvořena pomocí Jarníkova algoritmu (Primova). <br>

#### Jarníkův algoritmus
* ze začátku je přidán startovní uzel do seznamu U
* pak se hledá uzel, který je napojen jen jednou hranou s co nejmenší hodnotou na jakýkoliv uzel již přidaný do U
* nalezený uzel se přidá do U, hrana která ho propojila se přidá do seznamu H
* hledání uzlu a hran se opakuje do té doby než jsou do S přidány všechny uzly
* výsledný graf je seznam uzlů U je jeho hran H
* 
### Path-Finding Algoritmus
Path-Finding agoritmus slouží k vyhledání nejkratší cesty v grafu od startovního uzlu do cílového uzlu. <br>
Použitý algoritmus se jmenuje A* (AStar). <br>
Program ho používá kvůli tomu, že je to jeden z nejspolehlivějších algoritmů. <br>

#### Popis A* algoritmu
Upozornění: Popis algoritmu se vztahuje k upravené verzi algoritmu, kterou program využívá. 
* algoritmus začíná se seznamem uzavřených uzlů ***C*** a otevřených uzlů ***O***
* algoritmus používá uzel ***t***, na začátku je uzel ***t*** roven startovnímu uzlu, přidá se do ***O*** a určí se mu hodnota ***F***
* hodnota ***F*** uzlu ***t*** je suma hodnot ***G*** a ***H***
* hodnota ***H*** uzlu ***t*** je suma rozdílu horizontálních souřadnic uzlu ***t*** a cílového uzlu a rozdílu vertikálních souřadnic uzlu ***t*** a cílového uzlu
* hodnota ***G*** uzlu ***t*** se vypočítá tímto způsobem :
  * nejprve je hodnota ***G*** rovna 0
  * uzel ***x*** je aktuální uzel, ze začátku je uzel ***x*** roven uzlu ***t*** 
  * uzel ***p*** uzlu ***x*** je takový uzel, na který odkazuje uzel ***x***, každý uzel tedy odkazuje na svůj uzel ***p*** 
  * uzel ***x*** se opakovaně přemisťuje na pozici svého uzlu ***p*** do té doby než je uzel ***x*** roven startovnímu uzlu
  * pokaždé když se uzel ***x*** stane uzlem ***p*** hodnota ***G*** stoupne o 1
* dokud se uzel ***t*** nerovná cílovému uzlu používá se cyklus :
  * z ***O*** se vybere uzel s nejmenší hodnotou ***F***, označí se jako uzel ***t***, odebere se z ***O*** a přidá se do ***C***
  * pro každého souseda ***s*** uzlu ***t*** platí : <br>
    \> pokud je již v ***C*** nebo je ***s*** neprůchodný, přeskočí se <br>
    \> pokud není v ***O*** nebo když je hodnota ***G*** uzlu ***p*** uzlu ***s*** větší než hodnota ***G*** uzlu ***t***, pak <br>
       &emsp;\-\> ***p*** uzlu ***s*** se rovná ***t*** <br>
    \> pokud ***s*** není v ***O***, pak <br>
       &emsp;\-\> ***s*** se přidá do ***O***
* poté se optimální cesta hledá způsobem :
  * seznam ***L*** je prázdný seznam
  * uzel ***d*** se rovná cílovému uzlu
  * dokud uzel ***d*** se nerovná startovnímu uzlu tak se opakuje : <br>
    \> uzel ***d*** se přidá do ***L*** <br>
    \> uzel ***d*** se přemístí na pozici svého uzlu ***p***
* seznam ***L*** je finální seznam všech klíčových pozicí v optimální cestě

### Popis Cyklu
Algoritmus se řídí podmínkami. <br>
Podle vyhodnocení pozice se algoritmus rozhodne co had udělá. <br>
Podmínky jsou následující : 
* 
