---
title: "Az Azure lapblobokat egyedi szolgáltatása |} Microsoft Docs"
description: "Tudnivalók az Azure áttekintése lapblobokat, előnyöket, és a használati esetekben minta parancsfájlok."
services: storage
documentationcenter: 
author: anasouma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: wielriac
ms.openlocfilehash: 019b793f6d2b4cb70514d867b78c9501240baeda
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/23/2018
---
# <a name="unique-features-of-azure-page-blobs"></a>Az Azure Lapblobokat egyedi jellemzői

Az Azure Storage blob storage háromféle biztosít: Blokkblobokat, hozzáfűző blobokat és Lapblobokat. Blokkblobok blokkok és ideálisak szöveges vagy bináris fájlok tárolásához, és hatékonyan nagy fájlok feltöltése. Hozzáfűző blobok épülnek fel is blokkok, de vannak optimalizálva hozzáfűzése műveletek, így ideális a naplózási forgatókönyvekben. Lapblobokat 512 bájtos lapok alkotják a teljes méret és a gyakori véletlenszerű olvasási/írási műveletek hatékonyabb legfeljebb 8 TB. Lapblobokat az Azure infrastruktúra-szolgáltatási lemezek alapját. Ez a cikk összpontosít a szolgáltatások és a Lapblobokat előnyeit foglalja össze.

## <a name="overview"></a>Áttekintés
Lapblobokat 512 bájtos lapok, amelyek lehetővé teszik a olvasási/írási bájt tetszőleges tartományok gyűjteménye. Ezért Lapblobokat tárolására alkalmasak index alapján és ritka adatstruktúrák operációsrendszer- és adatlemezek például a virtuális gépek és adatbázisok. Például Azure SQL Database használja lapblobokat az állandó tároló az adatbázisok. Továbbá a tartomány-alapú frissítéseket tartalmazó fájlokat is gyakran használják Lapblobokat.  

Azure Lapblobokat kulcsfunkciói a következők: a REST-felület, a tároló és az Azure-bA zökkenőmentes áttelepítés képességek tartósságát. Ezen funkciókról további részleteket a következő szakaszban ismertetjük. Emellett Azure Lapblobokat jelenleg támogatott a két típusú tárhelyek: prémium és Standard tárolására. Prémium szintű Storage kifejezetten konzisztens nagy teljesítményt és alacsony késési igényű, így ideális, ha magas performant adatokat tároló adatbázis premium lapblobokat igénylő munkaterhelések készült.  Standard szintű tárolást költséghatékonyabb késésre nem érzékeny munkaterhelések futtatására.

## <a name="sample-use-cases"></a>Példa használati esetek

Most tárgyalják néhány használati eseteket lapblobokra kezdődően az Azure infrastruktúra-szolgáltatási lemezek. Az Azure Lapblobokat az Azure IaaS virtuális lemezek platform gerincét. Azure-OS és adatlemezek is használják, mint ahol adatok tartósan őrzi meg az Azure Storage platform, és ezután a felhasználóikhoz kerülnek a virtuális gépeket, a maximális teljesítmény virtuális lemezek. Azure-lemezeket a Hyper-V megmaradnak [VHD formátumú](https://technet.microsoft.com/library/dd979539.aspx) és tárolt, mint egy [oldalakra vonatkozó Blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) az Azure Storage. Virtuális lemezek használata az Azure IaaS virtuális gépeket, mellett Lapblobokat is triggerrel PaaS és típusú Azure SQL Database szolgáltatást, amely jelenleg használ a Lapblobokat SQL adatok tárolására, például az adatbázis gyors, véletlenszerű olvasási és írási műveletek engedélyezése. Egy másik példa erre, ha egy PaaS szolgáltatás megosztott media Access for együttműködő videó szerkesztési alkalmazások, a lapblobokat véletlenszerű helye az adathordozót a gyors hozzáférés engedélyezése. Azt is lehetővé teszi, hogy gyors és hatékony szerkesztését, és egyszerre több felhasználó ugyanabból az adathordozó egyesítését. 

Azure Site Recovery, Azure biztonsági mentés, valamint sok külső fejlesztők hasonló első fél Microsoft szolgáltatások megvalósítását iparágvezető innovációinak lap blob REST-felület használatával. Az alábbiakban néhány implementálva az Azure egyedi forgatókönyv: 
* Növekményes pillanatképet alkalmazás által vezérelt felügyeleti: alkalmazások használhatják fel a lap Blob pillanatképek és a REST API-k mentéséhez, az alkalmazás ellenőrzőpontokat költséges duplikálva lettek-e az adatok nélkül. Ez akkor lehetséges, mert a lapblobokat, amelyek nem igényelnek, a teljes adatok másolása helyi pillanatképeket támogatjuk. A nyilvános pillanatkép API-k is engedélyezheti elérése és pillanatképek közötti eltérések másolását.
* Élő áttelepítés alkalmazás és a helyszíni adatok felhőbe: a helyszíni adatok másolása és írható közvetlenül az Azure oldalakra vonatkozó Blob, amíg a helyszíni virtuális gép továbbra is fut a REST API-k használatával. Ha a cél rendelkezik szerepelnek, is gyorsan adatokat használó Azure virtuális gépek feladatátvétele. Ez lehetővé teszi a virtuális gépek áttelepítése, és a felhőben minimális állásidővel, mivel az adatok áttelepítése a háttérben történik, miközben továbbra is használhatja a virtuális gép és a feladatátvételi szükséges állásidő helyszíni virtuális lemezei rövid (percben).
* [SAS-alapú](../common/storage-dotnet-shared-access-signature-part-1.md) megosztott hozzáférést, amely lehetővé teszi a helyzetekben, például a több-olvasók és single-író egyidejűség-vezérlési támogatása.

## <a name="page-blob-features"></a>Az oldalakra vonatkozó Blob szolgáltatások

### <a name="rest-api"></a>REST API
A következő dokumentumban használatába [Lapblobokat használatával történő fejlesztéséhez](storage-dotnet-how-to-use-blobs.md). Tegyük fel ossza meg velünk tekintse meg a Storage ügyféloldali kódtára a .NET használatával Lapblobokat elérését. 

A következő ábra a fiók, tárolók és blobok általános kapcsolatai ismerteti.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-certain-size"></a>Egy adott méretet, egy üres oldalakra vonatkozó Blob létrehozása
Oldalakra vonatkozó Blob, azt először hozzon létre egy CloudBlobClient objektum eléréséhez szükséges a tárfiók (1. ábra a pbaccount) együtt a StorageCredentialsAccountAndKey objektum alább látható módon a blob-tároló alap Uri-azonosítójú.  A példa megjeleníti a CloudBlobContainer-objektumra mutató hivatkozás létrehozása, és ezután hozza létre a tárolót (testvhds), ha még nem létezik.  Ezután a CloudBlobContainer objektummal vannak, létrehozható egy hivatkozást egy CloudPageBlob objektum eléréséhez szeretnénk lap blob neve (os4.vhd) megadásával. Ezután a telepítésnek az oldalakra vonatkozó blob létrehozása [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) a BLOB maximális mérete benyújtása szeretnénk létrehozásához.  A blobSize nem 512 bájt többszöröse lehet.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Oldalakra vonatkozó Blob átméretezése
Oldalakra vonatkozó Blob létrehozása után átméretezéséhez használja a [átméretezése](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. A kért méret 512 bájt többszörösének kell lennie.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Az oldalakra vonatkozó Blob lapok írása
Lapok használja a [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) metódust.  Ez lehetővé teszi, hogy legfeljebb 4MBs lapok követő írása. Az eltolás írt 512 bájtos határra kell kezdődnie (startingOffset % 512 == 0), és a záró 512 határra - 1.  Az alábbi kódot érnek el azt a blob-objektum WritePages hívásának példáját mutatja be:
```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Amint követő lapok írási kérelmet a blob szolgáltatás sikeres lesz, és a tartósság és a rugalmasság replikálódik, az írási véglegesítette, és sikeres ad vissza az ügyfélnek.  

Az alábbi ábrán látható külön 2 írási műveleteket:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Írási művelet kezdődő eltolás: 0 hosszúsága 1024 bájt 
2.  Egy írási művelet kezdődő 1024 hosszának 4096 eltolása 

#### <a name="reading-pages-from-a-page-blob"></a>Az oldalakra vonatkozó Blob lapok olvasása
Olvassa el az oldalak, használja a [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) bájttartomány olvasni az oldalakra vonatkozó blob metódust. Ez lehetővé teszi, hogy töltse le a teljes blob vagy a tartomány minden eltolás a BLOB-től kezdődő bájt. Olvasásakor, az eltolás nem kell elindítani a 512 többszöröse. Bájtok NUL lapról olvasásakor a szolgáltatás nulla bájt adja vissza.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
Az alábbi ábrán a 256 és a 4352 rangeSize BlobOffSet egy olvasási műveletet. Narancssárga kijelölt által visszaadott adatokat. Nullák NUL lapok adja vissza

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Ha most töltse le a érvényes oldalmegjelenítésiméret-régiók a nulla bájtot egressing megfizetése alól, és a letöltési késés csökkentése érdekében érdemes lehet ritkán ki van töltve blob.  Határozhatja meg, hogy mely lapok adatok üzemelnek, [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). A visszaadott tartományok számbavétele, majd töltse le az adatokat a tartományok. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Oldalakra vonatkozó Blob lízing
A bérleti Blob művelet megállapítja és blob zárolását kezeli az írási és törlési műveletek. Ez a művelet olyan meglehetősen esetekben hasznos, ahol oldalakra vonatkozó blob annak érdekében, hogy csak egy ügyfél egyszerre írhat a blob több ügyfél is hozzáférnek. Azure-lemezeket, például használja. Ez lízing mechanizmus annak érdekében, hogy a lemez csak egy virtuális kezeli. A Zárolás időtartama 15 – 60 másodperc is lehet, vagy lehet végtelen. A dokumentációban [Itt](/rest/api/storageservices/lease-blob) további részleteket.

> A következő hivatkozás használatával beolvasása [Kódminták](/resources/samples/?service=storage&term=blob&sort=0) számos egyéb alkalmazás forgatókönyvek esetén. 

Mellett részletes REST API-k lapblobokat azt is megadhatja, megosztott elérési, a tartósság és a fokozott biztonságot. Bemutatjuk, hogy részletesebben a következő bekezdésekben ezek előnyeit. 

### <a name="concurrent-access"></a>Concurrent Access
A lap Blobok REST API-t, és lízing mechanizmus lehetővé teszi, hogy az alkalmazásokat az oldalakra vonatkozó blob több ügyfelekről. Például tételezzük fel kell felépítenie osztja meg a tárolási objektum több felhasználóval rendelkező elosztott felhőszolgáltatás. A webes alkalmazás szolgáló képek nagy gyűjteménye több felhasználó lehet. Ennek végrehajtásához egy lehetőség egy csatlakoztatott lemezzel rendelkező virtuális Gépet használ. Az ezt a következők downsides (i) a korlátozást, hogy egy lemezt csak egy így korlátozza a méretezhetőséget, a rugalmasságot, és a kockázatok növelése egyetlen virtuális géphez csatlakoztatható. Ha probléma van a virtuális gép vagy a szolgáltatás a virtuális gépen, majd a címbérlet, mert a kép nem érhető el addig, amíg a címbérlet lejár, vagy megszakadt; és (ii), hogy az infrastruktúra-szolgáltatási virtuális gép további költség nélkül. 

Egy másik lehetőség egy Azure Storage REST API-kon keresztül közvetlenül a Lapblobokat használnak. Ezt a beállítást szükségtelenné teszi a költséges IaaS virtuális gépeket, a közvetlen hozzáférés teljes rugalmasságot nyújt, több ügyfelekről, nem kell a lemezek csatlakoztatási/leválasztási szolgáltatás egyszerűbbé és elkerülhető az a kockázat, hibák elhárítása a virtuális Gépen. És a véletlenszerű olvasási/írási műveletek teljesítményének lemeznek azonos szinten biztosít

### <a name="durability-and-high-availability"></a>Tartósság és magas rendelkezésre állású
Standard és a prémium szintű storage tartós tárolási, ahol a lap Blobadatok mindig replikálódik tartósság és magas rendelkezésre állás biztosításához. További információ az Azure adattároló redundanciája, amely megjelenik ez [dokumentáció](../common/storage-redundancy.md). Azure-infrastruktúra-szolgáltatási lemezek és a lapblobokat, egy iparágvezető rendelkező % vállalati szintű tartósságot következetesen rendelkezik i [hibaaránya Annualized](https://en.wikipedia.org/wiki/Annualized_failure_rate). Ez azt jelenti, hogy Azure soha nem megszakadt az ügyfél oldalon blobadatokat. 

### <a name="seamless-migration-to-azure"></a>Az Azure-bA zökkenőmentes áttelepítés
Az ügyfelek és a fejlesztők számára, akik a saját testreszabott biztonsági mentési megoldás megvalósítása Azure csak tartsa a eltéréseit a növekményes pillanatképek is biztosít. Ez a szolgáltatás ezzel elkerülheti a kezdeti teljes másolat, amely jelentősen csökkenti a biztonsági mentési költségét. Hatékony olvasható, és másolja különbözeti adatokat lehetőséget, és ez az egy másik hatékony képesség, amely lehetővé teszi a fejlesztők számára, a legjobb minőségű biztonsági mentési és vész-helyreállítási felületet Azure vezető még több innovációinak. Beállíthatja a saját biztonsági mentési vagy a vész-Helyreállítási megoldás a virtuális gépek az Azure használatával [Blob pillanatkép](/rest/api/storageservices/snapshot-blob) együtt a [Get tartományokat](/rest/api/storageservices/get-page-ranges) API és a [növekményes másolatot Blob](/rest/api/storageservices/incremental-copy-blob) API, amely akkor is a növekményes adatok könnyen másolása a vész-Helyreállítási használja. 

Ezen kívül sok vállalat rendelkezik már fut a helyszíni adatközpontját a kritikus fontosságú munkaterhelésekhez. Az alkalmazások és szolgáltatások áttelepítéséhez a felhőbe, másolja az adatokat, és előre nem látott probléma a kockázatát a váltás után a szükséges állásidő lenne egyik fő fontos szempont. Sok esetben az állásidő egy showstopper a felhőre való áttérést az is lehet. Azure lap Blobok REST API használatával, áttelepülés a felhőbe a kritikus fontosságú munkaterhelésekhez minimális megszakadását engedélyezésével kezeli ezt a problémát. 

A példák pillanatképének elkészítéséhez és az oldalakra vonatkozó blob visszaállítás egy pillanatképből, tekintse meg a [beállítani a biztonsági mentési folyamat növekményes pillanatképek használata](../../virtual-machines/windows/incremental-snapshots.md) cikk.


## <a name="summary"></a>Összegzés
Azure nagy hangsúlyt fektet a legjobb minőségű élmény a felhasználók számára. Azt a tárolási platform-val készült vállalati szintű tartósságot, az adatokat, így a felhasználók Azure bízni a fontos adataikat. Az Azure Lapblobokat, amely nincs más nyilvános felhőplatform biztosít egyedi API támogatása és a fejlesztői környezetet kínál. Ez zökkenőmentes felhőalapú áttelepítések, felső szintű biztonsági mentés/DR élmény, PaaS és típusú támogatása, elosztott tárolási megoldások és más innovációinak IaaS virtuális gépek/lemezek számos gyártó 1. és 3. fél kapcsolatos vezetett. Általános minden a nyilvános felhő platform, az egyedi funkciók között feladat Azure, és ezekből az Azure-ügyfél előnyöket hozzáadott érték, hogy nincs más felhőplatformokkal képes biztosítani.
