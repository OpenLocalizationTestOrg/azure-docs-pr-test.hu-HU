---
title: "Oktatóanyag: Azure Active Directoryval integrált Voyance |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Voyance között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: e2cb9eb6b20e8611a9f6e8529b7c85a8d86ec3e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a><span data-ttu-id="61e3e-103">Oktatóanyag: Azure Active Directoryval integrált Voyance</span><span class="sxs-lookup"><span data-stu-id="61e3e-103">Tutorial: Azure Active Directory integration with Voyance</span></span>

<span data-ttu-id="61e3e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Voyance az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="61e3e-104">In this tutorial, you learn how toointegrate Voyance with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="61e3e-105">Voyance integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="61e3e-105">Integrating Voyance with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="61e3e-106">Megadhatja a hozzáférés tooVoyance rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="61e3e-106">You can control in Azure AD who has access tooVoyance</span></span>
- <span data-ttu-id="61e3e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVoyance (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="61e3e-107">You can enable your users tooautomatically get signed-on tooVoyance (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="61e3e-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="61e3e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="61e3e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="61e3e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61e3e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="61e3e-110">Prerequisites</span></span>

<span data-ttu-id="61e3e-111">az Azure AD integrálása Voyance tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="61e3e-111">tooconfigure Azure AD integration with Voyance, you need hello following items:</span></span>

- <span data-ttu-id="61e3e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="61e3e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="61e3e-113">Egy Voyance egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="61e3e-113">A Voyance single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="61e3e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="61e3e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="61e3e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="61e3e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="61e3e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="61e3e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="61e3e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="61e3e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="61e3e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="61e3e-118">Scenario description</span></span>
<span data-ttu-id="61e3e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="61e3e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="61e3e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="61e3e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="61e3e-121">Hello gyűjteményből Voyance hozzáadása</span><span class="sxs-lookup"><span data-stu-id="61e3e-121">Adding Voyance from hello gallery</span></span>
2. <span data-ttu-id="61e3e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="61e3e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-voyance-from-hello-gallery"></a><span data-ttu-id="61e3e-123">Hello gyűjteményből Voyance hozzáadása</span><span class="sxs-lookup"><span data-stu-id="61e3e-123">Adding Voyance from hello gallery</span></span>
<span data-ttu-id="61e3e-124">tooconfigure hello integrációja Voyance az Azure AD-be, meg kell tooadd Voyance hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="61e3e-124">tooconfigure hello integration of Voyance into Azure AD, you need tooadd Voyance from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="61e3e-125">**tooadd Voyance hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="61e3e-125">**tooadd Voyance from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="61e3e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="61e3e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="61e3e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="61e3e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="61e3e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="61e3e-133">Hello keresési mezőbe, írja be a **Voyance**, jelölje be **Voyance** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-133">In hello search box, type **Voyance**, select  **Voyance**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Voyance](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="61e3e-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="61e3e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="61e3e-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Voyance.</span><span class="sxs-lookup"><span data-stu-id="61e3e-136">In this section, you configure and test Azure AD single sign-on with Voyance based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="61e3e-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Voyance tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="61e3e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Voyance is tooa user in Azure AD.</span></span> <span data-ttu-id="61e3e-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Voyance közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="61e3e-138">In other words, a link relationship between an Azure AD user and hello related user in Voyance needs toobe established.</span></span>

<span data-ttu-id="61e3e-139">Voyance, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="61e3e-139">In Voyance, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="61e3e-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Voyance-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="61e3e-140">tooconfigure and test Azure AD single sign-on with Voyance, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="61e3e-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="61e3e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="61e3e-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61e3e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="61e3e-143">**[Voyance tesztfelhasználó létrehozása](#create-a-voyance-test-user)**  -toohave egy megfelelője a Britta Simon a Voyance, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="61e3e-143">**[Create a Voyance test user](#create-a-voyance-test-user)** - toohave a counterpart of Britta Simon in Voyance that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="61e3e-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="61e3e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="61e3e-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="61e3e-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="61e3e-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="61e3e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="61e3e-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Voyance alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="61e3e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Voyance application.</span></span>

<span data-ttu-id="61e3e-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Voyance, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="61e3e-148">**tooconfigure Azure AD single sign-on with Voyance, perform hello following steps:**</span></span>

1. <span data-ttu-id="61e3e-149">Az Azure portál, a hello hello **Voyance** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-149">In hello Azure portal, on hello **Voyance** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="61e3e-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="61e3e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_samlbase.png)

3. <span data-ttu-id="61e3e-153">A hello **Voyance tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="61e3e-153">On hello **Voyance Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Voyance tartomány és az URL-címeket az egyszeri bejelentkezés IDP adatai](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url1.png)

    <span data-ttu-id="61e3e-155">a.</span><span class="sxs-lookup"><span data-stu-id="61e3e-155">a.</span></span> <span data-ttu-id="61e3e-156">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.nyansa.com`</span><span class="sxs-lookup"><span data-stu-id="61e3e-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com`</span></span>

    <span data-ttu-id="61e3e-157">b.</span><span class="sxs-lookup"><span data-stu-id="61e3e-157">b.</span></span> <span data-ttu-id="61e3e-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.nyansa.com/saml/create/`</span><span class="sxs-lookup"><span data-stu-id="61e3e-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com/saml/create/`</span></span>

4. <span data-ttu-id="61e3e-159">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="61e3e-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Voyance tartomány és az URL-címeket az egyszeri bejelentkezés SP adatai](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url2.png)

    <span data-ttu-id="61e3e-161">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.nyansa.com/`</span><span class="sxs-lookup"><span data-stu-id="61e3e-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="61e3e-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="61e3e-162">These values are not real.</span></span> <span data-ttu-id="61e3e-163">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="61e3e-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="61e3e-164">Ügyfél [Voyance ügyfél-támogatási csoport](mailto:support@nyansa.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="61e3e-164">Contact [Voyance Client support team](mailto:support@nyansa.com) tooget these values.</span></span> 

5. <span data-ttu-id="61e3e-165">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="61e3e-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_certificate.png) 

6. <span data-ttu-id="61e3e-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-voyance-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="61e3e-169">A hello **Voyance konfigurációs** kattintson **konfigurálása Voyance** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="61e3e-169">On hello **Voyance Configuration** section, click **Configure Voyance** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="61e3e-170">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="61e3e-170">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Voyance konfiguráció](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_configure.png) 

8. <span data-ttu-id="61e3e-172">Egy másik webes böngészőablakban, bejelentkezés tooyour Voyance Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="61e3e-172">In a different web browser window, sign-on tooyour Voyance tenant as an administrator.</span></span>

9. <span data-ttu-id="61e3e-173">Nyissa meg toohello jobb felső sarkában hello navigációs sávján, majd kattintson a hello legördülő lista, amely szerint "**Soft egyetemi**".</span><span class="sxs-lookup"><span data-stu-id="61e3e-173">Go toohello top right corner of hello navigation bar and click on hello drop-down that says "**Acme University**".</span></span>
    
    ![Alkalmazás ügyféloldali Soft egyetemi egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

10. <span data-ttu-id="61e3e-175">Kattintson a "**rendszergazdai beállítások**".</span><span class="sxs-lookup"><span data-stu-id="61e3e-175">Click "**Admin Settings**".</span></span>

    ![Alkalmazás ügyféloldali felügyeleti beállítások az egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

11. <span data-ttu-id="61e3e-177">Kattintson a "**felhasználói hozzáférés**" lapon.</span><span class="sxs-lookup"><span data-stu-id="61e3e-177">Click "**User Access**" tab.</span></span>

    ![Alkalmazás ügyféloldali felhasználói hozzáférést az egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

12. <span data-ttu-id="61e3e-179">Kattintson a hello "**egyszeri bejelentkezés le van tiltva**" gomb tooconfigure az Azure AD mint az IdP az SAML 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="61e3e-179">Click hello "**SSO is disabled**" button tooconfigure Azure AD as an IdP using SAML 2.0.</span></span>

    ![Konfigurálása egyszeri bejelentkezés az alkalmazás ügyféloldali SSO le van tiltva gomb](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

13. <span data-ttu-id="61e3e-181">Nyissa meg túl**SAML v2** szakaszt, és hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="61e3e-181">Go too**SAML v2** section and perform below steps:</span></span>

    ![Alkalmazás oldalán SAML-alapú egyszeri bejelentkezés konfigurálása v2](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
    
    <span data-ttu-id="61e3e-183">a.</span><span class="sxs-lookup"><span data-stu-id="61e3e-183">a.</span></span> <span data-ttu-id="61e3e-184">Válassza ki **engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-184">Select **Enabled**.</span></span>
    
    <span data-ttu-id="61e3e-185">b.</span><span class="sxs-lookup"><span data-stu-id="61e3e-185">b.</span></span> <span data-ttu-id="61e3e-186">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **IdP bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="61e3e-186">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal Into hello **IdP Login URL** textbox.</span></span>

    <span data-ttu-id="61e3e-187">c.</span><span class="sxs-lookup"><span data-stu-id="61e3e-187">c.</span></span> <span data-ttu-id="61e3e-188">Nyissa meg a letöltött Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **IdP Cert** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="61e3e-188">Open your downloaded Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Cert** textbox.</span></span>
    
    <span data-ttu-id="61e3e-189">d.</span><span class="sxs-lookup"><span data-stu-id="61e3e-189">d.</span></span> <span data-ttu-id="61e3e-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="61e3e-191">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="61e3e-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="61e3e-192">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="61e3e-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="61e3e-193">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="61e3e-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="61e3e-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="61e3e-194">Create an Azure AD test user</span></span>

<span data-ttu-id="61e3e-195">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="61e3e-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="61e3e-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="61e3e-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="61e3e-198">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-voyance-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="61e3e-200">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-voyance-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="61e3e-202">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61e3e-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="61e3e-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="61e3e-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="61e3e-206">a.</span><span class="sxs-lookup"><span data-stu-id="61e3e-206">a.</span></span> <span data-ttu-id="61e3e-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="61e3e-208">b.</span><span class="sxs-lookup"><span data-stu-id="61e3e-208">b.</span></span> <span data-ttu-id="61e3e-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="61e3e-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="61e3e-210">c.</span><span class="sxs-lookup"><span data-stu-id="61e3e-210">c.</span></span> <span data-ttu-id="61e3e-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="61e3e-212">d.</span><span class="sxs-lookup"><span data-stu-id="61e3e-212">d.</span></span> <span data-ttu-id="61e3e-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-213">Click **Create**.</span></span>
 
### <a name="create-a-voyance-test-user"></a><span data-ttu-id="61e3e-214">Voyance tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="61e3e-214">Create a Voyance test user</span></span>

<span data-ttu-id="61e3e-215">hello ebben a szakaszban célja toocreate Voyance Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="61e3e-215">hello objective of this section is toocreate a user called Britta Simon in Voyance.</span></span> <span data-ttu-id="61e3e-216">Voyance támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="61e3e-216">Voyance supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="61e3e-217">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="61e3e-217">There is no action item for you in this section.</span></span> <span data-ttu-id="61e3e-218">Új felhasználó jön létre egy kísérlet tooaccess Voyance során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="61e3e-218">A new user is created during an attempt tooaccess Voyance if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="61e3e-219">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact [Voyance támogatási csoport](maiLto:support@nyansa.com).</span><span class="sxs-lookup"><span data-stu-id="61e3e-219">If you need toocreate a user manually, you need toocontact [Voyance support team](maiLto:support@nyansa.com).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="61e3e-220">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="61e3e-220">Assign hello Azure AD test user</span></span>

<span data-ttu-id="61e3e-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooVoyance megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="61e3e-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVoyance.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="61e3e-223">**tooassign Britta Simon tooVoyance, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="61e3e-223">**tooassign Britta Simon tooVoyance, perform hello following steps:**</span></span>

1. <span data-ttu-id="61e3e-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="61e3e-226">Hello alkalmazások listában válassza ki a **Voyance**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-226">In hello applications list, select **Voyance**.</span></span>

    ![hello Voyance hivatkozásra hello alkalmazások listája](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_app.png) 

3. <span data-ttu-id="61e3e-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="61e3e-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="61e3e-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="61e3e-230">Click **Add** button.</span></span> <span data-ttu-id="61e3e-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61e3e-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="61e3e-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="61e3e-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="61e3e-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61e3e-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="61e3e-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="61e3e-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="61e3e-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="61e3e-236">Test single sign-on</span></span>

<span data-ttu-id="61e3e-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="61e3e-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="61e3e-238">Ha a hozzáférési Panel hello hello Voyance csempe gombra kattint, automatikusan bejelentkezett tooyour Voyance alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="61e3e-238">When you click hello Voyance tile in hello Access Panel, you should get automatically signed-on tooyour Voyance application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61e3e-239">További források</span><span class="sxs-lookup"><span data-stu-id="61e3e-239">Additional resources</span></span>

* [<span data-ttu-id="61e3e-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="61e3e-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="61e3e-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="61e3e-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png

