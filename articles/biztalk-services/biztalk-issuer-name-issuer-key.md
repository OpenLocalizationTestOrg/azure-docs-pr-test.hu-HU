---
title: "aaaIssuer nevét és a BizTalk szolgáltatások Issuer Key |} Microsoft Docs"
description: "Megtudhatja, hogyan tooretrieve Kiállítónevet és Issuer Key Service Bus vagy Access Control (ACS) a BizTalk szolgáltatások. MABS, WABS"
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
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a><span data-ttu-id="8c8cf-104">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="8c8cf-104">BizTalk Services: Issuer Name and Issuer Key</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="8c8cf-105">Az Azure BizTalk szolgáltatások hello Service Bus kibocsátó neve és Issuer Key és hello Access Control kibocsátó neve és Issuer Key használja.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-105">Azure BizTalk Services uses hello Service Bus Issuer Name and Issuer Key, and hello Access Control Issuer Name and Issuer Key.</span></span> <span data-ttu-id="8c8cf-106">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-106">Specifically:</span></span>

| <span data-ttu-id="8c8cf-107">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="8c8cf-107">Task</span></span> | <span data-ttu-id="8c8cf-108">Mely kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-108">Which Issuer Name and Issuer Key</span></span> |
| --- | --- |
| <span data-ttu-id="8c8cf-109">A Visual Studio az alkalmazás telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="8c8cf-109">Deploying your application from Visual Studio</span></span> |<span data-ttu-id="8c8cf-110">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-110">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="8c8cf-111">Hello Azure BizTalk szolgáltatások portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c8cf-111">Configuring hello Azure BizTalk Services Portal</span></span> |<span data-ttu-id="8c8cf-112">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-112">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="8c8cf-113">LOB-továbbítók hello BizTalk Adapter szolgáltatások a Visual Studio létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c8cf-113">Creating LOB Relays with hello BizTalk Adapter Services in Visual Studio</span></span> |<span data-ttu-id="8c8cf-114">Service Bus kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-114">Service Bus Issuer Name and Issuer Key</span></span> |

<span data-ttu-id="8c8cf-115">Ez a témakör a hello lépéseket tooretrieve hello kibocsátó neve és Issuer Key sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-115">This topic lists hello steps tooretrieve hello Issuer Name and Issuer Key.</span></span> 

## <a name="access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="8c8cf-116">Hozzáférés-vezérlő Kibocsátónév és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-116">Access Control Issuer Name and Issuer Key</span></span>
<span data-ttu-id="8c8cf-117">hello Access Control kibocsátó neve és Issuer Key hello következő használják:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-117">hello Access Control Issuer Name and Issuer Key are used by hello following:</span></span>

* <span data-ttu-id="8c8cf-118">Az Azure BizTalk szolgáltatás alkalmazás létrehozása a Visual Studio: toosuccessfully a Visual Studio tooAzure BizTalk szolgáltatás alkalmazást helyezi üzembe, adja meg a hozzáférés-vezérlő Kibocsátónév hello és Issuer Key.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-118">Your Azure BizTalk Service application created in Visual Studio: toosuccessfully deploy your BizTalk Service application in Visual Studio tooAzure, you enter hello Access Control Issuer Name and Issuer Key.</span></span> 
* <span data-ttu-id="8c8cf-119">hello Azure BizTalk szolgáltatások portálja: BizTalk szolgáltatás létrehozása, és nyissa meg a hello BizTalk Services portálra, a hozzáférés-vezérlési kibocsátó neve és Issuer Key automatikusan be vannak jegyezve a telepítések esetén a hello értékeitől hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-119">hello Azure BizTalk Services  Portal: When you create a BizTalk Service and open hello BizTalk Services Portal, your Access Control Issuer Name and Issuer Key are automatically registered for your deployments with hello same Access Control values.</span></span>

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="8c8cf-120">Első Access Control kibocsátó neve és Issuer Key hello</span><span class="sxs-lookup"><span data-stu-id="8c8cf-120">Get hello Access Control Issuer Name and Issuer Key</span></span>

<span data-ttu-id="8c8cf-121">a hitelesítés és a get toouse ACS hello kibocsátó neve és Issuer Key értékek, hello általános lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-121">toouse ACS for authentication, and get hello Issuer Name and Issuer Key values, hello overall steps include:</span></span>

1. <span data-ttu-id="8c8cf-122">Telepítse a hello [Azure Powershell-parancsmagok](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="8c8cf-122">Install hello [Azure Powershell cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
2. <span data-ttu-id="8c8cf-123">Adja hozzá az Azure-fiókjával:`Add-AzureAccount`</span><span class="sxs-lookup"><span data-stu-id="8c8cf-123">Add your Azure account: `Add-AzureAccount`</span></span>
3. <span data-ttu-id="8c8cf-124">Az előfizetés nevét adja vissza:`get-azuresubscription`</span><span class="sxs-lookup"><span data-stu-id="8c8cf-124">Return your subscription name: `get-azuresubscription`</span></span>
4. <span data-ttu-id="8c8cf-125">Jelölje ki az előfizetését:`select-azuresubscription <name of your subscription>`</span><span class="sxs-lookup"><span data-stu-id="8c8cf-125">Select your subscription: `select-azuresubscription <name of your subscription>`</span></span> 
5. <span data-ttu-id="8c8cf-126">Új névtér létrehozása:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="8c8cf-126">Create a new namespace: `new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>

    <span data-ttu-id="8c8cf-127">Példa:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="8c8cf-127">Example:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>
      
5. <span data-ttu-id="8c8cf-128">Hello új ACS névtér (amely több percet is igénybe vehet) jön létre, amikor hello kibocsátó neve és Issuer Key értékek listáját hello kapcsolati karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-128">When hello new ACS namespace is created (which can take several minutes), hello Issuer Name and Issuer Key values are listed in hello connection string:</span></span> 

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

<span data-ttu-id="8c8cf-129">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-129">toosummarize:</span></span>  
<span data-ttu-id="8c8cf-130">Kiállító neve = SharedSecretIssuer</span><span class="sxs-lookup"><span data-stu-id="8c8cf-130">Issuer Name = SharedSecretIssuer</span></span>  
<span data-ttu-id="8c8cf-131">Kiállító kulcsát = SharedSecretKey</span><span class="sxs-lookup"><span data-stu-id="8c8cf-131">Issuer Key = SharedSecretKey</span></span>

<span data-ttu-id="8c8cf-132">A hello további [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-132">More on hello [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span></span> 

## <a name="service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="8c8cf-133">Service Bus kibocsátó neve és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-133">Service Bus Issuer Name and Issuer Key</span></span>
<span data-ttu-id="8c8cf-134">Service Bus kibocsátó neve és Issuer Key BizTalk Adapter szolgáltatások használják.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-134">Service Bus Issuer Name and Issuer Key are used by BizTalk Adapter Services.</span></span> <span data-ttu-id="8c8cf-135">BizTalk szolgáltatások projektre a Visual Studio, a hello BizTalk Adapter szolgáltatások tooconnect tooan helyszíni üzletági (LOB) rendszert használ.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-135">In your BizTalk Services project in Visual Studio, you use hello BizTalk Adapter Services tooconnect tooan on-premises Line-of-Business (LOB) system.</span></span> <span data-ttu-id="8c8cf-136">tooconnect, hello LOB Relay létrehozásához, és adja meg a LOB-rendszer részleteit.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-136">tooconnect, you create hello LOB Relay and enter your LOB system details.</span></span> <span data-ttu-id="8c8cf-137">Ennek során is be hello Service Bus kibocsátó neve és Issuer Key.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-137">When doing this, you also enter hello Service Bus Issuer Name and Issuer Key.</span></span>

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="8c8cf-138">Service Bus Kibocsátónév tooretrieve hello és Issuer Key</span><span class="sxs-lookup"><span data-stu-id="8c8cf-138">tooretrieve hello Service Bus Issuer Name and Issuer Key</span></span>
1. <span data-ttu-id="8c8cf-139">Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="8c8cf-139">Sign in toohello [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="8c8cf-140">Hello bal oldali navigációs panelen, jelölje ki a **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-140">In hello left navigation pane, select **Service Bus**.</span></span>
3. <span data-ttu-id="8c8cf-141">Válassza ki a névteret.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-141">Select your namespace.</span></span> <span data-ttu-id="8c8cf-142">Hello tálcán, válassza ki a **kapcsolatadatok**.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-142">In hello task bar, select **Connection Information**.</span></span> <span data-ttu-id="8c8cf-143">Ez megjeleníti a hello **alapértelmezett kibocsátó** (kibocsátó neve) és **alapértelmezett kulcs** (Issuer Key).</span><span class="sxs-lookup"><span data-stu-id="8c8cf-143">This displays hello **Default Issuer** (Issuer Name) and **Default Key** (Issuer Key).</span></span> <span data-ttu-id="8c8cf-144">Az értékekre másolhatók.</span><span class="sxs-lookup"><span data-stu-id="8c8cf-144">Their values can be copied.</span></span>  

<span data-ttu-id="8c8cf-145">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-145">toosummarize:</span></span>  
<span data-ttu-id="8c8cf-146">Kiállító neve alapértelmezett kibocsátó =</span><span class="sxs-lookup"><span data-stu-id="8c8cf-146">Issuer Name = Default Issuer</span></span>  
<span data-ttu-id="8c8cf-147">Kiállító kulcsát alapértelmezett kulcs =</span><span class="sxs-lookup"><span data-stu-id="8c8cf-147">Issuer Key = Default Key</span></span>

## <a name="next"></a><span data-ttu-id="8c8cf-148">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="8c8cf-148">Next</span></span>
<span data-ttu-id="8c8cf-149">További Azure BizTalk szolgáltatások témakörök:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-149">Additional Azure BizTalk Services topics:</span></span>

* [<span data-ttu-id="8c8cf-150">Hello Azure BizTalk szolgáltatások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="8c8cf-150">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="8c8cf-151">Oktatóanyag: Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="8c8cf-151">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="8c8cf-152">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t</span><span class="sxs-lookup"><span data-stu-id="8c8cf-152">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="8c8cf-153">Az Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="8c8cf-153">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="8c8cf-154">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8c8cf-154">See Also</span></span>
* [<span data-ttu-id="8c8cf-155">Hogyan: használata ACS felügyeleti szolgáltatás tooConfigure szolgáltatás-identitások</span><span class="sxs-lookup"><span data-stu-id="8c8cf-155">How to: Use ACS Management Service tooConfigure Service Identities</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [<span data-ttu-id="8c8cf-156">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="8c8cf-156">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="8c8cf-157">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="8c8cf-157">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="8c8cf-158">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="8c8cf-158">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="8c8cf-159">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="8c8cf-159">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="8c8cf-160">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="8c8cf-160">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="8c8cf-161">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="8c8cf-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

