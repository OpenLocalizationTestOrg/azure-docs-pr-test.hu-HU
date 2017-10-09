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
# <a name="create-an-android-app"></a>Android-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooan Android rendszerhez készült mobilalkalmazás.  Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Android-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.

Az oktatóanyag végrehajtása feltétele az összes többi Androidra vonatkozó oktatóanyagokra hello Mobile Apps szolgáltatás használatáról az Azure App Service-ben.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő hello szüksége:

* [Android Developer Tools](https://developer.android.com/sdk/index.html), amely magában foglalja a hello Android Studio integrált fejlesztőkörnyezetet és hello legújabb Android platformot.
* Az Azure Mobile Android SDK, amelyre a letöltött gyorsútmutató-projekt hello részeként automatikusan hivatkozik.
* [Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Új Azure Mobile Apps-háttéralkalmazás létrehozása
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-android-app"></a>Hello Android-alkalmazás letöltése és futtatása
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
