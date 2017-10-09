---
title: "Oktatóanyag: Azure Active Directoryval integrált Workfront |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Workfront között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: e7249b9ec769f19cf5aa7d44ff6f58705df4020a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a><span data-ttu-id="2cda4-103">Oktatóanyag: Azure Active Directoryval integrált Workfront</span><span class="sxs-lookup"><span data-stu-id="2cda4-103">Tutorial: Azure Active Directory integration with Workfront</span></span>

<span data-ttu-id="2cda4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workfront az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2cda4-104">In this tutorial, you learn how toointegrate Workfront with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2cda4-105">Workfront integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2cda4-105">Integrating Workfront with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2cda4-106">Megadhatja a hozzáférés tooWorkfront rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2cda4-106">You can control in Azure AD who has access tooWorkfront</span></span>
- <span data-ttu-id="2cda4-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkfront (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2cda4-107">You can enable your users tooautomatically get signed-on tooWorkfront (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2cda4-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2cda4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2cda4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2cda4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cda4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2cda4-110">Prerequisites</span></span>

<span data-ttu-id="2cda4-111">az Azure AD integrálása Workfront tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="2cda4-111">tooconfigure Azure AD integration with Workfront, you need hello following items:</span></span>

- <span data-ttu-id="2cda4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2cda4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2cda4-113">Egy Workfront egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2cda4-113">A Workfront single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2cda4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2cda4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2cda4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2cda4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2cda4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2cda4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2cda4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2cda4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2cda4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2cda4-118">Scenario description</span></span>
<span data-ttu-id="2cda4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2cda4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2cda4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2cda4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2cda4-121">Hello gyűjteményből Workfront hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2cda4-121">Adding Workfront from hello gallery</span></span>
2. <span data-ttu-id="2cda4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cda4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workfront-from-hello-gallery"></a><span data-ttu-id="2cda4-123">Hello gyűjteményből Workfront hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2cda4-123">Adding Workfront from hello gallery</span></span>
<span data-ttu-id="2cda4-124">tooconfigure hello integrációja Workfront az Azure AD-be, meg kell tooadd Workfront hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2cda4-124">tooconfigure hello integration of Workfront into Azure AD, you need tooadd Workfront from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2cda4-125">**tooadd Workfront hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cda4-125">**tooadd Workfront from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cda4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2cda4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2cda4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2cda4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2cda4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2cda4-133">Hello keresési mezőbe, írja be a **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-133">In hello search box, type **Workfront**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_search.png)

5. <span data-ttu-id="2cda4-135">A hello eredmények panelen válassza ki a **Workfront**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-135">In hello results panel, select **Workfront**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2cda4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cda4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2cda4-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Workfront</span><span class="sxs-lookup"><span data-stu-id="2cda4-138">In this section, you configure and test Azure AD single sign-on with Workfront based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2cda4-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Workfront tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2cda4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workfront is tooa user in Azure AD.</span></span> <span data-ttu-id="2cda4-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Workfront közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="2cda4-140">In other words, a link relationship between an Azure AD user and hello related user in Workfront needs toobe established.</span></span>

<span data-ttu-id="2cda4-141">Workfront, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2cda4-141">In Workfront, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2cda4-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Workfront-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2cda4-142">tooconfigure and test Azure AD single sign-on with Workfront, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2cda4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2cda4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2cda4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2cda4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2cda4-145">**[Workfront tesztfelhasználó létrehozása](#creating-a-workfront-test-user)**  -toohave egy megfelelője a Britta Simon a Workfront, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2cda4-145">**[Creating a Workfront test user](#creating-a-workfront-test-user)** - toohave a counterpart of Britta Simon in Workfront that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2cda4-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2cda4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2cda4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2cda4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2cda4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cda4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2cda4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Workfront alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2cda4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workfront application.</span></span>

<span data-ttu-id="2cda4-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Workfront, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cda4-150">**tooconfigure Azure AD single sign-on with Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cda4-151">Az Azure portál, a hello hello **Workfront** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-151">In hello Azure portal, on hello **Workfront** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2cda4-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2cda4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_samlbase.png)

3. <span data-ttu-id="2cda4-155">A hello **Workfront tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2cda4-155">On hello **Workfront Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_url.png)

    <span data-ttu-id="2cda4-157">a.</span><span class="sxs-lookup"><span data-stu-id="2cda4-157">a.</span></span> <span data-ttu-id="2cda4-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.attask-ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="2cda4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.attask-ondemand.com`</span></span>

    <span data-ttu-id="2cda4-159">b.</span><span class="sxs-lookup"><span data-stu-id="2cda4-159">b.</span></span> <span data-ttu-id="2cda4-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.attasksandbox.com/SAML2`</span><span class="sxs-lookup"><span data-stu-id="2cda4-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.attasksandbox.com/SAML2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2cda4-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2cda4-161">These values are not real.</span></span> <span data-ttu-id="2cda4-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="2cda4-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2cda4-163">Ügyfél [Workfront ügyfél-támogatási csoport](https://www.workfront.com/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2cda4-163">Contact [Workfront Client support team](https://www.workfront.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="2cda4-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2cda4-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_certificate.png) 

5. <span data-ttu-id="2cda4-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2cda4-168">A hello **Workfront konfigurációs** kattintson **konfigurálása Workfront** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2cda4-168">On hello **Workfront Configuration** section, click **Configure Workfront** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2cda4-169">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2cda4-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_configure.png) 

7. <span data-ttu-id="2cda4-171">Bejelentkezés tooyour Workfront vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2cda4-171">Sign-on tooyour Workfront company site as administrator.</span></span>

8. <span data-ttu-id="2cda4-172">Nyissa meg túl**egyszeri bejelentkezést a konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-172">Go too**Single Sign On Configuration**.</span></span>

9. <span data-ttu-id="2cda4-173">A hello **egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello</span><span class="sxs-lookup"><span data-stu-id="2cda4-173">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="2cda4-175">a.</span><span class="sxs-lookup"><span data-stu-id="2cda4-175">a.</span></span> <span data-ttu-id="2cda4-176">Mint **típus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-176">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="2cda4-177">b.</span><span class="sxs-lookup"><span data-stu-id="2cda4-177">b.</span></span> <span data-ttu-id="2cda4-178">Válassza ki **Szolgáltatóazonosító szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-178">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="2cda4-179">c.</span><span class="sxs-lookup"><span data-stu-id="2cda4-179">c.</span></span> <span data-ttu-id="2cda4-180">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **bejelentkezési portál URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2cda4-180">Paste hello **SAML Single Sign-On Service URL** into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="2cda4-181">d.</span><span class="sxs-lookup"><span data-stu-id="2cda4-181">d.</span></span> <span data-ttu-id="2cda4-182">Beillesztés **egyetlen Sign-Out URL-címe** történő hello **Sign-Out URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2cda4-182">Paste **Single Sign-Out Service URL** into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="2cda4-183">e.</span><span class="sxs-lookup"><span data-stu-id="2cda4-183">e.</span></span> <span data-ttu-id="2cda4-184">Beillesztés **jelszó URL-cím módosítása** történő hello **jelszó URL-cím módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2cda4-184">Paste **Change Password URL** into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="2cda4-185">f.</span><span class="sxs-lookup"><span data-stu-id="2cda4-185">f.</span></span> <span data-ttu-id="2cda4-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2cda4-187">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2cda4-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2cda4-188">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2cda4-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2cda4-189">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2cda4-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2cda4-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cda4-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="2cda4-191">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2cda4-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2cda4-193">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2cda4-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cda4-194">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2cda4-196">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2cda4-198">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cda4-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2cda4-200">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2cda4-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workfront-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2cda4-202">a.</span><span class="sxs-lookup"><span data-stu-id="2cda4-202">a.</span></span> <span data-ttu-id="2cda4-203">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2cda4-204">b.</span><span class="sxs-lookup"><span data-stu-id="2cda4-204">b.</span></span> <span data-ttu-id="2cda4-205">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2cda4-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2cda4-206">c.</span><span class="sxs-lookup"><span data-stu-id="2cda4-206">c.</span></span> <span data-ttu-id="2cda4-207">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2cda4-208">d.</span><span class="sxs-lookup"><span data-stu-id="2cda4-208">d.</span></span> <span data-ttu-id="2cda4-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-209">Click **Create**.</span></span>
 
### <a name="creating-a-workfront-test-user"></a><span data-ttu-id="2cda4-210">Workfront tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2cda4-210">Creating a Workfront test user</span></span>

<span data-ttu-id="2cda4-211">hello ebben a szakaszban célja toocreate Workfront Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2cda4-211">hello objective of this section is toocreate a user called Britta Simon in Workfront.</span></span>

<span data-ttu-id="2cda4-212">**toocreate Britta Simon meghívta Workfront, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2cda4-212">**toocreate a user called Britta Simon in Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cda4-213">Bejelentkezés tooyour Workfront vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2cda4-213">Sign on tooyour Workfront company site as administrator.</span></span>
2. <span data-ttu-id="2cda4-214">Hello hello felső menüben kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-214">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="2cda4-215">Kattintson a **új személy**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-215">Click **New Person**.</span></span> 
4. <span data-ttu-id="2cda4-216">Hello új személy párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="2cda4-216">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Hozzon létre egy Workfront tesztfelhasználó számára][21] 
   
    <span data-ttu-id="2cda4-218">a.</span><span class="sxs-lookup"><span data-stu-id="2cda4-218">a.</span></span> <span data-ttu-id="2cda4-219">A hello **Keresztnév** szövegmező, írja be a "Britta."</span><span class="sxs-lookup"><span data-stu-id="2cda4-219">In hello **First Name** textbox, type "Britta."</span></span>
   
    <span data-ttu-id="2cda4-220">b.</span><span class="sxs-lookup"><span data-stu-id="2cda4-220">b.</span></span> <span data-ttu-id="2cda4-221">A hello **Vezetéknév** szövegmező, írja be a "Simon."</span><span class="sxs-lookup"><span data-stu-id="2cda4-221">In hello **Last Name** textbox, type "Simon."</span></span>
   
    <span data-ttu-id="2cda4-222">c.</span><span class="sxs-lookup"><span data-stu-id="2cda4-222">c.</span></span> <span data-ttu-id="2cda4-223">A hello **E-mail cím** szövegmező, írja be a Britta Simon e-mail címét az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2cda4-223">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="2cda4-224">d.</span><span class="sxs-lookup"><span data-stu-id="2cda4-224">d.</span></span> <span data-ttu-id="2cda4-225">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-225">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2cda4-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2cda4-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2cda4-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkfront megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="2cda4-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkfront.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2cda4-229">**tooassign Britta Simon tooWorkfront, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2cda4-229">**tooassign Britta Simon tooWorkfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="2cda4-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2cda4-232">Hello alkalmazások listában válassza ki a **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-232">In hello applications list, select **Workfront**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_app.png) 

3. <span data-ttu-id="2cda4-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2cda4-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2cda4-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2cda4-236">Click **Add** button.</span></span> <span data-ttu-id="2cda4-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cda4-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2cda4-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2cda4-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2cda4-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cda4-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2cda4-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2cda4-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2cda4-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2cda4-242">Testing single sign-on</span></span>

<span data-ttu-id="2cda4-243">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2cda4-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2cda4-244">Ha a hozzáférési Panel hello hello Workfront csempe gombra kattint, Workfront alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="2cda4-244">When you click hello Workfront tile in hello Access Panel, you should get login page of Workfront application.</span></span>
<span data-ttu-id="2cda4-245">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2cda4-245">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2cda4-246">További források</span><span class="sxs-lookup"><span data-stu-id="2cda4-246">Additional resources</span></span>

* [<span data-ttu-id="2cda4-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2cda4-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2cda4-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2cda4-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_04.png
[21]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_08.png
[23]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_06.png
[100]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_203.png

