---
title: "Oktatóanyag: Azure Active Directoryval integrált Picturepark |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Picturepark között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 3d826d3f73aad2f0d123f8697c6caafad7bc926a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="f98ca-103">Oktatóanyag: Azure Active Directoryval integrált Picturepark</span><span class="sxs-lookup"><span data-stu-id="f98ca-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="f98ca-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Picturepark az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f98ca-104">In this tutorial, you learn how toointegrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f98ca-105">Picturepark integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f98ca-105">Integrating Picturepark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f98ca-106">Megadhatja a hozzáférés tooPicturepark rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f98ca-106">You can control in Azure AD who has access tooPicturepark</span></span>
- <span data-ttu-id="f98ca-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPicturepark (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f98ca-107">You can enable your users tooautomatically get signed-on tooPicturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f98ca-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f98ca-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f98ca-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f98ca-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f98ca-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f98ca-110">Prerequisites</span></span>

<span data-ttu-id="f98ca-111">az Azure AD integrálása Picturepark tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f98ca-111">tooconfigure Azure AD integration with Picturepark, you need hello following items:</span></span>

- <span data-ttu-id="f98ca-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f98ca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f98ca-113">Egy Picturepark egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f98ca-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f98ca-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f98ca-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f98ca-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f98ca-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f98ca-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f98ca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f98ca-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f98ca-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f98ca-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f98ca-118">Scenario description</span></span>
<span data-ttu-id="f98ca-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f98ca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f98ca-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f98ca-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f98ca-121">Hello gyűjteményből Picturepark hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f98ca-121">Adding Picturepark from hello gallery</span></span>
2. <span data-ttu-id="f98ca-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f98ca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-hello-gallery"></a><span data-ttu-id="f98ca-123">Hello gyűjteményből Picturepark hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f98ca-123">Adding Picturepark from hello gallery</span></span>
<span data-ttu-id="f98ca-124">tooconfigure hello integrációja Picturepark az Azure AD-be, meg kell tooadd Picturepark hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f98ca-124">tooconfigure hello integration of Picturepark into Azure AD, you need tooadd Picturepark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f98ca-125">**tooadd Picturepark hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-125">**tooadd Picturepark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f98ca-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f98ca-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f98ca-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f98ca-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f98ca-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f98ca-133">Hello keresési mezőbe, írja be a **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-133">In hello search box, type **Picturepark**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="f98ca-135">A hello eredmények panelen válassza ki a **Picturepark**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-135">In hello results panel, select **Picturepark**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f98ca-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f98ca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f98ca-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Picturepark.</span><span class="sxs-lookup"><span data-stu-id="f98ca-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f98ca-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Picturepark tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f98ca-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Picturepark is tooa user in Azure AD.</span></span> <span data-ttu-id="f98ca-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Picturepark közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f98ca-140">In other words, a link relationship between an Azure AD user and hello related user in Picturepark needs toobe established.</span></span>

<span data-ttu-id="f98ca-141">Picturepark, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f98ca-141">In Picturepark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f98ca-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Picturepark-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f98ca-142">tooconfigure and test Azure AD single sign-on with Picturepark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f98ca-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f98ca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f98ca-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f98ca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f98ca-145">**[Picturepark tesztfelhasználó létrehozása](#creating-a-picturepark-test-user)**  -toohave egy megfelelője a Britta Simon a Picturepark, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f98ca-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - toohave a counterpart of Britta Simon in Picturepark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f98ca-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f98ca-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f98ca-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f98ca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f98ca-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f98ca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f98ca-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Picturepark alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f98ca-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="f98ca-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Picturepark, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-150">**tooconfigure Azure AD single sign-on with Picturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="f98ca-151">Az Azure portál, a hello hello **Picturepark** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-151">In hello Azure portal, on hello **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f98ca-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f98ca-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="f98ca-155">A hello **Picturepark tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f98ca-155">On hello **Picturepark Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="f98ca-157">a.</span><span class="sxs-lookup"><span data-stu-id="f98ca-157">a.</span></span> <span data-ttu-id="f98ca-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="f98ca-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="f98ca-159">b.</span><span class="sxs-lookup"><span data-stu-id="f98ca-159">b.</span></span> <span data-ttu-id="f98ca-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="f98ca-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="f98ca-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f98ca-161">These values are not real.</span></span> <span data-ttu-id="f98ca-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="f98ca-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f98ca-163">Ügyfél [Picturepark ügyfél-támogatási csoport](https://picturepark.com/about/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f98ca-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f98ca-164">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="f98ca-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="f98ca-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f98ca-168">A hello **Picturepark konfigurációs** kattintson **konfigurálása Picturepark** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f98ca-168">On hello **Picturepark Configuration** section, click **Configure Picturepark** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f98ca-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f98ca-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="f98ca-171">Egy másik webes böngészőablakban jelentkezzen be a Picturepark vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="f98ca-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="f98ca-172">Hello hello felső eszköztárán kattintson **felügyeleti eszközök**, és kattintson a **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-172">In hello toolbar on hello top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="f98ca-173">![Felügyeleti konzol](./media/active-directory-saas-picturepark-tutorial/ic795062.png "felügyeleti konzol")</span><span class="sxs-lookup"><span data-stu-id="f98ca-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="f98ca-174">Kattintson a **hitelesítési**, és kattintson a **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="f98ca-175">![Hitelesítési](./media/active-directory-saas-picturepark-tutorial/ic795063.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="f98ca-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="f98ca-176">A hello **identitás szolgáltató konfigurálása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f98ca-176">In hello **Identity provider configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="f98ca-177">![Szolgáltató konfigurálása identitás](./media/active-directory-saas-picturepark-tutorial/ic795064.png "identitás szolgáltató konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="f98ca-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="f98ca-178">a.</span><span class="sxs-lookup"><span data-stu-id="f98ca-178">a.</span></span> <span data-ttu-id="f98ca-179">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-179">Click **Add**.</span></span>
  
    <span data-ttu-id="f98ca-180">b.</span><span class="sxs-lookup"><span data-stu-id="f98ca-180">b.</span></span> <span data-ttu-id="f98ca-181">Írja be a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="f98ca-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="f98ca-182">c.</span><span class="sxs-lookup"><span data-stu-id="f98ca-182">c.</span></span> <span data-ttu-id="f98ca-183">Válassza ki **alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="f98ca-184">d.</span><span class="sxs-lookup"><span data-stu-id="f98ca-184">d.</span></span> <span data-ttu-id="f98ca-185">A **Issuer URI** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="f98ca-185">In **Issuer URI** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="f98ca-186">e.</span><span class="sxs-lookup"><span data-stu-id="f98ca-186">e.</span></span> <span data-ttu-id="f98ca-187">A **megbízható kibocsátó görgetőgomb nyomtatás** szövegmezőhöz Beillesztés hello értékének **ujjlenyomat** , amely a másolt **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f98ca-187">In **Trusted Issuer Thumb Print** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="f98ca-188">Kattintson a **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="f98ca-189">tooset hello **Emailaddress** hello attribútum **jogcím** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-189">tooset hello **Emailaddress** attribute in hello **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="f98ca-190">![Konfigurációs](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="f98ca-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="f98ca-191">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f98ca-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f98ca-192">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f98ca-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f98ca-193">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f98ca-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f98ca-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f98ca-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="f98ca-195">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f98ca-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f98ca-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f98ca-198">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f98ca-200">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f98ca-202">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f98ca-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f98ca-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f98ca-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f98ca-206">a.</span><span class="sxs-lookup"><span data-stu-id="f98ca-206">a.</span></span> <span data-ttu-id="f98ca-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f98ca-208">b.</span><span class="sxs-lookup"><span data-stu-id="f98ca-208">b.</span></span> <span data-ttu-id="f98ca-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f98ca-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f98ca-210">c.</span><span class="sxs-lookup"><span data-stu-id="f98ca-210">c.</span></span> <span data-ttu-id="f98ca-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f98ca-212">d.</span><span class="sxs-lookup"><span data-stu-id="f98ca-212">d.</span></span> <span data-ttu-id="f98ca-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="f98ca-214">Picturepark tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f98ca-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="f98ca-215">A sorrend tooenable az Azure AD felhasználók toolog Picturepark be azok ki kell építenie Picturepark be.</span><span class="sxs-lookup"><span data-stu-id="f98ca-215">In order tooenable Azure AD users toolog into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="f98ca-216">Picturepark hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f98ca-216">In hello case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="f98ca-217">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-217">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="f98ca-218">Jelentkezzen be tooyour **Picturepark** bérlő.</span><span class="sxs-lookup"><span data-stu-id="f98ca-218">Log in tooyour **Picturepark** tenant.</span></span>

2. <span data-ttu-id="f98ca-219">Hello hello felső eszköztárán kattintson **felügyeleti eszközök**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-219">In hello toolbar on hello top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="f98ca-220">![Felhasználók](./media/active-directory-saas-picturepark-tutorial/ic795067.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="f98ca-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="f98ca-221">A hello **felhasználók áttekintése** lapra, majd **új**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-221">In hello **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="f98ca-222">![Felhasználókezelés](./media/active-directory-saas-picturepark-tutorial/ic795068.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="f98ca-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="f98ca-223">A hello **felhasználó létrehozása** párbeszédpanelen hajtsa végre a hello egy érvényes Azure Active Directory felhasználói lépéseit követve tooprovision szeretné:</span><span class="sxs-lookup"><span data-stu-id="f98ca-223">On hello **Create User** dialog, perform hello following steps of a valid Azure Active Directory User you want tooprovision:</span></span>
   
    <span data-ttu-id="f98ca-224">![Hozzon létre felhasználói](./media/active-directory-saas-picturepark-tutorial/ic795069.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="f98ca-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="f98ca-225">a.</span><span class="sxs-lookup"><span data-stu-id="f98ca-225">a.</span></span> <span data-ttu-id="f98ca-226">A hello **E-mail cím** szövegmezőhöz típus hello **e-mail cím** hello felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="f98ca-226">In hello **Email Address** textbox, type hello **email address** of hello user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="f98ca-227">b.</span><span class="sxs-lookup"><span data-stu-id="f98ca-227">b.</span></span> <span data-ttu-id="f98ca-228">A hello **jelszó** és **jelszó megerősítése** szövegmezőből, típus hello **jelszó** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f98ca-228">In hello **Password** and **Confirm Password** textboxes, type hello **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="f98ca-229">c.</span><span class="sxs-lookup"><span data-stu-id="f98ca-229">c.</span></span> <span data-ttu-id="f98ca-230">A hello **Utónév** szövegmezőhöz típus hello **Utónév** hello felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-230">In hello **First Name** textbox, type hello **First Name** of hello user **Britta**.</span></span> 
   
    <span data-ttu-id="f98ca-231">d.</span><span class="sxs-lookup"><span data-stu-id="f98ca-231">d.</span></span> <span data-ttu-id="f98ca-232">A hello **Vezetéknév** szövegmezőhöz típus hello **Vezetéknév** hello felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-232">In hello **Last Name** textbox, type hello **Last Name** of hello user **Simon**.</span></span>
   
    <span data-ttu-id="f98ca-233">e.</span><span class="sxs-lookup"><span data-stu-id="f98ca-233">e.</span></span> <span data-ttu-id="f98ca-234">A hello **vállalati** szövegmezőhöz típus hello **vállalatnév** hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f98ca-234">In hello **Company** textbox, type hello **Company name** of hello user.</span></span> 
   
    <span data-ttu-id="f98ca-235">f.</span><span class="sxs-lookup"><span data-stu-id="f98ca-235">f.</span></span> <span data-ttu-id="f98ca-236">A hello **ország** szövegmező, jelölje be hello **ország** hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f98ca-236">In hello **Country** textbox, select hello **Country** of hello user.</span></span>
  
    <span data-ttu-id="f98ca-237">g.</span><span class="sxs-lookup"><span data-stu-id="f98ca-237">g.</span></span> <span data-ttu-id="f98ca-238">A hello **ZIP-** szövegmezőhöz típus hello **irányítószám** hello város.</span><span class="sxs-lookup"><span data-stu-id="f98ca-238">In hello **ZIP** textbox, type hello **ZIP code** of hello city.</span></span>
   
    <span data-ttu-id="f98ca-239">h.</span><span class="sxs-lookup"><span data-stu-id="f98ca-239">h.</span></span> <span data-ttu-id="f98ca-240">A hello **Város** szövegmezőhöz típus hello **városnév** hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f98ca-240">In hello **City** textbox, type hello **City name** of hello user.</span></span>

    <span data-ttu-id="f98ca-241">i.</span><span class="sxs-lookup"><span data-stu-id="f98ca-241">i.</span></span> <span data-ttu-id="f98ca-242">Válassza ki a **nyelvi**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="f98ca-243">j.</span><span class="sxs-lookup"><span data-stu-id="f98ca-243">j.</span></span> <span data-ttu-id="f98ca-244">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="f98ca-245">Bármely más Picturepark felhasználói fiók létrehozása eszközök vagy Picturepark tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="f98ca-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f98ca-246">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f98ca-246">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f98ca-247">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPicturepark megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f98ca-247">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPicturepark.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f98ca-249">**tooassign Britta Simon tooPicturepark, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f98ca-249">**tooassign Britta Simon tooPicturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="f98ca-250">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-250">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f98ca-252">Hello alkalmazások listában válassza ki a **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-252">In hello applications list, select **Picturepark**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="f98ca-254">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f98ca-254">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f98ca-256">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f98ca-256">Click **Add** button.</span></span> <span data-ttu-id="f98ca-257">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f98ca-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f98ca-259">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f98ca-259">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f98ca-260">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f98ca-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f98ca-261">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f98ca-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f98ca-262">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f98ca-262">Testing single sign-on</span></span>

<span data-ttu-id="f98ca-263">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f98ca-263">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f98ca-264">Ha a hozzáférési Panel hello hello Picturepark csempe gombra kattint, automatikusan bejelentkezett tooyour Picturepark alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f98ca-264">When you click hello Picturepark tile in hello Access Panel, you should get automatically signed-on tooyour Picturepark application.</span></span> <span data-ttu-id="f98ca-265">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f98ca-265">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f98ca-266">További források</span><span class="sxs-lookup"><span data-stu-id="f98ca-266">Additional resources</span></span>

* [<span data-ttu-id="f98ca-267">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f98ca-267">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f98ca-268">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f98ca-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

