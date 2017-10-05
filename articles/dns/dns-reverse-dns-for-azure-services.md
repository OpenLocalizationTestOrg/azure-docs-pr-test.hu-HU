---
title: "Névkeresési DNS az Azure-szolgáltatásokhoz |} Microsoft Docs"
description: "Az Azure-ban tárolt szolgáltatások DNS-névlekérdezés konfigurálása"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="077f8-103">Az Azure-ban tárolt szolgáltatások címfeloldási DNS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="077f8-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="077f8-104">Ez a cikk az Azure-ban tárolt szolgáltatások DNS-névlekérdezés konfigurálását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="077f8-104">This article explains how to configure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="077f8-105">Az Azure szolgáltatások IP-címek hozzárendelése az Azure-ban, és a Microsoft tulajdonában használják.</span><span class="sxs-lookup"><span data-stu-id="077f8-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="077f8-106">A címfeloldási DNS-rekordok (PTR-rekordok) kell létrehozni a megfelelő Microsoft által birtokolt névkeresési DNS-címkeresési zónák.</span><span class="sxs-lookup"><span data-stu-id="077f8-106">These reverse DNS records (PTR records) must be created in the corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="077f8-107">Ez a cikk azt ismerteti, hogyan ehhez.</span><span class="sxs-lookup"><span data-stu-id="077f8-107">This article explains how to do this.</span></span>

<span data-ttu-id="077f8-108">Ez a forgatókönyv nem keverendő teszi [üzemeltetni a hozzárendelt IP-címtartományokhoz az Azure DNS-névkeresési DNS-címkeresési zónák](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="077f8-108">This scenario should not be confused with the ability to [host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="077f8-109">Ebben az esetben az IP-címtartományai a névkeresési zóna által képviselt kell rendelni a szervezetben, általában az Internetszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="077f8-109">In this case, the IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="077f8-110">A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="077f8-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="077f8-111">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="077f8-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="077f8-112">A Resource Manager üzembe helyezési modellel számítási erőforrások (például virtuális gépek, a virtuálisgép-méretezési csoportok vagy a Service Fabric-fürtök) PublicIpAddress erőforrás keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="077f8-112">In the Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="077f8-113">DNS-névlekérdezés úgy vannak konfigurálva, a PublicIpAddress "ReverseFqdn" tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="077f8-113">Reverse DNS lookups are configured using the 'ReverseFqdn' property of the PublicIpAddress.</span></span>
* <span data-ttu-id="077f8-114">A klasszikus üzembe helyezési modellel a számítási erőforrások elérhetők felhőalapú szolgáltatások segítségével.</span><span class="sxs-lookup"><span data-stu-id="077f8-114">In the Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="077f8-115">DNS-névlekérdezés úgy vannak konfigurálva, a felhőalapú szolgáltatás "ReverseDnsFqdn" tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="077f8-115">Reverse DNS lookups are configured using the 'ReverseDnsFqdn' property of the Cloud Service.</span></span>

<span data-ttu-id="077f8-116">Névkeresési DNS jelenleg nem támogatott az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="077f8-116">Reverse DNS is not currently supported for the Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="077f8-117">A címfeloldási DNS-rekordok érvényesítése</span><span class="sxs-lookup"><span data-stu-id="077f8-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="077f8-118">A harmadik fél nem kell tudni az Azure-szolgáltatás a leképezés csak akkor a DNS-tartományok címfeloldási DNS-rekordok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="077f8-118">A third party should not be able to create reverse DNS records for their Azure service mapping to your DNS domains.</span></span> <span data-ttu-id="077f8-119">Ennek megelőzése érdekében Azure csak lehetővé teszi, hogy amennyiben a címfeloldási DNS-rekord megadott tartománynév megegyezik a címfeloldási DNS-rekord létrehozása, vagy oldja fel, a DNS-nevét vagy IP-címhez a nyilvános vagy a Felhőszolgáltatás azonos Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="077f8-119">To prevent this, Azure only allows the creation of a reverse DNS record where domain name specified in the reverse DNS record is the same as, or resolves to, the DNS name or IP address of a PublicIpAddress or Cloud Service in the same Azure subscription.</span></span>

<span data-ttu-id="077f8-120">Az ellenőrzés csak akkor hajtható végre, ha a címfeloldási DNS-rekord beállítani vagy módosítani.</span><span class="sxs-lookup"><span data-stu-id="077f8-120">This validation is only performed when the reverse DNS record is set or modified.</span></span> <span data-ttu-id="077f8-121">Rendszeres ismételt érvényesítése nem megy végbe.</span><span class="sxs-lookup"><span data-stu-id="077f8-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="077f8-122">Például: Tegyük fel, hogy a nyilvános erőforrás van, a DNS-név contosoapp1.northus.cloudapp.azure.com és IP-cím 23.96.52.53.</span><span class="sxs-lookup"><span data-stu-id="077f8-122">For example: suppose the PublicIpAddress resource has the DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="077f8-123">Az a PublicIpAddress ReverseFqdn adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="077f8-123">The ReverseFqdn for the PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="077f8-124">A nyilvános DNS-nevének contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="077f8-124">The DNS name for the PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="077f8-125">Ugyanazt az előfizetést, például a contosoapp2.westus.cloudapp.azure.com egy másik nyilvános DNS-neve</span><span class="sxs-lookup"><span data-stu-id="077f8-125">The DNS name for a different PublicIpAddress in the same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="077f8-126">A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurálva, egy CNAME contosoapp1.northus.cloudapp.azure.com, vagy egy másik nyilvános ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="077f8-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME to contosoapp1.northus.cloudapp.azure.com, or to a different PublicIpAddress in the same subscription.</span></span>
* <span data-ttu-id="077f8-127">A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurálva, az A rekord az IP-cím 23.96.52.53, vagy olyan ugyanazt az előfizetést másik nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="077f8-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record to the IP address 23.96.52.53, or to the IP address of a different PublicIpAddress in the same subscription.</span></span>

<span data-ttu-id="077f8-128">Az azonos megkötések alkalmazása névkeresési DNS-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="077f8-128">The same constraints apply to reverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="077f8-129">Névkeresési DNS PublicIpAddress erőforrások</span><span class="sxs-lookup"><span data-stu-id="077f8-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="077f8-130">Ez a témakör részletes útmutatást PublicIpAddress erőforrások címfeloldási DNS konfigurálása a Resource Manager üzembe helyezési modellel, az Azure PowerShell, Azure CLI 1.0 vagy Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="077f8-130">This section provides detailed instructions for how to configure reverse DNS for PublicIpAddress resources in the Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="077f8-131">Nyilvános erőforrások címfeloldási DNS konfigurálása jelenleg nem támogatott az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="077f8-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via the Azure portal.</span></span>

<span data-ttu-id="077f8-132">Az Azure jelenleg támogatja a fordított DNS kizárólag a nyilvános IPv4-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="077f8-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="077f8-133">Az IPv6 nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="077f8-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a><span data-ttu-id="077f8-134">Névkeresési DNS hozzáadása egy meglévő PublicIpAddresses</span><span class="sxs-lookup"><span data-stu-id="077f8-134">Add reverse DNS to an existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="077f8-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="077f8-135">PowerShell</span></span>

<span data-ttu-id="077f8-136">Névkeresési DNS hozzáadása egy meglévő nyilvános:</span><span class="sxs-lookup"><span data-stu-id="077f8-136">To add reverse DNS to an existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="077f8-137">Névkeresési DNS hozzáadása egy meglévő nyilvános, amely még nem rendelkezik a DNS-név, is meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="077f8-137">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="077f8-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="077f8-138">Azure CLI 1.0</span></span>

<span data-ttu-id="077f8-139">Névkeresési DNS hozzáadása egy meglévő nyilvános:</span><span class="sxs-lookup"><span data-stu-id="077f8-139">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="077f8-140">Névkeresési DNS hozzáadása egy meglévő nyilvános, amely még nem rendelkezik a DNS-név, is meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="077f8-140">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="077f8-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="077f8-141">Azure CLI 2.0</span></span>

<span data-ttu-id="077f8-142">Névkeresési DNS hozzáadása egy meglévő nyilvános:</span><span class="sxs-lookup"><span data-stu-id="077f8-142">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="077f8-143">Névkeresési DNS hozzáadása egy meglévő nyilvános, amely még nem rendelkezik a DNS-név, is meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="077f8-143">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="077f8-144">Hozzon létre egy nyilvános IP-cím a címfeloldási DNS-ben</span><span class="sxs-lookup"><span data-stu-id="077f8-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="077f8-145">A címfeloldási DNS tulajdonság már meg van adva egy új nyilvános létrehozása:</span><span class="sxs-lookup"><span data-stu-id="077f8-145">To create a new PublicIpAddress with the reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="077f8-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="077f8-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="077f8-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="077f8-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="077f8-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="077f8-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="077f8-149">Egy meglévő nyilvános címfeloldási DNS megtekintése</span><span class="sxs-lookup"><span data-stu-id="077f8-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="077f8-150">A konfigurált értéket egy meglévő nyilvános megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="077f8-150">To view the configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="077f8-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="077f8-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="077f8-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="077f8-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="077f8-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="077f8-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="077f8-154">Távolítsa el a címfeloldási DNS a meglévő nyilvános IP-címek</span><span class="sxs-lookup"><span data-stu-id="077f8-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="077f8-155">A címfeloldási DNS tulajdonság eltávolítása egy meglévő nyilvános:</span><span class="sxs-lookup"><span data-stu-id="077f8-155">To remove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="077f8-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="077f8-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="077f8-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="077f8-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="077f8-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="077f8-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="077f8-159">Névkeresési DNS konfigurálása a Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="077f8-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="077f8-160">Ez a témakör részletes útmutatást a Felhőszolgáltatások fordított DNS konfigurálása a klasszikus üzembe helyezési modellel, az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="077f8-160">This section provides detailed instructions for how to configure reverse DNS for Cloud Services in the Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="077f8-161">Névkeresési DNS konfigurálása a Cloud Services nem támogatja az Azure-portál, Azure CLI 1.0-s vagy Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="077f8-161">Configuring reverse DNS for Cloud Services is not supported via the Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-to-existing-cloud-services"></a><span data-ttu-id="077f8-162">Névkeresési DNS hozzáadása meglévő Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="077f8-162">Add reverse DNS to existing Cloud Services</span></span>

<span data-ttu-id="077f8-163">A címfeloldási DNS-rekord hozzáadása egy meglévő felhőalapú szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="077f8-163">To add a reverse DNS record to an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="077f8-164">A címfeloldási DNS-felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="077f8-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="077f8-165">Új felhőalapú szolgáltatás létrehozása a címfeloldási DNS tulajdonság már meg van adva:</span><span class="sxs-lookup"><span data-stu-id="077f8-165">To create a new Cloud Service with the reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="077f8-166">Nézet névkeresési DNS meglévő Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="077f8-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="077f8-167">Létező felhőalapú szolgáltatást szeretné megtekinteni a címfeloldási DNS-tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="077f8-167">To view the reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="077f8-168">Távolítsa el a címfeloldási DNS a meglévő Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="077f8-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="077f8-169">A címfeloldási DNS tulajdonság eltávolítása egy meglévő felhőalapú szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="077f8-169">To remove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="077f8-170">GYIK</span><span class="sxs-lookup"><span data-stu-id="077f8-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="077f8-171">Milyen mértékű névkeresési DNS-rekordok költség?</span><span class="sxs-lookup"><span data-stu-id="077f8-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="077f8-172">Szabad fontosságúak!</span><span class="sxs-lookup"><span data-stu-id="077f8-172">They're free!</span></span>  <span data-ttu-id="077f8-173">Nincs a címfeloldási DNS-rekordok vagy lekérdezések további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="077f8-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a><span data-ttu-id="077f8-174">A címfeloldási DNS-rekordok feloldja az internetről?</span><span class="sxs-lookup"><span data-stu-id="077f8-174">Will my reverse DNS records resolve from the internet?</span></span>

<span data-ttu-id="077f8-175">Igen.</span><span class="sxs-lookup"><span data-stu-id="077f8-175">Yes.</span></span> <span data-ttu-id="077f8-176">Ha egyszer már megadta a címfeloldási DNS-tulajdonság az Azure Service, Azure kezeli a DNS-delegálás és győződjön meg arról, hogy címfeloldási DNS-rekord megszünteti az összes internetes felhasználó számára szükséges DNS-zónák.</span><span class="sxs-lookup"><span data-stu-id="077f8-176">Once you set the reverse DNS property for your Azure service, Azure manages all the DNS delegations and DNS zones required to ensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="077f8-177">Alapértelmezett címfeloldási DNS-rekordok létrejönnek az az Azure-szolgáltatásokat?</span><span class="sxs-lookup"><span data-stu-id="077f8-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="077f8-178">Nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-178">No.</span></span> <span data-ttu-id="077f8-179">Névkeresési DNS egyik választható szolgáltatása nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="077f8-180">Nincs alapértelmezett címfeloldási DNS-rekordok jönnek létre, ha nem konfigurálja őket.</span><span class="sxs-lookup"><span data-stu-id="077f8-180">No default reverse DNS records are created if you choose not to configure them.</span></span>

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="077f8-181">Mi az a teljes tartománynév (FQDN) formátumban?</span><span class="sxs-lookup"><span data-stu-id="077f8-181">What is the format for the fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="077f8-182">Teljes tartománynevek előre sorrendben legyenek megadva, és egy pontot (például "app1.contoso.com.") kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="077f8-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a><span data-ttu-id="077f8-183">Mi történik, ha az eredetiség ellenőrzésének a címfeloldási DNS-fiókjához megadott sikertelen?</span><span class="sxs-lookup"><span data-stu-id="077f8-183">What happens if the validation check for the reverse DNS I've specified fails?</span></span>

<span data-ttu-id="077f8-184">A névkeresési DNS-ellenőrzés sikertelen lesz, ahol konfigurálhatja a címfeloldási DNS-rekord a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="077f8-184">Where the reverse DNS validation check fails, the operation to configure the reverse DNS record fails.</span></span> <span data-ttu-id="077f8-185">Javítsa ki a címfeloldási DNS értékét szükség szerint, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="077f8-185">Correct the reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="077f8-186">Beállítható címfeloldási DNS az Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="077f8-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="077f8-187">Nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-187">No.</span></span> <span data-ttu-id="077f8-188">Névkeresési DNS nem támogatott az Azure App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="077f8-188">Reverse DNS is not supported for the Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="077f8-189">Beállítható több címfeloldási DNS-rekordok az Azure Service?</span><span class="sxs-lookup"><span data-stu-id="077f8-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="077f8-190">Nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-190">No.</span></span> <span data-ttu-id="077f8-191">Azure egyetlen címfeloldási DNS-rekord támogatja az egyes Azure Cloud Service vagy nyilvános.</span><span class="sxs-lookup"><span data-stu-id="077f8-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="077f8-192">Konfigurálhatja a címfeloldási DNS IPv6 PublicIpAddress erőforrások?</span><span class="sxs-lookup"><span data-stu-id="077f8-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="077f8-193">Nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-193">No.</span></span> <span data-ttu-id="077f8-194">Az Azure jelenleg támogatja a fordított DNS csak az IPv4-alapú PublicIpAddress erőforrások és a Felhőszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="077f8-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a><span data-ttu-id="077f8-195">Lehet küldeni e-mailek külső tartományokhoz a saját Azure számítási szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="077f8-195">Can I send emails to external domains from my Azure Compute services?</span></span>

<span data-ttu-id="077f8-196">Nem.</span><span class="sxs-lookup"><span data-stu-id="077f8-196">No.</span></span> [<span data-ttu-id="077f8-197">Az Azure Compute szolgáltatások nem támogatják a külső tartományokkal az e-maileket küldő</span><span class="sxs-lookup"><span data-stu-id="077f8-197">Azure Compute services do not support sending emails to external domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="077f8-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="077f8-198">Next steps</span></span>

<span data-ttu-id="077f8-199">A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="077f8-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="077f8-200">Megtudhatja, hogyan [az az Internetszolgáltató által hozzárendelt IP-címtartományt, az Azure DNS-névkeresési zóna](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="077f8-200">Learn how to [host the reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>

