---
title: "Oktatóanyag: Azure Active Directoryval integrált ITRP |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ITRP között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 35463a55fcfc1e55c90700737961c1ff2e58992a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="de085-103">Oktatóanyag: Azure Active Directoryval integrált ITRP</span><span class="sxs-lookup"><span data-stu-id="de085-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="de085-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ITRP az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="de085-104">In this tutorial, you learn how toointegrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="de085-105">ITRP integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="de085-105">Integrating ITRP with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="de085-106">Megadhatja a hozzáférés tooITRP rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="de085-106">You can control in Azure AD who has access tooITRP</span></span>
- <span data-ttu-id="de085-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooITRP (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="de085-107">You can enable your users tooautomatically get signed-on tooITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="de085-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="de085-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="de085-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="de085-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de085-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de085-110">Prerequisites</span></span>

<span data-ttu-id="de085-111">az Azure AD integrálása ITRP tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="de085-111">tooconfigure Azure AD integration with ITRP, you need hello following items:</span></span>

- <span data-ttu-id="de085-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="de085-112">An Azure AD subscription</span></span>
- <span data-ttu-id="de085-113">Egy ITRP egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="de085-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="de085-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="de085-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="de085-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="de085-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="de085-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="de085-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="de085-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de085-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="de085-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="de085-118">Scenario description</span></span>
<span data-ttu-id="de085-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="de085-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="de085-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="de085-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="de085-121">Hello gyűjteményből ITRP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de085-121">Adding ITRP from hello gallery</span></span>
2. <span data-ttu-id="de085-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de085-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-hello-gallery"></a><span data-ttu-id="de085-123">Hello gyűjteményből ITRP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de085-123">Adding ITRP from hello gallery</span></span>
<span data-ttu-id="de085-124">tooconfigure hello integrációja ITRP tooAzure AD a, meg kell tooadd ITRP hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="de085-124">tooconfigure hello integration of ITRP in tooAzure AD, you need tooadd ITRP from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="de085-125">**tooadd ITRP hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="de085-125">**tooadd ITRP from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="de085-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de085-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="de085-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="de085-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="de085-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de085-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="de085-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="de085-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="de085-133">Hello keresési mezőbe, írja be a **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="de085-133">In hello search box, type **ITRP**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="de085-135">A hello eredmények panelen válassza ki a **ITRP**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-135">In hello results panel, select **ITRP**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="de085-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de085-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="de085-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján ITRP</span><span class="sxs-lookup"><span data-stu-id="de085-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="de085-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ITRP tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="de085-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ITRP is tooa user in Azure AD.</span></span> <span data-ttu-id="de085-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ITRP közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="de085-140">In other words, a link relationship between an Azure AD user and hello related user in ITRP needs toobe established.</span></span>

<span data-ttu-id="de085-141">ITRP, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="de085-141">In ITRP, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="de085-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ITRP-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="de085-142">tooconfigure and test Azure AD single sign-on with ITRP, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="de085-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="de085-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="de085-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="de085-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="de085-145">**[Tesztfelhasználó létrehozása egy ITRP](#creating-an-itrp-test-user)**  -toohave egy megfelelője a Britta Simon a ITRP, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="de085-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - toohave a counterpart of Britta Simon in ITRP that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="de085-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="de085-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="de085-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="de085-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="de085-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de085-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="de085-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ITRP alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="de085-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="de085-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ITRP, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="de085-150">**tooconfigure Azure AD single sign-on with ITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="de085-151">Az Azure portál, a hello hello **ITRP** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="de085-151">In hello Azure portal, on hello **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="de085-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="de085-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="de085-155">A hello **ITRP tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="de085-155">On hello **ITRP Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="de085-157">a.</span><span class="sxs-lookup"><span data-stu-id="de085-157">a.</span></span> <span data-ttu-id="de085-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="de085-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="de085-159">b.</span><span class="sxs-lookup"><span data-stu-id="de085-159">b.</span></span> <span data-ttu-id="de085-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="de085-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="de085-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="de085-161">These values are not real.</span></span> <span data-ttu-id="de085-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="de085-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="de085-163">Ügyfél [ITRP ügyfél-támogatási csoport](https://www.itrp.com/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="de085-163">Contact [ITRP Client support team](https://www.itrp.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="de085-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="de085-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="de085-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="de085-168">A hello **ITRP konfigurációs** kattintson **konfigurálása ITRP** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="de085-168">On hello **ITRP Configuration** section, click **Configure ITRP** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="de085-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="de085-169">Copy hello **SAML Single Sign-On Service URL and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="de085-171">Egy másik webes böngészőablakban jelentkezzen tooyour ITRP vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="de085-171">In a different web browser window, log in tooyour ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="de085-172">Hello hello felső eszköztárán kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="de085-172">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="de085-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="de085-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="de085-174">Hello bal oldali navigációs panelen, jelölje ki a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="de085-174">In hello left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="de085-175">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775571.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="de085-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="de085-176">Az egyszeri bejelentkezést a konfigurációs szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="de085-176">In hello Single Sign-On configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="de085-177">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775572.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="de085-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="de085-178">![Egyszeri bejelentkezés](./media/active-directory-saas-itrp-tutorial/ic775573.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="de085-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="de085-179">a.</span><span class="sxs-lookup"><span data-stu-id="de085-179">a.</span></span> <span data-ttu-id="de085-180">Kattintson az **Engedélyezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-180">Click **Enable**.</span></span>

    <span data-ttu-id="de085-181">b.</span><span class="sxs-lookup"><span data-stu-id="de085-181">b.</span></span> <span data-ttu-id="de085-182">A **távoli napló kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="de085-182">In **Remote Log Out URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="de085-183">c.</span><span class="sxs-lookup"><span data-stu-id="de085-183">c.</span></span> <span data-ttu-id="de085-184">A **SAML SSO URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="de085-184">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="de085-185">d.In **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="de085-185">d.In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="de085-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="de085-187">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="de085-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="de085-188">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="de085-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="de085-189">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="de085-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="de085-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de085-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="de085-191">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="de085-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="de085-193">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="de085-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="de085-194">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de085-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="de085-196">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="de085-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="de085-198">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de085-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="de085-200">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="de085-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="de085-202">a.</span><span class="sxs-lookup"><span data-stu-id="de085-202">a.</span></span> <span data-ttu-id="de085-203">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="de085-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="de085-204">b.</span><span class="sxs-lookup"><span data-stu-id="de085-204">b.</span></span> <span data-ttu-id="de085-205">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="de085-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="de085-206">c.</span><span class="sxs-lookup"><span data-stu-id="de085-206">c.</span></span> <span data-ttu-id="de085-207">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="de085-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="de085-208">d.</span><span class="sxs-lookup"><span data-stu-id="de085-208">d.</span></span> <span data-ttu-id="de085-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="de085-210">Egy ITRP tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de085-210">Creating an ITRP test user</span></span>

<span data-ttu-id="de085-211">az Azure AD tooenable felhasználók toolog tooITRP, az ezeket ki kell építenie a tooITRP.</span><span class="sxs-lookup"><span data-stu-id="de085-211">tooenable Azure AD users toolog in tooITRP, they must be provisioned in tooITRP.</span></span>  

<span data-ttu-id="de085-212">ITRP hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="de085-212">In hello case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="de085-213">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="de085-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="de085-214">Jelentkezzen be tooyour **ITRP** bérlő.</span><span class="sxs-lookup"><span data-stu-id="de085-214">Log in tooyour **ITRP** tenant.</span></span>

2. <span data-ttu-id="de085-215">Hello hello felső eszköztárán kattintson **rekordok**.</span><span class="sxs-lookup"><span data-stu-id="de085-215">In hello toolbar on hello top, click **Records**.</span></span>
   
    <span data-ttu-id="de085-216">![Felügyeleti](./media/active-directory-saas-itrp-tutorial/ic775575.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="de085-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="de085-217">Hello előugró menüből válassza ki a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="de085-217">From hello popup menu, select **People**.</span></span>
   
    <span data-ttu-id="de085-218">![Személyek](./media/active-directory-saas-itrp-tutorial/ic775587.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="de085-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="de085-219">Kattintson a **adja hozzá az új személy** ("+").</span><span class="sxs-lookup"><span data-stu-id="de085-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="de085-220">![Felügyeleti](./media/active-directory-saas-itrp-tutorial/ic775576.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="de085-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="de085-221">Hello új személy hozzáadása párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="de085-221">On hello Add New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="de085-222">![Felhasználói](./media/active-directory-saas-itrp-tutorial/ic775577.png "felhasználó")</span><span class="sxs-lookup"><span data-stu-id="de085-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="de085-223">a.</span><span class="sxs-lookup"><span data-stu-id="de085-223">a.</span></span> <span data-ttu-id="de085-224">Típus hello **neve**, **E-mail** tooprovision kívánt fiók érvényes aad-ben.</span><span class="sxs-lookup"><span data-stu-id="de085-224">Type hello **Name**, **Email** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="de085-225">b.</span><span class="sxs-lookup"><span data-stu-id="de085-225">b.</span></span> <span data-ttu-id="de085-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="de085-227">Bármely más ITRP felhasználói fiók létrehozása eszközök vagy ITRP tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="de085-227">You can use any other ITRP user account creation tools or APIs provided by ITRP tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="de085-228">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="de085-228">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="de085-229">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooITRP megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="de085-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooITRP.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="de085-231">**tooassign Britta Simon tooITRP, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="de085-231">**tooassign Britta Simon tooITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="de085-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de085-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="de085-234">Hello alkalmazások listában válassza ki a **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="de085-234">In hello applications list, select **ITRP**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="de085-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="de085-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="de085-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="de085-238">Click **Add** button.</span></span> <span data-ttu-id="de085-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de085-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="de085-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="de085-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="de085-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de085-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="de085-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de085-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="de085-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="de085-244">Testing single sign-on</span></span>

<span data-ttu-id="de085-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="de085-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="de085-246">Hello ITRP hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ITRP alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="de085-246">When you click hello ITRP tile in hello Access Panel, you should get automatically signed-on tooyour ITRP application.</span></span>
<span data-ttu-id="de085-247">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="de085-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="de085-248">További források</span><span class="sxs-lookup"><span data-stu-id="de085-248">Additional resources</span></span>

* [<span data-ttu-id="de085-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="de085-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="de085-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="de085-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

