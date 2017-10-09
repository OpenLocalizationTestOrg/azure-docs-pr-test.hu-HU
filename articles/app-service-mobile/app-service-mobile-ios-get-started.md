---
title: "az iOS-alkalmazásoknak az Azure App Service Mobile Apps aaaCreate |} Microsoft Docs"
description: "Hajtsa végre az ezen oktatóanyag tooget az iOS-fejlesztésbe Objective-C vagy Swift Azure mobil-háttéralkalmazások használatával"
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
ms.openlocfilehash: 869fa971f7b5ab4a7119bbfa92808185d2ecdf8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ios-app"></a><span data-ttu-id="c87ce-103">iOS-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c87ce-103">Create an iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="c87ce-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c87ce-104">Overview</span></span>
<span data-ttu-id="c87ce-105">Ez az oktatóanyag bemutatja, hogyan tooadd [Azure Mobile Apps](app-service-mobile-value-prop.md), a felhőalapú háttérszolgáltatást, tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c87ce-105">This tutorial shows how tooadd [Azure Mobile Apps](app-service-mobile-value-prop.md), a cloud backend service, tooan iOS app.</span></span> <span data-ttu-id="c87ce-106">Először létre kell hoznunk egy új mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c87ce-106">We'll first create a new mobile backend.</span></span> <span data-ttu-id="c87ce-107">Ezt követően fogjuk használni egy egyszerű *teendőlista* iOS app toostore adatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c87ce-107">Then, we'll use a simple *Todo list* iOS app toostore data in Azure.</span></span>

<span data-ttu-id="c87ce-108">toocomplete ebben az oktatóanyagban egy Mac van szüksége, és [az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="c87ce-108">toocomplete this tutorial, you need a Mac and [an Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>

## <a name="step-i-create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="c87ce-109">1. lépés: Új Azure Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c87ce-109">Step I: Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-hello-backend-project"></a><span data-ttu-id="c87ce-110">II. lépés: Hello háttéralkalmazás-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c87ce-110">Step II: Configure hello backend project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-hello-ios-app"></a><span data-ttu-id="c87ce-111">III. lépés: Töltse le és hello iOS-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="c87ce-111">Step III: Download and run hello iOS app</span></span>
[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
