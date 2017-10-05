---
title: "Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az SAP Business objektumot felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: 6d517c5e302ac36e5bba2053998c75f8f4d42683
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a><span data-ttu-id="5d151-103">Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő</span><span class="sxs-lookup"><span data-stu-id="5d151-103">Tutorial: Azure Active Directory integration with SAP Business Object Cloud</span></span>

<span data-ttu-id="5d151-104">Ebben az oktatóanyagban elsajátíthatja SAP Business objektumot felhő integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5d151-104">In this tutorial, you learn how to integrate SAP Business Object Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d151-105">SAP Business objektumot felhőalapú Azure AD-val integrálásakor kapott a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5d151-105">You get the following benefits when you integrate SAP Business Object Cloud with Azure AD:</span></span>

- <span data-ttu-id="5d151-106">Az Azure ad-ben szabályozhatja, ki férhet hozzá az SAP Business objektumot felhő.</span><span class="sxs-lookup"><span data-stu-id="5d151-106">In Azure AD, you can control who has access to SAP Business Object Cloud.</span></span>
- <span data-ttu-id="5d151-107">Automatikusan bejelentkezhet a felhasználókat, hogy SAP Business objektumot felhő egyszeri bejelentkezést és a felhasználó Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="5d151-107">You can automatically sign in your users to SAP Business Object Cloud by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="5d151-108">A fiók egyetlen, központi helyen, az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="5d151-108">You can manage your accounts in one, central location, the Azure portal.</span></span>

<span data-ttu-id="5d151-109">További információért, egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatban lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d151-109">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d151-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d151-110">Prerequisites</span></span>

<span data-ttu-id="5d151-111">A SAP Business objektumot felhőalapú Azure AD-integráció beállítása, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5d151-111">To set up Azure AD integration with SAP Business Object Cloud, you need the following items:</span></span>

- <span data-ttu-id="5d151-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5d151-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d151-113">Egy SAP üzleti objektum felhőben, az egyszeri bejelentkezés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="5d151-113">An SAP Business Object Cloud, with single sign-on turned on</span></span>

> [!NOTE]
> <span data-ttu-id="5d151-114">Ha ebben az oktatóanyagban teszteli a lépéseket, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5d151-114">If you test the steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="5d151-115">Ebben az oktatóanyagban a lépéseket tesztelési ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="5d151-115">Recommendations for testing the steps in this tutorial:</span></span>

- <span data-ttu-id="5d151-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5d151-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="5d151-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d151-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5d151-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5d151-118">Scenario description</span></span>
<span data-ttu-id="5d151-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5d151-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="5d151-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5d151-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d151-121">SAP Business objektumot felhő hozzáadása a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="5d151-121">Add SAP Business Object Cloud from the gallery.</span></span>
2. <span data-ttu-id="5d151-122">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5d151-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-sap-business-object-cloud-from-the-gallery"></a><span data-ttu-id="5d151-123">SAP Business objektumot felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="5d151-123">Add SAP Business Object Cloud from the gallery</span></span>
<span data-ttu-id="5d151-124">A SAP üzleti objektum felhőalapú Azure ad-integráció beállítása a katalógusban, vegye fel SAP Business objektumot felhőalapú a kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="5d151-124">To set up the integration of SAP Business Object Cloud with Azure AD, in the gallery, add SAP Business Object Cloud to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5d151-125">SAP Business objektumot felhő hozzáadása a gyűjteményből:</span><span class="sxs-lookup"><span data-stu-id="5d151-125">To add SAP Business Object Cloud from the gallery:</span></span>

1. <span data-ttu-id="5d151-126">Az a [Azure-portálon](https://portal.azure.com), a bal oldali menüben válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5d151-126">In the [Azure portal](https://portal.azure.com), in the left menu, select **Azure Active Directory**.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="5d151-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d151-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![A vállalati alkalmazások lap][2]
    
3. <span data-ttu-id="5d151-130">Új alkalmazás hozzáadásához válassza **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d151-130">To add a new application, select **New application**.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="5d151-132">A keresési mezőbe, írja be a **SAP Business objektumot felhő**.</span><span class="sxs-lookup"><span data-stu-id="5d151-132">In the search box, enter **SAP Business Object Cloud**.</span></span>

    ![A keresési mezőbe](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. <span data-ttu-id="5d151-134">Az eredmények panelen válassza ki a **SAP Business objektumot felhő**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5d151-134">In the results panel, select **SAP Business Object Cloud**, and then select **Add**.</span></span>

    ![SAP Business objektumot felhő az eredménylistában](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5d151-136">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d151-136">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="5d151-137">Ebben a szakaszban a beállítása és tesztelése az Azure AD az egyszeri bejelentkezés SAP üzleti objektum a felhő alapú nevű tesztfelhasználó *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="5d151-137">In this section, you set up and test Azure AD single sign-on with SAP Business Object Cloud based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="5d151-138">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, az Azure AD-partner felhasználó SAP Business objektumot felhőben.</span><span class="sxs-lookup"><span data-stu-id="5d151-138">For single sign-on to work, Azure AD needs to know the Azure AD counterpart user in SAP Business Object Cloud.</span></span> <span data-ttu-id="5d151-139">Egy Azure AD-felhasználó és a kapcsolódó felhasználó SAP Business objektumot felhőben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5d151-139">A link relationship between an Azure AD user and the related user in SAP Business Object Cloud must be established.</span></span>

<span data-ttu-id="5d151-140">A hivatkozás kapcsolat, SAP Business objektumot felhő létrehozására a **felhasználónév**, rendelje az értékét a **felhasználónév** az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5d151-140">To establish the link relationship, in SAP Business Object Cloud, for **Username**, assign the value of the **user name** in Azure AD.</span></span>

<span data-ttu-id="5d151-141">Az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú tesztelése és konfigurálása, végezze el a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="5d151-141">To configure and test Azure AD single sign-on with SAP Business Object Cloud, complete the following tasks:</span></span>

1. <span data-ttu-id="5d151-142">[Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="5d151-142">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="5d151-143">A felhasználó beállítja a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5d151-143">Sets up a user to use this feature.</span></span>
2. <span data-ttu-id="5d151-144">[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="5d151-144">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="5d151-145">Az Azure AD tesztek egyszeri bejelentkezés Britta Simon felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="5d151-145">Tests Azure AD single sign-on with the user Britta Simon.</span></span>
3. <span data-ttu-id="5d151-146">[Hozzon létre egy SAP Business objektumot felhő tesztfelhasználó](#create-an-sap-business-object-cloud-test-user).</span><span class="sxs-lookup"><span data-stu-id="5d151-146">[Create an SAP Business Object Cloud test user](#create-an-sap-business-object-cloud-test-user).</span></span> <span data-ttu-id="5d151-147">Létrehoz egy Britta Simon megfelelője a felhőben SAP Business objektumot, amely csatolva van a felhasználó az Azure AD ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5d151-147">Creates a counterpart of Britta Simon in SAP Business Object Cloud that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="5d151-148">[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="5d151-148">[Assign the Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="5d151-149">Állítja be az Azure AD használatára Britta Simon egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="5d151-149">Sets up Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5d151-150">[Egyszeri bejelentkezés tesztelése](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="5d151-150">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="5d151-151">Ellenőrzi, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5d151-151">Verifies that the configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="5d151-152">Az Azure AD az egyszeri bejelentkezés beállítása</span><span class="sxs-lookup"><span data-stu-id="5d151-152">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="5d151-153">Ebben a szakaszban bekapcsolása az Azure AD-egyszeri bejelentkezés az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5d151-153">In this section, you turn on Azure AD single sign-on in the Azure portal.</span></span> <span data-ttu-id="5d151-154">Ezután állítsa be az egyszeri bejelentkezés az SAP Business objektumot felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5d151-154">Then, you set up single sign-on in your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="5d151-155">Beállítása az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú:</span><span class="sxs-lookup"><span data-stu-id="5d151-155">To set up Azure AD single sign-on with SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="5d151-156">Az Azure portálon a a **SAP Business objektumot felhő** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5d151-156">In the Azure portal, on the **SAP Business Object Cloud** application integration page, select **Single sign-on**.</span></span>

    ![Válassza ki az egyszeri bejelentkezés][4]

2. <span data-ttu-id="5d151-158">Az a **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5d151-158">On the **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Válassza ki a SAML-alapú bejelentkezés](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. <span data-ttu-id="5d151-160">A **SAP Business objektumot felhőalapú tartományt és URL-címek**, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d151-160">Under **SAP Business Object Cloud Domain and URLs**, complete the following steps:</span></span>

    1. <span data-ttu-id="5d151-161">Az a **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:</span><span class="sxs-lookup"><span data-stu-id="5d151-161">In the **Sign-on URL** box, enter a URL that has the following pattern:</span></span> 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. <span data-ttu-id="5d151-162">Az a **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:</span><span class="sxs-lookup"><span data-stu-id="5d151-162">In the **Identifier** box, enter a URL that has the following pattern:</span></span>
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business objektumot felhőalapú tartományt és URL-címek lap URL-címek](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > <span data-ttu-id="5d151-164">Az URL-címek értékei csak bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="5d151-164">The values in these URLs are for demonstration only.</span></span> <span data-ttu-id="5d151-165">Módosítsa a tényleges bejelentkezési URL-cím és azonosító URL-t.</span><span class="sxs-lookup"><span data-stu-id="5d151-165">Update the values with the actual sign-on URL and identifier URL.</span></span> <span data-ttu-id="5d151-166">A bejelentkezési URL-cím beszerzése, lépjen kapcsolatba a [SAP Business objektumot felhőalapú ügyfél-támogatási csoport](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span><span class="sxs-lookup"><span data-stu-id="5d151-166">To get the sign-on URL, contact the [SAP Business Object Cloud Client support team](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span></span> <span data-ttu-id="5d151-167">Az azonosító URL-t kaphat az SAP Business objektumot felhő metaadatok letöltése a felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="5d151-167">You can get the identifier URL by downloading the SAP Business Object Cloud metadata from the admin console.</span></span> <span data-ttu-id="5d151-168">Ennek a magyarázatát az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="5d151-168">This is explained later in the tutorial.</span></span> 

4. <span data-ttu-id="5d151-169">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**.</span><span class="sxs-lookup"><span data-stu-id="5d151-169">Under **SAML Signing Certificate**, select **Metadata XML**.</span></span> <span data-ttu-id="5d151-170">Mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5d151-170">Then, save the metadata file on your computer.</span></span>

    ![Válassza ki a metaadatok XML](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. <span data-ttu-id="5d151-172">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d151-172">Select **Save**.</span></span>

    ![A mentés kiválasztása](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5d151-174">Egy másik webes böngészőablakban jelentkezzen be a SAP Business objektumot felhő vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5d151-174">In a different web browser window, sign in to your SAP Business Object Cloud company site as an administrator.</span></span>

7. <span data-ttu-id="5d151-175">Válassza ki **menü** > **rendszer** > **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="5d151-175">Select **Menu** > **System** > **Administration**.</span></span>
    
    ![Válassza ki a menüben, majd a rendszer, majd felügyeleti](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. <span data-ttu-id="5d151-177">Az a **biztonsági** lapon jelölje be a **szerkesztése** (toll) ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d151-177">On the **Security** tab, select the **Edit** (pen) icon.</span></span>
    
    ![A biztonság lapon válassza ki a Szerkesztés ikonra](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. <span data-ttu-id="5d151-179">A **hitelesítési módszer**, jelölje be **SAML-alapú egyszeri bejelentkezés (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="5d151-179">For **Authentication Method**, select **SAML Single Sign-On (SSO)**.</span></span>

    ![SAML-alapú egyszeri bejelentkezést a hitelesítési mód kiválasztása](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. <span data-ttu-id="5d151-181">Töltse le a service provider metaadatok (1. lépés), jelölje be **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="5d151-181">To download the service provider metadata (Step 1), select **Download**.</span></span> <span data-ttu-id="5d151-182">A metaadatok fájlban keresse meg és másolja a **entityid beállítást** érték.</span><span class="sxs-lookup"><span data-stu-id="5d151-182">In the metadata file, find and copy the **entityID** value.</span></span> <span data-ttu-id="5d151-183">Az Azure portálon a **SAP Business objektumot felhőalapú tartományt és URL-címek**, illessze be az értéket a **azonosító** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="5d151-183">In the Azure portal, under **SAP Business Object Cloud Domain and URLs**, paste the value in the **Identifier** box.</span></span>

    ![Másolja és illessze be a entityid beállítást érték](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. <span data-ttu-id="5d151-185">A service provider metaadatok (2. lépés) feltölteni a fájlt a letöltött Azure-portálról, a **identitásszolgáltató feltöltése metaadatok**, jelölje be **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="5d151-185">To upload the service provider metadata (Step 2) in the file that you downloaded from the Azure portal, under **Upload Identity Provider metadata**, select **Upload**.</span></span>  

    ![A metaadatok feltöltése identitásszolgáltató, területen válassza a feltöltés](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. <span data-ttu-id="5d151-187">Az a **felhasználói attribútum** listára, válassza ki a megvalósítás a használni kívánt felhasználói attribútum (3. lépés).</span><span class="sxs-lookup"><span data-stu-id="5d151-187">In the **User Attribute** list, select the user attribute (Step 3) that you want to use for your implementation.</span></span> <span data-ttu-id="5d151-188">A felhasználói attribútum az identitásszolgáltató van leképezve.</span><span class="sxs-lookup"><span data-stu-id="5d151-188">This user attribute maps to the identity provider.</span></span> <span data-ttu-id="5d151-189">Egyéni attribútumot a felhasználói oldal, használja a **egyéni SAML leképezési** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5d151-189">To enter a custom attribute on the user's page, use the **Custom SAML Mapping** option.</span></span> <span data-ttu-id="5d151-190">Vagy, akkor ki lehet **E-mail** vagy **Felhasználóazonosító** felhasználói attribútumként.</span><span class="sxs-lookup"><span data-stu-id="5d151-190">Or, you can select either **Email** or **USER ID** as the user attribute.</span></span> <span data-ttu-id="5d151-191">Ebben a példában, hogy kiválasztott **E-mail** mivel azt a felhasználói azonosító jogcímet leképezése a **userprincipalname** attribútumnak a **felhasználói attribútumok** szakaszban az Azure-ban portál.</span><span class="sxs-lookup"><span data-stu-id="5d151-191">In our example, we selected **Email** because we mapped the user identifier claim with the **userprincipalname** attribute in the **User Attributes** section in the Azure portal.</span></span> <span data-ttu-id="5d151-192">Ez lehetővé teszi egy egyedi felhasználói e-mailt, az SAP Business objektumot felhő alkalmazás minden sikeres SAML válaszként küldött.</span><span class="sxs-lookup"><span data-stu-id="5d151-192">This provides a unique user email, which is sent to the SAP Business Object Cloud application in every successful SAML response.</span></span>

    ![Válassza ki a felhasználói attribútum](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. <span data-ttu-id="5d151-194">Ellenőrizzük a fiókját az identitásszolgáltató (4. lépés), az a a **bejelentkezési hitelesítő adatok (E-mail)** mezőbe írja be a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="5d151-194">To verify the account with the identity provider (Step 4), in the **Login Credential (Email)** box, enter the user's email address.</span></span> <span data-ttu-id="5d151-195">Ezt követően válassza **ellenőrizze fiókja**.</span><span class="sxs-lookup"><span data-stu-id="5d151-195">Then, select **Verify Account**.</span></span> <span data-ttu-id="5d151-196">A rendszer hozzáadja a felhasználói fiók bejelentkezési hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="5d151-196">The system adds sign-in credentials to the user account.</span></span>

    ![Adja meg e-mail címét, és válassza ki azt a fiókot ellenőrzése](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. <span data-ttu-id="5d151-198">Válassza ki a **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d151-198">Select the **Save** icon.</span></span>

    ![Mentés ikonra](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="5d151-200">Ezek az utasítások a tömör verzióját el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás!</span><span class="sxs-lookup"><span data-stu-id="5d151-200">You can read a concise version of these instructions in the [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="5d151-201">Miután hozzáadta az alkalmazás kiválasztásával **Active Directory** > **vállalati alkalmazások**, jelölje be a **egyszeri bejelentkezés** lapon. A beágyazott dokumentációja a **konfigurációs** szakaszban, a lap alján.</span><span class="sxs-lookup"><span data-stu-id="5d151-201">After you add the app by selecting **Active Directory** > **Enterprise Applications**, select the **Single Sign-On** tab. You can access the embedded documentation in the **Configuration** section, at the bottom of the page.</span></span> <span data-ttu-id="5d151-202">További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="5d151-202">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5d151-203">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5d151-203">Create an Azure AD test user</span></span>
<span data-ttu-id="5d151-204">Ebben a szakaszban az Azure portálon Britta Simon nevű tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d151-204">In this section, you create a test user named Britta Simon in the Azure portal.</span></span>

<span data-ttu-id="5d151-205">Tesztfelhasználó létrehozása az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="5d151-205">To create a test user in Azure AD:</span></span>

1. <span data-ttu-id="5d151-206">Az Azure portálon a bal oldali menüben válassza ki a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5d151-206">In the Azure portal, in the left menu, select **Azure Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d151-208">Válassza ki azon felhasználók listájának megjelenítéséhez **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5d151-208">To display the list of users, select **Users and groups**, and then select **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d151-210">Lehetőségre a **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5d151-210">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d151-212">Az a **felhasználói** párbeszédpanelen töltse ki az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d151-212">In the **User** dialog box, complete the following steps:</span></span>
 
    1. <span data-ttu-id="5d151-213">Az a **neve** adja meg a **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d151-213">In the **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="5d151-214">Az a **felhasználónév** mezőbe írja be a felhasználó Britta Simon e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="5d151-214">In the **User name** box, enter the email address of the user Britta Simon.</span></span>

    3. <span data-ttu-id="5d151-215">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="5d151-215">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    4. <span data-ttu-id="5d151-216">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d151-216">Select **Create**.</span></span>

        ![A felhasználó párbeszédpanel](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Az Azure AD-felhasználó létrehozása][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a><span data-ttu-id="5d151-219">Az SAP Business objektumot felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d151-219">Create an SAP Business Object Cloud test user</span></span>

<span data-ttu-id="5d151-220">Az Azure Active Directory-felhasználók ki kell építenie SAP Business objektumot felhőben, mielőtt bejelentkeznének SAP Business objektumot felhőbe.</span><span class="sxs-lookup"><span data-stu-id="5d151-220">Azure AD users must be provisioned in SAP Business Object Cloud before they can sign in to SAP Business Object Cloud.</span></span> <span data-ttu-id="5d151-221">SAP Business objektumot felhőben kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5d151-221">In SAP Business Object Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="5d151-222">A felhasználói fiók létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="5d151-222">To provision a user account:</span></span>

1. <span data-ttu-id="5d151-223">Jelentkezzen be rendszergazdaként a SAP Business objektumot felhő vállalati webhelyre.</span><span class="sxs-lookup"><span data-stu-id="5d151-223">Sign in to your SAP Business Object Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="5d151-224">Válassza ki **menü** > **biztonsági** > **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5d151-224">Select **Menu** > **Security** > **Users**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. <span data-ttu-id="5d151-226">Az a **felhasználók** lapra, adja hozzá az új felhasználó adatait, válassza ki  **+** .</span><span class="sxs-lookup"><span data-stu-id="5d151-226">On the **Users** page, to add new user details, select **+**.</span></span> 

    ![Felhasználók hozzáadására szolgáló oldala](./media/active-directory-saas-sapboc-tutorial/user4.png)

    <span data-ttu-id="5d151-228">Ezután kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5d151-228">Then, complete the following steps:</span></span>

    1. <span data-ttu-id="5d151-229">Az a **Felhasználóazonosító** mezőbe írja be például a felhasználó a felhasználói azonosító **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5d151-229">In the **USER ID** box, enter the user ID of the user, like **Britta**.</span></span>

    2. <span data-ttu-id="5d151-230">Az a **UTÓNÉV** mezőbe írja be például a felhasználó utóneve **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5d151-230">In the **FIRST NAME** box, enter the first name of the user, like **Britta**.</span></span>

    3. <span data-ttu-id="5d151-231">Az a **Vezetéknév** mezőbe írja be például a felhasználó vezetékneve **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5d151-231">In the **LAST NAME** box, enter the last name of the user, like **Simon**.</span></span>

    4. <span data-ttu-id="5d151-232">Az a **MEGJELENÍTETT név** mezőbe írja be például a teljes nevet, a felhasználó **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5d151-232">In the **DISPLAY NAME** box, enter the full name of the user, like **Britta Simon**.</span></span>

    5. <span data-ttu-id="5d151-233">Az a **E-MAIL** mezőbe írja be például a felhasználó e-mail címe  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="5d151-233">In the **E-MAIL** box, enter the email address of the user, like **brittasimon@contoso.com**.</span></span>

    6. <span data-ttu-id="5d151-234">Az a **szerepkörök kiválasztása** lapon válassza ki a megfelelő szerepkört a felhasználó számára, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d151-234">On the **Select Roles** page, select the appropriate role for the user, and then select **OK**.</span></span>

      ![Szerepkör kiválasztása](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. <span data-ttu-id="5d151-236">Válassza ki a **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d151-236">Select the **Save** icon.</span></span>    


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="5d151-237">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5d151-237">Assign the Azure AD test user</span></span>

<span data-ttu-id="5d151-238">Ebben a szakaszban engedélyezze a felhasználó Britta Simon szerint SAP Business objektumot felhőbe a felhasználóifiók-hozzáférés engedélyezése az Azure AD egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="5d151-238">In this section, you allow the user Britta Simon to use Azure AD single sign-on by granting the user account access to SAP Business Object Cloud.</span></span>

<span data-ttu-id="5d151-239">SAP Business objektumot felhő Britta Simon hozzárendelése:</span><span class="sxs-lookup"><span data-stu-id="5d151-239">To assign Britta Simon to SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="5d151-240">Az Azure portálon az alkalmazások nézet megnyitásához, és keresse meg a könyvtár nézet.</span><span class="sxs-lookup"><span data-stu-id="5d151-240">In the Azure portal, open the applications view, and then go to the directory view.</span></span> <span data-ttu-id="5d151-241">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d151-241">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5d151-243">Az alkalmazások listában válassza ki a **SAP Business objektumot felhő**.</span><span class="sxs-lookup"><span data-stu-id="5d151-243">In the applications list, select **SAP Business Object Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. <span data-ttu-id="5d151-245">A bal oldali menüben válassza ki a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d151-245">In the left menu, select **Users and groups**.</span></span>

    ![Felhasználók és csoportok kiválasztása][202] 

4. <span data-ttu-id="5d151-247">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5d151-247">Select **Add**.</span></span> <span data-ttu-id="5d151-248">Ezt követően a **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d151-248">Then, on the **Add Assignment** page, select **Users and groups**.</span></span>

    ![A hozzárendelés hozzáadása lap][203]

5. <span data-ttu-id="5d151-250">Az a **felhasználók és csoportok** lapra, jelölje be a felhasználók listáján **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5d151-250">On the **Users and groups** page, in the list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="5d151-251">Az a **felhasználók és csoportok** lapon jelölje be **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="5d151-251">On the **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="5d151-252">Az a **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="5d151-252">On the **Add Assignment** page, select **Assign**.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5d151-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d151-254">Test single sign-on</span></span>

<span data-ttu-id="5d151-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5d151-255">In this section, you test your Azure AD single sign-on configuration by using the access panel.</span></span>

<span data-ttu-id="5d151-256">Az SAP Business objektumot felhő csempe a hozzáférési panelen válassza ki, amikor be kell automatikusan jelentkeznie az SAP Business objektumot felhő alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5d151-256">When you select the SAP Business Object Cloud tile in the access panel, you should be automatically signed in to your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="5d151-257">A hozzáférési panel kapcsolatos további információkért lásd: [a hozzáférési panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5d151-257">For more information about the access panel, see [Introduction to the access panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d151-258">További források</span><span class="sxs-lookup"><span data-stu-id="5d151-258">Additional resources</span></span>

* [<span data-ttu-id="5d151-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5d151-259">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d151-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5d151-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

