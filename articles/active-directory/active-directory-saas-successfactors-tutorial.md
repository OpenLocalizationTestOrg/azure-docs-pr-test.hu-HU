---
title: "Oktatóanyag: Azure Active Directoryval integrált SuccessFactors |} Microsoft Docs"
description: "Megtudhatja, hogyan SuccessFactors használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="be510-103">Oktatóanyag: Azure Active Directoryval integrált SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="be510-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="be510-104">Ez az oktatóanyag célja SuccessFactors integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="be510-104">The objective of this tutorial is to show you how to integrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="be510-105">SuccessFactors integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="be510-105">Integrating SuccessFactors with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="be510-106">Megadhatja a SuccessFactors hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="be510-106">You can control in Azure AD who has access to SuccessFactors</span></span>
* <span data-ttu-id="be510-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett SuccessFactors (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="be510-107">You can enable your users to automatically get signed-on to SuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="be510-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="be510-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="be510-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="be510-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be510-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="be510-110">Prerequisites</span></span>
<span data-ttu-id="be510-111">Konfigurálása az Azure AD-integrációs SuccessFactors, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="be510-111">To configure Azure AD integration with SuccessFactors, you need the following items:</span></span>

* <span data-ttu-id="be510-112">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="be510-112">A valid Azure subscription</span></span>
* <span data-ttu-id="be510-113">A bérlő által SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="be510-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="be510-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="be510-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="be510-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="be510-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="be510-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="be510-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="be510-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be510-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="be510-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="be510-118">Scenario description</span></span>
<span data-ttu-id="be510-119">Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="be510-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="be510-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="be510-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="be510-121">A gyűjteményből SuccessFactors hozzáadása</span><span class="sxs-lookup"><span data-stu-id="be510-121">Adding SuccessFactors from the gallery</span></span>
2. <span data-ttu-id="be510-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="be510-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-the-gallery"></a><span data-ttu-id="be510-123">A gyűjteményből SuccessFactors hozzáadása</span><span class="sxs-lookup"><span data-stu-id="be510-123">Adding SuccessFactors from the gallery</span></span>
<span data-ttu-id="be510-124">Az Azure AD integrálása a SuccessFactors konfigurálásához kell hozzáadnia SuccessFactors a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="be510-124">To configure the integration of SuccessFactors into Azure AD, you need to add SuccessFactors from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="be510-125">**A gyűjteményből SuccessFactors hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be510-125">**To add SuccessFactors from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="be510-126">A klasszikus Azure portálon, a bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="be510-126">In the Azure classic portal, on the left navigation panel, click **Active Directory**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][1]
2. <span data-ttu-id="be510-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="be510-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="be510-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="be510-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][2]
4. <span data-ttu-id="be510-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="be510-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="be510-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="be510-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][4]
6. <span data-ttu-id="be510-135">Az a **keresőmezőbe**, típus **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="be510-135">In the **search box**, type **SuccessFactors**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][5]
7. <span data-ttu-id="be510-137">Az eredmények panelen válassza ki a **SuccessFactors**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="be510-137">In the results panel, select **SuccessFactors**, and then click **Complete** to add the application.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="be510-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="be510-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="be510-140">Ez a szakasz célja bemutatják a konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="be510-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="be510-141">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó SuccessFactors egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="be510-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SuccessFactors to an user in Azure AD is.</span></span> <span data-ttu-id="be510-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SuccessFactors közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="be510-142">In other words, a link relationship between an Azure AD user and the related user in SuccessFactors needs to be established.</span></span>

<span data-ttu-id="be510-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** SuccessFactors a.</span><span class="sxs-lookup"><span data-stu-id="be510-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SuccessFactors.</span></span>

<span data-ttu-id="be510-144">Az Azure AD egyszeri bejelentkezést a SuccessFactors tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="be510-144">To configure and test Azure AD single sign-on with SuccessFactors, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="be510-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="be510-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="be510-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="be510-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="be510-147">**[SuccessFactors tesztfelhasználó létrehozása](#creating-a-successfactors-test-user)**  - való Britta Simon valami SuccessFactors, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="be510-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - to have a counterpart of Britta Simon in SuccessFactors that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="be510-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="be510-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="be510-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="be510-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="be510-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="be510-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="be510-151">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a klasszikus portálon, és konfigurálása egyszeri bejelentkezéshez az SuccessFactors alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="be510-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="be510-152">**Konfigurálása az Azure AD az egyszeri bejelentkezés SuccessFactors, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be510-152">**To configure Azure AD single sign-on with SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="be510-153">A klasszikus Azure portálon a a **SuccessFactors** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="be510-153">In the Azure classic portal, on the **SuccessFactors** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][7]
2. <span data-ttu-id="be510-155">Az a **hová bejelentkezni SuccessFactors felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="be510-155">On the **How would you like users to sign on to SuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]
3. <span data-ttu-id="be510-157">Az a **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="be510-157">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][9]
   
    <span data-ttu-id="be510-159">a.</span><span class="sxs-lookup"><span data-stu-id="be510-159">a.</span></span> <span data-ttu-id="be510-160">Az a **URL-cím bejelentkezési** szövegmezőhöz használatával egyike annak a következő URL-címet írja be:</span><span class="sxs-lookup"><span data-stu-id="be510-160">In the **Sign On URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="be510-161">b.</span><span class="sxs-lookup"><span data-stu-id="be510-161">b.</span></span> <span data-ttu-id="be510-162">Az a **válasz URL-CÍMEN** szövegmezőhöz használatával egyike annak a következő URL-címet írja be:</span><span class="sxs-lookup"><span data-stu-id="be510-162">In the **Reply URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="be510-163">c.</span><span class="sxs-lookup"><span data-stu-id="be510-163">c.</span></span> <span data-ttu-id="be510-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="be510-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="be510-165">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="be510-165">Please note that these are not the real values.</span></span> <span data-ttu-id="be510-166">Akkor frissítheti ezeket az értékeket a tényleges URL-cím bejelentkezési és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="be510-166">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="be510-167">Ahhoz, hogy ezeket az értékeket, lépjen kapcsolatba [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="be510-167">To get these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="be510-168">A a **konfigurálhatja az egyszeri bejelentkezés SuccessFactors** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen a tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="be510-168">On the **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save the certificate file locally on your computer.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][10]

2. <span data-ttu-id="be510-170">Egy másik webes böngészőablakban, jelentkezzen be a **SuccessFactors felügyeleti portál** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="be510-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="be510-171">Látogasson el **alkalmazásbiztonsági** és natív módon **egyszeri bejelentkezést a szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="be510-171">Visit **Application Security** and native to **Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="be510-172">Helyezze el az értéket a **Token alaphelyzetbe** kattintson **Token mentése** SAML SSO engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="be510-172">Place any value in the **Reset Token** and click **Save Token** to enable SAML SSO.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][11]

    > [!NOTE] 
    > <span data-ttu-id="be510-174">Ezt az értéket csak használja, mint a be-és kikapcsolása kapcsolót.</span><span class="sxs-lookup"><span data-stu-id="be510-174">This value is just used as the on/off switch.</span></span> <span data-ttu-id="be510-175">Ha bármely érték menti, a SAML SSO ON értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="be510-175">If any value is saved, the SAML SSO is ON.</span></span> <span data-ttu-id="be510-176">Ha egy üres érték. menti a SAML SSO OFF állásban.</span><span class="sxs-lookup"><span data-stu-id="be510-176">If a blank value is saved the SAML SSO is OFF.</span></span>

1. <span data-ttu-id="be510-177">Natív módon alábbi képernyőfelvétel és az alábbi műveletek végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="be510-177">Native to below screenshot and perform the following actions.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][12]
   
    <span data-ttu-id="be510-179">a.</span><span class="sxs-lookup"><span data-stu-id="be510-179">a.</span></span> <span data-ttu-id="be510-180">Válassza ki a **SAML v2 SSO** választógomb</span><span class="sxs-lookup"><span data-stu-id="be510-180">Select the **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="be510-181">b.</span><span class="sxs-lookup"><span data-stu-id="be510-181">b.</span></span> <span data-ttu-id="be510-182">Állítsa be a SAML biztos fél Name(e.g. SAml issuer + company name).</span><span class="sxs-lookup"><span data-stu-id="be510-182">Set the SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="be510-183">c.</span><span class="sxs-lookup"><span data-stu-id="be510-183">c.</span></span> <span data-ttu-id="be510-184">Az a **SAML kibocsátó** szövegmezőbe írja be az értéket a **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="be510-184">In the **SAML Issuer** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="be510-185">d.</span><span class="sxs-lookup"><span data-stu-id="be510-185">d.</span></span> <span data-ttu-id="be510-186">Válassza ki **választ (felhasználói generált/IdP/AP)** , **kötelező kötelező aláírás**.</span><span class="sxs-lookup"><span data-stu-id="be510-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="be510-187">e.</span><span class="sxs-lookup"><span data-stu-id="be510-187">e.</span></span> <span data-ttu-id="be510-188">Válassza ki **engedélyezett** , **SAML jelző engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="be510-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="be510-189">f.</span><span class="sxs-lookup"><span data-stu-id="be510-189">f.</span></span> <span data-ttu-id="be510-190">Válassza ki **nem** , **bejelentkezési kérelem-aláírás (KB generált/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="be510-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="be510-191">g.</span><span class="sxs-lookup"><span data-stu-id="be510-191">g.</span></span> <span data-ttu-id="be510-192">Válassza ki **böngésző/utólagos profil** , **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="be510-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="be510-193">h.</span><span class="sxs-lookup"><span data-stu-id="be510-193">h.</span></span> <span data-ttu-id="be510-194">Válassza ki **nem** , **kényszerítése a tanúsítvány érvényes időtartama**.</span><span class="sxs-lookup"><span data-stu-id="be510-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="be510-195">i.</span><span class="sxs-lookup"><span data-stu-id="be510-195">i.</span></span> <span data-ttu-id="be510-196">Másolja a letöltött tanúsítvány fájl tartalmát, és illessze be azt a **SAML ellenőrzése tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="be510-196">Copy the content of the downloaded certificate file, and then paste it into the **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="be510-197">A tanúsítványok tartalmát kell kezdődniük tanúsítványt és a záró tanúsítvány címkék.</span><span class="sxs-lookup"><span data-stu-id="be510-197">The certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="be510-198">Navigáljon a SAML V2, és végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="be510-198">Navigate to SAML V2, and then perform the following steps:</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][13]
   
    <span data-ttu-id="be510-200">a.</span><span class="sxs-lookup"><span data-stu-id="be510-200">a.</span></span> <span data-ttu-id="be510-201">Válassza ki **Igen** , **támogatja a Szolgáltató által kezdeményezett globális kijelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="be510-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="be510-202">b.</span><span class="sxs-lookup"><span data-stu-id="be510-202">b.</span></span> <span data-ttu-id="be510-203">Az a **globális kijelentkezési URL-címe (LogoutRequest cél)** szövegmezőbe írja be az értéket a **távoli kijelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="be510-203">In the **Global Logout Service URL (LogoutRequest destination)** textbox put the value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="be510-204">c.</span><span class="sxs-lookup"><span data-stu-id="be510-204">c.</span></span> <span data-ttu-id="be510-205">Válassza ki **nem** , **sp titkosítani kell az összes NameID elem szükséges**.</span><span class="sxs-lookup"><span data-stu-id="be510-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="be510-206">d.</span><span class="sxs-lookup"><span data-stu-id="be510-206">d.</span></span> <span data-ttu-id="be510-207">Válassza ki **meghatározatlan** , **NameID formátum**.</span><span class="sxs-lookup"><span data-stu-id="be510-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="be510-208">e.</span><span class="sxs-lookup"><span data-stu-id="be510-208">e.</span></span> <span data-ttu-id="be510-209">Válassza ki **Igen** , **engedélyezése sp kezdeményezett (AuthnRequest) bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="be510-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="be510-210">f.</span><span class="sxs-lookup"><span data-stu-id="be510-210">f.</span></span> <span data-ttu-id="be510-211">Az a **küldési kérelmek vállalati szintű kiállítóként** szövegmezőbe írja be az értéket a **távoli bejelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="be510-211">In the **Send request as Company-Wide issuer** textbox put the value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="be510-212">Hajtsa végre ezeket a lépéseket, ha engedélyezni szeretné a bejelentkezési felhasználónevek-és nagybetűket,.</span><span class="sxs-lookup"><span data-stu-id="be510-212">Perform these steps if you want to make the login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="be510-213">a.</span><span class="sxs-lookup"><span data-stu-id="be510-213">a.</span></span> <span data-ttu-id="be510-214">Látogasson el **vállalati beállítások**(közelében alsó).</span><span class="sxs-lookup"><span data-stu-id="be510-214">Visit **Company Settings**(near the bottom).</span></span>
   
    <span data-ttu-id="be510-215">b.</span><span class="sxs-lookup"><span data-stu-id="be510-215">b.</span></span> <span data-ttu-id="be510-216">Jelölje be a jelölőnégyzetet közelében **engedélyezése nem-nagybetűk felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="be510-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="be510-217">c.Click **mentése**.</span><span class="sxs-lookup"><span data-stu-id="be510-217">c.Click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][29]

    > [!NOTE] 
    > <span data-ttu-id="be510-219">Ezzel a kísérli meg, ha a rendszer ellenőrzi, ha az létrehoz egy duplikált SAML-bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="be510-219">If you try to enable this, the system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="be510-220">Például ha az ügyfél rendelkezik felhasználónevek felhasználó1, mind a felhasználó1.</span><span class="sxs-lookup"><span data-stu-id="be510-220">For example if the customer has usernames User1 and user1.</span></span> <span data-ttu-id="be510-221">Ezeket az ismétlődéseket véve számítógépnél nagybetűk teszi.</span><span class="sxs-lookup"><span data-stu-id="be510-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="be510-222">A rendszer erre azért van szükség egy hibaüzenet, és nem engedélyezi a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="be510-222">The system will give you an error message and will not enable the feature.</span></span> <span data-ttu-id="be510-223">Az ügyfél módosítania kell a felhasználónevek egyik, a ténylegesen elképzelhető különböző.</span><span class="sxs-lookup"><span data-stu-id="be510-223">The customer will need to change one of the usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="be510-224">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="be510-224">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    ![Alkalmazások][14]
2. <span data-ttu-id="be510-226">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="be510-226">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Alkalmazások][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="be510-228">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="be510-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="be510-229">Ez a szakasz célja a tesztfelhasználó létrehozása Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="be510-229">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][16]

<span data-ttu-id="be510-231">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be510-231">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="be510-232">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="be510-232">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][17]
2. <span data-ttu-id="be510-234">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="be510-234">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="be510-235">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="be510-235">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][18]
4. <span data-ttu-id="be510-237">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="be510-237">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][19]
5. <span data-ttu-id="be510-239">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="be510-239">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][20]
   
    <span data-ttu-id="be510-241">a.</span><span class="sxs-lookup"><span data-stu-id="be510-241">a.</span></span> <span data-ttu-id="be510-242">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="be510-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="be510-243">b.</span><span class="sxs-lookup"><span data-stu-id="be510-243">b.</span></span> <span data-ttu-id="be510-244">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="be510-244">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="be510-245">c.</span><span class="sxs-lookup"><span data-stu-id="be510-245">c.</span></span> <span data-ttu-id="be510-246">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="be510-246">Click **Next**.</span></span>
6. <span data-ttu-id="be510-247">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="be510-247">On the **User Profile** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][21]
   
    <span data-ttu-id="be510-249">a.</span><span class="sxs-lookup"><span data-stu-id="be510-249">a.</span></span> <span data-ttu-id="be510-250">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="be510-250">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="be510-251">b.</span><span class="sxs-lookup"><span data-stu-id="be510-251">b.</span></span> <span data-ttu-id="be510-252">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="be510-252">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="be510-253">c.</span><span class="sxs-lookup"><span data-stu-id="be510-253">c.</span></span> <span data-ttu-id="be510-254">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="be510-254">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="be510-255">d.</span><span class="sxs-lookup"><span data-stu-id="be510-255">d.</span></span> <span data-ttu-id="be510-256">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="be510-256">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="be510-257">e.</span><span class="sxs-lookup"><span data-stu-id="be510-257">e.</span></span> <span data-ttu-id="be510-258">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="be510-258">Click **Next**.</span></span>
7. <span data-ttu-id="be510-259">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="be510-259">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][22]
8. <span data-ttu-id="be510-261">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="be510-261">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][23]
   
    <span data-ttu-id="be510-263">a.</span><span class="sxs-lookup"><span data-stu-id="be510-263">a.</span></span> <span data-ttu-id="be510-264">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="be510-264">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="be510-265">b.</span><span class="sxs-lookup"><span data-stu-id="be510-265">b.</span></span> <span data-ttu-id="be510-266">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be510-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="be510-267">SuccessFactors tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="be510-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="be510-268">Ahhoz, hogy az Azure AD-felhasználók SuccessFactors bejelentkezni, akkor ki kell építenie SuccessFactors be.</span><span class="sxs-lookup"><span data-stu-id="be510-268">In order to enable Azure AD users to log into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="be510-269">SuccessFactors, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="be510-269">In the case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="be510-270">Ahhoz, hogy a felhasználó hozott létre az SuccessFactors, kapcsolatba kell lépnie a [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="be510-270">To get users created in SuccessFactors, you need to contact the [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="be510-271">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="be510-271">Assigning the Azure AD test user</span></span>
<span data-ttu-id="be510-272">Ez a szakasz célja Britta Simon által biztosított a hozzáférés SuccessFactors használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="be510-272">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SuccessFactors.</span></span>

![Felhasználó hozzárendelése][24]

<span data-ttu-id="be510-274">**Britta Simon hozzárendelése SuccessFactors, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="be510-274">**To assign Britta Simon to SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="be510-275">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="be510-275">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][25]
2. <span data-ttu-id="be510-277">Az alkalmazások listában válassza ki a **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="be510-277">In the applications list, select **SuccessFactors**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][26]
3. <span data-ttu-id="be510-279">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="be510-279">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][27]
4. <span data-ttu-id="be510-281">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="be510-281">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="be510-282">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="be510-282">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="be510-284">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="be510-284">Testing single sign-on</span></span>
<span data-ttu-id="be510-285">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="be510-285">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="be510-286">Ha a hozzáférési panelen SuccessFactors csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SuccessFactors alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="be510-286">When you click the SuccessFactors tile in the Access Panel, you should get automatically signed-on to your SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be510-287">További források</span><span class="sxs-lookup"><span data-stu-id="be510-287">Additional resources</span></span>
* [<span data-ttu-id="be510-288">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="be510-288">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="be510-289">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="be510-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
