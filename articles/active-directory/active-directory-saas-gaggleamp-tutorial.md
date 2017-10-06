---
title: "Oktatóanyag: Azure Active Directoryval integrált GaggleAMP |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és GaggleAMP között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9761019a79f935b6695a5ae1214c256c887df457
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a><span data-ttu-id="b609c-103">Oktatóanyag: Azure Active Directoryval integrált GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="b609c-103">Tutorial: Azure Active Directory integration with GaggleAMP</span></span>

<span data-ttu-id="b609c-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate GaggleAMP az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b609c-104">In this tutorial, you learn how toointegrate GaggleAMP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b609c-105">GaggleAMP integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b609c-105">Integrating GaggleAMP with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b609c-106">Megadhatja a hozzáférés tooGaggleAMP rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b609c-106">You can control in Azure AD who has access tooGaggleAMP</span></span>
- <span data-ttu-id="b609c-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGaggleAMP (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b609c-107">You can enable your users tooautomatically get signed-on tooGaggleAMP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b609c-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b609c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b609c-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b609c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b609c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b609c-110">Prerequisites</span></span>

<span data-ttu-id="b609c-111">az Azure AD integrálása GaggleAMP tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b609c-111">tooconfigure Azure AD integration with GaggleAMP, you need hello following items:</span></span>

- <span data-ttu-id="b609c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b609c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b609c-113">Egy GaggleAMP egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b609c-113">A GaggleAMP single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b609c-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b609c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b609c-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b609c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b609c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b609c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b609c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b609c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b609c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b609c-118">Scenario description</span></span>
<span data-ttu-id="b609c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b609c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b609c-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b609c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b609c-121">Hello gyűjteményből GaggleAMP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b609c-121">Adding GaggleAMP from hello gallery</span></span>
2. <span data-ttu-id="b609c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b609c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gaggleamp-from-hello-gallery"></a><span data-ttu-id="b609c-123">Hello gyűjteményből GaggleAMP hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b609c-123">Adding GaggleAMP from hello gallery</span></span>
<span data-ttu-id="b609c-124">tooconfigure hello integrációja GaggleAMP az Azure AD-be, meg kell tooadd GaggleAMP hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="b609c-124">tooconfigure hello integration of GaggleAMP into Azure AD, you need tooadd GaggleAMP from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b609c-125">**tooadd GaggleAMP hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b609c-125">**tooadd GaggleAMP from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b609c-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b609c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b609c-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b609c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b609c-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b609c-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b609c-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b609c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b609c-133">Hello keresési mezőbe, írja be a **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="b609c-133">In hello search box, type **GaggleAMP**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. <span data-ttu-id="b609c-135">A hello eredmények panelen válassza ki a **GaggleAMP**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b609c-135">In hello results panel, select **GaggleAMP**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b609c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b609c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b609c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="b609c-138">In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b609c-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó GaggleAMP tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b609c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in GaggleAMP is tooa user in Azure AD.</span></span> <span data-ttu-id="b609c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello GaggleAMP közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="b609c-140">In other words, a link relationship between an Azure AD user and hello related user in GaggleAMP needs toobe established.</span></span>

<span data-ttu-id="b609c-141">GaggleAMP, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b609c-141">In GaggleAMP, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b609c-142">tooconfigure és az Azure AD az egyszeri bejelentkezés GaggleAMP-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b609c-142">tooconfigure and test Azure AD single sign-on with GaggleAMP, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b609c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b609c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b609c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b609c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b609c-145">**[GaggleAMP tesztfelhasználó létrehozása](#creating-a-gaggleamp-test-user)**  -toohave egy megfelelője a Britta Simon a GaggleAMP, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="b609c-145">**[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - toohave a counterpart of Britta Simon in GaggleAMP that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b609c-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b609c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b609c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b609c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b609c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b609c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b609c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az GaggleAMP alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b609c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your GaggleAMP application.</span></span>

<span data-ttu-id="b609c-150">**az Azure AD tooconfigure egyszeri bejelentkezést a GaggleAMP, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b609c-150">**tooconfigure Azure AD single sign-on with GaggleAMP, perform hello following steps:**</span></span>

1. <span data-ttu-id="b609c-151">Az Azure portál, a hello hello **GaggleAMP** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b609c-151">In hello Azure portal, on hello **GaggleAMP** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b609c-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b609c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. <span data-ttu-id="b609c-155">A hello **GaggleAMP tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b609c-155">On hello **GaggleAMP Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     <span data-ttu-id="b609c-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.gaggleamp.com`</span><span class="sxs-lookup"><span data-stu-id="b609c-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.gaggleamp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b609c-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b609c-158">hello value is not real.</span></span> <span data-ttu-id="b609c-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b609c-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b609c-160">Ügyfél [GaggleAMP ügyfél-támogatási csoport](mailto:sales@gaggleamp.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="b609c-160">Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="b609c-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b609c-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

5. <span data-ttu-id="b609c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b609c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b609c-165">A hello **GaggleAMP konfigurációs** kattintson **konfigurálása GaggleAMP** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b609c-165">On hello **GaggleAMP Configuration** section, click **Configure GaggleAMP** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b609c-166">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b609c-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

7. <span data-ttu-id="b609c-168">Egy másik böngészőben példány, lépjen a SAML SSO lapján létrehozott hello Gaggle által támogatott team toohello (például: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span><span class="sxs-lookup"><span data-stu-id="b609c-168">In another browser instance, navigate toohello SAML SSO page created for you by hello Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span></span>

8. <span data-ttu-id="b609c-169">Az a **SAML SSO** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b609c-169">On your **SAML SSO** page, perform hello following steps:</span></span>  
   
    ![GaggleAMP egyszeri bejelentkezés](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 
 
    <span data-ttu-id="b609c-171">a.</span><span class="sxs-lookup"><span data-stu-id="b609c-171">a.</span></span> <span data-ttu-id="b609c-172">A hello **Identity Provider kibocsátó** szövegmezőhöz Beillesztés hello értékének **kiállítójának URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b609c-172">In hello **Identity Provider Issuer** textbox, paste hello value of **Issuer URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="b609c-173">b.</span><span class="sxs-lookup"><span data-stu-id="b609c-173">b.</span></span> <span data-ttu-id="b609c-174">A hello **Identity Provider egyetlen bejelentkezési URL-címet** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b609c-174">In hello **Identity Provider Single Sign-On URL** textbox, paste hello  value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="b609c-175">c.</span><span class="sxs-lookup"><span data-stu-id="b609c-175">c.</span></span> <span data-ttu-id="b609c-176">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="b609c-176">Click **Save**</span></span>      

    <span data-ttu-id="b609c-177">d.</span><span class="sxs-lookup"><span data-stu-id="b609c-177">d.</span></span> <span data-ttu-id="b609c-178">Hello küldése **tanúsítvány (Base64)** tooyour tanúsítvány [GaggleAMP támogatási csoport](mailto:sales@gaggleamp.com).</span><span class="sxs-lookup"><span data-stu-id="b609c-178">Send hello **Certificate (Base64)** certificate tooyour [GaggleAMP support team](mailto:sales@gaggleamp.com).</span></span>

> [!TIP]
> <span data-ttu-id="b609c-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b609c-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b609c-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b609c-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b609c-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b609c-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b609c-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b609c-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="b609c-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b609c-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b609c-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b609c-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b609c-186">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b609c-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b609c-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b609c-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b609c-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b609c-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b609c-192">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b609c-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b609c-194">a.</span><span class="sxs-lookup"><span data-stu-id="b609c-194">a.</span></span> <span data-ttu-id="b609c-195">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b609c-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b609c-196">b.</span><span class="sxs-lookup"><span data-stu-id="b609c-196">b.</span></span> <span data-ttu-id="b609c-197">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b609c-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b609c-198">c.</span><span class="sxs-lookup"><span data-stu-id="b609c-198">c.</span></span> <span data-ttu-id="b609c-199">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b609c-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b609c-200">d.</span><span class="sxs-lookup"><span data-stu-id="b609c-200">d.</span></span> <span data-ttu-id="b609c-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b609c-201">Click **Create**.</span></span>
 
### <a name="creating-a-gaggleamp-test-user"></a><span data-ttu-id="b609c-202">GaggleAMP tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b609c-202">Creating a GaggleAMP test user</span></span>

<span data-ttu-id="b609c-203">hello ebben a szakaszban célja toocreate GaggleAMP Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b609c-203">hello objective of this section is toocreate a user called Britta Simon in GaggleAMP.</span></span> <span data-ttu-id="b609c-204">GaggleAMP támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="b609c-204">GaggleAMP supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b609c-205">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="b609c-205">There is no action item for you in this section.</span></span> <span data-ttu-id="b609c-206">Új felhasználó jön létre egy kísérlet tooaccess GaggleAMP során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="b609c-206">A new user is created during an attempt tooaccess GaggleAMP if it doesn't exist yet.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b609c-207">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b609c-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b609c-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooGaggleAMP megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b609c-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGaggleAMP.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b609c-210">**tooassign Britta Simon tooGaggleAMP, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b609c-210">**tooassign Britta Simon tooGaggleAMP, perform hello following steps:**</span></span>

1. <span data-ttu-id="b609c-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b609c-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b609c-213">Hello alkalmazások listában válassza ki a **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="b609c-213">In hello applications list, select **GaggleAMP**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. <span data-ttu-id="b609c-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b609c-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b609c-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b609c-217">Click **Add** button.</span></span> <span data-ttu-id="b609c-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b609c-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b609c-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b609c-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b609c-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b609c-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b609c-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b609c-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b609c-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b609c-223">Testing single sign-on</span></span>

<span data-ttu-id="b609c-224">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="b609c-224">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="b609c-225">Ha a hozzáférési Panel hello hello GaggleAMP csempe gombra kattint, automatikusan bejelentkezett tooyour GaggleAMP alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b609c-225">When you click hello GaggleAMP tile in hello Access Panel, you should get automatically signed-on tooyour GaggleAMP application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b609c-226">További források</span><span class="sxs-lookup"><span data-stu-id="b609c-226">Additional resources</span></span>

* [<span data-ttu-id="b609c-227">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b609c-227">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b609c-228">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b609c-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png

