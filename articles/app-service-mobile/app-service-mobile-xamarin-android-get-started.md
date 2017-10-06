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
# <a name="create-a-xamarinandroid-app"></a>Xamarin.Android-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás tooa Xamarin.Android-alkalmazást. További információ: [Mi a Mobile Apps szolgáltatás?](app-service-mobile-value-prop.md).

Befejeződött hello alkalmazás képernyőképe látható alatt van:

![][0]

Az oktatóanyag végrehajtása feltétele a Mobile Apps Xamarin.Android-alkalmazásokra vonatkozó összes többi oktatóanyag elérésének.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és too10 szabad Mobile Apps létrehozásához. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio és Xamarin. Az útmutatót lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobile Apps-háttéralkalmazás létrehozása
Kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak. Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a>Hello Xamarin.Android-alkalmazás letöltése és futtatása
1. A **letöltése és futtatása a Xamarin.Android-projekt**, kattintson a hello **letöltése** gombra.

      Mentse a hello tömörített projekt fájl tooyour helyi számítógépen, és jegyezze fel a mentési helyét.
2. Nyomja le az hello **F5** toobuild hello projekt kulcs és hello alkalmazás indításához.
3. Hello alkalmazásban írjon be egy értelmes szöveget, például *teljes hello oktatóanyag* majd hello **Hozzáadás** gombra.

    ![][10]

    Hello kérelemből adatok bekerülnek hello TodoItem tábla. Mobil-háttéralkalmazás hello által visszaadott hello táblában tárolt elemeket, és az adatok hello listájában jelenik meg.

   > [!NOTE]
   > Megtekintheti a mobilalkalmazás-háttérrendszer tooquery hozzáférő hello kódot, és helyezze be az adatokat, amelyek hello ToDoActivity.cs C# fájlban található.
   >
   >

## <a name="next-steps"></a>Következő lépések
* [Kapcsolat nélküli szinkronizálás tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-android-get-started-users.md)
* [Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása](app-service-mobile-xamarin-android-get-started-push.md)
* [Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
