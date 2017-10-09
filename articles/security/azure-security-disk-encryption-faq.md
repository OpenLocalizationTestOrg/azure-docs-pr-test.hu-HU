---
title: "aaaAzure lemez titkosítási – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk ismerteti a válaszok toofrequently kérdések a Microsoft Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.openlocfilehash: 17f084628ba4ef22e9d37dd3052ef10f8eb2cd7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-frequently-asked-questions-faq"></a>Az Azure Disk Encryption gyakori kérdések (GYIK)

Ez a GYIK a szolgáltatás olvasható további információ a Windows és Linux IaaS virtuális gépeket, Azure lemeztitkosítás kapcsolatos kérdésekre ad választ [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Általános kérdések
**K.** Melyik régióban az Azure disk encryption a GA?

**V:** Azure lemeztitkosítás Windows és Linux IaaS virtuális gépeket az Azure nyilvános régiókban GA érhető el.

**K:** milyen felhasználó érhetők el az Azure Disk Encryption?

**V:** Azure lemez titkosítási GA támogatja az Azure Resource Manager-sablonok, Azure PowerShell, az Azure parancssori felület. Ez lehetővé teszi a magas fokú rugalmasságot biztosít abban a három különböző beállítások érhetők el az infrastruktúra-szolgáltatási virtuális lemez-titkosítás engedélyezése. További részleteket a hello felhasználói élmény és részletes útmutatás érhető hello Azure Disk Encryption üzembe helyezési forgatókönyvek és feladatait.

**K:** mennyi does Azure Disk Encryption költség?

**V:** használata virtuális gépek lemezei az Azure Disk Encryption titkosítási díjmentes.

**K:** milyen virtuálisgép-réteg használható az Azure Disk Encryption?

**V:** Azure Disk Encryption áll rendelkezésre, csak a Standard szint virtuális gépek például [A, D, DS, G, GS, F](https://azure.microsoft.com/pricing/details/virtual-machines/) adatsorozat IaaS virtuális gépeket, prémium szintű Storage többek között a virtuális gépek stb. Nem érhető el a alapvető réteg virtuális gépeken.

**K:** mi Linux terjesztésekről Azure Disk Encryption által támogatott?

**V:** Azure Disk Encryption hello Linux server disztribúciók és verziók esetében támogatott:

| A Linux-Disztribúció | Verzió | Támogatott titkosítási a kötet típusa|
| --- | --- |--- |
| Ubuntu | 16.04-NAPI-ES LTS VERZIÓ | Operációs rendszer és az adatok lemezre |
| Ubuntu | 14.04.5-DAILY-LTS | Operációs rendszer és az adatok lemezre |
| RHEL | 7.3 | Operációs rendszer és az adatok lemezre |
| RHEL | 7.2 | Operációs rendszer és az adatok lemezre |
| RHEL | 6.8 | Operációs rendszer és az adatok lemezre |
| RHEL | 6.7 | Adatlemez |
| CentOS | 7.3 | Operációs rendszer és az adatok lemezre |
| CentOS | 7.2n | Operációs rendszer és az adatok lemezre |
| CentOS | 6.8 | Operációs rendszer és az adatok lemezre |
| CentOS | 7.1 | Adatlemez |
| CentOS | 7.0 | Adatlemez |
| CentOS | 6.7 | Adatlemez |
| CentOS | 6.6 | Adatlemez |
| CentOS | 6.5 | Adatlemez |
| openSUSE | 13.2 | Adatlemez |
| SLES | 12 SP1 | Adatlemez |
| SLES | Prioritás: 12-SP1 | Adatlemez |
| SLES | HPC 12 | Adatlemez |
| SLES | Prioritás: 11-SP4 | Adatlemez |
| SLES | 11 SP4 | Adatlemez |

**K:** hogyan lehet kezdjem el Azure Disk Encryption használatával?

**V:** ügyfelek megtudhatja, hogyan tooget indította olvasási hello Azure Disk Encryption tanulmány található [Itt](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

**K:** titkosíthatók a rendszerindító és adatkötetek számára, az Azure Disk Encryption?

**V:** Igen, a Windows és Linux IaaS virtuális gépeket titkosíthatja rendszerindító- és adatkötetek számára. Windows virtuális gépek első encrpting hello rendszerkötet nélkül hello adatok nem titkosíthatók. Linux virtuális gépekhez először titkosíthatja hello adatmennyiség encryptinng hello rendszerkötet nélkül. A Linux operációs rendszer hello kötet titkosított, miután a Linux IaaS virtuális gépeket az operációs rendszer kötet titkosításának letiltása nem támogatott

**K:** Does Azure Disk Encryption engedélyezése a "állapotba hozása a saját kulcs" (BYOK) funkció?

**V:** Igen, megadhatja a saját kulcs titkosítási kulcsokat. Ezeknek a kulcsoknak elő az Azure Key Vault, vagyis az Azure Disk Encryption hello kulcstároló. A hello kulcs titkosítási kulccsal kapcsolatos további részletek helyzetek feltételeinek megteremtésére című hello Azure Disk Encryption üzembe helyezési forgatókönyvek és elemek

**K:** használható egy Azure által létrehozott kulcs titkosítási kulcsot?

**V:** Igen, használhatja az Azure Key vault toogenerate kulcs titkosítási kulcsot az Azure disk encryption használatát. Ezeknek a kulcsoknak elő az Azure Key Vault, vagyis az Azure Disk Encryption hello kulcstároló. A hello kulcs titkosítási kulccsal kapcsolatos további részletek helyzetek feltételeinek megteremtésére című hello Azure Disk Encryption üzembe helyezési forgatókönyvek és elemek

**K:** használható helyszíni kulcskezelő szolgáltatás/HSM toosafeguard hello titkosítási kulcsokat?

**V:** hello helyszíni kulcskezelő szolgáltatás/HSM toosafeguard hello titkosítási kulcsok nem használhatja az Azure disk encryption. Csak használhat hello az Azure key vault szolgáltatás toosafeguard hello titkosítási kulcsokat. A hello kulcs titkosítási kulccsal kapcsolatos további részletek helyzetek feltételeinek megteremtésére című hello Azure Disk Encryption üzembe helyezési forgatókönyvek és elemek

**K:** hello Előfeltételek tooconfigure Azure disk encryption Mik?

**V:** hello Azure disk encryption előfeltétel PowerShell parancsfájl toocreate AAD-alkalmazást, hozzon létre új kulcstartó vagy a telepítés meglévő kulcstároló lemez titkosítási hozzáférés tooenable titkosítás és a biztonságos működés érdekében titkok és kulcs esetében.  A hello kulcs titkosítási kulccsal kapcsolatos további részletek helyzetek feltételeinek megteremtésére című hello Azure Disk Encryption előfeltétel és üzembe helyezési forgatókönyvek és elemek

**K:** Honnan kaphatok további információt az Azure Disk Encryption konfigurálása PowerShell toouse?

**V:** van néhány nagy cikkeket hogyan hajthat végre a alapszintű Azure Disk Encryption feladatokat, valamint speciális forgatókönyvekhez. Hello alapvető műveleteket, tekintse meg a böngészés [Azure Disk Encryption az Azure PowerShell - 1. rész](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Speciális forgatókönyvek esetén lásd: a böngészés [Azure Disk Encryption az Azure PowerShell – 2. rész](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/)

**K:** Azure PowerShell verziójának Azure Disk Encryption compatibilitytype?

**V:** használata hello legújabb Azure PowerShell SDK verzió tooconfigure Azure Disk Encryption. Hello legújabb verziójának letöltése [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure SDK 1.1.0-ás verziója nem támogatja az Azure Disk Encryption.

> [!NOTE]
> hello Linux Azure lemez titkosítási preview bővítmény elavult. További információkért tekintse meg a toodocumentation [Itt](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/)

**K:** alkalmazhatók az Azure disk encryption saját egyéni Linux rendszerképre?

**V:** Azure disk encryption az egyéni Linux-lemezkép nem lehet alkalmazni. Csak hello lévő Linux képek fent hello támogatott disztribúciókkal támogatjuk. Jelenleg nem támogatott Linux egyéni lemezképek

**K:** alkalmazhatók frissítések tooa Linux Red Hat VM Yum frissítést?

**V:** Igen, képes végre frissítést és, vagy a Red Hat Linnux virtuális gépek a következő foglalt útmutatásnak megfelelően javítás [Itt](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/)

**K:** ahol is I nyissa meg tooask kérdés vagy észrevétel küldése

**V:** megadhatja, kérje meg a kérdései vagy visszajelzései hello Azure disk encryption fórumon [Itt](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption)

## <a name="see-also"></a>Lásd még:
Ebből a dokumentumból megismerte több hello leggyakoribb kérdések kapcsolódó tooAzure lemeztitkosítás, ez a szolgáltatás és a funkció, olvassa el további információ:

- [Az Azure Security Centerben lemeztitkosítás alkalmazása](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Azure virtuális gép titkosítása](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Az Azure Data Encryption nyugalmi](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
