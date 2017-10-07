---
title: "Oktatóanyag: Azure Active Directoryval integrált Bonusly |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Bonusly között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 60ad06c028463af81a7901ab321c4ae9346798ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a><span data-ttu-id="68070-103">Oktatóanyag: Azure Active Directoryval integrált Bonusly</span><span class="sxs-lookup"><span data-stu-id="68070-103">Tutorial: Azure Active Directory integration with Bonusly</span></span>

<span data-ttu-id="68070-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Bonusly az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="68070-104">In this tutorial, you learn how toointegrate Bonusly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="68070-105">Bonusly integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="68070-105">Integrating Bonusly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="68070-106">Megadhatja a hozzáférés tooBonusly rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="68070-106">You can control in Azure AD who has access tooBonusly</span></span>
- <span data-ttu-id="68070-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBonusly (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="68070-107">You can enable your users tooautomatically get signed-on tooBonusly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="68070-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="68070-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="68070-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="68070-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68070-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="68070-110">Prerequisites</span></span>

<span data-ttu-id="68070-111">az Azure AD integrálása Bonusly tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="68070-111">tooconfigure Azure AD integration with Bonusly, you need hello following items:</span></span>

- <span data-ttu-id="68070-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="68070-112">An Azure AD subscription</span></span>
- <span data-ttu-id="68070-113">Egy Bonusly egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="68070-113">A Bonusly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="68070-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="68070-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="68070-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="68070-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="68070-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="68070-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="68070-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="68070-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="68070-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="68070-118">Scenario description</span></span>
<span data-ttu-id="68070-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="68070-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="68070-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="68070-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="68070-121">Hello gyűjteményből Bonusly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="68070-121">Adding Bonusly from hello gallery</span></span>
2. <span data-ttu-id="68070-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="68070-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bonusly-from-hello-gallery"></a><span data-ttu-id="68070-123">Hello gyűjteményből Bonusly hozzáadása</span><span class="sxs-lookup"><span data-stu-id="68070-123">Adding Bonusly from hello gallery</span></span>
<span data-ttu-id="68070-124">tooconfigure hello integrációja Bonusly az Azure AD-be, meg kell tooadd Bonusly hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="68070-124">tooconfigure hello integration of Bonusly into Azure AD, you need tooadd Bonusly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="68070-125">**tooadd Bonusly hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="68070-125">**tooadd Bonusly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="68070-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="68070-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="68070-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="68070-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="68070-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="68070-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="68070-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="68070-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="68070-133">Hello keresési mezőbe, írja be a **Bonusly**, jelölje be **Bonusly** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-133">In hello search box, type **Bonusly**, select **Bonusly** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Bonusly](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="68070-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="68070-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="68070-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Bonusly.</span><span class="sxs-lookup"><span data-stu-id="68070-136">In this section, you configure and test Azure AD single sign-on with Bonusly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="68070-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Bonusly tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="68070-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bonusly is tooa user in Azure AD.</span></span> <span data-ttu-id="68070-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bonusly közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="68070-138">In other words, a link relationship between an Azure AD user and hello related user in Bonusly needs toobe established.</span></span>

<span data-ttu-id="68070-139">Bonusly, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="68070-139">In Bonusly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="68070-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Bonusly-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="68070-140">tooconfigure and test Azure AD single sign-on with Bonusly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="68070-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="68070-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="68070-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="68070-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="68070-143">**[Bonusly tesztfelhasználó létrehozása](#create-a-bonusly-test-user)**  -toohave egy megfelelője a Britta Simon a Bonusly, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="68070-143">**[Create a Bonusly test user](#create-a-bonusly-test-user)** - toohave a counterpart of Britta Simon in Bonusly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="68070-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="68070-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="68070-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="68070-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="68070-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="68070-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="68070-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Bonusly alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="68070-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bonusly application.</span></span>

<span data-ttu-id="68070-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Bonusly, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="68070-148">**tooconfigure Azure AD single sign-on with Bonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="68070-149">Az Azure portál, a hello hello **Bonusly** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="68070-149">In hello Azure portal, on hello **Bonusly** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="68070-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="68070-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. <span data-ttu-id="68070-153">A hello **Bonusly tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="68070-153">On hello **Bonusly Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat bonusly tartomány- és URL-címek](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    <span data-ttu-id="68070-155">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://Bonus.ly/saml/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="68070-155">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://Bonus.ly/saml/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="68070-156">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="68070-156">hello value is not real.</span></span> <span data-ttu-id="68070-157">Frissítés hello értékének hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="68070-157">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="68070-158">Ügyfél [Bonusly támogatási csoport](https://Bonusly/contact) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="68070-158">Contact [Bonusly support team](https://Bonusly/contact) tooget hello value.</span></span>
 
4. <span data-ttu-id="68070-159">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="68070-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. <span data-ttu-id="68070-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="68070-163">A hello **Bonusly konfigurációs** kattintson **Bonusly konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="68070-163">On hello **Bonusly Configuration** section, click **Configure Bonusly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="68070-164">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="68070-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Bonusly konfiguráció](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. <span data-ttu-id="68070-166">Egy másik böngészőablakban, jelentkezzen be tooyour **Bonusly** bérlő.</span><span class="sxs-lookup"><span data-stu-id="68070-166">In a different browser window, log in tooyour **Bonusly** tenant.</span></span>

8. <span data-ttu-id="68070-167">Hello hello felső eszköztárán kattintson **beállítások**, majd válassza ki **integrációja és az alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="68070-167">In hello toolbar on hello top, click **Settings**, and then select **Integrations and apps**.</span></span>
   
    <span data-ttu-id="68070-168">![Bonusly közösségi szakasz](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="68070-168">![Bonusly Social Section](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span></span>
9. <span data-ttu-id="68070-169">A **egyszeri bejelentkezés**, jelölje be **SAML**.</span><span class="sxs-lookup"><span data-stu-id="68070-169">Under **Single Sign-On**, select **SAML**.</span></span>

10. <span data-ttu-id="68070-170">A hello **SAML** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="68070-170">On hello **SAML** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="68070-171">![Bonusly Saml párbeszédpanel lap](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="68070-171">![Bonusly Saml Dialog page](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span></span>
   
    <span data-ttu-id="68070-172">a.</span><span class="sxs-lookup"><span data-stu-id="68070-172">a.</span></span> <span data-ttu-id="68070-173">A hello **IdP SSO célként megadott URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="68070-173">In hello **IdP SSO target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="68070-174">b.</span><span class="sxs-lookup"><span data-stu-id="68070-174">b.</span></span> <span data-ttu-id="68070-175">A hello **IdP kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="68070-175">In hello **IdP Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="68070-176">c.</span><span class="sxs-lookup"><span data-stu-id="68070-176">c.</span></span> <span data-ttu-id="68070-177">A hello **IdP bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="68070-177">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="68070-178">d.</span><span class="sxs-lookup"><span data-stu-id="68070-178">d.</span></span> <span data-ttu-id="68070-179">Beillesztés a **ujjlenyomat** érték átkerül az Azure portálról hello **tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="68070-179">Paste the **Thumbprint** value copied from Azure portal into hello **Cert Fingerprint** textbox.</span></span>
   
11. <span data-ttu-id="68070-180">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-180">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="68070-181">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="68070-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="68070-182">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="68070-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="68070-183">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="68070-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="68070-184">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="68070-184">Create an Azure AD test user</span></span>
<span data-ttu-id="68070-185">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="68070-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="68070-187">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="68070-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="68070-188">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="68070-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="68070-190">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="68070-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="68070-192">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="68070-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="68070-194">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="68070-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="68070-196">a.</span><span class="sxs-lookup"><span data-stu-id="68070-196">a.</span></span> <span data-ttu-id="68070-197">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="68070-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="68070-198">b.</span><span class="sxs-lookup"><span data-stu-id="68070-198">b.</span></span> <span data-ttu-id="68070-199">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="68070-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="68070-200">c.</span><span class="sxs-lookup"><span data-stu-id="68070-200">c.</span></span> <span data-ttu-id="68070-201">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="68070-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="68070-202">d.</span><span class="sxs-lookup"><span data-stu-id="68070-202">d.</span></span> <span data-ttu-id="68070-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-203">Click **Create**.</span></span>
 
### <a name="create-a-bonusly-test-user"></a><span data-ttu-id="68070-204">Bonusly tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="68070-204">Create a Bonusly test user</span></span>

<span data-ttu-id="68070-205">A sorrend tooenable az Azure AD felhasználók toolog a tooBonusly azok ki kell építenie Bonusly be.</span><span class="sxs-lookup"><span data-stu-id="68070-205">In order tooenable Azure AD users toolog in tooBonusly, they must be provisioned into Bonusly.</span></span> <span data-ttu-id="68070-206">Bonusly hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="68070-206">In hello case of Bonusly, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="68070-207">Bármely más Bonusly felhasználói fiók létrehozása eszközök vagy Bonusly tooprovision AAD által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="68070-207">You can use any other Bonusly user account creation tools or APIs provided by Bonusly tooprovision AAD user accounts.</span></span>
>  

<span data-ttu-id="68070-208">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="68070-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="68070-209">A webböngésző ablakának tooyour Bonusly bérlői jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="68070-209">In a web browser window, log in tooyour Bonusly tenant.</span></span>

2. <span data-ttu-id="68070-210">Kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="68070-210">Click **Settings**.</span></span>
 
    <span data-ttu-id="68070-211">![Beállítások](./media/active-directory-saas-bonus-tutorial/ic781041.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="68070-211">![Settings](./media/active-directory-saas-bonus-tutorial/ic781041.png "Settings")</span></span>

3. <span data-ttu-id="68070-212">Kattintson a hello **felhasználók és a prémium** fülre.</span><span class="sxs-lookup"><span data-stu-id="68070-212">Click hello **Users and bonuses** tab.</span></span>
   
    <span data-ttu-id="68070-213">![Felhasználók és a prémium](./media/active-directory-saas-bonus-tutorial/ic781042.png "felhasználók és a prémium")</span><span class="sxs-lookup"><span data-stu-id="68070-213">![Users and bonuses](./media/active-directory-saas-bonus-tutorial/ic781042.png "Users and bonuses")</span></span>

4. <span data-ttu-id="68070-214">Kattintson a **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="68070-214">Click **Manage Users**.</span></span>
   
    <span data-ttu-id="68070-215">![Felhasználók kezelése](./media/active-directory-saas-bonus-tutorial/ic781043.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="68070-215">![Manage Users](./media/active-directory-saas-bonus-tutorial/ic781043.png "Manage Users")</span></span>

5. <span data-ttu-id="68070-216">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="68070-216">Click **Add User**.</span></span>
   
    <span data-ttu-id="68070-217">![Felhasználó hozzáadása](./media/active-directory-saas-bonus-tutorial/ic781044.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="68070-217">![Add User](./media/active-directory-saas-bonus-tutorial/ic781044.png "Add User")</span></span>

6. <span data-ttu-id="68070-218">A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="68070-218">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="68070-219">![Felhasználó hozzáadása](./media/active-directory-saas-bonus-tutorial/ic781045.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="68070-219">![Add User](./media/active-directory-saas-bonus-tutorial/ic781045.png "Add User")</span></span>  

    <span data-ttu-id="68070-220">a.</span><span class="sxs-lookup"><span data-stu-id="68070-220">a.</span></span> <span data-ttu-id="68070-221">A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="68070-221">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="68070-222">b.</span><span class="sxs-lookup"><span data-stu-id="68070-222">b.</span></span> <span data-ttu-id="68070-223">A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="68070-223">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="68070-224">c.</span><span class="sxs-lookup"><span data-stu-id="68070-224">c.</span></span> <span data-ttu-id="68070-225">A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="68070-225">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="68070-226">d.</span><span class="sxs-lookup"><span data-stu-id="68070-226">d.</span></span> <span data-ttu-id="68070-227">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-227">Click **Save**.</span></span>
   
     >[!NOTE]
     ><span data-ttu-id="68070-228">az Azure AD fióktulajdonos hello kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="68070-228">hello Azure AD account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span>
     >  

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="68070-229">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="68070-229">Assign hello Azure AD test user</span></span>

<span data-ttu-id="68070-230">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBonusly megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="68070-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBonusly.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="68070-232">**tooassign Britta Simon tooBonusly, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="68070-232">**tooassign Britta Simon tooBonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="68070-233">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="68070-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="68070-235">Hello alkalmazások listában válassza ki a **Bonusly**.</span><span class="sxs-lookup"><span data-stu-id="68070-235">In hello applications list, select **Bonusly**.</span></span>

    ![hello Bonusly hello alkalmazások listáját a hivatkozás](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. <span data-ttu-id="68070-237">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="68070-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="68070-239">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="68070-239">Click **Add** button.</span></span> <span data-ttu-id="68070-240">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="68070-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="68070-242">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="68070-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="68070-243">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="68070-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="68070-244">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="68070-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="68070-245">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="68070-245">Test single sign-on</span></span>

<span data-ttu-id="68070-246">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="68070-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="68070-247">Hello Bonusly csempére kattintva a hozzáférési Panel hello, automatikusan bejelentkezett tooyour Bonusly alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="68070-247">When you click hello Bonusly tile in hello Access Panel, you should get automatically signed-on tooyour Bonusly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68070-248">További források</span><span class="sxs-lookup"><span data-stu-id="68070-248">Additional resources</span></span>

* [<span data-ttu-id="68070-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="68070-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="68070-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="68070-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

