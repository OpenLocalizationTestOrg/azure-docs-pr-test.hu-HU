---
title: "Oktatóanyag: Azure Active Directoryval integrált HPE SaaS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és HPE SaaS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 314003d6-ca66-4456-88c3-934254d4a9a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: 7a846fb2298e51d249f4a406527130828bf7bbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hpe-saas"></a><span data-ttu-id="81c90-103">Oktatóanyag: Azure Active Directoryval integrált HPE SaaS</span><span class="sxs-lookup"><span data-stu-id="81c90-103">Tutorial: Azure Active Directory integration with HPE SaaS</span></span>

<span data-ttu-id="81c90-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HPE SaaS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="81c90-104">In this tutorial, you learn how toointegrate HPE SaaS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="81c90-105">HPE SaaS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="81c90-105">Integrating HPE SaaS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="81c90-106">Megadhatja a hozzáférés tooHPE SaaS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="81c90-106">You can control in Azure AD who has access tooHPE SaaS</span></span>
- <span data-ttu-id="81c90-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHPE SaaS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="81c90-107">You can enable your users tooautomatically get signed-on tooHPE SaaS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="81c90-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="81c90-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="81c90-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="81c90-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81c90-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81c90-110">Prerequisites</span></span>

<span data-ttu-id="81c90-111">az Azure AD integrálása HPE SaaS tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="81c90-111">tooconfigure Azure AD integration with HPE SaaS, you need hello following items:</span></span>

- <span data-ttu-id="81c90-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="81c90-112">An Azure AD subscription</span></span>
- <span data-ttu-id="81c90-113">Egy HPE SaaS egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="81c90-113">An HPE SaaS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="81c90-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="81c90-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="81c90-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="81c90-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="81c90-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="81c90-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="81c90-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81c90-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="81c90-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="81c90-118">Scenario description</span></span>
<span data-ttu-id="81c90-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="81c90-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="81c90-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="81c90-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="81c90-121">Hello gyűjteményből HPE SaaS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="81c90-121">Adding HPE SaaS from hello gallery</span></span>
2. <span data-ttu-id="81c90-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81c90-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hpe-saas-from-hello-gallery"></a><span data-ttu-id="81c90-123">Hello gyűjteményből HPE SaaS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="81c90-123">Adding HPE SaaS from hello gallery</span></span>
<span data-ttu-id="81c90-124">tooconfigure hello integrálása HPE SaaS az Azure AD-be, meg kell tooadd HPE SaaS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="81c90-124">tooconfigure hello integration of HPE SaaS into Azure AD, you need tooadd HPE SaaS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="81c90-125">**tooadd HPE SaaS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="81c90-125">**tooadd HPE SaaS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="81c90-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81c90-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="81c90-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="81c90-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="81c90-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81c90-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="81c90-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="81c90-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="81c90-133">Hello keresési mezőbe, írja be a **HPE SaaS**.</span><span class="sxs-lookup"><span data-stu-id="81c90-133">In hello search box, type **HPE SaaS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_search.png)

5. <span data-ttu-id="81c90-135">A hello eredmények panelen válassza ki a **HPE SaaS**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="81c90-135">In hello results panel, select **HPE SaaS**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="81c90-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="81c90-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="81c90-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés HPE SaaS "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="81c90-138">In this section, you configure and test Azure AD single sign-on with HPE SaaS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="81c90-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó HPE SaaS tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="81c90-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HPE SaaS is tooa user in Azure AD.</span></span> <span data-ttu-id="81c90-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello HPE SaaS közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="81c90-140">In other words, a link relationship between an Azure AD user and hello related user in HPE SaaS needs toobe established.</span></span>

<span data-ttu-id="81c90-141">Rendelje hozzá hello hello értékének HPE SaaS **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="81c90-141">In HPE SaaS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="81c90-142">tooconfigure és az Azure AD az egyszeri bejelentkezés HPE Szolgáltatottszoftver-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="81c90-142">tooconfigure and test Azure AD single sign-on with HPE SaaS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="81c90-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="81c90-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="81c90-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81c90-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="81c90-145">**[Egy HPE SaaS tesztfelhasználó létrehozása](#creating-an-hpe-saas-test-user)**  -toohave egy megfelelője a Britta Simon a HPE SaaS, a felhasználó csatolt toohello az Azure AD-ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="81c90-145">**[Creating an HPE SaaS test user](#creating-an-hpe-saas-test-user)** - toohave a counterpart of Britta Simon in HPE SaaS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="81c90-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="81c90-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="81c90-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="81c90-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="81c90-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81c90-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="81c90-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a HPE SaaS-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="81c90-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HPE SaaS application.</span></span>

<span data-ttu-id="81c90-150">**az Azure AD tooconfigure egyszeri bejelentkezés HPE SaaS, a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="81c90-150">**tooconfigure Azure AD single sign-on with HPE SaaS, perform hello following steps:**</span></span>

1. <span data-ttu-id="81c90-151">Az Azure portál, a hello hello **HPE SaaS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="81c90-151">In hello Azure portal, on hello **HPE SaaS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="81c90-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="81c90-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_samlbase.png)

3. <span data-ttu-id="81c90-155">A hello **HPE Szolgáltatottszoftver-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="81c90-155">On hello **HPE SaaS Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_url.png)

    <span data-ttu-id="81c90-157">a.</span><span class="sxs-lookup"><span data-stu-id="81c90-157">a.</span></span> <span data-ttu-id="81c90-158">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://login.saas.hpe.com/msg`</span><span class="sxs-lookup"><span data-stu-id="81c90-158">In hello **Sign-on URL** textbox, type a URL as: `https://login.saas.hpe.com/msg`</span></span>

    <span data-ttu-id="81c90-159">b.</span><span class="sxs-lookup"><span data-stu-id="81c90-159">b.</span></span> <span data-ttu-id="81c90-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.saas.hpe.com`</span><span class="sxs-lookup"><span data-stu-id="81c90-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.saas.hpe.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="81c90-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="81c90-161">These values are not real.</span></span> <span data-ttu-id="81c90-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="81c90-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="81c90-163">Ügyfél [HPE SaaS ügyfél-támogatási csoport](https://saas.hpe.com/en-us/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="81c90-163">Contact [HPE SaaS Client support team](https://saas.hpe.com/en-us/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="81c90-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="81c90-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_certificate.png) 

5. <span data-ttu-id="81c90-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="81c90-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hpesaas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="81c90-168">tooconfigure egyszeri bejelentkezést a **HPE SaaS** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[HPE SaaS támogatási csoport](https://saas.hpe.com/en-us/contact).</span><span class="sxs-lookup"><span data-stu-id="81c90-168">tooconfigure single sign-on on **HPE SaaS** side, you need toosend hello downloaded **Metadata XML** too[HPE SaaS support team](https://saas.hpe.com/en-us/contact).</span></span> <span data-ttu-id="81c90-169">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="81c90-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="81c90-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="81c90-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="81c90-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="81c90-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="81c90-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="81c90-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="81c90-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81c90-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="81c90-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="81c90-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="81c90-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="81c90-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="81c90-177">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="81c90-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="81c90-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="81c90-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="81c90-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81c90-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="81c90-183">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="81c90-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="81c90-185">a.</span><span class="sxs-lookup"><span data-stu-id="81c90-185">a.</span></span> <span data-ttu-id="81c90-186">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="81c90-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="81c90-187">b.</span><span class="sxs-lookup"><span data-stu-id="81c90-187">b.</span></span> <span data-ttu-id="81c90-188">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="81c90-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="81c90-189">c.</span><span class="sxs-lookup"><span data-stu-id="81c90-189">c.</span></span> <span data-ttu-id="81c90-190">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="81c90-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="81c90-191">d.</span><span class="sxs-lookup"><span data-stu-id="81c90-191">d.</span></span> <span data-ttu-id="81c90-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="81c90-192">Click **Create**.</span></span>
 
### <a name="creating-an-hpe-saas-test-user"></a><span data-ttu-id="81c90-193">Egy HPE SaaS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81c90-193">Creating an HPE SaaS test user</span></span>

<span data-ttu-id="81c90-194">hello ebben a szakaszban célja toocreate HPE SaaS Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="81c90-194">hello objective of this section is toocreate a user called Britta Simon in HPE SaaS.</span></span> <span data-ttu-id="81c90-195">Adjon együttműködve [HPE SaaS támogatási csoport](https://saas.hpe.com/en-us/contact) tooadd hello felhasználók hello HPE Szolgáltatottszoftver-fiók.</span><span class="sxs-lookup"><span data-stu-id="81c90-195">Please work with [HPE SaaS support team](https://saas.hpe.com/en-us/contact) tooadd hello users in hello HPE SaaS account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="81c90-196">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="81c90-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="81c90-197">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHPE SaaS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="81c90-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHPE SaaS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="81c90-199">**tooassign Britta Simon tooHPE SaaS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="81c90-199">**tooassign Britta Simon tooHPE SaaS, perform hello following steps:**</span></span>

1. <span data-ttu-id="81c90-200">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="81c90-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="81c90-202">Hello alkalmazások listában válassza ki a **HPE SaaS**.</span><span class="sxs-lookup"><span data-stu-id="81c90-202">In hello applications list, select **HPE SaaS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_app.png) 

3. <span data-ttu-id="81c90-204">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="81c90-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="81c90-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="81c90-206">Click **Add** button.</span></span> <span data-ttu-id="81c90-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81c90-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="81c90-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="81c90-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="81c90-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81c90-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="81c90-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81c90-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="81c90-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="81c90-212">Testing single sign-on</span></span>

<span data-ttu-id="81c90-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="81c90-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="81c90-214">Hello HPE SaaS hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour HPE SaaS-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="81c90-214">When you click hello HPE SaaS tile in hello Access Panel, you should get automatically signed-on tooyour HPE SaaS application.</span></span>
<span data-ttu-id="81c90-215">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="81c90-215">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="81c90-216">További források</span><span class="sxs-lookup"><span data-stu-id="81c90-216">Additional resources</span></span>

* [<span data-ttu-id="81c90-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="81c90-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="81c90-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="81c90-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_203.png

