---
title: "a Microsoft Azure Storage egyidejűségi aaaManaging"
description: "Hogyan toomanage CONCURRENCY paraméterének értékét hello Blob, a várólista, a tábla és a fájl szolgáltatások"
services: storage
documentationcenter: 
author: jasontang501
manager: tadb
editor: tysonn
ms.assetid: cc6429c4-23ee-46e3-b22d-50dd68bd4680
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.openlocfilehash: 277fbbb880906da6be67b2267ed5c8e457455bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>A párhuzamosság kezelése a Microsoft Azure Storage szolgáltatásban
## <a name="overview"></a>Áttekintés
Internet alapú modern alkalmazások megtekintéséhez és adatok frissítése egyszerre több felhasználó általában rendelkeznek. Ehhez szükséges alkalmazás fejlesztők toothink gondosan kapcsolatos hogyan egy előre jelezhető tooprovide tapasztalnak tootheir végfelhasználók számára, különösen a forgatókönyvekben, ahol frissítheti a több felhasználó hello ugyanaz az adatokat. Számos három fő adatok feldolgozási stratégiák fejlesztők általában figyelembe veszi.  

1. Egyidejű hozzáférések optimista – utolsó egy kérelem végrehajtása egy frissítést a frissítés részeként ellenőrzi, hogy ha hello adatok hello alkalmazás óta megváltozott olvasható adatok. Például ha egy frissítés toohello wiki lap megtekintésével két felhasználók azonos lapon akkor hello wiki platform győződjön meg arról, hogy hello második frissítés nem ír felül hello első frissítés –, és mindkét tisztában legyenek azzal, hogy a frissítés sikeres volt-e vagy sem. Ezt a stratégiát leggyakrabban a webes alkalmazásokhoz.
2. Pesszimista feldolgozási – tooperform frissítés keresése alkalmazás egy zároláshoz érvénybe meggátolja, hogy a más felhasználók hello adatok frissítése, amíg hello zárolás objektumon. Például, ahol csak hello fő frissítések végrehajtása fő/alárendelt adatok replikáció esetén hello fő általában tárolására kizárólagos zárolást a hosszabb idő hello adatok tooensure nincs senki más frissítéseket.
3. Utolsó író wins – megközelítés, amely lehetővé teszi a frissítési műveletek tooproceed nélkül ellenőrzése, ha egyetlen más alkalmazáshoz sem frissült hello adatok, mert hello alkalmazás először hello adatokat olvasni. A stratégia (vagy formális stratégia hiánya) általában használatát, ha az adatok particionálása úgy, hogy van-e nem valószínű, hogy több felhasználó érik el a hello ugyanazokat az adatokat. Lehet hasznos, ahol rövid élettartamú adatfolyamokat feldolgozott.  

Ez a cikk áttekintést hogyan hello Azure Storage platform egyszerűbbé teszi a fejlesztés első osztályú támogatást biztosít a ezek párhuzamossági stratégiák három.  

## <a name="azure-storage--simplifies-cloud-development"></a>Az Azure Storage – egyszerűbbé teszi a felhőalapú fejlesztési
az Azure storage szolgáltatás hello támogatja minden három stratégiák, bár a képes tooprovide teljes körű támogatását optimista és pesszimista feldolgozási megkülönböztető mert tervezett tooembrace garantálja, hogy ha az erős konzisztencia modellt volt hello tárolási szolgáltatás véglegesíti a adatok beszúrása vagy további el toothat adatokat fog látni a legújabb frissítés hello művelet. A végleges konzisztencia modellt használó tárolási platformok közötti, amikor egy felhasználó egy írási hajt végre egy lag rendelkezik, és hello frissítésekor adatok így megfelel a rendelés tooprevent inkonzisztenciát a az ügyfélalkalmazások fejlesztéséhez más felhasználók által látható befolyásolja a végfelhasználók számára.  

Ezenkívül tooselecting egy megfelelő feldolgozási stratégia fejlesztők is kell ügyelnie, hogyan egy tárolási platform elkülöníti a módosításokat – különösen módosítások toohello ugyanaz az objektum tranzakciók között. az Azure storage szolgáltatás hello pillanatkép-elkülönítési tooallow olvasási műveletek toohappen belül egyetlen partícióra írási műveletek egyidejűleg használja. Más elkülönítési szinten eltérően pillanatkép-elkülönítés biztosítja, hogy az, hogy az összes olvasási lásd: hello adatok alkalmazáskonzisztens pillanatfelvételi frissítések is megjelenhetnek – tulajdonképpen által hello utolsó véglegesített értékeket ad vissza, miközben egy frissítés tranzakció feldolgozása közben is.  

## <a name="managing-concurrency-in-blob-storage"></a>A Blob Storage tárolóban párhuzamossági kezelése
Dönthet úgy is toouse optimista vagy pesszimista feldolgozási modellek toomanage hozzáférés tooblobs, és a tárolók hello blob szolgáltatás. Ha nem kifejezetten megad egy stratégia utolsó ír wins hello alapértelmezett érték.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>A bináris objektumok és tárolók egyidejű hozzáférések optimista
hello társzolgáltatás tárolt azonosító tooevery objektum rendeli hozzá. Ez az azonosító minden alkalommal, amikor a frissítési művelet történik egy objektum frissül. hello azonosítója egy HTTP GET válaszban hello (entitáscímke) ETag fejlécet hello HTTP protokoll belül definiált részeként toohello ügyfél adja vissza. Eredeti ETag együtt egy feltételes fejlécet tooensure, amely a frissítés csak akkor történik, ha egy bizonyos feltétel teljesül – ebben az esetben a hello feltétele egy "If-Match" fejlécet hello tárolási igénylő felhasználó végrehajtása egy ilyen objektum frissítésének elküldhető hello Szolgáltatás tooensure hello értéke hello frissítés kérelemben megadott ETag van hello ugyanaz, mint a Társzolgáltatás hello tárolt hello.  

Ez a folyamat hello áttekintését a következőképpen történik:  

1. A blob lekérése hello társzolgáltatás, hello választ egy HTTP ETag Fejlécérték, amely azonosítja a hello tárolószolgáltatásban hello objektum aktuális verziója hello tartalmazza.
2. Hello blob frissítésekor a következők hello ETag érték hello az 1. lépésben kapott **If-Match** toohello szolgáltatás küld hello kérelem feltételes fejlécet.
3. hello szolgáltatás összehasonlítja hello ETag értéket hello kérelem hello blob hello jelenlegi ETag érték.
4. Ha a jelenlegi ETag érték hello hello blob egy másik verzió, mint a hello ETag hello **If-Match** hello kérelemben, hello szolgáltatás feltételes fejlécet 412 hiba toohello ügyfél adja vissza. Ez azt jelzi, hogy egy másik folyamat frissítette hello blob, mivel a hello ügyfél azt lekérése toohello ügyfél.
5. Ha hello jelenlegi ETag érték hello BLOB hello verziójával megegyező verzióra hello ETag hello **If-Match** hello kérelemben, hello szolgáltatás feltételes fejlécet végez hello a kért művelet, és a frissítések hello hello blob jelenlegi ETag érték tooshow hozott létre egy új verziója.  

hello (Storage Ügyfélkódtár 4.2.0 hello használata) alábbi C# kódrészletben láthatja egy egyszerű példa bemutatja, hogyan tooconstruct egy **If-Match AccessCondition** hello hello tulajdonságait, de a blob elérése ETag-érték alapján korábban lekért vagy beszúrt. Ezután hello **AccessCondition** objektum amikor azt hello blob frissítése: hello **AccessCondition** objektum hozzáadása hello **If-Match** fejléc toohello kérelmet. Ha egy másik folyamat hello blob frissítve van, a hello blob szolgáltatás egy HTTP 412 (előfeltétel nem teljesült) állapotüzenetet adja vissza. Letöltheti a hello teljes mintát itt: [kezelése egyidejű használata az Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve hello ETag from hello newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// toostorage blob service which returns hello etag in response.
string orignalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No etag, provided so orignal blob is overwritten (thus generating a new etag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try tooupdate hello blob using hello orignal ETag provided when hello blob was created
try
{
    Console.WriteLine("Trying tooupdate blob using orignal etag toogenerate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(orignalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
        // TODO: client can decide on how it wants toohandle hello 3rd party updated content.
    }
    else
        throw;
}  
```

hello tároló szolgáltatást is támogatja a további feltételes fejlécek például **If-Modified-Since**, **If-Unmodified-Since** és **If-None-Match** , valamint ezek kombinációi. További információ: [megadó feltételes fejlécek Blob szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dd179371.aspx) az MSDN Webhelyén.  

hello következő táblázat összefoglalja, hogy a kezelő például feltételes fejlécek hello tároló műveletek **If-Match** hello kérelmet, valamint a hello válaszul ETag értéket adnak vissza.  

| Művelet | Tároló ETag értékét adja vissza | Feltételes fejlécek fogad el |
|:--- |:--- |:--- |
| Tároló létrehozása |Igen |Nem |
| A tároló tulajdonságainak beolvasása |Igen |Nem |
| Tároló metaadatot beszerezni |Igen |Nem |
| Állítsa be a Csomagtároló metaadatai |Igen |Igen |
| Tároló hozzáférés-vezérlési lista beolvasása |Igen |Nem |
| Tároló hozzáférés-vezérlési lista beállítása |Igen |Igen (*) |
| Törli a tárolót |Nem |Igen |
| Címbérlet tároló |Igen |Igen |
| Lista Blobok |Nem |Nem |

(*) hello engedélyek határozzák meg SetContainerACL gyorsítótárba kerüljenek-e hajtanak végre bizonyos frissítések toothese engedélyek toopropagate mely időszakban frissítései nem garantált toobe konzisztens 30 másodperc.  

hello következő táblázat összefoglalja, hogy a kezelő például feltételes fejlécek hello blob műveletek **If-Match** hello kérelmet, valamint a hello válaszul ETag értéket adnak vissza.

| Művelet | ETag értéket ad vissza | Feltételes fejlécek fogad el |
|:--- |:--- |:--- |
| Helyezze a Blob |Igen |Igen |
| A Blob beolvasása |Igen |Igen |
| A Blob tulajdonságainak beolvasása |Igen |Igen |
| A Blob tulajdonságainak beállítása |Igen |Igen |
| A Blob metaadatot beszerezni |Igen |Igen |
| Állítsa be a Blob metaadatai |Igen |Igen |
| Címbérlet Blob (*) |Igen |Igen |
| Pillanatkép Blob |Igen |Igen |
| A Blob másolása |Igen |Igen (a forrás és cél blob) |
| A Blob másolási megszakítása |Nem |Nem |
| A Blob törlése |Nem |Igen |
| PUT letiltása |Nem |Nem |
| PUT tiltólista |Igen |Igen |
| Tiltólista beolvasása |Igen |Nem |
| Helyezze a lap |Igen |Igen |
| Get tartományokat |Igen |Igen |

(*) Címbérlet Blob nem változik a blob ETag hello.  

### <a name="pessimistic-concurrency-for-blobs"></a>A blobok pesszimista feldolgozási
toolock kizárólagos használatra blob, szerezzen be egy [bérleti](http://msdn.microsoft.com/library/azure/ee691972.aspx) rajta. Amikor bérletet szerezni, megadhatja, hogy mennyi ideig kell bérleti hello: Ez lehet a 15 too60 másodperc között vagy végtelen mely összegek tooan kizárólagos zárolást. Egy véges bérleti tooextend, és hogy kiadna címbérletet, amikor befejezte az megújíthatják. hello blob szolgáltatás véges címbérleteket automatikusan feloldja a lejárat után.  

Címbérleteket engedélyezése különböző szinkronizálási stratégiák toobe támogatja, beleértve a kizárólagos írási / olvasási, kizárólagos írási megosztott / kizárólagos olvasási és írási megosztott / kizárólagos olvasása. Ahol a címbérlet létezik hello tároló szolgáltatás érvényesíti kizárólagos írási (put, állítsa be és törlési műveletek) hello fejlesztői tooensure összes ügyfelet használó alkalmazások bérleti azonosító egy és, hogy csak egy ügyfél egyszerre azonban biztosítja az olvasási műveletek kizárólagosság igényel érvényes bérleti azonosítóval rendelkezik. Olvassa el, amelyek nem tartalmazzák a címbérlet azonosító eredményt megosztott olvasási műveletek.  

hello alábbi C# részlet példáját mutatja be egy kizárólagos címbérlet beszerzése a blob 30 másodpercig hello tartalom hello BLOB frissítése és hello bérleti majd felszabadítása. Ha már van egy érvényes bérleti hello blob egy új bérleti tooacquire meg, a hello blob szolgáltatás egy "HTTP (409) ütközés" állapot eredményt adja vissza. hello részlet alatt használ egy **AccessCondition** objektum tooencapsulate hello címbérleti információkat, amikor hello tároló szolgáltatás lehetővé teszi a kérelem tooupdate hello blob.  Letöltheti a hello teljes mintát itt: [kezelése egyidejű használata az Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update tooblob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying tooupdate blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Ha egy írási művelet bérelt blob hello bérleti azonosító továbbításához nélkül kísérli meg, hello kérelem egy 412 hibaüzenettel meghiúsul. Vegye figyelembe, hogy ha hello bérlete lejár hello hívása előtt **UploadText** metódust, de még mindig adjon át hello bérleti azonosítója, hello kérelem is sikertelen, és egy **412** hiba. Címbérlet lejárati idejét és a címbérlet azonosítók kezelésével kapcsolatos további információkért lásd: hello [bérleti Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST dokumentációját.  

hello következő blob műveletek használható címbérleteket toomanage pesszimista feldolgozási:  

* Helyezze a Blob
* A Blob beolvasása
* A Blob tulajdonságainak beolvasása
* A Blob tulajdonságainak beállítása
* A Blob metaadatot beszerezni
* Állítsa be a Blob metaadatai
* A Blob törlése
* PUT letiltása
* PUT tiltólista
* Tiltólista beolvasása
* Helyezze a lap
* Get tartományokat
* Pillanatkép a Blob - címbérlet azonosítója nem kötelező, ha a címbérlet létezik
* A Blob - azonosító szükséges, hogy található-e a címbérlet hello cél blob bérleti másolása
* Megszakítási másolási Blob - bérleti azonosító szükséges, ha egy végtelen címbérleti hello cél blob megtalálható-e
* Címbérlet Blob  

### <a name="pessimistic-concurrency-for-containers"></a>A tárolók pesszimista feldolgozási
A tárolók címbérleteket engedélyezése ugyanazon szinkronizálási stratégiák toobe támogatott, a blobok hello (kizárólagos írási és olvasási, kizárólagos írási megosztott / kizárólagos olvasási és írási megosztott kizárólagos olvasási /) azonban eltérően blobok hello társzolgáltatás csak kikényszeríti kizárólagosság a delete művelet. toodelete egy aktív bérleti jog vonatkozik a tárolóhoz, egy ügyfél tartalmaznia kell hello törlési kérés Azonosítójú hello aktív bérleti jog vonatkozik. Minden más tároló művelet sikerült bérelt tárolóba hello bérleti azonosító megadása nélkül megosztott ebben az esetben azok a műveletek. Ha szükség a frissítés (put vagy beállítása) vagy az olvasási műveletek kizárólagosság majd fejlesztők győződjön meg arról az összes ügyfél használni a címbérlet-Azonosítót, és hogy csak egy ügyfél egyszerre rendelkezik-e egy érvényes bérleti.  

hello következő tároló műveletek használható címbérleteket toomanage pesszimista feldolgozási:  

* Törli a tárolót
* A tároló tulajdonságainak beolvasása
* Tároló metaadatot beszerezni
* Állítsa be a Csomagtároló metaadatai
* Tároló hozzáférés-vezérlési lista beolvasása
* Tároló hozzáférés-vezérlési lista beállítása
* Címbérlet tároló  

További információk:  

* [A Blob szolgáltatás műveletek feltételes fejlécek megadása](http://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Címbérlet tároló](http://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Címbérlet Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-hello-table-service"></a>A Table szolgáltatás hello párhuzamossági kezelése
hello table szolgáltatás használja optimista párhuzamossági ellenőrzi hello alapértelmezett viselkedésként személyekkel szemben ha explicit módon kell választania tooperform optimista konkurencia ellenőrzése hello blob szolgáltatás használatakor. hello más hello tábla és a blob-szolgáltatások közötti különbség, hogy csak kezelheti hello párhuzamossági az entitások viselkedésének mivel hello blob szolgáltatással kezelheti a tárolók és blobok hello egyidejűségi.  

egyidejű hozzáférések optimista toouse és toocheck, ha egy másik folyamat entitás módosítva, mivel a hello table storage szolgáltatásból való lekérése, hello ETag érték hello table szolgáltatás az entitást adja vissza, amikor is használhatja. Ez a folyamat hello áttekintését a következőképpen történik:  

1. Egy entitás beolvasása hello table storage szolgáltatásból, hello válasz tartalmazza az egy ETag érték, amely azonosítja az alkalmazás hello társzolgáltatás társított hello aktuális azonosító.
2. Hello entitás frissítésekor a következők hello ETag érték hello kötelező az 1. lépésben kapott **If-Match** toohello szolgáltatás küld hello kérelem fejlécében.
3. hello szolgáltatás összehasonlítja hello ETag értéket hello kérelem jelenlegi ETag érték hello hello entitás.
4. Ha hello jelenlegi ETag hello entitás értéke eltér a kötelező hello ETag hello **If-Match** hello kérelemben, hello szolgáltatás fejlécet 412 hiba toohello ügyfél adja vissza. Ez azt jelzi, hogy egy másik folyamat frissítette hello entitás, mivel a hello ügyfél azt lekérése toohello ügyfél.
5. Ha a jelenlegi ETag érték hello hello entitás van hello megegyezik a kötelező hello ETag hello **If-Match** fejléc a következő hello kérelem vagy hello **If-Match** fejlécben hello helyettesítő karakter (*), hello szolgáltatás végrehajtja a kért hello művelet és a frissítések hello hello entitás tooshow, hogy frissült jelenlegi ETag-értékével.  

Vegye figyelembe, hogy hello blob szolgáltatás, eltérően hello table szolgáltatás futtatásához szükséges hello ügyfél tooinclude egy **If-Match** fejléc a következő frissítési kérelmek. Azonban lehetséges tooforce egy feltétel nélküli (utolsó író wins stratégia) frissítése és feldolgozási ellenőrzések megkerülését, ha hello ügyfél beállítja a hello **If-Match** fejléc toohello helyettesítő karakter (*) hello kérelemben.  

a következő kódrészletet C# hello egy ügyfélentitást vagy korábban létrehozott vagy frissített e-mail címüket kellene beolvasni jeleníti meg. hello kezdeti beszúrása vagy művelet tárolók hello ETag értéke hello felhasználói objektum, és mivel hello mintát használ hello azonos objektumpéldány hello végrehajtásakor a csere, automatikusan küldi hello ETag érték hátsó toohello table szolgáltatás, Egyidejűség megsértése a hello szolgáltatás toocheck engedélyezése. Ha egy másik folyamat frissítette hello entitás table storage-ban, a hello szolgáltatást egy HTTP 412 (előfeltétel nem teljesült) állapotüzenetet adja vissza.  Letöltheti a hello teljes mintát itt: [kezelése egyidejű használata az Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

tooexplicitly hello konkurencia ellenőrzése letiltásához állítsa be hello **ETag** hello tulajdonságának **alkalmazott** objektum túl "*" hello csere művelet végrehajtása előtt.  

```csharp
customer.ETag = "*";  
```

hello következő táblázat összefoglalja hogyan hello tábla entitás műveletek ETag értékeket használja:

| Művelet | ETag értéket ad vissza | If-Match fejléc igényel |
|:--- |:--- |:--- |
| Lekérdezés entitások |Igen |Nem |
| Entitás beszúrása |Igen |Nem |
| Entitás frissítése |Igen |Igen |
| Entitás egyesítése |Igen |Igen |
| Entitás törlése |Nem |Igen |
| Entitás cseréje vagy beszúrása |Igen |Nem |
| Az INSERT vagy egyesítés entitás |Igen |Nem |

Vegye figyelembe, hogy hello **Insert vagy az entitás cseréje** és **Insert vagy az egyesítési entitás** műveletek tegye *nem* bármely párhuzamossági ellenőrzéseket hajtanak végre, mert az egy ETag érték toohello nem küldenek TABLE szolgáltatás.  

Általában a fejlesztők táblák használata hagyatkozzon kizárólag az egyidejű hozzáférések optimista méretezhető alkalmazások fejlesztése során. Pesszimista zárolás van szüksége, egyik módszer fejlesztők Ha táblák elérése tooassign minden táblához kijelölt blob, és próbálja tootake hello blob egy bérlete előtt működő hello táblán. Ezt a módszert használja az összes adatelérési hello alkalmazás tooensure szükséges elérési utak beszerzése hello bérleti előzetes toooperating hello táblán. Is vegye figyelembe, hogy hello minimális bérleti idejének: 15 másodperc ehhez a alapos megfontolás a méretezhetőség érdekében.  

További információk:  

* [Entitások műveletek](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-hello-queue-service"></a>A Queue szolgáltatás hello párhuzamossági kezelése
Milyen párhuzamossági a fontos hello várólistázást szolgáltatásban egy például az is, ha több ügyfelet keres üzenetek várólistából való várólistából. Hello várólista lekért üzenet, amikor a hello válasz üdvözlőüzenetére és a pop fogadását számnak, szükséges toodelete üdvözlőüzenetére tartalmazza. üdvözlőüzenetére nem törlődnek automatikusan hello várólista, de után azt beolvasása, nincs látható tooother ügyfelek hello hello visibilitytimeout paraméter által megadott időtartam. hello üzenetének hello ügyfél üdvözlőüzenetére várt toodelete, feldolgozása megtörtént, és előtt hello idő által megadott hello hello választ, amelyet a program hello visibilitytimeout hello értékének TimeNextVisible eleme után a paraméter. hello értékének visibilitytimeout kerül toohello idő alatt, mely hello üzenet TimeNextVisible toodetermine hello értékének lekérése.  

hello várólista szolgáltatás nem rendelkezik optimista vagy pesszimista egyidejű támogatása, és a feldolgozása a sorból beolvasott üzenetekből OK ügyfelek biztosítania kell az idempotent módon dolgozza fel az üzeneteket. Utolsó író wins stratégia frissítési műveletek, például a SetQueueServiceProperties, SetQueueMetaData, SetQueueACL és UpdateMessage szolgál.  

További információk:  

* [Várólista szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Üzenet](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-hello-file-service"></a>A Fájlszolgáltatás hello párhuzamossági kezelése
hello szolgáltatása elérhető két különböző protokollvégpontokat – a többi pedig az SMB használatával. hello REST-szolgáltatást nem kell az optimista zárolása vagy pesszimista zárolását, és minden frissítés utolsó író wins stratégiát követi. Csatlakoztassa a fájlmegosztások SMB-ügyfelek használhatják fel a rendszer zárolási mechanizmusok toomanage hozzáférés tooshared fájlok – beleértve a hello képességét tooperform pesszimista zárolás. Ha egy SMB-ügyfél megnyit egy fájlt, adja meg hello fájl hozzáférés és a megosztáshoz mód. Az SMB-ügyfél zárolta hello fájl bezárásáig hello fájl beállítást egy fájl hozzáférés "Write" vagy "Olvasási/írási" együtt egy fájlmegosztás mód "None" ér véget. Ha egy fájlt, amelyben egy SMB-ügyfél rendelkezik-e zárolva hello fájl kísérletet REST-művelet hello REST-szolgáltatást állapotkód 409 (Ütközés) Hibakód SharingViolation vissza.  

Ha egy SMB-ügyfél megnyit egy fájlt törlésre, jelöli hello fájlt, amíg más SMB-ügyfél törlése függőben lévő fájl megnyitott kezelőkkel be van zárva. A fájl törlése függőben van megjelölve, amíg bármely REST művelet, hogy a fájl-állapotkódot (Ütközés) 409 hibakód SMBDeletePending ad vissza. Állapotkód: 404-es (nem található) a rendszer nem adja vissza, mert lehetséges, hogy hello SMB ügyfél tooremove hello függőben lévő törlési jelző előzetes tooclosing hello fájlt. Ez azt jelenti állapotkód: 404-es (nem található) csak várt hello fájl eltávolításakor. Vegye figyelembe, hogy közben az egy SMB-függő törlés állapotú, akkor nem szerepelni fog a tulajdonságlista-fájlok eredmények hello. Fontos: hello többi fájl törlése és a többi törlése Directory műveletek i lépnek, és nem eredményeznek függőben lévő is állapot törlése.  

További információk:  

* [Zárolja fájl kezelése](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Összegzés és további lépések
hello Microsoft Azure Storage szolgáltatás le lett tervezett toomeet hello igényeinek hello legösszetettebb az online alkalmazások telepítéséhez és a fejlesztők toocompromise vagy rethink tervezési feltételezéseket feldolgozási és az adatok konzisztenciájának például, hogy tootake származnak a jogosultsággal.  

Végezze el a blogban található hivatkozott mintaalkalmazás hello:  

* [Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

További információ az Azure Storage lásd:  

* [A Microsoft Azure Storage kezdőlap](https://azure.microsoft.com/services/storage/)
* [Bevezetés tooAzure tároló](storage-introduction.md)
* Bevezetés a tárolás [Blob](storage-dotnet-how-to-use-blobs.md), [tábla](storage-dotnet-how-to-use-tables.md), [várólisták](storage-dotnet-how-to-use-queues.md), és [fájlok](storage-dotnet-how-to-use-files.md)
* Tároló-architektúra – [az Azure Storage: egy magas rendelkezésre állású felhőalapú tárolási szolgáltatásba erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

