---
title: "a Cordova-alkalmazás az Azure App Service Mobile Apps aaaCreate |} Microsoft Docs"
description: "Hajtsa végre az Azure mobil-háttéralkalmazások használatával megvalósítható az Apache Cordova-fejlesztői útmutató tooget"
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
ms.openlocfilehash: 4981cbc0686c15230019ac9f30495f30cbf2c791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="66c85-104">Apache Cordova-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="66c85-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="66c85-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="66c85-105">Overview</span></span>
<span data-ttu-id="66c85-106">Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooan Apache Cordova-mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="66c85-106">This tutorial shows you how tooadd a cloud-based backend service tooan Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="66c85-107">A folyamat során létrehoz egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Apache Cordova-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="66c85-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="66c85-108">Az oktatóanyag végrehajtása feltétele az ismertető többi Apache Cordova-oktatóanyag az Azure App Service hello Mobile Apps szolgáltatás használatáról.</span><span class="sxs-lookup"><span data-stu-id="66c85-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66c85-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66c85-109">Prerequisites</span></span>
<span data-ttu-id="66c85-110">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="66c85-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="66c85-111">Számítógép, amelyen fut a [Visual Studio Community 2017] vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="66c85-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="66c85-112">[Visual Studio Tools for Apache Cordova].</span><span class="sxs-lookup"><span data-stu-id="66c85-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="66c85-113">[Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66c85-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="66c85-114">Is szükség a Visual Studióra, és közvetlenül a hello Apache Cordova parancssorának használata.</span><span class="sxs-lookup"><span data-stu-id="66c85-114">You may also bypass Visual Studio and use hello Apache Cordova command line directly.</span></span>  <span data-ttu-id="66c85-115">Hello parancssorból akkor hasznos, ha egy Mac számítógépen hello az oktatóanyagnak az elvégzése.</span><span class="sxs-lookup"><span data-stu-id="66c85-115">Using hello command line is useful when completing hello tutorial on a Mac computer.</span></span>  <span data-ttu-id="66c85-116">Ez az oktatóanyag nem vonatkozik az Apache Cordova-ügyfélalkalmazásokat hello parancssor segítségével lefordítani.</span><span class="sxs-lookup"><span data-stu-id="66c85-116">Compiling Apache Cordova client applications using hello command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="66c85-117">Azure mobil-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="66c85-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="66c85-118">Tekintse meg hasonló lépéseket ismertető videót</span><span class="sxs-lookup"><span data-stu-id="66c85-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a><span data-ttu-id="66c85-119">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66c85-119">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a><span data-ttu-id="66c85-120">Hello Apache Cordova-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="66c85-120">Download and run hello Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="66c85-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66c85-121">Next Steps</span></span>
<span data-ttu-id="66c85-122">Most, hogy a gyors üzembe helyezési útmutató befejezte, helyezze át, amely az alábbi oktatóanyagok hello tooone:</span><span class="sxs-lookup"><span data-stu-id="66c85-122">Now that you completed this quick start tutorial, move on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="66c85-123">[Kapcsolat nélküli adatok hozzáadása a](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="66c85-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="66c85-124">[Hitelesítés hozzáadása](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="66c85-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="66c85-125">[Leküldéses értesítések hozzáadása](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="66c85-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova app.</span></span>

<span data-ttu-id="66c85-126">További információk az Azure App Service alapjairól.</span><span class="sxs-lookup"><span data-stu-id="66c85-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="66c85-127">[Offline adatok]</span><span class="sxs-lookup"><span data-stu-id="66c85-127">[Offline Data]</span></span>
* <span data-ttu-id="66c85-128">[Hitelesítés]</span><span class="sxs-lookup"><span data-stu-id="66c85-128">[Authentication]</span></span>
* <span data-ttu-id="66c85-129">[Leküldéses értesítések]</span><span class="sxs-lookup"><span data-stu-id="66c85-129">[Push Notifications]</span></span>

<span data-ttu-id="66c85-130">Ismerje meg, hogyan toouse hello SDK-k.</span><span class="sxs-lookup"><span data-stu-id="66c85-130">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="66c85-131">[Apache Cordova SDK]</span><span class="sxs-lookup"><span data-stu-id="66c85-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="66c85-132">[ASP.NET Server SDK]</span><span class="sxs-lookup"><span data-stu-id="66c85-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="66c85-133">[Node.js Server SDK]</span><span class="sxs-lookup"><span data-stu-id="66c85-133">[Node.js Server SDK]</span></span>

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Offline adatok]: app-service-mobile-offline-data-sync.md
[Hitelesítés]: app-service-mobile-auth.md
[Leküldéses értesítések]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
