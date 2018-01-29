---
title: "Felhasználó által hozzárendelt felügyelt Szolgáltatásidentitás az Azure SDK-k használata a virtuális gép"
description: "Az Azure SDK-k használata a virtuális gép egy felhasználó által hozzárendelt MSI mintakódok."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f9a31a0500a6f5f1c49fc45d5811e28788e6f2b1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/22/2017
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Azure SDK-k használata az egy felhasználó által hozzárendelt felügyelt szolgáltatás identitás (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Ez a cikk a SDK-mintákat, amelyek bemutatják, használja a felhasználó által hozzárendelt MSI-fájl a megfelelő Azure SDK támogatása listáját tartalmazza.

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Minden minta kódot, a parancsfájl a cikkben azt feltételezi, hogy az ügyfél egy MSI-kompatibilis virtuális gépen futó. A virtuális gép "Csatlakozás" szolgáltatást használja az Azure-portálon távolról csatlakozni a virtuális Gépet. A virtuális gép MSI engedélyezésével kapcsolatos részletekért lásd: [konfigurálja a virtuális gép felügyelt szolgáltatás identitásának (MSI) az Azure parancssori felület használatával](msi-qs-configure-cli-windows-vm.md), vagy a variant cikkekben (a PowerShell, Azure portál, sablon vagy egy Azure SDK használatával). 

## <a name="sdk-code-samples"></a>SDK-kódmintákat

| SDK             | Kódminta |
| --------------- | ----------- |
| Python          | [MSI hitelesítéshez használni kívánt egyszerűen a egy virtuális Gépen belül](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Az MSI-kompatibilis virtuális erőforrások kezelése](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>További lépések

- Lásd: [Azure SDK-k](https://azure.microsoft.com/downloads/) Azure SDK-erőforrások teljes listáját, például a szalagtár letöltések, dokumentáció és egyéb.

Az alábbi Megjegyzések szakasz segítségével visszajelzést, és segítsen pontosítsa és a tartalom.








