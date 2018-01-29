---
title: "Tárfiókok létrehozása, kezelése vagy törlése az Azure Portalon | Microsoft Docs"
description: "Az Azure Portalon létrehozhat egy új tárfiókot, kezelheti a fiók hozzáférési kulcsait, vagy törölheti a tárfiókokat. Ismerje meg a standard és a prémium szintű tárfiókokat."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
f1_keywords: sql13.swb.windowsazurestorage.connect.f1
ms.date: 10/11/2017
ms.author: tamram
ms.openlocfilehash: dde2ec3b68f5951e268c32b1c6551641f22a0511
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/19/2017
---
# <a name="about-azure-storage-accounts"></a>Tudnivalók az Azure Storage-fiókokról

[!INCLUDE [storage-selector-portal-create-storage-account](../../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Áttekintés
Az Azure Storage-fiók egy egyedi névteret biztosít az Azure Storage-adatobjektumok tárolásához és hozzáféréséhez. A tárfiókban lévő összes objektum számlázása együtt, csoportosan történik. Alapértelmezés szerint a fiókban tárolt adatok csak Ön, azaz a fiók tulajdonosa számára érhetőek el.

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Tárfiókok számlázása

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

> [!NOTE]
> Azure virtuális gép létrehozásakor a rendszer automatikusan létrehoz egy tárfiókot az Ön számára a telepítés helyén, ha nem rendelkezik még tárfiókkal azon a helyen. Így nem szükséges a tárfiók létrehozásának alábbi lépéseit végrehajtania a virtuális gép lemezeihez. A tárfiók neve a virtuális gép nevén alapul. További részletek: [Azure Virtual Machines – dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/).
> 
> 

## <a name="storage-account-endpoints"></a>Tárfiókvégpontok
Az Azure Storage szolgáltatásban tárolt minden egyes objektum egyedi URL-címmel rendelkezik. A tárfiók neve képezi a cím altartományát. Az altartomány és a tartomány a szolgáltatásra jellemző neve együtt alkotnak egy *végpontot* a tárfiók számára.

Ha például a tárfiók neve *mystorageaccount*, a tárfiókhoz tartozó alapértelmezett végpontok a következők:

* Blob szolgáltatás: http://*mystorageaccount*.blob.core.windows.net
* Table Service: http://*mystorageaccount*.table.core.windows.net
* Queue szolgáltatás: http://*mystorageaccount*.queue.core.windows.net
* File szolgáltatás: http://*mystorageaccount*.file.core.windows.net

> [!NOTE]
> A Blob Storage-fiók csak a Blob szolgáltatásvégpontot mutatja meg.
> 
> 

A tárfiókban lévő objektumok eléréséhez szükséges URL-cím az objektum tárfiókbeli helyének a végponthoz való hozzáfűzésével épül fel. Például egy blobcím formátuma lehet a következő: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Emellett konfigurálhat egy egyéni tartománynevet a tárfiókkal való használatra. További információkért lásd: [Configure a custom domain Name for your Blob Storage Endpoint](../blobs/storage-custom-domain-name.md) (Egyéni tartományév konfigurálása a Blob Storage-végponthoz). A konfigurálást emellett a PowerShell használatával is végezheti. További információ: [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) parancsmag.  


## <a name="create-a-storage-account"></a>Create a storage account
1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Az Azure Portalon nyissa ki bal oldalon a szolgáltatásmenüt, és válassza a **További szolgáltatások** lehetőséget. Ezután görgessen le a **Storage** szakaszig, és válassza a **Storage-fiókok** lehetőséget. A megjelenő **Storage-fiókok** ablakban válassza a **Hozzáadás** lehetőséget.
3. Adja meg a tárfiók nevét. Lásd a [Tárfiókvégpontok](#storage-account-endpoints) című részt azzal kapcsolatban, hogy a rendszer hogyan használja a tárfiók nevét az objektumok címzéséhez az Azure Storage-ban.
   
   > [!NOTE]
   > A tárfiókok neve 3–24 karakter hosszúságú lehet, és csak számokból és kisbetűkből állhat.
   > 
   > A tárfiók nevének egyedinek kell lennie az Azure rendszerben. Az Azure Portal jelzi, ha a választott tárfióknév már használatban van.
   > 
   > 
4. Adja meg a használni kívánt telepítési modellt: **Resource Manager** vagy **Klasszikus**. Az ajánlott telepítési modell a **Resource Manager**. További információ: [Understanding Resource Manager deployment and classic deployment](../../azure-resource-manager/resource-manager-deployment-model.md) (A Resource Manager-telepítés és a klasszikus telepítés ismertetése).
   
   > [!NOTE]
   > Blob Storage-fiókok kizárólag a Resource Manager telepítési modell segítségével hozhatók létre.

5. Válassza ki a tárfiók típusát: **Általános célú** vagy **Blob Storage-fiók**. Az **Általános célú** típus az alapértelmezett.
   
    Ha az **Általános célú** lehetőséget választotta, adja meg a teljesítményszintet: **Standard** vagy **Prémium**. Az alapértelmezett beállítás a **Standard**. A standard és a prémium szintű tárfiókokkal kapcsolatos további információ: [A Microsoft Azure Storage bemutatása](storage-introduction.md) és [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../virtual-machines/windows/premium-storage.md) (Premium Storage: Nagy teljesítményű tárterület az Azure virtuális gépek számítási feladataihoz).
   
    Ha a **Blob Storage** volt kiválasztva, adja meg a hozzáférési szintet: **gyakran használt adatok** vagy **ritkán használt adatok**. Az alapértelmezett réteg a **gyakran használt adatok**. További részletek: [Azure Blob Storage: Cool and Hot tiers](../blobs/storage-blob-storage-tiers.md) (Azure Blob Storage: „ritkán használt adatok” és „gyakran használt adatok” hozzáférési szintek).
6. Válassza ki a tárfiók replikációs beállítását: **LRS**, **GRS**, **RA-GRS** vagy **ZRS**. Az alapértelmezett beállítás az **RA-GRS**. Az Azure Storage replikálási beállításaival kapcsolatos további részletek: [Azure Storage replication](storage-redundancy.md) (Az Azure Storage replikációja).
7. Válassza ki az előfizetést, amelyikben az új tárfiókot létre szeretné hozni.
8. Adjon meg egy új erőforráscsoportot, vagy válasszon ki egy meglévőt. További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md).
9. Válassza ki a tárfiók földrajzi helyét. Tekintse meg [Az Azure régiói](https://azure.microsoft.com/regions/#services) című lapot azzal kapcsolatban, hogy melyik régióban mely szolgáltatások érhetők el.
10. Kattintson a **Létrehozás** parancsra a tárfiók létrehozásához.

## <a name="manage-your-storage-account"></a>A tárfiók kezelése
### <a name="change-your-account-configuration"></a>A fiókkonfiguráció módosítása
A tárfiók létrehozása után módosíthatja annak konfigurációját, például a fiókhoz használt replikációs beállítást, vagy módosíthatja a Blob Storage-fiókhoz tartozó hozzáférési szintet. Az [Azure Portalon](https://portal.azure.com) lépjen a tárfiókhoz, keresse meg, és kattintson a **Konfiguráció** elemre a **BEÁLLÍTÁSOK** területen a fiók konfigurációjának megtekintéséhez és/vagy módosításához.

> [!NOTE]
> A tárfiók létrehozásánál kiválasztott teljesítményszint függvényében előfordulhat, hogy bizonyos replikációs beállítások nem lesznek elérhetők.
> 
> 

A replikációs beállítás módosításával együtt a díjszabás is módosul. További részletek: [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).

Blob Storage-fiókok esetén a hozzáférési szint módosítása a díjszabás módosításán felül további költségekkel járhat. További részletek: [Blob Storage-fiókok – Árak és számlázás](storage-account-options.md#pricing-and-billing).

### <a name="manage-your-storage-access-keys"></a>A tárelérési kulcsok kezelése
Tárfiók létrehozásakor az Azure létrehoz két 512 bites tárelérési kulcsot, amelyek a hitelesítéshez használhatóak a tárfiók elérésekor. Azáltal, hogy az Azure két tárelérési kulcsot biztosít, lehetővé teszi a kulcsok újragenerálását anélkül, hogy megszakadna a társzolgáltatás vagy az ahhoz való hozzáférés.

> [!NOTE]
> Javasoljuk, hogy a tárelérési kulcsokat ne ossza meg senkivel. Ha a tárolási erőforrásokhoz az elérési kulcsok kiadása nélkül szeretné engedélyezni a hozzáférést, használhat *közös hozzáférésű jogosultságkódot*. A közös hozzáférésű jogosultságkód hozzáférést biztosít a fiókban lévő egyes erőforrásokhoz az Ön által meghatározott időtartamra és engedélyekkel. További információk: [Using Shared Access Signatures (SAS) (Közös hozzáférésű jogosultságkód (SAS) használata)](storage-dotnet-shared-access-signature-part-1.md)
> 
> 
<a id="view-and-copy-storage-access-keys"/></a>
#### <a name="view-and-copy-storage-access-keys"></a>A tárelérési kulcsok megtekintése és másolása
Az [Azure Portalon](https://portal.azure.com) lépjen a tárfiókhoz, kattintson **Az összes beállítás** elemre, majd kattintson az **Elérési kulcsok** elemre a tárelérési kulcsok megtekintéséhez, másolásához és újragenerálásához. Az **Elérési kulcsok** panel is tartalmaz az elsődleges és másodlagos kulcsok segítségével előre konfigurált kapcsolati karakterláncokat, amelyeket az alkalmazásokban történő használat céljából másolhat.

#### <a name="regenerate-storage-access-keys"></a>Tárelérési kulcsok újragenerálása
Javasoljuk, hogy rendszeres időközönként cserélje le tárfiókja elérési kulcsait tárolási kapcsolatai biztonságának megőrzése érdekében. A rendszer két tárelérési kulcsot biztosít, így az egyik kulcs újragenerálása esetén a másikkal közben fenntarthatja a tárfiók kapcsolatait.

> [!WARNING]
> A tárelérési kulcsok újragenerálása hatással lehet a szolgáltatásokra az Azure-ban, valamint a tárfióktól függő saját alkalmazásaira is. A tárfiók eléréséhez a tárelérési kulcsot használó összes ügyfelet frissíteni kell, hogy az új kulcsot használják.
> 
> 

**Médiaszolgáltatások** – Ha rendelkezik a tárfióktól függő médiaszolgáltatásokkal, újra kell szinkronizálnia a tárelérési kulcsokat a médiaszolgáltatással, miután újragenerálta azokat.

**Alkalmazások** – Ha a tárfiókot használó webalkalmazásokkal vagy felhőszolgáltatásokkal rendelkezik, a kulcsok újragenerálásával elveszti a kapcsolatokat, kivéve, ha rotálja a kulcsokat.

**Tártallózók** – Ha bármilyen [tártallózó alkalmazást](storage-explorers.md) használ, valószínűleg frissítenie kell az alkalmazások által használt tárkulcsot.

A tárelérési kulcsok az alábbi folyamat végrehajtásával rotálhatóak:

1. Frissítse a kapcsolati karakterláncokat az alkalmazás kódjában, hogy a tárfiók másodlagos elérési kulcsára hivatkozzanak.
2. Generálja újra a tárfiók elsődleges elérési kulcsát. A **Tárelérési kulcsok** panelen kattintson az **1. kulcs újragenerálása** elemre, majd kattintson az **Igen** gombra annak megerősítéséhez, hogy egy új kulcsot kíván létrehozni.
3. Frissítse a kapcsolati karakterláncokat a kódban, hogy az új elsődleges tárelérési kulcsra hivatkozzanak.
4. Hasonló módon generálja újra a másodlagos elérési kulcsot.

## <a name="delete-a-storage-account"></a>Tárfiók törlése
A már nem használt tárfiókok eltávolításához lépjen az [Azure Portalra](https://portal.azure.com), és kattintson a **Törlés** gombra. A tárfiókkal együtt törlődik a teljes fiók, beleértve az abban lévő összes adatot is.

> [!WARNING]
> A törölt tárfiókokat nem lehet visszaállítani, és nem lehet a törlés előtt abban tárolt tartalmakat helyreállítani. A fiók törlése előtt készítsen biztonsági másolatot minden menteni kívánt tartalomról. Ez igaz a fiókban lévő összes erőforrásra is – ha töröl egy blobot, táblát, üzenetsort vagy fájlt, az véglegesen törölve lesz.
> 

Ha egy Azure virtuális géppel társított tárfiókot próbál törölni, egy hibaüzenetet kaphat, amely szerint a tárfiók még használatban van. A hiba elhárításával kapcsolatban lásd: [Troubleshoot errors when you delete storage accounts](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md) (A tárfiókok törlésével kapcsolatos hibák elhárítása).

## <a name="next-steps"></a>Következő lépések
* A [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.
* [Azure Blob Storage: Cool and Hot tiers (Azure Blob Storage: „ritkán használt adatok” és „gyakran használt adatok” hozzáférési szintek)](../blobs/storage-blob-storage-tiers.md)
* [Azure Storage replication (Azure Storage replikáció)](storage-redundancy.md)
* [Configure Azure Storage connection strings (Az Azure Storage kapcsolati karakterláncok konfigurálása)](../storage-configure-connection-string.md)
* [Adatátvitel az AzCopy parancssori segédprogrammal](storage-use-azcopy.md)
* Látogasson el [az Azure Storage csapat blogjára](http://blogs.msdn.com/b/windowsazurestorage/).

