---
title: "felhőalapú tárolás az Azure-ban az adatok nagy mennyiségű aaaMoving |} Microsoft Docs"
description: "Hello különböző módszerek áthelyezése adatok tooand az Azure Storage áttekintése."
services: storage
documentationcenter: 
author: JarrettRenshaw
manager: msmets
editor: tysonn
ms.assetid: 5e3947a9-d99b-4108-9d57-3eb67c03e7ba
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.openlocfilehash: 7c8ec9d99cbd48042b8146130c8827f9f25c024d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-tooand-from-azure-storage"></a>Adatok tooand áthelyezése az Azure Storage-ból
Ha azt szeretné, toomove a helyszíni adatok tooAzure Storage (vagy fordítva), nincsenek különböző módokon toodo ez. hello módszert alkalmaz, amely a legjobban, a forgatókönyvtől függ. Ez a cikk különböző alkalmazási helyzetek és minden egyes megfelelő ajánlatok gyors áttekintést biztosít.

## <a name="building-applications"></a>Alkalmazások létrehozása
Az alkalmazás most létrehozása, ha a kiváló módja toomove adatok tooand Azure Storage-ból hello REST API vagy valamelyik a sok ügyfél függvénytárainak fejlesztésről.

Az Azure Storage gazdag ügyfélkódtárakat biztosít a .NET, iOS, Java, Android, az univerzális Windows Platform (UWP), Xamarin, C++, Node.JS, PHP, Ruby és Python. hello ügyfélkódtárak olyan fejlett képességeket biztosítanak, például újrapróbálkozási logika, a naplózás vagy a párhuzamos feltöltések. Is fejleszthet közvetlenül a REST API, amely bármely olyan nyelvvel, amely lehetővé teszi a HTTP/HTTPS-kéréseket hívható hello szemben.

Lásd: [Ismerkedés az Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md) további toolearn.

Ezenkívül azt is kínál hello [Azure Storage adatátviteli könyvtár](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) Ez egy nagy teljesítményű tooand adatok másolása az Azure-ból készült könyvtár. Tekintse meg az adatátviteli könyvtár tooour [dokumentáció](https://github.com/Azure/azure-storage-net-data-movement) további toolearn. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Adatai gyors megtekintése, és interakció adataival
Ha szeretné, hogy egy egyszerűen tooview az Azure Storage-adatok hello képességét tooupload mellett is, és töltse le az adatokat, majd fontolja meg egy Azure Tártallózó használatát.

Tekintse meg a kiválasztott [Azure Tártallózók](../storage-explorers.md) további toolearn.

## <a name="system-administration"></a>Rendszer-felügyeleti
Ha szükséges, vagy több részeként (pl. a rendszergazdák) parancssori segédprogram, itt az Ön tooconsider néhány lehetőség áll:

### <a name="azcopy"></a>AzCopy
AzCopy egy Windows parancssori segédprogram készült Azure Storage-ból adatokat tooand másolását nagy teljesítményű. Adatok egy tárfiókon belül, vagy eltérő tárfiókokból között is másolhatja.

Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) további toolearn.

### <a name="azure-powershell"></a>Azure PowerShell
Az Azure PowerShell modul parancsmagokat biztosít az Azure-szolgáltatások kezeléséhez. Ez egy feladatalapú parancshéj és parancsnyelv, amely kifejezetten rendszer-felügyeleti célra készült.

Lásd: [Azure PowerShell használata az Azure Storage](storage-powershell-guide-full.md) további toolearn.

### <a name="azure-cli"></a>Azure CLI
Az Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsokon keresztül működő Azure-szolgáltatásokkal. Az Azure CLI Windows, OSX és Linux érhető el.

Lásd: [az Azure Storage Azure CLI használata hello](../storage-azure-cli.md) további toolearn.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Nagy mennyiségű adat egy lassú hálózati áthelyezése
Hello legnagyobb kihívást nagy mennyiségű adat áthelyezését társított egyik hello átvitelének az idejét. Ha azt szeretné tooget adatok Azure Storage és a hálózatok költségek aggódni és programozás nélkül, Azure Import/Export egy megfelelő megoldást.

Lásd: [Azure Import/Export](../storage-import-export-service.md) további toolearn.

## <a name="backing-up-your-data"></a>Az adatok biztonsági mentése
Ha egyszerűen kell toobackup az adatok tooAzure tárolási, Azure biztonsági mentés hello módon toogo. Ez egy hatékony megoldás a helyszíni adatokat és az Azure virtuális gépek biztonsági mentése.

Lásd: [Azure biztonsági mentés](../../backup/backup-introduction-to-azure-backup.md) további toolearn.

## <a name="accessing-your-data-on-premises-and-from-hello-cloud"></a>Az adatokat a helyszíni elérése és hello felhőből
Ha megoldásra van szüksége egy való hozzáféréshez szükséges adatokat a helyszíni és a hello felhőből, majd érdemes Azure hibrid felhőalapú tárolómegoldás, StorSimple használatával. Ez a megoldás, hogy intelligens módon tárolja a gyakran használt adatokat az SSD-k, alkalmanként használja az adatok a HDD-k, inaktív vagy biztonsági mentés/archiválási adatok Azure Storage a fizikai StorSimple eszköz áll.

Lásd: [StorSimple](../../storsimple/storsimple-overview.md) további toolearn.

## <a name="recovering-your-data"></a>Az adatok helyreállítása
Ha a helyszíni munkaterhelések és alkalmazások, olyan megoldás, amely lehetővé teszi, hogy a hello esemény egy olyan vészhelyzet esetén a futó üzleti toocontinue lesz szüksége. Az Azure Site Recovery kezeli a replikáció, feladatátvétel és helyreállítási virtuális gépek és fizikai kiszolgálók. A replikált adatok Azure Storage, hogy lehetővé teszi egy másodlagos helyszíni adatközpontba tooeliminate hello szükségességét tárolja.

Lásd: [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) további toolearn.
### <a name="moving-data-faq"></a>Gyakori kérdések az adatok mozgatása:
## <a name="can-i-migrate-vhds-from-one-region-tooanother-without-copying"></a>Áttelepíthetem a VHD-k az egy régióban tooanother másolása nélkül?
hello csak úgy toocopy VHD-k közötti terület az toocopy hello adatai között a minden egyes régió storage-fiókok. Az AZCopy is használhatja. Az AzCopy parancssori segédprogram toolearn hello további átviteli adatok megtekintéséhez. Nagyon nagy mennyiségű adat is Azure Import/Export. Lásd: [Azure Import/Export](https://docs.microsoft.com/en-us/azure/storage/storage-import-export-service) további toolearn.
