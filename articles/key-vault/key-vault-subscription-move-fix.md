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
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a><span data-ttu-id="ee64e-103">Kulcstartó bérlőazonosítójának módosítása az előfizetés áthelyezése után</span><span class="sxs-lookup"><span data-stu-id="ee64e-103">Change a key vault tenant ID after a subscription move</span></span>
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a><span data-ttu-id="ee64e-104">K: az előfizetésem helyezték át a bérlő A tootenant a b kiszolgálóra. Hogyan hello Bérlőazonosító módosítása a meglévő kulcstároló és állítsa be a megfelelő ACL-bérlőben B rendszerbiztonsági tagoknak?</span><span class="sxs-lookup"><span data-stu-id="ee64e-104">Q: My subscription was moved from tenant A tootenant B. How do I change hello tenant ID for my existing key vault and set correct ACLs for principals in tenant B?</span></span>
<span data-ttu-id="ee64e-105">Egy előfizetést hoz létre egy új kulcstartó, esetén automatikusan kapcsolt toohello alapértelmezett Azure Active Directory Bérlőazonosító feliratkozásban.</span><span class="sxs-lookup"><span data-stu-id="ee64e-105">When you create a new key vault in a subscription, it is automatically tied toohello default Azure Active Directory tenant ID for that subscription.</span></span> <span data-ttu-id="ee64e-106">Minden hozzáférési házirendet a rendszer is kapcsolt toothis bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ee64e-106">All access policy entries are also tied toothis tenant ID.</span></span> <span data-ttu-id="ee64e-107">Az Azure-előfizetéshez áthelyezésekor bérlőhöz egy tootenant B, tárolók által érhetők el a meglévő kulcs hello résztvevők (a felhasználók és az alkalmazások) bérlői b toofix a probléma, szükséges:</span><span class="sxs-lookup"><span data-stu-id="ee64e-107">When you move your Azure subscription from tenant A tootenant B, your existing key vaults are inaccessible by hello principals (users and applications) in tenant B. toofix this issue, you need to:</span></span>

* <span data-ttu-id="ee64e-108">Változás hello Bérlőazonosító társított összes meglévő kulcstárolójának az előfizetés tootenant a b kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ee64e-108">Change hello tenant ID associated with all existing key vaults in this subscription tootenant B.</span></span>
* <span data-ttu-id="ee64e-109">Törölnie kell minden meglévő hozzáférésiszabályzat-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="ee64e-109">Remove all existing access policy entries.</span></span>
* <span data-ttu-id="ee64e-110">Új, a B bérlőhöz kapcsolódó hozzáférésiszabályzat-bejegyzéseket kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="ee64e-110">Add new access policy entries that are associated with tenant B.</span></span>

<span data-ttu-id="ee64e-111">Például ha kulcstároló "myvault" olyan előfizetést, amelyet át lett helyezve a bérlő A tootenant B, itt meg hogyan toochange hello bérlői azonosító az ezen a kulcstartón és távolítsa el a régi hozzáférési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="ee64e-111">For example, if you have key vault 'myvault' in a subscription that has been moved from tenant A tootenant B, here's how toochange hello tenant ID for this key vault and remove old access policies.</span></span>

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

<span data-ttu-id="ee64e-112">Mivel ebben a tárolóban A bérlő hello áthelyezés előtt, hello eredeti értéke **$vault. Properties.TenantId** bérlő a kicsit **(Get-AzureRmContext). Tenant.TenantId** van bérlői a b kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ee64e-112">Because this vault was in tenant A before hello move, hello original value of **$vault.Properties.TenantId** is tenant A, while **(Get-AzureRmContext).Tenant.TenantId** is tenant B.</span></span>

<span data-ttu-id="ee64e-113">Most, hogy a tároló hello megfelelő Bérlőazonosító és régi hozzáférési házirend bejegyzések törlődnek, állítsa be új hozzáférési házirend bejegyzést, amelyben [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee64e-113">Now that your vault is associated with hello correct tenant ID and old access policy entries are removed, set new access policy entries with [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee64e-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee64e-114">Next steps</span></span>
<span data-ttu-id="ee64e-115">Ha az Azure Key Vault kapcsolatos kérdésekre, keresse fel a hello [Azure Key Vault fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span><span class="sxs-lookup"><span data-stu-id="ee64e-115">If you have questions about Azure Key Vault, visit hello [Azure Key Vault Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span></span>

