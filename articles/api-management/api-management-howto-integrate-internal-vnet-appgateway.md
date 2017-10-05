---
title: "Azure API Management használata a virtuális hálózatban az Alkalmazásátjáró |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse és konfigurálja a belső virtuális hálózat az alkalmazás átjáró (waf-ot), amelynek az Azure API Management"
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
ms.openlocfilehash: 8131ded6b74e9c544bf70b1a4659ed07e5def04d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="0f735-103">Egy belső virtuális Hálózatot az API Management integrálása Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="0f735-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="0f735-104"><a name="overview"></a> – Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0f735-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="0f735-105">Az API Management szolgáltatás konfigurálható egy virtuális hálózatot belső módban, így azokat csak a virtuális hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0f735-105">The API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within the Virtual Network.</span></span> <span data-ttu-id="0f735-106">Az Azure Application Gateway egy olyan PAAS szolgáltatás, amely réteg-7 terheléselosztót biztosít.</span><span class="sxs-lookup"><span data-stu-id="0f735-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="0f735-107">Fordított proxy szolgáltatásként működik, és biztosít ajánlat egy webes alkalmazás tűzfalat (WAF) között.</span><span class="sxs-lookup"><span data-stu-id="0f735-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="0f735-108">Egy belső virtuális Hálózatot és az Application Gateway előtér kiépítve API Management kombinálásával lehetővé teszi, hogy a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="0f735-108">Combining API Management provisioned in an internal VNET with the Application Gateway frontend enables the following scenarios:</span></span>

* <span data-ttu-id="0f735-109">Használja az ugyanazon API-kezelés erőforráshoz a belső fogyasztók és a külső felhasználók által megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="0f735-109">Use the same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="0f735-110">Egyetlen API Management erőforrást használ, és rendelkezik a külső felhasználók API Management érhető el a megadott API-t.</span><span class="sxs-lookup"><span data-stu-id="0f735-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="0f735-111">Kulcsrakész hardvermódosításainak váltani hozzáférés az API Management a nyilvános internetről be- és kikapcsolható.</span><span class="sxs-lookup"><span data-stu-id="0f735-111">Provide a turn-key way to switch access to API Management from the public Internet on and off.</span></span> 

##<span data-ttu-id="0f735-112"><a name="scenario"></a> Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0f735-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="0f735-113">Ez a cikk a külső és belső fogyasztók egyetlen API-kezelés szolgáltatást használ, és lehetővé teszi működjön, és egyetlen időtúllépést a két helyszíni és felhőalapú API-k fog foglalkozunk.</span><span class="sxs-lookup"><span data-stu-id="0f735-113">This article will cover how to use a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="0f735-114">Hogyan teszi közzé az API-k (a példa zöld kijelölt) csak egy részét a PathBasedRouting funkció érhető el az alkalmazás átjáró használatával külső felhasználásra is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="0f735-114">You will also see how to expose only a subset of your APIs (in the example they are highlighted in green) for External Consumption using the PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="0f735-115">Az első beállítás minden API felügyelt csak virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="0f735-115">In the first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="0f735-116">Belső fogyasztóknak (a kijelölt narancs) férhetnek hozzá a belső és külső API.</span><span class="sxs-lookup"><span data-stu-id="0f735-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="0f735-117">Forgalom soha nem kerül ki egy nagy teljesítményű kerülnek az interneten keresztül Expressroute-Kapcsolatcsoportok.</span><span class="sxs-lookup"><span data-stu-id="0f735-117">Traffic never goes out to Internet a high performance is delivered via Express Route circuits.</span></span>

![URL-cím útvonal](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="0f735-119"><a name="before-you-begin"></a> Megkezdése előtt</span><span class="sxs-lookup"><span data-stu-id="0f735-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="0f735-120">Telepítse az Azure PowerShell-parancsmagok legújabb verzióját a Webplatform-telepítővel.</span><span class="sxs-lookup"><span data-stu-id="0f735-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="0f735-121">A [Letöltések lap](https://azure.microsoft.com/downloads/) **Windows PowerShell** szakaszából letöltheti és telepítheti a legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="0f735-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="0f735-122">Hozzon létre egy virtuális hálózatot, és hozzon létre különálló alhálózatokat az API Management és az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="0f735-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="0f735-123">Ha azt tervezi, hogy hozzon létre egy egyéni DNS-kiszolgáló a virtuális hálózati, ehhez a központi telepítés elindítása előtt.</span><span class="sxs-lookup"><span data-stu-id="0f735-123">If you intend to create a custom DNS server for the Virtual Network, do so before starting the deployment.</span></span> <span data-ttu-id="0f735-124">Ellenőrizze az új alhálózat virtuális hálózatban létrehozott virtuális gépek biztosításával működik megoldhatja és Azure szolgáltatásvégpontokra eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0f735-124">Double check it works by ensuring a virtual machine created in a new subnet in the Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="0f735-125">Mi az API Management és az Application Gateway közötti integrációs létrehozásához szükséges?</span><span class="sxs-lookup"><span data-stu-id="0f735-125">What is required to create an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="0f735-126">**Háttér-kiszolgálófiók készlet:** Ez az a belső virtuális IP-címet a API-kezelés szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0f735-126">**Back-end server pool:** This is the internal virtual IP address of the API Management service.</span></span>
* <span data-ttu-id="0f735-127">**Háttér-kiszolgálókészlet beállításai:** Minden készletnek vannak beállításai, például port, protokoll vagy cookie-alapú affinitás.</span><span class="sxs-lookup"><span data-stu-id="0f735-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="0f735-128">Ezek a beállítások a készletben lévő összes kiszolgálót is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="0f735-128">These settings are applied to all servers within the pool.</span></span>
* <span data-ttu-id="0f735-129">**Előtér-port:** nyilvános portja, amely meg van nyitva, az alkalmazás-átjárón.</span><span class="sxs-lookup"><span data-stu-id="0f735-129">**Front-end port:** This is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="0f735-130">Elérte-e azt forgalom lekérdezi átirányítva a háttér-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="0f735-130">Traffic hitting it gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="0f735-131">**Figyelő:** A figyelő egy előtérbeli porttal, egy protokollal (Http vagy Https, a kis- és a nagybetűk megkülönböztetésével) és SSL tanúsítványnévvel rendelkezik (SSL-kiszervezés konfigurálásakor).</span><span class="sxs-lookup"><span data-stu-id="0f735-131">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="0f735-132">**Szabály:** a szabály a figyelőt kötve van egy háttér-kiszolgálófiók készlet.</span><span class="sxs-lookup"><span data-stu-id="0f735-132">**Rule:** The rule binds a listener to a back-end server pool.</span></span>
* <span data-ttu-id="0f735-133">**Egyéni Állapotmintáihoz:** Alkalmazásátjáró, alapértelmezés szerint a megadott IP-cím alapú mintavételt mérje fel, hogy mely kiszolgálók a BackendAddressPool aktívak.</span><span class="sxs-lookup"><span data-stu-id="0f735-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes to figure out which servers in the BackendAddressPool are active.</span></span> <span data-ttu-id="0f735-134">Az API Management szolgáltatást csak válaszol a kérelmekre, amelyek rendelkeznek a megfelelő állomásfejléc, ezért az alapértelmezett mintavételt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0f735-134">The API Management service only responds to requests which have the correct host header, hence the default probes fail.</span></span> <span data-ttu-id="0f735-135">Egy egyéni állapotmintáihoz kell segítségével határozza meg, hogy a szolgáltatás nem aktív, és továbbítsa a kérelmek Alkalmazásátjáró definiálni.</span><span class="sxs-lookup"><span data-stu-id="0f735-135">A custom health probe needs to be defined to help application gateway determine that the service is alive and it should forward requests.</span></span>
* <span data-ttu-id="0f735-136">**Az egyéni tartomány tanúsítvány:** API Management kell létrehoznia egy CNAME-leképezés az állomásnév az Alkalmazásátjáró előtér-DNS-nevével, az internet eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0f735-136">**Custom domain certificate:** To access API Management from the internet you need to create a CNAME mapping of its hostname to the Application Gateway front-end DNS name.</span></span> <span data-ttu-id="0f735-137">Ez biztosítja, hogy az állomásnév fejléc és a tanúsítvány, amely az API Management továbbíthatja a rendszer az Alkalmazásátjáró küldött egy APIM fel tudja ismerni, mint érvényes.</span><span class="sxs-lookup"><span data-stu-id="0f735-137">This ensures that the hostname header and certificate sent to Application Gateway that is forwarded to API Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="0f735-138"><a name="overview-steps"></a> API Management és az Application Gateway integrálásához szükséges lépések</span><span class="sxs-lookup"><span data-stu-id="0f735-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="0f735-139">Egy erőforráscsoport létrehozása a Resource Manager számára.</span><span class="sxs-lookup"><span data-stu-id="0f735-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="0f735-140">Hozzon létre egy virtuális hálózati alhálózat és nyilvános IP-cím az az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="0f735-140">Create a Virtual Network, subnet, and public IP for the Application Gateway.</span></span> <span data-ttu-id="0f735-141">Hozzon létre egy másik alhálózatot az API Management.</span><span class="sxs-lookup"><span data-stu-id="0f735-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="0f735-142">Az előbb létrehozott virtuális hálózat alhálózaton belüli API Management szolgáltatás létrehozása, és győződjön meg arról, a belső módot használja.</span><span class="sxs-lookup"><span data-stu-id="0f735-142">Create an API Management service inside the VNET subnet created above and ensure you use the Internal mode.</span></span>
4. <span data-ttu-id="0f735-143">Az API Management szolgáltatásban az egyéni tartománynév beállítása.</span><span class="sxs-lookup"><span data-stu-id="0f735-143">Setup the custom domain name in the API Management service.</span></span>
5. <span data-ttu-id="0f735-144">Az Application Gateway konfigurációs objektum létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0f735-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="0f735-145">Alkalmazásátjáró erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f735-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="0f735-146">Hozzon létre egy CNAME REKORDOT a nyilvános DNS-neve az alkalmazás-átjáró az API Management proxy állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="0f735-146">Create a CNAME from the public DNS name of the Application Gateway to the API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="0f735-147">Erőforráscsoport létrehozása a Resource Managerhez</span><span class="sxs-lookup"><span data-stu-id="0f735-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="0f735-148">Ügyeljen arra, hogy az Azure PowerShell legújabb verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="0f735-148">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="0f735-149">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0f735-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="0f735-150">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-150">Step 1</span></span>

<span data-ttu-id="0f735-151">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="0f735-151">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="0f735-152">Azokkal a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0f735-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="0f735-153">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-153">Step 2</span></span>

<span data-ttu-id="0f735-154">Ellenőrizze a fiókhoz tartozó előfizetések, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="0f735-154">Check the subscriptions for the account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="0f735-155">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-155">Step 3</span></span>

<span data-ttu-id="0f735-156">Hozzon létre egy erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ).</span><span class="sxs-lookup"><span data-stu-id="0f735-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="0f735-157">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="0f735-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="0f735-158">Ez szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként.</span><span class="sxs-lookup"><span data-stu-id="0f735-158">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="0f735-159">Győződjön meg arról, hogy minden parancs Alkalmazásátjáró létrehozása ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="0f735-159">Make sure that all commands to create an application gateway use the same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="0f735-160">Hozzon létre egy virtuális hálózatot és az Alkalmazásátjáró alhálózatot</span><span class="sxs-lookup"><span data-stu-id="0f735-160">Create a Virtual Network and a subnet for the application gateway</span></span>

<span data-ttu-id="0f735-161">A következő példa bemutatja, hogyan manager használata a virtuális hálózat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0f735-161">The following example shows how to create a Virtual Network using the resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="0f735-162">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-162">Step 1</span></span>

<span data-ttu-id="0f735-163">A cím tartomány 10.0.0.0/24 hozzárendelése a virtuális hálózat létrehozása során az Alkalmazásátjáró használt alhálózati változó.</span><span class="sxs-lookup"><span data-stu-id="0f735-163">Assign the address range 10.0.0.0/24 to the subnet variable to be used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="0f735-164">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-164">Step 2</span></span>

<span data-ttu-id="0f735-165">A cím tartomány 10.0.1.0/24 hozzárendelése a virtuális hálózat létrehozása során használt az API Management alhálózati változó.</span><span class="sxs-lookup"><span data-stu-id="0f735-165">Assign the address range 10.0.1.0/24 to the subnet variable to be used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="0f735-166">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-166">Step 3</span></span>

<span data-ttu-id="0f735-167">Hozzon létre egy virtuális hálózatot nevű **appgwvnet** erőforráscsoportban **apim-appGw-RG** az USA nyugati régiója régióba a előtag 10.0.0.0/16 tartozó alhálózatok 10.0.0.0/24 és 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="0f735-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for the West US region using the prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="0f735-168">4. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-168">Step 4</span></span>

<span data-ttu-id="0f735-169">Rendelje hozzá a következő lépéseket alhálózati változó</span><span class="sxs-lookup"><span data-stu-id="0f735-169">Assign a subnet variable for the next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="0f735-170">Egy belső módban konfigurált virtuális hálózaton belül az API Management szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f735-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="0f735-171">A következő példa bemutatja, hogyan API Management szolgáltatás létrehozása csak a belső hozzáférés beállítása a VNETEN belül.</span><span class="sxs-lookup"><span data-stu-id="0f735-171">The following example shows how to create an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="0f735-172">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-172">Step 1</span></span>
<span data-ttu-id="0f735-173">A fentiekben létrehozott $apimsubnetdata alhálózati használata API felügyeleti virtuális hálózati objektumot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0f735-173">Create an API Management Virtual Network object using the subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="0f735-174">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-174">Step 2</span></span>
<span data-ttu-id="0f735-175">A virtuális hálózaton belül az API Management szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f735-175">Create an API Management service inside the Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="0f735-176">A fenti parancs sikeres után tekintse meg a [belső hálózatok API-kezelés szolgáltatás eléréséhez szükséges DNS-konfiguráció](api-management-using-with-internal-vnet.md#apim-dns-configuration) való hozzáférésre.</span><span class="sxs-lookup"><span data-stu-id="0f735-176">After the above command succeeds refer to [DNS Configuration required to access internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) to access it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="0f735-177">Telepítés egy egyéni tartománynevet, az API Management</span><span class="sxs-lookup"><span data-stu-id="0f735-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="0f735-178">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-178">Step 1</span></span>
<span data-ttu-id="0f735-179">A tanúsítvány feltöltése a tartományhoz tartozó titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="0f735-179">Upload the certificate with private key for the domain.</span></span> <span data-ttu-id="0f735-180">Ehhez a példához lesz `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="0f735-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="0f735-181">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-181">Step 2</span></span>
<span data-ttu-id="0f735-182">A tanúsítvány a feltöltést követően állomásnév konfigurációs objektum létrehozása egy állomásnevét a proxy `api.contoso.net`, mert a példa tanúsítvány lehetővé teszi a szolgáltató a `*.contoso.net` tartomány.</span><span class="sxs-lookup"><span data-stu-id="0f735-182">Once the certificate is uploaded, create a hostname configuration object for the proxy with a hostname of `api.contoso.net`, as the example certificate provides authority for the  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="0f735-183">Nyilvános IP-cím létrehozása az előtérbeli konfigurációhoz</span><span class="sxs-lookup"><span data-stu-id="0f735-183">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="0f735-184">Hozzon létre egy nyilvános IP-erőforrás **publicIP01** erőforráscsoportban **apim-appGw-RG** az USA nyugati régiójában.</span><span class="sxs-lookup"><span data-stu-id="0f735-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="0f735-185">Amikor a szolgáltatás elindul, egy IP-cím lesz kiosztva az Application Gatewaynek.</span><span class="sxs-lookup"><span data-stu-id="0f735-185">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="0f735-186">Alkalmazás átjáró-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f735-186">Create application gateway configuration</span></span>

<span data-ttu-id="0f735-187">Az Application Gateway létrehozása előtt minden konfigurációs elemet be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="0f735-187">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="0f735-188">Az alábbi lépések létrehozzák az Application Gateway erőforráshoz szükséges konfigurációs elemeket.</span><span class="sxs-lookup"><span data-stu-id="0f735-188">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="0f735-189">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-189">Step 1</span></span>

<span data-ttu-id="0f735-190">Hozzon létre egy **gatewayIP01** nevű Application Gateway IP-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0f735-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="0f735-191">Amikor az Application Gateway elindul, a konfigurált alhálózatból felvesz egy IP-címet, és a hálózati forgalmat a háttérbeli IP-készlet IP-címeihez irányítja.</span><span class="sxs-lookup"><span data-stu-id="0f735-191">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="0f735-192">Ne feledje, hogy minden példány egy IP-címet vesz fel.</span><span class="sxs-lookup"><span data-stu-id="0f735-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="0f735-193">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-193">Step 2</span></span>

<span data-ttu-id="0f735-194">Konfigurálja az előtér-IP-portját a nyilvános IP-végponton.</span><span class="sxs-lookup"><span data-stu-id="0f735-194">Configure the front-end IP port for the public IP endpoint.</span></span> <span data-ttu-id="0f735-195">Ez a port nem a portot, amelyet a végfelhasználók csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="0f735-195">This port is the port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="0f735-196">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-196">Step 3</span></span>

<span data-ttu-id="0f735-197">Konfigurálja az előtérbeli IP-portot egy nyilvános IP-címvégponttal.</span><span class="sxs-lookup"><span data-stu-id="0f735-197">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="0f735-198">4. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-198">Step 4</span></span>

<span data-ttu-id="0f735-199">A tanúsítvány konfigurálása az Alkalmazásátjáró, hogy át kellene haladnia-forgalom újratitkosítása és visszafejtésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="0f735-199">Configure the certificate for the Application Gateway, used to decrypt and re-encrypt the traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="0f735-200">5. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-200">Step 5</span></span>

<span data-ttu-id="0f735-201">Az alkalmazás átjáró a HTTP-figyelő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f735-201">Create the HTTP listener for the Application Gateway.</span></span> <span data-ttu-id="0f735-202">Az előtér-IP konfigurációjával, a port és az ssl-tanúsítványt hozzárendeli azt.</span><span class="sxs-lookup"><span data-stu-id="0f735-202">Assign the front-end IP configuration, port, and ssl certificate to it.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="0f735-203">6. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-203">Step 6</span></span>

<span data-ttu-id="0f735-204">Hozzon létre egy egyéni mintavétel az API Management szolgáltatáshoz `ContosoApi` proxy tartomány végpont.</span><span class="sxs-lookup"><span data-stu-id="0f735-204">Create a custom probe to the API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="0f735-205">Az elérési út `/status-0123456789abcdef` egy alapértelmezett állapotfigyelő végpont az API Management-szolgáltatások üzemeltet.</span><span class="sxs-lookup"><span data-stu-id="0f735-205">The path `/status-0123456789abcdef` is a default health endpoint hosted on all the API Management services.</span></span> <span data-ttu-id="0f735-206">Állítsa be `api.contoso.net` mint egy SSL-tanúsítvánnyal biztosításához egyéni mintavételi állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="0f735-206">Set `api.contoso.net` as a custom probe hostname to secure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="0f735-207">A hostname `contosoapi.azure-api.net` konfigurálását, ha egy nevű szolgáltatás alapértelmezett proxy állomásneve `contosoapi` jön létre a nyilvános Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0f735-207">The hostname `contosoapi.azure-api.net` is the default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="0f735-208">7. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-208">Step 7</span></span>

<span data-ttu-id="0f735-209">Töltse fel az SSL-kompatibilis háttér készlet erőforrások használni kívánt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="0f735-209">Upload the certificate to be used on the SSL-enabled backend pool resources.</span></span> <span data-ttu-id="0f735-210">Ez az a fenti 4 megadott ugyanazt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="0f735-210">This is the same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a><span data-ttu-id="0f735-211">8. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-211">Step 8</span></span>

<span data-ttu-id="0f735-212">Az Alkalmazásátjáró HTTP háttér beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0f735-212">Configure HTTP backend settings for the Application Gateway.</span></span> <span data-ttu-id="0f735-213">Ez magában foglalja a háttérrendszer kérelem megszakítása elteltével időtúllépési korlát.</span><span class="sxs-lookup"><span data-stu-id="0f735-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="0f735-214">Ez az érték eltér a mintavételi időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="0f735-214">This value is different from the probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="0f735-215">9. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-215">Step 9</span></span>

<span data-ttu-id="0f735-216">Konfigurálja a háttér IP-címkészletet nevű **apimbackend** a belső virtuális IP-címet a API-kezelés szolgáltatás a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0f735-216">Configure a back-end IP address pool named **apimbackend**  with the internal virtual IP address of the API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="0f735-217">10. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-217">Step 10</span></span>

<span data-ttu-id="0f735-218">Hozzon létre egy üres (nem létező) háttér beállításait.</span><span class="sxs-lookup"><span data-stu-id="0f735-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="0f735-219">Kérelmek és API-elérési utakat, azt nem teszi közzé az API Management alkalmazás átjárón keresztül kíván elérte a háttér, és a 404 adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0f735-219">Requests to API paths that we do not want to expose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="0f735-220">Az üres háttér HTTP beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0f735-220">Configure HTTP settings for the dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="0f735-221">Egy üres háttér konfigurálása **dummyBackendPool**, amely a megadott FQDN cím mutat **dummybackend.com**. Az FQDN cím nem létezik a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0f735-221">Configure a dummy backend **dummyBackendPool**, which points to a FQDN address **dummybackend.com**. This FQDN address does not exist in the virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="0f735-222">Létrehoz egy szabály beállítást, amely az Alkalmazásátjáró fogja használni a nem létező háttér mutat, amely alapértelmezés szerint **dummybackend.com** virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="0f735-222">Create a rule setting that the Application Gateway will use by default which points to the non-existent backend **dummybackend.com** in the Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="0f735-223">11. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-223">Step 11</span></span>

<span data-ttu-id="0f735-224">A háttér-készletek az URL-cím szabály elérési útvonalainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0f735-224">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="0f735-225">Ez lehetővé teszi, hogy kijelölése az API-k közül csak az API Management nyilvánosan elérhetővé váljon.</span><span class="sxs-lookup"><span data-stu-id="0f735-225">This enables selecting only some of the APIs from API Management for being exposed to the public.</span></span> <span data-ttu-id="0f735-226">Például akkor, ha nincsenek `Echo API` (/ echo /) `Calculator API` csak (/calc/) stb. Ellenőrizze `Echo API` internetről hozzáférhető).</span><span class="sxs-lookup"><span data-stu-id="0f735-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="0f735-227">Az alábbi példa létrehoz egy egyszerű szabályt a "/ echo /" elérési út útválasztási adatforgalmát a háttér-"apimProxyBackendPool".</span><span class="sxs-lookup"><span data-stu-id="0f735-227">The following example creates a simple rule for the "/echo/" path routing traffic to the back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="0f735-228">Ha az elérési út nem egyezik meg szeretnénk az API-kezelés engedélyezése az elérésiút-szabály, a szabály térkép konfiguráció is konfigurálja egy alapértelmezett háttér címkészletet nevű **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="0f735-228">If the path doesn't match the path rules we want to enable from API Management, the rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="0f735-229">Például http://api.contoso.net/calc/ * ugrik **dummyBackendPool** , mint az alapértelmezett alkalmazáskészlet nem egyező forgalom van definiálva.</span><span class="sxs-lookup"><span data-stu-id="0f735-229">For example, http://api.contoso.net/calc/* goes to **dummyBackendPool** as it is defined as the default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="0f735-230">A fenti lépést biztosítja, hogy csak az elérési út vonatkozó kérések "/ echo" az alkalmazás átjárón keresztül engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="0f735-230">The above step ensures that only requests for the path "/echo" are allowed through the Application Gateway.</span></span> <span data-ttu-id="0f735-231">Más konfigurálva az API Management API-k kérelmek kivételhibát 404-es hibák az Alkalmazásátjáró, ha az internetről érik el.</span><span class="sxs-lookup"><span data-stu-id="0f735-231">Requests to other APIs configured in API Management will throw 404 errors from Application Gateway when accessed from the Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="0f735-232">12. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-232">Step 12</span></span>

<span data-ttu-id="0f735-233">Az Alkalmazásátjáró használandó URL-cím elérési út-alapú útválasztási szabály beállítás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f735-233">Create a rule setting for the Application Gateway to use URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="0f735-234">13. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-234">Step 13</span></span>

<span data-ttu-id="0f735-235">Adja meg a példányok és mérete a(z).</span><span class="sxs-lookup"><span data-stu-id="0f735-235">Configure the number of instances and size for the Application Gateway.</span></span> <span data-ttu-id="0f735-236">Itt a [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) az API-kezelés erőforráshoz a biztonság fokozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="0f735-236">Here we are using the [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of the API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="0f735-237">14. lépés</span><span class="sxs-lookup"><span data-stu-id="0f735-237">Step 14</span></span>

<span data-ttu-id="0f735-238">WAF "Megakadályozása" módban kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="0f735-238">Configure WAF to be in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="0f735-239">Alkalmazásátjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f735-239">Create Application Gateway</span></span>

<span data-ttu-id="0f735-240">Hozzon létre egy alkalmazást az előző lépéseket az összes konfigurációs objektumok.</span><span class="sxs-lookup"><span data-stu-id="0f735-240">Create an Application Gateway with all the configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a><span data-ttu-id="0f735-241">Az API Management proxy állomásnevet a nyilvános DNS-nevével, az Application Gateway erőforrás CNAME</span><span class="sxs-lookup"><span data-stu-id="0f735-241">CNAME the API Management proxy hostname to the public DNS name of the Application Gateway resource</span></span>

<span data-ttu-id="0f735-242">Az átjáró létrehozása után a következő lépés a kommunikációra szolgáló előtér konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0f735-242">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="0f735-243">Egy nyilvános IP-cím használata esetén az Application Gateway szükséges egy dinamikusan hozzárendelt DNS-nevet, amely nem lehet könnyen használható.</span><span class="sxs-lookup"><span data-stu-id="0f735-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy to use.</span></span> 

<span data-ttu-id="0f735-244">Az Alkalmazásátjáró DNS-nevét kell hozhatók létre egy CNAME rekordot, amely a APIM proxyállomásnév mutat (pl. `api.contoso.net` a fenti példákban) a DNS-névhez.</span><span class="sxs-lookup"><span data-stu-id="0f735-244">The Application Gateway's DNS name should be used to create a CNAME record which points the APIM proxy host name (e.g. `api.contoso.net` in the examples above) to this DNS name.</span></span> <span data-ttu-id="0f735-245">Az előtérbeli IP-CNAME rekord konfigurálásához részleteinek lehívására szolgáló az Alkalmazásátjáró és a társított IP-/ DNS-neve, a PublicIPAddress elem használatával.</span><span class="sxs-lookup"><span data-stu-id="0f735-245">To configure the frontend IP CNAME record, retrieve the details of the Application Gateway and its associated IP/DNS name using the PublicIPAddress element.</span></span> <span data-ttu-id="0f735-246">A-rekordok használata nem ajánlott, óta, a VIP előfordulhat, hogy az átjáró újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="0f735-246">The use of A-records is not recommended since the VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="0f735-247"><a name="summary"></a> Összegzése</span><span class="sxs-lookup"><span data-stu-id="0f735-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="0f735-248">Konfigurált egy VNETET az Azure API Management átjáró egyetlen felületet biztosít minden konfigurált API, legyenek azok helyszíni központi vagy a felhőben.</span><span class="sxs-lookup"><span data-stu-id="0f735-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in the cloud.</span></span> <span data-ttu-id="0f735-249">Alkalmazásátjáró integrálása az API Management szelektív módon engedélyezi az adott API-k az interneten érhető el, valamint a webalkalmazási tűzfal biztosít egy háttértűzfal a API Management-példányra, a rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="0f735-249">Integrating Application Gateway with API Management provides the flexibility of selectively enabling particular APIs to be accessible on the Internet, as well as providing a Web Application Firewall as a frontend to your API Management instance.</span></span>

##<span data-ttu-id="0f735-250"><a name="next-steps"></a> További lépések</span><span class="sxs-lookup"><span data-stu-id="0f735-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="0f735-251">További tudnivalók az Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="0f735-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="0f735-252">Átjáró – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0f735-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="0f735-253">Alkalmazás átjáró webalkalmazási tűzfal</span><span class="sxs-lookup"><span data-stu-id="0f735-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="0f735-254">Az Alkalmazásátjáró elérési alapú útválasztás használatával</span><span class="sxs-lookup"><span data-stu-id="0f735-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="0f735-255">Ismerje meg a további információk az API Management és Vnetek</span><span class="sxs-lookup"><span data-stu-id="0f735-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="0f735-256">Elérhető a VNETEN belül csak az API Management használata</span><span class="sxs-lookup"><span data-stu-id="0f735-256">Using API Management available only within the VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="0f735-257">Az API Management használatát a virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="0f735-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)
