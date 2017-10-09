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
# <a name="how-tooprotect-dns-zones-and-records"></a>Hogyan tooprotect DNS zónák, valamint megjegyzi

DNS-zónák és rekordok a fontos erőforrásokhoz. A DNS-zónához vagy akár csak egyetlen DNS-rekord törlése azt eredményezheti, hogy a teljes szolgáltatáskimaradás.  Ezért fontos, hogy a kritikus DNS-zónák és rekordok védett véletlen vagy illetéktelen módosítások ellen.

Ez a cikk azt ismerteti, hogyan Azure DNS-ben lehetővé teszi, hogy Ön tooprotect a DNS-zónák és a változások alapján.  Azure Resource Manager által biztosított két hatékony funkciókat érvénybe lépni: [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) és [erőforrás zárolások](../azure-resource-manager/resource-group-lock-resources.md).

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Azure szerepköralapú hozzáférés-vezérlés (RBAC) lehetővé teszi, hogy a részletes hozzáféréskezelést az Azure-felhasználók, csoportok és erőforrásokat. Szerepalapú használ, meg lehet adni pontosan hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat. Hogyan segít az RBAC-hozzáférés kezelése kapcsolatos további információkért lásd: [Mi az szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).

### <a name="hello-dns-zone-contributor-role"></a>hello "DNS-zóna közreműködői" szerepkör

hello "DNS-zóna közreműködői" szerepkör a rendszer beépített Azure által biztosított DNS-erőforrások kezelése.  DNS-zóna közreműködői engedélyekkel tooa felhasználó vagy csoport hozzárendelése lehetővé teszi, hogy a csoport toomanage DNS erőforrásokat, de nem bármilyen más típusú erőforrásokra.

Tegyük fel, hogy hello erőforrás csoport "myzones" Contoso Corporation öt zónák tartalmazza. Támogatást nyújtó hello DNS rendszergazda "DNS-zóna közreműködői" engedélyek toothat erőforráscsoport, lehetővé teszi, hogy ezeket a DNS-zónák teljes ellenőrzést. Azt is elkerüli a szükségtelen engedélyeket ad, például a hello DNS-rendszergazda nem hozható létre, vagy állítsa le a virtuális gépek.

hello legegyszerűbb módja tooassign RBAC bizton [hello Azure-portálon keresztül](../active-directory/role-based-access-control-configure.md).  Hello erőforráscsoport, hello "hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás" hello "DNS-zóna közreműködői" szerepkör és a szükséges válassza hello felhasználók vagy csoportok toogrant engedélyek.

![Erőforráscsoport szintjén RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac1.png)

Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>"
```

hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooall zones in a resource group
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --resourceGroup "<resource group name>"
```

### <a name="zone-level-rbac"></a>Zóna szintjét RBAC

Az Azure RBAC-szabályok lehet alkalmazott tooa előfizetés erőforrás csoport vagy tooan egyéni erőforrás. Az Azure DNS hello esetben, hogy az erőforrás egy egyéni DNS-zónát, vagy még egyedi rekordhalmaz lehet.

Tegyük fel, hogy hello erőforrás csoport "myzones" hello "contoso.com" zóna és a subzone "customers.contoso.com", amelyben a CNAME-rekordok létrejönnek az egyes felhasználói fiókokhoz tartalmazza.  hello használt fiók toomanage ezeket a CNAME rekordokat hozzá kell rendelni engedélyek toocreate rekordok hello "customers.contoso.com" zónában, nem rendelkezhet hozzáférés toohello más zónák.

Zónaszintű RBAC hozzárendelhető hello Azure-portálon keresztül.  Hello zóna hello "Hozzáférés-vezérlés (IAM)" panel megnyitásához, majd kattintson a "Hozzáadás" hello "DNS-zóna közreműködői" szerepkör és a szükséges válassza hello felhasználók vagy csoportok toogrant engedélyek.

![DNS-zóna szintű RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac2.png)

Engedélyek is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant 'DNS Zone Contributor' permissions tooa specific zone
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -ResourceGroupName "<resource group name>" -ResourceName "<zone name>" -ResourceType Microsoft.Network/DNSZones
```

hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant 'DNS Zone Contributor' permissions tooa specific zone
azure role assignment create --signInName <user email address> --roleName "DNS Zone Contributor" --resource-name <zone name> --resource-type Microsoft.Network/DNSZones --resource-group <resource group name>
```

### <a name="record-set-level-rbac"></a>A rekordhalmaz RBAC szint

Azt is egy lépéssel további. Vegye figyelembe a hello mail rendszergazdának Contoso Corporation, az access toohello MX és TXT rekord hello csúcsán hello "contoso.com" zóna.  Azt nem kell elérni tooany más MX vagy TXT-rekordot, vagy bármilyen más típusú tooany rekord.  Az Azure DNS lehetővé teszi, hogy tooassign hello rekordhalmaz szint tooprecisely hello azt jelzi, hogy hello mail rendszergazdai engedélyekkel kell elérnie.  hello mail rendszergazda kap hello vezérlő pontosan ugyanúgy kell, és nem toomake bármilyen más jellegű módosítását.

Rekordhalmaz szintű RBAC engedélyek állíthatók hello Azure-portálon, a "Hello"felhasználók"gombra kattintva hello a rekordkészlet panelen:

![A rekordhalmaz szint RBAC hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/rbac3.png)

Rekordhalmaz szintű RBAC engedélyeket is lehet [kap, az Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md):

```powershell
# Grant permissions tooa specific record set
New-AzureRmRoleAssignment -SignInName "<user email address>" -RoleDefinitionName "DNS Zone Contributor" -Scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

hello egyenértékű parancs egyben [hello Azure parancssori felületen keresztül elérhető](../active-directory/role-based-access-control-manage-access-azure-cli.md):

```azurecli
# Grant permissions tooa specific record set
azure role assignment create --signInName "<user email address>" --roleName "DNS Zone Contributor" --scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Egyéni szerepkörök

hello beépített "DNS-zóna közreműködői" szerepkör lehetővé teszi, hogy a DNS-erőforrásrekordok teljes ellenőrzést. Ez egyben lehetséges toobuild a saját Azure felhasználói szerepkörök, tooprovide még pontosabban megbízhatatlanná vezérlő.

Érdemes újra hello példában, amelyben hello zóna "customers.contoso.com" CNAME rekord jön létre az egyes Contoso Corporation felhasználói fiókokhoz.  hello használt fiók toomanage ezek CNAME adható engedély toomanage CNAME rekordok csak.  Ezt követően nem toomodify rekordok egyéb (például az MX-rekordok módosítása) meg kell adnia, vagy hajtsa végre a zóna szintű műveletek, például a zóna törlése van.

a következő példa hello CNAME rekordok csak kezelésére egy egyéni szerepkör-definíció látható:

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

hello műveletek tulajdonság határozza meg az alábbi DNS-specifikus engedélyek hello:

* `Microsoft.Network/dnsZones/CNAME/*`teljes hozzáférést biztosít a CNAME-rekordok
* `Microsoft.Network/dnsZones/read`engedélyezi a engedély tooread DNS-zónák, de nem toomodify őket, hogy toosee engedélyezése hello zóna mely hello a CNAME létrehozása folyamatban van.

fennmaradó műveletek hello átmásolva hello [DNS-zóna közreműködői beépített szerepkör](../active-directory/role-based-access-built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Egy egyéni RBAC szerepkör tooprevent bejegyzés törlése használatával beállítja a pedig továbbra is engedélyezi azok frissített toobe nem hatékony ellenőrzése. Megakadályozza, hogy rekordhalmazok törlés alatt áll, de nem akadályozza meg azok már nem módosítható.  Engedélyezett módosítások felvételét és rekordok eltávolítása hello rekordhalmaz, beleértve a eltávolítja az összes rögzíti tooleave egy "empty" rekordhalmaz. Hello hatást ugyanúgy hello bejegyzés törlése egy DNS-feloldási szempontból állítsa be azt.

Egyéni szerepkör-definíciók jelenleg nem definiált hello Azure-portálon keresztül. Egy egyéni biztonsági szerepkört a szerepkör-definíció alapján Azure PowerShell használatával hozhatók létre:

```powershell
# Create new role definition based on input file
New-AzureRmRoleDefinition -InputFile <file path>
```

Is létrehozhatók hello Azure parancssori felületen keresztül:

```azurecli
# Create new role definition based on input file
azure role create -inputfile <file path>
```

hello szerepkör lehet hozzárendelni a hello azonos módon beépített szerepkörök, mint az ebben a cikkben leírtak szerint.

További információ a toocreate, hogyan kezelheti, és egyéni szerepkörök hozzárendelése: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).

## <a name="resource-locks"></a>Erőforrás zárolások feloldása

Továbbá tooRBAC, Azure Resource Manager támogatja más típusú biztonsági ellenőrzést, nevezetesen hello képességét too'lock "erőforrásokat. Amennyiben RBAC szabályok lehetővé teszik toocontrol hello műveletek a megadott felhasználók és csoportok, erőforrás zárolások alkalmazott toohello erőforrás, és hatékony összes felhasználók és szerepkörök. További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](../azure-resource-manager/resource-group-lock-resources.md).

Erőforrás-zárolás két típusa van: **DoNotDelete** és **ReadOnly**. Ezek alkalmazható tooa DNS-zónát, vagy tooan egyedi rekordhalmaz.  hello következő több gyakori forgatókönyvek forgatókönyveit mutatják be, és hogyan toosupport erőforrás zárolások őket.

### <a name="protecting-against-all-changes"></a>Minden változást elleni védelem

tooprevent módosításokat végeznek, a csak olvasható zárolási toohello zóna alkalmazni.  Ez megakadályozza, hogy új rekordhalmazok létrehozása, és a meglévő rekordhalmazok módosítani vagy törlés alatt.

Zóna szintű erőforrás zárolások hello Azure-portálon keresztül is létrehozható.  A hello DNS-zóna paneljén kattintson az "Zárolások", majd "hozzáadása":

![Zóna szintű erőforrás zárolások hello Azure-portálon keresztül](./media/dns-protect-zones-recordsets/locks1.png)

Zóna szintű erőforrás zárolások is Azure PowerShell használatával hozható létre:

```powershell
# Lock a DNS zone
New-AzureRmResourceLock -LockLevel <lock level> -LockName <lock name> -ResourceName <zone name> -ResourceType Microsoft.Network/DNSZones -ResourceGroupName <resource group name>
```

Azure-erőforrás zárolásainak beállítása jelenleg nem támogatott hello Azure parancssori felületen keresztül.

### <a name="protecting-individual-records"></a>Az egyes rekordok védelme

tooprevent egy meglévő DNS-rekordot szemben, állítsa be a csak olvasható zárolási toohello rekordhalmaz alkalmazni.

> [!NOTE]
> Alkalmazásakor a DoNotDelete zárolási tooa rekordhalmaz nem hatékony ellenőrzése. Megakadályozza, hogy a hello rekordhalmaz törlését, de nem akadályozza meg, már nem módosítható.  Engedélyezett módosítások felvételét és rekordok eltávolítása hello rekordhalmaz, beleértve a eltávolítja az összes rögzíti tooleave egy "empty" rekordhalmaz. Hello hatást ugyanúgy hello bejegyzés törlése egy DNS-feloldási szempontból állítsa be azt.

Rekordhalmaz szintű erőforrás zárolások is jelenleg csak Azure PowerShell használatával konfigurálhatók.  Ezek nem támogatottak a hello Azure-portálon vagy az Azure parancssori felület.

```powershell
# Lock a DNS record set
New-AzureRmResourceLock -LockLevel <lock level> -LockName "<lock name>" -ResourceName "<zone name>/<record set name>" -ResourceType "Microsoft.Network/DNSZones/<record type>" -ResourceGroupName "<resource group name>"
```

### <a name="protecting-against-zone-deletion"></a>Zóna törlés elleni védelem

Az Azure DNS-zóna törlése után minden rekordkészletet hello zónában is törlődnek.  Ez a művelet nem vonható vissza.  A kritikus zóna véletlen törlése hello lehetséges toohave egy jelentős üzleti hatással van.  Éppen ezért rendkívül fontos tooprotect zóna véletlen törlése ellen.

DoNotDelete zárolási tooa zóna alkalmazása megakadályozza, hogy hello zóna törlés alatt áll.  Azonban mivel zárolások gyermekszintű erőforrása által örökölt, is megakadályozza, hogy bármely rekordhalmazok hello zónában törlését, esetlegesen nemkívánatos.  Ezenkívül hello Megjegyzés: a fenti leírtak egyben hatástalan óta rekordok továbbra is eltávolítható hello meglévő rekordhalmazok.

A másik lehetőség fontolja meg a hello zónában, például a hello SOA típusú rekordhalmaz DoNotDelete zárolási tooa rekord alkalmazása beállítása.  Hello zóna hello rekordhalmazok is törlése nélkül nem lehet törölni, mivel ez zóna törlése, miközben továbbra is lehetővé teszi a rekordhalmazok belül módosított szabadon hello zóna toobe ellen védelmet nyújt. Ha toodelete hello zóna tett kísérlet, Azure Resource Manager észleli, ez is törölni hello SOA típusú rekordhalmaz, és blokkok hello hívás, mert hello SOA zárolva van.  Nincs rekordhalmazok törlődnek.

a következő PowerShell-paranccsal hello elleni hello SOA típusú rekordjának zóna megadott hello DoNotDelete zárolást hoz létre:

```powershell
# Protect against zone delete with DoNotDelete lock on hello record set
New-AzureRmResourceLock -LockLevel DoNotDelete -LockName "<lock name>" -ResourceName "<zone name>/@" -ResourceType" Microsoft.Network/DNSZones/SOA" -ResourceGroupName "<resource group name>"
```

Másik módja tooprevent véletlen zóna törlésre van egy egyéni biztonsági szerepkört tooensure hello operátor és a szolgáltatás által használt fiókok toomanage a zónák zóna nem rendelkezik engedélyekkel törlése. Ha a zóna toodelete van szükség, kényszerítheti a egy kétlépéses törlése, első támogatást nyújtó zóna törlése (hatókörben hello zóna, tooprevent hello megfelelő zóna törlése) és a második toodelete hello zóna.

A második megközelítés azzal hello előnnyel jár, a működését, az összes olyan zónára, ezek a fiókok, anélkül, hogy tooremember toocreate bármely zárolások érhetők el. Hello hátránya, hogy egyetlen fiókot zóna törlése engedélyekkel, például az előfizetés tulajdonosa hello, még véletlenül törölheti a kritikus zóna rendelkezik.

Már lehetséges toouse mindkét megközelítés - erőforrás zárolások és egyéni szerepkör -: hello azonos idő, mint egy védelmi jellegű megközelítés tooDNS zóna protection.

## <a name="next-steps"></a>Következő lépések

* Az RBAC használatáról további információk: [kezelési hello Azure-portálon az első lépések](../active-directory/role-based-access-control-what-is.md).
* Működő erőforrás zárral kapcsolatos további információkért lásd: [erőforrások az Azure Resource Manager zárolása](../azure-resource-manager/resource-group-lock-resources.md).

