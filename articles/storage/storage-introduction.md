---
title: "aaaIntroduction tooAzure tárolási |} Microsoft Docs"
description: "Azure Storage, a Microsoft online adattárolás hello felhőben áttekintése. Ismerje meg, hogyan toouse hello legjobb elérhető felhőalapú tárolómegoldás az alkalmazásokban."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/26/2017
ms.author: marsma
ms.openlocfilehash: dec8280c77f4b23df4c2a471e1d755e365c14e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-storage"></a>Azure Storage bemutatása tooMicrosoft

## <a name="overview"></a>Áttekintés
Az Azure Storage egy hello felhőalapú tárolómegoldás modern alkalmazások tartósságot, rendelkezésre állását és méretezhetőségét toomeet hello az ügyfelek igényeinek megfelelően. A cikket olvasó fejlesztők, informatikai szakemberek és üzleti döntéshozók megtudhatják:

* Mi is az Azure Storage, és hogyan használható fel a felhőalapú, mobil-, kiszolgálói vagy asztali alkalmazásokhoz
* Milyen típusú adatok tárolhatók hello Azure Storage szolgáltatás: blob-(Objektumadatok), NoSQL-táblázatok adatai, üzenetsorbeli üzenetek és fájlmegosztások.
* Hogyan kezeli az Azure Storage access tooyour adatok
* Hogyan teszi tartóssá az Azure Storage-adatokat a redundancia és a replikáció
* Ha toogo következő toobuild az első Azure Storage-alkalmazás

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!-- tooget up and running with Azure Storage quickly, see [Get started with Azure Storage in five minutes](storage-getting-started-guide.md). -->

Az Azure Storage használatához szükséges eszközökkel, kódtárakkal és egyéb forrásokkal kapcsolatban további információt talál a cikk [Következő lépések](#next-steps) szakaszában.

## <a name="what-is-azure-storage"></a>Mi az Azure Storage?
A felhőalapú számítógép-használat új alkalmazási helyzeteket tesz lehetővé azon alkalmazások számára, amelyek adataihoz méretezhető, tartós és nagy rendelkezésre állású tárolóra van szükség – a Microsoft épp ezért fejlesztette ki az Azure Storage szolgáltatást. A hozzáadása toomaking azt lehetséges a fejlesztők toobuild nagyméretű alkalmazások toosupport új forgatókönyvek használhatók, Azure Storage is hello tárolási alapot nyújt az Azure Virtual Machines, a további testament tooits szolgáltatás megbízhatóságára.

Az Azure Storage egy nagymértékben méretezhető, így tárolja, és több száz terabájtos adatok toosupport hello big data forgatókönyvéhez szükséges tudományos és üzleti elemzések, vagy a médiaalkalmazások feldolgozni. Vagy tárolhatja az adatokat a kisvállalkozások webhelyeihez szükséges kis mennyiségű hello. Csökkennek az igényei, csak fizetnie hello adatokat tárolja. Az Azure Storage jelenleg több tízbillió egyéni ügyfélobjektumot tárol, és másodpercenként átlagosan több millió lekérést szolgál ki.

Az Azure Storage egy rugalmas, így hatalmas, globális közönségek, az alkalmazások és méretezhető ezeket az alkalmazásokat, szükség esetén – mind hello tárolt adatok mennyisége és hello rajta intézett kérések száma. Csak a valóban használt funkciókért kell fizetnie, és csak akkor, amikor használja őket.

Az Azure Storage automatikus particionálási rendszert használ, amely a forgalomtól függően automatikusan kiegyenlíti az adatterhelést. Ez azt jelenti, hogy hello nő az alkalmazás iránti igény, mint Azure Storage automatikusan lefoglalja számára hello megfelelő erőforrásokkal toomeet őket.

Az Azure Storage elérhető-e a bárhol hello world bármilyen alkalmazás, hogy hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön fut. vagy táblagépét eszköz. Használhatja az Azure Storage olyan mobil forgatókönyveknél, ha a hello alkalmazás hello eszközön tárolja az adatok egy részét, és azt egy hello felhőben tárolt teljes adatkészlettel szinkronizálja.

Az Azure Storage ügyfelei a kényelmes fejlesztés érdekében számos operációs rendszert (beleértve a Windows és a Linux rendszereket is) és programnyelvet (többek között a .NET, a Java, a Node.js, a Python, a Ruby, a PHP, a C++ és a mobil programnyelveket) támogatnak. Az Azure Storage is közzétesz keresztül egyszerű REST API-k, amelyek elérhető tooany ügyfél HTTP/HTTPS kapcsolaton adatot küldeni és fogadni képes erőforrásokat.

A prémium szintű Storage nagy teljesítményű, kis késleltetésű lemeztámogatást biztosít az Azure virtuális gépeken futtatott, nagy adatátviteli teljesítményt igénylő számítási feladatokhoz. Prémium szintű Azure Storage több állandó adatok lemezek tooa virtuális géphez csatolása és konfigurálásuk toomeet a teljesítményre vonatkozó követelmények. A maximális adatátviteli teljesítményt a minden adatlemezről elkészült, SSD-n tárolt biztonsági másolat biztosítja a prémium szintű Azure Storage-ban. A Premium Storage részletesebb áttekintéséért lásd: [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md) (Premium Storage: Nagy teljesítményű tárterület az Azure virtuális gépek számítási feladataihoz).

## <a name="introducing-hello-azure-storage-services"></a>Hello Azure Storage szolgáltatások bemutatása
Az Azure storage hello a következő négy szolgáltatást biztosítja: Blob-tároló, a Table storage, Queue storage és a File storage.

* A Blob Storage a strukturálatlan objektumadatokat tárolja. Egy blob állhat bármilyen szövegből vagy bináris adatból, lehet például egy dokumentum, egy médiafájl vagy egy alkalmazástelepítő. A BLOB storage hivatkozott tooas objektumtárnak is.
* A Table Storage a strukturált adatkészleteket tárolja. A TABLE storage a NoSQL-Kulcsattribútumok adattára, amely lehetővé teszi a gyors fejlesztést és gyors hozzáférést toolarge mennyiségű adat.
* A Queue Storage megbízható üzenetküldést biztosít a munkafolyamat-feldolgozáshoz és a felhőszolgáltatás összetevői közötti kommunikációhoz.
* A File Storage közös tárterületet hello szabványos SMB-protokollt használó örökölt alkalmazások számára biztosít. Az Azure virtuális gépek és felhőszolgáltatások megosztott csatlakoztatott megosztásokon keresztül alkalmazások összetevői között, és a helyszíni alkalmazások hozzáférhetnek a fájladatok olyan megosztáson található hello Fájlszolgáltatás REST API-n keresztül.

Azure-tárfiók olyan biztonságos fiók, amely lehetővé teszi az Azure Storage tooservices eléréséhez. A tárfiók biztosítja hello egyedi névteret a tárterület erőforrásainak. hello az alábbi képen látható hello tárfiókokban hello Azure storage erőforrásainak kapcsolatai:

![Azure Storage-erőforrások](./media/storage-introduction/storage-concepts.png)

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[!INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob Storage
A felhasználók nagy mennyiségű strukturálatlan adatok toostore hello felhőben a Blob storage költséghatékony, méretezhető megoldást kínál. Használhatja a Blob storage toostore tartalom többek között:

* Dokumentumok
* Közösségi adatok (fényképek, videók, zene és blogok)
* Fájlok, számítógépek, adatbázisok és eszközök biztonsági másolatai
* Webalkalmazásokhoz tartozó képek és szövegek
* Konfigurációs adatok a felhőalapú alkalmazásokhoz
* Big data (naplók és egyéb nagy adatkészletek)

Minden blob egy tárolóba van rendezve. A tárolók nagy előnye egy hasznos módja tooassign biztonsági házirendek toogroups objektumok. A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat, és egy tároló korlátlan számú blobot toohello 500 TB-os kapacitásán belül hello storage-fiók mentése tartalmazhat.

A Blob Storage háromféle blobot biztosít: blokkblobokat, hozzáfűző blobokat és lapblobokat.

* A blokkblobok a felhőbeli objektumok streamelésére és tárolására vannak optimalizálva, és kiválóan alkalmasak a dokumentumok, médiafájlok, biztonsági másolatok stb. tárolására.
* Hozzáfűző blobok hasonló tooblock blobokat, de vannak optimalizálva, műveletek hozzáfűzésére. A hozzáfűző blob csak egy új blokkot toohello end hozzáadásával lehet frissíteni. Hozzáfűző blobok olyan ahhoz hasonló forgatókönyvek esetén jelentkezik, amikor új adatok kell toobe írt hello blob csak toohello végéhez.
* A lapblobok az infrastruktúra-szolgáltatási lemezek optimalizált blobok, amelyek és támogató véletlenszerű ír, és lehet, hogy másolatot too1 TB-nál. Egy Azure virtuálisgép-hálózathoz csatlakoztatott infrastruktúra-szolgáltatási lemez tulajdonképpen egy lapblobként tárolt VHD.

A nagy adatkészleteknél, ahol a hálózati megkötések ellenőrizze vagy feltöltése a tooBlob adattárolás irreális hello hálózaton keresztül küldje el a merevlemez-meghajtóról tooMicrosoft tooimport, vagy exportálja az adatokat közvetlenül a hello adatközpont. Lásd: [hello a Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használata](storage-import-export-service.md).

## <a name="table-storage"></a>Table Storage

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

A modern alkalmazásokhoz gyakran az előző generációs szoftvereknél jobban méretezhető és rugalmasabb adattárolók szükségesek. A TABLE storage magas rendelkezésre állású, nagymértékben méretezhető tárolót, kínál, így az alkalmazás automatikusan átméretezheti magát toomeet felhasználó igény szerint. A Table Storage a Microsoft NoSQL kulcs-/attribútumtára, amely séma nélküli kivitelezésében különbözik a hagyományos relációs adatbázisoktól. Egy séma nélküli adattár alkalmazás igényeinek változásával kell hello az adatokat könnyen tooadapt. A TABLE storage könnyen toouse, így a fejlesztők gyorsan létrehozhatják benne alkalmazásaikat. Hozzáférés toodata gyors és költséghatékony, minden típusú alkalmazások.  Hasonló adatmennyiséggel számolva a Table Storage általában határozottan kevesebb költséggel jár, mint egy hagyományos SQL.

A Table Storage egy kulcs-/attribútumtár, ami azt jelenti, hogy a táblázatok minden értékét egy típusos tulajdonságnévvel tárolja. hello tulajdonságnév szűrést és a kiválasztási feltételek megadásához használható. A tulajdonságok és értékeik gyűjteménye egy entitást alkot. Mivel a Table storage séma nélküli, két entitása hello azonos tábla tulajdonságok különböző gyűjteményeit tartalmazhatja, és ezek a Tulajdonságok különböző típusúak lehetnek.

Table storage toostore rugalmas adatkészleteket, például webes alkalmazásokat, címtárakat, eszközadatokat és bármilyen más típusú metaadatokat, amelyek a szolgáltatásnak szüksége van a felhasználói adatok is használhatja.  Korlátlan számú entitást tárolhat egy táblát, és a storage-fiókok tartalmazhat korlátlan számú táblát toohello kapacitásán belül hello storage-fiók mentése.

Hasonlóan fejlesztők kezelése és hozzáférhetnek a Table storage szabványos REST-protokollal, azonban Table storage is támogatja a hello OData protokoll egy részhalmazát egyszerűsítése speciális lekérdezési képességeket, valamint engedélyezi az JSON és az AtomPub (XML-alapú) formázza.

A mai internetalapú alkalmazásoknál a Table storage hasonló NoSQL-adatbázisok relációs adatbázisok kínálnak a népszerű alternatív tootraditional.

## <a name="queue-storage"></a>Queue Storage
A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni. A Queue storage megbízható üzenetkezelési megoldást kínál a alkalmazás-összetevő közötti aszinkron kommunikációhoz e hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön futnak. A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.

Egy tárfiók tetszőleges számú üzenetsort tartalmazhat. Tartalmazhat korlátlan számú üzenetet toohello kapacitásán belül hello storage-fiók mentése. Egyes üzenetek mentése too64 KB méretű is lehet.

## <a name="file-storage"></a>File Storage
hello Azure fájlok szolgáltatás lehetővé teszi tooset hello szabványos Server Message Block (SMB) protokoll használatával elérhető magas rendelkezésre állású hálózati fájlmegosztások fel. Azt jelenti, hogy több virtuális gép megoszthatja hello azonos fájlokat az olvasási és írási hozzáférést is. Hello fájlokat hello REST-felületen vagy a hello storage ügyfélkódtáraival is olvasható.

Egyetlen művelet, amelyik megkülönbözteti az Azure File storage egy vállalati fájlt megosztáson, hogy el tudja érni hello fájlokat bárhonnan a hello world toohello fájlra mutat, és egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot tartalmazó URL-cím. Hozhat létre a SAS-tokenje; egy adott időtartamig lehetővé teszik a kívánt hozzáférés tooa személyes eszköz.

A fájlmegosztások számos gyakori forgatókönyvhöz használhatók:

* Számos helyszíni alkalmazás használ fájlmegosztásokat. A szolgáltatás révén egyszerűbben toomigrate ezeket az alkalmazásokat, amelyek közös adatok tooAzure. Csatlakoztatása hello fájl megosztási toohello azonos meghajtó betűjele, hello helyszíni alkalmazást használ, az alkalmazás hello fájlmegosztás hozzáférő hello részét minimális, ha van ilyen módosításait együtt kell működnie.

* A konfigurációs fájlok fájlmegosztásokon tárolhatók, és több virtuális gépről is elérhetők. Eszközök és segédprogramok egy csoport több fejlesztők által használt tárolható egy fájlmegosztást, győződjön meg arról, hogy mindenki választhatók ki, és használják-e a hello ugyanazt a verziót.

* Diagnosztikai naplók, metrikák és összeomlási memóriaképeket tooa fájlmegosztás írása és dolgozni, vagy később elemzett adatok három példák.

Ez idő, az Active Directory-alapú hitelesítés és hozzáférés vezérlési listák (ACL) nem támogatott, de a jövőbeli hello valamikor fogja. hello tárfiók hitelesítő adatainak használt tooprovide hitelesítési hozzáférési toohello fájlmegosztás. Ez azt jelenti, hogy birtokában bárki hello megosztás csatlakoztatva lesz teljes olvasási/írási hozzáférést toohello megosztást.

## <a name="access-tooblob-table-queue-and-file-resources"></a>Hozzáférés tooBlob, Table, Queue és fájl erőforrások
Alapértelmezés szerint csak hello tárfiók-tulajdonos hello tárfiókban lévő erőforrások eléréséhez. Hello biztonsági adatait a fiók erőforrásai felé irányuló összes kérelmet hitelesíteni kell. A hitelesítés megosztott kulcsos modellen alapszik. Blobok névtelen hitelesítéshez beállított toosupport is lehet.

A tárfiókhoz a létrehozáskor két titkos hívóbetű jár, amelyek a hitelesítéshez használhatók. Két kulcs biztosítja, hogy az alkalmazás akkor is elérhető, ha a közös kulcskezelés biztonság rendszeresen újragenerálja hello kulcsok.

Ha tooallow szabályozott felhasználók hozzáférési tooyour tárolási erőforrások kell, majd létrehozhat egy közös hozzáférésű jogosultságkódot. A közös hozzáférésű jogosultságkód (SAS) egy jogkivonatot, amely lehet, hogy lehetővé teszi, hogy meghatalmazott hozzáférést tooa tárolási erőforrás hozzáfűzött tooa URL-címe. Bárki, aki rendelkezik hello token férhetnek hozzá a hello erőforráshoz a kódban toowith hello engedélyekkel, hello ideje, hogy érvényes. A 2015-04-05-ös verzió óta az Azure Storage két közös hozzáférésű jogosultságkód-típust támogat: a szolgáltatásalapú SAS-t és a fiókalapú SAS-t.

hello szolgáltatás SAS delegáltak csak egy hello tárolószolgáltatások tooa erőforrás elérésére: hello Blob, a várólista, a tábla vagy a fájl szolgáltatás.

Fiókalapú SAS egy vagy több hello tárolószolgáltatások hozzáférés tooresources. Hozzáférés tooservice szintű műveletek nem érhetők el a szolgáltatásalapú SAS is átadhatja. Delegálhatja hozzáférés tooread, írási és törlési műveletek a blobtárolók, táblák, üzenetsorok és fájlmegosztások a szolgáltatásalapú SAS nem engedélyezett.

Végezetül pedig meghatározhatja, hogy egy tároló a blobjai, illetve egy adott blob nyilvánosan elérhető-e. Ha egy tárolót vagy blobot nyilvánosként jelöl meg, bárki névtelenül, hitelesítés nélkül megtekintheti.  A nyilvános tárolók és blobok az olyan erőforrások közzétételénél hasznosak, mint például a webhelyeken tárolt médiafájlok és dokumentumok.  egy globális közönség számára toodecrease a hálózati késés, gyorsítótárazhatja a webhelyek hello Azure CDN által használt blobadatokat.

További információ a közös hozzáférésű jogosultságkódokról: [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Közös hozzáférésű jogosultságkódok (SAS) használata). Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md) és [hello Azure Storage szolgáltatásainak hitelesítése](https://msdn.microsoft.com/library/azure/dd179428.aspx) biztonságos hozzáférést tooyour tárfiók olvashat.

## <a name="replication-for-durability-and-high-availability"></a>Replikáció a tartósság és magas rendelkezésre állás érdekében
hello adatokat a Microsoft Azure storage-fiók mindig van replikálva tooensure tartósság és magas rendelkezésre állású. Replikációs másolja az adatokat, vagy a belül hello ugyanabban az adatközpontban, illetve tooa második adatközpont, attól függően, hogy a replikációs beállítás választja. Replikációs védi az adatokat, és megőrzi az alkalmazás üzemidő hello eseményben az átmeneti hardverhibák esetén. Ha az adatok replikált tooa második adatközpontok, is attól óvja meg az adatok hello elsődleges helyen végzetes hiba ellen.

Replikációs biztosítja, hogy a tárfiók megfelel-e a hello [szolgáltatásiszint-szerződés (SLA) a tároláshoz](https://azure.microsoft.com/support/legal/sla/storage/) még akkor is, hello tapasztalt hibák. Tekintse meg az Azure Storage információt SLA hello biztosítja, hogy a tartósság és a rendelkezésre állási.

Amikor létrehoz egy tárfiókot, hello a következő replikációs lehetőségek közül választhat:

* **Helyileg redundáns tárolás (LRS)**. A helyileg redundáns tárolás három másolatot tart fenn adatairól. A rendszer háromszor replikálja az LRS-t egy régió egyetlen adatközpontján belül. LRS védi az adatokat, normál hardverhibák, azonban nem hello sikertelen volt-e egyetlen adatközpontba.

    Az LRS kedvezményes áron vásárolható meg. A maximális tartósság érdekében az alábbiakban ismertetett georedundáns tárolás használata javasolt.
* **Zónaredundáns tárolás (ZRS)**. A zónaredundáns tárolás három másolatot tart fenn adatairól. A ZRS van replikálja az adatokat két toothree létesítményekben, egy vagy két régióban LRS-nél nagyobb tartósságot biztosít. A ZRS biztosítja az adatok tartósságát egyetlen régión belül.

    A ZRS az LRS-nél nagyobb tartósságot biztosít, azonban a maximális tartósság érdekében az alábbiakban ismertetett georedundáns tárolás használata javasolt.

  > [!NOTE]
  > A ZRS jelenleg csak a blokkblobokhoz érhető el, és csak a 2014-02-14-es és frissebb verziókban támogatott.
  >
  > Miután létrehozott egy tárfiókot, és kijelölt zrs-t, akkor nem alakíthatja más toouse tooany írja be a replikáció, vagy fordítva.
  >
  >
* **Georedundáns tárolás (GRS)**. A GRS hat másolatot tart fenn adatairól. A GRS az adatok hello elsődleges régión belül háromszor replikálja, és a rendszer háromszor replikálja az miles hello elsődleges régióban, így hello legmagasabb szintű tartósságot befejeződött egy másodlagos régióban több száz. Hiba történne hello elsődleges régió hello esetben Azure Storage fogja feladatátvételi toohello másodlagos régióba. A GRS biztosítja az adatok tartósságát két külön régióban.

    További információ az elsődleges és másodlagos régiók párosításairól: [Az Azure régiói](https://azure.microsoft.com/regions/).
* **Írásvédett georedundáns tárolás (RA-GRS)**. Írásvédett georedundáns tárolás az adatokat tooa másodlagos földrajzi helyre replikálja, és is biztosít az olvasási hozzáférés tooyour adatok hello másodlagos helyen. Írásvédett georedundáns tárolás lehetővé teszi tooaccess az adatok hello elsődleges vagy másodlagos helyre hello, hello eseményben valamely helyszín elérhetetlenné válik. Írásvédett georedundáns tárolás beállítás hello alapértelmezett alapértelmezés szerint a tárfiók létrehozásakor.

  > [!IMPORTANT]
  > Hogyan adatait a rendszer replikálja a tárfiók létrehozása után csak hello fiók létrehozásakor a ZRS megadott módosíthatja. Vegye figyelembe azonban, hogy egy egyszeri adatátviteli költségek, ha az LRS tooGRS vagy RA-GRS fel Önnek.
  >
  >

További információ az Azure Storage replikációs beállításairól: [Azure Storage replication](storage-redundancy.md) (Azure Storage replikációja).

A tárfiók replikációjának díjszabásáról itt tájékozódhat: [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/). Tekintse meg [Az Azure régiói](https://azure.microsoft.com/regions/#services) című lapot azzal kapcsolatban, hogy az egyes régiókban mely szolgáltatások érhetőek el.

További információ az Azure Storage tartósságának szerkezeti részleteiről: [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (SOSP tanulmány – Azure Storage: Egy magas rendelkezésre állású, felhőalapú szolgáltatás erős konzisztenciával).

## <a name="transferring-data-tooand-from-azure-storage"></a>Adatok tooand átvitele az Azure Storage-ból
A tárfiókon belül vagy tárfiókok között használható hello AzCopy parancssori segédprogram toocopy blob, a fájl és a tábla adatai. Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) további információt.

AzCopy épül hello [Azure adatátviteli könyvtárra](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), amely érhető el jelenleg előzetes.

hello Azure Import/Export szolgáltatás lehetővé teszi egy módon tooimport blob adatokat, vagy exportálja a blobadatokat importáljon a tárfiókba, a merevlemez-meghajtóról lemez toohello Azure-adatközpontba postán keresztül. Hello Import/Export szolgáltatás kapcsolatos további információkért lásd: [hello a Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használata](storage-import-export-service.md).

## <a name="pricing"></a>Díjszabás
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Storage API-k, kódtárak és eszközök
Az Azure Storage-erőforrások bármely olyan nyelvvel hozzáférhetők, amelyekkel HTTP/HTTPS kérelmek indíthatók. Ezenfelül az Azure Storage számos népszerű nyelvhez biztosít programozási kódtárakat. Ezek a kódtárak sok szempontból leegyszerűsítik az Azure Storage használatát, mivel számos részletet kezelnek (például a szinkron és aszinkron hívás, műveletek kötegelése, kivételek kezelése, automatikus újrapróbálkozások, működési viselkedés stb.). Szalagtárak jelenleg a következő nyelvek hello és platformokhoz lévő többi hello folyamat:

### <a name="azure-storage-data-services"></a>Azure Storage-adatszolgáltatások
* [A Storage szolgáltatások REST API-ja](http://msdn.microsoft.com/library/azure/dd179355.aspx)
* [A Storage ügyféloldali kódtára a .NET-keretrendszerhez, a Windows Phone-hoz és a Windows-futtatókörnyezethez](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [A Storage ügyféloldali kódtára a C++ programnyelvhez](https://github.com/Azure/azure-storage-cpp)
* [A Storage ügyféloldali kódtára a Javához/Androidhoz](https://azure.microsoft.com/develop/java/)
* [A Storage ügyféloldali kódtára a Node.js-hez](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [A Storage ügyféloldali kódtára a PHP-hez](https://azure.microsoft.com/develop/php/)
* [A Storage ügyféloldali kódtára a Rubyhoz](https://azure.microsoft.com/develop/ruby/)
* [A Storage ügyféloldali kódtára a Pythonhoz](https://azure.microsoft.com/develop/python/)
* [Storage-parancsmagok a PowerShell 1.0-hoz](/powershell/module/azurerm.storage/#storage)

### <a name="azure-storage-management-services"></a>Azure Storage kezelési szolgáltatások
* [A Storage erőforrás-szolgáltató REST API-ja – referencia](/rest/api/storagerp/)
* [Storage erőforrás-szolgáltató ügyfél a .NET-hez](/dotnet/api/microsoft.azure.management.storage)
* [A Storage erőforrás-szolgáltató parancsmagjai a PowerShell 1.0-hoz](/powershell/module/azure.storage)
* [A Storage szolgáltatásfelügyelet REST API-ja](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure Storage adatátviteli szolgáltatások
* [A Storage Import/Export szolgáltatás REST API-ja](storage-import-export-service.md)
* [A Storage adatátviteli ügyféloldali kódtára a .NET-hez](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Eszközök és segédprogramok
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Azure Storage-ügyféleszközök](storage-explorers.md)
* [Azure SDK-k és eszközök](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy parancssori segédprogram](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Következő lépések
További információ az Azure Storage toolearn ismerheti meg ezeket az erőforrásokat:

### <a name="documentation"></a>Dokumentáció
* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)
* [Tárfiók létrehozása](storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>Rendszergazdáknak
* [Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)](storage-powershell-guide-full.md)
* [Using the Azure CLI with Azure Storage (Az Azure CLI és az Azure Storage együttes használata)](storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET-fejlesztőknek
* [Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel](storage-dotnet-how-to-use-blobs.md)
* [Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel](storage-dotnet-how-to-use-tables.md)
* [Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel](storage-dotnet-how-to-use-queues.md)
* [Ismerkedés a Windowshoz készült Azure File Storage szolgáltatással](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android-fejlesztőknek
* [Hogyan toouse Blob storage-ának Java](storage-java-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Java](storage-java-how-to-use-table-storage.md)
* [Hogyan toouse a Queue storage a Javával](storage-java-how-to-use-queue-storage.md)
* [Hogyan toouse File storage Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js-fejlesztőknek
* [Hogyan toouse Blob-tároló Node.js-ből](storage-nodejs-how-to-use-blob-storage.md)
* [Hogyan toouse a Table storage Node.js-ből](storage-nodejs-how-to-use-table-storage.md)
* [Hogyan toouse a Queue storage Node.js-ből](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP-fejlesztőknek
* [Hogyan toouse Blob-tároló php-ből](storage-php-how-to-use-blobs.md)
* [Hogyan toouse a Table storage php-ből](storage-php-how-to-use-table-storage.md)
* [Hogyan toouse a Queue storage php-ből](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby-fejlesztőknek
* [Hogyan toouse Blob storage-ának Ruby](storage-ruby-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Ruby](storage-ruby-how-to-use-table-storage.md)
* [Hogyan toouse a Queue storage a Rubyhoz](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python-fejlesztőknek
* [Hogyan toouse Blob storage-ának Python](storage-python-how-to-use-blob-storage.md)
* [Hogyan toouse Table storage-ának Python](storage-python-how-to-use-table-storage.md)
* [Hogyan toouse a Queue storage a Python](storage-python-how-to-use-queue-storage.md)
* [Hogyan toouse File storage Python](storage-python-how-to-use-file-storage.md)
