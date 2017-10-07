---
title: "aaaMutiple virtuális IP-címek a felhőalapú szolgáltatáshoz"
description: "Több virtuális áttekintése és hogyan tooset több virtuális IP-címek egy felhőalapú szolgáltatás"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a>Több virtuális IP-címek egy felhőalapú szolgáltatás konfigurálása

Azure cloud services Azure által megadott IP-cím használatával a nyilvános interneten hello keresztül érheti el. A nyilvános IP-cím hivatkozott tooas virtuális IP-címhez (virtuális IP-cím), mert a hozzá kapcsolódó toohello Azure terheléselosztó, és nem a virtuális gép (VM) példányokat hello felhőszolgáltatás hello. A Virtuálisgép-példány egy felhőalapú szolgáltatás belül egyetlen virtuális IP-címhez használatával végezheti el.

Van azonban, amelyben több VIP esetleg forgatókönyvek szerint egy belépési pont toohello ugyanaz a felhőalapú szolgáltatás. Például a felhőalapú szolgáltatás, amely minden helyen különböző ügyfél üzemeltetett hello alapértelmezett 443-as portot használja az SSL-kapcsolatot szükség van, vagy a bérlői több webhely futhatnak. Ebben a forgatókönyvben minden webhelyre vonatkozóan meg kell toohave egy másik nyilvános felé néző IP-címet. hello az alábbi ábra szemlélteti a tipikus több-bérlős webtároláshoz van szükség SSL-tanúsítványokat a hello azonos nyilvános port.

![Több virtuális IP-SSL forgatókönyv](./media/load-balancer-multivip/Figure1.png)

Az összes virtuális IP-címek használata hello a fenti hello példában ugyanazt a nyilvános portot (443) és a forgalom átirányított tooone vagy több betölteni egy egyedi magánhálózati port hello belső IP-cím üzemeltető összes hello webhely hello felhőszolgáltatás-alapú virtuális gépek elosztott terhelésű.

> [!NOTE]
> Egy másik helyzet megkövetelését hello használata hello több virtuális IP-címek több SQL AlwaysOn rendelkezésre állási csoport figyelőinek ugyanaz a virtuális gépek készletét hello üzemelteti.

Virtuális IP-címmel alapértelmezés, ami azt jelenti, hogy idővel megváltozhat hello tényleges IP-cím toohello cloud service dinamikus. tooprevent, hogy le, foglalhat virtuális IP-címhez a szolgáltatás. toolearn fenntartott virtuális IP-címek, kapcsolatos további információkért lásd: [foglalt nyilvános IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md).

> [!NOTE]
> Ellenőrizze a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/) árazás virtuális IP-címek és foglalt IP-cím.

PowerShell tooverify hello a felhőalapú szolgáltatás által használt virtuális IP-címek használata, valamint hozzáadhat és távolítsa el a virtuális IP-címek, társítsa a virtuális IP-tooan végpont, és adott VIP-címhez a terheléselosztás konfigurálása.

## <a name="limitations"></a>Korlátozások

Jelenleg a többszörös VIP funkció is korlátozott toohello a következő esetekben:

* **Csak az infrastruktúra-szolgáltatási**. A virtuális gépeket tartalmazó felhőszolgáltatások több VIP csak engedélyezheti. A PaaS szerepkörpéldányok feladatok több VIP nem használható.
* **Csak a PowerShell**. Több VIP csak kezelheti a PowerShell használatával.

Ezek a korlátozások ideiglenesek, és bármikor módosíthatja. Módosításokat toorevisit meg arról, hogy a lap tooverify jövőbeli.

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a>Hogyan tooadd VIP tooa felhőalapú szolgáltatás
virtuális IP-címhez tooadd tooyour szolgáltatást, futtassa a következő PowerShell-paranccsal hello:

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

A parancs egy példa a következő eredmény hasonló toohello jeleníti meg:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a>Hogyan tooremove virtuális IP-címhez egy felhőalapú szolgáltatás
tooremove hello VIP hozzáadott tooyour szolgáltatás hello fenti példában, a következő PowerShell-parancs futtatása hello:

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> Virtuális IP-címhez csak távolítsa el, ha nincsenek végpontok társítva tooit rendelkezik.


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a>Hogyan tooretrieve VIP adatait egy felhőalapú szolgáltatás
egy felhőalapú szolgáltatás, futtassa a következő PowerShell-parancsfájl hello kapcsolódó tooretrieve hello virtuális IP-címmel:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

hello parancsfájl egy minta a következő eredmény hasonló toohello jeleníti meg:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Ebben a példában hello felhőszolgáltatás 3 rendelkezik virtuális IP-címmel:

* **Vip1-en** van hello alapértelmezett VIP, tudja, hogy mivel hello IsDnsProgrammedName értéke tootrue.
* **Vip2** és **Vip3** nem kell használni, mint az IP-címek nem rendelkeznek. Használni fogják őket csak ha egy végpont toohello VIP rendel hozzá.

> [!NOTE]
> Az előfizetés csak megterheljük az extra virtuális IP-címek, ha társítva a végpont. Az árakkal kapcsolatos további információkért lásd: [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a>Hogyan tooassociate VIP tooan végpont

virtuális IP-címhez tooassociate egy felhőalapú szolgáltatás tooan végponton, futtassa a következő PowerShell-paranccsal hello:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

hello parancs létrehoz egy csatolt toohello VIP nevű végpontot *Vip2* porton *80*, és csatolja azt toohello nevű virtuális gép *myVM1* felhőszolgáltatásban nevű  *myService* használatával *TCP* porton *8080*.

tooverify hello konfigurációs, futtassa a következő PowerShell-paranccsal hello:

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

hello kimenete a következő példa hasonló toohello néz ki:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a>Hogyan tooenable terheléselosztás a megadott virtuális IP-címhez

Egyetlen virtuális IP-címhez több virtuális gép terheléselosztásához célokra is társíthat. Például, hogy nevű felhőszolgáltatás *myService*, és két virtuális gép nevű *myVM1* és *myVM2*. És a felhőalapú szolgáltatás több virtuális IP-címek, egyiket nevű *Vip2*. Ha azt szeretné, hogy minden forgalomnak tooport tooensure *81-es* a *Vip2* közötti elosztását *myVM1* és *myVM2* porton *8181* - ben futtassa hello következő PowerShell-parancsfájlt:

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

A load balancer toouse másik virtuális IP-címhez is frissítheti. Például ha hello az alábbi PowerShell-parancsot futtatja, megváltozik a hello terheléselosztási set toouse Vip1 nevű virtuális IP-címhez:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a>Következő lépések

[A terheléselosztás Azure naplóelemzés](load-balancer-monitor-log.md)

[Az Internet felé néző terhelés terheléselosztó áttekintése](load-balancer-internet-overview.md)

[Az internetre irányuló terheléselosztót első lépések](load-balancer-get-started-internet-arm-ps.md)

[Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md)

[Fenntartott IP-cím REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx)
