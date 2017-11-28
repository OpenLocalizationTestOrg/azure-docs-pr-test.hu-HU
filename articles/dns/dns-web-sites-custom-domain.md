---
title: "A webes alkalmazás egyéni DNS-rekordok létrehozása |} Microsoft Docs"
description: "Hogyan hozhat létre egyéni tartományt a webalkalmazás Azure DNS használatával DNS-rekordokat."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: b054a41ecd69ee1c802d8403fe4b25128f016e3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="3133f-103">Egyéni tartomány DNS-rekordok webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3133f-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="3133f-104">Azure DNS az egyéni tartománynév üzemeltetéséhez a web Apps használatával.</span><span class="sxs-lookup"><span data-stu-id="3133f-104">You can use Azure DNS to host a custom domain for your web apps.</span></span> <span data-ttu-id="3133f-105">Azure-webalkalmazás létrehozásakor és azt szeretné, hogy a felhasználók által eléréséhez például contoso.com, vagy a www.contoso.com teljes tartománynév használatával.</span><span class="sxs-lookup"><span data-stu-id="3133f-105">For example, you are creating an Azure web app and you want your users to access it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="3133f-106">Ehhez hozzon létre két rekordot rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3133f-106">To do this, you have to create two records:</span></span>

* <span data-ttu-id="3133f-107">Válassza a contoso.com gyökérrekordként "A"</span><span class="sxs-lookup"><span data-stu-id="3133f-107">A root "A" record pointing to contoso.com</span></span>
* <span data-ttu-id="3133f-108">A www-név, amely A-rekordra mutat "CNAME" rekord</span><span class="sxs-lookup"><span data-stu-id="3133f-108">A "CNAME" record for the www name that points to the A record</span></span>

<span data-ttu-id="3133f-109">Ne feledje, hogy az A rekord egy webalkalmazás létrehozásakor az Azure-ban, az A rekordot kell manuálisan frissíteni, ha az alapul szolgáló IP-cím a webes alkalmazás módosítások.</span><span class="sxs-lookup"><span data-stu-id="3133f-109">Keep in mind that if you create an A record for a web app in Azure, the A record must be manually updated if the underlying IP address for the web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3133f-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="3133f-110">Before you begin</span></span>

<span data-ttu-id="3133f-111">Mielőtt elkezdené, először hozzon létre egy DNS-zónát az Azure DNS-, és a zóna delegálása az Azure DNS a tartományregisztráló.</span><span class="sxs-lookup"><span data-stu-id="3133f-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate the zone in your registrar to Azure DNS.</span></span>

1. <span data-ttu-id="3133f-112">Hozzon létre egy DNS-zónát, kövesse a lépéseket a [hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="3133f-112">To create a DNS zone, follow the steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="3133f-113">A DNS, az Azure DNS-delegálását, kövesse a lépéseket a [DNS-tartomány delegálás](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="3133f-113">To delegate your DNS to Azure DNS, follow the steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="3133f-114">Zóna létrehozása, és azt az Azure DNS-delegálás, után is létrehozhat rögzíti az egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="3133f-114">After creating a zone and delegating it to Azure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="3133f-115">1. Az egyéni tartomány az A rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="3133f-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="3133f-116">Az A rekord segítségével hozzárendelhet egy nevet az IP-címét.</span><span class="sxs-lookup"><span data-stu-id="3133f-116">An A record is used to map a name to its IP address.</span></span> <span data-ttu-id="3133f-117">Az alábbi példa azt fogja rendeljen egy A rekordot egy IPv4-címére:</span><span class="sxs-lookup"><span data-stu-id="3133f-117">In the following example we will assign @ as an A record to an IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="3133f-118">1. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-118">Step 1</span></span>

<span data-ttu-id="3133f-119">Az A rekord létrehozása és hozzárendelése egy változó $rs</span><span class="sxs-lookup"><span data-stu-id="3133f-119">Create an A record and assign to a variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="3133f-120">2. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-120">Step 2</span></span>

<span data-ttu-id="3133f-121">Az IPv4-alapú értékkel adja hozzá a korábban létrehozott rekordkészlet "@" a hozzárendelt $rs változó segítségével.</span><span class="sxs-lookup"><span data-stu-id="3133f-121">Add the IPv4 value to the previously created record set "@" using the $rs variable assigned.</span></span> <span data-ttu-id="3133f-122">Hozzárendelt IPv4 érték lesz a webalkalmazáshoz tartozó IP-címét.</span><span class="sxs-lookup"><span data-stu-id="3133f-122">The IPv4 value assigned will be the IP address for your web app.</span></span>

<span data-ttu-id="3133f-123">Az IP-cím keresése a webes alkalmazás, kövesse a lépéseket a [egyéni tartománynév beállítása az Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="3133f-123">To find the IP address for a web app, follow the steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="3133f-124">3. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-124">Step 3</span></span>

<span data-ttu-id="3133f-125">A változtatások véglegesítése a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="3133f-125">Commit the changes to the record set.</span></span> <span data-ttu-id="3133f-126">Használjon `Set-AzureRMDnsRecordSet` feltölteni az Azure DNS-rekordot a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="3133f-126">Use `Set-AzureRMDnsRecordSet` to upload the changes to the record set to Azure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="3133f-127">2. Az egyéni tartomány CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="3133f-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="3133f-128">Ha a tartomány már az Azure DNS kezeli (lásd: [DNS-tartomány delegálás](dns-domain-delegation.md), az alábbi contoso.azurewebsites.net CNAME rekord létrehozása a példát.</span><span class="sxs-lookup"><span data-stu-id="3133f-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use the following the example to create a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="3133f-129">1. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-129">Step 1</span></span>

<span data-ttu-id="3133f-130">Nyissa meg a Powershellt, és hozzon létre egy új CNAME rekordkészlete, és rendelje hozzá egy változó $rs.</span><span class="sxs-lookup"><span data-stu-id="3133f-130">Open PowerShell and create a new CNAME record set and assign to a variable $rs.</span></span> <span data-ttu-id="3133f-131">Ebben a példában létrehoz egy "élő idő" rekordhalmaz típus CNAME 600 másodperc neve "contoso.com" DNS-zónában.</span><span class="sxs-lookup"><span data-stu-id="3133f-131">This example will create a record set type CNAME with a "time to live" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="3133f-132">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="3133f-132">The following example is the response.</span></span>

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="3133f-133">2. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-133">Step 2</span></span>

<span data-ttu-id="3133f-134">A CNAME-rekordhalmaz létrehozása után szeretne létrehozni a webalkalmazást mutasson alias értéke.</span><span class="sxs-lookup"><span data-stu-id="3133f-134">Once the CNAME record set is created, you need to create an alias value which will point to the web app.</span></span>

<span data-ttu-id="3133f-135">A korábban hozzárendelt változó "$rs" használatával az alábbi PowerShell-parancsot a webes alkalmazás contoso.azurewebsites.net aliasa létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3133f-135">Using the previously assigned variable "$rs" you can use the PowerShell command below to create the alias for the web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="3133f-136">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="3133f-136">The following example is the response.</span></span>

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="3133f-137">3. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-137">Step 3</span></span>

<span data-ttu-id="3133f-138">Véglegesítse a módosításokat a `Set-AzureRMDnsRecordSet` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="3133f-138">Commit the changes using the `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="3133f-139">Ellenőrizheti, hogy a rekord megfelelően lett létrehozva a "www.contoso.com" nslookup, használatával lekérdezésével alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="3133f-139">You can validate the record was created correctly by querying the "www.contoso.com" using nslookup, as shown below:</span></span>

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="3133f-140">A webalkalmazások egy "awverify" rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="3133f-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="3133f-141">Ha úgy dönt, hogy az A rekord használja a webalkalmazás, haladjon végig a annak érdekében, hogy Ön a tulajdonosa az egyéni tartomány-ellenőrzési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="3133f-141">If you decide to use an A record for your web app, you must go through a verification process to ensure you own the custom domain.</span></span> <span data-ttu-id="3133f-142">Az ellenőrzési lépés "awverify" nevű különleges CNAME rekord létrehozásával történik.</span><span class="sxs-lookup"><span data-stu-id="3133f-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="3133f-143">Ez a szakasz csak egy rekordok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3133f-143">This section applies to A records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="3133f-144">1. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-144">Step 1</span></span>

<span data-ttu-id="3133f-145">A "awverify" rekordot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3133f-145">Create the "awverify" record.</span></span> <span data-ttu-id="3133f-146">Az alábbi példában létre fogunk hozni a "aweverify" rekord contoso.com a tulajdonjogát az egyéni tartomány ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="3133f-146">In the example below, we will create the "aweverify" record for contoso.com to verify ownership for the custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="3133f-147">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="3133f-147">The following example is the response.</span></span>

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="3133f-148">2. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-148">Step 2</span></span>

<span data-ttu-id="3133f-149">A "awverify" rekordhalmaz létrehozása után rendelje hozzá a CNAME-rekordhalmazt alias.</span><span class="sxs-lookup"><span data-stu-id="3133f-149">Once the record set "awverify" is created, assign the CNAME record set alias.</span></span> <span data-ttu-id="3133f-150">Az alábbi példa azt a CNAMe rekord awverify.contoso.azurewebsites.net alias beállítása rendeli.</span><span class="sxs-lookup"><span data-stu-id="3133f-150">In the example below, we will assign the CNAMe record set alias to awverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="3133f-151">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="3133f-151">The following example is the response.</span></span>

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="3133f-152">3. lépés</span><span class="sxs-lookup"><span data-stu-id="3133f-152">Step 3</span></span>

<span data-ttu-id="3133f-153">Véglegesítse a módosításokat a `Set-AzureRMDnsRecordSet cmdlet`, ahogy az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="3133f-153">Commit the changes using the `Set-AzureRMDnsRecordSet cmdlet`, as shown in the command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="3133f-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3133f-154">Next steps</span></span>

<span data-ttu-id="3133f-155">Kövesse a [beállítása egy egyéni tartománynevet, az App Service](../app-service-web/web-sites-custom-domain-name.md) egyéni tartományt webalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3133f-155">Follow the steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) to configure your web app to use a custom domain.</span></span>
