---
title: "Hibaelhárítás az Azure hozzáférési Panel bővítményét az Internet Explorer |} Microsoft Docs"
description: "Hogyan kell az Internet Explorer bővítményt a saját alkalmazások portál telepítése a csoportházirenddel."
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
ms.openlocfilehash: 938d0b4046afa8c80eabe542f4541d0554948f5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="c5a1d-103">A hozzáférési Panel bővítményét az Internet Explorer hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="c5a1d-103">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="c5a1d-104">Ez a cikk a következő problémák megoldásához nyújt segítséget:</span><span class="sxs-lookup"><span data-stu-id="c5a1d-104">This article helps you troubleshoot the following problems:</span></span>

* <span data-ttu-id="c5a1d-105">Még nem érhető el az alkalmazásokat a saját alkalmazások portálon keresztül az Internet Explorer használata során.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-105">You're unable to access your apps through the My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="c5a1d-106">Megjelenik a "Szoftver telepítése" üzenet, annak ellenére, hogy már telepítette a szoftvert.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-106">You see the "Install Software" message even though you've already installed the software.</span></span>

<span data-ttu-id="c5a1d-107">Ha Ön rendszergazda, további információ: [központi telepítése a hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="c5a1d-107">If you are an admin, see also: [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-the-diagnostic-tool"></a><span data-ttu-id="c5a1d-108">A diagnosztikai eszköz futtatása</span><span class="sxs-lookup"><span data-stu-id="c5a1d-108">Run the Diagnostic Tool</span></span>
<span data-ttu-id="c5a1d-109">Ha letölti és a hozzáférési Panel diagnosztikai eszközt futtató diagnosztizálhatja a telepítési problémákat a hozzáférési Panel kiterjesztésű:</span><span class="sxs-lookup"><span data-stu-id="c5a1d-109">You can diagnose installation problems with the Access Panel Extension by downloading and running the Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="c5a1d-110">Ide kattintva letöltheti a diagnosztikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-110">Click here to download the diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="c5a1d-111">Nyissa meg a fájlt, és nyomja le az ENTER **az összes kibontása** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-111">Open the file, and press **Extract all** button.</span></span>
   
    ![Nyomja meg az összes kibontása](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="c5a1d-113">Nyomja le az **kibontása** gombra a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-113">Then press the **Extract** button to continue.</span></span>
   
    ![Nyomja le az Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="c5a1d-115">Futtassa az eszközt, kattintson a jobb gombbal a nevű fájlt **AccessPanelExtensionDiagnosticTool**, majd jelölje be **nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-115">To run the tool, right-click the file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Nyissa meg a > a Microsoft Windows alapú parancsfájlfuttató](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="c5a1d-117">Ekkor megjelenik a következő diagnosztikai ablak, amely leírja, milyen lehet még a telepítés.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-117">You will then see the following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![A diagnosztikai ablak minta](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="c5a1d-119">Kattintson a "**Igen**", hogy a program a talált hibák elhárításában.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-119">Click "**YES**" to let the program fix the issues that have been found.</span></span>
7. <span data-ttu-id="c5a1d-120">Ezek a módosítások mentéséhez zárja be a minden Internet Explorer ablakot, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-120">To save these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="c5a1d-121">Ha továbbra sem tud hozzáférni az alkalmazásokat, próbálkozzon az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-121">If you still can't access your apps, try the steps below.</span></span>

## <a name="check-that-the-access-panel-extension-is-enabled"></a><span data-ttu-id="c5a1d-122">Ellenőrizze, hogy engedélyezve van-e a hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="c5a1d-122">Check that the Access Panel Extension is enabled</span></span>
<span data-ttu-id="c5a1d-123">Ellenőrizheti, hogy a hozzáférési Panel bővítmény engedélyezve van-e az Internet Explorerben:</span><span class="sxs-lookup"><span data-stu-id="c5a1d-123">To verify that the Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="c5a1d-124">Az Internet Explorer, kattintson a **fogaskerék ikonra** a az ablak jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-124">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="c5a1d-125">Válassza ki **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="c5a1d-126">(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Kattintson az eszközök > Internetbeállítások](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="c5a1d-128">Kattintson a **programok** lapra, majd kattintson a **bővítmények kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-128">Click the **Programs** tab, then click the **Manage add-ons** button.</span></span>
   
    ![Kattintson a bővítmények kezelése](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="c5a1d-130">Ezen a párbeszédpanelen válassza ki a **hozzáférési Panel bővítmény** , majd a **engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-130">In this dialog, select **Access Panel Extension** and then click the **Enable** button.</span></span>
   
    ![Kattintson az Engedélyezés parancsra](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="c5a1d-132">A változtatások mentéséhez, zárjon be minden Internet Explorer-ablakban, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-132">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="c5a1d-133">Bővítmények engedélyezése az InPrivate-böngészés</span><span class="sxs-lookup"><span data-stu-id="c5a1d-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="c5a1d-134">Ha az InPrivate-böngészés mód használata esetén:</span><span class="sxs-lookup"><span data-stu-id="c5a1d-134">If you are using the InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="c5a1d-135">Az Internet Explorer, kattintson a **fogaskerék ikonra** a az ablak jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-135">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="c5a1d-136">Válassza ki **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="c5a1d-137">(Az Internet Explorer korábbi verzióiban ez alatt található **eszközök > Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![A diagnosztikai ablak minta](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="c5a1d-139">Lépjen a **adatvédelmi** lapra, majd **, törölje a jelet** feliratú jelölőnégyzet **letilthatja a eszköztárak és kiterjesztések, ha az InPrivate-böngészés indítása**</span><span class="sxs-lookup"><span data-stu-id="c5a1d-139">Go to the **Privacy** tab, then **uncheck** the checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Törölje a jelet letiltása eszköztárak és kiterjesztések InPrivate-böngészés indításakor](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="c5a1d-141">A változtatások mentéséhez, zárjon be minden Internet Explorer-ablakban, és ezután nyissa meg az Internet Explorert.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-141">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-the-access-panel-extension"></a><span data-ttu-id="c5a1d-142">A hozzáférési Panel-kiterjesztés eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c5a1d-142">Uninstall the Access Panel Extension</span></span>
<span data-ttu-id="c5a1d-143">A hozzáférési Panel bővítmény eltávolítása a számítógépről:</span><span class="sxs-lookup"><span data-stu-id="c5a1d-143">To uninstall the Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="c5a1d-144">A billentyűzeten, nyomja meg a **Windows kulcs** a Start menü megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-144">On your keyboard, press the **Windows key** to open the Start menu.</span></span> <span data-ttu-id="c5a1d-145">Ha meg nyitva a menü, beírhatja semmit való kereséshez.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-145">When the menu is open, you can type anything to do a search.</span></span> <span data-ttu-id="c5a1d-146">Írja be a "Vezérlőpult", majd nyissa meg a **Vezérlőpult** amikor megjelenik a keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-146">Type "Control Panel" and then open the **Control Panel** when it appears in the search results.</span></span>
   
    ![Keresse meg a Vezérlőpult](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="c5a1d-148">A jobb felső sarkában a Vezérlőpulton, módosítsa a **megtekintéséhez** lehetőséggel **nagy ikonok**.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-148">In the top right corner of the Control Panel, change the **View by** option to **Large icons**.</span></span> <span data-ttu-id="c5a1d-149">Majd keresse meg és kattintson a **programok és szolgáltatások** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-149">Then find and click the **Programs and Features** button.</span></span>
   
    ![A nézet cseréli a nagy ikonok megjelenítése](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="c5a1d-151">Válassza ki a listáról, **hozzáférési Panel bővítmény**, majd kattintson a **Eltávolítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-151">From the list, select **Access Panel Extension**, and the click the **Uninstall** button.</span></span>
   
    ![Kattintson az Eltávolítás gombra](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="c5a1d-153">Ezután próbálkozzon a bővítmény ismételt használatával ellenőrizheti, ha a probléma megoldódott-e telepíteni.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-153">You can then try to install the extension again to see if the problem has been resolved.</span></span>

<span data-ttu-id="c5a1d-154">Ha problémák lépnek fel a bővítmény eltávolítása, is eltávolíthatja azt használja a [Microsoft javítsa ki az](https://go.microsoft.com/?linkid=9779673) eszköz.</span><span class="sxs-lookup"><span data-stu-id="c5a1d-154">If you encounter issues uninstalling the extension, you can also remove it using the [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="c5a1d-155">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="c5a1d-155">Related Articles</span></span>
* [<span data-ttu-id="c5a1d-156">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="c5a1d-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="c5a1d-157">Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c5a1d-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c5a1d-158">A hozzáférési Panel bővítmény telepítése az Internet Explorer csoportházirend használatával</span><span class="sxs-lookup"><span data-stu-id="c5a1d-158">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

