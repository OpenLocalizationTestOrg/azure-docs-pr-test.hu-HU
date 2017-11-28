---
title: "Oktatóanyag: Azure Active Directoryval integrált UNIFI |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és UNIFI között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 09074d4628825909f0bb961c8001e53fb06cf7c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="dabb2-103">Oktatóanyag: Azure Active Directoryval integrált UNIFI</span><span class="sxs-lookup"><span data-stu-id="dabb2-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="dabb2-104">Ebben az oktatóanyagban elsajátíthatja UNIFI integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dabb2-104">In this tutorial, you learn how to integrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dabb2-105">UNIFI integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dabb2-105">Integrating UNIFI with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dabb2-106">Megadhatja a UNIFI hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dabb2-106">You can control in Azure AD who has access to UNIFI</span></span>
- <span data-ttu-id="dabb2-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett UNIFI (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="dabb2-107">You can enable your users to automatically get signed-on to UNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dabb2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dabb2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dabb2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dabb2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dabb2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dabb2-110">Prerequisites</span></span>

<span data-ttu-id="dabb2-111">Az Azure AD-integrációs UNIFI konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="dabb2-111">To configure Azure AD integration with UNIFI, you need the following items:</span></span>

- <span data-ttu-id="dabb2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dabb2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dabb2-113">Egy UNIFI egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="dabb2-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dabb2-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dabb2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dabb2-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dabb2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dabb2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dabb2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dabb2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dabb2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dabb2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dabb2-118">Scenario description</span></span>
<span data-ttu-id="dabb2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dabb2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dabb2-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dabb2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dabb2-121">A gyűjteményből UNIFI hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dabb2-121">Adding UNIFI from the gallery</span></span>
2. <span data-ttu-id="dabb2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dabb2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-the-gallery"></a><span data-ttu-id="dabb2-123">A gyűjteményből UNIFI hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dabb2-123">Adding UNIFI from the gallery</span></span>
<span data-ttu-id="dabb2-124">Az Azure AD integrálása a UNIFI konfigurálásához kell hozzáadnia UNIFI a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dabb2-124">To configure the integration of UNIFI into Azure AD, you need to add UNIFI from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dabb2-125">**A gyűjteményből UNIFI hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dabb2-125">**To add UNIFI from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dabb2-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dabb2-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dabb2-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="dabb2-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="dabb2-133">Írja be a keresőmezőbe, **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-133">In the search box, type **UNIFI**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="dabb2-135">Az eredmények panelen válassza ki a **UNIFI**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dabb2-135">In the results panel, select **UNIFI**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dabb2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dabb2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dabb2-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UNIFI.</span><span class="sxs-lookup"><span data-stu-id="dabb2-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dabb2-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó UNIFI a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dabb2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in UNIFI is to a user in Azure AD.</span></span> <span data-ttu-id="dabb2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a UNIFI közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dabb2-140">In other words, a link relationship between an Azure AD user and the related user in UNIFI needs to be established.</span></span>

<span data-ttu-id="dabb2-141">UNIFI, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dabb2-141">In UNIFI, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dabb2-142">Az Azure AD egyszeri bejelentkezést a UNIFI tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dabb2-142">To configure and test Azure AD single sign-on with UNIFI, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dabb2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dabb2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dabb2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dabb2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dabb2-145">**[Tesztfelhasználó létrehozása egy UNIFI](#creating-a-unifi-test-user)**  - való egy megfelelője a Britta Simon UNIFI, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dabb2-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - to have a counterpart of Britta Simon in UNIFI that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dabb2-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dabb2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dabb2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dabb2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dabb2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dabb2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dabb2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az UNIFI alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dabb2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="dabb2-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés UNIFI, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dabb2-150">**To configure Azure AD single sign-on with UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="dabb2-151">Az Azure portálon a a **UNIFI** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-151">In the Azure portal, on the **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="dabb2-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dabb2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="dabb2-155">Az a **UNIFI tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="dabb2-155">On the **UNIFI Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="dabb2-157">Az a **azonosító** szövegmező, írja be az értéket:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="dabb2-157">In the **Identifier** textbox, type the value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="dabb2-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="dabb2-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="dabb2-160">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="dabb2-160">In the **Sign-on URL** textbox, type the URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="dabb2-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dabb2-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="dabb2-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="dabb2-165">A a **UNIFI konfigurációs** kattintson **konfigurálása UNIFI** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="dabb2-165">On the **UNIFI Configuration** section, click **Configure UNIFI** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dabb2-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="dabb2-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="dabb2-168">Egy másik webes böngészőablakban, jelentkezzen be a **UNIFI** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dabb2-168">In a different web browser window, sign on to your **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="dabb2-169">Kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-169">Click on the **Users**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="dabb2-171">Kattintson a **adja hozzá az új identitásszolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-171">Click on the **Add New Identity Provider**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="dabb2-173">Az a **identitásszolgáltató hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dabb2-173">In the **Add Identity Provider** section, perform the following steps:</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="dabb2-175">a.</span><span class="sxs-lookup"><span data-stu-id="dabb2-175">a.</span></span> <span data-ttu-id="dabb2-176">Az a **szolgáltatónevet** szövegmező, írja be az identitásszolgáltató nevét...</span><span class="sxs-lookup"><span data-stu-id="dabb2-176">In the **Provider Name** textbox, type the name of the Identity Provider..</span></span>

    <span data-ttu-id="dabb2-177">b.</span><span class="sxs-lookup"><span data-stu-id="dabb2-177">b.</span></span> <span data-ttu-id="dabb2-178">Az a a **szolgáltató URL-cím** szövegmező illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="dabb2-178">In the the **Provider URL** textbox paste the **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="dabb2-179">c.</span><span class="sxs-lookup"><span data-stu-id="dabb2-179">c.</span></span> <span data-ttu-id="dabb2-180">Nyissa meg a Jegyzettömbben, Azure-portálról letöltött tanúsítvány eltávolítása a **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a maradék tartalmat a **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="dabb2-180">Open the Certificate that you have downloaded from the Azure portal in notepad, remove the **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste the remaining content in the **Certificate** textbox.</span></span>

    <span data-ttu-id="dabb2-181">d.</span><span class="sxs-lookup"><span data-stu-id="dabb2-181">d.</span></span> <span data-ttu-id="dabb2-182">Válassza ki a **alapértelmezett szolgáltató** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="dabb2-182">Select the **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="dabb2-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dabb2-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dabb2-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dabb2-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dabb2-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dabb2-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dabb2-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dabb2-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="dabb2-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dabb2-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="dabb2-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dabb2-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dabb2-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dabb2-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dabb2-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="dabb2-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dabb2-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dabb2-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dabb2-198">a.</span><span class="sxs-lookup"><span data-stu-id="dabb2-198">a.</span></span> <span data-ttu-id="dabb2-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dabb2-200">b.</span><span class="sxs-lookup"><span data-stu-id="dabb2-200">b.</span></span> <span data-ttu-id="dabb2-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dabb2-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dabb2-202">c.</span><span class="sxs-lookup"><span data-stu-id="dabb2-202">c.</span></span> <span data-ttu-id="dabb2-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dabb2-204">d.</span><span class="sxs-lookup"><span data-stu-id="dabb2-204">d.</span></span> <span data-ttu-id="dabb2-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="dabb2-206">UNIFI tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dabb2-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="dabb2-207">Ebben a szakaszban egy Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dabb2-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="dabb2-208">**UNIFI** támogatja az automatikus felhasználólétesítés, így manuális lépésekre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="dabb2-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="dabb2-209">Sikeres hitelesítést az Azure ad-felhasználók jönnek létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="dabb2-209">Users are created automatically after successful authentication from the Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dabb2-210">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dabb2-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dabb2-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés UNIFI Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dabb2-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UNIFI.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="dabb2-213">**Britta Simon hozzárendelése UNIFI, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dabb2-213">**To assign Britta Simon to UNIFI, perform the following steps:**</span></span>

1. <span data-ttu-id="dabb2-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dabb2-216">Az alkalmazások listában válassza ki a **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-216">In the applications list, select **UNIFI**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="dabb2-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dabb2-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="dabb2-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dabb2-220">Click **Add** button.</span></span> <span data-ttu-id="dabb2-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dabb2-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="dabb2-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dabb2-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dabb2-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dabb2-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dabb2-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dabb2-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dabb2-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dabb2-226">Testing single sign-on</span></span>

<span data-ttu-id="dabb2-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dabb2-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dabb2-228">Ha a hozzáférési panelen UNIFI csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az UNIFI alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="dabb2-228">When you click the UNIFI tile in the Access Panel, you should get automatically signed-on to your UNIFI application.</span></span>
<span data-ttu-id="dabb2-229">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dabb2-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="dabb2-230">További források</span><span class="sxs-lookup"><span data-stu-id="dabb2-230">Additional resources</span></span>

* [<span data-ttu-id="dabb2-231">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dabb2-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dabb2-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dabb2-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

