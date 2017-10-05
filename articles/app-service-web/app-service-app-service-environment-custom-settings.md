---
title: "Az App Service Environment-környezetek egyéni beállítások"
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
ms.openlocfilehash: 687475fae0c90713c15e8abbb92b71059eae81c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a><span data-ttu-id="76e58-103">App Service Environment-környezetek egyéni konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="76e58-103">Custom configuration settings for App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="76e58-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="76e58-104">Overview</span></span>
<span data-ttu-id="76e58-105">Mivel az App Service Environment-környezetek egyetlen ügyfél számára elkülönített, vannak bizonyos konfigurációs beállításainak kizárólag az App Service Environment-környezetek alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="76e58-105">Because App Service Environments are isolated to a single customer, there are certain configuration settings that can be applied exclusively to App Service Environments.</span></span> <span data-ttu-id="76e58-106">Ez a cikk a különböző testreszabásokat App Service Environment-környezetek számára elérhető dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="76e58-106">This article documents the various specific customizations that are available for App Service Environments.</span></span>

<span data-ttu-id="76e58-107">Ha még nem rendelkezik az App Service-környezetek, lásd: [egy App Service Environment-környezet létrehozása](app-service-web-how-to-create-an-app-service-environment.md).</span><span class="sxs-lookup"><span data-stu-id="76e58-107">If you do not have an App Service Environment, see [How to Create an App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span></span>

<span data-ttu-id="76e58-108">Az új tömböt használatával tárolhatja az App Service Environment-környezet testreszabások **clusterSettings** attribútum.</span><span class="sxs-lookup"><span data-stu-id="76e58-108">You can store App Service Environment customizations by using an array in the new **clusterSettings** attribute.</span></span> <span data-ttu-id="76e58-109">Ez az attribútum található a "Tulajdonságok" szótára a *hostingEnvironments* Azure Resource Manager entitás.</span><span class="sxs-lookup"><span data-stu-id="76e58-109">This attribute is found in the "Properties" dictionary of the *hostingEnvironments* Azure Resource Manager entity.</span></span>

<span data-ttu-id="76e58-110">A következő rövidítése Resource Manager sablon kódrészletet mutat be a **clusterSettings** attribútum:</span><span class="sxs-lookup"><span data-stu-id="76e58-110">The following abbreviated Resource Manager template snippet shows the **clusterSettings** attribute:</span></span>

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

<span data-ttu-id="76e58-111">A **clusterSettings** attribútum tartalmazhat egy Resource Manager-sablon frissítéséhez az App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="76e58-111">The **clusterSettings** attribute can be included in a Resource Manager template to update the App Service Environment.</span></span>

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a><span data-ttu-id="76e58-112">Erőforrás-kezelővel Azure App Service-környezetek frissítése</span><span class="sxs-lookup"><span data-stu-id="76e58-112">Use Azure Resource Explorer to update an App Service Environment</span></span>
<span data-ttu-id="76e58-113">Másik megoldásként frissítheti az App Service Environment-környezet használatával [Azure erőforrás-kezelő](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76e58-113">Alternatively, you can update the App Service Environment by using [Azure Resource Explorer](https://resources.azure.com).</span></span>  

1. <span data-ttu-id="76e58-114">Az erőforrás-kezelőben, nyissa meg a csomópontot az App Service Environment-környezet (**előfizetések** > **resourceGroups** > **szolgáltatók** > **Microsoft.Web** > **hostingEnvironments**).</span><span class="sxs-lookup"><span data-stu-id="76e58-114">In Resource Explorer, go to the node for the App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span></span> <span data-ttu-id="76e58-115">Kattintson az adott App Service-környezet, amely frissíti.</span><span class="sxs-lookup"><span data-stu-id="76e58-115">Then click the specific App Service Environment that you want to update.</span></span>
2. <span data-ttu-id="76e58-116">Kattintson a jobb oldali ablaktáblában **olvasási/írási** a felső eszköztár engedélyezése interaktív szerkesztése az erőforrás-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="76e58-116">In the right pane, click **Read/Write** in the upper toolbar to allow interactive editing in Resource Explorer.</span></span>  
3. <span data-ttu-id="76e58-117">Kattintson a kék **szerkesztése** gomb a Resource Manager-sablon szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="76e58-117">Click the blue **Edit** button to make the Resource Manager template editable.</span></span>
4. <span data-ttu-id="76e58-118">A jobb oldali ablaktábla alján görgessen.</span><span class="sxs-lookup"><span data-stu-id="76e58-118">Scroll to the bottom of the right pane.</span></span> <span data-ttu-id="76e58-119">A **clusterSettings** attribútum legalsó, ahol adja meg, vagy frissítse az értéket.</span><span class="sxs-lookup"><span data-stu-id="76e58-119">The **clusterSettings** attribute is at the very bottom, where you can enter or update its value.</span></span>
5. <span data-ttu-id="76e58-120">Írja be (vagy másolja és illessze be) a kívánt konfigurációs tömböt a **clusterSettings** attribútum.</span><span class="sxs-lookup"><span data-stu-id="76e58-120">Type (or copy and paste) the array of configuration values you want in the **clusterSettings** attribute.</span></span>  
6. <span data-ttu-id="76e58-121">Kattintson a zöld **PUT** gombra kattint, amely rendelkezik az App Service Environment-környezet a változtatás véglegesítése a jobb oldali ablaktábla tetején található.</span><span class="sxs-lookup"><span data-stu-id="76e58-121">Click the green **PUT** button that's located at the top of the right pane to commit the change to the App Service Environment.</span></span>

<span data-ttu-id="76e58-122">A módosítás küldje el, de az App Service-környezet a változtatás érvénybe léptetéséhez előtér-webkiszolgálóinak számát szorozva körülbelül 30 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="76e58-122">However you submit the change, it takes roughly 30 minutes multiplied by the number of front ends in the App Service Environment for the change to take effect.</span></span>
<span data-ttu-id="76e58-123">Például ha az App Service-környezetek négy előtér-webkiszolgálóinak, eltarthat körülbelül két órát a konfigurációs frissítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="76e58-123">For example, if an App Service Environment has four front ends, it will take roughly two hours for the configuration update to finish.</span></span> <span data-ttu-id="76e58-124">A konfigurációváltozás van megkezdődött, míg más skálázási műveletek vagy konfiguráció-módosítási műveletek nem történhet az App Service Environment-környezetben.</span><span class="sxs-lookup"><span data-stu-id="76e58-124">While the configuration change is being rolled out, no other scaling operations or configuration change operations can take place in the App Service Environment.</span></span>

## <a name="disable-tls-10"></a><span data-ttu-id="76e58-125">A TLS 1.0 letiltása</span><span class="sxs-lookup"><span data-stu-id="76e58-125">Disable TLS 1.0</span></span>
<span data-ttu-id="76e58-126">Ismétlődő kérdés az ügyfelektől, különösen az ügyfelek, akik a PCI-megfelelőséget foglalkoznak eseményeket, a explicit módon az alkalmazások a TLS 1.0 letiltása.</span><span class="sxs-lookup"><span data-stu-id="76e58-126">A recurring question from customers, especially customers who are dealing with PCI compliance audits, is how to explicitly disable TLS 1.0 for their apps.</span></span>

<span data-ttu-id="76e58-127">A TLS 1.0 a következő keresztül letiltható **clusterSettings** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="76e58-127">TLS 1.0 can be disabled through the following **clusterSettings** entry:</span></span>

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a><span data-ttu-id="76e58-128">Módosítsa a TLS titkosító csomag sorrendje</span><span class="sxs-lookup"><span data-stu-id="76e58-128">Change TLS cipher suite order</span></span>
<span data-ttu-id="76e58-129">-Ügyfél egy másik kérdést akkor, ha azok módosíthatja a kiszolgáló által egyeztetett titkosítási a listáját, és ez módosításával érhető el a **clusterSettings** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="76e58-129">Another question from customers is if they can modify the list of ciphers negotiated by their server and this can be achieved by modifying the **clusterSettings** as shown below.</span></span> <span data-ttu-id="76e58-130">Rendelkezésre álló titkosító csomagok listája olvasható be a [MSDN-cikkben](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span><span class="sxs-lookup"><span data-stu-id="76e58-130">The list of cipher suites available can be retrieved from [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span></span>

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> <span data-ttu-id="76e58-131">Érvénytelen értékek vannak beállítva, a titkosítási csomagok, amelyek nem érti a SChannel a, ha a kiszolgáló minden TLS kommunikációs leállhat működését.</span><span class="sxs-lookup"><span data-stu-id="76e58-131">If incorrect values are set for the cipher suite that SChannel cannot understand, all TLS communication to your server might stop functioning.</span></span> <span data-ttu-id="76e58-132">Ebben az esetben kell eltávolítani a *FrontEndSSLCipherSuiteOrder* bejegyzést **clusterSettings** , és küldje el a frissített Resource Manager-sablon, ha vissza kíván térni az alapértelmezett titkosító csomag beállításokat.</span><span class="sxs-lookup"><span data-stu-id="76e58-132">In such a case, you will need to remove the *FrontEndSSLCipherSuiteOrder* entry from **clusterSettings** and submit the updated Resource Manager template to revert back to the default cipher suite settings.</span></span>  <span data-ttu-id="76e58-133">Körültekintően használja ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="76e58-133">Please use this functionality with caution.</span></span>
> 
> 

## <a name="get-started"></a><span data-ttu-id="76e58-134">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="76e58-134">Get started</span></span>
<span data-ttu-id="76e58-135">Az Azure gyors üzembe helyezés Resource Manager sablon hely tartalmazza az alapdefinícióját tartalmazó sablon [egy App Service Environment-környezet létrehozása](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span><span class="sxs-lookup"><span data-stu-id="76e58-135">The Azure Quickstart Resource Manager template site includes a template with the base definition for [creating an App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span></span>

<!-- LINKS -->

<!-- IMAGES -->
