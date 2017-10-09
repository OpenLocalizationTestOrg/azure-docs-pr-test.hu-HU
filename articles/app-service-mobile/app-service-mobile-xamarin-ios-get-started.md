---
title: "a Xamarin.iOS-alkalmazásokhoz az Azure App Service Mobile Apps lépései aaaGet |} Microsoft Docs"
description: "Hajtsa végre az oktatóanyag tooget az Xamarin.iOS fejlesztési Mobile Apps használatával."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="48e5d-103">Xamarin.iOS-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48e5d-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="48e5d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="48e5d-104">Overview</span></span>
<span data-ttu-id="48e5d-105">Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooa Xamarin.iOS mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48e5d-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="48e5d-106">Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy olyan egyszerű *Teendőlista* Xamarin.iOS-alkalmazást, amely az alkalmazásadatokat az Azure-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="48e5d-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="48e5d-107">Az oktatóanyag végrehajtása feltétele az összes többi Xamarin.iOS-oktatóanyag az Azure App Service hello Mobile Apps szolgáltatás használatáról.</span><span class="sxs-lookup"><span data-stu-id="48e5d-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48e5d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48e5d-108">Prerequisites</span></span>
<span data-ttu-id="48e5d-109">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="48e5d-109">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="48e5d-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="48e5d-110">An active Azure account.</span></span> <span data-ttu-id="48e5d-111">Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat.</span><span class="sxs-lookup"><span data-stu-id="48e5d-111">If you don't have an account, sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="48e5d-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48e5d-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="48e5d-113">Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="48e5d-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="48e5d-114">Az útmutatót lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="48e5d-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="48e5d-115">Mac számítógép 7.0 vagy-s újabb verziójú Xcode-dal és Xamarin Studio Communityvel.</span><span class="sxs-lookup"><span data-stu-id="48e5d-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="48e5d-116">Lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése) és [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Beállítás, telepítés és ellenőrzés Macintosh-felhasználók számára) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="48e5d-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="48e5d-117">Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48e5d-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="48e5d-118">Kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48e5d-118">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="48e5d-119">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48e5d-119">Configure hello server project</span></span>
<span data-ttu-id="48e5d-120">Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak.</span><span class="sxs-lookup"><span data-stu-id="48e5d-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="48e5d-121">Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="48e5d-121">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

<span data-ttu-id="48e5d-122">Hajtsa végre a következő lépéseket tooconfigure hello server projekt toouse hello vagy hello Node.js vagy a .NET-háttérrendszer.</span><span class="sxs-lookup"><span data-stu-id="48e5d-122">Follow hello following steps tooconfigure hello server project toouse either hello Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a><span data-ttu-id="48e5d-123">Hello Xamarin.iOS-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="48e5d-123">Download and run hello Xamarin.iOS app</span></span>
1. <span data-ttu-id="48e5d-124">Nyissa meg hello [Azure-portálon] egy böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="48e5d-124">Open hello [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="48e5d-125">A Mobile Apps hello beállítási paneljén kattintson **Ismerkedés** > **Xamarin.iOS**.</span><span class="sxs-lookup"><span data-stu-id="48e5d-125">On hello settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="48e5d-126">A 3. lépésben kattintson az **Új alkalmazás létrehozása** lehetőségre, ha még nincs kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="48e5d-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="48e5d-127">Ezután kattintson a hello **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="48e5d-127">Next click hello **Download** button.</span></span>

      <span data-ttu-id="48e5d-128">Egy ügyfélalkalmazást, amely a mobil-háttéralkalmazást tooyour lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="48e5d-128">A client application that connects tooyour mobile backend is downloaded.</span></span> <span data-ttu-id="48e5d-129">Mentse a hello tömörített projektfájlt a helyi számítógépen, és jegyezze fel a mentési helyét.</span><span class="sxs-lookup"><span data-stu-id="48e5d-129">Save hello compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="48e5d-130">Bontsa ki a letöltött hello projektet, és nyissa meg a Xamarin Studio (vagy a Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="48e5d-130">Extract hello project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="48e5d-131">Nyomja le az ENTER hello F5 kulcs toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban.</span><span class="sxs-lookup"><span data-stu-id="48e5d-131">Press hello F5 key toobuild hello project and start hello app in hello iPhone emulator.</span></span>
5. <span data-ttu-id="48e5d-132">Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, majd kattintson a hello  **+**  gombra.</span><span class="sxs-lookup"><span data-stu-id="48e5d-132">In hello app, type meaningful text, such as *Learn Xamarin*, and then click hello **+** button.</span></span>

    ![][10]

    <span data-ttu-id="48e5d-133">Hello kérelemből adatok bekerülnek hello TodoItem tábla.</span><span class="sxs-lookup"><span data-stu-id="48e5d-133">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="48e5d-134">Mobil-háttéralkalmazás hello által visszaadott hello táblában tárolt elemeket, és az adatok hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="48e5d-134">Items stored in hello table are returned by hello mobile app backend, and the data is displayed in hello list.</span></span>

> [!NOTE]
> <span data-ttu-id="48e5d-135">Megtekintheti a mobilalkalmazás-háttérrendszer tooquery hozzáférő hello kódot, és adatokat beszúrni hello QSTodoService.cs C# fájlban.</span><span class="sxs-lookup"><span data-stu-id="48e5d-135">You can review hello code that accesses your mobile app backend tooquery and insert data in hello QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="48e5d-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48e5d-136">Next steps</span></span>
* [<span data-ttu-id="48e5d-137">Kapcsolat nélküli szinkronizálás tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48e5d-137">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="48e5d-138">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48e5d-138">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="48e5d-139">Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48e5d-139">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="48e5d-140">Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél</span><span class="sxs-lookup"><span data-stu-id="48e5d-140">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portálon]: https://portal.azure.com/
