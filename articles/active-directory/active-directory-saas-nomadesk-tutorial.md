---
title: "Oktatóanyag: Azure Active Directoryval integrált Nomadesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Nomadesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d261b776-b48e-45f0-9722-0297adefabb8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 9aa55ba6106ea24f556217cfe84d21d7415d9c78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadesk"></a><span data-ttu-id="e5a51-103">Oktatóanyag: Azure Active Directoryval integrált Nomadesk</span><span class="sxs-lookup"><span data-stu-id="e5a51-103">Tutorial: Azure Active Directory integration with Nomadesk</span></span>

<span data-ttu-id="e5a51-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Nomadesk az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5a51-104">In this tutorial, you learn how toointegrate Nomadesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5a51-105">Nomadesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e5a51-105">Integrating Nomadesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e5a51-106">Megadhatja a hozzáférés tooNomadesk rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e5a51-106">You can control in Azure AD who has access tooNomadesk</span></span>
- <span data-ttu-id="e5a51-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNomadesk (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e5a51-107">You can enable your users tooautomatically get signed-on tooNomadesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5a51-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5a51-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e5a51-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5a51-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5a51-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5a51-110">Prerequisites</span></span>

<span data-ttu-id="e5a51-111">az Azure AD integrálása Nomadesk tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e5a51-111">tooconfigure Azure AD integration with Nomadesk, you need hello following items:</span></span>

- <span data-ttu-id="e5a51-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e5a51-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5a51-113">Egy Nomadesk egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e5a51-113">A Nomadesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5a51-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e5a51-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5a51-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e5a51-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5a51-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e5a51-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5a51-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5a51-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5a51-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e5a51-118">Scenario description</span></span>
<span data-ttu-id="e5a51-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e5a51-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5a51-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e5a51-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5a51-121">Hello gyűjteményből Nomadesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5a51-121">Adding Nomadesk from hello gallery</span></span>
2. <span data-ttu-id="e5a51-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5a51-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nomadesk-from-hello-gallery"></a><span data-ttu-id="e5a51-123">Hello gyűjteményből Nomadesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5a51-123">Adding Nomadesk from hello gallery</span></span>
<span data-ttu-id="e5a51-124">tooconfigure hello integrációja Nomadesk az Azure AD-be, meg kell tooadd Nomadesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e5a51-124">tooconfigure hello integration of Nomadesk into Azure AD, you need tooadd Nomadesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e5a51-125">**tooadd Nomadesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5a51-125">**tooadd Nomadesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5a51-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5a51-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e5a51-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e5a51-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e5a51-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e5a51-133">Hello keresési mezőbe, írja be a **Nomadesk**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-133">In hello search box, type **Nomadesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_search.png)

5. <span data-ttu-id="e5a51-135">A hello eredmények panelen válassza ki a **Nomadesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-135">In hello results panel, select **Nomadesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e5a51-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5a51-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e5a51-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Nomadesk.</span><span class="sxs-lookup"><span data-stu-id="e5a51-138">In this section, you configure and test Azure AD single sign-on with Nomadesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e5a51-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Nomadesk tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e5a51-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nomadesk is tooa user in Azure AD.</span></span> <span data-ttu-id="e5a51-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Nomadesk közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e5a51-140">In other words, a link relationship between an Azure AD user and hello related user in Nomadesk needs toobe established.</span></span>

<span data-ttu-id="e5a51-141">Nomadesk, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e5a51-141">In Nomadesk, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e5a51-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Nomadesk-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e5a51-142">tooconfigure and test Azure AD single sign-on with Nomadesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e5a51-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e5a51-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e5a51-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5a51-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5a51-145">**[Nomadesk tesztfelhasználó létrehozása](#creating-a-nomadesk-test-user)**  -toohave egy megfelelője a Britta Simon a Nomadesk, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="e5a51-145">**[Creating a Nomadesk test user](#creating-a-nomadesk-test-user)** - toohave a counterpart of Britta Simon in Nomadesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5a51-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5a51-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5a51-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e5a51-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e5a51-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5a51-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e5a51-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Nomadesk alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e5a51-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nomadesk application.</span></span>

<span data-ttu-id="e5a51-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Nomadesk, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e5a51-150">**tooconfigure Azure AD single sign-on with Nomadesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5a51-151">Az Azure portál, a hello hello **Nomadesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-151">In hello Azure portal, on hello **Nomadesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e5a51-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e5a51-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_samlbase.png)

3. <span data-ttu-id="e5a51-155">A hello **Nomadesk tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5a51-155">On hello **Nomadesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_url.png)

    <span data-ttu-id="e5a51-157">a.</span><span class="sxs-lookup"><span data-stu-id="e5a51-157">a.</span></span> <span data-ttu-id="e5a51-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://mynomadesk.com/logon/saml/<TENANTID>`</span><span class="sxs-lookup"><span data-stu-id="e5a51-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mynomadesk.com/logon/saml/<TENANTID>`</span></span>

    <span data-ttu-id="e5a51-159">b.</span><span class="sxs-lookup"><span data-stu-id="e5a51-159">b.</span></span> <span data-ttu-id="e5a51-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://secure.nomadesk.com/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="e5a51-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://secure.nomadesk.com/saml/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5a51-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e5a51-161">These values are not real.</span></span> <span data-ttu-id="e5a51-162">Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e5a51-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="e5a51-163">Ügyfél [Nomadesk ügyfél-támogatási csoport](mailto:support@nomadesk.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e5a51-163">Contact [Nomadesk Client support team](mailto:support@nomadesk.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="e5a51-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e5a51-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_certificate.png) 

5. <span data-ttu-id="e5a51-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e5a51-168">A hello **Nomadesk konfigurációs** kattintson **konfigurálása Nomadesk** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e5a51-168">On hello **Nomadesk Configuration** section, click **Configure Nomadesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e5a51-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e5a51-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_configure.png) 

7. <span data-ttu-id="e5a51-171">tooconfigure egyszeri bejelentkezést a **Nomadesk** oldalon kell letöltött toosend hello **tanúsítvány**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[Nomadesk támogatási csoport](mailto:support@nomadesk.com).</span><span class="sxs-lookup"><span data-stu-id="e5a51-171">tooconfigure single sign-on on **Nomadesk** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Nomadesk support team](mailto:support@nomadesk.com).</span></span> <span data-ttu-id="e5a51-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="e5a51-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e5a51-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e5a51-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e5a51-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e5a51-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e5a51-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5a51-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e5a51-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5a51-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="e5a51-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e5a51-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e5a51-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e5a51-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5a51-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5a51-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5a51-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5a51-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5a51-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5a51-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5a51-188">a.</span><span class="sxs-lookup"><span data-stu-id="e5a51-188">a.</span></span> <span data-ttu-id="e5a51-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5a51-190">b.</span><span class="sxs-lookup"><span data-stu-id="e5a51-190">b.</span></span> <span data-ttu-id="e5a51-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5a51-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5a51-192">c.</span><span class="sxs-lookup"><span data-stu-id="e5a51-192">c.</span></span> <span data-ttu-id="e5a51-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e5a51-194">d.</span><span class="sxs-lookup"><span data-stu-id="e5a51-194">d.</span></span> <span data-ttu-id="e5a51-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-195">Click **Create**.</span></span>
 
### <a name="creating-a-nomadesk-test-user"></a><span data-ttu-id="e5a51-196">Nomadesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5a51-196">Creating a Nomadesk test user</span></span>

<span data-ttu-id="e5a51-197">hello ebben a szakaszban célja toocreate Nomadesk Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e5a51-197">hello objective of this section is toocreate a user called Britta Simon in Nomadesk.</span></span> <span data-ttu-id="e5a51-198">Nomadesk támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="e5a51-198">Nomadesk supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="e5a51-199">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="e5a51-199">There is no action item for you in this section.</span></span> <span data-ttu-id="e5a51-200">Új felhasználó jön létre egy kísérlet tooaccess Nomadesk során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e5a51-200">A new user is created during an attempt tooaccess Nomadesk if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="e5a51-201">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Nomadesk támogatási csoport](mailto:support@nomadesk.com).</span><span class="sxs-lookup"><span data-stu-id="e5a51-201">If you need toocreate a user manually, you need toocontact hello [Nomadesk support team](mailto:support@nomadesk.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e5a51-202">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e5a51-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e5a51-203">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNomadesk megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e5a51-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNomadesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e5a51-205">**tooassign Britta Simon tooNomadesk, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="e5a51-205">**tooassign Britta Simon tooNomadesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="e5a51-206">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-206">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e5a51-208">Hello alkalmazások listában válassza ki a **Nomadesk**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-208">In hello applications list, select **Nomadesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_app.png) 

3. <span data-ttu-id="e5a51-210">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e5a51-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e5a51-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5a51-212">Click **Add** button.</span></span> <span data-ttu-id="e5a51-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5a51-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e5a51-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e5a51-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e5a51-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5a51-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5a51-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5a51-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e5a51-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e5a51-218">Testing single sign-on</span></span>

<span data-ttu-id="e5a51-219">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e5a51-219">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e5a51-220">Ha a hozzáférési Panel hello hello Nomadesk csempe gombra kattint, automatikusan bejelentkezett tooyour Nomadesk alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e5a51-220">When you click hello Nomadesk tile in hello Access Panel,  you should get automatically signed-on tooyour Nomadesk application.</span></span>
<span data-ttu-id="e5a51-221">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e5a51-221">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5a51-222">További források</span><span class="sxs-lookup"><span data-stu-id="e5a51-222">Additional resources</span></span>

* [<span data-ttu-id="e5a51-223">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e5a51-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5a51-224">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e5a51-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_203.png

