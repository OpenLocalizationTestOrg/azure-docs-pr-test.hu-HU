---
title: "Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ScaleX vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="d3170-103">Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati</span><span class="sxs-lookup"><span data-stu-id="d3170-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="d3170-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ScaleX vállalati az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d3170-104">In this tutorial, you learn how toointegrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d3170-105">ScaleX vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d3170-105">Integrating ScaleX Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d3170-106">Megadhatja a vállalati hozzáférés tooScaleX rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d3170-106">You can control in Azure AD who has access tooScaleX Enterprise</span></span>
- <span data-ttu-id="d3170-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooScaleX vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d3170-107">You can enable your users tooautomatically get signed-on tooScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d3170-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d3170-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d3170-109">Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:.</span><span class="sxs-lookup"><span data-stu-id="d3170-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="d3170-110">Alkalmazás-hozzáférés és egyszeri bejelentkezés az [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d3170-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3170-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d3170-111">Prerequisites</span></span>

<span data-ttu-id="d3170-112">az Azure AD integrálása a ScaleX vállalati tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d3170-112">tooconfigure Azure AD integration with ScaleX Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="d3170-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d3170-113">An Azure AD subscription</span></span>
- <span data-ttu-id="d3170-114">Egy ScaleX vállalati egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d3170-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d3170-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d3170-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d3170-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d3170-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d3170-117">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d3170-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d3170-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3170-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d3170-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d3170-119">Scenario description</span></span>
<span data-ttu-id="d3170-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d3170-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d3170-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d3170-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d3170-122">Hello gyűjteményből ScaleX vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d3170-122">Adding ScaleX Enterprise from hello gallery</span></span>
2. <span data-ttu-id="d3170-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d3170-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-hello-gallery"></a><span data-ttu-id="d3170-124">Hello gyűjteményből ScaleX vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d3170-124">Adding ScaleX Enterprise from hello gallery</span></span>
<span data-ttu-id="d3170-125">tooconfigure hello integrálást ScaleX Enterprise tooAzure AD, meg kell tooadd ScaleX vállalati hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d3170-125">tooconfigure hello integration of ScaleX Enterprise in tooAzure AD, you need tooadd ScaleX Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d3170-126">**tooadd ScaleX vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d3170-126">**tooadd ScaleX Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3170-127">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d3170-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d3170-129">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d3170-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d3170-130">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d3170-130">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d3170-132">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d3170-132">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d3170-134">Hello keresési mezőbe, írja be a **ScaleX vállalati**.</span><span class="sxs-lookup"><span data-stu-id="d3170-134">In hello search box, type **ScaleX Enterprise**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="d3170-136">A hello eredmények panelen válassza ki a **ScaleX vállalati**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d3170-136">In hello results panel, select **ScaleX Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d3170-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d3170-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d3170-139">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a ScaleX vállalati "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="d3170-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d3170-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói ScaleX vállalati tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d3170-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScaleX Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="d3170-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello ScaleX vállalat közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d3170-141">In other words, a link relationship between an Azure AD user and hello related user in ScaleX Enterprise needs toobe established.</span></span>

<span data-ttu-id="d3170-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ScaleX vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="d3170-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="d3170-143">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a ScaleX vállalati, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d3170-143">tooconfigure and test Azure AD single sign-on with ScaleX Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d3170-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d3170-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d3170-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3170-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d3170-146">**[ScaleX vállalati tesztfelhasználó létrehozása](#creating-a-scalex-enterprise-test-user)**  -toohave egy megfelelője a Britta Simon ScaleX vállalat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d3170-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - toohave a counterpart of Britta Simon in ScaleX Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d3170-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d3170-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d3170-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d3170-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d3170-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d3170-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d3170-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ScaleX vállalati alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d3170-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="d3170-151">**az Azure AD tooconfigure egyszeri bejelentkezést a ScaleX vállalati, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d3170-151">**tooconfigure Azure AD single sign-on with ScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3170-152">Az Azure portál, a hello hello **ScaleX vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d3170-152">In hello Azure portal, on hello **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d3170-154">A hello **egyszeri bejelentkezés** párbeszédpanelen, **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d3170-154">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="d3170-156">A hello **ScaleX vállalati tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="d3170-156">On hello **ScaleX Enterprise Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="d3170-158">a.</span><span class="sxs-lookup"><span data-stu-id="d3170-158">a.</span></span> <span data-ttu-id="d3170-159">A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="d3170-159">In hello **Identifier** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="d3170-160">b.</span><span class="sxs-lookup"><span data-stu-id="d3170-160">b.</span></span> <span data-ttu-id="d3170-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="d3170-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="d3170-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="d3170-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="d3170-164">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="d3170-164">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d3170-165">Ezek a hello valódi értékek nem.</span><span class="sxs-lookup"><span data-stu-id="d3170-165">These are not hello real values.</span></span> <span data-ttu-id="d3170-166">Ezek az értékek frissítése hello tényleges azonosító, a válasz URL-cím vagy a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="d3170-166">Update these values with hello actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="d3170-167">Ügyfél [ScaleX vállalati ügyfél-támogatási csoport](http://info.rescale.com/contact_sales) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d3170-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) tooget these values.</span></span> 

5. <span data-ttu-id="d3170-168">A ScaleX alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy toomodify egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="d3170-168">Your ScaleX application expects hello SAML assertions in a specific format, which requires you toomodify custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="d3170-169">Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzet tooopen hello egyéni attribútumok beállításait.</span><span class="sxs-lookup"><span data-stu-id="d3170-169">Click **View and edit all other user attributes** checkbox tooopen hello custom attributes settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="d3170-171">a.</span><span class="sxs-lookup"><span data-stu-id="d3170-171">a.</span></span> <span data-ttu-id="d3170-172">Kattintson jobb gombbal a hello attribútum **neve** , és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="d3170-172">Right click hello attribute **name** and click delete.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="d3170-174">b.</span><span class="sxs-lookup"><span data-stu-id="d3170-174">b.</span></span> <span data-ttu-id="d3170-175">Kattintson a **emailaddress** attribútum tooopen hello attribútum szerkesztése ablak.</span><span class="sxs-lookup"><span data-stu-id="d3170-175">Click **emailaddress** attribute tooopen hello Edit Attribute window.</span></span> <span data-ttu-id="d3170-176">Módosítsa az értéket **user.mail** túl**user.userprincipalname** kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="d3170-176">Change its value from **user.mail** too**user.userprincipalname** and click Ok.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="d3170-178">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d3170-178">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="d3170-180">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d3170-180">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="d3170-182">A hello **ScaleX vállalati konfiguráció** kattintson **ScaleX vállalati konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d3170-182">On hello **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d3170-183">Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d3170-183">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="d3170-185">tooconfigure egyszeri bejelentkezést a **ScaleX vállalati** oldalon, a bejelentkezési toohello ScaleX vállalati vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d3170-185">tooconfigure single sign-on on **ScaleX Enterprise** side, login toohello ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="d3170-186">Jobb felső hello hello menüre kattint, és válassza ki **Contoso felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d3170-186">Click hello menu in hello upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d3170-187">Contoso csak egy példa.</span><span class="sxs-lookup"><span data-stu-id="d3170-187">Contoso is just an example.</span></span> <span data-ttu-id="d3170-188">A tényleges vállalat neve legyen.</span><span class="sxs-lookup"><span data-stu-id="d3170-188">This should be your actual Company Name.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="d3170-190">Válassza ki **integrációja** hello felső menüre, majd válassza a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d3170-190">Select **Integrations** from hello top menu and select **Single Sign-On**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="d3170-192">Hello űrlapot kitöltve a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="d3170-192">Complete hello form as follows:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="d3170-194">a.</span><span class="sxs-lookup"><span data-stu-id="d3170-194">a.</span></span> <span data-ttu-id="d3170-195">Válassza ki **"Bármely felhasználó, aki hitelesítheti létrehozása egyszeri bejelentkezési modellel."**</span><span class="sxs-lookup"><span data-stu-id="d3170-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="d3170-196">b.</span><span class="sxs-lookup"><span data-stu-id="d3170-196">b.</span></span> <span data-ttu-id="d3170-197">**Szolgáltató saml**: hello érték beillesztése ***urn: oasis: nevek: tc: SAML:2.0:nameid-formátum: állandó***</span><span class="sxs-lookup"><span data-stu-id="d3170-197">**Service Provider saml**: Paste hello value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="d3170-198">c.</span><span class="sxs-lookup"><span data-stu-id="d3170-198">c.</span></span> <span data-ttu-id="d3170-199">**Az ACS-válasz identitásszolgáltató e-mail mező neve**: hello érték beillesztése`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="d3170-199">**Name of Identity Provider email field in ACS response**: Paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="d3170-200">d.</span><span class="sxs-lookup"><span data-stu-id="d3170-200">d.</span></span> <span data-ttu-id="d3170-201">**Identity Provider EntityDescriptor Entitásazonosító:** Beillesztés hello **SAML Entitásazonosító** hello Azure-portálon átmásolja értéket.</span><span class="sxs-lookup"><span data-stu-id="d3170-201">**Identity Provider EntityDescriptor Entity ID:** Paste hello **SAML Entity ID** value copied from hello Azure portal.</span></span>

    <span data-ttu-id="d3170-202">e.</span><span class="sxs-lookup"><span data-stu-id="d3170-202">e.</span></span> <span data-ttu-id="d3170-203">**Identity Provider SingleSignOnService URL-címe:** Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d3170-203">**Identity Provider SingleSignOnService URL:** Paste hello **SAML Single Sign-On Service URL** from hello Azure portal.</span></span>

    <span data-ttu-id="d3170-204">f.</span><span class="sxs-lookup"><span data-stu-id="d3170-204">f.</span></span> <span data-ttu-id="d3170-205">**Szolgáltató nyilvános X509 identitástanúsítvány:** nyitott hello X509 tanúsítvány le: hello Azure Jegyzettömb és a Beillesztés hello tartalmában ebben a mezőben.</span><span class="sxs-lookup"><span data-stu-id="d3170-205">**Identity Provider public X509 certificate:** Open hello X509 certificate downloaded from hello Azure in notepad and paste hello contents in this box.</span></span> <span data-ttu-id="d3170-206">Ellenőrizze, hogy nincsenek a nincs sortörések hello középső hello tanúsítvány tartalmának.</span><span class="sxs-lookup"><span data-stu-id="d3170-206">Ensure there are no line breaks in hello middle of hello certificate contents.</span></span>
    
    <span data-ttu-id="d3170-207">g.</span><span class="sxs-lookup"><span data-stu-id="d3170-207">g.</span></span> <span data-ttu-id="d3170-208">Ellenőrizze a következő checkboxes hello: **engedélyezve, a NameID titkosítására és a bejelentkezési AuthnRequests.**</span><span class="sxs-lookup"><span data-stu-id="d3170-208">Check hello following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="d3170-209">h.</span><span class="sxs-lookup"><span data-stu-id="d3170-209">h.</span></span> <span data-ttu-id="d3170-210">Kattintson a **egyszeri bejelentkezési beállítások** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="d3170-210">Click **Update SSO Settings** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="d3170-211">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d3170-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d3170-212">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d3170-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d3170-213">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d3170-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d3170-214">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3170-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="d3170-215">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d3170-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d3170-217">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d3170-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3170-218">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d3170-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d3170-220">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="d3170-220">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d3170-222">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d3170-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d3170-224">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d3170-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d3170-226">a.</span><span class="sxs-lookup"><span data-stu-id="d3170-226">a.</span></span> <span data-ttu-id="d3170-227">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d3170-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d3170-228">b.</span><span class="sxs-lookup"><span data-stu-id="d3170-228">b.</span></span> <span data-ttu-id="d3170-229">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d3170-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d3170-230">c.</span><span class="sxs-lookup"><span data-stu-id="d3170-230">c.</span></span> <span data-ttu-id="d3170-231">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d3170-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d3170-232">d.</span><span class="sxs-lookup"><span data-stu-id="d3170-232">d.</span></span> <span data-ttu-id="d3170-233">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d3170-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="d3170-234">ScaleX vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3170-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="d3170-235">az Azure AD tooenable felhasználók toolog tooScaleX vállalati, a azok ki kell építenie a vállalati tooScaleX.</span><span class="sxs-lookup"><span data-stu-id="d3170-235">tooenable Azure AD users toolog in tooScaleX Enterprise, they must be provisioned in tooScaleX Enterprise.</span></span> <span data-ttu-id="d3170-236">ScaleX vállalati hello esetben egy automatikus feladat, és nincs manuális lépések szükségesek.</span><span class="sxs-lookup"><span data-stu-id="d3170-236">In hello case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="d3170-237">Bárki, aki képes sikeresen hitelesíteni egyszeri bejelentkezési hitelesítő adatokkal rendelkező automatikusan építi ki a hello ScaleX oldalán.</span><span class="sxs-lookup"><span data-stu-id="d3170-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on hello ScaleX side.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d3170-238">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d3170-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d3170-239">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés felhasználói hozzáférés tooScaleX vállalati megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d3170-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting user access tooScaleX Enterprise.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d3170-241">**tooassign Britta Simon tooScaleX vállalati, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d3170-241">**tooassign Britta Simon tooScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3170-242">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d3170-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d3170-244">Hello alkalmazások listában válassza ki a **ScaleX vállalati**.</span><span class="sxs-lookup"><span data-stu-id="d3170-244">In hello applications list, select **ScaleX Enterprise**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="d3170-246">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d3170-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d3170-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d3170-248">Click **Add** button.</span></span> <span data-ttu-id="d3170-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d3170-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d3170-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d3170-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d3170-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d3170-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d3170-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d3170-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="d3170-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d3170-254">Testing single sign-on</span></span>

<span data-ttu-id="d3170-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d3170-255">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d3170-256">Kattintson a hozzáférési Panel hello csempe ScaleX vállalati hello automatikusan bejelentkezett tooyour ScaleX vállalati alkalmazás fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="d3170-256">Click hello ScaleX Enterprise tile in hello Access Panel, you will get automatically signed-on tooyour ScaleX Enterprise application.</span></span> <span data-ttu-id="d3170-257">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="d3170-257">For more information about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d3170-258">További források</span><span class="sxs-lookup"><span data-stu-id="d3170-258">Additional resources</span></span>

* [<span data-ttu-id="d3170-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d3170-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d3170-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d3170-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

