---
title: "DNS-zónák és a rekordok védelme |} Microsoft Docs"
description: "Hogyan védi a DNS-zónák és a Microsoft Azure DNS rekordhalmazok."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 190e69eb-e820-4fc8-8e9a-baaf0b3fb74a
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2016
ms.author: jonatul
ms.openlocfilehash: 0b7040d6273b3a6b85cd55850d596807226b87fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-protect-dns-zones-and-records"></a><span data-ttu-id="5a185-103">DNS-zónák és rekordok védelme</span><span class="sxs-lookup"><span data-stu-id="5a185-103">How to protect DNS zones and records</span></span>

<span data-ttu-id="5a185-104">DNS-zónák és rekordok a fontos erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5a185-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="5a185-105">A DNS-zónához vagy akár csak egyetlen DNS-rekord törlése azt eredményezheti, hogy a teljes szolgáltatáskimaradás.</span><span class="sxs-lookup"><span data-stu-id="5a185-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="5a185-106">Ezért fontos, hogy a kritikus DNS-zónák és rekordok védett véletlen vagy illetéktelen módosítások ellen.</span><span class="sxs-lookup"><span data-stu-id="5a185-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="5a185-107">Ez a cikk azt ismerteti, hogyan Azure DNS lehetővé teszi a DNS-zónák és a rekordok ilyen változások elleni védelmét.</span><span class="sxs-lookup"><span data-stu-id="5a185-107">This article explains how Azure DNS enables you to protect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="5a185-108">Azure Resource Manager által biztosított két hatékony funkciókat érvénybe lépni: [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) és [erőforrás zárolások](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="5a185-109">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="5a185-109">Role-based access control</span></span>

<span data-ttu-id="5a185-110">Azure szerepköralapú hozzáférés-vezérlés (RBAC) lehetővé teszi, hogy a részletes hozzáféréskezelést az Azure-felhasználók, csoportok és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5a185-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="5a185-111">Az RBAC használ, meg lehet adni pontosan olyan mértékű hozzáférést a felhasználóknak frissíteniük kell a munkája elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a185-111">Using RBAC, you can grant precisely the amount of access that users need to perform their jobs.</span></span> <span data-ttu-id="5a185-112">Hogyan segít az RBAC-hozzáférés kezelése kapcsolatos további információkért lásd: [Mi az szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="the-dns-zone-contributor-role"></a><span data-ttu-id="5a185-113">A "DNS-zóna közreműködői" szerepkör</span><span class="sxs-lookup"><span data-stu-id="5a185-113">The 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="5a185-114">A "DNS-zóna közreműködői" szerepkör a rendszer beépített Azure által biztosított DNS-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="5a185-114">The 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="5a185-115">DNS-zóna közreműködői engedélyekkel hozzárendelése egy felhasználóhoz vagy csoporthoz lehetővé teszi, hogy a DNS-erőforrás, de nem bármilyen más típusú erőforrások kezelése a csoportot.</span><span class="sxs-lookup"><span data-stu-id="5a185-115">Assigning DNS Zone Contributor permissions to a user or group enables that group to manage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="5a185-116">Tegyük fel, hogy az erőforrás csoport "myzones" Contoso Corporation öt zónák tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5a185-116">For example, suppose the resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="5a185-117">A DNS-rendszergazda engedélyekkel "DNS-zóna közreműködői" erőforráscsoport, lehetővé teszi számára teljes hozzáférést adott DNS-zónák.</span><span class="sxs-lookup"><span data-stu-id="5a185-117">Granting the DNS administrator 'DNS Zone Contributor' permissions to that resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="5a185-118">Azt is elkerüli a szükségtelen engedélyeket ad, például a DNS-rendszergazda nem hozható létre, vagy állítsa le a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="5a185-118">It also avoids granting unnecessary permissions, for example the DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="5a185-119">A legegyszerűbb RBAC engedélyek hozzárendelése [az Azure-portálon](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-119">The simplest way to assign RBAC permissions is [via the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="5a185-120">Az erőforráscsoport "Hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás", majd válassza ki a "DNS-zóna közreműködői" szerepkör és válassza ki a szükséges felhasználók vagy csoportok olyan engedélyek megadásához.</span><span class="sxs-lookup"><span data-stu-id="5a185-120">Open the 'Access control (IAM)' blade for the resource group, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![Erőforráscsoport szintjén RBAC az Azure-portálon](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="5a185-122">Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="5a185-123">Az egyenértékű parancs egyben [elérhető az Azure parancssori felület](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-123">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="5a185-124">Zóna szintjét RBAC</span><span class="sxs-lookup"><span data-stu-id="5a185-124">Zone level RBAC</span></span>

<span data-ttu-id="5a185-125">Az Azure RBAC-szabályok előfizetés, egy erőforráscsoport vagy egy egyéni erőforrást lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="5a185-125">Azure RBAC rules can be applied to a subscription, a resource group or to an individual resource.</span></span> <span data-ttu-id="5a185-126">Esetén az Azure DNS-ben, hogy az erőforrás egy egyéni DNS-zónát, vagy még egyedi rekordhalmaz lehet.</span><span class="sxs-lookup"><span data-stu-id="5a185-126">In the case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="5a185-127">Tegyük fel, hogy az erőforrás "myzones" csoport tartalmazza a "contoso.com" zóna és a subzone "customers.contoso.com", amelyben a CNAME-rekordok létrejönnek az egyes felhasználói fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="5a185-127">For example, suppose the resource group 'myzones' contains the zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="5a185-128">A CNAME-rekordok kezelése használt fiókot hozzá kell rendelni rekordok csak a "customers.contoso.com" zónában való létrehozásához szükséges engedélyek, azt nem rendelkeznek hozzáféréssel a más zónákhoz.</span><span class="sxs-lookup"><span data-stu-id="5a185-128">The account used to manage these CNAME records should be assigned permissions to create records in the 'customers.contoso.com' zone only, it should not have access to the other zones.</span></span>

<span data-ttu-id="5a185-129">Zónaszintű RBAC hozzárendelhető az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5a185-129">Zone-level RBAC permissions can be granted via the Azure portal.</span></span>  <span data-ttu-id="5a185-130">A zóna a "Hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás", majd válassza ki a "DNS-zóna közreműködői" szerepkör és válassza ki a szükséges felhasználók vagy csoportok olyan engedélyek megadásához.</span><span class="sxs-lookup"><span data-stu-id="5a185-130">Open the 'Access control (IAM)' blade for the zone, then click 'Add', then select the 'DNS Zone Contributor' role and select the required users or groups to grant permissions.</span></span>

![DNS-zóna szintű RBAC az Azure-portálon](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="5a185-132">Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions to a specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="5a185-133">Az egyenértékű parancs egyben [elérhető az Azure parancssori felület](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-133">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="5a185-134">A rekordhalmaz RBAC szint</span><span class="sxs-lookup"><span data-stu-id="5a185-134">Record set level RBAC</span></span>

<span data-ttu-id="5a185-135">Azt is egy lépéssel további.</span><span class="sxs-lookup"><span data-stu-id="5a185-135">We can go one step further.</span></span> <span data-ttu-id="5a185-136">Vegye figyelembe a levelezési rendszergazda a Contoso Corporation, hozzá kell férnie a "contoso.com" zóna tetején MX és TXT rekord.</span><span class="sxs-lookup"><span data-stu-id="5a185-136">Consider the mail administrator for Contoso Corporation, who needs access to the MX and TXT records at the apex of the 'contoso.com' zone.</span></span>  <span data-ttu-id="5a185-137">Nem szükséges, ő hozzáférést minden olyan egyéb MX vagy TXT rekord vagy bármilyen más típusú rekordok.</span><span class="sxs-lookup"><span data-stu-id="5a185-137">She doesn't need access to any other MX or TXT records, or to any records of any other type.</span></span>  <span data-ttu-id="5a185-138">Az Azure DNS lehetővé teszi a rekordhalmaz szintű engedélyek hozzárendelése a pontosan az azt jelzi, hogy a levelezési rendszergazdai hozzáférésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5a185-138">Azure DNS allows you to assign permissions at the record set level, to precisely the records that the mail administrator needs access to.</span></span>  <span data-ttu-id="5a185-139">A levelezési rendszergazda kap pontosan a vezérlő egyenként kell, és nem tud más módosítást.</span><span class="sxs-lookup"><span data-stu-id="5a185-139">The mail administrator is granted precisely the control she needs, and is unable to make any other changes.</span></span>

<span data-ttu-id="5a185-140">Rekordhalmaz szintű RBAC engedélyek konfigurálható az Azure-portál, a "Felhasználók" gombra kattintva a rekordhalmaz panelen:</span><span class="sxs-lookup"><span data-stu-id="5a185-140">Record-set level RBAC permissions can be configured via the Azure portal, using the 'Users' button in the record set blade:</span></span>

![A rekordhalmaz szint RBAC az Azure-portálon](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="5a185-142">Rekordhalmaz szintű RBAC engedélyeket is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions to a specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="5a185-143">Az egyenértékű parancs egyben [elérhető az Azure parancssori felület](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="5a185-143">The equivalent command is also [available via the Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions to a specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="5a185-144">Egyéni szerepkörök</span><span class="sxs-lookup"><span data-stu-id="5a185-144">Custom roles</span></span>

<span data-ttu-id="5a185-145">A beépített "DNS-zóna közreműködői" szerepkör lehetővé teszi, hogy a DNS-erőforrásrekordok teljes ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="5a185-145">The built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="5a185-146">A rendszer is létre lehet hozni a saját felhasználói Azure szerepkörökhöz, akkor pontosabban megbízhatatlanná vezérlő.</span><span class="sxs-lookup"><span data-stu-id="5a185-146">It is also possible to build your own customer Azure roles, to provide even finer-grained control.</span></span>

<span data-ttu-id="5a185-147">Tekintse meg ismét a példát, amelyben egy olyan CNAME rekordot a zónában "customers.contoso.com" minden Contoso Corporation felhasználói fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="5a185-147">Consider again the example in which a CNAME record in the zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="5a185-148">A CNAME kezeléséhez használt fiók engedélyt fognak CNAME rekordok csak kezelését.</span><span class="sxs-lookup"><span data-stu-id="5a185-148">The account used to manage these CNAMEs should be granted permission to manage CNAME records only.</span></span>  <span data-ttu-id="5a185-149">Majd nem elvégezni a szükséges (például az MX-rekordok módosítása) más típusú rekordjainak módosítására és zóna szintű műveletek, például a zóna törlése.</span><span class="sxs-lookup"><span data-stu-id="5a185-149">It is then unable to modify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="5a185-150">A következő példa bemutatja egy egyéni biztonsági szerepkört definíciója csak CNAME-rekordok kezelése:</span><span class="sxs-lookup"><span data-stu-id="5a185-150">The following example shows a custom role definition for managing CNAME records only:</span></span>

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/ c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

<span data-ttu-id="5a185-151">A műveletek tulajdonság határozza meg a következő DNS-specifikus engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="5a185-151">The Actions property defines the following DNS-specific permissions:</span></span>

* <span data-ttu-id="5a185-152">`Microsoft.Network/dnsZones/CNAME/*`teljes hozzáférést biztosít a CNAME-rekordok</span><span class="sxs-lookup"><span data-stu-id="5a185-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="5a185-153">`Microsoft.Network/dnsZones/read`engedélyt ad a DNS-zónák olvasni, de nem módosíthatók, amely lehetővé teszi, hogy a zóna, ahol a CNAME létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="5a185-153">`Microsoft.Network/dnsZones/read` grants permission to read DNS zones, but not to modify them, enabling you to see the zone in which the CNAME is being created.</span></span>

<span data-ttu-id="5a185-154">A fennmaradó műveletek másolja át a [DNS-zóna közreműködői beépített szerepkör](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="5a185-154">The remaining Actions are copied from the [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="5a185-155">Egyéni RBAC szerepkör alapján rekordhalmazok törlése közben továbbra is lehetővé teheti, hogy frissíthető nem hatékony ellenőrzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="5a185-155">Using a custom RBAC role to prevent deleting record sets while still allowing them to be updated is not an effective control.</span></span> <span data-ttu-id="5a185-156">Megakadályozza, hogy rekordhalmazok törlés alatt áll, de nem akadályozza meg azok már nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="5a185-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="5a185-157">Engedélyezett módosítások hozzáadása és eltávolítása a bejegyzések a rekordhalmaz, beleértve az "empty" rekordhalmaz, hogy az összes rekord eltávolítását a tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5a185-157">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="5a185-158">Ugyanaz, mintha egy DNS-feloldási szempontból a rekordhalmaz törlése azt.</span><span class="sxs-lookup"><span data-stu-id="5a185-158">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="5a185-159">Egyéni szerepkör-definíciók jelenleg nem lehet meghatározni az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5a185-159">Custom role definitions cannot currently be defined via the Azure portal.</span></span> <span data-ttu-id="5a185-160">Egy egyéni biztonsági szerepkört a szerepkör-definíció alapján Azure PowerShell használatával hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="5a185-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="5a185-161">Is létrehozhatók az Azure parancssori felületen keresztül:</span><span class="sxs-lookup"><span data-stu-id="5a185-161">It can also be created via the Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="5a185-162">A szerepkör lehet hozzárendelni a beépített szerepkörök azonos módon az ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="5a185-162">The role can then be assigned in the same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="5a185-163">Létrehozására, kezelésére és egyéni szerepkörök hozzárendelése kapcsolatos további információkért lásd: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-163">For more information on how to create, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="5a185-164">Erőforrás zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="5a185-164">Resource locks</span></span>

<span data-ttu-id="5a185-165">Mellett RBAC Azure Resource Manager támogatja a más típusú biztonsági ellenőrzést, nevezetesen erőforrások "lock" lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5a185-165">In addition to RBAC, Azure Resource Manager supports another type of security control, namely the ability to 'lock' resources.</span></span> <span data-ttu-id="5a185-166">Ahol RBAC szabályok lehetővé teszik annak vezérléséhez a műveletek a megadott felhasználók és csoportok, erőforrás zárolások lesznek alkalmazva az erőforráshoz, és összes felhasználók és szerepkörök érvényben.</span><span class="sxs-lookup"><span data-stu-id="5a185-166">Where RBAC rules allow you to control the actions of specific users and groups, resource locks are applied to the resource, and are effective across all users and roles.</span></span> <span data-ttu-id="5a185-167">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="5a185-168">Erőforrás-zárolás két típusa van: **DoNotDelete** és **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="5a185-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="5a185-169">Ezek a DNS-zónát, vagy egy egyéni rekordhalmaz alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="5a185-169">These can be applied either to a DNS zone, or to an individual record set.</span></span>  <span data-ttu-id="5a185-170">A következő szakaszok ismertetik a számos gyakori helyzetek, és hogyan őket támogató erőforrás zárolások.</span><span class="sxs-lookup"><span data-stu-id="5a185-170">The following sections describe several common scenarios, and how to support them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="5a185-171">Minden változást elleni védelem</span><span class="sxs-lookup"><span data-stu-id="5a185-171">Protecting against all changes</span></span>

<span data-ttu-id="5a185-172">Megakadályozza a módosítását, végeznek, a zóna a csak olvasható zárolási vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="5a185-172">To prevent any changes being made, apply a ReadOnly lock to the zone.</span></span>  <span data-ttu-id="5a185-173">Ez megakadályozza, hogy új rekordhalmazok létrehozása, és a meglévő rekordhalmazok módosítani vagy törlés alatt.</span><span class="sxs-lookup"><span data-stu-id="5a185-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="5a185-174">Az Azure-portálon zóna szintű erőforrás zárolások hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="5a185-174">Zone level resource locks can be created via the Azure portal.</span></span>  <span data-ttu-id="5a185-175">A DNS-zóna paneljén kattintson a "Zárolások", majd "hozzáadása":</span><span class="sxs-lookup"><span data-stu-id="5a185-175">From the DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zóna szintű erőforrás zárolások az Azure-portálon](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="5a185-177">Zóna szintű erőforrás zárolások is Azure PowerShell használatával hozható létre:</span><span class="sxs-lookup"><span data-stu-id="5a185-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="5a185-178">Azure-erőforrás zárolásainak beállítása jelenleg nem támogatott az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="5a185-178">Configuring Azure resource locks is not currently supported via the Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="5a185-179">Az egyes rekordok védelme</span><span class="sxs-lookup"><span data-stu-id="5a185-179">Protecting individual records</span></span>

<span data-ttu-id="5a185-180">Egy meglévő DNS-rekordot szemben beállítása megelőzése érdekében a rekordhalmaz csak olvasható zárolást vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="5a185-180">To prevent an existing DNS record set against modification, apply a ReadOnly lock to the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="5a185-181">A DoNotDelete zárolási alkalmazása rekordhalmaz nincs hatékony ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5a185-181">Applying a DoNotDelete lock to a record set is not an effective control.</span></span> <span data-ttu-id="5a185-182">Megakadályozza, hogy a rekordhalmaz törlését, de nem akadályozza meg, már nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="5a185-182">It prevents the record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="5a185-183">Engedélyezett módosítások hozzáadása és eltávolítása a bejegyzések a rekordhalmaz, beleértve az "empty" rekordhalmaz, hogy az összes rekord eltávolítását a tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5a185-183">Permitted modifications include adding and removing records from the record set, including removing all records to leave an 'empty' record set.</span></span> <span data-ttu-id="5a185-184">Ugyanaz, mintha egy DNS-feloldási szempontból a rekordhalmaz törlése azt.</span><span class="sxs-lookup"><span data-stu-id="5a185-184">This has the same effect as deleting the record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="5a185-185">Rekordhalmaz szintű erőforrás zárolások is jelenleg csak Azure PowerShell használatával konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="5a185-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="5a185-186">Az Azure-portálon vagy az Azure parancssori felület azok nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5a185-186">They are not supported in the Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="5a185-187">Zóna törlés elleni védelem</span><span class="sxs-lookup"><span data-stu-id="5a185-187">Protecting against zone deletion</span></span>

<span data-ttu-id="5a185-188">Az Azure DNS-zóna törlése után a zónában minden rekordkészletet is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="5a185-188">When a zone is deleted in Azure DNS, all record sets in the zone are also deleted.</span></span>  <span data-ttu-id="5a185-189">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="5a185-189">This operation cannot be undone.</span></span>  <span data-ttu-id="5a185-190">A kritikus zóna véletlen törlése magában hordozza a jelentős üzleti hatással van.</span><span class="sxs-lookup"><span data-stu-id="5a185-190">Accidentally deleting a critical zone has the potential to have a significant business impact.</span></span>  <span data-ttu-id="5a185-191">Ezért nagyon fontos zóna véletlen törlés elleni védelem érdekében.</span><span class="sxs-lookup"><span data-stu-id="5a185-191">It is therefore very important to protect against accidental zone deletion.</span></span>

<span data-ttu-id="5a185-192">A DoNotDelete zárolási alkalmazása a zóna megakadályozza, hogy a zóna törlés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="5a185-192">Applying a DoNotDelete lock to a zone prevents the zone from being deleted.</span></span>  <span data-ttu-id="5a185-193">Azonban mivel zárolások gyermekszintű erőforrása által örökölt, is megakadályozza, hogy bármely rekordhalmazok törlését, a zónában, esetlegesen nemkívánatos.</span><span class="sxs-lookup"><span data-stu-id="5a185-193">However, since locks are inherited by child resources, it also prevents any record sets in the zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="5a185-194">Ezenkívül lásd: a fenti megjegyzést, ez a beállítás is hatástalan mivel rekordok továbbra is távolíthatók el a meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="5a185-194">Furthermore, as described in the note above, it is also ineffective since records can still be removed from the existing record sets.</span></span>

<span data-ttu-id="5a185-195">Alternatív megoldásként vegye figyelembe a DoNotDelete zárolási alkalmazása egy rekordot a zónában, például a SOA típusú rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="5a185-195">As an alternative, consider applying a DoNotDelete lock to a record set in the zone, such as the SOA record set.</span></span>  <span data-ttu-id="5a185-196">A zóna a rekordhalmazok is törlése nélkül nem lehet törölni, mivel ez véd zóna törlése, miközben továbbra is lehetővé teszi a rekordhalmazok szabadon lehet módosítani a zónán belül.</span><span class="sxs-lookup"><span data-stu-id="5a185-196">Since the zone cannot be deleted without also deleting the record sets, this protects against zone deletion, while still allowing record sets within the zone to be modified freely.</span></span> <span data-ttu-id="5a185-197">Ha törli a zóna tett kísérlet, Azure Resource Manager észleli, ez is törölni a SOA típusú rekordkészlet, és blokkolja a hívást, mert a SOA zárolva van.</span><span class="sxs-lookup"><span data-stu-id="5a185-197">If an attempt is made to delete the zone, Azure Resource Manager detects this would also delete the SOA record set, and blocks the call because the SOA is locked.</span></span>  <span data-ttu-id="5a185-198">Nincs rekordhalmazok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="5a185-198">No record sets are deleted.</span></span>

<span data-ttu-id="5a185-199">A következő PowerShell-parancsot a megadott zónában SOA típusú rekordjának elleni DoNotDelete zárolást hoz létre:</span><span class="sxs-lookup"><span data-stu-id="5a185-199">The following PowerShell command creates a DoNotDelete lock against the SOA record of the given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on the record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="5a185-200">Úgy is, hogy a zóna véletlen törlés annak biztosítása érdekében a következő operátor egy egyéni biztonsági szerepkört használatával, és a zónák kezelése használt szolgáltatásfiókokat nem rendelkezik engedélyekkel törlése zóna.</span><span class="sxs-lookup"><span data-stu-id="5a185-200">Another way to prevent accidental zone deletion is by using a custom role to ensure the operator and service accounts used to manage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="5a185-201">Ha törölni szeretne egy zónát, kényszerítheti a kétlépéses törlési, első támogatást nyújtó zóna törlése engedélyek (a hatókörben zóna, törölje a nem megfelelő zónát megelőzése érdekében), második törölni a zónát.</span><span class="sxs-lookup"><span data-stu-id="5a185-201">When you do need to delete a zone, you can enforce a two-step delete, first granting zone delete permissions (at the zone scope, to prevent deleting the wrong zone) and second to delete the zone.</span></span>

<span data-ttu-id="5a185-202">Ez a második megközelítés azzal az előnnyel jár, a működését, az összes olyan zónára, ezek a fiókok, anélkül, hogy meg kellene jegyezni a zárolások létrehozásához érhetők el.</span><span class="sxs-lookup"><span data-stu-id="5a185-202">This second approach has the advantage that it works for all zones accessed by those accounts, without having to remember to create any locks.</span></span> <span data-ttu-id="5a185-203">A hátránya, hogy minden zóna törlési engedéllyel, például az előfizetés tulajdonosa rendelkező fiókok véletlenül továbbra is törölheti a kritikus zóna rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5a185-203">It has the disadvantage that any accounts with zone delete permissions, such as the subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="5a185-204">Akkor lehet egy időben, a DNS-zóna védelemre védelemmel az olyan jellegű megközelítés mindkét megközelítés - erőforrás zárolások és egyéni szerepkör - használatát.</span><span class="sxs-lookup"><span data-stu-id="5a185-204">It is possible to use both approaches - resource locks and custom roles - at the same time, as a defense-in-depth approach to DNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a185-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a185-205">Next steps</span></span>

* <span data-ttu-id="5a185-206">Az RBAC használatáról további információk: [Ismerkedés az Azure-portálon kezelési](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-206">For more information about working with RBAC, see [Get started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="5a185-207">Működő erőforrás zárral kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5a185-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

