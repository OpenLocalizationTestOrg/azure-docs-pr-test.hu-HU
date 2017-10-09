---
title: "a webes alkalmazás aaaCreate egyéni DNS-rekordok |} Microsoft Docs"
description: "Hogyan toocreate egyéni tartomány DNS Azure DNS-sel webalkalmazás rögzíti."
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
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="def30-103">Egyéni tartomány DNS-rekordok webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="def30-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="def30-104">A webalkalmazások Azure DNS toohost egyéni tartományt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="def30-104">You can use Azure DNS toohost a custom domain for your web apps.</span></span> <span data-ttu-id="def30-105">Például egy Azure webalkalmazás létrehozásakor, és azt szeretné, hogy a felhasználók tooaccess azt által contoso.com, vagy a www.contoso.com teljes tartománynév használatával.</span><span class="sxs-lookup"><span data-stu-id="def30-105">For example, you are creating an Azure web app and you want your users tooaccess it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="def30-106">toodo, két bejegyzés toocreate van:</span><span class="sxs-lookup"><span data-stu-id="def30-106">toodo this, you have toocreate two records:</span></span>

* <span data-ttu-id="def30-107">Egy legfelső szintű "A" rekord mutató toocontoso.com</span><span class="sxs-lookup"><span data-stu-id="def30-107">A root "A" record pointing toocontoso.com</span></span>
* <span data-ttu-id="def30-108">Egy "CNAME" rekordja hello www nevét, amely toohello mutat egy rekord</span><span class="sxs-lookup"><span data-stu-id="def30-108">A "CNAME" record for hello www name that points toohello A record</span></span>

<span data-ttu-id="def30-109">Ne feledje, hogy az Azure-ban létrehoz egy A rekordot egy webalkalmazás, ha A rekord manuálisan frissíteni kell ha hello hello web app módosítások alapul szolgáló IP-cím hello.</span><span class="sxs-lookup"><span data-stu-id="def30-109">Keep in mind that if you create an A record for a web app in Azure, hello A record must be manually updated if hello underlying IP address for hello web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="def30-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="def30-110">Before you begin</span></span>

<span data-ttu-id="def30-111">Mielőtt elkezdené, először hozzon létre egy DNS-zóna Azure DNS-ben, és hello zóna delegálása a regisztráló tooAzure DNS a.</span><span class="sxs-lookup"><span data-stu-id="def30-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate hello zone in your registrar tooAzure DNS.</span></span>

1. <span data-ttu-id="def30-112">a DNS-zónák toocreate kövesse hello [hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="def30-112">toocreate a DNS zone, follow hello steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="def30-113">toodelegate a DNS-tooAzure DNS, kövesse hello [DNS-tartomány delegálás](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="def30-113">toodelegate your DNS tooAzure DNS, follow hello steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="def30-114">Zóna létrehozása és delegálása azt tooAzure DNS, után is létrehozhat rögzíti az egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="def30-114">After creating a zone and delegating it tooAzure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="def30-115">1. Az egyéni tartomány az A rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="def30-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="def30-116">Az A rekord használt toomap neve tooits IP-címnek.</span><span class="sxs-lookup"><span data-stu-id="def30-116">An A record is used toomap a name tooits IP address.</span></span> <span data-ttu-id="def30-117">Az alábbi példa hello azt fogja rendeljen egy A rekordot tooan IPv4-cím:</span><span class="sxs-lookup"><span data-stu-id="def30-117">In hello following example we will assign @ as an A record tooan IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="def30-118">1. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-118">Step 1</span></span>

<span data-ttu-id="def30-119">Az A rekord létrehozása és hozzárendelése tooa változó $rs</span><span class="sxs-lookup"><span data-stu-id="def30-119">Create an A record and assign tooa variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="def30-120">2. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-120">Step 2</span></span>

<span data-ttu-id="def30-121">Adja hozzá az IPv4-alapú hello érték korábban létrehozott toohello rekordhalmaz "@" hozzárendelt hello $rs változó segítségével.</span><span class="sxs-lookup"><span data-stu-id="def30-121">Add hello IPv4 value toohello previously created record set "@" using hello $rs variable assigned.</span></span> <span data-ttu-id="def30-122">hello hozzárendelt IPv4 érték lesz hello IP-címe a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="def30-122">hello IPv4 value assigned will be hello IP address for your web app.</span></span>

<span data-ttu-id="def30-123">toofind hello IP-cím egy webalkalmazás kövesse hello [egyéni tartománynév beállítása az Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="def30-123">toofind hello IP address for a web app, follow hello steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="def30-124">3. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-124">Step 3</span></span>

<span data-ttu-id="def30-125">Hello módosítások toohello rekordhalmaz véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="def30-125">Commit hello changes toohello record set.</span></span> <span data-ttu-id="def30-126">Használjon `Set-AzureRMDnsRecordSet` tooupload hello toohello rekordhalmaz tooAzure DNS módosítja:</span><span class="sxs-lookup"><span data-stu-id="def30-126">Use `Set-AzureRMDnsRecordSet` tooupload hello changes toohello record set tooAzure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="def30-127">2. Az egyéni tartomány CNAME rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="def30-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="def30-128">Ha a tartomány már az Azure DNS kezeli (lásd: [DNS-tartomány delegálás](dns-domain-delegation.md), hello példa toocreate contoso.azurewebsites.net CNAME rekord a következő hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="def30-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use hello following hello example toocreate a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="def30-129">1. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-129">Step 1</span></span>

<span data-ttu-id="def30-130">Nyissa meg a Powershellt, és hozzon létre egy új CNAME rekordkészlete, és rendelje hozzá a tooa változó $rs.</span><span class="sxs-lookup"><span data-stu-id="def30-130">Open PowerShell and create a new CNAME record set and assign tooa variable $rs.</span></span> <span data-ttu-id="def30-131">A példában a "idő toolive" 600 másodperc DNS-zónában "a contoso.com" nevű rekordkészlet típus CNAME hoz létre.</span><span class="sxs-lookup"><span data-stu-id="def30-131">This example will create a record set type CNAME with a "time toolive" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="def30-132">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="def30-132">hello following example is hello response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="def30-133">2. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-133">Step 2</span></span>

<span data-ttu-id="def30-134">Hello CNAME rekordhalmaz létrehozása után kell toocreate fog mutatni toohello web app alias értéke.</span><span class="sxs-lookup"><span data-stu-id="def30-134">Once hello CNAME record set is created, you need toocreate an alias value which will point toohello web app.</span></span>

<span data-ttu-id="def30-135">A korábban hozzárendelt változó "$rs" hello használatával toocreate hello alias hello PowerShell-parancsot a hello web app contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="def30-135">Using hello previously assigned variable "$rs" you can use hello PowerShell command below toocreate hello alias for hello web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="def30-136">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="def30-136">hello following example is hello response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="def30-137">3. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-137">Step 3</span></span>

<span data-ttu-id="def30-138">Hello segítségével hello változtatások véglegesítése a határidő `Set-AzureRMDnsRecordSet` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="def30-138">Commit hello changes using hello `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="def30-139">Ellenőrizheti hello rekord megfelelően lett létrehozva hello "www.contoso.com" nslookup, használatával lekérdezésével alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="def30-139">You can validate hello record was created correctly by querying hello "www.contoso.com" using nslookup, as shown below:</span></span>

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

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="def30-140">A webalkalmazások egy "awverify" rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="def30-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="def30-141">Ha úgy dönt, az A rekord toouse a webalkalmazás, haladjon végig a hitelesítési folyamat tooensure meg saját hello egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="def30-141">If you decide toouse an A record for your web app, you must go through a verification process tooensure you own hello custom domain.</span></span> <span data-ttu-id="def30-142">Az ellenőrzési lépés "awverify" nevű különleges CNAME rekord létrehozásával történik.</span><span class="sxs-lookup"><span data-stu-id="def30-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="def30-143">Ez a szakasz csak a tooA rekordok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="def30-143">This section applies tooA records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="def30-144">1. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-144">Step 1</span></span>

<span data-ttu-id="def30-145">Hello "awverify" rekordot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="def30-145">Create hello "awverify" record.</span></span> <span data-ttu-id="def30-146">Hello az alábbi példában szereplő létrehozunk hello "aweverify" rekord contoso.com tooverify tulajdonosát hello mutató egyéni tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="def30-146">In hello example below, we will create hello "aweverify" record for contoso.com tooverify ownership for hello custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="def30-147">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="def30-147">hello following example is hello response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="def30-148">2. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-148">Step 2</span></span>

<span data-ttu-id="def30-149">"Awverify" hello rekordhalmaz létrehozása után rendelje hozzá a hello CNAME-rekordhalmazt alias.</span><span class="sxs-lookup"><span data-stu-id="def30-149">Once hello record set "awverify" is created, assign hello CNAME record set alias.</span></span> <span data-ttu-id="def30-150">Hello az alábbi példában a Microsoft hello CNAMe rekordkészlete alias tooawverify.contoso.azurewebsites.net rendeli.</span><span class="sxs-lookup"><span data-stu-id="def30-150">In hello example below, we will assign hello CNAMe record set alias tooawverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="def30-151">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="def30-151">hello following example is hello response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="def30-152">3. lépés</span><span class="sxs-lookup"><span data-stu-id="def30-152">Step 3</span></span>

<span data-ttu-id="def30-153">Hello segítségével hello változtatások véglegesítése a határidő `Set-AzureRMDnsRecordSet cmdlet`, ahogy az alábbi hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="def30-153">Commit hello changes using hello `Set-AzureRMDnsRecordSet cmdlet`, as shown in hello command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="def30-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="def30-154">Next steps</span></span>

<span data-ttu-id="def30-155">Hello kövesse [beállítása egy egyéni tartománynevet, az App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure a webes alkalmazás toouse egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="def30-155">Follow hello steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure your web app toouse a custom domain.</span></span>
