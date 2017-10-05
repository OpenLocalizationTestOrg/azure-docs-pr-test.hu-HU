---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: a56a5af171eaca259db29f25fee4331a77313420
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a><span data-ttu-id="b4869-103">Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox</span><span class="sxs-lookup"><span data-stu-id="b4869-103">Tutorial: Azure Active Directory integration with Dropbox for Business</span></span>

<span data-ttu-id="b4869-104">Ebben az oktatóanyagban elsajátíthatja a vállalati Dropbox integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b4869-104">In this tutorial, you learn how to integrate Dropbox for Business with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b4869-105">A vállalati Dropbox integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b4869-105">Integrating Dropbox for Business with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b4869-106">Megadhatja a Dropbox vállalati hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b4869-106">You can control in Azure AD who has access to Dropbox for Business</span></span>
- <span data-ttu-id="b4869-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a dropbox alkalmazásba vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b4869-107">You can enable your users to automatically get signed-on to Dropbox for Business (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b4869-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b4869-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b4869-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b4869-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4869-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4869-110">Prerequisites</span></span>

<span data-ttu-id="b4869-111">Az Azure AD-integráció konfigurálása a Dropbox vállalati, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b4869-111">To configure Azure AD integration with Dropbox for Business, you need the following items:</span></span>

- <span data-ttu-id="b4869-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b4869-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b4869-113">A Dropbox üzleti egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="b4869-113">A Dropbox for Business single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b4869-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b4869-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b4869-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b4869-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b4869-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4869-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b4869-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4869-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b4869-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b4869-118">Scenario description</span></span>
<span data-ttu-id="b4869-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b4869-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b4869-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b4869-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b4869-121">A vállalati Dropbox hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b4869-121">Adding Dropbox for Business from the gallery</span></span>
2. <span data-ttu-id="b4869-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b4869-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dropbox-for-business-from-the-gallery"></a><span data-ttu-id="b4869-123">A vállalati Dropbox hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b4869-123">Adding Dropbox for Business from the gallery</span></span>
<span data-ttu-id="b4869-124">A dropbox-bA az Azure AD-be a vállalati integráció konfigurálásához kell hozzáadnia a vállalati Dropbox a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b4869-124">To configure the integration of Dropbox for Business into Azure AD, you need to add Dropbox for Business from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b4869-125">**Adja hozzá a vállalati Dropbox a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b4869-125">**To add Dropbox for Business from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b4869-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b4869-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b4869-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b4869-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b4869-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b4869-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b4869-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b4869-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b4869-133">Írja be a keresőmezőbe, **Dropbox vállalati**.</span><span class="sxs-lookup"><span data-stu-id="b4869-133">In the search box, type **Dropbox for Business**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. <span data-ttu-id="b4869-135">Az eredmények panelen válassza ki a **vállalati Dropbox**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b4869-135">In the results panel, select **Dropbox for Business**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b4869-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b4869-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b4869-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Dropbox vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="b4869-138">In this section, you configure and test Azure AD single sign-on with Dropbox for Business based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b4869-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Dropbox vállalati megfelelőjére felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b4869-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Dropbox for Business is to a user in Azure AD.</span></span> <span data-ttu-id="b4869-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a vállalati Dropbox közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b4869-140">In other words, a link relationship between an Azure AD user and the related user in Dropbox for Business needs to be established.</span></span>

<span data-ttu-id="b4869-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Dropbox vállalati.</span><span class="sxs-lookup"><span data-stu-id="b4869-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Dropbox for Business.</span></span>

<span data-ttu-id="b4869-142">Az Azure AD egyszeri bejelentkezést a vállalati Dropbox tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b4869-142">To configure and test Azure AD single sign-on with Dropbox for Business, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b4869-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b4869-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b4869-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b4869-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b4869-145">**[A Dropbox az üzleti tesztfelhasználó létrehozása](#creating-a-dropbox-for-business-test-user)**  - való Britta Simon egy megfelelője a Dropbox vállalati, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b4869-145">**[Creating a Dropbox for Business test user](#creating-a-dropbox-for-business-test-user)** - to have a counterpart of Britta Simon in Dropbox for Business that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b4869-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b4869-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b4869-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b4869-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b4869-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4869-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b4869-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a dropboxban üzleti alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b4869-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dropbox for Business application.</span></span>

<span data-ttu-id="b4869-150">**Konfigurálása az Azure AD egyszeri bejelentkezést a vállalati Dropbox, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b4869-150">**To configure Azure AD single sign-on with Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="b4869-151">Az Azure portálon a a **vállalati Dropbox** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b4869-151">In the Azure portal, on the **Dropbox for Business** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b4869-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b4869-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. <span data-ttu-id="b4869-155">Az a **Dropbox üzleti tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b4869-155">On the **Dropbox for Business Domain and URLs** section, perform the following steps:</span></span>

    <span data-ttu-id="b4869-156">a.</span><span class="sxs-lookup"><span data-stu-id="b4869-156">a.</span></span> <span data-ttu-id="b4869-157">Jelentkezzen be a Dropbox üzleti bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="b4869-157">Sign on to your Dropbox for business tenant.</span></span> 
   
    <span data-ttu-id="b4869-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b4869-158">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="b4869-159">b.</span><span class="sxs-lookup"><span data-stu-id="b4869-159">b.</span></span> <span data-ttu-id="b4869-160">A bal oldali navigációs ablaktábláján kattintson **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="b4869-160">In the navigation pane on the left side, click **Admin Console**.</span></span> 
   
    <span data-ttu-id="b4869-161">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b4869-161">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="b4869-162">c.</span><span class="sxs-lookup"><span data-stu-id="b4869-162">c.</span></span> <span data-ttu-id="b4869-163">Az a **felügyeleti konzol**, kattintson a **hitelesítési** a bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="b4869-163">On the **Admin Console**, click **Authentication** in the left navigation pane.</span></span> 
   
    <span data-ttu-id="b4869-164">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b4869-164">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="b4869-165">d.</span><span class="sxs-lookup"><span data-stu-id="b4869-165">d.</span></span> <span data-ttu-id="b4869-166">Az a **egyszeri bejelentkezés** szakaszban jelölje be **egyszeri bejelentkezés engedélyezése**, és kattintson a **további** bontsa ki az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b4869-166">In the **Single sign-on** section, select **Enable single sign-on**, and then click **More** to expand this section.</span></span>  
   
    <span data-ttu-id="b4869-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b4869-167">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="b4869-168">e.</span><span class="sxs-lookup"><span data-stu-id="b4869-168">e.</span></span> <span data-ttu-id="b4869-169">Az URL-Címének másolása mellett **is jelentkeznek be az e-mail cím beírásával vagy azok lépjen közvetlenül**.</span><span class="sxs-lookup"><span data-stu-id="b4869-169">Copy the URL next to **Users can sign in by entering their email address or they can go directly to**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    <span data-ttu-id="b4869-171">f.</span><span class="sxs-lookup"><span data-stu-id="b4869-171">f.</span></span> <span data-ttu-id="b4869-172">Az Azure portálon a a **bejelentkezési URL-cím** szövegmezőhöz beilleszteni az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="b4869-172">On the Azure portal, in the **Sign-on URL** textbox, paste the URL.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     <span data-ttu-id="b4869-174">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.dropbox.com/sso/<id>`</span><span class="sxs-lookup"><span data-stu-id="b4869-174">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.dropbox.com/sso/<id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b4869-175">Ez az érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="b4869-175">This value is not real value.</span></span> <span data-ttu-id="b4869-176">Frissítse az értéket a tényleges bejelentkezési URL-címet kap az egyszeri bejelentkezés szakasz.</span><span class="sxs-lookup"><span data-stu-id="b4869-176">Update the value with the actual Sign-on URL you get from their Single sign-on section.</span></span> <span data-ttu-id="b4869-177">Ügyfél [Dropbox üzleti ügyfél-támogatási csoport](https://www.dropbox.com/business/contact) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b4869-177">Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact) to get this value.</span></span> 
 
4. <span data-ttu-id="b4869-178">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b4869-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. <span data-ttu-id="b4869-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b4869-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b4869-182">A a **üzleti konfiguráció Dropbox** kattintson **Dropbox konfigurálása a vállalati** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b4869-182">On the **Dropbox for Business Configuration** section, click **Configure Dropbox for Business** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b4869-183">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b4869-183">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. <span data-ttu-id="b4869-185">Egyszeri bejelentkezés konfigurálása **vállalati Dropbox** oldalán, nyissa meg a Dropbox üzleti bérlő, a a **egyszeri bejelentkezés** szakasza a **hitelesítési** lapon, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b4869-185">To configure single sign-on on **Dropbox for Business** side, Go on your Dropbox for Business tenant, in the **Single sign-on** section of the **Authentication** page, perform the following steps:</span></span> 
   
    <span data-ttu-id="b4869-186">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b4869-186">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="b4869-187">a.</span><span class="sxs-lookup"><span data-stu-id="b4869-187">a.</span></span> <span data-ttu-id="b4869-188">Kattintson a **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="b4869-188">Click **Required**.</span></span>
   
    <span data-ttu-id="b4869-189">b.</span><span class="sxs-lookup"><span data-stu-id="b4869-189">b.</span></span> <span data-ttu-id="b4869-190">Az Azure portálon a a **bejelentkezés konfigurálása** ablakban, a Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be azt a **bejelentkezési URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b4869-190">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in URL** textbox.</span></span>

    <span data-ttu-id="b4869-191">c.</span><span class="sxs-lookup"><span data-stu-id="b4869-191">c.</span></span> <span data-ttu-id="b4869-192">Kattintson a **tanúsítvány kiválasztása**, majd keresse meg a **Base64 kódolású tanúsítvány fájl**.</span><span class="sxs-lookup"><span data-stu-id="b4869-192">Click **Choose certificate**, and then browse to your **Base64 encoded certificate file**.</span></span>

    <span data-ttu-id="b4869-193">d.</span><span class="sxs-lookup"><span data-stu-id="b4869-193">d.</span></span> <span data-ttu-id="b4869-194">Kattintson a **módosítások mentése** a DropBox üzleti bérlő konfigurálásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4869-194">Click **Save changes** to complete the configuration on your DropBox for Business tenant.</span></span>

> [!TIP]
> <span data-ttu-id="b4869-195">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="b4869-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b4869-196">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b4869-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b4869-197">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b4869-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b4869-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4869-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="b4869-199">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b4869-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b4869-201">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b4869-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b4869-202">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b4869-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="b4869-204">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b4869-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b4869-206">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b4869-206">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b4869-208">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b4869-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b4869-210">a.</span><span class="sxs-lookup"><span data-stu-id="b4869-210">a.</span></span> <span data-ttu-id="b4869-211">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b4869-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b4869-212">b.</span><span class="sxs-lookup"><span data-stu-id="b4869-212">b.</span></span> <span data-ttu-id="b4869-213">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b4869-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b4869-214">c.</span><span class="sxs-lookup"><span data-stu-id="b4869-214">c.</span></span> <span data-ttu-id="b4869-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b4869-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b4869-216">d.</span><span class="sxs-lookup"><span data-stu-id="b4869-216">d.</span></span> <span data-ttu-id="b4869-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4869-217">Click **Create**.</span></span>
 
### <a name="creating-a-dropbox-for-business-test-user"></a><span data-ttu-id="b4869-218">A Dropbox az üzleti tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4869-218">Creating a Dropbox for Business test user</span></span>

<span data-ttu-id="b4869-219">Ebben a szakaszban egy Britta Simon nevű felhasználó a vállalati Dropbox jön létre.</span><span class="sxs-lookup"><span data-stu-id="b4869-219">In this section, a user called Britta Simon is created in Dropbox for Business.</span></span> <span data-ttu-id="b4869-220">Vállalati Dropbox támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b4869-220">Dropbox for Business supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="b4869-221">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="b4869-221">There is no action item for you in this section.</span></span> <span data-ttu-id="b4869-222">Ha a felhasználó nem létezik a Dropbox vállalati, egy új jön létre, a vállalati Dropbox elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="b4869-222">If a user doesn't already exist in Dropbox for Business, a new one is created when you attempt to access Dropbox for Business.</span></span>

>[!Note]
><span data-ttu-id="b4869-223">Ha a felhasználót manuálisan kell létrehozni, lépjen kapcsolatba kell [Dropbox üzleti ügyfél-támogatási csoport](https://www.dropbox.com/business/contact)</span><span class="sxs-lookup"><span data-stu-id="b4869-223">If you need to create a user manually, Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact)</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b4869-224">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b4869-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b4869-225">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés a vállalati Dropbox Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b4869-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dropbox for Business.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b4869-227">**A dropbox alkalmazásba vállalati Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="b4869-227">**To assign Britta Simon to Dropbox for Business, perform the following steps:**</span></span>

1. <span data-ttu-id="b4869-228">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b4869-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b4869-230">Az alkalmazások listában válassza ki a **Dropbox vállalati**.</span><span class="sxs-lookup"><span data-stu-id="b4869-230">In the applications list, select **Dropbox for Business**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. <span data-ttu-id="b4869-232">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b4869-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b4869-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b4869-234">Click **Add** button.</span></span> <span data-ttu-id="b4869-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b4869-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b4869-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b4869-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b4869-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b4869-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b4869-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b4869-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b4869-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b4869-240">Testing single sign-on</span></span>

<span data-ttu-id="b4869-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b4869-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b4869-242">Amikor az üzleti csempe a hozzáférési panelen a Dropbox kattint, üzleti alkalmazás kapja meg a Dropbox bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="b4869-242">When you click the Dropbox for Business tile in the Access Panel, you should get login page of your Dropbox for Business application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4869-243">További források</span><span class="sxs-lookup"><span data-stu-id="b4869-243">Additional resources</span></span>

* [<span data-ttu-id="b4869-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b4869-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b4869-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b4869-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b4869-246">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4869-246">Configure User Provisioning</span></span>](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

