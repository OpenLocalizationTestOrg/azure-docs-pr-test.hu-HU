---
title: "aaaUsing Azure Import/Export tootransfer adatok tooand blobtárolóból |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate importálni és exportálni a feladatokat az Azure-portál a adatok tooand visz át a blob storage hello."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: muralikk
ms.openlocfilehash: 0be486a4badf2127b4613f3e9664e4cd308d3298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-microsoft-azure-importexport-service-tootransfer-data-tooblob-storage"></a>Hello Microsoft Azure Import/Export szolgáltatás tootransfer tooblob adattárolás használata
hello Azure Import/Export szolgáltatás lehetővé teszi a toosecurely átviteli nagymennyiségű adat tooAzure blob-tároló szállítási merevlemez-meghajtók tooan az Azure data center által. A szolgáltatás tootransfer az az Azure blob storage toohard meghajtók használata is, és tooyour helyszíni hely szolgáltatástól. Ez a szolgáltatás megfelelő helyzetekben. Ha több terabájt (TB) az Azure-ból adatokat tooor tootransfer kívánt, de vagy feltöltése a hello hálózaton keresztül toolimited sávszélesség miatt lehetetlen vagy nagy hálózati költségeket.

hello szolgáltatást igényli, hogy a merevlemez-meghajtók legyen-e a BitLocker meghajtótitkosítással hello biztonsági adatait. hello szolgáltatás mindkét hello klasszikus és az Azure Resource Manager storage-fiókok (standard és ritkán használt adatok réteg) szerepel a nyilvános Azure régiói összes hello támogatja. A cikk későbbi részében megadott hello támogatott helyek merevlemez-meghajtók tooone kell küldje el.

Ebből a cikkből megismerheti több hello Azure Import/Export szolgáltatásról, és hogyan tooship meghajtók az adatok tooand másolására az Azure Blob-tárolóból.

## <a name="when-should-i-use-hello-azure-importexport-service"></a>Mikor használjak hello Azure Import/Export szolgáltatás?
Érdemes lehet Azure Import/Export szolgáltatás feltöltésekor a rendszer vagy adatok letöltése hello hálózaton túl lassú, vagy további hálózati sávszélesség első megfizethetetlenné.

Forgatókönyvek például használhatja ezt a szolgáltatást:

* Áttelepítési adatok toohello felhő: gyors áthelyezése a nagy mennyiségű adatok tooAzure, és hatékonyan költség.
* Tartalomterjesztést: küldéséhez az adatok tooyour felhasználói helyek.
* Biztonsági mentés: A helyszíni adatok toostore biztonsági másolatokat az Azure blob storage igénybe vehet.
* Adat-helyreállítás: nagy mennyiségű blob storage-ban tárolt adatok helyreállításához, és annak tooyour helyszíni hely kézbesíteni.

## <a name="prerequisites"></a>Előfeltételek
Ebben a szakaszban látható hello Előfeltételek szükséges toouse ezt a szolgáltatást. Olvassa el őket figyelmesen a meghajtók, mielőtt.

### <a name="storage-account"></a>Tárfiók
Meglévő Azure-előfizetésre és egy vagy több tároló fiókok toouse hello Import/Export szolgáltatás kell rendelkeznie. Lehet, hogy minden feladat használt tootransfer adatok tooor csak egy tárfiókból. Más szóval egy egyetlen importálási/exportálási feladatok nem terjedhetnek ki több tárfiókok között. Új tárfiók létrehozásával kapcsolatos további információkért lásd: [hogyan tooCreate Tárfiók](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>A BLOB-típusok
Használhat Azure Import/Export szolgáltatás toocopy adatainak túl**blokk** blobok vagy **lap** blobokat. Viszont csak akkor exportálható **blokk** blobokat, **lap** blobok vagy **Append** BLOB az Azure storage-ban Ez a szolgáltatás.

### <a name="job"></a>Feladat
toobegin hello folyamata tooor Blob-tároló exportálása importálását, először létre kell hoznia egy feladatot. Egy feladat lehet, az importálási feladat vagy exportálási feladat:

* Importálási feladat létrehozása, ha azt szeretné, hogy az Azure storage-fiókok vannak a helyszíni tooblobs tootransfer adatokat.
* Exportálási feladat létrehozása. Ha azt szeretné, hogy tootransfer adatok jelenleg tárolt szállítottként blobot, amely a fiók toohard tárolóeszközöket, amelyek tooyou.s egy feladat létrehozásakor, akkor értesíteni hello Import/Export szolgáltatás, hogy Ön lesz kell szállítási egy vagy több merevlemez-meghajtók tooan Azure Az Adatközpont.

* Az importálási feladat meg lesz kell szállítási az adatokat tartalmazó merevlemez-meghajtókat.
* Az exportálási feladat akkor lesz kell szállítási üres merevlemez-meghajtókat.
* Too10 merevlemez-meghajtók száma feladat mentése elküldhet.

Az importálás létrehozhat vagy hello Azure-portálon vagy hello használó feladatok exportálása [Azure Storage Import/Export REST API felülete](/rest/api/storageimportexport).

### <a name="waimportexport-tool"></a>WAImportExport eszköz
létrehozásának első lépése hello egy **importálása** feladat tooprepare importálás közzéteendő meghajtó van. tooprepare a meghajtók csatlakoztassa tooa helyi kiszolgáló és a futási hello WAImportExport eszköz hello helyi kiszolgálón. Az WAImportExport eszköz könnyíti meg a adatmeghajtó toohello másolása, hello meghajtón a BitLocker hello adatok titkosítására és hello meghajtó Adatbázisnapló-fájlok generálása.

hello Adatbázisnapló-fájlok a feladat és a meghajtó meghajtó sorozatszáma és a tárfiók neve például alapvető adatainak tárolására. A naplófájl nem hello meghajtón tárolja. Importálási feladat létrehozásakor használható. Lépésről lépésre feladat létrehozása adatait a cikk későbbi részében valósul meg.

hello WAImportExport eszköze csak 64 bites Windows operációs rendszerrel kompatibilis. Lásd: hello [operációs rendszer](#operating-system) az adott operációsrendszer-verziók támogatottak területén.

Töltse le a legújabb verziójának hello hello [WAImportExport eszköz](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). További tudnivalók a hello WAImportExport eszköz, a következő témakörben: hello [Using hello WAImportExport eszköz](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Előző verzió:** is [WAImportExpot V1 letöltése](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) hello verziója eszközt, és tekintse meg a túl[WAImportExpot V1 használati útmutató](storage-import-export-tool-how-to-v1.md). Hello eszköz WAImportExpot V1-es verziója támogatást nyújt az **lemezek előkészítése, amikor az adatok írása már előre toohello lemez**. Akkor is toouse WAImportExpot V1-es eszköz, ha hello csak kulcs elérhető SAS-kulcs.

>

### <a name="hard-disk-drives"></a>Merevlemez-meghajtók
2,5 hüvelykes csak SSD vagy 2,5" vagy 3.5-ös" SATA II vagy III belső HDD támogatottak hello Import/Export szolgáltatás való használatra. Egy egyetlen importálási/exportálási feladatok lehet egy legfeljebb 10 HDD/SSD-k és minden egyes HDD/SSD tetszőleges méretű lehet. Nagy számú meghajtókat is elosztva több feladat, és nem hozható létre feladatok hello száma korlátozza van. 

Az importálási feladatok csak hello első adatmennyiség hello meghajtón dolgoz fel. hello kötetet NTFS fájlrendszerrel kell formázni.

> [!IMPORTANT]
> Külső merevlemez-meghajtók egy beépített USB-adapterrel járó nem támogatja ezt a szolgáltatást. Egy külső HDD hello kis-és hello lemezt is, nem használható; Ne küldjön külső HDD.
> 
> 

Az alábbiakban a, a külső USB-adapterek listáját használt toocopy adatok toointernal HDD-k. Anker 68UPSATAA - 02BU Anker 68UPSHHDS-BU Startech SATADOCK22UE Orico 6628SUS3-C-fekete (6628 sorozat) Thermaltake BlacX gyakran használt adatok-csere SATA külső merevlemez meghajtó rögzített állomás (USB 2.0-s & eSATA)

### <a name="encryption"></a>Titkosítás
hello meghajtón hello adatokat titkosítani kell a BitLocker meghajtótitkosítás segítségével. Ez védi az adatokat, amíg az átvitel során.

Az importálási feladatok nincsenek két módon tooperform hello titkosítás. hello első módon toospecify hello beállítás használata esetén a dataset CSV-fájl hello WAImportExport eszköz futtatásakor meghajtó előkészítése során. második módon hello tooenable BitLocker-titkosítást manuálisan hello meghajtó, és adja meg a hello titkosítási kulcs hello driveset CSV a meghajtó előkészítése során WAImportExport eszköz parancssori futtatásakor.

Az exportálási feladatok követően az adatok másolt toohello meghajtók hello szolgáltatás titkosítja hello meghajtón a BitLocker használatával hátsó tooyou szállítás előtt. hello titkosítási kulcs tooyou hello Azure-portálon keresztül biztosítja.  

### <a name="operating-system"></a>Operációs rendszer
64 bites operációs rendszerek tooprepare hello merevlemez-meghajtóról hello WAImportExport eszköz használatával hello meghajtó tooAzure, mielőtt a következő hello egyikét használhatja:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 rendszerben. Mindegyik említett operációs rendszerektől támogatja, a BitLocker meghajtótitkosítás.

### <a name="locations"></a>Helyek
hello Azure Import/Export szolgáltatás az összes nyilvános Azure-tárfiók másolása adatok tooand támogatja. Az alábbi helyek hello merevlemez-meghajtók tooone elküldhet. Ha a tárfiók egy nyilvános Azure helyre, amely nincs megadva itt, egy másik szállítási helyre lesz kell megadni, ha létrehozásakor hello feladat használ hello Azure-portálon, illetve hello Import/Export REST API-t.

Szállítási helyek támogatottak:

* USA keleti régiója
* USA nyugati régiója
* USA 2. keleti régiója
* USA nyugati régiója, 2.
* USA középső régiója
* USA északi középső régiója
* USA déli középső régiója
* USA nyugati középső régiója
* Észak-Európa
* Nyugat-Európa
* Kelet-Ázsia
* Délkelet-Ázsia
* Kelet-Ausztrália
* Délkelet-Ausztrália
* Nyugat-Japán
* Kelet-Japán
* Közép-India
* Dél-India
* Nyugat-India
* Közép-Kanada
* Kelet-Kanada
* Dél-Brazília
* Korea középső régiója
* USA-beli államigazgatás – Virginia
* USA-beli államigazgatás – Iowa
* US DoD – Kelet
* US DoD – Középső régió
* Kelet-Kína
* Észak-Kína
* Az Egyesült Királyság déli régiója

### <a name="shipping"></a>Szállítási
**Szállítási toohello adatközpont meghajtók:**

Az importálási vagy exportálási feladat létrehozásakor, adja meg a szállítási címet egy hello támogatott helyek tooship a meghajtók. hello szállítási megadott cím a tárfiók helye hello függ, de nem lehet ugyanaz, mint a tárfiók helyének hello.

Használhatja például a FedEx, DHL, UPS vagy hello Velünk postai tooship üzemeltető szolgáltatók a szállítási cím meghajtók toohello.

**Szállítási meghajtók hello adatközpontból:**

Az importálási vagy exportálási feladat létrehozásakor meg kell adnia egy címet Microsoft toouse szállítási hello meghajtók biztonsági a feladat befejezése után. Ellenőrizze, hogy egy érvényes címet feldolgozási tooavoid késést biztosít.

Az Ön által választott egy vivőjel rendelés tooforward szállítási hello merevlemez is használhatja. hello szolgáltatónként toomaintain lánc felügyeleti lánc fenntartása a megfelelő követési kell rendelkeznie. Meg kell adni egy érvényes FedEx vagy DHL szolgáltatónként fiók számú toobe Microsoft hello meghajtók vissza szállítási használ. Egy FedEx szám meghajtók szállítási újból az hello amerikai és Európai helyek szükség. Egy DHL szám meghajtók szállítási újból az Ázsia és a Ausztrália szükség. Létrehozhat egy [FedEx](http://www.fedex.com/us/oadr/) (az amerikai és európai) vagy [DHL](http://www.dhl.com/) (ázsiai és Ausztrália) szolgáltatónként a fiókot, ha még nem rendelkezik ilyennel. Ha már rendelkezik egy vivőjel-szám, győződjön meg arról, hogy legyen érvényes.

A szállítási a csomagok, hajtsa végre az hello feltételeit, [Microsoft Azure szolgáltatási feltételek](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Vegye figyelembe, hogy hello fizikai adathordozó valóban esetleg toocross nemzetközi határokon. Biztos, hogy a fizikai adathordozó és adatok vannak importálva és/vagy hello alkalmazható törvényekkel összhangban exportált biztosításáért felelős. Mielőtt hello fizikai adathordozó, kérje meg a tanácsadók tooverify, hogy az adathordozó és az adatok jogilag lehet azonosítani szállított toohello adatközpont. Ez segít, hogy időben eléri Microsoft tooensure. Bármelyik csomag nemzetközi határokon túl fogja keresztezi például egy kereskedelmi számla toobe (kivéve ha meghaladó határokon belül Európai Unió) hello csomaggal együtt kell. Hello kereskedelmi számla szolgáltatónként webhelyéről töltött másolatának sikerült kinyomtatása. Példa kereskedelmi számlák [DHL kereskedelmi számla](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) és [FedEx kereskedelmi számla](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Győződjön meg arról, hogy a Microsoft nem lett tüntetve ennek hello exportáló.
> 
> 

## <a name="how-does-hello-azure-importexport-service-work"></a>Hogyan működik a hello Azure Import/Export szolgáltatás?
A helyszíni hely és a feladatok létrehozása és merevlemez-meghajtók tooan Azure-adatközpont szállítási hello Azure Import/Export szolgáltatás használata az Azure blob storage közötti adattovábbításra is. Minden szállított merevlemez-meghajtó nem tartozik egyetlen feladat. Minden feladat tartozik egy tárfiókot. Felülvizsgálati hello [szükséges előfeltételek szakasz](#pre-requisites) gondosan kapcsolatos hello mintaadatokról a blob típusú szolgáltatásokat, például a támogatott, a lemez legyen kijelölve, a helyek és a szállítási toolearn.

Ez a szakasz azt ismerteti, magas szintű hello lépéseit az importálási és exportálni a feladatokat. Később a hello [gyors üzembe helyezés szakasz](#quick-start), azt adja meg a részletes utasításokat toocreate az importálás és exportálás feladat.

### <a name="inside-an-import-job"></a>Az importálási feladat belül
Magas szinten az importálási feladat magában foglalja a hello a következő lépéseket:

* Határozza meg az importált hello adatok toobe, és hello kell meghajtók száma.
* Azonosítsa a blob célhelye hello az adatok a Blob Storage tárolóban.
* Hello WAImportExport eszköz toocopy használja az adatok tooone vagy további merevlemez-meghajtókat, és a Bitlockerrel titkosítani.
* Hozzon létre egy importálási feladat a céloldali tárfiók hello Azure-portál használatával, vagy Import/Export REST API hello. Ha hello Azure-portált használja, hello meghajtó napló fájlok feltöltése.
* Adja meg a hello válaszcím és szolgáltatónként fiók számú toobe hello meghajtók hátsó tooyou szállítási használatos.
* Szállítási hello merevlemez-meghajtók toohello szállítási cím feladat létrehozása során.
* Hello kézbesítési követési hello importálási feladat részletei szám frissítése, valamint hello importálási feladat elküldéséhez.
* Fogadása és feldolgozása hello Azure-adatközpont.
* A vivőjel fiók toohello Válaszcím hello importálási feladat szerepel a meghajtók mellékeltük.
  
    ![Ábra 1:Import feladat folyamata](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>Exportálási feladat belül
Magas szinten exportálási feladat magában foglalja a hello a következő lépéseket:

* Hello adatok toobe exportálni, és hány meghajtót kell hello határozza meg.
* Azonosítsa a hello forrás blobokkal vagy a tároló elérési az adatok a Blob Storage tárolóban.
* Exportálási feladat létrehozása a forrás tárolási fiókjában hello Azure-portál használatával, vagy Import/Export REST API hello.
* Adja meg a forrás blobok hello, vagy a tároló elérési hello az adatok exportálása feladat.
* Hello meghajtók hátsó tooyou szállítási használt toobe hello visszatérési cím, illetve szolgáltatónként számát adja meg.
* Szállítási hello merevlemez-meghajtók toohello szállítási cím feladat létrehozása során.
* Nyomkövetési szám hello exportálási feladat részletei hello kézbesítési frissítése, valamint hello exportálási feladat elküldéséhez.
* hello fogadása és feldolgozása hello Azure-adatközpont.
* hello meghajtók titkosítása a BitLocker; hello kulcsok hello Azure-portálon keresztül érhetők el.  
* hello meghajtók mellékeltük a vivőjel fiók toohello visszatérési címével hello importálási feladat megadott.
  
    ![Ábra 2:Export feladat folyamata](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>A feladat és a meghajtó állapotának megtekintése
Az importálás hello állapotának nyomon követése, vagy a feladatok exportálása hello Azure-portálon. Kattintson a hello **Import/Export** fülre. A feladatokra hello lapon jelenik meg.

![Feladat állapotának megtekintése](./media/storage-import-export-service/jobstate.png)

Feladatállapotok attól függően, hogy a meghajtó hello folyamat az alábbi hello egyike jelenik meg.

| Feladat állapota | Leírás |
|:--- |:--- |
| Létrehozás | A feladat létrehozása után állapotában tooCreating van beállítva. Hello feladat hello létrehozása állapotban van, amíg hello Import/Export szolgáltatás azt feltételezi, hogy hello meghajtók még nincsenek szállított toohello adatközpont. Egy feladat hello létrehozása állapotának mentése tootwo hét, amely után a rendszer automatikusan törli hello szolgáltatás maradhatnak. |
| Szállítási | Miután a csomag küldje el, frissítenie kell a nyomkövetési adatokat az Azure-portálon hello hello.  Ez hello feladat ikonná "Szállítási". hello feladat hello szállítási állapotának mentése tootwo hét marad. 
| Fogadott | Összes fogadott hello data Center, miután hello feladatállapotot lesz beállítva, toohello fogadott. |
| Átvitele | Miután legalább egy meghajtó már megkezdődött a feldolgozás, hello feladatállapotot lesz beállítva toohello átadása. Lásd: hello meghajtó állapotok szakasz alatt részletes információkat. |
| Csomag | Után az összes meghajtón befejezte a feldolgozási, hello feladat kerül hello csomagolás állapotban végéig hello meghajtók szállított hátsó tooyou. |
| Befejeződött | Miután az összes meghajtó lett szállított hátsó toohello-ügyfelekre, ha hello feladat hiba nélkül befejeződött, majd hello feladat lesz beállítva toohello kész állapotban. hello feladat a kész állapot hello 90 nap után automatikusan törölve lesz. |
| Lezárt | Miután az összes meghajtó lett szállított hátsó toohello ügyfél, ha nem történt hiba hello feladat hello feldolgozása során, majd hello feladat lesz beállítva toohello lezárt állapotában. hello feladat automatikusan törli 90 nap után hello lezárt állapotban. |

hello az alábbi táblázat ismerteti a meghajtók egyéni hello életciklusának, akkor átkerül egy importálási vagy exportálási feladat keresztül. minden olyan meghajtó egy feladat aktuális állapotának hello már hello Azure-portálon kívülről.
hello következő táblázatban állapotokat ismerteti, amely minden olyan meghajtó feladatokban továbbítása is.

| Meghajtó állapotát | Leírás |
|:--- |:--- |
| A megadott | Az importálási feladatnak hello feladat létrehozása az Azure-portálon hello hello eredetiállapot meghajtó esetén hello megadott állapot. Az exportálási feladat a meghajtó nem adható meg hello feladat jön létre, mert hello kezdeti meghajtó állapota: hello fogadott állapota. |
| Fogadott | hello meghajtó toohello fogadott állapota közeledik, hello Import/Export szolgáltatás operátort, hogy az importálás vállalati szállítási hello érkezett hello meghajtók feldolgozásakor. Az exportálási feladat hello kezdeti meghajtó állapota hello fogadott állapotát. |
| NeverReceived | hello meghajtó hello csomag feladat megérkeznek, de hello csomag nem tartalmaz hello meghajtó áthelyezi toohello NeverReceived állapotát. A meghajtó is áthelyezheti a állapotba, ha két héten óta hello szolgáltatás hello szállítási adatot kapott, de hello csomag még nem érkezett meg hello adatközpont lett. |
| Átvitele | A meghajtó toohello átadása állapot áthelyezi hello szolgáltatás elindulásakor hello meghajtó tooWindows Azure Storage tootransfer adatait. |
| Befejeződött | A meghajtó toohello kész állapot áthelyezi amikor hello szolgáltatás sikeresen átadta összes hello adatok hibásak.
| CompletedMoreInfo | A meghajtó áthelyezi toohello CompletedMoreInfo állapothoz hello szolgáltatás észlelt kapcsolatos néhány problémát ismertetünk az adatok másolásának származhatnak vagy toohello meghajtót. hello információk között szerepelhet hibák, figyelmeztetések és információs üzenetek blobok felülírására.
| ShippedBack | Amikor teljesítettek a hello data center vissza toohello Válaszcím hello meghajtó áthelyezi toohello ShippedBack állapotát. |

A lemezkép hello Azure portál egy példa feladat hello meghajtó állapotát jeleníti meg:

![Meghajtó állapotának megtekintése](./media/storage-import-export-service/drivestate.png)

hello következő táblázat hello meghajtó hiba állapotok és az egyes állapotokhoz végrehajtott hello műveleteket.

| Meghajtó állapotát | Esemény | Megoldás / a következő lépés |
|:--- |:--- |:--- |
| NeverReceived | A meghajtó, amely NeverReceived (mert nem érkezett hello feladat szállítási részeként) érkezik egy másik szállítási van megjelölve. | hello műveleti csapata áthelyezi hello meghajtó toohello fogadott állapotát. |
| N/A | A meghajtó, amely nem része semmilyen feladatot hello adatközpont egy másik feladat részeként érkezik. | hello meghajtó egy extra meghajtóként lesz megjelölve, és az eredmény toohello ügyfél hello eredeti csomaghoz társított hello feladat elvégzésekor. |

### <a name="time-tooprocess-job"></a>Idő tooprocess feladat
hello mennyisége szükséges idő az importálási/exportálási feladatok szállítási idő, a feladat típusa, például különböző tényezőktől függően változik tooprocess írja be és a adatokat másolással hello, és a megadott hello lemezek mérete hello mérete. hello Import/Export szolgáltatás nem rendelkezik egy SLA-t, de után hello lemezek fogadásának hello szolgáltatás nagy hangsúlyt fektet toocomplete hello másolási too10 7 nap múlva. Hello REST API tootrack hello feladatok előrehaladásának közelebbről is használhatja. Hello lista feladatok művelet, amely arra utal, a másolási folyamat százalékban kifejezett teljes paramétere van. Kapcsolatfelvétel toous, ha egy becslés toocomplete egy idő kritikus importálási/exportálási feladatok van szüksége.

### <a name="pricing"></a>Díjszabás
**Meghajtó díj kezelése**

Minden meghajtó, az importálás részeként feldolgozott meghajtó kezelési díj ellenében történik, vagy exportálja a feladatot. Hello hello részletek [Azure Import/Export szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Szállítási költségek**

Amikor Ön meghajtók tooAzure, költség toohello szállítási szolgáltatónként szállítási hello kell fizetnie. A Microsoft hello meghajtók tooyou adja vissza, ha szállítási költség hello díjfizetéssel toohello szolgáltatónként fiók, amely a megadott feladat létrehozása a hello időpontjában.

**Tranzakciós költségek**

Nincsenek nem tranzakciós költségek adatok importálása a blob Storage tárolóban. hello szabványos kilépő költségek is alkalmazható, ha adatait a blob-tároló exportálja. Tranzakciós költségek a további részletekért lásd: [adatátviteli díjszabás.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Első lépések
Ebben a szakaszban részletes útmutatást ad az importálási és exportálási feladat létrehozása a Microsoft biztosítja. Ellenőrizze, hogy megfeleljen az összes hello [szükséges előfeltételek](#pre-requisites) soron előtt.

> [!IMPORTANT]
> hello szolgáltatást támogatja az importálási / standard szintű tárfiók vagy feladat exportálni, és nem támogatja a prémium szintű storage-fiókok. 
> 
> 
## <a name="create-an-import-job"></a>Importálási feladat létrehozása
Merevlemez-meghajtók adatközpont toohello megadott adatokat tartalmazó egy vagy több meghajtó-szállító által az importálási feladat toocopy adatok tooyour Azure storage-fiók létrehozása hello importálási feladat közvetíti merevlemez-meghajtók adatait, az adatok toobe másolta, a céloldali tárfiók és a szállítási toohello Azure Import/Export szolgáltatás. Az importálási feladat létrehozása egy három lépéses folyamat. Először készítsen a meghajtók hello WAImportExport eszközzel. Második küldje el az importálási feladat hello Azure-portál használatával. Harmadik küldje el a hello meghajtók toohello szállítási cím során feladat létrehozása és a frissítés hello info mobilalkalmazások a feladat részleteit.   

### <a name="prepare-your-drives"></a>Készítse elő a meghajtók
hello első lépéseként esetén hello Azure Import/Export szolgáltatás használatával az adatok importálása a tooprepare a meghajtók hello WAImportExport eszközzel. Lépések hello tooprepare alatt a meghajtók.

1. Hello adatok toobe importált azonosításához. Ennek oka az lehet, könyvtárak és fájlok önálló hello a helyi kiszolgálón vagy egy hálózati megosztásra.  
2. Szüksége lesz, attól függően, hogy hello adatok teljes mérete meghajtók hello számát határozza meg. SATA II vagy III merevlemez-meghajtók szerzik be a szükséges hello számú 2,5 hüvelyk SSD vagy 2,5" vagy 3.5-ös".
3. Hello cél tárfiók, a tároló, a virtuális könyvtárak és a blobok azonosításához.
4.  Határozza meg, hello könyvtárak és/vagy az önálló fájlok, amelyek merevlemez-meghajtó másolt tooeach lesznek.
5.  Hello CSV-fájlok a DataSet adatkészlet és driveset létrehozása.
    
    **A DataSet CSV-fájl**
    
    Az alábbiakban a következő egy minta dataset CSV-fájl például:
    
    ```
    BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
    "F:\50M_original\","containername/",BlockBlob,rename,"None",None 
    ```
   
    A fenti példa hello 100M_1.csv.txt lesz "containername" nevű hello tárolót másolt toohello gyökérmappájában. Ha hello tároló neve "containername" nem létezik, jön létre. Minden fájl és mappa alatti 50M_original rekurzív módon másolja toocontainername lesz. Gyökérmappa-szerkezetében maradnak.

    További információ [hello dataset CSV-fájl előkészítése](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    
    **Ne feledje**: alapértelmezés szerint hello adatokat importálja blokkblobként. Akár egy Lapblobokat hello BlobType mezőérték tooimport adatokat is használhatja. Például VHD-fájlokat, amely lemezként egy Azure virtuális gépen csatlakoztatási importálása, importálnia kell őket, Lapblobokat.

    **Driveset CSV-fájl**

    hello értékének hello driveset jelző ahhoz, hogy hello eszköz toocorrectly leképezve lemezek toowhich hello meghajtóbetűjelek hello listáját tartalmazó CSV-fájl válassza ki az előkészített lemezek toobe hello listája. 

    Az alábbiakban az driveset CSV-fájl hello példája:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    A fenti példa hello feltételezzük, hogy két lemezek vannak csatolva hozzá, és alapvető NTFS típusú kötetek G:\ kötet levelek és H:\ létrejött. hello eszköz formázza és hello lemez, amely H:\ futtatja, és nem formázása vagy G:\ kötet üzemeltető hello lemez titkosítása titkosításához.

    További információ [hello driveset CSV-fájl előkészítése](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Használjon hello [WAImportExport eszköz](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) toocopy az adatok tooone vagy további merevlemez-meghajtókat.
7.  A titkosítási mezője drivset CSV tooenable BitLocker-titkosítást a merevlemez-meghajtó hello "Titkosítás" is megadhat. Azt is megteheti sikerült is engedélyezheti a BitLocker titkosítást manuálisan hello merevlemez-meghajtó, és adja meg a "AlreadyEncrypted", majd adja meg a fürt megosztott kötetei szolgáltatás hello driveset hello kulcsot hello eszköz futtatása során.

8. Ne módosítsa a hello található adatok hello merevlemez-meghajtók vagy hello naplófájl lemezét befejezése után.

> [!IMPORTANT]
> Minden egyes merevlemez-meghajtó előkészíti a naplófájl eredményez. Ha hello importálási feladat hello Azure-portál használatával hoz létre, hello meghajtó adott importálási feladat részét képező összes hello napló fájlok kell feltöltenie. -Meghajtók nélkül napló fájlokat nem fogja feldolgozni.
> 
>

Az alábbiakban hello parancsok és példák hello merevlemez-meghajtó előkészítése WAImportExport eszköz használata.

WAImportExport eszköz hello PrepImport parancsot kell másolnia munkamenet toocopy könyvtárak és/vagy a fájlokat egy új munkamenet-példány:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**1. példa importálása**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Ahhoz, túl**további meghajtók hozzáadása**, egy hozzon létre egy új driveset fájlt, és futtassa az alábbi hello parancsot. További másolási munkamenetek toohello különböző lemezmeghajtókhoz megadottnál InitialDriveset csv-fájlban adjon meg egy új driveset CSV-fájlt, és adja meg paraméterként érték toohello "AdditionalDriveSet". Használjon hello **azonos naplófájl** nevet, és adjon meg egy **új munkamenet-azonosító**. hello AdditionalDriveset CSV-fájl formátuma ugyanaz, mint a InitialDriveSet formátumban.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Importálja a 2. példa**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

A sorrend tooadd további adatok toohello azonos driveset WAImportExport eszköz PrepImport parancs hívható későbbi másolási munkamenetek toocopy további fájlokat vagy könyvtár: az ezt követő másolási munkamenetek toohello azonos merevlemez-meghajtók megadott InitialDriveset .csv fájlt, adja meg a hello **azonos naplófájl** nevet, és adjon meg egy **új munkamenet-azonosító**; nincs szükség tooprovide hello tárfiók kulcsa van.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Importálja a 3. példa**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

A hello WAImportExport eszközzel kapcsolatos további részletek megtekintéséhez [merevlemezek előkészítése az importálási](storage-import-export-tool-preparing-hard-drives-import.md).

Emellett tekintse meg a toohello [mintát, hogy az importálás munkafolyamat tooprepare merevlemezek](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) részletesebb lépésenkénti leírását.  

### <a name="create-hello-import-job"></a>Hello importálási feladat létrehozása
1. Miután előkészítette a meghajtót, keresse meg a storage-fiókot tooyour hello Azure-portálon, és hello irányítópult megtekintése. A **gyors áttekintése**, kattintson a **importálási feladat létrehozása**. Tekintse át a hello lépéseket, és válassza ki a hello jelölőnégyzet tooindicate, hogy előkészítette a meghajtót, és hogy van-e hello meghajtó napló fájl elérhető.
2. 1. lépésben adja meg az importálási feladat és egy érvényes címet felelős hello személy kapcsolattartási adatait. Ha a részletes naplóadatok toosave hello importálási feladat, a beállítást hello túl**mentés hello részletes napló a "waimportexport" blob a tárolóban**.
3. 2. lépésben tölt fel hello meghajtó napló hello meghajtó előkészítése lépésben beszerzett. Szüksége lesz egy fájl tooupload minden előkészítette a meghajtót.
   
   ![Hozzon létre importálási feladat - 3. lépés](./media/storage-import-export-service/import-job-03.png)
4. 3. lépésben adjon meg egy leíró nevet a hello importálási feladat. Vegye figyelembe, hogy hello név is tartalmazhat, csak kisbetűket, számokat, kötőjeleket és aláhúzásjeleket tartalmazhat, betűvel kell kezdődnie, és nem tartalmazhat szóközt. Dönthet tootrack a feladatok, amíg azok folyamatban van, és azok befejezése után hello nevet fogja használni.
   
   Ezután válassza ki az adatterületen center hello listából. hello center adatterület hello data center, és oldja meg kell küldje el a csomag toowhich jelzi. Lásd: hello gyakran ismételt kérdések alább olvashat.
5. 4. lépés válassza ki a visszatérési szolgáltatónként hello listából, és adja meg a szolgáltatója. A Microsoft a fiók tooship hello meghajtók hátsó tooyou használja az importálási feladat befejezése után.
   
   Ha a nyomkövetési sok, válassza ki a kézbesítési szolgáltatónként hello listából, és adja meg a követési.
   
   Ha Ön nem rendelkezik number még, válassza ki a követési **I információt nyújt a saját szállítási az importálási feladat után szükségem van kiadva a csomag**, majd fejezze be a hello az importálási folyamat.
6. tooenter a nyilvántartási szám után a csomag teljesítették vissza toohello **Import/Export** hello Azure-portálon a tárfiók lapon válassza ki a feladatot hello listáról, és válassza a **szállítási információ**. Hello varázsló navigálhat, és adja meg a követési a 2.
   
    Ha követési szám hello hello feladat létrehozása két héten belül nem frissül, hello feladat lejár.
   
    Ha hello feladat hello létrehozása, szállítási vagy átadása állapotban van, a vivőjel fiók száma 2. lépésben hello varázsló is frissítheti. Miután hello feladat hello csomagolás állapotban van, a vivőjel számát, hogy a feladat nem frissíthető.
7. Nyomon követheti a feladat előrehaladását a portál irányítópultján hello. Tekintse meg, mi hello előző szakaszban minden feladat állapota: által [a feladat állapotának megtekintése](#viewing-your-job-status).

## <a name="create-an-export-job"></a>Exportálási feladat létrehozása
Hozzon létre egy exportálási feladat toonotify hello Import/Export szolgáltatásról, hogy meg kell lennie szállítási egy vagy több üres meghajtók toohello adatközpont, hogy az adatok a fiók toohello tárolóeszközöket exportálható, és a hello meghajtók majd szállított tooyou.

### <a name="prepare-your-drives"></a>Készítse elő a meghajtók
A meghajtók előkészítése exportálási feladat következő előzetes ellenőrzése ajánlott:

1. Ellenőrizze a hello hello WAImportExport eszköz PreviewExport paranccsal szükséges lemezek számát. További információkért lásd: [meghajtó használata Előnézet egy exportálása feladat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Ez segít megtekintése a kiválasztott hello blobok meghajtóinak használatát, toouse fog hello meghajtók hello mérete alapján.
2. Ellenőrizze, hogy olvasási/írási toohello merevlemez-meghajtóról hello exportálási feladat közzéteendő is.

### <a name="create-hello-export-job"></a>Hello exportálási feladat létrehozása
1. toocreate exportálási feladat, keresse meg a storage-fiókot tooyour hello Azure-portálon, és hello irányítópult megtekintéséhez. A **gyors áttekintése**, kattintson a **exportálása feladat létrehozása** , és folytassa hello varázsló.
2. 2. lépésben adja meg az exportálási feladat hello személy kapcsolattartási adatait. Ha a részletes naplóadatok toosave hello exportálási feladat, a beállítást hello túl**mentés hello részletes napló a "waimportexport" blob a tárolóban**.
3. 3. lépésben adja meg, mely adatokat tooexport a tárolási fiók tooyour üres a meghajtó, vagy a meghajtók blob. Dönthet tooexport összes Blobadatok hello tárfiókja, vagy is megadhat, amely blobok vagy beállítja a blobok tooexport.
   
   a blob tooexport toospecify hello használata **Equal To** választó, és adja meg a hello relatív elérési út toohello blob, hello Tárolónév kezdve. Használjon *$root* toospecify hello gyökérszintű tárolóban.
   
   minden toospecify blobok előtag, használjon hello kezdve **kezdődik** választó, és adja meg hello előtagot, perjellel kezdődik "/". hello előtag lehet hello előtag hello tároló neve, teljes Tárolónév hello vagy hello teljes tároló nevének előtagja hello hello blob neve követ.
   
   hello a következő táblázat példákat mutat a érvényes blob elérési utak:
   
   | A választó | BLOB elérési út | Leírás |
   | --- | --- | --- |
   | Kezdődik |/ |Hello tárfiókban lévő összes BLOB exportálása |
   | Kezdődik |/$root / |Exportálja az összes BLOB hello a gyökérszintű tárolóban |
   | Kezdődik |/Book |Exportálja az összes BLOB a tárolóban előtaggal kezdődő **könyv** |
   | Kezdődik |/Music/ |Exportálja a tárolóban lévő blobok összes **zene** |
   | Kezdődik |/ zene/szerelem |Exportálja a tárolóban lévő összes blobok **zene** előtaggal rendelkező karaktersorral **kedvelt** |
   | Túl egyenlő|$root/logo.bmp |Exportálja a blob **logo.bmp** hello a gyökérszintű tárolóban |
   | Túl egyenlő|Videos/Story.mp4 |Exportálja a blob **story.mp4** tárolóban **videók** |
   
   Meg kell adnia hello blob elérési utak a érvényes formátumok tooavoid hibákat a feldolgozás során a képernyőfelvételen látható módon.
   
   ![Hozzon létre exportálási feladat - 3. lépés](./media/storage-import-export-service/export-job-03.png)
4. 4. lépés adjon meg egy leíró nevet a hello exportálási feladat. hello név is tartalmazhat, csak kisbetűket, számokat, kötőjeleket és aláhúzásjeleket tartalmazhat, betűvel kell kezdődnie, és nem tartalmazhat szóközt.
   
   hello center adatterület hello data center toowhich kell küldje el a csomag jelzi. Lásd: hello gyakran ismételt kérdések alább olvashat.
5. 5. lépésben válassza ki a visszatérési szolgáltatónként hello listából, és adja meg a szolgáltatója. A Microsoft a fiók tooship használja a meghajtók tooyou biztonsági másolatot, az exportálási feladat befejezése után.
   
   Ha a nyomkövetési sok, válassza ki a kézbesítési szolgáltatónként hello listából, és adja meg a követési.
   
   Ha Ön nem rendelkezik number még, válassza ki a követési **I információt nyújt a saját szállítási az exportálási feladat után szükségem van kiadva a csomag**, majd fejezze be a hello exportálás során.
6. tooenter a nyilvántartási szám után a csomag teljesítették vissza toohello **Import/Export** hello Azure-portálon a tárfiók lapon válassza ki a feladatot hello listáról, és válassza a **szállítási információ**. Hello varázsló navigálhat, és adja meg a követési a 2.
   
    Ha követési szám hello hello feladat létrehozása két héten belül nem frissül, hello feladat lejár.
   
    Ha hello feladat hello létrehozása, szállítási vagy átadása állapotban van, a vivőjel fiók száma 2. lépésben hello varázsló is frissítheti. Miután hello feladat hello csomagolás állapotban van, a vivőjel számát, hogy a feladat nem frissíthető.
   
   > [!NOTE]
   > Hello blob toobe exportált hello időpontban másolásának toohard meghajtó használatban van, ha Azure Import/Export szolgáltatás hello blob, és másolja a hello pillanatkép pillanatképe vesz igénybe.
   > 
   > 
7. Nyomon követheti a feladat előrehaladását a hello irányítópulton a hello Azure-portálon. Mi minden feladat állapota azt jelenti, hogy az előző szakaszban hello lásd a "a feladat állapotának megtekintése".
8. Miután megkapta hello meghajtók az exportált adatok, megtekintheti, és másolja a meghajtó hello szolgáltatás által létrehozott hello BitLocker-kulcsok. Keresse meg az Azure-portálon hello tooyour storage-fiókot, majd hello Import/Export fülre. Válassza ki az exportálási feladat hello listából, és hello kulcsok megtekintése gombra. hello BitLocker-kulcsok az alábbi módon jelenik meg:
   
   ![BitLocker-kulcsok exportálási feladat megtekintése](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Nyissa meg keresztül hello – gyakori kérdések című szakaszt, hello az ügyfelek a szolgáltatás használata során felmerülő leggyakoribb kérdések magában foglalja.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Másolhatja a hello Azure Import/Export szolgáltatás használata Azure File storage?**

Nem, hello Azure Import/Export szolgáltatás blokk blobokat és Lapblobokat csak támogatja. Minden más Tárolótípusok, beleértve az Azure File storage, Table Storage és a Queue Storage használata nem támogatott.

**Hello Azure Import/Export szolgáltatás érhető el a CSP-előfizetések?**

Az Azure Import/Export szolgáltatás támogatja a kriptográfiai Szolgáltató előfizetések.

**I kihagyhatja hello meghajtó előkészítése az importálási feladat vagy szeretnék előkészítheti egy meghajtó másolása nélkül?**

Minden meghajtó, amelyet az tooship adatimportálási hello Azure WAImportExport eszközzel kell készíteni. Hello WAImportExport eszköz toocopy toohello adatmeghajtó kell használnia.

**Szükséges tooperform bármilyen lemez előkészítése exportálási feladat létrehozásakor?**

Nem, de néhány előzetes ellenőrzése használata ajánlott. Ellenőrizze a hello hello WAImportExport eszköz PreviewExport paranccsal szükséges lemezek számát. További információkért lásd: [meghajtó használata Előnézet egy exportálása feladat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Ez segít megtekintése a kiválasztott hello blobok meghajtóinak használatát, toouse fog hello meghajtók hello mérete alapján. Ellenőrizze, hogy Ön olvashatók be és írási toohello merevlemez-meghajtóról a hello közzéteendő exportálja feladat.

**Mi történik, ha egy HDD toohello meg nem felelő véletlenül küldése támogatott követelmények?**

hello Azure adatközpontba adja vissza, amely nem felel meg a támogatott toohello követelmények tooyou hello meghajtó. Ha csak néhány hello meghajtót hello csomag hello támogatási követelmények teljesítéséhez, ezek a meghajtók dolgoz fel, és hello meghajtók hello követelményeknek meg nem felelő visszahelyezi tooyou.

**Lehet megszakítani a feladatot?**

Megszakíthatja a feladat állapotának létrehozásakor, illetve szállítási.

**Mennyi ideig tekinthetők meg hello befejezett feladatok állapota a hello Azure-portálon?**

Megtekintheti a befejezett feladatokhoz tartozó mentése too90 nap hello állapotát. Befejezett feladatokhoz 90 nap után törlődnek.

**Mi a teendő, ha szeretné, hogy tooimport vagy 10-nél több meghajtó exportálni?**

Egy importálási vagy exportálási feladat hello Import/Export szolgáltatás egyetlen feladatban csak 10 meghajtók hivatkozhat. Ha azt szeretné, tooship 10-nél több meghajtó, több feladat is létrehozhat. Ugyanazt a feladatot együtt kell szállítani a hello társított meghajtók hello ugyanaz a csomag.
Microsoft nyújt útmutatást, és segítséget, ha az adatok kapacitás több lemezre kiterjedő importálási feladatok. Lépjen kapcsolatba a bulkimport@microsoft.com további információ

**Nem hello szolgáltatás formátum hello tenni, mielőtt azok?**

Nem. Az összes meghajtó bitlockerrel titkosított.

**Lehet az importálási/exportálási feladatok meghajtókat is vásárolni a Microsoft?**

Nem. Szüksége lesz tooship saját meghajtókat is importálni és exportálni a feladatokat.

** Hogyan hozzáférhet a szolgáltatás ** importált adatok

hello adatokat az Azure storage-fiókjában hello Azure portálon keresztül is elérhetők, vagy egy önálló eszközzel hívják meg a Tártallózó alkalmazással. https://docs.microsoft.com/en-us/Azure/VS-Azure-Tools-Storage-Manage-with-Storage-Explorer 

**Hello importálási feladat befejezése után mi lesz a adatok néz hello tárfiókban? A könyvtár-hierarchia megőrzi?**

Ha a merevlemez-meghajtó előkészítése az importálási feladat, hello cél hello DstBlobPathOrPrefix mező adatkészlet CSV határozza meg. Ez a hello céltárolója a hello tárolási fiók toowhich hello merevlemez-meghajtóról az adatok másolásakor. A cél tárolóban virtuális könyvtárak mappák hello merevlemez-meghajtóról jön létre, és blobok fájlok jönnek létre. 

**Ha hello meghajtó már megtalálható a tárfiókban lévő fájlok, hello szolgáltatás felülírja a létező blobot, amely a tárfiókot?**

Amikor hello meghajtó előkészítése, megadhatja e hello cél fájlok felülírható, és figyelmen kívül hagyja a hello mező használatát szabályozó nevű adatkészlet CSV-fájl: < átnevezése |} nem írható felül |} felülírása >. Alapértelmezés szerint hello szolgáltatást fog hello új fájlok átnevezése helyett felülírja a meglévő blobokat.

**Egy 32 bites operációs rendszerekkel kompatibilis hello WAImportExport eszköz?**
Nem. hello WAImportExport eszköze csak 64 bites Windows operációs rendszerekkel kompatibilis. Tekintse meg az operációs rendszerek szakasz toohello hello [szükséges előfeltételek](#pre-requisites) támogatott operációsrendszer-verziók teljes listáját.

**I bele kell foglalni hello merevlemez-meghajtó csakis a csomagot?**

Csak a merevlemez-meghajtók adjon szolgáltatástól. Ne adjon meg például a kiemelt tápkábelek vagy USB-kábelt.

**Van tooship a meghajtókat FedEx vagy DHL?**

Elküldhet a meghajtók toohello adatközpont bármely ismert szolgáltatónként FedEx, DHL, UPS vagy Velünk postai használatával. Azonban szállítási hello meghajtók hátsó tooyou hello adatközpontból, meg kell adnia egy FedEx fiók az Amerikai Egyesült Államok és EU hello, vagy DHL fiók szám hello Ázsia és a Ausztrália régióban.

**Vannak-e a nemzetközi a meghajtó szállítási korlátozások?**

Vegye figyelembe, hogy hello fizikai adathordozó valóban esetleg toocross nemzetközi határokon. Biztos, hogy a fizikai adathordozó és adatok vannak importálva és/vagy hello alkalmazható törvényekkel összhangban exportált biztosításáért felelős. Mielőtt hello fizikai adathordozó, kérje meg a tanácsadók tooverify, hogy az adathordozó és az adatok jogilag lehet azonosítani szállított toohello adatközpont. Ez segít, hogy időben eléri Microsoft tooensure.

**Egy feladat létrehozásakor a szállítási cím hello egy olyan hely, eltér a tárfiókhely. Mit tegyek?**

Néhány fiók tárolóhelyek csatlakoztatott tooalternate szállítási helyek. Rendelkezésre álló szállítási helyek korábban is lehet ideiglenesen csatlakoztatott tooalternate helyét. Mindig ellenőrzik a szállítási cím a megadott feladat létrehozásakor, a meghajtók, mielőtt hello.

**Ha a meghajtó szállítási, hello vivőjel hello data center címét és telefonszámát a telefonszám kér. Mit kell nyújtania?**

telefonszám és a tartományvezérlő hello cím valósul meg a projekt létrehozásának részeként.

**Használható hello Azure Import/Export szolgáltatás toocopy csendes-óceáni TÉLI postaládákhoz és SharePoint-adatok tooO365?**

Tekintse meg a túl[importálási csendes-óceáni TÉLI fájlokat vagy a SharePoint-adatok tooOffice 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Használhatok hello Azure Import/Export szolgáltatás toocopy a biztonsági mentések offline toohello Azure Backup szolgáltatásra?**

Tekintse meg a túl[Offline biztonsági másolat munkafolyamat Azure backup](../../backup/backup-azure-backup-import-export.md).

**Mi az a hello maximális számát a HDD egy részletben?**

Tetszőleges számú HDD lehet egy szállítási, és ha hello lemezek toomultiple feladatok tartoznak ajánlott túl egy) nevű hello megfelelő feladat feliratú hello lemezekkel rendelkeznek. b) frissítés hello feladatok követési számnak -1, a utótaggal rendelkező-2 stb.
  
**Mi az maximális Blokkblob hello és lemez Import/Export támogatott Blob oldalméret?**

Maximális Blokkblob mérete körülbelül 4.768TB vagy 5,000,000 MB.
Az oldalakra vonatkozó Blob maximális mérete 1TB.

**Támogatja a lemez Import/Export AES 256 titkosítás?**

Az Azure Import/Export szolgáltatás alapértelmezés szerint titkosítja a bitlocker titkosítás AES-128, de ez megnövekedett tooAES 256 lehet manuálisan titkosítása a bitlocker előtt az adatok másolásakor. 

Ha használ [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), az alábbiakban van egy minta parancs
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Ha használ [WAImportExport eszköz](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) "AlreadyEncrypted" adja meg, és adja meg a fürt megosztott kötetei szolgáltatás hello driveset hello kulcsot.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Következő lépések

* [Hello WAImportExport eszköz beállítása](storage-import-export-tool-how-to.md)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)
* [Az Azure importálási exportálása REST API minta](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

