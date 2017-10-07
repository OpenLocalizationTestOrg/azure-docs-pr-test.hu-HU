---
title: "Oktatóanyag: Azure Active Directoryval integrált Hackerone |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Hackerone között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: c9dc033e26e79a7233dcfb3899c62684d4a19652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="85cee-103">Oktatóanyag: Azure Active Directoryval integrált HackerOne</span><span class="sxs-lookup"><span data-stu-id="85cee-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="85cee-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HackerOne az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="85cee-104">In this tutorial, you learn how toointegrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="85cee-105">HackerOne integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-105">Integrating HackerOne with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="85cee-106">Megadhatja a hozzáférés tooHackerOne rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="85cee-106">You can control in Azure AD who has access tooHackerOne</span></span>
- <span data-ttu-id="85cee-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHackerOne (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="85cee-107">You can enable your users tooautomatically get signed-on tooHackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="85cee-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="85cee-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="85cee-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="85cee-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85cee-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="85cee-110">Prerequisites</span></span>

<span data-ttu-id="85cee-111">az Azure AD integrálása HackerOne tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-111">tooconfigure Azure AD integration with HackerOne, you need hello following items:</span></span>

- <span data-ttu-id="85cee-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="85cee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="85cee-113">Egy HackerOne egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="85cee-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="85cee-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="85cee-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="85cee-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="85cee-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="85cee-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="85cee-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="85cee-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="85cee-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="85cee-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="85cee-118">Scenario description</span></span>
<span data-ttu-id="85cee-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="85cee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="85cee-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="85cee-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="85cee-121">Hello gyűjteményből HackerOne hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85cee-121">Adding HackerOne from hello gallery</span></span>
2. <span data-ttu-id="85cee-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="85cee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-hello-gallery"></a><span data-ttu-id="85cee-123">Hello gyűjteményből HackerOne hozzáadása</span><span class="sxs-lookup"><span data-stu-id="85cee-123">Adding HackerOne from hello gallery</span></span>
<span data-ttu-id="85cee-124">tooconfigure hello integrációja HackerOne az Azure AD-be, meg kell tooadd HackerOne hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="85cee-124">tooconfigure hello integration of HackerOne into Azure AD, you need tooadd HackerOne from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="85cee-125">**tooadd HackerOne hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="85cee-125">**tooadd HackerOne from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="85cee-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="85cee-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="85cee-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="85cee-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="85cee-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="85cee-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="85cee-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="85cee-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="85cee-133">Hello keresési mezőbe, írja be a **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="85cee-133">In hello search box, type **HackerOne**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="85cee-135">A hello eredmények panelen válassza ki a **HackerOne**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="85cee-135">In hello results panel, select **HackerOne**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="85cee-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="85cee-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="85cee-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján HackerOne</span><span class="sxs-lookup"><span data-stu-id="85cee-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="85cee-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó HackerOne tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="85cee-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HackerOne is tooa user in Azure AD.</span></span> <span data-ttu-id="85cee-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello HackerOne közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="85cee-140">In other words, a link relationship between an Azure AD user and hello related user in HackerOne needs toobe established.</span></span>

<span data-ttu-id="85cee-141">HackerOne, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="85cee-141">In HackerOne, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="85cee-142">tooconfigure és az Azure AD az egyszeri bejelentkezés HackerOne-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="85cee-142">tooconfigure and test Azure AD single sign-on with HackerOne, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="85cee-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="85cee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="85cee-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85cee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="85cee-145">**[HackerOne tesztfelhasználó létrehozása](#creating-a-hackerone-test-user)**  -toohave egy megfelelője a Britta Simon a HackerOne, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="85cee-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - toohave a counterpart of Britta Simon in HackerOne that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="85cee-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="85cee-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="85cee-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="85cee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="85cee-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85cee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="85cee-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az HackerOne alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="85cee-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="85cee-150">**az Azure AD tooconfigure egyszeri bejelentkezést a HackerOne, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="85cee-150">**tooconfigure Azure AD single sign-on with HackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="85cee-151">Az Azure portál, a hello hello **HackerOne** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="85cee-151">In hello Azure portal, on hello **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="85cee-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="85cee-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="85cee-155">A hello **HackerOne egyszeri bejelentkezési URL-cím és azonosító** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-155">On hello **HackerOne Single sign-on URL and Identifier** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="85cee-157">a.</span><span class="sxs-lookup"><span data-stu-id="85cee-157">a.</span></span> <span data-ttu-id="85cee-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="85cee-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="85cee-159">b.</span><span class="sxs-lookup"><span data-stu-id="85cee-159">b.</span></span> <span data-ttu-id="85cee-160">A hello **azonosító** szövegmező, adja meg az URL-címet:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="85cee-160">In hello **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="85cee-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="85cee-161">This value is not real.</span></span> <span data-ttu-id="85cee-162">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="85cee-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="85cee-163">Ügyfél [HackerOne támogatási csoport](mailto:support@hackerone.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="85cee-163">Contact [HackerOne support team](mailto:support@hackerone.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="85cee-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="85cee-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="85cee-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="85cee-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="85cee-168">A hello **HackerOne konfigurációs** kattintson **konfigurálása HackerOne** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="85cee-168">On hello **HackerOne Configuration** section, click **Configure HackerOne** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="85cee-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="85cee-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="85cee-171">Bejelentkezés tooyour HackerOne Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="85cee-171">Sign On tooyour HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="85cee-172">Hello hello felső menüben kattintson a hello "**beállítások**."</span><span class="sxs-lookup"><span data-stu-id="85cee-172">In hello menu on hello top, click hello "**Settings**."</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="85cee-174">Keresse meg a túl"**hitelesítési**", és kattintson a"**SAML beállítások hozzáadása**."</span><span class="sxs-lookup"><span data-stu-id="85cee-174">Navigate too"**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="85cee-176">A hello **SAML beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-176">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="85cee-178">a.</span><span class="sxs-lookup"><span data-stu-id="85cee-178">a.</span></span> <span data-ttu-id="85cee-179">A hello **E-mail tartománya** szövegmező, adjon meg egy regisztrált tartományt.</span><span class="sxs-lookup"><span data-stu-id="85cee-179">In hello **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="85cee-180">b.</span><span class="sxs-lookup"><span data-stu-id="85cee-180">b.</span></span> <span data-ttu-id="85cee-181">A **egyszeri bejelentkezési URL-cím** szövegmezőből, illessze be a hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="85cee-181">In  **Single Sign On URL** textboxes, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="85cee-182">c.</span><span class="sxs-lookup"><span data-stu-id="85cee-182">c.</span></span> <span data-ttu-id="85cee-183">Nyissa meg a **tanúsítványfájl** a Jegyzettömbben az Azure portálról letöltött hello tartalmát, másolja a vágólapra és toohello beillesztési **X509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="85cee-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="85cee-184">d.</span><span class="sxs-lookup"><span data-stu-id="85cee-184">d.</span></span> <span data-ttu-id="85cee-185">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="85cee-185">Click **Save**.</span></span>

11. <span data-ttu-id="85cee-186">Hello hitelesítési beállítások párbeszédpanel hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-186">On hello Authentication Settings dialog, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="85cee-188">a.</span><span class="sxs-lookup"><span data-stu-id="85cee-188">a.</span></span> <span data-ttu-id="85cee-189">Kattintson a **teszt futtatása**.</span><span class="sxs-lookup"><span data-stu-id="85cee-189">Click **Run test**.</span></span>

    <span data-ttu-id="85cee-190">b.</span><span class="sxs-lookup"><span data-stu-id="85cee-190">b.</span></span> <span data-ttu-id="85cee-191">Ha hello értékének hello **állapot** mezőbe egyenlő **legutóbbi teszt állapota: létrehozott**, forduljon a [HackerOne támogatási csoport](mailto:support@hackerone.com) toorequest a konfiguráció áttekintése.</span><span class="sxs-lookup"><span data-stu-id="85cee-191">If hello value of hello **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) toorequest a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="85cee-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="85cee-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="85cee-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="85cee-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="85cee-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="85cee-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="85cee-195">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="85cee-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="85cee-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="85cee-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="85cee-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="85cee-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="85cee-199">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="85cee-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="85cee-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="85cee-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="85cee-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85cee-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="85cee-205">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="85cee-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="85cee-207">a.</span><span class="sxs-lookup"><span data-stu-id="85cee-207">a.</span></span> <span data-ttu-id="85cee-208">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="85cee-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="85cee-209">b.</span><span class="sxs-lookup"><span data-stu-id="85cee-209">b.</span></span> <span data-ttu-id="85cee-210">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="85cee-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="85cee-211">c.</span><span class="sxs-lookup"><span data-stu-id="85cee-211">c.</span></span> <span data-ttu-id="85cee-212">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="85cee-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="85cee-213">d.</span><span class="sxs-lookup"><span data-stu-id="85cee-213">d.</span></span> <span data-ttu-id="85cee-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="85cee-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="85cee-215">HackerOne tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="85cee-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="85cee-216">Ezután hozzon létre egy HackerOne Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="85cee-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="85cee-217">HackerOne támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="85cee-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="85cee-218">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="85cee-218">There is no action item for you in this section.</span></span> <span data-ttu-id="85cee-219">Amikor HackerOne fér hozzá, egy új felhasználó jön létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="85cee-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="85cee-220">Ha egy felhasználó toocreate manuálisan kell, kell toocontact hello Certify támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="85cee-220">If you need toocreate a user manually, you need toocontact hello Certify support team.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="85cee-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="85cee-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="85cee-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHackerOne megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="85cee-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHackerOne.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="85cee-224">**tooassign Britta Simon tooHackerOne, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="85cee-224">**tooassign Britta Simon tooHackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="85cee-225">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="85cee-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="85cee-227">Hello alkalmazások listában válassza ki a **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="85cee-227">In hello applications list, select **HackerOne**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="85cee-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="85cee-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="85cee-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="85cee-231">Click **Add** button.</span></span> <span data-ttu-id="85cee-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85cee-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="85cee-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="85cee-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="85cee-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85cee-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="85cee-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="85cee-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="85cee-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="85cee-237">Testing single sign-on</span></span>

<span data-ttu-id="85cee-238">Végezetül tesztelése az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="85cee-238">Finally, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="85cee-239">Ha a hozzáférési Panel hello hello HackerOne csempe gombra kattint, automatikusan bejelentkezett tooyour HackerOne alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="85cee-239">When you click hello HackerOne tile in hello Access Panel, you should get automatically signed-on tooyour HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85cee-240">További források</span><span class="sxs-lookup"><span data-stu-id="85cee-240">Additional resources</span></span>

* [<span data-ttu-id="85cee-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="85cee-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="85cee-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="85cee-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

