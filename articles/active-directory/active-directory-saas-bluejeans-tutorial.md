---
title: "Oktatóanyag: Azure Active Directoryval integrált BlueJeans |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BlueJeans között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="b1b87-103">Oktatóanyag: Azure Active Directoryval integrált BlueJeans</span><span class="sxs-lookup"><span data-stu-id="b1b87-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="b1b87-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BlueJeans az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b1b87-104">In this tutorial, you learn how toointegrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1b87-105">BlueJeans integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-105">Integrating BlueJeans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b1b87-106">Megadhatja a hozzáférés tooBlueJeans rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b1b87-106">You can control in Azure AD who has access tooBlueJeans</span></span>
- <span data-ttu-id="b1b87-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBlueJeans (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b1b87-107">You can enable your users tooautomatically get signed-on tooBlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1b87-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b1b87-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b1b87-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1b87-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1b87-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b1b87-110">Prerequisites</span></span>

<span data-ttu-id="b1b87-111">az Azure AD integrálása BlueJeans tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-111">tooconfigure Azure AD integration with BlueJeans, you need hello following items:</span></span>

- <span data-ttu-id="b1b87-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b1b87-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1b87-113">Egy BlueJeans egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b1b87-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1b87-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b1b87-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1b87-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b1b87-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1b87-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1b87-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1b87-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1b87-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1b87-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b1b87-118">Scenario description</span></span>
<span data-ttu-id="b1b87-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b1b87-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1b87-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b1b87-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1b87-121">Hello gyűjteményből BlueJeans hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1b87-121">Adding BlueJeans from hello gallery</span></span>
2. <span data-ttu-id="b1b87-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1b87-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-hello-gallery"></a><span data-ttu-id="b1b87-123">Hello gyűjteményből BlueJeans hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1b87-123">Adding BlueJeans from hello gallery</span></span>
<span data-ttu-id="b1b87-124">tooconfigure hello integrációja BlueJeans az Azure AD-be, meg kell tooadd BlueJeans hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b1b87-124">tooconfigure hello integration of BlueJeans into Azure AD, you need tooadd BlueJeans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b1b87-125">**tooadd BlueJeans hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b1b87-125">**tooadd BlueJeans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1b87-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1b87-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b1b87-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b1b87-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b1b87-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b1b87-133">Hello keresési mezőbe, írja be a **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-133">In hello search box, type **BlueJeans**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="b1b87-135">A hello eredmények panelen válassza ki a **BlueJeans**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-135">In hello results panel, select **BlueJeans**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1b87-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b1b87-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1b87-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BlueJeans</span><span class="sxs-lookup"><span data-stu-id="b1b87-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b1b87-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BlueJeans tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b1b87-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BlueJeans is tooa user in Azure AD.</span></span> <span data-ttu-id="b1b87-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BlueJeans közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b1b87-140">In other words, a link relationship between an Azure AD user and hello related user in BlueJeans needs toobe established.</span></span>

<span data-ttu-id="b1b87-141">BlueJeans, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b1b87-141">In BlueJeans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b1b87-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BlueJeans-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b1b87-142">tooconfigure and test Azure AD single sign-on with BlueJeans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b1b87-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b1b87-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b1b87-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1b87-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1b87-145">**[BlueJeans tesztfelhasználó létrehozása](#creating-a-bluejeans-test-user)**  -toohave egy megfelelője a Britta Simon BlueJeans az, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b1b87-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - toohave a counterpart of Britta Simon in BlueJeans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b1b87-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b1b87-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1b87-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b1b87-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1b87-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b1b87-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1b87-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BlueJeans alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b1b87-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="b1b87-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BlueJeans, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b1b87-150">**tooconfigure Azure AD single sign-on with BlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1b87-151">Az Azure portál, a hello hello **BlueJeans** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-151">In hello Azure portal, on hello **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b1b87-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b1b87-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="b1b87-155">A hello **BlueJeans tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-155">On hello **BlueJeans Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="b1b87-157">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-157">a.</span></span> <span data-ttu-id="b1b87-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="b1b87-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="b1b87-159">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-159">b.</span></span> <span data-ttu-id="b1b87-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="b1b87-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1b87-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b1b87-161">These values are not real.</span></span> <span data-ttu-id="b1b87-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="b1b87-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b1b87-163">Ügyfél [BlueJeans ügyfél-támogatási csoport](https://support.bluejeans.com/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b1b87-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="b1b87-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b1b87-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="b1b87-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b1b87-168">A hello **BlueJeans konfigurációs** kattintson **konfigurálása BlueJeans** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b1b87-168">On hello **BlueJeans Configuration** section, click **Configure BlueJeans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b1b87-169">Másolás hello **Sign-Out URL-cím, jelszó URL-cím módosítása és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b1b87-169">Copy hello **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="b1b87-171">Egy másik webes böngészőablakban, jelentkezzen be tooyour **BlueJeans** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b1b87-171">In a different web browser window, log in tooyour **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="b1b87-172">Nyissa meg túl**ADMIN \> Csoportbeállításokban \> biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-172">Go too**ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="b1b87-173">![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="b1b87-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="b1b87-174">A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-174">In hello **Security** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="b1b87-175">![SAML-alapú egyszeri bejelentkezés](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML-alapú egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="b1b87-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="b1b87-176">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-176">a.</span></span> <span data-ttu-id="b1b87-177">Válassza ki **SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="b1b87-178">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-178">b.</span></span> <span data-ttu-id="b1b87-179">Válassza ki **automatikus kiépítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="b1b87-180">Helyezze át, amely az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-180">Move on with hello following steps:</span></span>

    <span data-ttu-id="b1b87-181">![Tanúsítvány-elérési út](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "tanúsítvány elérési útja")</span><span class="sxs-lookup"><span data-stu-id="b1b87-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="b1b87-182">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-182">a.</span></span> <span data-ttu-id="b1b87-183">Kattintson a **Choose File**, majd töltse fel a letöltött hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b1b87-183">Click **Choose File**, and then upload hello downloaded certificate.</span></span>
   
    <span data-ttu-id="b1b87-184">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-184">b.</span></span> <span data-ttu-id="b1b87-185">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b1b87-185">Paste **SAML Single Sign-On Service URL** into hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="b1b87-186">c.</span><span class="sxs-lookup"><span data-stu-id="b1b87-186">c.</span></span> <span data-ttu-id="b1b87-187">Beillesztés **jelszó URL-cím módosítása** történő hello **URL-cím jelszó módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b1b87-187">Paste **Change Password URL** into hello **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="b1b87-188">d.</span><span class="sxs-lookup"><span data-stu-id="b1b87-188">d.</span></span> <span data-ttu-id="b1b87-189">Beillesztés **Sign-Out URL-cím** történő hello **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b1b87-189">Paste **Sign-Out URL** into hello **Logout URL** textbox.</span></span>

11. <span data-ttu-id="b1b87-190">Helyezze át, amely az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-190">Move on with hello following steps:</span></span>
    
    <span data-ttu-id="b1b87-191">![Módosítások mentése](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "módosítások mentése")</span><span class="sxs-lookup"><span data-stu-id="b1b87-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="b1b87-192">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-192">a.</span></span> <span data-ttu-id="b1b87-193">A hello **felhasználóazonosító** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="b1b87-193">In hello **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="b1b87-194">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-194">b.</span></span> <span data-ttu-id="b1b87-195">A hello **E-mail** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="b1b87-195">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="b1b87-196">c.</span><span class="sxs-lookup"><span data-stu-id="b1b87-196">c.</span></span> <span data-ttu-id="b1b87-197">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b1b87-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b1b87-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b1b87-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b1b87-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b1b87-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b1b87-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1b87-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1b87-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1b87-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b1b87-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b1b87-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b1b87-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1b87-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b1b87-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1b87-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1b87-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1b87-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1b87-213">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-213">a.</span></span> <span data-ttu-id="b1b87-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1b87-215">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-215">b.</span></span> <span data-ttu-id="b1b87-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1b87-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1b87-217">c.</span><span class="sxs-lookup"><span data-stu-id="b1b87-217">c.</span></span> <span data-ttu-id="b1b87-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b1b87-219">d.</span><span class="sxs-lookup"><span data-stu-id="b1b87-219">d.</span></span> <span data-ttu-id="b1b87-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="b1b87-221">BlueJeans tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1b87-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="b1b87-222">az Azure AD tooenable felhasználók toolog a tooBlueJeans, akkor ki kell építenie BlueJeans be.</span><span class="sxs-lookup"><span data-stu-id="b1b87-222">tooenable Azure AD users toolog in tooBlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="b1b87-223">BlueJeans, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b1b87-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="b1b87-224">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b1b87-224">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1b87-225">Jelentkezzen be tooyour **BlueJeans** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b1b87-225">Log in tooyour **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="b1b87-226">Nyissa meg túl**ADMIN \> felhasználók kezelése \> felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-226">Go too**ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="b1b87-227">![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="b1b87-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="b1b87-228">Hello **felhasználó hozzáadása** lap csak akkor használható, ha hello **Biztonság lap**, **automatikus kiépítés engedélyezéséhez** nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="b1b87-228">hello **Add User** tab is only available if, in hello **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="b1b87-229">A hello **felhasználó hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b1b87-229">In hello **Add User** section, perform hello following steps:</span></span>

    <span data-ttu-id="b1b87-230">![Felhasználó hozzáadása](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b1b87-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="b1b87-231">a.</span><span class="sxs-lookup"><span data-stu-id="b1b87-231">a.</span></span> <span data-ttu-id="b1b87-232">Adjon meg egy **BlueJeans felhasználónév**, egy **E-mail cím**, egy **BlueJeans értekezlet azonosító**, egy **moderátor PIN-kód**, egy **teljes neve** , hello **vállalati** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="b1b87-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, hello **Company** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
    
    <span data-ttu-id="b1b87-233">b.</span><span class="sxs-lookup"><span data-stu-id="b1b87-233">b.</span></span> <span data-ttu-id="b1b87-234">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="b1b87-235">Bármely más BlueJeans felhasználói fiók létrehozása eszközök vagy BlueJeans tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="b1b87-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b1b87-236">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b1b87-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b1b87-237">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBlueJeans megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b1b87-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlueJeans.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b1b87-239">**tooassign Britta Simon tooBlueJeans, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b1b87-239">**tooassign Britta Simon tooBlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="b1b87-240">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b1b87-242">Hello alkalmazások listában válassza ki a **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-242">In hello applications list, select **BlueJeans**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="b1b87-244">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b1b87-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b1b87-246">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b1b87-246">Click **Add** button.</span></span> <span data-ttu-id="b1b87-247">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1b87-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b1b87-249">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b1b87-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b1b87-250">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1b87-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1b87-251">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b1b87-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1b87-252">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b1b87-252">Testing single sign-on</span></span>

<span data-ttu-id="b1b87-253">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b1b87-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b1b87-254">Hello BlueJeans hello hozzáférési Panel csempére kattintva BlueJeans alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b1b87-254">When you click hello BlueJeans tile in hello Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="b1b87-255">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b1b87-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b1b87-256">További források</span><span class="sxs-lookup"><span data-stu-id="b1b87-256">Additional resources</span></span>

* [<span data-ttu-id="b1b87-257">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b1b87-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1b87-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b1b87-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

