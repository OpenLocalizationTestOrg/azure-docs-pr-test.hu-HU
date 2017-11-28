---
title: "aaaGet elindítva az Azure Mobile Apps Xamarin.Android-alkalmazásokhoz"
description: "Hajtsa végre az ezen oktatóanyag tooget Xamarin Android-fejlesztések az Azure Mobile Apps használatának megkezdése"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 38710660d9328fe3c068efca972f76aa8b6e049b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinandroid-app"></a><span data-ttu-id="a34ed-103">Xamarin.Android-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a34ed-103">Create a Xamarin.Android App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="a34ed-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a34ed-104">Overview</span></span>
<span data-ttu-id="a34ed-105">Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás tooa Xamarin.Android-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a34ed-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.Android app.</span></span> <span data-ttu-id="a34ed-106">További információ: [Mi a Mobile Apps szolgáltatás?](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="a34ed-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span>

<span data-ttu-id="a34ed-107">Befejeződött hello alkalmazás képernyőképe látható alatt van:</span><span class="sxs-lookup"><span data-stu-id="a34ed-107">A screenshot from hello completed app is below:</span></span>

![][0]

<span data-ttu-id="a34ed-108">Az oktatóanyag végrehajtása feltétele a Mobile Apps Xamarin.Android-alkalmazásokra vonatkozó összes többi oktatóanyag elérésének.</span><span class="sxs-lookup"><span data-stu-id="a34ed-108">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a34ed-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a34ed-109">Prerequisites</span></span>
<span data-ttu-id="a34ed-110">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a34ed-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="a34ed-111">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a34ed-111">An active Azure account.</span></span> <span data-ttu-id="a34ed-112">Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és too10 szabad Mobile Apps létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a34ed-112">If you don't have an account, sign up for an Azure trial and get up too10 free Mobile Apps.</span></span> <span data-ttu-id="a34ed-113">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a34ed-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a34ed-114">Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="a34ed-114">Visual Studio with Xamarin.</span></span> <span data-ttu-id="a34ed-115">Az útmutatót lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="a34ed-115">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="a34ed-116">Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a34ed-116">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="a34ed-117">Kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a34ed-117">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="a34ed-118">Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak.</span><span class="sxs-lookup"><span data-stu-id="a34ed-118">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="a34ed-119">Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="a34ed-119">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="a34ed-120">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a34ed-120">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a><span data-ttu-id="a34ed-121">Hello Xamarin.Android-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="a34ed-121">Download and run hello Xamarin.Android app</span></span>
1. <span data-ttu-id="a34ed-122">A **letöltése és futtatása a Xamarin.Android-projekt**, kattintson a hello **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a34ed-122">Under **Download and run your Xamarin.Android project**, click hello **Download** button.</span></span>

      <span data-ttu-id="a34ed-123">Mentse a hello tömörített projekt fájl tooyour helyi számítógépen, és jegyezze fel a mentési helyét.</span><span class="sxs-lookup"><span data-stu-id="a34ed-123">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>
2. <span data-ttu-id="a34ed-124">Nyomja le az hello **F5** toobuild hello projekt kulcs és hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="a34ed-124">Press hello **F5** key toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="a34ed-125">Hello alkalmazásban írjon be egy értelmes szöveget, például *teljes hello oktatóanyag* majd hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a34ed-125">In hello app, type meaningful text, such as *Complete hello tutorial* and then click hello **Add** button.</span></span>

    ![][10]

    <span data-ttu-id="a34ed-126">Hello kérelemből adatok bekerülnek hello TodoItem tábla.</span><span class="sxs-lookup"><span data-stu-id="a34ed-126">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="a34ed-127">Mobil-háttéralkalmazás hello által visszaadott hello táblában tárolt elemeket, és az adatok hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a34ed-127">Items stored in hello table are returned by hello mobile app backend, and the data appears in hello list.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a34ed-128">Megtekintheti a mobilalkalmazás-háttérrendszer tooquery hozzáférő hello kódot, és helyezze be az adatokat, amelyek hello ToDoActivity.cs C# fájlban található.</span><span class="sxs-lookup"><span data-stu-id="a34ed-128">You can review hello code that accesses your mobile app backend tooquery and insert data, which is found in hello ToDoActivity.cs C# file.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="a34ed-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a34ed-129">Next steps</span></span>
* [<span data-ttu-id="a34ed-130">Kapcsolat nélküli szinkronizálás tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a34ed-130">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [<span data-ttu-id="a34ed-131">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a34ed-131">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-android-get-started-users.md)
* [<span data-ttu-id="a34ed-132">Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a34ed-132">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-android-get-started-push.md)
* [<span data-ttu-id="a34ed-133">Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél</span><span class="sxs-lookup"><span data-stu-id="a34ed-133">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
