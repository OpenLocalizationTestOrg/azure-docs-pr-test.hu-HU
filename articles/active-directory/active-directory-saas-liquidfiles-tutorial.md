---
title: "Oktatóanyag: Azure Active Directoryval integrált LiquidFiles |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LiquidFiles között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 67eb888090f81e0ceb791ed45d564b98fe1eb6d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a><span data-ttu-id="617c4-103">Oktatóanyag: Azure Active Directoryval integrált LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="617c4-103">Tutorial: Azure Active Directory integration with LiquidFiles</span></span>

<span data-ttu-id="617c4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LiquidFiles az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="617c4-104">In this tutorial, you learn how toointegrate LiquidFiles with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="617c4-105">LiquidFiles integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="617c4-105">Integrating LiquidFiles with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="617c4-106">Megadhatja a hozzáférés tooLiquidFiles rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="617c4-106">You can control in Azure AD who has access tooLiquidFiles</span></span>
- <span data-ttu-id="617c4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLiquidFiles (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="617c4-107">You can enable your users tooautomatically get signed-on tooLiquidFiles (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="617c4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="617c4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="617c4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="617c4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="617c4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="617c4-110">Prerequisites</span></span>

<span data-ttu-id="617c4-111">az Azure AD integrálása LiquidFiles tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="617c4-111">tooconfigure Azure AD integration with LiquidFiles, you need hello following items:</span></span>

- <span data-ttu-id="617c4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="617c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="617c4-113">Egy LiquidFiles egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="617c4-113">A LiquidFiles single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="617c4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="617c4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="617c4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="617c4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="617c4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="617c4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="617c4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="617c4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="617c4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="617c4-118">Scenario description</span></span>
<span data-ttu-id="617c4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="617c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="617c4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="617c4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="617c4-121">Hello gyűjteményből LiquidFiles hozzáadása</span><span class="sxs-lookup"><span data-stu-id="617c4-121">Adding LiquidFiles from hello gallery</span></span>
2. <span data-ttu-id="617c4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="617c4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-liquidfiles-from-hello-gallery"></a><span data-ttu-id="617c4-123">Hello gyűjteményből LiquidFiles hozzáadása</span><span class="sxs-lookup"><span data-stu-id="617c4-123">Adding LiquidFiles from hello gallery</span></span>
<span data-ttu-id="617c4-124">tooconfigure hello integrációja LiquidFiles az Azure AD-be, meg kell tooadd LiquidFiles hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="617c4-124">tooconfigure hello integration of LiquidFiles into Azure AD, you need tooadd LiquidFiles from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="617c4-125">**tooadd LiquidFiles hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="617c4-125">**tooadd LiquidFiles from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="617c4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="617c4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="617c4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="617c4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="617c4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="617c4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="617c4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="617c4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="617c4-133">Hello keresési mezőbe, írja be a **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="617c4-133">In hello search box, type **LiquidFiles**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. <span data-ttu-id="617c4-135">A hello eredmények panelen válassza ki a **LiquidFiles**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="617c4-135">In hello results panel, select **LiquidFiles**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="617c4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="617c4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="617c4-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="617c4-138">In this section, you configure and test Azure AD single sign-on with LiquidFiles based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="617c4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LiquidFiles tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="617c4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LiquidFiles is tooa user in Azure AD.</span></span> <span data-ttu-id="617c4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LiquidFiles közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="617c4-140">In other words, a link relationship between an Azure AD user and hello related user in LiquidFiles needs toobe established.</span></span>

<span data-ttu-id="617c4-141">LiquidFiles, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="617c4-141">In LiquidFiles, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="617c4-142">tooconfigure és az Azure AD az egyszeri bejelentkezés LiquidFiles-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="617c4-142">tooconfigure and test Azure AD single sign-on with LiquidFiles, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="617c4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="617c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="617c4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="617c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="617c4-145">**[LiquidFiles tesztfelhasználó létrehozása](#creating-a-liquidfiles-test-user)**  -toohave egy megfelelője a Britta Simon a LiquidFiles, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="617c4-145">**[Creating a LiquidFiles test user](#creating-a-liquidfiles-test-user)** - toohave a counterpart of Britta Simon in LiquidFiles that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="617c4-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="617c4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="617c4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="617c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="617c4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="617c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="617c4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az LiquidFiles alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="617c4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LiquidFiles application.</span></span>

<span data-ttu-id="617c4-150">**az Azure AD tooconfigure egyszeri bejelentkezést a LiquidFiles, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="617c4-150">**tooconfigure Azure AD single sign-on with LiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="617c4-151">Az Azure portál, a hello hello **LiquidFiles** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="617c4-151">In hello Azure portal, on hello **LiquidFiles** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="617c4-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="617c4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. <span data-ttu-id="617c4-155">A hello **LiquidFiles tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="617c4-155">On hello **LiquidFiles Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    <span data-ttu-id="617c4-157">a.</span><span class="sxs-lookup"><span data-stu-id="617c4-157">a.</span></span> <span data-ttu-id="617c4-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<YOUR_SERVER_URL>/saml/init`</span><span class="sxs-lookup"><span data-stu-id="617c4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/init`</span></span>

    <span data-ttu-id="617c4-159">b.</span><span class="sxs-lookup"><span data-stu-id="617c4-159">b.</span></span> <span data-ttu-id="617c4-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<YOUR_SERVER_URL>`</span><span class="sxs-lookup"><span data-stu-id="617c4-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>`</span></span>

    <span data-ttu-id="617c4-161">c.</span><span class="sxs-lookup"><span data-stu-id="617c4-161">c.</span></span> <span data-ttu-id="617c4-162">b.</span><span class="sxs-lookup"><span data-stu-id="617c4-162">b.</span></span> <span data-ttu-id="617c4-163">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<YOUR_SERVER_URL>/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="617c4-163">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="617c4-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="617c4-164">These values are not real.</span></span> <span data-ttu-id="617c4-165">Ezek az értékek frissítés tényleges bejelentkezési URL-címe, hello azonosítója és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="617c4-165">Update these values with hello actual Sign-On URL, Identifier and, Reply URL.</span></span> <span data-ttu-id="617c4-166">Ügyfél [LiquidFiles ügyfél-támogatási csoport](https://www.liquidfiles.com/support.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="617c4-166">Contact [LiquidFiles Client support team](https://www.liquidfiles.com/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="617c4-167">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="617c4-167">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. <span data-ttu-id="617c4-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="617c4-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="617c4-171">A hello **LiquidFiles konfigurációs** kattintson **konfigurálása LiquidFiles** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="617c4-171">On hello **LiquidFiles Configuration** section, click **Configure LiquidFiles** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="617c4-172">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="617c4-172">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. <span data-ttu-id="617c4-174">Bejelentkezés tooyour LiquidFiles vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="617c4-174">Sign-on tooyour LiquidFiles company site as administrator.</span></span>

8. <span data-ttu-id="617c4-175">Kattintson a **egyszeri bejelentkezés** a hello **Admin > konfigurációs** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="617c4-175">Click **Single Sign-On** in hello **Admin > Configuration** from hello menu.</span></span>

9. <span data-ttu-id="617c4-176">A hello **egyszeri bejelentkezés konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello</span><span class="sxs-lookup"><span data-stu-id="617c4-176">On hello **Single Sign-On Configuration** page, perform hello following steps</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    <span data-ttu-id="617c4-178">a.</span><span class="sxs-lookup"><span data-stu-id="617c4-178">a.</span></span> <span data-ttu-id="617c4-179">Mint **módszer egyszeri bejelentkezési**, jelölje be **SAML 2**.</span><span class="sxs-lookup"><span data-stu-id="617c4-179">As **Single Sign On Method**, select **SAML 2**.</span></span>

    <span data-ttu-id="617c4-180">b.</span><span class="sxs-lookup"><span data-stu-id="617c4-180">b.</span></span> <span data-ttu-id="617c4-181">A hello **IDP bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="617c4-181">In hello **IDP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="617c4-182">c.</span><span class="sxs-lookup"><span data-stu-id="617c4-182">c.</span></span> <span data-ttu-id="617c4-183">A hello **IDP kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="617c4-183">In hello **IDP Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="617c4-184">d.</span><span class="sxs-lookup"><span data-stu-id="617c4-184">d.</span></span> <span data-ttu-id="617c4-185">A hello **IDP tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **UJJLENYOMAT** érték, amely az Azure-portálon másolta...</span><span class="sxs-lookup"><span data-stu-id="617c4-185">In hello **IDP Cert Fingerprint** textbox, paste hello **THUMBPRINT** value which you have copied from Azure portal..</span></span>

    <span data-ttu-id="617c4-186">e.</span><span class="sxs-lookup"><span data-stu-id="617c4-186">e.</span></span> <span data-ttu-id="617c4-187">Hello azonosító formátuma szövegmezőben, írja be a hello értéket **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="617c4-187">In hello Name Identifier Format textbox, type hello value **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="617c4-188">f.</span><span class="sxs-lookup"><span data-stu-id="617c4-188">f.</span></span> <span data-ttu-id="617c4-189">A hello Authn környezetben szövegmező, írja be a hello érték **urn: oasis: nevek: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.</span><span class="sxs-lookup"><span data-stu-id="617c4-189">In hello Authn Context textbox, type hello value **urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport**.</span></span>

    <span data-ttu-id="617c4-190">g.</span><span class="sxs-lookup"><span data-stu-id="617c4-190">g.</span></span> <span data-ttu-id="617c4-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="617c4-191">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="617c4-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="617c4-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="617c4-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="617c4-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="617c4-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="617c4-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="617c4-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="617c4-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="617c4-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="617c4-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="617c4-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="617c4-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="617c4-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="617c4-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="617c4-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="617c4-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="617c4-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="617c4-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="617c4-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="617c4-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="617c4-207">a.</span><span class="sxs-lookup"><span data-stu-id="617c4-207">a.</span></span> <span data-ttu-id="617c4-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="617c4-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="617c4-209">b.</span><span class="sxs-lookup"><span data-stu-id="617c4-209">b.</span></span> <span data-ttu-id="617c4-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="617c4-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="617c4-211">c.</span><span class="sxs-lookup"><span data-stu-id="617c4-211">c.</span></span> <span data-ttu-id="617c4-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="617c4-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="617c4-213">d.</span><span class="sxs-lookup"><span data-stu-id="617c4-213">d.</span></span> <span data-ttu-id="617c4-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="617c4-214">Click **Create**.</span></span>
 
### <a name="creating-a-liquidfiles-test-user"></a><span data-ttu-id="617c4-215">LiquidFiles tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="617c4-215">Creating a LiquidFiles test user</span></span>

<span data-ttu-id="617c4-216">hello ebben a szakaszban célja toocreate LiquidFiles Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="617c4-216">hello objective of this section is toocreate a user called Britta Simon in LiquidFiles.</span></span> <span data-ttu-id="617c4-217">Működik a LiquidFiles server rendszergazda tooget saját kezűleg felhasználójává tooyour LiquidFiles alkalmazás a bejelentkezés előtt.</span><span class="sxs-lookup"><span data-stu-id="617c4-217">Work with your LiquidFiles server administrator tooget yourself added as a user before logging in tooyour LiquidFiles application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="617c4-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="617c4-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="617c4-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLiquidFiles megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="617c4-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLiquidFiles.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="617c4-221">**tooassign Britta Simon tooLiquidFiles, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="617c4-221">**tooassign Britta Simon tooLiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="617c4-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="617c4-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="617c4-224">Hello alkalmazások listában válassza ki a **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="617c4-224">In hello applications list, select **LiquidFiles**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. <span data-ttu-id="617c4-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="617c4-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="617c4-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="617c4-228">Click **Add** button.</span></span> <span data-ttu-id="617c4-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="617c4-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="617c4-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="617c4-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="617c4-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="617c4-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="617c4-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="617c4-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="617c4-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="617c4-234">Testing single sign-on</span></span>

<span data-ttu-id="617c4-235">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="617c4-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="617c4-236">Hello LiquidFiles hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour LiquidFiles alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="617c4-236">When you click hello LiquidFiles tile in hello Access Panel, you should get automatically signed-on tooyour LiquidFiles application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="617c4-237">További források</span><span class="sxs-lookup"><span data-stu-id="617c4-237">Additional resources</span></span>

* [<span data-ttu-id="617c4-238">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="617c4-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="617c4-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="617c4-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

