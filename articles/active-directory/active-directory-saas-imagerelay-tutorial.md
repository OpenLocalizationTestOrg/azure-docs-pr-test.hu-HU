---
title: "Oktatóanyag: Azure Active Directoryval integrált kép továbbítási |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a lemezkép továbbítási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: baf39e4437b85c2de5b524984ad5ca39badbab63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="56967-103">Oktatóanyag: Azure Active Directoryval integrált kép továbbító</span><span class="sxs-lookup"><span data-stu-id="56967-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="56967-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate kép Relay Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56967-104">In this tutorial, you learn how toointegrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="56967-105">Kép továbbítási integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="56967-105">Integrating Image Relay with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="56967-106">Megadhatja a hozzáférés tooImage továbbítási rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="56967-106">You can control in Azure AD who has access tooImage Relay</span></span>
- <span data-ttu-id="56967-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooImage továbbítási (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="56967-107">You can enable your users tooautomatically get signed-on tooImage Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="56967-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="56967-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="56967-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="56967-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56967-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="56967-110">Prerequisites</span></span>

<span data-ttu-id="56967-111">tooconfigure kép Relay Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="56967-111">tooconfigure Azure AD integration with Image Relay, you need hello following items:</span></span>

- <span data-ttu-id="56967-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="56967-112">An Azure AD subscription</span></span>
- <span data-ttu-id="56967-113">Egy kép továbbítási egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="56967-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="56967-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="56967-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="56967-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="56967-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="56967-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="56967-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="56967-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="56967-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="56967-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="56967-118">Scenario description</span></span>
<span data-ttu-id="56967-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="56967-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="56967-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="56967-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="56967-121">Kép továbbítási hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="56967-121">Adding Image Relay from hello gallery</span></span>
2. <span data-ttu-id="56967-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56967-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-hello-gallery"></a><span data-ttu-id="56967-123">Kép továbbítási hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="56967-123">Adding Image Relay from hello gallery</span></span>
<span data-ttu-id="56967-124">tooconfigure hello integrációs lemezkép-továbbító, az Azure AD-be, meg kell tooadd kép továbbítási hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="56967-124">tooconfigure hello integration of Image Relay into Azure AD, you need tooadd Image Relay from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="56967-125">**tooadd kép továbbítási hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56967-125">**tooadd Image Relay from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="56967-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56967-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="56967-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="56967-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="56967-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56967-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="56967-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="56967-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="56967-133">Hello keresési mezőbe, írja be a **kép továbbítási**.</span><span class="sxs-lookup"><span data-stu-id="56967-133">In hello search box, type **Image Relay**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="56967-135">Hello eredmények panelen, jelölje ki a **kép továbbítási**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-135">In hello results panel, select **Image Relay**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="56967-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="56967-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="56967-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján kép továbbító.</span><span class="sxs-lookup"><span data-stu-id="56967-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="56967-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói lemezkép továbbító a tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="56967-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Image Relay is tooa user in Azure AD.</span></span> <span data-ttu-id="56967-140">Ez azt jelenti hello kapcsolódó felhasználó a kép továbbító és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="56967-140">In other words, a link relationship between an Azure AD user and hello related user in Image Relay needs toobe established.</span></span>

<span data-ttu-id="56967-141">Lemezkép-továbbító, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="56967-141">In Image Relay, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="56967-142">tooconfigure és az Azure AD az egyszeri bejelentkezés kép továbbítási-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="56967-142">tooconfigure and test Azure AD single sign-on with Image Relay, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="56967-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="56967-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="56967-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="56967-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="56967-145">**[Egy kép továbbítási tesztfelhasználó létrehozása](#creating-an-image-relay-test-user)**  -toohave egy megfelelője a Britta Simon a kép Relay, amelyek felhasználói csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="56967-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - toohave a counterpart of Britta Simon in Image Relay that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="56967-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56967-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="56967-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="56967-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="56967-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="56967-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="56967-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a lemezkép Relay alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="56967-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="56967-150">**tooconfigure az Azure AD egyszeri bejelentkezést a kép továbbítási, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56967-150">**tooconfigure Azure AD single sign-on with Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="56967-151">Az Azure portál, a hello hello **kép továbbítási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="56967-151">In hello Azure portal, on hello **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="56967-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="56967-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="56967-155">A hello **kép továbbítási tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56967-155">On hello **Image Relay Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="56967-157">a.</span><span class="sxs-lookup"><span data-stu-id="56967-157">a.</span></span> <span data-ttu-id="56967-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="56967-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="56967-159">b.</span><span class="sxs-lookup"><span data-stu-id="56967-159">b.</span></span> <span data-ttu-id="56967-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="56967-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="56967-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="56967-161">These values are not real.</span></span> <span data-ttu-id="56967-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="56967-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="56967-163">Ügyfél [kép továbbítási ügyfél-támogatási csoport](http://support.imagerelay.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="56967-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="56967-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="56967-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="56967-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="56967-168">A hello **kép továbbítási konfigurációs** területen kattintson **konfigurálása kép továbbítási** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="56967-168">On hello **Image Relay Configuration** section, click **Configure Image Relay** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="56967-169">Másolás hello **Sign-Out szolgáltatás URL-CÍMÉT és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="56967-169">Copy hello **Sign-Out Service URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="56967-171">Egy másik böngészőablakban a bejelentkezés tooyour kép továbbítási vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="56967-171">In another browser window, sign in tooyour Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="56967-172">Hello hello felső eszköztárán kattintson hello **felhasználók és engedélyek** munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="56967-172">In hello toolbar on hello top, click hello **Users & Permissions** workload.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="56967-174">Kattintson a **hozzon létre új engedély**.</span><span class="sxs-lookup"><span data-stu-id="56967-174">Click **Create New Permission**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="56967-176">A hello **egyetlen bejelentkezést a beállítások** munkaterhelés, jelölje be hello **ennek a csoportnak a is csak bejelentkezés egyszeri bejelentkezés útján** jelölőnégyzetet, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="56967-176">In hello **Single Sign On Settings** workload, select hello **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="56967-178">Nyissa meg túl**Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="56967-178">Go too**Account Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="56967-180">Nyissa meg toohello **egyetlen bejelentkezést a beállítások** munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="56967-180">Go toohello **Single Sign On Settings** workload.</span></span>
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="56967-182">A hello **SAML beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56967-182">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="56967-184">a.</span><span class="sxs-lookup"><span data-stu-id="56967-184">a.</span></span> <span data-ttu-id="56967-185">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="56967-185">In **Login URL** textbox, paste hello value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="56967-186">b.</span><span class="sxs-lookup"><span data-stu-id="56967-186">b.</span></span> <span data-ttu-id="56967-187">A **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **egyetlen Sign-Out URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="56967-187">In **Logout URL**  textbox, paste hello value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="56967-188">c.</span><span class="sxs-lookup"><span data-stu-id="56967-188">c.</span></span> <span data-ttu-id="56967-189">Mint **név Azonosítóformátumának**, jelölje be **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="56967-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="56967-190">d.</span><span class="sxs-lookup"><span data-stu-id="56967-190">d.</span></span> <span data-ttu-id="56967-191">Mint **kötelező beállítások hello szolgáltató (kép továbbító) a érkező kéréseket**, jelölje be **FELADÁS egy vagy több kötelező**.</span><span class="sxs-lookup"><span data-stu-id="56967-191">As **Binding Options for Requests from hello Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="56967-192">e.</span><span class="sxs-lookup"><span data-stu-id="56967-192">e.</span></span> <span data-ttu-id="56967-193">A **x.509 tanúsítvány**, kattintson a **frissítés tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="56967-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="56967-195">f.</span><span class="sxs-lookup"><span data-stu-id="56967-195">f.</span></span> <span data-ttu-id="56967-196">Hello letöltött tanúsítvány megnyitása a Jegyzettömbben, hello tartalmat másolja és illessze be hello x.509 tanúsítvány szövegmező.</span><span class="sxs-lookup"><span data-stu-id="56967-196">Open hello downloaded certificate in notepad, copy hello content, and then paste it into hello x.509 Certificate textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="56967-198">g.</span><span class="sxs-lookup"><span data-stu-id="56967-198">g.</span></span> <span data-ttu-id="56967-199">A **fordítója Felhasználólétesítés** szakaszban, jelölje be hello **fordítója Felhasználólétesítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="56967-199">In **Just-In-Time User Provisioning** section, select hello **Enable Just-In-Time User Provisioning**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="56967-201">h.</span><span class="sxs-lookup"><span data-stu-id="56967-201">h.</span></span> <span data-ttu-id="56967-202">Jelölje be hello engedélycsoport (például **SSO alapvető**) csak keresztül egyszeri bejelentkezést a toosign engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="56967-202">Select hello permission group (for example, **SSO Basic**) which is allowed toosign in only through single sign-on.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="56967-204">i.</span><span class="sxs-lookup"><span data-stu-id="56967-204">i.</span></span> <span data-ttu-id="56967-205">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="56967-206">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="56967-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="56967-207">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="56967-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="56967-208">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="56967-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="56967-209">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56967-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="56967-210">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="56967-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="56967-212">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="56967-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="56967-213">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="56967-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="56967-215">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="56967-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="56967-217">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56967-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="56967-219">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="56967-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="56967-221">a.</span><span class="sxs-lookup"><span data-stu-id="56967-221">a.</span></span> <span data-ttu-id="56967-222">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="56967-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="56967-223">b.</span><span class="sxs-lookup"><span data-stu-id="56967-223">b.</span></span> <span data-ttu-id="56967-224">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="56967-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="56967-225">c.</span><span class="sxs-lookup"><span data-stu-id="56967-225">c.</span></span> <span data-ttu-id="56967-226">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="56967-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="56967-227">d.</span><span class="sxs-lookup"><span data-stu-id="56967-227">d.</span></span> <span data-ttu-id="56967-228">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="56967-229">Egy kép továbbítási tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="56967-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="56967-230">hello ebben a szakaszban célja toocreate Britta Simon meghívta lemezképét továbbítási felhasználó.</span><span class="sxs-lookup"><span data-stu-id="56967-230">hello objective of this section is toocreate a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="56967-231">**toocreate Britta Simon nevű kép továbbító, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56967-231">**toocreate a user called Britta Simon in Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="56967-232">Bejelentkezés tooyour kép továbbítási vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="56967-232">Sign-on tooyour Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="56967-233">Nyissa meg túl**felhasználók és engedélyek** válassza **egyszeri Bejelentkezéses felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="56967-233">Go too**Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="56967-235">Adja meg a hello **E-mail**, **Utónév**, **Vezetéknév**, és **vállalati** hello felhasználó szeretné tooprovision válassza hello engedélycsoport (például egyszeri bejelentkezés alapvető) Ez az hello csoport, amely csak a egyszeri bejelentkezés jelentkezhetnek be.</span><span class="sxs-lookup"><span data-stu-id="56967-235">Enter hello **Email**, **First Name**, **Last Name**, and **Company** of hello user you want tooprovision and select hello permission group (for example, SSO Basic) which is hello group that can sign in only through single sign-on.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="56967-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-237">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="56967-238">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="56967-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="56967-239">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooImage továbbítási megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="56967-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooImage Relay.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="56967-241">**tooassign Britta Simon tooImage továbbítási, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="56967-241">**tooassign Britta Simon tooImage Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="56967-242">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="56967-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="56967-244">Hello alkalmazások listában válassza ki a **kép továbbítási**.</span><span class="sxs-lookup"><span data-stu-id="56967-244">In hello applications list, select **Image Relay**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="56967-246">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="56967-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="56967-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="56967-248">Click **Add** button.</span></span> <span data-ttu-id="56967-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56967-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="56967-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="56967-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="56967-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56967-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="56967-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="56967-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="56967-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="56967-254">Testing single sign-on</span></span>

<span data-ttu-id="56967-255">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="56967-255">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>    

<span data-ttu-id="56967-256">Hello kép továbbítási csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour kép továbbítási alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="56967-256">When you click hello Image Relay tile in hello Access Panel, you should get automatically signed-on tooyour Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="56967-257">További források</span><span class="sxs-lookup"><span data-stu-id="56967-257">Additional resources</span></span>

* [<span data-ttu-id="56967-258">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="56967-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="56967-259">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="56967-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

