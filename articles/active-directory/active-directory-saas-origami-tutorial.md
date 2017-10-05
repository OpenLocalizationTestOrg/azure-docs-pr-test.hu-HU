---
title: "Oktatóanyag: Azure Active Directoryval integrált Origami |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Origami között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3420409b72ff032e64ac59365083dd141dfc3c1b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a><span data-ttu-id="d867c-103">Oktatóanyag: Azure Active Directoryval integrált Origami</span><span class="sxs-lookup"><span data-stu-id="d867c-103">Tutorial: Azure Active Directory integration with Origami</span></span>

<span data-ttu-id="d867c-104">Ebben az oktatóanyagban elsajátíthatja Origami integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d867c-104">In this tutorial, you learn how to integrate Origami with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d867c-105">Origami integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d867c-105">Integrating Origami with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d867c-106">Megadhatja a Origami hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d867c-106">You can control in Azure AD who has access to Origami</span></span>
- <span data-ttu-id="d867c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Origami (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d867c-107">You can enable your users to automatically get signed-on to Origami (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d867c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d867c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d867c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d867c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d867c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d867c-110">Prerequisites</span></span>

<span data-ttu-id="d867c-111">Az Azure AD-integrációs Origami konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="d867c-111">To configure Azure AD integration with Origami, you need the following items:</span></span>

- <span data-ttu-id="d867c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d867c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d867c-113">Egy Origami egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d867c-113">An Origami single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d867c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d867c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d867c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d867c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d867c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d867c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d867c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d867c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d867c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d867c-118">Scenario description</span></span>
<span data-ttu-id="d867c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d867c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d867c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d867c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d867c-121">A gyűjteményből Origami hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d867c-121">Adding Origami from the gallery</span></span>
2. <span data-ttu-id="d867c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d867c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-origami-from-the-gallery"></a><span data-ttu-id="d867c-123">A gyűjteményből Origami hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d867c-123">Adding Origami from the gallery</span></span>
<span data-ttu-id="d867c-124">Az Azure AD integrálása a Origami konfigurálásához kell hozzáadnia Origami a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d867c-124">To configure the integration of Origami into Azure AD, you need to add Origami from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d867c-125">**A gyűjteményből Origami hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d867c-125">**To add Origami from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d867c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d867c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d867c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d867c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d867c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d867c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d867c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d867c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d867c-133">Írja be a keresőmezőbe, **Origami**.</span><span class="sxs-lookup"><span data-stu-id="d867c-133">In the search box, type **Origami**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. <span data-ttu-id="d867c-135">Az eredmények panelen válassza ki a **Origami**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d867c-135">In the results panel, select **Origami**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d867c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d867c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d867c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Origami.</span><span class="sxs-lookup"><span data-stu-id="d867c-138">In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d867c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Origami a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d867c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Origami is to a user in Azure AD.</span></span> <span data-ttu-id="d867c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Origami közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d867c-140">In other words, a link relationship between an Azure AD user and the related user in Origami needs to be established.</span></span>

<span data-ttu-id="d867c-141">Origami, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d867c-141">In Origami, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d867c-142">Az Azure AD egyszeri bejelentkezést a Origami tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d867c-142">To configure and test Azure AD single sign-on with Origami, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d867c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d867c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d867c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d867c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d867c-145">**[Egy Origami tesztfelhasználó létrehozása](#creating-an-origami-test-user)**  - való egy megfelelője a Britta Simon Origami, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d867c-145">**[Creating an Origami test user](#creating-an-origami-test-user)** - to have a counterpart of Britta Simon in Origami that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d867c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d867c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d867c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d867c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d867c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d867c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d867c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Origami alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d867c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Origami application.</span></span>

<span data-ttu-id="d867c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Origami, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d867c-150">**To configure Azure AD single sign-on with Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="d867c-151">Az Azure portálon a a **Origami** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d867c-151">In the Azure portal, on the **Origami** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d867c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d867c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. <span data-ttu-id="d867c-155">Az a **Origami tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d867c-155">On the **Origami Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    <span data-ttu-id="d867c-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://live.origamirisk.com/origami/account/login?account=<companyname>`</span><span class="sxs-lookup"><span data-stu-id="d867c-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d867c-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="d867c-158">The value is not real.</span></span> <span data-ttu-id="d867c-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d867c-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="d867c-160">Ügyfél [Origami ügyfél-támogatási csoport](https://wordpress.org/support/theme/origami) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="d867c-160">Contact [Origami Client support team](https://wordpress.org/support/theme/origami) to get the value.</span></span> 
 
4. <span data-ttu-id="d867c-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d867c-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. <span data-ttu-id="d867c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d867c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d867c-165">A a **Origami konfigurációs** kattintson **konfigurálása Origami** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d867c-165">On the **Origami Configuration** section, click **Configure Origami** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d867c-166">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d867c-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. <span data-ttu-id="d867c-168">Jelentkezzen be a Origami fiók rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="d867c-168">Log in to the Origami account with Admin rights.</span></span>

8. <span data-ttu-id="d867c-169">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d867c-169">In the menu on the top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. <span data-ttu-id="d867c-171">Az egyszeri bejelentkezést a telepítő párbeszédpanel oldalon hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d867c-171">On the Single Sign On Setup dialog page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    <span data-ttu-id="d867c-173">a.</span><span class="sxs-lookup"><span data-stu-id="d867c-173">a.</span></span> <span data-ttu-id="d867c-174">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="d867c-174">Select **Enable Single Sign On**.</span></span>

    <span data-ttu-id="d867c-175">b.</span><span class="sxs-lookup"><span data-stu-id="d867c-175">b.</span></span> <span data-ttu-id="d867c-176">Az a **identitásszolgáltató bejelentkezési lap URL** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d867c-176">In the **Identity Provider's Sign-in Page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d867c-177">c.</span><span class="sxs-lookup"><span data-stu-id="d867c-177">c.</span></span> <span data-ttu-id="d867c-178">A a **identitásszolgáltató Sign-out lap URL-címe** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d867c-178">In the **Identity Provider's Sign-out Page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d867c-179">d.</span><span class="sxs-lookup"><span data-stu-id="d867c-179">d.</span></span> <span data-ttu-id="d867c-180">Kattintson a **Tallózás** töltse fel az Azure-portálról letöltött tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d867c-180">Click **Browse** to upload the certificate you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="d867c-181">e.</span><span class="sxs-lookup"><span data-stu-id="d867c-181">e.</span></span> <span data-ttu-id="d867c-182">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="d867c-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="d867c-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d867c-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d867c-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d867c-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d867c-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d867c-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d867c-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d867c-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="d867c-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d867c-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d867c-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d867c-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d867c-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d867c-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d867c-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d867c-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d867c-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d867c-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d867c-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d867c-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d867c-198">a.</span><span class="sxs-lookup"><span data-stu-id="d867c-198">a.</span></span> <span data-ttu-id="d867c-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d867c-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d867c-200">b.</span><span class="sxs-lookup"><span data-stu-id="d867c-200">b.</span></span> <span data-ttu-id="d867c-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d867c-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d867c-202">c.</span><span class="sxs-lookup"><span data-stu-id="d867c-202">c.</span></span> <span data-ttu-id="d867c-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d867c-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d867c-204">d.</span><span class="sxs-lookup"><span data-stu-id="d867c-204">d.</span></span> <span data-ttu-id="d867c-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d867c-205">Click **Create**.</span></span>
 
### <a name="creating-an-origami-test-user"></a><span data-ttu-id="d867c-206">Egy Origami tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d867c-206">Creating an Origami test user</span></span>

<span data-ttu-id="d867c-207">Ebben a szakaszban egy Origami Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d867c-207">In this section, you create a user called Britta Simon in Origami.</span></span> 

1. <span data-ttu-id="d867c-208">Jelentkezzen be a Origami fiók rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="d867c-208">Log in to the Origami account with Admin rights.</span></span>

2. <span data-ttu-id="d867c-209">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d867c-209">In the menu on the top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. <span data-ttu-id="d867c-211">Az a **felhasználók és biztonsági** párbeszédpanel, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d867c-211">On the **Users and Security** dialog, click **Users**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. <span data-ttu-id="d867c-213">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d867c-213">Click **Add New User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. <span data-ttu-id="d867c-215">Új felhasználó hozzáadása párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d867c-215">On the Add New User dialog, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    <span data-ttu-id="d867c-217">a.</span><span class="sxs-lookup"><span data-stu-id="d867c-217">a.</span></span> <span data-ttu-id="d867c-218">Az a **felhasználónév** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d867c-218">In the **User Name** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="d867c-219">b.</span><span class="sxs-lookup"><span data-stu-id="d867c-219">b.</span></span> <span data-ttu-id="d867c-220">Az a **jelszó** szövegmező, a jelszót írja be.</span><span class="sxs-lookup"><span data-stu-id="d867c-220">In the **Password** textbox, type a password.</span></span>

    <span data-ttu-id="d867c-221">c.</span><span class="sxs-lookup"><span data-stu-id="d867c-221">c.</span></span> <span data-ttu-id="d867c-222">Az a **jelszó megerősítése** szövegmező, írja be ismét a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d867c-222">In the **Confirm Password** textbox, type the password again.</span></span>

    <span data-ttu-id="d867c-223">d.</span><span class="sxs-lookup"><span data-stu-id="d867c-223">d.</span></span> <span data-ttu-id="d867c-224">Az a **Utónév** szövegmező, írja be például a felhasználó utónevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d867c-224">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="d867c-225">e.</span><span class="sxs-lookup"><span data-stu-id="d867c-225">e.</span></span> <span data-ttu-id="d867c-226">Az a **Vezetéknév** szövegmező, írja be például a felhasználó vezetéknevét **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d867c-226">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>

    <span data-ttu-id="d867c-227">f.</span><span class="sxs-lookup"><span data-stu-id="d867c-227">f.</span></span> <span data-ttu-id="d867c-228">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d867c-228">Click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. <span data-ttu-id="d867c-230">Rendelje hozzá **felhasználói szerepkörök** és **ügyfél-hozzáférési** a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="d867c-230">Assign **User Roles** and **Client Access** to the user.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d867c-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d867c-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d867c-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Origami Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d867c-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Origami.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d867c-235">**Britta Simon hozzárendelése Origami, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d867c-235">**To assign Britta Simon to Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="d867c-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d867c-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d867c-238">Az alkalmazások listában válassza ki a **Origami**.</span><span class="sxs-lookup"><span data-stu-id="d867c-238">In the applications list, select **Origami**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. <span data-ttu-id="d867c-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d867c-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d867c-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d867c-242">Click **Add** button.</span></span> <span data-ttu-id="d867c-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d867c-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d867c-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d867c-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d867c-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d867c-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d867c-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d867c-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d867c-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d867c-248">Testing single sign-on</span></span>

<span data-ttu-id="d867c-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d867c-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d867c-250">Ha a hozzáférési panelen Origami csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Origami alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d867c-250">When you click the Origami tile in the Access Panel, you should get automatically signed-on to your Origami application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d867c-251">További források</span><span class="sxs-lookup"><span data-stu-id="d867c-251">Additional resources</span></span>

* [<span data-ttu-id="d867c-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d867c-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d867c-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d867c-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

