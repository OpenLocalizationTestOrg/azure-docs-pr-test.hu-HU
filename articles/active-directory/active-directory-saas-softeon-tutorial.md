---
title: "Oktatóanyag: Azure Active Directoryval integrált Softeon WMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Softeon WMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 07c5de0d-90aa-43b3-b24e-0cc334b2f9b0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: 135e4a8a4bc63fd24615e12d037120bf37342547
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-softeon-wms"></a><span data-ttu-id="40a74-103">Oktatóanyag: Azure Active Directoryval integrált Softeon WMS</span><span class="sxs-lookup"><span data-stu-id="40a74-103">Tutorial: Azure Active Directory integration with Softeon WMS</span></span>

<span data-ttu-id="40a74-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Softeon WMS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="40a74-104">In this tutorial, you learn how toointegrate Softeon WMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="40a74-105">Softeon WMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="40a74-105">Integrating Softeon WMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="40a74-106">Megadhatja a hozzáférés tooSofteon WMS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="40a74-106">You can control in Azure AD who has access tooSofteon WMS</span></span>
- <span data-ttu-id="40a74-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSofteon WMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="40a74-107">You can enable your users tooautomatically get signed-on tooSofteon WMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="40a74-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="40a74-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="40a74-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="40a74-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40a74-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="40a74-110">Prerequisites</span></span>

<span data-ttu-id="40a74-111">az Azure AD integrálása Softeon WMS tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="40a74-111">tooconfigure Azure AD integration with Softeon WMS, you need hello following items:</span></span>

- <span data-ttu-id="40a74-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="40a74-112">An Azure AD subscription</span></span>
- <span data-ttu-id="40a74-113">Egy Softeon WMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="40a74-113">A Softeon WMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="40a74-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="40a74-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="40a74-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="40a74-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="40a74-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="40a74-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="40a74-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40a74-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="40a74-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="40a74-118">Scenario description</span></span>
<span data-ttu-id="40a74-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="40a74-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="40a74-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="40a74-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="40a74-121">Hello gyűjteményből Softeon WMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="40a74-121">Adding Softeon WMS from hello gallery</span></span>
2. <span data-ttu-id="40a74-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="40a74-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-softeon-wms-from-hello-gallery"></a><span data-ttu-id="40a74-123">Hello gyűjteményből Softeon WMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="40a74-123">Adding Softeon WMS from hello gallery</span></span>
<span data-ttu-id="40a74-124">tooconfigure hello integrációja Softeon WMS az Azure AD-be, meg kell tooadd Softeon WMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="40a74-124">tooconfigure hello integration of Softeon WMS into Azure AD, you need tooadd Softeon WMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="40a74-125">**tooadd Softeon WMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="40a74-125">**tooadd Softeon WMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="40a74-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="40a74-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="40a74-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="40a74-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="40a74-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="40a74-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="40a74-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="40a74-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="40a74-133">Hello keresési mezőbe, írja be a **Softeon WMS**.</span><span class="sxs-lookup"><span data-stu-id="40a74-133">In hello search box, type **Softeon WMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_search.png)

5. <span data-ttu-id="40a74-135">A hello eredmények panelen válassza ki a **Softeon WMS**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="40a74-135">In hello results panel, select **Softeon WMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="40a74-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="40a74-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="40a74-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Softeon WMS-teszthez "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="40a74-138">In this section, you configure and test Azure AD single sign-on with Softeon WMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="40a74-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Softeon WMS tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="40a74-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Softeon WMS is tooa user in Azure AD.</span></span> <span data-ttu-id="40a74-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Softeon WMS közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="40a74-140">In other words, a link relationship between an Azure AD user and hello related user in Softeon WMS needs toobe established.</span></span>

<span data-ttu-id="40a74-141">Softeon WMS, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="40a74-141">In Softeon WMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="40a74-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Softeon WMS-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="40a74-142">tooconfigure and test Azure AD single sign-on with Softeon WMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="40a74-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="40a74-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="40a74-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40a74-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="40a74-145">**[Softeon WMS tesztfelhasználó létrehozása](#creating-a-softeon-wms-test-user)**  -toohave egy megfelelője a Britta Simon a Softeon WMS, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="40a74-145">**[Creating a Softeon WMS test user](#creating-a-softeon-wms-test-user)** - toohave a counterpart of Britta Simon in Softeon WMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="40a74-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="40a74-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="40a74-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="40a74-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="40a74-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="40a74-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="40a74-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Softeon WMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="40a74-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Softeon WMS application.</span></span>

<span data-ttu-id="40a74-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Softeon WMS, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="40a74-150">**tooconfigure Azure AD single sign-on with Softeon WMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="40a74-151">Az Azure portál, a hello hello **Softeon WMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="40a74-151">In hello Azure portal, on hello **Softeon WMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="40a74-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="40a74-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_samlbase.png)

3. <span data-ttu-id="40a74-155">A hello **Softeon WMS tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="40a74-155">On hello **Softeon WMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_url.png)

    <span data-ttu-id="40a74-157">a.</span><span class="sxs-lookup"><span data-stu-id="40a74-157">a.</span></span> <span data-ttu-id="40a74-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.softeon.com/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="40a74-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.softeon.com/<instancename>`</span></span>

    <span data-ttu-id="40a74-159">b.</span><span class="sxs-lookup"><span data-stu-id="40a74-159">b.</span></span> <span data-ttu-id="40a74-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.softeon.com/sp`</span><span class="sxs-lookup"><span data-stu-id="40a74-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.softeon.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="40a74-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="40a74-161">These values are not real.</span></span> <span data-ttu-id="40a74-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="40a74-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="40a74-163">Ügyfél [Softeon WMS ügyfél-támogatási csoport](mailto:contact@softeon.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="40a74-163">Contact [Softeon WMS Client support team](mailto:contact@softeon.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="40a74-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="40a74-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_certificate.png) 

5. <span data-ttu-id="40a74-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="40a74-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="40a74-168">A hello **Softeon WMS konfigurációs** kattintson **konfigurálása Softeon WMS** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="40a74-168">On hello **Softeon WMS Configuration** section, click **Configure Softeon WMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="40a74-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="40a74-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_configure.png) 

7. <span data-ttu-id="40a74-171">tooconfigure egyszeri bejelentkezést a **Softeon WMS** oldalon kell letöltött toosend hello **tanúsítvány (Base64), a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[Softeon WMS támogatása Team](mailto:contact@softeon.com).</span><span class="sxs-lookup"><span data-stu-id="40a74-171">tooconfigure single sign-on on **Softeon WMS** side, you need toosend hello downloaded **Certificate (Base64), SAML Entity ID, and SAML Single Sign-On Service URL** too[Softeon WMS support team](mailto:contact@softeon.com).</span></span> <span data-ttu-id="40a74-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="40a74-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="40a74-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="40a74-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="40a74-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="40a74-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="40a74-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="40a74-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="40a74-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="40a74-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="40a74-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="40a74-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="40a74-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="40a74-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="40a74-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="40a74-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="40a74-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="40a74-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="40a74-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="40a74-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="40a74-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="40a74-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-softeon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="40a74-188">a.</span><span class="sxs-lookup"><span data-stu-id="40a74-188">a.</span></span> <span data-ttu-id="40a74-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="40a74-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="40a74-190">b.</span><span class="sxs-lookup"><span data-stu-id="40a74-190">b.</span></span> <span data-ttu-id="40a74-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="40a74-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="40a74-192">c.</span><span class="sxs-lookup"><span data-stu-id="40a74-192">c.</span></span> <span data-ttu-id="40a74-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="40a74-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="40a74-194">d.</span><span class="sxs-lookup"><span data-stu-id="40a74-194">d.</span></span> <span data-ttu-id="40a74-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="40a74-195">Click **Create**.</span></span>
 
### <a name="creating-a-softeon-wms-test-user"></a><span data-ttu-id="40a74-196">Softeon WMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="40a74-196">Creating a Softeon WMS test user</span></span>

<span data-ttu-id="40a74-197">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="40a74-197">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="40a74-198">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="40a74-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="40a74-199">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSofteon WMS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="40a74-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSofteon WMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="40a74-201">**tooassign Britta Simon tooSofteon WMS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="40a74-201">**tooassign Britta Simon tooSofteon WMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="40a74-202">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="40a74-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="40a74-204">Hello alkalmazások listában válassza ki a **Softeon WMS**.</span><span class="sxs-lookup"><span data-stu-id="40a74-204">In hello applications list, select **Softeon WMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-softeon-tutorial/tutorial_softeon_app.png) 

3. <span data-ttu-id="40a74-206">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="40a74-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="40a74-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="40a74-208">Click **Add** button.</span></span> <span data-ttu-id="40a74-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="40a74-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="40a74-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="40a74-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="40a74-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="40a74-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="40a74-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="40a74-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="40a74-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="40a74-214">Testing single sign-on</span></span>

<span data-ttu-id="40a74-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="40a74-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="40a74-216">Ha hello Softeon WMS csempe a hozzáférési Panel hello gombra kattint, Softeon WMS alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="40a74-216">When you click hello Softeon WMS tile in hello Access Panel, you should get login page of Softeon WMS application.</span></span>
<span data-ttu-id="40a74-217">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="40a74-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="40a74-218">További források</span><span class="sxs-lookup"><span data-stu-id="40a74-218">Additional resources</span></span>

* [<span data-ttu-id="40a74-219">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="40a74-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="40a74-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="40a74-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-softeon-tutorial/tutorial_general_203.png

