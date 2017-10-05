---
title: "Az Azure BLOB, Azure-fájlok vagy Azure adatlemezek való"
description: "Ismerje meg a különböző módszereket tárolhatja és érheti el az Azure segítségével adatokat úgy dönt, hogy melyik technológiát használja."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: robinsh
ms.openlocfilehash: e282a8a7bec1cb6e48110e7c9859a3a19ab8a11e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-data-disks"></a>Az Azure BLOB, Azure-fájlok vagy Azure adatlemezek való

A Microsoft Azure számos szolgáltatást biztosít az Azure Storage tárolja, és a felhőben tárolt adatainak eléréséhez. Ez a cikk ismerteti az Azure-fájlok, Blobok és adatlemezek, és célja, hogy ezek a funkciók választás.

## <a name="scenarios"></a>Forgatókönyvek

Az alábbi táblázat összehasonlítja a fájlok, Blobok és adatlemezek, és az egyes megfelelő példaforgatókönyvek jeleníti meg.

| Szolgáltatás | Leírás | A következő esetekben használja |
|--------------|-------------|-------------|
| **Az Azure Files** | SMB illesztőfelület, klienskódtárak segítségével, és egy [REST-felület](/rest/api/storageservices/file-service-rest-api) , amely lehetővé teszi, hogy bárhonnan tárolt fájlokhoz. | "A növekedési és az eltolás mértékét megadó" szeretne egy alkalmazást a felhőbe, amely már használja a natív fájlrendszer API-k és más Azure-ban futó alkalmazások közötti adatmegosztásra.<br/><br/>Fejlesztési és hibakeresési eszközök, amelyek több virtuális gép elérhető kell tárolni szeretné. |
| **Azure-blobokat** | Ügyfélkódtárakat biztosít, és egy [REST-felület](/rest/api/storageservices/blob-service-rest-api) , amely lehetővé teszi, hogy a strukturálatlan adatok tárolása és egy nagy méretű a blokkblobokhoz érhető el. | Szeretné, hogy az alkalmazás támogassa a streaming és elérésű forgatókönyvek.<br/><br/>Érdemes tudni alkalmazásadatok hozzáférni bárhonnan. |
| **Az Azure Data lemezek** | Ügyfélkódtárakat biztosít, és egy [REST-felület](/rest/api/compute/virtualmachines/virtualmachines-create-or-update) , amely lehetővé teszi az adatok tartósan tárolja, és egy csatolt virtuális merevlemez érhető el. | Szeretné növekedési és az eltolás mértékét megadó olvasási és írási adatok állandó lemezek natív fájlrendszer API-k használó alkalmazások.<br/><br/>Szeretné tárolni az adatokat, nincs szükség a virtuális gép, amelyen a lemez csatolva van kívül érhető el. |

## <a name="comparison-files-and-blobs"></a>Összehasonlítás: Fájlok és Blobok

Az alábbi táblázat összehasonlítja a Azure BLOB Azure fájlokat.  
  
||||  
|-|-|-|  
|**Attribútum**|**Azure-blobokat**|**Az Azure Files**|  
|Tartóssági beállítások|LRS, a zrs-t, a GRS (és a magas rendelkezésre állás érdekében az RA-GRS)|LRS, GRS|  
|Kisegítő lehetőségek|REST API-k|REST API-k<br /><br /> SMB 2.1 és az SMB 3.0 (szabványos fájlrendszere API-k)|  
|Kapcsolatok|REST API-k – világszerte|REST API-k - világszerte<br /><br /> SMB 2.1--régión belül<br /><br /> Az SMB 3.0--világszerte|  
|Végpontok|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Könyvtárak|Egyszerű névtér|Igaz címtárobjektumok|  
|A nevek nagybetűk|Kis-és nagybetűket|Kis-és nagybetűk megkülönböztetése nélkül, de megőrzi az eset|  
|Kapacitás|Legfeljebb 500 TB tárolók|5 TB fájlmegosztások|  
|Teljesítmény|Legfeljebb 60 MB/s sebességet blokkblob|Az egy legfeljebb 60 MB/s|  
|Objektum mérete|200 GB/blokkblob|Legfeljebb 1 TB-os vagy fájlrendszer|  
|Számlázott kapacitás|Írt bájtok alapján|A fájl mérete alapján|  
|Klienskódtárak|Több nyelv|Több nyelv|  
  
## <a name="comparison-files-and-data-disks"></a>Összehasonlítás: Fájlok és adatlemezek

Az Azure Files Azure adatlemezek kiegészíteni. Adatlemez csak akkor csatolható számára egy Azure virtuális gép egyszerre. Adatlemezek rögzített formátumú virtuális merevlemezeket tárolja az Azure Storage lapblobokat, és a virtuális gép által tartós adatok tárolására használt. Azure-fájlokban szereplő fájlmegosztások is elérhetők ugyanúgy, mint a helyi lemez (fájlrendszer API-k használatával) érhető el, és több virtuális gép között megosztható legyen.  
 
Az alábbi táblázat összehasonlítja Azure Adatlemezekkel rendelkező Azure fájlokat.  
 
||||  
|-|-|-|  
|**Attribútum**|**Az Azure Data lemezek**|**Az Azure Files**|  
|Hatókör|Csak egyetlen virtuális gép|Több virtuális gép között megosztott hozzáférés|  
|A pillanatképek és másolása|Igen|Nem|  
|Konfiguráció|A virtuális gép indításakor csatlakoztatva|A virtuális gép megkezdése után csatlakoztatva|  
|Authentication|Beépített|Net use beállítása|  
|Tisztítás|Automatikus|Manuális|  
|REST-hozzáférés|A virtuális Merevlemezen található fájl nem érhető el|A megosztáson található fájlok is elérhetők.|  
|Maximális méret|1 TB méretű lemez|5 TB fájlmegosztási és 1 TB-os fájl megosztáson belüli|  
|Maximális iops-érték 8 KB-os|500 IOps|1000 IOps|  
|Teljesítmény|Lemezenként legfeljebb 60 MB/s|Legfeljebb 60 MB/s sebességet fájlmegosztás|  

## <a name="next-steps"></a>Következő lépések

Az adatok tárolásának és elért hogyan kapcsolatos döntések meghozásakor is érdemes a költségek jár. További információkért lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).
  
A felhő nem alkalmazhatók egyes SMB-szolgáltatások. További információkért lásd: [az Azure File service által nem támogatott funkciók](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Az adatlemezek kapcsolatos további információkért lásd: [lemezek és lemezképek kezelése](storage-about-disks-and-vhds-linux.md) és [hogyan lehet adatlemezt csatolni egy Windows rendszerű virtuális gép](../virtual-machines/windows/classic/attach-disk.md).