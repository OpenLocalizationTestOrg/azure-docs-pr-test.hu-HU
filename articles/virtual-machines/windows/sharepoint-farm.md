---
title: "SharePoint kiszolgálófarmok aaaCreate az Azure-ban |} Microsoft Docs"
description: "Gyorsan létrehozhat egy új SharePoint 2013-at vagy a SharePoint 2016-farm az Azure-ban az Azure portál piactér hello."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a>SharePoint használata az Azure portál piactér hello kiszolgálófarmok létrehozása

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>A SharePoint 2013-farmok
A Microsoft Azure portál Piactéri hello gyorsan létre tud hozni előre beállított a SharePoint Server 2013-farmok. Ez takaríthat meg sok időt Ha alapszintű vagy magas rendelkezésre állású SharePoint-farm fejlesztési/tesztelési környezetben kell, vagy ha éppen kipróbálja a SharePoint Server 2013 együttműködés megoldásként a szervezet számára.

> [!NOTE]
> Hello **SharePoint-kiszolgálófarm** hello Azure-portálon az Azure piactér hello elem el lett távolítva. Helyette a hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** és **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** elemeket.
>
>

Alapszintű hello SharePoint-farm ebben a konfigurációban a három virtuális gépet tartalmaz.

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

A farm konfigurációja használhat egy egyszerűbb beállítása a SharePoint-alkalmazások fejlesztéséhez vagy a SharePoint 2013 első értékelését.

toocreate hello alapvető (Háromkiszolgálós) SharePoint-farm:

1. Kattintson a [Itt](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Kattintson a **telepítése**.
3. A hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán kattintson a **létrehozása**.
4. Adja meg a beállításokat a hello hello lépésein **létrehozása a SharePoint 2013-as nem magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán, és kattintson **létrehozása**.

Ebben a konfigurációban kilenc virtuális gépek magas rendelkezésre állású SharePoint-farm hello áll.

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

Használhatja a farm konfigurációs tootest magasabb ügyfél terhelés, magas rendelkezésre állású hello külső SharePoint-webhely, az SQL Server AlwaysOn rendelkezésre állási csoportok SharePoint-farm. A SharePoint-alkalmazások fejlesztéséhez a magas rendelkezésre állású környezetekben is használhatja ezt a konfigurációt.

toocreate hello magas rendelkezésre állású (kilenc-kiszolgáló) SharePoint-farm:

1. Kattintson a [Itt](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Kattintson a **telepítése**.
3. A hello **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán kattintson a **létrehozása**.
4. Beállításainak megadása az hello hello hét lépésein **létrehozása a SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** ablaktáblán, és kattintson **létrehozása**.

> [!NOTE]
> Nem hozható létre hello **a SharePoint 2013 nem magas rendelkezésre ÁLLÁSÚ Farm** vagy **SharePoint 2013 magas rendelkezésre ÁLLÁSÚ Farm** egy Azure ingyenes próbaverziót.
>
>

hello Azure-portálon létrehoz mindkét gazdaság az Internet felé néző webes jelenlét csak felhőalapú virtuális hálózat. Nincs pont-pont VPN- vagy ExpressRoute kapcsolat hátsó tooyour szervezeti hálózat.

> [!NOTE]
> Ha alapszintű létrehozása hello, vagy a magas rendelkezésre állású SharePoint-farmok hello Azure-portál használatával, nem adható meg egy meglévő erőforráscsoportot. a korlátozás toowork gazdaság létrehozása az Azure PowerShell. További információkért lásd: [létrehozása a SharePoint 2013-as fejlesztési és tesztelési célú farmok az Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>A SharePoint 2016-farmok
Lásd: [Ez a cikk](https://technet.microsoft.com/library/mt723354.aspx) hello utasításokat toobuild hello egykiszolgálós SharePoint Server 2016 következő farm.

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a>SharePoint-farmok hello kezelése
A távoli asztali kapcsolaton keresztül a gazdaság hello-kiszolgálókat is felügyelhet. További információkért lásd: [jelentkezzen be a virtuális gép toohello](quick-create-portal.md#connect-to-virtual-machine).

Hello a SharePoint központi felügyeleti hely konfigurálhatja a helyeket, SharePoint-alkalmazások és egyéb funkciókat. További információkért lásd: [SharePoint konfigurálása](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Következő lépések
* Fedezze fel további [SharePoint konfigurációk](https://technet.microsoft.com/library/dn635309.aspx) az Azure infrastruktúra-szolgáltatásokat.
