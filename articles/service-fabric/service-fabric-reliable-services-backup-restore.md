---
title: "aaaService háló biztonsági mentése és visszaállítása |} Microsoft Docs"
description: "Service Fabric biztonsági mentési és visszaállítási fogalmi dokumentációja"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,jessebenson
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: mcoskun
ms.openlocfilehash: e502b59c84999c3fe825167383f00a5ebd70c9b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-restore-reliable-services-and-reliable-actors"></a>Biztonsági mentése és visszaállítása a Reliable Services és Reliable Actors
Az Azure Service Fabric egy magas rendelkezésre állású platformon keresztül replikálásra kerülő hello állapot több csomópontok toomaintain a magas rendelkezésre állású.  Így még akkor is, ha hello fürtben egy csomópont meghibásodik, a hello szolgáltatások elérhető toobe továbbra is. Amikor ezt a beépített redundanciát hello platform által biztosított lehet, hogy elegendő-e egyes, bizonyos esetekben kívánatos hello szolgáltatás tooback adatokat (tooan külső áruházban).

> [!NOTE]
> Kritikus toobackup, és állítsa vissza az adatait (és tesztelje, hogy a várt módon működik), akkor helyreállíthatja az adatvesztés.
> 
> 

Például egy szolgáltatás érdemes tooback rendelés tooprotect a következő forgatókönyvek hello adatokat:

- A teljes Service Fabric-fürt hello végleges adatvesztést hello eseményben.
- Szolgáltatás partíció hello replikák többsége végleges adatvesztést
- Felügyeleti hibák, amelyek hello állapot véletlenül lekérdezi törölt vagy sérült. Ez például akkor fordulhat elő, ha egy megfelelő jogosultsággal rendelkező rendszergazda tévesen törli hello szolgáltatást.
- Hibák hello szolgáltatásban adatvesztést okozhat. Ez például akkor fordulhat elő, a kód frissítése indításakor hibás adatok tooa megbízható gyűjtemény írása. Ebben az esetben is hello kódot, és hello adatok lehet toobe tooan vissza korábbi állapot.
- Kapcsolat nélküli adatok feldolgozása. Előfordulhat, hogy kényelmes toohave offline üzleti intelligencia, külön-külön történik, hello szolgáltatásból való, hello adatokat állít elő az adatok feldolgozása.

hello biztonsági mentési/visszaállítási funkció lehetővé teszi, hogy a szolgáltatások hello megbízható szolgáltatások API toocreate és visszaállítási biztonsági mentést készített. hello hello platform által biztosított biztonsági mentési API-k engedélyezése biztonsági mentését vagy mentéseit szolgáltatás partíció állapot nélkül blokkoló olvasási vagy írási műveleteket. hello visszaállítási API-k engedélyezése egy szolgáltatás partíció állapot toobe választott másolatból állítottak vissza.

## <a name="types-of-backup"></a>Biztonsági mentés típusai
Két biztonsági mentési lehetőség: teljes és növekményes.
Teljes biztonsági mentés egy összes hello szükséges adatok toorecreate hello állapotának hello replika magában foglaló biztonsági másolat: ellenőrzőpontok és az összes naplófájl rögzíti.
Hello ellenőrzőpontok és hello naplófájl van, mert egy teljes biztonsági mentés önállóan állíthatók vissza.

a teljes biztonsági mentés hello probléma merül fel, ha nagy hello ellenőrzőpontokat.
Például egy replikát, amelynek állapota 16 GB lesz, amely körülbelül too16 GB egyezzen az ellenőrzőpontokat.
Ha öt percig Helyreállításipont-célkitűzés van, a hello replikának szüksége lesz a biztonsági másolat, ötpercenként toobe.
Toocopy kell minden alkalommal, amikor biztonsági mentést készít az ellenőrzőpontok továbbá 16 GB too50 MB (konfigurálható használatával `CheckpointThresholdInMB`) értéke a naplók.

![Teljes biztonsági mentés példa.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

hello megoldás toothis probléma a növekményes biztonsági mentést, ha biztonsági mentést csak tartalmaz megváltozott hello naplórekordokat hello utolsó biztonsági mentés óta.

![Növekményes biztonsági mentési példa.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Mivel a növekményes biztonsági mentések csak történt változások hello utolsó biztonsági mentés óta (nem tartalmaz hello ellenőrzőpontokat), toobe gyorsabban általában, de azok nem állítható vissza, hogy saját maguk.
toorestore növekményes biztonsági mentést, a teljes biztonsági mentési láncolatát hello szükség.
A biztonsági mentési láncolatát lánc csatlakozik a biztonsági mentések teljes biztonsági mentés kezdve, és egy egybefüggő növekményes biztonsági mentések számát.

## <a name="backup-reliable-services"></a>Biztonsági mentési megbízható szolgáltatások
hello szolgáltatás Szerző teljes körű vezérléssel rendelkezik a toomake biztonsági mentések és a biztonsági másolatok tárolásához.

biztonsági másolat toostart, hello szolgáltatást kell tooinvoke hello örökölt tagot függvény `BackupAsync`.  
Biztonsági mentések tehető csak az elsődleges replikára változott, és igényelnek-e írási állapot toobe kap.

Az alábbi módon `BackupAsync` veszi a `BackupDescription` objektum, ahol egy adhat meg egy teljes vagy növekményes biztonsági mentés, valamint a visszahívási függvény, `Func<< BackupInfo, CancellationToken, Task<bool>>>` , amely nyílik meg, ha a biztonsági mentési mappája hello helyileg létrejött, és készen áll a toobe kimenő toosome áthelyezése külső tárhelyen.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

A növekményes biztonsági mentés meghiúsulhat kérelem tootake `FabricMissingFullBackupException`. Ezt a kivételt azt jelzi, hogy a következő dolgot hello egyik történik:

- hello replika soha nem tartott egy teljes biztonsági mentés óta elsődleges, vált
- Néhány hello rekordok naplózására, mivel a rendszer csonkolta a hello utolsó biztonsági mentés vagy
- az átadott hello replika `MaxAccumulatedBackupLogSizeInMB` korlátot.

Felhasználók megnövelik hello valószínűségét, hogy az képes toodo növekményes biztonsági mentések konfigurálásával `MinLogSizeInMB` vagy `TruncationThresholdFactor`.
Vegye figyelembe, hogy ezek az értékek növelése növeli a hello / replika lemezek használata terén.
További információkért lásd: [megbízható konfigurálása](service-fabric-reliable-services-configuration.md)

`BackupInfo`hello biztonsági mentés, beleértve a hello hello mappát, ahol hello futásidejű hello biztonsági mentés vonatkozóan nyújt információkat (`BackupInfo.Directory`). hello visszahívási függvény áthelyezheti hello `BackupInfo.Directory` tooan külső tároló vagy egy másik helyen.  Ez a funkció is egy logikai érték, amely jelzi, hogy volt-e képes toosuccessfully áthelyezés hello biztonsági mentési mappája tooits célhelyet adja vissza.

hello következő kód bemutatja, hogyan hello `BackupCallbackAsync` metódus lehet használt tooupload hello biztonsági mentési tooAzure tároló:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

Hello portáladatbázis előző példában `ExternalBackupStore` hello minta osztály, amely az Azure Blob storage szolgáltatással használt toointerface és `UploadBackupFolderAsync` hello módszer, amelynek hello mappa tömöríti, és elhelyezi hello Azure Blob-tárolóban.

Vegye figyelembe:

  - Lehetnek csak egy biztonsági mentési művelet egy replika az üzenetsoroktól egy adott időpontban. Egynél több `BackupAsync` egyszerre hívás kivételhibát `FabricBackupInProgressException` toolimit aktív biztonsági mentések tooone.
  - Ha egy replika átadja, amíg folyamatban van egy biztonsági mentés, hello biztonsági mentés lehetséges, hogy nem már befejeződött. Ebből kifolyólag hello feladatátvétel befejezése után is hello szolgáltatás feladata toorestart hello biztonsági mentés figyelőn `BackupAsync` szükség szerint.

## <a name="restore-reliable-services"></a>Megbízható szolgáltatások visszaállítása
Általában hello esetekben, amikor előfordulhat, hogy a visszaállítási művelet tooperform egyikébe ezen kategóriák:

  - hello szolgáltatás elveszett adatok particionálása. Például hello lemeze (beleértve az elsődleges replika hello) partíció két kívüli három replikák lekérdezi sérült vagy tartalmának végleges törléséig. új elsődleges hello esetleg toorestore adatokat egy biztonsági másolatból.
  - hello teljes szolgáltatási elvész. Például a rendszergazda törli a teljes hello szolgáltatást, és így hello szolgáltatást és hello adatokat kell toobe vissza.
  - hello szolgáltatás replikált sérült alkalmazás adatokat (például egy alkalmazás-hibák) miatt. Ebben az esetben hello szolgáltatást rendelkezik toobe frissítése vagy visszatért tooremove hello okát hello sérülést, és nem sérült toobe visszaállítva.

Számos különböző módszer is előfordulhatnak, amíg fel néhány példa használatával `RestoreAsync` a fenti forgatókönyvek hello toorecover.

## <a name="partition-data-loss-in-reliable-services"></a>Partíció adatvesztést a Reliable Services
Ebben az esetben hello futásidejű volna automatikus észlelése hello adatvesztés és invoke hello `OnDataLossAsync` API.

hello szolgáltatás szerzőjének kell a következő toorecover tooperform hello:

  - Bírálja felül hello virtuális alaposztály metódust `OnDataLossAsync`.
  - Hello legutóbbi biztonsági mentés található hello külső helyen hello szolgáltatás biztonsági másolatait tartalmazza.
  - Töltse le a legutóbbi biztonsági hello (és hello biztonsági mentés kibontani hello biztonsági mentési mappába, ha azt tömörítették).
  - Hello `OnDataLossAsync` módszer lehetővé teszi egy `RestoreContext`. Hello hívás `RestoreAsync` API a megadott hello `RestoreContext`.
  - Térjen vissza IGAZ, ha hello visszaállítása sikeres.

Az alábbiakban látható egy példa végrehajtásának hello `OnDataLossAsync` módszert:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription`az átadott toohello `RestoreContext.RestoreAsync` hívás nevű tagot tartalmaz `BackupFolderPath`.
Egy teljes biztonsági mentés, visszaállításakor ez `BackupFolderPath` toohello hello mappa, amely tartalmazza a teljes biztonsági mentés helyi elérési utat kell megadni.
Teljes és növekményes biztonsági mentések, számos visszaállításakor `BackupFolderPath` toohello hello mappa nem csak hello teljes biztonsági mentés, de is összes hello növekményes biztonsági mentés helyi elérési utat kell megadni.
`RestoreAsync`hívás segítségével throw `FabricMissingFullBackupException` Ha hello `BackupFolderPath` megadott egy teljes biztonsági mentést tartalmaz.
Azt is segítségével throw `ArgumentException` Ha `BackupFolderPath` növekményes biztonsági mentések hibás lánca.
Ha teljes biztonsági mentés hello tartalmaz, például hello először növekményes, és hello harmadik növekményes biztonsági mentést, de nincs hello második növekményes biztonsági mentés.

> [!NOTE]
> hello RestorePolicy alapértelmezés szerint beállított tooSafe.  Ez azt jelenti, hogy hello `RestoreAsync` API sikertelen lesz, és ArgumentException hello biztonsági mentési mappa tartalmazza, amely a replikában tárolt régebbi, mint vagy egyenlő toohello állapottal állapotának észlelésekor.  `RestorePolicy.Force`lehet használt tooskip biztonsági ellenőrzés. Ennek részeként van megadva `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>A törölt vagy elveszett szolgáltatás
Ha a szolgáltatás el lett távolítva, meg kell először hozza létre hello szolgáltatás előtt hello vissza tudja állítani.  Fontos toocreate hello szolgáltatást, amely ugyanazt a konfigurációt, például particionálási sémát úgy, hogy hello adatok zökkenőmentesen visszaállíthatók hello.  Miután hello szolgáltatás működik-e, hello API toorestore adatok (`OnDataLossAsync` fent) rendelkezik a szolgáltatás minden partícióján meghívott toobe. Egyik módszere azt használatával `[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)` minden partícióján.  

Ettől a ponttól kezdve megvalósítási hello ugyanaz, mint a fenti forgatókönyv hello. Mindegyik partíció kell toorestore hello legújabb vonatkozó biztonsági mentési hello külső áruházban. Egy szerint azonosítója most megváltozhatott, mivel hello futásidejű hoz partíció azonosítók dinamikusan hello partíció. Ebből kifolyólag hello szolgáltatást kell toostore hello megfelelő partíciónak az adatait és a szolgáltatás neve tooidentify hello megfelelő legfrissebb biztonsági mentési toorestore származó minden partíció esetében.

> [!NOTE]
> Nem ajánlott toouse `FabricClient.ServiceManager.InvokeDataLossAsync` minden partíció toorestore hello teljes szolgáltatáson, mert, megsérülhet a fürt állapota.
> 

## <a name="replication-of-corrupt-application-data"></a>A sérült alkalmazás adatainak replikálása
Ha újonnan telepített hello az alkalmazásfrissítés programhiba van, az adatok sérülése okozhatnak. Például az alkalmazásfrissítés előfordulhat, hogy elindítása tooupdate phone számú rekordot egy megbízható szótár érvénytelen területen kódot.  Ebben az esetben az érvénytelen telefonszámmal hello replikálja mivel a Service Fabric nem kompatibilis hello jellegű hello adatot tárol.

hello először thing toodo után észlelni az ilyen egy súlyos hiba adatsérülést okozó toofreeze hello szolgáltatás hello alkalmazás szintjén, és ha lehetséges, a hello alkalmazás kódja nem rendelkező hello hiba toohello verziójának frissítése.  Hello szolgáltatást kód megszüntetése után is hello adatok továbbra is megsérült, és így az adatok esetleg toobe visszaállítva.  Ebben az esetben nem lenne megfelelő toorestore hello legutóbbi biztonsági, mivel lehetséges, hogy hello legfrissebb biztonsági mentéseket is megsérült.  Így lehetősége van toofind hello utolsó biztonsági mentés óta történt előtt hello adatokat kapott sérült.

Ha nem biztos benne, hogy mely biztonsági mentések sérült, képes egy új Service Fabric-fürt központi telepítése és hello biztonsági másolatok hasonlóan a fenti "Törölt vagy elveszett szolgáltatás" hello érintett partíciók forgatókönyv.  Minden partíció esetében hello biztonsági másolatok visszaállítása a hello legutóbbi toohello legalább elindításához. Miután megtalálta az olyan biztonsági mentésből nincs hello sérülése, áthelyezés vagy törlése az összes biztonsági mentés a partíció volt újabb (mint a biztonsági másolat). Ismételje meg az eljárást minden partíció esetében. Most, amikor `OnDataLossAsync` nevezik hello éles fürt hello partíción hello utolsó biztonsági mentés található hello külső tároló hello fent folyamat egyik kivételezett hello lesz.

Most, "Deleted vagy elveszett szolgáltatás" hello hello lépéseit szakasz lehet toorestore hello állapotának hello szolgáltatás toohello állapotának használt előtt hello buggy kód hello állapota sérült.

Vegye figyelembe:

  - Történő visszaállításához, folyamatban van egy esélyét, hogy a biztonsági mentés hello vissza nincs régebbi, mint hello partíció hello állapotát előtt hello adatok megszakadt. Ebből kifolyólag állítsa vissza a legutóbbi megoldásként toorecover, csak adatmennyiség lehető.
  - hello biztonsági mentési mappa elérési útja jelölő karakterlánc hello és hello olyan biztonsági mentési hello mappában lévő fájlok elérési útjait lehet attól függően, hogy hello FabricDataRoot elérési útját és alkalmazástípus nevének hossza 255 karakternél. Emiatt néhány .NET módszerek, például `Directory.Move`, toothrow hello `PathTooLongException` kivétel. Egy megoldás, toodirectly kernel32 API-k, például a hívás `CopyFile`.

## <a name="backup-and-restore-reliable-actors"></a>Biztonsági mentés és visszaállítás Reliable Actors


Megbízható szereplője keretrendszer Reliable Services épül. hello hello actor(s) futtató ActorService egy olyan állapot-nyilvántartó megbízható szolgáltatás. Emiatt összes hello biztonsági mentési és visszaállítási funkció érhető el a Reliable Services is elérhető tooReliable szereplője (kivéve, amelyek adott állapotszolgáltató viselkedések). Mivel biztonsági mentések partíciónkénti alapon veszik, állapotai összes szereplő, hogy a partíció készül biztonsági másolat (visszaállítás hasonló, és partíciónkénti alapon történik). tooperform biztonsági mentés/visszaállítás, hello szolgáltatás tulajdonosa hozzon létre egy egyéni szereplő osztály, amely ActorService osztályból származik, és majd biztonsági mentés/visszaállítás hasonló tooReliable szolgáltatások korábbi szakaszokban a fent leírt módon.

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo)
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

Egyéni szereplő szolgáltatás osztályt hoz létre, meg kell, amely is tooregister hello szereplő regisztrálásakor.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
   (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

hello alapértelmezett állapota szolgáltató Reliable Actors `KvsActorStateProvider`. Alapértelmezés szerint nincs engedélyezve a növekményes biztonsági mentés `KvsActorStateProvider`. Engedélyezheti a növekményes biztonsági mentés létrehozásával `KvsActorStateProvider` hello megfelelő saját konstruktoraikban beállítása, majd átadja azt a tooActorService konstruktor, ahogy az alábbi kódrészletet:

```csharp
class MyCustomActorService : ActorService
{
     public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
            : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
     {                  
     }
    
    //
   // Method overrides and other code.
    //
}
```

A növekményes biztonsági mentés engedélyezése után egy növekményes biztonsági másolat FabricMissingFullBackupException a következő okok valamelyike miatt meghiúsulhat, és mielőtt kilépteti a növekményes biztonsági mentését vagy mentéseit tootake teljes biztonsági mentést kell:

  - hello replika soha nem tartott egy teljes biztonsági mentés óta elsődleges vált.
  - Néhány hello naplórekordokat csonkultak óta utolsó biztonsági mentés készült.

Ha engedélyezve van a növekményes biztonsági mentést, `KvsActorStateProvider` körkörös puffer toomanage a naplót rögzíti, és rendszeres időközönként csonkolja azt nem használja. Biztonsági mentés nélküli felhasználó 45 percig történik, ha a hello rendszer automatikusan levágja hello naplórekordokat. Ez az időtartam alatt konfigurálható megadásával `logTrunctationIntervalInMinutes` a `KvsActorStateProvider` konstruktor (hasonló toowhen növekményes biztonsági mentés engedélyezése). hello naplórekordokat is beolvasása csonkolva lesz, ha elsődleges replikán kell toobuild replikává úgy, hogy az összes adatot küld.

A biztonsági mentési láncolatát, hasonló tooReliable szolgáltatások való visszaállítása során hello BackupFolderPath tartalmaznia kell az egyik alkönyvtár tartalmazó teljes biztonsági mentés, míg mások növekményes biztonsági mentését vagy mentéseit tartalmazó alkönyvtárakat alkönyvtárak. hello visszaállítási API kivételhibát FabricException megfelelő hibaüzenet Ha hello biztonsági mentési tanúsítványlánc érvényesítése sikertelen. 

> [!NOTE]
> `KvsActorStateProvider`jelenleg figyelmen kívül hagyja a RestorePolicy.Safe hello lehetőséget. Ez a szolgáltatás támogatása tervezett egy jövőbeli verzióban.
> 

## <a name="testing-backup-and-restore"></a>Tesztelési biztonsági mentése és visszaállítása
Kritikus fontosságú adatok biztonsági mentése van folyamatban, amely visszaállíthatóak fontos tooensure. Ezt megteheti hello figyelőn `Start-ServiceFabricPartitionDataLoss` parancsmag a PowerShellben, amely egy adott partíció tootest adatvesztés lehet szükség, hogy hello adatok biztonsági mentése és visszaállítása funkciót az a szolgáltatás a várt módon működik.  Az is lehetséges tooprogrammatically adatvesztés el, és szeretné visszaállítani, valamint az, hogy az esemény.

> [!NOTE]
> Keresse meg a biztonsági mentés minta végrehajtásának, és működőképes állapotba hozni a hello webalkalmazás hivatkozás a Githubon. Tekintse meg hello `Inventory.Service` szolgáltatás további részleteket.
> 
> 

## <a name="under-hello-hood-more-details-on-backup-and-restore"></a>A hello technikai részletek: további részleteket a biztonsági mentés és helyreállítás
Íme, néhány további részleteket a biztonsági mentését és helyreállítását.

### <a name="backup"></a>Biztonsági mentés
hello megbízható állapotkezelője hello képességét toocreate konzisztens biztonsági mentések tiltása nélkül olvasási / írási műveleteket biztosít. toodo úgy, hogy használja egy ellenőrzőpont- és naplófájlok mechanizmus.  hello megbízható állapotkezelője zavaros (lightweight) ad hozzá ellenőrzőpontokat egyes pontok toorelieve nyomás hello tranzakciós naplóból vesz igénybe, és növeli a helyreállításra.  Amikor `BackupAsync` nevezik, megbízható állapotkezelője hello arra utasítja az összes megbízható objektumok toocopy a legújabb ellenőrzőpont fájlok tooa helyi biztonsági mentés mappájába.  Hello megbízható állapotkezelője, majd minden naplórekordok hello "mutató start" toohello legújabb naplóbejegyzés kiindulva hello biztonsági mentési mappába másolja.  Hello biztonsági mentés szereplő összes hello napló rekordok toohello legújabb naplórekord fel, és megbízható állapotkezelője hello írási előre naplózási megőrzi, hello megbízható állapotkezelője biztosítja, hogy az, hogy minden tranzakciók, amelyek véglegesített (`CommitAsync` adott vissza sikeresen) szerepelnek hello biztonsági mentés.

Bármely tranzakció, amely véglegesíti után `BackupAsync` előfordulhat, hogy hívása történt, vagy nem hello biztonsági mentése.  Miután hello helyi biztonsági mentési mappája töltöttek hello platform (azaz, helyi biztonsági másolat elkészült hello futtatókörnyezet), hello szolgáltatás biztonsági mentési visszahívási hívják.  A visszahívási hello biztonsági mentési mappája tooan külső helyre például az Azure Storage áthelyezése felelős.

### <a name="restore"></a>Visszaállítás
hello használatával hello megbízható állapotkezelője nyújt az olyan biztonsági hello képességét toorestore `RestoreAsync` API.  
Hello `RestoreAsync` metódusa `RestoreContext` csak belül hello hívható `OnDataLossAsync` metódust.
logikai érték által visszaadott hello `OnDataLossAsync` azt jelzi, hogy hello szolgáltatás visszaállítva-e külső forrásból állapotában.
Ha hello `OnDataLossAsync` igaz értéket ad vissza, a Service Fabric újra létrehozza az összes többi replikáit az elsődleges. A Service Fabric biztosítja, hogy fogadó replikák `OnDataLossAsync` elsődleges szerepkör első átmenet toohello hívja, de vannak nem rendelkeznek állapot írási vagy olvasási állapota.
Ez azt jelenti, hogy a StatefulService implementers `RunAsync` nem lesz meghívva, amíg `OnDataLossAsync` futtatása sikeresen befejeződött.
Ezt követően `OnDataLossAsync` hello új elsődleges fogja meghívni.
Mindaddig, amíg a szolgáltatás teszi teljessé az API sikeresen (ad vissza IGAZ vagy hamis), és végül hello vonatkozó újrakonfigurálás, hello API fog tartani meghívott egyenként.

`RestoreAsync`először elutasítja azokat az összes meglévő állapot hello elsődleges replika lett meghívva.  
Hello megbízható állapotkezelője majd hello biztonsági mentési mappája szereplő összes megbízható hello-objektumokat hoz létre.  
A következő hello megbízható objektum utasításai toorestore a biztonsági mentési mappája hello az ellenőrzőpontokat.  
Végül hello megbízható állapotkezelője saját állapot helyreállít a hello naplórekordok hello biztonsági mentési mappában, és helyreállítást hajt végre.  
Hello helyreállítási folyamat részeként ponttól hello "kezdő" hello biztonsági mentési mappája véglegesítési naplórekordokat rendelkezik olyan megismételt toohello megbízható objektumok.  
Ez a lépés biztosítja, hogy hello helyreállított állapot.

## <a name="next-steps"></a>Következő lépések
  - [Reliable Collections](service-fabric-work-with-reliable-collections.md)
  - [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
  - [Megbízható szolgáltatások értesítések](service-fabric-reliable-services-notifications.md)
  - [Megbízható konfigurálása](service-fabric-reliable-services-configuration.md)
  - [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

