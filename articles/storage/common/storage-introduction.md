---
title: "aaaIntroduction tooAzure tárolási |} Microsoft Docs"
description: "Bevezetés tooAzure tároló, a Microsoft adattárolás hello felhőben."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: robinsh
ms.openlocfilehash: f61324f98d0a8eb24023e4344acdb4ca58bb27f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
<!-- this is hello same version that is in hello MVC branch -->
# <a name="introduction-toomicrosoft-azure-storage"></a>Azure Storage bemutatása tooMicrosoft

A Microsoft Azure Storage egy, a Microsoft által felügyelt felhőszolgáltatás, amely magas rendelkezésre állású, biztonságos, tartós, méretezhető és redundáns tárolást tesz lehetővé. A karbantartást és a kritikus problémák kezelését a Microsoft végzi el Önnek. 

Az Azure Storage a következő három adatszolgáltatást tartalmazza: Blob Storage, File Storage és Queue Storage. BLOB storage támogatja a standard és a prémium szintű storage, prémium szintű Storage használata csak az SSD-k hello leggyorsabb teljesítmény lehetséges. Egy másik szolgáltatás ritkán használt adatok tárolási, hogy lehetővé teszi a ritkán használt adatokhoz egy alacsonyabb költségek a nagy mennyiségű toostorage.

Ebben a cikkben megismerkedhet a hello következő:
* hello Azure Storage szolgáltatás
* hello típusú tárfiókkal
* a blobokhoz, üzenetsorokhoz és fájlokhoz való hozzáférés módjai,
* titkosítás
* replikáció, 
* az adatok a tárolóba vagy a tárolóból való áthelyezése,
* hello sok storage ügyfélkódtáraival érhető el. 


<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->


## <a name="introducing-hello-azure-storage-services"></a>Hello Azure Storage szolgáltatások bemutatása

toouse hello szolgáltatások által biztosított Azure Storage – Blob-tároló, a File storage és a Queue storage--, először hozzon létre egy tárfiókot, és ezután belőle egy adott szolgáltatáshoz, hogy a tárfiókban lévő adatok vihetők át. 

## <a name="blob-storage"></a>Blob Storage

A blobok lényegében ugyanolyan fájlok, mint amilyeneket a számítógépén (vagy táblagépén, mobileszközén stb.) is tárol. Lehetnek képek, Microsoft Excel-fájlok, HTML-fájlok, virtuális merevlemezek (VHD-k), big data (például naplók), vagy adatbázisok biztonsági másolatai – lényegében szinte bármi. BLOB tárolók, amelyek hasonló toofolders vannak tárolva. 

Fájlok Blob Storage tárolóban végzett tárolása, után elérheti őket bárhonnan hello world URL-címekkel, a hello REST-felületen, vagy egy hello Azure SDK storage ügyfélkódtáraival. A tárolók ügyfélkódtárai több nyelvhez, köztük a Node.js, a Java, a PHP, a Ruby, a Python és a .NET nyelvekhez is elérhetők. 

A bloboknak három típusa létezik: a blokkblobok, a hozzáfűző blobok és lapblobok (ezek a VHD-fájlokhoz használatosak).

* Blokkblobok olyan használt toohold szokásos fájlok mentése tooabout 4.7 TB. 
* Lapblobokat a használt toohold elérésű fájlok mentése too8 TB-nál. Ezek a virtuális gépek biztonsági hello VHD-fájlok használhatók.
* Hozzáfűző blobok hello blokkblobokhoz hasonlóan blokkokból épülnek fel, de vannak optimalizálva, műveletek hozzáfűzésére. Ezek többek között információkat toohello azonos blob-naplózás több virtuális gépek használják.

A nagy adatkészleteknél, ahol a hálózati megkötések ellenőrizze vagy feltöltése a tooBlob adattárolás irreális hello hálózaton keresztül küldje el a merevlemez-meghajtók tooMicrosoft tooimport készletét, vagy exportálja az adatokat közvetlenül a hello adatközpont. Lásd: [hello a Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használata](../storage-import-export-service.md).

## <a name="file-storage"></a>File Storage

hello Azure fájlok szolgáltatás lehetővé teszi tooset hello szabványos Server Message Block (SMB) protokoll használatával elérhető magas rendelkezésre állású hálózati fájlmegosztások fel. Azt jelenti, hogy több virtuális gép megoszthatja hello azonos fájlokat az olvasási és írási hozzáférést is. Hello fájlokat hello REST-felületen vagy a hello storage ügyfélkódtáraival is olvasható. 

Egyetlen művelet, amelyik megkülönbözteti az Azure File storage egy vállalati fájlt megosztáson, hogy el tudja érni hello fájlokat bárhonnan a hello world toohello fájlra mutat, és egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot tartalmazó URL-cím. Hozhat létre a SAS-tokenje; egy adott időtartamig lehetővé teszik a kívánt hozzáférés tooa személyes eszköz. 

A fájlmegosztások számos gyakori forgatókönyvhöz használhatók: 

* Számos helyszíni alkalmazás használ fájlmegosztásokat. A szolgáltatás révén egyszerűbben toomigrate ezeket az alkalmazásokat, amelyek közös adatok tooAzure. Csatlakoztatása hello fájl megosztási toohello azonos meghajtó betűjele, hello helyszíni alkalmazást használ, az alkalmazás hello fájlmegosztás hozzáférő hello részét minimális, ha van ilyen módosításait együtt kell működnie.

* A konfigurációs fájlok fájlmegosztásokon tárolhatók, és több virtuális gépről is elérhetők. Eszközök és segédprogramok egy csoport több fejlesztők által használt tárolható egy fájlmegosztást, győződjön meg arról, hogy mindenki választhatók ki, és használják-e a hello ugyanazt a verziót.

* Diagnosztikai naplók, metrikák és összeomlási memóriaképeket tooa fájlmegosztás írása és dolgozni, vagy később elemzett adatok három példák.

Ez idő, az Active Directory-alapú hitelesítés és hozzáférés vezérlési listák (ACL) nem támogatott, de a jövőbeli hello valamikor fogja. hello tárfiók hitelesítő adatainak használt tooprovide hitelesítési hozzáférési toohello fájlmegosztás. Ez azt jelenti, hogy birtokában bárki hello megosztás csatlakoztatva lesz teljes olvasási/írási hozzáférést toohello megosztást.

## <a name="queue-storage"></a>Queue Storage

hello Azure Queue szolgáltatás használt toostore és lekérése üzenetek. Too64 KB méretű be lehet üzenetsor-üzeneteket, és több millió üzenetet tartalmazhat. A várólisták aszinkron módon feldolgozott üzenetek toobe általánosan használt toostore listája. 

Például hogy azt szeretné, hogy a felhasználók toobe képes tooupload képek, és szeretné toocreate miniatűr képek. Az ügyfél várni toocreate hello miniatűrök hello képek feltöltése során lehet. Alternatív toouse várólista lenne. Hello ügyfél a feltöltés befejeződése után írható toohello üzenet-várólista. Akkor kell egy Azure-függvény üdvözlőüzenetére beolvasni hello a várólistából, és hozzon létre hello miniatűrökön. Egyes hello részei a feldolgozási is méretezhető külön-külön, így jobban kézben hangolása azt a használat során.

<!-- this bookmark is used by other articles; you'll need tooupdate them before this goes into production ROBIN-->
## <a name="table-storage"></a>Table Storage
<!-- add a link toohello old table storage toothis paragraph once it's moved -->
A standard Azure Table Storage mostantól a Cosmos DB része. Emellett az Azure Table Storage prémium táblákra vonatkozó ajánlata is elérhető, teljesítményoptimalizált táblákkal, globális elosztással és automatikus másodlagos indexekkel. További toolearn, majd próbálja meg új premium élmény hello, vegye ki [Azure Cosmos DB: tábla API](https://aka.ms/premiumtables).

## <a name="disk-storage"></a>Lemezes tárolás

hello Azure Storage csapat tulajdonosa lemezek, amely tartalmazza az összes felügyelt hello és a nem felügyelt lemezt képességeket virtuális gépek által használt is. Ezekről a szolgáltatásokról további információkért lásd: hello [számítási szolgáltatás dokumentációja](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>A tárfiókok típusai 

Az alábbi táblázatban hello storage-fiókok és mely objektumok használhatók az összes különböző típusú.

|**Tárfiók típusa**|**Általános célú standard**|**Általános célú prémium**|**Blob Storage, a gyakran és a ritkán használt adatok tárolási rétegei**|
|-----|-----|-----|-----|
|**Támogatott szolgáltatások**| Blob, File és Queue szolgáltatások | Blob szolgáltatás | Blob szolgáltatás|
|**Támogatott blobtípusok**|Blokkblobok, lapblobok és hozzáfűző blobok | Lapblobok | Blokkblobok és hozzáfűző blobok|

### <a name="general-purpose-storage-accounts"></a>Általános célú tárfiókok

Kétféle általános célú tárfiók létezik. 

#### <a name="standard-storage"></a>Standard szintű Storage 

hello legszélesebb körben használható storage-fiókok szabványos tárfiókok, amelyek az összes típusú adatokhoz használható. Standard szintű storage-fiókok mágneses media toostore adatok használata.

#### <a name="premium-storage"></a>Prémium szintű Storage

A prémium szintű tárfiók nagy teljesítményű tárolást biztosít a lapblobokhoz, amelyek elsősorban VHD-fájlokhoz használatosak. Prémium szintű tárfiókok SSD toostore adatok használata. A Microsoft minden virtuális gépéhez a prémium szintű Storage szolgáltatást ajánlja.

### <a name="blob-storage-accounts"></a>Blob Storage-fiókok

hello Blob Storage-fiók egy speciális tárfiók használt toostore blokkblobok és hozzáfűző blobok. Az ilyen fiókokban nem lehet lapblobokat tárolni, így VHD-fájlokat sem. Ezek a fiókok lehetővé teszik a hozzáférési réteg tooHot tooset, vagy ritkán; hello réteg bármikor módosíthatja. 

hello gyakran használt adatok hozzáférési réteg a gyakran elért fájlokat szolgál – egy magasabb tárolási költsége kell fizetnie, de hello blobok elérése hello költségét sokkal alacsonyabb. A blobok hello ritkán használt adatok hozzáférési szint tárolja a magasabb költsége hello blobok elérése után kell fizetnie, de tárolók költségét hello sokkal alacsonyabb.

## <a name="accessing-your-blobs-files-and-queues"></a>A blobok, fájlok és üzenetsorok elérése

Mindegyik tárfiók két hitelesítési kulccsal rendelkezik, amely bármelyike használható bármilyen művelethez. Alkalmanként tooenhance biztonsági kulcsok hello keresztül lehet vonni, két kulcs van. Nagyon fontos, hogy ezek a kulcsok biztonságba helyezni, mivel a birtokukban hello fiók nevét, valamint korlátlan hozzáféréssel tooall adatok hello tárfiókban. 

Ez a szakasz a következőhöz: két módon toosecure hello tárfiók és a hozzá kapcsolódó adatokat. A tárfiókban és az adatok védelme kapcsolatos részletes információkért lásd: hello [Azure Storage biztonsági útmutató](storage-security-guide.md).

### <a name="securing-access-toostorage-accounts-using-azure-ad"></a>Hozzáférés biztosítása az Azure AD toostorage fiókok

Egyirányú toosecure hozzáférés tooyour tárolási adata toohello tárfiókkulcsok hozzáférés vezérlése. Az erőforrás-kezelő szerepköralapú hozzáférés-vezérlés (RBAC) hozzárendelheti a szerepkörökhöz toousers, csoportok és alkalmazások. Ezek a szerepkörök vannak társítva tooa meghatározott műveletek engedélyezett vagy le van tiltva. Az RBAC használata toogrant hozzáférés tooa tárfiók csak kezeli hello felügyeleti műveletek tárolási fiók, például a hello hozzáférési réteg módosítása. Az RBAC toogrant access toodata objektumok egy adott tárolóhoz vagy a fájlmegosztásnak például nem használható. Az RBAC toogrant hozzáférés toohello tárfiókkulcsok, amelyből a használt tooread hello adatobjektumainak azonban használhatja. 

### <a name="securing-access-using-shared-access-signatures"></a>Hozzáférés biztonságossá tétele közös hozzáférésű jogosultságkódok használatával 

Közös hozzáférésű jogosultságkód használnia, és hozzáférési házirendek toosecure tárolja az adatok objektumok. A közös hozzáférésű jogosultságkód (SAS), egy biztonsági jogkivonatot, amely egy karakterlánc van csatolva toohello URI, amely lehetővé teszi a toodelegate hozzáférés toospecific tárolási objektumokat és toospecify megkötések például engedélyekkel és hozzáférési számos hello dátum/idő az adott eszköz számára. Ez a szolgáltatás kiterjedt képességeket biztosít. Részletes információkért tekintse meg túl[használatával megosztott hozzáférési aláírásokkal (SAS)](storage-dotnet-shared-access-signature-part-1.md).

### <a name="public-access-tooblobs"></a>Nyilvános hozzáférés tooblobs

hello Blob szolgáltatás lehetővé teszi tooprovide nyilvános hozzáférés tooa tároló és a benne található blobokat, vagy egy adott blob. Ha egy tárolót vagy blobot nyilvánosként jelöl meg, bárki névtelenül, hitelesítés nélkül megtekintheti. Mikor érdemes toodo példát ez esetén rendelkezik egy webhellyel, képek, videó vagy dokumentumok blobtárolóból használ. További információkért lásd: [névtelen olvasási hozzáférés toocontainers és a bináris objektumok kezelése](../blobs/storage-manage-access-to-resources.md) 

## <a name="encryption"></a>Titkosítás

Nincsenek hello tárolási szolgáltatások néhány alapvető típusú titkosítás. 

### <a name="encryption-at-rest"></a>Titkosítás inaktív állapotban 

Engedélyezheti a Storage szolgáltatás titkosítási (SSE) vagy hello fájlok szolgáltatáson (előzetes verzió) vagy hello Azure-tárfiók a Blob szolgáltatás. Ha engedélyezve van, az adott szolgáltatás toohello írt összes adata titkosításra kerül-e írása előtt. Hello adatok olvasásakor az feladatátadás előtt adott vissza. 

### <a name="client-side-encryption"></a>Ügyféloldali titkosítás

hello storage ügyfélkódtáraival rendelkezik metódusok hívása tooprogrammatically adatok titkosítása, mielőtt elküldené a hello ügyfél tooAzure hello keresztülhaladnak a hálózaton keresztül. Az adatok tárolása titkosítva történik, tehát inaktív állapotban is titkosítva vannak. Vissza a hello adatok olvasásakor hello adatokat fogadni után vissza. 

### <a name="encryption-in-transit-with-azure-file-shares"></a>Titkosítás az átvitel során az Azure-fájlmegosztásokkal

További információ a közös hozzáférésű jogosultságkódokról: [Using Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md) (Közös hozzáférésű jogosultságkódok (SAS) használata). Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md) és [hello Azure Storage szolgáltatásainak hitelesítése](https://msdn.microsoft.com/library/azure/dd179428.aspx) biztonságos hozzáférést tooyour tárfiók olvashat.

A tárfiók és a titkosítás biztosításával kapcsolatos további részletekért lásd: hello [Azure Storage biztonsági útmutató](storage-security-guide.md).

## <a name="replication"></a>Replikáció

A sorrend tooensure, hogy az adatok tartósságát Azure Storage hello képességét tookeep rendelkezik (és kezelésére) az adatok többszörös lemásolását. Ezt replikációnak vagy más néven redundanciának nevezzük. A tárfiók üzembe helyezésekor meg kell választania a replikáció típusát. A legtöbb esetben ez a beállítás módosítható hello tárfiók telepítése után. 

Mindegyik tárfiókban elérhető a **helyileg redundáns tárolás (LRS)**. Ez azt jelenti, hogy három másolatot az adatok Azure Storage hello adatközpont által kezelt megadott, amikor hello tárfiók be lett állítva. Ha a változtatások véglegesítése tooone másolja, hello egyéb két másolatot frissítése sikeres visszatérése előtt. Ez azt jelenti, hogy hello három replikákat a rendszer mindig szinkronban. Is hello három másolatot a külön tartalék tartományokban lévő és frissítési tartományt, ami azt jelenti, még akkor is, ha az adatokat tároló csomópont meghibásodik, vagy tett offline toobe frissül érhető el az adatokat. 

**Helyileg redundáns tárolás (LRS)**

Amint azt fent kifejtettük, az LRS esetében az adatok három példányban vannak tárolva egyetlen adatközpontban. Ez kezeli hello probléma adatok elérhetetlenné válik, ha a tárolási csomópontnak meghibásodik vagy offline toobe frissítve lesz végrehajtva, de nem hello a gyorsítás esetében is az egész adatközpont elérhetetlenné válik.

**Zónaredundáns tárolás (ZRS)**

Zónaredundáns tárolás (ZRS) hello három helyi másolatot az adatok, valamint egy másik csoportja az adatok három másolatot tart fenn. hello három példányban második együttesét replikálja a rendszer aszinkron módon belül egy vagy két régióban üzemeltetésében. Megjegyzendő, hogy a ZRS kizárólag általános célú tárfiókokban lévő blokkblobokhoz érhető el. Is, ha létrehozott egy tárfiókot, és a kiválasztott zrs-t, akkor nem alakíthatja más toouse tooany írja be a replikáció, vagy fordítva.

A ZRS-fiókok nagyobb tartósságot biztosítanak az LRS-fiókoknál, azonban nem rendelkeznek metrikákkal vagy naplózási funkciókkal. 

**Georedundáns tárolás (GRS)**

Georedundáns tárolás (GRS) hello három helyi másolatot az adatok egy elsődleges régióban és más engedélykészletre miles hello elsődleges régió elhagyja az egy másodlagos régióban több száz az adatok három másolatot tart fenn. Hiba történne hello elsődleges régió hello esetben Azure Storage hajt végre feladatátvételt toohello másodlagos régióba. 

**Írásvédett georedundáns tárolás (RA-GRS)** 

Írásvédett georedundáns tárolás hasonlít pontosan Georedundáns azzal a különbséggel, hogy be olvasási hozzáférés toohello adatok hello másodlagos helyen. Ha hello elsődleges adatközpont átmenetileg nem érhető el, folytathatja a tooread hello adatok hello másodlagos helyről. Ezt rendkívül hasznos lehet. Például lehet egy webes alkalmazás, amely csak olvasható módra vált és toohello másolat, mutat néhány hozzáférést annak ellenére, hogy a frissítések nem érhetők el. 

> [!IMPORTANT]
> Hogyan adatait a rendszer replikálja a tárfiók létrehozása után csak hello fiók létrehozásakor a ZRS megadott módosíthatja. Vegye figyelembe azonban, hogy egy egyszeri adatátviteli költségek, ha az LRS tooGRS vagy RA-GRS fel Önnek.
>

A replikációval kapcsolatos további információk: [Azure Storage replication](storage-redundancy.md) (Az Azure Storage replikációja).

Vész-helyreállítási információkért lásd: [egy Azure Storage tervezett kimaradás esetén milyen toodo](storage-disaster-recovery-guidance.md).

A példa bemutatja, hogyan tooleverage RA-GRS tárolási tooensure magas rendelkezésre állású, lásd: [tervezése magas rendelkezésre álló használó alkalmazások RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="transferring-data-tooand-from-azure-storage"></a>Adatok tooand átvitele az Azure Storage-ból

Használhat hello AzCopy parancssori segédprogram toocopy blob és a fájladatok a tárfiókon belül vagy tárfiókok között. Lásd: hello alábbi cikkek segítséget:

* [Transfer data with AzCopy for Windows](storage-use-azcopy.md) (Adatok áthelyezése az AzCopy segédprogrammal Windows rendszeren)
* [Transfer data with AzCopy for Linux](storage-use-azcopy-linux.md) (Adatok áthelyezése az AzCopy segédprogrammal Linux rendszeren)

AzCopy épül hello [Azure adatátviteli könyvtárra](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), amely érhető el jelenleg előzetes.

hello Azure Import/Export szolgáltatás lehet használt tooimport vagy exportálási nagy mennyiségű blob adatok tooor importáljon a tárfiókba. Készítse elő, és több merevlemez-meghajtók tooan Azure-adatközpontba, ahol fogja átvinni hello adatok hello merevlemez-meghajtók és a levelezési és hello merevlemezek biztonsági tooyou küldése. Hello Import/Export szolgáltatás kapcsolatos további információkért lásd: [hello a Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használata](../storage-import-export-service.md).

## <a name="pricing"></a>Díjszabás

Az Azure Storage árazással kapcsolatos részletes információkért lásd: hello [árazás lap](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Storage API-k, kódtárak és eszközök
Az Azure Storage-erőforrások bármely olyan nyelvvel hozzáférhetők, amelyekkel HTTP/HTTPS kérelmek indíthatók. Ezenfelül az Azure Storage számos népszerű nyelvhez biztosít programozási kódtárakat. Ezek a kódtárak sok szempontból leegyszerűsítik az Azure Storage használatát, mivel számos részletet kezelnek (például a szinkron és aszinkron hívás, műveletek kötegelése, kivételek kezelése, automatikus újrapróbálkozások, működési viselkedés stb.). Szalagtárak jelenleg a következő nyelvek hello és platformokhoz lévő többi hello folyamat:

### <a name="azure-storage-data-services"></a>Azure Storage-adatszolgáltatások
* [A Storage szolgáltatások REST API-ja](/rest/api/storageservices/)
* [A Storage ügyféloldali kódtára a .NET-hez](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [A Storage ügyféloldali kódtára a C++ programnyelvhez](https://github.com/Azure/azure-storage-cpp)
* [A Storage ügyféloldali kódtára a Javához/Androidhoz](https://azure.microsoft.com/develop/java/)
* [A Storage ügyféloldali kódtára a Node.js-hez](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [A Storage ügyféloldali kódtára a PHP-hez](https://azure.microsoft.com/develop/php/)
* [A Storage ügyféloldali kódtára a Pythonhoz](https://azure.microsoft.com/develop/python/)
* [A Storage ügyféloldali kódtára a Rubyhoz](https://azure.microsoft.com/develop/ruby/)
* [Storage-parancsmagok a PowerShellhez](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)
* [Storage-parancsok a CLI 2.0-hoz](/cli/azure/storage)

## <a name="next-steps"></a>Következő lépések

* [További tudnivalók a Blob Storage-ról](../blobs/storage-blobs-introduction.md)
* [További tudnivalók a File Storage-ról](../storage-files-introduction.md)
* [További tudnivalók a Queue Storage-ról](../queues/storage-queues-introduction.md)

<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->

<!-- FIGURE OUT WHAT tooDO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for hello following languages and platforms, with others in hello pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
toolearn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>Rendszergazdáknak
* [Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)](storage-powershell-guide-full.md)
* [Using the Azure CLI with Azure Storage (Az Azure CLI és az Azure Storage együttes használata)](../storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET-fejlesztőknek
* [Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel](../storage-dotnet-how-to-use-queues.md)
* [Ismerkedés a Windowshoz készült Azure File Storage szolgáltatással](../storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android-fejlesztőknek
* [Hogyan toouse Blob storage-ának Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Hogyan toouse a Queue storage a Javával](../storage-java-how-to-use-queue-storage.md)
* [Hogyan toouse File storage Java](../storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js-fejlesztőknek
* [Hogyan toouse Blob-tároló Node.js-ből](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Hogyan toouse a Table storage Node.js-ből](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Hogyan toouse a Queue storage Node.js-ből](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP-fejlesztőknek
* [Hogyan toouse Blob-tároló php-ből](../blobs/storage-php-how-to-use-blobs.md)
* [Hogyan toouse a Table storage php-ből](../../cosmos-db/table-storage-how-to-use-php.md)
* [Hogyan toouse a Queue storage php-ből](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby-fejlesztőknek
* [Hogyan toouse Blob storage-ának Ruby](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Hogyan toouse a Queue storage a Rubyhoz](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python-fejlesztőknek
* [Hogyan toouse Blob storage-ának Python](../blobs/storage-python-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Hogyan toouse a Queue storage a Python](../storage-python-how-to-use-queue-storage.md)   
* [Hogyan toouse File storage Python](../storage-python-how-to-use-file-storage.md) 
-->