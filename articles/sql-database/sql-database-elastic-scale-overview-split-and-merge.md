---
title: "kiterjesztett felhő adatbázisok közötti aaaMoving adatok |} Microsoft Docs"
description: "Ismerteti, hogyan toomanipulate szilánkok és rugalmas használata önálló üzemeltetett szolgáltatáson keresztül move data adatbázis API-k."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 204fd902-0397-4185-985a-dea3ed7c7d9f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 629dee06e22609e9b61edf93ba5c38d997132d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Adatok mozgatása kiterjesztett felhőalapú adatbázisok között
Ha egy szoftver szolgáltatás fejlesztőként, és hirtelen az alkalmazás megy keresztül rengeteg igény szerinti, tooaccommodate hello növekedési kell. Így adhat meg további adatbázisok (szilánkok). Hogyan hello adatok toohello új adatbázisok újraterjeszteni a hello adatintegritás megszakítása nélkül Használjon hello **vegyes egyesítéses eszköz** toomove adatokat korlátozott adatbázis toohello új adatbázis.  

hello vegyes egyesítéses eszköz fut egy Azure webes. Egy rendszergazda vagy a fejlesztői használ hello eszköz toomove shardlets (a shard adatait) között a különböző adatbázisokhoz (szilánkok). hello eszköz shard felügyeleti toomaintain hello szolgáltatás metaadat adatbázist használ, és ellenőrizze a konzisztens leképezéseket.

![Áttekintés][1]

## <a name="download"></a>Letöltés
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Dokumentáció
1. [A rugalmas adatbázis vegyes egyesítéses eszköz oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Vegyes egyesítéses biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Vegyes egyesítéses biztonsági megfontolások](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [A szilánkleképezés kezelése](sql-database-elastic-scale-shard-map-management.md)
5. [Telepítse át a meglévő adatbázisok tooscale kimenő](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [A rugalmas adatbázis-eszközök](sql-database-elastic-scale-introduction.md)
7. [A rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md)

## <a name="why-use-hello-split-merge-tool"></a>Hello vegyes egyesítéses eszköz miért érdemes használni?
**Rugalmasság**

Alkalmazásokat kell toostretch rugalmasan túl egy Azure SQL Database-adatbázis hello határain. Szükséges toonew adatbázisok sértetlenségének megtartásával hello eszköz toomove adatokat használják.

**Vegyes toogrow** 

Általános tooincrease szükséges kapacitás toohandle robbanásszerűen növekedett. toodo tehát horizontális hello adatok és szétosztása Növekményesen további adatbázisok között, amíg kapacitásigények teljesülnek hozzon létre további kapacitást. Ez az a legjobb példa hello "vágási" funkció. 

**Tooshrink egyesítése**

Kapacitás toohello egy üzleti határozza jellege miatt kell zsugorítani. hello eszköz segítségével csökkentheti tartja toofewer méretezési egység, ha üzleti lelassul. hello "merge" szolgáltatása hello rugalmas bővítést vegyes egyesítéses szolgáltatás tartalmazza ezt a követelményt. 

**Shardlets mozgatásával elérési pontokhoz való kezelése**

Az adatbázisonként több bérlő shardlets tooshards hello elosztása az egyes szilánkok vezethet toocapacity szűk keresztmetszeteket. Ehhez az újbóli hozzárendelésének shardlets vagy foglalt shardlets toonew vagy kevesebb túlterhelt szilánkok áthelyezése. 

## <a name="concepts--key-features"></a>Alapfogalmak és a legfontosabb jellemzők
**Ügyfél által üzemeltetett szolgáltatások**

hello vegyes egyesítéses kerülnek az ügyfél által szolgáltatott szolgáltatásként. Telepíti, és a Microsoft Azure-előfizetés hello futtatására. hello letöltése a NuGet csomag egy konfigurációs sablon toocomplete hello információkat az adott telepítéshez. Lásd: hello [vegyes egyesítéses oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md) részleteiről. Hello szolgáltatás fut az Azure-előfizetéssel, mivel szabályozhatja, és konfigurálhatja a hello szolgáltatást a legtöbb biztonsági funkcióit. hello alapértelmezett sablon tartalmazza a hello beállítások tooconfigure SSL, a tanúsítvány alapú ügyfél-hitelesítéshez, a titkosítás a tárolt hitelesítő adatok, esetlegesen korán DoS és az IP-korlátozásokat. Található hello további információk biztonsági szempontjait a dokumentum a következő hello [vegyes egyesítéses biztonsági konfiguráció](sql-database-elastic-scale-split-merge-security-configuration.md).

hello alapértelmezett szolgáltatás fut egy worker és egy webes szerepkör telepítése. Azure Cloud Services használ minden egyes hello A1 Virtuálisgép-méretet. Míg ezek a beállítások nem módosítható, ha hello csomag telepítése, akkor a hello (keresztül hello Azure-portál) fut a felhőalapú szolgáltatás, a sikeres telepítés után sikerült módosítani. Vegye figyelembe, hogy hello feldolgozói szerepkör nem konfigurálható több mint egy egypéldányos technikai okokból. 

**A shard térkép integráció**

hello vegyes egyesítéses szolgáltatás hello shard térkép hello alkalmazás kommunikál. Használatakor hello vegyes egyesítéses szolgáltatás toosplit vagy egyesítés tartományok vagy toomove shardlets közötti szilánkok hello szolgáltatás szolgáltatás hello shard térkép toodate fel automatikusan. toodo tehát hello szolgáltatás toohello shard manager adatbázist hello alkalmazás kapcsolódik-e, és tárolja a tartományok hozzárendelések vegyes/egyesítési/move kérelmek állapotát. Ez biztosítja, hogy hello shard térkép mindig megadja egy naprakész nézetben amikor vegyes-merge műveletek fog. Szétválasztásához lemezegyesítési és -shardlet mozgási műveletek egy tranzakcióköteghez shardlets áthelyezése hello forrás shard toohello cél shard alapján történik. Hello shardlet adatátviteli művelet hello shardlets tulajdonos toohello aktuális köteg során megjelölve offline állapotúként hello shard leképezés, és nem állnak rendelkezésre adatok függő útválasztási kapcsolatot hello **OpenConnectionForKey** API. 

**Egységes shardlet kapcsolatok**

Adatátvitel egy új tranzakcióköteghez shardlets indításakor bármely shard térképét adatok függő útválasztási kapcsolatok toohello shard tárolni hello shardlet elejtett és az azt követő kapcsolatok hello shard leképezés API-k toohello ezek shardlets le vannak tiltva közben hello adatátvitelt jelölik a rendelés tooavoid inkonzisztenciát folyamatban van. Kapcsolatok tooother shardlets hello ugyanazt a shard is beolvasása leállítása, de akkor sikeres, újra meg azonnal az újra gombra. Miután hello kötegelt helyezik át, hello shardlets hello cél shard újra online megjelölve, és hello forrásadatok hello forrás shard törlődik. hello szolgáltatás végighalad ezeket a lépéseket minden kötegnél, amíg az összes shardlets át lett helyezve. Ez vezet tooseveral kapcsolódási kill műveleteket hello hello teljes vegyes/egyesítési/áthelyezési művelet során.  

**Shardlet rendelkezésre állásának kezelése**

Elérhetetlensége tooone köteg shardlets hello hatókörének korlátozása hello kapcsolat leállítása végleges, tehát a jelenlegi köteg toohello shardlets kapcsolatban a fentiekben ismertetett módon korlátozza egyszerre. Ez az előnyben részesített keresztül megközelítés ahol hello teljes shard marad a shardlets offline hello egy megosztott vagy egyesítési művelet során. hello egyszerre, különböző shardlets toomove hello értékként megadott köteg mérete egy konfigurációs paraméter. Minden felosztása és merge művelethez hello alkalmazás rendelkezésre állásának és teljesítményének igényeinek lehet meghatározni. Előfordulhat, hogy zárás alatt áll hello shard leképezés hello tartományba nagyobb, mint a megadott hello kötegmérete. Ennek az az oka hello szolgáltatást úgy, hogy hello tényleges hello adatok értékeit horizontális körülbelül megegyezzen hello kötegméret szerzi meg hello tartomány méretét. Ez a fontos tooremember különösen ritkán megadott horizontális skálázási kulcsok. 

**Metaadat-tároló**

hello vegyes egyesítéses szolgáltatás használ egy adatbázis toomaintain azok állapotát és tookeep naplózza a kérelem feldolgozása során. hello felhasználói hozza létre ezt az adatbázist az előfizetését és hello kapcsolati karakterláncát azt hello szolgáltatás központi telepítése a hello konfigurációs fájlban. Hello felhasználó szervezete rendszergazdákat is kapcsolódhatnak toothis adatbázis tooreview kérelem folyamatát és tooinvestigate részletes leírása a lehetséges hibákat.

**Horizontális-figyelése**

hello vegyes egyesítéses szolgáltatás (1) szilánkos táblák, (2) hivatkozás táblázatok, illetve (3) normál közötti különbséget tesz. vegyes/egyesítési/áthelyezés hello szemantikáját függ használt hello tábla hello típusú, és az alábbiak szerint definiáltuk: 

* **Horizontálisan skálázott táblák**: vegyes, egyesítési és áthelyezési műveletek shardlets áthelyezése forrás tootarget szilánkcímtárban. Hello sikeres befejezése után a teljes kérte, ezek shardlets már nincs jelen hello forrás. Vegye figyelembe, hogy hello célként megadott táblákhoz a hello cél shard tooexist, és nem tartalmazhat hello cél tartomány előzetes tooprocessing hello működési adatokat. 
* **Táblák hivatkozhat**: hivatkozási táblák, hello valószínűségét, egyesíteni, és helyezze át a műveletek hello adatokat másolni hello forrás toohello cél szilánkcímtárban. Vegye figyelembe azonban, hogy nem végez módosítást újrapróbálkozásoknak hello cél shard egy adott táblához tartozó, ha bármely sor már létezik ebben a táblázatban hello célponton. hello táblához toobe bármely hivatkozás tábla másolási művelet tooget feldolgozott üres.
* **Más táblák**: más táblák lehet jelen hello forrás- vagy egy megosztott és az egyesítési művelet hello célját. hello vegyes egyesítéses szolgáltatás parancsában ezek a táblázatok adatmozgás vagy másolási műveleteket. Vegye figyelembe azonban, hogy azok zavarhatja a megkötések esetén ezeket a műveleteket.

hivatkozás szilánkos táblák és hello információkat szolgáltatja hello **SchemaInfo** hello shard térképen API-k. a következő példa hello hello ezeket egy adott shard térkép manager objektum smm API-használatát mutatja be: 

    // Create hello schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

hello táblák "region" és "számára" hivatkozási táblák is meg van adva, és a felosztott/egyesítési/move műveletekkel másolja. "ügyfél" és "rendelések" pedig meg van adva szilánkos táblákat. C_CUSTKEY és O_CUSTKEY hello horizontális kulcsaként szolgál. 

**A hivatkozási integritás**

hello vegyes egyesítéses szolgáltatás a táblák közötti függőségek elemzésével, és használja az elsődleges kulcs külső kulcsok kapcsolatai toostage hello műveletek a referencia-táblák és -shardlets áthelyezését. Általában referencia táblák másolása először az függőségi sorrend, majd shardlets belül minden kötegelt függősége sorrendben kerülnek. Erre akkor szükség, így FK-PK korlátozza a hello cél shard hello új adatok érkeznek, figyelembe véve a vannak. 

**A shard térkép konzisztencia és végleges befejezése**

A hibák hello jelenlétét hello vegyes egyesítéses szolgáltatás bármely leállás után folytatja a műveletek, és célja toocomplete bármely, a folyamatban lévő kérések. Azonban lehetnek helyreállíthatatlan olyan helyzetekben, például amikor hello cél shard elveszett vagy sérült javíthatók. Ilyen körülmények néhány shardlets toobe áthelyezése volt amelyeknek továbbra is tooreside a hello forrás szilánkcímtárban. hello szolgáltatást biztosítja, hogy shardlet leképezéseket csak frissítése után hello szükséges adatok sikeresen másolt toohello cél. Shardlets csak törlődnek hello forrás, miután minden az adatok másolni toohello cél és hello megfelelő leképezések frissítése sikeresen megtörtént. hello törlési művelet során hello tartomány már online hello cél shard hello háttérben történik. hello vegyes egyesítéses szolgáltatás mindig biztosítja hello megfeleltetéseket tárolt hello shard leképezés helyességét.

## <a name="hello-split-merge-user-interface"></a>hello vegyes egyesítéses felhasználói felülete
hello vegyes egyesítéses service-csomag magában foglalja a feldolgozói szerepkör és a webes szerepkör. hello webes szerepkör használt toosubmit vegyes-egyesítési kérelem interaktív módon. hello hello felhasználói felület fő összetevői a következők:

* Művelet típusa: hello művelet típus, amely a hello típusú hello szolgáltatást a kérelem által végrehajtott művelet a megfelelő választógombra kattintva. Hello vegyes választhat, egyesítése, és helyezze át a forgatókönyveket. Szakítsa meg a korábban elküldött műveletet is. Vegyes használata, egyesítése és a tartomány shard maps áthelyezések. Lista shard csak a támogatási áthelyezési műveletek képezi le.
* A shard térkép: hello következő szakaszában kérelem paraméterek borítóján információ hello shard térkép és hello adatbázis-üzemeltetési a shard térképre. Különösen tooprovide hello nevét hello Azure SQL adatbázis-kiszolgáló és adatbázis-üzemeltetési hello shardmap van szüksége, hitelesítő adatok tooconnect toohello shard adatbázis rendelve, és végül hello hello shard leképezés neve. Hello művelet jelenleg csak egyetlen halmazába hitelesítő adatokat fogad el. Ezeket a hitelesítő adatokat kell toohave megfelelő engedélyekkel tooperform toohello shard térkép, valamint a felhasználói adatok toohello hello szilánkok változik.
* Forrástartományt (ossza fel, és egyesíti az): egy vegyes és az egyesítési művelet feldolgozza a alacsony és magas kulcsok a tartományt. egy unbounded magas kulcsérték, ellenőrzés művelet toospecify hello "felső kulcs maximális" jelölőnégyzetet, és hello magas kulcs mezőt hagyja üresen. hello tartomány kulcsértékei megadott tegye nem szükséges tooprecisely egyezik, a leképezési és a határok shard képezze. Ha nem ad meg tartományt határokat minden hello szolgáltatást fog következtet hello legközelebbi tartomány meg automatikusan. Egy adott shard leképezés hello GetMappings.ps1 PowerShell parancsfájl tooretrieve hello aktuális hozzárendelések is használhatja.
* Megosztott adatforrás viselkedés (vegyes): A felosztás műveleteket, adja meg a hello pont toosplit hello forrástartományt. Ehhez, adja meg a kívánt hello vágási toooccur hello horizontális kulcs. Használjon hello megfelelő választógombra kattintva adja meg, hogy hello (kivéve a hello ossza fel a kulcs) tartomány toomove alsó részén hello, vagy hogy szeretné-e hello felső rész toomove (hello megosztott kulcsot is beleértve).
* Forrás-Shardlet (lépés): helyezze át a műveletek eltérnek vegyes, vagy nincs szükségük a tartomány toodescribe hello forrás összevonhatja a műveletek. Áthelyezési forrást egyszerűen azonosíthatók hello horizontális kulcsérték toomove megtervezni.
* Cél Shard (vegyes): Miután hello adatok megadása a felosztás művelet hello forrás, ahová hello adatok toobe másolt tooby olyan hello Azure SQL adatbázis-kiszolgálót és adatbázist, hello cél toodefine kell.
* Cél tartományon (egyesítés): egyesítési műveletek áthelyezése shardlets tooan meglévő szilánkcímtárban. Hello meglévő shard hello tartományhatárokon hello meglévő tartomány toomerge a kívánt megadásával azonosította.
* Köteg mérete: hello kötegelt méretét szabályozza, hogy a rendszer kapcsolat nélküli munka hello adatátvitel során egyszerre shardlets száma hello. Egész szám azt, ahol ugyanúgy használhatók a kisebb értékek shardlets az érzékeny toolong idõszakok esetén. A nagyobb értékek növeli, amely egy adott shardlet hello ideje offline de javíthatja a teljesítményt.
* Művelet azonosítója (Mégse): Ha egy folyamatban lévő műveletet, amely már nem szükséges, megszakíthatja hello művelet azáltal, hogy a művelet azonosítója ebben a mezőben. Hello kérelem állapot tábla kérhetnek le hello Műveletazonosító (lásd a szakasz 8.1) vagy a hello kimenet hello webböngészőben, ahol hello kérés elküldése megtörtént.

## <a name="requirements-and-limitations"></a>Követelmények és korlátozások
hello hello vegyes egyesítéses szolgáltatás jelenlegi implementációja tulajdonos toohello a következő követelmények és korlátozások: 

* hello szilánkok tooexist kell, és hello shard leképezés egy vegyes-merge műveletet a szilánkok elvégzése előtt regisztrálni. 
* hello szolgáltatás hozta létre a táblák vagy bármely más adatbázis-objektumok automatikusan műveletei részeként. Ez azt jelenti, hogy a séma összes szilánkos táblák hello és táblák hivatkozik a hello cél shard előzetes tooany vegyes/egyesítési/áthelyezési művelet tooexist kell. Horizontálisan skálázott táblák különösen is szükséges toobe üres, ha az új shardlets nem osztott/egyesítési/áthelyezés által hozzáadott toobe hello közé. Ellenkező esetben hello művelet sikertelen lesz hello cél shard hello kezdeti konzisztencia-ellenőrzést. Is vegye figyelembe, hogy a referenciaadatok csak másolja, ha hello összefoglaló táblázat üres, és, hogy vannak-e nem konzisztencia biztosítja, hogy a legutóbb tooother egyidejű írási műveletek hello hivatkozási táblák. Ezt azért ajánljuk: vegyes/egyesítési műveletek futtatásakor más írási műveletek módosításokat toohello hivatkozási táblák.
* hello szolgáltatás sor azonosító egy egyedi index vagy a kulcs, amely tartalmazza az hello horizontális kulcs tooimprove teljesítménye és megbízhatósága a nagy shardlets létesíti támaszkodik. Ez lehetővé teszi, hogy hello szolgáltatás toomove adatainak még pontosabban részletességű értéknél csak hello horizontális kulcs. Ezzel a megoldással tooreduce hello maximális mennyisége naplóterület és zárolásokat, amelyek szükségesek hello művelet során. Érdemes létrehozni egy egyedi index vagy egy elsődleges kulcs, beleértve a hello horizontális kulcs egy adott táblán, ha azt szeretné, hogy toouse adott táblához vegyes/egyesítési/move kérelmek. A megfelelő teljesítmény érdekében hello horizontális kulcs hello kezdő oszlop hello kulcs vagy hello index kell lennie.
* Hello során végzett kérésfeldolgozással néhány shardlet adatok jelen, mind a hello forrás- és hello cél shard lehet. Ez a szükséges tooprotect-hibákkal szemben hello shardlet mozgás során. hello vegyes egyesítéses a hello shard térkép integrációja biztosítja, hogy a kapcsolatok keresztül hello függő útválasztási API-k használatával végzett hello **OpenConnectionForKey** hello shard térképen metódus nem látható minden inkonzisztens köztes állapotok. Azonban ha csatlakozó toohello forrás- vagy hello cél szilánkok hello használata nélkül **OpenConnectionForKey** metódus, köztes inkonzisztens állapotok visszatéréskor jelenik vegyes/egyesítési/move kérelmek fog. Ezek a kapcsolatok részleges vagy ismétlődő eredmények hello időzítés vagy hello shard hello kapcsolat függően előfordulhat, hogy megjelenítése. Ez a korlátozás jelenleg magában foglalja a rugalmas bővítést több-Shard-lekérdezések által létrehozott hello kapcsolatok.
* nem kell osztani hello metaadatokat tároló adatbázis hello vegyes egyesítéses szolgáltatás különböző szerepkörök között. Például a tesztelési futtató hello vegyes egyesítéses szolgáltatás egyik szerepköre kell toopoint tooa különböző metaadatokat tároló adatbázis hello éles szerepkörnél.

## <a name="billing"></a>Számlázás
hello vegyes egyesítéses szolgáltatás fut, felhőalapú szolgáltatásként a Microsoft Azure-előfizetést. Ezért a felhőalapú szolgáltatások hello szolgáltatás tooyour példánya alkalmazni. Kivéve, ha gyakran műveleteket hajt végre vegyes/egyesítési/áthelyezésére, javasoljuk a felosztás egyesítéses felhőszolgáltatás törlése. Menti a futó költségeit vagy felhő szolgáltatáspéldány telepített. Hozza létre újra, és indítsa el a könnyen futtatható konfiguráció tooperform van szüksége vágási vagy egyesítési műveletek. 

## <a name="monitoring"></a>Figyelés
### <a name="status-tables"></a>Állapot táblák
hello vegyes egyesítéses szolgáltatást biztosít hello **RequestStatus** hello metaadatokat tároló adatbázis befejeződött, és a folyamatban lévő kérelmek figyeléséhez. hello táblázat minden egyes vegyes-egyesítési kérelem töltött hello vegyes egyesítéses szolgáltatás elküldött toothis példánya egy sort. A következő információkat az egyes kérelmek hello biztosít:

* **Timestamp típusú**: hello hello kérelem bekapcsolásakor dátumát és időpontját.
* **OperationID azonosítójú**: A GUID, amely egyedileg azonosítja a hello kérelem. A kérelem is használt toocancel hello művelet, amíg még mindig folyamatban.
* **Állapot**: hello hello kérés jelenlegi állapotában. A folyamatban lévő kérések is felsorolja hello a mely hello kérelme, mert az aktuális fázisban.
* **CancelRequest**: A jelzőt, amely azt jelzi, hogy hello kérelem visszavonásra került.
* **Folyamatban lévő**: A százalékos becsült feldolgozottsági hello a művelethez. 50 érték azt jelzi, hogy-e a hello művelet körülbelül 50 % kész.
* **Részletek**: az XML-érték, amely egy részletesebb jelentést. Állapotjelentés hello beállítása a sorok másolja át a forrás tootarget rendszeresen frissül. Hibák vagy kivételek esetén ebben az oszlopban hello a hibával kapcsolatos részletes adatokat is tartalmaz.

### <a name="azure-diagnostics"></a>Azure Diagnostics
hello vegyes egyesítéses szolgáltatás figyelési és diagnosztika Azure SDK 2.5 alapján Azure Diagnostics használja. Hello diagnosztikai konfigurációja szabályozhatja az itt leírtak szerint: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](../cloud-services/cloud-services-dotnet-diagnostics.md). hello letöltött csomag tartalmazza a két diagnosztikai konfiguráció - egy hello webes szerepkör, egy hello feldolgozói szerepkör esetében. Ezen diagnosztika konfigurációk hello szolgáltatás követve hello a [Cloud Service Fundamentals a Microsoft Azure-ban](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Ez magában foglalja a hello definíciók toolog teljesítményszámlálók, a IIS naplókat, a Windows eseménynaplóiban keresse meg és a felosztott egyesítéses alkalmazás eseménynaplóiban. 

## <a name="deploy-diagnostics"></a>Diagnosztika telepítése
tooenable figyelési és -diagnosztika hello diagnosztikai konfiguráció hello hello NuGet-csomagot, futtassa a következő parancsokat az Azure PowerShell hello által biztosított webes és feldolgozói szerepkörök: 

    $storage_name = "<YourAzureStorageAccount>" 

    $key = "<YourAzureStorageAccountKey" 

    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  


    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 


    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Kapcsolatos további tájékoztatásért található tooconfigure és központi telepítése a diagnosztikai beállítások itt: [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Diagnosztika beolvasása
A diagnosztika könnyen elérhető hello hello Server Explorer fa Azure része a Visual Studio Server Explorer hello. Nyissa meg a Visual Studio példánya, és hello menüsávon kattintson a nézet, és a Server Explorer. Kattintson a hello Azure ikon tooconnect tooyour Azure-előfizetés. Keresse meg tooAzure -> tároló -> <your storage account> -> táblák WADLogsTable ->. További információkért lásd: [tárolási erőforrások keresse meg a Server Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

hello hello a fenti ábrán kiemelt WADLogsTable hello tartalmaz részletes hello vegyes egyesítéses szolgáltatás alkalmazásnaplóban származó események. Vegye figyelembe, hogy a letöltött hello hello használható alapértelmezett konfigurációt csomag éles környezet részesíti előnyben. Ezért, amellyel naplókat, valamint a számlálók kikerülnek hello szolgáltatáspéldány hello időköz, nagy (5 perc). Teszt és fejlesztési alacsonyabb hello hello web vagy hello feldolgozói szerepkör tooyour hello diagnosztikai beállítások módosításával időköz. Kattintson a jobb gombbal a Visual Studio Server Explorer (lásd fent) hello hello szerepkör, és adja meg az átviteli időszak hello hello párbeszédpanelen hello diagnosztika konfigurációs beállítások: 

![Konfiguráció][3]

## <a name="performance"></a>Teljesítmény
Jobb teljesítmény általában toobe elvárás hello nagyobb, a további performant szolgáltatási szinteket az Azure SQL-adatbázis. Hello magasabb szolgáltatásszintek magasabb IO-, Processzor- és memória helylefoglalását hello tömeges másolás előnyeit, és törölje a felosztott egyesítéses szolgáltatás által használt hello műveletek. Éppen ezért növelése hello szolgáltatási szintjei csak ezeket az adatbázisokat egy meghatározott, korlátozott ideig.

hello szolgáltatás is érvényesítési lekérdezések a normál műveletek részeként hajtja végre. A következő érvényesítési lekérdezések hello cél tartomány adatai váratlan jelenléte keressen, és győződjön meg arról, hogy a vegyes/egyesítési/áthelyezési művelet elindítja a konzisztens. Ezeket a lekérdezéseket az összes működik olyan nagy távolságú horizontális kulcstartományokkal hello hatókör hello művelet és hello kérelem definíciója részeként hello köteg mérete határozza meg. Ezeket a lekérdezéseket legjobb végre, ha az index jelen, hello horizontális kulccsal rendelkezik, mint a kezdő oszlop hello. 

Ezenkívül hello horizontális kulccsal hello kezdő oszlop egyediségének tulajdonság lehetővé teszi a hello szolgáltatás toouse optimalizált megközelítés, amely korlátozza az erőforrás-felhasználás naplózási hely- és memória szempontjából. Ez egyediségi tulajdonság szükséges toomove nagy adatmennyiség (általában felett 1GB). 

## <a name="how-tooupgrade"></a>Hogyan tooupgrade
1. Hello kövesse [vegyes egyesítéses szolgáltatás telepítése](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. A felhőalapú szolgáltatás konfigurációs fájlja a felosztás egyesítéses telepítési tooreflect hello új konfigurációs paraméterek módosítása. Egy új kötelező paraméter a titkosításhoz használt hello tanúsítvány hello adatait. Egy egyszerű módot toodo toocompare hello új konfigurációs sablon fájl hello le a meglévő konfigurációt ellen. Ellenőrizze, hogy hello webes és feldolgozói szerepkör hello "DataEncryptionPrimaryCertificateThumbprint" és "DataEncryptionPrimary" hello-beállítások.
3. Hello frissítés tooAzure telepítés megkezdése előtt győződjön meg arról, hogy az összes jelenleg futó vegyes merge műveletet végzett. Könnyen ehhez hello RequestStatus és PendingWorkflows táblák hello vegyes egyesítéses metaadatokat tároló adatbázis a folyamatban lévő kérelmek lekérdezésével.
4. A meglévő felhőalapú szolgáltatás az üzemelő példány frissítése vegyes-egyesítése az Azure-előfizetéshez hello új csomag és a frissített konfigurációs fájlt.

Vegyes egyesítéses tooupgrade tooprovision egy új metaadatokat tároló adatbázis nem szükséges. hello új verziója automatikusan frissíti a meglévő metaadatok adatbázis toohello új verziója. 

## <a name="best-practices--troubleshooting"></a>Ajánlott eljárások és hibaelhárítás
* Adja meg egy tesztelési bérlőn, valamint a legfontosabb vegyes/egyesítési/áthelyezési műveletek hello tesztbérlővel megadásával több szilánkok között. Győződjön meg arról, hogy minden helyesen van definiálva a shard képezze, hogy hello műveletek sérti vonatkozó megkötések vagy a külső kulcsokat.
* Tartsa hello tesztelési bérlői adatméret hello adatok maximális mérete a legnagyobb bérlő fent nem áll kapcsolatban adatméret tooensure kapcsolatos hiba lépett fel. Ezzel a megoldással egy felső határa felmérheti a hello időt toomove egy egybérlős körül. 
* Győződjön meg arról, hogy a séma lehetővé teszi a törlést. hello vegyes egyesítéses szolgáltatáshoz hello képességét tooremove hello forrás shard adatait hello adatok sikeresen másolt toohello cél követően. Például **triggerek törlése** megakadályozhatja, hogy a hello szolgáltatás hello forrás hello adatok törlése és a műveletek toofail.
* hello horizontális kulcs hello kezdő oszlop lehet az Ön elsődleges kulcs vagy egyedi index definícióját. Hello legjobb teljesítményt hello megosztott vagy egyesítés érvényesítési lekérdezések, ezzel biztosítható és hello tényleges mozgás és törlési műveletek horizontális kulcstartományokkal mindig működésbe, amelyek.
* A felosztott egyesítéses szolgáltatás az adatbázisokat tároló hello régió és az adatok Center elhelyezésének engedélyezése. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png

