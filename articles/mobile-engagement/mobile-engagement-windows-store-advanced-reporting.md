---
title: "Univerzális speciális jelentéskészítés a MobileApps Engagement aaaWindows"
description: "Hogyan tooIntegrate Azure Mobile Engagement az univerzális Windows-alkalmazások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a>Az univerzális alkalmazások Engagement SDK a Windows hello speciális jelentéskészítés
> [!div class="op_single_selector"]
> * [Univerzális Windows](mobile-engagement-windows-store-advanced-reporting.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Ez a témakör ismerteti az univerzális Windows-alkalmazás további jelentéskészítési forgatókönyvekhez. Ilyen például, beállítások, amelyek segítségével létrehozott hello tooapply toohello app [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Az oktatóanyag elindítása előtt először végezze hello [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) oktatóanyag, amely szándékosan közvetlen és egyszerű. Ez az oktatóanyag ismerteti a további beállítások közül választhat.

## <a name="specifying-engagement-configuration-at-runtime"></a>Futásidőben engagement konfiguráció megadása
hello Engagement konfigurációs rendszer központosított hello `Resources\EngagementConfiguration.xml` fájlt a projekt, amely ahol hello megadott [bevezetés](mobile-engagement-windows-store-dotnet-get-started.md) témakör.

De azt is megadhatja, hogy futásidőben: a következő metódus hello Engagement ügynök inicializálása előtt hello hívása:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Ajánlott módszer: túlterhelés a `Page` osztályok
minden tooactivate hello reporting összes hello naplók szükséges Engagement toocompute felhasználók, munkamenetek, tevékenységeket, összeomlik és műszaki statisztika, ellenőrizze a `Page` alosztályokat örökölhet hello `EngagementPage` osztályok.

Itt látható egy példa egy lap az alkalmazás. Mindent hello ugyanaz az alkalmazás összes lapja esetében.

### <a name="c-source-file"></a>C# forrásfájl
A lap módosítása `.xaml.cs` fájlt:

* Adja hozzá a tooyour `using` utasításokat:
  
      using Microsoft.Azure.Engagement;
* Cserélje le `Page` rendelkező `EngagementPage`:

**Nélkül Engagement:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Az Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`. Ellenkező esetben hello tevékenység nem kell jelenteni (hello `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus).
> 
> 

### <a name="xaml-file"></a>XAML-fájl
A lap módosítása `.xaml` fájlt:

* Tooyour névtér-deklarációk hozzáadása:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Cserélje le `Page` rendelkező `engagement:EngagementPage`:

**Nélkül Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Az Engagement:**

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-hello-default-behaviour"></a>Bírálja felül a hello alapértelmezett viselkedése
Alapértelmezés szerint hello osztálynév hello lap hello tevékenység nevét, nem extra akkor számít. Ha hello osztály hello "Lap" utótagot használ, az Engagement eltávolítja azt.

toooverride hello alapértelmezett viselkedése hello nevét, adja hozzá ezt a kódot:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

tooreport további információt a tevékenységet, adja hozzá ezt a kódot:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Ezek a módszerek is meghívhatók hello `OnNavigatedTo` módszer a lap.

### <a name="alternate-method-call-startactivity-manually"></a>Másodlagos módszer: hívja `StartActivity()` manuálisan
Ha nem, vagy nem toooverload a `Page` osztályok, ehelyett megkezdheti a tevékenységek meghívásával `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, hogy hívása `StartActivity` belül a `OnNavigatedTo` metódus a lap.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Gondoskodjon arról, hogy megfelelően a munkamenet befejezéséhez.
> 
> Univerzális Windows SDK hello automatikusan meghívja a hello `EndActivity` metódus hello alkalmazás bezárásakor. Így **magas** toocall hello ajánlott `StartActivity` metódus hello tevékenység hello felhasználó módosítja, amikor és túl**soha** hívás hello `EndActivity` metódust. Ez a módszer értesíti hello Engagement server hello jelenlegi felhasználó elhagyta hello alkalmazás, amely hatással van-e az összes alkalmazás naplóiban.
> 
> 

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Szükség esetén érdemes lehet tooreport alkalmazásspecifikus eseményeket, hibákat és feladatokat, toodo így, használjon más módszereket található hello hello `EngagementAgent` osztály. hello Engagement API lehetővé teszi, hogy minden bevonási speciális képességek használatát.

További információkért lásd: [hogyan toouse hello speciális a Mobile Engagement az univerzális Windows-alkalmazás API címkézés](mobile-engagement-windows-store-use-engagement-api.md).

