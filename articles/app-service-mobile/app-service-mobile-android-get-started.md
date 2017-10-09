---
title: "Android-alkalmazás az Azure App Service Mobile Apps aaaCreate |} Microsoft Docs"
description: "Hajtsa végre az ezen oktatóanyag tooget Azure mobil-háttéralkalmazások használatával megvalósítható az Android-fejlesztések"
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
ms.openlocfilehash: 0af85a3a4de9fc265976bbe3f34d73effc3807df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-android-app"></a><span data-ttu-id="ed8b2-103">Android-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed8b2-103">Create an Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="ed8b2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ed8b2-104">Overview</span></span>
<span data-ttu-id="ed8b2-105">Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooan Android rendszerhez készült mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ed8b2-105">This tutorial shows you how tooadd a cloud-based backend service tooan Android mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="ed8b2-106">Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Android-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ed8b2-106">You will create both a new mobile app backend and a simple *Todo list* Android app that stores app data in Azure.</span></span>

<span data-ttu-id="ed8b2-107">Az oktatóanyag végrehajtása feltétele az összes többi Androidra vonatkozó oktatóanyagokra hello Mobile Apps szolgáltatás használatáról az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="ed8b2-107">Completing this tutorial is a prerequisite for all other Android tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed8b2-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ed8b2-108">Prerequisites</span></span>
<span data-ttu-id="ed8b2-109">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ed8b2-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="ed8b2-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), amely magában foglalja a hello Android Studio integrált fejlesztőkörnyezetet és hello legújabb Android platformot.</span><span class="sxs-lookup"><span data-stu-id="ed8b2-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>
* <span data-ttu-id="ed8b2-111">Az Azure Mobile Android SDK, amelyre a letöltött gyorsútmutató-projekt hello részeként automatikusan hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ed8b2-111">Azure Mobile Android SDK, which is automatically referenced as part of hello quickstart project you download.</span></span>
* <span data-ttu-id="ed8b2-112">[Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ed8b2-112">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="ed8b2-113">Új Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed8b2-113">Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="ed8b2-114">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ed8b2-114">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-android-app"></a><span data-ttu-id="ed8b2-115">Hello Android-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="ed8b2-115">Download and run hello Android app</span></span>
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
