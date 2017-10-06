---
title: "Oktatóanyag: Azure Active Directoryval integrált hitelesítésszolgáltató PPM |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a hitelesítésszolgáltató PPM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 571130f3be0529c986aa0d8a08e4172015cd0b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="56a69-103">Oktatóanyag: Azure Active Directoryval integrált hitelesítésszolgáltató PPM</span><span class="sxs-lookup"><span data-stu-id="56a69-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="56a69-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate hitelesítésszolgáltató PPM az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56a69-104">In this tutorial, you learn how toointegrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="56a69-105">Hitelesítésszolgáltató PPM integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="56a69-105">Integrating CA PPM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="56a69-106">Megadhatja a hozzáférés tooCA PPM rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="56a69-106">You can control in Azure AD who has access tooCA PPM</span></span>
- <span data-ttu-id="56a69-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCA PPM (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="56a69-107">You can enable your users tooautomatically get signed-on tooCA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="56a69-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="56a69-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="56a69-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="56a69-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56a69-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="56a69-110">Prerequisites</span></span>

<span data-ttu-id="56a69-111">az Azure AD-integráció a hitelesítésszolgáltató PPM tooconfigure, kell hello a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="56a69-111">tooconfigure Azure AD integration with CA PPM, you need hello following items:</span></span>

- <span data-ttu-id="56a69-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="56a69-112">An Azure AD subscription</span></span>
- <span data-ttu-id="56a69-113">A hitelesítésszolgáltató PPM egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="56a69-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="56a69-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="56a69-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="56a69-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="56a69-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="56a69-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="56a69-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="56a69-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="56a69-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="56a69-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="56a69-118">Scenario description</span></span>
<span data-ttu-id="56a69-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="56a69-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="56a69-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="56a69-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="56a69-121">Hitelesítésszolgáltató PPM hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="56a69-121">Adding CA PPM from hello gallery</span></span>
2. <span data-ttu-id="56a69-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56a69-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-hello-gallery"></a><span data-ttu-id="56a69-123">Hitelesítésszolgáltató PPM hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="56a69-123">Adding CA PPM from hello gallery</span></span>
<span data-ttu-id="56a69-124">tooconfigure hello integrációs hitelesítésszolgáltató ppm, az Azure AD-be, meg kell tooadd hitelesítésszolgáltató PPM hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="56a69-124">tooconfigure hello integration of CA PPM into Azure AD, you need tooadd CA PPM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="56a69-125">**tooadd hitelesítésszolgáltató PPM hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56a69-125">**tooadd CA PPM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="56a69-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56a69-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="56a69-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="56a69-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="56a69-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56a69-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="56a69-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="56a69-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="56a69-133">Hello keresési mezőbe, írja be a **hitelesítésszolgáltató PPM**.</span><span class="sxs-lookup"><span data-stu-id="56a69-133">In hello search box, type **CA PPM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="56a69-135">A hello eredmények panelen válassza a **hitelesítésszolgáltató PPM**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="56a69-135">In hello results panel, select **CA PPM**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="56a69-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56a69-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="56a69-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a hitelesítésszolgáltató PPM "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="56a69-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="56a69-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó a CA PPM tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="56a69-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CA PPM is tooa user in Azure AD.</span></span> <span data-ttu-id="56a69-140">Ez azt jelenti hello kapcsolódó felhasználó a CA PPM és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="56a69-140">In other words, a link relationship between an Azure AD user and hello related user in CA PPM needs toobe established.</span></span>

<span data-ttu-id="56a69-141">A hitelesítésszolgáltató PPM, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="56a69-141">In CA PPM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="56a69-142">tooconfigure és a hitelesítésszolgáltató PPM az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="56a69-142">tooconfigure and test Azure AD single sign-on with CA PPM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="56a69-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="56a69-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="56a69-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="56a69-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="56a69-145">**[Hitelesítésszolgáltató PPM tesztfelhasználó létrehozása](#creating-a-ca-ppm-test-user)**  -toohave egy megfelelője a Britta Simon hitelesítésszolgáltató PPM, amely a felhasználó ábrázolása csatolt toohello az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="56a69-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - toohave a counterpart of Britta Simon in CA PPM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="56a69-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56a69-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="56a69-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="56a69-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="56a69-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="56a69-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="56a69-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a hitelesítésszolgáltató PPM alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="56a69-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="56a69-150">**az Azure AD tooconfigure egyszeri bejelentkezést a hitelesítésszolgáltató PPM, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56a69-150">**tooconfigure Azure AD single sign-on with CA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="56a69-151">Az Azure portál, a hello hello **hitelesítésszolgáltató PPM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="56a69-151">In hello Azure portal, on hello **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="56a69-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56a69-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="56a69-155">A hello **hitelesítésszolgáltató PPM tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56a69-155">On hello **CA PPM Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="56a69-157">a.</span><span class="sxs-lookup"><span data-stu-id="56a69-157">a.</span></span> <span data-ttu-id="56a69-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="56a69-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="56a69-159">b.</span><span class="sxs-lookup"><span data-stu-id="56a69-159">b.</span></span> <span data-ttu-id="56a69-160">A hello **válasz URL-CÍMEN** szövegmezőhöz típusú, mint:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="56a69-160">In hello **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="56a69-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="56a69-161">This value is not real.</span></span> <span data-ttu-id="56a69-162">Frissítse ezt az értéket hello tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="56a69-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="56a69-163">Ügyfél [hitelesítésszolgáltató PPM támogatási csoport](mailto:catechnicalsupport@ca.com) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="56a69-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) tooget this value.</span></span>
 
4. <span data-ttu-id="56a69-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="56a69-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="56a69-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="56a69-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="56a69-168">A hello **hitelesítésszolgáltató PPM konfigurációjának** területén kattintson **konfigurálása hitelesítésszolgáltató PPM** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="56a69-168">On hello **CA PPM Configuration** section, click **Configure CA PPM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="56a69-169">Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="56a69-169">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="56a69-171">tooconfigure egyszeri bejelentkezést a **hitelesítésszolgáltató PPM** oldalon kell letöltött toosend hello **Certificate(Base64)** és **SAML Entitásazonosító** túl[hitelesítésszolgáltató PPM támogatási csoport ](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="56a69-171">tooconfigure single sign-on on **CA PPM** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Entity ID** too[CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="56a69-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="56a69-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="56a69-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="56a69-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="56a69-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="56a69-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="56a69-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56a69-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="56a69-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="56a69-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="56a69-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="56a69-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="56a69-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56a69-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="56a69-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="56a69-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="56a69-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56a69-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="56a69-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56a69-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="56a69-187">a.</span><span class="sxs-lookup"><span data-stu-id="56a69-187">a.</span></span> <span data-ttu-id="56a69-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="56a69-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="56a69-189">b.</span><span class="sxs-lookup"><span data-stu-id="56a69-189">b.</span></span> <span data-ttu-id="56a69-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="56a69-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="56a69-191">c.</span><span class="sxs-lookup"><span data-stu-id="56a69-191">c.</span></span> <span data-ttu-id="56a69-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="56a69-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="56a69-193">d.</span><span class="sxs-lookup"><span data-stu-id="56a69-193">d.</span></span> <span data-ttu-id="56a69-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="56a69-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="56a69-195">Hitelesítésszolgáltató PPM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56a69-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="56a69-196">Ebben a szakaszban egy hitelesítésszolgáltató PPM Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="56a69-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="56a69-197">Együttműködve [hitelesítésszolgáltató PPM támogatási csoport](mailto:catechnicalsupport@ca.com) tooadd hello felhasználók hello hitelesítésszolgáltató PPM platform.</span><span class="sxs-lookup"><span data-stu-id="56a69-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) tooadd hello users in hello CA PPM platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="56a69-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56a69-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="56a69-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCA PPM megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="56a69-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCA PPM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="56a69-201">**tooassign Britta Simon tooCA PPM, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="56a69-201">**tooassign Britta Simon tooCA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="56a69-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56a69-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="56a69-204">Hello alkalmazások listában válassza ki a **hitelesítésszolgáltató PPM**.</span><span class="sxs-lookup"><span data-stu-id="56a69-204">In hello applications list, select **CA PPM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="56a69-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="56a69-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="56a69-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="56a69-208">Click **Add** button.</span></span> <span data-ttu-id="56a69-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56a69-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="56a69-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="56a69-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="56a69-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56a69-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="56a69-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56a69-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="56a69-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="56a69-214">Testing single sign-on</span></span>

<span data-ttu-id="56a69-215">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="56a69-215">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="56a69-216">Hello hitelesítésszolgáltató PPM csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour hitelesítésszolgáltató PPM alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="56a69-216">When you click hello CA PPM tile in hello Access Panel, you should get automatically signed-on tooyour CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56a69-217">További források</span><span class="sxs-lookup"><span data-stu-id="56a69-217">Additional resources</span></span>

* [<span data-ttu-id="56a69-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="56a69-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="56a69-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="56a69-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

