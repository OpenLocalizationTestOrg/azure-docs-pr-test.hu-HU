---
title: "Azure File storage kapcsolatos gyakori kérdések |} Microsoft Docs"
description: "Azure File storage gyakran feltett kérdésekre adott válaszok."
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
ms.date: 07/19/2017
ms.author: renash
ms.openlocfilehash: 17868591e1056ff2bddde0e082695af529f58f02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Azure File storage kapcsolatos gyakori kérdések

## <a name="general"></a>Általános kérdések
* **Q. Mi az Azure File storage?**  
   
    Az Azure File storage az elosztott fájlrendszer az Azure-ban. Biztosít egy SMB protokoll illesztőfelületet, amely lehetővé teszi a felhasználóknak a tároló csatlakoztatása natív megosztásnak támogatott Azure virtuális gépen vagy a helyi számítógépen.

* **Q. Miért érdemes az Azure File storage hasznos?**  
   
    Az Azure File storage megosztott adatokhoz való hozzáférést biztosít több virtuális gépek és platformokat. Tekintse meg [akkor hasznos, miért Azure File storage](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **Q. Mikor használjak Azure File vs Azure Blob vs Azure lemez?**  
   
    A Microsoft Azure többféleképpen is tárolhatja és érheti el az adatok a felhőben.  
   
    Az Azure File storage - szolgáltató SMB illesztőfelületet, klienskódtárak segítségével, és egy REST-felület, amely lehetővé teszi, hogy könnyen elérhetők az bárhol tárolt fájlokhoz.  
   
    Azure-Blobok - ügyfél függvénytárainak és egy REST-felület, amely lehetővé teszi a strukturálatlan adatok tárolása és egy nagy méretű a blokkblobokhoz érhető el.  
   
    Az Azure Data lemezek - ügyfél függvénytárainak és egy REST-felület, amely lehetővé teszi az adatok tartósan tárolja, és egy csatolt virtuális merevlemez érhető el.  
   
    A további [való Azure BLOB, Azure-fájlok vagy Azure adatlemezek használata](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* **Q. Hogyan kezdjem el az Azure File storage?**  
   
    Hozzon létre egy Azure fájlmegosztás megkezdése. Az Azure fájlmegosztások Azure portál, az Azure Storage PowerShell parancsmagjainak, az Azure Storage ügyfélkódtáraival vagy az Azure Storage REST API használatával hozhat létre. Ebben az oktatóanyagban témák köre:

    * [Útmutató a portál használatával Azure fájlmegosztás létrehozása](storage-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Útmutató Powershell-lel Azure fájlmegosztás létrehozása](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Útmutató: Azure parancssori felület használatával fájlmegosztás létrehozása](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **Q. Milyen replikációk támogat az Azure File storage?**  
   
    Az Azure File storage csak támogatja az LRS- vagy GRS most. Az RA-GRS támogatása szerepel a terveink között, de a bevezetés időpontja még nem ismert.

## <a name="security-authentication-and-access-control"></a>Biztonsági, hitelesítési és hozzáférés-vezérlés

* **Q. Mik azok a fájlok elérése az Azure File storage különböző módjai?**
    
    Akkor is csatlakoztathatja a fájlmegosztást SMB 3.0 protokoll használatával a helyi számítógépen vagy eszközök, például [Tártallózó](http://storageexplorer.com/) a fájlmegosztás található fájlokat. Az alkalmazásból használhatja a storage ügyfélkódtáraival, REST API-k vagy a Powershell a fájlok az Azure-fájlmegosztás eléréséhez.

* **Q. Hogyan biztosíthat hozzáférést egy adott fájlt, egy webböngésző segítségével?**
    
    Az SAS segítségével is létrehozhat, amelyek érvényesek a megadott időtartam különleges engedélyekkel rendelkező jogkivonatokat. Létrehozhat például egy jogkivonatot, amely csak olvasási hozzáféréssel rendelkezik egy bizonyos fájlhoz egy megadott ideig. Bárki, aki rendelkezik az URL-cím érhetik el a fájlt közvetlenül bármely webböngészővel érvényes állapotában. Az SAS-kulcsok egyszerűen létrehozhatóak például olyan felhasználói felületekkel, mint a Storage Explorer.

* **Q. Ennyi az egész lehetséges, adja meg a csak olvasható vagy csak írási engedélyeket a megosztáson belüli mappákhoz?**
    
    Ha a fájlmegosztást SMB-protokollal csatlakoztatja, nem kezelheti ilyen szinten a fájlmegosztást. Az viszont megoldás lehet, ha létrehoz közös hozzáférésű jogosultságkódokat (SAS) a REST API-n vagy a klienskódtáron keresztül.  

* **Q. Hogyan engedélyezhetem a kiszolgálóoldali titkosítás az Azure File storage?**

    [Kiszolgálóoldali titkosítás](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) az Azure File storage általánosan elérhető régiók és a nyilvános és a nemzeti. Az Azure File storage segítségével engedélyezheti az SSE [Azure-portálon](https://portal.azure.com/),[Microsoft Azure Storage erőforrás szolgáltató API](/rest/api/storagerp/storageaccounts), [Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) vagy [Azure parancssori felület ](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    
    Miután engedélyezte a SSE az Azure File storage, a fájl tárhelyen, storage-fiókhoz tartozó új adatokat automatikusan titkosítva lesznek. Ez a funkció a meglévő vagy új tárfiók minden meglévő vagy új megosztására írt új adathoz elérhető. A szolgáltatás díjmentesen engedélyezhető. A további [SSE engedélyezése az Azure File storage](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **Q. Azure File Storage támogatja az Active Directory-alapú hitelesítés?**
   
    Jelenleg nem támogatjuk az AD-alapú hitelesítést vagy az ACL-eket, de a támogatás már szerepel a funkciókra vonatkozó kérések listáján. Egyelőre az Azure Storage-fiókkulcsok segítségével hitelesíthető a fájlmegosztás. Létezik egy megkerülő megoldás is, amely közös hozzáférésű jogosultságkódokat (SAS) használt a REST API vagy a klienskódtárak segítségével. Az SAS segítségével is létrehozhat, amelyek érvényesek a megadott időtartam különleges engedélyekkel rendelkező jogkivonatokat. Például a jogkivonatot az adott fájlba 10 perc lejárati csak olvasási hozzáféréssel is létrehozhat. Csak olvasási hozzáféréssel bárki, aki rendelkezik a token érvényességi ideje alatt azt rendelkezik, amely a fájl ezen 10 percig.
   
    Az SAS csak REST API vagy klienskódtárak használatával támogatott. Ha a fájlmegosztást SMB-protokollal csatlakoztatja, SAS-kód nem használható a tartalmához való hozzáférés delegálására. 
    
* **Q. Mik az Azure File Storage támogatott megfelelőségi szabályzatok?**

   Az Azure File Storage a azonos tárolási architektúrával rendelkezik, mint más tárolási szolgáltatások az Azure Storage fut, és alkalmazza az azonos megfelelőségi szabályzatok. További információ az Azure Storage adatok megfelelőségi letöltheti, és tekintse meg [Microsoft Azure Data Protection dokumentum](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Helyszíni hozzáférés

* **Q.Do kell használni az Azure ExpressRoute egy helyszíni virtuális Gépre az Azure File storage csatlakozni?**
   
    Nem. Akkor is hozzáférhet a fájlmegosztáshoz a helyszíni virtuális gépekről, ha nem rendelkezik ExpressRoute-tal, feltéve, hogy tud a 445-ös (TCP, kimenő) porton keresztül csatlakozni az internethez. Azonban használhatja ExpressRoute az Azure File storage Ha szeretné.

* **Q. Hogyan lehet egy Azure fájlmegosztás csatlakoztatása a helyi számítógépen?** 
    
    Akkor is csatlakoztathatja a fájlmegosztást SMB-protokollal, amíg a port 445 (TCP, kimenő) meg nyitva, és az ügyfél támogatja az SMB 3.0 protokollt (például használata a Windows 10 vagy Windows Server 2012-ben). A port zárolásának feloldásához lépjen kapcsolatba a helyi internetszolgáltatóval. A belső, megtekintheti a fájlokat a [Tártallózó](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Számlázási és az árképzés terén
* **Q. Hálózati forgalom az Azure virtuális gép és a fájlszám megosztás között külső sávszélesség, amelyet az előfizetést?**
   
    Ha a fájlmegosztás és a virtuális gép ugyanazon Azure-régióban, akkor kimenő forgalomról közöttük szabad. Ha különböző régiókban, adatforgalom külső sávszélességnek minősül, fogjuk számlázni.

## <a name="backup"></a>Biztonsági mentés

* **Q. Hogyan biztonsági mentését a saját Azure fájlmegosztás?**
    
    A 3. fél biztonsági mentési eszköz, amely egy csatlakoztatott fájlmegosztást készíthet biztonsági másolatot, vagy használja az AzCopy, RoboCopy. Várhatóan képesek legyenek a pillanatkép fájlmegosztások a közeljövőben; e szolgáltatás használatával az Azure-fájlmegosztás biztonsági mentését lehet.

## <a name="performance"></a>Teljesítmény

* **Q. Az Azure File storage a skálázási korlátai**
    Az Azure File storage méretezhetőségi és Teljesítménycélok információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **Q. A teljesítmény nem volt kielégítő, amikor fájlokat próbáltam kicsomagolni Azure fájlt a tárba. Mit tegyek?**
    
    Azure File storage nagyszámú fájlt át, azt javasoljuk, hogy használjon AzCopy (Windows, Linux/UNIX előzetes verzió) vagy az Azure Powershell, ezek az eszközök a hálózati átvitel optimalizált.

* **Q. Mi a javításokkal kiadása a teljesítményével probléma az Azure File storage?**
    
    A Windows csapata nemrégiben megjelentetett egy javítást a teljesítményromlást, amikor az ügyfél hozzáfér a Azure File storage a Windows 8.1 vagy Windows Server 2012 R2. További információkért vegye ki a KB-cikkben talál [csökkentheti a teljesítményt, amikor Azure File storage fér hozzá a Windows 8.1 vagy Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Szolgáltatások és egyéb szolgáltatások együttműködés
* **Q. Van egy "tanúsító fájlmegosztása" feladatátvevő fürt egyik használati esete az Azure File storage?**
   
    Ez jelenleg nem támogatott.

* **Q. Van-e a REST API átnevezési művelettel?**
   
    Jelenleg nem.

* **Q. Létrehozható beágyazott megosztás, azaz megosztás egy megosztásban?**
    
    Nem. A fájlmegosztás a virtuális meghajtó, amelyet csatlakoztathat, így nem támogatott a beágyazott megosztás.

* **Q. Az Azure File storage IBM MQ-val**
    
    Az IBM kiadott egy útmutató dokumentumot az IBM MQ ügyfelei Azure File storage az általuk használt szolgáltatással konfigurálásakor. További információk: [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service) (Az IBM MQ többpéldányos üzenetsor-kezelőjének beállítása a Microsoft Azure File szolgáltatással).


## <a name="troubleshooting"></a>Hibaelhárítás
* **Q. Hogyan hibáinak elhárítása az Azure File storage hibák?**
    
    Olvassa el a [Azure File storage hibaelhárítási cikk](storage-troubleshoot-windows-file-connection-problems.md) végpont hibaelhárítási útmutatót. 


## <a name="see-also"></a>Lásd még:
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Az Azure File Storage használata Linuxszal](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>File Storage-eszköztámogatás
* [How to use AzCopy with Microsoft Azure Storage (Az AzCopy használata a Microsoft Azure Storage szolgáltatással)](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Using the Azure CLI with Azure Storage (Az Azure parancssori felülete és az Azure Storage együttes használata)](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Az Azure File Storage-problémák hibaelhárítása](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Azure File Storage adatok áttelepítése](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referencia
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
