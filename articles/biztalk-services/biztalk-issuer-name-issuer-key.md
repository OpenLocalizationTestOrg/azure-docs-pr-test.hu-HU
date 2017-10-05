---
title: "Kiállító neve és a BizTalk szolgáltatások Issuer Key |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet lekérni a kibocsátó neve és Issuer Key Service Bus vagy Access Control (ACS) a BizTalk szolgáltatások. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: b9fd985c23558596408e78eadae00dd0f95c4214
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a><span data-ttu-id="e33f6-104">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="e33f6-104">BizTalk Services: Issuer Name and Issuer Key</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="e33f6-105">Azure BizTalk-szolgáltatásokat használja, a Service Bus kibocsátó neve és Issuer Key, és a hozzáférés-vezérlő Kibocsátónév és Issuer Key.</span><span class="sxs-lookup"><span data-stu-id="e33f6-105">Azure BizTalk Services uses the Service Bus Issuer Name and Issuer Key, and the Access Control Issuer Name and Issuer Key.</span></span> <span data-ttu-id="e33f6-106">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="e33f6-106">Specifically:</span></span>

| <span data-ttu-id="e33f6-107">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="e33f6-107">Task</span></span> | <span data-ttu-id="e33f6-108">Mely kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-108">Which Issuer Name and Issuer Key</span></span> |
| --- | --- |
| <span data-ttu-id="e33f6-109">A Visual Studio az alkalmazás telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="e33f6-109">Deploying your application from Visual Studio</span></span> |<span data-ttu-id="e33f6-110">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-110">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="e33f6-111">Az Azure BizTalk szolgáltatások portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e33f6-111">Configuring the Azure BizTalk Services Portal</span></span> |<span data-ttu-id="e33f6-112">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-112">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="e33f6-113">LOB-továbbítók létrehozása a Visual Studio BizTalk Adapter szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="e33f6-113">Creating LOB Relays with the BizTalk Adapter Services in Visual Studio</span></span> |<span data-ttu-id="e33f6-114">Service Bus kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-114">Service Bus Issuer Name and Issuer Key</span></span> |

<span data-ttu-id="e33f6-115">Ez a témakör a kibocsátó neve és Issuer Key beolvasandó lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="e33f6-115">This topic lists the steps to retrieve the Issuer Name and Issuer Key.</span></span> 

## <a name="access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="e33f6-116">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-116">Access Control Issuer Name and Issuer Key</span></span>
<span data-ttu-id="e33f6-117">A hozzáférés-vezérlő Kibocsátónév és Issuer Key használnak a következő:</span><span class="sxs-lookup"><span data-stu-id="e33f6-117">The Access Control Issuer Name and Issuer Key are used by the following:</span></span>

* <span data-ttu-id="e33f6-118">Az Azure BizTalk szolgáltatás alkalmazás létrehozása a Visual Studio: sikeres telepítéséhez a BizTalk szolgáltatás alkalmazást a Visual Studio Azure Access Control kibocsátó neve és Issuer Key adja meg.</span><span class="sxs-lookup"><span data-stu-id="e33f6-118">Your Azure BizTalk Service application created in Visual Studio: To successfully deploy your BizTalk Service application in Visual Studio to Azure, you enter the Access Control Issuer Name and Issuer Key.</span></span> 
* <span data-ttu-id="e33f6-119">Az Azure BizTalk szolgáltatások portálon: BizTalk szolgáltatás létrehozása, és nyissa meg a BizTalk szolgáltatások portált, a hozzáférés-vezérlő Kibocsátónév és Issuer Key automatikusan regisztrálva vannak a hozzáférés-vezérlés értékeitől telepítések esetén.</span><span class="sxs-lookup"><span data-stu-id="e33f6-119">The Azure BizTalk Services  Portal: When you create a BizTalk Service and open the BizTalk Services Portal, your Access Control Issuer Name and Issuer Key are automatically registered for your deployments with the same Access Control values.</span></span>

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="e33f6-120">A hozzáférés-vezérlő kibocsátó neve és a kiállító kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="e33f6-120">Get the Access Control Issuer Name and Issuer Key</span></span>

<span data-ttu-id="e33f6-121">ACS használata a hitelesítéshez, és a kiállító nevével és Issuer Key értékek, az általános lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="e33f6-121">To use ACS for authentication, and get the Issuer Name and Issuer Key values, the overall steps include:</span></span>

1. <span data-ttu-id="e33f6-122">Telepítse a [Azure Powershell-parancsmagok](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="e33f6-122">Install the [Azure Powershell cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
2. <span data-ttu-id="e33f6-123">Adja hozzá az Azure-fiókjával:`Add-AzureAccount`</span><span class="sxs-lookup"><span data-stu-id="e33f6-123">Add your Azure account: `Add-AzureAccount`</span></span>
3. <span data-ttu-id="e33f6-124">Az előfizetés nevét adja vissza:`get-azuresubscription`</span><span class="sxs-lookup"><span data-stu-id="e33f6-124">Return your subscription name: `get-azuresubscription`</span></span>
4. <span data-ttu-id="e33f6-125">Jelölje ki az előfizetését:`select-azuresubscription <name of your subscription>`</span><span class="sxs-lookup"><span data-stu-id="e33f6-125">Select your subscription: `select-azuresubscription <name of your subscription>`</span></span> 
5. <span data-ttu-id="e33f6-126">Új névtér létrehozása:`new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="e33f6-126">Create a new namespace: `new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>

    <span data-ttu-id="e33f6-127">Példa:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="e33f6-127">Example:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>
      
5. <span data-ttu-id="e33f6-128">Létrehozásakor az új ACS-névtér (amely több percet is igénybe vehet), a kiállító nevével és Issuer Key értékek listáját a kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="e33f6-128">When the new ACS namespace is created (which can take several minutes), the Issuer Name and Issuer Key values are listed in the connection string:</span></span> 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

<span data-ttu-id="e33f6-129">Összefoglalásképpen:</span><span class="sxs-lookup"><span data-stu-id="e33f6-129">To summarize:</span></span>  
<span data-ttu-id="e33f6-130">Kiállító neve = SharedSecretIssuer</span><span class="sxs-lookup"><span data-stu-id="e33f6-130">Issuer Name = SharedSecretIssuer</span></span>  
<span data-ttu-id="e33f6-131">Kiállító kulcsát = SharedSecretKey</span><span class="sxs-lookup"><span data-stu-id="e33f6-131">Issuer Key = SharedSecretKey</span></span>

<span data-ttu-id="e33f6-132">A több a [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e33f6-132">More on the [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span></span> 

## <a name="service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="e33f6-133">Service Bus kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="e33f6-133">Service Bus Issuer Name and Issuer Key</span></span>
<span data-ttu-id="e33f6-134">Service Bus kibocsátó neve és Issuer Key BizTalk Adapter szolgáltatások használják.</span><span class="sxs-lookup"><span data-stu-id="e33f6-134">Service Bus Issuer Name and Issuer Key are used by BizTalk Adapter Services.</span></span> <span data-ttu-id="e33f6-135">BizTalk szolgáltatások projektre a Visual Studio, a segítségével a BizTalk Adapter szolgáltatások egy helyszíni üzletági (LOB) rendszerhez való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="e33f6-135">In your BizTalk Services project in Visual Studio, you use the BizTalk Adapter Services to connect to an on-premises Line-of-Business (LOB) system.</span></span> <span data-ttu-id="e33f6-136">Szeretne csatlakozni, a LOB-továbbítási létrehozása, és adja meg a LOB-rendszer részleteit.</span><span class="sxs-lookup"><span data-stu-id="e33f6-136">To connect, you create the LOB Relay and enter your LOB system details.</span></span> <span data-ttu-id="e33f6-137">Ennek során azt is adja meg a Service Bus kibocsátó neve és Issuer Key.</span><span class="sxs-lookup"><span data-stu-id="e33f6-137">When doing this, you also enter the Service Bus Issuer Name and Issuer Key.</span></span>

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="e33f6-138">A Service Bus kibocsátó neve és Issuer Key beolvasása</span><span class="sxs-lookup"><span data-stu-id="e33f6-138">To retrieve the Service Bus Issuer Name and Issuer Key</span></span>
1. <span data-ttu-id="e33f6-139">Jelentkezzen be a [klasszikus Azure portálra](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="e33f6-139">Sign in to the [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="e33f6-140">A bal oldali navigációs ablakból válassza **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="e33f6-140">In the left navigation pane, select **Service Bus**.</span></span>
3. <span data-ttu-id="e33f6-141">Válassza ki a névteret.</span><span class="sxs-lookup"><span data-stu-id="e33f6-141">Select your namespace.</span></span> <span data-ttu-id="e33f6-142">A tálcán válassza **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="e33f6-142">In the task bar, select **Connection Information**.</span></span> <span data-ttu-id="e33f6-143">Ez megjeleníti a **alapértelmezett kibocsátó** (kibocsátó neve) és **alapértelmezett kulcs** (Issuer Key).</span><span class="sxs-lookup"><span data-stu-id="e33f6-143">This displays the **Default Issuer** (Issuer Name) and **Default Key** (Issuer Key).</span></span> <span data-ttu-id="e33f6-144">Az értékekre másolhatók.</span><span class="sxs-lookup"><span data-stu-id="e33f6-144">Their values can be copied.</span></span>  

<span data-ttu-id="e33f6-145">Összefoglalásképpen:</span><span class="sxs-lookup"><span data-stu-id="e33f6-145">To summarize:</span></span>  
<span data-ttu-id="e33f6-146">Kiállító neve alapértelmezett kibocsátó =</span><span class="sxs-lookup"><span data-stu-id="e33f6-146">Issuer Name = Default Issuer</span></span>  
<span data-ttu-id="e33f6-147">Kiállító kulcsát alapértelmezett kulcs =</span><span class="sxs-lookup"><span data-stu-id="e33f6-147">Issuer Key = Default Key</span></span>

## <a name="next"></a><span data-ttu-id="e33f6-148">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="e33f6-148">Next</span></span>
<span data-ttu-id="e33f6-149">További Azure BizTalk szolgáltatások témakörök:</span><span class="sxs-lookup"><span data-stu-id="e33f6-149">Additional Azure BizTalk Services topics:</span></span>

* [<span data-ttu-id="e33f6-150">Az Azure BizTalk szolgáltatások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="e33f6-150">Installing the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="e33f6-151">Oktatóanyag: Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e33f6-151">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="e33f6-152">Hogyan kezdhetem el az Azure BizTalk Services SDK használatát</span><span class="sxs-lookup"><span data-stu-id="e33f6-152">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="e33f6-153">Az Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e33f6-153">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="e33f6-154">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e33f6-154">See Also</span></span>
* [<span data-ttu-id="e33f6-155">Hogyan: ACS felügyeleti szolgáltatás segítségével a szolgáltatás-identitások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e33f6-155">How to: Use ACS Management Service to Configure Service Identities</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [<span data-ttu-id="e33f6-156">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="e33f6-156">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="e33f6-157">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="e33f6-157">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="e33f6-158">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="e33f6-158">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="e33f6-159">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="e33f6-159">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="e33f6-160">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="e33f6-160">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="e33f6-161">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="e33f6-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

