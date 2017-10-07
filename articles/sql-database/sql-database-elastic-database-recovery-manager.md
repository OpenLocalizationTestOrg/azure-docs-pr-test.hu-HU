---
title: "aaaUsing helyreállítás-kezelő toofix szilánkok leképezése problémák |} Microsoft Docs"
description: "Hello RecoveryManager osztály toosolve problémák shard maps használata"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 45520ca3-6903-4b39-88ba-1d41b22da9fe
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: ddove
ms.openlocfilehash: 2218fb15122f1df466e65483480461e366317f2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-recoverymanager-class-toofix-shard-map-problems"></a>Hello RecoveryManager osztály toofix shard térkép problémák használatával
Hello [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) osztály lehetővé teszi a ADO.Net hello képességét tooeasily észleli, és kijavíthatja az hello globális shard térkép (GSM) és hello helyi shard térkép (LSM) szilánkos adatbázis környezetben közötti inkonzisztenciákat. 

hello GSM és LSM követése hello hozzárendelése minden egyes adatbázis szilánkos környezetben. Alkalmanként szünet hello GSM és hello LSM között történik. Ebben az esetben hello RecoveryManager osztály toodetect használja, és javítsa hello sortörés.

hello RecoveryManager osztály hello része [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md). 

![A shard térkép][1]

Kifejezés-definíciók, lásd: [rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md). Hogyan hello toounderstand **ShardMapManager** használt toomanage adatok a szilánkos megoldás lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md).

## <a name="why-use-hello-recovery-manager"></a>Miért érdemes használni a helyreállítás-kezelő hello?
Horizontálisan skálázott adatbázis környezetben nincs adatbázisonként egy bérlői, és számos más adatbázis kiszolgálónként. Lehetnek is sok kiszolgálók hello környezetben. Minden egyes adatbázis hozzá van rendelve hello shard leképezés, így hívások lehet irányított toohello megfelelő kiszolgálót és adatbázist. Adatbázisok tooa szerint követi **horizontális kulcs**, és minden shard hozzá van rendelve egy **értékek tartományán**. Például egy horizontális skálázási kulcs képviselhet hello ügyfélnevek "D" túl "F." hello (más néven adatbázisok) összes szilánkok leképezése és hello található a leképezési tartományok **globális shard térkép (GSM)**. Minden adatbázis is tartalmaz hello shard szerint hello tartalmazott hello tartományait térképre **helyi shard térkép (LSM)**. Amikor egy alkalmazás tooa shard csatlakozik, a rendszer gyorsítótárazza a hello leképezési hello alkalmazással a gyors beolvasásához. hello LSM használt toovalidate gyorsítótárazott adatokat. 

hello GSM és LSM válhat a következő okok miatt hello szinkronban:

1. egy amelynek tartomány várakozások toono hosszabb ideig szilánkcímtárban hello törlésének kell használatával, vagy átnevezésének a szilánkcímtárban. Egy shard törlése eredményezi egy **árván maradt a szilánkok leképezése**. Hasonlóképpen a átnevezett adatbázis egy árva szilánkok leképezése okozhat. Attól függően, hogy hello szándékával hello módosítása hello shard eltávolított toobe van szüksége, vagy hello shard hely toobe frissíteni kell. a törölt adatbázisok toorecover lásd [törölt adatbázis visszaállítása](sql-database-recovery-using-backups.md).
2. A földrajzi-feladatátadási esemény következik be. toocontinue, egy frissítenie kell hello kiszolgáló nevét, és shard térképen az összes szilánkok hello alkalmazást, majd a frissítés hello szilánkok leképezése részletei shard térkép Manager-adatbázis neve. A földrajzi feladatátvétel esetén ilyen helyreállítási logika érdemes automatizálni hello feladatátvételi munkafolyamaton belül. Helyreállítási műveletek automatizálása lehetővé teszi, hogy a frictionless kezelhetőségi adatbázisok földrajzi engedélyezve van, és ezzel elkerülheti a manuális emberi műveletek. egy adatbázist, ha egy adatközpont-meghibásodás után, olvassa el a beállítások toorecover kapcsolatos toolearn [az üzletmenet folytonossága](sql-database-business-continuity.md) és [vész-helyreállítási](sql-database-disaster-recovery.md).
3. A shard vagy hello ShardMapManager adatbázisa visszaállított tooan korábbi pont idő. toolearn időpontra történő visszaállítás biztonsági mentések használatával kapcsolatban lásd: [biztonsági másolatokból helyreállítási](sql-database-recovery-using-backups.md).

Azure SQL Database rugalmas adatbáziseszközöket, georeplikáció és visszaállításával kapcsolatban további információkért lásd: hello következő: 

* [Áttekintése: A felhő üzleti folytonossági és az adatbázis katasztrófa utáni helyreállítás az SQL Database](sql-database-business-continuity.md) 
* [Ismerkedés a rugalmas adatbázis-eszközök](sql-database-elastic-scale-get-started.md)  
* [ShardMap kezelése](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Egy ShardMapManager RecoveryManager lekérése
hello első lépéseként toocreate egy RecoveryManager példány. Hello [GetRecoveryManager metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) értéket ad vissza az aktuális hello helyreállítás-kezelő hello [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) példány. tooaddress esetlegesen bekövetkező Inkonzisztencia a hello szilánkok leképezése, hello RecoveryManager hello adott shard térkép először be kell olvasni. 

   ```
    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 
   ```

Az ebben a példában az hello RecoveryManager hello ShardMapManager az inicializálása. egy ShardMap tartalmazó ShardMapManager hello is már inicializálva van. 

Az alkalmazás kódjában hello shard térkép maga kezeli, mivel hello hello gyártómetódussal során használt hitelesítő adatok (az előző példában hello smmConnectionString) hitelesítő adatokat, amelyek hello által hivatkozott hello GSM adatbázis írási / olvasási engedélyeket kell lennie. kapcsolati karakterlánc. Ezek a hitelesítő adatok általában eltérnek a használt hitelesítő adatok tooopen kapcsolatok az adatok függő továbbításához. További információkért lásd: [hello rugalmas adatbázis-ügyfél a hitelesítő adatok használatával](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-hello-shardmap-after-a-shard-is-deleted"></a>A szilánkok eltávolítása hello ShardMap egy shard törlése után
Hello [DetachShard metódus](https://msdn.microsoft.com/library/azure/dn842083.aspx) szervezőről a megadott shard hello shard leképezés hello és hello shard társított hozzárendelések törlése.  

* hello hely paraméter hello shard helyét, kifejezetten a kiszolgáló neve és az adatbázis nevét, a hello shard leválasztás alatt áll. 
* hello shardMapName paraméter hello shard leképezés neve. Ez a tulajdonság csak akkor szükséges, ha több shard maps hello által kezelt ugyanazt a shard térkép manager. Választható. 


> [!IMPORTANT]
> Ez a módszer akkor használja, csak akkor, ha biztos benne, hogy hello frissítése hello leképezés értéke üres. a fenti hello módszerek hello tartomány áthelyezett adatok ellenőrzése, a legjobb tooinclude ellenőrzi a kódban.
>

Ez a példa szilánkok hello shard térkép eltávolítja. 

   ```
   rm.DetachShard(s.Location, customerMap);
   ``` 

hello shard térkép tükrözi hello shard helyét hello GSM hello shard hello törlése előtt. Hello shard törölve lett, mert van feltételezi, hogy ez szándékos volt, és hello horizontális kulcs tartomány már nem használja. Ha nem, a visszaállítás egy korábbi időpontra pont a hajthat végre. toorecover hello shard a egy korábbi-időpontban. (Ebben az esetben tekintse át a következő szakasz toodetect shard inkonzisztenciát hello.) toorecover, lásd: [időpontra történő visszaállítás](sql-database-recovery-using-backups.md).

Feltételezzük, hogy szándékos volt hello adatbázisok törlése, mivel hello végső felügyeleti törlési művelete toodelete hello bejegyzés toohello shard hello shard térkép Manager. Ez megakadályozza, hogy a nem várt információkat tooa tartomány véletlenül írásában hello alkalmazás.

## <a name="toodetect-mapping-differences"></a>toodetect leképezési különbségek
Hello [DetectMappingDifferences metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) kiválasztása és igazság hello adatforrásként hello shard maps (helyi vagy globális) egyikét adja vissza, és mindkét shard maps (GSM és LSM) leképezése rendezheti.

   ```
   rm.DetectMappingDifferences(location, shardMapName);
   ```

* Hello *hely* hello kiszolgáló és a nevét adja meg. 
* Hello *shardMapName* paraméter hello shard leképezés neve. Erre csak akkor van szükség, ha több shard maps hello által kezelt ugyanazt a shard térkép manager. Választható. 

## <a name="tooresolve-mapping-differences"></a>tooresolve leképezési különbségek
Hello [ResolveMappingDifferences metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) igazság hello adatforrásként kiválaszt egy hello shard maps (helyi vagy globális), és mindkét shard maps (GSM és LSM) leképezése összehangolja.

   ```
   ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   ```

* Hello *RecoveryToken* paraméter enumerálása hello hello leképezések és közötti különbségeket hello GSM hello LSM a hello adott szilánkcímtárban. 
* Hello [MappingDifferenceResolution számbavételi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) használt tooindicate hello módszer kapcsolatos hello shard hozzárendelések hello különbsége. 
* **MappingDifferenceResolution.KeepShardMapping** ajánlott, ha hello LSM tartalmazza a hello pontos, ezért hello shard hello leképezését használható. Ez helyzet általában hello feladatátvétel esetén: hello shard mostantól egy új kiszolgálón található. Hello shard először el kell távolítani hello GSM (hello RecoveryManager.DetachShard módszer alkalmazásával), mert a leképezés már nem létezik, a hello GSM. Emiatt hello LSM kell lennie a használt toore-hello shard leképezés létrehozásához.

## <a name="attach-a-shard-toohello-shardmap-after-a-shard-is-restored"></a>A szilánkok toohello ShardMap csatolni egy shard történt visszaállítása után
Hello [AttachShard metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) rendeli hello adott shard toohello shard leképezés. Ezután shard térkép inkonzisztenciákat észlelt, és hello hozzárendelések toomatch hello shard hello shard visszaállítás hello pontján frissíti. Feltételezzük, hogy hello az adatbázis egyben átnevezett tooreflect hello eredeti adatbázisnév (előtt hello shard vissza lett állítva), mert hello pont ideje visszaállítás alapértelmezés szerint használt érték hozzáíródik hello időbélyeg tooa új adatbázis. 

   ```
   rm.AttachShard(location, shardMapName)
   ``` 

* Hello *hely* paraméter hello kiszolgáló és adatbázis nevét, az éppen csatolt hello szilánkcímtárban. 
* Hello *shardMapName* paraméter hello shard leképezés neve. Ez a tulajdonság csak akkor szükséges, ha több shard maps hello által kezelt ugyanazt a shard térkép manager. Választható. 

Ebben a példában a shard toohello shard térképen nemrég helyreállt a pont a korábbi időpontra hozzáadja. Hello shard (azaz a hello LSM hello szilánkok leképezése hello) vissza lett állítva, mert nem potenciálisan következetes hello GSM hello shard bejegyzése. Ez a példa kód kívül hello shard vissza lett állítva, és átnevezett toohello hello adatbázis eredeti neve. Vissza lett állítva, mivel feltételezzük hello LSM hello leképezését hello megbízható leképezés. 

   ```
   rm.AttachShard(s.Location, customerMap); 
   var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 
   ```

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-hello-shards"></a>Egy földrajzi-feladatátvételt követően (visszaállítás) a hello szilánkok shard helyek frissítése
A földrajzi feladatátvétel esetén a hello másodlagos adatbázis írási elérhető lesz, és hello új elsődleges adatbázis válik. hello hello kiszolgálójának nevét, és potenciálisan hello adatbázis (attól függően, hogy a konfiguráció), előfordulhat, hogy különböző hello eredeti elsődleges. Ezért hello hello shard hello GSM a leképezési bejegyzést, és LSM javítani kell. Hasonlóképpen, ha hello adatbázisa visszaállított tooa másik nevet vagy helyet vagy tooan idő korábbi pont, ez lehet, hogy inkonzisztenciát okozhat hello shard leképezések. hello Shard térkép Manager hello terjesztési nyitott kapcsolatok toohello megfelelő adatbázis kezeli. Terjesztési hello shard és hello horizontális kulcs, amely hello cél hello alkalmazások kérelem hello értékének hello adatokon alapul. Egy földrajzi-feladatátvétel után ezek az információk hello pontos kiszolgáló nevét, az adatbázisnév és a szilánkok leképezése hello helyreállított adatbázis frissíteni kell. 

## <a name="best-practices"></a>Ajánlott eljárások
Földrajzi-feladatátvételi és helyreállítási műveletek általában kezeli a felhő rendszergazdájának hello alkalmazás szándékosan okhoz egy Azure SQL-adatbázisok üzleti folytonosságot biztosító szolgáltatásokat, amelyek. Üzleti folytonossági tervezési folyamatok, eljárások és intézkedések tooensure, amely az üzleti tevékenységre megszakítás nélkül továbbra is szükséges. hello módszer használható, mert hello RecoveryManager osztály részét kell használni a munkahelyi folyamat tooensure hello belül GSM és LSM mindig naprakészek alapján hello helyreállítási műveletről. Öt alapvető lépést hello biztosítása tooproperly GSM és LSM tükrözik hello pontos információk lekérdezésének a feladatátadási esemény után. Ezeket a lépéseket integrálható a meglévő eszközök és a munkafolyamat alkalmazás kód tooexecute hello. 

1. Hello ShardMapManager hello RecoveryManager lekérni. 
2. Válassza le a régi shard hello hello shard leképezés.
3. Rendeljen a hello új shard toohello shard leképezés, többek között a hello shard helyre.
4. Leképezési közötti hello GSM és LSM hello inkonzisztenciát észlelését. 
5. Szüntesse meg a globális szolgáltatásfigyelő szolgáltatás hello és hello LSM, megbízó hello LSM közötti különbséget. 

Ebben a példában a lépéseket követve hello hajtja végre:

1. Szilánkok eltávolítása hello Shard térkép, hogy tükrözze a shard helyek hello feladatátvétel megtörténte előtt.
2. Csatolja a szilánkok toohello Shard térkép tükröző hello shard helyét (hello paraméter "Configuration.SecondaryServer" hello új kiszolgáló neve, de azonos hello adatbázis neve).
3. Lekéri a hello helyreállítási jogkivonatok hello GSM és hello LSM az egyes szilánkok leképezése különbségei észlelésével. 
4. Oldja fel a hello inkonzisztenciát által az egyes shard LSM hello megbízó hello-leképezés. 
   
   ```
   var shards = smm.GetShards(); 
   foreach (shard s in shards) 
   { 
     if (s.Location.Server == Configuration.PrimaryServer) 
   
         { 
          ShardLocation slNew = new ShardLocation(Configuration.SecondaryServer, s.Location.Database); 
   
          rm.DetachShard(s.Location); 
   
          rm.AttachShard(slNew); 
   
          var gs = rm.DetectMappingDifferences(slNew); 
   
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 
   ```

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png

