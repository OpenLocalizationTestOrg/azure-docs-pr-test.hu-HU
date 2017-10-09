---
title: a File storage aaaIntroduction tooAzure |} Microsoft Docs
description: "Bevezetés tooAzure fájl tárolási, hálózati fájlt biztosító osztja meg a Microsoft Cloud hello"
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
ms.openlocfilehash: fe6826e79c364a6956831d2e273c4342a5fd47f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-file-storage"></a>A File storage bemutatása tooAzure

Az Azure File storage hálózati fájlmegosztást a hello iparági szabványos hello felhő kínáló [Server Message Block (SMB) protokoll](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) és [Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Az Azure-fájlmegosztások párhuzamosan csatlakoztathatók az Azure Virtual Machines és a Windows, macOS vagy Linux rendszerek helyszíni üzemelő példányai által. Egy általános célú tárfiókok biztosít hozzáférést tooAzure a File storage Azure Blob storage és Azure Queue storage.

## <a name="videos"></a>Videók
| Az Azure File Storage ismertetése (27 perc) | Az Azure File Storage oktatóanyaga (5 perc)  |
|-|-|
| [![Képernyőfelvétel a hello Introducing Azure File storage video - tooplay kattintson!](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Képernyőfelvétel a hello Azure File storage - oktatóanyag kattintson tooplay!](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Miért hasznos az Azure File Storage?

Az Azure File storage szolgáltatással tooreplace Windows Server, Linux, vagy NAS-alapú fájlkiszolgálókon helyben tárolt vagy megosztani az egy operációs rendszer szabad felhő fájl hello felhőben. Az Azure File storage a következő előnyöket hello rendelkezik:

* **Megosztott hozzáférés** Azure fájlmegosztások támogatási hello iparági szabványos SMB protokoll, ami azt jelenti, hogy zökkenőmentesen lecserélheti a helyszíni fájlmegosztások Azure fájlmegosztásokat anélkül, hogy alkalmazás kompatibilitását. Képes tooaccess éppen egy fájl több gépekről és alkalmazások/példány nem egy jelentős előnyt az Azure File storage szolgáltatással.

* **Teljes körűen felügyelt** Azure fájlmegosztásokat hello kell toomanage hardver vagy operációs, ami azt jelenti, hogy nincs toodeal hello server operációs rendszer kritikus fontosságú biztonsági frissítések javítását vagy hibás merevlemezek cseréje nélkül is létrehozható.

* **Parancsfájl-kezelési és tooling eszköz** PowerShell-parancsmagok és az Azure parancssori felület lehet használt toocreate, csatlakoztathatják és kezelhetik az Azure fájlmegosztások hello felügyeleti az Azure-alkalmazások részeként. Hozhat létre és kezelheti az Azure fájlmegosztások hello segítségével [Azure-portálon](https://portal.azure.com) és hello [Azure Tártallózó](https://storageexplorer.com). 

* **Rugalmasság** készült Azure File storage a mentése toobe mindig rendelkezésre álló szabad hello. A helyszíni fájlmegosztások lecserélését Azure File storage azt jelenti, hogy már nem kell toowake helyi áramkimaradások vagy hálózati problémák toodeal fel. 

* **Jól ismert programozhatóság** az Azure-ban futó alkalmazások adatok hello megosztáson keresztül férhetnek [fájlrendszer adatátviteli API-jain](https://msdn.microsoft.com/library/system.io.file.aspx). A fejlesztők ezért meglévő kódjaik és képességek toomigrate meglévő alkalmazások használhatják fel. Továbbá tooSystem I/O API-k, használhatja bármelyik hello az Azure storage ügyfél szalagtárak, például egy hello [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), vagy hello [Azure Storage REST API felülete](/rest/api/storageservices/file-service-rest-api).

Az Azure-fájlmegosztások az alábbiakra használhatók:

* **Cserélje le a helyszíni fájlkiszolgálókon** Azure File storage lehet használt toocompletely csere fájlmegosztások hagyományos helyszíni fájlkiszolgálókon és NAS-eszközökön. Ismert operációs rendszerek, például Windows, a macOS és a Linux könnyen is csatlakoztathatja egy Azure-fájlmegosztás, bárhol legyenek is a hello world.

* **Alkalmazások „átemelése”**

    Az Azure File storage könnyen túl közösen használja az "növekedési és az eltolás mértékét megadó" alkalmazások toohello felhő, amelyek a helyi fájl tooshare adatok hello alkalmazás részei között. tooimplement az egyes virtuális gépek toohello fájlmegosztás kapcsolódik, és ezután az olvasási és írási fájljait, ahogy azt egy helyi fájl elleni megosztásához.

* **A felhőalapú fejlesztés egyszerűsítése**
    
    Az Azure File storage több különböző módon toosimplify új felhőalapú fejlesztési projekt használható.
    
    * **Megosztott alkalmazásbeállítások**
    
        Az elosztott alkalmazások közös mintázatát toohave konfigurációs fájlok egy központi helyen, ha azok elérhetők a sok különböző virtuális gépek. Az ilyen konfigurációs fájlok mostantól az Azure-fájlmegosztásokban is tárolhatók, és az összes alkalmazáspéldány által olvashatók. Ezek a beállítások segítségével hello REST-felület, amely lehetővé teszi, hogy a globális hozzáférési toohello konfigurációs fájlok is kezelhetők.

    * **Diagnosztika megosztása**
    
        Egy Azure fájlmegosztás használt toosave diagnosztikai naplók, metrikák és összeomlási memóriaképeket fájlokat is lehet. Hello SMB és a REST-felületen keresztül elérhető fájlmegosztások lehetővé teszi az alkalmazások toobuild vagy számos különféle feldolgozási és hello diagnosztikai adatok elemzése elemzésére szolgáló eszközöket használja.

    * **Fejlesztés/Tesztelés/Hibakeresés**

        Ha a fejlesztők és rendszergazdák hello felhő virtuális gépek dolgozik, azok eszközök vagy segédprogramok gyakran kell. Az ilyen segédprogramok telepítése és kiosztása az egyes virtuális gépekre, ahol éppen szükség van rájuk, sok időt vehet igénybe. Az Azure File storage szolgáltatással a fejlesztők és a rendszergazda tudja tárolni a kedvenc eszközök egy fájlmegosztást, amely lehet könnyen csatlakoztatott toofrom bármely virtuális gép.
        
## <a name="how-does-it-work"></a>Hogyan működik?

Az Azure-fájlmegosztások kezelése sokkal egyszerűbb, mint a helyszíni fájlmegosztások esetében. a következő diagram hello hello Azure File storage management szerkezetek mutatja be:

![Fájlstruktúra](./media/storage-files-introduction/files-concepts.png)

* **A Tárfiók** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Méretezhetőségi és teljesítménycélok).

* **Megosztás** A File Storage-megosztás egy SMB-fájlmegosztás az Azure-ban. Minden könyvtárnak és fájlnak egy szülőmegosztásban kell létrejönnie. Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello 5 TB-os kapacitásáig hello fájlmegosztás korlátlan számú.

* **Könyvtár** Egy választható könyvtár-hierarchia.

* **Fájl** egy hello megosztásban található fájl. Lehet, hogy a fájl mentése too1 TB-nál.

* **URL-formátum** fájljai megcímezhető hello URL-cím formátuma a következő használatával:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```

## <a name="next-steps"></a>Következő lépések

* [Azure-fájlmegosztás létrehozása](storage-how-to-create-file-share.md)
* [Csatlakoztatás Windows rendszeren](storage-how-to-use-files-windows.md)
* [Csatlakoztatás Linux rendszeren](storage-how-to-use-files-linux.md)
* [Csatlakoztatás macOS rendszeren](storage-how-to-use-files-mac.md)
* [Gyakori kérdések](../storage-files-faq.md)
* [Hibaelhárítás a Windows rendszerben](storage-troubleshoot-windows-file-connection-problems.md)   
* [Hibaelhárítás a Linux rendszerben](storage-troubleshoot-linux-file-connection-problems.md)   

<!-- Rena I would remove any articles from here that are more than a year old. - Robin-->
### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Azure File Storage-eszköztámogatás
* [Hogyan toouse Microsoft Azure Storage AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Az Azure Storage hello Azure parancssori felület használatával](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Áttelepítési adatok tooAzure fájl](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referencia
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
