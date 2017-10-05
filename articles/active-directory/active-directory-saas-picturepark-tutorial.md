---
title: "Oktatóanyag: Azure Active Directoryval integrált Picturepark |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Picturepark között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 1c009aa1fdd3140a4466cf762b6c9687e74ce4c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="923c4-103">Oktatóanyag: Azure Active Directoryval integrált Picturepark</span><span class="sxs-lookup"><span data-stu-id="923c4-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="923c4-104">Ebben az oktatóanyagban elsajátíthatja Picturepark integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="923c4-104">In this tutorial, you learn how to integrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="923c4-105">Picturepark integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="923c4-105">Integrating Picturepark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="923c4-106">Megadhatja a Picturepark hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="923c4-106">You can control in Azure AD who has access to Picturepark</span></span>
- <span data-ttu-id="923c4-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Picturepark (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="923c4-107">You can enable your users to automatically get signed-on to Picturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="923c4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="923c4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="923c4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="923c4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="923c4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="923c4-110">Prerequisites</span></span>

<span data-ttu-id="923c4-111">Konfigurálása az Azure AD-integrációs Picturepark, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="923c4-111">To configure Azure AD integration with Picturepark, you need the following items:</span></span>

- <span data-ttu-id="923c4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="923c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="923c4-113">Egy Picturepark egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="923c4-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="923c4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="923c4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="923c4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="923c4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="923c4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="923c4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="923c4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="923c4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="923c4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="923c4-118">Scenario description</span></span>
<span data-ttu-id="923c4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="923c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="923c4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="923c4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="923c4-121">A gyűjteményből Picturepark hozzáadása</span><span class="sxs-lookup"><span data-stu-id="923c4-121">Adding Picturepark from the gallery</span></span>
2. <span data-ttu-id="923c4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="923c4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-the-gallery"></a><span data-ttu-id="923c4-123">A gyűjteményből Picturepark hozzáadása</span><span class="sxs-lookup"><span data-stu-id="923c4-123">Adding Picturepark from the gallery</span></span>
<span data-ttu-id="923c4-124">Az Azure AD integrálása a Picturepark konfigurálásához kell hozzáadnia Picturepark a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="923c4-124">To configure the integration of Picturepark into Azure AD, you need to add Picturepark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="923c4-125">**A gyűjteményből Picturepark hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="923c4-125">**To add Picturepark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="923c4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="923c4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="923c4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="923c4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="923c4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="923c4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="923c4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="923c4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="923c4-133">Írja be a keresőmezőbe, **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="923c4-133">In the search box, type **Picturepark**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="923c4-135">Az eredmények panelen válassza ki a **Picturepark**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="923c4-135">In the results panel, select **Picturepark**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="923c4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="923c4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="923c4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Picturepark.</span><span class="sxs-lookup"><span data-stu-id="923c4-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="923c4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Picturepark a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="923c4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Picturepark is to a user in Azure AD.</span></span> <span data-ttu-id="923c4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Picturepark közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="923c4-140">In other words, a link relationship between an Azure AD user and the related user in Picturepark needs to be established.</span></span>

<span data-ttu-id="923c4-141">Picturepark, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="923c4-141">In Picturepark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="923c4-142">Az Azure AD egyszeri bejelentkezést a Picturepark tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="923c4-142">To configure and test Azure AD single sign-on with Picturepark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="923c4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="923c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="923c4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="923c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="923c4-145">**[Picturepark tesztfelhasználó létrehozása](#creating-a-picturepark-test-user)**  - való Britta Simon valami Picturepark, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="923c4-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - to have a counterpart of Britta Simon in Picturepark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="923c4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="923c4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="923c4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="923c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="923c4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="923c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="923c4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Picturepark alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="923c4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="923c4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Picturepark, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="923c4-150">**To configure Azure AD single sign-on with Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="923c4-151">Az Azure portálon a a **Picturepark** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="923c4-151">In the Azure portal, on the **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="923c4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="923c4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="923c4-155">Az a **Picturepark tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="923c4-155">On the **Picturepark Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="923c4-157">a.</span><span class="sxs-lookup"><span data-stu-id="923c4-157">a.</span></span> <span data-ttu-id="923c4-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="923c4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="923c4-159">b.</span><span class="sxs-lookup"><span data-stu-id="923c4-159">b.</span></span> <span data-ttu-id="923c4-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="923c4-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="923c4-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="923c4-161">These values are not real.</span></span> <span data-ttu-id="923c4-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="923c4-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="923c4-163">Ügyfél [Picturepark ügyfél-támogatási csoport](https://picturepark.com/about/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="923c4-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="923c4-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="923c4-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="923c4-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="923c4-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="923c4-168">A a **Picturepark konfigurációs** kattintson **konfigurálása Picturepark** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="923c4-168">On the **Picturepark Configuration** section, click **Configure Picturepark** to open **Configure sign-on** window.</span></span> <span data-ttu-id="923c4-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="923c4-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="923c4-171">Egy másik webes böngészőablakban jelentkezzen be a Picturepark vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="923c4-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="923c4-172">A felső eszköztáron kattintson **felügyeleti eszközök**, és kattintson a **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="923c4-172">In the toolbar on the top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="923c4-173">![Felügyeleti konzol](./media/active-directory-saas-picturepark-tutorial/ic795062.png "felügyeleti konzol")</span><span class="sxs-lookup"><span data-stu-id="923c4-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="923c4-174">Kattintson a **hitelesítési**, és kattintson a **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="923c4-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="923c4-175">![Hitelesítési](./media/active-directory-saas-picturepark-tutorial/ic795063.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="923c4-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="923c4-176">Az a **identitás szolgáltató konfigurálása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="923c4-176">In the **Identity provider configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="923c4-177">![Szolgáltató konfigurálása identitás](./media/active-directory-saas-picturepark-tutorial/ic795064.png "identitás szolgáltató konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="923c4-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="923c4-178">a.</span><span class="sxs-lookup"><span data-stu-id="923c4-178">a.</span></span> <span data-ttu-id="923c4-179">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="923c4-179">Click **Add**.</span></span>
  
    <span data-ttu-id="923c4-180">b.</span><span class="sxs-lookup"><span data-stu-id="923c4-180">b.</span></span> <span data-ttu-id="923c4-181">Írja be a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="923c4-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="923c4-182">c.</span><span class="sxs-lookup"><span data-stu-id="923c4-182">c.</span></span> <span data-ttu-id="923c4-183">Válassza ki **alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="923c4-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="923c4-184">d.</span><span class="sxs-lookup"><span data-stu-id="923c4-184">d.</span></span> <span data-ttu-id="923c4-185">A **Issuer URI** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="923c4-185">In **Issuer URI** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="923c4-186">e.</span><span class="sxs-lookup"><span data-stu-id="923c4-186">e.</span></span> <span data-ttu-id="923c4-187">A **megbízható kibocsátó görgetőgomb nyomtatás** szövegmezőhöz illessze be az értékét **ujjlenyomat** , amely a másolt **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="923c4-187">In **Trusted Issuer Thumb Print** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="923c4-188">Kattintson a **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="923c4-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="923c4-189">Beállítása a **Emailaddress** attribútumnak a **jogcím** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="923c4-189">To set the **Emailaddress** attribute in the **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="923c4-190">![Konfigurációs](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="923c4-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="923c4-191">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="923c4-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="923c4-192">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="923c4-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="923c4-193">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="923c4-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="923c4-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="923c4-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="923c4-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="923c4-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="923c4-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="923c4-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="923c4-198">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="923c4-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="923c4-200">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="923c4-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="923c4-202">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="923c4-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="923c4-204">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="923c4-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="923c4-206">a.</span><span class="sxs-lookup"><span data-stu-id="923c4-206">a.</span></span> <span data-ttu-id="923c4-207">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="923c4-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="923c4-208">b.</span><span class="sxs-lookup"><span data-stu-id="923c4-208">b.</span></span> <span data-ttu-id="923c4-209">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="923c4-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="923c4-210">c.</span><span class="sxs-lookup"><span data-stu-id="923c4-210">c.</span></span> <span data-ttu-id="923c4-211">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="923c4-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="923c4-212">d.</span><span class="sxs-lookup"><span data-stu-id="923c4-212">d.</span></span> <span data-ttu-id="923c4-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="923c4-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="923c4-214">Picturepark tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="923c4-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="923c4-215">Ahhoz, hogy az Azure AD-felhasználók Picturepark bejelentkezni, akkor ki kell építenie Picturepark be.</span><span class="sxs-lookup"><span data-stu-id="923c4-215">In order to enable Azure AD users to log into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="923c4-216">Picturepark, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="923c4-216">In the case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="923c4-217">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="923c4-217">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="923c4-218">Jelentkezzen be a **Picturepark** bérlő.</span><span class="sxs-lookup"><span data-stu-id="923c4-218">Log in to your **Picturepark** tenant.</span></span>

2. <span data-ttu-id="923c4-219">A felső eszköztáron kattintson **felügyeleti eszközök**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="923c4-219">In the toolbar on the top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="923c4-220">![Felhasználók](./media/active-directory-saas-picturepark-tutorial/ic795067.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="923c4-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="923c4-221">Az a **felhasználók áttekintése** lapra, majd **új**.</span><span class="sxs-lookup"><span data-stu-id="923c4-221">In the **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="923c4-222">![Felhasználókezelés](./media/active-directory-saas-picturepark-tutorial/ic795068.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="923c4-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="923c4-223">Az a **felhasználó létrehozása** párbeszédpanelen a következő lépésekkel a egy érvényes Azure Active Directory-felhasználó azt szeretné, hogy építse ki:</span><span class="sxs-lookup"><span data-stu-id="923c4-223">On the **Create User** dialog, perform the following steps of a valid Azure Active Directory User you want to provision:</span></span>
   
    <span data-ttu-id="923c4-224">![Hozzon létre felhasználói](./media/active-directory-saas-picturepark-tutorial/ic795069.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="923c4-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="923c4-225">a.</span><span class="sxs-lookup"><span data-stu-id="923c4-225">a.</span></span> <span data-ttu-id="923c4-226">Az a **E-mail cím** szövegmezőhöz típusa a **e-mail cím** felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="923c4-226">In the **Email Address** textbox, type the **email address** of the user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="923c4-227">b.</span><span class="sxs-lookup"><span data-stu-id="923c4-227">b.</span></span> <span data-ttu-id="923c4-228">Az a **jelszó** és **jelszó megerősítése** szövegmezőből, típusa a **jelszó** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="923c4-228">In the **Password** and **Confirm Password** textboxes, type the **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="923c4-229">c.</span><span class="sxs-lookup"><span data-stu-id="923c4-229">c.</span></span> <span data-ttu-id="923c4-230">Az a **Utónév** szövegmezőhöz típusa a **Utónév** felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="923c4-230">In the **First Name** textbox, type the **First Name** of the user **Britta**.</span></span> 
   
    <span data-ttu-id="923c4-231">d.</span><span class="sxs-lookup"><span data-stu-id="923c4-231">d.</span></span> <span data-ttu-id="923c4-232">Az a **Vezetéknév** szövegmezőhöz típusa a **Vezetéknév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="923c4-232">In the **Last Name** textbox, type the **Last Name** of the user **Simon**.</span></span>
   
    <span data-ttu-id="923c4-233">e.</span><span class="sxs-lookup"><span data-stu-id="923c4-233">e.</span></span> <span data-ttu-id="923c4-234">Az a **vállalati** szövegmezőhöz típusa a **vállalatnév** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="923c4-234">In the **Company** textbox, type the **Company name** of the user.</span></span> 
   
    <span data-ttu-id="923c4-235">f.</span><span class="sxs-lookup"><span data-stu-id="923c4-235">f.</span></span> <span data-ttu-id="923c4-236">Az a **ország** szövegmező, jelölje be a **ország** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="923c4-236">In the **Country** textbox, select the **Country** of the user.</span></span>
  
    <span data-ttu-id="923c4-237">g.</span><span class="sxs-lookup"><span data-stu-id="923c4-237">g.</span></span> <span data-ttu-id="923c4-238">Az a **ZIP-** szövegmezőhöz típusa a **irányítószám** a város.</span><span class="sxs-lookup"><span data-stu-id="923c4-238">In the **ZIP** textbox, type the **ZIP code** of the city.</span></span>
   
    <span data-ttu-id="923c4-239">h.</span><span class="sxs-lookup"><span data-stu-id="923c4-239">h.</span></span> <span data-ttu-id="923c4-240">Az a **Város** szövegmezőhöz típusa a **városnév** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="923c4-240">In the **City** textbox, type the **City name** of the user.</span></span>

    <span data-ttu-id="923c4-241">i.</span><span class="sxs-lookup"><span data-stu-id="923c4-241">i.</span></span> <span data-ttu-id="923c4-242">Válassza ki a **nyelvi**.</span><span class="sxs-lookup"><span data-stu-id="923c4-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="923c4-243">j.</span><span class="sxs-lookup"><span data-stu-id="923c4-243">j.</span></span> <span data-ttu-id="923c4-244">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="923c4-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="923c4-245">Bármely más Picturepark felhasználói fiók létrehozása eszközök vagy Picturepark kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="923c4-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="923c4-246">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="923c4-246">Assigning the Azure AD test user</span></span>

<span data-ttu-id="923c4-247">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Picturepark Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="923c4-247">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Picturepark.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="923c4-249">**Britta Simon hozzárendelése Picturepark, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="923c4-249">**To assign Britta Simon to Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="923c4-250">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="923c4-250">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="923c4-252">Az alkalmazások listában válassza ki a **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="923c4-252">In the applications list, select **Picturepark**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="923c4-254">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="923c4-254">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="923c4-256">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="923c4-256">Click **Add** button.</span></span> <span data-ttu-id="923c4-257">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="923c4-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="923c4-259">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="923c4-259">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="923c4-260">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="923c4-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="923c4-261">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="923c4-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="923c4-262">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="923c4-262">Testing single sign-on</span></span>

<span data-ttu-id="923c4-263">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="923c4-263">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="923c4-264">Ha a hozzáférési panelen Picturepark csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Picturepark alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="923c4-264">When you click the Picturepark tile in the Access Panel, you should get automatically signed-on to your Picturepark application.</span></span> <span data-ttu-id="923c4-265">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="923c4-265">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="923c4-266">További források</span><span class="sxs-lookup"><span data-stu-id="923c4-266">Additional resources</span></span>

* [<span data-ttu-id="923c4-267">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="923c4-267">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="923c4-268">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="923c4-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

