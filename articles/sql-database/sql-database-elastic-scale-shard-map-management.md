---
title: "Azure SQL-adatbázis kimenő aaaScale |} Microsoft Docs"
description: "Hogyan toouse hello ShardMapManager, elastic database ügyféloldali kódtár"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a>Horizontális felskálázás hello shard térkép manager adatbázisokkal
a Azure SQL-adatbázisok tooeasily kibővítési-kezelővel a shard térkép. hello shard térkép manager shard csoportban lévő összes szilánkok (adatbázisok) globális hozzárendelés információt egy különleges adatbázis. hello metaadatok lehetővé teszi, hogy az alkalmazás tooconnect toohello megfelelő adatbázisához hello hello értéke alapján **horizontális kulcs**. Emellett minden shard hello készlet tartalmazza, amelyek nyomon követik a hello helyi részekre bonthatók az adatok (úgynevezett **shardlets**). 

![A shard leképezés kezelése](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Alapvető tooshard térkép felügyeleti megértése, hogyan össze ezeket a leképezéseket. Ebben az esetben hello segítségével [ShardMapManager osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), míg a talált a hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) toomanage shard képezi le.  

## <a name="shard-maps-and-shard-mappings"></a>A shard leképezések és shard hozzárendelések
Az egyes shard shard térkép toocreate hello típusú kell választania. hello választáshoz hello adatbázis-architektúra: 

1. Adatbázisonként egybérlős  
2. Több bérlő adatbázisonként (két típus):
   1. Lista leképezése
   2. Tartomány leképezése

Single-bérlő modell, hozzon létre egy **lista leképezési** shard leképezés. hello single-bérlős modell bérlőnként egy adatbázis rendeli hozzá. Ez megegyezik egy hatékony modell Szolgáltatottszoftver-fejlesztőknek egyszerűbbé teszi a felügyeleti.

![Lista leképezése][1]

hello több-bérlős modell rendeli hozzá több bérlő tooa egyetlen adatbázis (és a bérlő csoportok szét több adatbázis). Ezt a modellt használja, ha várhatóan mindegyik bérlő toohave kis adatokat. Ez a modell azt rendelje hozzá a bérlők tooa adatbázis használatával széles **tartomány leképezése**. 

![Tartomány leképezése][2]

Megvalósíthat egy adatbázis több-bérlős modell használatával vagy egy *lista leképezési* tooassign egy adatbázis több bérlő tooa. Például D1 bérlőazonosító 1 és 5 használt toostore információt, és DB2 eltárolja 7 bérlői és bérlői 10. 

![Az egyetlen DB Muliple bérlők][3] 

### <a name="supported-net-types-for-sharding-keys"></a>A horizontális kulcsok támogatott .net-típusok
Rugalmas méretezési támogatási hello a következő .net Framework meg kell adnia horizontális kulcsként:

* egész szám
* hosszú
* GUID
* Byte]  
* Dátum és idő
* A TimeSpan
* datetimeoffset

### <a name="list-and-range-shard-maps"></a>Lista és a tartomány shard leképezések
A shard maps használatával lehet létrehozni **listák az egyes horizontális kulcsértékek**, vagy azok lehet létrehozni használatával **horizontális tartományok kulcsértékek**. 

### <a name="list-shard-maps"></a>Lista shard maps
**Szilánkok** tartalmazhat **shardlets** és shardlets tooshards hello leképezése shard térképre tartja fenn. A **lista shard térkép** hello egyedi értékek hello shardlets azonosítására és szilánkok szolgálhat hello adatbázisok közötti társítás.  **Hozzárendelések listában** egyértelmű és különböző értékeit leképezett toohello lehet ugyanazt az adatbázist. Például kulcs 1 tooDatabase A maps, és 3. és 6 mindkét kulcsértékei hivatkozást adatbázis a b kiszolgálóra.

| Kulcs | A shard helye |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Tartomány shard maps
Az egy **tartomány shard térkép**, hello kulcs tartomány leírt két **[érték alacsony, magas érték)** ahol hello *alacsony értéket* hello minimális kulcs a hello tartományt és hello *Értékes* hello első érték nagyobb, mint hello tartományon. 

Például **[0, 100)** nagyobb vagy egyenlő 0 és 100-nál kisebb összes egész számokat tartalmaz. Vegye figyelembe, hogy a több tartomány is pont toohello azonos adatbázisából, és a tartományok különálló támogatottak-e (pl. [100,200) és a [400,600) mindkét pont tooDatabase C az alábbi példa hello.)

| Kulcs | A shard helye |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

A fenti hello táblák egy általános példát egy **ShardMap** objektum. Minden egyes sorára egy adott egyszerűsített példát **PointMapping** (az hello lista shard leképezés) vagy **RangeMapping** (a tartomány shard térkép hello) objektum.

## <a name="shard-map-manager"></a>A shard térkép manager
Hello ügyfél könyvtárban hello shard térkép manager gyűjteménye shard leképezések. hello által kezelt egy **ShardMapManager** példány tartják három helyen: 

1. **Globális Shard térkép (GSM)**: egy adatbázis tooserve hello tárháza összes shard leképezéseket és leképezéseket ad meg. Különleges táblák és tárolt eljárások automatikusan létrejönnek toomanage hello információkat. Ez általában egy kis adatbázist, és könnyebb érhető el, és azt nem használható más hello alkalmazás igényeinek. hello táblák vannak különleges a séma nevű **__ShardManagement**. 
2. **Helyi Shard térkép (LSM)**: minden adatbázis egy shard van toobe módosított toocontain több kis táblák és speciális tárolt eljárások, melyek és shard térkép információt adott toothat shard kezelése. Ezekkel az információkkal már redundáns és hello GSM hello adatait, és anélkül, hogy a terhelés a hello GSM; lehetővé teszi hello alkalmazás gyorsítótárazott toovalidate shard leképezési adatai hello alkalmazás hello LSM toodetermine használja, ha egy gyorsítótárazott leképezés továbbra is érvényes. hello táblázatot is hello séma vannak az egyes shard LSM megfelelő toohello **__ShardManagement**.
3. **Alkalmazás-gyorsítótárának**: minden egyes alkalmazás példány elérése a **ShardMapManager** objektum a leképezéseket egy helyi memóriabeli gyorsítótárában megtalálhatók. Útvonal-információkat, amelyek a legutóbb beolvasott tárol. 

## <a name="constructing-a-shardmapmanager"></a>Egy ShardMapManager létrehozása
A **ShardMapManager** objektum segítségével jön létre egy [gyári](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) mintát. Hello  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metódus hitelesítő adatait (beleértve a hello kiszolgáló és adatbázis nevét okozó hello GSM) formájában hello időt vesz igénybe. a  **ConnectionString** és egy példányát adja vissza egy **ShardMapManager**.  

**Ne feledje:** hello **ShardMapManager** kell példányosítani csak egyszer app tartományonként hello inicializálási kód egy alkalmazáshoz. Létrehozása a hello ShardMapManager további példányának egyazon appdomain eredményez nagyobb memória és CPU-felhasználás hello alkalmazás. A **ShardMapManager** shard maps tetszőleges számú tartalmazhat. Lehet, hogy elegendő-e számos alkalmazás egyetlen shard térképre, amíg nincsenek alkalommal, ha az adatbázisok más-más részhalmazához használt különböző séma vagy egyedi célokra; azokban az esetekben érdemes lehet több shard maps. 

Ebben a kódban, egy alkalmazás egy meglévő tooopen megpróbál **ShardMapManager** a hello [TryGetSqlShardMapManager metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).  Ha egy globális objektumokból **ShardMapManager** (GSM) létezhet hello adatbázis még nem, hello ügyféloldali kódtár létrehozásuk nincs hello segítségével [CreateSqlShardMapManager metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

Alternatív megoldásként használhatja a Powershell toocreate egy új Shard térkép Manager. Példa: elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Szerezze be a RangeShardMap vagy ListShardMap
Miután létrehozta a shard térkép kezelő, kaphat a hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) vagy [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) hello segítségével [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), vagy hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metódust.

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a>A shard térkép felügyeleti hitelesítő adatok
Alkalmazások, amelyek felügyelheti és kezelheti a shard maps különböznek hello shard maps tooroute kapcsolatok használó. 

tooadminister shard leképezi (hozzáadása vagy módosítása a szilánkok, shard maps, shard hozzárendelések, stb.) példányosítania kell hello **ShardMapManager** használatával **hitelesítő adatokat, amelyek olvasási/írási jogosultsággal mindkét hello GSM adatbázison, és minden egyes adatbázis, amely egy shard funkcionál**. hello hitelesítő adatok lehetővé kell tennie a mindkét hello hello táblák írására GSM és LSM shard megfeleltetési adatokat a megadott, vagy megváltozott, valamint LSM-táblázatok létrehozása az új szilánkok esetében.  

Lásd: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Csak az érintett metaadatok
Feltöltése vagy módosítása hello használt módszerek **ShardMapManager** adatok nem módosítja a hello szilánkok maguk tárolt hello felhasználói adatokat. Többek között például módszerek **CreateShard**, **DeleteShard**, **UpdateMapping**stb hello shard ábrázolási metaadatainak csak hatással. Ezek nem távolítható el, adja hozzá vagy alter hello szilánkok szereplő felhasználói adatokat. Ehelyett az alábbi módszerek egyike használt tervezett toobe külön műveletek együtt toocreate végrehajtani, vagy távolítsa el a tényleges adatbázisok, vagy ez a lépés sor egy shard tooanother toorebalance innen a szilánkos környezetekben.  (hello **vegyes egyesítéses** rugalmas adatbáziseszközöket eszköz lehetővé teszi, hogy ezek API-k között szilánkok adatok tényleges mozgatását koordinálása együtt használja.) Lásd: [méretezési hello rugalmas adatbázis vegyes egyesítéses eszközzel](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="populating-a-shard-map-example"></a>A szilánkok térkép példa feltöltése
Egy parancssorozat-példa műveletek toopopulate egy adott shard térkép alább láthatók. hello kód végrehajtja ezeket a lépéseket: 

1. Új shard leképezés egy shard térkép manager belül jön létre. 
2. két különböző szilánkok hello metaadatai toohello shard térkép kerül. 
3. Kulcs tartomány hozzárendelések számos ad hozzá, és hello teljes tartalma hello shard térkép jelenik meg. 

hello kód írása, hogy hello metódus is futtatható, ha a hiba akkor fordul elő. Minden egyes kérelem teszteli, hogy a shard vagy a hozzárendelés már létezik, mielőtt megpróbálná toocreate azt. hello kód azt feltételezi, hogy az adatbázisok nevű **sample_shard_0**, **sample_shard_1** és **sample_shard_2** létre lett hozva a hello server karakterlánc hivatkozik**shardServer**. 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

Mivel helyett használhatja a PowerShell parancsfájlok tooachieve hello azonos vezethet. Hello minta PowerShell-példák némelyike elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).     

Után shard maps vannak-e töltve, az adatok alkalmazásokat hozhatók létre, vagy a hello Maps Közösséggel toowork leírtakon. Feltöltése vagy módosítása hello maps kell nem fordulhat elő, amíg újra nem **térkép elrendezés** toochange kell.  

## <a name="data-dependent-routing"></a>Adatfüggő útválasztás
hello shard térkép manager legtöbb használandó adatbázis-kapcsolatok tooperform hello alkalmazás-specifikus adatok műveletek igénylő alkalmazások. Ezeket a kapcsolatokat hello megfelelő adatbázis társítani kell. Ez más néven **adatok függő útválasztási**. Az alkalmazás hozható létre a shard leképezési manager objektum, amely csak olvasási hozzáféréssel rendelkezik a hello GSM adatbázis hitelesítő adataival hello gyárból származó. Egyes kérelmeket a későbbi kapcsolatok toohello megfelelő shard adatbázis csatlakozáshoz szükséges hitelesítő adatokat fogja tartalmazni.

Vegye figyelembe, hogy ezek az alkalmazások (használatával **ShardMapManager** olvasási jogosultságokkal megnyitni) nem lehet módosítani toohello leképezéseket és leképezéseket. Hozzon létre felügyeleti-specifikus alkalmazások vagy a PowerShell-parancsfájlok, amelyek magasabb jogosultsági szintű hitelesítő adatokat a fentiekben taglaltak ezeket az igényeket. Lásd: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md).

További részletekért lásd: [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>A szilánkok térkép módosítása
A shard térképre különböző módokon lehet megváltoztatni. A következő módszerek hello mindegyikét: hello szilánkok és a hozzájuk tartozó leképezések hello metaadatok módosításához, de hello szilánkok adatainak fizikailag nem módosíthatják, és nem tegye azokat létrehozása vagy törlése hello tényleges adatbázisok.  Az alábbiakban hello shard térképen hello műveleteket esetleg toobe koordinált, amely fizikailag helyezi át adatokat, vagy hozzáadására és eltávolítására szolgáló szilánkok adatbázisok, amelyek a felügyeleti műveletekhez.

Ezek a módszerek működnek együtt a módosítását a rendelkezésre álló hello építőelemeiként hello a szilánkos adatbázis környezetében adatok teljes terjesztési.  

* tooadd vagy az eltávolítási szilánkok: használja  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  és  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  a hello [Shardmap osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx). 
  
    hello kiszolgáló és az adatbázis hello cél shard képviselő már léteznie kell a műveletek tooexecute. Ezek a módszerek nem rendelkezik gyakorolt hatás hello adatbázisok, csak a metaadatok hello shard leképezés.
* pontok toocreate, vagy távolítsa el vagy tartományokat toohello szilánkok leképezése: használja  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  a hello [RangeShardMapping osztály](https://msdn.microsoft.com/library/azure/dn807318.aspx), és  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  a hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)
  
    Számos különböző pontok vagy tartományok lehet csatlakoztatott toohello ugyanazt a shard. Ezek a módszerek csak hatással vannak a metaadat - adatokat, amelyek esetleg már szerepel a szilánkok nem befolyásolják. Ha adatokat kell-e eltávolítva a rendelés toobe konzisztens hello adatbázisból toobe **DeleteMapping** műveletek, szüksége lesz tooperform ezeket a műveleteket külön-külön, de ezekkel a módszerekkel együtt.  
* létező tartományok toosplit kettő vagy egyesítés szomszédos tartományok valamelyikébe: használja  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  és  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.  
  
    Vegye figyelembe, hogy ossza fel, és egyesíti az műveletek **ne változtassa meg hello shard toowhich kulcs képezi le**. Valószínűségét egy meglévő tartományt bontja két részből áll, de hagyja mindkét csatlakoztatott toohello, ugyanazt a shard. Két szomszédos tartományokat, amelyek már csatlakoztatott toohello működik egyesítésével ugyanazt a shard, egyesítése őket egy egyetlen tartományba.  hello pontok vagy közötti szilánkok maguk tartományok kell használatával koordinált toobe **UpdateMapping** adatok tényleges mozgatását együtt.  Használhatja a hello **vegyes/Egyesítés** rugalmas adatbázis részét képező szolgáltatás eszközei toocoordinate shard térkép módosításokat adatmozgás, amikor szükség van a mozgás. 
* toore-társítási (vagy áthelyezése) egyes pontok vagy tartományok toodifferent szilánkok: használja  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.  
  
    Mivel adatok esetleg egy shard tooanother rendelés toobe konzisztensek legyenek az áthelyezett toobe **UpdateMapping** műveletek, szüksége lesz tooperform, hogy a mozgás külön-külön, de ezekkel a módszerekkel együtt.
* online és offline tootake leképezéseket: használja  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  és  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online állapotát egy leképezés. 
  
    A shard hozzárendelések bizonyos műveletek csak vannak engedélyezve, ha a leképezés nem "offline" állapotú, beleértve a **UpdateMapping** és **DeleteMapping**. Ha a leképezés offline állapotban, a kulcs szerepel a leképezéseket alapuló adatok függő kérelem hibát adnak vissza. Emellett egy tartomány első offline állapotba kerül, ha minden kapcsolatok hatással toohello shard vannak automatikusan levágni tooprevent inkonzisztens vagy hiányos eredményeket lekérdezésekhez az módosítás alatt tartományok szemben. 

Hozzárendelések nem módosítható objektumokat a .net.  Az összes hello módszerek fenti leképezések módosításához bármely hivatkozások toothem a kódban is érvénytelenné válnak. toomake állapotváltozáshoz a leképezést, minden módosítása, leképezés hello módszerek műveletek könnyebb tooperform sorozatát adja vissza egy új informatikai leképezési hivatkozást, így műveletek elérése érdekében. Például egy meglévő leképezés az shardmap sm 25 hello kulcsot tartalmazó toodelete, hajthat végre a következő hello: 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a>A szilánkok hozzáadása
Alkalmazások gyakran kell toosimply új szilánkok toohandle adatok hozzáadása új kulcsokat vagy kulcstartományokkal, várt shard térképre már létezik. Például a bérlő-azonosító szerint szilánkos alkalmazás egy új shard tooprovision szükséges egy új bérlőt, vagy adatok szilánkos havi esetleg egy új shard kiépítése előtt minden új hónap elején hello. 

Ha hello új értékek tartománya nem már része egy meglévő hozzárendelést és nélküli adatátvitelt jelölik a nagyon egyszerű tooadd hello új shard szükség, társítson hello kulcsot vagy tartomány toothat szilánkcímtárban. Új szilánkok hozzáadásával kapcsolatos részletekért lásd: [hozzáadása egy új shard](sql-database-elastic-scale-add-a-shard.md).

Adatátvitel igénylő forgatókönyvek esetén azonban hello vegyes egyesítéses eszköze szükséges tooorchestrate hello adatátvitelt jelölik a szilánkok hello szükséges shard leképezési frissítések együtt között. Információk hello vegyes egyesítéses yool, lásd: [vegyes egyesítéses áttekintése](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
