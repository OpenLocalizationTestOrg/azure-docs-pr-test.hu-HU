---
title: "Az Azure Mobile Engagement webes SDK áttekintése |} Microsoft Docs"
description: "A legújabb frissítéseket és a webszolgáltatási SDK az Azure Mobile Engagement eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 770a83131a3e661771db50b22ce7de25b2d541cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a><span data-ttu-id="aa32d-103">Az Azure Mobile Engagement webes SDK-t</span><span class="sxs-lookup"><span data-stu-id="aa32d-103">Azure Mobile Engagement Web SDK</span></span>
<span data-ttu-id="aa32d-104">Minden a vonatkozó további információért integrálása az Azure Mobile Engagement egy web app alkalmazásban, kezdje itt.</span><span class="sxs-lookup"><span data-stu-id="aa32d-104">Start here for all the details about how to integrate Azure Mobile Engagement in a web app.</span></span> <span data-ttu-id="aa32d-105">Ha azt szeretné, próbálja ki a saját webalkalmazás első lépések előtt, olvassa el a [15 perces oktatóanyag](mobile-engagement-web-app-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="aa32d-105">If you'd like to give it a try before getting started with your own web app, see our [15-minute tutorial](mobile-engagement-web-app-get-started.md).</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="aa32d-106">Integráció eljárások</span><span class="sxs-lookup"><span data-stu-id="aa32d-106">Integration procedures</span></span>
1. <span data-ttu-id="aa32d-107">Ismerje meg, [integrálása a Mobile Engagement a webalkalmazáson belüli](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="aa32d-107">Learn [how to integrate Mobile Engagement in your web app](mobile-engagement-web-integrate-engagement.md).</span></span>
2. <span data-ttu-id="aa32d-108">Címke terv végrehajtásához további [hogyan használható a speciális Mobile Engagement API szerinti címkézését a webalkalmazás](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="aa32d-108">For tag plan implementation, learn [how to use the advanced Mobile Engagement tagging API in your web app](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="release-notes"></a><span data-ttu-id="aa32d-109">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="aa32d-109">Release notes</span></span>
### <a name="202-10182016"></a><span data-ttu-id="aa32d-110">2.0.2 (10/18/2016)</span><span class="sxs-lookup"><span data-stu-id="aa32d-110">2.0.2 (10/18/2016)</span></span>
* <span data-ttu-id="aa32d-111">Rögzített összeomlási a privát böngészés (Safari).</span><span class="sxs-lookup"><span data-stu-id="aa32d-111">Fixed crash on private browsing (Safari).</span></span>
* <span data-ttu-id="aa32d-112">Rögzített összeomlási a böngésző cookie-k le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="aa32d-112">Fixed crash on browsers with cookies disabled.</span></span>

<span data-ttu-id="aa32d-113">Az összes verzió, lásd: a [végezze el a kibocsátási megjegyzések](mobile-engagement-web-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="aa32d-113">For all versions, please see the [complete release notes](mobile-engagement-web-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="aa32d-114">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="aa32d-114">Upgrade procedures</span></span>
### <a name="upgrade-from-121-to-200"></a><span data-ttu-id="aa32d-115">2.0.0 1.2.1-es frissítésével</span><span class="sxs-lookup"><span data-stu-id="aa32d-115">Upgrade from 1.2.1 to 2.0.0</span></span>
<span data-ttu-id="aa32d-116">Az alábbi szakaszok ismertetik a Mobile Engagement webes SDK-integráció át a Capptain szolgáltatás, amelyet Capptain SAS-alkalmazásokhoz az Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="aa32d-116">The following sections describe how to migrate a Mobile Engagement Web SDK integration from the Capptain service, offered by Capptain SAS, to an Azure Mobile Engagement app.</span></span> <span data-ttu-id="aa32d-117">Ha az áttelepítés verziójából származó rendszernél korábbi 1.2.1-es, tekintse át a Capptain webhely 1.2.1-es először át, és ezután alkalmazza az alábbi eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="aa32d-117">If you are migrating from a version earlier than 1.2.1, please consult the Capptain website to migrate to 1.2.1 first, and then apply the following procedures.</span></span>

<span data-ttu-id="aa32d-118">A Mobile Engagement webes SDK ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy a Reach-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aa32d-118">This version of the Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or the Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa32d-119">Capptain és Azure Mobile Engagement nem ugyanazt a szolgáltatást, és a következő eljárások csak hogyan telepítheti át az ügyfélalkalmazás jelöljön ki.</span><span class="sxs-lookup"><span data-stu-id="aa32d-119">Capptain and Azure Mobile Engagement are not the same service, and the following procedures highlight only how to migrate the client app.</span></span> <span data-ttu-id="aa32d-120">A Mobile Engagement webes SDK az alkalmazás áttelepítése rendszer nem telepíti át az adatok egy Capptain kiszolgálóról a Mobile Engagement-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="aa32d-120">Migrating the Mobile Engagement Web SDK in the app will not migrate your data from a Capptain server to a Mobile Engagement server.</span></span>
> 
> 

#### <a name="javascript-files"></a><span data-ttu-id="aa32d-121">JavaScript-fájlok</span><span class="sxs-lookup"><span data-stu-id="aa32d-121">JavaScript files</span></span>
<span data-ttu-id="aa32d-122">A fájl capptain sdk.js cserélje le az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.</span><span class="sxs-lookup"><span data-stu-id="aa32d-122">Replace the file capptain-sdk.js with the file azure-engagement.js, and then update your script imports accordingly.</span></span>

#### <a name="remove-capptain-reach"></a><span data-ttu-id="aa32d-123">Capptain Reach eltávolítása</span><span class="sxs-lookup"><span data-stu-id="aa32d-123">Remove Capptain Reach</span></span>
<span data-ttu-id="aa32d-124">A Mobile Engagement webes SDK ezen verziója nem támogatja a Reach-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aa32d-124">This version of the Mobile Engagement Web SDK doesn't support the Reach feature.</span></span> <span data-ttu-id="aa32d-125">Ha az alkalmazás rendelkezik Capptain Reach integrálva, el kell távolítania azt.</span><span class="sxs-lookup"><span data-stu-id="aa32d-125">If you have integrated Capptain Reach into your application, you need to remove it.</span></span>

<span data-ttu-id="aa32d-126">Távolítsa el a CSS elérni importálása a lapról, és törli a kapcsolódó .css-fájl (capptain-reach.css, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="aa32d-126">Remove the Reach CSS import from your page and delete the related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="aa32d-127">A következő Reach erőforrások törlése: a Bezárás lemezkép (capptain-close.png, alapértelmezés szerint) és a márka ikon (capptain-értesítés-ikonra, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="aa32d-127">Delete the following Reach resources: the close image (capptain-close.png, by default) and the brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="aa32d-128">Távolítsa el az alkalmazásbeli értesítésekben elérni felhasználói felülete.</span><span class="sxs-lookup"><span data-stu-id="aa32d-128">Remove the Reach UI for in-app notifications.</span></span> <span data-ttu-id="aa32d-129">Az alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="aa32d-129">The default layout looks like this:</span></span>

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

<span data-ttu-id="aa32d-130">Távolítsa el a szöveget és webes mutató hirdetmények és szavazások esetén elérni felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="aa32d-130">Remove the Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="aa32d-131">Az alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="aa32d-131">The default layout looks like this:</span></span>

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

<span data-ttu-id="aa32d-132">Távolítsa el a `reach` a konfigurációját, ha van ilyen objektum.</span><span class="sxs-lookup"><span data-stu-id="aa32d-132">Remove the `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="aa32d-133">Néz ki:</span><span class="sxs-lookup"><span data-stu-id="aa32d-133">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="aa32d-134">Távolítsa el bármilyen egyéb Reach testreszabása: például kategóriák.</span><span class="sxs-lookup"><span data-stu-id="aa32d-134">Remove any other Reach customization, such as categories.</span></span>

#### <a name="remove-deprecated-apis"></a><span data-ttu-id="aa32d-135">Távolítsa el az elavult API-k</span><span class="sxs-lookup"><span data-stu-id="aa32d-135">Remove deprecated APIs</span></span>
<span data-ttu-id="aa32d-136">Néhány API-kat Capptain a Mobile Engagement webes SDK elavult.</span><span class="sxs-lookup"><span data-stu-id="aa32d-136">Some APIs from Capptain are deprecated in the Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="aa32d-137">Távolítsa el a következő API-k bármely hívások: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="aa32d-137">Remove any calls to the following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="aa32d-138">Távolítsa el a következő visszahívások bármelyikét a Capptain konfigurációjából: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="aa32d-138">Remove any of the following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

#### <a name="configuration"></a><span data-ttu-id="aa32d-139">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="aa32d-139">Configuration</span></span>
<span data-ttu-id="aa32d-140">A Mobile Engagement SDK-azonosítókat, például az alkalmazásazonosító konfigurálása egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="aa32d-140">Mobile Engagement uses a connection string to configure SDK identifiers, for example, the application identifier.</span></span>

<span data-ttu-id="aa32d-141">Az Alkalmazásazonosító cserélje le a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="aa32d-141">Replace the application ID with your connection string.</span></span> <span data-ttu-id="aa32d-142">Vegye figyelembe, hogy az SDK-konfigurációhoz a globális objektum-ről változik `capptain` való `azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="aa32d-142">Note that the global object for the SDK configuration changes from `capptain` to `azureEngagement`.</span></span>

<span data-ttu-id="aa32d-143">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="aa32d-143">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="aa32d-144">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="aa32d-144">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="aa32d-145">A kapcsolati karakterlánc az alkalmazás az Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="aa32d-145">The connection string for your application is displayed in the Azure portal.</span></span>

#### <a name="javascript-apis"></a><span data-ttu-id="aa32d-146">JavaScript API-k</span><span class="sxs-lookup"><span data-stu-id="aa32d-146">JavaScript APIs</span></span>
<span data-ttu-id="aa32d-147">A globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement`, de használhatja a `window.engagement` alias az API-hívásokhoz.</span><span class="sxs-lookup"><span data-stu-id="aa32d-147">The global JavaScript object `window.capptain` has been renamed `window.azureEngagement`, but you can use the `window.engagement` alias for API calls.</span></span> <span data-ttu-id="aa32d-148">Az alias nem használható SDK konfigurációjának meghatározása.</span><span class="sxs-lookup"><span data-stu-id="aa32d-148">You can't use this alias to define the SDK configuration.</span></span>

<span data-ttu-id="aa32d-149">Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="aa32d-149">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

<span data-ttu-id="aa32d-150">Ha az Azure Mobile Engagement webszolgáltatási SDK egy korábbi verzióját már a az alkalmazásba integrált része, kérjük, olvassa el kapcsolatos [eljárások frissítése](mobile-engagement-web-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="aa32d-150">If you have already integrated an earlier version of the Azure Mobile Engagement Web SDK into your application, please read about [upgrade procedures](mobile-engagement-web-upgrade-procedure.md).</span></span>

