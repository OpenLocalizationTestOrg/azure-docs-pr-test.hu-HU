---
title: "Oktatóanyag: Azure Active Directoryval integrált Freshservice |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Freshservice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d32775fa91d3a49da1ef55e57d1d38990fa09346
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a><span data-ttu-id="9931f-103">Oktatóanyag: Azure Active Directoryval integrált Freshservice</span><span class="sxs-lookup"><span data-stu-id="9931f-103">Tutorial: Azure Active Directory integration with Freshservice</span></span>

<span data-ttu-id="9931f-104">Ebben az oktatóanyagban elsajátíthatja Freshservice integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9931f-104">In this tutorial, you learn how to integrate Freshservice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9931f-105">Freshservice integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9931f-105">Integrating Freshservice with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9931f-106">Megadhatja a Freshservice hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9931f-106">You can control in Azure AD who has access to Freshservice</span></span>
- <span data-ttu-id="9931f-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Freshservice (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9931f-107">You can enable your users to automatically get signed-on to Freshservice (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9931f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9931f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9931f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9931f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9931f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9931f-110">Prerequisites</span></span>

<span data-ttu-id="9931f-111">Konfigurálása az Azure AD-integrációs Freshservice, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9931f-111">To configure Azure AD integration with Freshservice, you need the following items:</span></span>

- <span data-ttu-id="9931f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9931f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9931f-113">Egy Freshservice egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9931f-113">A Freshservice single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9931f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9931f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9931f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9931f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9931f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9931f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9931f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9931f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9931f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9931f-118">Scenario description</span></span>
<span data-ttu-id="9931f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9931f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9931f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9931f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9931f-121">A gyűjteményből Freshservice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9931f-121">Adding Freshservice from the gallery</span></span>
2. <span data-ttu-id="9931f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9931f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshservice-from-the-gallery"></a><span data-ttu-id="9931f-123">A gyűjteményből Freshservice hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9931f-123">Adding Freshservice from the gallery</span></span>
<span data-ttu-id="9931f-124">Az Azure AD integrálása a Freshservice konfigurálásához kell hozzáadnia Freshservice a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9931f-124">To configure the integration of Freshservice into Azure AD, you need to add Freshservice from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9931f-125">**A gyűjteményből Freshservice hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9931f-125">**To add Freshservice from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9931f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9931f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9931f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9931f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9931f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9931f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9931f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9931f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9931f-133">Írja be a keresőmezőbe, **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="9931f-133">In the search box, type **Freshservice**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. <span data-ttu-id="9931f-135">Az eredmények panelen válassza ki a **Freshservice**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9931f-135">In the results panel, select **Freshservice**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9931f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9931f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9931f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Freshservice.</span><span class="sxs-lookup"><span data-stu-id="9931f-138">In this section, you configure and test Azure AD single sign-on with Freshservice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9931f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Freshservice a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9931f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Freshservice is to a user in Azure AD.</span></span> <span data-ttu-id="9931f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Freshservice közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9931f-140">In other words, a link relationship between an Azure AD user and the related user in Freshservice needs to be established.</span></span>

<span data-ttu-id="9931f-141">Freshservice, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9931f-141">In Freshservice, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9931f-142">Az Azure AD egyszeri bejelentkezést a Freshservice tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9931f-142">To configure and test Azure AD single sign-on with Freshservice, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9931f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9931f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9931f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9931f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9931f-145">**[Freshservice tesztfelhasználó létrehozása](#creating-a-freshservice-test-user)**  - való Britta Simon valami Freshservice, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9931f-145">**[Creating a Freshservice test user](#creating-a-freshservice-test-user)** - to have a counterpart of Britta Simon in Freshservice that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9931f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9931f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9931f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9931f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9931f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9931f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9931f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Freshservice alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9931f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Freshservice application.</span></span>

<span data-ttu-id="9931f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Freshservice, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9931f-150">**To configure Azure AD single sign-on with Freshservice, perform the following steps:**</span></span>

1. <span data-ttu-id="9931f-151">Az Azure portálon a a **Freshservice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9931f-151">In the Azure portal, on the **Freshservice** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9931f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9931f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. <span data-ttu-id="9931f-155">Az a **Freshservice tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9931f-155">On the **Freshservice Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    <span data-ttu-id="9931f-157">a.</span><span class="sxs-lookup"><span data-stu-id="9931f-157">a.</span></span> <span data-ttu-id="9931f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="9931f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<democompany>.freshservice.com`</span></span>

    <span data-ttu-id="9931f-159">b.</span><span class="sxs-lookup"><span data-stu-id="9931f-159">b.</span></span> <span data-ttu-id="9931f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="9931f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<democompany>.freshservice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9931f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9931f-161">These values are not real.</span></span> <span data-ttu-id="9931f-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9931f-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9931f-163">Ügyfél [Freshservice ügyfél-támogatási csoport](https://support.freshservice.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9931f-163">Contact [Freshservice Client support team](https://support.freshservice.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="9931f-164">A a **SAML-aláíró tanúsítványa** területen másolása **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="9931f-164">On the **SAML Signing Certificate** section, copy **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. <span data-ttu-id="9931f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9931f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9931f-168">A a **Freshservice konfigurációs** kattintson **konfigurálása Freshservice** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9931f-168">On the **Freshservice Configuration** section, click **Configure Freshservice** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9931f-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9931f-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. <span data-ttu-id="9931f-171">Egy másik webes böngészőablakban jelentkezzen be a Freshservice vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9931f-171">In a different web browser window, log in to your Freshservice company site as an administrator.</span></span>

8. <span data-ttu-id="9931f-172">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="9931f-172">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="9931f-173">![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="9931f-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

9. <span data-ttu-id="9931f-174">Az a **Ügyfélportálra**, kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="9931f-174">In the **Customer Portal**, click **Security**.</span></span>
   
    <span data-ttu-id="9931f-175">![Biztonsági](./media/active-directory-saas-freshservice-tutorial/ic790815.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="9931f-175">![Security](./media/active-directory-saas-freshservice-tutorial/ic790815.png "Security")</span></span>

10. <span data-ttu-id="9931f-176">Az a **biztonsági** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9931f-176">In the **Security** section, perform the following steps:</span></span>
   
    <span data-ttu-id="9931f-177">![Egyszeri bejelentkezés](./media/active-directory-saas-freshservice-tutorial/ic790816.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="9931f-177">![Single Sign On](./media/active-directory-saas-freshservice-tutorial/ic790816.png "Single Sign On")</span></span>
   
    <span data-ttu-id="9931f-178">a.</span><span class="sxs-lookup"><span data-stu-id="9931f-178">a.</span></span> <span data-ttu-id="9931f-179">Kapcsoló **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9931f-179">Switch **Single Sign On**.</span></span>

    <span data-ttu-id="9931f-180">b.</span><span class="sxs-lookup"><span data-stu-id="9931f-180">b.</span></span> <span data-ttu-id="9931f-181">Válassza ki **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="9931f-181">Select **SAML SSO**.</span></span>

    <span data-ttu-id="9931f-182">c.</span><span class="sxs-lookup"><span data-stu-id="9931f-182">c.</span></span> <span data-ttu-id="9931f-183">Az a **SAML bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9931f-183">In the **SAML Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9931f-184">d.</span><span class="sxs-lookup"><span data-stu-id="9931f-184">d.</span></span> <span data-ttu-id="9931f-185">Az a **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9931f-185">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9931f-186">e.</span><span class="sxs-lookup"><span data-stu-id="9931f-186">e.</span></span> <span data-ttu-id="9931f-187">A **biztonsági tanúsítvány-ujjlenyomat** szövegmező, illessze be a **UJJLENYOMAT** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9931f-187">In **Security Certificate Fingerprint** textbox, paste the **THUMBPRINT** value of certificate which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9931f-188">f.</span><span class="sxs-lookup"><span data-stu-id="9931f-188">f.</span></span> <span data-ttu-id="9931f-189">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="9931f-189">Click **Save**</span></span>
   
> [!TIP]
> <span data-ttu-id="9931f-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9931f-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9931f-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9931f-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9931f-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9931f-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9931f-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9931f-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="9931f-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9931f-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9931f-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9931f-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9931f-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9931f-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9931f-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9931f-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9931f-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9931f-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9931f-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9931f-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9931f-205">a.</span><span class="sxs-lookup"><span data-stu-id="9931f-205">a.</span></span> <span data-ttu-id="9931f-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9931f-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9931f-207">b.</span><span class="sxs-lookup"><span data-stu-id="9931f-207">b.</span></span> <span data-ttu-id="9931f-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9931f-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9931f-209">c.</span><span class="sxs-lookup"><span data-stu-id="9931f-209">c.</span></span> <span data-ttu-id="9931f-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9931f-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9931f-211">d.</span><span class="sxs-lookup"><span data-stu-id="9931f-211">d.</span></span> <span data-ttu-id="9931f-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9931f-212">Click **Create**.</span></span>
 
### <a name="creating-a-freshservice-test-user"></a><span data-ttu-id="9931f-213">Freshservice tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9931f-213">Creating a Freshservice test user</span></span>

<span data-ttu-id="9931f-214">Ahhoz, hogy az Azure AD-felhasználók FreshService bejelentkezni, akkor ki kell építenie a FreshService.</span><span class="sxs-lookup"><span data-stu-id="9931f-214">To enable Azure AD users to log in to FreshService, they must be provisioned into FreshService.</span></span> <span data-ttu-id="9931f-215">FreshService, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9931f-215">In the case of FreshService, provisioning is a manual task.</span></span>

<span data-ttu-id="9931f-216">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9931f-216">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="9931f-217">Jelentkezzen be a **FreshService** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9931f-217">Log in to your **FreshService** company site as an administrator.</span></span>

2. <span data-ttu-id="9931f-218">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="9931f-218">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="9931f-219">![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="9931f-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

3. <span data-ttu-id="9931f-220">Az a **felhasználókezelés** kattintson **engedélyezését**.</span><span class="sxs-lookup"><span data-stu-id="9931f-220">In the **User Management** section, click **Requesters**.</span></span>
   
    <span data-ttu-id="9931f-221">![Engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790818.png "engedélyezését")</span><span class="sxs-lookup"><span data-stu-id="9931f-221">![Requesters](./media/active-directory-saas-freshservice-tutorial/ic790818.png "Requesters")</span></span>

4. <span data-ttu-id="9931f-222">Kattintson a **új kérelmező**.</span><span class="sxs-lookup"><span data-stu-id="9931f-222">Click **New Requester**.</span></span>
   
    <span data-ttu-id="9931f-223">![Új engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790819.png "új engedélyezését")</span><span class="sxs-lookup"><span data-stu-id="9931f-223">![New Requesters](./media/active-directory-saas-freshservice-tutorial/ic790819.png "New Requesters")</span></span>

5. <span data-ttu-id="9931f-224">Az a **új kérelmező** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9931f-224">In the **New Requester** section, perform the following steps:</span></span>
   
    <span data-ttu-id="9931f-225">![Új kérelmező](./media/active-directory-saas-freshservice-tutorial/ic790820.png "új kérelmező")</span><span class="sxs-lookup"><span data-stu-id="9931f-225">![New Requester](./media/active-directory-saas-freshservice-tutorial/ic790820.png "New Requester")</span></span>   

    <span data-ttu-id="9931f-226">a.</span><span class="sxs-lookup"><span data-stu-id="9931f-226">a.</span></span> <span data-ttu-id="9931f-227">Adja meg a **Utónév** és **E-mail** egy érvényes Azure Active Directory-fiókot szeretné azokat a kapcsolódó szövegmezők rendelkezés attribútumait.</span><span class="sxs-lookup"><span data-stu-id="9931f-227">Enter the **First Name** and **Email** attributes of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="9931f-228">b.</span><span class="sxs-lookup"><span data-stu-id="9931f-228">b.</span></span> <span data-ttu-id="9931f-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9931f-229">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="9931f-230">Az Azure Active Directory fióktulajdonos kap egy e-mailt, mielőtt aktívvá válik, győződjön meg arról, hogy a fiók hivatkozással</span><span class="sxs-lookup"><span data-stu-id="9931f-230">The Azure Active Directory account holder gets an email including a link to confirm the account before it becomes active</span></span>
    >  

>[!NOTE]
><span data-ttu-id="9931f-231">Bármely más FreshService felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz FreshService által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="9931f-231">You can use any other FreshService user account creation tools or APIs provided by FreshService to provision AAD user accounts.</span></span>
>  

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9931f-233">**Britta Simon hozzárendelése Freshservice, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9931f-233">**To assign Britta Simon to Freshservice, perform the following steps:**</span></span>

1. <span data-ttu-id="9931f-234">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9931f-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9931f-236">Az alkalmazások listában válassza ki a **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="9931f-236">In the applications list, select **Freshservice**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. <span data-ttu-id="9931f-238">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9931f-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9931f-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9931f-240">Click **Add** button.</span></span> <span data-ttu-id="9931f-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9931f-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9931f-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9931f-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9931f-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9931f-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9931f-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9931f-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9931f-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9931f-246">Testing single sign-on</span></span>

<span data-ttu-id="9931f-247">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="9931f-247">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9931f-248">Ha a hozzáférési panelen Freshservice csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Freshservice alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9931f-248">When you click the Freshservice tile in the Access Panel, you should get automatically signed-on to your Freshservice application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9931f-249">További források</span><span class="sxs-lookup"><span data-stu-id="9931f-249">Additional resources</span></span>

* [<span data-ttu-id="9931f-250">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9931f-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9931f-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9931f-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

