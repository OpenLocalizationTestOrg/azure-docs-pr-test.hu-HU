---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a LinkedIn jogosultságszint-emelés között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a><span data-ttu-id="e1521-103">Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés</span><span class="sxs-lookup"><span data-stu-id="e1521-103">Tutorial: Azure Active Directory integration with LinkedIn Elevate</span></span>

<span data-ttu-id="e1521-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn jogosultságszint-emelés az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e1521-104">In this tutorial, you learn how toointegrate LinkedIn Elevate with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e1521-105">Jogosultságszint-emelés LinkedIn integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e1521-105">Integrating LinkedIn Elevate with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e1521-106">Megadhatja a hozzáférés tooLinkedIn jogosultságszint-emelés rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e1521-106">You can control in Azure AD who has access tooLinkedIn Elevate</span></span>
- <span data-ttu-id="e1521-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn jogosultságszint-emelés (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e1521-107">You can enable your users tooautomatically get signed-on tooLinkedIn Elevate (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e1521-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="e1521-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="e1521-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e1521-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1521-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e1521-110">Prerequisites</span></span>

<span data-ttu-id="e1521-111">tooconfigure LinkedIn jogosultságszint-emelés integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="e1521-111">tooconfigure Azure AD integration with LinkedIn Elevate, you need hello following items:</span></span>

- <span data-ttu-id="e1521-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e1521-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e1521-113">A LinkedIn jogosultságszint-emelés egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e1521-113">A LinkedIn Elevate single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e1521-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e1521-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e1521-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e1521-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e1521-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e1521-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e1521-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e1521-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e1521-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e1521-118">Scenario description</span></span>
<span data-ttu-id="e1521-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e1521-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e1521-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e1521-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e1521-121">Hozzáadás a LinkedIn jogosultságszint-emelés hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e1521-121">Adding LinkedIn Elevate from hello gallery</span></span>
2. <span data-ttu-id="e1521-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e1521-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-elevate-from-hello-gallery"></a><span data-ttu-id="e1521-123">Hozzáadás a LinkedIn jogosultságszint-emelés hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e1521-123">Adding LinkedIn Elevate from hello gallery</span></span>
<span data-ttu-id="e1521-124">tooconfigure hello integrációja LinkedIn jogosultságszint-emelés az Azure AD-be, meg kell tooadd LinkedIn jogosultságszint-emelés hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e1521-124">tooconfigure hello integration of LinkedIn Elevate into Azure AD, you need tooadd LinkedIn Elevate from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e1521-125">**tooadd LinkedIn jogosultságszint-emelés hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e1521-125">**tooadd LinkedIn Elevate from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1521-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e1521-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e1521-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e1521-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e1521-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e1521-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e1521-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e1521-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e1521-133">Hello keresési mezőbe, írja be a **LinkedIn jogosultságszint-emelés**.</span><span class="sxs-lookup"><span data-stu-id="e1521-133">In hello search box, type **LinkedIn Elevate**.</span></span> <span data-ttu-id="e1521-134">Az eredmények panelen kattintson a **LinkedIn jogosultságszint-emelés** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e1521-134">From results panel, click **LinkedIn Elevate** tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e1521-136">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e1521-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e1521-137">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn jogosultságszint-emelés "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="e1521-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Elevate based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e1521-138">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LinkedIn jogosultságszint-emelés tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e1521-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Elevate is tooa user in Azure AD.</span></span> <span data-ttu-id="e1521-139">Ez azt jelenti hello kapcsolódó felhasználó a LinkedIn jogosultságszint-emelés és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e1521-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Elevate needs toobe established.</span></span>

<span data-ttu-id="e1521-140">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a LinkedIn jogosultságszint-emelés.</span><span class="sxs-lookup"><span data-stu-id="e1521-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Elevate.</span></span>

<span data-ttu-id="e1521-141">tooconfigure és a LinkedIn jogosultságszint-emelés az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e1521-141">tooconfigure and test Azure AD single sign-on with LinkedIn Elevate, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e1521-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e1521-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e1521-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e1521-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e1521-144">**[Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása](#creating-a-linkedin-elevate-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e1521-144">**[Creating a LinkedIn Elevate test user](#creating-a-linkedin-elevate-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="e1521-145">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e1521-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e1521-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e1521-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e1521-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e1521-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e1521-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a LinkedIn jogosultságszint-emelés alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e1521-148">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your LinkedIn Elevate application.</span></span>

<span data-ttu-id="e1521-149">**az Azure AD tooconfigure egyszeri bejelentkezést a LinkedIn jogosultságszint-emelés, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e1521-149">**tooconfigure Azure AD single sign-on with LinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1521-150">Hello Azure felügyeleti portálon, a hello **LinkedIn jogosultságszint-emelés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e1521-150">In hello Azure Management portal, on hello **LinkedIn Elevate** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e1521-152">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="e1521-152">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="e1521-154">Egy másik webes böngészőablakban, bejelentkezés tooyour LinkedIn jogosultságszint-emelés Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="e1521-154">In a different web browser window, sign-on tooyour LinkedIn Elevate tenant as an administrator.</span></span>

4. <span data-ttu-id="e1521-155">A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e1521-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="e1521-156">Jelölje ki, **jogosultságszint-emelés - jogosultságszint-emelés AAD teszt** hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="e1521-156">Also, select **Elevate - Elevate AAD Test** from hello dropdown list.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="e1521-158">Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**</span><span class="sxs-lookup"><span data-stu-id="e1521-158">Click on **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="e1521-160">Azure-portál a **LinkedIn jogosultságszint-emelés tartomány és az URL-címek**, hajtsa végre a következő lépéseket, ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure hello a **IdP kezdeményezett** mód</span><span class="sxs-lookup"><span data-stu-id="e1521-160">On Azure Portal, under **LinkedIn Elevate Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="e1521-162">a.</span><span class="sxs-lookup"><span data-stu-id="e1521-162">a.</span></span> <span data-ttu-id="e1521-163">A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="e1521-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="e1521-164">b.</span><span class="sxs-lookup"><span data-stu-id="e1521-164">b.</span></span> <span data-ttu-id="e1521-165">A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva</span><span class="sxs-lookup"><span data-stu-id="e1521-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="e1521-166">Ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure a **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése beállítás hello konfigurációs szakaszban, és hello bejelentkezési URL-címen a következő mintát hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="e1521-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. <span data-ttu-id="e1521-168">A LinkedIn jogosultságszint-emelés alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="e1521-168">Your LinkedIn Elevate application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="e1521-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="e1521-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="e1521-170">alapértelmezett értékének hello **felhasználói azonosító** van **user.userprincipalname** , de ez hello a felhasználó e-mail címét leképezve toobe LinkedIn jogosultságszint-emelés vár.</span><span class="sxs-lookup"><span data-stu-id="e1521-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Elevate expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="e1521-171">Az adott használhatja **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="e1521-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. <span data-ttu-id="e1521-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="e1521-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="e1521-174">Egy másik jogcím nevű kell tooadd **részleg** , ezért hello érték toobe leképezése túl**felhasználó.részleg**.</span><span class="sxs-lookup"><span data-stu-id="e1521-174">You need tooadd another claim named **department** and hello value needs toobe mapped too**user.department**.</span></span>

    | <span data-ttu-id="e1521-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="e1521-175">Attribute Name</span></span> | <span data-ttu-id="e1521-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="e1521-176">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="e1521-177">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="e1521-177">department</span></span>| <span data-ttu-id="e1521-178">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="e1521-178">user.department</span></span> |

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      <span data-ttu-id="e1521-180">a.</span><span class="sxs-lookup"><span data-stu-id="e1521-180">a.</span></span> <span data-ttu-id="e1521-181">Kattintson a Hozzáadás attribútum tooopen hello attribútum részleteit megjelenítő oldalra hello részleg attribútum hozzáadása, ahogy az alábbi-</span><span class="sxs-lookup"><span data-stu-id="e1521-181">Click on Add attribute tooopen hello attribute details page add hello department attribute as shown below-</span></span>

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      <span data-ttu-id="e1521-183">b.</span><span class="sxs-lookup"><span data-stu-id="e1521-183">b.</span></span> <span data-ttu-id="e1521-184">Kattintson a **Ok** toosave hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="e1521-184">Click on **Ok** toosave hello attribute.</span></span>

      <span data-ttu-id="e1521-185">c.</span><span class="sxs-lookup"><span data-stu-id="e1521-185">c.</span></span> <span data-ttu-id="e1521-186">Hello nevének módosítása hello attribútum **emailaddress** túl**e-mail**.</span><span class="sxs-lookup"><span data-stu-id="e1521-186">Change hello name of hello attribute **emailaddress** too**email**.</span></span>


10. <span data-ttu-id="e1521-187">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e1521-187">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. <span data-ttu-id="e1521-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e1521-189">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="e1521-191">Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="e1521-191">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="e1521-192">Töltse fel az imént letöltött hello Azure-portálon való feltöltése XML-fájl lehetőség hello kattintva hello XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="e1521-192">Upload hello XML file you just downloaded from hello Azure portal by clicking on hello Upload XML file option.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. <span data-ttu-id="e1521-194">Kattintson a **a** tooenable egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e1521-194">Click **On** tooenable SSO.</span></span> <span data-ttu-id="e1521-195">Egyszeri bejelentkezési állapot állapotúról **nincs csatlakoztatva** túl**csatlakoztatva**</span><span class="sxs-lookup"><span data-stu-id="e1521-195">SSO status will change from **Not Connected** too**Connected**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e1521-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1521-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="e1521-198">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="e1521-198">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e1521-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e1521-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1521-201">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e1521-201">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e1521-203">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="e1521-203">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e1521-205">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e1521-205">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e1521-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e1521-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e1521-209">a.</span><span class="sxs-lookup"><span data-stu-id="e1521-209">a.</span></span> <span data-ttu-id="e1521-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e1521-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e1521-211">b.</span><span class="sxs-lookup"><span data-stu-id="e1521-211">b.</span></span> <span data-ttu-id="e1521-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e1521-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e1521-213">c.</span><span class="sxs-lookup"><span data-stu-id="e1521-213">c.</span></span> <span data-ttu-id="e1521-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e1521-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e1521-215">d.</span><span class="sxs-lookup"><span data-stu-id="e1521-215">d.</span></span> <span data-ttu-id="e1521-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e1521-216">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-elevate-test-user"></a><span data-ttu-id="e1521-217">Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1521-217">Creating a LinkedIn Elevate test user</span></span>

<span data-ttu-id="e1521-218">Csatolt jogosultságszint-emelés alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="e1521-218">Linked Elevate Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="e1521-219">Hello rendszergazdai beállítások lapján hello LinkedIn jogosultságszint-emelés portál tükrözés hello kapcsolón **automatikus hozzárendelése licencek** tooactive tooenable csak idő üzembe helyezése, és ez is hozzárendelése a licenc toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e1521-219">On hello admin settings page on hello LinkedIn Elevate portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>

   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e1521-221">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e1521-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e1521-222">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooLinkedIn jogosultságszint-emelés megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e1521-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooLinkedIn Elevate.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e1521-224">**tooassign Britta Simon tooLinkedIn jogosultságszint-emelés, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e1521-224">**tooassign Britta Simon tooLinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1521-225">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e1521-225">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e1521-227">Hello alkalmazások listában válassza ki a **LinkedIn jogosultságszint-emelés**.</span><span class="sxs-lookup"><span data-stu-id="e1521-227">In hello applications list, select **LinkedIn Elevate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. <span data-ttu-id="e1521-229">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e1521-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e1521-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e1521-231">Click **Add** button.</span></span> <span data-ttu-id="e1521-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e1521-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e1521-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e1521-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e1521-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e1521-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e1521-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e1521-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e1521-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e1521-237">Testing single sign-on</span></span>

<span data-ttu-id="e1521-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e1521-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e1521-239">Hello LinkedIn jogosultságszint-emelés csempe a hozzáférési Panel hello kattintáskor szerezheti be hello Azure bejelentkezési oldalra, és a sikeres bejelentkezést, miután szerezheti be a jogosultságszint-emelés LinkedIn alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e1521-239">When you click hello LinkedIn Elevate tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Elevate application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1521-240">További források</span><span class="sxs-lookup"><span data-stu-id="e1521-240">Additional resources</span></span>

* [<span data-ttu-id="e1521-241">Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés az automatikus felhasználó kiépítése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e1521-241">Tutorial: Configuring LinkedIn Elevate for automatic user provisioning with Azure Active Directory</span></span>](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [<span data-ttu-id="e1521-242">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e1521-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e1521-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e1521-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
