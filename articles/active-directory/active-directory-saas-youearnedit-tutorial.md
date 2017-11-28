---
title: "Oktatóanyag: Azure Active Directoryval integrált YouEarnedIt |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és YouEarnedIt között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: cc9a8ae2f92751cf3fadbeec23c8319c83728a33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a><span data-ttu-id="1dafd-103">Oktatóanyag: Azure Active Directoryval integrált YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="1dafd-103">Tutorial: Azure Active Directory integration with YouEarnedIt</span></span>

<span data-ttu-id="1dafd-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate YouEarnedIt az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1dafd-104">In this tutorial, you learn how toointegrate YouEarnedIt with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1dafd-105">YouEarnedIt integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="1dafd-105">Integrating YouEarnedIt with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1dafd-106">Az Azure AD hozzáférési tooYouEarnedIt rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="1dafd-106">You can control in Azure AD who has access tooYouEarnedIt.</span></span>
- <span data-ttu-id="1dafd-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooYouEarnedIt (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="1dafd-107">You can enable your users tooautomatically get signed-on tooYouEarnedIt (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1dafd-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="1dafd-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="1dafd-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1dafd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dafd-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1dafd-110">Prerequisites</span></span>

<span data-ttu-id="1dafd-111">az Azure AD integrálása YouEarnedIt tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1dafd-111">tooconfigure Azure AD integration with YouEarnedIt, you need hello following items:</span></span>

- <span data-ttu-id="1dafd-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1dafd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1dafd-113">Egy YouEarnedIt egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1dafd-113">A YouEarnedIt single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1dafd-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1dafd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1dafd-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="1dafd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1dafd-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1dafd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1dafd-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1dafd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1dafd-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1dafd-118">Scenario description</span></span>
<span data-ttu-id="1dafd-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1dafd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1dafd-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1dafd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1dafd-121">Hello gyűjteményből YouEarnedIt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1dafd-121">Adding YouEarnedIt from hello gallery</span></span>
2. <span data-ttu-id="1dafd-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1dafd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-youearnedit-from-hello-gallery"></a><span data-ttu-id="1dafd-123">Hello gyűjteményből YouEarnedIt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1dafd-123">Adding YouEarnedIt from hello gallery</span></span>
<span data-ttu-id="1dafd-124">tooconfigure hello integrációja YouEarnedIt az Azure AD-be, meg kell tooadd YouEarnedIt hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="1dafd-124">tooconfigure hello integration of YouEarnedIt into Azure AD, you need tooadd YouEarnedIt from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1dafd-125">**tooadd YouEarnedIt hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1dafd-125">**tooadd YouEarnedIt from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1dafd-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="1dafd-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1dafd-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="1dafd-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="1dafd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="1dafd-133">Hello keresési mezőbe, írja be a **YouEarnedt**, jelölje be **YouEarnedt** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-133">In hello search box, type **YouEarnedt**, select  **YouEarnedt**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában YouEarnedIt](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1dafd-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1dafd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1dafd-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="1dafd-136">In this section, you configure and test Azure AD single sign-on with YouEarnedIt based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1dafd-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó YouEarnedIt tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1dafd-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in YouEarnedIt is tooa user in Azure AD.</span></span> <span data-ttu-id="1dafd-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello YouEarnedIt közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="1dafd-138">In other words, a link relationship between an Azure AD user and hello related user in YouEarnedIt needs toobe established.</span></span>

<span data-ttu-id="1dafd-139">YouEarnedIt, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1dafd-139">In YouEarnedIt, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1dafd-140">tooconfigure és az Azure AD az egyszeri bejelentkezés YouEarnedIt-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="1dafd-140">tooconfigure and test Azure AD single sign-on with YouEarnedIt, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1dafd-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1dafd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1dafd-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1dafd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1dafd-143">**[YouEarnedIt tesztfelhasználó létrehozása](#create-a-youearnedit-test-user)**  -toohave egy megfelelője a Britta Simon a YouEarnedIt, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="1dafd-143">**[Create a YouEarnedIt test user](#create-a-youearnedit-test-user)** - toohave a counterpart of Britta Simon in YouEarnedIt that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1dafd-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1dafd-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1dafd-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="1dafd-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1dafd-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1dafd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1dafd-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az YouEarnedIt alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1dafd-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your YouEarnedIt application.</span></span>

<span data-ttu-id="1dafd-148">**az Azure AD tooconfigure egyszeri bejelentkezést a YouEarnedIt, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="1dafd-148">**tooconfigure Azure AD single sign-on with YouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="1dafd-149">Az Azure portál, a hello hello **YouEarnedIt** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-149">In hello Azure portal, on hello **YouEarnedIt** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="1dafd-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="1dafd-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. <span data-ttu-id="1dafd-153">A hello **YouEarnedIt tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1dafd-153">On hello **YouEarnedIt Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk YouEarnedIt tartomány és az URL-címek](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_url.png)

    <span data-ttu-id="1dafd-155">a.</span><span class="sxs-lookup"><span data-stu-id="1dafd-155">a.</span></span> <span data-ttu-id="1dafd-156">A hello **bejelentkezési URL-cím** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="1dafd-156">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    | <span data-ttu-id="1dafd-157">Környezet</span><span class="sxs-lookup"><span data-stu-id="1dafd-157">Environment</span></span>  | <span data-ttu-id="1dafd-158">Minta</span><span class="sxs-lookup"><span data-stu-id="1dafd-158">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="1dafd-159">Éles</span><span class="sxs-lookup"><span data-stu-id="1dafd-159">Production</span></span> | `https://<company name>.youearnedit.com/users/sign_in` |
    | <span data-ttu-id="1dafd-160">A védőfal</span><span class="sxs-lookup"><span data-stu-id="1dafd-160">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    <span data-ttu-id="1dafd-161">b.</span><span class="sxs-lookup"><span data-stu-id="1dafd-161">b.</span></span> <span data-ttu-id="1dafd-162">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="1dafd-162">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>
    | <span data-ttu-id="1dafd-163">Környezet</span><span class="sxs-lookup"><span data-stu-id="1dafd-163">Environment</span></span>  | <span data-ttu-id="1dafd-164">Minta</span><span class="sxs-lookup"><span data-stu-id="1dafd-164">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="1dafd-165">Éles</span><span class="sxs-lookup"><span data-stu-id="1dafd-165">Production</span></span> | `https://<company name>.youearnedit.com` |
    | <span data-ttu-id="1dafd-166">A védőfal</span><span class="sxs-lookup"><span data-stu-id="1dafd-166">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > <span data-ttu-id="1dafd-167">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1dafd-167">These values are not real.</span></span> <span data-ttu-id="1dafd-168">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="1dafd-168">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1dafd-169">Ügyfél [YouEarnedIt ügyfél-támogatási csoport](https://youearnedit.freshdesk.com/support/tickets/new) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1dafd-169">Contact [YouEarnedIt Client support team](https://youearnedit.freshdesk.com/support/tickets/new) tooget these values.</span></span> 
 
4. <span data-ttu-id="1dafd-170">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1dafd-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. <span data-ttu-id="1dafd-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-172">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-youearnedit-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1dafd-174">A hello **YouEarnedIt konfigurációs** kattintson **konfigurálása YouEarnedIt** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1dafd-174">On hello **YouEarnedIt Configuration** section, click **Configure YouEarnedIt** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1dafd-175">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1dafd-175">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![YouEarnedIt konfiguráció](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. <span data-ttu-id="1dafd-177">tooconfigure egyszeri bejelentkezést a **YouEarnedIt** oldalon kell letöltött toosend hello **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[YouEarnedIt támogatási csoport](https://youearnedit.freshdesk.com/support/tickets/new).</span><span class="sxs-lookup"><span data-stu-id="1dafd-177">tooconfigure single sign-on on **YouEarnedIt** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[YouEarnedIt support team](https://youearnedit.freshdesk.com/support/tickets/new).</span></span> <span data-ttu-id="1dafd-178">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="1dafd-178">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="1dafd-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="1dafd-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1dafd-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="1dafd-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1dafd-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1dafd-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1dafd-182">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="1dafd-182">Create an Azure AD test user</span></span>

<span data-ttu-id="1dafd-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="1dafd-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="1dafd-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="1dafd-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1dafd-186">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-186">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1dafd-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-188">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1dafd-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1dafd-190">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1dafd-192">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1dafd-192">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1dafd-194">a.</span><span class="sxs-lookup"><span data-stu-id="1dafd-194">a.</span></span> <span data-ttu-id="1dafd-195">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-195">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1dafd-196">b.</span><span class="sxs-lookup"><span data-stu-id="1dafd-196">b.</span></span> <span data-ttu-id="1dafd-197">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="1dafd-197">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="1dafd-198">c.</span><span class="sxs-lookup"><span data-stu-id="1dafd-198">c.</span></span> <span data-ttu-id="1dafd-199">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="1dafd-199">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="1dafd-200">d.</span><span class="sxs-lookup"><span data-stu-id="1dafd-200">d.</span></span> <span data-ttu-id="1dafd-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-201">Click **Create**.</span></span>
 
### <a name="create-a-youearnedit-test-user"></a><span data-ttu-id="1dafd-202">YouEarnedIt tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1dafd-202">Create a YouEarnedIt test user</span></span>

<span data-ttu-id="1dafd-203">Ebben a szakaszban egy YouEarnedIt Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1dafd-203">In this section, you create a user called Britta Simon in YouEarnedIt.</span></span> <span data-ttu-id="1dafd-204">Felhasználóival YouEarnedIt támogatási csapatának tooadd hello hello YouEarnedIt platform működik.</span><span class="sxs-lookup"><span data-stu-id="1dafd-204">Please work with YouEarnedIt support team tooadd hello users in hello YouEarnedIt platform.</span></span>

>[!NOTE]
><span data-ttu-id="1dafd-205">YouEarnedIt identitásszolgáltató toosupply egy e-mail cím vagy felhasználónév hello hello NameID attribútumot várt.</span><span class="sxs-lookup"><span data-stu-id="1dafd-205">YouEarnedIt expect hello Identity Provider toosupply an EmailAddress  or UserName in hello NameID attribute.</span></span> <span data-ttu-id="1dafd-206">Ha egy megfelelő felhasználónév vagy e-mail cím hello adatbázison belül nem található, vagy nem felel meg pontosan a hitelesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="1dafd-206">Authentication will fail if a corresponding UserName or EmailAddress is not found within hello database or does not match exactly.</span></span> <span data-ttu-id="1dafd-207">Ehhez szükséges, hogy fiókok hello YouEarnedIt rendszer hello SSO integrációs (általában vagy API-t vagy a fürt megosztott kötetei szolgáltatás importálási keresztül) előtt importálni.</span><span class="sxs-lookup"><span data-stu-id="1dafd-207">This will require that accounts be imported into hello YouEarnedIt system before hello SSO integration (Typically either via API or CSV import).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="1dafd-208">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="1dafd-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="1dafd-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooYouEarnedIt megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1dafd-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYouEarnedIt.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="1dafd-211">**tooassign Britta Simon tooYouEarnedIt, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="1dafd-211">**tooassign Britta Simon tooYouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="1dafd-212">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1dafd-214">Hello alkalmazások listában válassza ki a **YouEarnedIt**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-214">In hello applications list, select **YouEarnedIt**.</span></span>

    ![hello YouEarnedIt hivatkozásra hello alkalmazások listája](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. <span data-ttu-id="1dafd-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1dafd-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="1dafd-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1dafd-218">Click **Add** button.</span></span> <span data-ttu-id="1dafd-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1dafd-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="1dafd-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1dafd-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1dafd-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1dafd-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1dafd-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1dafd-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1dafd-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1dafd-224">Test single sign-on</span></span>

<span data-ttu-id="1dafd-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="1dafd-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1dafd-226">Ha a hozzáférési Panel hello hello YouEarnedIt csempe gombra kattint, automatikusan bejelentkezett tooyour YouEarnedIt alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="1dafd-226">When you click hello YouEarnedIt tile in hello Access Panel, you should get automatically signed-on tooyour YouEarnedIt application.</span></span>
<span data-ttu-id="1dafd-227">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1dafd-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1dafd-228">További források</span><span class="sxs-lookup"><span data-stu-id="1dafd-228">Additional resources</span></span>

* [<span data-ttu-id="1dafd-229">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1dafd-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1dafd-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1dafd-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png

