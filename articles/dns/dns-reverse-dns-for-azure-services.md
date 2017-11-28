---
title: "az Azure szolgáltatások DNS aaaReverse |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure fordított az Azure-ban tárolt szolgáltatások DNS-keresések"
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
ms.openlocfilehash: c6fe1d80232f124be86dd7fc57abc20699be7eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="13218-103">Az Azure-ban tárolt szolgáltatások címfeloldási DNS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="13218-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="13218-104">Ez a cikk azt ismerteti, hogyan tooconfigure névkeresési DNS-keresések az Azure-ban tárolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="13218-104">This article explains how tooconfigure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="13218-105">Az Azure szolgáltatások IP-címek hozzárendelése az Azure-ban, és a Microsoft tulajdonában használják.</span><span class="sxs-lookup"><span data-stu-id="13218-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="13218-106">A címfeloldási DNS-rekordok (PTR-rekordok) hello megfelelő Microsoft által birtokolt névkeresési DNS-címkeresési zónák kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="13218-106">These reverse DNS records (PTR records) must be created in hello corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="13218-107">Ez a cikk azt ismerteti, hogyan toodo ez.</span><span class="sxs-lookup"><span data-stu-id="13218-107">This article explains how toodo this.</span></span>

<span data-ttu-id="13218-108">Ez a forgatókönyv nem tévesztendők össze a hello képességét túl[hello névkeresési DNS-címkeresési zónák a hozzárendelt IP-címtartományokhoz az Azure DNS-állomás](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="13218-108">This scenario should not be confused with hello ability too[host hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="13218-109">Ebben az esetben hello IP-címtartományok hello névkeresési zóna által képviselt kell rendelni tooyour a szervezeten belül, általában az Internetszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="13218-109">In this case, hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="13218-110">A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="13218-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="13218-111">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="13218-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="13218-112">Hello Resource Manager üzembe helyezési modellel számítási erőforrások (például virtuális gépek, a virtuálisgép-méretezési csoportok vagy a Service Fabric-fürtök) PublicIpAddress erőforrás keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="13218-112">In hello Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="13218-113">DNS-névlekérdezés hello "ReverseFqdn" hello PublicIpAddress tulajdonsága használatával vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="13218-113">Reverse DNS lookups are configured using hello 'ReverseFqdn' property of hello PublicIpAddress.</span></span>
* <span data-ttu-id="13218-114">Hello klasszikus üzembe helyezési modellel a számítási erőforrások elérhetők felhőalapú szolgáltatások segítségével.</span><span class="sxs-lookup"><span data-stu-id="13218-114">In hello Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="13218-115">DNS-névlekérdezés hello Felhőszolgáltatás hello "ReverseDnsFqdn" tulajdonságának használatával vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="13218-115">Reverse DNS lookups are configured using hello 'ReverseDnsFqdn' property of hello Cloud Service.</span></span>

<span data-ttu-id="13218-116">Névkeresési DNS jelenleg nem támogatott a hello Azure App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="13218-116">Reverse DNS is not currently supported for hello Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="13218-117">A címfeloldási DNS-rekordok érvényesítése</span><span class="sxs-lookup"><span data-stu-id="13218-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="13218-118">A harmadik fél nem lehet képes toocreate címfeloldási DNS-rekordok az Azure-szolgáltatások leképezési tooyour DNS-tartományok.</span><span class="sxs-lookup"><span data-stu-id="13218-118">A third party should not be able toocreate reverse DNS records for their Azure service mapping tooyour DNS domains.</span></span> <span data-ttu-id="13218-119">tooprevent, Azure csak lehetővé teszi a hello címfeloldási DNS-rekord megadott tartománynév esetén az azonos hello, vagy megegyezik a hello szolgáltatás oldja fel a rendszer, DNS-nevét vagy IP-címét egy nyilvános vagy felhő hello címfeloldási DNS-rekord létrehozása hello Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="13218-119">tooprevent this, Azure only allows hello creation of a reverse DNS record where domain name specified in hello reverse DNS record is hello same as, or resolves to, hello DNS name or IP address of a PublicIpAddress or Cloud Service in hello same Azure subscription.</span></span>

<span data-ttu-id="13218-120">Az ellenőrzés csak akkor hajtható végre, ha hello címfeloldási DNS-rekord beállítani vagy módosítani.</span><span class="sxs-lookup"><span data-stu-id="13218-120">This validation is only performed when hello reverse DNS record is set or modified.</span></span> <span data-ttu-id="13218-121">Rendszeres ismételt érvényesítése nem megy végbe.</span><span class="sxs-lookup"><span data-stu-id="13218-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="13218-122">Például: Tegyük fel, hogy hello PublicIpAddress erőforrás hello DNS-név contosoapp1.northus.cloudapp.azure.com és IP-cím 23.96.52.53 rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="13218-122">For example: suppose hello PublicIpAddress resource has hello DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="13218-123">ReverseFqdn hello hello PublicIpAddress adhatók meg:</span><span class="sxs-lookup"><span data-stu-id="13218-123">hello ReverseFqdn for hello PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="13218-124">hello PublicIpAddress hello DNS-nevét, contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="13218-124">hello DNS name for hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="13218-125">hello DNS-beli név olyan hello a különböző nyilvános az azonos előfizetést, például az contosoapp2.westus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="13218-125">hello DNS name for a different PublicIpAddress in hello same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="13218-126">A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurált, egy CNAME toocontosoapp1.northus.cloudapp.azure.com vagy tooa különböző PublicIpAddress hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="13218-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME toocontosoapp1.northus.cloudapp.azure.com, or tooa different PublicIpAddress in hello same subscription.</span></span>
* <span data-ttu-id="13218-127">A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurált IP-címet A rekord toohello 23.96.52.53, vagy egy másik nyilvános IP-címe toohello hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="13218-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record toohello IP address 23.96.52.53, or toohello IP address of a different PublicIpAddress in hello same subscription.</span></span>

<span data-ttu-id="13218-128">hello azonos megkötések alkalmazása tooreverse DNS-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="13218-128">hello same constraints apply tooreverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="13218-129">Névkeresési DNS PublicIpAddress erőforrások</span><span class="sxs-lookup"><span data-stu-id="13218-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="13218-130">Ez a témakör részletes útmutatást hogyan tooconfigure fordított PublicIpAddress erőforrások hello Resource Manager üzembe helyezési modellel, az Azure PowerShell, Azure CLI 1.0 vagy Azure CLI 2.0 DNS.</span><span class="sxs-lookup"><span data-stu-id="13218-130">This section provides detailed instructions for how tooconfigure reverse DNS for PublicIpAddress resources in hello Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="13218-131">Nyilvános erőforrások címfeloldási DNS konfigurálása jelenleg nem támogatott hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="13218-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via hello Azure portal.</span></span>

<span data-ttu-id="13218-132">Az Azure jelenleg támogatja a fordított DNS kizárólag a nyilvános IPv4-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="13218-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="13218-133">Az IPv6 nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="13218-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a><span data-ttu-id="13218-134">Meglévő PublicIpAddresses címfeloldási DNS tooan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="13218-134">Add reverse DNS tooan existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="13218-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13218-135">PowerShell</span></span>

<span data-ttu-id="13218-136">tooadd címfeloldási DNS tooan meglévő PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="13218-136">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="13218-137">tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="13218-137">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="13218-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="13218-138">Azure CLI 1.0</span></span>

<span data-ttu-id="13218-139">tooadd címfeloldási DNS tooan meglévő PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="13218-139">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="13218-140">tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="13218-140">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="13218-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13218-141">Azure CLI 2.0</span></span>

<span data-ttu-id="13218-142">tooadd címfeloldási DNS tooan meglévő PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="13218-142">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="13218-143">tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:</span><span class="sxs-lookup"><span data-stu-id="13218-143">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="13218-144">Hozzon létre egy nyilvános IP-cím a címfeloldási DNS-ben</span><span class="sxs-lookup"><span data-stu-id="13218-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="13218-145">új toocreate hello nyilvános névkeresési DNS tulajdonság már meg van adva:</span><span class="sxs-lookup"><span data-stu-id="13218-145">toocreate a new PublicIpAddress with hello reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="13218-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13218-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="13218-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="13218-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="13218-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13218-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="13218-149">Egy meglévő nyilvános címfeloldási DNS megtekintése</span><span class="sxs-lookup"><span data-stu-id="13218-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="13218-150">tooview hello értéket egy meglévő nyilvános konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="13218-150">tooview hello configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="13218-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13218-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="13218-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="13218-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="13218-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13218-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="13218-154">Távolítsa el a címfeloldási DNS a meglévő nyilvános IP-címek</span><span class="sxs-lookup"><span data-stu-id="13218-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="13218-155">a címfeloldási DNS-tulajdonságot egy meglévő nyilvános tooremove:</span><span class="sxs-lookup"><span data-stu-id="13218-155">tooremove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="13218-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13218-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="13218-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="13218-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="13218-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13218-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="13218-159">Névkeresési DNS konfigurálása a Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="13218-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="13218-160">Ez a témakör részletes útmutatást hogyan tooconfigure névkeresési DNS-szolgáltatásokhoz hello klasszikus üzembe helyezési modellel, az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="13218-160">This section provides detailed instructions for how tooconfigure reverse DNS for Cloud Services in hello Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="13218-161">Névkeresési DNS konfigurálása a Cloud Services Azure CLI 1.0-s vagy Azure CLI 2.0 hello Azure-portálon keresztül nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="13218-161">Configuring reverse DNS for Cloud Services is not supported via hello Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-tooexisting-cloud-services"></a><span data-ttu-id="13218-162">Adja hozzá a címfeloldási DNS tooexisting Cloud Services</span><span class="sxs-lookup"><span data-stu-id="13218-162">Add reverse DNS tooexisting Cloud Services</span></span>

<span data-ttu-id="13218-163">létező felhőalapú szolgáltatás, címfeloldási DNS rekord tooan tooadd:</span><span class="sxs-lookup"><span data-stu-id="13218-163">tooadd a reverse DNS record tooan existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="13218-164">A címfeloldási DNS-felhőalapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="13218-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="13218-165">toocreate hello címfeloldási DNS a tulajdonság már meg van adva egy új Felhőszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="13218-165">toocreate a new Cloud Service with hello reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="13218-166">Nézet névkeresési DNS meglévő Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="13218-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="13218-167">tooview hello címfeloldási DNS tulajdonsága egy meglévő felhőalapú szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="13218-167">tooview hello reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="13218-168">Távolítsa el a címfeloldási DNS a meglévő Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="13218-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="13218-169">a fordított DNS-tulajdonságot egy meglévő Felhőszolgáltatáshoz tooremove:</span><span class="sxs-lookup"><span data-stu-id="13218-169">tooremove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="13218-170">GYIK</span><span class="sxs-lookup"><span data-stu-id="13218-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="13218-171">Milyen mértékű névkeresési DNS-rekordok költség?</span><span class="sxs-lookup"><span data-stu-id="13218-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="13218-172">Szabad fontosságúak!</span><span class="sxs-lookup"><span data-stu-id="13218-172">They're free!</span></span>  <span data-ttu-id="13218-173">Nincs a címfeloldási DNS-rekordok vagy lekérdezések további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="13218-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a><span data-ttu-id="13218-174">A címfeloldási DNS rekordok hárítsa el a hello fog internet?</span><span class="sxs-lookup"><span data-stu-id="13218-174">Will my reverse DNS records resolve from hello internet?</span></span>

<span data-ttu-id="13218-175">Igen.</span><span class="sxs-lookup"><span data-stu-id="13218-175">Yes.</span></span> <span data-ttu-id="13218-176">Ha elvégezte a hello címfeloldási DNS-tulajdonság az Azure Service, Azure kezeli az összes hello DNS-delegálás, és a DNS-zónák szükséges, hogy a DNS-rekord fordított tooensure megszünteti az összes internetes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="13218-176">Once you set hello reverse DNS property for your Azure service, Azure manages all hello DNS delegations and DNS zones required tooensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="13218-177">Alapértelmezett címfeloldási DNS-rekordok létrejönnek az az Azure-szolgáltatásokat?</span><span class="sxs-lookup"><span data-stu-id="13218-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="13218-178">Nem.</span><span class="sxs-lookup"><span data-stu-id="13218-178">No.</span></span> <span data-ttu-id="13218-179">Névkeresési DNS egyik választható szolgáltatása nem.</span><span class="sxs-lookup"><span data-stu-id="13218-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="13218-180">Nincs alapértelmezett címfeloldási DNS-rekordok jönnek létre, ha úgy dönt, hogy nem tooconfigure őket.</span><span class="sxs-lookup"><span data-stu-id="13218-180">No default reverse DNS records are created if you choose not tooconfigure them.</span></span>

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="13218-181">Mi az a hello formátumát hello teljesen minősített tartománynevét (FQDN)?</span><span class="sxs-lookup"><span data-stu-id="13218-181">What is hello format for hello fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="13218-182">Teljes tartománynevek előre sorrendben legyenek megadva, és egy pontot (például "app1.contoso.com.") kell végződnie.</span><span class="sxs-lookup"><span data-stu-id="13218-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a><span data-ttu-id="13218-183">Mi történik, ha hello hello érvényességi ellenőrzését I megadott DNS fordított sikertelen?</span><span class="sxs-lookup"><span data-stu-id="13218-183">What happens if hello validation check for hello reverse DNS I've specified fails?</span></span>

<span data-ttu-id="13218-184">Hello címfeloldási DNS-ellenőrzés sikertelen lesz, ahol hello művelet tooconfigure hello címfeloldási DNS-rekord nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="13218-184">Where hello reverse DNS validation check fails, hello operation tooconfigure hello reverse DNS record fails.</span></span> <span data-ttu-id="13218-185">Javítsa ki a hello címfeloldási DNS-értéket szükség szerint, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="13218-185">Correct hello reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="13218-186">Beállítható címfeloldási DNS az Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="13218-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="13218-187">Nem.</span><span class="sxs-lookup"><span data-stu-id="13218-187">No.</span></span> <span data-ttu-id="13218-188">Névkeresési DNS hello Azure App Service nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="13218-188">Reverse DNS is not supported for hello Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="13218-189">Beállítható több címfeloldási DNS-rekordok az Azure Service?</span><span class="sxs-lookup"><span data-stu-id="13218-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="13218-190">Nem.</span><span class="sxs-lookup"><span data-stu-id="13218-190">No.</span></span> <span data-ttu-id="13218-191">Azure egyetlen címfeloldási DNS-rekord támogatja az egyes Azure Cloud Service vagy nyilvános.</span><span class="sxs-lookup"><span data-stu-id="13218-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="13218-192">Konfigurálhatja a címfeloldási DNS IPv6 PublicIpAddress erőforrások?</span><span class="sxs-lookup"><span data-stu-id="13218-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="13218-193">Nem.</span><span class="sxs-lookup"><span data-stu-id="13218-193">No.</span></span> <span data-ttu-id="13218-194">Az Azure jelenleg támogatja a fordított DNS csak az IPv4-alapú PublicIpAddress erőforrások és a Felhőszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="13218-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a><span data-ttu-id="13218-195">Lehet küldeni e-mailek tooexternal tartományok a saját Azure számítási szolgáltatások?</span><span class="sxs-lookup"><span data-stu-id="13218-195">Can I send emails tooexternal domains from my Azure Compute services?</span></span>

<span data-ttu-id="13218-196">Nem.</span><span class="sxs-lookup"><span data-stu-id="13218-196">No.</span></span> [<span data-ttu-id="13218-197">Az Azure Compute szolgáltatások nem támogatják a küldő e-mailek tooexternal tartományok</span><span class="sxs-lookup"><span data-stu-id="13218-197">Azure Compute services do not support sending emails tooexternal domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="13218-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13218-198">Next steps</span></span>

<span data-ttu-id="13218-199">A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="13218-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="13218-200">Ismerje meg, hogyan túl[állomás hello névkeresési zónát az Internetszolgáltató által hozzárendelt IP-címtartományt, az Azure DNS-](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="13218-200">Learn how too[host hello reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>

