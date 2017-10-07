---
title: "App Service Environment-környezetek aaaCustom beállításai"
description: "App Service Environment-környezetek egyéni konfigurációs beállításai"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 3d140688c88b389e71bfdd465c418339cccab3a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a><span data-ttu-id="7e19c-103">App Service Environment-környezetek egyéni konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="7e19c-103">Custom configuration settings for App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="7e19c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7e19c-104">Overview</span></span>
<span data-ttu-id="7e19c-105">Mivel az App Service Environment-környezetek elkülönített tooa egyetlen felhasználói, vannak bizonyos használható konfigurációs beállítások alkalmazása kizárólag tooApp környezetek.</span><span class="sxs-lookup"><span data-stu-id="7e19c-105">Because App Service Environments are isolated tooa single customer, there are certain configuration settings that can be applied exclusively tooApp Service Environments.</span></span> <span data-ttu-id="7e19c-106">Ez a cikk dokumentumok hello App Service Environment-környezetek számára elérhető különböző testreszabásokat.</span><span class="sxs-lookup"><span data-stu-id="7e19c-106">This article documents hello various specific customizations that are available for App Service Environments.</span></span>

<span data-ttu-id="7e19c-107">Ha még nem rendelkezik az App Service-környezetek, lásd: [hogyan tooCreate az App Service-környezetek](app-service-web-how-to-create-an-app-service-environment.md).</span><span class="sxs-lookup"><span data-stu-id="7e19c-107">If you do not have an App Service Environment, see [How tooCreate an App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span></span>

<span data-ttu-id="7e19c-108">App Service Environment-környezet testreszabások tárolhatja használatával tömb hello új **clusterSettings** attribútum.</span><span class="sxs-lookup"><span data-stu-id="7e19c-108">You can store App Service Environment customizations by using an array in hello new **clusterSettings** attribute.</span></span> <span data-ttu-id="7e19c-109">Ez az attribútum található hello "Tulajdonságok" szótára hello *hostingEnvironments* Azure Resource Manager entitás.</span><span class="sxs-lookup"><span data-stu-id="7e19c-109">This attribute is found in hello "Properties" dictionary of hello *hostingEnvironments* Azure Resource Manager entity.</span></span>

<span data-ttu-id="7e19c-110">hello rövidített Resource Manager sablon alábbi kódrészletben láthatja hello **clusterSettings** attribútum:</span><span class="sxs-lookup"><span data-stu-id="7e19c-110">hello following abbreviated Resource Manager template snippet shows hello **clusterSettings** attribute:</span></span>

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

<span data-ttu-id="7e19c-111">Hello **clusterSettings** attribútum tartalmazhat egy Resource Manager sablon tooupdate hello App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="7e19c-111">hello **clusterSettings** attribute can be included in a Resource Manager template tooupdate hello App Service Environment.</span></span>

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a><span data-ttu-id="7e19c-112">Használja az Azure erőforrás-kezelő tooupdate egy App Service Environment-környezet</span><span class="sxs-lookup"><span data-stu-id="7e19c-112">Use Azure Resource Explorer tooupdate an App Service Environment</span></span>
<span data-ttu-id="7e19c-113">Másik megoldásként frissítheti hello App Service Environment-környezet használatával [Azure erőforrás-kezelő](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e19c-113">Alternatively, you can update hello App Service Environment by using [Azure Resource Explorer](https://resources.azure.com).</span></span>  

1. <span data-ttu-id="7e19c-114">Az erőforrás-kezelőben, nyissa meg az App Service Environment-környezet hello toohello csomópontot (**előfizetések** > **resourceGroups** > **szolgáltatók**  >  **Microsoft.Web** > **hostingEnvironments**).</span><span class="sxs-lookup"><span data-stu-id="7e19c-114">In Resource Explorer, go toohello node for hello App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span></span> <span data-ttu-id="7e19c-115">Kattintson az adott App Service-környezet, amelyet az tooupdate hello.</span><span class="sxs-lookup"><span data-stu-id="7e19c-115">Then click hello specific App Service Environment that you want tooupdate.</span></span>
2. <span data-ttu-id="7e19c-116">Hello jobb oldali ablaktáblában kattintson **olvasási/írási** a hello felső eszköztár tooallow interaktív szerkesztése az erőforrás-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="7e19c-116">In hello right pane, click **Read/Write** in hello upper toolbar tooallow interactive editing in Resource Explorer.</span></span>  
3. <span data-ttu-id="7e19c-117">Kattintson a kék hello **szerkesztése** gomb toomake hello Resource Manager sablon szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="7e19c-117">Click hello blue **Edit** button toomake hello Resource Manager template editable.</span></span>
4. <span data-ttu-id="7e19c-118">Toohello hello jobb oldali ablaktábla alján görgessen.</span><span class="sxs-lookup"><span data-stu-id="7e19c-118">Scroll toohello bottom of hello right pane.</span></span> <span data-ttu-id="7e19c-119">Hello **clusterSettings** attribútum jelenleg hello legalsó, ahol adja meg, vagy frissítse az értéket.</span><span class="sxs-lookup"><span data-stu-id="7e19c-119">hello **clusterSettings** attribute is at hello very bottom, where you can enter or update its value.</span></span>
5. <span data-ttu-id="7e19c-120">Hello kívánt konfigurációs értékeket típusa (vagy a másolás és beillesztés) hello tömbje **clusterSettings** attribútum.</span><span class="sxs-lookup"><span data-stu-id="7e19c-120">Type (or copy and paste) hello array of configuration values you want in hello **clusterSettings** attribute.</span></span>  
6. <span data-ttu-id="7e19c-121">Kattintson a zöld hello **PUT** gombra kattint, amely rendelkezik hello jobb oldali toocommit hello módosítása toohello App Service Environment-környezet hello tetején található.</span><span class="sxs-lookup"><span data-stu-id="7e19c-121">Click hello green **PUT** button that's located at hello top of hello right pane toocommit hello change toohello App Service Environment.</span></span>

<span data-ttu-id="7e19c-122">Hello módosítás elküldését, de hello App Service Environment-környezet tootake hatás hello módosítása az előtér-webkiszolgálóinak hello számát szorozva körülbelül 30 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7e19c-122">However you submit hello change, it takes roughly 30 minutes multiplied by hello number of front ends in hello App Service Environment for hello change tootake effect.</span></span>
<span data-ttu-id="7e19c-123">Például ha az App Service-környezetek négy előtér-webkiszolgálóinak, körülbelül két órát hello konfigurációs frissítés toofinish tart.</span><span class="sxs-lookup"><span data-stu-id="7e19c-123">For example, if an App Service Environment has four front ends, it will take roughly two hours for hello configuration update toofinish.</span></span> <span data-ttu-id="7e19c-124">Hello konfigurációváltozás megkezdődött van, míg más skálázási műveletek vagy konfiguráció-módosítási műveletek nem történhet a hello App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="7e19c-124">While hello configuration change is being rolled out, no other scaling operations or configuration change operations can take place in hello App Service Environment.</span></span>

## <a name="disable-tls-10"></a><span data-ttu-id="7e19c-125">A TLS 1.0 letiltása</span><span class="sxs-lookup"><span data-stu-id="7e19c-125">Disable TLS 1.0</span></span>
<span data-ttu-id="7e19c-126">Ismétlődő kérdés az ügyfelektől, különösen az ügyfelek, akik a PCI-megfelelőséget foglalkoznak eseményeket, a hogyan tooexplicitly letiltja a TLS 1.0 alkalmazásaikat.</span><span class="sxs-lookup"><span data-stu-id="7e19c-126">A recurring question from customers, especially customers who are dealing with PCI compliance audits, is how tooexplicitly disable TLS 1.0 for their apps.</span></span>

<span data-ttu-id="7e19c-127">A TLS 1.0 hello következő keresztül letiltható **clusterSettings** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="7e19c-127">TLS 1.0 can be disabled through hello following **clusterSettings** entry:</span></span>

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a><span data-ttu-id="7e19c-128">Módosítsa a TLS titkosító csomag sorrendje</span><span class="sxs-lookup"><span data-stu-id="7e19c-128">Change TLS cipher suite order</span></span>
<span data-ttu-id="7e19c-129">-Ügyfél egy másik kérdést akkor, ha azok módosíthatja, hogy a kiszolgáló által egyeztetett titkosítási hello listáját, és ez elérhető hello módosításával **clusterSettings** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="7e19c-129">Another question from customers is if they can modify hello list of ciphers negotiated by their server and this can be achieved by modifying hello **clusterSettings** as shown below.</span></span> <span data-ttu-id="7e19c-130">hello elérhető titkosító csomagok listája olvasható be a [MSDN-cikkben](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span><span class="sxs-lookup"><span data-stu-id="7e19c-130">hello list of cipher suites available can be retrieved from [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span></span>

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> <span data-ttu-id="7e19c-131">Érvénytelen értékek vannak beállítva, a SChannel nem érti hello titkosítócsomag, ha minden TLS kommunikációs tooyour server előfordulhat, hogy működni.</span><span class="sxs-lookup"><span data-stu-id="7e19c-131">If incorrect values are set for hello cipher suite that SChannel cannot understand, all TLS communication tooyour server might stop functioning.</span></span> <span data-ttu-id="7e19c-132">Ebben az esetben szüksége lesz a tooremove hello *FrontEndSSLCipherSuiteOrder* bejegyzést **clusterSettings** , és küldje el a hello Resource Manager sablon toorevert hátsó toohello alapértelmezett titkosítás frissítése csomag beállításait.</span><span class="sxs-lookup"><span data-stu-id="7e19c-132">In such a case, you will need tooremove hello *FrontEndSSLCipherSuiteOrder* entry from **clusterSettings** and submit hello updated Resource Manager template toorevert back toohello default cipher suite settings.</span></span>  <span data-ttu-id="7e19c-133">Körültekintően használja ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="7e19c-133">Please use this functionality with caution.</span></span>
> 
> 

## <a name="get-started"></a><span data-ttu-id="7e19c-134">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7e19c-134">Get started</span></span>
<span data-ttu-id="7e19c-135">hello Azure gyors üzembe helyezés Resource Manager sablon webhely alap definíciója hello rendelkező sablont tartalmaz [egy App Service Environment-környezet létrehozása](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span><span class="sxs-lookup"><span data-stu-id="7e19c-135">hello Azure Quickstart Resource Manager template site includes a template with hello base definition for [creating an App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span></span>

<!-- LINKS -->

<!-- IMAGES -->
