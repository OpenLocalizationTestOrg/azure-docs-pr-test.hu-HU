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
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Az Azure-ban tárolt szolgáltatások címfeloldási DNS konfigurálása

Ez a cikk azt ismerteti, hogyan tooconfigure névkeresési DNS-keresések az Azure-ban tárolt szolgáltatások.

Az Azure szolgáltatások IP-címek hozzárendelése az Azure-ban, és a Microsoft tulajdonában használják. A címfeloldási DNS-rekordok (PTR-rekordok) hello megfelelő Microsoft által birtokolt névkeresési DNS-címkeresési zónák kell létrehozni. Ez a cikk azt ismerteti, hogyan toodo ez.

Ez a forgatókönyv nem tévesztendők össze a hello képességét túl[hello névkeresési DNS-címkeresési zónák a hozzárendelt IP-címtartományokhoz az Azure DNS-állomás](dns-reverse-dns-hosting.md). Ebben az esetben hello IP-címtartományok hello névkeresési zóna által képviselt kell rendelni tooyour a szervezeten belül, általában az Internetszolgáltató által.

A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).

Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).
* Hello Resource Manager üzembe helyezési modellel számítási erőforrások (például virtuális gépek, a virtuálisgép-méretezési csoportok vagy a Service Fabric-fürtök) PublicIpAddress erőforrás keresztül érhetők el. DNS-névlekérdezés hello "ReverseFqdn" hello PublicIpAddress tulajdonsága használatával vannak konfigurálva.
* Hello klasszikus üzembe helyezési modellel a számítási erőforrások elérhetők felhőalapú szolgáltatások segítségével. DNS-névlekérdezés hello Felhőszolgáltatás hello "ReverseDnsFqdn" tulajdonságának használatával vannak konfigurálva.

Névkeresési DNS jelenleg nem támogatott a hello Azure App Service szolgáltatásban.

## <a name="validation-of-reverse-dns-records"></a>A címfeloldási DNS-rekordok érvényesítése

A harmadik fél nem lehet képes toocreate címfeloldási DNS-rekordok az Azure-szolgáltatások leképezési tooyour DNS-tartományok. tooprevent, Azure csak lehetővé teszi a hello címfeloldási DNS-rekord megadott tartománynév esetén az azonos hello, vagy megegyezik a hello szolgáltatás oldja fel a rendszer, DNS-nevét vagy IP-címét egy nyilvános vagy felhő hello címfeloldási DNS-rekord létrehozása hello Azure-előfizetés.

Az ellenőrzés csak akkor hajtható végre, ha hello címfeloldási DNS-rekord beállítani vagy módosítani. Rendszeres ismételt érvényesítése nem megy végbe.

Például: Tegyük fel, hogy hello PublicIpAddress erőforrás hello DNS-név contosoapp1.northus.cloudapp.azure.com és IP-cím 23.96.52.53 rendelkezik. ReverseFqdn hello hello PublicIpAddress adhatók meg:
* hello PublicIpAddress hello DNS-nevét, contosoapp1.northus.cloudapp.azure.com
* hello DNS-beli név olyan hello a különböző nyilvános az azonos előfizetést, például az contosoapp2.westus.cloudapp.azure.com
* A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurált, egy CNAME toocontosoapp1.northus.cloudapp.azure.com vagy tooa különböző PublicIpAddress hello ugyanahhoz az előfizetéshez.
* A kreatív DNS-beli név, például a app1.contoso.com, mindaddig, amíg ez a név *első* konfigurált IP-címet A rekord toohello 23.96.52.53, vagy egy másik nyilvános IP-címe toohello hello ugyanahhoz az előfizetéshez.

hello azonos megkötések alkalmazása tooreverse DNS-szolgáltatásokhoz.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Névkeresési DNS PublicIpAddress erőforrások

Ez a témakör részletes útmutatást hogyan tooconfigure fordított PublicIpAddress erőforrások hello Resource Manager üzembe helyezési modellel, az Azure PowerShell, Azure CLI 1.0 vagy Azure CLI 2.0 DNS. Nyilvános erőforrások címfeloldási DNS konfigurálása jelenleg nem támogatott hello Azure-portálon keresztül.

Az Azure jelenleg támogatja a fordított DNS kizárólag a nyilvános IPv4-erőforrások. Az IPv6 nem támogatott.

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a>Meglévő PublicIpAddresses címfeloldási DNS tooan hozzáadása

#### <a name="powershell"></a>PowerShell

tooadd címfeloldási DNS tooan meglévő PublicIpAddress:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

tooadd címfeloldási DNS tooan meglévő PublicIpAddress:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

tooadd címfeloldási DNS tooan meglévő PublicIpAddress:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

tooadd fordított DNS tooan meglévő PublicIpAddress, amely még nem rendelkezik a DNS-név, meg kell adnia a DNS-név:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Hozzon létre egy nyilvános IP-cím a címfeloldási DNS-ben

új toocreate hello nyilvános névkeresési DNS tulajdonság már meg van adva:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Egy meglévő nyilvános címfeloldási DNS megtekintése

tooview hello értéket egy meglévő nyilvános konfigurálva:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Távolítsa el a címfeloldási DNS a meglévő nyilvános IP-címek

a címfeloldási DNS-tulajdonságot egy meglévő nyilvános tooremove:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Névkeresési DNS konfigurálása a Cloud Services csomag

Ez a témakör részletes útmutatást hogyan tooconfigure névkeresési DNS-szolgáltatásokhoz hello klasszikus üzembe helyezési modellel, az Azure PowerShell. Névkeresési DNS konfigurálása a Cloud Services Azure CLI 1.0-s vagy Azure CLI 2.0 hello Azure-portálon keresztül nem támogatott.

### <a name="add-reverse-dns-tooexisting-cloud-services"></a>Adja hozzá a címfeloldási DNS tooexisting Cloud Services

létező felhőalapú szolgáltatás, címfeloldási DNS rekord tooan tooadd:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>A címfeloldási DNS-felhőalapú szolgáltatás létrehozása

toocreate hello címfeloldási DNS a tulajdonság már meg van adva egy új Felhőszolgáltatás:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Nézet névkeresési DNS meglévő Cloud Services csomag

tooview hello címfeloldási DNS tulajdonsága egy meglévő felhőalapú szolgáltatást:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Távolítsa el a címfeloldási DNS a meglévő Cloud Services csomag

a fordított DNS-tulajdonságot egy meglévő Felhőszolgáltatáshoz tooremove:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>GYIK

### <a name="how-much-do-reverse-dns-records-cost"></a>Milyen mértékű névkeresési DNS-rekordok költség?

Szabad fontosságúak!  Nincs a címfeloldási DNS-rekordok vagy lekérdezések további költség nélkül.

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a>A címfeloldási DNS rekordok hárítsa el a hello fog internet?

Igen. Ha elvégezte a hello címfeloldási DNS-tulajdonság az Azure Service, Azure kezeli az összes hello DNS-delegálás, és a DNS-zónák szükséges, hogy a DNS-rekord fordított tooensure megszünteti az összes internetes felhasználó számára.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Alapértelmezett címfeloldási DNS-rekordok létrejönnek az az Azure-szolgáltatásokat?

Nem. Névkeresési DNS egyik választható szolgáltatása nem. Nincs alapértelmezett címfeloldási DNS-rekordok jönnek létre, ha úgy dönt, hogy nem tooconfigure őket.

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a>Mi az a hello formátumát hello teljesen minősített tartománynevét (FQDN)?

Teljes tartománynevek előre sorrendben legyenek megadva, és egy pontot (például "app1.contoso.com.") kell végződnie.

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a>Mi történik, ha hello hello érvényességi ellenőrzését I megadott DNS fordított sikertelen?

Hello címfeloldási DNS-ellenőrzés sikertelen lesz, ahol hello művelet tooconfigure hello címfeloldási DNS-rekord nem sikerül. Javítsa ki a hello címfeloldási DNS-értéket szükség szerint, és próbálkozzon újra.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Beállítható címfeloldási DNS az Azure App Service?

Nem. Névkeresési DNS hello Azure App Service nem támogatott.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Beállítható több címfeloldási DNS-rekordok az Azure Service?

Nem. Azure egyetlen címfeloldási DNS-rekord támogatja az egyes Azure Cloud Service vagy nyilvános.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Konfigurálhatja a címfeloldási DNS IPv6 PublicIpAddress erőforrások?

Nem. Az Azure jelenleg támogatja a fordított DNS csak az IPv4-alapú PublicIpAddress erőforrások és a Felhőszolgáltatások.

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a>Lehet küldeni e-mailek tooexternal tartományok a saját Azure számítási szolgáltatások?

Nem. [Az Azure Compute szolgáltatások nem támogatják a küldő e-mailek tooexternal tartományok](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Következő lépések

A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Ismerje meg, hogyan túl[állomás hello névkeresési zónát az Internetszolgáltató által hozzárendelt IP-címtartományt, az Azure DNS-](dns-reverse-dns-for-azure-services.md).

