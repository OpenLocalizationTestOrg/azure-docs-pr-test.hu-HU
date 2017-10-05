---
title: "Oktatóanyag: Azure Active Directory-integráció a Mozy vállalati |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Mozy vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: ac73aadcb8205f24f9d2dbce5af76f53bbcb9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="7c358-103">Oktatóanyag: Azure Active Directory-integráció a Mozy vállalati</span><span class="sxs-lookup"><span data-stu-id="7c358-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="7c358-104">Ebben az oktatóanyagban elsajátíthatja Mozy vállalati integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c358-104">In this tutorial, you learn how to integrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c358-105">Mozy vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7c358-105">Integrating Mozy Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7c358-106">Megadhatja a Mozy vállalati hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7c358-106">You can control in Azure AD who has access to Mozy Enterprise</span></span>
- <span data-ttu-id="7c358-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Mozy vállalati (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="7c358-107">You can enable your users to automatically get signed-on to Mozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c358-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7c358-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7c358-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c358-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c358-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7c358-110">Prerequisites</span></span>

<span data-ttu-id="7c358-111">Az Azure AD-integráció konfigurálása a Mozy vállalati, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7c358-111">To configure Azure AD integration with Mozy Enterprise, you need the following items:</span></span>

- <span data-ttu-id="7c358-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7c358-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c358-113">Egy Mozy vállalati egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7c358-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c358-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7c358-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c358-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7c358-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c358-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7c358-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c358-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c358-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c358-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7c358-118">Scenario description</span></span>
<span data-ttu-id="7c358-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7c358-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c358-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7c358-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c358-121">A gyűjteményből Mozy vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7c358-121">Adding Mozy Enterprise from the gallery</span></span>
2. <span data-ttu-id="7c358-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c358-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-the-gallery"></a><span data-ttu-id="7c358-123">A gyűjteményből Mozy vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7c358-123">Adding Mozy Enterprise from the gallery</span></span>
<span data-ttu-id="7c358-124">Az Azure AD integrálása a Mozy Enterprise konfigurálásához kell hozzáadnia Mozy vállalati a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7c358-124">To configure the integration of Mozy Enterprise into Azure AD, you need to add Mozy Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7c358-125">**A gyűjteményből Mozy vállalati hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c358-125">**To add Mozy Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7c358-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c358-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c358-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7c358-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7c358-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c358-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7c358-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="7c358-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7c358-133">Írja be a keresőmezőbe, **Mozy vállalati**.</span><span class="sxs-lookup"><span data-stu-id="7c358-133">In the search box, type **Mozy Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="7c358-135">Az eredmények panelen válassza ki a **Mozy vállalati**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7c358-135">In the results panel, select **Mozy Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c358-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c358-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c358-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Mozy vállalati "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="7c358-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c358-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Mozy vállalati a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7c358-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mozy Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="7c358-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Mozy vállalat közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7c358-140">In other words, a link relationship between an Azure AD user and the related user in Mozy Enterprise needs to be established.</span></span>

<span data-ttu-id="7c358-141">Mozy vállalat, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7c358-141">In Mozy Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7c358-142">Az Azure AD egyszeri bejelentkezést a vállalati Mozy tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7c358-142">To configure and test Azure AD single sign-on with Mozy Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7c358-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7c358-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7c358-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7c358-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c358-145">**[Mozy vállalati tesztfelhasználó létrehozása](#creating-a-mozy-enterprise-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon Mozy vállalat, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7c358-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - to have a counterpart of Britta Simon in Mozy Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c358-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c358-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c358-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7c358-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c358-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c358-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c358-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Mozy vállalati alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7c358-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="7c358-150">**Az Azure AD egyszeri bejelentkezést a Mozy vállalati megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c358-150">**To configure Azure AD single sign-on with Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="7c358-151">Az Azure portálon a a **Mozy vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7c358-151">In the Azure portal, on the **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7c358-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c358-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="7c358-155">Az a **Mozy vállalati tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7c358-155">On the **Mozy Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="7c358-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="7c358-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7c358-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="7c358-158">This value is not real.</span></span> <span data-ttu-id="7c358-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="7c358-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="7c358-160">Ügyfél [Mozy vállalati ügyfél-támogatási csoport](http://support.mozy.com/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="7c358-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) to get this value.</span></span>

4. <span data-ttu-id="7c358-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c358-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="7c358-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c358-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7c358-165">A a **Mozy vállalati konfiguráció** kattintson **konfigurálása Mozy vállalati** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7c358-165">On the **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7c358-166">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7c358-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="7c358-168">Egy másik webes böngészőablakban jelentkezzen be a vállalati Mozy vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c358-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="7c358-169">Az a **konfigurációs** kattintson **hitelesítési házirend**.</span><span class="sxs-lookup"><span data-stu-id="7c358-169">In the **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="7c358-170">![Hitelesítési házirend](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "hitelesítési házirend")</span><span class="sxs-lookup"><span data-stu-id="7c358-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="7c358-171">Az a **hitelesítési házirend** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7c358-171">On the **Authentication Policy** section, perform the following steps:</span></span>
   
   <span data-ttu-id="7c358-172">![Hitelesítési házirend](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "hitelesítési házirend")</span><span class="sxs-lookup"><span data-stu-id="7c358-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="7c358-173">a.</span><span class="sxs-lookup"><span data-stu-id="7c358-173">a.</span></span> <span data-ttu-id="7c358-174">Válassza ki **címtárszolgáltatás** , **szolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="7c358-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="7c358-175">b.</span><span class="sxs-lookup"><span data-stu-id="7c358-175">b.</span></span> <span data-ttu-id="7c358-176">Válassza ki **LDAP leküldéses használja**.</span><span class="sxs-lookup"><span data-stu-id="7c358-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="7c358-177">c.</span><span class="sxs-lookup"><span data-stu-id="7c358-177">c.</span></span> <span data-ttu-id="7c358-178">Kattintson a **SAML-alapú hitelesítés** fülre.</span><span class="sxs-lookup"><span data-stu-id="7c358-178">Click the **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="7c358-179">d.</span><span class="sxs-lookup"><span data-stu-id="7c358-179">d.</span></span> <span data-ttu-id="7c358-180">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **hitelesítés URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c358-180">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="7c358-181">e.</span><span class="sxs-lookup"><span data-stu-id="7c358-181">e.</span></span> <span data-ttu-id="7c358-182">Beillesztés **SAML Entitásazonosító**, amely az Azure-portálról másolta a **SAML-végpont** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c358-182">Paste **SAML Entity ID**, which you have copied from the Azure portal into the **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="7c358-183">f.</span><span class="sxs-lookup"><span data-stu-id="7c358-183">f.</span></span> <span data-ttu-id="7c358-184">Nyissa meg a letöltött base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be a teljes tanúsítványt **SAML tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c358-184">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="7c358-185">g.</span><span class="sxs-lookup"><span data-stu-id="7c358-185">g.</span></span> <span data-ttu-id="7c358-186">Válassza ki **egyszeri bejelentkezés engedélyezése a rendszergazdák számára, hogy jelentkezzen be a hálózati hitelesítő adataik**.</span><span class="sxs-lookup"><span data-stu-id="7c358-186">Select **Enable SSO for Admins to log in with their network credentials**.</span></span>
   
   <span data-ttu-id="7c358-187">h.</span><span class="sxs-lookup"><span data-stu-id="7c358-187">h.</span></span> <span data-ttu-id="7c358-188">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="7c358-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="7c358-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7c358-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7c358-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7c358-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7c358-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c358-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c358-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c358-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c358-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7c358-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7c358-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c358-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7c358-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c358-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c358-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7c358-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c358-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7c358-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c358-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7c358-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c358-204">a.</span><span class="sxs-lookup"><span data-stu-id="7c358-204">a.</span></span> <span data-ttu-id="7c358-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c358-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c358-206">b.</span><span class="sxs-lookup"><span data-stu-id="7c358-206">b.</span></span> <span data-ttu-id="7c358-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c358-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c358-208">c.</span><span class="sxs-lookup"><span data-stu-id="7c358-208">c.</span></span> <span data-ttu-id="7c358-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7c358-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7c358-210">d.</span><span class="sxs-lookup"><span data-stu-id="7c358-210">d.</span></span> <span data-ttu-id="7c358-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7c358-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="7c358-212">Mozy vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c358-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="7c358-213">Ahhoz, hogy az Azure AD-felhasználók Mozy vállalati bejelentkezni, akkor ki kell építenie Mozy vállalat.</span><span class="sxs-lookup"><span data-stu-id="7c358-213">In order to enable Azure AD users to log into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="7c358-214">Mozy vállalati, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7c358-214">In the case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="7c358-215">Bármely más Mozy vállalati felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz Mozy vállalati által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="7c358-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise to provision AAD user accounts.</span></span>

<span data-ttu-id="7c358-216">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c358-216">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="7c358-217">Jelentkezzen be a **Mozy vállalati** bérlő.</span><span class="sxs-lookup"><span data-stu-id="7c358-217">Log in to your **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="7c358-218">Kattintson a **felhasználók**, és kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="7c358-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="7c358-219">![Felhasználók](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="7c358-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="7c358-220">A **új felhasználó hozzáadása** a beállítás csak akkor látható, ha csak **Mozy** van jelölve, a szolgáltató **hitelesítési házirend**.</span><span class="sxs-lookup"><span data-stu-id="7c358-220">The **Add New User** option is only displayed only if **Mozy** is selected as the provider under **Authentication policy**.</span></span> <span data-ttu-id="7c358-221">Ha SAML-alapú hitelesítés van konfigurálva, majd a felhasználót adnak hozzá automatikusan az egyszeri bejelentkezési keresztül az első bejelentkezés a.</span><span class="sxs-lookup"><span data-stu-id="7c358-221">If SAML Authentication is configured, then the users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="7c358-222">Az új felhasználói párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7c358-222">On the new user dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="7c358-223">![Felhasználók hozzáadása az](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="7c358-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="7c358-224">a.</span><span class="sxs-lookup"><span data-stu-id="7c358-224">a.</span></span> <span data-ttu-id="7c358-225">Az a **válasszon ki egy csoportot** listában, válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="7c358-225">From the **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="7c358-226">b.</span><span class="sxs-lookup"><span data-stu-id="7c358-226">b.</span></span> <span data-ttu-id="7c358-227">Az a **felhasználó milyen típusú** listára, válassza ki a típus.</span><span class="sxs-lookup"><span data-stu-id="7c358-227">From the **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="7c358-228">c.</span><span class="sxs-lookup"><span data-stu-id="7c358-228">c.</span></span> <span data-ttu-id="7c358-229">Az a **felhasználónév** szövegmező, írja be az Azure AD-felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="7c358-229">In the **Username** textbox, type the name of the Azure AD user.</span></span>
   
   <span data-ttu-id="7c358-230">d.</span><span class="sxs-lookup"><span data-stu-id="7c358-230">d.</span></span> <span data-ttu-id="7c358-231">Az a **E-mail** szövegmező, írja be az Azure AD-felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="7c358-231">In the **Email** textbox, type the email address of the Azure AD user.</span></span>
   
   <span data-ttu-id="7c358-232">e.</span><span class="sxs-lookup"><span data-stu-id="7c358-232">e.</span></span> <span data-ttu-id="7c358-233">Válassza ki **felhasználói utasítás e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="7c358-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="7c358-234">f.</span><span class="sxs-lookup"><span data-stu-id="7c358-234">f.</span></span> <span data-ttu-id="7c358-235">Kattintson a **felhasználó(k) hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="7c358-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="7c358-236">Miután létrehozta a felhasználó, egy e-mailt kapnak az Azure AD-felhasználó, mielőtt aktívvá válik, győződjön meg arról, hogy a fiók mutató hivatkozást tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="7c358-236">After creating the user, an email will be sent to the Azure AD user that includes a link to confirm the account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7c358-237">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7c358-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7c358-238">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Mozy vállalati Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7c358-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mozy Enterprise.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7c358-240">**Britta Simon hozzárendelése Mozy vállalati, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c358-240">**To assign Britta Simon to Mozy Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="7c358-241">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c358-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7c358-243">Az alkalmazások listában válassza ki a **Mozy vállalati**.</span><span class="sxs-lookup"><span data-stu-id="7c358-243">In the applications list, select **Mozy Enterprise**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="7c358-245">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7c358-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7c358-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c358-247">Click **Add** button.</span></span> <span data-ttu-id="7c358-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c358-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7c358-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7c358-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7c358-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c358-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c358-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c358-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c358-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7c358-253">Testing single sign-on</span></span>

<span data-ttu-id="7c358-254">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7c358-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7c358-255">Ha a hozzáférési panelen Mozy vállalati csempére kattint, Mozy vállalati alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7c358-255">When you click the Mozy Enterprise tile in the Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="7c358-256">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7c358-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c358-257">További források</span><span class="sxs-lookup"><span data-stu-id="7c358-257">Additional resources</span></span>

* [<span data-ttu-id="7c358-258">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7c358-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c358-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7c358-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

