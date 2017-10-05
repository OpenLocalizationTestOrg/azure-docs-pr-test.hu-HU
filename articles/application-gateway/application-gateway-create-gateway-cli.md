---
title: "Hozzon létre egy Azure Application Gateway - Azure CLI 2.0 |} Microsoft Docs"
description: "Útmutató Alkalmazásátjáró létrehozása az Azure CLI 2.0 erőforrás-kezelő használatával"
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
ms.openlocfilehash: 052410db8c7619c7990dc319951a55663f2c2ba1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a><span data-ttu-id="ec13c-103">Alkalmazásátjáró létrehozása az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="ec13c-103">Create an application gateway by using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec13c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ec13c-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="ec13c-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec13c-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="ec13c-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec13c-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="ec13c-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="ec13c-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="ec13c-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ec13c-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="ec13c-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ec13c-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="ec13c-110">Alkalmazásátjáró egy dedikált virtuális készülék szolgáltatásként, az alkalmazás kézbesítési vezérlő (LÉPETT) biztosító ajánlat különböző réteg 7 betölteni az alkalmazás terheléselosztási képességeit.</span><span class="sxs-lookup"><span data-stu-id="ec13c-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="ec13c-111">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="ec13c-111">CLI versions to complete the task</span></span>

<span data-ttu-id="ec13c-112">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="ec13c-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="ec13c-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez.</span><span class="sxs-lookup"><span data-stu-id="ec13c-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="ec13c-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="ec13c-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="prerequisite-install-the-azure-cli-20"></a><span data-ttu-id="ec13c-115">Előfeltétel: Az Azure parancssori felület 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="ec13c-115">Prerequisite: Install the Azure CLI 2.0</span></span>

<span data-ttu-id="ec13c-116">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="ec13c-116">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="ec13c-117">Ha az Azure-fiók nem rendelkezik, meg kell egyet.</span><span class="sxs-lookup"><span data-stu-id="ec13c-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="ec13c-118">Itt regisztrálhat az [ingyenes próbaverzióra](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="ec13c-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="ec13c-119">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ec13c-119">Scenario</span></span>

<span data-ttu-id="ec13c-120">Ebben a forgatókönyvben ismerje meg az Azure portál használatával Alkalmazásátjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ec13c-120">In this scenario, you learn how to create an application gateway using the Azure portal.</span></span>

<span data-ttu-id="ec13c-121">Ez a forgatókönyv tartalma:</span><span class="sxs-lookup"><span data-stu-id="ec13c-121">This scenario will:</span></span>

* <span data-ttu-id="ec13c-122">Hozzon létre egy közepes méretű Alkalmazásátjáró két példányt.</span><span class="sxs-lookup"><span data-stu-id="ec13c-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="ec13c-123">Nevű AdatumAppGatewayVNET egy fenntartott CIDR-blokkja 10.0.0.0/16, a virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ec13c-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="ec13c-124">Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.</span><span class="sxs-lookup"><span data-stu-id="ec13c-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="ec13c-125">Az Alkalmazásátjáró, beleértve az egyéni állapotfigyelő további konfigurációs vizsgálat, háttér címkészletet címeket, és további szabályok is konfigurálva van az Alkalmazásátjáró konfigurálása után, és nem a kezdeti üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="ec13c-125">Additional configuration of the application gateway, including custom health probes, backend pool addresses, and additional rules are configured after the application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ec13c-126">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ec13c-126">Before you begin</span></span>

<span data-ttu-id="ec13c-127">Az Azure Application Gateway saját alhálózatba van szükség.</span><span class="sxs-lookup"><span data-stu-id="ec13c-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="ec13c-128">Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy hagyja-e elég hely a cím több alhálózattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ec13c-128">When creating a virtual network, ensure that you leave enough address space to have multiple subnets.</span></span> <span data-ttu-id="ec13c-129">Miután telepít egy alhálózatra Alkalmazásátjáró, csak további alkalmazásátjárót alhálózathoz lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ec13c-129">Once you deploy an application gateway to a subnet, only additional application gateways can be added to the subnet.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="ec13c-130">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="ec13c-130">Log in to Azure</span></span>

<span data-ttu-id="ec13c-131">Nyissa meg a **Microsoft Azure parancssori**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="ec13c-131">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="ec13c-132">Is `az login` az eszköz bejelentkezéshez írja be a kódot, aka.ms/devicelogin igénylő kapcsoló nélkül.</span><span class="sxs-lookup"><span data-stu-id="ec13c-132">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="ec13c-133">Miután beírta a fenti példában, a kód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="ec13c-133">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="ec13c-134">Nyissa meg a böngészőben a bejelentkezési folyamat folytatásához https://aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="ec13c-134">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd megjelenítő eszköz-bejelentkezés][1]

<span data-ttu-id="ec13c-136">A böngészőben írja be a kapott kódot.</span><span class="sxs-lookup"><span data-stu-id="ec13c-136">In the browser, enter the code you received.</span></span> <span data-ttu-id="ec13c-137">Ekkor megnyílik egy bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="ec13c-137">You are redirected to a sign-in page.</span></span>

![Írja be a kódját a böngésző][2]

<span data-ttu-id="ec13c-139">Ha nincs megadva a kód be van jelentkezve, zárja be a böngészőt, és a forgatókönyv a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ec13c-139">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![sikeres volt][3]

## <a name="create-the-resource-group"></a><span data-ttu-id="ec13c-141">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec13c-141">Create the resource group</span></span>

<span data-ttu-id="ec13c-142">Az Alkalmazásátjáró létrehozása, mielőtt egy erőforráscsoportot hoz létre az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="ec13c-142">Before creating the application gateway, a resource group is created to contain the application gateway.</span></span> <span data-ttu-id="ec13c-143">Az alábbiakban a parancs látható.</span><span class="sxs-lookup"><span data-stu-id="ec13c-143">The following shows the command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a><span data-ttu-id="ec13c-144">Application Gateway létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec13c-144">Create the application gateway</span></span>

<span data-ttu-id="ec13c-145">A háttérkiszolgáló az IP-címeit az IP-címet a háttérkiszolgálón is.</span><span class="sxs-lookup"><span data-stu-id="ec13c-145">The IP addresses used for the backend are the IP addresses for your backend server.</span></span> <span data-ttu-id="ec13c-146">Ezek az értékek lehetnek privát IP-címek a virtuális hálózat, nyilvános IP-cím vagy teljes tartománynevét a háttérkiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="ec13c-146">These values can be either private IPs in the virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="ec13c-147">A következő példa olyan átjárót hoz létre a http-beállítások, a portok és a szabályok további konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="ec13c-147">The following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

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

<span data-ttu-id="ec13c-148">Az előző példában látható sok tulajdonságok esetében nem szükséges Alkalmazásátjáró létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="ec13c-148">The preceding example shows many properties that are not required during the creation of an application gateway.</span></span> <span data-ttu-id="ec13c-149">A következő kódrészlet példa olyan átjárót hoz létre a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec13c-149">The following code example creates an application gateway with the required information.</span></span>

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
> <span data-ttu-id="ec13c-150">A következő parancs létrehozása során megadható paramétereket listáját: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="ec13c-150">For a list of parameters that can be provided during creation run the following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="ec13c-151">Ez a példa egy alapszintű application gateway alapértelmezett beállításait a figyelő, a háttérkészlet, a backendhttpsettings osztályhoz és a szabályok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ec13c-151">This example creates a basic application gateway with default settings for the listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="ec13c-152">Ezeket a beállításokat a központi telepítés megfelelően, ha a kiépítés sikeres módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ec13c-152">You can modify these settings to suit your deployment once the provisioning is successful.</span></span>
<span data-ttu-id="ec13c-153">Ha már van definiálva az előző lépésekben, a létrehozását követően, a háttérkészletben webalkalmazásokat terheléselosztás kezdődik.</span><span class="sxs-lookup"><span data-stu-id="ec13c-153">If you already have your web application defined with the backend pool in the preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="ec13c-154">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="ec13c-154">Get application gateway DNS name</span></span>

<span data-ttu-id="ec13c-155">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ec13c-155">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="ec13c-156">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="ec13c-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="ec13c-157">Ha szeretné, hogy a végfelhasználók elérjék az Application Gatewayt, használjon egy Application Gateway nyilvános végpontjára mutató CNAME-rekordot.</span><span class="sxs-lookup"><span data-stu-id="ec13c-157">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="ec13c-158">[Egyéni tartománynév konfigurálása az Azure-ban](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="ec13c-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="ec13c-159">Alias konfigurálásához az Alkalmazásátjáró és a társított IP-/ DNS-nevét, a PublicIPAddress elem csatolva az Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ec13c-159">To configure an alias, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="ec13c-160">Az Application Gateway DNS-nevének használatával létrehozhat egy CNAME rekordot, amely a két webalkalmazást erre a DNS-névre irányítja.</span><span class="sxs-lookup"><span data-stu-id="ec13c-160">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="ec13c-161">Az A-bejegyzések használata nem javasolt, mivel a virtuális IP-cím változhat az Application Gateway újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="ec13c-161">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="delete-all-resources"></a><span data-ttu-id="ec13c-162">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="ec13c-162">Delete all resources</span></span>

<span data-ttu-id="ec13c-163">A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ec13c-163">To delete all resources created in this article, complete the following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="ec13c-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec13c-164">Next steps</span></span>

<span data-ttu-id="ec13c-165">Megtudhatja, hogyan hozzon létre egyéni állapotteljesítmény ellátogatva [hozzon létre egy egyéni állapotmintáihoz](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ec13c-165">Learn how to create custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="ec13c-166">Megtudhatja, hogyan konfigurálja az SSL-Feladatkiszervezést, és tegye meg a költséges SSL visszafejtési ki a webkiszolgálók ellátogatva [SSL-kiszervezés konfigurálása](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="ec13c-166">Learn how to configure SSL Offloading and take the costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
