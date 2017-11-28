---
title: "Oktatóanyag: Azure Active Directoryval integrált ICIMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ICIMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 72dbd649-e4b1-4d72-ad76-636d84922596
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 3fa970f008e64e84b0a1280f691f0181851b757c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icims"></a><span data-ttu-id="5165f-103">Oktatóanyag: Azure Active Directoryval integrált ICIMS</span><span class="sxs-lookup"><span data-stu-id="5165f-103">Tutorial: Azure Active Directory integration with ICIMS</span></span>

<span data-ttu-id="5165f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ICIMS az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5165f-104">In this tutorial, you learn how toointegrate ICIMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5165f-105">ICIMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5165f-105">Integrating ICIMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5165f-106">Megadhatja a hozzáférés tooICIMS rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5165f-106">You can control in Azure AD who has access tooICIMS</span></span>
- <span data-ttu-id="5165f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooICIMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5165f-107">You can enable your users tooautomatically get signed-on tooICIMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5165f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5165f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5165f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5165f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5165f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5165f-110">Prerequisites</span></span>

<span data-ttu-id="5165f-111">az Azure AD integrálása ICIMS tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="5165f-111">tooconfigure Azure AD integration with ICIMS, you need hello following items:</span></span>

- <span data-ttu-id="5165f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5165f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5165f-113">Egy ICIMS egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5165f-113">An ICIMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5165f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5165f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5165f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5165f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5165f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5165f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5165f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5165f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5165f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5165f-118">Scenario description</span></span>
<span data-ttu-id="5165f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5165f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5165f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5165f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5165f-121">Hello gyűjteményből ICIMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5165f-121">Adding ICIMS from hello gallery</span></span>
2. <span data-ttu-id="5165f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5165f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icims-from-hello-gallery"></a><span data-ttu-id="5165f-123">Hello gyűjteményből ICIMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5165f-123">Adding ICIMS from hello gallery</span></span>
<span data-ttu-id="5165f-124">tooconfigure hello integrációja ICIMS az Azure AD-be, meg kell tooadd ICIMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5165f-124">tooconfigure hello integration of ICIMS into Azure AD, you need tooadd ICIMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5165f-125">**tooadd ICIMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5165f-125">**tooadd ICIMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5165f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5165f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="5165f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5165f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5165f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5165f-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="5165f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5165f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="5165f-133">Hello keresési mezőbe, írja be a **ICIMS**, jelölje be **ICIMS** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5165f-133">In hello search box, type **ICIMS**, select **ICIMS** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában ICIMS](./media/active-directory-saas-icims-tutorial/tutorial_icims_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5165f-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5165f-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="5165f-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ICIMS.</span><span class="sxs-lookup"><span data-stu-id="5165f-136">In this section, you configure and test Azure AD single sign-on with ICIMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5165f-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ICIMS tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5165f-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ICIMS is tooa user in Azure AD.</span></span> <span data-ttu-id="5165f-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ICIMS közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5165f-138">In other words, a link relationship between an Azure AD user and hello related user in ICIMS needs toobe established.</span></span>

<span data-ttu-id="5165f-139">ICIMS, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5165f-139">In ICIMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5165f-140">tooconfigure és az Azure AD az egyszeri bejelentkezés ICIMS-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5165f-140">tooconfigure and test Azure AD single sign-on with ICIMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5165f-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5165f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5165f-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5165f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5165f-143">**[Hozzon létre egy ICIMS tesztfelhasználó](#create-an-icims-test-user)**  -toohave egy megfelelője a Britta Simon a ICIMS, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5165f-143">**[Create an ICIMS test user](#create-an-icims-test-user)** - toohave a counterpart of Britta Simon in ICIMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5165f-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5165f-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5165f-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5165f-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5165f-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5165f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5165f-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ICIMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5165f-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ICIMS application.</span></span>

<span data-ttu-id="5165f-148">**az Azure AD tooconfigure egyszeri bejelentkezést a ICIMS, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5165f-148">**tooconfigure Azure AD single sign-on with ICIMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="5165f-149">Az Azure portál, a hello hello **ICIMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5165f-149">In hello Azure portal, on hello **ICIMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5165f-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5165f-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-icims-tutorial/tutorial_icims_samlbase.png)

3. <span data-ttu-id="5165f-153">A hello **ICIMS tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5165f-153">On hello **ICIMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk ICIMS tartomány és az URL-címek](./media/active-directory-saas-icims-tutorial/tutorial_icims_url.png)

    <span data-ttu-id="5165f-155">a.</span><span class="sxs-lookup"><span data-stu-id="5165f-155">a.</span></span> <span data-ttu-id="5165f-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="5165f-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant name>.icims.com`</span></span>

    <span data-ttu-id="5165f-157">b.</span><span class="sxs-lookup"><span data-stu-id="5165f-157">b.</span></span> <span data-ttu-id="5165f-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="5165f-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant name>.icims.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5165f-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="5165f-159">These values are not real.</span></span> <span data-ttu-id="5165f-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="5165f-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5165f-161">Ügyfél [ICIMS ügyfél-támogatási csoport](https://www.icims.com/contact-us) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5165f-161">Contact [ICIMS Client support team](https://www.icims.com/contact-us) tooget these values.</span></span> 
 
4. <span data-ttu-id="5165f-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5165f-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-icims-tutorial/tutorial_icims_certificate.png) 

5. <span data-ttu-id="5165f-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5165f-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-icims-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5165f-166">A hello **ICIMS konfigurációs** kattintson **konfigurálása ICIMS** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5165f-166">On hello **ICIMS Configuration** section, click **Configure ICIMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5165f-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5165f-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ICIMS konfiguráció](./media/active-directory-saas-icims-tutorial/tutorial_icims_configure.png) 

7. <span data-ttu-id="5165f-169">tooconfigure egyszeri bejelentkezést a **ICIMS** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl [ICIMS támogatási csoport](https://www.icims.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="5165f-169">tooconfigure single sign-on on **ICIMS** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[ICIMS support team](https://www.icims.com/contact-us).</span></span> <span data-ttu-id="5165f-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="5165f-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5165f-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="5165f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5165f-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="5165f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5165f-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5165f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5165f-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5165f-174">Create an Azure AD test user</span></span>
<span data-ttu-id="5165f-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="5165f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="5165f-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5165f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5165f-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5165f-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-icims-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5165f-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5165f-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-icims-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5165f-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5165f-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-icims-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5165f-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5165f-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-icims-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5165f-186">a.</span><span class="sxs-lookup"><span data-stu-id="5165f-186">a.</span></span> <span data-ttu-id="5165f-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5165f-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5165f-188">b.</span><span class="sxs-lookup"><span data-stu-id="5165f-188">b.</span></span> <span data-ttu-id="5165f-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5165f-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5165f-190">c.</span><span class="sxs-lookup"><span data-stu-id="5165f-190">c.</span></span> <span data-ttu-id="5165f-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5165f-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5165f-192">d.</span><span class="sxs-lookup"><span data-stu-id="5165f-192">d.</span></span> <span data-ttu-id="5165f-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5165f-193">Click **Create**.</span></span>
 
### <a name="create-an-icims-test-user"></a><span data-ttu-id="5165f-194">Hozzon létre egy ICIMS tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="5165f-194">Create an ICIMS test user</span></span>

<span data-ttu-id="5165f-195">hello ebben a szakaszban célja toocreate ICIMS Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="5165f-195">hello objective of this section is toocreate a user called Britta Simon in ICIMS.</span></span> <span data-ttu-id="5165f-196">Együttműködve [ICIMS támogatási csoport](https://www.icims.com/contact-us) tooadd hello felhasználók hello ICIMS fiók.</span><span class="sxs-lookup"><span data-stu-id="5165f-196">Work with [ICIMS support team](https://www.icims.com/contact-us) tooadd hello users in hello ICIMS account.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5165f-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="5165f-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5165f-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooICIMS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5165f-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooICIMS.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="5165f-200">**tooassign Britta Simon tooICIMS, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5165f-200">**tooassign Britta Simon tooICIMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="5165f-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5165f-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5165f-203">Hello alkalmazások listában válassza ki a **ICIMS**.</span><span class="sxs-lookup"><span data-stu-id="5165f-203">In hello applications list, select **ICIMS**.</span></span>

    ![hello ICIMS hivatkozásra hello alkalmazások listája](./media/active-directory-saas-icims-tutorial/tutorial_icims_app.png) 

3. <span data-ttu-id="5165f-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5165f-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="5165f-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5165f-207">Click **Add** button.</span></span> <span data-ttu-id="5165f-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5165f-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="5165f-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5165f-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5165f-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5165f-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5165f-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5165f-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5165f-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5165f-213">Test single sign-on</span></span>

<span data-ttu-id="5165f-214">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="5165f-214">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="5165f-215">Hello ICIMS hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ICIMS alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5165f-215">When you click hello ICIMS tile in hello Access Panel, you should get automatically signed-on tooyour ICIMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5165f-216">További források</span><span class="sxs-lookup"><span data-stu-id="5165f-216">Additional resources</span></span>

* [<span data-ttu-id="5165f-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5165f-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5165f-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5165f-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icims-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icims-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icims-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icims-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icims-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icims-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icims-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icims-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icims-tutorial/tutorial_general_203.png

