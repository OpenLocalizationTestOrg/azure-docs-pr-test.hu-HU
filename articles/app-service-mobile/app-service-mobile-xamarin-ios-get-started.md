---
title: "a Xamarin.iOS-alkalmazásokhoz az Azure App Service Mobile Apps lépései aaaGet |} Microsoft Docs"
description: "Hajtsa végre az oktatóanyag tooget az Xamarin.iOS fejlesztési Mobile Apps használatával."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a>Xamarin.iOS-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás az Azure mobile Apps-háttéralkalmazás segítségével tooa Xamarin.iOS mobilalkalmazás.  Létre fog hozni egy új mobil-háttéralkalmazást, illetve egy olyan egyszerű *Teendőlista* Xamarin.iOS-alkalmazást, amely az alkalmazásadatokat az Azure-ban tárolja.

Az oktatóanyag végrehajtása feltétele az összes többi Xamarin.iOS-oktatóanyag az Azure App Service hello Mobile Apps szolgáltatás használatáról.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio és Xamarin. Az útmutatót lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).
* Mac számítógép 7.0 vagy-s újabb verziójú Xcode-dal és Xamarin Studio Communityvel. Lásd: [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése) és [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Beállítás, telepítés és ellenőrzés Macintosh-felhasználók számára) (MSDN).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobile Apps-háttéralkalmazás létrehozása
Kövesse ezeket a lépéseket toocreate a Mobile Apps-háttéralkalmazás.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása
Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak. Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.

Hajtsa végre a következő lépéseket tooconfigure hello server projekt toouse hello vagy hello Node.js vagy a .NET-háttérrendszer.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a>Hello Xamarin.iOS-alkalmazás letöltése és futtatása
1. Nyissa meg hello [Azure-portálon] egy böngészőablakban.
2. A Mobile Apps hello beállítási paneljén kattintson **Ismerkedés** > **Xamarin.iOS**. A 3. lépésben kattintson az **Új alkalmazás létrehozása** lehetőségre, ha még nincs kiválasztva.  Ezután kattintson a hello **letöltése** gombra.

      Egy ügyfélalkalmazást, amely a mobil-háttéralkalmazást tooyour lesznek letöltve. Mentse a hello tömörített projektfájlt a helyi számítógépen, és jegyezze fel a mentési helyét.
3. Bontsa ki a letöltött hello projektet, és nyissa meg a Xamarin Studio (vagy a Visual Studio).

    ![][9]

    ![][8]
4. Nyomja le az ENTER hello F5 kulcs toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban.
5. Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, majd kattintson a hello  **+**  gombra.

    ![][10]

    Hello kérelemből adatok bekerülnek hello TodoItem tábla. Mobil-háttéralkalmazás hello által visszaadott hello táblában tárolt elemeket, és az adatok hello listája jelenik meg.

> [!NOTE]
> Megtekintheti a mobilalkalmazás-háttérrendszer tooquery hozzáférő hello kódot, és adatokat beszúrni hello QSTodoService.cs C# fájlban.
>
>

## <a name="next-steps"></a>Következő lépések
* [Kapcsolat nélküli szinkronizálás tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-ios-get-started-users.md)
* [Leküldéses értesítések tooyour Xamarin.Android-alkalmazás hozzáadása](app-service-mobile-xamarin-ios-get-started-push.md)
* [Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portálon]: https://portal.azure.com/
