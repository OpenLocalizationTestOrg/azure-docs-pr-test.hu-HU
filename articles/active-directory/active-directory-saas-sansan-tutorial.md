---
title: "Oktatóanyag: Azure Active Directoryval integrált Sansan |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Sansan között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: f58cc613a2e3a240e555b61a34db4155eb9dff71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a><span data-ttu-id="05b57-103">Oktatóanyag: Azure Active Directoryval integrált Sansan</span><span class="sxs-lookup"><span data-stu-id="05b57-103">Tutorial: Azure Active Directory integration with Sansan</span></span>

<span data-ttu-id="05b57-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Sansan az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="05b57-104">In this tutorial, you learn how toointegrate Sansan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="05b57-105">Sansan integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="05b57-105">Integrating Sansan with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="05b57-106">Megadhatja a hozzáférés tooSansan rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="05b57-106">You can control in Azure AD who has access tooSansan</span></span>
- <span data-ttu-id="05b57-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSansan (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="05b57-107">You can enable your users tooautomatically get signed-on tooSansan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="05b57-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="05b57-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="05b57-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="05b57-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05b57-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05b57-110">Prerequisites</span></span>

<span data-ttu-id="05b57-111">az Azure AD integrálása Sansan tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="05b57-111">tooconfigure Azure AD integration with Sansan, you need hello following items:</span></span>

- <span data-ttu-id="05b57-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="05b57-112">An Azure AD subscription</span></span>
- <span data-ttu-id="05b57-113">Egy Sansan egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="05b57-113">A Sansan single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="05b57-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="05b57-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="05b57-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="05b57-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="05b57-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="05b57-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="05b57-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="05b57-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="05b57-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="05b57-118">Scenario description</span></span>
<span data-ttu-id="05b57-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="05b57-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="05b57-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="05b57-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="05b57-121">Hello gyűjteményből Sansan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="05b57-121">Adding Sansan from hello gallery</span></span>
2. <span data-ttu-id="05b57-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="05b57-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sansan-from-hello-gallery"></a><span data-ttu-id="05b57-123">Hello gyűjteményből Sansan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="05b57-123">Adding Sansan from hello gallery</span></span>
<span data-ttu-id="05b57-124">tooconfigure hello integrációja Sansan az Azure AD-be, meg kell tooadd Sansan hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="05b57-124">tooconfigure hello integration of Sansan into Azure AD, you need tooadd Sansan from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="05b57-125">**tooadd Sansan hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="05b57-125">**tooadd Sansan from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="05b57-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="05b57-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="05b57-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="05b57-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="05b57-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="05b57-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="05b57-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="05b57-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="05b57-133">Hello keresési mezőbe, írja be a **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="05b57-133">In hello search box, type **Sansan**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_search.png)

5. <span data-ttu-id="05b57-135">A hello eredmények panelen válassza ki a **Sansan**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="05b57-135">In hello results panel, select **Sansan**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="05b57-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="05b57-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="05b57-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Sansan.</span><span class="sxs-lookup"><span data-stu-id="05b57-138">In this section, you configure and test Azure AD single sign-on with Sansan based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="05b57-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Sansan tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="05b57-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sansan is tooa user in Azure AD.</span></span> <span data-ttu-id="05b57-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Sansan közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="05b57-140">In other words, a link relationship between an Azure AD user and hello related user in Sansan needs toobe established.</span></span>

<span data-ttu-id="05b57-141">Sansan, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="05b57-141">In Sansan, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="05b57-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Sansan-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="05b57-142">tooconfigure and test Azure AD single sign-on with Sansan, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="05b57-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="05b57-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="05b57-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="05b57-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="05b57-145">**[Sansan tesztfelhasználó létrehozása](#creating-a-sansan-test-user)**  -toohave egy megfelelője a Britta Simon a Sansan, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="05b57-145">**[Creating a Sansan test user](#creating-a-sansan-test-user)** - toohave a counterpart of Britta Simon in Sansan that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="05b57-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="05b57-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="05b57-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="05b57-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="05b57-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="05b57-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="05b57-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Sansan alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="05b57-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sansan application.</span></span>

<span data-ttu-id="05b57-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Sansan, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="05b57-150">**tooconfigure Azure AD single sign-on with Sansan, perform hello following steps:**</span></span>

1. <span data-ttu-id="05b57-151">Az Azure portál, a hello hello **Sansan** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="05b57-151">In hello Azure portal, on hello **Sansan** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="05b57-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="05b57-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_samlbase.png)

3. <span data-ttu-id="05b57-155">A hello **Sansan tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="05b57-155">On hello **Sansan Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_url.png)

    <span data-ttu-id="05b57-157">a.</span><span class="sxs-lookup"><span data-stu-id="05b57-157">a.</span></span> <span data-ttu-id="05b57-158">A hello **bejelentkezési URL-cím** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="05b57-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | <span data-ttu-id="05b57-159">Környezet</span><span class="sxs-lookup"><span data-stu-id="05b57-159">Environment</span></span> | <span data-ttu-id="05b57-160">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="05b57-160">URL</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="05b57-161">PC-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="05b57-161">PC web</span></span> |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | <span data-ttu-id="05b57-162">Natív mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="05b57-162">Native Mobile app</span></span> |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | <span data-ttu-id="05b57-163">Mobil böngésző beállításai</span><span class="sxs-lookup"><span data-stu-id="05b57-163">Mobile browser settings</span></span> |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    <span data-ttu-id="05b57-164">b.</span><span class="sxs-lookup"><span data-stu-id="05b57-164">b.</span></span> <span data-ttu-id="05b57-165">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="05b57-165">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>
    | <span data-ttu-id="05b57-166">Környezet</span><span class="sxs-lookup"><span data-stu-id="05b57-166">Environment</span></span>             | <span data-ttu-id="05b57-167">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="05b57-167">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="05b57-168">PC-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="05b57-168">PC web</span></span>                  | `https://ap.sansan.com/v/saml2/<company name>`|
    | <span data-ttu-id="05b57-169">Natív mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="05b57-169">Native Mobile app</span></span>       | `https://internal.api.sansan.com/saml2/<company name>` |
    | <span data-ttu-id="05b57-170">Mobil böngésző beállításai</span><span class="sxs-lookup"><span data-stu-id="05b57-170">Mobile browser settings</span></span> | `https://ap.sansan.com/s/saml2/<company name>` |

    > [!NOTE] 
    > <span data-ttu-id="05b57-171">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="05b57-171">These values are not real.</span></span> <span data-ttu-id="05b57-172">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="05b57-172">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="05b57-173">Ügyfél [Sansan ügyfél-támogatási csoport](https://www.sansan.com/form/contact) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="05b57-173">Contact [Sansan Client support team](https://www.sansan.com/form/contact) tooget these values.</span></span> 

4. <span data-ttu-id="05b57-174">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="05b57-174">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_certificate.png) 

5. <span data-ttu-id="05b57-176">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="05b57-176">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="05b57-178">A hello **Sansan konfigurációs** kattintson **konfigurálása Sansan** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="05b57-178">On hello **Sansan Configuration** section, click **Configure Sansan** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="05b57-179">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="05b57-179">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_configure.png) 

7. <span data-ttu-id="05b57-181">tooconfigure egyszeri bejelentkezést a **Sansan** oldalon kell letöltött toosend hello **tanúsítvány**, **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[Sansan támogatási csoport](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="05b57-181">tooconfigure single sign-on on **Sansan** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** too[Sansan support team](https://www.sansan.com/form/contact).</span></span> <span data-ttu-id="05b57-182">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="05b57-182">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="05b57-183">PC-böngészőbeállítások a mobilalkalmazás és a mobil böngésző PC webes együtt is működik.</span><span class="sxs-lookup"><span data-stu-id="05b57-183">PC browser setting also work for Mobile app and Mobile browser along with PC web.</span></span>  

> [!TIP]
> <span data-ttu-id="05b57-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="05b57-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="05b57-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="05b57-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="05b57-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="05b57-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="05b57-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="05b57-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="05b57-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="05b57-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="05b57-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="05b57-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="05b57-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="05b57-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="05b57-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="05b57-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="05b57-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05b57-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="05b57-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="05b57-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="05b57-199">a.</span><span class="sxs-lookup"><span data-stu-id="05b57-199">a.</span></span> <span data-ttu-id="05b57-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="05b57-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="05b57-201">b.</span><span class="sxs-lookup"><span data-stu-id="05b57-201">b.</span></span> <span data-ttu-id="05b57-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="05b57-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="05b57-203">c.</span><span class="sxs-lookup"><span data-stu-id="05b57-203">c.</span></span> <span data-ttu-id="05b57-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="05b57-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="05b57-205">d.</span><span class="sxs-lookup"><span data-stu-id="05b57-205">d.</span></span> <span data-ttu-id="05b57-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="05b57-206">Click **Create**.</span></span>
 
### <a name="creating-a-sansan-test-user"></a><span data-ttu-id="05b57-207">Sansan tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="05b57-207">Creating a Sansan test user</span></span>

<span data-ttu-id="05b57-208">Ebben a szakaszban egy SanSan Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="05b57-208">In this section, you create a user called Britta Simon in SanSan.</span></span> <span data-ttu-id="05b57-209">SanSan alkalmazást kell hello felhasználói toobe hello alkalmazás egyszeri bejelentkezési előtte kiépítve.</span><span class="sxs-lookup"><span data-stu-id="05b57-209">SanSan application needs hello user toobe provisioned in hello application before doing SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="05b57-210">Ha manuálisan kell toocreate egy felhasználó vagy a batch-felhasználók, akkor kell toocontact hello [Sansan támogatási csoport](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="05b57-210">If you need toocreate a user manually or batch of users, you need toocontact hello [Sansan support team](https://www.sansan.com/form/contact).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="05b57-211">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="05b57-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="05b57-212">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSansan megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="05b57-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSansan.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="05b57-214">**tooassign Britta Simon tooSansan, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="05b57-214">**tooassign Britta Simon tooSansan, perform hello following steps:**</span></span>

1. <span data-ttu-id="05b57-215">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="05b57-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="05b57-217">Hello alkalmazások listában válassza ki a **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="05b57-217">In hello applications list, select **Sansan**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_app.png) 

3. <span data-ttu-id="05b57-219">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="05b57-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="05b57-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="05b57-221">Click **Add** button.</span></span> <span data-ttu-id="05b57-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05b57-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="05b57-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="05b57-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="05b57-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05b57-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="05b57-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05b57-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="05b57-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="05b57-227">Testing single sign-on</span></span>

<span data-ttu-id="05b57-228">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="05b57-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="05b57-229">Ha a hozzáférési Panel hello hello Sansan csempe gombra kattint, automatikusan bejelentkezett tooyour Sansan alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="05b57-229">When you click hello Sansan tile in hello Access Panel, you should get automatically signed-on tooyour Sansan application.</span></span>
<span data-ttu-id="05b57-230">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="05b57-230">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="05b57-231">További források</span><span class="sxs-lookup"><span data-stu-id="05b57-231">Additional resources</span></span>

* [<span data-ttu-id="05b57-232">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="05b57-232">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="05b57-233">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="05b57-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
