---
title: "iOS-alkalmazás létrehozása az Azure App Service Mobile Apps szolgáltatásban| Microsoft Docs"
description: "Az útmutató bevezeti Önt az Azure-alapú mobil-háttéralkalmazások használatával megvalósítható, Objective-C vagy Swift nyelven történő iOS-fejlesztésbe."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 36936ae66c458fcbedeec95cfa2f573a40c8af53
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-ios-app"></a><span data-ttu-id="98f83-103">iOS-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="98f83-103">Create an iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="98f83-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="98f83-104">Overview</span></span>
<span data-ttu-id="98f83-105">Az útmutató azt mutatja be, hogyan adhatja hozzá az [Azure Mobile Apps](app-service-mobile-value-prop.md) felhőalapú háttérszolgáltatást az iOS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="98f83-105">This tutorial shows how to add [Azure Mobile Apps](app-service-mobile-value-prop.md), a cloud backend service, to an iOS app.</span></span> <span data-ttu-id="98f83-106">Először létre kell hoznunk egy új mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="98f83-106">We'll first create a new mobile backend.</span></span> <span data-ttu-id="98f83-107">Ezt követően egy egyszerű *Tennivalólista* iOS-alkalmazást fogunk használni az adatoknak az Azure-ban való tárolására.</span><span class="sxs-lookup"><span data-stu-id="98f83-107">Then, we'll use a simple *Todo list* iOS app to store data in Azure.</span></span>

<span data-ttu-id="98f83-108">Az oktatóanyag elvégzéséhez egy Mac gépre és [egy Azure-fiókra](https://azure.microsoft.com/pricing/free-trial/) lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="98f83-108">To complete this tutorial, you need a Mac and [an Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>

## <a name="step-i-create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="98f83-109">1. lépés: Új Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="98f83-109">Step I: Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-the-backend-project"></a><span data-ttu-id="98f83-110">2. lépés: A háttéralkalmazás-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98f83-110">Step II: Configure the backend project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-the-ios-app"></a><span data-ttu-id="98f83-111">3. lépés: Az iOS-alkalmazás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="98f83-111">Step III: Download and run the iOS app</span></span>
[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
