---
title: "Univerzális alkalmazások SDK frissítési eljárások aaaWindows"
description: "Univerzális Windows-alkalmazások SDK és Azure Mobile Engagement frissítése a eljárások"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 95aba5d55cd65d4190aad35737f872414b5ed443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a>Univerzális Windows-alkalmazások SDK eljárások frissítése
Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.

Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban. Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.

## <a name="from-330-too340"></a>A 3.3.0 too3.4.0
### <a name="test-logs"></a>Vizsgálati naplók
Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve. toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Erőforrások
hello Reach átfedő javult. Hello SDK NuGet csomag erőforrások része.

Hello SDK toohello új verziójának frissítése közben választhat tookeep kíván-e a hello a meglévő fájlok vagy nem átfedő mappa az erőforrások:

* Ha hello előző átfedő működik-e meg, vagy hello integrációhoz `WebView` elemek manuálisan dönthet úgy is tookeep a meglévő fájlokat, majd azt továbbra is működni fognak. 
* Ha azt szeretné, hogy csak a név felülírandó hello teljes tooupdate toohello új átfedő `overlay` a hello újat hello SDK csomagban található erőforrások mappát (UWP-alkalmazások: hello frissítés után letölthető hello új átfedő mappát a(z) % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Új átfedő hello segítségével felülírja bármely testreszabott hello korábbi verziójához.
> 
> 

## <a name="from-320-too330"></a>A 3.2.0 too3.3.0
### <a name="resources"></a>Erőforrások
Ez a lépés csak egyéni erőforrások vonatkozik. Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.

## <a name="from-310-too320"></a>A 3.1.0 too3.2.0
### <a name="resources"></a>Erőforrások
Ez a lépés csak egyéni erőforrások vonatkozik. Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.

### <a name="webview-integration"></a>Webes nézet integráció
Néhány fejlesztései toomatch különböző helyigényekhez ebben a verzióban lett bevezetve. Győződjön meg arról, hogy megfelel-e a hello webes nézet integrációja hello következő:

Az az XAML lap ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

És a kapcsolódó .cs fájlban:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated toowithin a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

              #region tooimplement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update hello webview when hello app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update hello webview when hello app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure toodetach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are hello current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements toohello correct size.
              /// </summary>
              /// <param name="width">hello width of your current display.</param>
              /// <param name="height">hello height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes hello Windows.Current.SizeChanged and indicates that webviews have toobe resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes hello ApplicationView.VisibleBoundsChanged and indicates that webviews have toobe resized
              /// </summary>
              /// <param name="sender">hello related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-too300"></a>A 2.0.0 too3.0.0
### <a name="resources"></a>Erőforrások
Ez a lépés csak egyéni erőforrások vonatkozik. Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.

## <a name="from-111-too200"></a>1.1.1 a too2.0.0
hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba. 

> [!IMPORTANT]
> Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást. Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról
> 
> 

Ha egy korábbi verziójáról telepít, adjon hello Capptain webhely toomigrate too1.1.1 először tekintse át, akkor alkalmazza a következő eljárás hello

### <a name="nuget-package"></a>Nuget-csomag
Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.

### <a name="applying-mobile-engagement"></a>Mobilmarketing alkalmazása
hello SDK hello kifejezést használja `Engagement`. A projekt toomatch kell tooupdate ezt a módosítást.

Az aktuális Capptain nuget-csomagot kell toouninstall. Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást. Ha azt szeretné, hogy tookeep azokat a fájlokat készítsen egy másolatot kapnak.

Ezt követően telepítse a projekt hello új Microsoft Azure Engagement nuget-csomagot. Közvetlenül a [nuget webhelyén] találja. vagy az ide-index. Ez a művelet cserél összes erőforrás fájlok Engagement használják, és hozzáadja a hello új Engagement DLL tooyour projektre hivatkozik.

Hogy tooclean a projekt hivatkozásait Capptain DLL hivatkozások törlésével. Ha ez nem teszi meg, Capptain hello verziója ütközésbe fognak kerülni, és hiba történik.

Ha testreszabott Capptain erőforrások, a régi fájlok tartalmát, és illessze be őket hello új Engagement fájlokat. Vegye figyelembe, hogy xaml-és cs is rendelkezik-e a frissített toobe.

Ha ezek lépéseket csak akkor kell Capptain hivatkozásai tooreplace hello új Engagement hivatkozást.

1. Összes Capptain névtér toobe frissíteni kell.
   
    Mielőtt áttelepítése:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    : Az áttelepítés után
   
        using Microsoft.Azure.Engagement;
2. Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".
   
    Mielőtt áttelepítése:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    : Az áttelepítés után
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.
   
    Mielőtt áttelepítése:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    : Az áttelepítés után
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Átfedő módosításai
   
   > [!IMPORTANT]
   > Átfedő is megváltozik. Az új névtér `Microsoft.Azure.Engagement.Overlay`. Toobe xaml-és cs is használatban van. Továbbá `CapptainGrid` toobe nevű `EngagementGrid`, `capptain_notification_content` és `capptain_announcement_content` megnevezett `engagement_notification_content` és `engagement_announcement_content`.
   > 
   > 
   
    Az átmeneti területre:
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    Válik:
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. Hello a további erőforrások, például Capptain képek és HTML-fájlok, ne feledje, hogy azok is történt átnevezett toouse "Engagement".

### <a name="project-declaration"></a>Projekt nyilatkozat
A Package.appxmanifest `File Type Associations` a frissült:

* capptain\_elérni\_tartalom tooengagement\_elérni\_tartalom
* capptain\_napló\_tooengagement fájl\_napló\_fájl

### <a name="application-id--sdk-key"></a>Alkalmazásazonosító / SDK kulcs
Bevonási egy kapcsolati karakterláncot használ. Egy alkalmazás Azonosítót és egy Mobile Engagement SDK kulcs toospecify nincs, így nem toospecify egy kapcsolati karakterláncot. Állíthat be azt a EngagementConfiguration fájlokat.

hello Engagement konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.

A fájl toospecify szerkesztése:

* Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.

Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.

### <a name="items-name-change"></a>Elemek kiszolgálónév-változás
Minden elem nevű *capptain* megnevezett *engagement*. Ehhez hasonlóan az *Capptain* túl*Engagement*.

Példák a gyakran használt Capptain elemeket:

* Most nevű EngagementConfiguration CapptainConfiguration
* Most nevű EngagementAgent CapptainAgent
* Most nevű EngagementReach CapptainReach
* Most nevű EngagementHttpConfig CapptainHttpConfig
* Most nevű GetEngagementPageName GetCapptainPageName

Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.

