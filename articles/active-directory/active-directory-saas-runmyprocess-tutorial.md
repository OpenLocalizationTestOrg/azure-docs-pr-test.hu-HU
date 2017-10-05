---
title: "Oktatóanyag: Azure Active Directoryval integrált RunMyProcess |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és RunMyProcess között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f8a08ef4f90d5cb98e7648ae6001055a3f4696e8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a><span data-ttu-id="0f883-103">Oktatóanyag: Azure Active Directoryval integrált RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="0f883-103">Tutorial: Azure Active Directory integration with RunMyProcess</span></span>

<span data-ttu-id="0f883-104">Ebben az oktatóanyagban elsajátíthatja RunMyProcess integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0f883-104">In this tutorial, you learn how to integrate RunMyProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0f883-105">RunMyProcess integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0f883-105">Integrating RunMyProcess with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0f883-106">Megadhatja a RunMyProcess hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0f883-106">You can control in Azure AD who has access to RunMyProcess</span></span>
- <span data-ttu-id="0f883-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett RunMyProcess (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0f883-107">You can enable your users to automatically get signed-on to RunMyProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0f883-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0f883-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0f883-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0f883-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f883-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0f883-110">Prerequisites</span></span>

<span data-ttu-id="0f883-111">Konfigurálása az Azure AD-integrációs RunMyProcess, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0f883-111">To configure Azure AD integration with RunMyProcess, you need the following items:</span></span>

- <span data-ttu-id="0f883-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0f883-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0f883-113">Egy RunMyProcess egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0f883-113">A RunMyProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0f883-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0f883-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0f883-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0f883-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0f883-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0f883-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0f883-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat:[próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0f883-117">If you don't have an Azure AD trial environment, you can get a one-month trial here:[Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0f883-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0f883-118">Scenario description</span></span>
<span data-ttu-id="0f883-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0f883-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0f883-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0f883-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0f883-121">A gyűjteményből RunMyProcess hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0f883-121">Adding RunMyProcess from the gallery</span></span>
2. <span data-ttu-id="0f883-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0f883-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-runmyprocess-from-the-gallery"></a><span data-ttu-id="0f883-123">A gyűjteményből RunMyProcess hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0f883-123">Adding RunMyProcess from the gallery</span></span>
<span data-ttu-id="0f883-124">Az Azure AD integrálása a RunMyProcess konfigurálásához kell hozzáadnia RunMyProcess a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0f883-124">To configure the integration of RunMyProcess into Azure AD, you need to add RunMyProcess from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0f883-125">**A gyűjteményből RunMyProcess hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0f883-125">**To add RunMyProcess from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0f883-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0f883-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0f883-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0f883-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0f883-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0f883-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0f883-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0f883-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0f883-133">Írja be a keresőmezőbe, **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="0f883-133">In the search box, type **RunMyProcess**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. <span data-ttu-id="0f883-135">Az eredmények panelen válassza ki a **RunMyProcess**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0f883-135">In the results panel, select **RunMyProcess**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0f883-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0f883-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0f883-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="0f883-138">In this section, you configure and test Azure AD single sign-on with RunMyProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0f883-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó RunMyProcess a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0f883-139">For single sign-on to work, Azure AD needs to know what the counterpart user in RunMyProcess is to a user in Azure AD.</span></span> <span data-ttu-id="0f883-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a RunMyProcess közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0f883-140">In other words, a link relationship between an Azure AD user and the related user in RunMyProcess needs to be established.</span></span>

<span data-ttu-id="0f883-141">RunMyProcess, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0f883-141">In RunMyProcess, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0f883-142">Az Azure AD egyszeri bejelentkezést a RunMyProcess tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0f883-142">To configure and test Azure AD single sign-on with RunMyProcess, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0f883-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0f883-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0f883-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0f883-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0f883-145">**[RunMyProcess tesztfelhasználó létrehozása](#creating-a-runmyprocess-test-user)**  - való Britta Simon valami RunMyProcess, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0f883-145">**[Creating a RunMyProcess test user](#creating-a-runmyprocess-test-user)** - to have a counterpart of Britta Simon in RunMyProcess that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0f883-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0f883-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0f883-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0f883-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0f883-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0f883-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0f883-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az RunMyProcess alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0f883-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RunMyProcess application.</span></span>

<span data-ttu-id="0f883-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés RunMyProcess, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0f883-150">**To configure Azure AD single sign-on with RunMyProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="0f883-151">Az Azure portálon a a **RunMyProcess** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0f883-151">In the Azure portal, on the **RunMyProcess** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0f883-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0f883-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. <span data-ttu-id="0f883-155">Az a **RunMyProcess tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0f883-155">On the **RunMyProcess Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    <span data-ttu-id="0f883-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://live.runmyprocess.com/live/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="0f883-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.runmyprocess.com/live/<tenant id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0f883-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="0f883-158">The value is not real.</span></span> <span data-ttu-id="0f883-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="0f883-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="0f883-160">Ügyfél [RunMyProcess ügyfél-támogatási csoport](mailto:support@runmyprocess.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="0f883-160">Contact [RunMyProcess Client support team](mailto:support@runmyprocess.com) to get the value.</span></span> 

4. <span data-ttu-id="0f883-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0f883-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. <span data-ttu-id="0f883-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f883-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0f883-165">A a **RunMyProcess konfigurációs** kattintson **konfigurálása RunMyProcess** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0f883-165">On the **RunMyProcess Configuration** section, click **Configure RunMyProcess** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0f883-166">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0f883-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. <span data-ttu-id="0f883-168">Egy másik webes böngészőablakban bejelentkezés a RunMyProcess bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0f883-168">In a different web browser window, sign-on to your RunMyProcess tenant as an administrator.</span></span>

8. <span data-ttu-id="0f883-169">A bal oldali navigációs panelen kattintson a **fiók** válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="0f883-169">In left navigation panel, click **Account** and select **Configuration**.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. <span data-ttu-id="0f883-171">Ugrás a **hitelesítési módszer** szakaszt, és hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="0f883-171">Go to **Authentication method** section and perform below steps:</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    <span data-ttu-id="0f883-173">a.</span><span class="sxs-lookup"><span data-stu-id="0f883-173">a.</span></span> <span data-ttu-id="0f883-174">Mint **metódus**, jelölje be **SSO Samlv2**.</span><span class="sxs-lookup"><span data-stu-id="0f883-174">As **Method**, select **SSO with Samlv2**.</span></span> 

    <span data-ttu-id="0f883-175">b.</span><span class="sxs-lookup"><span data-stu-id="0f883-175">b.</span></span> <span data-ttu-id="0f883-176">Az a **SSO átirányítási** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="0f883-176">In the **SSO redirect** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="0f883-177">c.</span><span class="sxs-lookup"><span data-stu-id="0f883-177">c.</span></span> <span data-ttu-id="0f883-178">Az a **kijelentkezési átirányítási** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="0f883-178">In the **Logout redirect** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="0f883-179">d.</span><span class="sxs-lookup"><span data-stu-id="0f883-179">d.</span></span> <span data-ttu-id="0f883-180">Az a **név Azonosítóformátumának** szövegmező, írja be az értéket a **azonosító formátuma** , **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="0f883-180">In the **Name Id Format** textbox, type the value of **Name Identifier Format** as **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="0f883-181">e.</span><span class="sxs-lookup"><span data-stu-id="0f883-181">e.</span></span> <span data-ttu-id="0f883-182">A letöltött fájlt másolja és illessze be azt a **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0f883-182">Copy the content of the downloaded certificate file and then paste it into the **Certificate** textbox.</span></span> 
 
    <span data-ttu-id="0f883-183">f.</span><span class="sxs-lookup"><span data-stu-id="0f883-183">f.</span></span> <span data-ttu-id="0f883-184">Kattintson a **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0f883-184">Click **Save** icon.</span></span>

> [!TIP]
> <span data-ttu-id="0f883-185">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0f883-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0f883-186">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0f883-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0f883-187">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0f883-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0f883-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f883-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="0f883-189">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0f883-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0f883-191">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0f883-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0f883-192">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0f883-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0f883-194">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0f883-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0f883-196">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0f883-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0f883-198">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0f883-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0f883-200">a.</span><span class="sxs-lookup"><span data-stu-id="0f883-200">a.</span></span> <span data-ttu-id="0f883-201">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0f883-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0f883-202">b.</span><span class="sxs-lookup"><span data-stu-id="0f883-202">b.</span></span> <span data-ttu-id="0f883-203">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0f883-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0f883-204">c.</span><span class="sxs-lookup"><span data-stu-id="0f883-204">c.</span></span> <span data-ttu-id="0f883-205">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0f883-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0f883-206">d.</span><span class="sxs-lookup"><span data-stu-id="0f883-206">d.</span></span> <span data-ttu-id="0f883-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0f883-207">Click **Create**.</span></span>
 
### <a name="creating-a-runmyprocess-test-user"></a><span data-ttu-id="0f883-208">RunMyProcess tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f883-208">Creating a RunMyProcess test user</span></span>

<span data-ttu-id="0f883-209">Ahhoz, hogy az Azure AD-felhasználók RunMyProcess bejelentkezni, akkor ki kell építenie a RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="0f883-209">In order to enable Azure AD users to log in to RunMyProcess, they must be provisioned into RunMyProcess.</span></span> <span data-ttu-id="0f883-210">RunMyProcess, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="0f883-210">In the case of RunMyProcess, provisioning is a manual task.</span></span>

<span data-ttu-id="0f883-211">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0f883-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="0f883-212">Jelentkezzen be rendszergazdaként a RunMyProcess vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="0f883-212">Log in to your RunMyProcess company site as an administrator.</span></span>

2. <span data-ttu-id="0f883-213">Kattintson a **fiók** válassza **felhasználók** a bal oldali navigációs panelen, majd kattintson a **új felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="0f883-213">Click **Account** and select **Users** in left navigation panel, then click **New User**.</span></span>
   
    <span data-ttu-id="0f883-214">![Új felhasználó](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="0f883-214">![New User](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")</span></span>

3. <span data-ttu-id="0f883-215">Az a **felhasználói beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0f883-215">In the **User Settings** section, perform the following steps:</span></span>
   
    <span data-ttu-id="0f883-216">![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profil")</span><span class="sxs-lookup"><span data-stu-id="0f883-216">![Profile](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")</span></span> 
  
    <span data-ttu-id="0f883-217">a.</span><span class="sxs-lookup"><span data-stu-id="0f883-217">a.</span></span> <span data-ttu-id="0f883-218">Típus a **neve** és **E-mail** egy érvényes Azure AD-fiókot szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0f883-218">Type the **Name** and **E-mail** of a valid Azure AD account you want to provision into the related textboxes.</span></span> 

    <span data-ttu-id="0f883-219">b.</span><span class="sxs-lookup"><span data-stu-id="0f883-219">b.</span></span> <span data-ttu-id="0f883-220">Válasszon egy **IDE nyelvi**, **nyelvi**, és **profil**.</span><span class="sxs-lookup"><span data-stu-id="0f883-220">Select an **IDE language**, **Language**, and **Profile**.</span></span> 

    <span data-ttu-id="0f883-221">c.</span><span class="sxs-lookup"><span data-stu-id="0f883-221">c.</span></span> <span data-ttu-id="0f883-222">Válassza ki **fiók létrehozása e-mail küldése me**.</span><span class="sxs-lookup"><span data-stu-id="0f883-222">Select **Send account creation e-mail to me**.</span></span> 

    <span data-ttu-id="0f883-223">d.</span><span class="sxs-lookup"><span data-stu-id="0f883-223">d.</span></span> <span data-ttu-id="0f883-224">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0f883-224">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="0f883-225">Bármely más RunMyProcess felhasználói fiók létrehozása eszközök vagy API-k által biztosított RunMyProcess kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="0f883-225">You can use any other RunMyProcess user account creation tools or APIs provided by RunMyProcess to provision Azure Active Directory user accounts.</span></span> 
    > 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0f883-226">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0f883-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0f883-227">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés RunMyProcess Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0f883-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RunMyProcess.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0f883-229">**Britta Simon hozzárendelése RunMyProcess, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0f883-229">**To assign Britta Simon to RunMyProcess, perform the following steps:**</span></span>

1. <span data-ttu-id="0f883-230">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0f883-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0f883-232">Az alkalmazások listában válassza ki a **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="0f883-232">In the applications list, select **RunMyProcess**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. <span data-ttu-id="0f883-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0f883-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0f883-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f883-236">Click **Add** button.</span></span> <span data-ttu-id="0f883-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0f883-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0f883-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0f883-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0f883-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0f883-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0f883-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0f883-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0f883-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0f883-242">Testing single sign-on</span></span>

<span data-ttu-id="0f883-243">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="0f883-243">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="0f883-244">Ha a hozzáférési panelen RunMyProcess csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az RunMyProcess alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="0f883-244">When you click the RunMyProcess tile in the Access Panel, you should get automatically signed-on to your RunMyProcess application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0f883-245">További források</span><span class="sxs-lookup"><span data-stu-id="0f883-245">Additional resources</span></span>

* [<span data-ttu-id="0f883-246">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0f883-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0f883-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0f883-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

