---
title: "aaaManage Azure fenntartott IP-cím (klasszikus) - PowerShell |} Microsoft Docs"
description: "Fenntartott IP-címek (klasszikus) megérteni, hogyan toomanage őket a PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a>Fenntartott IP-címek (klasszikus)

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasszikus)](virtual-networks-reserved-public-ip.md)

IP-címek az Azure-ban két kategóriába sorolhatók: dinamikus és fenntartott. Azure által kezelt nyilvános IP-címek alapértelmezés szerint dinamikus. Adott azt jelenti, hogy egy adott felhőalapú szolgáltatás (VIP) használt IP-cím hello vagy a virtuális gép vagy szerepkör példánya közvetlenül (ILPIP) módosítható idő tootime, ha az erőforrások állítsa le vagy leállt (felszabadított) tooaccess.

tooprevent IP-címek módosítása, foglalhat IP-címet. Fenntartott IP-címek csak virtuális IP-címhez, győződjön meg arról, hogy hello IP-cím hello felhőszolgáltatás marad hello azonos, még akkor is, mint az erőforrások állnak le, vagy nem (felszabadított) használható. Ezenkívül alakíthatja ki meglévő dinamikus IP-címek egy VIP tooa fenntartott IP-címet használja.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogy egy statikus nyilvános IP cím használatával tooreserve hello [Resource Manager üzembe helyezési modellben](virtual-network-ip-addresses-overview-arm.md).

toolearn további információ az IP-címek az Azure-ban, olvassa el a hello [IP-címek](virtual-network-ip-addresses-overview-classic.md) cikk.

## <a name="when-do-i-need-a-reserved-ip"></a>Mikor kell a foglalt IP-cím?
* **Azt szeretné, amely hello IP tooensure az előfizetésében foglalt**. Ha olyan IP-cím kiadása nem történik tooreserve semmilyen körülmények között az előfizetésből, foglalt nyilvános IP-címhez kell használnia.  
* **Azt szeretné, hogy az IP-toostay a felhőalapú szolgáltatással is keresztben leállt, vagy állapota (VM) felszabadítása**. Ha azt szeretné, hogy a szolgáltatás toobe elérésük IP-címet, amely nem változik, akkor is, ha a virtuális gépek hello a felhőalapú szolgáltatás le van állítva vagy állítsa le a (felszabadított).
* **Azt szeretné, hogy a kimenő forgalom az Azure-ból egy előre jelezhető IP-címet használ tooensure**. Előfordulhat, hogy a helyszíni konfigurált tűzfal tooallow csak érkező forgalom bizonyos IP-címeket. Olyan IP-címet lefoglalásával hello forrás IP-cím tudja cím, és nem szükséges a tűzfalszabályok tooan IP-módosítása miatt tooupdate.

## <a name="faq"></a>GYIK
1. Használható az összes Azure-szolgáltatások egy fenntartott IP-cím? <br>
    Nem. Fenntartott IP-címek csak a virtuális gépek és virtuális IP-címhez keresztül közzétett felhő példány szerepköreit használható.
2. Hány foglalt IP-cím is van? <br>
    További információkért lásd: hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk.
3. Nem járnak a foglalt IP-cím? <br>
    Egyes esetekben. Díjszabása, lásd: hello [fenntartott IP cím díjszabás](http://go.microsoft.com/fwlink/?LinkID=398482) lap.
4. Hogyan foglaljon le egy IP-címet? <br>
    A PowerShell segítségével, hello [Azure szolgáltatásfelügyelet REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), vagy hello [Azure-portálon](https://portal.azure.com) tooreserve Azure-régióban IP-címet. A fenntartott IP-cím társítva tooyour előfizetés.
5. Használhatok egy fenntartott IP-cím affinitáscsoport-alapú Vnetekhez? <br>
    Nem. Fenntartott IP-címek csak a regionális Vnetek támogatottak. Fenntartott IP-címek nem támogatottak a Vnetek társított affinitáscsoportokat. Egy VNet társít egy régiót vagy affinitáscsoportot kapcsolatos további információkért lásd: hello [regionális Vnetek és Affinitáscsoportok](virtual-networks-migrate-to-regional-vnet.md) cikk.

## <a name="manage-reserved-vips"></a>Fenntartott virtuális IP-címek kezelése

Győződjön meg arról, hogy telepítette és konfigurálta a PowerShell hello lépések végrehajtásával a hello [telepítse és konfigurálja a PowerShell](/powershell/azure/overview) cikk. 

Fenntartott IP-címek használata előtt hozzá kell adnia azt tooyour előfizetés. toocreate foglalt IP-cím hello készletből nyilvános IP-címek hello elérhető *USA középső RÉGIÓJA* helyet, futtassa a következő parancs hello:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

Figyelje meg, azonban, hogy nem adható meg mi IP folyamatban van fenntartva. tooview milyen IP-címek fenntartott az előfizetéshez, futtassa a következő PowerShell-paranccsal, hello és figyelje meg a hello értékeinek *ReservedIPName* és *cím*:

```powershell
Get-AzureReservedIP
```

Várt kimenet:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>Amikor egy fenntartott IP-cím létrehozása a PowerShell használatával, nem adhat meg egy erőforrás csoport toocreate hello szolgáltatás számára fenntartott IP-Címmel. Még egy erőforráscsoportot az Azure helyek *alapértelmezett-hálózat* automatikusan. Ha hoz létre hello használó hello szolgáltatás számára fenntartott IP [Azure-portálon](http://portal.azure.com), megadhatja, hogy bármely erőforráscsoport választja. Eltérő erőforráscsoportban létrehozásakor hello szolgáltatás számára fenntartott IP *alapértelmezett-hálózat* azonban amikor hivatkozik hello foglalt IP-cím-parancsok, mint `Get-AzureReservedIP` és `Remove-AzureReservedIP`, hello neve hivatkozniakell*Csoportnevet erőforrás csoportnév fenntartott-ip*.  Például, ha egy nevű fenntartott IP-cím létrehozása *myReservedIP* erőforráscsoportban nevű *myResourceGroup*, hello szolgáltatás számára fenntartott IP mint hello nevét kell hivatkoznia *csoport myResourceGroup myReservedIP*.   

Egy IP-cím foglalt, ha a leválasztást társított tooyour előfizetés amíg nem törli azokat. toodelete egy fenntartott IP-cím, a következő PowerShell-parancs futtatása hello:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a>Foglaljon le egy meglévő felhőszolgáltatáshoz hello IP-címe
Hello IP-címét egy meglévő felhőszolgáltatáshoz foglalhat hello hozzáadásával `-ServiceName` paraméter. egy felhőalapú szolgáltatás tooreserve hello IP-címét *TestService* a hello *USA középső RÉGIÓJA* helyet, futtassa a következő PowerShell-paranccsal hello:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a>A fenntartott IP-tooa új felhőalapú szolgáltatás társítása
hello következő parancsfájlt hoz létre egy új fenntartott IP-cím, majd hozzárendeli tooa nevű új felhőalapú szolgáltatás *TestService*.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> Ha egy fenntartott IP-toouse létrehozásához egy felhőalapú szolgáltatás, továbbra is hivatkozunk toohello VM használatával *VIP:&lt;portszám >* bejövő kommunikáció. Ha olyan IP-címet nem jelenti azt toohello virtuális gép közvetlenül is elérheti. hello foglalt IP-cím hozzá van rendelve toohello felhő adott virtuális gép már alkalmazva van hello szolgáltatást. Ha közvetlenül tooconnect tooa virtuális gép IP-címekként, rendelkezik tooconfigure olyan példányszintű nyilvános IP-címet. Olyan példányszintű nyilvános IP-címet egy olyan nyilvános IP-cím (más néven egy ILPIP) közvetlenül hozzárendelt tooyour VM típusú. Nem lehet lefoglalni. További információkért olvassa el a hello [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) cikk.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>A foglalt IP-cím eltávolítása a futó központi telepítés
fenntartott IP-cím tooremove tooa új felhőalapú szolgáltatás, futtassa a következő PowerShell-paranccsal hello hozzáadva:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> A foglalt IP-cím eltávolítása a futó központi telepítés nem távolítja el hello foglalás az előfizetésből. Egyszerűen a előfizetés egy másik erőforrás használja hello IP toobe szabadít fel.
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a>A fenntartott IP-tooa-telepítést futtató társítása
hello következő parancsokat egy felhőalapú szolgáltatás létrehozása nevű *TestService2* nevű új virtuális gépen *TestVM2*. hello meglévő foglalt IP-cím, nevű *MyReservedIP* és társított toohello felhőalapú szolgáltatás.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a>Társítson egy fenntartott IP-tooa felhőalapú szolgáltatás a szolgáltatás konfigurációs fájl segítségével
A fenntartott IP-tooa felhőalapú szolgáltatás szolgáltatás (szolgáltatáskonfigurációs SÉMA) konfigurációs fájl használatával is társíthat. hello következő minta xml bemutatja, hogyan tooconfigure egy felhőalapú szolgáltatás toouse fenntartott virtuális IP-címhez nevű *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Következő lépések
* Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) hello klasszikus üzembe helyezési modellel működik.
* További tudnivalók [magánhálózati IP-címek fenntartott](virtual-networks-reserved-private-ip.md).
* További tudnivalók [példány szint nyilvános IP (ILPIP) címek](virtual-networks-instance-level-public-ip.md).

