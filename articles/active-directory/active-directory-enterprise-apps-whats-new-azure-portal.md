---
title: "What's new in Azure Active Directory vállalati Alkalmazáskezelés |} Microsoft Docs"
description: "Ismerje meg a vállalati alkalmazások kezelése az Azure Active Directory újdonságai."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 0c32a6719292aa903aa32dfdc4a31114e7a28346
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a><span data-ttu-id="6f9fe-103">Vállalati alkalmazások kezelése az Azure Active Directory újdonságai</span><span class="sxs-lookup"><span data-stu-id="6f9fe-103">What's new in Enterprise Application management in Azure Active Directory</span></span> 

<span data-ttu-id="6f9fe-104">Azure Active Directory (Azure AD) továbbfejlesztett vállalati alkalmazás felügyeleti eszközöket, az új szolgáltatásokat és képességeket egyszerűbb és hatékony kezelése alkalmazások tételére.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-104">Azure Active Directory (Azure AD) has enhanced enterprise application management tools, with new features and capabilities to make managing apps simpler and efficient.</span></span>

<span data-ttu-id="6f9fe-105">Az alábbiakban néhány olyan a fejlesztésen esett át az Azure AD-t a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6f9fe-105">Following are some of the enhancements for Azure AD in the [Azure portal](https://portal.azure.com).</span></span>

- <span data-ttu-id="6f9fe-106">Egy továbbfejlesztett alkalmazáskatalógusában egy egyszerűsített létrehozása alkalmazásmodell élményt, és a használt összes alkalmazástípusokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-106">An improved application gallery experience, with a simplified application creation model and support for all the application types that you’re used to.</span></span> 
- <span data-ttu-id="6f9fe-107">Egy vadonatúj gyors üzembe helyezési élményt nyújt segítséget az alkalmazás a próbaüzem az induláshoz.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-107">A brand-new quick start experience that can help you get going with a pilot of your application.</span></span> 
- <span data-ttu-id="6f9fe-108">Néhány kattintással önkiszolgáló-szabályzatok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-108">Configure self-service policies with just a few clicks.</span></span> 
- <span data-ttu-id="6f9fe-109">Az alkalmazásproxy, az egyszeri bejelentkezés konfigurálása és a saját felhasználói élményre, teszi több kész, mint előtt kapcsolja.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-109">Improvements to application proxy, single sign-on configuration, and bring your own application experiences, allowing you to get more done than before.</span></span>

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a><span data-ttu-id="6f9fe-110">Az Azure Active Directory Alkalmazáskatalógusában fejlesztései</span><span class="sxs-lookup"><span data-stu-id="6f9fe-110">Improvements to the Azure Active Directory Application Gallery</span></span>

<span data-ttu-id="6f9fe-111">Adja hozzá a kedvenc alkalmazások, hogy származó a [alkalmazáskatalógusában](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), akkor most kiterjesztése a felhőbe egyéni alkalmazások vagy kidolgozása új alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-111">Add your favorite applications whether they are from the [application gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), custom applications you’re extending to the cloud, or new applications you’re developing.</span></span>  <span data-ttu-id="6f9fe-112">Ismerkedés az új felhasználói élmény a kattintva **Hozzáadás** a a **vállalati alkalmazások** áttekintése vagy **összes alkalmazás** paneleken.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-112">You can get started with this new experience by clicking **Add** on the **Enterprise Applications** overview or **All applications** blades.</span></span>
 
  ![Egy alkalmazás hozzáadása](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

<span data-ttu-id="6f9fe-114">Egyszer az oldalon láthatja a kiemelt alkalmazások a felhasználók átadása támogató első középső jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-114">Once in the gallery, you’ll see all our featured applications which support user provisioning displayed front-and-center.</span></span>  <span data-ttu-id="6f9fe-115">Megkeresheti az Önt érdeklő alkalmazások elemezze különböző kategóriákban kívüli bármilyen típusú, vagy használhatja a keresési élményt biztosít az alkalmazásokat, amelyet integrálni szeretne gyorsan kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-115">You can browse all sorts of different categories to drill into the applications you care about, or you can use the search experience to rapidly find the applications you want to integrate.</span></span>

  ![Az alkalmazás gyűjteménye](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a><span data-ttu-id="6f9fe-117">Adja hozzá az egyéni alkalmazások egyetlen helyről</span><span class="sxs-lookup"><span data-stu-id="6f9fe-117">Add custom applications from one place</span></span>

<span data-ttu-id="6f9fe-118">Mellett előre integrált alkalmazások hozzáadása a gyűjteményből, az egyéni alkalmazás konfigurációjának észlel volt amellyel a klasszikus felügyeleti portálon mindegyike most már az új portálon lehetséges.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-118">In addition to adding pre-integrated applications from the gallery, all the custom application configuration experiences that you were used to in the classic management portal are now possible in the new portal.</span></span> <span data-ttu-id="6f9fe-119">E kiterjesztése a helyszíni saját jelszó vagy összevont egyszeri Bejelentkezéses alkalmazások integrálása az alkalmazásproxy használatával olyan alkalmazást, vagy egy, az alkalmazás beállításjegyzékkel vadonatúj alkalmazás létrehozása, is elérheti azt minden a Ez egy helyen.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-119">Whether you are extending an application from on-premises using the application proxy, integrating your own password or federated SSO application, or creating a brand-new application using the application registry, you can get to it all from this one single place.</span></span>

  ![Saját alkalmazás hozzáadása](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
<span data-ttu-id="6f9fe-121">**Első lépések hozzáadása a saját alkalmazás**:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-121">**To get started adding your own application**:</span></span>

1. <span data-ttu-id="6f9fe-122">Kattintson a **saját hivatkozás hozzáadása** a alkalmazáskatalógusában tetején.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-122">Click the **add your own link** at the top of the application gallery.</span></span> 
2. <span data-ttu-id="6f9fe-123">Látni fogja, elé, két lehetőség közül választhat: **telepítsen egy meglévő alkalmazást** vagy **új alkalmazások fejlesztése**.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-123">You’ll see two options in front of you: **deploy an existing application** or **develop a new application**.</span></span> <span data-ttu-id="6f9fe-124">Olvassa el a további tudnivalók a különbség a két beállítás és a használatukat.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-124">Read on to learn the difference between the two options and how to use them.</span></span>

### <a name="deploying-existing-applications"></a><span data-ttu-id="6f9fe-125">Meglévő alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6f9fe-125">Deploying existing applications</span></span>

1. <span data-ttu-id="6f9fe-126">Ha van egy alkalmazás már fut, és csak szeretné integrálni Azure ad-val egyszeri bejelentkezést a vagy átadása, válassza ki a **telepítsen egy meglévő alkalmazást** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-126">If you’ve got an application running already and just want to integrate it into Azure AD for single-sign on or provisioning, choose the **Deploy an existing application** option.</span></span> <span data-ttu-id="6f9fe-127">Adjon meg egy nevet az alkalmazáshoz, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-127">Pick a name for your application, click **Add**.</span></span>
2. <span data-ttu-id="6f9fe-128">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="6f9fe-128">That's it!</span></span> <span data-ttu-id="6f9fe-129">Helyett ismerete szükséges előre az alkalmazás adatait, most állíthatja be az új alkalmazás működése a bal oldali menü nézetre lépne, és a kérelem a tetszőlegesen a konfigurálásával bármikor.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-129">Instead of needing to know all the details about your application up front, you can now set up how your new application works by navigating through the left menu and configuring the application to your liking at any time.</span></span>

  ![Egy kattintással egy meglévő alkalmazás hozzáadása](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a><span data-ttu-id="6f9fe-131">Új alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="6f9fe-131">Developing new applications</span></span>

1. <span data-ttu-id="6f9fe-132">Ha az új alkalmazást, nincs egyszerűen tehet szert az alkalmazás beállításjegyzék jobb a gyűjteményből:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-132">If you’re developing a new application, there's an easy way for you to get to the Application Registry right from the gallery:</span></span>
2. <span data-ttu-id="6f9fe-133">Kattintson a a **hozzáadása a saját** lehetőséget az alkalmazás gyűjteményből, válassza ki a **kifejleszthet egy meglévő alkalmazást** választás, és megjelenik egy Gyorshivatkozás az alkalmazás hozzáadása élmény való jog.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-133">Click on the **add your own** option from the Application Gallery, select the **develop an existing application** choice, and you’ll see a quick link right to the application add experience.</span></span>

  ![Néhány kattintással egy újonnan által fejlesztett alkalmazás hozzáadása](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
><span data-ttu-id="6f9fe-135">Miután hozzáadta az alkalmazás beállításjegyzék használó alkalmazások, láthatja, ahol képes lesz az egyszeri bejelentkezés konfigurálása és kezelése az új alkalmazás-hozzáférési házirendjeit a vállalati alkalmazások listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-135">Once you add an application using the Application Registry, you’ll see it show up in the list of Enterprise Applications where you’ll be able to configure single sign-on and manage access policies for your new application.</span></span>

  ![Az új alkalmazást a vállalati alkalmazásokhoz való hozzáférés kezelése](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a><span data-ttu-id="6f9fe-137">Gyors üzembe helyezési: azonnal az új alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="6f9fe-137">Quick start: Get going with your new application right away</span></span> 

<span data-ttu-id="6f9fe-138">Miután hozzáadott egy alkalmazást, hogy előre integrált kell vagy az alkalmazásba, az első gyorsan földelve az új alkalmazások élményt nyújt a következőkhöz ideális gyors üzembe helyezési élmény nagyságú volt.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-138">After you’ve added an application, whether it be pre-integrated or your own app, we’ve created a tailored quick-start experience to get you grounded in the new applications experience quickly.</span></span> <span data-ttu-id="6f9fe-139">Ha rendszeresen kövesse a beállításokat, azt fogja a felhasználói felületen keresztül, illetve megmutatja, hogyan induláshoz a próbaüzem az új alkalmazás a lehető leggyorsabban tegye.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-139">If you follow each option systematically, we’ll walk you through the UI and show you how to get going with a pilot of your new application as quickly as possible.</span></span> 
 
  ![Az új alkalmazások gyors start élmény](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 <span data-ttu-id="6f9fe-141">Az új – gyors üzembe helyezési felhasználói élmény tetszőleges időpontban, és bármely alkalmazás, letöltheti kattintva **gyors üzembe helyezési** alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-141">You can visit this new quick start experience at any time, and for any application, by clicking on **Quick start** from the application left navigation menu.</span></span>


## <a name="updated-application-proxy-configuration"></a><span data-ttu-id="6f9fe-142">Frissített alkalmazás proxy konfigurációja</span><span class="sxs-lookup"><span data-stu-id="6f9fe-142">Updated application proxy configuration</span></span>
<span data-ttu-id="6f9fe-143">Most most szóbeli a hozzáadott új alkalmazások közül a helyszíni környezetben fut, és szeretné integrálni az Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-143">Now, let’s say one of the new applications you added is running in your on-premises environment and you want to integrate it with Azure AD.</span></span>  <span data-ttu-id="6f9fe-144">Az új alkalmazás konfigurációs tapasztalatairól a ritkán használt adatok új dolog az új Azure ad portálon, hogy az alkalmazás bejelentkezés módja az alkalmazás proxykonfigurációt a felosztás is most könnyen teszi Egyszeri jelszót, vagy összevont alkalmazásokat, hogy a vállalati hálózat közvetlenül a felhőbe, anélkül, hogy az alkalmazás több példánya létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-144">One of the cool new things about the new application configuration experience in the new Azure AD portal is that by splitting the application’s sign-on mode from its application proxy configuration, you can now easily expose password SSO or federated applications that are running in your corporate network right to the cloud, without having to create multiple instances of the application.</span></span>

<span data-ttu-id="6f9fe-145">Továbbá a is konfigurálhat az új alkalmazások felvett új portálról, beleértve az alkalmazások, amelyek támogatják a natív Windows-hitelesítés lép az Azure AD-alkalmazásproxy jobb való használatra.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-145">In addition to this, you can now also configure any of the new applications you’ve added for use with the Azure AD Application Proxy right from the new portal, including applications which support native Windows authentication experiences.</span></span>

  ![Az alkalmazás bejelentkezésre lehetőség integrált Windows-hitelesítés konfigurálása](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

<span data-ttu-id="6f9fe-147">A natív Windows-hitelesítés alkalmazás konfigurálása az alkalmazásproxy első lépések:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-147">To get started configuring a native Windows authentication application with the Application Proxy:</span></span>
1. <span data-ttu-id="6f9fe-148">Kattintson az egyszeri bejelentkezés navigációs elemre, és válassza a **integrált Windows-hitelesítés** a bejelentkezési beállítások panelen, és konfigurálja a tetszőlegesen a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-148">Click on the Single sign-on navigation item and choose **Integrated Windows Authentication** from the sign-on settings blade and configure the settings to your liking.</span></span>
2. <span data-ttu-id="6f9fe-149">Fölött támogató ezek új hitelesítési módok, most már is feltöltheti a tanúsítványokat a egyéni tartományok biztonságos végpontok a szervezeten belül futó alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-149">On top of supporting these new authentication modes, you can now also upload certificates from custom domains to support applications running on secure endpoints within your organization.</span></span>  
 
   ![Az alkalmazásproxy használni kívánt tanúsítvány feltöltése](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. <span data-ttu-id="6f9fe-151">Töltse fel a kedvenc a helyszíni alkalmazások új tanúsítványt, kattintson a a **alkalmazásproxy** a bal oldali navigációs menü, majd a **tanúsítvány** választó, és azt használatával titkosítsa tanúsítványfájl kéri le a felhőbeli végpont az alkalmazás feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-151">To upload a new certificate for your favorite on-premises application, click on the **Application proxy** option from the left navigation menu, click the **Certificate** selector, and upload a certificate file we can use to encrypt requests from our cloud endpoint to your application.</span></span>

## <a name="advanced-federated-single-sign-on-configuration"></a><span data-ttu-id="6f9fe-152">Speciális összevont egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f9fe-152">Advanced federated single sign-on configuration</span></span>

<span data-ttu-id="6f9fe-153">Azok a még ma a összevont alkalmazásokat használ nincsenek számos új funkciója a SAML-alapú bejelentkezés beállítási paneljén.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-153">For those of you using federated applications today, there are many new features in the SAML-based sign-on configuration blade.</span></span> <span data-ttu-id="6f9fe-154">Kezdő-és most meg teljesen testreszabása, hozzáadása, távolítsa el, és a meglévő felhasználói attribútumok a SAML-jogkivonatokat jogcímekként kiadott megfeleltetése.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-154">To start with, now you can fully customize, add, remove, and map the existing user attributes issued as claims in SAML tokens.</span></span>
 
  ![Egy összevont alkalmazás átadott a SAML-jogkivonat felhasználói attribútumok testreszabása](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


<span data-ttu-id="6f9fe-156">Ellenőrizze, hogy meg az új összevont egyszeri bejelentkezés konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-156">To check that out the new federated SSO configuration:</span></span>
1. <span data-ttu-id="6f9fe-157">Nyisson meg egy összevont alkalmazás **egyszeri bejelentkezés** panel a bal oldali navigációs menü, és győződjön meg arról, hogy a "*SAML-alapú bejelentkezés** mód van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-157">Open a federated application’s **single sign-on** blade from the left navigation menu and make sure the ‘*SAML-based Sign-on** mode is selected.</span></span> 
2. <span data-ttu-id="6f9fe-158">Egyszer, engedélyezze a jelölőnégyzet alatti a **felhasználói attribútumok** módosításához az összes attribútum szerepel a SAML-jogkivonat címsor átadott alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-158">Once there, enable the checkbox under the **User Attributes** heading to modify all of the attributes included in the SAML token passed to that application.</span></span>

<span data-ttu-id="6f9fe-159">Akkor is is létrehozása, helyettesítő, és összevont egyszeri bejelentkezéshez használt tanúsítványok kezelése, valamint szerkesztheti, akik lekérdezi értesíti, ha a tanúsítvány érvényessége hamarosan lejár.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-159">You can also create, rollover, and manage certificates used for federated single sign-on, as well as edit who gets notified when your certificate is about to expire.</span></span> <span data-ttu-id="6f9fe-160">Látni fogja az alábbi új beállításait a **tanúsítványok** az azonos egyszeri bejelentkezés panel fejléc.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-160">You’ll see these new options under the **Certificates** heading on the same Single sign-on blade.</span></span>
 
  ![Egy új tanúsítványt, a lejárat értesítő e-mailt és a tanúsítvány-aláírási beállítások testreszabása létrehozása](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a><span data-ttu-id="6f9fe-162">Továbbítási állapotot paramenter</span><span class="sxs-lookup"><span data-stu-id="6f9fe-162">Relay State paramenter</span></span>
<span data-ttu-id="6f9fe-163">Végezetül korábban is kiterjesztettük SAML-alapú URL-cím paraméterei közé tartozik a támogatott a **továbbítási-State paraméter**, vagyis a lapon, a felhasználók megnyílik az összevont alkalmazás belül a bejelentkezés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-163">Finally, we’ve also extended the set of SAML URL parameters we support to include the **Relay State parameter**, which is the page your users will land on inside of a federated application once the sign-in is completed.</span></span> <span data-ttu-id="6f9fe-164">Ez az nagyon hasznos beállítással konfigurálható, ha azt szeretné, hogy a felhasználónak egy adott helyre, hogy melyek gyorsan az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-164">This is very useful setting to configure if you want to send your users to a specific place within the application to get them going quickly.</span></span>

  ![A SAML továbbítási-State paraméter beállítása](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
<span data-ttu-id="6f9fe-166">**A relay-state paraméter beállítása**:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-166">**To set the relay state parameter**:</span></span>

1. <span data-ttu-id="6f9fe-167">Engedélyezze a **megjelenítése speciális URL-beállításainak** jelölőnégyzetet, a a **tartomány és az URL-címek** konfigurációs panel egyszeri bejelentkezést a fejléc.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-167">Enable the **Show advanced URL settings** check-box under the **Domain and URLs** heading on the single-sign on configuration blade.</span></span> 
2. <span data-ttu-id="6f9fe-168">Ezután a, láthatja, új URL-cím csoportja bemeneti mezőkbe jelenik meg, amely lehetővé teszi a erről és más, SAML-alapú URL-beállításainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-168">Once you do this, you’ll see a set of new URL input boxes appear which will allow you to set this, and other, SAML URL settings.</span></span>

## <a name="bring-your-own-password-sso-applications"></a><span data-ttu-id="6f9fe-169">Kapcsolja a saját jelszavát SSO alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6f9fe-169">Bring your own password SSO applications</span></span>

<span data-ttu-id="6f9fe-170">Tudjuk, hogy nem minden alkalmazás támogatja-e az összevonási jobb kívül a mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-170">We know that not every application supports federation right out of the box.</span></span> <span data-ttu-id="6f9fe-171">Például lehet, hogy a hozzáadott új alkalmazások egyike egy egyéni bejelentkezési képernyő, felhasználók által használt felhasználónévvel és jelszóval bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-171">For example, maybe one of the new applications you added has a custom login screen that your users use a username and password to sign in to.</span></span> <span data-ttu-id="6f9fe-172">Ilyen típusú alkalmazások az Azure AD használatával továbbra is integrálhatja a **kapcsolja a saját alkalmazásai** szolgáltatás, amely érhető el az új portálon konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-172">You can still integrate these types of applications with Azure AD using our **Bring your own applications** feature, which is now available for you to configure in the new portal.</span></span>
 
  ![Egyéni jelszó vaulting alkalmazások az Azure AD integrálása](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

<span data-ttu-id="6f9fe-174">**A "Kapcsolja a saját alkalmazásai" funkció kivétele**:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-174">**To check out the 'Bring your own applications' feature**:</span></span>

1. <span data-ttu-id="6f9fe-175">Miután beállította a egyetlen bejelentkezés módját egy új egyéni alkalmazás, amely való felvételét **jelszóalapú bejelentkezés**, adja meg az URL-cím, ahol az alkalmazás Ez a beállítás a bejelentkezési képernyőt, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-175">After you set the single sign-on mode for a new custom application that you’ve added to **Password-based Sign-on**, enter the URL where the application renders its login screen and click **Save**.</span></span>  
2. <span data-ttu-id="6f9fe-176">Ha így tesz, azt fogja automatikusan scrape a felhasználónévhez URL-címet, és jelszó beviteli mező, és biztonságos átvitelére a jelszavakat, hogy a hozzáférési panel bővítmény használó alkalmazások az Azure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-176">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="configure-self-service-application-access"></a><span data-ttu-id="6f9fe-177">Önkiszolgáló alkalmazás-hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f9fe-177">Configure self-service application access</span></span>

<span data-ttu-id="6f9fe-178">Nagy mennyiségű új alkalmazások hozzáadása után lehet, hogy engedélyezni szeretné a felhasználókat, keresse meg és adja hozzá ezeket az alkalmazásokat a saját hozzáférés panelek anélkül, hogy rendszergazdaként többet jelzi, hogy.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-178">After you’ve added lots of new applications, maybe you want to allow your users to browse and add those applications to their own access panels, without needing to bother you as an administrator.</span></span> <span data-ttu-id="6f9fe-179">Most ezzel a legújabb verzióval, konfigurálható és kezelhető önkiszolgáló alkalmazás-hozzáférés közvetlenül a az új portál.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-179">Now, with this latest release, you can configure and manage self-service application access right from the new portal.</span></span>

  ![Önkiszolgáló alkalmazás hozzáférése a jelszót egyszeri bejelentkezési alkalmazás konfigurálása](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
<span data-ttu-id="6f9fe-181">**Önkiszolgáló alkalmazás-hozzáférés kezelésére és beállítására**:</span><span class="sxs-lookup"><span data-stu-id="6f9fe-181">**To configure and manage self-service application access**:</span></span>

1. <span data-ttu-id="6f9fe-182">Először jelölje ki a **önkiszolgáló** lehetőséget az alkalmazás a bal oldali navigációs menü és állítsa be a **az alkalmazáshoz való hozzáférés kérését?** lehetőséggel "**Igen**".</span><span class="sxs-lookup"><span data-stu-id="6f9fe-182">To get started, you can select the **Self-service** option from the application’s left navigation menu and set the **Allow users to request access to this application?** option to ‘**Yes**’.</span></span> 
2. <span data-ttu-id="6f9fe-183">Ez lehetővé teszi ki jogosult az alkalmazáshoz való hozzáférés jóváhagyásához, és megkapja az önkiszolgáló felhasználók csoport konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-183">This will enable you to configure who is allowed to approve access to this application, and which group self-service users will be added.</span></span> <span data-ttu-id="6f9fe-184">Emellett ha az alkalmazás van konfigurálva, a jelszó egyszeri bejelentkezést, azt is megtudhatja egy másik lehetőség, amely lehetővé teszi, hogy ezek jóváhagyóknak kezelheti a jelszavakat az alkalmazáshoz rendelt opcionálisan engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-184">In addition, if the application is configured for password single-sign on, you’ll also see another option which lets you optionally allow those approvers to manage the passwords assigned to the application.</span></span>

##<a name="feedback"></a><span data-ttu-id="6f9fe-185">Visszajelzés</span><span class="sxs-lookup"><span data-stu-id="6f9fe-185">Feedback</span></span>

<span data-ttu-id="6f9fe-186">Reméljük, például a továbbfejlesztett használata az Azure AD felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-186">We hope you like using the improved Azure AD experience.</span></span> <span data-ttu-id="6f9fe-187">Ne hamarosan visszajelzését!</span><span class="sxs-lookup"><span data-stu-id="6f9fe-187">Please keep the feedback coming!</span></span> <span data-ttu-id="6f9fe-188">Visszajelzését és ötleteket javítására szolgáló utáni a **felügyeleti portál** szakasza a [visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span><span class="sxs-lookup"><span data-stu-id="6f9fe-188">Post your feedback and ideas for improvement in the **Admin Portal** section of our [feedback forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span></span>  <span data-ttu-id="6f9fe-189">Ritkán használt adatok új dolgai minden nap kiépítésével foglalkozó még érdeklődőbbek és az útmutató a shape felhasználása és határozza meg, mi készíteni.</span><span class="sxs-lookup"><span data-stu-id="6f9fe-189">We’re excited about building cool new stuff every day, and use your guidance to shape and define what we build next.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f9fe-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f9fe-190">Next steps</span></span>

<span data-ttu-id="6f9fe-191">További részletekért lásd: [alkalmazások kezelése az Azure Active Directoryval](active-directory-enable-sso-scenario.md).</span><span class="sxs-lookup"><span data-stu-id="6f9fe-191">For more details, see [Managing Applications with Azure Active Directory](active-directory-enable-sso-scenario.md).</span></span>



