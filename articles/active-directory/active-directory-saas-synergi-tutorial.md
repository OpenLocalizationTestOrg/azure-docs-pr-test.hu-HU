---
title: "Oktatóanyag: Azure Active Directoryval integrált Synergi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Synergi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5d4a9a596db2a60dda5bcac2c86b88b460a2e2cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a><span data-ttu-id="9f3af-103">Oktatóanyag: Azure Active Directoryval integrált Synergi</span><span class="sxs-lookup"><span data-stu-id="9f3af-103">Tutorial: Azure Active Directory integration with Synergi</span></span>

<span data-ttu-id="9f3af-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Synergi az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f3af-104">In this tutorial, you learn how toointegrate Synergi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f3af-105">Synergi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9f3af-105">Integrating Synergi with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9f3af-106">Az Azure AD hozzáférési tooSynergi rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="9f3af-106">You can control in Azure AD who has access tooSynergi.</span></span>
- <span data-ttu-id="9f3af-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSynergi (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="9f3af-107">You can enable your users tooautomatically get signed-on tooSynergi (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9f3af-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="9f3af-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="9f3af-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f3af-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f3af-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9f3af-110">Prerequisites</span></span>

<span data-ttu-id="9f3af-111">az Azure AD integrálása Synergi tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9f3af-111">tooconfigure Azure AD integration with Synergi, you need hello following items:</span></span>

- <span data-ttu-id="9f3af-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9f3af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9f3af-113">Egy Synergi egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9f3af-113">A Synergi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9f3af-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9f3af-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9f3af-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9f3af-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9f3af-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9f3af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9f3af-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f3af-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f3af-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9f3af-118">Scenario description</span></span>
<span data-ttu-id="9f3af-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9f3af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9f3af-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9f3af-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f3af-121">Hello gyűjteményből Synergi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9f3af-121">Adding Synergi from hello gallery</span></span>
2. <span data-ttu-id="9f3af-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9f3af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-synergi-from-hello-gallery"></a><span data-ttu-id="9f3af-123">Hello gyűjteményből Synergi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9f3af-123">Adding Synergi from hello gallery</span></span>
<span data-ttu-id="9f3af-124">tooconfigure hello integrációja Synergi az Azure AD-be, meg kell tooadd Synergi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9f3af-124">tooconfigure hello integration of Synergi into Azure AD, you need tooadd Synergi from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9f3af-125">**tooadd Synergi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3af-125">**tooadd Synergi from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3af-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="9f3af-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9f3af-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="9f3af-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9f3af-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="9f3af-133">Hello keresési mezőbe, írja be a **Synergi**, jelölje be **Synergi** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-133">In hello search box, type **Synergi**, select **Synergi** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Synergi](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9f3af-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f3af-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9f3af-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Synergi.</span><span class="sxs-lookup"><span data-stu-id="9f3af-136">In this section, you configure and test Azure AD single sign-on with Synergi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9f3af-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Synergi tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9f3af-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Synergi is tooa user in Azure AD.</span></span> <span data-ttu-id="9f3af-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Synergi közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9f3af-138">In other words, a link relationship between an Azure AD user and hello related user in Synergi needs toobe established.</span></span>

<span data-ttu-id="9f3af-139">Synergi, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="9f3af-139">In Synergi, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9f3af-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Synergi-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9f3af-140">tooconfigure and test Azure AD single sign-on with Synergi, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9f3af-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9f3af-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9f3af-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f3af-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9f3af-143">**[Synergi tesztfelhasználó létrehozása](#create-a-synergi-test-user)**  -toohave egy megfelelője a Britta Simon a Synergi, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9f3af-143">**[Create a Synergi test user](#create-a-synergi-test-user)** - toohave a counterpart of Britta Simon in Synergi that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9f3af-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9f3af-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9f3af-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9f3af-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9f3af-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f3af-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9f3af-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Synergi alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9f3af-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Synergi application.</span></span>

<span data-ttu-id="9f3af-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Synergi, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3af-148">**tooconfigure Azure AD single sign-on with Synergi, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3af-149">Az Azure portál, a hello hello **Synergi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-149">In hello Azure portal, on hello **Synergi** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="9f3af-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9f3af-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_samlbase.png)

3. <span data-ttu-id="9f3af-153">A hello **Synergi tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9f3af-153">On hello **Synergi Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Synergi tartomány és az URL-címek](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_url.png)

    <span data-ttu-id="9f3af-155">a.</span><span class="sxs-lookup"><span data-stu-id="9f3af-155">a.</span></span> <span data-ttu-id="9f3af-156">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.irmsecurity.com`</span><span class="sxs-lookup"><span data-stu-id="9f3af-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.irmsecurity.com`</span></span>

    <span data-ttu-id="9f3af-157">b.</span><span class="sxs-lookup"><span data-stu-id="9f3af-157">b.</span></span> <span data-ttu-id="9f3af-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.irmsecurity.com/sso/<organization id>`</span><span class="sxs-lookup"><span data-stu-id="9f3af-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.irmsecurity.com/sso/<organization id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9f3af-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9f3af-159">These values are not real.</span></span> <span data-ttu-id="9f3af-160">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="9f3af-160">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="9f3af-161">Ügyfél [Synergi támogatási csoport](https://www.irmsecurity.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9f3af-161">Contact [Synergi support team](https://www.irmsecurity.com/contact/) tooget these values.</span></span>

4. <span data-ttu-id="9f3af-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9f3af-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_certificate.png) 

5. <span data-ttu-id="9f3af-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-synergi-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9f3af-166">A hello **Synergi konfigurációs** kattintson **konfigurálása Synergi** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9f3af-166">On hello **Synergi Configuration** section, click **Configure Synergi** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9f3af-167">Másolás hello **Sign-Out URL-cím és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9f3af-167">Copy hello **Sign-Out URL and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Synergi konfiguráció](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_configure.png) 

7. <span data-ttu-id="9f3af-169">tooconfigure egyszeri bejelentkezést a **Synergi** oldalon kell letöltött toosend hello **Certificate(Base64) Sign-Out URL-cím és SAML Entitásazonosító** túl[Synergi támogatási csoport](https://www.irmsecurity.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="9f3af-169">tooconfigure single sign-on on **Synergi** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, and SAML Entity ID** too[Synergi support team](https://www.irmsecurity.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="9f3af-170">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="9f3af-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9f3af-171">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="9f3af-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9f3af-172">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9f3af-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9f3af-173">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9f3af-173">Create an Azure AD test user</span></span>

<span data-ttu-id="9f3af-174">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9f3af-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="9f3af-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9f3af-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3af-177">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-177">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-synergi-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9f3af-179">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-179">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-synergi-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9f3af-181">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="9f3af-181">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-synergi-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9f3af-183">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9f3af-183">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-synergi-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9f3af-185">a.</span><span class="sxs-lookup"><span data-stu-id="9f3af-185">a.</span></span> <span data-ttu-id="9f3af-186">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-186">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9f3af-187">b.</span><span class="sxs-lookup"><span data-stu-id="9f3af-187">b.</span></span> <span data-ttu-id="9f3af-188">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="9f3af-188">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="9f3af-189">c.</span><span class="sxs-lookup"><span data-stu-id="9f3af-189">c.</span></span> <span data-ttu-id="9f3af-190">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9f3af-190">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="9f3af-191">d.</span><span class="sxs-lookup"><span data-stu-id="9f3af-191">d.</span></span> <span data-ttu-id="9f3af-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-192">Click **Create**.</span></span>
  
### <a name="create-a-synergi-test-user"></a><span data-ttu-id="9f3af-193">Synergi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f3af-193">Create a Synergi test user</span></span>

<span data-ttu-id="9f3af-194">Ebben a szakaszban egy Synergi Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9f3af-194">In this section, you create a user called Britta Simon in Synergi.</span></span> <span data-ttu-id="9f3af-195">Együttműködve [Synergi támogatási csoport](https://www.irmsecurity.com/contact/) felhasználót is hozzáadhat hello hello Synergi platform.</span><span class="sxs-lookup"><span data-stu-id="9f3af-195">Work with [Synergi support team](https://www.irmsecurity.com/contact/) to add hello users in hello Synergi platform.</span></span> <span data-ttu-id="9f3af-196">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="9f3af-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9f3af-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="9f3af-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9f3af-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSynergi megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="9f3af-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSynergi.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="9f3af-200">**tooassign Britta Simon tooSynergi, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3af-200">**tooassign Britta Simon tooSynergi, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3af-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9f3af-203">Hello alkalmazások listában válassza ki a **Synergi**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-203">In hello applications list, select **Synergi**.</span></span>

    ![hello Synergi hivatkozásra hello alkalmazások listája](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_app.png)  

3. <span data-ttu-id="9f3af-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9f3af-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="9f3af-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9f3af-207">Click **Add** button.</span></span> <span data-ttu-id="9f3af-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9f3af-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="9f3af-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9f3af-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9f3af-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9f3af-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9f3af-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9f3af-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9f3af-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9f3af-213">Test single sign-on</span></span>

<span data-ttu-id="9f3af-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="9f3af-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9f3af-215">Ha a hozzáférési Panel hello hello Synergi csempe gombra kattint, automatikusan bejelentkezett tooyour Synergi alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="9f3af-215">When you click hello Synergi tile in hello Access Panel, you should get automatically signed-on tooyour Synergi application.</span></span>
<span data-ttu-id="9f3af-216">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9f3af-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9f3af-217">További források</span><span class="sxs-lookup"><span data-stu-id="9f3af-217">Additional resources</span></span>

* [<span data-ttu-id="9f3af-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9f3af-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f3af-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9f3af-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_203.png

