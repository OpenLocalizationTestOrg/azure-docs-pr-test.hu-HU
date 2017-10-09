---
title: "egy univerzális Windows Platform-(UWP), amely azonosítja a Mobile Apps aaaCreate |} Microsoft Docs"
description: "Hajtsa végre az oktatóanyag tooget az univerzális Windows Platform (UWP) alkalmazások fejlesztésére C#, Visual Basic vagy JavaScript Azure mobil-háttéralkalmazások használatával."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a><span data-ttu-id="e61a4-103">Windows-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e61a4-103">Create a Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="e61a4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e61a4-104">Overview</span></span>
<span data-ttu-id="e61a4-105">Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás tooa univerzális Windows Platform (UWP) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e61a4-105">This tutorial shows you how tooadd a cloud-based backend service tooa Universal Windows Platform (UWP) app.</span></span> <span data-ttu-id="e61a4-106">További információ: [Mi a Mobile Apps szolgáltatás?](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="e61a4-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span> <span data-ttu-id="e61a4-107">hello az alábbiakban képernyőfelvételek befejeződött hello alkalmazásból:</span><span class="sxs-lookup"><span data-stu-id="e61a4-107">hello following are screen captures from hello completed app:</span></span>

![Az elkészült asztali alkalmazás](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
<span data-ttu-id="e61a4-109">Asztali számítógépen futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e61a4-109">Running on a desktop.</span></span>

![Az elkészült mobilalkalmazás](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
<span data-ttu-id="e61a4-111">Telefonon futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e61a4-111">Running on a phone</span></span>

<span data-ttu-id="e61a4-112">Az oktatóanyag végrehajtása feltétele a Mobile Apps UWP-alkalmazásokra vonatkozó összes többi oktatóanyag elérésének.</span><span class="sxs-lookup"><span data-stu-id="e61a4-112">Completing this tutorial is a prerequisite for all other Mobile App tutorials for UWP apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e61a4-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e61a4-113">Prerequisites</span></span>
<span data-ttu-id="e61a4-114">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e61a4-114">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="e61a4-115">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="e61a4-115">An active Azure account.</span></span> <span data-ttu-id="e61a4-116">Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat.</span><span class="sxs-lookup"><span data-stu-id="e61a4-116">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="e61a4-117">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e61a4-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e61a4-118">[Visual Studio Community 2015] vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="e61a4-118">[Visual Studio Community 2015] or a later version.</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="e61a4-119">Új Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e61a4-119">Create a new Azure Mobile App backend</span></span>
<span data-ttu-id="e61a4-120">Kövesse ezeket a lépéseket toocreate egy új mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e61a4-120">Follow these steps toocreate a new Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="e61a4-121">Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak.</span><span class="sxs-lookup"><span data-stu-id="e61a4-121">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="e61a4-122">Ezután, amelyeket le fog tölteni egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="e61a4-122">Next, you will download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="e61a4-123">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e61a4-123">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a><span data-ttu-id="e61a4-124">Töltse le és futtassa a hello ügyfélprojekt</span><span class="sxs-lookup"><span data-stu-id="e61a4-124">Download and run hello client project</span></span>
<span data-ttu-id="e61a4-125">Miután konfigurálta a Mobile Apps-háttéralkalmazás, hozzon létre egy új ügyfél-alkalmazást, vagy módosíthatja egy meglévő app tooconnect tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e61a4-125">Once you have configured your Mobile App backend, you can either create a new client app or modify an existing app tooconnect tooAzure.</span></span> <span data-ttu-id="e61a4-126">Ebben a szakaszban egy UWP alkalmazás sablonprojektjét, amely testre szabott tooconnect tooyour Mobile Apps-háttéralkalmazás letöltése.</span><span class="sxs-lookup"><span data-stu-id="e61a4-126">In this section, you download a UWP app template project that is customized tooconnect tooyour Mobile App backend.</span></span>

1. <span data-ttu-id="e61a4-127">Vissza a hello **gyors üzembe helyezési** a Mobile Apps-háttéralkalmazás paneljén kattintson **hozzon létre egy új alkalmazást** > **letöltése**, bontsa ki a tömörített hello project fájlok tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e61a4-127">Back in hello **Quick start** blade for your Mobile App backend, click **Create a new app** > **Download**, then extract hello compressed project files tooyour local computer.</span></span>

    ![Gyorssablonra épülő Windows-projekt letöltése](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. <span data-ttu-id="e61a4-129">(Választható) Adja hozzá a hello UWP-alkalmazás projekt toohello hello projektként ugyanahhoz a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="e61a4-129">(Optional) Add hello UWP app project toohello same solution as hello server project.</span></span> <span data-ttu-id="e61a4-130">Ez megkönnyíti toodebug és az alkalmazás és hello háttér hello is vizsgálati hello ugyanabban a Visual Studio megoldás, ha úgy dönt, toodo tette.</span><span class="sxs-lookup"><span data-stu-id="e61a4-130">This makes it easier toodebug and test both hello app and hello backend in hello same Visual Studio solution, if you choose toodo so.</span></span> <span data-ttu-id="e61a4-131">tooadd UWP app projektet toohello megoldást, kell használnia a Visual Studio 2015-öt vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e61a4-131">tooadd a UWP app project toohello solution, you must be using Visual Studio 2015 or a later version.</span></span>
3. <span data-ttu-id="e61a4-132">Hello UWP-alkalmazást hello indítási projektként nyomja le az F5 kulcs toodeploy hello és futtatási hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e61a4-132">With hello UWP app as hello startup project, press hello F5 key toodeploy and run hello app.</span></span>
4. <span data-ttu-id="e61a4-133">Hello alkalmazásban írjon be egy értelmes szöveget, például *teljes hello oktatóanyag*, a hello **Insert a TodoItem** szövegmezőbe, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e61a4-133">In hello app, type meaningful text, such as *Complete hello tutorial*, in hello **Insert a TodoItem** text box, and then click **Save**.</span></span>

    ![Az elkészült, gyorssablonra épülő Windows-projekt asztali számítógépen](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    <span data-ttu-id="e61a4-135">Ez küld egy POST kérést toohello új mobil-háttéralkalmazás, amely az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e61a4-135">This sends a POST request toohello new mobile app backend that's hosted in Azure.</span></span>
5. <span data-ttu-id="e61a4-136">(Választható) Állítsa le hello alkalmazást, és indítsa újra egy másik eszközön vagy mobilemulátoron.</span><span class="sxs-lookup"><span data-stu-id="e61a4-136">(Optional) Stop hello app and restart it on a different device or mobile emulator.</span></span>

    ![Az elkészült, gyorssablonra épülő Windows-projekt telefonon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    <span data-ttu-id="e61a4-138">Figyelje meg, hogy hello előző lépésben mentett adatok betöltődnek Azure hello UWP-alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="e61a4-138">Notice that data saved from hello previous step is loaded from Azure after hello UWP app starts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e61a4-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e61a4-139">Next steps</span></span>
* [<span data-ttu-id="e61a4-140">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e61a4-140">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="e61a4-141">Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="e61a4-141">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="e61a4-142">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e61a4-142">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="e61a4-143">Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e61a4-143">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="e61a4-144">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="e61a4-144">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="e61a4-145">Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="e61a4-145">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="e61a4-146">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="e61a4-146">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
