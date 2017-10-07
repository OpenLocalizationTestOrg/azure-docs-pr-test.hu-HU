---
title: "aaaHow toouse Azure API Management belső virtuális hálózattal |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup és konfigurálása az Azure API Management belső virtuális hálózat."
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
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="00128-103">A belső virtuális hálózattal Azure API Management szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="00128-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="00128-104">Azure virtuális hálózatokról (Vnetekről) az API Management API-k hello Internet nem érhető el tud kezelni.</span><span class="sxs-lookup"><span data-stu-id="00128-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on hello Internet.</span></span> <span data-ttu-id="00128-105">VPN technológiáin számos elérhető toomake hello kapcsolat, és az API Management hello virtuális hálózaton belül két fő módban telepíthető:</span><span class="sxs-lookup"><span data-stu-id="00128-105">A number of VPN technologies are available toomake hello connection and API Management can be deployed in two main modes inside hello VNET:</span></span>
* <span data-ttu-id="00128-106">Külső</span><span class="sxs-lookup"><span data-stu-id="00128-106">External</span></span>
* <span data-ttu-id="00128-107">Belső</span><span class="sxs-lookup"><span data-stu-id="00128-107">Internal</span></span>

## <span data-ttu-id="00128-108"><a name="overview"></a>Áttekintés</span><span class="sxs-lookup"><span data-stu-id="00128-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="00128-109">Az API Management egy belső virtuális hálózat módban telepítésekor minden hello Szolgáltatásvégpontok (átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet és Git) láthatók csak való hozzáférés szabályozása virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="00128-109">When API Management is deployed in an Internal Virtual network mode, all hello service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="00128-110">Hello Szolgáltatásvégpontok egyike a hello nyilvános DNS-kiszolgáló van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="00128-110">None of hello service endpoints are registered on hello Public DNS Server.</span></span>

<span data-ttu-id="00128-111">Az API Management belső módban, érhet el a következő forgatókönyvek hello</span><span class="sxs-lookup"><span data-stu-id="00128-111">Using API Management in Internal mode, you can achieve hello following scenarios</span></span>
* <span data-ttu-id="00128-112">3. fél kívül, pont-pont vagy ExpressRoute VPN-kapcsolatok használatával érhető el a privát adatközpontban-környezetben üzemeltetett API biztonságosan ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="00128-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="00128-113">Engedélyezze a hibrid felhős rendszerekben jelentkezik, mintha a API-k felhőalapú és helyszíni API-k közös átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="00128-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="00128-114">Az átjáró egyetlen végpont használatával több földrajzi helyeken található üzemeltetett API kezelése.</span><span class="sxs-lookup"><span data-stu-id="00128-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="00128-115"><a name="enable-vpn"></a>Az API Management belső virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="00128-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="00128-116">hello API-kezelés szolgáltatás a belső virtuális hálózatban egy belső terheléselosztási Balancer(ILB) mögött helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="00128-116">hello API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="00128-117">hello hello ILB IP-címe van hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) tartományon.</span><span class="sxs-lookup"><span data-stu-id="00128-117">hello IP Address of hello ILB is in hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="00128-118">Azure-portál használatával VNET-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="00128-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="00128-119">Először hozzon létre hello API-kezelés szolgáltatás hello lépéseket követve [létrehozása API-kezelés szolgáltatás][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="00128-119">First create hello API Management service by following hello steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="00128-120">Majd beállításról API Management toobe rendszerbe a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="00128-120">Then set-up API Management toobe deployed inside a Virtual network.</span></span>

![Belső virtuális hálózat APIM beállításának menü][api-management-using-internal-vnet-menu]

<span data-ttu-id="00128-122">Miután hello központi telepítés sikeres, meg kell jelennie hello belső virtuális IP-címet, a szolgáltatás hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="00128-122">After hello deployment succeeds, you should see hello Internal Virtual IP Address of your service on hello dashboard.</span></span>

![Belső virtuális hálózaton konfigurált API-kezelési irányítópult][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="00128-124">Powershell-parancsmagok használatával VNET-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="00128-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="00128-125">VNET-kapcsolatot hello PowerShell-parancsmagok használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="00128-125">You can also enable VNET connectivity using hello PowerShell cmdlets.</span></span>

* <span data-ttu-id="00128-126">**A VNETEN belül az API Management szolgáltatás létrehozása**: hello parancsmaggal [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate az Azure API Management szolgáltatás a VNETEN belül, és állítsa be toouse hello belső virtuális hálózat típusát.</span><span class="sxs-lookup"><span data-stu-id="00128-126">**Create an API Management service inside a VNET**: Use hello cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate an Azure API Management service inside a VNET and configure it toouse hello Internal VNET type.</span></span>

* <span data-ttu-id="00128-127">**A VNETEN belül az meglévő API Management-szolgáltatások üzembe**: hello parancsmaggal [frissítés-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove egy meglévő Azure API Management szolgáltatást egy virtuális hálózaton belül, és konfigurálja toouse Belső virtuális hálózat típusát.</span><span class="sxs-lookup"><span data-stu-id="00128-127">**Deploy an existing API Management service inside a VNET**: Use hello cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove an existing Azure API Management service inside a Virtual network and configure it toouse Internal VNET type.</span></span>

## <span data-ttu-id="00128-128"><a name="apim-dns-configuration"></a>DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="00128-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="00128-129">Ha az API Management külső virtuális hálózati módban, Azure DNS kezeli.</span><span class="sxs-lookup"><span data-stu-id="00128-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="00128-130">Belső virtuális hálózat mód hogy toomanage a saját DNS.</span><span class="sxs-lookup"><span data-stu-id="00128-130">For Internal Virtual network mode, you have toomanage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="00128-131">API-kezelés szolgáltatás nem figyel toorequests hamarosan IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="00128-131">API Management service does not listen toorequests coming on IP addresses.</span></span> <span data-ttu-id="00128-132">Csak reagáljon toorequests toohello állomásnév konfigurált szolgáltatás végpontját (amely tartalmazza az átjáró, fejlesztői portálján, publisher portált, közvetlen felügyelet végpont és git).</span><span class="sxs-lookup"><span data-stu-id="00128-132">It only responds toorequests toohello Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="00128-133">Alapértelmezett állomásnevek hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="00128-133">Access on default host names:</span></span>
<span data-ttu-id="00128-134">Amikor létrehoz egy API-kezelés szolgáltatás nyilvános Azure felhőben, például a "contoso" nevű, hello következő végpontok által konfigurált alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="00128-134">When you create an API Management service in public Azure cloud, named "contoso" for example, hello following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="00128-135">Átjáró / Proxy - contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="00128-136">Közzétevő és fejlesztői portálhoz - contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="00128-137">Közvetlen felügyelet végpont - contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="00128-138">GIT - contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="00128-139">tooaccess ezeket API-kezelés szolgáltatás a végpontokat, létrehozhat egy virtuális gép van telepítve az API Management kapcsolódó alhálózati toohello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="00128-139">tooaccess these API Management service endpoints, you can create a Virtual Machine in a subnet connected toohello Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="00128-140">A szolgáltatás belső virtuális IP-cím hello feltéve 10.0.0.5, megteheti az alábbiak szerint hello állomások fájlhozzárendelést (% SystemDrive%\drivers\etc\hosts):</span><span class="sxs-lookup"><span data-stu-id="00128-140">Assuming hello Internal Virtual IP Address for your service is 10.0.0.5, you can do hello hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="00128-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="00128-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="00128-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="00128-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00128-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="00128-145">A létrehozott virtuális gép hello majd hello szolgáltatás végpontjai érheti el.</span><span class="sxs-lookup"><span data-stu-id="00128-145">You can then access all hello service endpoints from hello Virtual Machine you created.</span></span> <span data-ttu-id="00128-146">Ha egy egyéni DNS-kiszolgáló használatával egy virtuális hálózatban, is A DNS-rekordok létrehozása, és ezeket a végpontokat hozzáférni bárhonnan virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="00128-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="00128-147">Egyéni tartománynevek hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="00128-147">Access on custom domain names:</span></span>
<span data-ttu-id="00128-148">Ha nem szeretné tooaccess hello hello alapértelmezett állomásnevek API Management szolgáltatást, beállíthatja az összes a végpontok például alatt használt egyéni tartománynevek</span><span class="sxs-lookup"><span data-stu-id="00128-148">If you don’t want tooaccess hello API Management service with hello default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Egyéni tartománynév beállítása az API Management][api-management-custom-domain-name]

<span data-ttu-id="00128-150">Ezután létrehozhat egy rekordot a DNS-kiszolgáló tooaccess ezeket a végpontokat, amelyek csak a virtuális hálózaton belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="00128-150">Then you can create A records in your DNS Server tooaccess these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="00128-151"><a name="related-content"></a>Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="00128-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="00128-152">[Általános hálózati konfigurációs problémák virtuális APIM beállítása közben][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="00128-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="00128-153">Virtuális hálózat – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="00128-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="00128-154">Létrehozása A rekord a DNS-ben</span><span class="sxs-lookup"><span data-stu-id="00128-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
