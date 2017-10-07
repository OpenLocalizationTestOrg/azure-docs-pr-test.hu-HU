---
title: "Oktatóanyag: Azure Active Directoryval integrált Evidence.com |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Evidence.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: eed98c15dca8a6a77f0b5d407bb40ee0037fccad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="54f68-103">Oktatóanyag: Azure Active Directoryval integrált Evidence.com</span><span class="sxs-lookup"><span data-stu-id="54f68-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="54f68-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Evidence.com az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="54f68-104">In this tutorial, you learn how toointegrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="54f68-105">Evidence.com integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="54f68-105">Integrating Evidence.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="54f68-106">Az Azure AD hozzáférési tooEvidence.com rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="54f68-106">You can control in Azure AD who has access tooEvidence.com.</span></span>
- <span data-ttu-id="54f68-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEvidence.com (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="54f68-107">You can enable your users tooautomatically get signed-on tooEvidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="54f68-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="54f68-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="54f68-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="54f68-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54f68-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54f68-110">Prerequisites</span></span>

<span data-ttu-id="54f68-111">az Azure AD integrálása Evidence.com tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="54f68-111">tooconfigure Azure AD integration with Evidence.com, you need hello following items:</span></span>

- <span data-ttu-id="54f68-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="54f68-112">An Azure AD subscription</span></span>
- <span data-ttu-id="54f68-113">Egy Evidence.com egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="54f68-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="54f68-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="54f68-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="54f68-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="54f68-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="54f68-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="54f68-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="54f68-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="54f68-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="54f68-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="54f68-118">Scenario description</span></span>
<span data-ttu-id="54f68-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="54f68-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="54f68-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="54f68-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="54f68-121">Hello gyűjteményből Evidence.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54f68-121">Adding Evidence.com from hello gallery</span></span>
2. <span data-ttu-id="54f68-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="54f68-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-hello-gallery"></a><span data-ttu-id="54f68-123">Hello gyűjteményből Evidence.com hozzáadása</span><span class="sxs-lookup"><span data-stu-id="54f68-123">Adding Evidence.com from hello gallery</span></span>
<span data-ttu-id="54f68-124">tooconfigure hello integrációja Evidence.com az Azure AD-be, meg kell tooadd Evidence.com hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="54f68-124">tooconfigure hello integration of Evidence.com into Azure AD, you need tooadd Evidence.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="54f68-125">**tooadd Evidence.com hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="54f68-125">**tooadd Evidence.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="54f68-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="54f68-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="54f68-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="54f68-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="54f68-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54f68-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="54f68-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="54f68-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="54f68-133">Hello keresési mezőbe, írja be a **Evidence.com**, jelölje be **Evidence.com** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-133">In hello search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Evidence.com](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="54f68-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54f68-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="54f68-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="54f68-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="54f68-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Evidence.com tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="54f68-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evidence.com is tooa user in Azure AD.</span></span> <span data-ttu-id="54f68-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Evidence.com közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="54f68-138">In other words, a link relationship between an Azure AD user and hello related user in Evidence.com needs toobe established.</span></span>

<span data-ttu-id="54f68-139">Evidence.com, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="54f68-139">In Evidence.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="54f68-140">tooconfigure és az Azure AD az egyszeri bejelentkezés Evidence.com-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="54f68-140">tooconfigure and test Azure AD single sign-on with Evidence.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="54f68-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="54f68-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="54f68-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="54f68-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="54f68-143">**[Evidence.com tesztfelhasználó létrehozása](#create-a-evidencecom-test-user)**  -toohave egy megfelelője a Britta Simon a Evidence.com, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="54f68-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - toohave a counterpart of Britta Simon in Evidence.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="54f68-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="54f68-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="54f68-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="54f68-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="54f68-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54f68-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="54f68-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Evidence.com alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="54f68-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="54f68-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Evidence.com, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="54f68-148">**tooconfigure Azure AD single sign-on with Evidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="54f68-149">Az Azure portál, a hello hello **Evidence.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="54f68-149">In hello Azure portal, on hello **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="54f68-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="54f68-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="54f68-153">A hello **Evidence.com tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="54f68-153">On hello **Evidence.com Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Evidence.com tartomány és az URL-címek](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="54f68-155">a.</span><span class="sxs-lookup"><span data-stu-id="54f68-155">a.</span></span> <span data-ttu-id="54f68-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="54f68-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="54f68-157">b.</span><span class="sxs-lookup"><span data-stu-id="54f68-157">b.</span></span> <span data-ttu-id="54f68-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="54f68-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="54f68-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="54f68-159">These values are not real.</span></span> <span data-ttu-id="54f68-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="54f68-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="54f68-161">Ügyfél [Evidence.com ügyfél-támogatási csoport](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="54f68-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget these values.</span></span> 

4. <span data-ttu-id="54f68-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="54f68-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="54f68-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="54f68-166">A hello **Evidence.com konfigurációs** kattintson **konfigurálása Evidence.com** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="54f68-166">On hello **Evidence.com Configuration** section, click **Configure Evidence.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="54f68-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="54f68-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Evidence.com konfiguráció](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="54f68-169">Külön böngészőablak bejelentkezési tooyour Evidence.com bérlői rendszergazdaként, és keresse meg a túl**Admin** lap</span><span class="sxs-lookup"><span data-stu-id="54f68-169">In a separate web browser window, login tooyour Evidence.com tenant as an administrator and navigate too**Admin** Tab</span></span>

8. <span data-ttu-id="54f68-170">Kattintson a **ügynökség az egyszeri bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="54f68-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="54f68-171">Válassza ki **SAML alapján egyszeri bejelentkezéshez**</span><span class="sxs-lookup"><span data-stu-id="54f68-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="54f68-172">Másolás hello **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezési URL-címe** és **Sign-Out URL-cím** hello Azure-portálon és toohello Evidence.com megfelelő mezőiben szereplő értékek.</span><span class="sxs-lookup"><span data-stu-id="54f68-172">Copy hello **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in hello Azure portal and toohello corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="54f68-173">Nyissa meg a letöltött Certificate(Base64) fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **biztonsági tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="54f68-173">Open your downloaded Certificate(Base64) file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Security Certificate** box.</span></span> 

12. <span data-ttu-id="54f68-174">A Evidence.com hello konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="54f68-174">Save hello configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="54f68-175">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="54f68-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="54f68-176">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="54f68-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="54f68-177">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="54f68-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="54f68-178">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="54f68-178">Create an Azure AD test user</span></span>

<span data-ttu-id="54f68-179">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="54f68-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="54f68-181">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="54f68-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="54f68-182">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-182">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="54f68-184">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="54f68-184">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="54f68-186">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="54f68-186">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="54f68-188">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="54f68-188">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="54f68-190">a.</span><span class="sxs-lookup"><span data-stu-id="54f68-190">a.</span></span> <span data-ttu-id="54f68-191">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="54f68-191">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="54f68-192">b.</span><span class="sxs-lookup"><span data-stu-id="54f68-192">b.</span></span> <span data-ttu-id="54f68-193">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="54f68-193">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="54f68-194">c.</span><span class="sxs-lookup"><span data-stu-id="54f68-194">c.</span></span> <span data-ttu-id="54f68-195">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="54f68-195">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="54f68-196">d.</span><span class="sxs-lookup"><span data-stu-id="54f68-196">d.</span></span> <span data-ttu-id="54f68-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="54f68-198">Evidence.com tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="54f68-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="54f68-199">Az Azure AD felhasználók toobe képes toosign a azok ki kell építenie belül hello Evidence.com alkalmazás eléréshez.</span><span class="sxs-lookup"><span data-stu-id="54f68-199">For Azure AD users toobe able toosign in, they must be provisioned for access inside hello Evidence.com application.</span></span> <span data-ttu-id="54f68-200">Ez a szakasz ismerteti, hogyan toocreate az Azure AD-felhasználói fiókot, Evidence.com belül</span><span class="sxs-lookup"><span data-stu-id="54f68-200">This section describes how toocreate Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="54f68-201">**egy felhasználói fiókot az Evidence.com tooprovision:**</span><span class="sxs-lookup"><span data-stu-id="54f68-201">**tooprovision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="54f68-202">A webböngésző ablakának jelentkezzen be a Evidence.com vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="54f68-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="54f68-203">Keresse meg a túl**Admin** fülre.</span><span class="sxs-lookup"><span data-stu-id="54f68-203">Navigate too**Admin** tab.</span></span>

3. <span data-ttu-id="54f68-204">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="54f68-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="54f68-205">Kattintson a hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-205">Click hello **Add** button.</span></span>

5. <span data-ttu-id="54f68-206">Hello **E-mail cím** hello a hozzáadott felhasználói egyeznie kell az Azure ad-ben toogive kívánó hello felhasználónév hello felhasználók hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="54f68-206">hello **Email Address** of hello added user must match hello username of hello users in Azure AD who you wish toogive access.</span></span> <span data-ttu-id="54f68-207">Ha hello felhasználónevet és e-mail cím nem vannak hello ugyanazt az értéket a szervezetében, használhatja a hello **Evidence.com > attribútumok > egyszeri bejelentkezés** hello Azure portál toochange hello nameidenitifer küldött szakasza tooEvidence.com toobe hello e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="54f68-207">If hello username and email address are not hello same value in your organization, you can use hello **Evidence.com > Attributes > Single Sign-On** section of hello Azure portal toochange hello nameidenitifer sent tooEvidence.com toobe hello email address.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="54f68-208">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="54f68-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="54f68-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEvidence.com megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="54f68-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvidence.com.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="54f68-211">**tooassign Britta Simon tooEvidence.com, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="54f68-211">**tooassign Britta Simon tooEvidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="54f68-212">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54f68-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="54f68-214">Hello alkalmazások listában válassza ki a **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="54f68-214">In hello applications list, select **Evidence.com**.</span></span>

    ![hello Evidence.com hivatkozásra hello alkalmazások listája](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="54f68-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="54f68-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="54f68-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="54f68-218">Click **Add** button.</span></span> <span data-ttu-id="54f68-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54f68-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="54f68-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="54f68-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="54f68-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54f68-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="54f68-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54f68-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="54f68-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="54f68-224">Test single sign-on</span></span>

<span data-ttu-id="54f68-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="54f68-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="54f68-226">Ha a hozzáférési Panel hello hello Evidence.com csempe gombra kattint, automatikusan bejelentkezett tooyour Evidence.com alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="54f68-226">When you click hello Evidence.com tile in hello Access Panel, you should get automatically signed-on tooyour Evidence.com application.</span></span>
<span data-ttu-id="54f68-227">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="54f68-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="54f68-228">További források</span><span class="sxs-lookup"><span data-stu-id="54f68-228">Additional resources</span></span>

* [<span data-ttu-id="54f68-229">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="54f68-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="54f68-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="54f68-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

