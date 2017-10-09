---
title: "aaaProtecting DNS-zónák és rekordok |} Microsoft Docs"
description: "Hogyan tooprotect DNS-zónák és rekord beállítása a Microsoft Azure DNS-ben."
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
ms.openlocfilehash: 7945f6240feeed3d79a11d340f9f845e083026ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-dns-zones-and-records"></a><span data-ttu-id="c8e87-103">Hogyan tooprotect DNS zónák, valamint megjegyzi</span><span class="sxs-lookup"><span data-stu-id="c8e87-103">How tooprotect DNS zones and records</span></span>

<span data-ttu-id="c8e87-104">DNS-zónák és rekordok a fontos erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c8e87-104">DNS zones and records are critical resources.</span></span> <span data-ttu-id="c8e87-105">A DNS-zónához vagy akár csak egyetlen DNS-rekord törlése azt eredményezheti, hogy a teljes szolgáltatáskimaradás.</span><span class="sxs-lookup"><span data-stu-id="c8e87-105">Deleting a DNS zone or even just a single DNS record can result in a total service outage.</span></span>  <span data-ttu-id="c8e87-106">Ezért fontos, hogy a kritikus DNS-zónák és rekordok védett véletlen vagy illetéktelen módosítások ellen.</span><span class="sxs-lookup"><span data-stu-id="c8e87-106">It is therefore important that critical DNS zones and records are protected against unauthorized or accidental changes.</span></span>

<span data-ttu-id="c8e87-107">Ez a cikk azt ismerteti, hogyan Azure DNS-ben lehetővé teszi, hogy Ön tooprotect a DNS-zónák és a változások alapján.</span><span class="sxs-lookup"><span data-stu-id="c8e87-107">This article explains how Azure DNS enables you tooprotect your DNS zones and records against such changes.</span></span>  <span data-ttu-id="c8e87-108">Azure Resource Manager által biztosított két hatékony funkciókat érvénybe lépni: [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) és [erőforrás zárolások](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-108">We apply two powerful security features provided by Azure Resource Manager: [role-based access control](../active-directory/role-based-access-control-what-is.md) and [resource locks](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="c8e87-109">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="c8e87-109">Role-based access control</span></span>

<span data-ttu-id="c8e87-110">Azure szerepköralapú hozzáférés-vezérlés (RBAC) lehetővé teszi, hogy a részletes hozzáféréskezelést az Azure-felhasználók, csoportok és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c8e87-110">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure users, groups, and resources.</span></span> <span data-ttu-id="c8e87-111">Szerepalapú használ, meg lehet adni pontosan hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="c8e87-111">Using RBAC, you can grant precisely hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="c8e87-112">Hogyan segít az RBAC-hozzáférés kezelése kapcsolatos további információkért lásd: [Mi az szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-112">For more information about how RBAC helps you manage access, see [What is Role-Based Access Control](../active-directory/role-based-access-control-what-is.md).</span></span>

### <a name="hello-dns-zone-contributor-role"></a><span data-ttu-id="c8e87-113">hello "DNS-zóna közreműködői" szerepkör</span><span class="sxs-lookup"><span data-stu-id="c8e87-113">hello 'DNS Zone Contributor' role</span></span>

<span data-ttu-id="c8e87-114">hello "DNS-zóna közreműködői" szerepkör a rendszer beépített Azure által biztosított DNS-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="c8e87-114">hello 'DNS Zone Contributor' role is a built-in role provided by Azure for managing DNS resources.</span></span>  <span data-ttu-id="c8e87-115">DNS-zóna közreműködői engedélyekkel tooa felhasználó vagy csoport hozzárendelése lehetővé teszi, hogy a csoport toomanage DNS erőforrásokat, de nem bármilyen más típusú erőforrásokra.</span><span class="sxs-lookup"><span data-stu-id="c8e87-115">Assigning DNS Zone Contributor permissions tooa user or group enables that group toomanage DNS resources, but not resources of any other type.</span></span>

<span data-ttu-id="c8e87-116">Tegyük fel, hogy hello erőforrás csoport "myzones" Contoso Corporation öt zónák tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c8e87-116">For example, suppose hello resource group 'myzones' contains five zones for Contoso Corporation.</span></span> <span data-ttu-id="c8e87-117">Támogatást nyújtó hello DNS rendszergazda "DNS-zóna közreműködői" engedélyek toothat erőforráscsoport, lehetővé teszi, hogy ezeket a DNS-zónák teljes ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="c8e87-117">Granting hello DNS administrator 'DNS Zone Contributor' permissions toothat resource group, enables full control over those DNS zones.</span></span> <span data-ttu-id="c8e87-118">Azt is elkerüli a szükségtelen engedélyeket ad, például a hello DNS-rendszergazda nem hozható létre, vagy állítsa le a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c8e87-118">It also avoids granting unnecessary permissions, for example hello DNS administrator cannot create or stop Virtual Machines.</span></span>

<span data-ttu-id="c8e87-119">hello legegyszerűbb módja tooassign RBAC bizton [hello Azure-portálon keresztül](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-119">hello simplest way tooassign RBAC permissions is [via hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>  <span data-ttu-id="c8e87-120">Hello erőforráscsoport, hello "hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás" hello "DNS-zóna közreműködői" szerepkör és a szükséges válassza hello felhasználók vagy csoportok toogrant engedélyek.</span><span class="sxs-lookup"><span data-stu-id="c8e87-120">Open hello 'Access control (IAM)' blade for hello resource group, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![Erőforráscsoport szintjén RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac1.png)

<span data-ttu-id="c8e87-122">Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-122">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="c8e87-123">hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-123">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a><span data-ttu-id="c8e87-124">Zóna szintjét RBAC</span><span class="sxs-lookup"><span data-stu-id="c8e87-124">Zone level RBAC</span></span>

<span data-ttu-id="c8e87-125">Az Azure RBAC-szabályok lehet alkalmazott tooa előfizetés erőforrás csoport vagy tooan egyéni erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c8e87-125">Azure RBAC rules can be applied tooa subscription, a resource group or tooan individual resource.</span></span> <span data-ttu-id="c8e87-126">Az Azure DNS hello esetben, hogy az erőforrás egy egyéni DNS-zónát, vagy még egyedi rekordhalmaz lehet.</span><span class="sxs-lookup"><span data-stu-id="c8e87-126">In hello case of Azure DNS, that resource can be an individual DNS zone, or even an individual record set.</span></span>

<span data-ttu-id="c8e87-127">Tegyük fel, hogy hello erőforrás csoport "myzones" hello "contoso.com" zóna és a subzone "customers.contoso.com", amelyben a CNAME-rekordok létrejönnek az egyes felhasználói fiókokhoz tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c8e87-127">For example, suppose hello resource group 'myzones' contains hello zone 'contoso.com' and a subzone 'customers.contoso.com' in which CNAME records are created for each customer account.</span></span>  <span data-ttu-id="c8e87-128">hello használt fiók toomanage ezeket a CNAME rekordokat hozzá kell rendelni engedélyek toocreate rekordok hello "customers.contoso.com" zónában, nem rendelkezhet hozzáférés toohello más zónák.</span><span class="sxs-lookup"><span data-stu-id="c8e87-128">hello account used toomanage these CNAME records should be assigned permissions toocreate records in hello 'customers.contoso.com' zone only, it should not have access toohello other zones.</span></span>

<span data-ttu-id="c8e87-129">Zónaszintű RBAC hozzárendelhető hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="c8e87-129">Zone-level RBAC permissions can be granted via hello Azure portal.</span></span>  <span data-ttu-id="c8e87-130">Hello zóna hello "Hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás" hello "DNS-zóna közreműködői" szerepkör és a szükséges válassza hello felhasználók vagy csoportok toogrant engedélyek.</span><span class="sxs-lookup"><span data-stu-id="c8e87-130">Open hello 'Access control (IAM)' blade for hello zone, then click 'Add', then select hello 'DNS Zone Contributor' role and select hello required users or groups toogrant permissions.</span></span>

![DNS-zóna szintű RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac2.png)

<span data-ttu-id="c8e87-132">Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-132">Permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

<span data-ttu-id="c8e87-133">hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-133">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a><span data-ttu-id="c8e87-134">A rekordhalmaz RBAC szint</span><span class="sxs-lookup"><span data-stu-id="c8e87-134">Record set level RBAC</span></span>

<span data-ttu-id="c8e87-135">Azt is egy lépéssel további.</span><span class="sxs-lookup"><span data-stu-id="c8e87-135">We can go one step further.</span></span> <span data-ttu-id="c8e87-136">Vegye figyelembe a hello mail rendszergazdának Contoso Corporation, az access toohello MX és TXT rekord hello csúcsán hello "contoso.com" zóna.</span><span class="sxs-lookup"><span data-stu-id="c8e87-136">Consider hello mail administrator for Contoso Corporation, who needs access toohello MX and TXT records at hello apex of hello 'contoso.com' zone.</span></span>  <span data-ttu-id="c8e87-137">Azt nem kell elérni tooany más MX vagy TXT-rekordot, vagy bármilyen más típusú tooany rekord.</span><span class="sxs-lookup"><span data-stu-id="c8e87-137">She doesn't need access tooany other MX or TXT records, or tooany records of any other type.</span></span>  <span data-ttu-id="c8e87-138">Az Azure DNS lehetővé teszi, hogy tooassign hello rekordhalmaz szint tooprecisely hello azt jelzi, hogy hello mail rendszergazdai engedélyekkel kell elérnie.</span><span class="sxs-lookup"><span data-stu-id="c8e87-138">Azure DNS allows you tooassign permissions at hello record set level, tooprecisely hello records that hello mail administrator needs access to.</span></span>  <span data-ttu-id="c8e87-139">hello mail rendszergazda kap hello vezérlő pontosan ugyanúgy kell, és nem toomake bármilyen más jellegű módosítását.</span><span class="sxs-lookup"><span data-stu-id="c8e87-139">hello mail administrator is granted precisely hello control she needs, and is unable toomake any other changes.</span></span>

<span data-ttu-id="c8e87-140">Rekordhalmaz szintű RBAC engedélyek állíthatók hello Azure-portálon, a "Hello"felhasználók"gombra kattintva hello a rekordkészlet panelen:</span><span class="sxs-lookup"><span data-stu-id="c8e87-140">Record-set level RBAC permissions can be configured via hello Azure portal, using hello 'Users' button in hello record set blade:</span></span>

![A rekordhalmaz szint RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac3.png)

<span data-ttu-id="c8e87-142">Rekordhalmaz szintű RBAC engedélyeket is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-142">Record-set level RBAC permissions can also be [granted using Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):</span></span>

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

<span data-ttu-id="c8e87-143">hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span><span class="sxs-lookup"><span data-stu-id="c8e87-143">hello equivalent command is also [available via hello Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md):</span></span>

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a><span data-ttu-id="c8e87-144">Egyéni szerepkörök</span><span class="sxs-lookup"><span data-stu-id="c8e87-144">Custom roles</span></span>

<span data-ttu-id="c8e87-145">hello beépített "DNS-zóna közreműködői" szerepkör lehetővé teszi, hogy a DNS-erőforrásrekordok teljes ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="c8e87-145">hello built-in 'DNS Zone Contributor' role enables full control over a DNS resource.</span></span> <span data-ttu-id="c8e87-146">Ez egyben lehetséges toobuild a saját Azure felhasználói szerepkörök, tooprovide még pontosabban megbízhatatlanná vezérlő.</span><span class="sxs-lookup"><span data-stu-id="c8e87-146">It is also possible toobuild your own customer Azure roles, tooprovide even finer-grained control.</span></span>

<span data-ttu-id="c8e87-147">Érdemes újra hello példában, amelyben hello zóna "customers.contoso.com" CNAME rekord jön létre az egyes Contoso Corporation felhasználói fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="c8e87-147">Consider again hello example in which a CNAME record in hello zone 'customers.contoso.com' is created for each Contoso Corporation customer account.</span></span>  <span data-ttu-id="c8e87-148">hello használt fiók toomanage ezek CNAME adható engedély toomanage CNAME rekordok csak.</span><span class="sxs-lookup"><span data-stu-id="c8e87-148">hello account used toomanage these CNAMEs should be granted permission toomanage CNAME records only.</span></span>  <span data-ttu-id="c8e87-149">Ezt követően nem toomodify rekordok egyéb (például az MX-rekordok módosítása) meg kell adnia, vagy hajtsa végre a zóna szintű műveletek, például a zóna törlése van.</span><span class="sxs-lookup"><span data-stu-id="c8e87-149">It is then unable toomodify records of other types (such as changing MX records) or perform zone-level operations such as zone delete.</span></span>

<span data-ttu-id="c8e87-150">a következő példa hello CNAME rekordok csak kezelésére egy egyéni szerepkör-definíció látható:</span><span class="sxs-lookup"><span data-stu-id="c8e87-150">hello following example shows a custom role definition for managing CNAME records only:</span></span>

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

<span data-ttu-id="c8e87-151">hello műveletek tulajdonság határozza meg az alábbi DNS-specifikus engedélyek hello:</span><span class="sxs-lookup"><span data-stu-id="c8e87-151">hello Actions property defines hello following DNS-specific permissions:</span></span>

* <span data-ttu-id="c8e87-152">`Microsoft.Network/dnsZones/CNAME/*`teljes hozzáférést biztosít a CNAME-rekordok</span><span class="sxs-lookup"><span data-stu-id="c8e87-152">`Microsoft.Network/dnsZones/CNAME/*` grants full control over CNAME records</span></span>
* <span data-ttu-id="c8e87-153">`Microsoft.Network/dnsZones/read`engedélyezi a engedély tooread DNS-zónák, de nem toomodify őket, hogy toosee engedélyezése hello zóna mely hello a CNAME létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="c8e87-153">`Microsoft.Network/dnsZones/read` grants permission tooread DNS zones, but not toomodify them, enabling you toosee hello zone in which hello CNAME is being created.</span></span>

<span data-ttu-id="c8e87-154">fennmaradó műveletek hello átmásolva hello [DNS-zóna közreműködői beépített szerepkör](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span><span class="sxs-lookup"><span data-stu-id="c8e87-154">hello remaining Actions are copied from hello [DNS Zone Contributor built-in role](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).</span></span>

> [!NOTE]
> <span data-ttu-id="c8e87-155">Egy egyéni RBAC szerepkör tooprevent bejegyzés törlése használatával beállítja a pedig továbbra is engedélyezi azok frissített toobe nem hatékony ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c8e87-155">Using a custom RBAC role tooprevent deleting record sets while still allowing them toobe updated is not an effective control.</span></span> <span data-ttu-id="c8e87-156">Megakadályozza, hogy rekordhalmazok törlés alatt áll, de nem akadályozza meg azok már nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="c8e87-156">It prevents record sets from being deleted, but it does not prevent them from being modified.</span></span>  <span data-ttu-id="c8e87-157">Engedélyezett módosítások felvételét és rekordok eltávolítása hello rekordhalmaz, beleértve a eltávolítja az összes rögzíti tooleave egy "empty" rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8e87-157">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="c8e87-158">Hello hatást ugyanúgy hello bejegyzés törlése egy DNS-feloldási szempontból állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="c8e87-158">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="c8e87-159">Egyéni szerepkör-definíciók jelenleg nem definiált hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="c8e87-159">Custom role definitions cannot currently be defined via hello Azure portal.</span></span> <span data-ttu-id="c8e87-160">Egy egyéni biztonsági szerepkört a szerepkör-definíció alapján Azure PowerShell használatával hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="c8e87-160">A custom role based on this role definition can be created using Azure PowerShell:</span></span>

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

<span data-ttu-id="c8e87-161">Is létrehozhatók hello Azure parancssori felületen keresztül:</span><span class="sxs-lookup"><span data-stu-id="c8e87-161">It can also be created via hello Azure CLI:</span></span>

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

<span data-ttu-id="c8e87-162">hello szerepkör lehet hozzárendelni a hello azonos módon beépített szerepkörök, mint az ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="c8e87-162">hello role can then be assigned in hello same way as built-in roles, as described earlier in this article.</span></span>

<span data-ttu-id="c8e87-163">További információ a toocreate, hogyan kezelheti, és egyéni szerepkörök hozzárendelése: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-163">For more information on how toocreate, manage, and assign custom roles, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="resource-locks"></a><span data-ttu-id="c8e87-164">Erőforrás zárolások feloldása</span><span class="sxs-lookup"><span data-stu-id="c8e87-164">Resource locks</span></span>

<span data-ttu-id="c8e87-165">Továbbá tooRBAC, Azure Resource Manager támogatja más típusú biztonsági ellenőrzést, nevezetesen hello képességét too'lock "erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c8e87-165">In addition tooRBAC, Azure Resource Manager supports another type of security control, namely hello ability too'lock' resources.</span></span> <span data-ttu-id="c8e87-166">Amennyiben RBAC szabályok lehetővé teszik toocontrol hello műveletek a megadott felhasználók és csoportok, erőforrás zárolások alkalmazott toohello erőforrás, és hatékony összes felhasználók és szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="c8e87-166">Where RBAC rules allow you toocontrol hello actions of specific users and groups, resource locks are applied toohello resource, and are effective across all users and roles.</span></span> <span data-ttu-id="c8e87-167">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-167">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

<span data-ttu-id="c8e87-168">Erőforrás-zárolás két típusa van: **DoNotDelete** és **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="c8e87-168">There are two types of resource lock: **DoNotDelete** and **ReadOnly**.</span></span> <span data-ttu-id="c8e87-169">Ezek alkalmazható tooa DNS-zónát, vagy tooan egyedi rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8e87-169">These can be applied either tooa DNS zone, or tooan individual record set.</span></span>  <span data-ttu-id="c8e87-170">hello következő több gyakori forgatókönyvek forgatókönyveit mutatják be, és hogyan toosupport erőforrás zárolások őket.</span><span class="sxs-lookup"><span data-stu-id="c8e87-170">hello following sections describe several common scenarios, and how toosupport them using resource locks.</span></span>

### <a name="protecting-against-all-changes"></a><span data-ttu-id="c8e87-171">Minden változást elleni védelem</span><span class="sxs-lookup"><span data-stu-id="c8e87-171">Protecting against all changes</span></span>

<span data-ttu-id="c8e87-172">tooprevent módosításokat végeznek, a csak olvasható zárolási toohello zóna alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="c8e87-172">tooprevent any changes being made, apply a ReadOnly lock toohello zone.</span></span>  <span data-ttu-id="c8e87-173">Ez megakadályozza, hogy új rekordhalmazok létrehozása, és a meglévő rekordhalmazok módosítani vagy törlés alatt.</span><span class="sxs-lookup"><span data-stu-id="c8e87-173">This prevents new record sets from being created, and existing record sets from being modified or deleted.</span></span>

<span data-ttu-id="c8e87-174">Zóna szintű erőforrás zárolások hello Azure-portálon keresztül is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="c8e87-174">Zone level resource locks can be created via hello Azure portal.</span></span>  <span data-ttu-id="c8e87-175">A hello DNS-zóna paneljén kattintson az "Zárolások", majd "hozzáadása":</span><span class="sxs-lookup"><span data-stu-id="c8e87-175">From hello DNS zone blade, click 'Locks', then 'Add':</span></span>

![Zóna szintű erőforrás zárolások hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/locks1.png)

<span data-ttu-id="c8e87-177">Zóna szintű erőforrás zárolások is Azure PowerShell használatával hozható létre:</span><span class="sxs-lookup"><span data-stu-id="c8e87-177">Zone-level resource locks can also be created via Azure PowerShell:</span></span>

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

<span data-ttu-id="c8e87-178">Azure-erőforrás zárolásainak beállítása jelenleg nem támogatott hello Azure parancssori felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="c8e87-178">Configuring Azure resource locks is not currently supported via hello Azure CLI.</span></span>

### <a name="protecting-individual-records"></a><span data-ttu-id="c8e87-179">Az egyes rekordok védelme</span><span class="sxs-lookup"><span data-stu-id="c8e87-179">Protecting individual records</span></span>

<span data-ttu-id="c8e87-180">tooprevent egy meglévő DNS-rekordot szemben, állítsa be a csak olvasható zárolási toohello rekordhalmaz alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="c8e87-180">tooprevent an existing DNS record set against modification, apply a ReadOnly lock toohello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="c8e87-181">Alkalmazásakor a DoNotDelete zárolási tooa rekordhalmaz nem hatékony ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c8e87-181">Applying a DoNotDelete lock tooa record set is not an effective control.</span></span> <span data-ttu-id="c8e87-182">Megakadályozza, hogy a hello rekordhalmaz törlését, de nem akadályozza meg, már nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="c8e87-182">It prevents hello record set from being deleted, but it does not prevent it from being modified.</span></span>  <span data-ttu-id="c8e87-183">Engedélyezett módosítások felvételét és rekordok eltávolítása hello rekordhalmaz, beleértve a eltávolítja az összes rögzíti tooleave egy "empty" rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8e87-183">Permitted modifications include adding and removing records from hello record set, including removing all records tooleave an 'empty' record set.</span></span> <span data-ttu-id="c8e87-184">Hello hatást ugyanúgy hello bejegyzés törlése egy DNS-feloldási szempontból állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="c8e87-184">This has hello same effect as deleting hello record set from a DNS resolution viewpoint.</span></span>

<span data-ttu-id="c8e87-185">Rekordhalmaz szintű erőforrás zárolások is jelenleg csak Azure PowerShell használatával konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="c8e87-185">Record set level resource locks can currently only be configured using Azure PowerShell.</span></span>  <span data-ttu-id="c8e87-186">Ezek nem támogatottak a hello Azure-portálon vagy az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="c8e87-186">They are not supported in hello Azure portal or Azure CLI.</span></span>

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a><span data-ttu-id="c8e87-187">Zóna törlés elleni védelem</span><span class="sxs-lookup"><span data-stu-id="c8e87-187">Protecting against zone deletion</span></span>

<span data-ttu-id="c8e87-188">Az Azure DNS-zóna törlése után minden rekordkészletet hello zónában is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="c8e87-188">When a zone is deleted in Azure DNS, all record sets in hello zone are also deleted.</span></span>  <span data-ttu-id="c8e87-189">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="c8e87-189">This operation cannot be undone.</span></span>  <span data-ttu-id="c8e87-190">A kritikus zóna véletlen törlése hello lehetséges toohave egy jelentős üzleti hatással van.</span><span class="sxs-lookup"><span data-stu-id="c8e87-190">Accidentally deleting a critical zone has hello potential toohave a significant business impact.</span></span>  <span data-ttu-id="c8e87-191">Éppen ezért rendkívül fontos tooprotect zóna véletlen törlése ellen.</span><span class="sxs-lookup"><span data-stu-id="c8e87-191">It is therefore very important tooprotect against accidental zone deletion.</span></span>

<span data-ttu-id="c8e87-192">DoNotDelete zárolási tooa zóna alkalmazása megakadályozza, hogy hello zóna törlés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="c8e87-192">Applying a DoNotDelete lock tooa zone prevents hello zone from being deleted.</span></span>  <span data-ttu-id="c8e87-193">Azonban mivel zárolások gyermekszintű erőforrása által örökölt, is megakadályozza, hogy bármely rekordhalmazok hello zónában törlését, esetlegesen nemkívánatos.</span><span class="sxs-lookup"><span data-stu-id="c8e87-193">However, since locks are inherited by child resources, it also prevents any record sets in hello zone from being deleted, which may be undesirable.</span></span>  <span data-ttu-id="c8e87-194">Ezenkívül hello Megjegyzés: a fenti leírtak egyben hatástalan óta rekordok továbbra is eltávolítható hello meglévő rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="c8e87-194">Furthermore, as described in hello note above, it is also ineffective since records can still be removed from hello existing record sets.</span></span>

<span data-ttu-id="c8e87-195">A másik lehetőség fontolja meg a hello zónában, például a hello SOA típusú rekordhalmaz DoNotDelete zárolási tooa rekord alkalmazása beállítása.</span><span class="sxs-lookup"><span data-stu-id="c8e87-195">As an alternative, consider applying a DoNotDelete lock tooa record set in hello zone, such as hello SOA record set.</span></span>  <span data-ttu-id="c8e87-196">Hello zóna hello rekordhalmazok is törlése nélkül nem lehet törölni, mivel ez zóna törlése, miközben továbbra is lehetővé teszi a rekordhalmazok belül módosított szabadon hello zóna toobe ellen védelmet nyújt.</span><span class="sxs-lookup"><span data-stu-id="c8e87-196">Since hello zone cannot be deleted without also deleting hello record sets, this protects against zone deletion, while still allowing record sets within hello zone toobe modified freely.</span></span> <span data-ttu-id="c8e87-197">Ha toodelete hello zóna tett kísérlet, Azure Resource Manager észleli, ez is törölni hello SOA típusú rekordhalmaz, és blokkok hello hívás, mert hello SOA zárolva van.</span><span class="sxs-lookup"><span data-stu-id="c8e87-197">If an attempt is made toodelete hello zone, Azure Resource Manager detects this would also delete hello SOA record set, and blocks hello call because hello SOA is locked.</span></span>  <span data-ttu-id="c8e87-198">Nincs rekordhalmazok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="c8e87-198">No record sets are deleted.</span></span>

<span data-ttu-id="c8e87-199">a következő PowerShell-paranccsal hello elleni hello SOA típusú rekordjának zóna megadott hello DoNotDelete zárolást hoz létre:</span><span class="sxs-lookup"><span data-stu-id="c8e87-199">hello following PowerShell command creates a DoNotDelete lock against hello SOA record of hello given zone:</span></span>

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

<span data-ttu-id="c8e87-200">Másik módja tooprevent véletlen zóna törlésre van egy egyéni biztonsági szerepkört tooensure hello operátor és a szolgáltatás által használt fiókok toomanage a zónák zóna nem rendelkezik engedélyekkel törlése.</span><span class="sxs-lookup"><span data-stu-id="c8e87-200">Another way tooprevent accidental zone deletion is by using a custom role tooensure hello operator and service accounts used toomanage your zones do not have zone delete permissions.</span></span> <span data-ttu-id="c8e87-201">Ha a zóna toodelete van szükség, kényszerítheti a egy kétlépéses törlése, első támogatást nyújtó zóna törlése (hatókörben hello zóna, tooprevent hello megfelelő zóna törlése) és a második toodelete hello zóna.</span><span class="sxs-lookup"><span data-stu-id="c8e87-201">When you do need toodelete a zone, you can enforce a two-step delete, first granting zone delete permissions (at hello zone scope, tooprevent deleting hello wrong zone) and second toodelete hello zone.</span></span>

<span data-ttu-id="c8e87-202">A második megközelítés azzal hello előnnyel jár, a működését, az összes olyan zónára, ezek a fiókok, anélkül, hogy tooremember toocreate bármely zárolások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c8e87-202">This second approach has hello advantage that it works for all zones accessed by those accounts, without having tooremember toocreate any locks.</span></span> <span data-ttu-id="c8e87-203">Hello hátránya, hogy egyetlen fiókot zóna törlése engedélyekkel, például az előfizetés tulajdonosa hello, még véletlenül törölheti a kritikus zóna rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c8e87-203">It has hello disadvantage that any accounts with zone delete permissions, such as hello subscription owner, can still accidentally delete a critical zone.</span></span>

<span data-ttu-id="c8e87-204">Már lehetséges toouse mindkét megközelítés - erőforrás zárolások és egyéni szerepkör -: hello azonos idő, mint egy védelmi jellegű megközelítés tooDNS zóna protection.</span><span class="sxs-lookup"><span data-stu-id="c8e87-204">It is possible toouse both approaches - resource locks and custom roles - at hello same time, as a defense-in-depth approach tooDNS zone protection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8e87-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8e87-205">Next steps</span></span>

* <span data-ttu-id="c8e87-206">Az RBAC használatáról további információk: [kezelési hello Azure-portálon az első lépések](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-206">For more information about working with RBAC, see [Get started with access management in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>
* <span data-ttu-id="c8e87-207">Működő erőforrás zárral kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c8e87-207">For more information about working with resource locks, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

