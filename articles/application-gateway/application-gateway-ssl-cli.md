---
title: "aaaConfigure SSL - Azure Application Gateway - Azure CLI 2.0 kiszervezése |} Microsoft Docs"
description: "Ezen a lapon nyújt útmutatást toocreate Alkalmazásátjáró SSL-kiszervezés Azure CLI 2.0-s verziója"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: f8d50e0c6ffef17c807938d816410e6d85321c9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a><span data-ttu-id="78a60-103">Az SSL-kiszervezés Alkalmazásátjáró konfigurálása Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="78a60-103">Configure an application gateway for SSL offload by using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="78a60-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78a60-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="78a60-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="78a60-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="78a60-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="78a60-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="78a60-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="78a60-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="78a60-108">Az Azure Application Gateway konfigurált tooterminate hello Secure Sockets Layer (SSL) munkamenet: hello átjáró tooavoid költséges SSL visszafejtési feladatok toohappen: hello webfarm lehet.</span><span class="sxs-lookup"><span data-stu-id="78a60-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="78a60-109">SSL kiszervezési is egyszerűbbé teszi a Tanúsítványkezelő hello előtér-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="78a60-109">SSL offload also simplifies certificate management at hello front-end server.</span></span>

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="78a60-110">Előfeltétel: Hello Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="78a60-110">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="78a60-111">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="78a60-111">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="required-components"></a><span data-ttu-id="78a60-112">Szükséges összetevők</span><span class="sxs-lookup"><span data-stu-id="78a60-112">Required components</span></span>

* <span data-ttu-id="78a60-113">**Háttér-kiszolgálófiók készlet:** hello hello háttér-kiszolgálók IP-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="78a60-113">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="78a60-114">hello IP-címek felsorolt toohello virtuális hálózati alhálózat vagy kell tartoznia, vagy egy nyilvános IP-cím/VIP kell lennie.</span><span class="sxs-lookup"><span data-stu-id="78a60-114">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="78a60-115">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="78a60-115">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="78a60-116">Ezek a beállítások esetén tooa kapcsolt verem és a hello készlet alkalmazott tooall-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="78a60-116">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="78a60-117">**Előtér-port:** Ez a port nem hello nyilvános portot, amelyet a hello Alkalmazásátjáró meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="78a60-117">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="78a60-118">Forgalom találatok ezt a portot, és lekérdezi átirányítja tooone hello háttér-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="78a60-118">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="78a60-119">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek a beállítások-és nagybetűk), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="78a60-119">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="78a60-120">**Szabály:** hello szabály hello figyelő és hello háttér-kiszolgálófiók alkalmazáskészlet van kötve, és azt határozza meg, mely háttér-kiszolgálófiók készlet hello forgalom irányított toowhen találatok száma a egy adott figyelő.</span><span class="sxs-lookup"><span data-stu-id="78a60-120">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="78a60-121">Jelenleg csak hello *alapvető* szabály használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="78a60-121">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="78a60-122">Hello *alapvető* szabály-e időszeletelés terheléselosztási.</span><span class="sxs-lookup"><span data-stu-id="78a60-122">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="78a60-123">**További konfigurációs megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="78a60-123">**Additional configuration notes**</span></span>

<span data-ttu-id="78a60-124">SSL-tanúsítványok beállítása, a hello protokoll **HttpListener** túl kell módosítani*Https* (kis-és nagybetűket).</span><span class="sxs-lookup"><span data-stu-id="78a60-124">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="78a60-125">Hello **SslCertificate** elem túl kerül**HttpListener** hello változó értékével hello SSL-tanúsítvány konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="78a60-125">hello **SslCertificate** element is added too**HttpListener** with hello variable value configured for hello SSL certificate.</span></span> <span data-ttu-id="78a60-126">hello előtér-port frissített too443 lehet.</span><span class="sxs-lookup"><span data-stu-id="78a60-126">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="78a60-127">**tooenable cookie-alapú kapcsolat**: Alkalmazásátjáró lehet győződjön meg arról, hogy egy kérelem egy ügyfél-munkamenetből mindig irányított toohello konfigurált tooensure hello webfarm azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="78a60-127">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="78a60-128">Ebben a forgatókönyvben történik, hogy az egy munkamenetcookie-t, amely lehetővé teszi annak megfelelően hello átjáró toodirect forgalom.</span><span class="sxs-lookup"><span data-stu-id="78a60-128">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="78a60-129">cookie-alapú tooenable affinitás beállítása **CookieBasedAffinity** túl*engedélyezve* a hello **BackendHttpSettings** elemet.</span><span class="sxs-lookup"><span data-stu-id="78a60-129">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a><span data-ttu-id="78a60-130">A meglévő Alkalmazásátjáró SSL kiszervezési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78a60-130">Configure SSL offload on an existing application gateway</span></span>

```azurecli-interactive
#!/bin/bash

# Create a new front end port toobe used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload hello .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing hello port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool toobe used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using hello new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking hello listener toohello back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a><span data-ttu-id="78a60-131">Az SSL-kiszervezés Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="78a60-131">Create an application gateway with SSL Offload</span></span>

<span data-ttu-id="78a60-132">a következő minta hello Alkalmazásátjáró SSL kiszervezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="78a60-132">hello following sample creates an application gateway with SSL offload.</span></span>  <span data-ttu-id="78a60-133">hello tanúsítványt és a tanúsítvány jelszava frissített tooa érvényes titkos kulccsal kell lennie.</span><span class="sxs-lookup"><span data-stu-id="78a60-133">hello certificate and certificate password must be updated tooa valid private key.</span></span>

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="78a60-134">Az Application Gateway DNS-nevének beszerzése</span><span class="sxs-lookup"><span data-stu-id="78a60-134">Get application gateway DNS name</span></span>

<span data-ttu-id="78a60-135">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="78a60-135">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="78a60-136">Nyilvános IP-cím esetén az Application Gateway használatához dinamikusan hozzárendelt DNS-névre van szükség, amely nem valódi név.</span><span class="sxs-lookup"><span data-stu-id="78a60-136">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="78a60-137">tooensure a végfelhasználók is találati hello Alkalmazásátjáró, egy olyan CNAME rekordot is használt toopoint toohello nyilvános végpontot hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="78a60-137">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="78a60-138">[Egyéni tartománynév konfigurálása az Azure-ban](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="78a60-138">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="78a60-139">tooconfigure alias, hello Alkalmazásátjáró és a társított IP-/ DNS-nevét, hello PublicIPAddress elem csatolt toohello Alkalmazásátjáró beolvasása.</span><span class="sxs-lookup"><span data-stu-id="78a60-139">tooconfigure an alias, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="78a60-140">hello alkalmazás átjáró DNS-névnek kell lennie a használt toocreate egy olyan CNAME rekordot pontok hello két webes alkalmazások toothis DNS-név.</span><span class="sxs-lookup"><span data-stu-id="78a60-140">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="78a60-141">A-rekordok hello használata nem javasolt, mert hello VIP módosíthatja az Alkalmazásátjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="78a60-141">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="78a60-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78a60-142">Next steps</span></span>

<span data-ttu-id="78a60-143">Ha azt szeretné, hogy egy alkalmazás átjáró toouse egy belső terheléselosztón (ILB) rendelkező tooconfigure, [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="78a60-143">If you want tooconfigure an application gateway toouse with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="78a60-144">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="78a60-144">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="78a60-145">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="78a60-145">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="78a60-146">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="78a60-146">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
