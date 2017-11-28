---
title: "Oktatóanyag: Azure Active Directoryval integrált szakasza kereskedelmi – kezelése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és szakasza kereskedelmi – információkezelés között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 9cd8ab65d41d01832e0611f7f8254aa257120508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a><span data-ttu-id="d6559-103">Oktatóanyag: Azure Active Directoryval integrált szakasza kereskedelmi – kezelése</span><span class="sxs-lookup"><span data-stu-id="d6559-103">Tutorial: Azure Active Directory integration with Infor Retail – Information Management</span></span>

<span data-ttu-id="d6559-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate szakasza kereskedelmi – kezelése az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6559-104">In this tutorial, you learn how toointegrate Infor Retail – Information Management with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6559-105">Szakasza kereskedelmi – kezelése az Azure AD integrálása lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d6559-105">Integrating Infor Retail – Information Management with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d6559-106">Az Azure AD hozzáférési tooInfor kereskedelmi – információkezelés rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="d6559-106">You can control in Azure AD who has access tooInfor Retail – Information Management.</span></span>
- <span data-ttu-id="d6559-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInfor kereskedelmi – információkezelés (egyszeri bejelentkezés) az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="d6559-107">You can enable your users tooautomatically get signed-on tooInfor Retail – Information Management (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d6559-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d6559-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="d6559-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6559-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6559-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6559-110">Prerequisites</span></span>

<span data-ttu-id="d6559-111">a következő elemek hello kell tooconfigure szakasza kereskedelmi – adatok kezelését, az Azure AD integrálása:</span><span class="sxs-lookup"><span data-stu-id="d6559-111">tooconfigure Azure AD integration with Infor Retail – Information Management, you need hello following items:</span></span>

- <span data-ttu-id="d6559-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d6559-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6559-113">Egy szakasza kereskedelmi – információkezelés egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d6559-113">An Infor Retail – Information Management single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6559-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d6559-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6559-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d6559-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6559-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6559-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6559-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6559-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6559-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d6559-118">Scenario description</span></span>
<span data-ttu-id="d6559-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d6559-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6559-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d6559-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6559-121">Hozzáadás szakasza kereskedelmi – hello gyűjteményből kezelése</span><span class="sxs-lookup"><span data-stu-id="d6559-121">Adding Infor Retail – Information Management from hello gallery</span></span>
2. <span data-ttu-id="d6559-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6559-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-infor-retail--information-management-from-hello-gallery"></a><span data-ttu-id="d6559-123">Hozzáadás szakasza kereskedelmi – hello gyűjteményből kezelése</span><span class="sxs-lookup"><span data-stu-id="d6559-123">Adding Infor Retail – Information Management from hello gallery</span></span>
<span data-ttu-id="d6559-124">tooconfigure hello integrációs szakasza kiskereskedelmi – kezelése az Azure AD-be kell tooadd szakasza kereskedelmi – hello gyűjtemény tooyour listája felügyelt SaaS-alkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="d6559-124">tooconfigure hello integration of Infor Retail – Information Management into Azure AD, you need tooadd Infor Retail – Information Management from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d6559-125">**tooadd szakasza kereskedelmi – hello gyűjteményből információkezelés hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d6559-125">**tooadd Infor Retail – Information Management from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6559-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6559-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="d6559-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d6559-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d6559-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6559-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="d6559-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d6559-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="d6559-133">Hello keresési mezőbe, írja be a **szakasza kereskedelmi – információkezelés**, jelölje be **szakasza kereskedelmi – információkezelés** eredmény panelen kattintson a **hozzáadása** gomb tooadd hello az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d6559-133">In hello search box, type **Infor Retail – Information Management**, select **Infor Retail – Information Management** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Szakasza kereskedelmi – hello eredménylistában kezelése](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d6559-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6559-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d6559-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés szakasza kiskereskedelmi – kezelése "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="d6559-136">In this section, you configure and test Azure AD single sign-on with Infor Retail – Information Management based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d6559-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói szakasza kiskereskedelmi – információkezelés tooa felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d6559-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Infor Retail – Information Management is tooa user in Azure AD.</span></span> <span data-ttu-id="d6559-138">Ez azt jelenti hello kapcsolódó felhasználói szakasza kiskereskedelmi – adatok kezelését és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d6559-138">In other words, a link relationship between an Azure AD user and hello related user in Infor Retail – Information Management needs toobe established.</span></span>

<span data-ttu-id="d6559-139">Szakasza kereskedelmi – adatok kezelését, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d6559-139">In Infor Retail – Information Management, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d6559-140">tooconfigure és szakasza kiskereskedelmi – adatok kezelését, az Azure AD az egyszeri bejelentkezés tesztelése van szüksége a következő építőelemeket toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="d6559-140">tooconfigure and test Azure AD single sign-on with Infor Retail – Information Management, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d6559-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d6559-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d6559-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6559-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6559-143">**[Hozzon létre egy szakasza kereskedelmi – információkezelés tesztfelhasználó](#create-an-infor-retail--information-management-test-user)**  - toohave egy megfelelője a Britta Simon szakasza kiskereskedelmi – kezelése, amelyek felhasználói csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d6559-143">**[Create an Infor Retail – Information Management test user](#create-an-infor-retail--information-management-test-user)** - toohave a counterpart of Britta Simon in Infor Retail – Information Management that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6559-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d6559-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6559-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d6559-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d6559-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6559-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d6559-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a szakasza kiskereskedelmi – információkezelés alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="d6559-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Infor Retail – Information Management application.</span></span>

<span data-ttu-id="d6559-148">**Szakasza kiskereskedelmi – adatok kezelését, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d6559-148">**tooconfigure Azure AD single sign-on with Infor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6559-149">Az Azure portál, a hello hello **szakasza kereskedelmi – információkezelés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d6559-149">In hello Azure portal, on hello **Infor Retail – Information Management** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="d6559-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d6559-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. <span data-ttu-id="d6559-153">A hello **szakasza kereskedelmi – információk felügyeleti tartományhoz és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás IDP kezdeményezett mód hello:</span><span class="sxs-lookup"><span data-stu-id="d6559-153">On hello **Infor Retail – Information Management Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk IDP szakasza kereskedelmi – információk felügyeleti tartományhoz és URL-címek](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    <span data-ttu-id="d6559-155">a.</span><span class="sxs-lookup"><span data-stu-id="d6559-155">a.</span></span> <span data-ttu-id="d6559-156">A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:</span><span class="sxs-lookup"><span data-stu-id="d6559-156">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    <span data-ttu-id="d6559-157">b.</span><span class="sxs-lookup"><span data-stu-id="d6559-157">b.</span></span> <span data-ttu-id="d6559-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.mingle.infor.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="d6559-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/sp/ACS.saml2`</span></span>

4. <span data-ttu-id="d6559-159">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="d6559-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Szakasza kereskedelmi – információk felügyeleti tartományhoz és URL-címek egyetlen bejelentkezés információk SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    <span data-ttu-id="d6559-161">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.mingle.infor.com/<company code>`</span><span class="sxs-lookup"><span data-stu-id="d6559-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/<company code>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d6559-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d6559-162">These values are not real.</span></span> <span data-ttu-id="d6559-163">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6559-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="d6559-164">Ügyfél [szakasza kereskedelmi – információk a felügyeleti ügyfél-támogatási csoport](mailto:innovate@infor.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6559-164">Contact [Infor Retail – Information Management Client support team](mailto:innovate@infor.com) tooget these values.</span></span> 

5. <span data-ttu-id="d6559-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d6559-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. <span data-ttu-id="d6559-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6559-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="d6559-169">tooconfigure egyszeri bejelentkezést a **szakasza kereskedelmi – információkezelés** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[szakasza kereskedelmi – információkezelés támogatási csoport ](mailto:innovate@infor.com).</span><span class="sxs-lookup"><span data-stu-id="d6559-169">tooconfigure single sign-on on **Infor Retail – Information Management** side, you need toosend hello downloaded **Metadata XML** too[Infor Retail – Information Management support team](mailto:innovate@infor.com).</span></span> <span data-ttu-id="d6559-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d6559-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d6559-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d6559-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d6559-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d6559-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d6559-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6559-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d6559-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d6559-174">Create an Azure AD test user</span></span>

<span data-ttu-id="d6559-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d6559-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="d6559-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d6559-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6559-178">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6559-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d6559-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d6559-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d6559-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d6559-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d6559-184">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d6559-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d6559-186">a.</span><span class="sxs-lookup"><span data-stu-id="d6559-186">a.</span></span> <span data-ttu-id="d6559-187">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6559-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6559-188">b.</span><span class="sxs-lookup"><span data-stu-id="d6559-188">b.</span></span> <span data-ttu-id="d6559-189">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="d6559-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="d6559-190">c.</span><span class="sxs-lookup"><span data-stu-id="d6559-190">c.</span></span> <span data-ttu-id="d6559-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d6559-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="d6559-192">d.</span><span class="sxs-lookup"><span data-stu-id="d6559-192">d.</span></span> <span data-ttu-id="d6559-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6559-193">Click **Create**.</span></span>
 
### <a name="create-an-infor-retail--information-management-test-user"></a><span data-ttu-id="d6559-194">Hozzon létre egy szakasza kereskedelmi – információkezelés tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="d6559-194">Create an Infor Retail – Information Management test user</span></span>

<span data-ttu-id="d6559-195">Ebben a szakaszban Britta Simon meghívta szakasza kereskedelmi – információkezelés felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d6559-195">In this section, you create a user called Britta Simon in Infor Retail – Information Management.</span></span> <span data-ttu-id="d6559-196">Adjon együttműködve [szakasza kereskedelmi – információkezelés támogatási csoport](mailto:innovate@infor.com) tooadd hello felhasználók hello szakasza kereskedelmi – információk kezelési platformot.</span><span class="sxs-lookup"><span data-stu-id="d6559-196">Please work with [Infor Retail – Information Management support team](mailto:innovate@infor.com) tooadd hello users in hello Infor Retail – Information Management platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d6559-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="d6559-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d6559-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooInfor kereskedelmi – információkezelés megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d6559-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInfor Retail – Information Management.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="d6559-200">**tooassign Britta Simon tooInfor kereskedelmi – adatok kezelését, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d6559-200">**tooassign Britta Simon tooInfor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6559-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6559-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d6559-203">Hello alkalmazások listában válassza ki a **szakasza kereskedelmi – információkezelés**.</span><span class="sxs-lookup"><span data-stu-id="d6559-203">In hello applications list, select **Infor Retail – Information Management**.</span></span>

    ![hello szakasza kereskedelmi – hello alkalmazások listáját hivatkozásra kezelése](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. <span data-ttu-id="d6559-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d6559-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="d6559-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6559-207">Click **Add** button.</span></span> <span data-ttu-id="d6559-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6559-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="d6559-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d6559-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d6559-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6559-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6559-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6559-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d6559-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d6559-213">Test single sign-on</span></span>

<span data-ttu-id="d6559-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d6559-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d6559-215">Hello szakasza kereskedelmi – információkezelés csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour szakasza kereskedelmi – információkezelés alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d6559-215">When you click hello Infor Retail – Information Management tile in hello Access Panel, you should get automatically signed-on tooyour Infor Retail – Information Management application.</span></span>
<span data-ttu-id="d6559-216">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d6559-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d6559-217">További források</span><span class="sxs-lookup"><span data-stu-id="d6559-217">Additional resources</span></span>

* [<span data-ttu-id="d6559-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d6559-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6559-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d6559-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

