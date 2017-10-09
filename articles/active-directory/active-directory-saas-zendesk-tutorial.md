---
title: "Oktatóanyag: Azure Active Directoryval integrált Zendesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Zendesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="d418d-103">Oktatóanyag: Azure Active Directoryval integrált Zendesk</span><span class="sxs-lookup"><span data-stu-id="d418d-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="d418d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ehhez az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d418d-104">In this tutorial, you learn how toointegrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d418d-105">Zendesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d418d-105">Integrating Zendesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d418d-106">Megadhatja a hozzáférés tooZendesk rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d418d-106">You can control in Azure AD who has access tooZendesk</span></span>
- <span data-ttu-id="d418d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZendesk (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d418d-107">You can enable your users tooautomatically get signed-on tooZendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d418d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d418d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d418d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d418d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d418d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d418d-110">Prerequisites</span></span>

<span data-ttu-id="d418d-111">Ehhez az Azure AD integrálása tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d418d-111">tooconfigure Azure AD integration with Zendesk, you need hello following items:</span></span>

- <span data-ttu-id="d418d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d418d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d418d-113">A Zendesk egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d418d-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="d418d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d418d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="d418d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d418d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d418d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d418d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d418d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d418d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="d418d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d418d-118">Scenario description</span></span>
<span data-ttu-id="d418d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d418d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d418d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d418d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d418d-121">Zendesk hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d418d-121">Adding Zendesk from hello gallery</span></span>
2. <span data-ttu-id="d418d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d418d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-hello-gallery"></a><span data-ttu-id="d418d-123">Zendesk hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d418d-123">Adding Zendesk from hello gallery</span></span>
<span data-ttu-id="d418d-124">tooconfigure hello integrációja ehhez az Azure AD-be, meg kell tooadd Zendesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d418d-124">tooconfigure hello integration of Zendesk into Azure AD, you need tooadd Zendesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d418d-125">**tooadd Zendesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d418d-125">**tooadd Zendesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d418d-126">A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d418d-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d418d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d418d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d418d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d418d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d418d-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d418d-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d418d-133">Hello keresési mezőbe, írja be a **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d418d-133">In hello search box, type **Zendesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="d418d-135">A hello eredmények panelen válassza ki a **Zendesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d418d-135">In hello results panel, select **Zendesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d418d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d418d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d418d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zendesk.</span><span class="sxs-lookup"><span data-stu-id="d418d-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d418d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zendesk tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d418d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zendesk is tooa user in Azure AD.</span></span> <span data-ttu-id="d418d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Zendesk közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d418d-140">In other words, a link relationship between an Azure AD user and hello related user in Zendesk needs toobe established.</span></span>

<span data-ttu-id="d418d-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Zendesk.</span><span class="sxs-lookup"><span data-stu-id="d418d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zendesk.</span></span>

<span data-ttu-id="d418d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Zendesk-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d418d-142">tooconfigure and test Azure AD single sign-on with Zendesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d418d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d418d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d418d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d418d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d418d-145">**[Zendesk tesztfelhasználó létrehozása](#creating-a-zendesk-test-user)**  -toohave egy megfelelője a Britta Simon a Zendesk, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d418d-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - toohave a counterpart of Britta Simon in Zendesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d418d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d418d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d418d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d418d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d418d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d418d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d418d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Zendesk-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d418d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="d418d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Zendesk, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d418d-150">**tooconfigure Azure AD single sign-on with Zendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="d418d-151">Az Azure portál, a hello hello **Zendesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d418d-151">In hello Azure portal, on hello **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d418d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d418d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="d418d-155">A hello **Zendesk-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d418d-155">On hello **Zendesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="d418d-157">a.</span><span class="sxs-lookup"><span data-stu-id="d418d-157">a.</span></span> <span data-ttu-id="d418d-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="d418d-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="d418d-159">b.</span><span class="sxs-lookup"><span data-stu-id="d418d-159">b.</span></span> <span data-ttu-id="d418d-160">A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="d418d-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d418d-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d418d-161">These values are not real.</span></span> <span data-ttu-id="d418d-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosító URL-t.</span><span class="sxs-lookup"><span data-stu-id="d418d-162">Update these values with hello actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="d418d-163">Ügyfél [Zendesk támogatási csoport](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d418d-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget these values.</span></span> 

4. <span data-ttu-id="d418d-164">Zendesk hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="d418d-164">Zendesk expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="d418d-165">Nem kötelező SAML attribútum, de opcionálisan hozzáadhat egy attribútumot **felhasználói attribútumok** szakasz következő hello következő lépések alapján:</span><span class="sxs-lookup"><span data-stu-id="d418d-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following hello below steps:</span></span> 

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="d418d-167">a.</span><span class="sxs-lookup"><span data-stu-id="d418d-167">a.</span></span> <span data-ttu-id="d418d-168">Kattintson a hello **megtekintés és Szerkesztés az összes többi attribútumával hello** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d418d-168">Click hello **View and edit all hello other attributes** check box.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="d418d-170">b.</span><span class="sxs-lookup"><span data-stu-id="d418d-170">b.</span></span> <span data-ttu-id="d418d-171">Kattintson a hello **attribútum hozzáadása** tooopen **Hozzáadás attribútum** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d418d-171">Click hello **Add Attribute** tooopen **Add attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="d418d-173">c.</span><span class="sxs-lookup"><span data-stu-id="d418d-173">c.</span></span> <span data-ttu-id="d418d-174">A hello **neve** szövegmezőhöz hello attribútum neve (például **emailaddress**).</span><span class="sxs-lookup"><span data-stu-id="d418d-174">In hello **Name** textbox, type hello attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="d418d-175">d.</span><span class="sxs-lookup"><span data-stu-id="d418d-175">d.</span></span> <span data-ttu-id="d418d-176">A hello **érték** listájában, jelölje be hello attribútum értéke (mint **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="d418d-176">From hello **Value** list, select hello attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="d418d-177">e.</span><span class="sxs-lookup"><span data-stu-id="d418d-177">e.</span></span> <span data-ttu-id="d418d-178">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="d418d-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="d418d-179">Bővítmény attribútumok tooadd attribútumok, amelyek nincsenek alapértelmezés szerint az Azure AD-ben használja.</span><span class="sxs-lookup"><span data-stu-id="d418d-179">You use extension attributes tooadd attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="d418d-180">Kattintson a [állítható be SAML felhasználói attribútumok](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello útvonalairól SAML attribútumok, amelyek **Zendesk** fogad el.</span><span class="sxs-lookup"><span data-stu-id="d418d-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="d418d-181">A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.</span><span class="sxs-lookup"><span data-stu-id="d418d-181">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="d418d-183">A hello **Zendesk konfigurációs** területén kattintson **konfigurálása Zendesk** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d418d-183">On hello **Zendesk Configuration** section, click **Configure Zendesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d418d-184">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d418d-184">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="d418d-186">Egy másik webes böngészőablakban jelentkezzen be a Zendesk vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d418d-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="d418d-187">Kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d418d-187">Click **Admin**.</span></span>

9. <span data-ttu-id="d418d-188">Hello bal oldali navigációs ablaktábláján kattintson **beállítások**, és kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="d418d-188">In hello left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="d418d-189">A hello **biztonsági** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d418d-189">On hello **Security** page, perform hello following steps:</span></span> 
   
     <span data-ttu-id="d418d-190">![Biztonsági](./media/active-directory-saas-zendesk-tutorial/ic773089.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="d418d-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="d418d-191">![Egyszeri bejelentkezés](./media/active-directory-saas-zendesk-tutorial/ic773090.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="d418d-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="d418d-192">a.</span><span class="sxs-lookup"><span data-stu-id="d418d-192">a.</span></span> <span data-ttu-id="d418d-193">Kattintson a hello **Admin & ügynökök** fülre.</span><span class="sxs-lookup"><span data-stu-id="d418d-193">Click hello **Admin & Agents** tab.</span></span>

     <span data-ttu-id="d418d-194">b.</span><span class="sxs-lookup"><span data-stu-id="d418d-194">b.</span></span> <span data-ttu-id="d418d-195">Válassza ki **egyszeri bejelentkezés (SSO) és a SAML**, majd válassza ki **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d418d-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="d418d-196">c.</span><span class="sxs-lookup"><span data-stu-id="d418d-196">c.</span></span> <span data-ttu-id="d418d-197">A **SAML SSO URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d418d-197">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="d418d-198">d.</span><span class="sxs-lookup"><span data-stu-id="d418d-198">d.</span></span> <span data-ttu-id="d418d-199">A **távoli kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d418d-199">In **Remote Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="d418d-200">e.</span><span class="sxs-lookup"><span data-stu-id="d418d-200">e.</span></span> <span data-ttu-id="d418d-201">A **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d418d-201">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="d418d-202">f.</span><span class="sxs-lookup"><span data-stu-id="d418d-202">f.</span></span> <span data-ttu-id="d418d-203">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d418d-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d418d-204">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d418d-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="d418d-205">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d418d-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d418d-207">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d418d-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d418d-208">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d418d-208">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d418d-210">felhasználók toodisplay hello listája túl Ugrás**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d418d-210">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d418d-212">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d418d-212">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d418d-214">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d418d-214">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d418d-216">a.</span><span class="sxs-lookup"><span data-stu-id="d418d-216">a.</span></span> <span data-ttu-id="d418d-217">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d418d-217">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d418d-218">b.</span><span class="sxs-lookup"><span data-stu-id="d418d-218">b.</span></span> <span data-ttu-id="d418d-219">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d418d-219">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d418d-220">c.</span><span class="sxs-lookup"><span data-stu-id="d418d-220">c.</span></span> <span data-ttu-id="d418d-221">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d418d-221">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d418d-222">d.</span><span class="sxs-lookup"><span data-stu-id="d418d-222">d.</span></span> <span data-ttu-id="d418d-223">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d418d-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="d418d-224">Zendesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d418d-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="d418d-225">az Azure AD tooenable felhasználók toolog történő **Zendesk**, azokat be kell üzembe **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d418d-225">tooenable Azure AD users toolog into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="d418d-226">Attól függően, hogy hello szerepkörrel hello alkalmazásokban annak hello viselkedés várható:</span><span class="sxs-lookup"><span data-stu-id="d418d-226">Depending on hello role assigned in hello apps, it's hello expected behavior:</span></span>

 1. <span data-ttu-id="d418d-227">**Végfelhasználói** fiókok bejelentkezéskor automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="d418d-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="d418d-228">**Ügynök** és **Admin** fiókokat kell manuálisan kiépítve toobe **Zendesk** bejelentkezés előtt.</span><span class="sxs-lookup"><span data-stu-id="d418d-228">**Agent** and **Admin** accounts need toobe manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="d418d-229">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d418d-229">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="d418d-230">Jelentkezzen be tooyour **Zendesk** bérlő.</span><span class="sxs-lookup"><span data-stu-id="d418d-230">Log in tooyour **Zendesk** tenant.</span></span>

2. <span data-ttu-id="d418d-231">Jelölje be hello **Ügyféllista** fülre.</span><span class="sxs-lookup"><span data-stu-id="d418d-231">Select hello **Customer List** tab.</span></span>

3. <span data-ttu-id="d418d-232">Jelölje be hello **felhasználói** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d418d-232">Select hello **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="d418d-233">![Felhasználó hozzáadása](./media/active-directory-saas-zendesk-tutorial/ic773632.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="d418d-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="d418d-234">Írja be egy meglévő Azure AD-fiókot szeretné, hogy tooprovision, és kattintson a hello e-mail címét **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d418d-234">Type hello email address of an existing Azure AD account you want tooprovision, and then click **Save**.</span></span>
   
    <span data-ttu-id="d418d-235">![Új felhasználó](./media/active-directory-saas-zendesk-tutorial/ic773633.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="d418d-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="d418d-236">Bármely más Zendesk felhasználói fiók létrehozása eszközök vagy Zendesk tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="d418d-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk tooprovision AAD user accounts.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d418d-237">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d418d-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d418d-238">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZendesk megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d418d-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZendesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d418d-240">**tooassign Britta Simon tooZendesk, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d418d-240">**tooassign Britta Simon tooZendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="d418d-241">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d418d-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d418d-243">Hello alkalmazások listában válassza ki a **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="d418d-243">In hello applications list, select **Zendesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="d418d-245">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d418d-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d418d-247">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d418d-247">Click **Add** button.</span></span> <span data-ttu-id="d418d-248">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d418d-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d418d-250">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d418d-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d418d-251">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d418d-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d418d-252">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d418d-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d418d-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d418d-253">Testing single sign-on</span></span>

<span data-ttu-id="d418d-254">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d418d-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d418d-255">Ha a hozzáférési Panel hello hello Zendesk csempe gombra kattint, automatikusan bejelentkezett tooyour Zendesk alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d418d-255">When you click hello Zendesk tile in hello Access Panel, you should get automatically signed-on tooyour Zendesk application.</span></span>
<span data-ttu-id="d418d-256">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d418d-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d418d-257">További források</span><span class="sxs-lookup"><span data-stu-id="d418d-257">Additional resources</span></span>

* [<span data-ttu-id="d418d-258">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d418d-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d418d-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d418d-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
