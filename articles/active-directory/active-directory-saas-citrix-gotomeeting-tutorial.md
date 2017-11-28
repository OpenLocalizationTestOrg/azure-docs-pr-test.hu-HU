---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c1ac144c4fa43312ec26fce03cd0ee1bfcf73d4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a><span data-ttu-id="7c7eb-103">Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="7c7eb-103">Tutorial: Azure Active Directory integration with Citrix GoToMeeting</span></span>

<span data-ttu-id="7c7eb-104">Ebben az oktatóanyagban elsajátíthatja a Citrix GoToMeeting integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c7eb-104">In this tutorial, you learn how to integrate Citrix GoToMeeting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c7eb-105">Citrix GoToMeeting integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-105">Integrating Citrix GoToMeeting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7c7eb-106">Megadhatja a Citrix GoToMeeting hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7c7eb-106">You can control in Azure AD who has access to Citrix GoToMeeting</span></span>
- <span data-ttu-id="7c7eb-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Citrix GoToMeeting (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="7c7eb-107">You can enable your users to automatically get signed-on to Citrix GoToMeeting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c7eb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7c7eb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7c7eb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c7eb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c7eb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7c7eb-110">Prerequisites</span></span>

<span data-ttu-id="7c7eb-111">Az Azure AD-integráció konfigurálása a Citrix GoToMeeting, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-111">To configure Azure AD integration with Citrix GoToMeeting, you need the following items:</span></span>

- <span data-ttu-id="7c7eb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7c7eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c7eb-113">A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7c7eb-113">A Citrix GoToMeeting single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c7eb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c7eb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c7eb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c7eb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c7eb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c7eb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-118">Scenario description</span></span>
<span data-ttu-id="7c7eb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c7eb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c7eb-121">Citrix GoToMeeting hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7c7eb-121">Adding Citrix GoToMeeting from the gallery</span></span>
2. <span data-ttu-id="7c7eb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c7eb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-citrix-gotomeeting-from-the-gallery"></a><span data-ttu-id="7c7eb-123">Citrix GoToMeeting hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="7c7eb-123">Adding Citrix GoToMeeting from the gallery</span></span>
<span data-ttu-id="7c7eb-124">Az Azure AD integrálása a Citrix GoToMeeting konfigurálásához kell hozzáadnia a Citrix GoToMeeting a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-124">To configure the integration of Citrix GoToMeeting into Azure AD, you need to add Citrix GoToMeeting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7c7eb-125">**Adja hozzá a Citrix GoToMeeting a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-125">**To add Citrix GoToMeeting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7c7eb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c7eb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7c7eb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7c7eb-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7c7eb-133">Írja be a keresőmezőbe, **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-133">In the search box, type **Citrix GoToMeeting**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. <span data-ttu-id="7c7eb-135">Az eredmények panelen válassza ki a **Citrix GoToMeeting**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-135">In the results panel, select **Citrix GoToMeeting**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c7eb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c7eb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c7eb-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Citrix GoToMeeting "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="7c7eb-138">In this section, you configure and test Azure AD single sign-on with Citrix GoToMeeting based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7c7eb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Citrix GoToMeeting tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix GoToMeeting is to a user in Azure AD.</span></span> <span data-ttu-id="7c7eb-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Citrix GoToMeeting közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-140">In other words, a link relationship between an Azure AD user and the related user in Citrix GoToMeeting needs to be established.</span></span>

<span data-ttu-id="7c7eb-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Citrix GoToMeeting.</span></span>

<span data-ttu-id="7c7eb-142">Az Azure AD egyszeri bejelentkezést a Citrix GoToMeeting tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-142">To configure and test Azure AD single sign-on with Citrix GoToMeeting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7c7eb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7c7eb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c7eb-145">**[Citrix GoToMeeting tesztfelhasználó létrehozása](#creating-a-citrix-gotomeeting-test-user)**  - való Britta Simon egy megfelelője a Citrix GoToMeeting, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-145">**[Creating a Citrix GoToMeeting test user](#creating-a-citrix-gotomeeting-test-user)** - to have a counterpart of Britta Simon in Citrix GoToMeeting that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c7eb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c7eb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c7eb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c7eb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Citrix GoToMeeting alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix GoToMeeting application.</span></span>

<span data-ttu-id="7c7eb-150">**A Citrix GoToMeeting konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-150">**To configure Azure AD single sign-on with Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="7c7eb-151">Az Azure portálon a a **Citrix GoToMeeting** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-151">In the Azure portal, on the **Citrix GoToMeeting** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7c7eb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. <span data-ttu-id="7c7eb-155">Az a **Citrix GoToMeeting tartomány és az URL-címek** szakaszban, hajtsa végre a lépéseket nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-155">On the **Citrix GoToMeeting Domain and URLs** section, no need to perform any steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. <span data-ttu-id="7c7eb-157">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-157">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. <span data-ttu-id="7c7eb-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. <span data-ttu-id="7c7eb-161">A Citrix GoToMeeting SAML-alapú konfigurációs szakaszban kattintson a Citrix GoToMeeting SAML konfigurálása konfigurálása bejelentkezés ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-161">On the Citrix GoToMeeting SAML Configuration section, click Configure Citrix GoToMeeting SAML to open Configure sign-on window.</span></span> <span data-ttu-id="7c7eb-162">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-162">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

6. <span data-ttu-id="7c7eb-163">Egy másik böngészőablakban, jelentkezzen be a [Citrix szervezet Center](https://account.citrixonline.com/organization/administration/).</span><span class="sxs-lookup"><span data-stu-id="7c7eb-163">In a different browser window, log in to your [Citrix Organization Center](https://account.citrixonline.com/organization/administration/).</span></span>

7. <span data-ttu-id="7c7eb-164">Kattintson a **identitásszolgáltató** lapot, és végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-164">Click the **Identity Provider** tab, and then perform the following steps:</span></span>  
   
    <span data-ttu-id="7c7eb-165">![SAML-alapú telepítő](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-alapú telepítő")</span><span class="sxs-lookup"><span data-stu-id="7c7eb-165">![SAML setup](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML setup")</span></span>
   
    <span data-ttu-id="7c7eb-166">a.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-166">a.</span></span> <span data-ttu-id="7c7eb-167">Válassza ki **manuális**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-167">Select **Manual**</span></span>

    <span data-ttu-id="7c7eb-168">b.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-168">b.</span></span> <span data-ttu-id="7c7eb-169">Az Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be azt a **bejelentkezési lap URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-169">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Sign-in page URL** textbox.</span></span> 

    <span data-ttu-id="7c7eb-170">c.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-170">c.</span></span> <span data-ttu-id="7c7eb-171">Az Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás a **Sign-Out URL-cím** értékét, és illessze be azt a **kijelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-171">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **Sign-Out URL** value, and then paste it into the **Sign-out page URL** textbox.</span></span>

    <span data-ttu-id="7c7eb-172">d.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-172">d.</span></span> <span data-ttu-id="7c7eb-173">Az Azure portálon a a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás a **SAML Entitásazonosító** értékét, és illessze be azt a **Identity Provider Entitásazonosító**szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-173">In the Azure portal, on the **Configure single sign-on at Citrix GoToMeeting** dialog page, copy the **SAML Entity ID** value, and then paste it into the **Identity Provider Entity ID** textbox.</span></span>

    <span data-ttu-id="7c7eb-174">e.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-174">e.</span></span> <span data-ttu-id="7c7eb-175">A letöltött tanúsítvány feltöltése, kattintson a **tanúsítvány feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-175">To upload your downloaded certificate, click **Upload Certificate**.</span></span>

    <span data-ttu-id="7c7eb-176">f.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-176">f.</span></span> <span data-ttu-id="7c7eb-177">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7c7eb-178">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7c7eb-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7c7eb-179">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7c7eb-180">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c7eb-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c7eb-181">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-181">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c7eb-182">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7c7eb-184">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7c7eb-185">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-185">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c7eb-187">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-187">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c7eb-189">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-189">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c7eb-191">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7c7eb-191">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c7eb-193">a.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-193">a.</span></span> <span data-ttu-id="7c7eb-194">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c7eb-195">b.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-195">b.</span></span> <span data-ttu-id="7c7eb-196">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c7eb-197">c.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-197">c.</span></span> <span data-ttu-id="7c7eb-198">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7c7eb-199">d.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-199">d.</span></span> <span data-ttu-id="7c7eb-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-200">Click **Create**.</span></span>
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a><span data-ttu-id="7c7eb-201">Citrix GoToMeeting tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-201">Creating a Citrix GoToMeeting test user</span></span>

<span data-ttu-id="7c7eb-202">Ebben a szakaszban egy felhasználó Britta Simon nevű Citrix GoToMeeting jön létre.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-202">In this section, a user called Britta Simon is created in Citrix GoToMeeting.</span></span> <span data-ttu-id="7c7eb-203">Citrix GoToMeeting támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-203">Citrix GoToMeeting supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="7c7eb-204">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-204">There is no action item for you in this section.</span></span> <span data-ttu-id="7c7eb-205">Ha a felhasználó nem létezik a Citrix GoToMeeting, egy új jön létre, Citrix GoToMeeting elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-205">If a user doesn't already exist in Citrix GoToMeeting, a new one is created when you attempt to access Citrix GoToMeeting.</span></span>

>[!Note]
><span data-ttu-id="7c7eb-206">Ha a felhasználót manuálisan kell létrehozni, lépjen kapcsolatba kell [Citrix GoToMeeting támogatási csoport](https://care.citrixonline.com/gotomeeting)</span><span class="sxs-lookup"><span data-stu-id="7c7eb-206">If you need to create a user manually, Contact [Citrix GoToMeeting support team](https://care.citrixonline.com/gotomeeting)</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7c7eb-207">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7c7eb-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7c7eb-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Citrix GoToMeeting Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix GoToMeeting.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7c7eb-210">**Citrix GoToMeeting Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7c7eb-210">**To assign Britta Simon to Citrix GoToMeeting, perform the following steps:**</span></span>

1. <span data-ttu-id="7c7eb-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7c7eb-213">Az alkalmazások listában válassza ki a **Citrix GoToMeeting**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-213">In the applications list, select **Citrix GoToMeeting**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. <span data-ttu-id="7c7eb-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7c7eb-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-217">Click **Add** button.</span></span> <span data-ttu-id="7c7eb-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7c7eb-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7c7eb-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c7eb-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c7eb-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7c7eb-223">Testing single sign-on</span></span>

<span data-ttu-id="7c7eb-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7c7eb-225">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="7c7eb-225">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="7c7eb-226">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7c7eb-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c7eb-227">További források</span><span class="sxs-lookup"><span data-stu-id="7c7eb-227">Additional resources</span></span>

* [<span data-ttu-id="7c7eb-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c7eb-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7c7eb-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7c7eb-230">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c7eb-230">Configure User Provisioning</span></span>](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

