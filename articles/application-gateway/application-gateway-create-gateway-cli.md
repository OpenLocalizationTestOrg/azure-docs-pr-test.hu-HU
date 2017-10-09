---
title: egy Azure Application Gateway - aaaCreate Azure CLI 2.0 |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate használatával Alkalmazásátjáró hello Azure CLI 2.0 az erőforrás-kezelőben"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 952065586cd87d253882438bb779b768d9fd59fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a><span data-ttu-id="63574-103">Alkalmazásátjáró létrehozása hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="63574-103">Create an application gateway by using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="63574-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="63574-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="63574-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="63574-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="63574-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="63574-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="63574-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="63574-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="63574-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="63574-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="63574-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="63574-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="63574-110">Alkalmazásátjáró egy dedikált virtuális készülék szolgáltatásként, az alkalmazás kézbesítési vezérlő (LÉPETT) biztosító ajánlat különböző réteg 7 betölteni az alkalmazás terheléselosztási képességeit.</span><span class="sxs-lookup"><span data-stu-id="63574-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="63574-111">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="63574-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="63574-112">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="63574-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="63574-113">[Az Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="63574-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="63574-114">[Az Azure CLI 2.0](application-gateway-create-gateway-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="63574-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="63574-115">Előfeltétel: Hello Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="63574-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="63574-116">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="63574-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="63574-117">Ha az Azure-fiók nem rendelkezik, meg kell egyet.</span><span class="sxs-lookup"><span data-stu-id="63574-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="63574-118">Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="63574-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="63574-119">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="63574-119">Scenario</span></span>

<span data-ttu-id="63574-120">Ebben a forgatókönyvben a megismerheti, hogyan egy alkalmazást átjáró, amely toocreate hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="63574-120">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="63574-121">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="63574-121">This scenario will:</span></span>

* <span data-ttu-id="63574-122">Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.</span><span class="sxs-lookup"><span data-stu-id="63574-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="63574-123">Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="63574-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="63574-124">Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.</span><span class="sxs-lookup"><span data-stu-id="63574-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="63574-125">További konfigurációs hello Alkalmazásátjáró, beleértve az egyéni rendszerállapot-vizsgálat, háttér címkészletet címek és a további szabályok hello Alkalmazásátjáró konfigurálása után, és nem a kezdeti telepítése során konfigurált.</span><span class="sxs-lookup"><span data-stu-id="63574-125">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="63574-126">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="63574-126">Before you begin</span></span>

<span data-ttu-id="63574-127">Az Azure Application Gateway saját alhálózatba van szükség.</span><span class="sxs-lookup"><span data-stu-id="63574-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="63574-128">Virtuális hálózat létrehozásakor győződjön meg arról, hogy hagyja elég cím terület toohave több alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="63574-128">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="63574-129">Miután telepít egy alkalmazást tooa átjáróalhálózatot, csak további alkalmazásátjárót toohello alhálózati lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="63574-129">Once you deploy an application gateway tooa subnet, only additional application gateways can be added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="63574-130">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="63574-130">Log in tooAzure</span></span>

<span data-ttu-id="63574-131">Nyissa meg hello **Microsoft Azure parancssori**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="63574-131">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="63574-132">Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő hello kapcsoló nélkül.</span><span class="sxs-lookup"><span data-stu-id="63574-132">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="63574-133">Amennyiben az előző példa hello írja be, a kód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="63574-133">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="63574-134">Keresse meg a böngésző toocontinue hello bejelentkezési folyamat toohttps://aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="63574-134">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd megjelenítő eszköz-bejelentkezés][1]

<span data-ttu-id="63574-136">Hello böngészőben írja be a kapott hello kódot.</span><span class="sxs-lookup"><span data-stu-id="63574-136">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="63574-137">Biztosan átirányított tooa bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="63574-137">You are redirected tooa sign-in page.</span></span>

![böngésző tooenter kódot][2]

<span data-ttu-id="63574-139">Ha nincs megadva hello kód be van jelentkezve, Bezárás hello böngésző toocontinue hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="63574-139">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![sikeres volt][3]

## <a name="create-hello-resource-group"></a><span data-ttu-id="63574-141">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="63574-141">Create hello resource group</span></span>

<span data-ttu-id="63574-142">Mielőtt létrehozna hello Alkalmazásátjáró, egy erőforráscsoport toocontain hello Alkalmazásátjáró jön létre.</span><span class="sxs-lookup"><span data-stu-id="63574-142">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="63574-143">hello következő hello parancs jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="63574-143">hello following shows hello command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="63574-144">Hello Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="63574-144">Create hello application gateway</span></span>

<span data-ttu-id="63574-145">hello IP-címekre hello háttérkiszolgáló hello IP-címet a háttérkiszolgálón is.</span><span class="sxs-lookup"><span data-stu-id="63574-145">hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="63574-146">Ezek az értékek lehetnek vagy privát IP-címek hello virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="63574-146">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="63574-147">hello alábbi példa hoz létre a meglévő Alkalmazásátjáró http-beállítások, a portok és a szabályok további konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="63574-147">hello following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

<span data-ttu-id="63574-148">hello előző példa sok tulajdonságok láthatók, amelyek hello Alkalmazásátjáró létrehozása során nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="63574-148">hello preceding example shows many properties that are not required during hello creation of an application gateway.</span></span> <span data-ttu-id="63574-149">a következő példakód hello Alkalmazásátjáró hoz létre a hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="63574-149">hello following code example creates an application gateway with hello required information.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> <span data-ttu-id="63574-150">A következő parancs futtatása létrehozása hello során megadható paramétereket listáját: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="63574-150">For a list of parameters that can be provided during creation run hello following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="63574-151">Ez a példa egy alapszintű application gateway hello figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok az alapértelmezett beállításokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="63574-151">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="63574-152">Módosíthatja a beállítások toosuit a központi telepítés után hello kiépítés sikeres.</span><span class="sxs-lookup"><span data-stu-id="63574-152">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="63574-153">Ha már van definiálva az előző lépésekben létrehozását követően hello hello háttérkészlet webalkalmazásokat terheléselosztás kezdődik.</span><span class="sxs-lookup"><span data-stu-id="63574-153">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="63574-154">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="63574-154">Get application gateway DNS name</span></span>

<span data-ttu-id="63574-155">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="63574-155">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="63574-156">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="63574-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="63574-157">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="63574-157">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="63574-158">[Egyéni tartománynév konfigurálása az Azure-ban](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="63574-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="63574-159">tooconfigure alias, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="63574-159">tooconfigure an alias, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="63574-160">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="63574-160">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="63574-161">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="63574-161">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a><span data-ttu-id="63574-162">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="63574-162">Delete all resources</span></span>

<span data-ttu-id="63574-163">toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="63574-163">toodelete all resources created in this article, complete hello following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="63574-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63574-164">Next steps</span></span>

<span data-ttu-id="63574-165">Ismerje meg, hogyan toocreate egyéni állapot-mintavételi csomagjai ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="63574-165">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="63574-166">Ismerje meg, hogyan SSL-Feladatkiszervezést tooconfigure és hajtsa végre a megfelelő hello költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="63574-166">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
