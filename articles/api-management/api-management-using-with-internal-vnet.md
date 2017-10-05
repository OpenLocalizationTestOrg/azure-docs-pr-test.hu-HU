---
title: "Belső virtuális hálózat Azure API Management használata |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítése és konfigurálása az Azure API Management belső virtuális hálózat."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="74cc3-103">A belső virtuális hálózattal Azure API Management szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="74cc3-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="74cc3-104">Azure virtuális hálózatokról (Vnetekről) az API Management kezelhet API-k nem érhető el, az interneten.</span><span class="sxs-lookup"><span data-stu-id="74cc3-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on the Internet.</span></span> <span data-ttu-id="74cc3-105">VPN technológiáin számos érhetők el a kapcsolatot, és az API Management állítható rendszerbe a virtuális hálózaton belül két fő módban:</span><span class="sxs-lookup"><span data-stu-id="74cc3-105">A number of VPN technologies are available to make the connection and API Management can be deployed in two main modes inside the VNET:</span></span>
* <span data-ttu-id="74cc3-106">Külső</span><span class="sxs-lookup"><span data-stu-id="74cc3-106">External</span></span>
* <span data-ttu-id="74cc3-107">Belső</span><span class="sxs-lookup"><span data-stu-id="74cc3-107">Internal</span></span>

## <span data-ttu-id="74cc3-108"><a name="overview"> </a>Áttekintés</span><span class="sxs-lookup"><span data-stu-id="74cc3-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="74cc3-109">Az API Management telepítésekor egy belső virtuális hálózat módban minden szolgáltatásvégpontokra (átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet és Git) csak egy virtuális hálózatban, amelyek látható a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="74cc3-109">When API Management is deployed in an Internal Virtual network mode, all the service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="74cc3-110">A végpontok egyike a nyilvános DNS-kiszolgálón van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="74cc3-110">None of the service endpoints are registered on the Public DNS Server.</span></span>

<span data-ttu-id="74cc3-111">Az API Management belső módban, érhető el az alábbi esetekben</span><span class="sxs-lookup"><span data-stu-id="74cc3-111">Using API Management in Internal mode, you can achieve the following scenarios</span></span>
* <span data-ttu-id="74cc3-112">3. fél kívül, pont-pont vagy ExpressRoute VPN-kapcsolatok használatával érhető el a privát adatközpontban-környezetben üzemeltetett API biztonságosan ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="74cc3-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="74cc3-113">Engedélyezze a hibrid felhős rendszerekben jelentkezik, mintha a API-k felhőalapú és helyszíni API-k közös átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="74cc3-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="74cc3-114">Az átjáró egyetlen végpont használatával több földrajzi helyeken található üzemeltetett API kezelése.</span><span class="sxs-lookup"><span data-stu-id="74cc3-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="74cc3-115"><a name="enable-vpn"></a>Az API Management belső virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="74cc3-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="74cc3-116">Az API Management szolgáltatás belső virtuális hálózatban egy belső terheléselosztási Balancer(ILB) mögött helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="74cc3-116">The API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="74cc3-117">A ILB IP-címe van a [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) tartományon.</span><span class="sxs-lookup"><span data-stu-id="74cc3-117">The IP Address of the ILB is in the [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="74cc3-118">Azure-portál használatával VNET-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="74cc3-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="74cc3-119">Először létre kell hoznia az API-kezelés szolgáltatás a lépéseket követve [létrehozása API-kezelés szolgáltatás][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="74cc3-119">First create the API Management service by following the steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="74cc3-120">Majd beállításról API-kezelés szolgáltatás telepíthető egy virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="74cc3-120">Then set-up API Management to be deployed inside a Virtual network.</span></span>

![Belső virtuális hálózat APIM beállításának menü][api-management-using-internal-vnet-menu]

<span data-ttu-id="74cc3-122">Miután a telepítés sikeres, a szolgáltatás belső virtuális IP-címe megjelenik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="74cc3-122">After the deployment succeeds, you should see the Internal Virtual IP Address of your service on the dashboard.</span></span>

![Belső virtuális hálózaton konfigurált API-kezelési irányítópult][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="74cc3-124">Powershell-parancsmagok használatával VNET-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="74cc3-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="74cc3-125">VNET-kapcsolatot a PowerShell-parancsmagok használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="74cc3-125">You can also enable VNET connectivity using the PowerShell cmdlets.</span></span>

* <span data-ttu-id="74cc3-126">**A VNETEN belül az API Management szolgáltatás létrehozása**: parancsmag [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) hozzon létre egy Azure API Management szolgáltatást egy VNETEN belül, és konfigurálja úgy, hogy a belső hálózatok típusával.</span><span class="sxs-lookup"><span data-stu-id="74cc3-126">**Create an API Management service inside a VNET**: Use the cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) to create an Azure API Management service inside a VNET and configure it to use the Internal VNET type.</span></span>

* <span data-ttu-id="74cc3-127">**A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: parancsmag [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) helyezze át egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül, és konfigurálja úgy, hogy használata Belső virtuális hálózat típusát.</span><span class="sxs-lookup"><span data-stu-id="74cc3-127">**Deploy an existing API Management service inside a VNET**: Use the cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) to move an existing Azure API Management service inside a Virtual network and configure it to use Internal VNET type.</span></span>

## <span data-ttu-id="74cc3-128"><a name="apim-dns-configuration"></a>DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="74cc3-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="74cc3-129">Ha az API Management külső virtuális hálózati módban, Azure DNS kezeli.</span><span class="sxs-lookup"><span data-stu-id="74cc3-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="74cc3-130">Belső virtuális hálózat mód felügyelni a saját DNS kell.</span><span class="sxs-lookup"><span data-stu-id="74cc3-130">For Internal Virtual network mode, you have to manage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="74cc3-131">API-kezelés szolgáltatás nem figyel az IP-címek érkező kérésekre.</span><span class="sxs-lookup"><span data-stu-id="74cc3-131">API Management service does not listen to requests coming on IP addresses.</span></span> <span data-ttu-id="74cc3-132">Csak a konfigurált szolgáltatás végpontját az állomásnév (amely tartalmazza az átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet végpont és git) kérelmére reagálás.</span><span class="sxs-lookup"><span data-stu-id="74cc3-132">It only responds to requests to the Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="74cc3-133">Alapértelmezett állomásnevek hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="74cc3-133">Access on default host names:</span></span>
<span data-ttu-id="74cc3-134">Amikor létrehoz egy API-kezelés szolgáltatás nyilvános Azure felhőben, például a "contoso" nevű, a következő végpontok alapértelmezés szerint vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="74cc3-134">When you create an API Management service in public Azure cloud, named "contoso" for example, the following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="74cc3-135">Átjáró / Proxy - contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="74cc3-136">Közzétevő és fejlesztői portálhoz - contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="74cc3-137">Közvetlen felügyelet végpont - contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="74cc3-138">GIT - contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="74cc3-139">API-kezelés szolgáltatás a végpontokkal eléréséhez hozzon létre egy virtuális gépet a virtuális hálózathoz van telepítve az API Management alhálózat.</span><span class="sxs-lookup"><span data-stu-id="74cc3-139">To access these API Management service endpoints, you can create a Virtual Machine in a subnet connected to the Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="74cc3-140">A szolgáltatás 10.0.0.5 érték, ha a belső virtuális IP-címet, elvégezhető a gazdagép (% SystemDrive%\drivers\etc\hosts) leképezési fájl az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="74cc3-140">Assuming the Internal Virtual IP Address for your service is 10.0.0.5, you can do the hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="74cc3-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="74cc3-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="74cc3-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="74cc3-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="74cc3-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="74cc3-145">Ezután hozzáférhetnek a Szolgáltatásvégpontok létrehozott virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="74cc3-145">You can then access all the service endpoints from the Virtual Machine you created.</span></span> <span data-ttu-id="74cc3-146">Ha egy egyéni DNS-kiszolgáló használatával egy virtuális hálózatban, is A DNS-rekordok létrehozása, és ezeket a végpontokat hozzáférni bárhonnan virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="74cc3-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="74cc3-147">Egyéni tartománynevek hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="74cc3-147">Access on custom domain names:</span></span>
<span data-ttu-id="74cc3-148">Ha nem szeretné, az API Management szolgáltatás az alapértelmezett állomásneveket eléréséhez, beállíthatja az összes a végpontok például alatt használt egyéni tartománynevek</span><span class="sxs-lookup"><span data-stu-id="74cc3-148">If you don’t want to access the API Management service with the default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Egyéni tartománynév beállítása az API Management][api-management-custom-domain-name]

<span data-ttu-id="74cc3-150">Majd hozhat létre A rekordokat a DNS-kiszolgáló eléréséhez ezeket a végpontokat, amelyek csak a virtuális hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="74cc3-150">Then you can create A records in your DNS Server to access these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="74cc3-151"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="74cc3-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="74cc3-152">[Általános hálózati konfigurációs problémák virtuális APIM beállítása közben][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="74cc3-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="74cc3-153">Virtuális hálózat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="74cc3-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="74cc3-154">Létrehozása A rekord a DNS-ben</span><span class="sxs-lookup"><span data-stu-id="74cc3-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
