---
title: "PremiumV2 réteg konfigurálása az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Útmutató a jobb teljesítmény, a webes, mobil és API-alkalmazás az Azure App Service-ben a használatát, mivel az új PremiumV2 árképzési szint."
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: cephalin
ms.openlocfilehash: 92cc8d8b0f67dde95ea2e3fc2f0f083bd8ac8aab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>Az Azure App Service-PremiumV2 szint konfigurálása

Az új **PremiumV2** biztosít IP-címek [Dv2 sorozatú virtuális gépek](../virtual-machines/windows/sizes-general.md#dv2-series) gyorsabb processzorok, a SSD-re és a dupla memória-core arány képest **szabványos** réteg. Ebből a cikkből megismerheti, hogyan hozzon létre egy alkalmazást a **PremiumV2** szintjüket, vagy egy alkalmazás növelheti **PremiumV2** réteg.

## <a name="prerequisites"></a>Előfeltételek

Webes alkalmazás növelni készül **PremiumV2**, telepíteni kell egy webalkalmazást az Azure App Service alacsonyabb tarifacsomagot futó **PremiumV2**.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2 rendelkezésre állása

A PremiumV2 réteg érhető el jelenleg az App Service _Windows_ csak. Linux-tárolók még nem támogatott.

PremiumV2 már elérhető az Azure-régiók és az egyre növekvő. Ha az elérhető az adott régióban, a következő parancsot az Azure parancssori felület a [Azure Cloud rendszerhéj](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

Ha hibaüzenetet kap létrehozása vagy az alkalmazásszolgáltatási csomag létrehozása, majd során **PremiumV2** valószínűleg nem érhető el a kiválasztott régióban.

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>Hozzon létre egy alkalmazást PremiumV2 rétegben

Egy App Service-alkalmazás az árképzési szint van megadva a [App Service-csomag](azure-web-sites-web-hosting-plans-in-depth-overview.md) azt futtató. App Service-csomagot is létrehozhat, önmagában, vagy a webes alkalmazás létrehozásának részeként.

Az App Service-csomag konfigurálása esetén a <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>, jelölje be **tarifacsomag**. 

Válasszon egyet a a **PremiumV2** , és kattintson **válasszon**.

![](media/app-service-configure-premium-tier/pick-premium-tier.png)

> [!IMPORTANT] 
> Ha nem látja **P1V2**, **P2V2**, és **P3V2** regisztrációja, mivel a beállításokat, vagy **PremiumV2** nem áll rendelkezésre a választott régió vagy áll a Linux App Service-csomag, amely nem támogatja a konfigurálása **PremiumV2**.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Vertikális felskálázás PremiumV2 réteghez egy meglévő alkalmazáshoz

Egy meglévő alkalmazást, hogy skálázás előtt **PremiumV2** réteg, ellenőrizze, hogy **PremiumV2** érhető el az adott régióban. További információ: [PremiumV2 rendelkezésre állási](#availability). Ha az adott régióban nincs elérhető [vertikális felskálázás egy nem támogatott régióban](#unsupported).

Attól függően, hogy az üzemeltetési környezetbe vertikális felskálázásával szükség lehet további lépéseket. 

Az a <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>, nyissa meg az App Service alkalmazás lapját.

Válassza ki az App Service alkalmazás oldal bal oldali navigációs **vertikális felskálázás (App Service-csomag)**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Válasszon egyet a a **PremiumV2** méretét, majd kattintson a **válasszon**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

Ha a művelet sikeresen befejeződött, az alkalmazásban – áttekintés oldalra jeleníti meg, hogy mostantól az egy **PremiumV2** réteg.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Ha hibaüzenetet kap

Egy App Service-csomagok nem vertikális felskálázás PremiumV2 csomagra. A méretezett művelet olyan hibaüzenetet ad, ha az alkalmazás egy új App Service-csomag szükséges.

Hozzon létre egy _Windows_ azonos régióban és erőforrás tartozik, mint a meglévő App Service-alkalmazás az App Service-csomag. Kövesse a [hozzon létre egy alkalmazást PremiumV2 réteg](#create) állítsa be **PremiumV2** réteg. Szükség esetén ugyanazt a kibővített konfigurációt használja, a meglévő App Service-csomag (száma példányok, automatikus skálázás és így tovább).

Nyissa meg újra az App Service alkalmazás oldalát. Válassza ki az App Service bal oldali navigációs sáv **módosítás App Service-csomag**.

![](media/app-service-configure-premium-tier/change-plan.png)

Válassza ki az imént létrehozott App Service-csomag.

![](media/app-service-configure-premium-tier/select-plan.png)

Miután befejeződött a műveletet, az alkalmazás fut-e **PremiumV2** réteg.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-region"></a>Egy nem támogatott régióban méretezése

Ha az alkalmazás fut egy régióban ahol **PremiumV2** van még nem érhető el, áthelyezheti az alkalmazás egy másik régióban előnyeit **PremiumV2**. Erre két lehetősége van:

- Új alkalmazás létrehozása **PremiumV2** tervezze meg, majd telepítse újra az alkalmazás kódjában. Kövesse a [hozzon létre egy alkalmazást PremiumV2 réteg](#create) állítsa be **PremiumV2** réteg. Szükség esetén ugyanazt a kibővített konfigurációt használja, a meglévő App Service-csomag (száma példányok, automatikus skálázás és így tovább).
- Ha az alkalmazás már fut egy meglévő **prémium** réteg, akkor átmásolhatja az alkalmazás az összes alkalmazás beállításait, a kapcsolati karakterláncok és a központi telepítés konfigurálása.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    Az a **Klónozott alkalmazás** lapon létrehozhat egy új App Service-csomagot, és adja meg a beállításokat, amelyet szeretne klónozza a régióban.

## <a name="automate-with-scripts"></a>Parancsfájlok automatizálásához

Automatizálható az alkalmazás létrehozása a **PremiumV2** réteg parancsfájlok használata a [Azure CLI](/cli/azure/install-azure-cli) vagy [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

A következő parancsot az App Service-csomagot hoz létre _P1V2_. A felhő rendszerhéj futtatható. A beállítások `--sku` P1V2, amelyek _P2V2_, és _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

A következő parancsot az App Service-csomagot hoz létre _P1V2_. A beállítások `-WorkerSize` vannak _kis_, _Közepes_, és _nagy_.

```PowerShell
New-AzureRmAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>További erőforrások

[Vertikális felskálázás egy alkalmazást az Azure-ban](web-sites-scale.md)  
[Példányszám manuális vagy automatikus méretezése](../monitoring-and-diagnostics/insights-how-to-scale.md)