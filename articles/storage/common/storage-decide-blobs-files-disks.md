---
title: "Amikor toouse Azure Blobok aaaDeciding, Azure-fájlok vagy Azure adatlemezek"
description: "További információk a hello különböző módokon toostore és a hozzáférési adatok Azure toohelp úgy dönt, hogy mely technológia toouse."
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
ms.openlocfilehash: cd43abde43daf33dd7c43aa2696a9c8d5cd19612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deciding-when-toouse-azure-blobs-azure-files-or-azure-data-disks"></a>Annak eldöntése, amikor toouse Azure Blobok, Azure-fájlok vagy Azure adatlemezek

A Microsoft Azure számos szolgáltatást biztosít az Azure Storage tárolja, és a hello felhő adatait. Ez a cikk ismerteti az Azure-fájlok, Blobok és adatlemezek, és válassza a szolgáltatások között kialakított toohelp.

## <a name="scenarios"></a>Forgatókönyvek

hello a következő táblázat összehasonlítja a fájlok, Blobok és adatlemezek, és az egyes megfelelő példaforgatókönyvek jeleníti meg.

| Szolgáltatás | Leírás | Ha toouse |
|--------------|-------------|-------------|
| **Az Azure Files** | SMB illesztőfelület, klienskódtárak segítségével, és egy [REST-felület](/rest/api/storageservices/file-service-rest-api) , amely lehetővé teszi, hogy bárhonnan toostored fájlokat. | Azt szeretné, hogy túl "növekedési és az eltolás mértékét megadó" az alkalmazás toohello felhő hello natív rendszer API-k tooshare fájladatok és más Azure-ban futó alkalmazások közötti már használja.<br/><br/>Érdemes toostore fejlesztési és hibakeresés érhető el, amely több virtuális gép toobe igénylő eszközök. |
| **Azure-blobokat** | Ügyfélkódtárakat biztosít, és egy [REST-felület](/rest/api/storageservices/blob-service-rest-api) , amely lehetővé teszi, hogy a strukturálatlan adatok túl tárolhatók és egy nagy méretű a blokkblobokhoz érhető el. | Szeretné, hogy az alkalmazások toosupport adatfolyamként való továbbítására és elérésű forgatókönyvek.<br/><br/>Azt szeretné, hogy toobe képes tooaccess alkalmazásadatok bárhonnan. |
| **Az Azure Data lemezek** | Ügyfélkódtárakat biztosít, és egy [REST-felület](/rest/api/compute/virtualmachines/virtualmachines-create-or-update) , amely lehetővé teszi az adatok toobe tartósan tárolja, és egy csatolt virtuális merevlemez érhető el. | Szeretné, hogy toolift, és az eltolás mértékét megadó alkalmazások, amelyek natív fájl rendszer API-k tooread és adatlemezek toopersistent írni.<br/><br/>Azt szeretné, amely nem érhető el a külső hello virtuális gép toowhich hello lemezről szükséges toobe toostore adatok van csatolva. |

## <a name="comparison-files-and-blobs"></a>Összehasonlítás: Fájlok és Blobok

a következő táblázat hello Azure fájlokat a Azure BLOB hasonlítja össze.  
  
||||  
|-|-|-|  
|**Attribútum**|**Azure-blobokat**|**Az Azure Files**|  
|Tartóssági beállítások|LRS, a zrs-t, a GRS (és a magas rendelkezésre állás érdekében az RA-GRS)|LRS, GRS|  
|Kisegítő lehetőségek|REST API-k|REST API-k<br /><br /> SMB 2.1 és az SMB 3.0 (szabványos fájlrendszere API-k)|  
|Kapcsolatok|REST API-k – világszerte|REST API-k - világszerte<br /><br /> SMB 2.1--régión belül<br /><br /> Az SMB 3.0--világszerte|  
|Végpontok|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Könyvtárak|Egyszerű névtér|Igaz címtárobjektumok|  
|A nevek nagybetűk|Kis-és nagybetűket|Kis-és nagybetűk megkülönböztetése nélkül, de megőrzi az eset|  
|Kapacitás|Too500 TB tárolók mentése|5 TB fájlmegosztások|  
|Teljesítmény|Too60 MB/s sebességet blokkblob mentése|Másolatot megosztásonként too60 MB/s|  
|Objektum mérete|Too200 GB/blokkblob mentése|Too1TB/fájl mentése|  
|Számlázott kapacitás|Írt bájtok alapján|A fájl mérete alapján|  
|Klienskódtárak|Több nyelv|Több nyelv|  
  
## <a name="comparison-files-and-data-disks"></a>Összehasonlítás: Fájlok és adatlemezek

Az Azure Files Azure adatlemezek kiegészíteni. Adatlemez csak lehet csatolt tooone Azure virtuális gép egyszerre. Adatlemezek rögzített formátumú virtuális merevlemezeket lapblobokat az Azure Storage tárolja, és hello virtuális gép toostore tartós adatokat használják. Azure-fájlokban szereplő fájlmegosztások is elérhetők a hello ugyanaz, mint hello helyi lemez (fájlrendszer API-k használatával) érhető el, és több virtuális gép között megosztható legyen.  
 
a következő táblázat hello Azure Adatlemezekkel rendelkező Azure fájlok hasonlítja össze.  
 
||||  
|-|-|-|  
|**Attribútum**|**Az Azure Data lemezek**|**Az Azure Files**|  
|Hatókör|Kizárólagos tooa egyetlen virtuális gép|Több virtuális gép között megosztott hozzáférés|  
|A pillanatképek és másolása|Igen|Nem|  
|Konfiguráció|A virtuális gép hello indításakor csatlakoztatva|Miután elindult hello virtuális gép csatlakoztatva|  
|Authentication|Beépített|Net use beállítása|  
|Tisztítás|Automatikus|Manuális|  
|REST-hozzáférés|Hello VHD található fájl nem érhető el|A megosztáson található fájlok is elérhetők.|  
|Maximális méret|1 TB méretű lemez|5 TB fájlmegosztási és 1 TB-os fájl megosztáson belüli|  
|Maximális iops-érték 8 KB-os|500 IOps|1000 IOps|  
|Teljesítmény|Másolatot lemezenként too60 MB/s|Too60 MB/s egy fájl mentése|  

## <a name="next-steps"></a>Következő lépések

Az adatok tárolásának és elért hogyan kapcsolatos döntések meghozásakor is érdemes hello költségek jár. További információkért lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).
  
Egyes SMB-szolgáltatások nincsenek alkalmazandó toohello felhő. További információkért lásd: [hello Azure File service által nem támogatott funkciók](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Az adatlemezek kapcsolatos további információkért lásd: [lemezek és lemezképek kezelése](../../virtual-machines/windows/about-disks-and-vhds.md) és [hogyan tooAttach egy adatlemez tooa Windows rendszerű virtuális gép](../../virtual-machines/windows/classic/attach-disk.md).
