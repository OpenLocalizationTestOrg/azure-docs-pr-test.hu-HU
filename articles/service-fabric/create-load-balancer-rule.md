---
title: "egy Azure Load Balancer szabály fürt aaaCreate"
description: "Állítson be egy Azure Load Balancer tooopen portot az Azure Service Fabric-fürt."
services: service-fabric
documentationcenter: na
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: adegeo
ms.openlocfilehash: 4a40f62422bd895d782be8cbaace5f4e1af81db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Nyissa meg a Service Fabric-fürt által használt portokat

az Azure Service Fabric-fürt üzembe helyezéséhez hello terheléselosztó irányítja a forgalmat tooyour app egy csomóponton futó. Ha megváltoztatja az alkalmazás toouse egy másik portra, tesz közzé, hogy a port (vagy az útvonal egy másik portot) a hello Azure Load Balancer.

A service fabric-fürt tooAzure üzembe helyezésekor, a terheléselosztó automatikusan létrejött meg. Ha nem rendelkezik olyan terheléselosztóhoz, lásd: [egy internetre irányuló terheléskiegyenlítő](..\load-balancer\load-balancer-get-started-internet-portal.md).

## <a name="configure-service-fabric"></a>A service fabric konfigurálása

A Service Fabric-alkalmazás **ServiceManifest.xml** konfigurációs fájl határozza meg az alkalmazás vár toouse hello végpontok. Hello konfigurációs fájl frissített toodefine végpont után hello terheléselosztó kell lennie, hogy (vagy egy másik) frissített tooexpose port. Hogyan toocreate hello szolgáltatás hálóvégpont további információkért lásd: [egy végpont beállítása](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Hozzon létre olyan terheléselosztó szabályhoz

Olyan terheléselosztó szabályhoz egy internetre irányuló portot nyit, és az alkalmazás által használt forgalom toohello belső csomópont port továbbítja. Ha nem rendelkezik olyan terheléselosztóhoz, lásd: [egy internetre irányuló terheléskiegyenlítő](..\load-balancer\load-balancer-get-started-internet-portal.md).

terheléselosztó szabály toocollect hello a következő információkat kell toocreate:

- Terheléselosztó neve.
- Erőforráscsoport hello a terheléselosztó és a service fabric-fürt betöltése.
- Külső porton.
- Belső port.

## <a name="azure-cli"></a>Azure CLI
>[!NOTE]
>Hello terheléselosztó toodetermine hello neve van szüksége, használja a parancs tooquickly get terheléselosztókról, illetve a kapcsolódó hello erőforráscsoportok listáját.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>

Ami mindössze egy egyetlen parancs toocreate olyan terheléselosztó szabályhoz a hello **Azure CLI**. Egyszerűen tooknow mindkét hello hello terhelés terheléselosztó és erőforrás csoport toocreate új szabály nevét.

```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

hello Azure CLI parancs néhány hello a következő táblázatban ismertetett paraméterekkel rendelkezik:

| Paraméter | Leírás |
| --------- | ----------- |
| `--backend-port`  | hello port hello service fabric-alkalmazás figyel. |
| `--frontend-port` | hello port hello terheléselosztó tesz elérhetővé a külső kapcsolatok betölteni. |
| `-lb-name` | hello hello neve terheléselosztó toochange betölteni. |
| `-g`       | hello erőforráscsoport, amelyen hello terheléselosztó és a service fabric-fürt. |
| `-n`       | hello kiválasztott hello szabály nevét. |


>[!NOTE]
>További információ a hogyan toocreate hello Azure parancssori felület, a terheléselosztó: [hozzon létre egy terhelés-kiegyenlítő hello Azure CLI](..\load-balancer\load-balancer-get-started-internet-arm-cli.md).

## <a name="powershell"></a>PowerShell

>[!NOTE]
>Hello terheléselosztó toodetermine hello neve van szüksége, használja a parancs tooquickly get terheléselosztókról, illetve a kapcsolódó erőforráscsoportok listáját.
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

PowerShell egy kicsit bonyolultabb, mint az Azure parancssori felület hello. Fogalmilag akkor hajtsa végre a következő lépéseket toocreate hello szabály.

1. Hello terheléselosztó beolvasása az Azure-ból.
2. Hozzon létre egy szabályt.
3. Hello szabály toohello terheléselosztó felvétele.
4. Hello terheléselosztó frissítése.

```powershell
# Get hello load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create hello rule based on information from hello load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add hello rule toohello load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update hello load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

Hello vonatkozó `New-AzureRmLoadBalancerRuleConfig` parancs hello `-FrontendPort` jelöli hello port hello terheléselosztó elérhetővé teszi a külső kapcsolatot, és hello `-BackendPort` jelöli hello port hello service fabric-alkalmazást figyel.

>[!NOTE]
>További információ a hogyan toocreate PowerShell, a terheléselosztó: [terheléselosztó létrehozása a PowerShell használatával](..\load-balancer\load-balancer-get-started-internet-arm-ps.md).

