---
title: "Azure API Management virtuális hálózatban az Alkalmazásátjáró aaaHow toouse |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup és belső virtuális hálózat az alkalmazás átjáró (waf-ot), amelynek az Azure API Management konfigurálása"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="89256-103">Egy belső virtuális Hálózatot az API Management integrálása Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="89256-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="89256-104"><a name="overview"></a> – Áttekintés</span><span class="sxs-lookup"><span data-stu-id="89256-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="89256-105">hello API-kezelés szolgáltatás konfigurálható egy virtuális hálózatot belső módban, így azokat csak hello virtuális hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="89256-105">hello API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within hello Virtual Network.</span></span> <span data-ttu-id="89256-106">Az Azure Application Gateway egy olyan PAAS szolgáltatás, amely réteg-7 terheléselosztót biztosít.</span><span class="sxs-lookup"><span data-stu-id="89256-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="89256-107">Fordított proxy szolgáltatásként működik, és biztosít ajánlat egy webes alkalmazás tűzfalat (WAF) között.</span><span class="sxs-lookup"><span data-stu-id="89256-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="89256-108">Egy belső virtuális Hálózatot a hello Alkalmazásátjáró előtér kiépítve API Management kombinálásával lehetővé teszi, hogy a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="89256-108">Combining API Management provisioned in an internal VNET with hello Application Gateway frontend enables hello following scenarios:</span></span>

* <span data-ttu-id="89256-109">Használjon hello azonos API-kezelés erőforráshoz a belső fogyasztók és a külső felhasználók által megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="89256-109">Use hello same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="89256-110">Egyetlen API Management erőforrást használ, és rendelkezik a külső felhasználók API Management érhető el a megadott API-t.</span><span class="sxs-lookup"><span data-stu-id="89256-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="89256-111">Adjon meg egy kulcsrakész módon tooswitch hozzáférés tooAPI felügyeleti hello a nyilvános interneten be- és kikapcsolható.</span><span class="sxs-lookup"><span data-stu-id="89256-111">Provide a turn-key way tooswitch access tooAPI Management from hello public Internet on and off.</span></span> 

##<span data-ttu-id="89256-112"><a name="scenario"></a> Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="89256-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="89256-113">Ez a cikk foglalkozik, hogyan toouse egyetlen API-kezelési szolgáltatás fut a külső és belső fogyasztók, és a működjön, és egyetlen időtúllépést a két helyszíni és felhőalapú API-k révén.</span><span class="sxs-lookup"><span data-stu-id="89256-113">This article will cover how toouse a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="89256-114">Látni fogja emellett hogyan tooexpose az API-k (hello példa zöld kijelölt) csak egy részhalmazát hello PathBasedRouting funkció, amely az alkalmazás átjáró használata külső felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="89256-114">You will also see how tooexpose only a subset of your APIs (in hello example they are highlighted in green) for External Consumption using hello PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="89256-115">Hello első telepítő példában minden API felügyelt csak virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="89256-115">In hello first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="89256-116">Belső fogyasztóknak (a kijelölt narancs) férhetnek hozzá a belső és külső API.</span><span class="sxs-lookup"><span data-stu-id="89256-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="89256-117">Forgalom soha nem kerül ki a rendszer egy nagy teljesítményű tooInternet Expressroute-Kapcsolatcsoportok keresztül.</span><span class="sxs-lookup"><span data-stu-id="89256-117">Traffic never goes out tooInternet a high performance is delivered via Express Route circuits.</span></span>

![URL-cím útvonal](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="89256-119"><a name="before-you-begin"></a> Megkezdése előtt</span><span class="sxs-lookup"><span data-stu-id="89256-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="89256-120">Webplatform-telepítő hello segítségével hello hello Azure PowerShell-parancsmagok legújabb verzióját telepítse.</span><span class="sxs-lookup"><span data-stu-id="89256-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="89256-121">Töltse le, és telepítse hello legújabb verziót a hello **Windows PowerShell** hello szakasza [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="89256-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="89256-122">Hozzon létre egy virtuális hálózatot, és hozzon létre különálló alhálózatokat az API Management és az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="89256-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="89256-123">Ha azt tervezi, hogy egy egyéni DNS-kiszolgáló a virtuális hálózati hello toocreate, megteheti hello központi telepítés elindítása előtt.</span><span class="sxs-lookup"><span data-stu-id="89256-123">If you intend toocreate a custom DNS server for hello Virtual Network, do so before starting hello deployment.</span></span> <span data-ttu-id="89256-124">Ellenőrizze a virtuális hálózati hello új alhálózat létrehozott virtuális gépek biztosításával működik megoldhatja és Azure szolgáltatásvégpontokra eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="89256-124">Double check it works by ensuring a virtual machine created in a new subnet in hello Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="89256-125">Mi az az API Management és az Application Gateway közötti integrációs szükséges toocreate?</span><span class="sxs-lookup"><span data-stu-id="89256-125">What is required toocreate an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="89256-126">**Háttér-kiszolgálófiók készlet:** hello belső virtuális IP-címét az API Management szolgáltatás hello azt.</span><span class="sxs-lookup"><span data-stu-id="89256-126">**Back-end server pool:** This is hello internal virtual IP address of hello API Management service.</span></span>
* <span data-ttu-id="89256-127">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="89256-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="89256-128">Ezek a beállítások hello készlet alkalmazott tooall-kiszolgálók esetén.</span><span class="sxs-lookup"><span data-stu-id="89256-128">These settings are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="89256-129">**Előtér-port:** hello nyilvános port már meg van nyitva a hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="89256-129">**Front-end port:** This is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="89256-130">Forgalom elérte-e azt lekérése a hello átirányított tooone háttérkiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="89256-130">Traffic hitting it gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="89256-131">**Figyelő:** hello figyelő rendelkezik egy előtér-portot, a protokollt (Http vagy Https, ezek az értékek kis-és nagybetűket), és hello SSL tanúsítvány neve (ha az SSL beállításának-kiszervezés).</span><span class="sxs-lookup"><span data-stu-id="89256-131">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="89256-132">**Szabály:** hello szabály van kötve egy figyelő tooa háttér-kiszolgálókészletet.</span><span class="sxs-lookup"><span data-stu-id="89256-132">**Rule:** hello rule binds a listener tooa back-end server pool.</span></span>
* <span data-ttu-id="89256-133">**Egyéni Állapotmintáihoz:** Alkalmazásátjáró, alapértelmezés szerint használt IP-cím alapú mintavételt toofigure meg azok a kiszolgálók a hello BackendAddressPool aktívak.</span><span class="sxs-lookup"><span data-stu-id="89256-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes toofigure out which servers in hello BackendAddressPool are active.</span></span> <span data-ttu-id="89256-134">hello API Management szolgáltatást csak hello megfelelő állomásfejléc rendelkező toorequests válaszol, ezért a hello alapértelmezett mintavételt művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="89256-134">hello API Management service only responds toorequests which have hello correct host header, hence hello default probes fail.</span></span> <span data-ttu-id="89256-135">Egy egyéni állapotmintáihoz kell definiált toobe toohelp Alkalmazásátjáró határozza meg, hogy hello szolgáltatás életben, és továbbítsa a kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="89256-135">A custom health probe needs toobe defined toohelp application gateway determine that hello service is alive and it should forward requests.</span></span>
* <span data-ttu-id="89256-136">**Az egyéni tartomány tanúsítvány:** hello az API Management tooaccess internet toocreate az állomásnév toohello Alkalmazásátjáró előtér-DNS-név egy CNAME-leképezés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="89256-136">**Custom domain certificate:** tooaccess API Management from hello internet you need toocreate a CNAME mapping of its hostname toohello Application Gateway front-end DNS name.</span></span> <span data-ttu-id="89256-137">Ez biztosítja, hogy hello állomásnév fejléc és küldött tanúsítvány tooApplication tooAPI továbbított átjáró felügyeleti egy APIM fel tudja ismerni, mint érvényes.</span><span class="sxs-lookup"><span data-stu-id="89256-137">This ensures that hello hostname header and certificate sent tooApplication Gateway that is forwarded tooAPI Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="89256-138"><a name="overview-steps"></a> API Management és az Application Gateway integrálásához szükséges lépések</span><span class="sxs-lookup"><span data-stu-id="89256-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="89256-139">Egy erőforráscsoport létrehozása a Resource Manager számára.</span><span class="sxs-lookup"><span data-stu-id="89256-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="89256-140">Hozzon létre egy virtuális hálózati alhálózat és nyilvános IP-Címek hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="89256-140">Create a Virtual Network, subnet, and public IP for hello Application Gateway.</span></span> <span data-ttu-id="89256-141">Hozzon létre egy másik alhálózatot az API Management.</span><span class="sxs-lookup"><span data-stu-id="89256-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="89256-142">Hozzon létre egy API-kezelés szolgáltatás a fenti létrehozott hello VNET subnet belül, és győződjön meg arról, hello belső módot használja.</span><span class="sxs-lookup"><span data-stu-id="89256-142">Create an API Management service inside hello VNET subnet created above and ensure you use hello Internal mode.</span></span>
4. <span data-ttu-id="89256-143">A telepítő egyéni tartománynevet hello hello API-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="89256-143">Setup hello custom domain name in hello API Management service.</span></span>
5. <span data-ttu-id="89256-144">Az Application Gateway konfigurációs objektum létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="89256-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="89256-145">Alkalmazásátjáró erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89256-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="89256-146">Hozzon létre egy CNAME REKORDOT a hello Alkalmazásátjáró toohello API Management proxy állomásnév hello nyilvános DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="89256-146">Create a CNAME from hello public DNS name of hello Application Gateway toohello API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="89256-147">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="89256-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="89256-148">Győződjön meg arról, hogy azok be hello Azure PowerShell legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="89256-148">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="89256-149">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="89256-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="89256-150">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-150">Step 1</span></span>

<span data-ttu-id="89256-151">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="89256-151">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="89256-152">Azokkal a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="89256-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="89256-153">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-153">Step 2</span></span>

<span data-ttu-id="89256-154">Ellenőrizze a hello előfizetések hello fiókhoz, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="89256-154">Check hello subscriptions for hello account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="89256-155">3. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-155">Step 3</span></span>

<span data-ttu-id="89256-156">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="89256-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="89256-157">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="89256-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="89256-158">Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál.</span><span class="sxs-lookup"><span data-stu-id="89256-158">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="89256-159">Győződjön meg arról, hogy az összes parancsok toocreate egy alkalmazás átjáró használata hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="89256-159">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="89256-160">Hozzon létre egy virtuális hálózatot és hello Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="89256-160">Create a Virtual Network and a subnet for hello application gateway</span></span>

<span data-ttu-id="89256-161">hello a következő példa bemutatja, hogyan egy virtuális hálózat használatával toocreate hello erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="89256-161">hello following example shows how toocreate a Virtual Network using hello resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="89256-162">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-162">Step 1</span></span>

<span data-ttu-id="89256-163">Rendelje hozzá a hello cím tartomány 10.0.0.0/24 toohello alhálózati változó toobe virtuális hálózat létrehozása során használt.</span><span class="sxs-lookup"><span data-stu-id="89256-163">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="89256-164">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-164">Step 2</span></span>

<span data-ttu-id="89256-165">Rendelje hozzá a hello cím tartomány 10.0.1.0/24 toohello alhálózati változó toobe API-felügyelethez használt virtuális hálózat létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="89256-165">Assign hello address range 10.0.1.0/24 toohello subnet variable toobe used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="89256-166">3. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-166">Step 3</span></span>

<span data-ttu-id="89256-167">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **apim-appGw-RG** hello USA nyugati régiójában hello előtag 10.0.0.0/16 használatával a 10.0.0.0/24 alhálózat és 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="89256-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for hello West US region using hello prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="89256-168">4. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-168">Step 4</span></span>

<span data-ttu-id="89256-169">Rendelje hozzá a következő lépéseket hello alhálózati változó</span><span class="sxs-lookup"><span data-stu-id="89256-169">Assign a subnet variable for hello next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="89256-170">Egy belső módban konfigurált virtuális hálózaton belül az API Management szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="89256-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="89256-171">hello következő példa bemutatja, hogyan toocreate egy API-kezelés szolgáltatás a VNETEN belül beállítást csak a belső hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="89256-171">hello following example shows how toocreate an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="89256-172">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-172">Step 1</span></span>
<span data-ttu-id="89256-173">A fenti létrehozott $apimsubnetdata hello alhálózati használata API felügyeleti virtuális hálózati objektumot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="89256-173">Create an API Management Virtual Network object using hello subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="89256-174">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-174">Step 2</span></span>
<span data-ttu-id="89256-175">Hello virtuális hálózaton belül az API Management szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89256-175">Create an API Management service inside hello Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="89256-176">Hello fent parancs sikeres követően tekintse meg a túl[DNS-konfiguráció szükséges tooaccess belső hálózatok API-kezelés szolgáltatás](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess azt.</span><span class="sxs-lookup"><span data-stu-id="89256-176">After hello above command succeeds refer too[DNS Configuration required tooaccess internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="89256-177">Telepítés egy egyéni tartománynevet, az API Management</span><span class="sxs-lookup"><span data-stu-id="89256-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="89256-178">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-178">Step 1</span></span>
<span data-ttu-id="89256-179">Hello-tanúsítvány feltöltése hello tartományhoz tartozó titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="89256-179">Upload hello certificate with private key for hello domain.</span></span> <span data-ttu-id="89256-180">Ehhez a példához lesz `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="89256-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="89256-181">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-181">Step 2</span></span>
<span data-ttu-id="89256-182">Hello tanúsítványt a feltöltést követően hozzon létre egy állomásnév konfigurációs objektuma hello proxy egy állomásnevét `api.contoso.net`, mert a hello példa tanúsítvány hatóság biztosít hello `*.contoso.net` tartomány.</span><span class="sxs-lookup"><span data-stu-id="89256-182">Once hello certificate is uploaded, create a hostname configuration object for hello proxy with a hostname of `api.contoso.net`, as hello example certificate provides authority for hello  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="89256-183">A nyilvános IP-cím hello előtér-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="89256-183">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="89256-184">Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **apim-appGw-RG** hello USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="89256-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="89256-185">IP-cím hozzá van rendelve toohello Alkalmazásátjáró hello szolgáltatás elindulásakor.</span><span class="sxs-lookup"><span data-stu-id="89256-185">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="89256-186">Alkalmazás átjáró-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="89256-186">Create application gateway configuration</span></span>

<span data-ttu-id="89256-187">Az összes konfigurációs elemek hello Alkalmazásátjáró létrehozása előtt kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="89256-187">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="89256-188">hello lépések hello konfigurációs elemek létrehozását, amelyek szükségesek az alkalmazás átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="89256-188">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="89256-189">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-189">Step 1</span></span>

<span data-ttu-id="89256-190">Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="89256-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="89256-191">Alkalmazásátjáró indításakor szerzi be hello alhálózati konfigurált IP-címeit, és útvonal-hello háttér IP-készletet a hálózati forgalom toohello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="89256-191">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="89256-192">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="89256-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="89256-193">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-193">Step 2</span></span>

<span data-ttu-id="89256-194">Konfigurálja az előtér-IP-port hello hello nyilvános IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="89256-194">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="89256-195">Ez a port nem hello port, amelyet a végfelhasználók csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="89256-195">This port is hello port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="89256-196">3. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-196">Step 3</span></span>

<span data-ttu-id="89256-197">Nyilvános IP-végponton hello előtér-IP konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="89256-197">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="89256-198">4. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-198">Step 4</span></span>

<span data-ttu-id="89256-199">Hello Alkalmazásátjáró, használja a toodecrypt hello (SCEP) konfigurálhat, és hogy át kellene haladnia hello-forgalom újratitkosítása.</span><span class="sxs-lookup"><span data-stu-id="89256-199">Configure hello certificate for hello Application Gateway, used toodecrypt and re-encrypt hello traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="89256-200">5. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-200">Step 5</span></span>

<span data-ttu-id="89256-201">Hello HTTP-figyelő hello Alkalmazásátjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89256-201">Create hello HTTP listener for hello Application Gateway.</span></span> <span data-ttu-id="89256-202">Rendelje hozzá a hello előtér-IP-konfiguráció, a port és az ssl-tanúsítvány tooit.</span><span class="sxs-lookup"><span data-stu-id="89256-202">Assign hello front-end IP configuration, port, and ssl certificate tooit.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="89256-203">6. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-203">Step 6</span></span>

<span data-ttu-id="89256-204">Hozzon létre egy egyéni mintavételi toohello API-kezelés szolgáltatás `ContosoApi` proxy tartomány végpont.</span><span class="sxs-lookup"><span data-stu-id="89256-204">Create a custom probe toohello API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="89256-205">hello elérési `/status-0123456789abcdef` egy alapértelmezett állapotfigyelő végpont futó összes hello API Management szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="89256-205">hello path `/status-0123456789abcdef` is a default health endpoint hosted on all hello API Management services.</span></span> <span data-ttu-id="89256-206">Állítsa be `api.contoso.net` egy egyéni mintavételi állomásnév toosecure, az SSL-tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="89256-206">Set `api.contoso.net` as a custom probe hostname toosecure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="89256-207">állomásnév hello `contosoapi.azure-api.net` a rendszer hello alapértelmezett proxy állomásnév konfigurálja a szolgáltatás nevű `contosoapi` jön létre a nyilvános Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="89256-207">hello hostname `contosoapi.azure-api.net` is hello default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="89256-208">7. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-208">Step 7</span></span>

<span data-ttu-id="89256-209">Hello SSL Protokollt használó háttér címkészletet erőforrások használt toobe hello-tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="89256-209">Upload hello certificate toobe used on hello SSL-enabled backend pool resources.</span></span> <span data-ttu-id="89256-210">Ez a hello ugyanazt a tanúsítványt, amely a fenti 4 megadott.</span><span class="sxs-lookup"><span data-stu-id="89256-210">This is hello same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a><span data-ttu-id="89256-211">8. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-211">Step 8</span></span>

<span data-ttu-id="89256-212">HTTP háttér hello Alkalmazásátjáró beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="89256-212">Configure HTTP backend settings for hello Application Gateway.</span></span> <span data-ttu-id="89256-213">Ez magában foglalja a háttérrendszer kérelem megszakítása elteltével időtúllépési korlát.</span><span class="sxs-lookup"><span data-stu-id="89256-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="89256-214">Ez az érték eltér hello mintavételi időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="89256-214">This value is different from hello probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="89256-215">9. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-215">Step 9</span></span>

<span data-ttu-id="89256-216">Konfigurálja a háttér IP-címkészletet nevű **apimbackend** hello belső virtuális IP-címe hello API-kezelés szolgáltatás a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="89256-216">Configure a back-end IP address pool named **apimbackend**  with hello internal virtual IP address of hello API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="89256-217">10. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-217">Step 10</span></span>

<span data-ttu-id="89256-218">Hozzon létre egy üres (nem létező) háttér beállításait.</span><span class="sxs-lookup"><span data-stu-id="89256-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="89256-219">Nem szeretnénk az API Management alkalmazás átjárón keresztül tooexpose a rendszer elérte a háttér, majd térjen vissza a 404-es tooAPI útvonal kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="89256-219">Requests tooAPI paths that we do not want tooexpose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="89256-220">Hello dummy háttér HTTP beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="89256-220">Configure HTTP settings for hello dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="89256-221">Egy üres háttér konfigurálása **dummyBackendPool**, amely mutat tooa FQDN cím **dummybackend.com**. Az FQDN cím hello virtuális hálózat nem létezik.</span><span class="sxs-lookup"><span data-stu-id="89256-221">Configure a dummy backend **dummyBackendPool**, which points tooa FQDN address **dummybackend.com**. This FQDN address does not exist in hello virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="89256-222">Hozzon létre egy szabályt, hogy Alkalmazásátjáró toohello nem létező háttér mutat, amely alapértelmezés szerint használandó hello beállítása **dummybackend.com** hello virtuális hálózat található.</span><span class="sxs-lookup"><span data-stu-id="89256-222">Create a rule setting that hello Application Gateway will use by default which points toohello non-existent backend **dummybackend.com** in hello Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="89256-223">11. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-223">Step 11</span></span>

<span data-ttu-id="89256-224">URL-cím szabály útvonalak hello háttér-címkészletek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="89256-224">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="89256-225">Ez lehetővé teszi, hogy csak néhány hello API-kat API Management ahhoz, hogy meg toohello nyilvánosan elérhetővé tett kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="89256-225">This enables selecting only some of hello APIs from API Management for being exposed toohello public.</span></span> <span data-ttu-id="89256-226">Például akkor, ha nincsenek `Echo API` (/ echo /) `Calculator API` csak (/calc/) stb. Ellenőrizze `Echo API` internetről hozzáférhető).</span><span class="sxs-lookup"><span data-stu-id="89256-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="89256-227">hello alábbi példa létrehoz egy egyszerű szabályt hello "/ echo /" elérési út útválasztási forgalom toohello back-end "apimProxyBackendPool".</span><span class="sxs-lookup"><span data-stu-id="89256-227">hello following example creates a simple rule for hello "/echo/" path routing traffic toohello back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="89256-228">Ha hello elérési út nem felel meg a hello elérési szabályok azt szeretnénk, ha az API Management, térkép konfiguráció is konfigurálja egy alapértelmezett háttér címkészletet nevű hello szabály tooenable **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="89256-228">If hello path doesn't match hello path rules we want tooenable from API Management, hello rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="89256-229">Például http://api.contoso.net/calc/ * kerül túl**dummyBackendPool** , mint a nem egyező forgalom hello alapértelmezett alkalmazáskészlet van definiálva.</span><span class="sxs-lookup"><span data-stu-id="89256-229">For example, http://api.contoso.net/calc/* goes too**dummyBackendPool** as it is defined as hello default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="89256-230">hello fenti lépés biztosítja, amelyek csak hello elérési vonatkozó kérések "/ echo" hello Alkalmazásátjáró engedélyezettek legyenek.</span><span class="sxs-lookup"><span data-stu-id="89256-230">hello above step ensures that only requests for hello path "/echo" are allowed through hello Application Gateway.</span></span> <span data-ttu-id="89256-231">Az API Management API-k konfigurált kérelmek tooother 404-es hibák kivételhibát hello Internet történő alkalmazás-átjáróról.</span><span class="sxs-lookup"><span data-stu-id="89256-231">Requests tooother APIs configured in API Management will throw 404 errors from Application Gateway when accessed from hello Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="89256-232">12. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-232">Step 12</span></span>

<span data-ttu-id="89256-233">Hello Alkalmazásátjáró toouse URL-cím elérési út-alapú útválasztási szabály beállítás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89256-233">Create a rule setting for hello Application Gateway toouse URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="89256-234">13. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-234">Step 13</span></span>

<span data-ttu-id="89256-235">Példányok és az Application Gateway hello mérete hello számának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="89256-235">Configure hello number of instances and size for hello Application Gateway.</span></span> <span data-ttu-id="89256-236">Itt használjuk hello [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) hello API-kezelés erőforráshoz a biztonság fokozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="89256-236">Here we are using hello [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of hello API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="89256-237">14. lépés</span><span class="sxs-lookup"><span data-stu-id="89256-237">Step 14</span></span>

<span data-ttu-id="89256-238">WAF toobe konfigurálása "Megakadályozása" módban.</span><span class="sxs-lookup"><span data-stu-id="89256-238">Configure WAF toobe in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="89256-239">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="89256-239">Create Application Gateway</span></span>

<span data-ttu-id="89256-240">Hozzon létre egy alkalmazás összes hello konfigurációs objektumot az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="89256-240">Create an Application Gateway with all hello configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a><span data-ttu-id="89256-241">CNAME hello API Management proxy állomásnév toohello nyilvános DNS-neve hello Alkalmazásátjáró erőforrás</span><span class="sxs-lookup"><span data-stu-id="89256-241">CNAME hello API Management proxy hostname toohello public DNS name of hello Application Gateway resource</span></span>

<span data-ttu-id="89256-242">Hello átjáró létrehozása után hello következő lépésre tooconfigure hello előtér-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="89256-242">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="89256-243">Egy nyilvános IP-cím használata esetén az Application Gateway egy dinamikusan hozzárendelt DNS-nevet, amely nem lehet könnyen toouse van szükség.</span><span class="sxs-lookup"><span data-stu-id="89256-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy toouse.</span></span> 

<span data-ttu-id="89256-244">hello Application Gateway DNS-nevét kell használt toocreate egy olyan CNAME rekordot, amely hello APIM proxyállomásnév mutat (pl. `api.contoso.net` a fenti példákban hello) toothis DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="89256-244">hello Application Gateway's DNS name should be used toocreate a CNAME record which points hello APIM proxy host name (e.g. `api.contoso.net` in hello examples above) toothis DNS name.</span></span> <span data-ttu-id="89256-245">tooconfigure hello előtérbeli IP-CNAME rekordot, hello Alkalmazásátjáró hello részleteit és a társított IP-/ DNS-nevét, hello PublicIPAddress elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="89256-245">tooconfigure hello frontend IP CNAME record, retrieve hello details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element.</span></span> <span data-ttu-id="89256-246">A-rekordok hello használata nem ajánlott, óta hello VIP előfordulhat, hogy az átjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="89256-246">hello use of A-records is not recommended since hello VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="89256-247"><a name="summary"></a> Összegzése</span><span class="sxs-lookup"><span data-stu-id="89256-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="89256-248">Konfigurált egy VNETET az Azure API Management átjáró egyetlen felületet biztosít minden konfigurált API, legyenek azok helyszíni üzemeltetett vagy hello felhő.</span><span class="sxs-lookup"><span data-stu-id="89256-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in hello cloud.</span></span> <span data-ttu-id="89256-249">Alkalmazásátjáró integrálása az API Management rugalmasan hello szelektív engedélyezése az adott API-k toobe hello Internet elérhető, valamint a webalkalmazási tűzfal előtér tooyour API Management példányt biztosít.</span><span class="sxs-lookup"><span data-stu-id="89256-249">Integrating Application Gateway with API Management provides hello flexibility of selectively enabling particular APIs toobe accessible on hello Internet, as well as providing a Web Application Firewall as a frontend tooyour API Management instance.</span></span>

##<span data-ttu-id="89256-250"><a name="next-steps"></a> További lépések</span><span class="sxs-lookup"><span data-stu-id="89256-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="89256-251">További tudnivalók az Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="89256-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="89256-252">Átjáró – áttekintés</span><span class="sxs-lookup"><span data-stu-id="89256-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="89256-253">Alkalmazás átjáró webalkalmazási tűzfal</span><span class="sxs-lookup"><span data-stu-id="89256-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="89256-254">Az Alkalmazásátjáró elérési alapú útválasztás használatával</span><span class="sxs-lookup"><span data-stu-id="89256-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="89256-255">Ismerje meg a további információk az API Management és Vnetek</span><span class="sxs-lookup"><span data-stu-id="89256-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="89256-256">Rendelkezésre álló hello virtuális hálózaton belül csak az API Management használata</span><span class="sxs-lookup"><span data-stu-id="89256-256">Using API Management available only within hello VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="89256-257">Az API Management használatát a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="89256-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)
