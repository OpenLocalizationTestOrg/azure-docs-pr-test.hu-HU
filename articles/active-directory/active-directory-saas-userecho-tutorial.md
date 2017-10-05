---
title: "Oktatóanyag: Azure Active Directoryval integrált UserEcho |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és UserEcho között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2d824d8d5eb8a25db128397b908a126bd87213ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a><span data-ttu-id="36247-103">Oktatóanyag: Azure Active Directoryval integrált UserEcho</span><span class="sxs-lookup"><span data-stu-id="36247-103">Tutorial: Azure Active Directory integration with UserEcho</span></span>

<span data-ttu-id="36247-104">Ebben az oktatóanyagban elsajátíthatja UserEcho integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36247-104">In this tutorial, you learn how to integrate UserEcho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36247-105">UserEcho integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="36247-105">Integrating UserEcho with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="36247-106">Megadhatja a UserEcho hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="36247-106">You can control in Azure AD who has access to UserEcho</span></span>
- <span data-ttu-id="36247-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett UserEcho (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="36247-107">You can enable your users to automatically get signed-on to UserEcho (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36247-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="36247-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="36247-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36247-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36247-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="36247-110">Prerequisites</span></span>

<span data-ttu-id="36247-111">Konfigurálása az Azure AD-integrációs UserEcho, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="36247-111">To configure Azure AD integration with UserEcho, you need the following items:</span></span>

- <span data-ttu-id="36247-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="36247-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36247-113">Egy UserEcho egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="36247-113">A UserEcho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="36247-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="36247-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="36247-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="36247-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36247-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="36247-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="36247-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36247-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="36247-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="36247-118">Scenario description</span></span>
<span data-ttu-id="36247-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="36247-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36247-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="36247-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36247-121">A gyűjteményből UserEcho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="36247-121">Adding UserEcho from the gallery</span></span>
2. <span data-ttu-id="36247-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36247-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-userecho-from-the-gallery"></a><span data-ttu-id="36247-123">A gyűjteményből UserEcho hozzáadása</span><span class="sxs-lookup"><span data-stu-id="36247-123">Adding UserEcho from the gallery</span></span>
<span data-ttu-id="36247-124">Az Azure AD integrálása a UserEcho konfigurálásához kell hozzáadnia UserEcho a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="36247-124">To configure the integration of UserEcho into Azure AD, you need to add UserEcho from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="36247-125">**A gyűjteményből UserEcho hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36247-125">**To add UserEcho from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="36247-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36247-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36247-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="36247-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="36247-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36247-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="36247-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="36247-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="36247-133">Írja be a keresőmezőbe, **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="36247-133">In the search box, type **UserEcho**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. <span data-ttu-id="36247-135">Az eredmények panelen válassza ki a **UserEcho**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="36247-135">In the results panel, select **UserEcho**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36247-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36247-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36247-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UserEcho.</span><span class="sxs-lookup"><span data-stu-id="36247-138">In this section, you configure and test Azure AD single sign-on with UserEcho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36247-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó UserEcho a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="36247-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UserEcho is to a user in Azure AD.</span></span> <span data-ttu-id="36247-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a UserEcho közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="36247-140">In other words, a link relationship between an Azure AD user and the related user in UserEcho needs to be established.</span></span>

<span data-ttu-id="36247-141">UserEcho, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="36247-141">In UserEcho, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="36247-142">Az Azure AD egyszeri bejelentkezést a UserEcho tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="36247-142">To configure and test Azure AD single sign-on with UserEcho, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="36247-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="36247-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="36247-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="36247-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36247-145">**[UserEcho tesztfelhasználó létrehozása](#creating-a-userecho-test-user)**  - való Britta Simon valami UserEcho, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="36247-145">**[Creating a UserEcho test user](#creating-a-userecho-test-user)** - to have a counterpart of Britta Simon in UserEcho that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="36247-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="36247-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36247-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="36247-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36247-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36247-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36247-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az UserEcho alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="36247-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UserEcho application.</span></span>

<span data-ttu-id="36247-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés UserEcho, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36247-150">**To configure Azure AD single sign-on with UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="36247-151">Az Azure portálon a a **UserEcho** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="36247-151">In the Azure portal, on the **UserEcho** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="36247-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="36247-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. <span data-ttu-id="36247-155">Az a **UserEcho tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="36247-155">On the **UserEcho Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    <span data-ttu-id="36247-157">a.</span><span class="sxs-lookup"><span data-stu-id="36247-157">a.</span></span> <span data-ttu-id="36247-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.userecho.com/`</span><span class="sxs-lookup"><span data-stu-id="36247-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.userecho.com/`</span></span>

    <span data-ttu-id="36247-159">b.</span><span class="sxs-lookup"><span data-stu-id="36247-159">b.</span></span> <span data-ttu-id="36247-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.userecho.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="36247-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.userecho.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="36247-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="36247-161">These values are not real.</span></span> <span data-ttu-id="36247-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="36247-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="36247-163">Ügyfél [UserEcho ügyfél-támogatási csoport](https://feedback.userecho.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="36247-163">Contact [UserEcho Client support team](https://feedback.userecho.com/) to get these values.</span></span> 

4. <span data-ttu-id="36247-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="36247-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. <span data-ttu-id="36247-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="36247-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="36247-168">A a **UserEcho konfigurációs** kattintson **konfigurálása UserEcho** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="36247-168">On the **UserEcho Configuration** section, click **Configure UserEcho** to open **Configure sign-on** window.</span></span> <span data-ttu-id="36247-169">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="36247-169">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. <span data-ttu-id="36247-171">Egy másik böngészőablakban jelentkezzen be a UserEcho vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="36247-171">In another browser window, sign on to your UserEcho company site as an administrator.</span></span>

8. <span data-ttu-id="36247-172">Az a felső eszköztáron kattintson a felhasználónevére, bontsa ki a menüt, és kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="36247-172">In the toolbar on the top, click your user name to expand the menu, and then click **Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. <span data-ttu-id="36247-174">Kattintson a **integrációja**.</span><span class="sxs-lookup"><span data-stu-id="36247-174">Click **Integrations**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. <span data-ttu-id="36247-176">Kattintson a **webhely**, és kattintson a **egyszeri bejelentkezés (egy SAML2)**.</span><span class="sxs-lookup"><span data-stu-id="36247-176">Click **Website**, and then click **Single sign-on (SAML2)**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. <span data-ttu-id="36247-178">Az a **egyszeri bejelentkezés (SAML)** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="36247-178">On the **Single sign-on (SAML)** page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    <span data-ttu-id="36247-180">a.</span><span class="sxs-lookup"><span data-stu-id="36247-180">a.</span></span> <span data-ttu-id="36247-181">Mint **SAML-kompatibilis**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="36247-181">As **SAML-enabled**, select **Yes**.</span></span>
    
    <span data-ttu-id="36247-182">b.</span><span class="sxs-lookup"><span data-stu-id="36247-182">b.</span></span> <span data-ttu-id="36247-183">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **SAML SSO URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="36247-183">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **SAML SSO URL** textbox.</span></span>
    
    <span data-ttu-id="36247-184">c.</span><span class="sxs-lookup"><span data-stu-id="36247-184">c.</span></span> <span data-ttu-id="36247-185">Beillesztés **Sign-Out URL-cím**, amely az Azure-portálról másolta a **távoli logoout URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="36247-185">Paste **Sign-Out URL**, which you have copied from the Azure portal into the **Remote logoout URL** textbox.</span></span>
    
    <span data-ttu-id="36247-186">d.</span><span class="sxs-lookup"><span data-stu-id="36247-186">d.</span></span> <span data-ttu-id="36247-187">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="36247-187">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **X.509 Certificate** textbox.</span></span>
    
    <span data-ttu-id="36247-188">e.</span><span class="sxs-lookup"><span data-stu-id="36247-188">e.</span></span> <span data-ttu-id="36247-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="36247-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="36247-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="36247-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="36247-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="36247-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="36247-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="36247-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36247-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36247-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="36247-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="36247-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="36247-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36247-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="36247-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36247-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36247-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="36247-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36247-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="36247-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36247-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="36247-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36247-205">a.</span><span class="sxs-lookup"><span data-stu-id="36247-205">a.</span></span> <span data-ttu-id="36247-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36247-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36247-207">b.</span><span class="sxs-lookup"><span data-stu-id="36247-207">b.</span></span> <span data-ttu-id="36247-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36247-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36247-209">c.</span><span class="sxs-lookup"><span data-stu-id="36247-209">c.</span></span> <span data-ttu-id="36247-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="36247-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="36247-211">d.</span><span class="sxs-lookup"><span data-stu-id="36247-211">d.</span></span> <span data-ttu-id="36247-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="36247-212">Click **Create**.</span></span>
 
### <a name="creating-a-userecho-test-user"></a><span data-ttu-id="36247-213">UserEcho tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36247-213">Creating a UserEcho test user</span></span>

<span data-ttu-id="36247-214">Ez a szakasz célja UserEcho Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="36247-214">The objective of this section is to create a user called Britta Simon in UserEcho.</span></span>

<span data-ttu-id="36247-215">**A felhasználó Britta Simon meghívta UserEcho létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36247-215">**To create a user called Britta Simon in UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="36247-216">Bejelentkezés a UserEcho vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="36247-216">Sign-on to your UserEcho company site as an administrator.</span></span>

2. <span data-ttu-id="36247-217">Az a felső eszköztáron kattintson a felhasználónevére, bontsa ki a menüt, és kattintson **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="36247-217">In the toolbar on the top, click your user name to expand the menu, and then click **Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. <span data-ttu-id="36247-219">Kattintson a **felhasználók**, bontsa ki a **felhasználók** szakasz.</span><span class="sxs-lookup"><span data-stu-id="36247-219">Click **Users**, to expand the **Users** section.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. <span data-ttu-id="36247-221">Kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="36247-221">Click **Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. <span data-ttu-id="36247-223">Kattintson a **új felhasználó meghívása**.</span><span class="sxs-lookup"><span data-stu-id="36247-223">Click **Invite a new user**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. <span data-ttu-id="36247-225">Az a **új felhasználó meghívása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="36247-225">On the **Invite a new user** dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    <span data-ttu-id="36247-227">a.</span><span class="sxs-lookup"><span data-stu-id="36247-227">a.</span></span> <span data-ttu-id="36247-228">Az a **neve** szövegmezőhöz Britta Simon például a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="36247-228">In the **Name** textbox, type name of the user like Britta Simon.</span></span>
    
    <span data-ttu-id="36247-229">b.</span><span class="sxs-lookup"><span data-stu-id="36247-229">b.</span></span>  <span data-ttu-id="36247-230">Az a **E-mail** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="36247-230">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="36247-231">c.</span><span class="sxs-lookup"><span data-stu-id="36247-231">c.</span></span> <span data-ttu-id="36247-232">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="36247-232">Click **Invite**.</span></span>

<span data-ttu-id="36247-233">A rendszer meghívót küld Britta, amely lehetővé teszi, hogy UserEcho indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="36247-233">An invitation is sent to Britta, which enables her to start using UserEcho.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="36247-234">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="36247-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="36247-235">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés UserEcho Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="36247-235">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UserEcho.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="36247-237">**Britta Simon hozzárendelése UserEcho, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36247-237">**To assign Britta Simon to UserEcho, perform the following steps:**</span></span>

1. <span data-ttu-id="36247-238">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36247-238">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="36247-240">Az alkalmazások listában válassza ki a **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="36247-240">In the applications list, select **UserEcho**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. <span data-ttu-id="36247-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="36247-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="36247-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="36247-244">Click **Add** button.</span></span> <span data-ttu-id="36247-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36247-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="36247-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="36247-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="36247-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36247-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36247-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36247-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="36247-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="36247-250">Testing single sign-on</span></span>

<span data-ttu-id="36247-251">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="36247-251">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="36247-252">Ha a hozzáférési panelen UserEcho csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az UserEcho alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="36247-252">When you click the UserEcho tile in the Access Panel, you should get automatically signed-on to your UserEcho application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36247-253">További források</span><span class="sxs-lookup"><span data-stu-id="36247-253">Additional resources</span></span>

* [<span data-ttu-id="36247-254">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="36247-254">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36247-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="36247-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

