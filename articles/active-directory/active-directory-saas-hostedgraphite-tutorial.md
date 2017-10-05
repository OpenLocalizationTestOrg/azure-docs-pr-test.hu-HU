---
title: "Oktatóanyag: Azure Active Directoryval integrált üzemeltetett grafit |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az üzemeltetett grafit között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f6ed02cc67be4090402a115c30819ff6cff99c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a><span data-ttu-id="9c5e4-103">Oktatóanyag: Azure Active Directoryval integrált üzemeltetett grafit</span><span class="sxs-lookup"><span data-stu-id="9c5e4-103">Tutorial: Azure Active Directory integration with Hosted Graphite</span></span>

<span data-ttu-id="9c5e4-104">Ebben az oktatóanyagban elsajátíthatja üzemeltetett grafit integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9c5e4-104">In this tutorial, you learn how to integrate Hosted Graphite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c5e4-105">Üzemeltetett grafit integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-105">Integrating Hosted Graphite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c5e4-106">Szabályozhatja az Azure ad-ben üzemeltetett grafit hozzáféréssel rendelkező</span><span class="sxs-lookup"><span data-stu-id="9c5e4-106">You can control in Azure AD who has access to Hosted Graphite</span></span>
- <span data-ttu-id="9c5e4-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az üzemeltetett grafit (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9c5e4-107">You can enable your users to automatically get signed-on to Hosted Graphite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c5e4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9c5e4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c5e4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c5e4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c5e4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c5e4-110">Prerequisites</span></span>

<span data-ttu-id="9c5e4-111">Az Azure AD-integrációs üzemeltetett grafit konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-111">To configure Azure AD integration with Hosted Graphite, you need the following items:</span></span>

- <span data-ttu-id="9c5e4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9c5e4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c5e4-113">Egy üzemeltetett grafit egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9c5e4-113">A Hosted Graphite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c5e4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c5e4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c5e4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c5e4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c5e4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c5e4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9c5e4-118">Scenario description</span></span>
<span data-ttu-id="9c5e4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c5e4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c5e4-121">Üzemeltetett grafit hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9c5e4-121">Adding Hosted Graphite from the gallery</span></span>
2. <span data-ttu-id="9c5e4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9c5e4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hosted-graphite-from-the-gallery"></a><span data-ttu-id="9c5e4-123">Üzemeltetett grafit hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9c5e4-123">Adding Hosted Graphite from the gallery</span></span>
<span data-ttu-id="9c5e4-124">Az Azure AD integrálása a üzemeltetett grafit konfigurálásához kell hozzáadnia üzemeltetett grafit a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-124">To configure the integration of Hosted Graphite into Azure AD, you need to add Hosted Graphite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c5e4-125">**Adja hozzá az üzemeltetett grafit a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c5e4-125">**To add Hosted Graphite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c5e4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c5e4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c5e4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9c5e4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9c5e4-133">Írja be a keresőmezőbe, **üzemeltetett grafit**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-133">In the search box, type **Hosted Graphite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. <span data-ttu-id="9c5e4-135">Az eredmények panelen válassza ki a **üzemeltetett grafit**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-135">In the results panel, select **Hosted Graphite**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c5e4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9c5e4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9c5e4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló szolgáltatott grafit.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-138">In this section, you configure and test Azure AD single sign-on with Hosted Graphite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9c5e4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a grafit üzemeltetett megfelelőjén felhasználót a felhasználók az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Hosted Graphite is to a user in Azure AD.</span></span> <span data-ttu-id="9c5e4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a üzemeltetett grafit közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-140">In other words, a link relationship between an Azure AD user and the related user in Hosted Graphite needs to be established.</span></span>

<span data-ttu-id="9c5e4-141">Üzemeltetett grafit, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-141">In Hosted Graphite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c5e4-142">Az Azure AD egyszeri bejelentkezést az üzemeltetett grafit tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-142">To configure and test Azure AD single sign-on with Hosted Graphite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c5e4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c5e4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c5e4-145">**[Üzemeltetett grafit tesztfelhasználó létrehozása](#creating-a-hosted-graphite-test-user)**  - való egy megfelelője a Britta Simon üzemeltetett grafit, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-145">**[Creating a Hosted Graphite test user](#creating-a-hosted-graphite-test-user)** - to have a counterpart of Britta Simon in Hosted Graphite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c5e4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c5e4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c5e4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c5e4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c5e4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az üzemeltetett grafit alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Hosted Graphite application.</span></span>

<span data-ttu-id="9c5e4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés üzemeltetett grafit, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c5e4-150">**To configure Azure AD single sign-on with Hosted Graphite, perform the following steps:**</span></span>

1. <span data-ttu-id="9c5e4-151">Az Azure portálon a a **üzemeltetett grafit** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-151">In the Azure portal, on the **Hosted Graphite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9c5e4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. <span data-ttu-id="9c5e4-155">Az a **üzemeltetett grafit tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-155">On the **Hosted Graphite Domain and URLs** section, if you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    <span data-ttu-id="9c5e4-157">a.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-157">a.</span></span> <span data-ttu-id="9c5e4-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.hostedgraphite.com/metadata/<user id>`</span><span class="sxs-lookup"><span data-stu-id="9c5e4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/metadata/<user id>`</span></span>

    <span data-ttu-id="9c5e4-159">b.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-159">b.</span></span> <span data-ttu-id="9c5e4-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.hostedgraphite.com/complete/saml/<user id>`</span><span class="sxs-lookup"><span data-stu-id="9c5e4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/complete/saml/<user id>`</span></span>

4. <span data-ttu-id="9c5e4-161">Az a **üzemeltetett grafit tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-161">On the **Hosted Graphite Domain and URLs** section, if you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    <span data-ttu-id="9c5e4-163">a.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-163">a.</span></span> <span data-ttu-id="9c5e4-164">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="9c5e4-164">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="9c5e4-165">b.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-165">b.</span></span> <span data-ttu-id="9c5e4-166">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.hostedgraphite.com/login/saml/<user id>/`</span><span class="sxs-lookup"><span data-stu-id="9c5e4-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://www.hostedgraphite.com/login/saml/<user id>/`</span></span>   

    > [!NOTE] 
    > <span data-ttu-id="9c5e4-167">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-167">Please note that these are not the real values.</span></span> <span data-ttu-id="9c5e4-168">Akkor kell frissíteni ezeket az értékeket a tényleges azonosítója, válasz és bejelentkezési az URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-168">You have to update these values with the actual Identifier, Reply URL and Sign On URL.</span></span> <span data-ttu-id="9c5e4-169">Ahhoz, hogy ezeket az értékeket, elvégezheti a hozzáférés -> az alkalmazás oldalán, vagy forduljon a SAML-alapú telepítő [üzemeltetett grafit támogatási csoport](mailto:help@hostedgraphite.com).</span><span class="sxs-lookup"><span data-stu-id="9c5e4-169">To get these values, you can go to Access->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).</span></span>
    >
 
5. <span data-ttu-id="9c5e4-170">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-170">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. <span data-ttu-id="9c5e4-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-172">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="9c5e4-174">A a **üzemeltetett grafit konfigurációs** kattintson **üzemeltetett grafit konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-174">On the **Hosted Graphite Configuration** section, click **Configure Hosted Graphite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9c5e4-175">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9c5e4-175">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. <span data-ttu-id="9c5e4-177">Bejelentkezés az üzemeltetett grafit bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-177">Sign-on to your Hosted Graphite tenant as an administrator.</span></span>

9. <span data-ttu-id="9c5e4-178">Lépjen a **SAML-alapú telepítő oldalán** az oldalsávon (**Access -> SAML-alapú telepítő**).</span><span class="sxs-lookup"><span data-stu-id="9c5e4-178">Go to the **SAML Setup page** in the sidebar (**Access -> SAML Setup**).</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. <span data-ttu-id="9c5e4-180">URL-egyezniük hajtható végre a konfigurációt a **üzemeltetett grafit tartomány és az URL-címek** szakasza az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-180">Confirm these URls match your configuration done on the **Hosted Graphite Domain and URLs** section of the Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. <span data-ttu-id="9c5e4-182">A **entitás vagy a kibocsátó azonosító** és **Egyszeri bejelentkezési URL-cím** szövegmezőből, illessze be az értékét **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-182">In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste the value of **SAML Entity ID** and **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. <span data-ttu-id="9c5e4-184">Jelölje ki "**írásvédett**" mint **felhasználói szerepkör alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-184">Select "**Read-only**" as **Default User Role**.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. <span data-ttu-id="9c5e4-186">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról letöltött, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. <span data-ttu-id="9c5e4-188">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="9c5e4-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9c5e4-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c5e4-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c5e4-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c5e4-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c5e4-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c5e4-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c5e4-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9c5e4-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c5e4-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c5e4-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c5e4-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c5e4-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c5e4-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9c5e4-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c5e4-204">a.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-204">a.</span></span> <span data-ttu-id="9c5e4-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c5e4-206">b.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-206">b.</span></span> <span data-ttu-id="9c5e4-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c5e4-208">c.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-208">c.</span></span> <span data-ttu-id="9c5e4-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c5e4-210">d.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-210">d.</span></span> <span data-ttu-id="9c5e4-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-211">Click **Create**.</span></span>
 
### <a name="creating-a-hosted-graphite-test-user"></a><span data-ttu-id="9c5e4-212">Üzemeltetett grafit tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c5e4-212">Creating a Hosted Graphite test user</span></span>

<span data-ttu-id="9c5e4-213">Ez a szakasz célja üzemeltetett grafit Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-213">The objective of this section is to create a user called Britta Simon in Hosted Graphite.</span></span> <span data-ttu-id="9c5e4-214">Üzemeltetett grafit támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-214">Hosted Graphite supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9c5e4-215">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-215">There is no action item for you in this section.</span></span> <span data-ttu-id="9c5e4-216">Új felhasználó jön létre az üzemeltetett grafit elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-216">A new user will be created during an attempt to access Hosted Graphite if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="9c5e4-217">Hozza létre a felhasználó manuálisan kell, ha szeretné-e a üzemeltetett grafit támogatási csoportjához keresztül < mailto:help@hostedgraphite.com >.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-217">If you need to create a user manually, you need to contact the Hosted Graphite support team via <mailto:help@hostedgraphite.com>.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9c5e4-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9c5e4-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9c5e4-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés üzemeltetett grafit Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Hosted Graphite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9c5e4-221">**Az üzemeltetett grafit Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9c5e4-221">**To assign Britta Simon to Hosted Graphite, perform the following steps:**</span></span>

1. <span data-ttu-id="9c5e4-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9c5e4-224">Az alkalmazások listában válassza ki a **üzemeltetett grafit**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-224">In the applications list, select **Hosted Graphite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. <span data-ttu-id="9c5e4-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9c5e4-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-228">Click **Add** button.</span></span> <span data-ttu-id="9c5e4-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9c5e4-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c5e4-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c5e4-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c5e4-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9c5e4-234">Testing single sign-on</span></span>

<span data-ttu-id="9c5e4-235">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-235">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9c5e4-236">Ha a hozzáférési Panel üzemeltetett grafit mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az üzemeltetett grafit alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9c5e4-236">When you click the Hosted Graphite tile in the Access Panel, you should get automatically signed-on to your Hosted Graphite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c5e4-237">További források</span><span class="sxs-lookup"><span data-stu-id="9c5e4-237">Additional resources</span></span>

* [<span data-ttu-id="9c5e4-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9c5e4-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c5e4-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9c5e4-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

