---
title: "aaaTroubleshooting hello Azure hozzáférési Panel bővítményét az Internet Explorer |} Microsoft Docs"
description: "Hogyan toouse csoport házirend toodeploy hello Internet Explorer bővítmény hello saját alkalmazások portálhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23cbb6117f34759330206de3a26f1397486acedb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="e412b-103">Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="e412b-103">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="e412b-104">Ez a cikk hello a következő problémák elhárításához nyújt segítséget:</span><span class="sxs-lookup"><span data-stu-id="e412b-104">This article helps you troubleshoot hello following problems:</span></span>

* <span data-ttu-id="e412b-105">Ön éppen nem tooaccess hello saját alkalmazások portálon keresztül az alkalmazások Internet Explorer használata során.</span><span class="sxs-lookup"><span data-stu-id="e412b-105">You're unable tooaccess your apps through hello My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="e412b-106">Lásd a "Szoftver telepítése" üzenet, annak ellenére, hogy a korábban telepített hello szoftver hello.</span><span class="sxs-lookup"><span data-stu-id="e412b-106">You see hello "Install Software" message even though you've already installed hello software.</span></span>

<span data-ttu-id="e412b-107">Ha Ön rendszergazda, további információ: [hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="e412b-107">If you are an admin, see also: [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-hello-diagnostic-tool"></a><span data-ttu-id="e412b-108">Hello diagnosztikai eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="e412b-108">Run hello Diagnostic Tool</span></span>
<span data-ttu-id="e412b-109">Ha letölti és hello hozzáférési Panel diagnosztikai eszközt futtató felderítheti hello hozzáférési Panel bővítmény telepítési problémákat:</span><span class="sxs-lookup"><span data-stu-id="e412b-109">You can diagnose installation problems with hello Access Panel Extension by downloading and running hello Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="e412b-110">Kattintson ide a toodownload hello diagnosztikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="e412b-110">Click here toodownload hello diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="e412b-111">Nyitott hello fájlt, és nyomja le az **az összes kibontása** gombra.</span><span class="sxs-lookup"><span data-stu-id="e412b-111">Open hello file, and press **Extract all** button.</span></span>
   
    ![Nyomja meg az összes kibontása](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="e412b-113">Nyomja le az hello **kibontása** gomb toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e412b-113">Then press hello **Extract** button toocontinue.</span></span>
   
    ![Nyomja le az Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="e412b-115">toorun hello eszközt, kattintson a jobb gombbal hello fájlt **AccessPanelExtensionDiagnosticTool**, majd jelölje be **nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató**.</span><span class="sxs-lookup"><span data-stu-id="e412b-115">toorun hello tool, right-click hello file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="e412b-117">Ekkor megjelenik a következő diagnosztikai ablak, amely leírja, milyen lehet még a telepítés hello.</span><span class="sxs-lookup"><span data-stu-id="e412b-117">You will then see hello following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![Egy minta hello diagnosztikai ablak](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="e412b-119">Kattintson a "**Igen**" toolet hello program javítás hello problémákat talált.</span><span class="sxs-lookup"><span data-stu-id="e412b-119">Click "**YES**" toolet hello program fix hello issues that have been found.</span></span>
7. <span data-ttu-id="e412b-120">a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="e412b-120">toosave these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="e412b-121">Ha továbbra sem tud hozzáférni az alkalmazásokat, próbálja hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e412b-121">If you still can't access your apps, try hello steps below.</span></span>

## <a name="check-that-hello-access-panel-extension-is-enabled"></a><span data-ttu-id="e412b-122">Ellenőrizze, hogy engedélyezve van a hozzáférési Panel bővítmény hello</span><span class="sxs-lookup"><span data-stu-id="e412b-122">Check that hello Access Panel Extension is enabled</span></span>
<span data-ttu-id="e412b-123">amely a hozzáférési Panel bővítmény hello tooverify engedélyezve van az Internet Explorerben:</span><span class="sxs-lookup"><span data-stu-id="e412b-123">tooverify that hello Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="e412b-124">Az Internet Explorer, kattintson a hello **fogaskerék ikonra** a hello jobb felső sarkában hello ablak.</span><span class="sxs-lookup"><span data-stu-id="e412b-124">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="e412b-125">Válassza ki **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e412b-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="e412b-126">(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e412b-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Nyissa meg tooTools > Internetbeállítások](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="e412b-128">Kattintson a hello **programok** lapra, majd kattintson az hello **bővítmények kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e412b-128">Click hello **Programs** tab, then click hello **Manage add-ons** button.</span></span>
   
    ![Kattintson a bővítmények kezelése](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="e412b-130">Ezen a párbeszédpanelen válassza ki a **hozzáférési Panel bővítmény** majd hello **engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e412b-130">In this dialog, select **Access Panel Extension** and then click hello **Enable** button.</span></span>
   
    ![Kattintson az Engedélyezés parancsra](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="e412b-132">a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="e412b-132">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="e412b-133">Bővítmények engedélyezése az InPrivate-böngészés</span><span class="sxs-lookup"><span data-stu-id="e412b-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="e412b-134">Ha a hello InPrivate-böngészés módot:</span><span class="sxs-lookup"><span data-stu-id="e412b-134">If you are using hello InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="e412b-135">Az Internet Explorer, kattintson a hello **fogaskerék ikonra** a hello jobb felső sarkában hello ablak.</span><span class="sxs-lookup"><span data-stu-id="e412b-135">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="e412b-136">Válassza ki **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e412b-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="e412b-137">(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e412b-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Egy minta hello diagnosztikai ablak](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="e412b-139">Nyissa meg toohello **adatvédelmi** lapra, majd **, törölje a jelet** hello feliratú jelölőnégyzet **letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor**</span><span class="sxs-lookup"><span data-stu-id="e412b-139">Go toohello **Privacy** tab, then **uncheck** hello checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Törölje a jelet letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="e412b-141">a módosítások, toosave minden Internet Explorer ablak bezárása, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="e412b-141">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-hello-access-panel-extension"></a><span data-ttu-id="e412b-142">Távolítsa el a hozzáférési Panel bővítmény hello</span><span class="sxs-lookup"><span data-stu-id="e412b-142">Uninstall hello Access Panel Extension</span></span>
<span data-ttu-id="e412b-143">toouninstall hello hozzáférési Panel bővítmény a számítógépről:</span><span class="sxs-lookup"><span data-stu-id="e412b-143">toouninstall hello Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="e412b-144">A billentyűzeten, nyomja le az ENTER hello **Windows kulcs** tooopen hello Start menüből.</span><span class="sxs-lookup"><span data-stu-id="e412b-144">On your keyboard, press hello **Windows key** tooopen hello Start menu.</span></span> <span data-ttu-id="e412b-145">Ha hello menü meg nyitva, beírhatja minden toodo keresést.</span><span class="sxs-lookup"><span data-stu-id="e412b-145">When hello menu is open, you can type anything toodo a search.</span></span> <span data-ttu-id="e412b-146">Írja be a "Vezérlőpult", és nyissa meg hello **Vezérlőpult** amikor megjelenik a hello keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="e412b-146">Type "Control Panel" and then open hello **Control Panel** when it appears in hello search results.</span></span>
   
    ![Keresse meg a Vezérlőpult](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="e412b-148">A hello jobb felső sarkában hello Vezérlőpult, módosítsa a hello **megtekintéséhez** beállítás túl**nagy ikonok**.</span><span class="sxs-lookup"><span data-stu-id="e412b-148">In hello top right corner of hello Control Panel, change hello **View by** option too**Large icons**.</span></span> <span data-ttu-id="e412b-149">Majd keresse meg és kattintson a hello **programok és szolgáltatások** gombra.</span><span class="sxs-lookup"><span data-stu-id="e412b-149">Then find and click hello **Programs and Features** button.</span></span>
   
    ![Átalakítás hello nézet tooshow nagy ikonok](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="e412b-151">Hello listájából válassza ki **hozzáférési Panel bővítmény**, és kattintson a hello hello **Eltávolítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e412b-151">From hello list, select **Access Panel Extension**, and hello click hello **Uninstall** button.</span></span>
   
    ![Kattintson az Eltávolítás gombra](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="e412b-153">Majd tooinstall hello bővítmény megpróbálhatja újra toosee Ha hello problémát sikerült megoldani.</span><span class="sxs-lookup"><span data-stu-id="e412b-153">You can then try tooinstall hello extension again toosee if hello problem has been resolved.</span></span>

<span data-ttu-id="e412b-154">Ha hibát tapasztal hello bővítmény eltávolítása, is eltávolíthatja azt hello segítségével [Microsoft javítsa ki az](https://go.microsoft.com/?linkid=9779673) eszköz.</span><span class="sxs-lookup"><span data-stu-id="e412b-154">If you encounter issues uninstalling hello extension, you can also remove it using hello [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="e412b-155">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="e412b-155">Related Articles</span></span>
* [<span data-ttu-id="e412b-156">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="e412b-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="e412b-157">Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e412b-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="e412b-158">Hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával</span><span class="sxs-lookup"><span data-stu-id="e412b-158">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

