---
title: "Oktatóanyag: Azure Active Directoryval integrált Thoughtworks Mingle |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Thoughtworks Mingle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a><span data-ttu-id="9ab69-103">Oktatóanyag: Azure Active Directoryval integrált Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="9ab69-103">Tutorial: Azure Active Directory integration with Thoughtworks Mingle</span></span>

<span data-ttu-id="9ab69-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Thoughtworks Mingle az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ab69-104">In this tutorial, you learn how toointegrate Thoughtworks Mingle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ab69-105">Thoughtworks Mingle integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="9ab69-105">Integrating Thoughtworks Mingle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9ab69-106">Megadhatja a hozzáférés tooThoughtworks Mingle rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9ab69-106">You can control in Azure AD who has access tooThoughtworks Mingle</span></span>
- <span data-ttu-id="9ab69-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooThoughtworks Mingle (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9ab69-107">You can enable your users tooautomatically get signed-on tooThoughtworks Mingle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ab69-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9ab69-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9ab69-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ab69-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ab69-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ab69-110">Prerequisites</span></span>

<span data-ttu-id="9ab69-111">tooconfigure Thoughtworks Mingle integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="9ab69-111">tooconfigure Azure AD integration with Thoughtworks Mingle, you need hello following items:</span></span>

- <span data-ttu-id="9ab69-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9ab69-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ab69-113">Egy Thoughtworks Mingle egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9ab69-113">A Thoughtworks Mingle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ab69-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9ab69-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ab69-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="9ab69-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ab69-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9ab69-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ab69-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ab69-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ab69-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9ab69-118">Scenario description</span></span>
<span data-ttu-id="9ab69-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9ab69-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ab69-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9ab69-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ab69-121">Hozzáadás a Thoughtworks Mingle hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9ab69-121">Adding Thoughtworks Mingle from hello gallery</span></span>
2. <span data-ttu-id="9ab69-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ab69-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a><span data-ttu-id="9ab69-123">Hozzáadás a Thoughtworks Mingle hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9ab69-123">Adding Thoughtworks Mingle from hello gallery</span></span>
<span data-ttu-id="9ab69-124">tooconfigure hello integrációja Thoughtworks Mingle az Azure AD-be, meg kell tooadd Thoughtworks Mingle hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="9ab69-124">tooconfigure hello integration of Thoughtworks Mingle into Azure AD, you need tooadd Thoughtworks Mingle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9ab69-125">**tooadd Thoughtworks Mingle hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9ab69-125">**tooadd Thoughtworks Mingle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ab69-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="9ab69-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9ab69-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="9ab69-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="9ab69-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="9ab69-133">Hello keresési mezőbe, írja be a **Thoughtworks Mingle**, jelölje be **Thoughtworks Mingle** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-133">In hello search box, type **Thoughtworks Mingle**, select **Thoughtworks Mingle** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Mingle Thoughtworks](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9ab69-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ab69-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9ab69-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Thoughtworks Mingle "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="9ab69-136">In this section, you configure and test Azure AD single sign-on with Thoughtworks Mingle based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ab69-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Thoughtworks Mingle tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9ab69-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Thoughtworks Mingle is tooa user in Azure AD.</span></span> <span data-ttu-id="9ab69-138">Ez azt jelenti hello kapcsolódó felhasználó a Thoughtworks Mingle és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="9ab69-138">In other words, a link relationship between an Azure AD user and hello related user in Thoughtworks Mingle needs toobe established.</span></span>

<span data-ttu-id="9ab69-139">Thoughtworks Mingle, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="9ab69-139">In Thoughtworks Mingle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9ab69-140">tooconfigure és az Azure AD az egyszeri bejelentkezés teszthez Thoughtworks Mingle, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="9ab69-140">tooconfigure and test Azure AD single sign-on with Thoughtworks Mingle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9ab69-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9ab69-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9ab69-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ab69-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ab69-143">**[Thoughtworks Mingle tesztfelhasználó létrehozása](#create-a-thoughtworks-mingle-test-user)**  -toohave egy megfelelője a Britta Simon a Thoughtworks Mingle, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9ab69-143">**[Create a Thoughtworks Mingle test user](#create-a-thoughtworks-mingle-test-user)** - toohave a counterpart of Britta Simon in Thoughtworks Mingle that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ab69-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9ab69-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ab69-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="9ab69-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9ab69-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ab69-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9ab69-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Thoughtworks Mingle alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="9ab69-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Thoughtworks Mingle application.</span></span>

<span data-ttu-id="9ab69-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Thoughtworks Mingle, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9ab69-148">**tooconfigure Azure AD single sign-on with Thoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ab69-149">Az Azure portál, a hello hello **Thoughtworks Mingle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-149">In hello Azure portal, on hello **Thoughtworks Mingle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9ab69-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="9ab69-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. <span data-ttu-id="9ab69-153">A hello **Thoughtworks Mingle tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9ab69-153">On hello **Thoughtworks Mingle Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Thoughtworks Mingle tartomány és az URL-címek](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    <span data-ttu-id="9ab69-155">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.mingle.thoughtworks.com`</span><span class="sxs-lookup"><span data-stu-id="9ab69-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.mingle.thoughtworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ab69-156">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="9ab69-156">hello value is not real.</span></span> <span data-ttu-id="9ab69-157">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9ab69-157">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="9ab69-158">Ügyfél [Thoughtworks Mingle ügyfél-támogatási csoport](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="9ab69-158">Contact [Thoughtworks Mingle Client support team](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello value.</span></span> 
 
4. <span data-ttu-id="9ab69-159">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9ab69-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. <span data-ttu-id="9ab69-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9ab69-163">Jelentkezzen be tooyour **Thoughtworks Mingle** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ab69-163">Log in tooyour **Thoughtworks Mingle** company site as administrator.</span></span>

7. <span data-ttu-id="9ab69-164">Hello kattintson **Admin** lapot, és kattintson a **SSO Config**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-164">Click hello **Admin** tab, and then, click **SSO Config**.</span></span>
   
    <span data-ttu-id="9ab69-165">![Felügyelet lapját](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="9ab69-165">![Admin tab](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span></span>

8. <span data-ttu-id="9ab69-166">A hello **SSO Config** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9ab69-166">In hello **SSO Config** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9ab69-167">![Egyszeri bejelentkezés Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="9ab69-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span></span>
    
    <span data-ttu-id="9ab69-168">a.</span><span class="sxs-lookup"><span data-stu-id="9ab69-168">a.</span></span> <span data-ttu-id="9ab69-169">tooupload hello metaadat-fájlt, kattintson a **fájl kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-169">tooupload hello metadata file, click **Choose file**.</span></span> 

    <span data-ttu-id="9ab69-170">b.</span><span class="sxs-lookup"><span data-stu-id="9ab69-170">b.</span></span> <span data-ttu-id="9ab69-171">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-171">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="9ab69-172">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="9ab69-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9ab69-173">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="9ab69-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9ab69-174">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ab69-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9ab69-175">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9ab69-175">Create an Azure AD test user</span></span>
<span data-ttu-id="9ab69-176">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="9ab69-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="9ab69-178">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="9ab69-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ab69-179">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9ab69-181">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ab69-183">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ab69-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ab69-185">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9ab69-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ab69-187">a.</span><span class="sxs-lookup"><span data-stu-id="9ab69-187">a.</span></span> <span data-ttu-id="9ab69-188">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ab69-189">b.</span><span class="sxs-lookup"><span data-stu-id="9ab69-189">b.</span></span> <span data-ttu-id="9ab69-190">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ab69-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ab69-191">c.</span><span class="sxs-lookup"><span data-stu-id="9ab69-191">c.</span></span> <span data-ttu-id="9ab69-192">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9ab69-193">d.</span><span class="sxs-lookup"><span data-stu-id="9ab69-193">d.</span></span> <span data-ttu-id="9ab69-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-194">Click **Create**.</span></span>
 
### <a name="create-a-thoughtworks-mingle-test-user"></a><span data-ttu-id="9ab69-195">Thoughtworks Mingle tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ab69-195">Create a Thoughtworks Mingle test user</span></span>

<span data-ttu-id="9ab69-196">Az Azure AD felhasználók toobe képes toosign a, a kiépített toohello Thoughtworks Mingle alkalmazást az Azure Active Directory felhasználói neveket kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="9ab69-196">For Azure AD users toobe able toosign in, they must be provisioned toohello Thoughtworks Mingle application using their Azure Active Directory user names.</span></span> <span data-ttu-id="9ab69-197">Thoughtworks Mingle hello esetében kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9ab69-197">In hello case of Thoughtworks Mingle, provisioning is a manual task.</span></span>

<span data-ttu-id="9ab69-198">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="9ab69-198">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ab69-199">Bejelentkezés tooyour Thoughtworks Mingle vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ab69-199">Log in tooyour Thoughtworks Mingle company site as administrator.</span></span>

2. <span data-ttu-id="9ab69-200">Kattintson a **profil**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-200">Click **Profile**.</span></span>
   
    <span data-ttu-id="9ab69-201">![Az első projektet](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "az első projektet")</span><span class="sxs-lookup"><span data-stu-id="9ab69-201">![Your First Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "Your First Project")</span></span>

3. <span data-ttu-id="9ab69-202">Kattintson a hello **Admin** fülre, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-202">Click hello **Admin** tab, and then click **Users**.</span></span>
   
    <span data-ttu-id="9ab69-203">![Felhasználók](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="9ab69-203">![Users](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "Users")</span></span>

4. <span data-ttu-id="9ab69-204">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-204">Click **New User**.</span></span>
   
    <span data-ttu-id="9ab69-205">![Új felhasználó](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="9ab69-205">![New User](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "New User")</span></span>

5. <span data-ttu-id="9ab69-206">A hello **új felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9ab69-206">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="9ab69-207">![Új felhasználó párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="9ab69-207">![New User dialog](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "New User")</span></span>  
 
    <span data-ttu-id="9ab69-208">a.</span><span class="sxs-lookup"><span data-stu-id="9ab69-208">a.</span></span> <span data-ttu-id="9ab69-209">Típus hello **bejelentkezési név**, **megjelenített név**, **válasszon jelszó**, **jelszó megerősítése** egy érvényes Azure AD-fiókot szeretne tooprovision a hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="9ab69-209">Type hello **Sign-in name**, **Display name**, **Choose password**, **Confirm password** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="9ab69-210">b.</span><span class="sxs-lookup"><span data-stu-id="9ab69-210">b.</span></span> <span data-ttu-id="9ab69-211">Mint **felhasználótípust**, jelölje be **teljes felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-211">As **User type**, select **Full user**.</span></span>

    <span data-ttu-id="9ab69-212">c.</span><span class="sxs-lookup"><span data-stu-id="9ab69-212">c.</span></span> <span data-ttu-id="9ab69-213">Kattintson a **a profil létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-213">Click **Create This Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="9ab69-214">Bármely más Thoughtworks Mingle felhasználói fiók létrehozása eszközök vagy Thoughtworks Mingle tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="9ab69-214">You can use any other Thoughtworks Mingle user account creation tools or APIs provided by Thoughtworks Mingle tooprovision AAD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9ab69-215">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="9ab69-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9ab69-216">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooThoughtworks Mingle megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="9ab69-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThoughtworks Mingle.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="9ab69-218">**tooassign Britta Simon tooThoughtworks Mingle, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="9ab69-218">**tooassign Britta Simon tooThoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ab69-219">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9ab69-221">Hello alkalmazások listában válassza ki a **Thoughtworks Mingle**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-221">In hello applications list, select **Thoughtworks Mingle**.</span></span>

    ![hello Thoughtworks Mingle hivatkozásra hello alkalmazások listája](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. <span data-ttu-id="9ab69-223">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9ab69-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="9ab69-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ab69-225">Click **Add** button.</span></span> <span data-ttu-id="9ab69-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ab69-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="9ab69-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9ab69-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9ab69-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ab69-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ab69-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ab69-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9ab69-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9ab69-231">Test single sign-on</span></span>

<span data-ttu-id="9ab69-232">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="9ab69-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9ab69-233">Hello Thoughtworks Mingle csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Thoughtworks Mingle alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="9ab69-233">When you click hello Thoughtworks Mingle tile in hello Access Panel, you should get automatically signed-on tooyour Thoughtworks Mingle application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ab69-234">További források</span><span class="sxs-lookup"><span data-stu-id="9ab69-234">Additional resources</span></span>

* [<span data-ttu-id="9ab69-235">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="9ab69-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ab69-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9ab69-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

