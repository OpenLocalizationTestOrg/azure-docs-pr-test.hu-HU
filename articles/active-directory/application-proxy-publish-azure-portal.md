---
title: "az Azure AD alkalmazásproxy aaaPublish alkalmazások |} Microsoft Docs"
description: "A helyszíni alkalmazások toohello felhőalapú Azure AD-alkalmazásproxyval közzététele a hello Azure-portálon."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="feade-103">Alkalmazások közzététele az Azure AD-alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="feade-103">Publish applications using Azure AD Application Proxy</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="feade-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="feade-104">Azure portal</span></span>](application-proxy-publish-azure-portal.md)
> * [<span data-ttu-id="feade-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="feade-105">Azure classic portal</span></span>](active-directory-application-proxy-publish.md)

<span data-ttu-id="feade-106">Azure Active Directory (AD) alkalmazásproxy segít a távoli dolgozók támogatja a helyszíni alkalmazások toobe hello keresztül elérhető közzétételével internet.</span><span class="sxs-lookup"><span data-stu-id="feade-106">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications toobe accessed over hello internet.</span></span> <span data-ttu-id="feade-107">Ezeket az alkalmazásokat hello Azure portál tooprovide biztonságos távoli hozzáférés a keresztül közzéteheti a hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="feade-107">You can publish these applications through hello Azure portal tooprovide secure remote access from outside your network.</span></span>

<span data-ttu-id="feade-108">Ez a cikk bemutatja, hogyan hello lépéseket toopublish az alkalmazásproxy a helyszíni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="feade-108">This article walks you through hello steps toopublish an on-premises app with Application Proxy.</span></span> <span data-ttu-id="feade-109">Ez a cikk befejezése után fog-e a felhasználók kell tudni tooaccess az alkalmazás távolról.</span><span class="sxs-lookup"><span data-stu-id="feade-109">After you complete this article, your users will be able tooaccess your app remotely.</span></span> <span data-ttu-id="feade-110">– Ekkor készen áll a tooconfigure különleges funkciókat hello alkalmazáshoz hasonlóan az egyszeri bejelentkezés, a személyre szabott adatok és a biztonsági követelményeket.</span><span class="sxs-lookup"><span data-stu-id="feade-110">And you'll be ready tooconfigure extra features for hello application like single sign-on, personalized information, and security requirements.</span></span>

<span data-ttu-id="feade-111">Ha új tooApplication Proxy, kapcsolatos további Ez a szolgáltatás hello cikk [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="feade-111">If you're new tooApplication Proxy, learn more about this feature with hello article [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>


## <a name="publish-an-on-premises-app-for-remote-access"></a><span data-ttu-id="feade-112">A távoli hozzáférés a helyszíni alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="feade-112">Publish an on-premises app for remote access</span></span>

<span data-ttu-id="feade-113">Kövesse ezeket a lépéseket toopublish az alkalmazásproxy alkalmazásait.</span><span class="sxs-lookup"><span data-stu-id="feade-113">Follow these steps toopublish your apps with Application Proxy.</span></span> <span data-ttu-id="feade-114">Ha még nem már letöltötte és összekötő konfigurálva a szervezet számára, nyissa meg túl[az alkalmazásproxy első lépései és hello összekötő telepítéséhez](active-directory-application-proxy-enable.md) első, majd tegye közzé az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="feade-114">If you haven't already downloaded and configured a connector for your organization, go too[Get started with Application Proxy and install hello connector](active-directory-application-proxy-enable.md) first, and then publish your app.</span></span>

> [!TIP]
> <span data-ttu-id="feade-115">Ha a tesztelést kimenő alkalmazásproxy hello az első alkalommal, válasszon olyan alkalmazás, amely jelszóalapú hitelesítés van beállítva.</span><span class="sxs-lookup"><span data-stu-id="feade-115">If you're testing out Application Proxy for hello first time, choose an application that's set up for password-based authentication.</span></span> <span data-ttu-id="feade-116">Alkalmazásproxy más típusú hitelesítés támogatja, de jelszó-alapú alkalmazások hello legegyszerűbb tooget fel, akinek gyorsan.</span><span class="sxs-lookup"><span data-stu-id="feade-116">Application Proxy supports other types of authentication, but password-based apps are hello easiest tooget up and running quickly.</span></span> 

1. <span data-ttu-id="feade-117">Jelentkezzen be rendszergazdaként a hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="feade-117">Sign in as an administrator in hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="feade-118">Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="feade-118">Select **Azure Active Directory** > **Enterprise applications** > **New application**.</span></span>

  ![Vállalati alkalmazás hozzáadása](./media/application-proxy-publish-azure-portal/add-app.png)

3. <span data-ttu-id="feade-120">Válassza ki **összes**, majd jelölje be **helyszíni alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="feade-120">Select **All**, then select **On-premises application**.</span></span>  

  ![Saját alkalmazás felvétele](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. <span data-ttu-id="feade-122">Adja meg a következő információkat az alkalmazásról hello:</span><span class="sxs-lookup"><span data-stu-id="feade-122">Provide hello following information about your application:</span></span>

   - <span data-ttu-id="feade-123">**Név**: hello alkalmazás, amely megjelenik majd hello hozzáférési panel és hello Azure-portálon a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="feade-123">**Name**: hello name of hello application that will appear on hello access panel and in hello Azure portal.</span></span> 

   - <span data-ttu-id="feade-124">**Belső URL-cím**: hello URL-cím a magánhálózaton belül használt tooaccess hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="feade-124">**Internal URL**: hello URL that you use tooaccess hello application from inside your private network.</span></span> <span data-ttu-id="feade-125">Megadott elérési útra hello háttér server toopublish biztosíthat, amíg hello rest hello kiszolgáló nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="feade-125">You can provide a specific path on hello backend server toopublish, while hello rest of hello server is unpublished.</span></span> <span data-ttu-id="feade-126">Ezzel a módszerrel közzéteheti a különböző helyek hello a különböző alkalmazások ugyanarra a kiszolgálóra, és mindegyiknek saját nevet és hozzáférési szabályokat.</span><span class="sxs-lookup"><span data-stu-id="feade-126">In this way, you can publish different sites on hello same server as different apps, and give each one its own name and access rules.</span></span>

     > [!TIP]
     > <span data-ttu-id="feade-127">Ha közzéteszi a egy elérési utat, győződjön meg arról, hogy minden hello szükséges lemezképek, parancsprogramok és stíluslapok, az alkalmazás tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="feade-127">If you publish a path, make sure that it includes all hello necessary images, scripts, and style sheets for your application.</span></span> <span data-ttu-id="feade-128">Például ha az alkalmazás https://yourapp/app, illetve lemezkép https://yourapp/media helyen, majd kell közzé tenni https://yourapp/ hello elérési útjával.</span><span class="sxs-lookup"><span data-stu-id="feade-128">For example, if your app is at https://yourapp/app and uses images located at https://yourapp/media, then you should publish https://yourapp/ as hello path.</span></span> <span data-ttu-id="feade-129">A belső URL-cím nincs toobe hello kezdőlapja a felhasználók látni.</span><span class="sxs-lookup"><span data-stu-id="feade-129">This internal URL doesn't have toobe hello landing page your users see.</span></span> <span data-ttu-id="feade-130">További információkért lásd: [állítsa be a közzétett alkalmazások egyéni kezdőlapját](application-proxy-office365-app-launcher.md).</span><span class="sxs-lookup"><span data-stu-id="feade-130">For more information, see [Set a custom home page for published apps](application-proxy-office365-app-launcher.md).</span></span>

   - <span data-ttu-id="feade-131">**Külső URL-cím**: hello cím kerül, hogy a felhasználók tooin rendelés tooaccess hello alkalmazást, a hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="feade-131">**External URL**: hello address your users will go tooin order tooaccess hello app from outside your network.</span></span> <span data-ttu-id="feade-132">Ha nem szeretné toouse hello alapértelmezett alkalmazásproxy-tartományt, olvassa el [egyéni tartományok az Azure AD alkalmazásproxy](active-directory-application-proxy-custom-domains.md).</span><span class="sxs-lookup"><span data-stu-id="feade-132">If you don't want toouse hello default Application Proxy domain, read about [custom domains in Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).</span></span>
   - <span data-ttu-id="feade-133">**Előzetes hitelesítésének**: hogyan alkalmazásproxy access tooyour alkalmazás engedélyezése előtt ellenőrzi a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="feade-133">**Pre Authentication**: How Application Proxy verifies users before giving them access tooyour application.</span></span> 

     - <span data-ttu-id="feade-134">Az Azure Active Directory: Alkalmazásproxy átirányítja a felhasználók toosign hitelesíti a rájuk vonatkozó engedélyek hello címtár és az alkalmazás az Azure AD-be.</span><span class="sxs-lookup"><span data-stu-id="feade-134">Azure Active Directory: Application Proxy redirects users toosign in with Azure AD, which authenticates their permissions for hello directory and application.</span></span> <span data-ttu-id="feade-135">Azt javasoljuk, hogy ez a beállítás hello alapértelmezett, így például a feltételes hozzáférés és a többtényezős hitelesítés az Azure AD biztonsági funkciók előnyeit élvezheti.</span><span class="sxs-lookup"><span data-stu-id="feade-135">We recommend keeping this option as hello default, so that you can take advantage of Azure AD security features like conditional access and Multi-Factor Authentication.</span></span>
     - <span data-ttu-id="feade-136">Átengedés: A felhasználóknak nem kell tooauthenticate Azure Active Directory tooaccess hello alkalmazás ellen.</span><span class="sxs-lookup"><span data-stu-id="feade-136">Passthrough: Users don't have tooauthenticate against Azure Active Directory tooaccess hello application.</span></span> <span data-ttu-id="feade-137">Hitelesítési követelmények hello háttérkiszolgálón továbbra is állíthat be.</span><span class="sxs-lookup"><span data-stu-id="feade-137">You can still set up authentication requirements on hello backend.</span></span>
   - <span data-ttu-id="feade-138">**Összekötő csoport**: összekötők folyamat hello távelérési tooyour alkalmazás és az összekötő csoportok megkönnyítik, összekötők és alkalmazások régió, hálózati vagy célja.</span><span class="sxs-lookup"><span data-stu-id="feade-138">**Connector Group**: Connectors process hello remote access tooyour application, and connector groups help you organize connectors and apps by region, network, or purpose.</span></span> <span data-ttu-id="feade-139">Ha nincs még létrehozva összekötő csoportok, az alkalmazás túl van-e hozzárendelve**alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="feade-139">If you don't have any connector groups created yet, your app is assigned too**Default**.</span></span>

   ![Az alkalmazás konfigurálása](./media/application-proxy-publish-azure-portal/configure-app.png)
5. <span data-ttu-id="feade-141">Szükség esetén további beállításokat.</span><span class="sxs-lookup"><span data-stu-id="feade-141">If necessary, configure additional settings.</span></span> <span data-ttu-id="feade-142">A legtöbb alkalmazás esetén kell venni ezeket a beállításokat az alapértelmezett állapotra.</span><span class="sxs-lookup"><span data-stu-id="feade-142">For most applications, you should keep these settings in their default states.</span></span> 
   - <span data-ttu-id="feade-143">**Háttér-alkalmazás időtúllépés**: az érték túl**hosszú** csak akkor, ha az alkalmazás tooauthenticate lassú, és csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="feade-143">**Backend Application Timeout**: Set this value too**Long** only if your application is slow tooauthenticate and connect.</span></span> 
   - <span data-ttu-id="feade-144">**Fejlécek URL-címek fordítása**: ezt az értéket tartsa **Igen** kivéve, ha az alkalmazás szükséges hello eredeti szereplő állomás fejlécével hello hitelesítési kérelmet.</span><span class="sxs-lookup"><span data-stu-id="feade-144">**Translate URLs in Headers**: Keep this value as **Yes** unless your application required hello original host header in hello authentication request.</span></span>
   - <span data-ttu-id="feade-145">**A kérelem törzsében URL-címek fordítása**: tartani ezt az értéket **nem** kivéve, ha szoftveresen kötött HTML hivatkozások tooother helyszíni alkalmazásokkal rendelkezik, és ne használjon egyéni tartományokat.</span><span class="sxs-lookup"><span data-stu-id="feade-145">**Translate URLs in Application Body**: Keep this value as **No** unless you have hardcoded HTML links tooother on-premises applications, and don't use custom domains.</span></span> <span data-ttu-id="feade-146">További információkért lásd: [hivatkozásra az alkalmazásproxy fordítási](application-proxy-link-translation.md).</span><span class="sxs-lookup"><span data-stu-id="feade-146">For more information, see [Link translation with Application Proxy](application-proxy-link-translation.md).</span></span>
   
   ![Az alkalmazás konfigurálása](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. <span data-ttu-id="feade-148">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="feade-148">Select **Add**.</span></span>


## <a name="add-a-test-user"></a><span data-ttu-id="feade-149">Teszt felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="feade-149">Add a test user</span></span> 

<span data-ttu-id="feade-150">az alkalmazás megfelelően, közzétett tootest teszt felhasználói fiók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="feade-150">tootest that your app was published correctly, add a test user account.</span></span> <span data-ttu-id="feade-151">Ellenőrizze, hogy ez a fiók rendelkezik-e engedélyek tooaccess hello alkalmazásából hello vállalati hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="feade-151">Verify that this account already has permissions tooaccess hello app from inside hello corporate network.</span></span>

1. <span data-ttu-id="feade-152">Vissza a hello – első lépések panelen válassza ki a **rendelje hozzá a felhasználót tesztelési**.</span><span class="sxs-lookup"><span data-stu-id="feade-152">Back on hello Quick start blade, select **Assign a user for testing**.</span></span>

  ![Rendelje hozzá a felhasználót teszteléshez](./media/application-proxy-publish-azure-portal/assign-user.png)

2. <span data-ttu-id="feade-154">Hello felhasználók és csoportok panelen, jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="feade-154">On hello Users and groups blade, select **Add**.</span></span>

  ![Egy felhasználó vagy csoport hozzáadása](./media/application-proxy-publish-azure-portal/add-user.png)

3. <span data-ttu-id="feade-156">A hello Hozzáadás hozzárendelés panelen válassza ki a **felhasználók és csoportok** kattintson a kívánt tooadd hello fiók.</span><span class="sxs-lookup"><span data-stu-id="feade-156">On hello Add assignment blade, select **Users and groups** then choose hello account you want tooadd.</span></span> 
4. <span data-ttu-id="feade-157">Válassza ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="feade-157">Select **Assign**.</span></span>

## <a name="test-your-published-app"></a><span data-ttu-id="feade-158">A közzétett alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="feade-158">Test your published app</span></span>

<span data-ttu-id="feade-159">A böngészőben nyissa meg a toohello külső URL-cím hello során konfigurált lépés közzététele.</span><span class="sxs-lookup"><span data-stu-id="feade-159">In your browser, navigate toohello external URL that you configured during hello publish step.</span></span> <span data-ttu-id="feade-160">Hello kezdőképernyőn kell megjelennie, és képes toosign be hello teszt fiókkal akkor állítható be.</span><span class="sxs-lookup"><span data-stu-id="feade-160">You should see hello start screen, and be able toosign in with hello test account you set up.</span></span>

![A közzétett alkalmazás tesztelése](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a><span data-ttu-id="feade-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="feade-162">Next steps</span></span>
- <span data-ttu-id="feade-163">[Töltse le az összekötők](active-directory-application-proxy-enable.md) és [összekötő csoportok létrehozása a](active-directory-application-proxy-connectors-azure-portal.md) toopublish alkalmazások külön hálózatok és helyek.</span><span class="sxs-lookup"><span data-stu-id="feade-163">[Download connectors](active-directory-application-proxy-enable.md) and [create connector groups](active-directory-application-proxy-connectors-azure-portal.md) toopublish applications on separate networks and locations.</span></span>

- <span data-ttu-id="feade-164">[Egyszeri bejelentkezés beállítása](application-proxy-sso-azure-portal.md) az újonnan közzétett alkalmazások</span><span class="sxs-lookup"><span data-stu-id="feade-164">[Set up single sign-on](application-proxy-sso-azure-portal.md) for your newly published app</span></span>
