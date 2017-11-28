---
title: "aaaHeader-alapú hitelesítés PingAccess az az Azure AD alkalmazásproxy |} Microsoft Docs"
description: "Alkalmazások közzététele az PingAccess és alkalmazás Proxy toosupport fejléc-alapú hitelesítés."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="9ec93-103">Az egyszeri bejelentkezés az alkalmazásproxy és PingAccess fejléc-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="9ec93-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="9ec93-104">Az Azure Active Directory Alkalmazásproxyjával és PingAccess közösen együtt tooprovide Azure Active Directory felhasználók hozzáférési tooeven további alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9ec93-104">Azure Active Directory Application Proxy and PingAccess have partnered together tooprovide Azure Active Directory customers with access tooeven more applications.</span></span> <span data-ttu-id="9ec93-105">PingAccess bővíti hello [meglévő alkalmazásproxy ajánlatok](active-directory-application-proxy-get-started.md) tooinclude egyszeri bejelentkezéses hozzáférést tooapplications, fejlécek használata a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="9ec93-105">PingAccess expands hello [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) tooinclude single sign-on access tooapplications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="9ec93-106">Mi az az PingAccess az Azure AD?</span><span class="sxs-lookup"><span data-stu-id="9ec93-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="9ec93-107">Az Azure Active Directory PingAccess egy ajánlat, amely lehetővé teszi a toogive felhasználók hozzáférést PingAccess és egyszeri bejelentkezés tooapplications, fejlécek használata a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="9ec93-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you toogive users access and single sign-on tooapplications that use headers for authentication.</span></span> <span data-ttu-id="9ec93-108">Alkalmazásproxy ezekhez az alkalmazásokhoz, mint bármely más, az Azure AD tooauthenticate hozzáféréssel, és a majd áthaladó forgalmat keresztül hello összekötő szolgáltatás kezeli.</span><span class="sxs-lookup"><span data-stu-id="9ec93-108">Application Proxy treats these apps like any other, using Azure AD tooauthenticate access and then passing traffic through hello connector service.</span></span> <span data-ttu-id="9ec93-109">PingAccess hello alkalmazások előtt helyezkedik el, és hello hozzáférési jogkivonatot az Azure AD fordítja fejléc, hogy hello alkalmazás hello hitelesítési kap az elolvashatják hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="9ec93-109">PingAccess sits in front of hello apps and translates hello access token from Azure AD into a header so that hello application receives hello authentication in hello format it can read.</span></span>

<span data-ttu-id="9ec93-110">A felhasználók nem bármilyen figyelje, amikor bejelentkeznek a toouse a vállalati alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="9ec93-110">Your users won’t notice anything different when they sign in toouse your corporate apps.</span></span> <span data-ttu-id="9ec93-111">Akkor is még mindig bárhonnan dolgozhatnak bármilyen eszközön.</span><span class="sxs-lookup"><span data-stu-id="9ec93-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="9ec93-112">Hello alkalmazásproxy összekötők távoli forgalom tooall alkalmazásoknak a hitelesítési típus közvetlen, mivel azok tooload egyenleg automatikusan, illetve fogja folytatni.</span><span class="sxs-lookup"><span data-stu-id="9ec93-112">Since hello Application Proxy connectors direct remote traffic tooall apps regardless of their authentication type, they’ll continue tooload balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="9ec93-113">Hogyan szerezhetek hozzáférést?</span><span class="sxs-lookup"><span data-stu-id="9ec93-113">How do I get access?</span></span>

<span data-ttu-id="9ec93-114">Mivel ebben a forgatókönyvben az Azure Active Directory és PingAccess közötti partnerség kínált, licencek kell mindkét szolgáltatás esetében.</span><span class="sxs-lookup"><span data-stu-id="9ec93-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="9ec93-115">Azonban az Azure Active Directory Premium előfizetéshez tartozik egy PingAccess alaplicenc, amely lefedi too20 alkalmazások fel.</span><span class="sxs-lookup"><span data-stu-id="9ec93-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up too20 applications.</span></span> <span data-ttu-id="9ec93-116">Ha több mint 20 fejléc-alapú alkalmazások kell toopublish, PingAccess vásárolhat további licencre.</span><span class="sxs-lookup"><span data-stu-id="9ec93-116">If you need toopublish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="9ec93-117">További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).</span><span class="sxs-lookup"><span data-stu-id="9ec93-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="9ec93-118">Az alkalmazás közzététele az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="9ec93-118">Publish your application in Azure</span></span>

<span data-ttu-id="9ec93-119">Ez a cikk tesznek közzé egy alkalmazást ebben a forgatókönyvben hello először személyek számára készült.</span><span class="sxs-lookup"><span data-stu-id="9ec93-119">This article is intended for people who are publishing an app with this scenario for hello first time.</span></span> <span data-ttu-id="9ec93-120">Az végigvezeti hogyan tooget el az alkalmazás- és PingAccess, továbbá toohello közzétételi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9ec93-120">It walks through how tooget started with both Application and PingAccess, in addition toohello publishing steps.</span></span> <span data-ttu-id="9ec93-121">Ha már beállított mindkét szolgáltatás, de a lépések közzétételi hello frissítő, hello összekötő telepítése, és helyezze át, amely túl[adja hozzá az alkalmazás tooAzure AD az alkalmazásproxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="9ec93-121">If you’ve already configured both services but want a refresher on hello publishing steps, you can skip hello connector installation and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="9ec93-122">Mivel ez a forgatókönyv az Azure AD közötti partnerség és PingAccess, bizonyos hello utasítások találhatók a hello Ping identitás hely.</span><span class="sxs-lookup"><span data-stu-id="9ec93-122">Since this scenario is a partnership between Azure AD and PingAccess, some of hello instructions exist on hello Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="9ec93-123">Az alkalmazásproxy-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="9ec93-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="9ec93-124">Ha már Application Proxy engedélyezve van, és egy összekötő telepítve van, hagyja ki ezt a szakaszt, és helyezze át, amely túl[adja hozzá az alkalmazás tooAzure AD az alkalmazásproxy](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="9ec93-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="9ec93-125">hello alkalmazásproxy-összekötő egy Windows Server szolgáltatás, amely arra utasítja a távoli alkalmazottak tooyour hello forgalmát közzétett alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ec93-125">hello Application Proxy connector is a Windows Server service that directs hello traffic from your remote employees tooyour published apps.</span></span> <span data-ttu-id="9ec93-126">Telepítési utasításokat, lásd: [alkalmazásproxy engedélyezése az Azure portál hello](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="9ec93-126">For more detailed installation instructions, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="9ec93-127">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ec93-127">Sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="9ec93-128">Válassza ki **Azure Active Directory** > **alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="9ec93-129">Válassza ki **összekötő letöltése** toostart hello Application Proxy connector download.</span><span class="sxs-lookup"><span data-stu-id="9ec93-129">Select **Download Connector** toostart hello Application Proxy connector download.</span></span> <span data-ttu-id="9ec93-130">Kövesse a hello telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="9ec93-130">Follow hello installation instructions.</span></span>

   ![Alkalmazásproxy engedélyezése és hello összekötő letöltése](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="9ec93-132">Hello összekötő letöltése kell automatikusan alkalmazásproxy engedélyezése a címtáron, de ha nem választhat **alkalmazásproxy engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-132">Downloading hello connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a><span data-ttu-id="9ec93-133">Adja hozzá az alkalmazás tooAzure AD-alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="9ec93-133">Add your app tooAzure AD with Application Proxy</span></span>

<span data-ttu-id="9ec93-134">Nincsenek két műveletet kell tootake a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9ec93-134">There are two actions you need tootake in hello Azure portal.</span></span> <span data-ttu-id="9ec93-135">Először toopublish az alkalmazás az alkalmazásproxy.</span><span class="sxs-lookup"><span data-stu-id="9ec93-135">First, you need toopublish your application with Application Proxy.</span></span> <span data-ttu-id="9ec93-136">Ezt követően kell toocollect hello app hello PingAccess lépések során használható kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="9ec93-136">Then, you need toocollect some information about hello app that you can use during hello PingAccess steps.</span></span>

<span data-ttu-id="9ec93-137">Kövesse ezeket a lépéseket toopublish az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ec93-137">Follow these steps toopublish your app.</span></span> <span data-ttu-id="9ec93-138">A részletes lépéseket bemutató 1-8, tekintse meg a még [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9ec93-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="9ec93-139">Ha nem hello utolsó szakaszában, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ec93-139">If you didn't in hello last section, sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="9ec93-140">Válassza ki **Azure Active Directory** > **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="9ec93-141">Válassza ki **Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="9ec93-141">Select **Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="9ec93-142">Válassza ki **helyszíni alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="9ec93-143">Töltse ki az új alkalmazás adatait tartalmazó hello kötelező mezőket.</span><span class="sxs-lookup"><span data-stu-id="9ec93-143">Fill out hello required fields with information about your new app.</span></span> <span data-ttu-id="9ec93-144">Útmutató a hello-beállítások a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="9ec93-144">Use hello following guidance for hello settings:</span></span>
   - <span data-ttu-id="9ec93-145">**Belső URL-cím**: általában hello URL-címet, amely toohello app bejelentkezési oldal Ha hello a vállalati hálózaton már megadta.</span><span class="sxs-lookup"><span data-stu-id="9ec93-145">**Internal URL**: Normally you provide hello URL that takes you toohello app’s sign in page when you’re on hello corporate network.</span></span> <span data-ttu-id="9ec93-146">Ebben a forgatókönyvben hello összekötő tootreat hello PingAccess proxy hello alkalmazás hello első lapként van szüksége.</span><span class="sxs-lookup"><span data-stu-id="9ec93-146">For this scenario hello connector needs tootreat hello PingAccess proxy as hello front page of hello app.</span></span> <span data-ttu-id="9ec93-147">Ezt a formátumot használja: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="9ec93-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="9ec93-148">hello port 3000 alapértelmezés szerint, de a PingAccess konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="9ec93-148">hello port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="9ec93-149">**Előhitelesítési módszer**: Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="9ec93-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="9ec93-150">**URL-címet a fejlécekben fordítása**: nincs</span><span class="sxs-lookup"><span data-stu-id="9ec93-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="9ec93-151">Ha ez az első alkalmazás, port 3000 toostart használja, és térjen vissza tooupdate ezt a beállítást, ha az PingAccess konfigurációjának módosítását.</span><span class="sxs-lookup"><span data-stu-id="9ec93-151">If this is your first application, use port 3000 toostart and come back tooupdate this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="9ec93-152">Ha ez a második vagy újabb alkalmazáshoz, ez kell toomatch hello figyelő PingAccess konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="9ec93-152">If this is your second or later app, this will need toomatch hello Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="9ec93-153">További információ [PingAccess a figyelők](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="9ec93-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="9ec93-154">Válassza ki **Hozzáadás** hello hello panel alsó részén.</span><span class="sxs-lookup"><span data-stu-id="9ec93-154">Select **Add** at hello bottom of hello blade.</span></span> <span data-ttu-id="9ec93-155">Az alkalmazás kerül, és hello quick start menü megnyitása.</span><span class="sxs-lookup"><span data-stu-id="9ec93-155">Your application is added, and hello quick start menu opens.</span></span>
7. <span data-ttu-id="9ec93-156">Hello gyors üzembe helyezési menüben válasszon ki **rendelje hozzá a felhasználót tesztelési**, és legalább egy felhasználói toohello alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9ec93-156">In hello quick start menu, select **Assign a user for testing**, and add at least one user toohello application.</span></span> <span data-ttu-id="9ec93-157">Ellenőrizze, hogy a teszt fiók rendelkezik hozzáféréssel toohello a helyszíni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9ec93-157">Make sure this test account has access toohello on-premises application.</span></span>
8. <span data-ttu-id="9ec93-158">Válassza ki **hozzárendelése** toosave hello teszt felhasználó hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="9ec93-158">Select **Assign** toosave hello test user assignment.</span></span>
9. <span data-ttu-id="9ec93-159">Hello felügyeleti panelen jelölje ki **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-159">On hello app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="9ec93-160">Válasszon **fejléc-alapú bejelentkezés** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="9ec93-160">Choose **Header-based sign-on** from hello drop-down menu.</span></span> <span data-ttu-id="9ec93-161">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ec93-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="9ec93-162">Ha ez az első alkalommal használja a fejléc-alapú egyszeri bejelentkezést, tooinstall PingAccess kell.</span><span class="sxs-lookup"><span data-stu-id="9ec93-162">If this is your first time using header-based single sign-on, you need tooinstall PingAccess.</span></span> <span data-ttu-id="9ec93-163">toomake meg arról, hogy az Azure-előfizetéshez tartozik automatikusan az PingAccess telepítése, az egyszeri bejelentkezési oldalra toodownload PingAccess a hello kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="9ec93-163">toomake sure your Azure subscription is automatically associated with your PingAccess installation, use hello link on this single sign-on page toodownload PingAccess.</span></span> <span data-ttu-id="9ec93-164">Most nyissa meg a letöltési hely hello, vagy visszatérhet, toothis lap később.</span><span class="sxs-lookup"><span data-stu-id="9ec93-164">You can open hello download site now, or come back toothis page later.</span></span> 

   ![Válassza ki a fejléc-alapú bejelentkezés](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="9ec93-166">Hello vállalati alkalmazások panel bezárása, vagy minden hello módon toohello bal oldali tooreturn toohello Azure Active Directory menü görgessen.</span><span class="sxs-lookup"><span data-stu-id="9ec93-166">Close hello Enterprise applications blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
12. <span data-ttu-id="9ec93-167">Válassza ki **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-167">Select **App registrations**.</span></span>

   ![Válassza ki az alkalmazás-regisztráció](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="9ec93-169">Az előzőekben adott hozzá, majd jelölje be hello app **válasz URL-címek**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-169">Select hello app you just added, then **Reply URLs**.</span></span>

   ![Válassza ki a válasz URL-címek](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="9ec93-171">Ellenőrizze a toosee, ha hello külső URL-címe, hogy hozzárendelt tooyour alkalmazást az 5 hello válasz URL-címei listáján.</span><span class="sxs-lookup"><span data-stu-id="9ec93-171">Check toosee if hello external URL that you assigned tooyour app in step 5 is in hello Reply URLs list.</span></span> <span data-ttu-id="9ec93-172">Ha nem, vegye fel azt.</span><span class="sxs-lookup"><span data-stu-id="9ec93-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="9ec93-173">A hello alkalmazás-beállítások panelen válassza ki a **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-173">On hello app settings blade, select **Required permissions**.</span></span>

  ![Válassza ki a szükséges engedélyek](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="9ec93-175">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9ec93-175">Select **Add**.</span></span> <span data-ttu-id="9ec93-176">Hello API, válasszon **Windows Azure Active Directory**, majd **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-176">For hello API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="9ec93-177">Hello engedélyek, válassza **olvasási és írása az összes alkalmazás** és **jelentkezzen be a felhasználói profil olvasása és**, majd **válassza ki** és **végzett**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-177">For hello permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Engedélyek kiválasztása](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a><span data-ttu-id="9ec93-179">Információgyűjtés a hello PingAccess lépéseket</span><span class="sxs-lookup"><span data-stu-id="9ec93-179">Collect information for hello PingAccess steps</span></span>

1. <span data-ttu-id="9ec93-180">Válassza a beállítások panelen **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-180">On your app settings blade, select **Properties**.</span></span> 

  ![Válassza a Tulajdonságok](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="9ec93-182">Mentse a hello **alkalmazásazonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="9ec93-182">Save hello **Application Id** value.</span></span> <span data-ttu-id="9ec93-183">Ügyfél-azonosító hello szolgál PingAccess konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="9ec93-183">This is used for hello client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="9ec93-184">A hello alkalmazás-beállítások panelen válassza ki a **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-184">On hello app settings blade, select **Keys**.</span></span>

  ![Válassza ki a kulcsok](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="9ec93-186">Hozzon létre egy kulcsot a fő leírás, majd válassza a lejárati dátum hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="9ec93-186">Create a key by entering a key description and choosing an expiration date from hello drop-down menu.</span></span>
5. <span data-ttu-id="9ec93-187">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ec93-187">Select **Save**.</span></span> <span data-ttu-id="9ec93-188">Megjelenik egy GUID hello **érték** mező.</span><span class="sxs-lookup"><span data-stu-id="9ec93-188">A GUID appears in hello **Value** field.</span></span>

  <span data-ttu-id="9ec93-189">Mentse ezt az értéket, nem fogja tudni toosee az után megismételni az ablak bezárása.</span><span class="sxs-lookup"><span data-stu-id="9ec93-189">Save this value now, as you won’t be able toosee it again after you close this window.</span></span>

  ![Hozzon létre egy új kulcsot](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="9ec93-191">Hello App regisztrációk panel bezárása, vagy minden hello módon toohello bal oldali tooreturn toohello Azure Active Directory menü görgessen.</span><span class="sxs-lookup"><span data-stu-id="9ec93-191">Close hello App registrations blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
7. <span data-ttu-id="9ec93-192">Válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-192">Select **Properties**.</span></span>
8. <span data-ttu-id="9ec93-193">Mentse a hello **könyvtár-azonosítója** GUID.</span><span class="sxs-lookup"><span data-stu-id="9ec93-193">Save hello **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-toosend-custom-fields"></a><span data-ttu-id="9ec93-194">Nem kötelező – a frissítés GraphAPI toosend egyéni mezők</span><span class="sxs-lookup"><span data-stu-id="9ec93-194">Optional - Update GraphAPI toosend custom fields</span></span>

<span data-ttu-id="9ec93-195">A biztonsági jogkivonatok, amelyek az Azure AD küld a hitelesítéshez listájáért lásd: [az Azure AD-jogkivonatok referenciájából](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="9ec93-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="9ec93-196">Egy egyéni jogcím, amelyet más tokenekbe küld van szüksége, mezőjét GraphAPI tooset hello app *acceptMappedClaims* túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="9ec93-196">If you need a custom claim that sends other tokens, use GraphAPI tooset hello app field *acceptMappedClaims* too**True**.</span></span> <span data-ttu-id="9ec93-197">Azure AD Graph Explorer vagy a MS Graph toomake használhatja ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9ec93-197">You can use either Azure AD Graph Explorer or MS Graph toomake this configuration.</span></span> 

<span data-ttu-id="9ec93-198">Ez a példa Graph Explorer:</span><span class="sxs-lookup"><span data-stu-id="9ec93-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="9ec93-199">Töltse le a PingAccess, és állítsa be alkalmazását</span><span class="sxs-lookup"><span data-stu-id="9ec93-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="9ec93-200">Most, hogy befejezte az összes hello Azure Active Directory telepítési lépéseinek tooconfiguring PingAccess továbbléphet.</span><span class="sxs-lookup"><span data-stu-id="9ec93-200">Now that you've completed all hello Azure Active Directory setup steps, you can move on tooconfiguring PingAccess.</span></span> 

<span data-ttu-id="9ec93-201">hello részletes, lépésenkénti leírását a forgatókönyv részét PingAccess hello folytatását hello Ping identitáskezelési dokumentáció, [PingAccess konfigurálása az Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="9ec93-201">hello detailed steps for hello PingAccess part of this scenario continue in hello Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="9ec93-202">Ezek a lépések végigvezetik hello folyamat első egy PingAccess fiókot, ha még nem rendelkezik egy, a hello PingAccess kiszolgáló telepítése és a hello hello Azure-portálon fájlból másolt könyvtár-azonosítója az Azure AD OIDC szolgáltató kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9ec93-202">Those steps walk you through hello process of getting a PingAccess account if you don't already have one, installing hello PingAccess Server, and creating an Azure AD OIDC Provider connection with hello Directory ID that you copied from hello Azure portal.</span></span> <span data-ttu-id="9ec93-203">Ezt követően hello alkalmazás Azonosítóját és kulcsát értékek toocreate a webes munkamenet használja PingAccess.</span><span class="sxs-lookup"><span data-stu-id="9ec93-203">Then, you use hello Application ID and Key values toocreate a Web Session on PingAccess.</span></span> <span data-ttu-id="9ec93-204">Ezt követően Identitásleképezés beállítása, és hozzon létre egy virtuális gazdagép, a hely és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9ec93-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="9ec93-205">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="9ec93-205">Test your app</span></span>

<span data-ttu-id="9ec93-206">Ha végrehajtotta ezeket a lépéseket, az alkalmazás működik, és kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9ec93-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="9ec93-207">tootest, nyisson meg egy böngészőt, és keresse meg a toohello külső URL-címet, amelyet közzé a hello alkalmazást az Azure-ban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="9ec93-207">tootest it, open a browser and navigate toohello external URL that you created when you published hello app in Azure.</span></span> <span data-ttu-id="9ec93-208">Bejelentkezés hello fiókot a hozzárendelt toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ec93-208">Sign in with hello test account that you assigned toohello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ec93-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ec93-209">Next steps</span></span>

- [<span data-ttu-id="9ec93-210">Az Azure AD PingAccess konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ec93-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="9ec93-211">Hogyan nyújt az Azure AD-alkalmazásproxy egyszeri bejelentkezéshez?</span><span class="sxs-lookup"><span data-stu-id="9ec93-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="9ec93-212">Alkalmazásproxy hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="9ec93-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
