---
title: "Storage szolgáltatás titkosítási az inaktív adatok aaaAzure |} Microsoft Docs"
description: "Használjon hello Azure Storage szolgáltatás titkosítási szolgáltatás tooencrypt az Azure Blob Storage hello szolgáltatás oldalán hello adatok tárolására, és a visszafejtésre hello adatok beolvasása közben."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: robinsh
ms.openlocfilehash: 4e03c5704071281a798936d41d86456afcfdec77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Az Azure Storage szolgáltatás inaktívadat-titkosítása
Az Azure Storage szolgáltatás titkosítási (SSE) inaktív adatok segítségével és az adatok toomeet megvédeni a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel. Ez a szolgáltatás Azure Storage automatikusan titkosítja az adatokat a korábbi toopersisting toostorage és előzetes tooretrieval visszafejti. hello titkosítási, visszafejtési és kulcskezelés az teljesen átlátható toousers.

hello alábbiakban ad részletes útmutatást hogyan toouse hello Storage szolgáltatás titkosítási szolgáltatásokat, valamint a hello támogatott forgatókönyvek és felhasználói feladatait.

## <a name="overview"></a>Áttekintés
Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja. Adatok védve legyenek az alkalmazás és az Azure közötti átvitel során használatával [ügyféloldali titkosítás](../storage-client-side-encryption.md), HTTPs és SMB 3.0-s. Storage szolgáltatás titkosítási titkosítását, kezelési titkosítási, visszafejtési és kulcskezelés teljesen átlátható módon biztosít. Összes adat titkosítva van, 256 bites [AES titkosítási](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), hello legerősebb blokk egyik Rejtjelek érhető el.

SSE tooAzure tárolási írása, és az Azure Blob Storage és File Storage használható hello adatok titkosításával működik. Hello következő működik:

* Standard szintű Storage: Általános célú tárfiókok a Blobok és a tárolás és a Blob storage-fiókok
* Prémium szintű Storage 
* Minden redundancia szintek (LRS, zrs-t, Georedundáns, RA-GRS)
* Az Azure Resource Manager tárfiókok (de nem klasszikus) 
* Minden egyes.

toolearn több, tekintse meg az toohello gyakran ismételt kérdések.

tooenable vagy tiltsa le a Storage szolgáltatás titkosítási egy tárfiók bejelentkezni hello [Azure-portálon](https://portal.azure.com) , és válasszon egy tárfiókot. A hello-beállítások panelen keresse meg a Blob szolgáltatás szakasz hello ezen a képernyőfelvételen látható módon, és kattintson a titkosítás.

![A titkosítási beállítással portál ábrázoló képernyőfelvétel](./media/storage-service-encryption/image1.png)
<br/>*1. ábra: SSE engedélyezése a Blob szolgáltatás (1. lépés)*

![A titkosítási beállítással portál ábrázoló képernyőfelvétel](./media/storage-service-encryption/image3.png)
<br/>*2. ábra: (1. lépés) Fájlszolgáltatás SSE engedélyezése*

Hello titkosítási beállítás gombra kattintva engedélyezheti vagy letilthatja a Storage szolgáltatás titkosítási.

![Képernyőfelvétel: portál-titkosítás tulajdonságai](./media/storage-service-encryption/image2.png)
<br/>*3. ábra: BLOB SSE engedélyezése és a szolgáltatás (2. lépés)*

## <a name="encryption-scenarios"></a>Titkosítási forgatókönyvek
Storage szolgáltatás titkosítási a tárolási fiók szintjén engedélyezhető. Miután engedélyezte őket, az ügyfelek mely szolgáltatások tooencrypt fogja választani. A következő forgatókönyvet hello támogatja:

* A Blob Storage és File Storage erőforrás-kezelő titkosításra.
* Blob és titkosításának Fájlszolgáltatás a klasszikus tárfiókokba egyszer tooResource Manager tárfiókok át.

SSE rendelkezik hello a következő korlátozások vonatkoznak:

* Klasszikus tárfiókokba titkosítása nem támogatott.
* Meglévő adatok - SSE csak titkosítja az újonnan létrehozott adatokat, miután hello titkosítás engedélyezve van. Ha például egy új erőforrás-kezelő storage-fiók létrehozása, de ne kapcsolja be a titkosítás, majd töltse fel a blobok vagy archivált VHD-k toothat tárfiók, és kapcsolja be SSE, be van jelölve, kivéve, ha a rendszeren, vagy másolja ezeket a blobok nem titkosított.
* Piactér támogatási - alapján létrehozott virtuális gépek titkosítási engedélyezése hello hello segítségével piactér [Azure-portálon](https://portal.azure.com), PowerShell és az Azure parancssori felület. hello VHD alapjául szolgáló lemezképhez marad titkosítatlan; azonban bármilyen végre, miután a virtuális gép hello rendelkezik hoz létre írás lesz titkosítva.
* Tábla és a várólisták adatok nem lesznek titkosítva.

## <a name="getting-started"></a>Első lépések
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>1. lépés: [hozzon létre egy új tárfiókot](../storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>2. lépés: Engedélyezze a titkosítást.
Engedélyezheti a titkosítást hello segítségével [Azure-portálon](https://portal.azure.com).

> [!NOTE]
> Ha szeretné, hogy tooprogrammatically engedélyezése, vagy tiltsa le a Storage szolgáltatás titkosítási hello egy tárfiókon, használhatja a hello [Azure Storage erőforrás szolgáltató REST API felülete](https://msdn.microsoft.com/library/azure/mt163683.aspx), hello [Storage erőforrás-szolgáltató ügyféloldali kódtár a .NET-hez](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs), vagy hello [Azure CLI](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-toostorage-account"></a>3. lépés:, Másolja át az toostorage fiókja
Blob szolgáltatás hello SSE engedélyezte, ha bármely BLOB írt toothat tárfiók lesz titkosítva. Csak azok a rendszer újraírja bármely már a tárfiókban található blobok nem lesznek titkosítva. Hello adatokat másolni egy tárolási fiók tooone az SSE titkosított, vagy még akkor is engedélyezheti SSE és hello blobot másolni a több tároló tooanother toosure korábbi adatok titkosítását. Ezzel a következő eszközök tooaccomplish hello bármelyikét. Ez az hello ugyanez a viselkedés a File Storage is.

#### <a name="using-azcopy"></a>AzCopy használatával
AzCopy egy Windows parancssori segédprogram tooand adatok másolása az egyszerű parancsokkal optimális teljesítménnyel Microsoft Azure Blob, a fájl és a Table storage készült. A toocopy is használhat, a blobok vagy egy, az SSE engedélyezve van, egy tárolási fiók tooanother fájlokat. 

toolearn több, látogasson el a [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).

#### <a name="using-smb"></a>Az SMB-
Az Azure File storage hello szabványos SMB protokollt használó hello felhőben fájlmegosztásokat kínál. Egy fájlmegosztást csatlakoztathatnak egy ügyfél a helyszínen vagy az Azure-ban. Ha csatlakoztatva, és eszközöket, például a Robocopy lehet használt toocopy fájlok fájlmegosztások tooAzure keresztül. További információkért lásd: [hogyan toomount Azure fájlmegosztás Windows rendszeren](../files/storage-how-to-use-files-windows.md) és [hogyan toomount Azure-fájl megosztása Linux](../storage-how-to-use-files-linux.md).


#### <a name="using-hello-storage-client-libraries"></a>Hello Storage Ügyfélkódtáraival használatával
Blob vagy a fájl adatainak tooand másolhatja a blob storage vagy a Storage Ügyfélkódtáraival, beleértve a .NET, C++, Java, Android, Node.js, PHP, Python és Ruby széles skáláját alkalmazó tárfiókok között.

toolearn több, látogasson el a [az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>A Tártallózó használatával
Ön egy tárolási explorer toocreate tárfiókok használata, feltöltése és adatok letöltése, megtekintheti BLOB tartalmát, és haladjon végig a könyvtárak. A titkosítás engedélyezve van ezen tooupload blobok tooyour tárfiók egyikét használhatja. Az egyes tártallózók is másolhatja meglévő blob tooa különböző tároló hello tárfiókban lévő adatokat vagy egy új tárfiókot, amely rendelkezik az SSE engedélyezve van.

toolearn több, látogasson el a [Azure Tártallózók](../storage-explorers.md).

### <a name="step-4-query-hello-status-of-hello-encrypted-data"></a>4. lépés: A titkosított adatok hello hello állapotának lekérdezése
Hello Storage ügyfélkódtáraival frissített verziója telepítve van, amely lehetővé teszi egy objektum toodetermine tooquery hello állapotát, ha vagy nincs titkosítva van. Jelenleg ez csak a Blob storage érhető el. A File storage funkció hello terv. 

A hello addig hívása [fiók tulajdonságok beolvasása](https://msdn.microsoft.com/library/azure/mt163553.aspx) , amely a tárfiók hello tooverify engedélyezhető a titkosítás vagy hello hello Azure-portálon a tárfiók tulajdonságai nézet rendelkezik.

## <a name="encryption-and-decryption-workflow"></a>Titkosítás és visszafejtés munkafolyamat
Hello titkosítási/visszafejtési munkafolyamat rövid leírása itt található:

* hello ügyfél lehetővé teszi, hogy a titkosítás hello tárfiók.
* Ha hello ügyfél írja az új adatok (PUT Blob, PUT blokk, PUT lap, PUT fájl stb.) tooBlob vagy a File storage; minden egyes van titkosítva, 256 bites [AES titkosítási](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), hello legerősebb blokk egyik Rejtjelek érhető el.
* Ha hello ügyféligények tooaccess adatokat (a Blob LEKÉRÉSE, stb.), adatai automatikusan visszafejtett toohello felhasználói visszatérése előtt.
* Védelem le van tiltva, ha új írási műveletek többé nem lesznek titkosítva, és a meglévő titkosított adatok titkosítva maradnak, amíg hello felhasználó írni. Titkosítási be van kapcsolva, írja be a tooBlob vagy a File storage titkosítja. az adatok hello állapotát hello felhasználói hello tárfiók titkosítás engedélyezése vagy tiltása váltása nem változik.
* Minden titkosítási kulcs tárolása, titkosított, és a Microsoft által felügyelt.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Gyakori kérdések Storage szolgáltatás titkosítási az inaktív adatok
**K: van egy létező klasszikus storage-fiókot. Engedélyezhető az SSE rajta?**

A: nem SSE csak támogatott erőforrás-kezelő storage-fiókok.

**K: hogyan is titkosítani a klasszikus tárfiókban lévő adatokat?**

V: hozhat létre egy új erőforrás-kezelő tárfiókot és másolja az adatokat használó [AzCopy](storage-use-azcopy.md) az létező klasszikus tárfiókból tooyour az újonnan létrehozott tárfiók erőforrás-kezelő. 

A hagyományos tárolási fiók tooa erőforrás-kezelő tárfiók telepít át, ha ez a művelet azonnali, a fiók típusa hello megváltozik, de nincs hatással a meglévő adatokat. Bármely új adatokat csak a titkosítás engedélyezése után lesz titkosítva. További információkért lásd: [Platform támogatott áttelepítési az IaaS-erőforrásokra a klasszikus tooResource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Vegye figyelembe, hogy ez csak a Blob vagy a fájl szolgáltatás támogatott.

**K: van egy meglévő Resource Manager storage-fiókot. Engedélyezhető az SSE rajta?**

Igen, de csak az újonnan írt adatok A: lesz titkosítva. Lépjen vissza, és nem már meglévő adatok titkosítására. Ez jelenleg nem támogatott hello fájl tárolási Preview a.

**K: szeretném tooencrypt hello aktuális tárfiókban lévő adatokat egy meglévő Resource Manager?**

V: engedélyezheti SSE erőforrás-kezelő tárfiókokban bármikor. Azonban már jelen adatok nem lesznek titkosítva. meglévő adatok tooencrypt, másolja őket tooanother nevét vagy egy másik tárolóban, és távolítsa el a titkosítás nélkül hello verziók.

**K: használom a prémium szintű storage; használható SSE?**

A: SSE Igen, Standard szintű tárolást és a prémium szintű Storage esetén támogatott.  Prémium szintű Storage hello szolgáltatása nem támogatott.

**K:, ha szeretnék hozzon létre egy új tárfiókot SSE engedélyezése, majd hozzon létre egy új virtuális Gépet tároló fiókot használva, ez jelent a virtuális gép titkosítása?**

V: Igen. A létrehozott lemezek, amelyek hello új tárfiók lesz titkosítva, mindaddig, amíg a létrehozásuk után az SSE engedélyezve van. Ha marad, hogy a virtuális gép az Azure piactéren, hello VHD alapjául szolgáló lemezképhez használatával hoztak létre hello titkosítatlan; azonban bármilyen végre, miután a virtuális gép hello rendelkezik hoz létre írás lesz titkosítva.

**K: hozható létre új tárfiókok az SSE engedélyezve van az Azure PowerShell és az Azure parancssori felület használatával?**

V: Igen.

**K: hogyan sokkal nem költségeket, Azure Storage Ha SSE engedélyezve van?**

V: nincs további költség nélkül.

**K: kezelő hello titkosítási kulcsokat?**

A: a Microsoft által felügyelt hello kulcsok.

**K: használhatok saját titkosítási kulcsokat?**

V: jelenleg is dolgozunk ügyfelek toobring képességek biztosítása a saját titkosítási kulcsokat.

**K: hozzáférés toohello titkosítási kulcsok visszavonása?**

V: jelenleg nem; a Microsoft hello kulcsok teljes felügyeletét.

**K: SSE alapértelmezés szerint engedélyezve van, egy új tárfiók létrehozásakor?**

V: SSE; alapértelmezés szerint nincs engedélyezve az Azure portál tooenable hello használhatja azt. Szoftveresen is engedélyezheti ezt a funkciót hello Storage erőforrás-szolgáltató REST API használatával.

**K: hogyan eltér a Azure Disk Encryption?**

V: Ez a funkció az használt tooencrypt adatai az Azure Blob Storage tárolóban. hello Azure Disk Encryption használt tooencrypt operációsrendszer- és adatlemezek IaaS virtuális gépeket. További részletekért tekintse meg a [tárolási biztonsági útmutatója](../storage-security-guide.md).

**K: Mit tegyek, ha engedélyezhetem SSE, és majd keresse meg és engedélyezze az Azure Disk Encryption hello lemezeken?**

A: Ez zökkenőmentesen működnek. Mindkét módszer által az adatok titkosítva lesznek.

**K: a tárfiók földrajzi redundantly replikált toobe be van állítva. Ha az SSE engedélyezéséhez saját redundáns példány is titkosítva lesznek?**

A: Igen hello storage-fiók összes másolatát titkosított, és – helyileg redundáns tárolás (LRS), zóna-redundáns tárolás (ZRS), Georedundáns tárolás (GRS) és írásvédett Georedundáns tárolás (RA-GRS) – összes redundancia beállítások támogatottak.

**K: nem engedélyezhető a titkosítás a storage-fiókom.**

V: az azt egy erőforrás-kezelő tárfiókot? Klasszikus tárfiókok nem támogatottak. 

**K: SSE csak engedélyezett meghatározott régióiba?**

V: hello SSE Blob Storage minden területen érhető el. Ellenőrizze, hogy hello rendelkezésre állással kapcsolatos szakaszának fájlok tárolására. 

**K: hogyan do I kapcsolatfelvételre Ha problémák merülnek fel, vagy visszajelzés tooprovide I?**

A: forduljon a [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pedig problémákkal kapcsolatos tooStorage titkosítását.

## <a name="next-steps"></a>Következő lépések
Az Azure Storage a biztonságos alkalmazások toobuild biztonsági képességeket, amelyek együtt lehetővé teszik a fejlesztők széles választékát nyújtja. További részletekért látogasson el a hello [tárolási biztonsági útmutatója](../storage-security-guide.md).

