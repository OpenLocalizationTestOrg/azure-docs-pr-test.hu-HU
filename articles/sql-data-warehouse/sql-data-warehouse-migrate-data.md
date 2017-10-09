---
title: "aaaMigrate az adatok tooSQL adatraktár |} Microsoft Docs"
description: "Tippek az adatok tooAzure SQL Data Warehouse adattárházzal történő, megoldások áttelepítéséhez."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a>Adatok áttelepítése
Adatok az SQL Data Warehouse egy különböző eszközeivel mozgathatók különböző forrásokból származó.  ADF másolás, SSIS és bcp lehet az összes használt tooachieve a cél. Azonban, ha hello adatok mennyisége növekszik gondolja bontásához hello adatok áttelepítési folyamat azokat a lépéseket. Ez számára biztosítja, hogy hello lehetőség toooptimize egyes lépések a teljesítmény és a rugalmasság tooensure zökkenőmentes adatáttelepítés.

A cikk ismerteti a hello egyszerű áttelepítési forgatókönyvek ADF másolása, SSIS és bcp először. Ezután kissé mélyebben hogyan optimalizálható a hello áttelepítési megjelenését.

## <a name="azure-data-factory-adf-copy"></a>Az Azure Data Factory (ADF) másolása
[ADF másolási] [ ADF Copy] része [Azure Data Factory][Azure Data Factory]. Használhatja ADF másolási tooexport a várt a helyi tároló tooremote egybesimított fájlokba tartani az Azure blob Storage tárolóban, vagy közvetlenül az SQL Data Warehouse-adatok tooflat fájlokat.

Ha az adatok indul egybesimított fájlokba, majd tootransfer először azt tooAzure tárolási blob egy terheléselosztási kezdeményezése előtt azt az SQL Data Warehouse. Miután hello adatátvitel Azure blob Storage tárolóban történő toouse választhat [ADF másolási] [ ADF Copy] újra toopush hello adatokat az SQL Data Warehouse.

A PolyBase is biztosít egy nagy teljesítményű beállítása hello adatok betöltésekor. Azonban, hogy: a két eszköz helyett egy. Ha hello legjobb teljesítmény érdekében szükséges, akkor a polybase szolgáltatást akkor használja. Ha azt szeretné, hogy egy eszköz-élmény (és hello adatok nem jelentős) ADF a válasz.


> 
> 

A következő cikk néhány nagy toohello keresztül központi [ADF minták][ADF samples].

## <a name="integration-services"></a>Integrációs szolgáltatások
Integration Services (SSIS) egy hatékony és rugalmas bontsa ki az átalakítási és betöltési (ETL) eszközt, amely támogatja a bonyolult munkafolyamatok, adatok átalakítása és több adatok betöltését lehetőség. SSIS toosimply átviteli adatok tooAzure használja vagy szélesebb körű áttelepítés részeként.

> [!NOTE]
> SSIS exportálhatók tooUTF-8 nélkül hello bájt rendelés hello fájl be van jelölve. Ez előbb használnia kell hello származtatott oszlop összetevő tooconvert hello karakteres adatokat tooconfigure hello adatok folyamata toouse hello 65001 UTF-8 kódlapján találhatók. Hello oszlopok konvertálás után írható hello adatok toohello egybesimított fájl cél adapter győződjön meg arról, hogy 65001-es is van kiválasztva hello kódlapjának hello fájlt.
> 
> 

SSIS tooSQL adatraktár csatlakozik, mint amelyhez tooa SQL Server-telepítés csatlakozni. Azonban a kapcsolatokat. használja az ADO.NET Csatlakozáskezelő toobe kell. Meg is gondoskodunk a tooconfigure hello "Használata tömeges beszúrás eredményről" beállítás toomaximize átviteli sebességet. Tekintse meg a toohello [ADO.NET céladapter] [ ADO.NET destination adapter] cikk toolearn további információk a tulajdonság

> [!NOTE]
> Nem támogatott a Kapcsolódás az SQL Data Warehouse tooAzure OLEDB használatával.
> 
> 

Ezenkívül, mindig fennáll hello lehetséges, hogy a csomag sikertelen lehet toothrottling vagy hálózati problémák miatt. Tervezési csomagok, így azok folytathatók hello pontján, hiba nélkül megismétlése működnek az elvégzett hello meghibásodása előtt.

További információt talál részletes hello [SSIS-dokumentáció][SSIS documentation].

## <a name="bcp"></a>bcp
BCP parancssori segédprogram, amely a strukturálatlan fájl adatok importálásának és exportálásának szolgál. Néhány átalakítási adatok exportálásakor akkor kerül sor. tooperform egyszerű átalakítások egy lekérdezés tooselect használja, és hello adatok. Exportálása után hello egybesimított fájlokba majd töltődnek be közvetlenül a hello cél hello SQL Data Warehouse-adatbázis.

> [!NOTE]
> Általában célszerű a jó ötlet tooencapsulate hello átalakítások során adatok exportálása hello forrásrendszer nézetben. Ez biztosítja, hogy hello logika megmarad hello folyamat, és ismételhető.
> 
> 

Bcp előnyei a következők:

* Egyszerű. egyszerű toobuild BCP parancsok és végrehajtása
* Újra elindítható betöltési folyamatot. Egyszer exportált hello terhelés lehet tetszőleges számú alkalommal végre

A bcp korlátozásai a következők:

* BCP egyszerű fájlok táblázatos működik. Nem működik az XML- vagy JSON például fájlok
* Adatok átalakítása képességek csak korlátozott toohello exportálási előkészítés, és egyszerű jellegű
* BCP nem lett igazítani toobe robusztus, amikor az adatok betöltésekor keresztül hello internet. Minden hálózati instabilitás okozhat a betöltési hiba történt.
* BCP megtalálhatók-e a hello cél adatbázis előzetes toohello terhelés hello séma támaszkodik.

További információkért lásd: [bcp tooload adatokat használja az SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Adatok áttelepítése optimalizálása
Egy SQLDW adatok áttelepítési folyamat hatékonyan sorolhatók három különálló lépésből áll:

1. A forrásadatok exportálása
2. Adatok tooAzure átruházása
3. Hello SQLDW céladatbázis betöltése

Az egyes lépések lehet külön-külön optimalizált toocreate egy robusztus, újra elindítható és rugalmas áttelepítési folyamat, amely a lehető legnagyobbra növeli a teljesítményt lépésről lépésre.

## <a name="optimizing-data-load"></a>Optimalizálás adatok betöltése
A következő fordított sorrendben egy pillanatra; keresése hello leggyorsabb módon tooload adata polybase. PolyBase-betöltési folyamat optimalizálása helyezi Előfeltételek megelőző lépéseket, hogy ajánlott toounderstand ez hello társaságuk. Ezek a következők:

1. Fájlok kódolása
2. Az adatfájlok formátuma
3. Adatforrás adatfájljainak helyét

### <a name="encoding"></a>Encoding
A PolyBase adatok fájlok toobe UTF-8 vagy UTF-16FE van szükség. 



### <a name="format-of-data-files"></a>Az adatfájlok formátuma
A PolyBase csak egy rögzített méretű sor lezárójele \n vagy új sor. Az adatfájlok meg kell felelnie toothis szabvány. Nincs karakterlánc- vagy lezárói korlátozásait.

Hogy toodefine minden egyes oszlophoz hello fájlban a PolyBase külső táblájában részeként. Győződjön meg arról, hogy minden exportált oszlopokra szükség, és hogy hello típusok megfelelnek-e a szükséges toohello szabványoknak.

Tekintse meg ismét toohello [telepítse át a séma] cikk a támogatott adattípusokat részletek.

### <a name="location-of-data-files"></a>Adatforrás adatfájljainak helyét
Az SQL Data Warehouse PolyBase tooload adatokat az Azure Blob Storage kizárólag használja. Következésképpen hello adatokat kell először átadott a blob-tárolóba.

## <a name="optimizing-data-transfer"></a>Optimalizálás adatátvitel
Adatok áttelepítése a leghosszabb részei hello egyik hello adatok tooAzure hello adatátvitelt. Nem csak lehet hálózati sávszélesség problémát, de a hálózat megbízhatóságát is komolyan gátolja folyamatban van. Alapértelmezés szerint az adatok áttelepítése tooAzure internet így hello ésszerűen valószínűleg előforduló átviteli hibák előfordulási esélyét hello felett van. Ezek a hibák azonban egészben vagy részben újra küldött adatok toobe lehet szükség.

Szerencsére még több beállítások tooimprove hello sebesség és a rugalmasság, a folyamat során:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
Érdemes lehet tooconsider használatával [ExpressRoute] [ ExpressRoute] toospeed hello átviteli be. [ExpressRoute] [ ExpressRoute] biztosít hozzá, és egy személyes kapcsolat tooAzure, hello kapcsolat nem halad át hello nyilvános internethez. Ez egy kötelező lépés semmiképpen sem. Azonban azt fogja javítják a teljesítményt adatok tooAzure küldését egy helyszíni vagy a közös elhelyezés létesítmény.

hello használatának előnyei [ExpressRoute] [ ExpressRoute] vannak:

1. Nagyobb megbízhatóságot
2. Gyorsabb hálózati sebesség
3. Kisebb hálózati késés
4. magasabb szintű hálózati biztonság

[ExpressRoute] [ ExpressRoute] forgatókönyvek számos hasznos; nem csak az áttelepítési hello.

Felkeltettük az érdeklődését? További információt és az árképzés terén adja a Microsoft hello [ExpressRoute dokumentációja][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Az Azure Import és Export szolgáltatásról
hello Azure importálása és exportálása szolgáltatás rendkívül adatok átviteli folyamat nagy (GB ++) toomassive (TB ++) adatok továbbítása az Azure tervezték. Az adatok toodisks írást, és a szállítási őket az Azure data center tooan magában foglalja. hello lemez tartalma majd betölti Azure Storage blobs szolgáltatásában az Ön nevében.

Hello importálási exportálási folyamat áttekintése a következőképpen történik:

1. Egy Azure Blob Storage tárolóban tooreceive hello adatok konfigurálása
2. Az adattároló toolocal exportálása
3. Másolja a hello adatok too3.5 hüvelyk SATA II/III merevlemez-meghajtók [Azure Import/Export eszköz] hello használata
4. Hello Azure Import és hello Adatbázisnapló-fájlok hello [Azure Import/Export eszköz] által visszaadott biztosító exportálása szolgáltatás importálási feladat létrehozása
5. A kijelölt Azure-adatközpont küldje el a hello lemezek
6. Az adatok egy átvitt tooyour Azure Blob Storage-tároló
7. Hello adatok betöltése a PolyBase használatával SQLDW

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] segédprogram
Hello [AZCopy][AZCopy] segédprogram egy remek eszköz az első Azure Storage Blobs átvihetők az adatokat. Kis (MB ++) toovery nagy (GB ++) adatátviteli tervezték. [AZCopy] is kialakították tervezett tooprovide jó rugalmas átviteli, amikor adatokat tooAzure, ezért átvitele hello adatok átvitel lépés kiváló választás. Egyszer átvitt hello adatokkal a PolyBase használatával az SQL Data Warehouse betöltése. AZCopy is beépítheti a SSIS-csomagok használata egy "Hajtható végre folyamat" feladatot.

toouse AZCopy először fog toodownload kell, és telepítse azt. Van egy [éles verziójával] [ production version] és egy [előzetes verzióval] [ preview version] érhető el.

tooupload egy fájlt a fájlrendszer alatt kell egy hello hasonló parancsot:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Egy magas szintű folyamat összegzése lehet:

1. Egy Azure storage blob tároló tooreceive hello adatok konfigurálása
2. Az adattároló toolocal exportálása
3. AZCopy hello Azure Blob Storage tárolóban adatait
4. Hello adatok betöltése az SQL Data Warehouse PolyBase használatával

Teljes rendelkezésre álló dokumentáció: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>Optimalizálás adatok exportálása
Továbbá, hogy hello exportálási megfelel-e toohello követelményeinek megfelelnek a PolyBase által meg tooensuring toooptimize hello exportálási hello adatok tooimprove hello folyamat további is kérhet.



### <a name="data-compression"></a>Adattömörítés
A PolyBase gzip tömörített adatok olvashatók. Ha tudja toocompress toogzip az adatfájlokat, akkor minimálisra csökkentheti hello adatmennyiség alatt leküldött hello hálózaton keresztül.

### <a name="multiple-files"></a>Több fájl
Több fájlokba nagy táblák összeállításának nem csak tooimprove sebesség exportálása, emellett segít az átviteli re-startability, és egyszer az hello Azure blob storage hello adatok teljes kezelhetőségi hello. A PolyBase sok töltött szolgáltatását, hogy emellett a mappában lévő összes hello fájlok olvasása és egy táblázat azt tekinti hello egyikét. Éppen ezért egy jó ötlet tooisolate hello fájlok minden táblához saját mappába.

PolyBase az úgynevezett "rekurzív mappa átjárás" is támogatja. A szolgáltatás használható toofurther javíthatja az exportált adatok tooimprove hello felépítése az adatok kezelése.

toolearn PolyBase, az adatok betöltése kapcsolatos további információkért lásd: [az SQL Data Warehouse PolyBase használata tooload adatok][Use PolyBase tooload data into SQL Data Warehouse].

## <a name="next-steps"></a>Következő lépések
Áttelepítési kapcsolatban bővebben lásd: [telepítse át a megoldás tooSQL adatraktár][Migrate your solution tooSQL Data Warehouse].
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
