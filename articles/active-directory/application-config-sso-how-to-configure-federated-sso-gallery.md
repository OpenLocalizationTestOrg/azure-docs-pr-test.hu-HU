---
title: "Összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása |} Microsoft Docs"
description: "Összevont egyszeri bejelentkezéshez egy meglévő Azure AD-katalógusában és használati oktatóanyagok gyorsan induláshoz konfigurálása"
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
ms.openlocfilehash: 1b1d00718981b2c7d11f5b88428d02e16dd0b34d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="07125-103">Összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="07125-103">How to configure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="07125-104">Minden alkalmazást vállalati egyszeri bejelentkezésre alkalmas engedélyezve van az Azure AD-katalógus elérhető egy részletes oktatóanyag rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="07125-104">All applications in the Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="07125-105">Megnyitja a [SaaS-alkalmazásokhoz az Azure Active Directoryval integrációjával kapcsolatos bemutatók felsorolása](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) részletes, lépésenkénti útmutatást.</span><span class="sxs-lookup"><span data-stu-id="07125-105">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="07125-106">A szükséges lépések – áttekintés</span><span class="sxs-lookup"><span data-stu-id="07125-106">Overview of steps required</span></span>
<span data-ttu-id="07125-107">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="07125-107">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="07125-108">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="07125-108">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="07125-109">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="07125-109">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="07125-110">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="07125-110">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="07125-111">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="07125-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="07125-112">Konfigurálja az Azure AD metaadatok értékeket az alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="07125-112">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="07125-113">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="07125-113">Assign users to the application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="07125-114">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="07125-114">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="07125-115">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07125-115">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="07125-116">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="07125-116">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="07125-117">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="07125-117">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="07125-118">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="07125-118">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="07125-119">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-119">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="07125-120">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="07125-120">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="07125-121">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="07125-121">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="07125-122">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.</span><span class="sxs-lookup"><span data-stu-id="07125-122">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="07125-123">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="07125-123">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="07125-124">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="07125-124">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="07125-125">Rövid időn belül el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="07125-125">After a short period of time, you be able to see the application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="07125-126">Egyszeri bejelentkezés egy alkalmazás az Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="07125-126">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="07125-127">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07125-127">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="07125-128">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="07125-128">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="07125-129">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="07125-129">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="07125-130">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="07125-130">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="07125-131">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-131">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="07125-132">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-132">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="07125-133">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="07125-133">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="07125-134">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="07125-134">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="07125-135">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-135">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="07125-136">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="07125-136">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="07125-137">Adja meg a szükséges értékeket a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="07125-137">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="07125-138">Ezeket az értékeket az alkalmazás gyártójának szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="07125-138">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="07125-139">Az alkalmazás beállítása a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, a bejelentkezési URL-címen nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="07125-139">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="07125-140">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="07125-140">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="07125-141">Az alkalmazás beállítása a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, a válasz URL-CÍMEN nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="07125-141">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="07125-142">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="07125-142">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="07125-143">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha meg szeretné tekinteni a nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="07125-143">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="07125-144">Az a **felhasználói attribútumok**, válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="07125-144">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="07125-145">**Választható lehetőség:** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-145">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

  <span data-ttu-id="07125-146">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="07125-146">To add an attribute:</span></span>
   
   1. <span data-ttu-id="07125-147">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="07125-147">click **Add attribute**.</span></span> <span data-ttu-id="07125-148">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="07125-148">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   1. <span data-ttu-id="07125-149">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="07125-149">Click **Save.**</span></span> <span data-ttu-id="07125-150">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="07125-150">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="07125-151">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  hozzáférés dokumentációjának egyszeri bejelentkezést az alkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="07125-151">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="07125-152">Is hogy rendelkezik, a metadata URL-címek és az egyszeri bejelentkezés beállítása az alkalmazáshoz szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="07125-152">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="07125-153">Kattintson a **mentése** a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-153">Click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="07125-154">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="07125-154">Assign users to the application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="07125-155">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="07125-155">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="07125-156">Válassza ki a felhasználói azonosító, vagy adja hozzá a felhasználói attribútumok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07125-156">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="07125-157">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="07125-157">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="07125-158">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="07125-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="07125-159">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="07125-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="07125-160">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="07125-161">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-161">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="07125-162">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="07125-162">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="07125-163">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="07125-163">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="07125-164">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-164">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="07125-165">Az a **felhasználói attribútumok** területen válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="07125-165">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="07125-166">A kiválasztott beállítás a várt érték a felhasználó hitelesítésére az alkalmazásban egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="07125-166">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="07125-167">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="07125-167">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="07125-168">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy szakaszban.</span><span class="sxs-lookup"><span data-stu-id="07125-168">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="07125-169">Felhasználói attribútumok hozzáadásához kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-169">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="07125-170">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="07125-170">To add an attribute:</span></span>
  
   1. <span data-ttu-id="07125-171">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="07125-171">click **Add attribute**.</span></span> <span data-ttu-id="07125-172">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="07125-172">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="07125-173">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="07125-173">Click **Save**.</span></span> <span data-ttu-id="07125-174">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="07125-174">You see the new attribute in the table.</span></span>

## <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="07125-175">Az Azure AD-metaadatok vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="07125-175">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="07125-176">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07125-176">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="07125-177">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="07125-177">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="07125-178">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="07125-178">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="07125-179">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="07125-179">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="07125-180">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-180">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="07125-181">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-181">click **All Applications** to view a list of all your applications.</span></span>

  *  <span data-ttu-id="07125-182">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="07125-182">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications**.</span></span>

6.  <span data-ttu-id="07125-183">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="07125-183">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="07125-184">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-184">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="07125-185">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="07125-185">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="07125-186">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="07125-186">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

<span data-ttu-id="07125-187">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="07125-187">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="07125-188">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="07125-188">The metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-to-the-application"></a><span data-ttu-id="07125-189">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="07125-189">Assign users to the application</span></span>

<span data-ttu-id="07125-190">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="07125-190">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="07125-191">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="07125-191">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="07125-192">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="07125-192">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="07125-193">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="07125-193">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="07125-194">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-194">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="07125-195">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="07125-195">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="07125-196">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="07125-196">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="07125-197">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="07125-197">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="07125-198">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="07125-198">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="07125-199">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="07125-199">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="07125-200">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="07125-200">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="07125-201">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="07125-201">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="07125-202">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="07125-202">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="07125-203">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="07125-203">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="07125-204">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="07125-204">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="07125-205">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="07125-205">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="07125-206">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="07125-206">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="07125-207">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="07125-207">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="07125-208">Rövid időn belül a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a megoldás leírása szakaszban ismertetett módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="07125-208">After a short period of time, the users you have selected be able to launch these applications using the methods described in the solution description section.</span></span>

## <a name="customizing-the-saml-claims-sent-to-an-application"></a><span data-ttu-id="07125-209">A kérelmet küldött SAML-jogcímek testreszabása</span><span class="sxs-lookup"><span data-stu-id="07125-209">Customizing the SAML claims sent to an application</span></span>

<span data-ttu-id="07125-210">Megtudhatja, hogyan szabhatja testre a SAML attribútum típusú jogcímek az alkalmazás számára, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.</span><span class="sxs-lookup"><span data-stu-id="07125-210">To learn how to customize the SAML attribute claims sent to your application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07125-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07125-211">Next steps</span></span>
[<span data-ttu-id="07125-212">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="07125-212">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



