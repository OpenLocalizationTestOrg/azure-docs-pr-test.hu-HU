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
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a><span data-ttu-id="f5bfb-103">Alkalmazásátjáró létrehozása hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="f5bfb-103">Create an application gateway by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5bfb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5bfb-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="f5bfb-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5bfb-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="f5bfb-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5bfb-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="f5bfb-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="f5bfb-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="f5bfb-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f5bfb-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="f5bfb-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f5bfb-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)
> 
> 

<span data-ttu-id="f5bfb-110">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-110">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="f5bfb-111">Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-111">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="f5bfb-112">Alkalmazásátjáró rendelkezik a következő kézbesítési alkalmazásszolgáltatáshoz hello: HTTP terheléselosztás, a munkamenet cookie-alapú kapcsolat és a Secure Sockets Layer (SSL) kiszervezési egyéni állapotfigyelő mintavételt betölteni, és többhelyes támogatás.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-112">Application gateway has hello following application delivery features: HTTP load balancing, cookie-based session affinity, and Secure Sockets Layer (SSL) offload, custom health probes, and support for multi-site.</span></span>

## <a name="prerequisite-install-hello-azure-cli"></a><span data-ttu-id="f5bfb-113">Előfeltétel: Hello Azure parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="f5bfb-113">Prerequisite: Install hello Azure CLI</span></span>

<span data-ttu-id="f5bfb-114">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](../xplat-cli-install.md) túl van szüksége, és[tooAzure bejelentkezés](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f5bfb-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](../xplat-cli-install.md) and you need too[log on tooAzure](../xplat-cli-connect.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="f5bfb-115">Ha az Azure-fiók nem rendelkezik, meg kell egyet.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-115">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="f5bfb-116">Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="f5bfb-116">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="f5bfb-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f5bfb-117">Scenario</span></span>

<span data-ttu-id="f5bfb-118">Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-118">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="f5bfb-119">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="f5bfb-119">This scenario will:</span></span>

* <span data-ttu-id="f5bfb-120">Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-120">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="f5bfb-121">Nevű ContosoVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-121">Create a virtual network named ContosoVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="f5bfb-122">A CIDR-blokkja 10.0.0.0/28 használó subnet01 nevű alhálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-122">Create a subnet called subnet01 that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="f5bfb-123">További konfigurációs hello Alkalmazásátjáró, beleértve az egyéni rendszerállapot-vizsgálat, háttér címkészletet címek és a további szabályok hello Alkalmazásátjáró konfigurálása után, és nem a kezdeti telepítése során konfigurált.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-123">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f5bfb-124">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f5bfb-124">Before you begin</span></span>

<span data-ttu-id="f5bfb-125">Az Azure Application Gateway saját alhálózatba van szükség.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-125">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="f5bfb-126">Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-126">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="f5bfb-127">Miután telepít egy alkalmazást tooa átjáróalhálózatot, csak további alkalmazásátjárót-e fel tudja toobe toohello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-127">Once you deploy an application gateway tooa subnet, only additional application gateways are able toobe added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="f5bfb-128">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="f5bfb-128">Log in tooAzure</span></span>

<span data-ttu-id="f5bfb-129">Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-129">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
azure login
```

<span data-ttu-id="f5bfb-130">Amennyiben az előző példa hello írja be, a kód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-130">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="f5bfb-131">Keresse meg a böngésző toocontinue hello bejelentkezési folyamat toohttps://aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-131">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd megjelenítő eszköz-bejelentkezés][1]

<span data-ttu-id="f5bfb-133">Hello böngészőben írja be a kapott hello kódot.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-133">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="f5bfb-134">Biztosan átirányított tooa bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-134">You are redirected tooa sign-in page.</span></span>

![böngésző tooenter kódot][2]

<span data-ttu-id="f5bfb-136">Ha nincs megadva hello kód be van jelentkezve, Bezárás hello böngésző toocontinue hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-136">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![sikeres volt][3]

## <a name="switch-tooresource-manager-mode"></a><span data-ttu-id="f5bfb-138">Váltás tooResource kezelő módban</span><span class="sxs-lookup"><span data-stu-id="f5bfb-138">Switch tooResource Manager Mode</span></span>

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a><span data-ttu-id="f5bfb-139">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5bfb-139">Create hello resource group</span></span>

<span data-ttu-id="f5bfb-140">Mielőtt létrehozna hello Alkalmazásátjáró, egy erőforráscsoport toocontain hello Alkalmazásátjáró jön létre.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-140">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="f5bfb-141">hello következő hello parancs jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-141">hello following shows hello command.</span></span>

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="f5bfb-142">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5bfb-142">Create a virtual network</span></span>

<span data-ttu-id="f5bfb-143">Hello erőforráscsoport létrehozása után a virtuális hálózati hello Alkalmazásátjáró jön létre.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-143">Once hello resource group is created, a virtual network is created for hello application gateway.</span></span>  <span data-ttu-id="f5bfb-144">A következő példa hello hello címterület volt, a forgatókönyv megjegyzések megelőző hello 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-144">In hello following example, hello address space was as 10.0.0.0/16 as defined in hello preceding scenario notes.</span></span>

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a><span data-ttu-id="f5bfb-145">Alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5bfb-145">Create a subnet</span></span>

<span data-ttu-id="f5bfb-146">Hello virtuális hálózat létrejötte után, az Alkalmazásátjáró hello alhálózat jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-146">After hello virtual network is created, a subnet is added for hello application gateway.</span></span>  <span data-ttu-id="f5bfb-147">Ha azt tervezi, a webes alkalmazás toouse Alkalmazásátjáró üzemeltetett hello azonos virtuális hello alkalmazás átjáróként hálózati, lehet, hogy tooleave elég hellyel rendelkezzen a másik alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-147">If you plan toouse application gateway with a web app hosted in hello same virtual network as hello application gateway, be sure tooleave enough room for another subnet.</span></span>

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="f5bfb-148">Hello Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5bfb-148">Create hello application gateway</span></span>

<span data-ttu-id="f5bfb-149">Hello virtuális hálózati és alhálózati létrehozása után az Alkalmazásátjáró hello hello előfeltételek teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-149">Once hello virtual network and subnet are created, hello pre-requisites for hello application gateway are complete.</span></span> <span data-ttu-id="f5bfb-150">Emellett a korábban exportált .pfx tanúsítvány és hello jelszó hello Tanúsítvány szükségesek a következő lépés hello: hello IP-címekre hello háttérkiszolgáló hello IP-címet a háttérkiszolgálón is.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-150">Additionally a previously exported .pfx certificate and hello password for hello certificate are required for hello following step: hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="f5bfb-151">Ezek az értékek lehetnek vagy privát IP-címek hello virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-151">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span>

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
> <span data-ttu-id="f5bfb-152">Futtassa a következő parancs hello létrehozása során megadható paramétereket listáját: **azure hálózati Alkalmazásátjáró létrehozása – súgó**.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-152">For a list of parameters that can be provided during creation run hello following command: **azure network application-gateway create --help**.</span></span>

<span data-ttu-id="f5bfb-153">Ez a példa egy alapszintű application gateway hello figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-153">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="f5bfb-154">Módosíthatja a beállítások toosuit a központi telepítés után hello kiépítés sikeres.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-154">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="f5bfb-155">Ha már van definiálva az előző lépésekben létrehozását követően hello hello háttérkészlet webalkalmazásokat terheléselosztás kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f5bfb-155">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5bfb-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5bfb-156">Next steps</span></span>

<span data-ttu-id="f5bfb-157">Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="f5bfb-157">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="f5bfb-158">Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="f5bfb-158">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
