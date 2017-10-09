---
title: "Oktatóanyag: Azure Active Directoryval integrált PurelyHR |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PurelyHR között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: bdc1748ed650cff36b1ef7d7330dd2a17b3bc760
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a><span data-ttu-id="a7df8-103">Oktatóanyag: Azure Active Directoryval integrált PurelyHR</span><span class="sxs-lookup"><span data-stu-id="a7df8-103">Tutorial: Azure Active Directory integration with PurelyHR</span></span>

<span data-ttu-id="a7df8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PurelyHR az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a7df8-104">In this tutorial, you learn how toointegrate PurelyHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7df8-105">PurelyHR integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a7df8-105">Integrating PurelyHR with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a7df8-106">Megadhatja a hozzáférés tooPurelyHR rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a7df8-106">You can control in Azure AD who has access tooPurelyHR</span></span>
- <span data-ttu-id="a7df8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPurelyHR (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a7df8-107">You can enable your users tooautomatically get signed-on tooPurelyHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a7df8-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a7df8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a7df8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7df8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7df8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7df8-110">Prerequisites</span></span>

<span data-ttu-id="a7df8-111">az Azure AD integrálása PurelyHR tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a7df8-111">tooconfigure Azure AD integration with PurelyHR, you need hello following items:</span></span>

- <span data-ttu-id="a7df8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a7df8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7df8-113">Egy PurelyHR egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a7df8-113">A PurelyHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7df8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a7df8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7df8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a7df8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7df8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7df8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a7df8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7df8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7df8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a7df8-118">Scenario description</span></span>
<span data-ttu-id="a7df8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a7df8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7df8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a7df8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7df8-121">Hello gyűjteményből PurelyHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7df8-121">Adding PurelyHR from hello gallery</span></span>
2. <span data-ttu-id="a7df8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a7df8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-purelyhr-from-hello-gallery"></a><span data-ttu-id="a7df8-123">Hello gyűjteményből PurelyHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7df8-123">Adding PurelyHR from hello gallery</span></span>
<span data-ttu-id="a7df8-124">tooconfigure hello integrációja PurelyHR az Azure AD-be, meg kell tooadd PurelyHR hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a7df8-124">tooconfigure hello integration of PurelyHR into Azure AD, you need tooadd PurelyHR from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a7df8-125">**tooadd PurelyHR hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a7df8-125">**tooadd PurelyHR from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7df8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a7df8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a7df8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a7df8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a7df8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a7df8-133">Hello keresési mezőbe, írja be a **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-133">In hello search box, type **PurelyHR**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. <span data-ttu-id="a7df8-135">A hello eredmények panelen válassza ki a **PurelyHR**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-135">In hello results panel, select **PurelyHR**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a7df8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a7df8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a7df8-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján PurelyHR</span><span class="sxs-lookup"><span data-stu-id="a7df8-138">In this section, you configure and test Azure AD single sign-on with PurelyHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a7df8-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PurelyHR tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a7df8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PurelyHR is tooa user in Azure AD.</span></span> <span data-ttu-id="a7df8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PurelyHR közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a7df8-140">In other words, a link relationship between an Azure AD user and hello related user in PurelyHR needs toobe established.</span></span>

<span data-ttu-id="a7df8-141">PurelyHR, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a7df8-141">In PurelyHR, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a7df8-142">tooconfigure és az Azure AD az egyszeri bejelentkezés PurelyHR-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a7df8-142">tooconfigure and test Azure AD single sign-on with PurelyHR, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a7df8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a7df8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a7df8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7df8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7df8-145">**[PurelyHR tesztfelhasználó létrehozása](#creating-a-purelyhr-test-user)**  -toohave egy megfelelője a Britta Simon a PurelyHR, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a7df8-145">**[Creating a PurelyHR test user](#creating-a-purelyhr-test-user)** - toohave a counterpart of Britta Simon in PurelyHR that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a7df8-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a7df8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7df8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a7df8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a7df8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a7df8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a7df8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PurelyHR alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a7df8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PurelyHR application.</span></span>

<span data-ttu-id="a7df8-150">**az Azure AD tooconfigure egyszeri bejelentkezést a PurelyHR, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a7df8-150">**tooconfigure Azure AD single sign-on with PurelyHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7df8-151">Az Azure portál, a hello hello **PurelyHR** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-151">In hello Azure portal, on hello **PurelyHR** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a7df8-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a7df8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. <span data-ttu-id="a7df8-155">A hello **PurelyHR tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a7df8-155">On hello **PurelyHR Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    <span data-ttu-id="a7df8-157">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyID>.purelyhr.com/sso-consume`</span><span class="sxs-lookup"><span data-stu-id="a7df8-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyID>.purelyhr.com/sso-consume`</span></span>

4. <span data-ttu-id="a7df8-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a7df8-158">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    <span data-ttu-id="a7df8-160">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<companyID>.purelyhr.com/sso-initiate`</span><span class="sxs-lookup"><span data-stu-id="a7df8-160">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<companyID>.purelyhr.com/sso-initiate`</span></span>
     
    > [!NOTE]
    > <span data-ttu-id="a7df8-161">Ezek az értékek nincsenek hello valós.</span><span class="sxs-lookup"><span data-stu-id="a7df8-161">These values are not hello real.</span></span> <span data-ttu-id="a7df8-162">Ezek az értékek frissítése hello tényleges válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="a7df8-162">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="a7df8-163">Ügyfél [PurelyHR ügyfél-támogatási csoport](http://support.purelyhr.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a7df8-163">Contact [PurelyHR Client support team](http://support.purelyhr.com/) tooget these values.</span></span> 

5. <span data-ttu-id="a7df8-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a7df8-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. <span data-ttu-id="a7df8-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="a7df8-168">A hello **PurelyHR konfigurációs** kattintson **konfigurálása PurelyHR** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a7df8-168">On hello **PurelyHR Configuration** section, click **Configure PurelyHR** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a7df8-169">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a7df8-169">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. <span data-ttu-id="a7df8-171">tooconfigure egyszeri bejelentkezést a **PurelyHR** oldalon, a bejelentkezési tootheir webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a7df8-171">tooconfigure single sign-on on **PurelyHR** side, login tootheir website as an administrator.</span></span>

9. <span data-ttu-id="a7df8-172">Nyissa meg hello **irányítópult** hello kapcsolók hello eszköztár, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-172">Open hello **Dashboard** from hello options in hello toolbar and click **SSO Settings**.</span></span>

10. <span data-ttu-id="a7df8-173">Beillesztés hello értékek hello mezőiben lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="a7df8-173">Paste hello values in hello boxes as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    <span data-ttu-id="a7df8-175">a.</span><span class="sxs-lookup"><span data-stu-id="a7df8-175">a.</span></span> <span data-ttu-id="a7df8-176">Nyissa meg hello **Certificate(Bas64)** letöltésének hello Azure portálra az Jegyzettömb, és másolja hello tanúsítvány érték.</span><span class="sxs-lookup"><span data-stu-id="a7df8-176">Open hello **Certificate(Bas64)** downloaded from hello Azure portal in notepad and copy hello certificate value.</span></span> <span data-ttu-id="a7df8-177">Beillesztés hello értéket másol a hello **X.509 tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a7df8-177">Paste hello copied value into hello **X.509 Certificate** box.</span></span>

    <span data-ttu-id="a7df8-178">b.</span><span class="sxs-lookup"><span data-stu-id="a7df8-178">b.</span></span> <span data-ttu-id="a7df8-179">A hello **Idp kiállítójának URL-címe** mezőbe illessze be a hello **SAML Entitásazonosító** átmásolva hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a7df8-179">In hello **Idp Issuer URL** box, paste hello **SAML Entity ID** copied from hello Azure portal.</span></span>

    <span data-ttu-id="a7df8-180">c.</span><span class="sxs-lookup"><span data-stu-id="a7df8-180">c.</span></span> <span data-ttu-id="a7df8-181">A hello **Idp végponti URL-cím** mezőbe illessze be a hello **SAML-alapú egyszeri bejelentkezési URL-címe** átmásolva hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a7df8-181">In hello **Idp Endpoint URL** box, paste hello **SAML Single Sign-On Service URL** copied from hello Azure portal.</span></span> 

    <span data-ttu-id="a7df8-182">d.</span><span class="sxs-lookup"><span data-stu-id="a7df8-182">d.</span></span> <span data-ttu-id="a7df8-183">Ellenőrizze a hello **automatikus létrehozása felhasználók** jelölőnégyzet tooenable automatikus felhasználói kiépítési PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="a7df8-183">Check hello **Auto-Create Users** checkbox tooenable automatic user provisioning in PurelyHR.</span></span>

    <span data-ttu-id="a7df8-184">e.</span><span class="sxs-lookup"><span data-stu-id="a7df8-184">e.</span></span> <span data-ttu-id="a7df8-185">Kattintson a **módosítások mentése** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="a7df8-185">Click **Save Changes** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="a7df8-186">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a7df8-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a7df8-187">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a7df8-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a7df8-188">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a7df8-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a7df8-189">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7df8-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="a7df8-190">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a7df8-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a7df8-192">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a7df8-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7df8-193">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a7df8-195">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a7df8-197">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7df8-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a7df8-199">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a7df8-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a7df8-201">a.</span><span class="sxs-lookup"><span data-stu-id="a7df8-201">a.</span></span> <span data-ttu-id="a7df8-202">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7df8-203">b.</span><span class="sxs-lookup"><span data-stu-id="a7df8-203">b.</span></span> <span data-ttu-id="a7df8-204">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a7df8-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a7df8-205">c.</span><span class="sxs-lookup"><span data-stu-id="a7df8-205">c.</span></span> <span data-ttu-id="a7df8-206">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a7df8-207">d.</span><span class="sxs-lookup"><span data-stu-id="a7df8-207">d.</span></span> <span data-ttu-id="a7df8-208">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-208">Click **Create**.</span></span>
 
### <a name="creating-a-purelyhr-test-user"></a><span data-ttu-id="a7df8-209">PurelyHR tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7df8-209">Creating a PurelyHR test user</span></span>

<span data-ttu-id="a7df8-210">az Azure AD tooenable felhasználók toolog a tooPurelyHR, akkor ki kell építenie PurelyHR be.</span><span class="sxs-lookup"><span data-stu-id="a7df8-210">tooenable Azure AD users toolog in tooPurelyHR, they must be provisioned into PurelyHR.</span></span> <span data-ttu-id="a7df8-211">Egy automatikus feladat PurelyHR, és nincs manuális lépések szükségesek, ha engedélyezve van az automatikus felhasználólétesítés.</span><span class="sxs-lookup"><span data-stu-id="a7df8-211">In PurelyHR, provisioning is an automatic task and no manual steps are required when automatic user provisioning is enabled.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a7df8-212">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a7df8-212">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a7df8-213">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPurelyHR megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a7df8-213">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPurelyHR.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a7df8-215">**tooassign Britta Simon tooPurelyHR, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a7df8-215">**tooassign Britta Simon tooPurelyHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="a7df8-216">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-216">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a7df8-218">Hello alkalmazások listában válassza ki a **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-218">In hello applications list, select **PurelyHR**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. <span data-ttu-id="a7df8-220">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a7df8-220">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a7df8-222">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7df8-222">Click **Add** button.</span></span> <span data-ttu-id="a7df8-223">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7df8-223">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a7df8-225">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a7df8-225">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a7df8-226">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7df8-226">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7df8-227">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7df8-227">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a7df8-228">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a7df8-228">Testing single sign-on</span></span>

<span data-ttu-id="a7df8-229">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a7df8-229">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a7df8-230">Hello LMS felvegye a hozzáférési Panel hello csempén kattintson automatikusan bejelentkezett tooyour felvegye LMS alkalmazás kap.</span><span class="sxs-lookup"><span data-stu-id="a7df8-230">Click hello Absorb LMS tile in hello Access Panel, you get automatically signed-on tooyour Absorb LMS application.</span></span>

<span data-ttu-id="a7df8-231">További információ a hozzáférési Panel hello:.</span><span class="sxs-lookup"><span data-stu-id="a7df8-231">For more information about hello Access Panel, see.</span></span> <span data-ttu-id="a7df8-232">[Hozzáférési Panel bemutatása toohello](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="a7df8-232">[Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7df8-233">További források</span><span class="sxs-lookup"><span data-stu-id="a7df8-233">Additional resources</span></span>

* [<span data-ttu-id="a7df8-234">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a7df8-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7df8-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a7df8-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

