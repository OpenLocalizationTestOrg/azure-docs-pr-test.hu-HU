---
title: "Oktatóanyag: Azure Active Directoryval integrált Samanage |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Samanage között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8edc29f113b8088438618a731e97c0f4f155b9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a><span data-ttu-id="18f49-103">Oktatóanyag: Azure Active Directoryval integrált Samanage</span><span class="sxs-lookup"><span data-stu-id="18f49-103">Tutorial: Azure Active Directory integration with Samanage</span></span>

<span data-ttu-id="18f49-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Samanage az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="18f49-104">In this tutorial, you learn how toointegrate Samanage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="18f49-105">Samanage integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="18f49-105">Integrating Samanage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="18f49-106">Megadhatja a hozzáférés tooSamanage rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="18f49-106">You can control in Azure AD who has access tooSamanage</span></span>
- <span data-ttu-id="18f49-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSamanage (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="18f49-107">You can enable your users tooautomatically get signed-on tooSamanage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="18f49-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="18f49-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="18f49-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="18f49-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18f49-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="18f49-110">Prerequisites</span></span>

<span data-ttu-id="18f49-111">az Azure AD integrálása Samanage tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="18f49-111">tooconfigure Azure AD integration with Samanage, you need hello following items:</span></span>

- <span data-ttu-id="18f49-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="18f49-112">An Azure AD subscription</span></span>
- <span data-ttu-id="18f49-113">Egy Samanage egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="18f49-113">A Samanage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="18f49-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="18f49-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="18f49-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="18f49-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="18f49-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="18f49-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="18f49-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="18f49-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="18f49-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="18f49-118">Scenario description</span></span>
<span data-ttu-id="18f49-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="18f49-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="18f49-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="18f49-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="18f49-121">Hello gyűjteményből Samanage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18f49-121">Adding Samanage from hello gallery</span></span>
2. <span data-ttu-id="18f49-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="18f49-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-samanage-from-hello-gallery"></a><span data-ttu-id="18f49-123">Hello gyűjteményből Samanage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18f49-123">Adding Samanage from hello gallery</span></span>
<span data-ttu-id="18f49-124">tooconfigure hello integrációja Samanage az Azure AD-be, meg kell tooadd Samanage hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="18f49-124">tooconfigure hello integration of Samanage into Azure AD, you need tooadd Samanage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="18f49-125">**tooadd Samanage hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="18f49-125">**tooadd Samanage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="18f49-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="18f49-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="18f49-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="18f49-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="18f49-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="18f49-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="18f49-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="18f49-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="18f49-133">Hello keresési mezőbe, írja be a **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="18f49-133">In hello search box, type **Samanage**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. <span data-ttu-id="18f49-135">A hello eredmények panelen válassza ki a **Samanage**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="18f49-135">In hello results panel, select **Samanage**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="18f49-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="18f49-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="18f49-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Samanage.</span><span class="sxs-lookup"><span data-stu-id="18f49-138">In this section, you configure and test Azure AD single sign-on with Samanage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="18f49-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Samanage tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="18f49-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Samanage is tooa user in Azure AD.</span></span> <span data-ttu-id="18f49-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Samanage közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="18f49-140">In other words, a link relationship between an Azure AD user and hello related user in Samanage needs toobe established.</span></span>

<span data-ttu-id="18f49-141">Samanage, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="18f49-141">In Samanage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="18f49-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Samanage-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="18f49-142">tooconfigure and test Azure AD single sign-on with Samanage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="18f49-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="18f49-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="18f49-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18f49-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="18f49-145">**[Samanage tesztfelhasználó létrehozása](#creating-a-samanage-test-user)**  -toohave egy megfelelője a Britta Simon a Samanage, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="18f49-145">**[Creating a Samanage test user](#creating-a-samanage-test-user)** - toohave a counterpart of Britta Simon in Samanage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="18f49-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="18f49-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="18f49-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="18f49-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="18f49-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="18f49-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="18f49-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Samanage alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="18f49-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Samanage application.</span></span>

<span data-ttu-id="18f49-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Samanage, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="18f49-150">**tooconfigure Azure AD single sign-on with Samanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="18f49-151">Az Azure portál, a hello hello **Samanage** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="18f49-151">In hello Azure portal, on hello **Samanage** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="18f49-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="18f49-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. <span data-ttu-id="18f49-155">A hello **Samanage tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18f49-155">On hello **Samanage Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    <span data-ttu-id="18f49-157">a.</span><span class="sxs-lookup"><span data-stu-id="18f49-157">a.</span></span> <span data-ttu-id="18f49-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Company Name>.samanage.com/saml_login/<Company Name>`</span><span class="sxs-lookup"><span data-stu-id="18f49-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com/saml_login/<Company Name>`</span></span>

    <span data-ttu-id="18f49-159">b.</span><span class="sxs-lookup"><span data-stu-id="18f49-159">b.</span></span> <span data-ttu-id="18f49-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Company Name>.samanage.com`</span><span class="sxs-lookup"><span data-stu-id="18f49-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="18f49-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="18f49-161">These values are not real.</span></span> <span data-ttu-id="18f49-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója, amelynek az ismertetése hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="18f49-162">Update these values with hello actual Sign-on URL and Identifier, which is explained later in hello tutorial.</span></span> <span data-ttu-id="18f49-163">További részletekért forduljon [Samanage ügyfél-támogatási csoport](https://www.samanage.com/support).</span><span class="sxs-lookup"><span data-stu-id="18f49-163">For more details contact [Samanage Client support team](https://www.samanage.com/support).</span></span>    
 
4. <span data-ttu-id="18f49-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="18f49-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. <span data-ttu-id="18f49-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="18f49-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="18f49-168">A hello **Samanage konfigurációs** kattintson **konfigurálása Samanage** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="18f49-168">On hello **Samanage Configuration** section, click **Configure Samanage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="18f49-169">Másolás hello **Sign-Out URL-címet, és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="18f49-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. <span data-ttu-id="18f49-171">Egy másik webes böngészőablakban jelentkezzen be a Samanage vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="18f49-171">In a different web browser window, log into your Samanage company site as an administrator.</span></span>

8. <span data-ttu-id="18f49-172">Kattintson a **irányítópult** válassza **telepítő** bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="18f49-172">Click **Dashboard** and select **Setup** in left navigation pane.</span></span>
   
    <span data-ttu-id="18f49-173">![Irányítópult](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "irányítópult")</span><span class="sxs-lookup"><span data-stu-id="18f49-173">![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")</span></span>

9. <span data-ttu-id="18f49-174">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="18f49-174">Click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="18f49-175">![Egyszeri bejelentkezés](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="18f49-175">![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")</span></span>

10. <span data-ttu-id="18f49-176">Keresse meg a túl**SAML használatával bejelentkezési** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18f49-176">Navigate too**Login using SAML** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="18f49-177">![Bejelentkezés SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "bejelentkezési SAML használatával")</span><span class="sxs-lookup"><span data-stu-id="18f49-177">![Login using SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Login using SAML")</span></span>
 
    <span data-ttu-id="18f49-178">a.</span><span class="sxs-lookup"><span data-stu-id="18f49-178">a.</span></span> <span data-ttu-id="18f49-179">Kattintson a **engedélyezése egyszeri bejelentkezéshez a SAML**.</span><span class="sxs-lookup"><span data-stu-id="18f49-179">Click **Enable Single Sign-On with SAML**.</span></span>  
 
    <span data-ttu-id="18f49-180">b.</span><span class="sxs-lookup"><span data-stu-id="18f49-180">b.</span></span> <span data-ttu-id="18f49-181">A hello **identitási szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="18f49-181">In hello **Identity Provider URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>    
 
    <span data-ttu-id="18f49-182">c.</span><span class="sxs-lookup"><span data-stu-id="18f49-182">c.</span></span> <span data-ttu-id="18f49-183">Hello megerősítése **bejelentkezési URL-cím** egyező hello **URL-cím bejelentkezési** a **Samanage tartomány és az URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="18f49-183">Confirm hello **Login URL** matches hello **Sign On URL** of **Samanage Domain and URLs** section in Azure portal.</span></span>
 
    <span data-ttu-id="18f49-184">d.</span><span class="sxs-lookup"><span data-stu-id="18f49-184">d.</span></span> <span data-ttu-id="18f49-185">A hello **kijelentkezési URL-cím** szövegmezőhöz hello értékének megadása **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="18f49-185">In hello **Logout URL** textbox, enter hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="18f49-186">e.</span><span class="sxs-lookup"><span data-stu-id="18f49-186">e.</span></span> <span data-ttu-id="18f49-187">A hello **SAML kibocsátó** szövegmezőhöz típus hello app id URI be az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="18f49-187">In hello **SAML Issuer** textbox, type hello app id URI set in your identity provider.</span></span>
 
    <span data-ttu-id="18f49-188">f.</span><span class="sxs-lookup"><span data-stu-id="18f49-188">f.</span></span> <span data-ttu-id="18f49-189">A base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött megnyitása és toohello Beillesztés **illessze be az identitásszolgáltató x.509 tanúsítvány az alábbi** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="18f49-189">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **Paste your Identity Provider x.509 Certificate below** textbox.</span></span>
 
    <span data-ttu-id="18f49-190">g.</span><span class="sxs-lookup"><span data-stu-id="18f49-190">g.</span></span> <span data-ttu-id="18f49-191">Kattintson a **felhasználók létrehozása, ha azok nem léteznek Samanage**.</span><span class="sxs-lookup"><span data-stu-id="18f49-191">Click **Create users if they do not exist in Samanage**.</span></span>
 
    <span data-ttu-id="18f49-192">h.</span><span class="sxs-lookup"><span data-stu-id="18f49-192">h.</span></span> <span data-ttu-id="18f49-193">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="18f49-193">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="18f49-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="18f49-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="18f49-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="18f49-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="18f49-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="18f49-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="18f49-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="18f49-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="18f49-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="18f49-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="18f49-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="18f49-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="18f49-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="18f49-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="18f49-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="18f49-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="18f49-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18f49-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="18f49-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="18f49-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="18f49-209">a.</span><span class="sxs-lookup"><span data-stu-id="18f49-209">a.</span></span> <span data-ttu-id="18f49-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="18f49-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="18f49-211">b.</span><span class="sxs-lookup"><span data-stu-id="18f49-211">b.</span></span> <span data-ttu-id="18f49-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="18f49-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="18f49-213">c.</span><span class="sxs-lookup"><span data-stu-id="18f49-213">c.</span></span> <span data-ttu-id="18f49-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="18f49-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="18f49-215">d.</span><span class="sxs-lookup"><span data-stu-id="18f49-215">d.</span></span> <span data-ttu-id="18f49-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="18f49-216">Click **Create**.</span></span>
 
### <a name="creating-a-samanage-test-user"></a><span data-ttu-id="18f49-217">Samanage tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="18f49-217">Creating a Samanage test user</span></span>

<span data-ttu-id="18f49-218">az Azure AD tooenable felhasználók toolog a tooSamanage, akkor ki kell építenie Samanage be.</span><span class="sxs-lookup"><span data-stu-id="18f49-218">tooenable Azure AD users toolog in tooSamanage, they must be provisioned into Samanage.</span></span>  
<span data-ttu-id="18f49-219">Samanage hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="18f49-219">In hello case of Samanage, provisioning is a manual task.</span></span>

<span data-ttu-id="18f49-220">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="18f49-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="18f49-221">Jelentkezzen be a Samanage vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="18f49-221">Log into your Samanage company site as an administrator.</span></span>

2. <span data-ttu-id="18f49-222">Kattintson a **irányítópult** válassza **telepítő** a bal oldali navigációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="18f49-222">Click **Dashboard** and select **Setup** in left navigation pan.</span></span>
   
    <span data-ttu-id="18f49-223">![A telepítő](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="18f49-223">![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")</span></span>

3. <span data-ttu-id="18f49-224">Kattintson a hello **felhasználók** lap</span><span class="sxs-lookup"><span data-stu-id="18f49-224">Click hello **Users** tab</span></span>
   
    <span data-ttu-id="18f49-225">![Felhasználók](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="18f49-225">![Users](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Users")</span></span>

4. <span data-ttu-id="18f49-226">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="18f49-226">Click **New User**.</span></span>
   
    <span data-ttu-id="18f49-227">![Új felhasználó](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="18f49-227">![New User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "New User")</span></span>

5. <span data-ttu-id="18f49-228">Típus hello **neve** és hello **E-mail cím** tooprovision ki, majd kattintson egy Azure Active Directory-fiók **a felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="18f49-228">Type hello **Name** and hello **Email Address** of an Azure Active Directory account you want tooprovision and click **Create user**.</span></span>
   
    <span data-ttu-id="18f49-229">![Hozzon létre felhasználói](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="18f49-229">![Create User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Create User")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="18f49-230">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="18f49-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="18f49-231">Bármely más Samanage felhasználói fiók létrehozása eszközök vagy Samanage tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="18f49-231">You can use any other Samanage user account creation tools or APIs provided by Samanage tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="18f49-232">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="18f49-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="18f49-233">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSamanage megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="18f49-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSamanage.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="18f49-235">**tooassign Britta Simon tooSamanage, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="18f49-235">**tooassign Britta Simon tooSamanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="18f49-236">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="18f49-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="18f49-238">Hello alkalmazások listában válassza ki a **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="18f49-238">In hello applications list, select **Samanage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. <span data-ttu-id="18f49-240">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="18f49-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="18f49-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="18f49-242">Click **Add** button.</span></span> <span data-ttu-id="18f49-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18f49-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="18f49-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="18f49-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="18f49-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18f49-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="18f49-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="18f49-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="18f49-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="18f49-248">Testing single sign-on</span></span>

<span data-ttu-id="18f49-249">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="18f49-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="18f49-250">Ha a hozzáférési Panel hello hello Samanage csempe gombra kattint, automatikusan bejelentkezett tooyour Samanage alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="18f49-250">When you click hello Samanage tile in hello Access Panel, you should get automatically signed-on tooyour Samanage application.</span></span>
<span data-ttu-id="18f49-251">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="18f49-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18f49-252">További források</span><span class="sxs-lookup"><span data-stu-id="18f49-252">Additional resources</span></span>

* [<span data-ttu-id="18f49-253">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="18f49-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="18f49-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="18f49-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

