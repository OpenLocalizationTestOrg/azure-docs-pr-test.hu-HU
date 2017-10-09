---
title: a File storage aaaIntroduction tooAzure |} Microsoft Docs
description: "Azure File storage áttekintése, egy szolgáltatás, amely lehetővé teszi, hogy Ön toocreate és -felhasználási hálózati fájl közösen használja az hello a Microsoft Cloud hello iparági szabvány használatával."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: a0a6a80a2ccd9742aa470bdd02ff375387a1629b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-file-storage"></a>A File storage bemutatása tooAzure
Az Azure File storage hálózati fájlmegosztást a hello iparági szabványos hello felhő kínáló [Server Message Block (SMB) protokoll](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) és [Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Az Azure-fájlmegosztások párhuzamosan több ügyfél (például Windows, macOS vagy Linux rendszerek helyszíni üzemelő példányai) vagy Azure virtuális gép által is csatlakoztathatók. Egy általános célú tárfiókok biztosít hozzáférést a File Storage tooAzure és más szolgáltatásokon, például BLOB, Azure virtuális gépek lemezeit, várólisták egyetlen fiókban.



## <a name="videos"></a>Videók
| Az Azure File Storage ismertetése (27 perc) | Az Azure File Storage oktatóanyaga (5 perc)  |
|-|-|
| [![Introducing Azure File storage videó hello - Screencap kattintson tooplay!](media/storage-file-storage/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![A hello Azure File storage - oktatóanyag Screencap kattintson tooplay!](media/storage-file-storage/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Miért hasznos az Azure File Storage?
Az Azure File storage szolgáltatással tooreplace Windows Server, Linux, vagy NAS-alapú fájlkiszolgálókon helyben tárolt vagy megosztani az egy operációs rendszer szabad felhő fájl hello felhőben. Ez tartalmazza a következő előnyöket hello:

* **Megosztott hozzáférés:**. Az Azure fájlmegosztások támogatási hello iparági szabványos SMB protokoll, ami azt jelenti, hogy zökkenőmentesen lecserélheti a helyszíni fájlmegosztások Azure fájlmegosztásokat anélkül, hogy alkalmazás kompatibilitását. A fájlrendszer az képes tooshare alatt több számítógépen, alkalmazások vagy példány egy jelentős előnyt az Azure File storage shareability igénylő alkalmazásokhoz. 
* **Teljes mértékben felügyelt**. Az Azure fájlmegosztások hello kell toomanage hardver- vagy egy operációs rendszer nélkül hozhatók létre. Ez azt jelenti, hogy nincs toodeal hello server operációs rendszer kritikus fontosságú biztonsági frissítések javítását vagy hibás merevlemezek cseréje.
* **Szkripthasználat és eszköztámogatás**. PowerShell-parancsmagok és az Azure parancssori felület lehet használt toocreate, csatlakoztathatják és kezelhetik a File storage-megosztásokat az Azure-alkalmazások hello felügyeleti részeként. Hozhat létre és kezelheti az Azure portál és az Azure storage Explorer használatával Azure fájlmegosztásokat. 
* **Rugalmasság**. Az Azure File storage a mentése toobe mindig rendelkezésre álló szabad hello készült. A helyszíni fájlmegosztások lecserélését Azure File storage azt jelenti, hogy már nem kell toowake helyi áramkimaradások vagy hálózati problémák toodeal fel. 
* **Ismerős programozhatóság**. Az Azure-ban futó alkalmazások férhetnek fájlon keresztül hello megosztás adataihoz [rendszer i/o-API-k](https://msdn.microsoft.com/library/system.io.file.aspx). A fejlesztők ezért meglévő kódjaik és képességek toomigrate meglévő alkalmazások használhatják fel. Ezenkívül tooSystem I/O API-k, használhat [Azure Storage Ügyfélkódtáraival](https://msdn.microsoft.com/library/azure/dn261237.aspx) vagy hello [Azure Storage REST API felülete](/rest/api/storageservices/file-service-rest-api).

Az Azure-fájlmegosztások az alábbiakra használhatók:

* **Helyszíni fájlkiszolgálók lecserélése**:  
    Az Azure File storage lehet használt toocompletely csere fájlmegosztások hagyományos helyszíni fájlkiszolgálókon és NAS-eszközökön. Ismert operációs rendszerek, például Windows, a macOS és a Linux könnyen is csatlakoztathatja egy Azure-fájlmegosztás, bárhol legyenek is a hello world.

* **Alkalmazások „átemelése”**:  
    Az Azure File storage könnyen túl közösen használja az "növekedési és az eltolás mértékét megadó" alkalmazások toohello felhő, amelyek a helyi fájl tooshare adatok hello alkalmazás részei között. toomake Ez esetben fordulhat elő, minden virtuális gép kapcsolódik toohello fájlmegosztást, és ezután az olvasási és írási fájljait, ahogy azt egy helyi fájl elleni megosztásához.

* **A felhőalapú fejlesztés egyszerűsítése**:  
    Az Azure File storage több különböző módon toosimplify új felhőalapú fejlesztési projekt használható.
    * **Megosztott alkalmazásbeállítások**:  
        Az elosztott alkalmazások közös mintázatát toohave konfigurációs fájlok egy központi helyen, ha azok elérhetők a sok különböző virtuális gépek. Az ilyen konfigurációs fájlok mostantól az Azure-fájlmegosztásokban is tárolhatók, és az összes alkalmazáspéldány által olvashatók. Ezek a beállítások segítségével hello REST-felület, amely lehetővé teszi, hogy a globális hozzáférési toohello konfigurációs fájlok is kezelhetők.

    * **Diagnosztika megosztása**:  
        Egy Azure fájlmegosztás használt toosave diagnosztikai naplók, metrikák és összeomlási memóriaképeket fájlokat is lehet. A rendelkezésre álló hello SMB és a REST-felületen keresztül lehetővé teszi az alkalmazások toobuild vagy számos különféle feldolgozási és hello diagnosztikai adatok elemzése elemzésére szolgáló eszközöket használja.

    * **Fejlesztés/Tesztelés/Hibakeresés**:  
        Ha a fejlesztők és rendszergazdák hello felhő virtuális gépek dolgozik, azok eszközök vagy segédprogramok gyakran kell. Az ilyen segédprogramok telepítése és kiosztása az egyes virtuális gépekre, ahol éppen szükség van rájuk, sok időt vehet igénybe. Az Azure File storage szolgáltatással a fejlesztők és a rendszergazda tudja tárolni a kedvenc eszközök egy fájlmegosztást, amely lehet könnyen csatlakoztatott toofrom bármely virtuális gép.
        
## <a name="how-does-it-work"></a>Hogyan működik?
Az Azure-fájlmegosztások kezelése sokkal egyszerűbb, mint a helyszíni fájlmegosztások esetében. a következő diagram hello hello Azure File storage management szerkezetek mutatja be:

![Fájlstruktúra](../../includes/media/storage-file-concepts-include/files-concepts.png)

* **A Tárfiók**: összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. A tárfiókok kapacitásával kapcsolatos további információkért lásd: Az Azure Storage méretezhetőségi és teljesítménycéljai.
* **Megosztás**: A File Storage-megosztás egy SMB-fájlmegosztás az Azure-ban. Minden könyvtárnak és fájlnak egy szülőmegosztásban kell létrejönnie. Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello 5 TB-os kapacitásáig hello fájlmegosztás korlátlan számú.
* **Könyvtár**: Egy választható könyvtár-hierarchia.
* **Fájl**: hello megosztáson található, A fájl. Lehet, hogy a fájl mentése too1 TB-nál.
* **URL-formátum**: a fájlok megcímezhető hello URL-cím formátuma a következő használatával:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```
## <a name="next-steps"></a>Következő lépések
* [Azure-fájlmegosztás létrehozása](storage-file-how-to-create-file-share.md)
* [Csatlakoztatás Windows rendszeren](storage-file-how-to-use-files-windows.md)
* [Csatlakoztatás Linux rendszeren](storage-how-to-use-files-linux.md)
* [Csatlakoztatás macOS rendszeren](storage-file-how-to-use-files-mac.md)
* [Gyakori kérdések](storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Azure File Storage-eszköztámogatás
* [Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)](storage-powershell-guide-full.md)
* [Hogyan toouse Microsoft Azure Storage AzCopy](storage-use-azcopy.md)
* [Az Azure Storage hello Azure parancssori felület használatával](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Áttelepítési adatok tooAzure fájl](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referencia
* [az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
