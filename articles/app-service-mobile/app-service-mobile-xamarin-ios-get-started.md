---
title: "Bevezetés az Azure App Service Mobile Apps szolgáltatásnak a Xamarin.iOS-alkalmazásokkal való használatába| Microsoft Docs"
description: "Ezt az oktatóanyagot követve megismerkedhet azokkal a kezdeti lépésekkel, amelyekkel Xamarin.iOS-alapú fejlesztésre használhatja a Mobile Apps szolgáltatást."
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
ms.openlocfilehash: 8dc965df2cd45366970effb29f246b0045a94717
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="7979c-103">Xamarin.iOS-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7979c-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="7979c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7979c-104">Overview</span></span>
<span data-ttu-id="7979c-105">Ez az cikk azt ismerteti, hogyan adhat felhőalapú háttérszolgáltatást a Xamarin.iOS-mobilalkalmazásokhoz egy Azure-alapú mobil-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="7979c-105">This tutorial shows you how to add a cloud-based backend service to a Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="7979c-106">Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy olyan egyszerű *Teendőlista* Xamarin.iOS-alkalmazást, amely az alkalmazásadatokat az Azure-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="7979c-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="7979c-107">Az oktatóanyag végrehajtása feltétele az Azure App Service Mobile Apps szolgáltatásának használatát ismertető többi Xamarin.iOS-oktatóanyag elérésének.</span><span class="sxs-lookup"><span data-stu-id="7979c-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7979c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7979c-108">Prerequisites</span></span>
<span data-ttu-id="7979c-109">Az oktatóanyag teljesítéséhez a következő előfeltételekre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="7979c-109">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="7979c-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7979c-110">An active Azure account.</span></span> <span data-ttu-id="7979c-111">Ha nincs fiókja, regisztráljon az Azure próba-előfizetésére, és akár 10 ingyenes mobilalkalmazáshoz is hozzájuthat, amelyeket a próba-előfizetés lejárta után is tovább használhat.</span><span class="sxs-lookup"><span data-stu-id="7979c-111">If you don't have an account, sign up for an Azure trial and get up to 10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="7979c-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7979c-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7979c-113">Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="7979c-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="7979c-114">Az útmutatót lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="7979c-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="7979c-115">Mac számítógép 7.0 vagy-s újabb verziójú Xcode-dal és Xamarin Studio Communityvel.</span><span class="sxs-lookup"><span data-stu-id="7979c-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="7979c-116">Lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése) és [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Beállítás, telepítés és ellenőrzés Macintosh-felhasználók számára) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="7979c-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="7979c-117">Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7979c-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="7979c-118">Mobile Apps-háttéralkalmazás létrehozásához tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="7979c-118">Follow these steps to create a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a><span data-ttu-id="7979c-119">Kiszolgálóprojekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7979c-119">Configure the server project</span></span>
<span data-ttu-id="7979c-120">Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak.</span><span class="sxs-lookup"><span data-stu-id="7979c-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="7979c-121">A következő lépésben le kell töltenie egy kiszolgálóprojektet egy egyszerű „Teendőlista” háttéralkalmazáshoz, és közzé kell tennie az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7979c-121">Next, download a server project for a simple "todo list" backend and publish it to Azure.</span></span>

<span data-ttu-id="7979c-122">Konfigurálja a kiszolgálóprojektet a Node.js vagy a .NET-háttéralkalmazás használatára az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="7979c-122">Follow the following steps to configure the server project to use either the Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a><span data-ttu-id="7979c-123">A Xamarin.iOS-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="7979c-123">Download and run the Xamarin.iOS app</span></span>
1. <span data-ttu-id="7979c-124">Nyissa meg az [Azure Portal] egy böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="7979c-124">Open the [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="7979c-125">A Mobile Apps beállítási paneljén kattintson az **Első lépések** > **Xamarin.iOS** elemre.</span><span class="sxs-lookup"><span data-stu-id="7979c-125">On the settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="7979c-126">A 3. lépésben kattintson az **Új alkalmazás létrehozása** lehetőségre, ha még nincs kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="7979c-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="7979c-127">Ezután kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7979c-127">Next click the **Download** button.</span></span>

      <span data-ttu-id="7979c-128">A program ekkor letölt egy, a mobilháttérmodulhoz csatlakozó ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7979c-128">A client application that connects to your mobile backend is downloaded.</span></span> <span data-ttu-id="7979c-129">Mentse el a tömörített projektfájlt a helyi számítógépen, és jegyezze fel a mentési helyét.</span><span class="sxs-lookup"><span data-stu-id="7979c-129">Save the compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="7979c-130">Bontsa ki a letöltött projektet, és nyissa meg a Xamarin Studio (vagy a Visual Studio) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7979c-130">Extract the project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="7979c-131">Készítse el a projekt buildjét az F5 billentyűt lenyomásával, és indítsa el az alkalmazást az iPhone-emulátoron.</span><span class="sxs-lookup"><span data-stu-id="7979c-131">Press the F5 key to build the project and start the app in the iPhone emulator.</span></span>
5. <span data-ttu-id="7979c-132">Az alkalmazásban írjon be egy értelmes szöveget, például *Xamarin-tanulás*, majd kattintson a **+** gombra.</span><span class="sxs-lookup"><span data-stu-id="7979c-132">In the app, type meaningful text, such as *Learn Xamarin*, and then click the **+** button.</span></span>

    ![][10]

    <span data-ttu-id="7979c-133">A kérelem adatai beillesztésre kerülnek a TodoItem táblába.</span><span class="sxs-lookup"><span data-stu-id="7979c-133">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="7979c-134">A táblában tárolt elemeket a mobil-háttéralkalmazás visszaküldi, és az adatok megjelennek a listában.</span><span class="sxs-lookup"><span data-stu-id="7979c-134">Items stored in the table are returned by the mobile app backend, and the data is displayed in the list.</span></span>

> [!NOTE]
> <span data-ttu-id="7979c-135">A mobil-háttéralkalmazás számára az adatok lekérdezéséhez és beszúrásához hozzáférést biztosító kódot a QSTodoService.cs C# fájlban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7979c-135">You can review the code that accesses your mobile app backend to query and insert data in the QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="7979c-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7979c-136">Next steps</span></span>
* [<span data-ttu-id="7979c-137">Offline szinkronizálás hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="7979c-137">Add Offline Sync to your app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="7979c-138">Hitelesítés hozzáadása az alkalmazáshoz </span><span class="sxs-lookup"><span data-stu-id="7979c-138">Add authentication to your app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="7979c-139">Leküldéses értesítések hozzáadása Xamarin.Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="7979c-139">Add push notifications to your Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="7979c-140">A felügyelt ügyfelek használata az Azure Mobile Apps-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="7979c-140">How to use the managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

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
<span data-ttu-id="7979c-141">[Azure Portal]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="7979c-141">[Azure portal]: https://portal.azure.com/</span></span>
