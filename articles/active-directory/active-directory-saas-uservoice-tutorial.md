---
title: "Oktatóanyag: Azure Active Directoryval integrált UserVoice |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a UserVoice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: fcfda1c2ecb162fb93b70574a18bd745b72ee4db
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a><span data-ttu-id="6809e-103">Oktatóanyag: Azure Active Directoryval integrált UserVoice</span><span class="sxs-lookup"><span data-stu-id="6809e-103">Tutorial: Azure Active Directory integration with UserVoice</span></span>

<span data-ttu-id="6809e-104">Ebben az oktatóanyagban elsajátíthatja UserVoice integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6809e-104">In this tutorial, you learn how to integrate UserVoice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6809e-105">UserVoice integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6809e-105">Integrating UserVoice with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6809e-106">Az Azure AD, aki hozzáfér UserVoice szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="6809e-106">You can control in Azure AD who has access to UserVoice.</span></span>
- <span data-ttu-id="6809e-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a UserVoice (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="6809e-107">You can enable your users to automatically get signed-on to UserVoice (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6809e-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="6809e-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6809e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6809e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6809e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6809e-110">Prerequisites</span></span>

<span data-ttu-id="6809e-111">Az Azure AD-integráció konfigurálása a UserVoice, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6809e-111">To configure Azure AD integration with UserVoice, you need the following items:</span></span>

- <span data-ttu-id="6809e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6809e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6809e-113">A UserVoice egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6809e-113">A UserVoice single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6809e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6809e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6809e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6809e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6809e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6809e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6809e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6809e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6809e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6809e-118">Scenario description</span></span>
<span data-ttu-id="6809e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6809e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6809e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6809e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6809e-121">UserVoice hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6809e-121">Adding UserVoice from the gallery</span></span>
2. <span data-ttu-id="6809e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6809e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-uservoice-from-the-gallery"></a><span data-ttu-id="6809e-123">UserVoice hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="6809e-123">Adding UserVoice from the gallery</span></span>
<span data-ttu-id="6809e-124">Az Azure AD integrálása a UserVoice konfigurálásához kell hozzáadnia UserVoice a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6809e-124">To configure the integration of UserVoice into Azure AD, you need to add UserVoice from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6809e-125">**Adja hozzá a UserVoice a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6809e-125">**To add UserVoice from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6809e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6809e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="6809e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6809e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6809e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6809e-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="6809e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6809e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="6809e-133">Írja be a keresőmezőbe, **UserVoice**, jelölje be **UserVoice** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6809e-133">In the search box, type **UserVoice**, select **UserVoice** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6809e-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6809e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6809e-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UserVoice.</span><span class="sxs-lookup"><span data-stu-id="6809e-136">In this section, you configure and test Azure AD single sign-on with UserVoice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6809e-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a UserVoice tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6809e-137">For single sign-on to work, Azure AD needs to know what the counterpart user in UserVoice is to a user in Azure AD.</span></span> <span data-ttu-id="6809e-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a UserVoice közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6809e-138">In other words, a link relationship between an Azure AD user and the related user in UserVoice needs to be established.</span></span>

<span data-ttu-id="6809e-139">A UserVoice, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6809e-139">In UserVoice, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6809e-140">Az Azure AD egyszeri bejelentkezést a UserVoice tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6809e-140">To configure and test Azure AD single sign-on with UserVoice, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6809e-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6809e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6809e-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6809e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6809e-143">**[UserVoice tesztfelhasználó létrehozása](#create-a-uservoice-test-user)**  - való Britta Simon egy megfelelője a UserVoice, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6809e-143">**[Create a UserVoice test user](#create-a-uservoice-test-user)** - to have a counterpart of Britta Simon in UserVoice that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6809e-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6809e-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6809e-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6809e-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6809e-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6809e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6809e-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a UserVoice-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6809e-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your UserVoice application.</span></span>

<span data-ttu-id="6809e-148">**Konfigurálja az Azure AD egyszeri bejelentkezést a UserVoice, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6809e-148">**To configure Azure AD single sign-on with UserVoice, perform the following steps:**</span></span>

1. <span data-ttu-id="6809e-149">Az Azure portálon a a **UserVoice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6809e-149">In the Azure portal, on the **UserVoice** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="6809e-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6809e-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. <span data-ttu-id="6809e-153">Az a **UserVoice tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6809e-153">On the **UserVoice Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információt UserVoice tartomány és az URL-címek](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    <span data-ttu-id="6809e-155">a.</span><span class="sxs-lookup"><span data-stu-id="6809e-155">a.</span></span> <span data-ttu-id="6809e-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="6809e-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    <span data-ttu-id="6809e-157">b.</span><span class="sxs-lookup"><span data-stu-id="6809e-157">b.</span></span> <span data-ttu-id="6809e-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="6809e-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6809e-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6809e-159">These values are not real.</span></span> <span data-ttu-id="6809e-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6809e-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6809e-161">Ügyfél [UserVoice ügyfél-támogatási csoport](https://www.uservoice.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6809e-161">Contact [UserVoice Client support team](https://www.uservoice.com/) to get these values.</span></span>

4. <span data-ttu-id="6809e-162">Az a **SAML-aláíró tanúsítványa** szakaszban, másolja a **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="6809e-162">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. <span data-ttu-id="6809e-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6809e-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6809e-166">A a **UserVoice konfigurációs** kattintson **konfigurálása UserVoice** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6809e-166">On the **UserVoice Configuration** section, click **Configure UserVoice** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6809e-167">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6809e-167">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![UserVoice-konfiguráció](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. <span data-ttu-id="6809e-169">Egy másik webes böngészőablakban jelentkezzen be a UserVoice vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6809e-169">In a different web browser window, log in to your UserVoice company site as an administrator.</span></span>

8. <span data-ttu-id="6809e-170">A felső eszköztáron kattintson **beállítások**, majd válassza ki **webes portál** a menüből.</span><span class="sxs-lookup"><span data-stu-id="6809e-170">In the toolbar on the top, click **Settings**, and then select **Web portal** from the menu.</span></span>
   
    <span data-ttu-id="6809e-171">![Az alkalmazás ügyféloldali beállítások szakaszban](./media/active-directory-saas-uservoice-tutorial/ic777519.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="6809e-171">![Settings Section On App Side](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Settings")</span></span>

9. <span data-ttu-id="6809e-172">A a **webes portál** lap a **felhasználóhitelesítés** területen kattintson **szerkesztése** megnyitásához a **felhasználói hitelesítés szerkesztése** párbeszédpanel lap.</span><span class="sxs-lookup"><span data-stu-id="6809e-172">On the **Web portal** tab, in the **User authentication** section, click **Edit** to open the **Edit User Authentication** dialog page.</span></span>
   
    <span data-ttu-id="6809e-173">![Webes portál lapon](./media/active-directory-saas-uservoice-tutorial/ic777520.png "webes portál")</span><span class="sxs-lookup"><span data-stu-id="6809e-173">![Web portal Tab](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portal")</span></span>

10. <span data-ttu-id="6809e-174">Az a **felhasználói hitelesítés szerkesztése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6809e-174">On the **Edit User Authentication** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="6809e-175">![Felhasználói hitelesítés szerkesztése](./media/active-directory-saas-uservoice-tutorial/ic777521.png "felhasználói hitelesítés szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="6809e-175">![Edit user authentication](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Edit user authentication")</span></span>
   
    <span data-ttu-id="6809e-176">a.</span><span class="sxs-lookup"><span data-stu-id="6809e-176">a.</span></span> <span data-ttu-id="6809e-177">Kattintson a **egyszeri bejelentkezés (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="6809e-177">Click **Single Sign-On (SSO)**.</span></span>
 
    <span data-ttu-id="6809e-178">b.</span><span class="sxs-lookup"><span data-stu-id="6809e-178">b.</span></span> <span data-ttu-id="6809e-179">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **SSO távoli bejelentkezés** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6809e-179">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **SSO Remote Sign-In** textbox.</span></span>

    <span data-ttu-id="6809e-180">c.</span><span class="sxs-lookup"><span data-stu-id="6809e-180">c.</span></span> <span data-ttu-id="6809e-181">Beillesztés a **Sign-Out URL-cím** értéket, amely az Azure-portálról másolta a **SSO távoli Sign-Out szövegmező**.</span><span class="sxs-lookup"><span data-stu-id="6809e-181">Paste the **Sign-Out URL** value, which you have copied from the Azure portal into the **SSO Remote Sign-Out textbox**.</span></span>
 
    <span data-ttu-id="6809e-182">d.</span><span class="sxs-lookup"><span data-stu-id="6809e-182">d.</span></span> <span data-ttu-id="6809e-183">Beillesztés a **ujjlenyomat** értéket, amely az Azure portálról másolta a **aktuális SHA1 tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6809e-183">Paste the **Thumbprint** value , which you have copied from Azure portal  into the **Current certificate SHA1 fingerprint** textbox.</span></span>
    
    <span data-ttu-id="6809e-184">e.</span><span class="sxs-lookup"><span data-stu-id="6809e-184">e.</span></span> <span data-ttu-id="6809e-185">Kattintson a **hitelesítési beállításainak mentése**.</span><span class="sxs-lookup"><span data-stu-id="6809e-185">Click **Save authentication settings**.</span></span>

> [!TIP]
> <span data-ttu-id="6809e-186">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6809e-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6809e-187">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6809e-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6809e-188">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6809e-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6809e-189">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="6809e-189">Create an Azure AD test user</span></span>

<span data-ttu-id="6809e-190">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6809e-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="6809e-192">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6809e-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6809e-193">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="6809e-193">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6809e-195">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6809e-195">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6809e-197">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="6809e-197">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6809e-199">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6809e-199">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6809e-201">a.</span><span class="sxs-lookup"><span data-stu-id="6809e-201">a.</span></span> <span data-ttu-id="6809e-202">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6809e-202">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6809e-203">b.</span><span class="sxs-lookup"><span data-stu-id="6809e-203">b.</span></span> <span data-ttu-id="6809e-204">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6809e-204">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6809e-205">c.</span><span class="sxs-lookup"><span data-stu-id="6809e-205">c.</span></span> <span data-ttu-id="6809e-206">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6809e-206">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6809e-207">d.</span><span class="sxs-lookup"><span data-stu-id="6809e-207">d.</span></span> <span data-ttu-id="6809e-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6809e-208">Click **Create**.</span></span>
 
### <a name="create-a-uservoice-test-user"></a><span data-ttu-id="6809e-209">UserVoice tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6809e-209">Create a UserVoice test user</span></span>

<span data-ttu-id="6809e-210">Ahhoz, hogy az Azure AD-felhasználók UserVoice bejelentkezni, akkor ki kell építenie a UserVoice.</span><span class="sxs-lookup"><span data-stu-id="6809e-210">To enable Azure AD users to log in to UserVoice, they must be provisioned into UserVoice.</span></span> <span data-ttu-id="6809e-211">UserVoice, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6809e-211">In the case of UserVoice, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="6809e-212">Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6809e-212">To provision a user account, perform the following steps:</span></span>
1. <span data-ttu-id="6809e-213">Jelentkezzen be a **UserVoice** bérlő.</span><span class="sxs-lookup"><span data-stu-id="6809e-213">Log in to your **UserVoice** tenant.</span></span>

2. <span data-ttu-id="6809e-214">Ugrás a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6809e-214">Go to **Settings**.</span></span>
   
    <span data-ttu-id="6809e-215">![Beállítások](./media/active-directory-saas-uservoice-tutorial/ic777811.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="6809e-215">![Settings](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Settings")</span></span>

3. <span data-ttu-id="6809e-216">Kattintson a **általános**.</span><span class="sxs-lookup"><span data-stu-id="6809e-216">Click **General**.</span></span>

4. <span data-ttu-id="6809e-217">Kattintson a **az ügynökök és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="6809e-217">Click **Agents and permissions**.</span></span>
   
    <span data-ttu-id="6809e-218">![Az ügynökök és az engedélyek](./media/active-directory-saas-uservoice-tutorial/ic777812.png "az ügynökök és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="6809e-218">![Agents and permissions](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents and permissions")</span></span>

5. <span data-ttu-id="6809e-219">Kattintson a **rendszergazdák hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6809e-219">Click **Add admins**.</span></span>
   
    <span data-ttu-id="6809e-220">![Adja hozzá a rendszergazdák](./media/active-directory-saas-uservoice-tutorial/ic777813.png "rendszergazdák hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6809e-220">![Add admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Add admins")</span></span>

6. <span data-ttu-id="6809e-221">Az a **rendszergazdák meghívása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6809e-221">On the **Invite admins** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="6809e-222">![Rendszergazdák meghívása](./media/active-directory-saas-uservoice-tutorial/ic777814.png "rendszergazdák meghívása")</span><span class="sxs-lookup"><span data-stu-id="6809e-222">![Invite admins](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Invite admins")</span></span>
   
    <span data-ttu-id="6809e-223">a.</span><span class="sxs-lookup"><span data-stu-id="6809e-223">a.</span></span> <span data-ttu-id="6809e-224">Az e-mailek szövegmezőjének írja be a fiók kiépítése szeretne, és kattintson az e-mail cím **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6809e-224">In the Emails textbox, type the email address of the account you want to provision, and then click **Add**.</span></span>
   
    <span data-ttu-id="6809e-225">b.</span><span class="sxs-lookup"><span data-stu-id="6809e-225">b.</span></span> <span data-ttu-id="6809e-226">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="6809e-226">Click **Invite**.</span></span>

> [!NOTE]
> <span data-ttu-id="6809e-227">Bármely más UserVoice felhasználói fiók létrehozása eszközök, vagy API-k megadott uservoice rendelkezés AAD felhasználói fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="6809e-227">You can use any other UserVoice user account creation tools or APIs provided by UserVoice to provision AAD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6809e-228">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="6809e-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="6809e-229">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés UserVoice Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="6809e-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to UserVoice.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="6809e-231">**Britta Simon hozzárendelése UserVoice, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6809e-231">**To assign Britta Simon to UserVoice, perform the following steps:**</span></span>

1. <span data-ttu-id="6809e-232">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6809e-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6809e-234">Az alkalmazások listában válassza ki a **UserVoice**.</span><span class="sxs-lookup"><span data-stu-id="6809e-234">In the applications list, select **UserVoice**.</span></span>

    ![Az alkalmazások listáját a UserVoice-hivatkozás](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. <span data-ttu-id="6809e-236">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6809e-236">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="6809e-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6809e-238">Click **Add** button.</span></span> <span data-ttu-id="6809e-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6809e-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="6809e-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6809e-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6809e-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6809e-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6809e-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6809e-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6809e-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6809e-244">Test single sign-on</span></span>

<span data-ttu-id="6809e-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6809e-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6809e-246">A hozzáférési panelen a UserVoice csempére kattintva, meg kell beolvasása automatikusan bejelentkezett a UserVoice-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6809e-246">When you click the UserVoice tile in the Access Panel, you should get automatically signed-on to your UserVoice application.</span></span>
<span data-ttu-id="6809e-247">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6809e-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6809e-248">További források</span><span class="sxs-lookup"><span data-stu-id="6809e-248">Additional resources</span></span>

* [<span data-ttu-id="6809e-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6809e-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6809e-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6809e-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

