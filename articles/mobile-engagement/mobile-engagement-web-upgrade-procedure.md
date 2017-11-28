---
title: "Mobile Engagement webes SDK frissítési eljárásait aaaAzure |} Microsoft Docs"
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
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
ms.openlocfilehash: a2df65904c6b56584ce6588ed26a9b79f3aa27ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a><span data-ttu-id="f168b-103">Az Azure Mobile Engagement webes SDK frissítési eljárások</span><span class="sxs-lookup"><span data-stu-id="f168b-103">Azure Mobile Engagement Web SDK upgrade procedures</span></span>
<span data-ttu-id="f168b-104">Ha hello Azure Mobile Engagement webszolgáltatási SDK egy korábbi verziója már a webes alkalmazás van integrálva, akkor tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="f168b-104">If you have already integrated an earlier version of hello Azure Mobile Engagement Web SDK into your web application, you need tooconsider hello following points when you upgrade hello SDK.</span></span>

<span data-ttu-id="f168b-105">Ha több hello Mobile Engagement SDK-t webes verzió kihagyott, akkor toocomplete számos eljárást hello frissítési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="f168b-105">If you skipped multiple versions of hello Mobile Engagement Web SDK, you might need toocomplete several procedures during hello upgrade process.</span></span> <span data-ttu-id="f168b-106">Például, ha telepít át 1.4.0 too1.6.0, első kövesse hello eljárások tooupgrade a 1.4.0 too1.5.0.</span><span class="sxs-lookup"><span data-stu-id="f168b-106">For example, if you migrate from 1.4.0 too1.6.0, first follow hello procedures tooupgrade from 1.4.0 too1.5.0.</span></span> <span data-ttu-id="f168b-107">Ezután kövesse a 1.5.0 hello eljárások tooupgrade too1.6.0.</span><span class="sxs-lookup"><span data-stu-id="f168b-107">Then, follow hello procedures tooupgrade from 1.5.0 too1.6.0.</span></span>

<span data-ttu-id="f168b-108">Bármelyik verzióra frissít, bármely korábbi verziója hello fájl azure-engagement.js cserélje hello hello fájl legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="f168b-108">Whichever version you upgrade from, replace any earlier version of hello file azure-engagement.js with hello latest version of hello file.</span></span>

## <a name="upgrade-from-121-too200"></a><span data-ttu-id="f168b-109">Frissítés a 1.2.1-es too2.0.0</span><span class="sxs-lookup"><span data-stu-id="f168b-109">Upgrade from 1.2.1 too2.0.0</span></span>
<span data-ttu-id="f168b-110">Ez a szakasz ismerteti, hogyan toomigrate egy Mobile Engagement webes SDK-integráció hello Capptain szolgáltatásból által kínált Capptain SAS tooan Azure Mobile Engagement-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f168b-110">This section describes how toomigrate a Mobile Engagement Web SDK integration from hello Capptain service, offered by Capptain SAS, tooan Azure Mobile Engagement app.</span></span> <span data-ttu-id="f168b-111">Ha az áttelepítés egy korábbi adjon tájékoztatást hello Capptain webhely toofirst too1.2.1 át, és ezután alkalmazza az alábbi eljárásokat hello.</span><span class="sxs-lookup"><span data-stu-id="f168b-111">If you are migrating from an earlier version, please consult hello Capptain website toofirst migrate too1.2.1, and then apply hello following procedures.</span></span>

<span data-ttu-id="f168b-112">Mobile Engagement webes SDK hello ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy hello Reach szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f168b-112">This version of hello Mobile Engagement Web SDK doesn't support Samsung Smart TV, Opera TV, webOS, or hello Reach feature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f168b-113">Capptain és Azure Mobile Engagement vannak nem hello ugyanazt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f168b-113">Capptain and Azure Mobile Engagement are not hello same service.</span></span> <span data-ttu-id="f168b-114">a következő eljárás hello mutatja be, hogyan csak toomigrate hello ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f168b-114">hello following procedure highlights only how toomigrate hello client app.</span></span> <span data-ttu-id="f168b-115">Áttelepítése a Mobile Engagement webes SDK hello alkalmazásban hello rendszer nem telepíti át az adatokat egy Capptain server tooa a Mobile Engagement kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="f168b-115">Migrating hello Mobile Engagement Web SDK in hello app will not migrate your data from a Capptain server tooa Mobile Engagement server.</span></span>
> 
> 

### <a name="javascript-files"></a><span data-ttu-id="f168b-116">JavaScript-fájlok</span><span class="sxs-lookup"><span data-stu-id="f168b-116">JavaScript files</span></span>
<span data-ttu-id="f168b-117">Csere hello fájl capptain-sdk.js hello az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.</span><span class="sxs-lookup"><span data-stu-id="f168b-117">Replace hello file capptain-sdk.js with hello file azure-engagement.js, and then update your script imports accordingly.</span></span>

### <a name="remove-capptain-reach"></a><span data-ttu-id="f168b-118">Capptain Reach eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f168b-118">Remove Capptain Reach</span></span>
<span data-ttu-id="f168b-119">Mobile Engagement webes SDK hello ezen verziója nem támogatja a hello Reach szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f168b-119">This version of hello Mobile Engagement Web SDK doesn't support hello Reach feature.</span></span> <span data-ttu-id="f168b-120">Az alkalmazás Capptain Reach integrálva, ha szüksége van-e tooremove azt.</span><span class="sxs-lookup"><span data-stu-id="f168b-120">If you integrated Capptain Reach into your application, you need tooremove it.</span></span>

<span data-ttu-id="f168b-121">Távolítsa el a hello elérni CSS importálása a lapról, és törölje a hello kapcsolódó .css-fájljában (capptain-reach.css, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="f168b-121">Remove hello Reach CSS import from your page and delete hello related .css file (capptain-reach.css, by default).</span></span>

<span data-ttu-id="f168b-122">Törölje a következő Reach erőforrások hello: hello Bezárás kép (capptain-close.png, alapértelmezés szerint) és hello márka ikon (capptain-értesítés-ikonra, alapértelmezés szerint).</span><span class="sxs-lookup"><span data-stu-id="f168b-122">Delete hello following Reach resources: hello close image (capptain-close.png, by default) and hello brand icon (capptain-notification-icon, by default).</span></span>

<span data-ttu-id="f168b-123">Távolítsa el a hello UI elérni az alkalmazásbeli értesítések.</span><span class="sxs-lookup"><span data-stu-id="f168b-123">Remove hello Reach UI for in-app notifications.</span></span> <span data-ttu-id="f168b-124">hello alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="f168b-124">hello default layout looks like this:</span></span>

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

<span data-ttu-id="f168b-125">Távolítsa el a hello UI elérni a szöveg és a webes hirdetmények és szavazások esetén.</span><span class="sxs-lookup"><span data-stu-id="f168b-125">Remove hello Reach UI for text and web announcements and polls.</span></span> <span data-ttu-id="f168b-126">hello alapértelmezett elrendezési így néz ki:</span><span class="sxs-lookup"><span data-stu-id="f168b-126">hello default layout looks like this:</span></span>

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

<span data-ttu-id="f168b-127">Távolítsa el a hello `reach` a konfigurációját, ha van ilyen objektum.</span><span class="sxs-lookup"><span data-stu-id="f168b-127">Remove hello `reach` object from your configuration, if it exists.</span></span> <span data-ttu-id="f168b-128">Néz ki:</span><span class="sxs-lookup"><span data-stu-id="f168b-128">It looks like this:</span></span>

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

<span data-ttu-id="f168b-129">Távolítsa el bármilyen egyéb Reach testreszabása: például kategóriák.</span><span class="sxs-lookup"><span data-stu-id="f168b-129">Remove any other Reach customization, such as categories.</span></span>

### <a name="remove-deprecated-apis"></a><span data-ttu-id="f168b-130">Távolítsa el az elavult API-k</span><span class="sxs-lookup"><span data-stu-id="f168b-130">Remove deprecated APIs</span></span>
<span data-ttu-id="f168b-131">Néhány API-kat Capptain a Mobile Engagement webes SDK hello elavultak.</span><span class="sxs-lookup"><span data-stu-id="f168b-131">Some APIs from Capptain are deprecated in hello Mobile Engagement Web SDK.</span></span>

<span data-ttu-id="f168b-132">Távolítsa el a következő API-k hívások toohello: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.</span><span class="sxs-lookup"><span data-stu-id="f168b-132">Remove any calls toohello following APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, and `agent.sendMessageToDevice`.</span></span>

<span data-ttu-id="f168b-133">Távolítsa el a következő visszahívások a Capptain konfigurációból hello minden példányát: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.</span><span class="sxs-lookup"><span data-stu-id="f168b-133">Remove any instances of hello following callbacks from your Capptain configuration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, and `onPushMessageReceived`.</span></span>

### <a name="configuration"></a><span data-ttu-id="f168b-134">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f168b-134">Configuration</span></span>
<span data-ttu-id="f168b-135">A Mobile Engagement egy kapcsolati karakterlánc tooconfigure SDK azonosítók, például hello azonosítót használja.</span><span class="sxs-lookup"><span data-stu-id="f168b-135">Mobile Engagement uses a connection string tooconfigure SDK identifiers, for example, hello application identifier.</span></span>

<span data-ttu-id="f168b-136">Hello Alkalmazásazonosító cserélje le a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="f168b-136">Replace hello application ID with your connection string.</span></span> <span data-ttu-id="f168b-137">Vegye figyelembe, hogy hello globális objektum hello SDK konfigurációs módosul a `capptain` túl`azureEngagement`.</span><span class="sxs-lookup"><span data-stu-id="f168b-137">Note that hello global object for hello SDK configuration changes from `capptain` too`azureEngagement`.</span></span>

<span data-ttu-id="f168b-138">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="f168b-138">Before migration:</span></span>

    window.capptain = {
      appId: ...,
      [...]
    };

<span data-ttu-id="f168b-139">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="f168b-139">After migration:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

<span data-ttu-id="f168b-140">hello kapcsolati karakterlánc az alkalmazás hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f168b-140">hello connection string for your application is displayed in hello Azure Portal.</span></span>

### <a name="javascript-apis"></a><span data-ttu-id="f168b-141">JavaScript API-k</span><span class="sxs-lookup"><span data-stu-id="f168b-141">JavaScript APIs</span></span>
<span data-ttu-id="f168b-142">hello globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement` hello is használhat, de `window.engagement` alias az API-hívásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f168b-142">hello global JavaScript object `window.capptain` has been renamed `window.azureEngagement` but you can use hello `window.engagement` alias for API calls.</span></span> <span data-ttu-id="f168b-143">Hello alias toodefine hello SDK konfigurációja nem használható.</span><span class="sxs-lookup"><span data-stu-id="f168b-143">You can't use hello alias toodefine hello SDK configuration.</span></span>

<span data-ttu-id="f168b-144">Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="f168b-144">For instance, `capptain.deviceId` becomes `engagement.deviceId`, `capptain.agent.startActivity` becomes `engagement.agent.startActivity`, and so on.</span></span>

