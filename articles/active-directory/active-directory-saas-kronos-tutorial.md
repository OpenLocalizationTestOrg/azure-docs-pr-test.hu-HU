---
title: "Oktatóanyag: Azure Active Directoryval integrált Kronos |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Kronos között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 16fd5c203162d10b78f51b00d79017adaf8632c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="20123-103">Oktatóanyag: Azure Active Directoryval integrált Kronos</span><span class="sxs-lookup"><span data-stu-id="20123-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="20123-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kronos az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20123-104">In this tutorial, you learn how toointegrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20123-105">Kronos integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="20123-105">Integrating Kronos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="20123-106">Megadhatja a hozzáférés tooKronos rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="20123-106">You can control in Azure AD who has access tooKronos</span></span>
- <span data-ttu-id="20123-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKronos (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="20123-107">You can enable your users tooautomatically get signed-on tooKronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="20123-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="20123-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="20123-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20123-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20123-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="20123-110">Prerequisites</span></span>

<span data-ttu-id="20123-111">az Azure AD integrálása Kronos tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="20123-111">tooconfigure Azure AD integration with Kronos, you need hello following items:</span></span>

- <span data-ttu-id="20123-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="20123-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20123-113">A **Kronos munkaerő központi** SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="20123-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="20123-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="20123-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="20123-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="20123-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20123-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="20123-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="20123-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20123-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="20123-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="20123-118">Scenario description</span></span>
<span data-ttu-id="20123-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="20123-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20123-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="20123-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20123-121">Hello gyűjteményből Kronos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20123-121">Adding Kronos from hello gallery</span></span>
2. <span data-ttu-id="20123-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20123-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-hello-gallery"></a><span data-ttu-id="20123-123">Hello gyűjteményből Kronos hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20123-123">Adding Kronos from hello gallery</span></span>
<span data-ttu-id="20123-124">tooconfigure hello integrációja Kronos az Azure AD-be, meg kell tooadd Kronos hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="20123-124">tooconfigure hello integration of Kronos into Azure AD, you need tooadd Kronos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="20123-125">**tooadd Kronos hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="20123-125">**tooadd Kronos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="20123-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20123-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="20123-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="20123-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="20123-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20123-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="20123-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="20123-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="20123-133">Hello keresési mezőbe, írja be a **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="20123-133">In hello search box, type **Kronos**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="20123-135">A hello eredmények panelen válassza ki a **Kronos**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="20123-135">In hello results panel, select **Kronos**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="20123-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20123-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="20123-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Kronos</span><span class="sxs-lookup"><span data-stu-id="20123-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="20123-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kronos tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="20123-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kronos is tooa user in Azure AD.</span></span> <span data-ttu-id="20123-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Kronos közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="20123-140">In other words, a link relationship between an Azure AD user and hello related user in Kronos needs toobe established.</span></span>

<span data-ttu-id="20123-141">Kronos, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="20123-141">In Kronos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="20123-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Kronos-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="20123-142">tooconfigure and test Azure AD single sign-on with Kronos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="20123-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="20123-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="20123-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20123-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="20123-145">**[Kronos tesztfelhasználó létrehozása](#creating-a-kronos-test-user)**  -toohave egy megfelelője a Britta Simon a Kronos, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="20123-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - toohave a counterpart of Britta Simon in Kronos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="20123-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="20123-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20123-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="20123-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="20123-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20123-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="20123-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Kronos alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="20123-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="20123-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Kronos, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="20123-150">**tooconfigure Azure AD single sign-on with Kronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="20123-151">Az Azure portál, a hello hello **Kronos** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="20123-151">In hello Azure portal, on hello **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="20123-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="20123-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="20123-155">A hello **Kronos tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="20123-155">On hello **Kronos Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="20123-157">a.</span><span class="sxs-lookup"><span data-stu-id="20123-157">a.</span></span> <span data-ttu-id="20123-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="20123-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="20123-159">b.</span><span class="sxs-lookup"><span data-stu-id="20123-159">b.</span></span> <span data-ttu-id="20123-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="20123-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="20123-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="20123-161">These values are not real.</span></span> <span data-ttu-id="20123-162">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="20123-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="20123-163">Ügyfél [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="20123-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) tooget these values.</span></span>
 
4. <span data-ttu-id="20123-164">A Kronos alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="20123-164">Your Kronos application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="20123-165">Együttműködve [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) első tooidentify hello megfelelő felhasználói azonosítót, amely hello alkalmazásba van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="20123-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first tooidentify hello correct user identifier, which is mapped into hello application.</span></span> <span data-ttu-id="20123-166">Is hajtsa végre hello attribútum, amely kívánják hello útmutatást toouse Ez a leképezés.</span><span class="sxs-lookup"><span data-stu-id="20123-166">Also please take hello guidance about hello attribute, which they want toouse for this mapping.</span></span>
 
     <span data-ttu-id="20123-167">A Microsoft azt javasolja, hello segítségével **"NameIdentifier"** attribútum az felhasználói azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="20123-167">Microsoft recommends using hello **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="20123-168">Ezek az attribútumok értékének hello kezelheti hello **"Felhasználói attribútumok"** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="20123-168">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="20123-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="20123-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="20123-170">Itt azt leképezett hello **felhasználói azonosító (nameid)** rendelkező **ExtractMailPrefix()** funkciója **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="20123-170">Here we have mapped hello **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="20123-171">Így az hello előtag értéke az e-mailek hello felhasználó, amelynek van hello egyedi felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="20123-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="20123-172">Minden sikeres választ küldött toohello Kronos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="20123-172">This is sent toohello Kronos application in every successful response.</span></span> 
     
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="20123-174">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="20123-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="20123-175">a.</span><span class="sxs-lookup"><span data-stu-id="20123-175">a.</span></span> <span data-ttu-id="20123-176">Hello felhasználói azonosítóját a legördülő listában válassza ki a **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="20123-176">In hello User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="20123-177">b.</span><span class="sxs-lookup"><span data-stu-id="20123-177">b.</span></span> <span data-ttu-id="20123-178">A hello **Mail** legördülő listában válassza ki **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="20123-178">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="20123-179">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="20123-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="20123-181">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="20123-181">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="20123-183">tooconfigure egyszeri bejelentkezést a **Kronos** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="20123-183">tooconfigure single sign-on on **Kronos** side, you need toosend hello downloaded **Metadata XML** too[Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="20123-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="20123-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="20123-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="20123-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="20123-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="20123-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="20123-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20123-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="20123-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="20123-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="20123-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="20123-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="20123-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20123-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="20123-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="20123-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="20123-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20123-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="20123-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="20123-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="20123-199">a.</span><span class="sxs-lookup"><span data-stu-id="20123-199">a.</span></span> <span data-ttu-id="20123-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20123-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20123-201">b.</span><span class="sxs-lookup"><span data-stu-id="20123-201">b.</span></span> <span data-ttu-id="20123-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="20123-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="20123-203">c.</span><span class="sxs-lookup"><span data-stu-id="20123-203">c.</span></span> <span data-ttu-id="20123-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="20123-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="20123-205">d.</span><span class="sxs-lookup"><span data-stu-id="20123-205">d.</span></span> <span data-ttu-id="20123-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="20123-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="20123-207">Kronos tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20123-207">Creating a Kronos test user</span></span>

<span data-ttu-id="20123-208">Ebben a szakaszban egy Kronos Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="20123-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="20123-209">Kronos alkalmazást kell minden hello felhasználók toobe hello alkalmazás egyszeri bejelentkezési előtte kiépítve.</span><span class="sxs-lookup"><span data-stu-id="20123-209">Kronos application needs all hello users toobe provisioned in hello application before doing SSO.</span></span> 

<span data-ttu-id="20123-210">Hello együttműködve [Kronos támogatási csoport](https://www.kronos.in/contact/en-in/form) tooprovision felhasználóira hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="20123-210">Work with hello [Kronos support team](https://www.kronos.in/contact/en-in/form) tooprovision all these users into hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="20123-211">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="20123-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="20123-212">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooKronos megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="20123-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKronos.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="20123-214">**tooassign Britta Simon tooKronos, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="20123-214">**tooassign Britta Simon tooKronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="20123-215">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20123-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="20123-217">Hello alkalmazások listában válassza ki a **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="20123-217">In hello applications list, select **Kronos**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="20123-219">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="20123-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="20123-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="20123-221">Click **Add** button.</span></span> <span data-ttu-id="20123-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20123-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="20123-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="20123-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="20123-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20123-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20123-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20123-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="20123-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="20123-227">Testing single sign-on</span></span>

<span data-ttu-id="20123-228">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="20123-228">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="20123-229">Hello Kronos hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Kronos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="20123-229">When you click hello Kronos tile in hello Access Panel, you should get automatically signed-on tooyour Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20123-230">További források</span><span class="sxs-lookup"><span data-stu-id="20123-230">Additional resources</span></span>

* [<span data-ttu-id="20123-231">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="20123-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20123-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="20123-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

