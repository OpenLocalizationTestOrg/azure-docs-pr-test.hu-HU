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
# <a name="create-an-apache-cordova-app"></a>Apache Cordova-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooan Apache Cordova-mobilalkalmazás.  A folyamat során létrehoz egy új mobil-háttéralkalmazást, illetve egy egyszerű *Tennivalólista* Apache Cordova-alkalmazást, amely alkalmazásadatokat tárol az Azure-ban.

Az oktatóanyag végrehajtása feltétele az ismertető többi Apache Cordova-oktatóanyag az Azure App Service hello Mobile Apps szolgáltatás használatáról.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:

* Számítógép, amelyen fut a [Visual Studio Community 2017] vagy újabb verzió.
* [Visual Studio Tools for Apache Cordova].
* [Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).

Is szükség a Visual Studióra, és közvetlenül a hello Apache Cordova parancssorának használata.  Hello parancssorból akkor hasznos, ha egy Mac számítógépen hello az oktatóanyagnak az elvégzése.  Ez az oktatóanyag nem vonatkozik az Apache Cordova-ügyfélalkalmazásokat hello parancssor segítségével lefordítani.

## <a name="create-an-azure-mobile-app-backend"></a>Azure mobil-háttéralkalmazás létrehozása
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Tekintse meg hasonló lépéseket ismertető videót](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a>Hello Apache Cordova-alkalmazás letöltése és futtatása
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy a gyors üzembe helyezési útmutató befejezte, helyezze át, amely az alábbi oktatóanyagok hello tooone:

* [Kapcsolat nélküli adatok hozzáadása a](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova-alkalmazáshoz.
* [Hitelesítés hozzáadása](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova-alkalmazáshoz.
* [Leküldéses értesítések hozzáadása](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova-alkalmazáshoz.

További információk az Azure App Service alapjairól.

* [Offline adatok]
* [Hitelesítés]
* [Leküldéses értesítések]

Ismerje meg, hogyan toouse hello SDK-k.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

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
