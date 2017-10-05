---
title: "Oktatóanyag: Azure Active Directory-integráció laposabb fájlokkal |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és laposabb fájlok között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: e02150cb27768d7b403bdca191bc1f189821def4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="3bf32-103">Oktatóanyag: Azure Active Directoryval integrált laposabb fájlok</span><span class="sxs-lookup"><span data-stu-id="3bf32-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="3bf32-104">Ebben az oktatóanyagban elsajátíthatja laposabb fájlok integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3bf32-104">In this tutorial, you learn how to integrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3bf32-105">Laposabb fájlok integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3bf32-105">Integrating Flatter Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3bf32-106">Megadhatja a laposabb fájlok hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3bf32-106">You can control in Azure AD who has access to Flatter Files</span></span>
- <span data-ttu-id="3bf32-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett laposabb fájlok (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="3bf32-107">You can enable your users to automatically get signed-on to Flatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3bf32-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3bf32-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3bf32-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3bf32-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bf32-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3bf32-110">Prerequisites</span></span>

<span data-ttu-id="3bf32-111">Konfigurálása az Azure AD-integrációs laposabb fájlok, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3bf32-111">To configure Azure AD integration with Flatter Files, you need the following items:</span></span>

- <span data-ttu-id="3bf32-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3bf32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3bf32-113">Egy laposabb fájlok egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3bf32-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3bf32-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3bf32-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3bf32-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3bf32-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3bf32-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3bf32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3bf32-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3bf32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3bf32-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3bf32-118">Scenario description</span></span>
<span data-ttu-id="3bf32-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3bf32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3bf32-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3bf32-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3bf32-121">Laposabb fájlok hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3bf32-121">Adding Flatter Files from the gallery</span></span>
2. <span data-ttu-id="3bf32-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3bf32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-the-gallery"></a><span data-ttu-id="3bf32-123">Laposabb fájlok hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3bf32-123">Adding Flatter Files from the gallery</span></span>
<span data-ttu-id="3bf32-124">Az Azure AD integrálása a laposabb fájlok konfigurálásához szüksége laposabb fájlok hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="3bf32-124">To configure the integration of Flatter Files into Azure AD, you need to add Flatter Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3bf32-125">**Laposabb-fájlok hozzáadása a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bf32-125">**To add Flatter Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3bf32-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3bf32-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3bf32-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3bf32-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3bf32-133">Írja be a keresőmezőbe, **laposabb fájlok**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-133">In the search box, type **Flatter Files**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="3bf32-135">Az eredmények panelen válassza ki a **laposabb fájlok**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3bf32-135">In the results panel, select **Flatter Files**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3bf32-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3bf32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3bf32-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján laposabb fájlokat.</span><span class="sxs-lookup"><span data-stu-id="3bf32-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3bf32-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó laposabb fájlokban a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3bf32-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Flatter Files is to a user in Azure AD.</span></span> <span data-ttu-id="3bf32-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a laposabb fájlok közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3bf32-140">In other words, a link relationship between an Azure AD user and the related user in Flatter Files needs to be established.</span></span>

<span data-ttu-id="3bf32-141">Laposabb fájlokban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3bf32-141">In Flatter Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3bf32-142">Laposabb fájlok az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3bf32-142">To configure and test Azure AD single sign-on with Flatter Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3bf32-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3bf32-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3bf32-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3bf32-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3bf32-145">**[Laposabb fájlok tesztfelhasználó létrehozása](#creating-a-flatter-files-test-user)**  - való egy megfelelője a Britta Simon laposabb fájlok, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3bf32-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - to have a counterpart of Britta Simon in Flatter Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3bf32-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3bf32-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3bf32-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3bf32-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3bf32-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3bf32-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3bf32-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az laposabb fájlok alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3bf32-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="3bf32-150">**Az Azure AD az egyszeri bejelentkezés laposabb fájlokkal megadásához a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="3bf32-150">**To configure Azure AD single sign-on with Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="3bf32-151">Az Azure portálon a a **laposabb fájlok** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-151">In the Azure portal, on the **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3bf32-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3bf32-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="3bf32-155">Az a **laposabb fájlok tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3bf32-155">On the **Flatter Files Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="3bf32-157">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3bf32-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="3bf32-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3bf32-161">A a **laposabb fájlok konfigurációs** kattintson **laposabb fájlok konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="3bf32-161">On the **Flatter Files Configuration** section, click **Configure Flatter Files** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3bf32-162">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="3bf32-162">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="3bf32-164">Bejelentkezés a laposabb fájlok alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3bf32-164">Sign-on to your Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="3bf32-165">Kattintson a **IRÁNYÍTÓPULT**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-165">Click **DASHBOARD**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="3bf32-167">Kattintson a **beállítások**, majd hajtsa végre a következő lépéseket és a **vállalati** lapon:</span><span class="sxs-lookup"><span data-stu-id="3bf32-167">Click **Settings**, and then perform the following steps on the **Company** tab:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="3bf32-169">a.</span><span class="sxs-lookup"><span data-stu-id="3bf32-169">a.</span></span> <span data-ttu-id="3bf32-170">Válassza ki **SAML 2.0 hitelesítéshez használandó**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="3bf32-171">b.</span><span class="sxs-lookup"><span data-stu-id="3bf32-171">b.</span></span> <span data-ttu-id="3bf32-172">Kattintson a **SAML konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="3bf32-173">Az a **SAML-alapú konfigurációs** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3bf32-173">On the **SAML Configuration** dialog, perform the following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="3bf32-175">a.</span><span class="sxs-lookup"><span data-stu-id="3bf32-175">a.</span></span> <span data-ttu-id="3bf32-176">Az a **tartomány** szövegmező, írja be a regisztrált tartománya.</span><span class="sxs-lookup"><span data-stu-id="3bf32-176">In the **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="3bf32-177">Ha még nem rendelkezik egy regisztrált tartományt, mégis, lépjen kapcsolatba a laposabb fájlok támogatja-e a csapat keresztül [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="3bf32-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="3bf32-178">b.</span><span class="sxs-lookup"><span data-stu-id="3bf32-178">b.</span></span> <span data-ttu-id="3bf32-179">A **identitási szolgáltató URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** másolta, amely Azure-portálon alkotnak.</span><span class="sxs-lookup"><span data-stu-id="3bf32-179">In **Identity Provider URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="3bf32-180">c.</span><span class="sxs-lookup"><span data-stu-id="3bf32-180">c.</span></span>  <span data-ttu-id="3bf32-181">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a tartalmának másolása a vágólapra és illessze be azt a **szolgáltató Identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3bf32-181">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="3bf32-182">d.</span><span class="sxs-lookup"><span data-stu-id="3bf32-182">d.</span></span> <span data-ttu-id="3bf32-183">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="3bf32-184">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3bf32-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3bf32-185">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3bf32-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3bf32-186">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3bf32-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3bf32-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bf32-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="3bf32-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3bf32-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3bf32-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bf32-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3bf32-191">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3bf32-193">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3bf32-195">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3bf32-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3bf32-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3bf32-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3bf32-199">a.</span><span class="sxs-lookup"><span data-stu-id="3bf32-199">a.</span></span> <span data-ttu-id="3bf32-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3bf32-201">b.</span><span class="sxs-lookup"><span data-stu-id="3bf32-201">b.</span></span> <span data-ttu-id="3bf32-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3bf32-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3bf32-203">c.</span><span class="sxs-lookup"><span data-stu-id="3bf32-203">c.</span></span> <span data-ttu-id="3bf32-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3bf32-205">d.</span><span class="sxs-lookup"><span data-stu-id="3bf32-205">d.</span></span> <span data-ttu-id="3bf32-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="3bf32-207">Laposabb fájlok tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bf32-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="3bf32-208">Ez a szakasz célja laposabb fájlok Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3bf32-208">The objective of this section is to create a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="3bf32-209">**A felhasználó Britta Simon meghívta laposabb fájlok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bf32-209">**To create a user called Britta Simon in Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="3bf32-210">Jelentkezzen be a **laposabb fájlok** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3bf32-210">Sign on to your **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="3bf32-211">A bal oldali navigációs ablaktábláján kattintson **beállítások**, majd kattintson a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="3bf32-211">In the navigation pane on the left, click **Settings**, and then click the **Users** tab.</span></span>
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="3bf32-213">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="3bf32-214">Az a **felhasználó hozzáadása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3bf32-214">On the **Add User** dialog, perform the following steps:</span></span>
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="3bf32-216">a.</span><span class="sxs-lookup"><span data-stu-id="3bf32-216">a.</span></span> <span data-ttu-id="3bf32-217">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-217">In the **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="3bf32-218">b.</span><span class="sxs-lookup"><span data-stu-id="3bf32-218">b.</span></span> <span data-ttu-id="3bf32-219">Az a **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-219">In the **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="3bf32-220">c.</span><span class="sxs-lookup"><span data-stu-id="3bf32-220">c.</span></span> <span data-ttu-id="3bf32-221">Az a **E-mail cím** szövegmező, írja be a Britta meg e-mail címét az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3bf32-221">In the **Email Address** textbox, type Britta's email address in the Azure portal.</span></span>
   
    <span data-ttu-id="3bf32-222">d.</span><span class="sxs-lookup"><span data-stu-id="3bf32-222">d.</span></span> <span data-ttu-id="3bf32-223">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-223">Click **Submit**.</span></span>   


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3bf32-224">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3bf32-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3bf32-225">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés laposabb fájlok Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3bf32-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Flatter Files.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3bf32-227">**Fájlokhoz hozzárendelni kívánt Britta Simon laposabb, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bf32-227">**To assign Britta Simon to Flatter Files, perform the following steps:**</span></span>

1. <span data-ttu-id="3bf32-228">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3bf32-230">Az alkalmazások listában válassza ki a **laposabb fájlok**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-230">In the applications list, select **Flatter Files**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="3bf32-232">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3bf32-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3bf32-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3bf32-234">Click **Add** button.</span></span> <span data-ttu-id="3bf32-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bf32-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3bf32-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3bf32-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3bf32-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bf32-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3bf32-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bf32-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3bf32-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3bf32-240">Testing single sign-on</span></span>

<span data-ttu-id="3bf32-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3bf32-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3bf32-242">Ha a hozzáférési panelen laposabb fájlok csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az laposabb fájlok alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3bf32-242">When you click the Flatter Files tile in the Access Panel, you should get automatically signed-on to your Flatter Files application.</span></span>
<span data-ttu-id="3bf32-243">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3bf32-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bf32-244">További források</span><span class="sxs-lookup"><span data-stu-id="3bf32-244">Additional resources</span></span>

* [<span data-ttu-id="3bf32-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3bf32-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3bf32-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3bf32-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

