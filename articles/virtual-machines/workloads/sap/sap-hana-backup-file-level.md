---
title: "Azure biztonsági mentés HANA fájlszintű aaaSAP |} Microsoft Docs"
description: "Két fő biztonsági mentési lehetőség SAP Hana az Azure virtuális gépeken, ez a cikk foglalkozik SAP HANA Azure biztonsági mentési fájl szinten"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: d5a55de5634ac7724e7fd0fa3760c6c408c3db74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-azure-backup-on-file-level"></a>A fájl szintjén SAP HANA Azure biztonsági mentés

## <a name="introduction"></a>Bevezetés

Ez egy SAP HANA biztonsági másolaton kapcsolódó cikkek háromrészes sorozatát része. [Biztonsági mentési útmutató SAP Hana Azure virtuális gépeken](./sap-hana-backup-guide.md) áttekintése és információkat nyújt a kezdeti lépések, és [SAP HANA biztonsági másolat alapján storage-pillanatfelvételekkel](./sap-hana-backup-storage-snapshots.md) magában foglalja az hello tárolási pillanatkép-alapú biztonsági mentési beállítás.

Hello Azure Virtuálisgép-méretek megnézi, egy látható, hogy egy GS5 lehetővé teszi a mellékelt adatok 64 lemez. A nagyméretű SAP HANA-rendszerek esetében lemezek jelentős számú már adatainak és naplókönyvtárainak fájlokat, valószínűleg szoftveres RAID optimális lemez I/O kapacitása együtt kerülhet sor. majd hello kérdés, ahol toostore SAP HANA biztonsági mentési fájlokat, amelyek sikerült megtelnek hello csatolt adatok lemezek időbeli? Lásd: [Azure Linux virtuális gépek méretei](../../linux/sizes.md) hello Azure virtuális gép mérete táblákhoz.

Nincs nincs biztonsági mentési SAP HANA-integráció az Azure Backup szolgáltatás jelenleg. hello normál módon toomanage hello fájlszintű biztonsági mentés/visszaállítás egy fájl alapú biztonsági mentéssel keresztül SAP HANA Studio vagy SAP HANA SQL-utasítások segítségével. Lásd: [SAP HANA-SQL és a rendszer nézetek hivatkozás](https://help.sap.com/hana/SAP_HANA_SQL_and_System_Views_Reference_en.pdf) további információt.

![Az ábrán látható, az SAP HANA Studio hello biztonsági mentési menüpont hello párbeszédpanel](media/sap-hana-backup-file-level/image022.png)

Az ábrán látható, hello párbeszédpanel hello biztonsági mentési menüpont SAP HANA Studio. Ha hálózatiadapter-típus &quot;fájl,&quot; egy van toospecify egy útvonal hello fájlrendszer, amelybe a SAP HANA hello biztonságimásolat-fájlok írja. Visszaállítás hello működik ugyanúgy.

Ez a választás hangvételére egyszerű és egyszerű, amíg nincsenek szempontokat. Ahogy korábban említettük, az Azure virtuális gép csatolható adatlemezek száma korlátozás van érvényben. Nem lehet kapacitás toostore SAP HANA biztonságimásolat-fájlok hello fájlrendszerek hello VM, hello lemez- és adatbázis-átviteli követelmények, amelyek során előfordulhat, hogy a szoftver hello méretétől függően a RAID használatával több adat között szétosztott lemezek. Különböző lehetőségek áthelyezésére e biztonságimásolat-fájlokat, és kezelését fájl mérete korlátozások és teljesítmény terabájtos adatkészleteket, kezelésekor a cikk későbbi részében találhatók.

Egy másik lehetőség, mely rugalmasabban vonatkozó teljes kapacitás biztosítja, az Azure blob Storage tárolóban. Míg egy blob is korlátozott too1 TB, hello teljes kapacitás egyetlen blob-tároló jelenleg 500 TB. Emellett biztosít az ügyfelek hello choice tooselect úgynevezett &quot;lassú&quot; tárolóról, ami egy költség előnye blob. Lásd: [Azure Blob Storage: közbeni és a tárolási rétegek cool](../../../storage/blobs/storage-blob-storage-tiers.md) ritkán használt adatok a blob storage vonatkozó további információért.

További biztonsági használja a georeplikált tárolási fiók toostore hello SAP HANA biztonsági mentéseket. Lásd: [Azure Storage replikációs](../../../storage/common/storage-redundancy.md) tárfiók replikációjának vonatkozó további információért.

Egy SAP HANA-biztonsági mentések dedikált virtuális merevlemezek sikerült helyez egy dedikált biztonsági másolatok tárolási fiók georeplikált. Ellenkező esetben az egyik hello SAP HANA biztonsági mentések megtartása tooa georeplikált tárfiók hello VHD-k, vagy egy másik régióban tooa tárfiók másolja.

## <a name="azure-backup-agent"></a>Az Azure Backup szolgáltatás ügynöke

Az Azure biztonsági mentési ajánlatok hello beállítás toonot csak biztonsági teljes virtuális gépeket, de is fájlok és könyvtárak keresztül hello biztonságimásolat-készítő ügynök, amely toobe hello vendég operációs rendszer telepítve van. 2016. December, ez az ügynök csak akkor támogatott a Windows, de (lásd: [biztonsági mentése a Windows Server vagy az ügyfél tooAzure hello erőforrás-kezelő telepítési modellel](../../../backup/backup-configure-vault.md)).

A probléma megoldásához toofirst másolatok SAP HANA biztonságimásolat-fájlok tooa Windows Azure virtuális gép (például keresztül SAMBA megosztás) és a hello Azure Backup szolgáltatás ügynöke onnan. Bár ez technikailag lehetséges, azt kellene összetettebbé és lelassul hello biztonsági mentési vagy visszaállítási folyamat túl kicsit miatt toohello másolási hello Linux és a Windows virtuális gép hello között. Nem ajánlott toofollow ezt a módszert használja.

## <a name="azure-blobxfer-utility-details"></a>Az Azure blobxfer segédprogram részletei

toostore könyvtárak és fájlok az Azure storage, egy használhatja a parancssori felületen vagy a PowerShell-lel, vagy fejlesszen ki egy eszközt hello egyikével [Azure SDK-k](https://azure.microsoft.com/downloads/). Szerepel továbbá egy használatra kész segédprogram AzCopy, tooAzure adattárolás másolására, de Windows csak (lásd: [adatátvitel az AzCopy parancssori segédprogram használatát ismertető hello](../../../storage/common/storage-use-azcopy.md)).

Ezért blobxfer használt biztonsági mentési SAP HANA-fájlok másolása. Nyílt forráskódú, éles környezetben számos ügyfél használja, és elérhető legyen [GitHub](https://github.com/Azure/blobxfer). Ez az eszköz lehetővé teszi egy toocopy közvetlen tooeither Azure blob-tároló vagy az Azure-fájlmegosztáshoz. Számos hasznos szolgáltatáshoz, például az md5 kivonatoló vagy automatikus párhuzamossági, ha egy könyvtárat a több fájl is biztosít.

## <a name="sap-hana-backup-performance"></a>Biztonsági mentési SAP HANA-teljesítmény

![Ezen a képernyőfelvételen látható hello SAP HANA biztonsági mentési konzol SAP HANA Studio verziója](media/sap-hana-backup-file-level/image023.png)

Ezen a képernyőfelvételen látható hello SAP HANA biztonsági mentési konzol SAP HANA Studio van. Hamarosan 42 perc toodo hello biztonsági mentés végrehajtásának hello az egyetlen Azure standard tárolási lemezen 230 GB csatolt toohello HANA VM XFS fájlrendszert használ.

![Ezen a képernyőfelvételen látható verziója YaST hello SAP HANA-teszt virtuális gép](media/sap-hana-backup-file-level/image024.png)

Ezen a képernyőfelvételen látható a YaST hello SAP HANA-teszt virtuális gép van. Ahogy korábban említettük, egy hello 1 TB méretű lemez egy SAP HANA biztonsági mentés láthatja. Hamarosan 42 perc toobackup tartott 230 GB. Ezenkívül öt 200 GB-os lemezeken lenne csatlakoztatva, és a szoftver RAID md0 létre, csíkozást fölött öt az Azure data köteteit.

![Ismétlődő ugyanazt biztonsági mentése során a szoftveres RAID csíkozást az öt különböző hello csatolt Azure standard szintű tárolást adatlemezek](media/sap-hana-backup-file-level/image025.png)

Ismétlődő hello ugyanazt biztonsági mentése során a szoftveres RAID csíkozást az öt különböző csatolt Azure standard szintű tárolást adatainak indított lemezek hello biztonságimásolat-készítési időpont 42 perces too10 perc le. hello lemezek toohello VM gyorsítótárazása nélkül lenne csatlakoztatva. Ezért ezt a nyilvánvaló mennyire fontos lemezírás teljesítménye hello biztonságimásolat-készítési időpont szolgál. Egy ekkor az kapcsoló tooAzure prémium szintű storage toofurther hello folyamat az optimális teljesítmény érdekében. Prémium szintű Azure storage általában éles rendszerek esetén használandó.

## <a name="copy-sap-hana-backup-files-tooazure-blob-storage"></a>Másolja az SAP HANA biztonságimásolat-fájlok tooAzure blob-tároló

Módon a 2016. December hello legjobban a beállítás tooquickly SAP HANA biztonsági mentési fájlok tárolása az Azure blob Storage tárolóban. Több egyetlen blob-tároló rendelkezik egy legfeljebb 500 TB, a legtöbb SAP HANA-rendszerek esetén GS5 virtuális gépen futó Azure-ban tookeep elegendő SAP HANA biztonsági mentések elegendő. Az ügyfelek közötti választás hello rendelkezik &quot;forró&quot; és &quot;cold&quot; blob-tároló (lásd: [Azure Blob Storage: közbeni és a tárolási rétegek cool](../../../storage/blobs/storage-blob-storage-tiers.md)).

Hello blobxfer eszközzel is könnyen toocopy hello SAP HANA biztonságimásolat-fájlok közvetlen tooAzure blob-tároló.

![Itt látható egy SAP HANA fájl teljes biztonsági mentés fájljainak hello](media/sap-hana-backup-file-level/image026.png)

Itt láthatja egy SAP HANA fájl teljes biztonsági mentés fájljainak hello. Négy fájlok vannak, és hello legnagyobb nincs nagyjából 230 GB.

![Toocopy hello 230 GB tooan Azure standard fiók tárolóra tartott nagyjából 3000 másodpercben](media/sap-hana-backup-file-level/image027.png)

Nem használja az md5 kivonatoló hello kezdeti teszt, végrehajtásának nagyjából 3000 másodperc toocopy hello 230 GB tooan Azure standard fiók tárolóra.

![Ezen a képernyőfelvételen látható egyik látható megjelenését a hello Azure-portálon](media/sap-hana-backup-file-level/image028.png)

Ezen a képernyőfelvételen látható egy láthatja, hogyan fog kinézni a hello Azure-portálon. A következő nevű blobtárolóban &quot;sap-hana-biztonsági mentések&quot; hozta létre, és tartalmazza a hello négy blobok, amelyek hello SAP HANA biztonságimásolat-fájlok tartalmazzák. Az egyik legyen egy korlátja nagyjából 230 GB.

hello HANA Studio biztonsági mentési konzol lehetővé teszi, hogy egy toorestrict hello maximális HANA biztonsági mentési fájlok méretét. Hello minta környezetben teljesítményét azáltal, hogy több kisebb biztonságimásolat-fájlokat, egy nagy 230 GB-os fájl helyett lehetséges toohave javult.

![Hello biztonságimásolat-fájl maximális mérete beállítása hello HANA ügyféloldali nem &#39; t hello biztonságimásolat-készítési időpont javítása](media/sap-hana-backup-file-level/image029.png)

Hello biztonságimásolat-fájl maximális mérete beállítása hello HANA ügyféloldali nem &#39; t javítása hello biztonságimásolat-készítési időpont, mert egymás után, az ábrán látható módon írja hello fájlt. hello méretkorlátot too60 GB lett beállítva, így hello biztonsági másolat négy nagy fájlok hello 230 GB-os helyett egy fájlból.

![tootest párhuzamosságát hello blobxfer eszköz, a maximális fájlméret hello HANA biztonsági mentések majd állították a too15 GB](media/sap-hana-backup-file-level/image030.png)

tootest párhuzamosságát hello blobxfer eszköz, a maximális fájlméret hello HANA biztonsági mentések majd too15 GB, ami 19 biztonságimásolat-fájlok lett beállítva. Ez a konfiguráció hello idő blobxfer toocopy hello 230 GB tooAzure blob Storage állapotba le too875 másodperc 3000 másodperc.

Ez eredménye toohello legfeljebb 60 MB/s írásra, az Azure blob miatt. Több blobok keresztül párhuzamossági megoldja hello szűk keresztmetszet, de a hátránya: hello blobxfer eszköz toocopy teljesítményének növelése ezek HANA biztonságimásolat-fájlok tooAzure blob-tároló helyezi terhelés hello HANA VM és hello hálózati is. HANA rendszer működésének hatással lesz.

## <a name="blob-copy-of-dedicated-azure-data-disks-in-backup-software-raid"></a>Biztonsági mentési szoftver RAID dedikált Azure adatlemezek BLOB másolata

Hello manuális VM adatok lemezes biztonsági mentés, ellentétben a ezt a módszert használja egy nem készítsen biztonsági másolatot a virtuális gép toosave hello egész SAP-telepítésen, HANA adatokat, beleértve az összes hello adatlemezek HANA naplózza fájljai és konfigurációs fájlok. Ehelyett hello lényege, dedikált toohave szoftveres RAID csíkozást a több Azure adatok VHD-k között SAP HANA fájl teljes biztonsági mentés tárolásához. Csak ezek a lemezek, amelyek hello SAP HANA biztonsági másolat egy másolja. Dedikált HANA biztonsági mentési tárfiókokban könnyen megmarad, vagy dedikált tooa csatolt &quot;biztonsági mentése a felügyeleti virtuális gép&quot; további feldolgozásra.

![Érintett összes virtuális merevlemez másolása hello segítségével ** start-azurestorageblobcopy ** PowerShell-paranccsal](media/sap-hana-backup-file-level/image031.png)

Hello biztonsági mentési toohello helyi szoftveres RAID után minden érintett virtuális másolása hello segítségével **start-azurestorageblobcopy** PowerShell-parancsot (lásd: [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy)). Csak befolyásolja a dedikált hello fájlrendszere hello biztonságimásolat-fájlok, nincsenek nem kétségei SAP HANA adat- vagy naplófájl fájl konzisztencia hello lemezen. Ez a parancs előnye, hogy működik a miközben hello virtuális gép online marad. toobe bizonyos, hogy egyetlen folyamat sem ír toohello biztonsági mentési paritásos be van állítva, lehet, hogy toounmount előtt hello blob másolási, majd csatlakoztatnia azt újra később. Vagy egy használhat egy megfelelő túl&quot;rögzítése&quot; hello fájlrendszer. Például keresztül xfs\_hello XFS fájlrendszer rögzítése.

![Ezen a képernyőfelvételen látható blobok hello listáját hello VHD-k tárolóban az Azure-portálon hello jeleníti meg.](media/sap-hana-backup-file-level/image032.png)

Ezen a képernyőfelvételen látható blobok hello listája látható hello &quot;VHD-k&quot; hello Azure-portálon a tárolóban. hello képernyőfelvételen látható hello öt VHD-k, amelyek lenne, csatlakoztatva a toohello SAP HANA server VM tooserve hello szoftver RAID tookeep SAP HANA biztonságimásolat-fájlok formájában. Ezenfelül itt látható hello keresztül hello blob másolási parancs vett öt másolatát.

![Tesztelési célokra hello SAP HANA biztonsági mentési szoftver RAID lemezek hello példánya volt csatolt toohello alkalmazáskiszolgálóval VM](media/sap-hana-backup-file-level/image033.png)

Tesztelési célokra hello SAP HANA biztonsági mentési szoftver RAID lemezek hello példánya volt csatolt toohello app server rendszerű virtuális Géphez.

![hello alkalmazáskiszolgálóval VM tooattach hello lemez másolja le volt állítva.](media/sap-hana-backup-file-level/image034.png)

hello alkalmazáskiszolgálóval VM tooattach hello lemez másolja le volt állítva. Hello VM indítás után hello lemezek és hello RAID derített fel megfelelően (csatlakoztatott UUID keresztül). Csak a hello csatlakoztatási pont hiányzott, amely hello YaST particionáló lett létrehozva. Ezt követően hello SAP HANA biztonságimásolat-fájlt másolja vált látható az operációs rendszer szintjén.

## <a name="copy-sap-hana-backup-files-toonfs-share"></a>Másolja a fájlokat biztonsági mentési SAP HANA tooNFS megosztása

toolessen hello gyakorolt lehetséges hatásának a hello SAP HANA-rendszer a teljesítmény vagy a szabad lemezterület szempontjából, egy esetleg érdemes tárolni hello SAP HANA biztonságimásolat-fájlok az NFS-megosztások. Technikailag működik, de az azt jelenti, hogy egy második Azure virtuális gép használja, mint az NFS-megosztások hello hello gazdagépe. Nem lehet egy kis Virtuálisgép-méretet toohello VM hálózati sávszélesség miatt. Ez tenné logika majd lefelé ez tooshut &quot;biztonsági mentése a virtuális gép&quot; és csak emelni feliratkozott hello SAP HANA biztonsági mentés végrehajtásakor. Az NFS írás megosztás hello hálózati terheléselosztási helyezi és hatások hello SAP HANA-rendszer, de csupán kezelése hello biztonságimásolat-fájlok ezután hello &quot;biztonsági mentése a virtuális gép&quot; volna nem befolyásolják hello SAP HANA-rendszer semmihez.

![Az NFS-megosztások egy másik Azure virtuális gép csatlakoztatott toohello SAP HANA-kiszolgáló virtuális gép volt](media/sap-hana-backup-file-level/image035.png)

tooverify hello NFS-és nagybetűhasználattal, az NFS-megosztások egy másik Azure virtuális gép csatlakoztatott toohello SAP HANA-kiszolgáló virtuális gép volt. Hiba történt az semmilyen különleges NFS hangolása alkalmazza.

![1 óra és 46 perc toodo hello biztonsági mentés közvetlenül tartott](media/sap-hana-backup-file-level/image036.png)

hello NFS-megosztás lett gyors paritásos készletként, például a hello hello SAP HANA-kiszolgálón. Ettől függetlenül végrehajtásának 1 óra és 46 perc toodo hello közvetlenül hello NFS-megosztás helyett 10 perc, a biztonsági mentési tooa helyi paritásos set írása közben.

![hello alternatív nem sokkal gyorsabb, 1 óra 43 perc](media/sap-hana-backup-file-level/image037.png)

hello alternatív történt a biztonsági mentési tooa helyi paritásos készletként, és toohello másolása az NFS-megosztások OS szinten (egy egyszerű **cp - avr** parancs) nem sokkal gyorsabb. Végrehajtásának 1 óra 43 perc.

Így működik, de a teljesítmény nem volt helyes hello 230 GB-os biztonsági mentési teszteléshez. Lenne a több terabájt még rosszabb.

## <a name="copy-sap-hana-backup-files-tooazure-file-service"></a>Másolja a biztonságimásolat-fájlok tooAzure Fájlszolgáltatás SAP HANA

Egy Azure fájlmegosztás belül az Azure Linux virtuális gép lehetséges toomount. hello cikk [hogyan toouse Linux Azure File storage](../../../storage/files/storage-how-to-use-files-linux.md) kapcsolatos részleteket tartalmazza toodo azt. Ne feledje, hogy jelenleg egy 5-TB-os kvótakorlátot egy Azure fájlmegosztás és a fájl mérete legfeljebb 1 TB-os fájlonként. Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](../../../storage/common/storage-scalability-targets.md) tárhelyet olvashat.

Tesztek kimutatták, azonban SAP HANA biztonsági mentési állapotszolgáltatáson &#39; t közvetlenül az ilyen típusú CIFS csatlakoztatási jelenleg működik. Azt is van megadva a [SAP Megjegyzés 1820529](https://launchpad.support.sap.com/#/notes/1820529) CIFS nem ajánlott.

![Az ábrán látható hiba az SAP HANA Studio hello a biztonsági mentési párbeszédpanelen](media/sap-hana-backup-file-level/image038.png)

Ez a szám tooback közvetlenül tooa CIFS csatlakoztatott Azure fájlmegosztás mentése közben hello a SAP HANA Studio, a biztonsági mentés párbeszédpanelen jelez hibát. Így egy toodo egy szabványos SAP HANA biztonsági mentéssel rendelkező VM fájl rendszerben először, és majd másolási hello biztonsági mentés fájljainak ott tooAzure file szolgáltatás.

![Az ábrán látható, hogy tartott kapcsolatos 929 másodperc toocopy 19 SAP HANA biztonságimásolat-fájlok](media/sap-hana-backup-file-level/image039.png)

Az ábrán látható, hogy tartott 929 másodperc toocopy kapcsolatos 19 SAP HANA biztonságimásolat-fájlok a teljes méret nagyjából 230 GB toohello Azure fájlmegosztás.

![hello forrás mappastruktúrát hello SAP HANA VM lett másolt toohello Azure fájlmegosztás](media/sap-hana-backup-file-level/image040.png)

Ezen a képernyőfelvételen látható egy láthatja, hogy hello forrás mappastruktúrát hello SAP HANA VM volt-e a másolt toohello Azure fájlmegosztás: egy könyvtárat (hana\_biztonsági mentési\_fsl\_15 gb) és egyes biztonsági mentési fájlok 19.

SAP HANA biztonságimásolat-fájlok az Azure files tárolása lehet jövőbeni hello érdekes lehetőségek SAP HANA-fájlok biztonsági másolatait közvetlenül támogatja. Vagy lehetséges válásakor toomount Azure fájlok NFS keresztül, és hello kvótakorlátot jelentősen nagyobb, mint 5 TB.

## <a name="next-steps"></a>Következő lépések
* [Biztonsági mentési útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md) áttekintése és bevezető információkat biztosít.
* [A storage-pillanatfelvételekkel alapján SAP HANA biztonsági mentés](sap-hana-backup-storage-snapshots.md) hello tárolási pillanatkép-alapú biztonsági mentési beállítás ismerteti.
* Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).
