---
title: "egy univerzális Windows Platform-(UWP), amely azonosítja a Mobile Apps aaaCreate |} Microsoft Docs"
description: "Hajtsa végre az oktatóanyag tooget az univerzális Windows Platform (UWP) alkalmazások fejlesztésére C#, Visual Basic vagy JavaScript Azure mobil-háttéralkalmazások használatával."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a>Windows-alkalmazás létrehozása
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan tooadd felhőalapú háttérkiszolgálón szolgáltatás tooa univerzális Windows Platform (UWP) alkalmazást. További információ: [Mi a Mobile Apps szolgáltatás?](app-service-mobile-value-prop.md). hello az alábbiakban képernyőfelvételek befejeződött hello alkalmazásból:

![Az elkészült asztali alkalmazás](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Asztali számítógépen futó alkalmazás.

![Az elkészült mobilalkalmazás](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Telefonon futó alkalmazás.

Az oktatóanyag végrehajtása feltétele a Mobile Apps UWP-alkalmazásokra vonatkozó összes többi oktatóanyag elérésének.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Aktív Azure-fiók. Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio Community 2015] vagy újabb verzió.

## <a name="create-a-new-azure-mobile-app-backend"></a>Új Azure Mobile Apps-háttéralkalmazás létrehozása
Kövesse ezeket a lépéseket toocreate egy új mobil-háttéralkalmazást.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Már kiépített egy Azure Mobile Apps-háttérszolgáltatást, amelyet mobil ügyfélalkalmazásai használni tudnak. Ezután, amelyeket le fog tölteni egy kiszolgálóprojektet egy egyszerű "teendőlista" háttér és tooAzure közzététele.

## <a name="configure-hello-server-project"></a>Hello server projekt konfigurálása
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a>Töltse le és futtassa a hello ügyfélprojekt
Miután konfigurálta a Mobile Apps-háttéralkalmazás, hozzon létre egy új ügyfél-alkalmazást, vagy módosíthatja egy meglévő app tooconnect tooAzure. Ebben a szakaszban egy UWP alkalmazás sablonprojektjét, amely testre szabott tooconnect tooyour Mobile Apps-háttéralkalmazás letöltése.

1. Vissza a hello **gyors üzembe helyezési** a Mobile Apps-háttéralkalmazás paneljén kattintson **hozzon létre egy új alkalmazást** > **letöltése**, bontsa ki a tömörített hello project fájlok tooyour helyi számítógépen.

    ![Gyorssablonra épülő Windows-projekt letöltése](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. (Választható) Adja hozzá a hello UWP-alkalmazás projekt toohello hello projektként ugyanahhoz a megoldáshoz. Ez megkönnyíti toodebug és az alkalmazás és hello háttér hello is vizsgálati hello ugyanabban a Visual Studio megoldás, ha úgy dönt, toodo tette. tooadd UWP app projektet toohello megoldást, kell használnia a Visual Studio 2015-öt vagy újabb verziója.
3. Hello UWP-alkalmazást hello indítási projektként nyomja le az F5 kulcs toodeploy hello és futtatási hello alkalmazást.
4. Hello alkalmazásban írjon be egy értelmes szöveget, például *teljes hello oktatóanyag*, a hello **Insert a TodoItem** szövegmezőbe, és kattintson **mentése**.

    ![Az elkészült, gyorssablonra épülő Windows-projekt asztali számítógépen](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Ez küld egy POST kérést toohello új mobil-háttéralkalmazás, amely az Azure-ban.
5. (Választható) Állítsa le hello alkalmazást, és indítsa újra egy másik eszközön vagy mobilemulátoron.

    ![Az elkészült, gyorssablonra épülő Windows-projekt telefonon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Figyelje meg, hogy hello előző lépésben mentett adatok betöltődnek Azure hello UWP-alkalmazás indítása után.

## <a name="next-steps"></a>Következő lépések
* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.
* [Leküldéses értesítések tooyour alkalmazás hozzáadása](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.
* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
