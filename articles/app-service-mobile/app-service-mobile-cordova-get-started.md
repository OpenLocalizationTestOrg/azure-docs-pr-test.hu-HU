---
title: "Cordova-alkalmazás létrehozása az Azure App Service Mobile Apps szolgáltatásban| Microsoft Docs"
description: "Az útmutató bevezeti Önt az Azure-alapú mobil-háttéralkalmazások használatával megvalósítható, Apache Cordova keretrendszerben történő fejlesztésbe."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
tags: 
keywords: "cordova,javascript,mobil,ügyfél"
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 07/07/2017
ms.author: glenga
ms.openlocfilehash: b620465cdc3cfa04933dc6e70163fc32aa9a839b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="48b73-104">Apache Cordova-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48b73-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="48b73-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="48b73-105">Overview</span></span>
<span data-ttu-id="48b73-106">Ez az oktatóanyag azt ismerteti, hogyan adhat felhőalapú háttérszolgáltatásokat az Apache Cordova-mobilalkalmazásokhoz egy Azure-alapú mobil-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="48b73-106">This tutorial shows you how to add a cloud-based backend service to an Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="48b73-107">A folyamat során létrehoz egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Apache Cordova-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="48b73-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="48b73-108">Az oktatóanyag végrehajtása feltétele az Azure App Service Mobile Apps szolgáltatásának használatát ismertető többi Apache Cordova-oktatóanyag megértésének.</span><span class="sxs-lookup"><span data-stu-id="48b73-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48b73-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48b73-109">Prerequisites</span></span>
<span data-ttu-id="48b73-110">Az oktatóanyag teljesítéséhez a következő előfeltételekre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="48b73-110">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="48b73-111">Számítógép, amelyen fut a [Visual Studio Community 2017] vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="48b73-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="48b73-112">[Visual Studio Tools for Apache Cordova].</span><span class="sxs-lookup"><span data-stu-id="48b73-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="48b73-113">[Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48b73-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="48b73-114">Közvetlenül az Apache Cordova parancssorának használata esetén nincs szükség a Visual Studióra.</span><span class="sxs-lookup"><span data-stu-id="48b73-114">You may also bypass Visual Studio and use the Apache Cordova command line directly.</span></span>  <span data-ttu-id="48b73-115">A parancssor használata akkor hasznos, ha az oktatóanyagot Mac gépen szeretné elvégezni.</span><span class="sxs-lookup"><span data-stu-id="48b73-115">Using the command line is useful when completing the tutorial on a Mac computer.</span></span>  <span data-ttu-id="48b73-116">Az oktatóanyag nem tér ki arra, hogy miképpen lehet az Apache Cordova-ügyfélalkalmazásokat parancssor segítségével lefordítani.</span><span class="sxs-lookup"><span data-stu-id="48b73-116">Compiling Apache Cordova client applications using the command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="48b73-117">Azure mobil-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48b73-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="48b73-118">Tekintse meg hasonló lépéseket ismertető videót</span><span class="sxs-lookup"><span data-stu-id="48b73-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a><span data-ttu-id="48b73-119">Kiszolgálóprojekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48b73-119">Configure the server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a><span data-ttu-id="48b73-120">Az Apache Cordova-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="48b73-120">Download and run the Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="48b73-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48b73-121">Next Steps</span></span>
<span data-ttu-id="48b73-122">Ha elvégezte ezt a bevezető oktatóanyagot, lépjen tovább valamelyik további anyagra:</span><span class="sxs-lookup"><span data-stu-id="48b73-122">Now that you completed this quick start tutorial, move on to one of the following tutorials:</span></span>

* <span data-ttu-id="48b73-123">[Offline adatok hozzáadása](app-service-mobile-cordova-get-started-offline-data.md) az Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="48b73-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="48b73-124">[Hitelesítés hozzáadása](app-service-mobile-cordova-get-started-users.md) az Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="48b73-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="48b73-125">[Leküldéses értesítések hozzáadása](app-service-mobile-cordova-get-started-push.md) az Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="48b73-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) to your Apache Cordova app.</span></span>

<span data-ttu-id="48b73-126">További információk az Azure App Service alapjairól.</span><span class="sxs-lookup"><span data-stu-id="48b73-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="48b73-127">[Offline adatok]</span><span class="sxs-lookup"><span data-stu-id="48b73-127">[Offline Data]</span></span>
* <span data-ttu-id="48b73-128">[Hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="48b73-128">[Authentication]</span></span>
* <span data-ttu-id="48b73-129">[Leküldéses értesítések]</span><span class="sxs-lookup"><span data-stu-id="48b73-129">[Push Notifications]</span></span>

<span data-ttu-id="48b73-130">Útmutató az SDK-k használatához.</span><span class="sxs-lookup"><span data-stu-id="48b73-130">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="48b73-131">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="48b73-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="48b73-132">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="48b73-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="48b73-133">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="48b73-133">[Node.js Server SDK]</span></span>

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
<span data-ttu-id="48b73-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="48b73-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="48b73-135">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="48b73-135">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
<span data-ttu-id="48b73-136">[Offline adatok]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="48b73-136">[Offline Data]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="48b73-137">[Hitelesítés]: app-service-mobile-auth.md</span><span class="sxs-lookup"><span data-stu-id="48b73-137">[Authentication]: app-service-mobile-auth.md</span></span>
<span data-ttu-id="48b73-138">[Leküldéses értesítések]: ../notification-hubs/notification-hubs-push-notification-overview.md</span><span class="sxs-lookup"><span data-stu-id="48b73-138">[Push Notifications]: ../notification-hubs/notification-hubs-push-notification-overview.md</span></span>
<span data-ttu-id="48b73-139">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span><span class="sxs-lookup"><span data-stu-id="48b73-139">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span></span>
<span data-ttu-id="48b73-140">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="48b73-140">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span></span>
<span data-ttu-id="48b73-141">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="48b73-141">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span></span>
