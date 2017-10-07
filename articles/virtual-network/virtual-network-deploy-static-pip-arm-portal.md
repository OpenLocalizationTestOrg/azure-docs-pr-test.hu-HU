---
title: "egy virtuális Gépet egy statikus nyilvános IP-cím - Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate virtuális gép és a statikus nyilvános IP cím használatával hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a>Virtuális gép létrehozása egy statikus nyilvános IP-cím hello Azure-portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasszikus)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím

toocreate egy virtuális Gépet egy statikus nyilvános IP-cím hello Azure-portálon teljes hello a következő lépéseket:

1. Egy böngészőből keresse meg a toohello [Azure-portálon](https://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.
2. Hello felső bal oldali sarokban hello portál, kattintson a **új**>>**számítási**>**Windows Server 2012 R2 Datacenter**.
3. A hello **telepítési modell kiválasztása** listára, válassza ki **erőforrás-kezelő** kattintson **létrehozása**.
4. A hello **alapjai** panelen adja meg a Virtuálisgép-adatok hello alább látható módon, és kattintson a **OK**.
   
    ![Azure portál – alapok](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. A hello **méret kiválasztása** panelen kattintson **A1 szabványos** az alábbi módon, majd **kiválasztása**.
   
    ![Azure portál – méret kiválasztása](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. A hello **beállítások** panelen kattintson a **nyilvános IP-cím**, ezt a hello **nyilvános IP-cím létrehozása** panelen, a **hozzárendelés**, kattintson a **Statikus** alább látható módon. Majd **OK**.
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. A hello **beállítások** panelen kattintson a **OK**.
8. Felülvizsgálati hello **összegzés** panelen, a lent látható módon, és kattintson **OK**.
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Figyelje meg hello új csempe az irányítópulton.
   
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Hello virtuális gép létrehozása után hello **beállítások** panelen megjelenik a lent látható módon
    
    ![Azure portál – nyilvános IP-cím létrehozása](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

