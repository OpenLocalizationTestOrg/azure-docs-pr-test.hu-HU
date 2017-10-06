---
title: "Oktatóanyag: Azure Active Directoryval integrált Kintone |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Kintone között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 4f61fb95c643743504699b175beeff06e01c95c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a><span data-ttu-id="80c7f-103">Oktatóanyag: Azure Active Directoryval integrált Kintone</span><span class="sxs-lookup"><span data-stu-id="80c7f-103">Tutorial: Azure Active Directory integration with Kintone</span></span>

<span data-ttu-id="80c7f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kintone az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="80c7f-104">In this tutorial, you learn how toointegrate Kintone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="80c7f-105">Kintone integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-105">Integrating Kintone with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="80c7f-106">Megadhatja a hozzáférés tooKintone rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="80c7f-106">You can control in Azure AD who has access tooKintone</span></span>
- <span data-ttu-id="80c7f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKintone (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="80c7f-107">You can enable your users tooautomatically get signed-on tooKintone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="80c7f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="80c7f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="80c7f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="80c7f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80c7f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80c7f-110">Prerequisites</span></span>

<span data-ttu-id="80c7f-111">az Azure AD integrálása Kintone tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-111">tooconfigure Azure AD integration with Kintone, you need hello following items:</span></span>

- <span data-ttu-id="80c7f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="80c7f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="80c7f-113">Egy Kintone egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="80c7f-113">A Kintone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="80c7f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="80c7f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="80c7f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="80c7f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="80c7f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="80c7f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="80c7f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="80c7f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="80c7f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="80c7f-118">Scenario description</span></span>
<span data-ttu-id="80c7f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="80c7f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="80c7f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="80c7f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="80c7f-121">Hello gyűjteményből Kintone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="80c7f-121">Adding Kintone from hello gallery</span></span>
2. <span data-ttu-id="80c7f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="80c7f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kintone-from-hello-gallery"></a><span data-ttu-id="80c7f-123">Hello gyűjteményből Kintone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="80c7f-123">Adding Kintone from hello gallery</span></span>
<span data-ttu-id="80c7f-124">tooconfigure hello integrációja Kintone az Azure AD-be, meg kell tooadd Kintone hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="80c7f-124">tooconfigure hello integration of Kintone into Azure AD, you need tooadd Kintone from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="80c7f-125">**tooadd Kintone hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="80c7f-125">**tooadd Kintone from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="80c7f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="80c7f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="80c7f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="80c7f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="80c7f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="80c7f-133">Hello keresési mezőbe, írja be a **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-133">In hello search box, type **Kintone**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_search.png)

5. <span data-ttu-id="80c7f-135">A hello eredmények panelen válassza ki a **Kintone**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-135">In hello results panel, select **Kintone**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="80c7f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="80c7f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="80c7f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Kintone.</span><span class="sxs-lookup"><span data-stu-id="80c7f-138">In this section, you configure and test Azure AD single sign-on with Kintone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="80c7f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kintone tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="80c7f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kintone is tooa user in Azure AD.</span></span> <span data-ttu-id="80c7f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Kintone közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="80c7f-140">In other words, a link relationship between an Azure AD user and hello related user in Kintone needs toobe established.</span></span>

<span data-ttu-id="80c7f-141">Kintone, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="80c7f-141">In Kintone, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="80c7f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Kintone-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="80c7f-142">tooconfigure and test Azure AD single sign-on with Kintone, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="80c7f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="80c7f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="80c7f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="80c7f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="80c7f-145">**[Kintone tesztfelhasználó létrehozása](#creating-a-kintone-test-user)**  -toohave egy megfelelője a Britta Simon a Kintone, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="80c7f-145">**[Creating a Kintone test user](#creating-a-kintone-test-user)** - toohave a counterpart of Britta Simon in Kintone that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="80c7f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="80c7f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="80c7f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="80c7f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="80c7f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="80c7f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="80c7f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Kintone alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="80c7f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kintone application.</span></span>

<span data-ttu-id="80c7f-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Kintone, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="80c7f-150">**tooconfigure Azure AD single sign-on with Kintone, perform hello following steps:**</span></span>

1. <span data-ttu-id="80c7f-151">Az Azure portál, a hello hello **Kintone** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-151">In hello Azure portal, on hello **Kintone** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="80c7f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="80c7f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_samlbase.png)

3. <span data-ttu-id="80c7f-155">A hello **Kintone tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-155">On hello **Kintone Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_url.png)

    <span data-ttu-id="80c7f-157">a.</span><span class="sxs-lookup"><span data-stu-id="80c7f-157">a.</span></span> <span data-ttu-id="80c7f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.kintone.com`</span><span class="sxs-lookup"><span data-stu-id="80c7f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.kintone.com`</span></span>

    <span data-ttu-id="80c7f-159">b.</span><span class="sxs-lookup"><span data-stu-id="80c7f-159">b.</span></span> <span data-ttu-id="80c7f-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="80c7f-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.cybozu.com`|
    | `https://<companyname>.kintone.com`|

    > [!NOTE] 
    > <span data-ttu-id="80c7f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="80c7f-161">These values are not real.</span></span> <span data-ttu-id="80c7f-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="80c7f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="80c7f-163">Ügyfél [Kintone ügyfél-támogatási csoport](https://www.kintone.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="80c7f-163">Contact [Kintone Client support team](https://www.kintone.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="80c7f-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="80c7f-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_certificate.png) 

5. <span data-ttu-id="80c7f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="80c7f-168">A hello **Kintone konfigurációs** kattintson **konfigurálása Kintone** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="80c7f-168">On hello **Kintone Configuration** section, click **Configure Kintone** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="80c7f-169">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="80c7f-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_configure.png) 

7. <span data-ttu-id="80c7f-171">Egy másik webes böngészőablakban, jelentkezzen be a **Kintone** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="80c7f-171">In a different web browser window, log into your **Kintone** company site as an administrator.</span></span>

8. <span data-ttu-id="80c7f-172">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-172">Click **Settings**.</span></span>
   
    <span data-ttu-id="80c7f-173">![Beállítások](./media/active-directory-saas-kintone-tutorial/ic785879.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="80c7f-173">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

9. <span data-ttu-id="80c7f-174">Kattintson a **felhasználók és rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-174">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="80c7f-175">![Felhasználók és rendszergazdák](./media/active-directory-saas-kintone-tutorial/ic785880.png "felhasználók és rendszergazdák")</span><span class="sxs-lookup"><span data-stu-id="80c7f-175">![Users & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "Users & System Administration")</span></span>

10. <span data-ttu-id="80c7f-176">A **rendszerfelügyelet \> biztonsági** kattintson **bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-176">Under **System Administration \> Security** click **Login**.</span></span>
   
    <span data-ttu-id="80c7f-177">![Bejelentkezési](./media/active-directory-saas-kintone-tutorial/ic785881.png "bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="80c7f-177">![Login](./media/active-directory-saas-kintone-tutorial/ic785881.png "Login")</span></span>

11. <span data-ttu-id="80c7f-178">Kattintson a **engedélyezése SAML-alapú hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-178">Click **Enable SAML authentication**.</span></span>
   
    <span data-ttu-id="80c7f-179">![SAML-alapú hitelesítés](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="80c7f-179">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785882.png "SAML Authentication")</span></span>

12. <span data-ttu-id="80c7f-180">A SAML-alapú hitelesítés szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-180">In hello SAML Authentication section, perform hello following steps:</span></span>
    
    <span data-ttu-id="80c7f-181">![SAML-alapú hitelesítés](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML-alapú hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="80c7f-181">![SAML Authentication](./media/active-directory-saas-kintone-tutorial/ic785883.png "SAML Authentication")</span></span>
    
    <span data-ttu-id="80c7f-182">a.</span><span class="sxs-lookup"><span data-stu-id="80c7f-182">a.</span></span> <span data-ttu-id="80c7f-183">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="80c7f-183">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="80c7f-184">b.</span><span class="sxs-lookup"><span data-stu-id="80c7f-184">b.</span></span> <span data-ttu-id="80c7f-185">A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="80c7f-185">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="80c7f-186">c.</span><span class="sxs-lookup"><span data-stu-id="80c7f-186">c.</span></span> <span data-ttu-id="80c7f-187">Kattintson a **Tallózás** tooupload a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="80c7f-187">Click **Browse** tooupload your downloaded certificate.</span></span>
    
    <span data-ttu-id="80c7f-188">d.</span><span class="sxs-lookup"><span data-stu-id="80c7f-188">d.</span></span> <span data-ttu-id="80c7f-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="80c7f-190">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="80c7f-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="80c7f-191">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="80c7f-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="80c7f-192">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="80c7f-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="80c7f-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="80c7f-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="80c7f-194">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="80c7f-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="80c7f-196">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="80c7f-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="80c7f-197">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="80c7f-199">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="80c7f-201">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c7f-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="80c7f-203">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kintone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="80c7f-205">a.</span><span class="sxs-lookup"><span data-stu-id="80c7f-205">a.</span></span> <span data-ttu-id="80c7f-206">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="80c7f-207">b.</span><span class="sxs-lookup"><span data-stu-id="80c7f-207">b.</span></span> <span data-ttu-id="80c7f-208">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="80c7f-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="80c7f-209">c.</span><span class="sxs-lookup"><span data-stu-id="80c7f-209">c.</span></span> <span data-ttu-id="80c7f-210">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="80c7f-211">d.</span><span class="sxs-lookup"><span data-stu-id="80c7f-211">d.</span></span> <span data-ttu-id="80c7f-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-212">Click **Create**.</span></span>
 
### <a name="creating-a-kintone-test-user"></a><span data-ttu-id="80c7f-213">Kintone tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="80c7f-213">Creating a Kintone test user</span></span>

<span data-ttu-id="80c7f-214">az Azure AD tooenable felhasználók toolog a tooKintone, akkor ki kell építenie Kintone be.</span><span class="sxs-lookup"><span data-stu-id="80c7f-214">tooenable Azure AD users toolog in tooKintone, they must be provisioned into Kintone.</span></span>  
<span data-ttu-id="80c7f-215">Kintone hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="80c7f-215">In hello case of Kintone, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="80c7f-216">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-216">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="80c7f-217">Jelentkezzen be tooyour **Kintone** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="80c7f-217">Log in tooyour **Kintone** company site as an administrator.</span></span>

2. <span data-ttu-id="80c7f-218">Kattintson a **beállítás**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-218">Click **Setting**.</span></span>
   
    <span data-ttu-id="80c7f-219">![Beállítások](./media/active-directory-saas-kintone-tutorial/ic785879.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="80c7f-219">![Settings](./media/active-directory-saas-kintone-tutorial/ic785879.png "Settings")</span></span>

3. <span data-ttu-id="80c7f-220">Kattintson a **felhasználók és rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-220">Click **Users & System Administration**.</span></span>
   
    <span data-ttu-id="80c7f-221">![& Felhasználói rendszerfelügyelet](./media/active-directory-saas-kintone-tutorial/ic785880.png "felhasználó & rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="80c7f-221">![User & System Administration](./media/active-directory-saas-kintone-tutorial/ic785880.png "User & System Administration")</span></span>

4. <span data-ttu-id="80c7f-222">A **felhasználói felügyeleti**, kattintson a **részlegek & felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-222">Under **User Administration**, click **Departments & Users**.</span></span>
   
    <span data-ttu-id="80c7f-223">![Részleg és a felhasználók](./media/active-directory-saas-kintone-tutorial/ic785888.png "részleg és a felhasználók")</span><span class="sxs-lookup"><span data-stu-id="80c7f-223">![Department & Users](./media/active-directory-saas-kintone-tutorial/ic785888.png "Department & Users")</span></span>

5. <span data-ttu-id="80c7f-224">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-224">Click **New User**.</span></span>
   
    <span data-ttu-id="80c7f-225">![Új felhasználók](./media/active-directory-saas-kintone-tutorial/ic785889.png "új felhasználók")</span><span class="sxs-lookup"><span data-stu-id="80c7f-225">![New Users](./media/active-directory-saas-kintone-tutorial/ic785889.png "New Users")</span></span>

6. <span data-ttu-id="80c7f-226">A hello **új felhasználó** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="80c7f-226">In hello **New User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="80c7f-227">![Új felhasználók](./media/active-directory-saas-kintone-tutorial/ic785890.png "új felhasználók")</span><span class="sxs-lookup"><span data-stu-id="80c7f-227">![New Users](./media/active-directory-saas-kintone-tutorial/ic785890.png "New Users")</span></span>
   
    <span data-ttu-id="80c7f-228">a.</span><span class="sxs-lookup"><span data-stu-id="80c7f-228">a.</span></span> <span data-ttu-id="80c7f-229">Adjon meg egy **megjelenítendő név**, **bejelentkezési név**, **új jelszó**, **jelszó megerősítése**, **E-mail cím**, és egyéb részletek tooprovision hello a kívánt fiók érvényes AAD kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="80c7f-229">Type a **Display Name**, **Login Name**, **New Password**, **Confirm Password**, **E-mail Address**, and other details of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
 
    <span data-ttu-id="80c7f-230">b.</span><span class="sxs-lookup"><span data-stu-id="80c7f-230">b.</span></span> <span data-ttu-id="80c7f-231">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-231">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="80c7f-232">Bármely más Kintone felhasználói fiók létrehozása eszközök vagy Kintone tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="80c7f-232">You can use any other Kintone user account creation tools or APIs provided by Kintone tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="80c7f-233">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="80c7f-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="80c7f-234">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKintone megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="80c7f-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKintone.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="80c7f-236">**tooassign Britta Simon tooKintone, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="80c7f-236">**tooassign Britta Simon tooKintone, perform hello following steps:**</span></span>

1. <span data-ttu-id="80c7f-237">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="80c7f-239">Hello alkalmazások listában válassza ki a **Kintone**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-239">In hello applications list, select **Kintone**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kintone-tutorial/tutorial_kintone_app.png) 

3. <span data-ttu-id="80c7f-241">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="80c7f-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="80c7f-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c7f-243">Click **Add** button.</span></span> <span data-ttu-id="80c7f-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c7f-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="80c7f-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="80c7f-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="80c7f-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c7f-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="80c7f-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c7f-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="80c7f-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="80c7f-249">Testing single sign-on</span></span>

<span data-ttu-id="80c7f-250">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="80c7f-250">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="80c7f-251">Ha a hozzáférési Panel hello hello Kintone csempe gombra kattint, automatikusan bejelentkezett tooyour Kintone alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="80c7f-251">When you click hello Kintone tile in hello Access Panel, you should get automatically signed-on tooyour Kintone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80c7f-252">További források</span><span class="sxs-lookup"><span data-stu-id="80c7f-252">Additional resources</span></span>

* [<span data-ttu-id="80c7f-253">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="80c7f-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="80c7f-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="80c7f-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kintone-tutorial/tutorial_general_203.png

