---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3f33a43ca8fbd60486d7a400ae8246af768376ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a><span data-ttu-id="ea564-103">Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox</span><span class="sxs-lookup"><span data-stu-id="ea564-103">Tutorial: Azure Active Directory integration with Dropbox for Business</span></span>

<span data-ttu-id="ea564-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Dropbox vállalati az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ea564-104">In this tutorial, you learn how toointegrate Dropbox for Business with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ea564-105">A vállalati Dropbox integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ea564-105">Integrating Dropbox for Business with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ea564-106">Szabályozhatja a vállalati hozzáférés tooDropbox rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ea564-106">You can control in Azure AD who has access tooDropbox for Business</span></span>
- <span data-ttu-id="ea564-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDropbox vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ea564-107">You can enable your users tooautomatically get signed-on tooDropbox for Business (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ea564-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ea564-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ea564-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ea564-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea564-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea564-110">Prerequisites</span></span>

<span data-ttu-id="ea564-111">tooconfigure vállalati dropbox-bA az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="ea564-111">tooconfigure Azure AD integration with Dropbox for Business, you need hello following items:</span></span>

- <span data-ttu-id="ea564-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ea564-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ea564-113">A Dropbox üzleti egyszeri bejelentkezést az előfizetés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="ea564-113">A Dropbox for Business single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ea564-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ea564-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ea564-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ea564-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ea564-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ea564-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ea564-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ea564-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ea564-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ea564-118">Scenario description</span></span>
<span data-ttu-id="ea564-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ea564-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ea564-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ea564-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ea564-121">A vállalati Dropbox hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ea564-121">Adding Dropbox for Business from hello gallery</span></span>
2. <span data-ttu-id="ea564-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ea564-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dropbox-for-business-from-hello-gallery"></a><span data-ttu-id="ea564-123">A vállalati Dropbox hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ea564-123">Adding Dropbox for Business from hello gallery</span></span>
<span data-ttu-id="ea564-124">tooconfigure hello integrálása az Azure AD-be a vállalati Dropbox, szükség van tooadd Dropbox üzleti hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ea564-124">tooconfigure hello integration of Dropbox for Business into Azure AD, you need tooadd Dropbox for Business from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ea564-125">**tooadd Dropbox vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ea564-125">**tooadd Dropbox for Business from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ea564-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ea564-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ea564-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ea564-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ea564-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ea564-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ea564-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ea564-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ea564-133">Hello keresési mezőbe, írja be a **Dropbox vállalati**.</span><span class="sxs-lookup"><span data-stu-id="ea564-133">In hello search box, type **Dropbox for Business**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. <span data-ttu-id="ea564-135">Hello eredmények panelen, jelölje ki a **vállalati Dropbox**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ea564-135">In hello results panel, select **Dropbox for Business**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ea564-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ea564-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ea564-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Dropbox vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="ea564-138">In this section, you configure and test Azure AD single sign-on with Dropbox for Business based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ea564-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a vállalati Dropbox tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ea564-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Dropbox for Business is tooa user in Azure AD.</span></span> <span data-ttu-id="ea564-140">Ez azt jelenti hello kapcsolódó felhasználó a vállalati Dropbox és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="ea564-140">In other words, a link relationship between an Azure AD user and hello related user in Dropbox for Business needs toobe established.</span></span>

<span data-ttu-id="ea564-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Dropbox vállalati.</span><span class="sxs-lookup"><span data-stu-id="ea564-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Dropbox for Business.</span></span>

<span data-ttu-id="ea564-142">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Dropbox vállalati, a következő építőelemeket toocomplete hello kell:</span><span class="sxs-lookup"><span data-stu-id="ea564-142">tooconfigure and test Azure AD single sign-on with Dropbox for Business, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ea564-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ea564-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ea564-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea564-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ea564-145">**[A Dropbox az üzleti tesztfelhasználó létrehozása](#creating-a-dropbox-for-business-test-user)**  -toohave egy megfelelője a Britta Simon a Dropbox vállalati, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ea564-145">**[Creating a Dropbox for Business test user](#creating-a-dropbox-for-business-test-user)** - toohave a counterpart of Britta Simon in Dropbox for Business that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ea564-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ea564-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ea564-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ea564-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ea564-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ea564-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ea564-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a dropboxban üzleti alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="ea564-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Dropbox for Business application.</span></span>

<span data-ttu-id="ea564-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Dropbox vállalati, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ea564-150">**tooconfigure Azure AD single sign-on with Dropbox for Business, perform hello following steps:**</span></span>

1. <span data-ttu-id="ea564-151">Az Azure portál, a hello hello **vállalati Dropbox** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ea564-151">In hello Azure portal, on hello **Dropbox for Business** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ea564-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ea564-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. <span data-ttu-id="ea564-155">A hello **Dropbox üzleti tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ea564-155">On hello **Dropbox for Business Domain and URLs** section, perform hello following steps:</span></span>

    <span data-ttu-id="ea564-156">a.</span><span class="sxs-lookup"><span data-stu-id="ea564-156">a.</span></span> <span data-ttu-id="ea564-157">Bejelentkezés a Dropbox tooyour üzleti bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="ea564-157">Sign on tooyour Dropbox for business tenant.</span></span> 
   
    <span data-ttu-id="ea564-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="ea564-158">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="ea564-159">b.</span><span class="sxs-lookup"><span data-stu-id="ea564-159">b.</span></span> <span data-ttu-id="ea564-160">A bal oldalon hello hello navigációs ablaktábláján kattintson **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="ea564-160">In hello navigation pane on hello left side, click **Admin Console**.</span></span> 
   
    <span data-ttu-id="ea564-161">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="ea564-161">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="ea564-162">c.</span><span class="sxs-lookup"><span data-stu-id="ea564-162">c.</span></span> <span data-ttu-id="ea564-163">A hello **felügyeleti konzol**, kattintson a **hitelesítési** hello bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="ea564-163">On hello **Admin Console**, click **Authentication** in hello left navigation pane.</span></span> 
   
    <span data-ttu-id="ea564-164">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="ea564-164">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="ea564-165">d.</span><span class="sxs-lookup"><span data-stu-id="ea564-165">d.</span></span> <span data-ttu-id="ea564-166">A hello **egyszeri bejelentkezés** szakaszban jelölje be **egyszeri bejelentkezés engedélyezése**, és kattintson a **további** tooexpand ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ea564-166">In hello **Single sign-on** section, select **Enable single sign-on**, and then click **More** tooexpand this section.</span></span>  
   
    <span data-ttu-id="ea564-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="ea564-167">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="ea564-168">e.</span><span class="sxs-lookup"><span data-stu-id="ea564-168">e.</span></span> <span data-ttu-id="ea564-169">Hello URL-Címének másolása mellett túl**is jelentkeznek be az e-mail cím beírásával vagy azok lépjen közvetlenül**.</span><span class="sxs-lookup"><span data-stu-id="ea564-169">Copy hello URL next too**Users can sign in by entering their email address or they can go directly to**.</span></span> 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    <span data-ttu-id="ea564-171">f.</span><span class="sxs-lookup"><span data-stu-id="ea564-171">f.</span></span> <span data-ttu-id="ea564-172">Az Azure portálon hello hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="ea564-172">On hello Azure portal, in hello **Sign-on URL** textbox, paste hello URL.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     <span data-ttu-id="ea564-174">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.dropbox.com/sso/<id>`</span><span class="sxs-lookup"><span data-stu-id="ea564-174">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.dropbox.com/sso/<id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ea564-175">Ez az érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="ea564-175">This value is not real value.</span></span> <span data-ttu-id="ea564-176">Frissítse a hello érték hello tényleges bejelentkezési URL-címet kap az egyszeri bejelentkezés szakasz.</span><span class="sxs-lookup"><span data-stu-id="ea564-176">Update hello value with hello actual Sign-on URL you get from their Single sign-on section.</span></span> <span data-ttu-id="ea564-177">Ügyfél [üzleti ügyfél-támogatási csoport Dropbox](https://www.dropbox.com/business/contact) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="ea564-177">Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact) tooget this value.</span></span> 
 
4. <span data-ttu-id="ea564-178">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ea564-178">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. <span data-ttu-id="ea564-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ea564-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ea564-182">A hello **Dropbox üzleti konfiguráció** kattintson **Dropbox konfigurálása a vállalati** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ea564-182">On hello **Dropbox for Business Configuration** section, click **Configure Dropbox for Business** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ea564-183">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ea564-183">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. <span data-ttu-id="ea564-185">tooconfigure egyszeri bejelentkezést a **vállalati Dropbox** oldalán, nyissa meg a Dropbox üzleti bérlő, a hello **egyszeri bejelentkezés** hello szakasza **hitelesítési** lapon hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ea564-185">tooconfigure single sign-on on **Dropbox for Business** side, Go on your Dropbox for Business tenant, in hello **Single sign-on** section of hello **Authentication** page, perform hello following steps:</span></span> 
   
    <span data-ttu-id="ea564-186">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="ea564-186">![Configure single sign-on](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="ea564-187">a.</span><span class="sxs-lookup"><span data-stu-id="ea564-187">a.</span></span> <span data-ttu-id="ea564-188">Kattintson a **szükséges**.</span><span class="sxs-lookup"><span data-stu-id="ea564-188">Click **Required**.</span></span>
   
    <span data-ttu-id="ea564-189">b.</span><span class="sxs-lookup"><span data-stu-id="ea564-189">b.</span></span> <span data-ttu-id="ea564-190">Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="ea564-190">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **Sign-in URL** textbox.</span></span>

    <span data-ttu-id="ea564-191">c.</span><span class="sxs-lookup"><span data-stu-id="ea564-191">c.</span></span> <span data-ttu-id="ea564-192">Kattintson a **tanúsítvány kiválasztása**, majd keresse meg a tooyour **Base64 kódolású tanúsítvány fájl**.</span><span class="sxs-lookup"><span data-stu-id="ea564-192">Click **Choose certificate**, and then browse tooyour **Base64 encoded certificate file**.</span></span>

    <span data-ttu-id="ea564-193">d.</span><span class="sxs-lookup"><span data-stu-id="ea564-193">d.</span></span> <span data-ttu-id="ea564-194">Kattintson a **módosítások mentése** toocomplete hello konfigurációt a DropBox üzleti bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="ea564-194">Click **Save changes** toocomplete hello configuration on your DropBox for Business tenant.</span></span>

> [!TIP]
> <span data-ttu-id="ea564-195">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ea564-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ea564-196">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ea564-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ea564-197">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ea564-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ea564-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea564-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="ea564-199">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ea564-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ea564-201">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ea564-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ea564-202">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ea564-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="ea564-204">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ea564-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ea564-206">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea564-206">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ea564-208">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ea564-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ea564-210">a.</span><span class="sxs-lookup"><span data-stu-id="ea564-210">a.</span></span> <span data-ttu-id="ea564-211">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ea564-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ea564-212">b.</span><span class="sxs-lookup"><span data-stu-id="ea564-212">b.</span></span> <span data-ttu-id="ea564-213">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ea564-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ea564-214">c.</span><span class="sxs-lookup"><span data-stu-id="ea564-214">c.</span></span> <span data-ttu-id="ea564-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ea564-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ea564-216">d.</span><span class="sxs-lookup"><span data-stu-id="ea564-216">d.</span></span> <span data-ttu-id="ea564-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ea564-217">Click **Create**.</span></span>
 
### <a name="creating-a-dropbox-for-business-test-user"></a><span data-ttu-id="ea564-218">A Dropbox az üzleti tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea564-218">Creating a Dropbox for Business test user</span></span>

<span data-ttu-id="ea564-219">Ebben a szakaszban egy Britta Simon nevű felhasználó a vállalati Dropbox jön létre.</span><span class="sxs-lookup"><span data-stu-id="ea564-219">In this section, a user called Britta Simon is created in Dropbox for Business.</span></span> <span data-ttu-id="ea564-220">Vállalati Dropbox támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="ea564-220">Dropbox for Business supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="ea564-221">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="ea564-221">There is no action item for you in this section.</span></span> <span data-ttu-id="ea564-222">Ha a felhasználó nem létezik a Dropbox vállalati, egy új jön létre, a vállalati Dropbox tooaccess tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="ea564-222">If a user doesn't already exist in Dropbox for Business, a new one is created when you attempt tooaccess Dropbox for Business.</span></span>

>[!Note]
><span data-ttu-id="ea564-223">Ha a felhasználó manuálisan, forduljon a toocreate kell [Dropbox üzleti ügyfél-támogatási csoport](https://www.dropbox.com/business/contact)</span><span class="sxs-lookup"><span data-stu-id="ea564-223">If you need toocreate a user manually, Contact [Dropbox for Business Client support team](https://www.dropbox.com/business/contact)</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ea564-224">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ea564-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ea564-225">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést a vállalati hozzáférés tooDropbox megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ea564-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDropbox for Business.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ea564-227">**tooassign Britta Simon tooDropbox vállalati, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ea564-227">**tooassign Britta Simon tooDropbox for Business, perform hello following steps:**</span></span>

1. <span data-ttu-id="ea564-228">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ea564-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ea564-230">Hello alkalmazások listában válassza ki a **Dropbox vállalati**.</span><span class="sxs-lookup"><span data-stu-id="ea564-230">In hello applications list, select **Dropbox for Business**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. <span data-ttu-id="ea564-232">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ea564-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ea564-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ea564-234">Click **Add** button.</span></span> <span data-ttu-id="ea564-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea564-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ea564-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ea564-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ea564-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea564-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ea564-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ea564-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ea564-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ea564-240">Testing single sign-on</span></span>

<span data-ttu-id="ea564-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="ea564-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ea564-242">Ha hello Dropbox az üzleti csempe a hozzáférési Panel hello gombra kattint, üzleti alkalmazás kapja meg a Dropbox bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="ea564-242">When you click hello Dropbox for Business tile in hello Access Panel, you should get login page of your Dropbox for Business application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea564-243">További források</span><span class="sxs-lookup"><span data-stu-id="ea564-243">Additional resources</span></span>

* [<span data-ttu-id="ea564-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ea564-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ea564-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ea564-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ea564-246">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ea564-246">Configure User Provisioning</span></span>](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

