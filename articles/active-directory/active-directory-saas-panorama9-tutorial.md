---
title: "Oktatóanyag: Azure Active Directoryval integrált Panorama9 |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Panorama9 között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 548fb6434d920e076db98a0193f8dfdf8a958a91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="bf015-103">Oktatóanyag: Azure Active Directoryval integrált Panorama9</span><span class="sxs-lookup"><span data-stu-id="bf015-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="bf015-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Panorama9 az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bf015-104">In this tutorial, you learn how toointegrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bf015-105">Panorama9 integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bf015-105">Integrating Panorama9 with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bf015-106">Megadhatja a hozzáférés tooPanorama9 rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bf015-106">You can control in Azure AD who has access tooPanorama9</span></span>
- <span data-ttu-id="bf015-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPanorama9 (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bf015-107">You can enable your users tooautomatically get signed-on tooPanorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bf015-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bf015-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bf015-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bf015-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf015-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf015-110">Prerequisites</span></span>

<span data-ttu-id="bf015-111">az Azure AD integrálása Panorama9 tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bf015-111">tooconfigure Azure AD integration with Panorama9, you need hello following items:</span></span>

- <span data-ttu-id="bf015-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bf015-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bf015-113">Egy Panorama9 egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bf015-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bf015-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bf015-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bf015-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bf015-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bf015-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bf015-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bf015-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bf015-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bf015-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bf015-118">Scenario description</span></span>
<span data-ttu-id="bf015-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bf015-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bf015-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bf015-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bf015-121">Hello gyűjteményből Panorama9 hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf015-121">Adding Panorama9 from hello gallery</span></span>
2. <span data-ttu-id="bf015-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bf015-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-hello-gallery"></a><span data-ttu-id="bf015-123">Hello gyűjteményből Panorama9 hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bf015-123">Adding Panorama9 from hello gallery</span></span>
<span data-ttu-id="bf015-124">tooconfigure hello integrációja Panorama9 az Azure AD-be, meg kell tooadd Panorama9 hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bf015-124">tooconfigure hello integration of Panorama9 into Azure AD, you need tooadd Panorama9 from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bf015-125">**tooadd Panorama9 hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bf015-125">**tooadd Panorama9 from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf015-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bf015-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bf015-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bf015-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bf015-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bf015-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bf015-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bf015-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bf015-133">Hello keresési mezőbe, írja be a **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="bf015-133">In hello search box, type **Panorama9**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="bf015-135">A hello eredmények panelen válassza ki a **Panorama9**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bf015-135">In hello results panel, select **Panorama9**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bf015-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bf015-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="bf015-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Panorama9</span><span class="sxs-lookup"><span data-stu-id="bf015-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bf015-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Panorama9 tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bf015-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Panorama9 is tooa user in Azure AD.</span></span> <span data-ttu-id="bf015-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Panorama9 közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bf015-140">In other words, a link relationship between an Azure AD user and hello related user in Panorama9 needs toobe established.</span></span>

<span data-ttu-id="bf015-141">Panorama9, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bf015-141">In Panorama9, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bf015-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Panorama9-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bf015-142">tooconfigure and test Azure AD single sign-on with Panorama9, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bf015-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bf015-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bf015-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bf015-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bf015-145">**[Panorama9 tesztfelhasználó létrehozása](#creating-a-panorama9-test-user)**  -toohave egy megfelelője a Britta Simon a Panorama9, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bf015-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - toohave a counterpart of Britta Simon in Panorama9 that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bf015-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bf015-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bf015-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bf015-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bf015-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bf015-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bf015-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Panorama9 alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bf015-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="bf015-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Panorama9, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bf015-150">**tooconfigure Azure AD single sign-on with Panorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf015-151">Az Azure portál, a hello hello **Panorama9** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bf015-151">In hello Azure portal, on hello **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bf015-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bf015-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="bf015-155">A hello **Panorama9 tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bf015-155">On hello **Panorama9 Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="bf015-157">a.</span><span class="sxs-lookup"><span data-stu-id="bf015-157">a.</span></span> <span data-ttu-id="bf015-158">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="bf015-158">In hello **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="bf015-159">b.</span><span class="sxs-lookup"><span data-stu-id="bf015-159">b.</span></span> <span data-ttu-id="bf015-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="bf015-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bf015-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bf015-161">These values are not real.</span></span> <span data-ttu-id="bf015-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="bf015-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="bf015-163">Ügyfél [Panorama9 ügyfél-támogatási csoport](https://support.panorama9.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bf015-163">Contact [Panorama9 Client support team](https://support.panorama9.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="bf015-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="bf015-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="bf015-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf015-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bf015-168">A hello **Panorama9 konfigurációs** kattintson **konfigurálása Panorama9** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bf015-168">On hello **Panorama9 Configuration** section, click **Configure Panorama9** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bf015-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bf015-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="bf015-171">Egy másik webes böngészőablakban jelentkezzen be a Panorama9 vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bf015-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="bf015-172">Hello hello felső eszköztárán kattintson **kezelése**, és kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="bf015-172">In hello toolbar on hello top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="bf015-173">![Bővítmények](./media/active-directory-saas-panorama9-tutorial/ic790023.png "bővítmények")</span><span class="sxs-lookup"><span data-stu-id="bf015-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="bf015-174">A hello **bővítmények** párbeszédpanel, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bf015-174">On hello **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="bf015-175">![Egyszeri bejelentkezés](./media/active-directory-saas-panorama9-tutorial/ic790024.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="bf015-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="bf015-176">A hello **beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bf015-176">In hello **Settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="bf015-177">![Beállítások](./media/active-directory-saas-panorama9-tutorial/ic790025.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="bf015-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="bf015-178">a.</span><span class="sxs-lookup"><span data-stu-id="bf015-178">a.</span></span> <span data-ttu-id="bf015-179">A **identitási szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="bf015-179">In **Identity provider URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="bf015-180">b.</span><span class="sxs-lookup"><span data-stu-id="bf015-180">b.</span></span> <span data-ttu-id="bf015-181">A **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** tanúsítványt, amely az Azure-portálon másolta értékét.</span><span class="sxs-lookup"><span data-stu-id="bf015-181">In **Certificate fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="bf015-182">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bf015-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bf015-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bf015-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bf015-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bf015-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bf015-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bf015-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bf015-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf015-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="bf015-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bf015-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bf015-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bf015-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf015-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bf015-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bf015-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bf015-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bf015-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf015-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bf015-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bf015-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bf015-198">a.</span><span class="sxs-lookup"><span data-stu-id="bf015-198">a.</span></span> <span data-ttu-id="bf015-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bf015-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bf015-200">b.</span><span class="sxs-lookup"><span data-stu-id="bf015-200">b.</span></span> <span data-ttu-id="bf015-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bf015-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bf015-202">c.</span><span class="sxs-lookup"><span data-stu-id="bf015-202">c.</span></span> <span data-ttu-id="bf015-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bf015-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bf015-204">d.</span><span class="sxs-lookup"><span data-stu-id="bf015-204">d.</span></span> <span data-ttu-id="bf015-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bf015-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="bf015-206">Panorama9 tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf015-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="bf015-207">A sorrend tooenable az Azure AD felhasználók toolog Panorama9 be azok ki kell építenie Panorama9 be.</span><span class="sxs-lookup"><span data-stu-id="bf015-207">In order tooenable Azure AD users toolog into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="bf015-208">Panorama9 hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="bf015-208">In hello case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="bf015-209">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bf015-209">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf015-210">Jelentkezzen be tooyour **Panorama9** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bf015-210">Log in tooyour **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="bf015-211">Hello hello felső menüben kattintson a **kezelése**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="bf015-211">In hello menu on hello top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="bf015-212">![Felhasználók](./media/active-directory-saas-panorama9-tutorial/ic790027.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="bf015-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="bf015-213">A felhasználók szakaszban hello, kattintson  **+**  tooadd új felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bf015-213">In hello Users section, Click **+** tooadd new user.</span></span>

 <span data-ttu-id="bf015-214">![Felhasználók](./media/active-directory-saas-panorama9-tutorial/ic790028.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="bf015-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="bf015-215">Felhasználói adatok szakasz toohello lépjen típus hello e-mail címe egy érvényes Azure Active Directory felhasználó tooprovision hello történő **E-mail** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="bf015-215">Go toohello User data section, type hello email address of a valid Azure Active Directory user you want tooprovision into hello **Email** textbox.</span></span>

5. <span data-ttu-id="bf015-216">Felhasználók szakaszban kattintson toohello származnak **mentése**.</span><span class="sxs-lookup"><span data-stu-id="bf015-216">Come toohello Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="bf015-217">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="bf015-217">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bf015-218">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bf015-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bf015-219">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPanorama9 megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bf015-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPanorama9.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bf015-221">**tooassign Britta Simon tooPanorama9, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bf015-221">**tooassign Britta Simon tooPanorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf015-222">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bf015-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bf015-224">Hello alkalmazások listában válassza ki a **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="bf015-224">In hello applications list, select **Panorama9**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="bf015-226">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bf015-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bf015-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf015-228">Click **Add** button.</span></span> <span data-ttu-id="bf015-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf015-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bf015-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bf015-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bf015-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf015-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bf015-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bf015-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bf015-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bf015-234">Testing single sign-on</span></span>

<span data-ttu-id="bf015-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bf015-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bf015-236">Ha a hozzáférési Panel hello hello Panorama9 csempe gombra kattint, automatikusan bejelentkezett tooPanorama9 alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="bf015-236">When you click hello Panorama9 tile in hello Access Panel, you should get automatically signed-on tooPanorama9 application.</span></span>
<span data-ttu-id="bf015-237">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bf015-237">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf015-238">További források</span><span class="sxs-lookup"><span data-stu-id="bf015-238">Additional resources</span></span>

* [<span data-ttu-id="bf015-239">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bf015-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bf015-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bf015-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

