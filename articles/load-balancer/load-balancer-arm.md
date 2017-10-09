---
title: "Terheléselosztó Resource Manager támogatása aaaAzure |} Microsoft Docs"
description: "Az Azure Resource Manager terheléselosztóhoz powershell használatával. Terheléselosztó sablonok használata"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Az Azure erőforrás-kezelő támogatási használata az Azure terheléselosztó

Az Azure Resource Manager hello előnyben részesített felügyeleti keretrendszere, amely az Azure. Az Azure Load Balancer Azure Resource Manager-alapú API-k és eszközök használatával kezelhetők.

## <a name="concepts"></a>Alapelvek

A Resource Manager Azure Load Balancer tartalmazza a következő gyermekerőforrásait hello:

* Terheléselosztó előtér-IP-konfiguráció – tartalmazhat egy vagy több előtérbeli IP-címek, más néven a virtuális IP-címek (VIP). A következő IP-címek érkező forgalom hello szolgál.
* Háttér-címkészlet – ezek a hello virtuális gép hálózati kártya (NIC) toowhich terhelés eloszlik a társított IP-címek.
* Terheléselosztási szabályok – a szabály tulajdonság képezi le egy adott előtérbeli IP-cím és port kombinációja tooa set háttérbeli IP-címek és port kombináció. Egyetlen terheléselosztó több terheléselosztási szabállyal is rendelkezhet. Minden szabály egy virtuális gépekhez társított előtérbeli IP és port, illetve háttérbeli IP és port kombinációja.
* Mintavételt – mintavételek menüpontban, tookeep nyomon Virtuálisgép-példányok állapotát hello engedélyezése. Ha nem sikerül egy állapotmintáihoz, hello Virtuálisgép-példány megnyílik kívül Elforgatás automatikusan.
* Bejövő forgalmat kezelő NAT-szabályok – NAT-szabályok meghatározása hello hello front-end IP átfutó bejövő forgalmat, és vissza az elosztott toohello befejező IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Gyorsindítási sablonok

Az Azure Resource Manager lehetővé teszi tooprovision olyan deklaratív sablonok segítségével az alkalmazások. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt. Hello használata azonos sablon toorepeatedly az alkalmazást, minden szakasza hello alkalmazás-életciklus helyezi üzembe.

Sablonok tartalmazhatnak a virtuális gépek, virtuális hálózatok, rendelkezésre állási csoportok, hálózati adapterek (NIC), Storage-fiókok, Terheléselosztók, hálózati biztonsági csoportok és nyilvános IP-címek definícióit. A sablonokkal hozhat létre mindent megtalál az összetett alkalmazások. hello sablonfájl ellenőrizhetők a verziókövetés és együttműködés tartalomkezelési rendszerbe.

[További információ a sablonok](../azure-resource-manager/resource-manager-template-walkthrough.md)

[További információ a hálózati erőforrások](../virtual-network/resource-groups-networking.md)

Gyors üzembe helyezés sablonok használata az Azure Load Balancer itt található: egy [GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates) üzemeltető közösségi létrehozott sablonok készlete.

Sablonok példái:

* [2 virtuális gép egy terheléselosztó és terheléselosztási szabályok](http://go.microsoft.com/fwlink/?LinkId=544799)
* [2 virtuális gép egy belső terheléselosztó és a terheléselosztó szabályait a VNETEN belül](http://go.microsoft.com/fwlink/?LinkId=544800)
* [2 virtuális gép egy terheléselosztó és hello Terheléselosztó NAT-szabályok konfigurálása](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>A PowerShell vagy a CLI Azure Load Balancer beállítása

Ismerkedés az Azure Resource Manager parancsmagok, a parancssori eszköz és a REST API-k

* [Azure-hálózat parancsmagokat](https://msdn.microsoft.com/library/azure/mt163510.aspx) lehet használt toocreate terheléselosztót.
* [Hogyan toocreate egy terhelés-kiegyenlítő Azure Resource Manager használatával](load-balancer-get-started-ilb-arm-ps.md)
* [Hello Azure CLI használata az Azure erőforrás-kezelés](../xplat-cli-azure-resource-manager.md)
* [Terheléselosztó REST API-k](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Következő lépések

Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.

Megtudhatja, hogyan toomanage [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md). Ez akkor fontos, ha az alkalmazás igényeinek tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött.
