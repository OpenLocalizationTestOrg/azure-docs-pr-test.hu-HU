
>[!NOTE]
>A Log Analytics korábban Operational Insights néven volt ismert.
>
>

a következő korlátozások hello tooLog Analytics erőforrások előfizetésenként vonatkoznak:

| Erőforrás | Alapértelmezett korlát | Megjegyzések
| --- | --- | --- |
| Ingyenes munkaterületek száma előfizetésenként | 10 | Ez a korlát nem növelhető. |
| Fizetős munkaterületek száma előfizetésenként | N/A | Vannak korlátozva erőforrások erőforráscsoporton belül hello száma és az erőforráscsoportok előfizetésenként | 


a következő korlátozások hello tooeach Naplóelemzési munkaterület vonatkoznak:

|  | Ingyenes | Standard | Prémium | Különálló | OMS |
| --- | --- | --- | --- | --- | --- |
| Naponta összegyűjtött adatmennyiség |500 MB<sup>1</sup> |None |None | None | None
| Adatmegőrzés időtartama |7 nap |1 hónap |12 hónap | 1 hónap<sup>2</sup> | 1 hónap<sup>2</sup>|

<sup>1</sup> Ha az ügyfelek eléri az 500 MB-os napi adatátviteli korlátot, adatelemzés leáll, és hello hello elején folytatja a következő napon. A napi elszámolás UTC-alapú.

<sup>2</sup> hello adatok megőrzési időtartam hello önálló és díjszabások OMS megnövekedett too730 nap lehet.

| Kategória | Korlátok | Megjegyzések
| --- | --- | --- |
| Data Collector API | Az egyedi közzétételek maximális mérete 30 MB<br>A mezőértékek maximális mérete 32 KB | A nagyobb mennyiségeket bontsa több közzétételre<br>A 32 KB-nál hosszabb mezők csonkolva lesznek. |
| Keresési API | 5000 visszaadott rekord a nem összesített adatok esetében<br>500 000 rekord az összesített adatok esetében | Összesített adatok, amely tartalmazza az hello keresés `measure` parancs
 
