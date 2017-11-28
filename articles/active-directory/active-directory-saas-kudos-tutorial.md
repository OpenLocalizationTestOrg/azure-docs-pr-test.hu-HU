---
title: "Oktatóanyag: Azure Active Directoryval integrált Kudos |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Kudos között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 353798fcfd4ad7ce017fc2fddf4110715db3ace2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="3cd71-103">Oktatóanyag: Azure Active Directoryval integrált Kudos</span><span class="sxs-lookup"><span data-stu-id="3cd71-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="3cd71-104">Ebben az oktatóanyagban elsajátíthatja Kudos integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3cd71-104">In this tutorial, you learn how to integrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3cd71-105">Kudos integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3cd71-105">Integrating Kudos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3cd71-106">Megadhatja a Kudos hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3cd71-106">You can control in Azure AD who has access to Kudos</span></span>
- <span data-ttu-id="3cd71-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Kudos (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3cd71-107">You can enable your users to automatically get signed-on to Kudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3cd71-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3cd71-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3cd71-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3cd71-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cd71-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3cd71-110">Prerequisites</span></span>

<span data-ttu-id="3cd71-111">Konfigurálása az Azure AD-integrációs Kudos, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3cd71-111">To configure Azure AD integration with Kudos, you need the following items:</span></span>

- <span data-ttu-id="3cd71-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3cd71-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3cd71-113">Egy Kudos egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3cd71-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3cd71-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3cd71-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3cd71-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3cd71-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3cd71-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3cd71-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3cd71-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3cd71-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3cd71-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3cd71-118">Scenario description</span></span>
<span data-ttu-id="3cd71-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3cd71-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3cd71-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3cd71-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3cd71-121">A gyűjteményből Kudos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3cd71-121">Adding Kudos from the gallery</span></span>
2. <span data-ttu-id="3cd71-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3cd71-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-the-gallery"></a><span data-ttu-id="3cd71-123">A gyűjteményből Kudos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3cd71-123">Adding Kudos from the gallery</span></span>
<span data-ttu-id="3cd71-124">Az Azure AD integrálása a Kudos konfigurálásához kell hozzáadnia Kudos a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3cd71-124">To configure the integration of Kudos into Azure AD, you need to add Kudos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3cd71-125">**A gyűjteményből Kudos hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3cd71-125">**To add Kudos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3cd71-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3cd71-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3cd71-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3cd71-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3cd71-133">Írja be a keresőmezőbe, **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-133">In the search box, type **Kudos**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="3cd71-135">Az eredmények panelen válassza ki a **Kudos**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3cd71-135">In the results panel, select **Kudos**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3cd71-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3cd71-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3cd71-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kudos.</span><span class="sxs-lookup"><span data-stu-id="3cd71-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3cd71-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kudos a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3cd71-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kudos is to a user in Azure AD.</span></span> <span data-ttu-id="3cd71-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Kudos közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3cd71-140">In other words, a link relationship between an Azure AD user and the related user in Kudos needs to be established.</span></span>

<span data-ttu-id="3cd71-141">Kudos, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3cd71-141">In Kudos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3cd71-142">Az Azure AD egyszeri bejelentkezést a Kudos tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3cd71-142">To configure and test Azure AD single sign-on with Kudos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3cd71-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3cd71-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3cd71-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3cd71-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3cd71-145">**[Kudos tesztfelhasználó létrehozása](#creating-a-kudos-test-user)**  - való Britta Simon valami Kudos, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3cd71-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - to have a counterpart of Britta Simon in Kudos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3cd71-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3cd71-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3cd71-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3cd71-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3cd71-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3cd71-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3cd71-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Kudos alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3cd71-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="3cd71-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Kudos, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3cd71-150">**To configure Azure AD single sign-on with Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="3cd71-151">Az Azure portálon a a **Kudos** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-151">In the Azure portal, on the **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3cd71-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3cd71-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="3cd71-155">Az a **Kudos tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3cd71-155">On the **Kudos Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="3cd71-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="3cd71-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="3cd71-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="3cd71-158">This value is not real.</span></span> <span data-ttu-id="3cd71-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="3cd71-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3cd71-160">Ügyfél [Kudos ügyfél-támogatási csoport](http://success.kudosnow.com/home) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="3cd71-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) to get this value.</span></span> 
 
4. <span data-ttu-id="3cd71-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3cd71-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="3cd71-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3cd71-165">A a **Kudos konfigurációs** kattintson **konfigurálása Kudos** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="3cd71-165">On the **Kudos Configuration** section, click **Configure Kudos** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3cd71-166">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="3cd71-166">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="3cd71-168">Egy másik webes böngészőablakban jelentkezzen be a Kudos vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3cd71-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="3cd71-169">Kattintson a felső menüben **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-169">In the menu on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="3cd71-170">![Beállítások](./media/active-directory-saas-kudos-tutorial/ic787806.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="3cd71-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="3cd71-171">Kattintson a **integrációja \> SSO**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="3cd71-172">Az a **SSO** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3cd71-172">In the **SSO** section, perform the following steps:</span></span>
   
    <span data-ttu-id="3cd71-173">![EGYSZERI BEJELENTKEZÉS](./media/active-directory-saas-kudos-tutorial/ic787807.png "EGYSZERI BEJELENTKEZÉS")</span><span class="sxs-lookup"><span data-stu-id="3cd71-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="3cd71-174">a.</span><span class="sxs-lookup"><span data-stu-id="3cd71-174">a.</span></span> <span data-ttu-id="3cd71-175">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3cd71-175">In **Sign on URL** textbox, paste the value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="3cd71-176">b.</span><span class="sxs-lookup"><span data-stu-id="3cd71-176">b.</span></span> <span data-ttu-id="3cd71-177">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="3cd71-177">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="3cd71-178">c.</span><span class="sxs-lookup"><span data-stu-id="3cd71-178">c.</span></span> <span data-ttu-id="3cd71-179">A **kijelentkezési URL-CÍMRE való**, illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3cd71-179">In **Logout To URL**, paste the value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="3cd71-180">d.</span><span class="sxs-lookup"><span data-stu-id="3cd71-180">d.</span></span> <span data-ttu-id="3cd71-181">Az a **a Kudos URL-cím** szövegmező, írja be a cég nevét.</span><span class="sxs-lookup"><span data-stu-id="3cd71-181">In the **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="3cd71-182">e.</span><span class="sxs-lookup"><span data-stu-id="3cd71-182">e.</span></span> <span data-ttu-id="3cd71-183">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3cd71-184">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3cd71-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3cd71-185">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3cd71-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3cd71-186">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3cd71-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3cd71-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3cd71-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="3cd71-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3cd71-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3cd71-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3cd71-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3cd71-191">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3cd71-193">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3cd71-195">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3cd71-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3cd71-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3cd71-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3cd71-199">a.</span><span class="sxs-lookup"><span data-stu-id="3cd71-199">a.</span></span> <span data-ttu-id="3cd71-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3cd71-201">b.</span><span class="sxs-lookup"><span data-stu-id="3cd71-201">b.</span></span> <span data-ttu-id="3cd71-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3cd71-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3cd71-203">c.</span><span class="sxs-lookup"><span data-stu-id="3cd71-203">c.</span></span> <span data-ttu-id="3cd71-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3cd71-205">d.</span><span class="sxs-lookup"><span data-stu-id="3cd71-205">d.</span></span> <span data-ttu-id="3cd71-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="3cd71-207">Kudos tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3cd71-207">Creating a Kudos test user</span></span>

<span data-ttu-id="3cd71-208">Ahhoz, hogy az Azure AD-felhasználók Kudos bejelentkezni, akkor ki kell építenie Kudos be.</span><span class="sxs-lookup"><span data-stu-id="3cd71-208">In order to enable Azure AD users to log into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="3cd71-209">Kudos, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3cd71-209">In the case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="3cd71-210">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3cd71-210">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="3cd71-211">Jelentkezzen be a **Kudos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3cd71-211">Log in to your **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="3cd71-212">Kattintson a felső menüben **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-212">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="3cd71-213">![Beállítások](./media/active-directory-saas-kudos-tutorial/ic787806.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="3cd71-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="3cd71-214">Kattintson a **felhasználói Admin**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="3cd71-215">Kattintson a **felhasználók** fülre, majd **hozzáadni egy felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-215">Click the **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="3cd71-216">![Felhasználó rendszergazda](./media/active-directory-saas-kudos-tutorial/ic787809.png "felhasználó rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="3cd71-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="3cd71-217">Az a **hozzáadni egy felhasználót** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3cd71-217">In the **Add a User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="3cd71-218">![Felhasználó hozzáadása](./media/active-directory-saas-kudos-tutorial/ic787810.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3cd71-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="3cd71-219">a.</span><span class="sxs-lookup"><span data-stu-id="3cd71-219">a.</span></span> <span data-ttu-id="3cd71-220">Típus a **Utónév**, **Vezetéknév**, **E-mail** és egyéb részleteket a egy érvényes Azure Active Directory-fiók szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3cd71-220">Type the **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="3cd71-221">b.</span><span class="sxs-lookup"><span data-stu-id="3cd71-221">b.</span></span> <span data-ttu-id="3cd71-222">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="3cd71-223">Bármely más Kudos felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Kudos által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="3cd71-223">You can use any other Kudos user account creation tools or APIs provided by Kudos to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3cd71-224">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3cd71-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3cd71-225">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kudos Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3cd71-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kudos.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3cd71-227">**Britta Simon hozzárendelése Kudos, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3cd71-227">**To assign Britta Simon to Kudos, perform the following steps:**</span></span>

1. <span data-ttu-id="3cd71-228">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3cd71-230">Az alkalmazások listában válassza ki a **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-230">In the applications list, select **Kudos**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="3cd71-232">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3cd71-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3cd71-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3cd71-234">Click **Add** button.</span></span> <span data-ttu-id="3cd71-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3cd71-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3cd71-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3cd71-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3cd71-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3cd71-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3cd71-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3cd71-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3cd71-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3cd71-240">Testing single sign-on</span></span>

<span data-ttu-id="3cd71-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3cd71-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3cd71-242">Ha a hozzáférési panelen Kudos csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Kudos alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="3cd71-242">When you click the Kudos tile in the Access Panel, you should get automatically signed-on to your Kudos application.</span></span> <span data-ttu-id="3cd71-243">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3cd71-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3cd71-244">További források</span><span class="sxs-lookup"><span data-stu-id="3cd71-244">Additional resources</span></span>

* [<span data-ttu-id="3cd71-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3cd71-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3cd71-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3cd71-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

