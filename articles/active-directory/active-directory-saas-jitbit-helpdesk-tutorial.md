---
title: "Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jitbit segélyszolgálat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f6c837bbb910c1e84f7ed9da676c5ab40f75338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="c51ca-103">Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat</span><span class="sxs-lookup"><span data-stu-id="c51ca-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="c51ca-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jitbit segélyszolgálat az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c51ca-104">In this tutorial, you learn how toointegrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c51ca-105">Jitbit segélyszolgálat integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="c51ca-105">Integrating Jitbit Helpdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c51ca-106">Megadhatja a hozzáférés tooJitbit segélyszolgálat rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c51ca-106">You can control in Azure AD who has access tooJitbit Helpdesk</span></span>
- <span data-ttu-id="c51ca-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJitbit segélyszolgálat (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c51ca-107">You can enable your users tooautomatically get signed-on tooJitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c51ca-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c51ca-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c51ca-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c51ca-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c51ca-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c51ca-110">Prerequisites</span></span>

<span data-ttu-id="c51ca-111">tooconfigure Jitbit segélyszolgálat az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="c51ca-111">tooconfigure Azure AD integration with Jitbit Helpdesk, you need hello following items:</span></span>

- <span data-ttu-id="c51ca-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c51ca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c51ca-113">Egy Jitbit segélyszolgálat egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c51ca-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c51ca-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c51ca-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c51ca-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="c51ca-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c51ca-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c51ca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c51ca-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c51ca-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c51ca-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c51ca-118">Scenario description</span></span>
<span data-ttu-id="c51ca-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c51ca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c51ca-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c51ca-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c51ca-121">Hello gyűjteményből Jitbit segélyszolgálat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c51ca-121">Adding Jitbit Helpdesk from hello gallery</span></span>
2. <span data-ttu-id="c51ca-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c51ca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-hello-gallery"></a><span data-ttu-id="c51ca-123">Hello gyűjteményből Jitbit segélyszolgálat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c51ca-123">Adding Jitbit Helpdesk from hello gallery</span></span>
<span data-ttu-id="c51ca-124">tooconfigure hello integrációja Jitbit segélyszolgálat az Azure AD-be, meg kell tooadd Jitbit segélyszolgálat hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="c51ca-124">tooconfigure hello integration of Jitbit Helpdesk into Azure AD, you need tooadd Jitbit Helpdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c51ca-125">**tooadd Jitbit segélyszolgálat hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c51ca-125">**tooadd Jitbit Helpdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c51ca-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c51ca-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c51ca-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c51ca-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="c51ca-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c51ca-133">Hello keresési mezőbe, írja be a **Jitbit segélyszolgálat**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-133">In hello search box, type **Jitbit Helpdesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="c51ca-135">A hello eredmények panelen válassza ki a **Jitbit segélyszolgálat**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-135">In hello results panel, select **Jitbit Helpdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c51ca-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c51ca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c51ca-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="c51ca-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c51ca-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jitbit segélyszolgálat tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c51ca-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jitbit Helpdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="c51ca-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Jitbit segélyszolgálat közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="c51ca-140">In other words, a link relationship between an Azure AD user and hello related user in Jitbit Helpdesk needs toobe established.</span></span>

<span data-ttu-id="c51ca-141">Jitbit segélyszolgálat, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c51ca-141">In Jitbit Helpdesk, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c51ca-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="c51ca-142">tooconfigure and test Azure AD single sign-on with Jitbit Helpdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c51ca-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c51ca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c51ca-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c51ca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c51ca-145">**[Jitbit segélyszolgálat tesztfelhasználó létrehozása](#creating-a-jitbit-helpdesk-test-user)**  -toohave egy megfelelője a Britta Simon a Jitbit segélyszolgálat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c51ca-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - toohave a counterpart of Britta Simon in Jitbit Helpdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c51ca-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c51ca-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c51ca-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="c51ca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c51ca-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c51ca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c51ca-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jitbit támogató alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c51ca-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="c51ca-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Jitbit segélyszolgálat, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="c51ca-150">**tooconfigure Azure AD single sign-on with Jitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="c51ca-151">Az Azure portál, a hello hello **Jitbit segélyszolgálat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-151">In hello Azure portal, on hello **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c51ca-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c51ca-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="c51ca-155">A hello **Jitbit segélyszolgálat tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c51ca-155">On hello **Jitbit Helpdesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="c51ca-157">a.</span><span class="sxs-lookup"><span data-stu-id="c51ca-157">a.</span></span> <span data-ttu-id="c51ca-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="c51ca-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="c51ca-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c51ca-159">This value is not real.</span></span> <span data-ttu-id="c51ca-160">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c51ca-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c51ca-161">Ügyfél [Jitbit segélyszolgálat ügyfél-támogatási csoport](https://www.jitbit.com/support/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c51ca-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) tooget this value.</span></span> 
    
    <span data-ttu-id="c51ca-162">b.</span><span class="sxs-lookup"><span data-stu-id="c51ca-162">b.</span></span>  <span data-ttu-id="c51ca-163">A hello **azonosító** szövegmező, a következő URL-címet írja be:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="c51ca-163">In hello **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="c51ca-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c51ca-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="c51ca-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c51ca-168">A hello **Jitbit segélyszolgálat konfigurációs** kattintson **Jitbit segélyszolgálat konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c51ca-168">On hello **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c51ca-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c51ca-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="c51ca-171">Egy másik webes böngészőablakban jelentkezzen be a Jitbit segélyszolgálat vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c51ca-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="c51ca-172">Hello hello felső eszköztárán kattintson **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-172">In hello toolbar on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="c51ca-173">![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="c51ca-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="c51ca-174">Kattintson a **általános beállítások**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="c51ca-175">![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "felhasználók, a vállalatok és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="c51ca-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="c51ca-176">A hello **hitelesítési beállítások** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c51ca-176">In hello **Authentication settings** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="c51ca-177">![Hitelesítési beállítások](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "hitelesítési beállítások")</span><span class="sxs-lookup"><span data-stu-id="c51ca-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="c51ca-178">a.</span><span class="sxs-lookup"><span data-stu-id="c51ca-178">a.</span></span> <span data-ttu-id="c51ca-179">Válassza ki **engedélyezése SAML 2.0-s egyszeri bejelentkezés**, az egyszeri bejelentkezés (SSO), használja a toosign **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-179">Select **Enable SAML 2.0 single sign on**, toosign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="c51ca-180">b.</span><span class="sxs-lookup"><span data-stu-id="c51ca-180">b.</span></span> <span data-ttu-id="c51ca-181">A hello **végponti URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c51ca-181">In hello **EndPoint URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c51ca-182">c.</span><span class="sxs-lookup"><span data-stu-id="c51ca-182">c.</span></span> <span data-ttu-id="c51ca-183">Nyissa meg a **base-64** kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="c51ca-183">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="c51ca-184">d.</span><span class="sxs-lookup"><span data-stu-id="c51ca-184">d.</span></span> <span data-ttu-id="c51ca-185">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="c51ca-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="c51ca-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c51ca-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="c51ca-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c51ca-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c51ca-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c51ca-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c51ca-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="c51ca-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="c51ca-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c51ca-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="c51ca-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c51ca-193">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c51ca-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c51ca-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c51ca-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c51ca-199">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="c51ca-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c51ca-201">a.</span><span class="sxs-lookup"><span data-stu-id="c51ca-201">a.</span></span> <span data-ttu-id="c51ca-202">A hello **neve** szövegmezőhöz típus neve megegyezik **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-202">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="c51ca-203">b.</span><span class="sxs-lookup"><span data-stu-id="c51ca-203">b.</span></span> <span data-ttu-id="c51ca-204">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c51ca-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c51ca-205">c.</span><span class="sxs-lookup"><span data-stu-id="c51ca-205">c.</span></span> <span data-ttu-id="c51ca-206">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c51ca-207">d.</span><span class="sxs-lookup"><span data-stu-id="c51ca-207">d.</span></span> <span data-ttu-id="c51ca-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="c51ca-209">Jitbit segélyszolgálat tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c51ca-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="c51ca-210">A sorrend tooenable az Azure AD felhasználók toolog Jitbit segélyszolgálat be azok ki kell építenie Jitbit segélyszolgálat be.</span><span class="sxs-lookup"><span data-stu-id="c51ca-210">In order tooenable Azure AD users toolog into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="c51ca-211">Jitbit segélyszolgálat hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c51ca-211">In hello case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="c51ca-212">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c51ca-212">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c51ca-213">Jelentkezzen be tooyour **Jitbit segélyszolgálat** bérlő.</span><span class="sxs-lookup"><span data-stu-id="c51ca-213">Log in tooyour **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="c51ca-214">Hello hello felső menüben kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-214">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="c51ca-215">![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="c51ca-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="c51ca-216">Kattintson a **felhasználók, a vállalatok és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="c51ca-217">![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "felhasználók, a vállalatok és engedélyek")</span><span class="sxs-lookup"><span data-stu-id="c51ca-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="c51ca-218">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="c51ca-219">![Felhasználó hozzáadása](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="c51ca-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="c51ca-220">A hello létrehozása szakaszban írja be a hello adatok a következőképpen kívánt tooprovision hello Azure AD-fiókot:</span><span class="sxs-lookup"><span data-stu-id="c51ca-220">In hello Create section, type hello data of hello Azure AD account you want tooprovision as follows:</span></span>

    <span data-ttu-id="c51ca-221">![Hozzon létre](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="c51ca-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="c51ca-222">a.</span><span class="sxs-lookup"><span data-stu-id="c51ca-222">a.</span></span> <span data-ttu-id="c51ca-223">A hello **felhasználónév** szövegmezőhöz típus **BrittaSimon**, hello felhasználónév hasonlóan hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c51ca-223">In hello **Username** textbox, type **BrittaSimon**, hello user name as in hello Azure portal.</span></span>

   <span data-ttu-id="c51ca-224">b.</span><span class="sxs-lookup"><span data-stu-id="c51ca-224">b.</span></span> <span data-ttu-id="c51ca-225">A hello **E-mail** szövegmezőhöz hello felhasználó e-mailek, például  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="c51ca-225">In hello **Email** textbox, type email of hello user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="c51ca-226">c.</span><span class="sxs-lookup"><span data-stu-id="c51ca-226">c.</span></span> <span data-ttu-id="c51ca-227">A hello **Utónév** szövegmezőhöz első típusnév hello felhasználó például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-227">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>

   <span data-ttu-id="c51ca-228">d.</span><span class="sxs-lookup"><span data-stu-id="c51ca-228">d.</span></span> <span data-ttu-id="c51ca-229">A hello **Vezetéknév** szövegmezőhöz utolsó típusnév hello felhasználó például **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-229">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span>
   
   <span data-ttu-id="c51ca-230">e.</span><span class="sxs-lookup"><span data-stu-id="c51ca-230">e.</span></span> <span data-ttu-id="c51ca-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="c51ca-232">Bármely más Jitbit segélyszolgálat felhasználói fiók létrehozása eszközök vagy Jitbit segélyszolgálat tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c51ca-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk tooprovision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c51ca-233">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c51ca-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c51ca-234">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJitbit segélyszolgálat megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="c51ca-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJitbit Helpdesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c51ca-236">**tooassign Britta Simon tooJitbit segélyszolgálat, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="c51ca-236">**tooassign Britta Simon tooJitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="c51ca-237">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c51ca-239">Hello alkalmazások listában válassza ki a **Jitbit segélyszolgálat**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-239">In hello applications list, select **Jitbit Helpdesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="c51ca-241">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c51ca-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c51ca-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c51ca-243">Click **Add** button.</span></span> <span data-ttu-id="c51ca-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c51ca-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c51ca-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c51ca-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c51ca-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c51ca-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c51ca-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c51ca-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c51ca-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c51ca-249">Testing single sign-on</span></span>

<span data-ttu-id="c51ca-250">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c51ca-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c51ca-251">Hello Jitbit segélyszolgálat hello hozzáférési Panel csempére kattintva Jitbit segélyszolgálat alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="c51ca-251">When you click hello Jitbit Helpdesk tile in hello Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="c51ca-252">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c51ca-252">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c51ca-253">További források</span><span class="sxs-lookup"><span data-stu-id="c51ca-253">Additional resources</span></span>

* [<span data-ttu-id="c51ca-254">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="c51ca-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c51ca-255">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c51ca-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

