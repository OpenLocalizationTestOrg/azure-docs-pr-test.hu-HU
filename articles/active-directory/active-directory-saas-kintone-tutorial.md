---
title: "Oktatóanyag: Azure Active Directoryval integrált Kintone |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Kintone között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: e5e847c12cba3611ce7ea2c3e956dbd55b1e0cac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a><span data-ttu-id="667cd-103">Oktatóanyag: Azure Active Directoryval integrált Kintone</span><span class="sxs-lookup"><span data-stu-id="667cd-103">Tutorial: Azure Active Directory integration with Kintone</span></span>

<span data-ttu-id="667cd-104">Ebben az oktatóanyagban elsajátíthatja Kintone integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="667cd-104">In this tutorial, you learn how to integrate Kintone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="667cd-105">Kintone integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="667cd-105">Integrating Kintone with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="667cd-106">Megadhatja a Kintone hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="667cd-106">You can control in Azure AD who has access to Kintone</span></span>
- <span data-ttu-id="667cd-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Kintone (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="667cd-107">You can enable your users to automatically get signed-on to Kintone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="667cd-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="667cd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="667cd-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="667cd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="667cd-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="667cd-110">Prerequisites</span></span>

<span data-ttu-id="667cd-111">Konfigurálása az Azure AD-integrációs Kintone, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="667cd-111">To configure Azure AD integration with Kintone, you need the following items:</span></span>

- <span data-ttu-id="667cd-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="667cd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="667cd-113">Egy Kintone egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="667cd-113">A Kintone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="667cd-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="667cd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="667cd-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="667cd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="667cd-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="667cd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="667cd-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="667cd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="667cd-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="667cd-118">Scenario description</span></span>
<span data-ttu-id="667cd-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="667cd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="667cd-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="667cd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="667cd-121">A gyűjteményből Kintone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="667cd-121">Adding Kintone from the gallery</span></span>
2. <span data-ttu-id="667cd-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="667cd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kintone-from-the-gallery"></a><span data-ttu-id="667cd-123">A gyűjteményből Kintone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="667cd-123">Adding Kintone from the gallery</span></span>
<span data-ttu-id="667cd-124">Az Azure AD integrálása a Kintone konfigurálásához kell hozzáadnia Kintone a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="667cd-124">To configure the integration of Kintone into Azure AD, you need to add Kintone from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="667cd-125">**A gyűjteményből Kintone hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="667cd-125">**To add Kintone from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="667cd-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="667cd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="667cd-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="667cd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="667cd-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="667cd-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="667cd-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="667cd-133">Írja be a keresőmezőbe, **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="667cd-133">In the search box, type **Kintone**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. <span data-ttu-id="667cd-135">Az eredmények panelen válassza ki a **Kintone**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="667cd-135">In the results panel, select **Kintone**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="667cd-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="667cd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="667cd-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kintone.</span><span class="sxs-lookup"><span data-stu-id="667cd-138">In this section, you configure and test Azure AD single sign-on with Kintone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="667cd-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kintone a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="667cd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kintone is to a user in Azure AD.</span></span> <span data-ttu-id="667cd-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Kintone közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="667cd-140">In other words, a link relationship between an Azure AD user and the related user in Kintone needs to be established.</span></span>

<span data-ttu-id="667cd-141">Kintone, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="667cd-141">In Kintone, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="667cd-142">Az Azure AD egyszeri bejelentkezést a Kintone tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="667cd-142">To configure and test Azure AD single sign-on with Kintone, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="667cd-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="667cd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="667cd-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="667cd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="667cd-145">**[Kintone tesztfelhasználó létrehozása](#creating-a-kintone-test-user)**  - való Britta Simon valami Kintone, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="667cd-145">**[Creating a Kintone test user](#creating-a-kintone-test-user)** - to have a counterpart of Britta Simon in Kintone that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="667cd-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="667cd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="667cd-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="667cd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="667cd-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="667cd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="667cd-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Kintone alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="667cd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kintone application.</span></span>

<span data-ttu-id="667cd-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Kintone, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="667cd-150">**To configure Azure AD single sign-on with Kintone, perform the following steps:**</span></span>

1. <span data-ttu-id="667cd-151">Az Azure portálon a a **Kintone** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="667cd-151">In the Azure portal, on the **Kintone** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="667cd-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="667cd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. <span data-ttu-id="667cd-155">Az a **Kintone tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="667cd-155">On the **Kintone Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    <span data-ttu-id="667cd-157">a.</span><span class="sxs-lookup"><span data-stu-id="667cd-157">a.</span></span> <span data-ttu-id="667cd-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.kintone.com`</span><span class="sxs-lookup"><span data-stu-id="667cd-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.kintone.com`</span></span>

    <span data-ttu-id="667cd-159">b.</span><span class="sxs-lookup"><span data-stu-id="667cd-159">b.</span></span> <span data-ttu-id="667cd-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="667cd-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > <span data-ttu-id="667cd-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="667cd-161">These values are not real.</span></span> <span data-ttu-id="667cd-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="667cd-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="667cd-163">Ügyfél [Kintone ügyfél-támogatási csoport](https://www.kintone.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="667cd-163">Contact [Kintone Client support team](https://www.kintone.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="667cd-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="667cd-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. <span data-ttu-id="667cd-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="667cd-168">A a **Kintone konfigurációs** kattintson **konfigurálása Kintone** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="667cd-168">On the **Kintone Configuration** section, click **Configure Kintone** to open **Configure sign-on** window.</span></span> <span data-ttu-id="667cd-169">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="667cd-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. <span data-ttu-id="667cd-171">Egy másik webes böngészőablakban, jelentkezzen be a **Kintone** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="667cd-171">In a different web browser window, log into your **Kintone** company site as an administrator.</span></span>

8. <span data-ttu-id="667cd-172">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="667cd-172">Click **Settings**.</span></span>
   
    <span data-ttu-id="667cd-173">![Beállítások](./media/active-directory-saas-kintone-tutorial/ic785879.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="667cd-173">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

9. <span data-ttu-id="667cd-174">Kattintson a **felhasználók és rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="667cd-174">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="667cd-175">![Felhasználók és rendszergazdák](./media/active-directory-saas-kintone-tutorial/ic785880.png "felhasználók és rendszergazdák")</span><span class="sxs-lookup"><span data-stu-id="667cd-175">![Users & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "Users & System Administration")</span></span>

10. <span data-ttu-id="667cd-176">A **rendszerfelügyelet \> biztonsági** kattintson **bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="667cd-176">Under **System Administration \> Security** click **Login**.</span></span>
   
    <span data-ttu-id="667cd-177">![Bejelentkezési](./media/active-directory-saas-kintone-tutorial/ic785881.png "bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="667cd-177">![Login](./media/active-directory-saas-kintone-tutorial/ic785881.png "Login")</span></span>

11. <span data-ttu-id="667cd-178">Kattintson a **engedélyezése SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="667cd-178">Click **Enable SAML authentication**.</span></span>
   
    <span data-ttu-id="667cd-179">![SAML-alapú hitelesítés](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="667cd-179">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML Authentication")</span></span>

12. <span data-ttu-id="667cd-180">A SAML-alapú hitelesítés területen a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="667cd-180">In the SAML Authentication section, perform the following steps:</span></span>
    
    <span data-ttu-id="667cd-181">![SAML-alapú hitelesítés](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="667cd-181">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML Authentication")</span></span>
    
    <span data-ttu-id="667cd-182">a.</span><span class="sxs-lookup"><span data-stu-id="667cd-182">a.</span></span> <span data-ttu-id="667cd-183">Az a **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="667cd-183">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="667cd-184">b.</span><span class="sxs-lookup"><span data-stu-id="667cd-184">b.</span></span> <span data-ttu-id="667cd-185">Az a **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="667cd-185">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="667cd-186">c.</span><span class="sxs-lookup"><span data-stu-id="667cd-186">c.</span></span> <span data-ttu-id="667cd-187">Kattintson a **Tallózás** a letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="667cd-187">Click **Browse** to upload your downloaded certificate.</span></span>
    
    <span data-ttu-id="667cd-188">d.</span><span class="sxs-lookup"><span data-stu-id="667cd-188">d.</span></span> <span data-ttu-id="667cd-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="667cd-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="667cd-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="667cd-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="667cd-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="667cd-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="667cd-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="667cd-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="667cd-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="667cd-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="667cd-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="667cd-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="667cd-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="667cd-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="667cd-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="667cd-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="667cd-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="667cd-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="667cd-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="667cd-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="667cd-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="667cd-205">a.</span><span class="sxs-lookup"><span data-stu-id="667cd-205">a.</span></span> <span data-ttu-id="667cd-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="667cd-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="667cd-207">b.</span><span class="sxs-lookup"><span data-stu-id="667cd-207">b.</span></span> <span data-ttu-id="667cd-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="667cd-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="667cd-209">c.</span><span class="sxs-lookup"><span data-stu-id="667cd-209">c.</span></span> <span data-ttu-id="667cd-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="667cd-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="667cd-211">d.</span><span class="sxs-lookup"><span data-stu-id="667cd-211">d.</span></span> <span data-ttu-id="667cd-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-212">Click **Create**.</span></span>
 
### <a name="creating-a-kintone-test-user"></a><span data-ttu-id="667cd-213">Kintone tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="667cd-213">Creating a Kintone test user</span></span>

<span data-ttu-id="667cd-214">Ahhoz, hogy az Azure AD-felhasználók Kintone bejelentkezni, akkor ki kell építenie a Kintone.</span><span class="sxs-lookup"><span data-stu-id="667cd-214">To enable Azure AD users to log in to Kintone, they must be provisioned into Kintone.</span></span>  
<span data-ttu-id="667cd-215">Kintone, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="667cd-215">In the case of Kintone, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="667cd-216">Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="667cd-216">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="667cd-217">Jelentkezzen be a **Kintone** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="667cd-217">Log in to your **Kintone** company site as an administrator.</span></span>

2. <span data-ttu-id="667cd-218">Kattintson a **beállítás**.</span><span class="sxs-lookup"><span data-stu-id="667cd-218">Click **Setting**.</span></span>
   
    <span data-ttu-id="667cd-219">![Beállítások](./media/active-directory-saas-kintone-tutorial/ic785879.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="667cd-219">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

3. <span data-ttu-id="667cd-220">Kattintson a **felhasználók és rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="667cd-220">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="667cd-221">![& Felhasználói rendszerfelügyelet](./media/active-directory-saas-kintone-tutorial/ic785880.png "felhasználó & rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="667cd-221">![User & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "User & System Administration")</span></span>

4. <span data-ttu-id="667cd-222">A **felhasználói felügyeleti**, kattintson a **részlegek & felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="667cd-222">Under **User Administration**, click **Departments & Users**.</span></span>
   
    <span data-ttu-id="667cd-223">![Részleg és a felhasználók](./media/active-directory-saas-kintone-tutorial/ic785888.png "részleg és a felhasználók")</span><span class="sxs-lookup"><span data-stu-id="667cd-223">![Department & Users](./media/active-directory-saas-kintone-tutorial/ic785888.png "Department & Users")</span></span>

5. <span data-ttu-id="667cd-224">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="667cd-224">Click **New User**.</span></span>
   
    <span data-ttu-id="667cd-225">![Új felhasználók](./media/active-directory-saas-kintone-tutorial/ic785889.png "új felhasználók")</span><span class="sxs-lookup"><span data-stu-id="667cd-225">![New Users](./media/active-directory-saas-kintone-tutorial/ic785889.png "New Users")</span></span>

6. <span data-ttu-id="667cd-226">Az a **új felhasználó** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="667cd-226">In the **New User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="667cd-227">![Új felhasználók](./media/active-directory-saas-kintone-tutorial/ic785890.png "új felhasználók")</span><span class="sxs-lookup"><span data-stu-id="667cd-227">![New Users](./media/active-directory-saas-kintone-tutorial/ic785890.png "New Users")</span></span>
   
    <span data-ttu-id="667cd-228">a.</span><span class="sxs-lookup"><span data-stu-id="667cd-228">a.</span></span> <span data-ttu-id="667cd-229">Adjon meg egy **megjelenítendő név**, **bejelentkezési név**, **új jelszó**, **jelszó megerősítése**, **E-mail cím**, és egy érvényes AAD-fiókba más részleteit szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="667cd-229">Type a **Display Name**, **Login Name**, **New Password**, **Confirm Password**, **E-mail Address**, and other details of a valid AAD account you want to provision into the related textboxes.</span></span>
 
    <span data-ttu-id="667cd-230">b.</span><span class="sxs-lookup"><span data-stu-id="667cd-230">b.</span></span> <span data-ttu-id="667cd-231">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-231">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="667cd-232">Bármely más Kintone felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Kintone által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="667cd-232">You can use any other Kintone user account creation tools or APIs provided by Kintone to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="667cd-233">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="667cd-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="667cd-234">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kintone Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="667cd-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kintone.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="667cd-236">**Britta Simon hozzárendelése Kintone, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="667cd-236">**To assign Britta Simon to Kintone, perform the following steps:**</span></span>

1. <span data-ttu-id="667cd-237">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="667cd-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="667cd-239">Az alkalmazások listában válassza ki a **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="667cd-239">In the applications list, select **Kintone**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. <span data-ttu-id="667cd-241">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="667cd-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="667cd-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="667cd-243">Click **Add** button.</span></span> <span data-ttu-id="667cd-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="667cd-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="667cd-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="667cd-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="667cd-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="667cd-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="667cd-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="667cd-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="667cd-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="667cd-249">Testing single sign-on</span></span>

<span data-ttu-id="667cd-250">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="667cd-250">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="667cd-251">Ha a hozzáférési panelen Kintone csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Kintone alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="667cd-251">When you click the Kintone tile in the Access Panel, you should get automatically signed-on to your Kintone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="667cd-252">További források</span><span class="sxs-lookup"><span data-stu-id="667cd-252">Additional resources</span></span>

* [<span data-ttu-id="667cd-253">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="667cd-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="667cd-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="667cd-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

