---
title: "aaaChange hello kulcstároló Bérlőazonosító előfizetés áthelyezése után |} Microsoft Docs"
description: "Ismerje meg, hogyan tooswitch hello Bérlőazonosító után egy előfizetés kulcstároló áthelyezése tooa különböző bérlői"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 4d0607208c61c57959439d2d0bd8feade4141fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Kulcstartó bérlőazonosítójának módosítása az előfizetés áthelyezése után
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>K: az előfizetésem helyezték át a bérlő A tootenant a b kiszolgálóra. Hogyan hello Bérlőazonosító módosítása a meglévő kulcstároló és állítsa be a megfelelő ACL-bérlőben B rendszerbiztonsági tagoknak?
Egy előfizetést hoz létre egy új kulcstartó, esetén automatikusan kapcsolt toohello alapértelmezett Azure Active Directory Bérlőazonosító feliratkozásban. Minden hozzáférési házirendet a rendszer is kapcsolt toothis bérlő azonosítója. Az Azure-előfizetéshez áthelyezésekor bérlőhöz egy tootenant B, tárolók által érhetők el a meglévő kulcs hello résztvevők (a felhasználók és az alkalmazások) bérlői b toofix a probléma, szükséges:

* Változás hello Bérlőazonosító társított összes meglévő kulcstárolójának az előfizetés tootenant a b kiszolgálóra.
* Törölnie kell minden meglévő hozzáférésiszabályzat-bejegyzést.
* Új, a B bérlőhöz kapcsolódó hozzáférésiszabályzat-bejegyzéseket kell létrehoznia.

Például ha kulcstároló "myvault" olyan előfizetést, amelyet át lett helyezve a bérlő A tootenant B, itt meg hogyan toochange hello bérlői azonosító az ezen a kulcstartón és távolítsa el a régi hozzáférési házirendeket.

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Mivel ebben a tárolóban A bérlő hello áthelyezés előtt, hello eredeti értéke **$vault. Properties.TenantId** bérlő a kicsit **(Get-AzureRmContext). Tenant.TenantId** van bérlői a b kiszolgálóra.

Most, hogy a tároló hello megfelelő Bérlőazonosító és régi hozzáférési házirend bejegyzések törlődnek, állítsa be új hozzáférési házirend bejegyzést, amelyben [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Következő lépések
Ha az Azure Key Vault kapcsolatos kérdésekre, keresse fel a hello [Azure Key Vault fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

