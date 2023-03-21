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
Program je napsaný v jazyce C# verze ____. Verze použitého DOTNETu je 7.0. <br>
Program používá šablonu WPF, která umožňuje lehčí stylizaci programu a vizualizaci hry pomocí XAML jazyku. <br>
## Popis algoritmu
Algoritmus ovládá hada tak, že mu na každém poli, na které had připlazí, podává instrukce o dalším směru, kterým se má had dále vydat. <br>
Algoritmus se dělí na dvě části : <br>
### Hamiltonovský cyklus
Hamiltonovský cyklus je pojem v topologii, který označuje graf, který lze projít tak, že každý uzel grafu je navštíven jen jednou, kromě startovního uzlu, který je zároven uzlem konečným. <br>
Tento pojem je perfektní pro dohrání Snaka v tom, že pokud had bude Hamiltonovský cyklus následovat, vždy sebere v cyklu jablko a nidky se nesrazí, protože jde jen jedním směrem. <br>
Tento cyklus je generovaný náhodně a je základní částí algoritmu.
#### Generace náhodného Cyklu
Za normálních podmínek je vytvořit takovýto cyklus velice těžké. Ovšem na tento program jen stačí, aby program generoval cyklus ve tvaru mřížky. <br>
Postup :
* Nejprve se vytvoří graf s dvakrát menšími stranami než má graf původní. Vznikne tak graf jehož každý uzel pokrývá 4 uzle (2 x 2) z původního grafu.
* Pak se každý uzel grafu propojí horizontálně a vertikálně se sousedními uzly. Každá hrana grafu je náhodně ohodnocena přirozeným číslem.
* Poté se graf předělá na minimální kostru grafu
