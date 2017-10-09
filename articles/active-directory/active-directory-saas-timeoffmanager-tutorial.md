---
title: "Oktatóanyag: Azure Active Directoryval integrált TimeOffManager |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TimeOffManager között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c871257bfb49883e31b1c4860a9d7faa70e9ab48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a><span data-ttu-id="e5bc4-103">Oktatóanyag: Azure Active Directoryval integrált TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="e5bc4-103">Tutorial: Azure Active Directory integration with TimeOffManager</span></span>

<span data-ttu-id="e5bc4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TimeOffManager az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5bc4-104">In this tutorial, you learn how toointegrate TimeOffManager with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5bc4-105">TimeOffManager integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-105">Integrating TimeOffManager with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e5bc4-106">Megadhatja a hozzáférés tooTimeOffManager rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e5bc4-106">You can control in Azure AD who has access tooTimeOffManager</span></span>
- <span data-ttu-id="e5bc4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTimeOffManager (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e5bc4-107">You can enable your users tooautomatically get signed-on tooTimeOffManager (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5bc4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5bc4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e5bc4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5bc4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5bc4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5bc4-110">Prerequisites</span></span>

<span data-ttu-id="e5bc4-111">az Azure AD integrálása TimeOffManager tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-111">tooconfigure Azure AD integration with TimeOffManager, you need hello following items:</span></span>

- <span data-ttu-id="e5bc4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e5bc4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5bc4-113">Egy TimeOffManager egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e5bc4-113">A TimeOffManager single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5bc4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5bc4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5bc4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5bc4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5bc4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5bc4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e5bc4-118">Scenario description</span></span>
<span data-ttu-id="e5bc4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5bc4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5bc4-121">Adja hozzá a TimeOffManager hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e5bc4-121">Add TimeOffManager from hello gallery</span></span>
2. <span data-ttu-id="e5bc4-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5bc4-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-timeoffmanager-from-hello-gallery"></a><span data-ttu-id="e5bc4-123">Adja hozzá a TimeOffManager hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e5bc4-123">Add TimeOffManager from hello gallery</span></span>
<span data-ttu-id="e5bc4-124">tooconfigure hello integrációja TimeOffManager az Azure AD-be, meg kell tooadd TimeOffManager hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-124">tooconfigure hello integration of TimeOffManager into Azure AD, you need tooadd TimeOffManager from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e5bc4-125">**tooadd TimeOffManager hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5bc4-125">**tooadd TimeOffManager from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5bc4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5bc4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e5bc4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e5bc4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e5bc4-133">Hello keresési mezőbe, írja be a **TimeOffManager**, jelölje be **TimeOffManager** eredmény panelen, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-133">In hello search box, type **TimeOffManager**, select **TimeOffManager** from result panel and then click **Add** button tooadd hello application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e5bc4-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5bc4-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="e5bc4-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-136">In this section, you configure and test Azure AD single sign-on with TimeOffManager based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e5bc4-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TimeOffManager tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TimeOffManager is tooa user in Azure AD.</span></span> <span data-ttu-id="e5bc4-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TimeOffManager közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-138">In other words, a link relationship between an Azure AD user and hello related user in TimeOffManager needs toobe established.</span></span>

<span data-ttu-id="e5bc4-139">TimeOffManager, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-139">In TimeOffManager, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e5bc4-140">tooconfigure és az Azure AD az egyszeri bejelentkezés TimeOffManager-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-140">tooconfigure and test Azure AD single sign-on with TimeOffManager, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e5bc4-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e5bc4-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5bc4-143">**[TimeOffManager tesztfelhasználó létrehozása](#create-a-timeoffmanager-test-user)**  -toohave egy megfelelője a Britta Simon a TimeOffManager, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-143">**[Create a TimeOffManager test user](#create-a-timeoffmanager-test-user)** - toohave a counterpart of Britta Simon in TimeOffManager that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5bc4-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5bc4-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e5bc4-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5bc4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e5bc4-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TimeOffManager alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TimeOffManager application.</span></span>

<span data-ttu-id="e5bc4-148">**az Azure AD tooconfigure egyszeri bejelentkezést a TimeOffManager, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5bc4-148">**tooconfigure Azure AD single sign-on with TimeOffManager, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5bc4-149">Az Azure portál, a hello hello **TimeOffManager** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-149">In hello Azure portal, on hello **TimeOffManager** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e5bc4-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. <span data-ttu-id="e5bc4-153">A hello **TimeOffManager tartomány és az URL-címek** területen hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-153">On hello **TimeOffManager Domain and URLs** section, perform hello following:</span></span>

     ![TimeOffManager tartomány és az URL-címek szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    <span data-ttu-id="e5bc4-155">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span><span class="sxs-lookup"><span data-stu-id="e5bc4-155">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5bc4-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-156">This value is not real.</span></span> <span data-ttu-id="e5bc4-157">Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-157">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="e5bc4-158">Ezt az értéket kaphat **egyszeri bejelentkezési beállítások lapon** amely magyarázatát megtalálja a hello oktatóanyag, vagy forduljon a [TimeOffManager támogatási csoport](http://www.timeoffmanager.com/contact-us.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5bc4-158">You can get this value from **Single Sign on settings page** which is explained later in hello tutorial or Contact [TimeOffManager support team](http://www.timeoffmanager.com/contact-us.aspx).</span></span>
 
4. <span data-ttu-id="e5bc4-159">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. <span data-ttu-id="e5bc4-161">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooTimeOffManger fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-161">hello objective of this section is toooutline how tooenable users tooauthenticate tooTimeOffManger with their account in Azure AD using federation based on hello SAML protocol.</span></span>
    
    <span data-ttu-id="e5bc4-162">A TimeOffManger alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-162">Your TimeOffManger application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="e5bc4-163">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-163">hello following screenshot shows an example for this.</span></span>

    <span data-ttu-id="e5bc4-164">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-164">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml token attributes")</span></span>
    
    | <span data-ttu-id="e5bc4-165">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="e5bc4-165">Attribute Name</span></span> | <span data-ttu-id="e5bc4-166">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="e5bc4-166">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="e5bc4-167">Utónév</span><span class="sxs-lookup"><span data-stu-id="e5bc4-167">Firstname</span></span> |<span data-ttu-id="e5bc4-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e5bc4-168">User.givenname</span></span> |
    | <span data-ttu-id="e5bc4-169">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="e5bc4-169">Lastname</span></span> |<span data-ttu-id="e5bc4-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="e5bc4-170">User.surname</span></span> |
    | <span data-ttu-id="e5bc4-171">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="e5bc4-171">Email</span></span> |<span data-ttu-id="e5bc4-172">User.mail</span><span class="sxs-lookup"><span data-stu-id="e5bc4-172">User.mail</span></span> |
    
    <span data-ttu-id="e5bc4-173">a.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-173">a.</span></span>  <span data-ttu-id="e5bc4-174">Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-174">For each data row in hello table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="e5bc4-175">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-175">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml token attributes")</span></span>
    
    <span data-ttu-id="e5bc4-176">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-176">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml token attributes")</span></span>
    
    <span data-ttu-id="e5bc4-177">b.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-177">b.</span></span>  <span data-ttu-id="e5bc4-178">A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-178">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="e5bc4-179">c.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-179">c.</span></span>  <span data-ttu-id="e5bc4-180">A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-180">In hello **Attribute Value** textbox, select hello attribute  value shown for that row.</span></span>
    
    <span data-ttu-id="e5bc4-181">d.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-181">d.</span></span>  <span data-ttu-id="e5bc4-182">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-182">Click **Ok**.</span></span>
    
6. <span data-ttu-id="e5bc4-183">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-183">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="e5bc4-185">A hello **TimeOffManager konfigurációs** kattintson **konfigurálása TimeOffManager** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-185">On hello **TimeOffManager Configuration** section, click **Configure TimeOffManager** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e5bc4-186">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e5bc4-186">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TimeOffManager konfigurációs szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. <span data-ttu-id="e5bc4-188">Egy másik webes böngészőablakban jelentkezzen be a TimeOffManager vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-188">In a different web browser window, log into your TimeOffManager company site as an administrator.</span></span>

9. <span data-ttu-id="e5bc4-189">Nyissa meg túl**fiók \> beállítások \> egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-189">Go too**Account \> Account Options \> Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="e5bc4-190">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-190">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Single Sign-On Settings")</span></span>
7. <span data-ttu-id="e5bc4-191">A hello **egyszeri bejelentkezési beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-191">In hello **Single Sign-On Settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="e5bc4-192">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-192">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="e5bc4-193">a.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-193">a.</span></span> <span data-ttu-id="e5bc4-194">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be a teljes tanúsítvány hello **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-194">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>
   
   <span data-ttu-id="e5bc4-195">b.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-195">b.</span></span> <span data-ttu-id="e5bc4-196">A **Idp kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-196">In **Idp Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="e5bc4-197">c.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-197">c.</span></span> <span data-ttu-id="e5bc4-198">A **IdP végponti URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-198">In **IdP Endpoint URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="e5bc4-199">d.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-199">d.</span></span> <span data-ttu-id="e5bc4-200">Mint **kényszerítése SAML**, jelölje be **nem**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-200">As **Enforce SAML**, select **No**.</span></span>
   
   <span data-ttu-id="e5bc4-201">e.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-201">e.</span></span> <span data-ttu-id="e5bc4-202">Mint **automatikus létrehozása felhasználók**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-202">As **Auto-Create Users**, select **Yes**.</span></span>
   
   <span data-ttu-id="e5bc4-203">f.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-203">f.</span></span> <span data-ttu-id="e5bc4-204">A **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-204">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="e5bc4-205">g.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-205">g.</span></span> <span data-ttu-id="e5bc4-206">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-206">click **Save Changes**.</span></span>

11. <span data-ttu-id="e5bc4-207">A **egyszeri bejelentkezési beállítások** lapon, a Másolás hello értékének **helyességi feltétel ügyfél szolgáltatás URL-címe** és beillesztheti hello **válasz URL-CÍMEN** szövegmező alatt **TimeOffManager Tartomány- és URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-207">In **Single Sign on settings** page, copy hello value of **Assertion Consumer Service URL** and paste it in hello **Reply URL** text box under **TimeOffManager Domain and URLs** section in Azure portal.</span></span> 

      <span data-ttu-id="e5bc4-208">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="e5bc4-208">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Single Sign-On Settings")</span></span>

> [!TIP]
> <span data-ttu-id="e5bc4-209">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e5bc4-209">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e5bc4-210">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-210">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e5bc4-211">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5bc4-211">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e5bc4-212">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e5bc4-212">Create an Azure AD test user</span></span>
<span data-ttu-id="e5bc4-213">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-213">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e5bc4-215">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e5bc4-215">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5bc4-216">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-216">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5bc4-218">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-218">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok--> minden felhasználó](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5bc4-220">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-220">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5bc4-222">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5bc4-222">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5bc4-224">a.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-224">a.</span></span> <span data-ttu-id="e5bc4-225">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-225">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5bc4-226">b.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-226">b.</span></span> <span data-ttu-id="e5bc4-227">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-227">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5bc4-228">c.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-228">c.</span></span> <span data-ttu-id="e5bc4-229">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-229">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e5bc4-230">d.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-230">d.</span></span> <span data-ttu-id="e5bc4-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-231">Click **Create**.</span></span>
 
### <a name="create-a-timeoffmanager-test-user"></a><span data-ttu-id="e5bc4-232">TimeOffManager tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5bc4-232">Create a TimeOffManager test user</span></span>

<span data-ttu-id="e5bc4-233">Rendelés tooenable az Azure AD felhasználók toolog történő TimeOffManager kiosztott tooTimeOffManager kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-233">In order tooenable Azure AD users toolog into TimeOffManager, they must be provisioned tooTimeOffManager.</span></span>  

<span data-ttu-id="e5bc4-234">TimeOffManager csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-234">TimeOffManager supports just in time user provisioning.</span></span> <span data-ttu-id="e5bc4-235">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-235">There is no action item for you.</span></span>  

<span data-ttu-id="e5bc4-236">hello felhasználót adnak hozzá automatikusan az egyszeri bejelentkezés használatával hello első bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-236">hello users are added automatically during hello first login using single sign on.</span></span>

>[!NOTE]
><span data-ttu-id="e5bc4-237">Bármely más TimeOffManager felhasználói fiók létrehozása eszközök vagy TimeOffManager tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-237">You can use any other TimeOffManager user account creation tools or APIs provided by TimeOffManager tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e5bc4-238">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e5bc4-238">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e5bc4-239">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTimeOffManager megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTimeOffManager.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e5bc4-241">**tooassign Britta Simon tooTimeOffManager, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e5bc4-241">**tooassign Britta Simon tooTimeOffManager, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5bc4-242">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e5bc4-244">Hello alkalmazások listában válassza ki a **TimeOffManager**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-244">In hello applications list, select **TimeOffManager**.</span></span>

    ![TimeOffManager alkalmazáslistában](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. <span data-ttu-id="e5bc4-246">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e5bc4-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-248">Click **Add** button.</span></span> <span data-ttu-id="e5bc4-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e5bc4-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e5bc4-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5bc4-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e5bc4-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e5bc4-254">Test single sign-on</span></span>

<span data-ttu-id="e5bc4-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-255">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e5bc4-256">Ha a hozzáférési Panel hello hello TimeOffManager csempe gombra kattint, automatikusan bejelentkezett tooyour TimeOffManager alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e5bc4-256">When you click hello TimeOffManager tile in hello Access Panel, you should get automatically signed-on tooyour TimeOffManager application.</span></span> <span data-ttu-id="e5bc4-257">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e5bc4-257">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5bc4-258">További források</span><span class="sxs-lookup"><span data-stu-id="e5bc4-258">Additional resources</span></span>

* [<span data-ttu-id="e5bc4-259">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e5bc4-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5bc4-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e5bc4-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

