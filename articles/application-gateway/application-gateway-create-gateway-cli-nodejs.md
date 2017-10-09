---
title: Azure CLI 1.0 Azure Application Gateway - aaaCreate |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate használatával Alkalmazásátjáró hello Azure CLI 1.0 az erőforrás-kezelőben"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3c0d2d96b6be404d0372d00f0deb2a32959ca419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a>Alkalmazásátjáró létrehozása hello Azure parancssori felület használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-sablon](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)
> 
> 

Az Azure Application Gateway egy 7. rétegbeli terheléselosztó. Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között. Alkalmazásátjáró rendelkezik a következő kézbesítési alkalmazásszolgáltatáshoz hello: HTTP terheléselosztás, a munkamenet cookie-alapú kapcsolat és a Secure Sockets Layer (SSL) kiszervezési egyéni állapotfigyelő mintavételt betölteni, és többhelyes támogatás.

## <a name="prerequisite-install-hello-azure-cli"></a>Előfeltétel: Hello Azure parancssori felület telepítése

tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](../xplat-cli-install.md) túl van szüksége, és[tooAzure bejelentkezés](../xplat-cli-connect.md). 

> [!NOTE]
> Ha az Azure-fiók nem rendelkezik, meg kell egyet. Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Forgatókönyv

Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate hello Azure-portálon.

Ez a forgatókönyv tartalma:

* Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.
* Nevű ContosoVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.
* A CIDR-blokkja 10.0.0.0/28 használó subnet01 nevű alhálózat létrehozása.

> [!NOTE]
> További konfigurációs hello Alkalmazásátjáró, beleértve az egyéni rendszerállapot-vizsgálat, háttér címkészletet címek és a további szabályok hello Alkalmazásátjáró konfigurálása után, és nem a kezdeti telepítése során konfigurált.

## <a name="before-you-begin"></a>Előkészületek

Az Azure Application Gateway saját alhálózatba van szükség. Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal. Miután telepít egy alkalmazást tooa átjáróalhálózatot, csak további alkalmazásátjárót-e fel tudja toobe toohello alhálózat.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be. 

```azurecli-interactive
azure login
```

Amennyiben az előző példa hello írja be, a kód valósul meg. Keresse meg a böngésző toocontinue hello bejelentkezési folyamat toohttps://aka.ms/devicelogin.

![cmd megjelenítő eszköz-bejelentkezés][1]

Hello böngészőben írja be a kapott hello kódot. Biztosan átirányított tooa bejelentkezési oldalára.

![böngésző tooenter kódot][2]

Ha nincs megadva hello kód be van jelentkezve, Bezárás hello böngésző toocontinue hello forgatókönyv.

![sikeres volt][3]

## <a name="switch-tooresource-manager-mode"></a>Váltás tooResource kezelő módban

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Mielőtt létrehozna hello Alkalmazásátjáró, egy erőforráscsoport toocontain hello Alkalmazásátjáró jön létre. hello következő hello parancs jeleníti meg.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása

Hello erőforráscsoport létrehozása után a virtuális hálózati hello Alkalmazásátjáró jön létre.  A következő példa hello hello címterület volt, a forgatókönyv megjegyzések megelőző hello 10.0.0.0/16.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Alhálózat létrehozása

Hello virtuális hálózat létrejötte után, az Alkalmazásátjáró hello alhálózat jelenik meg.  Ha azt tervezi, a webes alkalmazás toouse Alkalmazásátjáró üzemeltetett hello azonos virtuális hello alkalmazás átjáróként hálózati, lehet, hogy tooleave elég hellyel rendelkezzen a másik alhálózaton.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a>Hello Alkalmazásátjáró létrehozása

Hello virtuális hálózati és alhálózati létrehozása után az Alkalmazásátjáró hello hello előfeltételek teljesülnek. Emellett a korábban exportált .pfx tanúsítvány és hello jelszó hello Tanúsítvány szükségesek a következő lépés hello: hello IP-címekre hello háttérkiszolgáló hello IP-címet a háttérkiszolgálón is. Ezek az értékek lehetnek vagy privát IP-címek hello virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Futtassa a következő parancs hello létrehozása során megadható paramétereket listáját: **azure hálózati Alkalmazásátjáró létrehozása – súgó**.

Ez a példa egy alapszintű application gateway hello figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokat hoz létre. Módosíthatja a beállítások toosuit a központi telepítés után hello kiépítés sikeres.
Ha már van definiálva az előző lépésekben létrehozását követően hello hello háttérkészlet webalkalmazásokat terheléselosztás kezdődik.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)

Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
