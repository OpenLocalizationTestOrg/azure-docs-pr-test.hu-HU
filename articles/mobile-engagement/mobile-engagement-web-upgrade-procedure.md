---
title: "Az Azure Mobile Engagement webes SDK frissítési eljárásait |} Microsoft Docs"
description: "A legújabb frissítéseket és a webszolgáltatási SDK az Azure Mobile Engagement eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: afa8037dcb7a53042fa606e2c4014b442d4be326
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a><span data-ttu-id="43171-103">Az Azure Mobile Engagement webes SDK frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="43171-103">Azure Mobile Engagement Web SDK upgrade procedures</span></span>
<span data-ttu-id="43171-104">Ha az Azure Mobile Engagement webszolgáltatási SDK egy korábbi verzióját már a webes alkalmazás van integrálva, vegye figyelembe a következő szempontokat, amikor frissít, az SDK-t szeretné.</span><span class="sxs-lookup"><span data-stu-id="43171-104">If you have already integrated an earlier version of the Azure Mobile Engagement Web SDK into your web application, you need to consider the following points when you upgrade the SDK.</span></span>

<span data-ttu-id="43171-105">Ha kihagyja a Mobile Engagement webes SDK több verziója, szükség lehet a frissítési folyamat során számos műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="43171-105">If you skipped multiple versions of the Mobile Engagement Web SDK, you might need to complete several procedures during the upgrade process.</span></span> <span data-ttu-id="43171-106">Például ha áttelepít 1.4.0 1.6.0, először eljárások a 1.5.0 1.4.0 frissítésével.</span><span class="sxs-lookup"><span data-stu-id="43171-106">For example, if you migrate from 1.4.0 to 1.6.0, first follow the procedures to upgrade from 1.4.0 to 1.5.0.</span></span> <span data-ttu-id="43171-107">Ezután kövesse 1.6.0 1.5.0 frissítésével.</span><span class="sxs-lookup"><span data-stu-id="43171-107">Then, follow the procedures to upgrade from 1.5.0 to 1.6.0.</span></span>

<span data-ttu-id="43171-108">Bármelyik verzióra frissít, a fájl azure-engagement.js korábbi verzióját cserélje le a legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="43171-108">Whichever version you upgrade from, replace any earlier version of the file azure-engagement.js with the latest version of the file.</span></span>

## <a name="upgrade-from-121-to-200"></a><span data-ttu-id="43171-109">2.0.0 1.2.1-es frissítésével</span><span class="sxs-lookup"><span data-stu-id="43171-109">Upgrade from 1.2.1 to 2.0.0</span></span>
<span data-ttu-id="43171-110">Ez a szakasz ismerteti, hogyan telepíthetők át a Mobile Engagement webes SDK-integráció a Capptain szolgáltatás, amelyet Capptain SAS-alkalmazásokhoz az Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="43171-110">This section describes how to migrate a Mobile Engagement Web SDK integration from the Capptain service, offered by Capptain SAS, to an Azure Mobile Engagement app.</span></span> <span data-ttu-id="43171-111">Ha egy korábbi verziójáról telepít, tekintse át a Capptain webhely 1.2.1-es először át, és ezután alkalmazza az alábbi eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="43171-111">If you are migrating from an earlier version, please consult the Capptain website to first migrate to 1.2.1, and then apply the following procedures.</span></span>

<span data-ttu-id="43171-112">A Mobile Engagement webes SDK ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy a Reach-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43171-112">This version of the Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or the Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43171-113">Capptain és Azure Mobile Engagement nincsenek ugyanazt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="43171-113">Capptain and Azure Mobile Engagement are not the same service.</span></span> <span data-ttu-id="43171-114">Az alábbi eljárás csak hogyan telepítheti át az ügyfélalkalmazás mutatja be.</span><span class="sxs-lookup"><span data-stu-id="43171-114">The following procedure highlights only how to migrate the client app.</span></span> <span data-ttu-id="43171-115">A Mobile Engagement webes SDK az alkalmazás áttelepítése rendszer nem telepíti át az adatok egy Capptain kiszolgálóról a Mobile Engagement-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="43171-115">Migrating the Mobile Engagement Web SDK in the app will not migrate your data from a Capptain server to a Mobile Engagement server.</span></span>
> 
> 

### <a name="javascript-files"></a><span data-ttu-id="43171-116">JavaScript-fájlok</span><span class="sxs-lookup"><span data-stu-id="43171-116">JavaScript files</span></span>
<span data-ttu-id="43171-117">A fájl capptain sdk.js cserélje le az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.</span><span class="sxs-lookup"><span data-stu-id="43171-117">Replace the file capptain-sdk.js with the file azure-engagement.js, and then update your script imports accordingly.</span></span>

### <a name="remove-capptain-reach"></a><span data-ttu-id="43171-118">Capptain Reach eltávolítása</span><span class="sxs-lookup"><span data-stu-id="43171-118">Remove Capptain Reach</span></span>
<span data-ttu-id="43171-119">A Mobile Engagement webes SDK ezen verziója nem támogatja a Reach-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="43171-119">This version of the Mobile Engagement Web SDK doesn't support the Reach feature.</span></span> <span data-ttu-id="43171-120">Ha az alkalmazás Capptain Reach integrálva, el kell távolítania azt.</span><span class="sxs-lookup"><span data-stu-id="43171-120">If you integrated Capptain Reach into your application, you need to remove it.</span></span>

<span data-ttu-id="43171-121">Távolítsa el a CSS elérni importálása a lapról, és törli a kapcsolódó .css-fájl (capptain-reach.css, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="43171-121">Remove the Reach CSS import from your page and delete the related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="43171-122">A következő Reach erőforrások törlése: a Bezárás lemezkép (capptain-close.png, alapértelmezés szerint) és a márka ikon (capptain-értesítés-ikonra, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="43171-122">Delete the following Reach resources: the close image (capptain-close.png, by default) and the brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="43171-123">Távolítsa el az alkalmazásbeli értesítésekben elérni felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="43171-123">Remove the Reach UI for in-app notifications.</span></span> <span data-ttu-id="43171-124">Az alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="43171-124">The default layout looks like this:</span></span>

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

<span data-ttu-id="43171-125">Távolítsa el a szöveget és webes mutató hirdetmények és szavazások esetén elérni felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="43171-125">Remove the Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="43171-126">Az alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="43171-126">The default layout looks like this:</span></span>

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

<span data-ttu-id="43171-127">Távolítsa el a `reach` a konfigurációját, ha van ilyen objektum.</span><span class="sxs-lookup"><span data-stu-id="43171-127">Remove the `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="43171-128">Néz ki:</span><span class="sxs-lookup"><span data-stu-id="43171-128">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="43171-129">Távolítsa el bármilyen egyéb Reach testreszabása: például kategóriák.</span><span class="sxs-lookup"><span data-stu-id="43171-129">Remove any other Reach customization, such as categories.</span></span>

### <a name="remove-deprecated-apis"></a><span data-ttu-id="43171-130">Távolítsa el az elavult API-k</span><span class="sxs-lookup"><span data-stu-id="43171-130">Remove deprecated APIs</span></span>
<span data-ttu-id="43171-131">Néhány API-kat Capptain a Mobile Engagement webes SDK elavult.</span><span class="sxs-lookup"><span data-stu-id="43171-131">Some APIs from Capptain are deprecated in the Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="43171-132">Távolítsa el a következő API-k bármely hívások: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="43171-132">Remove any calls to the following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="43171-133">Távolítsa el a következő visszahívások minden példányát a Capptain konfigurációjából: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="43171-133">Remove any instances of the following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

### <a name="configuration"></a><span data-ttu-id="43171-134">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="43171-134">Configuration</span></span>
<span data-ttu-id="43171-135">A Mobile Engagement SDK-azonosítókat, például az alkalmazásazonosító konfigurálása egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="43171-135">Mobile Engagement uses a connection string to configure SDK identifiers, for example, the application identifier.</span></span>

<span data-ttu-id="43171-136">Az Alkalmazásazonosító cserélje le a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="43171-136">Replace the application ID with your connection string.</span></span> <span data-ttu-id="43171-137">Vegye figyelembe, hogy az SDK-konfigurációhoz a globális objektum-ről változik `capptain` való `azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="43171-137">Note that the global object for the SDK configuration changes from `capptain` to `azureEngagement`.</span></span>

<span data-ttu-id="43171-138">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="43171-138">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="43171-139">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="43171-139">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="43171-140">A kapcsolati karakterlánc az alkalmazás az Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="43171-140">The connection string for your application is displayed in the Azure Portal.</span></span>

### <a name="javascript-apis"></a><span data-ttu-id="43171-141">JavaScript API-k</span><span class="sxs-lookup"><span data-stu-id="43171-141">JavaScript APIs</span></span>
<span data-ttu-id="43171-142">A globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement` is használhat, de a `window.engagement` alias az API-hívásokhoz.</span><span class="sxs-lookup"><span data-stu-id="43171-142">The global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use the `window.engagement` alias for API calls.</span></span> <span data-ttu-id="43171-143">Az alias nem használható az SDK-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="43171-143">You can't use the alias to define the SDK configuration.</span></span>

<span data-ttu-id="43171-144">Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="43171-144">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

