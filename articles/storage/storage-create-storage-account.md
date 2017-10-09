---
title: "aaaHow toocreate, kezelése vagy törlése az Azure-portálon hello |} Microsoft Docs"
description: "Hozzon létre egy új tárfiókot, a tárelérési kulcsok kezelése vagy törlése a hello Azure-portálon. Ismerje meg a standard és a prémium szintű tárfiókokat."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
f1_keywords: sql13.swb.windowsazurestorage.connect.f1
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: c11c6509e192170db4812f47c389fc1009b94daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Tudnivalók az Azure Storage-fiókokról
[!INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Áttekintés
Egy Azure storage-fiók egy egyedi névtér toostore biztosít, és az Azure Storage-adatobjektumok eléréséhez. A tárfiókban lévő összes objektum számlázása együtt, csoportosan történik. Alapértelmezés szerint hello a fiók adatai elérhető csak tooyou, hello fiók tulajdonosának.

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Tárfiókok számlázása
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [!NOTE]
> Azure virtuális gép létrehozásakor a storage-fiók automatikusan létrejön, meg hello telepítési helyen Ha még nem rendelkezik egy tárfiókot az adott helyre. Így nem szükséges toofollow hello lépéseket alatti toocreate egy tárfiók a virtuális gépek lemezeit. a tárfiók neve hello hello virtuális gép neve alapul. Lásd: hello [Azure Virtual Machines – dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/) további részleteket.
> 
> 

## <a name="storage-account-endpoints"></a>Tárfiókvégpontok
Az Azure Storage szolgáltatásban tárolt minden egyes objektum egyedi URL-címmel rendelkezik. hello tárolási fiók neve űrlapok hello cím altartományát. hello altartomány és a tartomány nevét, amely egy adott tooeach szolgáltatás, kombinációja képezi egy *végpont* a tárfiók.

Például, ha a tárfiók neve *mystorageaccount*, akkor a tárfiók hello alapértelmezett végpontok a következők:

* Blob szolgáltatás: http://*mystorageaccount*.blob.core.windows.net
* Table Service: http://*mystorageaccount*.table.core.windows.net
* Queue szolgáltatás: http://*mystorageaccount*.queue.core.windows.net
* File szolgáltatás: http://*mystorageaccount*.file.core.windows.net

> [!NOTE]
> A Blob storage-fiók csak hello Blob-szolgáltatásvégpont tesz elérhetővé.
> 
> 

a tárfiók eléréséhez szükséges hello URL hozzáfűzésével épül fel hello tárolási fiók toohello végpont hello objektum helyét. Például egy blobcím formátuma lehet a következő: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

A tárfiók egy egyéni tartomány nevét toouse is konfigurálható. A klasszikus tárfiókokkal kapcsolatos részletekért lásd: [Configure a custom domain Name for your Blob Storage Endpoint](storage-custom-domain-name.md) (Egyéni tartományév konfigurálása a Blob Storage-végponthoz). Erőforrás-kezelő storage-fiókok, ez a funkció még nem lett hozzáadva toohello [Azure-portálon](https://portal.azure.com) még, de a PowerShell segítségével konfigurálhatja. További információkért lásd: hello [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) parancsmag.  

## <a name="create-a-storage-account"></a>Create a storage account
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello központ menüben válassza ki a **új** -> **tárolási** -> **tárfiók**.
3. Adja meg a tárfiók nevét. Lásd: [tárfiókvégpontok](#storage-account-endpoints) a vonatkozó részletek hogyan hello tárfióknév használt tooaddress lesz-e az Azure Storage az objektumok.
   
   > [!NOTE]
   > A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.
   > 
   > A tárfiók nevének egyedinek kell lennie az Azure rendszerben. hello Azure-portál jelzi, ha a választ hello tárfióknév már használatban van.
   > 
   > 
4. Adja meg a hello telepítési modell toobe használt: **erőforrás-kezelő** vagy **klasszikus**. **Erőforrás-kezelő** hello javasolt üzembe helyezési modellben. További információ: [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) (A Resource Manager-telepítés és a klasszikus telepítés ismertetése).
   
   > [!NOTE]
   > BLOB storage-fiókok csak hello Resource Manager telepítési modell segítségével hozhatók létre.
   > 
   > 
5. Válassza ki a tárfiók típusát hello: **általános célú** vagy **Blob-tároló**. **Általános célú** hello alapértelmezett.
   
    Ha **általános célú** kiválasztva, majd adja meg a hello teljesítményszinttel: **szabványos** vagy **prémium**. hello alapértelmezett érték a **szabványos**. A standard és prémium szintű storage-fiókok további részletekért lásd: [Azure Storage bemutatása tooMicrosoft](storage-introduction.md) és [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](storage-premium-storage.md).
   
    Ha **Blob Storage** kiválasztva, majd adja meg a hello hozzáférési réteget: **gyakran használt adatok** vagy **lassú**. hello alapértelmezett érték a **gyakran használt adatok**. További részletek: [Azure Blob Storage: Cool and Hot tiers](storage-blob-storage-tiers.md) (Azure Blob Storage: „ritkán használt adatok” és „gyakran használt adatok” hozzáférési szintek).
6. A beállításnak a hello replikációs hello tárfiók: **LRS**, **Georedundáns**, **RA-GRS**, vagy **ZRS**. hello alapértelmezett érték a **RA-GRS**. Az Azure Storage replikálási beállításaival kapcsolatos további részletek: [Azure Storage replication](storage-redundancy.md) (Az Azure Storage replikációja).
7. Válassza ki a kívánt toocreate hello új tárfiók hello előfizetést.
8. Adjon meg egy új erőforráscsoportot, vagy válasszon ki egy meglévőt. További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
9. Válassza ki a tárfiók hello földrajzi helyét. Tekintse meg [Az Azure régiói](https://azure.microsoft.com/regions/#services) című lapot azzal kapcsolatban, hogy melyik régióban mely szolgáltatások érhetők el.
10. Kattintson a **létrehozása** toocreate hello tárfiók.

## <a name="manage-your-storage-account"></a>A tárfiók kezelése
### <a name="change-your-account-configuration"></a>A fiókkonfiguráció módosítása
A tárfiók létrehozása után módosíthatja annak konfigurációját, például hello fióknak vagy a Blob storage-fiók módosítása hello hozzáférési szint használt hello replikációs beállítás módosításával. A hello [Azure-portálon](https://portal.azure.com), keresse meg a tárfiók tooyour, a Keresés és a kattintson **konfigurációs** alatt **beállítások** tooview és/vagy módosítsa a hello konfigurációs.

> [!NOTE]
> Hello tárfiók létrehozásánál kiválasztott hello teljesítményszinttel, attól függően, hogy bizonyos replikációs beállítások nem érhetők el.
> 
> 

Hello replikációs beállítás módosítása a díjszabás változik. További részletek: [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).

A BLOB storage-fiókok hello hozzáférési réteg módosítása járhat hello költségekkel továbbá toochanging a díjszabás is módosul. Tekintse meg a hello [Blob storage-fiókok – árak és számlázás](storage-blob-storage-tiers.md#pricing-and-billing) további részleteket.

### <a name="manage-your-storage-access-keys"></a>A tárelérési kulcsok kezelése
Amikor létrehoz egy tárfiókot, az Azure létrehoz két 512 bites tárelérési kulcsot, a hitelesítéshez használt hello tárfiók elérésekor. Két tárelérési kulcsot biztosít, Azure lehetővé teszi tooregenerate hello kulcsok megszakítás tooyour társzolgáltatás és toothat szolgáltatás nélkül.

> [!NOTE]
> Javasoljuk, hogy a tárelérési kulcsokat ne ossza meg senkivel. toopermit toostorage erőforrások eléréséhez a tárelérési kulcsok kiadása nélkül, használhatja a *közös hozzáférésű jogosultságkódot*. A közös hozzáférésű jogosultságkód hozzáférést tooa erőforrás fiókjában biztosít, Ön által meghatározott időtartamra és hello jogosultságokkal rendelkező. További információk: [Using Shared Access Signatures (SAS) (Közös hozzáférésű jogosultságkód (SAS) használata)](storage-dotnet-shared-access-signature-part-1.md)
> 
> 
<a id="view-and-copy-storage-access-keys"/></a>
#### <a name="view-and-copy-storage-access-keys"></a>A tárelérési kulcsok megtekintése és másolása
A hello [Azure-portálon](https://portal.azure.com)tooyour tárfiók lépjen, kattintson **összes beállítás** majd **hívóbetűk** tooview, másolása és a tárelérési kulcsok újragenerálása. Hello **hívóbetűk** panel is tartalmaz, hogy toouse másolhatja az alkalmazásokban az elsődleges és másodlagos kulcsok segítségével előre konfigurált kapcsolati karakterláncokat.

#### <a name="regenerate-storage-access-keys"></a>Tárelérési kulcsok újragenerálása
Azt javasoljuk, hogy módosítsa a hello hívóbetűk tooyour tárolási fiók rendszeresen toohelp garantálják a tárolási kapcsolatok biztonságát. Két tárelérési kulcsok vannak hozzárendelve, így akkor is fenntartható a kapcsolatok toohello tárfiók hello újragenerálja egyik kulcs segítségével más hozzáférési kulcsot.

> [!WARNING]
> A tárelérési kulcsok újragenerálása hatással lehet az Azure, valamint a saját alkalmazásai hello tárfiók függő szolgáltatások. Hello hozzáférési kulcs tooaccess hello tárfiókot használó összes ügyfelet frissített toouse hello új kulcsot kell lennie.
> 
> 

**A Media services** -Ha a tárfiók függő media services, újra kell szinkronizálnia hello hívóbetűk a médiaszolgáltatással után hello kulcsok újragenerálása.

**Alkalmazások** – Ha a webes alkalmazások cloud services – hello tárolási fiók használatát, vagy megszakad a hello kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat.

**Tártallózók** – Ha bármilyen [Tártallózó alkalmazást](storage-explorers.md), valószínűleg szüksége lesz az alkalmazások által használt kulcs tooupdate hello.

Hello folyamat végrehajtásával rotálhatóak a tárelérési kulcsok itt található:

1. Hello kapcsolati karakterláncokat az alkalmazás kódja tooreference hello másodlagos elérési kulcsát hello storage-fiókjának frissítése.
2. A tárfiók hello elsődleges elérési kulcs újragenerálása. A hello **hívóbetűk** panelen kattintson **újragenerálja Key1**, és kattintson a **Igen** , amelyet az új kulcs toogenerate tooconfirm.
3. Hello kapcsolati karakterláncok a kód tooreference hello új elsődleges elérési kulcs frissítése.
4. Kulcs újragenerálása hello másodlagos hozzáférés a hello azonos módon.

## <a name="delete-a-storage-account"></a>Tárfiók törlése
tooremove egy tárfiókot, amely már nem használ, keresse meg a storage-fiókot toohello hello [Azure-portálon](https://portal.azure.com), és kattintson a **törlése**. A tárfiók törlése hello teljes fiók, beleértve az összes adat hello fiók törlése.

> [!WARNING]
> Nem lehetséges toorestore törölt tárfiókokat, vagy a törlés előtt abban tárolt hello tartalmakat bármelyikét beolvasása. Bármi is meg arról, hogy tooback kívánt toosave hello fiók törlése előtt. Ez is igaznak minden olyan erőforrásnál hello fiók – Ha töröl egy blob, table, várólista vagy fájlt, az véglegesen törlődni fog.
> 
> 

a storage-fiók egy Azure virtuális géphez társított toodelete, akkor előbb ellenőrizze, hogy a virtuális gép lemezei törölve lett. Ha először nem törli a virtuális gépek lemezeit, majd tett kísérlet során toodelete a tárfiók jelenik meg egy ehhez hasonló hibaüzenetet küld:

    Failed toodelete storage account <vm-storage-account-name>. Unable toodelete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Ha hello tárfiók hello klasszikus telepítési modellt használ, eltávolíthatja hello az alábbi lépéseket követve hello virtuálisgép-lemez [Azure-portálon](https://manage.windowsazure.com):

1. Keresse meg a toohello [klasszikus Azure portálon](https://manage.windowsazure.com).
2. Keresse meg a toohello virtuális gépek lapra.
3. Kattintson a hello lemezek fülre.
4. Válassza ki az adatlemezt, majd kattintson a Lemez törlése gombra.
5. toodelete lemezképeket, keresse meg a toohello lemezképek lapra, és törölje az összes hello fiókban tárolt lemezképet.

További információkért lásd: hello [Azure Virtual machines – dokumentáció](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Azure Blob Storage: Cool and Hot tiers (Azure Blob Storage: „ritkán használt adatok” és „gyakran használt adatok” hozzáférési szintek)](storage-blob-storage-tiers.md)
* [Azure Storage replication (Azure Storage replikáció)](storage-redundancy.md)
* [Configure Azure Storage connection strings (Az Azure Storage kapcsolati karakterláncok konfigurálása)](storage-configure-connection-string.md)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)
* A Microsoft hello [Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/).

