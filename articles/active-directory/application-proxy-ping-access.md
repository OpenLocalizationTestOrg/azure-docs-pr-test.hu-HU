---
title: "Fejléc-alapú hitelesítés PingAccess az az Azure AD alkalmazásproxy |} Microsoft Docs"
description: "Alkalmazások közzététele az PingAccess és alkalmazás Proxy fejléc-alapú hitelesítés támogatására."
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
ms.openlocfilehash: 58034ab8830cf655199875b448948ea14dc04a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="7b806-103">Az egyszeri bejelentkezés az alkalmazásproxy és PingAccess fejléc-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7b806-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="7b806-104">Az Azure Active Directory Alkalmazásproxyjával és PingAccess közösen egyszerre több alkalmazást is hozzáférést biztosít az Azure Active Directory ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="7b806-104">Azure Active Directory Application Proxy and PingAccess have partnered together to provide Azure Active Directory customers with access to even more applications.</span></span> <span data-ttu-id="7b806-105">PingAccess bővíti a [meglévő alkalmazásproxy ajánlatok](active-directory-application-proxy-get-started.md) egyszeri bejelentkezéses hozzáférést azon alkalmazásoknak, amelyek a hitelesítéshez használandó fejlécek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7b806-105">PingAccess expands the [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) to include single sign-on access to applications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="7b806-106">Mi az az PingAccess az Azure AD?</span><span class="sxs-lookup"><span data-stu-id="7b806-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="7b806-107">Az Azure Active Directory PingAccess, amely lehetővé teszi a felhasználók hozzáférést és egyszeri bejelentkezést biztosítanak fejlécek hitelesítés használó alkalmazások PingAccess egy elérhető.</span><span class="sxs-lookup"><span data-stu-id="7b806-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you to give users access and single sign-on to applications that use headers for authentication.</span></span> <span data-ttu-id="7b806-108">Alkalmazás Proxy ezekhez az alkalmazásokhoz, mint bármely más, való hozzáférés hitelesítéséhez az Azure AD, és ezután átadja az összekötő szolgáltatás forgalmát kezeli.</span><span class="sxs-lookup"><span data-stu-id="7b806-108">Application Proxy treats these apps like any other, using Azure AD to authenticate access and then passing traffic through the connector service.</span></span> <span data-ttu-id="7b806-109">PingAccess az alkalmazások előtt helyezkedik el, és az Azure ad-jogkivonat fordítja fejléc, hogy az alkalmazás a hitelesítési kapjon, azt is olvasható formátumban.</span><span class="sxs-lookup"><span data-stu-id="7b806-109">PingAccess sits in front of the apps and translates the access token from Azure AD into a header so that the application receives the authentication in the format it can read.</span></span>

<span data-ttu-id="7b806-110">A felhasználók nem bármilyen figyelje, amikor bejelentkeznek a használhatja a vállalati alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="7b806-110">Your users won’t notice anything different when they sign in to use your corporate apps.</span></span> <span data-ttu-id="7b806-111">Akkor is még mindig bárhonnan dolgozhatnak bármilyen eszközön.</span><span class="sxs-lookup"><span data-stu-id="7b806-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="7b806-112">Mivel az alkalmazásproxy-összekötők összes alkalmazásoknak a hitelesítési típus távoli forgalom közvetlen, azokat továbbra automatikusan, illetve a terhelést.</span><span class="sxs-lookup"><span data-stu-id="7b806-112">Since the Application Proxy connectors direct remote traffic to all apps regardless of their authentication type, they’ll continue to load balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="7b806-113">Hogyan szerezhetek hozzáférést?</span><span class="sxs-lookup"><span data-stu-id="7b806-113">How do I get access?</span></span>

<span data-ttu-id="7b806-114">Mivel ebben a forgatókönyvben az Azure Active Directory és PingAccess közötti partnerség kínált, licencek kell mindkét szolgáltatás esetében.</span><span class="sxs-lookup"><span data-stu-id="7b806-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="7b806-115">Azonban az Azure Active Directory Premium előfizetéshez tartozik, amely lefedi legfeljebb 20 alkalmazások alapszintű PingAccess licencet.</span><span class="sxs-lookup"><span data-stu-id="7b806-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up to 20 applications.</span></span> <span data-ttu-id="7b806-116">Ha több mint 20 fejléc-alapú alkalmazások közzétételét van szüksége, a PingAccess vásárolhat további licencre.</span><span class="sxs-lookup"><span data-stu-id="7b806-116">If you need to publish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="7b806-117">További információk: [Azure Active Directory editions](active-directory-editions.md) (Azure Active Directory-kiadások).</span><span class="sxs-lookup"><span data-stu-id="7b806-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="7b806-118">Az alkalmazás közzététele az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="7b806-118">Publish your application in Azure</span></span>

<span data-ttu-id="7b806-119">Ez a cikk tesznek közzé egy alkalmazást ebben a forgatókönyvben az első alkalommal személyek számára készült.</span><span class="sxs-lookup"><span data-stu-id="7b806-119">This article is intended for people who are publishing an app with this scenario for the first time.</span></span> <span data-ttu-id="7b806-120">Első lépések alkalmazás és a PingAccess, mellett a közzétételi lépéseket végigvezeti.</span><span class="sxs-lookup"><span data-stu-id="7b806-120">It walks through how to get started with both Application and PingAccess, in addition to the publishing steps.</span></span> <span data-ttu-id="7b806-121">Ha már beállított mindkét szolgáltatás, de a közzétételi lépéseket frissítő, az összekötő telepítése, és helyezze át a [adja hozzá az alkalmazás az Azure AD-alkalmazásproxyval](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="7b806-121">If you’ve already configured both services but want a refresher on the publishing steps, you can skip the connector installation and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="7b806-122">Mivel ez a forgatókönyv az Azure AD közötti partnerség és PingAccess, bizonyos utasítások találhatók a Ping identitás helyen.</span><span class="sxs-lookup"><span data-stu-id="7b806-122">Since this scenario is a partnership between Azure AD and PingAccess, some of the instructions exist on the Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="7b806-123">Az alkalmazásproxy-összekötő telepítése</span><span class="sxs-lookup"><span data-stu-id="7b806-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="7b806-124">Ha már Application Proxy engedélyezve van, és egy összekötő telepítve van, hagyja ki ezt a szakaszt, és helyezze át a [adja hozzá az alkalmazás az Azure AD-alkalmazásproxyval](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="7b806-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="7b806-125">Az alkalmazásproxy-összekötő egy Windows Server-szolgáltatás, amely arra utasítja a közzétett alkalmazások a távoli alkalmazottak érkező forgalom.</span><span class="sxs-lookup"><span data-stu-id="7b806-125">The Application Proxy connector is a Windows Server service that directs the traffic from your remote employees to your published apps.</span></span> <span data-ttu-id="7b806-126">Telepítési utasításokat, lásd: [alkalmazásproxy engedélyezése az Azure-portálon](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="7b806-126">For more detailed installation instructions, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="7b806-127">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7b806-127">Sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="7b806-128">Válassza ki **Azure Active Directory** > **alkalmazásproxy**.</span><span class="sxs-lookup"><span data-stu-id="7b806-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="7b806-129">Válassza ki **összekötő letöltése** a alkalmazásproxy-összekötő letöltés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="7b806-129">Select **Download Connector** to start the Application Proxy connector download.</span></span> <span data-ttu-id="7b806-130">Kövesse a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7b806-130">Follow the installation instructions.</span></span>

   ![Alkalmazásproxy engedélyezése és az összekötő letöltése](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="7b806-132">Az összekötő letöltése kell automatikusan alkalmazásproxy engedélyezése a címtáron, de ha nem választhat **alkalmazásproxy engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="7b806-132">Downloading the connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a><span data-ttu-id="7b806-133">Az alkalmazás hozzáadása az Azure AD-alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="7b806-133">Add your app to Azure AD with Application Proxy</span></span>

<span data-ttu-id="7b806-134">Nincsenek két műveletet kell tennie az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7b806-134">There are two actions you need to take in the Azure portal.</span></span> <span data-ttu-id="7b806-135">Először az alkalmazásproxy az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="7b806-135">First, you need to publish your application with Application Proxy.</span></span> <span data-ttu-id="7b806-136">Ezt követően kell összegyűjtenie némi információt az alkalmazást, amelynek során a PingAccess lépéseket használhatja.</span><span class="sxs-lookup"><span data-stu-id="7b806-136">Then, you need to collect some information about the app that you can use during the PingAccess steps.</span></span>

<span data-ttu-id="7b806-137">Kövesse az alábbi lépéseket az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="7b806-137">Follow these steps to publish your app.</span></span> <span data-ttu-id="7b806-138">A részletes lépéseket bemutató 1-8, tekintse meg a még [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7b806-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="7b806-139">Ha nem az utolsó szakaszban, jelentkezzen be a [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7b806-139">If you didn't in the last section, sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="7b806-140">Válassza ki **Azure Active Directory** > **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7b806-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="7b806-141">Válassza ki **Hozzáadás** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="7b806-141">Select **Add** at the top of the blade.</span></span>
4. <span data-ttu-id="7b806-142">Válassza ki **helyszíni alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7b806-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="7b806-143">Töltse ki a kötelező mezőket az új alkalmazás adatait tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="7b806-143">Fill out the required fields with information about your new app.</span></span> <span data-ttu-id="7b806-144">A következő útmutatást használhatja a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="7b806-144">Use the following guidance for the settings:</span></span>
   - <span data-ttu-id="7b806-145">**Belső URL-cím**: általában viszi az alkalmazás bejelentkezési oldalára, ha a vállalati hálózaton már megnyitott URL-címet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="7b806-145">**Internal URL**: Normally you provide the URL that takes you to the app’s sign in page when you’re on the corporate network.</span></span> <span data-ttu-id="7b806-146">Ebben a forgatókönyvben az összekötő kell a PingAccess proxy tekinti az alkalmazás első oldalán.</span><span class="sxs-lookup"><span data-stu-id="7b806-146">For this scenario the connector needs to treat the PingAccess proxy as the front page of the app.</span></span> <span data-ttu-id="7b806-147">Ezt a formátumot használja: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="7b806-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="7b806-148">A port 3000 alapértelmezés szerint, de a PingAccess konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="7b806-148">The port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="7b806-149">**Előhitelesítési módszer**: Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="7b806-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="7b806-150">**URL-címet a fejlécekben fordítása**: nincs</span><span class="sxs-lookup"><span data-stu-id="7b806-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="7b806-151">Ha ez az első alkalmazás, port 3000 elindításához használja, majd visszatérhet, és frissítse ezt a beállítást, ha az PingAccess konfigurációjának módosítását.</span><span class="sxs-lookup"><span data-stu-id="7b806-151">If this is your first application, use port 3000 to start and come back to update this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="7b806-152">Ha ez a második vagy újabb alkalmazáshoz, ez kell felel meg a figyelő a PingAccess konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="7b806-152">If this is your second or later app, this will need to match the Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="7b806-153">További információ [PingAccess a figyelők](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="7b806-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="7b806-154">Válassza ki **Hozzáadás** a panel alján.</span><span class="sxs-lookup"><span data-stu-id="7b806-154">Select **Add** at the bottom of the blade.</span></span> <span data-ttu-id="7b806-155">Az alkalmazás kerül, és a quick start menü megnyitása.</span><span class="sxs-lookup"><span data-stu-id="7b806-155">Your application is added, and the quick start menu opens.</span></span>
7. <span data-ttu-id="7b806-156">A quick start menüben válassza ki a **rendelje hozzá a felhasználót tesztelési**, és legalább egy felhasználói hozzáadni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7b806-156">In the quick start menu, select **Assign a user for testing**, and add at least one user to the application.</span></span> <span data-ttu-id="7b806-157">Győződjön meg arról, hogy a teszt fiókjának van hozzáférési joga a helyszíni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="7b806-157">Make sure this test account has access to the on-premises application.</span></span>
8. <span data-ttu-id="7b806-158">Válassza ki **hozzárendelése** menteni a teszt felhasználó hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="7b806-158">Select **Assign** to save the test user assignment.</span></span>
9. <span data-ttu-id="7b806-159">Válassza ki az alkalmazás-felügyelet panel **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7b806-159">On the app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="7b806-160">Válasszon **fejléc-alapú bejelentkezés** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="7b806-160">Choose **Header-based sign-on** from the drop-down menu.</span></span> <span data-ttu-id="7b806-161">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b806-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="7b806-162">Ha ez az első alkalommal használja a fejléc-alapú egyszeri bejelentkezést, telepítendő PingAccess.</span><span class="sxs-lookup"><span data-stu-id="7b806-162">If this is your first time using header-based single sign-on, you need to install PingAccess.</span></span> <span data-ttu-id="7b806-163">Legyen az Azure-előfizetés automatikusan hozzárendeli a PingAccess telepített, ezen az oldalon egyszeri bejelentkezést a hivatkozás segítségével töltse le a PingAccess.</span><span class="sxs-lookup"><span data-stu-id="7b806-163">To make sure your Azure subscription is automatically associated with your PingAccess installation, use the link on this single sign-on page to download PingAccess.</span></span> <span data-ttu-id="7b806-164">Nyissa meg a letöltési hely most, vagy visszatérhet, és ezen az oldalon később.</span><span class="sxs-lookup"><span data-stu-id="7b806-164">You can open the download site now, or come back to this page later.</span></span> 

   ![Válassza ki a fejléc-alapú bejelentkezés](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="7b806-166">Zárja be a vállalati alkalmazások panel vagy görgessen egészen a bal oldali térjen vissza az Azure Active Directory menübe.</span><span class="sxs-lookup"><span data-stu-id="7b806-166">Close the Enterprise applications blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
12. <span data-ttu-id="7b806-167">Válassza ki **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="7b806-167">Select **App registrations**.</span></span>

   ![Válassza ki az alkalmazás-regisztráció](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="7b806-169">Válassza ki az alkalmazást, az előzőekben adott hozzá, majd **válasz URL-címek**.</span><span class="sxs-lookup"><span data-stu-id="7b806-169">Select the app you just added, then **Reply URLs**.</span></span>

   ![Válassza ki a válasz URL-címek](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="7b806-171">Ellenőrizze, hogy a külső URL-címet, az alkalmazás 5. lépésben hozzárendelt van-e a válasz URL-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="7b806-171">Check to see if the external URL that you assigned to your app in step 5 is in the Reply URLs list.</span></span> <span data-ttu-id="7b806-172">Ha nem, vegye fel azt.</span><span class="sxs-lookup"><span data-stu-id="7b806-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="7b806-173">Válassza a beállítások panelről, **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="7b806-173">On the app settings blade, select **Required permissions**.</span></span>

  ![Válassza ki a szükséges engedélyek](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="7b806-175">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="7b806-175">Select **Add**.</span></span> <span data-ttu-id="7b806-176">Az API kiválasztása **Windows Azure Active Directory**, majd **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="7b806-176">For the API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="7b806-177">Az engedélyek, válassza ki **olvasási és írása az összes alkalmazás** és **jelentkezzen be és felhasználói profil olvasása**, majd **válassza** és **kész**.</span><span class="sxs-lookup"><span data-stu-id="7b806-177">For the permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Engedélyek kiválasztása](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-the-pingaccess-steps"></a><span data-ttu-id="7b806-179">Információgyűjtés a PingAccess lépései</span><span class="sxs-lookup"><span data-stu-id="7b806-179">Collect information for the PingAccess steps</span></span>

1. <span data-ttu-id="7b806-180">Válassza a beállítások panelen **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="7b806-180">On your app settings blade, select **Properties**.</span></span> 

  ![Válassza a Tulajdonságok](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="7b806-182">Mentse a **alkalmazásazonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="7b806-182">Save the **Application Id** value.</span></span> <span data-ttu-id="7b806-183">Az ügyfél-azonosító szolgál PingAccess konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="7b806-183">This is used for the client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="7b806-184">Válassza a beállítások panelről, **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="7b806-184">On the app settings blade, select **Keys**.</span></span>

  ![Válassza ki a kulcsok](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="7b806-186">Hozzon létre egy kulcsot a fő leírás, majd válassza a lejárati dátumot a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="7b806-186">Create a key by entering a key description and choosing an expiration date from the drop-down menu.</span></span>
5. <span data-ttu-id="7b806-187">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b806-187">Select **Save**.</span></span> <span data-ttu-id="7b806-188">Megjelenik egy GUID Azonosítót a **érték** mező.</span><span class="sxs-lookup"><span data-stu-id="7b806-188">A GUID appears in the **Value** field.</span></span>

  <span data-ttu-id="7b806-189">Most, mentse ezt az értéket, mert nem lehet majd megjeleníteni az ablak bezárását követően.</span><span class="sxs-lookup"><span data-stu-id="7b806-189">Save this value now, as you won’t be able to see it again after you close this window.</span></span>

  ![Hozzon létre egy új kulcsot](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="7b806-191">Zárja be a regisztrációk panelen vagy görgessen egészen a bal oldali térjen vissza az Azure Active Directory menübe.</span><span class="sxs-lookup"><span data-stu-id="7b806-191">Close the App registrations blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
7. <span data-ttu-id="7b806-192">Válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="7b806-192">Select **Properties**.</span></span>
8. <span data-ttu-id="7b806-193">Mentse a **könyvtár-azonosítója** GUID.</span><span class="sxs-lookup"><span data-stu-id="7b806-193">Save the **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-to-send-custom-fields"></a><span data-ttu-id="7b806-194">Nem kötelező – a frissítés GraphAPI küldése egyéni mezők</span><span class="sxs-lookup"><span data-stu-id="7b806-194">Optional - Update GraphAPI to send custom fields</span></span>

<span data-ttu-id="7b806-195">A biztonsági jogkivonatok, amelyek az Azure AD küld a hitelesítéshez listájáért lásd: [az Azure AD-jogkivonatok referenciájából](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="7b806-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="7b806-196">Ha egy egyéni jogcím, amelyet más tokenekbe küld van szüksége, GraphAPI beállításához használja az alkalmazás mező *acceptMappedClaims* való **igaz**.</span><span class="sxs-lookup"><span data-stu-id="7b806-196">If you need a custom claim that sends other tokens, use GraphAPI to set the app field *acceptMappedClaims* to **True**.</span></span> <span data-ttu-id="7b806-197">Azure AD Graph Explorer vagy a MS Graph használatával ellenőrizze ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7b806-197">You can use either Azure AD Graph Explorer or MS Graph to make this configuration.</span></span> 

<span data-ttu-id="7b806-198">Ez a példa Graph Explorer:</span><span class="sxs-lookup"><span data-stu-id="7b806-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="7b806-199">Töltse le a PingAccess, és állítsa be alkalmazását</span><span class="sxs-lookup"><span data-stu-id="7b806-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="7b806-200">Most, hogy befejezte az Azure Active Directory beállítási lépéseket, továbbléphet PingAccess konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="7b806-200">Now that you've completed all the Azure Active Directory setup steps, you can move on to configuring PingAccess.</span></span> 

<span data-ttu-id="7b806-201">A részletes, lépésenkénti leírását ebben a forgatókönyvben PingAccess része továbbra is a Ping identitáskezelési dokumentáció [PingAccess konfigurálása az Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="7b806-201">The detailed steps for the PingAccess part of this scenario continue in the Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="7b806-202">Ezen lépések végigvezetik a folyamat első egy PingAccess fiókot, ha még nem rendelkezik egy, a PingAccess kiszolgáló telepítése és az Azure AD OIDC szolgáltatói kapcsolat létrehozása az Azure portálról másolt könyvtár-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="7b806-202">Those steps walk you through the process of getting a PingAccess account if you don't already have one, installing the PingAccess Server, and creating an Azure AD OIDC Provider connection with the Directory ID that you copied from the Azure portal.</span></span> <span data-ttu-id="7b806-203">Az alkalmazás Azonosítóját és kulcsát értékek, majd a webes munkamenet a PingAccess létrehozásához használhat.</span><span class="sxs-lookup"><span data-stu-id="7b806-203">Then, you use the Application ID and Key values to create a Web Session on PingAccess.</span></span> <span data-ttu-id="7b806-204">Ezt követően Identitásleképezés beállítása, és hozzon létre egy virtuális gazdagép, a hely és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7b806-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="7b806-205">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="7b806-205">Test your app</span></span>

<span data-ttu-id="7b806-206">Ha végrehajtotta ezeket a lépéseket, az alkalmazás működik, és kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7b806-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="7b806-207">Ha kipróbálja, nyisson meg egy böngészőt, és keresse meg a külső URL-címet, amelyet közzé az alkalmazást az Azure-ban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="7b806-207">To test it, open a browser and navigate to the external URL that you created when you published the app in Azure.</span></span> <span data-ttu-id="7b806-208">Jelentkezzen be a fiókot, amelyet az alkalmazáshoz rendelt.</span><span class="sxs-lookup"><span data-stu-id="7b806-208">Sign in with the test account that you assigned to the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b806-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b806-209">Next steps</span></span>

- [<span data-ttu-id="7b806-210">Az Azure AD PingAccess konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7b806-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="7b806-211">Hogyan nyújt az Azure AD-alkalmazásproxy egyszeri bejelentkezéshez?</span><span class="sxs-lookup"><span data-stu-id="7b806-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="7b806-212">Alkalmazásproxy hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7b806-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
