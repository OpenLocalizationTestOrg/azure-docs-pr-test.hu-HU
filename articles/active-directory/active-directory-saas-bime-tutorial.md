---
title: "Oktatóanyag: Azure Active Directoryval integrált Bime |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Bime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 8f46ff1265d302ab114747b4b45227e58718166b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a><span data-ttu-id="d23aa-103">Oktatóanyag: Azure Active Directoryval integrált Bime</span><span class="sxs-lookup"><span data-stu-id="d23aa-103">Tutorial: Azure Active Directory integration with Bime</span></span>

<span data-ttu-id="d23aa-104">Ebben az oktatóanyagban elsajátíthatja Bime integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d23aa-104">In this tutorial, you learn how to integrate Bime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d23aa-105">Bime integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d23aa-105">Integrating Bime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d23aa-106">Megadhatja a Bime hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d23aa-106">You can control in Azure AD who has access to Bime</span></span>
- <span data-ttu-id="d23aa-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Bime (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d23aa-107">You can enable your users to automatically get signed-on to Bime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d23aa-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d23aa-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d23aa-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d23aa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d23aa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d23aa-110">Prerequisites</span></span>

<span data-ttu-id="d23aa-111">Konfigurálása az Azure AD-integrációs Bime, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d23aa-111">To configure Azure AD integration with Bime, you need the following items:</span></span>

- <span data-ttu-id="d23aa-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d23aa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d23aa-113">Egy Bime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d23aa-113">A Bime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d23aa-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d23aa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d23aa-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d23aa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d23aa-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d23aa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d23aa-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d23aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d23aa-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d23aa-118">Scenario description</span></span>
<span data-ttu-id="d23aa-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d23aa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d23aa-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d23aa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d23aa-121">A gyűjteményből Bime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23aa-121">Adding Bime from the gallery</span></span>
2. <span data-ttu-id="d23aa-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d23aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bime-from-the-gallery"></a><span data-ttu-id="d23aa-123">A gyűjteményből Bime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23aa-123">Adding Bime from the gallery</span></span>
<span data-ttu-id="d23aa-124">Az Azure AD integrálása a Bime konfigurálásához kell hozzáadnia Bime a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d23aa-124">To configure the integration of Bime into Azure AD, you need to add Bime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d23aa-125">**A gyűjteményből Bime hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d23aa-125">**To add Bime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d23aa-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d23aa-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d23aa-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d23aa-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d23aa-133">Írja be a keresőmezőbe, **Bime**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-133">In the search box, type **Bime**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. <span data-ttu-id="d23aa-135">Az eredmények panelen válassza ki a **Bime**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d23aa-135">In the results panel, select **Bime**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d23aa-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d23aa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d23aa-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Bime.</span><span class="sxs-lookup"><span data-stu-id="d23aa-138">In this section, you configure and test Azure AD single sign-on with Bime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d23aa-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Bime a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d23aa-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bime is to a user in Azure AD.</span></span> <span data-ttu-id="d23aa-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Bime közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d23aa-140">In other words, a link relationship between an Azure AD user and the related user in Bime needs to be established.</span></span>

<span data-ttu-id="d23aa-141">Bime, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d23aa-141">In Bime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d23aa-142">Az Azure AD egyszeri bejelentkezést a Bime tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d23aa-142">To configure and test Azure AD single sign-on with Bime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d23aa-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d23aa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d23aa-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d23aa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d23aa-145">**[Bime tesztfelhasználó létrehozása](#creating-a-bime-test-user)**  - való Britta Simon valami Bime, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d23aa-145">**[Creating a Bime test user](#creating-a-bime-test-user)** - to have a counterpart of Britta Simon in Bime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d23aa-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d23aa-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d23aa-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d23aa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d23aa-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d23aa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d23aa-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Bime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d23aa-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bime application.</span></span>

<span data-ttu-id="d23aa-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Bime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d23aa-150">**To configure Azure AD single sign-on with Bime, perform the following steps:**</span></span>

1. <span data-ttu-id="d23aa-151">Az Azure portálon a a **Bime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-151">In the Azure portal, on the **Bime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d23aa-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d23aa-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. <span data-ttu-id="d23aa-155">Az a **Bime tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d23aa-155">On the **Bime Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    <span data-ttu-id="d23aa-157">a.</span><span class="sxs-lookup"><span data-stu-id="d23aa-157">a.</span></span> <span data-ttu-id="d23aa-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d23aa-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    <span data-ttu-id="d23aa-159">b.</span><span class="sxs-lookup"><span data-stu-id="d23aa-159">b.</span></span> <span data-ttu-id="d23aa-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d23aa-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d23aa-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d23aa-161">These values are not real.</span></span> <span data-ttu-id="d23aa-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d23aa-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d23aa-163">Ügyfél [Bime ügyfél-támogatási csoport](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d23aa-163">Contact [Bime Client support team](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) to get these values.</span></span> 
 
4. <span data-ttu-id="d23aa-164">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** érték a tanúsítványból.</span><span class="sxs-lookup"><span data-stu-id="d23aa-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value from the certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. <span data-ttu-id="d23aa-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d23aa-168">A a **Bime konfigurációs** kattintson **konfigurálása Bime** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d23aa-168">On the **Bime Configuration** section, click **Configure Bime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d23aa-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d23aa-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. <span data-ttu-id="d23aa-171">Egy másik webes böngészőablakban jelentkezzen be a Bime vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d23aa-171">In a different web browser window, log into your Bime company site as an administrator.</span></span>

8. <span data-ttu-id="d23aa-172">Az eszköztáron kattintson **Admin**, majd **fiók**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-172">In the toolbar, click **Admin**, and then **Account**.</span></span>
   
    <span data-ttu-id="d23aa-173">![Felügyeleti](./media/active-directory-saas-bime-tutorial/ic775558.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="d23aa-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span></span>

9. <span data-ttu-id="d23aa-174">A fiók konfigurálása lapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d23aa-174">On the account configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="d23aa-175">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/ic775559.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="d23aa-175">![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/ic775559.png "Configure Single Sign-On")</span></span>
   
    <span data-ttu-id="d23aa-176">a.</span><span class="sxs-lookup"><span data-stu-id="d23aa-176">a.</span></span> <span data-ttu-id="d23aa-177">Válassza ki **engedélyezése SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-177">Select **Enable SAML authentication**.</span></span>

    <span data-ttu-id="d23aa-178">b.</span><span class="sxs-lookup"><span data-stu-id="d23aa-178">b.</span></span> <span data-ttu-id="d23aa-179">A a **távoli bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d23aa-179">In the **Remote Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d23aa-180">c.</span><span class="sxs-lookup"><span data-stu-id="d23aa-180">c.</span></span>  <span data-ttu-id="d23aa-181">Beillesztés a **ujjlenyomat** érték az Azure-portálon a **tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d23aa-181">Paste the **Thumbprint** value from Azure portal into the **Certificate Fingerprint** textbox.</span></span>       
   
    <span data-ttu-id="d23aa-182">d.</span><span class="sxs-lookup"><span data-stu-id="d23aa-182">d.</span></span> <span data-ttu-id="d23aa-183">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d23aa-184">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d23aa-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d23aa-185">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d23aa-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d23aa-186">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d23aa-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d23aa-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d23aa-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="d23aa-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d23aa-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d23aa-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d23aa-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d23aa-191">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d23aa-193">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d23aa-195">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d23aa-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d23aa-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d23aa-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d23aa-199">a.</span><span class="sxs-lookup"><span data-stu-id="d23aa-199">a.</span></span> <span data-ttu-id="d23aa-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d23aa-201">b.</span><span class="sxs-lookup"><span data-stu-id="d23aa-201">b.</span></span> <span data-ttu-id="d23aa-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d23aa-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d23aa-203">c.</span><span class="sxs-lookup"><span data-stu-id="d23aa-203">c.</span></span> <span data-ttu-id="d23aa-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d23aa-205">d.</span><span class="sxs-lookup"><span data-stu-id="d23aa-205">d.</span></span> <span data-ttu-id="d23aa-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-206">Click **Create**.</span></span>
 
### <a name="creating-a-bime-test-user"></a><span data-ttu-id="d23aa-207">Bime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d23aa-207">Creating a Bime test user</span></span>

<span data-ttu-id="d23aa-208">Ahhoz, hogy az Azure AD-felhasználók Bime bejelentkezni, akkor ki kell építenie a Bime.</span><span class="sxs-lookup"><span data-stu-id="d23aa-208">In order to enable Azure AD users to log in to Bime, they must be provisioned into Bime.</span></span> <span data-ttu-id="d23aa-209">Bime, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d23aa-209">In the case of Bime, provisioning is a manual task.</span></span>

<span data-ttu-id="d23aa-210">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d23aa-210">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="d23aa-211">Jelentkezzen be a **Bime** bérlő.</span><span class="sxs-lookup"><span data-stu-id="d23aa-211">Log in to your **Bime** tenant.</span></span>

2. <span data-ttu-id="d23aa-212">Az eszköztáron kattintson **Admin**, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-212">In the toolbar, click **Admin**, and then **Users**.</span></span>
   
    <span data-ttu-id="d23aa-213">![Felügyeleti](./media/active-directory-saas-bime-tutorial/ic775561.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="d23aa-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span></span>

3. <span data-ttu-id="d23aa-214">Az a **felhasználók listában**, kattintson a **új felhasználó hozzáadása** ("+").</span><span class="sxs-lookup"><span data-stu-id="d23aa-214">In the **Users List**, click **Add New User** (“+”).</span></span>
   
    <span data-ttu-id="d23aa-215">![Felhasználók](./media/active-directory-saas-bime-tutorial/ic775562.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="d23aa-215">![Users](./media/active-directory-saas-bime-tutorial/ic775562.png "Users")</span></span>

4. <span data-ttu-id="d23aa-216">Az a **felhasználó adatait** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d23aa-216">On the **User Details** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="d23aa-217">![Felhasználó adatait](./media/active-directory-saas-bime-tutorial/ic775563.png "felhasználó részletei")</span><span class="sxs-lookup"><span data-stu-id="d23aa-217">![User Details](./media/active-directory-saas-bime-tutorial/ic775563.png "User Details")</span></span>
   
    <span data-ttu-id="d23aa-218">a.</span><span class="sxs-lookup"><span data-stu-id="d23aa-218">a.</span></span> <span data-ttu-id="d23aa-219">Az a **Utónév** szövegmező, írja be például a felhasználó utónevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-219">In the **First name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="d23aa-220">b.</span><span class="sxs-lookup"><span data-stu-id="d23aa-220">b.</span></span> <span data-ttu-id="d23aa-221">Az a **Vezetéknév** szövegmező, írja be például a felhasználó vezetéknevét **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-221">In the **Last name** textbox, enter the last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="d23aa-222">c.</span><span class="sxs-lookup"><span data-stu-id="d23aa-222">c.</span></span> <span data-ttu-id="d23aa-223">Az a **E-mail** szövegmező, adja meg az e-mail címét, például a felhasználó  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d23aa-223">In the **Email** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="d23aa-224">d.</span><span class="sxs-lookup"><span data-stu-id="d23aa-224">d.</span></span> <span data-ttu-id="d23aa-225">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-225">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="d23aa-226">Bármely más Bime felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Bime által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="d23aa-226">You can use any other Bime user account creation tools or APIs provided by Bime to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d23aa-227">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d23aa-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d23aa-228">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Bime Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d23aa-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Bime.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d23aa-230">**Britta Simon hozzárendelése Bime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d23aa-230">**To assign Britta Simon to Bime, perform the following steps:**</span></span>

1. <span data-ttu-id="d23aa-231">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d23aa-233">Az alkalmazások listában válassza ki a **Bime**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-233">In the applications list, select **Bime**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. <span data-ttu-id="d23aa-235">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d23aa-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d23aa-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d23aa-237">Click **Add** button.</span></span> <span data-ttu-id="d23aa-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23aa-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d23aa-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d23aa-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d23aa-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23aa-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d23aa-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23aa-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d23aa-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d23aa-243">Testing single sign-on</span></span>

<span data-ttu-id="d23aa-244">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="d23aa-244">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d23aa-245">Ha a hozzáférési panelen Bime csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Bime alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d23aa-245">When you click the Bime tile in the Access Panel, you should get automatically signed-on to your Bime application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d23aa-246">További források</span><span class="sxs-lookup"><span data-stu-id="d23aa-246">Additional resources</span></span>

* [<span data-ttu-id="d23aa-247">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d23aa-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d23aa-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d23aa-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

