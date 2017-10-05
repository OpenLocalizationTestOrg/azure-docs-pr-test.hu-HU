---
title: "Oktatóanyag: Azure Active Directory-integráció Igloo szoftverrel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Igloo szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ab3891e11eb33b4d233e4fc967a40c7df06e4f4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="9d441-103">Oktatóanyag: Azure Active Directory-integráció Igloo szoftverrel</span><span class="sxs-lookup"><span data-stu-id="9d441-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="9d441-104">Ebben az oktatóanyagban elsajátíthatja Igloo szoftver integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9d441-104">In this tutorial, you learn how to integrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9d441-105">Igloo szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9d441-105">Integrating Igloo Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9d441-106">Szabályozhatja az Azure AD, aki hozzáfér Igloo szoftver</span><span class="sxs-lookup"><span data-stu-id="9d441-106">You can control in Azure AD who has access to Igloo Software</span></span>
- <span data-ttu-id="9d441-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Igloo szoftverre (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9d441-107">You can enable your users to automatically get signed-on to Igloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9d441-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9d441-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9d441-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9d441-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d441-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d441-110">Prerequisites</span></span>

<span data-ttu-id="9d441-111">Az Azure AD-integráció konfigurálása Igloo szoftverrel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9d441-111">To configure Azure AD integration with Igloo Software, you need the following items:</span></span>

- <span data-ttu-id="9d441-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9d441-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9d441-113">Egy Igloo szoftver egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9d441-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9d441-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9d441-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9d441-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9d441-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9d441-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9d441-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9d441-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9d441-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9d441-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9d441-118">Scenario description</span></span>
<span data-ttu-id="9d441-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9d441-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9d441-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9d441-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9d441-121">A gyűjteményből Igloo szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d441-121">Adding Igloo Software from the gallery</span></span>
2. <span data-ttu-id="9d441-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9d441-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-the-gallery"></a><span data-ttu-id="9d441-123">A gyűjteményből Igloo szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d441-123">Adding Igloo Software from the gallery</span></span>
<span data-ttu-id="9d441-124">Az Azure AD integrálása a Igloo szoftver konfigurálásához kell hozzáadnia Igloo szoftver a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9d441-124">To configure the integration of Igloo Software into Azure AD, you need to add Igloo Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9d441-125">**Igloo hozzáadása a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d441-125">**To add Igloo Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9d441-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d441-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9d441-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9d441-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9d441-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9d441-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9d441-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9d441-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9d441-133">Írja be a keresőmezőbe, **Igloo szoftver**.</span><span class="sxs-lookup"><span data-stu-id="9d441-133">In the search box, type **Igloo Software**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="9d441-135">Az eredmények panelen válassza ki a **Igloo szoftver**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9d441-135">In the results panel, select **Igloo Software**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9d441-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9d441-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9d441-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Igloo szoftverrel.</span><span class="sxs-lookup"><span data-stu-id="9d441-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9d441-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Igloo szoftver a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9d441-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Igloo Software is to a user in Azure AD.</span></span> <span data-ttu-id="9d441-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Igloo szoftver közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9d441-140">In other words, a link relationship between an Azure AD user and the related user in Igloo Software needs to be established.</span></span>

<span data-ttu-id="9d441-141">Igloo szoftver, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9d441-141">In Igloo Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9d441-142">Az Azure AD az egyszeri bejelentkezés Igloo szoftverrel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9d441-142">To configure and test Azure AD single sign-on with Igloo Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9d441-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9d441-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9d441-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9d441-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9d441-145">**[Egy Igloo szoftver tesztfelhasználó létrehozása](#creating-an-igloo-software-test-user)**  - való egy megfelelője a Britta Simon Igloo szoftver, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9d441-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - to have a counterpart of Britta Simon in Igloo Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9d441-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9d441-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9d441-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9d441-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9d441-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d441-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9d441-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Igloo alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9d441-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="9d441-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Igloo szoftverrel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d441-150">**To configure Azure AD single sign-on with Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="9d441-151">Az Azure portálon a a **Igloo szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9d441-151">In the Azure portal, on the **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9d441-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9d441-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="9d441-155">Az a **Igloo szoftver tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9d441-155">On the **Igloo Software Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="9d441-157">a.</span><span class="sxs-lookup"><span data-stu-id="9d441-157">a.</span></span> <span data-ttu-id="9d441-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="9d441-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="9d441-159">b.</span><span class="sxs-lookup"><span data-stu-id="9d441-159">b.</span></span> <span data-ttu-id="9d441-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="9d441-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="9d441-161">c.</span><span class="sxs-lookup"><span data-stu-id="9d441-161">c.</span></span> <span data-ttu-id="9d441-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="9d441-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9d441-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9d441-163">These values are not real.</span></span> <span data-ttu-id="9d441-164">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="9d441-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="9d441-165">Ügyfél [Igloo szoftver ügyfél-támogatási csoport](https://www.igloosoftware.com/services/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9d441-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) to get these values.</span></span> 

4. <span data-ttu-id="9d441-166">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9d441-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="9d441-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d441-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="9d441-170">A a **Igloo szoftverkonfigurációt** kattintson **Igloo szoftver konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9d441-170">On the **Igloo Software Configuration** section, click **Configure Igloo Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9d441-171">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9d441-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="9d441-173">Egy másik webes böngészőablakban jelentkezzen be a Igloo szoftver vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9d441-173">In a different web browser window, log in to your Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="9d441-174">Lépjen a **vezérlőpultot**.</span><span class="sxs-lookup"><span data-stu-id="9d441-174">Go to the **Control Panel**.</span></span>
   
     <span data-ttu-id="9d441-175">![Vezérlőpult](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "vezérlőpultot")</span><span class="sxs-lookup"><span data-stu-id="9d441-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="9d441-176">Az a **tagsági** lapra, majd **beállításaival bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="9d441-176">In the **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="9d441-177">![Jelentkezzen be a beállítások](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "beállítások bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="9d441-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="9d441-178">A SAML-alapú konfigurációs szakaszban kattintson **SAML-alapú hitelesítés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="9d441-178">In the SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="9d441-179">![SAML-alapú konfigurációs](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="9d441-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="9d441-180">Az a **általános konfigurációs** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9d441-180">In the **General Configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="9d441-181">![Általános konfiguráció](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "általános konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="9d441-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="9d441-182">a.</span><span class="sxs-lookup"><span data-stu-id="9d441-182">a.</span></span> <span data-ttu-id="9d441-183">A a **kapcsolatnév** szövegmező, írjon be egy egyéni nevet a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="9d441-183">In the **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="9d441-184">b.</span><span class="sxs-lookup"><span data-stu-id="9d441-184">b.</span></span> <span data-ttu-id="9d441-185">Az a **IdP bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9d441-185">In the **IdP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="9d441-186">c.</span><span class="sxs-lookup"><span data-stu-id="9d441-186">c.</span></span> <span data-ttu-id="9d441-187">Az a **IdP kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9d441-187">In the **IdP Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="9d441-188">d.</span><span class="sxs-lookup"><span data-stu-id="9d441-188">d.</span></span> <span data-ttu-id="9d441-189">Válassza ki **kijelentkezési válasz és kérelem HTTP típusú** , **POST**.</span><span class="sxs-lookup"><span data-stu-id="9d441-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="9d441-190">e.</span><span class="sxs-lookup"><span data-stu-id="9d441-190">e.</span></span> <span data-ttu-id="9d441-191">Nyissa meg a **base-64** kódolt tanúsítvány a Jegyzettömbben az Azure portálról letöltött, annak tartalmának másolása a vágólapra és illessze be azt a **nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9d441-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="9d441-192">Az a **válasz és a hitelesítési beállításokat**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d441-192">In the **Response and Authentication Configuration**, perform the following steps:</span></span>
    
    <span data-ttu-id="9d441-193">![Válasz és a hitelesítési beállításokat](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "válasz és hitelesítés konfigurációját.")</span><span class="sxs-lookup"><span data-stu-id="9d441-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="9d441-194">a.</span><span class="sxs-lookup"><span data-stu-id="9d441-194">a.</span></span> <span data-ttu-id="9d441-195">Mint **identitásszolgáltató**, jelölje be **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="9d441-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="9d441-196">b.</span><span class="sxs-lookup"><span data-stu-id="9d441-196">b.</span></span> <span data-ttu-id="9d441-197">Mint **azonosítótípusa**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="9d441-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="9d441-198">c.</span><span class="sxs-lookup"><span data-stu-id="9d441-198">c.</span></span> <span data-ttu-id="9d441-199">Az a **E-mail attribútum** szövegmezőhöz típus **emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="9d441-199">In the **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="9d441-200">d.</span><span class="sxs-lookup"><span data-stu-id="9d441-200">d.</span></span> <span data-ttu-id="9d441-201">Az a **Keresztnév attribútum** szövegmezőhöz típus **givenname**.</span><span class="sxs-lookup"><span data-stu-id="9d441-201">In the **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="9d441-202">e.</span><span class="sxs-lookup"><span data-stu-id="9d441-202">e.</span></span> <span data-ttu-id="9d441-203">Az a **utolsó Name attribútum** szövegmezőhöz típus **vezetékneve**.</span><span class="sxs-lookup"><span data-stu-id="9d441-203">In the **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="9d441-204">Hajtsa végre az alábbi lépéseket a konfigurálás befejezéséhez:</span><span class="sxs-lookup"><span data-stu-id="9d441-204">Perform the following steps to complete the configuration:</span></span>
    
    <span data-ttu-id="9d441-205">![Jelentkezzen be a felhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "jelentkezzen be a felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="9d441-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="9d441-206">a.</span><span class="sxs-lookup"><span data-stu-id="9d441-206">a.</span></span> <span data-ttu-id="9d441-207">Mint **jelentkezzen be a felhasználó létrehozása**, jelölje be **a hely új felhasználó létrehozása, amikor bejelentkeznek a**.</span><span class="sxs-lookup"><span data-stu-id="9d441-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="9d441-208">b.</span><span class="sxs-lookup"><span data-stu-id="9d441-208">b.</span></span> <span data-ttu-id="9d441-209">Mint **beállítások bejelentkezés**, jelölje be **használható SAML gomb a "Bejelentkezés" képernyőn**.</span><span class="sxs-lookup"><span data-stu-id="9d441-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="9d441-210">c.</span><span class="sxs-lookup"><span data-stu-id="9d441-210">c.</span></span> <span data-ttu-id="9d441-211">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d441-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="9d441-212">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9d441-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9d441-213">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9d441-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9d441-214">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9d441-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9d441-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d441-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="9d441-216">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9d441-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9d441-218">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d441-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9d441-219">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d441-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9d441-221">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9d441-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9d441-223">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9d441-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9d441-225">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9d441-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9d441-227">a.</span><span class="sxs-lookup"><span data-stu-id="9d441-227">a.</span></span> <span data-ttu-id="9d441-228">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9d441-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9d441-229">b.</span><span class="sxs-lookup"><span data-stu-id="9d441-229">b.</span></span> <span data-ttu-id="9d441-230">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9d441-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9d441-231">c.</span><span class="sxs-lookup"><span data-stu-id="9d441-231">c.</span></span> <span data-ttu-id="9d441-232">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9d441-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9d441-233">d.</span><span class="sxs-lookup"><span data-stu-id="9d441-233">d.</span></span> <span data-ttu-id="9d441-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d441-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="9d441-235">Egy Igloo szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d441-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="9d441-236">Nincs művelet elem ahhoz, hogy a felhasználók átadása Igloo szoftver konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9d441-236">There is no action item for you to configure user provisioning to Igloo Software.</span></span>  

<span data-ttu-id="9d441-237">Ha egy hozzárendelt felhasználó megpróbál bejelentkezni a hozzáférési panelen Igloo szoftver, a Igloo szoftver ellenőrzi, hogy a felhasználó létezik-e.</span><span class="sxs-lookup"><span data-stu-id="9d441-237">When an assigned user tries to log in to Igloo Software using the access panel, Igloo Software checks whether the user exists.</span></span>  <span data-ttu-id="9d441-238">Ha nincs felhasználói fiók elérhető még, automatikusan létrejön Igloo szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="9d441-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9d441-239">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9d441-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9d441-240">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés Igloo szoftver használatára.</span><span class="sxs-lookup"><span data-stu-id="9d441-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Igloo Software.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9d441-242">**Britta Simon hozzárendelése Igloo szoftver, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d441-242">**To assign Britta Simon to Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="9d441-243">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9d441-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9d441-245">Az alkalmazások listában válassza ki a **Igloo szoftver**.</span><span class="sxs-lookup"><span data-stu-id="9d441-245">In the applications list, select **Igloo Software**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="9d441-247">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9d441-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9d441-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d441-249">Click **Add** button.</span></span> <span data-ttu-id="9d441-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d441-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9d441-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9d441-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9d441-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d441-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9d441-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d441-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9d441-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9d441-255">Testing single sign-on</span></span>

<span data-ttu-id="9d441-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9d441-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9d441-257">Ha a hozzáférési panelen Igloo szoftver csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Igloo szoftverfrissítések alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9d441-257">When you click the Igloo Software tile in the Access Panel, you should get automatically signed-on to your Igloo Software application.</span></span>
<span data-ttu-id="9d441-258">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9d441-258">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9d441-259">További források</span><span class="sxs-lookup"><span data-stu-id="9d441-259">Additional resources</span></span>

* [<span data-ttu-id="9d441-260">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9d441-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9d441-261">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9d441-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

