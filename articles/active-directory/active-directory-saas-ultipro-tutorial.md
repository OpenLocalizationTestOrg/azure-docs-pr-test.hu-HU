---
title: "Oktatóanyag: Azure Active Directoryval integrált UltiPro |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és UltiPro között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: afc0f2b9-2eac-47ec-af04-65ed0fb0ca5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 8cb613228893e8cbf997957452d55d0ab7939171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ultipro"></a><span data-ttu-id="ceca8-103">Oktatóanyag: Azure Active Directoryval integrált UltiPro</span><span class="sxs-lookup"><span data-stu-id="ceca8-103">Tutorial: Azure Active Directory integration with UltiPro</span></span>

<span data-ttu-id="ceca8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate UltiPro az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ceca8-104">In this tutorial, you learn how toointegrate UltiPro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ceca8-105">UltiPro integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="ceca8-105">Integrating UltiPro with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ceca8-106">Megadhatja a hozzáférés tooUltiPro rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ceca8-106">You can control in Azure AD who has access tooUltiPro</span></span>
- <span data-ttu-id="ceca8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooUltiPro (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ceca8-107">You can enable your users tooautomatically get signed-on tooUltiPro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ceca8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ceca8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ceca8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ceca8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ceca8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ceca8-110">Prerequisites</span></span>

<span data-ttu-id="ceca8-111">az Azure AD integrálása UltiPro tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="ceca8-111">tooconfigure Azure AD integration with UltiPro, you need hello following items:</span></span>

- <span data-ttu-id="ceca8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ceca8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ceca8-113">Egy UltiPro egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ceca8-113">A UltiPro single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ceca8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ceca8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ceca8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="ceca8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ceca8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ceca8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ceca8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ceca8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ceca8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ceca8-118">Scenario description</span></span>
<span data-ttu-id="ceca8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ceca8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ceca8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ceca8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ceca8-121">Hello gyűjteményből UltiPro hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ceca8-121">Adding UltiPro from hello gallery</span></span>
2. <span data-ttu-id="ceca8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ceca8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ultipro-from-hello-gallery"></a><span data-ttu-id="ceca8-123">Hello gyűjteményből UltiPro hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ceca8-123">Adding UltiPro from hello gallery</span></span>
<span data-ttu-id="ceca8-124">tooconfigure hello integrációja UltiPro az Azure AD-be, meg kell tooadd UltiPro hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="ceca8-124">tooconfigure hello integration of UltiPro into Azure AD, you need tooadd UltiPro from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ceca8-125">**tooadd UltiPro hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ceca8-125">**tooadd UltiPro from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceca8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ceca8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ceca8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ceca8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="ceca8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ceca8-133">Hello keresési mezőbe, írja be a **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-133">In hello search box, type **UltiPro**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_search.png)

5. <span data-ttu-id="ceca8-135">A hello eredmények panelen válassza ki a **UltiPro**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-135">In hello results panel, select **UltiPro**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ceca8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ceca8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ceca8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UltiPro.</span><span class="sxs-lookup"><span data-stu-id="ceca8-138">In this section, you configure and test Azure AD single sign-on with UltiPro based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ceca8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó UltiPro tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="ceca8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UltiPro is tooa user in Azure AD.</span></span> <span data-ttu-id="ceca8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello UltiPro közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="ceca8-140">In other words, a link relationship between an Azure AD user and hello related user in UltiPro needs toobe established.</span></span>

<span data-ttu-id="ceca8-141">UltiPro, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ceca8-141">In UltiPro, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ceca8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés UltiPro-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="ceca8-142">tooconfigure and test Azure AD single sign-on with UltiPro, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ceca8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ceca8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ceca8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ceca8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ceca8-145">**[UltiPro tesztfelhasználó létrehozása](#creating-a-ultipro-test-user)**  -toohave egy megfelelője a Britta Simon a UltiPro, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ceca8-145">**[Creating a UltiPro test user](#creating-a-ultipro-test-user)** - toohave a counterpart of Britta Simon in UltiPro that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ceca8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ceca8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ceca8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="ceca8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ceca8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ceca8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ceca8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az UltiPro alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ceca8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UltiPro application.</span></span>

<span data-ttu-id="ceca8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a UltiPro, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="ceca8-150">**tooconfigure Azure AD single sign-on with UltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceca8-151">Az Azure portál, a hello hello **UltiPro** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-151">In hello Azure portal, on hello **UltiPro** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ceca8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="ceca8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_samlbase.png)

3. <span data-ttu-id="ceca8-155">A hello **UltiPro tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ceca8-155">On hello **UltiPro Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_url.png)

    <span data-ttu-id="ceca8-157">a.</span><span class="sxs-lookup"><span data-stu-id="ceca8-157">a.</span></span> <span data-ttu-id="ceca8-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="ceca8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/`|
    | `https://<companyname>.ultiproworkplace.com?cpi=AZUREADISSSUERURL`|
    | ` https://<companyname>.ultipro.ca`|
    
    <span data-ttu-id="ceca8-159">b.</span><span class="sxs-lookup"><span data-stu-id="ceca8-159">b.</span></span> <span data-ttu-id="ceca8-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="ceca8-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/adfs/services/trust`|
    | `https://<companyname>.ultiproworkplace.com/adfs/services/trust`|
    | `https://<companyname>.ultipro.ca/adfs/services/trust`|
    
    <span data-ttu-id="ceca8-161">c.</span><span class="sxs-lookup"><span data-stu-id="ceca8-161">c.</span></span> <span data-ttu-id="ceca8-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="ceca8-162">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/<instancename>`|
    | `https://<companyname>.ultiproworkplace.com/<instancename>`|
    | `https://<companyname>.ultipro.ca/<instancename>`|
    
    > [!NOTE] 
    > <span data-ttu-id="ceca8-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ceca8-163">These values are not real.</span></span> <span data-ttu-id="ceca8-164">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="ceca8-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ceca8-165">Ügyfél [UltiPro ügyfél-támogatási csoport](https://www.ultimatesoftware.com/ContactUs) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ceca8-165">Contact [UltiPro Client support team](https://www.ultimatesoftware.com/ContactUs) tooget these values.</span></span> 

5. <span data-ttu-id="ceca8-166">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ceca8-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_certificate.png) 

6. <span data-ttu-id="ceca8-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="ceca8-170">A hello **UltiPro konfigurációs** kattintson **konfigurálása UltiPro** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ceca8-170">On hello **UltiPro Configuration** section, click **Configure UltiPro** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ceca8-171">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ceca8-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_configure.png) 

8. <span data-ttu-id="ceca8-173">tooconfigure egyszeri bejelentkezést a **UltiPro** oldalon kell letöltött toosend hello **Certificate(Base64), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[UltiPro támogatási csoport](https://www.ultimatesoftware.com/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="ceca8-173">tooconfigure single sign-on on **UltiPro** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[UltiPro support team](https://www.ultimatesoftware.com/ContactUs).</span></span>

> [!TIP]
> <span data-ttu-id="ceca8-174">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="ceca8-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ceca8-175">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ceca8-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ceca8-176">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ceca8-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ceca8-177">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ceca8-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="ceca8-178">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="ceca8-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ceca8-180">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="ceca8-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceca8-181">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ceca8-183">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ceca8-185">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ceca8-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ceca8-187">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ceca8-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ultipro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ceca8-189">a.</span><span class="sxs-lookup"><span data-stu-id="ceca8-189">a.</span></span> <span data-ttu-id="ceca8-190">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ceca8-191">b.</span><span class="sxs-lookup"><span data-stu-id="ceca8-191">b.</span></span> <span data-ttu-id="ceca8-192">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ceca8-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ceca8-193">c.</span><span class="sxs-lookup"><span data-stu-id="ceca8-193">c.</span></span> <span data-ttu-id="ceca8-194">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ceca8-195">d.</span><span class="sxs-lookup"><span data-stu-id="ceca8-195">d.</span></span> <span data-ttu-id="ceca8-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-196">Click **Create**.</span></span>
 
### <a name="creating-a-ultipro-test-user"></a><span data-ttu-id="ceca8-197">UltiPro tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ceca8-197">Creating a UltiPro test user</span></span>

<span data-ttu-id="ceca8-198">Ebben a szakaszban egy UltiPro Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ceca8-198">In this section, you create a user called Britta Simon in UltiPro.</span></span> <span data-ttu-id="ceca8-199">Együttműködve [UltiPro támogatási csoport](https://www.ultimatesoftware.com/ContactUs) felhasználót is hozzáadhat hello hello UltiPro platform.</span><span class="sxs-lookup"><span data-stu-id="ceca8-199">Work with [UltiPro support team](https://www.ultimatesoftware.com/ContactUs) to add hello users in hello UltiPro platform.</span></span> <span data-ttu-id="ceca8-200">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="ceca8-200">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ceca8-201">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ceca8-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ceca8-202">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooUltiPro megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ceca8-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUltiPro.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ceca8-204">**tooassign Britta Simon tooUltiPro, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="ceca8-204">**tooassign Britta Simon tooUltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceca8-205">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ceca8-207">Hello alkalmazások listában válassza ki a **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-207">In hello applications list, select **UltiPro**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_app.png) 

3. <span data-ttu-id="ceca8-209">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ceca8-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ceca8-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ceca8-211">Click **Add** button.</span></span> <span data-ttu-id="ceca8-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ceca8-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ceca8-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ceca8-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ceca8-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ceca8-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ceca8-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ceca8-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ceca8-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ceca8-217">Testing single sign-on</span></span>

<span data-ttu-id="ceca8-218">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="ceca8-218">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="ceca8-219">Ha a hozzáférési Panel hello hello UltiPro csempe gombra kattint, automatikusan bejelentkezett tooyour UltiPro alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="ceca8-219">When you click hello UltiPro tile in hello Access Panel, you should get automatically signed-on tooyour UltiPro application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ceca8-220">További források</span><span class="sxs-lookup"><span data-stu-id="ceca8-220">Additional resources</span></span>

* [<span data-ttu-id="ceca8-221">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ceca8-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ceca8-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ceca8-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_203.png

