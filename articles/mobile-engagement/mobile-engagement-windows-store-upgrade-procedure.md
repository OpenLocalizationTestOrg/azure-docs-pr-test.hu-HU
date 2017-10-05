---
title: "Univerzális Windows-alkalmazások SDK eljárások frissítése"
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
ms.openlocfilehash: fe85a99a92fb39082cafe7422b356de1f20f14bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="4caf0-103">Univerzális Windows-alkalmazások SDK eljárások frissítése</span><span class="sxs-lookup"><span data-stu-id="4caf0-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="4caf0-104">Ha Ön már rendelkezik integrált Engagement régebbi verzióját az alkalmazásba, hogy az SDK-val történő frissítése során, vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="4caf0-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="4caf0-105">Előfordulhat, hogy kell eljárnia számos eljárást, ha nem fogadta az SDK a különböző verzióiban.</span><span class="sxs-lookup"><span data-stu-id="4caf0-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="4caf0-106">Például ha áttelepít 0.10.1 0.11.0 először hajtsa végre a "from a 0.10.1 0.9.0-s" eljárást kell majd a "from a 0.11.0 0.10.1" eljárást.</span><span class="sxs-lookup"><span data-stu-id="4caf0-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-330-to-340"></a><span data-ttu-id="4caf0-107">A 3.3.0 3.4.0 számára</span><span class="sxs-lookup"><span data-stu-id="4caf0-107">From 3.3.0 to 3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="4caf0-108">Vizsgálati naplók</span><span class="sxs-lookup"><span data-stu-id="4caf0-108">Test logs</span></span>
<span data-ttu-id="4caf0-109">Az SDK által előállított konzolnaplófájlokban mostantól lehet engedélyezve vagy letiltva/szűrve.</span><span class="sxs-lookup"><span data-stu-id="4caf0-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="4caf0-110">Ez testreszabásához frissítse a tulajdonságát `EngagementAgent.Instance.TestLogEnabled` egy érhetők el az érték a `EngagementTestLogLevel` számbavételi, például:</span><span class="sxs-lookup"><span data-stu-id="4caf0-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="4caf0-111">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="4caf0-111">Resources</span></span>
<span data-ttu-id="4caf0-112">A Reach átfedő javult.</span><span class="sxs-lookup"><span data-stu-id="4caf0-112">The Reach overlay has been improved.</span></span> <span data-ttu-id="4caf0-113">Az SDK NuGet-csomag erőforrások része.</span><span class="sxs-lookup"><span data-stu-id="4caf0-113">It is part of the SDK NuGet package resources.</span></span>

<span data-ttu-id="4caf0-114">Amíg az SDK újabb verziójára frissít, hogy szeretné-e az átmeneti területre mappából az erőforrások mindig a meglévő fájlokat, vagy nem választhat:</span><span class="sxs-lookup"><span data-stu-id="4caf0-114">While upgrading to the new version of the SDK you can choose whether you want to keep your existing files from the overlay folder of your resources or not:</span></span>

* <span data-ttu-id="4caf0-115">Ha az előző átfedés működik-e meg, vagy az integrációhoz a `WebView` manuálisan majd beállíthatja úgy, hogy a meglévő fájlok elemeket, hogy továbbra is működni fognak.</span><span class="sxs-lookup"><span data-stu-id="4caf0-115">If the previous overlay is working for you or you are integrating the `WebView` elements manually then you can decide to keep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="4caf0-116">Ha szeretné frissíteni az új átmeneti területre, helyettesítse a teljes `overlay` mappa az erőforrások közül az újjal az SDK-csomagból (UWP-alkalmazások: a frissítés után az % USERPROFILE % kérheti le az új átfedő mappa\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="4caf0-116">If you want to update to the new overlay, just replace the whole `overlay` folder from your resources with the new one from the SDK package (UWP apps: after the upgrade, you can get the new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="4caf0-117">Az új átfedés használatával felülírja az előző verzió végzett testreszabások.</span><span class="sxs-lookup"><span data-stu-id="4caf0-117">Using the new overlay will overwrite any customizations made on the previous version.</span></span>
> 
> 

## <a name="from-320-to-330"></a><span data-ttu-id="4caf0-118">A 3.2.0 3.3.0 számára</span><span class="sxs-lookup"><span data-stu-id="4caf0-118">From 3.2.0 to 3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="4caf0-119">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="4caf0-119">Resources</span></span>
<span data-ttu-id="4caf0-120">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-120">This step concerns customized resources only.</span></span> <span data-ttu-id="4caf0-121">Ha az SDK-val (html, képek, átfedő) által biztosított erőforrások testreszabott akkor rendelkezik biztonsági mentése azokat a frissítés előtt, és alkalmazza újra a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4caf0-121">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-to-320"></a><span data-ttu-id="4caf0-122">A 3.1.0 3.2.0 számára</span><span class="sxs-lookup"><span data-stu-id="4caf0-122">From 3.1.0 to 3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="4caf0-123">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="4caf0-123">Resources</span></span>
<span data-ttu-id="4caf0-124">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-124">This step concerns customized resources only.</span></span> <span data-ttu-id="4caf0-125">Ha az SDK-val (html, képek, átfedő) által biztosított erőforrások testreszabott akkor rendelkezik biztonsági mentése azokat a frissítés előtt, és alkalmazza újra a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4caf0-125">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="4caf0-126">Webes nézet integráció</span><span class="sxs-lookup"><span data-stu-id="4caf0-126">Webview integration</span></span>
<span data-ttu-id="4caf0-127">Néhány fejlesztései felel meg a különböző helyigényekhez ebben a verzióban lett bevezetve.</span><span class="sxs-lookup"><span data-stu-id="4caf0-127">Some improvements to match different device form factors were introduced in this version.</span></span> <span data-ttu-id="4caf0-128">Győződjön meg arról, hogy a webes nézet az integráció felel meg a következő:</span><span class="sxs-lookup"><span data-stu-id="4caf0-128">Make sure that your integration of the webview match the following:</span></span>

<span data-ttu-id="4caf0-129">Az az XAML lap ():</span><span class="sxs-lookup"><span data-stu-id="4caf0-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="4caf0-130">És a kapcsolódó .cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="4caf0-130">And in your associated .cs file:</span></span>

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
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
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-to-300"></a><span data-ttu-id="4caf0-131">A 2.0.0 3.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="4caf0-131">From 2.0.0 to 3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="4caf0-132">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="4caf0-132">Resources</span></span>
<span data-ttu-id="4caf0-133">Ez a lépés csak egyéni erőforrások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-133">This step concerns customized resources only.</span></span> <span data-ttu-id="4caf0-134">Ha az SDK-val (html, képek, átfedő) által biztosított erőforrások testreszabott akkor rendelkezik biztonsági mentése azokat a frissítés előtt, és alkalmazza újra a testreszabás, a frissített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4caf0-134">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-to-200"></a><span data-ttu-id="4caf0-135">A 1.1.1 2.0.0 számára</span><span class="sxs-lookup"><span data-stu-id="4caf0-135">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="4caf0-136">A következő ismerteti, hogyan telepíthetők át az SDK-integráció az alkalmazásba az Azure Mobile Engagement technológiával Capptain SAS által kínált Capptain szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4caf0-136">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="4caf0-137">Capptain és a Mobile Engagement nem ugyanazok a szolgáltatások és az alábbi eljárás csak emel ki, hogyan telepítheti át az ügyfélalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4caf0-137">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="4caf0-138">Az SDK-t az alkalmazás áttelepítése rendszer nem telepíti át az adatok a Capptain kiszolgálókról a Mobile Engagement-kiszolgálókon</span><span class="sxs-lookup"><span data-stu-id="4caf0-138">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="4caf0-139">Ha egy korábbi verziójáról telepít, tekintse meg a Capptain webhely 1.1.1 először át, akkor alkalmazza az alábbi eljárás</span><span class="sxs-lookup"><span data-stu-id="4caf0-139">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="4caf0-140">Nuget-csomag</span><span class="sxs-lookup"><span data-stu-id="4caf0-140">Nuget package</span></span>
<span data-ttu-id="4caf0-141">Cserélje le **Capptain.WindowsPhone** által **MicrosoftAzure.MobileEngagement** Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="4caf0-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="4caf0-142">Mobilmarketing alkalmazása</span><span class="sxs-lookup"><span data-stu-id="4caf0-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="4caf0-143">Az SDK-t használ a kifejezés `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="4caf0-143">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="4caf0-144">Frissítenie kell a módosítás megfelelő a projekthez.</span><span class="sxs-lookup"><span data-stu-id="4caf0-144">You need to update your project to match this change.</span></span>

<span data-ttu-id="4caf0-145">El kell távolítania a jelenlegi Capptain nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="4caf0-145">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="4caf0-146">Vegye figyelembe, hogy eltávolítja-e Capptain erőforrások mappában összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="4caf0-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="4caf0-147">Ha meg szeretné tartani ezeket a fájlokat készítsen egy másolatot kapnak.</span><span class="sxs-lookup"><span data-stu-id="4caf0-147">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="4caf0-148">Ezt követően a Microsoft Azure Engagement új nuget-csomag telepítése a projekten.</span><span class="sxs-lookup"><span data-stu-id="4caf0-148">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="4caf0-149">Közvetlenül a [nuget webhelyén] találja.</span><span class="sxs-lookup"><span data-stu-id="4caf0-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="4caf0-150">vagy az ide-index.</span><span class="sxs-lookup"><span data-stu-id="4caf0-150">or here index.</span></span> <span data-ttu-id="4caf0-151">Ez a művelet lecseréli a bevonási által használt összes erőforrások fájlokat, és az új Engagement DLL ad hozzá a projekt hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="4caf0-151">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="4caf0-152">A projekt hivatkozásait tisztítása Capptain DLL hivatkozások törlésével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-152">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="4caf0-153">Ha ez nem teszi meg, Capptain verziója ütközésbe fognak kerülni, és hiba történik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-153">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="4caf0-154">Ha testre szabott Capptain erőforrásokat, a régi fájlok tartalmát, és illessze be őket az új Engagement fájlok.</span><span class="sxs-lookup"><span data-stu-id="4caf0-154">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="4caf0-155">Vegye figyelembe, hogy az xaml-és cs is frissítenie kell őket.</span><span class="sxs-lookup"><span data-stu-id="4caf0-155">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="4caf0-156">Ha ezek lépéseket csak akkor kell cserélje le az új Engagement hivatkozást Capptain hivatkozásai.</span><span class="sxs-lookup"><span data-stu-id="4caf0-156">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="4caf0-157">Összes Capptain névteret kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="4caf0-157">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="4caf0-158">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="4caf0-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="4caf0-159">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="4caf0-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="4caf0-160">Minden Capptain osztály, amely tartalmazza a "Capptain" tartalmaznia kell "Engagement".</span><span class="sxs-lookup"><span data-stu-id="4caf0-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="4caf0-161">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="4caf0-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="4caf0-162">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="4caf0-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="4caf0-163">Az xaml-fájlokkal Capptain névtér és attribútumok is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="4caf0-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="4caf0-164">Mielőtt áttelepítése:</span><span class="sxs-lookup"><span data-stu-id="4caf0-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="4caf0-165">: Az áttelepítés után</span><span class="sxs-lookup"><span data-stu-id="4caf0-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="4caf0-166">Átfedő módosításai</span><span class="sxs-lookup"><span data-stu-id="4caf0-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="4caf0-167">Átfedő is megváltozik.</span><span class="sxs-lookup"><span data-stu-id="4caf0-167">Overlay also changes.</span></span> <span data-ttu-id="4caf0-168">Az új névtér `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="4caf0-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="4caf0-169">Xaml-és cs is használatban van.</span><span class="sxs-lookup"><span data-stu-id="4caf0-169">It has to be used in both xaml and cs files.</span></span> <span data-ttu-id="4caf0-170">Továbbá `CapptainGrid` annak neve lehet `EngagementGrid`, `capptain_notification_content` és `capptain_announcement_content` megnevezett `engagement_notification_content` és `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="4caf0-170">Moreover `CapptainGrid` is to be named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="4caf0-171">Az átmeneti területre:</span><span class="sxs-lookup"><span data-stu-id="4caf0-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="4caf0-172">Válik:</span><span class="sxs-lookup"><span data-stu-id="4caf0-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="4caf0-173">Az egyéb erőforrások például Capptain képek és HTML-fájlok vegye figyelembe, hogy azok is átnevezték használható az "Engagement".</span><span class="sxs-lookup"><span data-stu-id="4caf0-173">For the other resources like Capptain pictures and HTML files, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="4caf0-174">Projekt nyilatkozat</span><span class="sxs-lookup"><span data-stu-id="4caf0-174">Project declaration</span></span>
<span data-ttu-id="4caf0-175">A Package.appxmanifest `File Type Associations` a frissült:</span><span class="sxs-lookup"><span data-stu-id="4caf0-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="4caf0-176">capptain\_elérni\_tartalom engagement\_elérni\_tartalom</span><span class="sxs-lookup"><span data-stu-id="4caf0-176">capptain\_reach\_content to engagement\_reach\_content</span></span>
* <span data-ttu-id="4caf0-177">capptain\_napló\_engagement fájl\_napló\_fájl</span><span class="sxs-lookup"><span data-stu-id="4caf0-177">capptain\_log\_file to engagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="4caf0-178">Alkalmazásazonosító / SDK kulcs</span><span class="sxs-lookup"><span data-stu-id="4caf0-178">Application ID / SDK Key</span></span>
<span data-ttu-id="4caf0-179">Bevonási egy kapcsolati karakterláncot használ.</span><span class="sxs-lookup"><span data-stu-id="4caf0-179">Engagement uses a connection string.</span></span> <span data-ttu-id="4caf0-180">Nincs a Mobile Engagement egy alkalmazás és az SDK kulcs adható meg, csak akkor adjon meg egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4caf0-180">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="4caf0-181">Állíthat be azt a EngagementConfiguration fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4caf0-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="4caf0-182">A bevonási konfigurációs állítható be a `Resources\EngagementConfiguration.xml` fájlt a projekt.</span><span class="sxs-lookup"><span data-stu-id="4caf0-182">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="4caf0-183">Adja meg, hogy a fájl szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="4caf0-183">Edit this file to specify:</span></span>

* <span data-ttu-id="4caf0-184">Az alkalmazás kapcsolati karakterlánc címkék között `<connectionString>` és `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="4caf0-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="4caf0-185">Ha azt szeretné, ehelyett meg futásidőben, hívása előtt az Engagement ügynök inicializálása a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="4caf0-185">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="4caf0-186">A kapcsolati karakterlánc az alkalmazás a klasszikus Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4caf0-186">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="4caf0-187">Elemek kiszolgálónév-változás</span><span class="sxs-lookup"><span data-stu-id="4caf0-187">Items name change</span></span>
<span data-ttu-id="4caf0-188">Minden elem nevű *capptain* megnevezett *engagement*.</span><span class="sxs-lookup"><span data-stu-id="4caf0-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="4caf0-189">Ehhez hasonlóan az *Capptain* való *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="4caf0-189">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="4caf0-190">Példák a gyakran használt Capptain elemeket:</span><span class="sxs-lookup"><span data-stu-id="4caf0-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="4caf0-191">Most nevű EngagementConfiguration CapptainConfiguration</span><span class="sxs-lookup"><span data-stu-id="4caf0-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="4caf0-192">Most nevű EngagementAgent CapptainAgent</span><span class="sxs-lookup"><span data-stu-id="4caf0-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="4caf0-193">Most nevű EngagementReach CapptainReach</span><span class="sxs-lookup"><span data-stu-id="4caf0-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="4caf0-194">Most nevű EngagementHttpConfig CapptainHttpConfig</span><span class="sxs-lookup"><span data-stu-id="4caf0-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="4caf0-195">Most nevű GetEngagementPageName GetCapptainPageName</span><span class="sxs-lookup"><span data-stu-id="4caf0-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="4caf0-196">Vegye figyelembe, hogy az Átnevezés is hatással van a többszörösen definiált metódusok.</span><span class="sxs-lookup"><span data-stu-id="4caf0-196">Note that rename also affects overridden methods.</span></span>

