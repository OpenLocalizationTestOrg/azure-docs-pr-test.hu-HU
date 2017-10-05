---
title: "A probléma összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása |} Microsoft Docs"
description: "Cím gyakori problémák jelentkezhetnek, ha egyetlen konfigurálása összevont bejelentkezést az alkalmazások az Azure AD Application Gallery szereplő SAML használatával"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 290ca66048281de5e031b0404919bed84ab19ffa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="72706-103">A probléma összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72706-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="72706-104">Ha probléma merül fel, ha az alkalmazások konfigurálásáról.</span><span class="sxs-lookup"><span data-stu-id="72706-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="72706-105">Ellenőrizze, hogy a lépéseket követte az oktatóanyag az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="72706-105">Verify you have followed all the steps in the tutorial for the application.</span></span> <span data-ttu-id="72706-106">Az alkalmazás konfigurációját, a beágyazott dokumentációjában az alkalmazás konfigurálásával kapcsolatos rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="72706-106">In the application’s configuration, you have inline documentation on how to configure the application.</span></span> <span data-ttu-id="72706-107">Emellett érheti el a [SaaS-alkalmazásokhoz az Azure Active Directoryval integrációjával kapcsolatos bemutatók felsorolása](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="72706-107">Also, you can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-the-application"></a><span data-ttu-id="72706-108">Az alkalmazás egy másik példánya nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="72706-108">Can’t add another instance of the application</span></span>

<span data-ttu-id="72706-109">Adja hozzá az alkalmazás második példányát, kell tennie:</span><span class="sxs-lookup"><span data-stu-id="72706-109">To add a second instance of an application, you need to be able to:</span></span>

-   <span data-ttu-id="72706-110">Konfigurálja a második példány egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="72706-110">Configure a unique identifier for the second instance.</span></span> <span data-ttu-id="72706-111">Ön nem fog tudni konfigurálása az első példánynál használt ugyanezzel az azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="72706-111">You won’t be able to configure the same identifier used for the first instance.</span></span>

-   <span data-ttu-id="72706-112">Az első példánynál használttól eltérő tanúsítvány konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="72706-112">Configure a different certificate than the one used for the first instance.</span></span>

<span data-ttu-id="72706-113">Ha az alkalmazás nem támogatja a fentiek közül.</span><span class="sxs-lookup"><span data-stu-id="72706-113">If the application doesn’t support any of the above.</span></span> <span data-ttu-id="72706-114">Ezt követően nem fogja tudni konfigurálni egy második példányt.</span><span class="sxs-lookup"><span data-stu-id="72706-114">Then, you won’t be able to configure a second instance.</span></span>

## <a name="cant-add-the-identifier-or-the-reply-url"></a><span data-ttu-id="72706-115">Nem adható hozzá, az azonosító vagy a válasz URL-címe</span><span class="sxs-lookup"><span data-stu-id="72706-115">Can’t add the Identifier or the Reply URL</span></span>

<span data-ttu-id="72706-116">Ha még nem sikerült konfigurálnia az azonosítóját vagy a válasz URL-CÍMEN, győződjön meg arról, a azonosítója és a válasz URL-CÍMEN az értékeknek egyezniük a minta az alkalmazás előre konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="72706-116">If you’re not able to configure the Identifier or the Reply URL, confirm the Identifier and Reply URL values match the patterns pre-configured for the application.</span></span>

<span data-ttu-id="72706-117">Tudni, hogy az alkalmazás előre konfigurálva van a minták:</span><span class="sxs-lookup"><span data-stu-id="72706-117">To know the patterns pre-configured for the application:</span></span>

1.  <span data-ttu-id="72706-118">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="72706-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> <span data-ttu-id="72706-119">Ugorjon a 7.</span><span class="sxs-lookup"><span data-stu-id="72706-119">Go to step 7.</span></span> <span data-ttu-id="72706-120">Ha már az alkalmazás konfigurációs panel az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="72706-120">If you are already in the application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="72706-121">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="72706-121">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="72706-122">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="72706-122">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="72706-123">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="72706-123">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="72706-124">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="72706-124">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="72706-125">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="72706-125">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="72706-126">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="72706-126">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="72706-127">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="72706-127">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="72706-128">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="72706-128">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="72706-129">Lépjen a **azonosítója** vagy **válasz URL-CÍMEN** szövegmező alatt a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="72706-129">Go to the **Identifier** or **Reply URL** textbox, under the **Domain and URLs section.**</span></span>

10. <span data-ttu-id="72706-130">Háromféleképpen tudni, hogy az alkalmazás támogatott minták:</span><span class="sxs-lookup"><span data-stu-id="72706-130">There are three ways to know the supported patterns for the application:</span></span>

   * <span data-ttu-id="72706-131">A szövegmezőhöz látni a támogatott példány szükséges helyőrzőként *példa:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="72706-131">In the textbox, you see the supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="72706-132">a minta nem támogatott, ha egy piros felkiáltójel megjelenik a szövegmezőben adja meg az érték megkísérlésekor.</span><span class="sxs-lookup"><span data-stu-id="72706-132">if the pattern is not supported, you see a red exclamation mark when you try to enter the value in the textbox.</span></span> <span data-ttu-id="72706-133">Ha az egérmutatóval rámutat a piros felkiáltójel, tekintse meg a támogatott kombinációját.</span><span class="sxs-lookup"><span data-stu-id="72706-133">If you hover your mouse over the red exclamation mark, you see the supported patterns.</span></span>

   * <span data-ttu-id="72706-134">Az oktatóanyag az alkalmazáshoz a támogatott mintájával kapcsolatos információkért is kaphat.</span><span class="sxs-lookup"><span data-stu-id="72706-134">In the tutorial for the application, you can also get information about the supported patterns.</span></span> <span data-ttu-id="72706-135">Az a **az Azure AD konfigurálása egyszeri bejelentkezéshez** szakasz.</span><span class="sxs-lookup"><span data-stu-id="72706-135">Under the **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="72706-136">Folytassa a beállítva alatt az értékeket a **tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="72706-136">Go to the step for configured the values under the **Domain and URLs** section.</span></span>

<span data-ttu-id="72706-137">Ha az értékek nem egyeznek meg, a mintákat, előre konfigurált Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="72706-137">If the values don’t match with the patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="72706-138">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="72706-138">You can:</span></span>

-   <span data-ttu-id="72706-139">Az alkalmazás, amely megfelel a mintának előre konfigurált Azure ad-val értékek szállítójával közösen</span><span class="sxs-lookup"><span data-stu-id="72706-139">Work with the application vendor to get values that match the pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="72706-140">Másik lehetőségként felveheti a kapcsolatot az Azure AD-csapatának a < aadapprequest@microsoft.com > vagy megjegyzés hagyja az oktatóprogram kérése a támogatott minták az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="72706-140">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in the tutorial to request the update of the supported patterns for the application</span></span>

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a><span data-ttu-id="72706-141">Ha állította be a entityid beállítást (felhasználói azonosító) formátumát</span><span class="sxs-lookup"><span data-stu-id="72706-141">Where do I set the EntityID (User Identifier) format</span></span>

<span data-ttu-id="72706-142">Ön nem fog tudni válassza ki, hogy az Azure AD elküldi az alkalmazásnak a válaszban szereplő felhasználók hitelesítése után entityid beállítást (felhasználói azonosító) formátumát.</span><span class="sxs-lookup"><span data-stu-id="72706-142">You won’t be able to select the EntityID (User Identifier) format that Azure AD sends to the application in the response after user authentication.</span></span>

<span data-ttu-id="72706-143">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="72706-143">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="72706-144">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) szakaszban NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="72706-144">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy,</span></span>

## <a name="cant-find-the-azure-ad-metadata-to-complete-the-configuration-with-the-application"></a><span data-ttu-id="72706-145">Nem található a az alkalmazás konfigurálása az Azure AD metaadatok</span><span class="sxs-lookup"><span data-stu-id="72706-145">Can’t find the Azure AD metadata to complete the configuration with the application</span></span>

<span data-ttu-id="72706-146">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="72706-146">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="72706-147">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="72706-147">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="72706-148">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="72706-148">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="72706-149">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="72706-149">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="72706-150">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="72706-150">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="72706-151">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="72706-151">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="72706-152">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="72706-152">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="72706-153">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="72706-153">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="72706-154">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="72706-154">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="72706-155">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="72706-155">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="72706-156">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="72706-156">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="72706-157">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="72706-157">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="72706-158">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="72706-158">The metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-to-customize-saml-claims-sent-to-an-application"></a><span data-ttu-id="72706-159">Nem tudjuk kérelmet küldött SAML-jogcímek testreszabása</span><span class="sxs-lookup"><span data-stu-id="72706-159">Don't know how to customize SAML claims sent to an application</span></span>

<span data-ttu-id="72706-160">Megtudhatja, hogyan szabhatja testre a SAML attribútum típusú jogcímek az alkalmazás számára, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="72706-160">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72706-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72706-161">Next steps</span></span>
[<span data-ttu-id="72706-162">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="72706-162">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
