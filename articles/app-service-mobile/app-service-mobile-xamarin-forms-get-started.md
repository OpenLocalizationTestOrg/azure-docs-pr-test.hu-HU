---
title: "aaaGet használatába a Mobile Apps Xamarin.Forms használatával"
description: "Hajtsa végre az ezen oktatóanyag toostart használhatja a Mobile Apps Xamarin.Forms-fejlesztési"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a>Xamarin.Forms-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd egy felhőalapú háttér-szolgáltatás tooa Xamarin.Forms mobilalkalmazás használatával hello hello háttér, Azure App Service Mobile Apps szolgáltatásának. Létrehoz egy új Mobile Apps-háttéralkalmazást, illetve egy olyan egyszerű Xamarin.Forms-alapú teendőlista alkalmazást, amely az alkalmazásadatait az Azure-ban tárolja.

Az oktatóanyag végrehajtása feltétele a Mobile Apps Xamarin.Forms-alkalmazásokra vonatkozó összes többi oktatóanyagának elérésének.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat. További információk: [Ingyenes Azure-próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio és Xamarin. Információ: hello [állítsa össze, és telepítse a Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) lap.

* Mac számítógép 7.0 vagy-s újabb verziójú Xcode-dal és Xamarin Studio Communityvel. További információk: [A Visual Studio és a Xamarin beállítása és telepítése](https://msdn.microsoft.com/library/mt613162.aspx) és [Beállítás, telepítés és ellenőrzés Mac felhasználók részére](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Új Mobile Apps-háttéralkalmazás létrehozása

vissza egy új Mobile Apps leállításához toocreate hello a következő:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Ezennel beállított egy Mobile Apps-háttéralkalmazást, amelyet a mobilos ügyfélalkalmazásai használhatnak. Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű Tennivalólista lista háttér és tooAzure közzététele.

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása

tooconfigure hello server projekt toouse hello Node.js vagy .NET háttér, a következő hello:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a>Hello Xamarin.Forms-megoldás letöltése és futtatása

Hello megoldás két módon töltheti le. Tooa Mac töltse le és nyissa meg a Xamarin Studio vagy tooa Windows rendszerű számítógépen töltse le és nyissa meg a Visual Studio egy hálózati Mac használatával történő megtervezésével hello iOS-alkalmazás. További információk: [A Visual Studio és a Xamarin beállítása és telepítése](https://msdn.microsoft.com/library/mt613162.aspx).

Az hello a Mac vagy Windows-számítógépen a következő:

1. Nyissa meg toohello [Azure-portálon].

2. A hello **beállítások** mobil alkalmazás, a **Mobile**, jelölje be **Ismerkedés** > **Xamarin.Forms**. A **3. lépésben** válassza az **Új alkalmazás létrehozása**, majd a **Letöltés** lehetőséget.

   Ez a művelet egy projekt, amely tartalmaz egy ügyfélalkalmazást, amely csatlakoztatott tooyour mobilalkalmazás tölti le. Mentse a hello tömörített projekt fájl tooyour helyi számítógépen, és jegyezze fel a mentési helyét.

3. Bontsa ki a letöltött hello projektet, és nyissa meg a Xamarin Studio (Mac) vagy a Visual Studio (Windows).

   ![Kibontott projekt a Xamarin Studióban][9]

   ![Kibontott projekt a Visual Studióban][8]

## <a name="optional-run-hello-ios-project"></a>(Választható) Hello iOS-projekt futtatása
Ebben a szakaszban hello Xamarin iOS-projektet az iOS-eszközök futtatása. Kihagyhatja ezt a részt, ha nem dolgozik iOS-eszközökkel.

#### <a name="in-xamarin-studio"></a>Xamarin Studióban
1. Kattintson a jobb gombbal a hello iOS-projektre, majd válassza ki **beállítása szerint Startup Project**.

2. A hello **futtatása** menüjében válassza **Start Debugging** toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban.

#### <a name="in-visual-studio"></a>Visual Studióban
1. Kattintson a jobb gombbal a hello iOS-projektre, majd válassza ki **beállítás kezdőprojektként**.

2. A hello **Build** menü **Configuration Manager**.

3. A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello iOS-projektet.

4. toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban, jelölje be hello **F5** kulcs.

   > [!NOTE]
   > Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag. Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.    
   >
   >

5. Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).

    ![][10]

    Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello. Hello kérelemből adatok bekerülnek hello TodoItem tábla. Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.

    > [!NOTE]
    > Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.
    >
    >

## <a name="optional-run-hello-android-project"></a>(Választható) Hello Android-projekt futtatása
Ebben a szakaszban a hello Xamarin droid-projekt az Android futtatja. Kihagyhatja ezt a részt, ha nem dolgozik Android-eszközökkel.

#### <a name="in-xamarin-studio"></a>Xamarin Studióban

1. Kattintson a jobb gombbal a hello Android-projekt, majd válassza ki **beállítása szerint Startup Project**.

2. toobuild hello projektet, és indítsa el a hello alkalmazást egy Android-emulátorban, a hello **futtatása** menü **Start Debugging**.

#### <a name="in-visual-studio"></a>Visual Studióban

1. Kattintson a jobb gombbal a hello (Droid) Android-projektet, majd válassza ki **beállítás kezdőprojektként**.

2. A hello **Build** menü **Configuration Manager**.

3. A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello Androidos projekt.

4. toobuild hello projektet, és indítsa el a hello alkalmazást egy Android-emulátorban, jelölje be hello **F5** kulcs.

   > [!NOTE]
   > Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag. Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.    
   >
   >

5. Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).

    ![][11]
    
    Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello. Hello kérelemből adatok bekerülnek hello TodoItem tábla. Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.
    
    > [!NOTE]
    > Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.
    >
    >

## <a name="optional-run-hello-windows-project"></a>(Választható) Futtassa a Windows hello-projekt

Ebben a szakaszban hello Xamarin WinApp-projektjét, amely a Windows-eszközök futtatása. Kihagyhatja ezt a részt, ha nem dolgozik Windows-eszközökkel.

#### <a name="in-visual-studio"></a>Visual Studióban

1. Kattintson a jobb gombbal bármelyik Windows hello-projektek egyikére, majd válassza ki **beállítás kezdőprojektként**.

2. A hello **Build** menü **Configuration Manager**.

3. A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello Windows-projekt választott.

4. toobuild hello projektet, és indítsa el a hello alkalmazást egy Windows-emulátorban, jelölje be hello **F5** kulcs.

   > [!NOTE]
   > Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag. Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.    
   >
   >

5. Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).

    Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello. Hello kérelemből adatok bekerülnek hello TodoItem tábla. Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.
    
    ![][12]
    
    > [!NOTE]
    > Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.
    >
    >

## <a name="next-steps"></a>Következő lépések

* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-forms-get-started-users.md)  
  Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.

* [Leküldéses értesítések tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-forms-get-started-push.md)  
  Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a Mobile Apps háttér toouse Azure Notification Hubs toosend hello leküldéses értesítések konfigurálása.

* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Megtudhatja, hogyan háttér tooadd offline támogatást az alkalmazásához egy Mobile Apps használatával. A kapcsolat nélküli szinkronizálás lehetővé teszi a mobilalkalmazás adatainak megtekintését, hozzáadását és módosítását akkor is, ha nincs hálózati kapcsolat.

* [A Mobile Apps hello felügyelt ügyfelek használata](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Ismerje meg, hogyan toowork hello való kezelése a Xamarin-alkalmazás ügyfél-SDK.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-portálon]: https://portal.azure.com/
