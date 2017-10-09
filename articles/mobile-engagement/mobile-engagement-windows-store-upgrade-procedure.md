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
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="435d5-103">Univerzális Windows-alkalmazások SDK eljárások frissítése</span><span class="sxs-lookup"><span data-stu-id="435d5-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="435d5-104">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy tooconsider hello hello SDK frissítéskor a következő pontok.</span><span class="sxs-lookup"><span data-stu-id="435d5-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="435d5-105">Előfordulhat, hogy toofollow számos eljárást, ha nem fogadta hello SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="435d5-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="435d5-106">Például ha telepít át 0.10.1 hello kövesse toofirst rendelkezik too0.11.0 "0.9.0-s a too0.10.1" eljárást akkor hello "0.10.1 a too0.11.0" eljárás.</span><span class="sxs-lookup"><span data-stu-id="435d5-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-330-too340"></a><span data-ttu-id="435d5-107">A 3.3.0 too3.4.0</span><span class="sxs-lookup"><span data-stu-id="435d5-107">From 3.3.0 too3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="435d5-108">Vizsgálati naplók</span><span class="sxs-lookup"><span data-stu-id="435d5-108">Test logs</span></span>
<span data-ttu-id="435d5-109">Hello SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve.</span><span class="sxs-lookup"><span data-stu-id="435d5-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="435d5-110">toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="435d5-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="435d5-111">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="435d5-111">Resources</span></span>
<span data-ttu-id="435d5-112">hello Reach átfedő javult.</span><span class="sxs-lookup"><span data-stu-id="435d5-112">hello Reach overlay has been improved.</span></span> <span data-ttu-id="435d5-113">Hello SDK NuGet csomag erőforrások része.</span><span class="sxs-lookup"><span data-stu-id="435d5-113">It is part of hello SDK NuGet package resources.</span></span>

<span data-ttu-id="435d5-114">Hello SDK toohello új verziójának frissítése közben választhat tookeep kíván-e a hello a meglévő fájlok vagy nem átfedő mappa az erőforrások:</span><span class="sxs-lookup"><span data-stu-id="435d5-114">While upgrading toohello new version of hello SDK you can choose whether you want tookeep your existing files from hello overlay folder of your resources or not:</span></span>

* <span data-ttu-id="435d5-115">Ha hello előző átfedő működik-e meg, vagy hello integrációhoz `WebView` elemek manuálisan dönthet úgy is tookeep a meglévő fájlokat, majd azt továbbra is működni fognak.</span><span class="sxs-lookup"><span data-stu-id="435d5-115">If hello previous overlay is working for you or you are integrating hello `WebView` elements manually then you can decide tookeep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="435d5-116">Ha azt szeretné, hogy csak a név felülírandó hello teljes tooupdate toohello új átfedő `overlay` a hello újat hello SDK csomagban található erőforrások mappát (UWP-alkalmazások: hello frissítés után letölthető hello új átfedő mappát a(z) % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="435d5-116">If you want tooupdate toohello new overlay, just replace hello whole `overlay` folder from your resources with hello new one from hello SDK package (UWP apps: after hello upgrade, you can get hello new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="435d5-117">Új átfedő hello segítségével felülírja bármely testreszabott hello korábbi verziójához.</span><span class="sxs-lookup"><span data-stu-id="435d5-117">Using hello new overlay will overwrite any customizations made on hello previous version.</span></span>
> 
> 

## <a name="from-320-too330"></a><span data-ttu-id="435d5-118">A 3.2.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="435d5-118">From 3.2.0 too3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="435d5-119">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="435d5-119">Resources</span></span>
<span data-ttu-id="435d5-120">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="435d5-120">This step concerns customized resources only.</span></span> <span data-ttu-id="435d5-121">Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="435d5-121">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-too320"></a><span data-ttu-id="435d5-122">A 3.1.0 too3.2.0</span><span class="sxs-lookup"><span data-stu-id="435d5-122">From 3.1.0 too3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="435d5-123">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="435d5-123">Resources</span></span>
<span data-ttu-id="435d5-124">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="435d5-124">This step concerns customized resources only.</span></span> <span data-ttu-id="435d5-125">Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="435d5-125">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="435d5-126">Webes nézet integráció</span><span class="sxs-lookup"><span data-stu-id="435d5-126">Webview integration</span></span>
<span data-ttu-id="435d5-127">Néhány fejlesztései toomatch különböző helyigényekhez ebben a verzióban lett bevezetve.</span><span class="sxs-lookup"><span data-stu-id="435d5-127">Some improvements toomatch different device form factors were introduced in this version.</span></span> <span data-ttu-id="435d5-128">Győződjön meg arról, hogy megfelel-e a hello webes nézet integrációja hello következő:</span><span class="sxs-lookup"><span data-stu-id="435d5-128">Make sure that your integration of hello webview match hello following:</span></span>

<span data-ttu-id="435d5-129">Az az XAML lap ():</span><span class="sxs-lookup"><span data-stu-id="435d5-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="435d5-130">És a kapcsolódó .cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="435d5-130">And in your associated .cs file:</span></span>

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

## <a name="from-200-too300"></a><span data-ttu-id="435d5-131">A 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="435d5-131">From 2.0.0 too3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="435d5-132">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="435d5-132">Resources</span></span>
<span data-ttu-id="435d5-133">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="435d5-133">This step concerns customized resources only.</span></span> <span data-ttu-id="435d5-134">Ha testre szabott hello (html, képek, átfedő) SDK által biztosított, akkor el kell toobackup hello erőforrások őket előtt frissíti, és legyen a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="435d5-134">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-too200"></a><span data-ttu-id="435d5-135">1.1.1 a too2.0.0</span><span class="sxs-lookup"><span data-stu-id="435d5-135">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="435d5-136">hello következő ismerteti, hogyan toomigrate az SDK-integráció a hello Capptain szolgáltatás által kínált Capptain SAS technológiával az Azure Mobile Engagement az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="435d5-136">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="435d5-137">Capptain és a Mobile Engagement nem hello azonos szolgáltatások és hello alábbi eljárás csak emel ki, hogyan toomigrate hello ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="435d5-137">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="435d5-138">Áttelepítése hello SDK hello alkalmazásban rendszer nem telepíti át az adatokat a hello Capptain kiszolgálók toohello a Mobile Engagement kiszolgálókról</span><span class="sxs-lookup"><span data-stu-id="435d5-138">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="435d5-139">Ha egy korábbi verziójáról telepít, adjon hello Capptain webhely toomigrate too1.1.1 először tekintse át, akkor alkalmazza a következő eljárás hello</span><span class="sxs-lookup"><span data-stu-id="435d5-139">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="435d5-140">Nuget-csomag</span><span class="sxs-lookup"><span data-stu-id="435d5-140">Nuget package</span></span>
<span data-ttu-id="435d5-141">Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="435d5-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="435d5-142">Mobilmarketing alkalmazása</span><span class="sxs-lookup"><span data-stu-id="435d5-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="435d5-143">hello SDK hello kifejezést használja `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="435d5-143">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="435d5-144">A projekt toomatch kell tooupdate ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="435d5-144">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="435d5-145">Az aktuális Capptain nuget-csomagot kell toouninstall.</span><span class="sxs-lookup"><span data-stu-id="435d5-145">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="435d5-146">Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="435d5-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="435d5-147">Ha azt szeretné, hogy tookeep azokat a fájlokat készítsen egy másolatot kapnak.</span><span class="sxs-lookup"><span data-stu-id="435d5-147">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="435d5-148">Ezt követően telepítse a projekt hello új Microsoft Azure Engagement nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="435d5-148">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="435d5-149">Közvetlenül a [nuget webhelyén] találja.</span><span class="sxs-lookup"><span data-stu-id="435d5-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="435d5-150">vagy az ide-index.</span><span class="sxs-lookup"><span data-stu-id="435d5-150">or here index.</span></span> <span data-ttu-id="435d5-151">Ez a művelet cserél összes erőforrás fájlok Engagement használják, és hozzáadja a hello új Engagement DLL tooyour projektre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="435d5-151">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="435d5-152">Hogy tooclean a projekt hivatkozásait Capptain DLL hivatkozások törlésével.</span><span class="sxs-lookup"><span data-stu-id="435d5-152">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="435d5-153">Ha ez nem teszi meg, Capptain hello verziója ütközésbe fognak kerülni, és hiba történik.</span><span class="sxs-lookup"><span data-stu-id="435d5-153">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="435d5-154">Ha testreszabott Capptain erőforrások, a régi fájlok tartalmát, és illessze be őket hello új Engagement fájlokat.</span><span class="sxs-lookup"><span data-stu-id="435d5-154">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="435d5-155">Vegye figyelembe, hogy xaml-és cs is rendelkezik-e a frissített toobe.</span><span class="sxs-lookup"><span data-stu-id="435d5-155">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="435d5-156">Ha ezek lépéseket csak akkor kell Capptain hivatkozásai tooreplace hello új Engagement hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="435d5-156">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="435d5-157">Összes Capptain névtér toobe frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="435d5-157">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="435d5-158">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="435d5-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="435d5-159">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="435d5-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="435d5-160">Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".</span><span class="sxs-lookup"><span data-stu-id="435d5-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="435d5-161">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="435d5-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="435d5-162">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="435d5-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="435d5-163">Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="435d5-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="435d5-164">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="435d5-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="435d5-165">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="435d5-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="435d5-166">Átfedő módosításai</span><span class="sxs-lookup"><span data-stu-id="435d5-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="435d5-167">Átfedő is megváltozik.</span><span class="sxs-lookup"><span data-stu-id="435d5-167">Overlay also changes.</span></span> <span data-ttu-id="435d5-168">Az új névtér `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="435d5-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="435d5-169">Toobe xaml-és cs is használatban van.</span><span class="sxs-lookup"><span data-stu-id="435d5-169">It has toobe used in both xaml and cs files.</span></span> <span data-ttu-id="435d5-170">Továbbá `CapptainGrid` toobe nevű `EngagementGrid`, `capptain_notification_content` és `capptain_announcement_content` megnevezett `engagement_notification_content` és `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="435d5-170">Moreover `CapptainGrid` is toobe named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="435d5-171">Az átmeneti területre:</span><span class="sxs-lookup"><span data-stu-id="435d5-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="435d5-172">Válik:</span><span class="sxs-lookup"><span data-stu-id="435d5-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="435d5-173">Hello a további erőforrások, például Capptain képek és HTML-fájlok, ne feledje, hogy azok is történt átnevezett toouse "Engagement".</span><span class="sxs-lookup"><span data-stu-id="435d5-173">For hello other resources like Capptain pictures and HTML files, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="435d5-174">Projekt nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="435d5-174">Project declaration</span></span>
<span data-ttu-id="435d5-175">A Package.appxmanifest `File Type Associations` a frissült:</span><span class="sxs-lookup"><span data-stu-id="435d5-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="435d5-176">capptain\_elérni\_tartalom tooengagement\_elérni\_tartalom</span><span class="sxs-lookup"><span data-stu-id="435d5-176">capptain\_reach\_content tooengagement\_reach\_content</span></span>
* <span data-ttu-id="435d5-177">capptain\_napló\_tooengagement fájl\_napló\_fájl</span><span class="sxs-lookup"><span data-stu-id="435d5-177">capptain\_log\_file tooengagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="435d5-178">Alkalmazásazonosító / SDK kulcs</span><span class="sxs-lookup"><span data-stu-id="435d5-178">Application ID / SDK Key</span></span>
<span data-ttu-id="435d5-179">Bevonási egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="435d5-179">Engagement uses a connection string.</span></span> <span data-ttu-id="435d5-180">Egy alkalmazás Azonosítót és egy Mobile Engagement SDK kulcs toospecify nincs, így nem toospecify egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="435d5-180">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="435d5-181">Állíthat be azt a EngagementConfiguration fájlokat.</span><span class="sxs-lookup"><span data-stu-id="435d5-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="435d5-182">hello Engagement konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="435d5-182">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="435d5-183">A fájl toospecify szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="435d5-183">Edit this file toospecify:</span></span>

* <span data-ttu-id="435d5-184">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="435d5-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="435d5-185">Ha azt szeretné, hogy a futtatókörnyezet ehelyett hívása hello következő toospecify metódus hello Engagement ügynök inicializálása előtt:</span><span class="sxs-lookup"><span data-stu-id="435d5-185">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="435d5-186">hello kapcsolati karakterlánc az alkalmazás megjelenik a klasszikus Azure portál hello.</span><span class="sxs-lookup"><span data-stu-id="435d5-186">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="435d5-187">Elemek kiszolgálónév-változás</span><span class="sxs-lookup"><span data-stu-id="435d5-187">Items name change</span></span>
<span data-ttu-id="435d5-188">Minden elem nevű *capptain* megnevezett *engagement*.</span><span class="sxs-lookup"><span data-stu-id="435d5-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="435d5-189">Ehhez hasonlóan az *Capptain* túl*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="435d5-189">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="435d5-190">Példák a gyakran használt Capptain elemeket:</span><span class="sxs-lookup"><span data-stu-id="435d5-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="435d5-191">Most nevű EngagementConfiguration CapptainConfiguration</span><span class="sxs-lookup"><span data-stu-id="435d5-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="435d5-192">Most nevű EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="435d5-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="435d5-193">Most nevű EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="435d5-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="435d5-194">Most nevű EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="435d5-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="435d5-195">Most nevű GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="435d5-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="435d5-196">Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.</span><span class="sxs-lookup"><span data-stu-id="435d5-196">Note that rename also affects overridden methods.</span></span>

