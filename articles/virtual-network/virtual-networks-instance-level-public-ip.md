---
title: "aaaAzure példányszintű nyilvános IP-cím (klasszikus) címek |} Microsoft Docs"
description: "Példány szintű nyilvános IP (ILPIP) címek megérteni, hogyan toomanage őket a PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 832143ee6fdd80b634e1cebfddc759a8cacda583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="instance-level-public-ip-classic-overview"></a>Példány nyilvános IP (klasszikus) áttekintése
Egy szint nyilvános IP (ILPIP) példány egy nyilvános IP-cím rendelhető közvetlenül tooa virtuális gép vagy Felhőszolgáltatás szerepkörpéldányt, nem pedig toohello felhőszolgáltatás, amely a virtuális gép vagy szerepkör példányát találhatók. Egy ILPIP hello hello virtuális IP-cím (VIP) tooyour felhőszolgáltatás hozzárendelt hely nem veszi. Ehelyett használható tooconnect további IP-cím közvetlenül tooyour virtuális gép vagy szerepkör-példányt.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, virtuális gépek erőforrás-kezelő létrehozása. Győződjön meg arról, hogy hogyan [IP-címek](virtual-network-ip-addresses-overview-classic.md) munkahelyi az Azure-ban.

![Különbség ILPIP és a VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

1. ábrán látható, VIP hello felhőalapú szolgáltatás hozzáfér, hello során egyes virtuális gépek általában hozzáférnek VIP:&lt;portszámát&gt;. Egy ILPIP tooa hozzárendelésével speciális virtuális Gépet, virtuális gép használható érhetők el közvetlenül az adott IP-címet használja.

Amikor az Azure-ban létrehoz egy felhőalapú szolgáltatás, megfelelő DNS A-rekordokat jönnek létre automatikusan tooallow toohello szolgáltatás egy teljesen minősített tartománynevét (FQDN), a tényleges VIP hello használata helyett. hello egy ILPIP, így access toohello virtuális gép vagy szerepkör-példány a teljes tartománynév alapján hello ILPIP helyett a zavartalan ugyanazt a folyamatot. Például ha nevű felhőalapú szolgáltatás létrehozása *contosoadservice*, és konfigurálja a webes szerepkör nevű *contosoweb* két példánnyal, A következő Azure regiszterekben hello hello példányok rögzíti:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> Minden virtuális gép vagy szerepkör-példány csak egy ILPIP rendelhet hozzá. Előfizetésenként too5 ILPIPs mentése használhatja. ILPIPs több hálózati Adapterrel virtuális gépek esetén nem támogatottak.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Miért kellene kérelem egy ILPIP?
Ha azt szeretné, hogy toobe képes tooconnect tooyour VM, vagy egy IP-cím szerint szerepkörpéldányt közvetlenül hozzárendelt tooit, ahelyett, hogy az hello felhőalapú szolgáltatás VIP:&lt;portszámát&gt;, egy ILPIP kérhetnek a virtuális gép vagy a szerepkör példánya.

* **Aktív FTP** -egy ILPIP tooa VM hozzárendelésével megkaphatja a forgalom a portokon. Végpontok esetén nincs szükség virtuális gépek tooreceive forgalma hello.  (Https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [az FTP protokoll áttekintése] talál információt hello FTP protokollt.
* **Kimenő IP** - kimenő forgalmat a virtuális gép hello leképezve toohello ILPIP hello forrásként, és hello ILPIP egyedileg azonosítja a hello VM tooexternal entitásokat.

> [!NOTE]
> Elmúlt hello ILPIP-címnek hivatkozott tooas nyilvános IP (PIP)-cím volt.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>A virtuális gép egy ILPIP kezelése
a következő feladatok hello lehetővé teszik toocreate, hozzárendelése és eltávolítása a virtuális gépekről érkező ILPIPs:

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a>Hogyan toorequest egy ILPIP virtuális gép létrehozása PowerShell használatával
a következő PowerShell-parancsfájl hello létrehoz egy felhőalapú szolgáltatás nevű *FTPService*, lekéri a lemezkép az Azure-ból, létrehoz egy nevű virtuális gép *FTPInstance* beolvasott hello lemezképpel beállítja hello VM toouse egy ILPIP és hello VM toohello új szolgáltatás hozzáadása:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a>Hogyan tooretrieve ILPIP információkat a virtuális gépek
tooview hello ILPIP információi hello hello, előző parancsfájl létrehozott virtuális gép hello a következő PowerShell-parancs futtatása, és tekintse meg az hello értékeinek *PublicIPAddress* és *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Várt kimenet:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-tooremove-an-ilpip-from-a-vm"></a>Hogyan tooremove egy ILPIP VM
tooremove hello ILPIP toohello VM hello előző parancsfájlban, futtassa a következő PowerShell-paranccsal hello hozzáadva:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a>Hogyan tooadd egy ILPIP tooan meglévő virtuális gép
tooadd egy ILPIP toohello előző, hello parancsfájl használatával létrehozott virtuális gép hello a következő parancs futtatásával:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>A Cloud Services szerepkör példány egy ILPIP kezelése

tooadd ILPIP tooa Cloud Services szerepkör példánya, teljes hello a következő lépéseket:

1. Hello .cscfg fájl tölthető hello lépéseit felhőszolgáltatás hello hello elvégzésével [tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.
2. Frissítés hello .cscfg fájl hello hozzáadásával `InstanceAddress` elemet. hello következő minta hozzáadása egy elnevezett ILPIP *MyPublicIP* nevű tooa szerepkörpéldányt *WebRole1*: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Hello .cscfg fájl feltöltése a hello lépéseit felhőszolgáltatás hello hello elvégzésével [tooConfigure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) cikk.

## <a name="next-steps"></a>Következő lépések
* Megértéséhez hogyan [IP-címzés](virtual-network-ip-addresses-overview-classic.md) hello klasszikus üzembe helyezési modellel működik.
* További tudnivalók [fenntartott IP-címek](virtual-networks-reserved-public-ip.md).
