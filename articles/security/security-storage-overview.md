---
title: "az Azure Storage használható aaaSecurity szolgáltatások |} Microsoft Docs"
description: " Ez a cikk áttekintést hello Azure biztonsági alapszolgáltatások használható az Azure Storage. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: 663cd2705527957d21ff9475a6322b42a16c95e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-overview"></a>Az Azure storage biztonsági áttekintése
Az Azure Storage egy hello felhőalapú tárolómegoldás modern alkalmazások tartósságot, rendelkezésre állását és méretezhetőségét toomeet hello az ügyfelek igényeinek megfelelően. Az Azure Storage biztonsági funkciók széles választékát nyújtja:

* hello tárfiók szerepköralapú hozzáférés-vezérlés és az Azure Active Directory használatával biztosítható.
* A védett adatok az alkalmazás és az Azure közötti átvitel során ügyféloldali titkosítás, HTTPS és SMB 3.0-s.
* Adatok titkosítása automatikusan megtörténik toobe állítható be Ha a tárolókészletet a Storage szolgáltatás titkosítási tooAzure ír.
* Virtuális gépek által használt operációsrendszer- és adatlemezek titkosítja az Azure Disk Encryption toobe állítható be.
* Delegált hozzáférést toohello adatobjektumainak az Azure Storage a megosztott hozzáférési aláírásokkal használatával engedélyezhetők.
* hello hitelesítési módszerének valaki tároló elérésekor tárolási analitika követhető nyomon.

Az Azure Storage biztonsági részletesebb vizsgálja meg, lásd: hello [Azure Storage biztonsági útmutató](../storage/common/storage-security-guide.md). Ez az útmutató részletes bemutatója a tárfiókok kulcsait, például az Azure Storage hello biztonsági funkciói adattitkosítás átvitel közben, és a rest és a tárolási analitika.

Ez a cikk áttekintést az Azure Storage használható az Azure biztonsági funkciók. Hivatkozásokkal érhető el, amelyek egyes szolgáltatások részleteit biztosítanak, így további tooarticles.

Az alábbiakban a cikkben szereplő hello alapvető szolgáltatások toobe:

* Szerepköralapú hozzáférés-vezérlés
* Delegált hozzáférést toostorage objektumok
* Az átvitel során titkosítás
* Rest/Storage szolgáltatás titkosítási titkosítását
* Azure Disk Encryption
* Azure Key Vault

## <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-vezérlés (RBAC)
A storage-fiókkal, és a szerepköralapú hozzáférés-vezérlést (RBAC) biztonságát. Hello alapján történő hozzáférés [tooknow kell](https://en.wikipedia.org/wiki/Need_to_know) és [legalacsonyabb jogosultsági szint](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági elveket elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket. A következő hozzáférési jogokat kapnak hello megfelelő RBAC szerepkör toogroups és alkalmazások egy adott hatókörhöz hozzárendelésével. Használhat [beépített RBAC-szerepkörök](../active-directory/role-based-access-built-in-roles.md), például a tárolási fiók közreműködői, tooassign toousers jogosultsággal.

További információ:

* [Azure Active Directory szerepköralapú hozzáférés-vezérlése](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-toostorage-objects"></a>Delegált hozzáférést toostorage objektumok
A közös hozzáférésű jogosultságkód (SAS) delegált hozzáférést tooresources a tárfiókban lévő biztosít. hello SAS azt jelenti, hogy egy ügyfél egy adott időszakban, és meghatározott engedélyekkel vannak beállítva a tárfiókban lévő engedélyek tooobjects csak korlátozott biztosíthat. A korlátozott engedélyek megadásához anélkül, hogy tooshare a tárelérési kulcsok. hello SAS URI, amely a lekérdezési paraméterek magában foglalja a hitelesített hozzáférést tooa tárolási erőforrás szükséges minden hello információt. tárolási erőforrások tooaccess hello SAS, hello ügyfélnek csak kell tooprovide hello SAS toohello megfelelő konstruktort vagy metódust.

További információ:

* [Understanding hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [Hozzon létre, és használhatja az SAS-Blob-tároló](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Az átvitel során titkosítás
Titkosítás az átvitel során, akkor egy eszköz adatok védelmének hálózatok közötti átvitelekor. Az Azure Storage segítségével védetté tehet adatokat:

* [Átviteli szintű titkosítást](../storage/common/storage-security-guide.md#encryption-in-transit), például a HTTPS vagy abból az Azure Storage-adatok átvitel során.
* [Hozzá kell fűznie titkosítási](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), például az Azure fájlmegosztások SMB 3.0 titkosítást.
* [Ügyféloldali titkosítás](../storage/common/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello adatokhoz, mielőtt továbbított tárolási és toodecrypt hello adatokká tárolási kivitt után.

További információ az ügyféloldali titkosítás:

* [A Microsoft Azure Storage ügyféloldali titkosítása](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
* [Felhőalapú biztonsági intézkedések adatsorozat: titkosított adatokat átvitel közben](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Titkosítás inaktív állapotban
A legtöbb szervezet számára [adatok titkosítását](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) kötelező lépés adatvédelmi, megfelelőségi és az adatok közös joghatóság alá felé. Nincsenek adatok, amelyek "Inaktív" titkosítás három Azure funkciókat:

* [Storage szolgáltatás titkosítási](../storage/common/storage-security-guide.md#encryption-at-rest) lehetővé teszi, hogy hello tároló szolgáltatás automatikusan adatok titkosítása írásakor azt tooAzure tárolási toorequest.
* [Ügyféloldali titkosítás](../storage/common/storage-security-guide.md#client-side-encryption) aktívan titkosítási hello funkcióval is rendelkezik.
* [Az Azure Disk Encryption](../storage/common/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezek lehetővé teszi.

Ismerje meg a Storage szolgáltatás titkosítási:

* [Az Azure Storage szolgáltatás titkosítási](https://azure.microsoft.com/services/storage/) érhető el [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/). További részletek a más az Azure storage-típusok: [fájl](https://azure.microsoft.com/services/storage/files/), [lemez (prémium szintű Storage)](https://azure.microsoft.com/services/storage/premium-storage/), [tábla](https://azure.microsoft.com/services/storage/tables/), és [várólista](https://azure.microsoft.com/services/storage/queues/).
* [Az Azure Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure Disk Encryption
A virtuális gépek (VM) az Azure Disk Encryption segít cím szervezeti biztonsági és megfelelőségi követelményeknek a Virtuálisgép-lemezek (beleértve a rendszerindító és adatlemezek) titkosítja a kulcsokat és szabályozhatja a házirendek [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

A Linux és Windows operációs rendszerek az adatok titkosítása a virtuális gépek működik. Key Vault toohelp védelme, kezelése és a lemez titkosítási kulcsok használatát naplózni is használja. A virtuális gépek lemezei hello adatait az Azure Storage-fiókok szabványos titkosítási technológiával titkosítása. lemeztitkosítás megoldás a Windows hello alapul [Microsoft BitLocker meghajtótitkosítás](https://technet.microsoft.com/library/cc732774.aspx), és a Linux-megoldás hello alapul [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

További információ:

* [Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális gépeket az Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure Key Vault
Használja az Azure Disk Encryption [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp, szabályozása és lemez titkosítási kulcsok és titkos kódokat a kulcstartót az előfizetést, biztosítva, hogy a hello virtuális gépek lemezeit az összes adat titkosítva legyenek az Azure lévő, aktívan Tárolás. Key Vault tooaudit kulcsok és a házirend-használati kell használni.

További információ:

* [Mi az Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md)
