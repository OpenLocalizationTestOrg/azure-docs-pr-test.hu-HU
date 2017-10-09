hello a következő táblázat ismerteti az egyes hello fő kvóták, korlátok, alapértelmezett és az Azure Scheduler szabályozások.

| Erőforrás | Korlát leírása |
| --- | --- |
| **Feladat mérete** |Feladat maximális mérete 16 KB-os. Ha egy PUT vagy egy javítás egy nagyobb, mint a működés felső korlátjának feladatot eredményez, 400 Hibás kérés állapotkód adja vissza. |
| **Kérelem URL-cím mérete** |Hello kérelem URL-cím maximális mérete 2048 karakter. |
| **Összesített fejléc mérete** |Maximális összesített fejléc mérete 4096 karakter. |
| **Fejléc száma** |A fejléc maximális száma 50 fejlécek. |
| **Törzs mérete** |Maximális törzs mérete: 8192 karakter. |
| **Ismétlődés span** |Maximális ismétlődésére span 18 hónap. |
| **Toostart időpont** |"Idő toostart idő" legfeljebb 18 hónap. |
| **Feladatelőzmények** |A feladatelőzmények tárolt maximális választörzs mérete 2048 bájt. |
| **Gyakoriság** |hello alapértelmezett maximális gyakoriság kvóta a tartalmaz szabad feladatgyűjteményt 1 óra és a szabványos feladatgyűjteményben 1 perc. hello maximális gyakoriság értéke egy feladat gyűjtemény toobe alacsonyabb, mint a maximális hello konfigurálható. Hello feladatgyűjtemény levő összes feladatnak korlátozott hello értéke hello feladatgyűjtemény állítható be. Ha egy feladat, mint a maximális gyakorisága hello hello gyűjteményen nagyobb gyakorisággal toocreate majd kérelem sikertelen lesz, és ütközés 409 állapotkódot. |
| **Feladatok** |hello alapértelmezett maximális feladatok kvóta tartalmaz szabad feladatgyűjteményt 5 feladatok és a szabványos feladatgyűjteményben 50 feladat. feladatok maximális száma hello lehet konfigurálni egy gyűjteményen. Hello feladatgyűjtemény levő összes feladatnak korlátozott hello értéke hello feladatgyűjtemény állítható be. Ha toocreate hello maximális feladatok kvóta, akkor a hello kérelem több feladat 409 ütközés-állapotkód: sikertelen lesz. |
| **Feladatgyűjtemények** |Előfizetésenként feladatgyűjtemény maximális száma érték 200 000. |
| **Feladat előzményeinek megőrzése** |Feladatelőzmények mentése too2 hónapig megmarad, vagy be toohello utolsó 1000 végrehajtások. |
| **Feladat befejeződött, és a hibás megőrzése** |Befejeződött, és a hibás feladatok 60 napig maradnak. |
| **Időtúllépés** |Nincs olyan statikus (nem konfigurálható) kérelmi időkorlátot. a HTTP-műveletek 60 másodperc. A hosszabb ideig futó műveletek kövesse a HTTP aszinkron protokollok; például egy 202 azonnal vissza, de továbbra is működik-e hello háttérben. |

