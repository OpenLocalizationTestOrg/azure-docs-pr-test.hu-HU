---
title: "Oktatóanyag: Azure Active Directory-integráció a Adobe kreatív felhőalapú |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az Adobe kreatív felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="7c664-103">Oktatóanyag: Azure Active Directoryval integrált Adobe kreatív felhő</span><span class="sxs-lookup"><span data-stu-id="7c664-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="7c664-104">Ebben az oktatóanyagban elsajátíthatja Adobe kreatív felhő integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c664-104">In this tutorial, you learn how to integrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c664-105">Az Adobe kreatív felhő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7c664-105">Integrating Adobe Creative Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7c664-106">Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik az Adobe kreatív felhőbe</span><span class="sxs-lookup"><span data-stu-id="7c664-106">You can control in Azure AD who has access to Adobe Creative Cloud</span></span>
- <span data-ttu-id="7c664-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Adobe kreatív felhőbe (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="7c664-107">You can enable your users to automatically get signed-on to Adobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c664-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="7c664-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="7c664-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c664-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c664-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7c664-110">Prerequisites</span></span>

<span data-ttu-id="7c664-111">Az Azure AD-integráció konfigurálása a Adobe kreatív felhőalapú, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7c664-111">To configure Azure AD integration with Adobe Creative Cloud, you need the following items:</span></span>

- <span data-ttu-id="7c664-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7c664-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c664-113">Egy Adobe kreatív felhő egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7c664-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c664-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7c664-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c664-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7c664-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c664-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7c664-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="7c664-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c664-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c664-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7c664-118">Scenario description</span></span>
<span data-ttu-id="7c664-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7c664-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c664-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7c664-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c664-121">Az Adobe kreatív felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7c664-121">Adding Adobe Creative Cloud from the gallery</span></span>
2. <span data-ttu-id="7c664-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c664-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a><span data-ttu-id="7c664-123">Az Adobe kreatív felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7c664-123">Adding Adobe Creative Cloud from the gallery</span></span>
<span data-ttu-id="7c664-124">Az Azure AD integrálása a Adobe kreatív felhő konfigurálásához kell hozzáadnia Adobe kreatív felhőalapú a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7c664-124">To configure the integration of Adobe Creative Cloud into Azure AD, you need to add Adobe Creative Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7c664-125">**A gyűjteményből Adobe kreatív felhő hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c664-125">**To add Adobe Creative Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7c664-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c664-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c664-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7c664-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7c664-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c664-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7c664-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7c664-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7c664-133">Írja be a keresőmezőbe, **Adobe kreatív felhő**.</span><span class="sxs-lookup"><span data-stu-id="7c664-133">In the search box, type **Adobe Creative Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="7c664-135">Az eredmények panelen válassza ki a **Adobe kreatív felhő**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7c664-135">In the results panel, select **Adobe Creative Cloud**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c664-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c664-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c664-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Adobe kreatív felhő.</span><span class="sxs-lookup"><span data-stu-id="7c664-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c664-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Adobe kreatív felhőben Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7c664-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Creative Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="7c664-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Adobe kreatív felhőben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7c664-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Creative Cloud needs to be established.</span></span>

<span data-ttu-id="7c664-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Adobe kreatív felhőben.</span><span class="sxs-lookup"><span data-stu-id="7c664-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="7c664-142">Az Azure AD egyszeri bejelentkezést a Adobe kreatív felhőalapú tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7c664-142">To configure and test Azure AD single sign-on with Adobe Creative Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7c664-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7c664-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7c664-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7c664-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c664-145">**[Az Adobe kreatív felhő tesztfelhasználó létrehozása](#creating-an-adobe-creative-cloud-test-user)**  - rendelkezik egy megfelelője a Britta Simon Adobe kreatív felhőben, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7c664-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - to have a counterpart of Britta Simon in Adobe Creative Cloud that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="7c664-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c664-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c664-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7c664-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c664-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c664-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c664-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Adobe kreatív felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7c664-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="7c664-150">**Adobe kreatív felhő konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c664-150">**To configure Azure AD single sign-on with Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7c664-151">Az Azure felügyeleti portálján a a **Adobe kreatív felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7c664-151">In the Azure Management portal, on the **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7c664-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="7c664-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="7c664-155">Az a **Adobe kreatív felhőalapú tartományt és URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="7c664-155">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="7c664-157">a.</span><span class="sxs-lookup"><span data-stu-id="7c664-157">a.</span></span> <span data-ttu-id="7c664-158">Az a **azonosító** szövegmező, írja be az értéket, mint:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="7c664-158">In the **Identifier** textbox, type the value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="7c664-159">b.</span><span class="sxs-lookup"><span data-stu-id="7c664-159">b.</span></span> <span data-ttu-id="7c664-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="7c664-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7c664-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="7c664-161">Please note that these are not the real values.</span></span> <span data-ttu-id="7c664-162">Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="7c664-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="7c664-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="7c664-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="7c664-164">Hozzon létre egy felhasználó manuálisan kell, ha szüksége az Adobe kreatív felhő támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="7c664-164">If you need to create an user manually, you need to contact the Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="7c664-165">Az a **Adobe kreatív felhőalapú tartományt és URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="7c664-165">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="7c664-167">a.</span><span class="sxs-lookup"><span data-stu-id="7c664-167">a.</span></span> <span data-ttu-id="7c664-168">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="7c664-168">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="7c664-169">b.</span><span class="sxs-lookup"><span data-stu-id="7c664-169">b.</span></span> <span data-ttu-id="7c664-170">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="7c664-170">In the **Sign-on URL** textbox, type the value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="7c664-171">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c664-171">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="7c664-173">A a **Adobe egy Felhőkonfigurációk kreatív** kattintson **Adobe kreatív felhő konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7c664-173">On the **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7c664-174">Másolja a **SAML entitásazonosító** és **SAML SSO URL-címe** rövid összefoglaló szakaszából.</span><span class="sxs-lookup"><span data-stu-id="7c664-174">Please copy the **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="7c664-176">Egy másik webes böngészőablakban bejelentkezés az Adobe kreatív felhő bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c664-176">In a different web browser window, sign-on to your Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="7c664-177">Nyissa meg a **identitás** a bal oldali navigációs ablaktáblán kattintson a tartomány.</span><span class="sxs-lookup"><span data-stu-id="7c664-177">Go to **Identity** on the left navigation pane and click your domain.</span></span> <span data-ttu-id="7c664-178">Végezze el az alábbi lépéseket a **egyszeri bejelentkezési a konfiguráció szükséges** szakasz.</span><span class="sxs-lookup"><span data-stu-id="7c664-178">Then perform the following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="7c664-179">![Beállítások](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="7c664-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="7c664-180">Kattintson a **Tallózás** az Azure AD-be a letöltött tanúsítvány feltöltése **IDP tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="7c664-180">Click **Browse** to upload the downloaded certificate from Azure AD to **IDP Certificate**.</span></span>

10. <span data-ttu-id="7c664-181">Az a **IDP kibocsátó** szövegmező, írja be az értéket a **SAML entitásazonosító** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7c664-181">In the **IDP issuer** textbox, put the value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="7c664-182">Az a **IDP bejelentkezési URL-cím** szövegmező, írja be az értéket a **SAML SSO URL-címe** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7c664-182">In the **IDP Login URL** textbox, put the value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="7c664-183">Válassza ki **HTTP - átirányítási** , **IDP kötés**.</span><span class="sxs-lookup"><span data-stu-id="7c664-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="7c664-184">Válassza ki **E-mail cím** , **felhasználói bejelentkezési beállítás**.</span><span class="sxs-lookup"><span data-stu-id="7c664-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="7c664-185">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c664-185">Click **Save** button.</span></span>

15. <span data-ttu-id="7c664-186">Az irányítópult mostantól megjelennek az XML-fájl **"Metaadatok letöltése"** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c664-186">The dashboard will now present the XML **"Download Metadata"** file.</span></span> <span data-ttu-id="7c664-187">Adobe EntityDescriptor és URL-címe AssertionConsumerService tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7c664-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="7c664-188">Nyissa meg a fájlt, és konfigurálja őket az Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7c664-188">Please open the file and configure them in the Azure AD application.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="7c664-191">a.</span><span class="sxs-lookup"><span data-stu-id="7c664-191">a.</span></span> <span data-ttu-id="7c664-192">Használja az Adobe meg a megadott EntityDescriptor értéket **azonosító** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-192">Use the EntityDescriptor value Adobe provided you for **Identifier** on the **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="7c664-193">b.</span><span class="sxs-lookup"><span data-stu-id="7c664-193">b.</span></span> <span data-ttu-id="7c664-194">Használja az Adobe meg a megadott AssertionConsumerService értéket **válasz URL-CÍMEN** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-194">Use the AssertionConsumerService value Adobe provided you for **Reply URL** on the **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c664-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c664-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c664-196">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7c664-196">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7c664-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c664-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7c664-199">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c664-199">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c664-201">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7c664-201">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c664-203">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-203">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c664-205">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7c664-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c664-207">a.</span><span class="sxs-lookup"><span data-stu-id="7c664-207">a.</span></span> <span data-ttu-id="7c664-208">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c664-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c664-209">b.</span><span class="sxs-lookup"><span data-stu-id="7c664-209">b.</span></span> <span data-ttu-id="7c664-210">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c664-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c664-211">c.</span><span class="sxs-lookup"><span data-stu-id="7c664-211">c.</span></span> <span data-ttu-id="7c664-212">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7c664-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7c664-213">d.</span><span class="sxs-lookup"><span data-stu-id="7c664-213">d.</span></span> <span data-ttu-id="7c664-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7c664-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="7c664-215">Az Adobe kreatív felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c664-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="7c664-216">Ahhoz, hogy az Azure AD-felhasználók Adobe kreatív felhő bejelentkezni, akkor ki kell építenie Adobe kreatív felhőbe.</span><span class="sxs-lookup"><span data-stu-id="7c664-216">In order to enable Azure AD users to log into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="7c664-217">Adobe kreatív felhő, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7c664-217">In the case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="7c664-218">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c664-218">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="7c664-219">Jelentkezzen be rendszergazdaként az Adobe kreatív felhő vállalati hely.</span><span class="sxs-lookup"><span data-stu-id="7c664-219">Log in to your Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="7c664-220">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="7c664-220">Click **People**.</span></span>

    <span data-ttu-id="7c664-221">![Személyek](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="7c664-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="7c664-222">Kattintson a **felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="7c664-222">Click **Invite User**.</span></span>

    <span data-ttu-id="7c664-223">![Meghívott felhasználóknak](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "meghívott felhasználóknak")</span><span class="sxs-lookup"><span data-stu-id="7c664-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="7c664-224">Az a **meghívása személyek** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7c664-224">On the **Invite People** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="7c664-225">![Felkérése](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="7c664-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="7c664-226">a.</span><span class="sxs-lookup"><span data-stu-id="7c664-226">a.</span></span> <span data-ttu-id="7c664-227">Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="7c664-227">In the **Email** textbox, type the email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="7c664-228">b.</span><span class="sxs-lookup"><span data-stu-id="7c664-228">b.</span></span> <span data-ttu-id="7c664-229">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="7c664-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c664-230">Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="7c664-230">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7c664-231">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7c664-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7c664-232">Ebben a szakaszban Britta Simon saját hozzáférést biztosít az Adobe kreatív felhő által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c664-232">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Adobe Creative Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7c664-234">**Az Adobe kreatív felhő Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c664-234">**To assign Britta Simon to Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="7c664-235">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c664-235">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7c664-237">Az alkalmazások listában válassza ki a **Adobe kreatív felhő**.</span><span class="sxs-lookup"><span data-stu-id="7c664-237">In the applications list, select **Adobe Creative Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="7c664-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7c664-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7c664-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c664-241">Click **Add** button.</span></span> <span data-ttu-id="7c664-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7c664-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7c664-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7c664-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c664-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c664-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c664-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7c664-247">Testing single sign-on</span></span>

<span data-ttu-id="7c664-248">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7c664-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7c664-249">Ha a hozzáférési Panel Adobe kreatív felhő mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az Adobe kreatív felhő alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7c664-249">When you click the Adobe Creative Cloud tile in the Access Panel, you should get automatically signed-on to your Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7c664-250">További források</span><span class="sxs-lookup"><span data-stu-id="7c664-250">Additional resources</span></span>

* [<span data-ttu-id="7c664-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7c664-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c664-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7c664-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png