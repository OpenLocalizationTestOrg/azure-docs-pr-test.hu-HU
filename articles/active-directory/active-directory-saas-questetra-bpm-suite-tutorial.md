---
title: "Oktatóanyag: Azure Active Directory-integráció Questetra BPM Suite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Questetra BPM Suite között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 7ae75446c9d19ce15a82caa9604658a528ab9941
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a><span data-ttu-id="ccf98-103">Oktatóanyag: Azure Active Directory-integráció Questetra BPM csomaggal</span><span class="sxs-lookup"><span data-stu-id="ccf98-103">Tutorial: Azure Active Directory integration with Questetra BPM Suite</span></span>
<span data-ttu-id="ccf98-104">Ez az oktatóanyag célja Questetra BPM Suite integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="ccf98-104">The objective of this tutorial is to show you how to integrate Questetra BPM Suite with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="ccf98-105">Questetra BPM Suite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ccf98-105">Integrating Questetra BPM Suite with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="ccf98-106">Megadhatja a Questetra BPM Suite hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ccf98-106">You can control in Azure AD who has access to Questetra BPM Suite</span></span> 
* <span data-ttu-id="ccf98-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Questetra BPM Suite (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="ccf98-107">You can enable your users to automatically get signed-on to Questetra BPM Suite (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="ccf98-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="ccf98-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="ccf98-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ccf98-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccf98-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ccf98-110">Prerequisites</span></span>
<span data-ttu-id="ccf98-111">Az Azure AD-integráció konfigurálása Questetra BPM csomaggal, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ccf98-111">To configure Azure AD integration with Questetra BPM Suite, you need the following items:</span></span>

* <span data-ttu-id="ccf98-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ccf98-112">An Azure AD subscription</span></span>
* <span data-ttu-id="ccf98-113">Egy [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ccf98-113">An [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ccf98-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ccf98-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="ccf98-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ccf98-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="ccf98-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ccf98-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="ccf98-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ccf98-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="ccf98-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ccf98-118">Scenario Description</span></span>
<span data-ttu-id="ccf98-119">Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ccf98-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="ccf98-120">A forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ccf98-120">The scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="ccf98-121">A gyűjteményből Questetra BPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ccf98-121">Adding Questetra BPM Suite from the gallery</span></span> 
2. <span data-ttu-id="ccf98-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ccf98-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-questetra-bpm-suite-from-the-gallery"></a><span data-ttu-id="ccf98-123">A gyűjteményből Questetra BPM csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ccf98-123">Adding Questetra BPM Suite from the gallery</span></span>
<span data-ttu-id="ccf98-124">Az Azure AD integrálása a Questetra BPM Suite konfigurálásához kell hozzáadnia Questetra BPM Suite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ccf98-124">To configure the integration of Questetra BPM Suite into Azure AD, you need to add Questetra BPM Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ccf98-125">**A gyűjteményből Questetra BPM Suite hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ccf98-125">**To add Questetra BPM Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ccf98-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="ccf98-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="ccf98-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="ccf98-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="ccf98-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="ccf98-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="ccf98-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="ccf98-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="ccf98-135">Írja be a keresőmezőbe, **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-135">In the search box, type **Questetra BPM Suite**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="ccf98-137">Az eredmények ablaktáblájában válassza **Questetra BPM Suite**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ccf98-137">In the results pane, select **Questetra BPM Suite**, and then click **Complete** to add the application.</span></span>

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ccf98-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ccf98-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ccf98-139">Ez a szakasz célja bemutatják a konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Questetra BPM Suite "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ccf98-139">The objective of this section is to show you how to configure and test Azure AD single sign-on with Questetra BPM Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ccf98-140">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Questetra BPM csomagban található egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="ccf98-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Questetra BPM Suite to an user in Azure AD is.</span></span> <span data-ttu-id="ccf98-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Questetra BPM csomagban található hivatkozás kapcsolatának kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ccf98-141">In other words, a link relationship between an Azure AD user and the related user in Questetra BPM Suite needs to be established.</span></span>  
<span data-ttu-id="ccf98-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Questetra BPM csomagban található.</span><span class="sxs-lookup"><span data-stu-id="ccf98-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Questetra BPM Suite.</span></span>

<span data-ttu-id="ccf98-143">Az Azure AD az egyszeri bejelentkezés Questetra BPM Suite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ccf98-143">To configure and test Azure AD single sign-on with Questetra BPM Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ccf98-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ccf98-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ccf98-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ccf98-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ccf98-146">**[Questetra BPM Suite tesztfelhasználó létrehozása](#creating-a-questetra-bpm-suite-test-user)**  - való egy megfelelője a Britta Simon Questetra BPM csomag, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ccf98-146">**[Creating a Questetra BPM Suite test user](#creating-a-questetra-bpm-suite-test-user)** - to have a counterpart of Britta Simon in Questetra BPM Suite that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ccf98-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ccf98-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ccf98-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ccf98-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ccf98-149">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ccf98-149">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="ccf98-150">Ez a szakasz célja az Azure AD az egyszeri bejelentkezés a klasszikus Azure portálon engedélyezése és konfigurálása egyszeri bejelentkezéshez az Questetra BPM Suite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ccf98-150">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your Questetra BPM Suite application.</span></span>

<span data-ttu-id="ccf98-151">**Konfigurálja az Azure AD az egyszeri bejelentkezés Questetra BPM csomaggal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ccf98-151">**To configure Azure AD single sign-on with Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="ccf98-152">A klasszikus Azure portálon a a **Questetra BPM Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ccf98-152">In the Azure classic portal, on the **Questetra BPM Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. <span data-ttu-id="ccf98-154">Az a **hová bejelentkezni Questetra BPM Suite felhasználók** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-154">On the **How would you like users to sign on to Questetra BPM Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. <span data-ttu-id="ccf98-156">Egy másik webes böngészőablakban, jelentkezzen be a **Questetra BPM Suite** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ccf98-156">In a different web browser window, log into your **Questetra BPM Suite** company site as an administrator.</span></span>

4. <span data-ttu-id="ccf98-157">Kattintson a felső menüben **rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-157">In the menu on the top, click **System Settings**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

5. <span data-ttu-id="ccf98-159">Lehetőségre a **SingleSignOnSAML** kattintson **egyszeri Bejelentkezést (SAML)**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-159">To open the **SingleSignOnSAML** page, click **SSO (SAML)**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

6. <span data-ttu-id="ccf98-161">A klasszikus Azure portálon a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ccf98-161">In the Azure classic portal, on the **Configure App Settings** dialog page, perform the following steps:</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][13]
   
    <span data-ttu-id="ccf98-163">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-163">a.</span></span> <span data-ttu-id="ccf98-164">Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **ACS URL-cím**, és illessze be azt a **URL-cím bejelentkezési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-164">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **ACS URL**, and then paste it into the **Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="ccf98-165">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-165">b.</span></span> <span data-ttu-id="ccf98-166">Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **Entitásazonosító**, és illessze be azt a **kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-166">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **Entity ID**, and then paste it into the **Issuer URL** textbox.</span></span>
   
    <span data-ttu-id="ccf98-167">c.</span><span class="sxs-lookup"><span data-stu-id="ccf98-167">c.</span></span> <span data-ttu-id="ccf98-168">Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **ACS URL-cím**, és illessze be azt a **válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-168">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **ACS URL**, and then paste it into the **Reply URL** textbox.</span></span>
   
    <span data-ttu-id="ccf98-169">d.</span><span class="sxs-lookup"><span data-stu-id="ccf98-169">d.</span></span> <span data-ttu-id="ccf98-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf98-170">Click **Next**.</span></span>

7. <span data-ttu-id="ccf98-171">A a **konfigurálhatja az egyszeri bejelentkezés Questetra BPM Suite** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen a tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="ccf98-171">On the **Configure single sign-on at Questetra BPM Suite** page, click **Download certificate**, and then save the certificate file locally on your computer.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][14]

8. <span data-ttu-id="ccf98-173">Az Ön **Questetra BPM Suite** a vállalati webhely, az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ccf98-173">On you **Questetra BPM Suite** company site, perform the following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][15]
   
    <span data-ttu-id="ccf98-175">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-175">a.</span></span> <span data-ttu-id="ccf98-176">Válassza ki **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-176">Select **Enable Single Sign-On**.</span></span>
   
    <span data-ttu-id="ccf98-177">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-177">b.</span></span> <span data-ttu-id="ccf98-178">A klasszikus Azure portálra, másolja a **kiállítójának URL-címe** értékét, és illessze be azt a **Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-178">On the Azure classic portal, copy the **Issuer URL** value, and then paste it into the **Entity ID** textbox.</span></span>
   
    <span data-ttu-id="ccf98-179">c.</span><span class="sxs-lookup"><span data-stu-id="ccf98-179">c.</span></span> <span data-ttu-id="ccf98-180">A klasszikus Azure portálra, másolja a **egyszeri bejelentkezési URL-címe** értékét, és illessze be azt a **bejelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-180">On the Azure classic portal, copy the **Single Sign-On Service URL** value, and then paste it into the **Sign-in page URL** textbox.</span></span>
   
    <span data-ttu-id="ccf98-181">d.</span><span class="sxs-lookup"><span data-stu-id="ccf98-181">d.</span></span> <span data-ttu-id="ccf98-182">A klasszikus Azure portálra, másolja a **egyetlen Sign-Out URL-címe** értékét, és illessze be azt a **kijelentkezési URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-182">On the Azure classic portal, copy the **Single Sign-Out Service URL** value, and then paste it into the **Sign-out page URL** textbox.</span></span>
   
    <span data-ttu-id="ccf98-183">e.</span><span class="sxs-lookup"><span data-stu-id="ccf98-183">e.</span></span> <span data-ttu-id="ccf98-184">Az a **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-184">In the **NameID format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="ccf98-185">f.</span><span class="sxs-lookup"><span data-stu-id="ccf98-185">f.</span></span> <span data-ttu-id="ccf98-186">Hozzon létre egy base-64 kódolású fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ccf98-186">Create a base-64 encoded file from your downloaded certificate.</span></span> 

    >[!TIP] 
    ><span data-ttu-id="ccf98-187">További részletekért lásd: [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="ccf98-187">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

    <span data-ttu-id="ccf98-188">g.</span><span class="sxs-lookup"><span data-stu-id="ccf98-188">g.</span></span> <span data-ttu-id="ccf98-189">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **érvényesítési tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ccf98-189">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **Validation certificate** textbox.</span></span> 

    <span data-ttu-id="ccf98-190">h.</span><span class="sxs-lookup"><span data-stu-id="ccf98-190">h.</span></span> <span data-ttu-id="ccf98-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf98-191">Click **Save**.</span></span>

1. <span data-ttu-id="ccf98-192">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-192">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Mi az az Azure AD Connect?][17]

2. <span data-ttu-id="ccf98-194">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-194">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Mi az az Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ccf98-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf98-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="ccf98-197">Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ccf98-197">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="ccf98-198">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ccf98-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ccf98-199">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-199">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][100] 

2. <span data-ttu-id="ccf98-201">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="ccf98-201">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="ccf98-202">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-202">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][101] 

4. <span data-ttu-id="ccf98-204">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-204">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása][102] 

5. <span data-ttu-id="ccf98-206">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ccf98-206">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][103]
   
    <span data-ttu-id="ccf98-208">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-208">a.</span></span> <span data-ttu-id="ccf98-209">Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-209">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="ccf98-210">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-210">b.</span></span> <span data-ttu-id="ccf98-211">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-211">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="ccf98-212">c.</span><span class="sxs-lookup"><span data-stu-id="ccf98-212">c.</span></span> <span data-ttu-id="ccf98-213">Kattintson a Next (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf98-213">Click Next.</span></span>

6. <span data-ttu-id="ccf98-214">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ccf98-214">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása][104] 
   
    <span data-ttu-id="ccf98-216">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-216">a.</span></span> <span data-ttu-id="ccf98-217">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-217">In the **First Name** textbox, type **Britta**.</span></span> 
   
    <span data-ttu-id="ccf98-218">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-218">b.</span></span> <span data-ttu-id="ccf98-219">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-219">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="ccf98-220">c.</span><span class="sxs-lookup"><span data-stu-id="ccf98-220">c.</span></span> <span data-ttu-id="ccf98-221">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-221">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="ccf98-222">d.</span><span class="sxs-lookup"><span data-stu-id="ccf98-222">d.</span></span> <span data-ttu-id="ccf98-223">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-223">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="ccf98-224">e.</span><span class="sxs-lookup"><span data-stu-id="ccf98-224">e.</span></span> <span data-ttu-id="ccf98-225">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf98-225">Click **Next**.</span></span>

7. <span data-ttu-id="ccf98-226">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-226">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][105]  

8. <span data-ttu-id="ccf98-228">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ccf98-228">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása][106]   
   
    <span data-ttu-id="ccf98-230">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-230">a.</span></span> <span data-ttu-id="ccf98-231">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-231">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="ccf98-232">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-232">b.</span></span> <span data-ttu-id="ccf98-233">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ccf98-233">Click **Complete**.</span></span>   

### <a name="creating-a-questetra-bpm-suite-test-user"></a><span data-ttu-id="ccf98-234">Questetra BPM Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ccf98-234">Creating a Questetra BPM Suite test user</span></span>
<span data-ttu-id="ccf98-235">Ez a szakasz célja Britta Simon Questetra BPM Suite nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ccf98-235">The objective of this section is to create a user called Britta Simon in Questetra BPM Suite.</span></span>

<span data-ttu-id="ccf98-236">**A felhasználó Britta Simon Questetra BPM Suite nevű létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ccf98-236">**To create a user called Britta Simon in Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="ccf98-237">Bejelentkezés a Questetra BPM Suite vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ccf98-237">Sign-on to your Questetra BPM Suite company site as an administrator.</span></span>
2. <span data-ttu-id="ccf98-238">Ugrás a **Rendszerbeállítások > Felhasználólista > Új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-238">Go to **System Settings > User List > New User**.</span></span> 
3. <span data-ttu-id="ccf98-239">Új felhasználó párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ccf98-239">On the New User dialog, perform the following steps:</span></span> 
   
    ![Tesztfelhasználó létrehozása][300] 
   
    <span data-ttu-id="ccf98-241">a.</span><span class="sxs-lookup"><span data-stu-id="ccf98-241">a.</span></span> <span data-ttu-id="ccf98-242">Az a **neve** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ccf98-242">In the **Name** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="ccf98-243">b.</span><span class="sxs-lookup"><span data-stu-id="ccf98-243">b.</span></span> <span data-ttu-id="ccf98-244">Az a **E-mail** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ccf98-244">In the **Email** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="ccf98-245">c.</span><span class="sxs-lookup"><span data-stu-id="ccf98-245">c.</span></span> <span data-ttu-id="ccf98-246">Az a **jelszó** szövegmező, a jelszót írja be.</span><span class="sxs-lookup"><span data-stu-id="ccf98-246">In the **Password** textbox, type a password.</span></span>

4. <span data-ttu-id="ccf98-247">Kattintson a **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-247">Click **Add new user**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ccf98-248">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ccf98-248">Assigning the Azure AD test user</span></span>
<span data-ttu-id="ccf98-249">Ez a szakasz célja Britta Simon által biztosított a hozzáférés Questetra BPM Suite használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ccf98-249">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Questetra BPM Suite.</span></span>

![Mi az az Azure AD Connect?][200]

<span data-ttu-id="ccf98-251">**Britta Simon hozzárendelése Questetra BPM Suite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ccf98-251">**To assign Britta Simon to Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="ccf98-252">A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="ccf98-252">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Mi az az Azure AD Connect?][201]
2. <span data-ttu-id="ccf98-254">Az alkalmazások listában válassza ki a **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-254">In the applications list, select **Questetra BPM Suite**.</span></span>
   
    ![Mi az az Azure AD Connect?][205]
3. <span data-ttu-id="ccf98-256">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-256">In the menu on the top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][202]
4. <span data-ttu-id="ccf98-258">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-258">In the Users list, select **Britta Simon**.</span></span>
   
    ![Mi az az Azure AD Connect?][203]
5. <span data-ttu-id="ccf98-260">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="ccf98-260">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Mi az az Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a><span data-ttu-id="ccf98-262">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ccf98-262">Testing Single Sign-On</span></span>
<span data-ttu-id="ccf98-263">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="ccf98-263">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="ccf98-264">Ha a hozzáférési panelen Questetra BPM Suite csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Questetra BPM Suite alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ccf98-264">When you click the Questetra BPM Suite tile in the Access Panel, you should get automatically signed-on to your Questetra BPM Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ccf98-265">További források</span><span class="sxs-lookup"><span data-stu-id="ccf98-265">Additional Resources</span></span>
* [<span data-ttu-id="ccf98-266">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ccf98-266">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ccf98-267">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ccf98-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
