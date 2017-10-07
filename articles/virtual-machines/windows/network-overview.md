---
title: "aaaVirtual hálózatok és a Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "További információk a hálózatkezelés Windows virtuális gépek létrehozása az Azure-ban toohello alapjait vonatkozik."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5493e9f7-7d45-4e98-be9a-657a53708746
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: e28a4782f9f6c69f6101e45dbb560ccd694a1e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-networks-and-windows-virtual-machines-in-azure"></a>Virtuális hálózatok és az Azure Windows rendszerű virtuális gépei 

Azure virtuális gép létrehozásakor létre kell hoznia egy [virtuális hálózatot](../../virtual-network/virtual-networks-overview.md) (VNet), vagy egy meglévő VNetet kell használnia. Hogyan történik az a virtuális gépek a kívánt toobe elérése hello VNet toodecide is kell. Fontos túl[terv erőforrások létrehozása előtt](../../virtual-network/virtual-network-vnet-plan-design-arm.md) és győződjön meg arról, hogy tudomásul veszi hello [a hálózati erőforrások korlátai](../../azure-subscription-service-limits.md#networking-limits).

A következő ábra hello a virtuális gépek jelentésekként jelennek meg a webkiszolgálók és adatbázis-kiszolgálók. Minden egyes virtuális gépek készletét hello virtuális hálózat alhálózatai tooseparate vannak hozzárendelve.

![Azure virtuális hálózat](./media/network-overview/vnetoverview.png)

Létrehozhat egy Vnetet is, előtt a virtuális gép létrehozása, vagy létrehozhat hello hálózatok, virtuális gép létrehozásakor. Saját maga is létrehozhat egy VNetet, vagy a virtuális gép létrehozásakor automatikusan is létrehozhatja azt.

Létrehozhat ezeket az erőforrásokat toosupport kommunikációs virtuális gépen:

- Hálózati illesztők
- IP-címek
- Virtuális hálózat és alhálózatok

Ezenkívül toothose alapvető erőforrások, akkor is figyelembe kell venni ezeket az opcionális erőforrásokat:

- Network security groups (Hálózati biztonsági csoportok)
- Terheléselosztók 

## <a name="network-interfaces"></a>Hálózati illesztők

A [hálózati illesztőt (NIC)](../../virtual-network/virtual-network-network-interface.md) hello összekapcsolása egy virtuális gép és egy virtuális hálózatot (VNet) között van. A virtuális gépek rendelkeznie kell legalább egy hálózati adapter, de több mint egy, a virtuális gép létrehozása hello hello méretétől függően. Az [Azure-ban található virtuális gépek méreteivel](sizes.md) foglalkozó szakaszból megtudhatja, hogy az egyes virtuálisgép-méretek esetében hány hálózati adapter támogatott. 

Ha azt szeretné, hogy a virtuális gép több hálózati adapter és toocreate, és legalább két virtuális gép hello kell létrehoznia.  Létrehozása után is hozzáadhat további hálózati adapterek hello virtuális gép mérete által támogatott toohello telefonszámát, de nem adhat hozzá további hálózati adapterek tooa csak létrehozni egy virtuális gép, függetlenül attól, hogy hány hálózati adaptert támogat hello Virtuálisgép-méretet. 

Virtuális gép hello tooan a rendelkezésre állási csoporthoz ad hozzá, ha belül hello rendelkezésre állási csoport összes virtuális gép egy vagy több hálózati adapterrel kell rendelkeznie. Egynél több hálózati adapter nem virtuális gépek kötelező toohave hello azonos számú hálózati adaptert, de minden rendelkeznie kell legalább kettőt.

Minden virtuális gép már léteznie kell a hálózati adapter csatlakoztatva tooa hello azonos helyen és az előfizetés, a virtuális gép hello. Egyes hálózati adapterek csatlakoztatott tooa virtuális hálózat már szerepel hello azonosnak kell lennie. Azure-beli hely és az előfizetés, hello hálózati adaptert. A hálózati adapter létrehozása után módosíthatja a hello alhálózat van csatlakoztatva, de hello csatlakozik a virtuális hálózat nem módosítható.  Egyes hálózati adapterek csatlakoztatva tooa virtuális gép, amely nem változik, amíg nem törli a virtuális gép hello MAC-cím van hozzárendelve.

Ez a táblázat felsorolja, hogy használhatja-e egy adott hálózati csatoló toocreate hello módszerek.

| Módszer | Leírás |
| ------ | ----------- |
| Azure Portal | Amikor hello Azure-portálon létrehoz egy virtuális gép, egy adott hálózati csatoló automatikusan létrejön az Ön (egy hálózati Adaptert, akkor hozza létre külön nem használható). hello portálon hoz létre egy virtuális gép csak egy hálózati adaptert. Ha azt szeretné, hogy a virtuális gép több hálózati adapter és toocreate, egy másik módszerrel kell létrehoznia. |
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) | Használjon [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) a hello **- PublicIpAddressId** paraméter tooprovide hello azonosítóját hello nyilvános IP-cím, amelyet korábban hozott létre. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md) | tooprovide hello azonosítóját hello nyilvános IP-cím, amikor korábban létrehozott, használjon [az hálózat összevont hálózati létrehozása](https://docs.microsoft.com/cli/azure/network/nic#create) a hello **--nyilvános ip-címek** paraméter. |
| [Sablon](../../virtual-network/virtual-network-deploy-multinic-arm-template.md) | Hálózati adapter sablon használatával történő üzembe helyezéséhez segítségképp használja a [nyilvános IP-címmel rendelkező virtuális hálózatban található hálózati adapter](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) sablonját. |

## <a name="ip-addresses"></a>IP-címek 

Az ilyen típusú rendelhet [IP-címek](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) tooa NIC az Azure-ban:

- **Nyilvános IP-címek** -toocommunicate bejövő és kimenő (nélkül hálózati címfordítás (NAT)) használt hello az internetes és az egyéb Azure-erőforrások nincs csatlakoztatva virtuális hálózat tooa. A nyilvános IP-cím tooa hozzárendelése a hálózati adapter nem kötelező. A nyilvános IP-címekre névleges díj vonatkozik, és előfizetésenként egy adott mennyiség áll rendelkezésre.
- **Privát IP-címek** – egy Vnetet, a helyszíni hálózat és a hello (NAT) internetes kommunikációra használt. Jelöljön ki legalább egy privát IP cím tooa virtuális gép. További információ az Azure, a NAT toolearn olvasási [ismertetése az Azure-ban kimenő kapcsolatok](../../load-balancer/load-balancer-outbound-connections.md).

Nyilvános IP-címek tooVMs vagy internet felé néző terheléselosztók is hozzárendelhető. Magánhálózati IP-címek tooVMs és a belső terheléselosztók rendelhet hozzá. Hozzárendelt IP-címek tooa virtuális gépet egy adott hálózati csatoló.

Kétféleképpen, amelyben egy IP-cím van lefoglalva tooa erőforrás - dinamikus vagy statikus. hello alapértelmezett foglalási metódus dinamikus, ahol az IP-cím létrehozásakor ne legyen lefoglalva. Ehelyett hello IP-címet kapnak, hozzon létre egy virtuális Gépet vagy leállított virtuális gép indításakor. hello IP-cím leállítása vagy törlése a virtuális gép hello megjelenik. 

tooensure hello IP-címet a virtuális gép továbbra is hello hello azonos, explicit módon állíthatja be a hello elosztási módszert toostatic. Ebben az esetben a rendszer azonnal hozzárendel egy IP-címet. Csak akkor, ha törli a virtuális gép hello, vagy módosítsa a Foglalás metódus toodynamic megjelent.
    
Ez a táblázat hello módszerek használható toocreate IP-címet.

| Módszer | Leírás |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | Alapértelmezés szerint nyilvános IP-címek dinamikus és hello tartozó címre toothem módosíthatja, ha hello VM leállítása vagy törlése. használ, amely a virtuális gép mindig hello tooguarantee hello azonos nyilvános IP-címet, és hozzon létre egy statikus nyilvános IP-címet. Alapértelmezés szerint a hello portál egy dinamikus privát IP cím tooa NIC rendel a virtuális gép létrehozásakor. A toostatic hello virtuális gép létrehozása után módosíthatja.|
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | Használhat [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) a hello **- AllocationMethod** , dinamikus vagy statikus paraméter. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | Használhat [létrehozása az hálózati nyilvános ip-](https://docs.microsoft.com/cli/azure/network/public-ip#create) a hello **--elosztási-módszert** , dinamikus vagy statikus paraméter. |
| [Sablon](../../virtual-network/virtual-network-deploy-static-pip-arm-template.md) | Nyilvános IP-cím sablon használatával történő üzembe helyezéséhez segítségképp használja a [nyilvános IP-címmel rendelkező virtuális hálózatban található hálózati adapter](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) sablonját. |

Miután létrehozott egy nyilvános IP-címet, társíthatja azt egy virtuális gép hozzá tooa hálózati adaptert.

## <a name="virtual-network-and-subnets"></a>Virtuális hálózat és alhálózatok

Egy alhálózat egy adott IP-címek hello VNet. A VNetet a rendszerezés és a biztonság érdekében több alhálózatra lehet osztani. Egyes hálózati adapterek virtuális gépen egy virtuális hálózat alhálózatában csatlakoztatott tooone. Hálózati adapter csatlakoztatott toosubnets (azonos vagy azoktól eltérő) a Vneten belül további konfigurálás nélkül is kommunikálhatnak egymással.

Ha beállít egy Vnetet, hello topológia, beleértve a hello elérhető címterületek és alhálózatok adja meg. Ha hello VNet csatlakoztatott toobe tooother Vnetekhez vagy a helyszíni hálózatokhoz, ki kell választania, amely nem fedi át címtartományai. hello IP-címek személyesek, és nem érhető el az internethez, ami csak az hello nem routeable IP-címhez például 10.0.0.0/8, 172.16.0.0/12 vagy 192.168.0.0/16 true volt hello. Most Azure bármely címtartomány hello titkos virtuális hálózat IP-címterület, amely csak akkor érhető el a virtuális hálózaton belül összekapcsolt Vnetek, és a helyszíni helyről hello belül részeként kezeli. 

Ha Ön működik, amelyben más felelős hello belső hálózatok számára a szervezeten belül, mielőtt kiválasztja a címterület forduljon toothat személy. Győződjön meg arról, hogy nincs átfedés, és tájékoztassa hello távolságot toouse, ne kísérelje meg toouse hello ugyanazon az IP-címeket. 

Alapértelmezés szerint nincs nincs biztonsági határ alhálózatok között, ezért ezek alhálózatok minden egyes virtuális gépek tooone működik egy másik. Azonban a hálózati biztonsági csoportokkal (NSG-ket), amelyek lehetővé teszik a toocontrol hello forgalom folyamat tooand az alhálózatok és virtuális gépek tooand is beállítása. 

Ez a táblázat felsorolja, hogy használhatja-e toocreate egy VNet és alhálózat hello módszerek.   

| Módszer | Leírás |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md) | Hozzon létre egy Vnetet, ha a virtuális gép létrehozása Azure lehetővé teszik, hello neve akkor hello az erőforráscsoport neve, amely tartalmazza a virtuális hálózat hello kombinációja és **- vnet**. hello címterület 10.0.0.0/24, hello szükséges alhálózati név **alapértelmezett**, és hello címtartománya a 10.0.0.0/24. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-vnet-arm-ps.md) | Használhat [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetworkSubnetConfig) és [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetwork) toocreate alhálózat és virtuális hálózat. Is [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) tooadd egy alhálózat tooan meglévő virtuális hálózatot. |
| [Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md) | hello alhálózat és virtuális hálózat hello jönnek létre: hello azonos idő. Adjon meg egy **--alhálózat-neve** paraméter túl[az hálózati vnet létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) hello alhálózati névvel. |
| [Sablon](../../virtual-network/virtual-networks-create-vnet-arm-template-click.md) | hello legegyszerűbb módja toocreate egy VNet és alhálózatok toodownload egy meglévő sablont, például a [két Vnettel rendelkező virtuális hálózati](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets), és módosítsa az igényeinek. |

## <a name="network-security-groups"></a>Network security groups (Hálózati biztonsági csoportok)

A [hálózati biztonsági csoport (NSG)](../../virtual-network/virtual-networks-nsg.md) hozzáférés-vezérlési lista (ACL) szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom toosubnets, hálózati adapterek vagy mindkettő listáját tartalmazza. Lehet, hogy az NSG-k ket alhálózatokhoz vagy az egyes hálózati adapterek csatlakoztatott tooa alhálózati társítani. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello virtuális gépek alhálózat. Emellett a tooan NGS társítása korlátozza az egyes hálózati forgalom közvetlenül tooa hálózati adaptert.

Az NSG-k két szabálykészletet tartalmaznak: bejövőt és kimenőt. hello a szabály prioritásának az egyes készleten belül egyedinek kell lennie. Minden szabály rendelkezik protokolltulajdonságokkal, forrás- és célport-tartományokkal, címelőtagokkal, forgalomiránnyal, prioritással és hozzáférési típussal. 

Minden NSG tartalmaz egy alapértelmezett szabálykészletet. hello alapértelmezett szabályokat nem lehet törölni, de a legalacsonyabb prioritású hello hozzárendeli őket, mert azok felülbírálhatja hello létrehozott szabályok. 

Amikor társít egy NSG tooa hálózati adapter, hello hello NSG hálózatelérési szabályait-e az alkalmazott csak toothat hálózati adaptert. Ha egy NSG alkalmazott tooa egyetlen hálózati adapter egy több hálózati Adapterrel virtuális gépen nem befolyásolja forgalom toohello más hálózati adaptert. Különböző NSG-k tooa hálózati adapter (vagy virtuális gép, attól függően, hogy hello üzembe helyezési modellel) társítani, és hello alhálózatot, amely egy hálózati adapter vagy a virtuális gép van kötve. Elsőbbséget alapján a hello forgalom iránya a.

Ne feledje túl[terv](../../virtual-network/virtual-networks-nsg.md#planning) az NSG-k a virtuális gépek és virtuális hálózat tervezése során.

Ez a táblázat felsorolja, hogy használhatja-e a hálózati biztonsági csoport toocreate hello módszerek.

| Módszer | Leírás |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md) | Amikor hello Azure-portálon létrehoz egy virtuális Gépet, automatikusan létrejön egy NSG-t, és társított toohello NIC hello portál létrehoz egyet. hello NSG hello neve nem hello VM hello neve kombinációja és **- nsg**. Az NSG tartalmaz egy bejövő forgalomra vonatkozó szabály egy 1000-es, service set tooRDP, hello protokoll beállítása tooTCP, port too3389 és művelet tooAllow. Ha szeretné tooallow bármely egyéb bejövő forgalom toohello VM, hozzá kell adnia a további szabályok toohello NSG. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-nsg-arm-ps.md) | Használjon [New-AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityRuleConfig) , és adja meg a szükséges hello szabályra vonatkozó információt. Használjon [New-AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityGroup) toocreate hello NSG. Használjon [Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/Set-AzureRmVirtualNetworkSubnetConfig) tooconfigure hello NSG hello alhálózathoz. Használjon [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) tooadd hello NSG toohello virtuális hálózat. |
| [Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md) | Használjon [az hálózati nsg létrehozása](https://docs.microsoft.com/cli/azure/network/nsg#create) tooinitially létrehozása hello NSG. Használjon [az hálózati nsg-szabály létrehozása](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) tooadd toohello NSG-szabályok. Használjon [az hálózati vnet alhálózati frissítés](https://docs.microsoft.com/en-us/cli/azure/network/vnet/subnet#update) tooadd hello NSG toohello alhálózat. |
| [Sablon](../../virtual-network/virtual-networks-create-nsg-arm-template.md) | Hálózati biztonsági csoport sablon használatával történő üzembe helyezéséhez segítségképp használja a [hálózati biztonsági csoport létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create) sablont. |

## <a name="load-balancers"></a>Terheléselosztók

[Az Azure Load Balancer](../../load-balancer/load-balancer-overview.md) tooyour alkalmazások magas rendelkezésre állás és a hálózati teljesítményt nyújt. A terheléselosztó beállítható, hogy túl[bejövő internetes forgalmat osztani](../../load-balancer/load-balancer-internet-overview.md) tooVMs vagy [elosztása a virtuális gépek közötti forgalmat](../../load-balancer/load-balancer-internal-overview.md). A terheléselosztó is is el tudnak osztani a létesítmények közötti hálózati vagy a külső forgalom tooa a helyszíni számítógépek és a virtuális gépek közötti forgalom speciális virtuális Gépet.

hello load balancer maps bejövő és kimenő forgalmat közötti hello nyilvános IP-cím és port hello terheléselosztó és hello magánhálózati IP-cím és port a virtuális gép hello.

Terheléselosztó létrehozásakor figyelembe kell vennie az alábbi konfigurációs elemeket is:

- **Előtérbeli IP-konfiguráció** – A terheléselosztó egy vagy több előtérbeli IP-címet, vagy más néven virtuális IP-címet (VIP) tartalmazhat. A következő IP-címek érkező forgalom hello szolgál.
- **Háttér-címkészlet** – hello NIC toowhich terhelés társított IP-címek terjesztése.
- **NAT-szabályok** -határozza meg, hogyan bejövő forgalom adatfolyamok hello előtér-IP-cím és az elosztott toohello háttér IP keresztül.
- **Terheléselosztói szabály** -leképezi a megadott előtér-IP-cím és port kombinációja tooa készlete háttér IP-cím és port kombinációja. Egyetlen terheléselosztó több terheléselosztási szabállyal is rendelkezhet. Minden szabály egy virtuális gépekhez társított előtérbeli IP és port, illetve háttérbeli IP és port kombinációja.
- **[Vizsgálat](../../load-balancer/load-balancer-custom-probe-overview.md)**  -figyelők hello virtuális gépek állapotát. Egy mintavételt toorespond sikertelensége esetén hello terheléselosztó leállítja az új kapcsolatok toohello küldése nem megfelelő állapotú virtuális gép. hello meglévő kapcsolatok nem érinti, és új kapcsolatok küldött toohealthy virtuális gépeket.

Ez a táblázat felsorolja, hogy használható-e egy internetre irányuló terheléselosztót toocreate hello módszereket.

| Módszer | Leírás |
| ------ | ----------- |
| Azure Portal | Egy internetre irányuló terheléselosztót hello Azure-portál használatával most nem lehet létrehozni. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-internet-arm-ps.md) | tooprovide hello azonosítóját hello nyilvános IP-cím, amikor korábban létrehozott, használjon [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) a hello **- PublicIpAddress** paraméter. Használjon [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurációs hello háttér-címkészlet. Használjon [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate bejövő hello előtér-IP-konfiguráció létrehozott társított NAT-szabályok. Használjon [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello vizsgálat kell. Használjon [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) toocreate hello terheléselosztó-konfigurációt. Használjon [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) toocreate hello terheléselosztóhoz.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-internet-arm-cli.md) | Használjon [az hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb#create) toocreate hello kezdeti terheléselosztó-konfigurációt. Használjon [az előtér-IP-hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) tooadd hello nyilvános IP-cím, amelyet korábban hozott létre. Használjon [az hálózati lb-címkészlet létrehozása](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurációs hello háttér-címkészlet. Használjon [az hálózati terheléselosztó bejövő forgalmat kezelő nat-szabályok létrehozása](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) tooadd NAT-szabályok. Használjon [az hálózati terheléselosztó szabály létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) tooadd hello load balancer szabályok. Használjon [az hálózati lb mintavétel létrehozása](https://docs.microsoft.com/cli/azure/network/lb/probe#create) tooadd hello mintavételt. |
| [Sablon](../../load-balancer/load-balancer-get-started-internet-arm-template.md) | Használjon [2 virtuális gép egy terheléselosztó és a NAT-szabályok konfigurálása a hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) útmutatóként üzembe helyezéséhez egy terheléselosztó-sablon használatával. |
    
Ez a táblázat hello módszerek használható toocreate belső terheléselosztót.

| Módszer | Leírás |
| ------ | ----------- |
| Azure Portal | A belső terheléselosztók hello Azure-portál használatával most nem lehet létrehozni. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-ilb-arm-ps.md) | egy magánhálózati IP-cím hello alhálózaton, használja a tooprovide [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) hello a **- privateipaddress tulajdonságot** paraméter. Használjon [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurációs hello háttér-címkészlet. Használjon [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate bejövő hello előtér-IP-konfiguráció létrehozott társított NAT-szabályok. Használjon [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello vizsgálat kell. Használjon [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) toocreate hello terheléselosztó-konfigurációt. Használjon [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) toocreate hello terheléselosztóhoz.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-ilb-arm-cli.md) | Használjon hello [az hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb#create) parancs toocreate hello kezdeti terheléselosztó-konfigurációt. toodefine hello magánhálózati IP-cím, használjon [az előtér-IP-hálózati terheléselosztó létrehozása](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) a hello **--magánfelhő-ip-cím** paraméter. Használjon [az hálózati lb-címkészlet létrehozása](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurációs hello háttér-címkészlet. Használjon [az hálózati terheléselosztó bejövő forgalmat kezelő nat-szabályok létrehozása](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) tooadd NAT-szabályok. Használjon [az hálózati terheléselosztó szabály létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) tooadd hello load balancer szabályok. Használjon [az hálózati lb mintavétel létrehozása](https://docs.microsoft.com/cli/azure/network/lb/probe#create) tooadd hello mintavételt.|
| [Sablon](../../load-balancer/load-balancer-get-started-ilb-arm-template.md) | Használjon [2 virtuális gép egy terheléselosztó és a NAT-szabályok konfigurálása a hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) útmutatóként üzembe helyezéséhez egy terheléselosztó-sablon használatával. |

## <a name="vms"></a>Virtuális gépek

Virtuális gépek hozhatók létre hello azonos virtuális hálózaton, és képes kapcsolódni a más tooeach privát IP-címek használata. Csatlakozás akkor is, ha egy átjáró nélkül hello kell tooconfigure külön alhálózatokon, vagy a nyilvános IP-címek használatát. tooput be egy virtuális hálózat virtuális gépek, létrehozhat hello VNet, majd egyes virtuális gépek létrehozásakor, akkor rendelheti hozzá toohello VNet és alhálózat. A virtuális gépek az üzembe helyezés vagy az indítás során kérik le a hálózati beállításaikat.  

A virtuális gépek az üzembe helyezésükkor kapnak IP-címet. Ha több virtuális gépet helyez üzembe egy VNetben vagy alhálózatban, akkor a rendszerindításkor kapnak IP-címet. A dinamikus IP-címet (DIP) hello egy virtuális Géphez társított belső IP-címet. Hozzárendelhet egy statikus DIP tooa virtuális gép. Ha egy statikus DIP foglal le, érdemes egy bizonyos alhálózat tooavoid véletlenül újból felhasználja a statikus DIP egy másik virtuális gép használ.  

Ha egy virtuális gép létrehozása, és később toomigrate azt be egy virtuális hálózat nincs egyszerű módosításait. Hello VM hello VNet be kell telepítenie. hello legegyszerűbb módja tooredeploy toodelete hello VM, de nem olyan lemezek csatolt tooit, és majd újból létre kell hozni hello virtuális gép használata a virtuális hálózat hello eredeti lemezek hello. 

Ez a táblázat hello módszerek használható toocreate egy virtuális Gépet a Vneten belül.

| Módszer | Leírás |
| ------ | ----------- |
| [Azure Portal](../virtual-machines-windows-hero-tutorial.md) | Hello alapértelmezett hálózati beállításait használja, amelyek korábban már említett toocreate virtuális gép és egy egyetlen hálózati adaptert. a több hálózati adapterrel rendelkező virtuális gépek toocreate, más módszert kell használnia. |
| [Azure PowerShell](../virtual-machines-windows-ps-create.md) | Hello használatát tartalmazza [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooadd hello korábban létrehozott toohello Virtuálisgép-konfiguráció hálózati adapter. |
| [Sablon](ps-template.md) | Virtuális gép sablon használatával történő üzembe helyezéséhez segítségképp használja a [windowsos virtuális gépek leegyszerűsített üzembe helyezésére](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) szolgáló sablont. |

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan tooconfigure [felhasználó által definiált útvonalak és IP-továbbítás](../../virtual-network/virtual-networks-udr-overview.md). 
- Megtudhatja, hogyan tooconfigure [VNet tooVNet kapcsolatok](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Ismerje meg, hogyan túl[útvonalak hibaelhárítása](../../virtual-network/virtual-network-routes-troubleshoot-portal.md).
