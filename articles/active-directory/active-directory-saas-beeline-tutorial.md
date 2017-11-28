---
title: "Oktatóanyag: Azure Active Directoryval integrált BeeLine |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BeeLine között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 92f228d33980c21ad934185ab89d73795f7f69bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a><span data-ttu-id="bccba-103">Oktatóanyag: Azure Active Directoryval integrált BeeLine</span><span class="sxs-lookup"><span data-stu-id="bccba-103">Tutorial: Azure Active Directory integration with BeeLine</span></span>

<span data-ttu-id="bccba-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BeeLine az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bccba-104">In this tutorial, you learn how toointegrate BeeLine with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bccba-105">BeeLine integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="bccba-105">Integrating BeeLine with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bccba-106">Megadhatja a hozzáférés tooBeeLine rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bccba-106">You can control in Azure AD who has access tooBeeLine</span></span>
- <span data-ttu-id="bccba-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBeeLine (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bccba-107">You can enable your users tooautomatically get signed-on tooBeeLine (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bccba-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bccba-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bccba-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bccba-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bccba-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bccba-110">Prerequisites</span></span>

<span data-ttu-id="bccba-111">az Azure AD integrálása BeeLine tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="bccba-111">tooconfigure Azure AD integration with BeeLine, you need hello following items:</span></span>

- <span data-ttu-id="bccba-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bccba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bccba-113">Egy BeeLine egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bccba-113">A BeeLine single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bccba-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bccba-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bccba-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="bccba-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bccba-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bccba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bccba-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bccba-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bccba-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bccba-118">Scenario description</span></span>
<span data-ttu-id="bccba-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bccba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bccba-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bccba-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bccba-121">Hello gyűjteményből BeeLine hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bccba-121">Adding BeeLine from hello gallery</span></span>
2. <span data-ttu-id="bccba-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bccba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-beeline-from-hello-gallery"></a><span data-ttu-id="bccba-123">Hello gyűjteményből BeeLine hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bccba-123">Adding BeeLine from hello gallery</span></span>
<span data-ttu-id="bccba-124">tooconfigure hello integrációja BeeLine az Azure AD-be, meg kell tooadd BeeLine hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="bccba-124">tooconfigure hello integration of BeeLine into Azure AD, you need tooadd BeeLine from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bccba-125">**tooadd BeeLine hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bccba-125">**tooadd BeeLine from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bccba-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bccba-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bccba-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bccba-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bccba-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bccba-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bccba-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="bccba-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bccba-133">Hello keresési mezőbe, írja be a **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="bccba-133">In hello search box, type **BeeLine**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. <span data-ttu-id="bccba-135">A hello eredmények panelen válassza ki a **BeeLine**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="bccba-135">In hello results panel, select **BeeLine**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bccba-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bccba-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bccba-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BeeLine</span><span class="sxs-lookup"><span data-stu-id="bccba-138">In this section, you configure and test Azure AD single sign-on with BeeLine based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bccba-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BeeLine tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="bccba-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BeeLine is tooa user in Azure AD.</span></span> <span data-ttu-id="bccba-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BeeLine közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="bccba-140">In other words, a link relationship between an Azure AD user and hello related user in BeeLine needs toobe established.</span></span>

<span data-ttu-id="bccba-141">BeeLine, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bccba-141">In BeeLine, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bccba-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BeeLine-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="bccba-142">tooconfigure and test Azure AD single sign-on with BeeLine, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bccba-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bccba-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bccba-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bccba-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bccba-145">**[BeeLine tesztfelhasználó létrehozása](#creating-a-beeline-test-user)**  -toohave egy megfelelője a Britta Simon a BeeLine, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="bccba-145">**[Creating a BeeLine test user](#creating-a-beeline-test-user)** - toohave a counterpart of Britta Simon in BeeLine that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bccba-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bccba-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bccba-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="bccba-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bccba-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bccba-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bccba-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BeeLine alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bccba-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BeeLine application.</span></span>

<span data-ttu-id="bccba-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BeeLine, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="bccba-150">**tooconfigure Azure AD single sign-on with BeeLine, perform hello following steps:**</span></span>

1. <span data-ttu-id="bccba-151">Az Azure portál, a hello hello **BeeLine** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bccba-151">In hello Azure portal, on hello **BeeLine** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bccba-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="bccba-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. <span data-ttu-id="bccba-155">A hello **BeeLine tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bccba-155">On hello **BeeLine Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    <span data-ttu-id="bccba-157">a.</span><span class="sxs-lookup"><span data-stu-id="bccba-157">a.</span></span> <span data-ttu-id="bccba-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://projects.beeline.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="bccba-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://projects.beeline.net/<instancename>`</span></span>

    <span data-ttu-id="bccba-159">b.</span><span class="sxs-lookup"><span data-stu-id="bccba-159">b.</span></span> <span data-ttu-id="bccba-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="bccba-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > <span data-ttu-id="bccba-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bccba-161">These values are not real.</span></span> <span data-ttu-id="bccba-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="bccba-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="bccba-163">Ügyfél [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bccba-163">Contact [BeeLine support team](https://www.beeline.com/contact-us/) tooget these values.</span></span>
 
4. <span data-ttu-id="bccba-164">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bccba-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. <span data-ttu-id="bccba-166">A Beeline alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="bccba-166">Your Beeline application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="bccba-167">Adjon együttműködve [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) első tooidentify hello megfelelő felhasználói azonosító, amely rendelendő számítógépekre hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bccba-167">Please work with [BeeLine support team](https://www.beeline.com/contact-us/) first tooidentify hello correct user identifier which will be mapped into hello application.</span></span> <span data-ttu-id="bccba-168">Is hajtsa végre a hello útmutatást [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) kapcsolatos hello attribútum, amely toouse kívánják-e a leképezés.</span><span class="sxs-lookup"><span data-stu-id="bccba-168">Also please take hello guidance from [BeeLine support team](https://www.beeline.com/contact-us/) about hello attribute which they want toouse for this mapping.</span></span> <span data-ttu-id="bccba-169">Ez az attribútum értékének hello kezelheti hello **felhasználói attribútumok** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="bccba-169">You can manage hello value of this attribute from hello **User Attributes** tab of hello application.</span></span> <span data-ttu-id="bccba-170">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="bccba-170">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="bccba-171">Itt azt leképezett hello **felhasználói azonosító** hello jogcím **userprincipalname** attribútum, amely egyedi felhasználói azonosító, ez az elküldött toohello Beeline alkalmazás hello biztosít minden sikeres SAML Válasz.</span><span class="sxs-lookup"><span data-stu-id="bccba-171">Here we have mapped hello **User Identifier** claim with hello **userprincipalname** attribute, which provides unique user ID, which will be sent toohello Beeline application in hello every successful SAML Response.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. <span data-ttu-id="bccba-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bccba-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="bccba-175">A hello **BeeLine konfigurációs** kattintson **konfigurálása BeeLine** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="bccba-175">On hello **BeeLine Configuration** section, click **Configure BeeLine** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bccba-176">Másolás hello **Sign-Out URL-cím** és **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="bccba-176">Copy hello **Sign-Out URL** and **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. <span data-ttu-id="bccba-178">tooconfigure egyszeri bejelentkezést a **BeeLine** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **SAML Entitásazonosító**, **Sign-Out URL-cím**túl[BeeLine támogatási csoport](https://www.beeline.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="bccba-178">tooconfigure single sign-on on **BeeLine** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID**, **Sign-Out URL** too[BeeLine support team](https://www.beeline.com/contact-us/).</span></span>

> [!TIP]
> <span data-ttu-id="bccba-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="bccba-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bccba-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="bccba-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bccba-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bccba-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bccba-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bccba-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="bccba-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="bccba-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bccba-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="bccba-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bccba-186">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bccba-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bccba-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bccba-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bccba-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bccba-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bccba-192">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bccba-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bccba-194">a.</span><span class="sxs-lookup"><span data-stu-id="bccba-194">a.</span></span> <span data-ttu-id="bccba-195">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bccba-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bccba-196">b.</span><span class="sxs-lookup"><span data-stu-id="bccba-196">b.</span></span> <span data-ttu-id="bccba-197">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bccba-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bccba-198">c.</span><span class="sxs-lookup"><span data-stu-id="bccba-198">c.</span></span> <span data-ttu-id="bccba-199">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bccba-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bccba-200">d.</span><span class="sxs-lookup"><span data-stu-id="bccba-200">d.</span></span> <span data-ttu-id="bccba-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bccba-201">Click **Create**.</span></span>
 
### <a name="creating-a-beeline-test-user"></a><span data-ttu-id="bccba-202">BeeLine tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bccba-202">Creating a BeeLine test user</span></span>

<span data-ttu-id="bccba-203">Ebben a szakaszban egy Beeline Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bccba-203">In this section, you create a user called Britta Simon in Beeline.</span></span> <span data-ttu-id="bccba-204">Beeline alkalmazást minden hello felhasználók toobe hello alkalmazás egyszeri bejelentkezés végrehajtása előtt üzembe kell.</span><span class="sxs-lookup"><span data-stu-id="bccba-204">Beeline application needs all hello users toobe provisioned in hello application before doing Single Sign On.</span></span> <span data-ttu-id="bccba-205">Ezért dolgozhat a hello [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) tooprovision felhasználóira hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bccba-205">So work with hello [BeeLine support team](https://www.beeline.com/contact-us/) tooprovision all these users into hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bccba-206">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bccba-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bccba-207">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBeeLine megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bccba-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBeeLine.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bccba-209">**tooassign Britta Simon tooBeeLine, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="bccba-209">**tooassign Britta Simon tooBeeLine, perform hello following steps:**</span></span>

1. <span data-ttu-id="bccba-210">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bccba-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bccba-212">Hello alkalmazások listában válassza ki a **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="bccba-212">In hello applications list, select **BeeLine**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. <span data-ttu-id="bccba-214">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bccba-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bccba-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bccba-216">Click **Add** button.</span></span> <span data-ttu-id="bccba-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bccba-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bccba-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bccba-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bccba-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bccba-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bccba-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bccba-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bccba-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bccba-222">Testing single sign-on</span></span>

<span data-ttu-id="bccba-223">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bccba-223">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="bccba-224">Ha a hozzáférési Panel hello hello Beeline csempe gombra kattint, automatikusan bejelentkezett tooyour Beeline alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="bccba-224">When you click hello Beeline tile in hello Access Panel, you should get automatically signed-on tooyour Beeline application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bccba-225">További források</span><span class="sxs-lookup"><span data-stu-id="bccba-225">Additional resources</span></span>

* [<span data-ttu-id="bccba-226">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bccba-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bccba-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bccba-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

