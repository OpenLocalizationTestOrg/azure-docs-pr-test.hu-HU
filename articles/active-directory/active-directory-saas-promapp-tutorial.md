---
title: "Oktatóanyag: Azure Active Directoryval integrált Promapp |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Promapp között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 27013ca9724cf2f57fc85f5f4ccb71921ca57a3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a><span data-ttu-id="ed18d-103">Oktatóanyag: Azure Active Directoryval integrált Promapp</span><span class="sxs-lookup"><span data-stu-id="ed18d-103">Tutorial: Azure Active Directory integration with Promapp</span></span>

<span data-ttu-id="ed18d-104">Ebben az oktatóanyagban elsajátíthatja Promapp integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ed18d-104">In this tutorial, you learn how to integrate Promapp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ed18d-105">Promapp integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ed18d-105">Integrating Promapp with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ed18d-106">Megadhatja a Promapp hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ed18d-106">You can control in Azure AD who has access to Promapp</span></span>
- <span data-ttu-id="ed18d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Promapp (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ed18d-107">You can enable your users to automatically get signed-on to Promapp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ed18d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ed18d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ed18d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ed18d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed18d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ed18d-110">Prerequisites</span></span>

<span data-ttu-id="ed18d-111">Konfigurálása az Azure AD-integrációs Promapp, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ed18d-111">To configure Azure AD integration with Promapp, you need the following items:</span></span>

- <span data-ttu-id="ed18d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ed18d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ed18d-113">Egy Promapp egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ed18d-113">A Promapp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ed18d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ed18d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ed18d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ed18d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ed18d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ed18d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ed18d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ed18d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ed18d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ed18d-118">Scenario description</span></span>
<span data-ttu-id="ed18d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ed18d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ed18d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ed18d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ed18d-121">A gyűjteményből Promapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ed18d-121">Adding Promapp from the gallery</span></span>
2. <span data-ttu-id="ed18d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ed18d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-promapp-from-the-gallery"></a><span data-ttu-id="ed18d-123">A gyűjteményből Promapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ed18d-123">Adding Promapp from the gallery</span></span>
<span data-ttu-id="ed18d-124">Az Azure AD integrálása a Promapp konfigurálásához kell hozzáadnia Promapp a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ed18d-124">To configure the integration of Promapp into Azure AD, you need to add Promapp from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ed18d-125">**A gyűjteményből Promapp hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ed18d-125">**To add Promapp from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ed18d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ed18d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ed18d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ed18d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ed18d-133">Írja be a keresőmezőbe, **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-133">In the search box, type **Promapp**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. <span data-ttu-id="ed18d-135">Az eredmények panelen válassza ki a **Promapp**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ed18d-135">In the results panel, select **Promapp**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ed18d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ed18d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ed18d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Promapp.</span><span class="sxs-lookup"><span data-stu-id="ed18d-138">In this section, you configure and test Azure AD single sign-on with Promapp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ed18d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Promapp a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ed18d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Promapp is to a user in Azure AD.</span></span> <span data-ttu-id="ed18d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Promapp közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ed18d-140">In other words, a link relationship between an Azure AD user and the related user in Promapp needs to be established.</span></span>

<span data-ttu-id="ed18d-141">Promapp, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="ed18d-141">In Promapp, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ed18d-142">Az Azure AD egyszeri bejelentkezést a Promapp tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ed18d-142">To configure and test Azure AD single sign-on with Promapp, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ed18d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ed18d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ed18d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ed18d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ed18d-145">**[Promapp tesztfelhasználó létrehozása](#creating-a-promapp-test-user)**  - való Britta Simon valami Promapp, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ed18d-145">**[Creating a Promapp test user](#creating-a-promapp-test-user)** - to have a counterpart of Britta Simon in Promapp that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ed18d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ed18d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ed18d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ed18d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ed18d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ed18d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ed18d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Promapp alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ed18d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Promapp application.</span></span>

<span data-ttu-id="ed18d-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Promapp, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ed18d-150">**To configure Azure AD single sign-on with Promapp, perform the following steps:**</span></span>

1. <span data-ttu-id="ed18d-151">Az Azure portálon a a **Promapp** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-151">In the Azure portal, on the **Promapp** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ed18d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ed18d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. <span data-ttu-id="ed18d-155">Az a **Promapp tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ed18d-155">On the **Promapp Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    <span data-ttu-id="ed18d-157">a.</span><span class="sxs-lookup"><span data-stu-id="ed18d-157">a.</span></span> <span data-ttu-id="ed18d-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span><span class="sxs-lookup"><span data-stu-id="ed18d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span></span>

    <span data-ttu-id="ed18d-159">b.</span><span class="sxs-lookup"><span data-stu-id="ed18d-159">b.</span></span> <span data-ttu-id="ed18d-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://DOMAINNAME.promapp.com/TENANTNAME`</span><span class="sxs-lookup"><span data-stu-id="ed18d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ed18d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ed18d-161">These values are not real.</span></span> <span data-ttu-id="ed18d-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ed18d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ed18d-163">Ügyfél [Promapp ügyfél-támogatási csoport](https://www.promapp.com/about-us/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ed18d-163">Contact [Promapp Client support team](https://www.promapp.com/about-us/contact-us/) to get these values.</span></span>

4. <span data-ttu-id="ed18d-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ed18d-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

5. <span data-ttu-id="ed18d-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ed18d-168">A a **Promapp konfigurációs** kattintson **konfigurálása Promapp** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ed18d-168">On the **Promapp Configuration** section, click **Configure Promapp** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ed18d-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ed18d-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

7. <span data-ttu-id="ed18d-171">Bejelentkezés a Promapp vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ed18d-171">Sign-on to your Promapp company site as administrator.</span></span> 

8. <span data-ttu-id="ed18d-172">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-172">In the menu on the top, click **Admin**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][12]

9. <span data-ttu-id="ed18d-174">Kattintson a **Configure** (Konfigurálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="ed18d-174">Click **Configure**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][13]

10. <span data-ttu-id="ed18d-176">Az a **biztonsági** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ed18d-176">On the **Security** dialog, perform the following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][14]
    
    <span data-ttu-id="ed18d-178">a.</span><span class="sxs-lookup"><span data-stu-id="ed18d-178">a.</span></span> <span data-ttu-id="ed18d-179">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálról másolta a **SSO-bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ed18d-179">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal into the **SSO-Login URL** textbox.</span></span>
    
    <span data-ttu-id="ed18d-180">b.</span><span class="sxs-lookup"><span data-stu-id="ed18d-180">b.</span></span> <span data-ttu-id="ed18d-181">Mint **SSO - egyszeri bejelentkezés mód**, jelölje be **nem kötelező**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-181">As **SSO - Single Sign-on Mode**, select **Optional**, and then click **Save**.</span></span>

    <span data-ttu-id="ed18d-182">c.</span><span class="sxs-lookup"><span data-stu-id="ed18d-182">c.</span></span> <span data-ttu-id="ed18d-183">Nyissa meg a Jegyzettömbben a letöltött tanúsítvány tartalmának másolása a tanúsítvány nélkül az első sorban (---BEGIN CERTIFICATE---) és az utolsó sort (---vége tanúsítvány---), illessze be azt a **SSO-x.509 tanúsítvány** szövegmezőhöz és Kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-183">Open the downloaded certificate in notepad, copy the certificate content without the first line (-----BEGIN CERTIFICATE-----) and the last line (-----END CERTIFICATE-----), paste it into the **SSO-x.509 Certificate** textbox, and then click **Save**.</span></span>
        
> [!TIP]
> <span data-ttu-id="ed18d-184">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ed18d-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ed18d-185">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ed18d-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ed18d-186">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ed18d-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ed18d-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed18d-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="ed18d-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ed18d-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ed18d-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ed18d-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ed18d-191">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ed18d-193">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ed18d-195">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ed18d-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ed18d-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ed18d-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ed18d-199">a.</span><span class="sxs-lookup"><span data-stu-id="ed18d-199">a.</span></span> <span data-ttu-id="ed18d-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ed18d-201">b.</span><span class="sxs-lookup"><span data-stu-id="ed18d-201">b.</span></span> <span data-ttu-id="ed18d-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ed18d-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ed18d-203">c.</span><span class="sxs-lookup"><span data-stu-id="ed18d-203">c.</span></span> <span data-ttu-id="ed18d-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ed18d-205">d.</span><span class="sxs-lookup"><span data-stu-id="ed18d-205">d.</span></span> <span data-ttu-id="ed18d-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-206">Click **Create**.</span></span>
 
### <a name="creating-a-promapp-test-user"></a><span data-ttu-id="ed18d-207">Promapp tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed18d-207">Creating a Promapp test user</span></span>

<span data-ttu-id="ed18d-208">A Promapp alkalmazás támogatja-e közvetlenül időponthoz kötött kiépítés.</span><span class="sxs-lookup"><span data-stu-id="ed18d-208">The Promapp application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="ed18d-209">Ez azt jelenti, egy felhasználói fiók automatikusan létrejön szükség esetén az alkalmazás a hozzáférési panelen elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="ed18d-209">This means, a user account is automatically created if necessary during an attempt to access the application using the Access Panel.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ed18d-210">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ed18d-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ed18d-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Promapp Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ed18d-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Promapp.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ed18d-213">**Britta Simon hozzárendelése Promapp, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ed18d-213">**To assign Britta Simon to Promapp, perform the following steps:**</span></span>

1. <span data-ttu-id="ed18d-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ed18d-216">Az alkalmazások listában válassza ki a **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-216">In the applications list, select **Promapp**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. <span data-ttu-id="ed18d-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ed18d-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ed18d-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ed18d-220">Click **Add** button.</span></span> <span data-ttu-id="ed18d-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ed18d-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ed18d-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ed18d-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ed18d-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ed18d-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ed18d-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ed18d-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ed18d-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ed18d-226">Testing single sign-on</span></span>

<span data-ttu-id="ed18d-227">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ed18d-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ed18d-228">Ha a hozzáférési panelen Promapp csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Promapp alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="ed18d-228">When you click the Promapp tile in the Access Panel, you should get automatically signed-on to your Promapp application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed18d-229">További források</span><span class="sxs-lookup"><span data-stu-id="ed18d-229">Additional resources</span></span>

* [<span data-ttu-id="ed18d-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ed18d-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ed18d-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ed18d-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

