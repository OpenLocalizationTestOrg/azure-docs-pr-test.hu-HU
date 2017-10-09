---
title: "Mobile Engagement webes SDK – áttekintés aaaAzure |} Microsoft Docs"
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
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
ms.openlocfilehash: 9e60a232b5eb2c41c405041a88e09d7137563513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a><span data-ttu-id="8f69e-103">Az Azure Mobile Engagement webes SDK-t</span><span class="sxs-lookup"><span data-stu-id="8f69e-103">Azure Mobile Engagement Web SDK</span></span>
<span data-ttu-id="8f69e-104">Az összes hello részleteit itt Start toointegrate Azure Mobile Engagement egy webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8f69e-104">Start here for all hello details about how toointegrate Azure Mobile Engagement in a web app.</span></span> <span data-ttu-id="8f69e-105">Ha azt szeretné, hogy toogive azt egy try saját webes alkalmazáshoz, az első lépések előtt tekintse meg a [15 perces oktatóanyag](mobile-engagement-web-app-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="8f69e-105">If you'd like toogive it a try before getting started with your own web app, see our [15-minute tutorial](mobile-engagement-web-app-get-started.md).</span></span>

## <a name="integration-procedures"></a><span data-ttu-id="8f69e-106">Integráció eljárások</span><span class="sxs-lookup"><span data-stu-id="8f69e-106">Integration procedures</span></span>
1. <span data-ttu-id="8f69e-107">Ismerje meg, [módját a webalkalmazás a Mobile Engagement toointegrate](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="8f69e-107">Learn [how toointegrate Mobile Engagement in your web app](mobile-engagement-web-integrate-engagement.md).</span></span>
2. <span data-ttu-id="8f69e-108">Címke terv végrehajtásához további [hogyan toouse hello speciális API szerinti címkézését a webalkalmazás a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="8f69e-108">For tag plan implementation, learn [how toouse hello advanced Mobile Engagement tagging API in your web app](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="release-notes"></a><span data-ttu-id="8f69e-109">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8f69e-109">Release notes</span></span>
### <a name="202-10182016"></a><span data-ttu-id="8f69e-110">2.0.2 (10/18/2016)</span><span class="sxs-lookup"><span data-stu-id="8f69e-110">2.0.2 (10/18/2016)</span></span>
* <span data-ttu-id="8f69e-111">Rögzített összeomlási a privát böngészés (Safari).</span><span class="sxs-lookup"><span data-stu-id="8f69e-111">Fixed crash on private browsing (Safari).</span></span>
* <span data-ttu-id="8f69e-112">Rögzített összeomlási a böngésző cookie-k le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="8f69e-112">Fixed crash on browsers with cookies disabled.</span></span>

<span data-ttu-id="8f69e-113">Az összes verzió, lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-web-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="8f69e-113">For all versions, please see hello [complete release notes](mobile-engagement-web-release-notes.md).</span></span>

## <a name="upgrade-procedures"></a><span data-ttu-id="8f69e-114">Frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="8f69e-114">Upgrade procedures</span></span>
### <a name="upgrade-from-121-too200"></a><span data-ttu-id="8f69e-115">Frissítés a 1.2.1-es too2.0.0</span><span class="sxs-lookup"><span data-stu-id="8f69e-115">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="8f69e-116">hello következő részek a hogyan toomigrate egy Mobile Engagement webes SDK-integráció hello Capptain szolgáltatásból által kínált Capptain SAS tooan Azure Mobile Engagement-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f69e-116">hello following sections describe how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="8f69e-117">Ha telepít át a korábbi 1.2.1-es, mint tekintse át először hello Capptain webhely toomigrate too1.2.1, és ezután alkalmazza az alábbi eljárásokat hello.</span><span class="sxs-lookup"><span data-stu-id="8f69e-117">If you are migrating from a version earlier than 1.2.1, please consult hello Capptain website toomigrate too1.2.1 first, and then apply hello following procedures.</span></span>

<span data-ttu-id="8f69e-118">Mobile Engagement webes SDK hello ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy hello Reach szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8f69e-118">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f69e-119">Capptain és Azure Mobile Engagement nem hello ugyanazt a szolgáltatást, és a következő eljárások hello jelölje ki, hogyan csak toomigrate hello ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8f69e-119">Capptain and Azure Mobile Engagement are not hello same service, and hello following procedures highlight only how toomigrate hello client app.</span></span> <span data-ttu-id="8f69e-120">Áttelepítése a Mobile Engagement webes SDK hello alkalmazásban hello rendszer nem telepíti át az adatokat egy Capptain server tooa a Mobile Engagement kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="8f69e-120">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

#### <a name="javascript-files"></a><span data-ttu-id="8f69e-121">JavaScript-fájlok</span><span class="sxs-lookup"><span data-stu-id="8f69e-121">JavaScript files</span></span>
<span data-ttu-id="8f69e-122">Csere hello fájl capptain-sdk.js hello az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.</span><span class="sxs-lookup"><span data-stu-id="8f69e-122">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

#### <a name="remove-capptain-reach"></a><span data-ttu-id="8f69e-123">Capptain Reach eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8f69e-123">Remove Capptain Reach</span></span>
<span data-ttu-id="8f69e-124">Mobile Engagement webes SDK hello ezen verziója nem támogatja a hello Reach szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8f69e-124">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="8f69e-125">Ha az alkalmazás rendelkezik Capptain Reach integrálva, tooremove kell azt.</span><span class="sxs-lookup"><span data-stu-id="8f69e-125">If you have integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="8f69e-126">Távolítsa el a hello elérni CSS importálása a lapról, és törölje a hello kapcsolódó .css-fájljában (capptain-reach.css, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="8f69e-126">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="8f69e-127">Törölje a következő Reach erőforrások hello: hello Bezárás kép (capptain-close.png, alapértelmezés szerint) és hello márka ikon (capptain-értesítés-ikonra, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="8f69e-127">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="8f69e-128">Távolítsa el a hello UI elérni az alkalmazásbeli értesítések.</span><span class="sxs-lookup"><span data-stu-id="8f69e-128">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="8f69e-129">hello alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="8f69e-129">hello default layout looks like this:</span></span>

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

<span data-ttu-id="8f69e-130">Távolítsa el a hello UI elérni a szöveg és a webes hirdetmények és szavazások esetén.</span><span class="sxs-lookup"><span data-stu-id="8f69e-130">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="8f69e-131">hello alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="8f69e-131">hello default layout looks like this:</span></span>

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

<span data-ttu-id="8f69e-132">Távolítsa el a hello `reach` a konfigurációját, ha van ilyen objektum.</span><span class="sxs-lookup"><span data-stu-id="8f69e-132">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="8f69e-133">Néz ki:</span><span class="sxs-lookup"><span data-stu-id="8f69e-133">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="8f69e-134">Távolítsa el bármilyen egyéb Reach testreszabása: például kategóriák.</span><span class="sxs-lookup"><span data-stu-id="8f69e-134">Remove any other Reach customization, such as categories.</span></span>

#### <a name="remove-deprecated-apis"></a><span data-ttu-id="8f69e-135">Távolítsa el az elavult API-k</span><span class="sxs-lookup"><span data-stu-id="8f69e-135">Remove deprecated APIs</span></span>
<span data-ttu-id="8f69e-136">Néhány API-kat Capptain a Mobile Engagement webes SDK hello elavultak.</span><span class="sxs-lookup"><span data-stu-id="8f69e-136">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="8f69e-137">Távolítsa el a következő API-k hívások toohello: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="8f69e-137">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="8f69e-138">A következő visszahívások a Capptain konfigurációból hello törlését: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="8f69e-138">Remove any of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

#### <a name="configuration"></a><span data-ttu-id="8f69e-139">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8f69e-139">Configuration</span></span>
<span data-ttu-id="8f69e-140">A Mobile Engagement egy kapcsolati karakterlánc tooconfigure SDK azonosítók, például hello azonosítót használja.</span><span class="sxs-lookup"><span data-stu-id="8f69e-140">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="8f69e-141">Hello Alkalmazásazonosító cserélje le a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8f69e-141">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="8f69e-142">Vegye figyelembe, hogy hello globális objektum hello SDK konfigurációs módosul a `capptain` túl`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="8f69e-142">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="8f69e-143">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="8f69e-143">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="8f69e-144">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="8f69e-144">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="8f69e-145">hello kapcsolati karakterlánc az alkalmazás hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8f69e-145">hello connection string for your application is displayed in hello Azure portal.</span></span>

#### <a name="javascript-apis"></a><span data-ttu-id="8f69e-146">JavaScript API-k</span><span class="sxs-lookup"><span data-stu-id="8f69e-146">JavaScript APIs</span></span>
<span data-ttu-id="8f69e-147">hello globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement`, de használhat hello `window.engagement` alias az API-hívásokhoz.</span><span class="sxs-lookup"><span data-stu-id="8f69e-147">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement`, but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="8f69e-148">Az alias toodefine hello SDK konfigurációs nem használható.</span><span class="sxs-lookup"><span data-stu-id="8f69e-148">You can't use this alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="8f69e-149">Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="8f69e-149">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

<span data-ttu-id="8f69e-150">Ha egy korábbi hello Azure Mobile Engagement webes SDK már a az alkalmazásba integrált része, kérjük, olvassa el kapcsolatos [eljárások frissítése](mobile-engagement-web-upgrade-procedure.md).</span><span class="sxs-lookup"><span data-stu-id="8f69e-150">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your application, please read about [upgrade procedures](mobile-engagement-web-upgrade-procedure.md).</span></span>

