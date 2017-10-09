---
title: aaaUsing PowerShell toomanage az Azure Traffic Manager |} Microsoft Docs
description: "A Traffic Manager az Azure Resource Manager eszközzel PowerShell használatával"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a>PowerShell toomanage Traffic Manager használatával

Az Azure Resource Manager rendszer hello előnyben részesített felügyeleti szolgáltatások az Azure-ban, Az Azure Traffic Manager-profilok használatával az Azure Resource Manager-alapú API-k és eszközök is felügyelhetők.

## <a name="resource-model"></a>Erőforrás-modellje

Az Azure Traffic Manager a Traffic Manager-profil neve beállítások segítségével konfigurálható. Ez a profil tartalmazza a DNS-beállítások, a forgalom útválasztásához tartozó beállításokat, a figyelési beállításokat végpont, és a végpontok toowhich forgalmának listáját történik.

Minden egyes Traffic Manager-profil "TrafficManagerProfiles" típusú erőforrást jelképezi. A hello REST API-szintet az egyes profilok URI hello a következőképpen történik:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Azure PowerShell telepítése

Ezek az utasítások a Microsoft Azure PowerShell használata. hello alábbi cikk azt ismerteti, hogyan tooinstall Azure PowerShell és konfigurálása.

* [Hogyan tooinstall Azure PowerShell és konfigurálása](/powershell/azure/overview)

hello a cikkben szereplő példák azt feltételezik, hogy rendelkezik-e egy meglévő erőforráscsoportot. Létrehozhat egy olyan erőforráscsoport, a következő parancs hello használata:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Az Azure Resource Manager megköveteli, hogy a hely összes erőforráscsoport. Ezen a helyen hello alapértelmezett szolgál az erőforráscsoport létrejött erőforrásokat. Azonban mivel a Traffic Manager-profil erőforrások globális, nem pedig regionális, hello választás az erőforráscsoport helye nincs hatással van az Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>A Traffic Manager-profil létrehozása

Traffic Manager-profil toocreate hello használata `New-AzureRmTrafficManagerProfile` parancsmagot:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

a következő táblázat hello hello paramétereket ismerteti:

| Paraméter | Leírás |
| --- | --- |
| Név |a Traffic Manager-profil erőforrás hello hello erőforrás neve. Profilok hello az azonos erőforráscsoport neve csak egyedi lehet. Ez a név nem azonos DNS-lekérdezések használt hello DNS-név. |
| erőforráscsoport-név |hello hello erőforrás csoportot tartalmazó hello profil erőforrás neve. |
| TrafficRoutingMethod |Hello forgalom-útválasztási használt módszer toodetermine melyik végponthoz válaszként visszaadott a DNS-lekérdezés megadása Lehetséges értékek: "Teljesítmény", "Weighted" vagy "Priority". |
| RelativeDnsName |Adja meg a Traffic Manager-profil által meghatározott hello DNS-név hello hostname része. Ez az érték hello Azure Traffic Manager tooform hello teljesen minősített tartománynevét (FQDN) hello-profil által használt DNS-tartománynév együtt. Például "contoso" hello érték lesz "contoso.trafficmanager.net." |
| ÉLETTARTAM |Hello DNS idő élettartamát (TTL), másodpercben. A TTL arról tájékoztatja a helyi DNS-feloldókat hello és a DNS-ügyfelek mennyi ideig toocache DNS-válaszok a Traffic Manager-profil. |
| MonitorProtocol |Hello protokoll toouse toomonitor végpont állapota határozza meg. Lehetséges értékek: "HTTP" és "HTTPS". |
| MonitorPort |Meghatározza, hello TCP-portot használja toomonitor végpont állapotát. |
| MonitorPath |Megadja, hello elérési útja relatív toohello végpont tartománynév tooprobe használandó végpont állapotát. |

hello parancsmag Traffic Manager-profil létrehozása az Azure-ban, és a megfelelő profil objektum tooPowerShell adja vissza. Ezen a ponton hello-profil nem tartalmaz végpontok. Végpontok tooa Traffic Manager-profil hozzáadásával kapcsolatos további információkért lásd: [Traffic Manager-végpontok hozzáadása](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Traffic Manager-profil beolvasása

egy meglévő Traffic Manager-profil objektumban, tooretrieve hello használata `Get-AzureRmTrafficManagerProfle` parancsmagot:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Ez a parancsmag egy Traffic Manager-profil objektumot ad vissza.

## <a name="update-a-traffic-manager-profile"></a>Traffic Manager-profil frissítése

Traffic Manager-profilok módosítása a 3. lépés folyamatot követi:

1. Kérje le hello-profil használatával `Get-AzureRmTrafficManagerProfile` , vagy használjon hello-profil által visszaadott `New-AzureRmTrafficManagerProfile`.
2. Hello módosításához. Adja hozzá, és távolítsa el a végpontok, vagy módosítsa a végpont vagy profil paraméterek. A kapcsolat nélküli műveletek, amelyek ezeket a módosításokat. Csak módosítjuk hello helyi objektum a memóriában, amely hello-profil jelöli.
3. Hello segítségével a változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerProfile` parancsmag.

Az összes profil tulajdonságai hello-profil RelativeDnsName kivéve módosíthatók. toochange hello RelativeDnsName, törölnie kell a profil és egy új profilt egy új nevet.

hello a következő példa bemutatja, hogyan toochange hello-profil élettartam:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

A Traffic Manager-végpontok három típusa van:

1. **Azure-végpontok** Azure-ban üzemeltetett szolgáltatások
2. **Külső végpontok száma** Azure-on kívüli üzemeltetett szolgáltatások
3. **A beágyazott végpontok** beágyazott használt tooconstruct hierarchia a Traffic Manager-profilokat. Beágyazott végpontok összetett alkalmazások speciális forgalom-útválasztási konfigurációi engedélyezése.

A három esetben végpontok hozzáadása is lehetséges két módon:

1. A fentiekben ismertetett 3. lépés folyamatok használata. hello ezt a módszert előnye, hogy több végpont módosítás egy frissítés.
2. Hello New-AzureRmTrafficManagerEndpoint parancsmag használatával. Ez a parancsmag egy végpont tooan meglévő Traffic Manager-profil hozzáadása egy művelettel.

## <a name="adding-azure-endpoints"></a>Azure-végpontok hozzáadása

Azure-végpontok Azure-ban üzemeltetett szolgáltatások hivatkozik. Azure-végpontok két típusú támogatottak:

1. Azure Web Apps
2. Az Azure nyilvános erőforrások (csatolt tooa terheléselosztó és a virtuális gép hálózati adapter is lehet). hello PublicIpAddress hozzárendelt toobe használja őket a Traffic Manager DNS névvel kell rendelkeznie.

Minden esetben:

* hello "targetresourceid azonosítója" paraméterének hello szolgáltatás adott `Add-AzureRmTrafficManagerEndpointConfig` vagy `New-AzureRmTrafficManagerEndpoint`.
* hello "Target" és "EndpointLocation" vet hello targetresourceid azonosítója által.
* Hello "Súly" megadása nem kötelező. Súlyozással beállított toouse hello "Weighted" forgalom-útválasztási módszer esetén is hello-profil csak használt. Ellenkező esetben nem veszi őket figyelembe. Ha meg van adva, a hello érték 1 és 1000 közötti számnak kell lennie. hello alapértelmezett értéke "1".
* Adja meg hello "Priority" megadása nem kötelező. Prioritások csak használhatók, ha hello-profil konfigurált toouse hello "Priority" forgalom-útválasztási módszer. Ellenkező esetben nem veszi őket figyelembe. Érvényes értékek: 1 too1000 a magasabb prioritású jelző alacsonyabb értékeket. Ha meg van adva egy végpontot, azok összes végpontok adható meg. Ha nincs megadva, "1"-től kezdődő alapértelmezett értékeket, hogy látható-e a hello végpontok hello sorrendben alkalmazza.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>1. példa: A webalkalmazás végpontjaitól hozzáadása`Add-AzureRmTrafficManagerEndpointConfig`

Ebben a példában azt a Traffic Manager-profil létrehozása, és adja hozzá a két Web App végpontjaitól hello `Add-AzureRmTrafficManagerEndpointConfig` parancsmag.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>2. példa: Hozzáadása egy nyilvános végpontot a`New-AzureRmTrafficManagerEndpoint`

Ebben a példában egy nyilvános IP-cím erőforrás kerül toohello Traffic Manager-profil. hello nyilvános IP-cím rendelkeznie kell egy DNS-név konfigurálva, és lehet kötött vagy toohello VM vagy tooa terheléselosztó hálózati adapter.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Külső végpontok hozzáadása

Külső végpontok száma toodirect forgalom tooservices tárolt Azure-on kívüli Manager által használt forgalom. Mivel az Azure-végpontok, külső végpontok hozzáadása is lehetséges vagy használatával `Add-AzureRmTrafficManagerEndpointConfig` követ `Set-AzureRmTrafficManagerProfile`, vagy `New-AzureRMTrafficManagerEndpoint`.

Külső végpontok száma megadásakor:

* hello végpont tartománynév hello "Target" paraméter használatával kell megadni
* Hello "Teljesítmény" forgalom-útválasztási módszer használata esetén a "EndpointLocation" hello megadása kötelező. Ellenkező esetben nem kötelező. hello értéknek kell lennie egy [érvényes Azure-régiót neve](https://azure.microsoft.com/regions/).
* hello "Súlyozási" és "Priority" választható.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>1. példa: Használó külső végpontok hozzáadása `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`

Ebben a példában azt Traffic Manager-profil létrehozása, vegye fel a két külső végpont és hello változtatások véglegesítése a határidő.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>2. példa: Hozzáadása használata külső végpontok száma`New-AzureRmTrafficManagerEndpoint`

Ebben a példában a Microsoft egy külső végpont tooan meglévő profil hozzáadásához. hello-profil van megadva, hello-profil és az erőforrás nevének használatával.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>"Nested" végpontok hozzáadása

Minden egyes Traffic Manager-profil megadja egy forgalom-útválasztási módszert. Vannak azonban forgatókönyv esetén van szükség, kifinomultabb a forgalom útválasztásához, mint egyetlen Traffic Manager-profil által biztosított hello útválasztást. A Traffic Manager profilok toocombine hello előnyei egynél több forgalom-útválasztási módszert ágyazhatja be. Beágyazott profilok lehetővé teszik toooverride hello alapértelmezett Traffic Manager viselkedés toosupport nagyobb és összetettebb alkalmazások központi telepítéseit. További részletes példák: [beágyazott Traffic Manager-profilok](traffic-manager-nested-profiles.md).

Beágyazott végpontok hello szülő profilnál, egy adott típusú végpont, "NestedEndpoints" használatával vannak konfigurálva. Beágyazott végpontok megadásakor:

* hello végpont hello "targetresourceid azonosítója" paraméter használatával kell megadni
* Hello "Teljesítmény" forgalom-útválasztási módszer használata esetén a "EndpointLocation" hello megadása kötelező. Ellenkező esetben nem kötelező. hello értéknek kell lennie egy [érvényes Azure-régiót neve](http://azure.microsoft.com/regions/).
* hello "Súlyozási" és "Priority" nem kötelező megadni, mint az Azure-végpontok.
* hello "MinChildEndpoints" paraméter nem kötelező. hello alapértelmezett értéke "1". Ha hello elérhető végpontok száma a küszöbérték alá esik, hello szülő profil úgy véli, hello gyermek profil "csökkentett teljesítményű", és hozunk forgalom toohello hello szülő profil végpontja.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>1. példa: Hozzáadása beágyazott végpontjaitól `Add-AzureRmTrafficManagerEndpointConfig` és`Set-AzureRmTrafficManagerProfile`

Ebben a példában a Microsoft új Traffic Manager gyermek és szülő profilok létrehozása hello gyermek hozzáadása beágyazott végpont toohello szülőjeként és hello változtatások véglegesítése a határidő.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Ebben a példában kivonatosan mutatja bármely más végpontok toohello gyermek vagy szülő profilok nem azt adta hozzá.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>2. példa: Használó beágyazott végpontok hozzáadása`New-AzureRmTrafficManagerEndpoint`

Ebben a példában jelenleg egy meglévő gyermek-profilt a beágyazott végpont tooan meglévő szülő tanúsítványprofilként kell felvenni. hello-profil van megadva, hello-profil és az erőforrás nevének használatával.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a>A Traffic Manager-végpont frissítése

Számos két módon tooupdate egy meglévő Traffic Manager-végpontot.

1. Első hello Traffic Manager-profil használatával `Get-AzureRmTrafficManagerProfile`hello profilon belül hello végpont tulajdonságainak frissítéséhez és használatával hello változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerProfile`. Ez a módszer hello előnye, hogy képes tooupdate egy művelettel több végpont rendelkezik.
2. Első hello Traffic Manager-végpontot a `Get-AzureRmTrafficManagerEndpoint`hello végpont tulajdonságainak frissítéséhez és használatával hello változtatások véglegesítése a határidő `Set-AzureRmTrafficManagerEndpoint`. Ez a módszer is egyszerűbb, mivel nem igényel indexelő hello-profil hello végpontok tömbbe.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>1. példa: Frissítése végpontjaitól `Get-AzureRmTrafficManagerProfile` és`Set-AzureRmTrafficManagerProfile`

Ebben a példában azt módosítani egy meglévő profilt belül két végpontokon hello prioritású virtuális gép.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>2. példa: Frissítése egy végpontot a `Get-AzureRmTrafficManagerEndpoint` és`Set-AzureRmTrafficManagerEndpoint`

Ebben a példában azt módosítani egy meglévő profil egyetlen végpont súlyának hello.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Engedélyezése és letiltása a végpontok és profilok

A TRAFFIC Manager lehetővé teszi, hogy az egyes végpontok toobe engedélyezve van, és le van tiltva, valamint engedélyezése és letiltása teljes profilok engedélyezése.
Ezek a változások első/frissítése/beállítás hello végpont vagy profil erőforrásokat is végezhető. toostreamline e közös műveletek támogatottak továbbá akkor dedikált parancsmagok segítségével.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>1. példa: Engedélyezése és letiltása Traffic Manager-profil

Traffic Manager-profil tooenable használja `Enable-AzureRmTrafficManagerProfile`. hello-profil egy profil objektum adható meg. hello-profil objektumban átadhatók keresztül hello csővezeték vagy hello segítségével "-TrafficManagerProfile" paraméter. Ebben a példában azt adja meg hello-profil hello-profil és az erőforrás neve.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Traffic Manager-profil toodisable:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

hello Disable-AzureRmTrafficManagerProfile parancsmag megerősítés kéri. Ez a kérdés hello letilthatók "-Force" paraméter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>2. példa: Engedélyezése és letiltása Traffic Manager-végpontot

a Traffic Manager-végpont tooenable használja `Enable-AzureRmTrafficManagerEndpoint`. Nincsenek a két módon toospecify hello végpont

1. Egy hello csővezeték átadása TrafficManagerEndpoint objektum használatával vagy az hello "-TrafficManagerEndpoint" paraméter
2. Hello a végpont neve, a típusú végpont, a profil nevét és a erőforráscsoport-név használatával:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Ehhez hasonlóan toodisable egy Traffic Manager-végpont:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

A `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` parancsmag jóváhagyást kér. Ez a kérdés hello letilthatók "-Force" paraméter.

## <a name="delete-a-traffic-manager-endpoint"></a>A Traffic Manager-végpont törlése

tooremove egyedi végpontok, használja a hello `Remove-AzureRmTrafficManagerEndpoint` parancsmagot:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Ez a parancsmag felszólítja megerősítést kér. Ez a kérdés hello letilthatók "-Force" paraméter.

## <a name="delete-a-traffic-manager-profile"></a>Traffic Manager-profil törlése

Traffic Manager-profil toodelete hello használata `Remove-AzureRmTrafficManagerProfile` parancsmag hello-profil és az erőforrás nevének megadása:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Ez a parancsmag felszólítja megerősítést kér. Ez a kérdés hello letilthatók "-Force" paraméter.

törölt hello-profil toobe is is meg kell adni egy profil objektummal:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Ebben a sorozatban is átirányítható:

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Következő lépések

[A TRAFFIC Manager figyelése](traffic-manager-monitoring.md)

[A Traffic Manager teljesítményével kapcsolatos megfontolások](traffic-manager-performance-considerations.md)
