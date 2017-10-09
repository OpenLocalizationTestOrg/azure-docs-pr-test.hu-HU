---
title: "Microsoft Azure File storage kapcsolatban felmerülő kérdések aaaFrequently |} Microsoft Docs"
description: "Választ találhat az Azure File storage kérdések toofrequently."
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
ms.openlocfilehash: d8a94fdc77e928dbc6996e1e635cd3527e0c67d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Azure File storage kapcsolatos gyakori kérdések

## <a name="general"></a>Általános kérdések
* **Q. Mi az Azure File storage?**  
   
    Az Azure File storage az elosztott fájlrendszer az Azure-ban. Biztosít egy SMB protokoll illesztőfelületet, amely lehetővé teszi, hogy a felhasználók toomount hello tárolási natív megosztásnak támogatott Azure virtuális gépen vagy a helyi számítógépen.

* **Q. Miért érdemes az Azure File storage hasznos?**  
   
    Az Azure File storage megosztott adatokhoz való hozzáférést biztosít több virtuális gépek és platformokat. Tekintse meg a túl[akkor hasznos, miért Azure File storage](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **Q. Mikor használjak Azure File vs Azure Blob vs Azure lemez?**  
   
    A Microsoft Azure toostore és a hozzáférési adatok hello felhőben számos lehetőséget biztosít.  
   
    Az Azure File storage - SMB illesztőfelület, klienskódtárak és egy REST-felület, amely lehetővé teszi, hogy a könnyű hozzáférés bárhonnan toostored fájlok biztosít.  
   
    Azure-Blobok - ügyfél függvénytárainak és egy REST-felület, amely lehetővé teszi a strukturálatlan adatok toobe tárolja, és egy nagy méretű a blokkblobokhoz érhető el.  
   
    Az Azure Data lemezek - ügyfél függvénytárainak és egy REST-felület, amely lehetővé teszi az adatok toobe tartósan tárolja, és egy csatolt virtuális merevlemez érhető el.  
   
    A további [Deciding, amikor toouse Azure Blobok, Azure-fájlok vagy Azure adatlemezek](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* **Q. Hogyan kezdjem el az Azure File storage?**  
   
    Hozzon létre egy Azure fájlmegosztás megkezdése. Az Azure fájlmegosztások Azure portál, hello Azure Storage PowerShell parancsmagjainak, hello Azure Storage ügyfélkódtáraival vagy hello Azure Storage REST API használatával hozhat létre. Ebben az oktatóanyagban témák köre:

    * [Ismerje meg, hogyan toocreate Azure-fájl megosztása hello portál használatával](storage-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Ismerje meg, hogyan toocreate Azure-fájl megosztása Powershell használatával](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Ismerje meg, hogyan toocreate Azure-fájl megosztása parancssori felület használatával](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **Q. Milyen replikációk támogat az Azure File storage?**  
   
    Az Azure File storage csak támogatja az LRS- vagy GRS most. RA-GRS toosupport tervezzük, de nincs még nincs ütemterv tooshare.

## <a name="security-authentication-and-access-control"></a>Biztonsági, hitelesítési és hozzáférés-vezérlés

* **Q. Mik azok a különböző módokon tooaccess fájlok az Azure File storage?**
    
    Hello fájlmegosztást csatlakoztathatnak SMB 3.0 protokoll használatával a helyi számítógépen vagy eszközök, például [Tártallózó](http://storageexplorer.com/) tooaccess fájlok a fájlmegosztásban. Az alkalmazásból storage ügyfélkódtáraival, REST API-k vagy az Azure-fájl a fájlok megosztása Powershell tooaccess is használhatja.

* **Q. Hogyan biztosíthat hozzáférést tooa adott fájl webböngésző segítségével?**
    
    Az SAS segítségével is létrehozhat, amelyek érvényesek a megadott időtartam különleges engedélyekkel rendelkező jogkivonatokat. Például létrehozhat egy token tooa fájl csak olvasási hozzáféréssel rendelkező egy adott időn. Bárki, aki rendelkezik az URL-cím el hello fájl közvetlenül bármely webböngészővel érvényes állapotában. Az SAS-kulcsok egyszerűen létrehozhatóak például olyan felhasználói felületekkel, mint a Storage Explorer.

* **Q. Ennyi az egész lehetséges toospecify csak olvasható, vagy csak írási engedélyeket hello megosztáson belüli mappákhoz?**
    
    Ez a szint engedélyek ellenőrzését nincs Ha hello fájlmegosztást SMB-protokollal csatlakoztatja. Azonban érhet el ezt a közös hozzáférésű jogosultságkód (SAS) keresztül hello REST API vagy klienskódtárak létrehozásával.  

* **Q. Hogyan engedélyezhetem a kiszolgálóoldali titkosítás az Azure File storage?**

    [Kiszolgálóoldali titkosítás](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) az Azure File storage általánosan elérhető régiók és a nyilvános és a nemzeti. Az Azure File storage segítségével engedélyezheti az SSE [Azure-portálon](https://portal.azure.com/),[Microsoft Azure Storage erőforrás szolgáltató API](/rest/api/storagerp/storageaccounts), [Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) vagy [Azure parancssori felület ](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    
    Miután engedélyezte a SSE az Azure File storage, toohello a file storage írt storage-fiókhoz tartozó új adatokat automatikusan titkosítva lesznek. Ez a szolgáltatás minden új adatokat tooexisting vagy új megosztások egy meglévő vagy új tárfiók érhető el. A szolgáltatás díjmentesen engedélyezhető. A további [hogyan tooenable SSE az Azure File storage](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **Q. Azure File Storage támogatja az Active Directory-alapú hitelesítés?**
   
    Jelenleg nem támogatjuk az AD-alapú hitelesítést vagy az ACL-eket, de a támogatás már szerepel a funkciókra vonatkozó kérések listáján. Most hello Azure tárfiókkulcsok használt tooprovide hitelesítési toohello fájlmegosztás. A REST API vagy hello hello ügyfél függvénytárainak keresztül használja a közös hozzáférésű jogosultságkód (SAS) megoldás fel. Az SAS segítségével is létrehozhat, amelyek érvényesek a megadott időtartam különleges engedélyekkel rendelkező jogkivonatokat. Például a csak olvasási hozzáféréssel tooa 10 perc lejárati fájl megadott jogkivonat is létrehozhat. Bárki, aki rendelkezik a token érvényességi ideje alatt azt vannak a csak olvasási hozzáféréssel toothat fájl 10 percben.
   
    SAS csak támogatott keresztül hello REST API-n vagy szalagtárak. Ha csatlakoztatja a fájlmegosztást hello hello SMB protokollon keresztül, a biztonsági Társítások toodelegate hozzáférés tooits tartalma nem használható. 
    
* **Q. Mik azok a hello adatok megfelelőségi szabályzatok az Azure File storage támogatott?**

   Az Azure File Storage futtatja a hello azonos tároló-architektúra egyéb tárolási szolgáltatások az Azure Storage és hello alkalmazza ugyanazt az megfelelőségi házirendek. További információ az Azure Storage adatok megfelelőségi letöltheti, és tekintse meg a túl[Microsoft Azure Data Protection dokumentum](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Helyszíni hozzáférés

* **Q.Do toouse Azure ExpressRoute tooconnect tooAzure File storage egy helyszíni virtuális Gépre van?**
   
    Nem. Ha nem rendelkezik ExpressRoute, továbbra is elérheti hello fájlmegosztáshoz a helyszíni mindaddig, amíg még a 445-ös porton (TCP, kimenő) az internethez. Azonban használhatja ExpressRoute az Azure File storage Ha szeretné.

* **Q. Hogyan lehet egy Azure fájlmegosztás csatlakoztatása a helyi számítógépen?** 
    
    Mindaddig, amíg port 445 (TCP, kimenő) meg nyitva, és az ügyfél hello SMB 3.0-s protokollt támogatja, akkor is csatlakoztathatja hello fájlmegosztás hello SMB protokollon keresztül (például használata a Windows 10 vagy Windows Server 2012-ben). A helyi Internetszolgáltatói szolgáltató toounblock hello port működik. Hello ideiglenes, megtekintheti a fájlokat a [Tártallózó](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Számlázási és az árképzés terén
* **Q. Egy Azure virtuális gép és fájlmegosztás fájlszám közötti hálózati forgalom nem hello toohello előfizetés felszámított külső sávszélességnek minősül?**
   
    Ha hello fájlmegosztás és a virtuális gép hello azonos Azure-régiót hello adatforgalom ingyenes. Ha különböző régiókban, hello adatforgalom külső sávszélességnek minősül, fogjuk számlázni.

## <a name="backup"></a>Biztonsági mentés

* **Q. Hogyan biztonsági mentését a saját Azure fájlmegosztás?**
    
    A 3. fél biztonsági mentési eszköz, amely egy csatlakoztatott fájlmegosztást készíthet biztonsági másolatot, vagy használja az AzCopy, RoboCopy. A jövőben; közelében hello várhatóan toohave hello képességét tootake pillanatképek fájlmegosztások képes toouse lesz a szolgáltatás toobackup az Azure fájlmegosztás.

## <a name="performance"></a>Teljesítmény

* **Q. Az Azure File storage hello skálázási korlátai**
    Az Azure File storage méretezhetőségi és Teljesítménycélok információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **Q. A teljesítmény nem volt kielégítő, az Azure File storage toounzip fájlok közben. Mit tegyek?**
    
    tootransfer nagyszámú fájlt az Azure File storage, azt javasoljuk, hogy használható AzCopy (Windows, Linux/UNIX előzetes verzió) vagy az Azure Powershell ezen eszközök hálózati átviteli lettek optimalizálva.

* **Q. Mi volt az javítások, amely a toofix teljesítményével probléma az Azure File storage?**
    
    hello Windows csapata nemrégiben megjelentetett egy javítást toofix a teljesítményromlást, amikor hello ügyfél hozzáfér a Azure File storage a Windows 8.1 vagy Windows Server 2012 R2. További információkért adja hello kivételt a Tudásbázis következő cikkét, kapcsolódó [csökkentheti a teljesítményt, amikor Azure File storage fér hozzá a Windows 8.1 vagy Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Szolgáltatások és egyéb szolgáltatások együttműködés
* **Q. Egy "tanúsító fájlmegosztása" feladatátvevő fürt egyik hello használati esetekben az Azure File storage?**
   
    Ez jelenleg nem támogatott.

* **Q. Van-e a REST API hello átnevezési művelettel?**
   
    Jelenleg nem.

* **Q. Létrehozható beágyazott megosztás, azaz megosztás egy megosztásban?**
    
    Nem. hello fájlmegosztás hello virtuális illesztőprogramot, amelyet csatlakoztathat, így nem támogatottak a beágyazott megosztás.

* **Q. Az Azure File storage IBM MQ-val**
    
    IBM kiadott egy dokumentum tooguide IBM MQ ügyfelei, amikor Azure File storage konfigurálásához az általuk használt szolgáltatással. További információkért vegye ki [hogyan toosetup IBM MQ Multi instance várólista-kezelő Microsoft Azure fájl szolgáltatással](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).


## <a name="troubleshooting"></a>Hibaelhárítás
* **Q. Hogyan hibáinak elhárítása az Azure File storage hibák?**
    
    Olvassa el a túl[Azure File storage hibaelhárítási cikk](storage-troubleshoot-windows-file-connection-problems.md) végpont hibaelhárítási útmutatót. 


## <a name="see-also"></a>Lásd még:
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hogyan toouse Azure File storage Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>File Storage-eszköztámogatás
* [Hogyan toouse Microsoft Azure Storage AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Az Azure Storage hello Azure parancssori felület használatával](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Az Azure File Storage-problémák hibaelhárítása](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Áttelepítési adatok tooAzure fájltároló](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referencia
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
