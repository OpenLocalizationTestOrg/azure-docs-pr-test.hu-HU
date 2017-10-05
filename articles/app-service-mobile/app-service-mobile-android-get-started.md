---
title: "Android-alkalmazás létrehozása az Azure App Service Mobile Apps szolgáltatásban| Microsoft Docs"
description: "Az útmutató bevezeti Önt az Azure-alapú mobil-háttéralkalmazások használatával megvalósítható, Android rendszerben történő fejlesztésbe."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 418a5229a084d570bc6cab5925dbd8d30945a3c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-android-app"></a><span data-ttu-id="8e11a-103">Android-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e11a-103">Create an Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="8e11a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8e11a-104">Overview</span></span>
<span data-ttu-id="8e11a-105">Ez az oktatóanyag azt ismerteti, hogyan adhat felhőalapú háttérszolgáltatásokat az Android-mobilalkalmazásokhoz egy Azure-alapú mobil-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="8e11a-105">This tutorial shows you how to add a cloud-based backend service to an Android mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="8e11a-106">Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Android-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8e11a-106">You will create both a new mobile app backend and a simple *Todo list* Android app that stores app data in Azure.</span></span>

<span data-ttu-id="8e11a-107">Az oktatóanyag végrehajtása feltétele az Azure App Service Mobile Apps szolgáltatásának használatát ismertető többi Android-oktatóanyag megértésének.</span><span class="sxs-lookup"><span data-stu-id="8e11a-107">Completing this tutorial is a prerequisite for all other Android tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e11a-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8e11a-108">Prerequisites</span></span>
<span data-ttu-id="8e11a-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="8e11a-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="8e11a-110">[Android Developer Tools](https://developer.android.com/sdk/index.html): ez a csomag tartalmazza az androidos integrált fejlesztőkörnyezetet, valamint a legújabb Android-platformot.</span><span class="sxs-lookup"><span data-stu-id="8e11a-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), which includes the Android Studio integrated development environment, and the latest Android platform.</span></span>
* <span data-ttu-id="8e11a-111">Azure Mobile Android SDK, amelyre a rendszer automatikusan hivatkozik a letöltött bevezető gyorssablonos projekt letöltésekor.</span><span class="sxs-lookup"><span data-stu-id="8e11a-111">Azure Mobile Android SDK, which is automatically referenced as part of the quickstart project you download.</span></span>
* <span data-ttu-id="8e11a-112">[Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8e11a-112">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="8e11a-113">Új Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e11a-113">Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a><span data-ttu-id="8e11a-114">Kiszolgálóprojekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e11a-114">Configure the server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a><span data-ttu-id="8e11a-115">Az Android-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="8e11a-115">Download and run the Android app</span></span>
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
