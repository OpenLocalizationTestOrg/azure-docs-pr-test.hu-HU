---
title: "Oktatóanyag: Azure Active Directoryval integrált nevezetesen |} Microsoft Docs"
description: "Egyszeri bejelentkezés Azure Active Directory közötti konfigurálásával, nevezetesen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 1d7e8fbcfc757853ab909bbb05522f3dc387715d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="97ac2-103">Oktatóanyag: Azure Active Directoryval integrált nevezetesen</span><span class="sxs-lookup"><span data-stu-id="97ac2-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="97ac2-104">Ebben az oktatóanyagban elsajátíthatja, nevezetesen integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="97ac2-104">In this tutorial, you learn how to integrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="97ac2-105">Kulcstartó integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="97ac2-105">Integrating Namely with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="97ac2-106">Szabályozhatja, aki nevezetesen Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="97ac2-106">You can control in Azure AD who has access to Namely</span></span>
- <span data-ttu-id="97ac2-107">Engedélyezheti a felhasználók automatikusan el nevezetesen a bejelentkezett (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="97ac2-107">You can enable your users to automatically get signed-on to Namely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="97ac2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="97ac2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="97ac2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97ac2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97ac2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="97ac2-110">Prerequisites</span></span>

<span data-ttu-id="97ac2-111">Az Azure AD konfigurálása, hogy az integrációs kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="97ac2-111">To configure Azure AD integration with Namely, you need the following items:</span></span>

- <span data-ttu-id="97ac2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="97ac2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="97ac2-113">Kulcstartó egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="97ac2-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="97ac2-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="97ac2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="97ac2-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="97ac2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97ac2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="97ac2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="97ac2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97ac2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97ac2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="97ac2-118">Scenario description</span></span>
<span data-ttu-id="97ac2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="97ac2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="97ac2-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="97ac2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97ac2-121">A gyűjteményből nevezetesen hozzáadása</span><span class="sxs-lookup"><span data-stu-id="97ac2-121">Adding Namely from the gallery</span></span>
2. <span data-ttu-id="97ac2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="97ac2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-the-gallery"></a><span data-ttu-id="97ac2-123">A gyűjteményből nevezetesen hozzáadása</span><span class="sxs-lookup"><span data-stu-id="97ac2-123">Adding Namely from the gallery</span></span>
<span data-ttu-id="97ac2-124">Nevezetesen az Azure AD integrálása konfigurálásához kell hozzáadnia nevezetesen a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="97ac2-124">To configure the integration of Namely into Azure AD, you need to add Namely from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="97ac2-125">**Kulcstartó adja hozzá a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="97ac2-125">**To add Namely from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="97ac2-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97ac2-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="97ac2-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="97ac2-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="97ac2-133">Írja be a keresőmezőbe, **nevezetesen**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-133">In the search box, type **Namely**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="97ac2-135">Az eredmények panelen válassza ki a **nevezetesen**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="97ac2-135">In the results panel, select **Namely**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="97ac2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="97ac2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="97ac2-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést, nevezetesen "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="97ac2-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="97ac2-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó nevezetesen egy felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="97ac2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Namely is to a user in Azure AD.</span></span> <span data-ttu-id="97ac2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a hivatkozás kapcsolatának nevezetesen kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="97ac2-140">In other words, a link relationship between an Azure AD user and the related user in Namely needs to be established.</span></span>

<span data-ttu-id="97ac2-141">A kulcstartó, rendelje az értékét a **felhasználónév** az Azure AD-be a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="97ac2-141">In Namely, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="97ac2-142">Az Azure AD tesztelése és konfigurálása egyszeri bejelentkezést, Önnek kell végezze el a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="97ac2-142">To configure and test Azure AD single sign-on with Namely, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="97ac2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="97ac2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="97ac2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="97ac2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97ac2-145">**[Létrehozása egy nevezetesen tesztfelhasználó](#creating-a-namely-test-user)**  – egy partner Britta Simon, hogy a nevezetesen, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="97ac2-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - to have a counterpart of Britta Simon in Namely that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="97ac2-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="97ac2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97ac2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="97ac2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="97ac2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="97ac2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="97ac2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a nevezetesen alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="97ac2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="97ac2-150">**Az Azure AD konfigurálása egyszeri bejelentkezést a nevezetesen, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="97ac2-150">**To configure Azure AD single sign-on with Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="97ac2-151">Az Azure portálon a a **nevezetesen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-151">In the Azure portal, on the **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="97ac2-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="97ac2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="97ac2-155">Az a **nevezetesen tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="97ac2-155">On the **Namely Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="97ac2-157">a.</span><span class="sxs-lookup"><span data-stu-id="97ac2-157">a.</span></span> <span data-ttu-id="97ac2-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="97ac2-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="97ac2-159">b.</span><span class="sxs-lookup"><span data-stu-id="97ac2-159">b.</span></span> <span data-ttu-id="97ac2-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="97ac2-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="97ac2-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="97ac2-161">These values are not real.</span></span> <span data-ttu-id="97ac2-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="97ac2-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="97ac2-163">Ügyfél [nevezetesen ügyfél-támogatási csoport](https://www.namely.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="97ac2-163">Contact [Namely Client support team](https://www.namely.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="97ac2-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="97ac2-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="97ac2-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="97ac2-168">A a **nevezetesen konfigurációs** kattintson **konfigurálása nevezetesen** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="97ac2-168">On the **Namely Configuration** section, click **Configure Namely** to open **Configure sign-on** window.</span></span> <span data-ttu-id="97ac2-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="97ac2-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="97ac2-171">Egy másik böngészőablakban, jelentkezzen be a nevezetesen vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="97ac2-171">In another browser window, sign on to your Namely company site as an administrator.</span></span>

8. <span data-ttu-id="97ac2-172">A felső eszköztáron kattintson **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-172">In the toolbar on the top, click **Company**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="97ac2-174">Kattintson a **Beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="97ac2-174">Click the **Settings** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="97ac2-176">Kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-176">Click **SAML**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="97ac2-178">Az a **SAML beállítások** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="97ac2-178">On the **SAML Settings** page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="97ac2-180">a.</span><span class="sxs-lookup"><span data-stu-id="97ac2-180">a.</span></span> <span data-ttu-id="97ac2-181">Kattintson a **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="97ac2-182">b.</span><span class="sxs-lookup"><span data-stu-id="97ac2-182">b.</span></span> <span data-ttu-id="97ac2-183">A a **identitási szolgáltató egyszeri bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="97ac2-183">In the **Identity provider SSO url** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="97ac2-184">c.</span><span class="sxs-lookup"><span data-stu-id="97ac2-184">c.</span></span> <span data-ttu-id="97ac2-185">A letöltött tanúsítvány megnyitása a Jegyzettömbben, másolja a tartalmat, és illessze be azt a **szolgáltató identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="97ac2-185">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="97ac2-186">d.</span><span class="sxs-lookup"><span data-stu-id="97ac2-186">d.</span></span> <span data-ttu-id="97ac2-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="97ac2-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="97ac2-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="97ac2-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="97ac2-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="97ac2-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="97ac2-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="97ac2-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="97ac2-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="97ac2-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="97ac2-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="97ac2-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="97ac2-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="97ac2-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97ac2-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97ac2-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="97ac2-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97ac2-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="97ac2-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="97ac2-203">a.</span><span class="sxs-lookup"><span data-stu-id="97ac2-203">a.</span></span> <span data-ttu-id="97ac2-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="97ac2-205">b.</span><span class="sxs-lookup"><span data-stu-id="97ac2-205">b.</span></span> <span data-ttu-id="97ac2-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97ac2-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97ac2-207">c.</span><span class="sxs-lookup"><span data-stu-id="97ac2-207">c.</span></span> <span data-ttu-id="97ac2-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="97ac2-209">d.</span><span class="sxs-lookup"><span data-stu-id="97ac2-209">d.</span></span> <span data-ttu-id="97ac2-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="97ac2-211">Létrehozása egy nevezetesen teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="97ac2-211">Creating a Namely test user</span></span>

<span data-ttu-id="97ac2-212">Ez a szakasz célja, nevezetesen Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="97ac2-212">The objective of this section is to create a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="97ac2-213">**A felhasználó, nevezetesen Britta Simon meghívta létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="97ac2-213">**To create a user called Britta Simon in Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="97ac2-214">Bejelentkezés a nevezetesen vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="97ac2-214">Sign-on to your Namely company site as an administrator.</span></span>

2. <span data-ttu-id="97ac2-215">A felső eszköztáron kattintson **személyek**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-215">In the toolbar on the top, click **People**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="97ac2-217">Kattintson a **Directory** fülre.</span><span class="sxs-lookup"><span data-stu-id="97ac2-217">Click the **Directory** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="97ac2-219">Kattintson a **adja hozzá az új személy**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-219">Click **Add New Person**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="97ac2-221">Az a **hozzáadása új személy** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="97ac2-221">On the **Add New Person** dialog, perform the following steps:</span></span>

    <span data-ttu-id="97ac2-222">a.</span><span class="sxs-lookup"><span data-stu-id="97ac2-222">a.</span></span> <span data-ttu-id="97ac2-223">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-223">In the **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="97ac2-224">b.</span><span class="sxs-lookup"><span data-stu-id="97ac2-224">b.</span></span> <span data-ttu-id="97ac2-225">Az a **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-225">In the **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="97ac2-226">c.</span><span class="sxs-lookup"><span data-stu-id="97ac2-226">c.</span></span> <span data-ttu-id="97ac2-227">Az a **E-mail** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97ac2-227">In the **Email** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="97ac2-228">d.</span><span class="sxs-lookup"><span data-stu-id="97ac2-228">d.</span></span> <span data-ttu-id="97ac2-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-229">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="97ac2-230">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="97ac2-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="97ac2-231">Ebben a szakaszban Britta Simon való hozzáférés biztosítása nevezetesen által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="97ac2-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Namely.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="97ac2-233">**A következő lépésekkel, a Britta Simon hozzárendelése:**</span><span class="sxs-lookup"><span data-stu-id="97ac2-233">**To assign Britta Simon to Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="97ac2-234">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="97ac2-236">Az alkalmazások listában válassza ki a **nevezetesen**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-236">In the applications list, select **Namely**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="97ac2-238">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="97ac2-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="97ac2-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="97ac2-240">Click **Add** button.</span></span> <span data-ttu-id="97ac2-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97ac2-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="97ac2-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="97ac2-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="97ac2-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97ac2-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97ac2-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="97ac2-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="97ac2-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="97ac2-246">Testing single sign-on</span></span>

<span data-ttu-id="97ac2-247">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="97ac2-247">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="97ac2-248">Amikor rákattint az nevezetesen csempére a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett a nevezetesen alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="97ac2-248">When you click the Namely tile in the Access Panel, you should get automatically signed-on to your Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97ac2-249">További források</span><span class="sxs-lookup"><span data-stu-id="97ac2-249">Additional resources</span></span>

* [<span data-ttu-id="97ac2-250">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="97ac2-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97ac2-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="97ac2-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

